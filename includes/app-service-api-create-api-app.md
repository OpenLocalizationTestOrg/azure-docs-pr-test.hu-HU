
<span data-ttu-id="203a5-101">Az [ az webapp create](/cli/azure/appservice/web#create) paranccsal hozzon létre egy [API-alkalmazást](../articles/app-service-api/app-service-api-apps-why-best-platform.md) az `myAppServicePlan`App Service-csomagban.</span><span class="sxs-lookup"><span data-stu-id="203a5-101">Create a [API app](../articles/app-service-api/app-service-api-apps-why-best-platform.md) in the `myAppServicePlan` App Service plan with the [az webapp create](/cli/azure/appservice/web#create) command.</span></span> 

<span data-ttu-id="203a5-102">A webalkalmazás üzemeltetési tárterületet biztosít a API-hoz, valamint megadja az üzembe helyezett alkalmazás megtekintéséhez szükséges URL-címet.</span><span class="sxs-lookup"><span data-stu-id="203a5-102">The web app provides a hosting space for your API and provides a URL to view the deployed app.</span></span>

<span data-ttu-id="203a5-103">Az alábbi parancsban cserélje le az *\<alkalmazás_neve>* elemet egy egyedi névre.</span><span class="sxs-lookup"><span data-stu-id="203a5-103">In the following command, replace *\<app_name>* with a unique name.</span></span> <span data-ttu-id="203a5-104">Ha az `<app_name>` nem egyedi, a következő hibaüzenet jelenik meg: „A megadott <alkalmazás_neve> névvel már létezik webhely.”</span><span class="sxs-lookup"><span data-stu-id="203a5-104">If `<app_name>` is not unique, you get the error message "Website with given name <app_name> already exists."</span></span> <span data-ttu-id="203a5-105">A webalkalmazás alapértelmezett URL-címe `https://<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="203a5-105">The default URL of the web app is `https://<app_name>.azurewebsites.net`.</span></span> 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

<span data-ttu-id="203a5-106">A webalkalmazás létrehozása után az Azure CLI az alábbi példához hasonló információkat jelenít meg:</span><span class="sxs-lookup"><span data-stu-id="203a5-106">When the web app has been created, the Azure CLI shows information similar to the following example:</span></span>

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