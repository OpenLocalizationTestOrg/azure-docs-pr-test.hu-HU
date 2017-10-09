---
title: "aaaUse Azure DevTest Labs szolgáltatásban a fejlesztők számára |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Azure DevTest Labs fejlesztői forgatókönyvek esetén."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 22e070e5-3d1a-49fe-9d4c-5e07cb0b7fe2
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: tarcher
ms.openlocfilehash: 16a3ef47c9fcbca3050dd50db5b472a9a1e3c62c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-devtest-labs-for-developers"></a><span data-ttu-id="f02f3-103">Használja az Azure DevTest Labs szolgáltatásban a fejlesztők számára</span><span class="sxs-lookup"><span data-stu-id="f02f3-103">Use Azure DevTest Labs for developers</span></span>
<span data-ttu-id="f02f3-104">Az Azure DevTest Labs szolgáltatásban használt tooimplement sok főbb forgatókönyvek, de hello elsődleges forgatókönyvek közül magában foglalja a DevTest Labs toohost fejlesztési gépek segítségével a fejlesztők számára lehet.</span><span class="sxs-lookup"><span data-stu-id="f02f3-104">Azure DevTest Labs can be used tooimplement many key scenarios, but one of hello primary scenarios involves using DevTest Labs toohost development machines for developers.</span></span> <span data-ttu-id="f02f3-105">Ebben a forgatókönyvben a DevTest Labs alábbi előnyöket nyújtja:</span><span class="sxs-lookup"><span data-stu-id="f02f3-105">In this scenario, DevTest Labs provides these benefits:</span></span>

- <span data-ttu-id="f02f3-106">A fejlesztők gyorsan építhető ki az igény szerinti fejlesztési gépeik.</span><span class="sxs-lookup"><span data-stu-id="f02f3-106">Developers can quickly provision their development machines on demand.</span></span>
- <span data-ttu-id="f02f3-107">A fejlesztők könnyen teste szabhatja a fejlesztési gépeikhez, amikor erre szükség van.</span><span class="sxs-lookup"><span data-stu-id="f02f3-107">Developers can easily customize their development machines whenever needed.</span></span>
- <span data-ttu-id="f02f3-108">A rendszergazdák szabályozhatják a költségek biztosításával, hogy:</span><span class="sxs-lookup"><span data-stu-id="f02f3-108">Administrators can control costs by ensuring that:</span></span>
  - <span data-ttu-id="f02f3-109">A fejlesztők további virtuális gépek fejlesztési van szükségük, mint nem olvasható be.</span><span class="sxs-lookup"><span data-stu-id="f02f3-109">Developers cannot get more VMs than they need for development.</span></span>
  - <span data-ttu-id="f02f3-110">Virtuális gépek leállnak le, ha nincsenek használatban.</span><span class="sxs-lookup"><span data-stu-id="f02f3-110">VMs are shut down when not in use.</span></span> 

![DevTest Labs használatát képzés](./media/devtest-lab-developer-lab/devtest-lab-developer-lab.png)

<span data-ttu-id="f02f3-112">Ebből a cikkből megismerheti használt toomeet fejlesztői követelmények és részletes lépéseket, hogy másolatot labor tooset követheti hello Azure DevTest Labs szolgáltatásokra vonatkozó.</span><span class="sxs-lookup"><span data-stu-id="f02f3-112">In this article, you learn about various Azure DevTest Labs features that can be used toomeet developer requirements and hello detailed steps that you can follow tooset up a lab.</span></span>

