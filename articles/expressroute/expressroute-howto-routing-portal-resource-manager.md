---
title: "Útválasztási (társviszony-létesítés) a egy ExpressRoute-áramkör konfigurálása: erőforrás-kezelő: Azure |} Microsoft Docs"
description: "A cikk az ExpressRoute-kapcsolatcsoportok privát, nyilvános és Microsoft társviszony-létesítéses létrehozásának és kiépítésének lépéseit ismerteti. A cikk azt is bemutatja, hogyan ellenőrizheti a kapcsolatcsoport társviszonyainak állapotát, illetve hogyan frissítheti vagy törölheti őket."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8c2a7ed2-ae5c-4e49-81f6-77cf9f2b2ac9
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: cherylmc
ms.openlocfilehash: 55ccadfea55b8098ee58dcaef942f6ba54093665
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit"></a><span data-ttu-id="151c3-104">Létrehozásához és módosításához az ExpressRoute-kör társviszony</span><span class="sxs-lookup"><span data-stu-id="151c3-104">Create and modify peering for an ExpressRoute circuit</span></span>

<span data-ttu-id="151c3-105">Ez a cikk segít hozhat létre és kezelhet a Resource Manager üzembe helyezési modellel, az Azure portál használatával ExpressRoute-kapcsolatcsoportot útválasztási konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="151c3-105">This article helps you create and manage routing configuration for an ExpressRoute circuit in the Resource Manager deployment model using the Azure portal.</span></span> <span data-ttu-id="151c3-106">Ellenőrizze az állapot, frissítési vagy törlési is, és az ExpressRoute-kör társviszony kiosztásának megszüntetése.</span><span class="sxs-lookup"><span data-stu-id="151c3-106">You can also check the status, update, or delete and deprovision peerings for an ExpressRoute circuit.</span></span> <span data-ttu-id="151c3-107">Ha más módszert használja a kapcsolatcsoport dolgozni szeretne, válassza ki egy cikk az alábbi listából:</span><span class="sxs-lookup"><span data-stu-id="151c3-107">If you want to use a different method to work with your circuit, select an article from the following list:</span></span>


## <a name="configuration-prerequisites"></a><span data-ttu-id="151c3-108">Konfigurációs előfeltételek</span><span class="sxs-lookup"><span data-stu-id="151c3-108">Configuration prerequisites</span></span>

* <span data-ttu-id="151c3-109">A konfigurálás megkezdése előtt mindenképp tekintse át az [előfeltételek](expressroute-prerequisites.md), az [útválasztási követelmények](expressroute-routing.md) és a [munkafolyamatok](expressroute-workflows.md) lapot.</span><span class="sxs-lookup"><span data-stu-id="151c3-109">Make sure that you have reviewed the [prerequisites](expressroute-prerequisites.md) page, the [routing requirements](expressroute-routing.md) page, and the [workflows](expressroute-workflows.md) page before you begin configuration.</span></span>
* <span data-ttu-id="151c3-110">Egy aktív ExpressRoute-kapcsolatcsoportra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="151c3-110">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="151c3-111">Kövesse az [ExpressRoute-kapcsolatcsoport létrehozása](expressroute-howto-circuit-portal-resource-manager.md) részben foglalt lépéseket, és engedélyeztesse a kapcsolatcsoportot kapcsolatszolgáltatójával, mielőtt továbblépne.</span><span class="sxs-lookup"><span data-stu-id="151c3-111">Follow the instructions to [Create an ExpressRoute circuit](expressroute-howto-circuit-portal-resource-manager.md) and have the circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="151c3-112">Az ExpressRoute-kapcsolatcsoport ahhoz, hogy tud-parancsmagok futtatásához a következő szakaszokban lévő kiépített és engedélyezett állapotban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="151c3-112">The ExpressRoute circuit must be in a provisioned and enabled state for you to be able to run the cmdlets in the next sections.</span></span>
* <span data-ttu-id="151c3-113">Ha egy megosztott kulcsot/MD5 kivonatoló használja, ügyeljen arra, hogy az használja ezt a-alagút mindkét oldalon, és legfeljebb 25 karakterek számát.</span><span class="sxs-lookup"><span data-stu-id="151c3-113">If you plan to use a shared key/MD5 hash, be sure to use this on both sides of the tunnel and limit the number of characters to a maximum of 25.</span></span>

