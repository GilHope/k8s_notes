# Logging

## Overview

Generally, any pods deployed on K8s write logs in plain text to 'stdout' and 'stderr' streams as opposed to writing logs to a dedicated log file. The underlying container engine does this work.

The 'kubelet' runs on all of your nodes to ensure the containers on the node are healthy and running. However, if the container does not stream its logs to STDOUT and STDERR then you will not get the logs using the 'kubelet logs' command because it will then not have access to them.

## K8s Container Log Format

When exploring a container log file, you will find the following for each log:

1. __Log Level:__ This is the log data that indicates the severity level of the log entry, such as INFO, WARN, ERROR, etc.
2. __Log Message:__ This is the actual description of what is logged.
3. __Metadata:__ This displays the hostname, pod name, IP address, labels, etc.
4. __Time:__ Time stamp.

## Types of K8s Logs

1. __Application Logs:__ Logs from your application. These help determine what is happening from inside the application.
2. __K8s Cluster Components:__ Logs from the api-server, kube-scheduler, etcd, kube-proxy, etc. These help you troubleshoot your cluster.
3. __K8s Audit Logs:__ All logs related to the API activity that is recorded by the API server. Primarily used for investigating suspicious API activity.

## K8s Logging Patterns

1. Node level logging agent
2. Streaming sidecar container
3. Sidecar logging agent

### 1. Node Level Logging Agent

![](https://devopscube.com/wp-content/uploads/2021/11/logging-with-node-agent.png)

This method uses a node-level logging agent (ex- Fluentd) to read a log file created using a container with STDOUT and STDERR streams. It then sends it to a loggin backend (ex- Elasticsearch). 

In managed K8s services like EKS or GKE, the backend would be AWS Cloudwatch or Google Stackdriver.

### 2. Streaming Sidecar Container

![](https://devopscube.com/wp-content/uploads/2021/11/logging-with-streaming-sidecar.png)

This method is useful when your application cannot write logs to STDOUT or STDERR streams directly.

Instead, the application container writes all of its logs to a file within the container. Then the "sidecar container" reads from that log file and streams it to STDOUT and STDERR.

### 3. Sidecar Logging Agent

![](https://devopscube.com/wp-content/uploads/2021/11/logging-with-sidecar-agent.png)

In this method, the logs are not streamed to STDOUT or STDERR. Instead, a sidecar container with a logging agent would run along with the application container. Then the logging agent woul stream the logs to the logging backend.

## K8s Logging Tools

1. __Elasticsearch__ - Log aggregator / search / analysis
2. __Fluentd/Fluentbit__ - Logging agent (Fluentbit is the light-weight agent designed for container workloads)
3. __Kibana__ - Log visualization and dashboarding tool
4. Grafana Loki - Logging for K8s

One of the best open-source logging setups for K8s is the __EFK stack__. This stands for __Elasticsearch, Fluentd, and Kibana__.

For organizations needing enterprise logging solutions to comply with their respective log retention standards, these are some popular tool options:

1. Logz.io
2. Splunk
3. Elastic
4. Sumologic
5. LogDNA