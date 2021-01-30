# Push to protected branch from GitHub actions

It turns out that you really can't just `git push` from your GitHub actions [if the repository has branch protection turned on](https://github.community/t/how-to-push-to-protected-branches-in-a-github-action/16101) or required checks before merging. Sorta makes sense, but still a PITA.

The solution that worked for me was to [use a different token on checkout](https://github.community/t/how-to-push-to-protected-branches-in-a-github-action/16101/34). Since the awesome GitHub CLI [allows using a separate, higher-permissions token](https://github.com/cli/cli/issues/1229) named `GH_TOKEN` \(since depending on the command you use, you might need a different one than `GITHUB_TOKEN`\), I decided to \(ab\)use the same:

An [example workflow](https://github.com/devlooped/oss/blob/main/.github/workflows/changelog.yml) that uses this to generate a full changelog and push it to main on releases looks like this:

```text
name: changelog
on:
  release:
    types: [released]

env:
  GH_TOKEN: ${{ secrets.GH_TOKEN }}

jobs:
  changelog:
    runs-on: ubuntu-latest
    steps:
      - name: 🔍 GH_TOKEN
        if: env.GH_TOKEN == ''
        env: 
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        run: echo "GH_TOKEN=${GITHUB_TOKEN}" >> $GITHUB_ENV

      - name: 🤘 checkout
        uses: actions/checkout@v2
        with:
          fetch-depth: 0
          ref: main
          token: ${{ env.GH_TOKEN }}

      - name: ⚙ changelog
        uses: faberNovel/github-changelog-generator-action@master
        with:
          options: --token ${{ secrets.GITHUB_TOKEN }} --o changelog.md

      - name: 🚀 changelog
        run: |
          git config --local user.name github-actions
          git config --local user.email github-actions@github.com
          git add changelog.md
          git commit -m "🖉 Update changelog with ${GITHUB_REF#refs/*/}"
          git push
```

Important parts:

* I default the `GH_TOKEN` envvar to a same-name secret, if present
* If it's not present, I default it to `GITHUB_TOKEN`
* Checkout always uses `GH_TOKEN`, which now may be a higher-permissions PAT than the default
* I do the defaulting since the push **will** succeed if the repository doesn't use branch protection for `main` and in that case I don't want to always force the presence of a `GH_TOKEN` secret.

