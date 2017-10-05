---
title: "Használja az Azure DevTest Labs szolgáltatásban a fejlesztők számára |} Microsoft Docs"
description: "Útmutató: Azure DevTest Labs fejlesztői helyzetekben használhatja."
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
ms.openlocfilehash: c187819e9392908c8979556f80e8c94739eb14d5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-devtest-labs-for-developers"></a><span data-ttu-id="7a73e-103">Használja az Azure DevTest Labs szolgáltatásban a fejlesztők számára</span><span class="sxs-lookup"><span data-stu-id="7a73e-103">Use Azure DevTest Labs for developers</span></span>
<span data-ttu-id="7a73e-104">Azure DevTest Labs szolgáltatásban több kulcs forgatókönyv végrehajtásához használható, de egy elsődleges forgatókönyv magában foglalja a DevTest Labs segítségével fejlesztési gazdagépeken a fejlesztők számára.</span><span class="sxs-lookup"><span data-stu-id="7a73e-104">Azure DevTest Labs can be used to implement many key scenarios, but one of the primary scenarios involves using DevTest Labs to host development machines for developers.</span></span> <span data-ttu-id="7a73e-105">Ebben a forgatókönyvben a DevTest Labs alábbi előnyöket nyújtja:</span><span class="sxs-lookup"><span data-stu-id="7a73e-105">In this scenario, DevTest Labs provides these benefits:</span></span>

- <span data-ttu-id="7a73e-106">A fejlesztők gyorsan építhető ki az igény szerinti fejlesztési gépeik.</span><span class="sxs-lookup"><span data-stu-id="7a73e-106">Developers can quickly provision their development machines on demand.</span></span>
- <span data-ttu-id="7a73e-107">A fejlesztők könnyen teste szabhatja a fejlesztési gépeikhez, amikor erre szükség van.</span><span class="sxs-lookup"><span data-stu-id="7a73e-107">Developers can easily customize their development machines whenever needed.</span></span>
- <span data-ttu-id="7a73e-108">A rendszergazdák szabályozhatják a költségek biztosításával, hogy:</span><span class="sxs-lookup"><span data-stu-id="7a73e-108">Administrators can control costs by ensuring that:</span></span>
  - <span data-ttu-id="7a73e-109">A fejlesztők további virtuális gépek fejlesztési van szükségük, mint nem olvasható be.</span><span class="sxs-lookup"><span data-stu-id="7a73e-109">Developers cannot get more VMs than they need for development.</span></span>
  - <span data-ttu-id="7a73e-110">Virtuális gépek leállnak le, ha nincsenek használatban.</span><span class="sxs-lookup"><span data-stu-id="7a73e-110">VMs are shut down when not in use.</span></span> 

![DevTest Labs használatát képzés](./media/devtest-lab-developer-lab/devtest-lab-developer-lab.png)

<span data-ttu-id="7a73e-112">Ebből a cikkből megismerheti fejlesztői követelmények teljesítéséhez használható különböző Azure DevTest Labs-szolgáltatások és a részletes lépéseket, amelyek követésével egy tesztlabor beállításához.</span><span class="sxs-lookup"><span data-stu-id="7a73e-112">In this article, you learn about various Azure DevTest Labs features that can be used to meet developer requirements and the detailed steps that you can follow to set up a lab.</span></span>

