---
title: "aaaManage csomagrögzítéseket Azure hálózati figyelőt - PowerShell |} Microsoft Docs"
description: "Ezen a lapon azt ismerteti, hogyan toomanage hello csomagok rögzítése a PowerShell használatával hálózati figyelőt szolgáltatása"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 04d82085-c9ea-4ea1-b050-a3dd4960f3aa
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 77a522a1b05e020a73ba7140c1410615eb8761da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-powershell"></a>Csomag rögzítésekre kezelése a PowerShell használata Azure hálózati figyelőt

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-packet-capture-manage-portal.md)
> - [PowerShell](network-watcher-packet-capture-manage-powershell.md)
> - [CLI 1.0](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-packet-capture-manage-cli.md)
> - [Az Azure REST API-n](network-watcher-packet-capture-manage-rest.md)

Hálózati figyelő csomagrögzítéssel lehetővé teszi toocreate rögzítési munkamenetek tootrack forgalom tooand virtuális gépről. Szűrők előírt hello rögzítési munkamenet tooensure csak a kívánt hello forgalom rögzíti. Csomagrögzítéssel segít toodiagnose hálózati rendellenességek észlelését, proaktív és reaktív is. Egyéb felhasználásra tartalmazzák a hálózati statisztikákat, való információk hálózati behatolások, toodebug ügyfél-kiszolgáló közötti kommunikációt, és még sok más összegyűjtéséhez. Képes tooremotely eseményindító csomag rögzíti őket, ez a funkció megkönnyíti a hello okozta terheket egy csomagrögzítéssel fut, manuálisan, hello kívánt számítógépet, amely értékes időt takaríthat meg.

Ez a cikk végigvezeti hello csomagrögzítéssel jelenleg elérhető különböző felügyeleti feladatok.

