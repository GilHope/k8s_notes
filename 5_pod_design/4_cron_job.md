# Cron Jobs

## Overview

CronJobs in Kubernetes are objects that execute a specified job at recurring intervals. 

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