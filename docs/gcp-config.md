## Define Infrastructure Templates and Blueprints  

{!docs/common/config-1-overview.md!}   


| Configuration | Description |  Create Cluster | 
|---|---|---|
| [Networks](#networks) |(Required) Virtual networks provide the networking infrastructure (network, subnet, Internet gateway, and so on) in which your clusters run. Virtual networks on GCP are called VPC networks. You can create new virtual networks or reuse existing virtual networks for your clusters. For basic information VPC networks, refer to [GCP documentation](https://cloud.google.com/compute/docs/vpc/). | You can select the network configuration for your clusters in the **Create Cluster** wizard > **Set up Network and Security** page. If no custom network is selected, default is used. | 
| [Security Groups](#security-groups) | (Required) Security groups include rules which define inbound traffic allowed to the instances in your cluster. You can define different security group configurations for different nodes of your cluster. The security group rules in Cloudbreak correspond with the firewall rules on GCP. To learn more about firewall rules on GCP, refer to [GCP documentation](https://cloud.google.com/compute/docs/vpc/firewalls). | You can select the security group configurations for each host group in the **Create Cluster** wizard > **Choose Blueprint** page. If no custom security groups are selected, default is used. | 
| [VMs and Storage](#vms-and-storage) | (Required) "Templates" define the GCP infrastructure for the instances on which your cluster runs. You can select the VM instance types and their attached storage, including storage type, size, and count. You can reuse the same template for different cluster host groups or create different templates for different host groups.  | You can select infrastructure templates for each host group  in the **Create Cluster** wizard > **Choose Blueprint** page. If no custom templates are selected, default is used. | 
| [Blueprints](#blueprints) | (Required) Blueprints are your declarative definition of a Hadoop cluster, defining the host groups and which components to install on which host group. Ambari uses them as a base for your clusters. | You can select a blueprint for your cluster in the **Create Cluster** wizard > **Choose Blueprint** page. A few default blueprints are available. |  

 

### Networks

You have five options:

* **Use the default network configuration template**: This requires no further action. Every time a cluster is created with this kind of network setup, a new virtual network and a new subnet with the specified IP range will be created for the instances on GCP.
* **Create a new virtual network and a new subnet**: Every time a cluster is created with this kind of network setup, a new virtual network and a new subnet with the specified IP range will be created for the instances on GCP.    
* **Create a new subnet in an existing virtual network**: Use this option if you already have a virtual network on GCP where you'd like to put the cluster but you don't want to have a separate subnet for it.  
* **Use an existing subnet in an existing virtual network**: Use this option if you have an existing virtual network with one or more subnets on GCP and you'd like to start the instances of a cluster in one of these existing subnets.  
* **Use a legacy network without subnets**: Use this option if you have a legacy virtual network on GCP that doesn't have subnet support and you'd like to start instances in that virtual network.

> If creating a new subnet in an existing VPC, make sure that the new subnets that you define don't overlap with already deployed subnets in that VPC. 

> If using an existing subnet, make sure that you have enough room within your network space for the new instances. 


#### Default Network

Cloudbreak includes one pre-defined network configuration called *default-gcp-network*, which is used by default when creating a cluster.  You can see it in the **manage networks** tab. The configuration is: 

|Parameter | Value | 
|---|---|
| Name | default-gcp-network | 
| Description | Default network settings for Gcp clusters. |
| Subnet (CIDR) | 10.0.0.0/16 |

With this default configuration, a new virtual network with a 10.0.0.0/16 subnet will be created every time a cluster is created. No resources will be created until you create a cluster using this configuration.

#### Add New Network and Subnet 

You can define reusable network configurations for your clusters in the **manage networks** tab:

1. Click on **Create a new virtual network and a new subnet**.

2. Provide required parameters:

    | Parameter | Description | 
|---|---|
| Name | Enter a name for your configuration. |
| Description | (Optional) Enter a description. |
| Subnet (CIDR) | Enter a valid [CIDR](http://www.ipaddressguide.com/cidr) for a new subnet that will be created within the VPC. For example `10.0.0.0/24`. |
| Public In Account | (Optional) If this option is checked, all the users belonging to your account will be able to use this network template to create clusters, but they cannot delete it. |
| Select Platform | (Optional) Select a previously created platform. |

3. Click on **+create network**.

    No resources will be created until you create a cluster using this configuration.

#### Add New Subnet in Existing Network

You can define reusable network configurations for your clusters in the **manage networks** tab:

1. Click on **Create a new subnet in an existing virtual network**. 

2. Provide required parameters: 

    > Make sure that the new subnet defined here doesn't overlap with any of your already deployed subnets in the network. 

    |Parameter | Description| 
|---|---|
| Name | Enter a name for your configuration. |
| Description | (Optional) Enter a description. |
| Virtual Network Identifier |  |
| Subnet (CIDR) | Enter a valid [CIDR](http://www.ipaddressguide.com/cidr) for a new subnet that will be created within the VPC. For example `10.0.0.0/24`. |
| Public In Account | (Optional) If this option is checked, all the users belonging to your account will be able to use this network template to create clusters, but they cannot delete it. |
| Select Platform | (Optional) Select a previously created platform. |


3. Click on **+create network**.

    No resources will be created until you create a cluster using this configuration.

#### Use Existing Network and Subnet 

You can define reusable network configurations for your clusters in the **manage networks** tab:

1. Click on **Use an existing subnet in an existing virtual network**. 

2. Provide required parameters: 

    |Parameter | Description| 
|---|---|
| Name | Enter a name for your configuration. |
| Description | (Optional) Enter a description. |
| Virtual Network Identifier | Paste the name of your existing VPC network. You can obtain it from your GCP account by coping the value of the "Nname" property. |
| Subnet name | Paste the name of your existing subnet. You can obtain it from your GCP account.  |
| Don't create public IPs | If you enable this option, Cloudbreak will not create security groups. You must make sure that the created instances in the subnet can reach each other. |
| Don't create new firewall rules | If you enable this option, Cloudbreak will not create firewall rules (equivalent to security group rules). You must make sure (by openining every port in the subnet) that the created instances in the subnet can reach each other. |
| Public In Account | (Optional) If this option is checked, all the users belonging to your account will be able to use this network template to create clusters, but they cannot delete it. |
| Select Platform | (Optional) Select a previously created platform. |

3. Click on **+create network**.

    No resources will be created until you create a cluster using this configuration.

#### Use Legacy Network Without Subnets

You can define reusable network configurations for your clusters in the **manage networks** tab:

1. Click on **Use a legacy network without subnets**. 

2. Provide required parameters: 

    |Parameter | Description| 
|---|---|
| Name | Enter a name for your configuration. |
| Description | (Optional) Enter a description. |
| Virtual Network Identifier | Paste the name of your existing VPC network. You can obtain it from your GCP account by coping the value of the "Nname" property. |
| Public In Account | (Optional) If this option is checked, all the users belonging to your account will be able to use this network template to create clusters, but they cannot delete it. |
| Select Platform | (Optional) Select a previously created platform. |

3. Click on **+create network**.

    No resources will be created until you create a cluster using this configuration.
    
    
### Security Groups

You have two options:

* **Use the default security group configuration**: This requires no further action.  
* **Add your own custom security group configuration**: You can define your own security group by adding the ports, protocols, and CIDR ranges that you'd like to use. The rules defined here don't need to contain the internal rules (these are automatically added by Cloudbreak to the security group on GCP). 

You can define different security group configurations for different nodes of your cluster.


#### Default Security Group

Cloudbreak includes one pre-defined security group configuration called *default-gcp-only-ssh-and-ssl*, which is used by default when creating a cluster. You can see it in the **manage security groups** tab. The configuration is: 

{!docs/common/config-sg.md!} 
   
> Ports used by Hadoop services: Ambari (8080) Consul (8500) NN (50070) RM Web (8088) Scheduler (8030RM) IPC (8050RM) Job history server (19888) HBase master (60000) HBase master web (60010) HBase RS (16020) HBase RS info (60030) Falcon (15000) Storm (8744) Hive metastore (9083) Hive server (10000) Hive server HTTP (10001) Accumulo master (9999) Accumulo Tserver (9997) Atlas (21000) KNOX (8443) Oozie (11000) Spark HS (18080) NM Web (8042) Zeppelin WebSocket (9996) Zeppelin UI (9995) Kibana (3080)  Elasticsearch (9200)



### VMs and Storage  

You have two options:

* **Use the default infrastructure template**: This requires no further action.  
* **Add your own custom infrastructure template**: Use this option if you have specific infrastructure requirements. A typical setup may combine multiple templates in a cluster for the different types of nodes. For example you may want to attach multiple large disks to the data nodes or have memory optimized instances for Spark nodes. 

#### Default Template

Cloudbreak includes one pre-defined infrastructure template called *minviable-gcp*, which is used by default when creating a cluster. You can see it in the **manage templates** tab. The configuration is:


|Parameter | Value |  
|---|---|
| Name | minviable-gcp |
| Instance Type | n1-standard-4 |
| Attached Volumes Per Instance | 1 |
| Volume Size (GB) | 100 |
| Volume Type | Solid-state persistent disks (SSD) |

#### Add Custom Template 

You can define reusable cluster templates in the **manage templates** tab: 

1. Click on **+create template**.

2. Provide required parameters: 

    | Parameter | Description |
|---|---|
| Name | Enter a name for your configuration. |
| Description | (Optional) Enter a description. |
| Instance Type | Select a VM instance type. For more information about instance types on Azure refer to [GCP documentation](https://cloud.google.com/compute/docs/machine-types).|
| Volume Type |  <p>Select the volume type. The options are:<ul><li>Standard persistent disks (HDD)<li><li>Solid-state persistent disks (SSD)<li></ul> For more information about these options refer to <a href="https://cloud.google.com/compute/docs/disks/" target="_blank">GCP documentation</a>. |
| Attached Volumes Per Instance | Enter the number of volumes attached per instance. Default is 1. |
| Volume Size (GB) | Enter the size in GBs for each volume. Default is 100. |
| Public In Account | (Optional) If this option is checked, all the users belonging to your account will be able to use this template to create clusters, but they cannot delete it. |
| Preemptible | If Preemptible is checked then the template will be preemptible. A [preemptible](https://cloud.google.com/compute/docs/instances/preemptible) VM is an instance that you can create and run at a much lower price than normal instances. |
| Public In Account | (Optional) If this option is checked, all the users belonging to your account will be able to use this template to create clusters, but they cannot delete it. |
| Select Platform | (Optional) Select a previously created platform. |

3. Click on **+create template**.

    No resources will be created until you create a cluster using this configuration.


### Blueprints 

{!docs/common/blueprints.md!}  




<div class="next">
<a href="../gcp-create/index.html">Next: Create a Cluster</a>
</div>