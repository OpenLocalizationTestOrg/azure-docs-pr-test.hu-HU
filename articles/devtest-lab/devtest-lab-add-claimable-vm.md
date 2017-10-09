---
title: "aaaAdd egy claimable VM tooa, amikor a Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Megtudhatja, hogyan tooadd egy claimable virtuális gép tooa, amikor a Azure DevTest Labs szolgáltatásban"
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
ms.openlocfilehash: fe6385ae2e59b9636b82aec250dc3a1f8a40ba5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-claimable-vm-tooa-lab-in-azure-devtest-labs"></a><span data-ttu-id="f4716-103">Az Azure DevTest Labs claimable VM tooa labor hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f4716-103">Add a claimable VM tooa lab in Azure DevTest Labs</span></span>
<span data-ttu-id="f4716-104">Egy virtuális gép claimable tooa, amikor hozzáadja egy hasonló módon toohow meg [szabványos virtuális gép hozzáadása](devtest-lab-add-vm.md) – a egy *alap* , amely vagy egy [egyéni lemezkép](devtest-lab-create-template.md), [képlet](devtest-lab-manage-formulas.md), vagy [Piactéri lemezképhez](devtest-lab-configure-marketplace-images.md).</span><span class="sxs-lookup"><span data-stu-id="f4716-104">You add a claimable VM tooa lab in a similar manner toohow you [add a standard VM](devtest-lab-add-vm.md) – from a *base* that is either a [custom image](devtest-lab-create-template.md), [formula](devtest-lab-manage-formulas.md), or [Marketplace image](devtest-lab-configure-marketplace-images.md).</span></span> <span data-ttu-id="f4716-105">Ez az oktatóanyag végigvezeti az Azure portál tooadd hello segítségével a DevTest Labs szolgáltatásban claimable VM tooa labor, és a felhasználó a következő virtuális gép tooclaim hello hello menetét mutatja.</span><span class="sxs-lookup"><span data-stu-id="f4716-105">This tutorial walks you through using hello Azure portal tooadd a claimable VM tooa lab in DevTest Labs, and shows hello process a user follows tooclaim hello VM.</span></span>

> [!NOTE]
> <span data-ttu-id="f4716-106">Ha a labor virtuális gépeken keresztül telepít [Azure Resource Manager-sablonok](devtest-lab-create-environment-from-arm.md), claimable virtuális gépek létrehozásához hello beállítása **allowClaim** tulajdonság tootrue hello tulajdonságok szakaszban.</span><span class="sxs-lookup"><span data-stu-id="f4716-106">If you deploy lab VMs through [Azure Resource Manager templates](devtest-lab-create-environment-from-arm.md), you can create claimable VMs by setting hello **allowClaim** property tootrue in hello properties section.</span></span>
>
>

