---
title: "aaaManage labor házirendek az Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Megtudhatja, hogyan toodefine labor házirendek, például a virtuális gép méretét, maximális virtuális gépek minden felhasználóhoz, és a Leállítás automation."
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
ms.openlocfilehash: 351b3645a1fd729455884e5d177877c2986bd853
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-all-policies-for-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="9d295-103">Egy Azure DevTest Labs szolgáltatásban található, amikor az összes házirend kezeléséhez</span><span class="sxs-lookup"><span data-stu-id="9d295-103">Manage all policies for a lab in Azure DevTest Labs</span></span>

<span data-ttu-id="9d295-104">Az Azure DevTest Labs lehetővé teszi a költségek szabályozásához, és a fejlesztőlaborokban lévő pazarlás minimalizálásához (beállítások) házirendek kezelése által az egyes labor.</span><span class="sxs-lookup"><span data-stu-id="9d295-104">Azure DevTest Labs lets you control cost and minimize waste in your labs by managing policies (settings) for each lab.</span></span> <span data-ttu-id="9d295-105">Ez a cikk részletes részletesen ismerteti hogyan tooset minden házirend.</span><span class="sxs-lookup"><span data-stu-id="9d295-105">This article explains in step-by-step detail how tooset each policy.</span></span>  

## <a name="set-allowed-virtual-machine-sizes"></a><span data-ttu-id="9d295-106">Virtuális gépek méretét engedélyezett beállítása</span><span class="sxs-lookup"><span data-stu-id="9d295-106">Set allowed virtual machine sizes</span></span>
<span data-ttu-id="9d295-107">hello beállítás hello házirend engedélyezett Virtuálisgép-méretek segít toominimize labor hulladék azáltal, hogy mely Virtuálisgép-méretek engedélyezettek hello labor toospecify engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="9d295-107">hello policy for setting hello allowed VM sizes helps toominimize lab waste by enabling you toospecify which VM sizes are allowed in hello lab.</span></span> <span data-ttu-id="9d295-108">Ha ez a házirend aktív, csak Virtuálisgép-méretek a listából lehet használt toocreate virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="9d295-108">If this policy is activated, only VM sizes from this list can be used toocreate VMs.</span></span>

1. <span data-ttu-id="9d295-109">A hello labor **konfigurációs és házirendek** panelen válassza **engedélyezett virtuális gépek méretét**.</span><span class="sxs-lookup"><span data-stu-id="9d295-109">On hello lab's **Configuration and policies** blade, select **Allowed virtual machines sizes**.</span></span>
   
    ![Engedélyezett virtuális gépek méretét](./media/devtest-lab-set-lab-policy/allowed-vm-sizes.png)

1. <span data-ttu-id="9d295-111">Válassza ki **a** tooenable ezt a házirendet, és **ki** toodisable azt.</span><span class="sxs-lookup"><span data-stu-id="9d295-111">Select **On** tooenable this policy, and **Off** toodisable it.</span></span>

1. <span data-ttu-id="9d295-112">Ha engedélyezi ezt a házirendet, válassza ki a tesztkörnyezetben hozható létre egy vagy több Virtuálisgép-méretek.</span><span class="sxs-lookup"><span data-stu-id="9d295-112">If you enable this policy, select one or more VM sizes that can be created in your lab.</span></span>

1. <span data-ttu-id="9d295-113">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="9d295-113">Select **Save**.</span></span>

## <a name="set-virtual-machines-per-user"></a><span data-ttu-id="9d295-114">Beállítása virtuális gépek száma felhasználónként</span><span class="sxs-lookup"><span data-stu-id="9d295-114">Set virtual machines per user</span></span>
<span data-ttu-id="9d295-115">a házirend hello **virtuális gépek száma felhasználónként** lehetővé teszi a toospecify hello maximális számát egy adott felhasználó által létrehozott virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="9d295-115">hello policy for **Virtual machines per user** allows you toospecify hello maximum number of VMs that can be created by an individual user.</span></span> <span data-ttu-id="9d295-116">Akkor a toocreate vagy a jogcím egy virtuális gép hello megadott felhasználói korlátot teljesülésekor, hibaüzenet jelzi, hogy virtuális gép nem hozható létre /, hello.</span><span class="sxs-lookup"><span data-stu-id="9d295-116">If a user attempts toocreate or claim a VM when hello user limit has been met, an error message indicates that hello VM cannot be created/claimed.</span></span> 

