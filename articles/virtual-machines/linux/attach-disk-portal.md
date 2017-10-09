---
title: "egy adatok lemez tooa Linux virtuális gép aaaAttach |} Microsoft Docs"
description: "Hogyan tooattach adatok új vagy meglévő lemez tooa Linux virtuális gép az Azure portál használatával hello hello Resource Manager üzembe helyezési modellben."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 5e1c6212-976c-4962-a297-177942f90907
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: cynthn
ms.openlocfilehash: 069c3c6f5e71a8c495342e6d0c6f3d92c4ed5053
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooattach-a-data-disk-tooa-linux-vm-in-hello-azure-portal"></a>Hogyan tooattach az adatok lemezre tooa Linux virtuális gép a hello Azure-portálon
Ez a cikk bemutatja, hogyan tooattach meglévő és új lemezek tooa Linux virtuális gép hello Azure-portálon keresztül. Emellett [csatolni egy adatok lemez tooa VM a Windows hello Azure-portálon](../windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Választhat toouse vagy Azure felügyelt lemezek vagy nem felügyelt lemezek. Felügyelt lemezek hello Azure platform kezeli, és nem igényelnek bármely előkészítő vagy a hely toostore őket. Nem felügyelt lemezeket a tárfiók szükséges, és olyan [kvótái és korlátai vonatkozó](../../azure-subscription-service-limits.md#storage-limits). További információ az Azure Managed Disksről: [Azure Managed Disks – áttekintés](../windows/managed-disks-overview.md).

Lemezek tooyour virtuális gép csatlakoztatása előtt tekintse át a ezek a tippek:

* hello mérete hello virtuális gép csatolhat hány adatlemezek határozza meg. További információkért lásd: [virtuális gépek méretei](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* toouse prémium szintű storage, kell a DS-méretek és GS sorozatnak virtuális gép. Ezek a virtuális gépek prémium és Standard lemezek is használható. Prémium szintű storage bizonyos régiókban érhető el. További információkért lásd: [prémium szintű Storage: nagy teljesítményű tárolást Azure virtuális gépek terheléseihez](../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Lemezek csatolt toovirtual gépek az Azure-ban tárolt ténylegesen .vhd fájlok. További információkért lásd: [lemezek és a virtuális merevlemezek a virtuális gépek](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="find-hello-virtual-machine"></a>Található hello virtuális gép
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Hello központ menüben kattintson a **virtuális gépek**.
3. Válassza ki a virtuális gép hello hello listából.
4. toohello virtuális gépek panelen a **Essentials**, kattintson a **lemezek**.
   
    ![Nyissa meg a lemez beállításai](./media/attach-disk-portal/find-disk-settings.png)

A következő utasítások vagy csatlakoztatása a következő lépésként a [felügyelt lemezes](#use-azure-managed-disks) vagy [nem felügyelt lemezt](#use-unmanaged-disks).

## <a name="use-azure-managed-disks"></a>Az Azure kezelt lemez használata

### <a name="attach-a-new-disk"></a>Új lemez csatolása

1. A hello **lemezek** panelen kattintson a **+ Hozzáadás adatlemez**.
2. Kattintson a legördülő menü hello **neve** válassza ki **létrehozása lemez**:

    ![Hozzon létre Azure felügyelt lemezes](./media/attach-disk-portal/create-new-md.png)

3. Adjon meg egy nevet a felügyelt lemezes. Tekintse át a hello alapértelmezett beállításokat, szükség esetén frissítse, majd **létrehozása**.
   
   ![Lemez beállítások áttekintése](./media/attach-disk-portal/create-new-md-settings.png)

4. Kattintson a **mentése** toocreate hello felügyelt lemezes és a frissítés hello Virtuálisgép-konfiguráció:

   ![Új Azure kezelt lemezre mentése](./media/attach-disk-portal/confirm-create-new-md.png)

5. Miután Azure hello lemezt hoz létre, és csatolja toohello virtuális gépet, hello új lemez szerepel-e hello virtuális gép lemezbeállításokat alatt **adatlemezek**. Egy legfelső szintű erőforrás felügyelt lemezek amennyi hello lemez hello gyökerében hello erőforráscsoport jelenik meg:

   ![Felügyelt lemezt Azure erőforráscsoport](./media/attach-disk-portal/view-md-resource-group.png)

### <a name="attach-an-existing-disk"></a>Meglévő lemez csatlakoztatása
1. A hello **lemezek** panelen kattintson a **+ Hozzáadás adatlemez**.
2. Kattintson a legördülő menü hello **neve** tooview meglévő listájának felügyelt lemezek elérhető tooyour Azure-előfizetés. Felügyelt válassza hello lemez tooattach:

   ![Meglévő Azure kezelt lemez csatolása](./media/attach-disk-portal/select-existing-md.png)

3. Kattintson a **mentése** tooattach hello meglévő felügyelt lemezes és a frissítés hello Virtuálisgép-konfiguráció:
   
   ![Azure kezelt lemez frissítések mentése](./media/attach-disk-portal/confirm-attach-existing-md.png)

4. Után az Azure rendeli hello lemez toohello virtuális gép, hello virtuális gép lemezbeállításokat alatt felsorolt **adatlemezek**.

## <a name="use-unmanaged-disks"></a>Nem felügyelt lemezek használata

### <a name="attach-a-new-disk"></a>Új lemez csatolása

1. A hello **lemezek** panelen kattintson a **+ Hozzáadás adatlemez**.
2. Tekintse át a hello alapértelmezett beállításokat, szükség esetén frissítse, majd **OK**.
   
   ![Lemez beállítások áttekintése](./media/attach-disk-portal/attach-new.png)
3. Miután Azure hello lemezt hoz létre, és csatolja toohello virtuális gépet, hello új lemez szerepel-e hello virtuális gép lemezbeállításokat alatt **adatlemezek**.

### <a name="attach-an-existing-disk"></a>Meglévő lemez csatlakoztatása
1. A hello **lemezek** panelen kattintson a **+ Hozzáadás adatlemez**.
2. A **meglévő lemez csatolása**, kattintson a **VHD-fájl**.
   
   ![Meglévő lemez csatolása](./media/attach-disk-portal/attach-existing.png)
3. A **tárfiókok**, válassza ki a hello fiók és a tároló, amely hello .vhd fájl.
   
   ![Található virtuális merevlemez helye](./media/attach-disk-portal/find-storage-container.png)
4. Válassza ki a hello .vhd fájlt.
5. A **meglévő lemez csatolása**, csak a kijelölt hello fájl megtalálható-e **VHD-fájl**. Kattintson az **OK** gombra.
6. Után az Azure rendeli hello lemez toohello virtuális gép, hello virtuális gép lemezbeállításokat alatt felsorolt **adatlemezek**.


## <a name="next-steps"></a>Következő lépések
Hello lemez hozzáadása után tooprepare kell azt való használatra. További információkért lásd: [Útmutató: a Linux új adatlemezt inicializálása](add-disk.md).
