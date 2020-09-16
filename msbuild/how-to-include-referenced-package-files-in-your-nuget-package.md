# How to include package reference files in your nuget

This is a particularly common scenario if you're developing MSBuild tasks or Roslyn code analyzers: all the dependencies you use in your task, analyzer or source generator, need to be included in your nuget package too, alongside your project primary output \(i.e. under the proper`analyzer, tools or build`folders\). A very convenient way \(if not comprehensive, since it won't include with transitive dependencies\) to do so is by annotating your `PackageReference` themselves, like:

```markup
<PackageReference Include="Scriban" Version="2.1.2" PrivateAssets="all" Pack="true" />
```

The `Pack` metadata value can then be used to include the assets in the package with the following target:

```markup
  <!-- For every PackageReference with Pack=true, we include the assemblies from it in the package -->
  <Target Name="AddPackDependencies" Inputs="@(RuntimeCopyLocalItems)" Outputs="%(RuntimeCopyLocalItems.NuGetPackageId)" AfterTargets="ResolvePackageAssets">
    <ItemGroup>
      <NuGetPackageId Include="@(RuntimeCopyLocalItems -> '%(NuGetPackageId)')" />
    </ItemGroup>
    <PropertyGroup>
      <NuGetPackageId>@(NuGetPackageId -&gt; Distinct())</NuGetPackageId>
    </PropertyGroup>
    <ItemGroup>
      <PackageReferenceDependency Include="@(PackageReference -&gt; WithMetadataValue('Identity', '$(NuGetPackageId)'))" />
    </ItemGroup>
    <PropertyGroup>
      <NuGetPackagePack>@(PackageReferenceDependency -> '%(Pack)')</NuGetPackagePack>
    </PropertyGroup>
    <ItemGroup Condition="'$(NuGetPackagePack)' == 'true'">
      <_PackageFiles Include="@(RuntimeCopyLocalItems)" PackagePath="$(BuildOutputTargetFolder)/$(TargetFramework)/%(Filename)%(Extension)" />
      <RuntimeCopyLocalItems Update="@(RuntimeCopyLocalItems)" CopyLocal="true" Private="true" />
      <ResolvedFileToPublish Include="@(RuntimeCopyLocalItems)" CopyToPublishDirectory="PreserveNewest" RelativePath="%(Filename)%(Extension)" />
    </ItemGroup>
  </Target>
```

Quite a few things to note in the above target that aren't too obvious:

1. We use the `RuntimeCopyLocalItems` item group which is resolved by `ResolvePackageAssets` and contains the stuff that is needed for the dependency to run \(i.e. the actual binaries, not reference assemblies, if it includes them\).
2. We use Inputs/Outputs on it so we can batch by `%(RuntimeCopyLocalItems.NuGetPackageId)`: this makes processing simpler inside the target, since we'll be dealing with a single `NuGetPackageId` for each batch, regardless of how many `RuntimeCopyLocalItems`there are.
3. We next get that package ID as a property, and find the `@(PackageReference)` with that ID, to determine if it needs to be packed or not.
4. Note we use property syntax next since we know there can be at most one such `@(PackageReferenceDependency)`, in which case we'd get either an empty value or `true` for `%(Pack)`.
5. If we have to pack, we include all the `@(RuntimeCopyLocalItems)` \(in the current batch, MSBuild does this for us for free thanks to the Inputs/Outputs\) and use as the package path the same as what the primary project output will use. These are added directly as `_PackageFiles` which is what the SDK-style project `Pack` uses.
6. We update the items metadata so they are also flagged as copy-local
7. Finally, the `ResolvedFileToPublish` is useful when creating dotnet tools, since packing those is slightly different and includes a publish operation.



