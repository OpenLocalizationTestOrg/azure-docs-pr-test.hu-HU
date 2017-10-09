<span data-ttu-id="f0809-101">telepítés közben egy erőforrás tootag hozzáadása hello `tags` elem toohello erőforrás telepít.</span><span class="sxs-lookup"><span data-stu-id="f0809-101">tootag a resource during deployment, add hello `tags` element toohello resource you are deploying.</span></span> <span data-ttu-id="f0809-102">Adja meg a hello címke nevét és értékét.</span><span class="sxs-lookup"><span data-stu-id="f0809-102">Provide hello tag name and value.</span></span>

### <a name="apply-a-literal-value-toohello-tag-name"></a><span data-ttu-id="f0809-103">Alkalmazza a literálérték toohello címke nevét</span><span class="sxs-lookup"><span data-stu-id="f0809-103">Apply a literal value toohello tag name</span></span>
<span data-ttu-id="f0809-104">hello alábbi példa bemutatja a tárfiók két címkét (`Dept` és `Environment`) tooliteral értékek meg:</span><span class="sxs-lookup"><span data-stu-id="f0809-104">hello following example shows a storage account with two tags (`Dept` and `Environment`) that are set tooliteral values:</span></span>

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

### <a name="apply-an-object-toohello-tag-element"></a><span data-ttu-id="f0809-105">Egy objektum toohello címke elem alkalmazása</span><span class="sxs-lookup"><span data-stu-id="f0809-105">Apply an object toohello tag element</span></span>
<span data-ttu-id="f0809-106">Határozzon meg egy objektum paramétert, amely több címkék tárolja, és az adott objektum toohello címke elemet.</span><span class="sxs-lookup"><span data-stu-id="f0809-106">You can define an object parameter that stores several tags, and apply that object toohello tag element.</span></span> <span data-ttu-id="f0809-107">Hello objektum minden egyes tulajdonság egy külön címkét hello erőforrás lesz.</span><span class="sxs-lookup"><span data-stu-id="f0809-107">Each property in hello object becomes a separate tag for hello resource.</span></span> <span data-ttu-id="f0809-108">hello alábbi példa paraméterének nevű `tagValues` , amely alkalmazott toohello címke elem..</span><span class="sxs-lookup"><span data-stu-id="f0809-108">hello following example has a parameter named `tagValues` that is applied toohello tag element.</span></span>

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

### <a name="apply-a-json-string-toohello-tag-name"></a><span data-ttu-id="f0809-109">Alkalmazza a JSON karakterlánc toohello címke nevét</span><span class="sxs-lookup"><span data-stu-id="f0809-109">Apply a JSON string toohello tag name</span></span>

<span data-ttu-id="f0809-110">toostore sok egyetlen címkében alakul hello értéket jelölő JSON karakterláncnak.</span><span class="sxs-lookup"><span data-stu-id="f0809-110">toostore many values in a single tag, apply a JSON string that represents hello values.</span></span> <span data-ttu-id="f0809-111">hello teljes JSON-karakterláncban tárolja egy címke, amely nem lehet hosszabb 256 karakternél.</span><span class="sxs-lookup"><span data-stu-id="f0809-111">hello entire JSON string is stored as one tag that cannot exceed 256 characters.</span></span> <span data-ttu-id="f0809-112">hello alábbi példa egy egyetlen címkét tartalmaz nevű `CostCenter` JSON karakterláncból több értéket tartalmaz, amelyek:</span><span class="sxs-lookup"><span data-stu-id="f0809-112">hello following example has a single tag named `CostCenter` that contains several values from a JSON string:</span></span>  

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