Hozzon létre egy App Service-csomag hello [az App Service-csomagot hozzon létre](/cli/azure/appservice/plan#create) parancsot.

[!INCLUDE [app-service-plan](app-service-plan.md)]

hello alábbi példakód létrehozza az App Service-csomag nevű `myAppServicePlan` a hello **szabad** IP-címek:

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

Hello App Service-csomag létrehozásakor a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "West Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "West Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  < JSON data removed for brevity. >
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
} 
```