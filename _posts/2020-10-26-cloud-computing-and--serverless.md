# Cloud Computing & Serverless

## Introduction

Initially the term serverless were used to describe applications in which the functionality is covered by third-party services and/or custom code running in containers that are maintained by the service provider. The key here is not to manage any servers but to pay for the service or functionality that you use. This approach removes the complexity of running and maintaining infrastructure as well as removing the costs. 

With the rise of the technology provided by cloud computing companies like Amazon and Google, the term serverless is also used to describe a wide range of products provided by these companies.

In this article, I would like to give information about cloud computing and computing models and then talk about serverless applications and possible use cases.

## Cloud Computing

### Evolution of technology

Let's go back to the early days (the 90s if possible) and meet our friend Peter who owns a bookstore and wants to grow his business by selling books online (visionary guy). 

Peter hires some developers and asks them to deliver his online bookstore. When the application is delivered, Peter realizes that he has to put the application somewhere so that people from his town can access his website and start ordering books. After some investigation, Peter understands that he has to buy some sort of special computer called `server` so he decides to buy one.

He goes back to his store with his new special computer and starts setting up. In a couple of hours, his excitement turns into frustration because simply doesn't understand how all of this works and he is not capable of handling this all by himself. Despairingly, Peter hires a guy to help him with his server. After one week the server guy (the ancestor of the IT department) puts everything together and the bookstore becomes accessible from the internet. It was the first time that Peter saw his investment and he immediately starts testing it. The testing and bug fixing took some time but somehow this phase is passed and now finally Peter thoughts that he can start advertising this online bookstore and start making money out of it.
The next day when Peter goes back to the store he saw his server guy helplessly sitting in front of the `out of service` server. After his advertisement campaign, everybody in the town decided to visit his web page and the single server failed to handle the load.

Long story short Peter bought a couple of servers and he rented another place to contain them (at those days no one knew the word data center). He hired more server guys to maintain his servers. He paid for extra hardware and software to keep his servers safe and healthy.
He was able to serve more customers but the amount that he has to pay to keep his business alive also skyrocketed.

Also, Peter realized that most of the daytime his online store doesn't have too many visitors and his servers are idle. He was paying to be able to handle high traffic, not the traffic that his servers are enduring but he had no other option.

In these years, some other guys invested in servers and built bigger versions of what Peter has called data centers. With the emerging technology called `virtualization`, they were able to run thousands of servers in one of those computers for cheaper prices, and even better you also don't have to upkeep them by yourself. Trained professionals for this purpose will maintain these servers and they guarantee that your servers will be available all the time. This was the best choice for most of the business owners to pursue their digital affairs so they start carrying their applications to these vendors and started to rent servers that are far away from them.

### Cloud Computing Models

Up to this now it was a bit slow and painful (ask Peter about it) but I promise it will get faster and exciting. Web hosting companies failed to meet their growing computing needs and running IT infrastructure on-premises was getting costly and complex every day. This paved the way for cloud companies to take over and start providing solutions for vastly varied customer groups having different requirements and needs. These solutions can be considered under the four main types of cloud computing.

#### Infrastructure as a Service (IaaS)

Emerging cloud providers start providing computing infrastructure as a service that allows IT professionals to create virtual machines with desired computing power with a single click and terminate it with another click. Instead of building and maintaining an on-premise IT structure which is costly and requires labor, people started to buy this infrastructure as a service. Iaas solutions bring flexibility and scalability and providing more control over the infrastructure whereas taking away the huge cost of owning an on-premise IT infrastructure.

Even though it brings huge computing power to people's fingertips by abstracting all the infrastructure, it has its downsides. First of all the created VMs has to be managed and maintained. Also, all of the architectural components need to be put together, for example, if you need load balancing, you have to set up all VM instances for your application, the network and the security of the system is under the development teams responsibility. IaaS offers the building blocks but someone has to build the system by using these building blocks. In other terms, it requires a lot of configuration and attention from some IT professionals.

Right now all the cloud providers have an IaaS solution. Some popular examples are

- AWS EC2
- Google Compute Engine
- Microsoft Azure

It's also worth mentioning the costs for IaaS. It makes much more computing power available for cheaper costs. However, in IaaS solutions you rent computing power so you billed based on the computing power that you use. Even though cloud providers provide scaling options that can lower the costs you probably need to run at least one instance for your application availability that means using computing power.

To conclude, IaaS is the closest thing to having a data center. It's like having a data center affiliated with the cloud provider. Moreover, you can spread your data centers across different parts of the globe but it comes with a price.

### Platform as a Service (PaaS)

Platform as a Service is the cloud computing model where the cloud provider provides the hardware alongside the software and tools needed to run the application. PaaS solutions can be thought of as an IaaS solution sophisticated to run your application. 
It is mostly used by developers building software or applications. It simply abstracts away the complexity of building and managing infrastructure so that developers can focus build their apps and run it as they are running on their local machine. PaaS solutions offer tools to deploy your application from docker or Github which can be further automatized by using third-party tools.

Some popular examples of PaaS are
- AWS Elastic Beanstalk
- Heroku

The pricing model for PaaS is the same as IaaS, you pay on-demand basis and pay for the number of resources that you consume. However again for your application to be accessible at least a daemon should run somewhere to handle the requests received which means you have to reserve some capacity and pay for it. However, it is much cheaper than the IaaS and you can find reasonable prices between 5-10$ per month to host your application.

### Function as a Service (FaaS)

Function as a Service (FaaS) aka `the serverless` can be described as an execution model where the cloud provider is responsible to allocate the necessary resources and storage and execute the given piece of code in response to events triggered. Events can an HTTP call to an endpoint or a notification indication file upload completed.  

