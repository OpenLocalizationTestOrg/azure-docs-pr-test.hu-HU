---
title: "Claimable virtuális gép hozzáadása egy laborhoz a Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Megtudhatja, hogyan claimable virtuális gép hozzáadása egy laborhoz a Azure DevTest Labs szolgáltatásban"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: f671e66e-9630-4e30-a131-a6bad9ed9c11
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: tarcher
ms.openlocfilehash: 98950d72e90b0e178bae2fffa7644fd824a25eea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-a-claimable-vm-to-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="72896-103">Claimable virtuális gép hozzáadása egy laborhoz a Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="72896-103">Add a claimable VM to a lab in Azure DevTest Labs</span></span>
<span data-ttu-id="72896-104">Ad hozzá egy claimable virtuális labor be, hogyan hasonló módon, [szabványos virtuális gép hozzáadása](devtest-lab-add-vm.md) – a egy *alap* , amely vagy egy [egyéni lemezkép](devtest-lab-create-template.md), [képlet](devtest-lab-manage-formulas.md) , vagy [Piactéri lemezképhez](devtest-lab-configure-marketplace-images.md).</span><span class="sxs-lookup"><span data-stu-id="72896-104">You add a claimable VM to a lab in a similar manner to how you [add a standard VM](devtest-lab-add-vm.md) – from a *base* that is either a [custom image](devtest-lab-create-template.md), [formula](devtest-lab-manage-formulas.md), or [Marketplace image](devtest-lab-configure-marketplace-images.md).</span></span> <span data-ttu-id="72896-105">Ez az oktatóanyag végigvezeti az Azure portál használata claimable virtuális gép hozzáadása egy laborhoz a DevTest Labs szolgáltatásban, és bemutatja a felhasználó a következő igényelni a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="72896-105">This tutorial walks you through using the Azure portal to add a claimable VM to a lab in DevTest Labs, and shows the process a user follows to claim the VM.</span></span>

> [!NOTE]
> <span data-ttu-id="72896-106">Ha a labor virtuális gépeken keresztül telepít [Azure Resource Manager-sablonok](devtest-lab-create-environment-from-arm.md), claimable virtuális gépeket hozhat létre úgy, hogy a **allowClaim** tulajdonság igaz értékű a Tulajdonságok szakaszának.</span><span class="sxs-lookup"><span data-stu-id="72896-106">If you deploy lab VMs through [Azure Resource Manager templates](devtest-lab-create-environment-from-arm.md), you can create claimable VMs by setting the **allowClaim** property to true in the properties section.</span></span>
>
>

