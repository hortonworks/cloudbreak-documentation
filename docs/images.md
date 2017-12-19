## Custom Images

By default, Cloudbreak launches clusters from **default base images**, which include default configuration and default tooling. These images are base images: they include the OS but do not include Ambari or HDP software. The Ambari and HDP software is installed as part of the VM create and cluster provisioning process. 

Default base images are available for each supported cloud provider and region. The following table lists the default base images available: 

[Comment]: <> (For AWS and Azure, per region images are provided.)

| Cloud Provider | Base Image |
|---|---|
| AWS | Amazon Linux 2017 | 
| Azure | CentOS 7 | 
| GCP | CentOS 7 |  
| OpenStack | CentOS 7 | 


Since these standard default images may not fit the requirements of some users (for example when user requirements include custom OS hardening, custom libraries, custom tooling, and so on) Cloudbeak allows you to use your own **custom base images**.

In order to use your own custom base images you must:

1. Build your custom images  
2. Prepare the custom image catalog JSON file and save it in a location accessible to the Cloudbreak VM  
3. Register your custom image catalog with Cloudbreak    
4. Select a custom image when creating a cluster  

<div class="danger">
    <p class="first admonition-title">Important</p>
    <p class="last">
Only base images can be created and registered as custom images. Do not create or register prewarmed images using the instructions provided in this section. 
</p>
</div>



### Build Custom Images

