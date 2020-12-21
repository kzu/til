# Use C\# 9 records in non-net5.0 projects

The new C\# 9 records syntax is quite nice:

```csharp
[DebuggerDisplay("{Name} = {Value}")]
record ResourceValue(string Name, string Value, bool HasFormat)
{
    public bool IsIndexed { get; init; }
    public List<string> Format { get; } = new List<string>();
}
```

When using it in a non-_net5.0_ project \(i.e. _netstandard2.0_\), [compilation fails with an error](https://github.com/dotnet/roslyn/issues/45510):

```text
Predefined type 'System.Runtime.CompilerServices.IsExternalInit' is not defined or imported
```

To workaround the issue, simply declare the missing type in your project:

```csharp
// Licensed to the .NET Foundation under one or more agreements.
// The .NET Foundation licenses this file to you under the MIT license.
// See the LICENSE file in the project root for more information.

using System.ComponentModel;

namespace System.Runtime.CompilerServices
{
    /// <summary>
    /// Reserved to be used by the compiler for tracking metadata.
    /// This class should not be used by developers in source code.
    /// </summary>
    [EditorBrowsable(EditorBrowsableState.Never)]
    sealed class IsExternalInit
    {
    }
}
```

\([as seen elsewhere too](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Text.Json/tests/Serialization/IsExternalInit.cs)\). Note the class doesn't even need to be public.

If you're multitargeting net5.0 and other TFMs, just add this bit of MSBuild to remove it for net5.0 since it's built-in:

```markup
<ItemGroup>
  <Compile Remove="IsExternalInit.cs" Condition="'$(TargetFramework)' == 'net5.0'" />
</ItemGroup>
```

