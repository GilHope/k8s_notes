# Jobs

## Overview

In K8s, a Job is a resource type that manages a task or set of tasks that run to completion. This is opposed to long-running services that typically run continuously. 

Jobs are useful for performing a specific operation and then terminate, such as batch processing, data analysis, or batch-oriented workloads. 

Once the task or tasks that a Job manages has been completed, the Job is also considered complete itself.

## Useful Commands

To view a list of your jobs in a given namespace, use this command: 
`kubectl get jobs`

