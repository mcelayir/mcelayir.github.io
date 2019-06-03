---
layout: post
title:  "Start building your scala project with CircleCi"
date:   2019-05-31
categories: posts
---
# Overview

This tutorial is intended to give information about circleci and demonstrate its basic concepts by integrating circleci with a scala project. We are going to automate and orchestrate the steps necessary to compile, test and publish

You can find the github repository for the tutorial <a href="https://github.com/muratcancelayir/sbt-circle-ci-integration">here</a>

The second part of this tutorial `Fine Tuning Your CircleCi Builds` can be found <a href="https://muratcancelayir.github.io/posts/2019/05/31/fine-tuning-your-circleci-builds.html">here</a> 

# CircleCi

## What is continuous integration (CI)

Continuous integration (CI) is a practice that requires developers to commit
and integrate their codes into a central repository. Instead of building
and verifying the integrity in an isolated system, the integrated code is built
and verified by each commit. This allows teams to detect problems and take
preventive actions.

## What is CircleCi

As you can guess from the context, <b>circleci</b> is yet another CI service that
you can make use of. 

With <b>circleci</b> you can utilize CI in your development process easily. As you will
later see in this tutorial, you can spin up everything with a simple yml file.
You can configure workflows and orchestrate them in various ways depending
on your requirements. In a nutshell you can:

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
    <li> Community support </li>
</ul> 

# Configuration

CircleCI 2.0 requires `config.yml` file to be under the
`.circleci` directory which should be under the projects root directory.
Thus the file location should follow `.circleci/config.yml`

To achieve that, you can execute following commands in your projects root
directory.

```
> mkdir .circleci/
> touch .circleci/config.yml
```

## config.yml

### version

Circleci config.yml should specify its version in the beginning of the file
so we start with stating the version.

```yml
version: 2
```

### jobs & build

Before defining the steps we want to run, we have to configure circeci
with an entry point and a job to run. To provide an endpoint, we can use
the `build` keyword and we can create jobs to run under the keyword `jobs`.


## Configuring jobs

### Defining docker images

Docker images needed for the job can be defined under `docker`section as
shown below. Given images will be pulled by circleci.

```yml
  jobs:
    build:
        docker:
            - image: <<image-name>>
            - image: <<image-name>>
            ...
```

Since we are going to build a sbt project, we can use the official docker
image for `openjdk8`

```yml
  jobs:
    build:
        docker:
            - image: openjdk:8
```

### Defining environment variables

Values that we want to use during the execution of the job 
should be defined under the `environment` section as shown below.

```yml
jobs:
    build:
        environment:
            - MY_VAR_1: <<some-value>>
            - MY_VAR_2: <<some-value>>
```

Since we need sbt, we can set the version that we want to download 
as an environment variable. (yes we are going to download that :sweat_smile:) 

```yml
jobs:
    build:
        environment:
            - SBT_VERSION: 1.2.8
```

### Setting up steps

We can define each command that we want to run under the `steps` section.
Commands can be defined by the `run` keyword.

```yml
jobs:
    build:
        steps:
            - run: <<command>>
            - run: <<command>>
```

Also a name can be defined for a `step`to identify its purpose.

Multiline commands can be defined by using a pipe `|` at the beginning
of the command

```yml
jobs:
    build:
        steps:
            - run: 
                name: Say hello
                command: |
                          apt update && apt install -y curl
                          apt-get update
                          apt-get install -y sbt python-pip git
                          ....                        
```

All the given commands will run inside the docker container for the job.

Now we can start defining the steps needed to compile and execute other
commands for our project.

### Installing the Sbt

Right now we have an empty container and to run our jobs we have to install
the tools that we need.

To install the sbt, we have to install the `curl` application to be able to download
the artifacts via http calls. Downloaded artifact will be installed by the `dpkg` command.
We can run all the commands in one step by using the pipe `|` as described above.

```yml
steps:
    - run:
        name: Get sbt binary
        command: |
                 apt update && apt install -y curl
                 curl -L -o sbt-$SBT_VERSION.deb https://dl.bintray.com/sbt/debian/sbt-$SBT_VERSION.deb
                 dpkg -i sbt-$SBT_VERSION.deb
                 rm sbt-$SBT_VERSION.deb

```

