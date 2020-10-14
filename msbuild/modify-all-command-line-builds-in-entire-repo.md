# Modify all command-line builds in entire repo

I used to place `msbuild.rsp` [response files](https://docs.microsoft.com/en-us/visualstudio/msbuild/msbuild-response-files?view=vs-2019#msbuildrsp) alongside solution and project files to avoid repeating `-nr:false -v:m -bl` \(no node reuse, minimal verbosity, binlogs\). It was super annoying to have to have it all over the place. While [browsing another repo](https://github.com/microsoft/vs-streamjsonrpc/blob/master/Directory.Build.rsp), I just learned that since MSBuild 15.6+, you can now have a [Directory.Build.rsp response file](https://docs.microsoft.com/en-us/visualstudio/msbuild/msbuild-response-files?view=vs-2019#directorybuildrsp) that affects every build in every descendent folder. This sounds like a great default one for me:

```text
# See https://docs.microsoft.com/en-us/visualstudio/msbuild/msbuild-response-files
-nr:false
-m:1
-v:m
-clp:Summary;ForceNoAlign
```

Note I also prefer much more the `-` syntax rather than `/` for switches.

