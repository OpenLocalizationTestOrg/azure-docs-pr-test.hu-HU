---
title: "Labor létrehozása az Azure DevTest Labs szolgáltatásban | Microsoft Docs"
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
ms.openlocfilehash: 265a968f902f53c7561c8c7e937f8eacfdb37167
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="653d0-103">Labor létrehozása az Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="653d0-103">Create a lab in Azure DevTest Labs</span></span>
<span data-ttu-id="653d0-104">Az Azure DevTest Labs szolgáltatásban létrehozott tesztkörnyezet erőforráscsoportokat, például virtuális gépeket (VM-eket) magában foglaló infrastruktúra, amely korlátok és kvóták meghatározásával segíti ezen erőforráscsoportok jobb kezelését.</span><span class="sxs-lookup"><span data-stu-id="653d0-104">A lab in Azure DevTest Labs is the infrastructure that encompasses a group of resources, such as Virtual Machines (VMs), that lets you better manage those resources by specifying limits and quotas.</span></span> <span data-ttu-id="653d0-105">Ez a cikk végigvezeti a tesztkörnyezet Azure Portalon való létrehozásának folyamatán.</span><span class="sxs-lookup"><span data-stu-id="653d0-105">This article walks you through the process of creating a lab using the Azure portal.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="653d0-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="653d0-106">Prerequisites</span></span>
<span data-ttu-id="653d0-107">Labor létrehozásához a következőkre van szüksége:</span><span class="sxs-lookup"><span data-stu-id="653d0-107">To create a lab, you need:</span></span>

