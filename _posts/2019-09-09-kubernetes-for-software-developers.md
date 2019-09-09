---
layout: post
title:  "Kubernetes for software developers"
date:   2019-09-09
categories: posts
---
# Kubernetes for software developers

# What is Kubernetes?

Kubernetes is a portable, extensible, open-source platform to orchestrate containerized workloads and services.

To explain what Kubernetes can do for you, lets briefly explain what are the containers.

## Containers

Docker defines containers as `A standardized unit of software` which means that
a container is basicly a software which is packaged up with all its dependencies so that
the software can run reliably on different environments.

Most of the time, we faced the same software producing different outputs on different environments ("Works on my machine" sounds familiar I guess). A slight difference between the versions of a dependency, or a missing environment variable is more than enough to give you headaches and sleepless nights.

So how does containers fix this problem. Containers are basicly an operating system virtualizations bundled with the resources needed to run the application. A container
contains executables, binaries and config files which are not going to change from environment to environment. When you spin up your container, you can be 100% sure that
it is going to spin up as how you configured it.

## Kubernetes vs Docker

Actually this comparison doesn't make sense at all since they are not alternatives but complementary products. Docker is a software that you can install on your computer and start creating and running containers. On the other hand with kubernetes you can orchestrate the containers as pods. Kubernetes allows container provisioning, networking, load balancing and scaling. At the end of the day, we can develop our applications with Docker and use Kubernetes to orchestrate them.

Let's imagine a scenario that you want to run your application on Kubernetes and let's say that you need 10 nodes for your application. Some of your nodes might use Docker and some of them might prefer Containerd container runtimes. Kubernetes allows you to run and manage both of them. This means that Docker/Containerd is the low-level technology that starts and stops containers and so on, and Kubernetes is the higher-level technology that looks after the bigger picture.

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/kubernetes/B15101_01_02.jpg"/>

## Kubernetes vs Docker Compose

Docker compose helps you to run multi container application in a single host. It saves you starting multiple containers and creating the network which allowes them to communicate. Docker compose will spin up your services as containers and create the network so that they can communicate with each other as they are different hosts whereas
Kubernetes is an orchestration which can run and manage containers on multiple hosts. It handles scheduling of containers onto nodes in a cluster and manages workloads. Kubernetes will create the illusion of running the components on a single large computer,
whereas the components are running on different machines.

So what can Kubernetes do for you:

- Manage your containers at a scale
- Automated rollouts and rollbacks
- Service discovery and load balancing
- Storage orchestration

To understand this capabilities let's explore kubernetes further

# Kubernates Basics

## Kubernetes facade

On the top level, kubernetes provides two things for you:

- A cluster to run your applications
- An orchestrator to ensure that things are running smoothly

On cluster part, Kubernetes provides a bunch of nodes and a control plane. The control plane, provides an interface to receive configurations, a scheduler to distribute the work
between nodes and a persistent store to keep track of the state.

On orchestration part, kubernetes organizes everything into a useful app, ensures that things are running smoothly and responds to changes in circumstances.

### Control Plane and Masters

The state of all of the Kubernetes objects are maintaned by the `Control Plane`. Control plane is also responsible from running continous checks to manage the objects state.
These loops will detect and respond to changes in the states to ensure that current state matches the desired state.

Control plane also provides the API server which is responsible from handling all of the communication between components. External user components like Deployments are provided over this API.

For example when a deployment provided, the yaml configuration file is posted to the API server and transated into a new desired state. Then this new desired state is stored by the control plane and bunch of instructions are created to start the applications and schedule those instructions to cluster nodes to make sure that current state matches the desired state.

### Nodes

Nodes are the workers of a Kubernetes cluster. At a high-level, they do three things:

- Watch the API Server for new work assignments
- Execute new work assignments
- Report back to the control plane

<img src="https://www.cloudbees.com/sites/default/files/kubernetes-architecture.png"  height="700"/>

# Pods

What Container means for the Docker world, is as same as the `Pod` in Kubernetes world.
They are the atomic unit of scheduling.

