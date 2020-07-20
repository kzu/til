---
description: >-
  Even if it's typically used in a static field, the values never leak since
  they are bound to a transient ExecutionContext
---

# AsyncLocal never leaks and is safe for CallContext-like state

Sometime ago I wrote on [How to migrate CallContext to .NETStandard and .NETCore](https://www.cazzulino.com/callcontext-netstandard-netcore.html), and one question mentioned that the values themselves might leak, which actually is not the case, as shown here, due to the "magic" that is AsyncLocal in combination with the [transient nature of the ExecutionContext](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Private.CoreLib/src/System/Threading/ExecutionContext.cs#L133-L200):

```text
static AsyncLocal<object> local = new AsyncLocal<object>();

async Task Main()
{
    WeakReference data = null;

    await Task.Run(() =>
    {
        var o = new object();
        data = new WeakReference(o);
        // We assign to the static async local, to see if it leaks.
        local.Value = o;
    });

    // After execution is finished, we do have a live reference still.
    System.Diagnostics.Debug.Assert(data.IsAlive == true);

    GC.Collect();

    // But a GC proves nobody is holding a strong reference to it.
    System.Diagnostics.Debug.Assert(data.IsAlive == false);
}

```

