---
title: "a virtuális gép és a PaaS aaaUse Azure DevTest Labs szolgáltatásban tesztelési környezetben |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure DevTest Labs szolgáltatásban virtuális gép és a PaaS tesztelése környezet-forgatókönyv."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: d4e2c334-643a-40c9-9051-625b8f39fc86
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: tarcher
ms.openlocfilehash: 9285090da768491e1275942318b094fae89e3273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-devtest-labs-for-vm-and-paas-test-environments"></a><span data-ttu-id="7a894-103">A virtuális gép és a PaaS használata Azure DevTest Labs szolgáltatásban tesztelési környezetben</span><span class="sxs-lookup"><span data-stu-id="7a894-103">Use Azure DevTest Labs for VM and PaaS test environments</span></span>

<span data-ttu-id="7a894-104">Azure DevTest Labs tooimplement sok főbb forgatókönyvek is használhat, de egy elsődleges forgatókönyv magában foglalja a DevTest Labs toohost gépek használatával tesztelők webhelyről.</span><span class="sxs-lookup"><span data-stu-id="7a894-104">You can use Azure DevTest Labs tooimplement many key scenarios, but a primary scenario involves using DevTest Labs toohost machines for testers.</span></span> 

<span data-ttu-id="7a894-105">Ebben a forgatókönyvben a DevTest Labs alábbi előnyöket nyújtja:</span><span class="sxs-lookup"><span data-stu-id="7a894-105">In this scenario, DevTest Labs provides these benefits:</span></span>

- <span data-ttu-id="7a894-106">Tesztelők webhelyről által gyors kiépítése az újrafelhasználható sablonokkal és az összetevők Windows és Linux környezetben tesztelheti az alkalmazás legújabb verziója hello.</span><span class="sxs-lookup"><span data-stu-id="7a894-106">Testers can test hello latest version of their application by quickly provisioning Windows and Linux environments using reusable templates and artifacts.</span></span>
- <span data-ttu-id="7a894-107">A betöltési tesztelése a kiépítés több teszt ügynök költenie tesztelők webhelyről.</span><span class="sxs-lookup"><span data-stu-id="7a894-107">Testers can scale up their load testing by provisioning multiple test agents.</span></span>
- <span data-ttu-id="7a894-108">A rendszergazdák szabályozhatják a költségek biztosításával, hogy:</span><span class="sxs-lookup"><span data-stu-id="7a894-108">Administrators can control costs by ensuring that:</span></span>
  - <span data-ttu-id="7a894-109">Tesztelők webhelyről van szükségük, mint további virtuális gépek nem olvasható be.</span><span class="sxs-lookup"><span data-stu-id="7a894-109">Testers cannot get more VMs than they need.</span></span>
  - <span data-ttu-id="7a894-110">Virtuális gépek leállnak le, ha nincsenek használatban.</span><span class="sxs-lookup"><span data-stu-id="7a894-110">VMs are shut down when not in use.</span></span>

![DevTest Labs használatát képzés](./media/devtest-lab-developer-lab/devtest-lab-developer-lab.png)

<span data-ttu-id="7a894-112">Ebben a cikkben vonatkozó különböző Azure DevTest Labs szolgáltatásban használt funkciók toomeet tester, és részletes lépéseket toofollow tooset mentése labor hello.</span><span class="sxs-lookup"><span data-stu-id="7a894-112">In this article, you learn about various Azure DevTest Labs features used toomeet tester requirements and hello detailed steps toofollow tooset up a lab.</span></span>

