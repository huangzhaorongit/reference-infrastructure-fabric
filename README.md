# Reference Infrastructure Fabric

Bootstrapping a microservices system is often a very difficult process for many small teams because there is a diverse ecosystem of tools that span a number of technical disciplines from operations to application development. This repository is intended for a single developer on a small team that meets all of the following criteria:

1. Building a simple modern web application using a service-oriented or microservices approach.

2. Using Amazon Web Services ("AWS") because of its best-in-class commodity "run-and-forget" infrastructure such as RDS Aurora, PostgreSQL, Elasticsearch or Redis.

3. Limited operations experience or budget and wants to "get going quickly" but with a reasonably architected foundation that will not cause major headaches two weeks down the road because the foundation "was just a toy".

If all the above criteria match then this project is for you and you should keep reading!

## What do you mean by "simple modern web application"?

### Simple

The concept of simplicity is a subjective, but for the purpose of this architecture "simple" means that the application conforms to two constraints:

1. Business logic, for example, a REST API is containerized and run on the Kubernetes cluster.
2. Persistence is offloaded into an external system.

### Modern

Similarly the term "modern" is ambiguous, but for the purpose of this architecture "modern" means that the application has a very narrow downtime constraint. We will be targetting an application that is designed for at least "four nines" of availability. Practically speaking, this means the app can be updated or modified without downtime.

## Technical Design in Five Minutes

**NOTE**: More advanced documentation is available in [docs/](docs/).

To keep this infrastructure fabric simple, but also robust we are going to make some opinionated design decisions. The following things will be performed on AWS:

1. A single new Virtual Private Cloud ("VPC") will be created in a single region (us-east-2 "Ohio") that holds the Kubernetes cluster along with all long-lived systems (e.g. databases). A VPC is a namespace for networking. It provides strong network-level isolation from other "stuff" running in an AWS account. It's a good idea to create a separate VPC rather than relying on the default AWS VPC because over time the default VPC becomes cluttered and hard to maintain or keep configured properly with other systems and VPC's are a cost-free abstraction in AWS.

2. The VPC will be segmented into several subnets that are assigned to at least three availability zones ("AZ") within the region. An availability zone in AWS is a physically isolated datacenter within a region that has high-performance networking links with the other AZ's in the *same* region. The individual subnets will be used to ensure that both the Kubernetes cluster as well as any other systems such as an RDS database can be run simultaneously in at least two availability zones to ensure there is some robustness in the infrastructure fabric in case one zone fails.

3. A Kubernetes cluster will be installed into the newly created VPC and setup with a single master node and several worker nodes. Despite the single master node this is still a HA setup because the worker nodes which actually run containers will be spread across several availability zones. In the rare, but potential situation where the master node fails the Kubernetes system will continue to be available until the master is automatically replaced (the mechanics of this are documented in [/docs/high_availability.md](docs/high_availability.md).

### Diagram

Here's a pretty graphical diagram of all the above information...

## Getting Started

### Prerequisites

1. You need an active AWS account and a AWS API credentials. Please read our five-minute "Bootstrapping AWS" guide if you do not have this yet.

2. Install Hashicorp's [Terraform](https://terraform.io).

3. Install Kubernetes Ops ("kops") tool.

### Sanity Checking

Run the script `bin/sanity` to check that everything is OK for deployment.

### Clone Repository

Clone this repository into your own account or organization.

### Configure a couple of things

### Generate the AWS networking environment

### Verify the AWS networking environment

### Generate the Kubernetes cluster configuration

### Generate the Kubernetes cluster

## Next Steps

Check out Datawire's Reference Application - Snackchat! 

## License

Project is open-source software licensed under **Apache 2.0**. Please see [LICENSE](LICENSE) for more information.