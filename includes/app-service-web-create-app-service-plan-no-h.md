<span data-ttu-id="1f67b-101">Hozzon létre egy App Service-csomag hello [az App Service-csomagot hozzon létre](/cli/azure/appservice/plan#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="1f67b-101">Create an App Service plan with hello [az appservice plan create](/cli/azure/appservice/plan#create) command.</span></span>

[!INCLUDE [app-service-plan](app-service-plan.md)]

<span data-ttu-id="1f67b-102">hello alábbi példakód létrehozza az App Service-csomag nevű `myAppServicePlan` a hello **szabad** IP-címek:</span><span class="sxs-lookup"><span data-stu-id="1f67b-102">hello following example creates an App Service plan named `myAppServicePlan` in hello **Free** pricing tier:</span></span>

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

<span data-ttu-id="1f67b-103">Hello App Service-csomag létrehozásakor a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="1f67b-103">When hello App Service plan has been created, hello Azure CLI shows information similar toohello following example:</span></span>

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