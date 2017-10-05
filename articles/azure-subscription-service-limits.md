---
title: "Azure-előfizetés korlátozásai és a kvóták |} Microsoft Docs"
description: "A közös Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések listáját tartalmazza. Ide tartoznak a korlátjának növelésére korlátok együtt maximális értékeket."
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
ms.openlocfilehash: a76acd67e9ba7822f2837b3c08e2ede389047f11
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a><span data-ttu-id="f2482-104">Az Azure-előfizetésekre és -szolgáltatásokra vonatkozó korlátozások, kvóták és megkötések</span><span class="sxs-lookup"><span data-stu-id="f2482-104">Azure subscription and service limits, quotas, and constraints</span></span>
<span data-ttu-id="f2482-105">Ez a dokumentum mutatja a leggyakrabban használt Microsoft Azure korlátok, kvóták néven is ismert.</span><span class="sxs-lookup"><span data-stu-id="f2482-105">This document lists some of the most common Microsoft Azure limits, which are also sometimes called quotas.</span></span> <span data-ttu-id="f2482-106">Ez a dokumentum jelenleg nem fedi le az összes Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="f2482-106">This document doesn't currently cover all Azure services.</span></span> <span data-ttu-id="f2482-107">Adott idő alatt a lista lesz kibontható vagy frissíteni, amelyek több, a platform.</span><span class="sxs-lookup"><span data-stu-id="f2482-107">Over time, the list will be expanded and updated to cover more of the platform.</span></span>

