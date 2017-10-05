---
title: "PostgreSQL-adatbázisban az Azure korlátozásai |} Microsoft Docs"
description: "Azure-adatbázis korlátozásai PostgreSQL ismerteti."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.topic: article
ms.date: 06/01/2017
ms.openlocfilehash: 38988fc5c0dc05331ea078534cd1a05e9eca2493
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="limitations-in-azure-database-for-postgresql"></a><span data-ttu-id="8ee18-103">Az Azure-adatbázis PostgreSQL korlátozásai</span><span class="sxs-lookup"><span data-stu-id="8ee18-103">Limitations in Azure Database for PostgreSQL</span></span>
<span data-ttu-id="8ee18-104">Az Azure-adatbázis PostgreSQL szolgáltatás nyilvános előzetes verzió van.</span><span class="sxs-lookup"><span data-stu-id="8ee18-104">The Azure Database for PostgreSQL service is in public preview.</span></span> <span data-ttu-id="8ee18-105">A következő szakaszok ismertetik a kapacitás és az adatbázis szolgáltatásban működik korlátok.</span><span class="sxs-lookup"><span data-stu-id="8ee18-105">The following sections describe capacity and functional limits in the database service.</span></span>

## <a name="service-tier-maximums"></a><span data-ttu-id="8ee18-106">Szolgáltatási szint méretkorlát</span><span class="sxs-lookup"><span data-stu-id="8ee18-106">Service Tier Maximums</span></span>
<span data-ttu-id="8ee18-107">Azure PostgreSQL-adatbázishoz a kiszolgáló létrehozása választhat több szolgáltatásszinttel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="8ee18-107">Azure Database for PostgreSQL has multiple service tiers you can choose from when creating a server.</span></span> <span data-ttu-id="8ee18-108">További információkért lásd: [az egyes szolgáltatásszinteken elérhető](concepts-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="8ee18-108">For more information, see [Understand what’s available in each service tier](concepts-service-tiers.md).</span></span>  

<span data-ttu-id="8ee18-109">Nincs kapcsolatok, a számítási egység és a tárolás, az egyes szolgáltatásszintek tartalmának maximális száma a szolgáltatás előzetes az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="8ee18-109">There is a maximum number of connections, compute units, and storage in each service tier during the service preview, as follows:</span></span> 

|                            |                   |
| :------------------------- | :---------------- |
| <span data-ttu-id="8ee18-110">**Kapcsolatok maximális száma**</span><span class="sxs-lookup"><span data-stu-id="8ee18-110">**Max connections**</span></span>        |                   |
| <span data-ttu-id="8ee18-111">Alapszintű 50 számítási egység</span><span class="sxs-lookup"><span data-stu-id="8ee18-111">Basic 50 Compute Units</span></span>     | <span data-ttu-id="8ee18-112">50 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="8ee18-112">50 connections</span></span>    |
| <span data-ttu-id="8ee18-113">Alapszintű 100 számítási egység</span><span class="sxs-lookup"><span data-stu-id="8ee18-113">Basic 100 Compute Units</span></span>    | <span data-ttu-id="8ee18-114">100 kapcsolatok száma</span><span class="sxs-lookup"><span data-stu-id="8ee18-114">100 connections</span></span>   |
| <span data-ttu-id="8ee18-115">Standard 100 számítási egység</span><span class="sxs-lookup"><span data-stu-id="8ee18-115">Standard 100 Compute Units</span></span> | <span data-ttu-id="8ee18-116">200 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="8ee18-116">200 connections</span></span>   |
| <span data-ttu-id="8ee18-117">Standard 200 számítási egység</span><span class="sxs-lookup"><span data-stu-id="8ee18-117">Standard 200 Compute Units</span></span> | <span data-ttu-id="8ee18-118">300 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="8ee18-118">300 connections</span></span>   |
| <span data-ttu-id="8ee18-119">Standard 400 számítási egység</span><span class="sxs-lookup"><span data-stu-id="8ee18-119">Standard 400 Compute Units</span></span> | <span data-ttu-id="8ee18-120">400 kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="8ee18-120">400 connections</span></span>   |
| <span data-ttu-id="8ee18-121">Standard 800 számítási egység</span><span class="sxs-lookup"><span data-stu-id="8ee18-121">Standard 800 Compute Units</span></span> | <span data-ttu-id="8ee18-122">500 kapcsolatok száma</span><span class="sxs-lookup"><span data-stu-id="8ee18-122">500 connections</span></span>   |
| <span data-ttu-id="8ee18-123">**Maximális számítási egység**</span><span class="sxs-lookup"><span data-stu-id="8ee18-123">**Max Compute Units**</span></span>      |                   |
| <span data-ttu-id="8ee18-124">Alapszintű szolgáltatásszint</span><span class="sxs-lookup"><span data-stu-id="8ee18-124">Basic service tier</span></span>         | <span data-ttu-id="8ee18-125">100 számítási egység</span><span class="sxs-lookup"><span data-stu-id="8ee18-125">100 Compute Units</span></span> |
| <span data-ttu-id="8ee18-126">Standard szolgáltatásszint</span><span class="sxs-lookup"><span data-stu-id="8ee18-126">Standard service tier</span></span>      | <span data-ttu-id="8ee18-127">800 számítási egység</span><span class="sxs-lookup"><span data-stu-id="8ee18-127">800 Compute Units</span></span> |
| <span data-ttu-id="8ee18-128">**Maximális tárolási**</span><span class="sxs-lookup"><span data-stu-id="8ee18-128">**Max storage**</span></span>            |                   |
| <span data-ttu-id="8ee18-129">Alapszintű szolgáltatásszint</span><span class="sxs-lookup"><span data-stu-id="8ee18-129">Basic service tier</span></span>         | <span data-ttu-id="8ee18-130">1 TB</span><span class="sxs-lookup"><span data-stu-id="8ee18-130">1 TB</span></span>              |
| <span data-ttu-id="8ee18-131">Standard szolgáltatásszint</span><span class="sxs-lookup"><span data-stu-id="8ee18-131">Standard service tier</span></span>      | <span data-ttu-id="8ee18-132">1 TB</span><span class="sxs-lookup"><span data-stu-id="8ee18-132">1 TB</span></span>              |

<span data-ttu-id="8ee18-133">Túl sok a kapcsolat elérésekor a következő hibaüzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="8ee18-133">When too many connections are reached, you may receive the following error:</span></span>
> <span data-ttu-id="8ee18-134">Súlyos hiba: sajnos már túl sok ügyfél</span><span class="sxs-lookup"><span data-stu-id="8ee18-134">FATAL:  sorry, too many clients already</span></span>

## <a name="preview-functional-limitations"></a><span data-ttu-id="8ee18-135">Előzetes verzió működési korlátozásai</span><span class="sxs-lookup"><span data-stu-id="8ee18-135">Preview functional limitations</span></span>
### <a name="scale-operations"></a><span data-ttu-id="8ee18-136">A skálázási műveletek</span><span class="sxs-lookup"><span data-stu-id="8ee18-136">Scale operations</span></span>
1.  <span data-ttu-id="8ee18-137">Dinamikus méretezés kiszolgálók között szolgáltatásszintek jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="8ee18-137">Dynamic scaling of servers across service tiers is currently not supported.</span></span> <span data-ttu-id="8ee18-138">Ez azt jelenti, hogy Basic és Standard szolgáltatásszintek közötti váltás.</span><span class="sxs-lookup"><span data-stu-id="8ee18-138">That is, switching between Basic and Standard service tiers.</span></span>
2.  <span data-ttu-id="8ee18-139">Dinamikus igény szerinti növelését tárolási előre létrehozott kiszolgálón jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="8ee18-139">Dynamic on-demand increase of storage on pre-created server is currently not supported.</span></span>
3.  <span data-ttu-id="8ee18-140">Kiszolgáló tároló méretének csökkentése nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="8ee18-140">Decreasing server storage size is not supported.</span></span>

### <a name="server-version-upgrades"></a><span data-ttu-id="8ee18-141">Kiszolgáló verziófrissítések</span><span class="sxs-lookup"><span data-stu-id="8ee18-141">Server version upgrades</span></span>
- <span data-ttu-id="8ee18-142">Fő adatbázis motor verziók közötti automatikus áttelepítési jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="8ee18-142">Automated migration between major database engine versions is currently not supported.</span></span>

### <a name="subscription-management"></a><span data-ttu-id="8ee18-143">Előfizetés-kezelés</span><span class="sxs-lookup"><span data-stu-id="8ee18-143">Subscription management</span></span>
- <span data-ttu-id="8ee18-144">Dinamikusan áthelyezése előfizetés és az erőforráscsoport előre létrehozott kiszolgálók jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="8ee18-144">Dynamically moving pre-created servers across subscription and resource group is currently not supported.</span></span>

### <a name="point-in-time-restore"></a><span data-ttu-id="8ee18-145">Időponthoz kötött visszaállítás</span><span class="sxs-lookup"><span data-stu-id="8ee18-145">Point-in-time-restore</span></span>
1.  <span data-ttu-id="8ee18-146">Különböző szolgáltatási rétegben és/vagy számítási egység és a tárhely mérete visszaállítása nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="8ee18-146">Restoring to different service tier and/or Compute Units and Storage size is not allowed.</span></span>
2.  <span data-ttu-id="8ee18-147">Az eldobott kiszolgáló visszaállítása nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="8ee18-147">Restoring a dropped server is not supported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8ee18-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8ee18-148">Next steps</span></span>
- <span data-ttu-id="8ee18-149">Megértéséhez [egyes tarifacsomagok elérhető](concepts-service-tiers.md)</span><span class="sxs-lookup"><span data-stu-id="8ee18-149">Understand [What’s available in each pricing tier](concepts-service-tiers.md)</span></span>
- <span data-ttu-id="8ee18-150">Megértéséhez [támogatott PostgreSQL-adatbázis verziója](concepts-supported-versions.md)</span><span class="sxs-lookup"><span data-stu-id="8ee18-150">Understand [Supported PostgreSQL Database Versions](concepts-supported-versions.md)</span></span>
- <span data-ttu-id="8ee18-151">Felülvizsgálati [készítsen biztonsági másolatot, és a kiszolgáló Azure-adatbázis visszaállítása az Azure portál használatával PostgreSQL](howto-restore-server-portal.md)</span><span class="sxs-lookup"><span data-stu-id="8ee18-151">Review [How To Back up and Restore a server in Azure Database for PostgreSQL using the Azure portal](howto-restore-server-portal.md)</span></span>