<span data-ttu-id="151c3-114">Az utasítások csak 2. rétegbeli kapcsolatszolgáltatásokat kínáló szolgáltatóknál létrehozott kapcsolatcsoportokra vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="151c3-114">These instructions only apply to circuits created with service providers offering Layer 2 connectivity services.</span></span> <span data-ttu-id="151c3-115">A szolgáltató által kezelt használata réteg (általában egy IPVPN, például az MPLS) 3 szolgáltatások, a kapcsolat szolgáltatójánál konfigurálja és kezeli az Ön útválasztást.</span><span class="sxs-lookup"><span data-stu-id="151c3-115">If you are using a service provider that offers managed Layer 3 services (typically an IPVPN, like MPLS), your connectivity provider configures and manages routing for you.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="151c3-116">A szolgáltatásfelügyeleti portálon jelenleg nem hirdetünk szolgáltatók által konfigurált társviszony-létesítéseket.</span><span class="sxs-lookup"><span data-stu-id="151c3-116">We currently do not advertise peerings configured by service providers through the service management portal.</span></span> <span data-ttu-id="151c3-117">Dolgozunk azon, hogy hamarosan bevezethessük ezt a képességet.</span><span class="sxs-lookup"><span data-stu-id="151c3-117">We are working on enabling this capability soon.</span></span> <span data-ttu-id="151c3-118">A BGP társviszony konfigurálása előtt ellenőrizze a szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="151c3-118">Check with your service provider before configuring BGP peerings.</span></span>
> 
> 

<span data-ttu-id="151c3-119">Egy, két vagy akár mindhárom társviszony-létesítést (Azure privát, Azure nyilvános és Microsoft) is konfigurálhatja egy adott ExpressRoute-kapcsolatcsoportban.</span><span class="sxs-lookup"><span data-stu-id="151c3-119">You can configure one, two, or all three peerings (Azure private, Azure public and Microsoft) for an ExpressRoute circuit.</span></span> <span data-ttu-id="151c3-120">A társviszony-létesítéseket tetszőleges sorrendben konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="151c3-120">You can configure peerings in any order you choose.</span></span> <span data-ttu-id="151c3-121">Az egyes társviszony-létesítéseket azonban mindenképp egyenként kell végrehajtania.</span><span class="sxs-lookup"><span data-stu-id="151c3-121">However, you must make sure that you complete the configuration of each peering one at a time.</span></span>

## <a name="azure-private-peering"></a><span data-ttu-id="151c3-122">Azure privát társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="151c3-122">Azure private peering</span></span>

<span data-ttu-id="151c3-123">Ez a szakasz segítséget nyújt a létrehozása, beolvasása, frissítése és törölni az Azure magánhálózati társviszony-létesítési ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="151c3-123">This section helps you create, get, update, and delete the Azure private peering configuration for an ExpressRoute circuit.</span></span>

### <a name="to-create-azure-private-peering"></a><span data-ttu-id="151c3-124">Azure privát társviszony-létesítés létrehozása</span><span class="sxs-lookup"><span data-stu-id="151c3-124">To create Azure private peering</span></span>

