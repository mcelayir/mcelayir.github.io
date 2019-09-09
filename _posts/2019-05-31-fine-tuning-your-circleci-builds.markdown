---
layout: post
title:  "Fine Tuning Your CircleCi Builds"
date:   2019-05-31
categories: posts
---
# Overview

You can find the github repository for the tutorial <a href="https://github.com/muratcancelayir/sbt-circle-ci-integration">here</a>

In the previous <a href="https://muratcancelayir.github.io/posts/2019/05/31/sbt-circleci-integration.html">tutorial</a>, we created a simple scala application and compiled, tested and published docker image for that project. If you check the tutorial you can find some flaws about the configuration, which can be described as:

<ul>
    <li>In each job, same applications are downloaded and installled </li>
    <li>Sbt downloads same dependencies for each job in the workflow</li>
</ul>

Because of this, we are spending some of our build time just to download and install sbt, curl and download the dependencies for the project. So let's check the ways to get rid of them.

# Caching and Workspaces

We can make the data flow between jobs in two directions. If you want to use the data for different executions of the workflow then you need caching and if you want to share the data between the <b>jobs</b> of the workflow then you need workspaces.

With the following images, you can understand the concept better

## Caching

<img src="https://circleci.com/blog/media/Diagram-v3-Cache.png"
     alt="Caching"
     style="float: left; margin-right: 10px; margin-bottom: 10px;" />

## Workspaces

<img src="https://circleci.com/blog/media/Diagram-v3-Workspaces.png"
     alt="Workspaces"
     style="float: left; margin-right: 10px; margin-bottom: 10px;" />

## Initial situation

Before we begin, let's evaluate the current situation first.

At this point, our configuration file: 
```yml
jobs:
  compile:
    docker:
    - image: openjdk:8
    environment:
    - SBT_VERSION: 1.2.8
    steps:
    - run:
        name: Get sbt binary
        command: |
          apt update && apt install -y curl
          curl -L -o sbt-$SBT_VERSION.deb https://dl.bintray.com/sbt/debian/sbt-$SBT_VERSION.deb
          dpkg -i sbt-$SBT_VERSION.deb
          rm sbt-$SBT_VERSION.deb
    - checkout
    - run:
        name: Clean
        command: sbt clean
    - run:
        name: Compile
        command: sbt compile
  test:
    docker:
    - image: openjdk:8
    environment:
    - SBT_VERSION: 1.2.8
    steps:
    - run:
        name: Get sbt binary
        command: |
          apt update && apt install -y curl
          curl -L -o sbt-$SBT_VERSION.deb https://dl.bintray.com/sbt/debian/sbt-$SBT_VERSION.deb
          dpkg -i sbt-$SBT_VERSION.deb
          rm sbt-$SBT_VERSION.deb
    - checkout
    - run:
        name: Test
        command: sbt test
  publish:
    docker:
    - image: openjdk:8
    environment:
    - SBT_VERSION: 1.2.8
    steps:
    - run:
        name: Get sbt binary
        command: |
          apt update && apt install -y curl
          curl -L -o sbt-$SBT_VERSION.deb https://dl.bintray.com/sbt/debian/sbt-$SBT_VERSION.deb
          dpkg -i sbt-$SBT_VERSION.deb
          rm sbt-$SBT_VERSION.deb
    - setup_remote_docker
    - run:
        name: Install Docker client
        command: |
          set -x
          VER="17.03.0-ce"
          curl -L -o /tmp/docker-$VER.tgz https://get.docker.com/builds/Linux/x86_64/docker-$VER.tgz
          tar -xz -C /tmp -f /tmp/docker-$VER.tgz
          mv /tmp/docker/* /usr/bin
    - run:
        name: Docker login
        command: docker login -u $DOCKER_USER -p $DOCKER_PASS
    - checkout
    - run:
        name: Publish
command: sbt docker:publish

```

So what are the problems with this configuration?

<ul>
    <li>If we check the logs of the test and publish jobs, we will see that, before invoking the first sbt command, dependencies for the project are downloaded again in each job.</li>
    <li>Same applications are downloaded again for each job</li>
</ul>

Because of these reasons, the execution time for each job to run gets longer.

Execution time for each job in the workflow can be observed by using the Workflows tab in the dashboard.
<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/circleci/master-initial.png"
    alt="Initial durations"
    style="float: left; margin-right: 10px; margin-bottom: 10px; margin-top: 10px;">


## Introducing workspaces

In each of the jobs, an sbt command is executed. By running an sbt command, we make sbt download all the dependencies for the project because each job is executed on its own freshly created container. By using workspaces, we can move the data from one container to another container in one workflow.

We can persist a temporary file to be used by another job in the workflow by using `persist_to_workspace` keyword. To use this keyword, we have to specify a root directory which is the directory on the container which is taken to be the root directory of the workspace and paths for the files or folder that we want to share. Paths values should be specified relative to the root.

Documentation for `persist_to_workspace` can be found <a href="https://circleci.com/docs/2.0/configuration-reference/#persist_to_workspace">here</a>

Since we want to persist the dependencies, we can add following configuration to job `compile` after then the step `sbt compile`. 
```
- persist_to_workspace:
        root: /root
        paths:
        - .ivy2/cache
        - .sbt
        - .m2
```

You might be suspicious about the line `root: /root`. `root` that we specified as a path, points the root folder in the container.

To access the workspace, we use `attach_workspace` <a href="https://circleci.com/docs/2.0/configuration-reference/#attach_workspace">command</a> and specify the path for the workspace. We can specify our workspace as follows:

```
- attach_workspace:
        at: /root
```

Now, we saved the dependencies while executing the `compile` job and retrieve them to use in `test` and `publish` jobs. Therefore we expect a change in the execution time for these jobs.

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/circleci/m1-workspace.png"
    alt="Workspace created"
    style="float: left; margin-right: 10px; margin-bottom: 10px; margin-top: 10px;">

As you can see execution time for `compile` didn't change much however it is improved for the others.
We can improve this by caching the artifacts to use in later executions. 

In this example caching the artifacts to prevent downloading same artifacts for each run makes much more sense however you can make use of workspaces in situations such that a job depends on an artifact as an output of another job.

## Caching dependencies

To cache the dependencies, instead of `persist_to_workspace`, we are going to use `save_cache`.

```
- save_cache:
          key: sbt-cache
          paths:
            - "~/.ivy2/cache"
            - "~/.sbt"
            - "~/.m2"
```

To restore from cache:

```
- restore_cache:
    key: sbt-cache
```

And don't forget to add `restore_cache` to `compile` job. We can restore the caches before checking out the code. This will come in handy when we decide to cache the source code also :wink:

Let's observe the execution time.

First run after the push:

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/circleci/m2-cache-1.png"
    alt="Workspace created"
    style="float: left; margin-right: 10px; margin-bottom: 10px; margin-top: 10px;">

As you can see `compile` job took a little bit longer to complete because of the newly added step, but other jobs took the same amount of time to complete. Now run it again.

Second run after the push:
<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/circleci/m2-cache-2.png"
    alt="Workspace created"
    style="float: left; margin-right: 10px; margin-bottom: 10px; margin-top: 10px;">

As the dependencies are in cache ready to roll, `compile` job didn't have to download them again but instead executed the command directly. 

# Creating a custom image

By using caching, we saved ourselves from downloading the same dependencies over and over again and improved our execution time for the workflow. Now we are ready to tackle the other problem, which is 
downloading the same applications for each job over and over again.

We need sbt and docker to complete the execution of the workflow. Instead of downloading and installing them in the context of each job, we can create a docker image with them and use that image in each job.

Although there are some other solutions and workarounds to cache and reuse the downloaded applications, this solution is actually more convenient since we can use the same image for different tasks, instead of creating and maintaining caches.

So let's start creating a docker image. This is actually pretty straight forward, we are going to run the same commands, however this time we are going to do this in a Dockerfile.

```
FROM openjdk:8
RUN apt update && apt install -y curl
ENV SBT_VERSION 1.2.8
ENV DOCKER_VERSION 17.03.0-ce


RUN curl -L -o sbt-$SBT_VERSION.deb https://dl.bintray.com/sbt/debian/sbt-$SBT_VERSION.deb && \
    dpkg -i sbt-$SBT_VERSION.deb && \
    rm sbt-$SBT_VERSION.deb

RUN curl -L -o /tmp/docker-$DOCKER_VERSION.tgz https://get.docker.com/builds/Linux/x86_64/docker-$DOCKER_VERSION.tgz && \
    tar -xz -C /tmp -f /tmp/docker-$DOCKER_VERSION.tgz && \
    mv /tmp/docker/* /usr/bin
```

Let's go over the file.

We are going to use `openjdk:8` image as a base image for our image. Then we installed curl to be able to make HTTP calls and lastly, we downloaded and installed `sbt` and `docker` apps.

Build the image with the command

```
 docker build -t <<tag>> .
```

And push it with command

```
 docker push <<tag>>
```

Now we can start using our in our `config.yml` and get rid of the commands to download and install the apps that we included in our docker image.

# Latest status

Let's check the latest status for our configuration file and execution times.

`config.yml`:

```yml
version: 2

jobs:
  compile:
    docker:
    - image: mcelayir/sbt-jdk:latest
    steps:
    - restore_cache:
        key: sbt-cache
    - checkout
    - run:
        name: Clean
        command: sbt clean
    - run:
        name: Compile
        command: sbt compile
    - save_cache:
        key: sbt-cache
        paths:
        - "~/.ivy2/cache"
        - "~/.sbt"
        - "~/.m2"
  test:
    docker:
    - image: mcelayir/sbt-jdk:latest
    steps:
    - restore_cache:
        key: sbt-cache
    - checkout
    - run:
        name: Test
        command: sbt test
  publish:
    docker:
    - image: mcelayir/sbt-jdk:latest
    steps:
    - setup_remote_docker
    - run:
        name: Docker login
        command: docker login -u $DOCKER_USER -p $DOCKER_PASS
    - restore_cache:
        key: sbt-cache
    - checkout
    - run:
        name: Publish
        command: sbt docker:publish

workflows:
  version: 2
  build-and-test-and-deploy:
    jobs:
    - compile
    - test:
        requires:
        - compile
    - publish:
        requires:
        - test
```

As you can see, we removed the overhead of downloading and installing applications from our configuration file. This way our configuration file looks more simple and clean. Also by introducing 
caching for the dependencies we improved our execution time.

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/circleci/m3-custom-image.png"
    alt="Workspace created"
    style="float: left; margin-right: 10px; margin-bottom: 10px; margin-top: 10px;">

# References

<ul>
    <li><a href="https://circleci.com/docs/2.0/configuration-reference/">https://circleci.com/docs/2.0/configuration-reference/</a></li>
    <li><a  href="https://circleci.com/blog/deep-diving-into-circleci-workspaces/">https://circleci.com/blog/deep-diving-into-circleci-workspaces/</a></li>
</ul>