It's true that Kubernetes runs containerized apps. However, you cannot run a container directly on a Kubernetes cluster. Containers must always run as wrapped inside of Pods.

A Kubernetes Pod can run multiple containers inside. It is useful to note that, the pod itself doesn't actually run anything, it is just the shell that containers run inside.
All containers that are running inside the same pod, share the same environment. That means that, they will use the same memory, volumes and network.

Multiple containers in the same pod will come in handy that if you have tightly coupled containers that needs to be share the same memory and resources. However keep in mind that, the best practice is to create a Pod for a container and use complementary containers if needed. 

Also as we said that the Pods are the atomic unit of scheduling, pods are added or removed to scale the application (not by adding more containers into a Pod) so it is useful to design the application with that sense.

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/kubernetes/Screenshot+2019-09-09+at+11.42.20.png"/>

Lastly, it is useful to keep in mind that, pods are mortal and they can die unexpectedly. When a pod dies, instead of trying to bring it back to life, a new instance will be started to replace the dead one.

The Pod concenpt affects the design the components of the application. It is better to design components to be loosely coupled, stateless and has a single responsibility.

# Deployments

A `Deployment` is a kubernetes object, which makes easy to manage a set of identical pods. Without Deployment, one has to create, delete and update the pods manually. Deployments makes sure that one or more instances of your application are available to serve user requests.

Benefits of using a Deployment:
- Deploying a ReplicaSet with multiple instances of the given pod.
- Apply updates and rollbacks
- Declare a new state for the pods
- Scale up and down

# Services

A `Service` exposes an application running on a set of Pods as a network service. As mentioned pods are mortal and pods can be created and destroyed by a `Deployment`. When a pod created, it is created with its own IP address, however since the pods can die and a new pod will be created to replace the old one, using the assigned IP to access to the instance is not a convenient way.

Imagine a scenarion that involves some frontends needs to access some backends. In this case frontends needs to keep track of track of the backend addresses and find the appropriate address to interact with the backend.

To overcome this issue, `Services` could be used. A Service defines a set of pods and the way to access them. Services also provide load-balancing and service discovery between applications.

# Example

We are going to realize a simple url shortener application on Kubernetes. The application depends on three components to do its job. `Creator` is responsible to create a short key for the given url. `Fetcher` is responsible to retrieve the url for given short key. Lastly the data will be stored on `Postgres` DB.

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/kubernetes/Screenshot+2019-09-09+at+13.48.45.png"/>

The source code along side with the kubernetes and docker-compose files can be found at
https://bitbucket.org/urlshort/profile/repositories

## Postgress

Let's start by deploying the database since every other component in the system depends on it.

### Configuring the database
Postgres uses several environment variables to set a user and its password and setting the name for the default db. We can use a config map to set those environment variables. 

```yaml
kind: ConfigMap
apiVersion: v1
metadata:
  name: postgres-config
  labels:
    app: postgres
data:
  POSTGRES_PASSWORD: "123456"
  POSTGRES_USER: "admin"
  POSTGRES_DB: "urlshort"
```
### Creating tables

Postgres allows users to do additional initialization. Script files copied to `/docker-entrypoint-initdb.d` will be run after the container started. To start the database with tables created, we are going to use another config map and then mount it to given path.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: postgres-initdb-configmap
data:
  initdb.sql: |
    CREATE TABLE shorturls (
        shorturl varchar(255),
        url text,
        userid varchar(255),
        created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
        PRIMARY KEY (shorturl)
    );
```

When mounted, this config map will create the `initdb.sql` file in the given directory.

### Deployment

```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: postgres
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: postgres
    spec:
      containers:
      - name: postgres
        image: postgres:10.4
        imagePullPolicy: "IfNotPresent"
        ports:
        - containerPort: 5432
        envFrom:
        - configMapRef:
            name: postgres-config
        volumeMounts:
        - mountPath: /docker-entrypoint-initdb.d
          name: postgredb-init-data
      volumes:
      - name: postgredb-init-data
        configMap:
          name: postgres-initdb-configmap
