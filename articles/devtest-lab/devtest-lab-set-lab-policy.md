---
title: "Kezelheti a labor házirendeket a Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Megtudhatja, hogyan határozza meg a labor házirendek, például a Virtuálisgép-méretek, minden felhasználó és a leállítási automation maximális virtuális gépeket."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 7756aa64-49ca-45a0-9f90-0fd101c7be85
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: tarcher
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 328a4d893637d7150807855e118b485a2c3bbfc5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-all-policies-for-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="43476-103">Egy Azure DevTest Labs szolgáltatásban található, amikor az összes házirend kezeléséhez</span><span class="sxs-lookup"><span data-stu-id="43476-103">Manage all policies for a lab in Azure DevTest Labs</span></span>

<span data-ttu-id="43476-104">Az Azure DevTest Labs lehetővé teszi a költségek szabályozásához, és a fejlesztőlaborokban lévő pazarlás minimalizálásához (beállítások) házirendek kezelése által az egyes labor.</span><span class="sxs-lookup"><span data-stu-id="43476-104">Azure DevTest Labs lets you control cost and minimize waste in your labs by managing policies (settings) for each lab.</span></span> <span data-ttu-id="43476-105">Ez a cikk részletes részletesen ismerteti, hogyan minden házirend beállítása.</span><span class="sxs-lookup"><span data-stu-id="43476-105">This article explains in step-by-step detail how to set each policy.</span></span>  

## <a name="set-allowed-virtual-machine-sizes"></a><span data-ttu-id="43476-106">Virtuális gépek méretét engedélyezett beállítása</span><span class="sxs-lookup"><span data-stu-id="43476-106">Set allowed virtual machine sizes</span></span>
<span data-ttu-id="43476-107">Az engedélyezett Virtuálisgép-méretek beállításához a szabályzattal meggyőződhetnek labor pazarlás minimalizálásához engedélyezésével adhatja meg, melyik Virtuálisgép-méretek engedélyezettek a laborkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="43476-107">The policy for setting the allowed VM sizes helps to minimize lab waste by enabling you to specify which VM sizes are allowed in the lab.</span></span> <span data-ttu-id="43476-108">Ha ez a házirend aktív, csak Virtuálisgép-méretek a listáról a virtuális gépek létrehozásához használható.</span><span class="sxs-lookup"><span data-stu-id="43476-108">If this policy is activated, only VM sizes from this list can be used to create VMs.</span></span>

1. <span data-ttu-id="43476-109">A tesztlabor a **konfigurációs és házirendek** panelen válassza **engedélyezett virtuális gépek méretét**.</span><span class="sxs-lookup"><span data-stu-id="43476-109">On the lab's **Configuration and policies** blade, select **Allowed virtual machines sizes**.</span></span>
   
    ![Engedélyezett virtuális gépek méretét](./media/devtest-lab-set-lab-policy/allowed-vm-sizes.png)

1. <span data-ttu-id="43476-111">Válassza ki **a** ahhoz, hogy ezt a házirendet, és **ki** le kell tiltani.</span><span class="sxs-lookup"><span data-stu-id="43476-111">Select **On** to enable this policy, and **Off** to disable it.</span></span>

1. <span data-ttu-id="43476-112">Ha engedélyezi ezt a házirendet, válassza ki a tesztkörnyezetben hozható létre egy vagy több Virtuálisgép-méretek.</span><span class="sxs-lookup"><span data-stu-id="43476-112">If you enable this policy, select one or more VM sizes that can be created in your lab.</span></span>

1. <span data-ttu-id="43476-113">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="43476-113">Select **Save**.</span></span>

## <a name="set-virtual-machines-per-user"></a><span data-ttu-id="43476-114">Beállítása virtuális gépek száma felhasználónként</span><span class="sxs-lookup"><span data-stu-id="43476-114">Set virtual machines per user</span></span>
<span data-ttu-id="43476-115">A házirend **virtuális gépek száma felhasználónként** megadhatja az egyes felhasználók által létrehozott virtuális gépek maximális számát.</span><span class="sxs-lookup"><span data-stu-id="43476-115">The policy for **Virtual machines per user** allows you to specify the maximum number of VMs that can be created by an individual user.</span></span> <span data-ttu-id="43476-116">Ha egy felhasználó megpróbál létrehozásához vagy a virtuális gépek jogcímek a megadott felhasználói korlátot teljesülésekor, egy hibaüzenet arra utalnak, hogy a virtuális gép nem létrehozott/igényt.</span><span class="sxs-lookup"><span data-stu-id="43476-116">If a user attempts to create or claim a VM when the user limit has been met, an error message indicates that the VM cannot be created/claimed.</span></span> 

