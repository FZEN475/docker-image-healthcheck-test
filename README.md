# docker-image-mirroring

## Description

Модуль для [to-be-continuous/Docker](https://gitlab.fizn.ru/library/to-be-continuous/Docker.git)  
Добавляет тестирование собранных SNAPSHOT в докере на удалённом хосте.  
Зависим от `.docker-scripts:`, `.test-policy` и [utils](https://gitlab.fizn.ru/library/cicd/utils.git)

## ENV

| Name                              | Description                               | Defaults                        | Notes                                                                                     |
|-----------------------------------|-------------------------------------------|---------------------------------|-------------------------------------------------------------------------------------------|
| DOCKER_IMAGE                      | Образ `docker`                            | docker.io/library/docker:latest | из [to-be-continuous/Docker](https://gitlab.fizn.ru/library/to-be-continuous/Docker.git)  |
| DOCKER_CONFIG_FILE                | Файл с доступами к репозиториям.          | /tmp/config.json                | из [to-be-continuous/Docker](https://gitlab.fizn.ru/library/to-be-continuous/Docker.git)  |
| DOCKER_BUILD_TOOL                 | Работает только для `buildah`             | buildah                         | из [to-be-continuous/Docker](https://gitlab.fizn.ru/library/to-be-continuous/Docker.git)  |
| DOCKER_HEALTHCHECK_OPTIONS        | Ключи docker run                          |                                 | из [to-be-continuous/Docker](https://gitlab.fizn.ru/library/to-be-continuous/Docker.git)  |
| DOCKER_HEALTHCHECK_CONTAINER_ARGS | Ключи контейнера docker run               |                                 | из [to-be-continuous/Docker](https://gitlab.fizn.ru/library/to-be-continuous/Docker.git)  |
| DOCKER_HEALTHCHECK_TIMEOUT        | Время ожидания теста                      | 60                              | из [to-be-continuous/Docker](https://gitlab.fizn.ru/library/to-be-continuous/Docker.git)  |
| REGISTRY_IMAGE                    | Переназначение для $DOCKER_SNAPSHOT_IMAGE | $DOCKER_SNAPSHOT_IMAGE          |                                                                                           |
| DOCKER_HOST                       | ip хоста для тестов                       | tcp://docker:2375               |                                                                                           |
| GLOBAL_REGISTRY_URL               | Подстановка внешнего endpoint egistry     | gitlab-registry.fizn.ru         |                                                                                           |

## Использование

```yaml
include:
  - component: $CI_SERVER_FQDN/library/to-be-continuous/Docker/gitlab-ci-docker@master
    
  - component: $CI_SERVER_FQDN/library/cicd/extands/docker-image-healthcheck-test/remote-docker@main

```