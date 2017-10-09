---
title: "aaaManage hálózati biztonsági csoport – az Azure portálon |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage hálózati biztonsági csoportok használatával hello Azure-portálon."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: faee5ac8-f4c4-4f97-ade5-197a37aad496
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 53fb29e60cbc2a535f6cf03e430d9e703e97b216
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-network-security-groups-using-hello-azure-portal"></a><span data-ttu-id="cbe8a-103">Hello Azure-portál használatával a hálózati biztonsági csoportok kezelése</span><span class="sxs-lookup"><span data-stu-id="cbe8a-103">Manage network security groups using hello Azure portal</span></span>

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="cbe8a-104">Ez a cikk ismerteti a hello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="cbe8a-104">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="cbe8a-105">Emellett [NSG-k létrehozása hello klasszikus üzembe helyezési modellel](virtual-networks-create-nsg-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="cbe8a-105">You can also [create NSGs in hello classic deployment model](virtual-networks-create-nsg-classic-ps.md).</span></span>

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

<span data-ttu-id="cbe8a-106">hello minta az alábbi parancsok várt már létrehozott egy egyszerű környezetben PowerShell fenti hello forgatókönyv alapján.</span><span class="sxs-lookup"><span data-stu-id="cbe8a-106">hello sample PowerShell commands below expect a simple environment already created based on hello scenario above.</span></span> <span data-ttu-id="cbe8a-107">Ha toorun hello parancsok ebben a dokumentumban megjelenített, először létre hello tesztkörnyezetben üzembe helyezésével [sablon](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), kattintson a **tooAzure telepítése**, cserélje le a hello alapértelmezett paraméterértékek Ha szükséges, és kövesse az utasításokat hello a hello portálon.</span><span class="sxs-lookup"><span data-stu-id="cbe8a-107">If you want toorun hello commands as they are displayed in this document, first build hello test environment by deploying [this template](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), click **Deploy tooAzure**, replace hello default parameter values if necessary, and follow hello instructions in hello portal.</span></span> <span data-ttu-id="cbe8a-108">hello használata alatt lépések **RG-NSG** , hello tanúsítványsablon-nevet, hello erőforrás csoport hello állított be.</span><span class="sxs-lookup"><span data-stu-id="cbe8a-108">hello steps below use **RG-NSG** as hello name of hello resource group hello template was deployed to.</span></span>

## <a name="create-hello-nsg-frontend-nsg"></a><span data-ttu-id="cbe8a-109">NSG-előtérbeli NSG hello létrehozása</span><span class="sxs-lookup"><span data-stu-id="cbe8a-109">Create hello NSG-FrontEnd NSG</span></span>
<span data-ttu-id="cbe8a-110">toocreate hello **NSG-előtérbeli** NSG hello forgatókönyv a fenti ábrán kövesse az alábbi hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="cbe8a-110">toocreate hello **NSG-FrontEnd** NSG as shown in hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="cbe8a-111">Egy böngészőből keresse meg a toohttp://portal.azure.com, és ha szükséges, jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="cbe8a-111">From a browser, navigate toohttp://portal.azure.com and, if necessary, sign in with your Azure account.</span></span>
2. <span data-ttu-id="cbe8a-112">Kattintson a **Tallózás >** > **hálózati biztonsági csoportok**.</span><span class="sxs-lookup"><span data-stu-id="cbe8a-112">Click **Browse >** > **Network Security Groups**.</span></span>
   
    ![Azure portál – NSG-k](./media/virtual-networks-create-nsg-arm-pportal/figure11.png)
3. <span data-ttu-id="cbe8a-114">A hello **hálózati biztonsági csoportok** panelen kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="cbe8a-114">In hello **Network security groups** blade, click **Add**.</span></span>
   
    ![Azure portál – NSG-k](./media/virtual-networks-create-nsg-arm-pportal/figure12.png)
4. <span data-ttu-id="cbe8a-116">A hello **hálózati biztonsági csoport létrehozása** panelen, hozzon létre egy NSG nevű *NSG-előtér* a hello *RG-NSG* erőforráscsoportban, és kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="cbe8a-116">In hello **Create network security group** blade, create an NSG named *NSG-FrontEnd* in hello *RG-NSG* resource group, and then click **Create**.</span></span>
   
    ![Azure portál – NSG-k](./media/virtual-networks-create-nsg-arm-pportal/figure13.png)

## <a name="create-rules-in-an-existing-nsg"></a><span data-ttu-id="cbe8a-118">Szabályok létrehozása egy létező NSG-ben</span><span class="sxs-lookup"><span data-stu-id="cbe8a-118">Create rules in an existing NSG</span></span>
<span data-ttu-id="cbe8a-119">egy meglévő NSG hello Azure-portálon a toocreate szabályait kövesse hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="cbe8a-119">toocreate rules in an existing NSG from hello Azure portal, follow hello steps below.</span></span>

1. <span data-ttu-id="cbe8a-120">Kattintson a **Tallózás >** > **hálózati biztonsági csoportok**.</span><span class="sxs-lookup"><span data-stu-id="cbe8a-120">Click **Browse >** > **Network security groups**.</span></span>
2. <span data-ttu-id="cbe8a-121">Az NSG-k hello listájában kattintson **NSG-előtérbeli** > **bejövő biztonsági szabályok**</span><span class="sxs-lookup"><span data-stu-id="cbe8a-121">In hello list of NSGs, click **NSG-FrontEnd** > **Inbound security rules**</span></span>
   
    ![Azure portál – NSG-előtér](./media/virtual-networks-create-nsg-arm-pportal/figure2.png)
3. <span data-ttu-id="cbe8a-123">Hello listájában **bejövő biztonsági szabályok**, kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="cbe8a-123">In hello list of **Inbound security rules**, click **Add**.</span></span>
   
    ![Azure portál – szabály hozzáadása](./media/virtual-networks-create-nsg-arm-pportal/figure3.png)
4. <span data-ttu-id="cbe8a-125">A hello **Hozzáadás bejövő biztonsági szabály** panelen nevű szabályt létrehozni *web-szabály* prioritását *200* keresztül hozzáférést *TCP* tooport *80* tooany VM bármelyik forrás-, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="cbe8a-125">In hello **Add inbound security rule** blade, create a rule named *web-rule* with priority of *200* allowing access via *TCP* tooport *80* tooany VM from any source, and then click **OK**.</span></span> <span data-ttu-id="cbe8a-126">Figyelje meg, hogy ezek a beállítások a legtöbb alapértelmezett értékei lesznek már.</span><span class="sxs-lookup"><span data-stu-id="cbe8a-126">Notice that most of these settings are default values already.</span></span>
   
    ![Azure portál – szabálybeállításai](./media/virtual-networks-create-nsg-arm-pportal/figure4.png)
5. <span data-ttu-id="cbe8a-128">Néhány másodpercen belül megjelenik hello NSG hello új szabályt.</span><span class="sxs-lookup"><span data-stu-id="cbe8a-128">After a few seconds you will see hello new rule in hello NSG.</span></span>
   
    ![Azure portál – új szabály](./media/virtual-networks-create-nsg-arm-pportal/figure5.png)
6. <span data-ttu-id="cbe8a-130">Ismételje meg a too6 toocreate egy bejövő forgalomra vonatkozó szabály nevű *rdp-szabály* prioritással *250* keresztül hozzáférést *TCP* tooport *3389-es* a forrás virtuális gép tooany.</span><span class="sxs-lookup"><span data-stu-id="cbe8a-130">Repeat steps  too6 toocreate an inbound rule named *rdp-rule* with a priority of *250* allowing access via *TCP* tooport *3389* tooany VM from any source.</span></span>

## <a name="associate-hello-nsg-toohello-frontend-subnet"></a><span data-ttu-id="cbe8a-131">Társítsa a hello NSG toohello FrontEnd alhálózathoz</span><span class="sxs-lookup"><span data-stu-id="cbe8a-131">Associate hello NSG toohello FrontEnd subnet</span></span>
1. <span data-ttu-id="cbe8a-132">Kattintson a **Tallózás >** > **erőforráscsoportok** > **RG-NSG**.</span><span class="sxs-lookup"><span data-stu-id="cbe8a-132">Click **Browse >** > **Resource groups** > **RG-NSG**.</span></span>
2. <span data-ttu-id="cbe8a-133">A hello **RG-NSG** paneljén kattintson **...**   >  **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="cbe8a-133">In hello **RG-NSG** blade, click **...** > **TestVNet**.</span></span>
   
    ![Azure portál – TestVNet](./media/virtual-networks-create-nsg-arm-pportal/figure14.png)
3. <span data-ttu-id="cbe8a-135">A hello **beállítások** panelen kattintson a **alhálózatok** > **előtér** > **hálózati biztonsági csoport**  >  **NSG-előtérbeli**.</span><span class="sxs-lookup"><span data-stu-id="cbe8a-135">In hello **Settings** blade, click **Subnets** > **FrontEnd** > **Network security group** > **NSG-FrontEnd**.</span></span>
   
    ![Azure portál – az alhálózati beállítások](./media/virtual-networks-create-nsg-arm-pportal/figure15.png)
4. <span data-ttu-id="cbe8a-137">A hello **előtér** panelen kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="cbe8a-137">In hello **FrontEnd** blade, click **Save**.</span></span>
   
    ![Azure portál – az alhálózati beállítások](./media/virtual-networks-create-nsg-arm-pportal/figure16.png)

## <a name="create-hello-nsg-backend-nsg"></a><span data-ttu-id="cbe8a-139">Hello NSG NSG-háttéralkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="cbe8a-139">Create hello NSG-BackEnd NSG</span></span>
<span data-ttu-id="cbe8a-140">toocreate hello **NSG-háttérrendszer** NSG és társítsa azt toohello **háttér** alhálózati, hajtsa végre hello alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="cbe8a-140">toocreate hello **NSG-BackEnd** NSG and associate it toohello **BackEnd** subnet, follow hello steps below.</span></span>

1. <span data-ttu-id="cbe8a-141">Ismétlődő hello szükséges lépések [létrehozás hello NSG-előtérbeli NSG](#Create-the-NSG-FrontEnd-NSG) toocreate egy NSG nevű *NSG-háttérrendszer*</span><span class="sxs-lookup"><span data-stu-id="cbe8a-141">Repeat hello steps in [Create hello NSG-FrontEnd NSG](#Create-the-NSG-FrontEnd-NSG) toocreate an NSG named *NSG-BackEnd*</span></span>
2. <span data-ttu-id="cbe8a-142">Ismétlődő hello szükséges lépések [egy meglévő NSG-szabályok létrehozására](#Create-rules-in-an-existing-NSG) toocreate hello **bejövő** hello az alábbi táblázatban a szabályok.</span><span class="sxs-lookup"><span data-stu-id="cbe8a-142">Repeat hello steps in [Create rules in an existing NSG](#Create-rules-in-an-existing-NSG) toocreate hello **inbound** rules in hello table below.</span></span>
   
   | <span data-ttu-id="cbe8a-143">Bejövő forgalomra vonatkozó szabály</span><span class="sxs-lookup"><span data-stu-id="cbe8a-143">Inbound rule</span></span> | <span data-ttu-id="cbe8a-144">Kimenő forgalomra vonatkozó szabály</span><span class="sxs-lookup"><span data-stu-id="cbe8a-144">Outbound rule</span></span> |
   | --- | --- |
   | ![Azure portál – bejövő forgalomra vonatkozó szabály](./media/virtual-networks-create-nsg-arm-pportal/figure17.png) |![Azure portál – kimenő forgalomra vonatkozó szabály](./media/virtual-networks-create-nsg-arm-pportal/figure18.png) |
3. <span data-ttu-id="cbe8a-147">Ismétlődő hello szükséges lépések [hello NSG toohello FrontEnd alhálózathoz társítása](#Associate-the-NSG-to-the-FrontEnd-subnet) tooassociate hello **NSG-háttérrendszer** NSG toohello **háttér** alhálózat.</span><span class="sxs-lookup"><span data-stu-id="cbe8a-147">Repeat hello steps in [Associate hello NSG toohello FrontEnd subnet](#Associate-the-NSG-to-the-FrontEnd-subnet) tooassociate hello **NSG-Backend** NSG toohello **BackEnd** subnet.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cbe8a-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cbe8a-148">Next Steps</span></span>
* <span data-ttu-id="cbe8a-149">Ismerje meg, hogyan túl[meglévő NSG-k kezelése](virtual-network-manage-nsg-arm-portal.md)</span><span class="sxs-lookup"><span data-stu-id="cbe8a-149">Learn how too[manage existing NSGs](virtual-network-manage-nsg-arm-portal.md)</span></span>
* <span data-ttu-id="cbe8a-150">[Naplózás engedélyezése](virtual-network-nsg-manage-log.md) az NSG-ket.</span><span class="sxs-lookup"><span data-stu-id="cbe8a-150">[Enable logging](virtual-network-nsg-manage-log.md) for NSGs.</span></span>

