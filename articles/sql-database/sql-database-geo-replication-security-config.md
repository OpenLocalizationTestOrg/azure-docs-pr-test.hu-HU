---
title: "az Azure SQL Database biztonsági vész-helyreállítási aaaConfigure |} Microsoft Docs"
description: "Ez a témakör azt ismerteti, konfigurálásához és kezeléséhez biztonsági egy adatbázis visszaállítása vagy egy feladatátvételi tooa másodlagos kiszolgáló egy adatközpont-meghibásodás után vagy más katasztrófa hello esemény után a biztonsági szempontok"
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
ms.openlocfilehash: c3172568e1b8ad2a53958200aa6cf19b4a9434ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-and-manage-azure-sql-database-security-for-geo-restore-or-failover"></a><span data-ttu-id="eee6d-103">Konfigurálhatja és kezelheti az Azure SQL Database biztonsági georedundáns helyreállítás vagy feladatátvételi</span><span class="sxs-lookup"><span data-stu-id="eee6d-103">Configure and manage Azure SQL Database security for geo-restore or failover</span></span> 

> [!NOTE]
> <span data-ttu-id="eee6d-104">[Aktív georeplikáció](sql-database-geo-replication-overview.md) érhető el az összes olyan adatbázis összes szolgáltatásrétegeiben használt funkciókkal.</span><span class="sxs-lookup"><span data-stu-id="eee6d-104">[Active geo-replication](sql-database-geo-replication-overview.md) is now available for all databases in all service tiers.</span></span>
>  

