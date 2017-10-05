---
title: "Az első virtuális gép létrehozása egy tesztkörnyezetben, a Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Útmutató az első virtuális gép létrehozása a Azure DevTest Labs szolgáltatásban egy tesztkörnyezetben"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: fbc5a438-6e02-4952-b654-b8fa7322ae5f
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/24/2017
ms.author: tarcher
ms.openlocfilehash: aa6b60b799e1e98815cf288d5612f98cd77cc00e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-your-first-vm-in-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="b4d19-103">Az első virtuális gép létrehozása az Azure DevTest Labs szolgáltatásban egy tesztkörnyezetben</span><span class="sxs-lookup"><span data-stu-id="b4d19-103">Create your first VM in a lab in Azure DevTest Labs</span></span>

<span data-ttu-id="b4d19-104">Kezdetben DevTest Labs eléréséhez és az első virtuális gép létrehozásához, akkor lesz valószínűleg megteheti segítségével előre betöltött [alap Piactéri lemezképhez](devtest-lab-configure-marketplace-images.md).</span><span class="sxs-lookup"><span data-stu-id="b4d19-104">When you initially access DevTest Labs and want to create your first VM, you will likely do so using a pre-loaded [base marketplace image](devtest-lab-configure-marketplace-images.md).</span></span> <span data-ttu-id="b4d19-105">Később a is képes lesz választhat egy [egyéni lemezképet, és egy képletet](devtest-lab-add-vm.md) további virtuális gépek létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="b4d19-105">Later on, you'll also be able to choose from a [custom image and a formula](devtest-lab-add-vm.md) when creating more VMs.</span></span> 

<span data-ttu-id="b4d19-106">Ez az oktatóanyag végigvezeti az első virtuális gép hozzáadása egy laborhoz a DevTest Labs szolgáltatásban az Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="b4d19-106">This tutorial walks you through using the Azure portal to add your first VM to a lab in DevTest Labs.</span></span>

## <a name="steps-to-add-your-first-vm-to-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="b4d19-107">Lépések végrehajtásával adja hozzá az első virtuális gép egy tesztkörnyezetet a Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="b4d19-107">Steps to add your first VM to a lab in Azure DevTest Labs</span></span>
1. <span data-ttu-id="b4d19-108">Jelentkezzen be az [Azure Portalra](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="b4d19-108">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
1. <span data-ttu-id="b4d19-109">Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** a listából.</span><span class="sxs-lookup"><span data-stu-id="b4d19-109">Select **More Services**, and then select **DevTest Labs** from the list.</span></span>
1. <span data-ttu-id="b4d19-110">Labs listában jelölje ki a labor kívánja a virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b4d19-110">From the list of labs, select the lab in which you want to create the VM.</span></span>  
1. <span data-ttu-id="b4d19-111">A tesztlabor a **áttekintése** panelen válassza **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="b4d19-111">On the lab's **Overview** blade, select **+ Add**.</span></span>  

    ![Adja hozzá a virtuális gép gomb](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. <span data-ttu-id="b4d19-113">Az a **base válasszon** panelen válassza a Piactéri lemezképet a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="b4d19-113">On the **Choose a base** blade, select a marketplace image for the VM.</span></span>
1. <span data-ttu-id="b4d19-114">Az a **virtuális gép** panelen adjon meg egy nevet az új virtuális gép a **virtuálisgép-nevet** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="b4d19-114">On the **Virtual machine** blade, enter a name for the new virtual machine in the **Virtual machine name** text box.</span></span>

    ![Labor VM panel](./media/devtest-lab-add-vm/devtestlab-lab-add-first-vm.png)

1. <span data-ttu-id="b4d19-116">Adjon meg egy **felhasználónév** kap, amely rendszergazdai jogosultságokkal a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="b4d19-116">Enter a **User Name** that is granted administrator privileges on the virtual machine.</span></span>  
1. <span data-ttu-id="b4d19-117">Adjon meg egy jelszót a szövegmező feliratú **írjon be egy értéket**.</span><span class="sxs-lookup"><span data-stu-id="b4d19-117">Enter a password in the text field labeled **Type a value**.</span></span>
1. <span data-ttu-id="b4d19-118">A **virtuális gép lemeztípus** határozza meg, hogy milyen típusú a virtuális gépek a tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="b4d19-118">The **Virtual machine disk type** determines which storage disk type is allowed for the virtual machines in the lab.</span></span>
1. <span data-ttu-id="b4d19-119">Válassza ki **virtuálisgép-méret** , és válassza ki a Processzormagok RAM memória méretét és a merevlemez mérete a virtuális gép létrehozásához adjon meg előre meghatározott elemek.</span><span class="sxs-lookup"><span data-stu-id="b4d19-119">Select **Virtual machine size** and select one of the predefined items that specify the processor cores, RAM size, and the hard drive size of the VM to create.</span></span>
1. <span data-ttu-id="b4d19-120">Válassza ki **összetevők** és - műtermék - listából válassza ki és konfigurálja az alapjául szolgáló lemezképhez hozzáadni kívánt összetevők.</span><span class="sxs-lookup"><span data-stu-id="b4d19-120">Select **Artifacts** and - from the list of artifacts - select and configure the artifacts that you want to add to the base image.</span></span>
    <span data-ttu-id="b4d19-121">**Megjegyzés:** Ha ismerkedik a DevTest Labs szolgáltatásban, vagy tekintse meg az összetevők, konfigurálása a [meglévő összetevő felvétele a virtuális gépek](./devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) szakaszt, és térjen vissza ide befejezésekor.</span><span class="sxs-lookup"><span data-stu-id="b4d19-121">**Note:** If you're new to DevTest Labs or configuring artifacts, refer to the [Add an existing artifact to a VM](./devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) section, and then return here when finished.</span></span>
1. <span data-ttu-id="b4d19-122">Válassza ki **létrehozása** a megadott virtuális gép hozzáadása a labor.</span><span class="sxs-lookup"><span data-stu-id="b4d19-122">Select **Create** to add the specified VM to the lab.</span></span>

   <span data-ttu-id="b4d19-123">A tesztkörnyezet panel állapotát jeleníti meg a virtuális gép létrehozás - először mint **létrehozása**, majd, mint a **futtató** a virtuális gép elindítása után.</span><span class="sxs-lookup"><span data-stu-id="b4d19-123">The lab blade displays the status of the VM's creation - first as **Creating**, then as **Running** after the VM has been started.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b4d19-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b4d19-124">Next steps</span></span>
* <span data-ttu-id="b4d19-125">A virtuális gép létrehozása után keresztül csatlakozhat a virtuális gép kiválasztásával **Connect** a virtuális gép paneljén.</span><span class="sxs-lookup"><span data-stu-id="b4d19-125">Once the VM has been created, you can connect to the VM by selecting **Connect** on the VM's blade.</span></span>
* <span data-ttu-id="b4d19-126">Tekintse meg [virtuális gép hozzáadása egy laborhoz](devtest-lab-add-vm.md) bővebb információt a további virtuális gépek hozzáadása a tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="b4d19-126">Check out [Add a VM to a lab](devtest-lab-add-vm.md) for more complete info about adding subsequent VMs in your lab.</span></span>
* <span data-ttu-id="b4d19-127">Megismerkedhet a [Office DevTest Labs Azure Resource Manager gyorsindítási sablonok](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates).</span><span class="sxs-lookup"><span data-stu-id="b4d19-127">Explore the [DevTest Labs Azure Resource Manager QuickStart template gallery](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates).</span></span>
