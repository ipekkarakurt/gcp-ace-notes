## Bigtable

NoSQL wide-column database optimized for **heavy reads and writes**. key-value store. designed as a sparsely populated table.

**capacity: petabytes
max unit size: 10 MB per cell, 100 MB per row
best for: large-scale, low-latency applications as well as throughput-intensive data processing and analytics, analytical data with heavy read/write. adtech, financial, IoT**

if you need to store more than one terabyte of structured data, have very high volumes of writes, need read, write latency of less than ten milliseconds along with strong consistency. Or need a storage service that is compatible with the HBase API, consider using Cloud Bigtable

- Scales well for big amounts of data (billions of rows and thousands of columns)
  - throughput can be adjusted by adding/removing nodes: each node provides up to 10,000 queries per second (read and write)
- does not support SQL queries or multi-row transactions.
- no transactional consistency!
- high availability (99.5% zonal SLA)
- strongly consistent in a single cluster; replication adds eventual consistency across two clusters, and increases SLA to 99.99%.
- Each row column intersection can contain multiple cells or versions at different time stamps providing a record of how the stored data has been altered over time.
- store large amounts of data per row or per item => good for machine learning predictions.
- sharded into blocks of contiguous rows called tablets.
- Cloud Bigtable learns to adjust to specific access patterns. If a certain big table node is frequently accessing a certain subset of data, Cloud Bigtable will update the indexes so that other nodes can distribute that workload evenly.
- ideal data source for MapReduce-style operations
- integrates easily with existing big data tools such as Hadoop, Dataflow, and Dataproc, supports the open-source HBase API standard to easily integrate with the Apache ecosystem.
- Using APIs data can be read from and written to Cloud Bigtable through a data service layer like Managed VMs, the HBase REST server, or a Java server using the HBase client. Typically this is used to serve data to applications, dashboards, and data services.
- Data can also be streamed in through a variety of popular stream processing frameworks like data flow streaming, spark streaming, and storm.
- If streaming is not an option, data can also be read from and written to Cloud Bigtable through batch processes like Hadoop MapReduce, Dataflow, or Spark.
