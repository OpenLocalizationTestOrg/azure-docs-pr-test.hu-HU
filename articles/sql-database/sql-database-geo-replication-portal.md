---
title: "Azure-portálon: SQL Database georeplikációja |} Microsoft Docs"
description: "A georeplikáció konfigurálása az Azure SQL Database az Azure-portálon és kezdeményezhet feladatátvételi"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: d0b29822-714f-4633-a5ab-fb1a09d43ced
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/06/2016
ms.author: carlrab
ms.openlocfilehash: db90fad2fe397f0c8466db6bdc1bd8c8d1cf8f15
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-active-geo-replication-for-azure-sql-database-in-the-azure-portal-and-initiate-failover"></a><span data-ttu-id="fe738-103">Aktív georeplikáció konfigurálása az Azure SQL Database az Azure-portálon és kezdeményezhet feladatátvételi</span><span class="sxs-lookup"><span data-stu-id="fe738-103">Configure active geo-replication for Azure SQL Database in the Azure portal and initiate failover</span></span>

<span data-ttu-id="fe738-104">Ez a cikk bemutatja, hogyan aktív georeplikáció konfigurálja az SQL-adatbázis a [Azure-portálon](http://portal.azure.com) és a feladatátvétel kezdeményezése.</span><span class="sxs-lookup"><span data-stu-id="fe738-104">This article shows you how to configure active geo-replication for SQL Database in the [Azure portal](http://portal.azure.com) and to initiate failover.</span></span>

<span data-ttu-id="fe738-105">Kezdeményezze a feladatátvételt az Azure portálon, lásd: [kezdeményezzen tervezett vagy nem tervezett feladatátvételt az Azure SQL Database és az Azure portál](sql-database-geo-replication-portal.md).</span><span class="sxs-lookup"><span data-stu-id="fe738-105">To initiate failover with the Azure portal, see [Initiate a planned or unplanned failover for Azure SQL Database with the Azure portal](sql-database-geo-replication-portal.md).</span></span>

<span data-ttu-id="fe738-106">Aktív georeplikáció konfigurálása az Azure-portál használatával, a következő erőforrás van szükség:</span><span class="sxs-lookup"><span data-stu-id="fe738-106">To configure active geo-replication by using the Azure portal, you need the following resource:</span></span>

* <span data-ttu-id="fe738-107">Azure SQL-adatbázis: az elsődleges adatbázis egy másik földrajzi régióban replikálni kívánt.</span><span class="sxs-lookup"><span data-stu-id="fe738-107">An Azure SQL database: The primary database that you want to replicate to a different geographical region.</span></span>

> [!Note]
<span data-ttu-id="fe738-108">Aktív georeplikáció ugyanahhoz az előfizetéshez adatbázisok között kell lennie.</span><span class="sxs-lookup"><span data-stu-id="fe738-108">Active geo-replication must be between databases in the same subscription.</span></span>

## <a name="add-a-secondary-database"></a><span data-ttu-id="fe738-109">A másodlagos adatbázis hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fe738-109">Add a secondary database</span></span>
<span data-ttu-id="fe738-110">Az alábbi lépéseket a georeplikáció együttműködve hozzon létre egy új másodlagos adatbázis.</span><span class="sxs-lookup"><span data-stu-id="fe738-110">The following steps create a new secondary database in a geo-replication partnership.</span></span>  

<span data-ttu-id="fe738-111">Vegye fel a másodlagos adatbázist, az előfizetés tulajdonosa vagy a társtulajdonos kell lennie.</span><span class="sxs-lookup"><span data-stu-id="fe738-111">To add a secondary database, you must be the subscription owner or co-owner.</span></span>

<span data-ttu-id="fe738-112">A másodlagos adatbázis ugyanaz a neve, mint az elsődleges adatbázissal, és alapértelmezés szerint rendelkezik ugyanazt a szolgáltatási szint.</span><span class="sxs-lookup"><span data-stu-id="fe738-112">The secondary database has the same name as the primary database and has, by default, the same service level.</span></span> <span data-ttu-id="fe738-113">A másodlagos adatbázis egy adatbázis vagy a rugalmas készletekben található adatbázis lehet.</span><span class="sxs-lookup"><span data-stu-id="fe738-113">The secondary database can be a single database or a database in an elastic pool.</span></span> <span data-ttu-id="fe738-114">További információkért lásd: [szolgáltatásszintek](sql-database-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="fe738-114">For more information, see [Service tiers](sql-database-service-tiers.md).</span></span>
<span data-ttu-id="fe738-115">Miután a másodlagos létrehozták, és a rendezés, adatok kezdődik, az új másodlagos adatbázis replikálása az elsődleges adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="fe738-115">After the secondary is created and seeded, data begins replicating from the primary database to the new secondary database.</span></span>

> [!NOTE]
> <span data-ttu-id="fe738-116">A parancs futása sikertelen, ha a partner adatbázis már létezik (például miatt leállítja egy előző georeplikáció kapcsolat).</span><span class="sxs-lookup"><span data-stu-id="fe738-116">If the partner database already exists (for example, as a result of terminating a previous geo-replication relationship) the command fails.</span></span>
> 

1. <span data-ttu-id="fe738-117">Az a [Azure-portálon](http://portal.azure.com), keresse meg az adatbázis, amelyet a georeplikációért beállítása.</span><span class="sxs-lookup"><span data-stu-id="fe738-117">In the [Azure portal](http://portal.azure.com), browse to the database that you want to set up for geo-replication.</span></span>
2. <span data-ttu-id="fe738-118">Az SQL-adatbázis oldalon válassza ki a **georeplikáció**, majd válassza ki a régiót, a másodlagos adatbázis létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="fe738-118">On the SQL database page, select **geo-replication**, and then select the region to create the secondary database.</span></span> <span data-ttu-id="fe738-119">Kiválaszthatja a régió eltérő a régió, az elsődleges adatbázist üzemeltető, de ajánlott a [párosított régió](../best-practices-availability-paired-regions.md).</span><span class="sxs-lookup"><span data-stu-id="fe738-119">You can select any region other than the region hosting the primary database, but we recommend the [paired region](../best-practices-availability-paired-regions.md).</span></span>
   
    ![Aktív georeplikáció konfigurálása](./media/sql-database-geo-replication-portal/configure-geo-replication.png)
3. <span data-ttu-id="fe738-121">Válasszon, vagy adja meg a kiszolgáló és a másodlagos adatbázis tarifacsomagjának.</span><span class="sxs-lookup"><span data-stu-id="fe738-121">Select or configure the server and pricing tier for the secondary database.</span></span>
   
    ![Másodlagos konfigurálása](./media/sql-database-geo-replication-portal/create-secondary.png)
4. <span data-ttu-id="fe738-123">Opcionálisan hozzáadhat egy másodlagos adatbázis rugalmas készlethez.</span><span class="sxs-lookup"><span data-stu-id="fe738-123">Optionally, you can add a secondary database to an elastic pool.</span></span> <span data-ttu-id="fe738-124">A másodlagos adatbázis létrehozása a készletben, kattintson a **rugalmas készlet** , és válasszon ki egy készletet, a célkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="fe738-124">To create the secondary database in a pool, click **elastic pool** and select a pool on the target server.</span></span> <span data-ttu-id="fe738-125">Egy készlet már léteznie kell a célkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="fe738-125">A pool must already exist on the target server.</span></span> <span data-ttu-id="fe738-126">Ez a munkafolyamat nem hoz létre a készletet.</span><span class="sxs-lookup"><span data-stu-id="fe738-126">This workflow does not create a pool.</span></span>
5. <span data-ttu-id="fe738-127">Kattintson a **létrehozása** hozzáadása a másodlagos.</span><span class="sxs-lookup"><span data-stu-id="fe738-127">Click **Create** to add the secondary.</span></span>
6. <span data-ttu-id="fe738-128">A másodlagos adatbázis jön létre, és az index-összehangolási folyamat megkezdése.</span><span class="sxs-lookup"><span data-stu-id="fe738-128">The secondary database is created and the seeding process begins.</span></span>
   
    ![Másodlagos konfigurálása](./media/sql-database-geo-replication-portal/seeding0.png)
7. <span data-ttu-id="fe738-130">Ha az index-összehangolási folyamat befejeződik, a másodlagos adatbázis állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="fe738-130">When the seeding process is complete, the secondary database displays its status.</span></span>
   
    ![Teljes összehangolása](./media/sql-database-geo-replication-portal/seeding-complete.png)

## <a name="initiate-a-failover"></a><span data-ttu-id="fe738-132">Kezdeményezzen feladatátvételt</span><span class="sxs-lookup"><span data-stu-id="fe738-132">Initiate a failover</span></span>

<span data-ttu-id="fe738-133">A másodlagos adatbázist már kapcsolható legyen, az elsődleges.</span><span class="sxs-lookup"><span data-stu-id="fe738-133">The secondary database can be switched to become the primary.</span></span>  

1. <span data-ttu-id="fe738-134">Az a [Azure-portálon](http://portal.azure.com), keresse meg az elsődleges adatbázis a georeplikáció együttműködve.</span><span class="sxs-lookup"><span data-stu-id="fe738-134">In the [Azure portal](http://portal.azure.com), browse to the primary database in the geo-replication partnership.</span></span>
2. <span data-ttu-id="fe738-135">SQL-adatbázis paneljén válassza **összes beállítás** > **georeplikáció**.</span><span class="sxs-lookup"><span data-stu-id="fe738-135">On the SQL Database blade, select **All settings** > **geo-replication**.</span></span>
3. <span data-ttu-id="fe738-136">Az a **másodlagos** listára, válassza ki az adatbázist az új elsődleges válik, és kattintson a kívánt **feladatátvételi**.</span><span class="sxs-lookup"><span data-stu-id="fe738-136">In the **SECONDARIES** list, select the database you want to become the new primary and click **Failover**.</span></span>
   
    ![feladatátvétel](./media/sql-database-geo-replication-failover-portal/secondaries.png)
4. <span data-ttu-id="fe738-138">Kattintson a **Igen** megkezdéséhez a feladatátvételt.</span><span class="sxs-lookup"><span data-stu-id="fe738-138">Click **Yes** to begin the failover.</span></span>

<span data-ttu-id="fe738-139">A parancs azonnal vált, a másodlagos adatbázist az elsődleges szerepkör.</span><span class="sxs-lookup"><span data-stu-id="fe738-139">The command immediately switches the secondary database into the primary role.</span></span> 

<span data-ttu-id="fe738-140">Egy rövid időre időintervalluma (terabájtok rendelése 0-25 másodpercig) nem érhető el, amíg a szerepkörök bekapcsolt állapotban van.</span><span class="sxs-lookup"><span data-stu-id="fe738-140">There is a short period during which both databases are unavailable (on the order of 0 to 25 seconds) while the roles are switched.</span></span> <span data-ttu-id="fe738-141">Ha az elsődleges adatbázis több másodlagos adatbázist, a parancs automatikusan újrakonfigurálja a más másodlagos adatbázist az új elsődleges való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="fe738-141">If the primary database has multiple secondary databases, the command automatically reconfigures the other secondaries to connect to the new primary.</span></span> <span data-ttu-id="fe738-142">A teljes műveletet kell végrehajtani egy percen belül befejeződik normál körülmények között.</span><span class="sxs-lookup"><span data-stu-id="fe738-142">The entire operation should take less than a minute to complete under normal circumstances.</span></span> 

> [!NOTE]
> <span data-ttu-id="fe738-143">Ez a parancs a gyors helyreállítás az adatbázis egy esetleges leállás esetén szolgál.</span><span class="sxs-lookup"><span data-stu-id="fe738-143">This command is designed for quick recovery of the database in case of an outage.</span></span> <span data-ttu-id="fe738-144">Elindítja a feladatátvétel (kényszerített feladatátvételi) adatok szinkronizálás nélkül.</span><span class="sxs-lookup"><span data-stu-id="fe738-144">It triggers failover without data synchronization (forced failover).</span></span>  <span data-ttu-id="fe738-145">Ha az elsődleges online állapotban, és a tranzakciók végrehajtása, ha a parancs kiadása adatvesztést fordulhat elő.</span><span class="sxs-lookup"><span data-stu-id="fe738-145">If the primary is online and committing transactions when the command is issued some data loss may occur.</span></span> 
> 
> 

## <a name="remove-secondary-database"></a><span data-ttu-id="fe738-146">Távolítsa el a másodlagos adatbázis</span><span class="sxs-lookup"><span data-stu-id="fe738-146">Remove secondary database</span></span>
<span data-ttu-id="fe738-147">Ez a művelet végleg leáll a replikálás, a másodlagos adatbázishoz, és a szerepkör a másodlagos vált rendszeres írható-olvasható adatbázis.</span><span class="sxs-lookup"><span data-stu-id="fe738-147">This operation permanently terminates the replication to the secondary database, and changes the role of the secondary to a regular read-write database.</span></span> <span data-ttu-id="fe738-148">Ha a másodlagos adatbázis-kapcsolata megszakad, a parancs végrehajtása sikeres, de a másodlagos nem írható-olvasható kapcsolat után nem lett helyreáll.</span><span class="sxs-lookup"><span data-stu-id="fe738-148">If the connectivity to the secondary database is broken, the command succeeds but the secondary does not become read-write until after connectivity is restored.</span></span>  

1. <span data-ttu-id="fe738-149">Az a [Azure-portálon](http://portal.azure.com), keresse meg az elsődleges adatbázis a georeplikáció együttműködve.</span><span class="sxs-lookup"><span data-stu-id="fe738-149">In the [Azure portal](http://portal.azure.com), browse to the primary database in the geo-replication partnership.</span></span>
2. <span data-ttu-id="fe738-150">Az SQL-adatbázis oldalon válassza ki a **georeplikáció**.</span><span class="sxs-lookup"><span data-stu-id="fe738-150">On the SQL database page, select **geo-replication**.</span></span>
3. <span data-ttu-id="fe738-151">Az a **másodlagos** listára, válassza ki a georeplikáció partnerség eltávolítása kívánt adatbázist.</span><span class="sxs-lookup"><span data-stu-id="fe738-151">In the **SECONDARIES** list, select the database you want to remove from the geo-replication partnership.</span></span>
4. <span data-ttu-id="fe738-152">Kattintson a **állítsa le a replikációt**.</span><span class="sxs-lookup"><span data-stu-id="fe738-152">Click **Stop Replication**.</span></span>
   
    ![Távolítsa el a másodlagos](./media/sql-database-geo-replication-portal/remove-secondary.png)
5. <span data-ttu-id="fe738-154">Megnyílik egy ablak.</span><span class="sxs-lookup"><span data-stu-id="fe738-154">A confirmation window opens.</span></span> <span data-ttu-id="fe738-155">Kattintson a **Igen** az adatbázis eltávolítása a georeplikáció partneri kapcsolat áll fenn.</span><span class="sxs-lookup"><span data-stu-id="fe738-155">Click **Yes** to remove the database from the geo-replication partnership.</span></span> <span data-ttu-id="fe738-156">(Állítsa írható-olvasható adatbázis nem része semmilyen replikációs.)</span><span class="sxs-lookup"><span data-stu-id="fe738-156">(Set it to a read-write database not part of any replication.)</span></span>

## <a name="next-steps"></a><span data-ttu-id="fe738-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fe738-157">Next steps</span></span>
* <span data-ttu-id="fe738-158">Aktív georeplikáció kapcsolatos további információkért lásd: [aktív georeplikáció](sql-database-geo-replication-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fe738-158">To learn more about active geo-replication, see [active geo-replication](sql-database-geo-replication-overview.md).</span></span>
* <span data-ttu-id="fe738-159">Egy üzleti folytonosság – áttekintés és forgatókönyvek: [üzleti folytonosság – áttekintés](sql-database-business-continuity.md).</span><span class="sxs-lookup"><span data-stu-id="fe738-159">For a business continuity overview and scenarios, see [Business continuity overview](sql-database-business-continuity.md).</span></span>

