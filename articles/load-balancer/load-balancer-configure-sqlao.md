---
title: "az always on SQL aaaConfigure terheléselosztó |} Microsoft Docs"
description: "SQL mindig a, és hogyan tooleverage powershell toocreate terheléselosztóját hello SQL végrehajtása Load balancer toowork konfigurálása"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: d7bc3790-47d3-4e95-887c-c533011e4afd
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: ac6200b18f725dadee2555b80055327d379417d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-load-balancer-for-sql-always-on"></a>Konfigurálása terheléselosztó SQL mindig

SQL Server AlwaysOn rendelkezésre állási csoportok futtatható a Példánynak. Rendelkezésre állási csoport egy SQL Server flagship megoldás magas rendelkezésre állási és vészhelyreállítási helyreállításhoz. rendelkezésre állási csoport figyelőjének hello lehetővé teszi, hogy ügyfél alkalmazások tooseamlessly csatlakozás toohello elsődleges másodpéldány, függetlenül hello replikák hello konfigurációban hello száma.

hello figyelőjének (DNS) nevével csatlakoztatott tooa elosztott terhelésű IP-cím, Azure terheléselosztója hello replikakészlethez hello bejövő forgalom tooonly hello elsődleges kiszolgáló gazdagépére továbbítja.

Az SQL Server AlwaysOn (figyelő) végpontokhoz ILB támogatási is használhatja. Most hello kisegítő hello figyelő szabályozhatják, és választhat hello elosztott terhelésű IP-cím a megadott alhálózat a virtuális hálózat (VNet).

Hello figyelő ILB használatával hello az SQL server endpoint (pl. Server = tcp:ListenerName, 1433; adatbázis = DatabaseName) csak úgy érhető el:

* Szolgáltatások és a virtuális gépek hello azonos virtuális hálózaton
* Szolgáltatások és virtuális gépek csatlakoztatott helyszíni hálózatról
* Szolgáltatások és az összekapcsolt Vnetek virtuális gépek

![ILB_SQLAO_NewPic](./media/load-balancer-configure-sqlao/sqlao1.png)

1. ábra – az SQL AlwaysOn konfigurálva az Internet felé néző terheléselosztó

## <a name="add-internal-load-balancer-toohello-service"></a>Belső terheléselosztó toohello szolgáltatás hozzáadása

1. A következő példa hello hogy egy virtuális hálózatot, amely tartalmazza a "Alhálózat-1" nevű alhálózat konfigurálhatja:

    ```powershell
    Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc
    ```
2. Elosztott terhelésű végpont-hozzáadáshoz a Példánynak az egyes virtuális gépek

    ```powershell
    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -
    DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM
    ```

    Hello a fenti példában fel kell 2 VM hívott "sqlsvc1" és "sqlsvc2" fut a felhőben hello szolgáltatást a "Sqlsvc". Létrehozása után a ILB hello `DirectServerReturn` váltani, hozzáadhat betölteni az elosztott terhelésű végpont toohello ILB tooallow SQL tooconfigure hello figyelői hello rendelkezésre állási csoportok számára.

Az SQL AlwaysOn szolgáltatással kapcsolatos további információkért lásd: [belső terheléselosztót egy AlwaysOn rendelkezésre állási csoport konfigurálása az Azure-](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

## <a name="see-also"></a>Lásd még:
[Ismerkedés az Internet felé néző terheléselosztó konfigurálása](load-balancer-get-started-internet-arm-ps.md)

[Ismerkedés a belső terheléselosztók konfigurálása](load-balancer-get-started-ilb-arm-ps.md)

[A terheléselosztó elosztási módjának konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)
