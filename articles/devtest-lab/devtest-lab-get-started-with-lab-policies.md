---
title: "aaaManage alapvető tesztlabor házirendek az Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Megtudhatja, hogyan tooset néhány hello alapvető házirendek (beállítások) egy, amikor a DevTest Labs szolgáltatásban"
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
ms.openlocfilehash: 792c0d19cfe73964be9c03d3de7751a868fd86e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-basic-policies-for-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="99b9c-103">Az Azure DevTest Labs szolgáltatásban a labor alapvető házirendjeinek kezelése</span><span class="sxs-lookup"><span data-stu-id="99b9c-103">Manage basic policies for a lab in Azure DevTest Labs</span></span>

<span data-ttu-id="99b9c-104">Az Azure DevTest Labs lehetővé teszi toocontrol költség, és a fejlesztőlaborokban lévő pazarlás minimalizálásához (beállítások) házirendek kezelése által az egyes labor.</span><span class="sxs-lookup"><span data-stu-id="99b9c-104">Azure DevTest Labs enables you toocontrol cost and minimize waste in your labs by managing policies (settings) for each lab.</span></span> <span data-ttu-id="99b9c-105">Ebben a cikkben megkezdése házirendek hogyan tooset két hello legfontosabb házirendek - korlátozása hello száma tanulás által létrehozott vagy egyetlen felhasználói fiókot, és konfigurálása automatikus rendszerleállítást által igényelt virtuális gépek (VM).</span><span class="sxs-lookup"><span data-stu-id="99b9c-105">In this article, you get started with policies by learning how tooset two of hello most critical policies - limiting hello number of virtual machines (VM) that can be created or claimed by a single user, and configuring auto-shutdown.</span></span> <span data-ttu-id="99b9c-106">tooview hogyan tooset minden tesztkörnyezeti házirend hello cikke [labor házirendeket definiálhat az Azure DevTest Labs](devtest-lab-set-lab-policy.md).</span><span class="sxs-lookup"><span data-stu-id="99b9c-106">tooview how tooset every lab policy, see hello article, [Define lab policies in Azure DevTest Labs](devtest-lab-set-lab-policy.md).</span></span>  

## <a name="accessing-a-labs-policies-in-azure-devtest-labs"></a><span data-ttu-id="99b9c-107">Egy tesztlabor házirendek az Azure DevTest Labs elérése</span><span class="sxs-lookup"><span data-stu-id="99b9c-107">Accessing a lab's policies in Azure DevTest Labs</span></span>
<span data-ttu-id="99b9c-108">hello következő lépések végigvezetik a Azure DevTest Labs szolgáltatásban labor házirendek beállítása:</span><span class="sxs-lookup"><span data-stu-id="99b9c-108">hello following steps guide you through setting up policies for a lab in Azure DevTest Labs:</span></span>

<span data-ttu-id="99b9c-109">egy tesztlabor tooview (és módosítása) hello szabályzatok kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="99b9c-109">tooview (and change) hello policies for a lab, follow these steps:</span></span>

1. <span data-ttu-id="99b9c-110">Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="99b9c-110">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="99b9c-111">Válassza ki **további szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.</span><span class="sxs-lookup"><span data-stu-id="99b9c-111">Select **More services**, and then select **DevTest Labs** from hello list.</span></span>

1. <span data-ttu-id="99b9c-112">Labs hello listában jelölje ki hello kívánt labor.</span><span class="sxs-lookup"><span data-stu-id="99b9c-112">From hello list of labs, select hello desired lab.</span></span>   

1. <span data-ttu-id="99b9c-113">Válassza ki **konfigurációs és házirendek**.</span><span class="sxs-lookup"><span data-stu-id="99b9c-113">Select **Configuration and policies**.</span></span>

    ![Szabályzat-beállítások panelen](./media/devtest-lab-set-lab-policy/policies-menu.png)

1. <span data-ttu-id="99b9c-115">Hello **konfigurációs és házirendek** panel megadható beállítások menüt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="99b9c-115">hello **Configuration and policies** blade contains a menu of settings that you can specify.</span></span> <span data-ttu-id="99b9c-116">Ez a cikk foglalkozik csak hello beállításainak **virtuális gépek száma felhasználónként** és **automatikus leállítási**.</span><span class="sxs-lookup"><span data-stu-id="99b9c-116">This article covers only hello settings for **Virtual machines per user** and **Auto-shutdown**.</span></span> <span data-ttu-id="99b9c-117">toolearn fennmaradó beállításokat, hello kapcsolatban lásd: [egy Azure DevTest Labs szolgáltatásban található, amikor az összes házirend kezeléséhez](./devtest-lab-set-lab-policy.md).</span><span class="sxs-lookup"><span data-stu-id="99b9c-117">toolearn about hello remaining settings, see [Manage all policies for a lab in Azure DevTest Labs](./devtest-lab-set-lab-policy.md).</span></span> 
   
