<span data-ttu-id="3e5bb-101">Ha üzembe helyezés közben szeretne címkével ellátni egy erőforrást, adja hozzá a `tags` elemet ahhoz az erőforráshoz, amelyet éppen üzembe helyez.</span><span class="sxs-lookup"><span data-stu-id="3e5bb-101">To tag a resource during deployment, add the `tags` element to the resource you are deploying.</span></span> <span data-ttu-id="3e5bb-102">Adja meg a címke nevét és értékét.</span><span class="sxs-lookup"><span data-stu-id="3e5bb-102">Provide the tag name and value.</span></span>

### <a name="apply-a-literal-value-to-the-tag-name"></a><span data-ttu-id="3e5bb-103">Szövegkonstansérték alkalmazása a címkenévre</span><span class="sxs-lookup"><span data-stu-id="3e5bb-103">Apply a literal value to the tag name</span></span>
<span data-ttu-id="3e5bb-104">Az alábbi példában egy tárfiók látható két címkével (`Dept` és `Environment`), amelyek szövegkonstansértékre vannak beállítva:</span><span class="sxs-lookup"><span data-stu-id="3e5bb-104">The following example shows a storage account with two tags (`Dept` and `Environment`) that are set to literal values:</span></span>

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

### <a name="apply-an-object-to-the-tag-element"></a><span data-ttu-id="3e5bb-105">Objektum alkalmazása a címkeelemre</span><span class="sxs-lookup"><span data-stu-id="3e5bb-105">Apply an object to the tag element</span></span>
<span data-ttu-id="3e5bb-106">Megadhat olyan objektumparamétert, amely több címkét tartalmaz, majd alkalmazhatja azt az objektumot a címkeelemre.</span><span class="sxs-lookup"><span data-stu-id="3e5bb-106">You can define an object parameter that stores several tags, and apply that object to the tag element.</span></span> <span data-ttu-id="3e5bb-107">Az objektum minden tulajdonsága az erőforrás külön címkéjévé válik.</span><span class="sxs-lookup"><span data-stu-id="3e5bb-107">Each property in the object becomes a separate tag for the resource.</span></span> <span data-ttu-id="3e5bb-108">Az alábbi példa egy `tagValues` nevű paramétert tartalmaz, amely a címkeelemre van alkalmazva.</span><span class="sxs-lookup"><span data-stu-id="3e5bb-108">The following example has a parameter named `tagValues` that is applied to the tag element.</span></span>

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

### <a name="apply-a-json-string-to-the-tag-name"></a><span data-ttu-id="3e5bb-109">JSON-karakterlánc alkalmazása a címkenévre</span><span class="sxs-lookup"><span data-stu-id="3e5bb-109">Apply a JSON string to the tag name</span></span>

<span data-ttu-id="3e5bb-110">Ha több értéket szeretne tárolni egyetlen címkében, alkalmazzon a megfelelő értékeket képviselő JSON-karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="3e5bb-110">To store many values in a single tag, apply a JSON string that represents the values.</span></span> <span data-ttu-id="3e5bb-111">A teljes JSON-karakterlánc egyetlen címkeként tárolódik, amelynek hossza nem lépheti túl a 256 karaktert.</span><span class="sxs-lookup"><span data-stu-id="3e5bb-111">The entire JSON string is stored as one tag that cannot exceed 256 characters.</span></span> <span data-ttu-id="3e5bb-112">Az alábbi példában egy `CostCenter` nevű címke szerepel, amely egy JSON-karakterlánc számos értékét tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="3e5bb-112">The following example has a single tag named `CostCenter` that contains several values from a JSON string:</span></span>  

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