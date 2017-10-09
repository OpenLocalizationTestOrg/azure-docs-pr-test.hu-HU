---
title: "egy felügyelt adatok lemez tooa Windows Virtuálisgép - Azure aaaAttach |} Microsoft Docs"
description: "Hogyan tooattach új adatok lemez tooa felügyelt windowsos virtuális gép az Azure portál használatával hello hello Resource Manager üzembe helyezési modellben."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: cynthn
ms.openlocfilehash: bacc0589ad2d93e4d3d055c8f837f8db27291ead
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooattach-a-managed-data-disk-tooa-windows-vm-in-hello-azure-portal"></a>Hogyan tooattach felügyelt adatok lemezre tooa Windows virtuális gép a hello Azure-portálon

Ez a cikk bemutatja, hogyan tooattach egy új kezelt adatok lemezre tooWindows virtuális gépek hello Azure-portálon keresztül. Mielőtt ezt megtehetné, tekintse át az alábbi tippek:

* hello mérete hello virtuális gép csatolhat hány adatlemezek határozza meg. További információkért lásd: [virtuális gépek méretei](sizes.md).
* Egy új lemezt, akkor nincs szükség toocreate az első Azure létrehoz csatolása, mert.

Emellett [powershellel adatlemezzel](attach-disk-ps.md).



## <a name="add-a-data-disk"></a>Adatlemez hozzáadása
1. Hello hello bal oldali menüben kattintson a **virtuális gépek**.
2. Válassza ki a virtuális gép hello hello listából.
3. Hello virtuális gép paneljén kattintson **lemezek**.
   4. A hello **lemezek** panelen kattintson a **+ Hozzáadás adatlemez**.
5. Hello legördülő hello új lemezhez, válassza ki **hozzon létre üres**.
6. A hello **létrehozás felügyelt lemezes** panelen adjon meg egy nevet a hello lemez, és állítsa be, szükség esetén más beállítások hello. Amikor elkészült, kattintson a **létrehozása**.
7. A hello **lemezek** panelen toosave hello új lemezkonfiguráció hello VM a Mentés gombra.
6. Miután Azure hello lemezt hoz létre, és csatolja toohello virtuális gépet, hello új lemez szerepel-e hello virtuális gép lemezbeállításokat alatt **adatlemezek**.


## <a name="initialize-a-new-data-disk"></a>Az új adatok lemez inicializálása

1. Csatlakoztassa a virtuális gép toohello.
1. Kattintson a hello start menü belül hello VM és típus **diskmgmt.msc** és találati **Enter**. Ekkor elindul hello a Lemezkezelés beépülő modult.
2. A Lemezkezelés eszköz felismeri, hogy van-e egy új, nem inicializált lemezt, és hello lemez inicializálása ablak jelenik meg.
3. Ellenőrizze, hogy hello új lemez legyen kijelölve, majd kattintson a **OK** tooinitialize azt.
4. Új lemez hello lesz **le nem foglalt**. Kattintson a jobb gombbal a hello lemezen bárhol, és válassza ki **új egyszerű kötet**. Hello **új egyszerű kötet varázslóban** indul el.
5. Lépkedjen végig hello varázsló, továbbra is hello alapértelmezett beállításokat, amikor elkészült, válassza ki **Befejezés**.
6. Zárja be a Lemezkezelés eszközben.
7. Elérhetővé válik egy előugró ablak, amely azt használatba vétele előtt szükség van tooformat hello új lemezre. Kattintson a **lemez formázása**.
8. A hello **új lemez formázása** párbeszédpanel, hello beállításokat, majd kattintson **Start**.
9. Hogy formázás hello lemezek töröl hello adatok, egy figyelmeztetés fog megjelenni kattintson **OK**.
10. Hello formátum befejeztével kattintson **OK**.

## <a name="use-trim-with-standard-storage"></a>Standard szintű tárolóval vágást használata

Standard szintű tárolást (HDD) használatakor vágást kell engedélyeznie. VÁGÁS elveti a nem használt blokkok hello lemezen, így csak a ténylegesen használt tárolási kell fizetni. Ez is mentheti költségek Ha nagy fájlok létrehozása, majd törli őket. 

A parancs toocheck hello vágás beállítás is futtathatja. Nyissa meg a Windows virtuális gép, és írja be a parancssorba:

```
fsutil behavior query DisableDeleteNotify
```

Hello parancs 0 értéket adja vissza, vágást engedélyezve van-e megfelelően. 1 adja vissza, ha futtassa a következő parancs tooenable vágást hello:
```
fsutil behavior set DisableDeleteNotify 0
```

Adatok törlése a lemezről, után gondoskodhat a vágás töredezettségmentesítési hello vágás műveletek kiürítési megfelelően futtatásával:

```
defrag.exe <volume:> -l
```

Hello teljes kötet van rövidített hello kötet formázását is biztosítható.

## <a name="next-steps"></a>Következő lépések
Ha Ön alkalmazás toouse hello D: meghajtó toostore adatokra van szüksége, akkor [hello meghajtóbetűjel hello Windows ideiglenes lemez](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
