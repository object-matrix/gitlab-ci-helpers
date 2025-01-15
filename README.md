# gitlab-ci-helpers
Gitlab helper scripts for easier setup of CI/CD pipelines in your projects.

# Gitlab CI variables
Following environment variables must be defined

For all gradle tasks
- GRADLE_DOCKER_IMAGE (in .gitlab-ci.yml)

For `gradle-boot-publish.yml` and `docker-push-to-registry.yml`
- REGISTRY_HOST
- REGISTRY_USERNAME
- REGISTRY_PASSWORD

For `docker-push-to-aws-ecr.yml`
- AWS_ACCOUNT_ID
- AWS_REGION
- AWS_ACCESS_KEY_ID
- AWS_SECRET_ACCESS_KEY

# Examples

For most common use cases you can use in `.gitlab-ci.yml`

```yml
include:
  - https://raw.githubusercontent.com/object-matrix/gitlab-ci-helpers/refs/heads/main/src/gradle-main.yml
  - https://raw.githubusercontent.com/object-matrix/gitlab-ci-helpers/refs/heads/main/src/gradle-boot-publish.yml
  - https://raw.githubusercontent.com/object-matrix/gitlab-ci-helpers/refs/heads/main/src/docker-push-to-registry.yml
  - https://raw.githubusercontent.com/object-matrix/gitlab-ci-helpers/refs/heads/main/src/docker-push-to-aws-ecr.yml

stages:
  - build
  - test
  - publish
  - docker-build-images
  - docker-push-images
  - docker-publish-manifests
  - docker-cleanup-images

variables:
  GRADLE_DOCKER_IMAGE: gradle:8.5-jdk17
```

If needed selective jobs can be included instead.
This may be useful in debugging jobs
For example to skip test and publish stages:

```yml
include:
  - https://raw.githubusercontent.com/object-matrix/gitlab-ci-helpers/refs/heads/main/src/gradle-base.yml
  - https://raw.githubusercontent.com/object-matrix/gitlab-ci-helpers/refs/heads/main/src/gradle-assemble.yml
  - https://raw.githubusercontent.com/object-matrix/gitlab-ci-helpers/refs/heads/main/src/gradle-boot-publish.yml
  - https://raw.githubusercontent.com/object-matrix/gitlab-ci-helpers/refs/heads/main/src/docker-push-to-registry.yml
  - https://raw.githubusercontent.com/object-matrix/gitlab-ci-helpers/refs/heads/main/src/docker-push-to-aws-ecr.yml

stages:
  - build
  - docker-build-images
  - docker-push-images
  - docker-publish-manifests
  - docker-cleanup-images

variables:
  GRADLE_DOCKER_IMAGE: gradle:8.5-jdk17
```
