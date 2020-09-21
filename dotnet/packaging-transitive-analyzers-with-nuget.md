# Packaging transitive analyzers with NuGet

Consider the scenario of [ThisAssembly](https://github.com/kzu/ThisAssembly) and its referenced packages: the main package is essentially a meta-package so that anyone wanting to leverage all the codegen in all the ThisAssembly.\* packages can reference a single one. By default, NuGet pack will create a package that declares the project reference dependencies like so:

```markup
    <dependencies>
      <group targetFramework=".NETStandard2.0">
        <dependency id="ThisAssembly.AssemblyInfo" version="42.42.42" exclude="Build,Analyzers" />
        <dependency id="ThisAssembly.Metadata" version="42.42.42" exclude="Build,Analyzers" />
        <dependency id="ThisAssembly.Project" version="42.42.42" exclude="Build,Analyzers" />
        <dependency id="ThisAssembly.Strings" version="42.42.42" exclude="Build,Analyzers" />
      </group>
    </dependencies>
```

 Note all those `exclude`. Not good since now it means projects referencing this package will not get the transitive analyzers, which are precisely the point of this meta-package. 

The fix is [highly non-obvious](https://github.com/NuGet/Home/issues/3697#issuecomment-342983009): you must explicitly state that _none_ of the referenced project assets are to be flagged as private:

```markup
  <ItemGroup>
    <ProjectReference Include="../ThisAssembly.AssemblyInfo/ThisAssembly.AssemblyInfo.csproj" PrivateAssets="none" />
    <ProjectReference Include="../ThisAssembly.Metadata/ThisAssembly.Metadata.csproj" PrivateAssets="none" />
    <ProjectReference Include="../ThisAssembly.Project/ThisAssembly.Project.csproj" PrivateAssets="none" />
    <ProjectReference Include="../ThisAssembly.Strings/ThisAssembly.Strings.csproj" PrivateAssets="none" />
  </ItemGroup>
```

This properly allows the transitive build and analyzer assets to be properly installed on the referencing project.



