---
title: "a Service Fabric aaaScale fürt bejövő vagy kimenő |} Microsoft Docs"
description: "Bővítse a Service Fabric-fürt bejövő vagy kimenő toomatch igény szerinti automatikus skálázása szabályok beállítása az egyes csomópont típus vagy virtuális gép méretezési készlet. Hozzáadása vagy eltávolítása, csomópontok tooa Service Fabric-fürt"
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: aeb76f63-7303-4753-9c64-46146340b83d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/22/2017
ms.author: chackdan
ms.openlocfilehash: 37cfeaf80edc016cf6de017d1c2dc6fbcb8acc2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scale-a-service-fabric-cluster-in-or-out-using-auto-scale-rules"></a>Bejövő vagy kimenő automatikus méretezése szabályok használatával a Service Fabric-fürt méretezése
Virtuálisgép-méretezési csoportok olyan Azure számítási erőforrás toodeploy használja és a virtuális gépek készletként gyűjteményeinek kezelését is. Minden csomópont-típus, a Service Fabric-fürt definiált egy külön virtuálisgép-méretezési csoport lett beállítva. Az egyes csomóponttípusok majd méretezhetők a kimenő portok nyitva különböző tulajdonságkészletekkel rendelkező egymástól függetlenül, illetve különböző teljesítmény-mérőszámait rendelkezhet. További információk a hello [Service Fabric NodeType tulajdonságok értéke](service-fabric-cluster-nodetypes.md) dokumentum. Hello Service Fabric csomóponttípusok a fürt virtuálisgép-méretezési csoportok: hello háttér épülnek, mivel minden egyes csomópont típus vagy virtuális gép méretezési kell tooset automatikus méretezése szabályokat.

> [!NOTE]
> Az előfizetés elég új virtuális gépek, a fürtöt alkotó hello magok tooadd kell rendelkeznie. Jelenleg nincs Modellellenőrzés, hogy egy központi telepítési idő hiba, ha bármelyik hello kvótakorlát találati.
> 
> 

## <a name="choose-hello-node-typevirtual-machine-scale-set-tooscale"></a>Válassza ki a hello csomópont típus vagy virtuális gép méretezési tooscale beállítása
Jelenleg nem képes toospecify hello automatikus méretezése virtuálisgép-méretezési csoportok hello portálon szabályait, így tudassa velünk Azure PowerShell (1.0 +) toolist hello csomóponttípusok majd automatikus méretezése szabályok toothem.

a virtuálisgép-méretezési csoport, amely tooget hello listája jött létre a fürthöz, futtassa a következő parancsmagok hello:

```powershell
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Compute/VirtualMachineScaleSets

Get-AzureRmVmss -ResourceGroupName <RGname> -VMScaleSetName <Virtual Machine scale set name>
```

## <a name="set-auto-scale-rules-for-hello-node-typevirtual-machine-scale-set"></a>Hello csomópont típus vagy virtuális gép méretezési automatikus méretezése-szabályok
Ha a fürt több csomópont-típust, majd ismételje meg a minden egyes csomópont típusok/virtuális gépek méretezési állít be, amelyet tooscale (bejövő vagy kimenő). Csomópontok hello számát figyelembe vennie rendelkeznie kell az automatikus skálázás beállítása előtt. rendelkeznie kell elsődleges csomóponttípus hello csomópontok minimális száma hello célja a kiválasztott hello megbízhatóság szintje. Tudjon meg többet az [megbízhatóságának](service-fabric-cluster-capacity.md).

> [!NOTE]
> Skálázás hello elsődleges csomópont típus tooless le, mint hello minimális száma hello fürt instabillá tehetik vagy érdekében, hogy. Az alkalmazások és -szolgáltatások hello adatvesztést eredményezhet.
> 
> 

Jelenleg hello automatikus méretezése a szolgáltatás nem célja, hogy az alkalmazások is készítőnek tekintene tooService háló hello terhelések. Ezért a idő hello, automatikus méretezése kap tisztán célja a hello teljesítményszámlálók hello virtuálisgép-méretezési készlet példányok mindegyikének által kibocsátott.  

A következő lépések követésével [be az egyes virtuálisgép-méretezési csoport automatikus méretezése tooset](../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md).

