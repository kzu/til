# How to build project when content files change

Visual Studio will typically just report the project is up-to-date and doesn't need to be built if you just changed a `Content` or `None` item. If you want it to consider those file types to also trigger a build, just add the relevant items as `UpToDateCheckInput`:

```text
  <ItemGroup>
    <UpToDateCheckInput Include="@(Content);@(None)" />
  </ItemGroup>
```

