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
# <a name="create-a-custom-image-from-a-vhd-file"></a><span data-ttu-id="b97d6-103">Létrehozhat egyéni rendszerképeket a VHD-fájl</span><span class="sxs-lookup"><span data-stu-id="b97d6-103">Create a custom image from a VHD file</span></span>

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a><span data-ttu-id="b97d6-104">Lépésenkénti utasítások</span><span class="sxs-lookup"><span data-stu-id="b97d6-104">Step-by-step instructions</span></span>

<span data-ttu-id="b97d6-105">hello következő lépések végigvezetik a VHD-fájlt hello Azure-portálon az egyéni lemezkép létrehozása:</span><span class="sxs-lookup"><span data-stu-id="b97d6-105">hello following steps walk you through creating a custom image from a VHD file using hello Azure portal:</span></span>

1. <span data-ttu-id="b97d6-106">Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="b97d6-106">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="b97d6-107">Válassza ki **további szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.</span><span class="sxs-lookup"><span data-stu-id="b97d6-107">Select **More services**, and then select **DevTest Labs** from hello list.</span></span>

1. <span data-ttu-id="b97d6-108">Labs hello listában jelölje ki hello kívánt labor.</span><span class="sxs-lookup"><span data-stu-id="b97d6-108">From hello list of labs, select hello desired lab.</span></span>  

1. <span data-ttu-id="b97d6-109">Hello labor paneljén válassza **konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="b97d6-109">On hello lab's blade, select **Configuration**.</span></span> 

1. <span data-ttu-id="b97d6-110">A tesztkörnyezet hello **konfigurációs** panelen válassza **egyéni lemezképeket (VHD)**.</span><span class="sxs-lookup"><span data-stu-id="b97d6-110">On hello lab **Configuration** blade, select **Custom images (VHDs)**.</span></span>

1. <span data-ttu-id="b97d6-111">A hello **egyéni lemezképek** panelen válassza **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="b97d6-111">On hello **Custom images** blade, select **+Add**.</span></span>

    ![Egyéni lemezkép hozzáadása](./media/devtest-lab-create-template/add-custom-image.png)

1. <span data-ttu-id="b97d6-113">Adjon meg egyéni lemezkép hello hello nevét.</span><span class="sxs-lookup"><span data-stu-id="b97d6-113">Enter hello name of hello custom image.</span></span> <span data-ttu-id="b97d6-114">Ez a név alap képek hello listája jelenik meg a virtuális gépek létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="b97d6-114">This name is displayed in hello list of base images when creating a VM.</span></span>

1. <span data-ttu-id="b97d6-115">Adja meg a hello egyéni lemezkép hello leírását.</span><span class="sxs-lookup"><span data-stu-id="b97d6-115">Enter hello description of hello custom image.</span></span> <span data-ttu-id="b97d6-116">A leírást alap képek hello listája jelenik meg a virtuális gépek létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="b97d6-116">This description is displayed in hello list of base images when creating a VM.</span></span>

1. <span data-ttu-id="b97d6-117">Válassza ki **VHD**.</span><span class="sxs-lookup"><span data-stu-id="b97d6-117">Select **VHD**.</span></span>

1. <span data-ttu-id="b97d6-118">A hello **VHD** panelen, a select hello kívánt VHD-fájlt.</span><span class="sxs-lookup"><span data-stu-id="b97d6-118">From hello **VHD** blade, select hello desired VHD file.</span></span>

1. <span data-ttu-id="b97d6-119">Válassza ki **OK** tooclose hello **VHD** panelen.</span><span class="sxs-lookup"><span data-stu-id="b97d6-119">Select **OK** tooclose hello **VHD** blade.</span></span>

1. <span data-ttu-id="b97d6-120">Válassza ki **operációs rendszer konfigurációja**.</span><span class="sxs-lookup"><span data-stu-id="b97d6-120">Select **OS configuration**.</span></span>

1. <span data-ttu-id="b97d6-121">A hello **operációs rendszer konfigurációja** lapra, válassza ki vagy **Windows** vagy **Linux**.</span><span class="sxs-lookup"><span data-stu-id="b97d6-121">On hello **OS configuration** tab, select either **Windows** or **Linux**.</span></span>

1. <span data-ttu-id="b97d6-122">Ha **Windows** van jelölve, adja meg a hello jelölőnégyzet keresztül e *Sysprep* hello gépen futott.</span><span class="sxs-lookup"><span data-stu-id="b97d6-122">If **Windows** is selected, specify via hello checkbox whether *Sysprep* has been run on hello machine.</span></span> 

1. <span data-ttu-id="b97d6-123">Válassza ki **OK** tooclose hello **operációs rendszer konfigurációja** panelen.</span><span class="sxs-lookup"><span data-stu-id="b97d6-123">Select **OK** tooclose hello **OS configuration** blade.</span></span>

1. <span data-ttu-id="b97d6-124">Válassza ki **OK** toocreate hello egyéni lemezképet.</span><span class="sxs-lookup"><span data-stu-id="b97d6-124">Select **OK** toocreate hello custom image.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="b97d6-125">Kapcsolódó blogbejegyzések</span><span class="sxs-lookup"><span data-stu-id="b97d6-125">Related blog posts</span></span>

- [<span data-ttu-id="b97d6-126">Egyéni lemezképek vagy képletek?</span><span class="sxs-lookup"><span data-stu-id="b97d6-126">Custom images or formulas?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [<span data-ttu-id="b97d6-127">Az Azure DevTest Labs között egyéni lemezképek másolása</span><span class="sxs-lookup"><span data-stu-id="b97d6-127">Copying Custom Images between Azure DevTest Labs</span></span>](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a><span data-ttu-id="b97d6-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b97d6-128">Next steps</span></span>

- [<span data-ttu-id="b97d6-129">Virtuális gép tooyour labor hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b97d6-129">Add a VM tooyour lab</span></span>](./devtest-lab-add-vm-with-artifacts.md)