## <a name="steps-to-add-a-claimable-vm-to-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="72896-107">Ismerteti a végrehajtás lépéseit claimable virtuális gép hozzáadása egy laborhoz a Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="72896-107">Steps to add a claimable VM to a lab in Azure DevTest Labs</span></span>
1. <span data-ttu-id="72896-108">Jelentkezzen be az [Azure Portalra](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="72896-108">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
1. <span data-ttu-id="72896-109">Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** a listából.</span><span class="sxs-lookup"><span data-stu-id="72896-109">Select **More Services**, and then select **DevTest Labs** from the list.</span></span>
1. <span data-ttu-id="72896-110">Labs listában jelölje ki a labor kívánja claimable virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="72896-110">From the list of labs, select the lab in which you want to create the claimable VM.</span></span>  
1. <span data-ttu-id="72896-111">A tesztlabor a **áttekintése** panelen válassza **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="72896-111">On the lab's **Overview** blade, select **+ Add**.</span></span>  

    ![Adja hozzá a virtuális gép gomb](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. <span data-ttu-id="72896-113">Az a **base válasszon** panelen válassza ki a megfelelő alapja a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="72896-113">On the **Choose a base** blade, select a base for the VM.</span></span>
1. <span data-ttu-id="72896-114">Az a **virtuális gép** panelen adjon meg egy nevet az új virtuális gép a **virtuálisgép-nevet** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="72896-114">On the **Virtual machine** blade, enter a name for the new virtual machine in the **Virtual machine name** text box.</span></span>

    ![Labor VM panel](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. <span data-ttu-id="72896-116">Adjon meg egy **felhasználónév** kap, amely rendszergazdai jogosultságokkal a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="72896-116">Enter a **User Name** that is granted administrator privileges on the virtual machine.</span></span>  
1. <span data-ttu-id="72896-117">Ha azt szeretné tárolni a jelszó használatát a [titkos tároló](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store), jelölje be **mentett titkos kulcs használata**, és adja meg a kulcs értéke, amely megfelel a titkos kulcs (jelszó).</span><span class="sxs-lookup"><span data-stu-id="72896-117">If you want to use a password stored in your [secret store](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store), select **Use a saved secret**, and specify a key value that corresponds to your secret (password).</span></span> <span data-ttu-id="72896-118">Ellenkező esetben a feliratú szövegmezőbe írja be a jelszót **írjon be egy értéket**.</span><span class="sxs-lookup"><span data-stu-id="72896-118">Otherwise, enter a password in the text field labeled **Type a value**.</span></span>
1. <span data-ttu-id="72896-119">A **virtuális gép lemeztípus** határozza meg, hogy milyen típusú a virtuális gépek a tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="72896-119">The **Virtual machine disk type** determines which storage disk type is allowed for the virtual machines in the lab.</span></span>
1. <span data-ttu-id="72896-120">Válassza ki **virtuálisgép-méret** , és válassza ki a Processzormagok RAM memória méretét és a merevlemez mérete a virtuális gép létrehozásához adjon meg előre meghatározott elemek.</span><span class="sxs-lookup"><span data-stu-id="72896-120">Select **Virtual machine size** and select one of the predefined items that specify the processor cores, RAM size, and the hard drive size of the VM to create.</span></span>
1. <span data-ttu-id="72896-121">Válassza ki **összetevők** és az összetevők listájában válassza ki és konfigurálja az alapjául szolgáló lemezképhez hozzáadni kívánt összetevők.</span><span class="sxs-lookup"><span data-stu-id="72896-121">Select **Artifacts** and from the list of artifacts, select and configure the artifacts that you want to add to the base image.</span></span> <span data-ttu-id="72896-122">Ha ismerkedik a DevTest Labs szolgáltatásban, vagy tekintse meg az összetevők, konfigurálása a [meglévő összetevő felvétele a virtuális gépek](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) szakaszt, és térjen vissza ide befejezésekor.</span><span class="sxs-lookup"><span data-stu-id="72896-122">If you're new to DevTest Labs or configuring artifacts, refer to the [Add an existing artifact to a VM](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) section, and then return here when finished.</span></span>
1. <span data-ttu-id="72896-123">Válassza ki **speciális beállítások** a virtuális gép hálózati beállításai és a lejárati beállítások konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="72896-123">Select **Advanced settings** to configure the VM's network options and expiration options.</span></span> <span data-ttu-id="72896-124">A **jogcím-beállítások**, válassza a **Igen** claimable ellenőrizze a gép számára.</span><span class="sxs-lookup"><span data-stu-id="72896-124">Under **Claim options**, choose **Yes** to make the machine claimable.</span></span>

  ![Válassza a virtuális gép claimable legyen.](./media/devtest-lab-add-vm/devtestlab-claim-VM-option.png)

1. <span data-ttu-id="72896-126">Ha szeretné megtekinteni, illetve az Azure Resource Manager sablon másolása, tekintse meg a [mentése Azure Resource Manager-sablon](devtest-lab-add-vm.md#save-azure-resource-manager-template) szakaszt, és térjen vissza ide, ha befejeződött.</span><span class="sxs-lookup"><span data-stu-id="72896-126">If you want to view or copy the Azure Resource Manager template, refer to the [Save Azure Resource Manager template](devtest-lab-add-vm.md#save-azure-resource-manager-template) section, and return here when finished.</span></span>
1. <span data-ttu-id="72896-127">Válassza ki **létrehozása** a megadott virtuális gép hozzáadása a labor.</span><span class="sxs-lookup"><span data-stu-id="72896-127">Select **Create** to add the specified VM to the lab.</span></span>
1. <span data-ttu-id="72896-128">A tesztkörnyezet panel állapotát jeleníti meg a virtuális gép létrehozás - először mint **létrehozása**, majd, mint a **futtató** a virtuális gép elindítása után.</span><span class="sxs-lookup"><span data-stu-id="72896-128">The lab blade displays the status of the VM's creation - first as **Creating**, then as **Running** after the VM has been started.</span></span>


## <a name="using-a-claimable-vm"></a><span data-ttu-id="72896-129">A claimable virtuális gépek használata</span><span class="sxs-lookup"><span data-stu-id="72896-129">Using a claimable VM</span></span>

<span data-ttu-id="72896-130">A felhasználó igényelhet a virtuális gép "Claimable virtual machines" közül lépések egyikének végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="72896-130">A user can claim any VM from the list of "Claimable virtual machines" by doing one of these steps:</span></span>

* <span data-ttu-id="72896-131">"Claimable virtuális gépnek" a tesztlabor áttekintése panel alján a listából, kattintson a jobb gombbal a virtuális gépeket, a lista egyik, és válassza a **jogcím gép**.</span><span class="sxs-lookup"><span data-stu-id="72896-131">From the list of "Claimable virtual machines" at the bottom of the lab's Overview blade, right-click on one of the VMs in the list and choose **Claim machine**.</span></span>

 ![Egy adott claimable VM kérelmet.](./media/devtest-lab-add-vm/devtestlab-claim-VM.png)


* <span data-ttu-id="72896-133">Felső részén a **áttekintése** paneljén válassza **minden jogcím**.</span><span class="sxs-lookup"><span data-stu-id="72896-133">At the top of the **Overview** blade, choose **Claim any**.</span></span> <span data-ttu-id="72896-134">Egy véletlenszerű virtuális géphez claimable virtuális gépek közül van hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="72896-134">A random virtual machine is assigned from the list of claimable VMs.</span></span>

 ![Bármely claimable VM kérelmet.](./media/devtest-lab-add-vm/devtestlab-claim-any.png)


<span data-ttu-id="72896-136">Miután a felhasználó a jogcímek egy virtuális Gépet, a átkerül az "A virtuális gépnek" tartalmazó, és már nem claimable, amelyet semmilyen más felhasználó.</span><span class="sxs-lookup"><span data-stu-id="72896-136">After a user claims a VM, it is moved up into their list of "My virtual machines" and is no longer claimable by any other user.</span></span>

## <a name="next-steps"></a><span data-ttu-id="72896-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="72896-137">Next steps</span></span>
* <span data-ttu-id="72896-138">A virtuális gép létrehozása után keresztül csatlakozhat a virtuális gép kiválasztásával **Connect** a virtuális gép paneljén.</span><span class="sxs-lookup"><span data-stu-id="72896-138">Once the VM has been created, you can connect to the VM by selecting **Connect** on the VM's blade.</span></span>
* <span data-ttu-id="72896-139">Megismerkedhet a [DevTest Labs Azure Resource Manager gyorsindítási sablonok gyűjteménye](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates)</span><span class="sxs-lookup"><span data-stu-id="72896-139">Explore the [DevTest Labs Azure Resource Manager QuickStart template gallery](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates)</span></span>
