# Global configuration

```json
{
  // Docker client configuration section
  "Docker": {
    // The communication endpoint for the docker engine API.
    //
    // On Windows installations, the local engine is reachable through a named pipe.
    "Endpoint": "npipe://./pipe/docker_engine"
  },

  // IIS management configuration section
  "IIS": {
    // The name of the server which hosts the IIS instance which is managed over WMI.
    //
    // If IIS is installed locally, leave this option commented out.
    //"ServerName": "SRV-0153"
  }
}
```

## Docker

- `Endpoint`: The endpoint which the Docker Engine can be reached at. On Windows, this is a named pipe which can be specified as `"npipe://./pipe/docker_engine"`. As such, this is the default value for the option.

## IIS

- `ServerName`: The name of the server which hosts the IIS instance. The server should have Windows Management Services running, please see [how to use WMI to configure IIS](https://learn.microsoft.com/en-us/previous-versions/iis/6.0-sdk/ms525309(v=vs.90)). **Note:** If you are using the local machine, do not configure this value or configure it as `null`.