## <a name="implementing-developer-environments-with-azure-devtest-labs"></a><span data-ttu-id="f02f3-113">Azure DevTest Labs végrehajtási fejlesztői környezetek</span><span class="sxs-lookup"><span data-stu-id="f02f3-113">Implementing developer environments with Azure DevTest Labs</span></span>
1. <span data-ttu-id="f02f3-114">**Hello labor létrehozása**</span><span class="sxs-lookup"><span data-stu-id="f02f3-114">**Create hello lab**</span></span> 
   
    <span data-ttu-id="f02f3-115">Labs hello Azure DevTest Labs szolgáltatásban a kiindulási pontjaként.</span><span class="sxs-lookup"><span data-stu-id="f02f3-115">Labs are hello starting point in Azure DevTest Labs.</span></span> <span data-ttu-id="f02f3-116">Labor létrehozása után a feladatokat, mint a felhasználók (fejlesztők) toohello labor, a beállítás házirendek toocontrol költségek, gyorsan, hozhat létre virtuális gép lemezképek meghatározása és a több hozzáadását végezheti el.</span><span class="sxs-lookup"><span data-stu-id="f02f3-116">Once you create a lab, you can perform tasks such as adding users (developers) toohello lab, setting policies toocontrol costs, defining VM images that can create quickly, and more.</span></span>  
   
    <span data-ttu-id="f02f3-117">További hello hivatkozásokat a következő táblázat hello parancsával:</span><span class="sxs-lookup"><span data-stu-id="f02f3-117">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="f02f3-118">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="f02f3-118">Task</span></span> | <span data-ttu-id="f02f3-119">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="f02f3-119">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="f02f3-120">Labor létrehozása a Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="f02f3-120">Create a lab in Azure DevTest Labs</span></span>](devtest-lab-create-lab.md) |<span data-ttu-id="f02f3-121">Ismerje meg, hogyan toocreate egy Azure DevTest Labs labor hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="f02f3-121">Learn how toocreate a lab in Azure DevTest Labs in hello Azure portal.</span></span> |
2. <span data-ttu-id="f02f3-122">**Hozzon létre a virtuális gépek beépített piactéren elérhető rendszerkép és az egyéni lemezképek használatával percek alatt**</span><span class="sxs-lookup"><span data-stu-id="f02f3-122">**Create VMs in minutes using ready-made marketplace images and custom images**</span></span> 
   
    <span data-ttu-id="f02f3-123">Válassza ki az előre elkészített képek széles képek hello Azure piactér a, és hello, amikor elérhetővé teszi azokat.</span><span class="sxs-lookup"><span data-stu-id="f02f3-123">You can pick ready-made images from a wide variety of images in hello Azure Marketplace and make them available in hello lab.</span></span> <span data-ttu-id="f02f3-124">Ha hello előre elkészített lemezképek nem megfelelnek az elvárásainak, hozzon létre egy virtuális gép hello laborban egyéni lemezképként az Azure piactér van szüksége, minden hello szoftver telepítése és a virtuális gép mentése hello előre elkészített lemezkép használatával, amikor egyéni lemezkép is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="f02f3-124">If hello ready-made images don't meet your requirements, you can create a custom image by creating a lab VM using a ready-made image from Azure Marketplace, installing all hello software that you need, and saving hello VM as a custom image in hello lab.</span></span>

    <span data-ttu-id="f02f3-125">Ha egyéni lemezképek fog használni, fontolja meg egy kép gyári toocreate használatát, és a lemezképek terjesztése.</span><span class="sxs-lookup"><span data-stu-id="f02f3-125">If you will be using custom images, consider using an image factory toocreate and distribute your images.</span></span> <span data-ttu-id="f02f3-126">Egy kép gyári egy olyan konfigurációs, kód megoldás, rendszeresen készítésére és a beállított képek automatikusan továbbítja.</span><span class="sxs-lookup"><span data-stu-id="f02f3-126">An image factory is a configuration-as-code solution that regularly builds and distributes your configured images automatically.</span></span> <span data-ttu-id="f02f3-127">Ez menti hello szükséges idő toomanually hello rendszer konfigurálása után a virtuális gépek hello jött létre az operációs rendszer alapjául.</span><span class="sxs-lookup"><span data-stu-id="f02f3-127">This saves hello time required toomanually configure hello system after a VM has been created with hello base OS.</span></span>
  
    <span data-ttu-id="f02f3-128">További hello hivatkozásokat a következő táblázat hello parancsával:</span><span class="sxs-lookup"><span data-stu-id="f02f3-128">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="f02f3-129">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="f02f3-129">Task</span></span> | <span data-ttu-id="f02f3-130">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="f02f3-130">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="f02f3-131">Azure piactéren elérhető rendszerkép konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f02f3-131">Configure Azure Marketplace images</span></span>](devtest-lab-configure-marketplace-images.md) |<span data-ttu-id="f02f3-132">Megtudhatja, hogyan engedélyezett Azure piactéren elérhető rendszerkép zajlik, a kijelölés csak hello lemezképek elérhetővé kívánja hello fejlesztők számára.</span><span class="sxs-lookup"><span data-stu-id="f02f3-132">Learn how you can whitelist Azure Marketplace images, making available for selection only hello images you want for hello developers.</span></span>|
   | [<span data-ttu-id="f02f3-133">Egyéni lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="f02f3-133">Create a custom image</span></span>](devtest-lab-create-template.md) |<span data-ttu-id="f02f3-134">Hozzon létre egy egyéni lemezképet kell, hogy a fejlesztők gyorsan létre tud hozni egy virtuális gép hello egyéni lemezkép használatával hello szoftver előtti telepítésével.</span><span class="sxs-lookup"><span data-stu-id="f02f3-134">Create a custom image by pre-installing hello software you need so that developers can quickly create a VM using hello custom image.</span></span>|
   | [<span data-ttu-id="f02f3-135">Kép gyári megismerése</span><span class="sxs-lookup"><span data-stu-id="f02f3-135">Learn about image factory</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/) |<span data-ttu-id="f02f3-136">Bemutató videó ismerteti, hogyan tooset össze, és egy kép gyári használja.</span><span class="sxs-lookup"><span data-stu-id="f02f3-136">Watch a video that describes how tooset up and use an image factory.</span></span>|