## <a name="set-virtual-machines-per-user"></a><span data-ttu-id="99b9c-118">Beállítása virtuális gépek száma felhasználónként</span><span class="sxs-lookup"><span data-stu-id="99b9c-118">Set virtual machines per user</span></span>
<span data-ttu-id="99b9c-119">a házirend hello **virtuális gépek száma felhasználónként** lehetővé teszi a toospecify hello maximális számát egy adott felhasználó által létrehozott virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="99b9c-119">hello policy for **Virtual machines per user** allows you toospecify hello maximum number of VMs that can be created by an individual user.</span></span> <span data-ttu-id="99b9c-120">Akkor a toocreate vagy a jogcím egy virtuális gép hello megadott felhasználói korlátot teljesülésekor, hibaüzenet jelzi, hogy virtuális gép nem hozható létre /, hello.</span><span class="sxs-lookup"><span data-stu-id="99b9c-120">If a user attempts toocreate or claim a VM when hello user limit has been met, an error message indicates that hello VM cannot be created/claimed.</span></span> 

1. <span data-ttu-id="99b9c-121">A hello labor **konfigurációs és házirendek** menüjében válassza **virtuális gépek száma felhasználónként**.</span><span class="sxs-lookup"><span data-stu-id="99b9c-121">On hello lab's **Configuration and policies** menu, select **Virtual machines per user**.</span></span>
   
    ![Virtuális gépek száma felhasználónként](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. <span data-ttu-id="99b9c-123">Válassza ki **Igen** toolimit hello virtuális gépek száma felhasználónként.</span><span class="sxs-lookup"><span data-stu-id="99b9c-123">Select **Yes** toolimit hello number of VMs per user.</span></span> <span data-ttu-id="99b9c-124">Ha nem szeretné, hogy toolimit hello virtuális gépek száma felhasználónként, válassza ki a **nem**.</span><span class="sxs-lookup"><span data-stu-id="99b9c-124">If you do not want toolimit hello number of VMs per user, select **No**.</span></span> <span data-ttu-id="99b9c-125">Ha **Igen**, adjon meg egy numerikus érték, amely hello létrehozott vagy a felhasználó által igényelt virtuális gépek maximális számát.</span><span class="sxs-lookup"><span data-stu-id="99b9c-125">If you select **Yes**, enter a numeric value indicating hello maximum number of VMs that can be created or claimed by a user.</span></span> 

1. <span data-ttu-id="99b9c-126">Válassza ki **Igen** SSD (SSD lemezt) használó virtuális gépek toolimit hello száma.</span><span class="sxs-lookup"><span data-stu-id="99b9c-126">Select **Yes** toolimit hello number of VMs that can use SSD (solid-state disk).</span></span> <span data-ttu-id="99b9c-127">Ha nem szeretné, hogy a virtuális gépek által használható SSD toolimit hello száma, válassza ki a **nem**.</span><span class="sxs-lookup"><span data-stu-id="99b9c-127">If you do not want toolimit hello number of VMs that can use SSD, select **No**.</span></span> <span data-ttu-id="99b9c-128">Ha **Igen**, adjon meg egy értéket hello SSD segítségével hozhatók létre virtuális gépek maximális számát.</span><span class="sxs-lookup"><span data-stu-id="99b9c-128">If you select **Yes**, enter a value indicating hello maximum number of VMs that can be created using SSD.</span></span> 

1. <span data-ttu-id="99b9c-129">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="99b9c-129">Select **Save**.</span></span>

## <a name="set-auto-shutdown"></a><span data-ttu-id="99b9c-130">Készlet automatikus rendszerleállítást</span><span class="sxs-lookup"><span data-stu-id="99b9c-130">Set auto-shutdown</span></span>
<span data-ttu-id="99b9c-131">hello automatikus leállítási házirendet lehetővé teszik, hogy a labor virtuális gépek leállítása toospecify hello idő hulladék toominimize tesztkörnyezet segítségével.</span><span class="sxs-lookup"><span data-stu-id="99b9c-131">hello auto-shutdown policy helps toominimize lab waste by allowing you toospecify hello time that this lab's VMs shut down.</span></span>

1. <span data-ttu-id="99b9c-132">A hello labor **konfigurációs és házirendek** panelen válassza **automatikus leállítási**.</span><span class="sxs-lookup"><span data-stu-id="99b9c-132">On hello lab's **Configuration and policies** blade, select **Auto-shutdown**.</span></span>
   
    ![Automatikus leállítási](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. <span data-ttu-id="99b9c-134">Válassza ki **a** tooenable ezt a házirendet, és **ki** toodisable azt.</span><span class="sxs-lookup"><span data-stu-id="99b9c-134">Select **On** tooenable this policy, and **Off** toodisable it.</span></span>

1. <span data-ttu-id="99b9c-135">Ha engedélyezi ezt a házirendet, adja meg a hello idő (és időzóna) tooshut minden virtuális gép le hello aktuális tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="99b9c-135">If you enable this policy, specify hello time (and time zone) tooshut down all VMs in hello current lab.</span></span>

1. <span data-ttu-id="99b9c-136">Adja meg **Igen** vagy **nem** hello beállítás toosend egy értesítési 15 perccel korábbi toohello megadva automatikus leállítási ideje.</span><span class="sxs-lookup"><span data-stu-id="99b9c-136">Specify **Yes** or **No** for hello option toosend a notification 15 minutes prior toohello specified auto-shutdown time.</span></span> <span data-ttu-id="99b9c-137">Ha megad **Igen**, írja be a webhook URL-cím végpont tooreceive hello értesítést.</span><span class="sxs-lookup"><span data-stu-id="99b9c-137">If you specify **Yes**, enter a webhook URL endpoint tooreceive hello notification.</span></span> <span data-ttu-id="99b9c-138">További információ a webhookok: [webhook vagy API Azure-függvény létrehozása](../azure-functions/functions-create-a-web-hook-or-api-function.md).</span><span class="sxs-lookup"><span data-stu-id="99b9c-138">For more information about webhooks, see [Create a webhook or API Azure Function](../azure-functions/functions-create-a-web-hook-or-api-function.md).</span></span> 

1. <span data-ttu-id="99b9c-139">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="99b9c-139">Select **Save**.</span></span>

    <span data-ttu-id="99b9c-140">Alapértelmezés szerint egyszer engedélyezve van ez a házirend érvényes tooall virtuális gépek hello aktuális tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="99b9c-140">By default, once enabled, this policy applies tooall VMs in hello current lab.</span></span> <span data-ttu-id="99b9c-141">tooremove ezt a beállítást egy adott virtuális gépről, nyissa meg a hello VM panelt, és módosítsa a **automatikus leállítási** beállítás</span><span class="sxs-lookup"><span data-stu-id="99b9c-141">tooremove this setting from a specific VM, open hello VM's blade and change its **Auto-shutdown** setting</span></span> 

## <a name="set-auto-start"></a><span data-ttu-id="99b9c-142">Készlet automatikus indítás</span><span class="sxs-lookup"><span data-stu-id="99b9c-142">Set auto-start</span></span>
<span data-ttu-id="99b9c-143">hello automatikusan elinduló házirend lehetővé teszi toospecify amikor hello aktuális tesztkörnyezetben lévő virtuális gépek hello el kell indítani.</span><span class="sxs-lookup"><span data-stu-id="99b9c-143">hello auto-start policy allows you toospecify when hello VMs in hello current lab should be started.</span></span>  

1. <span data-ttu-id="99b9c-144">A hello labor **konfigurációs és házirendek** panelen válassza **automatikusan elinduló**.</span><span class="sxs-lookup"><span data-stu-id="99b9c-144">On hello lab's **Configuration and policies** blade, select **Auto-start**.</span></span>
   
    ![Automatikus indítás](./media/devtest-lab-set-lab-policy/auto-start.png)

2. <span data-ttu-id="99b9c-146">Válassza ki **a** tooenable ezt a házirendet, és **ki** toodisable azt.</span><span class="sxs-lookup"><span data-stu-id="99b9c-146">Select **On** tooenable this policy, and **Off** toodisable it.</span></span>

3. <span data-ttu-id="99b9c-147">Ha engedélyezi ezt a házirendet, adja meg a hello ütemezett kezdési ideje, időzóna és hello napot hello mely hello idő vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="99b9c-147">If you enable this policy, specify hello scheduled start time, time zone, and hello days of hello week for which hello time applies.</span></span> 

4. <span data-ttu-id="99b9c-148">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="99b9c-148">Select **Save**.</span></span>

    <span data-ttu-id="99b9c-149">Engedélyezve van, ez a házirend nincs automatikusan alkalmazott tooany virtuális gépek hello aktuális tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="99b9c-149">Once enabled, this policy is not automatically applied tooany VMs in hello current lab.</span></span> <span data-ttu-id="99b9c-150">tooapply Ez a beállítás tooa speciális virtuális Gépet, a nyitott hello VM panel és a módosítás a **automatikusan elinduló** beállítás</span><span class="sxs-lookup"><span data-stu-id="99b9c-150">tooapply this setting tooa specific VM, open hello VM's blade and change its **Auto-start** setting</span></span> 

## <a name="next-steps"></a><span data-ttu-id="99b9c-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="99b9c-151">Next steps</span></span>

- <span data-ttu-id="99b9c-152">[Labor házirendeket definiálhat az Azure DevTest Labs](devtest-lab-set-lab-policy.md) -megtudhatja, hogyan toomodify egyéb tesztkörnyezeti házirendek</span><span class="sxs-lookup"><span data-stu-id="99b9c-152">[Define lab policies in Azure DevTest Labs](devtest-lab-set-lab-policy.md) - Learn how toomodify other lab policies</span></span> 
