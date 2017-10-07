---
title: "aaaAzure előfizetés korlátozásai és a kvóták |} Microsoft Docs"
description: "A közös Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések listáját tartalmazza. Ide tartoznak a hogyan tooincrease korlátozza a maximális értékek együtt."
services: 
documentationcenter: 
author: rothja
manager: jeffreyg
editor: 
tags: billing
ms.assetid: 60d848f9-ff26-496e-a5ec-ccf92ad7d125
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: byvinyal
ms.openlocfilehash: a754d56124520791254ab8f1729808f0750ff222
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a><span data-ttu-id="07fef-104">Az Azure-előfizetésekre és -szolgáltatásokra vonatkozó korlátozások, kvóták és megkötések</span><span class="sxs-lookup"><span data-stu-id="07fef-104">Azure subscription and service limits, quotas, and constraints</span></span>
<span data-ttu-id="07fef-105">Ez a dokumentum soroljuk hello leggyakrabban használt Microsoft Azure-korlátok, kvóták néven is ismert.</span><span class="sxs-lookup"><span data-stu-id="07fef-105">This document lists some of hello most common Microsoft Azure limits, which are also sometimes called quotas.</span></span> <span data-ttu-id="07fef-106">Ez a dokumentum jelenleg nem fedi le az összes Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="07fef-106">This document doesn't currently cover all Azure services.</span></span> <span data-ttu-id="07fef-107">Az idő múlásával hello lista kibontásra váró, és hello platform további toocover frissítése.</span><span class="sxs-lookup"><span data-stu-id="07fef-107">Over time, hello list will be expanded and updated toocover more of hello platform.</span></span>

