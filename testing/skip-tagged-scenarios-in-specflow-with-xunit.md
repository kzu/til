---
description: How to skip scenarios with a certain tag from executing
---

# Skip tagged scenarios in SpecFlow with Xunit

First apply a tag of your choosing to flag scenarios that are still not ready for running:

```text
Feature: Skipping scenarios by tag

@Draft
Scenario: This is not ready yet
    Given Something not done yet
    Then It should not fail the CI build (yet)
```

Now in the `[Binding]` class for any of the steps in the scenario, create a constructor that receives the `ScenarioContext` as follows:

```text
public Steps(ScenarioContext context)
{
   Skip.If(context.ScenarioInfo.Tags.Contains("Draft"));
}
```

That's it. Turns out that the generated test for a scenario is annotated with `[SkippableFact]` so you can just skip from anywhere during the test run. From [SkippableFact](https://www.nuget.org/packages/Xunit.SkippableFact/) package.

