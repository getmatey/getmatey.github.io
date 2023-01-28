# Concepts
- Running containers communicate [service configurations](#service-configuration) to Matey through labels, which are used to configure traffic rules on the webserver.
- [Frontends](#frontends) specify routes to [backends](#backends) based on request properties, such as a host name.
- [Backends](#backends) are formed of one or more containers which process incoming requests.

## Frontends
A frontend is a set of rules which specify how incoming traffic should be forwarded through the webserver. If the frontend rules match request properties, the request will be forwarded to the corresponding backend.

### Rules
| Expression          | Description         |
| ------------------- | ------------------- |
| `Host: example.com` | Match request host. |

## Backends
A backend is a set of rules which specify how traffic is distributed to containers. Traffic can be directed to single containers or load-balanced across multiple.

### Load-balancing

#### Weighting
Weighting determines the proportion of traffic each container should handle. For example, if `container_1` has a weight of `20` and `container_2` has a weight of `80`, then `container_1` will receive 20% of the traffic while `container_2` will receive 80%.

#### Sticky sessions
Sticky sessions are a feature of most load balancers which ensure fixed session paths.

When enabled, a cookie is set on the first request with the load balancer. This cookie is then used to ensure that all following requests are sent to the same container.

??? note
    Sticky sessions are usually helpful if your application retains state in memory. They are otherwise generally not necessary.

    Enabling sticky sessions will also effectively make the weighting "by session" instead of "by request."

## Configuration
Matey's configuration is separated into two categories:

- [Global configuration](#global-configuration) which is defined at start-up and determines how Matey runs and communicates with your infrastructure.
- [Service configuration](#service-configuration) which is defined at runtime and determines how traffic is forwarded to your containers.

### Global configuration
The global configuration specifies connections to webservers to configure and container runtimes. By default, i.e. with no configuration, the local machine is used.

#### Configuration file
Matey will find a `matey.json` file in its working directory. When installed, this is `C:\Program Files\Matey`.

See the [global configuration](configuration/global.md) reference for more information.

### Service configuration
The service configuration is used when setting up [frontends](#frontends) and [backends](#backends).

Matey will watch for changes in these configurations and hot-reload webserver configurations automatically to create routes to your services.

Please see the [Docker configuration](./configuration/docker.md) reference for more information.