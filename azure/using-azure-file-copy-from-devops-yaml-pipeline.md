---
description: >-
  I learned that it's not enough to authorize Azure Resource Manager access from
  DevOps
---

# Using Azure File Copy from DevOps yaml pipeline

Oh boy, did I waste time on this one :\(. So I had my pipeline pretty naively doing an upload to blob storage:

```text
- task: AzureFileCopy@4
  displayName: Upload Vsix
  inputs:
    SourcePath: '$(Pipeline.Workspace)\vsix\RoslynDeployment.$(Build.BuildId).vsix'
    azureSubscription: 'roslyn-Azure'
    Destination: 'AzureBlob'
    storage: 'roslyn'
    ContainerName: 'vsix'
    BlobPrefix: '$(Build.SourceBranchName)/RoslynDeployment.$(Build.BuildId).vsix'
```

I used a service principal [managed by DevOps which is the recommended approach](https://docs.microsoft.com/en-us/azure/devops/pipelines/library/connect-to-azure?view=azure-devops#create-an-azure-resource-manager-service-connection-using-automated-security). The blob storage account was under the same subscription, where the automatically created app properly showed up in IAM:

![Access control \(IAM\) pane for storage account](../.gitbook/assets/azure-storage-iam.png)

as a contributor:

![DevOps-managed app as contributor to the storage account](../.gitbook/assets/azure-storage-contributor.png)

I kept getting a 403 response when the task run, with the message `This request is not authorized to perform this operation using this permission.`

Turns out being a **Contributor** is not enough. I tried [changing guest user permissions](https://docs.microsoft.com/en-us/azure/devops/pipelines/release/azure-rm-endpoint?view=azure-devops#insufficient-privileges-to-complete-the-operation), but in the end the only thing that worked was manually adding the [Storage Blob Data Contributor role](https://github.com/MicrosoftDocs/azure-docs/issues/36454), which I found mentioned in a[ blog post](https://www.catrina.me/azcopy-403-error/). 

In the process I learned how DevOps creates the app registration and what-not, but still, not fun.

[Submitted a doc fix](https://github.com/MicrosoftDocs/azure-devops-docs/pull/8622) for the [AzureFileCopy task docs](https://docs.microsoft.com/en-us/azure/devops/pipelines/tasks/deploy/azure-file-copy-version3?view=azure-devops) so this is more easily discoverable.









