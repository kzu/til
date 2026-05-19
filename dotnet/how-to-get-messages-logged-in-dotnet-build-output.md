---
description: <Message Importance="high" Text="Gone?!" /> gone?
---

# How to get messages logged in dotnet build output

I learned that the reason my high-importance messages were totally gone from the `dotnet build` output is due to the new [terminal logger](https://learn.microsoft.com/en-us/dotnet/core/compatibility/sdk/9.0/terminal-logger) being the new default.

To bring the old logger (and those messages back), you need to pass `-tl:off` now, or set `MSBUILDTERMINALLOGGER=false` [envvar](https://github.com/dotnet/msbuild/blob/main/documentation/terminallogger/Opt-In-Mechanism.md).&#x20;

You can set this for all your local builds in a folder by creating an `MSBuild.rsp` with the `-tl:off` switch which is [picked up automatically by MSBuild](https://learn.microsoft.com/en-us/visualstudio/msbuild/msbuild-response-files?view=visualstudio).

