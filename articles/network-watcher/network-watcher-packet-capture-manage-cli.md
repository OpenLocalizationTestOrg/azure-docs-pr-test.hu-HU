---
title: "aaaManage csomagrögzítéseket Azure hálózati figyelőt - Azure CLI 2.0 |} Microsoft Docs"
description: "Ez a lap azt ismerteti, hogyan toomanage hello csomag rögzítése funkció az Azure CLI 2.0 használatával hálózati figyelőt"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: cb0c1d10-f7f2-4c34-b08c-f73452430be8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: d19cb7d0ca3b7a9bc0546859e07ef6d4df4f42d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-azure-cli-20"></a>Csomag rögzítésekre kezelése az Azure CLI 2.0 verziót használja Azure hálózati figyelőt

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-packet-capture-manage-portal.md)
> - [PowerShell](network-watcher-packet-capture-manage-powershell.md)
> - [CLI 1.0](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-packet-capture-manage-cli.md)
> - [Az Azure REST API-n](network-watcher-packet-capture-manage-rest.md)

Hálózati figyelő csomagrögzítéssel lehetővé teszi toocreate rögzítési munkamenetek tootrack forgalom tooand virtuális gépről. Szűrők előírt hello rögzítési munkamenet tooensure csak a kívánt hello forgalom rögzíti. Csomagrögzítéssel segít toodiagnose hálózati rendellenességek észlelését, proaktív és reaktív is. Egyéb felhasználásra tartalmazzák a hálózati statisztikákat, való információk hálózati behatolások, toodebug ügyfél-kiszolgáló közötti kommunikációt, és még sok más összegyűjtéséhez. Képes tooremotely eseményindító csomag rögzíti őket, ez a funkció megkönnyíti a hello okozta terheket egy csomagrögzítéssel fut, manuálisan, hello kívánt számítógépet, amely értékes időt takaríthat meg.

Ez a cikk használja a következő generációs CLI hello erőforrás management üzembe helyezési modelljével Azure CLI 2.0, elérhető a Windows, Mac és Linux.

