---
title: "Nem felügyelt adatok lemez csatolása Windows VM - Azure |} Microsoft Docs"
description: "Nem felügyelt adatok új vagy meglévő lemez csatolása egy Windows virtuális Gépet az Azure portálon a Resource Manager üzembe helyezési modellel hogyan."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3790fc59-7264-41df-b7a3-8d1226799885
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: cynthn
ms.openlocfilehash: c0886302c82641f8cc1a00d3972870d58ba8afb4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-attach-an-unmanaged-data-disk-to-a-windows-vm-in-the-azure-portal"></a>Hogyan lehet egy nem felügyelt adatlemezt csatolni egy Windows virtuális Gépet az Azure-portálon

Ez a cikk bemutatja, hogyan csatlakoztassa az új és meglévő nem felügyelt lemezeket a Windows virtuális gépek az Azure portálon keresztül. Emellett [powershellel adatlemezzel](./attach-disk-ps.md). Mielőtt ezt megtehetné, tekintse át az alábbi tippek:

* A virtuális gép mérete csatolhat hány adatlemezek szabályozza. További információkért lásd: [virtuális gépek méretei](sizes.md).
* A prémium szintű storage, kell a DS-méretek és GS sorozatnak virtuális gép. Ezek a virtuális gépek lemezeit mind prémium és Standard storage-fiókok használatával. Prémium szintű storage bizonyos régiókban érhető el. További információkért lásd: [prémium szintű Storage: nagy teljesítményű tárolást Azure virtuális gépek terheléseihez](../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Egy új lemezt nem kell először hozza létre, mert az Azure létrehozza azt csatolása.


Emellett [powershellel adatlemezzel](attach-disk-ps.md).


## <a name="find-the-virtual-machine"></a>A virtuális gép található.
1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com/).
2. A bal oldali menüben kattintson a **virtuális gépek**.
3. Válassza ki a virtuális gépet a listából.
4. A virtuális gépek paneljén kattintson **lemezek**.
   
A következő utasítások vagy csatlakoztatása a következő lépésként a [új lemez](#option-1-attach-a-new-disk) vagy egy [meglévő lemez](#option-2-attach-an-existing-disk).

## <a name="option-1-attach-and-initialize-a-new-disk"></a>1. lehetőség: Csatolja, és az új lemez inicializálása
1. Az a **lemezek** panelen kattintson a **+ Hozzáadás adatlemez**.
2. Az a **Attach felügyelt lemezes** panelen adja meg nevet a lemeznek **neve** , és válassza **új (üres lemez)** a **adatforrástípust**.
3. A **tároló**, kattintson a **Tallózás** gombra, és keresse meg a tárfiók és tároló, hol szeretné tárolni, és kattintson az új virtuális Merevlemezt a **kiválasztása**. 
  
   ![Lemez beállítások áttekintése](./media/attach-disk-portal/attach-empty-unmanaged.png)
   
3. Amikor elkészült, a beállítások a adatlemez, kattintson a **OK**.
4. Vissza a **lemezek** panelen kattintson **mentése** a lemez hozzáadása a Virtuálisgép-konfigurációhoz.


### <a name="initialize-a-new-data-disk"></a>Az új adatok lemez inicializálása

1. Csatlakozzon a virtuális géphez. Útmutatásért lásd: [csatlakoztatása, és jelentkezzen be a Windowst futtató Azure virtuális gép](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
1. Kattintson a **Start** belül a virtuális gép és a típus menü **diskmgmt.msc** és találati **Enter**. Ekkor elindul, a Lemezkezelés beépülő modult.
2. A Lemezkezelés eszköz felismeri, hogy van-e egy új, nem inicializált lemezt, és a lemez inicializálása ablak jelenik meg.
3. Győződjön meg arról, hogy az új lemez legyen kijelölve, és kattintson a **OK** inicializálása azt.
4. Az új lemez most jelenik meg **le nem foglalt**. Kattintson a jobb gombbal a lemezre, és válasszon **új egyszerű kötet**. A **új egyszerű kötet varázslóban** kezdődik.
5. Lépkedjen végig a varázslón, továbbra is az alapértelmezett beállításokat, amikor elkészült, válassza ki **Befejezés**.
6. Zárja be a Lemezkezelés eszközben.
7. Kapott egy előugró ablak, amelyekre szüksége van az új lemez formázása előtt is használhatja. Kattintson a **lemez formázása**.
8. Az a **új lemez formázása** párbeszédpanelen ellenőrizze a beállításokat, és kattintson a **Start**.
9. Megjelenik egy figyelmeztetés, hogy a lemezek formázása törli az összes adat, kattintson a **OK**.
10. A formátum befejeztével kattintson **OK**.


## <a name="option-2-attach-an-existing-disk"></a>2. lehetőség: A meglévő lemez csatolása
1. Az a **lemezek** panelen kattintson a **+ Hozzáadás adatlemez**.
2. Az a **csatlakoztatása nem felügyelt lemez** panelen, a **adatforrástípust** kiválasztása **meglévő blob**.

    ![Lemez beállítások áttekintése](./media/attach-disk-portal/attach-existing-unmanaged.png)

    3. Kattintson a **Tallózás** lehetőségre, és navigáljon a tárfiók és tároló, ahol a meglévő virtuális merevlemez. Kattintson és a VHD-t, és kattintson **válasszon**.
4. Kattintson a **OK** a a **csatlakoztatása nem felügyelt lemezt** panelen.
5. Az a **lemezek** panelen kattintson a **mentése** a lemez hozzáadása a virtuális gép konfigurációját.
   


## <a name="use-trim-with-standard-storage"></a>Standard szintű tárolóval vágást használata

Standard szintű tárolást (HDD) használatakor vágást kell engedélyeznie. VÁGÁS elveti a nem használt blokkok a lemezen, így csak a ténylegesen használt tárolási kell fizetni. Ez is mentheti költségek Ha nagy fájlok létrehozása, majd törli őket. 

Ez a parancs a vágás beállítás ellenőrzéséhez futtathatja. Nyissa meg a Windows virtuális gép, és írja be a parancssorba:

```
fsutil behavior query DisableDeleteNotify
```

A parancs a 0 értéket adja vissza, vágást engedélyezve van-e megfelelően. Ha a visszaadott érték 1, vágást engedélyezéséhez a következő parancsot:
```
fsutil behavior set DisableDeleteNotify 0
```

Adatok törlése a lemezről, után gondoskodhat a vágás töredezettségmentesítési kiürítési megfelelően futtatásával vágás műveletek:

```
defrag.exe <volume:> -l
```

A teljes kötet van levágja a kötet formázását is biztosítható.


## <a name="next-steps"></a>Következő lépések
Ha Ön alkalmazás van szüksége a d meghajtó adatainak tárolásához, érdemes [a meghajtóbetűjel, a Windows ideiglenes lemez](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

