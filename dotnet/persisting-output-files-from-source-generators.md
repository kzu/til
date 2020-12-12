# Persisting output files from source generators

Some time back, I had my own I/O code that [based on some MSBuild property would persist my generated source](https://github.com/kzu/ThisAssembly/commit/65d43b2f6) for troubleshooting. This is no longer needed since you can now set 

```markup
<EmitCompilerGeneratedFiles>true</EmitCompilerGeneratedFiles>
```

That will emit the sources to the `$(IntermediateOutputPath)/generated/[GeneratorAssembly]/[GeneratorTypeFullName]` folder by default. If you want to also change where the generated sources are placed, you can additionally set the `CompilerGeneratedFilesOutputPath` property.



