---
title: "erről a VHD-fájl egy Azure DevTest Labs egyéni lemezképet aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egyéni lemezképként az Azure DevTest Labs szolgáltatásban virtuális merevlemez fájl használatával hello Azure-portálon"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: b795bc61-7c28-40e6-82fc-96d629ee0568
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.openlocfilehash: 80af8ea1cb72380f868df0a76c4a0dcd92e63cf5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-image-from-a-vhd-file"></a>Létrehozhat egyéni rendszerképeket a VHD-fájl

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a>Lépésenkénti utasítások

hello következő lépések végigvezetik a VHD-fájlt hello Azure-portálon az egyéni lemezkép létrehozása:

1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Válassza ki **további szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.

1. Labs hello listában jelölje ki hello kívánt labor.  

1. Hello labor paneljén válassza **konfigurációs**. 

1. A tesztkörnyezet hello **konfigurációs** panelen válassza **egyéni lemezképeket (VHD)**.

1. A hello **egyéni lemezképek** panelen válassza **+ Hozzáadás**.

    ![Egyéni lemezkép hozzáadása](./media/devtest-lab-create-template/add-custom-image.png)

1. Adjon meg egyéni lemezkép hello hello nevét. Ez a név alap képek hello listája jelenik meg a virtuális gépek létrehozásakor.

1. Adja meg a hello egyéni lemezkép hello leírását. A leírást alap képek hello listája jelenik meg a virtuális gépek létrehozásakor.

1. Válassza ki **VHD**.

1. A hello **VHD** panelen, a select hello kívánt VHD-fájlt.

1. Válassza ki **OK** tooclose hello **VHD** panelen.

1. Válassza ki **operációs rendszer konfigurációja**.

1. A hello **operációs rendszer konfigurációja** lapra, válassza ki vagy **Windows** vagy **Linux**.

1. Ha **Windows** van jelölve, adja meg a hello jelölőnégyzet keresztül e *Sysprep* hello gépen futott. 

1. Válassza ki **OK** tooclose hello **operációs rendszer konfigurációja** panelen.

1. Válassza ki **OK** toocreate hello egyéni lemezképet.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Kapcsolódó blogbejegyzések

- [Egyéni lemezképek vagy képletek?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Az Azure DevTest Labs között egyéni lemezképek másolása](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a>Következő lépések

- [Virtuális gép tooyour labor hozzáadása](./devtest-lab-add-vm-with-artifacts.md)