1. <span data-ttu-id="151c3-125">Konfigurálja az ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="151c3-125">Configure the ExpressRoute circuit.</span></span> <span data-ttu-id="151c3-126">A folytatás előtt győződjön meg róla, hogy a kapcsolatszolgáltató teljesen kiépítette a kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="151c3-126">Ensure that the circuit is fully provisioned by the connectivity provider before continuing.</span></span>

  ![lista](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. <span data-ttu-id="151c3-128">Konfigurálja az Azure privát társviszony-létesítést a kapcsolatcsoport számára.</span><span class="sxs-lookup"><span data-stu-id="151c3-128">Configure Azure private peering for the circuit.</span></span> <span data-ttu-id="151c3-129">Mielőtt folytatná a következő lépésekkel, ellenőrizze az alábbi elemek meglétét:</span><span class="sxs-lookup"><span data-stu-id="151c3-129">Make sure that you have the following items before you proceed with the next steps:</span></span>

  * <span data-ttu-id="151c3-130">Egy /30 alhálózat az elsődleges kapcsolat számára.</span><span class="sxs-lookup"><span data-stu-id="151c3-130">A /30 subnet for the primary link.</span></span> <span data-ttu-id="151c3-131">Az alhálózat nem virtuális hálózatok számára fenntartott címtartomány részének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="151c3-131">The subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="151c3-132">Egy /30 alhálózat a másodlagos kapcsolat számára.</span><span class="sxs-lookup"><span data-stu-id="151c3-132">A /30 subnet for the secondary link.</span></span> <span data-ttu-id="151c3-133">Az alhálózat nem virtuális hálózatok számára fenntartott címtartomány részének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="151c3-133">The subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="151c3-134">Egy érvényes VLAN-azonosító a tárviszony-létesítés létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="151c3-134">A valid VLAN ID to establish this peering on.</span></span> <span data-ttu-id="151c3-135">Győződjön meg róla, hogy a kapcsolatcsoporton egy másik társviszony-létesítés sem használja ugyanezt a VLAN-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="151c3-135">Ensure that no other peering in the circuit uses the same VLAN ID.</span></span>
  * <span data-ttu-id="151c3-136">Egy AS-szám a társviszony-létesítéshez.</span><span class="sxs-lookup"><span data-stu-id="151c3-136">AS number for peering.</span></span> <span data-ttu-id="151c3-137">2 és 4 bájtos AS-számokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="151c3-137">You can use both 2-byte and 4-byte AS numbers.</span></span> <span data-ttu-id="151c3-138">Ehhez a társviszony-létesítéshez használhat privát AS-számokat is.</span><span class="sxs-lookup"><span data-stu-id="151c3-138">You can use a private AS number for this peering.</span></span> <span data-ttu-id="151c3-139">Ne használja a 65515 számot.</span><span class="sxs-lookup"><span data-stu-id="151c3-139">Ensure that you are not using 65515.</span></span>
  * <span data-ttu-id="151c3-140">**Választható -** az MD5 kivonatoló, ha úgy dönt, hogy egyetlen módszer alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="151c3-140">**Optional -** An MD5 hash if you choose to use one.</span></span>
3. <span data-ttu-id="151c3-141">Válassza ki az Azure magánhálózati társviszony-létesítési sorban, a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="151c3-141">Select the Azure Private peering row, as shown in the following example:</span></span>

  ![Magánfelhő](./media/expressroute-howto-routing-portal-resource-manager/rprivate1.png)
4. <span data-ttu-id="151c3-143">Konfigurálja a privát társviszony-létesítést.</span><span class="sxs-lookup"><span data-stu-id="151c3-143">Configure private peering.</span></span> <span data-ttu-id="151c3-144">Az alábbi képen a konfigurációs példát mutat be:</span><span class="sxs-lookup"><span data-stu-id="151c3-144">The following image shows a configuration example:</span></span>

  ![Konfigurálja a magánhálózati társviszony-létesítés](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)
5. <span data-ttu-id="151c3-146">Mentse a konfigurációt, miután megadta az összes paramétert.</span><span class="sxs-lookup"><span data-stu-id="151c3-146">Save the configuration once you have specified all parameters.</span></span> <span data-ttu-id="151c3-147">Miután a konfiguráció sikeresen elfogadva, valami a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="151c3-147">After the configuration has been accepted successfully, you see something similar to the following example:</span></span>

  ![Mentse a magánhálózati társviszony-létesítés](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="to-view-azure-private-peering-details"></a><span data-ttu-id="151c3-149">Azure privát társviszony-létesítés részleteinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="151c3-149">To view Azure private peering details</span></span>

<span data-ttu-id="151c3-150">Az Azure privát társviszony-létesítés tulajdonságainak megtekintéséhez válassza ki a társviszony-létesítést.</span><span class="sxs-lookup"><span data-stu-id="151c3-150">You can view the properties of Azure private peering by selecting the peering.</span></span>

![magánhálózati társviszony-létesítés megtekintése](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="to-update-azure-private-peering-configuration"></a><span data-ttu-id="151c3-152">Azure privát társviszony-létesítés konfigurációjának frissítése</span><span class="sxs-lookup"><span data-stu-id="151c3-152">To update Azure private peering configuration</span></span>

<span data-ttu-id="151c3-153">A társviszony-létesítés sorának kijelölésével módosíthatja a társviszony-létesítés tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="151c3-153">You can select the row for peering and modify the peering properties.</span></span>

![Frissítse a magánhálózati társviszony-létesítés](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)

### <a name="to-delete-azure-private-peering"></a><span data-ttu-id="151c3-155">Azure privát társviszony-létesítés törlése</span><span class="sxs-lookup"><span data-stu-id="151c3-155">To delete Azure private peering</span></span>

<span data-ttu-id="151c3-156">Eltávolíthatja a társviszony-létesítési konfiguráció; ehhez válassza a Törlés ikonra, a következő ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="151c3-156">You can remove your peering configuration by selecting the delete icon, as shown in the following image:</span></span>

![Törölje a magánhálózati társviszony-létesítés](./media/expressroute-howto-routing-portal-resource-manager/rprivate4.png)

## <a name="azure-public-peering"></a><span data-ttu-id="151c3-158">Azure nyilvános társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="151c3-158">Azure public peering</span></span>

<span data-ttu-id="151c3-159">Ez a szakasz segítséget nyújt a létrehozása, beolvasása, frissítése és törlése az Azure nyilvános társviszony-létesítési konfiguráció ExpressRoute-kör.</span><span class="sxs-lookup"><span data-stu-id="151c3-159">This section helps you create, get, update, and delete the Azure public peering configuration for an ExpressRoute circuit.</span></span>

### <a name="to-create-azure-public-peering"></a><span data-ttu-id="151c3-160">Azure nyilvános társviszony-létesítés létrehozása</span><span class="sxs-lookup"><span data-stu-id="151c3-160">To create Azure public peering</span></span>

1. <span data-ttu-id="151c3-161">Konfigurálja az ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="151c3-161">Configure ExpressRoute circuit.</span></span> <span data-ttu-id="151c3-162">A folytatás előtt győződjön meg róla, hogy a kapcsolatszolgáltató teljesen kiépítette a kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="151c3-162">Ensure that the circuit is fully provisioned by the connectivity provider before continuing further.</span></span>

  ![nyilvános társviszony felsorolása](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. <span data-ttu-id="151c3-164">Konfigurálja az Azure nyilvános társviszony-létesítést a kapcsolatcsoporthoz.</span><span class="sxs-lookup"><span data-stu-id="151c3-164">Configure Azure public peering for the circuit.</span></span> <span data-ttu-id="151c3-165">Mielőtt folytatná a következő lépésekkel, ellenőrizze az alábbi elemek meglétét:</span><span class="sxs-lookup"><span data-stu-id="151c3-165">Make sure that you have the following items before you proceed with the next steps:</span></span>

  * <span data-ttu-id="151c3-166">Egy /30 alhálózat az elsődleges kapcsolat számára.</span><span class="sxs-lookup"><span data-stu-id="151c3-166">A /30 subnet for the primary link.</span></span> <span data-ttu-id="151c3-167">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="151c3-167">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="151c3-168">Egy /30 alhálózat a másodlagos kapcsolat számára.</span><span class="sxs-lookup"><span data-stu-id="151c3-168">A /30 subnet for the secondary link.</span></span> <span data-ttu-id="151c3-169">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="151c3-169">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="151c3-170">Egy érvényes VLAN-azonosító a tárviszony-létesítés létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="151c3-170">A valid VLAN ID to establish this peering on.</span></span> <span data-ttu-id="151c3-171">Győződjön meg róla, hogy a kapcsolatcsoporton egy másik társviszony-létesítés sem használja ugyanezt a VLAN-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="151c3-171">Ensure that no other peering in the circuit uses the same VLAN ID.</span></span>
  * <span data-ttu-id="151c3-172">Egy AS-szám a társviszony-létesítéshez.</span><span class="sxs-lookup"><span data-stu-id="151c3-172">AS number for peering.</span></span> <span data-ttu-id="151c3-173">2 és 4 bájtos AS-számokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="151c3-173">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="151c3-174">**Választható -** az MD5 kivonatoló, ha úgy dönt, hogy egyetlen módszer alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="151c3-174">**Optional -** An MD5 hash if you choose to use one.</span></span>
3. <span data-ttu-id="151c3-175">Válassza ki az Azure nyilvános társviszony-létesítési sorban, a következő ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="151c3-175">Select the Azure public peering row, as shown in the following image:</span></span>

  ![Válassza ki a nyilvános társviszony-létesítési sor](./media/expressroute-howto-routing-portal-resource-manager/rpublic1.png)
4. <span data-ttu-id="151c3-177">Konfigurálja a nyilvános társviszony-létesítést.</span><span class="sxs-lookup"><span data-stu-id="151c3-177">Configure public peering.</span></span> <span data-ttu-id="151c3-178">Az alábbi képen a konfigurációs példát mutat be:</span><span class="sxs-lookup"><span data-stu-id="151c3-178">The following image shows a configuration example:</span></span>

  ![Nyilvános társviszony konfigurálása](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)
5. <span data-ttu-id="151c3-180">Mentse a konfigurációt, miután megadta az összes paramétert.</span><span class="sxs-lookup"><span data-stu-id="151c3-180">Save the configuration once you have specified all parameters.</span></span> <span data-ttu-id="151c3-181">Miután a konfiguráció sikeresen elfogadva, valami a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="151c3-181">After the configuration has been accepted successfully, you see something similar to the following example:</span></span>

  ![Nyilvános társviszony-létesítési konfiguráció mentése](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="to-view-azure-public-peering-details"></a><span data-ttu-id="151c3-183">Azure nyilvános társviszony-létesítés részleteinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="151c3-183">To view Azure public peering details</span></span>

<span data-ttu-id="151c3-184">Az Azure nyilvános társviszony-létesítés tulajdonságainak megtekintéséhez válassza ki a társviszony-létesítést.</span><span class="sxs-lookup"><span data-stu-id="151c3-184">You can view the properties of Azure public peering by selecting the peering.</span></span>

![nyilvános társviszony-létesítési tulajdonságainak megtekintése](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="to-update-azure-public-peering-configuration"></a><span data-ttu-id="151c3-186">Azure nyilvános társviszony-létesítés konfigurációjának frissítése</span><span class="sxs-lookup"><span data-stu-id="151c3-186">To update Azure public peering configuration</span></span>

<span data-ttu-id="151c3-187">A társviszony-létesítés sorának kijelölésével módosíthatja a társviszony-létesítés tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="151c3-187">You can select the row for peering and modify the peering properties.</span></span>

![Válassza ki a nyilvános társviszony-létesítési sor](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)

### <a name="to-delete-azure-public-peering"></a><span data-ttu-id="151c3-189">Azure nyilvános társviszony-létesítés törlése</span><span class="sxs-lookup"><span data-stu-id="151c3-189">To delete Azure public peering</span></span>

<span data-ttu-id="151c3-190">Eltávolíthatja a társviszony-létesítési konfiguráció; ehhez válassza a Törlés ikonra, a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="151c3-190">You can remove your peering configuration by selecting the delete icon, as shown in the following example:</span></span>

![nyilvános társviszony törlése](./media/expressroute-howto-routing-portal-resource-manager/rpublic4.png)

## <a name="microsoft-peering"></a><span data-ttu-id="151c3-192">Microsoft társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="151c3-192">Microsoft peering</span></span>

<span data-ttu-id="151c3-193">Ez a szakasz segítséget nyújt a létrehozása, beolvasása, frissítése és a Microsoft társviszony-létesítési konfiguráció az ExpressRoute-kapcsolatcsoportot törli.</span><span class="sxs-lookup"><span data-stu-id="151c3-193">This section helps you create, get, update, and delete the Microsoft peering configuration for an ExpressRoute circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="151c3-194">A Microsoft társviszony-létesítést az ExpressRoute-Kapcsolatcsoportok 2017. augusztus 1. előtt konfigurált meghirdetett keresztül a Microsoft társviszony-létesítést, még akkor is, ha az útvonal-szűrők nem definiált összes szolgáltatás előtagok fog rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="151c3-194">Microsoft peering of ExpressRoute circuits that were configured prior to August 1, 2017 will have all service prefixes advertised through the Microsoft peering, even if route filters are not defined.</span></span> <span data-ttu-id="151c3-195">A Microsoft társviszony-létesítést az ExpressRoute-Kapcsolatcsoportok vannak konfigurálva, vagy azt követően 2017. augusztus 1. nem rendelkezik a előtagokat amíg útvonal szűrő nem csatlakoztatja a kapcsolatcsoport hirdetve.</span><span class="sxs-lookup"><span data-stu-id="151c3-195">Microsoft peering of ExpressRoute circuits that are configured on or after August 1, 2017 will not have any prefixes advertised until a route filter is attached to the circuit.</span></span> <span data-ttu-id="151c3-196">További információkért lásd: [konfigurálása a Microsoft társviszony-létesítéshez útvonal szűrő](how-to-routefilter-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="151c3-196">For more information, see [Configure a route filter for Microsoft peering](how-to-routefilter-powershell.md).</span></span>
> 
> 

### <a name="to-create-microsoft-peering"></a><span data-ttu-id="151c3-197">Microsoft társviszony-létesítés létrehozása</span><span class="sxs-lookup"><span data-stu-id="151c3-197">To create Microsoft peering</span></span>

1. <span data-ttu-id="151c3-198">Konfigurálja az ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="151c3-198">Configure ExpressRoute circuit.</span></span> <span data-ttu-id="151c3-199">A folytatás előtt győződjön meg róla, hogy a kapcsolatszolgáltató teljesen kiépítette a kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="151c3-199">Ensure that the circuit is fully provisioned by the connectivity provider before continuing further.</span></span>

  ![a Microsoft társviszony-létesítés felsorolása](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. <span data-ttu-id="151c3-201">Konfigurálja a Microsoft társviszony-létesítést a kapcsolatcsoporthoz.</span><span class="sxs-lookup"><span data-stu-id="151c3-201">Configure Microsoft peering for the circuit.</span></span> <span data-ttu-id="151c3-202">Mielőtt folytatná, ellenőrizze az alábbi információk meglétét.</span><span class="sxs-lookup"><span data-stu-id="151c3-202">Make sure that you have the following information before you proceed.</span></span>

  * <span data-ttu-id="151c3-203">Egy /30 alhálózat az elsődleges kapcsolat számára.</span><span class="sxs-lookup"><span data-stu-id="151c3-203">A /30 subnet for the primary link.</span></span> <span data-ttu-id="151c3-204">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie, amely az Ön tulajdonában van, és regisztrálva van egy RIR-/IRR-jegyzékben.</span><span class="sxs-lookup"><span data-stu-id="151c3-204">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="151c3-205">Egy /30 alhálózat a másodlagos kapcsolat számára.</span><span class="sxs-lookup"><span data-stu-id="151c3-205">A /30 subnet for the secondary link.</span></span> <span data-ttu-id="151c3-206">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie, amely az Ön tulajdonában van, és regisztrálva van egy RIR-/IRR-jegyzékben.</span><span class="sxs-lookup"><span data-stu-id="151c3-206">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="151c3-207">Egy érvényes VLAN-azonosító a tárviszony-létesítés létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="151c3-207">A valid VLAN ID to establish this peering on.</span></span> <span data-ttu-id="151c3-208">Győződjön meg róla, hogy a kapcsolatcsoporton egy másik társviszony-létesítés sem használja ugyanezt a VLAN-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="151c3-208">Ensure that no other peering in the circuit uses the same VLAN ID.</span></span>
  * <span data-ttu-id="151c3-209">Egy AS-szám a társviszony-létesítéshez.</span><span class="sxs-lookup"><span data-stu-id="151c3-209">AS number for peering.</span></span> <span data-ttu-id="151c3-210">2 és 4 bájtos AS-számokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="151c3-210">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="151c3-211">Meghirdetett előtagok: Meg kell adnia a BGP-munkamenetben meghirdetni kívánt összes előtag listáját.</span><span class="sxs-lookup"><span data-stu-id="151c3-211">Advertised prefixes: You must provide a list of all prefixes you plan to advertise over the BGP session.</span></span> <span data-ttu-id="151c3-212">A rendszer kizárólag a nyilvános IP-cím-előtagokat fogadja el.</span><span class="sxs-lookup"><span data-stu-id="151c3-212">Only public IP address prefixes are accepted.</span></span> <span data-ttu-id="151c3-213">Ha szeretne elküldhető a előtagokat, elküldheti egy vesszővel elválasztott listában.</span><span class="sxs-lookup"><span data-stu-id="151c3-213">If you plan to send a set of prefixes, you can send a comma-separated list.</span></span> <span data-ttu-id="151c3-214">Az előtagoknak egy RIR/IRR jegyzékben regisztrálva kell lenniük az Ön neve alatt.</span><span class="sxs-lookup"><span data-stu-id="151c3-214">These prefixes must be registered to you in an RIR / IRR.</span></span>
  * <span data-ttu-id="151c3-215">**Választható -** ügyfél ASN: Ha nincs regisztrálva a társviszony-létesítés SZÁMOT hirdetési előtagok, megadhatja a AS számot, amelyhez regisztrálja azokat a rendszer.</span><span class="sxs-lookup"><span data-stu-id="151c3-215">**Optional -** Customer ASN: If you are advertising prefixes that are not registered to the peering AS number, you can specify the AS number to which they are registered.</span></span>
  * <span data-ttu-id="151c3-216">Útválasztási jegyzék neve: Megadhatja az RIR/IRR jegyzék nevét, amelyben az AS-szám és az előtagok regisztrálva vannak.</span><span class="sxs-lookup"><span data-stu-id="151c3-216">Routing Registry Name: You can specify the RIR / IRR against which the AS number and prefixes are registered.</span></span>
  * <span data-ttu-id="151c3-217">**Választható -** az MD5 kivonatoló, ha úgy dönt, hogy egyetlen módszer alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="151c3-217">**Optional -** An MD5 hash if you choose to use one.</span></span>
3. <span data-ttu-id="151c3-218">Kiválaszthatja a társviszony-létesítés szeretne beállítani, a következő példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="151c3-218">You can select the peering you wish to configure, as shown in the following example.</span></span> <span data-ttu-id="151c3-219">Válassza ki a Microsoft társviszony-létesítés sorát.</span><span class="sxs-lookup"><span data-stu-id="151c3-219">Select the Microsoft peering row.</span></span>

  ![Válassza ki a Microsoft társviszony-létesítési sor](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft1.png)
4. <span data-ttu-id="151c3-221">Konfigurálja a Microsoft társviszony-beállítást.</span><span class="sxs-lookup"><span data-stu-id="151c3-221">Configure Microsoft peering.</span></span> <span data-ttu-id="151c3-222">Az alábbi képen a konfigurációs példát mutat be:</span><span class="sxs-lookup"><span data-stu-id="151c3-222">The following image shows a configuration example:</span></span>

  ![Konfigurálja a Microsoft társviszony-létesítés](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft2.png)
5. <span data-ttu-id="151c3-224">Mentse a konfigurációt, miután megadta az összes paramétert.</span><span class="sxs-lookup"><span data-stu-id="151c3-224">Save the configuration once you have specified all parameters.</span></span>

  <span data-ttu-id="151c3-225">Ha a kapcsolatcsoport kap-e a "Validation szükséges" állapotba kerül (mivel az ábrán látható), meg kell nyitnia egy támogatási jegy megjelenítése a támogatási csapat az előtagok tulajdonjogát igazolása.</span><span class="sxs-lookup"><span data-stu-id="151c3-225">If your circuit gets to a 'Validation needed' state (as shown in the image), you must open a support ticket to show proof of ownership of the prefixes to our support team.</span></span>

  ![A Microsoft társviszony-létesítési konfiguráció mentése](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft5.png)

  <span data-ttu-id="151c3-227">Közvetlenül a portálról, egy támogatási jegy nyithatja meg a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="151c3-227">You can open a support ticket directly from the portal, as shown in the following example:</span></span>

  ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft6.png)


1. <span data-ttu-id="151c3-228">Miután a konfiguráció sikeresen elfogadva, az alábbi képen hasonló látja:</span><span class="sxs-lookup"><span data-stu-id="151c3-228">After the configuration has been accepted successfully, you see something similar to the following image:</span></span>

  ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="to-view-microsoft-peering-details"></a><span data-ttu-id="151c3-229">Microsoft társviszony-létesítés részleteinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="151c3-229">To view Microsoft peering details</span></span>

<span data-ttu-id="151c3-230">Az Azure nyilvános társviszony-létesítés tulajdonságainak megtekintéséhez válassza ki a társviszony-létesítést.</span><span class="sxs-lookup"><span data-stu-id="151c3-230">You can view the properties of Azure public peering by selecting the peering.</span></span>

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft3.png)

### <a name="to-update-microsoft-peering-configuration"></a><span data-ttu-id="151c3-231">Microsoft társviszony-létesítés konfigurációjának frissítése</span><span class="sxs-lookup"><span data-stu-id="151c3-231">To update Microsoft peering configuration</span></span>

<span data-ttu-id="151c3-232">A társviszony-létesítés sorának kijelölésével módosíthatja a társviszony-létesítés tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="151c3-232">You can select the row for peering and modify the peering properties.</span></span>

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="to-delete-microsoft-peering"></a><span data-ttu-id="151c3-233">Microsoft társviszony-létesítés törlése</span><span class="sxs-lookup"><span data-stu-id="151c3-233">To delete Microsoft peering</span></span>

<span data-ttu-id="151c3-234">Eltávolíthatja a társviszony-létesítési konfiguráció; ehhez válassza a Törlés ikonra, a következő ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="151c3-234">You can remove your peering configuration by selecting the delete icon, as shown in the following image:</span></span>

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft4.png)

## <a name="next-steps"></a><span data-ttu-id="151c3-235">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="151c3-235">Next steps</span></span>

<span data-ttu-id="151c3-236">Következő lépés, [egy virtuális hálózat csatolása ExpressRoute-kapcsolatcsoportot](expressroute-howto-linkvnet-portal-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="151c3-236">Next step, [Link a VNet to an ExpressRoute circuit](expressroute-howto-linkvnet-portal-resource-manager.md)</span></span>
* <span data-ttu-id="151c3-237">Az ExpressRoute-munkafolyamatokkal kapcsolatos további információkért lásd: [ExpressRoute workflows](expressroute-workflows.md) (ExpressRoute-munkafolyamatok).</span><span class="sxs-lookup"><span data-stu-id="151c3-237">For more information about ExpressRoute workflows, see [ExpressRoute workflows](expressroute-workflows.md).</span></span>
* <span data-ttu-id="151c3-238">A kapcsolatcsoportok társviszony-létesítéseivel kapcsolatos további információkért lásd: [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md) (ExpressRoute-kapcsolatcsoportok és útválasztási tartományok).</span><span class="sxs-lookup"><span data-stu-id="151c3-238">For more information about circuit peering, see [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md).</span></span>
* <span data-ttu-id="151c3-239">További információkért a virtuális hálózatok használatáról lásd: [Virtual network overview](../virtual-network/virtual-networks-overview.md) (Virtuális hálózatok áttekintése).</span><span class="sxs-lookup"><span data-stu-id="151c3-239">For more information about working with virtual networks, see [Virtual network overview](../virtual-network/virtual-networks-overview.md).</span></span>
