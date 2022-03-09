# Publishing function app from GitHub folder

For very simple functions, I really liked just hacking them in the Azure portal. This is super brittle, however, since changes that break the app can't be undone. Great for learning, not so much once you start depending on those functions.

While you can deploy using [GitHub Actions](https://docs.microsoft.com/en-us/azure/azure-functions/functions-how-to-github-actions?tabs=javascript) and [Azure Pipelines](https://docs.microsoft.com/en-us/azure/azure-functions/functions-how-to-azure-devops?tabs=csharp), both seem a massive leap in complexity from the beauty of in-portal editing and updating. [Kudu](https://docs.microsoft.com/en-us/azure/app-service/deploy-continuous-deployment#option-1-kudu-app-service) (or _App Service build service_) is way simpler and generally sufficient for simple functions. You just select GitHub as the source:

![](<../.gitbook/assets/image (10) (1).png>)

And App Service build service as the build provider:

![](<../.gitbook/assets/image (6) (1).png>)

You next simply connect it to your GitHub repository and that's it. But what if you want to deploy a [subfolder](https://github.com/kzu/azdo/tree/main/redir) from the repository as the function app?

Turns out you can just add an application setting to the function app, named `DEPLOYMENT_SOURCE`, pointing to the right subfolder (i.e. `.\redir` or `.\api` in my case) and that's it! Here you can see it in action in the logs, where just the `redir` subfolder is being sync'ed to the `wwwroot` for the function app:

![](<../.gitbook/assets/image (11) (1).png>)
