## <a name="clean-up-deployment"></a><span data-ttu-id="0c8f9-101">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="0c8f9-101">Clean up deployment</span></span>

<span data-ttu-id="0c8f9-102">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="0c8f9-102">After the script sample has been run, the follow command can be used to remove the resource group and all resources associated with it.</span></span>

```azurecli
az group delete --name myResourceGroup
```