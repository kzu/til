---
description: >-
  How to quickly and simply configure redirections without writing code in Azure
  Functions
---

# Code-less redirection with serverless Azure Functions

Say you want to have a nicer URI for something \(like an Azure storage blob, a feed or something else\). You  likely have a nice short custom domain \(i.e. I use kzu.io for things like this\), and would like to set up arbitrary \(temporary or permanent\) redirections. This can trivially be achieved by creating an empty Functions App and leveraging [Functions Proxies](https://docs.microsoft.com/en-us/azure/azure-functions/functions-proxies). 

The `proxies.json` file for code-less redirects looks as follows:

```javascript
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "[SOME_ID]": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "[SHORT_PATH_HERE]"
            },
            "responseOverrides": {
                "response.statusCode": "[301|302|307|308|",
                "response.headers.location": "[LONG_URL_HERE]"
            }
        }
    }
}
```

You can have as many of those IDs/entries as needed. For example, this is one I use to set up `https://pkg.kzu.io/index.json` &gt; `https://kzu.blob.core.windows.net/nuget/index.json`:

```javascript
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "default": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "index.json"
            },
            "responseOverrides": {
                "response.statusCode": "307",
                "response.headers.location": "https://kzu.blob.core.windows.net/nuget/index.json"
            }
        }
    }
}
```