3. <span data-ttu-id="f02f3-137">**A fejlesztői gépek újrafelhasználható sablonok létrehozása**</span><span class="sxs-lookup"><span data-stu-id="f02f3-137">**Create reusable templates for developer machines**</span></span> 
   
    <span data-ttu-id="f02f3-138">A Azure DevTest Labs szolgáltatásban képlet alapértelmezett tulajdonság értékek listájának használt toocreate egy virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="f02f3-138">A formula in Azure DevTest Labs is a list of default property values used toocreate a VM.</span></span> <span data-ttu-id="f02f3-139">Létrehozhat egy képletet hello laborkörnyezetben ki kép, a Virtuálisgép-méretet (kombinációja Processzor és memória szempontjából) és a virtuális hálózati.</span><span class="sxs-lookup"><span data-stu-id="f02f3-139">You can create a formula in hello lab by picking an image, a VM size (a combination of CPU and RAM), and a virtual network.</span></span> <span data-ttu-id="f02f3-140">Mindegyik fejlesztő látható hello képlet hello laborban és toocreate egy virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="f02f3-140">Each developer can see hello formula in hello lab and use it toocreate a VM.</span></span> 
   
    <span data-ttu-id="f02f3-141">További hello hivatkozásokat a következő táblázat hello parancsával:</span><span class="sxs-lookup"><span data-stu-id="f02f3-141">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="f02f3-142">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="f02f3-142">Task</span></span> | <span data-ttu-id="f02f3-143">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="f02f3-143">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="f02f3-144">DevTest Labs képletek toocreate virtuális gépek kezelése</span><span class="sxs-lookup"><span data-stu-id="f02f3-144">Manage DevTest Labs formulas toocreate VMs</span></span>](devtest-lab-manage-formulas.md) |<span data-ttu-id="f02f3-145">Ismerje meg, hogyan is létrehozhat egy képletet fel egy lemezképet, a Virtuálisgép-méretet (Processzor és memória szempontjából kombinációja) és a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="f02f3-145">Learn how you can create a formula by picking up an image, VM size (combination of CPU and RAM), and a virtual network.</span></span>|

