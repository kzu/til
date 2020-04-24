---
description: >-
  How to run xunit tests conditionally depending on the environment that is
  running the tests
---

# Conditional unit tests

Turns out that there aren't any conditional `FactAttribute` in [xunit](https://github.com/xunit/xunit), but you can pretty much copy wholesale the extensions created by the [ASP.NET Core team from their repo](https://github.com/dotnet/runtime/tree/master/src/libraries/Common/tests/Extensions/TestingUtils/Microsoft.AspNetCore.Testing/src/xunit).

It allows you to do things like:

```text
[SkipOnCI]
[ConditionalFact]
public async Task SomeTest()
```

to avoid running a test entirely if you're on CI. the implementation didn't take into account CI builds running via GH actions/workflows, so I tweaked the [OnCI\(\) method](https://github.com/dotnet/runtime/blob/master/src/libraries/Common/tests/Extensions/TestingUtils/Microsoft.AspNetCore.Testing/src/xunit/SkipOnCIAttribute.cs#L38) like so to also consider the commonly used `CI` envvar too:

```text
public static bool OnCI() => 
   (bool.TryParse(Environment.GetEnvironmentVariable("CI"), out var ci) && ci) || 
   OnHelix() || OnAzdo();
```

I didn't think it was necessary to create an `OnGitHub` method since the envvar is just `CI` and well, the method is already called `OnCI` :\)

Another alternative is using the [Xunit.SkippableFact nuget package](https://github.com/AArnott/Xunit.SkippableFact).

