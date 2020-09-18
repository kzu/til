# Use C\# 9 records in non-net5.0 projects

When using the new records syntax, [compilation fails with an error](https://github.com/dotnet/roslyn/issues/45510):

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
    public sealed class IsExternalInit
    {
    }
}
```

\([as seen elsewhere too](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Text.Json/tests/Serialization/IsExternalInit.cs)\)

