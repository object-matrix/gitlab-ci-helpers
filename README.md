# gitlab-ci-helpers
Gitlab helper scripts for easier setup of CI/CD pipelines in your projects.



# How scripts work

Docker image builds are triggered on amd64 and arm64 runners.

Runners must be tagged with `docker-amd64` and `docker-arm64` respectively.

Images are build only for tags.

Docker multiarch manifest is added automatically.

Docker tag `latest` is added only it the git tag had format `major.minor.build` i.e. official versioned build.



For pushing images to AWS ECR repository must already exist in ECR.



# Gitlab CI variables

Following environment variables must be defined

For all gradle tasks
- GRADLE_DOCKER_IMAGE (in .gitlab-ci.yml)

```properties
# Example
variables:
  GRADLE_DOCKER_IMAGE: gradle:7.6-jdk17
```



For `gradle-boot-publish.yml` and `docker-push-to-registry.yml` 
- REGISTRY_HOST
- REGISTRY_USERNAME
- REGISTRY_PASSWORD



For `docker-push-to-aws-ecr.yml`

- AWS_ACCOUNT_ID

- AWS_REGION

- AWS_ACCESS_KEY_ID

- AWS_SECRET_ACCESS_KEY

- optional AWS_ECR_NAMESPACE if your repository has a namespace part

  Example:

  ```properties
  # for repository 12345.dkr.ecr.us-east-1.amazonaws.com/my-namespace/mcs-project-name
  AWS_ECR_NAMESPACE=my-namespace
  ```

  

# Examples

For most common use cases your  `.gitlab-ci.yml` may look as follows

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
