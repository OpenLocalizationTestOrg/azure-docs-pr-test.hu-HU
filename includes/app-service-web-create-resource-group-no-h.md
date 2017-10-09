<span data-ttu-id="3ad8b-101">Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="3ad8b-101">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span>

[!INCLUDE [resource group intro text](resource-group.md)]

<span data-ttu-id="3ad8b-102">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *westeurope* helyét.</span><span class="sxs-lookup"><span data-stu-id="3ad8b-102">hello following example creates a resource group named *myResourceGroup* in hello *westeurope* location.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="3ad8b-103">Általában létrehozhat az erőforrás csoport és hello erőforrások környéken régióban.</span><span class="sxs-lookup"><span data-stu-id="3ad8b-103">You generally create your resource group and hello resources in a region near you.</span></span> <span data-ttu-id="3ad8b-104">toosee minden támogatott helyek az Azure Web Apps, futtassa a hello `az appservice list-locations` parancsot.</span><span class="sxs-lookup"><span data-stu-id="3ad8b-104">toosee all supported locations for Azure Web Apps, run hello `az appservice list-locations` command.</span></span> 