### Checking out the code

After installing the `sbt`, we need our source to be available. We can make
circleci to do a `git checkout`by utilizng the keyword `checkout` as a step
in the job. After this step is executed, the project will be cloned into container

```yml
steps:
    - run: ...
    - run: ...
    - checkout  
    ...
```

### Running sbt commands

Now we have our code ready and sbt installed, we can define the steps to 
clean, compile and test our project

```yml
steps:
    - run:
        name: Clean
        command: sbt clean
    - run:
        name: Compile
        command: sbt compile
    - run:
        name: Test
        command: sbt test
```

### Setting up your build on circleci

Up to now, our `config.yml` should look like this.

```yml
version: 2
jobs:
  build:
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
    - run:
        name: Test
        command: sbt test
```

At this point I assume that you have a github repository for your project. If you
don't then create one and push your code to that repository (chap! chap! :sweat_smile:).

If you have, then you can continue enjoying this tutorial from this line :laughing:

Now, navigate to <a>https://circleci.com</a> and signup. You can also select
`Start with github` option.

Go to add project section. Be sure that your github account is selected in 
the dropdown on the top-left of the page. You can click the `Set Up Project`
button next the project that you want to select.

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/circleci/circleci-select-project.png"
     alt="Select project"
     style="float: left; margin-right: 10px;" />

