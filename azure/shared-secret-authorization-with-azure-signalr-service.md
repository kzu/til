# Shared secret authorization with Azure SignalR Service

While testing out [Azure Functions development and configuration with Azure SignalR Service](https://docs.microsoft.com/en-us/azure/azure-signalr/signalr-concept-serverless-development-config#negotiate-experience-in-class-based-model), I needed a very simple key-based \(shared secret\) authorization mechanism so that my console-based SignalR client could connect to my Azure SignalR Service-powered hub using a very simple mechanism. 

The docs and the sample showcased the `negotiate` endpoint returning the connection info directly:

```text
[FunctionName("negotiate")]
public SignalRConnectionInfo Negotiate([HttpTrigger(AuthorizationLevel.Anonymous)]HttpRequest req)
```

But a [stackoverflow answer](https://stackoverflow.com/a/55586165/24684) pointed me to the solution: just the proper `IActionResult` using `OkObjectResult` with the connection info when the access key is properly validated:

```text
[FunctionName("negotiate")]
public IActionResult Negotiate(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")] HttpRequest req,
    [SignalRConnectionInfo(HubName = "events")] SignalRConnectionInfo connectionInfo)
{
    var expectedKey = Environment.GetEnvironmentVariable("AccessKey");
    if (string.IsNullOrEmpty(expectedKey))
        return new OkObjectResult(connectionInfo);

    var accessKey = req.Query["accessKey"];
    if (StringValues.IsNullOrEmpty(accessKey) ||
        !StringValues.Equals(expectedKey, accessKey))
        return new UnauthorizedResult();

    return new OkObjectResult(connectionInfo);
}
```