## <a name="implementing-developer-environments-with-azure-devtest-labs"></a><span data-ttu-id="7a73e-113">Azure DevTest Labs végrehajtási fejlesztői környezetek</span><span class="sxs-lookup"><span data-stu-id="7a73e-113">Implementing developer environments with Azure DevTest Labs</span></span>
1. <span data-ttu-id="7a73e-114">**A labor létrehozása**</span><span class="sxs-lookup"><span data-stu-id="7a73e-114">**Create the lab**</span></span> 
   
    <span data-ttu-id="7a73e-115">Labs szerepelnek a Azure DevTest Labs szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="7a73e-115">Labs are the starting point in Azure DevTest Labs.</span></span> <span data-ttu-id="7a73e-116">Labor létrehozása után feladatokat végezheti el például a felhasználók (fejlesztők) hozzáadása a labor szabályozására a költségek, gyorsan, hozhat létre virtuális gép lemezképek meghatározása és egyéb házirendek beállítása.</span><span class="sxs-lookup"><span data-stu-id="7a73e-116">Once you create a lab, you can perform tasks such as adding users (developers) to the lab, setting policies to control costs, defining VM images that can create quickly, and more.</span></span>  
   
    <span data-ttu-id="7a73e-117">Az alábbi táblázatban szereplő hivatkozásokra kattintva, ahol további:</span><span class="sxs-lookup"><span data-stu-id="7a73e-117">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="7a73e-118">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="7a73e-118">Task</span></span> | <span data-ttu-id="7a73e-119">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="7a73e-119">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="7a73e-120">Labor létrehozása a Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="7a73e-120">Create a lab in Azure DevTest Labs</span></span>](devtest-lab-create-lab.md) |<span data-ttu-id="7a73e-121">Útmutató a labor létrehozása a Azure DevTest Labs szolgáltatásban az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="7a73e-121">Learn how to create a lab in Azure DevTest Labs in the Azure portal.</span></span> |
2. <span data-ttu-id="7a73e-122">**Hozzon létre a virtuális gépek beépített piactéren elérhető rendszerkép és az egyéni lemezképek használatával percek alatt**</span><span class="sxs-lookup"><span data-stu-id="7a73e-122">**Create VMs in minutes using ready-made marketplace images and custom images**</span></span> 
   
    <span data-ttu-id="7a73e-123">Válasszon az előre elkészített képek széles képek az Azure piactéren, és elérhetővé tétele a tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="7a73e-123">You can pick ready-made images from a wide variety of images in the Azure Marketplace and make them available in the lab.</span></span> <span data-ttu-id="7a73e-124">Ha az előre elkészített lemezképeket nem megfelelnek az elvárásainak, hozzon létre egy virtuális gépet egy előre elkészített lemezkép az Azure piactérről, telepíteni a szoftvert, amelyekre szüksége van, és a virtuális gép mentése az, amikor egyéni lemezképként, amikor egyéni kép is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="7a73e-124">If the ready-made images don't meet your requirements, you can create a custom image by creating a lab VM using a ready-made image from Azure Marketplace, installing all the software that you need, and saving the VM as a custom image in the lab.</span></span>

    <span data-ttu-id="7a73e-125">Ha egyéni lemezképek fog használni, érdemes lehet egy kép gyári létrehozásához, és a lemezképek terjesztése.</span><span class="sxs-lookup"><span data-stu-id="7a73e-125">If you will be using custom images, consider using an image factory to create and distribute your images.</span></span> <span data-ttu-id="7a73e-126">Egy kép gyári egy olyan konfigurációs, kód megoldás, rendszeresen készítésére és a beállított képek automatikusan továbbítja.</span><span class="sxs-lookup"><span data-stu-id="7a73e-126">An image factory is a configuration-as-code solution that regularly builds and distributes your configured images automatically.</span></span> <span data-ttu-id="7a73e-127">Ez menti a rendszer az alap operációs rendszer virtuális gép létrehozása után kézzel konfigurálásához szükséges időt.</span><span class="sxs-lookup"><span data-stu-id="7a73e-127">This saves the time required to manually configure the system after a VM has been created with the base OS.</span></span>
  
    <span data-ttu-id="7a73e-128">Az alábbi táblázatban szereplő hivatkozásokra kattintva, ahol további:</span><span class="sxs-lookup"><span data-stu-id="7a73e-128">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="7a73e-129">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="7a73e-129">Task</span></span> | <span data-ttu-id="7a73e-130">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="7a73e-130">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="7a73e-131">Azure piactéren elérhető rendszerkép konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7a73e-131">Configure Azure Marketplace images</span></span>](devtest-lab-configure-marketplace-images.md) |<span data-ttu-id="7a73e-132">Megtudhatja, hogyan zajlik engedélyezett Azure piactéren elérhető rendszerkép, így kijelölésnél érhetők el a lemezképek csak kívánja a fejlesztők számára.</span><span class="sxs-lookup"><span data-stu-id="7a73e-132">Learn how you can whitelist Azure Marketplace images, making available for selection only the images you want for the developers.</span></span>|
   | [<span data-ttu-id="7a73e-133">Egyéni lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="7a73e-133">Create a custom image</span></span>](devtest-lab-create-template.md) |<span data-ttu-id="7a73e-134">Létrehozhat egyéni rendszerképeket a szoftvert, hogy a fejlesztők gyorsan létre tud hozni egy virtuális Gépet, az egyéni lemezkép használatával kell előre telepítésével.</span><span class="sxs-lookup"><span data-stu-id="7a73e-134">Create a custom image by pre-installing the software you need so that developers can quickly create a VM using the custom image.</span></span>|
   | [<span data-ttu-id="7a73e-135">Kép gyári megismerése</span><span class="sxs-lookup"><span data-stu-id="7a73e-135">Learn about image factory</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/) |<span data-ttu-id="7a73e-136">Bemutató videó ismerteti, hogyan állítson be, és egy kép gyári használja.</span><span class="sxs-lookup"><span data-stu-id="7a73e-136">Watch a video that describes how to set up and use an image factory.</span></span>|