* <span data-ttu-id="653d0-108">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="653d0-108">An Azure subscription.</span></span> <span data-ttu-id="653d0-109">Az Azure megvásárlási lehetőségeinek ismertetése: [Az Azure megvásárlása](https://azure.microsoft.com/pricing/purchase-options/) vagy [Ingyenes egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="653d0-109">To learn about Azure purchase options, see [How to buy Azure](https://azure.microsoft.com/pricing/purchase-options/) or [Free one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="653d0-110">Az előfizetés tulajdonosának kell lennie a labor létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="653d0-110">You must be the owner of the subscription to create the lab.</span></span>

## <a name="steps-to-create-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="653d0-111">A labor létrehozásának lépései az Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="653d0-111">Steps to create a lab in Azure DevTest Labs</span></span>
<span data-ttu-id="653d0-112">A következő lépések bemutatják, hogyan használhatja az Azure Portalt labor létrehozására az Azure DevTest Labs szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="653d0-112">The following steps illustrate how to use the Azure portal to create a lab in Azure DevTest Labs.</span></span> 

1. <span data-ttu-id="653d0-113">Jelentkezzen be az [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="653d0-113">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
1. <span data-ttu-id="653d0-114">A bal oldali főmenüben kattintson a lista alján látható **További szolgáltatások** elemre.</span><span class="sxs-lookup"><span data-stu-id="653d0-114">From the main menu on the left side, select **More Services** (at the bottom of the list).</span></span>

    ![További szolgáltatások menüpont](./media/devtest-lab-create-lab/more-services-menu-option.png)

1. <span data-ttu-id="653d0-116">Válassza ki a **DevTest Labs** lehetőséget az elérhető szolgáltatások listájából.</span><span class="sxs-lookup"><span data-stu-id="653d0-116">From the list of available services, **DevTest Labs**.</span></span>
1. <span data-ttu-id="653d0-117">A **DevTest Labs** panelen válassza a **Hozzáadás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="653d0-117">On the **DevTest Labs** blade, select **Add**.</span></span>
   
    ![Labor hozzáadása](./media/devtest-lab-create-lab/add-lab-button.png)

1. <span data-ttu-id="653d0-119">A **Fejlesztési és tesztelési labor létrehozása** panelen:</span><span class="sxs-lookup"><span data-stu-id="653d0-119">On the **Create a DevTest Lab** blade:</span></span>
   
    1. <span data-ttu-id="653d0-120">Adja meg a **Labor nevét**.</span><span class="sxs-lookup"><span data-stu-id="653d0-120">Enter a **Lab Name** for the new lab.</span></span>
    2. <span data-ttu-id="653d0-121">Válassza ki a laborhoz társítani kívánt **Előfizetést**.</span><span class="sxs-lookup"><span data-stu-id="653d0-121">Select the **Subscription** to associate with the lab.</span></span>
    3. <span data-ttu-id="653d0-122">Válassza ki a labor tárolásának **Helyét**.</span><span class="sxs-lookup"><span data-stu-id="653d0-122">Select a **Location** in which to store the lab.</span></span>
    4. <span data-ttu-id="653d0-123">Válassza az **Automatikus leállítás** elemet annak megadásához, hogy engedélyezi-e az automatikus leállítást a labor összes virtuális gépénél (valamint az automatikus leállítás paramétereit is megadhatja).</span><span class="sxs-lookup"><span data-stu-id="653d0-123">Select **Auto-shutdown** to specify if you want to enable - and define the parameters for - the automatic shutting down of all the lab's VMs.</span></span> <span data-ttu-id="653d0-124">Az automatikus rendszerleállítási funkció elsősorban költségkímélő szolgáltatás, amelynek segítségével megadhatja a virtuális gép automatikus leállásának időpontját.</span><span class="sxs-lookup"><span data-stu-id="653d0-124">The auto-shutdown feature is mainly a cost-saving feature whereby you can specify when you want the VM to automatically be shut down.</span></span> <span data-ttu-id="653d0-125">A tesztkörnyezet létrehozása után módosíthatja az automatikus rendszerleállítás beállításait, ha követi az [Manage all policies for a lab in Azure DevTest Labs](./devtest-lab-set-lab-policy.md#set-auto-shutdown) (Azure DevTest Labs szolgáltatásban létrehozott tesztkörnyezet szabályzatainak kezelése) témakörben leírt lépéseket.</span><span class="sxs-lookup"><span data-stu-id="653d0-125">You can change auto-shutdown settings after creating the lab by following the steps outlined in the article, [Manage all policies for a lab in Azure DevTest Labs](./devtest-lab-set-lab-policy.md#set-auto-shutdown).</span></span>
    5. <span data-ttu-id="653d0-126">Ha azt szeretné, hogy a labor parancsikonja megjelenjen a portál irányítópultján, válassza a **Rögzítés az irányítópulton** elemet.</span><span class="sxs-lookup"><span data-stu-id="653d0-126">Select **Pin to Dashboard** if you want a shortcut of the lab to appear on the portal dashboard.</span></span>
    6. <span data-ttu-id="653d0-127">Az **Automatizálási beállítások** lehetőséget választva elérheti az Azure Resource Manager-sablonokat a konfigurálás automatizálásához.</span><span class="sxs-lookup"><span data-stu-id="653d0-127">Select **Automation options** to get Azure Resource Manager templates for configuration automation.</span></span> 
    7. <span data-ttu-id="653d0-128">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="653d0-128">Select **Create**.</span></span> <span data-ttu-id="653d0-129">A **Létrehozás** gombra kattintva megjelenítheti a **DevTest Labs** panelt.</span><span class="sxs-lookup"><span data-stu-id="653d0-129">After selecting **Create**, the **DevTest Labs** blade displays.</span></span> <span data-ttu-id="653d0-130">A tesztkörnyezet létrehozásának folyamatát az **Értesítések** területen figyelheti.</span><span class="sxs-lookup"><span data-stu-id="653d0-130">You can monitor the status of the lab creation process by watching the **Notifications** area.</span></span> <span data-ttu-id="653d0-131">A művelet végeztével frissítse az oldalt, ekkor a tesztkörnyezetek listájában megjelenik az újonnan létrehozott tesztkörnyezet.</span><span class="sxs-lookup"><span data-stu-id="653d0-131">Once completed, refresh the page to see the newly created lab in the list of labs.</span></span>  
    
    ![Laborpanel létrehozása](./media/devtest-lab-create-lab/create-devtestlab-blade.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="653d0-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="653d0-133">Next steps</span></span>
<span data-ttu-id="653d0-134">A labor létrehozása után ezeket a lépéseket érdemes figyelembe venni:</span><span class="sxs-lookup"><span data-stu-id="653d0-134">Once you've created your lab, here are some next steps to consider:</span></span>

* <span data-ttu-id="653d0-135">[Biztonságos hozzáférés a laborokhoz](devtest-lab-add-devtest-user.md).</span><span class="sxs-lookup"><span data-stu-id="653d0-135">[Secure access to a lab](devtest-lab-add-devtest-user.md).</span></span>
* <span data-ttu-id="653d0-136">[Laborházirendek megadása](devtest-lab-set-lab-policy.md).</span><span class="sxs-lookup"><span data-stu-id="653d0-136">[Set lab policies](devtest-lab-set-lab-policy.md).</span></span>
* <span data-ttu-id="653d0-137">[Laborsablon létrehozása](devtest-lab-create-template.md).</span><span class="sxs-lookup"><span data-stu-id="653d0-137">[Create a lab template](devtest-lab-create-template.md).</span></span>
* <span data-ttu-id="653d0-138">[Egyéni összetevők létrehozása a virtuális géphez](devtest-lab-artifact-author.md).</span><span class="sxs-lookup"><span data-stu-id="653d0-138">[Create custom artifacts for your VMs](devtest-lab-artifact-author.md).</span></span>
* <span data-ttu-id="653d0-139">[Összetevőkkel rendelkező virtuális gép hozzáadása egy laborhoz](https://azure.microsoft.com/resources/videos/how-to-create-vms-with-artifacts-in-a-devtest-lab/).</span><span class="sxs-lookup"><span data-stu-id="653d0-139">[Add a VM with artifacts to a lab](https://azure.microsoft.com/resources/videos/how-to-create-vms-with-artifacts-in-a-devtest-lab/).</span></span>

