## <a name="clean-up-deployment"></a><span data-ttu-id="ddc2d-101">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="ddc2d-101">Clean up deployment</span></span>

<span data-ttu-id="ddc2d-102">Hello parancsfájl minta futtatása után a hello kövesse parancs lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="ddc2d-102">After hello script sample has been run, hello follow command can be used tooremove hello resource group and all resources associated with it.</span></span>

```azurecli
az group delete --name myResourceGroup
```