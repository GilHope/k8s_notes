# Readiness Probes

## Overview
Readiness probes are a way to manage how traffic is routed to pods within your cluster. They determine whether a pod is ready to handle requests.

The readiness probe is used to signal to K8s when your pods have been successfully started and are ready to process requests.

If a readiness probe is NOT configured, K8s will assume your pods are already ready to receive traffic as soon as they are started, although this can often be not the case especially for applications which require time to warm-up.

There are three types of Readiness Probe options: HTTP, TCP, and Exec.

1. __HTTP GET Probe__
This probe performs an HTTP GET request on the container's IP address on the port and path you specify. K8s will consider you container ready if the probe receives a response with a success status code. This type is particularly useful for apps serving HTTP traffic, as it directly checks the HTTP endpoint's for responsiveness.

2. __TCP Socket Probe__
This probe attempts to establish an TCP connection to your specified port of the container. If the probe can establish this connection, the container is considered ready. This type is useful for apps that listen to TCP ports but do not use HTTP. Things such as databases or application services which communitcate over proprietary protocols are best suited for this probe type.

3. __Exec Probe__
This probe type executes a specified command inside of the container. K8s considers the container ready if the command executes successfully. This is a flexible option which can be used for customized checks. This allows for a much wider arrange of readiness checks beyond simple HTTP or TCP.

## Configuration

Under the 'spec' section of your config file, add a 'readinessProbe' field.

For HTTP GET, use 'httpget', followed by specifying the 'path' and 'port' sections.

Example:
```readinessProbe:
    httpGet:
        path: /api/ready
        port: 8080
```

For TCP, use the 'tcpSocket' option, followed by specifying the 'port'.

Example:
```readinessProbe:
    tcpSocket:
        port: 3306
```

For EXEC, use the 'exec' option, followed by specifying the 'command' section and its subsequent commands in array format.

Example:
```readinessProbe:
    exec:
    - cat
    - /app/is_ready
```

## Additional Considerations

If you know it will take a certain period of time for your application to warm up, you can specify the 'initialDelaySeconds' option.

Example:
```readinessProbe:
    httpGet:
        path: /api/ready
        port: 8080
    initialDelaySeconds: 10
```

If you would like to specify how often to probe, you can do that using the 'periodSeconds' option.

Example:
```readinessProbe:
    httpGet:
        path: /api/ready
        port: 8080
    initialDelaySeconds: 10
    periodSeconds: 5
```

If you would like to specify how many attempts to make, you can use the 'failureThreshold' option.

Example:
```readinessProbe:
    httpGet:
        path: /api/ready
        port: 8080
    initialDelaySeconds: 10
    periodSeconds: 5
    failureThreshold: 8
```