---
title: "Azure-portálon: SQL Database georeplikációja |} Microsoft Docs"
description: "Georeplikáció konfigurálása az Azure SQL Database hello Azure-portálon és kezdeményezhet feladatátvevő"
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
ms.openlocfilehash: 09cbbdb040f36c42593e3be87ce6db2238f36656
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-active-geo-replication-for-azure-sql-database-in-hello-azure-portal-and-initiate-failover"></a><span data-ttu-id="ccec6-103">Aktív georeplikáció konfigurálása az Azure SQL Database hello Azure-portálon és kezdeményezhet feladatátvételi</span><span class="sxs-lookup"><span data-stu-id="ccec6-103">Configure active geo-replication for Azure SQL Database in hello Azure portal and initiate failover</span></span>

<span data-ttu-id="ccec6-104">Ez a cikk bemutatja, hogyan tooconfigure aktív georeplikáció SQL-adatbázis a hello [Azure-portálon](http://portal.azure.com) és tooinitiate feladatátvételi.</span><span class="sxs-lookup"><span data-stu-id="ccec6-104">This article shows you how tooconfigure active geo-replication for SQL Database in hello [Azure portal](http://portal.azure.com) and tooinitiate failover.</span></span>

<span data-ttu-id="ccec6-105">hello Azure-portálon tooinitiate feladatátvétel lásd [kezdeményezze a tervezett vagy nem tervezett feladatátvétel az Azure SQL Database hello Azure-portálon](sql-database-geo-replication-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ccec6-105">tooinitiate failover with hello Azure portal, see [Initiate a planned or unplanned failover for Azure SQL Database with hello Azure portal](sql-database-geo-replication-portal.md).</span></span>

<span data-ttu-id="ccec6-106">tooconfigure aktív georeplikáció hello Azure-portál használatával, a következő erőforrás hello kell:</span><span class="sxs-lookup"><span data-stu-id="ccec6-106">tooconfigure active geo-replication by using hello Azure portal, you need hello following resource:</span></span>

* <span data-ttu-id="ccec6-107">Azure SQL-adatbázis: hello elsődleges adatbázis, amelyet az tooreplicate tooa különböző földrajzi régióban.</span><span class="sxs-lookup"><span data-stu-id="ccec6-107">An Azure SQL database: hello primary database that you want tooreplicate tooa different geographical region.</span></span>

> [!Note]
<span data-ttu-id="ccec6-108">Aktív georeplikáció hello adatbázisok közé kell esnie ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="ccec6-108">Active geo-replication must be between databases in hello same subscription.</span></span>

## <a name="add-a-secondary-database"></a><span data-ttu-id="ccec6-109">A másodlagos adatbázis hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ccec6-109">Add a secondary database</span></span>
<span data-ttu-id="ccec6-110">hello lépések hozzon létre egy új másodlagos adatbázis egy georeplikáció együttműködve.</span><span class="sxs-lookup"><span data-stu-id="ccec6-110">hello following steps create a new secondary database in a geo-replication partnership.</span></span>  

<span data-ttu-id="ccec6-111">egy másodlagos adatbázis tooadd hello előfizetés tulajdonosa vagy a társtulajdonos kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ccec6-111">tooadd a secondary database, you must be hello subscription owner or co-owner.</span></span>

<span data-ttu-id="ccec6-112">hello másodlagos adatbázis hello azonos nevet hello elsődleges adatbázisként van, és alapértelmezés szerint rendelkezik, hello azonos.</span><span class="sxs-lookup"><span data-stu-id="ccec6-112">hello secondary database has hello same name as hello primary database and has, by default, hello same service level.</span></span> <span data-ttu-id="ccec6-113">hello másodlagos adatbázis egy adatbázis vagy a rugalmas készletekben található adatbázis lehet.</span><span class="sxs-lookup"><span data-stu-id="ccec6-113">hello secondary database can be a single database or a database in an elastic pool.</span></span> <span data-ttu-id="ccec6-114">További információkért lásd: [szolgáltatásszintek](sql-database-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="ccec6-114">For more information, see [Service tiers](sql-database-service-tiers.md).</span></span>
<span data-ttu-id="ccec6-115">Miután másodlagos hello létrehozták, és a rendezés, az adatok kezdete hello elsődleges adatbázis toohello új másodlagos adatbázis replikálásához.</span><span class="sxs-lookup"><span data-stu-id="ccec6-115">After hello secondary is created and seeded, data begins replicating from hello primary database toohello new secondary database.</span></span>

> [!NOTE]
> <span data-ttu-id="ccec6-116">Ha hello partner adatbázis már létezik (például miatt leállítja egy előző georeplikáció kapcsolat) hello parancs sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="ccec6-116">If hello partner database already exists (for example, as a result of terminating a previous geo-replication relationship) hello command fails.</span></span>
> 

1. <span data-ttu-id="ccec6-117">A hello [Azure-portálon](http://portal.azure.com), keresse meg, hogy szeretné-e az georeplikáció szolgáltatáshoz tooset toohello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="ccec6-117">In hello [Azure portal](http://portal.azure.com), browse toohello database that you want tooset up for geo-replication.</span></span>
2. <span data-ttu-id="ccec6-118">Hello SQL-adatbázis lapján válassza ki a **georeplikáció**, majd válassza ki a hello régió toocreate hello másodlagos adatbázist.</span><span class="sxs-lookup"><span data-stu-id="ccec6-118">On hello SQL database page, select **geo-replication**, and then select hello region toocreate hello secondary database.</span></span> <span data-ttu-id="ccec6-119">Kiválaszthatja, hogy minden régióban hello régió hello elsődleges adatbázist üzemeltető, de azt javasoljuk, hogy hello [párosított régió](../best-practices-availability-paired-regions.md).</span><span class="sxs-lookup"><span data-stu-id="ccec6-119">You can select any region other than hello region hosting hello primary database, but we recommend hello [paired region](../best-practices-availability-paired-regions.md).</span></span>
   
    ![Aktív georeplikáció konfigurálása](./media/sql-database-geo-replication-portal/configure-geo-replication.png)
3. <span data-ttu-id="ccec6-121">Válassza ki, vagy konfigurálja a kiszolgáló hello és hello másodlagos adatbázis tarifacsomagjának.</span><span class="sxs-lookup"><span data-stu-id="ccec6-121">Select or configure hello server and pricing tier for hello secondary database.</span></span>
   
    ![Másodlagos konfigurálása](./media/sql-database-geo-replication-portal/create-secondary.png)
4. <span data-ttu-id="ccec6-123">Ha szükséges egy másodlagos adatbázis-tooan rugalmas készletet is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="ccec6-123">Optionally, you can add a secondary database tooan elastic pool.</span></span> <span data-ttu-id="ccec6-124">toocreate hello másodlagos adatbázis egy tárolókészletben, kattintson a **rugalmas készlet** , és válasszon ki egy készletet hello cél kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="ccec6-124">toocreate hello secondary database in a pool, click **elastic pool** and select a pool on hello target server.</span></span> <span data-ttu-id="ccec6-125">Egy készlet hello célkiszolgálón már léteznie kell.</span><span class="sxs-lookup"><span data-stu-id="ccec6-125">A pool must already exist on hello target server.</span></span> <span data-ttu-id="ccec6-126">Ez a munkafolyamat nem hoz létre a készletet.</span><span class="sxs-lookup"><span data-stu-id="ccec6-126">This workflow does not create a pool.</span></span>
5. <span data-ttu-id="ccec6-127">Kattintson a **létrehozása** tooadd hello másodlagos.</span><span class="sxs-lookup"><span data-stu-id="ccec6-127">Click **Create** tooadd hello secondary.</span></span>
6. <span data-ttu-id="ccec6-128">hello másodlagos adatbázis jön létre, és a folyamat összehangolása hello kezdődik.</span><span class="sxs-lookup"><span data-stu-id="ccec6-128">hello secondary database is created and hello seeding process begins.</span></span>
   
    ![Másodlagos konfigurálása](./media/sql-database-geo-replication-portal/seeding0.png)
7. <span data-ttu-id="ccec6-130">Hello összehangolása a folyamat befejeződése után a hello másodlagos adatbázis állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="ccec6-130">When hello seeding process is complete, hello secondary database displays its status.</span></span>
   
    ![Teljes összehangolása](./media/sql-database-geo-replication-portal/seeding-complete.png)

## <a name="initiate-a-failover"></a><span data-ttu-id="ccec6-132">Kezdeményezzen feladatátvételt</span><span class="sxs-lookup"><span data-stu-id="ccec6-132">Initiate a failover</span></span>

<span data-ttu-id="ccec6-133">hello másodlagos adatbázis elsődleges kapcsolt toobecome hello lehet.</span><span class="sxs-lookup"><span data-stu-id="ccec6-133">hello secondary database can be switched toobecome hello primary.</span></span>  

1. <span data-ttu-id="ccec6-134">A hello [Azure-portálon](http://portal.azure.com), keresse meg az elsődleges adatbázis toohello hello georeplikáció együttműködve.</span><span class="sxs-lookup"><span data-stu-id="ccec6-134">In hello [Azure portal](http://portal.azure.com), browse toohello primary database in hello geo-replication partnership.</span></span>
2. <span data-ttu-id="ccec6-135">Hello SQL-adatbázis paneljén válassza **összes beállítás** > **georeplikáció**.</span><span class="sxs-lookup"><span data-stu-id="ccec6-135">On hello SQL Database blade, select **All settings** > **geo-replication**.</span></span>
3. <span data-ttu-id="ccec6-136">A hello **másodlagos** lista, jelölje be hello adatbázis toobecome hello új elsődleges, és kattintson a **feladatátvételi**.</span><span class="sxs-lookup"><span data-stu-id="ccec6-136">In hello **SECONDARIES** list, select hello database you want toobecome hello new primary and click **Failover**.</span></span>
   
    ![feladatátvétel](./media/sql-database-geo-replication-failover-portal/secondaries.png)
4. <span data-ttu-id="ccec6-138">Kattintson a **Igen** toobegin hello feladatátvételi.</span><span class="sxs-lookup"><span data-stu-id="ccec6-138">Click **Yes** toobegin hello failover.</span></span>

<span data-ttu-id="ccec6-139">hello parancs azonnal kapcsolók hello másodlagos adatbázis hello elsődleges szerepkörhöz.</span><span class="sxs-lookup"><span data-stu-id="ccec6-139">hello command immediately switches hello secondary database into hello primary role.</span></span> 

<span data-ttu-id="ccec6-140">Egy rövid időszak során, ami két adatbázis nem érhető el (a 0 too25 másodperc hello sorrendben) közben hello szerepkörök bekapcsolt állapotban van.</span><span class="sxs-lookup"><span data-stu-id="ccec6-140">There is a short period during which both databases are unavailable (on hello order of 0 too25 seconds) while hello roles are switched.</span></span> <span data-ttu-id="ccec6-141">Ha a hello elsődleges adatbázis több másodlagos adatbázist, hello parancs automatikusan Átkonfigurálás hello más másodlagos tooconnect toohello új elsődleges.</span><span class="sxs-lookup"><span data-stu-id="ccec6-141">If hello primary database has multiple secondary databases, hello command automatically reconfigures hello other secondaries tooconnect toohello new primary.</span></span> <span data-ttu-id="ccec6-142">hello teljes műveletet kell végrehajtani kisebb, mint egy perc toocomplete normál körülmények között.</span><span class="sxs-lookup"><span data-stu-id="ccec6-142">hello entire operation should take less than a minute toocomplete under normal circumstances.</span></span> 

> [!NOTE]
> <span data-ttu-id="ccec6-143">Ez a parancs a gyors helyreállításának hello adatbázis egy esetleges leállás esetén szolgál.</span><span class="sxs-lookup"><span data-stu-id="ccec6-143">This command is designed for quick recovery of hello database in case of an outage.</span></span> <span data-ttu-id="ccec6-144">Elindítja a feladatátvétel (kényszerített feladatátvételi) adatok szinkronizálás nélkül.</span><span class="sxs-lookup"><span data-stu-id="ccec6-144">It triggers failover without data synchronization (forced failover).</span></span>  <span data-ttu-id="ccec6-145">Ha elsődleges hello online állapotban, és a tranzakciók véglegesítése, amikor hello parancs adatvesztést fordulhat elő.</span><span class="sxs-lookup"><span data-stu-id="ccec6-145">If hello primary is online and committing transactions when hello command is issued some data loss may occur.</span></span> 
> 
> 

## <a name="remove-secondary-database"></a><span data-ttu-id="ccec6-146">Távolítsa el a másodlagos adatbázis</span><span class="sxs-lookup"><span data-stu-id="ccec6-146">Remove secondary database</span></span>
<span data-ttu-id="ccec6-147">Ez a művelet végleg megszakítja hello replikációs toohello másodlagos adatbázis, és módosításokat hello hello másodlagos tooa rendszeres olvasási és írási adatbázis szerepe.</span><span class="sxs-lookup"><span data-stu-id="ccec6-147">This operation permanently terminates hello replication toohello secondary database, and changes hello role of hello secondary tooa regular read-write database.</span></span> <span data-ttu-id="ccec6-148">Ha hello kapcsolat toohello másodlagos adatbázis nem működő, hello parancs sikeresen befejeződik, de hello másodlagos biztosítja, nem lett írható-olvasható, amíg a kapcsolat helyreállítását követő rögzítésénél.</span><span class="sxs-lookup"><span data-stu-id="ccec6-148">If hello connectivity toohello secondary database is broken, hello command succeeds but hello secondary does not become read-write until after connectivity is restored.</span></span>  

1. <span data-ttu-id="ccec6-149">A hello [Azure-portálon](http://portal.azure.com), keresse meg az elsődleges adatbázis toohello hello georeplikáció együttműködve.</span><span class="sxs-lookup"><span data-stu-id="ccec6-149">In hello [Azure portal](http://portal.azure.com), browse toohello primary database in hello geo-replication partnership.</span></span>
2. <span data-ttu-id="ccec6-150">Hello SQL-adatbázis lapján válassza ki a **georeplikáció**.</span><span class="sxs-lookup"><span data-stu-id="ccec6-150">On hello SQL database page, select **geo-replication**.</span></span>
3. <span data-ttu-id="ccec6-151">A hello **másodlagos** lista, jelölje be hello adatbázis tooremove hello georeplikáció partneri kapcsolat áll fenn a kívánt.</span><span class="sxs-lookup"><span data-stu-id="ccec6-151">In hello **SECONDARIES** list, select hello database you want tooremove from hello geo-replication partnership.</span></span>
4. <span data-ttu-id="ccec6-152">Kattintson a **állítsa le a replikációt**.</span><span class="sxs-lookup"><span data-stu-id="ccec6-152">Click **Stop Replication**.</span></span>
   
    ![Távolítsa el a másodlagos](./media/sql-database-geo-replication-portal/remove-secondary.png)
5. <span data-ttu-id="ccec6-154">Megnyílik egy ablak.</span><span class="sxs-lookup"><span data-stu-id="ccec6-154">A confirmation window opens.</span></span> <span data-ttu-id="ccec6-155">Kattintson a **Igen** tooremove hello adatbázis hello georeplikáció partneri kapcsolat áll fenn.</span><span class="sxs-lookup"><span data-stu-id="ccec6-155">Click **Yes** tooremove hello database from hello geo-replication partnership.</span></span> <span data-ttu-id="ccec6-156">(Állítani tooa olvasási és írási adatbázis nem része semmilyen replikációs.)</span><span class="sxs-lookup"><span data-stu-id="ccec6-156">(Set it tooa read-write database not part of any replication.)</span></span>

## <a name="next-steps"></a><span data-ttu-id="ccec6-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ccec6-157">Next steps</span></span>
* <span data-ttu-id="ccec6-158">További információ az aktív georeplikáció, toolearn lásd [aktív georeplikáció](sql-database-geo-replication-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ccec6-158">toolearn more about active geo-replication, see [active geo-replication](sql-database-geo-replication-overview.md).</span></span>
* <span data-ttu-id="ccec6-159">Egy üzleti folytonosság – áttekintés és forgatókönyvek: [üzleti folytonosság – áttekintés](sql-database-business-continuity.md).</span><span class="sxs-lookup"><span data-stu-id="ccec6-159">For a business continuity overview and scenarios, see [Business continuity overview](sql-database-business-continuity.md).</span></span>

