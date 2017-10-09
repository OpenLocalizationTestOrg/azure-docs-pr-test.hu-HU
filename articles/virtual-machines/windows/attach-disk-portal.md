---
title: "egy nem felügyelt adatok lemez tooa Windows Virtuálisgép - Azure aaaAttach |} Microsoft Docs"
description: "Hogyan tooattach nem felügyelt adatok új vagy meglévő lemez tooa Windows virtuális gép az Azure portál használatával hello hello Resource Manager üzembe helyezési modellben."
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
ms.openlocfilehash: 923a6663974143bf359970f9b0eb0d36cabcba9c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooattach-an-unmanaged-data-disk-tooa-windows-vm-in-hello-azure-portal"></a>Hogyan tooattach egy nem felügyelt adatok lemezre tooa Windows virtuális gép a hello Azure-portálon

Ez a cikk bemutatja, hogyan nem felügyelt tooattach meglévő és új lemezek tooWindows virtuális gépek hello Azure-portálon keresztül. Emellett [powershellel adatlemezzel](./attach-disk-ps.md). Mielőtt ezt megtehetné, tekintse át az alábbi tippek:

* hello mérete hello virtuális gép csatolhat hány adatlemezek határozza meg. További információkért lásd: [virtuális gépek méretei](sizes.md).
* toouse prémium szintű storage, kell a DS-méretek és GS sorozatnak virtuális gép. Ezek a virtuális gépek lemezeit mind prémium és Standard storage-fiókok használatával. Prémium szintű storage bizonyos régiókban érhető el. További információkért lásd: [prémium szintű Storage: nagy teljesítményű tárolást Azure virtuális gépek terheléseihez](../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Egy új lemezt, akkor nincs szükség toocreate az első Azure létrehoz csatolása, mert.


Emellett [powershellel adatlemezzel](attach-disk-ps.md).


## <a name="find-hello-virtual-machine"></a>Található hello virtuális gép
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Hello hello bal oldali menüben kattintson a **virtuális gépek**.
3. Válassza ki a virtuális gép hello hello listából.
4. Hello virtuális gépek paneljén kattintson **lemezek**.
   
A következő utasítások vagy csatlakoztatása a következő lépésként a [új lemez](#option-1-attach-a-new-disk) vagy egy [meglévő lemez](#option-2-attach-an-existing-disk).

## <a name="option-1-attach-and-initialize-a-new-disk"></a>1. lehetőség: Csatolja, és az új lemez inicializálása
1. A hello **lemezek** panelen kattintson a **+ Hozzáadás adatlemez**.
2. A hello **Attach felügyelt lemezes** panelen írja be a hello lemez nevét **neve** , és válassza **új (üres lemez)** a **adatforrástípust**.
3. A **tároló**, hello kattintson **Tallózás** toohello tárfiók és tároló, hol kívánja hello tárolt új virtuális merevlemez toobe, majd kattintson a Tallózás gombra, és **válassza ki a**. 
  
   ![Lemez beállítások áttekintése](./media/attach-disk-portal/attach-empty-unmanaged.png)
   
3. Amikor elkészült, hello beállítások hello adatlemez, kattintson a **OK**.
4. Vissza a hello **lemezek** panelen kattintson a **mentése** tooadd hello lemez toohello Virtuálisgép-konfiguráció.


### <a name="initialize-a-new-data-disk"></a>Az új adatok lemez inicializálása

1. Csatlakoztassa a toohello virtuális gépet. Útmutatásért lásd: [hogyan tooconnect és a bejelentkezés tooan Azure virtuális gépen futó Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
1. Hello kattintson **Start** belül hello VM és típus menü **diskmgmt.msc** és találati **Enter**. Ez elindítja a hello a Lemezkezelés beépülő modult.
2. A Lemezkezelés eszköz felismeri, hogy van-e egy új, nem inicializált lemezt, és hello lemez inicializálása ablak jelenik meg.
3. Ellenőrizze, hogy hello új lemez legyen kijelölve, majd kattintson a **OK** tooinitialize azt.
4. hello új lemez most jelenik meg **le nem foglalt**. Kattintson a jobb gombbal a hello lemezen bárhol, és válassza ki **új egyszerű kötet**. Hello **új egyszerű kötet varázslóban** kezdődik.
5. Lépkedjen végig hello varázsló, továbbra is hello alapértelmezett beállításokat, amikor elkészült, válassza ki **Befejezés**.
6. Zárja be a Lemezkezelés eszközben.
7. Kapott egy előugró ablak, amely azt használatba vétele előtt szükség van tooformat hello új lemezre. Kattintson a **lemez formázása**.
8. A hello **új lemez formázása** párbeszédpanel, hello beállításokat, majd kattintson **Start**.
9. Megjelenik egy figyelmeztetés, hogy hello lemezek formázása törlő hello adatok, kattintson a **OK**.
10. Hello formátum befejeztével kattintson **OK**.


## <a name="option-2-attach-an-existing-disk"></a>2. lehetőség: A meglévő lemez csatolása
1. A hello **lemezek** panelen kattintson a **+ Hozzáadás adatlemez**.
2. A hello **csatlakoztatása nem felügyelt lemez** panelen, a **adatforrástípust** válasszon **meglévő blob**.

    ![Lemez beállítások áttekintése](./media/attach-disk-portal/attach-existing-unmanaged.png)

    3. Kattintson a **Tallózás** toonavigate toohello tárfiók és tároló, ahol hello meglévő VHD. Kattintson és hello virtuális Merevlemezt, majd **válasszon**.
4. Kattintson a **OK** a hello **csatlakoztatása nem felügyelt lemezt** panelen.
5. A hello **lemezek** panelen kattintson a **mentése** tooadd hello toohello lemezkonfigurációja hello virtuális gép.
   


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