tooperform hello ebben a cikkben ismertetett visszaállítási lépésekkel, túl kell[hello Azure parancssori felület Mac, Linux és Windows (Azure CLI) telepítése](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

Ez a cikk végigvezeti hello csomagrögzítéssel jelenleg elérhető különböző felügyeleti feladatok.

- [**A csomagrögzítéssel indítása**](#start-a-packet-capture)
- [**A csomagrögzítéssel leállítása**](#stop-a-packet-capture)
- [**A csomagrögzítéssel törlése**](#delete-a-packet-capture)
- [**A csomagrögzítéssel letöltése**](#download-a-packet-capture)

## <a name="before-you-begin"></a>Előkészületek

Ez a cikk feltételezi, hogy rendelkezik-e a következő erőforrások hello:

- Egy példányát a csomagrögzítéssel toocreate kívánt hálózati figyelőt hello régióban
- Hello csomagok rendelkező virtuális gép rögzítése engedélyezett bővítményekhez.

> [!IMPORTANT]
> Csomagrögzítéssel egy hello virtuális gépen futó ügynök toobe igényel. egy bővítmény hello ügynök van telepítve. Virtuálisgép-bővítmények útmutatásra van szüksége, látogasson el [virtuálisgép-bővítmények és a szolgáltatások](../virtual-machines/windows/extensions-features.md).

## <a name="install-vm-extension"></a>Virtuálisgép-bővítmény telepítése

### <a name="step-1"></a>1. lépés

Futtassa a hello `az vm extension set` parancsmag tooinstall hello csomagok rögzítési ügynök hello Vendég virtuális gépen.

A Windows virtuális gépek:

```azurecli
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentWindows --version 1.4
```

A Linux virtuális gépek:

```azurecli
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentLinux--version 1.4
````

### <a name="step-2"></a>2. lépés

tooensure, amely hello ügynök van telepítve, futtassa hello `vm extension get` parancsmagot, és adja át hello erőforráscsoport és a virtuális gép nevét. Ellenőrizze, hogy hello eredményül kapott lista tooensure hello ügynök telepítve van.

```azurecli
az vm extension show -resource-group resourceGroupName --vm-name virtualMachineName --name NetworkWatcherAgentWindows
```

a következő minta hello hello válasz futtatását példája`az vm extension show`

```json
{
  "autoUpgradeMinorVersion": true,
  "forceUpdateTag": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}/extensions/NetworkWatcherAgentWindows",
  "instanceView": null,
  "location": "westcentralus",
  "name": "NetworkWatcherAgentWindows",
  "protectedSettings": null,
  "provisioningState": "Succeeded",
  "publisher": "Microsoft.Azure.NetworkWatcher",
  "resourceGroup": "{resourceGroupName}",
  "settings": null,
  "tags": null,
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "typeHandlerVersion": "1.4",
  "virtualMachineExtensionType": "NetworkWatcherAgentWindows"
}
```

## <a name="start-a-packet-capture"></a>A csomagrögzítéssel indítása

Ha hello előző lépések befejeződött, hello csomagok rögzítési ügynök hello virtuális gépen telepítve van.

### <a name="step-1"></a>1. lépés

hello tovább tooretrieve hello hálózati figyelőt példány. Hello hálózati figyelőt Tnem neve átadása toohello `az network watcher show` parancsmag a 4. lépésben.

```azurecli
az network watcher show -resource-group resourceGroup -name networkWatcherName
```

### <a name="step-2"></a>2. lépés

A storage-fiók beolvasása. Ez a tárfiók használt toostore hello csomagok rögzítési fájl.

```azurecli
azure storage account list
```

### <a name="step-3"></a>3. lépés

Szűrők használt toolimit hello hello csomagrögzítéssel által tárolt adatokat is lehet. hello alábbi példa állít be egy csomagrögzítéssel több szűrők.  hello első három szűrők gyűjteni kimenő TCP-forgalom csak helyi IP 10.0.0.3 toodestination 20, a 80-as és a 443-as portot.  hello utolsó szűrő csak a UDP-forgalom gyűjti.

```azurecli
az network watcher packet-capture create --resource-group {resoureceurceGroupName} --vm {vmName} --name packetCaptureName --storage-account gwteststorage123abc --filters "[{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"20\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"80\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"443\"},{\"protocol\":\"UDP\"}]"
```

hello alábbi példa egy hello várható kimenet hello futtatását `az network watcher packet-capture create` parancsmag.

```json
{
  "bytesToCapturePerPacket": 0,
  "etag": "W/\"b8cf3528-2e14-45cb-a7f3-5712ffb687ac\"",
  "filters": [
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "20"
    },
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "80"
    },
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "443"
    },
    {
      "localIpAddress": "",
      "localPort": "",
      "protocol": "UDP",
      "remoteIpAddress": "",
      "remotePort": ""
    }
  ],
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/pa
cketCaptures/packetCaptureName",
  "name": "packetCaptureName",
  "provisioningState": "Succeeded",
  "resourceGroup": "NetworkWatcherRG",
  "storageLocation": {
    "filePath": null,
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/gwteststorage123abc",
    "storagePath": "https://gwteststorage123abc.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/{resourceGroupName}/p
roviders/microsoft.compute/virtualmachines/{vmName}/2017/05/25/packetcapture_16_22_34_630.cap"
  },
  "target": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}",
  "timeLimitInSeconds": 18000,
  "totalBytesPerSession": 1073741824
}
```

## <a name="get-a-packet-capture"></a>A csomagrögzítéssel beolvasása

Hello futtató `az network watcher packet-capture show` parancsmag, beolvassa a jelenleg futó vagy befejezett csomagrögzítéssel hello állapotát.

```azurecli
az network watcher packet-capture show --name packetCaptureName --location westcentralus
```

hello alábbi példa: hello hello kimenete `az network watcher packet-capture show` parancsmag. hello következő példa egy hello rögzítési befejeződése után. hello PacketCaptureStatus érték le van állítva, az egy TimeExceeded StopReason. Ezt az értéket jeleníti meg, hogy hello csomagrögzítéssel sikeres volt-e, és az idő futott.

```
{
  "bytesToCapturePerPacket": 0,
  "etag": "W/\"b8cf3528-2e14-45cb-a7f3-5712ffb687ac\"",
  "filters": [
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "20"
    },
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "80"
    },
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "443"
    },
    {
      "localIpAddress": "",
      "localPort": "",
      "protocol": "UDP",
      "remoteIpAddress": "",
      "remotePort": ""
    }
  ],
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/packetCaptures/packetCaptureName",
  "name": "packetCaptureName",
  "provisioningState": "Succeeded",
  "resourceGroup": "NetworkWatcherRG",
  "storageLocation": {
    "filePath": null,
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/gwteststorage123abc",
    "storagePath": "https://gwteststorage123abc.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/{resourceGroupName}/providers/microsoft.compute/virtualmachines/{vmName}/2017/05/25/packetcapt
ure_16_22_34_630.cap"
  },
  "target": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}",
  "timeLimitInSeconds": 18000,
  "totalBytesPerSession": 1073741824
}
```

## <a name="stop-a-packet-capture"></a>A csomagrögzítéssel leállítása

Hello futtatásával `az network watcher packet-capture stop` parancsmagot, ha a rögzítési munkamenet folyamatban az le van állítva.

```azurecli
az network watcher packet-capture stop --name packetCaptureName --location westcentralus
```

> [!NOTE]
> hello parancsmag nem választ ad vissza. Ha a jelenleg futó rögzítési munkamenet, vagy már leállt a meglévő munkamenetekben futtatott.

## <a name="delete-a-packet-capture"></a>A csomagrögzítéssel törlése

```azurecli
az network watcher packet-capture delete --name packetCaptureName --location westcentralus
```

> [!NOTE]
> A csomagrögzítéssel törlése nem érinti hello fájl hello tárfiókban.

## <a name="download-a-packet-capture"></a>A csomagrögzítéssel letöltése

A csomag rögzítési munkamenet befejezése után hello rögzítési fájl lehet feltöltött tooblob tooa vagy a helyi fájl a virtuális gép hello. hello tárolási helye hello csomagrögzítéssel definiálása hello munkamenet létrehozását. Egy eszköz tooaccess ezek fájlok rögzítését, mentett tooa tárfiók a Microsoft Azure Tártallózó, amely innen tölthető le: http://storageexplorer.com/

Ha egy tárfiókot meg van adva, csomag rögzítési fájlok mentése tooa tárfiókot, a következő helyen hello:

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a>Következő lépések

Ismerje meg, hogyan tooautomate csomagrögzítéseket virtuális gép riasztások megtekintésével [riasztási kiváltott csomagrögzítéssel létrehozása](network-watcher-alert-triggered-packet-capture.md)

Keresése, ha bizonyos adatforgalom engedélyezett a virtuális gép kívül vagy belül ellátogatva [ellenőrizze IP folyamat ellenőrzése](network-watcher-check-ip-flow-verify-portal.md)

<!-- Image references -->
