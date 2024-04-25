# Cron Jobs

## Overview

CronJobs in Kubernetes are objects that execute a specified job at recurring intervals. 

When you think of " ChronJobs ", think of "Chron" as in time, ie Chronos. Job is self explanatory. A Chronjob will perform a job on a time schedule.

CronJobs are the automated equivalent of running a scheduled task on a traditional server, but are managed within the K8s environment.

You might use CronJobs for scheduling things, such as backups or generating reports, that need to run periodically or a specific times.

## Features

- __Scheduled Execution:__ CronJobs use cron expressions to define when jobs should be executed. These expressions allow for precise control over the timing of job executions, such as "at 8:00am every Monday" or "every hour".

- __Job Management:__ For each scheduled time that a CronJob triggers, it creates a JOb object to perform the actual work. The CronJob manages these Jobs, ensuring that they are executed according to the schedule and configuration.

- __Failure Handling and Retry Policies:__ CronJobs allow you to specify how to hadnle job failures and retries. You can configure policies that determine whether and when a failed job should be retried.

- __Concurrency Policy:__ You can specify how the CronJob handles concurrent executions. For example, you can prevent a new job from starting if the previous one is still running, or you can allow jobs to run concurrently.

- __History Limit:__ You can control how many completed and fialed jobs are kept. This allows you to manage the accumulation of old job instances and their assocaited pods, helping to prevent clutter and resource overuse.

## Cron Expression Format

CronJobs use something called 'cron syntax' to specify its schedule. A cron expression represents a set of times and is made up of five fields, representing minute, hour, day of month, month, and day of the week.

Example:

'0 * * * *': would run the job every hour on the hour.

'30 4 * * *': would run the job at 4:30 AM every day.

## Useful Fields

- __'schedule':__ A __required field__ that specifies when the job should start in Cron format, such as "* * * * *" (representing minute, hour, day of the month, month, day of the week).

- __'startingDeadlineSeconds':__ An optional field that sets a deadline for starting the job if it misses its scheduled time for any reason. If the job misses its scheduled time by more than this period, it is not started.

- __'concurrencyPolicy':__ Defines how to treat concurrent executions of a job. Options include 'Allow' (default), 'Forbid' (prevents concurrent runs), and 'Replace' (cancelling current jobs and replacing with new).

- __'suspend':__ An optional boolean field that allows you to control whether subsequent executions are suspended. This dones not apply o already started executions.

- __'successfulJobsHistoryLimit' and 'failedJobsHistoryLimit':__ Specifies the number of successful and failed finished jobs to retain, respectively. This helps in keeping a history of completed and failed jobs for review while controlling the amount of stored job data.

- __'jobTemplate':__ This is where you define the job specification just like you would for a standalone job. It includes the template for creating pods, along with any relevant settings like 'backoffLimit', 'completions', and 'parallelism'.