Refer to [Custom Images for Cloudbreak](https://github.com/hortonworks/cloudbreak-images) for information on how to build custom images. 

This repository includes instructions and scripts to help you build custom images. Once you have the images, refer to the documentation below for information on how to create an image catalog and register it with Cloudbreak.


### Prepare the Image Catalog 

Once you've built the custom images, prepare your custom image catalog JSON file. Once your image catalog JSON file is ready, save it in a location accessible via HTTP/HTTPS. 
 

#### Structure of the Image Catalog JSON File 

The image catalog JSON file includes the following two high-level sections: 

* `images`: Contains information about the created images. The burned images are stored in the `base-images` section.  
* `versions`: Contains the `cloudbreak` entry, which includes mapping between Cloudbreak versions and the image identifiers of burned images available for these Cloudbreak versions. 

**Images Section**  

The burned images are stored in the `base-images` sub-section of `images`. The `base-images` section stores one or more image "records". Every image "record" must contain the date, description, images, os, os_type, and uuid fields.

| Parameter | Description |
|---|---|
| date | Date for your image catalog entry. |
| description | Description for your image catalog entry. |
| images | The image sets by cloud provider. An image set must store the virtual machine image IDs by the related region of the provider (AWS, Azure) or contain one default image for all regions (GCP, OpenStack). The virtual machine image IDs come from the result of the image burning process and must be an existing identifier of a virtual machine image on the related provider side. For the providers which use global rather than per-region images, the region should be replaced with **`default`**. |
| os | The operating system used in the image. |
| os_type | The type of operating system which will be used to determine the default Ambari and HDP repositories to use. Set `os_type` to "redhat6" for amazonlinux or centos6 images. Set `os_type` to "redhat7" for centos7 or rhel7 images. |
| uuid | The `uuid` field must be a unique identifier within the file. You can generate it or select it manually. The utility `uuidgen` available from your command line is a convenient way to generate a unique ID. |

**Versions Section**  

The `versions` section includes a single "cloudbreak" entry, which maps the uuids to a specific Cloudbreak version:

| Parameter | Description |
|---|---|
| images | Image `uuid`, same as the one that you specified in the `base-images` section. |
| versions | The Cloudbreak version(s) for which you would like to use the images. |


#### Example Image Catalog JSON File 

Here is an example image catalog JSON file that includes two sets of custom base images: 

* A custom base image for AWS:
    * That is using Amazon Linux operating system 
    * That will use the Redhat 6 repos as default Ambari and HDP repositories during cluster create     
    * Has a unique ID of "44b140a4-bd0b-457d-b174-e988bee3ca47"
    * Is available for Cloudbreak 2.1.0    
*  A custom base image for Azure, Google, and OpenStack: 
    * That is using CentOS 7 operating system 
    * That will use the Redhat 7 repos as default Ambari and HDP repositories during cluster create   
    * Has a unique ID of "f6e778fc-7f17-4535-9021-515351df3692"
    * Is available to Cloudbreak 2.1.0      


You can also download it from [here](https://docs.hortonworks.com/HDPDocuments/Cloudbreak/Cb-doc-resources/custom-image-catalog.json).


<pre><small>
{
  "images": {
    "base-images": [
      {
        "date": "2017-10-13",
        "description": "Cloudbreak official base image",
        "images": {
          "aws": {
            "ap-northeast-1": "ami-78e9311e",
            "ap-northeast-2": "ami-84b613ea",
            "ap-southeast-1": "ami-75226716",
            "ap-southeast-2": "ami-92ce23f0",
            "eu-central-1": "ami-d95be5b6",
            "eu-west-1": "ami-46429e3f",
            "sa-east-1": "ami-86d5abea",
            "us-east-1": "ami-51a2742b",
            "us-west-1": "ami-21ccfe41",
            "us-west-2": "ami-2a1cdc52"
          }
        },
        "os": "amazonlinux",
        "os_type": "redhat6",
        "uuid": "44b140a4-bd0b-457d-b174-e988bee3ca47"
      },
      {
        "date": "2017-10-13",
        "description": "Cloudbreak official base image",
        "images": {
          "azure": {
            "Australia East": "https://hwxaustraliaeast.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Australia South East": "https://hwxaustralisoutheast.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Brazil South": "https://sequenceiqbrazilsouth2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Canada Central": "https://sequenceiqcanadacentral.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Canada East": "https://sequenceiqcanadaeast.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Central India": "https://hwxcentralindia.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Central US": "https://sequenceiqcentralus2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "East Asia": "https://sequenceiqeastasia2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "East US": "https://sequenceiqeastus12.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "East US 2": "https://sequenceiqeastus22.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Japan East": "https://sequenceiqjapaneast2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Japan West": "https://sequenceiqjapanwest2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Korea Central": "https://hwxkoreacentral.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Korea South": "https://hwxkoreasouth.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "North Central US": "https://sequenceiqorthcentralus2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "North Europe": "https://sequenceiqnortheurope2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "South Central US": "https://sequenceiqouthcentralus2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "South India": "https://hwxsouthindia.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Southeast Asia": "https://sequenceiqsoutheastasia2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "UK South": "https://hwxsouthuk.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "UK West": "https://hwxwestuk.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "West Central US": "https://hwxwestcentralus.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "West Europe": "https://sequenceiqwesteurope2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "West India": "https://hwxwestindia.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "West US": "https://sequenceiqwestus2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "West US 2": "https://hwxwestus2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd"
          },
          "gcp": {
            "default": "sequenceiqimage/hdc-hdp--1710161226.tar.gz"
          },
          "openstack": {
            "default": "hdc-hdp--1710161226"
          }
        },
        "os": "centos7",
        "os_type": "redhat7",
        "uuid": "f6e778fc-7f17-4535-9021-515351df3691"
      }
    ]
},
  "versions": {
    "cloudbreak": [
      {
        "images": [
          "44b140a4-bd0b-457d-b174-e988bee3ca47",
          "f6e778fc-7f17-4535-9021-515351df3692"
        ],
        "versions": [
          "2.1.0"
        ]
      }
    ]
  }
}
</small></pre>


### Register Image Catalog 

Now that you have created your image catalog JSON file, register it with your Cloudbreak instance. 

#### Register Image Catalog in the UI

Use these steps to register your custom image catalog in the Cloudbreak UI. 

**Steps**

1. In the Cloudbreak UI, select **Settings** > **Image Catalogs** from the navigation menu.  
2. Click **Create Image Catalog**.
    <a href="../images/cb-images-register.png" target="_blank" title="click to enlarge"><img src="../images/cb-images-register.png" width="650" title="Cloudbreak UI"></a>
  
3. Enter name for your image catalog and the URL to the location where it is stored. 
4. Click **Create**. 

After performing these steps, the image catalog will be available in the create cluster wizard.  

#### Register Image Catalog in the CLI

To register your custom image catalog using the CLI, use the `cb imagecatalog create` command. Refer to [CLI documentation](cli-reference.md#imagecatalog-create). 


### Select a Custom Image When Creating a Cluster

Once you have registered your image catalog, you can use your custom image(s) when creating a cluster. 

#### Select a Custom Image in Cloudbreak UI

Perform these steps in the advanced **General Configuration** section of the create wizard.

**Steps**  

1. Under **Choose Image Catalog**, select your custom image catalog.  
2. Under **Base Images** > **Choose Image**, select the provider-specific image that you would like to use.   
    The "os" that you specified in the image catalog will be displayed in the selection and the content of the "description" will be displayed in green.    
3. You can leave the default entries for the Ambari and HDP repositories, or you can customize to point to specific versions of Ambari and HDP that you want to use for the cluster.  

    <a href="../images/cb-images.png" target="_blank" title="click to enlarge"><img src="../images/cb-images.png" width="650" title="Cloudbreak UI"></a>


#### Select a Custom Image in CLI

To use the custom image when creating a cluster via CLI, perform these steps.  

**Steps**  

1. Obtain the image ID. For example: 

    <pre>cb imagecatalog images aws --imagecatalog custom-catalog</pre>
```json
  [
    {
      "Date": "2017-10-13",
      "Description": "Cloudbreak official base image",
      "Version": "2.5.1.0",
      "ImageID": "44b140a4-bd0b-457d-b174-e988bee3ca47"
    },
    {
      "Date": "2017-11-16",
      "Description": "Official Cloudbreak image",
      "Version": "2.5.1.0",
      "ImageID": "3c7598a4-ebd6-4a02-5638-882f5c7f7add"
    }
  ]
```

2. When preparing a CLI JSON template for your cluster, set the "imageCatalog" parameter to the image catalog name that you would like to use, and set the "imageId" parameter to the uuid of the image from that catalog that you would like to use. For example: 
```json
    ...
    "name": "aszegedi-cli-ci",
    "network": {
      "subnetCIDR": "10.0.0.0/16"
    },
    "orchestrator": {
      "type": "SALT"
    },
    "parameters": {
      "instanceProfileStrategy": "CREATE"
    },
    "region": "eu-west-1",
    "stackAuthentication": {
      "publicKeyId": "seq-master"
    },
    "userDefinedTags": {
      "owner": "aszegedi"
    },
    "imageCatalog": "custom-catalog",
    "imageId": "3c7598a4-ebd6-4a02-5638-882f5c7f7add"
  }
```

**Related Links**  
[CLI Reference](cli-reference.md)  