4. <span data-ttu-id="f02f3-146">**Az összetevők tooenable rugalmas VM testreszabási létrehozása**</span><span class="sxs-lookup"><span data-stu-id="f02f3-146">**Create artifacts tooenable flexible VM customization**</span></span>

   <span data-ttu-id="f02f3-147">Az összetevők használt toodeploy, és állítsa be az alkalmazását, a virtuális gép kiépítése után.</span><span class="sxs-lookup"><span data-stu-id="f02f3-147">Artifacts are used toodeploy and configure your application after a VM is provisioned.</span></span> <span data-ttu-id="f02f3-148">Az összetevők lehetnek:</span><span class="sxs-lookup"><span data-stu-id="f02f3-148">Artifacts can be:</span></span>

   - <span data-ttu-id="f02f3-149">Az eszközök, amelyet a Virtuálisgép - hello például ügynökök, a Fiddler és a Visual Studio tooinstall.</span><span class="sxs-lookup"><span data-stu-id="f02f3-149">Tools that you want tooinstall on hello VM - such as agents, Fiddler, and Visual Studio.</span></span>
   - <span data-ttu-id="f02f3-150">A Virtuálisgép - hello toorun kívánt például egy tárház klónozása műveleteket.</span><span class="sxs-lookup"><span data-stu-id="f02f3-150">Actions that you want toorun on hello VM - such as cloning a repo.</span></span>
   - <span data-ttu-id="f02f3-151">Az alkalmazások, amelyet az tootest.</span><span class="sxs-lookup"><span data-stu-id="f02f3-151">Applications that you want tootest.</span></span>

   <span data-ttu-id="f02f3-152">Sok az összetevők még elérhető, a-kész.</span><span class="sxs-lookup"><span data-stu-id="f02f3-152">Many artifacts are already available out-of-the-box.</span></span> <span data-ttu-id="f02f3-153">A saját egyéni összetevők hozhat létre, ha azt szeretné, további testreszabási saját igényeinek.</span><span class="sxs-lookup"><span data-stu-id="f02f3-153">You can create your own custom artifacts if you want more customization for your specific needs.</span></span>

   <span data-ttu-id="f02f3-154">További hello hivatkozásokat a következő táblázat hello parancsával:</span><span class="sxs-lookup"><span data-stu-id="f02f3-154">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="f02f3-155">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="f02f3-155">Task</span></span> | <span data-ttu-id="f02f3-156">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="f02f3-156">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="f02f3-157">Egyéni összetevők létrehozása a DevTest Labs szolgáltatásban virtuális Géphez</span><span class="sxs-lookup"><span data-stu-id="f02f3-157">Create custom artifacts for your DevTest Labs VM</span></span>](devtest-lab-artifact-author.md) |<span data-ttu-id="f02f3-158">A labor létrehozása a saját egyéni összetevők hello virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="f02f3-158">Create your own custom artifacts for hello virtual machines in your lab.</span></span>|
   | [<span data-ttu-id="f02f3-159">Adja hozzá a Git tárház toostore egyéni összetevők és az Azure Resource Manager sablonokban használható Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="f02f3-159">Add a Git repository toostore custom artifacts and Azure Resource Manager templates for use in Azure DevTest Labs</span></span>](devtest-lab-add-artifact-repo.md) |<span data-ttu-id="f02f3-160">Megtudhatja, hogyan toostore az egyéni összetevők a saját privát Git-tárház.</span><span class="sxs-lookup"><span data-stu-id="f02f3-160">Learn how toostore your custom artifacts in your own private Git repo.</span></span>|

