---
title: "Windows virtuális gép üzembe helyezéséhez az Azure-ban aaaTroubleshoot |} Microsoft Docs"
description: "Erőforrás-kezelő telepítési problémák elhárításához, amikor egy új Windows virtuális gép létrehozása az Azure-ban"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: willchen
editor: 
tags: top-support-issue, azure-resource-manager
ms.assetid: afc6c1a4-2769-41f6-bbf9-76f9f23bcdf4
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: cjiang
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c27d4f63b67db2d1c9f117f05a2eba9ef130ef37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deployment-issues-when-creating-a-new-windows-vm-in-azure"></a>Telepítési problémák elhárításához, amikor egy új Windows virtuális gép létrehozása az Azure-ban
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="top-issues"></a>Problémák
[!INCLUDE [support-disclaimer](../../../includes/virtual-machines-windows-troubleshoot-deploy-vm-top.md)]

Más virtuális gép telepítési problémákat és kérdéseket: [elhárítása telepítését Windows virtuális gép az Azure-ban](troubleshoot-deploy-vm.md).

## <a name="collect-activity-logs"></a>A gyűjtés tevékenység naplói
hibaelhárítási, toostart gyűjtése hello tevékenység hello probléma tooidentify hello hibákat naplózza. hello következő hivatkozások részletes információkat tartalmaznak hello folyamat toofollow.

[Üzembe helyezési műveletek megtekintése](../../azure-resource-manager/resource-manager-deployment-operations.md)

[Tevékenység naplók toomanage Azure megtekintése erőforrások](../../resource-group-audit.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

**Y** Ha hello az operációs rendszer általánosított, Windows és feltöltött, illetve a általánosítva hello beállítása rögzített, akkor nem lesz ki a hibákat. Hasonlóképpen ha hello az operációs rendszer Windows kifejezetten, és a feltöltött, illetve a rögzített hello speciális beállítás, nem lesz ki a hibákat, majd.

**Töltse fel a hibák:**

**N<sup>1</sup>:** Ha hello az operációs rendszer Windows általánosítva van, és mint feltöltött speciális, a virtuális gép Beragadt hello OOBE képernyő hello létesítési időtúllépési hiba fog kapni.

**N<sup>2</sup>:** Windows kifejezetten, és töltheti fel, mivel általánosítva hello az operációs rendszer esetén a virtuális gép Beragadt hello OOBE képernyő, mert hello új virtuális gép fut az eredeti számítógép hello hello egy üzembe helyezési hiba hiba jelenik-e nevét, a felhasználónév és jelszó.

**Megoldás**

tooresolve mindkét ezeket a hibákat, használjon [Add-AzureRmVhd tooupload hello eredeti VHD](https://msdn.microsoft.com/library/mt603554.aspx), érhető el a helyszíni, az hello hello az operációs rendszer (általánosítva/speciális), mint a azonos beállításait. tooupload, általánosítva van, ne feledje toorun sysprep először.

**Rögzítés a hibák:**

**N<sup>3</sup>:** Ha hello az operációs rendszer Windows általánosítva van, és mint rögzítése speciális, elérhetővé válik egy üzembe helyezési időtúllépési hiba mert hello eredeti virtuális gép nem használható, mivel általánosítva van megjelölve.

**N<sup>4</sup>:** Windows kifejezetten, és mivel általánosítva rögzítése az operációs rendszer hello esetén elérhetővé válik egy üzembe helyezési hiba mert hello új virtuális gép fut-e hello eredeti számítógép neve, a felhasználónevet és jelszót. Hello eredeti virtuális gép is nem használható, mert meg van jelölve a speciális.

**Megoldás**

tooresolve mindkét ezeket a hibákat, hello aktuális lemezkép törlése hello portálról és [vegye fel újra hello a jelenlegi VHD-k](create-vm-specialized.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a hello ugyanaz, mint a hello (általánosítva/speciális) operációs rendszer beállítása.

## <a name="issue-customgallerymarketplace-image-allocation-failure"></a>Probléma: Egyéni/gyűjtemény/Piactéri lemezképhez; foglalási hiba
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
Ha problémát tapasztal a leállított Windows virtuális gépek elindításakor vagy egy meglévő Windows Azure-ban átméretezése, lásd: [újraindításával és átméretezésével egy meglévő Windows rendszerű virtuális gép az Azure erőforrás-kezelő hibaelhárítása telepítési problémákat](restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

