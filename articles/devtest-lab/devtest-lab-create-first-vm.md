---
title: "aaaCreate az első virtuális gép egy tesztkörnyezetben, a Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate az első virtuális gép egy tesztkörnyezetben, a Azure DevTest Labs szolgáltatásban"
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
ms.openlocfilehash: 4c3257efca9be6fdd190eaac1db731464e07fcfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-vm-in-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="7e3d2-103">Az első virtuális gép létrehozása az Azure DevTest Labs szolgáltatásban egy tesztkörnyezetben</span><span class="sxs-lookup"><span data-stu-id="7e3d2-103">Create your first VM in a lab in Azure DevTest Labs</span></span>

<span data-ttu-id="7e3d2-104">Kezdetben DevTest Labs eléréséhez, és szeretné, hogy az első virtuális gép toocreate, akkor lesz valószínűleg megteheti segítségével előre betöltött [alap Piactéri lemezképhez](devtest-lab-configure-marketplace-images.md).</span><span class="sxs-lookup"><span data-stu-id="7e3d2-104">When you initially access DevTest Labs and want toocreate your first VM, you will likely do so using a pre-loaded [base marketplace image](devtest-lab-configure-marketplace-images.md).</span></span> <span data-ttu-id="7e3d2-105">Később a is lesz a képes toochoose egy [egyéni lemezképet, és egy képletet](devtest-lab-add-vm.md) további virtuális gépek létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="7e3d2-105">Later on, you'll also be able toochoose from a [custom image and a formula](devtest-lab-add-vm.md) when creating more VMs.</span></span> 

<span data-ttu-id="7e3d2-106">Ez az oktatóanyag bemutatja, hogyan hello Azure portál tooadd használatával a virtuális gép első tooa labor a DevTest Labs szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="7e3d2-106">This tutorial walks you through using hello Azure portal tooadd your first VM tooa lab in DevTest Labs.</span></span>

