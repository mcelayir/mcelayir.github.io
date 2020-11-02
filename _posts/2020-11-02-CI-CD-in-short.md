## What Is CI/CD

The term `CI/CD` stands for `Continuous Integration` / `Continuous Delivery` or sometimes `Continuous Deployment`. The idea is to improve the quality of the product and achieve faster delivery of the improvements to the customers.

## How software is delivered

To simplify, the software delivery process can be evaluated under 3 phases following each other.

- Development
- Quality & Assurance
- Operations

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/cicd-in-short/process.png"/>

## Continuous Integration

Developers push their code changes to a repository on a `Version Control System (VCS)`. The compliance of each change for integration is checked on the build server by building the code and running tests. If the checks are successful, the changes are integrated into the main code. In the end, the main branch on the VCS always contains the latest version of the tested code which can be released any time needed.

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/cicd-in-short/ci.png"/>

CI / CD practices require developers to post changes frequently in small chunks. Benefits of this approach are:

- Find and fix bugs earlier
- Constantly adding new features and improvements
- Ensure the code is tested and satisfying quality metrics
- Faster and reliable delivery

## Continuous Delivery

Continuous Delivery aims to deliver the code changes to the users reliably and sustainably. Code changes can be new features, improvements, configuration changes, or bug fixes and the changes needs to be delivered to users as they become available.
After changes are integrated into the main branch, the code is built and the acceptance tests run. If the tests pass, generated artifacts are deployed to an artifact repository where they will be available for deployment.

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/cicd-in-short/cd.png"/>

Advantages are

- Ensuring that the product meets QA standards.
- QA procedures automatized
-  Prevent producement of faulty products

## Continuous Deployment

Continuous deployment is the step of bringing the produced software product together with the users. Generated artifacts are deployed to the servers and become available for their consumers' usage. Automated processes deploy the latest version of the software the application servers as the artifact is generated.

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/cicd-in-short/cdep.png"/>

Advantages are

- Deployments performed at any given time
- Deployments performed at any given time
- Faster time to market
- Reduces complexity and risks of deployments
- Lowers operational costs

## Conclusion

CICD is the set of practices applied to automatize the delivery of the software product to the market. Applying these processes to the delivery process enables large teams to function efficiently and faster delivery of products while improving the quality.

After developers push their changes, automated processes run to validate and assembly the software product and make them available to consumers.

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/cicd-in-short/overall.png"/>

The purpose of applying these practices is to be able to deliver high-quality software in a faster, reliable, and sustainable way. Small changes are frequently integrated into the product and the new versions are frequently delivered to the users.
