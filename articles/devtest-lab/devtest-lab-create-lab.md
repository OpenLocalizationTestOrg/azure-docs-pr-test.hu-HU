---
title: az Azure DevTest Labs labor aaaCreate |} Microsoft Docs
description: "Labor létrehozása virtuális gépekhez az Azure DevTest Labs szolgáltatásban"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 8b6d3e70-6528-42a4-a2ef-449575d0f928
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/30/2017
ms.author: tarcher
ms.openlocfilehash: 2ec5498f10e0cc06a196a71e319e340937abb1a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="906ad-103">Labor létrehozása az Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="906ad-103">Create a lab in Azure DevTest Labs</span></span>
<span data-ttu-id="906ad-104">A Azure DevTest Labs szolgáltatásban labor hello infrastruktúra, amely magában foglalja az erőforrások csoportja, például, amely lehetővé teszi nagyobb virtuális gépek (VM), ezeket az erőforrásokat kezelése korlátozásai és a kvóták megadásával.</span><span class="sxs-lookup"><span data-stu-id="906ad-104">A lab in Azure DevTest Labs is hello infrastructure that encompasses a group of resources, such as Virtual Machines (VMs), that lets you better manage those resources by specifying limits and quotas.</span></span> <span data-ttu-id="906ad-105">Ez a cikk bemutatja, hogyan hello hello Azure-portál használatával labor létrehozásának folyamatán.</span><span class="sxs-lookup"><span data-stu-id="906ad-105">This article walks you through hello process of creating a lab using hello Azure portal.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="906ad-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="906ad-106">Prerequisites</span></span>
<span data-ttu-id="906ad-107">toocreate egy tesztkörnyezetet, lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="906ad-107">toocreate a lab, you need:</span></span>

