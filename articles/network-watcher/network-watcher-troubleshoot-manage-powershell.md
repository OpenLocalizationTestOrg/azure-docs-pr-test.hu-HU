---
title: "aaaTroubleshoot Azure virtuális hálózati átjáró és a kapcsolatok - PowerShell |} Microsoft Docs"
description: "Ez a lap azt ismerteti, hogyan toouse hello Azure hálózati figyelőt hibaelhárítása a PowerShell-parancsmag"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: f6f0a813-38b6-4a1f-8cfc-1dfdf979f595
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: gwallace
ms.openlocfilehash: b7dbe6e44e0fa1ea0481223a84098d7a57c068a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-powershell"></a>Virtuális hálózati átjáró és az Azure hálózati figyelő PowerShell kapcsolatok hibáinak elhárítása

> [!div class="op_single_selector"]
> - [Portál](network-watcher-troubleshoot-manage-portal.md)
> - [PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [CLI 1.0](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-troubleshoot-manage-cli.md)
> - [REST API](network-watcher-troubleshoot-manage-rest.md)

Hálózati figyelőt számos lényeges képességét biztosítja, felügyeletében toounderstanding a hálózati erőforrások az Azure-ban. Ezek a képességek egyik erőforrás hibaelhárítás. Erőforrások hibaelhárítása hívható hello portál, a PowerShell, a CLI vagy a REST API-n keresztül. Meghívásakor, a hálózati figyelőt megvizsgálja a virtuális hálózati átjáró vagy a kapcsolat hello állapotát, és adja vissza az eredményekről.

## <a name="before-you-begin"></a>Előkészületek

Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.

Látogasson el a támogatott átjáró típusok, listája [támogatott átjárótípusok](network-watcher-troubleshoot-overview.md#supported-gateway-types).

## <a name="overview"></a>Áttekintés

Erőforrás hibakeresési hello lehetőséget nyújt a virtuális hálózati átjárók és kapcsolatok felmerülő problémák hibaelhárításához. A kérelem tooresource hibaelhárítása, amikor folyamatban van-e a naplók lekérdezett és megvizsgálni. Ha vizsgálat befejeződött, hello eredmény akkor minősül. Erőforrás-hibaelhárítási kérelmek hosszú ideig futniuk kér, ami eltarthat több percig tooreturn eredményt. a hibaelhárítás hello naplófájljainak tárolása a megadott tárfiók egy tárolót.

## <a name="retrieve-network-watcher"></a>Hálózati figyelőt beolvasása

hello első lépéseként tooretrieve hello hálózati figyelőt példány. Hello `$networkWatcher` változó átadása toohello `Start-AzureRmNetworkWatcherResourceTroubleshooting` parancsmag a 4. lépésben.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="retrieve-a-virtual-network-gateway-connection"></a>A virtuális hálózati átjáró kapcsolat beolvasása

Ebben a példában erőforrás hibakeresési van folyamatban futtatott egy kapcsolatot. Is átadhatja azt a virtuális hálózati átjáró.

```powershell
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name "2to3" -ResourceGroupName "testrg"
```

## <a name="create-a-storage-account"></a>Create a storage account

Hello hello erőforrás állapotának adatait erőforrás hibakeresési adja vissza, naplók tooa tárolási fiók toobe felül is menti. Ebben a lépésben létrehozhatunk egy tárfiókot, ha létezik-e egy meglévő tárfiók használata.

```powershell
$sa = New-AzureRmStorageAccount -Name "contosoexamplesa" -SKU "Standard_LRS" -ResourceGroupName "testrg" -Location "WestCentralUS"
Set-AzureRmCurrentStorageAccount -ResourceGroupName $sa.ResourceGroupName -Name $sa.StorageAccountName
$sc = New-AzureStorageContainer -Name logs
```

## <a name="run-network-watcher-resource-troubleshooting"></a>Futtassa a hálózati figyelőt erőforrás hibaelhárítása

Hibaelhárítás hello erőforrások `Start-AzureRmNetworkWatcherResourceTroubleshooting` parancsmag. Azt adja át a hello parancsmag hello hálózati figyelőt objektum, a hello hello kapcsolat azonosítója vagy a virtuális hálózati átjáró, a hello tárfiók azonosítója és a hello elérési toostore hello eredmények.

> [!NOTE]
> Hello `Start-AzureRmNetworkWatcherResourceTroubleshooting` parancsmag hosszú ideig futó és eltarthat néhány percig toocomplete.

```powershell
Start-AzureRmNetworkWatcherResourceTroubleshooting -NetworkWatcher $networkWatcher -TargetResourceId $connection.Id -StorageId $sa.Id -StoragePath "$($sa.PrimaryEndpoints.Blob)$($sc.name)"
```

Hello parancsmag futtatása után hálózati figyelőt ellenőrzi, hogy hello erőforrás tooverify hello állapota. Toohello rendszerhéj hello eredményeket ad vissza, és hello eredmények naplók megadott hello tárfiók tárolja.

## <a name="understanding-hello-results"></a>Hello eredmények ismertetése

hello művelet szöveg hogyan tooresolve hello probléma nyújt általános útmutatást. Is művelet az hello probléma, ha egy hivatkozás által biztosított további útmutatást. Hello esetében nincs további útmutatás, ahol hello választ biztosít hello URL-cím tooopen támogatási esetet.  Hello választ, és mi tartozik hello tulajdonságainak kapcsolatos további információkért látogasson el [hálózati figyelő hibaelhárítása – áttekintés](network-watcher-troubleshoot-overview.md)

A fájlok letöltését az azure storage-fiókok útmutatásért tekintse meg túl[az Azure Blob storage .NET használatának első lépései](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Egy másik eszköz, amely használható a Tártallózó. Tártallózó további információt itt található: a következő hivatkozás hello: [Tártallózó](http://storageexplorer.com/)

## <a name="next-steps"></a>Következő lépések

Ha a beállítások módosítása, hogy stop VPN-kapcsolatot, lásd: [hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md) hello hálózati biztonsági csoport és a biztonsági szabályokat, amelyek lehet, hogy a szóban forgó tootrack.
