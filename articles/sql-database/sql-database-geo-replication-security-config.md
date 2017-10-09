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
# <a name="configure-and-manage-azure-sql-database-security-for-geo-restore-or-failover"></a>Konfigurálhatja és kezelheti az Azure SQL Database biztonsági georedundáns helyreállítás vagy feladatátvételi 

> [!NOTE]
> [Aktív georeplikáció](sql-database-geo-replication-overview.md) érhető el az összes olyan adatbázis összes szolgáltatásrétegeiben használt funkciókkal.
>  

## <a name="overview-of-authentication-requirements-for-disaster-recovery"></a>Vész-helyreállítási hitelesítési követelmények áttekintése
Ez a témakör ismerteti a hello hitelesítési követelmények tooconfigure és vezérlés [aktív georeplikáció](sql-database-geo-replication-overview.md) és hello felhasználói hozzáférés toohello másodlagos adatbázis szükséges tooset lépéseket. Azt is ismerteti, hogyan engedélyezze a hozzáférést toohello helyreállított adatbázis használata után [georedundáns helyreállítás](sql-database-recovery-using-backups.md#geo-restore). Helyreállítási lehetőségekről további információkért lásd: [üzleti folytonosság – áttekintés](sql-database-business-continuity.md).

## <a name="disaster-recovery-with-contained-users"></a>A tartalmazott felhasználók katasztrófa utáni helyreállítás
Hagyományos felhasználók eltérően toologins hello master adatbázisban kell lennie, amely leképezve, egy felhasználó maga hello adatbázis teljesen kezeli. Azt két előnnyel jár. Hello vészhelyreállítás esetén hello felhasználók továbbra is tooconnect toohello új elsődleges adatbázis vagy hello adatbázis helyre georedundáns helyreállítás, további konfiguráció nélkül használja, mert hello adatbázis hello felhasználók kezeli. Nincsenek is lehetséges méretezhetőségét és teljesítményét számos előnyt biztosít az ebben a konfigurációban egy bejelentkezési szempontjából. További információt a [tartalmazottadatbázis-felhasználókkal kapcsolatos, az adatbázis hordozhatóvá tételével foglalkozó](https://msdn.microsoft.com/library/ff929188.aspx) cikkben talál. 

hello fő kompromisszum az, hogy hello vész-helyreállítási folyamat léptékű kezelése bonyolult. Ha több adatbázist, hogy használjon hello azonos bejelentkezési karbantartása hello hitelesítő adatokat tartalmazott a felhasználók több adatbázis előfordulhat, hogy semlegesítsék tartalmazott felhasználók hello előnyeit. Például hello Elforgatás jelszóházirend megköveteli-, hogy módosítható következetesen több adatbázisok helyett hello bejelentkezési azonosító változó hello jelszavának egyszer hello master adatbázisban. Ezért, ha több adatbázist, hogy használjon hello ugyanazt a felhasználónevet és jelszót, a tartalmazott felhasználók használata nem ajánlott. 

## <a name="how-tooconfigure-logins-and-users"></a>Hogyan tooconfigure bejelentkezéseket és a felhasználók
Bejelentkezések és felhasználók használata (helyett a benne lévő felhasználók), érdemes további lépések tooinsure, hogy ugyanazt a bejelentkezési adatok léteznek hello hello master adatbázisban. hello alábbi szakaszok felsorolják hello lépéseket érintett és további szempontokat.

### <a name="set-up-user-access-tooa-secondary-or-recovered-database"></a>Felhasználói hozzáférés tooa másodlagos vagy a helyreállított adatbázis létrehozása
Ahhoz, hogy hello másodlagos adatbázis toobe használható, egy csak olvasható másodlagos adatbázis és tooensure megfelelő hozzáféréssel toohello új elsődleges adatbázis vagy hello adatbázis helyreállítása a földrajzi-visszaállítással, hello fő célkiszolgáló hello kell biztosítani hello megfelelő biztonsági konfiguráció hello helyreállítás előtt a helyükön.

a témakör későbbi részében hello egyes lépéseihez szükséges engedélyeket ismerteti.

Felkészülés a felhasználói hozzáférés tooa georeplikáció másodlagos georeplikáció konfigurálása során kell végrehajtani. Felkészülés a felhasználói hozzáférés toohello földrajzi vissza adatbázisok kell hajtható végre, tetszőleges időpontban, amikor hello eredeti kiszolgáló online állapotban-e (pl. hello vész-Helyreállítási részletezési részeként).

> [!NOTE]
> Ha nem sikerül over vagy georedundáns helyreállítás, amely nem rendelkezik megfelelően konfigurált bejelentkezések tooa server hozzáférés tooit lesz korlátozott toohello server rendszergazdai fiók.
> 
> 

Az alábbiakban leírt három lépést beállítása a célkiszolgálón hello bejelentkezések foglal magában:

#### <a name="1-determine-logins-with-access-toohello-primary-database"></a>1. Határozza meg az access toohello elsődleges adatbázissal bejelentkezések:
hello hello folyamatának első lépésében toodetermine mely bejelentkezések hello célkiszolgálón kell készíteni. Ez két KIVÁLASZTÓ utasítást, hello logikai master adatbázis hello forráskiszolgálón szerkezetével és az elsődleges adatbázis hello maga érhető el.

Csak a kiszolgáló rendszergazdája vagy egy másik hello hello **LoginManager** kiszolgálói szerepkör megállapíthatja, hogy a SELECT utasítás a következő hello hello bejelentkezések hello forráskiszolgálón. 

    SELECT [name], [sid] 
    FROM [sys].[sql_logins] 
    WHERE [type_desc] = 'SQL_Login'

Csak hello db_owner adatbázis-szerepkör, hello dbo felhasználói vagy kiszolgálói rendszergazda, tag összes hello adatbázis felhasználói hello elsődleges adatbázis rendszerbiztonsági tagok meghatározásához.

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

#### <a name="2-find-hello-sid-for-hello-logins-identified-in-step-1"></a>2. 1. lépésben azonosított hello bejelentkezések hello SID található:
Az előző szakaszban hello és egyező hello SID hello lekérdezések hello kimeneti összehasonlításával hello bejelentkezési toodatabase felhasználója is leképezheti. Egy adatbázis-felhasználó a megfelelő SID-AZONOSÍTÓVAL rendelkező bejelentkezések felhasználói toothat adatbázist, hogy a fő adatbázis felhasználó rendelkezik. 

hello következő lekérdezés lehet használt toosee összes hello felhasználó rendszerbiztonsági tagok és az SID-adatbázisban. Csak hello db_owner adatbázis szerepkör- vagy felügyeleti tagjai futtathatják a lekérdezés.

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

> [!NOTE]
> Hello **entitástulajdonos** és **sys** felhasználóknál *NULL* SID-k és hello **vendég** SID **0x00**. Hello **dbo** SID kezdődik *0x01060000000001648000000000048454*, ha az adatbázis-készítő hello hello kiszolgálói rendszergazda helyett tagja volt **DbManager**.
> 
> 

#### <a name="3-create-hello-logins-on-hello-target-server"></a>3. Hozzon létre hello bejelentkezések hello célkiszolgálón:
hello utolsó lépése toogo toohello célkiszolgáló, vagy kiszolgálókon, és hello hello felmérhetők készítése a megfelelő SID-k. hello alapvető szintaxisa a következő.

    CREATE LOGIN [<login name>]
    WITH PASSWORD = <login password>,
    SID = <desired login SID>

> [!NOTE]
> Ha szeretné toogrant felhasználói hozzáférés toohello másodlagos, de nem toohello elsődleges, azt hello felhasználói bejelentkezés hello elsődleges kiszolgálón megváltoztatásával hello a következő szintaxis használatával teheti meg.
> 
> ALTER LOGIN <login name> LETILTÁSA
> 
> Tiltsa le nem jelszómódosítás hello, így mindig engedélyezheti azt szükség esetén.
> 
> 

## <a name="next-steps"></a>Következő lépések
* Az adatbázis-hozzáférési és bejelentkezések kezelése további információkért lásd: [SQL-adatbázis biztonsági: adatbázis-hozzáférési és bejelentkezési biztonság kezeléséhez](sql-database-manage-logins.md).
* A tartalmazott adatbázis-felhasználók további információkért lásd: [tartalmazott adatbázis-felhasználók – így az adatbázis hordozható](https://msdn.microsoft.com/library/ff929188.aspx).
* Aktív georeplikáció konfigurálásával kapcsolatos további információkért lásd: [aktív georeplikáció](sql-database-geo-replication-overview.md)
* Georedundáns helyreállítás használatával kapcsolatos információkért lásd: [georedundáns helyreállítás](sql-database-recovery-using-backups.md#geo-restore)

