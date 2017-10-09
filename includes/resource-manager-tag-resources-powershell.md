<span data-ttu-id="37b10-101">Hello AzureRm.Resources modul 3.0-s verziója működése címkékkel jelentős módosításokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="37b10-101">Version 3.0 of hello AzureRm.Resources module included significant changes in how you work with tags.</span></span> <span data-ttu-id="37b10-102">A továbblépés előtt ellenőrizze, milyen verziót használ:</span><span class="sxs-lookup"><span data-stu-id="37b10-102">Before you proceed, check your version:</span></span>

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

<span data-ttu-id="37b10-103">Ha az eredmények megjelenítéséhez verzió 3.0-s vagy újabb hello példákat a témakör a környezetben használható.</span><span class="sxs-lookup"><span data-stu-id="37b10-103">If your results show version 3.0 or later, hello examples in this topic work with your environment.</span></span> <span data-ttu-id="37b10-104">Ha a 3.0-nál régebbi verzióval rendelkezik, először [frissítsen egy újabb verzióra](/powershell/azureps-cmdlets-docs/) a PowerShell-galériával vagy a Webplatform-telepítővel, és csak ezt követően lépjen tovább.</span><span class="sxs-lookup"><span data-stu-id="37b10-104">If you do not have version 3.0 or later, [update your version](/powershell/azureps-cmdlets-docs/) by using PowerShell Gallery or Web Platform Installer before you proceed with this topic.</span></span>

```powershell
Version
-------
3.5.0
```

<span data-ttu-id="37b10-105">toosee hello meglévő címkék egy *erőforráscsoport*, használja:</span><span class="sxs-lookup"><span data-stu-id="37b10-105">toosee hello existing tags for a *resource group*, use:</span></span>

```powershell
(Get-AzureRmResourceGroup -Name examplegroup).Tags
```

<span data-ttu-id="37b10-106">Ez a parancsfájl hello a következő formátumban adja vissza:</span><span class="sxs-lookup"><span data-stu-id="37b10-106">That script returns hello following format:</span></span>

```powershell
Name                           Value
----                           -----
Dept                           IT
Environment                    Test
```

<span data-ttu-id="37b10-107">toosee hello meglévő címkék egy *erőforrása, amely egy megadott erőforrás-azonosító*, használja:</span><span class="sxs-lookup"><span data-stu-id="37b10-107">toosee hello existing tags for a *resource that has a specified resource ID*, use:</span></span>

```powershell
(Get-AzureRmResource -ResourceId {resource-id}).Tags
```

<span data-ttu-id="37b10-108">Vagy toosee hello meglévő címkék egy *erőforrást, amely rendelkezik a megadott névvel és erőforrás csoport*, használja:</span><span class="sxs-lookup"><span data-stu-id="37b10-108">Or, toosee hello existing tags for a *resource that has a specified name and resource group*, use:</span></span>

```powershell
(Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
```

<span data-ttu-id="37b10-109">tooget *egy adott címkét tartalmazó erőforráscsoportokat*, használja:</span><span class="sxs-lookup"><span data-stu-id="37b10-109">tooget *resource groups that have a specific tag*, use:</span></span>

```powershell
(Find-AzureRmResourceGroup -Tag @{ Dept="Finance" }).Name 
```

<span data-ttu-id="37b10-110">tooget *egy adott címkével rendelkező erőforrások*, használja:</span><span class="sxs-lookup"><span data-stu-id="37b10-110">tooget *resources that have a specific tag*, use:</span></span>

```powershell
(Find-AzureRmResource -TagName Dept -TagValue Finance).Name
```