## <a name="implementing-test-environments-with-azure-devtest-labs"></a><span data-ttu-id="7a894-113">Az Azure DevTest Labs végrehajtási tesztkörülmények között</span><span class="sxs-lookup"><span data-stu-id="7a894-113">Implementing test environments with Azure DevTest Labs</span></span>
1. <span data-ttu-id="7a894-114">**Hello labor létrehozása**</span><span class="sxs-lookup"><span data-stu-id="7a894-114">**Create hello lab**</span></span> 
   
    <span data-ttu-id="7a894-115">Labs hello Azure DevTest Labs szolgáltatásban a kiindulási pontjaként.</span><span class="sxs-lookup"><span data-stu-id="7a894-115">Labs are hello starting point in Azure DevTest Labs.</span></span> <span data-ttu-id="7a894-116">Ha létrehoz egy tesztkörnyezetet, mint például a felhasználók (tesztelők webhelyről) toohello labor, beállítás házirendek toocontrol költségek, gyorsan, hozhat létre virtuális gép lemezképek meghatározása, és további hozzáadása feladatokat végezheti el.</span><span class="sxs-lookup"><span data-stu-id="7a894-116">Once you create a lab, you can perform tasks such as adding users (testers) toohello lab, setting policies toocontrol costs, defining VM images that can create quickly, and more.</span></span>  
   
    <span data-ttu-id="7a894-117">További hello hivatkozásokat a következő táblázat hello parancsával:</span><span class="sxs-lookup"><span data-stu-id="7a894-117">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="7a894-118">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="7a894-118">Task</span></span> | <span data-ttu-id="7a894-119">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="7a894-119">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="7a894-120">Labor létrehozása a Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="7a894-120">Create a lab in Azure DevTest Labs</span></span>](devtest-lab-create-lab.md) |<span data-ttu-id="7a894-121">Ismerje meg, hogyan toocreate egy Azure DevTest Labs labor hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="7a894-121">Learn how toocreate a lab in Azure DevTest Labs in hello Azure portal.</span></span> |
2. <span data-ttu-id="7a894-122">**Hozzon létre a virtuális gépek beépített piactéren elérhető rendszerkép és az egyéni lemezképek használatával percek alatt**</span><span class="sxs-lookup"><span data-stu-id="7a894-122">**Create VMs in minutes using ready-made marketplace images and custom images**</span></span> 
   
    <span data-ttu-id="7a894-123">Válassza ki az előre elkészített képek széles képek hello Azure piactér a, és hello, amikor elérhetővé teszi azokat.</span><span class="sxs-lookup"><span data-stu-id="7a894-123">You can pick ready-made images from a wide variety of images in hello Azure Marketplace and make them available in hello lab.</span></span> <span data-ttu-id="7a894-124">Ha hello előre elkészített lemezképek nem megfelelnek az elvárásainak, hozzon létre egy virtuális gép hello laborban egyéni lemezképként az Azure piactér van szüksége, minden hello szoftver telepítése és a virtuális gép mentése hello előre elkészített lemezkép használatával, amikor egyéni lemezkép is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="7a894-124">If hello ready-made images don't meet your requirements, you can create a custom image by creating a lab VM using a ready-made image from Azure Marketplace, installing all hello software that you need, and saving hello VM as a custom image in hello lab.</span></span>

    <span data-ttu-id="7a894-125">Ha egyéni lemezképek fog használni, fontolja meg egy kép gyári toocreate használatát, és a lemezképek terjesztése.</span><span class="sxs-lookup"><span data-stu-id="7a894-125">If you will be using custom images, consider using an image factory toocreate and distribute your images.</span></span> <span data-ttu-id="7a894-126">Egy kép gyári egy olyan konfigurációs, kód megoldás, rendszeresen készítésére és a beállított képek automatikusan továbbítja.</span><span class="sxs-lookup"><span data-stu-id="7a894-126">An image factory is a configuration-as-code solution that regularly builds and distributes your configured images automatically.</span></span> <span data-ttu-id="7a894-127">Ez menti hello szükséges idő toomanually hello rendszer konfigurálása után a virtuális gépek hello jött létre az operációs rendszer alapjául.</span><span class="sxs-lookup"><span data-stu-id="7a894-127">This saves hello time required toomanually configure hello system after a VM has been created with hello base OS.</span></span>
  
    <span data-ttu-id="7a894-128">További hello hivatkozásokat a következő táblázat hello parancsával:</span><span class="sxs-lookup"><span data-stu-id="7a894-128">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="7a894-129">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="7a894-129">Task</span></span> | <span data-ttu-id="7a894-130">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="7a894-130">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="7a894-131">Azure piactéren elérhető rendszerkép konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7a894-131">Configure Azure Marketplace images</span></span>](devtest-lab-configure-marketplace-images.md) |<span data-ttu-id="7a894-132">Megtudhatja, hogyan engedélyezett Azure piactéren elérhető rendszerkép zajlik, a kijelölés csak hello lemezképek elérhetővé kívánja hello tesztelők webhelyről.</span><span class="sxs-lookup"><span data-stu-id="7a894-132">Learn how you can whitelist Azure Marketplace images, making available for selection only hello images you want for hello testers.</span></span>|
   | [<span data-ttu-id="7a894-133">Egyéni lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="7a894-133">Create a custom image</span></span>](devtest-lab-create-template.md) |<span data-ttu-id="7a894-134">Hozzon létre egy egyéni lemezképet kell, hogy a tesztelők webhelyről gyorsan létre tud hozni egy virtuális gép hello egyéni lemezkép használatával hello szoftver előtti telepítésével.</span><span class="sxs-lookup"><span data-stu-id="7a894-134">Create a custom image by pre-installing hello software you need so that testers can quickly create a VM using hello custom image.</span></span>|
   | [<span data-ttu-id="7a894-135">Kép gyári megismerése</span><span class="sxs-lookup"><span data-stu-id="7a894-135">Learn about image factory</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/) |<span data-ttu-id="7a894-136">Bemutató videó ismerteti, hogyan tooset össze, és egy kép gyári használja.</span><span class="sxs-lookup"><span data-stu-id="7a894-136">Watch a video that describes how tooset up and use an image factory.</span></span>|

