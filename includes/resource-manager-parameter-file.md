<span data-ttu-id="3ec69-101">Ha egy fájl toopass paraméter paraméterértékeket központi telepítése során használja, kell toocreate egy JSON-fájl, egy hasonló toohello formátumban, például a következő:</span><span class="sxs-lookup"><span data-stu-id="3ec69-101">If you use a parameter file toopass parameter values during deployment, you need toocreate a JSON file with a format similar toohello following example:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "webSiteName": {
            "value": "ExampleSite"
        },
        "webSiteHostingPlanName": {
            "value": "DefaultPlan"
        },
        "webSiteLocation": {
            "value": "West US"
        },
        "adminPassword": {
            "reference": {
               "keyVault": {
                  "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
               }, 
               "secretName": "sqlAdminPassword" 
            }   
        }
   }
}
```

<span data-ttu-id="3ec69-102">hello hello paraméter fájl mérete nem lehet több mint 64 KB.</span><span class="sxs-lookup"><span data-stu-id="3ec69-102">hello size of hello parameter file cannot be more than 64 KB.</span></span>

<span data-ttu-id="3ec69-103">Ha tooprovide kényes értéket a paraméterhez (például jelszót), adja hozzá az adott érték tooa kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="3ec69-103">If you need tooprovide a sensitive value for a parameter (such as a password), add that value tooa key vault.</span></span> <span data-ttu-id="3ec69-104">Hello kulcstároló beolvasása hello előző példában látható módon üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="3ec69-104">Retrieve hello key vault during deployment as shown in hello previous example.</span></span> <span data-ttu-id="3ec69-105">További információkért lásd: [biztonságos értéket átadni a telepítés során](../articles/azure-resource-manager/resource-manager-keyvault-parameter.md).</span><span class="sxs-lookup"><span data-stu-id="3ec69-105">For more information, see [Pass secure values during deployment](../articles/azure-resource-manager/resource-manager-keyvault-parameter.md).</span></span> 

