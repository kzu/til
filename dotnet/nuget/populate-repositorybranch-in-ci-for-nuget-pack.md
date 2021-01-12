# Populate RepositoryBranch in CI for NuGet Pack

I learned that [RepositoryBranch](https://github.com/dotnet/sourcelink/issues/188#issuecomment-427975234) is the only NuGet Pack-related property not populated already by the .NET SDK+SourceLink. So instead of going for a full-blown \(and typically over-blown\) solution for build/assembly versioning from Git information \(such as GitInfo or GitVersion or the myriad others\), you can trivially pass in this value from your CI script with:

```text
 dotnet pack -p:RepositoryBranch=${GITHUB_REF#refs/*/}
```

This works in all OSes if you're running with a bash shell. On a Windows agent, you'd need to opt-in to that using `shell: bash` on the `run` action \(if you're using GitHub Actions\). Or you can also just [default to bash for all run actions](https://github.com/kzu/oss/blob/main/.github/workflows/build.yml#L14-L16).

NOTE: the wildcard instead of `heads` is so that this also works for tags, in which case the "branch" will be the tag name.





