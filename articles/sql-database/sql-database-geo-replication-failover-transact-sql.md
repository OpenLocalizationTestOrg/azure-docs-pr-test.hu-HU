---
title: "Az Azure SQL Database TSQL:initiate feladatátvételi |} Microsoft Docs"
description: "Kezdeményezzen tervezett vagy nem tervezett feladatátvételt az Azure SQL Database Transact-SQL használatával"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 5eb2d256-025d-4f5a-99d4-17f702b37f14
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: 418953e044ba84ce758063d56a371af45d5cdfe1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-transact-sql"></a>Kezdeményezzen tervezett vagy nem tervezett feladatátvételt az Azure SQL Database Transact-SQL

Ez a cikk bemutatja, hogyan tooinitiate feladatátvételi tooa másodlagos SQL-adatbázis a Transact-SQL használatával. tooconfigure georeplikáció, lásd: [georeplikáció konfigurálása az Azure SQL Database](sql-database-geo-replication-transact-sql.md).

a feladatátvétel tooinitiate következőkre lesz szüksége hello:

* A bejelentkezési azonosítót, amely a DBManager az elsődleges hello
* Hello helyi adatbázis földrajzi-replikálás fogja végrehajtani db_ownership rendelkezik
* A georeplikáció konfigurálja a DBManager lehet hello partner kiszolgáló(k) toowhich
* Legújabb verziója az SQL Server Management Studio (SSMS)