- [**A csomagrögzítéssel indítása**](#start-a-packet-capture)
- [**A csomagrögzítéssel leállítása**](#stop-a-packet-capture)
- [**A csomagrögzítéssel törlése**](#delete-a-packet-capture)
- [**A csomagrögzítéssel letöltése**](#download-a-packet-capture)

## <a name="before-you-begin"></a>Előkészületek

Ez a cikk feltételezi, hogy rendelkezik-e a következő erőforrások hello:

* Egy példányát a csomagrögzítéssel toocreate kívánt hálózati figyelőt hello régióban

* Hello csomagok rendelkező virtuális gép rögzítése engedélyezett bővítményekhez.

> [!IMPORTANT]
> Csomagrögzítéssel van szükség a virtuálisgép-bővítmény `AzureNetworkWatcherExtension`. A virtuális gép Windows hello-bővítmény telepítése a Microsoft [a Windows Azure hálózati figyelő ügynök virtuálisgép-bővítmény](../virtual-machines/windows/extensions-nwa.md) és a Linux virtuális gép helyezést [Azure hálózati figyelő ügynök virtuálisgép-bővítmény Linux](../virtual-machines/linux/extensions-nwa.md).

## <a name="install-vm-extension"></a>Virtuálisgép-bővítmény telepítése

### <a name="step-1"></a>1. lépés

```powershell
$VM = Get-AzureRmVM -ResourceGroupName testrg -Name VM1
```

### <a name="step-2"></a>2. lépés

hello lekéri hello sémakiterjesztési adatok, a következő példa szükséges toorun hello `Set-AzureRmVMExtension` parancsmag. Ez a parancsmag hello csomagok rögzítési ügynököt telepít hello Vendég virtuális gépen.

> [!NOTE]
> Hello `Set-AzureRmVMExtension` parancsmag eltarthat néhány percig toocomplete.

A Windows virtuális gépek:

```powershell
$AzureNetworkWatcherExtension = Get-AzureRmVMExtensionImage -Location WestCentralUS -PublisherName Microsoft.Azure.NetworkWatcher -Type NetworkWatcherAgentWindows -Version 1.4.13.0
$ExtensionName = "AzureNetworkWatcherExtension"
Set-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -Location $VM.Location -VMName $VM.Name -Name $ExtensionName -Publisher $AzureNetworkWatcherExtension.PublisherName -ExtensionType $AzureNetworkWatcherExtension.Type -TypeHandlerVersion $AzureNetworkWatcherExtension.Version.Substring(0,3)
```

A Linux virtuális gépek:

```powershell
$AzureNetworkWatcherExtension = Get-AzureRmVMExtensionImage -Location WestCentralUS -PublisherName Microsoft.Azure.NetworkWatcher -Type NetworkWatcherAgentLinux -Version 1.4.13.0
$ExtensionName = "AzureNetworkWatcherExtension"
Set-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -Location $VM.Location -VMName $VM.Name -Name $ExtensionName -Publisher $AzureNetworkWatcherExtension.PublisherName -ExtensionType $AzureNetworkWatcherExtension.Type -TypeHandlerVersion $AzureNetworkWatcherExtension.Version.Substring(0,3)
````

hello következő példája a sikeres válasz hello futtatása után `Set-AzureRmVMExtension` parancsmag.

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   
```

### <a name="step-3"></a>3. lépés

tooensure, amely hello ügynök van telepítve, futtassa hello `Get-AzureRmVMExtension` parancsmagot, és adja át hello virtuális gép nevét és hello bővítmény neve.

```powershell
Get-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -VMName $VM.Name -Name $ExtensionName
```

a következő minta hello hello válasz futtatását példája`Get-AzureRmVMExtension`

```
ResourceGroupName       : testrg
VMName                  : testvm1
Name                    : AzureNetworkWatcherExtension
Location                : westcentralus
Etag                    : null
Publisher               : Microsoft.Azure.NetworkWatcher
ExtensionType           : NetworkWatcherAgentWindows
TypeHandlerVersion      : 1.4
Id                      : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testvm1/
                          extensions/AzureNetworkWatcherExtension
PublicSettings          : 
ProtectedSettings       : 
ProvisioningState       : Succeeded
Statuses                : 
SubStatuses             : 
AutoUpgradeMinorVersion : True
ForceUpdateTag          : 
```

## <a name="start-a-packet-capture"></a>A csomagrögzítéssel indítása

Ha hello előző lépések befejeződött, hello csomagok rögzítési ügynök hello virtuális gépen telepítve van.

### <a name="step-1"></a>1. lépés

hello tovább tooretrieve hello hálózati figyelőt példány. Ez a változó átadása toohello `New-AzureRmNetworkWatcherPacketCapture` parancsmag a 4. lépésben.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName  
```

### <a name="step-2"></a>2. lépés

A storage-fiók beolvasása. Ez a tárfiók használt toostore hello csomagok rögzítési fájl.

```powershell
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName testrg -Name testrgsa123
```

### <a name="step-3"></a>3. lépés

Szűrők használt toolimit hello hello csomagrögzítéssel által tárolt adatokat is lehet. hello alábbi példa hoz létre két szűrőt.  Egy szűrő gyűjti a TCP kimenő forgalom csak a helyi IP 10.0.0.3 toodestination 20, a 80-as és a 443-as portot.  hello második szűrő csak a UDP-forgalom gyűjti.

```powershell
$filter1 = New-AzureRmPacketCaptureFilterConfig -Protocol TCP -RemoteIPAddress "1.1.1.1-255.255.255" -LocalIPAddress "10.0.0.3" -LocalPort "1-65535" -RemotePort "20;80;443"
$filter2 = New-AzureRmPacketCaptureFilterConfig -Protocol UDP
```

> [!NOTE]
> A csomagrögzítéssel több szűrő adható meg.

### <a name="step-4"></a>4. lépés

Futtassa a hello `New-AzureRmNetworkWatcherPacketCapture` parancsmag toostart hello csomagok rögzítési folyamat, hello szükséges értékeket az előző lépésekben hello beolvasott átadásakor.
```powershell

New-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -TargetVirtualMachineId $vm.Id -PacketCaptureName "PacketCaptureTest" -StorageAccountId $storageAccount.id -TimeLimitInSeconds 60 -Filters $filter1, $filter2
```

hello alábbi példa egy hello várható kimenet hello futtatását `New-AzureRmNetworkWatcherPacketCapture` parancsmag.

```
Name                    : PacketCaptureTest
Id                      : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatcher
                          s/NetworkWatcher_westcentralus/packetCaptures/PacketCaptureTest
Etag                    : W/"3bf27278-8251-4651-9546-c7f369855e4e"
ProvisioningState       : Succeeded
Target                  : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testvm1
BytesToCapturePerPacket : 0
TotalBytesPerSession    : 1073741824
TimeLimitInSeconds      : 60
StorageLocation         : {
                            "StorageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Storage/storageA
                          ccounts/examplestorage",
                            "StoragePath": "https://examplestorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-00000
                          0000000/resourcegroups/testrg/providers/microsoft.compute/virtualmachines/testvm1/2017/02/01/packetcapture_22_42_48_238.cap"
                          }
Filters                 : [
                            {
                              "Protocol": "TCP",
                              "RemoteIPAddress": "1.1.1.1-255.255.255",
                              "LocalIPAddress": "10.0.0.3",
                              "LocalPort": "1-65535",
                              "RemotePort": "20;80;443"
                            },
                            {
                              "Protocol": "UDP",
                              "RemoteIPAddress": "",
                              "LocalIPAddress": "",
                              "LocalPort": "",
                              "RemotePort": ""
                            }
                          ]


```

## <a name="get-a-packet-capture"></a>A csomagrögzítéssel beolvasása

Hello futtató `Get-AzureRmNetworkWatcherPacketCapture` parancsmag, beolvassa a jelenleg futó vagy befejezett csomagrögzítéssel hello állapotát.

```powershell
Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

hello alábbi példa: hello hello kimenete `Get-AzureRmNetworkWatcherPacketCapture` parancsmag. hello következő példa egy hello rögzítési befejeződése után. hello PacketCaptureStatus érték le van állítva, az egy TimeExceeded StopReason. Ezt az értéket jeleníti meg, hogy hello csomagrögzítéssel sikeres volt-e, és az idő futott.
```
Name                    : PacketCaptureTest
Id                      : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatcher
                          s/NetworkWatcher_westcentralus/packetCaptures/PacketCaptureTest
Etag                    : W/"4b9a81ed-dc63-472e-869e-96d7166ccb9b"
ProvisioningState       : Succeeded
Target                  : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testvm1
BytesToCapturePerPacket : 0
TotalBytesPerSession    : 1073741824
TimeLimitInSeconds      : 60
StorageLocation         : {
                            "StorageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Storage/storageA
                          ccounts/examplestorage",
                            "StoragePath": "https://examplestorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-00000
                          0000000/resourcegroups/testrg/providers/microsoft.compute/virtualmachines/testvm1/2017/02/01/packetcapture_22_42_48_238.cap"
                          }
Filters                 : [
                            {
                              "Protocol": "TCP",
                              "RemoteIPAddress": "1.1.1.1-255.255.255",
                              "LocalIPAddress": "10.0.0.3",
                              "LocalPort": "1-65535",
                              "RemotePort": "20;80;443"
                            },
                            {
                              "Protocol": "UDP",
                              "RemoteIPAddress": "",
                              "LocalIPAddress": "",
                              "LocalPort": "",
                              "RemotePort": ""
                            }
                          ]
CaptureStartTime        : 2/1/2017 10:43:01 PM
PacketCaptureStatus     : Stopped
StopReason              : TimeExceeded
PacketCaptureError      : []
```

## <a name="stop-a-packet-capture"></a>A csomagrögzítéssel leállítása

Hello futtatásával `Stop-AzureRmNetworkWatcherPacketCapture` parancsmagot, ha a rögzítési munkamenet folyamatban az le van állítva.

```powershell
Stop-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

> [!NOTE]
> hello parancsmag nem választ ad vissza. Ha a jelenleg futó rögzítési munkamenet, vagy már leállt a meglévő munkamenetekben futtatott.

## <a name="delete-a-packet-capture"></a>A csomagrögzítéssel törlése

```powershell
Remove-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
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

Keresése, ha bizonyos forgalom engedélyezett kívül a virtuális gép orr ellátogatva [ellenőrizze IP folyamat ellenőrzése](network-watcher-check-ip-flow-verify-portal.md)

<!-- Image references -->














