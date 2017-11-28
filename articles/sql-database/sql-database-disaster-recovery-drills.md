---
title: "Adatbázis Vészhelyreállítások részletezésének aaaSQL |} Microsoft Docs"
description: "További útmutatás és ajánlott eljárások az Azure SQL Database tooperform katasztrófa utáni helyreállítás csukja toohelp megőrzése a a kritikus kritikus fontosságú üzleti alkalmazások rugalmas toofailures és a kimaradások esetén."
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
ms.openlocfilehash: bf17857a19fdebddf0d4f55e4db3a1b33efb4e8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="performing-disaster-recovery-drill"></a><span data-ttu-id="4189a-103">Vész-helyreállítási részletezési végrehajtása</span><span class="sxs-lookup"><span data-stu-id="4189a-103">Performing Disaster Recovery Drill</span></span>
<span data-ttu-id="4189a-104">Javasoljuk, hogy az alkalmazás felkészültségét helyreállítási munkafolyamat érvényesség rendszeres időközönként.</span><span class="sxs-lookup"><span data-stu-id="4189a-104">It is recommended that validation of application readiness for recovery workflow is performed periodically.</span></span> <span data-ttu-id="4189a-105">Ellenőrzése hello alkalmazás viselkedését, és adatok elvesztését és/vagy hello megszakítása következményei magában foglalja a, hogy a feladatátvétel mérnöki célszerű.</span><span class="sxs-lookup"><span data-stu-id="4189a-105">Verifying hello application behavior and implications of data loss and/or hello disruption that failover involves is a good engineering practice.</span></span> <span data-ttu-id="4189a-106">A legtöbb iparági normák szerint üzleti folytonossági hitelesítő részeként követelmény egyben.</span><span class="sxs-lookup"><span data-stu-id="4189a-106">It is also a requirement by most industry standards as part of business continuity certification.</span></span>

<span data-ttu-id="4189a-107">A vész-helyreállítási részletezési végrehajtása áll:</span><span class="sxs-lookup"><span data-stu-id="4189a-107">Performing a disaster recovery drill consists of:</span></span>

* <span data-ttu-id="4189a-108">Amelyek adatok réteg leállás</span><span class="sxs-lookup"><span data-stu-id="4189a-108">Simulating data tier outage</span></span>
* <span data-ttu-id="4189a-109">Helyreállítása</span><span class="sxs-lookup"><span data-stu-id="4189a-109">Recovering</span></span>
* <span data-ttu-id="4189a-110">Alkalmazás integritási utáni helyreállítás ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="4189a-110">Validate application integrity post recovery</span></span>

<span data-ttu-id="4189a-111">Attól függően, hogy hogyan meg [az alkalmazás az üzletmenet folytonossága érdekében tervezett](sql-database-business-continuity.md), hello munkafolyamat tooexecute hello részletezési eltérőek lehetnek.</span><span class="sxs-lookup"><span data-stu-id="4189a-111">Depending on how you [designed your application for business continuity](sql-database-business-continuity.md), hello workflow tooexecute hello drill can vary.</span></span> <span data-ttu-id="4189a-112">Az alábbiakban egy vész-helyreállítási részletezéshez hello környezetében az Azure SQL Database végző hello ajánlott eljárások azt ismertetik.</span><span class="sxs-lookup"><span data-stu-id="4189a-112">Below we describe hello best practices conducting a disaster recovery drill in hello context of Azure SQL Database.</span></span>

## <a name="geo-restore"></a><span data-ttu-id="4189a-113">Georedundáns helyreállítás</span><span class="sxs-lookup"><span data-stu-id="4189a-113">Geo-restore</span></span>
<span data-ttu-id="4189a-114">tooprevent hello lehetséges adatvesztést a vész-helyreállítási részletezési során, ajánlott hello részletezési hello éles környezetben másolatának elkészítése és használja azt a tesztkörnyezet használatával végez tooverify hello alkalmazás feladatátvételi munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="4189a-114">tooprevent hello potential data loss when conducting a disaster recovery drill, we recommend performing hello drill using a test environment by creating a copy of hello production environment and using it tooverify hello application’s failover workflow.</span></span>

