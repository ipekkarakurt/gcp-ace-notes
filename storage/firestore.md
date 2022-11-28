## Firestore

NoSQL cloud database for mobile, web, and server development

**best for: massive scaling, real time query results, offline query support
capacity: terabytes
max unit size: 1MB per entity**

If your schema might change and you need an adaptable database, you need to scale to zero or you want low maintenance overhead scaling up to terabytes consider using Cloud Firestore.

- horizontally scalable
- data is stored in **documents** and then organized into collections
- **NoSQL queries** to retrieve documents in a collection that match your query parameters. Queries can include multiple, chained filters and combine filtering and sorting options. They're also indexed by default, so query performance is proportional to the size of the result set, not the dataset.
- uses data synchronization to update data on any connected device
- **offline data persistence**: caches data that an app is actively using, so the app can write, read, listen to, and query data even if the device is offline. When the device comes back online, Firestore syncs local changes back to Firestore.
- automatic multi-region data replication, strong consistency guarantees atomic batch operations and real **transaction support**
- allows you to run sophisticated queries against your NoSQL data
- A general guideline is to use Cloud Firestore in Datastore mode for new server projects and native mode for new mobile and web apps.
- Free quota per day of: 50,000 document reads, 20,000 document writes 20,000 document deletes, 1 GB of stored data
- Charges begin once the free daily quota has been exceeded. You are charged for:
  - each document read, write, and delete
  - queries: charged at the rate of one “document read” per query (even if query does not return data)
  - the amount of storage your data consumes
  - certain kinds of network bandwidth used to access your data (ingress free, up to 10 GiB of egress free in US)
