## Cloud Run
serverless managed compute platform that lets you run **stateless** containers via web requests or Pub/Sub events

- built on Knative
- can automatically **scale up and down from/to zero** almost instantaneously
- billing: you are charged only for the resources you use, calculated down to the nearest 100 milliseconds.
  - don't pay for anything if your container doesn't handle requests.
  - a container with more vCPU and memory is more expensive.
- can allocate up to 4 vCPUs and 32 GiBs of memory.
- By default, your application is exposed on a unique sub domain of the globalrun.appdomain. You can also use your own custom domain.
- **Cloud Run can only deploy images that are stored in Artifact Registry.**

- building containers for Cloud Run:
  - source-based workflow (using buildpacks):
    - deploy your source code instead of a container image
    - Cloud Run builds your source and packages the application into a container image for you
    - supported platforms: Go, Node.js, Python, Java, .NET Core
  - container-based workflow (using a Dockerfile):
    - build using Cloud Build
    - build locally using Docker & push to Container Registry

### When to use Cloud Run
- deploy stateless containers that listen for requests or events delivered via HTTP requests.
- build your applications in any language using whatever frameworks and tools you wish and deploy them in seconds without having to manage and maintain that server infrastructure.
