---
title: "aaaVM újraindításával és átméretezésével állít ki az Azure-ban |} Microsoft Docs"
description: "A újraindításával és átméretezésével egy meglévő Linux virtuális gépet az Azure Resource Manager telepítési problémák elhárításához"
services: virtual-machines-linux, azure-resource-manager
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 51f1610c-0290-464a-97d4-c2e485c7ae7f
ms.service: virtual-machines-linux
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.workload: required
ms.date: 01/10/2017
ms.author: delhan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d9ff76b64b6646b04565eb93110759c4ebc858e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deployment-issues-with-restarting-or-resizing-an-existing-linux-vm-in-azure"></a>Központi telepítés elhárítása újraindításával és átméretezésével egy meglévő Linux virtuális Gépre az Azure-ban
Próbálja toostart leállított Azure virtuális gép (VM), vagy egy meglévő Azure virtuális gép átméretezése, hello előforduló gyakori hiba esetén egy memóriafoglalási hiba. Ez a hiba eredménye, amikor hello fürt vagy a régió vagy nem rendelkezik elérhető erőforrások vagy támogatási hello nem kért Virtuálisgép-méretet.

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="collect-activity-logs"></a>A gyűjtés tevékenység naplói
hibaelhárítási, toostart gyűjtése hello tevékenység hello probléma tooidentify hello hibákat naplózza. a következő hivatkozások hello hello folyamat részletes információkat tartalmaznak:

[Üzembe helyezési műveletek megtekintése](../../azure-resource-manager/resource-manager-deployment-operations.md)

[Tevékenység naplók toomanage Azure megtekintése erőforrások](../../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a>Hiba: Hiba a leállított virtuális gép indításakor
A leállt virtuális gépek toostart próbálja, de egy memóriafoglalási hiba beolvasása.

### <a name="cause"></a>Ok
hello kérelem toostart hello leállt a virtuális gép toobe kísérelték meg futtatni: hello eredeti fürt hello felhőszolgáltatást üzemeltető rendelkezik. Hello fürt azonban nem rendelkezik szabad terület érhető el toofulfill hello kérelem.

### <a name="resolution"></a>Megoldás:
* Állítsa le a virtuális gépek hello rendelkezésre állási állítsa be, és indítsa újra az egyes virtuális gépek összes hello.
  
  1. Kattintson a **erőforráscsoportok** > *az erőforráscsoport* > **erőforrások** > *a rendelkezésre állási csoport* > **virtuális gépek** > *a virtuális gép* > **leállítása**.
  2. Amikor az összes virtuális gépek leállítási hello válassza ki minden egyes hello leállt virtuális, válassza az Indítás parancsot.
* Ismételje meg a hello Újraindítási kérést később.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Hiba: Hiba a meglévő virtuális átméretezése
Próbálkozzon a meglévő virtuális tooresize, de egy memóriafoglalási hiba beolvasása.

### <a name="cause"></a>Ok
hello kérelem tooresize hello VM rendelkezik toobe kísérelték meg futtatni: hello eredeti fürt, amely a gazdagépek hello felhőszolgáltatás. Azonban nem támogatja a hello fürt hello a kért Virtuálisgép-méretet.

### <a name="resolution"></a>Megoldás:
* Próbálja megismételni a kérelmet hello kisebb Virtuálisgép-méretet.
* Ha hello a hello mérete nem lehet módosítani a virtuális gép kért:
  
  1. Állítsa le a rendelkezésre állási csoport hello hello virtuális gépeinek.
     
     * Kattintson a **erőforráscsoportok** > *az erőforráscsoport* > **erőforrások** > *a rendelkezésre állási csoport* > **virtuális gépek** > *a virtuális gép* > **leállítása**.
  2. Ha minden hello virtuális gépek leállítása, méretezze át a kívánt hello VM tooa nagyobb méretű.
  3. Jelölje be hello átméretezni a virtuális Gépet, és kattintson **Start**, majd elindítása minden hello leállt virtuális gépek.

## <a name="next-steps"></a>Következő lépések
Ha tapasztal, amikor új Linux virtuális gép létrehozása az Azure-ban, tekintse meg [egy új Linux virtuális gép létrehozása az Azure-ban a telepítési problémák elhárításához](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