> [!NOTE]
> A forgatókönyv vertikális, kivéve, ha a csomóponttípus rendelkezik egy arany vagy ezüst tartóssági szintjének kell toocall hello [Remove-ServiceFabricNodeState parancsmag](https://msdn.microsoft.com/library/azure/mt125993.aspx) hello megfelelő csomópont névvel.
> 
> 

## <a name="manually-add-vms-tooa-node-typevirtual-machine-scale-set"></a>Adja hozzá manuálisan a virtuális gépek tooa csomópont típus vagy virtuális gép méretezési csoport
Hello hello minta/utasításokat követve [gyors üzembe helyezési sablon gyűjtemény](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) toochange hello számát a virtuális gépek minden egyes csomóponttípusban. 

> [!NOTE]
> A virtuális gépek hozzáadása időt vesz igénybe, így nem várható hello kiegészítéseket toobe azonnali. Ezért tooadd kapacitástervezés és az idő, mielőtt hello Virtuálisgép-kapacitást hello replikák több mint 10 percig tooallow, / service példányok tooget helyezni.
> 
> 

## <a name="manually-remove-vms-from-hello-primary-node-typevirtual-machine-scale-set"></a>Manuálisan távolítsa el virtuális gépek hello elsődleges csomópont típus vagy virtuális gép méretezési csoport
> [!NOTE]
> hello service fabric rendszerszolgáltatások hello elsődleges csomóponttípusok a fürtön futtatni. Ezért soha ne állítsa le vagy csökkentheti a csomóponttípusok található példányok száma hello kisebb, mint hogy milyen hello megbízhatósági szint indokol. Tekintse meg a túl[részleteit itt megbízhatóság rétegen hello](service-fabric-cluster-capacity.md). 
> 
> 

Tooexecute kell hello alábbi lépéseit egy Virtuálisgép-példány egyszerre. Ez lehetővé teszi a hello rendszerszolgáltatások (és az állapotalapú szolgáltatások) toobe leállítása ezzel a művelettel eltávolítja a hello Virtuálisgép-példány és a többi csomóponton létrehozott új replikák.

1. Futtatás [Disable-ServiceFabricNode](https://msdn.microsoft.com/library/mt125852.aspx) leképezési "RemoveNode" toodisable hello csomóponttal fog tooremove (hello adott csomóponttípus legmagasabb példány).
2. Futtatás [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) meg arról, hogy hello csomópont valóban átváltott toodisabled toomake. Ha nem, akkor várjon, amíg a hello csomópont le van tiltva. Ez a lépés nem hurry.
3. Hello hello minta/utasításokat követve [gyors üzembe helyezési sablon gyűjtemény](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) toochange hello a virtuális gépek számát egy adott csomóponttípusban. hello eltávolított példány hello legmagasabb Virtuálisgép-példány. 
4. Ismételje meg az 1 – 3 igény szerint, de soha nem csökkentheti a hello elsődleges csomóponttípusok kisebb milyen hello megbízhatósági szint indokol található példányok száma hello. Tekintse meg a túl[részleteit itt megbízhatóság rétegen hello](service-fabric-cluster-capacity.md). 

## <a name="manually-remove-vms-from-hello-non-primary-node-typevirtual-machine-scale-set"></a>Manuálisan távolítsa el virtuális gépek hello nem elsődleges csomópontot típus vagy virtuális gép méretezési csoport
> [!NOTE]
> Egy állapotalapú szolgáltatás kell bizonyos számú csomópontok toobe mindig toomaintain rendelkezésre állását és preserve állapotát a szolgáltatás. Nagyon minimális hello meg kell hello partíció/szolgáltatás csomópontok set egyenlő toohello cél replikaszám hello száma. 
> 
> 

Hello kell végrehajtani a következő lépéseket egy Virtuálisgép-példány egyszerre hello. Ez lehetővé teszi a toobe leállítása hello a Virtuálisgép-példány eltávolítása hello rendszerszolgáltatások (és az állapotalapú szolgáltatások), és új replikák létrehozott más helyét.

1. Futtatás [Disable-ServiceFabricNode](https://msdn.microsoft.com/library/mt125852.aspx) leképezési "RemoveNode" toodisable hello csomóponttal fog tooremove (hello adott csomóponttípus legmagasabb példány).
2. Futtatás [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) meg arról, hogy hello csomópont valóban átváltott toodisabled toomake. Ha nem várja meg, amíg hello csomópont le van tiltva. Ez a lépés nem hurry.
3. Hello hello minta/utasításokat követve [gyors üzembe helyezési sablon gyűjtemény](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) toochange hello a virtuális gépek számát egy adott csomóponttípusban. Ez a művelet most eltávolítja hello legmagasabb Virtuálisgép-példány. 
4. Ismételje meg az 1 – 3 igény szerint, de soha nem csökkentheti a hello elsődleges csomóponttípusok kisebb milyen hello megbízhatósági szint indokol található példányok száma hello. Tekintse meg a túl[részleteit itt megbízhatóság rétegen hello](service-fabric-cluster-capacity.md).

## <a name="behaviors-you-may-observe-in-service-fabric-explorer"></a>Viselkedéshez jelenhet meg a Service Fabric Explorerben
Ha a skálától beállítható egy fürt hello Service Fabric Explorer hello hello fürtbe (virtuálisgép-méretezési készlet példányok) csomópontok száma fogja tartalmazni.  Akkor a fürt működik, akkor megjelenik eltávolított hello csomópontot vagy Virtuálisgép-példány jelenik meg a nem kifogástalan állapotú, kivéve, ha meghívja a [Remove-ServiceFabricNodeState cmd](https://msdn.microsoft.com/library/mt125993.aspx) hello megfelelő csomópont névvel.   

Ez a viselkedés hello magyarázata itt található.

hello a Service Fabric Explorerben felsorolt csomópontra milyen hello Service Fabric rendszerszolgáltatások tükre (FM kifejezetten) csomópontok hello fürt kellett/rendelkezik hello száma ismer. Akkor hello meghatározott virtuálisgép-méretezési, hello virtuális gép törölve lett, azonban a FM rendszerszolgáltatás úgy továbbra is értelmezi, (amelyek csatlakoztatott toohello virtuális Gépet, amely törölve lett) hello csomóponton térjen vissza lesz. Ezért Service Fabric Explorer továbbra is toodisplay csomópontot (bár a hello állapota hibás vagy ismeretlen is lehet).

A sorrend toomake meg arról, hogy a csomópont eltávolítása, a virtuális gép eltávolításakor két lehetőség közül választhat:

1) Válassza ki a tartóssági szint arany vagy ezüst (rendelkezésre álló hamarosan) hello csomópont esetében a fürtben, amely lehetővé teszi az infrastruktúra integrációs hello. Amely majd automatikusan eltávolítja hello csomópontok a rendszerállapot-szolgáltatások (FM) akkor.
Tekintse meg a túl[hello részleteit itt tartóssági szinten](service-fabric-cluster-capacity.md)

2) Hello Virtuálisgép-példány mérete, ha szüksége van-e toocall hello [Remove-ServiceFabricNodeState parancsmag](https://msdn.microsoft.com/library/mt125993.aspx).

> [!NOTE]
> Service Fabric-fürtök csomópontok toobe bizonyos számú fel minden hello időt igénybe rendelés toomaintain rendelkezésre állási és preserve állapotban - hivatkozott tooas "kvórum fenntartása." Így is le minden hello gépeket hello fürt általában nem biztonságos tooshut kivéve, ha először elvégezte a [teljes biztonsági mentés a állapot](service-fabric-reliable-services-backup-restore.md).
> 
> 

## <a name="next-steps"></a>Következő lépések
A következő tooalso olvasási hello fürt kapacitás megtervezésének, fürt frissítése és particionálás szolgáltatások megismerése:

* [A fürt kapacitásának megtervezése](service-fabric-cluster-capacity.md)
* [Fürt frissítése](service-fabric-cluster-upgrade.md)
* [A maximális skálája partíció állapotalapú szolgáltatások](service-fabric-concepts-partitioning.md)

<!--Image references-->
[BrowseServiceFabricClusterResource]: ./media/service-fabric-cluster-scale-up-down/BrowseServiceFabricClusterResource.png
[ClusterResources]: ./media/service-fabric-cluster-scale-up-down/ClusterResources.png
