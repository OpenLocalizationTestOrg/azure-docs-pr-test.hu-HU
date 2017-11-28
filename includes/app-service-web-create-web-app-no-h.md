<span data-ttu-id="23721-101">Az [ az webapp create](/cli/azure/webapp#create) paranccsal hozzon létre egy [webalkalmazást](../articles/app-service-web/app-service-web-overview.md) a `myAppServicePlan` App Service-csomagban.</span><span class="sxs-lookup"><span data-stu-id="23721-101">Create a [web app](../articles/app-service-web/app-service-web-overview.md) in the `myAppServicePlan` App Service plan with the [az webapp create](/cli/azure/webapp#create) command.</span></span> 

<span data-ttu-id="23721-102">A webalkalmazás üzemeltetési tárterületet biztosít a kódhoz, valamint megadja az üzembe helyezett alkalmazás megtekintéséhez szükséges URL-címet.</span><span class="sxs-lookup"><span data-stu-id="23721-102">The web app provides a hosting space for your code and provides a URL to view the deployed app.</span></span>

<span data-ttu-id="23721-103">Az alábbi parancsban cserélje ki az *\<app_name>* nevet egy egyedi névre (érvényes karakterek: `a-z`, `0-9` és `-`).</span><span class="sxs-lookup"><span data-stu-id="23721-103">In the following command, replace *\<app_name>* with a unique name (valid characters are `a-z`, `0-9`, and `-`).</span></span> <span data-ttu-id="23721-104">Ha az `<app_name>` nem egyedi, a következő hibaüzenet jelenik meg: „A megadott <alkalmazás_neve> névvel már létezik webhely.”</span><span class="sxs-lookup"><span data-stu-id="23721-104">If `<app_name>` is not unique, you get the error message "Website with given name <app_name> already exists."</span></span> <span data-ttu-id="23721-105">A webalkalmazás alapértelmezett URL-címe `https://<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="23721-105">The default URL of the web app is `https://<app_name>.azurewebsites.net`.</span></span> 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

<span data-ttu-id="23721-106">A webalkalmazás létrehozása után az Azure CLI az alábbi példához hasonló információkat jelenít meg:</span><span class="sxs-lookup"><span data-stu-id="23721-106">When the web app has been created, the Azure CLI shows information similar to the following example:</span></span>

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "<app_name>.azurewebsites.net",
    "<app_name>.scm.azurewebsites.net"
  ],
  "gatewaySiteName": null,
  "hostNameSslStates": [
    {
      "hostType": "Standard",
      "name": "<app_name>.azurewebsites.net",
      "sslState": "Disabled",
      "thumbprint": null,
      "toUpdate": null,
      "virtualIp": null
    }
    < JSON data removed for brevity. >
}
```

<span data-ttu-id="23721-107">Az újonnan létrehozott webapp megtekintéséhez tallózással keresse meg a helyet.</span><span class="sxs-lookup"><span data-stu-id="23721-107">Browse to the site to see your newly created web app.</span></span>

```bash
http://<app_name>.azurewebsites.net
```
