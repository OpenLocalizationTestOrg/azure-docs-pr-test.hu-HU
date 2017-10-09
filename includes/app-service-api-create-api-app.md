
<span data-ttu-id="f1891-101">Hozzon létre egy [API-alkalmazás](../articles/app-service-api/app-service-api-apps-why-best-platform.md) a hello `myAppServicePlan` hello az App Service-csomag [az webalkalmazás létrehozása](/cli/azure/appservice/web#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="f1891-101">Create a [API app](../articles/app-service-api/app-service-api-apps-why-best-platform.md) in hello `myAppServicePlan` App Service plan with hello [az webapp create](/cli/azure/appservice/web#create) command.</span></span> 

<span data-ttu-id="f1891-102">hello webalkalmazás üzemeltetési helyet biztosít az API-t, és egy URL-cím tooview hello telepített alkalmazás biztosít.</span><span class="sxs-lookup"><span data-stu-id="f1891-102">hello web app provides a hosting space for your API and provides a URL tooview hello deployed app.</span></span>

<span data-ttu-id="f1891-103">Hello a következő parancsban cserélje le  *\<alkalmazás_neve >* egyedi névvel.</span><span class="sxs-lookup"><span data-stu-id="f1891-103">In hello following command, replace *\<app_name>* with a unique name.</span></span> <span data-ttu-id="f1891-104">Ha `<app_name>` van nem egyedi, a hibaüzenet hello hiba "A megadott nevű < alkalmazásnév > webhely már létezik."</span><span class="sxs-lookup"><span data-stu-id="f1891-104">If `<app_name>` is not unique, you get hello error message "Website with given name <app_name> already exists."</span></span> <span data-ttu-id="f1891-105">az alapértelmezett hello webalkalmazás URL-címe hello `https://<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="f1891-105">hello default URL of hello web app is `https://<app_name>.azurewebsites.net`.</span></span> 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

<span data-ttu-id="f1891-106">Hello webalkalmazás létrehozásakor hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="f1891-106">When hello web app has been created, hello Azure CLI shows information similar toohello following example:</span></span>

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