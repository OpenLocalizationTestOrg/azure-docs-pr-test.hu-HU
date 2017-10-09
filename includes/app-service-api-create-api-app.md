
Hozzon létre egy [API-alkalmazás](../articles/app-service-api/app-service-api-apps-why-best-platform.md) a hello `myAppServicePlan` hello az App Service-csomag [az webalkalmazás létrehozása](/cli/azure/appservice/web#create) parancsot. 

hello webalkalmazás üzemeltetési helyet biztosít az API-t, és egy URL-cím tooview hello telepített alkalmazás biztosít.

Hello a következő parancsban cserélje le  *\<alkalmazás_neve >* egyedi névvel. Ha `<app_name>` van nem egyedi, a hibaüzenet hello hiba "A megadott nevű < alkalmazásnév > webhely már létezik." az alapértelmezett hello webalkalmazás URL-címe hello `https://<app_name>.azurewebsites.net`. 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

Hello webalkalmazás létrehozásakor hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:

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