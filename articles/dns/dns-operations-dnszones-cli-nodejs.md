---
title: "az Azure CLI 1.0 - az Azure DNS-zónák aaaManage DNS |} Microsoft Docs"
description: "DNS-zónák Azure CLI 1.0 használatával kezelheti. Ez a cikk bemutatja, hogyan tooupdate, törlése és a DNS-zóna létrehozása az Azure DNS szolgáltatásra."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 8ab63bc4-5135-4ed8-8c0b-5f0712b9afed
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/21/2016
ms.author: gwallace
ms.openlocfilehash: cb9790cc46626ef7f38a43edb57511104fe6057e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-dns-zones-in-azure-dns-using-hello-azure-cli-10"></a>Hogyan toomanage DNS-zónák az Azure DNS használatával hello Azure CLI 1.0

> [!div class="op_single_selector"]
> * [Portál](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)

Ez az útmutató bemutatja, hogyan toomanage a DNS-zóna számára elérhető a Windows, Mac és Linux hello platformfüggetlen 1.0, az Azure CLI segítségével. Emellett kezelhetők a DNS-zónák használatával [Azure PowerShell](dns-operations-dnszones.md) vagy hello Azure-portálon.

## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat

Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:

* [Az Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) -hello klasszikus és resource management üzembe helyezési modellel a parancssori felületen.
* [Az Azure CLI 2.0](dns-operations-dnszones-cli.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell.

## <a name="introduction"></a>Bevezetés

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-cli-setup](../../includes/dns-cli-setup-include.md)]

## <a name="getting-help"></a>Segítségkérés

TooAzure DNS vonatkozó összes CLI 1.0 parancsok kezdődnie `azure network dns`. Minden egyes parancsnál hello segítségével érhető el súgó `--help` beállítás (rövid alak `-h`).  Példa:

```azurecli
azure network dns -h
azure network dns zone -h
azure network dns zone create -h
```

## <a name="create-a-dns-zone"></a>DNS-zóna létrehozása

A DNS-zóna létrehozása hello használatával `azure network dns zone create` parancsot. További segítségért lásd: `azure network dns zone create -h`.

hello alábbi példa létrehoz egy DNS-zónát *contoso.com* nevű hello erőforráscsoportban *MyResourceGroup*:

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```

### <a name="toocreate-a-dns-zone-with-tags"></a>egy DNS-zónát címkékkel toocreate

hello következő példa bemutatja, hogyan toocreate DNS zónát, és két [Azure Resource Manager címkéket](dns-zones-records.md#tags), *project = demo* és *env = test*, hello segítségével `--tags` a paraméter (rövid alak `-t`):

```azurecli
azure network dns zone create MyResourceGroup contoso.com -t "project=demo";"env=test"
```

## <a name="get-a-dns-zone"></a>A DNS-zóna beolvasása

használja a DNS-zónák tooretrieve `azure network dns zone show`. További segítségért lásd: `azure network dns zone show -h`.

hello következő példa eredménye hello DNS-zóna *contoso.com* és kapcsolódó adataik erőforráscsoportból *MyResourceGroup*. 

```azurecli
azure network dns zone show MyResourceGroup contoso.com
```

a következő példa hello hello válasz.

```
info:    Executing command network dns zone show
+ Looking up hello dns zone "contoso.com"
data:    Id                              : /subscriptions/.../contoso.com
data:    Name                            : contoso.com
data:    Type                            : Microsoft.Network/dnszones
data:    Location                        : global
data:    Number of record sets           : 2
data:    Max number of record sets       : 5000
data:    Name servers:
data:        ns1-01.azure-dns.com.
data:        ns2-01.azure-dns.net.
data:        ns3-01.azure-dns.org.
data:        ns4-01.azure-dns.info.
data:    Tags                            : project=demo;env=test
info:    network dns zone show command OK
```

Megjegyzés: a DNS-rekordokat nem által visszaadott `azure network dns zone show`. toolist DNS-rekordokat, használjon `azure network dns record-set list`.


## <a name="list-dns-zones"></a>Lista DNS-zónák

DNS-zónák tooenumerate, használjon `azure network dns zone list`. További segítségért lásd: `azure network dns zone list -h`.

Megadását hello erőforráscsoport hello erőforráscsoporton belül zónák sorolja fel:

```azurecli
azure network dns zone list MyResourceGroup
```

Hello erőforráscsoport kihagyásával hello előfizetés minden zóna tartalmazza:

```azurecli
azure network dns zone list 
```

## <a name="update-a-dns-zone"></a>A DNS-zóna frissítéséhez

DNS-zóna erőforrásrekordok végezhetők változások tooa `azure network dns zone set`. További segítségért lásd: `azure network dns zone set -h`.

Ez a parancs frissíti hello DNS-rekordhalmazok hello zónán belül (lásd: [hogyan tooManage DNS-rekordok](dns-operations-recordsets-cli-nodejs.md)). Csak használt tooupdate tulajdonságok hello zóna erőforrás maga is. Ezek a tulajdonságok jelenleg korlátozott toohello [Azure Resource Manager "címke"](dns-zones-records.md#tags) hello zóna erőforrás.

hello következő példa bemutatja, hogyan tooupdate hello címkéket a DNS-zóna. hello meglévő címkék helyébe hello érték van megadva.

```azurecli
azure network dns zone set MyResourceGroup contoso.com -t "team=support"
```

## <a name="delete-a-dns-zone"></a>A DNS-zóna törlése

DNS-zónák törölhetők segítségével `azure network dns zone delete`. További segítségért lásd: `azure network dns zone delete -h`.

> [!NOTE]
> A DNS-zónák törlésekor a hello zónán belül minden DNS-rekordokat is törlődnek. Ez a művelet nem vonható vissza. Hello DNS-zóna van használatban, ha hello zóna használó szolgáltatások nem indulnak hello zóna törlésekor.
>
>a zóna véletlen törlése ellen tooprotect lásd: [hogyan tooprotect DNS zónák, valamint megjegyzi](dns-protect-zones-recordsets.md).

Ez a parancs felszólítja megerősítést kér. nem kötelező hello `--quiet` váltás (rövid alak `-q`) mellőzi a parancssorhoz.

hello következő példa bemutatja, hogyan toodelete hello zóna *contoso.com* erőforráscsoportból *MyResourceGroup*.

```azurecli
azure network dns zone delete MyResourceGroup contoso.com
```

## <a name="next-steps"></a>Következő lépések

Ismerje meg, hogyan túl[rekordhalmazokat és rekordokat kezelése](dns-getstarted-create-recordset-cli-nodejs.md) a DNS-zónában.

Ismerje meg, hogyan túl[delegálása a tartományi tooAzure DNS](dns-domain-delegation.md).

