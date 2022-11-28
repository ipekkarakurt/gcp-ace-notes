# TODO

- Cloud Interconnect vs Cloud VPN
- Bigtable: HBase does not allow ad-hoc queries.
- gsutil mb [-c class] [-l location] [-p proj_id] url…
If you don’t specify a -c option, the bucket is created with the default storage class Standard Storage, which is equivalent to Multi-Regional Storage or Regional Storage, depending on whether the bucket was created in a multi-regional location or regional location, respectively
- "There was a problem refreshing your current auth tokens: invalid_grant: Bad Request" => gcloud auth login allows you to obtain new google cloud credentials using your existing email id and password to activate your gcloud sdk
- In order to mount a persistent disk, you need to create a PersistentVolumeClaim after creating a PersistentVolume and then attach the PersistentVolumeClaim to the pod
- Log sinks can be exported to Cloud Storage, Pub/Sub and BigQuery only
- Snapshots are primarily targeting backup and disaster recovery scenarios, they are cheaper, easier to create (can often be uploaded without stopping the VM). They are meant for frequent regular upload, and rare downloads.

Images are primarily meant for boot disk creation. They optimized for multiple downloads of the same data over and over. If the same image downloaded many times, subsequent to the first download the following downloads are going to be very fast (even for large images).
- points of presence
- Alias IP Ranges???
- Routes & firewall rules -> https://www.coursera.org/learn/gcp-infrastructure-foundation/lecture/SD7Bv/routes-and-firewall-rules
- Firewall rule vs firewall policy
