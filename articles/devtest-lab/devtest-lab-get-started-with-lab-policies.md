---
title: "Az Azure DevTest Labs alapvető tesztlabor-házirendek kezeléséhez |} Microsoft Docs"
description: "Útmutató az alapvető házirendeket (beállítások) labor némelyike a DevTest Labs szolgáltatásban"
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
ms.date: 06/29/2017
ms.author: tarcher
ms.openlocfilehash: ed35d081b191ec41ed9e5970515057a4715c0d59
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-basic-policies-for-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="81622-103">Az Azure DevTest Labs szolgáltatásban a labor alapvető házirendjeinek kezelése</span><span class="sxs-lookup"><span data-stu-id="81622-103">Manage basic policies for a lab in Azure DevTest Labs</span></span>

<span data-ttu-id="81622-104">Azure DevTest Labs segítségével a labs a pazarlás minimalizálásához által az egyes labor (beállítások) házirendek kezelése és költségek szabályozásához.</span><span class="sxs-lookup"><span data-stu-id="81622-104">Azure DevTest Labs enables you to control cost and minimize waste in your labs by managing policies (settings) for each lab.</span></span> <span data-ttu-id="81622-105">Ebben a cikkben megkezdése házirendek beállítása két legfontosabb házirendek hogyan - virtuális gépek (VM) létrehozott vagy egy felhasználó által igényelt számának korlátozása, és az automatikus rendszerleállítást konfigurálásával.</span><span class="sxs-lookup"><span data-stu-id="81622-105">In this article, you get started with policies by learning how to set two of the most critical policies - limiting the number of virtual machines (VM) that can be created or claimed by a single user, and configuring auto-shutdown.</span></span> <span data-ttu-id="81622-106">Minden tesztkörnyezeti házirend tájékoztatásért tekintse meg a cikk megtekintéséhez [labor házirendeket definiálhat az Azure DevTest Labs](devtest-lab-set-lab-policy.md).</span><span class="sxs-lookup"><span data-stu-id="81622-106">To view how to set every lab policy, see the article, [Define lab policies in Azure DevTest Labs](devtest-lab-set-lab-policy.md).</span></span>  

## <a name="accessing-a-labs-policies-in-azure-devtest-labs"></a><span data-ttu-id="81622-107">Egy tesztlabor házirendek az Azure DevTest Labs elérése</span><span class="sxs-lookup"><span data-stu-id="81622-107">Accessing a lab's policies in Azure DevTest Labs</span></span>
<span data-ttu-id="81622-108">A következő lépések végigvezetik a Azure DevTest Labs szolgáltatásban labor házirendek beállítása:</span><span class="sxs-lookup"><span data-stu-id="81622-108">The following steps guide you through setting up policies for a lab in Azure DevTest Labs:</span></span>

<span data-ttu-id="81622-109">Megtekintése (és módosítása) a házirendek egy tesztkörnyezetet, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="81622-109">To view (and change) the policies for a lab, follow these steps:</span></span>

