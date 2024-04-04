# Jobs

## Overview

In K8s, a Job is a resource type that manages a task or set of tasks that run to completion. This is opposed to long-running services that typically run continuously. 

Jobs are useful for performing a specific operation and then terminate, such as batch processing, data analysis, or batch-oriented workloads. 

Once the task or tasks that a Job manages has been completed, the Job is also considered complete itself.

## Useful Fields

- __'parallelism':__ Specifies the maximum number of pods that can be run simultaneously. This allows a job to run multiple tasks in parallel.

- __'completions':__ Defines the desired number of successfully finished pods. THe job is considered completed when this cumber of completions is reached.

- __'activeDeadlineSeconds':__ Sets the duration in seconds the job is allowed to run. This helps in terminating the job if it runs longer than expected.

- __'backoffLimit':__ Specifies the number of retries for a job before its considered failed. This is useful for jobs that might fial transiently and can succeed upon retry.

- __'template':__ This is a pod template that defines the pod's speculation, including containers, volumes, and settings that should be used for each pod created by the job.

## Useful Commands

To view a list of your jobs in a given namespace, use this command: 
`kubectl get jobs`

