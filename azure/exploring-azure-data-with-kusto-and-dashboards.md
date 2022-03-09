# Exploring Azure Data with Kusto and Dashboards

In order to more effectively learn [Kusto](https://docs.microsoft.com/en-us/azure/azure-monitor/log-query/query-language) (the query language powering Azure analytics, log querying and PowerBI) and data visualization capabilites in Azure, I did the following:

1. [Created a cluster and database](https://docs.microsoft.com/en-us/azure/data-explorer/create-cluster-database-portal) (this takes quite a while)
2. Open the [Azure Data Explorer](https://aka.ms/kwe) (a.k.a. Kusto Web Explorer) at [https://aka.ms/kwe](https://aka.ms/kwe)
3. Add the cluster with the full URI or alternatively just the `name.region` parts (i.e. `kzukusto.centralus` vs [`https://kzukusto.centralus.kusto.windows.net`](https://kzukusto.centralus.kusto.windows.net))
4. Optionally add an arbitrary Application Insights app as a "virtual cluster", [using a url with the format](https://docs.microsoft.com/en-us/azure/data-explorer/query-monitor-data#connect-to-the-proxy) `https://ade.applicationinsights.io/subscriptions/<id>/resourcegroups/<name>/providers/microsoft.insights/components/<ai-app-name>`
5.  Right--lick database and select Ingest new data

    ![Ingest new data context menu](<../.gitbook/assets/image (12).png>)
6.  Find some interesting [Azure Open Dataset from the catalog](https://azure.microsoft.com/en-us/services/open-datasets/catalog/) that has an Azure storage URL readily available from the Azure Open Datasets catalog, such as the [Bing COVID-19 Data](https://azure.microsoft.com/en-us/services/open-datasets/catalog/bing-covid-19-data/) (I used the `.jsonl` link). NOTE: the `.json` one will not properly infer schema because it has a root object of type array. The `.jsonl` is actually a "JSON fragment" (if that even exists, would be the equivalent of an XML fragment) where each entry is just a JSON entry/line in the file.

    ![](<../.gitbook/assets/image (1) (1).png>)

    The JSON version is preferable to `.csv` because it properly infer the data type for columns.
7. Click on `Dashboards` for the new stuff here. Parameters driven by queries are particularly handy:

![Query-driven multi-select parameter for widget](<../.gitbook/assets/image (2) (1).png>)

While editing the query/widget, if the parameter is used in the query, you can interactively change its value to explore the visualizations. For example:

![Used parameter becomes enabled for selection](<../.gitbook/assets/image (3) (1).png>)

Whereas if it wasn't used:

![Unused parameter unavailable](<../.gitbook/assets/image (4) (1).png>)

After shaping the `Results`the way you want, clicking the Visual tab allows configuring a bunch of visualizations. Inference works quite nicely if the data/results are filtered to just what you want to display.

![Many chart options and inference that works great](<../.gitbook/assets/image (5) (1).png>)