## <a name="steps-tooadd-a-claimable-vm-tooa-lab-in-azure-devtest-labs"></a><span data-ttu-id="f4716-107">Lépéseket tooadd egy claimable VM tooa, amikor a Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="f4716-107">Steps tooadd a claimable VM tooa lab in Azure DevTest Labs</span></span>
1. <span data-ttu-id="f4716-108">Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="f4716-108">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
1. <span data-ttu-id="f4716-109">Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.</span><span class="sxs-lookup"><span data-stu-id="f4716-109">Select **More Services**, and then select **DevTest Labs** from hello list.</span></span>
1. <span data-ttu-id="f4716-110">Labs hello listában jelölje ki hello labor kívánja a toocreate hello claimable virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="f4716-110">From hello list of labs, select hello lab in which you want toocreate hello claimable VM.</span></span>  
1. <span data-ttu-id="f4716-111">A hello labor **áttekintése** panelen válassza **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="f4716-111">On hello lab's **Overview** blade, select **+ Add**.</span></span>  

    ![Adja hozzá a virtuális gép gomb](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. <span data-ttu-id="f4716-113">A hello **base válasszon** panelen válassza ki a megfelelő virtuális gép hello alapja.</span><span class="sxs-lookup"><span data-stu-id="f4716-113">On hello **Choose a base** blade, select a base for hello VM.</span></span>
1. <span data-ttu-id="f4716-114">A hello **virtuális gép** panelen hello hello új virtuális gép nevét adja meg **virtuálisgép-nevet** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="f4716-114">On hello **Virtual machine** blade, enter a name for hello new virtual machine in hello **Virtual machine name** text box.</span></span>

    ![Labor VM panel](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. <span data-ttu-id="f4716-116">Adjon meg egy **felhasználónév** kap, amely rendszergazdai jogosultságokkal hello virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="f4716-116">Enter a **User Name** that is granted administrator privileges on hello virtual machine.</span></span>  
1. <span data-ttu-id="f4716-117">Ha azt szeretné, hogy toouse jelszó tárolja a [titkos tároló](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store), jelölje be **mentett titkos kulcs használata**, és adja meg a kulcs értéke, amely megfelel a tooyour titkos (jelszó).</span><span class="sxs-lookup"><span data-stu-id="f4716-117">If you want toouse a password stored in your [secret store](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store), select **Use a saved secret**, and specify a key value that corresponds tooyour secret (password).</span></span> <span data-ttu-id="f4716-118">Ellenkező esetben feliratú hello szövegmezőbe írja be a jelszót **írjon be egy értéket**.</span><span class="sxs-lookup"><span data-stu-id="f4716-118">Otherwise, enter a password in hello text field labeled **Type a value**.</span></span>
1. <span data-ttu-id="f4716-119">Hello **virtuális gép lemeztípus** határozza meg, hogy milyen típusú hello virtuális gépek hello tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="f4716-119">hello **Virtual machine disk type** determines which storage disk type is allowed for hello virtual machines in hello lab.</span></span>
1. <span data-ttu-id="f4716-120">Válassza ki **virtuálisgép-méret** hello közül előre meg hello Processzormagok, a RAM memória méretét és a merevlemez méretének hello hello VM toocreate elemekre.</span><span class="sxs-lookup"><span data-stu-id="f4716-120">Select **Virtual machine size** and select one of hello predefined items that specify hello processor cores, RAM size, and hello hard drive size of hello VM toocreate.</span></span>
1. <span data-ttu-id="f4716-121">Válassza ki **összetevők** és összetevők hello listából válassza ki és konfigurálja, amelyet az tooadd toohello alapjául szolgáló lemezképhez hello összetevők.</span><span class="sxs-lookup"><span data-stu-id="f4716-121">Select **Artifacts** and from hello list of artifacts, select and configure hello artifacts that you want tooadd toohello base image.</span></span> <span data-ttu-id="f4716-122">Ha új tooDevTest Labs vagy konfigurálása összetevők, tekintse meg a toohello [hozzáadása egy meglévő virtuális gép összetevő tooa](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) szakaszt, és térjen vissza ide befejezésekor.</span><span class="sxs-lookup"><span data-stu-id="f4716-122">If you're new tooDevTest Labs or configuring artifacts, refer toohello [Add an existing artifact tooa VM](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) section, and then return here when finished.</span></span>
1. <span data-ttu-id="f4716-123">Válassza ki **speciális beállítások** tooconfigure hello Virtuálisgép-hálózati beállítások és a lejárati beállítások.</span><span class="sxs-lookup"><span data-stu-id="f4716-123">Select **Advanced settings** tooconfigure hello VM's network options and expiration options.</span></span> <span data-ttu-id="f4716-124">A **beállítások jogcím**, válassza a **Igen** toomake hello gép claimable.</span><span class="sxs-lookup"><span data-stu-id="f4716-124">Under **Claim options**, choose **Yes** toomake hello machine claimable.</span></span>

  ![Válassza ki a toomake hello VM claimable.](./media/devtest-lab-add-vm/devtestlab-claim-VM-option.png)

1. <span data-ttu-id="f4716-126">Ha szeretné, hogy tooview vagy hello Azure Resource Manager sablon másolása, tekintse meg a toohello [mentése Azure Resource Manager-sablon](devtest-lab-add-vm.md#save-azure-resource-manager-template) szakaszt, és térjen vissza ide, ha befejeződött.</span><span class="sxs-lookup"><span data-stu-id="f4716-126">If you want tooview or copy hello Azure Resource Manager template, refer toohello [Save Azure Resource Manager template](devtest-lab-add-vm.md#save-azure-resource-manager-template) section, and return here when finished.</span></span>
1. <span data-ttu-id="f4716-127">Válassza ki **létrehozása** tooadd hello megadott virtuális gép toohello labor.</span><span class="sxs-lookup"><span data-stu-id="f4716-127">Select **Create** tooadd hello specified VM toohello lab.</span></span>
1. <span data-ttu-id="f4716-128">hello labor panel állapotát jeleníti meg az hello hello virtuális gép létrehozás - először mint **létrehozása**, majd, mint a **futtató** hello virtuális gép elindítása után.</span><span class="sxs-lookup"><span data-stu-id="f4716-128">hello lab blade displays hello status of hello VM's creation - first as **Creating**, then as **Running** after hello VM has been started.</span></span>


## <a name="using-a-claimable-vm"></a><span data-ttu-id="f4716-129">A claimable virtuális gépek használata</span><span class="sxs-lookup"><span data-stu-id="f4716-129">Using a claimable VM</span></span>

<span data-ttu-id="f4716-130">A felhasználó igényelhet a virtuális gép "Claimable virtual machines" hello listája lépések egyikének végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="f4716-130">A user can claim any VM from hello list of "Claimable virtual machines" by doing one of these steps:</span></span>

* <span data-ttu-id="f4716-131">Hello listájából "Claimable virtual machines" hello hello tesztlabor áttekintése panel alsó részén, kattintson a jobb gombbal a virtuális gépek hello hello lista egyik, és válassza a **jogcím gép**.</span><span class="sxs-lookup"><span data-stu-id="f4716-131">From hello list of "Claimable virtual machines" at hello bottom of hello lab's Overview blade, right-click on one of hello VMs in hello list and choose **Claim machine**.</span></span>

 ![Egy adott claimable VM kérelmet.](./media/devtest-lab-add-vm/devtestlab-claim-VM.png)


* <span data-ttu-id="f4716-133">Hello hello tetején **áttekintése** paneljén válassza **minden jogcím**.</span><span class="sxs-lookup"><span data-stu-id="f4716-133">At hello top of hello **Overview** blade, choose **Claim any**.</span></span> <span data-ttu-id="f4716-134">Egy véletlenszerű virtuális géphez claimable virtuális gépek listájából hello van hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="f4716-134">A random virtual machine is assigned from hello list of claimable VMs.</span></span>

 ![Bármely claimable VM kérelmet.](./media/devtest-lab-add-vm/devtestlab-claim-any.png)


<span data-ttu-id="f4716-136">Miután a felhasználó a jogcímek egy virtuális Gépet, a átkerül az "A virtuális gépnek" tartalmazó, és már nem claimable, amelyet semmilyen más felhasználó.</span><span class="sxs-lookup"><span data-stu-id="f4716-136">After a user claims a VM, it is moved up into their list of "My virtual machines" and is no longer claimable by any other user.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f4716-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f4716-137">Next steps</span></span>
* <span data-ttu-id="f4716-138">Egyszer hello a virtuális gép létrehozása, csatlakoztathatja toohello virtuális gép kiválasztásával **Connect** hello VM panelen.</span><span class="sxs-lookup"><span data-stu-id="f4716-138">Once hello VM has been created, you can connect toohello VM by selecting **Connect** on hello VM's blade.</span></span>
* <span data-ttu-id="f4716-139">Fedezze fel hello [DevTest Labs Azure Resource Manager gyorsindítási sablonok gyűjteménye](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates)</span><span class="sxs-lookup"><span data-stu-id="f4716-139">Explore hello [DevTest Labs Azure Resource Manager QuickStart template gallery](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates)</span></span>