3. <span data-ttu-id="7a73e-137">**A fejlesztői gépek újrafelhasználható sablonok létrehozása**</span><span class="sxs-lookup"><span data-stu-id="7a73e-137">**Create reusable templates for developer machines**</span></span> 
   
    <span data-ttu-id="7a73e-138">Az Azure DevTest Labs képlet egy virtuális gép létrehozásához használt alapértelmezett tulajdonság értékek listáját.</span><span class="sxs-lookup"><span data-stu-id="7a73e-138">A formula in Azure DevTest Labs is a list of default property values used to create a VM.</span></span> <span data-ttu-id="7a73e-139">Képlet létrehozhatja a laborban lemezkép, a Virtuálisgép-méretet (kombinációja Processzor és memória szempontjából) és egy virtuális hálózatot válassza háttérszínnek.</span><span class="sxs-lookup"><span data-stu-id="7a73e-139">You can create a formula in the lab by picking an image, a VM size (a combination of CPU and RAM), and a virtual network.</span></span> <span data-ttu-id="7a73e-140">Minden egyes fejlesztői tekintse meg a képlet a laborkörnyezetben, és segítségével hozzon létre egy virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="7a73e-140">Each developer can see the formula in the lab and use it to create a VM.</span></span> 
   
    <span data-ttu-id="7a73e-141">Az alábbi táblázatban szereplő hivatkozásokra kattintva, ahol további:</span><span class="sxs-lookup"><span data-stu-id="7a73e-141">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="7a73e-142">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="7a73e-142">Task</span></span> | <span data-ttu-id="7a73e-143">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="7a73e-143">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="7a73e-144">Virtuális gépek létrehozása a DevTest Labs képletek kezelése</span><span class="sxs-lookup"><span data-stu-id="7a73e-144">Manage DevTest Labs formulas to create VMs</span></span>](devtest-lab-manage-formulas.md) |<span data-ttu-id="7a73e-145">Ismerje meg, hogyan is létrehozhat egy képletet fel egy lemezképet, a Virtuálisgép-méretet (Processzor és memória szempontjából kombinációja) és a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="7a73e-145">Learn how you can create a formula by picking up an image, VM size (combination of CPU and RAM), and a virtual network.</span></span>|

