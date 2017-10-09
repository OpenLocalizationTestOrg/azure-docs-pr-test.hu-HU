---
title: "Háló aaaService csomóponttípusok és a Virtuálisgép-méretezési készlet |} Microsoft Docs"
description: "Ismerteti, milyen kapcsolatban áll a Service Fabric csomóponttípusok tooVM méretezési csoportok és hogyan kapcsolódnak az tooremote a tooa Virtuálisgép-méretezési csoportban példányt vagy egy fürt csomópontja."
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
ms.openlocfilehash: 830ea2816f5864de146a77483c85de26f91c2425
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="hello-relationship-between-service-fabric-node-types-and-virtual-machine-scale-sets"></a>a Service Fabric csomóponttípusok és a virtuálisgép-méretezési csoportok hello kapcsolata
Virtuálisgép-méretezési csoportok olyan Azure számítási erőforrás toodeploy használja, és a virtuális gépek készletként gyűjteményeinek kezelését. Minden csomópont-típus, a Service Fabric-fürt definiált egy külön Virtuálisgép-méretezési csoportban lett beállítva. Az egyes csomóponttípusok akár majd is méretezhető vagy rendelkezik egymástól függetlenül, a portok megnyitása más-más részhalmazához le, és különböző teljesítmény-mérőszámait lehet.

képernyőfelvétel a következő hello mutatja egy fürt, amely két csomópont tartozik: előtér- és háttérszolgáltatások.  Minden csomópont az öt csomóponttal rendelkezik.

![Képernyőfelvétel egy fürt, amely két csomópont tartozik][NodeTypes]

## <a name="mapping-vm-scale-set-instances-toonodes"></a>Virtuálisgép-méretezési csoportban példányok toonodes leképezése
Fent látható, hello Virtuálisgép-méretezési csoportban példányok példányt 0-tól kezdődnek, majd következik be. hello számozására hello neveket is megjelenik. Például BackEnd_0 háttér Virtuálisgép-méretezési csoportban hello 0-példány. Öt példánya, BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 és BackEnd_4 az adott Virtuálisgép-méretezési csoportban van.

Ha Ön növelheti a Virtuálisgép-méretezési csoportban egy új példány jön létre. hello új Virtuálisgép-méretezési csoportban példánynév hello Virtuálisgép-méretezési csoportban neve + hello következő példányszámának általában legyen. A jelen példában BackEnd_5.

## <a name="mapping-vm-scale-set-load-balancers-tooeach-node-typevm-scale-set"></a>Méretezési betöltése hozzárendelése a virtuális gép egy terheléselosztó tooeach csomópont típus/VM méretezési
Ha telepítette a fürt hello portálról, vagy hello minta Resource Manager-sablon, amely azt megadva, majd amikor egy erőforráscsoportba tartozó összes erőforrás listáját használja majd látni fogja az egyes Virtuálisgép-méretezési csoportban vagy csomópont hello terheléselosztók.

hello neve volna hasonlót: **LB -&lt;NodeType neve&gt;**. Például LB-sfcluster4doc-0, a képernyőfelvételen látható módon:

![Erőforrások][Resources]

## <a name="remote-connect-tooa-vm-scale-set-instance-or-a-cluster-node"></a>Távoli kapcsolódás tooa Virtuálisgép-méretezési csoportban példányt vagy egy fürt csomópontja
Minden csomópont-típus, egy fürt definiált egy külön Virtuálisgép-méretezési csoportban lett beállítva.  Megadhat az, hogy azt jelenti, hogy hello csomóponttípusok akár vagy egymástól függetlenül le, és különböző VM termékváltozatok teszik lehetővé. Egypéldányos virtuális gépeket, eltérően hello Virtuálisgép-méretezési csoport példányai nem kérdezhető le egy virtuális IP-címet saját. Így jelző bit lehet kihívást, ha a keresett IP cím és port használható tooremote csatlakozás tooa példány.

Az alábbiakban hello lépések követésével toodiscover őket.

### <a name="step-1-find-out-hello-virtual-ip-address-for-hello-node-type-and-then-inbound-nat-rules-for-rdp"></a>1. lépés: Hello csomóponttípus hello virtuális IP-címét, majd a bejövő forgalmat kezelő NAT-szabályokat RDP megállapítása
A sorrend tooget, telepíteni kell, amely tooget hello bejövő forgalmat kezelő NAT szabályok hello erőforrás-definíció részeként meghatározott értékek **Microsoft.Network/loadBalancers**.