3. <span data-ttu-id="7a894-137">**A teszt gépek újrafelhasználható sablonok létrehozása**</span><span class="sxs-lookup"><span data-stu-id="7a894-137">**Create reusable templates for test machines**</span></span> 
   
    <span data-ttu-id="7a894-138">A Azure DevTest Labs szolgáltatásban képlet alapértelmezett tulajdonság értékek listájának használt toocreate egy virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="7a894-138">A formula in Azure DevTest Labs is a list of default property values used toocreate a VM.</span></span> <span data-ttu-id="7a894-139">Létrehozhat egy képletet hello laborkörnyezetben ki kép, a Virtuálisgép-méretet (kombinációja Processzor és memória szempontjából) és a virtuális hálózati.</span><span class="sxs-lookup"><span data-stu-id="7a894-139">You can create a formula in hello lab by picking an image, a VM size (a combination of CPU and RAM), and a virtual network.</span></span> <span data-ttu-id="7a894-140">Minden egyes tester hello képlet hello laborban látható és toocreate egy virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="7a894-140">Each tester can see hello formula in hello lab and use it toocreate a VM.</span></span> 
   
    <span data-ttu-id="7a894-141">További hello hivatkozásokat a következő táblázat hello parancsával:</span><span class="sxs-lookup"><span data-stu-id="7a894-141">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="7a894-142">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="7a894-142">Task</span></span> | <span data-ttu-id="7a894-143">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="7a894-143">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="7a894-144">DevTest Labs képletek toocreate virtuális gépek kezelése</span><span class="sxs-lookup"><span data-stu-id="7a894-144">Manage DevTest Labs formulas toocreate VMs</span></span>](devtest-lab-manage-formulas.md) |<span data-ttu-id="7a894-145">Ismerje meg, hogyan is létrehozhat egy képletet fel egy lemezképet, a Virtuálisgép-méretet (Processzor és memória szempontjából kombinációja) és a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="7a894-145">Learn how you can create a formula by picking up an image, VM size (combination of CPU and RAM), and a virtual network.</span></span>|

3. <span data-ttu-id="7a894-146">**Hozzon létre a virtuális Gépre kiterjedő tesztkörülmények között**</span><span class="sxs-lookup"><span data-stu-id="7a894-146">**Create multi-VM test environments**</span></span> 
   
    <span data-ttu-id="7a894-147">Azure Resource Manager sablonok toodefine hello infrastruktúrát és az Azure-megoldás konfigurációját, és több teszt virtuális gépek konzisztens állapotban ismételten telepítheti.</span><span class="sxs-lookup"><span data-stu-id="7a894-147">You can use Azure Resource Manager templates toodefine hello infrastructure and configuration of your Azure solution and repeatedly deploy multiple test VMs in a consistent state.</span></span>

    <span data-ttu-id="7a894-148">Azure PaaS-erőforrások kiépítése a Resource Manager-sablon a környezetben, és költségkövetést jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="7a894-148">Azure PaaS resources can be provisioned in an environment from a Resource Manager template and appear in cost tracking.</span></span> <span data-ttu-id="7a894-149">Virtuális gép automatikus leállítási azonban nem alkalmazza a tooPaaS erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="7a894-149">However, VM auto shutdown does not apply tooPaaS resources.</span></span>

    <span data-ttu-id="7a894-150">További hello hivatkozásokat a következő táblázat hello parancsával:</span><span class="sxs-lookup"><span data-stu-id="7a894-150">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="7a894-151">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="7a894-151">Task</span></span> | <span data-ttu-id="7a894-152">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="7a894-152">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="7a894-153">Több virtuális gépes környezet és PaaS-erőforrás létrehozása Azure Resource Manager-sablonokkal</span><span class="sxs-lookup"><span data-stu-id="7a894-153">Create multi-VM environments and PaaS resources with Azure Resource Manager templates</span></span>](devtest-lab-create-environment-from-arm.md) |<span data-ttu-id="7a894-154">Ismerje meg, hogyan telepíthet több virtuális gépek konzisztens állapotban a tesztelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="7a894-154">Learn how you can deploy multiple VMs in a consistent state for your test environment.</span></span>|

