# How to launch multiple Azure Functions apps on different ports

Turns out setting up `Hosts.LocalHostPort` via local.settings.json doesn't work \(consistently at least, I keep getting semi-random _Value cannot be null. \(Parameter 'provider'\)_ errors on function startup\), [contrary to what the documentation says](https://docs.microsoft.com/en-us/azure/azure-functions/functions-run-local?tabs=windows%2Ccsharp%2Cbash#local-settings-file). If you override the command line args directly, via a `launchSettings.json` , it works consistently and I even get faster startup ¯\_ \(ツ\)\_/¯:

```javascript
{
  "profiles": {
    "api": {
      "commandName": "Project",
      "commandLineArgs": "start --port 7072"
    }
  }
}
```

You might want to add a `--pause-on-error` in there too.

