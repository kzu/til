# Detect CI builds for every CI system

In order to unify the environment variable to detect whether a build is a CI build as simply `CI` \(as is done already in [GitHub Actions](https://docs.github.com/en/free-pro-team@latest/actions/reference/environment-variables), [Circle CI](https://circleci.com/docs/2.0/env-vars/#built-in-environment-variables) and [GitLab](https://docs.gitlab.com/ee/ci/variables/predefined_variables.html)\), you just place this bit of MSBuild in every project's `Directory.Build.props` to make things consistent everywhere:

```markup
<PropertyGroup Label="CI" Condition="'$(CI)' == ''">
  <CI>false</CI>
  <!-- GH, CircleCI, GitLab and BitBucket already use CI -->
  <CI Condition="'$(TF_BUILD)' == 'true' or 
                 '$(TEAMCITY_VERSION)' != '' or 
                 '$(APPVEYOR)' != '' or 
                 '$(BuildRunner)' == 'MyGet' or 
                 '$(JENKINS_URL)' != '' or 
                 '$(TRAVIS)' == 'true' or 
                 '$(BUDDY)' == 'true' or
                 '$(CODEBUILD_CI)' == 'true'">true</CI>
</PropertyGroup>
```



