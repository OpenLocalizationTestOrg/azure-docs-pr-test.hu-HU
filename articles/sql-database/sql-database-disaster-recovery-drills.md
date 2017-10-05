---
title: "SQL adatbázis Vészhelyreállítások részletezésének |} Microsoft Docs"
description: "További útmutatás és ajánlott eljárások az Azure SQL Database segítségével hajtsa végre a vészhelyreállítások részletezésének védheti a kritikus kritikus hibák és a kimaradások esetén refs üzleti alkalmazások."
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: b44a269c-fe2a-404f-b013-290030860bd1
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 07/31/2016
ms.author: sashan
ms.openlocfilehash: 1b1d65a41a794a566287dcffe3581ac58e2a965f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="performing-disaster-recovery-drill"></a><span data-ttu-id="1d30f-103">Vész-helyreállítási részletezési végrehajtása</span><span class="sxs-lookup"><span data-stu-id="1d30f-103">Performing Disaster Recovery Drill</span></span>
<span data-ttu-id="1d30f-104">Javasoljuk, hogy az alkalmazás felkészültségét helyreállítási munkafolyamat érvényesség rendszeres időközönként.</span><span class="sxs-lookup"><span data-stu-id="1d30f-104">It is recommended that validation of application readiness for recovery workflow is performed periodically.</span></span> <span data-ttu-id="1d30f-105">Az alkalmazás viselkedését, és a megvalósítását ellenőrzése az adatvesztéssel és/vagy a problémákat, hogy a feladatátvétel magában foglalja a mérnöki célszerű.</span><span class="sxs-lookup"><span data-stu-id="1d30f-105">Verifying the application behavior and implications of data loss and/or the disruption that failover involves is a good engineering practice.</span></span> <span data-ttu-id="1d30f-106">A legtöbb iparági normák szerint üzleti folytonossági hitelesítő részeként követelmény egyben.</span><span class="sxs-lookup"><span data-stu-id="1d30f-106">It is also a requirement by most industry standards as part of business continuity certification.</span></span>

<span data-ttu-id="1d30f-107">A vész-helyreállítási részletezési végrehajtása áll:</span><span class="sxs-lookup"><span data-stu-id="1d30f-107">Performing a disaster recovery drill consists of:</span></span>

* <span data-ttu-id="1d30f-108">Amelyek adatok réteg leállás</span><span class="sxs-lookup"><span data-stu-id="1d30f-108">Simulating data tier outage</span></span>
* <span data-ttu-id="1d30f-109">Helyreállítása</span><span class="sxs-lookup"><span data-stu-id="1d30f-109">Recovering</span></span>
* <span data-ttu-id="1d30f-110">Alkalmazás integritási utáni helyreállítás ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="1d30f-110">Validate application integrity post recovery</span></span>

<span data-ttu-id="1d30f-111">Attól függően, hogy hogyan meg [az alkalmazás az üzletmenet folytonossága érdekében tervezett](sql-database-business-continuity.md), a munkafolyamat végrehajtása a részletezési eltérőek lehetnek.</span><span class="sxs-lookup"><span data-stu-id="1d30f-111">Depending on how you [designed your application for business continuity](sql-database-business-continuity.md), the workflow to execute the drill can vary.</span></span> <span data-ttu-id="1d30f-112">Az alábbiakban azt írják le az ajánlott eljárások végrehajtása egy vész-helyreállítási részletezéshez Azure SQL adatbázis környezetében.</span><span class="sxs-lookup"><span data-stu-id="1d30f-112">Below we describe the best practices conducting a disaster recovery drill in the context of Azure SQL Database.</span></span>

## <a name="geo-restore"></a><span data-ttu-id="1d30f-113">Georedundáns helyreállítás</span><span class="sxs-lookup"><span data-stu-id="1d30f-113">Geo-restore</span></span>
<span data-ttu-id="1d30f-114">A vész-helyreállítási részletezési végző az esetleges adatvesztés elkerülése ajánlott másolat készítését az üzemi környezetben, és felhasználja az alkalmazás feladatátvételi munkafolyamat ellenőrizze egy tesztkörnyezetben használva részletezési végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="1d30f-114">To prevent the potential data loss when conducting a disaster recovery drill, we recommend performing the drill using a test environment by creating a copy of the production environment and using it to verify the application’s failover workflow.</span></span>

