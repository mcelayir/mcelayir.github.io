# Cloud Computing & Serverless

## Introduction

Initially the term serverless were used to describe applications which the functionaly is covered by third party services and/or custom code running in containers which is maintained by the service provider. The key here is not to manage any servers but to pay for the service or functionality that you use. This approach removes the complexity of running and maintaining an infrastructe as well as removing the costs. 

With the rise of the technology provided by cloud computing companies like Amazon and Google, the term serverless is also used to describe a wide range of product provided by these companies.

In this article I would like to give information about cloud computing and computing models and then talk about serverless applications and possible use cases.

## Cloud Computing

### Evolution of techology

Lets go back to the early days (90s if possible) and meet our friend Peter who owns a bookstore and wants to grow his business by selling books online (visionary guy). 

Peter hires some developers and ask them to deliver the his online bookstore. When the application delivered, Peter realizes that the has to put the application to somewhere so that people from his town can access to his website and start ordering books. After some investigation, Peter understands that he has to buy some sort of special computer called `server` so he decides to buy one of this computer.

He goes back to his store with his new special computer and starts installing it. In a couple of hours his excitement turns into anger because simply doesnt understand how all of this works and he is not capable of running this by himself. With despair Peter hires a guy to help him with his server. After one week the server guy (the ancestor of IT department) puts everything together and make the bookstore accessible from the internet. It was the first time that Peter saw his investment and he immediately start testing it. The testing and bug fixing took sime time but somehow this phase is passed and now finally Peter thinks that he can start advertising this online bookstore and start making money out of it.
Next day when Peter goes back to the store he saw his server guy helplessly sitting in front of the `out of service` server. After his advertisement campaign everybody in the town tried to visit his web page and the single server couldnt been able to handle the load.

Long story short Peter bought a couple of servers and he rented another place to contain them (at those days no one knew the word data center). He hired more server guys to maintain his servers. He paid for extra hardware and software to keep his servers safe and healthy.
He was able to serve more customers but the amount that he has to pay to keep his business alive also skyrocketed.

Also Peter realized that, most of the daytime his online store doesnt have too many customers and his servers are idle. He was basicly paying to be able handle high traffic, not the traffic that his servers are managing but he had no other option.

In these years, some other guys invested in servers and built bigger versions of what Peter has which called data centers. With the emerging technology called `virtualisation` they were able to run thousands of servers in one of those computers for cheaper prices and even better you also dont have to upkeep them by yourself. Professionals trained for this purpose will maintain these servers and they guarantee that your servers will be available all the time. Obviosly this was the best choice for most of the business owners to pursue their digital affairs so they start carrying their applications to this provider companies and started to rent the servers that are far away from them.

### Cloud Computing Models

Up to this now it was a bit slow and painful (ask Peter about it) but I promise it will get faster and exciting. Web hosting companies failed to meet their growing computing needs and running IT infrastructure on-premises was getting costly and complex every day. This paved the way for cloud companies to take over and start providing solutions for vastly varied customer groups having different requirements and needs. These solutions can be considered under the four main types of cloud computing.

#### Infrastructure as a Service (IaaS)

Emerging cloud providers start providing computing infrastructure as a service which allows IT professionals to create virtual machines with desired computing power with a single click and terminate it with another click. Instead of building and maintaining an on-premise IT structure which is costly and requires labor, people started to buy this infrastructe as a service. Iaas solutions bring flexibility and scalability and provided more control over the infrastructure whereas taking away the huge cost of owning an on-premise it infrastructure.

Eventhough it brings huge computing power to peoples fingertips by abstracting all the infrastructure, it has its downsides. First of all the created VMs has to be managed and maintained. Also all of the architectural components needs to be put together, for example if you need load balancing, you have to set up all vm instances for your application, but an instance for load balancing and do all the configurations. Same applies for other concerns like storage or networking. IaaS offers the building blocks but someone has to build the system by using these building blocks. In other terms it requires a lot of configuration and attention of some IT professional.

Right now all the cloud providers have a IaaS solution. Some popular examples are

- AWS EC2
- Google Compute Engine
- Microsoft Azure

Its also worth mentioning the costs for IaaS. It is obvious that it makes much more computing power avaliable for more cheaper costs. However in IaaS solutions you basically rent computing power so you billed based the computing power that you use. Eventhough cloud providers provides scaling options which can lower the costs you probably would run some instances for your applications availability that means using computing power.

To conclude, IaaS is the closest thing to having a data center. Its like having a data center affiliated by cloud provider. Moreover you can spread your data centers across the different parts of the globe but it comes with a price.

### Platform as a Service (PaaS)

Platform as a Service is the cloud computing model where the cloud provider provides the hardware alongside with the software and tools needed to run the application. PaaS solutions can be think as an IaaS solution sophisticated to run your application. 
It is mostly used by developers building software or applications. It simply abstracts away the complexity of building an managing an infrastructure so that developers can focus build their apps and run it as they are running on their local machine. PaaS solutions offer tools to deploy your application from docker or github which can be further automatised by using third party tools.

Some populer examples of PaaS are
- AWS Elastic Beanstalk
- Heroku