<span data-ttu-id="37b10-111">Minden alkalommal, amikor a címkék tooa erőforrás vagy egy erőforráscsoport alkalmazásához, felülírhatja hello meglévő címkét az adott erőforrás vagy az erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="37b10-111">Every time you apply tags tooa resource or a resource group, you overwrite hello existing tags on that resource or resource group.</span></span> <span data-ttu-id="37b10-112">Ezért hello erőforrás vagy erőforráscsoport rendelkezik-e meglévő címkék alapján másik módszert kell használnia.</span><span class="sxs-lookup"><span data-stu-id="37b10-112">Therefore, you must use a different approach based on whether hello resource or resource group has existing tags.</span></span> 

<span data-ttu-id="37b10-113">tooadd címkéket tooa *erőforráscsoport meglévő címkék nélkül*, használja:</span><span class="sxs-lookup"><span data-stu-id="37b10-113">tooadd tags tooa *resource group without existing tags*, use:</span></span>

```powershell
Set-AzureRmResourceGroup -Name examplegroup -Tag @{ Dept="IT"; Environment="Test" }
```

<span data-ttu-id="37b10-114">tooadd címkéket tooa *meglévő címkékkel rendelkező erőforráscsoport*, hello meglévő címkék beolvasása, hello új címke hozzáadása, és alkalmazza újra hello címkék:</span><span class="sxs-lookup"><span data-stu-id="37b10-114">tooadd tags tooa *resource group that has existing tags*, retrieve hello existing tags, add hello new tag, and reapply hello tags:</span></span>

```powershell
$tags = (Get-AzureRmResourceGroup -Name examplegroup).Tags
$tags += @{Status="Approved"}
Set-AzureRmResourceGroup -Tag $tags -Name examplegroup
```

<span data-ttu-id="37b10-115">tooadd címkéket tooa *meglévő címkék nélkül erőforrás*, használja:</span><span class="sxs-lookup"><span data-stu-id="37b10-115">tooadd tags tooa *resource without existing tags*, use:</span></span>

```powershell
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName examplevnet -ResourceGroupName examplegroup
```

<span data-ttu-id="37b10-116">tooadd címkéket tooa *meglévő címkék erőforrást*, használja:</span><span class="sxs-lookup"><span data-stu-id="37b10-116">tooadd tags tooa *resource that has existing tags*, use:</span></span>

```powershell
$tags = (Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
$tags += @{Status="Approved"}
Set-AzureRmResource -Tag $tags -ResourceName examplevnet -ResourceGroupName examplegroup
```

<span data-ttu-id="37b10-117">minden tooapply egy erőforrás tooits erőforrások, a címkéket és *nem őriz meg a meglévő címkék hello erőforrások*, a következő parancsfájl hello használata:</span><span class="sxs-lookup"><span data-stu-id="37b10-117">tooapply all tags from a resource group tooits resources, and *not retain existing tags on hello resources*, use hello following script:</span></span>

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups) 
{
    Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force } 
}
```

<span data-ttu-id="37b10-118">minden tooapply egy erőforrás tooits erőforrások, a címkéket és *megtartja a meglévő címkéket az erőforrásokat, amelyek nincsenek ismétlődések*, a következő parancsfájl hello használata:</span><span class="sxs-lookup"><span data-stu-id="37b10-118">tooapply all tags from a resource group tooits resources, and *retain existing tags on resources that are not duplicates*, use hello following script:</span></span>

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups) 
{
    if ($g.Tags -ne $null) {
        $resources = Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName 
        foreach ($r in $resources)
        {
            $resourcetags = (Get-AzureRmResource -ResourceId $r.ResourceId).Tags
            foreach ($key in $g.Tags.Keys)
            {
                if ($resourcetags.ContainsKey($key)) { $resourcetags.Remove($key) }
            }
            $resourcetags += $g.Tags
            Set-AzureRmResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
        }
    }
}
```

<span data-ttu-id="37b10-119">tooremove található összes kódcímkének, egy üres kivonattábla át:</span><span class="sxs-lookup"><span data-stu-id="37b10-119">tooremove all tags, pass an empty hash table:</span></span>

```powershell
Set-AzureRmResourceGroup -Tag @{} -Name examplegroup
```



