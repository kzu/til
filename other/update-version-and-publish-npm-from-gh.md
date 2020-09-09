---
description: >-
  Setting up CI with GitHub Actions to update a node.js package version from
  GitHub release tag and publish it to npm
---

# Update version and publish npm from GH

Super proud of this, since it's my first node.js package ever \(created brand-new account on [https://www.npmjs.com/](https://www.npmjs.com/) and all 😁\), for doing [syntax highlighting](https://github.com/dotnetconfig/highlightjs-dotnetconfig) for [dotnetconfig](https://dotnetconfig.org/) that works in [docfx](https://github.com/dotnetconfig/dotnet-config/tree/dev/docs). 

Assuming you already have a `package.json` in the root repo dir, add the `.github\workflows\npm.yml` as follows:

```text
name: publish

on:
  release:
    types: [created]

jobs:
  publish-npm:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v1
        with:
          node-version: 12
          registry-url: https://registry.npmjs.org/
      - name: Set version from tag
        run: npm --no-git-tag-version version ${GITHUB_REF#refs/*/}
      - run: npm publish
        env:
          NODE_AUTH_TOKEN: ${{secrets.npm_token}}
```

1. I'm running only on release creation
2. Using the GitHub envvar for the [checked out tag](https://stackoverflow.com/questions/58177786/get-the-current-pushed-tag-in-github-actions/58178121#58178121), I'm [updating the version](https://docs.npmjs.com/cli/version) but opting out of the git behavior \(which fails on CI because it's a detached head at that point\)
3. Finally you'll need to create that secret in [GH](https://docs.github.com/en/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets) for the publish step.

