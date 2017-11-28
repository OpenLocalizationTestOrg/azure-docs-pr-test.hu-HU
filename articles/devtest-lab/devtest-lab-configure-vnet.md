---
title: "Virtuális hálózat konfigurálása a Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálja egy meglévő virtuális hálózat és az alhálózatot, és használhatja őket az Azure DevTest Labs szolgáltatásban virtuális gépen"
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
ms.openlocfilehash: 848752085729df7d98a3a4b7be36d894c12cd033
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-virtual-network-in-azure-devtest-labs"></a><span data-ttu-id="fa996-103">A Azure DevTest Labs szolgáltatásban virtuális hálózat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fa996-103">Configure a virtual network in Azure DevTest Labs</span></span>
<span data-ttu-id="fa996-104">A cikkben leírtak szerint [egy összetevőkkel rendelkező virtuális gép hozzáadása egy laborhoz](devtest-lab-add-vm-with-artifacts.md), egy tesztkörnyezetben, a virtuális gépek létrehozásakor megadhatja, hogy egy konfigurált virtuális hálózati.</span><span class="sxs-lookup"><span data-stu-id="fa996-104">As explained in the article, [Add a VM with artifacts to a lab](devtest-lab-add-vm-with-artifacts.md), when you create a VM in a lab, you can specify a configured virtual network.</span></span> <span data-ttu-id="fa996-105">Mindezt egy például az is, ha a virtuális gépek a virtuális hálózaton keresztül, expressroute-on vagy a telephelyek közötti VPN konfigurálva lett a vállalati hálózat erőforrások eléréséhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="fa996-105">One scenario for doing this is if you need to access your corpnet resources from your VMs using the virtual network that was configured with ExpressRoute or site-to-site VPN.</span></span> <span data-ttu-id="fa996-106">Az alábbi szakaszok bemutatják, hogyan lehet, hogy az elérhető, kiválaszthatja a virtuális gépek létrehozásakor adja hozzá a meglévő virtuális hálózatot a labor virtuális hálózati beállításait.</span><span class="sxs-lookup"><span data-stu-id="fa996-106">The following sections illustrate how to add your existing virtual network into a lab's Virtual Network settings so that it is available to choose when creating VMs.</span></span>

## <a name="configure-a-virtual-network-for-a-lab-using-the-azure-portal"></a><span data-ttu-id="fa996-107">Virtuális hálózat az Azure portál használatával labor beállítása</span><span class="sxs-lookup"><span data-stu-id="fa996-107">Configure a virtual network for a lab using the Azure portal</span></span>
<span data-ttu-id="fa996-108">A következő lépések végigvezetik hozzáadása egy meglévő virtuális hálózat (és alhálózati) a laborkörnyezetben, hogy az azonos, amikor a virtuális gép létrehozásakor használható.</span><span class="sxs-lookup"><span data-stu-id="fa996-108">The following steps walk you through adding an existing virtual network (and subnet) to a lab so that it can be used when creating a VM in the same lab.</span></span> 

