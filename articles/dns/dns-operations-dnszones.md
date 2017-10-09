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
# <a name="how-toomanage-dns-zones-using-powershell"></a>Hogyan toomanage DNS-zónákat a PowerShell használatával

> [!div class="op_single_selector"]
> * [Portál](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)

Ez a cikk bemutatja, hogyan toomanage a DNS-zóna Azure PowerShell használatával. A DNS-zónák hello platformfüggetlen segítségével is kezelheti [Azure CLI](dns-operations-dnszones-cli.md) vagy hello Azure-portálon.

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-powershell-setup](../../includes/dns-powershell-setup-include.md)]


## <a name="create-a-dns-zone"></a>DNS-zóna létrehozása

A DNS-zónák hello segítségével hozhatók létre `New-AzureRmDnsZone` parancsmag.

hello alábbi példa létrehoz egy DNS-zónát *contoso.com* nevű hello erőforráscsoportban *MyResourceGroup*:

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

hello következő példa bemutatja, hogyan toocreate DNS zónát, és két [Azure Resource Manager címkéket](dns-zones-records.md#tags), *project = demo* és *env = test*:

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

## <a name="get-a-dns-zone"></a>A DNS-zóna beolvasása

egy DNS-zóna tooretrieve hello használata `Get-AzureRmDnsZone` parancsmag. Ez a művelet adja vissza, a DNS-zónák objektum megfelelő tooan meglévő zóna Azure DNS-ben. hello objektum hello zóna (például hello rekordkészletek) kapcsolatos adatokat tartalmaz, de nem tartalmaz maguk rekordhalmazok hello (lásd: `Get-AzureRmDnsRecordSet`).

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

## <a name="list-dns-zones"></a>Lista DNS-zónák

Hello zóna nevét a kihagyásával őriznek `Get-AzureRmDnsZone`, minden zóna erőforráscsoportban enumerálása. Ez a művelet a zóna objektumok tömbjét adja vissza.

```powershell
$zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup
```

Hello zóna neve és a hello erőforráscsoport-név a kihagyásával őriznek `Get-AzureRmDnsZone`, minden zónákat az Azure-előfizetés hello enumerálása.

```powershell
$zoneList = Get-AzureRmDnsZone
```

## <a name="update-a-dns-zone"></a>A DNS-zóna frissítéséhez

Módosítja a DNS-zóna erőforrásrekordok használatával tehető tooa `Set-AzureRmDnsZone`. Ez a parancsmag nem frissíti a hello DNS-rekordhalmazok hello zónán belül (lásd: [hogyan tooManage DNS-rekordok](dns-operations-recordsets.md)). Hello zóna erőforrás maga tooupdate tulajdonságainak csak használatban van. hello írható zóna tulajdonságai jelenleg korlátozott toohello [Azure Resource Manager "címkék" hello zóna erőforrás](dns-zones-records.md#tags).

A DNS-zónák használja a következő két módon tooupdate hello egyikét:

### <a name="specify-hello-zone-using-hello-zone-name-and-resource-group"></a>Adja meg a hello zóna hello zóna nevét és az erőforrás csoport használata

Ez a megközelítés hello meglévő zóna címkék lecseréli a megadott hello értékek.

```powershell
Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

### <a name="specify-hello-zone-using-a-zone-object"></a>Adjon meg egy $zone objektummal hello zóna

Ez a megközelítés hello meglévő zóna objektum lekéri, hello címkék módosítja, és majd a hello módosítások véglegesítése. Ezzel a módszerrel meglévő címkék megőrizhetők.

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

Használata esetén `Set-AzureRmDnsZone` $zone objektummal [Etag ellenőrzi](dns-zones-records.md#etags) használt tooensure egyidejű változtatások nem íródnak felül. Használhatja a választható hello `-Overwrite` kapcsoló toosuppress ezen ellenőrzések.

## <a name="delete-a-dns-zone"></a>A DNS-zóna törlése

DNS-zónák hello törölhetők `Remove-AzureRmDnsZone` parancsmag.

> [!NOTE]
> A DNS-zónák törlésekor a hello zónán belül minden DNS-rekordokat is törlődnek. Ez a művelet nem vonható vissza. Hello DNS-zóna van használatban, ha hello zóna használó szolgáltatások nem indulnak hello zóna törlésekor.
>
>a zóna véletlen törlése ellen tooprotect lásd: [hogyan tooprotect DNS zónák, valamint megjegyzi](dns-protect-zones-recordsets.md).


A DNS-zónák használja a következő két módon toodelete hello egyikét:

### <a name="specify-hello-zone-using-hello-zone-name-and-resource-group-name"></a>Adja meg a hello zóna hello zóna neve és az erőforráscsoport neve

```powershell
Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

### <a name="specify-hello-zone-using-a-zone-object"></a>Adjon meg egy $zone objektummal hello zóna

Hello zóna toobe törölni is megadhat egy `$zone` által visszaadott objektum `Get-AzureRmDnsZone`.

```powershell
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
Remove-AzureRmDnsZone -Zone $zone
```

hello zóna objektum is átirányítható paraméterként átadott helyett:

```powershell
Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone

```

A `Set-AzureRmDnsZone`, megadó hello zóna használatával egy `$zone` objektum lehetővé teszi, hogy Etag ellenőrzi, hogy tooensure egyidejű változtatások nem törlődnek. Használjon hello `-Overwrite` kapcsoló toosuppress ezen ellenőrzések.

## <a name="confirmation-prompts"></a>Megerősítés

Hello `New-AzureRmDnsZone`, `Set-AzureRmDnsZone`, és `Remove-AzureRmDnsZone` parancsmagok minden megerősítés támogatja.

Mindkét `New-AzureRmDnsZone` és `Set-AzureRmDnsZone` megerősítés kérése, ha hello `$ConfirmPreference` PowerShell preferenciaváltozót értéke `Medium` vagy alacsonyabb. Lejáró toohello potenciálisan nagy jelentőségű törli a DNS-zónák hello `Remove-AzureRmDnsZone` parancsmag kell megerősítést kérni fogja, ha hello `$ConfirmPreference` PowerShell változó értéke bármely eltérő `None`.

Hello alapértelmezett értéke óta `$ConfirmPreference` van `High`, csak `Remove-AzureRmDnsZone` alapértelmezés szerint rákérdez.

Felülbírálhatja hello aktuális `$ConfirmPreference` hello használata beállítást `-Confirm` paraméter. Ha megad `-Confirm` vagy `-Confirm:$True` , hello parancsmag megerősítést kér, mielőtt futtatja. Ha megad `-Confirm:$False` , hello parancsmag nem figyelmeztet megerősítést kér.

További információ `-Confirm` és `$ConfirmPreference`, lásd: [kapcsolatos Preferenciaváltozók](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).

## <a name="next-steps"></a>Következő lépések

Ismerje meg, hogyan túl[rekordhalmazokat és rekordokat kezelése](dns-operations-recordsets.md) a DNS-zónában.
<br>
Ismerje meg, hogyan túl[delegálása a tartományi tooAzure DNS](dns-domain-delegation.md).
<br>
Felülvizsgálati hello [Azure DNS PowerShell referenciadokumentációt](/powershell/module/azurerm.dns).

