# Ignore folder from dotnet-format

The easiest way to ignore an entire folder (i.e. one containing files you're syncing from an external repository such as [catbag](https://github.com/devlooped/catbag) using [dotnet-file](https://www.nuget.org/packages/dotnet-file)) is to create a `.editorconfig` file with the following content:

```editorconfig
[*]
generated_code = true
```
