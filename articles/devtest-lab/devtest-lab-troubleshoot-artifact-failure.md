---
title: "Azure DevTest Labs szolgáltatásban virtuális aaaDiagnose összetevő hibáinak |} Microsoft Docs"
description: "Megtudhatja, hogyan tootroubleshoot összetevő hibák a DevTest Labs szolgáltatásban"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 115e0086-3293-4adf-8738-9f639f31f918
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: tarcher
ms.openlocfilehash: 40b3cea72cf071cc5d9a6d002d309d923c3d3084
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-artifact-failures-in-hello-lab"></a><span data-ttu-id="54b94-103">Összetevő hibák diagnosztizálása a hello tesztkörnyezet</span><span class="sxs-lookup"><span data-stu-id="54b94-103">Diagnose artifact failures in hello lab</span></span> 
<span data-ttu-id="54b94-104">A létrehozást követően összetevő, toosee ellenőrizheti, ha sikeres vagy sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="54b94-104">After you have created an artifact, you can check toosee if it succeeded or failed.</span></span> <span data-ttu-id="54b94-105">Összetevő-naplókat a DevTest Labs szolgáltatásban információkkal toodiagnose összetevő hibát is használhatja.</span><span class="sxs-lookup"><span data-stu-id="54b94-105">Artifact logs in DevTest Labs provide information you can use toodiagnose an artifact failure.</span></span> <span data-ttu-id="54b94-106">Többféleképpen néhány hello összetevő naplózási információk a Windows virtuális gépek is megtekinthetők.</span><span class="sxs-lookup"><span data-stu-id="54b94-106">There are a couple different ways you can view hello artifact log information for a Windows VM.</span></span>

> [!NOTE]
> <span data-ttu-id="54b94-107">tooensure hibák helyesen azonosítani és ismertetése, fontos, hogy hello összetevő megfelelően épül.</span><span class="sxs-lookup"><span data-stu-id="54b94-107">tooensure that failures are correctly identified and explained, it is important that hello artifact is properly structured.</span></span> <span data-ttu-id="54b94-108">Hogyan toocorrectly összeállításához összetevő kapcsolatos információkért lásd: [egyéni összetevők létrehozása](devtest-lab-artifact-author.md).</span><span class="sxs-lookup"><span data-stu-id="54b94-108">For information about how toocorrectly construct an artifact, see [Create custom artifacts](devtest-lab-artifact-author.md).</span></span> <span data-ttu-id="54b94-109">Toosee például lehet egy megfelelően strukturált összetevő, és tekintse meg a [paraméter teszttípust](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-test-paramtypes) összetevő.</span><span class="sxs-lookup"><span data-stu-id="54b94-109">And toosee an example of a properly structured artifact, check out this [Test Parameter Types](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-test-paramtypes) artifact.</span></span>

## <a name="troubleshoot-artifact-failures-using-hello-azure-portal"></a><span data-ttu-id="54b94-110">Hello Azure-portál használatával összetevő hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="54b94-110">Troubleshoot artifact failures using hello Azure portal</span></span>
<span data-ttu-id="54b94-111">toouse hello Azure portál toodiagnose összetevő létrehozása során fellépő hibák kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="54b94-111">toouse hello Azure portal toodiagnose failures during artifact creation, follow these steps:</span></span>

1. <span data-ttu-id="54b94-112">Az erőforrások hello listájából válassza ki a labor.</span><span class="sxs-lookup"><span data-stu-id="54b94-112">From hello list of resources, select your lab.</span></span>

2. <span data-ttu-id="54b94-113">Válassza ki a Windows virtuális Gépet, amely tartalmazza a kívánt tooinvestigate hello összetevő hello.</span><span class="sxs-lookup"><span data-stu-id="54b94-113">Choose hello Windows VM that includes hello artifact you want tooinvestigate.</span></span>

3. <span data-ttu-id="54b94-114">A bal oldali panelen hello alatt **általános**, válassza a **összetevők**.</span><span class="sxs-lookup"><span data-stu-id="54b94-114">In hello left panel under **GENERAL**, choose **Artifacts**.</span></span> <span data-ttu-id="54b94-115">Meg ezt a virtuális Gépet társított összetevők listáját, jelző hello nevét hello összetevő és annak állapotát.</span><span class="sxs-lookup"><span data-stu-id="54b94-115">A list of artifacts associated with that VM appears, indicating hello name of hello artifact and its status.</span></span>

   ![Összetevő git-tárház példa](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifacts-failure.png)

4. <span data-ttu-id="54b94-117">Válassza ki, amely egy állapotát jeleníti meg összetevő **sikertelen**.</span><span class="sxs-lookup"><span data-stu-id="54b94-117">Choose an artifact that shows a status of **Failed**.</span></span> <span data-ttu-id="54b94-118">hello összetevő megnyílik, és egy hello összetevő hibája hello kapcsolatos adatokat tartalmazó bővítmény üzenetet jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="54b94-118">hello artifact opens and shows an extension message that includes details about hello failure of hello artifact.</span></span>

   ![Összetevő git-tárház példa](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error.png)


## <a name="troubleshoot-artifact-failures-from-within-hello-vm"></a><span data-ttu-id="54b94-120">Virtuális gép hello belül származó összetevő hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="54b94-120">Troubleshoot artifact failures from within hello VM</span></span>
<span data-ttu-id="54b94-121">tooview hello összetevő származó naplók hello virtuális gépekről, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="54b94-121">tooview hello artifact logs from within hello virtual machine, follow these steps:</span></span>

1. <span data-ttu-id="54b94-122">Jelentkezzen be a virtuális Gépet, amely tartalmazza azt szeretné, hogy toodiagnose hello összetevő toohello.</span><span class="sxs-lookup"><span data-stu-id="54b94-122">Log in toohello VM that contains hello artifact you want toodiagnose.</span></span>

2. <span data-ttu-id="54b94-123">Keresse meg a tooC:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.9\Status "1.9 esetén hello ügyféloldali Bővítmény verziószámát.</span><span class="sxs-lookup"><span data-stu-id="54b94-123">Navigate tooC:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.9\Status where "1.9 is hello CSE version number.</span></span>

   ![Összetevő git-tárház példa](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error-vm-status.png)

3. <span data-ttu-id="54b94-125">Nyissa meg hello **állapot** tooview adatait, hogy a segítségével összetevő hibák diagnosztizálása ezt a virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="54b94-125">Open hello **status** file tooview information that helps diagnose artifact failures for that VM.</span></span>




[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="54b94-126">Kapcsolódó blogbejegyzések</span><span class="sxs-lookup"><span data-stu-id="54b94-126">Related blog posts</span></span>
* [<span data-ttu-id="54b94-127">Csatlakozás a virtuális gép tooexisting resource manager-sablon használata az Azure DevTest Labs AD-tartomány</span><span class="sxs-lookup"><span data-stu-id="54b94-127">Join a VM tooexisting AD Domain using a resource manager template in Azure DevTest Labs</span></span>](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a><span data-ttu-id="54b94-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="54b94-128">Next steps</span></span>
* <span data-ttu-id="54b94-129">Ismerje meg, hogyan túl[egy Git-tárház tooa labor hozzáadása](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="54b94-129">Learn how too[add a Git repository tooa lab](devtest-lab-add-artifact-repo.md).</span></span>

