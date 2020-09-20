1- What is serverless

In order to understand what serverless is, we have to understand how applications or apis runs in the cloud.
To be able to deliver the functionality, a server needs to be spinned up and accept connections which requires resources like CPU and RAM should be allocated. In addition to that you have to make sure that your server is running 24/7 to handle the requests and also you have to make sure that the server is behind firewalls with latest security patches applied and only the allowed ones are accessing it. 

Moreover, to you have ensure that the server scales up or down depending on the load. You would like your application to be able to handle the load and serve every user even if the traffic on your application increases enormously. On the other hand you wouldn't want your application to consume resources even if there is no traffic at all. Dedicating resources to a contantly running insance can come up with huge bills from the cloud provider.

To summarize, developing, deploying and maintaining these applications comes up with other challenges which often prevents developers to focus on their jobs and deliver or improve a functionality.

Keeping all above in mind, serverless can be described as an execution model where the cloud provider is responsible to allocate the necessary resources and storage and execute the given piece of code. Cloud provider charge based on resources used to run the code so basically you pay according to the demand.

Of course servers still involve in the serverless, however you don't have to manage any physical server, virtual machines and you dont have to worry about the availability and scalability. Those are taken care by the cloud provider. All you have to do is to provide the piece of code that you want to run in response to an event (http request, finish notification for a file upload etc.) and do some configuration depending on the cloud provider.

2- Why serverless

Benefits of serverless

- Not managing servers reduces the operational complexity 
- Lowers the bills because of on demand usage of resources
- Even if the resourcs used on demand, scalability and availability guaranteed by the cloud provider

For big compaines, it probably doesnt pay to become dependant to a cloud provider and adapt the development and integration to satisfy the cloud providers requirements. However the advantages are priceless for independent developers and prototyping. Instead of paying for dedicated servers to host the application, developers can take advantage of serverless capabilities, get rid of the high cost for maybe halfly utilized servers. Last but not least, with serverless, developers can focus on building the product rather than managing servers and can reduce time to market.

3- Function as a service
4- Stateless functions
5- Aws lambda and use cases
6- Writing/deploying your first lamda function 
