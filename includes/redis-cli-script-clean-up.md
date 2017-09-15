## <a name="clean-up-deployment"></a><span data-ttu-id="26a33-101">Központi telepítés tisztítása</span><span class="sxs-lookup"><span data-stu-id="26a33-101">Clean up deployment</span></span> 

<span data-ttu-id="26a33-102">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot, Azure Redis Cache példány és a kapcsolódó erőforrások az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="26a33-102">After the script sample has been run, the follow command can be used to remove the resource group, Azure Redis Cache instance, and any related resources in the resource group.</span></span>

```azurecli
az group delete --name contosoGroup
```