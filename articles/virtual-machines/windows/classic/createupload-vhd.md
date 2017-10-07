---
title: "aaaCreate és feltöltése a virtuális gépek lemezképet a Powershell használatával |} Microsoft Docs"
description: "Ismerje meg, toocreate, és töltse fel az általános Windows Server képet (VHD) hello klasszikus telepítési modell és Azure PowerShell használatával."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 8c4a08fe-7714-4bf0-be87-c728a7806d3f
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: cynthn
ms.openlocfilehash: 093b57c9157cea5f348c8ba02b5700c917adbcdd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-upload-a-windows-server-vhd-tooazure"></a>Hozzon létre, és töltse fel a Windows Server VHD tooAzure
Ez a cikk bemutatja, hogyan tooupload saját általánosított virtuális gép rendszerképet, a virtuális merevlemez (VHD), hogy használhassa az toocreate virtuális gépek. A lemezek és a Microsoft Azure virtuális merevlemezek kapcsolatos további tudnivalókért lásd: [kapcsolatos lemezek és a virtuális merevlemezek a virtuális gépek](../about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!IMPORTANT]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Emellett [feltöltése](../upload-generalized-managed.md) egy virtuális gép hello Resource Manager modellt használja.

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk feltételezi, hogy rendelkezik:

* **Azure-előfizetés** – Ha még nincs fiókja, akkor [szabad nyissa meg az Azure-fiók](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
* **[A Microsoft Azure PowerShell](/powershell/azure/overview)**  -rendelkezik hello Microsoft Azure PowerShell modul telepítve és konfigurálva toouse az előfizetéshez.
* **A. VHD-fájl** - támogatott Windows operációs rendszer egy .vhd fájl és csatlakoztatott tooa virtuális gép tárolja. Ellenőrizze a toosee, ha fut a virtuális merevlemez hello hello kiszolgálói szerepkörök Sysprep által támogatott. További információkért lásd: [Sysprep támogatási kiszolgálói szerepköre tekintetében](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

    > [!IMPORTANT]
    > hello VHDX formátum nem támogatott a Microsoft Azure-ban. Hello tooVHD lemezformátum Hyper-V kezelőjével vagy a hello konvertálhatja [Convert-VHD parancsmag](http://technet.microsoft.com/library/hh848454.aspx). További információkért lásd: a [blogbejegyzés](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).

## <a name="step-1-prep-hello-vhd"></a>1. lépés: Prep hello virtuális merevlemez
Mielőtt hello VHD tooAzure feltölti az általánosítást a Sysprep eszközzel hello toobe kell. Ez felkészíti hello VHD toobe képként használni. A Sysprep kapcsolatos részletekért lásd: [hogyan tooUse Sysprep: Bevezetés](http://technet.microsoft.com/library/bb457073.aspx). Készítsen biztonsági másolatot hello VM a Sysprep futtatása előtt.

Hello virtuális gépről, amely az operációs rendszer hello befejezéséhez, a következő eljárás hello telepítették:

1. Jelentkezzen be toohello operációs rendszer.
2. Nyisson meg egy parancssori ablakot rendszergazdaként. Hello könyvtárváltás túl**%windir%\system32\sysprep**, majd futtassa a `sysprep.exe`.

    ![Nyisson meg egy parancssori ablakot](./media/createupload-vhd/sysprep_commandprompt.png)
3. Hello **rendszer-előkészítő eszköz** párbeszédpanel jelenik meg.

   ![Indítsa el a Sysprep](./media/createupload-vhd/sysprepgeneral.png)
4. A hello **rendszer-előkészítő eszköz**, jelölje be **meg rendszer kívüli kezdőélmény (OOBE)** , és győződjön meg arról, hogy **Generalize** be van jelölve.
5. A **leállítási beállítások**, jelölje be **leállítási**.
6. Kattintson az **OK** gombra.

## <a name="step-2-create-a-storage-account-and-a-container"></a>2. lépés: A tárfiók és a tároló létrehozása
Egy Azure storage-fiókot, egy hely tooupload hello .vhd fájlt kell. Ez a lépés bemutatja, hogyan toocreate egy fiókot, vagy a get hello adatait a meglévő fiók van szüksége. Cserélje le a hello változók &lsaquo; zárójeleket &rsaquo; a saját adataival.

1. Bejelentkezés

    ```powershell
    Add-AzureAccount
    ```

2. Állítsa be az Azure-előfizetéshez.

    ```powershell
    Select-AzureSubscription -SubscriptionName <SubscriptionName>
    ```

3. Hozzon létre egy új tárfiókot. hello tárfiók hello neve egyedi, kell lennie 3-24 karakterből állhat. hello neve betűk és számok tetszőleges kombinációja lehet. Emellett szükség van egy helyen, például az "Amerikai keleti" toospecify

    ```powershell
    New-AzureStorageAccount –StorageAccountName <StorageAccountName> -Location <Location>
    ```

4. Új tárfiók hello beállítása hello alapértelmezettként.

    ```powershell
    Set-AzureSubscription -CurrentStorageAccountName <StorageAccountName> -SubscriptionName <SubscriptionName>
    ```

5. Hozzon létre egy új tároló.

    ```powershell
    New-AzureStorageContainer -Name <ContainerName> -Permission Off
    ```

## <a name="step-3-upload-hello-vhd-file"></a>3. lépés: Hello .vhd fájl feltöltése
Használjon hello [Add-AzureVhd](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevhd) tooupload hello VHD-t.

Hello Azure PowerShell ablakában hello előző lépésben használt típus hello következő parancsot, és cserélje le a hello változók &lsaquo; zárójeleket &rsaquo; a saját adataival.

```powershell
Add-AzureVhd -Destination "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -LocalFilePath <LocalPathtoVHDFile>
```

## <a name="step-4-add-hello-image-tooyour-list-of-custom-images"></a>4. lépés: Tooyour képlistában hello egyéni lemezképek hozzáadása
Használjon hello [Add-AzureVMImage](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevmimage) parancsmag tooadd hello kép toohello listája az egyéni lemezképek.

```powershell
Add-AzureVMImage -ImageName <ImageName> -MediaLocation "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -OS "Windows"
```

## <a name="next-steps"></a>Következő lépések
Most [hozzon létre egy egyéni virtuális Gépet](createportal.md) hello segítségével Rendszerkép feltöltése.
