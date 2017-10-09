---
title: "Hogyan tooconfigure útválasztás (társviszony) az ExpressRoute-kapcsolatcsoportot: erőforrás-kezelő: Azure |} Microsoft Docs"
description: "Ez a cikk végigvezeti hello létrehozásához és a kiépítés hello saját, a nyilvános és a Microsoft társviszony-létesítést az ExpressRoute-kapcsolatcsoportot. Ez a cikk is bemutatja, hogyan toocheck hello állapotát, a frissítést, vagy a-kör társviszony törlése."
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
ms.openlocfilehash: a8ca25f488dde728cb3b06cd2c91873f3069feaf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit"></a><span data-ttu-id="97062-104">Létrehozásához és módosításához az ExpressRoute-kör társviszony</span><span class="sxs-lookup"><span data-stu-id="97062-104">Create and modify peering for an ExpressRoute circuit</span></span>

<span data-ttu-id="97062-105">Ez a cikk segít hozhat létre és kezelhet útválasztási konfigurációja ExpressRoute-kapcsolatcsoportot hello Resource Manager üzembe helyezési modellel hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="97062-105">This article helps you create and manage routing configuration for an ExpressRoute circuit in hello Resource Manager deployment model using hello Azure portal.</span></span> <span data-ttu-id="97062-106">Is hello állapota, update vagy delete ellenőrizze és kiosztásának megszüntetése ExpressRoute-kör társviszony.</span><span class="sxs-lookup"><span data-stu-id="97062-106">You can also check hello status, update, or delete and deprovision peerings for an ExpressRoute circuit.</span></span> <span data-ttu-id="97062-107">Ha egy másik módszer toowork toouse a kapcsolatcsoport rendelkező, jelölje be a cikk a következő lista hello:</span><span class="sxs-lookup"><span data-stu-id="97062-107">If you want toouse a different method toowork with your circuit, select an article from hello following list:</span></span>


## <a name="configuration-prerequisites"></a><span data-ttu-id="97062-108">Konfigurációs előfeltételek</span><span class="sxs-lookup"><span data-stu-id="97062-108">Configuration prerequisites</span></span>

* <span data-ttu-id="97062-109">Győződjön meg arról, hogy átolvasta hello [Előfeltételek](expressroute-prerequisites.md) lap hello [útválasztási követelmények](expressroute-routing.md) lap és hello [munkafolyamatok](expressroute-workflows.md) lapon konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="97062-109">Make sure that you have reviewed hello [prerequisites](expressroute-prerequisites.md) page, hello [routing requirements](expressroute-routing.md) page, and hello [workflows](expressroute-workflows.md) page before you begin configuration.</span></span>
* <span data-ttu-id="97062-110">Egy aktív ExpressRoute-kapcsolatcsoportra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="97062-110">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="97062-111">Útmutatás alapján hello túl[ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-portal-resource-manager.md) , és folytassa a kapcsolat szolgáltatójánál előtt által engedélyezett hello áramkör.</span><span class="sxs-lookup"><span data-stu-id="97062-111">Follow hello instructions too[Create an ExpressRoute circuit](expressroute-howto-circuit-portal-resource-manager.md) and have hello circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="97062-112">hello ExpressRoute-kapcsolatcsoportot meg toobe képes toorun hello parancsmagok a következő szakaszokban hello kiépített és engedélyezett állapotban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="97062-112">hello ExpressRoute circuit must be in a provisioned and enabled state for you toobe able toorun hello cmdlets in hello next sections.</span></span>
* <span data-ttu-id="97062-113">Ha azt tervezi, hogy a megosztott kulcs/MD5 kivonatoló toouse, lehet, hogy toouse ez hello alagút és a limit hello száma karakterek tooa legfeljebb 25 mindkét oldalán.</span><span class="sxs-lookup"><span data-stu-id="97062-113">If you plan toouse a shared key/MD5 hash, be sure toouse this on both sides of hello tunnel and limit hello number of characters tooa maximum of 25.</span></span>

