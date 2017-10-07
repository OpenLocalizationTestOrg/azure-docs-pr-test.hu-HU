---
title: "Azure Linux virtuális merevlemez aaaDownload |} Microsoft Docs"
description: "Töltse le a Linux virtuális merevlemez hello Azure parancssori felület használatával, és hello Azure-portálon."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: davidmu
ms.openlocfilehash: 7e08e985a64a6be581b8f5eedcce60fbd314eaf1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="download-a-linux-vhd-from-azure"></a>Töltse le a Linux virtuális merevlemez az Azure-ból

Ebből a cikkből megismerheti, hogyan toodownload egy [Linux virtuális merevlemez (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) a fájlt az Azure használatával hello Azure CLI és az Azure-portálon. 

Virtuális gépek (VM) Azure használatban [lemezek](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) egy hely toostore az operációs rendszerek, alkalmazások és adatok. Minden Azure virtuális gépek legalább két lemezt – a Windows operációs rendszer és egy ideiglenes lemezzel rendelkezik. hello operációsrendszer-lemez először lemezképből hozza létre, és hello operációsrendszer-lemez és a hello lemezkép az Azure storage-fiókban tárolt VHD-k. Virtuális gépek is rendelkeznek legalább egy adatlemezt, virtuális merevlemezekként is tárolt.

Ha még nem tette meg, telepítse [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).

## <a name="stop-hello-vm"></a>Hello VM leállítása

Virtuális merevlemez nem lehet letölteni az Azure-ból csatlakoztatott tooa futó virtuális gép. Toostop hello VM toodownload virtuális merevlemez van szüksége. Ha azt szeretné, toouse egy virtuális Merevlemezt egy [kép](tutorial-custom-images.md) toocreate más virtuális gépek új lemezek toodeprovision kell, és generalize hello operációs rendszer hello szereplő fájlt, és állítsa le a virtuális gép hello. toouse hello VHD-t egy meglévő virtuális gép vagy adatlemez új példányának lemezként, akkor csak toostop kell és hello virtuális gép felszabadítása.

toouse VHD hello egy kép toocreate, más virtuális gépek, a lépések elvégzésével:

1. SSH, hello fióknevet és hello VM tooconnect tooit hello nyilvános IP-címét, és kiosztásának megszüntetése azt. hello + felhasználói paraméter is eltávolítja a hello utolsó kiépített felhasználói fiókot. Ha a fiók hitelesítő adatait, a virtuális gép toohello vannak sütés, hagyja ki ennek + a user paraméterhez. hello következő példában eltávolítjuk hello utolsó kiépített felhasználói fiók:

    ```bash
    ssh azureuser@40.118.249.235
    sudo waagent -deprovision+user -force
    exit 
    ```

2. Az Azure-fiók tooyour bejelentkezés [az bejelentkezési](https://docs.microsoft.com/cli/azure/#login).
3. Állítsa le és hello VM felszabadítani.

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

4. Virtuális gép hello generalize. 

    ```azurecli
    az vm generalize --resource-group myResourceGroup --name myVM
    ``` 

egy meglévő virtuális gép vagy adatlemez, új példányának lemezként VHD toouse hello végezze el ezeket a lépéseket:

1.  Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2.  Hello központ menüben kattintson a **virtuális gépek**.
3.  Válassza ki a virtuális gép hello hello listából.
4.  Virtuális gép hello hello paneljén kattintson **leállítása**.

    ![Virtuális gép leállítása](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a>SAS URL-cím létrehozása

toodownload hello VHD-fájlt kell toogenerate egy [közös hozzáférésű jogosultságkód (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL-CÍMÉT. Előállításakor az hello URL-cím lejárati idő toohello URL-cím van hozzárendelve.

1.  A hello VM hello paneljén hello menüben kattintson **lemezek**.
2.  Jelölje ki az operációs rendszert tároló lemezének hello hello virtuális gép, és kattintson **exportálása**.
3.  Kattintson a **URL-cím készítése**.

    ![URL-cím létrehozása](./media/download-vhd/export-generate.png)

## <a name="download-vhd"></a>Töltse le a virtuális merevlemez

1.  A hello URL-cím lett létrehozva kattintson a letöltés hello VHD-fájlt.

    ![Töltse le a virtuális merevlemez](./media/download-vhd/export-download.png)

2.  Előfordulhat, hogy tooclick **mentése** hello böngésző toostart hello letöltés. hello VHD-fájl alapértelmezett neve hello *abcd*.

    ![Kattintson a Mentés hello böngészőben](./media/download-vhd/export-save.png)

## <a name="next-steps"></a>Következő lépések

- Ismerje meg, hogyan túl[feltöltése és a Linux virtuális gép létrehozása az Azure CLI 2.0 hello egyéni lemezről](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 
- [Azure-lemezeket hello Azure CLI kezelése](tutorial-manage-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

