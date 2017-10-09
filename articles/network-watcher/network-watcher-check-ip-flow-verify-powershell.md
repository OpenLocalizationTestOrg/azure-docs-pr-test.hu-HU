---
title: "Ellenőrizze a aaaverify forgalom Azure hálózati figyelő IP-adatfolyam - PowerShell |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toocheck, ha a virtuális gép forgalom tooor engedélyezett vagy megtagadott a PowerShell használatával"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: e1dad757-8c5d-467f-812e-7cc751143207
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 924da1de1bd554e15816886f8e51d7f170f0e7ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-tooor-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a>Ellenőrizze, hogy a forgalom engedélyezett vagy tiltott tooor a virtuális gép IP-adatfolyam és Azure hálózati figyelőt összetevője ellenőrzése

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-ip-flow-verify-portal.md)
> - [PowerShell](network-watcher-check-ip-flow-verify-powershell.md)
> - [CLI 1.0](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-ip-flow-verify-cli.md)
> - [Az Azure REST API-n](network-watcher-check-ip-flow-verify-rest.md)


IP-adatfolyam ellenőrizze, hogy egy funkciója, amely lehetővé teszi tooverify forgalom engedélyezve van a virtuális gép tooor hálózati figyelőt. Ebben a forgatókönyvben hasznos tooget e virtuális gép működik tooan külső erőforrás- vagy háttéradatbázis aktuális állapotának. IP-adatfolyam ellenőrzésére használt tooverify, ha a hálózati biztonsági csoport (NSG) szabályok konfigurációja megfelelő-e, és az NSG-szabályok blokkolt adatfolyamok hibaelhárítása. Egy másik oka IP folyamata győződjön meg arról, amelyet a letiltott tooensure forgalmat blokkol megfelelően hello NSG.

## <a name="before-you-begin"></a>Előkészületek

Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt, vagy hálózati figyelőt meglévő példányát. hello is feltételezzük, hogy létezik-e egy érvényes virtuális géppel erőforrás csoport toobe használt.

## <a name="scenario"></a>Forgatókönyv

Ezen forgatókönyv által használt IP-adatfolyam tooverify ellenőrizze-e a virtuális gép működik tooa ismert Bing IP-címet. Ha megtagadja a hello forgalom, hello biztonsági szabály, amely megtagadja a forgalom adja vissza. toolearn kapcsolatos IP folyamata győződjön meg arról, látogasson el [IP folyamat ellenőrzése – áttekintés](network-watcher-ip-flow-verify-overview.md)

## <a name="retrieve-network-watcher"></a>Hálózati figyelőt beolvasása

hello első lépéseként tooretrieve hello hálózati figyelőt példány. Hello `$networkWatcher` változó átadása toohello IP folyamata parancsmag ellenőrzése.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-vm"></a>A virtuális gép beolvasása

IP-adatfolyam tesztek forgalom tooor tartozó IP-címet a virtuális gép tooor a távoli célhelyről a ellenőrzése. A virtuális gép azonosítóját hello parancsmag szükség. Ha már ismeri hello virtuális gép toouse hello azonosítója, kihagyhatja ezt a lépést.

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

## <a name="get-hello-nics"></a>Hálózati adapter hello beolvasása

hello hello virtuális gépen egy hálózati adapter IP-címe szükséges, ebben a példában beolvassuk hello hálózati adaptert egy virtuális gépen. Ha már ismeri a hello IP-címet, amelyet szeretne tootest hello virtuális gépen ezt a lépést kihagyhatja.

```powershell
$Nics = Get-AzureRmNetworkInterface | Where {$_.Id -eq $vm.NetworkProfile.NetworkInterfaces.Id.ForEach({$_})}
```

## <a name="run-ip-flow-verify"></a>Futtatási IP-adatfolyam ellenőrzése

Most, hogy hello információ szükséges toorun hello parancsmag, hello Futtatás `Test-AzureRmNetworkWatcherIPFlow` parancsmag tootest hello forgalmat. A jelen példában használjuk hello első IP-cím hello első adapteren zajlik.

```powershell
Test-AzureRmNetworkWatcherIPFlow -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id `
-Direction Outbound -Protocol TCP `
-LocalIPAddress $nics[0].IpConfigurations[0].PrivateIpAddress -LocalPort 6895 -RemoteIPAddress 204.79.197.200 -RemotePort 80
```

> [!NOTE]
> IP-adatfolyam ellenőrizze, hogy megköveteli, hogy hello VM erőforrás toorun van lefoglalva.

## <a name="review-results"></a>Tekintse át az eredményeket

Futtatása után `Test-AzureRmNetworkWatcherIPFlow` hello eredményeinek, hello alábbi példa az előző lépésben hello hello eredményének.

```
Access RuleName                                  
------ --------                                  
Allow  defaultSecurityRules/AllowInternetOutBound
```

## <a name="next-steps"></a>Következő lépések

Ha a forgalmat blokkol, és nem kell, lásd: [hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack hello hálózati biztonsági csoport és a biztonsági szabályok definiált.

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png













