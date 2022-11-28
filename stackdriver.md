## Monitoring

- metrics scope: the root entity that holds monitoring and configuration information in Cloud Monitoring.
  - each metrics scope can have 1-100 monitored projects.
  - you can have as many metrics scopes as you want, but Google Cloud projects and AWS accounts can't be monitored by more than one metrics scope.
  - metrics scope contains the custom dashboards, alerting policies, uptime checks, notification channels, and group definitions that you use with your monitored projects.
  - In order to give people different roles per project and to control visibility to data, consider placing the monitoring of those projects in separate metrics scopes.
- charts can be customized with filters to remove noise, groups to reduce the number of time series, and aggregates to group multiple time series together.
- alerting policies notify you when specific conditions are met.
  - ex: the network egress of your VM instance goes above a certain threshold for a specific timeframe
  - notified through email, SMS, or other channels
- Monitoring Agent: access additional system resources and application services
  - needs to be installed separately, via curl download & bash commands => include in startup script
- possible to create custom metrics

- Cloud Logging data can be exported as files to Cloud Storage, as messages through Pub/Sub, or into BigQuery tables. Data access logs are retained by default for 30 days, but this is configurable up to a maximum of 3,650 days. Admin logs are stored by default for 400 days. Logs can be exported to Cloud Storage or BigQuery to extend retention.
  - it's a best practice to install the logging agent on all of your VM instances => include in startup script
- Stackdriver Error Reporting counts, analyses, and aggregates the errors in your running Cloud services
  - generally available for the App Engine standard environment, and is a Beta feature for App Engine flexible environment, Compute Engine, and AWS EC2
- Cloud Trace: tracing system that collects latency data from your distributed applications and displays it in the Google Cloud console.
  - captures traces from App Engine, HTTPS load balancers, and applications instrumented with the Stackdriver Trace API
- Cloud Profiler presents the call hierarchy and resource consumption of the relevant function in an interactive flame graph
- Stackdriver Debugger is a feature of GCP, that lets you inspect the state of a running application in real time without stopping or slowing it
