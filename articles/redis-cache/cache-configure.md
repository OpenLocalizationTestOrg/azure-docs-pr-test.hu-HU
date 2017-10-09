---
title: Azure Redis Cache aaaHow tooconfigure |} Microsoft Docs
description: "Az Azure Redis Cache hello alapértelmezett Redis konfigurációjának megértéséhez, valamint megtudhatja, hogyan tooconfigure az Azure Redis gyorsítótár-példányokon"
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
ms.openlocfilehash: 46bffb74cdf40e0e0a99c3a83dbe06d6fe1ea65b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-azure-redis-cache"></a><span data-ttu-id="c484b-103">Hogyan tooconfigure Azure Redis Cache-gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="c484b-103">How tooconfigure Azure Redis Cache</span></span>
<span data-ttu-id="c484b-104">Ez a témakör ismerteti, hogyan tooreview és frissítés hello konfigurációját az Azure Redis Cache példányt, és magában foglalja az hello alapértelmezett Redis kiszolgáló beállítása az Azure Redis Cache példány.</span><span class="sxs-lookup"><span data-stu-id="c484b-104">This topic describes how tooreview and update hello configuration for your Azure Redis Cache instances, and covers hello default Redis server configuration for Azure Redis Cache instances.</span></span>

> [!NOTE]
> <span data-ttu-id="c484b-105">Konfigurálásához és használatához prémium gyorsítótár-funkciók további információkért lásd: [hogyan tooconfigure adatmegőrzési](cache-how-to-premium-persistence.md), [hogyan fürtszolgáltatás tooconfigure](cache-how-to-premium-clustering.md), és [hogyan támogatják a virtuális hálózati tooconfigure ](cache-how-to-premium-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="c484b-105">For more information on configuring and using premium cache features, see [How tooconfigure persistence](cache-how-to-premium-persistence.md), [How tooconfigure clustering](cache-how-to-premium-clustering.md), and [How tooconfigure Virtual Network support](cache-how-to-premium-vnet.md).</span></span>
> 
> 

## <a name="configure-redis-cache-settings"></a><span data-ttu-id="c484b-106">Redis gyorsítótár beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c484b-106">Configure Redis cache settings</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

<span data-ttu-id="c484b-107">Az Azure Redis Cache-gyorsítótár beállításai vannak tekinthetők meg és hello konfigurált **Redis Cache** hello segítségével panel **erőforrás menü**.</span><span class="sxs-lookup"><span data-stu-id="c484b-107">Azure Redis Cache settings are viewed and configured on hello **Redis Cache** blade using hello **Resource Menu**.</span></span>

![Redis gyorsítótár beállításai](./media/cache-configure/redis-cache-settings.png)

<span data-ttu-id="c484b-109">Tekintheti meg és konfigurálja a következő beállításokat a hello hello **erőforrás menü**.</span><span class="sxs-lookup"><span data-stu-id="c484b-109">You can view and configure hello following settings using hello **Resource Menu**.</span></span>

* [<span data-ttu-id="c484b-110">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="c484b-110">Overview</span></span>](#overview)
* [<span data-ttu-id="c484b-111">Tevékenységnapló</span><span class="sxs-lookup"><span data-stu-id="c484b-111">Activity log</span></span>](#activity-log)
* [<span data-ttu-id="c484b-112">Hozzáférés-vezérlés (IAM)</span><span class="sxs-lookup"><span data-stu-id="c484b-112">Access control (IAM)</span></span>](#access-control-iam)
* [<span data-ttu-id="c484b-113">Címkék</span><span class="sxs-lookup"><span data-stu-id="c484b-113">Tags</span></span>](#tags)
* [<span data-ttu-id="c484b-114">Problémák diagnosztizálása és megoldása</span><span class="sxs-lookup"><span data-stu-id="c484b-114">Diagnose and solve problems</span></span>](#diagnose-and-solve-problems)
* [<span data-ttu-id="c484b-115">Beállítások</span><span class="sxs-lookup"><span data-stu-id="c484b-115">Settings</span></span>](#settings)
    * [<span data-ttu-id="c484b-116">Tárelérési kulcsok</span><span class="sxs-lookup"><span data-stu-id="c484b-116">Access keys</span></span>](#access-keys)
    * [<span data-ttu-id="c484b-117">Speciális beállítások</span><span class="sxs-lookup"><span data-stu-id="c484b-117">Advanced settings</span></span>](#advanced-settings)
    * [<span data-ttu-id="c484b-118">Redis gyorsítótár Advisor</span><span class="sxs-lookup"><span data-stu-id="c484b-118">Redis Cache Advisor</span></span>](#redis-cache-advisor)
    * [<span data-ttu-id="c484b-119">Méretezés</span><span class="sxs-lookup"><span data-stu-id="c484b-119">Scale</span></span>](#scale)
    * [<span data-ttu-id="c484b-120">Redis foglalásiegység-méret</span><span class="sxs-lookup"><span data-stu-id="c484b-120">Redis cluster size</span></span>](#cluster-size)
    * [<span data-ttu-id="c484b-121">Redis-adatmegőrzés</span><span class="sxs-lookup"><span data-stu-id="c484b-121">Redis data persistence</span></span>](#redis-data-persistence)
    * [<span data-ttu-id="c484b-122">Frissítések ütemezése</span><span class="sxs-lookup"><span data-stu-id="c484b-122">Schedule updates</span></span>](#schedule-updates)
    * [<span data-ttu-id="c484b-123">Georeplikáció</span><span class="sxs-lookup"><span data-stu-id="c484b-123">Geo-replication</span></span>](#geo-replication)
    * [<span data-ttu-id="c484b-124">Virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="c484b-124">Virtual Network</span></span>](#virtual-network)
    * [<span data-ttu-id="c484b-125">Tűzfal</span><span class="sxs-lookup"><span data-stu-id="c484b-125">Firewall</span></span>](#firewall)
    * [<span data-ttu-id="c484b-126">Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="c484b-126">Properties</span></span>](#properties)
    * [<span data-ttu-id="c484b-127">Zárolások feloldása</span><span class="sxs-lookup"><span data-stu-id="c484b-127">Locks</span></span>](#locks)
    * [<span data-ttu-id="c484b-128">Automatizálási parancsfájl</span><span class="sxs-lookup"><span data-stu-id="c484b-128">Automation script</span></span>](#automation-script)
* [<span data-ttu-id="c484b-129">Felügyeleti</span><span class="sxs-lookup"><span data-stu-id="c484b-129">Administration</span></span>](#administration)
    * [<span data-ttu-id="c484b-130">Adatok importálása</span><span class="sxs-lookup"><span data-stu-id="c484b-130">Import data</span></span>](#importexport)
    * [<span data-ttu-id="c484b-131">Adatok exportálása</span><span class="sxs-lookup"><span data-stu-id="c484b-131">Export data</span></span>](#importexport)
    * [<span data-ttu-id="c484b-132">Újraindítás</span><span class="sxs-lookup"><span data-stu-id="c484b-132">Reboot</span></span>](#reboot)
* [<span data-ttu-id="c484b-133">Figyelés</span><span class="sxs-lookup"><span data-stu-id="c484b-133">Monitoring</span></span>](#monitoring)
    * [<span data-ttu-id="c484b-134">Redis metrikák</span><span class="sxs-lookup"><span data-stu-id="c484b-134">Redis metrics</span></span>](#redis-metrics)
    * [<span data-ttu-id="c484b-135">A riasztási szabályok</span><span class="sxs-lookup"><span data-stu-id="c484b-135">Alert rules</span></span>](#alert-rules)
    * [<span data-ttu-id="c484b-136">Diagnosztika</span><span class="sxs-lookup"><span data-stu-id="c484b-136">Diagnostics</span></span>](#diagnostics)
* [<span data-ttu-id="c484b-137">Támogatási és hibaelhárítási beállítások</span><span class="sxs-lookup"><span data-stu-id="c484b-137">Support & troubleshooting settings</span></span>](#support-amp-troubleshooting-settings)
    * [<span data-ttu-id="c484b-138">Erőforrás állapota</span><span class="sxs-lookup"><span data-stu-id="c484b-138">Resource health</span></span>](#resource-health)
    * [<span data-ttu-id="c484b-139">Új támogatási kérelem</span><span class="sxs-lookup"><span data-stu-id="c484b-139">New support request</span></span>](#new-support-request)


## <a name="overview"></a><span data-ttu-id="c484b-140">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="c484b-140">Overview</span></span>

<span data-ttu-id="c484b-141">**Áttekintés** biztosít alapvető információkat, például nevét, a gyorsítótár, a portok, a tarifacsomag, és kijelölt gyorsítótár metrikákat.</span><span class="sxs-lookup"><span data-stu-id="c484b-141">**Overview** provides you with basic information about your cache, such as name, ports, pricing tier, and selected cache metrics.</span></span>

### <a name="activity-log"></a><span data-ttu-id="c484b-142">Tevékenységnapló</span><span class="sxs-lookup"><span data-stu-id="c484b-142">Activity log</span></span>

<span data-ttu-id="c484b-143">Kattintson a **tevékenységnapló** tooview műveleteket végezni a gyorsítótárhoz.</span><span class="sxs-lookup"><span data-stu-id="c484b-143">Click **Activity log** tooview actions performed on your cache.</span></span> <span data-ttu-id="c484b-144">Is használhatja szűrési tooexpand a nézet tooinclude más erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="c484b-144">You can also use filtering tooexpand this view tooinclude other resources.</span></span> <span data-ttu-id="c484b-145">A vizsgálati naplók munkáról bővebben lásd: [naplózási műveletek a Resource Manager](../azure-resource-manager/resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="c484b-145">For more information on working with audit logs, see [Audit operations with Resource Manager](../azure-resource-manager/resource-group-audit.md).</span></span> <span data-ttu-id="c484b-146">Azure Redis Cache események figyelésével kapcsolatos további információkért lásd: [műveletek és a riasztások](cache-how-to-monitor.md#operations-and-alerts).</span><span class="sxs-lookup"><span data-stu-id="c484b-146">For more information on monitoring Azure Redis Cache events, see [Operations and alerts](cache-how-to-monitor.md#operations-and-alerts).</span></span>

### <a name="access-control-iam"></a><span data-ttu-id="c484b-147">Hozzáférés-vezérlés (IAM)</span><span class="sxs-lookup"><span data-stu-id="c484b-147">Access control (IAM)</span></span>

<span data-ttu-id="c484b-148">Hello **hozzáférés-vezérlés (IAM)** szakasz támogatja a szerepköralapú hozzáférés-vezérlést (RBAC) hello Azure portál toohelp szervezetek megfelel az access management igényeik egyszerűen és pontosan.</span><span class="sxs-lookup"><span data-stu-id="c484b-148">hello **Access control (IAM)** section provides support for role-based access control (RBAC) in hello Azure portal toohelp organizations meet their access management requirements simply and precisely.</span></span> <span data-ttu-id="c484b-149">További információkért lásd: [hello Azure portál szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="c484b-149">For more information, see [Role-based access control in hello Azure portal](../active-directory/role-based-access-control-configure.md).</span></span>

### <a name="tags"></a><span data-ttu-id="c484b-150">Címkék</span><span class="sxs-lookup"><span data-stu-id="c484b-150">Tags</span></span>

<span data-ttu-id="c484b-151">Hello **címkék** szakasz segítséget nyújt az erőforrások rendszerezése.</span><span class="sxs-lookup"><span data-stu-id="c484b-151">hello **Tags** section helps you organize your resources.</span></span> <span data-ttu-id="c484b-152">További információkért lásd: [Using címkéket tooorganize az Azure-erőforrások](../azure-resource-manager/resource-group-using-tags.md).</span><span class="sxs-lookup"><span data-stu-id="c484b-152">For more information, see [Using tags tooorganize your Azure resources](../azure-resource-manager/resource-group-using-tags.md).</span></span>


### <a name="diagnose-and-solve-problems"></a><span data-ttu-id="c484b-153">Problémák diagnosztizálása és megoldása</span><span class="sxs-lookup"><span data-stu-id="c484b-153">Diagnose and solve problems</span></span>

<span data-ttu-id="c484b-154">Kattintson a **Diagnosztizálás és problémák megoldására** toobe a gyakori problémák és stratégiák előírt megoldása.</span><span class="sxs-lookup"><span data-stu-id="c484b-154">Click **Diagnose and solve problems** toobe provided with common issues and strategies for resolving them.</span></span>



## <a name="settings"></a><span data-ttu-id="c484b-155">Beállítások</span><span class="sxs-lookup"><span data-stu-id="c484b-155">Settings</span></span>
<span data-ttu-id="c484b-156">Hello **beállítások** szakasz lehetővé teszi a tooaccess, és a gyorsítótár beállításait a következő hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="c484b-156">hello **Settings** section allows you tooaccess and configure hello following settings for your cache.</span></span>

* [<span data-ttu-id="c484b-157">Tárelérési kulcsok</span><span class="sxs-lookup"><span data-stu-id="c484b-157">Access keys</span></span>](#access-keys)
* [<span data-ttu-id="c484b-158">Speciális beállítások</span><span class="sxs-lookup"><span data-stu-id="c484b-158">Advanced settings</span></span>](#advanced-settings)
* [<span data-ttu-id="c484b-159">Redis gyorsítótár Advisor</span><span class="sxs-lookup"><span data-stu-id="c484b-159">Redis Cache Advisor</span></span>](#redis-cache-advisor)
* [<span data-ttu-id="c484b-160">Méretezés</span><span class="sxs-lookup"><span data-stu-id="c484b-160">Scale</span></span>](#scale)
* [<span data-ttu-id="c484b-161">Redis foglalásiegység-méret</span><span class="sxs-lookup"><span data-stu-id="c484b-161">Redis cluster size</span></span>](#cluster-size)
* [<span data-ttu-id="c484b-162">Redis-adatmegőrzés</span><span class="sxs-lookup"><span data-stu-id="c484b-162">Redis data persistence</span></span>](#redis-data-persistence)
* [<span data-ttu-id="c484b-163">Frissítések ütemezése</span><span class="sxs-lookup"><span data-stu-id="c484b-163">Schedule updates</span></span>](#schedule-updates)
* [<span data-ttu-id="c484b-164">Georeplikáció</span><span class="sxs-lookup"><span data-stu-id="c484b-164">Geo-replication</span></span>](#geo-replication)
* [<span data-ttu-id="c484b-165">Virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="c484b-165">Virtual Network</span></span>](#virtual-network)
* [<span data-ttu-id="c484b-166">Tűzfal</span><span class="sxs-lookup"><span data-stu-id="c484b-166">Firewall</span></span>](#firewall)
* [<span data-ttu-id="c484b-167">Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="c484b-167">Properties</span></span>](#properties)
* [<span data-ttu-id="c484b-168">Zárolások feloldása</span><span class="sxs-lookup"><span data-stu-id="c484b-168">Locks</span></span>](#locks)
* [<span data-ttu-id="c484b-169">Automatizálási parancsfájl</span><span class="sxs-lookup"><span data-stu-id="c484b-169">Automation script</span></span>](#automation-script)



### <a name="access-keys"></a><span data-ttu-id="c484b-170">Elérési kulcs</span><span class="sxs-lookup"><span data-stu-id="c484b-170">Access keys</span></span>
<span data-ttu-id="c484b-171">Kattintson a **hívóbetűk** tooview vagy újragenerálása hello elérési kulcsainak a gyorsítótárhoz.</span><span class="sxs-lookup"><span data-stu-id="c484b-171">Click **Access keys** tooview or regenerate hello access keys for your cache.</span></span> <span data-ttu-id="c484b-172">Ezek a kulcsok tooyour gyorsítótár kapcsolódó hello ügyfelek által használt.</span><span class="sxs-lookup"><span data-stu-id="c484b-172">These keys are used by hello clients connecting tooyour cache.</span></span>

![Redis gyorsítótár elérési kulcsainak](./media/cache-configure/redis-cache-manage-keys.png)

### <a name="advanced-settings"></a><span data-ttu-id="c484b-174">Speciális beállítások</span><span class="sxs-lookup"><span data-stu-id="c484b-174">Advanced settings</span></span>
<span data-ttu-id="c484b-175">hello alábbi beállításainak konfigurálása a hello **speciális beállítások** panelen.</span><span class="sxs-lookup"><span data-stu-id="c484b-175">hello following settings are configured on hello **Advanced settings** blade.</span></span>

* [<span data-ttu-id="c484b-176">Hozzáférési portok</span><span class="sxs-lookup"><span data-stu-id="c484b-176">Access Ports</span></span>](#access-ports)
* [<span data-ttu-id="c484b-177">Memória házirendek</span><span class="sxs-lookup"><span data-stu-id="c484b-177">Memory policies</span></span>](#memory-policies)
* [<span data-ttu-id="c484b-178">Kulcstérértesítések használatával értesítések (Speciális beállítások)</span><span class="sxs-lookup"><span data-stu-id="c484b-178">Keyspace notifications (advanced settings)</span></span>](#keyspace-notifications-advanced-settings)

#### <a name="access-ports"></a><span data-ttu-id="c484b-179">Hozzáférési portok</span><span class="sxs-lookup"><span data-stu-id="c484b-179">Access Ports</span></span>
<span data-ttu-id="c484b-180">A nem SSL hozzáférés alapértelmezés szerint le van tiltva az új gyorsítótárakhoz.</span><span class="sxs-lookup"><span data-stu-id="c484b-180">By default, non-SSL access is disabled for new caches.</span></span> <span data-ttu-id="c484b-181">tooenable hello nem SSL port, kattintson a **nem** a **csak SSL keresztül hozzáférést** a hello **speciális beállítások** panel megnyitásához, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="c484b-181">tooenable hello non-SSL port, click **No** for **Allow access only via SSL** on hello **Advanced settings** blade and then click **Save**.</span></span>

![A redis Cache hozzáférési portok](./media/cache-configure/redis-cache-access-ports.png)

<a name="maxmemory-policy-and-maxmemory-reserved"></a>
#### <a name="memory-policies"></a><span data-ttu-id="c484b-183">Memória házirendek</span><span class="sxs-lookup"><span data-stu-id="c484b-183">Memory policies</span></span>
<span data-ttu-id="c484b-184">Hello **Maxmemory házirend**, **maxmemory fenntartott**, és **maxfragmentationmemory fenntartott** hello beállítások **speciális beállítások**panel hello memória házirendek hello gyorsítótár konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="c484b-184">hello **Maxmemory policy**, **maxmemory-reserved**, and **maxfragmentationmemory-reserved** settings on hello **Advanced settings** blade configure hello memory policies for hello cache.</span></span>

![Redis gyorsítótár Maxmemory házirend](./media/cache-configure/redis-cache-maxmemory-policy.png)

<span data-ttu-id="c484b-186">**Maxmemory házirend** hello kiürítés irányelvet hello gyorsítótár, és lehetővé teszi a következő kiürítési házirendek hello toochoose:</span><span class="sxs-lookup"><span data-stu-id="c484b-186">**Maxmemory policy** configures hello eviction policy for hello cache and allows you toochoose from hello following eviction policies:</span></span>

* <span data-ttu-id="c484b-187">`volatile-lru`-Ez az alapértelmezett hello.</span><span class="sxs-lookup"><span data-stu-id="c484b-187">`volatile-lru` - this is hello default.</span></span>
* `allkeys-lru`
* `volatile-random`
* `allkeys-random`
* `volatile-ttl`
* `noeviction`

<span data-ttu-id="c484b-188">További információ `maxmemory` házirendek, lásd: [kiürítés házirendek](http://redis.io/topics/lru-cache#eviction-policies).</span><span class="sxs-lookup"><span data-stu-id="c484b-188">For more information about `maxmemory` policies, see [Eviction policies](http://redis.io/topics/lru-cache#eviction-policies).</span></span>

<span data-ttu-id="c484b-189">Hello **maxmemory fenntartott** beállítással hello memóriamennyiség (MB), amely nem gyorsítótár műveletek, például a feladatátvétel során replikációs számára van fenntartva.</span><span class="sxs-lookup"><span data-stu-id="c484b-189">hello **maxmemory-reserved** setting configures hello amount of memory in MB that is reserved for non-cache operations such as replication during failover.</span></span> <span data-ttu-id="c484b-190">Ha az érték lehetővé teszi toohave Redis server egységesebb amikor változik a terhelés.</span><span class="sxs-lookup"><span data-stu-id="c484b-190">Setting this value allows you toohave a more consistent Redis server experience when your load varies.</span></span> <span data-ttu-id="c484b-191">Ez az érték nagyobb munkaterhelésekhez, amelyek írni a gyakori kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="c484b-191">This value should be set higher for workloads that are write heavy.</span></span> <span data-ttu-id="c484b-192">Amikor memória a műveletek számára van fenntartva, nem érhető el a gyorsítótárazott adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="c484b-192">When memory is reserved for such operations, it is unavailable for storage of cached data.</span></span>

<span data-ttu-id="c484b-193">Hello **maxfragmentationmemory fenntartott** beállítással hello memóriamennyiség, amely a memória töredezettségét fenntartott tooaccommodate MB-ban.</span><span class="sxs-lookup"><span data-stu-id="c484b-193">hello **maxfragmentationmemory-reserved** setting configures hello amount of memory in MB that is reserved tooaccommodate for memory fragmentation.</span></span> <span data-ttu-id="c484b-194">Ha az érték lehetővé teszi toohave Redis server egységesebb amikor hello gyorsítótár megtelt, vagy a Bezárás toofull és hello töredezettsége arány is nagy.</span><span class="sxs-lookup"><span data-stu-id="c484b-194">Setting this value allows you toohave a more consistent Redis server experience when hello cache is full or close toofull and hello fragmentation ratio is also high.</span></span> <span data-ttu-id="c484b-195">Amikor memória a műveletek számára van fenntartva, nem érhető el a gyorsítótárazott adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="c484b-195">When memory is reserved for such operations, it is unavailable for storage of cached data.</span></span>

<span data-ttu-id="c484b-196">Egy új foglalás memóriaméretnél kiválasztásakor egy dolog tooconsider (**maxmemory fenntartott** vagy **maxfragmentationmemory fenntartott**) milyen hatással van a módosítás az egy gyorsítótár, amely már fut. nagy mennyiségű adatot.</span><span class="sxs-lookup"><span data-stu-id="c484b-196">One thing tooconsider when choosing a new memory reservation value (**maxmemory-reserved** or **maxfragmentationmemory-reserved**) is how this change might affect a cache that is already running with large amounts of data in it.</span></span> <span data-ttu-id="c484b-197">Például ha 49 GB adatot 53 GB gyorsítótár rendelkezik, majd hello foglalási érték too8 GB, ezzel eldobja hello maximális memória hello rendszer too45 GB le.</span><span class="sxs-lookup"><span data-stu-id="c484b-197">For instance, if you have a 53 GB cache with 49 GB of data, then change hello reservation value too8 GB, this will drop hello max available memory for hello system down too45 GB.</span></span> <span data-ttu-id="c484b-198">Ha az aktuális `used_memory` vagy a `used_memory_rss` értékek magasabbak hello új 45 GB-os korlátját, majd hello rendszer tooevict adatokat fog rendelkezni, amíg `used_memory` és `used_memory_rss` 45 GB alatt van.</span><span class="sxs-lookup"><span data-stu-id="c484b-198">If either your current `used_memory` or your `used_memory_rss` values are higher than hello new limit of 45 GB, then hello system will have tooevict data until both `used_memory` and `used_memory_rss` are below 45 GB.</span></span> <span data-ttu-id="c484b-199">A kiürítési növelheti a kiszolgáló terhelés és a memória töredezettsége.</span><span class="sxs-lookup"><span data-stu-id="c484b-199">Eviction can increase server load and memory fragmentation.</span></span> <span data-ttu-id="c484b-200">További információt a gyorsítótár mérőszámokat például `used_memory` és `used_memory_rss`, lásd: [elérhető és a jelentéskészítés intervallumok](cache-how-to-monitor.md#available-metrics-and-reporting-intervals).</span><span class="sxs-lookup"><span data-stu-id="c484b-200">For more information on cache metrics such as `used_memory` and `used_memory_rss`, see [Available metrics and reporting intervals](cache-how-to-monitor.md#available-metrics-and-reporting-intervals).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c484b-201">Hello **maxmemory fenntartott** és **maxfragmentationmemory fenntartott** beállítások csak érhetők el a Standard és Premium gyorsítótárazza.</span><span class="sxs-lookup"><span data-stu-id="c484b-201">hello **maxmemory-reserved** and **maxfragmentationmemory-reserved** settings are only available for Standard and Premium caches.</span></span>
> 
> 

#### <a name="keyspace-notifications-advanced-settings"></a><span data-ttu-id="c484b-202">Kulcstérértesítések használatával értesítések (Speciális beállítások)</span><span class="sxs-lookup"><span data-stu-id="c484b-202">Keyspace notifications (advanced settings)</span></span>
<span data-ttu-id="c484b-203">Kulcstérértesítések használatával értesítések konfigurálása a hello redis **speciális beállítások** panelen.</span><span class="sxs-lookup"><span data-stu-id="c484b-203">Redis keyspace notifications are configured on hello **Advanced settings** blade.</span></span> <span data-ttu-id="c484b-204">Kulcstérértesítések használatával értesítések engedélyezése az ügyfelek tooreceive értesítések bizonyos események megtörténtekor.</span><span class="sxs-lookup"><span data-stu-id="c484b-204">Keyspace notifications allow clients tooreceive notifications when certain events occur.</span></span>

![Redis gyorsítótár speciális beállításai](./media/cache-configure/redis-cache-advanced-settings.png)

> [!IMPORTANT]
> <span data-ttu-id="c484b-206">Kulcstérértesítések használatával értesítéseket és hello **értesítés kulcstérértesítések használatával-események** beállítás csak akkor érhetők Standard és Premium gyorsítótárak esetében.</span><span class="sxs-lookup"><span data-stu-id="c484b-206">Keyspace notifications and hello **notify-keyspace-events** setting are only available for Standard and Premium caches.</span></span>
> 
> 

<span data-ttu-id="c484b-207">További információkért lásd: [Redis kulcstérértesítések használatával értesítések](http://redis.io/topics/notifications).</span><span class="sxs-lookup"><span data-stu-id="c484b-207">For more information, see [Redis Keyspace Notifications](http://redis.io/topics/notifications).</span></span> <span data-ttu-id="c484b-208">Mintakód, lásd: hello [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) hello fájlban [Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) minta.</span><span class="sxs-lookup"><span data-stu-id="c484b-208">For sample code, see hello [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) file in hello [Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) sample.</span></span>


<a name="recommendations"></a>
## <a name="redis-cache-advisor"></a><span data-ttu-id="c484b-209">Redis gyorsítótár Advisor</span><span class="sxs-lookup"><span data-stu-id="c484b-209">Redis Cache Advisor</span></span>
<span data-ttu-id="c484b-210">Hello **Redis gyorsítótár Advisor** csempe megjeleníti a gyorsítótár javaslatok.</span><span class="sxs-lookup"><span data-stu-id="c484b-210">hello **Redis Cache Advisor** blade displays recommendations for your cache.</span></span> <span data-ttu-id="c484b-211">A normál működés során nincsenek ajánlatok jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="c484b-211">During normal operations, no recommendations are displayed.</span></span> 

![Javaslatok](./media/cache-configure/redis-cache-no-recommendations.png)

<span data-ttu-id="c484b-213">Azokat a feltételeket a gyorsítótár például magas memóriahasználat, a hálózati sávszélesség vagy a kiszolgáló terhelését hello műveletek során fordul elő, ha egy riasztás jelenik meg hello **Redis Cache** panelen.</span><span class="sxs-lookup"><span data-stu-id="c484b-213">If any conditions occur during hello operations of your cache such as high memory usage, network bandwidth, or server load, an alert is displayed on hello **Redis Cache** blade.</span></span>

![Javaslatok](./media/cache-configure/redis-cache-recommendations-alert.png)

<span data-ttu-id="c484b-215">További információ a hello **javaslatok** panelen.</span><span class="sxs-lookup"><span data-stu-id="c484b-215">Further information can be found on hello **Recommendations** blade.</span></span>

![Javaslatok](./media/cache-configure/redis-cache-recommendations.png)

<span data-ttu-id="c484b-217">A metrikák hello a figyelheti [diagramok figyelési](cache-how-to-monitor.md#monitoring-charts) és [használati diagramok](cache-how-to-monitor.md#usage-charts) hello szakasza **Redis Cache** panelen.</span><span class="sxs-lookup"><span data-stu-id="c484b-217">You can monitor these metrics on hello [Monitoring charts](cache-how-to-monitor.md#monitoring-charts) and [Usage charts](cache-how-to-monitor.md#usage-charts) sections of hello **Redis Cache** blade.</span></span>

<span data-ttu-id="c484b-218">Minden tarifacsomag különböző korlátai ügyfélkapcsolatokat, a memória és a sávszélesség rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c484b-218">Each pricing tier has different limits for client connections, memory, and bandwidth.</span></span> <span data-ttu-id="c484b-219">Ha a gyorsítótár maximális kapacitás metrikákat megközelíti egy huzamosabb ideig keresztül, az ajánlás jön létre.</span><span class="sxs-lookup"><span data-stu-id="c484b-219">If your cache approaches maximum capacity for these metrics over a sustained period of time, a recommendation is created.</span></span> <span data-ttu-id="c484b-220">Hello metrikák és korlátai hello vizsgálja felül további információt **javaslatok** eszköz, tekintse meg a következő táblázat hello:</span><span class="sxs-lookup"><span data-stu-id="c484b-220">For more information about hello metrics and limits reviewed by hello **Recommendations** tool, see hello following table:</span></span>

| <span data-ttu-id="c484b-221">Redis gyorsítótár metrika</span><span class="sxs-lookup"><span data-stu-id="c484b-221">Redis Cache metric</span></span> | <span data-ttu-id="c484b-222">További információ</span><span class="sxs-lookup"><span data-stu-id="c484b-222">More information</span></span> |
| --- | --- |
| <span data-ttu-id="c484b-223">A sávszélesség-használat</span><span class="sxs-lookup"><span data-stu-id="c484b-223">Network bandwidth usage</span></span> |[<span data-ttu-id="c484b-224">Gyorsítótár teljesítmény - rendelkezésre álló sávszélesség</span><span class="sxs-lookup"><span data-stu-id="c484b-224">Cache performance - available bandwidth</span></span>](cache-faq.md#cache-performance) |
| <span data-ttu-id="c484b-225">Csatlakozott ügyfelek</span><span class="sxs-lookup"><span data-stu-id="c484b-225">Connected clients</span></span> |[<span data-ttu-id="c484b-226">Alapértelmezett Redis-kiszolgáló konfiguráció - maxclients</span><span class="sxs-lookup"><span data-stu-id="c484b-226">Default Redis server configuration - maxclients</span></span>](#maxclients) |
| <span data-ttu-id="c484b-227">A kiszolgálóterhelés</span><span class="sxs-lookup"><span data-stu-id="c484b-227">Server load</span></span> |[<span data-ttu-id="c484b-228">Használati diagramok - Redis-kiszolgáló terhelését</span><span class="sxs-lookup"><span data-stu-id="c484b-228">Usage charts - Redis Server Load</span></span>](cache-how-to-monitor.md#usage-charts) |
| <span data-ttu-id="c484b-229">Memóriahasználat</span><span class="sxs-lookup"><span data-stu-id="c484b-229">Memory usage</span></span> |[<span data-ttu-id="c484b-230">Gyorsítótár teljesítmény - mérete</span><span class="sxs-lookup"><span data-stu-id="c484b-230">Cache performance - size</span></span>](cache-faq.md#cache-performance) |

<span data-ttu-id="c484b-231">tooupgrade a gyorsítótár kattintson **frissítés most** toochange hello IP-címek és [méretezési](#scale) a gyorsítótárhoz.</span><span class="sxs-lookup"><span data-stu-id="c484b-231">tooupgrade your cache, click **Upgrade now** toochange hello pricing tier and [scale](#scale) your cache.</span></span> <span data-ttu-id="c484b-232">Tarifacsomag kiválasztásáról további információkért lásd: [milyen Redis Cache-ajánlatot és méretet használjam?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)</span><span class="sxs-lookup"><span data-stu-id="c484b-232">For more information on choosing a pricing tier, see [What Redis Cache offering and size should I use?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)</span></span>


### <a name="scale"></a><span data-ttu-id="c484b-233">Méretezés</span><span class="sxs-lookup"><span data-stu-id="c484b-233">Scale</span></span>
<span data-ttu-id="c484b-234">Kattintson a **méretezési** tooview, vagy módosítsa hello IP-címek a gyorsítótárhoz.</span><span class="sxs-lookup"><span data-stu-id="c484b-234">Click **Scale** tooview or change hello pricing tier for your cache.</span></span> <span data-ttu-id="c484b-235">A méretezés további információkért lásd: [hogyan tooScale Azure Redis Cache-gyorsítótár](cache-how-to-scale.md).</span><span class="sxs-lookup"><span data-stu-id="c484b-235">For more information on scaling, see [How tooScale Azure Redis Cache](cache-how-to-scale.md).</span></span>

![Redis gyorsítótár tarifacsomag](./media/cache-configure/pricing-tier.png)

<a name="cluster-size"></a>

### <a name="redis-cluster-size"></a><span data-ttu-id="c484b-237">Redis foglalásiegység-méret</span><span class="sxs-lookup"><span data-stu-id="c484b-237">Redis Cluster Size</span></span>
<span data-ttu-id="c484b-238">Kattintson a **(előzetes verzió) Redis fürtméret** toochange hello fürtméret futó Premium gyorsítótár fürtözési engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="c484b-238">Click **(PREVIEW) Redis Cluster Size** toochange hello cluster size for a running premium cache with clustering enabled.</span></span>

> [!NOTE]
> <span data-ttu-id="c484b-239">Vegye figyelembe, hogy közben hello Azure Redis Cache prémium szint lett tooGeneral rendelkezésre állás érdekében hello fürtméret Redis funkció, amely jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="c484b-239">Note that while hello Azure Redis Cache Premium tier has been released tooGeneral Availability, hello Redis Cluster Size feature is currently in preview.</span></span>
> 
> 

![Redis foglalásiegység-méret](./media/cache-configure/redis-cache-redis-cluster-size.png)

<span data-ttu-id="c484b-241">toochange hello fürtméret hello csúszkával, vagy írjon be egy számot 1 és 10 közötti hello **Shard száma** szöveges értéket, majd kattintson **OK** toosave.</span><span class="sxs-lookup"><span data-stu-id="c484b-241">toochange hello cluster size, use hello slider or type a number between 1 and 10 in hello **Shard count** text box and click **OK** toosave.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c484b-242">Redis fürtszolgáltatás csak érhető el prémium gyorsítótárak esetében.</span><span class="sxs-lookup"><span data-stu-id="c484b-242">Redis clustering is only available for Premium caches.</span></span> <span data-ttu-id="c484b-243">További információkért lásd: [hogyan fürtözése a Premium Azure Redis Cache tooconfigure](cache-how-to-premium-clustering.md).</span><span class="sxs-lookup"><span data-stu-id="c484b-243">For more information, see [How tooconfigure clustering for a Premium Azure Redis Cache](cache-how-to-premium-clustering.md).</span></span>
> 
> 


### <a name="redis-data-persistence"></a><span data-ttu-id="c484b-244">Redis-adatmegőrzés</span><span class="sxs-lookup"><span data-stu-id="c484b-244">Redis data persistence</span></span>
<span data-ttu-id="c484b-245">Kattintson a **Redis-adatmegőrzés** tooenable, letiltása, vagy konfigurálja a prémium szintű gyorsítótár adatainak megőrzését.</span><span class="sxs-lookup"><span data-stu-id="c484b-245">Click **Redis data persistence** tooenable, disable, or configure data persistence for your premium cache.</span></span> <span data-ttu-id="c484b-246">Azure Redis Cache Redis-adatmegőrzés használatával biztosít [Rekordadatbázis adatmegőrzési](cache-how-to-premium-persistence.md#configure-rdb-persistence) vagy [AOF adatmegőrzési](cache-how-to-premium-persistence.md#configure-aof-persistence).</span><span class="sxs-lookup"><span data-stu-id="c484b-246">Azure Redis Cache offers Redis persistence using either [RDB persistence](cache-how-to-premium-persistence.md#configure-rdb-persistence) or [AOF persistence](cache-how-to-premium-persistence.md#configure-aof-persistence).</span></span>

<span data-ttu-id="c484b-247">További információkért lásd: [hogyan tooconfigure megőrzését egy prémium szintű Azure Redis Cache](cache-how-to-premium-persistence.md).</span><span class="sxs-lookup"><span data-stu-id="c484b-247">For more information, see [How tooconfigure persistence for a Premium Azure Redis Cache](cache-how-to-premium-persistence.md).</span></span>


> [!IMPORTANT]
> <span data-ttu-id="c484b-248">Prémium szintű gyorsítótárak redis-adatmegőrzés csak érhető el.</span><span class="sxs-lookup"><span data-stu-id="c484b-248">Redis data persistence is only available for Premium caches.</span></span> 
> 
> 

### <a name="schedule-updates"></a><span data-ttu-id="c484b-249">Frissítések ütemezése</span><span class="sxs-lookup"><span data-stu-id="c484b-249">Schedule updates</span></span>
<span data-ttu-id="c484b-250">Hello **frissítések ütemezése** panel lehetővé teszi a toodesignate Redis server frissítései, a gyorsítótár egy karbantartási időszakot.</span><span class="sxs-lookup"><span data-stu-id="c484b-250">hello **Schedule updates** blade allows you toodesignate a maintenance window for Redis server updates for your cache.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="c484b-251">hello karbantartási időszak vonatkozzon, csak tooRedis kiszolgáló, és nem tooany Azure frissíti, vagy toohello hello hello gyorsítótár üzemeltető virtuális gépek operációs rendszerének frissítése.</span><span class="sxs-lookup"><span data-stu-id="c484b-251">hello maintenance window applies only tooRedis server updates, and not tooany Azure updates or updates toohello operating system of hello VMs that host hello cache.</span></span>
> 
> 

![Frissítések ütemezése](./media/cache-configure/redis-schedule-updates.png)

<span data-ttu-id="c484b-253">toospecify karbantartási időszak, ellenőrizze a szükségeskonfiguráció-hello nap és hello karbantartási időszak kezdő időpontja minden nap adja meg, majd kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="c484b-253">toospecify a maintenance window, check hello desired days and specify hello maintenance window start hour for each day, and click **OK**.</span></span> <span data-ttu-id="c484b-254">Vegye figyelembe, hogy hello karbantartási ablak időpontja UTC szerint.</span><span class="sxs-lookup"><span data-stu-id="c484b-254">Note that hello maintenance window time is in UTC.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="c484b-255">Hello **frissítések ütemezése** funkciót csak érhető el prémium réteghez gyorsítótárainak.</span><span class="sxs-lookup"><span data-stu-id="c484b-255">hello **Schedule updates** functionality is only available for Premium tier caches.</span></span> <span data-ttu-id="c484b-256">További információt és útmutatást lásd: [Azure Redis Cache felügyeleti - frissítések ütemezése](cache-administration.md#schedule-updates).</span><span class="sxs-lookup"><span data-stu-id="c484b-256">For more information and instructions, see [Azure Redis Cache administration - Schedule updates](cache-administration.md#schedule-updates).</span></span>
> 
> 

### <a name="geo-replication"></a><span data-ttu-id="c484b-257">Georeplikáció</span><span class="sxs-lookup"><span data-stu-id="c484b-257">Geo-replication</span></span>

<span data-ttu-id="c484b-258">Hello **georeplikáció** panel lehetővé teszi a csatolás két Premium szint Azure Redis Cache példányt.</span><span class="sxs-lookup"><span data-stu-id="c484b-258">hello **Geo-replication** blade provides a mechanism for linking two Premium tier Azure Redis Cache instances.</span></span> <span data-ttu-id="c484b-259">Egy gyorsítótár hello elsődleges csatolt gyorsítótárat és más, mint hello másodlagos csatolt gyorsítótár hello van kijelölve.</span><span class="sxs-lookup"><span data-stu-id="c484b-259">One cache is designated as hello primary linked cache, and hello other as hello secondary linked cache.</span></span> <span data-ttu-id="c484b-260">hello másodlagos csatolt gyorsítótár csak olvashatóvá válik, és adatok írásbeli toohello elsődleges gyorsítótár toohello másodlagos csatolt gyorsítótár replikálva.</span><span class="sxs-lookup"><span data-stu-id="c484b-260">hello secondary linked cache becomes read-only, and data written toohello primary cache is replicated toohello secondary linked cache.</span></span> <span data-ttu-id="c484b-261">Ez a funkció használt tooreplicate a gyorsítótár Azure-régiók közötti lehet.</span><span class="sxs-lookup"><span data-stu-id="c484b-261">This functionality can be used tooreplicate a cache across Azure regions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c484b-262">**A georeplikáció** lehetőség csak a prémium szintű réteghez gyorsítótárainak.</span><span class="sxs-lookup"><span data-stu-id="c484b-262">**Geo-replication** is only available for Premium tier caches.</span></span> <span data-ttu-id="c484b-263">További információt és útmutatást lásd: [hogyan tooconfigure georeplikáció az Azure Redis Cache](cache-how-to-geo-replication.md).</span><span class="sxs-lookup"><span data-stu-id="c484b-263">For more information and instructions, see [How tooconfigure Geo-replication for Azure Redis Cache](cache-how-to-geo-replication.md).</span></span>
> 
> 

### <a name="virtual-network"></a><span data-ttu-id="c484b-264">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="c484b-264">Virtual Network</span></span>
<span data-ttu-id="c484b-265">Hello **virtuális hálózati** szakasz lehetővé teszi a gyorsítótár tooconfigure hello virtuális hálózati beállításait.</span><span class="sxs-lookup"><span data-stu-id="c484b-265">hello **Virtual Network** section allows you tooconfigure hello virtual network settings for your cache.</span></span> <span data-ttu-id="c484b-266">A prémium szintű gyorsítótár virtuális hálózaton létrehozásával kapcsolatos információkat támogatja, és a beállítások frissítése, lásd: [hogyan tooconfigure a virtuális hálózati támogatása a Premium Azure Redis Cache](cache-how-to-premium-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="c484b-266">For information on creating a premium cache with VNET support and updating its settings, see [How tooconfigure Virtual Network Support for a Premium Azure Redis Cache](cache-how-to-premium-vnet.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c484b-267">Virtuális hálózati beállítások csak érhetők el a prémium szintű gyorsítótárak konfigurált VNET támogató gyorsítótár létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="c484b-267">Virtual network settings are only available for premium caches that were configured with VNET support during cache creation.</span></span> 
> 
> 

### <a name="firewall"></a><span data-ttu-id="c484b-268">Tűzfal</span><span class="sxs-lookup"><span data-stu-id="c484b-268">Firewall</span></span>

<span data-ttu-id="c484b-269">Kattintson a **tűzfal** tooview és tűzfalszabályok beállítása a prémium szintű Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="c484b-269">Click **Firewall** tooview and configure firewall rules for your Premium Azure Redis Cache.</span></span>

![Tűzfal](./media/cache-configure/redis-firewall-rules.png)

<span data-ttu-id="c484b-271">A tűzfalszabályok kezdő és záró IP-címtartománnyal rendelkező adhatja meg.</span><span class="sxs-lookup"><span data-stu-id="c484b-271">You can specify firewall rules with a start and end IP address range.</span></span> <span data-ttu-id="c484b-272">Tűzfalszabályok konfigurálásakor csak hello érkező ügyfélkapcsolatokat megadott, az IP-címtartományok toohello gyorsítótár kapcsolódhatnak.</span><span class="sxs-lookup"><span data-stu-id="c484b-272">When firewall rules are configured, only client connections from hello specified IP address ranges can connect toohello cache.</span></span> <span data-ttu-id="c484b-273">Egy tűzfalszabály mentésekor a rendszer nincs rövid késleltetés előtt hello szabály érvényben.</span><span class="sxs-lookup"><span data-stu-id="c484b-273">When a firewall rule is saved there is a short delay before hello rule is effective.</span></span> <span data-ttu-id="c484b-274">Ez a késés van általában kevesebb mint egy perc.</span><span class="sxs-lookup"><span data-stu-id="c484b-274">This delay is typically less than one minute.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c484b-275">Azure Redis Cache rendszerek figyelése a kapcsolatok mindig engedélyezettek, még akkor is, ha a tűzfal-szabályok úgy vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="c484b-275">Connections from Azure Redis Cache monitoring systems are always permitted, even if firewall rules are configured.</span></span>
> 
> <span data-ttu-id="c484b-276">Tűzfalszabályok esetén csak prémium szintű réteghez gyorsítótárainak érhetők el.</span><span class="sxs-lookup"><span data-stu-id="c484b-276">Firewall rules are only available for Premium tier caches.</span></span>
> 
> 

### <a name="properties"></a><span data-ttu-id="c484b-277">Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="c484b-277">Properties</span></span>
<span data-ttu-id="c484b-278">Kattintson a **tulajdonságok** tooview információt a gyorsítótár, beleértve a gyorsítótár végpontjához hello és portok.</span><span class="sxs-lookup"><span data-stu-id="c484b-278">Click **Properties** tooview information about your cache, including hello cache endpoint and ports.</span></span>

![Redis gyorsítótár tulajdonságai](./media/cache-configure/redis-cache-properties.png)

### <a name="locks"></a><span data-ttu-id="c484b-280">Zárolások</span><span class="sxs-lookup"><span data-stu-id="c484b-280">Locks</span></span>
<span data-ttu-id="c484b-281">Hello **zárolja** szakasz lehetővé teszi a toolock előfizetés, erőforrás vagy az erőforrás tooprevent véletlen törlése vagy a kritikus erőforrásokat módosítása a munkahely más felhasználóinak.</span><span class="sxs-lookup"><span data-stu-id="c484b-281">hello **Locks** section allows you toolock a subscription, resource group, or resource tooprevent other users in your organization from accidentally deleting or modifying critical resources.</span></span> <span data-ttu-id="c484b-282">További információ: [Erőforrások zárolása az Azure Resource Manager eszközzel](../azure-resource-manager/resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="c484b-282">For more information, see [Lock resources with Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).</span></span>

### <a name="automation-script"></a><span data-ttu-id="c484b-283">Automatizálási parancsfájl</span><span class="sxs-lookup"><span data-stu-id="c484b-283">Automation script</span></span>

<span data-ttu-id="c484b-284">Kattintson a **automatizálási parancsfájl** toobuild és a telepített erőforrások a későbbi telepítési sablon exportálása.</span><span class="sxs-lookup"><span data-stu-id="c484b-284">Click **Automation script** toobuild and export a template of your deployed resources for future deployments.</span></span> <span data-ttu-id="c484b-285">A sablonok használatának kapcsolatos további információkért lásd: [telepítése Azure Resource Manager-sablonok erőforrások](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="c484b-285">For more information about working with templates, see [Deploy resources with Azure Resource Manager templates](../azure-resource-manager/resource-group-template-deploy.md).</span></span>

## <a name="administration-settings"></a><span data-ttu-id="c484b-286">Felügyeleti beállítások</span><span class="sxs-lookup"><span data-stu-id="c484b-286">Administration settings</span></span>
<span data-ttu-id="c484b-287">hello szolgáltatásbeállításai hello **felügyeleti** szakasz lehetővé teszi a következő felügyeleti feladatok a gyorsítótárhoz tooperform hello.</span><span class="sxs-lookup"><span data-stu-id="c484b-287">hello settings in hello **Administration** section allow you tooperform hello following administrative tasks for your cache.</span></span> 

![Adminisztráció](./media/cache-configure/redis-cache-administration.png)

* [<span data-ttu-id="c484b-289">Adatok importálása</span><span class="sxs-lookup"><span data-stu-id="c484b-289">Import data</span></span>](#importexport)
* [<span data-ttu-id="c484b-290">Adatok exportálása</span><span class="sxs-lookup"><span data-stu-id="c484b-290">Export data</span></span>](#importexport)
* [<span data-ttu-id="c484b-291">Újraindítás</span><span class="sxs-lookup"><span data-stu-id="c484b-291">Reboot</span></span>](#reboot)


### <a name="importexport"></a><span data-ttu-id="c484b-292">Import/Export</span><span class="sxs-lookup"><span data-stu-id="c484b-292">Import/Export</span></span>
<span data-ttu-id="c484b-293">Importálási/exportálási során Azure Redis Cache adatok felügyeleti, amely lehetővé teszi által importálás és exportálás a Redis Cache-adatbázis (Rekordadatbázis) pillanatkép prémium gyorsítótár tooa oldalakra vonatkozó blob egy Azure Storage-fiók hello gyorsítótár tooimport és exportálási adatokat.</span><span class="sxs-lookup"><span data-stu-id="c484b-293">Import/Export is an Azure Redis Cache data management operation, which allows you tooimport and export data in hello cache by importing and exporting a Redis Cache Database (RDB) snapshot from a premium cache tooa page blob in an Azure Storage Account.</span></span> <span data-ttu-id="c484b-294">Importálási/exportálási lehetővé teszi a különböző Azure Redis Cache példányai között toomigrate vagy hello gyorsítótár adatokkal használat előtt.</span><span class="sxs-lookup"><span data-stu-id="c484b-294">Import/Export enables you toomigrate between different Azure Redis Cache instances or populate hello cache with data before use.</span></span>

<span data-ttu-id="c484b-295">Importálás használt toobring Redis kompatibilis Rekordadatbázis fájlokat minden futtató bármely felhő és a környezet, beleértve a futó Linux, Windows vagy bármely felhőalapú szolgáltató, például az Amazon Web Services, míg mások Redis Redis-kiszolgáló lehet.</span><span class="sxs-lookup"><span data-stu-id="c484b-295">Import can be used toobring Redis compatible RDB files from any Redis server running in any cloud or environment, including Redis running on Linux, Windows, or any cloud provider such as Amazon Web Services and others.</span></span> <span data-ttu-id="c484b-296">Adatok importálása egy egyszerűen toocreate egy előre megadott adatokkal gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="c484b-296">Importing data is an easy way toocreate a cache with pre-populated data.</span></span> <span data-ttu-id="c484b-297">Hello importálási folyamat során az Azure Redis Cache hello Rekordadatbázis fájlok az Azure storage betölti a memóriába, és, majd a kívánt hello kulcsok hello gyorsítótárba.</span><span class="sxs-lookup"><span data-stu-id="c484b-297">During hello import process, Azure Redis Cache loads hello RDB files from Azure storage into memory, and then inserts hello keys into hello cache.</span></span>

<span data-ttu-id="c484b-298">Exportálás lehetővé teszi tooexport hello adatok Azure Redis Cache tooRedis kompatibilis Rekordadatbázis fájlokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="c484b-298">Export allows you tooexport hello data stored in Azure Redis Cache tooRedis compatible RDB files.</span></span> <span data-ttu-id="c484b-299">Ez a szolgáltatás toomove adatokat egy Azure Redis Cache példány tooanother vagy tooanother Redis-kiszolgáló is használhatja.</span><span class="sxs-lookup"><span data-stu-id="c484b-299">You can use this feature toomove data from one Azure Redis Cache instance tooanother or tooanother Redis server.</span></span> <span data-ttu-id="c484b-300">VM állomások hello Azure Redis Cache server-példány, és hello fájl kijelölve tárfiók feltöltött toohello hello hello exportálás során ideiglenes fájl jön létre.</span><span class="sxs-lookup"><span data-stu-id="c484b-300">During hello export process, a temporary file is created on hello VM that hosts hello Azure Redis Cache server instance, and hello file is uploaded toohello designated storage account.</span></span> <span data-ttu-id="c484b-301">Hello az exportálási művelet befejezése után a vagy állapota sikeres végrehajtásával vagy hibajelzéssel hello ideiglenes fájl törlődik.</span><span class="sxs-lookup"><span data-stu-id="c484b-301">When hello export operation completes with either a status of success or failure, hello temporary file is deleted.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c484b-302">Prémium szintű réteghez gyorsítótárainak importálási/exportálási csak érhető el.</span><span class="sxs-lookup"><span data-stu-id="c484b-302">Import/Export is only available for Premium tier caches.</span></span> <span data-ttu-id="c484b-303">További információt és útmutatást lásd: [importálhat és exportálhat adatokat az Azure Redis Cache](cache-how-to-import-export-data.md).</span><span class="sxs-lookup"><span data-stu-id="c484b-303">For more information and instructions, see [Import and Export data in Azure Redis Cache](cache-how-to-import-export-data.md).</span></span>
> 
> 

### <a name="reboot"></a><span data-ttu-id="c484b-304">Újraindítás</span><span class="sxs-lookup"><span data-stu-id="c484b-304">Reboot</span></span>
<span data-ttu-id="c484b-305">Hello **újraindítás** panel lehetővé teszi a gyorsítótár tooreboot hello csomópontok.</span><span class="sxs-lookup"><span data-stu-id="c484b-305">hello **Reboot** blade allows you tooreboot hello nodes of your cache.</span></span> <span data-ttu-id="c484b-306">Az újraindítás funkció lehetővé teszi, hogy Ön tootest a rugalmasságot az alkalmazás egy gyorsítótár-csomópont hibája esetén.</span><span class="sxs-lookup"><span data-stu-id="c484b-306">This reboot capability enables you tootest your application for resiliency if there is a failure of a cache node.</span></span>

![Újraindítás](./media/cache-configure/redis-cache-reboot.png)

<span data-ttu-id="c484b-308">Ha a prémium szintű gyorsítótár fürtözési engedélyezve van, mely szilánkok hello gyorsítótár tooreboot a választhatja meg.</span><span class="sxs-lookup"><span data-stu-id="c484b-308">If you have a premium cache with clustering enabled, you can select which shards of hello cache tooreboot.</span></span>

![Újraindítás](./media/cache-configure/redis-cache-reboot-cluster.png)

<span data-ttu-id="c484b-310">tooreboot a gyorsítótár egy vagy több csomópontján válasszon szükséges hello csomópontot, majd kattintson a **újraindítás**.</span><span class="sxs-lookup"><span data-stu-id="c484b-310">tooreboot one or more nodes of your cache, select hello desired nodes and click **Reboot**.</span></span> <span data-ttu-id="c484b-311">Ha a prémium szintű gyorsítótár fürtözési engedélyezve van, jelölje ki a hello shard(s) tooreboot, és kattintson **újraindítás**.</span><span class="sxs-lookup"><span data-stu-id="c484b-311">If you have a premium cache with clustering enabled, select hello shard(s) tooreboot and then click **Reboot**.</span></span> <span data-ttu-id="c484b-312">Néhány perc elteltével hello kijelölt csomópont újraindítás, és néhány percen belül újra online állapotba kerülnek.</span><span class="sxs-lookup"><span data-stu-id="c484b-312">After a few minutes, hello selected node(s) reboot, and are back online a few minutes later.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c484b-313">Minden tarifacsomagok Újraindítás most érhető el.</span><span class="sxs-lookup"><span data-stu-id="c484b-313">Reboot is now available for all pricing tiers.</span></span> <span data-ttu-id="c484b-314">További információt és útmutatást lásd: [Azure Redis Cache felügyeleti - újraindítás](cache-administration.md#reboot).</span><span class="sxs-lookup"><span data-stu-id="c484b-314">For more information and instructions, see [Azure Redis Cache administration - Reboot](cache-administration.md#reboot).</span></span>
> 
> 


## <a name="monitoring"></a><span data-ttu-id="c484b-315">Figyelés</span><span class="sxs-lookup"><span data-stu-id="c484b-315">Monitoring</span></span>

<span data-ttu-id="c484b-316">Hello **figyelés** szakasz lehetővé teszi a tooconfigure diagnosztikai és a Redis Cache figyelését.</span><span class="sxs-lookup"><span data-stu-id="c484b-316">hello **Monitoring** section allows you tooconfigure diagnostics and monitoring for your Redis Cache.</span></span> <span data-ttu-id="c484b-317">Azure Redis Cache-figyelés és diagnosztika további információkért lásd: [hogyan toomonitor Azure Redis Cache-gyorsítótár](cache-how-to-monitor.md).</span><span class="sxs-lookup"><span data-stu-id="c484b-317">For more information on Azure Redis Cache monitoring and diagnostics, see [How toomonitor Azure Redis Cache](cache-how-to-monitor.md).</span></span>

![Diagnosztika](./media/cache-configure/redis-cache-diagnostics.png)

* [<span data-ttu-id="c484b-319">Redis metrikák</span><span class="sxs-lookup"><span data-stu-id="c484b-319">Redis metrics</span></span>](#redis-metrics)
* [<span data-ttu-id="c484b-320">A riasztási szabályok</span><span class="sxs-lookup"><span data-stu-id="c484b-320">Alert rules</span></span>](#alert-rules)
* [<span data-ttu-id="c484b-321">Diagnosztika</span><span class="sxs-lookup"><span data-stu-id="c484b-321">Diagnostics</span></span>](#diagnostics)

### <a name="redis-metrics"></a><span data-ttu-id="c484b-322">Redis metrikák</span><span class="sxs-lookup"><span data-stu-id="c484b-322">Redis metrics</span></span>
<span data-ttu-id="c484b-323">Kattintson a **metrikák Redis** túl[metrikák megtekintése](cache-how-to-monitor.md#view-cache-metrics) a gyorsítótárhoz.</span><span class="sxs-lookup"><span data-stu-id="c484b-323">Click **Redis metrics** too[view metrics](cache-how-to-monitor.md#view-cache-metrics) for your cache.</span></span>

### <a name="alert-rules"></a><span data-ttu-id="c484b-324">A riasztási szabályok</span><span class="sxs-lookup"><span data-stu-id="c484b-324">Alert rules</span></span>

<span data-ttu-id="c484b-325">Kattintson a **riasztási szabályok** tooconfigure riasztások a Redis Cache metrikák alapján.</span><span class="sxs-lookup"><span data-stu-id="c484b-325">Click **Alert rules** tooconfigure alerts based on Redis Cache metrics.</span></span> <span data-ttu-id="c484b-326">További információkért lásd: [riasztások](cache-how-to-monitor.md#alerts).</span><span class="sxs-lookup"><span data-stu-id="c484b-326">For more information, see [Alerts](cache-how-to-monitor.md#alerts).</span></span>

### <a name="diagnostics"></a><span data-ttu-id="c484b-327">Diagnosztika</span><span class="sxs-lookup"><span data-stu-id="c484b-327">Diagnostics</span></span>

<span data-ttu-id="c484b-328">Gyorsítótár-metrikát a Azure figyelő alapesetben [30 napig tárolja a](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#store-and-archive) , majd törölni.</span><span class="sxs-lookup"><span data-stu-id="c484b-328">By default, cache metrics in Azure Monitor are [stored for 30 days](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#store-and-archive) and then deleted.</span></span> <span data-ttu-id="c484b-329">toopersist a 30 napnál hosszabb gyorsítótár metrikáját kattintson **diagnosztika** túl[hello tárfiók konfigurálása](cache-how-to-monitor.md#export-cache-metrics) használt toostore gyorsítótár diagnosztika.</span><span class="sxs-lookup"><span data-stu-id="c484b-329">toopersist your cache metrics for longer than 30 days, click **Diagnostics** too[configure hello storage account](cache-how-to-monitor.md#export-cache-metrics) used toostore cache diagnostics.</span></span>

>[!NOTE]
><span data-ttu-id="c484b-330">A hozzáadása tooarchiving a gyorsítótár metrikák toostorage is [tooan eseményközpont adatfolyam formájában, vagy küldhet nekik tooLog Analytics](../monitoring-and-diagnostics/monitoring-overview-metrics.md#export-metrics).</span><span class="sxs-lookup"><span data-stu-id="c484b-330">In addition tooarchiving your cache metrics toostorage, you can also [stream them tooan Event hub or send them tooLog Analytics](../monitoring-and-diagnostics/monitoring-overview-metrics.md#export-metrics).</span></span>
>
>

## <a name="support--troubleshooting-settings"></a><span data-ttu-id="c484b-331">Támogatási és hibaelhárítási beállítások</span><span class="sxs-lookup"><span data-stu-id="c484b-331">Support & troubleshooting settings</span></span>
<span data-ttu-id="c484b-332">hello szolgáltatásbeállításai hello **támogatási + hibaelhárítási** szakasz a gyorsítótár kapcsolatos problémák megoldása lehetőségeket biztosít.</span><span class="sxs-lookup"><span data-stu-id="c484b-332">hello settings in hello **Support + troubleshooting** section provide you with options for resolving issues with your cache.</span></span>

![Támogatási + hibaelhárítása](./media/cache-configure/redis-cache-support-troubleshooting.png)

* [<span data-ttu-id="c484b-334">Erőforrás állapota</span><span class="sxs-lookup"><span data-stu-id="c484b-334">Resource health</span></span>](#resource-health)
* [<span data-ttu-id="c484b-335">Új támogatási kérelem</span><span class="sxs-lookup"><span data-stu-id="c484b-335">New support request</span></span>](#new-support-request)

### <a name="resource-health"></a><span data-ttu-id="c484b-336">Erőforrás állapota</span><span class="sxs-lookup"><span data-stu-id="c484b-336">Resource health</span></span>
<span data-ttu-id="c484b-337">**Erőforrás állapota** az erőforrás figyeli, és jelzi, hogy ha a várt módon fut..</span><span class="sxs-lookup"><span data-stu-id="c484b-337">**Resource health** watches your resource and tells you if it's running as expected.</span></span> <span data-ttu-id="c484b-338">Hello Azure Resource health service kapcsolatos további információkért lásd: [Azure-erőforrás állapotának áttekintése](../resource-health/resource-health-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c484b-338">For more information about hello Azure Resource health service, see [Azure Resource health overview](../resource-health/resource-health-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="c484b-339">Erőforrás állapota jelenleg nem tudja tooreport hello egészségügyi üzemeltetett virtuális hálózatban Azure Redis Cache példány.</span><span class="sxs-lookup"><span data-stu-id="c484b-339">Resource health is currently unable tooreport on hello health of Azure Redis Cache instances hosted in a virtual network.</span></span> <span data-ttu-id="c484b-340">További információkért lásd: [minden gyorsítótár-funkciók esetén egy virtuális hálózat a gyorsítótárhoz működik?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)</span><span class="sxs-lookup"><span data-stu-id="c484b-340">For more information, see [Do all cache features work when hosting a cache in a VNET?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)</span></span>
> 
> 

### <a name="new-support-request"></a><span data-ttu-id="c484b-341">Új támogatási kérelem</span><span class="sxs-lookup"><span data-stu-id="c484b-341">New support request</span></span>
<span data-ttu-id="c484b-342">Kattintson a **új támogatja a kérelem** tooopen egy támogatási kérést a gyorsítótárhoz.</span><span class="sxs-lookup"><span data-stu-id="c484b-342">Click **New support request** tooopen a support request for your cache.</span></span>





## <a name="default-redis-server-configuration"></a><span data-ttu-id="c484b-343">Alapértelmezett Redis-kiszolgálókonfiguráció</span><span class="sxs-lookup"><span data-stu-id="c484b-343">Default Redis server configuration</span></span>
<span data-ttu-id="c484b-344">Új Azure Redis Cache példány alapértelmezett Redis konfigurációs értékeket a következő hello vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="c484b-344">New Azure Redis Cache instances are configured with hello following default Redis configuration values.</span></span>

> [!NOTE]
> <span data-ttu-id="c484b-345">Ebben a szakaszban hello-beállítások használatával nem lehet megváltoztatni hello `StackExchange.Redis.IServer.ConfigSet` metódust.</span><span class="sxs-lookup"><span data-stu-id="c484b-345">hello settings in this section cannot be changed using hello `StackExchange.Redis.IServer.ConfigSet` method.</span></span> <span data-ttu-id="c484b-346">Ha ezt a módszert nevezik ebben a szakaszban hello parancsok egyikét, egy kivétel hasonló toohello következő fordul elő:</span><span class="sxs-lookup"><span data-stu-id="c484b-346">If this method is called with one of hello commands in this section, an exception similar toohello following is thrown:</span></span>  
> 
> `StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
> 
> <span data-ttu-id="c484b-347">Bármely értékek, amelyek konfigurálhatók, például **maximális memória-házirenddel**, hello Azure-portálon vagy a parancssori felügyeleti eszközök, például az Azure parancssori felület vagy a PowerShell segítségével konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="c484b-347">Any values that are configurable, such as **max-memory-policy**, are configurable through hello Azure portal or command-line management tools such as Azure CLI or PowerShell.</span></span>
> 
> 

| <span data-ttu-id="c484b-348">Beállítás</span><span class="sxs-lookup"><span data-stu-id="c484b-348">Setting</span></span> | <span data-ttu-id="c484b-349">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="c484b-349">Default value</span></span> | <span data-ttu-id="c484b-350">Leírás</span><span class="sxs-lookup"><span data-stu-id="c484b-350">Description</span></span> |
| --- | --- | --- |
| `databases` |<span data-ttu-id="c484b-351">16</span><span class="sxs-lookup"><span data-stu-id="c484b-351">16</span></span> |<span data-ttu-id="c484b-352">hello alapértelmezett számú adatbázis 16, de be lehet állítani egy másik száma alapján hello IP-címek. <sup>1</sup> hello alapértelmezett adatbázis DB 0, kiválaszthatja, hogy egy másik kapcsolati alapját használatával `connection.GetDatabase(dbid)` ahol `dbid` közötti szám `0` és `databases - 1`.</span><span class="sxs-lookup"><span data-stu-id="c484b-352">hello default number of databases is 16 but you can configure a different number based on hello pricing tier.<sup>1</sup> hello default database is DB 0, you can select a different one on a per-connection basis using `connection.GetDatabase(dbid)` where `dbid` is a number between `0` and `databases - 1`.</span></span> |
| `maxclients` |<span data-ttu-id="c484b-353">IP-címek hello függ<sup>2</sup></span><span class="sxs-lookup"><span data-stu-id="c484b-353">Depends on hello pricing tier<sup>2</sup></span></span> |<span data-ttu-id="c484b-354">Hello engedélyezett azonos kapcsolódó ügyfelek maximális száma hello idő.</span><span class="sxs-lookup"><span data-stu-id="c484b-354">This is hello maximum number of connected clients allowed at hello same time.</span></span> <span data-ttu-id="c484b-355">Hello korlát elérését követően a Redis bezárása hello új kapcsolatok egy "elért ügyfelek maximális száma" hibát ad vissza.</span><span class="sxs-lookup"><span data-stu-id="c484b-355">Once hello limit is reached Redis closes all hello new connections, returning a 'max number of clients reached' error.</span></span> |
| `maxmemory-policy` |`volatile-lru` |<span data-ttu-id="c484b-356">Maxmemory házirend hello beállítás hogyan választja ki a Redis milyen tooremove amikor `maxmemory` (hello hello gyorsítótár létrehozásakor kiválasztott ajánlat hello gyorsítótár) méretet.</span><span class="sxs-lookup"><span data-stu-id="c484b-356">Maxmemory policy is hello setting for how Redis selects what tooremove when `maxmemory` (hello size of hello cache offering you selected when you created hello cache) is reached.</span></span> <span data-ttu-id="c484b-357">Azure Redis Cache hello alapértelmezett a beállítás `volatile-lru`, amely hello kulcsok távolítja el a jelszólejárat LRU algoritmus használatával.</span><span class="sxs-lookup"><span data-stu-id="c484b-357">With Azure Redis Cache hello default setting is `volatile-lru`, which removes hello keys with an expiration set using an LRU algorithm.</span></span> <span data-ttu-id="c484b-358">Ez a beállítás hello Azure-portálon konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="c484b-358">This setting can be configured in hello Azure portal.</span></span> <span data-ttu-id="c484b-359">További információkért lásd: [memória házirendek](#memory-policies).</span><span class="sxs-lookup"><span data-stu-id="c484b-359">For more information, see [Memory policies](#memory-policies).</span></span> |
| `maxmemory-samples` |<span data-ttu-id="c484b-360">3</span><span class="sxs-lookup"><span data-stu-id="c484b-360">3</span></span> |<span data-ttu-id="c484b-361">toosave memória, a LRU és a minimális TTL algoritmusok is közelítő algoritmusok pontos algoritmusok helyett.</span><span class="sxs-lookup"><span data-stu-id="c484b-361">toosave memory, LRU and minimal TTL algorithms are approximated algorithms instead of precise algorithms.</span></span> <span data-ttu-id="c484b-362">Alapértelmezés szerint Redis ellenőrzések három kulcsok és kivételezések hello egy kisebb mostanában használt.</span><span class="sxs-lookup"><span data-stu-id="c484b-362">By default Redis checks three keys and picks hello one that was used less recently.</span></span> |
| `lua-time-limit` |<span data-ttu-id="c484b-363">5,000</span><span class="sxs-lookup"><span data-stu-id="c484b-363">5,000</span></span> |<span data-ttu-id="c484b-364">Maximális végrehajtási idő ezredmásodpercben Lua parancsfájlra.</span><span class="sxs-lookup"><span data-stu-id="c484b-364">Max execution time of a Lua script in milliseconds.</span></span> <span data-ttu-id="c484b-365">Hello maximális végrehajtási idő elérésekor a Redis naplózza, hogy egy parancsfájl még végrehajtása után hello maximális engedélyezett idő, és hiba tooreply tooqueries kezdődik.</span><span class="sxs-lookup"><span data-stu-id="c484b-365">If hello maximum execution time is reached, Redis logs that a script is still in execution after hello maximum allowed time, and starts tooreply tooqueries with an error.</span></span> |
| `lua-event-limit` |<span data-ttu-id="c484b-366">500</span><span class="sxs-lookup"><span data-stu-id="c484b-366">500</span></span> |<span data-ttu-id="c484b-367">Parancsfájl esemény sor maximális mérete.</span><span class="sxs-lookup"><span data-stu-id="c484b-367">Max size of script event queue.</span></span> |
| <span data-ttu-id="c484b-368">`client-output-buffer-limit` `normalclient-output-buffer-limit` `pubsub`</span><span class="sxs-lookup"><span data-stu-id="c484b-368">`client-output-buffer-limit` `normalclient-output-buffer-limit` `pubsub`</span></span> |<span data-ttu-id="c484b-369">0 032mb 8mb 60 0</span><span class="sxs-lookup"><span data-stu-id="c484b-369">0 0 032mb 8mb 60</span></span> |<span data-ttu-id="c484b-370">hello ügyfél kimeneti puffer korlátok lehetnek ügyfelek, amelyek nem adatok beolvasása hello server elég gyors (gyakori oka az, hogy Pub/Sub ügyfelet nem lehet gyors hello publisher születik őket felhasználni üzenetek) valamilyen okból tooforce leválasztása használt.</span><span class="sxs-lookup"><span data-stu-id="c484b-370">hello client output buffer limits can be used tooforce disconnection of clients that are not reading data from hello server fast enough for some reason (a common reason is that a Pub/Sub client can't consume messages as fast as hello publisher can produce them).</span></span> <span data-ttu-id="c484b-371">További információkért lásd: [http://redis.io/topics/clients](http://redis.io/topics/clients).</span><span class="sxs-lookup"><span data-stu-id="c484b-371">For more information, see [http://redis.io/topics/clients](http://redis.io/topics/clients).</span></span> |

<span data-ttu-id="c484b-372"><a name="databases"></a>
<sup>1</sup>hello korlát `databases` eltér az egyes Azure Redis Cache IP-címek és a gyorsítótár létrehozásakor állítható be.</span><span class="sxs-lookup"><span data-stu-id="c484b-372"><a name="databases"></a>
<sup>1</sup>hello limit for `databases` is different for each Azure Redis Cache pricing tier and can be set at cache creation.</span></span> <span data-ttu-id="c484b-373">Ha nincs `databases` beállítás van megadva gyorsítótár létrehozása során hello alapértelmezett érték 16.</span><span class="sxs-lookup"><span data-stu-id="c484b-373">If no `databases` setting is specified during cache creation, hello default is 16.</span></span>

* <span data-ttu-id="c484b-374">Basic és Standard gyorsítótárak</span><span class="sxs-lookup"><span data-stu-id="c484b-374">Basic and Standard caches</span></span>
  * <span data-ttu-id="c484b-375">C0 csomag (250 MB) gyorsítótár - too16 adatbázisok</span><span class="sxs-lookup"><span data-stu-id="c484b-375">C0 (250 MB) cache - up too16 databases</span></span>
  * <span data-ttu-id="c484b-376">C1 (1 GB-os) gyorsítótár - too16 adatbázisok</span><span class="sxs-lookup"><span data-stu-id="c484b-376">C1 (1 GB) cache - up too16 databases</span></span>
  * <span data-ttu-id="c484b-377">C2 csomag (2,5 GB) gyorsítótár - too16 adatbázisok</span><span class="sxs-lookup"><span data-stu-id="c484b-377">C2 (2.5 GB) cache - up too16 databases</span></span>
  * <span data-ttu-id="c484b-378">C3 csomag (6 GB) gyorsítótár - too16 adatbázisok</span><span class="sxs-lookup"><span data-stu-id="c484b-378">C3 (6 GB) cache - up too16 databases</span></span>
  * <span data-ttu-id="c484b-379">C4 (13 GB) gyorsítótár - too32 adatbázisok</span><span class="sxs-lookup"><span data-stu-id="c484b-379">C4 (13 GB) cache - up too32 databases</span></span>
  * <span data-ttu-id="c484b-380">C5 (26 GB) gyorsítótár - too48 adatbázisok</span><span class="sxs-lookup"><span data-stu-id="c484b-380">C5 (26 GB) cache - up too48 databases</span></span>
  * <span data-ttu-id="c484b-381">C6 (53 GB) gyorsítótár - too64 adatbázisok</span><span class="sxs-lookup"><span data-stu-id="c484b-381">C6 (53 GB) cache - up too64 databases</span></span>
* <span data-ttu-id="c484b-382">Prémium szintű gyorsítótárak</span><span class="sxs-lookup"><span data-stu-id="c484b-382">Premium caches</span></span>
  * <span data-ttu-id="c484b-383">(6 GB - 60 GB) – P1 too16 adatbázisok</span><span class="sxs-lookup"><span data-stu-id="c484b-383">P1 (6 GB - 60 GB) - up too16 databases</span></span>
  * <span data-ttu-id="c484b-384">(13 GB - 130 GB) – P2 too32 adatbázisok</span><span class="sxs-lookup"><span data-stu-id="c484b-384">P2 (13 GB - 130 GB) - up too32 databases</span></span>
  * <span data-ttu-id="c484b-385">(26 GB - 260 GB) – P3 too48 adatbázisok</span><span class="sxs-lookup"><span data-stu-id="c484b-385">P3 (26 GB - 260 GB) - up too48 databases</span></span>
  * <span data-ttu-id="c484b-386">P4 (53 GB - 530 GB) – too64 adatbázisok</span><span class="sxs-lookup"><span data-stu-id="c484b-386">P4 (53 GB - 530 GB) - up too64 databases</span></span>
  * <span data-ttu-id="c484b-387">Engedélyezve - Redis-fürt minden prémium gyorsítótárak Redis fürt csak 0-adatbázis úgy hello támogatja `databases` korlátozza a prémium szintű gyorsítótár engedélyezve van a Redis-fürt hatékonyan érték 1 és hello [válasszon](http://redis.io/commands/select) parancs nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="c484b-387">All premium caches with Redis cluster enabled - Redis cluster only supports use of database 0 so hello `databases` limit for any premium cache with Redis cluster enabled is effectively 1 and hello [Select](http://redis.io/commands/select) command is not allowed.</span></span> <span data-ttu-id="c484b-388">További információkért lásd: [van toomake bármely módosítások toomy ügyfél alkalmazás toouse fürtszolgáltatás?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)</span><span class="sxs-lookup"><span data-stu-id="c484b-388">For more information, see [Do I need toomake any changes toomy client application toouse clustering?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)</span></span>

<span data-ttu-id="c484b-389">Adatbázisokkal kapcsolatos további információkért lásd: [Mik azok a Redis-adatbázisok?](cache-faq.md#what-are-redis-databases)</span><span class="sxs-lookup"><span data-stu-id="c484b-389">For more information about databases, see [What are Redis databases?](cache-faq.md#what-are-redis-databases)</span></span>

> [!NOTE]
> <span data-ttu-id="c484b-390">Hello `databases` beállítás csak a gyorsítótár létrehozása során konfigurált, és csak a PowerShell, a parancssori felületen vagy a más felügyeleti ügyfelek lehet.</span><span class="sxs-lookup"><span data-stu-id="c484b-390">hello `databases` setting can be configured only during cache creation and only using PowerShell, CLI, or other management clients.</span></span> <span data-ttu-id="c484b-391">Példa konfigurálása `databases` gyorsítótár létrehozása PowerShell használatával, lásd: [New-AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).</span><span class="sxs-lookup"><span data-stu-id="c484b-391">For an example of configuring `databases` during cache creation using PowerShell, see [New-AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).</span></span>
> 
> 

<span data-ttu-id="c484b-392"><a name="maxclients"></a>
<sup>2</sup> `maxclients` eltér az egyes Azure Redis Cache tarifacsomagra vált.</span><span class="sxs-lookup"><span data-stu-id="c484b-392"><a name="maxclients"></a>
<sup>2</sup>`maxclients` is different for each Azure Redis Cache pricing tier.</span></span>

* <span data-ttu-id="c484b-393">Basic és Standard gyorsítótárak</span><span class="sxs-lookup"><span data-stu-id="c484b-393">Basic and Standard caches</span></span>
  * <span data-ttu-id="c484b-394">C0 csomag (250 MB) gyorsítótár - too256 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="c484b-394">C0 (250 MB) cache - up too256 connections</span></span>
  * <span data-ttu-id="c484b-395">C1 (1 GB-os) gyorsítótár - too1 mentése, 000 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="c484b-395">C1 (1 GB) cache - up too1,000 connections</span></span>
  * <span data-ttu-id="c484b-396">C2 csomag (2,5 GB) gyorsítótár - too2 mentése, 000 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="c484b-396">C2 (2.5 GB) cache - up too2,000 connections</span></span>
  * <span data-ttu-id="c484b-397">C3 csomag (6 GB) gyorsítótár - too5 mentése, 000 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="c484b-397">C3 (6 GB) cache - up too5,000 connections</span></span>
  * <span data-ttu-id="c484b-398">C4 (13 GB) gyorsítótár - too10 mentése, 000 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="c484b-398">C4 (13 GB) cache - up too10,000 connections</span></span>
  * <span data-ttu-id="c484b-399">C5 (26 GB) gyorsítótár - too15 mentése, 000 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="c484b-399">C5 (26 GB) cache - up too15,000 connections</span></span>
  * <span data-ttu-id="c484b-400">C6 (53 GB) gyorsítótár - too20 mentése, 000 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="c484b-400">C6 (53 GB) cache - up too20,000 connections</span></span>
* <span data-ttu-id="c484b-401">Prémium szintű gyorsítótárak</span><span class="sxs-lookup"><span data-stu-id="c484b-401">Premium caches</span></span>
  * <span data-ttu-id="c484b-402">(6 GB - 60 GB) – P1 too7, 500 kapcsolatok mentése</span><span class="sxs-lookup"><span data-stu-id="c484b-402">P1 (6 GB - 60 GB) - up too7,500 connections</span></span>
  * <span data-ttu-id="c484b-403">(13 GB - 130 GB) – P2 too15, 000 kapcsolatok mentése</span><span class="sxs-lookup"><span data-stu-id="c484b-403">P2 (13 GB - 130 GB) - up too15,000 connections</span></span>
  * <span data-ttu-id="c484b-404">(26 GB - 260 GB) – P3 too30, 000 kapcsolatok mentése</span><span class="sxs-lookup"><span data-stu-id="c484b-404">P3 (26 GB - 260 GB) - up too30,000 connections</span></span>
  * <span data-ttu-id="c484b-405">P4 (53 GB - 530 GB) – too40, 000 kapcsolatok mentése</span><span class="sxs-lookup"><span data-stu-id="c484b-405">P4 (53 GB - 530 GB) - up too40,000 connections</span></span>

> [!NOTE]
> <span data-ttu-id="c484b-406">Miközben lehetővé teszi, hogy minden egyes gyorsítótár méretének *legfeljebb* bizonyos számú kapcsolatok, minden kapcsolat tooRedis rendelkezik terhet társítva.</span><span class="sxs-lookup"><span data-stu-id="c484b-406">While each size of cache allows *up to* a certain number of connections, each connection tooRedis has overhead associated with it.</span></span> <span data-ttu-id="c484b-407">Erre példa ilyen terheléssel lehet Processzor- és memóriahasználatról miatt a TLS/SSL-titkosítást.</span><span class="sxs-lookup"><span data-stu-id="c484b-407">An example of such overhead would be CPU and memory usage as a result of TLS/SSL encryption.</span></span> <span data-ttu-id="c484b-408">a megadott gyorsítótárméret hello maximális kapcsolathoz megadott korlátot azt feltételezi, hogy egy némileg betöltött gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="c484b-408">hello maximum connection limit for a given cache size assumes a lightly loaded cache.</span></span> <span data-ttu-id="c484b-409">Ha a kapcsolat terhelés betöltése *plus* Ügyfélműveletek a terhelés meghaladja a kapacitását hello rendszerhez, hello gyorsítótár kapacitás problémákat tapasztalhat, még akkor is, ha nem lépik túl a gyorsítótár jelenlegi mérete hello hello kapcsolathoz megadott korlátot.</span><span class="sxs-lookup"><span data-stu-id="c484b-409">If load from connection overhead *plus* load from client operations exceeds capacity for hello system, hello cache can experience capacity issues even if you have not exceeded hello connection limit for hello current cache size.</span></span>
> 
> 



## <a name="redis-commands-not-supported-in-azure-redis-cache"></a><span data-ttu-id="c484b-410">A parancsok nem támogatott az Azure Redis Cache redis</span><span class="sxs-lookup"><span data-stu-id="c484b-410">Redis commands not supported in Azure Redis Cache</span></span>
> [!IMPORTANT]
> <span data-ttu-id="c484b-411">Microsoft által felügyelt konfigurálása és kezelése az Azure Redis Cache példányt, mert hello következő parancsok le vannak tiltva.</span><span class="sxs-lookup"><span data-stu-id="c484b-411">Because configuration and management of Azure Redis Cache instances is managed by Microsoft, hello following commands are disabled.</span></span> <span data-ttu-id="c484b-412">Ha tooinvoke próbálja őket, túl a hasonló hibaüzenetet kap`"(error) ERR unknown command"`.</span><span class="sxs-lookup"><span data-stu-id="c484b-412">If you try tooinvoke them, you receive an error message similar too`"(error) ERR unknown command"`.</span></span>
> 
> * <span data-ttu-id="c484b-413">BGREWRITEAOF</span><span class="sxs-lookup"><span data-stu-id="c484b-413">BGREWRITEAOF</span></span>
> * <span data-ttu-id="c484b-414">BGSAVE</span><span class="sxs-lookup"><span data-stu-id="c484b-414">BGSAVE</span></span>
> * <span data-ttu-id="c484b-415">CONFIG</span><span class="sxs-lookup"><span data-stu-id="c484b-415">CONFIG</span></span>
> * <span data-ttu-id="c484b-416">HIBAKERESÉSI</span><span class="sxs-lookup"><span data-stu-id="c484b-416">DEBUG</span></span>
> * <span data-ttu-id="c484b-417">ÁTTELEPÍTÉSE</span><span class="sxs-lookup"><span data-stu-id="c484b-417">MIGRATE</span></span>
> * <span data-ttu-id="c484b-418">MENTÉSE</span><span class="sxs-lookup"><span data-stu-id="c484b-418">SAVE</span></span>
> * <span data-ttu-id="c484b-419">LEÁLLÍTÁS</span><span class="sxs-lookup"><span data-stu-id="c484b-419">SHUTDOWN</span></span>
> * <span data-ttu-id="c484b-420">SLAVEOF</span><span class="sxs-lookup"><span data-stu-id="c484b-420">SLAVEOF</span></span>
> * <span data-ttu-id="c484b-421">FÜRT - fürt írási parancsokat le vannak tiltva, de csak olvasható fürt parancsot.</span><span class="sxs-lookup"><span data-stu-id="c484b-421">CLUSTER - Cluster write commands are disabled, but read-only Cluster commands are permitted.</span></span>
> 
> 

<span data-ttu-id="c484b-422">Redis parancsokkal kapcsolatos további információkért lásd: [http://redis.io/commands](http://redis.io/commands).</span><span class="sxs-lookup"><span data-stu-id="c484b-422">For more information about Redis commands, see [http://redis.io/commands](http://redis.io/commands).</span></span>

## <a name="redis-console"></a><span data-ttu-id="c484b-423">Redis-konzol</span><span class="sxs-lookup"><span data-stu-id="c484b-423">Redis console</span></span>
<span data-ttu-id="c484b-424">Biztonságosan kiadhatja parancsok tooyour Azure Redis Cache példány hello segítségével **Redis konzol**, a hello Azure-portál a gyorsítótár minden csomagban elérhető.</span><span class="sxs-lookup"><span data-stu-id="c484b-424">You can securely issue commands tooyour Azure Redis Cache instances using hello **Redis Console**, which is available in hello Azure portal for all cache tiers.</span></span>

> [!IMPORTANT]
> - <span data-ttu-id="c484b-425">hello Redis-konzol nem tud együttműködni [VNET](cache-how-to-premium-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="c484b-425">hello Redis Console does not work with [VNET](cache-how-to-premium-vnet.md).</span></span> <span data-ttu-id="c484b-426">Ha a gyorsítótár egy virtuális hálózat része, csak a virtuális hálózat hello ügyfelek hello gyorsítótár férhet hozzá.</span><span class="sxs-lookup"><span data-stu-id="c484b-426">When your cache is part of a VNET, only clients in hello VNET can access hello cache.</span></span> <span data-ttu-id="c484b-427">Konzol Redis fut a helyi böngészőben, amely hello virtuális hálózaton kívül, mert tooyour gyorsítótár nem tud kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="c484b-427">Because Redis Console runs in your local browser, which is outside hello VNET, it can't connect tooyour cache.</span></span>
> - <span data-ttu-id="c484b-428">Nem minden Redis-parancsok az Azure Redis Cache támogatottak.</span><span class="sxs-lookup"><span data-stu-id="c484b-428">Not all Redis commands are supported in Azure Redis Cache.</span></span> <span data-ttu-id="c484b-429">Le vannak tiltva, az Azure Redis Cache Redis-parancsok listáját, tekintse meg az előző hello [parancsok nem támogatott az Azure Redis Cache Redis](#redis-commands-not-supported-in-azure-redis-cache) szakasz.</span><span class="sxs-lookup"><span data-stu-id="c484b-429">For a list of Redis commands that are disabled for Azure Redis Cache, see hello previous [Redis commands not supported in Azure Redis Cache](#redis-commands-not-supported-in-azure-redis-cache) section.</span></span> <span data-ttu-id="c484b-430">Redis parancsokkal kapcsolatos további információkért lásd: [http://redis.io/commands](http://redis.io/commands).</span><span class="sxs-lookup"><span data-stu-id="c484b-430">For more information about Redis commands, see [http://redis.io/commands](http://redis.io/commands).</span></span>
> 
> 

<span data-ttu-id="c484b-431">tooaccess hello Redis konzol, kattintson a **konzol** a hello **Redis Cache** panelen.</span><span class="sxs-lookup"><span data-stu-id="c484b-431">tooaccess hello Redis Console, click **Console** from hello **Redis Cache** blade.</span></span>

![Redis-konzol](./media/cache-configure/redis-console-menu.png)

<span data-ttu-id="c484b-433">tooissue parancsok a gyorsítótárpéldány ellen, egyszerűen típus hello a keresett parancs hello konzolba.</span><span class="sxs-lookup"><span data-stu-id="c484b-433">tooissue commands against your cache instance, simply type hello desired command into hello console.</span></span>

![Redis-konzol](./media/cache-configure/redis-console.png)


### <a name="using-hello-redis-console-with-a-premium-clustered-cache"></a><span data-ttu-id="c484b-435">Hello segítségével a prémium konzolon Redis gyorsítótár fürtözött</span><span class="sxs-lookup"><span data-stu-id="c484b-435">Using hello Redis Console with a premium clustered cache</span></span>

<span data-ttu-id="c484b-436">Ha a gyorsítótár hello Redis-konzol használata a prémium fürtözött, parancsok tooa egyetlen shard hello gyorsítótár adhat ki.</span><span class="sxs-lookup"><span data-stu-id="c484b-436">When using hello Redis Console with a premium clustered cache, you can issue commands tooa single shard of hello cache.</span></span> <span data-ttu-id="c484b-437">tooissue parancs tooa adott shard, először kapcsolódik toohello kívánt shard hello shard objektumválasztó kattintva.</span><span class="sxs-lookup"><span data-stu-id="c484b-437">tooissue a command tooa specific shard, first connect toohello desired shard by clicking it on hello shard picker.</span></span>

![Redis-konzol](./media/cache-configure/redis-console-premium-cluster.png)

<span data-ttu-id="c484b-439">Ha egy kulcsot, amely egy másik shard van tárolva, mint a csatlakoztatott shard hello tooaccess kísérli meg, egy hiba üzenet hasonló toohello a következő üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="c484b-439">If you attempt tooaccess a key that is stored in a different shard than hello connected shard, you receive an error message similar toohello following message.</span></span>

```
shard1>get myKey
(error) MOVED 866 13.90.202.154:13000 (shard 0)
```

<span data-ttu-id="c484b-440">Hello előző példában shard 1: hello kijelölt shard, de `myKey` található a szilánkcímtárban 0, hello jelöli `(shard 0)` hello hibaüzenet része.</span><span class="sxs-lookup"><span data-stu-id="c484b-440">In hello previous example, shard 1 is hello selected shard, but `myKey` is located in shard 0, as indicated by hello `(shard 0)` portion of hello error message.</span></span> <span data-ttu-id="c484b-441">Ebben a példában tooaccess `myKey`, jelölje be shard 0 használatával hello shard objektumválasztó, majd a probléma hello szükséges parancsot.</span><span class="sxs-lookup"><span data-stu-id="c484b-441">In this example, tooaccess `myKey`, select shard 0 using hello shard picker, and then issue hello desired command.</span></span>


## <a name="move-your-cache-tooa-new-subscription"></a><span data-ttu-id="c484b-442">Helyezze át a gyorsítótár tooa új előfizetés</span><span class="sxs-lookup"><span data-stu-id="c484b-442">Move your cache tooa new subscription</span></span>
<span data-ttu-id="c484b-443">A gyorsítótár tooa új előfizetés kattintva áthelyezheti **áthelyezése**.</span><span class="sxs-lookup"><span data-stu-id="c484b-443">You can move your cache tooa new subscription by clicking **Move**.</span></span>

![Helyezze át a Redis gyorsítótár](./media/cache-configure/redis-cache-move.png)

<span data-ttu-id="c484b-445">Az erőforrások áthelyezése egy erőforrás csoport tooanother, és egy előfizetés tooanother információkért lásd: [erőforrások toonew erőforráscsoportba vagy előfizetésbe áthelyezése](../azure-resource-manager/resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="c484b-445">For information on moving resources from one resource group tooanother, and from one subscription tooanother, see [Move resources toonew resource group or subscription](../azure-resource-manager/resource-group-move-resources.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c484b-446">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c484b-446">Next steps</span></span>
* <span data-ttu-id="c484b-447">További információ a Redis-parancsok használatával: [hogyan futtathatom Redis parancsok?](cache-faq.md#how-can-i-run-redis-commands)</span><span class="sxs-lookup"><span data-stu-id="c484b-447">For more information on working with Redis commands, see [How can I run Redis commands?](cache-faq.md#how-can-i-run-redis-commands)</span></span>

