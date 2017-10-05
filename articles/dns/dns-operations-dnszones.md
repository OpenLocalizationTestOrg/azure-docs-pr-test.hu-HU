---
title: "Az Azure DNS - PowerShell DNS-zónák kezelése |} Microsoft Docs"
description: "DNS-zónák Azure Powershell használatával kezelheti. Ez a cikk ismerteti, hogyan frissítés, törlés és DNS-zóna létrehozása az Azure DNS szolgáltatásra"
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
ms.openlocfilehash: 92f1da660d875c76d5d826669d6c1d12018c3d0a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-dns-zones-using-powershell"></a><span data-ttu-id="b8195-104">PowerShell-lel DNS-zónák kezelése</span><span class="sxs-lookup"><span data-stu-id="b8195-104">How to manage DNS Zones using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="b8195-105">Portal</span><span class="sxs-lookup"><span data-stu-id="b8195-105">Portal</span></span>](dns-operations-dnszones-portal.md)
> * [<span data-ttu-id="b8195-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b8195-106">PowerShell</span></span>](dns-operations-dnszones.md)
> * [<span data-ttu-id="b8195-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="b8195-107">Azure CLI 1.0</span></span>](dns-operations-dnszones-cli-nodejs.md)
> * [<span data-ttu-id="b8195-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="b8195-108">Azure CLI 2.0</span></span>](dns-operations-dnszones-cli.md)

<span data-ttu-id="b8195-109">Ez a cikk bemutatja, hogyan a DNS-zónák kezelése az Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="b8195-109">This article shows you how to manage your DNS zones by using Azure PowerShell.</span></span> <span data-ttu-id="b8195-110">Emellett kezelhetők a DNS-zónák használata a platformok közötti [Azure CLI](dns-operations-dnszones-cli.md) vagy az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="b8195-110">You can also manage your DNS zones using the cross-platform [Azure CLI](dns-operations-dnszones-cli.md) or the Azure portal.</span></span>

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-powershell-setup](../../includes/dns-powershell-setup-include.md)]


## <a name="create-a-dns-zone"></a><span data-ttu-id="b8195-111">DNS-zóna létrehozása</span><span class="sxs-lookup"><span data-stu-id="b8195-111">Create a DNS zone</span></span>

<span data-ttu-id="b8195-112">A DNS-zóna az `New-AzureRmDnsZone` parancsmag használatával hozható létre.</span><span class="sxs-lookup"><span data-stu-id="b8195-112">A DNS zone is created by using the `New-AzureRmDnsZone` cmdlet.</span></span>

<span data-ttu-id="b8195-113">Az alábbi példa létrehoz egy DNS-zónát *contoso.com* erőforráscsoportban nevű *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="b8195-113">The following example creates a DNS zone called *contoso.com* in the resource group called *MyResourceGroup*:</span></span>

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