1. <span data-ttu-id="9d295-117">A hello labor **konfigurációs és házirendek** menüjében válassza **virtuális gépek száma felhasználónként**.</span><span class="sxs-lookup"><span data-stu-id="9d295-117">On hello lab's **Configuration and policies** menu, select **Virtual machines per user**.</span></span>
   
    ![Virtuális gépek száma felhasználónként](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. <span data-ttu-id="9d295-119">Válassza ki **Igen** toolimit hello virtuális gépek száma felhasználónként.</span><span class="sxs-lookup"><span data-stu-id="9d295-119">Select **Yes** toolimit hello number of VMs per user.</span></span> <span data-ttu-id="9d295-120">Ha nem szeretné, hogy toolimit hello virtuális gépek száma felhasználónként, válassza ki a **nem**.</span><span class="sxs-lookup"><span data-stu-id="9d295-120">If you do not want toolimit hello number of VMs per user, select **No**.</span></span> <span data-ttu-id="9d295-121">Ha **Igen**, adjon meg egy numerikus érték, amely hello létrehozott vagy a felhasználó által igényelt virtuális gépek maximális számát.</span><span class="sxs-lookup"><span data-stu-id="9d295-121">If you select **Yes**, enter a numeric value indicating hello maximum number of VMs that can be created or claimed by a user.</span></span> 

1. <span data-ttu-id="9d295-122">Válassza ki **Igen** SSD (SSD lemezt) használó virtuális gépek toolimit hello száma.</span><span class="sxs-lookup"><span data-stu-id="9d295-122">Select **Yes** toolimit hello number of VMs that can use SSD (solid-state disk).</span></span> <span data-ttu-id="9d295-123">Ha nem szeretné, hogy a virtuális gépek által használható SSD toolimit hello száma, válassza ki a **nem**.</span><span class="sxs-lookup"><span data-stu-id="9d295-123">If you do not want toolimit hello number of VMs that can use SSD, select **No**.</span></span> <span data-ttu-id="9d295-124">Ha **Igen**, adjon meg egy értéket hello SSD segítségével hozhatók létre virtuális gépek maximális számát.</span><span class="sxs-lookup"><span data-stu-id="9d295-124">If you select **Yes**, enter a value indicating hello maximum number of VMs that can be created using SSD.</span></span> 

1. <span data-ttu-id="9d295-125">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="9d295-125">Select **Save**.</span></span>

## <a name="set-virtual-machines-per-lab"></a><span data-ttu-id="9d295-126">Beállítása virtuális gépek száma tesztkörnyezetenként</span><span class="sxs-lookup"><span data-stu-id="9d295-126">Set virtual machines per lab</span></span>
<span data-ttu-id="9d295-127">a házirend hello **virtuális gépek száma tesztkörnyezetenként** lehetővé teszi az aktuális tesztkörnyezetben hello toospecify hello maximális számát a virtuális gépek hozható létre.</span><span class="sxs-lookup"><span data-stu-id="9d295-127">hello policy for **Virtual machines per lab** allows you toospecify hello maximum number of VMs that can be created for hello current lab.</span></span> <span data-ttu-id="9d295-128">Akkor a virtuális gép toocreate hello labor korlát teljesülésekor, hibaüzenet jelzi, hogy hello virtuális gép nem hozható létre.</span><span class="sxs-lookup"><span data-stu-id="9d295-128">If a user attempts toocreate a VM when hello lab limit has been met, an error message indicates that hello VM cannot be created.</span></span> 

1. <span data-ttu-id="9d295-129">A hello labor **konfigurációs és házirendek** menüjében válassza **virtuális gépek száma tesztkörnyezetenként**.</span><span class="sxs-lookup"><span data-stu-id="9d295-129">On hello lab's **Configuration and policies** menu, select **Virtual machines per lab**.</span></span>
   
    ![Virtuális gépek száma tesztkörnyezetenként](./media/devtest-lab-set-lab-policy/max-vms-per-lab.png)

1. <span data-ttu-id="9d295-131">Válassza ki **Igen** toolimit hello számát a virtuális gépek száma tesztkörnyezetenként.</span><span class="sxs-lookup"><span data-stu-id="9d295-131">Select **Yes** toolimit hello number of VMs per lab.</span></span> <span data-ttu-id="9d295-132">Ha nem szeretné, hogy toolimit hello számát a virtuális gépek száma tesztkörnyezetenként, válassza ki a **nem**.</span><span class="sxs-lookup"><span data-stu-id="9d295-132">If you do not want toolimit hello number of VMs per lab, select **No**.</span></span> <span data-ttu-id="9d295-133">Ha **Igen**, adjon meg egy numerikus érték, amely hello létrehozott vagy a felhasználó által igényelt virtuális gépek maximális számát.</span><span class="sxs-lookup"><span data-stu-id="9d295-133">If you select **Yes**, enter a numeric value indicating hello maximum number of VMs that can be created or claimed by a user.</span></span> 

1. <span data-ttu-id="9d295-134">Válassza ki **Igen** SSD (SSD lemezt) használó virtuális gépek toolimit hello száma.</span><span class="sxs-lookup"><span data-stu-id="9d295-134">Select **Yes** toolimit hello number of VMs that can use SSD (solid-state disk).</span></span> <span data-ttu-id="9d295-135">Ha nem szeretné, hogy a virtuális gépek által használható SSD toolimit hello száma, válassza ki a **nem**.</span><span class="sxs-lookup"><span data-stu-id="9d295-135">If you do not want toolimit hello number of VMs that can use SSD, select **No**.</span></span> <span data-ttu-id="9d295-136">Ha **Igen**, adjon meg egy értéket hello SSD segítségével hozhatók létre virtuális gépek maximális számát.</span><span class="sxs-lookup"><span data-stu-id="9d295-136">If you select **Yes**, enter a value indicating hello maximum number of VMs that can be created using SSD.</span></span> 

1. <span data-ttu-id="9d295-137">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="9d295-137">Select **Save**.</span></span>

## <a name="set-auto-shutdown"></a><span data-ttu-id="9d295-138">Készlet automatikus rendszerleállítást</span><span class="sxs-lookup"><span data-stu-id="9d295-138">Set auto-shutdown</span></span>
<span data-ttu-id="9d295-139">hello automatikus leállítási házirendet lehetővé teszik, hogy a labor virtuális gépek leállítása toospecify hello idő hulladék toominimize tesztkörnyezet segítségével.</span><span class="sxs-lookup"><span data-stu-id="9d295-139">hello auto-shutdown policy helps toominimize lab waste by allowing you toospecify hello time that this lab's VMs shut down.</span></span>

1. <span data-ttu-id="9d295-140">A hello labor **konfigurációs és házirendek** panelen válassza **automatikus leállítási**.</span><span class="sxs-lookup"><span data-stu-id="9d295-140">On hello lab's **Configuration and policies** blade, select **Auto-shutdown**.</span></span>
   
    ![Automatikus leállítási](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. <span data-ttu-id="9d295-142">Válassza ki **a** tooenable ezt a házirendet, és **ki** toodisable azt.</span><span class="sxs-lookup"><span data-stu-id="9d295-142">Select **On** tooenable this policy, and **Off** toodisable it.</span></span>

1. <span data-ttu-id="9d295-143">Ha engedélyezi ezt a házirendet, adja meg a hello idő (és időzóna) tooshut minden virtuális gép le hello aktuális tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="9d295-143">If you enable this policy, specify hello time (and time zone) tooshut down all VMs in hello current lab.</span></span>

1. <span data-ttu-id="9d295-144">Adja meg **Igen** vagy **nem** hello beállítás toosend egy értesítési 15 perccel korábbi toohello megadva automatikus leállítási ideje.</span><span class="sxs-lookup"><span data-stu-id="9d295-144">Specify **Yes** or **No** for hello option toosend a notification 15 minutes prior toohello specified auto-shutdown time.</span></span> <span data-ttu-id="9d295-145">Ha megad **Igen**, írja be a webhook URL-cím végpont tooreceive hello értesítést.</span><span class="sxs-lookup"><span data-stu-id="9d295-145">If you specify **Yes**, enter a webhook URL endpoint tooreceive hello notification.</span></span> <span data-ttu-id="9d295-146">További információ a webhookok: [webhook vagy API Azure-függvény létrehozása](../azure-functions/functions-create-a-web-hook-or-api-function.md).</span><span class="sxs-lookup"><span data-stu-id="9d295-146">For more information about webhooks, see [Create a webhook or API Azure Function](../azure-functions/functions-create-a-web-hook-or-api-function.md).</span></span> 

1. <span data-ttu-id="9d295-147">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="9d295-147">Select **Save**.</span></span>

    <span data-ttu-id="9d295-148">Alapértelmezés szerint egyszer engedélyezve van ez a házirend érvényes tooall virtuális gépek hello aktuális tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="9d295-148">By default, once enabled, this policy applies tooall VMs in hello current lab.</span></span> <span data-ttu-id="9d295-149">tooremove ezt a beállítást egy adott virtuális gépről, nyissa meg a hello VM panelt, és módosítsa a **automatikus leállítási** beállítás</span><span class="sxs-lookup"><span data-stu-id="9d295-149">tooremove this setting from a specific VM, open hello VM's blade and change its **Auto-shutdown** setting</span></span> 

## <a name="set-auto-start"></a><span data-ttu-id="9d295-150">Készlet automatikus indítás</span><span class="sxs-lookup"><span data-stu-id="9d295-150">Set auto-start</span></span>
<span data-ttu-id="9d295-151">hello automatikusan elinduló házirend lehetővé teszi toospecify amikor hello aktuális tesztkörnyezetben lévő virtuális gépek hello el kell indítani.</span><span class="sxs-lookup"><span data-stu-id="9d295-151">hello auto-start policy allows you toospecify when hello VMs in hello current lab should be started.</span></span>  

1. <span data-ttu-id="9d295-152">A hello labor **konfigurációs és házirendek** panelen válassza **automatikusan elinduló**.</span><span class="sxs-lookup"><span data-stu-id="9d295-152">On hello lab's **Configuration and policies** blade, select **Auto-start**.</span></span>
   
    ![Automatikus indítás](./media/devtest-lab-set-lab-policy/auto-start.png)

2. <span data-ttu-id="9d295-154">Válassza ki **a** tooenable ezt a házirendet, és **ki** toodisable azt.</span><span class="sxs-lookup"><span data-stu-id="9d295-154">Select **On** tooenable this policy, and **Off** toodisable it.</span></span>

3. <span data-ttu-id="9d295-155">Ha engedélyezi ezt a házirendet, adja meg a hello ütemezett kezdési ideje, időzóna és hello napot hello mely hello idő vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="9d295-155">If you enable this policy, specify hello scheduled start time, time zone, and hello days of hello week for which hello time applies.</span></span> 

4. <span data-ttu-id="9d295-156">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="9d295-156">Select **Save**.</span></span>

    <span data-ttu-id="9d295-157">Engedélyezve van, ez a házirend nincs automatikusan alkalmazott tooany virtuális gépek hello aktuális tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="9d295-157">Once enabled, this policy is not automatically applied tooany VMs in hello current lab.</span></span> <span data-ttu-id="9d295-158">tooapply Ez a beállítás tooa speciális virtuális Gépet, a nyitott hello VM panel és a módosítás a **automatikusan elinduló** beállítás</span><span class="sxs-lookup"><span data-stu-id="9d295-158">tooapply this setting tooa specific VM, open hello VM's blade and change its **Auto-start** setting</span></span> 

## <a name="set-expiration-date"></a><span data-ttu-id="9d295-159">Lejárat dátuma</span><span class="sxs-lookup"><span data-stu-id="9d295-159">Set expiration date</span></span>
<span data-ttu-id="9d295-160">Megadhat egy lejárati dátum, [hello virtuális gép létrehozása](devtest-lab-add-vm.md).</span><span class="sxs-lookup"><span data-stu-id="9d295-160">You can set an expiration date when you [create hello VM](devtest-lab-add-vm.md).</span></span> <span data-ttu-id="9d295-161">A **speciális beállítások**, válassza ki a hello naptár ikon toospecify a dátum, amelyen hello a virtuális gép automatikusan törölve lesznek.</span><span class="sxs-lookup"><span data-stu-id="9d295-161">In **Advanced settings**, choose hello calendar icon toospecify a date on which hello VM will be automatically deleted.</span></span>  <span data-ttu-id="9d295-162">Alapértelmezés szerint a virtuális gép hello nem jár.</span><span class="sxs-lookup"><span data-stu-id="9d295-162">By default, hello VM will never expire.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="9d295-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9d295-163">Next steps</span></span>
<span data-ttu-id="9d295-164">Amennyiben már definiálva, és alkalmazza hello különböző VM házirend-beállítások a tesztkörnyezet, az alábbiakban néhány dolgot tootry mellett:</span><span class="sxs-lookup"><span data-stu-id="9d295-164">Once you've defined and applied hello various VM policy settings for your lab, here are some things tootry next:</span></span>

* <span data-ttu-id="9d295-165">[Megosztott IP-címek megértéséhez](devtest-lab-shared-ip.md) -ismerteti, hogyan megosztott IP címeket használják a DevTest Labs toominimize hello számot nyilvános IP címek szükséges tooconnect tooyour labor virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="9d295-165">[Understand shared IP addresses](devtest-lab-shared-ip.md) - Explains how shared IP addresses are used in DevTest Labs toominimize hello number of public IP addresses required tooconnect tooyour lab VMs.</span></span>
* <span data-ttu-id="9d295-166">[Költség felügyeletének konfigurálásához](devtest-lab-configure-cost-management.md) -bemutatja, hogyan toouse hello **havi becsült Költségtrend** diagram</span><span class="sxs-lookup"><span data-stu-id="9d295-166">[Configure cost management](devtest-lab-configure-cost-management.md) - Illustrates how toouse hello **Monthly Estimated Cost Trend** chart</span></span>  
  <span data-ttu-id="9d295-167">tooview hello az aktuális hónap becsült költség-date és tervezett hello hónap végi költség.</span><span class="sxs-lookup"><span data-stu-id="9d295-167">tooview hello current month's estimated cost-to-date and hello projected end-of-month cost.</span></span>
* <span data-ttu-id="9d295-168">[Hozzon létre egyéni lemezkép](devtest-lab-create-template.md) – a virtuális gépek létrehozásakor megad egy talál, amely lehet, vagy egy egyéni lemezképet, vagy egy Piactéri lemezképhez.</span><span class="sxs-lookup"><span data-stu-id="9d295-168">[Create custom image](devtest-lab-create-template.md) - When you create a VM, you specify a base, which can be either a custom image or a Marketplace image.</span></span> <span data-ttu-id="9d295-169">Ez a cikk bemutatja, hogyan toocreate egyéni kép VHD-fájl.</span><span class="sxs-lookup"><span data-stu-id="9d295-169">This article illustrates how toocreate a custom image from a VHD file.</span></span>
* <span data-ttu-id="9d295-170">[Konfigurálja a piactéren elérhető rendszerkép](devtest-lab-configure-marketplace-images.md) – Azure DevTest Labs támogatja az Azure piactéren elérhető rendszerkép alapján virtuális gépek létrehozását.</span><span class="sxs-lookup"><span data-stu-id="9d295-170">[Configure Marketplace images](devtest-lab-configure-marketplace-images.md) - Azure DevTest Labs supports creating VMs based on Azure Marketplace images.</span></span> <span data-ttu-id="9d295-171">Ez a cikk bemutatja, hogyan toospecify, amely, ha bármely, az Azure piactéren elérhető rendszerkép lehet egy, amikor a virtuális gépek létrehozásakor használt.</span><span class="sxs-lookup"><span data-stu-id="9d295-171">This article illustrates how toospecify which, if any, Azure Marketplace images can be used when creating VMs in a lab.</span></span>
* <span data-ttu-id="9d295-172">[Hozzon létre egy virtuális Gépet egy tesztkörnyezetben](devtest-lab-add-vm-with-artifacts.md) -bemutatja, hogyan toocreate alapjául szolgáló lemezképhez a virtuális gép (vagy egyéni vagy piactér), és hogyan toowork együtt a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="9d295-172">[Create a VM in a lab](devtest-lab-add-vm-with-artifacts.md) - Illustrates how toocreate a VM from a base image (either custom or Marketplace), and how toowork with artifacts in your VM.</span></span>

