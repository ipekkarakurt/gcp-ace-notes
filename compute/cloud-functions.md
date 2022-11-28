## Cloud Functions

event-based, asynchronous compute solution
small, single-purpose functions that respond to cloud events

- only charged for the time that your code runs, billed to the nearest 100 milliseconds
  - provides free tier, many Cloud Functions use cases could be free of charge
- supports functions written in Node.js, Python, Go, Java, .Net Core, Ruby, and PHP.
- triggered by:
  - HTTP triggers: run function in response to HTTP(S) requests
  - event triggers
    - PubSub: whenever a message is published to the specified topic
    - Cloud Storage:  function to be called in response to changes in Cloud Storage
      - [1st gen]/[2nd gen via Eventarc]
      - google.storage.object.finalize/google.cloud.storage.object.v1.finalized: when a new object is created, or an existing object is overwritten and a new generation of that object is created
      - google.storage.object.delete/google.cloud.storage.object.v1.deleted
      - google.storage.object.archive/google.cloud.storage.object.v1.archived
      - google.storage.object.metadataUpdate/google.cloud.storage.object.v1.metadataUpdated
    - Eventarc (only 2nd gen): GCP events, Cloud Audit logs & some 3rd party events
    - Firebase events (only 1st gen)

### When to use Cloud Functions
- microservices architecture
- serverless backend
- intelligent apps (virtual assistant, video/image/sentiment analysis)
