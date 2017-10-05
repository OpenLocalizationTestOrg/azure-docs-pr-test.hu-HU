---
title: "Az Azure SQL Database biztonsági vész-helyreállítási konfigurálása |} Microsoft Docs"
description: "Ez a témakör azt ismerteti, konfigurálásához és kezeléséhez biztonsági egy adatbázis visszaállítása vagy egy másodlagos kiszolgáló a feladatátvétel után egy adatközpont-meghibásodás után vagy egyéb katasztrófa esetén a biztonsági szempontok"
services: sql-database
documentationcenter: na
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: c7c898c9-69d4-4e16-8b7e-720bbb3353dd
ms.service: sql-database
ms.custom: business continuity
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 10/13/2016
ms.author: sashan
ms.openlocfilehash: de5e1732dab570b80692efcdd08e4ed2d8c98800
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-and-manage-azure-sql-database-security-for-geo-restore-or-failover"></a><span data-ttu-id="aa9e2-103">Konfigurálhatja és kezelheti az Azure SQL Database biztonsági georedundáns helyreállítás vagy feladatátvételi</span><span class="sxs-lookup"><span data-stu-id="aa9e2-103">Configure and manage Azure SQL Database security for geo-restore or failover</span></span> 

> [!NOTE]
> <span data-ttu-id="aa9e2-104">[Aktív georeplikáció](sql-database-geo-replication-overview.md) érhető el az összes olyan adatbázis összes szolgáltatásrétegeiben használt funkciókkal.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-104">[Active geo-replication](sql-database-geo-replication-overview.md) is now available for all databases in all service tiers.</span></span>
>  

## <a name="overview-of-authentication-requirements-for-disaster-recovery"></a><span data-ttu-id="aa9e2-105">Vész-helyreállítási hitelesítési követelmények áttekintése</span><span class="sxs-lookup"><span data-stu-id="aa9e2-105">Overview of authentication requirements for disaster recovery</span></span>
<span data-ttu-id="aa9e2-106">Ez a témakör ismerteti a hitelesítési követelmények konfigurálása és [aktív georeplikáció](sql-database-geo-replication-overview.md) és a felhasználó hozzáférését a másodlagos adatbázis beállításához szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-106">This topic describes the authentication requirements to configure and control [active geo-replication](sql-database-geo-replication-overview.md) and the steps required to set up user access to the secondary database.</span></span> <span data-ttu-id="aa9e2-107">Azt is ismerteti, hogyan a helyreállított adatbázis elérésének lehetővé tétele használata után [georedundáns helyreállítás](sql-database-recovery-using-backups.md#geo-restore).</span><span class="sxs-lookup"><span data-stu-id="aa9e2-107">It also describes how enable access to the recovered database after using [geo-restore](sql-database-recovery-using-backups.md#geo-restore).</span></span> <span data-ttu-id="aa9e2-108">Helyreállítási lehetőségekről további információkért lásd: [üzleti folytonosság – áttekintés](sql-database-business-continuity.md).</span><span class="sxs-lookup"><span data-stu-id="aa9e2-108">For more information on recovery options, see [Business Continuity Overview](sql-database-business-continuity.md).</span></span>

