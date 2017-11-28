<span data-ttu-id="8e655-101">Hozzon létre egy erőforráscsoportot az [az group create](/cli/azure/group#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="8e655-101">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span>

[!INCLUDE [resource group intro text](resource-group.md)]

<span data-ttu-id="8e655-102">A következő példában létrehozunk egy *myResourceGroup* nevű erőforráscsoportot a *westeurope* helyen.</span><span class="sxs-lookup"><span data-stu-id="8e655-102">The following example creates a resource group named *myResourceGroup* in the *westeurope* location.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="8e655-103">Az erőforráscsoportot és az erőforrásokat általában a közelében található régiókban hozhatja létre.</span><span class="sxs-lookup"><span data-stu-id="8e655-103">You generally create your resource group and the resources in a region near you.</span></span> <span data-ttu-id="8e655-104">Az összes Azure Web Apps-t támogató hely megtekintéséhez futtassa az `az appservice list-locations` parancsot.</span><span class="sxs-lookup"><span data-stu-id="8e655-104">To see all supported locations for Azure Web Apps, run the `az appservice list-locations` command.</span></span> 