---
title: "a lemez tooa aaaAttach klasszikus Azure virtuális gép |} Microsoft Docs"
description: "Adatok lemez tooa Windows létrehozott virtuális gépek hello klasszikus üzembe helyezési modellel csatolja, és inicializálja."
services: virtual-machines-windows, storage
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: be4e3e74-05bc-4527-969f-84f10a1d66a7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/21/2017
ms.author: cynthn
ms.openlocfilehash: bfe1b0fa066277d28d3862a18da4b1023cb4452d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="attach-a-data-disk-tooa-windows-virtual-machine-created-with-hello-classic-deployment-model"></a>Adatok lemez tooa Windows létrehozott virtuális gépek hello klasszikus üzembe helyezési modellel csatolása
<!--
Refernce article:
    If you want toouse hello new portal, see [How tooattach a data disk tooa Windows VM in hello Azure portal](../../virtual-machines-windows-attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
-->

Ez a cikk bemutatja, hogyan tooattach meglévő és új lemezek hozza létre a hello klasszikus telepítési modell tooa Windows rendszerű virtuális gép hello használata az Azure-portálon.

Emellett [csatolni egy adatok lemez tooa Linux virtuális gép hello Azure-portálon](../../linux/attach-disk-portal.md).

Mielőtt lemezt csatlakoztatni, tekintse át a következő tippek:

* hello mérete hello virtuális gép csatolhat hány adatlemezek határozza meg. További információkért lásd: [virtuális gépek méretei](../../virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

* toouse prémium szintű storage, kell a DS-méretek és GS sorozatnak virtuális gép. Ezek a virtuális gépek lemezeit mind prémium és Standard storage-fiókok használatával. Prémium szintű storage bizonyos régiókban érhető el. További információkért lásd: [prémium szintű Storage: nagy teljesítményű tárolást Azure virtuális gépek terheléseihez](../../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

* Egy új lemezt, akkor nincs szükség toocreate az első Azure létrehoz csatolása, mert.

Emellett [powershellel adatlemezzel](../../virtual-machines-windows-attach-disk-ps.md).

> [!IMPORTANT]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).

## <a name="find-hello-virtual-machine"></a>Található hello virtuális gép
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. A virtuális gép válassza hello hello irányítópulton szereplő hello erőforrásra.
3. Hello bal oldali ablaktábla **beállítások**, kattintson a **lemezek**.

    ![Nyissa meg a lemez beállításai](./media/attach-disk/virtualmachinedisks.png)

A következő utasítások vagy csatlakoztatása a következő lépésként a [új lemez](#option-1-attach-a-new-disk) vagy egy [meglévő lemez](#option-2-attach-an-existing-disk).

## <a name="option-1-attach-and-initialize-a-new-disk"></a>1. lehetőség: Csatolja, és az új lemez inicializálása

1. A hello **lemezek** panelen kattintson a **új Attach**.
2. Tekintse át a hello alapértelmezett beállításokat, szükség esetén frissítse, majd **OK**.

   ![Lemez beállítások áttekintése](./media/attach-disk/attach-new.png)

3. Miután Azure hello lemezt hoz létre, és csatolja toohello virtuális gépet, hello új lemez szerepel-e hello virtuális gép lemezbeállításokat alatt **adatlemezek**.

### <a name="initialize-a-new-data-disk"></a>Az új adatok lemez inicializálása

1. Csatlakoztassa a toohello virtuális gépet. Útmutatásért lásd: [hogyan tooconnect és a bejelentkezés tooan Azure virtuális gépen futó Windows](../../virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
2. Virtuális gép toohello bejelentkezés után nyissa meg a **Kiszolgálókezelő**. Hello bal oldali ablaktáblában jelöljön ki **fájl- és tárolási szolgáltatások**.

    ![Nyissa meg a Kiszolgálókezelőt](../media/attach-disk-portal/fileandstorageservices.png)

3. Válassza ki **lemezek**.
4. Hello **lemezek** szakasz hello lemezek sorolja fel. Leggyakrabban a virtuális gépnek lemez 0, 1, és lemezzel 2. 0 lemez hello operációsrendszer-lemez, lemez hello ideiglenes lemez 1, pedig 2 lemez adatlemez hello újonnan csatolt toohello virtuális gép. hello adatok lemez listák hello partíciót **ismeretlen**.

 Kattintson a jobb gombbal a hello lemez, és válassza ki **inicializálása**.

5. Értesítést kap, hogy minden adat törlődik, hello lemez inicializálásakor. Kattintson a **Igen** tooacknowledge hello figyelmeztető és inicializálási hello lemez. Művelet befejeződése után hello partíció állapottal jelenik **GPT**. Kattintson ismét a jobb gombbal hello lemez, és válassza ki **új kötet**.

6. Hello alapértelmezett értékeivel hello varázsló befejezéséhez. Ha hello varázsló elvégezte, hello **kötetek** szakasz hello új kötet sorolja fel. hello lemez most már online és üzemkész toostore adatokat.

    ![Sikeresen inicializálta kötet](./media/attach-disk/newdiskafterinitialization.png)

## <a name="option-2-attach-an-existing-disk"></a>2. lehetőség: A meglévő lemez csatolása
1. A hello **lemezek** panelen kattintson a **Csatolás meglévő**.
2. A **meglévő lemez csatolása**, kattintson a **hely**.

   ![Meglévő lemez csatolása](./media/attach-disk/attachexistingdisksettings.png)
3. A **tárfiókok**, válassza ki a hello fiók és a tároló, amely hello .vhd fájl.

   ![Található virtuális merevlemez helye](./media/attach-disk/existdiskstorageaccountandcontainer.png)

4. Válassza ki a hello .vhd fájlt.
5. A **meglévő lemez csatolása**, csak a kijelölt hello fájl megtalálható-e **VHD-fájl**. Kattintson az **OK** gombra.
6. Után az Azure rendeli hello lemez toohello virtuális gép, hello virtuális gép lemezbeállításokat alatt felsorolt **adatlemezek**.

## <a name="use-trim-with-standard-storage"></a>Standard szintű tárolóval vágást használata

Standard szintű tárolást (HDD) használatakor vágást kell engedélyeznie. VÁGÁS elveti a nem használt blokkok hello lemezen, így csak a ténylegesen használt tárolási kell fizetni. VÁGÁS használatával mentheti a költségek, beleértve a nem használt blokkok a nagy fájlok törlése.

A parancs toocheck hello vágás beállítás is futtathatja. Nyissa meg a Windows virtuális gép, és írja be a parancssorba:

```
fsutil behavior query DisableDeleteNotify
```

Hello parancs 0 értéket adja vissza, vágást engedélyezve van-e megfelelően. 1 adja vissza, ha futtassa a következő parancs tooenable vágást hello:
```
fsutil behavior set DisableDeleteNotify 0
```

## <a name="next-steps"></a>Következő lépések
Ha az alkalmazás toouse hello D: meghajtó toostore adatokra van szüksége, akkor [hello meghajtóbetűjel hello Windows ideiglenes lemez](../../virtual-machines-windows-change-drive-letter.md).

## <a name="additional-resources"></a>További források
[Lemez és virtuális merevlemezek a virtuális gépek](../../virtual-machines-linux-about-disks-vhds.md)