## <a name="disaster-recovery-with-contained-users"></a><span data-ttu-id="aa9e2-109">A tartalmazott felhasználók katasztrófa utáni helyreállítás</span><span class="sxs-lookup"><span data-stu-id="aa9e2-109">Disaster recovery with contained users</span></span>
<span data-ttu-id="aa9e2-110">Ellentétben a hagyományos felhasználóknak, amelyet le kell képezni a master adatbázisban bejelentkezéseket, egy felhasználó teljesen által felügyelt maga az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-110">Unlike traditional users, which must be mapped to logins in the master database, a contained user is managed completely by the database itself.</span></span> <span data-ttu-id="aa9e2-111">Azt két előnnyel jár.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-111">This has two benefits.</span></span> <span data-ttu-id="aa9e2-112">A vészhelyreállítás esetén a felhasználók továbbra is az új elsődleges adatbázishoz való kapcsolódáshoz vagy az adatbázist helyre az georedundáns helyreállítás, további konfiguráció nélkül használja, mert az adatbázis kezeli a felhasználók.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-112">In the disaster recovery scenario, the users can continue to connect to the new primary database or the database recovered using geo-restore without any additional configuration, because the database manages the users.</span></span> <span data-ttu-id="aa9e2-113">Nincsenek is lehetséges méretezhetőségét és teljesítményét számos előnyt biztosít az ebben a konfigurációban egy bejelentkezési szempontjából.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-113">There are also potential scalability and performance benefits from this configuration from a login perspective.</span></span> <span data-ttu-id="aa9e2-114">További információt a [tartalmazottadatbázis-felhasználókkal kapcsolatos, az adatbázis hordozhatóvá tételével foglalkozó](https://msdn.microsoft.com/library/ff929188.aspx) cikkben talál.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-114">For more information, see [Contained Database Users - Making Your Database Portable](https://msdn.microsoft.com/library/ff929188.aspx).</span></span> 

<span data-ttu-id="aa9e2-115">A fő kompromisszum, annál nagyobb kihívás a vész-helyreállítási folyamat léptékű kezelésére.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-115">The main trade-off is that managing the disaster recovery process at scale is more challenging.</span></span> <span data-ttu-id="aa9e2-116">Ha egyszerre több adatbázis ugyanazon bejelentkezési használó, a hitelesítő adatokat tartalmazott felhasználók több adatbázisok karbantartása előfordulhat, hogy semlegesítsék a tartalmazott felhasználók előnyeit.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-116">When you have multiple databases that use the same login, maintaining the credentials using contained users in multiple databases may negate the benefits of contained users.</span></span> <span data-ttu-id="aa9e2-117">Például a jelszóházirend elforgatási van szükség, hogy lehet változtatás következetesen több adatbázis ahelyett, hogy megváltoztatta a jelszavát, a bejelentkezési azonosítóhoz egyszer a master adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-117">For example, the password rotation policy requires that changes be made consistently in multiple databases rather than changing the password for the login once in the master database.</span></span> <span data-ttu-id="aa9e2-118">Ezért ha több, ugyanazt a felhasználónevet és jelszót, használó adatbázisok tartalmazott felhasználók használata nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-118">For this reason, if you have multiple databases that use the same user name and password, using contained users is not recommended.</span></span> 

## <a name="how-to-configure-logins-and-users"></a><span data-ttu-id="aa9e2-119">Bejelentkezések és a felhasználók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="aa9e2-119">How to configure logins and users</span></span>
<span data-ttu-id="aa9e2-120">Bejelentkezések és felhasználók használata (helyett a benne lévő felhasználók), annak érdekében, hogy létezik-e az azonos bejelentkezések a master adatbázisban további lépéseket kell végrehajtania.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-120">If you are using logins and users (rather than contained users), you must take extra steps to insure that the same logins exist in the master database.</span></span> <span data-ttu-id="aa9e2-121">Az alábbi szakaszok felsorolják a lépéseket érintett és további szempontokat.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-121">The following sections outline the steps involved and additional considerations.</span></span>

### <a name="set-up-user-access-to-a-secondary-or-recovered-database"></a><span data-ttu-id="aa9e2-122">Állítsa be a másodlagos, és a helyreállított adatbázis felhasználói hozzáférése</span><span class="sxs-lookup"><span data-stu-id="aa9e2-122">Set up user access to a secondary or recovered database</span></span>
<span data-ttu-id="aa9e2-123">Ahhoz, hogy a másodlagos adatbázis csak olvasható másodlagos adatbázis használható, és megfelelő hozzáféréssel az új elsődleges adatbázis vagy az adatbázis visszaállítással földrajzi helyre a célkiszolgáló a master adatbázis konfigurációval kell rendelkeznie a megfelelő biztonsági a helyreállítás előtt a helyükön.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-123">In order for the secondary database to be usable as a read-only secondary database, and to ensure proper access to the new primary database or the database recovered using geo-restore, the master database of the target server must have the appropriate security configuration in place before the recovery.</span></span>

<span data-ttu-id="aa9e2-124">A témakör későbbi részében egyes lépéseihez szükséges engedélyeket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-124">The specific permissions for each step are described later in this topic.</span></span>

<span data-ttu-id="aa9e2-125">Felkészülés a felhasználói hozzáférés a georeplikáció másodlagos georeplikáció konfigurálása során kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-125">Preparing user access to a geo-replication secondary should be performed as part configuring geo-replication.</span></span> <span data-ttu-id="aa9e2-126">Felkészülés a földrajzi vissza adatbázisok felhasználói hozzáférést kell hajtható végre, tetszőleges időpontban, amikor az eredeti kiszolgáló online állapotban-e (pl. a vész-Helyreállítási részletezési részeként).</span><span class="sxs-lookup"><span data-stu-id="aa9e2-126">Preparing user access to the geo-restored databases should be performed at any time when the original server is online (e.g. as part of the DR drill).</span></span>

> [!NOTE]
> <span data-ttu-id="aa9e2-127">Ha a rendszer átadja, vagy olyan kiszolgálóra, amely nem rendelkezik hozzáféréssel a megfelelően konfigurált bejelentkezések georedundáns helyreállítás korlátozódik a kiszolgáló-rendszergazdai fiókot.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-127">If you fail over or geo-restore to a server that does not have properly configured logins, access to it will be limited to the server admin account.</span></span>
> 
> 

<span data-ttu-id="aa9e2-128">Az alábbiakban leírt három lépést beállítása a célkiszolgálón bejelentkezések foglal magában:</span><span class="sxs-lookup"><span data-stu-id="aa9e2-128">Setting up logins on the target server involves three steps outlined below:</span></span>

#### <a name="1-determine-logins-with-access-to-the-primary-database"></a><span data-ttu-id="aa9e2-129">1. Határozza meg az elsődleges adatbázis eléréséhez felmérhetők:</span><span class="sxs-lookup"><span data-stu-id="aa9e2-129">1. Determine logins with access to the primary database:</span></span>
<span data-ttu-id="aa9e2-130">A folyamat az első lépés annak meghatározásához, hogy mely bejelentkezések kell készíteni a cél kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-130">The first step of the process is to determine which logins must be duplicated on the target server.</span></span> <span data-ttu-id="aa9e2-131">Ez KIVÁLASZTÓ utasítást, az a logikai master adatbázis a forráskiszolgálón és a maga az elsődleges adatbázis két érhető el.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-131">This is accomplished with a pair of SELECT statements, one in the logical master database on the source server and one in the primary database itself.</span></span>

<span data-ttu-id="aa9e2-132">Csak a kiszolgáló rendszergazdája vagy egy másik a **LoginManager** kiszolgálói szerepkör megállapíthatja, hogy a bejelentkezési adatok, a forráskiszolgálón a következő SELECT utasítással.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-132">Only the server admin or a member of the **LoginManager** server role can determine the logins on the source server with the following SELECT statement.</span></span> 

    SELECT [name], [sid] 
    FROM [sys].[sql_logins] 
    WHERE [type_desc] = 'SQL_Login'

<span data-ttu-id="aa9e2-133">Csak a db_owner adatbázis-szerepkör, a dbo felhasználói vagy kiszolgálói rendszergazda, tag lehet meghatározni az összes, az adatbázis-felhasználó rendszerbiztonsági tagot az elsődleges adatbázis.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-133">Only a member of the db_owner database role, the dbo user, or server admin, can determine all of the database user principals in the primary database.</span></span>

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

#### <a name="2-find-the-sid-for-the-logins-identified-in-step-1"></a><span data-ttu-id="aa9e2-134">2. 1. lépésben azonosított bejelentkezések tartozó SID kereséséhez:</span><span class="sxs-lookup"><span data-stu-id="aa9e2-134">2. Find the SID for the logins identified in step 1:</span></span>
<span data-ttu-id="aa9e2-135">A fenti szakaszban leírt lekérdezések kimenetének összehasonlítása, és a biztonsági azonosítók megfelelő, a kiszolgálói bejelentkezési adatbázis-felhasználó is leképezheti.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-135">By comparing the output of the queries from the previous section and matching the SIDs, you can map the server login to database user.</span></span> <span data-ttu-id="aa9e2-136">Egy adatbázis-felhasználó a megfelelő SID-AZONOSÍTÓVAL rendelkező felhasználók hozzáférhetnek felhasználó ahhoz az adatbázishoz adott adatbázis elsődleges felhasználóként.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-136">Logins that have a database user with a matching SID have user access to that database as that database user principal.</span></span> 

<span data-ttu-id="aa9e2-137">A következő lekérdezés segítségével láthatja az összes, a felhasználó rendszerbiztonsági tagok és az SID-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-137">The following query can be used to see all of the user principals and their SIDs in a database.</span></span> <span data-ttu-id="aa9e2-138">Csak a db_owner adatbázis-szerepkör- vagy felügyeleti tagjai futtathatják a lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-138">Only a member of the db_owner database role or server admin can run this query.</span></span>

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

> [!NOTE]
> <span data-ttu-id="aa9e2-139">A **entitástulajdonos** és **sys** felhasználóknál *NULL* SID-k, és a **vendég** SID **0x00**.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-139">The **INFORMATION_SCHEMA** and **sys** users have *NULL* SIDs, and the **guest** SID is **0x00**.</span></span> <span data-ttu-id="aa9e2-140">A **dbo** SID kezdődik *0x01060000000001648000000000048454*, ha az adatbázis-készítő tagja helyett a kiszolgáló rendszergazdája **DbManager**.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-140">The **dbo** SID may start with *0x01060000000001648000000000048454*, if the database creator was the server admin instead of a member of **DbManager**.</span></span>
> 
> 

#### <a name="3-create-the-logins-on-the-target-server"></a><span data-ttu-id="aa9e2-141">3. A bejelentkezési adatok létrehozása a célkiszolgálón:</span><span class="sxs-lookup"><span data-stu-id="aa9e2-141">3. Create the logins on the target server:</span></span>
<span data-ttu-id="aa9e2-142">Az utolsó lépése, hogy nyissa meg a célként megadott kiszolgáló vagy kiszolgálók, és a megfelelő SID-k a felmérhetők készítése.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-142">The last step is to go to the target server, or servers, and generate the logins with the appropriate SIDs.</span></span> <span data-ttu-id="aa9e2-143">Az alapvető szintaxisa a következő.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-143">The basic syntax is as follows.</span></span>

    CREATE LOGIN [<login name>]
    WITH PASSWORD = <login password>,
    SID = <desired login SID>

> [!NOTE]
> <span data-ttu-id="aa9e2-144">Ha meg szeretné adni a felhasználói hozzáférést a másodlagos, de nem az elsődleges, akkor a következő szintaxis használatával az elsődleges kiszolgálón a felhasználói bejelentkezési módosításával teheti meg.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-144">If you want to grant user access to the secondary, but not to the primary, you can do that by altering the user login on the primary server by using the following syntax.</span></span>
> 
> <span data-ttu-id="aa9e2-145">ALTER LOGIN <login name> LETILTÁSA</span><span class="sxs-lookup"><span data-stu-id="aa9e2-145">ALTER LOGIN <login name> DISABLE</span></span>
> 
> <span data-ttu-id="aa9e2-146">Tiltsa le a jelszó nem változik, ezért mindig engedélyezheti azt szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="aa9e2-146">DISABLE doesn’t change the password, so you can always enable it if needed.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="aa9e2-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aa9e2-147">Next steps</span></span>
* <span data-ttu-id="aa9e2-148">Az adatbázis-hozzáférési és bejelentkezések kezelése további információkért lásd: [SQL-adatbázis biztonsági: adatbázis-hozzáférési és bejelentkezési biztonság kezeléséhez](sql-database-manage-logins.md).</span><span class="sxs-lookup"><span data-stu-id="aa9e2-148">For more information on managing database access and logins, see [SQL Database security: Manage database access and login security](sql-database-manage-logins.md).</span></span>
* <span data-ttu-id="aa9e2-149">A tartalmazott adatbázis-felhasználók további információkért lásd: [tartalmazott adatbázis-felhasználók – így az adatbázis hordozható](https://msdn.microsoft.com/library/ff929188.aspx).</span><span class="sxs-lookup"><span data-stu-id="aa9e2-149">For more information on contained database users, see [Contained Database Users - Making Your Database Portable](https://msdn.microsoft.com/library/ff929188.aspx).</span></span>
* <span data-ttu-id="aa9e2-150">Aktív georeplikáció konfigurálásával kapcsolatos további információkért lásd: [aktív georeplikáció](sql-database-geo-replication-overview.md)</span><span class="sxs-lookup"><span data-stu-id="aa9e2-150">For information about using and configuring active geo-replication, see [active geo-replication](sql-database-geo-replication-overview.md)</span></span>
* <span data-ttu-id="aa9e2-151">Georedundáns helyreállítás használatával kapcsolatos információkért lásd: [georedundáns helyreállítás](sql-database-recovery-using-backups.md#geo-restore)</span><span class="sxs-lookup"><span data-stu-id="aa9e2-151">For information about using geo-restore, see [geo-restore](sql-database-recovery-using-backups.md#geo-restore)</span></span>