#### <a name="outage-simulation"></a><span data-ttu-id="4189a-115">Kimaradás szimulálása</span><span class="sxs-lookup"><span data-stu-id="4189a-115">Outage simulation</span></span>
<span data-ttu-id="4189a-116">toosimulate hello kimaradás, törölheti vagy hello source adatbázis átnevezése.</span><span class="sxs-lookup"><span data-stu-id="4189a-116">toosimulate hello outage, you can delete or rename hello source database.</span></span> <span data-ttu-id="4189a-117">Ennek hatására a kapcsolat alkalmazáshibák.</span><span class="sxs-lookup"><span data-stu-id="4189a-117">This causes application connectivity failures.</span></span>

#### <a name="recovery"></a><span data-ttu-id="4189a-118">Helyreállítás</span><span class="sxs-lookup"><span data-stu-id="4189a-118">Recovery</span></span>
* <span data-ttu-id="4189a-119">Hajtsa végre a hello georedundáns helyreállítás hello adatbázis egy másik kiszolgálóra történő leírtak [Itt](sql-database-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="4189a-119">Perform hello geo-restore of hello database into a different server as described [here](sql-database-disaster-recovery.md).</span></span>
* <span data-ttu-id="4189a-120">Változás hello alkalmazás konfigurációs tooconnect toohello helyreállított adatbázis és követi hello [adatbázis konfigurálnia a helyreállítás után](sql-database-disaster-recovery.md) toocomplete hello helyreállítási ismerteti.</span><span class="sxs-lookup"><span data-stu-id="4189a-120">Change hello application configuration tooconnect toohello recovered database and follow hello [Configure a database after recovery](sql-database-disaster-recovery.md) guide toocomplete hello recovery.</span></span>

#### <a name="validation"></a><span data-ttu-id="4189a-121">Ellenőrzés</span><span class="sxs-lookup"><span data-stu-id="4189a-121">Validation</span></span>
* <span data-ttu-id="4189a-122">Teljes hello részletezési hello alkalmazás integritási utáni helyreállítás (beleértve a kapcsolati karakterláncok, bejelentkezéseket, alapvető funkciókat tesztelési, illetve más megszokott alkalmazástelepítési signoffs eljárások érvényesítést része) ellenőrzésével.</span><span class="sxs-lookup"><span data-stu-id="4189a-122">Complete hello drill by verifying hello application integrity post recovery (including connection strings, logins, basic functionality testing or other validations part of standard application signoffs procedures).</span></span>

## <a name="geo-replication"></a><span data-ttu-id="4189a-123">Georeplikáció</span><span class="sxs-lookup"><span data-stu-id="4189a-123">Geo-replication</span></span>
<span data-ttu-id="4189a-124">Georeplikálási hello részletezési használatával védett adatbázis a gyakorlatban magában foglalja a tervezett feladatátvétel toohello másodlagos adatbázis.</span><span class="sxs-lookup"><span data-stu-id="4189a-124">For a database that is protected using geo-replication hello drill exercise involves planned failover toohello secondary database.</span></span> <span data-ttu-id="4189a-125">hello tervezett feladatátvétel biztosítja, hogy hello elsődleges és másodlagos adatbázisok hello maradjanak szinkronban hello szerepkörök bekapcsolásakor.</span><span class="sxs-lookup"><span data-stu-id="4189a-125">hello planned failover ensures that hello primary and hello secondary databases remain in sync when hello roles are switched.</span></span> <span data-ttu-id="4189a-126">Ellentétben a nem tervezett feladatátvétel hello, ez a művelet nem eredményez adatvesztés, így hello részletezési végrehajtható hello éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="4189a-126">Unlike hello unplanned failover, this operation does not result in data loss, so hello drill can be performed in hello production environment.</span></span>

#### <a name="outage-simulation"></a><span data-ttu-id="4189a-127">Kimaradás szimulálása</span><span class="sxs-lookup"><span data-stu-id="4189a-127">Outage simulation</span></span>
<span data-ttu-id="4189a-128">toosimulate hello kimaradás, letilthatja hello webalkalmazáshoz vagy a virtuális gép csatlakoztatott toohello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="4189a-128">toosimulate hello outage, you can disable hello web application or virtual machine connected toohello database.</span></span> <span data-ttu-id="4189a-129">Az eredmény hello kapcsolathibái hello-ügyfelek számára.</span><span class="sxs-lookup"><span data-stu-id="4189a-129">This results in hello connectivity failures for hello web clients.</span></span>

#### <a name="recovery"></a><span data-ttu-id="4189a-130">Helyreállítás</span><span class="sxs-lookup"><span data-stu-id="4189a-130">Recovery</span></span>
* <span data-ttu-id="4189a-131">Győződjön meg arról, hogy hello Alkalmazáskonfiguráció hello vész-Helyreállítási régió pontok toohello volt másodlagos hello válik a teljes elérhető új elsődleges.</span><span class="sxs-lookup"><span data-stu-id="4189a-131">Make sure hello application configuration in hello DR region points toohello former secondary which becomes hello fully accessible new primary.</span></span>
* <span data-ttu-id="4189a-132">Hajtsa végre [tervezett feladatátvétel](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) toomake hello másodlagos adatbázis-egy új elsődleges</span><span class="sxs-lookup"><span data-stu-id="4189a-132">Perform [planned failover](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) toomake hello secondary database a new primary</span></span>
* <span data-ttu-id="4189a-133">Hajtsa végre a hello [adatbázis konfigurálnia a helyreállítás után](sql-database-disaster-recovery.md) toocomplete hello helyreállítási ismerteti.</span><span class="sxs-lookup"><span data-stu-id="4189a-133">Follow hello [Configure a database after recovery](sql-database-disaster-recovery.md) guide toocomplete hello recovery.</span></span>

#### <a name="validation"></a><span data-ttu-id="4189a-134">Ellenőrzés</span><span class="sxs-lookup"><span data-stu-id="4189a-134">Validation</span></span>
* <span data-ttu-id="4189a-135">Teljes hello részletezési hello alkalmazás integritási utáni helyreállítás (beleértve a kapcsolati karakterláncok, bejelentkezéseket, alapvető funkciókat tesztelési, illetve más megszokott alkalmazástelepítési signoffs eljárások érvényesítést része) ellenőrzésével.</span><span class="sxs-lookup"><span data-stu-id="4189a-135">Complete hello drill by verifying hello application integrity post recovery (including connection strings, logins, basic functionality testing or other validations part of standard application signoffs procedures).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4189a-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4189a-136">Next steps</span></span>
* <span data-ttu-id="4189a-137">üzleti folytonosság-forgatókönyvekkel kapcsolatos toolearn lásd [folytonosságának forgatókönyvek](sql-database-business-continuity.md)</span><span class="sxs-lookup"><span data-stu-id="4189a-137">toolearn about business continuity scenarios, see [Continuity scenarios](sql-database-business-continuity.md)</span></span>
* <span data-ttu-id="4189a-138">tudnivalók Azure SQL adatbázis automatikus biztonsági mentés, toolearn lásd: [SQL-adatbázis automatikus biztonsági mentés](sql-database-automated-backups.md)</span><span class="sxs-lookup"><span data-stu-id="4189a-138">toolearn about Azure SQL Database automated backups, see [SQL Database automated backups](sql-database-automated-backups.md)</span></span>
* <span data-ttu-id="4189a-139">toolearn a helyreállításhoz, az automatikus biztonsági mentés használatával kapcsolatban lásd: [adatbázis visszaállítása biztonsági másolatból hello szolgáltatás által kezdeményezett](sql-database-recovery-using-backups.md)</span><span class="sxs-lookup"><span data-stu-id="4189a-139">toolearn about using automated backups for recovery, see [restore a database from hello service-initiated backups](sql-database-recovery-using-backups.md)</span></span>
* <span data-ttu-id="4189a-140">toolearn gyorsabb helyreállítási beállításokkal kapcsolatban lásd: [aktív georeplikáció](sql-database-geo-replication-overview.md)</span><span class="sxs-lookup"><span data-stu-id="4189a-140">toolearn about faster recovery options, see [active geo-replication](sql-database-geo-replication-overview.md)</span></span>  
