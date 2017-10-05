---
title: "Az Azure storage-fiókok, a tárolók és a VHD törlésekor hibák elhárítása |} Microsoft Docs"
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
ms.openlocfilehash: 11944dd38b1cc30106c0b76a108480c018ca39d4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshoot-errors-when-you-delete-azure-storage-accounts-containers-or-vhds"></a>Hibaelhárítás az Azure storage-fiókok, a tárolók és a VHD törlésekor

Előfordulhat, hogy hibák merülnek fel egy Azure storage-fiók, a tárolót, vagy a virtuális merevlemez (VHD) törlése közben a a [Azure-portálon](https://portal.azure.com). Ez a cikk nyújt hibaelhárítási útmutatót Azure Resource Manager-telepítés a probléma megoldása érdekében.

Ha ez a cikk nem oldja meg az Azure problémát, látogasson el az Azure-fórumok a [MSDN és a Stack Overflow](https://azure.microsoft.com/support/forums/). A probléma, beküldheti, ezek fórumokban, vagy @AzureSupport a Twitteren. Emellett fájlt a az Azure támogatási kérelmet kiválasztásával **segítségre van szüksége** a a [az Azure támogatási](https://azure.microsoft.com/support/options/) hely.

## <a name="symptoms"></a>Probléma
### <a name="scenario-1"></a>1. forgatókönyv
Egy tárfiókot a Resource Manager üzembe a virtuális merevlemez törlését kísérli meg, amikor a következő hibaüzenet jelenhet meg:

**Nem sikerült törölni a blobot "vhds/BlobName.vhd". Hiba: Jelenleg a címbérlet blobot, és a kérelemben nincs bérleti azonosító lett megadva.**

Ez a probléma akkor fordulhat elő, mert a virtuális gép (VM) a címbérlet a törölni kívánt virtuális.

### <a name="scenario-2"></a>2. forgatókönyv
Amikor megpróbálja törölni egy tárfiókot a Resource Manager üzembe egy tárolóját, a következő hibaüzenet jelenhet meg:

**Nem sikerült törölni a tárolási tároló "VHD". Hiba: Jelenleg a címbérlet a tárolóra, és a kérelemben nincs bérleti azonosító lett megadva.**

Ez a probléma akkor fordulhat elő, mert a tároló virtuális Merevlemezt, amely a címbérlet állapotban zárolva van.

### <a name="scenario-3"></a>3. forgatókönyv
A Resource Manager üzembe tárfiók törlése megkísérlésekor a következő hibaüzenet jelenhet meg:

**Nem sikerült törölni a tárfiókot "StorageAccountName". Hiba: A tárfiók nem törölhető a mert az összetevői használatban vannak.**

Ez a probléma akkor fordulhat elő, mert a tárfiók tartalmaz virtuális Merevlemezt, amely a címbérlet állapotban van.

## <a name="solution"></a>Megoldás 
Ezek a problémák megoldása érdekében meg kell adnia a VHD-t, a hibát okozó és a társított virtuális Gépet. Ezután válassza le a virtuális Merevlemezt a virtuális gép (az adatlemezek), vagy törölje a virtuális Gépet, amely a VHD-t használja (az operációsrendszer-lemezek). Ez a címbérlet eltávolítja a VHD-t, és lehetővé teszi, hogy törölhető. 

Ehhez használja a következő módszerek egyikét:

### <a name="method-1---use-azure-storage-explorer"></a>1 - metódus használata Azure Tártallózó

### <a name="step-1-identify-the-vhd-that-prevent-deletion-of-the-storage-account"></a>1. lépés azonosítása a virtuális Merevlemezt, amely a tárfiók törlésének megakadályozása

1. Ha törli a tárfiókot, kapni fog egy üzenet párbeszédpanelen a következő: 

    ![jelenik meg, amikor a tárfiók törlése](././media/storage-resource-manager-cannot-delete-storage-account-container-vhd/delete-storage-error.png) 

2. Ellenőrizze a **lemez URL-cím** azonosítására a tárolási fiók és a virtuális Merevlemezt, amely megakadályozza, hogy a tárfiók törlése. A következő példában a karakterlánc előtt ". blob.core.windows.net" a tárfiók nevét, és "SCCM2012-2015-08-28.vhd" a virtuális merevlemez nevét.  

        https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd

### <a name="step-2-delete-the-vhd-by-using-azure-storage-explorer"></a>2. lépés törlése a virtuális merevlemez Azure Tártallózó használatával

1. Töltse le és telepítse a legújabb [Azure Tártallózó](http://storageexplorer.com/). Ez az eszköz egy különálló alkalmazás, amely lehetővé teszi, hogy egyszerűen dolgozhat Azure Storage-adatokkal Windows, a macOS és a Linux Microsoft.
2. Nyissa meg az Azure Tártallózó, kiválasztása ![fiók ikon](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/account.png) a bal oldali sávon válassza ki az Azure környezetben, és jelentkezzen be.

3. Válassza ki az összes előfizetést, vagy törölni szeretné az előfizetést, amely tartalmazza a tárfiók.

    ![Előfizetés hozzáadása](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/addsub.png)

4. Lépjen a tárfiókhoz, hogy azt a lemez URL-cím, korábbi, jelölje be a **Blobtárolók** > **VHD-k** , és keresse meg a virtuális Merevlemezt, amely megakadályozza, hogy a tárfiók törlése.
5. Ha a virtuális Merevlemezen található, ellenőrizze a **nevet a virtuális gép** oszlopban keresse meg a virtuális Gépet, amely a VHD-t használja.

    ![Ellenőrizze a virtuális gép](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/check-vm.png)

6. Távolítsa el a címbérlet a VHD-t az Azure portál használatával. További információkért lásd: [távolítsa el a virtuális Merevlemezt a címbérlet](#remove-the-lease-from-the-vhd). 

7. Ugrás az Azure Tártallózó, kattintson a jobb gombbal a virtuális Merevlemezt, és válassza a törlés.

8. Tárfiók törlése.

### <a name="method-2---use-azure-portal"></a>2 - módszer használata Azure-portálon 

#### <a name="step-1-identify-the-vhd-that-prevent-deletion-of-the-storage-account"></a>1. lépés: A virtuális Merevlemezt, amely a tárfiók törlésének megakadályozása azonosítása

1. Ha törli a tárfiókot, kapni fog egy üzenet párbeszédpanelen a következő: 

    ![jelenik meg, amikor a tárfiók törlése](././media/storage-resource-manager-cannot-delete-storage-account-container-vhd/delete-storage-error.png) 

2. Ellenőrizze a **lemez URL-cím** azonosítására a tárolási fiók és a virtuális Merevlemezt, amely megakadályozza, hogy a tárfiók törlése. A következő példában a karakterlánc előtt ". blob.core.windows.net" a tárfiók nevét, és "SCCM2012-2015-08-28.vhd" a virtuális merevlemez nevét.  

        https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd

2. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
3. A központ menüben válassza ki a **összes erőforrás**. Nyissa meg a tárfiók, és válassza ki **Blobok** > **VHD-k**.

    ![Képernyőkép a tárfiók és a kiemelt "VHD" tárolóban, a portál](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/opencontainer.png)

4. Keresse meg a virtuális Merevlemezt, amely azt a lemez URL-címről korábban beszerzett. Ezt követően, hogy melyik virtuális gép használ a VHD-t. Általában megadhatja, hogy melyik virtuális gép, amely tárolja a virtuális merevlemez a VHD-fájl nevének ellenőrzésével:

Virtuális gép Resource Manager fejlesztési modellben

   * Az operációs rendszer lemezek általában kövesse ezt az elnevezési konvenciót: VMName-éééé-hh-nn-HHMMSS.vhd
   * Az adatlemezek általában kövesse ezt az elnevezési konvenciót: VMName-éééé-hh-nn-HHMMSS.vhd

A klasszikus fejlesztési modell VM

   * Az operációs rendszer lemezek általában kövesse ezt az elnevezési konvenciót: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd
   * Az adatlemezek általában kövesse ezt az elnevezési konvenciót: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd

#### <a name="step-2-remove-the-lease-from-the-vhd"></a>2. lépés: A címbérlet távolítsa el a virtuális merevlemez

[Távolítsa el a virtuális Merevlemezt a címbérlet](#remove-the-lease-from-the-vhd), majd törli a tárfiókot.

## <a name="what-is-a-lease"></a>Mi az a címbérlet?
A címbérlet egy zároláshoz használható (például egy VHD-k) blob való hozzáférés vezérlése érdekében. Ha egy blobot egy bérelt, csak a bérlet tulajdonosai férhet hozzá a blob. A címbérlet fontos a következők lehetnek az okai:

* Ha több tulajdonosok próbálja írni egy időben, a blob azonos részéhez megakadályozza, hogy sérült adatokat.
* Megakadályozza, hogy a blob törlődnek, ha valami aktívan használja (például virtuális gép).
* Megakadályozza, hogy a tárfiók törlődnek, ha valami aktívan használja (például virtuális gép).

### <a name="remove-the-lease-from-the-vhd"></a>A bérlet eltávolítása a virtuális merevlemez
Ha a virtuális Merevlemezt egy operációsrendszer-lemez, törölnie kell a virtuális gép eltávolítása a címbérlet:

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
2. Az a **Hub** menü **virtuális gépek**.
3. Válassza ki a virtuális Gépet, amely tárolja a címbérlet a virtuális merevlemezen.
4. Győződjön meg arról, hogy semmi sem aktívan használ a virtuális gép és a virtuális gép már nem szükséges.
5. Felső részén a **VM részletek** panelen válassza **törlése**, és kattintson a **Igen** megerősítéséhez.
6. A virtuális Gépet törölni kell, de a VHD-t meg kell őrizni. Azonban a virtuális merevlemez már nem kell a címbérlet rajta. A címbérlet mikorra várható néhány percig is eltarthat. Győződjön meg arról, hogy megjelent-e a címbérlet, keresse fel **összes erőforrás** > **Tárfióknév** > **Blobok**  >   **virtuális merevlemezek**. Az a **Blob-tulajdonságok** ablaktáblán, a **bérleti állapot** értéket kell **feloldva**.

Ha a virtuális merevlemez adatlemezt, válassza le a virtuális Merevlemezt a virtuális gép eltávolítása a címbérlet:

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
2. Az a **Hub** menü **virtuális gépek**.
3. Válassza ki a virtuális Gépet, amely tárolja a címbérlet a virtuális merevlemezen.
4. Válassza ki **lemezek** a a **VM részletek** panelen.
5. Válassza ki az adatlemezt, amely tárolja a címbérlet a virtuális merevlemezen. Segítségével meghatározhatja a virtuális merevlemez csatolva a lemezt a virtuális merevlemez URL-CÍMÉT ellenőrzésével.
6. Határozza meg, hogy semmi sem aktívan használja az adatok biztosan.
7. Kattintson a **leválasztási** a a **részletek lemez** panelen.
8. A lemez most a virtuális gép választható le, és a virtuális merevlemez már nem kell rendelkeznie a címbérlet rajta. A címbérlet mikorra várható néhány percig is eltarthat. Győződjön meg arról, hogy a címbérlet kiadása, keresse fel **összes erőforrás** > **Tárfióknév** > **Blobok**  >  **VHD-k**. Az a **Blob-tulajdonságok** ablaktáblán, a **bérleti állapot** értéket kell **feloldva**.

## <a name="next-steps"></a>Következő lépések
* [Tárfiók törlése](storage-create-storage-account.md#delete-a-storage-account)
* [Hogyan bontsa a zárolt címbérlete a blob storage (PowerShell) a Microsoft Azure-ban](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)
