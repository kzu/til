# Accessing Tor URLs via HttpClient with .NET6

In .NET6, there is built-in [support for SOCKS4/5 proxies](https://github.com/dotnet/runtime/pull/48883)! This means you can install the [Tor](https://dist.torproject.org/torbrowser/) service and without anything else, access any `.onion` URL with plain `HttpClient`:

```csharp
using System;
using System.Net;
using System.Net.Http;

var http = new HttpClient(new HttpClientHandler
{
    Proxy = new WebProxy("socks5://127.0.0.1:1338")
});

Console.WriteLine(await http.GetStringAsync("https://bbcnewsv2vjtpsuy.onion"));
```

NOTE: I could not get this to work with gRPC, which requires HTTP/2.

You can get the daily .NET6 SDK installer permalinks from:

| Platform | Main |
| :--- | :---: |
| **Windows \(x64\)** | [https://aka.ms/dotnet/6.0/daily/dotnet-sdk-win-x64.exe](https://aka.ms/dotnet/6.0/daily/dotnet-sdk-win-x64.exe) |
| **Windows \(x86\)** | [https://aka.ms/dotnet/6.0/daily/dotnet-sdk-win-x86.exe](https://aka.ms/dotnet/6.0/daily/dotnet-sdk-win-x86.exe) |
| **Windows \(arm64\)** | [https://aka.ms/dotnet/6.0/daily/dotnet-sdk-win-arm64.exe](https://aka.ms/dotnet/6.0/daily/dotnet-sdk-win-arm64.exe) |
| **macOS \(x64\)** | [https://aka.ms/dotnet/6.0/daily/dotnet-sdk-osx-x64.pkg](https://aka.ms/dotnet/6.0/daily/dotnet-sdk-osx-x64.pkg) |
| **macOS \(arm64\)** | [https://aka.ms/dotnet/6.0/daily/dotnet-sdk-osx-arm64.pkg](https://aka.ms/dotnet/6.0/daily/dotnet-sdk-osx-arm64.pkg) |