Pricing model for PaaS is same as IaaS, you pay per-use basis and pay for the amount of resources that you consume. However again for your application to be accessible at least a deamon should run somewhere to handle the requests received which means you have to reserve some capacity and pay for it. However it is much cheaper than the IaaS and you can find reasonable prices between 5-10$ per month to host your application.

### Function as a Service (FaaS)

Function as a Service (FaaS) aka `the serverless` can be described as an execution model where the cloud provider is responsible to allocate the necessary resources and storage and execute the given piece of code in response to events triggered. Events can an http call to an endpoint or a notification indication file upload completed.  

Cloud provider charge based on number of times that the function is executed. For example AWS Lambda’s pricing is set on $0.20 per million requests and $0.00001667 per GB-s (Gigabyte per second) so you get 1 million requests and 400.000 GB-s per month for free.

Some popular examples are
- AWS Lambda
- Google Functions
- Azure Functions

Of course servers still involve in the serverless, however you don't have to manage any physical server, virtual machines and you dont have to worry about the availability and scalability. Those are taken care by the cloud provider. All you have to do is to provide the piece of code that you want to run in response to an event (http request, finish notification for a file upload etc.) and do some configuration depending on the cloud provider. Its safe to say `serverless` doesnt mean `no servers` but it means `no servers that you have to manage`

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/serverless/IaaS-PaaS-FaaS+Diagram_ASSIST_Software.png"/>
To concude, each computing model comes with a trade off. Depending on the computing model that you choose you increase or decrease your control over the platform that you run your application. This will affect the design of the application, how you program and how you deploy. Basically choosing the right product according to your needs and budget is maybe the first decision that you make since everything will be shaped according to that.

## Serverless 

The term `Serverless` is indeed confusing since the example applications are generally referred as `client-server` applications where a client calls a server for data or posts data for some purpose. In this case the refers to server as `hardware` so it stands for `no hardware to manage`. Still there will be a server process running on some hardware but the owner of the serverless application doesn't have to manage this hardware. The availability of the server process is guaranteed by the vendor.

Purpose of the serverless is to keep developers away from the hustle of creating and maintaining an infrastructure for their applications and let them focus on creating value for their application.

We come across two terms while talking about this concept.

### Backend as a Service (BaaS)

BaaS is used to describe the third party applications and/or services consumed by the application to deliver a functionality. These services are not developed or maintained by the organization. The application talks to these services over the provided protocols. Its also worths mentioning that a Software as a Service (SaaS) solution can be considered as a BaaS if it serves for this purpose

### FaaS

FaaS is a way of developing serverless applications where application is designed as seperate functions executed independently to achieve a task. Functions will run in stateless, ephemeral containers in response to a triggering event. The computing resources necessary to execute the function is in vendors responsibility.

## When Serverless is not recommended
Alongside with all the amazing features that serverless has, it has its downsides.

To begin with, it is not ideal Long running applications better use dedicated resources for them as the resources for the function can cost more

Secondly developers needs to be aware of the cold starts. When a function is invoked, the provider will spin up a container and execute the function. After the output the container will kept alive for certain amount of time and then will be terminated. If another requests comes in before the termination, the same container will be used. However after termination, another container will be created to handle the incoming request. Creating another container from scratch means that functions cannot trust the local storage to load a value needed for the calculation. This can lead to redesing of the application architecture. Also this will introduce a delay in the response time.

Lastly depending on cloud provider there will be a overhead of invoking functions. Services could only be access by the defined interfaces of the provider which can limit development and deployment options. Becoming over adapted to a vendor will make things harder incase of a need to change.

## Conclusion

### XaaS

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/serverless/container-orchestration-wars-2017-edition-21-638.jpg" alt="https://techthought.org/wp-content/uploads/sites/2/2018/07/container-orchestration-wars-2017-edition-21-638.jpg" />

<b>IaaS:</b> Only the base infrastructure. Eveything needs to be configured by the end-user.

<b>PaaS:</b> Platform to deploy, run and manage applications without dealing with the infrasturcture

<b>FaaS:</b> Platform to deploy, run and functions in response to events without dealing with any sort of hardware

### Benefits of serverless

Benefits of serverless

- Not managing servers reduces the operational complexity 
- Cost efficient because of on demand usage of resources
- Scalability and availability guaranteed by the cloud provider
- Reduces the complexity of writing and maintaining software
- Faster time to market

### Downsides of Serverless

- Becoming vendor dependant. 
- Losing control over platform and infrastructure
- Not efficient for stateful applications and long running tasks
- Cold-starts: means that platform needs to 
- Brings overhead of adapting the microservices architecture if the organization is not ready


REFS
https://www.sumologic.com/glossary/function-as-a-service/
https://www.cloudflare.com/learning/serverless/glossary/function-as-a-service-faas/
https://www.scalyr.com/blog/function-as-a-service-faas/
https://martinfowler.com/articles/serverless.html
https://hackernoon.com/what-is-serverless-architecture-what-are-its-pros-and-cons-cc4b804022e9
https://calculator.aws/#/createCalculator
https://assist-software.net/blog/pros-and-cons-serverless-computing-faas-comparison-aws-lambda-vs-azure-functions-vs-google
https://blog.iron.io/what-is-the-difference-between-iaas-caas-paas-and-faas/