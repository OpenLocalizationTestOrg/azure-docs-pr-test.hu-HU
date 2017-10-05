---
title: "Azure Redis Cache konfigurálása |} Microsoft Docs"
description: "Azure Redis Cache Redis alapértelmezett konfigurációjának megértéséhez, valamint megtudhatja, hogyan konfigurálhatja az Azure Redis Cache példányt"
services: redis-cache
documentationcenter: na
author: steved0x
manager: douge
editor: tysonn
ms.assetid: d0bf2e1f-6a26-4e62-85ba-d82b35fc5aa6
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 08/22/2017
ms.author: sdanie
ms.openlocfilehash: 0274e58eb2e83202d4dbc58da0c67d0fdde22ede
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-configure-azure-redis-cache"></a><span data-ttu-id="32bd0-103">Azure Redis Cache konfigurálása</span><span class="sxs-lookup"><span data-stu-id="32bd0-103">How to configure Azure Redis Cache</span></span>
<span data-ttu-id="32bd0-104">Ez a témakör ismerteti, hogyan lehet felülvizsgálata és aktualizálása céljából az Azure Redis Cache példány konfigurációját, és hozzá van rendelve az alapértelmezett Redis kiszolgálókonfiguráció az Azure Redis Cache példány.</span><span class="sxs-lookup"><span data-stu-id="32bd0-104">This topic describes how to review and update the configuration for your Azure Redis Cache instances, and covers the default Redis server configuration for Azure Redis Cache instances.</span></span>

