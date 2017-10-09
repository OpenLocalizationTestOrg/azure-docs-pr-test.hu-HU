---
title: "az Azure storage-fiókok, a tárolók és a VHD törlése a klasszikus telepítési aaaTroubleshoot |} Microsoft Docs"
description: "Az Azure storage-fiókok, a tárolók és a VHD törlése a klasszikus telepítési hibaelhárítása"
services: storage
documentationcenter: 
author: genlin
manager: felixwu
editor: tysonn
tags: storage
ms.assetid: 0f7a8243-d8dc-432a-9d37-1272a0cb3a5c
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: genli
ms.openlocfilehash: 6bbfa032e1968718c623227bb426d553e2951075
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deleting-azure-storage-accounts-containers-or-vhds-in-a-classic-deployment"></a>Az Azure storage-fiókok, a tárolók és a VHD törlése a klasszikus telepítési hibaelhárítása
[!INCLUDE [storage-selector-cannot-delete-storage-account-container-vhd](../../includes/storage-selector-cannot-delete-storage-account-container-vhd.md)]

Hibák akkor fordulhat elő, amikor toodelete hello Azure storage-fiók, a tároló vagy a virtuális Merevlemezt próbál hello [Azure-portálon](https://portal.azure.com/) vagy hello [a klasszikus Azure portálon](https://manage.windowsazure.com/). hello problémák hello a következő körülmények okozhatják:

* Ha töröl egy virtuális Gépet, hello lemez és a virtuális merevlemez nem törlődnek automatikusan. Lehet, hogy storage-fiók törlése a hiba okának hello. Hello lemez azt ne törölje, így hello lemez toomount másik virtuális gép is használhatja.
* Nincs még a címbérlet hello lemez tartozó lemez vagy hello blob.
* Van még egy Virtuálisgép-lemezkép, amely egy blob, -tároló vagy tárolási fiókot használja.

Ha az Azure nem problémával az ebben a cikkben, látogasson el az Azure-fórumok hello [MSDN és hello a Stack Overflow](https://azure.microsoft.com/support/forums/). A probléma a fórumokban beküldheti vagy too@AzureSupport a Twitteren. Emellett fájlt a az Azure támogatási kérelmet kiválasztásával **segítségre van szüksége** a hello [az Azure támogatási](https://azure.microsoft.com/support/options/) hely.

## <a name="symptoms"></a>Probléma
a következő szakasz hello közös azon hibákat tartalmazza, akkor fordulhat elő, amikor megpróbál toodelete hello Azure storage-fiókok, a tárolók és a VHD.

### <a name="scenario-1-unable-toodelete-a-storage-account"></a>1. eset: Nem toodelete a storage-fiók
Klasszikus tárfiókokat toohello hello kiválasztásakor [Azure-portálon](https://portal.azure.com/) válassza **törlése**, akadályozzák a hello tárfiók törlésének objektumok listáját típusától:

  ![Hiba történt a kép Ha a hello Storage-fiók törlése](./media/storage-cannot-delete-storage-account-container-vhd/newerror.png)

Toohello storage-fiókot hello kiválasztásakor [a klasszikus Azure portálon](https://manage.windowsazure.com/) válassza **törlése**, előfordulhat, hogy tekintse meg a következő hibák hello egyikét:

- *A tárfiók StorageAccountName VM lemezképet is tartalmaz. Győződjön meg arról, ez a tárfiók törlése előtt a Virtuálisgép-lemezképek eltávolításáról.*

- *Nem sikerült toodelete tárfiók < vm-storage-fiók-neve >. Nem lehet toodelete tárfiók < vm-storage-fiók-neve >: "< virtuális gép-storage-fiók-neve > tárfiók rendelkezik néhány tárfiókban aktív lemezképek és/vagy lemezek. Győződjön meg arról, a lemezkép és/vagy lemezek vannak eltávolítására, mielőtt törölné a tárfiókot. ".*

- *A tárfiók < vm-storage-fiók-neve > rendelkezik néhány tárfiókban aktív lemezképek és/vagy lemezek, pl. xxxxxxxxx-xxxxxxxxx-O-209490240936090599. Győződjön meg arról, ezek a lemezképek és/vagy lemez(ek) törlődnek, mielőtt törölné a tárfiókot.*

- *A tárfiók < vm-storage-fiók-neve > 1 tárolója, amely aktív lemezkép és/vagy a lemez összetevők rendelkezik. Győződjön meg arról, ez a tárfiók törlése előtt ezeket az összetevőket eltávolításuk hello lemezképtárból*.

- *Küldje el a sikertelen tárfiók < vm-storage-fiók-neve > 1 tárolója, amely aktív lemezkép és/vagy a lemez összetevők rendelkezik. Gondoskodjon arról, hogy ezek az összetevők eltávolításuk hello lemezképtárból, ez a tárfiók törlése előtt. Toodelete tárfiók kísérli meg, és a vele társított még mindig aktív lemezek vannak, megjelenik egy üzenet jelenik meg törölt toobe igénylő aktív lemezek*.

### <a name="scenario-2-unable-toodelete-a-container"></a>2. eset: Nem toodelete tárolója
Toodelete hello tároló meg, láthatja a következő hiba hello:

*Nem sikerült toodelete tároló <container name>. Hiba: "jelenleg a címbérlet hello tárolóra, és nincs bérleti azonosító hello kérelemben megadott*.

Vagy

*a következő virtuális gépek lemezeit hello ebben a tárolóban lévő blobok használ, így a hello tároló nem törölhető: VirtualMachineDiskName1, VirtualMachineDiskName2,...*

### <a name="scenario-3-unable-toodelete-a-vhd"></a>3. eset: Nem toodelete virtuális merevlemez
Miután törli a virtuális gép, és ezután próbálja toodelete hello blobok hello tartozó virtuális merevlemezek, akkor fordulhat elő, a következő üzenet hello:

*Nem sikerült toodelete blob "elérési út/XXXXXX-XXXXXX-os-1447379084699.vhd'. Hiba: "jelenleg a címbérlet hello blob és hello kérelemben nincs bérleti azonosító lett megadva.*

Vagy

*BLOB "BlobName.vhd" virtuálisgép-lemez "VirtualMachineDiskName" használatban van, ezért hello blob nem törölhető.*

## <a name="solution"></a>Megoldás
tooresolve hello leggyakoribb problémák, próbálja meg a következő metódus hello:

### <a name="step-1-delete-any-disks-that-are-preventing-deletion-of-hello-storage-account-container-or-vhd"></a>1. lépés: Akadályozzák a törlés hello tárfiókot, a tároló vagy a VHD lemezek törlése
1. Váltás toohello [a klasszikus Azure portálon](https://manage.windowsazure.com/).
2. Válassza ki **virtuális gép** > **lemezek**.

    ![A klasszikus Azure portálon található virtuális gépek lemezek képe.](./media/storage-cannot-delete-storage-account-container-vhd/VMUI.png)
3. Keresse meg a társított hello tárfiókot, tároló vagy VHD-t, amelyet az toodelete hello lemezek. Ha engedélyezi az hello lemez hello helyét, található hello kapcsolódó tárfiók, a tároló vagy a virtuális merevlemez.

    ![A klasszikus Azure portálon helyére vonatkozó információkat lemezek bemutató kép](./media/storage-cannot-delete-storage-account-container-vhd/DiskLocation.png)
4. Hello lemezek törlése hello a következő módszerek egyikével:

  - Ha nincs virtuális gép szerepel hello **csatlakoztatva** mező hello lemez, törölheti a hello lemez közvetlenül.

  - Ha hello lemez adatlemez, kövesse az alábbi lépéseket:

    1. Ellenőrizze a virtuális Gépet, amely hello lemez csatolva van hello hello nevét.
    2. Nyissa meg túl**virtuális gépek** > **példányok**, majd keresse meg a virtuális gép hello.
    3. Győződjön meg arról, hogy semmi sem aktívan hello lemezt használ.
    4. Válassza ki **leválasztani a lemezt** hello portál toodetach hello lemez hello alján.
    5. Nyissa meg túl**virtuális gépek** > **lemezek**, és várja meg, hello **csatlakoztatva** mező tooturn üres. Ez azt jelzi, hogy hello lemez sikeresen le hello virtuális gép.
    6. Válassza ki **törlése** hello aljához **virtuális gépek** > **lemezek** toodelete hello lemez.

  - Ha hello lemez operációsrendszer-lemez (hello **tartalmaz operációs rendszer** mező értéke Windows-) és a csatolt tooa VM, kövesse az alábbi lépéseket toodelete hello virtuális gép. hello operációsrendszer-lemez nem választható le, hogy toodelete hello VM toorelease hello bérleti.

    1. Hello Névellenőrzés hello virtuális gép hello adatlemez van csatolva.  
    2. Nyissa meg túl**virtuális gépek** > **példányok**, majd jelölje be hello virtuális Gépet, amely a lemez hello kapcsolódik.
    3. Győződjön meg arról, hogy semmi sem aktívan használ hello virtuális gépet, és, hogy Ön már nem kell hello virtuális gépet.
    4. Jelölje be hello VM hello lemez csatolva van, majd válassza ki **törlése** > **Delete hello csatlakoztatott lemezekkel**.
    5. Nyissa meg túl**virtuális gépek** > **lemezek**, és várja meg, hello lemez toodisappear.  A toooccur néhány percig is eltarthat, és esetleg toorefresh hello lap.
    6. Ha hello lemez nem tűnik el, várjon, amíg hello **csatlakoztatva** mező tooturn üres. Ez azt jelzi, hogy hello lemez teljesen le hello virtuális gép.  Ezután válasszon hello lemezt, és válassza ki **törlése** hello lap toodelete hello lemez hello alján.


   > [!NOTE]
   > Ha a lemez csatlakoztatott tooa VM, csak akkor tudja toodelete azt. Lemezek aszinkron módon leválasztott törölt virtuális gép alapján. Ez a mező tooclear fel a hello virtuális gép törlése után néhány percig is eltarthat.
   >
   >


### <a name="step-2-delete-any-vm-images-that-are-preventing-deletion-of-hello-storage-account-or-container"></a>2. lépés: Törölje az összes virtuális gép lemezképet, amely akadályozzák a hello tárfiók és tároló törlése
1. Váltás toohello [a klasszikus Azure portálon](https://manage.windowsazure.com/).
2. Válassza ki **virtuális gép** > **képek**, és törölje a társított hello tárfiókot, a tároló vagy a virtuális merevlemez képek hello.

    Ezt követően próbálja meg toodelete hello tárfiókot, a tároló vagy a virtuális merevlemez újra.

> [!WARNING]
> Bármi is meg arról, hogy tooback kívánt toosave hello fiók törlése előtt. Miután törli a virtuális merevlemez, blob, tábla, várólista vagy fájlt, az véglegesen törölve lesz. Győződjön meg arról, hogy hello erőforrás nincs használatban.
>
>

## <a name="about-hello-stopped-deallocated-status"></a>Hello állapot Leállítva (felszabadítva) kapcsolatban
Virtuális gépek hello klasszikus üzembe helyezési modellel létrehozott, és, amely megőrzi lesz hello **leállítva (felszabadítva)** vagy hello állapot [Azure-portálon](https://portal.azure.com/) vagy [a klasszikus Azure portálon ](https://manage.windowsazure.com/).

**A klasszikus Azure portálon**:

![Azure-portál virtuális gépek leállt (Deallocated) állapotát.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo2.png)

**Azure-portálon**:

![Leállított (felszabadított) állapota a virtuális gépek a klasszikus Azure portálon.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo1.png)

Egy "Leállítva (felszabadítva)" állapot hello számítógépes erőforrások, például hello Processzor, memória és a hálózati kiadását. hello lemezek, azonban továbbra is megmaradnak, hogy a gyors újbóli létrehozásához hello VM szükség esetén. Ezek a lemezek VHD-k, az Azure storage által támogatott funkciók mellett kártevőészlelést jönnek létre. hello tárfiók vannak a virtuális merevlemezek, és hello lemezek bérleteket kell azokat a virtuális merevlemezeken.

## <a name="next-steps"></a>Következő lépések
* [Tárfiók törlése](storage-create-storage-account.md#delete-a-storage-account)
