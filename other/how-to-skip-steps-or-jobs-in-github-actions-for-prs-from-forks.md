# How to skip steps or jobs in GitHub Actions for PRs from forks

[Encrypted secrets in GitHub](https://docs.github.com/en/free-pro-team@latest/actions/reference/encrypted-secrets#using-encrypted-secrets-in-a-workflow) actions aren't available for builds from forks, so if your build script includes PRs, like [Avatar](https://github.com/kzu/avatar/blob/main/.github/workflows/build.yml#L2-L6):

```yaml
on: 
  push:
    branches: [ main, dev, 'feature/*', 'rel/*' ]
  pull_request:
    types: [opened, synchronize, reopened]
```

You may need to limit steps or entire jobs from running when PRs are coming from forks due to missing secrets \(i.e. pushing an output package to a CI nuget feed, say\).

The "magic" thing is to use an `if` condition with the following expression:

```yaml
  push:
    name: push nuget.ci
    runs-on: ubuntu-latest
    needs: [build, acceptance]
    if: ${{ !github.event.pull_request.head.repo.fork }}
    steps:
```

Note that in the above case, an entire job is skipped, but you can also apply it to a step instead.

