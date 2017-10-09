<span data-ttu-id="e8620-101">egy címke tooa erőforráscsoportot tooadd használja **azure védelmicsoport-készlet**.</span><span class="sxs-lookup"><span data-stu-id="e8620-101">tooadd a tag tooa resource group, use **azure group set**.</span></span> <span data-ttu-id="e8620-102">Ha hello erőforráscsoport nem rendelkezik meglévő címkék, adjon át hello címke.</span><span class="sxs-lookup"><span data-stu-id="e8620-102">If hello resource group does not have any existing tags, pass in hello tag.</span></span>

```azurecli
azure group set -n tag-demo-group -t Dept=Finance
```

<span data-ttu-id="e8620-103">Címkék egész frissülnek.</span><span class="sxs-lookup"><span data-stu-id="e8620-103">Tags are updated as a whole.</span></span> <span data-ttu-id="e8620-104">Ha azt szeretné, hogy a címke tooa erőforráscsoport, amelynek létező címkék tooadd, adja át az összes hello címkét.</span><span class="sxs-lookup"><span data-stu-id="e8620-104">If you want tooadd a tag tooa resource group that has existing tags, pass all hello tags.</span></span> 

```azurecli
azure group set -n tag-demo-group -t Dept=Finance;Environment=Production;Project=Upgrade
```

<span data-ttu-id="e8620-105">Címkék nem örökli erőforrás egy erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="e8620-105">Tags are not inherited by resources in a resource group.</span></span> <span data-ttu-id="e8620-106">egy címke tooa erőforrás tooadd használja **azure erőforráskészlethez**.</span><span class="sxs-lookup"><span data-stu-id="e8620-106">tooadd a tag tooa resource, use **azure resource set**.</span></span> <span data-ttu-id="e8620-107">Hello hello erőforrástípus hello címkét felvenni kívánt API verziószámát adja át.</span><span class="sxs-lookup"><span data-stu-id="e8620-107">Pass hello API version number for hello resource type that you are adding hello tag to.</span></span> <span data-ttu-id="e8620-108">Tooretrieve hello API verziójára van szükség, ha használja a következő parancs az erőforrás-szolgáltató hello hello típus állítja hello:</span><span class="sxs-lookup"><span data-stu-id="e8620-108">If you need tooretrieve hello API version, use hello following command with hello resource provider for hello type you are setting:</span></span>

```azurecli
azure provider show -n Microsoft.Storage --json
```

<span data-ttu-id="e8620-109">Keresse meg szeretné hello erőforrástípus hello eredmény elérése érdekében.</span><span class="sxs-lookup"><span data-stu-id="e8620-109">In hello results, look for hello resource type you want.</span></span>

```azurecli
"resourceTypes": [
{
  "resourceType": "storageAccounts",
  ...
  "apiVersions": [
    "2016-01-01",
    "2015-06-15",
    "2015-05-01-preview"
  ]
}
...
```

<span data-ttu-id="e8620-110">Most, adja meg az adott API-verzió, az erőforráscsoport neve, erőforrás nevét, erőforrástípus és címke paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="e8620-110">Now, provide that API version, resource group name, resource name, resource type, and tag value as parameters.</span></span>

```azurecli
azure resource set -g tag-demo-group -n storagetagdemo -r Microsoft.Storage/storageAccounts -t Dept=Finance -o 2016-01-01
```

<span data-ttu-id="e8620-111">Címkék közvetlenül erőforrások és csoportok léteznek.</span><span class="sxs-lookup"><span data-stu-id="e8620-111">Tags exist directly on resources and resource groups.</span></span> <span data-ttu-id="e8620-112">toosee hello meglévő címkéket, az erőforráscsoportot és az erőforrások lekérése **azure-csoportok megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="e8620-112">toosee hello existing tags, get a resource group and its resources with **azure group show**.</span></span>

```azurecli
azure group show -n tag-demo-group --json
```

<span data-ttu-id="e8620-113">Amely hello erőforráscsoport, beleértve bármilyen alkalmazott címkék tooit kapcsolatos metaadatokat adja vissza.</span><span class="sxs-lookup"><span data-stu-id="e8620-113">Which returns metadata about hello resource group, including any tags applied tooit.</span></span>

```azurecli
{
  "id": "/subscriptions/4705409c-9372-42f0-914c-64a504530837/resourceGroups/tag-demo-group",
  "name": "tag-demo-group",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "location": "southcentralus",
  "tags": {
    "Dept": "Finance",
    "Environment": "Production",
    "Project": "Upgrade"
  },
  ...
}
```

<span data-ttu-id="e8620-114">Megtekintheti egy adott erőforráshoz hello címkék használatával **azure-erőforrás megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="e8620-114">You view hello tags for a particular resource by using **azure resource show**.</span></span>

```azurecli
azure resource show -g tag-demo-group -n storagetagdemo -r Microsoft.Storage/storageAccounts -o 2016-01-01 --json
```

<span data-ttu-id="e8620-115">tooretrieve címke értékű, minden hello erőforrás használata:</span><span class="sxs-lookup"><span data-stu-id="e8620-115">tooretrieve all hello resources with a tag value, use:</span></span>

```azurecli
azure resource list -t Dept=Finance --json
```

<span data-ttu-id="e8620-116">tooretrieve címke értékű, minden hello erőforráscsoportok használja:</span><span class="sxs-lookup"><span data-stu-id="e8620-116">tooretrieve all hello resource groups with a tag value, use:</span></span>

```azurecli
azure group list -t Dept=Finance
```

<span data-ttu-id="e8620-117">A parancs a következő hello hello meglévő címkék az előfizetésében tekintheti meg:</span><span class="sxs-lookup"><span data-stu-id="e8620-117">You can view hello existing tags in your subscription with hello following command:</span></span>

```azurecli
azure tag list
```
