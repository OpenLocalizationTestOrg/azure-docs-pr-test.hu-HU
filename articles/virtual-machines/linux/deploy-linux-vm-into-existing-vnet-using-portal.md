---
title: "Linux virtuális gépek aaaDeploy a meglévő hálózati Azure portállal |} Microsoft Docs"
description: "Linux virtuális gép üzembe helyezés hello portálon meglévő Azure virtuális hálózat."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 51272b8cdb1dc7f3fe768d2ebbf4ebe0d754cf19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-hello-azure-portal"></a><span data-ttu-id="02114-103">Hogyan toodeploy hello Azure-portálon a meglévő Azure virtuális hálózat a Linux virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="02114-103">How toodeploy a Linux virtual machine into an existing Azure Virtual Network with hello Azure portal</span></span>

<span data-ttu-id="02114-104">Ez a cikk bemutatja, hogyan toodeploy egy virtuális gép (VM) be egy meglévő virtuális hálózatot (VNet).</span><span class="sxs-lookup"><span data-stu-id="02114-104">This article shows you how toodeploy a virtual machine (VM) into an existing virtual network (VNet).</span></span> <span data-ttu-id="02114-105">Az Azure eszközök, például a virtuális hálózatokat és a hálózati biztonsági csoportok statikusnak kell lennie, és hosszabb élettartamú ritkán telepített erőforrások esetén.</span><span class="sxs-lookup"><span data-stu-id="02114-105">Azure assets like VNets and network security groups should be static and long lived resources that are rarely deployed.</span></span> <span data-ttu-id="02114-106">Miután a virtuális hálózaton van telepítve, azt is adhat meg újból állandó redeployments bármely kedvezőtlen hatással van a toohello infrastruktúra nélkül.</span><span class="sxs-lookup"><span data-stu-id="02114-106">Once a VNet has been deployed, it can be reused by constant redeployments without any adverse affects toohello infrastructure.</span></span> <span data-ttu-id="02114-107">Egy Vnetet, hogy egy hagyományos gondolat hardver hálózati kapcsoló - tooconfigure egyes központi telepítések kapcsoló egy új hardver nem kell.</span><span class="sxs-lookup"><span data-stu-id="02114-107">Thinking about a VNet as being a traditional hardware network switch - you would not need tooconfigure a brand new hardware switch with each deployment.</span></span>  

<span data-ttu-id="02114-108">Egy megfelelően konfigurált virtuális hálózaton folytathatja toodeploy új kiszolgálók be, hogy a VNet és újra hello VNet hello élettartama során szükséges néhány, az esetleges módosításokat.</span><span class="sxs-lookup"><span data-stu-id="02114-108">With a correctly configured VNet, you can continue toodeploy new servers into that VNet over and over with few, if any, changes required over hello life of hello VNet.</span></span>

## <a name="create-hello-resource-group"></a><span data-ttu-id="02114-109">Hello erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="02114-109">Create hello resource group</span></span>

<span data-ttu-id="02114-110">Először hozzon létre egy erőforrás csoport tooorganize mindent hoz létre ebben a forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="02114-110">First, create a resource group tooorganize everything you create in this walkthrough.</span></span> <span data-ttu-id="02114-111">Azure erőforráscsoport-sablonok kapcsolatos további információkért lásd: [Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md)</span><span class="sxs-lookup"><span data-stu-id="02114-111">For more information about Azure resource groups, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md)</span></span>

![createResourceGroup](./media/deploy-linux-vm-into-existing-vnet-using-portal/createResourceGroup.png)


## <a name="create-hello-vnet"></a><span data-ttu-id="02114-113">Hello virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="02114-113">Create hello VNet</span></span>

<span data-ttu-id="02114-114">A következő hozhat létre. a virtuális hálózat toolaunch hello a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="02114-114">Next, build a VNet toolaunch hello VMs into.</span></span> <span data-ttu-id="02114-115">hello VNet egy alhálózat és társított hálózati biztonsági csoport hello Ez az alhálózat egy későbbi lépésben.</span><span class="sxs-lookup"><span data-stu-id="02114-115">hello VNet contains one subnet and is associated with hello network security group with this subnet in a later step.</span></span>

![createVNet](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVNet.png)

## <a name="add-a-vnic-toohello-subnet"></a><span data-ttu-id="02114-117">Adjon hozzá egy virtuális hálózati toohello alhálózatot</span><span class="sxs-lookup"><span data-stu-id="02114-117">Add a VNic toohello subnet</span></span>

<span data-ttu-id="02114-118">Virtuális hálózati adapterek (VNics) fontosak, mivel toodifferent virtuális gépek is elérheti őket.</span><span class="sxs-lookup"><span data-stu-id="02114-118">Virtual network cards (VNics) are important as you can connect them toodifferent VMs.</span></span> <span data-ttu-id="02114-119">Ez a megközelítés hello virtuális hálózati tartja a statikus erőforrásként hello virtuális gépek ideiglenes lehetnek.</span><span class="sxs-lookup"><span data-stu-id="02114-119">This approach keeps hello VNic as a static resource while hello VMs can be temporary.</span></span> <span data-ttu-id="02114-120">Hozzon létre egy virtuális hálózati, és társítsa azt az hello alhálózati hello előző lépésben létrehozott.</span><span class="sxs-lookup"><span data-stu-id="02114-120">Create a VNic and associate it with hello subnet created in hello previous step.</span></span>

![createVNic](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVNic.png)

## <a name="create-hello-network-security-group"></a><span data-ttu-id="02114-122">Hello hálózati biztonsági csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="02114-122">Create hello network security group</span></span>

