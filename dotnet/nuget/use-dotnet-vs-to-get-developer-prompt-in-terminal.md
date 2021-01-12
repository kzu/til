# How to add search to static nuget feed

I have been using [static serverless nuget](https://www.cazzulino.com/serverless-nuget-feed.html) feeds for a while now. [Sleet](https://github.com/emgarten/Sleet) is totally awesome. One missing piece was search: searching a static feed would just return everything, always. [Not anymore](https://github.com/emgarten/Sleet/pull/142)!

To set it up:

1. Fork [Sleet.Search](https://github.com/emgarten/Sleet.Search): this is the Azure function project that will perform the search on your static feeds. You'll host your own in a consumption \(aka serverless\) plan in Azure.
2. Create an Azure Functions app and follow the [deployment steps from my blog](https://www.cazzulino.com/minimalist-shortlinks.html#deployment): that's basically setting up the simplest CI/CD for a GH repo.
3. By default, the project allows passing in the static feed "search" url \(which is a blob named under `/search/query` in your sleet blob container\) and requires function authentication. In my case, I wanted a simpler search URL and anonymous access, so I [made that change to Sleet.Search](https://github.com/kzu/Sleet.Search/commit/99c736b). Basically the function can now be accessed at `/query`, without URL-encoding another URL parameter there.
4. Follow instructions to [enable search for your sleet feed via settings](https://github.com/emgarten/Sleet/blob/master/doc/external-search.md). 

That's it. Now you can configure my feed https://pkg.kzu.io/index.json and it will be fully searchable. Plus, I used [Azure Functions Proxies](https://docs.microsoft.com/en-us/azure/azure-functions/functions-proxies) to make the blob storage URL that much nicer. I even made the query URL nice, since it was trivial too: https://pkg.kzu.io/search 😍.

