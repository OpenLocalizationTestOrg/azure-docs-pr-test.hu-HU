telepítés közben egy erőforrás tootag hozzáadása hello `tags` elem toohello erőforrás telepít. Adja meg a hello címke nevét és értékét.

### <a name="apply-a-literal-value-toohello-tag-name"></a>Alkalmazza a literálérték toohello címke nevét
hello alábbi példa bemutatja a tárfiók két címkét (`Dept` és `Environment`) tooliteral értékek meg:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

### <a name="apply-an-object-toohello-tag-element"></a>Egy objektum toohello címke elem alkalmazása
Határozzon meg egy objektum paramétert, amely több címkék tárolja, és az adott objektum toohello címke elemet. Hello objektum minden egyes tulajdonság egy külön címkét hello erőforrás lesz. hello alábbi példa paraméterének nevű `tagValues` , amely alkalmazott toohello címke elem..

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "tagValues": {
      "type": "object",
      "defaultValue": {
        "Dept": "Finance",
        "Environment": "Production"
      }
    }
  },
  "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "tags": "[parameters('tagValues')]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": {}
    }
  ]
}
```

### <a name="apply-a-json-string-toohello-tag-name"></a>Alkalmazza a JSON karakterlánc toohello címke nevét

toostore sok egyetlen címkében alakul hello értéket jelölő JSON karakterláncnak. hello teljes JSON-karakterláncban tárolja egy címke, amely nem lehet hosszabb 256 karakternél. hello alábbi példa egy egyetlen címkét tartalmaz nevű `CostCenter` JSON karakterláncból több értéket tartalmaz, amelyek:  

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "tags": {
        "CostCenter": "{\"Dept\":\"Finance\",\"Environment\":\"Production\"}"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```