---
title: "aaaVM újraindításával és átméretezésével kapcsolatos problémák |} Microsoft Docs"
description: "Klasszikus üzembe helyezési elhárítása újraindításával és átméretezésével egy meglévő Linux virtuális gépet az Azure-ban"
services: virtual-machines-linux
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 73f2672c-602e-4766-8948-2b180115d299
ms.service: virtual-machines-linux
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.workload: required
ms.date: 01/10/2017
ms.devlang: na
ms.author: delhan
ms.openlocfilehash: fb1dc88bb1b83043c434590118bc8810ad402872
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-linux-virtual-machine-in-azure"></a>Klasszikus üzembe helyezési elhárítása újraindításával és átméretezésével egy meglévő Linux virtuális gépet az Azure-ban
> [!div class="op_single_selector"]
> * [Klasszikus](restart-resize-error-troubleshooting.md)
> * [Resource Manager](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> 
> 

Próbálja toostart leállított Azure virtuális gép (VM), vagy egy meglévő Azure virtuális gép átméretezése, hello előforduló gyakori hiba esetén egy memóriafoglalási hiba. Ez a hiba eredménye, amikor hello fürt vagy a régió vagy nem rendelkezik elérhető erőforrások vagy támogatási hello nem kért Virtuálisgép-méretet.

> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Hello Resource Manager-verziójáért lásd: [Itt](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>A gyűjtés auditnaplókat
hibaelhárítás, toostart gyűjtése hello naplózási tooidentify hello hiba hello probléma társított naplózza.

Hello Azure-portálon, kattintson **Tallózás** > **virtuális gépek** > *a Linux virtuális gép*  >   **Beállítások** > **naplók**.

## <a name="issue-error-when-starting-a-stopped-vm"></a>Hiba: Hiba a leállított virtuális gép indításakor
A leállt virtuális gépek toostart próbálja, de egy memóriafoglalási hiba beolvasása.

### <a name="cause"></a>Ok
hello kérelem toostart hello leállt a virtuális gép toobe kísérelték meg futtatni: hello eredeti fürt hello felhőszolgáltatást üzemeltető rendelkezik. Hello fürt azonban nem rendelkezik szabad terület érhető el toofulfill hello kérelem.

### <a name="resolution"></a>Megoldás:
* Új felhőalapú szolgáltatás létrehozása és társítása vagy egy régiót vagy régió-alapú virtuális hálózatokon, de nem affinitáscsoport.
* Törlés hello VM leállítása.
* Hozza létre a VM hello hello új felhőszolgáltatásban hello lemezek használatával.
* Indítsa el hello újból létrehozza a virtuális gép.

Új felhőalapú szolgáltatás toocreate megkísérlésekor hibaüzenetet kap, ha később próbálkozzon újra, vagy hello régió hello felhőalapú szolgáltatás módosítása.

> [!IMPORTANT]
> hello új felhőalapú szolgáltatás lesz egy új nevet és a VIP, ezért szüksége lesz toochange ezt az információt hello meglévő felhőszolgáltatás adatokat használó összes hello-függőség.
> 
> 

## <a name="issue-error-when-resizing-an-existing-vm"></a>Hiba: Hiba a meglévő virtuális átméretezése
Próbálkozzon a meglévő virtuális tooresize, de egy memóriafoglalási hiba beolvasása.

### <a name="cause"></a>Ok
hello kérelem tooresize hello VM rendelkezik toobe kísérelték meg futtatni: hello eredeti fürt, amely a gazdagépek hello felhőszolgáltatás. Azonban nem támogatja a hello fürt hello a kért Virtuálisgép-méretet.

### <a name="resolution"></a>Megoldás:
Csökkentse hello a kért Virtuálisgép-méretet, és újra hello méretezze át a kérést.

* Kattintson a **összes tallózása** > **virtuális gépek (klasszikus)** > *a virtuális gép* > **beállítások** > **mérete**. Részletes útmutató: [átméretezni a virtuális gépet hello](https://msdn.microsoft.com/library/dn168976.aspx).

Ha nem lehetséges tooreduce hello Virtuálisgép-méretet, kövesse az alábbi lépéseket:

* Hozzon létre egy új felhőalapú szolgáltatás, amely nem csatolt tooan affinitáscsoportban, és nincs hozzárendelve virtuális hálózat, amely csatolt tooan affinitáscsoportban.
* Hozzon létre egy új, a nagyobb méretű virtuális azt.

A virtuális gépek képes egyesíteni a hello ugyanaz a felhőalapú szolgáltatás. Ha a meglévő felhőalapú szolgáltatást a terület-alapú virtuális hálózathoz társítva, hello új felhőalapú szolgáltatás toohello már meglévő virtuális hálózatot is elérheti.

Ha hello meglévő felhőalapú szolgáltatás nincs hozzárendelve régió-alapú virtuális hálózat, majd rendelkezik toodelete hello virtuális gépek hello meglévő felhőszolgáltatásban, és hozza létre őket hello új felhőalapú szolgáltatás a lemezekről. Azt azonban fontos, hogy hello új felhőalapú szolgáltatás lesz egy új nevet és a VIP, így tooupdate kell az összes hello függőségek, amelyek jelenleg használják ezeket az információkat hello meglévő felhőszolgáltatás tooremember.

## <a name="next-steps"></a>Következő lépések
Ha tapasztal, amikor új Linux virtuális gép létrehozása az Azure-ban, tekintse meg [egy új Linux virtuális gép létrehozása az Azure-ban a telepítési problémák elhárításához](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

