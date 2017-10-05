---
title: "Egy Azure DevTest Labs egyéni lemezképet létrehozni egy virtuális |} Microsoft Docs"
description: "Útmutató egyéni lemezkép létrehozása a Azure DevTest Labs szolgáltatásban az Azure portál használatával kiépített VM"
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
ms.openlocfilehash: 9d2dcf7164985508d691e8a0c123efaf3b8aa19a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-custom-image-from-a-vm"></a><span data-ttu-id="4b986-103">Egy egyéni lemezképet létrehozni egy virtuális</span><span class="sxs-lookup"><span data-stu-id="4b986-103">Create a custom image from a VM</span></span>

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

## <a name="step-by-step-instructions"></a><span data-ttu-id="4b986-104">Lépésenkénti utasítások</span><span class="sxs-lookup"><span data-stu-id="4b986-104">Step-by-step instructions</span></span>

<span data-ttu-id="4b986-105">Létrehozhat egyéni rendszerképeket kiosztott virtuális gépről, és ezt követően a egyéni lemezkép használatával hozzon létre azonos virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="4b986-105">You can create a custom image from a provisioned VM, and afterwards use that custom image to create identical VMs.</span></span> <span data-ttu-id="4b986-106">A következő lépések bemutatják, hogyan lehet egy egyéni lemezképet létrehozni egy virtuális:</span><span class="sxs-lookup"><span data-stu-id="4b986-106">The following steps illustrate how to create a custom image from a VM:</span></span>

1. <span data-ttu-id="4b986-107">Jelentkezzen be az [Azure Portalra](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="4b986-107">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="4b986-108">Válassza a **További szolgáltatások**, majd a **DevTest Labs** elemet a listából.</span><span class="sxs-lookup"><span data-stu-id="4b986-108">Select **More services**, and then select **DevTest Labs** from the list.</span></span>

1. <span data-ttu-id="4b986-109">Válassza ki a kívánt labor labs listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="4b986-109">From the list of labs, select the desired lab.</span></span>  

1. <span data-ttu-id="4b986-110">A labor paneljén válassza **a virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="4b986-110">On the lab's blade, select **My virtual machines**.</span></span>
 
1. <span data-ttu-id="4b986-111">Az a **a virtuális gépek** panelen válassza ki a virtuális gép, amelyből el kívánja az egyéni lemezkép létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="4b986-111">On the **My virtual machines** blade, select the VM from which you want to create the custom image.</span></span>

1. <span data-ttu-id="4b986-112">A virtuális gép paneljén válassza **egyéni kép létrehozása (VHD)**.</span><span class="sxs-lookup"><span data-stu-id="4b986-112">On the VM's blade, select **Create custom image (VHD)**.</span></span>

    ![Hozzon létre egyéni lemezkép menüpont](./media/devtest-lab-create-template/create-custom-image.png)

1. <span data-ttu-id="4b986-114">Az a **kép létrehozása** panelen adjon nevet és leírást az egyéni lemezképet.</span><span class="sxs-lookup"><span data-stu-id="4b986-114">On the **Create image** blade, enter a name and description for your custom image.</span></span> <span data-ttu-id="4b986-115">Ezt az információt az adatbázisok listája jelenik meg a virtuális gépek létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="4b986-115">This information is displayed in the list of bases when you create a VM.</span></span>

    ![Hozzon létre egyéni lemezkép panel](./media/devtest-lab-create-template/create-custom-image-blade.png)

1. <span data-ttu-id="4b986-117">Adja meg, hogy a sysprep futtatása a virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="4b986-117">Select whether sysprep was run on the VM.</span></span> <span data-ttu-id="4b986-118">Ha a sysprep nem futott le a virtuális Gépre, adja meg, hogy egy virtuális gép létrehozásakor a egyéni lemezképből futtatja a sysprep.</span><span class="sxs-lookup"><span data-stu-id="4b986-118">If the sysprep was not run on the VM, specify whether you want sysprep run when a VM is created from this custom image.</span></span>

1. <span data-ttu-id="4b986-119">Válassza ki **OK** amikor befejeződött az egyéni lemezkép létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="4b986-119">Select **OK** when finished to create the custom image.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="4b986-120">Kapcsolódó blogbejegyzések</span><span class="sxs-lookup"><span data-stu-id="4b986-120">Related blog posts</span></span>

- [<span data-ttu-id="4b986-121">Egyéni lemezképek vagy képletek?</span><span class="sxs-lookup"><span data-stu-id="4b986-121">Custom images or formulas?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [<span data-ttu-id="4b986-122">Az Azure DevTest Labs között egyéni lemezképek másolása</span><span class="sxs-lookup"><span data-stu-id="4b986-122">Copying Custom Images between Azure DevTest Labs</span></span>](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a><span data-ttu-id="4b986-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4b986-123">Next steps</span></span>

- [<span data-ttu-id="4b986-124">A virtuális gépek hozzáadása a tesztkörnyezet</span><span class="sxs-lookup"><span data-stu-id="4b986-124">Add a VM to your lab</span></span>](./devtest-lab-add-vm-with-artifacts.md)