4. <span data-ttu-id="7a894-155">**Az összetevők tooenable rugalmas VM testreszabási létrehozása**</span><span class="sxs-lookup"><span data-stu-id="7a894-155">**Create artifacts tooenable flexible VM customization**</span></span>

   <span data-ttu-id="7a894-156">Az összetevők használt toodeploy, és állítsa be az alkalmazását, a virtuális gép kiépítése után.</span><span class="sxs-lookup"><span data-stu-id="7a894-156">Artifacts are used toodeploy and configure your application after a VM is provisioned.</span></span> <span data-ttu-id="7a894-157">Az összetevők lehetnek:</span><span class="sxs-lookup"><span data-stu-id="7a894-157">Artifacts can be:</span></span>

   - <span data-ttu-id="7a894-158">Az eszközök, amelyet a Virtuálisgép - hello például ügynökök, a Fiddler és a Visual Studio tooinstall.</span><span class="sxs-lookup"><span data-stu-id="7a894-158">Tools that you want tooinstall on hello VM - such as agents, Fiddler, and Visual Studio.</span></span>
   - <span data-ttu-id="7a894-159">A Virtuálisgép - hello toorun kívánt például egy tárház klónozása műveleteket.</span><span class="sxs-lookup"><span data-stu-id="7a894-159">Actions that you want toorun on hello VM - such as cloning a repo.</span></span>
   - <span data-ttu-id="7a894-160">Az alkalmazások, amelyet az tootest.</span><span class="sxs-lookup"><span data-stu-id="7a894-160">Applications that you want tootest.</span></span>

   <span data-ttu-id="7a894-161">Sok az összetevők még elérhető, a-kész.</span><span class="sxs-lookup"><span data-stu-id="7a894-161">Many artifacts are already available out-of-the-box.</span></span> <span data-ttu-id="7a894-162">De ha szeretne további testreszabási igény szerinti, saját egyéni összetevők hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="7a894-162">But if you want more customization for your specific needs, you can create your own custom artifacts.</span></span>

   <span data-ttu-id="7a894-163">További hello hivatkozásokat a következő táblázat hello parancsával:</span><span class="sxs-lookup"><span data-stu-id="7a894-163">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="7a894-164">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="7a894-164">Task</span></span> | <span data-ttu-id="7a894-165">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="7a894-165">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="7a894-166">Egyéni összetevők létrehozása a DevTest Labs szolgáltatásban virtuális Géphez</span><span class="sxs-lookup"><span data-stu-id="7a894-166">Create custom artifacts for your DevTest Labs VM</span></span>](devtest-lab-artifact-author.md) |<span data-ttu-id="7a894-167">A labor létrehozása a saját egyéni összetevők hello virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="7a894-167">Create your own custom artifacts for hello virtual machines in your lab.</span></span>|
   | [<span data-ttu-id="7a894-168">Adja hozzá a Git tárház toostore egyéni összetevők és az Azure Resource Manager sablonokban használható Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="7a894-168">Add a Git repository toostore custom artifacts and Azure Resource Manager templates for use in Azure DevTest Labs</span></span>](devtest-lab-add-artifact-repo.md) |<span data-ttu-id="7a894-169">Megtudhatja, hogyan toostore az egyéni összetevők a saját privát Git-tárház.</span><span class="sxs-lookup"><span data-stu-id="7a894-169">Learn how toostore your custom artifacts in your own private Git repo.</span></span>|

