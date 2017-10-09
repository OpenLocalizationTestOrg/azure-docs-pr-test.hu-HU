---
title: "Windows virtuális gép aaaSelect lemezképek az Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure PowerSHell toodetermine hello közzétevő, az ajánlat, SKU és verziója a piactér Virtuálisgép-lemezképek."
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 188b8974-fabd-4cd3-b7dc-559cbb86b98a
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/12/2017
ms.author: danlep
ms.openlocfilehash: 752edcd0935f5141832e49503ae800ea0145e219
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toofind-windows-vm-images-in-hello-azure-marketplace-with-azure-powershell"></a>Hogyan toofind Windows virtuális gép az Azure piactérről az Azure PowerShell hello lemezképek

Ez a témakör ismerteti, hogyan toouse Azure PowerShell toofind VM hello Azure piactér a lemezképeket. Ezen információk toospecify Piactéri rendszerkép használja, a Windows virtuális gépek létrehozásakor.

Győződjön meg arról, hogy telepítve, és hello legújabb konfigurált [Azure PowerShell modul](/powershell/azure/install-azurerm-ps).



## <a name="table-of-commonly-used-windows-images"></a>A gyakran használt Windows-lemezképek tábla
| Közzétevő neve | Ajánlat | SKU |
|:--- |:--- |:--- |:--- |
| MicrosoftWindowsServer |WindowsServer |2016-Datacenter |
| MicrosoftWindowsServer |WindowsServer |2016-adatközpont-kiszolgálómag |
| MicrosoftWindowsServer |WindowsServer |2016-adatközpont-az-tárolók |
| MicrosoftWindowsServer |WindowsServer |2016-Nano-kiszolgáló |
| MicrosoftWindowsServer |WindowsServer |2012-R2-Datacenter |
| MicrosoftWindowsServer |WindowsServer |2008 R2 SP1 CSOMAG |
| MicrosoftDynamicsNAV |DynamicsNAV |2017 |
| MicrosoftSharePoint |MicrosoftSharePointServer |2016 |
| MicrosoftSQLServer |SQL2016-WS2016 |Enterprise |
| MicrosoftSQLServer |SQL2014SP2-WS2012R2 |Enterprise |
| MicrosoftWindowsServerHPCPack |WindowsServerHPCPack |2012R2 |
| MicrosoftWindowsServerEssentials |WindowsServerEssentials |WindowsServerEssentials |

## <a name="find-specific-images"></a>Adott rendszerképek keresése


Amikor egy új virtuális gép létrehozása az Azure Resource Manager eszközzel, bizonyos esetekben van szüksége toospecify lemezkép hello kombinációja hello következő lemezkép tulajdonságai:

* Közzétevő
* Ajánlat
* SKU

Például használni ezeket az értékeket hello [Set-AzureRMVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) PowerShell-parancsmagot, vagy egy erőforrás-sablon, amelyben meg kell adnia a létrehozott virtuális gép toobe hello típusát.

Ha toodetermine kell ezeket az értékeket, futtathatja hello [Get-AzureRMVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher), [Get-AzureRMVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer), és [Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) parancsmagok toonavigate hello képek. Meghatározhatja, hogy ezeket az értékeket:

1. Lista hello kép közzétevők.
2. Listázza egy adott közzétevő ajánlatait.
3. Listázza egy adott ajánlathoz tartozó termékváltozatokat.

Először listában hello közzétevők a hello a következő parancsokat:

```powershell
$locName="<Azure location, such as West US>"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

Adja meg a kiválasztott közzétevő neve, és futtassa a következő parancsok hello:

```powershell
$pubName="<publisher>"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Adja meg a kiválasztott ajánlat nevét, és futtassa a következő parancsok hello:

```powershell
$offerName="<offer>"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Hello hello kimenetét a `Get-AzureRMVMImageSku` parancsot egy új virtuális gép toospecify hello lemezkép szükséges összes hello információt rendelkezik.

hello következő teljes példa látható:

```powershell
$locName="West US"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName

```

Kimenet:

```
PublisherName
-------------
a10networks
aiscaler-cache-control-ddos-and-url-rewriting-
alertlogic
AlertLogic.Extension
Barracuda.Azure.ConnectivityAgent
barracudanetworks
basho
boxless
bssw
Canonical
...
```

Hello "MicrosoftWindowsServer" közzétevő:

```powershell
$pubName="MicrosoftWindowsServer"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Kimenet:

```
Offer
-----
Windows-HUB
WindowsServer
WindowsServer-HUB
```

Hello "WindowsServer" szolgáltatásokat:

```powershell
$offerName="WindowsServer"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Kimenet:

```
Skus
----
2008-R2-SP1
2008-R2-SP1-smalldisk
2012-Datacenter
2012-Datacenter-smalldisk
2012-R2-Datacenter
2012-R2-Datacenter-smalldisk
2016-Datacenter
2016-Datacenter-Server-Core
2016-Datacenter-Server-Core-smalldisk
2016-Datacenter-smalldisk
2016-Datacenter-with-Containers
2016-Nano-Server
```

A listáról, másolja a kiválasztott termékváltozat hello, és rendelkezik az összes hello adatot hello `Set-AzureRMVMSourceImage` PowerShell-parancsmagot, vagy egy erőforrás-sablon.

## <a name="next-steps"></a>Következő lépések
Most kiválaszthatja, hogy pontosan hello kép toouse szeretné. toocreate egy virtuális gép hello kép információk, csak talált, amely segítségével gyorsan lásd [Windows virtuális gép létrehozása a PowerShell-lel](quick-create-powershell.md).
