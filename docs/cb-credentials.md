
## Managing Cloudbreak credentials 

You can view and manage Cloudbreak credentials in the **Credentials** tab by clicking **Create credential** and providing required parameters. You must create at least one credential in order to be able to create a cluster. 


### Create Cloudbreak credential  

For steps, refer to:

* [Create Cloudbreak credential on AWS](aws-launch.md#create-cloudbreak-credential)  
* [Create Cloudbreak credential on Azure](azure-launch.md#create-cloudbreak-credential)  
* [Create Cloudbreak credential on GCP](gcp-launch.md#create-cloudbreak-credential) 
* [Create Cloudbreak credential on OpenStack](os-launch.md#create-cloudbreak-credential)


### Modify an existing credential

You can modify an existing Cloudbreak credential by following these steps.

> The value of the "Name" parameter cannot be changed.    
> The values of sensitive parameters will not be displayed and you will have to reenter them.    

**Steps**

1. In the Cloudbreak UI, select **Credentials** from the navigation pane.  
2.Click on <img src="../images/cb_edit.png" alt="On" />  next to the credential that you want to edit.  
3. When done making changes, click **Save** to save your changes.  


### Set a default credential

If using multiple Cloudbreak credentials, you can select one credential and use it as default for creating clusters. This default credential will be pre-selected in the create cluster wizard.
 
**Steps**

1. In the Cloudbreak UI, select **Credentials** from the navigation pane.  
2. Click **Set as default** next to the credential that you would like to set as default.  
3. Click **Yes** to confirm. 




