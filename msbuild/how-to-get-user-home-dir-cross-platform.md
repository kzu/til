# How to get user home dir \~ cross-platform

It would be great if you could just use `$(~)`, say, but that would be too good to be true :).&#x20;

Here's how you can get the user profile directory in both Unix-like OS (Linux/Mac) and Windows:

```markup
<PropertyGroup>
  <UserProfileHome Condition="'$([MSBuild]::IsOSUnixLike())' == 'true'">$(HOME)</UserProfileHome>
  <UserProfileHome Condition="'$([MSBuild]::IsOSUnixLike())' != 'true'">$(USERPROFILE)</UserProfileHome>
</PropertyGroup>
```
