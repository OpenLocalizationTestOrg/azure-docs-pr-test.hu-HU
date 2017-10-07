---
title: "aaaManage DNS-zónák az Azure DNS - Azure CLI 2.0 |} Microsoft Docs"
description: "Kezelheti a DNS-zónákat az Azure CLI 2.0 verziót használja. Ez a cikk bemutatja, hogyan tooupdate, törlése és a DNS-zóna létrehozása az Azure DNS szolgáltatásra."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 8ab63bc4-5135-4ed8-8c0b-5f0712b9afed
ms.service: dns
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: gwallace
ms.openlocfilehash: 3945a558b2db3490e50678d8395a47e55a85c8fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-dns-zones-in-azure-dns-using-hello-azure-cli-20"></a>Hogyan toomanage DNS-zónák az Azure DNS használatával hello Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Portál](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)


Ez az útmutató bemutatja, hogyan toomanage a DNS-zónák használatával hello platformfüggetlen Azure CLI Windows, Mac és Linux elérhető. Emellett kezelhetők a DNS-zónák használatával [Azure PowerShell](dns-operations-dnszones.md) vagy hello Azure-portálon.

## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat

Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:

* [Az Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) -hello klasszikus és resource management üzembe helyezési modellel a parancssori felületen.
* [Az Azure CLI 2.0](dns-operations-dnszones-cli.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell.

## <a name="introduction"></a>Bevezetés

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="set-up-azure-cli-20-for-azure-dns"></a>Az Azure CLI 2.0 beállítása az Azure DNS-hez

### <a name="before-you-begin"></a>Előkészületek

Győződjön meg arról, hogy rendelkezik-e elemek a konfigurációs megkezdése előtt a következő hello.

* Azure-előfizetés. Ha még nincs Azure-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), vagy regisztrálhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).

