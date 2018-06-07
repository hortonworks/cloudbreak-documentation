## Configuring access to Google Cloud Storage (GCS)

Google Cloud Storage (GCS) is not supported as a default file system, but access to data in GCS is possible via the gs connector. Use these steps to configure access from your cluster to GCS. 

These steps assume that you are using an HDP version that supports the gs cloud storage connector (HDP 2.6.5 introduced this feature as a TP).

### Prerequisites

1. Access to Google Cloud Storage is via a service account. If you already started using Cloudbreak, you must already have a service account. 

2. The service account that you provide to Cloudbreak must have access to the buckets that you want to use with Cloudbreak-managed clusters. For example instructions on how to configure the access refer to [Modify GCS Bucket Permissions](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.5/bk_cloud-data-access/content/edit-bucket-permissions.html). 

### Configure access to GCS 

Access to Google Cloud Storage is via a service account. 


**Steps**

1. On the **Cloud Storage** page in the advanced cluster wizard view, select **Use existing GSC storage**. 

2. Under **Service Account Email Address** provide the email for your GCS storage account.  

Once your cluster is in the running state, you should be able to access buckets that the configured storage account has access to.  


### Testing access to GCS 

Test access to the Google Cloud Storage bucket by running a few commands from any cluster node. For example, you can use the command listed below (replace “mytestbucket” with the name of your bucket):

<pre>hadoop fs -ls gs://my-test-bucket/</pre>


### Configure GCS storage locations 

After configuring access to GCS via service account, you can optionally use a GCS bucket as a base storage location; this storage location is mainly for the Hive Warehouse Directory (used for storing the table data for managed tables).   

**Prerequisites**

* You must have an existing bucket. For instructions on how to create a bucket on GCS, refer to [GCP documentation](https://cloud.google.com/storage/docs/creating-buckets).  
* The service account that you configured under [Configure access to GCS](#configure-access-to-gcs) must allow access to the bucket.   

**Steps**

1. When creating a cluster, on the **Cloud Storage** page in the advanced cluster wizard view, select **Use existing GCS storage** and select the existing service account, as described in [Configure access to GCS](#configure-access-to-gcs).    
2. Under **Storage Locations**, enable **Configure Storage Locations** by clicking the <img src="../images/cb_toggle.png" alt="On"/> button.   
3. Provide your existing bucket name under **Base Storage Location**.  
4. Under **Path for Hive Warehouse Directory property (hive.metastore.warehouse.dir)**, Cloudbreak automatically suggests a location within the bucket. For example, if the bucket that you specified is `my-test-bucket` then the suggested location will be `my-test-bucket/apps/hive/warehouse`.  You may optionally update this path.        
