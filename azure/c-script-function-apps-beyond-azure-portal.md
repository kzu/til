# C# script function apps beyond Azure portal

Creating a C# [function app in the Azure portal](https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-first-azure-function) is incredibly simple and great for playing and learning. I love the simplicity and light-ness that comes from just having a single `.csx` script file with everything you need for the function. The more "serious" approach with a "proper" [C# project, the functions SDK](https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-your-first-function-visual-studio) and the corresponding CI/CD setup seems quite the leap, in comparison.

I [recently had to move](https://twitter.com/kzu/status/1300481855565725696) a bunch of functions from one subscription to another and wanted to take the chance to improve the maintainability for some in-portal functions I had. Moving them also failed in the portal, so I looking at a non-enjoyable time copying/pasting files over. Made me question why I used in-portal functions at all.

So, while trying to save myself some time doing that, I got a zip of the existing in-portal function app using the Kudu debug console at [_https://\[APP\_NAME\].scm.azurewebsites.net/DebugConsole_](https://azdo-api.scm.azurewebsites.net/DebugConsole), and simply clicking the download icon next to the _wwwroot_ folder:

![](<../.gitbook/assets/image (8) (1).png>)

And that's where it clicked: why not just put all those files in my own [GitHub repo](https://github.com/kzu/azdo/tree/main/api) and deploy straight from there via a [subfolder](https://til.cazzulino.com/azure/publishing-function-app-from-github-folder)?

![](<../.gitbook/assets/image (7) (1).png>)

Well, that \*totally\* works, and you can keep the simplicity of the `.csx` while having a proper deployment history (and rollback capabilities) you're likely going to need sooner or later, no matter how simple the function.

I couldn't get rid of the _function.json_ file though, so placing the Functions SDK attributes on the code in the .csx doesn't work, but I'd say it's an acceptable trade-off.
