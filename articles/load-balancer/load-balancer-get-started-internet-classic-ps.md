---
title: "az internetre irányuló aaaCreate terheléselosztó - klasszikus Azure PowerShell |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy internetre terheléselosztó klasszikus módban PowerShell használatával"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-service-management
ms.assetid: 73e8bfa4-8086-4ef0-9e35-9e00b24be319
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 76d9a712a0acda223fc86b80be9c35c0ed9f3a50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-powershell"></a>Bevezetés az internetkapcsolattal rendelkező terheléselosztó (klasszikus) létrehozásába a PowerShellben

> [!div class="op_single_selector"]
> * [klasszikus Azure portál](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [Azure Cloud Services](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> Azure-erőforrások használata előtt-e, hogy Azure jelenleg két üzembe helyezési modellel rendelkezik fontos toounderstand: Azure Resource Manager és klasszikus. Bizonyosodjon meg arról, hogy megfelelő ismeretekkel rendelkezik az [üzembe helyezési modellekről és eszközökről](../azure-classic-rm.md), mielőtt elkezdene dolgozni az Azure-erőforrásokkal. Ez a cikk hello tetején hello fülekre kattintva megtekintheti a különféle eszközök dokumentációit hello. Ez a cikk ismerteti a hello klasszikus üzembe helyezési modellben. Emellett [megtudhatja, hogyan toocreate egy internetre terheléselosztó Azure Resource Manager használatával](load-balancer-get-started-internet-arm-ps.md).

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="set-up-load-balancer-using-powershell"></a>A terheléselosztó beállítása a PowerShell használatával

tooset fel egy terhelés-kiegyenlítő powershellel, kövesse a hello alábbi lépéseket:

1. Ha még sosem használta az Azure PowerShell, lásd: [hogyan tooInstall és konfigurálása az Azure PowerShell](/powershell/azure/overview) és az útmutatás hello összes hello módon toohello toosign befejezése az Azure, és jelölje ki az előfizetését.
2. A virtuális gép létrehozása utáni használhatja a PowerShell-parancsmagok tooadd load balancer tooa virtuális gép hello belül ugyanaz a felhőalapú szolgáltatás.

Hello adhat egy terheléselosztási terheléselosztó-készlet neve "webfarm" toocloud szolgáltatás "mytestcloud" (vagy myctestcloud.cloudapp.net), hello hello végpontjainak betöltése terheléselosztó toovirtual hozzáadása gépek alábbi példa neve "web1" és a "weben 2". hello terheléselosztó fogad hálózati forgalmat a 80-as porton és terhelés kiegyensúlyozza a helyi végpont hello (az az eset 80-as port) által meghatározott hello virtuális gépek közötti TCP használatával.

### <a name="step-1"></a>1. lépés

A web1"hello első virtuális gép" elosztott terhelésű végpont létrehozása

```powershell
Get-AzureVM -ServiceName "mytestcloud" -Name "web1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM
```

### <a name="step-2"></a>2. lépés

Hozzon létre egy másik végpont a hello második virtuális gép "web2" hello azonos betölteni a terheléselosztó-készlet neve

```powershell
Get-AzureVM -ServiceName "mytestcloud" -Name "web2" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM
```

## <a name="remove-a-virtual-machine-from-a-load-balancer"></a>Virtuális gép eltávolítása a terheléselosztóból

Használhatja a Remove-AzureEndpoint tooremove egy virtuális gép végpontjának hello terheléselosztó

```powershell
Get-azureVM -ServiceName mytestcloud  -Name web1 |Remove-AzureEndpoint -Name httpin | Update-AzureVM
```

## <a name="next-steps"></a>Következő lépések

Emellett [megkezdheti egy belső terheléselosztó létrehozását](load-balancer-get-started-ilb-classic-ps.md), és beállíthatja az [elosztási mód](load-balancer-distribution-mode.md) típusát egy adott terheléselosztó hálózati forgalmára vonatkozóan.

Ha az alkalmazásnak tookeep kapcsolatok életben kiszolgálók egy terheléselosztó mögött, akkor is jobban átláthatja kapcsolatos [üresjárati TCP időtúllépési beállítások terheléselosztó](load-balancer-tcp-idle-timeout.md). Az Azure Load Balancer használatakor segítségével toolearn kapcsolat üresjárati működése.
