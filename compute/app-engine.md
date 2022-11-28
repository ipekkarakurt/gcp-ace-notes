##App Engine

fully managed, serverless platform for developing and hosting web applications at scale. automatically provision servers & scale app instances based on demand.

### Standard Environment
- based on container instances running on Google's infrastructure.
  - containers are pre-configured with a runtime from a standardized list of supported languages and versions
- automatic scaling and load balancing
- asynchronous task queues for performing work outside the scope of a request
- scheduled tasks for triggering events at specified times or regular intervals

requirements:
- You must use specified versions of Java, Python, PHP, Go, Node.js, and Ruby
- your application must conform to sandbox constraints that are dependent on runtime.

### Flexible Environment
- specify the container your web application will run in
- your app runs inside Docker containers on Google Cloud's Compute Engine virtual machines. **App Engine manages Compute Engine machines for you.**
- allows you to customize the runtime in the OS of your virtual machine by using Dockerfiles
  - supported runtimes: Python, Java, Go, Node.js, PHP, and Ruby.
    - you can use different versions of these runtimes or provide your own custom runtime by supplying a custom Docker image or using a Dockerfile from the open-source community
- instances are health-checked, healed as necessary, and co-located with other module instances within the project
- updates are automatically applied to the underlying OS
- VM instances are automatically located by geographical region according to the settings in your project. Google's management services ensure that all of our projects' VM instances are co-located for optimal performance
- VM instances are *restarted on a weekly basis*. During restarts, Google's management services will apply necessary OS and security updates.

### Standard Environment vs Flexible Environment
- Standard Environment: seconds startup time, no SSH, can't write to local disk, supports 3rd party binaries for some languages, use App Engine to make calls to the network, after free tier usage you pay per instance class with automatic shutdown
- Flexible Environment: minutes startup time, SSH access, use local disk for scratch space (ephemeral-disk initialized on each startup!), supports 3rd party binaries, allows access to network, pay for resource allocation per hour with no automatic shutdown

gcloud app deploy app.yaml

### When to use App Engine
- focus on building applications instead of deploying and managing the environment.
- use cases for App Engine: websites, mobile apps, gaming backends, and as a way to present a RESTful API to the Internet.
