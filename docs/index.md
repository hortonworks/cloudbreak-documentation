## Introduction

Welcome to the **Cloudbreak 2.6 Technical Preview** documentation! 

Cloudbreak simplifies the provisioning, management, and monitoring of on-demand HDP and HDF clusters in virtual and cloud environments. It leverages cloud infrastructure to create host instances, and uses Apache Ambari via Ambari blueprints to provision and manage HDP clusters. 

Cloudbreak allows you to create clusters using the Cloudbreak web UI, Cloudbreak CLI, and Cloudbreak REST API. Clusters can be launched on public cloud infrastructure platforms **Microsoft Azure**, **Amazon Web Services (AWS)**, and **Google Cloud Platform (GCP)**, and on the private cloud infrastructure platform **OpenStack**.

<a href="./images/cb_cb-ui3.png" target="_blank" title="click to enlarge"><img src="./images/cb_cb-ui3.png" width="650" title="Cloudbreak web UI"></a>   


### Primary use cases

Cloudbreak allows you to create, manage, and monitor your HDP and HDF clusters on your chosen cloud platform:

* Dynamically deploy, configure, and manage clusters on public and private clouds (AWS, Azure, Google Cloud, OpenStack).   
* Use automated scaling to seamlessly manage elasticity requirements as cluster workloads change.  
* Secure your cluster by enabling Kerberos.   

### Default cluster configurations

Cloudbreak includes default cluster configurations (in the form of blueprints) and supports using your own custom cluster configurations (in the form of custom blueprints).

The following default cluster configurations are available:

 

Platform version: **HDP 2.6**

| Cluster type  | Main services | Description |  List of all services included |
|---|---|---|---|
| Data Science | <i class="fa fa-check" style="color: green"></i> Spark 2,<br>Zeppelin | Useful for data science with Spark 2 and Zeppelin. | HDFS, YARN, MapReduce2, Tez, Hive, Pig, Sqoop, ZooKeeper, Ambari Metrics, Spark 2, Zeppelin |
| EDW - Analytics | <span><i class="fa fa-check" style="color: green"></i> Hive 2 LLAP</span>,<br>Zeppelin | Useful for EDW analytics using Hive LLAP. | HDFS, YARN, MapReduce2, Tez, Hive 2 LLAP, Druid, Pig, ZooKeeper, Ambari Metrics, Spark 2 | 
| EDW - ETL | <i class="fa fa-check" style="color: green"></i> Hive,<br> Spark 2 | Useful for ETL data processing with Hive and Spark 2. | HDFS, YARN, MapReduce2, Tez, Hive, Pig, ZooKeeper, Ambari Metrics, Spark 2 |


Platform version: **HDF 3.1**

| Cluster type  | Main services | Description |  List of all services included |
|---|---|---|---|
| Flow Management | <i class="fa fa-check" style="color: green"></i> NiFi | Useful for flow management with NiFi. | NiFi, NiFi Registry, ZooKeeper, Ambari Metrics |
| Messaging Management | <i class="fa fa-check" style="color: green"></i> Kafka |  Useful for messaging management with Kafka. | Kafka, ZooKeeper, Ambari Metrics |


### Core concepts   

Refer to [Architecture](architecture.md) and [Core concepts](architecture.md#core-concepts).


### Get started

To get started with Cloudbreak:

1. Select the [cloud platform](#select-a-cloud-platform) on which you would like to launch Cloudbreak.   
1. Select the [deployment option](#select-a-deployment-option) that you would like to use. 
1. [Launch Cloudbreak](#launch-cloudbreak). 


#### Select a cloud platform 

You can deploy and use Cloudbreak on the following cloud platforms:

* Amazon Web Services (AWS)
* Microsoft Azure
* Google Cloud Platform (GCP)
* OpenStack


#### Select a deployment option

There are two basic deployment options:

| Deployment option | When to use |
|---|---|
| Option 1: Instantiate Cloudbreak using one of the provided pre-built cloud images | <p>This is the basic deployment option and the easiest to get started with.</p><p> The cloud images include Cloudbreak deployer pre-installed on a CentOS VM.</p>  |
| Option 2: Install the Cloudbreak deployer on your own VM | <p>This is an advanced deployment option.</p> <p>Select this option if you have custom VM requirements. The supported operating systems are RHEL, CentOS, and Oracle Linux 7 (64-bit).</p> |


#### Launch Cloudbreak 

**(Option 1)** You can launch Cloudbreak from one of the pre-built images:  

* [Launch on AWS](aws-launch.md)  
* [Launch on Azure](azure-launch.md)  
* [Launch on GCP](gcp-launch.md)   
* [Launch on OpenStack](os-launch.md)    
     
**(Option 2)** Or you can launch Cloudbreak [on your own VM](vm-launch.md) on one of these cloud platforms. This is an advanced deployment option that you should only use if you have custom VM requirements. 

In general, the steps include meeting the prerequisites, launching Cloudbreak on a VM, and creating the Cloudbreak credential. After performing these steps, you can create a cluster based on one of the default blueprints or upload your own blueprint and then create a cluster. 


<div class="note">
    <p class="first admonition-title">Note</p>
    <p class="last">The Cloudbreak software runs in your cloud environment. You are responsible for cloud infrastructure related charges while running Cloudbreak and the clusters being managed by Cloudbreak.</p>
</div>



