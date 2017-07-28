## Launch Cloudbreak on OpenStack


### Meet Minimum System Requirements

Before launching Cloudbreak on your OpenStack, make sure that your OpenStack deployment fulfills the following requirements.

#### Supported Linux Distributions 

The following versions of the [Red Hat Distribution of OpenStack](https://www.rdoproject.org/) (RDO) are supported:

* Juno
* Kilo
* Liberty
* Mitaka
 

#### Standard Modules 

Cloudbreak requires that the following standard modules are installed and configured on OpenStack:

* Keystone V2 or Keystone V3  
* Neutron (Self-service and provider networking)  
* Nova (KVM or Xen hypervisor)  
* Glance  
* Cinder (Optional)  
* Heat (Optional but highly recommended, since provisioning through native API calls will be deprecated in the future)  

### Meet the Prerequisites

Before launching Cloudbreak on OpenStack, you must meet the following prerequisites.

#### SSH Key Pair 

Create a new SSH key pair or import an existing SSH key pair. 

#### Security Group  

In order to launch Cloudbreak, you must have an existing security group with the following ports open: 22 (SSH) and 443 (HTTPS). 

For information about OpenStack security groups, refer to the [OpenStack Operations Guide](https://docs.openstack.org/ops-guide/index.html).

### Import Images to OpenStack 

An OpenStack administrator must perform these steps to add the Cloudbreak deployer and HDP images to your OpenStack deployment.

#### Import Cloudbreak Deployer Image 

1. Download the latest Cloudbreak deployer image to your local machine: 

    <pre>curl -O https://public-repo-1.hortonworks.com/HDP/cloudbreak/cloudbreak-deployer-1161-2017-06-15.img</pre>
    
2. Set the following environment variables for the OpenStack image import: 

    <pre>export CBD_LATEST_IMAGE=cloudbreak-deployer-1161-2017-06-15.img
export OS_IMAGE_NAME=cloudbreak-deployer-1161-2017-06-15.img
export OS_USERNAME=your_os_user_name
export OS_AUTH_URL=your_authentication_url
export OS_TENANT_NAME=your_os_tenant_name</pre>


3. Import the new image into your OpenStack:

    <pre>glance image-create --name "$OS_IMAGE_NAME" --file "$CBD_LATEST_IMAGE" --disk-format qcow2 --container-format bare --progress</pre> 

After performing the import, you should be able to see the Cloudbreak deployer image among your other OpenStack images. 

>>>>TO-DO: Some of this content was automatically generated. 
    
    
#### Import HDP Image
    
1. Download the latest HDP image to your local machine: 

    <pre>curl -O https://public-repo-1.hortonworks.com/HDP/cloudbreak/hdc-hdp--1705081316.img
</pre>

2. Set the following environment variables for the OpenStack image import: 

    <pre>export CB_LATEST_IMAGE=hdc-hdp--1705081316.img
export CB_LATEST_IMAGE_NAME=hdc-hdp--1705081316.img
export OS_USERNAME=your_os_user_name
export OS_AUTH_URL=your_authentication_url
export OS_TENANT_NAME=your_os_tenant_name</pre>

3. Import the new image into your OpenStack:

    <pre>glance image-create --name "$CB_LATEST_IMAGE_NAME" --file "$CB_LATEST_IMAGE" --disk-format qcow2 --container-format bare --progress</pre>

After performing the import, you should be able to see the Cloudbreak image among your OpenStack images. 

>>>>TO-DO: Some of this content was automatically generated.  


### Launch the VM  

In your OpenStack, launch and instance providing the following parameters:
 
* Select a VM flavor which meets the following minimum requirements: 4GB RAM, 10GB disk, 2 cores.  
* Select the Cloudbreak deployer image that you imported earlier and launch an instance using that image. 
* Select your SSH key pair.  
* Select the security group which has the following ports open: 22 (SSH) and 443 (HTTPS). 
* Select your preconfigured network.  
    

### SSH to the VM 

Now that your VM is ready, access it via SSH: 
  
* Use a private key matching the public key that you added to your OpenStack project.
* The SSH user is called "cloudbreak".
* You can obtain the VM's IP address from the details of your instance.

### Initialize Your Profile 

After accessing the VM via SSH: 

1. Navigate to the cloudbreak-deployment directory:

    <pre>cd /var/lib/cloudbreak-deployment/</pre>
    
    This directory contains configuration files and the supporting binaries for Cloudbreak deployer.
    
2.  Initialize your profile by creating a new file called `Profile` and adding the following content:

    <pre>export UAA_DEFAULT_SECRET=MY-SECRET
export UAA_DEFAULT_USER_PW=MY-PASSWORD
export PUBLIC_IP=VM-PUBLIC-IP</pre>  

    For example: 

    <pre>export UAA_DEFAULT_SECRET=MySecret123
export UAA_DEFAULT_USER_PW=MySecurePassword123
export PUBLIC_IP=34.212.141.253</pre> 

    > You will need to provide the password when logging in to the Cloudbreak web UI and when using the Cloudbreak Shell. The secret will be used by Cloudbreak for authentication.  


### Perform Optional Configurations

> These configurations are optional. 
 

#### Configuring a Self-Signed Certificate 

If your OpenStack is secured with a self-signed certificate, you need to import that certificate into Cloudbreak, or else Cloudbreak won't be able to communicate with your OpenStack. 

To import the certificate, place the certificate file in the `/certs/trusted/` directory:

1. Navigate to the `certs` directory (automatically generated).
2. Create the `trusted` directory.
3. Copy the certificate to the `trusted` directory. 

Cloudbreak will automatically pick up the certificate and import it into its truststore upon start.


#### Configuring Availability Zone and Region

By default, Cloudbreak uses `RegionOne` region with `nova` availability zone, but you can customize Cloudbreak deployment and enable multiple regions and availability zones by creating an `openstack-zone.json` file in the `etc` directory of Cloudbreak deployment (that is`/var/lib/cloudbreak-deployment/etc/openstack-zone.json`). If the etc directory does not exist in the Cloudbreak deployment directory, then create it. 

The following is an example of `openstack-zone.json` containing two regions and four availability zones:

<pre>{
  "items": [
    {
      "name": "MyRegionOne",
      "zones": [ "az1", "az2", "az3"]
    },
    {
      "name": "MyRegionTwo",
      "zones": [ "myaz"]
    }
  ]
}
</pre>

>>>>TO-DO: Restart of what is needed? restart of cbd
>>>>TO-DO: Can this be done later after cbd start? yes but then you should restart the application if you are using a different configuration


### Launch Cloudbreak Deployer 

1. Start the Cloudbreak application by using the following command:

    <pre>cbd start</pre>
    
    This will start the Docker containers and initialize the application. The first time you start the Coudbreak app, the process will take longer than usual due to the download of all the necessary docker images.
    
    The `cbd start` command includes the `cbd generate` command which applies the following steps:

    * Creates the `docker-compose.yml` file, which describes the configuration of all the Docker containers needed for the Cloudbreak deployment.
    * Creates the `uaa.yml` file, which holds the configuration of the identity server used to authenticate users with Cloudbreak.

    > Once the `cbd start` has finished, it returns the "Uluwatu (Cloudbreak UI) url" which you can later paste in your browser and log in to Cloudbreak web UI.

2. Check Cloudbreak deployer version and health: 
    
    <pre>cbd doctor</pre>
    
3. Next, check Cloudbreak Application logs: 

    <pre>cbd logs cloudbreak</pre>
    
    You should see a message like this in the log: `Started CloudbreakApplication in 36.823 seconds.` Cloudbreak normally takes less than a minute to start.
 

### Access Cloudbreak UI

1. You can log into the Cloudbreak application at `https://IP_Address` where "IP_Address" if the public IP of your OpenStack VM. For example `https://34.212.141.253`.

2. Confirm the security exception to proceed to the Cloudbreak web UI.

    The first time you access Cloudbreak UI, Cloudbreak will automatically generate a self-signed certificate, due to which your browser will warn you about an untrusted connection and will ask you to confirm a security exception.

3. Log in to the Cloudbreak web UI:

    * The default username is `admin@example.com` but you should sign up with your own email address.
    * The password is the value of the `UAA_DEFAULT_USER_PW` variable that you configured in your `Profile` file when [launching Cloudbreak deployer](#launch-cloudbreak-deployer).

    <a href="../images/cloudbreak-ui.png" target="_blank" title="click to enlarge"><img src="../images/cloudbreak-ui.png" width="650" title="Cloudbreak web UI"></a>  


### Create Cloudbreak Credential

Cloudbreak works by connecting your OpenStack account through this credential, and then uses it to create resources on your behalf. Before you can start provisioning cluster using Cloudbreak, you must create a Cloudbreak credential.

1. In the Cloudbreak web UI, open the **manage credentials** pane. 

2. Click **+create credential**. 

3. Provide the following information:

    | Parameter | Description |
|---|---|
| Keystone Version | Select the keystone version. | 
| Name | Enter a name for your credential. |
| Description | (Optional) Enter a description. | 
| User | Enter your OpenStack user name. |
| Password | Enter your OpenStack password. |
| Tenant Name | Enter the OpenStack tenant name. |
| Endpoint | Enter the OpenStack endpoint. |
| API Facing | Select *public* or *private*. | 
| SSH Public Key | Paste your SSH public key. |
| Select Platform | (Optional) Select a platform (if previously configured). |
| Public In Account | (Optional) If you check this, other users added to your Cloudbreak instance will be able to use this credential to create clusters. |
 
4. Click **+create credential**.

5. Your credential should now be displayed at the top of the page and in the **manage credentials** tab.

    Congratulations! You've successfully launched and configured Cloudbreak. Now you can use Cloudbreak to [create clusters](os-create.md). 

<div class="next">
<a href="../os-config/index.html">Next: Define Infrastructure Templates</a>
</div>

