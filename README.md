# ci-remote-exec

Набор шаблонов GitLab CI/CD для выполнения задач на внешних серверах.

---

## docker-healthcheck

Шаблон для проверки состояния (healthcheck) контейнера на удалённом хосте.

### Использование

```yaml
include:
  - component: $CI_SERVER_FQDN/library/cicd/templates/ci-remote-exec/docker-healthcheck@main
    inputs:
      stage: package-test
      verified-image: "gitlab-registry.gitlab.svc:5000/repo/example-snapshot:main"
      docker-host: "tcp://192.168.0.0:2375"
      global-registry-url: "gitlab-registry.example.com"
```

### inputs

| Параметр                  | Описание                                                          | Тип       | По умолчанию                      | Обязательный |
|---------------------------|-------------------------------------------------------------------|-----------|-----------------------------------|--------------|
| `docker-healthcheck`      | Включает выполнение docker healthcheck                            | `boolean` | `true`                            | нет          |
| `docker-image`            | Образ, используемый для запуска docker-клиента                    | `string`  | `docker.io/library/docker:latest` | нет          |
| `docker-config-file`      | Путь к файлу Docker config.json                                   | `string`  | `/root/.docker/config.json`       | нет          |
| `stage`                   | Стадия                                                            | `string`  | `package-test`                    | нет          |
| `verified-image`          | Docker-образ, который проверяется в процессе healthcheck          | `string`  | `""`                              | нет          |
| `docker-host`             | TCP-подключение к удалённому Docker                               | `string`  | —                                 | да           |
| `global-registry-url`     | Внешний URL Docker-реестра (используется вне Kubernetes-кластера) | `string`  | —                                 | нет          |
| `docker-healthcheck-args` | Дополнительные аргументы для контейнера healthcheck               | `string`  | `""`                              | нет          |
| `docker-container-args`   | Дополнительные аргументы для основной команды docker run          | `string`  | `""`                              | нет          |

### Environment Variables

| Переменная окружения                | Значение                                | Notes                             |
|-------------------------------------|-----------------------------------------|-----------------------------------|
| `DOCKER_HEALTHCHECK_IMAGE`          | `$[[ inputs.docker-image ]]`            |                                   |
| `DOCKER_CONFIG_FILE`                | `$[[ inputs.docker-config-file ]]`      |                                   |
| `VERIFIED_IMAGE`                    | `$[[ inputs.verified-image ]]`          |                                   |
| `DOCKER_HOST`                       | `$[[ inputs.docker-host ]]`             |                                   |
| `GLOBAL_REGISTRY_URL`               | `$[[ inputs.global-registry-url ]]`     |                                   |
| `DOCKER_HEALTHCHECK_ARGS`           | `$[[ inputs.docker-healthcheck-args ]]` |                                   |
| `DOCKER_HEALTHCHECK_CONTAINER_ARGS` | `$[[ inputs.docker-container-args ]]`   |                                   |
| `VERIFIED_IMAGE`                    | `$[[ inputs.verified-image ]]`          | Переназначается в отдельном блоке |

### Template Extension Points

| Расширение / Блок                                         | Назначение                            |
|-----------------------------------------------------------|---------------------------------------|
| `.ci-remote-exec:docker-healthcheck:public:env-overwrite` | Переопределение переменных окружения. |
| `.ci-remote-exec:docker-healthcheck:public:rules-extends` | Переопределение правил запуска.       |
| `.ci-remote-exec:docker-healthcheck:public:extends`       | Переопределение блоков job.           |
| `.ci-remote-exec:docker-healthcheck:public:needs`         | Определяет зависимости от других job. |
