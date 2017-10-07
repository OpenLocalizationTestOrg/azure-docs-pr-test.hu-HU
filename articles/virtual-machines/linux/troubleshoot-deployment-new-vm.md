---
title: "Linux virtuális gép telepítési-RM aaaTroubleshoot |} Microsoft Docs"
description: "Erőforrás-kezelő telepítési problémák elhárításához, amikor egy új Linux virtuális gép létrehozása az Azure-ban"
services: virtual-machines-linux, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue, azure-resource-manager
ms.assetid: 906a9c89-6866-496b-b4a4-f07fb39f990c
ms.service: virtual-machines-linux
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/09/2016
ms.author: cjiang
ms.openlocfilehash: 2dd7f1855bba75d86eb90f88e6d573cd42fd8d87
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a>Az új Linux virtuális gép létrehozása az Azure Resource Manager telepítési problémák elhárításához
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="top-issues"></a>Problémák
[!INCLUDE [support-disclaimer](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

Más virtuális gép telepítési problémákat és kérdéseket: [elhárítása telepítését Linux virtuális gép az Azure-ban](troubleshoot-deploy-vm.md).
## <a name="collect-activity-logs"></a>A gyűjtés tevékenység naplói
hibaelhárítási, toostart gyűjtése hello tevékenység hello probléma tooidentify hello hibákat naplózza. hello következő hivatkozások részletes információkat tartalmaznak hello folyamat toofollow.

[Üzembe helyezési műveletek megtekintése](../../azure-resource-manager/resource-manager-deployment-operations.md)

[Tevékenység naplók toomanage Azure megtekintése erőforrások](../../resource-group-audit.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**Y** Ha hello az operációs rendszer általánosított, Linux és feltöltött, illetve a általánosítva hello beállítása rögzített, akkor nem lesz ki a hibákat. Ehhez hasonlóan hello az operációs rendszer esetén Linux kifejezetten, és a feltöltött, illetve a rögzített hello kifejezetten beállítást, majd a hibák nem lesz.

**Töltse fel a hibák:**

**N<sup>1</sup>:** Ha hello az operációs rendszer általánosított Linux, és mint feltöltött speciális, elérhetővé válik egy üzembe helyezési időtúllépési hiba hello VM Beragadt hello szakasz kiépítés, mert.

**N<sup>2</sup>:** Linux kifejezetten, és töltheti fel, mivel általánosítva hello az operációs rendszer esetén elérhetővé válik egy üzembe helyezési hiba mert hello új virtuális gép fut hello eredeti számítógép neve, a felhasználónévvel és jelszóval.

**Megoldás:**

tooresolve mindkét ezeket a hibákat, feltöltése hello eredeti VHD, érhető el a helyszínen, a hello ugyanaz, mint a hello (általánosítva/speciális) operációs rendszer beállítása. tooupload, általánosítva van, ne feledje toorun-először kiosztásának megszüntetése.

**Rögzítés a hibák:**

**N<sup>3</sup>:** Ha hello az operációs rendszer általánosított Linux, és mint rögzítése speciális, elérhetővé válik egy üzembe helyezési időtúllépési hiba mert hello eredeti virtuális gép nem használható, mivel általánosítva van megjelölve.

**N<sup>4</sup>:** Linux kifejezetten, és mivel általánosítva rögzítése az operációs rendszer hello esetén elérhetővé válik egy üzembe helyezési hiba mert hello új virtuális gép fut hello eredeti számítógép neve, a felhasználónévvel és jelszóval. Hello eredeti virtuális gép is nem használható, mert meg van jelölve a speciális.

**Megoldás:**

tooresolve mindkét ezeket a hibákat, hello aktuális lemezkép törlése hello portálról és [vegye fel újra hello a jelenlegi VHD-k](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) a hello ugyanaz, mint a hello (általánosítva/speciális) operációs rendszer beállítása.

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Probléma: Egyéni / gyűjtemény / Piactéri lemezképhez; foglalási hiba
Ez a hiba akkor jelentkeznek helyzetekben, amikor hello új virtuális gép kérelme, mert rögzített tooa fürt, amely nem támogatja a kért hello Virtuálisgép-méretet, vagy nem rendelkezik a rendelkezésre álló szabad területet tooaccommodate hello kérelem.

**1. ok:** hello fürt nem támogatja a kért Virtuálisgép-méretet hello.

**1. megoldás:**

* Próbálja megismételni a kérelmet hello kisebb Virtuálisgép-méretet.
* Ha hello a hello mérete nem lehet módosítani a virtuális gép kért:
  * Állítsa le a rendelkezésre állási csoport hello hello virtuális gépeinek.
    Kattintson a **erőforráscsoportok** > *az erőforráscsoport* > **erőforrások** > *a rendelkezésre állási csoport* > **virtuális gépek** > *a virtuális gép* > **leállítása**.
  * Az összes hello virtuális gépek leállítása, létre kell hoznia a hello hello az új virtuális gép mérete szükséges.
  * Indítsa el először hello új virtuális Gépet, és jelölje hello le virtuális gépeket, és kattintson **Start**.

**2. ok:** hello fürtnek nincs szabad erőforrást.

**2. megoldás:**

* Ismételje meg a hello kérelmet egy későbbi időpontban.
* Ha hello új virtuális gép is része egy másik rendelkezésre állási beállítani
  * Hozzon létre egy új virtuális Gépet egy másik rendelkezésre állási készlet (a hello ugyanabban a régióban).
  * Adja hozzá a hello új virtuális gép toohello ugyanazt a virtuális hálózatot.

## <a name="next-steps"></a>Következő lépések
Ha problémát tapasztal, amikor elindítja a Linux virtuális gép leállított vagy méretezze át egy meglévő Linux virtuális Gépre az Azure-ban, tekintse meg [újraindításával és átméretezésével egy meglévő Linux virtuális gépet az Azure erőforrás-kezelő hibaelhárítása telepítési problémákat](restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