5. <span data-ttu-id="f02f3-161">**Költségek szabályozása**</span><span class="sxs-lookup"><span data-stu-id="f02f3-161">**Control costs**</span></span>
   
    <span data-ttu-id="f02f3-162">Az Azure DevTest Labs lehetővé teszi tooset házirend hello labor toospecify hello maximálisan megengedett számú hello, amikor a fejlesztők által létrehozott virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="f02f3-162">Azure DevTest Labs allows you tooset a policy in hello lab toospecify hello maximum number of VMs that can be created by a developer in hello lab.</span></span> 
   
    <span data-ttu-id="f02f3-163">Ha a fejlesztői csapat tartozik egy ütemezés működik, és szeretné, hogy toostop hello virtuális gépeinek hello nap adott időpontban, és automatikusan indítsa újra őket követő hello, könnyen elvégezhető, amely beállítása automatikus leállítás be- és az automatikus indítási szabályzatok hello tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="f02f3-163">If your developer team has a set work schedule and you want toostop all hello VMs at a particular time of hello day and then automatically restart them hello following day, you can easily accomplish that by setting auto-shutdown and auto-start policies in hello lab.</span></span> 
   
    <span data-ttu-id="f02f3-164">Végül alkalmazásfejlesztés befejeződése után törölheti hello virtuális gépeinek egyszerre egy PowerShell-parancsfájl futtatásával.</span><span class="sxs-lookup"><span data-stu-id="f02f3-164">Finally, when app development is complete, you can delete all hello VMs at once by running a single PowerShell script.</span></span> 
   
    <span data-ttu-id="f02f3-165">További hello hivatkozásokat a következő táblázat hello parancsával:</span><span class="sxs-lookup"><span data-stu-id="f02f3-165">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="f02f3-166">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="f02f3-166">Task</span></span> | <span data-ttu-id="f02f3-167">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="f02f3-167">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="f02f3-168">Laborszabályzatok definiálása</span><span class="sxs-lookup"><span data-stu-id="f02f3-168">Define lab policies</span></span>](devtest-lab-set-lab-policy.md) |<span data-ttu-id="f02f3-169">Hello laborban házirendek beállításával kapcsolatos költségek szabályozását.</span><span class="sxs-lookup"><span data-stu-id="f02f3-169">Control costs by setting policies in hello lab.</span></span> |
   | [<span data-ttu-id="f02f3-170">Törölje az összes hello labor virtuális gépeken, egy PowerShell-parancsfájl használatával</span><span class="sxs-lookup"><span data-stu-id="f02f3-170">Delete all hello lab VMs using a PowerShell script</span></span>](devtest-lab-faq.md#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |<span data-ttu-id="f02f3-171">Törli az összes hello labs egyetlen művelettel, ha fejlesztési befejeződött.</span><span class="sxs-lookup"><span data-stu-id="f02f3-171">Delete all hello labs in one operation when development is complete.</span></span>|

1. <span data-ttu-id="f02f3-172">**A virtuális gép virtuális hálózati tooa hozzáadása**</span><span class="sxs-lookup"><span data-stu-id="f02f3-172">**Add a virtual network tooa VM**</span></span> 
   
    <span data-ttu-id="f02f3-173">DevTest Labs hoz létre egy új virtuális hálózatot (VNET), ha a labor létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f02f3-173">DevTest Labs creates a new virtual network (VNET) whenever a lab is created.</span></span> <span data-ttu-id="f02f3-174">Ha konfigurálta a saját virtuális Hálózatot – például használatával expressroute-on vagy a telephelyek közötti VPN – a virtuális hálózat tooyour labor virtuális hálózati beállításait úgy, hogy az elérhető virtuális gépek létrehozásakor is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="f02f3-174">If you have configured your own VNET – for example, by using ExpressRoute or site-to-site VPN – you can add this VNET tooyour lab's virtual network settings so that it is available when creating VMs.</span></span>

    <span data-ttu-id="f02f3-175">Emellett nincs Azure Active Directory tartományi csatlakozási összetevő érhető el, amely virtuális gép tooa tartományhoz fog csatlakozni, hello virtuális gép létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="f02f3-175">In addition, there is an Azure Active Directory domain join artifact available that will join a VM tooa domain when hello VM is being created.</span></span> 
   
    <span data-ttu-id="f02f3-176">További hello hivatkozásokat a következő táblázat hello parancsával:</span><span class="sxs-lookup"><span data-stu-id="f02f3-176">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="f02f3-177">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="f02f3-177">Task</span></span> | <span data-ttu-id="f02f3-178">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="f02f3-178">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="f02f3-179">A Azure DevTest Labs szolgáltatásban virtuális hálózat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f02f3-179">Configure a virtual network in Azure DevTest Labs</span></span>](devtest-lab-configure-vnet.md) |<span data-ttu-id="f02f3-180">Ismerje meg, hogyan tooconfigure Azure DevTest Labs segítségével a virtuális hálózat hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="f02f3-180">Learn how tooconfigure a virtual network in Azure DevTest Labs using hello Azure portal.</span></span>|

6. <span data-ttu-id="f02f3-181">**Minden egyes fejlesztői hello labor megosztása**</span><span class="sxs-lookup"><span data-stu-id="f02f3-181">**Share hello lab with each developer**</span></span>
   
    <span data-ttu-id="f02f3-182">Labs közvetlenül elérhető a fejlesztők a megosztott kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="f02f3-182">Labs can be directly accessed using a link that you share with your developers.</span></span> <span data-ttu-id="f02f3-183">Még nincs toohave az Azure-fiók, mindaddig, amíg azok rendelkeznek egy [Microsoft-fiók](devtest-lab-faq.md#what-is-a-microsoft-account).</span><span class="sxs-lookup"><span data-stu-id="f02f3-183">They don't even have toohave an Azure account, as long as they have a [Microsoft account](devtest-lab-faq.md#what-is-a-microsoft-account).</span></span> <span data-ttu-id="f02f3-184">A fejlesztők más fejlesztők által létrehozott virtuális gépek nem látható.</span><span class="sxs-lookup"><span data-stu-id="f02f3-184">Developers cannot see VMs created by other developers.</span></span>  
   
    <span data-ttu-id="f02f3-185">További hello hivatkozásokat a következő táblázat hello parancsával:</span><span class="sxs-lookup"><span data-stu-id="f02f3-185">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="f02f3-186">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="f02f3-186">Task</span></span> | <span data-ttu-id="f02f3-187">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="f02f3-187">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="f02f3-188">Fejlesztői tooa labor hozzáadása a Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="f02f3-188">Add a developer tooa lab in Azure DevTest Labs</span></span>](devtest-lab-add-devtest-user.md) |<span data-ttu-id="f02f3-189">Hello Azure portál tooadd fejlesztők tooyour labor használja.</span><span class="sxs-lookup"><span data-stu-id="f02f3-189">Use hello Azure portal tooadd developers tooyour lab.</span></span>|
   | [<span data-ttu-id="f02f3-190">Adja hozzá a fejlesztők toohello tesztlabor egy PowerShell-parancsfájl használatával</span><span class="sxs-lookup"><span data-stu-id="f02f3-190">Add developers toohello lab using a PowerShell script</span></span>](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |<span data-ttu-id="f02f3-191">A fejlesztők tooyour labor hozzáadása PowerShell tooautomate használja.</span><span class="sxs-lookup"><span data-stu-id="f02f3-191">Use PowerShell tooautomate adding developers tooyour lab.</span></span> |
   | [<span data-ttu-id="f02f3-192">Hivatkozás toohello labor beolvasása</span><span class="sxs-lookup"><span data-stu-id="f02f3-192">Get a link toohello lab</span></span>](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |<span data-ttu-id="f02f3-193">Ismerje meg, hogyan fejlesztők közvetlenül hozzáférhetnek a labor hivatkozáson keresztül.</span><span class="sxs-lookup"><span data-stu-id="f02f3-193">Learn how developers can directly access a lab via a hyperlink.</span></span>|

7. <span data-ttu-id="f02f3-194">**Labor létrehozása a további csapatokra automatizálása**</span><span class="sxs-lookup"><span data-stu-id="f02f3-194">**Automate lab creation for more teams**</span></span> 
   
    <span data-ttu-id="f02f3-195">Labor létrehozása, egyéni beállításait, például úgy, hogy a Resource Manager-sablon létrehozása és használata azt újra és újra toocreate azonos labs automatizálható.</span><span class="sxs-lookup"><span data-stu-id="f02f3-195">You can automate lab creation, including custom settings, by creating a Resource Manager template and using it toocreate identical labs again and again.</span></span> 
   
    <span data-ttu-id="f02f3-196">További hello hivatkozásokat a következő táblázat hello parancsával:</span><span class="sxs-lookup"><span data-stu-id="f02f3-196">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="f02f3-197">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="f02f3-197">Task</span></span> | <span data-ttu-id="f02f3-198">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="f02f3-198">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="f02f3-199">A Resource Manager sablonnal labor létrehozása</span><span class="sxs-lookup"><span data-stu-id="f02f3-199">Create a lab using a Resource Manager template</span></span>](devtest-lab-faq.md#how-do-i-create-a-lab-from-an-azure-resource-manager-template) |<span data-ttu-id="f02f3-200">Hozzon létre labs Azure DevTest Labs Resource Manager-sablonok használatával.</span><span class="sxs-lookup"><span data-stu-id="f02f3-200">Create labs in Azure DevTest Labs using Resource Manager templates.</span></span> |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

