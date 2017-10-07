---
title: "egy virtuális Gépet az Azure DevTest Labs egyéni lemezképének aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egyéni lemezképként az Azure DevTest Labs segítségével kiosztott virtuális gép hello Azure-portálon"
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
ms.openlocfilehash: 7dccb79d3db4aae676c7bd2f6b800301210491e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-image-from-a-vm"></a>Egy egyéni lemezképet létrehozni egy virtuális

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

## <a name="step-by-step-instructions"></a>Lépésenkénti utasítások

Létrehozhat egyéni rendszerképeket kiosztott virtuális gépről, és ezt követően használja az adott egyéni lemezkép toocreate azonos virtuális gépeket. hello lépések bemutatják, hogyan toocreate egyéni rendszerképet virtuális gép alapján:

1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Válassza ki **további szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.

1. Labs hello listában jelölje ki hello kívánt labor.  

1. Hello labor paneljén válassza **a virtuális gépek**.
 
1. A hello **a virtuális gépek** panelen, jelölje be hello VM, amelyből el kívánja toocreate hello egyéni lemezképet.

1. Hello virtuális gép paneljén válassza **egyéni kép létrehozása (VHD)**.

    ![Hozzon létre egyéni lemezkép menüpont](./media/devtest-lab-create-template/create-custom-image.png)

1. A hello **kép létrehozása** panelen adjon nevet és leírást az egyéni lemezképet. Ez az információ körrel hello listája jelenik meg a virtuális gépek létrehozásakor.

    ![Hozzon létre egyéni lemezkép panel](./media/devtest-lab-create-template/create-custom-image-blade.png)

1. Adja meg, hogy a sysprep hello VM volt futtatva. Ha hello sysprep hello virtuális gép nem volt futtatva, adja meg, hogy egy virtuális gép létrehozásakor a egyéni lemezképből futtatja a sysprep.

1. Válassza ki **OK** Amikor végzett toocreate hello egyéni lemezképet.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Kapcsolódó blogbejegyzések

- [Egyéni lemezképek vagy képletek?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Az Azure DevTest Labs között egyéni lemezképek másolása](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a>Következő lépések

- [Virtuális gép tooyour labor hozzáadása](./devtest-lab-add-vm-with-artifacts.md)