```

As you can see, the environment variables are attached by using `envFrom`and providing the name.

For the database initialization script, the config map is mounted as a volume to `/docker-entrypoint-initdb.d`

### Service

We need to define a service for postgres so that other components can access the database.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: postgres
spec:
  ports:
  - port: 5432
  selector:
    app: postgres
```

All of the declerations can be merged in a single yaml file and the file can be applied as
```
> kubectl apply -f postgresql.yml
configmap/postgres-config created
configmap/postgres-initdb-configmap created
deployment.extensions/postgres created
service/postgres created
```

We can check the pods, to see if the database is running.
```
> kubectl get pods
NAME                       READY   STATUS    RESTARTS   AGE
postgres-797694b9f-9vvmz   1/1     Running   0          74s
```

To check the logs for the database (actually for any pod)
```
> kubectl logs -f postgres-797694b9f-9vvmz
```

Let's see if the service is created.
```
> kubectl get svc                         
NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP    3d3h
postgres     ClusterIP   10.96.128.189   <none>        5432/TCP   8m9s
```

Since the `postgres` service is defined as `ClusterIP`. This means that, the services inside the cluster can access the postgres but no access from the outside. To access the database, we must directly access to the Pod. 

Now, let's connect to database and check everything is there.

```
> kubectl exec -i postgres-797694b9f-9vvmz sh
psql -U admin --password urlshort
Password for user admin: 123456

select * from shorturls;
 shorturl | url | userid | created_at 
----------+-----+--------+------------
(0 rows)

```

## Creator and Fetcher services

Creating deployments and services for these services are pretty straightforward. However, I think this is a nice point to talk about `Init Containers`. 

### Init Containers

We know that, a pod can have multiple containers running inside, but it can also have one or more init containers, which are going to run before the application containers are started.

Keep in mind that init containers
- Always run to complation
- An init container must finish so that the next one can start.

Let's discuss a scenarion that we would like to create pods for these services if the database is ready because if it is not then having those services doesn't have a point. To overcome this situation we can add an init container to check the readiness of the database.

```
initContainers:
      - name: check-db-ready
        image: postgres:9.6.5
        command: ['sh', '-c',
                  'until pg_isready -h postgres -p 5432;
                do echo waiting for database; sleep 2; done;']
```

This init container will start before the app container started. It is going to execute `pg_isready` to check if the database is ready. When the command returns a successfull response, the init container will be completed and the app container can be started.

Also its important to note that, we are able to use `postgres` as the hostname to access the database since we have the service defined. 

## Testing the application

To access the services, we need their external ip.

```
> kubectl get svc 
NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                         AGE
creator      NodePort    10.99.202.57     <none>        8080:31202/TCP,9095:30294/TCP   2m38s
fetcher      NodePort    10.111.102.234   <none>        8080:31159/TCP,9095:32661/TCP   2m8s
kubernetes   ClusterIP   10.96.0.1        <none>        443/TCP                         3d3h
postgres     ClusterIP   10.96.128.189    <none>        5432/TCP                        36m
```

Services and ports are defined to access the components. To access the creator, we should hit `localhost:31202` and for the fetcher `localhost:31159`

Let's hit the creator to create a short url
<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/kubernetes/Screenshot+2019-09-09+at+16.05.30.png" />

Now, let's query with the key to get the original url.
<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/kubernetes/Screenshot+2019-09-09+at+16.06.29.png" />


# References
- https://www.devon.nl/en/what-are-containers/
- https://www.netapp.com/us/info/what-are-containers.aspx
- https://dzone.com/articles/kubernetes-in-10-minutes-a-complete-guide-to-look
- https://medium.com/google-cloud/kubernetes-101-pods-nodes-containers-and-clusters-c1509e409e16
- https://www.redhat.com/en/topics/containers/what-is-kubernetes
- https://www.infoworld.com/article/3268073/what-is-kubernetes-container-orchestration-explained.html
- https://www.toptal.com/kubernetes/what-is-kubernetes
- https://www.packtpub.com/cloud-networking/the-kubernetes-book
- https://matthewpalmer.net/kubernetes-app-developer/articles/kubernetes-deployment-tutorial-example-yaml.html
- https://hub.docker.com/_/postgres