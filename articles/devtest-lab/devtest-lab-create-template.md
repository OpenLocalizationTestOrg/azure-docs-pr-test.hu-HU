---
title: "A VHD-fájl Azure DevTest Labs egyéni lemezkép létrehozása |} Microsoft Docs"
description: "Útmutató egyéni lemezkép létrehozása a Azure DevTest Labs szolgáltatásban a VHD-fájl az Azure portál használatával"
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
ms.openlocfilehash: 9983ea9b847f44ed18a6169a4bdb224b63626a64
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-custom-image-from-a-vhd-file"></a><span data-ttu-id="957ad-103">Létrehozhat egyéni rendszerképeket a VHD-fájl</span><span class="sxs-lookup"><span data-stu-id="957ad-103">Create a custom image from a VHD file</span></span>

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a><span data-ttu-id="957ad-104">Lépésenkénti utasítások</span><span class="sxs-lookup"><span data-stu-id="957ad-104">Step-by-step instructions</span></span>

<span data-ttu-id="957ad-105">A következő lépések végigvezetik a VHD-fájl az Azure portál használatával egyéni lemezkép létrehozása:</span><span class="sxs-lookup"><span data-stu-id="957ad-105">The following steps walk you through creating a custom image from a VHD file using the Azure portal:</span></span>

1. <span data-ttu-id="957ad-106">Jelentkezzen be az [Azure Portalra](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="957ad-106">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="957ad-107">Válassza a **További szolgáltatások**, majd a **DevTest Labs** elemet a listából.</span><span class="sxs-lookup"><span data-stu-id="957ad-107">Select **More services**, and then select **DevTest Labs** from the list.</span></span>

1. <span data-ttu-id="957ad-108">Válassza ki a kívánt labor labs listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="957ad-108">From the list of labs, select the desired lab.</span></span>  

1. <span data-ttu-id="957ad-109">A labor paneljén válassza **konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="957ad-109">On the lab's blade, select **Configuration**.</span></span> 

1. <span data-ttu-id="957ad-110">A tesztlabor a **konfigurációs** panelen válassza **egyéni lemezképeket (VHD)**.</span><span class="sxs-lookup"><span data-stu-id="957ad-110">On the lab **Configuration** blade, select **Custom images (VHDs)**.</span></span>

1. <span data-ttu-id="957ad-111">Az a **egyéni lemezképek** panelen válassza **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="957ad-111">On the **Custom images** blade, select **+Add**.</span></span>

    ![Egyéni lemezkép hozzáadása](./media/devtest-lab-create-template/add-custom-image.png)

1. <span data-ttu-id="957ad-113">Adja meg az egyéni lemezkép nevét.</span><span class="sxs-lookup"><span data-stu-id="957ad-113">Enter the name of the custom image.</span></span> <span data-ttu-id="957ad-114">Ez a név alap képek listájának a virtuális gép létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="957ad-114">This name is displayed in the list of base images when creating a VM.</span></span>

1. <span data-ttu-id="957ad-115">Adja meg az egyéni lemezkép leírását.</span><span class="sxs-lookup"><span data-stu-id="957ad-115">Enter the description of the custom image.</span></span> <span data-ttu-id="957ad-116">A leírást alap képek listája jelenik meg a virtuális gépek létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="957ad-116">This description is displayed in the list of base images when creating a VM.</span></span>

1. <span data-ttu-id="957ad-117">Válassza ki **VHD**.</span><span class="sxs-lookup"><span data-stu-id="957ad-117">Select **VHD**.</span></span>

1. <span data-ttu-id="957ad-118">Az a **VHD** panelen válassza ki a kívánt VHD-fájlt.</span><span class="sxs-lookup"><span data-stu-id="957ad-118">From the **VHD** blade, select the desired VHD file.</span></span>

1. <span data-ttu-id="957ad-119">Válassza ki **OK** bezárásához a **VHD** panelen.</span><span class="sxs-lookup"><span data-stu-id="957ad-119">Select **OK** to close the **VHD** blade.</span></span>

1. <span data-ttu-id="957ad-120">Válassza ki **operációs rendszer konfigurációja**.</span><span class="sxs-lookup"><span data-stu-id="957ad-120">Select **OS configuration**.</span></span>

1. <span data-ttu-id="957ad-121">Az a **operációs rendszer konfigurációja** lapra, válassza ki vagy **Windows** vagy **Linux**.</span><span class="sxs-lookup"><span data-stu-id="957ad-121">On the **OS configuration** tab, select either **Windows** or **Linux**.</span></span>

1. <span data-ttu-id="957ad-122">Ha **Windows** van jelölve, adja meg a jelölőnégyzet keresztül e *Sysprep* már futtatták azon a gépen.</span><span class="sxs-lookup"><span data-stu-id="957ad-122">If **Windows** is selected, specify via the checkbox whether *Sysprep* has been run on the machine.</span></span> 

1. <span data-ttu-id="957ad-123">Válassza ki **OK** bezárásához a **operációs rendszer konfigurációja** panelen.</span><span class="sxs-lookup"><span data-stu-id="957ad-123">Select **OK** to close the **OS configuration** blade.</span></span>

1. <span data-ttu-id="957ad-124">Válassza ki **OK** egyéni lemezkép létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="957ad-124">Select **OK** to create the custom image.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="957ad-125">Kapcsolódó blogbejegyzések</span><span class="sxs-lookup"><span data-stu-id="957ad-125">Related blog posts</span></span>

- [<span data-ttu-id="957ad-126">Egyéni lemezképek vagy képletek?</span><span class="sxs-lookup"><span data-stu-id="957ad-126">Custom images or formulas?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [<span data-ttu-id="957ad-127">Az Azure DevTest Labs között egyéni lemezképek másolása</span><span class="sxs-lookup"><span data-stu-id="957ad-127">Copying Custom Images between Azure DevTest Labs</span></span>](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a><span data-ttu-id="957ad-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="957ad-128">Next steps</span></span>

- [<span data-ttu-id="957ad-129">A virtuális gépek hozzáadása a tesztkörnyezet</span><span class="sxs-lookup"><span data-stu-id="957ad-129">Add a VM to your lab</span></span>](./devtest-lab-add-vm-with-artifacts.md)
