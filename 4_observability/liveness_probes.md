# Liveness Probes

## Overview

Liveness probes work to determine if a container is running properly and when to restart a container if necessary. If a liveness probe fails, then the kubelet kills the container, which will then be restarted based on the pod's 'restartPolicy'.

Like 'readiness probes', liveness probes have three types: HTTP Get, TCP Socket, and Exec Command.

1. __HTTP Get Probe__
This probe performs an HTTP GET request to the container's IP address on the port and path you specify. K8s considers the container alive and well if the HTTP response code responds with success. This type of probe is useful for apps serving HTTP traffic and is ideal for web servers or RESTful APIs where specific endpoints can be designated to respond with the current health status of your application.

2. __TCP Probe__
This probe attempts to establish a TCP connection to the specified port of the container. The container is considered healthy by K8s if the probe successfully established a connection. Best suited for apps that might not be HTTP-based but still listen on a TCP port, such a databases.

3.__Exec Probe__
This probe executes a specified command inside your container. K8s considers the container healthy if the command runs successfully. This type of probe is ideal for when you need to perform more complicated or specific health checks which HTTP or TCP cannot easily cover.

## Configuration

Under the 'spec' section of your config file, add a 'livenessProbe' field.

Example:
```livenessProbe:
    httpGet:
        path: /api/healthy
        port: 8080
```

For TCP, use the 'tcpSocket' option, followed by specifying the 'port'.

Example:
```livenessProbe:
    tcpSocket:
        port: 3306
```

For EXEC, use the 'exec' option, followed by specifying the 'command' section and its subsequent commands in array format.

Example:
```livenessProbe:
    exec:
    - cat
    - /app/is_healthy
```

## Additional Considerations

If you know it will take a certain period of time for your application to warm up, you can specify the 'initialDelaySeconds' option.

If you would like to specify how often to probe, you can do that using the 'periodSeconds' option.

If you would like to specify how many attempts to make, you can use the 'failureThreshold' option.

Example:
```livenessProbe:
    httpGet:
        path: /api/ready
        port: 8080
    initialDelaySeconds: 10
    periodSeconds: 5
    failureThreshold: 8
```