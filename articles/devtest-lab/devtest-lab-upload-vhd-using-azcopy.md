---
title: "virtuális merevlemez aaaUpload fájl tooAzure DevTest Labs AzCopy használatával |} Microsoft Docs"
description: "Töltse fel a VHD fájlt toolab tárfiók AzCopy használatával"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.openlocfilehash: 14f9e933b0bd27451f6bcb94841ecc381213e578
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-vhd-file-toolabs-storage-account-using-azcopy"></a>Töltse fel a VHD fájlt toolab tárfiók AzCopy használatával

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

Az Azure DevTest Labs szolgáltatásban a VHD-fájlokat lehet használt toocreate egyéni képek, amelyek használt tooprovision virtuális gépek. hello lépések végigvezetik Önt hello AzCopy parancssori segédprogram tooupload használatával egy virtuális merevlemez fájl tooa tesztkörnyezet tárfiókja. A VHD-fájl feltöltése után hello [további lépések szakaszt](#next-steps) felsorolja az egyes cikkeket, amelyek bemutatják, hogyan toocreate hello egy egyéni lemezkép feltöltése a VHD-fájlt. További információ a lemezek és a VHD-ken az Azure-ban: [lemezek és a virtuális merevlemezek a virtuális gépek](../virtual-machines/linux/about-disks-and-vhds.md)

> [!NOTE] 
>  
> AzCopy csak Windows parancssori segédprogram.

## <a name="step-by-step-instructions"></a>Lépésenkénti utasítások

következő lépések bejárása le a virtuális merevlemez feltöltése fájl tooAzure DevTest Labs segítségével hello [AzCopy](http://aka.ms/downloadazcopy). 

1. Hello Azure-portál használatával hello tesztkörnyezet tárfiókja nevére hello beolvasása:

1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Válassza ki **további szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.

1. Labs hello listában jelölje ki hello kívánt labor.  

1. Hello labor paneljén válassza **konfigurációs**. 

1. A tesztkörnyezet hello **konfigurációs** panelen válassza **egyéni lemezképeket (VHD)**.

1. A hello **egyéni lemezképek** panelen válasszon ki **+ Hozzáadás**. 

1. A hello **egyéni lemezkép** panelen válassza **VHD**.

1. A hello **VHD** panelen válassza **a PowerShell használatával virtuális merevlemez feltöltéséhez**.

    ![Töltse fel a virtuális merevlemez a PowerShell használatával](./media/devtest-lab-upload-vhd-using-azcopy/upload-image-using-psh.png)

1. Hello **PowerShell-lel lemezkép feltöltése a** panelt jeleníti meg a hívás toohello **Add-AzureVhd** parancsmag. az első paraméter hello (*cél*) hello URI blob-tároló tartalmazza (*feltölt*) a következő formátum hello:

    ```
    https://<STORAGE-ACCOUNT-NAME>.blob.core.windows.net/uploads/...
    ``` 

1. Jegyezze fel a hello teljes URI, a későbbi lépésekben használatban van.

1. Töltse fel az AzCopy segítségével hello VHD-fájlt:
 
1. [Töltse le és telepítse az AzCopy legújabb verzióját hello](http://aka.ms/downloadazcopy).

1. Nyisson meg egy parancsablakot, és keresse meg a toohello AzCopy telepítési könyvtárára. Másik lehetőségként hello AzCopy telepítési tooyour rendszer elérési útjának is hozzáadhat. Alapértelmezés szerint az AzCopy telepített toohello a következő könyvtár:

    ```command-line
    %ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy
    ```

1. Hello fiók kulcs és a blob tároló URI, futtassa a következő parancs hello parancssorba hello segítségével. Hello *vhdFileName* értéket kell toobe idézőjelben. a VHD-fájl feltöltése hello folyamat megnőhet hello hello VHD-fájl méretétől és a kapcsolat sebességétől függően.   

    ```command-line
    AzCopy /Source:<sourceDirectory> /Dest:<blobContainerUri> /DestKey:<storageAccountKey> /Pattern:"<vhdFileName>" /BlobType:page
    ```

## <a name="next-steps"></a>Következő lépések

- [Hozzon létre egy egyéni lemezképet egy VHD-fájlt hello Azure-portálon az Azure DevTest Labs szolgáltatásban](devtest-lab-create-template.md)
- [Egyéni lemezkép létrehozása a PowerShell használatával VHD-fájl az Azure DevTest Labs szolgáltatásban](devtest-lab-create-custom-image-from-vhd-using-powershell.md)