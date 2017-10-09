Hello AzureRm.Resources modul 3.0-s verziója működése címkékkel jelentős módosításokat tartalmazza. A továbblépés előtt ellenőrizze, milyen verziót használ:

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

Ha az eredmények megjelenítéséhez verzió 3.0-s vagy újabb hello példákat a témakör a környezetben használható. Ha a 3.0-nál régebbi verzióval rendelkezik, először [frissítsen egy újabb verzióra](/powershell/azureps-cmdlets-docs/) a PowerShell-galériával vagy a Webplatform-telepítővel, és csak ezt követően lépjen tovább.

```powershell
Version
-------
3.5.0
```

toosee hello meglévő címkék egy *erőforráscsoport*, használja:

```powershell
(Get-AzureRmResourceGroup -Name examplegroup).Tags
```

Ez a parancsfájl hello a következő formátumban adja vissza:

```powershell
Name                           Value
----                           -----
Dept                           IT
Environment                    Test
```

toosee hello meglévő címkék egy *erőforrása, amely egy megadott erőforrás-azonosító*, használja:

```powershell
(Get-AzureRmResource -ResourceId {resource-id}).Tags
```

Vagy toosee hello meglévő címkék egy *erőforrást, amely rendelkezik a megadott névvel és erőforrás csoport*, használja:

```powershell
(Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
```

tooget *egy adott címkét tartalmazó erőforráscsoportokat*, használja:

```powershell
(Find-AzureRmResourceGroup -Tag @{ Dept="Finance" }).Name 
```

tooget *egy adott címkével rendelkező erőforrások*, használja:

```powershell
(Find-AzureRmResource -TagName Dept -TagValue Finance).Name
```

Minden alkalommal, amikor a címkék tooa erőforrás vagy egy erőforráscsoport alkalmazásához, felülírhatja hello meglévő címkét az adott erőforrás vagy az erőforráscsoportot. Ezért hello erőforrás vagy erőforráscsoport rendelkezik-e meglévő címkék alapján másik módszert kell használnia. 

tooadd címkéket tooa *erőforráscsoport meglévő címkék nélkül*, használja:

```powershell
Set-AzureRmResourceGroup -Name examplegroup -Tag @{ Dept="IT"; Environment="Test" }
```

tooadd címkéket tooa *meglévő címkékkel rendelkező erőforráscsoport*, hello meglévő címkék beolvasása, hello új címke hozzáadása, és alkalmazza újra hello címkék:

```powershell
$tags = (Get-AzureRmResourceGroup -Name examplegroup).Tags
$tags += @{Status="Approved"}
Set-AzureRmResourceGroup -Tag $tags -Name examplegroup
```

tooadd címkéket tooa *meglévő címkék nélkül erőforrás*, használja:

```powershell
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName examplevnet -ResourceGroupName examplegroup
```

tooadd címkéket tooa *meglévő címkék erőforrást*, használja:

```powershell
$tags = (Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
$tags += @{Status="Approved"}
Set-AzureRmResource -Tag $tags -ResourceName examplevnet -ResourceGroupName examplegroup
```

minden tooapply egy erőforrás tooits erőforrások, a címkéket és *nem őriz meg a meglévő címkék hello erőforrások*, a következő parancsfájl hello használata:

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups) 
{
    Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force } 
}
```

minden tooapply egy erőforrás tooits erőforrások, a címkéket és *megtartja a meglévő címkéket az erőforrásokat, amelyek nincsenek ismétlődések*, a következő parancsfájl hello használata:

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

tooremove található összes kódcímkének, egy üres kivonattábla át:

```powershell
Set-AzureRmResourceGroup -Tag @{} -Name examplegroup
```



