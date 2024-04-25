# Stateful Sets Introduction

## Overview

Depending on your specific needs for your application deployment, if you need to use a stateful set over a regular deployment it is very simple...

You create your stateful set file almost exactly the same as you would if creating a config file for a regular deployment...

In your file, simply change the 'kind:' from 'Deployment' to 'StatefulSet'...

And add a field under 'spec:' for 'serviceName:'