# Setting up CI Pipeline Using CircleCI and Github

## Overview

This tutorial is intended to give information about CircleCI and demonstrate its basic concepts by integrating CircleCI with a Github project. We are going to automate and orchestrate the steps necessary to compile, test and publish.

If you are not familiar with the concept of CI/CD, you can check out our <a href="https://folksdevtv.medium.com/ci-cd-in-a-nutshell-abe1b03d2e5d">CI/CD In a Nutshell</a> article to get the most out of this tutorial.

## What is CircleCi

As you can guess from the context, <b>circleci</b> is yet another CI service that
you can make use of. 

With <b>circleci</b> you can utilize CI in your development process easily. As you will
later see in this tutorial, you can spin up everything with a simple yml file.
You can configure workflows and orchestrate them in various ways depending
on your requirements. In a nutshell, you can:

<ul>
    <li> Compile and test your code </li>
    <li> Schedule your jobs </li>
    <li> Create custom workflows and pipelines </li>
    <li> Orchestrate your jobs </li>
</ul> 

Key features of <b>circleci</b> include (but not limited to):
<ul>
    <li> No dedicated servers and administration needed </li>
    <li> Has a free plan </li>
    <li> Nice cli to run tasks locally </li>
    <li> Rest api to access projects, builds and artifacts </li>
    <li> Easy configuration </li>
    <li> VCS integration </li>
    <li> Large user community </li>
</ul> 

## CircleCI Building Blocks

### Pipeline

It represents all the workflows that will run and all the data such as configuration, security keys, and passwords required to run these workflows. In other words, it represents the entire operating environment. The configuration we have made for our project is the pipeline we have created for that project in the CircleCI world. The workflows defined in this configuration file will run every time a change is going to be applied to the project.

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/circleci/Pipeline-1.png"/>

### Workflow & Job & Step

In the CircleCi world, the workflow is the configuration part that specifies which job will work in what order and under what conditions. When a workflow starts, the jobs defined under the workflow will be executed. The workflow will terminate if a job fails or it will be completed when the last job finishes.  Although more than one workflow can be defined in a pipeline, the best practice is to define one.

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/circleci/bb-overall.png"/>
 
A job represents a series of steps defined to accomplish this job. Steps are executable commands like checking out the code from VCS, loading data from the cache, or running tests. Shell commands can be used on CircleCI as well as commands in CircleCI API.

### Executors

All jobs that will work under a workflow will be run by a new executor. It worths highlighting here that a new executor will be created for each job. CircleCI contains four types of trainers. Three of these are operating system virtual machines, including Linux, Windows, and macOS. The other is Docker containers, which means jobs run in docker containers. Containers can use configuration and data defined in the pipeline, and besides, they load and configure the container to execute the steps defined. When the job completes, the container is terminated. 

<img src="https://circleci.com/docs/assets/img/docs/executor_types.png"/>

Finally, it worths noting that for jobs that need to run Docker command, such as creating a docker image, it is run in a remote container, so the step `-setup_remote_docker 'must be added to the job.

### Data Persistence

When a new executor is created from scratch for each job, the new executor is started with a clean memory without any data from the previous job. CircleCI offers three strategies to share data between jobs and make it permanent for use in future workflows, as well as making things repeatable and efficient.

- Workspace
- Cache
- Artifact

### Workspace

Workspace is used to share data between jobs in the same workflow. While the job is running, the data generated on the container is saved to the workspace for later use. A job that needs this data can load it from the workspace. Saving to and loading from cache is done by adding `- persist_to_workspace` and `-attach_workspace` steps to the jobs.
Data is stored in the workspace for 15 days. But it should not be forgotten that this data will only be available when running that workflow again. Otherwise, every time the pipeline runs, a new workspace will be created for the workflow, meaning there will be no data available for that run.

<img src="https://circleci.com/docs/assets/img/docs/workspaces.png"/>

### Cache

The cache is used to share data between multiple runs of a pipeline. The cache is often used to avoid repeating costly, large-file downloads. One of the important examples of the cache is used to store the dependencies of the project. The dependencies of the projects are clear and they do not change very often. At least they do not change at every commit and even if they change, they do not change all at once, and it will only be sufficient to download the new version of the updated dependency. Therefore, dependencies can be stored in the cache to avoid re-downloading each time. Cache writing and loading are done by adding the steps `- save_cache` and` - restore_cache` to the job. In the cache, data is stored for 15 days.

<img src="https://circleci.com/docs/assets/img/docs/caching-dependencies-overview.png"/>

### Artifact

Let's say you have a Java project in your CI pipeline, it will probably generate a jar file as a result of your build, or the build process for an Android project generates an `apk` file as output. These files are called `artifacts` and CircleCI can store these artifacts for us. Artifacts are saved with the step `-store_artifacts`. It is necessary to make a rest call to the CircleCI API to download the artifacts resulting from a run. Artifacts are stored for 30 days.

<img src="https://circleci.com/docs/assets/img/docs/Diagram-v3-Artifact.png"/>

## Setting Up The Pipeline

The goals of setting up a CI Pipeline are:
- Prevent broken changes to be merged to main branch
- Ensure QA process applied
- Create artifacts to be distributed

Therefore our pipeline should be able to build and test the code and create artifacts. In this tutorial we are going to publish docker images to Dockerhub as artifacts.

### Configuration

CircleCI 2.0 requires `config.yml` file to be under the `.circleci` directory which should be under the projects root directory. Thus the file location should follow `.circleci/config.yml`. This config file is the representation of the whole pipeline.

#### Version

Circleci config.yml should specify its version at the beginning of the file so we start with stating the version.

```yml
version: 2.1
```

## Configuring jobs

Jobs needs to be defined under `jobs`. Each job should be represented as separate entities starting with their name as key.
Then the executor for the job needs to be specified. Since we are going to use docker containers as executors, `docker` key has to be used. The images to build the executor can be defined under the `docker` section as shown below. Given images will be pulled by circleci and container will be created based on those images. Image configuration must be followed by `steps` that is the list of commands to be executed in given order to comple the job. Environment variables can be configured under `environment`
section.

```yml
  jobs: 
    build: # Job Name
        docker: # Executor
            - image: <image>
            - image: <image>
            ...
        environment: # Environment variables to be baked into container
            - <key>: <value>
            - <key>: <value>
            ....
        steps: # List of steps
            - run: # Keyword for step
                name: <name> # Name for the step
                command: <command> # Command to execute
            - run: 
                name: <name>
                command: <command>
            ...
    ...
