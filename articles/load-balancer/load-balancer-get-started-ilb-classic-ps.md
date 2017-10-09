---
title: "egy Azure belső aaaCreate terheléselosztó - klasszikus PowerShell |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy belső terheléselosztó hello klasszikus üzembe helyezési modellben a PowerShell használatával"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 3be93168-3787-45a5-a194-9124fe386493
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 382db80c42ffab09905513019b72e85a4f9dfeff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internal-load-balancer-classic-using-powershell"></a>Bevezetés a belső terheléselosztó (klasszikus) PowerShell használatával történő létrehozásába

> [!div class="op_single_selector"]
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [Felhőszolgáltatások](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!IMPORTANT]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).  Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Ismerje meg, hogyan túl[hello Resource Manager modellt használja a következő lépésekkel](load-balancer-get-started-ilb-arm-ps.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-internal-load-balancer-set-for-virtual-machines"></a>Belső terheléselosztó készlet létrehozása a virtuális gépekhez

belső terheléselosztót beállítani, és szeretne küldeni a forgalom tooit kiszolgálók hello toocreate, toodo hello következő rendelkezik:

1. Belső terheléselosztás hello végpont egy elosztott terhelésű készlet hello kiszolgálóin bejövő forgalom toobe terhelés leendő példányt létrehozni.
2. Hello bejövő forgalmat fog kapni toohello virtuális gépek megfelelő végpont-hozzáadáshoz.
3. Hello forgalom toobe az elosztott terhelésű toosend a forgalom toohello virtuális IP-cím (VIP) cím hello belső terheléselosztás példány küldi hello kiszolgálók konfigurálása.

### <a name="step-1-create-an-internal-load-balancing-instance"></a>1. lépés: Belső terheléselosztási példány létrehozása

Létező felhőalapú szolgáltatást, vagy egy felhőalapú szolgáltatás, a regionális virtuális hálózatot telepített a következő Windows PowerShell-parancsok hello belső terheléselosztás példány hozhatja létre:

```powershell
$svc="<Cloud Service Name>"
$ilb="<Name of your ILB instance>"
$subnet="<Name of hello subnet within your virtual network>"
$IP="<hello IPv4 address toouse on hello subnet-optional>"

Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP
```

Vegye figyelembe, hogy a hello használata [Add-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx) Windows PowerShell-parancsmag hello DefaultProbe paraméterhalmaz használja. A további paraméterkészletekkel kapcsolatos további információkért lásd: [Add-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx).

### <a name="step-2-add-endpoints-toohello-internal-load-balancing-instance"></a>2. lépés: Végpontok toohello belső terheléselosztás példány hozzáadása

Például:

```powershell
$svc="mytestcloud"
$vmname="DB1"
$epname="TCP-1433-1433"
$lbsetname="lbset"
$prot="tcp"
$locport=1433
$pubport=1433
$ilb="ilbset"
Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -Lbset $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM
```

### <a name="step-3-configure-your-servers-toosend-their-traffic-toohello-new-internal-load-balancing-endpoint"></a>3. lépés: Konfigurálja a kiszolgálók toosend a forgalom toohello új belső terheléselosztás végpontot

Konfigurálnia kell túl hello kiszolgálók érkező forgalmat folyamatos toobe elosztott terhelésű toouse hello új IP-címét (hello VIP) hello belső terheléselosztás példány. Ez az hello cím melyik belső terheléselosztás hello példány figyel. A legtöbb esetben szüksége toojust hozzáadása vagy módosítása egy DNS-rekordot hello VIP hello belső terheléselosztás példány.

Hello belső terheléselosztás példány hello létrehozásakor megadott hello IP-címet, ha már rendelkezik hello VIP. Ellenkező esetben a következő parancsok hello hello VIP látható:

```powershell
$svc="<Cloud Service Name>"
Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer
```

toouse ezek a parancsok, töltse ki a hello értékek és eltávolítás hello < és >. Például:

```powershell
$svc="mytestcloud"
Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer
```

A Get-AzureInternalLoadBalancer parancs hello hello megjelenítését jegyezze fel a hello IP-címet, és hello a szükséges módosításokat tooyour kiszolgálók vagy DNS-rekordok tooensure, amely a forgalom toohello VIP küldi el.

> [!NOTE]
> hello Microsoft Azure platform különféle felügyeleti forgatókönyvekhez, amik egy nyilvánosan elérhető, statikus IPv4-címet használ. hello IP-cím 168.63.129.16. Ezt az IP-címet nem blokkolhatja egy tűzfal sem, mert ez nem várt viselkedést okozhat.
> A tekintetben tooAzure belső terheléselosztás az IP-címet használják mintavételt hello load balancer toodetermine hello állapotát a virtuális gépek elosztott terhelésű készlet figyelése. Ha egy hálózati biztonsági csoportot használt toorestrict forgalom tooAzure virtuális gépek egy belső elosztott terhelésű készlet vagy alkalmazott tooa virtuális hálózati alhálózat, győződjön meg arról, hogy a hálózati biztonsági szabály legyen-e adva a 168.63.129.16 tooallow forgalom.

## <a name="example-of-internal-load-balancing"></a>Példa a belső terheléselosztásra

toostep le a hello end-tooend létrehozásának folyamatán egy elosztott terhelésű készlet két példa konfigurációk esetén lásd a következő szakaszok hello.

### <a name="an-internet-facing-multi-tier-application"></a>Az internetről elérhető, több rétegből álló alkalmazás

Azt szeretné, hogy tooprovide Internet felé néző webes kiszolgálók egy csoportja egy elosztott terhelésű dokumentumadatbázis-szolgáltatás. Mindkét kiszolgálókészletet ugyanaz az Azure felhőszolgáltatás üzemelteti. Web server forgalom tooTCP 1433-as port hello adatbázis szinten lévő két virtuális gép között elérhetőnek kell lennie. 1. ábra hello konfiguráció látható.

![Adatbázis-rétegből hello belső elosztott terhelésű készlet](./media/load-balancer-internal-getstarted/IC736321.png)

hello konfigurációs hello következő tevődik össze:

* hello meglévő felhőszolgáltatás hello virtuális gépeket mytestcloud neve.
* hello két meglévő adatbázis-kiszolgáló megnevezett D1, DB2.
* Hello webes réteg webkiszolgálók toohello Helyadatbázis-kiszolgálói adatbázis-rétegből hello hello magánhálózati IP-cím használatával kapcsolódnak. Egy másik lehetőség toouse a saját DNS hello virtuális hálózat, és manuálisan regisztrálni egy A rekordot hello belső terheléselosztó-készlet.

hello következő parancsok konfigurálása nevű új belső terheléselosztás példány **ILBset** , és adja hozzá a végpontok toohello virtuális gépek megfelelő toohello a két adatbázis-kiszolgálóin:

```powershell
$svc="mytestcloud"
$ilb="ilbset"
Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb
$prot="tcp"
$locport=1433
$pubport=1433
$epname="TCP-1433-1433"
$lbsetname="lbset"
$vmname="DB1"
Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM

$epname="TCP-1433-1433-2"
$vmname="DB2"
Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM
```

## <a name="remove-an-internal-load-balancing-configuration"></a>Belső terheléselosztási konfiguráció eltávolítása

a virtuális gép egy belső terheléselosztó példányból, a következő parancsok használata hello végpontjaként tooremove:

```powershell
$svc="<Cloud service name>"
$vmname="<Name of hello VM>"
$epname="<Name of hello endpoint>"
Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM
```

toouse ezek a parancsok, töltse ki hello értékek eltávolításával hello < és >.

Például:

```powershell
$svc="mytestcloud"
$vmname="DB1"
$epname="TCP-1433-1433"
Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM
```

egy belső terheléselosztó példány egy felhőalapú szolgáltatás, a következő parancsok használata hello tooremove:

```powershell
$svc="<Cloud service name>"
Remove-AzureInternalLoadBalancer -ServiceName $svc
```

Ezek a parancsok toouse hello érték töltse ki, és távolítsa el a hello < és >.

Például:

```powershell
$svc="mytestcloud"
Remove-AzureInternalLoadBalancer -ServiceName $svc
```

## <a name="additional-information-about-internal-load-balancer-cmdlets"></a>További információ a belső terheléselosztási parancsmagokról

Belső terheléselosztás parancsmagokról, futtassa a következő parancsokat egy Windows PowerShell-parancssorba hello tooobtain további információ:

```powershell
Get-Help New-AzureInternalLoadBalancerConfig -full
Get-Help Add-AzureInternalLoadBalancer -full
Get-Help Get-AzureInternalLoadbalancer -full
Get-Help Remove-AzureInternalLoadBalancer -full
```

## <a name="next-steps"></a>Következő lépések

[A terheléselosztó elosztási módjának konfigurálása forrás IP-affinitás használatával](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)

