# Suppress dependencies when packing

Some packing scenarios, especially those involving tools, require ignoring all dependencies \(PackageReference as well as framework references\) from the resulting package dependencies.

I [learned of a property](https://github.com/NuGet/Home/issues/6354) supported by `dotnet pack` for this:  

```markup
<PropertyGroup>
  <SuppressDependenciesWhenPacking>true</SuppressDependenciesWhenPacking>
</PropertyGroup>
```

NuGetizer has more flexibility in this regard, providing both a `$(PackFrameworkReferences)` property as well as granular control over referenced packages via the `Pack` metadata on each `PackageReference`:  

```markup
<PropertyGroup>
  <!-- Opt out of all framework references/dependencies -->
  <PackFrameworkReferences>false</PackFrameworkReferences>
</PropertyGroup>

<ItemDefinitionGroup>
  <PackageReference>
    <!-- Unless specified otherwise, opt-out of all dependencies -->
    <Pack>false</Pack>
  </PackageReference>
</ItemDefinitionGroup>

<ItemGroup>
  <!-- Example of explicitly opting in for a particular one -->
  <PackageReference Include="Foo" Pack="true" />
  <PackageReference Include="Bar" /> <!-- Will not be packed -->
</ItemGrop>
```

But in order to simplify this scenario further, both the compatibility `SuppressDependenciesWhenPacking` property as well as a new `PackDependencies` property is supported in [nugetizer](https://www.nuget.org/packages/nugetizer) to achieve the same.

