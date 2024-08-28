# create new .NET 8 App Service and deploy to Azure + install APM via Azure Portal UI

```bash
# create new webapi project
dotnet new webapi

# use VSCode extension to deploy to Azure App Service
# make sure you can send request to it

curl https://azurefuncdemoappservice.azurewebsites.net/weatherforecast

# Go to the App Service in Azure Portal, select Development Tools > Extensions
# search for 'New Relic .NET Agent and install it

# Go to Settings > Environment variables and add 2 variables and restart the service
NEW_RELIC_APP_NAME = App Service Demo
NEW_RELIC_LICENSE_KEY = <Your license key>

# send some request to https://azurefuncdemoappservice.azurewebsites.net/weatherforecast and you should see data in New Relic after few minutes
```

# call the Azure Function which is deployed to AKS
```csharp
..
// call the Azure Func
app.MapGet("/azurefunc", async () =>
{
    var httpClient = new HttpClient();

    // call the child service
    var response = await httpClient.GetAsync($"http://20.11.83.209/api/HttpExampleParent");
    var rsp = await response.Content.ReadAsStringAsync();
    var finalString = $"From /api/HttpExampleParent:\n\t'{rsp}'";
    return finalString;
})
.WithName("azurefunc")
.WithOpenApi();
```
- redeploy the app and send request to https://azurefuncdemoappservice.azurewebsites.net/azurefunc