<span data-ttu-id="f2482-108">Látogasson el a [Azure díjszabása áttekintése](https://azure.microsoft.com/pricing/) tudhat meg többet az Azure-beli árakról.</span><span class="sxs-lookup"><span data-stu-id="f2482-108">Please visit [Azure Pricing Overview](https://azure.microsoft.com/pricing/) to learn more about Azure pricing.</span></span> <span data-ttu-id="f2482-109">Van, a költségek használatával megbecsülhető a [Díjkalkulátor](https://azure.microsoft.com/pricing/calculator/) vagy egy szolgáltatás árképzési részleteit megjelenítő oldalon felkeresésével (például [Windows virtuális gépek](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)).</span><span class="sxs-lookup"><span data-stu-id="f2482-109">There, you can estimate your costs using the [Pricing Calculator](https://azure.microsoft.com/pricing/calculator/) or by visiting the pricing details page for a service (for example, [Windows VMs](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)).</span></span> <span data-ttu-id="f2482-110">Tippek a költségek kezeléséhez, tekintse meg a [Azure számlázás és költség felügyeleti váratlan költségek megakadályozása](billing/billing-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="f2482-110">For tips to help manage your costs, see [Prevent unexpected costs with Azure billing and cost management](billing/billing-getting-started.md).</span></span>

> [!NOTE]
> <span data-ttu-id="f2482-111">Ha azt szeretné, a korlát vagy a fenti kvóta emelése a **alapértelmezett korlát**, [nyissa meg az online támogatás ügyfélkérés díjmentesen](azure-supportability/resource-manager-core-quotas-request.md).</span><span class="sxs-lookup"><span data-stu-id="f2482-111">If you want to raise the limit or quota above the **Default Limit**, [open an online customer support request at no charge](azure-supportability/resource-manager-core-quotas-request.md).</span></span> <span data-ttu-id="f2482-112">A korlátai nem léptethető elő fent a **maximális** az alábbi táblázatban szereplő érték.</span><span class="sxs-lookup"><span data-stu-id="f2482-112">The limits can't be raised above the **Maximum Limit** value shown in the following tables.</span></span> <span data-ttu-id="f2482-113">Ha nincs **maximális** oszlop, akkor az erőforrás nem állítható korlátokkal rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="f2482-113">If there is no **Maximum Limit** column, then the resource doesn't have adjustable limits.</span></span> 
> 
> <span data-ttu-id="f2482-114">Ingyenes próba-előfizetések nem jogosultak a korlátot, vagy kvóta növeli.</span><span class="sxs-lookup"><span data-stu-id="f2482-114">Free Trial subscriptions are not eligible for limit or quota increases.</span></span> <span data-ttu-id="f2482-115">Ha egy ingyenes próbaverziót, frissíthet egy [használatalapú fizetés](https://azure.microsoft.com/offers/ms-azr-0003p/) előfizetés.</span><span class="sxs-lookup"><span data-stu-id="f2482-115">If you have a Free Trial, you can upgrade to a [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) subscription.</span></span> <span data-ttu-id="f2482-116">További információkért lásd: [frissítése az Azure ingyenes próbaverzió használatalapú fizetésre](billing/billing-upgrade-azure-subscription.md).</span><span class="sxs-lookup"><span data-stu-id="f2482-116">For more information, see [Upgrade Azure Free Trial to Pay-As-You-Go](billing/billing-upgrade-azure-subscription.md).</span></span>
> 

## <a name="limits-and-the-azure-resource-manager"></a><span data-ttu-id="f2482-117">Korlátozásai és az Azure erőforrás-kezelő</span><span class="sxs-lookup"><span data-stu-id="f2482-117">Limits and the Azure Resource Manager</span></span>
<span data-ttu-id="f2482-118">Már lehetséges a több Azure-erőforrások az Azure egyetlen erőforráscsoporthoz kombinálni.</span><span class="sxs-lookup"><span data-stu-id="f2482-118">It is now possible to combine multiple Azure resources in to a single Azure Resource Group.</span></span> <span data-ttu-id="f2482-119">Erőforráscsoportok használata esetén, amelyek egyszer volt a globális korlátok regionális szinten az Azure Resource Manager felügyelhető legyen.</span><span class="sxs-lookup"><span data-stu-id="f2482-119">When using Resource Groups, limits that once were global become managed at a regional level with the Azure Resource Manager.</span></span> <span data-ttu-id="f2482-120">Azure erőforráscsoport-sablonok kapcsolatos további információkért lásd: [Azure Resource Manager áttekintése](azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f2482-120">For more information about Azure Resource Groups, see [Azure Resource Manager overview](azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="f2482-121">Az alábbi korlátok új tábla összes különbséget korlátok megfelelően az Azure Resource Manager használatakor bővült.</span><span class="sxs-lookup"><span data-stu-id="f2482-121">In the limits below, a new table has been added to reflect any differences in limits when using the Azure Resource Manager.</span></span> <span data-ttu-id="f2482-122">Például van egy **előfizetési korlátozásait** tábla és egy **előfizetési korlátozásait - Azure Resource Manager** tábla.</span><span class="sxs-lookup"><span data-stu-id="f2482-122">For example, there is a **Subscription Limits** table and a **Subscription Limits - Azure Resource Manager** table.</span></span> <span data-ttu-id="f2482-123">Ha a megadott korlát érvényes, a mindkét forgatókönyvet, csak látható az első tábla.</span><span class="sxs-lookup"><span data-stu-id="f2482-123">When a limit applies to both scenarios, it is only shown in the first table.</span></span> <span data-ttu-id="f2482-124">Hiányában korlátok legyenek globális minden régióban.</span><span class="sxs-lookup"><span data-stu-id="f2482-124">Unless otherwise indicated, limits are global across all regions.</span></span>

> [!NOTE]
> <span data-ttu-id="f2482-125">Fontos, hogy Azure erőforráscsoport-sablonok az erőforrásokra vonatkozó kvótákat /-régióban elérhető-e az előfizetés, és nem előfizetésenként, mert a szolgáltatás felügyeleti kvóták emelje ki.</span><span class="sxs-lookup"><span data-stu-id="f2482-125">It is important to emphasize that quotas for resources in Azure Resource Groups are per-region accessible by your subscription, and are not per-subscription, as the service management quotas are.</span></span> <span data-ttu-id="f2482-126">Most használja core kvóták példaként.</span><span class="sxs-lookup"><span data-stu-id="f2482-126">Let's use core quotas as an example.</span></span> <span data-ttu-id="f2482-127">Ha magok támogatása a kvóta növelését van szüksége, döntse el, hogy hány magok régiók használja, és végezze el az összegek és régiók, amelyet egy adott kérelem az Azure-erőforráscsoport core kvóták szüksége.</span><span class="sxs-lookup"><span data-stu-id="f2482-127">If you need to request a quota increase with support for cores, you need to decide how many cores you want to use in which regions, and then make a specific request for Azure Resource Group core quotas for the amounts and regions that you want.</span></span> <span data-ttu-id="f2482-128">Ezért, ha szeretné Nyugat-Európában 30 mag használatával futtassa az alkalmazást; Nyugat-Európában 30 magok kifejezetten igényeljen.</span><span class="sxs-lookup"><span data-stu-id="f2482-128">Therefore, if you need to use 30 cores in West Europe to run your application there; you should specifically request 30 cores in West Europe.</span></span> <span data-ttu-id="f2482-129">Azonban Ön nem rendelkezik a core kvóta növelése más régióban – csak Nyugat-Európában fog rendelkezni a 30-core kvótát.</span><span class="sxs-lookup"><span data-stu-id="f2482-129">But you will not have a core quota increase in any other region -- only West Europe will have the 30-core quota.</span></span>
> <!-- -->
> <span data-ttu-id="f2482-130">Ennek köszönhetően Ön érdemes figyelembe venni annak eldöntése, az Azure-erőforráscsoport kvóták kell lennie a munkaterheléshez bármely egy régióban, és minden régióban, amelybe a központi telepítés tervezi, hogy mennyi kérelem.</span><span class="sxs-lookup"><span data-stu-id="f2482-130">As a result, you may find it useful to consider deciding what your Azure Resource Group quotas need to be for your workload in any one region, and request that amount in each region into which you are considering deployment.</span></span> <span data-ttu-id="f2482-131">Lásd: [telepítési problémák elhárítása](resource-manager-common-deployment-errors.md) további segítséget itt találhat az aktuális kvóták adott régióban felderítéséhez.</span><span class="sxs-lookup"><span data-stu-id="f2482-131">See [troubleshooting deployment issues](resource-manager-common-deployment-errors.md) for more help discovering your current quotas for specific regions.</span></span>
> 
> 

## <a name="service-specific-limits"></a><span data-ttu-id="f2482-132">Szolgáltatás-specifikus korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-132">Service-specific limits</span></span>
* [<span data-ttu-id="f2482-133">Active Directory</span><span class="sxs-lookup"><span data-stu-id="f2482-133">Active Directory</span></span>](#active-directory-limits)
* [<span data-ttu-id="f2482-134">API Management</span><span class="sxs-lookup"><span data-stu-id="f2482-134">API Management</span></span>](#api-management-limits)
* [<span data-ttu-id="f2482-135">APP SERVICE</span><span class="sxs-lookup"><span data-stu-id="f2482-135">App Service</span></span>](#app-service-limits)
* [<span data-ttu-id="f2482-136">Application Gateway</span><span class="sxs-lookup"><span data-stu-id="f2482-136">Application Gateway</span></span>](#application-gateway-limits)
* [<span data-ttu-id="f2482-137">Application Insights</span><span class="sxs-lookup"><span data-stu-id="f2482-137">Application Insights</span></span>](#application-insights-limits)
* [<span data-ttu-id="f2482-138">Automatizálás</span><span class="sxs-lookup"><span data-stu-id="f2482-138">Automation</span></span>](#automation-limits)
* [<span data-ttu-id="f2482-139">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="f2482-139">Azure Cosmos DB</span></span>](#azure-cosmos-db-limits)
* [<span data-ttu-id="f2482-140">Az Azure Event rács</span><span class="sxs-lookup"><span data-stu-id="f2482-140">Azure Event Grid</span></span>](#azure-event-grid-limits)
* [<span data-ttu-id="f2482-141">Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="f2482-141">Azure Redis Cache</span></span>](#azure-redis-cache-limits)
* [<span data-ttu-id="f2482-142">Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="f2482-142">Azure RemoteApp</span></span>](#azure-remoteapp-limits)
* [<span data-ttu-id="f2482-143">Biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="f2482-143">Backup</span></span>](#backup-limits)
* [<span data-ttu-id="f2482-144">Batch</span><span class="sxs-lookup"><span data-stu-id="f2482-144">Batch</span></span>](#batch-limits)
* [<span data-ttu-id="f2482-145">BizTalk szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="f2482-145">BizTalk Services</span></span>](#biztalk-services-limits)
* [<span data-ttu-id="f2482-146">TARTALOMKÉZBESÍTÉSI HÁLÓZAT (CDN)</span><span class="sxs-lookup"><span data-stu-id="f2482-146">CDN</span></span>](#cdn-limits)
* [<span data-ttu-id="f2482-147">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="f2482-147">Cloud Services</span></span>](#cloud-services-limits)
* [<span data-ttu-id="f2482-148">Tárolópéldányok</span><span class="sxs-lookup"><span data-stu-id="f2482-148">Container Instances</span></span>](#container-instances-limits)
* [<span data-ttu-id="f2482-149">Data Factory</span><span class="sxs-lookup"><span data-stu-id="f2482-149">Data Factory</span></span>](#data-factory-limits)
* [<span data-ttu-id="f2482-150">Data Lake analitikai szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="f2482-150">Data Lake Analytics</span></span>](#data-lake-analytics-limits)
* [<span data-ttu-id="f2482-151">Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="f2482-151">Data Lake Store</span></span>](#data-lake-store-limits)
* [<span data-ttu-id="f2482-152">DNS</span><span class="sxs-lookup"><span data-stu-id="f2482-152">DNS</span></span>](#dns-limits)
* [<span data-ttu-id="f2482-153">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="f2482-153">Event Hubs</span></span>](#event-hubs-limits)
* [<span data-ttu-id="f2482-154">IoT Hub</span><span class="sxs-lookup"><span data-stu-id="f2482-154">IoT Hub</span></span>](#iot-hub-limits)
* [<span data-ttu-id="f2482-155">Key Vault</span><span class="sxs-lookup"><span data-stu-id="f2482-155">Key Vault</span></span>](#key-vault-limits)
* [<span data-ttu-id="f2482-156">Naplófájl Analytics / Operational Insights</span><span class="sxs-lookup"><span data-stu-id="f2482-156">Log Analytics / Operational Insights</span></span>](#log-analytics-limits)
* [<span data-ttu-id="f2482-157">Médiaszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="f2482-157">Media Services</span></span>](#media-services-limits)
* [<span data-ttu-id="f2482-158">Mobilmarketing</span><span class="sxs-lookup"><span data-stu-id="f2482-158">Mobile Engagement</span></span>](#mobile-engagement-limits)
* [<span data-ttu-id="f2482-159">Mobilszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="f2482-159">Mobile Services</span></span>](#mobile-services-limits)
* [<span data-ttu-id="f2482-160">Figyelés</span><span class="sxs-lookup"><span data-stu-id="f2482-160">Monitor</span></span>](#monitor-limits)
* [<span data-ttu-id="f2482-161">Többtényezős hitelesítés</span><span class="sxs-lookup"><span data-stu-id="f2482-161">Multi-Factor Authentication</span></span>](#multi-factor-authentication)
* [<span data-ttu-id="f2482-162">Hálózat</span><span class="sxs-lookup"><span data-stu-id="f2482-162">Networking</span></span>](#networking-limits)
* [<span data-ttu-id="f2482-163">Hálózati figyelőt</span><span class="sxs-lookup"><span data-stu-id="f2482-163">Network Watcher</span></span>](#network-watcher-limits)
* [<span data-ttu-id="f2482-164">Értesítési központ szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="f2482-164">Notification Hub Service</span></span>](#notification-hub-service-limits)
* [<span data-ttu-id="f2482-165">Erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="f2482-165">Resource Group</span></span>](#resource-group-limits)
* [<span data-ttu-id="f2482-166">Scheduler</span><span class="sxs-lookup"><span data-stu-id="f2482-166">Scheduler</span></span>](#scheduler-limits)
* [<span data-ttu-id="f2482-167">Keresés</span><span class="sxs-lookup"><span data-stu-id="f2482-167">Search</span></span>](#search-limits)
* [<span data-ttu-id="f2482-168">Szolgáltatásbusz</span><span class="sxs-lookup"><span data-stu-id="f2482-168">Service Bus</span></span>](#service-bus-limits)
* [<span data-ttu-id="f2482-169">Site Recovery</span><span class="sxs-lookup"><span data-stu-id="f2482-169">Site Recovery</span></span>](#site-recovery-limits)
* [<span data-ttu-id="f2482-170">SQL Database</span><span class="sxs-lookup"><span data-stu-id="f2482-170">SQL Database</span></span>](#sql-database-limits)
* [<span data-ttu-id="f2482-171">Storage</span><span class="sxs-lookup"><span data-stu-id="f2482-171">Storage</span></span>](#storage-limits)
* [<span data-ttu-id="f2482-172">StorSimple rendszer</span><span class="sxs-lookup"><span data-stu-id="f2482-172">StorSimple System</span></span>](#storsimple-system-limits)
* [<span data-ttu-id="f2482-173">Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="f2482-173">Stream Analytics</span></span>](#stream-analytics-limits)
* [<span data-ttu-id="f2482-174">Előfizetés</span><span class="sxs-lookup"><span data-stu-id="f2482-174">Subscription</span></span>](#subscription-limits)
* [<span data-ttu-id="f2482-175">Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="f2482-175">Traffic Manager</span></span>](#traffic-manager-limits)
* [<span data-ttu-id="f2482-176">Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="f2482-176">Virtual Machines</span></span>](#virtual-machines-limits)
* [<span data-ttu-id="f2482-177">Virtual Machine Scale Sets</span><span class="sxs-lookup"><span data-stu-id="f2482-177">Virtual Machine Scale Sets</span></span>](#virtual-machine-scale-sets-limits)

### <a name="subscription-limits"></a><span data-ttu-id="f2482-178">Előfizetési korlátozásait</span><span class="sxs-lookup"><span data-stu-id="f2482-178">Subscription limits</span></span>
#### <a name="subscription-limits"></a><span data-ttu-id="f2482-179">Előfizetési korlátozásait</span><span class="sxs-lookup"><span data-stu-id="f2482-179">Subscription limits</span></span>
[!INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a><span data-ttu-id="f2482-180">Előfizetési korlátozásait - Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="f2482-180">Subscription limits - Azure Resource Manager</span></span>
<span data-ttu-id="f2482-181">Az alábbi korlátokat alkalmazza, ha az Azure Resource Manager és az Azure erőforráscsoport-sablonok használatával.</span><span class="sxs-lookup"><span data-stu-id="f2482-181">The following limits apply when using the Azure Resource Manager and Azure Resource Groups.</span></span> <span data-ttu-id="f2482-182">Korlátok, nem módosított rendelkező az Azure erőforrás-kezelő nem az alábbiak.</span><span class="sxs-lookup"><span data-stu-id="f2482-182">Limits that have not changed with the Azure Resource Manager are not listed below.</span></span> <span data-ttu-id="f2482-183">Tekintse meg az előző táblázatban ezeket a határértékeket.</span><span class="sxs-lookup"><span data-stu-id="f2482-183">Please refer to the previous table for those limits.</span></span>

<span data-ttu-id="f2482-184">Erőforrás-kezelő kérelmekre vonatkozó korlátozások kezelésére vonatkozó információkért lásd: [sávszélesség-szabályozás erőforrás-kezelő kérelmek](resource-manager-request-limits.md).</span><span class="sxs-lookup"><span data-stu-id="f2482-184">For information about handling limits on Resource Manager requests, see [Throttling Resource Manager requests](resource-manager-request-limits.md).</span></span>

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]

### <a name="resource-group-limits"></a><span data-ttu-id="f2482-185">Erőforráscsoport korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-185">Resource Group limits</span></span>
[!INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a><span data-ttu-id="f2482-186">Virtuális gépek korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-186">Virtual Machines limits</span></span>
#### <a name="virtual-machine-limits"></a><span data-ttu-id="f2482-187">Virtuális gépek korlátai</span><span class="sxs-lookup"><span data-stu-id="f2482-187">Virtual Machine limits</span></span>
[!INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]

#### <a name="virtual-machines-limits---azure-resource-manager"></a><span data-ttu-id="f2482-188">Virtuális gépek korlátok - Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="f2482-188">Virtual Machines limits - Azure Resource Manager</span></span>
<span data-ttu-id="f2482-189">Az alábbi korlátokat alkalmazza, ha az Azure Resource Manager és az Azure erőforráscsoport-sablonok használatával.</span><span class="sxs-lookup"><span data-stu-id="f2482-189">The following limits apply when using the Azure Resource Manager and Azure Resource Groups.</span></span> <span data-ttu-id="f2482-190">Korlátok, nem módosított rendelkező az Azure erőforrás-kezelő nem az alábbiak.</span><span class="sxs-lookup"><span data-stu-id="f2482-190">Limits that have not changed with the Azure Resource Manager are not listed below.</span></span> <span data-ttu-id="f2482-191">Tekintse meg az előző táblázatban ezeket a határértékeket.</span><span class="sxs-lookup"><span data-stu-id="f2482-191">Please refer to the previous table for those limits.</span></span>

[!INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a><span data-ttu-id="f2482-192">Virtuálisgép-méretezési készlet korlátai</span><span class="sxs-lookup"><span data-stu-id="f2482-192">Virtual Machine Scale Sets limits</span></span>
[!INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="container-instances-limits"></a><span data-ttu-id="f2482-193">Tároló-példányok korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-193">Container Instances Limits</span></span>
[!INCLUDE [container-instances-limits](../includes/container-instances-limits.md)]

### <a name="networking-limits"></a><span data-ttu-id="f2482-194">Hálózatkezelési korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-194">Networking limits</span></span>
[!INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a><span data-ttu-id="f2482-195">Hálózatkezelési korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-195">Networking limits</span></span>
[!INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="application-gateway-limits"></a><span data-ttu-id="f2482-196">Alkalmazásátjáró korlátozza.</span><span class="sxs-lookup"><span data-stu-id="f2482-196">Application Gateway limits</span></span>
[!INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="network-watcher-limits"></a><span data-ttu-id="f2482-197">Korlátozza a hálózati figyelőt</span><span class="sxs-lookup"><span data-stu-id="f2482-197">Network Watcher limits</span></span>
[!INCLUDE [network-watcher-limits](../includes/network-watcher-limits.md)]

#### <a name="traffic-manager-limits"></a><span data-ttu-id="f2482-198">A TRAFFIC Manager-korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-198">Traffic Manager limits</span></span>
[!INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="dns-limits"></a><span data-ttu-id="f2482-199">DNS-korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-199">DNS limits</span></span>
[!INCLUDE [dns-limits](../includes/dns-limits.md)]

### <a name="storage-limits"></a><span data-ttu-id="f2482-200">Tárolási korlátai</span><span class="sxs-lookup"><span data-stu-id="f2482-200">Storage limits</span></span>
<span data-ttu-id="f2482-201">A tárfiókok korlátai további részletekért lásd: [Azure Storage méretezhetőségi és teljesítménycéloknak](storage/common/storage-scalability-targets.md).</span><span class="sxs-lookup"><span data-stu-id="f2482-201">For additional details on storage account limits, see [Azure Storage Scalability and Performance Targets](storage/common/storage-scalability-targets.md).</span></span>
<!--like # storage accts --> 
#### <a name="storage-service-limits"></a><span data-ttu-id="f2482-202">Storage szolgáltatásra vonatkozó korlátozások</span><span class="sxs-lookup"><span data-stu-id="f2482-202">Storage Service limits</span></span>
[!INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

<!-- conceptual info about disk limits -- applies to unmanaged and managed -->
#### <a name="virtual-machine-disk-limits"></a><span data-ttu-id="f2482-203">Virtuális gépek lemez korlátai</span><span class="sxs-lookup"><span data-stu-id="f2482-203">Virtual machine disk limits</span></span> 
[!INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

<span data-ttu-id="f2482-204">Lásd: [virtuálisgép-méretek](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="f2482-204">See [Virtual machine sizes](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for additional details.</span></span>

#### <a name="managed-virtual-machine-disks"></a><span data-ttu-id="f2482-205">Felügyelt virtuális gépek lemezei</span><span class="sxs-lookup"><span data-stu-id="f2482-205">Managed virtual machine disks</span></span>

[!INCLUDE [azure-storage-limits-vm-disks-managed](../includes/azure-storage-limits-vm-disks-managed.md)]

#### <a name="unmanaged-virtual-machine-disks"></a><span data-ttu-id="f2482-206">Nem felügyelt virtuális gépek lemezei</span><span class="sxs-lookup"><span data-stu-id="f2482-206">Unmanaged virtual machine disks</span></span>

[!INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

#### <a name="storage-resource-provider-limits"></a><span data-ttu-id="f2482-207">Korlátozza a Storage erőforrás-szolgáltató</span><span class="sxs-lookup"><span data-stu-id="f2482-207">Storage Resource Provider limits</span></span>
[!INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]

### <a name="cloud-services-limits"></a><span data-ttu-id="f2482-208">Cloud Services korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-208">Cloud Services limits</span></span>
[!INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]

### <a name="app-service-limits"></a><span data-ttu-id="f2482-209">App Service szolgáltatásra vonatkozó korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-209">App Service limits</span></span>
<span data-ttu-id="f2482-210">Az alábbi korlátokat App Service Web Apps, a Mobile Apps, az API-alkalmazások és a Logic Apps korlátok tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f2482-210">The following App Service limits include limits for Web Apps, Mobile Apps, API Apps, and Logic Apps.</span></span>

[!INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a><span data-ttu-id="f2482-211">A Feladatütemező korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-211">Scheduler limits</span></span>
[!INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a><span data-ttu-id="f2482-212">Kötegelt korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-212">Batch limits</span></span>
[!INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

### <a name="biztalk-services-limits"></a><span data-ttu-id="f2482-213">BizTalk szolgáltatások korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-213">BizTalk Services limits</span></span>
<span data-ttu-id="f2482-214">A következő táblázat a korlátok Azure Biztalk szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="f2482-214">The following table shows the limits for Azure Biztalk Services.</span></span>

[!INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]

### <a name="azure-cosmos-db-limits"></a><span data-ttu-id="f2482-215">Az Azure Cosmos DB korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-215">Azure Cosmos DB limits</span></span>
<span data-ttu-id="f2482-216">Azure Cosmos-adatbázis egy globális méretű adatbázist, amelyben átviteli sebesség és tárterület is méretezhető kezelni, függetlenül az alkalmazás által igényelt.</span><span class="sxs-lookup"><span data-stu-id="f2482-216">Azure Cosmos DB is a global scale database in which throughput and storage can be scaled to handle whatever your application requires.</span></span> <span data-ttu-id="f2482-217">Ha az Azure Cosmos DB biztosít méretezésének kérdése van, kérjük, küldjön e-mailek askcosmosdb@microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="f2482-217">If you have any questions about the scale Azure Cosmos DB provides, please send email to askcosmosdb@microsoft.com.</span></span>

### <a name="mobile-engagement-limits"></a><span data-ttu-id="f2482-218">A Mobile Engagement korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-218">Mobile Engagement limits</span></span>
[!INCLUDE [azure-mobile-engagement-limits](../includes/azure-mobile-engagement-limits.md)]

### <a name="search-limits"></a><span data-ttu-id="f2482-219">Keresési korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-219">Search limits</span></span>
<span data-ttu-id="f2482-220">Tarifacsomagok határozza meg, a kapacitás és a keresőszolgáltatása határain.</span><span class="sxs-lookup"><span data-stu-id="f2482-220">Pricing tiers determine the capacity and limits of your search service.</span></span> <span data-ttu-id="f2482-221">Szolgáltatásszintek:</span><span class="sxs-lookup"><span data-stu-id="f2482-221">Tiers include:</span></span>

* <span data-ttu-id="f2482-222">*Szabad* értékelési és kis fejlesztési projektek szánt más Azure-előfizetők megosztott, több-bérlős szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="f2482-222">*Free* multi-tenant service, shared with other Azure subscribers, intended for evaluation and small development projects.</span></span>
* <span data-ttu-id="f2482-223">*Alapszintű* dedikált számítási erőforrások biztosít a termelési számítási feladatokhoz kisebb méretekben, magas rendelkezésre állású lekérdezés munkaterhelések legfeljebb három replikával.</span><span class="sxs-lookup"><span data-stu-id="f2482-223">*Basic* provides dedicated computing resources for production workloads at a smaller scale, with up to three replicas for highly available query workloads.</span></span>
* <span data-ttu-id="f2482-224">*Standard (S1, S2, S3, S3 nagy sűrűségű)* értéke nagyobb a termelési számítási feladatokhoz.</span><span class="sxs-lookup"><span data-stu-id="f2482-224">*Standard (S1, S2, S3, S3 High Density)* is for larger production workloads.</span></span> <span data-ttu-id="f2482-225">Több szinten vannak a standard csomagot, hogy Ön egy erőforrás-konfigurációhoz, amely a legjobban illik a munkaterhelés-profil.</span><span class="sxs-lookup"><span data-stu-id="f2482-225">Multiple levels exist within the standard tier so that you can choose a resource configuration that best matches your workload profile.</span></span>

<span data-ttu-id="f2482-226">**Előfizetésenként korlátok**</span><span class="sxs-lookup"><span data-stu-id="f2482-226">**Limits per subscription**</span></span>

[!INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

<span data-ttu-id="f2482-227">**Érhető el keresési szolgáltatásonként korlátok**</span><span class="sxs-lookup"><span data-stu-id="f2482-227">**Limits per search service**</span></span>

[!INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

<span data-ttu-id="f2482-228">További kapcsolatos részletesebb felügyeletét, például a dokumentum mérete, a lekérdezések száma másodpercenként, kulcsok, kérések és válaszok, használati korlátait: [szolgáltatási korlátait, az Azure Search](search/search-limits-quotas-capacity.md).</span><span class="sxs-lookup"><span data-stu-id="f2482-228">To learn more about limits on a more granular level, such as document size, queries per second, keys, requests, and responses, see [Service limits in Azure Search](search/search-limits-quotas-capacity.md).</span></span>

### <a name="media-services-limits"></a><span data-ttu-id="f2482-229">A Media Services korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-229">Media Services limits</span></span>
[!INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="cdn-limits"></a><span data-ttu-id="f2482-230">CDN-korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-230">CDN limits</span></span>
[!INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a><span data-ttu-id="f2482-231">A Mobile Services korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-231">Mobile Services limits</span></span>
[!INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitor-limits"></a><span data-ttu-id="f2482-232">A figyelő korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-232">Monitor limits</span></span>
[!INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hub-service-limits"></a><span data-ttu-id="f2482-233">Értesítési központ szolgáltatásra vonatkozó korlátozások</span><span class="sxs-lookup"><span data-stu-id="f2482-233">Notification Hub Service limits</span></span>
[!INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a><span data-ttu-id="f2482-234">Event Hubs korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-234">Event Hubs limits</span></span>
[!INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a><span data-ttu-id="f2482-235">A Service Bus korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-235">Service Bus limits</span></span>
[!INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a><span data-ttu-id="f2482-236">Az IoT-központ korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-236">IoT Hub limits</span></span>
[!INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="data-factory-limits"></a><span data-ttu-id="f2482-237">Adat-előállító korlátozza.</span><span class="sxs-lookup"><span data-stu-id="f2482-237">Data Factory limits</span></span>
[!INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### <a name="data-lake-analytics-limits"></a><span data-ttu-id="f2482-238">A Data Lake Analytics korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-238">Data Lake Analytics limits</span></span>
[!INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### <a name="data-lake-store-limits"></a><span data-ttu-id="f2482-239">Data Lake Store korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-239">Data Lake Store limits</span></span>
[!INCLUDE [azure-data-lake-store-limits](../includes/azure-data-lake-store-limits.md)]

### <a name="stream-analytics-limits"></a><span data-ttu-id="f2482-240">A Stream Analytics korlátozza.</span><span class="sxs-lookup"><span data-stu-id="f2482-240">Stream Analytics limits</span></span>
[!INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### <a name="active-directory-limits"></a><span data-ttu-id="f2482-241">Active Directory-korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-241">Active Directory limits</span></span>
[!INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]

### <a name="azure-event-grid-limits"></a><span data-ttu-id="f2482-242">Az Azure Event rács korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-242">Azure Event Grid limits</span></span>
[!INCLUDE [event-grid-limits](../includes/event-grid-limits.md)]

### <a name="azure-remoteapp-limits"></a><span data-ttu-id="f2482-243">Az Azure RemoteApp korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-243">Azure RemoteApp limits</span></span>
[!INCLUDE [azure-remoteapp-limits](../includes/azure-remoteapp-limits.md)]

### <a name="storsimple-system-limits"></a><span data-ttu-id="f2482-244">StorSimple rendszer korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-244">StorSimple System limits</span></span>
[!INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]

### <a name="log-analytics-limits"></a><span data-ttu-id="f2482-245">A Naplóelemzési korlátozza.</span><span class="sxs-lookup"><span data-stu-id="f2482-245">Log Analytics limits</span></span>
[!INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### <a name="backup-limits"></a><span data-ttu-id="f2482-246">Biztonsági mentési korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-246">Backup limits</span></span>
[!INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="site-recovery-limits"></a><span data-ttu-id="f2482-247">A Site Recovery korlátai</span><span class="sxs-lookup"><span data-stu-id="f2482-247">Site Recovery limits</span></span>
[!INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a><span data-ttu-id="f2482-248">Application Insights korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-248">Application Insights limits</span></span>
[!INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a><span data-ttu-id="f2482-249">Korlátozza az API Management</span><span class="sxs-lookup"><span data-stu-id="f2482-249">API Management limits</span></span>
[!INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-redis-cache-limits"></a><span data-ttu-id="f2482-250">Azure Redis Cache-korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-250">Azure Redis Cache limits</span></span>
[!INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a><span data-ttu-id="f2482-251">Key Vault korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-251">Key Vault limits</span></span>
[!INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication"></a><span data-ttu-id="f2482-252">Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="f2482-252">Multi-Factor Authentication</span></span>
[!INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a><span data-ttu-id="f2482-253">Automatizálási korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-253">Automation limits</span></span>
[!INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="sql-database-limits"></a><span data-ttu-id="f2482-254">SQL-adatbázis korlátok</span><span class="sxs-lookup"><span data-stu-id="f2482-254">SQL Database limits</span></span>
<span data-ttu-id="f2482-255">SQL adatbázis-korlátok, lásd: [SQL adatbázis erőforrás korlátok](sql-database/sql-database-resource-limits.md).</span><span class="sxs-lookup"><span data-stu-id="f2482-255">For SQL Database limits, see [SQL Database Resource Limits](sql-database/sql-database-resource-limits.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="f2482-256">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="f2482-256">See also</span></span>
[<span data-ttu-id="f2482-257">Azure korlátozását és növekszik ismertetése</span><span class="sxs-lookup"><span data-stu-id="f2482-257">Understanding Azure Limits and Increases</span></span>](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[<span data-ttu-id="f2482-258">Virtuális gépek és Felhőszolgáltatások mérete az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="f2482-258">Virtual Machine and Cloud Service Sizes for Azure</span></span>](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="f2482-259">A Felhőszolgáltatások mérete</span><span class="sxs-lookup"><span data-stu-id="f2482-259">Sizes for Cloud Services</span></span>](cloud-services/cloud-services-sizes-specs.md)

