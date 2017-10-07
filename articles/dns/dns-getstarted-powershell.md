---
title: "aaaGet lépések az Azure DNS PowerShell-lel |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate DNS zóna, illetve az Azure DNS-rekord. Ez egy részletes útmutató toocreate és kezelése az első DNS-zónát, és rögzítse a PowerShell használatával."
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: jonatul
ms.openlocfilehash: 0f9dead1e4b44fcc74c84a024c41cdfaeb02b5d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-dns-using-powershell"></a>Az Azure DNS PowerShell-lel való használatának első lépései

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Azure CLI 1.0](dns-getstarted-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-cli.md)

Ez a cikk bemutatja, hogyan hello lépéseket toocreate az első DNS-zónát és az Azure PowerShell rögzítése. A következő lépésekkel hello Azure-portál használatával vagy platformok közötti Azure CLI hello is.

A DNS-zónák használt toohost hello DNS-rekordok az adott tartományban. az Azure DNS, a tartomány toostart toocreate DNS-zóna van szüksége a tartománynevet. Ezután a tartománya összes DNS-rekordja ebben a DNS-zónában jön létre. Végezetül toopublish a DNS-zónák toohello Internet, tooconfigure hello névkiszolgálók hello tartomány van szüksége. Az egyes lépéseket az alábbiakban ismertetjük.

Ezek az utasítások azt feltételezik, ha már telepítette, és tooAzure PowerShell bejelentkezett. Útmutatásért lásd: [hogyan toomanage DNS zónák PowerShell-lel](dns-operations-dnszones.md).

## <a name="create-hello-resource-group"></a>Hello erőforráscsoport létrehozása

Mielőtt létrehozna hello DNS-zónát, egy erőforráscsoportot toocontain hello DNS-zóna jön létre. hello következő hello parancs jeleníti meg.

```powershell
New-AzureRMResourceGroup -name MyResourceGroup -location "westus"
```

## <a name="create-a-dns-zone"></a>DNS-zóna létrehozása

A DNS-zónák hello segítségével hozhatók létre `New-AzureRmDnsZone` parancsmag. hello alábbi példa létrehoz egy DNS-zónát *contoso.com* nevű hello erőforráscsoportban *MyResourceGroup*. Hello példa toocreate egy DNS-zónát és hello értékeket a saját használja.

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyResourceGroup
```

## <a name="create-a-dns-record"></a>DNS-rekord létrehozása

Rekordhalmazok hello segítségével hozhatja létre `New-AzureRmDnsRecordSet` parancsmag. hello alábbi példa létrehoz egy rekordot hello relatív névvel "www" hello "contoso.com", az erőforráscsoportban "Contoso.com" DNS-zónát. hello rekordhalmaz hello teljesen minősített neve "www.contoso.com". hello rekordtípus "A", "1.2.3.4" IP-címmel, pedig hello élettartam 3600 másodperc.

```powershell
New-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName contoso.com -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4")
```

Egyéb típusú bejegyzés a rekordhalmazok egynél több rekordot, és toomodify meglévő rekordokat, lásd: [kezelése DNS-rekordok és a rekordhalmazok az Azure PowerShell](dns-operations-recordsets.md). 


## <a name="view-records"></a>A rekordok megtekintése

toolist hello DNS-rekordokat, amelyek a zónához használja:

```powershell
Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyResourceGroup
```


## <a name="update-name-servers"></a>A névkiszolgálók frissítése

Ha mindent megfelelőnek talált, hogy a DNS-zóna és a rekordok beállított megfelelően, tooconfigure van szüksége a tartománynév toouse hello Azure DNS névkiszolgálóit. Ez lehetővé teszi a más felhasználóktól a hello Internet toofind a DNS-rekordokat.

Adja meg a zóna névkiszolgálóit hello hello `Get-AzureRmDnsZone` parancsmagot:

```powershell
Get-AzureRmDnsZone -ZoneName contoso.com -ResourceGroupName MyResourceGroup

Name                  : contoso.com
ResourceGroupName     : myresourcegroup
Etag                  : 00000003-0000-0000-b40d-0996b97ed101
Tags                  : {}
NameServers           : {ns1-01.azure-dns.com., ns2-01.azure-dns.net., ns3-01.azure-dns.org., ns4-01.azure-dns.info.}
NumberOfRecordSets    : 3
MaxNumberOfRecordSets : 5000
```

Ezeket a kiszolgálókat (vásárolta hello tartománynevet) hello regisztrációs kell konfigurálni. A regisztráló felajánlja, hello beállítás tooset hello neve kiszolgáló hello tartományhoz. További információkért lásd: [delegálása a tartományi tooAzure DNS](dns-domain-delegation.md).

## <a name="delete-all-resources"></a>Az összes erőforrás törlése

toodelete ebben a cikkben a következő lépés hajtsa végre a megfelelő hello létrehozott összes erőforrás:

```powershell
Remove-AzureRMResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a>Következő lépések

További információ az Azure DNS-beli toolearn lásd [Azure DNS áttekintése](dns-overview.md).

További információ az Azure DNS-, DNS-zónák kezelése toolearn lásd [kezelése DNS-zónák az Azure DNS PowerShell-lel](dns-operations-dnszones.md).

További információ az Azure DNS-, DNS-rekordok kezelése toolearn lásd [kezelése DNS-rekordok és a rekord beállítja az Azure DNS PowerShell-lel](dns-operations-recordsets.md).

