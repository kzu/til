---
description: How to locate dotnet from the currently running .NET Core application
---

# How to locate dotnet

I've seen a bunch of `DotNetMuxer` implementations, but they all look quite similar \(if not exactly the same\). 

* [ASP.NET Core](https://github.com/dotnet/aspnetcore/blob/master/src/Shared/CommandLineUtils/Utilities/DotNetMuxer.cs) \(same used by others, like [Orleans](https://github.com/dotnet/orleans/blob/master/src/Orleans.CodeGenerator.MSBuild.Tasks/DotNetMuxer.cs)\)
* [.NET runtime](https://github.com/dotnet/runtime/blob/master/src/libraries/Common/src/Extensions/CommandLineUtils/Utilities/DotNetMuxer.cs), [.NET extensions](https://github.com/dotnet/extensions/blob/master/src/Shared/src/CommandLineUtils/Utilities/DotNetMuxer.cs)
* [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Analyzer/ExtensionViewer/DotNetMuxer.cs) \(same as [Azure Functions SDK](https://github.com/Azure/azure-functions-vs-build-sdk/blob/master/src/Microsoft.NET.Sdk.Functions.MSBuild/Tasks/DotNetMuxer.cs)\)
* [.NET command-line-api](https://github.com/dotnet/command-line-api/blob/master/src/System.CommandLine.Suggest/DotnetMuxer.cs)

Seems like there are two approaches: those that try to locate a `FX_DEPS_FILE` file and those that just use the current process' `MainModule` \(which would be `dotnet[.exe]` itself for a .NET Core app. The latter looks simpler and the former seems unnecessary, until you take into account [self-contained .NET Core apps](https://docs.microsoft.com/en-us/dotnet/core/deploying/#publish-self-contained) or even [trimmed self-contained](https://docs.microsoft.com/en-us/dotnet/core/deploying/trim-self-contained), but even that version doesn't work in those cases :\(