1. <span data-ttu-id="43476-117">A tesztlabor a **konfigurációs és házirendek** menüjében válassza **virtuális gépek száma felhasználónként**.</span><span class="sxs-lookup"><span data-stu-id="43476-117">On the lab's **Configuration and policies** menu, select **Virtual machines per user**.</span></span>
   
    ![Virtuális gépek száma felhasználónként](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. <span data-ttu-id="43476-119">Válassza ki **Igen** korlátozza a virtuális gépek száma felhasználónként.</span><span class="sxs-lookup"><span data-stu-id="43476-119">Select **Yes** to limit the number of VMs per user.</span></span> <span data-ttu-id="43476-120">Ha nem szeretné, hogy a virtuális gépek száma felhasználónként korlátozni, válassza ki a **nem**.</span><span class="sxs-lookup"><span data-stu-id="43476-120">If you do not want to limit the number of VMs per user, select **No**.</span></span> <span data-ttu-id="43476-121">Ha **Igen**, adjon meg egy numerikus érték, amely a létrehozott vagy a felhasználó által igényelt virtuális gépek maximális számát.</span><span class="sxs-lookup"><span data-stu-id="43476-121">If you select **Yes**, enter a numeric value indicating the maximum number of VMs that can be created or claimed by a user.</span></span> 

1. <span data-ttu-id="43476-122">Válassza ki **Igen** SSD (SSD lemezt) használó virtuális gépek számát.</span><span class="sxs-lookup"><span data-stu-id="43476-122">Select **Yes** to limit the number of VMs that can use SSD (solid-state disk).</span></span> <span data-ttu-id="43476-123">Ha nem szeretné, hogy a virtuális gépek, amelyek is SSD használni, válassza a számának korlátozásához **nem**.</span><span class="sxs-lookup"><span data-stu-id="43476-123">If you do not want to limit the number of VMs that can use SSD, select **No**.</span></span> <span data-ttu-id="43476-124">Ha **Igen**, adjon meg egy értéket, amely SSD segítségével hozhatók létre virtuális gépek maximális száma.</span><span class="sxs-lookup"><span data-stu-id="43476-124">If you select **Yes**, enter a value indicating the maximum number of VMs that can be created using SSD.</span></span> 

1. <span data-ttu-id="43476-125">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="43476-125">Select **Save**.</span></span>

## <a name="set-virtual-machines-per-lab"></a><span data-ttu-id="43476-126">Beállítása virtuális gépek száma tesztkörnyezetenként</span><span class="sxs-lookup"><span data-stu-id="43476-126">Set virtual machines per lab</span></span>
<span data-ttu-id="43476-127">A házirend **virtuális gépek száma tesztkörnyezetenként** megadhatja, hogy az aktuális tesztkörnyezetben hozható létre virtuális gépek maximális számát.</span><span class="sxs-lookup"><span data-stu-id="43476-127">The policy for **Virtual machines per lab** allows you to specify the maximum number of VMs that can be created for the current lab.</span></span> <span data-ttu-id="43476-128">Ha a felhasználó megkísérli a virtuális gép létrehozása a tesztkörnyezeti korlátot teljesülésekor, egy hibaüzenet azt jelzi, hogy a virtuális gép nem hozható létre.</span><span class="sxs-lookup"><span data-stu-id="43476-128">If a user attempts to create a VM when the lab limit has been met, an error message indicates that the VM cannot be created.</span></span> 

1. <span data-ttu-id="43476-129">A tesztlabor a **konfigurációs és házirendek** menü **virtuális gépek száma tesztkörnyezetenként**.</span><span class="sxs-lookup"><span data-stu-id="43476-129">On the lab's **Configuration and policies** menu, select **Virtual machines per lab**.</span></span>
   
    ![Virtuális gépek száma tesztkörnyezetenként](./media/devtest-lab-set-lab-policy/max-vms-per-lab.png)

1. <span data-ttu-id="43476-131">Válassza ki **Igen** korlátozza a virtuális gépek száma tesztkörnyezetenként számát.</span><span class="sxs-lookup"><span data-stu-id="43476-131">Select **Yes** to limit the number of VMs per lab.</span></span> <span data-ttu-id="43476-132">Ha nem szeretné, hogy a virtuális gépek száma tesztkörnyezetenként számát korlátozni, válassza ki a **nem**.</span><span class="sxs-lookup"><span data-stu-id="43476-132">If you do not want to limit the number of VMs per lab, select **No**.</span></span> <span data-ttu-id="43476-133">Ha **Igen**, adjon meg egy numerikus érték, amely a létrehozott vagy a felhasználó által igényelt virtuális gépek maximális számát.</span><span class="sxs-lookup"><span data-stu-id="43476-133">If you select **Yes**, enter a numeric value indicating the maximum number of VMs that can be created or claimed by a user.</span></span> 

1. <span data-ttu-id="43476-134">Válassza ki **Igen** SSD (SSD lemezt) használó virtuális gépek számát.</span><span class="sxs-lookup"><span data-stu-id="43476-134">Select **Yes** to limit the number of VMs that can use SSD (solid-state disk).</span></span> <span data-ttu-id="43476-135">Ha nem szeretné, hogy a virtuális gépek, amelyek is SSD használni, válassza a számának korlátozásához **nem**.</span><span class="sxs-lookup"><span data-stu-id="43476-135">If you do not want to limit the number of VMs that can use SSD, select **No**.</span></span> <span data-ttu-id="43476-136">Ha **Igen**, adjon meg egy értéket, amely SSD segítségével hozhatók létre virtuális gépek maximális száma.</span><span class="sxs-lookup"><span data-stu-id="43476-136">If you select **Yes**, enter a value indicating the maximum number of VMs that can be created using SSD.</span></span> 

1. <span data-ttu-id="43476-137">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="43476-137">Select **Save**.</span></span>

## <a name="set-auto-shutdown"></a><span data-ttu-id="43476-138">Készlet automatikus rendszerleállítást</span><span class="sxs-lookup"><span data-stu-id="43476-138">Set auto-shutdown</span></span>
<span data-ttu-id="43476-139">Az automatikus rendszerleállítást a szabályzattal meggyőződhetnek labor pazarlás minimalizálásához azáltal, hogy adja meg a labor virtuális gépek leállítása idejét.</span><span class="sxs-lookup"><span data-stu-id="43476-139">The auto-shutdown policy helps to minimize lab waste by allowing you to specify the time that this lab's VMs shut down.</span></span>

1. <span data-ttu-id="43476-140">A tesztlabor a **konfigurációs és házirendek** panelen válassza **automatikus leállítási**.</span><span class="sxs-lookup"><span data-stu-id="43476-140">On the lab's **Configuration and policies** blade, select **Auto-shutdown**.</span></span>
   
    ![Automatikus leállítási](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. <span data-ttu-id="43476-142">Válassza ki **a** ahhoz, hogy ezt a házirendet, és **ki** le kell tiltani.</span><span class="sxs-lookup"><span data-stu-id="43476-142">Select **On** to enable this policy, and **Off** to disable it.</span></span>

1. <span data-ttu-id="43476-143">Ha engedélyezi ezt a házirendet, adja meg az időt (és időzóna) leállítása az összes virtuális gép az aktuális tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="43476-143">If you enable this policy, specify the time (and time zone) to shut down all VMs in the current lab.</span></span>

1. <span data-ttu-id="43476-144">Adja meg **Igen** vagy **nem** beállítás értesítés küldése előtt a megadott automatikus rendszerleállítást idő 15 perc.</span><span class="sxs-lookup"><span data-stu-id="43476-144">Specify **Yes** or **No** for the option to send a notification 15 minutes prior to the specified auto-shutdown time.</span></span> <span data-ttu-id="43476-145">Ha megad **Igen**, írja be a webhook URL-végpontjának az értesítések fogadásához.</span><span class="sxs-lookup"><span data-stu-id="43476-145">If you specify **Yes**, enter a webhook URL endpoint to receive the notification.</span></span> <span data-ttu-id="43476-146">További információ a webhookok: [webhook vagy API Azure-függvény létrehozása](../azure-functions/functions-create-a-web-hook-or-api-function.md).</span><span class="sxs-lookup"><span data-stu-id="43476-146">For more information about webhooks, see [Create a webhook or API Azure Function](../azure-functions/functions-create-a-web-hook-or-api-function.md).</span></span> 

1. <span data-ttu-id="43476-147">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="43476-147">Select **Save**.</span></span>

    <span data-ttu-id="43476-148">Alapértelmezés szerint engedélyezve van, ez a házirend vonatkozik az összes virtuális gépen az aktuális tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="43476-148">By default, once enabled, this policy applies to all VMs in the current lab.</span></span> <span data-ttu-id="43476-149">Távolítsa el ezt a beállítást egy adott virtuális géptől, nyissa meg a virtuális gép panelt, és módosítsa a **automatikus leállítási** beállítás</span><span class="sxs-lookup"><span data-stu-id="43476-149">To remove this setting from a specific VM, open the VM's blade and change its **Auto-shutdown** setting</span></span> 

## <a name="set-auto-start"></a><span data-ttu-id="43476-150">Készlet automatikus indítás</span><span class="sxs-lookup"><span data-stu-id="43476-150">Set auto-start</span></span>
<span data-ttu-id="43476-151">Az automatikus indítási házirend lehetővé teszi, hogy adja meg, amikor el kell indítani a virtuális gépeket az aktuális tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="43476-151">The auto-start policy allows you to specify when the VMs in the current lab should be started.</span></span>  

1. <span data-ttu-id="43476-152">A tesztlabor a **konfigurációs és házirendek** panelen válassza **automatikusan elinduló**.</span><span class="sxs-lookup"><span data-stu-id="43476-152">On the lab's **Configuration and policies** blade, select **Auto-start**.</span></span>
   
    ![Automatikus indítás](./media/devtest-lab-set-lab-policy/auto-start.png)

2. <span data-ttu-id="43476-154">Válassza ki **a** ahhoz, hogy ezt a házirendet, és **ki** le kell tiltani.</span><span class="sxs-lookup"><span data-stu-id="43476-154">Select **On** to enable this policy, and **Off** to disable it.</span></span>

3. <span data-ttu-id="43476-155">Ha engedélyezi ezt a házirendet, adja meg az ütemezett kezdési időpontban, időzóna és a napokat a hét, amelyre a idő vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="43476-155">If you enable this policy, specify the scheduled start time, time zone, and the days of the week for which the time applies.</span></span> 

4. <span data-ttu-id="43476-156">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="43476-156">Select **Save**.</span></span>

    <span data-ttu-id="43476-157">Az engedélyezést követően a házirend nem automatikusan érvényben van a virtuális gépek az aktuális tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="43476-157">Once enabled, this policy is not automatically applied to any VMs in the current lab.</span></span> <span data-ttu-id="43476-158">Alkalmazza ezt a beállítást egy adott virtuális géphez, nyissa meg a virtuális gép panelt, és módosítsa a **automatikusan elinduló** beállítás</span><span class="sxs-lookup"><span data-stu-id="43476-158">To apply this setting to a specific VM, open the VM's blade and change its **Auto-start** setting</span></span> 

## <a name="set-expiration-date"></a><span data-ttu-id="43476-159">Lejárat dátuma</span><span class="sxs-lookup"><span data-stu-id="43476-159">Set expiration date</span></span>
<span data-ttu-id="43476-160">Megadhat egy lejárati dátum, [a virtuális gép létrehozása](devtest-lab-add-vm.md).</span><span class="sxs-lookup"><span data-stu-id="43476-160">You can set an expiration date when you [create the VM](devtest-lab-add-vm.md).</span></span> <span data-ttu-id="43476-161">A **speciális beállítások**, válassza ki a naptár ikonra, és adjon meg egy dátum, amelyen a virtuális gép automatikusan törölve lesznek.</span><span class="sxs-lookup"><span data-stu-id="43476-161">In **Advanced settings**, choose the calendar icon to specify a date on which the VM will be automatically deleted.</span></span>  <span data-ttu-id="43476-162">Alapértelmezés szerint a virtuális Gépet soha nem jár le.</span><span class="sxs-lookup"><span data-stu-id="43476-162">By default, the VM will never expire.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="43476-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="43476-163">Next steps</span></span>
<span data-ttu-id="43476-164">Miután definiálva, és a tesztkörnyezet a különböző virtuális gép házirend-beállításokat alkalmazza, az alábbiakban ezután próbálja meg a következőkről:</span><span class="sxs-lookup"><span data-stu-id="43476-164">Once you've defined and applied the various VM policy settings for your lab, here are some things to try next:</span></span>

* <span data-ttu-id="43476-165">[Megosztott IP-címek megértéséhez](devtest-lab-shared-ip.md) -ismerteti, hogyan megosztott IP címek szolgálnak a DevTest Labs szolgáltatásban a labor virtuális gépeken való kapcsolódáshoz szükséges nyilvános IP-címek számának minimalizálása érdekében.</span><span class="sxs-lookup"><span data-stu-id="43476-165">[Understand shared IP addresses](devtest-lab-shared-ip.md) - Explains how shared IP addresses are used in DevTest Labs to minimize the number of public IP addresses required to connect to your lab VMs.</span></span>
* <span data-ttu-id="43476-166">[Költség felügyeletének konfigurálásához](devtest-lab-configure-cost-management.md) -bemutatja, hogyan használható a **havi becsült Költségtrend** diagram</span><span class="sxs-lookup"><span data-stu-id="43476-166">[Configure cost management](devtest-lab-configure-cost-management.md) - Illustrates how to use the **Monthly Estimated Cost Trend** chart</span></span>  
  <span data-ttu-id="43476-167">az aktuális hónap megtekintheti a következőre becsült költség-date és a várható befejezési hónap költsége.</span><span class="sxs-lookup"><span data-stu-id="43476-167">to view the current month's estimated cost-to-date and the projected end-of-month cost.</span></span>
* <span data-ttu-id="43476-168">[Hozzon létre egyéni lemezkép](devtest-lab-create-template.md) – a virtuális gépek létrehozásakor megad egy talál, amely lehet, vagy egy egyéni lemezképet, vagy egy Piactéri lemezképhez.</span><span class="sxs-lookup"><span data-stu-id="43476-168">[Create custom image](devtest-lab-create-template.md) - When you create a VM, you specify a base, which can be either a custom image or a Marketplace image.</span></span> <span data-ttu-id="43476-169">Ez a cikk bemutatja, hogyan egyéni lemezkép létrehozása a VHD-fájl.</span><span class="sxs-lookup"><span data-stu-id="43476-169">This article illustrates how to create a custom image from a VHD file.</span></span>
* <span data-ttu-id="43476-170">[Konfigurálja a piactéren elérhető rendszerkép](devtest-lab-configure-marketplace-images.md) – Azure DevTest Labs támogatja az Azure piactéren elérhető rendszerkép alapján virtuális gépek létrehozását.</span><span class="sxs-lookup"><span data-stu-id="43476-170">[Configure Marketplace images](devtest-lab-configure-marketplace-images.md) - Azure DevTest Labs supports creating VMs based on Azure Marketplace images.</span></span> <span data-ttu-id="43476-171">Ez a cikk bemutatja, hogyan határozhatja meg, ha bármely, az Azure piactéren elérhető rendszerkép lehet egy, amikor a virtuális gépek létrehozásakor használt.</span><span class="sxs-lookup"><span data-stu-id="43476-171">This article illustrates how to specify which, if any, Azure Marketplace images can be used when creating VMs in a lab.</span></span>
* <span data-ttu-id="43476-172">[Hozzon létre egy virtuális Gépet egy tesztkörnyezetben](devtest-lab-add-vm-with-artifacts.md) -bemutatja, hogyan hozható létre a virtuális gépek alapjául szolgáló lemezképhez (vagy egyéni vagy Marketplace), és hogyan működnek együtt a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="43476-172">[Create a VM in a lab](devtest-lab-add-vm-with-artifacts.md) - Illustrates how to create a VM from a base image (either custom or Marketplace), and how to work with artifacts in your VM.</span></span>

