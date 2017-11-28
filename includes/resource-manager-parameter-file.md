<span data-ttu-id="35aa6-101">A paraméterfájl használata paraméterértékek felelt meg a telepítés során, ha szüksége hozzon létre egy JSON-fájl formátuma nem a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="35aa6-101">If you use a parameter file to pass parameter values during deployment, you need to create a JSON file with a format similar to the following example:</span></span>

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

<span data-ttu-id="35aa6-102">A paraméter-fájl mérete nem lehet több mint 64 KB.</span><span class="sxs-lookup"><span data-stu-id="35aa6-102">The size of the parameter file cannot be more than 64 KB.</span></span>

<span data-ttu-id="35aa6-103">Meg kell adnia a kényes értéket a paraméterhez (például jelszót), ha hozzá kulcstároló ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="35aa6-103">If you need to provide a sensitive value for a parameter (such as a password), add that value to a key vault.</span></span> <span data-ttu-id="35aa6-104">Olvashatók be a key vault üzembe helyezése során, mert az előző példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="35aa6-104">Retrieve the key vault during deployment as shown in the previous example.</span></span> <span data-ttu-id="35aa6-105">További információkért lásd: [biztonságos értéket átadni a telepítés során](../articles/azure-resource-manager/resource-manager-keyvault-parameter.md).</span><span class="sxs-lookup"><span data-stu-id="35aa6-105">For more information, see [Pass secure values during deployment](../articles/azure-resource-manager/resource-manager-keyvault-parameter.md).</span></span> 

