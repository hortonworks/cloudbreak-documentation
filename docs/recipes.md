
## Recipes (Custom Scripts) 

When creating a cluster, you can optionally upload one or more recipes (i.e. scripts) and they will be executed on specific host group before or after the cluster installation. You can use recipes for tasks such as installing additional software or performing advanced cluster configuration. 

### Writing Recipes

When using recipes, consider the following:

* The scripts will be executed on the node types you specify (such as "master", "worker", "compute"). If you want to run a a script on all nodes, define the recipe one per node type.  
* The script will execute on all of the nodes of that type as root.  
* In order to be executed, your script must be in a network location which is accessible from the cloud controller and cluster instances VPC.  
* Make sure to follow Linux best practices when creating your scripts. For example, don't forget to script "Yes" auto-answers where needed.  
* Do not execute yum update –y since it may update other components on the instances (such as salt), which can create unintended or unstable behavior.  

### Adding Node Recipes

To add node recipes:

1. Place your scripts in a network location accessible from Cloudbreak and cluster instances virtual network. 
  
2. Define the recipe when creating a cluster using the Cloudbreak UI or Cloudbreak Shell. You must provide:

    | Parameter | Value |
|---|---|
| Name | Enter a name for your recipe. |
| Description | (Optional) Enter a description for your recipe.|
| Execution Type | Select **PRE** or **POST**, depending on whether you want the script to be executed prior to or post Ambari cluster deployment. |
| Script | <p>Select one of: <ul><li>**Script**: Paste the script.</li><li> **File**: Point to a file on your machine that contains the recipe.</li><li> **URL**: Specify the URL for your recipe.</li></ul> |
| Public In Account | (Optional) If this option is checked, all the users belonging to your account will be able to use this recipe to create clusters, but they cannot delete it. | 
    
3. When creating a cluster, select **Show Advanced Options** > **Choose Blueprint** and specify which recipe you want to execute on which host group. 

### Managing Recipes

You can define reusable cluster recipes (i.e. custom scripts that will be run on selected cluster nodes before or after Ambari cluster deployment) in the **manage recipes** tab by clicking on **+create recipee** and providing required parameters.

You can delete previously defined items using the **delete** option.


### Executing Recipes

The scripts will be executed as root. The recipe output is written to `/var/log/recipes` on each node on which it was executed.
 

### Sample Recipe for Yum Proxy Setting

<pre>#!/bin/bash
cat >> /etc/yum.conf <<ENDOF
proxy=http://10.0.0.133:3128
ENDOF</pre>


>>>>TO-DO: Move Shell commands to the Cb Shell doc. I think the cb shell doc should be part of this documentation