Hello portálon lépjen toohello Load balancer panelen, majd **beállítások**.

![LBBlade][LBBlade]

A **beállítások**, kattintson a **bejövő forgalmat kezelő NAT-szabályok**. Ez most által biztosított IP-cím és port használható tooremote hello toohello Virtuálisgép-méretezési csoportban elsőként csatlakoztassa. Hello alábbi képernyőképen látható, hogy a rendszer **104.42.106.156** és **3389-es**

![NATRules][NATRules]

### <a name="step-2-find-out-hello-port-that-you-can-use-tooremote-connect-toohello-specific-vm-scale-set-instancenode"></a>2. lépés: Hello port megtudhatja, hogy használhatja-e tooremote csatlakozás toohello adott Virtuálisgép-méretezési csoportban példány/csomópont
A jelen dokumentum korábbi I arról volt szó, hogyan hello Virtuálisgép-méretezési csoportban példányok leképezése toohello csomópontok. Adott toofigure hello pontos port kimenő használjuk.

hello portok hello Virtuálisgép-méretezési csoportban példány növekvő sorrendben foglal le. ezért a hello előtér csomóponttípus példában hello hello öt példányok mindegyikének portjait hello következő. Ön most kell toodo hello a Virtuálisgép-méretezési csoportban példány azonosak maradjanak.

| **Virtuálisgép-méretezési készlet példány** | **Port** |
| --- | --- |
| FrontEnd_0 |3389 |
| FrontEnd_1 |3390 |
| FrontEnd_2 |3391 |
| FrontEnd_3 |3392 |
| FrontEnd_4 |3393 |
| FrontEnd_5 |3394 |

### <a name="step-3-remote-connect-toohello-specific-vm-scale-set-instance"></a>3. lépés: Távoli csatlakozás toohello Virtuálisgép-méretezési csoportban példány
Az alábbi képernyőképen hello használata a távoli asztali kapcsolat tooconnect toohello FrontEnd_1:

![RDP][RDP]

## <a name="how-toochange-hello-rdp-port-range-values"></a>Hogyan toochange hello RDP-portjára tartomány értékek
### <a name="before-cluster-deployment"></a>Fürttelepítés előtt
Hello fürt Resource Manager-sablonnal állít be, amikor megadhat hello tartomány hello **inboundNatPools**.

Nyissa meg az erőforrás-definíció toohello **Microsoft.Network/loadBalancers**. Amely alatt található hello leírását **inboundNatPools**.  Cserélje le a hello *frontendportrangestart értékénél* és *frontendportrangeend értékének* értékeket.

![InboundNatPools][InboundNatPools]

### <a name="after-cluster-deployment"></a>Fürt telepítése után
Ez egy kicsit bonyolultabb, és azt eredményezheti, hello virtuális gépek újbóli felhasználását. Most kell tooset Azure PowerShell használatával új értékeket. Győződjön meg arról, hogy az Azure PowerShell 1.0-s vagy újabb verziója telepítve van-e a számítógépen. Ha nem ezt megelőzően, I erősen javasolt lépéseket hello leírt [hogyan tooinstall Azure PowerShell és konfigurálása.](/powershell/azure/overview)

Bejelentkezés tooyour Azure-fiók. Ha a PowerShell-parancs valamilyen okból nem sikerül, ellenőrizze az Azure PowerShell megfelelően telepítve van-e.

```
Login-AzureRmAccount
```

Futtassa a következő tooget adatok a terheléselosztón hello és hello leírását hello értéke látható **inboundNatPools**:

```
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name>
```

Most már készen *frontendportrangeend értékének* és *frontendportrangestart értékénél* toohello kívánt értékeket.

```
$PropertiesObject = @{
    #Property = value;
}
Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName <RG name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load Balancer name> -ApiVersion <use hello API version that get returned> -Force
```


## <a name="next-steps"></a>Következő lépések
* [Hello "Bárhol rendszerbe állítás" szolgáltatás és az Azure által kezelt fürtökkel összehasonlítása áttekintése](service-fabric-deploy-anywhere.md)
* [Fürtbiztonság](service-fabric-cluster-security.md)
* [Service Fabric SDK és az első lépések](service-fabric-get-started.md)

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png