> [!IMPORTANT]
> Javasoljuk, hogy mindig használjon hello Azure és az SQL-adatbázis a frissítések tooMicrosoft szinkronizálva tooremain Management Studio legújabb verzióját. [Az SQL Server Management Studio frissítése](https://msdn.microsoft.com/library/mt238290.aspx).
>  

## <a name="initiate-a-planned-failover-promoting-a-secondary-database-toobecome-hello-new-primary"></a>Egy másodlagos adatbázis toobecome hello új elsődleges előléptetni egy tervezett feladatátvételt kezdeményezni
Használhatja a hello **ALTER DATABASE** utasítás toopromote egy másodlagos adatbázis toobecome hello új elsődleges adatbázis hello meglévő elsődleges toobecome lefokozásához, a tervezett módon egy másodlagos. Az utasítás végrehajtása hello főadatbázis hello Azure SQL Database logikai kiszolgáló, mely hello georeplikált másodlagos adatbázis, amely az előléptetendő található. Ez a funkció célja a tervezett feladatátvétel, többek között során hello vész-Helyreállítási gyakorlatokat, és hogy hello az elsődleges adatbázis rendelkezésre kell lenniük.

hello parancs a következő munkafolyamat hello hajtja végre:

1. Ideiglenesen kapcsolók replikációs toosynchronous módban, amely az összes nyitott tranzakciók toobe kiürített toohello másodlagos, és blokkolja az összes új tranzakciók;
2. Kapcsolók hello hello két adatbázis hello georeplikáció együttműködve szerepkörök.  

Ebben a sorozatban biztosítja, hogy az adott hello két adatbázis szinkronizálva előtt hello szerepkörök váltson, és így nincs adatvesztés történik. Egy rövid időszak során, ami két adatbázis nem érhető el (a 0 too25 másodperc hello sorrendben) közben hello szerepkörök bekapcsolt állapotban van. Ha a hello elsődleges adatbázis több másodlagos adatbázist, hello parancs lesz automatikusan reconfigure hello más másodlagos tooconnect toohello új elsődleges.  hello teljes műveletet kell végrehajtani kisebb, mint egy perc toocomplete normál körülmények között. További információkért lásd: [(Transact-SQL) adatbázis módosítása](https://msdn.microsoft.com/library/mt574871.aspx) és [szolgáltatásszintek](sql-database-service-tiers.md).

A következő lépéseket tooinitiate egy tervezett feladatátvételt hello használata.

1. A Management Studio eszközben csatlakozzon a toohello Azure SQL Database logikai kiszolgáló, amely egy georeplikált másodlagos adatbázis található.
2. Nyissa meg a hello adatbázismappát, és bontsa ki a hello **Rendszeradatbázisokban** mappát, kattintson a jobb gombbal a **fő**, és kattintson a **új lekérdezés**.
3. Hello következő **ALTER DATABASE** utasítás tooswitch hello másodlagos adatbázis toohello elsődleges szerepkör.
   
        ALTER DATABASE <MyDB> FAILOVER;
4. Kattintson a **Execute** toorun hello lekérdezés.

> [!NOTE]
> Bizonyos ritkán előforduló esetekben akkor lehetséges, hogy hello művelet nem hajtható végre, és a lefagyott jelennek meg. Ebben az esetben hello felhasználói hello kényszerített feladatátvételi parancs végrehajtása és fogadja el az adatvesztés.
> 
> 

## <a name="initiate-an-unplanned-failover-from-hello-primary-database-toohello-secondary-database"></a>Hello elsődleges adatbázis toohello másodlagos adatbázis egy nem tervezett feladatátvételt kezdeményezni
Használhatja a hello **ALTER DATABASE** utasítás toopromote egy másodlagos adatbázis toobecome hello új elsődleges adatbázis hello meglévő elsődleges toobecome hello lefokozása kényszerítése, egy nem tervezett módon másodlagos egyszerre amikor hello elsődleges adatbázis már nem érhető el. Az utasítás végrehajtása hello főadatbázis hello Azure SQL Database logikai kiszolgáló, mely hello georeplikált másodlagos adatbázis, amely az előléptetendő található.

Ez a funkció a vész-helyreállítási szolgál, ha fontos hello adatbázis visszaállítási rendelkezésre állását és adatvesztést elfogadható. Kényszerített feladatátvételi meghívásakor hello megadott másodlagos adatbázis azonnal hello elsődleges adatbázis válik, és írási tranzakción elfogadása kezdődik. Amint hello eredeti elsődleges adatbázis képes tooreconnect az új elsődleges adatbázissal, a növekményes biztonsági mentés születik hello eredeti elsődleges adatbázis és hello régi elsődleges adatbázist állítanak be egy új elsődleges adatbázis hello; másodlagos adatbázis Ezt követően már csupán szinkronizálási hello új elsődleges replika.

Azonban mivel a pont a kötött visszaállítás nem támogatott a hello másodlagos adatbázisok, ha hello felhasználó által elkövetett toorecover adatok toohello régi elsődleges adatbázis, amely nem volt replikált toohello új elsődleges adatbázis előtt hello kényszerített feladatátvétel történt, hello a felhasználónak kell tooengage támogatási toorecover Ez az adatvesztés.

Ha a hello elsődleges adatbázis több másodlagos adatbázist, hello parancs lesz automatikusan reconfigure hello más másodlagos tooconnect toohello új elsődleges.

A következő lépéseket tooinitiate feladatátvételhez hello használata.

1. A Management Studio eszközben csatlakozzon a toohello Azure SQL Database logikai kiszolgáló, amely egy georeplikált másodlagos adatbázis található.
2. Nyissa meg a hello adatbázismappát, és bontsa ki a hello **Rendszeradatbázisokban** mappát, kattintson a jobb gombbal a **fő**, és kattintson a **új lekérdezés**.
3. Hello következő **ALTER DATABASE** utasítás tooswitch hello másodlagos adatbázis toohello elsődleges szerepkör.
   
        ALTER DATABASE <MyDB>   FORCE_FAILOVER_ALLOW_DATA_LOSS;
4. Kattintson a **Execute** toorun hello lekérdezés.

> [!NOTE]
> Ha az elsődleges és másodlagos is online állapotba kerülnek hello parancs kiadása hello régi elsődleges válik az adatszinkronizálás azonnali nélkül új másodlagos hello. Ha elsődleges hello tranzakciók végrehajtása, ha hello parancs kiadása adatvesztést fordulhat elő.
> 
> 

## <a name="next-steps"></a>Következő lépések
* A feladatátvétel után ellenőrizze a kiszolgáló és az adatbázis hello hitelesítési követelmények hello új elsődleges. További információkért lásd: [SQL-adatbázis biztonsági katasztrófa utáni helyreállítás után](sql-database-geo-replication-security-config.md).
* aktív georeplikáció, beleértve a helyreállítási előtti és utáni helyreállítás lépéseit, és végrehajtása egy vész-helyreállítási részletezési használata katasztrófa utáni helyreállítás toolearn lásd [katasztrófa utáni helyreállítás](sql-database-disaster-recovery.md)
* Egy Sasha Nosov aktív georeplikáció kapcsolatos blogbejegyzést, lásd: [új georeplikáció képességek a Spotlight](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
* Felhőalapú alkalmazások toouse aktív georeplikáció megtervezésével kapcsolatos további információkért lásd: [felhőalapú alkalmazások megtervezése az üzletmenet folytonossága érdekében georeplikáció használatával](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
* Aktív georeplikáció a rugalmas készletek használatával kapcsolatos információkért lásd: [rugalmas készlet vész-helyreállítási stratégiák](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
* Az üzletmenet folytonossága áttekintését lásd: [üzleti folytonosság – áttekintés](sql-database-business-continuity.md)

