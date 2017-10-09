---
title: "Azure-portálon aaaOpen portok tooa VM használatával hello |} Microsoft Docs"
description: "Megtudhatja, hogyan tooopen port / hozzon létre egy végpont tooyour VM a Windows hello resource manager üzembe helyezési modellben hello Azure portál használatával"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: f7cf0319-5ee7-435e-8f94-c484bf5ee6f1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: iainfou
ms.openlocfilehash: aba789c65254651899aa688f256fe616c3d0126d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooopen-ports-tooa-virtual-machine-with-hello-azure-portal"></a><span data-ttu-id="d2543-103">Hogyan tooopen portok tooa virtuális gép hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d2543-103">How tooopen ports tooa virtual machine with hello Azure portal</span></span>
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a><span data-ttu-id="d2543-104">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="d2543-104">Quick commands</span></span>
<span data-ttu-id="d2543-105">Emellett [elvégzi ezeket a lépéseket az Azure PowerShell](nsg-quickstart-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="d2543-105">You can also [perform these steps using Azure PowerShell](nsg-quickstart-powershell.md).</span></span>

<span data-ttu-id="d2543-106">Először hozzon létre a hálózati biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="d2543-106">First, create your Network Security Group.</span></span> <span data-ttu-id="d2543-107">Válasszon egy erőforráscsoportot hello portálon, válassza a **Hozzáadás**, majd keresse meg és jelölje ki **hálózati biztonsági csoport**:</span><span class="sxs-lookup"><span data-stu-id="d2543-107">Select a resource group in hello portal, choose **Add**, then search for and select **Network security group**:</span></span>

![Hálózati biztonsági csoport hozzáadása](./media/nsg-quickstart-portal/add-nsg.png)

<span data-ttu-id="d2543-109">Adja meg a hálózati biztonsági csoport nevét, válassza ki vagy hozzon létre egy erőforráscsoportot, és jelöljön ki egy helyet.</span><span class="sxs-lookup"><span data-stu-id="d2543-109">Enter a name for your Network Security Group, select or create a resource group, and select a location.</span></span> <span data-ttu-id="d2543-110">Válassza ki **létrehozása** befejezése:</span><span class="sxs-lookup"><span data-stu-id="d2543-110">Select **Create** when finished:</span></span>

![Hálózati biztonsági csoport létrehozása](./media/nsg-quickstart-portal/create-nsg.png)

<span data-ttu-id="d2543-112">Válassza ki az új hálózati biztonsági csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="d2543-112">Select your new Network Security Group.</span></span> <span data-ttu-id="d2543-113">Válassza a "Bejövő biztonsági szabályok", majd válassza ki a hello **Hozzáadás** gomb toocreate egy szabályt:</span><span class="sxs-lookup"><span data-stu-id="d2543-113">Select 'Inbound security rules', then select hello **Add** button toocreate a rule:</span></span>

![Egy bejövő forgalomra vonatkozó szabály hozzáadása](./media/nsg-quickstart-portal/add-inbound-rule.png)

<span data-ttu-id="d2543-115">Válasszon egy közös **szolgáltatás** hello legördülő menüből, például a *HTTP*.</span><span class="sxs-lookup"><span data-stu-id="d2543-115">Choose a common **Service** from hello drop-down menu, such as *HTTP*.</span></span> <span data-ttu-id="d2543-116">Igény szerint kiválaszthatja *egyéni* tooprovide egy adott portot toouse.</span><span class="sxs-lookup"><span data-stu-id="d2543-116">You can also select *Custom* tooprovide a specific port toouse.</span></span> <span data-ttu-id="d2543-117">Szükség esetén módosítsa a hello prioritású vagy neve.</span><span class="sxs-lookup"><span data-stu-id="d2543-117">If desired, change hello priority or name.</span></span> <span data-ttu-id="d2543-118">hello prioritása meghatározza hello sorrendben, amelyben a szabályok érvényesek - hello alacsonyabb hello numerikus érték, hello korábbi hello-szabály alkalmazásakor.</span><span class="sxs-lookup"><span data-stu-id="d2543-118">hello priority affects hello order in which rules are applied - hello lower hello numerical value, hello earlier hello rule is applied.</span></span> <span data-ttu-id="d2543-119">Igény szerint kiválaszthatja **speciális** a képernyő tooenter hello tetején egy adott forrás IP-blokk vagy port tartomány, például.</span><span class="sxs-lookup"><span data-stu-id="d2543-119">You can also select **Advanced** at hello top of this screen tooenter a specific source IP block or port range, for example.</span></span> <span data-ttu-id="d2543-120">Amikor elkészült, válassza ki a **OK** toocreate hello szabály:</span><span class="sxs-lookup"><span data-stu-id="d2543-120">When you are ready, select **OK** toocreate hello rule:</span></span>

![Egy bejövő forgalomra vonatkozó szabály létrehozása](./media/nsg-quickstart-portal/create-inbound-rule.png)

<span data-ttu-id="d2543-122">Az utolsó lépés a hálózati biztonsági csoport egy alhálózatot vagy egy adott hálózati illesztőn tooassociate.</span><span class="sxs-lookup"><span data-stu-id="d2543-122">Your final step is tooassociate your Network Security Group with a subnet or a specific network interface.</span></span> <span data-ttu-id="d2543-123">Hello hálózati biztonsági csoport most társítandó alhálózat.</span><span class="sxs-lookup"><span data-stu-id="d2543-123">Let's associate hello Network Security Group with a subnet.</span></span> <span data-ttu-id="d2543-124">Válassza ki **alhálózatok**, majd válassza a **társítása**:</span><span class="sxs-lookup"><span data-stu-id="d2543-124">Select **Subnets**, then choose **Associate**:</span></span>

![Hálózati biztonsági csoport társítandó alhálózat](./media/nsg-quickstart-portal/associate-subnet.png)

<span data-ttu-id="d2543-126">Válassza ki a virtuális hálózatot, majd válassza ki a megfelelő alhálózati hello:</span><span class="sxs-lookup"><span data-stu-id="d2543-126">Select your virtual network, and then select hello appropriate subnet:</span></span>

![Egy hálózati biztonsági csoportot társít a virtuális hálózat](./media/nsg-quickstart-portal/select-vnet-subnet.png)

<span data-ttu-id="d2543-128">Hálózati biztonsági csoport, egy bejövő szabályt, amely lehetővé teszi a forgalom 80-as porton, és azt egy alhálózathoz társított létre most hozott létre.</span><span class="sxs-lookup"><span data-stu-id="d2543-128">You have now created a Network Security Group, created an inbound rule that allows traffic on port 80, and associated it with a subnet.</span></span> <span data-ttu-id="d2543-129">A virtuális gépek toothat alhálózati csatlakozás 80-as porton érhetők el.</span><span class="sxs-lookup"><span data-stu-id="d2543-129">Any VMs you connect toothat subnet are reachable on port 80.</span></span>

## <a name="more-information-on-network-security-groups"></a><span data-ttu-id="d2543-130">További információ a hálózati biztonsági csoportok</span><span class="sxs-lookup"><span data-stu-id="d2543-130">More information on Network Security Groups</span></span>
<span data-ttu-id="d2543-131">hello itt gyors parancsok lehetővé teszik tooget be és a forgalom szereplő tooyour virtuális gép futtatása.</span><span class="sxs-lookup"><span data-stu-id="d2543-131">hello quick commands here allow you tooget up and running with traffic flowing tooyour VM.</span></span> <span data-ttu-id="d2543-132">Hálózati biztonsági csoportok adja meg, hány különleges szolgáltatásait és részletességgel ellenőrző tooyour erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="d2543-132">Network Security Groups provide many great features and granularity for controlling access tooyour resources.</span></span> <span data-ttu-id="d2543-133">További tudnivalók [itt szabályok létrehozása a hálózati biztonsági csoport és a hozzáférés-vezérlési lista](../../virtual-network/virtual-networks-create-nsg-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="d2543-133">You can read more about [creating a Network Security Group and ACL rules here](../../virtual-network/virtual-networks-create-nsg-arm-ps.md).</span></span>

<span data-ttu-id="d2543-134">Magas rendelkezésre állású webes alkalmazásokhoz helyezze a virtuális gépek az Azure terheléselosztó mögött.</span><span class="sxs-lookup"><span data-stu-id="d2543-134">For highly available web applications, you should place your VMs behind an Azure Load Balancer.</span></span> <span data-ttu-id="d2543-135">hello terheléselosztó osztja el a forgalmat tooVMs forgalomszűrést végez biztosító hálózati biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="d2543-135">hello load balancer distributes traffic tooVMs, with a Network Security Group that provides traffic filtering.</span></span> <span data-ttu-id="d2543-136">További információkért lásd: [hogyan tooload egyenleg Linux virtuális gépek az Azure toocreate a magas rendelkezésre állású alkalmazások](tutorial-load-balancer.md).</span><span class="sxs-lookup"><span data-stu-id="d2543-136">For more information, see [How tooload balance Linux virtual machines in Azure toocreate a highly available application](tutorial-load-balancer.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d2543-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d2543-137">Next steps</span></span>
<span data-ttu-id="d2543-138">Ebben a példában létrehozott egy egyszerű szabályt tooallow HTTP-forgalmat.</span><span class="sxs-lookup"><span data-stu-id="d2543-138">In this example, you created a simple rule tooallow HTTP traffic.</span></span> <span data-ttu-id="d2543-139">További részletes környezetek létrehozásáról a következő cikkek hello információt talál:</span><span class="sxs-lookup"><span data-stu-id="d2543-139">You can find information on creating more detailed environments in hello following articles:</span></span>

* [<span data-ttu-id="d2543-140">Az Azure Resource Manager áttekintése</span><span class="sxs-lookup"><span data-stu-id="d2543-140">Azure Resource Manager overview</span></span>](../../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="d2543-141">Mi az a hálózati biztonsági csoport (NSG)?</span><span class="sxs-lookup"><span data-stu-id="d2543-141">What is a Network Security Group (NSG)?</span></span>](../../virtual-network/virtual-networks-nsg.md)