* <span data-ttu-id="906ad-108">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="906ad-108">An Azure subscription.</span></span> <span data-ttu-id="906ad-109">Azure megvásárlási lehetőségeinek toolearn lásd [hogyan toobuy Azure](https://azure.microsoft.com/pricing/purchase-options/) vagy [ingyenes egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="906ad-109">toolearn about Azure purchase options, see [How toobuy Azure](https://azure.microsoft.com/pricing/purchase-options/) or [Free one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="906ad-110">Hello előfizetés toocreate hello labor hello tulajdonosának kell lennie.</span><span class="sxs-lookup"><span data-stu-id="906ad-110">You must be hello owner of hello subscription toocreate hello lab.</span></span>

## <a name="steps-toocreate-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="906ad-111">Lépéseket toocreate egy tesztkörnyezetet a Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="906ad-111">Steps toocreate a lab in Azure DevTest Labs</span></span>
<span data-ttu-id="906ad-112">hello lépések bemutatják, hogyan toouse hello Azure portál toocreate egy tesztkörnyezetet a Azure DevTest Labs szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="906ad-112">hello following steps illustrate how toouse hello Azure portal toocreate a lab in Azure DevTest Labs.</span></span> 

1. <span data-ttu-id="906ad-113">Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="906ad-113">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
1. <span data-ttu-id="906ad-114">Főmenüben hello hello bal oldalon válassza ki a **több szolgáltatások** (alján hello hello lista).</span><span class="sxs-lookup"><span data-stu-id="906ad-114">From hello main menu on hello left side, select **More Services** (at hello bottom of hello list).</span></span>

    ![További szolgáltatások menüpont](./media/devtest-lab-create-lab/more-services-menu-option.png)

1. <span data-ttu-id="906ad-116">Az elérhető szolgáltatások listájából hello **DevTest Labs**.</span><span class="sxs-lookup"><span data-stu-id="906ad-116">From hello list of available services, **DevTest Labs**.</span></span>
1. <span data-ttu-id="906ad-117">A hello **DevTest Labs** panelen válassza **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="906ad-117">On hello **DevTest Labs** blade, select **Add**.</span></span>
   
    ![Labor hozzáadása](./media/devtest-lab-create-lab/add-lab-button.png)

1. <span data-ttu-id="906ad-119">A hello **létrehozása a DevTest Lab** panel:</span><span class="sxs-lookup"><span data-stu-id="906ad-119">On hello **Create a DevTest Lab** blade:</span></span>
   
    1. <span data-ttu-id="906ad-120">Adjon meg egy **tesztkörnyezet nevének** hello új labor számára.</span><span class="sxs-lookup"><span data-stu-id="906ad-120">Enter a **Lab Name** for hello new lab.</span></span>
    2. <span data-ttu-id="906ad-121">Jelölje be hello **előfizetés** hello amelyiknek tooassociate.</span><span class="sxs-lookup"><span data-stu-id="906ad-121">Select hello **Subscription** tooassociate with hello lab.</span></span>
    3. <span data-ttu-id="906ad-122">Válassza ki a **hely** mely toostore hello tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="906ad-122">Select a **Location** in which toostore hello lab.</span></span>
    4. <span data-ttu-id="906ad-123">Válassza ki **automatikus leállítási** toospecify Ha tooenable - és paramétereit hello - hello automatikus leállítása az összes hello labor virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="906ad-123">Select **Auto-shutdown** toospecify if you want tooenable - and define hello parameters for - hello automatic shutting down of all hello lab's VMs.</span></span> <span data-ttu-id="906ad-124">hello automatikus leállítási szolgáltatása lehetővé teszi az főként költségkímélő során megadhatja, ha szeretné a virtuális gép hello tooautomatically le kell állítani.</span><span class="sxs-lookup"><span data-stu-id="906ad-124">hello auto-shutdown feature is mainly a cost-saving feature whereby you can specify when you want hello VM tooautomatically be shut down.</span></span> <span data-ttu-id="906ad-125">Hello a cikkben leírt hello lépéseket követve hello labor létrehozása után módosíthatja az automatikus leállítási beállítások [egy Azure DevTest Labs szolgáltatásban található, amikor az összes házirend kezeléséhez](./devtest-lab-set-lab-policy.md#set-auto-shutdown).</span><span class="sxs-lookup"><span data-stu-id="906ad-125">You can change auto-shutdown settings after creating hello lab by following hello steps outlined in hello article, [Manage all policies for a lab in Azure DevTest Labs](./devtest-lab-set-lab-policy.md#set-auto-shutdown).</span></span>
    5. <span data-ttu-id="906ad-126">Válassza ki **PIN-kód tooDashboard** Ha azt szeretné, hogy hello labor tooappear hello portál irányítópultján a parancsikont.</span><span class="sxs-lookup"><span data-stu-id="906ad-126">Select **Pin tooDashboard** if you want a shortcut of hello lab tooappear on hello portal dashboard.</span></span>
    6. <span data-ttu-id="906ad-127">Válassza ki **automatizálási lehetőségek** tooget Azure Resource Manager-sablonok a konfigurációs automatizálásához.</span><span class="sxs-lookup"><span data-stu-id="906ad-127">Select **Automation options** tooget Azure Resource Manager templates for configuration automation.</span></span> 
    7. <span data-ttu-id="906ad-128">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="906ad-128">Select **Create**.</span></span> <span data-ttu-id="906ad-129">Kiválasztása után **létrehozása**, hello **DevTest Labs** panelt jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="906ad-129">After selecting **Create**, hello **DevTest Labs** blade displays.</span></span> <span data-ttu-id="906ad-130">Hello labor létrehozási folyamata hello állapotának végezhet figyelést, ha hello figyeli **értesítések** területen.</span><span class="sxs-lookup"><span data-stu-id="906ad-130">You can monitor hello status of hello lab creation process by watching hello **Notifications** area.</span></span> <span data-ttu-id="906ad-131">Ezt követően frissítse a hello lap toosee az újonnan létrehozott hello labor labs hello listájában.</span><span class="sxs-lookup"><span data-stu-id="906ad-131">Once completed, refresh hello page toosee hello newly created lab in hello list of labs.</span></span>  
    
    ![Laborpanel létrehozása](./media/devtest-lab-create-lab/create-devtestlab-blade.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="906ad-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="906ad-133">Next steps</span></span>
<span data-ttu-id="906ad-134">A labor létrehozása után az alábbiakban néhány következő lépések tooconsider:</span><span class="sxs-lookup"><span data-stu-id="906ad-134">Once you've created your lab, here are some next steps tooconsider:</span></span>

* <span data-ttu-id="906ad-135">[Biztonságos hozzáférés tooa labor](devtest-lab-add-devtest-user.md).</span><span class="sxs-lookup"><span data-stu-id="906ad-135">[Secure access tooa lab](devtest-lab-add-devtest-user.md).</span></span>
* <span data-ttu-id="906ad-136">[Laborházirendek megadása](devtest-lab-set-lab-policy.md).</span><span class="sxs-lookup"><span data-stu-id="906ad-136">[Set lab policies](devtest-lab-set-lab-policy.md).</span></span>
* <span data-ttu-id="906ad-137">[Laborsablon létrehozása](devtest-lab-create-template.md).</span><span class="sxs-lookup"><span data-stu-id="906ad-137">[Create a lab template](devtest-lab-create-template.md).</span></span>
* <span data-ttu-id="906ad-138">[Egyéni összetevők létrehozása a virtuális géphez](devtest-lab-artifact-author.md).</span><span class="sxs-lookup"><span data-stu-id="906ad-138">[Create custom artifacts for your VMs](devtest-lab-artifact-author.md).</span></span>
* <span data-ttu-id="906ad-139">[Adja hozzá a virtuális gép és az összetevők tooa labor](https://azure.microsoft.com/resources/videos/how-to-create-vms-with-artifacts-in-a-devtest-lab/).</span><span class="sxs-lookup"><span data-stu-id="906ad-139">[Add a VM with artifacts tooa lab](https://azure.microsoft.com/resources/videos/how-to-create-vms-with-artifacts-in-a-devtest-lab/).</span></span>