<span data-ttu-id="02114-123">Az Azure hálózati biztonsági csoportok állnak egyenértékű tooa tűzfal hello hálózati rétegben.</span><span class="sxs-lookup"><span data-stu-id="02114-123">Azure network security groups are equivalent tooa firewall at hello network layer.</span></span> <span data-ttu-id="02114-124">Az Azure hálózati biztonsági csoportokkal kapcsolatos további információkért lásd: [Mi az a hálózati biztonsági csoport](../../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="02114-124">For more information on Azure network security groups, see [What is a Network Security Group](../../virtual-network/virtual-networks-nsg.md).</span></span>

![createNSG](./media/deploy-linux-vm-into-existing-vnet-using-portal/createNSG.png)

## <a name="add-an-inbound-ssh-allow-rule"></a><span data-ttu-id="02114-126">Egy bejövő SSH engedélyezési szabály hozzáadása</span><span class="sxs-lookup"><span data-stu-id="02114-126">Add an inbound SSH allow rule</span></span>

<span data-ttu-id="02114-127">virtuális gép hello hozzá kell férnie a hello internet, így lehetővé téve a 22-es portot a bejövő forgalom toobe szabály továbbítja hello hálózati tooport 22 hello virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="02114-127">hello VM needs access from hello internet, so a rule allowing inbound port 22 traffic toobe passed through hello network tooport 22 on hello VM is created.</span></span>

![createInboundSSH](./media/deploy-linux-vm-into-existing-vnet-using-portal/createInboundSSH.png)

## <a name="associate-hello-nsg-with-hello-subnet"></a><span data-ttu-id="02114-129">Hello alhálózati hello NSG társítása</span><span class="sxs-lookup"><span data-stu-id="02114-129">Associate hello NSG with hello subnet</span></span>

<span data-ttu-id="02114-130">Hello virtuális hálózat és a létrehozott hello alhálózati hello alhálózati hello hálózati biztonsági csoport társítani.</span><span class="sxs-lookup"><span data-stu-id="02114-130">With hello VNet and hello subnet created, associate hello network security group with hello subnet.</span></span> <span data-ttu-id="02114-131">Lehet, hogy a hálózati biztonsági csoportok vagy egy teljes alhálózatot, vagy egy különálló virtuális hálózati társítani.</span><span class="sxs-lookup"><span data-stu-id="02114-131">Network security groups can be associated with either an entire subnet or an individual VNic.</span></span> <span data-ttu-id="02114-132">Hello tűzfal forgalom szűrésénél hello alhálózati szinten minden VNics és hello alhálózaton belüli virtuális gépek hello által védett hello hálózati biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="02114-132">With hello firewall filtering traffic at hello subnet level, all VNics and hello VMs within hello subnet are protected by hello network security group.</span></span> <span data-ttu-id="02114-133">hello más megközelítést van hello hálózati biztonsági csoport alatt csak egyetlen virtuális hálózati társított, és csak egy virtuális gép védelmét.</span><span class="sxs-lookup"><span data-stu-id="02114-133">hello other approach is hello network security group being associated with just a single VNic and protecting just one VM.</span></span>

![associateNSG](./media/deploy-linux-vm-into-existing-vnet-using-portal/associateNSG.png)


## <a name="deploy-hello-vm-into-hello-vnet-and-nsg"></a><span data-ttu-id="02114-135">Hello virtuális hálózatot a virtuális gép hello és NSG központi telepítése</span><span class="sxs-lookup"><span data-stu-id="02114-135">Deploy hello VM into hello VNet and NSG</span></span>

<span data-ttu-id="02114-136">Hello Azure-portálon, Linux virtuális gép hello használata meglévő Azure-erőforráscsoportot, virtuális hálózatot, alhálózatot és virtuális hálózati telepített toohello.</span><span class="sxs-lookup"><span data-stu-id="02114-136">Using hello Azure portal, hello Linux VM is deployed toohello existing Azure Resource Group, VNet, Subnet, and VNic.</span></span>

![createVM](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVM.png)

<span data-ttu-id="02114-138">Hello portál toochoose meglévő erőforrásokat használ, kérje a Azure toodeploy hello VM hello meglévő hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="02114-138">By using hello portal toochoose existing resources, you instruct Azure toodeploy hello VM inside hello existing network.</span></span> <span data-ttu-id="02114-139">Egy VNet és alhálózat van telepítve, ha azok statikus vagy állandó erőforráshoz, az Azure-régió, mint maradhatnak.</span><span class="sxs-lookup"><span data-stu-id="02114-139">Once a VNet and subnet have been deployed, they can be left as static or permanent resources inside your Azure region.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="02114-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="02114-140">Next steps</span></span>

* [<span data-ttu-id="02114-141">Az Azure Resource Manager sablon toocreate egy adott központi telepítés használata</span><span class="sxs-lookup"><span data-stu-id="02114-141">Use an Azure Resource Manager template toocreate a specific deployment</span></span>](../windows/cli-deploy-templates.md)
* [<span data-ttu-id="02114-142">Saját egyéni környezet létrehozása Linux virtuális gép számára közvetlenül Azure parancssori felület parancsait használva</span><span class="sxs-lookup"><span data-stu-id="02114-142">Create your own custom environment for a Linux VM using Azure CLI commands directly</span></span>](create-cli-complete.md)
* [<span data-ttu-id="02114-143">Linux virtuális gép létrehozása az Azure-ban sablonok használatával</span><span class="sxs-lookup"><span data-stu-id="02114-143">Create a Linux VM on Azure using templates</span></span>](create-ssh-secured-vm-from-template.md)