<span data-ttu-id="97062-114">Ezek az utasítások csak a 2. rétegbeli kapcsolatot szolgáltatásokat nyújtó szolgáltatók létre toocircuits vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="97062-114">These instructions only apply toocircuits created with service providers offering Layer 2 connectivity services.</span></span> <span data-ttu-id="97062-115">A szolgáltató által kezelt használata réteg (általában egy IPVPN, például az MPLS) 3 szolgáltatások, a kapcsolat szolgáltatójánál konfigurálja és kezeli az Ön útválasztást.</span><span class="sxs-lookup"><span data-stu-id="97062-115">If you are using a service provider that offers managed Layer 3 services (typically an IPVPN, like MPLS), your connectivity provider configures and manages routing for you.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="97062-116">A Microsoft jelenleg hirdetményt hello felügyeleti portálon keresztül szolgáltatók által konfigurált esetében.</span><span class="sxs-lookup"><span data-stu-id="97062-116">We currently do not advertise peerings configured by service providers through hello service management portal.</span></span> <span data-ttu-id="97062-117">Dolgozunk azon, hogy hamarosan bevezethessük ezt a képességet.</span><span class="sxs-lookup"><span data-stu-id="97062-117">We are working on enabling this capability soon.</span></span> <span data-ttu-id="97062-118">A BGP társviszony konfigurálása előtt ellenőrizze a szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="97062-118">Check with your service provider before configuring BGP peerings.</span></span>
> 
> 

<span data-ttu-id="97062-119">Egy, két vagy akár mindhárom társviszony-létesítést (Azure privát, Azure nyilvános és Microsoft) is konfigurálhatja egy adott ExpressRoute-kapcsolatcsoportban.</span><span class="sxs-lookup"><span data-stu-id="97062-119">You can configure one, two, or all three peerings (Azure private, Azure public and Microsoft) for an ExpressRoute circuit.</span></span> <span data-ttu-id="97062-120">A társviszony-létesítéseket tetszőleges sorrendben konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="97062-120">You can configure peerings in any order you choose.</span></span> <span data-ttu-id="97062-121">Azonban kell győződjön meg arról, hogy elvégezte-e minden társviszony-létesítési egyszerre csak egy hello konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="97062-121">However, you must make sure that you complete hello configuration of each peering one at a time.</span></span>

## <a name="azure-private-peering"></a><span data-ttu-id="97062-122">Azure privát társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="97062-122">Azure private peering</span></span>

<span data-ttu-id="97062-123">Ez a szakasz segítséget nyújt a létrehozása, beolvasása, frissítése és törlése hello Azure privát társviszony-létesítési konfiguráció ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="97062-123">This section helps you create, get, update, and delete hello Azure private peering configuration for an ExpressRoute circuit.</span></span>

### <a name="toocreate-azure-private-peering"></a><span data-ttu-id="97062-124">az Azure magánhálózati társviszony-létesítés toocreate</span><span class="sxs-lookup"><span data-stu-id="97062-124">toocreate Azure private peering</span></span>

