# How to select first item in an ItemGroup

I was entirely unaware of this [simple trick](https://gist.github.com/shadow-cs/cb5499b010bdacd1f778be29daf7f04c)! Turns out that when you assign a property value to an item metadata, and there are multiple items, all items are iterated and the property is assigned consecutively to each item metadata, leaving you with the last such item metadata as the property value.

If you want the first item, you reverse the item group using the built-in item function and then assign the property:

```markup
<Target Name="FirstCompile" BeforeTargets="Compile">
  <ItemGroup>
    <Reversed Include="@(Compile->Reverse())" />
  </ItemGroup>
  <PropertyGroup>
    <First>%(Reversed.Identity)</First>
  </PropertyGroup>

  <Message Text="First compile item is $(First)" Importance="high" />
</Target>
```