4. <span data-ttu-id="7a73e-146">**Ahhoz, hogy rugalmas VM testreszabási összetevők létrehozása**</span><span class="sxs-lookup"><span data-stu-id="7a73e-146">**Create artifacts to enable flexible VM customization**</span></span>

   <span data-ttu-id="7a73e-147">Összetevők segítségével telepítheti és konfigurálhatja az alkalmazás, egy virtuális gép kiépítése után.</span><span class="sxs-lookup"><span data-stu-id="7a73e-147">Artifacts are used to deploy and configure your application after a VM is provisioned.</span></span> <span data-ttu-id="7a73e-148">Az összetevők lehetnek:</span><span class="sxs-lookup"><span data-stu-id="7a73e-148">Artifacts can be:</span></span>

   - <span data-ttu-id="7a73e-149">A virtuális Gépen – például ügynökök, a Fiddler és a Visual Studio telepíteni kívánt eszközök.</span><span class="sxs-lookup"><span data-stu-id="7a73e-149">Tools that you want to install on the VM - such as agents, Fiddler, and Visual Studio.</span></span>
   - <span data-ttu-id="7a73e-150">A virtuális Gépen – például egy tárház klónozása futtatni kívánt műveletek.</span><span class="sxs-lookup"><span data-stu-id="7a73e-150">Actions that you want to run on the VM - such as cloning a repo.</span></span>
   - <span data-ttu-id="7a73e-151">Tesztelni kívánt alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="7a73e-151">Applications that you want to test.</span></span>

   <span data-ttu-id="7a73e-152">Sok az összetevők még elérhető, a-kész.</span><span class="sxs-lookup"><span data-stu-id="7a73e-152">Many artifacts are already available out-of-the-box.</span></span> <span data-ttu-id="7a73e-153">A saját egyéni összetevők hozhat létre, ha azt szeretné, további testreszabási saját igényeinek.</span><span class="sxs-lookup"><span data-stu-id="7a73e-153">You can create your own custom artifacts if you want more customization for your specific needs.</span></span>

   <span data-ttu-id="7a73e-154">Az alábbi táblázatban szereplő hivatkozásokra kattintva, ahol további:</span><span class="sxs-lookup"><span data-stu-id="7a73e-154">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="7a73e-155">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="7a73e-155">Task</span></span> | <span data-ttu-id="7a73e-156">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="7a73e-156">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="7a73e-157">Egyéni összetevők létrehozása a DevTest Labs szolgáltatásban virtuális Géphez</span><span class="sxs-lookup"><span data-stu-id="7a73e-157">Create custom artifacts for your DevTest Labs VM</span></span>](devtest-lab-artifact-author.md) |<span data-ttu-id="7a73e-158">A virtuális gépek a saját egyéni összetevők létrehozása a tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="7a73e-158">Create your own custom artifacts for the virtual machines in your lab.</span></span>|
   | [<span data-ttu-id="7a73e-159">Egyéni összetevők és az Azure Resource Manager sablonokban használható Azure DevTest Labs szolgáltatásban tárolni egy Git-tárház hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7a73e-159">Add a Git repository to store custom artifacts and Azure Resource Manager templates for use in Azure DevTest Labs</span></span>](devtest-lab-add-artifact-repo.md) |<span data-ttu-id="7a73e-160">Útmutató az egyéni összetevők tárolása a saját privát Git-tárház.</span><span class="sxs-lookup"><span data-stu-id="7a73e-160">Learn how to store your custom artifacts in your own private Git repo.</span></span>|

5. <span data-ttu-id="7a73e-161">**Költségek szabályozása**</span><span class="sxs-lookup"><span data-stu-id="7a73e-161">**Control costs**</span></span>
   
    <span data-ttu-id="7a73e-162">Az Azure DevTest Labs lehetővé teszi, hogy meg kell adnia egy házirendet a laborban a, amikor a fejlesztők által létrehozott virtuális gépek maximális száma.</span><span class="sxs-lookup"><span data-stu-id="7a73e-162">Azure DevTest Labs allows you to set a policy in the lab to specify the maximum number of VMs that can be created by a developer in the lab.</span></span> 
   
    <span data-ttu-id="7a73e-163">Ha a fejlesztői csapat tartozik egy munkahelyi ütemezést, és a virtuális gépek leállítása a nap adott időpontban, és automatikusan indítsa újra őket a következő napon, könnyen elvégezhető, amely a laborkörnyezetben automatikus leállítás be- és az automatikus indítási házirendek beállításával.</span><span class="sxs-lookup"><span data-stu-id="7a73e-163">If your developer team has a set work schedule and you want to stop all the VMs at a particular time of the day and then automatically restart them the following day, you can easily accomplish that by setting auto-shutdown and auto-start policies in the lab.</span></span> 
   
    <span data-ttu-id="7a73e-164">Végül alkalmazásfejlesztés befejeződése után törölheti a virtuális gépek egyszerre egy PowerShell-parancsfájl futtatásával.</span><span class="sxs-lookup"><span data-stu-id="7a73e-164">Finally, when app development is complete, you can delete all the VMs at once by running a single PowerShell script.</span></span> 
   
    <span data-ttu-id="7a73e-165">Az alábbi táblázatban szereplő hivatkozásokra kattintva, ahol további:</span><span class="sxs-lookup"><span data-stu-id="7a73e-165">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="7a73e-166">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="7a73e-166">Task</span></span> | <span data-ttu-id="7a73e-167">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="7a73e-167">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="7a73e-168">Laborszabályzatok definiálása</span><span class="sxs-lookup"><span data-stu-id="7a73e-168">Define lab policies</span></span>](devtest-lab-set-lab-policy.md) |<span data-ttu-id="7a73e-169">A labor házirendek beállításával kapcsolatos költségek szabályozását.</span><span class="sxs-lookup"><span data-stu-id="7a73e-169">Control costs by setting policies in the lab.</span></span> |
   | [<span data-ttu-id="7a73e-170">Törölje a labor virtuális gépeken, egy PowerShell-parancsfájl használatával</span><span class="sxs-lookup"><span data-stu-id="7a73e-170">Delete all the lab VMs using a PowerShell script</span></span>](devtest-lab-faq.md#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |<span data-ttu-id="7a73e-171">Egyetlen művelettel összes labs törölni fejlesztési befejeződött.</span><span class="sxs-lookup"><span data-stu-id="7a73e-171">Delete all the labs in one operation when development is complete.</span></span>|

1. <span data-ttu-id="7a73e-172">**Virtuális hálózat hozzáadása a virtuális gépek**</span><span class="sxs-lookup"><span data-stu-id="7a73e-172">**Add a virtual network to a VM**</span></span> 
   
    <span data-ttu-id="7a73e-173">DevTest Labs hoz létre egy új virtuális hálózatot (VNET), ha a labor létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7a73e-173">DevTest Labs creates a new virtual network (VNET) whenever a lab is created.</span></span> <span data-ttu-id="7a73e-174">Ha konfigurálta a saját virtuális Hálózatot – például használatával expressroute-on vagy a telephelyek közötti VPN – adhat hozzá a VNETET a labor virtuális hálózati beállításait úgy, hogy az elérhető virtuális gépek létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="7a73e-174">If you have configured your own VNET – for example, by using ExpressRoute or site-to-site VPN – you can add this VNET to your lab's virtual network settings so that it is available when creating VMs.</span></span>

    <span data-ttu-id="7a73e-175">Emellett nincs Azure Active Directory tartományi csatlakozási összetevő érhető el, akkor csatlakozik egy virtuális Gépet egy tartományhoz a virtuális gép létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="7a73e-175">In addition, there is an Azure Active Directory domain join artifact available that will join a VM to a domain when the VM is being created.</span></span> 
   
    <span data-ttu-id="7a73e-176">Az alábbi táblázatban szereplő hivatkozásokra kattintva, ahol további:</span><span class="sxs-lookup"><span data-stu-id="7a73e-176">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="7a73e-177">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="7a73e-177">Task</span></span> | <span data-ttu-id="7a73e-178">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="7a73e-178">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="7a73e-179">A Azure DevTest Labs szolgáltatásban virtuális hálózat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7a73e-179">Configure a virtual network in Azure DevTest Labs</span></span>](devtest-lab-configure-vnet.md) |<span data-ttu-id="7a73e-180">Útmutató: virtuális hálózat konfigurálása a Azure DevTest Labs szolgáltatásban az Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="7a73e-180">Learn how to configure a virtual network in Azure DevTest Labs using the Azure portal.</span></span>|

6. <span data-ttu-id="7a73e-181">**A labor megosztása minden fejlesztői**</span><span class="sxs-lookup"><span data-stu-id="7a73e-181">**Share the lab with each developer**</span></span>
   
    <span data-ttu-id="7a73e-182">Labs közvetlenül elérhető a fejlesztők a megosztott kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="7a73e-182">Labs can be directly accessed using a link that you share with your developers.</span></span> <span data-ttu-id="7a73e-183">Még nincs Azure-fiókot, hogy mindaddig, amíg azok rendelkeznek egy [Microsoft-fiók](devtest-lab-faq.md#what-is-a-microsoft-account).</span><span class="sxs-lookup"><span data-stu-id="7a73e-183">They don't even have to have an Azure account, as long as they have a [Microsoft account](devtest-lab-faq.md#what-is-a-microsoft-account).</span></span> <span data-ttu-id="7a73e-184">A fejlesztők más fejlesztők által létrehozott virtuális gépek nem látható.</span><span class="sxs-lookup"><span data-stu-id="7a73e-184">Developers cannot see VMs created by other developers.</span></span>  
   
    <span data-ttu-id="7a73e-185">Az alábbi táblázatban szereplő hivatkozásokra kattintva, ahol további:</span><span class="sxs-lookup"><span data-stu-id="7a73e-185">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="7a73e-186">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="7a73e-186">Task</span></span> | <span data-ttu-id="7a73e-187">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="7a73e-187">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="7a73e-188">Egy fejlesztő a Azure DevTest Labs szolgáltatásban labor hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7a73e-188">Add a developer to a lab in Azure DevTest Labs</span></span>](devtest-lab-add-devtest-user.md) |<span data-ttu-id="7a73e-189">Az Azure-portál használatával adja hozzá a fejlesztők a laborkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="7a73e-189">Use the Azure portal to add developers to your lab.</span></span>|
   | [<span data-ttu-id="7a73e-190">Adja hozzá a fejlesztők a labor egy PowerShell-parancsfájl használatával</span><span class="sxs-lookup"><span data-stu-id="7a73e-190">Add developers to the lab using a PowerShell script</span></span>](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |<span data-ttu-id="7a73e-191">PowerShell használatával automatizálhatja a labor hozzáadása fejlesztők.</span><span class="sxs-lookup"><span data-stu-id="7a73e-191">Use PowerShell to automate adding developers to your lab.</span></span> |
   | [<span data-ttu-id="7a73e-192">Szerezzen be egy hivatkozást a laborkörnyezetben</span><span class="sxs-lookup"><span data-stu-id="7a73e-192">Get a link to the lab</span></span>](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |<span data-ttu-id="7a73e-193">Ismerje meg, hogyan fejlesztők közvetlenül hozzáférhetnek a labor hivatkozáson keresztül.</span><span class="sxs-lookup"><span data-stu-id="7a73e-193">Learn how developers can directly access a lab via a hyperlink.</span></span>|

7. <span data-ttu-id="7a73e-194">**Labor létrehozása a további csapatokra automatizálása**</span><span class="sxs-lookup"><span data-stu-id="7a73e-194">**Automate lab creation for more teams**</span></span> 
   
    <span data-ttu-id="7a73e-195">Labor létrehozása, egyéni beállításokat, beleértve a Resource Manager-sablonok létrehozásával, és újra és újra létre azonos labs segítségével automatizálható.</span><span class="sxs-lookup"><span data-stu-id="7a73e-195">You can automate lab creation, including custom settings, by creating a Resource Manager template and using it to create identical labs again and again.</span></span> 
   
    <span data-ttu-id="7a73e-196">Az alábbi táblázatban szereplő hivatkozásokra kattintva, ahol további:</span><span class="sxs-lookup"><span data-stu-id="7a73e-196">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="7a73e-197">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="7a73e-197">Task</span></span> | <span data-ttu-id="7a73e-198">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="7a73e-198">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="7a73e-199">A Resource Manager sablonnal labor létrehozása</span><span class="sxs-lookup"><span data-stu-id="7a73e-199">Create a lab using a Resource Manager template</span></span>](devtest-lab-faq.md#how-do-i-create-a-lab-from-an-azure-resource-manager-template) |<span data-ttu-id="7a73e-200">Hozzon létre labs Azure DevTest Labs Resource Manager-sablonok használatával.</span><span class="sxs-lookup"><span data-stu-id="7a73e-200">Create labs in Azure DevTest Labs using Resource Manager templates.</span></span> |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

