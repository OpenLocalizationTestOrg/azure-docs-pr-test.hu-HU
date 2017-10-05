---
title: "A Service Fabric csomóponttípusok és a Virtuálisgép-méretezési készlet |} Microsoft Docs"
description: "Útmutatás a Service Fabric csomóponttípusok Virtuálisgép-méretezési készlet kapcsolódnak és hogyan távolról csatlakozni a Virtuálisgép-méretezési csoportban példányt vagy egy fürt csomópontja."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/05/2017
ms.author: chackdan
ms.openlocfilehash: 3b1a22bb3653abb68fc73645ad2cb623fabc7736
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="the-relationship-between-service-fabric-node-types-and-virtual-machine-scale-sets"></a>A Service Fabric csomóponttípusok és a virtuálisgép-méretezési csoportok közötti kapcsolat
Virtuálisgép-méretezési csoportok olyan Azure számítási erőforrás a telepíthetnek és kezelhetnek olyan virtuális gépek gyűjteménye. Minden csomópont-típus, a Service Fabric-fürt definiált egy külön Virtuálisgép-méretezési csoportban lett beállítva. Az egyes csomóponttípusok akár majd is méretezhető vagy rendelkezik egymástól függetlenül, a portok megnyitása más-más részhalmazához le, és különböző teljesítmény-mérőszámait lehet.

Az alábbi képernyőfelvételen látható egy fürt, amely két csomópont tartozik: előtér- és háttérszolgáltatások.  Minden csomópont az öt csomóponttal rendelkezik.

![Képernyőfelvétel egy fürt, amely két csomópont tartozik][NodeTypes]

## <a name="mapping-vm-scale-set-instances-to-nodes"></a>Virtuálisgép-méretezési csoportban példányok leképezése-csomópontokra
Ahogy fent látja, a Virtuálisgép-méretezési csoportban példányok indítsa el példányból 0 és be is megnőnek. A nevek számozására is megjelenik. Például a BackEnd_0 0 példánya a háttérrendszer Virtuálisgép-méretezési csoportban. Öt példánya, BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 és BackEnd_4 az adott Virtuálisgép-méretezési csoportban van.

Ha Ön növelheti a Virtuálisgép-méretezési csoportban egy új példány jön létre. Az új Virtuálisgép-méretezési csoportban példány általában lesz, a Virtuálisgép-méretezési csoportban neve + a következő példányok számát. A jelen példában BackEnd_5.

## <a name="mapping-vm-scale-set-load-balancers-to-each-node-typevm-scale-set"></a>Virtuálisgép-méretezési leképezési terheléselosztók csomópont típus/virtuális gépek méretezési beállítása
Ha telepítette a fürtöt a portálról, vagy a minta Resource Manager-sablon, amely azt megadva, majd amikor egy erőforráscsoportba tartozó összes erőforrás listáját használja majd látni fogja az egyes Virtuálisgép-méretezési csoportban vagy csomópont azokat a terheléselosztókat.

A név lesz hasonlót: **LB -&lt;NodeType neve&gt;**. Például LB-sfcluster4doc-0, a képernyőfelvételen látható módon:

![Erőforrások][Resources]

## <a name="remote-connect-to-a-vm-scale-set-instance-or-a-cluster-node"></a>Távoli kapcsolódás egy Virtuálisgép-méretezési csoportban példányt vagy egy fürt csomópontja
Minden csomópont-típus, egy fürt definiált egy külön Virtuálisgép-méretezési csoportban lett beállítva.  Ez azt jelenti, hogy azok a csomóponttípusok méretezhetők akár vagy egymástól függetlenül le, és különböző VM termékváltozatok teszik lehetővé. Egypéldányos virtuális gépeket, ellentétben a Virtuálisgép-méretezési csoport példányai nem kérdezhető le egy virtuális IP-címet saját. Ezért ez lehet egy kicsit kihívást keresett IP-címet és portot, amelyet segítségével távoli csatlakozás egy adott példányt.

Az alábbiakban a lépéseket, amelyeket követve derítheti fel azokat.

### <a name="step-1-find-out-the-virtual-ip-address-for-the-node-type-and-then-inbound-nat-rules-for-rdp"></a>1. lépés: A virtuális IP-címet a csomóponttípus és bejövő NAT-szabályokat RDP megállapítása
Ahhoz, hogy beolvasása, amely, le kell töltenie a bejövő NAT szabályok értékek az erőforrás-definíciójának részeként meghatározott **Microsoft.Network/loadBalancers**.