Cloud provider charges based on the number of times that the function is executed. For example, AWS Lambda’s pricing is set on $0.20 per million requests and $0.00001667 per GB-s (Gigabyte per second) so you get 1 million requests and 400.000 GB-s per month for free.

Some popular examples are
- AWS Lambda
- Google Functions
- Azure Functions

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/serverless/IaaS-PaaS-FaaS+Diagram_ASSIST_Software.png"/>
To conclude, each computing model comes with a trade-off. Depending on the computing model that you choose you to increase or decrease your control over the platform that you run your application. This will affect the design of the application, how you program, and how you deploy. Choosing the right product according to your needs and budget is maybe the first decision that you make since everything will be shaped according to that.

## Serverless 

The term `Serverless` is indeed confusing since the example applications are generally referred to as `client-server` applications where a client calls a server for data or posts data for some purpose. In this case, the term refers to `server` as `hardware` so it stands for `no hardware to manage`. Still, there will be a server process running on some hardware but the owner of the serverless application doesn't have to manage this hardware. The availability of the server process is guaranteed by the vendor.

The purpose of the serverless is to keep developers away from the hustle of creating and maintaining an infrastructure for their applications and let them focus on creating value for their application.

We come across two terms while talking about this concept.

### Backend as a Service (BaaS)

BaaS is used to describing the third-party applications and/or services consumed by the application to deliver functionality. These services are not developed or maintained by the organization. The application talks to these services over the provided protocols. Its also worths mentioning that a Software as a Service (SaaS) solution can be considered as a BaaS if it serves this purpose

### FaaS

FaaS is another way of developing serverless where the application is designed as separate functions executed independently to achieve a task. Functions will run in stateless, ephemeral containers in response to a triggering event. The computing resources necessary to execute the function are the vendor's responsibility.

### Designing for Serverless 


Let's take Peter's bookstore application and take it to serverless. Lets assume that there is an already up and running application that users can

- login to the application
- search for books
- purchase books 

and the existing application is a three-tier application consisting of following layers

- Presentation (Client) Layer
    This layer is where users interact with the application. Application as HTML, CSS and Javascript files will live here and served using by utilizing different technologies.

- Business (Server) Layer
    Business layer will be called from the presentation layer. It is responsible for handling all the requests received from the users like serving the list of books or taking purchase orders.

- Storage (Database) Layer
    This is where the data needed to keep the business alive will be stored.

#### Monolith Phase

Even though the application is divided into layers and the concerns are seperated  the bookstore server is a monolith which means the server is responsible to handle all of the requests coming from the clients and provide services for application to function is running inside the server. 

Now lets examine what kind of issues that we can run into with this design.

- Any failure that can make the server down would break down the entire functionality of the application

- As the size of the application incrase in terms of functinality, the required computing power will also increase. Shortly more RAM more CPU

- Scaling up will be costly

- The complexity of the code will increase as new functinality added. At some point, will become impossible to calculate the impact of changes.
 
<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/serverless/traditional-approach.png"/>

#### Microservices



<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/serverless/serverless-approach.png" />

## When Serverless is not recommended
Alongside all the amazing features that serverless has, it has its downsides.

To begin with, it is not ideal for long-running tasks instead it is better to use dedicated resources for this use case.

Secondly, developers need to be aware of the cold starts. When a function is invoked, the provider will spin up a container and execute the function. After producing the output, the container can be kept alive for a certain amount of time, and then it will be terminated. If another request comes in before the termination, the same container will be used. However, after termination, another container will be created to handle the incoming request. Creating another container from scratch means that functions cannot trust the local storage to load a value needed for the calculation. This can lead to a re-design of the application architecture for performance. Also, keep in mind that cold-starts will introduce a delay in the response time.

Lastly, depending on the cloud provider there will be an overhead of invoking functions. Services could only be access by the defined interfaces of the provider which can limit development and deployment options. Becoming over adapted to a vendor will make things harder in case of a need to change.

## Summary

### XaaS

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/serverless/container-orchestration-wars-2017-edition-21-638.jpg" alt="https://techthought.org/wp-content/uploads/sites/2/2018/07/container-orchestration-wars-2017-edition-21-638.jpg" />

<b>IaaS:</b> Only the base infrastructure. Everything needs to be configured by the end-user.

<b>PaaS:</b> Platform to deploy, run and manage applications without dealing with the infrastructure

<b>FaaS:</b> Platform to deploy, run, and functions in response to events without dealing with any sort of hardware

### Benefits of serverless

Benefits of serverless

- Not managing servers reduces the operational complexity 
- Cost-efficient because of on-demand usage of resources
- Scalability and availability guaranteed by the cloud provider
- Reduces the complexity of writing and maintaining software
- Faster time to market

### Downsides of Serverless

- Becoming vendor dependant. 
- Losing control over platform and infrastructure
- Not efficient for stateful applications and long-running tasks
- Cold-starts
- Brings overhead of adapting the microservices architecture if the organization is not ready


REFS
- https://www.sumologic.com/glossary/function-as-a-service/
- https://www.cloudflare.com/learning/serverless/glossary/function-as-a-service-faas/
- https://www.scalyr.com/blog/function-as-a-service-faas/
- https://martinfowler.com/articles/serverless.html
- https://hackernoon.com/what-is-serverless-architecture-what-are-its-pros-and-cons-cc4b804022e9
- https://calculator.aws/#/createCalculator
- https://assist-software.net/blog/- pros-and-cons-serverless-computing-faas-comparison-aws-lambda-vs-azure-functions-vs-google
- https://blog.iron.io/what-is-the-difference-between-iaas-caas-paas-and-faas/