---
title: "a MySQL az Azure-adatbázis aaaLimitations |} Microsoft Docs"
description: "A MySQL az Azure-adatbázis előzetes verzió korlátozásai ismerteti."
services: mysql
author: jasonh
ms.author: kamathsun
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 9c877c592bf640f62182d8761c9c51363882d706
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="limitations-in-azure-database-for-mysql-preview"></a><span data-ttu-id="ec470-103">Az Azure-adatbázis korlátozásai MySQL (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="ec470-103">Limitations in Azure Database for MySQL (Preview)</span></span>
<span data-ttu-id="ec470-104">hello Azure adatbázis MySQL szolgáltatás nyilvános előzetes verziójában van.</span><span class="sxs-lookup"><span data-stu-id="ec470-104">hello Azure Database for MySQL service is in public preview.</span></span> <span data-ttu-id="ec470-105">hello következő részek a kapacitás és működési korlátai hello adatbázis szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="ec470-105">hello following sections describe capacity and functional limits in hello database service.</span></span>

## <a name="service-tier-maximums"></a><span data-ttu-id="ec470-106">Szolgáltatási szint méretkorlát</span><span class="sxs-lookup"><span data-stu-id="ec470-106">Service Tier Maximums</span></span>
<span data-ttu-id="ec470-107">Azure MySQL-adatbázis a kiszolgáló létrehozása választhat több szolgáltatásszinttel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ec470-107">Azure Database for MySQL has multiple service tiers you can choose from when creating a server.</span></span> <span data-ttu-id="ec470-108">További információkért lásd: [az egyes szolgáltatásszinteken elérhető](concepts-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="ec470-108">For more information, see [Understand what’s available in each service tier](concepts-service-tiers.md).</span></span>  

<span data-ttu-id="ec470-109">Hiba egy maximális száma érték a kapcsolatok, a számítási egység és a tárolás, az egyes szolgáltatásszinteken hello szolgáltatás előzetes, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="ec470-109">There is a maximum number of connections, compute units, and storage in each service tier during hello service preview, as follows:</span></span> 

|                            |                   |
| :------------------------- | :---------------- |
| <span data-ttu-id="ec470-110">**Kapcsolatok maximális száma**</span><span class="sxs-lookup"><span data-stu-id="ec470-110">**Max connections**</span></span>        |                   |
| <span data-ttu-id="ec470-111">Alapszintű 50 számítási egység</span><span class="sxs-lookup"><span data-stu-id="ec470-111">Basic 50 Compute Units</span></span>     | <span data-ttu-id="ec470-112">50 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="ec470-112">50 connections</span></span>    |
| <span data-ttu-id="ec470-113">Alapszintű 100 számítási egység</span><span class="sxs-lookup"><span data-stu-id="ec470-113">Basic 100 Compute Units</span></span>    | <span data-ttu-id="ec470-114">100 kapcsolatok száma</span><span class="sxs-lookup"><span data-stu-id="ec470-114">100 connections</span></span>   |
| <span data-ttu-id="ec470-115">Standard 100 számítási egység</span><span class="sxs-lookup"><span data-stu-id="ec470-115">Standard 100 Compute Units</span></span> | <span data-ttu-id="ec470-116">200 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="ec470-116">200 connections</span></span>   |
| <span data-ttu-id="ec470-117">Standard 200 számítási egység</span><span class="sxs-lookup"><span data-stu-id="ec470-117">Standard 200 Compute Units</span></span> | <span data-ttu-id="ec470-118">300 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="ec470-118">300 connections</span></span>   |
| <span data-ttu-id="ec470-119">Standard 400 számítási egység</span><span class="sxs-lookup"><span data-stu-id="ec470-119">Standard 400 Compute Units</span></span> | <span data-ttu-id="ec470-120">400 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="ec470-120">400 connections</span></span>   |
| <span data-ttu-id="ec470-121">Standard 800 számítási egység</span><span class="sxs-lookup"><span data-stu-id="ec470-121">Standard 800 Compute Units</span></span> | <span data-ttu-id="ec470-122">500 kapcsolatok száma</span><span class="sxs-lookup"><span data-stu-id="ec470-122">500 connections</span></span>   |
| <span data-ttu-id="ec470-123">**Maximális számítási egység**</span><span class="sxs-lookup"><span data-stu-id="ec470-123">**Max Compute Units**</span></span>      |                   |
| <span data-ttu-id="ec470-124">Alapszintű szolgáltatásszint</span><span class="sxs-lookup"><span data-stu-id="ec470-124">Basic service tier</span></span>         | <span data-ttu-id="ec470-125">100 számítási egység</span><span class="sxs-lookup"><span data-stu-id="ec470-125">100 Compute Units</span></span> |
| <span data-ttu-id="ec470-126">Standard szolgáltatásszint</span><span class="sxs-lookup"><span data-stu-id="ec470-126">Standard service tier</span></span>      | <span data-ttu-id="ec470-127">800 számítási egység</span><span class="sxs-lookup"><span data-stu-id="ec470-127">800 Compute Units</span></span> |
| <span data-ttu-id="ec470-128">**Maximális tárolási**</span><span class="sxs-lookup"><span data-stu-id="ec470-128">**Max storage**</span></span>            |                   |
| <span data-ttu-id="ec470-129">Alapszintű szolgáltatásszint</span><span class="sxs-lookup"><span data-stu-id="ec470-129">Basic service tier</span></span>         | <span data-ttu-id="ec470-130">1 TB</span><span class="sxs-lookup"><span data-stu-id="ec470-130">1 TB</span></span>              |
| <span data-ttu-id="ec470-131">Standard szolgáltatásszint</span><span class="sxs-lookup"><span data-stu-id="ec470-131">Standard service tier</span></span>      | <span data-ttu-id="ec470-132">1 TB</span><span class="sxs-lookup"><span data-stu-id="ec470-132">1 TB</span></span>              |

<span data-ttu-id="ec470-133">Túl sok a kapcsolat elérésekor hello a következő hiba jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="ec470-133">When too many connections are reached, you may receive hello following error:</span></span>
> <span data-ttu-id="ec470-134">1040 (08004). hiba: Túl sok a kapcsolat</span><span class="sxs-lookup"><span data-stu-id="ec470-134">ERROR 1040 (08004): Too many connections</span></span>

## <a name="preview-functional-limitations"></a><span data-ttu-id="ec470-135">Előzetes verzió működési korlátozásai:</span><span class="sxs-lookup"><span data-stu-id="ec470-135">Preview functional limitations:</span></span>
### <a name="scale-operations"></a><span data-ttu-id="ec470-136">A skálázási műveletek:</span><span class="sxs-lookup"><span data-stu-id="ec470-136">Scale operations:</span></span>
1.  <span data-ttu-id="ec470-137">Dinamikus méretezés kiszolgálók között szolgáltatásszintek jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="ec470-137">Dynamic scaling of servers across service tiers is currently not supported.</span></span> <span data-ttu-id="ec470-138">Ez azt jelenti, hogy Basic és Standard szolgáltatásszintek közötti váltás.</span><span class="sxs-lookup"><span data-stu-id="ec470-138">That is, switching between Basic and Standard service tiers.</span></span>
2.  <span data-ttu-id="ec470-139">Dinamikus igény szerinti növelését tárolási előre létrehozott kiszolgálón jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="ec470-139">Dynamic on-demand increase of storage on pre-created server is currently not supported.</span></span>
3.  <span data-ttu-id="ec470-140">Kiszolgáló tároló méretének csökkentése nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="ec470-140">Decreasing server storage size is not supported.</span></span>

### <a name="server-version-upgrades"></a><span data-ttu-id="ec470-141">Kiszolgáló verziófrissítések:</span><span class="sxs-lookup"><span data-stu-id="ec470-141">Server version upgrades:</span></span>
- <span data-ttu-id="ec470-142">Fő adatbázis motor verziók közötti automatikus áttelepítési jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="ec470-142">Automated migration between major database engine versions is currently not supported.</span></span>

### <a name="subscription-management"></a><span data-ttu-id="ec470-143">Előfizetés-kezelés:</span><span class="sxs-lookup"><span data-stu-id="ec470-143">Subscription management:</span></span>
- <span data-ttu-id="ec470-144">Dinamikusan áthelyezése előfizetés és az erőforráscsoport előre létrehozott kiszolgálók jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="ec470-144">Dynamically moving pre-created servers across subscription and resource group is currently not supported.</span></span>

### <a name="point-in-time-restore"></a><span data-ttu-id="ec470-145">Pont-a--visszaállítás egy korábbi időpontra:</span><span class="sxs-lookup"><span data-stu-id="ec470-145">Point-in-time-restore:</span></span>
1.  <span data-ttu-id="ec470-146">Toodifferent szolgáltatásréteg és/vagy számítási egység és a tárhely mérete visszaállítása nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="ec470-146">Restoring toodifferent service tier and/or Compute Units and Storage size is not allowed.</span></span>
2.  <span data-ttu-id="ec470-147">Az eldobott kiszolgáló visszaállítása nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="ec470-147">Restoring a dropped server is not supported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ec470-148">További lépések:</span><span class="sxs-lookup"><span data-stu-id="ec470-148">Next Steps:</span></span>
<span data-ttu-id="ec470-149">[Az egyes szolgáltatásszinteken elérhető](concepts-service-tiers.md)
[MySQL-adatbázis verziója támogatott](concepts-supported-versions.md)</span><span class="sxs-lookup"><span data-stu-id="ec470-149">[What’s available in each service tier](concepts-service-tiers.md)
[Supported MySQL Database Versions](concepts-supported-versions.md)</span></span>