A portálon lépjen a Load balancer paneljét, majd **beállítások**.

![LBBlade][LBBlade]

A **beállítások**, kattintson a **bejövő forgalmat kezelő NAT-szabályok**. Ez most nyújt az IP-címet és portot, amelyet segítségével távoli csatlakozás az első Virtuálisgép-méretezési csoportban példányhoz. Az alábbi képernyőképen látható, hogy a rendszer **104.42.106.156** és **3389-es**

![NATRules][NATRules]

### <a name="step-2-find-out-the-port-that-you-can-use-to-remote-connect-to-the-specific-vm-scale-set-instancenode"></a>2. lépés: Kibővített a portot, amelynek távoli csatlakozás a megadott Virtuálisgép-méretezési csoportban példány/csomópont keresése
A jelen dokumentum korábbi I arról volt szó, hogyan a Virtuálisgép-méretezési csoportban példányok hozzárendelését a csomópontok. Használjuk, amely mérje fel, a pontos port.

A portok növekvő sorrendben a Virtuálisgép-méretezési csoportban példány foglal le. úgy, hogy a példában a FrontEnd csomóponttípus, a portok az egyes öt példánya a következő. most kell tennie, hogy a Virtuálisgép-méretezési csoportban példány azonosak maradjanak.

| **Virtuálisgép-méretezési készlet példány** | **Port** |
| --- | --- |
| FrontEnd_0 |3389 |
| FrontEnd_1 |3390 |
| FrontEnd_2 |3391 |
| FrontEnd_3 |3392 |
| FrontEnd_4 |3393 |
| FrontEnd_5 |3394 |

### <a name="step-3-remote-connect-to-the-specific-vm-scale-set-instance"></a>3. lépés: Távoli csatlakozás a megadott Virtuálisgép-méretezési csoportban példányhoz
Az alábbi képernyőfelvételen a távoli asztali kapcsolat a FrontEnd_1 való kapcsolódáshoz használni:

![RDP][RDP]

## <a name="how-to-change-the-rdp-port-range-values"></a>Az RDP-port tartományértékeknek módosítása
### <a name="before-cluster-deployment"></a>Fürttelepítés előtt
Állítja be a fürt Resource Manager-sablonnal, megadhatja a tartomány a **inboundNatPools**.

Keresse fel az erőforrás-definíció **Microsoft.Network/loadBalancers**. Amely alatt a leírása található **inboundNatPools**.  Cserélje le a *frontendportrangestart értékénél* és *frontendportrangeend értékének* értékeket.

![InboundNatPools][InboundNatPools]

### <a name="after-cluster-deployment"></a>Fürt telepítése után
Ez egy kicsit bonyolultabb, és azt eredményezheti, a virtuális gépek újbóli felhasználását. Most fog Azure PowerShell használatával új értékeinek beállításához. Győződjön meg arról, hogy az Azure PowerShell 1.0-s vagy újabb verziója telepítve van-e a számítógépen. Ha nem ezt megelőzően, I erősen javasolt lépéseket a leírt [hogyan Azure PowerShell telepítése és konfigurálása.](/powershell/azure/overview)

Jelentkezzen be az Azure-fiókjával. Ha a PowerShell-parancs valamilyen okból nem sikerül, ellenőrizze az Azure PowerShell megfelelően telepítve van-e.

```
Login-AzureRmAccount
```

Futtassa a következő kapcsolatban a terheléselosztó és a leírást a értéke látható **inboundNatPools**:

```
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name>
```

Most már készen *frontendportrangeend értékének* és *frontendportrangestart értékénél* az értékeket.

```
$PropertiesObject = @{
    #Property = value;
}
Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName <RG name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load Balancer name> -ApiVersion <use the API version that get returned> -Force
```


## <a name="next-steps"></a>Következő lépések
* [A "Üzembe helyezés bárhol" szolgáltatás és az Azure által kezelt fürtökkel összehasonlítása áttekintése](service-fabric-deploy-anywhere.md)
* [Fürtbiztonság](service-fabric-cluster-security.md)
* [Service Fabric SDK és az első lépések](service-fabric-get-started.md)

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png
