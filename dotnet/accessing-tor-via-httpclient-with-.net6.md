# Accessing Tor via HttpClient with .NET6

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
| **Windows \(x64\)** | \[SDK Installer\]\[win-x64-sdkinstaller-6.0.X\] \[Runtime Installer\]\[win-x64-installer-6.0.X\] \(\[Checksum\]\[win-x64-installer-checksum-6.0.X\]\) \[zip\]\[win-x64-zip-6.0.X\] \(\[Checksum\]\[win-x64-zip-checksum-6.0.X\]\) |
| **Windows \(x86\)** | \[SDK Installer\]\[win-x86-sdkinstaller-6.0.X\] \[Runtime Installer\]\[win-x86-installer-6.0.X\] \(\[Checksum\]\[win-x86-installer-checksum-6.0.X\]\) \[zip\]\[win-x86-zip-6.0.X\] \(\[Checksum\]\[win-x86-zip-checksum-6.0.X\]\) |
| **Windows \(arm64\)** | \[SDK Installer\]\[win-arm64-sdkinstaller-6.0.X\] \[Runtime Installer\]\[win-arm64-installer-6.0.X\] \(\[Checksum\]\[win-arm64-installer-checksum-6.0.X\]\) \[zip\]\[win-arm64-zip-6.0.X\] \(\[Checksum\]\[win-arm64-zip-checksum-6.0.X\]\) |
| **macOS \(x64\)** | \[SDK Installer\]\[osx-x64-sdkinstaller-6.0.X\] \[Runtime Installer\]\[osx-x64-installer-6.0.X\] \(\[Checksum\]\[osx-x64-installer-checksum-6.0.X\]\) \[tar.gz\]\[osx-x64-targz-6.0.X\] \(\[Checksum\]\[osx-x64-targz-checksum-6.0.X\]\) |
| **macOS \(arm64\)** | \[SDK Installer\]\[osx-arm64-sdkinstaller-6.0.X\] \[Runtime Installer\]\[osx-arm64-installer-6.0.X\] \(\[Checksum\]\[osx-arm64-installer-checksum-6.0.X\]\) \[tar.gz\]\[osx-arm64-targz-6.0.X\] \(\[Checksum\]\[osx-arm64-targz-checksum-6.0.X\]\) |

 



