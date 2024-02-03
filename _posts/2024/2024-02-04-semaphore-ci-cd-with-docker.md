---
title: "Semaphore CI/CD using Docker Hub (instead of Semaphore's built-in Docker registry)"
excerpt: ""
date: 2024-02-02 03:30:00 PM UTC
date_last_modified:
categories:
  - Programming
tags: 
  - CI/CD
  - Docker
published: true
---

I recently found a beautifully crafted free ebook on CI/CD, from Semaphore. The name of the book is "CI/CD with Docker and Kubernetes". You can [download it for free](https://semaphoreci.com/resources/cicd-docker-kubernetes).

Chapter 4 of the book is about implementing a CI/CD pipeline. But the commands given in the examples uses the built-in Docker registry of Semaphore, but it's not available in the free plan. Page 66 of the book says this:

> Semaphore’s built-in Docker registry is available under the enterprise plan. If you’re using a free, open-source, or startup plan, use an external service like Docker Hub instead. The pipeline will be slower, but the workflow will be the same.

So I looked for a way on how to use Docker Hub to complete the tutorial. 

Below is the complete `semaphore.yml` file containing the relevant commands for connecting to Docker Hub. Compare it with the [original `semaphore.yml` file](https://github.com/semaphoreci-demos/semaphore-demo-cicd-kubernetes/blob/master/.semaphore/semaphore.yml).

Note that the _blocks_ named `Build` and `Push` contains a `prologue` for logging into Docker Hub.

Also, you need to replace the Docker Hub repository name. Mine is named `jflaga/demo` in the `.yml` file below. Replace that with the name of your repository.

You also need to create a secret in Semaphore for `$DOCKERHUB_USERNAME` and `$DOCKERHUB_PASSWORD`.

``` yaml
version: v1.0
name: Docker
agent:
  machine:
    type: e1-standard-2
    os_image: ubuntu2004

blocks:
  - name: Build
    task:
      jobs:
        - name: docker build
          commands:
            - checkout
            - 'docker pull jflaga/demo:latest || true'
            - 'docker build --cache-from jflaga/demo:latest -t jflaga/demo:$SEMAPHORE_WORKFLOW_ID .'
            - 'docker push jflaga/demo:$SEMAPHORE_WORKFLOW_ID'
      secrets:
        - name: Docker Hub Secret
      prologue:
        commands:
          - echo $DOCKERHUB_PASSWORD | docker login --username "$DOCKERHUB_USERNAME" --password-stdin
  - name: Tests
    task:
      prologue:
        commands:
          - 'docker pull jflaga/demo:$SEMAPHORE_WORKFLOW_ID'
      jobs:
        - name: Unit test
          commands:
            - 'docker run -it jflaga/demo:$SEMAPHORE_WORKFLOW_ID npm run lint'
        - name: Functional test
          commands:
            - sem-service start postgres
            - 'docker run --net=host -it jflaga/demo:$SEMAPHORE_WORKFLOW_ID npm run ping'
            - 'docker run --net=host -it jflaga/demo:$SEMAPHORE_WORKFLOW_ID npm run migrate'
        - name: Integration test
          commands:
            - sem-service start postgres
            - 'docker run --net=host -it jflaga/demo:$SEMAPHORE_WORKFLOW_ID npm run test'
  - name: Push
    task:
      jobs:
        - name: Push
          commands:
            - 'docker pull jflaga/demo:$SEMAPHORE_WORKFLOW_ID'
            - 'docker tag jflaga/demo:$SEMAPHORE_WORKFLOW_ID jflaga/demo:latest'
            - 'docker push jflaga/demo:latest'
      secrets:
        - name: Docker Hub Secret
      prologue:
        commands:
          - echo $DOCKERHUB_PASSWORD | docker login --username "$DOCKERHUB_USERNAME" --password-stdin
```

Enjoy!