<span data-ttu-id="07fef-108">Látogasson el a [Azure díjszabása áttekintése](https://azure.microsoft.com/pricing/) további információk az Azure-beli árakról toolearn.</span><span class="sxs-lookup"><span data-stu-id="07fef-108">Please visit [Azure Pricing Overview](https://azure.microsoft.com/pricing/) toolearn more about Azure pricing.</span></span> <span data-ttu-id="07fef-109">Van, a költségek hello használatával megbecsülheti [Díjkalkulátor](https://azure.microsoft.com/pricing/calculator/) vagy hello díjszabás részleteit megjelenítő oldalon a szolgáltatás számára (például [Windows virtuális gépek](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)).</span><span class="sxs-lookup"><span data-stu-id="07fef-109">There, you can estimate your costs using hello [Pricing Calculator](https://azure.microsoft.com/pricing/calculator/) or by visiting hello pricing details page for a service (for example, [Windows VMs](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)).</span></span> <span data-ttu-id="07fef-110">A tippek toohelp a költségeinek kezelése című [Azure számlázás és költség felügyeleti váratlan költségek megakadályozása](billing/billing-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="07fef-110">For tips toohelp manage your costs, see [Prevent unexpected costs with Azure billing and cost management](billing/billing-getting-started.md).</span></span>

> [!NOTE]
> <span data-ttu-id="07fef-111">Ha azt szeretné, tooraise hello korlátot vagy a fenti hello kvóta **alapértelmezett korlát**, [nyissa meg az online támogatás ügyfélkérés díjmentesen](azure-supportability/resource-manager-core-quotas-request.md).</span><span class="sxs-lookup"><span data-stu-id="07fef-111">If you want tooraise hello limit or quota above hello **Default Limit**, [open an online customer support request at no charge](azure-supportability/resource-manager-core-quotas-request.md).</span></span> <span data-ttu-id="07fef-112">hello korlátai nem léptethető fent hello **maximális** a következő táblák hello szereplő érték.</span><span class="sxs-lookup"><span data-stu-id="07fef-112">hello limits can't be raised above hello **Maximum Limit** value shown in hello following tables.</span></span> <span data-ttu-id="07fef-113">Ha nincs **maximális** oszlopban, majd hello erőforrás nem állítható korlátokkal rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="07fef-113">If there is no **Maximum Limit** column, then hello resource doesn't have adjustable limits.</span></span> 
> 
> <span data-ttu-id="07fef-114">Ingyenes próba-előfizetések nem jogosultak a korlátot, vagy kvóta növeli.</span><span class="sxs-lookup"><span data-stu-id="07fef-114">Free Trial subscriptions are not eligible for limit or quota increases.</span></span> <span data-ttu-id="07fef-115">Ha egy ingyenes próbaverziót, frissítheti tooa [használatalapú fizetés](https://azure.microsoft.com/offers/ms-azr-0003p/) előfizetés.</span><span class="sxs-lookup"><span data-stu-id="07fef-115">If you have a Free Trial, you can upgrade tooa [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) subscription.</span></span> <span data-ttu-id="07fef-116">További információkért lásd: [Azure ingyenes próbaverzió frissítése tooPay-,-akkor-Ugrás](billing/billing-upgrade-azure-subscription.md).</span><span class="sxs-lookup"><span data-stu-id="07fef-116">For more information, see [Upgrade Azure Free Trial tooPay-As-You-Go](billing/billing-upgrade-azure-subscription.md).</span></span>
> 

## <a name="limits-and-hello-azure-resource-manager"></a><span data-ttu-id="07fef-117">Korlátozásai és hello Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="07fef-117">Limits and hello Azure Resource Manager</span></span>
<span data-ttu-id="07fef-118">Már lehetséges toocombine tooa a több Azure-erőforrások egy Azure-erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="07fef-118">It is now possible toocombine multiple Azure resources in tooa single Azure Resource Group.</span></span> <span data-ttu-id="07fef-119">Erőforráscsoportok használatakor egyszer volt a globális korlátozza az Azure Resource Manager hello regionális szintű felügyelhető legyen.</span><span class="sxs-lookup"><span data-stu-id="07fef-119">When using Resource Groups, limits that once were global become managed at a regional level with hello Azure Resource Manager.</span></span> <span data-ttu-id="07fef-120">Azure erőforráscsoport-sablonok kapcsolatos további információkért lásd: [Azure Resource Manager áttekintése](azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="07fef-120">For more information about Azure Resource Groups, see [Azure Resource Manager overview](azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="07fef-121">Az alábbi új tábla hello korlátok lett hozzáadott tooreflect összes különbséget korlátok hello Azure Resource Manager használata esetén.</span><span class="sxs-lookup"><span data-stu-id="07fef-121">In hello limits below, a new table has been added tooreflect any differences in limits when using hello Azure Resource Manager.</span></span> <span data-ttu-id="07fef-122">Például van egy **előfizetési korlátozásait** tábla és egy **előfizetési korlátozásait - Azure Resource Manager** tábla.</span><span class="sxs-lookup"><span data-stu-id="07fef-122">For example, there is a **Subscription Limits** table and a **Subscription Limits - Azure Resource Manager** table.</span></span> <span data-ttu-id="07fef-123">Ha a megadott korlát érvényes tooboth forgatókönyvek, csak látható hello első tábla.</span><span class="sxs-lookup"><span data-stu-id="07fef-123">When a limit applies tooboth scenarios, it is only shown in hello first table.</span></span> <span data-ttu-id="07fef-124">Hiányában korlátok legyenek globális minden régióban.</span><span class="sxs-lookup"><span data-stu-id="07fef-124">Unless otherwise indicated, limits are global across all regions.</span></span>

> [!NOTE]
> <span data-ttu-id="07fef-125">Ez megegyezik, hogy Azure erőforráscsoport-sablonok az erőforrásokra vonatkozó kvótákat /-régióban elérhető-e az előfizetés, és nem előfizetésenként, fontos tooemphasize hello szolgáltatás felügyeleti kvóták.</span><span class="sxs-lookup"><span data-stu-id="07fef-125">It is important tooemphasize that quotas for resources in Azure Resource Groups are per-region accessible by your subscription, and are not per-subscription, as hello service management quotas are.</span></span> <span data-ttu-id="07fef-126">Most használja core kvóták példaként.</span><span class="sxs-lookup"><span data-stu-id="07fef-126">Let's use core quotas as an example.</span></span> <span data-ttu-id="07fef-127">Ha a kvóta növeléséhez magok támogatása toorequest van szüksége, akkor toodecide hogyan szeretné, hogy melyik régióban toouse, és végezze el egy adott kérelem az Azure erőforráscsoport sok magok kvóták alapvető hello összegeket és a kívánt régiók.</span><span class="sxs-lookup"><span data-stu-id="07fef-127">If you need toorequest a quota increase with support for cores, you need toodecide how many cores you want toouse in which regions, and then make a specific request for Azure Resource Group core quotas for hello amounts and regions that you want.</span></span> <span data-ttu-id="07fef-128">Ezért ha toouse 30 kell processzormag, Nyugat-Európában toorun a az alkalmazás Nyugat-Európában 30 magok kifejezetten igényeljen.</span><span class="sxs-lookup"><span data-stu-id="07fef-128">Therefore, if you need toouse 30 cores in West Europe toorun your application there; you should specifically request 30 cores in West Europe.</span></span> <span data-ttu-id="07fef-129">Azonban Ön nem rendelkezik a core kvóta növelése más régióban – csak Nyugat-Európában hello 30-core kvóta lesz.</span><span class="sxs-lookup"><span data-stu-id="07fef-129">But you will not have a core quota increase in any other region -- only West Europe will have hello 30-core quota.</span></span>
> <!-- -->
> <span data-ttu-id="07fef-130">Ennek eredményeképpen előfordulhat azt hasznos tooconsider dönt, hogy mi az Azure-erőforráscsoport kvóták kell toobe minden olyan egy régió tartozik, és kérelmet, amely minden régióban, amelybe a központi telepítés tervezi összeg a munkaterhelés számára.</span><span class="sxs-lookup"><span data-stu-id="07fef-130">As a result, you may find it useful tooconsider deciding what your Azure Resource Group quotas need toobe for your workload in any one region, and request that amount in each region into which you are considering deployment.</span></span> <span data-ttu-id="07fef-131">Lásd: [telepítési problémák elhárítása](resource-manager-common-deployment-errors.md) további segítséget itt találhat az aktuális kvóták adott régióban felderítéséhez.</span><span class="sxs-lookup"><span data-stu-id="07fef-131">See [troubleshooting deployment issues](resource-manager-common-deployment-errors.md) for more help discovering your current quotas for specific regions.</span></span>
> 
> 

## <a name="service-specific-limits"></a><span data-ttu-id="07fef-132">Szolgáltatás-specifikus korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-132">Service-specific limits</span></span>
* [<span data-ttu-id="07fef-133">Active Directory</span><span class="sxs-lookup"><span data-stu-id="07fef-133">Active Directory</span></span>](#active-directory-limits)
* [<span data-ttu-id="07fef-134">API Management</span><span class="sxs-lookup"><span data-stu-id="07fef-134">API Management</span></span>](#api-management-limits)
* [<span data-ttu-id="07fef-135">APP SERVICE</span><span class="sxs-lookup"><span data-stu-id="07fef-135">App Service</span></span>](#app-service-limits)
* [<span data-ttu-id="07fef-136">Application Gateway</span><span class="sxs-lookup"><span data-stu-id="07fef-136">Application Gateway</span></span>](#application-gateway-limits)
* [<span data-ttu-id="07fef-137">Application Insights</span><span class="sxs-lookup"><span data-stu-id="07fef-137">Application Insights</span></span>](#application-insights-limits)
* [<span data-ttu-id="07fef-138">Automatizálás</span><span class="sxs-lookup"><span data-stu-id="07fef-138">Automation</span></span>](#automation-limits)
* [<span data-ttu-id="07fef-139">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="07fef-139">Azure Cosmos DB</span></span>](#azure-cosmos-db-limits)
* [<span data-ttu-id="07fef-140">Az Azure Event rács</span><span class="sxs-lookup"><span data-stu-id="07fef-140">Azure Event Grid</span></span>](#azure-event-grid-limits)
* [<span data-ttu-id="07fef-141">Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="07fef-141">Azure Redis Cache</span></span>](#azure-redis-cache-limits)
* [<span data-ttu-id="07fef-142">Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="07fef-142">Azure RemoteApp</span></span>](#azure-remoteapp-limits)
* [<span data-ttu-id="07fef-143">Biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="07fef-143">Backup</span></span>](#backup-limits)
* [<span data-ttu-id="07fef-144">Batch</span><span class="sxs-lookup"><span data-stu-id="07fef-144">Batch</span></span>](#batch-limits)
* [<span data-ttu-id="07fef-145">BizTalk szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="07fef-145">BizTalk Services</span></span>](#biztalk-services-limits)
* [<span data-ttu-id="07fef-146">TARTALOMKÉZBESÍTÉSI HÁLÓZAT (CDN)</span><span class="sxs-lookup"><span data-stu-id="07fef-146">CDN</span></span>](#cdn-limits)
* [<span data-ttu-id="07fef-147">Felhőszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="07fef-147">Cloud Services</span></span>](#cloud-services-limits)
* [<span data-ttu-id="07fef-148">Tárolópéldányok</span><span class="sxs-lookup"><span data-stu-id="07fef-148">Container Instances</span></span>](#container-instances-limits)
* [<span data-ttu-id="07fef-149">Data Factory</span><span class="sxs-lookup"><span data-stu-id="07fef-149">Data Factory</span></span>](#data-factory-limits)
* [<span data-ttu-id="07fef-150">Data Lake analitikai szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="07fef-150">Data Lake Analytics</span></span>](#data-lake-analytics-limits)
* [<span data-ttu-id="07fef-151">Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="07fef-151">Data Lake Store</span></span>](#data-lake-store-limits)
* [<span data-ttu-id="07fef-152">DNS</span><span class="sxs-lookup"><span data-stu-id="07fef-152">DNS</span></span>](#dns-limits)
* [<span data-ttu-id="07fef-153">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="07fef-153">Event Hubs</span></span>](#event-hubs-limits)
* [<span data-ttu-id="07fef-154">IoT Hub</span><span class="sxs-lookup"><span data-stu-id="07fef-154">IoT Hub</span></span>](#iot-hub-limits)
* [<span data-ttu-id="07fef-155">Key Vault</span><span class="sxs-lookup"><span data-stu-id="07fef-155">Key Vault</span></span>](#key-vault-limits)
* [<span data-ttu-id="07fef-156">Naplófájl Analytics / Operational Insights</span><span class="sxs-lookup"><span data-stu-id="07fef-156">Log Analytics / Operational Insights</span></span>](#log-analytics-limits)
* [<span data-ttu-id="07fef-157">Médiaszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="07fef-157">Media Services</span></span>](#media-services-limits)
* [<span data-ttu-id="07fef-158">Mobilmarketing</span><span class="sxs-lookup"><span data-stu-id="07fef-158">Mobile Engagement</span></span>](#mobile-engagement-limits)
* [<span data-ttu-id="07fef-159">Mobilszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="07fef-159">Mobile Services</span></span>](#mobile-services-limits)
* [<span data-ttu-id="07fef-160">Figyelés</span><span class="sxs-lookup"><span data-stu-id="07fef-160">Monitor</span></span>](#monitor-limits)
* [<span data-ttu-id="07fef-161">Többtényezős hitelesítés</span><span class="sxs-lookup"><span data-stu-id="07fef-161">Multi-Factor Authentication</span></span>](#multi-factor-authentication)
* [<span data-ttu-id="07fef-162">Hálózat</span><span class="sxs-lookup"><span data-stu-id="07fef-162">Networking</span></span>](#networking-limits)
* [<span data-ttu-id="07fef-163">Hálózati figyelőt</span><span class="sxs-lookup"><span data-stu-id="07fef-163">Network Watcher</span></span>](#network-watcher-limits)
* [<span data-ttu-id="07fef-164">Értesítési központ szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="07fef-164">Notification Hub Service</span></span>](#notification-hub-service-limits)
* [<span data-ttu-id="07fef-165">Erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="07fef-165">Resource Group</span></span>](#resource-group-limits)
* [<span data-ttu-id="07fef-166">Scheduler</span><span class="sxs-lookup"><span data-stu-id="07fef-166">Scheduler</span></span>](#scheduler-limits)
* [<span data-ttu-id="07fef-167">Search</span><span class="sxs-lookup"><span data-stu-id="07fef-167">Search</span></span>](#search-limits)
* [<span data-ttu-id="07fef-168">Szolgáltatásbusz</span><span class="sxs-lookup"><span data-stu-id="07fef-168">Service Bus</span></span>](#service-bus-limits)
* [<span data-ttu-id="07fef-169">Site Recovery</span><span class="sxs-lookup"><span data-stu-id="07fef-169">Site Recovery</span></span>](#site-recovery-limits)
* [<span data-ttu-id="07fef-170">SQL Database</span><span class="sxs-lookup"><span data-stu-id="07fef-170">SQL Database</span></span>](#sql-database-limits)
* [<span data-ttu-id="07fef-171">Tárolás</span><span class="sxs-lookup"><span data-stu-id="07fef-171">Storage</span></span>](#storage-limits)
* [<span data-ttu-id="07fef-172">StorSimple rendszer</span><span class="sxs-lookup"><span data-stu-id="07fef-172">StorSimple System</span></span>](#storsimple-system-limits)
* [<span data-ttu-id="07fef-173">Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="07fef-173">Stream Analytics</span></span>](#stream-analytics-limits)
* [<span data-ttu-id="07fef-174">Előfizetés</span><span class="sxs-lookup"><span data-stu-id="07fef-174">Subscription</span></span>](#subscription-limits)
* [<span data-ttu-id="07fef-175">Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="07fef-175">Traffic Manager</span></span>](#traffic-manager-limits)
* [<span data-ttu-id="07fef-176">Virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="07fef-176">Virtual Machines</span></span>](#virtual-machines-limits)
* [<span data-ttu-id="07fef-177">Virtual Machine Scale Sets</span><span class="sxs-lookup"><span data-stu-id="07fef-177">Virtual Machine Scale Sets</span></span>](#virtual-machine-scale-sets-limits)

### <a name="subscription-limits"></a><span data-ttu-id="07fef-178">Előfizetési korlátozásait</span><span class="sxs-lookup"><span data-stu-id="07fef-178">Subscription limits</span></span>
#### <a name="subscription-limits"></a><span data-ttu-id="07fef-179">Előfizetési korlátozásait</span><span class="sxs-lookup"><span data-stu-id="07fef-179">Subscription limits</span></span>
[!INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a><span data-ttu-id="07fef-180">Előfizetési korlátozásait - Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="07fef-180">Subscription limits - Azure Resource Manager</span></span>
<span data-ttu-id="07fef-181">a következő korlátozások hello alkalmazza, ha hello Azure Resource Manager és az Azure erőforráscsoport-sablonok használatával.</span><span class="sxs-lookup"><span data-stu-id="07fef-181">hello following limits apply when using hello Azure Resource Manager and Azure Resource Groups.</span></span> <span data-ttu-id="07fef-182">Nem módosított rendelkező hello Azure Resource Manager korlátok alább nem láthatók.</span><span class="sxs-lookup"><span data-stu-id="07fef-182">Limits that have not changed with hello Azure Resource Manager are not listed below.</span></span> <span data-ttu-id="07fef-183">Az ilyen határidők toohello előző táblázatban tájékozódhat.</span><span class="sxs-lookup"><span data-stu-id="07fef-183">Please refer toohello previous table for those limits.</span></span>

<span data-ttu-id="07fef-184">Erőforrás-kezelő kérelmekre vonatkozó korlátozások kezelésére vonatkozó információkért lásd: [sávszélesség-szabályozás erőforrás-kezelő kérelmek](resource-manager-request-limits.md).</span><span class="sxs-lookup"><span data-stu-id="07fef-184">For information about handling limits on Resource Manager requests, see [Throttling Resource Manager requests](resource-manager-request-limits.md).</span></span>

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]

### <a name="resource-group-limits"></a><span data-ttu-id="07fef-185">Erőforráscsoport korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-185">Resource Group limits</span></span>
[!INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a><span data-ttu-id="07fef-186">Virtuális gépek korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-186">Virtual Machines limits</span></span>
#### <a name="virtual-machine-limits"></a><span data-ttu-id="07fef-187">Virtuális gépek korlátai</span><span class="sxs-lookup"><span data-stu-id="07fef-187">Virtual Machine limits</span></span>
[!INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]

#### <a name="virtual-machines-limits---azure-resource-manager"></a><span data-ttu-id="07fef-188">Virtuális gépek korlátok - Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="07fef-188">Virtual Machines limits - Azure Resource Manager</span></span>
<span data-ttu-id="07fef-189">a következő korlátozások hello alkalmazza, ha hello Azure Resource Manager és az Azure erőforráscsoport-sablonok használatával.</span><span class="sxs-lookup"><span data-stu-id="07fef-189">hello following limits apply when using hello Azure Resource Manager and Azure Resource Groups.</span></span> <span data-ttu-id="07fef-190">Nem módosított rendelkező hello Azure Resource Manager korlátok alább nem láthatók.</span><span class="sxs-lookup"><span data-stu-id="07fef-190">Limits that have not changed with hello Azure Resource Manager are not listed below.</span></span> <span data-ttu-id="07fef-191">Az ilyen határidők toohello előző táblázatban tájékozódhat.</span><span class="sxs-lookup"><span data-stu-id="07fef-191">Please refer toohello previous table for those limits.</span></span>

[!INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a><span data-ttu-id="07fef-192">Virtuálisgép-méretezési készlet korlátai</span><span class="sxs-lookup"><span data-stu-id="07fef-192">Virtual Machine Scale Sets limits</span></span>
[!INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="container-instances-limits"></a><span data-ttu-id="07fef-193">Tároló-példányok korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-193">Container Instances Limits</span></span>
[!INCLUDE [container-instances-limits](../includes/container-instances-limits.md)]

### <a name="networking-limits"></a><span data-ttu-id="07fef-194">Hálózatkezelési korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-194">Networking limits</span></span>
[!INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a><span data-ttu-id="07fef-195">Hálózatkezelési korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-195">Networking limits</span></span>
[!INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="application-gateway-limits"></a><span data-ttu-id="07fef-196">Alkalmazásátjáró korlátozza.</span><span class="sxs-lookup"><span data-stu-id="07fef-196">Application Gateway limits</span></span>
[!INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="network-watcher-limits"></a><span data-ttu-id="07fef-197">Korlátozza a hálózati figyelőt</span><span class="sxs-lookup"><span data-stu-id="07fef-197">Network Watcher limits</span></span>
[!INCLUDE [network-watcher-limits](../includes/network-watcher-limits.md)]

#### <a name="traffic-manager-limits"></a><span data-ttu-id="07fef-198">A TRAFFIC Manager-korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-198">Traffic Manager limits</span></span>
[!INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="dns-limits"></a><span data-ttu-id="07fef-199">DNS-korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-199">DNS limits</span></span>
[!INCLUDE [dns-limits](../includes/dns-limits.md)]

### <a name="storage-limits"></a><span data-ttu-id="07fef-200">Tárolási korlátai</span><span class="sxs-lookup"><span data-stu-id="07fef-200">Storage limits</span></span>
<span data-ttu-id="07fef-201">A tárfiókok korlátai további részletekért lásd: [Azure Storage méretezhetőségi és teljesítménycéloknak](storage/common/storage-scalability-targets.md).</span><span class="sxs-lookup"><span data-stu-id="07fef-201">For additional details on storage account limits, see [Azure Storage Scalability and Performance Targets](storage/common/storage-scalability-targets.md).</span></span>
<!--like # storage accts --> 
#### <a name="storage-service-limits"></a><span data-ttu-id="07fef-202">Storage szolgáltatásra vonatkozó korlátozások</span><span class="sxs-lookup"><span data-stu-id="07fef-202">Storage Service limits</span></span>
[!INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

<!-- conceptual info about disk limits -- applies toounmanaged and managed -->
#### <a name="virtual-machine-disk-limits"></a><span data-ttu-id="07fef-203">Virtuális gépek lemez korlátai</span><span class="sxs-lookup"><span data-stu-id="07fef-203">Virtual machine disk limits</span></span> 
[!INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

<span data-ttu-id="07fef-204">Lásd: [virtuálisgép-méretek](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="07fef-204">See [Virtual machine sizes](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for additional details.</span></span>

#### <a name="managed-virtual-machine-disks"></a><span data-ttu-id="07fef-205">Felügyelt virtuális gépek lemezei</span><span class="sxs-lookup"><span data-stu-id="07fef-205">Managed virtual machine disks</span></span>

[!INCLUDE [azure-storage-limits-vm-disks-managed](../includes/azure-storage-limits-vm-disks-managed.md)]

#### <a name="unmanaged-virtual-machine-disks"></a><span data-ttu-id="07fef-206">Nem felügyelt virtuális gépek lemezei</span><span class="sxs-lookup"><span data-stu-id="07fef-206">Unmanaged virtual machine disks</span></span>

[!INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

#### <a name="storage-resource-provider-limits"></a><span data-ttu-id="07fef-207">Korlátozza a Storage erőforrás-szolgáltató</span><span class="sxs-lookup"><span data-stu-id="07fef-207">Storage Resource Provider limits</span></span>
[!INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]

### <a name="cloud-services-limits"></a><span data-ttu-id="07fef-208">Cloud Services korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-208">Cloud Services limits</span></span>
[!INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]

### <a name="app-service-limits"></a><span data-ttu-id="07fef-209">App Service szolgáltatásra vonatkozó korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-209">App Service limits</span></span>
<span data-ttu-id="07fef-210">hello következő korlátozza az App Service Web Apps, a Mobile Apps, az API-alkalmazások és a Logic Apps korlátok tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="07fef-210">hello following App Service limits include limits for Web Apps, Mobile Apps, API Apps, and Logic Apps.</span></span>

[!INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a><span data-ttu-id="07fef-211">A Feladatütemező korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-211">Scheduler limits</span></span>
[!INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a><span data-ttu-id="07fef-212">Kötegelt korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-212">Batch limits</span></span>
[!INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

### <a name="biztalk-services-limits"></a><span data-ttu-id="07fef-213">BizTalk szolgáltatások korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-213">BizTalk Services limits</span></span>
<span data-ttu-id="07fef-214">hello következő táblázat hello korlátok Azure Biztalk szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="07fef-214">hello following table shows hello limits for Azure Biztalk Services.</span></span>

[!INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]

### <a name="azure-cosmos-db-limits"></a><span data-ttu-id="07fef-215">Az Azure Cosmos DB korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-215">Azure Cosmos DB limits</span></span>
<span data-ttu-id="07fef-216">Azure Cosmos-adatbázis egy olyan globális méretű adatbázis, mely átviteli sebesség és tárterület lehet méretezett toohandle függetlenül az alkalmazás által igényelt.</span><span class="sxs-lookup"><span data-stu-id="07fef-216">Azure Cosmos DB is a global scale database in which throughput and storage can be scaled toohandle whatever your application requires.</span></span> <span data-ttu-id="07fef-217">Ha Azure Cosmos DB biztosít hello méretezésének kérdése van, kérjük, küldjön e-mailek tooaskcosmosdb@microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="07fef-217">If you have any questions about hello scale Azure Cosmos DB provides, please send email tooaskcosmosdb@microsoft.com.</span></span>

### <a name="mobile-engagement-limits"></a><span data-ttu-id="07fef-218">A Mobile Engagement korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-218">Mobile Engagement limits</span></span>
[!INCLUDE [azure-mobile-engagement-limits](../includes/azure-mobile-engagement-limits.md)]

### <a name="search-limits"></a><span data-ttu-id="07fef-219">Keresési korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-219">Search limits</span></span>
<span data-ttu-id="07fef-220">Tarifacsomagok hello kapacitás és a keresési szolgáltatás határain határozza meg.</span><span class="sxs-lookup"><span data-stu-id="07fef-220">Pricing tiers determine hello capacity and limits of your search service.</span></span> <span data-ttu-id="07fef-221">Szolgáltatásszintek:</span><span class="sxs-lookup"><span data-stu-id="07fef-221">Tiers include:</span></span>

* <span data-ttu-id="07fef-222">*Szabad* értékelési és kis fejlesztési projektek szánt más Azure-előfizetők megosztott, több-bérlős szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="07fef-222">*Free* multi-tenant service, shared with other Azure subscribers, intended for evaluation and small development projects.</span></span>
* <span data-ttu-id="07fef-223">*Alapszintű* biztosít a számítási erőforrások dedikált kisebb léptékű termelési számítási feladatokhoz toothree replikák a munkaterhelések magas rendelkezésre állású lekérdezés mentése.</span><span class="sxs-lookup"><span data-stu-id="07fef-223">*Basic* provides dedicated computing resources for production workloads at a smaller scale, with up toothree replicas for highly available query workloads.</span></span>
* <span data-ttu-id="07fef-224">*Standard (S1, S2, S3, S3 nagy sűrűségű)* értéke nagyobb a termelési számítási feladatokhoz.</span><span class="sxs-lookup"><span data-stu-id="07fef-224">*Standard (S1, S2, S3, S3 High Density)* is for larger production workloads.</span></span> <span data-ttu-id="07fef-225">Több szinten vannak, hogy Ön egy erőforrás-konfigurációhoz, amely a legjobban illik a munkaterhelés profil hello standard csomagra.</span><span class="sxs-lookup"><span data-stu-id="07fef-225">Multiple levels exist within hello standard tier so that you can choose a resource configuration that best matches your workload profile.</span></span>

<span data-ttu-id="07fef-226">**Előfizetésenként korlátok**</span><span class="sxs-lookup"><span data-stu-id="07fef-226">**Limits per subscription**</span></span>

[!INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

<span data-ttu-id="07fef-227">**Érhető el keresési szolgáltatásonként korlátok**</span><span class="sxs-lookup"><span data-stu-id="07fef-227">**Limits per search service**</span></span>

[!INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

<span data-ttu-id="07fef-228">toolearn részletesebb felügyeletét, például a dokumentum mérete, a lekérdezések száma másodpercenként, kulcsok, kérések és válaszok, használati korlátait kapcsolatos további információkért lásd: [szolgáltatási korlátait, az Azure Search](search/search-limits-quotas-capacity.md).</span><span class="sxs-lookup"><span data-stu-id="07fef-228">toolearn more about limits on a more granular level, such as document size, queries per second, keys, requests, and responses, see [Service limits in Azure Search](search/search-limits-quotas-capacity.md).</span></span>

### <a name="media-services-limits"></a><span data-ttu-id="07fef-229">A Media Services korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-229">Media Services limits</span></span>
[!INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="cdn-limits"></a><span data-ttu-id="07fef-230">CDN-korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-230">CDN limits</span></span>
[!INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a><span data-ttu-id="07fef-231">A Mobile Services korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-231">Mobile Services limits</span></span>
[!INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitor-limits"></a><span data-ttu-id="07fef-232">A figyelő korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-232">Monitor limits</span></span>
[!INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hub-service-limits"></a><span data-ttu-id="07fef-233">Értesítési központ szolgáltatásra vonatkozó korlátozások</span><span class="sxs-lookup"><span data-stu-id="07fef-233">Notification Hub Service limits</span></span>
[!INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a><span data-ttu-id="07fef-234">Event Hubs korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-234">Event Hubs limits</span></span>
[!INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a><span data-ttu-id="07fef-235">A Service Bus korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-235">Service Bus limits</span></span>
[!INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a><span data-ttu-id="07fef-236">Az IoT-központ korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-236">IoT Hub limits</span></span>
[!INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="data-factory-limits"></a><span data-ttu-id="07fef-237">Adat-előállító korlátozza.</span><span class="sxs-lookup"><span data-stu-id="07fef-237">Data Factory limits</span></span>
[!INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### <a name="data-lake-analytics-limits"></a><span data-ttu-id="07fef-238">A Data Lake Analytics korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-238">Data Lake Analytics limits</span></span>
[!INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### <a name="data-lake-store-limits"></a><span data-ttu-id="07fef-239">Data Lake Store korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-239">Data Lake Store limits</span></span>
[!INCLUDE [azure-data-lake-store-limits](../includes/azure-data-lake-store-limits.md)]

### <a name="stream-analytics-limits"></a><span data-ttu-id="07fef-240">A Stream Analytics korlátozza.</span><span class="sxs-lookup"><span data-stu-id="07fef-240">Stream Analytics limits</span></span>
[!INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### <a name="active-directory-limits"></a><span data-ttu-id="07fef-241">Active Directory-korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-241">Active Directory limits</span></span>
[!INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]

### <a name="azure-event-grid-limits"></a><span data-ttu-id="07fef-242">Az Azure Event rács korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-242">Azure Event Grid limits</span></span>
[!INCLUDE [event-grid-limits](../includes/event-grid-limits.md)]

### <a name="azure-remoteapp-limits"></a><span data-ttu-id="07fef-243">Az Azure RemoteApp korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-243">Azure RemoteApp limits</span></span>
[!INCLUDE [azure-remoteapp-limits](../includes/azure-remoteapp-limits.md)]

### <a name="storsimple-system-limits"></a><span data-ttu-id="07fef-244">StorSimple rendszer korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-244">StorSimple System limits</span></span>
[!INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]

### <a name="log-analytics-limits"></a><span data-ttu-id="07fef-245">A Naplóelemzési korlátozza.</span><span class="sxs-lookup"><span data-stu-id="07fef-245">Log Analytics limits</span></span>
[!INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### <a name="backup-limits"></a><span data-ttu-id="07fef-246">Biztonsági mentési korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-246">Backup limits</span></span>
[!INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="site-recovery-limits"></a><span data-ttu-id="07fef-247">A Site Recovery korlátai</span><span class="sxs-lookup"><span data-stu-id="07fef-247">Site Recovery limits</span></span>
[!INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a><span data-ttu-id="07fef-248">Application Insights korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-248">Application Insights limits</span></span>
[!INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a><span data-ttu-id="07fef-249">Korlátozza az API Management</span><span class="sxs-lookup"><span data-stu-id="07fef-249">API Management limits</span></span>
[!INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-redis-cache-limits"></a><span data-ttu-id="07fef-250">Azure Redis Cache-korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-250">Azure Redis Cache limits</span></span>
[!INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a><span data-ttu-id="07fef-251">Key Vault korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-251">Key Vault limits</span></span>
[!INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication"></a><span data-ttu-id="07fef-252">Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="07fef-252">Multi-Factor Authentication</span></span>
[!INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a><span data-ttu-id="07fef-253">Automatizálási korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-253">Automation limits</span></span>
[!INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="sql-database-limits"></a><span data-ttu-id="07fef-254">SQL-adatbázis korlátok</span><span class="sxs-lookup"><span data-stu-id="07fef-254">SQL Database limits</span></span>
<span data-ttu-id="07fef-255">SQL adatbázis-korlátok, lásd: [SQL adatbázis erőforrás korlátok](sql-database/sql-database-resource-limits.md).</span><span class="sxs-lookup"><span data-stu-id="07fef-255">For SQL Database limits, see [SQL Database Resource Limits](sql-database/sql-database-resource-limits.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="07fef-256">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="07fef-256">See also</span></span>
[<span data-ttu-id="07fef-257">Azure korlátozását és növekszik ismertetése</span><span class="sxs-lookup"><span data-stu-id="07fef-257">Understanding Azure Limits and Increases</span></span>](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[<span data-ttu-id="07fef-258">Virtuális gépek és Felhőszolgáltatások mérete az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="07fef-258">Virtual Machine and Cloud Service Sizes for Azure</span></span>](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="07fef-259">A Felhőszolgáltatások mérete</span><span class="sxs-lookup"><span data-stu-id="07fef-259">Sizes for Cloud Services</span></span>](cloud-services/cloud-services-sizes-specs.md)