* Hello hello Azure CLI 2.0-t, Windows, Linux és Mac legújabb verziójának telepítése További információt [telepítés hello Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

### <a name="sign-in-tooyour-azure-account"></a>Jelentkezzen be tooyour Azure-fiók

Nyisson meg egy konzolablakot, adja meg a saját hitelesítő adatait. További információkért tekintse meg a naplót a tooAzure a hello Azure parancssori felület

```
az login
```

### <a name="select-hello-subscription"></a>Válassza ki a hello előfizetés

Hello előfizetések hello fiók ellenőrzése.

```
az account list
```

Válassza ki, amely az Azure-előfizetések toouse.

```azurecli
az account set --subscription "subscription name"
```

### <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet. Ez az erőforráscsoport erőforrások hello alapértelmezett helye szolgál. Azonban mivel minden DNS-erőforrás globális, nem pedig regionális, hello választás az erőforráscsoport helye nincs hatással van az Azure DNS szolgáltatásra.

Ezt a lépést kihagyhatja, ha egy meglévő erőforráscsoportot használ.

```azurecli
az group create --name myresourcegroup --location "West US"
```

## <a name="getting-help"></a>Segítségkérés

TooAzure DNS vonatkozó összes CLI 2.0 parancsok kezdődnie `az network dns`. Minden egyes parancsnál hello segítségével érhető el súgó `--help` beállítás (rövid alak `-h`).  Példa:

```azurecli
az network dns --help
az network dns zone --help
az network dns zone create --help
```

## <a name="create-a-dns-zone"></a>DNS-zóna létrehozása

A DNS-zóna létrehozása hello használatával `az network dns zone create` parancsot. További segítségért lásd: `az network dns zone create -h`.

hello alábbi példa létrehoz egy DNS-zónát *contoso.com* nevű hello erőforráscsoportban *MyResourceGroup*:

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com
```

### <a name="toocreate-a-dns-zone-with-tags"></a>egy DNS-zónát címkékkel toocreate

hello következő példa bemutatja, hogyan toocreate DNS zónát, és két [Azure Resource Manager címkéket](dns-zones-records.md#tags), *project = demo* és *env = test*, hello segítségével `--tags` a paraméter (rövid alak `-t`):

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com --tags "project=demo" "env=test"
```

## <a name="get-a-dns-zone"></a>A DNS-zóna beolvasása

használja a DNS-zónák tooretrieve `az network dns zone show`. További segítségért lásd: `az network dns zone show --help`.

hello következő példa eredménye hello DNS-zóna *contoso.com* és kapcsolódó adataik erőforráscsoportból *MyResourceGroup*. 

```azurecli
az network dns zone show --resource-group myresourcegroup --name contoso.com
```

a következő példa hello hello válasz.

```json
{
  "etag": "00000002-0000-0000-3d4d-64aa3689d201",
  "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "contoso.com",
  "nameServers": [
    "ns1-04.azure-dns.com.",
    "ns2-04.azure-dns.net.",
    "ns3-04.azure-dns.org.",
    "ns4-04.azure-dns.info."
  ],
  "numberOfRecordSets": 4,
  "resourceGroup": "myresourcegroup",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

Megjegyzés: a DNS-rekordokat nem által visszaadott `az network dns zone show`. toolist DNS-rekordokat, használjon `az network dns record-set list`.


## <a name="list-dns-zones"></a>Lista DNS-zónák

DNS-zónák tooenumerate, használjon `az network dns zone list`. További segítségért lásd: `az network dns zone list --help`.

Megadását hello erőforráscsoport hello erőforráscsoporton belül zónák sorolja fel:

```azurecli
az network dns zone list --resource-group MyResourceGroup
```

Hello erőforráscsoport kihagyásával hello előfizetés minden zóna tartalmazza:

```azurecli
az network dns zone list 
```

## <a name="update-a-dns-zone"></a>A DNS-zóna frissítéséhez

DNS-zóna erőforrásrekordok végezhetők változások tooa `az network dns zone update`. További segítségért lásd: `az network dns zone update --help`.

Ez a parancs frissíti hello DNS-rekordhalmazok hello zónán belül (lásd: [hogyan tooManage DNS-rekordok](dns-operations-recordsets-cli.md)). Csak használt tooupdate tulajdonságok hello zóna erőforrás maga is. Ezek a tulajdonságok jelenleg korlátozott toohello [Azure Resource Manager "címke"](dns-zones-records.md#tags) hello zóna erőforrás.

hello következő példa bemutatja, hogyan tooupdate hello címkéket a DNS-zóna. hello meglévő címkék helyébe hello érték van megadva.

```azurecli
az network dns zone update --resource-group myresourcegroup --name contoso.com --set tags.team=support
```

## <a name="delete-a-dns-zone"></a>A DNS-zóna törlése

DNS-zónák törölhetők segítségével `az network dns zone delete`. További segítségért lásd: `az network dns zone delete --help`.

> [!NOTE]
> A DNS-zónák törlésekor a hello zónán belül minden DNS-rekordokat is törlődnek. Ez a művelet nem vonható vissza. Hello DNS-zóna van használatban, ha hello zóna használó szolgáltatások nem indulnak hello zóna törlésekor.
>
>a zóna véletlen törlése ellen tooprotect lásd: [hogyan tooprotect DNS zónák, valamint megjegyzi](dns-protect-zones-recordsets.md).

Ez a parancs felszólítja megerősítést kér. nem kötelező hello `--yes` letiltja a parancssorhoz.

hello következő példa bemutatja, hogyan toodelete hello zóna *contoso.com* erőforráscsoportból *MyResourceGroup*.

```azurecli
az network dns zone delete --resource-group myresourcegroup --name contoso.com
```

## <a name="next-steps"></a>Következő lépések

Ismerje meg, hogyan túl[rekordhalmazokat és rekordokat kezelése](dns-getstarted-create-recordset-cli.md) a DNS-zónában.

Ismerje meg, hogyan túl[delegálása a tartományi tooAzure DNS](dns-domain-delegation.md).