```

### Build & Test & Publish

Since we are going to build an sbt project we need to have jdk and sbt installed in out executor container. We can use the official docker image for `openjdk11` to have jdk installed in the container. However sbt and docker are missing and still needs to be downloaded and installed into the container. This means two steps needs to be configured for to install these. 

Then, to be able to build, test, and publish, we need the source code to be installed in the container. CircleCI can checkout the code from Github. To checkout the code, `checkout` step needs to be added. The code will be checked out from the respective origin branch.

To be able to publish a docker image at the end, we need to be able to run docker commands. CircleCI executes docker commands in a remote docker environment for security reasons. This environment is remote, fully-isolated and has been configured to execute Docker commands. If your job requires `docker` or `docker-compose` commands, add the `setup_remote_docker` step needs to be added.

The critical thing with running docker commands in CircleCI is, if you are not using CircleCI's images for your executor container then it is possible that your executor doesn't have docker client installed. Docker client needs to be installed in the container to run docker commands, so we have to include a step for installing the docker client.

Lastly set the `$DOCKER_USER` and `$DOCKER_PASS` environment variables to be able to publish to docker hub. Since it is sensitive data, they can be configured using `Enviroment Variables` menu under the `Project Settings` from the CircleCI UI. Those environment variables will be baked in to the container while setting up.


```yml
version: 2.1 # CircleCI api version
jobs:
  build:
    docker: # set the executor type
      - image: openjdk:11 # 
    environment: # set env vars to be baked in to the container
      - SBT_VERSION: 1.4.2
    steps:
      - checkout # Checkout from origin branch
      - run:
          name: Install sbt # Download and install sbt binary
          command: |
            apt update && apt install -y curl
            curl -L -o sbt-$SBT_VERSION.deb https://dl.bintray.com/sbt/debian/sbt-$SBT_VERSION.deb
            dpkg -i sbt-$SBT_VERSION.deb
            rm sbt-$SBT_VERSION.deb
      - run:
          name: Install docker # Download and install docker client
          command: |
            curl -fsSL https://get.docker.com -o get-docker.sh
            sh get-docker.sh
      - setup_remote_docker # Setup remote docker environment
      - run:
          name: Docker login # Login to docker
          command: |
            docker login -u $DOCKER_USER -p $DOCKER_PASS
            docker version --format '{{.Server.APIVersion}}'
      - run:
          name: Clean # Clean project
          command: sbt clean
      - run:
          name: Compile # Build project
          command: sbt compile
      - run:
          name: Test # Run tests
          command: sbt test
      - run:
          name: Publish # Publish docker container
          command: sbt docker:publish

```
Now let's push our changes to Github and open CircleCI from your browser. You can login with your Github account and set up your project with few easy steps. After setting up the project on CircleCI, you can see the CircleCI start running our job. Since we have only one job in our pipeline, it will be started automatically. 

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/circleci/circleci-success.png"/>

AS our last step completed, we should be able to see our image published to Dockerhub successfully.

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/circleci/dockerhub-success.png"/>