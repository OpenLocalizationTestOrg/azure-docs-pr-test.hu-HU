---
title: "Több VPN gateway pont-pont kapcsolatok tooa virtuális hálózat hozzáadása: Azure-portálon: erőforrás-kezelő |} Microsoft Docs"
description: "Többhelyes S2S kapcsolatok tooa VPN-átjáró, amely rendelkezik egy létező kapcsolat hozzáadása"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f3e8b165-f20a-42ab-afbb-bf60974bb4b1
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/20/2017
ms.author: cherylmc
ms.openlocfilehash: b8c9ff454967f509dcef725f8bcec8564fad9b00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-site-to-site-connection-tooa-vnet-with-an-existing-vpn-gateway-connection"></a><span data-ttu-id="1d3ac-103">A pont-pont kapcsolat tooa virtuális hálózatot egy meglévő VPN-átjáró kapcsolattal hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1d3ac-103">Add a Site-to-Site connection tooa VNet with an existing VPN gateway connection</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="1d3ac-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1d3ac-104">Azure portal</span></span>](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="1d3ac-105">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="1d3ac-105">PowerShell (classic)</span></span>](vpn-gateway-multi-site.md)
>
> 

<span data-ttu-id="1d3ac-106">Ez a cikk bemutatja, hogyan hello Azure portál tooadd webhelyek (közötti S2S) kapcsolatok tooa VPN-átjáró, amely rendelkezik egy létező kapcsolat használatával.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-106">This article walks you through using hello Azure portal tooadd Site-to-Site (S2S) connections tooa VPN gateway that has an existing connection.</span></span> <span data-ttu-id="1d3ac-107">Ez a kapcsolat típus gyakran hivatkozott tooas egy "több" Helykonfiguráció.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-107">This type of connection is often referred tooas a "multi-site" configuration.</span></span> <span data-ttu-id="1d3ac-108">Hozzáadhat egy S2S kapcsolat tooa virtuális hálózatot, amely már rendelkezik egy S2S kapcsolatot, a pont – hely típusú kapcsolatot vagy a VNet – VNet-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-108">You can add a S2S connection tooa VNet that already has a S2S connection, Point-to-Site connection, or VNet-to-VNet connection.</span></span> <span data-ttu-id="1d3ac-109">Bizonyos korlátozások is kapcsolatok hozzáadása során.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-109">There are some limitations when adding connections.</span></span> <span data-ttu-id="1d3ac-110">Ellenőrizze a hello [megkezdése előtt](#before) Ez a cikk tooverify konfigurációtól elindítása előtt című szakasza.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-110">Check hello [Before you begin](#before) section in this article tooverify before you start your configuration.</span></span> 

<span data-ttu-id="1d3ac-111">Ez a cikk vonatkozik tooVNets hello erőforrás-kezelő telepítési modellel készült, amelyek RouteBased VPN-átjáró.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-111">This article applies tooVNets created using hello Resource Manager deployment model that have a RouteBased VPN gateway.</span></span> <span data-ttu-id="1d3ac-112">Ezeket a lépéseket ne alkalmazza a tooExpressRoute/pont-pont vizsgálatát a kísérő kapcsolat konfigurációkat.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-112">These steps do not apply tooExpressRoute/Site-to-Site coexisting connection configurations.</span></span> <span data-ttu-id="1d3ac-113">Lásd: [vizsgálatát a kísérő ExpressRoute és az S2S-kapcsolatok](../expressroute/expressroute-howto-coexist-resource-manager.md) vizsgálatát a kísérő kapcsolatok kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-113">See [ExpressRoute/S2S coexisting connections](../expressroute/expressroute-howto-coexist-resource-manager.md) for information about coexisting connections.</span></span>

### <a name="deployment-models-and-methods"></a><span data-ttu-id="1d3ac-114">Üzemi modellek és módszerek</span><span class="sxs-lookup"><span data-stu-id="1d3ac-114">Deployment models and methods</span></span>
[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

<span data-ttu-id="1d3ac-115">Ebben a táblázatban, új cikket frissítjük, és további eszközök ehhez a konfigurációhoz elérhetővé válnak.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-115">We update this table as new articles and additional tools become available for this configuration.</span></span> <span data-ttu-id="1d3ac-116">Egy cikk nem érhető el, hogy hivatkozás közvetlenül tooit ebből a táblázatból.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-116">When an article is available, we link directly tooit from this table.</span></span>

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <span data-ttu-id="1d3ac-117"><a name="before"></a>Előkészületek</span><span class="sxs-lookup"><span data-stu-id="1d3ac-117"><a name="before"></a>Before you begin</span></span>
<span data-ttu-id="1d3ac-118">Ellenőrizze a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="1d3ac-118">Verify hello following items:</span></span>

* <span data-ttu-id="1d3ac-119">Nem hoz létre egy vizsgálatát a kísérő ExpressRoute és az S2S-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-119">You are not creating an ExpressRoute/S2S coexisting connection.</span></span>
* <span data-ttu-id="1d3ac-120">A virtuális hálózati létrehozott hello Resource Manager üzembe helyezési modellben már meglévő kapcsolattal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-120">You have a virtual network that was created using hello Resource Manager deployment model with an existing connection.</span></span>
* <span data-ttu-id="1d3ac-121">hello virtuális hálózati átjáró a Vnethez tartozó RouteBased.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-121">hello virtual network gateway for your VNet is RouteBased.</span></span> <span data-ttu-id="1d3ac-122">Ha PolicyBased VPN-átjáró, akkor törölje a hello virtuális hálózati átjáró, majd hozzon létre egy új VPN-átjáró mint RouteBased.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-122">If you have a PolicyBased VPN gateway, you must delete hello virtual network gateway and create a new VPN gateway as RouteBased.</span></span>
* <span data-ttu-id="1d3ac-123">Nincs hello címtartományai átfedésben vannak, bármely hello Vnetek, hogy ez a virtuális hálózat csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-123">None of hello address ranges overlap for any of hello VNets that this VNet is connecting to.</span></span>
* <span data-ttu-id="1d3ac-124">VPN-kompatibilis eszköz és a rendszer képes tooconfigure azt.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-124">You have compatible VPN device and someone who is able tooconfigure it.</span></span> <span data-ttu-id="1d3ac-125">Lásd: [About VPN Devices](vpn-gateway-about-vpn-devices.md) (Tudnivalók a VPN-eszközökről).</span><span class="sxs-lookup"><span data-stu-id="1d3ac-125">See [About VPN Devices](vpn-gateway-about-vpn-devices.md).</span></span> <span data-ttu-id="1d3ac-126">Ha nem ismeri a konfigurálása a VPN-eszköz, vagy nem ismeri a hello IP-címtartományok található, a helyszíni hálózati konfigurációtól, toocoordinate kell valakitől meg ezeket az információkat is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-126">If you aren't familiar with configuring your VPN device, or are unfamiliar with hello IP address ranges located in your on-premises network configuration, you need toocoordinate with someone who can provide those details for you.</span></span>
* <span data-ttu-id="1d3ac-127">A VPN-eszköz van külsőleg irányuló nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-127">You have an externally facing public IP address for your VPN device.</span></span> <span data-ttu-id="1d3ac-128">Ez az IP-cím nem lehet NAT mögötti.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-128">This IP address cannot be located behind a NAT.</span></span>

## <span data-ttu-id="1d3ac-129"><a name="part1"></a>1. rész - kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="1d3ac-129"><a name="part1"></a>Part 1 - Configure a connection</span></span>
1. <span data-ttu-id="1d3ac-130">Egy böngészőből keresse meg a toohello [Azure-portálon](http://portal.azure.com) és szükség esetén jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-130">From a browser, navigate toohello [Azure portal](http://portal.azure.com) and, if necessary, sign in with your Azure account.</span></span>
2. <span data-ttu-id="1d3ac-131">Kattintson a **összes erőforrás** , és keresse meg a **virtuális hálózati átjáró** erőforrásokat hello lista, és kattintson rá.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-131">Click **All resources** and locate your **virtual network gateway** from hello list of resources and click it.</span></span>
3. <span data-ttu-id="1d3ac-132">A hello **virtuális hálózati átjáró** panelen kattintson a **kapcsolatok**.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-132">On hello **Virtual network gateway** blade, click **Connections**.</span></span>
   
    <span data-ttu-id="1d3ac-133">![Kapcsolatpanel](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Kapcsolatpanel")</span><span class="sxs-lookup"><span data-stu-id="1d3ac-133">![Connections blade](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Connections blade")</span></span><br>
4. <span data-ttu-id="1d3ac-134">A hello **kapcsolatok** panelen kattintson a **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-134">On hello **Connections** blade, click **+Add**.</span></span>
   
    <span data-ttu-id="1d3ac-135">![Hozzáadás kapcsolat gombra](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "Hozzáadás kapcsolat gombra")</span><span class="sxs-lookup"><span data-stu-id="1d3ac-135">![Add connection button](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "Add connection button")</span></span><br>
5. <span data-ttu-id="1d3ac-136">A hello **kapcsolat hozzáadása a** panelen kitöltése során a következő mezők hello:</span><span class="sxs-lookup"><span data-stu-id="1d3ac-136">On hello **Add connection** blade, fill out hello following fields:</span></span>
   
   * <span data-ttu-id="1d3ac-137">**Name:** hello neve toogive toohello webhely hello kapcsolatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-137">**Name:** hello name you want toogive toohello site you are creating hello connection to.</span></span>
   * <span data-ttu-id="1d3ac-138">**Kapcsolat típusa:** válasszon **pont-pont (IPsec)**.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-138">**Connection type:** Select **Site-to-site (IPsec)**.</span></span>
     
     <span data-ttu-id="1d3ac-139">![Hozzáadás kapcsolat panelen](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "Hozzáadás kapcsolat panelen")</span><span class="sxs-lookup"><span data-stu-id="1d3ac-139">![Add connection blade](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "Add connection blade")</span></span><br>

## <span data-ttu-id="1d3ac-140"><a name="part2"></a>2. rész – a helyi hálózati átjáró hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1d3ac-140"><a name="part2"></a>Part 2 - Add a local network gateway</span></span>
1. <span data-ttu-id="1d3ac-141">Kattintson a **helyi hálózati átjáró** ***helyi hálózati átjáró kiválasztása***.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-141">Click **Local network gateway** ***Choose a local network gateway***.</span></span> <span data-ttu-id="1d3ac-142">Ekkor megnyílik hello **válassza a helyi hálózati átjáró** panelen.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-142">This will open hello **Choose local network gateway** blade.</span></span>
   
    <span data-ttu-id="1d3ac-143">![Válassza a helyi hálózati átjáró](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "helyi hálózati átjáró kiválasztása")</span><span class="sxs-lookup"><span data-stu-id="1d3ac-143">![Choose local network gateway](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "Choose local network gateway")</span></span><br>
2. <span data-ttu-id="1d3ac-144">Kattintson a **hozzon létre új** tooopen hello **helyi hálózati átjáró létrehozása** panelen.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-144">Click **Create new** tooopen hello **Create local network gateway** blade.</span></span>
   
    <span data-ttu-id="1d3ac-145">![Létrehozás helyi hálózati átjáró panel](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "helyi hálózati átjáró létrehozása")</span><span class="sxs-lookup"><span data-stu-id="1d3ac-145">![Create local network gateway blade](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "Create local network gateway")</span></span><br>
3. <span data-ttu-id="1d3ac-146">A hello **helyi hálózati átjáró létrehozása** panelen kitöltése során a következő mezők hello:</span><span class="sxs-lookup"><span data-stu-id="1d3ac-146">On hello **Create local network gateway** blade, fill out hello following fields:</span></span>
   
   * <span data-ttu-id="1d3ac-147">**Name:** hello nevet toogive toohello helyi hálózati átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-147">**Name:** hello name you want toogive toohello local network gateway resource.</span></span>
   * <span data-ttu-id="1d3ac-148">**IP-cím:** hello hello VPN-eszköz, amelyet a tooconnect hello helyen nyilvános IP-címét.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-148">**IP address:** hello public IP address of hello VPN device on hello site that you want tooconnect to.</span></span>
   * <span data-ttu-id="1d3ac-149">**Címtér:** , amelyet az toobe hello címterület toohello új helyi hálózati telephely átirányítva.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-149">**Address space:** hello address space that you want toobe routed toohello new local network site.</span></span>
4. <span data-ttu-id="1d3ac-150">Kattintson a **OK** a hello **helyi hálózati átjáró létrehozása** panel toosave hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-150">Click **OK** on hello **Create local network gateway** blade toosave hello changes.</span></span>

## <span data-ttu-id="1d3ac-151"><a name="part3"></a>3. rész – hello megosztott kulcs hozzáadása és hello kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="1d3ac-151"><a name="part3"></a>Part 3 - Add hello shared key and create hello connection</span></span>
1. <span data-ttu-id="1d3ac-152">A hello **kapcsolat hozzáadása a** panelen hello megosztott kulcsot, amelyet toouse toocreate a kapcsolatot hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-152">On hello **Add connection** blade, add hello shared key that you want toouse toocreate your connection.</span></span> <span data-ttu-id="1d3ac-153">Is megadhat vagy hello megosztott kulcs beszerzése a VPN-eszköz vagy egy itt, majd konfigurálja a VPN-eszköz toouse hello azonos megosztott kulcs.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-153">You can either get hello shared key from your VPN device, or make one up here and then configure your VPN device toouse hello same shared key.</span></span> <span data-ttu-id="1d3ac-154">fontos dolog, hogy hello kulcs pontosan van-e hello hello azonos.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-154">hello important thing is that hello keys are exactly hello same.</span></span>
   
    <span data-ttu-id="1d3ac-155">![Megosztott kulcs](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Megosztott kulcs")</span><span class="sxs-lookup"><span data-stu-id="1d3ac-155">![Shared key](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Shared key")</span></span><br>
2. <span data-ttu-id="1d3ac-156">Hello hello panel alsó részén, kattintson a **OK** toocreate hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-156">At hello bottom of hello blade, click **OK** toocreate hello connection.</span></span>

## <span data-ttu-id="1d3ac-157"><a name="part4"></a>Rész 4 – hello VPN-kapcsolat ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="1d3ac-157"><a name="part4"></a>Part 4 - Verify hello VPN connection</span></span>


[!INCLUDE [vpn-gateway-verify-connection-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="next-steps"></a><span data-ttu-id="1d3ac-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1d3ac-158">Next steps</span></span>

<span data-ttu-id="1d3ac-159">Ha a kapcsolat befejeződött, a virtuális gépek tooyour virtuális hálózatok is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-159">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="1d3ac-160">Tekintse meg a virtuális gépek hello [képzési](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) további információt.</span><span class="sxs-lookup"><span data-stu-id="1d3ac-160">See hello virtual machines [learning path](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) for more information.</span></span>
