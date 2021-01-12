# Hide contentFiles from your nuget packages

When you package compile items with your package \(i.e. `.cs)`, they are visible by default in your consumer's project. That's not always what you want. From [a github issue on NuGet](https://github.com/NuGet/Home/issues/4856#issuecomment-288151716), I learned a neat trick to hide all the files in your package, automatically! All you need is to include a .props file in your package `build` folder \(i.e. if your package is named `Foo`, make sure the file ends up `build\Foo.props`\)

```markup
<Project>

  <ItemGroup>
    <Compile Update="@(Compile)">
      <Visible Condition="'%(NuGetItemType)' == 'Compile' and '%(NuGetPackageId)' == 'Foo'">false</Visible>
    </Compile>
  </ItemGroup>

</Project>
```

This works because NuGet generates a .props file containing all your contentFiles items, under the `obj` folder, which contains both pieces of metadata used above to filter the visibility on items coming from our package. 

