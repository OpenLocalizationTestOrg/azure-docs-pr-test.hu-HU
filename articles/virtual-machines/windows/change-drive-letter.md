---
title: "Ellenőrizze, hogy a virtuális gépek adatlemezt D: meghajtó hello |} Microsoft Docs"
description: "Ismerteti, hogyan toochange meghajtó-betűjelek, egy Windows virtuális gép számára, hogy egy adatmeghajtó hello D: meghajtó is használhatja."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 0867a931-0055-4e31-8403-9b38a3eeb904
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: cynthn
ms.openlocfilehash: 43939da1a890ac4049266487951e3889aa0ed9d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-d-drive-as-a-data-drive-on-a-windows-vm"></a>Egy adatmeghajtó a virtuális gép Windows hello D: meghajtó használata
Ha az alkalmazásnak toouse hello D meghajtó toostore adatokat, kövesse ezeket utasításokat toouse meghajtó-betűjelű más hello ideiglenes lemez. Soha ne használja, hogy szükséges-e tookeep hello mennyiségű ideiglenes lemezes toostore adatokat.

Átméretezésekor vagy **Stop (Deallocate)** virtuális gép, ez indíthatnak hello virtuális gép tooa új hipervizor elhelyezését. A tervezett vagy nem tervezett karbantartási események is kiválthatják az elhelyezést. Ebben a forgatókönyvben a hello mennyiségű ideiglenes lemezes lesz máshoz hozzárendelt toohello első elérhető meghajtóbetűjelet. Ha olyan alkalmazás, amely kifejezetten a hello D: meghajtó szükséges, toofollow szüksége ezen lépéseket tootemporarily áthelyezés hello pagefile.sys, új adatlemezt csatolni, és rendelje hozzá D hello betűt, és a move hello pagefile.sys toohello ideiglenes meghajtó biztonsági. Művelet befejeződése után Azure nem veszi vissza hello D: Ha hello VM áthelyezése tooa különböző hipervizor.

További információ a hogyan Azure hello ideiglenes lemezt használ: [hello ideiglenes meghajtó Microsoft Azure virtuális gépeken ismertetése](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="attach-hello-data-disk"></a>Hello adatlemez csatolása
Első lépésként tooattach hello adatok lemez toohello virtuális gép lesz szüksége. toodo a hello portál használatával, lásd: [hogyan tooattach felügyelt adatok hello Azure-portálon a lemez](attach-managed-disk-portal.md).

## <a name="temporarily-move-pagefilesys-tooc-drive"></a>Ideiglenesen a pagefile.sys tooC meghajtó áthelyezése
1. Csatlakoztassa a toohello virtuális gépet. 
2. Kattintson a jobb gombbal hello **Start** menüre, majd válassza **rendszer**.
3. Hello bal oldali menüben válasszon ki **Speciális rendszerbeállítások**.
4. A hello **teljesítmény** szakaszban jelölje be **beállítások**.
5. Jelölje be hello **speciális** fülre.
6. A hello **virtuális memória** szakaszban jelölje be **módosítása**.
7. Jelölje be hello **C** meghajtó, és kattintson a **rendszer kezeli a méretet** , majd **beállítása**.
8. Jelölje be hello **D** meghajtó, és kattintson a **nincs lapozófájl** , majd **beállítása**.
9. Az Alkalmaz gombra. Egy hello számítógépen kell toobe hello módosítások tootake ronthatja a újraindul figyelmeztetés fog megjelenni.
10. Hello virtuális gép újraindításához.

## <a name="change-hello-drive-letters"></a>Hello meghajtóbetűjelek módosítása
1. Egyszer hello a virtuális gép újraindul, jelentkezzen be a virtuális gép toohello.
2. Kattintson a hello **Start** menüt, és írja be **diskmgmt.msc** és le az ENTER billentyűt. A Lemezkezelés eszköz indul el.
3. Kattintson a jobb gombbal a **D**, hello ideiglenes tárolási meghajtó, és válassza ki **meghajtóbetűjel és elérési utak**.
4. A meghajtóbetűjelet, válasszon egy új meghajtót például **T** majd **OK**. 
5. Kattintson a jobb gombbal a hello adatok lemezen, és válassza ki **módosítása Meghajtóbetűjel és elérési út**.
6. A meghajtóbetűjelet, jelölje ki a meghajtót **D** majd **OK**. 

## <a name="move-pagefilesys-back-toohello-temporary-storage-drive"></a>Pagefile.sys hátsó toohello ideiglenes tárolási meghajtó áthelyezése
1. Kattintson a jobb gombbal hello **Start** menüre, majd válassza **rendszer**
2. Hello bal oldali menüben válasszon ki **Speciális rendszerbeállítások**.
3. A hello **teljesítmény** szakaszban jelölje be **beállítások**.
4. Jelölje be hello **speciális** fülre.
5. A hello **virtuális memória** szakaszban jelölje be **módosítása**.
6. Jelölje ki az operációs rendszer hello meghajtót **C** kattintson **nincs lapozófájl** majd **beállítása**.
7. Válassza ki a hello ideiglenes tárolási meghajtó **T** , majd **rendszer kezeli a méretet** , majd **beállítása**.
8. Kattintson az **Alkalmaz** gombra. Egy hello számítógépen kell toobe hello módosítások tootake ronthatja a újraindul figyelmeztetés fog megjelenni.
9. Hello virtuális gép újraindításához.

## <a name="next-steps"></a>Következő lépések
* Hello tárolási elérhető tooyour virtuális gép által növelhető [adatlemezt csatol további](attach-managed-disk-portal.md).