1. <span data-ttu-id="81622-110">Jelentkezzen be az [Azure Portalra](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="81622-110">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="81622-111">Válassza a **További szolgáltatások**, majd a **DevTest Labs** elemet a listából.</span><span class="sxs-lookup"><span data-stu-id="81622-111">Select **More services**, and then select **DevTest Labs** from the list.</span></span>

1. <span data-ttu-id="81622-112">Válassza ki a kívánt labor labs listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="81622-112">From the list of labs, select the desired lab.</span></span>   

1. <span data-ttu-id="81622-113">Válassza ki **konfigurációs és házirendek**.</span><span class="sxs-lookup"><span data-stu-id="81622-113">Select **Configuration and policies**.</span></span>

    ![Szabályzat-beállítások panelen](./media/devtest-lab-set-lab-policy/policies-menu.png)

1. <span data-ttu-id="81622-115">A **konfigurációs és házirendek** panel megadható beállítások menüt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="81622-115">The **Configuration and policies** blade contains a menu of settings that you can specify.</span></span> <span data-ttu-id="81622-116">Ez a cikk foglalkozik beállításait csak **virtuális gépek száma felhasználónként** és **automatikus leállítási**.</span><span class="sxs-lookup"><span data-stu-id="81622-116">This article covers only the settings for **Virtual machines per user** and **Auto-shutdown**.</span></span> <span data-ttu-id="81622-117">A fennmaradó beállításaival kapcsolatos további tudnivalókért lásd: [egy Azure DevTest Labs szolgáltatásban található, amikor az összes házirend kezeléséhez](./devtest-lab-set-lab-policy.md).</span><span class="sxs-lookup"><span data-stu-id="81622-117">To learn about the remaining settings, see [Manage all policies for a lab in Azure DevTest Labs](./devtest-lab-set-lab-policy.md).</span></span> 
   
## <a name="set-virtual-machines-per-user"></a><span data-ttu-id="81622-118">Beállítása virtuális gépek száma felhasználónként</span><span class="sxs-lookup"><span data-stu-id="81622-118">Set virtual machines per user</span></span>
<span data-ttu-id="81622-119">A házirend **virtuális gépek száma felhasználónként** megadhatja az egyes felhasználók által létrehozott virtuális gépek maximális számát.</span><span class="sxs-lookup"><span data-stu-id="81622-119">The policy for **Virtual machines per user** allows you to specify the maximum number of VMs that can be created by an individual user.</span></span> <span data-ttu-id="81622-120">Ha egy felhasználó megpróbál létrehozásához vagy a virtuális gépek jogcímek a megadott felhasználói korlátot teljesülésekor, egy hibaüzenet arra utalnak, hogy a virtuális gép nem létrehozott/igényt.</span><span class="sxs-lookup"><span data-stu-id="81622-120">If a user attempts to create or claim a VM when the user limit has been met, an error message indicates that the VM cannot be created/claimed.</span></span> 

1. <span data-ttu-id="81622-121">A tesztlabor a **konfigurációs és házirendek** menüjében válassza **virtuális gépek száma felhasználónként**.</span><span class="sxs-lookup"><span data-stu-id="81622-121">On the lab's **Configuration and policies** menu, select **Virtual machines per user**.</span></span>
   
    ![Virtuális gépek száma felhasználónként](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. <span data-ttu-id="81622-123">Válassza ki **Igen** korlátozza a virtuális gépek száma felhasználónként.</span><span class="sxs-lookup"><span data-stu-id="81622-123">Select **Yes** to limit the number of VMs per user.</span></span> <span data-ttu-id="81622-124">Ha nem szeretné, hogy a virtuális gépek száma felhasználónként korlátozni, válassza ki a **nem**.</span><span class="sxs-lookup"><span data-stu-id="81622-124">If you do not want to limit the number of VMs per user, select **No**.</span></span> <span data-ttu-id="81622-125">Ha **Igen**, adjon meg egy numerikus érték, amely a létrehozott vagy a felhasználó által igényelt virtuális gépek maximális számát.</span><span class="sxs-lookup"><span data-stu-id="81622-125">If you select **Yes**, enter a numeric value indicating the maximum number of VMs that can be created or claimed by a user.</span></span> 

1. <span data-ttu-id="81622-126">Válassza ki **Igen** SSD (SSD lemezt) használó virtuális gépek számát.</span><span class="sxs-lookup"><span data-stu-id="81622-126">Select **Yes** to limit the number of VMs that can use SSD (solid-state disk).</span></span> <span data-ttu-id="81622-127">Ha nem szeretné, hogy a virtuális gépek, amelyek is SSD használni, válassza a számának korlátozásához **nem**.</span><span class="sxs-lookup"><span data-stu-id="81622-127">If you do not want to limit the number of VMs that can use SSD, select **No**.</span></span> <span data-ttu-id="81622-128">Ha **Igen**, adjon meg egy értéket, amely SSD segítségével hozhatók létre virtuális gépek maximális száma.</span><span class="sxs-lookup"><span data-stu-id="81622-128">If you select **Yes**, enter a value indicating the maximum number of VMs that can be created using SSD.</span></span> 

1. <span data-ttu-id="81622-129">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="81622-129">Select **Save**.</span></span>

## <a name="set-auto-shutdown"></a><span data-ttu-id="81622-130">Készlet automatikus rendszerleállítást</span><span class="sxs-lookup"><span data-stu-id="81622-130">Set auto-shutdown</span></span>
<span data-ttu-id="81622-131">Az automatikus rendszerleállítást a szabályzattal meggyőződhetnek labor pazarlás minimalizálásához azáltal, hogy adja meg a labor virtuális gépek leállítása idejét.</span><span class="sxs-lookup"><span data-stu-id="81622-131">The auto-shutdown policy helps to minimize lab waste by allowing you to specify the time that this lab's VMs shut down.</span></span>

1. <span data-ttu-id="81622-132">A tesztlabor a **konfigurációs és házirendek** panelen válassza **automatikus leállítási**.</span><span class="sxs-lookup"><span data-stu-id="81622-132">On the lab's **Configuration and policies** blade, select **Auto-shutdown**.</span></span>
   
    ![Automatikus leállítási](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. <span data-ttu-id="81622-134">Válassza ki **a** ahhoz, hogy ezt a házirendet, és **ki** le kell tiltani.</span><span class="sxs-lookup"><span data-stu-id="81622-134">Select **On** to enable this policy, and **Off** to disable it.</span></span>

1. <span data-ttu-id="81622-135">Ha engedélyezi ezt a házirendet, adja meg az időt (és időzóna) leállítása az összes virtuális gép az aktuális tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="81622-135">If you enable this policy, specify the time (and time zone) to shut down all VMs in the current lab.</span></span>

1. <span data-ttu-id="81622-136">Adja meg **Igen** vagy **nem** beállítás értesítés küldése előtt a megadott automatikus rendszerleállítást idő 15 perc.</span><span class="sxs-lookup"><span data-stu-id="81622-136">Specify **Yes** or **No** for the option to send a notification 15 minutes prior to the specified auto-shutdown time.</span></span> <span data-ttu-id="81622-137">Ha megad **Igen**, írja be a webhook URL-végpontjának az értesítések fogadásához.</span><span class="sxs-lookup"><span data-stu-id="81622-137">If you specify **Yes**, enter a webhook URL endpoint to receive the notification.</span></span> <span data-ttu-id="81622-138">További információ a webhookok: [webhook vagy API Azure-függvény létrehozása](../azure-functions/functions-create-a-web-hook-or-api-function.md).</span><span class="sxs-lookup"><span data-stu-id="81622-138">For more information about webhooks, see [Create a webhook or API Azure Function](../azure-functions/functions-create-a-web-hook-or-api-function.md).</span></span> 

1. <span data-ttu-id="81622-139">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="81622-139">Select **Save**.</span></span>

    <span data-ttu-id="81622-140">Alapértelmezés szerint engedélyezve van, ez a házirend vonatkozik az összes virtuális gépen az aktuális tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="81622-140">By default, once enabled, this policy applies to all VMs in the current lab.</span></span> <span data-ttu-id="81622-141">Távolítsa el ezt a beállítást egy adott virtuális géptől, nyissa meg a virtuális gép panelt, és módosítsa a **automatikus leállítási** beállítás</span><span class="sxs-lookup"><span data-stu-id="81622-141">To remove this setting from a specific VM, open the VM's blade and change its **Auto-shutdown** setting</span></span> 

## <a name="set-auto-start"></a><span data-ttu-id="81622-142">Készlet automatikus indítás</span><span class="sxs-lookup"><span data-stu-id="81622-142">Set auto-start</span></span>
<span data-ttu-id="81622-143">Az automatikus indítási házirend lehetővé teszi, hogy adja meg, amikor el kell indítani a virtuális gépeket az aktuális tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="81622-143">The auto-start policy allows you to specify when the VMs in the current lab should be started.</span></span>  

1. <span data-ttu-id="81622-144">A tesztlabor a **konfigurációs és házirendek** panelen válassza **automatikusan elinduló**.</span><span class="sxs-lookup"><span data-stu-id="81622-144">On the lab's **Configuration and policies** blade, select **Auto-start**.</span></span>
   
    ![Automatikus indítás](./media/devtest-lab-set-lab-policy/auto-start.png)

2. <span data-ttu-id="81622-146">Válassza ki **a** ahhoz, hogy ezt a házirendet, és **ki** le kell tiltani.</span><span class="sxs-lookup"><span data-stu-id="81622-146">Select **On** to enable this policy, and **Off** to disable it.</span></span>

3. <span data-ttu-id="81622-147">Ha engedélyezi ezt a házirendet, adja meg az ütemezett kezdési időpontban, időzóna és a napokat a hét, amelyre a idő vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="81622-147">If you enable this policy, specify the scheduled start time, time zone, and the days of the week for which the time applies.</span></span> 

4. <span data-ttu-id="81622-148">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="81622-148">Select **Save**.</span></span>

    <span data-ttu-id="81622-149">Az engedélyezést követően a házirend nem automatikusan érvényben van a virtuális gépek az aktuális tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="81622-149">Once enabled, this policy is not automatically applied to any VMs in the current lab.</span></span> <span data-ttu-id="81622-150">Alkalmazza ezt a beállítást egy adott virtuális géphez, nyissa meg a virtuális gép panelt, és módosítsa a **automatikusan elinduló** beállítás</span><span class="sxs-lookup"><span data-stu-id="81622-150">To apply this setting to a specific VM, open the VM's blade and change its **Auto-start** setting</span></span> 

## <a name="next-steps"></a><span data-ttu-id="81622-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="81622-151">Next steps</span></span>

- <span data-ttu-id="81622-152">[Labor házirendeket definiálhat az Azure DevTest Labs](devtest-lab-set-lab-policy.md) -megtudhatja, hogyan egyéb tesztkörnyezeti házirendek módosítása</span><span class="sxs-lookup"><span data-stu-id="81622-152">[Define lab policies in Azure DevTest Labs](devtest-lab-set-lab-policy.md) - Learn how to modify other lab policies</span></span> 