#### <a name="outage-simulation"></a><span data-ttu-id="1d30f-115">Kimaradás szimulálása</span><span class="sxs-lookup"><span data-stu-id="1d30f-115">Outage simulation</span></span>
<span data-ttu-id="1d30f-116">A leállás szimulálása, törölheti vagy nevezze át a forrás-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="1d30f-116">To simulate the outage, you can delete or rename the source database.</span></span> <span data-ttu-id="1d30f-117">Ennek hatására a kapcsolat alkalmazáshibák.</span><span class="sxs-lookup"><span data-stu-id="1d30f-117">This causes application connectivity failures.</span></span>

#### <a name="recovery"></a><span data-ttu-id="1d30f-118">Helyreállítás</span><span class="sxs-lookup"><span data-stu-id="1d30f-118">Recovery</span></span>
* <span data-ttu-id="1d30f-119">Hajtsa végre a georedundáns helyreállítás az adatbázis egy másik kiszolgálóra történő leírtak [Itt](sql-database-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="1d30f-119">Perform the geo-restore of the database into a different server as described [here](sql-database-disaster-recovery.md).</span></span>
* <span data-ttu-id="1d30f-120">Módosítsa az alkalmazás konfigurációját, és csatlakozzon a helyreállított adatbázis, és kövesse a [adatbázis konfigurálnia a helyreállítás után](sql-database-disaster-recovery.md) útmutató, hogy elvégezhesse a helyreállítást.</span><span class="sxs-lookup"><span data-stu-id="1d30f-120">Change the application configuration to connect to the recovered database and follow the [Configure a database after recovery](sql-database-disaster-recovery.md) guide to complete the recovery.</span></span>

#### <a name="validation"></a><span data-ttu-id="1d30f-121">Ellenőrzés</span><span class="sxs-lookup"><span data-stu-id="1d30f-121">Validation</span></span>
* <span data-ttu-id="1d30f-122">Fejezze be a részletezési (beleértve a kapcsolati karakterláncok, bejelentkezéseket, alapvető funkciókat tesztelési, illetve más megszokott alkalmazástelepítési signoffs eljárások érvényesítést része) alkalmazás-integritási utáni helyreállítás ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="1d30f-122">Complete the drill by verifying the application integrity post recovery (including connection strings, logins, basic functionality testing or other validations part of standard application signoffs procedures).</span></span>

## <a name="geo-replication"></a><span data-ttu-id="1d30f-123">Georeplikáció</span><span class="sxs-lookup"><span data-stu-id="1d30f-123">Geo-replication</span></span>
<span data-ttu-id="1d30f-124">A részletezés gyakorlat georeplikáció használatával védett adatbázis magában foglalja a tervezett feladatátvételt a másodlagos adatbázis.</span><span class="sxs-lookup"><span data-stu-id="1d30f-124">For a database that is protected using geo-replication the drill exercise involves planned failover to the secondary database.</span></span> <span data-ttu-id="1d30f-125">A tervezett feladatátvétel biztosítja, hogy az elsődleges és a másodlagos adatbázisok maradjanak szinkronban a szerepkörök bekapcsolásakor.</span><span class="sxs-lookup"><span data-stu-id="1d30f-125">The planned failover ensures that the primary and the secondary databases remain in sync when the roles are switched.</span></span> <span data-ttu-id="1d30f-126">Ellentétben a nem tervezett feladatátvétel után ez a művelet eredményez adatvesztés, a részletezés az éles környezetben hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="1d30f-126">Unlike the unplanned failover, this operation does not result in data loss, so the drill can be performed in the production environment.</span></span>

#### <a name="outage-simulation"></a><span data-ttu-id="1d30f-127">Kimaradás szimulálása</span><span class="sxs-lookup"><span data-stu-id="1d30f-127">Outage simulation</span></span>
<span data-ttu-id="1d30f-128">A szolgáltatáskimaradás szimulálása, letilthatja a webes alkalmazás vagy a virtuális gép csatlakozott az adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="1d30f-128">To simulate the outage, you can disable the web application or virtual machine connected to the database.</span></span> <span data-ttu-id="1d30f-129">Ennek eredményeképp a csatlakozási hibák az ügyfelek számára.</span><span class="sxs-lookup"><span data-stu-id="1d30f-129">This results in the connectivity failures for the web clients.</span></span>

#### <a name="recovery"></a><span data-ttu-id="1d30f-130">Helyreállítás</span><span class="sxs-lookup"><span data-stu-id="1d30f-130">Recovery</span></span>
* <span data-ttu-id="1d30f-131">Győződjön meg arról, hogy az alkalmazás konfigurációját, a vész-Helyreállítási régióban mutat, a korábbi másodlagos, amely a teljes elérhető új elsődleges válik.</span><span class="sxs-lookup"><span data-stu-id="1d30f-131">Make sure the application configuration in the DR region points to the former secondary which becomes the fully accessible new primary.</span></span>
* <span data-ttu-id="1d30f-132">Hajtsa végre [tervezett feladatátvétel](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) egy új elsődleges a másodlagos adatbázis létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="1d30f-132">Perform [planned failover](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) to make the secondary database a new primary</span></span>
* <span data-ttu-id="1d30f-133">Kövesse a [adatbázis konfigurálnia a helyreállítás után](sql-database-disaster-recovery.md) útmutató, hogy elvégezhesse a helyreállítást.</span><span class="sxs-lookup"><span data-stu-id="1d30f-133">Follow the [Configure a database after recovery](sql-database-disaster-recovery.md) guide to complete the recovery.</span></span>

#### <a name="validation"></a><span data-ttu-id="1d30f-134">Ellenőrzés</span><span class="sxs-lookup"><span data-stu-id="1d30f-134">Validation</span></span>
* <span data-ttu-id="1d30f-135">Fejezze be a részletezési (beleértve a kapcsolati karakterláncok, bejelentkezéseket, alapvető funkciókat tesztelési, illetve más megszokott alkalmazástelepítési signoffs eljárások érvényesítést része) alkalmazás-integritási utáni helyreállítás ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="1d30f-135">Complete the drill by verifying the application integrity post recovery (including connection strings, logins, basic functionality testing or other validations part of standard application signoffs procedures).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1d30f-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1d30f-136">Next steps</span></span>
* <span data-ttu-id="1d30f-137">Üzleti folytonosság forgatókönyvekkel kapcsolatos további tudnivalókért lásd: [folytonosságának forgatókönyvek](sql-database-business-continuity.md)</span><span class="sxs-lookup"><span data-stu-id="1d30f-137">To learn about business continuity scenarios, see [Continuity scenarios](sql-database-business-continuity.md)</span></span>
* <span data-ttu-id="1d30f-138">További tudnivalók az Azure SQL adatbázis automatikus biztonsági mentés című [SQL-adatbázis automatikus biztonsági mentés](sql-database-automated-backups.md)</span><span class="sxs-lookup"><span data-stu-id="1d30f-138">To learn about Azure SQL Database automated backups, see [SQL Database automated backups](sql-database-automated-backups.md)</span></span>
* <span data-ttu-id="1d30f-139">A helyreállítás automatikus biztonsági mentés használatával kapcsolatos további tudnivalókért lásd: [adatbázis visszaállítása a szolgáltatás által kezdeményezett biztonsági másolatból](sql-database-recovery-using-backups.md)</span><span class="sxs-lookup"><span data-stu-id="1d30f-139">To learn about using automated backups for recovery, see [restore a database from the service-initiated backups](sql-database-recovery-using-backups.md)</span></span>
* <span data-ttu-id="1d30f-140">Gyorsabb helyreállítási beállításokkal kapcsolatos további tudnivalókért lásd: [aktív georeplikáció](sql-database-geo-replication-overview.md)</span><span class="sxs-lookup"><span data-stu-id="1d30f-140">To learn about faster recovery options, see [active geo-replication](sql-database-geo-replication-overview.md)</span></span>  
