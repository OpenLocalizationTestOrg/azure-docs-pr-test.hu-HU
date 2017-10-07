---
title: "az Azure storage-fiókok, a tárolók és a VHD törlésekor aaaTroubleshoot hibák |} Microsoft Docs"
description: "Hibaelhárítás az Azure storage-fiókok, a tárolók és a VHD törlésekor"
services: storage
documentationcenter: 
author: genlin
manager: cshepard
editor: na
tags: storage
ms.assetid: 17403aa1-fe8d-45ec-bc33-2c0b61126286
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: genli
ms.openlocfilehash: 33261562a2dd2614b35bc1118924513f8c624d6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-errors-when-you-delete-azure-storage-accounts-containers-or-vhds"></a>Hibaelhárítás az Azure storage-fiókok, a tárolók és a VHD törlésekor

Előfordulhat, hogy hibák merülnek fel toodelete meg egy Azure storage-fiók, a tárolót, vagy a virtuális merevlemez (VHD) a hello [Azure-portálon](https://portal.azure.com). Ez a cikk nyújt hibaelhárítási útmutatót toohelp resolve hello probléma Azure Resource Manager-telepítés.

Ha ez a cikk nem oldja meg az Azure problémát, látogasson el az Azure-fórumok hello [MSDN és a Stack Overflow](https://azure.microsoft.com/support/forums/). A probléma a fórumokban beküldheti vagy too@AzureSupport a Twitteren. Emellett fájlt a az Azure támogatási kérelmet kiválasztásával **segítségre van szüksége** a hello [az Azure támogatási](https://azure.microsoft.com/support/options/) hely.

## <a name="symptoms"></a>Probléma
### <a name="scenario-1"></a>1. forgatókönyv
Amikor toodelete egy tárfiókot a Resource Manager üzembe a virtuális merevlemez, hello a következő hibaüzenet jelenhet meg:

**Nem sikerült toodelete blob "vhds/BlobName.vhd". Hiba: Nincs jelenleg a címbérlet hello blob, és nincs bérleti azonosító hello kérelemben lett megadva.**

Ez a probléma akkor fordulhat elő, mert a virtuális gép (VM) rendelkezik a címbérlet hello toodelete próbált VHD.

### <a name="scenario-2"></a>2. forgatókönyv
Amikor toodelete egy tárfiókot a Resource Manager üzembe egy tárolóját, hello a következő hibaüzenet jelenhet meg:

**Nem sikerült toodelete tárolási tároló "VHD". Hiba: Jelenleg a címbérlet hello tárolóra, és nincs bérleti azonosító hello kérelemben lett megadva.**

Ez a probléma akkor fordulhat elő, mert hello tároló hello bérleti állapotban zárolva van egy virtuális Merevlemezt.

### <a name="scenario-3"></a>3. forgatókönyv
Amikor toodelete egy tárfiókot a Resource Manager üzembe, hello a következő hibaüzenet jelenhet meg:

**Nem sikerült toodelete tárfiók "StorageAccountName". Hiba: hello storage-fiók nem törölhető a miatt tooits összetevői használatban vannak.**

Ez a probléma akkor fordulhat elő, mert hello tárfiók hello bérleti állapotú virtuális Merevlemezt tartalmaz.

## <a name="solution"></a>Megoldás 
tooresolve ezek a problémák, meg kell adnia hello hello hibát okozó VHD és hello kapcsolódó virtuális gép. Ezután válassza le a virtuális gép hello (az adatlemezek) a virtuális merevlemez hello, vagy hello (az operációsrendszer-lemezek) hello VHD-t használó virtuális gép törlése. Ez hello bérleti eltávolítja hello VHD-t, és lehetővé teszi, hogy törölte toobe. 

toodo a, használja a következő módszerek hello:

### <a name="method-1---use-azure-storage-explorer"></a>1 - metódus használata Azure Tártallózó

### <a name="step-1-identify-hello-vhd-that-prevent-deletion-of-hello-storage-account"></a>1. lépés azonosítása hello hello tárfiók törlésének megakadályozása VHD-t.

1. Hello tárfiók törlése, kap egy üzenet párbeszédpanelen például hello következő: 

    ![jelenik meg, amikor hello tárfiók törlése](././media/storage-resource-manager-cannot-delete-storage-account-container-vhd/delete-storage-error.png) 

2. Ellenőrizze a hello **lemez URL-cím** tooidentify hello tárfiók és a virtuális Merevlemezt, amely megakadályozza a hello hello a tárfiók törlése. A következő példa hello, hello előtt karakterlánc ". blob.core.windows.net" hello tárfiók neve, és "SCCM2012-2015-08-28.vhd" hello virtuális merevlemez nevét.  

        https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd

### <a name="step-2-delete-hello-vhd-by-using-azure-storage-explorer"></a>2. lépés törlése hello VHD Azure Tártallózó használatával

1. A letöltés és telepítés hello legújabb verziójának [Azure Tártallózó](http://storageexplorer.com/). Ez az eszköz egy különálló alkalmazás, amely lehetővé teszi tooeasily dolgozhat Azure Storage-adatokkal Windows, a macOS és a Linux Microsoft.
2. Nyissa meg az Azure Tártallózó, kiválasztása ![fiók ikon](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/account.png) a bal oldali sávon hello válassza ki az Azure környezetben, és jelentkezzen be.

3. Válassza ki az összes előfizetések vagy hello előfizetés, amely tartalmazza azt szeretné, hogy toodelete hello tárfiók.

    ![Előfizetés hozzáadása](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/addsub.png)

4. Nyissa meg a Microsoft hello lemez URL-cím korábbi, jelölje be hello lekért toohello tárfiók **Blobtárolók** > **VHD-k** , és keresse meg, amely megakadályozza a virtuális merevlemez hello hello tárfiók törlése.
5. Ha hello VHD található, ellenőrizze a hello **nevet a virtuális gép** oszlop toofind hello virtuális Gépet, amely a VHD-t használja.

    ![Ellenőrizze a virtuális gép](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/check-vm.png)

6. Távolítsa el hello bérleti hello VHD az Azure portál használatával. További információkért lásd: [eltávolítása hello bérletet hello VHD](#remove-the-lease-from-the-vhd). 

7. Nyissa meg toohello Azure Tártallózó, kattintson a jobb gombbal a virtuális merevlemez hello, és válassza a törlés.

8. Hello a tárfiók törlése.

### <a name="method-2---use-azure-portal"></a>2 - módszer használata Azure-portálon 

#### <a name="step-1-identify-hello-vhd-that-prevent-deletion-of-hello-storage-account"></a>1. lépés: Azonosítása hello hello tárfiók törlésének megakadályozása VHD-t.

1. Hello tárfiók törlése, kap egy üzenet párbeszédpanelen például hello következő: 

    ![jelenik meg, amikor hello tárfiók törlése](././media/storage-resource-manager-cannot-delete-storage-account-container-vhd/delete-storage-error.png) 

2. Ellenőrizze a hello **lemez URL-cím** tooidentify hello tárfiók és a virtuális Merevlemezt, amely megakadályozza a hello hello a tárfiók törlése. A következő példa hello, hello előtt karakterlánc ". blob.core.windows.net" hello tárfiók neve, és "SCCM2012-2015-08-28.vhd" hello virtuális merevlemez nevét.  

        https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd

2. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
3. Hello központ menüben válassza ki a **összes erőforrás**. Nyissa meg a tárfiók toohello, és válassza ki **Blobok** > **VHD-k**.

    ![Képernyőfelvétel a hello portálhoz, a hello tárfiók és hello "VHD" tároló kiemelve](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/opencontainer.png)

4. Keresse meg a virtuális Merevlemezt, amely a Microsoft hello lemez URL-címről korábban beszerzett hello. Ezt követően, hogy melyik virtuális gép által használt virtuális merevlemez hello. Általában megadhatja, hogy melyik virtuális gép rendelkezik hello VHD hello virtuális merevlemez neve ellenőrzésével:

Virtuális gép Resource Manager fejlesztési modellben

   * Az operációs rendszer lemezek általában kövesse ezt az elnevezési konvenciót: VMName-éééé-hh-nn-HHMMSS.vhd
   * Az adatlemezek általában kövesse ezt az elnevezési konvenciót: VMName-éééé-hh-nn-HHMMSS.vhd

A klasszikus fejlesztési modell VM

   * Az operációs rendszer lemezek általában kövesse ezt az elnevezési konvenciót: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd
   * Az adatlemezek általában kövesse ezt az elnevezési konvenciót: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd

#### <a name="step-2-remove-hello-lease-from-hello-vhd"></a>2. lépés: Virtuális merevlemez hello hello bérleti eltávolítása

[Hello bérleti eltávolítása hello VHD](#remove-the-lease-from-the-vhd), és törölje a hello tárfiók.

## <a name="what-is-a-lease"></a>Mi az a címbérlet?
A címbérlet egy zároláshoz használt toocontrol hozzáférés tooa blob (például egy VHD-k) lehet. Ha egy blobot egy bérelt, hello bérleti csak hello tulajdonosainak hello blob férhet hozzá. A címbérlet fontos hello a következő okok miatt:

* Megakadályozza, hogy a program sérült adatokat, ha több tulajdonosok próbálja toowrite toohello hello blob: hello azonos részében ugyanannyi időt vesz igénybe.
* Megakadályozza, hogy hello blob törlődnek, ha valami aktívan használja (például virtuális gép).
* Megakadályozza, hogy hello tárfiók törlődnek, ha valami aktívan használja (például virtuális gép).

### <a name="remove-hello-lease-from-hello-vhd"></a>Virtuális merevlemez hello hello bérleti eltávolítása
Ha hello VHD egy operációsrendszer-lemez, törölnie kell az hello VM tooremove hello bérleti:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. A hello **Hub** menü **virtuális gépek**.
3. Válassza ki, amely tárolja a címbérlet hello virtuális Merevlemezt a virtuális gép hello.
4. Győződjön meg arról, hogy semmi sem aktívan használ hello virtuális gépet, és, hogy Ön már nem kell hello virtuális gépet.
5. Hello hello tetején **VM részletek** panelen válassza **törlése**, és kattintson a **Igen** tooconfirm.
6. virtuális gép hello törölni kell, de hello VHD-t meg kell őrizni. Azonban hello virtuális merevlemez már nem kell a címbérlet rajta. Hello bérleti toobe kiadott néhány percig is eltarthat. amely bérleti hello tooverify felszabadul, nyissa meg túl**összes erőforrás** > **Tárfióknév** > **Blobok**  >  **VHD-k**. A hello **Blob-tulajdonságok** ablaktáblában hello **bérleti állapot** értéket kell **feloldva**.

Ha hello VHD adatlemezt, válassza le a hello VM tooremove hello bérleti a virtuális merevlemez hello:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. A hello **Hub** menü **virtuális gépek**.
3. Válassza ki, amely tárolja a címbérlet hello virtuális Merevlemezt a virtuális gép hello.
4. Válassza ki **lemezek** a hello **VM részletek** panelen.
5. Válassza ki, amely tárolja a címbérlet a hello virtuális merevlemez adatlemeze hello. Megadhatja, hogy melyik virtuális merevlemez a csatolt hello lemez hello VHD hello URL-címe ellenőrzésével.
6. Határozza meg, hogy semmi sem aktívan használ hello adatlemez biztosan.
7. Kattintson a **leválasztási** a hello **részletek lemez** panelen.
8. hello lemez most hello virtuális gép választható le, és hello virtuális merevlemez már nem kell a címbérlet rajta. Hello bérleti toobe kiadott néhány percig is eltarthat. amely bérleti hello tooverify kiadása, nyissa meg túl**összes erőforrás** > **Tárfióknév** > **Blobok**  >  **VHD-k**. A hello **Blob-tulajdonságok** ablaktáblában hello **bérleti állapot** értéket kell **feloldva**.

## <a name="next-steps"></a>Következő lépések
* [Tárfiók törlése](storage-create-storage-account.md#delete-a-storage-account)
* [Hogyan toobreak hello zárolva van címbérlete a blob storage (PowerShell) a Microsoft Azure](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)
