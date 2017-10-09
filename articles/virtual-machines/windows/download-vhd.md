---
title: "az Azure-ból Windows VHD aaaDownload |} Microsoft Docs"
description: "Töltse le a Windows virtuális merevlemez hello Azure-portál használatával."
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
ms.openlocfilehash: d0ca8842db98f22751f01648c0ba4e5cde090043
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="download-a-windows-vhd-from-azure"></a>Töltse le a virtuális merevlemez Windows Azure-ból

Ebből a cikkből megismerheti, hogyan toodownload egy [Windows virtuális merevlemez (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a fájlt az Azure használatával hello Azure-portálon. 

Virtuális gépek (VM) Azure használatban [lemezek](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) egy hely toostore az operációs rendszerek, alkalmazások és adatok. Minden Azure virtuális gépek legalább két lemezt – a Windows operációs rendszer és egy ideiglenes lemezzel rendelkezik. hello operációsrendszer-lemez először lemezképből hozza létre, és hello operációsrendszer-lemez és a hello lemezkép az Azure storage-fiókban tárolt VHD-k. Virtuális gépek is rendelkeznek legalább egy adatlemezt, virtuális merevlemezekként is tárolt.

## <a name="stop-hello-vm"></a>Hello VM leállítása

Virtuális merevlemez nem lehet letölteni az Azure-ból csatlakoztatott tooa futó virtuális gép. Toostop hello VM toodownload virtuális merevlemez van szüksége. Ha azt szeretné, toouse egy virtuális Merevlemezt egy [kép](tutorial-custom-images.md) toocreate más virtuális gépek új lemezek bevezetéséhez, használhat [Sysprep](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation) toogeneralize hello hello fájlban lévő operációs rendszer, és állíthatja le a virtuális gép hello. toouse hello VHD-t egy meglévő virtuális gép vagy adatlemez új példányának lemezként, akkor csak toostop kell és hello virtuális gép felszabadítása.

toouse VHD hello egy kép toocreate, más virtuális gépek, a lépések elvégzésével:

1.  Ha még nem tette meg, jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2.  [Csatlakozás a virtuális gép toohello](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 
3.  A virtuális gép hello nyissa meg a hello parancssort rendszergazdaként.
4.  Hello könyvtárváltás túl*%windir%\system32\sysprep* , és futtassa a sysprep.exe.
5.  Hello rendszer-előkészítő eszköz párbeszédpanel, válassza ki a **adja meg a rendszer Out-of-Box élmény (OOBE)**, és győződjön meg arról, hogy **Generalize** van kiválasztva.
6.  Válassza a leállítási beállítások **leállítási**, és kattintson a **OK**. 

egy meglévő virtuális gép vagy adatlemez, új példányának lemezként VHD toouse hello végezze el ezeket a lépéseket:

1.  Az Azure-portálon hello hello központ menüben kattintson a **virtuális gépek**.
2.  Válassza ki a virtuális gép hello hello listából.
3.  Virtuális gép hello hello paneljén kattintson **leállítása**.

    ![Virtuális gép leállítása](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a>SAS URL-cím létrehozása

toodownload hello VHD-fájlt kell toogenerate egy [közös hozzáférésű jogosultságkód (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL-CÍMÉT. Előállításakor az hello URL-cím lejárati idő toohello URL-cím van hozzárendelve.

1.  A hello VM hello paneljén hello menüben kattintson **lemezek**.
2.  Jelölje ki az operációs rendszert tároló lemezének hello hello virtuális gép, és kattintson **exportálása**.
3.  Hello lejárati idő hello URL-címének beállítása túl*36000*.
4.  Kattintson a **URL-cím készítése**.

    ![URL-cím létrehozása](./media/download-vhd/export-generate.png)

> [!NOTE]
> hello lejárati idő jobb lesz a hello alapértelmezett tooprovide elegendő időt toodownload hello nagy VHD-fájlt a Windows Server operációs rendszert. A VHD-fájlt, amely tartalmazza a hello Windows Server operációs rendszer tootake több óra toodownload attól függően, hogy a kapcsolat számíthat. Ha adatlemezt virtuális letölteni, hello alapértelmezett érték megfelelő. 
> 
> 

## <a name="download-vhd"></a>Töltse le a virtuális merevlemez

1.  A hello URL-cím lett létrehozva kattintson a letöltés hello VHD-fájlt.

    ![Töltse le a virtuális merevlemez](./media/download-vhd/export-download.png)

2.  Előfordulhat, hogy tooclick **mentése** hello böngésző toostart hello letöltés. hello VHD-fájl alapértelmezett neve hello *abcd*.

    ![Kattintson a Mentés hello böngészőben](./media/download-vhd/export-save.png)

## <a name="next-steps"></a>Következő lépések

- Ismerje meg, hogyan túl[töltse fel a VHD-fájl tooAzure](upload-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 
- [Hozzon létre felügyelt lemezek tárfiókokban nem felügyelt lemezekből](attach-disk-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
- [Azure-lemezeket a PowerShell-lel kezelése](tutorial-manage-data-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

