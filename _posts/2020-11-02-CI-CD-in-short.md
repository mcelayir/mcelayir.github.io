## What Is CI/CD

The term `CI/CD` stands for `Continuous Integration` / `Continuous Delivery` or sometimes `Continuous Deployment`. In short `CICD` is described as a set of practices applied to deliver the improvements produced in a fast, reliable, and sustainable manner to the end-user.

## How software is delivered

To simplify, the software delivery process can be evaluated under 3 phases following each other.

- Development
- Quality & Assurance
- Operations

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/cicd-in-short/process.png"/>

These steps are constantly repeated to maintain the software product or add new features or just bug fixing.

## How CICD Emerged

The delivery process was manual consisting of execting following steps one after each other.

- Developers writes software
- Quality control team tests software
- Operations deploy the software

The most significant shortcomings of the traditional delivery process are
- Slow delivery
- Long feedback cycles
- Producing faulty software
- Process depends on human effort and error-prone
- Complex and risky process

## Main Idea

The main idea is to automatize the QA and deployment processes. CI / CD practices recommend developers to post changes frequently in small chunks and deploy often. QA processes automated and run to validate the code after every change is applied. If the checks pass, changes are integrated and artifacts are generated. These artifacts are rapidly deployed with automated processes for end-users to use.

## Continuous Integration

Developers push their code changes to a repository on a `Version Control System (VCS)`. The compliance of each change for integration is checked on the build server by building the code and running tests. If the checks are successful, the changes are integrated into the main code. 

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/cicd-in-short/ci.png"/>

After changes are integrated into the main branch, the code is built and the acceptance tests run. If the tests pass, generated artifacts are deployed to an artifact repository where they will be available for deployment.

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/cicd-in-short/cd.png"/>

In the end, the main branch on the VCS always contains the latest version of the tested code which can be released any time needed. Moreover, the latest version of the release will be always available in the artifact repository.

- Find and fix bugs earlier
- Constantly adding new features and improvements
- QA procedures automatized
- Ensure the code is always tested and satisfying quality metrics
- Prevent producement of faulty products

## Continuous Delivery

Continuous Delivery aims to deliver the added value to the users reliably and sustainably. Code changes can be new features, improvements, configuration changes, or bug fixes and the changes needs to be delivered to users as they become available.

Generated artifacts are deployed to the servers and become available for their consumers' usage. Automated processes deploy the latest version of the software the application servers as the artifact is generated. 

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/cicd-in-short/cdep.png"/>

Advantages are

- Delivery process is automatized
- Deployments performed at any given time
- Faster time to market
- Reduces complexity and risks of deployments
- Lowers operational costs

## Overall

CICD is the set of practices applied to automatize the delivery of the software product to the market. Applying these processes to the delivery process enables large teams to function efficiently and faster delivery of products while improving the quality.

After developers push their changes, automated processes run to validate and assembly the software product and make them available to consumers.

<img src="https://s3.eu-central-1.amazonaws.com/tutorial.assets/cicd-in-short/overall.png"/>

The purpose of applying these practices is to be able to deliver high-quality software in a faster, reliable, and sustainable way. Small changes are frequently integrated into the product and the new versions are frequently delivered to the users.
