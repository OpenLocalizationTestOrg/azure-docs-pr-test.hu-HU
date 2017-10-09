---
title: "a Azure DevTest Labs szolgáltatásban virtuális hálózat aaaConfigure |} Microsoft Docs"
description: "Megtudhatja, hogyan tooconfigure egy meglévő virtuális hálózat és az alhálózatot, és felhasználja az Azure DevTest Labs szolgáltatásban virtuális gépen"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 6cda99c2-b87e-4047-90a0-5df10d8e9e14
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2017
ms.author: tarcher
ms.openlocfilehash: a11ce8315e3c540e44aeacc9c5ee3dde014d4621
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-virtual-network-in-azure-devtest-labs"></a><span data-ttu-id="43f50-103">A Azure DevTest Labs szolgáltatásban virtuális hálózat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="43f50-103">Configure a virtual network in Azure DevTest Labs</span></span>
<span data-ttu-id="43f50-104">Hello a cikkben leírtak szerint [adja hozzá a virtuális gép és az összetevők tooa labor](devtest-lab-add-vm-with-artifacts.md), egy tesztkörnyezetben, a virtuális gépek létrehozásakor megadhatja, hogy egy konfigurált virtuális hálózati.</span><span class="sxs-lookup"><span data-stu-id="43f50-104">As explained in hello article, [Add a VM with artifacts tooa lab](devtest-lab-add-vm-with-artifacts.md), when you create a VM in a lab, you can specify a configured virtual network.</span></span> <span data-ttu-id="43f50-105">Mindezt egy például az is, ha tooaccess van szüksége a virtuális gépek használata a vállalati hálózat erőforrások hello virtuális hálózati telephelyek közötti VPN és ExpressRoute konfigurált.</span><span class="sxs-lookup"><span data-stu-id="43f50-105">One scenario for doing this is if you need tooaccess your corpnet resources from your VMs using hello virtual network that was configured with ExpressRoute or site-to-site VPN.</span></span> <span data-ttu-id="43f50-106">hello következő szakaszok bemutatják, hogyan tooadd be a labor virtuális hálózati beállításait, hogy elérhető toochoose virtuális gépek létrehozásakor a meglévő virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="43f50-106">hello following sections illustrate how tooadd your existing virtual network into a lab's Virtual Network settings so that it is available toochoose when creating VMs.</span></span>

## <a name="configure-a-virtual-network-for-a-lab-using-hello-azure-portal"></a><span data-ttu-id="43f50-107">Virtuális hálózat hello Azure-portál használatával labor beállítása</span><span class="sxs-lookup"><span data-stu-id="43f50-107">Configure a virtual network for a lab using hello Azure portal</span></span>
<span data-ttu-id="43f50-108">hello következő lépések végigvezetik hozzáadása egy meglévő virtuális hálózat (és alhálózati) tooa labor, hogy a virtuális gép létrehozásakor a hello használható azonos labor.</span><span class="sxs-lookup"><span data-stu-id="43f50-108">hello following steps walk you through adding an existing virtual network (and subnet) tooa lab so that it can be used when creating a VM in hello same lab.</span></span> 

1. <span data-ttu-id="43f50-109">Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="43f50-109">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="43f50-110">Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.</span><span class="sxs-lookup"><span data-stu-id="43f50-110">Select **More Services**, and then select **DevTest Labs** from hello list.</span></span>
3. <span data-ttu-id="43f50-111">Labs hello listában jelölje ki hello kívánt labor.</span><span class="sxs-lookup"><span data-stu-id="43f50-111">From hello list of labs, select hello desired lab.</span></span> 
4. <span data-ttu-id="43f50-112">Hello labor paneljén válassza **konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="43f50-112">On hello lab's blade, select **Configuration**.</span></span>
5. <span data-ttu-id="43f50-113">A hello labor **konfigurációs** panelen válassza **virtuális hálózatok**.</span><span class="sxs-lookup"><span data-stu-id="43f50-113">On hello lab's **Configuration** blade, select **Virtual networks**.</span></span>
6. <span data-ttu-id="43f50-114">A hello **virtuális hálózatok** panelen konfigurált hello aktuális tesztkörnyezetben, valamint a hello alapértelmezett virtuális hálózat a tesztkörnyezethez létrehozott virtuális hálózatok listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="43f50-114">On hello **Virtual networks** blade, you see a list of virtual networks configured for hello current lab as well as hello default virtual network that is created for your lab.</span></span> 
7. <span data-ttu-id="43f50-115">Válassza ki **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="43f50-115">Select **+ Add**.</span></span>
   
    ![Egy meglévő virtuális hálózat tooyour labor hozzáadása](./media/devtest-lab-configure-vnet/lab-settings-vnet-add.png)
8. <span data-ttu-id="43f50-117">A hello **virtuális hálózati** panelen válassza **[Select virtuális hálózati]**.</span><span class="sxs-lookup"><span data-stu-id="43f50-117">On hello **Virtual network** blade, select **[Select virtual network]**.</span></span>
   
    ![Válassza ki a meglévő virtuális hálózat](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet1.png)