In the next page, you will see circleci will try to offer you a config.yml file
and instructions to add the file to your project (that's why circleci is the man).
Since we have configuration ready, we can click on `Start Building` button.

After you select `Start Building` circleci will trigger the build of your
project automatically and you will see your project built.

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/circleci/circleci-builds.png"
     alt="Builds"
     style="float: left; margin-right: 10px;" />


# Introducing workflows

Until now we have a basic configuration to build and test our project but circleci
can offer more. We can make use of the workflows feature to increase our total control
over the whole process.

According to circleci, a workflow is
`a set of rules for defining a collection of jobs and their run order. 
Workflows support complex job orchestration using a simple set of configuration 
keys to help you resolve failures sooner.`

To simply put, with workflows, you can:
<ul>
    <li>Run jobs independently</li>
    <li>Schedule jobs</li>
    <li>Compose jobs</li>
    <li>Orchestrate jobs</li>
    <li>Configure different stages</li>
</ul>

So let's start.

## Creating jobs

Previously we configured a single job to build and test our application. Lets seperate that into two jobs and combine them in a workflow.

```yml
version: 2
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
```

Notice that we had to download sbt and checkout the code for both jobs since the commands for the job will run in the job's context and each job 
will run the commands in its own container. So seperating build and test for an sbt project does not make much sense.

### Creating a sequential workflow

A sequential workflow requires the previous step run and complete succesfully for the current step start running.  

<img src="https://circleci.com/docs/assets/img/docs/sequential_workflow.png"
     alt="Sequential workflow"
     style="float: left; margin-right: 10px;" />


Each workflow requires a unique name and a map. In the map we can configure the jobs that we want to run, schedule 
the workflow using cron timers and so on. For more details you can check <a href="https://circleci.com/docs/2.0/configuration-reference/#workflows">workflow configuration
reference</a>.

We can configure our workflow to execute build then test as following:

```yml
workflows:
    version: 2
    build-and-test:
        jobs:
            - compile
            - test:
                requires:
                    - compile
```

Let's observe the configuration
<ul>
<li> <b>workflows</b> defines the section that the workflows are defined (suprisingly :sweat_smile:)</li>
<li> <b>build-and-test</b> is the name of our workflow</li>
<li> <b>jobs</b> are the section that we define our jobs for the workflow</li>
<li> <b>compile</b> job is configured as the first job to run in the workflow</li>
<li> <b>test</b> job is configured as the job which will start when the <b>compile</b> job completes</li>
</ul>

After pushing your code, circleci will start executing your workflow
Navigate to `Workflows` tab in circleci dashboard to see the result of your workflow

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/circleci/circleci-workflow-build.png"
     alt="Workflow result"
     style="float: left; margin-right: 10px;" />
     
### Fan in/out workflow example

You may want to compose your workflow in a way that it will run some jobs sequentially and
some jobs in parallel. In this case you can take advantage of `Fan In/Out` workflow.

<img src="https://circleci.com/docs/assets/img/docs/fan_in_out.png"
     alt="Workflow result"
     style="float: left; margin-right: 10px;" />

To be more specific, let's imagine a use case that you want to first compile your project,
then run some test in parallel and if all the tests pass successfully, you want to deploy your application.

To do that, we can define a workflow like this:

```yml
workflows:
    version: 2
    build-test-deploy:
        jobs:
            - compile
            - unit-test:
                requires:
                    - compile
            - integration-test:
                requires:
                    - compile
            - deploy:
                requires:
                    - unit-test
                    - integration-test
```

In this case, as soon as the `compile` job finishes, `unit-test` and `integration-test` jobs will start.
The `deploy` job must wait for all test jobs to complete successfully before it starts.

# Creating and deploying docker images

As the last part of this tutorial, we are going to build a docker image for our project
and publish it to docker hub by using circleci.

To create docker image for a sbt project, we are going to use `SBT Native Packager`
plugin. To activate the plugin for your project, simply add the following line to
`plugins.sbt` file.

```scala
addSbtPlugin("com.typesafe.sbt" % "sbt-native-packager" % "1.3.12")
```

After that you have to enable plugins `JavaAppPackaging` and `DockerPlugin` in your `build.sbt`
file.

```scala
enablePlugins(
    JavaAppPackaging,
    DockerPlugin
)
```

Now let's define the repository that we want our image to be pushed. To do that
add the following line to your project settings.

```scala
dockerRepository := Option("<<repository name>>")
```

You can test the image creation by using the command

```
sbt docker:publishLocal
```

## Adding publish job

Let's add a new job to create and publish a docker image for our project.

To build our docker image we need a docker engine. We can use 

```yml
- setup_remote_docker
```

command to tell circleci to create a remote docker engine for us and the container for the job will be 
configured to use it.

After that, we need to install the docker client to be able to build and push docker images. Lets add the following 
step to our job to install the docker client

```yml
- run:
        name: Install Docker client
        command: |
          set -x
          VER="17.03.0-ce"
          curl -L -o /tmp/docker-$VER.tgz https://get.docker.com/builds/Linux/x86_64/docker-$VER.tgz
          tar -xz -C /tmp -f /tmp/docker-$VER.tgz
          mv /tmp/docker/* /usr/bin
```

OK, docker client installed and now we have to authenticate to be able to publish our image to docker hub. But wait
how can we supply our credentials for that. Are we going directly write our credentials to our config.yml and expose our 
credentials. Luckily circleci have a solution for that. You can define environment variables for your project.

Simply navigate your project settings and select `Environment Variables` under `Build Settings`

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/circleci/circleci-environment-variables.png"
     alt="Environment variables"
     style="float: left; margin-right: 10px;" />

and add `DOCKER_USER` and `DOCKER_PASS` environment variables for your project. Then we can add the following command 
to login to docker.

```yml
- run:
        name: Docker login
        command: docker login -u $DOCKER_USER -p $DOCKER_PASS

```

Lastly, add the following command to invoke docker publish.

```yml
    - run:
        name: Publish docker artifacts
        command: sbt docker:publish

```

### Adding to workflow

We need to add the publish job as the last job in the workflow. We can configure the publish job to run when
the test job completes.

```yml
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

Now, if we execute the workflow, we can observe its result.

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/circleci/circleci-workflow-success.png"
     alt="Workflow success"
     style="float: left; margin-right: 10px;" />

Also we can check if the docker image is published or not.

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/circleci/dockerhub.png"
     alt="Docker hub"
     style="float: left; margin-right: 10px;" />
     

# References

- <a>https://www.thoughtworks.com/continuous-integration</a>
- <a>https://circleci.com/docs/2.0/about-circleci/</a>
- <a>https://hackernoon.com/continuous-integration-circleci-vs-travis-ci-vs-jenkins-41a1c2bd95f5</a>
- <a>https://circleci.com/docs/2.0/language-scala/</a>
- <a>https://circleci.com/docs/2.0/tutorials/</a>

