---
title: "Hozzon létre a hálózati biztonsági csoport – az Azure portálon |} Microsoft Docs"
description: "Megtudhatja, hogyan hozhat létre és telepíthet a hálózati biztonsági csoportok az Azure portál használatával."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 5bc8fc2e-1e81-40e2-8231-0484cd5605cb
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 865032f350735d35668bb199ccf1ef3f0fae81de
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-network-security-groups-using-the-azure-portal"></a><span data-ttu-id="a38c9-103">Hozza létre a hálózati biztonsági csoportokat az Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="a38c9-103">Create network security groups using the Azure portal</span></span>

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="a38c9-104">Ez a cikk a Resource Manager-alapú üzemi modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="a38c9-104">This article covers the Resource Manager deployment model.</span></span> <span data-ttu-id="a38c9-105">Emellett [NSG-k létrehozása a klasszikus üzembe helyezési modellel](virtual-networks-create-nsg-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="a38c9-105">You can also [create NSGs in the classic deployment model](virtual-networks-create-nsg-classic-ps.md).</span></span>

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

<span data-ttu-id="a38c9-106">A minta az alábbi parancsok várt már létrehozott egy egyszerű környezetben PowerShell a fenti forgatókönyv alapján.</span><span class="sxs-lookup"><span data-stu-id="a38c9-106">The sample PowerShell commands below expect a simple environment already created based on the scenario above.</span></span> <span data-ttu-id="a38c9-107">Ha szeretné a parancsokat a jelen dokumentum megjelenített, először összeállítása a tesztkörnyezetben üzembe helyezésével [sablon](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), kattintson a **az Azure telepítéséhez**, cserélje le az alapértelmezett paraméterértékek, ha szükséges, és kövesse az utasításokat a portálon.</span><span class="sxs-lookup"><span data-stu-id="a38c9-107">If you want to run the commands as they are displayed in this document, first build the test environment by deploying [this template](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), click **Deploy to Azure**, replace the default parameter values if necessary, and follow the instructions in the portal.</span></span> <span data-ttu-id="a38c9-108">Az alábbi használata lépéseket **RG-NSG** az erőforráscsoport a sablon telepítve van a neveként.</span><span class="sxs-lookup"><span data-stu-id="a38c9-108">The steps below use **RG-NSG** as the name of the resource group the template was deployed to.</span></span>

## <a name="create-the-nsg-frontend-nsg"></a><span data-ttu-id="a38c9-109">Az NSG-előtérbeli NSG létrehozása</span><span class="sxs-lookup"><span data-stu-id="a38c9-109">Create the NSG-FrontEnd NSG</span></span>
<span data-ttu-id="a38c9-110">Létrehozásához a **NSG-előtérbeli** NSG látható a fenti forgatókönyvben kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="a38c9-110">To create the **NSG-FrontEnd** NSG as shown in the scenario above, follow the steps below.</span></span>

1. <span data-ttu-id="a38c9-111">Egy böngészőből keresse fel a http://portal.azure.com címet, majd jelentkezzen be az Azure-fiókjával, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="a38c9-111">From a browser, navigate to http://portal.azure.com and, if necessary, sign in with your Azure account.</span></span>
2. <span data-ttu-id="a38c9-112">Kattintson a **Tallózás >** > **hálózati biztonsági csoportok**.</span><span class="sxs-lookup"><span data-stu-id="a38c9-112">Click **Browse >** > **Network Security Groups**.</span></span>
   
    ![Azure portál – NSG-k](./media/virtual-networks-create-nsg-arm-pportal/figure11.png)
3. <span data-ttu-id="a38c9-114">Az a **hálózati biztonsági csoportok** panelen kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="a38c9-114">In the **Network security groups** blade, click **Add**.</span></span>
   
    ![Azure portál – NSG-k](./media/virtual-networks-create-nsg-arm-pportal/figure12.png)
4. <span data-ttu-id="a38c9-116">Az a **hálózati biztonsági csoport létrehozása** panelen, hozzon létre egy NSG nevű *NSG-előtér* a a *RG-NSG* erőforráscsoportban, és kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="a38c9-116">In the **Create network security group** blade, create an NSG named *NSG-FrontEnd* in the *RG-NSG* resource group, and then click **Create**.</span></span>
   
    ![Azure portál – NSG-k](./media/virtual-networks-create-nsg-arm-pportal/figure13.png)

## <a name="create-rules-in-an-existing-nsg"></a><span data-ttu-id="a38c9-118">Szabályok létrehozása egy létező NSG-ben</span><span class="sxs-lookup"><span data-stu-id="a38c9-118">Create rules in an existing NSG</span></span>
<span data-ttu-id="a38c9-119">Azure-portálról egy meglévő NSG-szabályok létrehozására, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="a38c9-119">To create rules in an existing NSG from the Azure portal, follow the steps below.</span></span>

1. <span data-ttu-id="a38c9-120">Kattintson a **Tallózás >** > **hálózati biztonsági csoportok**.</span><span class="sxs-lookup"><span data-stu-id="a38c9-120">Click **Browse >** > **Network security groups**.</span></span>
2. <span data-ttu-id="a38c9-121">Az NSG-ket, kattintson a **NSG-előtérbeli** > **bejövő biztonsági szabályok**</span><span class="sxs-lookup"><span data-stu-id="a38c9-121">In the list of NSGs, click **NSG-FrontEnd** > **Inbound security rules**</span></span>
   
    ![Azure portál – NSG-előtér](./media/virtual-networks-create-nsg-arm-pportal/figure2.png)
3. <span data-ttu-id="a38c9-123">A közül **bejövő biztonsági szabályok**, kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="a38c9-123">In the list of **Inbound security rules**, click **Add**.</span></span>
   
    ![Azure portál – szabály hozzáadása](./media/virtual-networks-create-nsg-arm-pportal/figure3.png)
4. <span data-ttu-id="a38c9-125">A a **Hozzáadás bejövő biztonsági szabály** panelen nevű szabályt létrehozni *web-szabály* prioritását *200* keresztül hozzáférést *TCP* portra *80* e bármelyik virtuális Gépet a forrás-, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="a38c9-125">In the **Add inbound security rule** blade, create a rule named *web-rule* with priority of *200* allowing access via *TCP* to port *80* to any VM from any source, and then click **OK**.</span></span> <span data-ttu-id="a38c9-126">Figyelje meg, hogy ezek a beállítások a legtöbb alapértelmezett értékei lesznek már.</span><span class="sxs-lookup"><span data-stu-id="a38c9-126">Notice that most of these settings are default values already.</span></span>
   
    ![Azure portál – szabálybeállításai](./media/virtual-networks-create-nsg-arm-pportal/figure4.png)
5. <span data-ttu-id="a38c9-128">Néhány másodpercen belül megjelenik az új szabály az NSG.</span><span class="sxs-lookup"><span data-stu-id="a38c9-128">After a few seconds you will see the new rule in the NSG.</span></span>
   
    ![Azure portál – új szabály](./media/virtual-networks-create-nsg-arm-pportal/figure5.png)
6. <span data-ttu-id="a38c9-130">Ismételje meg a 6-hozzon létre egy bejövő forgalomra vonatkozó szabály nevű *rdp-szabály* prioritással *250* keresztül hozzáférést *TCP* portra *3389-es* forrásból bármely virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="a38c9-130">Repeat steps  to 6 to create an inbound rule named *rdp-rule* with a priority of *250* allowing access via *TCP* to port *3389* to any VM from any source.</span></span>

## <a name="associate-the-nsg-to-the-frontend-subnet"></a><span data-ttu-id="a38c9-131">NSG hozzárendelése az előtérben levő alhálózathoz</span><span class="sxs-lookup"><span data-stu-id="a38c9-131">Associate the NSG to the FrontEnd subnet</span></span>
1. <span data-ttu-id="a38c9-132">Kattintson a **Tallózás >** > **erőforráscsoportok** > **RG-NSG**.</span><span class="sxs-lookup"><span data-stu-id="a38c9-132">Click **Browse >** > **Resource groups** > **RG-NSG**.</span></span>
2. <span data-ttu-id="a38c9-133">Az a **RG-NSG** paneljén kattintson **...**   >  **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="a38c9-133">In the **RG-NSG** blade, click **...** > **TestVNet**.</span></span>
   
    ![Azure portál – TestVNet](./media/virtual-networks-create-nsg-arm-pportal/figure14.png)
3. <span data-ttu-id="a38c9-135">Az a **beállítások** panelen kattintson a **alhálózatok** > **előtér** > **hálózati biztonsági csoport** > **NSG-előtér**.</span><span class="sxs-lookup"><span data-stu-id="a38c9-135">In the **Settings** blade, click **Subnets** > **FrontEnd** > **Network security group** > **NSG-FrontEnd**.</span></span>
   
    ![Azure portál – az alhálózati beállítások](./media/virtual-networks-create-nsg-arm-pportal/figure15.png)
4. <span data-ttu-id="a38c9-137">Az a **előtér** panelen kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="a38c9-137">In the **FrontEnd** blade, click **Save**.</span></span>
   
    ![Azure portál – az alhálózati beállítások](./media/virtual-networks-create-nsg-arm-pportal/figure16.png)

## <a name="create-the-nsg-backend-nsg"></a><span data-ttu-id="a38c9-139">Az NSG-háttérrendszer NSG létrehozása</span><span class="sxs-lookup"><span data-stu-id="a38c9-139">Create the NSG-BackEnd NSG</span></span>
<span data-ttu-id="a38c9-140">Létrehozásához a **NSG-háttérrendszer** NSG-t, és rendelje hozzá azt a **háttér** alhálózati, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="a38c9-140">To create the **NSG-BackEnd** NSG and associate it to the **BackEnd** subnet, follow the steps below.</span></span>

1. <span data-ttu-id="a38c9-141">Ismételje meg a [létrehozása az NSG-előtérbeli NSG](#Create-the-NSG-FrontEnd-NSG) létrehozni egy NSG nevű *NSG-háttérrendszer*</span><span class="sxs-lookup"><span data-stu-id="a38c9-141">Repeat the steps in [Create the NSG-FrontEnd NSG](#Create-the-NSG-FrontEnd-NSG) to create an NSG named *NSG-BackEnd*</span></span>
2. <span data-ttu-id="a38c9-142">Ismételje meg a [egy meglévő NSG-szabályok létrehozására](#Create-rules-in-an-existing-NSG) létrehozásához a **bejövő** szabályok az alábbi táblázatban.</span><span class="sxs-lookup"><span data-stu-id="a38c9-142">Repeat the steps in [Create rules in an existing NSG](#Create-rules-in-an-existing-NSG) to create the **inbound** rules in the table below.</span></span>
   
   | <span data-ttu-id="a38c9-143">Bejövő forgalomra vonatkozó szabály</span><span class="sxs-lookup"><span data-stu-id="a38c9-143">Inbound rule</span></span> | <span data-ttu-id="a38c9-144">Kimenő forgalomra vonatkozó szabály</span><span class="sxs-lookup"><span data-stu-id="a38c9-144">Outbound rule</span></span> |
   | --- | --- |
   | ![Azure portál – bejövő forgalomra vonatkozó szabály](./media/virtual-networks-create-nsg-arm-pportal/figure17.png) |![Azure portál – kimenő forgalomra vonatkozó szabály](./media/virtual-networks-create-nsg-arm-pportal/figure18.png) |
3. <span data-ttu-id="a38c9-147">Ismételje meg a [társít a NSG a FrontEnd alhálózathoz](#Associate-the-NSG-to-the-FrontEnd-subnet) társítja a **NSG-háttérrendszer** NSG a **háttér** alhálózati.</span><span class="sxs-lookup"><span data-stu-id="a38c9-147">Repeat the steps in [Associate the NSG to the FrontEnd subnet](#Associate-the-NSG-to-the-FrontEnd-subnet) to associate the **NSG-Backend** NSG to the **BackEnd** subnet.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a38c9-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a38c9-148">Next Steps</span></span>
* <span data-ttu-id="a38c9-149">Megtudhatja, hogyan [meglévő NSG-k kezelése](virtual-network-manage-nsg-arm-portal.md)</span><span class="sxs-lookup"><span data-stu-id="a38c9-149">Learn how to [manage existing NSGs](virtual-network-manage-nsg-arm-portal.md)</span></span>
* <span data-ttu-id="a38c9-150">[Naplózás engedélyezése](virtual-network-nsg-manage-log.md) az NSG-ket.</span><span class="sxs-lookup"><span data-stu-id="a38c9-150">[Enable logging](virtual-network-nsg-manage-log.md) for NSGs.</span></span>

