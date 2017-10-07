---
title: "Linux virtuális gép telepítési klasszikus aaaTroubleshoot |} Microsoft Docs"
description: "Klasszikus telepítési problémák elhárításához, amikor egy új Linux virtuális gép létrehozása az Azure-ban"
services: virtual-machines-linux
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: c8a963fa-6b2a-4c7a-a1f4-7793adb02b19
ms.service: virtual-machines-linux
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: cjiang
ms.openlocfilehash: a642bfd9a543cd6d2104b03fe04193d9352ccae1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-classic-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a>Egy új Linux virtuális gép létrehozása az Azure klasszikus üzembe helyezési problémáinak elhárítása
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-selectors](../../../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-selectors-include.md)]

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Ez a cikk hello erőforrás-kezelő verziója: [Itt](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>A gyűjtés auditnaplókat
hibaelhárítás, toostart gyűjtése hello naplózási tooidentify hello hiba hello probléma társított naplózza.

Hello Azure-portálon, kattintson **Tallózás** > **virtuális gépek** > *a Windows rendszerű virtuális gép*  >   **Beállítások** > **naplók**.

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**Y** Ha hello az operációs rendszer általánosított, Linux és feltöltött, illetve a általánosítva hello beállítása rögzített, akkor nem lesz ki a hibákat. Ehhez hasonlóan hello az operációs rendszer esetén Linux kifejezetten, és a feltöltött, illetve a rögzített hello kifejezetten beállítást, majd a hibák nem lesz.

**Töltse fel a hibák:**

**N<sup>1</sup>:** Ha hello az operációs rendszer általánosított Linux, és mint feltöltött speciális, elérhetővé válik egy üzembe helyezési időtúllépési hiba hello VM Beragadt hello szakasz kiépítés, mert.

**N<sup>2</sup>:** Linux kifejezetten, és töltheti fel, mivel általánosítva hello az operációs rendszer esetén elérhetővé válik egy üzembe helyezési hiba mert hello új virtuális gép fut hello eredeti számítógép neve, a felhasználónévvel és jelszóval.

**Megoldás:**

tooresolve mindkét ezeket a hibákat, feltöltése hello eredeti VHD, érhető el a helyszínen, a hello ugyanaz, mint a hello (általánosítva/speciális) operációs rendszer beállítása. tooupload, általánosítva van, ne feledje toorun-először kiosztásának megszüntetése. Lásd: [létrehozása és feltöltése, hogy Contains hello Linux operációs rendszer virtuális merevlemez](create-upload-vhd.md) további információt.

**Rögzítés a hibák:**

**N<sup>3</sup>:** Ha hello az operációs rendszer általánosított Linux, és mint rögzítése speciális, elérhetővé válik egy üzembe helyezési időtúllépési hiba mert hello eredeti virtuális gép nem használható, mivel általánosítva van megjelölve.

**N<sup>4</sup>:** Linux kifejezetten, és mivel általánosítva rögzítése az operációs rendszer hello esetén elérhetővé válik egy üzembe helyezési hiba mert hello új virtuális gép fut hello eredeti számítógép neve, a felhasználónévvel és jelszóval. Hello eredeti virtuális gép is nem használható, mert meg van jelölve a speciális.

**Megoldás:**

tooresolve mindkét ezeket a hibákat, hello aktuális lemezkép törlése hello portálról és [vegye fel újra hello a jelenlegi VHD-k](capture-image.md) a hello ugyanaz, mint a hello (általánosítva/speciális) operációs rendszer beállítása.

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Probléma: Egyéni / gyűjtemény / Piactéri lemezképhez; foglalási hiba
Ez a hiba merül fel helyzetek hello új virtuális gép kérelem tooa fürt, vagy nincs rendelkezésre álló szabad területet tooaccommodate hello kérelem elküldésekor, vagy nem támogatja a kért hello Virtuálisgép-méretet. Már nem lehetséges toomix másik adatsorozathoz hello a virtuális gépek ugyanazt a felhőalapú szolgáltatás. Ezért ha egy új virtuális gép mérete eltér a felhőalapú szolgáltatás támogathatja a toocreate, hello számítási kérelme sikertelen lesz.

Attól függően, hogy hello megkötések hello felhőszolgáltatás használja toocreate hello új virtuális Gépet, akkor léphetnek fel két esetben által okozott hiba.

**1. ok:** hello felhőszolgáltatás rögzített tooa adott fürt, vagy csatolt tooan affinitáscsoport, és ezért rögzített tooa adott fürt úgy lett kialakítva. Így az új számítási erőforrás-kérelmek affinitás csoport azonos fürt hello meglévő erőforrásokat a rendszer hol tárolja hello kísérlet történt. Hello ugyanazon fürt azonban nem támogatási hello a kért Virtuálisgép-méretet, vagy nincs elegendő szabad terület, ami azt eredményezi, hogy foglalási hiba van. Ez érvényét veszti, hogy hello új erőforrások jönnek létre egy új felhőalapú szolgáltatás vagy egy meglévő felhőszolgáltatáshoz keresztül.

**1. megoldás:**

* Hozzon létre egy új felhőalapú szolgáltatás, és társíthatja egy régiót vagy a régió-alapú virtuális hálózat.
* Hozzon létre egy új virtuális gép hello új felhőalapú szolgáltatás.
  Új felhőalapú szolgáltatás toocreate megkísérlésekor hibaüzenetet kap, ha később próbálkozzon újra, vagy hello régió hello felhőalapú szolgáltatás módosítása.

> [!IMPORTANT]
> Ha egy új virtuális gép egy meglévő felhőalapú szolgáltatást a toocreate az elérni próbált, de nem sikerült, és toocreate volna az új virtuális gép egy új felhőalapú szolgáltatás, választhat tooconsolidate a virtuális gépek a hello ugyanaz a felhőalapú szolgáltatás. toodo Igen, törölje a hello hello meglévő felhőalapú szolgáltatás virtuális gépe, és ismét rögzíti őket az új felhőszolgáltatás hello merevlemezről. Azt azonban fontos, hogy hello új felhőalapú szolgáltatás lesz egy új nevet és a VIP, így tooupdate kell az összes hello függőségek, amelyek jelenleg használják ezeket az információkat hello meglévő felhőszolgáltatás tooremember.
> 
> 

**2. ok:** hello felhőszolgáltatás társítva van egy virtuális hálózathoz csatolt tooan affinitáscsoport, ezért ezt a rögzített tooa adott fürt úgy lett kialakítva. Minden új számítási erőforrás kérelmek affinitás csoport ezért hello azonos fürt hello meglévő erőforrásokat a rendszer hol tárolja a rendszer próbált. Hello ugyanazon fürt azonban nem támogatási hello a kért Virtuálisgép-méretet, vagy nincs elegendő szabad terület, ami azt eredményezi, hogy foglalási hiba van. Ez érvényét veszti, hogy hello új erőforrások jönnek létre egy új felhőalapú szolgáltatás vagy egy meglévő felhőszolgáltatáshoz keresztül.

**2. megoldás:**

* Hozzon létre egy új regionális virtuális hálózatot.
* Hozzon létre új virtuális Gépet az új virtuális hálózat hello hello.
* [A meglévő virtuális hálózat](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/) toohello új virtuális hálózat. További tudnivalók [regionális virtuális hálózatokba](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/). Alternatív megoldásként, [telepítse át a virtuális hálózati kapcsolat csoport-alapú tooa regionális virtuális hálózat](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/), majd hozzon létre új virtuális gép hello.

## <a name="next-steps"></a>Következő lépések
Ha problémát tapasztal, amikor elindítja a Linux virtuális gép leállított vagy méretezze át egy meglévő Linux virtuális Gépre az Azure-ban, tekintse meg [újraindításával és átméretezésével egy meglévő Linux virtuális gépet az Azure klasszikus üzembe helyezési problémáinak elhárítása](restart-resize-error-troubleshooting.md).

