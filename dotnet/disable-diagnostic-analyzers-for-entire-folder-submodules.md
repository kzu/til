# Disable diagnostic analyzers for entire folder/submodules

When you have submodules in your repository, it's not uncommon for those to be subject to a different quality bar than your main project. You may not even have a say in fixing issues upstream, or conventions may be totally different even. 

In this case, you can introduce an `.editorconfig` in your repository which turns off all analyzer diagnostics for an entire directory path \(and all subdirectories\) by leveraging wildcards. Say your submodules are in a folder `ext` a couple levels up from your `.editorconfig`:

```text
[../../ext/**/*.cs]
dotnet_analyzer_diagnostic.severity = none
```

Learned from [this commit](https://github.com/AArnott/Nerdbank.Streams/commit/50e322a90903283191ccaf67a8f27cf5aba6784c) after following it from a related [roslyn issue](https://github.com/dotnet/roslyn/issues/42762).

