---
title: "aaaUpgrade egy Azure virtuálisgép-méretezési csoport |} Microsoft Docs"
description: "Egy Azure virtuálisgép-méretezési csoport frissítése"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e229664e-ee4e-4f12-9d2e-a4f456989e5d
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: guybo
ms.openlocfilehash: 068e98503f8d37ea71e45b8673a01da2e814f521
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-a-virtual-machine-scale-set"></a>A virtuálisgép-méretezési csoport frissítése
Ez a cikk ismerteti, hogyan lehet megkezdik az operációs rendszer frissítési tooan Azure virtuálisgép-méretezési állásidő nélkül. Ebben a környezetben az operációs rendszer frissítés magában foglalja a hello verzió vagy hello az operációs rendszer Termékváltozata módosítása vagy egy egyéni lemezkép URI hello megváltoztatása. Erre a frissítése nélkül állásidő azt jelenti, hogy frissítési egyszerre helyett virtuális gépet egyszerre, vagy a csoportok (például egy tartalék tartomány egyszerre). Ezzel a módszerrel bármely nem frissített virtuális gépek adatközpontnak futnia.

tooavoid kétértelműség most megkülönböztetni a négy típusú operációs rendszer frissítés-érdemes tooperform:

* Hello verzió vagy platformlemezkép Termékváltozata módosítása. Például megváltoztathatja az Ubuntu 14.04.201506100 14.04.2-LTS verziójában too14.04.201507060 vagy hello Ubuntu 15.10/legújabb SKU too16.04.0-LTS/latest módosítása. Ebben a forgatókönyvben cikkben szereplő.
* URI, amely egy egyéni lemezkép parancsfájlkezelő új verziója tooa mutat módosítása hello (**tulajdonságok** > **virtualMachineProfile** > **storageProfile**  >  **osDisk** > **kép** > **uri**). Ebben a forgatókönyvben cikkben szereplő.
* Egy Azure felügyelt lemezekkel létrehozott méretezési hello Képhivatkozás módosítása.
* A virtuális gépekről az operációs rendszer javítását hello (ezt például a biztonsági javítás telepítése és a Windows Update futtatása). Ez a forgatókönyv használata támogatott, de nem tartalmazza az ebben a cikkben.

Virtuálisgép-méretezési csoportok részeként üzembe helyezett egy [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) fürthöz nem tartoznak ide. Lásd: [javítás Windows operációs rendszer a Service Fabric-fürt](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-patch-orchestration-application) további információt a Service Fabric javítását.

hello alapvető sorrend módosítása a az operációs rendszer verziója/Termékváltozata-platformlemezkép hello, vagy egy egyéni lemezkép URI hello a következőképpen néz ki:

1. Virtuális gép méretezési modellel hello beolvasása.
2. Hello verzió módosítása, SKU, Képhivatkozás vagy URI azonosítóját hello modellben.
3. Frissítési hello modell.
4. Hajtsa végre a *manualUpgrade* hívás hello méretezési csoportban lévő hello virtuális gépeken. Ez a lépés csak akkor releváns Ha *upgradePolicy* értéke túl**manuális** a méretezési készletben. Ha a beállítás túl**automatikus**, az összes hello virtuális gép frissítése egyszerre, így üzemkiesése.

Vegye figyelembe az információ nézzük meg, hogyan sikerült frissíteni a PowerShellben és hello REST API használatával terjedő skálán hello verzióját. Ezekben a példákban a platform-lemezkép hello esetekre, de ez a cikk az Ön tooadapt elegendő információt biztosít a folyamat tooa egyéni lemezképet.

## <a name="powershell"></a>PowerShell
Ez a példa frissíti a Windows virtuálisgép-méretezési csoport (létrehozásakor toohello új verzió 4.0.20160229. Miután frissített hello modell, végez el egy frissítés egy virtuálisgép-példány egyszerre.

```powershell
$rgname = "myrg"
$vmssname = "myvmss"
$newversion = "4.0.20160229"
$instanceid = "1"

# get hello VMSS model
$vmss = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname

# set hello new version in hello model data
$vmss.virtualMachineProfile.storageProfile.imageReference.version = $newversion

# update hello virtual machine scale set model
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $vmss

# now start updating instances
Update-AzureRmVmssInstance -ResourceGroupName $rgname -VMScaleSetName $vmssname -InstanceId $instanceId
```

A platform lemezkép verziója módosítása helyett egyéni kép URI hello frissítésekor, cserélje le a "hello új verzió beállítása" hello sor egy parancs frissíti hello forrás lemezkép URI. Például ha hello méretezési Azure felügyelt lemezek használata nélkül hozták létre, hello frissítés volna néznek ki:

```powershell
# set hello new version in hello model data
$vmss.virtualMachineProfile.storageProfile.osDisk.image.uri= $newURI
```

Ha egyéni lemezkép-alapú méretezési csoportban hozták létre Azure kezelt lemezeken, akkor hello Képhivatkozás frissíti a szeretné. Példa:

```powershell
# set hello new version in hello model data
$vmss.virtualMachineProfile.storageProfile.imageReference.id = $newImageReference
```

## <a name="hello-rest-api"></a>hello REST API-n
Az alábbiakban néhány hello Azure REST API tooroll ki az operációs rendszer verziója frissítés használó Python példák. Mindkettő használja a hello lightweight [azurerm](https://pypi.python.org/pypi/azurerm) Azure REST API burkoló funkciók toodo hello skálán GET könyvtárának beállítása modell egy frissített modellel PUT követ. Akkor is tekintse meg virtuálisgép-példányok nézetek tooidentify hello virtuális gépek által frissítési tartományt.

### <a name="vmssupgrade"></a>Vmssupgrade
 [Vmssupgrade](https://github.com/gbowerman/vmsstools) egy Python-parancsfájl által használt ki egy virtuálisgép-méretezési futó operációs rendszer frissítési tooa tooroll frissítési tartományok be egyszerre.

![A virtuális gépek vagy egy frissítési tartományt Vmssupgrade parancsfájl](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssupgrade-screenshot.png)

Ez a parancsfájl lehetővé teszi adott virtuális gépek tooupdate válasszon vagy adjon meg egy frissítési tartományt. A platform lemezkép verziója módosítása vagy egy egyéni lemezkép URI hello megváltoztatása támogatja.

### <a name="vmsseditor"></a>Vmsseditor
[Vmsseditor](https://github.com/gbowerman/vmssdashboard) van egy általános célú szerkesztő virtuálisgép-méretezési készlet, amely tartalmazza a virtuális gép állapotát a heatmap, ahol több sorban is jelenti a frissítési tartományok. Többek között hello modell skálázási készletének frissítése egy új verziója, SKU vagy egyéni lemezkép URI, és beállíthatja, majd válassza ki a tartalék tartományok tooupgrade használni. Ha így tesz, a frissítési tartomány összes hello virtuális gépek a frissített toohello új modell. Másik lehetőségként a működés közbeni frissítés az Ön által választott hello köteg mérete alapján végezhet.  

hello következő képernyőfelvétel egy modell a skála Ubuntu 14.04-2LTS 14.04.201507060 verzió beállítása. Számos további lehetőség toothis eszköz váltak elérhetővé, mert ezen a képernyőfelvételen látható kerül.

![A skála Ubuntu 14.04-2LTS beállítása Vmsseditor modell](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor1.png)

Miután rákattintott **frissítése** , majd **részletek beszerzése**, UD 0 lévő virtuális gépek elindítása tooupdate.

![Vmsseditor ábrázoló frissítése folyamatban](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor2.png)

