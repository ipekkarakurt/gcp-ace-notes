## Cloud Storage
HA object storage, strong global consistency\
objects are stored in a packaged format which contains the binary form of the actual data + associated meta-data (such as date created, author, resource type, and permissions) +  globally unique identifier (in the form of URLs)\
used for: serving website content, storing data for archival and disaster recovery, and distributing large data objects to end users via Direct Download\
**best for: storing immutable blobs larger than 10 MB\
capacity: petabytes\
max unit size: 5 TB per project**

- not a file system!
  - it is a collection of buckets that you place objects into.
  - directory is just another object that points to different objects in the bucket
  - can't easily index all of these files like you would in a file system. You just have a specific URL to access objects.
- organized into buckets.
  - buckets have globally unique name & geographical location.
- storage objects are immutable (can't edit, a new version is created with each change)
  - If you don’t turn on object versioning, by default new versions will always overwrite older versions.
  - Object versioning enabled => you can list the archived versions of an object, restore an object to an older state, or permanently delete a version of an object.
  -  You are charged for the versions as if there are multiple files
- Roles are inherited from project to bucket to object.
  - If you need finer control, you can create access control lists. ACL = scope (who can access and perform an action, specific user/group of users) + permission (what actions can be performed)
  - **ACL vs IAM**: In most cases, IAM is the recommended method for controlling access to your resources. Use ACLs if you need to customize access to individual objects within a bucket, since IAM permissions apply to all objects within a bucket. However, you should still use IAM for any access that is common to all objects in a bucket, because this reduces the amount of micro-managing you have to do.
  - max 100 ACL entries per bucket.
  - **signed URLs**: even finer control than ACL. provides a cryptographic key that gives time-limited access to a bucket or object.
    - **signed policy document**: determines what kind of file can be uploaded by someone with a signed URL
- Storage class of object is inherited from the bucket
- lifecycle management policies are available.
  - assigned to buckets, applies to all objects in that bucket
- Object change notification can be used to notify an application when an object is updated or added to a bucket through a watch request (webhook notification channel). -> recommended way is PubSub
- no minimum fee, pay only for what you use.
- always encrypts data on the server side, before it’s written to disk, at no additional charge
- Data traveling between a customer’s device and Google is encrypted by default using HTTPS/TLS (Transport Layer Security)
- Where to store your data:
  - Multi region: HA across largest area (data accessed around the world: serving website content, streaming videos, executing interactive workloads, or serving data supporting mobile and gaming applications)
  - Dual-region: High availability and low latency across 2 regions
  - Region: Lowest latency within a single region
- Multi-regional buckets can not be changed back to regional.
- offers directory synchronization: you can sync a VM directory with a bucket.
- .boto file contains credentials & config for boto library for gsutil. encryption and decryption keys are stored here.
- Cost: Data ingress is free.

### Data transfer into Cloud Storage
- using gsutil
- drag and drop option in the Cloud Console
- for larger amounts of data:
  - Storage Transfer Service: schedule and manage batch transfers to Cloud Storage from another cloud provider, from a different Cloud Storage region, or from an HTTP(S) endpoint
  - Offline Media Import: 3rd party provider uploads the data from physical media
  - Transfer Appliance: rackable, high-capacity storage server that you lease from Google Cloud.
    - connect it to your network, load it with data, and then ship it to an upload facility where the data is uploaded to Cloud Storage. You can transfer up to a petabyte of data on a single appliance.

### Storage Classes
#### Standard Storage
best for hot data, stored for brief periods of time\
#### Nearline Storage
best for infrequent access, (**once per month**).\
min storage duration: 30 days\
ex: data backups, long-tail multimedia content, or data archiving\
#### Coldline Storage
best for infrequent access, (**once per year**).\
min storage duration: 90 days\
#### Archive Storage
best for very infrequent access, (**less than once per year**)\
higher costs for data access and operations\
minimum storage duration: 365 days\
ex: data archiving, online backup, and disaster recovery\


gsutil mb gs://<BUCKET_NAME>
gsutil cp [MY_FILE] gs://[BUCKET_NAME]
gsutil acl set private gs://$BUCKET_NAME_1/setup.html
gsutil acl ch -u AllUsers:R gs://$BUCKET_NAME/FILE
gsutil iam ch allUsers:objectViewer gs://$BUCKET_NAME/FILE
gsutil lifecycle get gs://$BUCKET_NAME_1
gsutil lifecycle set life.json gs://$BUCKET_NAME_1
gsutil versioning get gs://$BUCKET_NAME_1
gsutil cp -v setup.html gs://$BUCKET_NAME_1 (Copy with versioning)
gsutil ls -a gs://$BUCKET_NAME_1/setup.html (List all versions of file)
gsutil rsync -r ./firstlevel gs://$BUCKET_NAME_1/firstlevel (syncronize content of bucket & directory)
gsutil ls -r gs://$BUCKET_NAME_1/firstlevel (recursive print)
gsutil rewrite -s [STORAGE_CLASS] gs://[BUCKET_NAME] (Change storage class)
gsutil stat gs://BUCKET_NAME/OBJECT_NAME (list metadata)