<span data-ttu-id="b8195-114">A következő példa bemutatja, hogyan hozzon létre egy DNS-zónát és két [Azure Resource Manager címkéket](dns-zones-records.md#tags), *project = demo* és *env = test*:</span><span class="sxs-lookup"><span data-stu-id="b8195-114">The following example shows how to create a DNS zone with two [Azure Resource Manager tags](dns-zones-records.md#tags), *project = demo* and *env = test*:</span></span>

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

## <a name="get-a-dns-zone"></a><span data-ttu-id="b8195-115">A DNS-zóna beolvasása</span><span class="sxs-lookup"><span data-stu-id="b8195-115">Get a DNS zone</span></span>

<span data-ttu-id="b8195-116">A DNS-zónák lekéréséhez használja a `Get-AzureRmDnsZone` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b8195-116">To retrieve a DNS zone, use the `Get-AzureRmDnsZone` cmdlet.</span></span> <span data-ttu-id="b8195-117">Ez a művelet egy meglévő az Azure DNS-zónához tartozó DNS-zóna objektum adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b8195-117">This operation returns a DNS zone object corresponding to an existing zone in Azure DNS.</span></span> <span data-ttu-id="b8195-118">Az objektum tartalmazza a zóna (például a rekordhalmazok száma) adatait, de nem tartalmaz a rekordhalmazok maguk (lásd: `Get-AzureRmDnsRecordSet`).</span><span class="sxs-lookup"><span data-stu-id="b8195-118">The object contains data about the zone (such as the number of record sets), but does not contain the record sets themselves (see `Get-AzureRmDnsRecordSet`).</span></span>

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

## <a name="list-dns-zones"></a><span data-ttu-id="b8195-119">Lista DNS-zónák</span><span class="sxs-lookup"><span data-stu-id="b8195-119">List DNS zones</span></span>

<span data-ttu-id="b8195-120">A zóna nevét a kihagyásával őriznek `Get-AzureRmDnsZone`, minden zóna erőforráscsoportban enumerálása.</span><span class="sxs-lookup"><span data-stu-id="b8195-120">By omitting the zone name from `Get-AzureRmDnsZone`, you can enumerate all zones in a resource group.</span></span> <span data-ttu-id="b8195-121">Ez a művelet a zóna objektumok tömbjét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b8195-121">This operation returns an array of zone objects.</span></span>

```powershell
$zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup
```

<span data-ttu-id="b8195-122">A zóna nevét, mind az erőforráscsoport neve a kihagyásával őriznek `Get-AzureRmDnsZone`, az Azure-előfizetés az összes zóna enumerálása.</span><span class="sxs-lookup"><span data-stu-id="b8195-122">By omitting both the zone name and the resource group name from `Get-AzureRmDnsZone`, you can enumerate all zones in the Azure subscription.</span></span>

```powershell
$zoneList = Get-AzureRmDnsZone
```

## <a name="update-a-dns-zone"></a><span data-ttu-id="b8195-123">A DNS-zóna frissítéséhez</span><span class="sxs-lookup"><span data-stu-id="b8195-123">Update a DNS zone</span></span>

<span data-ttu-id="b8195-124">A DNS-zóna erőforrás is lehet módosítani a `Set-AzureRmDnsZone`.</span><span class="sxs-lookup"><span data-stu-id="b8195-124">Changes to a DNS zone resource can be made by using `Set-AzureRmDnsZone`.</span></span> <span data-ttu-id="b8195-125">Ez a parancsmag nem frissíti a DNS-rekordhalmazok a zónán belül (lásd: [kezelése DNS-rekordok hogyan](dns-operations-recordsets.md)).</span><span class="sxs-lookup"><span data-stu-id="b8195-125">This cmdlet does not update any of the DNS record sets within the zone (see [How to Manage DNS records](dns-operations-recordsets.md)).</span></span> <span data-ttu-id="b8195-126">Csak a zóna erőforrás maga tulajdonságainak frissítésére szolgál.</span><span class="sxs-lookup"><span data-stu-id="b8195-126">It's only used to update properties of the zone resource itself.</span></span> <span data-ttu-id="b8195-127">Az írható zóna tulajdonságai jelenleg csak a [Azure Resource Manager "címkék" a zóna erőforrás](dns-zones-records.md#tags).</span><span class="sxs-lookup"><span data-stu-id="b8195-127">The writable zone properties are currently limited to the [Azure Resource Manager ‘tags’ for the zone resource](dns-zones-records.md#tags).</span></span>

<span data-ttu-id="b8195-128">A DNS-zóna frissítéséhez az alábbi két módszer egyikével:</span><span class="sxs-lookup"><span data-stu-id="b8195-128">Use one of the following two ways to update a DNS zone:</span></span>

### <a name="specify-the-zone-using-the-zone-name-and-resource-group"></a><span data-ttu-id="b8195-129">Adja meg a zóna, a zóna nevét és az erőforrás csoport használata</span><span class="sxs-lookup"><span data-stu-id="b8195-129">Specify the zone using the zone name and resource group</span></span>

<span data-ttu-id="b8195-130">Ez a megközelítés lecseréli a meglévő zóna címkék megadott értékek.</span><span class="sxs-lookup"><span data-stu-id="b8195-130">This approach replaces the existing zone tags with the values specified.</span></span>

```powershell
Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

### <a name="specify-the-zone-using-a-zone-object"></a><span data-ttu-id="b8195-131">Adjon meg egy $zone objektum használatával</span><span class="sxs-lookup"><span data-stu-id="b8195-131">Specify the zone using a $zone object</span></span>

<span data-ttu-id="b8195-132">Ez a megközelítés átveszi a meglévő zóna objektumot, módosítja a címkék és majd véglegesíti a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="b8195-132">This approach retrieves the existing zone object, modifies the tags, and then commits the changes.</span></span> <span data-ttu-id="b8195-133">Ezzel a módszerrel meglévő címkék megőrizhetők.</span><span class="sxs-lookup"><span data-stu-id="b8195-133">In this way, existing tags can be preserved.</span></span>

```powershell
# Get the zone object
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup

# Remove an existing tag
$zone.Tags.Remove("project")

# Add a new tag
$zone.Tags.Add("status","approved")

# Commit changes
Set-AzureRmDnsZone -Zone $zone
```

<span data-ttu-id="b8195-134">Használata esetén `Set-AzureRmDnsZone` $zone objektummal [Etag ellenőrzi](dns-zones-records.md#etags) biztosítják az egyidejű változtatások nem íródnak felül.</span><span class="sxs-lookup"><span data-stu-id="b8195-134">When using `Set-AzureRmDnsZone` with a $zone object, [Etag checks](dns-zones-records.md#etags) are used to ensure concurrent changes are not overwritten.</span></span> <span data-ttu-id="b8195-135">A választható használható `-Overwrite` kapcsoló ezen ellenőrzések letiltásához.</span><span class="sxs-lookup"><span data-stu-id="b8195-135">You can use the optional `-Overwrite` switch to suppress these checks.</span></span>

## <a name="delete-a-dns-zone"></a><span data-ttu-id="b8195-136">A DNS-zóna törlése</span><span class="sxs-lookup"><span data-stu-id="b8195-136">Delete a DNS Zone</span></span>

<span data-ttu-id="b8195-137">DNS-zónák segítségével törölhetők a `Remove-AzureRmDnsZone` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b8195-137">DNS zones can be deleted using the `Remove-AzureRmDnsZone` cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="b8195-138">Is egy DNS-zóna törlésével törli az összes DNS-rekordokat a zónán belül.</span><span class="sxs-lookup"><span data-stu-id="b8195-138">Deleting a DNS zone also deletes all DNS records within the zone.</span></span> <span data-ttu-id="b8195-139">Ez a művelet nem vonható vissza.</span><span class="sxs-lookup"><span data-stu-id="b8195-139">This operation cannot be undone.</span></span> <span data-ttu-id="b8195-140">Ha a DNS-zóna használatban van, a zóna szolgáltatásokat sikertelen lesz a zóna törlődik.</span><span class="sxs-lookup"><span data-stu-id="b8195-140">If the DNS zone is in use, services using the zone will fail when the zone is deleted.</span></span>
>
><span data-ttu-id="b8195-141">A zóna véletlen törlés elleni védelem érdekében, lásd: [hogyan védi a DNS-zónák és rekordok](dns-protect-zones-recordsets.md).</span><span class="sxs-lookup"><span data-stu-id="b8195-141">To protect against accidental zone deletion, see [How to protect DNS zones and records](dns-protect-zones-recordsets.md).</span></span>


<span data-ttu-id="b8195-142">Használja a DNS-zóna törlése a következő két módszer egyikével:</span><span class="sxs-lookup"><span data-stu-id="b8195-142">Use one of the following two ways to delete a DNS zone:</span></span>

### <a name="specify-the-zone-using-the-zone-name-and-resource-group-name"></a><span data-ttu-id="b8195-143">Adja meg a zóna nevét és az erőforráscsoport-név használatával</span><span class="sxs-lookup"><span data-stu-id="b8195-143">Specify the zone using the zone name and resource group name</span></span>

```powershell
Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

### <a name="specify-the-zone-using-a-zone-object"></a><span data-ttu-id="b8195-144">Adjon meg egy $zone objektum használatával</span><span class="sxs-lookup"><span data-stu-id="b8195-144">Specify the zone using a $zone object</span></span>

<span data-ttu-id="b8195-145">Megadhatja a zónát, hogy törölni kell a `$zone` által visszaadott objektum `Get-AzureRmDnsZone`.</span><span class="sxs-lookup"><span data-stu-id="b8195-145">You can specify the zone to be deleted using a `$zone` object returned by `Get-AzureRmDnsZone`.</span></span>

```powershell
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
Remove-AzureRmDnsZone -Zone $zone
```

<span data-ttu-id="b8195-146">A zóna objektum is átirányítható paraméterként átadott helyett:</span><span class="sxs-lookup"><span data-stu-id="b8195-146">The zone object can also be piped instead of being passed as a parameter:</span></span>

```powershell
Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone

```

<span data-ttu-id="b8195-147">A `Set-AzureRmDnsZone`, adja meg a zóna használatával egy `$zone` objektum lehetővé teszi, hogy a Etag ellenőrzi, hogy ellenőrizze a egyidejű változtatások nem törlődnek.</span><span class="sxs-lookup"><span data-stu-id="b8195-147">As with `Set-AzureRmDnsZone`, specifying the zone using a `$zone` object enables Etag checks to ensure concurrent changes are not deleted.</span></span> <span data-ttu-id="b8195-148">Használja a `-Overwrite` kapcsoló ezen ellenőrzések letiltásához.</span><span class="sxs-lookup"><span data-stu-id="b8195-148">Use the `-Overwrite` switch to suppress these checks.</span></span>

## <a name="confirmation-prompts"></a><span data-ttu-id="b8195-149">Megerősítés</span><span class="sxs-lookup"><span data-stu-id="b8195-149">Confirmation prompts</span></span>

<span data-ttu-id="b8195-150">A `New-AzureRmDnsZone`, `Set-AzureRmDnsZone`, és `Remove-AzureRmDnsZone` parancsmagok minden megerősítés támogatja.</span><span class="sxs-lookup"><span data-stu-id="b8195-150">The `New-AzureRmDnsZone`, `Set-AzureRmDnsZone`, and `Remove-AzureRmDnsZone` cmdlets all support confirmation prompts.</span></span>

<span data-ttu-id="b8195-151">Mindkét `New-AzureRmDnsZone` és `Set-AzureRmDnsZone` megerősítés kérése, ha a `$ConfirmPreference` PowerShell preferenciaváltozót értéke `Medium` vagy alacsonyabb.</span><span class="sxs-lookup"><span data-stu-id="b8195-151">Both `New-AzureRmDnsZone` and `Set-AzureRmDnsZone` prompt for confirmation if the `$ConfirmPreference` PowerShell preference variable has a value of `Medium` or lower.</span></span> <span data-ttu-id="b8195-152">Törli a DNS-zónát, potenciálisan nagy jelentőségű miatt a `Remove-AzureRmDnsZone` parancsmag kell megerősítést kérni fogja, ha a `$ConfirmPreference` PowerShell változó értéke bármely eltérő `None`.</span><span class="sxs-lookup"><span data-stu-id="b8195-152">Due to the potentially high impact of deleting a DNS zone, the `Remove-AzureRmDnsZone` cmdlet prompts for confirmation if the `$ConfirmPreference` PowerShell variable has any value other than `None`.</span></span>

<span data-ttu-id="b8195-153">Az alapértelmezett érték óta `$ConfirmPreference` van `High`, csak `Remove-AzureRmDnsZone` alapértelmezés szerint rákérdez.</span><span class="sxs-lookup"><span data-stu-id="b8195-153">Since the default value for `$ConfirmPreference` is `High`, only `Remove-AzureRmDnsZone` prompts for confirmation by default.</span></span>

<span data-ttu-id="b8195-154">Ha szeretné felülbírálni az aktuális `$ConfirmPreference` használatának beállítása a `-Confirm` paraméter.</span><span class="sxs-lookup"><span data-stu-id="b8195-154">You can override the current `$ConfirmPreference` setting using the `-Confirm` parameter.</span></span> <span data-ttu-id="b8195-155">Ha megad `-Confirm` vagy `-Confirm:$True` , a parancsmag megerősítést kér, mielőtt futtatja.</span><span class="sxs-lookup"><span data-stu-id="b8195-155">If you specify `-Confirm` or `-Confirm:$True` , the cmdlet prompts you for confirmation before it runs.</span></span> <span data-ttu-id="b8195-156">Ha megad `-Confirm:$False` , a parancsmag nem figyelmeztet megerősítést kér.</span><span class="sxs-lookup"><span data-stu-id="b8195-156">If you specify `-Confirm:$False` , the cmdlet does not prompt you for confirmation.</span></span>

<span data-ttu-id="b8195-157">További információ `-Confirm` és `$ConfirmPreference`, lásd: [kapcsolatos Preferenciaváltozók](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).</span><span class="sxs-lookup"><span data-stu-id="b8195-157">For more information about `-Confirm` and `$ConfirmPreference`, see [About Preference Variables](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b8195-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b8195-158">Next steps</span></span>

<span data-ttu-id="b8195-159">Megtudhatja, hogyan [rekordhalmazokat és rekordokat kezelése](dns-operations-recordsets.md) a DNS-zónában.</span><span class="sxs-lookup"><span data-stu-id="b8195-159">Learn how to [manage record sets and records](dns-operations-recordsets.md) in your DNS zone.</span></span>
<br>
<span data-ttu-id="b8195-160">Megtudhatja, hogyan [tartomány delegálása az Azure DNS-](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="b8195-160">Learn how to [delegate your domain to Azure DNS](dns-domain-delegation.md).</span></span>
<br>
<span data-ttu-id="b8195-161">Tekintse át a [Azure DNS PowerShell referenciadokumentációt](/powershell/module/azurerm.dns).</span><span class="sxs-lookup"><span data-stu-id="b8195-161">Review the [Azure DNS PowerShell reference documentation](/powershell/module/azurerm.dns).</span></span>