> [!NOTE]
> <span data-ttu-id="32bd0-105">Konfigurálásához és használatához prémium gyorsítótár-funkciók további információkért lásd: [adatmegőrzési konfigurálása](cache-how-to-premium-persistence.md), [hogyan konfigurálhatja a fürtözést](cache-how-to-premium-clustering.md), és [virtuális hálózat támogatásának konfigurálása ](cache-how-to-premium-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="32bd0-105">For more information on configuring and using premium cache features, see [How to configure persistence](cache-how-to-premium-persistence.md), [How to configure clustering](cache-how-to-premium-clustering.md), and [How to configure Virtual Network support](cache-how-to-premium-vnet.md).</span></span>
> 
> 

## <a name="configure-redis-cache-settings"></a><span data-ttu-id="32bd0-106">Redis gyorsítótár beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="32bd0-106">Configure Redis cache settings</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

<span data-ttu-id="32bd0-107">Az Azure Redis Cache-gyorsítótár beállításai vannak tekinthetők meg és konfigurálni a **Redis Cache** panel használatával a **erőforrás menü**.</span><span class="sxs-lookup"><span data-stu-id="32bd0-107">Azure Redis Cache settings are viewed and configured on the **Redis Cache** blade using the **Resource Menu**.</span></span>

![Redis gyorsítótár beállításai](./media/cache-configure/redis-cache-settings.png)

<span data-ttu-id="32bd0-109">Megtekintheti és a következő beállításokat használja a **erőforrás menü**.</span><span class="sxs-lookup"><span data-stu-id="32bd0-109">You can view and configure the following settings using the **Resource Menu**.</span></span>

* [<span data-ttu-id="32bd0-110">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="32bd0-110">Overview</span></span>](#overview)
* [<span data-ttu-id="32bd0-111">Tevékenységnapló</span><span class="sxs-lookup"><span data-stu-id="32bd0-111">Activity log</span></span>](#activity-log)
* [<span data-ttu-id="32bd0-112">Hozzáférés-vezérlés (IAM)</span><span class="sxs-lookup"><span data-stu-id="32bd0-112">Access control (IAM)</span></span>](#access-control-iam)
* [<span data-ttu-id="32bd0-113">Címkék</span><span class="sxs-lookup"><span data-stu-id="32bd0-113">Tags</span></span>](#tags)
* [<span data-ttu-id="32bd0-114">Problémák diagnosztizálása és megoldása</span><span class="sxs-lookup"><span data-stu-id="32bd0-114">Diagnose and solve problems</span></span>](#diagnose-and-solve-problems)
* [<span data-ttu-id="32bd0-115">Beállítások</span><span class="sxs-lookup"><span data-stu-id="32bd0-115">Settings</span></span>](#settings)
    * [<span data-ttu-id="32bd0-116">Tárelérési kulcsok</span><span class="sxs-lookup"><span data-stu-id="32bd0-116">Access keys</span></span>](#access-keys)
    * [<span data-ttu-id="32bd0-117">Speciális beállítások</span><span class="sxs-lookup"><span data-stu-id="32bd0-117">Advanced settings</span></span>](#advanced-settings)
    * [<span data-ttu-id="32bd0-118">Redis gyorsítótár Advisor</span><span class="sxs-lookup"><span data-stu-id="32bd0-118">Redis Cache Advisor</span></span>](#redis-cache-advisor)
    * [<span data-ttu-id="32bd0-119">Méretezés</span><span class="sxs-lookup"><span data-stu-id="32bd0-119">Scale</span></span>](#scale)
    * [<span data-ttu-id="32bd0-120">Redis foglalásiegység-méret</span><span class="sxs-lookup"><span data-stu-id="32bd0-120">Redis cluster size</span></span>](#cluster-size)
    * [<span data-ttu-id="32bd0-121">Redis-adatmegőrzés</span><span class="sxs-lookup"><span data-stu-id="32bd0-121">Redis data persistence</span></span>](#redis-data-persistence)
    * [<span data-ttu-id="32bd0-122">Frissítések ütemezése</span><span class="sxs-lookup"><span data-stu-id="32bd0-122">Schedule updates</span></span>](#schedule-updates)
    * [<span data-ttu-id="32bd0-123">Georeplikáció</span><span class="sxs-lookup"><span data-stu-id="32bd0-123">Geo-replication</span></span>](#geo-replication)
    * [<span data-ttu-id="32bd0-124">Virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="32bd0-124">Virtual Network</span></span>](#virtual-network)
    * [<span data-ttu-id="32bd0-125">Tűzfal</span><span class="sxs-lookup"><span data-stu-id="32bd0-125">Firewall</span></span>](#firewall)
    * [<span data-ttu-id="32bd0-126">Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="32bd0-126">Properties</span></span>](#properties)
    * [<span data-ttu-id="32bd0-127">Zárolások feloldása</span><span class="sxs-lookup"><span data-stu-id="32bd0-127">Locks</span></span>](#locks)
    * [<span data-ttu-id="32bd0-128">Automatizálási parancsfájl</span><span class="sxs-lookup"><span data-stu-id="32bd0-128">Automation script</span></span>](#automation-script)
* [<span data-ttu-id="32bd0-129">Felügyeleti</span><span class="sxs-lookup"><span data-stu-id="32bd0-129">Administration</span></span>](#administration)
    * [<span data-ttu-id="32bd0-130">Adatok importálása</span><span class="sxs-lookup"><span data-stu-id="32bd0-130">Import data</span></span>](#importexport)
    * [<span data-ttu-id="32bd0-131">Adatok exportálása</span><span class="sxs-lookup"><span data-stu-id="32bd0-131">Export data</span></span>](#importexport)
    * [<span data-ttu-id="32bd0-132">Újraindítás</span><span class="sxs-lookup"><span data-stu-id="32bd0-132">Reboot</span></span>](#reboot)
* [<span data-ttu-id="32bd0-133">Figyelés</span><span class="sxs-lookup"><span data-stu-id="32bd0-133">Monitoring</span></span>](#monitoring)
    * [<span data-ttu-id="32bd0-134">Redis metrikák</span><span class="sxs-lookup"><span data-stu-id="32bd0-134">Redis metrics</span></span>](#redis-metrics)
    * [<span data-ttu-id="32bd0-135">A riasztási szabályok</span><span class="sxs-lookup"><span data-stu-id="32bd0-135">Alert rules</span></span>](#alert-rules)
    * [<span data-ttu-id="32bd0-136">Diagnosztika</span><span class="sxs-lookup"><span data-stu-id="32bd0-136">Diagnostics</span></span>](#diagnostics)
* [<span data-ttu-id="32bd0-137">Támogatási és hibaelhárítási beállítások</span><span class="sxs-lookup"><span data-stu-id="32bd0-137">Support & troubleshooting settings</span></span>](#support-amp-troubleshooting-settings)
    * [<span data-ttu-id="32bd0-138">Erőforrás állapota</span><span class="sxs-lookup"><span data-stu-id="32bd0-138">Resource health</span></span>](#resource-health)
    * [<span data-ttu-id="32bd0-139">Új támogatási kérelem</span><span class="sxs-lookup"><span data-stu-id="32bd0-139">New support request</span></span>](#new-support-request)


## <a name="overview"></a><span data-ttu-id="32bd0-140">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="32bd0-140">Overview</span></span>

<span data-ttu-id="32bd0-141">**Áttekintés** biztosít alapvető információkat, például nevét, a gyorsítótár, a portok, a tarifacsomag, és kijelölt gyorsítótár metrikákat.</span><span class="sxs-lookup"><span data-stu-id="32bd0-141">**Overview** provides you with basic information about your cache, such as name, ports, pricing tier, and selected cache metrics.</span></span>

### <a name="activity-log"></a><span data-ttu-id="32bd0-142">Tevékenységnapló</span><span class="sxs-lookup"><span data-stu-id="32bd0-142">Activity log</span></span>

<span data-ttu-id="32bd0-143">Kattintson a **tevékenységnapló** művelet elvégezhető a gyorsítótár megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="32bd0-143">Click **Activity log** to view actions performed on your cache.</span></span> <span data-ttu-id="32bd0-144">Is segítségével szűrés bontsa ki az ebben a nézetben más erőforrások.</span><span class="sxs-lookup"><span data-stu-id="32bd0-144">You can also use filtering to expand this view to include other resources.</span></span> <span data-ttu-id="32bd0-145">A vizsgálati naplók munkáról bővebben lásd: [naplózási műveletek a Resource Manager](../azure-resource-manager/resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="32bd0-145">For more information on working with audit logs, see [Audit operations with Resource Manager](../azure-resource-manager/resource-group-audit.md).</span></span> <span data-ttu-id="32bd0-146">Azure Redis Cache események figyelésével kapcsolatos további információkért lásd: [műveletek és a riasztások](cache-how-to-monitor.md#operations-and-alerts).</span><span class="sxs-lookup"><span data-stu-id="32bd0-146">For more information on monitoring Azure Redis Cache events, see [Operations and alerts](cache-how-to-monitor.md#operations-and-alerts).</span></span>

### <a name="access-control-iam"></a><span data-ttu-id="32bd0-147">Hozzáférés-vezérlés (IAM)</span><span class="sxs-lookup"><span data-stu-id="32bd0-147">Access control (IAM)</span></span>

<span data-ttu-id="32bd0-148">A **hozzáférés-vezérlés (IAM)** szakasz támogatja a szerepköralapú hozzáférés-vezérlést (RBAC) a szervezetek számára követelményeknek a access management egyszerűen és pontosan Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="32bd0-148">The **Access control (IAM)** section provides support for role-based access control (RBAC) in the Azure portal to help organizations meet their access management requirements simply and precisely.</span></span> <span data-ttu-id="32bd0-149">További információkért lásd: [az Azure portál szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="32bd0-149">For more information, see [Role-based access control in the Azure portal](../active-directory/role-based-access-control-configure.md).</span></span>

### <a name="tags"></a><span data-ttu-id="32bd0-150">Címkék</span><span class="sxs-lookup"><span data-stu-id="32bd0-150">Tags</span></span>

<span data-ttu-id="32bd0-151">A **címkék** szakasz segítséget nyújt az erőforrások rendszerezése.</span><span class="sxs-lookup"><span data-stu-id="32bd0-151">The **Tags** section helps you organize your resources.</span></span> <span data-ttu-id="32bd0-152">További információkért lásd: [az Azure-erőforrások rendszerezése címkék használatával](../azure-resource-manager/resource-group-using-tags.md).</span><span class="sxs-lookup"><span data-stu-id="32bd0-152">For more information, see [Using tags to organize your Azure resources](../azure-resource-manager/resource-group-using-tags.md).</span></span>


### <a name="diagnose-and-solve-problems"></a><span data-ttu-id="32bd0-153">Problémák diagnosztizálása és megoldása</span><span class="sxs-lookup"><span data-stu-id="32bd0-153">Diagnose and solve problems</span></span>

<span data-ttu-id="32bd0-154">Kattintson a **Diagnosztizálás és problémák megoldására** megoldása érdekében ezeket a gyakori problémák és stratégiák meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="32bd0-154">Click **Diagnose and solve problems** to be provided with common issues and strategies for resolving them.</span></span>



## <a name="settings"></a><span data-ttu-id="32bd0-155">Beállítások</span><span class="sxs-lookup"><span data-stu-id="32bd0-155">Settings</span></span>
<span data-ttu-id="32bd0-156">A **beállítások** szakasz lehetővé teszi a eléréséhez, és a következő beállításokat a gyorsítótárhoz.</span><span class="sxs-lookup"><span data-stu-id="32bd0-156">The **Settings** section allows you to access and configure the following settings for your cache.</span></span>

* [<span data-ttu-id="32bd0-157">Tárelérési kulcsok</span><span class="sxs-lookup"><span data-stu-id="32bd0-157">Access keys</span></span>](#access-keys)
* [<span data-ttu-id="32bd0-158">Speciális beállítások</span><span class="sxs-lookup"><span data-stu-id="32bd0-158">Advanced settings</span></span>](#advanced-settings)
* [<span data-ttu-id="32bd0-159">Redis gyorsítótár Advisor</span><span class="sxs-lookup"><span data-stu-id="32bd0-159">Redis Cache Advisor</span></span>](#redis-cache-advisor)
* [<span data-ttu-id="32bd0-160">Méretezés</span><span class="sxs-lookup"><span data-stu-id="32bd0-160">Scale</span></span>](#scale)
* [<span data-ttu-id="32bd0-161">Redis foglalásiegység-méret</span><span class="sxs-lookup"><span data-stu-id="32bd0-161">Redis cluster size</span></span>](#cluster-size)
* [<span data-ttu-id="32bd0-162">Redis-adatmegőrzés</span><span class="sxs-lookup"><span data-stu-id="32bd0-162">Redis data persistence</span></span>](#redis-data-persistence)
* [<span data-ttu-id="32bd0-163">Frissítések ütemezése</span><span class="sxs-lookup"><span data-stu-id="32bd0-163">Schedule updates</span></span>](#schedule-updates)
* [<span data-ttu-id="32bd0-164">Georeplikáció</span><span class="sxs-lookup"><span data-stu-id="32bd0-164">Geo-replication</span></span>](#geo-replication)
* [<span data-ttu-id="32bd0-165">Virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="32bd0-165">Virtual Network</span></span>](#virtual-network)
* [<span data-ttu-id="32bd0-166">Tűzfal</span><span class="sxs-lookup"><span data-stu-id="32bd0-166">Firewall</span></span>](#firewall)
* [<span data-ttu-id="32bd0-167">Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="32bd0-167">Properties</span></span>](#properties)
* [<span data-ttu-id="32bd0-168">Zárolások feloldása</span><span class="sxs-lookup"><span data-stu-id="32bd0-168">Locks</span></span>](#locks)
* [<span data-ttu-id="32bd0-169">Automatizálási parancsfájl</span><span class="sxs-lookup"><span data-stu-id="32bd0-169">Automation script</span></span>](#automation-script)



### <a name="access-keys"></a><span data-ttu-id="32bd0-170">Elérési kulcs</span><span class="sxs-lookup"><span data-stu-id="32bd0-170">Access keys</span></span>
<span data-ttu-id="32bd0-171">Kattintson a **hívóbetűk** megtekintéséhez vagy a gyorsítótár elérési kulcsainak újragenerálása.</span><span class="sxs-lookup"><span data-stu-id="32bd0-171">Click **Access keys** to view or regenerate the access keys for your cache.</span></span> <span data-ttu-id="32bd0-172">Ezeket a kulcsokat a gyorsítótárhoz kapcsolódó ügyfelek által használt.</span><span class="sxs-lookup"><span data-stu-id="32bd0-172">These keys are used by the clients connecting to your cache.</span></span>

![Redis gyorsítótár elérési kulcsainak](./media/cache-configure/redis-cache-manage-keys.png)

### <a name="advanced-settings"></a><span data-ttu-id="32bd0-174">Speciális beállítások</span><span class="sxs-lookup"><span data-stu-id="32bd0-174">Advanced settings</span></span>
<span data-ttu-id="32bd0-175">A következő beállításokat a **speciális beállítások** panelen.</span><span class="sxs-lookup"><span data-stu-id="32bd0-175">The following settings are configured on the **Advanced settings** blade.</span></span>

* [<span data-ttu-id="32bd0-176">Hozzáférési portok</span><span class="sxs-lookup"><span data-stu-id="32bd0-176">Access Ports</span></span>](#access-ports)
* [<span data-ttu-id="32bd0-177">Memória házirendek</span><span class="sxs-lookup"><span data-stu-id="32bd0-177">Memory policies</span></span>](#memory-policies)
* [<span data-ttu-id="32bd0-178">Kulcstérértesítések használatával értesítések (Speciális beállítások)</span><span class="sxs-lookup"><span data-stu-id="32bd0-178">Keyspace notifications (advanced settings)</span></span>](#keyspace-notifications-advanced-settings)

#### <a name="access-ports"></a><span data-ttu-id="32bd0-179">Hozzáférési portok</span><span class="sxs-lookup"><span data-stu-id="32bd0-179">Access Ports</span></span>
<span data-ttu-id="32bd0-180">A nem SSL hozzáférés alapértelmezés szerint le van tiltva az új gyorsítótárakhoz.</span><span class="sxs-lookup"><span data-stu-id="32bd0-180">By default, non-SSL access is disabled for new caches.</span></span> <span data-ttu-id="32bd0-181">A nem SSL port engedélyezéséhez kattintson **nem** a **engedélyezi a hozzáférést csak SSL keresztül** a a **speciális beállítások** panel megnyitásához, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="32bd0-181">To enable the non-SSL port, click **No** for **Allow access only via SSL** on the **Advanced settings** blade and then click **Save**.</span></span>

![A redis Cache hozzáférési portok](./media/cache-configure/redis-cache-access-ports.png)

<a name="maxmemory-policy-and-maxmemory-reserved"></a>
#### <a name="memory-policies"></a><span data-ttu-id="32bd0-183">Memória házirendek</span><span class="sxs-lookup"><span data-stu-id="32bd0-183">Memory policies</span></span>
<span data-ttu-id="32bd0-184">A **Maxmemory házirend**, **maxmemory fenntartott**, és **maxfragmentationmemory fenntartott** beállításait a **speciális beállítások** panelen konfigurálhatja a memória-házirendeket, a gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="32bd0-184">The **Maxmemory policy**, **maxmemory-reserved**, and **maxfragmentationmemory-reserved** settings on the **Advanced settings** blade configure the memory policies for the cache.</span></span>

![Redis gyorsítótár Maxmemory házirend](./media/cache-configure/redis-cache-maxmemory-policy.png)

<span data-ttu-id="32bd0-186">**Maxmemory házirend** a kiürítés irányelvet konfigurál a gyorsítótárban, és lehetővé teszi a következő kiürítési házirendek közül választhat:</span><span class="sxs-lookup"><span data-stu-id="32bd0-186">**Maxmemory policy** configures the eviction policy for the cache and allows you to choose from the following eviction policies:</span></span>

* <span data-ttu-id="32bd0-187">`volatile-lru`-Ez az alapértelmezett beállítás.</span><span class="sxs-lookup"><span data-stu-id="32bd0-187">`volatile-lru` - this is the default.</span></span>
* `allkeys-lru`
* `volatile-random`
* `allkeys-random`
* `volatile-ttl`
* `noeviction`

<span data-ttu-id="32bd0-188">További információ `maxmemory` házirendek, lásd: [kiürítés házirendek](http://redis.io/topics/lru-cache#eviction-policies).</span><span class="sxs-lookup"><span data-stu-id="32bd0-188">For more information about `maxmemory` policies, see [Eviction policies](http://redis.io/topics/lru-cache#eviction-policies).</span></span>

<span data-ttu-id="32bd0-189">A **maxmemory fenntartott** beállítással memória MB-ban, amely nem gyorsítótár műveletek, például a feladatátvétel során replikációs számára van fenntartva.</span><span class="sxs-lookup"><span data-stu-id="32bd0-189">The **maxmemory-reserved** setting configures the amount of memory in MB that is reserved for non-cache operations such as replication during failover.</span></span> <span data-ttu-id="32bd0-190">Ha az érték lehetővé teszi, hogy a Redis server egységesebb, amikor változik a terhelés.</span><span class="sxs-lookup"><span data-stu-id="32bd0-190">Setting this value allows you to have a more consistent Redis server experience when your load varies.</span></span> <span data-ttu-id="32bd0-191">Ez az érték nagyobb munkaterhelésekhez, amelyek írni a gyakori kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="32bd0-191">This value should be set higher for workloads that are write heavy.</span></span> <span data-ttu-id="32bd0-192">Amikor memória a műveletek számára van fenntartva, nem érhető el a gyorsítótárazott adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="32bd0-192">When memory is reserved for such operations, it is unavailable for storage of cached data.</span></span>

<span data-ttu-id="32bd0-193">A **maxfragmentationmemory fenntartott** beállítással memória MB-ban, amely a memória töredezettségét befogadásához van fenntartva.</span><span class="sxs-lookup"><span data-stu-id="32bd0-193">The **maxfragmentationmemory-reserved** setting configures the amount of memory in MB that is reserved to accommodate for memory fragmentation.</span></span> <span data-ttu-id="32bd0-194">Ha az érték lehetővé teszi egy egységesebb Redis-kiszolgálót, ha a gyorsítótár megtelt vagy megközelíti a teljes telepítési és a töredezettséget arány is nagy.</span><span class="sxs-lookup"><span data-stu-id="32bd0-194">Setting this value allows you to have a more consistent Redis server experience when the cache is full or close to full and the fragmentation ratio is also high.</span></span> <span data-ttu-id="32bd0-195">Amikor memória a műveletek számára van fenntartva, nem érhető el a gyorsítótárazott adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="32bd0-195">When memory is reserved for such operations, it is unavailable for storage of cached data.</span></span>

<span data-ttu-id="32bd0-196">Érdemes egy új foglalás memóriaméretnél kiválasztásakor (**maxmemory fenntartott** vagy **maxfragmentationmemory fenntartott**) milyen hatással van a módosítás az egy gyorsítótár, amely már fut. nagy mennyiségű adatot.</span><span class="sxs-lookup"><span data-stu-id="32bd0-196">One thing to consider when choosing a new memory reservation value (**maxmemory-reserved** or **maxfragmentationmemory-reserved**) is how this change might affect a cache that is already running with large amounts of data in it.</span></span> <span data-ttu-id="32bd0-197">Például ha 49 GB adatot 53 GB gyorsítótár rendelkezik, majd módosítsa a Foglalás 8 GB, ezzel eldobja a maximális rendelkezésre álló memória, a rendszer le 45 GB.</span><span class="sxs-lookup"><span data-stu-id="32bd0-197">For instance, if you have a 53 GB cache with 49 GB of data, then change the reservation value to 8 GB, this will drop the max available memory for the system down to 45 GB.</span></span> <span data-ttu-id="32bd0-198">Ha az aktuális `used_memory` vagy a `used_memory_rss` értékek magasabbak, mint az új 45 GB-os korlátját, akkor a rendszer lesz, amíg adatok kizárása `used_memory` és `used_memory_rss` 45 GB alatt van.</span><span class="sxs-lookup"><span data-stu-id="32bd0-198">If either your current `used_memory` or your `used_memory_rss` values are higher than the new limit of 45 GB, then the system will have to evict data until both `used_memory` and `used_memory_rss` are below 45 GB.</span></span> <span data-ttu-id="32bd0-199">A kiürítési növelheti a kiszolgáló terhelés és a memória töredezettsége.</span><span class="sxs-lookup"><span data-stu-id="32bd0-199">Eviction can increase server load and memory fragmentation.</span></span> <span data-ttu-id="32bd0-200">További információt a gyorsítótár mérőszámokat például `used_memory` és `used_memory_rss`, lásd: [elérhető és a jelentéskészítés intervallumok](cache-how-to-monitor.md#available-metrics-and-reporting-intervals).</span><span class="sxs-lookup"><span data-stu-id="32bd0-200">For more information on cache metrics such as `used_memory` and `used_memory_rss`, see [Available metrics and reporting intervals](cache-how-to-monitor.md#available-metrics-and-reporting-intervals).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="32bd0-201">A **maxmemory fenntartott** és **maxfragmentationmemory fenntartott** beállítások csak érhetők el a Standard és Premium gyorsítótárazza.</span><span class="sxs-lookup"><span data-stu-id="32bd0-201">The **maxmemory-reserved** and **maxfragmentationmemory-reserved** settings are only available for Standard and Premium caches.</span></span>
> 
> 

#### <a name="keyspace-notifications-advanced-settings"></a><span data-ttu-id="32bd0-202">Kulcstérértesítések használatával értesítések (Speciális beállítások)</span><span class="sxs-lookup"><span data-stu-id="32bd0-202">Keyspace notifications (advanced settings)</span></span>
<span data-ttu-id="32bd0-203">Kulcstérértesítések használatával értesítések konfigurálása a redis a **speciális beállítások** panelen.</span><span class="sxs-lookup"><span data-stu-id="32bd0-203">Redis keyspace notifications are configured on the **Advanced settings** blade.</span></span> <span data-ttu-id="32bd0-204">Kulcstérértesítések használatával értesítések engedélyezése az ügyfelek értesítéseket meghatározott események bekövetkezésekor.</span><span class="sxs-lookup"><span data-stu-id="32bd0-204">Keyspace notifications allow clients to receive notifications when certain events occur.</span></span>

![Redis gyorsítótár speciális beállításai](./media/cache-configure/redis-cache-advanced-settings.png)

> [!IMPORTANT]
> <span data-ttu-id="32bd0-206">Kulcstérértesítések használatával értesítések és a **értesítés kulcstérértesítések használatával-események** beállítás csak akkor érhetők Standard és Premium gyorsítótárak esetében.</span><span class="sxs-lookup"><span data-stu-id="32bd0-206">Keyspace notifications and the **notify-keyspace-events** setting are only available for Standard and Premium caches.</span></span>
> 
> 

<span data-ttu-id="32bd0-207">További információkért lásd: [Redis kulcstérértesítések használatával értesítések](http://redis.io/topics/notifications).</span><span class="sxs-lookup"><span data-stu-id="32bd0-207">For more information, see [Redis Keyspace Notifications](http://redis.io/topics/notifications).</span></span> <span data-ttu-id="32bd0-208">Mintakód, tekintse meg a [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) fájlt a [Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) minta.</span><span class="sxs-lookup"><span data-stu-id="32bd0-208">For sample code, see the [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) file in the [Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) sample.</span></span>


<a name="recommendations"></a>
## <a name="redis-cache-advisor"></a><span data-ttu-id="32bd0-209">Redis gyorsítótár Advisor</span><span class="sxs-lookup"><span data-stu-id="32bd0-209">Redis Cache Advisor</span></span>
<span data-ttu-id="32bd0-210">A **Redis gyorsítótár Advisor** csempe megjeleníti a gyorsítótár javaslatok.</span><span class="sxs-lookup"><span data-stu-id="32bd0-210">The **Redis Cache Advisor** blade displays recommendations for your cache.</span></span> <span data-ttu-id="32bd0-211">A normál működés során nincsenek ajánlatok jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="32bd0-211">During normal operations, no recommendations are displayed.</span></span> 

![Javaslatok](./media/cache-configure/redis-cache-no-recommendations.png)

<span data-ttu-id="32bd0-213">Ha például magas memóriahasználat, a hálózati sávszélesség vagy a kiszolgáló terhelését a gyorsítótár a műveletek során a feltételek teljesülnek egy riasztás jelenik meg a **Redis Cache** panelen.</span><span class="sxs-lookup"><span data-stu-id="32bd0-213">If any conditions occur during the operations of your cache such as high memory usage, network bandwidth, or server load, an alert is displayed on the **Redis Cache** blade.</span></span>

![Javaslatok](./media/cache-configure/redis-cache-recommendations-alert.png)

<span data-ttu-id="32bd0-215">További információ található a **javaslatok** panelen.</span><span class="sxs-lookup"><span data-stu-id="32bd0-215">Further information can be found on the **Recommendations** blade.</span></span>

![Javaslatok](./media/cache-configure/redis-cache-recommendations.png)

<span data-ttu-id="32bd0-217">A metrikák a figyelheti a [diagramok figyelési](cache-how-to-monitor.md#monitoring-charts) és [használati diagramok](cache-how-to-monitor.md#usage-charts) szakasza a **Redis Cache** panelen.</span><span class="sxs-lookup"><span data-stu-id="32bd0-217">You can monitor these metrics on the [Monitoring charts](cache-how-to-monitor.md#monitoring-charts) and [Usage charts](cache-how-to-monitor.md#usage-charts) sections of the **Redis Cache** blade.</span></span>

<span data-ttu-id="32bd0-218">Minden tarifacsomag különböző korlátai ügyfélkapcsolatokat, a memória és a sávszélesség rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="32bd0-218">Each pricing tier has different limits for client connections, memory, and bandwidth.</span></span> <span data-ttu-id="32bd0-219">Ha a gyorsítótár maximális kapacitás metrikákat megközelíti egy huzamosabb ideig keresztül, az ajánlás jön létre.</span><span class="sxs-lookup"><span data-stu-id="32bd0-219">If your cache approaches maximum capacity for these metrics over a sustained period of time, a recommendation is created.</span></span> <span data-ttu-id="32bd0-220">További információ a metrikák és korlátai vizsgálja felül a **javaslatok** eszköz, a következő táblázatban találja:</span><span class="sxs-lookup"><span data-stu-id="32bd0-220">For more information about the metrics and limits reviewed by the **Recommendations** tool, see the following table:</span></span>

| <span data-ttu-id="32bd0-221">Redis gyorsítótár metrika</span><span class="sxs-lookup"><span data-stu-id="32bd0-221">Redis Cache metric</span></span> | <span data-ttu-id="32bd0-222">További információ</span><span class="sxs-lookup"><span data-stu-id="32bd0-222">More information</span></span> |
| --- | --- |
| <span data-ttu-id="32bd0-223">A sávszélesség-használat</span><span class="sxs-lookup"><span data-stu-id="32bd0-223">Network bandwidth usage</span></span> |[<span data-ttu-id="32bd0-224">Gyorsítótár teljesítmény - rendelkezésre álló sávszélesség</span><span class="sxs-lookup"><span data-stu-id="32bd0-224">Cache performance - available bandwidth</span></span>](cache-faq.md#cache-performance) |
| <span data-ttu-id="32bd0-225">Csatlakozott ügyfelek</span><span class="sxs-lookup"><span data-stu-id="32bd0-225">Connected clients</span></span> |[<span data-ttu-id="32bd0-226">Alapértelmezett Redis-kiszolgáló konfiguráció - maxclients</span><span class="sxs-lookup"><span data-stu-id="32bd0-226">Default Redis server configuration - maxclients</span></span>](#maxclients) |
| <span data-ttu-id="32bd0-227">A kiszolgálóterhelés</span><span class="sxs-lookup"><span data-stu-id="32bd0-227">Server load</span></span> |[<span data-ttu-id="32bd0-228">Használati diagramok - Redis-kiszolgáló terhelését</span><span class="sxs-lookup"><span data-stu-id="32bd0-228">Usage charts - Redis Server Load</span></span>](cache-how-to-monitor.md#usage-charts) |
| <span data-ttu-id="32bd0-229">Memóriahasználat</span><span class="sxs-lookup"><span data-stu-id="32bd0-229">Memory usage</span></span> |[<span data-ttu-id="32bd0-230">Gyorsítótár teljesítmény - mérete</span><span class="sxs-lookup"><span data-stu-id="32bd0-230">Cache performance - size</span></span>](cache-faq.md#cache-performance) |

<span data-ttu-id="32bd0-231">Frissítse a gyorsítótárat, kattintson a **frissítés most** a tarifacsomag módosítása és [méretezési](#scale) a gyorsítótárhoz.</span><span class="sxs-lookup"><span data-stu-id="32bd0-231">To upgrade your cache, click **Upgrade now** to change the pricing tier and [scale](#scale) your cache.</span></span> <span data-ttu-id="32bd0-232">Tarifacsomag kiválasztásáról további információkért lásd: [milyen Redis Cache-ajánlatot és méretet használjam?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)</span><span class="sxs-lookup"><span data-stu-id="32bd0-232">For more information on choosing a pricing tier, see [What Redis Cache offering and size should I use?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)</span></span>


### <a name="scale"></a><span data-ttu-id="32bd0-233">Méretezés</span><span class="sxs-lookup"><span data-stu-id="32bd0-233">Scale</span></span>
<span data-ttu-id="32bd0-234">Kattintson a **méretezési** lehet megtekinteni vagy módosítani a gyorsítótár tarifacsomag.</span><span class="sxs-lookup"><span data-stu-id="32bd0-234">Click **Scale** to view or change the pricing tier for your cache.</span></span> <span data-ttu-id="32bd0-235">A méretezés további információkért lásd: [Scale Azure Redis Cache hogyan](cache-how-to-scale.md).</span><span class="sxs-lookup"><span data-stu-id="32bd0-235">For more information on scaling, see [How to Scale Azure Redis Cache](cache-how-to-scale.md).</span></span>

![Redis gyorsítótár tarifacsomag](./media/cache-configure/pricing-tier.png)

<a name="cluster-size"></a>

### <a name="redis-cluster-size"></a><span data-ttu-id="32bd0-237">Redis foglalásiegység-méret</span><span class="sxs-lookup"><span data-stu-id="32bd0-237">Redis Cluster Size</span></span>
<span data-ttu-id="32bd0-238">Kattintson a **(előzetes verzió) Redis fürtméret** futó a fürt méretének módosításához prémium gyorsítótár fürtözési engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="32bd0-238">Click **(PREVIEW) Redis Cluster Size** to change the cluster size for a running premium cache with clustering enabled.</span></span>

> [!NOTE]
> <span data-ttu-id="32bd0-239">Vegye figyelembe, hogy közben az Azure Redis Cache prémium szintjének kiadott általános rendelkezésre állási, a fürtméret Redis funkció jelenleg előzetes verzióban érhetők.</span><span class="sxs-lookup"><span data-stu-id="32bd0-239">Note that while the Azure Redis Cache Premium tier has been released to General Availability, the Redis Cluster Size feature is currently in preview.</span></span>
> 
> 

![Redis foglalásiegység-méret](./media/cache-configure/redis-cache-redis-cluster-size.png)

<span data-ttu-id="32bd0-241">A fürt méretének módosításához használja a csúszkát, vagy írjon be egy számot 1 és 10 között a **Shard száma** szöveges értéket, majd kattintson **OK** mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="32bd0-241">To change the cluster size, use the slider or type a number between 1 and 10 in the **Shard count** text box and click **OK** to save.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="32bd0-242">Redis fürtszolgáltatás csak érhető el prémium gyorsítótárak esetében.</span><span class="sxs-lookup"><span data-stu-id="32bd0-242">Redis clustering is only available for Premium caches.</span></span> <span data-ttu-id="32bd0-243">További információk: [How to configure clustering for a Premium Azure Redis Cache](cache-how-to-premium-clustering.md) (Fürtözés konfigurálása prémium szintű Azure Redis Cache-gyorsítótárhoz).</span><span class="sxs-lookup"><span data-stu-id="32bd0-243">For more information, see [How to configure clustering for a Premium Azure Redis Cache](cache-how-to-premium-clustering.md).</span></span>
> 
> 


### <a name="redis-data-persistence"></a><span data-ttu-id="32bd0-244">Redis-adatmegőrzés</span><span class="sxs-lookup"><span data-stu-id="32bd0-244">Redis data persistence</span></span>
<span data-ttu-id="32bd0-245">Kattintson a **Redis-adatmegőrzés** engedélyezése, letiltása, vagy konfigurálja a prémium szintű gyorsítótár adatainak megőrzését.</span><span class="sxs-lookup"><span data-stu-id="32bd0-245">Click **Redis data persistence** to enable, disable, or configure data persistence for your premium cache.</span></span> <span data-ttu-id="32bd0-246">Azure Redis Cache Redis-adatmegőrzés használatával biztosít [Rekordadatbázis adatmegőrzési](cache-how-to-premium-persistence.md#configure-rdb-persistence) vagy [AOF adatmegőrzési](cache-how-to-premium-persistence.md#configure-aof-persistence).</span><span class="sxs-lookup"><span data-stu-id="32bd0-246">Azure Redis Cache offers Redis persistence using either [RDB persistence](cache-how-to-premium-persistence.md#configure-rdb-persistence) or [AOF persistence](cache-how-to-premium-persistence.md#configure-aof-persistence).</span></span>

<span data-ttu-id="32bd0-247">További információkért lásd: [konfigurálása prémium szintű Azure Redis Cache megőrzését](cache-how-to-premium-persistence.md).</span><span class="sxs-lookup"><span data-stu-id="32bd0-247">For more information, see [How to configure persistence for a Premium Azure Redis Cache](cache-how-to-premium-persistence.md).</span></span>


> [!IMPORTANT]
> <span data-ttu-id="32bd0-248">Prémium szintű gyorsítótárak redis-adatmegőrzés csak érhető el.</span><span class="sxs-lookup"><span data-stu-id="32bd0-248">Redis data persistence is only available for Premium caches.</span></span> 
> 
> 

### <a name="schedule-updates"></a><span data-ttu-id="32bd0-249">Frissítések ütemezése</span><span class="sxs-lookup"><span data-stu-id="32bd0-249">Schedule updates</span></span>
<span data-ttu-id="32bd0-250">A **frissítések ütemezése** panel lehetővé teszi, hogy a gyorsítótár Redis-kiszolgáló frissítések karbantartási időszakot.</span><span class="sxs-lookup"><span data-stu-id="32bd0-250">The **Schedule updates** blade allows you to designate a maintenance window for Redis server updates for your cache.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="32bd0-251">A karbantartási időszak csak a kiszolgálói frissítések Redis vonatkozik, és nem az összes Azure-bA frissít, vagy frissíti az operációs rendszer, a gyorsítótár üzemeltető virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="32bd0-251">The maintenance window applies only to Redis server updates, and not to any Azure updates or updates to the operating system of the VMs that host the cache.</span></span>
> 
> 

![Frissítések ütemezése](./media/cache-configure/redis-schedule-updates.png)

<span data-ttu-id="32bd0-253">Adja meg a karbantartási időszak, ellenőrizze a kívánt napok, és adja meg a karbantartási időszak kezdő időpontja minden nap, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="32bd0-253">To specify a maintenance window, check the desired days and specify the maintenance window start hour for each day, and click **OK**.</span></span> <span data-ttu-id="32bd0-254">Vegye figyelembe, hogy a karbantartási ablak időpontja UTC szerint.</span><span class="sxs-lookup"><span data-stu-id="32bd0-254">Note that the maintenance window time is in UTC.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="32bd0-255">A **frissítések ütemezése** funkciót csak érhető el prémium réteghez gyorsítótárainak.</span><span class="sxs-lookup"><span data-stu-id="32bd0-255">The **Schedule updates** functionality is only available for Premium tier caches.</span></span> <span data-ttu-id="32bd0-256">További információt és útmutatást lásd: [Azure Redis Cache felügyeleti - frissítések ütemezése](cache-administration.md#schedule-updates).</span><span class="sxs-lookup"><span data-stu-id="32bd0-256">For more information and instructions, see [Azure Redis Cache administration - Schedule updates](cache-administration.md#schedule-updates).</span></span>
> 
> 

### <a name="geo-replication"></a><span data-ttu-id="32bd0-257">Georeplikáció</span><span class="sxs-lookup"><span data-stu-id="32bd0-257">Geo-replication</span></span>

<span data-ttu-id="32bd0-258">A **georeplikáció** panel lehetővé teszi a csatolás két Premium szint Azure Redis Cache példányt.</span><span class="sxs-lookup"><span data-stu-id="32bd0-258">The **Geo-replication** blade provides a mechanism for linking two Premium tier Azure Redis Cache instances.</span></span> <span data-ttu-id="32bd0-259">Egy gyorsítótár az elsődleges társított gyorsítótár, míg a másik a másodlagos csatolt gyorsítótár van kijelölve.</span><span class="sxs-lookup"><span data-stu-id="32bd0-259">One cache is designated as the primary linked cache, and the other as the secondary linked cache.</span></span> <span data-ttu-id="32bd0-260">A másodlagos csatolt gyorsítótár csak olvashatóvá válik, és az elsődleges gyorsítótár írt adatokat a rendszer replikálja a másodlagos csatolt gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="32bd0-260">The secondary linked cache becomes read-only, and data written to the primary cache is replicated to the secondary linked cache.</span></span> <span data-ttu-id="32bd0-261">Ez a funkció a gyorsítótár Azure-régiók közötti replikáció használható.</span><span class="sxs-lookup"><span data-stu-id="32bd0-261">This functionality can be used to replicate a cache across Azure regions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="32bd0-262">**A georeplikáció** lehetőség csak a prémium szintű réteghez gyorsítótárainak.</span><span class="sxs-lookup"><span data-stu-id="32bd0-262">**Geo-replication** is only available for Premium tier caches.</span></span> <span data-ttu-id="32bd0-263">További információt és útmutatást lásd: [georeplikáció konfigurálása az Azure Redis Cache](cache-how-to-geo-replication.md).</span><span class="sxs-lookup"><span data-stu-id="32bd0-263">For more information and instructions, see [How to configure Geo-replication for Azure Redis Cache](cache-how-to-geo-replication.md).</span></span>
> 
> 

### <a name="virtual-network"></a><span data-ttu-id="32bd0-264">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="32bd0-264">Virtual Network</span></span>
<span data-ttu-id="32bd0-265">A **virtuális hálózati** szakasz lehetővé teszi, hogy a virtuális hálózat beállításait a gyorsítótárhoz.</span><span class="sxs-lookup"><span data-stu-id="32bd0-265">The **Virtual Network** section allows you to configure the virtual network settings for your cache.</span></span> <span data-ttu-id="32bd0-266">A prémium szintű gyorsítótár virtuális hálózaton létrehozásával kapcsolatos információkat támogatja, és a beállítások frissítése, lásd: [konfigurálása a virtuális hálózat támogatja a Premium Azure Redis Cache](cache-how-to-premium-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="32bd0-266">For information on creating a premium cache with VNET support and updating its settings, see [How to configure Virtual Network Support for a Premium Azure Redis Cache](cache-how-to-premium-vnet.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="32bd0-267">Virtuális hálózati beállítások csak érhetők el a prémium szintű gyorsítótárak konfigurált VNET támogató gyorsítótár létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="32bd0-267">Virtual network settings are only available for premium caches that were configured with VNET support during cache creation.</span></span> 
> 
> 

### <a name="firewall"></a><span data-ttu-id="32bd0-268">Tűzfal</span><span class="sxs-lookup"><span data-stu-id="32bd0-268">Firewall</span></span>

<span data-ttu-id="32bd0-269">Kattintson a **tűzfal** megtekintéséhez és a Premium Azure Redis Cache tűzfalszabályok konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="32bd0-269">Click **Firewall** to view and configure firewall rules for your Premium Azure Redis Cache.</span></span>

![Tűzfal](./media/cache-configure/redis-firewall-rules.png)

<span data-ttu-id="32bd0-271">A tűzfalszabályok kezdő és záró IP-címtartománnyal rendelkező adhatja meg.</span><span class="sxs-lookup"><span data-stu-id="32bd0-271">You can specify firewall rules with a start and end IP address range.</span></span> <span data-ttu-id="32bd0-272">Tűzfalszabályok konfigurálásakor csak a megadott IP-címtartományok érkező ügyfélkapcsolatokat is elérheti a gyorsítótárban.</span><span class="sxs-lookup"><span data-stu-id="32bd0-272">When firewall rules are configured, only client connections from the specified IP address ranges can connect to the cache.</span></span> <span data-ttu-id="32bd0-273">Egy tűzfalszabály mentésekor a rendszer nincs rövid késleltetés előtt a szabály érvényben.</span><span class="sxs-lookup"><span data-stu-id="32bd0-273">When a firewall rule is saved there is a short delay before the rule is effective.</span></span> <span data-ttu-id="32bd0-274">Ez a késés van általában kevesebb mint egy perc.</span><span class="sxs-lookup"><span data-stu-id="32bd0-274">This delay is typically less than one minute.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="32bd0-275">Azure Redis Cache rendszerek figyelése a kapcsolatok mindig engedélyezettek, még akkor is, ha a tűzfal-szabályok úgy vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="32bd0-275">Connections from Azure Redis Cache monitoring systems are always permitted, even if firewall rules are configured.</span></span>
> 
> <span data-ttu-id="32bd0-276">Tűzfalszabályok esetén csak prémium szintű réteghez gyorsítótárainak érhetők el.</span><span class="sxs-lookup"><span data-stu-id="32bd0-276">Firewall rules are only available for Premium tier caches.</span></span>
> 
> 

### <a name="properties"></a><span data-ttu-id="32bd0-277">Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="32bd0-277">Properties</span></span>
<span data-ttu-id="32bd0-278">Kattintson a **tulajdonságok** a gyorsítótárhoz, beleértve a gyorsítótár végpontjához és portok vonatkozó információk megtekintése.</span><span class="sxs-lookup"><span data-stu-id="32bd0-278">Click **Properties** to view information about your cache, including the cache endpoint and ports.</span></span>

![Redis gyorsítótár tulajdonságai](./media/cache-configure/redis-cache-properties.png)

### <a name="locks"></a><span data-ttu-id="32bd0-280">Zárolások</span><span class="sxs-lookup"><span data-stu-id="32bd0-280">Locks</span></span>
<span data-ttu-id="32bd0-281">A **zárolja** szakasz lehetővé teszi, hogy egy előfizetés, erőforráscsoportból vagy erőforrás véletlen törlése vagy a kritikus erőforrásokat módosítása a munkahely más felhasználóinak megelőzése érdekében zárolja.</span><span class="sxs-lookup"><span data-stu-id="32bd0-281">The **Locks** section allows you to lock a subscription, resource group, or resource to prevent other users in your organization from accidentally deleting or modifying critical resources.</span></span> <span data-ttu-id="32bd0-282">További információ: [Erőforrások zárolása az Azure Resource Manager eszközzel](../azure-resource-manager/resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="32bd0-282">For more information, see [Lock resources with Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).</span></span>

### <a name="automation-script"></a><span data-ttu-id="32bd0-283">Automatizálási parancsfájl</span><span class="sxs-lookup"><span data-stu-id="32bd0-283">Automation script</span></span>

<span data-ttu-id="32bd0-284">Kattintson a **automatizálási parancsfájl** felépítéséhez és az üzembe helyezett erőforrások a későbbi telepítési sablon exportálása.</span><span class="sxs-lookup"><span data-stu-id="32bd0-284">Click **Automation script** to build and export a template of your deployed resources for future deployments.</span></span> <span data-ttu-id="32bd0-285">A sablonok használatának kapcsolatos további információkért lásd: [telepítése Azure Resource Manager-sablonok erőforrások](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="32bd0-285">For more information about working with templates, see [Deploy resources with Azure Resource Manager templates](../azure-resource-manager/resource-group-template-deploy.md).</span></span>

## <a name="administration-settings"></a><span data-ttu-id="32bd0-286">Felügyeleti beállítások</span><span class="sxs-lookup"><span data-stu-id="32bd0-286">Administration settings</span></span>
<span data-ttu-id="32bd0-287">A beállítások a **felügyeleti** szakasz lehetővé teszi a gyorsítótár a következő felügyeleti feladatokat hajthat végre.</span><span class="sxs-lookup"><span data-stu-id="32bd0-287">The settings in the **Administration** section allow you to perform the following administrative tasks for your cache.</span></span> 

![Adminisztráció](./media/cache-configure/redis-cache-administration.png)

* [<span data-ttu-id="32bd0-289">Adatok importálása</span><span class="sxs-lookup"><span data-stu-id="32bd0-289">Import data</span></span>](#importexport)
* [<span data-ttu-id="32bd0-290">Adatok exportálása</span><span class="sxs-lookup"><span data-stu-id="32bd0-290">Export data</span></span>](#importexport)
* [<span data-ttu-id="32bd0-291">Újraindítás</span><span class="sxs-lookup"><span data-stu-id="32bd0-291">Reboot</span></span>](#reboot)


### <a name="importexport"></a><span data-ttu-id="32bd0-292">Import/Export</span><span class="sxs-lookup"><span data-stu-id="32bd0-292">Import/Export</span></span>
<span data-ttu-id="32bd0-293">Importálási/exportálási egy Azure Redis Cache adatok felügyeleti művelet, amely lehetővé teszi a gyorsítótárban lévő adatok importálása és exportálása Redis gyorsítótár-adatbázis (Rekordadatbázis) pillanatkép prémium gyorsítótárában egy Azure Storage-fiók oldalakra vonatkozó blob exportálására és importálásra.</span><span class="sxs-lookup"><span data-stu-id="32bd0-293">Import/Export is an Azure Redis Cache data management operation, which allows you to import and export data in the cache by importing and exporting a Redis Cache Database (RDB) snapshot from a premium cache to a page blob in an Azure Storage Account.</span></span> <span data-ttu-id="32bd0-294">Importálási/exportálási lehetővé teszi különböző Azure Redis Cache-példányok közötti áttelepítése vagy a gyorsítótár adatokkal használat előtt.</span><span class="sxs-lookup"><span data-stu-id="32bd0-294">Import/Export enables you to migrate between different Azure Redis Cache instances or populate the cache with data before use.</span></span>

<span data-ttu-id="32bd0-295">Importálás a Redis-kiszolgáló futtatja a felhő vagy a környezet, beleértve a futó Linux, Windows vagy bármely felhőalapú szolgáltató, például az Amazon Web Services, míg mások Redis vinnie a Redis-kompatibilis Rekordadatbázis fájlok használható.</span><span class="sxs-lookup"><span data-stu-id="32bd0-295">Import can be used to bring Redis compatible RDB files from any Redis server running in any cloud or environment, including Redis running on Linux, Windows, or any cloud provider such as Amazon Web Services and others.</span></span> <span data-ttu-id="32bd0-296">Adatok importálása egyszerű módja a gyorsítótár létrehozásához előre megadott adatokkal.</span><span class="sxs-lookup"><span data-stu-id="32bd0-296">Importing data is an easy way to create a cache with pre-populated data.</span></span> <span data-ttu-id="32bd0-297">Az importálási folyamat során az Azure Redis Cache Rekordadatbázis fájlokat az Azure storage betölti a memóriába, és majd szúrja be a kulcsokat a gyorsítótárba.</span><span class="sxs-lookup"><span data-stu-id="32bd0-297">During the import process, Azure Redis Cache loads the RDB files from Azure storage into memory, and then inserts the keys into the cache.</span></span>

<span data-ttu-id="32bd0-298">Exportálás lehetővé teszi az Azure Redis Cache Redis kompatibilis Rekordadatbázis fájlok a tárolt adatok exportálását.</span><span class="sxs-lookup"><span data-stu-id="32bd0-298">Export allows you to export the data stored in Azure Redis Cache to Redis compatible RDB files.</span></span> <span data-ttu-id="32bd0-299">Ez a szolgáltatás segítségével tárolt adatok mozgatása egy Azure Redis Cache példányt, vagy egy másik Redis-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="32bd0-299">You can use this feature to move data from one Azure Redis Cache instance to another or to another Redis server.</span></span> <span data-ttu-id="32bd0-300">Az exportálási folyamat során egy ideiglenes fájl jön létre a virtuális Gépen, amelyen az Azure Redis Cache server-példányt, és a fájl feltöltése a kijelölt tárfiókkal.</span><span class="sxs-lookup"><span data-stu-id="32bd0-300">During the export process, a temporary file is created on the VM that hosts the Azure Redis Cache server instance, and the file is uploaded to the designated storage account.</span></span> <span data-ttu-id="32bd0-301">Az exportálási művelet befejezése után a vagy állapota sikeres végrehajtásával vagy hibajelzéssel az ideiglenes fájl törlődik.</span><span class="sxs-lookup"><span data-stu-id="32bd0-301">When the export operation completes with either a status of success or failure, the temporary file is deleted.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="32bd0-302">Prémium szintű réteghez gyorsítótárainak importálási/exportálási csak érhető el.</span><span class="sxs-lookup"><span data-stu-id="32bd0-302">Import/Export is only available for Premium tier caches.</span></span> <span data-ttu-id="32bd0-303">További információt és útmutatást lásd: [importálhat és exportálhat adatokat az Azure Redis Cache](cache-how-to-import-export-data.md).</span><span class="sxs-lookup"><span data-stu-id="32bd0-303">For more information and instructions, see [Import and Export data in Azure Redis Cache](cache-how-to-import-export-data.md).</span></span>
> 
> 

### <a name="reboot"></a><span data-ttu-id="32bd0-304">Újraindítás</span><span class="sxs-lookup"><span data-stu-id="32bd0-304">Reboot</span></span>
<span data-ttu-id="32bd0-305">A **újraindítás** panel lehetővé teszi, hogy a gyorsítótár a csomópontok újraindítását.</span><span class="sxs-lookup"><span data-stu-id="32bd0-305">The **Reboot** blade allows you to reboot the nodes of your cache.</span></span> <span data-ttu-id="32bd0-306">A rendszer újraindítása funkció lehetővé teszi a rugalmasságot az alkalmazás tesztelése, ha sikertelen. a gyorsítótár-csomópont.</span><span class="sxs-lookup"><span data-stu-id="32bd0-306">This reboot capability enables you to test your application for resiliency if there is a failure of a cache node.</span></span>

![Újraindítás](./media/cache-configure/redis-cache-reboot.png)

<span data-ttu-id="32bd0-308">Ha a prémium szintű gyorsítótár fürtözési engedélyezve van, mely szilánkok újraindítását a gyorsítótár választhatja meg.</span><span class="sxs-lookup"><span data-stu-id="32bd0-308">If you have a premium cache with clustering enabled, you can select which shards of the cache to reboot.</span></span>

![Újraindítás](./media/cache-configure/redis-cache-reboot-cluster.png)

<span data-ttu-id="32bd0-310">Indítsa újra a gyorsítótár egy vagy több csomópontot, válassza ki a kívánt csomópontokat, majd **újraindítás**.</span><span class="sxs-lookup"><span data-stu-id="32bd0-310">To reboot one or more nodes of your cache, select the desired nodes and click **Reboot**.</span></span> <span data-ttu-id="32bd0-311">Ha a prémium szintű gyorsítótár fürtözési engedélyezve van, jelölje ki a shard(s) újraindíthatja, és kattintson a **újraindítás**.</span><span class="sxs-lookup"><span data-stu-id="32bd0-311">If you have a premium cache with clustering enabled, select the shard(s) to reboot and then click **Reboot**.</span></span> <span data-ttu-id="32bd0-312">Néhány perc múlva a kijelölt csomópont újraindítása, és amelyeket biztonsági online néhány percen belül.</span><span class="sxs-lookup"><span data-stu-id="32bd0-312">After a few minutes, the selected node(s) reboot, and are back online a few minutes later.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="32bd0-313">Minden tarifacsomagok Újraindítás most érhető el.</span><span class="sxs-lookup"><span data-stu-id="32bd0-313">Reboot is now available for all pricing tiers.</span></span> <span data-ttu-id="32bd0-314">További információt és útmutatást lásd: [Azure Redis Cache felügyeleti - újraindítás](cache-administration.md#reboot).</span><span class="sxs-lookup"><span data-stu-id="32bd0-314">For more information and instructions, see [Azure Redis Cache administration - Reboot](cache-administration.md#reboot).</span></span>
> 
> 


## <a name="monitoring"></a><span data-ttu-id="32bd0-315">Figyelés</span><span class="sxs-lookup"><span data-stu-id="32bd0-315">Monitoring</span></span>

<span data-ttu-id="32bd0-316">A **figyelés** szakasz lehetővé teszi a diagnosztika és a Redis Cache-figyelés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="32bd0-316">The **Monitoring** section allows you to configure diagnostics and monitoring for your Redis Cache.</span></span> <span data-ttu-id="32bd0-317">Azure Redis Cache-figyelés és diagnosztika további információkért lásd: [figyelése Azure Redis Cache](cache-how-to-monitor.md).</span><span class="sxs-lookup"><span data-stu-id="32bd0-317">For more information on Azure Redis Cache monitoring and diagnostics, see [How to monitor Azure Redis Cache](cache-how-to-monitor.md).</span></span>

![Diagnosztika](./media/cache-configure/redis-cache-diagnostics.png)

* [<span data-ttu-id="32bd0-319">Redis metrikák</span><span class="sxs-lookup"><span data-stu-id="32bd0-319">Redis metrics</span></span>](#redis-metrics)
* [<span data-ttu-id="32bd0-320">A riasztási szabályok</span><span class="sxs-lookup"><span data-stu-id="32bd0-320">Alert rules</span></span>](#alert-rules)
* [<span data-ttu-id="32bd0-321">Diagnosztika</span><span class="sxs-lookup"><span data-stu-id="32bd0-321">Diagnostics</span></span>](#diagnostics)

### <a name="redis-metrics"></a><span data-ttu-id="32bd0-322">Redis metrikák</span><span class="sxs-lookup"><span data-stu-id="32bd0-322">Redis metrics</span></span>
<span data-ttu-id="32bd0-323">Kattintson a **metrikák Redis** való [metrikák megtekintése](cache-how-to-monitor.md#view-cache-metrics) a gyorsítótárhoz.</span><span class="sxs-lookup"><span data-stu-id="32bd0-323">Click **Redis metrics** to [view metrics](cache-how-to-monitor.md#view-cache-metrics) for your cache.</span></span>

### <a name="alert-rules"></a><span data-ttu-id="32bd0-324">A riasztási szabályok</span><span class="sxs-lookup"><span data-stu-id="32bd0-324">Alert rules</span></span>

<span data-ttu-id="32bd0-325">Kattintson a **riasztási szabályok** Redis Cache mérőszámok alapján értesítések konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="32bd0-325">Click **Alert rules** to configure alerts based on Redis Cache metrics.</span></span> <span data-ttu-id="32bd0-326">További információkért lásd: [riasztások](cache-how-to-monitor.md#alerts).</span><span class="sxs-lookup"><span data-stu-id="32bd0-326">For more information, see [Alerts](cache-how-to-monitor.md#alerts).</span></span>

### <a name="diagnostics"></a><span data-ttu-id="32bd0-327">Diagnosztika</span><span class="sxs-lookup"><span data-stu-id="32bd0-327">Diagnostics</span></span>

<span data-ttu-id="32bd0-328">Gyorsítótár-metrikát a Azure figyelő alapesetben [30 napig tárolja a](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#store-and-archive) , majd törölni.</span><span class="sxs-lookup"><span data-stu-id="32bd0-328">By default, cache metrics in Azure Monitor are [stored for 30 days](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#store-and-archive) and then deleted.</span></span> <span data-ttu-id="32bd0-329">Továbbra is fennáll, a 30 napnál hosszabb gyorsítótár metrikáját, kattintson a **diagnosztika** való [a tárfiók konfigurálása](cache-how-to-monitor.md#export-cache-metrics) gyorsítótár diagnosztika tárolására.</span><span class="sxs-lookup"><span data-stu-id="32bd0-329">To persist your cache metrics for longer than 30 days, click **Diagnostics** to [configure the storage account](cache-how-to-monitor.md#export-cache-metrics) used to store cache diagnostics.</span></span>

>[!NOTE]
><span data-ttu-id="32bd0-330">Mellett a gyorsítótár metrikákat Storage archiválás, is [adatfolyamként küldje el azokat az Eseményközpontok felé, vagy küldje el a Naplóelemzési](../monitoring-and-diagnostics/monitoring-overview-metrics.md#export-metrics).</span><span class="sxs-lookup"><span data-stu-id="32bd0-330">In addition to archiving your cache metrics to storage, you can also [stream them to an Event hub or send them to Log Analytics](../monitoring-and-diagnostics/monitoring-overview-metrics.md#export-metrics).</span></span>
>
>

## <a name="support--troubleshooting-settings"></a><span data-ttu-id="32bd0-331">Támogatási és hibaelhárítási beállítások</span><span class="sxs-lookup"><span data-stu-id="32bd0-331">Support & troubleshooting settings</span></span>
<span data-ttu-id="32bd0-332">A beállítások a **támogatási + hibaelhárítási** szakasz a gyorsítótár kapcsolatos problémák megoldása lehetőségeket biztosít.</span><span class="sxs-lookup"><span data-stu-id="32bd0-332">The settings in the **Support + troubleshooting** section provide you with options for resolving issues with your cache.</span></span>

![Támogatási + hibaelhárítása](./media/cache-configure/redis-cache-support-troubleshooting.png)

* [<span data-ttu-id="32bd0-334">Erőforrás állapota</span><span class="sxs-lookup"><span data-stu-id="32bd0-334">Resource health</span></span>](#resource-health)
* [<span data-ttu-id="32bd0-335">Új támogatási kérelem</span><span class="sxs-lookup"><span data-stu-id="32bd0-335">New support request</span></span>](#new-support-request)

### <a name="resource-health"></a><span data-ttu-id="32bd0-336">Erőforrás állapota</span><span class="sxs-lookup"><span data-stu-id="32bd0-336">Resource health</span></span>
<span data-ttu-id="32bd0-337">**Erőforrás állapota** az erőforrás figyeli, és jelzi, hogy ha a várt módon fut..</span><span class="sxs-lookup"><span data-stu-id="32bd0-337">**Resource health** watches your resource and tells you if it's running as expected.</span></span> <span data-ttu-id="32bd0-338">Az Azure Resource health szolgáltatással kapcsolatos további információkért lásd: [Azure-erőforrás állapotának áttekintése](../resource-health/resource-health-overview.md).</span><span class="sxs-lookup"><span data-stu-id="32bd0-338">For more information about the Azure Resource health service, see [Azure Resource health overview](../resource-health/resource-health-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="32bd0-339">Erőforrás állapota jelenleg nem készíthető jelentés a virtuális hálózat található Azure Redis Cache példány állapotának.</span><span class="sxs-lookup"><span data-stu-id="32bd0-339">Resource health is currently unable to report on the health of Azure Redis Cache instances hosted in a virtual network.</span></span> <span data-ttu-id="32bd0-340">További információkért lásd: [minden gyorsítótár-funkciók esetén egy virtuális hálózat a gyorsítótárhoz működik?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)</span><span class="sxs-lookup"><span data-stu-id="32bd0-340">For more information, see [Do all cache features work when hosting a cache in a VNET?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)</span></span>
> 
> 

### <a name="new-support-request"></a><span data-ttu-id="32bd0-341">Új támogatási kérelem</span><span class="sxs-lookup"><span data-stu-id="32bd0-341">New support request</span></span>
<span data-ttu-id="32bd0-342">Kattintson a **új támogatja a kérelem** a gyorsítótárhoz támogatási kérést nyithat.</span><span class="sxs-lookup"><span data-stu-id="32bd0-342">Click **New support request** to open a support request for your cache.</span></span>





## <a name="default-redis-server-configuration"></a><span data-ttu-id="32bd0-343">Alapértelmezett Redis-kiszolgálókonfiguráció</span><span class="sxs-lookup"><span data-stu-id="32bd0-343">Default Redis server configuration</span></span>
<span data-ttu-id="32bd0-344">Új Azure Redis Cache példányt az alábbi alapértelmezett értékekkel Redis konfigurációs vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="32bd0-344">New Azure Redis Cache instances are configured with the following default Redis configuration values.</span></span>

> [!NOTE]
> <span data-ttu-id="32bd0-345">Ebben a szakaszban a beállítások használatával nem módosítható a `StackExchange.Redis.IServer.ConfigSet` metódust.</span><span class="sxs-lookup"><span data-stu-id="32bd0-345">The settings in this section cannot be changed using the `StackExchange.Redis.IServer.ConfigSet` method.</span></span> <span data-ttu-id="32bd0-346">Ha ez a módszer egy ebben a szakaszban szereplő parancsok nevezik, az alábbihoz hasonló kivétel történik:</span><span class="sxs-lookup"><span data-stu-id="32bd0-346">If this method is called with one of the commands in this section, an exception similar to the following is thrown:</span></span>  
> 
> `StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
> 
> <span data-ttu-id="32bd0-347">Bármely értékek, amelyek konfigurálhatók, például **maximális memória-házirenddel**, az Azure portálon vagy a parancssori felügyeleti eszközök, például az Azure parancssori felület vagy a PowerShell segítségével konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="32bd0-347">Any values that are configurable, such as **max-memory-policy**, are configurable through the Azure portal or command-line management tools such as Azure CLI or PowerShell.</span></span>
> 
> 

| <span data-ttu-id="32bd0-348">Beállítás</span><span class="sxs-lookup"><span data-stu-id="32bd0-348">Setting</span></span> | <span data-ttu-id="32bd0-349">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="32bd0-349">Default value</span></span> | <span data-ttu-id="32bd0-350">Leírás</span><span class="sxs-lookup"><span data-stu-id="32bd0-350">Description</span></span> |
| --- | --- | --- |
| `databases` |<span data-ttu-id="32bd0-351">16</span><span class="sxs-lookup"><span data-stu-id="32bd0-351">16</span></span> |<span data-ttu-id="32bd0-352">Az adatbázis alapértelmezett érték 16, de be lehet állítani egy másik számot, a tarifacsomag alapján. <sup>1</sup> az alapértelmezett adatbázis DB 0, kiválaszthatja, hogy egy másik kapcsolati alapját használatával `connection.GetDatabase(dbid)` ahol `dbid` közötti szám `0` és `databases - 1`.</span><span class="sxs-lookup"><span data-stu-id="32bd0-352">The default number of databases is 16 but you can configure a different number based on the pricing tier.<sup>1</sup> The default database is DB 0, you can select a different one on a per-connection basis using `connection.GetDatabase(dbid)` where `dbid` is a number between `0` and `databases - 1`.</span></span> |
| `maxclients` |<span data-ttu-id="32bd0-353">Ez a tarifacsomag függ<sup>2</sup></span><span class="sxs-lookup"><span data-stu-id="32bd0-353">Depends on the pricing tier<sup>2</sup></span></span> |<span data-ttu-id="32bd0-354">Ez az egyidejűleg engedélyezett kapcsolódó ügyfelek maximális számát.</span><span class="sxs-lookup"><span data-stu-id="32bd0-354">This is the maximum number of connected clients allowed at the same time.</span></span> <span data-ttu-id="32bd0-355">A korlát elérését követően a Redis bezárul minden új kapcsolatot, és a "ügyfelek maximális száma elérte a" hiba újra.</span><span class="sxs-lookup"><span data-stu-id="32bd0-355">Once the limit is reached Redis closes all the new connections, returning a 'max number of clients reached' error.</span></span> |
| `maxmemory-policy` |`volatile-lru` |<span data-ttu-id="32bd0-356">Maxmemory házirend az beállítása a hogyan Redis választja ki, mit és mikor `maxmemory` (ajánlat választotta, a gyorsítótár létrehozása után a gyorsítótár méretét) elérésekor.</span><span class="sxs-lookup"><span data-stu-id="32bd0-356">Maxmemory policy is the setting for how Redis selects what to remove when `maxmemory` (the size of the cache offering you selected when you created the cache) is reached.</span></span> <span data-ttu-id="32bd0-357">Azure Redis Cache segítségével az alapértelmezett beállítás: `volatile-lru`, amely a kulcsok távolítja el a jelszólejárat LRU algoritmus használatával.</span><span class="sxs-lookup"><span data-stu-id="32bd0-357">With Azure Redis Cache the default setting is `volatile-lru`, which removes the keys with an expiration set using an LRU algorithm.</span></span> <span data-ttu-id="32bd0-358">Ezt a beállítást kell megadni az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="32bd0-358">This setting can be configured in the Azure portal.</span></span> <span data-ttu-id="32bd0-359">További információkért lásd: [memória házirendek](#memory-policies).</span><span class="sxs-lookup"><span data-stu-id="32bd0-359">For more information, see [Memory policies](#memory-policies).</span></span> |
| `maxmemory-samples` |<span data-ttu-id="32bd0-360">3</span><span class="sxs-lookup"><span data-stu-id="32bd0-360">3</span></span> |<span data-ttu-id="32bd0-361">A memóriahasználat LRU és minimális TTL algoritmusok közelítő algoritmusok pontos algoritmusok helyett.</span><span class="sxs-lookup"><span data-stu-id="32bd0-361">To save memory, LRU and minimal TTL algorithms are approximated algorithms instead of precise algorithms.</span></span> <span data-ttu-id="32bd0-362">Alapértelmezés szerint Redis ellenőrzések három kulcsok és kivételezések azt, amelyik kevesebb nemrég lett megadva.</span><span class="sxs-lookup"><span data-stu-id="32bd0-362">By default Redis checks three keys and picks the one that was used less recently.</span></span> |
| `lua-time-limit` |<span data-ttu-id="32bd0-363">5,000</span><span class="sxs-lookup"><span data-stu-id="32bd0-363">5,000</span></span> |<span data-ttu-id="32bd0-364">Maximális végrehajtási idő ezredmásodpercben Lua parancsfájlra.</span><span class="sxs-lookup"><span data-stu-id="32bd0-364">Max execution time of a Lua script in milliseconds.</span></span> <span data-ttu-id="32bd0-365">Ha eléri a maximális végrehajtási ideje, Redis naplózza, hogy egy parancsfájl még végrehajtása után a maximális engedélyezett idő, és hiba történt a lekérdezések megválaszolásához kezdődik.</span><span class="sxs-lookup"><span data-stu-id="32bd0-365">If the maximum execution time is reached, Redis logs that a script is still in execution after the maximum allowed time, and starts to reply to queries with an error.</span></span> |
| `lua-event-limit` |<span data-ttu-id="32bd0-366">500</span><span class="sxs-lookup"><span data-stu-id="32bd0-366">500</span></span> |<span data-ttu-id="32bd0-367">Parancsfájl esemény sor maximális mérete.</span><span class="sxs-lookup"><span data-stu-id="32bd0-367">Max size of script event queue.</span></span> |
| <span data-ttu-id="32bd0-368">`client-output-buffer-limit` `normalclient-output-buffer-limit` `pubsub`</span><span class="sxs-lookup"><span data-stu-id="32bd0-368">`client-output-buffer-limit` `normalclient-output-buffer-limit` `pubsub`</span></span> |<span data-ttu-id="32bd0-369">0 032mb 8mb 60 0</span><span class="sxs-lookup"><span data-stu-id="32bd0-369">0 0 032mb 8mb 60</span></span> |<span data-ttu-id="32bd0-370">Az ügyfél kimeneti puffer korlátok segítségével kényszerítheti az ügyfelek, amelyek nem adatainak olvasása elég gyors kiszolgálóról (gyakori oka az, hogy Pub/Sub ügyfél nem lehet felhasználni a lehető leghamarabb a közzétevő születik őket üzenetek) valamilyen okból megszakad.</span><span class="sxs-lookup"><span data-stu-id="32bd0-370">The client output buffer limits can be used to force disconnection of clients that are not reading data from the server fast enough for some reason (a common reason is that a Pub/Sub client can't consume messages as fast as the publisher can produce them).</span></span> <span data-ttu-id="32bd0-371">További információkért lásd: [http://redis.io/topics/clients](http://redis.io/topics/clients).</span><span class="sxs-lookup"><span data-stu-id="32bd0-371">For more information, see [http://redis.io/topics/clients](http://redis.io/topics/clients).</span></span> |

<span data-ttu-id="32bd0-372"><a name="databases"></a>
<sup>1</sup>vonatkozó korlát `databases` eltér az egyes Azure Redis Cache IP-címek és a gyorsítótár létrehozásakor állítható be.</span><span class="sxs-lookup"><span data-stu-id="32bd0-372"><a name="databases"></a>
<sup>1</sup>The limit for `databases` is different for each Azure Redis Cache pricing tier and can be set at cache creation.</span></span> <span data-ttu-id="32bd0-373">Ha nincs `databases` beállítás során megadott gyorsítótár létrehozását, az alapértelmezett érték 16.</span><span class="sxs-lookup"><span data-stu-id="32bd0-373">If no `databases` setting is specified during cache creation, the default is 16.</span></span>

* <span data-ttu-id="32bd0-374">Basic és Standard gyorsítótárak</span><span class="sxs-lookup"><span data-stu-id="32bd0-374">Basic and Standard caches</span></span>
  * <span data-ttu-id="32bd0-375">C0 csomag (250 MB) gyorsítótár - legfeljebb 16 adatbázisok</span><span class="sxs-lookup"><span data-stu-id="32bd0-375">C0 (250 MB) cache - up to 16 databases</span></span>
  * <span data-ttu-id="32bd0-376">C1 (1 GB-os) gyorsítótár - legfeljebb 16 adatbázisok</span><span class="sxs-lookup"><span data-stu-id="32bd0-376">C1 (1 GB) cache - up to 16 databases</span></span>
  * <span data-ttu-id="32bd0-377">C2 csomag (2,5 GB) gyorsítótár - legfeljebb 16 adatbázisok</span><span class="sxs-lookup"><span data-stu-id="32bd0-377">C2 (2.5 GB) cache - up to 16 databases</span></span>
  * <span data-ttu-id="32bd0-378">C3 csomag (6 GB) gyorsítótár - legfeljebb 16 adatbázisok</span><span class="sxs-lookup"><span data-stu-id="32bd0-378">C3 (6 GB) cache - up to 16 databases</span></span>
  * <span data-ttu-id="32bd0-379">C4 (13 GB) gyorsítótár - legfeljebb 32 adatbázisok</span><span class="sxs-lookup"><span data-stu-id="32bd0-379">C4 (13 GB) cache - up to 32 databases</span></span>
  * <span data-ttu-id="32bd0-380">C5 (26 GB) gyorsítótár - legfeljebb 48 adatbázisok</span><span class="sxs-lookup"><span data-stu-id="32bd0-380">C5 (26 GB) cache - up to 48 databases</span></span>
  * <span data-ttu-id="32bd0-381">C6 (53 GB) gyorsítótár - legfeljebb 64 adatbázisok</span><span class="sxs-lookup"><span data-stu-id="32bd0-381">C6 (53 GB) cache - up to 64 databases</span></span>
* <span data-ttu-id="32bd0-382">Prémium szintű gyorsítótárak</span><span class="sxs-lookup"><span data-stu-id="32bd0-382">Premium caches</span></span>
  * <span data-ttu-id="32bd0-383">Legfeljebb 16 adatbázisok (6 GB - 60 GB) – P1</span><span class="sxs-lookup"><span data-stu-id="32bd0-383">P1 (6 GB - 60 GB) - up to 16 databases</span></span>
  * <span data-ttu-id="32bd0-384">Legfeljebb 32 adatbázisok (13 GB - 130 GB) – P2</span><span class="sxs-lookup"><span data-stu-id="32bd0-384">P2 (13 GB - 130 GB) - up to 32 databases</span></span>
  * <span data-ttu-id="32bd0-385">P3 (26 GB - 260 GB) – akár 48 adatbázisok</span><span class="sxs-lookup"><span data-stu-id="32bd0-385">P3 (26 GB - 260 GB) - up to 48 databases</span></span>
  * <span data-ttu-id="32bd0-386">P4 (53 GB - 530 GB) – akár 64 adatbázisok</span><span class="sxs-lookup"><span data-stu-id="32bd0-386">P4 (53 GB - 530 GB) - up to 64 databases</span></span>
  * <span data-ttu-id="32bd0-387">Engedélyezve - Redis-fürt minden prémium gyorsítótárak Redis-fürt csak használatát támogatja az adatbázis 0, a `databases` korlátozza a prémium szintű gyorsítótár engedélyezve van a Redis-fürt hatékonyan értéke 1 és a [kiválasztása](http://redis.io/commands/select) parancs nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="32bd0-387">All premium caches with Redis cluster enabled - Redis cluster only supports use of database 0 so the `databases` limit for any premium cache with Redis cluster enabled is effectively 1 and the [Select](http://redis.io/commands/select) command is not allowed.</span></span> <span data-ttu-id="32bd0-388">További információkért lásd: [van saját ügyfélalkalmazás fürtszolgáltatással használandó végezze el a módosításokat?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)</span><span class="sxs-lookup"><span data-stu-id="32bd0-388">For more information, see [Do I need to make any changes to my client application to use clustering?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)</span></span>

<span data-ttu-id="32bd0-389">Adatbázisokkal kapcsolatos további információkért lásd: [Mik azok a Redis-adatbázisok?](cache-faq.md#what-are-redis-databases)</span><span class="sxs-lookup"><span data-stu-id="32bd0-389">For more information about databases, see [What are Redis databases?](cache-faq.md#what-are-redis-databases)</span></span>

> [!NOTE]
> <span data-ttu-id="32bd0-390">A `databases` beállítás csak a gyorsítótár létrehozása során konfigurált, és csak a PowerShell, a parancssori felületen vagy a más felügyeleti ügyfelek lehet.</span><span class="sxs-lookup"><span data-stu-id="32bd0-390">The `databases` setting can be configured only during cache creation and only using PowerShell, CLI, or other management clients.</span></span> <span data-ttu-id="32bd0-391">Példa konfigurálása `databases` gyorsítótár létrehozása PowerShell használatával, lásd: [New-AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).</span><span class="sxs-lookup"><span data-stu-id="32bd0-391">For an example of configuring `databases` during cache creation using PowerShell, see [New-AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).</span></span>
> 
> 

<span data-ttu-id="32bd0-392"><a name="maxclients"></a>
<sup>2</sup> `maxclients` eltér az egyes Azure Redis Cache tarifacsomagra vált.</span><span class="sxs-lookup"><span data-stu-id="32bd0-392"><a name="maxclients"></a>
<sup>2</sup>`maxclients` is different for each Azure Redis Cache pricing tier.</span></span>

* <span data-ttu-id="32bd0-393">Basic és Standard gyorsítótárak</span><span class="sxs-lookup"><span data-stu-id="32bd0-393">Basic and Standard caches</span></span>
  * <span data-ttu-id="32bd0-394">C0 csomag (250 MB) gyorsítótár - legfeljebb 256 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="32bd0-394">C0 (250 MB) cache - up to 256 connections</span></span>
  * <span data-ttu-id="32bd0-395">C1 (1 GB-os) gyorsítótár - legfeljebb 1000 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="32bd0-395">C1 (1 GB) cache - up to 1,000 connections</span></span>
  * <span data-ttu-id="32bd0-396">C2 csomag (2,5 GB) gyorsítótár - legfeljebb 2000 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="32bd0-396">C2 (2.5 GB) cache - up to 2,000 connections</span></span>
  * <span data-ttu-id="32bd0-397">C3 csomag (6 GB) gyorsítótár - legfeljebb 5000 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="32bd0-397">C3 (6 GB) cache - up to 5,000 connections</span></span>
  * <span data-ttu-id="32bd0-398">C4 (13 GB) gyorsítótár - legfeljebb 10 000 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="32bd0-398">C4 (13 GB) cache - up to 10,000 connections</span></span>
  * <span data-ttu-id="32bd0-399">C5 (26 GB) gyorsítótár - legfeljebb 15 000 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="32bd0-399">C5 (26 GB) cache - up to 15,000 connections</span></span>
  * <span data-ttu-id="32bd0-400">C6 (53 GB) gyorsítótár - legfeljebb 20 000 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="32bd0-400">C6 (53 GB) cache - up to 20,000 connections</span></span>
* <span data-ttu-id="32bd0-401">Prémium szintű gyorsítótárak</span><span class="sxs-lookup"><span data-stu-id="32bd0-401">Premium caches</span></span>
  * <span data-ttu-id="32bd0-402">P1 (6 GB - 60 GB) – akár 7500 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="32bd0-402">P1 (6 GB - 60 GB) - up to 7,500 connections</span></span>
  * <span data-ttu-id="32bd0-403">P2 (13 GB - 130 GB) – akár 15 000 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="32bd0-403">P2 (13 GB - 130 GB) - up to 15,000 connections</span></span>
  * <span data-ttu-id="32bd0-404">P3 (26 GB - 260 GB) – akár 30 000 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="32bd0-404">P3 (26 GB - 260 GB) - up to 30,000 connections</span></span>
  * <span data-ttu-id="32bd0-405">P4 (53 GB - 530 GB) – akár 40 000 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="32bd0-405">P4 (53 GB - 530 GB) - up to 40,000 connections</span></span>

> [!NOTE]
> <span data-ttu-id="32bd0-406">Miközben lehetővé teszi, hogy minden egyes gyorsítótár méretének *legfeljebb* bizonyos számú kapcsolatok, minden egyes való kapcsolat rendelkezik terhet társítva.</span><span class="sxs-lookup"><span data-stu-id="32bd0-406">While each size of cache allows *up to* a certain number of connections, each connection to Redis has overhead associated with it.</span></span> <span data-ttu-id="32bd0-407">Erre példa ilyen terheléssel lehet Processzor- és memóriahasználatról miatt a TLS/SSL-titkosítást.</span><span class="sxs-lookup"><span data-stu-id="32bd0-407">An example of such overhead would be CPU and memory usage as a result of TLS/SSL encryption.</span></span> <span data-ttu-id="32bd0-408">A megadott gyorsítótárméret maximális kapcsolódási korlátja azt feltételezi, hogy egy némileg betöltött gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="32bd0-408">The maximum connection limit for a given cache size assumes a lightly loaded cache.</span></span> <span data-ttu-id="32bd0-409">Ha tölthető be a kapcsolat terhelés *plus* az Ügyfélműveletek terhelése meghaladja a rendszer a kapacitás, a gyorsítótár is kapacitás problémákba ütközhetnek, még akkor is, ha nem lépik túl a gyorsítótár jelenlegi mérete a kapcsolathoz megadott korlátot.</span><span class="sxs-lookup"><span data-stu-id="32bd0-409">If load from connection overhead *plus* load from client operations exceeds capacity for the system, the cache can experience capacity issues even if you have not exceeded the connection limit for the current cache size.</span></span>
> 
> 



## <a name="redis-commands-not-supported-in-azure-redis-cache"></a><span data-ttu-id="32bd0-410">A parancsok nem támogatott az Azure Redis Cache redis</span><span class="sxs-lookup"><span data-stu-id="32bd0-410">Redis commands not supported in Azure Redis Cache</span></span>
> [!IMPORTANT]
> <span data-ttu-id="32bd0-411">Microsoft által felügyelt konfigurálása és kezelése az Azure Redis Cache példányt, mert a következő parancsok le vannak tiltva.</span><span class="sxs-lookup"><span data-stu-id="32bd0-411">Because configuration and management of Azure Redis Cache instances is managed by Microsoft, the following commands are disabled.</span></span> <span data-ttu-id="32bd0-412">Ha megpróbálja meghívni őket, hasonló hibaüzenetet kap `"(error) ERR unknown command"`.</span><span class="sxs-lookup"><span data-stu-id="32bd0-412">If you try to invoke them, you receive an error message similar to `"(error) ERR unknown command"`.</span></span>
> 
> * <span data-ttu-id="32bd0-413">BGREWRITEAOF</span><span class="sxs-lookup"><span data-stu-id="32bd0-413">BGREWRITEAOF</span></span>
> * <span data-ttu-id="32bd0-414">BGSAVE</span><span class="sxs-lookup"><span data-stu-id="32bd0-414">BGSAVE</span></span>
> * <span data-ttu-id="32bd0-415">CONFIG</span><span class="sxs-lookup"><span data-stu-id="32bd0-415">CONFIG</span></span>
> * <span data-ttu-id="32bd0-416">HIBAKERESÉSI</span><span class="sxs-lookup"><span data-stu-id="32bd0-416">DEBUG</span></span>
> * <span data-ttu-id="32bd0-417">ÁTTELEPÍTÉSE</span><span class="sxs-lookup"><span data-stu-id="32bd0-417">MIGRATE</span></span>
> * <span data-ttu-id="32bd0-418">MENTÉSE</span><span class="sxs-lookup"><span data-stu-id="32bd0-418">SAVE</span></span>
> * <span data-ttu-id="32bd0-419">LEÁLLÍTÁS</span><span class="sxs-lookup"><span data-stu-id="32bd0-419">SHUTDOWN</span></span>
> * <span data-ttu-id="32bd0-420">SLAVEOF</span><span class="sxs-lookup"><span data-stu-id="32bd0-420">SLAVEOF</span></span>
> * <span data-ttu-id="32bd0-421">FÜRT - fürt írási parancsokat le vannak tiltva, de csak olvasható fürt parancsot.</span><span class="sxs-lookup"><span data-stu-id="32bd0-421">CLUSTER - Cluster write commands are disabled, but read-only Cluster commands are permitted.</span></span>
> 
> 

<span data-ttu-id="32bd0-422">Redis parancsokkal kapcsolatos további információkért lásd: [http://redis.io/commands](http://redis.io/commands).</span><span class="sxs-lookup"><span data-stu-id="32bd0-422">For more information about Redis commands, see [http://redis.io/commands](http://redis.io/commands).</span></span>

## <a name="redis-console"></a><span data-ttu-id="32bd0-423">Redis-konzol</span><span class="sxs-lookup"><span data-stu-id="32bd0-423">Redis console</span></span>
<span data-ttu-id="32bd0-424">Az Azure Redis Cache példány használatával biztonságosan kiadhatja parancsok a **Redis konzol**, az Azure portálon, minden gyorsítótár rétege számára elérhető.</span><span class="sxs-lookup"><span data-stu-id="32bd0-424">You can securely issue commands to your Azure Redis Cache instances using the **Redis Console**, which is available in the Azure portal for all cache tiers.</span></span>

> [!IMPORTANT]
> - <span data-ttu-id="32bd0-425">A Redis-konzol nem tud együttműködni [VNET](cache-how-to-premium-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="32bd0-425">The Redis Console does not work with [VNET](cache-how-to-premium-vnet.md).</span></span> <span data-ttu-id="32bd0-426">Ha a gyorsítótár egy virtuális hálózat része, csak a virtuális hálózaton lévő ügyfelek is gyorsítótár-hozzáféréshez.</span><span class="sxs-lookup"><span data-stu-id="32bd0-426">When your cache is part of a VNET, only clients in the VNET can access the cache.</span></span> <span data-ttu-id="32bd0-427">Konzol Redis fut a helyi böngészőben, amely a virtuális hálózaton kívül, mert nem tud kapcsolódni a gyorsítótárhoz.</span><span class="sxs-lookup"><span data-stu-id="32bd0-427">Because Redis Console runs in your local browser, which is outside the VNET, it can't connect to your cache.</span></span>
> - <span data-ttu-id="32bd0-428">Nem minden Redis-parancsok az Azure Redis Cache támogatottak.</span><span class="sxs-lookup"><span data-stu-id="32bd0-428">Not all Redis commands are supported in Azure Redis Cache.</span></span> <span data-ttu-id="32bd0-429">Le vannak tiltva, az Azure Redis Cache Redis-parancsok listáját lásd az előző [parancsok nem támogatott az Azure Redis Cache Redis](#redis-commands-not-supported-in-azure-redis-cache) szakasz.</span><span class="sxs-lookup"><span data-stu-id="32bd0-429">For a list of Redis commands that are disabled for Azure Redis Cache, see the previous [Redis commands not supported in Azure Redis Cache](#redis-commands-not-supported-in-azure-redis-cache) section.</span></span> <span data-ttu-id="32bd0-430">Redis parancsokkal kapcsolatos további információkért lásd: [http://redis.io/commands](http://redis.io/commands).</span><span class="sxs-lookup"><span data-stu-id="32bd0-430">For more information about Redis commands, see [http://redis.io/commands](http://redis.io/commands).</span></span>
> 
> 

<span data-ttu-id="32bd0-431">A Redis-konzol megnyitásához kattintson a **konzol** a a **Redis Cache** panelen.</span><span class="sxs-lookup"><span data-stu-id="32bd0-431">To access the Redis Console, click **Console** from the **Redis Cache** blade.</span></span>

![Redis-konzol](./media/cache-configure/redis-console-menu.png)

<span data-ttu-id="32bd0-433">A gyorsítótárpéldány elleni parancsok kiadását, egyszerűen csak írja be a kívánt parancsot a konzolba.</span><span class="sxs-lookup"><span data-stu-id="32bd0-433">To issue commands against your cache instance, simply type the desired command into the console.</span></span>

![Redis-konzol](./media/cache-configure/redis-console.png)


### <a name="using-the-redis-console-with-a-premium-clustered-cache"></a><span data-ttu-id="32bd0-435">A Redis-konzol használata egy fürtözött prémium gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="32bd0-435">Using the Redis Console with a premium clustered cache</span></span>

<span data-ttu-id="32bd0-436">A Redis-konzol használata a prémium fürtözött gyorsítótár, ha a gyorsítótár egyetlen shard a parancsok adhat ki.</span><span class="sxs-lookup"><span data-stu-id="32bd0-436">When using the Redis Console with a premium clustered cache, you can issue commands to a single shard of the cache.</span></span> <span data-ttu-id="32bd0-437">A parancs kiadni egy adott shard, először kapcsolódnia kell a kívánt shard a shard kiválasztójának kattintva.</span><span class="sxs-lookup"><span data-stu-id="32bd0-437">To issue a command to a specific shard, first connect to the desired shard by clicking it on the shard picker.</span></span>

![Redis-konzol](./media/cache-configure/redis-console-premium-cluster.png)

<span data-ttu-id="32bd0-439">Ha megkísérli a csatlakoztatott shard mint egy másik shard tárolt hívóbetű, az alábbihoz hasonló hibaüzenetet kap.</span><span class="sxs-lookup"><span data-stu-id="32bd0-439">If you attempt to access a key that is stored in a different shard than the connected shard, you receive an error message similar to the following message.</span></span>

```
shard1>get myKey
(error) MOVED 866 13.90.202.154:13000 (shard 0)
```

<span data-ttu-id="32bd0-440">Az előző példában a shard 1 a kijelölt shard, de `myKey` található shard 0, ahogy azt a `(shard 0)` a hibaüzenet a következő része.</span><span class="sxs-lookup"><span data-stu-id="32bd0-440">In the previous example, shard 1 is the selected shard, but `myKey` is located in shard 0, as indicated by the `(shard 0)` portion of the error message.</span></span> <span data-ttu-id="32bd0-441">Ebben a példában eléréséhez `myKey`, és válassza ki a shard 0 a shard objektumválasztó használata, majd adja ki a kívánt parancsot.</span><span class="sxs-lookup"><span data-stu-id="32bd0-441">In this example, to access `myKey`, select shard 0 using the shard picker, and then issue the desired command.</span></span>


## <a name="move-your-cache-to-a-new-subscription"></a><span data-ttu-id="32bd0-442">Helyezze át a gyorsítótár egy új előfizetés</span><span class="sxs-lookup"><span data-stu-id="32bd0-442">Move your cache to a new subscription</span></span>
<span data-ttu-id="32bd0-443">Egy új előfizetést a gyorsítótár a gombra kattintva áthelyezheti **áthelyezése**.</span><span class="sxs-lookup"><span data-stu-id="32bd0-443">You can move your cache to a new subscription by clicking **Move**.</span></span>

![Helyezze át a Redis gyorsítótár](./media/cache-configure/redis-cache-move.png)

<span data-ttu-id="32bd0-445">Az erőforrások áthelyezése az egyik erőforráscsoportból a másikba, és a másik egy előfizetés információkért lásd: [erőforrások áthelyezése új erőforráscsoportba vagy előfizetésbe](../azure-resource-manager/resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="32bd0-445">For information on moving resources from one resource group to another, and from one subscription to another, see [Move resources to new resource group or subscription](../azure-resource-manager/resource-group-move-resources.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="32bd0-446">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="32bd0-446">Next steps</span></span>
* <span data-ttu-id="32bd0-447">További információ a Redis-parancsok használatával: [hogyan futtathatom Redis parancsok?](cache-faq.md#how-can-i-run-redis-commands)</span><span class="sxs-lookup"><span data-stu-id="32bd0-447">For more information on working with Redis commands, see [How can I run Redis commands?](cache-faq.md#how-can-i-run-redis-commands)</span></span>

