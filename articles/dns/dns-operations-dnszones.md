---
title: "aaaManage DNS-zónák az Azure DNS - PowerShell |} Microsoft Docs"
description: "DNS-zónák Azure Powershell használatával kezelheti. Ez a cikk ismerteti, hogyan tooupdate, törlése és a DNS-zóna létrehozása az Azure DNS szolgáltatásra"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: a67992ab-8166-4052-9b28-554c5a39e60c
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/14/2016
ms.author: gwallace
ms.openlocfilehash: 261b89f72213aa9784034d47ff9d1c55a4e80d65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-dns-zones-using-powershell"></a><span data-ttu-id="00acb-104">Hogyan toomanage DNS-zónákat a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="00acb-104">How toomanage DNS Zones using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="00acb-105">Portál</span><span class="sxs-lookup"><span data-stu-id="00acb-105">Portal</span></span>](dns-operations-dnszones-portal.md)
> * [<span data-ttu-id="00acb-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="00acb-106">PowerShell</span></span>](dns-operations-dnszones.md)
> * [<span data-ttu-id="00acb-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="00acb-107">Azure CLI 1.0</span></span>](dns-operations-dnszones-cli-nodejs.md)
> * [<span data-ttu-id="00acb-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="00acb-108">Azure CLI 2.0</span></span>](dns-operations-dnszones-cli.md)

<span data-ttu-id="00acb-109">Ez a cikk bemutatja, hogyan toomanage a DNS-zóna Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="00acb-109">This article shows you how toomanage your DNS zones by using Azure PowerShell.</span></span> <span data-ttu-id="00acb-110">A DNS-zónák hello platformfüggetlen segítségével is kezelheti [Azure CLI](dns-operations-dnszones-cli.md) vagy hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="00acb-110">You can also manage your DNS zones using hello cross-platform [Azure CLI](dns-operations-dnszones-cli.md) or hello Azure portal.</span></span>

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-powershell-setup](../../includes/dns-powershell-setup-include.md)]


## <a name="create-a-dns-zone"></a><span data-ttu-id="00acb-111">DNS-zóna létrehozása</span><span class="sxs-lookup"><span data-stu-id="00acb-111">Create a DNS zone</span></span>

<span data-ttu-id="00acb-112">A DNS-zónák hello segítségével hozhatók létre `New-AzureRmDnsZone` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="00acb-112">A DNS zone is created by using hello `New-AzureRmDnsZone` cmdlet.</span></span>

<span data-ttu-id="00acb-113">hello alábbi példa létrehoz egy DNS-zónát *contoso.com* nevű hello erőforráscsoportban *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="00acb-113">hello following example creates a DNS zone called *contoso.com* in hello resource group called *MyResourceGroup*:</span></span>

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

<span data-ttu-id="00acb-114">hello következő példa bemutatja, hogyan toocreate DNS zónát, és két [Azure Resource Manager címkéket](dns-zones-records.md#tags), *project = demo* és *env = test*:</span><span class="sxs-lookup"><span data-stu-id="00acb-114">hello following example shows how toocreate a DNS zone with two [Azure Resource Manager tags](dns-zones-records.md#tags), *project = demo* and *env = test*:</span></span>

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

## <a name="get-a-dns-zone"></a><span data-ttu-id="00acb-115">A DNS-zóna beolvasása</span><span class="sxs-lookup"><span data-stu-id="00acb-115">Get a DNS zone</span></span>

<span data-ttu-id="00acb-116">egy DNS-zóna tooretrieve hello használata `Get-AzureRmDnsZone` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="00acb-116">tooretrieve a DNS zone, use hello `Get-AzureRmDnsZone` cmdlet.</span></span> <span data-ttu-id="00acb-117">Ez a művelet adja vissza, a DNS-zónák objektum megfelelő tooan meglévő zóna Azure DNS-ben.</span><span class="sxs-lookup"><span data-stu-id="00acb-117">This operation returns a DNS zone object corresponding tooan existing zone in Azure DNS.</span></span> <span data-ttu-id="00acb-118">hello objektum hello zóna (például hello rekordkészletek) kapcsolatos adatokat tartalmaz, de nem tartalmaz maguk rekordhalmazok hello (lásd: `Get-AzureRmDnsRecordSet`).</span><span class="sxs-lookup"><span data-stu-id="00acb-118">hello object contains data about hello zone (such as hello number of record sets), but does not contain hello record sets themselves (see `Get-AzureRmDnsRecordSet`).</span></span>

```powershell
Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup

Name                  : contoso.com
ResourceGroupName     : myresourcegroup
Etag                  : 00000003-0000-0000-8ec2-f4879750d201
Tags                  : {project, env}
NameServers           : {ns1-01.azure-dns.com., ns2-01.azure-dns.net., ns3-01.azure-dns.org.,
                        ns4-01.azure-dns.info.}
NumberOfRecordSets    : 2
MaxNumberOfRecordSets : 5000
```

## <a name="list-dns-zones"></a><span data-ttu-id="00acb-119">Lista DNS-zónák</span><span class="sxs-lookup"><span data-stu-id="00acb-119">List DNS zones</span></span>

<span data-ttu-id="00acb-120">Hello zóna nevét a kihagyásával őriznek `Get-AzureRmDnsZone`, minden zóna erőforráscsoportban enumerálása.</span><span class="sxs-lookup"><span data-stu-id="00acb-120">By omitting hello zone name from `Get-AzureRmDnsZone`, you can enumerate all zones in a resource group.</span></span> <span data-ttu-id="00acb-121">Ez a művelet a zóna objektumok tömbjét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="00acb-121">This operation returns an array of zone objects.</span></span>

```powershell
$zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup
```

<span data-ttu-id="00acb-122">Hello zóna neve és a hello erőforráscsoport-név a kihagyásával őriznek `Get-AzureRmDnsZone`, minden zónákat az Azure-előfizetés hello enumerálása.</span><span class="sxs-lookup"><span data-stu-id="00acb-122">By omitting both hello zone name and hello resource group name from `Get-AzureRmDnsZone`, you can enumerate all zones in hello Azure subscription.</span></span>

```powershell
$zoneList = Get-AzureRmDnsZone
```

## <a name="update-a-dns-zone"></a><span data-ttu-id="00acb-123">A DNS-zóna frissítéséhez</span><span class="sxs-lookup"><span data-stu-id="00acb-123">Update a DNS zone</span></span>

<span data-ttu-id="00acb-124">Módosítja a DNS-zóna erőforrásrekordok használatával tehető tooa `Set-AzureRmDnsZone`.</span><span class="sxs-lookup"><span data-stu-id="00acb-124">Changes tooa DNS zone resource can be made by using `Set-AzureRmDnsZone`.</span></span> <span data-ttu-id="00acb-125">Ez a parancsmag nem frissíti a hello DNS-rekordhalmazok hello zónán belül (lásd: [hogyan tooManage DNS-rekordok](dns-operations-recordsets.md)).</span><span class="sxs-lookup"><span data-stu-id="00acb-125">This cmdlet does not update any of hello DNS record sets within hello zone (see [How tooManage DNS records](dns-operations-recordsets.md)).</span></span> <span data-ttu-id="00acb-126">Hello zóna erőforrás maga tooupdate tulajdonságainak csak használatban van.</span><span class="sxs-lookup"><span data-stu-id="00acb-126">It's only used tooupdate properties of hello zone resource itself.</span></span> <span data-ttu-id="00acb-127">hello írható zóna tulajdonságai jelenleg korlátozott toohello [Azure Resource Manager "címkék" hello zóna erőforrás](dns-zones-records.md#tags).</span><span class="sxs-lookup"><span data-stu-id="00acb-127">hello writable zone properties are currently limited toohello [Azure Resource Manager ‘tags’ for hello zone resource](dns-zones-records.md#tags).</span></span>

<span data-ttu-id="00acb-128">A DNS-zónák használja a következő két módon tooupdate hello egyikét:</span><span class="sxs-lookup"><span data-stu-id="00acb-128">Use one of hello following two ways tooupdate a DNS zone:</span></span>

### <a name="specify-hello-zone-using-hello-zone-name-and-resource-group"></a><span data-ttu-id="00acb-129">Adja meg a hello zóna hello zóna nevét és az erőforrás csoport használata</span><span class="sxs-lookup"><span data-stu-id="00acb-129">Specify hello zone using hello zone name and resource group</span></span>

<span data-ttu-id="00acb-130">Ez a megközelítés hello meglévő zóna címkék lecseréli a megadott hello értékek.</span><span class="sxs-lookup"><span data-stu-id="00acb-130">This approach replaces hello existing zone tags with hello values specified.</span></span>

```powershell
Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

### <a name="specify-hello-zone-using-a-zone-object"></a><span data-ttu-id="00acb-131">Adjon meg egy $zone objektummal hello zóna</span><span class="sxs-lookup"><span data-stu-id="00acb-131">Specify hello zone using a $zone object</span></span>

<span data-ttu-id="00acb-132">Ez a megközelítés hello meglévő zóna objektum lekéri, hello címkék módosítja, és majd a hello módosítások véglegesítése.</span><span class="sxs-lookup"><span data-stu-id="00acb-132">This approach retrieves hello existing zone object, modifies hello tags, and then commits hello changes.</span></span> <span data-ttu-id="00acb-133">Ezzel a módszerrel meglévő címkék megőrizhetők.</span><span class="sxs-lookup"><span data-stu-id="00acb-133">In this way, existing tags can be preserved.</span></span>

```powershell
# Get hello zone object
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup

# Remove an existing tag
$zone.Tags.Remove("project")

# Add a new tag
$zone.Tags.Add("status","approved")

# Commit changes
Set-AzureRmDnsZone -Zone $zone
```

<span data-ttu-id="00acb-134">Használata esetén `Set-AzureRmDnsZone` $zone objektummal [Etag ellenőrzi](dns-zones-records.md#etags) használt tooensure egyidejű változtatások nem íródnak felül.</span><span class="sxs-lookup"><span data-stu-id="00acb-134">When using `Set-AzureRmDnsZone` with a $zone object, [Etag checks](dns-zones-records.md#etags) are used tooensure concurrent changes are not overwritten.</span></span> <span data-ttu-id="00acb-135">Használhatja a választható hello `-Overwrite` kapcsoló toosuppress ezen ellenőrzések.</span><span class="sxs-lookup"><span data-stu-id="00acb-135">You can use hello optional `-Overwrite` switch toosuppress these checks.</span></span>

## <a name="delete-a-dns-zone"></a><span data-ttu-id="00acb-136">A DNS-zóna törlése</span><span class="sxs-lookup"><span data-stu-id="00acb-136">Delete a DNS Zone</span></span>

<span data-ttu-id="00acb-137">DNS-zónák hello törölhetők `Remove-AzureRmDnsZone` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="00acb-137">DNS zones can be deleted using hello `Remove-AzureRmDnsZone` cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="00acb-138">A DNS-zónák törlésekor a hello zónán belül minden DNS-rekordokat is törlődnek.</span><span class="sxs-lookup"><span data-stu-id="00acb-138">Deleting a DNS zone also deletes all DNS records within hello zone.</span></span> <span data-ttu-id="00acb-139">Ez a művelet nem vonható vissza.</span><span class="sxs-lookup"><span data-stu-id="00acb-139">This operation cannot be undone.</span></span> <span data-ttu-id="00acb-140">Hello DNS-zóna van használatban, ha hello zóna használó szolgáltatások nem indulnak hello zóna törlésekor.</span><span class="sxs-lookup"><span data-stu-id="00acb-140">If hello DNS zone is in use, services using hello zone will fail when hello zone is deleted.</span></span>
>
><span data-ttu-id="00acb-141">a zóna véletlen törlése ellen tooprotect lásd: [hogyan tooprotect DNS zónák, valamint megjegyzi](dns-protect-zones-recordsets.md).</span><span class="sxs-lookup"><span data-stu-id="00acb-141">tooprotect against accidental zone deletion, see [How tooprotect DNS zones and records](dns-protect-zones-recordsets.md).</span></span>


<span data-ttu-id="00acb-142">A DNS-zónák használja a következő két módon toodelete hello egyikét:</span><span class="sxs-lookup"><span data-stu-id="00acb-142">Use one of hello following two ways toodelete a DNS zone:</span></span>

### <a name="specify-hello-zone-using-hello-zone-name-and-resource-group-name"></a><span data-ttu-id="00acb-143">Adja meg a hello zóna hello zóna neve és az erőforráscsoport neve</span><span class="sxs-lookup"><span data-stu-id="00acb-143">Specify hello zone using hello zone name and resource group name</span></span>

```powershell
Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

### <a name="specify-hello-zone-using-a-zone-object"></a><span data-ttu-id="00acb-144">Adjon meg egy $zone objektummal hello zóna</span><span class="sxs-lookup"><span data-stu-id="00acb-144">Specify hello zone using a $zone object</span></span>

<span data-ttu-id="00acb-145">Hello zóna toobe törölni is megadhat egy `$zone` által visszaadott objektum `Get-AzureRmDnsZone`.</span><span class="sxs-lookup"><span data-stu-id="00acb-145">You can specify hello zone toobe deleted using a `$zone` object returned by `Get-AzureRmDnsZone`.</span></span>

```powershell
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
Remove-AzureRmDnsZone -Zone $zone
```

<span data-ttu-id="00acb-146">hello zóna objektum is átirányítható paraméterként átadott helyett:</span><span class="sxs-lookup"><span data-stu-id="00acb-146">hello zone object can also be piped instead of being passed as a parameter:</span></span>

```powershell
Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone

```

<span data-ttu-id="00acb-147">A `Set-AzureRmDnsZone`, megadó hello zóna használatával egy `$zone` objektum lehetővé teszi, hogy Etag ellenőrzi, hogy tooensure egyidejű változtatások nem törlődnek.</span><span class="sxs-lookup"><span data-stu-id="00acb-147">As with `Set-AzureRmDnsZone`, specifying hello zone using a `$zone` object enables Etag checks tooensure concurrent changes are not deleted.</span></span> <span data-ttu-id="00acb-148">Használjon hello `-Overwrite` kapcsoló toosuppress ezen ellenőrzések.</span><span class="sxs-lookup"><span data-stu-id="00acb-148">Use hello `-Overwrite` switch toosuppress these checks.</span></span>

## <a name="confirmation-prompts"></a><span data-ttu-id="00acb-149">Megerősítés</span><span class="sxs-lookup"><span data-stu-id="00acb-149">Confirmation prompts</span></span>

<span data-ttu-id="00acb-150">Hello `New-AzureRmDnsZone`, `Set-AzureRmDnsZone`, és `Remove-AzureRmDnsZone` parancsmagok minden megerősítés támogatja.</span><span class="sxs-lookup"><span data-stu-id="00acb-150">hello `New-AzureRmDnsZone`, `Set-AzureRmDnsZone`, and `Remove-AzureRmDnsZone` cmdlets all support confirmation prompts.</span></span>

<span data-ttu-id="00acb-151">Mindkét `New-AzureRmDnsZone` és `Set-AzureRmDnsZone` megerősítés kérése, ha hello `$ConfirmPreference` PowerShell preferenciaváltozót értéke `Medium` vagy alacsonyabb.</span><span class="sxs-lookup"><span data-stu-id="00acb-151">Both `New-AzureRmDnsZone` and `Set-AzureRmDnsZone` prompt for confirmation if hello `$ConfirmPreference` PowerShell preference variable has a value of `Medium` or lower.</span></span> <span data-ttu-id="00acb-152">Lejáró toohello potenciálisan nagy jelentőségű törli a DNS-zónák hello `Remove-AzureRmDnsZone` parancsmag kell megerősítést kérni fogja, ha hello `$ConfirmPreference` PowerShell változó értéke bármely eltérő `None`.</span><span class="sxs-lookup"><span data-stu-id="00acb-152">Due toohello potentially high impact of deleting a DNS zone, hello `Remove-AzureRmDnsZone` cmdlet prompts for confirmation if hello `$ConfirmPreference` PowerShell variable has any value other than `None`.</span></span>

<span data-ttu-id="00acb-153">Hello alapértelmezett értéke óta `$ConfirmPreference` van `High`, csak `Remove-AzureRmDnsZone` alapértelmezés szerint rákérdez.</span><span class="sxs-lookup"><span data-stu-id="00acb-153">Since hello default value for `$ConfirmPreference` is `High`, only `Remove-AzureRmDnsZone` prompts for confirmation by default.</span></span>

<span data-ttu-id="00acb-154">Felülbírálhatja hello aktuális `$ConfirmPreference` hello használata beállítást `-Confirm` paraméter.</span><span class="sxs-lookup"><span data-stu-id="00acb-154">You can override hello current `$ConfirmPreference` setting using hello `-Confirm` parameter.</span></span> <span data-ttu-id="00acb-155">Ha megad `-Confirm` vagy `-Confirm:$True` , hello parancsmag megerősítést kér, mielőtt futtatja.</span><span class="sxs-lookup"><span data-stu-id="00acb-155">If you specify `-Confirm` or `-Confirm:$True` , hello cmdlet prompts you for confirmation before it runs.</span></span> <span data-ttu-id="00acb-156">Ha megad `-Confirm:$False` , hello parancsmag nem figyelmeztet megerősítést kér.</span><span class="sxs-lookup"><span data-stu-id="00acb-156">If you specify `-Confirm:$False` , hello cmdlet does not prompt you for confirmation.</span></span>

<span data-ttu-id="00acb-157">További információ `-Confirm` és `$ConfirmPreference`, lásd: [kapcsolatos Preferenciaváltozók](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).</span><span class="sxs-lookup"><span data-stu-id="00acb-157">For more information about `-Confirm` and `$ConfirmPreference`, see [About Preference Variables](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).</span></span>

## <a name="next-steps"></a><span data-ttu-id="00acb-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="00acb-158">Next steps</span></span>

<span data-ttu-id="00acb-159">Ismerje meg, hogyan túl[rekordhalmazokat és rekordokat kezelése](dns-operations-recordsets.md) a DNS-zónában.</span><span class="sxs-lookup"><span data-stu-id="00acb-159">Learn how too[manage record sets and records](dns-operations-recordsets.md) in your DNS zone.</span></span>
<br>
<span data-ttu-id="00acb-160">Ismerje meg, hogyan túl[delegálása a tartományi tooAzure DNS](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="00acb-160">Learn how too[delegate your domain tooAzure DNS](dns-domain-delegation.md).</span></span>
<br>
<span data-ttu-id="00acb-161">Felülvizsgálati hello [Azure DNS PowerShell referenciadokumentációt](/powershell/module/azurerm.dns).</span><span class="sxs-lookup"><span data-stu-id="00acb-161">Review hello [Azure DNS PowerShell reference documentation](/powershell/module/azurerm.dns).</span></span>

