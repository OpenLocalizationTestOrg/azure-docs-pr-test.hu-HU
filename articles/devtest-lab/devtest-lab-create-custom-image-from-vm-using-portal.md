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
# <a name="create-a-custom-image-from-a-vm"></a><span data-ttu-id="87bb9-103">Egy egyéni lemezképet létrehozni egy virtuális</span><span class="sxs-lookup"><span data-stu-id="87bb9-103">Create a custom image from a VM</span></span>

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

## <a name="step-by-step-instructions"></a><span data-ttu-id="87bb9-104">Lépésenkénti utasítások</span><span class="sxs-lookup"><span data-stu-id="87bb9-104">Step-by-step instructions</span></span>

<span data-ttu-id="87bb9-105">Létrehozhat egyéni rendszerképeket kiosztott virtuális gépről, és ezt követően használja az adott egyéni lemezkép toocreate azonos virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="87bb9-105">You can create a custom image from a provisioned VM, and afterwards use that custom image toocreate identical VMs.</span></span> <span data-ttu-id="87bb9-106">hello lépések bemutatják, hogyan toocreate egyéni rendszerképet virtuális gép alapján:</span><span class="sxs-lookup"><span data-stu-id="87bb9-106">hello following steps illustrate how toocreate a custom image from a VM:</span></span>

1. <span data-ttu-id="87bb9-107">Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="87bb9-107">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="87bb9-108">Válassza ki **további szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.</span><span class="sxs-lookup"><span data-stu-id="87bb9-108">Select **More services**, and then select **DevTest Labs** from hello list.</span></span>

1. <span data-ttu-id="87bb9-109">Labs hello listában jelölje ki hello kívánt labor.</span><span class="sxs-lookup"><span data-stu-id="87bb9-109">From hello list of labs, select hello desired lab.</span></span>  

1. <span data-ttu-id="87bb9-110">Hello labor paneljén válassza **a virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="87bb9-110">On hello lab's blade, select **My virtual machines**.</span></span>
 
1. <span data-ttu-id="87bb9-111">A hello **a virtuális gépek** panelen, jelölje be hello VM, amelyből el kívánja toocreate hello egyéni lemezképet.</span><span class="sxs-lookup"><span data-stu-id="87bb9-111">On hello **My virtual machines** blade, select hello VM from which you want toocreate hello custom image.</span></span>

1. <span data-ttu-id="87bb9-112">Hello virtuális gép paneljén válassza **egyéni kép létrehozása (VHD)**.</span><span class="sxs-lookup"><span data-stu-id="87bb9-112">On hello VM's blade, select **Create custom image (VHD)**.</span></span>

    ![Hozzon létre egyéni lemezkép menüpont](./media/devtest-lab-create-template/create-custom-image.png)

1. <span data-ttu-id="87bb9-114">A hello **kép létrehozása** panelen adjon nevet és leírást az egyéni lemezképet.</span><span class="sxs-lookup"><span data-stu-id="87bb9-114">On hello **Create image** blade, enter a name and description for your custom image.</span></span> <span data-ttu-id="87bb9-115">Ez az információ körrel hello listája jelenik meg a virtuális gépek létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="87bb9-115">This information is displayed in hello list of bases when you create a VM.</span></span>

    ![Hozzon létre egyéni lemezkép panel](./media/devtest-lab-create-template/create-custom-image-blade.png)

1. <span data-ttu-id="87bb9-117">Adja meg, hogy a sysprep hello VM volt futtatva.</span><span class="sxs-lookup"><span data-stu-id="87bb9-117">Select whether sysprep was run on hello VM.</span></span> <span data-ttu-id="87bb9-118">Ha hello sysprep hello virtuális gép nem volt futtatva, adja meg, hogy egy virtuális gép létrehozásakor a egyéni lemezképből futtatja a sysprep.</span><span class="sxs-lookup"><span data-stu-id="87bb9-118">If hello sysprep was not run on hello VM, specify whether you want sysprep run when a VM is created from this custom image.</span></span>

1. <span data-ttu-id="87bb9-119">Válassza ki **OK** Amikor végzett toocreate hello egyéni lemezképet.</span><span class="sxs-lookup"><span data-stu-id="87bb9-119">Select **OK** when finished toocreate hello custom image.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="87bb9-120">Kapcsolódó blogbejegyzések</span><span class="sxs-lookup"><span data-stu-id="87bb9-120">Related blog posts</span></span>

- [<span data-ttu-id="87bb9-121">Egyéni lemezképek vagy képletek?</span><span class="sxs-lookup"><span data-stu-id="87bb9-121">Custom images or formulas?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [<span data-ttu-id="87bb9-122">Az Azure DevTest Labs között egyéni lemezképek másolása</span><span class="sxs-lookup"><span data-stu-id="87bb9-122">Copying Custom Images between Azure DevTest Labs</span></span>](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a><span data-ttu-id="87bb9-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="87bb9-123">Next steps</span></span>

- [<span data-ttu-id="87bb9-124">Virtuális gép tooyour labor hozzáadása</span><span class="sxs-lookup"><span data-stu-id="87bb9-124">Add a VM tooyour lab</span></span>](./devtest-lab-add-vm-with-artifacts.md)
