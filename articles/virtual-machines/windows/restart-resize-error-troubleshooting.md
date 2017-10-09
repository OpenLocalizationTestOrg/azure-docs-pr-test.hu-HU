---
title: "aaaVM újraindításával és átméretezésével állít ki az Azure-ban |} Microsoft Docs"
description: "A újraindításával és átméretezésével egy meglévő Windows rendszerű virtuális gép az Azure Resource Manager telepítési problémák elhárításához"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 0756b52d-4f5a-4503-ae45-c00a6a2edcdf
ms.service: virtual-machines-windows
ms.topic: support-article
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.workload: required
ms.date: 06/13/2017
ms.author: delhan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2cf7c2d19bf5f79fab4ffc0eff9ccc1182d601c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deployment-issues-with-restarting-or-resizing-an-existing-windows-vm-in-azure"></a>Központi telepítés elhárítása újraindításával és átméretezésével egy meglévő Windows Azure-ban
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
Ha tapasztal, amikor új Windows virtuális gép létrehozása az Azure-ban, tekintse meg [egy új Windows virtuális gép létrehozása az Azure-ban a telepítési problémák elhárításához](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

