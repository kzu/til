# Write entire XML fragments in MSBuild with XmlPoke

I recently needed to write an entire project file via a targets \(weird, I know\). I was dreading all the crazy angle brackets escaping as `&lt;` and `&gt;` when I decided to check the [latest official docs on XmlPoke](https://docs.microsoft.com/en-us/visualstudio/msbuild/xmlpoke-task?view=vs-2019) to refresh the parameters and format. To my surprise, I found this example:

```markup
<Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <Namespace>
        <Namespace Prefix="dn" Uri="http://schemas.microsoft.com/appx/manifest/foundation/windows10" />
        <Namespace Prefix="mp" Uri="http://schemas.microsoft.com/appx/2014/phone/manifest" />
        <Namespace Prefix="uap" Uri="http://schemas.microsoft.com/appx/manifest/uap/windows10" />
    </Namespace>
</PropertyGroup>

<Target Name="Poke">
  <XmlPoke
    XmlInputPath="Sample.xml"
    Value="MyId"
    Query="/dn:Package/mp:PhoneIdentity/@PhoneProductId"
    Namespaces="$(Namespace)"/>
</Target>
</Project>
```

Notice how the `$(Namespace)` property has **beautiful** XML inside. Which only makes sense, since, there had to be **some** advantage in MSBuild being XML, right? So I figured, if the namespaces can be a full nested XML element, could the `Value` be too? And the answer was a [resounding YEAHHH](https://github.com/kzu/SmallSharp/blob/main/src/SmallSharp/SmallSharp.targets#L88-L100):

```markup
  <PropertyGroup>
    <UserPropertyGroup>
      <PropertyGroup>
        <ActiveDebugProfile>$(StartupFile)</ActiveDebugProfile>
      </PropertyGroup>
    </UserPropertyGroup>    
  </PropertyGroup>

  <XmlPoke XmlInputPath="$(MSBuildProjectFullPath).user"
           Value="$(UserPropertyGroup)"
           Query="/msb:Project"
           Namespaces="$(UserProjectNamespace)"/>
```

That writes an entire `<PropertyGroup>` element into a .user project file!

For the attentive reader: you'd think I made a mistake, since, in XML-land, elements inherit the XML namespace of their parent element. Seems like the XML namespace management in XmlPoke is a bit on the loose side of things: the above snippets are in a .targets file without any xmlns \(it's an SDK-style `<Project>` without namespace, to keep it clean. Yet, the property group is added without messing up the namespace:

```markup
<Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <ActiveDebugProfile>Program.cs</ActiveDebugProfile>
  </PropertyGroup>
</Project>
```

If it had inserted it without a namespace, per the XML spec, it should have added an `xmlns=""` to clear the parent Project node namespace. But alas, it didn't, and in this particular case, it makes it way more convenient this way :\).

I'm using this in [SmallSharp](https://github.com/kzu/SmallSharp) to initialize the .user project options properly with the right startup file for multi-startup top-level statements scripts in a single project :\).

