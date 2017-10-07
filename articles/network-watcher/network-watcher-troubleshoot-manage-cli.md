---
title: "aaaTroubleshoot Azure virtuális hálózati átjáró és a kapcsolatok - Azure CLI 2.0 |} Microsoft Docs"
description: "Ez a lap azt ismerteti, hogyan toouse hello Azure hálózati figyelőt hibaelhárítása Azure CLI 2.0"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 2838bc61-b182-4da8-8533-27db8fdbd177
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: gwallace
ms.openlocfilehash: 389e86f247722412a1d0e2e722dbf80bb7cff291
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-azure-cli-20"></a>Virtuális hálózati átjáró és az Azure hálózati figyelő Azure CLI 2.0 verziót használja kapcsolatok hibáinak elhárítása

> [!div class="op_single_selector"]
> - [Portál](network-watcher-troubleshoot-manage-portal.md)
> - [PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [CLI 1.0](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-troubleshoot-manage-cli.md)
> - [REST API](network-watcher-troubleshoot-manage-rest.md)

Hálózati figyelőt számos lényeges képességét biztosítja, felügyeletében toounderstanding a hálózati erőforrások az Azure-ban. Ezek a képességek egyik erőforrás hibaelhárítás. Erőforrások hibaelhárítása hívható hello portál, a PowerShell, a CLI vagy a REST API-n keresztül. Meghívásakor, a hálózati figyelőt megvizsgálja a virtuális hálózati átjáró vagy a kapcsolat hello állapotát, és adja vissza az eredményekről.

Ez a cikk használja a következő generációs CLI hello erőforrás management üzembe helyezési modelljével Azure CLI 2.0, elérhető a Windows, Mac és Linux.

tooperform hello ebben a cikkben ismertetett visszaállítási lépésekkel, túl kell[hello Azure parancssori felület Mac, Linux és Windows (Azure CLI) telepítése](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

## <a name="before-you-begin"></a>Előkészületek

Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.

Látogasson el a támogatott átjáró típusok, listája [támogatott átjárótípusok](network-watcher-troubleshoot-overview.md#supported-gateway-types).

## <a name="overview"></a>Áttekintés

Erőforrás hibakeresési hello lehetőséget nyújt a virtuális hálózati átjárók és kapcsolatok felmerülő problémák hibaelhárításához. A kérelem tooresource hibaelhárítása, amikor folyamatban van-e a naplók lekérdezett és megvizsgálni. Ha vizsgálat befejeződött, hello eredmény akkor minősül. Erőforrás-hibaelhárítási kérelmek hosszú ideig futniuk kér, ami eltarthat több percig tooreturn eredményt. a hibaelhárítás hello naplófájljainak tárolása a megadott tárfiók egy tárolót.

## <a name="retrieve-a-virtual-network-gateway-connection"></a>A virtuális hálózati átjáró kapcsolat beolvasása

Ebben a példában erőforrás hibakeresési van folyamatban futtatott egy kapcsolatot. Is átadhatja azt a virtuális hálózati átjáró. hello következő parancsmag listája hello vpn-kapcsolatok erőforráscsoportban.

```azurecli
az network vpn-connection list --resource-group resourceGroupName
```

Miután hello hello kapcsolat neve, futtathatja az erőforrás-azonosítót a parancs tooget:

```azurecli
az network vpn-connection show --resource-group resourceGroupName --ids vpnConnectionIds
```

## <a name="create-a-storage-account"></a>Create a storage account

Hello hello erőforrás állapotának adatait erőforrás hibakeresési adja vissza, naplók tooa tárolási fiók toobe felül is menti. Ebben a lépésben létrehozhatunk egy tárfiókot, ha létezik-e egy meglévő tárfiók használata.

1. Hello storage-fiók létrehozása

    ```azurecli
    az storage account create --name storageAccountName --location westcentralus --resource-group resourceGroupName --sku Standard_LRS
    ```

1. Hello tárfiókkulcsok beolvasása

    ```azurecli
    az storage account keys list --resource-group resourcegroupName --account-name storageAccountName
    ```

1. Hello tároló létrehozása

    ```azurecli
    az storage container create --account-name storageAccountName --account-key {storageAccountKey} --name logs
    ```

## <a name="run-network-watcher-resource-troubleshooting"></a>Futtassa a hálózati figyelőt erőforrás hibaelhárítása

Hibaelhárítás hello erőforrások `az network watcher troubleshooting` parancsmag. Hello parancsmag hello erőforráscsoport továbbítja azt, hello hello hálózati figyelőt, hello hello kapcsolat azonosítója neve hello hello tárfiók azonosítója és hello elérési toohello blob toostore hello hibaelhárításához eredményez.

```azurecli
az network watcher troubleshooting start --resource-group resourceGroupName --resource resourceName --resource-type {vnetGateway/vpnConnection} --storage-account storageAccountName  --storage-path https://{storageAccountName}.blob.core.windows.net/{containerName}
```

Hello parancsmag futtatása után hálózati figyelőt ellenőrzi, hogy hello erőforrás tooverify hello állapota. Toohello rendszerhéj hello eredményeket ad vissza, és hello eredmények naplók megadott hello tárfiók tárolja.

## <a name="understanding-hello-results"></a>Hello eredmények ismertetése

hello művelet szöveg hogyan tooresolve hello probléma nyújt általános útmutatást. Is művelet az hello probléma, ha egy hivatkozás által biztosított további útmutatást. Hello esetében nincs további útmutatás, ahol hello választ biztosít hello URL-cím tooopen támogatási esetet.  Hello választ, és mi tartozik hello tulajdonságainak kapcsolatos további információkért látogasson el [hálózati figyelő hibaelhárítása – áttekintés](network-watcher-troubleshoot-overview.md)

A fájlok letöltését az azure storage-fiókok útmutatásért tekintse meg túl[az Azure Blob storage .NET használatának első lépései](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Egy másik eszköz, amely használható a Tártallózó. Tártallózó további információt itt található: a következő hivatkozás hello: [Tártallózó](http://storageexplorer.com/)

## <a name="next-steps"></a>Következő lépések

Ha a beállítások módosítása, hogy stop VPN-kapcsolatot, lásd: [hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md) hello hálózati biztonsági csoport és a biztonsági szabályokat, amelyek lehet, hogy a szóban forgó tootrack.
