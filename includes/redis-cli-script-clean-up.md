## <a name="clean-up-deployment"></a><span data-ttu-id="110f3-101">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="110f3-101">Clean up deployment</span></span> 

<span data-ttu-id="110f3-102">Hello parancsfájl minta futtatása után a hello kövesse parancs lehet használt tooremove hello erőforráscsoport, Azure Redis Cache példány és a kapcsolódó erőforrások hello erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="110f3-102">After hello script sample has been run, hello follow command can be used tooremove hello resource group, Azure Redis Cache instance, and any related resources in hello resource group.</span></span>

```azurecli
az group delete --name contosoGroup
```