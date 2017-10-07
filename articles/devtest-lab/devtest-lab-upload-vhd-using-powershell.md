---
title: "virtuális merevlemez aaaUpload fájl tooAzure DevTest Labs PowerShell használatával |} Microsoft Docs"
description: "Töltse fel a VHD fájlt toolab tárfiók PowerShell használatával"
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
ms.openlocfilehash: 9c3ee96e212457b0ef8203714b419350cb97f895
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-vhd-file-toolabs-storage-account-using-powershell"></a>Töltse fel a VHD fájlt toolab tárfiók PowerShell használatával

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

Az Azure DevTest Labs szolgáltatásban a VHD-fájlokat lehet használt toocreate egyéni képek, amelyek használt tooprovision virtuális gépek. hello következő lépések végigvezetik PowerShell tooupload használatával egy virtuális merevlemez fájl tooa tesztkörnyezet tárfiókja. A VHD-fájl feltöltése után hello [további lépések szakaszt](#next-steps) felsorolja az egyes cikkeket, amelyek bemutatják, hogyan toocreate hello egy egyéni lemezkép feltöltése a VHD-fájlt. További információ a lemezek és a VHD-ken az Azure-ban: [lemezek és a virtuális merevlemezek a virtuális gépek](../virtual-machines/linux/about-disks-and-vhds.md)

## <a name="step-by-step-instructions"></a>Lépésenkénti utasítások

a következő lépéseket bejárása le a virtuális merevlemez feltöltése fájl tooAzure DevTest Labs PowerShell-lel hello. 

1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Válassza ki **további szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.

1. Labs hello listában jelölje ki hello kívánt labor.  

1. Hello labor paneljén válassza **konfigurációs**. 

1. A tesztkörnyezet hello **konfigurációs** panelen válassza **egyéni lemezképeket (VHD)**.

1. A hello **egyéni lemezképek** panelen válasszon ki **+ Hozzáadás**. 

1. A hello **egyéni lemezkép** panelen válassza **VHD**.

1. A hello **VHD** panelen válassza **a PowerShell használatával virtuális merevlemez feltöltéséhez**.

    ![Töltse fel a virtuális merevlemez a PowerShell használatával](./media/devtest-lab-upload-vhd-using-powershell/upload-image-using-psh.png)

1. A hello **PowerShell-lel lemezkép feltöltése a** panelen, a Másolás generált hello PowerShell parancsfájl tooa szövegszerkesztőben.

1. Módosítsa a hello **LocalFilePath** hello paramétere **Add-AzureVhd** parancsmag toopoint toohello hello tooupload kívánt VHD-fájl helyét.

1. A PowerShell parancssorból futtassa a hello **Add-AzureVhd** parancsmag (hello módosított rendelkező **LocalFilePath** paraméter).

> [!WARNING] 
> 
> a VHD-fájl feltöltése hello folyamat megnőhet hello hello VHD-fájl méretétől és a kapcsolat sebességétől függően.

## <a name="next-steps"></a>Következő lépések

- [Hozzon létre egy egyéni lemezképet egy VHD-fájlt hello Azure-portálon az Azure DevTest Labs szolgáltatásban](devtest-lab-create-template.md)
- [Egyéni lemezkép létrehozása a PowerShell használatával VHD-fájl az Azure DevTest Labs szolgáltatásban](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
