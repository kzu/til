---
description: 'How to use HashCode type in full .NET, which doesn''t support it'
---

# Using HashCode in .NETFramework

So, I just learned that not even in .net472/net48, you can leverage the cool [HashCode](https://github.com/dotnet/coreclr/blob/master/src/System.Private.CoreLib/shared/System/HashCode.cs) type from .NETCore. It **is** [mentioned in docs](https://docs.microsoft.com/en-us/dotnet/api/system.hashcode?view=dotnet-plat-ext-3.1) as available in ".NET Platform Extensions 3.1", which isn't quite clear what it means. After a quick search here and there, I found out the [Microsoft.Bcl.HashCode](https://www.nuget.org/packages/Microsoft.Bcl.HashCode/) package, which seems to be one such ".NET platform extension" \(although the version \# doesn't match either :/\).

With that, I can now make a `KeyValuePairComparer<TKey, TValue>`to check for dictionaries equality like:

```text
	public class KeyValuePairComparer<TKey, TValue> : IEqualityComparer<KeyValuePair<TKey, TValue>>
	{
		public static KeyValuePairComparer<TKey, TValue> Default { get;} = new KeyValuePairComparer<TKey, TValue>();
		
		public bool Equals(KeyValuePair<TKey, TValue> x, KeyValuePair<TKey, TValue> y) => object.Equals(x.Key, y.Key) && object.Equals(x.Value, y.Value);

		public int GetHashCode(KeyValuePair<TKey, TValue> obj) => HashCode.Combine(obj.Key, obj.Value);
	}
```

To be used as: 

```text
IReadOnlyDictionary<string, string> first = ...
IReadOnlyDictionary<string, string> second = ...

return first.SequenceEquals(second, KeyValuePairComparer<string, string>.Default);

```