1. <span data-ttu-id="97062-125">ExpressRoute-kapcsolatcsoportot hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="97062-125">Configure hello ExpressRoute circuit.</span></span> <span data-ttu-id="97062-126">Bejelölheti, hello áramkör teljesen hello kapcsolat szolgáltató folytatása előtt.</span><span class="sxs-lookup"><span data-stu-id="97062-126">Ensure that hello circuit is fully provisioned by hello connectivity provider before continuing.</span></span>

  ![lista](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. <span data-ttu-id="97062-128">Az Azure magánhálózati társviszony-létesítés hello kör megadása</span><span class="sxs-lookup"><span data-stu-id="97062-128">Configure Azure private peering for hello circuit.</span></span> <span data-ttu-id="97062-129">Győződjön meg arról, hogy rendelkezik-e hello hello lépések folytatása előtt a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="97062-129">Make sure that you have hello following items before you proceed with hello next steps:</span></span>

  * <span data-ttu-id="97062-130">/ 30-as alhálózat hello elsődleges kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="97062-130">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="97062-131">hello alhálózat nem virtuális hálózatok számára fenntartott címtartomány részének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="97062-131">hello subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="97062-132">/ 30-as alhálózat hello másodlagos kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="97062-132">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="97062-133">hello alhálózat nem virtuális hálózatok számára fenntartott címtartomány részének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="97062-133">hello subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="97062-134">Egy érvényes VLAN-azonosító tooestablish a társviszony.</span><span class="sxs-lookup"><span data-stu-id="97062-134">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="97062-135">Győződjön meg arról, hogy egyetlen másik társviszonya se a hello áramkör használ hello azonos VLAN-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="97062-135">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
  * <span data-ttu-id="97062-136">Egy AS-szám a társviszony-létesítéshez.</span><span class="sxs-lookup"><span data-stu-id="97062-136">AS number for peering.</span></span> <span data-ttu-id="97062-137">2 és 4 bájtos AS-számokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="97062-137">You can use both 2-byte and 4-byte AS numbers.</span></span> <span data-ttu-id="97062-138">Ehhez a társviszony-létesítéshez használhat privát AS-számokat is.</span><span class="sxs-lookup"><span data-stu-id="97062-138">You can use a private AS number for this peering.</span></span> <span data-ttu-id="97062-139">Ne használja a 65515 számot.</span><span class="sxs-lookup"><span data-stu-id="97062-139">Ensure that you are not using 65515.</span></span>
  * <span data-ttu-id="97062-140">**Választható -** az MD5 kivonatoló, ha úgy dönt, hogy egy toouse.</span><span class="sxs-lookup"><span data-stu-id="97062-140">**Optional -** An MD5 hash if you choose toouse one.</span></span>
3. <span data-ttu-id="97062-141">Válassza ki a hello Azure magánhálózati társviszony-létesítési sorban, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="97062-141">Select hello Azure Private peering row, as shown in hello following example:</span></span>

  ![Magánfelhő](./media/expressroute-howto-routing-portal-resource-manager/rprivate1.png)
4. <span data-ttu-id="97062-143">Konfigurálja a privát társviszony-létesítést.</span><span class="sxs-lookup"><span data-stu-id="97062-143">Configure private peering.</span></span> <span data-ttu-id="97062-144">a következő kép hello egy konfigurációs példát mutat be:</span><span class="sxs-lookup"><span data-stu-id="97062-144">hello following image shows a configuration example:</span></span>

  ![Konfigurálja a magánhálózati társviszony-létesítés](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)
5. <span data-ttu-id="97062-146">Hello konfiguráció mentéséhez, miután megadta az összes paramétert.</span><span class="sxs-lookup"><span data-stu-id="97062-146">Save hello configuration once you have specified all parameters.</span></span> <span data-ttu-id="97062-147">Hello konfigurációja sikeresen elfogadva, miután a következő példa valami hasonló toohello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="97062-147">After hello configuration has been accepted successfully, you see something similar toohello following example:</span></span>

  ![Mentse a magánhálózati társviszony-létesítés](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="tooview-azure-private-peering-details"></a><span data-ttu-id="97062-149">tooview Azure magánhálózati társviszony-létesítés részletei</span><span class="sxs-lookup"><span data-stu-id="97062-149">tooview Azure private peering details</span></span>

<span data-ttu-id="97062-150">Azure magánhálózati társviszony-létesítés hello társviszony-létesítés kiválasztásával hello tulajdonságait tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="97062-150">You can view hello properties of Azure private peering by selecting hello peering.</span></span>

![magánhálózati társviszony-létesítés megtekintése](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="tooupdate-azure-private-peering-configuration"></a><span data-ttu-id="97062-152">tooupdate Azure magánhálózati társviszony-létesítési konfiguráció</span><span class="sxs-lookup"><span data-stu-id="97062-152">tooupdate Azure private peering configuration</span></span>

<span data-ttu-id="97062-153">Jelölje ki a társviszony-létesítéshez hello sort, és társviszony-létesítési hello tulajdonságainak módosítása.</span><span class="sxs-lookup"><span data-stu-id="97062-153">You can select hello row for peering and modify hello peering properties.</span></span>

![Frissítse a magánhálózati társviszony-létesítés](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)

### <a name="toodelete-azure-private-peering"></a><span data-ttu-id="97062-155">az Azure magánhálózati társviszony-létesítés toodelete</span><span class="sxs-lookup"><span data-stu-id="97062-155">toodelete Azure private peering</span></span>

<span data-ttu-id="97062-156">Eltávolíthatja a társviszony-létesítési konfiguráció kiválasztásával hello Törlés ikonra, ahogy az a következő kép hello:</span><span class="sxs-lookup"><span data-stu-id="97062-156">You can remove your peering configuration by selecting hello delete icon, as shown in hello following image:</span></span>

![Törölje a magánhálózati társviszony-létesítés](./media/expressroute-howto-routing-portal-resource-manager/rprivate4.png)

## <a name="azure-public-peering"></a><span data-ttu-id="97062-158">Azure nyilvános társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="97062-158">Azure public peering</span></span>

<span data-ttu-id="97062-159">Ez a szakasz segítséget nyújt a létrehozása, beolvasása, frissítése és hello Azure nyilvános társviszony-létesítési konfiguráció ExpressRoute-kapcsolatcsoportot törli.</span><span class="sxs-lookup"><span data-stu-id="97062-159">This section helps you create, get, update, and delete hello Azure public peering configuration for an ExpressRoute circuit.</span></span>

### <a name="toocreate-azure-public-peering"></a><span data-ttu-id="97062-160">az Azure nyilvános társviszony toocreate</span><span class="sxs-lookup"><span data-stu-id="97062-160">toocreate Azure public peering</span></span>

1. <span data-ttu-id="97062-161">Konfigurálja az ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="97062-161">Configure ExpressRoute circuit.</span></span> <span data-ttu-id="97062-162">Bejelölheti, hello áramkör teljesen hello kapcsolat szolgáltató további folytatása előtt.</span><span class="sxs-lookup"><span data-stu-id="97062-162">Ensure that hello circuit is fully provisioned by hello connectivity provider before continuing further.</span></span>

  ![nyilvános társviszony felsorolása](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. <span data-ttu-id="97062-164">Az Azure nyilvános társviszony-létesítés hello kör megadása</span><span class="sxs-lookup"><span data-stu-id="97062-164">Configure Azure public peering for hello circuit.</span></span> <span data-ttu-id="97062-165">Győződjön meg arról, hogy rendelkezik-e hello hello lépések folytatása előtt a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="97062-165">Make sure that you have hello following items before you proceed with hello next steps:</span></span>

  * <span data-ttu-id="97062-166">/ 30-as alhálózat hello elsődleges kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="97062-166">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="97062-167">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="97062-167">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="97062-168">/ 30-as alhálózat hello másodlagos kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="97062-168">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="97062-169">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="97062-169">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="97062-170">Egy érvényes VLAN-azonosító tooestablish a társviszony.</span><span class="sxs-lookup"><span data-stu-id="97062-170">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="97062-171">Győződjön meg arról, hogy egyetlen másik társviszonya se a hello áramkör használ hello azonos VLAN-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="97062-171">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
  * <span data-ttu-id="97062-172">Egy AS-szám a társviszony-létesítéshez.</span><span class="sxs-lookup"><span data-stu-id="97062-172">AS number for peering.</span></span> <span data-ttu-id="97062-173">2 és 4 bájtos AS-számokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="97062-173">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="97062-174">**Választható -** az MD5 kivonatoló, ha úgy dönt, hogy egy toouse.</span><span class="sxs-lookup"><span data-stu-id="97062-174">**Optional -** An MD5 hash if you choose toouse one.</span></span>
3. <span data-ttu-id="97062-175">Válassza ki az Azure nyilvános társviszony-létesítési sor hello, ahogy az a következő kép hello:</span><span class="sxs-lookup"><span data-stu-id="97062-175">Select hello Azure public peering row, as shown in hello following image:</span></span>

  ![Válassza ki a nyilvános társviszony-létesítési sor](./media/expressroute-howto-routing-portal-resource-manager/rpublic1.png)
4. <span data-ttu-id="97062-177">Konfigurálja a nyilvános társviszony-létesítést.</span><span class="sxs-lookup"><span data-stu-id="97062-177">Configure public peering.</span></span> <span data-ttu-id="97062-178">a következő kép hello egy konfigurációs példát mutat be:</span><span class="sxs-lookup"><span data-stu-id="97062-178">hello following image shows a configuration example:</span></span>

  ![Nyilvános társviszony konfigurálása](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)
5. <span data-ttu-id="97062-180">Hello konfiguráció mentéséhez, miután megadta az összes paramétert.</span><span class="sxs-lookup"><span data-stu-id="97062-180">Save hello configuration once you have specified all parameters.</span></span> <span data-ttu-id="97062-181">Hello konfigurációja sikeresen elfogadva, miután a következő példa valami hasonló toohello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="97062-181">After hello configuration has been accepted successfully, you see something similar toohello following example:</span></span>

  ![Nyilvános társviszony-létesítési konfiguráció mentése](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="tooview-azure-public-peering-details"></a><span data-ttu-id="97062-183">tooview Azure nyilvános társviszony-létesítés részletei</span><span class="sxs-lookup"><span data-stu-id="97062-183">tooview Azure public peering details</span></span>

<span data-ttu-id="97062-184">Az Azure nyilvános társviszony-létesítés hello társviszony-létesítés kiválasztásával hello tulajdonságait tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="97062-184">You can view hello properties of Azure public peering by selecting hello peering.</span></span>

![nyilvános társviszony-létesítési tulajdonságainak megtekintése](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="tooupdate-azure-public-peering-configuration"></a><span data-ttu-id="97062-186">az Azure nyilvános társviszony-létesítési konfiguráció tooupdate</span><span class="sxs-lookup"><span data-stu-id="97062-186">tooupdate Azure public peering configuration</span></span>

<span data-ttu-id="97062-187">Jelölje ki a társviszony-létesítéshez hello sort, és társviszony-létesítési hello tulajdonságainak módosítása.</span><span class="sxs-lookup"><span data-stu-id="97062-187">You can select hello row for peering and modify hello peering properties.</span></span>

![Válassza ki a nyilvános társviszony-létesítési sor](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)

### <a name="toodelete-azure-public-peering"></a><span data-ttu-id="97062-189">az Azure nyilvános társviszony toodelete</span><span class="sxs-lookup"><span data-stu-id="97062-189">toodelete Azure public peering</span></span>

<span data-ttu-id="97062-190">Eltávolíthatja a társviszony-létesítési konfiguráció hello Törlés ikonra, kiválasztásával, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="97062-190">You can remove your peering configuration by selecting hello delete icon, as shown in hello following example:</span></span>

![nyilvános társviszony törlése](./media/expressroute-howto-routing-portal-resource-manager/rpublic4.png)

## <a name="microsoft-peering"></a><span data-ttu-id="97062-192">Microsoft társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="97062-192">Microsoft peering</span></span>

<span data-ttu-id="97062-193">Ez a szakasz segítséget nyújt a létrehozása, beolvasása, frissítése és törlése hello Microsoft társviszony-létesítési konfiguráció ExpressRoute-kör.</span><span class="sxs-lookup"><span data-stu-id="97062-193">This section helps you create, get, update, and delete hello Microsoft peering configuration for an ExpressRoute circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="97062-194">Előzetes tooAugust 1 Microsoft társviszony-létesítést az ExpressRoute-Kapcsolatcsoportok volt beállítva, akkor 2017 fog rendelkezni az összes szolgáltatás előtagok keresztül hello Microsoft társviszony-létesítést, meghirdetett, akkor is, ha nincsenek megadva útvonal szűrők.</span><span class="sxs-lookup"><span data-stu-id="97062-194">Microsoft peering of ExpressRoute circuits that were configured prior tooAugust 1, 2017 will have all service prefixes advertised through hello Microsoft peering, even if route filters are not defined.</span></span> <span data-ttu-id="97062-195">A Microsoft társviszony-létesítést az ExpressRoute-Kapcsolatcsoportok vannak konfigurálva, vagy azt követően 2017. augusztus 1. nem rendelkezik a előtagokat meghirdetett, amíg egy útvonal-szűrő nem csatlakoztatja toohello körön.</span><span class="sxs-lookup"><span data-stu-id="97062-195">Microsoft peering of ExpressRoute circuits that are configured on or after August 1, 2017 will not have any prefixes advertised until a route filter is attached toohello circuit.</span></span> <span data-ttu-id="97062-196">További információkért lásd: [konfigurálása a Microsoft társviszony-létesítéshez útvonal szűrő](how-to-routefilter-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="97062-196">For more information, see [Configure a route filter for Microsoft peering](how-to-routefilter-powershell.md).</span></span>
> 
> 

### <a name="toocreate-microsoft-peering"></a><span data-ttu-id="97062-197">toocreate Microsoft társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="97062-197">toocreate Microsoft peering</span></span>

1. <span data-ttu-id="97062-198">Konfigurálja az ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="97062-198">Configure ExpressRoute circuit.</span></span> <span data-ttu-id="97062-199">Bejelölheti, hello áramkör teljesen hello kapcsolat szolgáltató további folytatása előtt.</span><span class="sxs-lookup"><span data-stu-id="97062-199">Ensure that hello circuit is fully provisioned by hello connectivity provider before continuing further.</span></span>

  ![a Microsoft társviszony-létesítés felsorolása](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. <span data-ttu-id="97062-201">Konfigurálja a Microsoft hello-kör társviszony-létesítés.</span><span class="sxs-lookup"><span data-stu-id="97062-201">Configure Microsoft peering for hello circuit.</span></span> <span data-ttu-id="97062-202">Győződjön meg arról, hogy rendelkezik-e folytatni a következő információk előtt hello.</span><span class="sxs-lookup"><span data-stu-id="97062-202">Make sure that you have hello following information before you proceed.</span></span>

  * <span data-ttu-id="97062-203">/ 30-as alhálózat hello elsődleges kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="97062-203">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="97062-204">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie, amely az Ön tulajdonában van, és regisztrálva van egy RIR-/IRR-jegyzékben.</span><span class="sxs-lookup"><span data-stu-id="97062-204">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="97062-205">/ 30-as alhálózat hello másodlagos kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="97062-205">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="97062-206">Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie, amely az Ön tulajdonában van, és regisztrálva van egy RIR-/IRR-jegyzékben.</span><span class="sxs-lookup"><span data-stu-id="97062-206">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="97062-207">Egy érvényes VLAN-azonosító tooestablish a társviszony.</span><span class="sxs-lookup"><span data-stu-id="97062-207">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="97062-208">Győződjön meg arról, hogy egyetlen másik társviszonya se a hello áramkör használ hello azonos VLAN-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="97062-208">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
  * <span data-ttu-id="97062-209">Egy AS-szám a társviszony-létesítéshez.</span><span class="sxs-lookup"><span data-stu-id="97062-209">AS number for peering.</span></span> <span data-ttu-id="97062-210">2 és 4 bájtos AS-számokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="97062-210">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="97062-211">Meghirdetett előtagok: meg kell adnia a lista az összes előtagok hello BGP munkameneten keresztül tervezi tooadvertise.</span><span class="sxs-lookup"><span data-stu-id="97062-211">Advertised prefixes: You must provide a list of all prefixes you plan tooadvertise over hello BGP session.</span></span> <span data-ttu-id="97062-212">A rendszer kizárólag a nyilvános IP-cím-előtagokat fogadja el.</span><span class="sxs-lookup"><span data-stu-id="97062-212">Only public IP address prefixes are accepted.</span></span> <span data-ttu-id="97062-213">Ha azt tervezi, toosend előtagok készlete, elküldheti a vesszővel tagolt listáját.</span><span class="sxs-lookup"><span data-stu-id="97062-213">If you plan toosend a set of prefixes, you can send a comma-separated list.</span></span> <span data-ttu-id="97062-214">Ezeket az előtagokat kell lenniük egy RIR-ben regisztrált tooyou vagy IRR-ben.</span><span class="sxs-lookup"><span data-stu-id="97062-214">These prefixes must be registered tooyou in an RIR / IRR.</span></span>
  * <span data-ttu-id="97062-215">**Választható -** ügyfél ASN: Ha hirdetési-előtagok nem regisztrált toohello társviszony-létesítés SZÁMOT, megadhatja hello számú toowhich regisztrálva vannak.</span><span class="sxs-lookup"><span data-stu-id="97062-215">**Optional -** Customer ASN: If you are advertising prefixes that are not registered toohello peering AS number, you can specify hello AS number toowhich they are registered.</span></span>
  * <span data-ttu-id="97062-216">Útválasztási beállításjegyzék-név: Hello RIR megadhat / BMR mely hello ellen, számát, és előtagok regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="97062-216">Routing Registry Name: You can specify hello RIR / IRR against which hello AS number and prefixes are registered.</span></span>
  * <span data-ttu-id="97062-217">**Választható -** az MD5 kivonatoló, ha úgy dönt, hogy egy toouse.</span><span class="sxs-lookup"><span data-stu-id="97062-217">**Optional -** An MD5 hash if you choose toouse one.</span></span>
3. <span data-ttu-id="97062-218">Kiválaszthatja a hello társviszony-létesítés kívánja tooconfigure, ahogy az alábbi hello példa.</span><span class="sxs-lookup"><span data-stu-id="97062-218">You can select hello peering you wish tooconfigure, as shown in hello following example.</span></span> <span data-ttu-id="97062-219">Válassza ki a Microsoft társviszony-létesítési sor hello.</span><span class="sxs-lookup"><span data-stu-id="97062-219">Select hello Microsoft peering row.</span></span>

  ![Válassza ki a Microsoft társviszony-létesítési sor](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft1.png)
4. <span data-ttu-id="97062-221">Konfigurálja a Microsoft társviszony-beállítást.</span><span class="sxs-lookup"><span data-stu-id="97062-221">Configure Microsoft peering.</span></span> <span data-ttu-id="97062-222">a következő kép hello egy konfigurációs példát mutat be:</span><span class="sxs-lookup"><span data-stu-id="97062-222">hello following image shows a configuration example:</span></span>

  ![Konfigurálja a Microsoft társviszony-létesítés](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft2.png)
5. <span data-ttu-id="97062-224">Hello konfiguráció mentéséhez, miután megadta az összes paramétert.</span><span class="sxs-lookup"><span data-stu-id="97062-224">Save hello configuration once you have specified all parameters.</span></span>

  <span data-ttu-id="97062-225">Ha a kapcsolatcsoport lekérdezi tooa "Validation szükséges" állapotba kerül (hello ábrának megfelelően), meg kell nyitnia egy támogatási jegy tooshow igazoló hello előtagok tooour támogatási csoport tulajdonjogának.</span><span class="sxs-lookup"><span data-stu-id="97062-225">If your circuit gets tooa 'Validation needed' state (as shown in hello image), you must open a support ticket tooshow proof of ownership of hello prefixes tooour support team.</span></span>

  ![A Microsoft társviszony-létesítési konfiguráció mentése](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft5.png)

  <span data-ttu-id="97062-227">Támogatási jegy megnyithatja közvetlenül portálról hello, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="97062-227">You can open a support ticket directly from hello portal, as shown in hello following example:</span></span>

  ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft6.png)


1. <span data-ttu-id="97062-228">Miután hello konfigurációja sikeresen elfogadva, valami hasonló toohello kép a következő jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="97062-228">After hello configuration has been accepted successfully, you see something similar toohello following image:</span></span>

  ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="tooview-microsoft-peering-details"></a><span data-ttu-id="97062-229">tooview Microsoft társviszony-létesítési részletei</span><span class="sxs-lookup"><span data-stu-id="97062-229">tooview Microsoft peering details</span></span>

<span data-ttu-id="97062-230">Az Azure nyilvános társviszony-létesítés hello társviszony-létesítés kiválasztásával hello tulajdonságait tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="97062-230">You can view hello properties of Azure public peering by selecting hello peering.</span></span>

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft3.png)

### <a name="tooupdate-microsoft-peering-configuration"></a><span data-ttu-id="97062-231">a Microsoft társviszony-létesítési konfiguráció tooupdate</span><span class="sxs-lookup"><span data-stu-id="97062-231">tooupdate Microsoft peering configuration</span></span>

<span data-ttu-id="97062-232">Jelölje ki a társviszony-létesítéshez hello sort, és társviszony-létesítési hello tulajdonságainak módosítása.</span><span class="sxs-lookup"><span data-stu-id="97062-232">You can select hello row for peering and modify hello peering properties.</span></span>

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="toodelete-microsoft-peering"></a><span data-ttu-id="97062-233">toodelete Microsoft társviszony-létesítés</span><span class="sxs-lookup"><span data-stu-id="97062-233">toodelete Microsoft peering</span></span>

<span data-ttu-id="97062-234">Eltávolíthatja a társviszony-létesítési konfiguráció kiválasztásával hello Törlés ikonra, ahogy az a következő kép hello:</span><span class="sxs-lookup"><span data-stu-id="97062-234">You can remove your peering configuration by selecting hello delete icon, as shown in hello following image:</span></span>

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft4.png)

## <a name="next-steps"></a><span data-ttu-id="97062-235">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="97062-235">Next steps</span></span>

<span data-ttu-id="97062-236">Következő lépés, [csatolása a virtuális hálózat tooan ExpressRoute-kapcsolatcsoportot](expressroute-howto-linkvnet-portal-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="97062-236">Next step, [Link a VNet tooan ExpressRoute circuit](expressroute-howto-linkvnet-portal-resource-manager.md)</span></span>
* <span data-ttu-id="97062-237">Az ExpressRoute-munkafolyamatokkal kapcsolatos további információkért lásd: [ExpressRoute workflows](expressroute-workflows.md) (ExpressRoute-munkafolyamatok).</span><span class="sxs-lookup"><span data-stu-id="97062-237">For more information about ExpressRoute workflows, see [ExpressRoute workflows](expressroute-workflows.md).</span></span>
* <span data-ttu-id="97062-238">A kapcsolatcsoportok társviszony-létesítéseivel kapcsolatos további információkért lásd: [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md) (ExpressRoute-kapcsolatcsoportok és útválasztási tartományok).</span><span class="sxs-lookup"><span data-stu-id="97062-238">For more information about circuit peering, see [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md).</span></span>
* <span data-ttu-id="97062-239">További információkért a virtuális hálózatok használatáról lásd: [Virtual network overview](../virtual-network/virtual-networks-overview.md) (Virtuális hálózatok áttekintése).</span><span class="sxs-lookup"><span data-stu-id="97062-239">For more information about working with virtual networks, see [Virtual network overview](../virtual-network/virtual-networks-overview.md).</span></span>