5. <span data-ttu-id="7a894-170">**Költségek szabályozása**</span><span class="sxs-lookup"><span data-stu-id="7a894-170">**Control costs**</span></span>
   
    <span data-ttu-id="7a894-171">Azure DevTest Labs lehetővé teszi tooset házirend hello labor toospecify hello maximális számú virtuális gépeket hozhat létre egy tester hello tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="7a894-171">Azure DevTest Labs allows you tooset a policy in hello lab toospecify hello maximum number of VMs that can be created by a tester in hello lab.</span></span> 
   
    <span data-ttu-id="7a894-172">Ha a teszt csoport tartozik egy ütemezés működik, és szeretné, hogy toostop hello virtuális gépeinek hello nap adott időpontban, és automatikusan indítsa újra őket követő hello, könnyen elvégezhető, amely beállítása automatikus leállítás be- és az automatikus indítási szabályzatok hello tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="7a894-172">If your test team has a set work schedule and you want toostop all hello VMs at a particular time of hello day and then automatically restart them hello following day, you can easily accomplish that by setting auto-shutdown and auto-start policies in hello lab.</span></span> 
   
    <span data-ttu-id="7a894-173">Végül alkalmazásfejlesztés befejeződése után törölheti hello virtuális gépeinek egyszerre egy PowerShell-parancsfájl futtatásával.</span><span class="sxs-lookup"><span data-stu-id="7a894-173">Finally, when app development is complete, you can delete all hello VMs at once by running a single PowerShell script.</span></span> 
   
    <span data-ttu-id="7a894-174">További hello hivatkozásokat a következő táblázat hello parancsával:</span><span class="sxs-lookup"><span data-stu-id="7a894-174">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="7a894-175">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="7a894-175">Task</span></span> | <span data-ttu-id="7a894-176">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="7a894-176">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="7a894-177">Laborszabályzatok definiálása</span><span class="sxs-lookup"><span data-stu-id="7a894-177">Define lab policies</span></span>](devtest-lab-set-lab-policy.md) |<span data-ttu-id="7a894-178">Hello laborban házirendek beállításával kapcsolatos költségek szabályozását.</span><span class="sxs-lookup"><span data-stu-id="7a894-178">Control costs by setting policies in hello lab.</span></span> |
   | [<span data-ttu-id="7a894-179">Törölje az összes hello labor virtuális gépeken, egy PowerShell-parancsfájl használatával</span><span class="sxs-lookup"><span data-stu-id="7a894-179">Delete all hello lab VMs using a PowerShell script</span></span>](devtest-lab-faq.md#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |<span data-ttu-id="7a894-180">Törli az összes hello labs egyetlen művelettel, ha a tesztelés.</span><span class="sxs-lookup"><span data-stu-id="7a894-180">Delete all hello labs in one operation when testing is complete.</span></span>|

1. <span data-ttu-id="7a894-181">**A virtuális hálózati tooa labor hozzáadása**</span><span class="sxs-lookup"><span data-stu-id="7a894-181">**Add a virtual network tooa Lab**</span></span> 
   
    <span data-ttu-id="7a894-182">DevTest Labs hoz létre egy új virtuális hálózatot (VNET), ha a labor létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7a894-182">DevTest Labs creates a new virtual network (VNET) whenever a lab is created.</span></span> <span data-ttu-id="7a894-183">Ha konfigurálta a saját virtuális Hálózatot – például használatával expressroute-on vagy a telephelyek közötti VPN – a virtuális hálózat tooyour labor virtuális hálózati beállításait úgy, hogy az elérhető virtuális gépek létrehozásakor is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="7a894-183">If you have configured your own VNET – for example, by using ExpressRoute or site-to-site VPN – you can add this VNET tooyour lab's virtual network settings so that it is available when creating VMs.</span></span>

    <span data-ttu-id="7a894-184">Emellett nincs Azure Active Directory tartományi csatlakozási összetevő érhető el, amely tartományhoz VM tooa hello virtuális gép létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="7a894-184">In addition, there is an Azure Active Directory domain join artifact available that joins a VM tooa domain when hello VM is being created.</span></span> 
   
    <span data-ttu-id="7a894-185">További hello hivatkozásokat a következő táblázat hello parancsával:</span><span class="sxs-lookup"><span data-stu-id="7a894-185">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="7a894-186">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="7a894-186">Task</span></span> | <span data-ttu-id="7a894-187">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="7a894-187">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="7a894-188">A Azure DevTest Labs szolgáltatásban virtuális hálózat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7a894-188">Configure a virtual network in Azure DevTest Labs</span></span>](devtest-lab-configure-vnet.md) |<span data-ttu-id="7a894-189">Ismerje meg, hogyan tooconfigure Azure DevTest Labs segítségével a virtuális hálózat hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="7a894-189">Learn how tooconfigure a virtual network in Azure DevTest Labs using hello Azure portal.</span></span>|

6. <span data-ttu-id="7a894-190">**Minden egyes tester hello labor megosztása**</span><span class="sxs-lookup"><span data-stu-id="7a894-190">**Share hello lab with each tester**</span></span>
   
    <span data-ttu-id="7a894-191">Labs közvetlenül elérhetők a tesztelők webhelyről közösen-kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="7a894-191">Labs can be directly accessed using a link that you share with your testers.</span></span> <span data-ttu-id="7a894-192">Még nincs toohave az Azure-fiók, mindaddig, amíg azok rendelkeznek egy [Microsoft-fiók](devtest-lab-faq.md#what-is-a-microsoft-account).</span><span class="sxs-lookup"><span data-stu-id="7a894-192">They don't even have toohave an Azure account, as long as they have a [Microsoft account](devtest-lab-faq.md#what-is-a-microsoft-account).</span></span> <span data-ttu-id="7a894-193">Tesztelők webhelyről más tesztelők webhelyről által létrehozott virtuális gépek nem látható.</span><span class="sxs-lookup"><span data-stu-id="7a894-193">Testers cannot see VMs created by other testers.</span></span>  
   
    <span data-ttu-id="7a894-194">További hello hivatkozásokat a következő táblázat hello parancsával:</span><span class="sxs-lookup"><span data-stu-id="7a894-194">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="7a894-195">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="7a894-195">Task</span></span> | <span data-ttu-id="7a894-196">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="7a894-196">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="7a894-197">Az Azure DevTest Labs tester tooa labor hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7a894-197">Add a tester tooa lab in Azure DevTest Labs</span></span>](devtest-lab-add-devtest-user.md) |<span data-ttu-id="7a894-198">Hello Azure portál tooadd tesztelők webhelyről tooyour labor használja.</span><span class="sxs-lookup"><span data-stu-id="7a894-198">Use hello Azure portal tooadd testers tooyour lab.</span></span>|
   | [<span data-ttu-id="7a894-199">Adja hozzá a tesztelők webhelyről toohello tesztlabor egy PowerShell-parancsfájl használatával</span><span class="sxs-lookup"><span data-stu-id="7a894-199">Add testers toohello lab using a PowerShell script</span></span>](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |<span data-ttu-id="7a894-200">Használjon PowerShell tooautomate tesztelők webhelyről tooyour labor hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="7a894-200">Use PowerShell tooautomate adding testers tooyour lab.</span></span> |
   | [<span data-ttu-id="7a894-201">Hivatkozás toohello labor beolvasása</span><span class="sxs-lookup"><span data-stu-id="7a894-201">Get a link toohello lab</span></span>](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |<span data-ttu-id="7a894-202">Ismerje meg, hogyan tesztelők webhelyről közvetlenül hozzáférhetnek a labor hivatkozáson keresztül.</span><span class="sxs-lookup"><span data-stu-id="7a894-202">Learn how testers can directly access a lab via a hyperlink.</span></span>|

7. <span data-ttu-id="7a894-203">**Labor létrehozása a további csapatokra automatizálása**</span><span class="sxs-lookup"><span data-stu-id="7a894-203">**Automate lab creation for more teams**</span></span> 
   
    <span data-ttu-id="7a894-204">Labor létrehozása, egyéni beállításait, például úgy, hogy a Resource Manager-sablon létrehozása és használata azt újra és újra toocreate azonos labs automatizálható.</span><span class="sxs-lookup"><span data-stu-id="7a894-204">You can automate lab creation, including custom settings, by creating a Resource Manager template and using it toocreate identical labs again and again.</span></span> 
   
    <span data-ttu-id="7a894-205">További hello hivatkozásokat a következő táblázat hello parancsával:</span><span class="sxs-lookup"><span data-stu-id="7a894-205">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="7a894-206">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="7a894-206">Task</span></span> | <span data-ttu-id="7a894-207">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="7a894-207">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="7a894-208">A Resource Manager sablonnal labor létrehozása</span><span class="sxs-lookup"><span data-stu-id="7a894-208">Create a lab using a Resource Manager template</span></span>](devtest-lab-faq.md#how-do-i-create-a-lab-from-an-azure-resource-manager-template) |<span data-ttu-id="7a894-209">Hozzon létre labs Azure DevTest Labs Resource Manager-sablonok használatával.</span><span class="sxs-lookup"><span data-stu-id="7a894-209">Create labs in Azure DevTest Labs using Resource Manager templates.</span></span> |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

