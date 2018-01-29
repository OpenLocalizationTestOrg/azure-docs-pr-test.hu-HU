---
title: "Az Azure virtuális hálózati átjáró és a kapcsolatok - Azure CLI 1.0 hibaelhárítása |} Microsoft Docs"
description: "Ez a lap ismerteti, hogyan Azure hálózati figyelőt Azure CLI 1.0 hibaelhárítása"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 2838bc61-b182-4da8-8533-27db8fdbd177
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: jdial
ms.openlocfilehash: 2acbc47970acf0eb2aa1aea8535d7157bc73cbb6
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/21/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-azure-cli-10"></a>Virtuális hálózati átjáró és az Azure hálózati figyelő Azure CLI 1.0 használatával kapcsolatok hibáinak elhárítása

> [!div class="op_single_selector"]
> - [Portal](network-watcher-troubleshoot-manage-portal.md)
> - [PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [CLI 1.0](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-troubleshoot-manage-cli.md)
> - [REST API](network-watcher-troubleshoot-manage-rest.md)

Hálózati figyelőt sok képességeket biztosít, a hálózati erőforrások az Azure-ban ismertetése vonatkozik. Ezek a képességek egyik erőforrás hibaelhárítás. Erőforrások hibaelhárítása hívható a portálon, a PowerShell, a CLI vagy a REST API-n keresztül. Meghívásakor, a hálózati figyelőt megvizsgálja a virtuális hálózati átjáró vagy a kapcsolat állapotát, és visszaadja az eredményekről.

Ebben a cikkben az Azure CLI 1.0 platformfüggetlen, elérhető a Windows, Mac és Linux. 

## <a name="before-you-begin"></a>Előkészületek

Ez a forgatókönyv azt feltételezi, hogy már követte lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) létrehozása egy hálózati figyelőt.

Látogasson el a támogatott átjáró típusok, listája [támogatott átjárótípusok](/network-watcher-troubleshoot-overview.md#supported-gateway-types).

## <a name="overview"></a>Áttekintés

Erőforrás hibakeresési lehetővé teszi a virtuális hálózati átjárók és kapcsolatok felmerülő problémák hibaelhárításához. A kérelem erőforrás hibáinak elhárításához, amikor folyamatban van-e a naplók lekérdezett és megvizsgálni. Ha vizsgálat befejeződött, a rendszer visszairányítja az eredményeket. Erőforrás-hibaelhárítási kérelmek hosszú ideig futniuk kér, amely több percig is beletelhet visszaküldeni az eredményt. A naplók hibaelhárítási egy tárolót, a megadott tárfiók tárolja.

## <a name="retrieve-a-virtual-network-gateway-connection"></a>A virtuális hálózati átjáró kapcsolat beolvasása

Ebben a példában erőforrás hibakeresési van folyamatban futtatott egy kapcsolatot. Is átadhatja azt a virtuális hálózati átjáró. A következő parancsmagot a vpn-kapcsolatok a erőforráscsoport sorolja fel.

```azurecli
azure network vpn-connection list -g resourceGroupName
```

Futtathatja a parancsot egy előfizetésben kapcsolatok megtekintéséhez.

```azurecli
azure network vpn-connection list -s subscription
```

Miután a kapcsolat neve, futtathatja a parancsot az erőforrás-azonosítót:

```azurecli
azure network vpn-connection show -g resourceGroupName -n connectionName
```

## <a name="create-a-storage-account"></a>Create a storage account

Erőforrás hibakeresési erőforrás állapotával kapcsolatos adatokat ad vissza, a naplók tárfiókba vizsgálni is menti. Ebben a lépésben létrehozhatunk egy tárfiókot, ha létezik-e egy meglévő tárfiók használata.

1. A storage-fiók létrehozása

    ```azurecli
    azure storage account create -n storageAccountName -l location -g resourceGroupName
    ```

1. A tárfiók kulcsait beolvasása

    ```azurecli
    azure storage account keys list storageAccountName -g resourcegroupName
    ```

1. A tároló létrehozása

    ```azurecli
    azure storage container create --account-name storageAccountName -g resourcegroupName --account-key {storageAccountKey} --container logs
    ```

## <a name="run-network-watcher-resource-troubleshooting"></a>Futtassa a hálózati figyelőt erőforrás hibaelhárítása

Hibaelhárítás az erőforrások a `network watcher troubleshoot` parancsmag. Azt át a parancsmag az erőforráscsoportot, a hálózati figyelőt, a kapcsolat, a tárfiók Id azonosítója neve és elérési útját a hibaelhárítás tárolni a blob eredményez.

```azurecli
azure network watcher troubleshoot -g resourceGroupName -n networkWatcherName -t connectionId -i storageId -p storagePath
```

A parancsmag futtatása után hálózati figyelőt ellenőrzi, hogy az erőforrás állapotát ellenőrizni. Az eredményt ad vissza. a rendszerhéj, és az eredmények naplók a megadott tárfiók tárolja.

## <a name="understanding-the-results"></a>Az eredmények ismertetése

A művelet szöveg általános útmutatást biztosít a probléma megoldására. Is művelet az a probléma, ha egy hivatkozás által biztosított további útmutatást. Abban az esetben nincs további útmutatás, ha a válasz biztosít nyissa meg a támogatási esetet URL-címét.  A válasz és tartalmát képező tulajdonságaival kapcsolatos további információkért látogasson el a [hálózati figyelő hibaelhárítása – áttekintés](network-watcher-troubleshoot-overview.md)

A fájlok letöltését az azure storage-fiókok útmutatásért tekintse meg [az Azure Blob storage .NET használatának első lépései](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Egy másik eszköz, amely használható a Tártallózó. Tártallózó további információt itt található: a következő hivatkozásra: [Tártallózó](http://storageexplorer.com/)

## <a name="next-steps"></a>További lépések

Ha a beállítások módosítása, hogy stop VPN-kapcsolatot, lásd: [hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md) nyomon követheti a hálózati biztonsági csoport és a biztonsági szabályok, amelyek lehet, hogy a szóban forgó.
