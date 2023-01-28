# Docker
A straight-forward docker setup.
![Diagram 2](../img/diagram_2_light_mode.png#only-light)
![Diagram 2](../img/diagram_2_dark_mode.png#only-dark)

## Install Matey with the Docker backend
If you haven't already, [install Matey](../index.md#1---installation).

By default, it is configured to work with the local Docker Engine.

## Matey will watch your Docker containers and configure your webserver
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

## Container Labels
| **Label**                                                                | **Description**                                                            |
| ------------------------------------------------------------------------ | -------------------------------------------------------------------------- |
| `matey.enabled=true`                                                     | Whether Matey should configure the webserver to forward to the container. |
| `matey.<segment_name>.port=80`                                           | Sets the container port to which traffic should be forwarded to.           |
| `matey.<segment_name>.weight=50`                                         | Assigns a weight to the container for load balancing.                      |
| `matey.<segment_name>.loadbalancer.stickiness.enabled=true`              | Whether to use sticky load balancing for the container.                    |
| `matey.<segment_name>.loadbalancer.stickiness.cookieName=_mateyaffinity` | Sets the cookie name to use to track sessions for sticky load balancing.   |
| `matey.<segment_name>.domain=example.com`                                | Sets the default base domain provided to the frontend rule expression.     |
| `matey.<segment_name>.frontend.rule=Host:api.example.com`                | Overrides the frontend rule. Default: `Host:{container_name}.{domain}`.    |