1. <span data-ttu-id="fa996-109">Jelentkezzen be az [Azure Portalra](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="fa996-109">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="fa996-110">Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** a listából.</span><span class="sxs-lookup"><span data-stu-id="fa996-110">Select **More Services**, and then select **DevTest Labs** from the list.</span></span>
3. <span data-ttu-id="fa996-111">Válassza ki a kívánt labor labs listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="fa996-111">From the list of labs, select the desired lab.</span></span> 
4. <span data-ttu-id="fa996-112">A labor paneljén válassza **konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="fa996-112">On the lab's blade, select **Configuration**.</span></span>
5. <span data-ttu-id="fa996-113">A tesztlabor a **konfigurációs** panelen válassza **virtuális hálózatok**.</span><span class="sxs-lookup"><span data-stu-id="fa996-113">On the lab's **Configuration** blade, select **Virtual networks**.</span></span>
6. <span data-ttu-id="fa996-114">Az a **virtuális hálózatok** panelen, megjelenik az aktuális tesztkörnyezetben, valamint az alapértelmezett virtuális hálózat a tesztkörnyezethez létrehozott konfigurált virtuális hálózatokból álló listát.</span><span class="sxs-lookup"><span data-stu-id="fa996-114">On the **Virtual networks** blade, you see a list of virtual networks configured for the current lab as well as the default virtual network that is created for your lab.</span></span> 
7. <span data-ttu-id="fa996-115">Válassza ki **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="fa996-115">Select **+ Add**.</span></span>
   
    ![Meglévő virtuális hálózat hozzáadása a tesztkörnyezet](./media/devtest-lab-configure-vnet/lab-settings-vnet-add.png)
8. <span data-ttu-id="fa996-117">Az a **virtuális hálózati** panelen válassza **[Select virtuális hálózati]**.</span><span class="sxs-lookup"><span data-stu-id="fa996-117">On the **Virtual network** blade, select **[Select virtual network]**.</span></span>
   
    ![Válassza ki a meglévő virtuális hálózat](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet1.png)
9. <span data-ttu-id="fa996-119">Az a **válasszon virtuális hálózati** panelen válassza ki a kívánt virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="fa996-119">On the **Choose virtual network** blade, select the desired virtual network.</span></span> <span data-ttu-id="fa996-120">A panel látható a virtuális hálózatok alatti ugyanabban a régióban, mint a labor az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="fa996-120">The blade shows all the virtual networks that are under the same region in the subscription as the lab.</span></span>  
10. <span data-ttu-id="fa996-121">Miután kiválasztott egy virtuális hálózatot, a rendszer visszairányítja a **virtuális hálózati** kattintson a panel alján a listában az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="fa996-121">After selecting a virtual network, you are returned to the **Virtual network** Click the subnet in the list at the bottom of the blade.</span></span>

    ![Alhálózati listája](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet2.png)
    
    <span data-ttu-id="fa996-123">A tesztkörnyezet alhálózati panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="fa996-123">The Lab Subnet blade is displayed.</span></span>

    ![Labor alhálózati panel](./media/devtest-lab-configure-vnet/lab-subnet.png)

11. <span data-ttu-id="fa996-125">Adjon meg egy **tesztkörnyezet alhálózatának nevét**.</span><span class="sxs-lookup"><span data-stu-id="fa996-125">Specify a **Lab subnet name**.</span></span>
12. <span data-ttu-id="fa996-126">Virtuális gép létrehozása, amikor használandó alhálózat engedélyezéséhez válassza **használja a virtuális gépek létrehozásához**.</span><span class="sxs-lookup"><span data-stu-id="fa996-126">To allow a subnet to be used in lab VM creation, select **Use in virtual machine creation**.</span></span>
13. <span data-ttu-id="fa996-127">Ahhoz, hogy egy [nyilvános IP-cím megosztott](devtest-lab-shared-ip.md), jelölje be **engedélyezése megosztott nyilvános IP-cím**.</span><span class="sxs-lookup"><span data-stu-id="fa996-127">To enable a [shared public IP address](devtest-lab-shared-ip.md), select **Enable shared public IP**.</span></span>
14. <span data-ttu-id="fa996-128">Az alhálózat nyilvános IP-címek engedélyezéséhez válassza **nyilvános IP-létrehozásának engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="fa996-128">To allow public IP addresses in a subnet, select **Allow public IP creation**.</span></span>
15. <span data-ttu-id="fa996-129">Az a **virtuális gépek maximális száma felhasználónként** mezőben adja meg a maximális virtuális gépek minden felhasználóhoz az egyes alhálózatokon.</span><span class="sxs-lookup"><span data-stu-id="fa996-129">In the **Maximum virtual machines per user** field, specify the maximum VMs per user for each subnet.</span></span> <span data-ttu-id="fa996-130">Ha azt szeretné, hogy a virtuális gépek korlátlan számú, hagyja üresen ezt a mezőt.</span><span class="sxs-lookup"><span data-stu-id="fa996-130">If you want an unrestricted number of VMs, leave this field blank.</span></span>
16. <span data-ttu-id="fa996-131">Válassza ki **OK** a labor alhálózati panel bezárásához.</span><span class="sxs-lookup"><span data-stu-id="fa996-131">Select **OK** to close the Lab Subnet blade.</span></span>
17. <span data-ttu-id="fa996-132">Válassza ki **mentése** a virtuális hálózat panel bezárásához.</span><span class="sxs-lookup"><span data-stu-id="fa996-132">Select **Save** to close the Virtual network blade.</span></span>
18. <span data-ttu-id="fa996-133">Most, hogy a virtuális hálózat van konfigurálva, akkor jelölhető ki a virtuális gépek létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="fa996-133">Now that the virtual network is configured, it can be selected when creating a VM.</span></span> 
    <span data-ttu-id="fa996-134">Hozzon létre egy virtuális Gépet, és adjon meg egy virtuális hálózatot, a cikkben találhat [egy összetevőkkel rendelkező virtuális gép hozzáadása egy laborhoz](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="fa996-134">To see how to create a VM and specify a virtual network, refer to the article, [Add a VM with artifacts to a lab](devtest-lab-add-vm-with-artifacts.md).</span></span> 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="fa996-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fa996-135">Next steps</span></span>
<span data-ttu-id="fa996-136">Miután hozzáadta a kívánt virtuális hálózat a tesztkörnyezet, a következő lépés, hogy [a virtuális gépek hozzáadása a labor](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="fa996-136">Once you have added the desired virtual network to your lab, the next step is to [add a VM to your lab](devtest-lab-add-vm-with-artifacts.md).</span></span>

