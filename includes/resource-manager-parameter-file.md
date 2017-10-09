Ha egy fájl toopass paraméter paraméterértékeket központi telepítése során használja, kell toocreate egy JSON-fájl, egy hasonló toohello formátumban, például a következő:

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

hello hello paraméter fájl mérete nem lehet több mint 64 KB.

Ha tooprovide kényes értéket a paraméterhez (például jelszót), adja hozzá az adott érték tooa kulcstároló. Hello kulcstároló beolvasása hello előző példában látható módon üzembe helyezése során. További információkért lásd: [biztonságos értéket átadni a telepítés során](../articles/azure-resource-manager/resource-manager-keyvault-parameter.md). 