## <a name="overview-of-authentication-requirements-for-disaster-recovery"></a><span data-ttu-id="eee6d-105">Vész-helyreállítási hitelesítési követelmények áttekintése</span><span class="sxs-lookup"><span data-stu-id="eee6d-105">Overview of authentication requirements for disaster recovery</span></span>
<span data-ttu-id="eee6d-106">Ez a témakör ismerteti a hello hitelesítési követelmények tooconfigure és vezérlés [aktív georeplikáció](sql-database-geo-replication-overview.md) és hello felhasználói hozzáférés toohello másodlagos adatbázis szükséges tooset lépéseket.</span><span class="sxs-lookup"><span data-stu-id="eee6d-106">This topic describes hello authentication requirements tooconfigure and control [active geo-replication](sql-database-geo-replication-overview.md) and hello steps required tooset up user access toohello secondary database.</span></span> <span data-ttu-id="eee6d-107">Azt is ismerteti, hogyan engedélyezze a hozzáférést toohello helyreállított adatbázis használata után [georedundáns helyreállítás](sql-database-recovery-using-backups.md#geo-restore).</span><span class="sxs-lookup"><span data-stu-id="eee6d-107">It also describes how enable access toohello recovered database after using [geo-restore](sql-database-recovery-using-backups.md#geo-restore).</span></span> <span data-ttu-id="eee6d-108">Helyreállítási lehetőségekről további információkért lásd: [üzleti folytonosság – áttekintés](sql-database-business-continuity.md).</span><span class="sxs-lookup"><span data-stu-id="eee6d-108">For more information on recovery options, see [Business Continuity Overview](sql-database-business-continuity.md).</span></span>

## <a name="disaster-recovery-with-contained-users"></a><span data-ttu-id="eee6d-109">A tartalmazott felhasználók katasztrófa utáni helyreállítás</span><span class="sxs-lookup"><span data-stu-id="eee6d-109">Disaster recovery with contained users</span></span>
<span data-ttu-id="eee6d-110">Hagyományos felhasználók eltérően toologins hello master adatbázisban kell lennie, amely leképezve, egy felhasználó maga hello adatbázis teljesen kezeli.</span><span class="sxs-lookup"><span data-stu-id="eee6d-110">Unlike traditional users, which must be mapped toologins in hello master database, a contained user is managed completely by hello database itself.</span></span> <span data-ttu-id="eee6d-111">Azt két előnnyel jár.</span><span class="sxs-lookup"><span data-stu-id="eee6d-111">This has two benefits.</span></span> <span data-ttu-id="eee6d-112">Hello vészhelyreállítás esetén hello felhasználók továbbra is tooconnect toohello új elsődleges adatbázis vagy hello adatbázis helyre georedundáns helyreállítás, további konfiguráció nélkül használja, mert hello adatbázis hello felhasználók kezeli.</span><span class="sxs-lookup"><span data-stu-id="eee6d-112">In hello disaster recovery scenario, hello users can continue tooconnect toohello new primary database or hello database recovered using geo-restore without any additional configuration, because hello database manages hello users.</span></span> <span data-ttu-id="eee6d-113">Nincsenek is lehetséges méretezhetőségét és teljesítményét számos előnyt biztosít az ebben a konfigurációban egy bejelentkezési szempontjából.</span><span class="sxs-lookup"><span data-stu-id="eee6d-113">There are also potential scalability and performance benefits from this configuration from a login perspective.</span></span> <span data-ttu-id="eee6d-114">További információt a [tartalmazottadatbázis-felhasználókkal kapcsolatos, az adatbázis hordozhatóvá tételével foglalkozó](https://msdn.microsoft.com/library/ff929188.aspx) cikkben talál.</span><span class="sxs-lookup"><span data-stu-id="eee6d-114">For more information, see [Contained Database Users - Making Your Database Portable](https://msdn.microsoft.com/library/ff929188.aspx).</span></span> 

<span data-ttu-id="eee6d-115">hello fő kompromisszum az, hogy hello vész-helyreállítási folyamat léptékű kezelése bonyolult.</span><span class="sxs-lookup"><span data-stu-id="eee6d-115">hello main trade-off is that managing hello disaster recovery process at scale is more challenging.</span></span> <span data-ttu-id="eee6d-116">Ha több adatbázist, hogy használjon hello azonos bejelentkezési karbantartása hello hitelesítő adatokat tartalmazott a felhasználók több adatbázis előfordulhat, hogy semlegesítsék tartalmazott felhasználók hello előnyeit.</span><span class="sxs-lookup"><span data-stu-id="eee6d-116">When you have multiple databases that use hello same login, maintaining hello credentials using contained users in multiple databases may negate hello benefits of contained users.</span></span> <span data-ttu-id="eee6d-117">Például hello Elforgatás jelszóházirend megköveteli-, hogy módosítható következetesen több adatbázisok helyett hello bejelentkezési azonosító változó hello jelszavának egyszer hello master adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="eee6d-117">For example, hello password rotation policy requires that changes be made consistently in multiple databases rather than changing hello password for hello login once in hello master database.</span></span> <span data-ttu-id="eee6d-118">Ezért, ha több adatbázist, hogy használjon hello ugyanazt a felhasználónevet és jelszót, a tartalmazott felhasználók használata nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="eee6d-118">For this reason, if you have multiple databases that use hello same user name and password, using contained users is not recommended.</span></span> 

## <a name="how-tooconfigure-logins-and-users"></a><span data-ttu-id="eee6d-119">Hogyan tooconfigure bejelentkezéseket és a felhasználók</span><span class="sxs-lookup"><span data-stu-id="eee6d-119">How tooconfigure logins and users</span></span>
<span data-ttu-id="eee6d-120">Bejelentkezések és felhasználók használata (helyett a benne lévő felhasználók), érdemes további lépések tooinsure, hogy ugyanazt a bejelentkezési adatok léteznek hello hello master adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="eee6d-120">If you are using logins and users (rather than contained users), you must take extra steps tooinsure that hello same logins exist in hello master database.</span></span> <span data-ttu-id="eee6d-121">hello alábbi szakaszok felsorolják hello lépéseket érintett és további szempontokat.</span><span class="sxs-lookup"><span data-stu-id="eee6d-121">hello following sections outline hello steps involved and additional considerations.</span></span>

### <a name="set-up-user-access-tooa-secondary-or-recovered-database"></a><span data-ttu-id="eee6d-122">Felhasználói hozzáférés tooa másodlagos vagy a helyreállított adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="eee6d-122">Set up user access tooa secondary or recovered database</span></span>
<span data-ttu-id="eee6d-123">Ahhoz, hogy hello másodlagos adatbázis toobe használható, egy csak olvasható másodlagos adatbázis és tooensure megfelelő hozzáféréssel toohello új elsődleges adatbázis vagy hello adatbázis helyreállítása a földrajzi-visszaállítással, hello fő célkiszolgáló hello kell biztosítani hello megfelelő biztonsági konfiguráció hello helyreállítás előtt a helyükön.</span><span class="sxs-lookup"><span data-stu-id="eee6d-123">In order for hello secondary database toobe usable as a read-only secondary database, and tooensure proper access toohello new primary database or hello database recovered using geo-restore, hello master database of hello target server must have hello appropriate security configuration in place before hello recovery.</span></span>

<span data-ttu-id="eee6d-124">a témakör későbbi részében hello egyes lépéseihez szükséges engedélyeket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="eee6d-124">hello specific permissions for each step are described later in this topic.</span></span>

<span data-ttu-id="eee6d-125">Felkészülés a felhasználói hozzáférés tooa georeplikáció másodlagos georeplikáció konfigurálása során kell végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="eee6d-125">Preparing user access tooa geo-replication secondary should be performed as part configuring geo-replication.</span></span> <span data-ttu-id="eee6d-126">Felkészülés a felhasználói hozzáférés toohello földrajzi vissza adatbázisok kell hajtható végre, tetszőleges időpontban, amikor hello eredeti kiszolgáló online állapotban-e (pl. hello vész-Helyreállítási részletezési részeként).</span><span class="sxs-lookup"><span data-stu-id="eee6d-126">Preparing user access toohello geo-restored databases should be performed at any time when hello original server is online (e.g. as part of hello DR drill).</span></span>

> [!NOTE]
> <span data-ttu-id="eee6d-127">Ha nem sikerül over vagy georedundáns helyreállítás, amely nem rendelkezik megfelelően konfigurált bejelentkezések tooa server hozzáférés tooit lesz korlátozott toohello server rendszergazdai fiók.</span><span class="sxs-lookup"><span data-stu-id="eee6d-127">If you fail over or geo-restore tooa server that does not have properly configured logins, access tooit will be limited toohello server admin account.</span></span>
> 
> 

<span data-ttu-id="eee6d-128">Az alábbiakban leírt három lépést beállítása a célkiszolgálón hello bejelentkezések foglal magában:</span><span class="sxs-lookup"><span data-stu-id="eee6d-128">Setting up logins on hello target server involves three steps outlined below:</span></span>

#### <a name="1-determine-logins-with-access-toohello-primary-database"></a><span data-ttu-id="eee6d-129">1. Határozza meg az access toohello elsődleges adatbázissal bejelentkezések:</span><span class="sxs-lookup"><span data-stu-id="eee6d-129">1. Determine logins with access toohello primary database:</span></span>
<span data-ttu-id="eee6d-130">hello hello folyamatának első lépésében toodetermine mely bejelentkezések hello célkiszolgálón kell készíteni.</span><span class="sxs-lookup"><span data-stu-id="eee6d-130">hello first step of hello process is toodetermine which logins must be duplicated on hello target server.</span></span> <span data-ttu-id="eee6d-131">Ez két KIVÁLASZTÓ utasítást, hello logikai master adatbázis hello forráskiszolgálón szerkezetével és az elsődleges adatbázis hello maga érhető el.</span><span class="sxs-lookup"><span data-stu-id="eee6d-131">This is accomplished with a pair of SELECT statements, one in hello logical master database on hello source server and one in hello primary database itself.</span></span>

<span data-ttu-id="eee6d-132">Csak a kiszolgáló rendszergazdája vagy egy másik hello hello **LoginManager** kiszolgálói szerepkör megállapíthatja, hogy a SELECT utasítás a következő hello hello bejelentkezések hello forráskiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="eee6d-132">Only hello server admin or a member of hello **LoginManager** server role can determine hello logins on hello source server with hello following SELECT statement.</span></span> 

    SELECT [name], [sid] 
    FROM [sys].[sql_logins] 
    WHERE [type_desc] = 'SQL_Login'

<span data-ttu-id="eee6d-133">Csak hello db_owner adatbázis-szerepkör, hello dbo felhasználói vagy kiszolgálói rendszergazda, tag összes hello adatbázis felhasználói hello elsődleges adatbázis rendszerbiztonsági tagok meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="eee6d-133">Only a member of hello db_owner database role, hello dbo user, or server admin, can determine all of hello database user principals in hello primary database.</span></span>

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

#### <a name="2-find-hello-sid-for-hello-logins-identified-in-step-1"></a><span data-ttu-id="eee6d-134">2. 1. lépésben azonosított hello bejelentkezések hello SID található:</span><span class="sxs-lookup"><span data-stu-id="eee6d-134">2. Find hello SID for hello logins identified in step 1:</span></span>
<span data-ttu-id="eee6d-135">Az előző szakaszban hello és egyező hello SID hello lekérdezések hello kimeneti összehasonlításával hello bejelentkezési toodatabase felhasználója is leképezheti.</span><span class="sxs-lookup"><span data-stu-id="eee6d-135">By comparing hello output of hello queries from hello previous section and matching hello SIDs, you can map hello server login toodatabase user.</span></span> <span data-ttu-id="eee6d-136">Egy adatbázis-felhasználó a megfelelő SID-AZONOSÍTÓVAL rendelkező bejelentkezések felhasználói toothat adatbázist, hogy a fő adatbázis felhasználó rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="eee6d-136">Logins that have a database user with a matching SID have user access toothat database as that database user principal.</span></span> 

<span data-ttu-id="eee6d-137">hello következő lekérdezés lehet használt toosee összes hello felhasználó rendszerbiztonsági tagok és az SID-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="eee6d-137">hello following query can be used toosee all of hello user principals and their SIDs in a database.</span></span> <span data-ttu-id="eee6d-138">Csak hello db_owner adatbázis szerepkör- vagy felügyeleti tagjai futtathatják a lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="eee6d-138">Only a member of hello db_owner database role or server admin can run this query.</span></span>

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

> [!NOTE]
> <span data-ttu-id="eee6d-139">Hello **entitástulajdonos** és **sys** felhasználóknál *NULL* SID-k és hello **vendég** SID **0x00**.</span><span class="sxs-lookup"><span data-stu-id="eee6d-139">hello **INFORMATION_SCHEMA** and **sys** users have *NULL* SIDs, and hello **guest** SID is **0x00**.</span></span> <span data-ttu-id="eee6d-140">Hello **dbo** SID kezdődik *0x01060000000001648000000000048454*, ha az adatbázis-készítő hello hello kiszolgálói rendszergazda helyett tagja volt **DbManager**.</span><span class="sxs-lookup"><span data-stu-id="eee6d-140">hello **dbo** SID may start with *0x01060000000001648000000000048454*, if hello database creator was hello server admin instead of a member of **DbManager**.</span></span>
> 
> 

#### <a name="3-create-hello-logins-on-hello-target-server"></a><span data-ttu-id="eee6d-141">3. Hozzon létre hello bejelentkezések hello célkiszolgálón:</span><span class="sxs-lookup"><span data-stu-id="eee6d-141">3. Create hello logins on hello target server:</span></span>
<span data-ttu-id="eee6d-142">hello utolsó lépése toogo toohello célkiszolgáló, vagy kiszolgálókon, és hello hello felmérhetők készítése a megfelelő SID-k.</span><span class="sxs-lookup"><span data-stu-id="eee6d-142">hello last step is toogo toohello target server, or servers, and generate hello logins with hello appropriate SIDs.</span></span> <span data-ttu-id="eee6d-143">hello alapvető szintaxisa a következő.</span><span class="sxs-lookup"><span data-stu-id="eee6d-143">hello basic syntax is as follows.</span></span>

    CREATE LOGIN [<login name>]
    WITH PASSWORD = <login password>,
    SID = <desired login SID>

> [!NOTE]
> <span data-ttu-id="eee6d-144">Ha szeretné toogrant felhasználói hozzáférés toohello másodlagos, de nem toohello elsődleges, azt hello felhasználói bejelentkezés hello elsődleges kiszolgálón megváltoztatásával hello a következő szintaxis használatával teheti meg.</span><span class="sxs-lookup"><span data-stu-id="eee6d-144">If you want toogrant user access toohello secondary, but not toohello primary, you can do that by altering hello user login on hello primary server by using hello following syntax.</span></span>
> 
> <span data-ttu-id="eee6d-145">ALTER LOGIN <login name> LETILTÁSA</span><span class="sxs-lookup"><span data-stu-id="eee6d-145">ALTER LOGIN <login name> DISABLE</span></span>
> 
> <span data-ttu-id="eee6d-146">Tiltsa le nem jelszómódosítás hello, így mindig engedélyezheti azt szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="eee6d-146">DISABLE doesn’t change hello password, so you can always enable it if needed.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="eee6d-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="eee6d-147">Next steps</span></span>
* <span data-ttu-id="eee6d-148">Az adatbázis-hozzáférési és bejelentkezések kezelése további információkért lásd: [SQL-adatbázis biztonsági: adatbázis-hozzáférési és bejelentkezési biztonság kezeléséhez](sql-database-manage-logins.md).</span><span class="sxs-lookup"><span data-stu-id="eee6d-148">For more information on managing database access and logins, see [SQL Database security: Manage database access and login security](sql-database-manage-logins.md).</span></span>
* <span data-ttu-id="eee6d-149">A tartalmazott adatbázis-felhasználók további információkért lásd: [tartalmazott adatbázis-felhasználók – így az adatbázis hordozható](https://msdn.microsoft.com/library/ff929188.aspx).</span><span class="sxs-lookup"><span data-stu-id="eee6d-149">For more information on contained database users, see [Contained Database Users - Making Your Database Portable](https://msdn.microsoft.com/library/ff929188.aspx).</span></span>
* <span data-ttu-id="eee6d-150">Aktív georeplikáció konfigurálásával kapcsolatos további információkért lásd: [aktív georeplikáció](sql-database-geo-replication-overview.md)</span><span class="sxs-lookup"><span data-stu-id="eee6d-150">For information about using and configuring active geo-replication, see [active geo-replication](sql-database-geo-replication-overview.md)</span></span>
* <span data-ttu-id="eee6d-151">Georedundáns helyreállítás használatával kapcsolatos információkért lásd: [georedundáns helyreállítás](sql-database-recovery-using-backups.md#geo-restore)</span><span class="sxs-lookup"><span data-stu-id="eee6d-151">For information about using geo-restore, see [geo-restore](sql-database-recovery-using-backups.md#geo-restore)</span></span>

