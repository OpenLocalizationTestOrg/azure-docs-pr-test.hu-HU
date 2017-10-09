<span data-ttu-id="2e6ee-101">Hozzon létre egy [webalkalmazás](../articles/app-service-web/app-service-web-overview.md) a hello `myAppServicePlan` hello az App Service-csomag [az webalkalmazás létrehozása](/cli/azure/webapp#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="2e6ee-101">Create a [web app](../articles/app-service-web/app-service-web-overview.md) in hello `myAppServicePlan` App Service plan with hello [az webapp create](/cli/azure/webapp#create) command.</span></span> 

<span data-ttu-id="2e6ee-102">hello web app üzemeltetési helyet biztosít a kódot, és egy URL-cím tooview hello telepített alkalmazás biztosítja.</span><span class="sxs-lookup"><span data-stu-id="2e6ee-102">hello web app provides a hosting space for your code and provides a URL tooview hello deployed app.</span></span>

<span data-ttu-id="2e6ee-103">Hello a következő parancsban cserélje le  *\<alkalmazás_neve >* egyedi névvel (érvényes karakterek: `a-z`, `0-9`, és `-`).</span><span class="sxs-lookup"><span data-stu-id="2e6ee-103">In hello following command, replace *\<app_name>* with a unique name (valid characters are `a-z`, `0-9`, and `-`).</span></span> <span data-ttu-id="2e6ee-104">Ha `<app_name>` van nem egyedi, a hibaüzenet hello hiba "A megadott nevű < alkalmazásnév > webhely már létezik."</span><span class="sxs-lookup"><span data-stu-id="2e6ee-104">If `<app_name>` is not unique, you get hello error message "Website with given name <app_name> already exists."</span></span> <span data-ttu-id="2e6ee-105">az alapértelmezett hello webalkalmazás URL-címe hello `https://<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="2e6ee-105">hello default URL of hello web app is `https://<app_name>.azurewebsites.net`.</span></span> 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

<span data-ttu-id="2e6ee-106">Hello webalkalmazás létrehozásakor hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="2e6ee-106">When hello web app has been created, hello Azure CLI shows information similar toohello following example:</span></span>

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

<span data-ttu-id="2e6ee-107">Keresse meg az újonnan létrehozott webalkalmazás toohello hely toosee.</span><span class="sxs-lookup"><span data-stu-id="2e6ee-107">Browse toohello site toosee your newly created web app.</span></span>

```bash
http://<app_name>.azurewebsites.net
```
