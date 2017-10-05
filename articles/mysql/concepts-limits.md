---
title: "MySQL az Azure-adatbázis korlátozásai |} Microsoft Docs"
description: "A MySQL az Azure-adatbázis előzetes verzió korlátozásai ismerteti."
services: mysql
author: jasonh
ms.author: kamathsun
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: c61d70897b66c2ffee819ac98c38ab75000db907
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="limitations-in-azure-database-for-mysql-preview"></a><span data-ttu-id="f12a0-103">Az Azure-adatbázis korlátozásai MySQL (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="f12a0-103">Limitations in Azure Database for MySQL (Preview)</span></span>
<span data-ttu-id="f12a0-104">Az Azure-adatbázishoz a MySQL-szolgáltatás nyilvános előzetes verziójában van.</span><span class="sxs-lookup"><span data-stu-id="f12a0-104">The Azure Database for MySQL service is in public preview.</span></span> <span data-ttu-id="f12a0-105">A következő szakaszok ismertetik a kapacitás és az adatbázis szolgáltatásban működik korlátok.</span><span class="sxs-lookup"><span data-stu-id="f12a0-105">The following sections describe capacity and functional limits in the database service.</span></span>

## <a name="service-tier-maximums"></a><span data-ttu-id="f12a0-106">Szolgáltatási szint méretkorlát</span><span class="sxs-lookup"><span data-stu-id="f12a0-106">Service Tier Maximums</span></span>
<span data-ttu-id="f12a0-107">Azure MySQL-adatbázis a kiszolgáló létrehozása választhat több szolgáltatásszinttel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f12a0-107">Azure Database for MySQL has multiple service tiers you can choose from when creating a server.</span></span> <span data-ttu-id="f12a0-108">További információkért lásd: [az egyes szolgáltatásszinteken elérhető](concepts-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="f12a0-108">For more information, see [Understand what’s available in each service tier](concepts-service-tiers.md).</span></span>  

<span data-ttu-id="f12a0-109">Nincs kapcsolatok, a számítási egység és a tárolás, az egyes szolgáltatásszintek tartalmának maximális száma a szolgáltatás előzetes az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="f12a0-109">There is a maximum number of connections, compute units, and storage in each service tier during the service preview, as follows:</span></span> 

|                            |                   |
| :------------------------- | :---------------- |
| <span data-ttu-id="f12a0-110">**Kapcsolatok maximális száma**</span><span class="sxs-lookup"><span data-stu-id="f12a0-110">**Max connections**</span></span>        |                   |
| <span data-ttu-id="f12a0-111">Alapszintű 50 számítási egység</span><span class="sxs-lookup"><span data-stu-id="f12a0-111">Basic 50 Compute Units</span></span>     | <span data-ttu-id="f12a0-112">50 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="f12a0-112">50 connections</span></span>    |
| <span data-ttu-id="f12a0-113">Alapszintű 100 számítási egység</span><span class="sxs-lookup"><span data-stu-id="f12a0-113">Basic 100 Compute Units</span></span>    | <span data-ttu-id="f12a0-114">100 kapcsolatok száma</span><span class="sxs-lookup"><span data-stu-id="f12a0-114">100 connections</span></span>   |
| <span data-ttu-id="f12a0-115">Standard 100 számítási egység</span><span class="sxs-lookup"><span data-stu-id="f12a0-115">Standard 100 Compute Units</span></span> | <span data-ttu-id="f12a0-116">200 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="f12a0-116">200 connections</span></span>   |
| <span data-ttu-id="f12a0-117">Standard 200 számítási egység</span><span class="sxs-lookup"><span data-stu-id="f12a0-117">Standard 200 Compute Units</span></span> | <span data-ttu-id="f12a0-118">300 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="f12a0-118">300 connections</span></span>   |
| <span data-ttu-id="f12a0-119">Standard 400 számítási egység</span><span class="sxs-lookup"><span data-stu-id="f12a0-119">Standard 400 Compute Units</span></span> | <span data-ttu-id="f12a0-120">400 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="f12a0-120">400 connections</span></span>   |
| <span data-ttu-id="f12a0-121">Standard 800 számítási egység</span><span class="sxs-lookup"><span data-stu-id="f12a0-121">Standard 800 Compute Units</span></span> | <span data-ttu-id="f12a0-122">500 kapcsolatok száma</span><span class="sxs-lookup"><span data-stu-id="f12a0-122">500 connections</span></span>   |
| <span data-ttu-id="f12a0-123">**Maximális számítási egység**</span><span class="sxs-lookup"><span data-stu-id="f12a0-123">**Max Compute Units**</span></span>      |                   |
| <span data-ttu-id="f12a0-124">Alapszintű szolgáltatásszint</span><span class="sxs-lookup"><span data-stu-id="f12a0-124">Basic service tier</span></span>         | <span data-ttu-id="f12a0-125">100 számítási egység</span><span class="sxs-lookup"><span data-stu-id="f12a0-125">100 Compute Units</span></span> |
| <span data-ttu-id="f12a0-126">Standard szolgáltatásszint</span><span class="sxs-lookup"><span data-stu-id="f12a0-126">Standard service tier</span></span>      | <span data-ttu-id="f12a0-127">800 számítási egység</span><span class="sxs-lookup"><span data-stu-id="f12a0-127">800 Compute Units</span></span> |
| <span data-ttu-id="f12a0-128">**Maximális tárolási**</span><span class="sxs-lookup"><span data-stu-id="f12a0-128">**Max storage**</span></span>            |                   |
| <span data-ttu-id="f12a0-129">Alapszintű szolgáltatásszint</span><span class="sxs-lookup"><span data-stu-id="f12a0-129">Basic service tier</span></span>         | <span data-ttu-id="f12a0-130">1 TB</span><span class="sxs-lookup"><span data-stu-id="f12a0-130">1 TB</span></span>              |
| <span data-ttu-id="f12a0-131">Standard szolgáltatásszint</span><span class="sxs-lookup"><span data-stu-id="f12a0-131">Standard service tier</span></span>      | <span data-ttu-id="f12a0-132">1 TB</span><span class="sxs-lookup"><span data-stu-id="f12a0-132">1 TB</span></span>              |

<span data-ttu-id="f12a0-133">Túl sok a kapcsolat elérésekor a következő hibaüzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="f12a0-133">When too many connections are reached, you may receive the following error:</span></span>
> <span data-ttu-id="f12a0-134">1040 (08004). hiba: Túl sok a kapcsolat</span><span class="sxs-lookup"><span data-stu-id="f12a0-134">ERROR 1040 (08004): Too many connections</span></span>

## <a name="preview-functional-limitations"></a><span data-ttu-id="f12a0-135">Előzetes verzió működési korlátozásai:</span><span class="sxs-lookup"><span data-stu-id="f12a0-135">Preview functional limitations:</span></span>
### <a name="scale-operations"></a><span data-ttu-id="f12a0-136">A skálázási műveletek:</span><span class="sxs-lookup"><span data-stu-id="f12a0-136">Scale operations:</span></span>
1.  <span data-ttu-id="f12a0-137">Dinamikus méretezés kiszolgálók között szolgáltatásszintek jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="f12a0-137">Dynamic scaling of servers across service tiers is currently not supported.</span></span> <span data-ttu-id="f12a0-138">Ez azt jelenti, hogy Basic és Standard szolgáltatásszintek közötti váltás.</span><span class="sxs-lookup"><span data-stu-id="f12a0-138">That is, switching between Basic and Standard service tiers.</span></span>
2.  <span data-ttu-id="f12a0-139">Dinamikus igény szerinti növelését tárolási előre létrehozott kiszolgálón jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="f12a0-139">Dynamic on-demand increase of storage on pre-created server is currently not supported.</span></span>
3.  <span data-ttu-id="f12a0-140">Kiszolgáló tároló méretének csökkentése nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="f12a0-140">Decreasing server storage size is not supported.</span></span>

### <a name="server-version-upgrades"></a><span data-ttu-id="f12a0-141">Kiszolgáló verziófrissítések:</span><span class="sxs-lookup"><span data-stu-id="f12a0-141">Server version upgrades:</span></span>
- <span data-ttu-id="f12a0-142">Fő adatbázis motor verziók közötti automatikus áttelepítési jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="f12a0-142">Automated migration between major database engine versions is currently not supported.</span></span>

### <a name="subscription-management"></a><span data-ttu-id="f12a0-143">Előfizetés-kezelés:</span><span class="sxs-lookup"><span data-stu-id="f12a0-143">Subscription management:</span></span>
- <span data-ttu-id="f12a0-144">Dinamikusan áthelyezése előfizetés és az erőforráscsoport előre létrehozott kiszolgálók jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="f12a0-144">Dynamically moving pre-created servers across subscription and resource group is currently not supported.</span></span>

### <a name="point-in-time-restore"></a><span data-ttu-id="f12a0-145">Pont-a--visszaállítás egy korábbi időpontra:</span><span class="sxs-lookup"><span data-stu-id="f12a0-145">Point-in-time-restore:</span></span>
1.  <span data-ttu-id="f12a0-146">Különböző szolgáltatási rétegben és/vagy számítási egység és a tárhely mérete visszaállítása nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="f12a0-146">Restoring to different service tier and/or Compute Units and Storage size is not allowed.</span></span>
2.  <span data-ttu-id="f12a0-147">Az eldobott kiszolgáló visszaállítása nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="f12a0-147">Restoring a dropped server is not supported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f12a0-148">További lépések:</span><span class="sxs-lookup"><span data-stu-id="f12a0-148">Next Steps:</span></span>
<span data-ttu-id="f12a0-149">[Az egyes szolgáltatásszinteken elérhető](concepts-service-tiers.md)
[MySQL-adatbázis verziója támogatott](concepts-supported-versions.md)</span><span class="sxs-lookup"><span data-stu-id="f12a0-149">[What’s available in each service tier](concepts-service-tiers.md)
[Supported MySQL Database Versions](concepts-supported-versions.md)</span></span>
