<span data-ttu-id="024d3-101">Egy címkét adhat hozzá egy erőforráscsoportba **azure védelmicsoport-készlet**.</span><span class="sxs-lookup"><span data-stu-id="024d3-101">To add a tag to a resource group, use **azure group set**.</span></span> <span data-ttu-id="024d3-102">Ha az erőforráscsoport nem rendelkezik meglévő címkék, továbbítja a kódban.</span><span class="sxs-lookup"><span data-stu-id="024d3-102">If the resource group does not have any existing tags, pass in the tag.</span></span>

```azurecli
azure group set -n tag-demo-group -t Dept=Finance
```

<span data-ttu-id="024d3-103">Címkék egész frissülnek.</span><span class="sxs-lookup"><span data-stu-id="024d3-103">Tags are updated as a whole.</span></span> <span data-ttu-id="024d3-104">Ha szeretne egy címke hozzáadása egy meglévő címkék rendelkező erőforrás csoporthoz, adja át a címkék.</span><span class="sxs-lookup"><span data-stu-id="024d3-104">If you want to add a tag to a resource group that has existing tags, pass all the tags.</span></span> 

```azurecli
azure group set -n tag-demo-group -t Dept=Finance;Environment=Production;Project=Upgrade
```

<span data-ttu-id="024d3-105">Címkék nem örökli erőforrás egy erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="024d3-105">Tags are not inherited by resources in a resource group.</span></span> <span data-ttu-id="024d3-106">Egy erőforrást egy címkét adhat hozzá, **azure erőforráskészlethez**.</span><span class="sxs-lookup"><span data-stu-id="024d3-106">To add a tag to a resource, use **azure resource set**.</span></span> <span data-ttu-id="024d3-107">Az erőforrástípus hozzáadni a címkéhez való API verziószáma át.</span><span class="sxs-lookup"><span data-stu-id="024d3-107">Pass the API version number for the resource type that you are adding the tag to.</span></span> <span data-ttu-id="024d3-108">Ha beolvasása az API-verzió van szüksége, a következő paranccsal az erőforrás-szolgáltató állítja a típushoz:</span><span class="sxs-lookup"><span data-stu-id="024d3-108">If you need to retrieve the API version, use the following command with the resource provider for the type you are setting:</span></span>

```azurecli
azure provider show -n Microsoft.Storage --json
```

<span data-ttu-id="024d3-109">Az eredményeket keresse meg a kívánt erőforrás-típus.</span><span class="sxs-lookup"><span data-stu-id="024d3-109">In the results, look for the resource type you want.</span></span>

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

<span data-ttu-id="024d3-110">Most, adja meg az adott API-verzió, az erőforráscsoport neve, erőforrás nevét, erőforrástípus és címke paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="024d3-110">Now, provide that API version, resource group name, resource name, resource type, and tag value as parameters.</span></span>

```azurecli
azure resource set -g tag-demo-group -n storagetagdemo -r Microsoft.Storage/storageAccounts -t Dept=Finance -o 2016-01-01
```

<span data-ttu-id="024d3-111">Címkék közvetlenül erőforrások és csoportok léteznek.</span><span class="sxs-lookup"><span data-stu-id="024d3-111">Tags exist directly on resources and resource groups.</span></span> <span data-ttu-id="024d3-112">A meglévő címkék megtekintéséhez beolvasása erőforráscsoport és az erőforrások **azure-csoportok megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="024d3-112">To see the existing tags, get a resource group and its resources with **azure group show**.</span></span>

```azurecli
azure group show -n tag-demo-group --json
```

<span data-ttu-id="024d3-113">Ami az erőforráscsoportot, többek között az alkalmazott címkéket kapcsolatos metaadatokat adja vissza.</span><span class="sxs-lookup"><span data-stu-id="024d3-113">Which returns metadata about the resource group, including any tags applied to it.</span></span>

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

<span data-ttu-id="024d3-114">Megtekintheti egy adott erőforráshoz címkék használatával **azure-erőforrás megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="024d3-114">You view the tags for a particular resource by using **azure resource show**.</span></span>

```azurecli
azure resource show -g tag-demo-group -n storagetagdemo -r Microsoft.Storage/storageAccounts -o 2016-01-01 --json
```

<span data-ttu-id="024d3-115">Egy címke az összes erőforrást lekéréséhez használja:</span><span class="sxs-lookup"><span data-stu-id="024d3-115">To retrieve all the resources with a tag value, use:</span></span>

```azurecli
azure resource list -t Dept=Finance --json
```

<span data-ttu-id="024d3-116">A címkeérték erőforráscsoportok lekéréséhez használja:</span><span class="sxs-lookup"><span data-stu-id="024d3-116">To retrieve all the resource groups with a tag value, use:</span></span>

```azurecli
azure group list -t Dept=Finance
```

<span data-ttu-id="024d3-117">A meglévő címkéket az előfizetés a következő paranccsal tekintheti meg:</span><span class="sxs-lookup"><span data-stu-id="024d3-117">You can view the existing tags in your subscription with the following command:</span></span>

```azurecli
azure tag list
```