9. <span data-ttu-id="43f50-119">A hello **válasszon virtuális hálózati** panelen, jelölje be hello kívánt virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="43f50-119">On hello **Choose virtual network** blade, select hello desired virtual network.</span></span> <span data-ttu-id="43f50-120">hello panelen látható minden hello virtuális hálózaton vannak a hello azonos hello előfizetés hello lab-régiót.</span><span class="sxs-lookup"><span data-stu-id="43f50-120">hello blade shows all hello virtual networks that are under hello same region in hello subscription as hello lab.</span></span>  
10. <span data-ttu-id="43f50-121">Miután kiválasztott egy virtuális hálózatot, amelyről toohello **virtuális hálózati** hello alhálózati hello listában hello hello panel alsó részén kattintson.</span><span class="sxs-lookup"><span data-stu-id="43f50-121">After selecting a virtual network, you are returned toohello **Virtual network** Click hello subnet in hello list at hello bottom of hello blade.</span></span>

    ![Alhálózati listája](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet2.png)
    
    <span data-ttu-id="43f50-123">hello labor alhálózati panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="43f50-123">hello Lab Subnet blade is displayed.</span></span>

    ![Labor alhálózati panel](./media/devtest-lab-configure-vnet/lab-subnet.png)

11. <span data-ttu-id="43f50-125">Adjon meg egy **tesztkörnyezet alhálózatának nevét**.</span><span class="sxs-lookup"><span data-stu-id="43f50-125">Specify a **Lab subnet name**.</span></span>
12. <span data-ttu-id="43f50-126">Válasszon egy virtuális gép létrehozása, a tesztkörnyezetben használt alhálózati toobe tooallow **használja a virtuális gépek létrehozásához**.</span><span class="sxs-lookup"><span data-stu-id="43f50-126">tooallow a subnet toobe used in lab VM creation, select **Use in virtual machine creation**.</span></span>
13. <span data-ttu-id="43f50-127">tooenable egy [nyilvános IP-cím megosztott](devtest-lab-shared-ip.md), jelölje be **engedélyezése megosztott nyilvános IP-cím**.</span><span class="sxs-lookup"><span data-stu-id="43f50-127">tooenable a [shared public IP address](devtest-lab-shared-ip.md), select **Enable shared public IP**.</span></span>
14. <span data-ttu-id="43f50-128">Válassza ki a nyilvános IP-címek tooallow az alhálózat, **nyilvános IP-létrehozásának engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="43f50-128">tooallow public IP addresses in a subnet, select **Allow public IP creation**.</span></span>
15. <span data-ttu-id="43f50-129">A hello **virtuális gépek maximális száma felhasználónként** mezőben adja meg az egyes alhálózatokon felhasználónkénti maximális virtuális gépek hello.</span><span class="sxs-lookup"><span data-stu-id="43f50-129">In hello **Maximum virtual machines per user** field, specify hello maximum VMs per user for each subnet.</span></span> <span data-ttu-id="43f50-130">Ha azt szeretné, hogy a virtuális gépek korlátlan számú, hagyja üresen ezt a mezőt.</span><span class="sxs-lookup"><span data-stu-id="43f50-130">If you want an unrestricted number of VMs, leave this field blank.</span></span>
16. <span data-ttu-id="43f50-131">Válassza ki **OK** tooclose hello labor alhálózati panelen.</span><span class="sxs-lookup"><span data-stu-id="43f50-131">Select **OK** tooclose hello Lab Subnet blade.</span></span>
17. <span data-ttu-id="43f50-132">Válassza ki **mentése** tooclose hello virtuális hálózat panel.</span><span class="sxs-lookup"><span data-stu-id="43f50-132">Select **Save** tooclose hello Virtual network blade.</span></span>
18. <span data-ttu-id="43f50-133">Most, hogy hello virtuális hálózat van konfigurálva, akkor jelölhető ki a virtuális gépek létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="43f50-133">Now that hello virtual network is configured, it can be selected when creating a VM.</span></span> 
    <span data-ttu-id="43f50-134">toosee hogyan toocreate a virtuális gépek és adjon meg egy virtuális hálózatot, tekintse meg a toohello cikk [adja hozzá a virtuális gép és az összetevők tooa labor](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="43f50-134">toosee how toocreate a VM and specify a virtual network, refer toohello article, [Add a VM with artifacts tooa lab](devtest-lab-add-vm-with-artifacts.md).</span></span> 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="43f50-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="43f50-135">Next steps</span></span>
<span data-ttu-id="43f50-136">Hello szükséges virtuális hálózati tooyour labor hozzáadása után hello tovább túl van-e[VM tooyour labor hozzáadása](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="43f50-136">Once you have added hello desired virtual network tooyour lab, hello next step is too[add a VM tooyour lab](devtest-lab-add-vm-with-artifacts.md).</span></span>

