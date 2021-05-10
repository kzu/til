# How to run Azure Storage unit tests in CI

If you have tests that need to exercise Blob, Queue or Table storage from Azure Storage, you can use [Azurite](https://github.com/Azure/Azurite) \(v3\) in CI, which can be configured for GitHub Actions as follows:

```text
  - name: ⚙ azurite
    run: |
      npm install azurite
      npx azurite-table &
```

The first line will install it, and the second will start it in a background process. Tests will need to use `CloudStorageAccount.DevelopmentStorageAccount` to access the locally running instance automatically. Note that this is compatible with the older Azure Storage Emulator included with the Azure SDK with Visual Studio, so no changes are needed between local test runs and CI.

You can see this in action in the following run: [https://github.com/devlooped/TableStorage/actions/runs/829193167](https://github.com/devlooped/TableStorage/actions/runs/829193167) which is running on all supported .NET platforms via the workflow definition at [https://github.com/devlooped/TableStorage/blob/main/.github/workflows/build.yml](https://github.com/devlooped/TableStorage/blob/main/.github/workflows/build.yml).

