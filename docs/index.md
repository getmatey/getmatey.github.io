# Getting started
Matey is an [open-source](https://github.com/getmatey/Matey) container ingress configurator for webservers. It configures your webservers to act as a reverse proxy or load balancer for your containers. It currently supports [Docker](configuration/docker.md) and IIS. All you need to do is point Matey at your container runtime and webservers.

Matey's design considers that existing webservers have decades of work, fixes and platform-specific optimizations behind them. Why re-invent the webserver when they are highly configurable and can easily integrate with other system components?

## Overview
Let's say your environment has a lot of containers, serving web requests, which should be reachable from the outside world. You need a reverse proxy to forward requests to the correct container. This becomes a huge effort to configure correctly, especially if your containers are often swapped or moved.

Matey is an automated configurator designed to perform this for you. It will watch your container runtime and automatically configure your webserver to forward traffic to your services from the outside world.

![Diagram 1](img/diagram_1_light_mode.png#only-light)
![Diagram 1](img/diagram_1_dark_mode.png#only-dark)

## Matey quick start (using Docker)

### 1 - Installation
!!! note
    Matey is currently only supported for **Windows Server** hosts using IIS. We may introduce support for Linux compatible webservers in the future.

You will first need to install Matey. Let's start there.

#### Prerequisites
- Docker ([Installation guide](https://learn.microsoft.com/en-us/virtualization/windowscontainers/quick-start/set-up-environment?tabs=dockerce){:target="_blank"})
- IIS 7.0+ ([Installation guide](https://learn.microsoft.com/en-us/iis/web-hosting/web-server-for-shared-hosting/installing-the-web-server-role){:target="_blank"})
- ARR module for IIS ([Installation guide](https://learn.microsoft.com/en-us/iis/extensions/installing-application-request-routing-arr/install-application-request-routing){:target="_blank"})

#### Use the installer
Grab the latest installer from the [releases](https://github.com/getmatey/Matey/releases) page.

Run the installer on your webserver. After installation, you should see =="Matey Configurator Service"== in your Services list.

??? tip
    Run the following PowerShell command to check the service installation:
    ```pwsh
    Get-Service -DisplayName "Matey Configurator Service" | Select DisplayName, StartType, Status
    ```
    You should see one entry, like this:
    ```
    DisplayName                StartType  Status
    -----------                ---------  ------
    Matey Configurator Service Automatic  Running
    ```

The service will be set to start automatically.

By default, Matey is configured to point to Docker and IIS on the local machine.

### 2 - Start a container - Matey sees it and configures your webserver
With the Matey service running and waiting for containers, let's start one!

```
docker start --label matey.enable=true --label matey.frontend.rule=Host:hello-world.localhost getmatey/hello-world
```

??? tip
    You can also use the equivalent `docker-compose.yml`:
    ```yaml
    version: '3'

    services:
        hello-world:
            image: getmatey/hello-world
            labels:
            - "matey.enabled=true"
            - "matey.frontend.rule=Host: hello-world.localhost"
    ```
    and start the container with the following command:
    ```
    docker compose up -d
    ```
When Matey sees the service, it configures your webserver to route requests to your container so you can reach it. Let's check it with PowerShell.
```pwsh
(Invoke-WebRequest http://hello-world.localhost).Content
```
Should give you:
```
Hello, world!
```