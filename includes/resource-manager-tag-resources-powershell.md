<span data-ttu-id="9d982-101">Az AzureRm.Resources modul 3.0-s verziója jelentős változásokat hozott a címkék használata terén.</span><span class="sxs-lookup"><span data-stu-id="9d982-101">Version 3.0 of the AzureRm.Resources module included significant changes in how you work with tags.</span></span> <span data-ttu-id="9d982-102">A továbblépés előtt ellenőrizze, milyen verziót használ:</span><span class="sxs-lookup"><span data-stu-id="9d982-102">Before you proceed, check your version:</span></span>

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

<span data-ttu-id="9d982-103">Ha 3.0-s vagy annál újabb verziót, a témakörben szereplő példák alkalmazhatók az Ön által használt környezetre.</span><span class="sxs-lookup"><span data-stu-id="9d982-103">If your results show version 3.0 or later, the examples in this topic work with your environment.</span></span> <span data-ttu-id="9d982-104">Ha a 3.0-nál régebbi verzióval rendelkezik, először [frissítsen egy újabb verzióra](/powershell/azureps-cmdlets-docs/) a PowerShell-galériával vagy a Webplatform-telepítővel, és csak ezt követően lépjen tovább.</span><span class="sxs-lookup"><span data-stu-id="9d982-104">If you do not have version 3.0 or later, [update your version](/powershell/azureps-cmdlets-docs/) by using PowerShell Gallery or Web Platform Installer before you proceed with this topic.</span></span>

```powershell
Version
-------
3.5.0
```

<span data-ttu-id="9d982-105">*Erőforráscsoportok* meglévő címkéinek megtekintéséhez használja a következőt:</span><span class="sxs-lookup"><span data-stu-id="9d982-105">To see the existing tags for a *resource group*, use:</span></span>

```powershell
(Get-AzureRmResourceGroup -Name examplegroup).Tags
```

<span data-ttu-id="9d982-106">A szkript a következő formátumot adja vissza:</span><span class="sxs-lookup"><span data-stu-id="9d982-106">That script returns the following format:</span></span>

```powershell
Name                           Value
----                           -----
Dept                           IT
Environment                    Test
```

<span data-ttu-id="9d982-107">*Megadott erőforrás-azonosítóval rendelkező erőforrás* meglévő címkéinek megtekintéséhez használja a következőt:</span><span class="sxs-lookup"><span data-stu-id="9d982-107">To see the existing tags for a *resource that has a specified resource ID*, use:</span></span>

```powershell
(Get-AzureRmResource -ResourceId {resource-id}).Tags
```

<span data-ttu-id="9d982-108">Vagy *megadott névvel és erőforráscsoporttal rendelkező erőforrás* meglévő címkéinek megtekintéséhez használja a következőt:</span><span class="sxs-lookup"><span data-stu-id="9d982-108">Or, to see the existing tags for a *resource that has a specified name and resource group*, use:</span></span>

```powershell
(Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
```

<span data-ttu-id="9d982-109">*Adott címkével rendelkező erőforráscsoportok* lekéréséhez használja a következőt:</span><span class="sxs-lookup"><span data-stu-id="9d982-109">To get *resource groups that have a specific tag*, use:</span></span>

```powershell
(Find-AzureRmResourceGroup -Tag @{ Dept="Finance" }).Name 
```

<span data-ttu-id="9d982-110">*Adott címkével rendelkező erőforrások* lekéréséhez használja a következőt:</span><span class="sxs-lookup"><span data-stu-id="9d982-110">To get *resources that have a specific tag*, use:</span></span>

```powershell
(Find-AzureRmResource -TagName Dept -TagValue Finance).Name
```

<span data-ttu-id="9d982-111">Minden alkalommal, amikor címkével lát el egy erőforrást vagy erőforráscsoportot, felülírja a hozzá tartozó korábbi címkéket.</span><span class="sxs-lookup"><span data-stu-id="9d982-111">Every time you apply tags to a resource or a resource group, you overwrite the existing tags on that resource or resource group.</span></span> <span data-ttu-id="9d982-112">Ezért különböző megközelítéseket kell alkalmaznia annak függvényében, hogy az adott erőforrás vagy erőforráscsoport rendelkezik-e címkével.</span><span class="sxs-lookup"><span data-stu-id="9d982-112">Therefore, you must use a different approach based on whether the resource or resource group has existing tags.</span></span> 

<span data-ttu-id="9d982-113">Ha *meglévő címkék nélküli erőforráscsoporthoz* szeretne címkéket adni, használja a következőt:</span><span class="sxs-lookup"><span data-stu-id="9d982-113">To add tags to a *resource group without existing tags*, use:</span></span>

```powershell
Set-AzureRmResourceGroup -Name examplegroup -Tag @{ Dept="IT"; Environment="Test" }
```

<span data-ttu-id="9d982-114">Ha *meglévő címkékkel rendelkező erőforráscsoporthoz* szeretne címkéket adni, kérje le a meglévő címkéket, adja hozzá az új címkét, és alkalmazza ismét a címkéket:</span><span class="sxs-lookup"><span data-stu-id="9d982-114">To add tags to a *resource group that has existing tags*, retrieve the existing tags, add the new tag, and reapply the tags:</span></span>

```powershell
$tags = (Get-AzureRmResourceGroup -Name examplegroup).Tags
$tags += @{Status="Approved"}
Set-AzureRmResourceGroup -Tag $tags -Name examplegroup
```

<span data-ttu-id="9d982-115">Ha *meglévő címkék nélküli erőforráshoz* szeretne címkéket adni, használja a következőt:</span><span class="sxs-lookup"><span data-stu-id="9d982-115">To add tags to a *resource without existing tags*, use:</span></span>

```powershell
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName examplevnet -ResourceGroupName examplegroup
```

<span data-ttu-id="9d982-116">Ha *meglévő címkékkel rendelkező erőforráshoz* szeretne címkéket adni, használja a következőt:</span><span class="sxs-lookup"><span data-stu-id="9d982-116">To add tags to a *resource that has existing tags*, use:</span></span>

```powershell
$tags = (Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
$tags += @{Status="Approved"}
Set-AzureRmResource -Tag $tags -ResourceName examplevnet -ResourceGroupName examplegroup
```

<span data-ttu-id="9d982-117">Ha az erőforráscsoport összes címkéjét szeretné alkalmazni a csoport erőforrásaira, és *nem szeretné megőrizni az erőforrások meglévő címkéit*, használja a következő szkriptet:</span><span class="sxs-lookup"><span data-stu-id="9d982-117">To apply all tags from a resource group to its resources, and *not retain existing tags on the resources*, use the following script:</span></span>

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups) 
{
    Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force } 
}
```

<span data-ttu-id="9d982-118">Ha az erőforráscsoport összes címkéjét szeretné alkalmazni a csoport erőforrásaira, és *nem szeretné megőrizni az erőforrások meglévő, nem ismétlődő címkéit*, a következő szkriptet kell használnia:</span><span class="sxs-lookup"><span data-stu-id="9d982-118">To apply all tags from a resource group to its resources, and *retain existing tags on resources that are not duplicates*, use the following script:</span></span>

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

<span data-ttu-id="9d982-119">Az összes címke eltávolításához adjon át egy üres kivonattáblát:</span><span class="sxs-lookup"><span data-stu-id="9d982-119">To remove all tags, pass an empty hash table:</span></span>

```powershell
Set-AzureRmResourceGroup -Tag @{} -Name examplegroup
```



