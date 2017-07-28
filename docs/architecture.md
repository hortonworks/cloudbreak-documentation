## Architecture  


### Cloudbreak Deployer Architecture

Cloudbreak deployer includes the following components:

| Component | Description |
|---|---|
| **Cloudbreak Application** | Cloudbreak application is built on the foundation of cloud provider APIs and Apache Ambari. | 
| **Cloudbreak web** | This is Cloudbreak web UI, which can be used to create, manage, and monitor clusters. |
| **Cloudbreak Shell** | This is Cloudbreak's command line tool, which can be used to create, manage, and monitor clusters. | 
| **UAA** | This is Cloudbreak's OAuth identity server implementation, which utilizes UAA. |
| **Sultans** | This is Cloudbreak's user management system. | 
| **Periscope** | This is Cloudbreak's autoscaling application, which is responsible for automatically increasing or decreasing the capacity of the cluster when your pre-defined conditions are met. |


>>>>TO-DO: Is this correct? Are there any other components of Cloudbreak which are not listed here? 

These component names are used in Cloudbreak logs, so for troubleshooting purposes it is useful to know what they refer to.

#### System Level Containers

Cloudbreak deployer utilizes **containerization** - also known as container-based virtualization or application containerization - which is an OS-level virtualization method for deploying and running distributed applications. 

Cloudbreak deployer includes the following system-level containers:

* Consul: Cloudbreak service registry  
* Registrator: automatically registers/unregisters containers with consul 
* Database: Database container for cloudbreak, autoscaling, and uaa
* Traefik: Proxy container 

### Cloudbreak Application Architecture 

The Cloudbreak application is a web application that communicates with the cloud provider account to create cloud resources on your behalf. Once the cloud resources are in place, Cloudbreak uses Apache Ambari to deploy and configure the cluster on cloud VMs. Once your cluster is deployed, you can use Cloudbreak to scale the cluster.

Cloudbreak application is built on the foundation of cloud provider APIs and Apache Ambari:

* Cloudbreak uses **Apache Ambari** to provision, manage, and monitor HDP clusters. 

    Ambari **blueprints** are used as a declarative definition of a cluster. With a blueprint, you can specify stack, component layout, and configurations to materialize an HDP cluster instance via Ambari REST API, without having to use the Ambari cluster install wizard. 
    
* Cloudbreak uses **cloud provider APIs** to create cloud resources required for the HDP clusters. 

    You can define these resources via **templates** for networks, security groups, and VMs and storage in the Cloudbreak web UI. Resources are only provisioned once you create a cluster using the templates.  
    
The use of blueprints and templates is illustrated in the following image:

<a href="../images/templates-and-blueprints2.png" target="_blank" title="click to enlarge"><img src="../images/templates-and-blueprints2.png" width="550" title="How Cloudbreak uses templates and blueprints"></a> 

>>>>TO-DO: Is this correct?  

>>>>TO-DO: What is the role of Salt and how is it related to Cloudbreak application? 

>>>>"Salt manages complex systems at scale. Salt can be used for data-driven orchestration, remote execution for any infrastructure, configuration management for any app stack, and more."


### Cloud Resources Used 

Cloubbreak and clusters deployed by it run in your cloud infrastructure. In general, Cloudbreak uses the following types of cloud resources provisioned on your cloud account:

| Category | Description | AWS | Azure | GCP |
|---|---|---|---|---|
| *VMs and Storage* | Cloudbreak and cluster nodes created by it run on **VMs**. | [Amazon EC2](https://aws.amazon.com/documentation/ec2/) | [Virtual machines](https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-linux-azure-overview?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | [Compute Engine](https://cloud.google.com/compute/docs/) | 
| *Network and Security* | VMs run within a **virtual network** and **subnets**. An **Internet gateway** is used to enable outbound access to the Internet from the Cloudbreak instance and the cluster instances, and a route table is used to connect the subnet to the Internet gateway. **Security groups** control the inbound and outbound traffic to and from the VMs. | [Amazon VPC](https://aws.amazon.com/documentation/vpc/) | [Virtual network](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview) | [Virtual Private Cloud](https://cloud.google.com/compute/docs/vpc/) |
| *Access Control* |  | AWS IAM | Active Directory | IAM |
| *Resource Management* | Cloudbreak uses a designated cloud service to create and manage a collection of related AWS resources. | AWS CloudFormation | Resource Groups | ???? |
| *Cloud Storage* |  | Amazon S3 | ADLS and [WASB](https://docs.microsoft.com/en-us/azure/storage/storage-introduction) | Google Cloud Storage |


>>>>TO-DO: Need to get this reviewed. Is there anything else that I should mention here? 