## <a name="steps-tooadd-your-first-vm-tooa-lab-in-azure-devtest-labs"></a><span data-ttu-id="7e3d2-107">Lépések tooadd az első virtuális gép tooa tesztlabor a Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="7e3d2-107">Steps tooadd your first VM tooa lab in Azure DevTest Labs</span></span>
1. <span data-ttu-id="7e3d2-108">Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="7e3d2-108">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
1. <span data-ttu-id="7e3d2-109">Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.</span><span class="sxs-lookup"><span data-stu-id="7e3d2-109">Select **More Services**, and then select **DevTest Labs** from hello list.</span></span>
1. <span data-ttu-id="7e3d2-110">Labs hello listáról válassza ki a használni kívánt virtuális gép toocreate hello hello labor.</span><span class="sxs-lookup"><span data-stu-id="7e3d2-110">From hello list of labs, select hello lab in which you want toocreate hello VM.</span></span>  
1. <span data-ttu-id="7e3d2-111">A hello labor **áttekintése** panelen válassza **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="7e3d2-111">On hello lab's **Overview** blade, select **+ Add**.</span></span>  

    ![Adja hozzá a virtuális gép gomb](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. <span data-ttu-id="7e3d2-113">A hello **base válasszon** panelen válassza a Piactéri lemezképet a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="7e3d2-113">On hello **Choose a base** blade, select a marketplace image for hello VM.</span></span>
1. <span data-ttu-id="7e3d2-114">A hello **virtuális gép** panelen hello hello új virtuális gép nevét adja meg **virtuálisgép-nevet** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="7e3d2-114">On hello **Virtual machine** blade, enter a name for hello new virtual machine in hello **Virtual machine name** text box.</span></span>

    ![Labor VM panel](./media/devtest-lab-add-vm/devtestlab-lab-add-first-vm.png)

1. <span data-ttu-id="7e3d2-116">Adjon meg egy **felhasználónév** kap, amely rendszergazdai jogosultságokkal hello virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="7e3d2-116">Enter a **User Name** that is granted administrator privileges on hello virtual machine.</span></span>  
1. <span data-ttu-id="7e3d2-117">Címkével hello szövegmezőbe írja be a jelszót **írjon be egy értéket**.</span><span class="sxs-lookup"><span data-stu-id="7e3d2-117">Enter a password in hello text field labeled **Type a value**.</span></span>
1. <span data-ttu-id="7e3d2-118">Hello **virtuális gép lemeztípus** határozza meg, hogy milyen típusú hello virtuális gépek hello tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="7e3d2-118">hello **Virtual machine disk type** determines which storage disk type is allowed for hello virtual machines in hello lab.</span></span>
1. <span data-ttu-id="7e3d2-119">Válassza ki **virtuálisgép-méret** hello közül előre meg hello Processzormagok, a RAM memória méretét és a merevlemez méretének hello hello VM toocreate elemekre.</span><span class="sxs-lookup"><span data-stu-id="7e3d2-119">Select **Virtual machine size** and select one of hello predefined items that specify hello processor cores, RAM size, and hello hard drive size of hello VM toocreate.</span></span>
1. <span data-ttu-id="7e3d2-120">Válassza ki **összetevők** és - műtermék - hello listából válassza ki és konfigurálja, amelyet az tooadd toohello alapjául szolgáló lemezképhez hello összetevők.</span><span class="sxs-lookup"><span data-stu-id="7e3d2-120">Select **Artifacts** and - from hello list of artifacts - select and configure hello artifacts that you want tooadd toohello base image.</span></span>
    <span data-ttu-id="7e3d2-121">**Megjegyzés:** Ha új tooDevTest Labs vagy konfigurálása összetevők, tekintse meg a toohello [hozzáadása egy meglévő virtuális gép összetevő tooa](./devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) szakaszt, és térjen vissza ide befejezésekor.</span><span class="sxs-lookup"><span data-stu-id="7e3d2-121">**Note:** If you're new tooDevTest Labs or configuring artifacts, refer toohello [Add an existing artifact tooa VM](./devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) section, and then return here when finished.</span></span>
1. <span data-ttu-id="7e3d2-122">Válassza ki **létrehozása** tooadd hello megadott virtuális gép toohello labor.</span><span class="sxs-lookup"><span data-stu-id="7e3d2-122">Select **Create** tooadd hello specified VM toohello lab.</span></span>

   <span data-ttu-id="7e3d2-123">hello labor panel állapotát jeleníti meg az hello hello virtuális gép létrehozás - először mint **létrehozása**, majd, mint a **futtató** hello virtuális gép elindítása után.</span><span class="sxs-lookup"><span data-stu-id="7e3d2-123">hello lab blade displays hello status of hello VM's creation - first as **Creating**, then as **Running** after hello VM has been started.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7e3d2-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7e3d2-124">Next steps</span></span>
* <span data-ttu-id="7e3d2-125">Egyszer hello a virtuális gép létrehozása, csatlakoztathatja toohello virtuális gép kiválasztásával **Connect** hello VM panelen.</span><span class="sxs-lookup"><span data-stu-id="7e3d2-125">Once hello VM has been created, you can connect toohello VM by selecting **Connect** on hello VM's blade.</span></span>
* <span data-ttu-id="7e3d2-126">Tekintse meg [VM tooa labor hozzáadása](devtest-lab-add-vm.md) bővebb információt a további virtuális gépek hozzáadása a tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="7e3d2-126">Check out [Add a VM tooa lab](devtest-lab-add-vm.md) for more complete info about adding subsequent VMs in your lab.</span></span>
* <span data-ttu-id="7e3d2-127">Fedezze fel hello [Office DevTest Labs Azure Resource Manager gyorsindítási sablonok](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates).</span><span class="sxs-lookup"><span data-stu-id="7e3d2-127">Explore hello [DevTest Labs Azure Resource Manager QuickStart template gallery](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates).</span></span>
