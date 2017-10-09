---
title: "aaaGet Azure DNS használata az Azure CLI 1.0-s használatába |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate DNS zóna, illetve az Azure DNS-rekord. Ez egy részletes útmutató toocreate, és kezeli az első DNS-zóna és a rekord hello Azure CLI 1.0 használatával."
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
ms.openlocfilehash: e200c848ad261160e593306dbb8a1d92bf26880b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-dns-using-azure-cli-10"></a>Az Azure DNS használatának első lépései az Azure CLI 1.0-val

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Azure CLI 1.0](dns-getstarted-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-cli.md)

Ez a cikk bemutatja, hogyan hello lépéseket toocreate az első DNS-zónát, és rekord segítségével hello Azure CLI 1.0 platformfüggetlen, elérhető a Windows, Mac és Linux. Ezeket a lépéseket hello Azure-portálon vagy az Azure PowerShell használatával is elvégezheti.

A DNS-zónák használt toohost hello DNS-rekordok az adott tartományban. az Azure DNS, a tartomány toostart toocreate DNS-zóna van szüksége a tartománynevet. Ezután a tartománya összes DNS-rekordja ebben a DNS-zónában jön létre. Végezetül toopublish a DNS-zónák toohello Internet, tooconfigure hello névkiszolgálók hello tartomány van szüksége. Az egyes lépéseket az alábbiakban ismertetjük.

Ezek az utasítások azt feltételezik, ha már telepítette, és tooAzure CLI 1.0 bejelentkezve. Útmutatásért lásd: [hogyan toomanage DNS zónák használata az Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md).

## <a name="create-hello-resource-group"></a>Hello erőforráscsoport létrehozása

Mielőtt létrehozna hello DNS-zónát, egy erőforráscsoportot toocontain hello DNS-zóna jön létre. hello következő hello parancs jeleníti meg.

```azurecli
azure group create --name MyResourceGroup --location "West US"
```

## <a name="create-a-dns-zone"></a>DNS-zóna létrehozása

A DNS-zóna létrehozása hello használatával `azure network dns zone create` parancsot. a parancs toosee súgójának, írja be `azure network dns zone create -h`.

hello alábbi példa létrehoz egy DNS-zónát *contoso.com* nevű hello erőforráscsoportban *MyResourceGroup*. Hello példa toocreate egy DNS-zónát és hello értékeket a saját használja.

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```


## <a name="create-a-dns-record"></a>DNS-rekord létrehozása

egy DNS-rekord toocreate hello használata `azure network dns record-set add-record` parancsot. További segítségért lásd: `azure network dns record-set add-record -h`.

hello alábbi példa létrehoz egy rekordot hello relatív névvel "www" hello "contoso.com", az erőforráscsoportban "Contoso.com" DNS-zónát. hello rekordhalmaz hello teljesen minősített neve "www.contoso.com". hello rekordtípus "A", "1.2.3.4" IP-címmel, és egy alapértelmezett élettartam 3600 másodperc (1 óra) használt.

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

Egyéb típusú bejegyzés a rekordhalmazok alternatív TTL értékeket, és toomodify meglévő rekordokat, a több rekordot tartalmazó lásd: [kezelése DNS-rekordok és a rekordhalmazok használatával hello Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md).


## <a name="view-records"></a>A rekordok megtekintése

toolist hello DNS-rekordokat, amelyek a zónához használja:

```azurecli
azure network dns record-set list MyResourceGroup contoso.com
```


## <a name="update-name-servers"></a>A névkiszolgálók frissítése

Ha mindent megfelelőnek talált, hogy a DNS-zóna és a rekordok beállított megfelelően, tooconfigure van szüksége a tartománynév toouse hello Azure DNS névkiszolgálóit. Ez lehetővé teszi a más felhasználóktól a hello Internet toofind a DNS-rekordokat.

Adja meg a zóna névkiszolgálóit hello hello `azure network dns zone show` parancs:

```azurecli
azure network dns zone show MyResourceGroup contoso.com

info:    Executing command network dns zone show
+ Looking up hello dns zone "contoso.com"
data:    Id                              : /subscriptions/a385a691-bd93-41b0-8084-8213ebc5bff7/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com
data:    Name                            : contoso.com
data:    Type                            : Microsoft.Network/dnszones
data:    Location                        : global
data:    Number of record sets           : 3
data:    Max number of record sets       : 5000
data:    Name servers:
data:        ns1-01.azure-dns.com.
data:        ns2-01.azure-dns.net.
data:        ns3-01.azure-dns.org.
data:        ns4-01.azure-dns.info.
data:    Tags                            :
info:    network dns zone show command OK
```

Ezeket a kiszolgálókat (vásárolta hello tartománynevet) hello regisztrációs kell konfigurálni. A regisztráló felajánlja, hello beállítás tooset hello neve kiszolgáló hello tartományhoz. További információkért lásd: [delegálása a tartományi tooAzure DNS](dns-domain-delegation.md).

## <a name="delete-all-resources"></a>Az összes erőforrás törlése
 
toodelete ebben a cikkben a következő lépés hajtsa végre a megfelelő hello létrehozott összes erőforrás:

```azurecli
azure group delete --name MyResourceGroup
```

## <a name="next-steps"></a>Következő lépések

További információ az Azure DNS-beli toolearn lásd [Azure DNS áttekintése](dns-overview.md).

További információ az Azure DNS-, DNS-zónák kezelése toolearn lásd [kezelése DNS-zónák az Azure DNS használata az Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md).

További információ az Azure DNS-, DNS-rekordok kezelése toolearn lásd [kezelése DNS-rekordok és a rekord beállítja az Azure DNS használata az Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md).

