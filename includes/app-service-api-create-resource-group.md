<span data-ttu-id="3aca1-101">Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="3aca1-101">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span>

[!INCLUDE [resource group intro text](resource-group.md)]

<span data-ttu-id="3aca1-102">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *westeurope* helyét.</span><span class="sxs-lookup"><span data-stu-id="3aca1-102">hello following example creates a resource group named *myResourceGroup* in hello *westeurope* location.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="3aca1-103">toosee hello elérhető helyről, futtassa a hello `az appservice list-locations` parancsot.</span><span class="sxs-lookup"><span data-stu-id="3aca1-103">toosee hello available locations, run hello `az appservice list-locations` command.</span></span> <span data-ttu-id="3aca1-104">Az erőforrásokat általában a közelében található régiókban hozhatja létre.</span><span class="sxs-lookup"><span data-stu-id="3aca1-104">You generally create resources in a region near you.</span></span>
