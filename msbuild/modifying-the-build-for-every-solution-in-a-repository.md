# Modifying the build for every solution in a repository

Just like you can have [Directory.Build.props and Directory.Build.targets](https://docs.microsoft.com/en-us/visualstudio/msbuild/customize-your-build?view=vs-2019#directorybuildprops-and-directorybuildtargets) to customize your projects' build, you can also use `Directory.Solution.props` and `Directory.Solution.targets` to customize your solutions \(command-line\) builds. Just like the original \(older?\) mecanisms, [Visual Studio will not load those customizations either](https://docs.microsoft.com/en-us/visualstudio/msbuild/customize-your-build?view=vs-2019#customize-the-solution-build), however.

In order to inspect how and where they are included in the build, it's useful to set use the [troubleshooting technique](https://docs.microsoft.com/en-us/visualstudio/msbuild/how-to-build-specific-targets-in-solutions-by-using-msbuild-exe?view=vs-2019#troubleshooting) of setting the envvar `MSBUILDEMITSOLUTION=1` and run a build. You can inspect the _.metaproj_ MSBuild project generated from the solution, where you will see the imported projects.

For a Directory.Solution.props with:

```markup
<Project>
  <PropertyGroup>
    <SolutionPropsProp>from-solution.props</SolutionPropsProp>
  </PropertyGroup>
</Project>
```

And a Directory.Solution.targets with:

```markup
<Project>
  <PropertyGroup>
    <SolutionTargetsProp>from-solution.targets</SolutionTargetsProp>
  </PropertyGroup>

  <Target Name="CustomSolutionBuild">
  </Target>
</Project>
```

You will see a _.metaproj_ similar to:

```markup
<?xml version="1.0" encoding="utf-8"?>
<Project ToolsVersion="Current" xmlns="http://schemas.microsoft.com/developer/msbuild/2003" InitialTargets="ValidateSolutionConfiguration;ValidateToolsVersions;ValidateProjects" DefaultTargets="Build">
  <PropertyGroup>
    <RoslynTargetsPath>C:\Program Files\dotnet\sdk\5.0.100-rc.2.20479.15\Roslyn</RoslynTargetsPath>
    <_DirectorySolutionPropsFile>Directory.Solution.props</_DirectorySolutionPropsFile>
    <_DirectorySolutionPropsBasePath>C:\Code\kzu\moq</_DirectorySolutionPropsBasePath>
    <DirectorySolutionPropsPath>C:\Code\kzu\moq\Directory.Solution.props</DirectorySolutionPropsPath>
    <SolutionPropsProp>from-solution.props</SolutionPropsProp>
    <Configuration>Debug</Configuration>
    <Platform>Any CPU</Platform>
    ...
    <_DirectorySolutionTargetsFile>Directory.Solution.targets</_DirectorySolutionTargetsFile>
    <_DirectorySolutionTargetsBasePath>C:\Code\kzu\moq</_DirectorySolutionTargetsBasePath>
    <DirectorySolutionTargetsPath>C:\Code\kzu\moq\Directory.Solution.targets</DirectorySolutionTargetsPath>
    <SolutionTargetsProp>from-solution.targets</SolutionTargetsProp>
  </PropertyGroup>
  ...
  <Target Name="_IsProjectRestoreSupported" Returns="@(_ValidProjectsForRestore)" />
  <Target Name="CustomSolutionBuild" />
  <Target Name="Build" Outputs="@(CollectedBuildOutput)">
  ...
</Project>
```

Notice how the targets aren't imported but rather embedded in specific places \(top of property group for .props-declared properties, bottom of property group for .targets-declared properties, and before Build target for targets\). If you have properties, they are actually even evaluated before being embedded in the file, i.e.:

```markup
<SolutionNow>$([System.DateTime]::Now)</SolutionNow>
```

is embedded in the .metaproj as the actually evaluated value, such as:

```markup
    <SolutionNow>10/21/2020 4:36:30 AM</SolutionNow>
```



 





