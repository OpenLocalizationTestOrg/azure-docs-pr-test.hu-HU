---
title: "Adatbázis vész-helyreállítási aaaSQL |} Microsoft Docs"
description: "Ismerje meg, hogyan toorecover regionális adatközpontban szolgáltatáskimaradás vagy hiba az adatbázis hello Azure SQL Database aktív georeplikáció és georedundáns helyreállítás képességeit."
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: 4800960e-3f9d-40ce-9e55-fb7f2784c067
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/14/2017
ms.author: sashan
ms.openlocfilehash: bae08485863067748107ec4808e52d8e88e2de0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="restore-an-azure-sql-database-or-failover-tooa-secondary"></a>Visszaállítás egy másodlagos Azure SQL Database vagy feladatátvételi tooa
Az Azure SQL-adatbázis a következő lehetőségeket biztosít a kimaradás helyreállítása hello kínálja:

* [Aktív georeplikáció](sql-database-geo-replication-overview.md)
* [Georedundáns helyreállítás](sql-database-recovery-using-backups.md#point-in-time-restore)

toolearn üzleti folytonossági forgatókönyvek és ezek a forgatókönyvek támogatása hello szolgáltatásokkal kapcsolatban lásd: [az üzletmenet folytonossága](sql-database-business-continuity.md).

### <a name="prepare-for-hello-event-of-an-outage"></a>A nem tervezett kimaradás hello esemény előkészítése
A sikeres a helyreállítás tooanother adatterület aktív georeplikáció vagy georedundáns biztonsági mentések van szüksége egy kiszolgáló egy másik adatközponti szolgáltatáskimaradás toobecome hello új elsődleges kiszolgáló tooprepare kell hello kell merülnek fel, valamint jól meghatározott lépései dokumentált és tesztelt tooensure zökkenőmentes helyreállítást. Előkészítő lépések a következők:

* Hello logikai kiszolgáló egy másik régióban toobecome hello új elsődleges kiszolgáló azonosítására szolgál. Az aktív georeplikáció ez lesz, és lehet, hogy mindegyik hello másodlagos kiszolgáló legalább egy. Georedundáns helyreállítás, a rendszer általában lehet egy kiszolgálóhoz az hello [párosított régió](../best-practices-availability-paired-regions.md) hello régió, ahol az adatbázis is található.
* Azonosíthatja, és opcionálisan, hello kiszolgálószintű tűzfal szabályok meghatározásához szükséges a felhasználók tooaccess hello új elsődleges adatbázis.
* Határozza meg, hogyan fog tooredirect felhasználók toohello új elsődleges kiszolgáló, például kapcsolati karakterláncok módosításával, vagy ha megváltoztatja a DNS-bejegyzéseket.
* Azonosíthatja, és opcionálisan létrehozása, hello bejelentkezések kell hello főadatbázis hello új elsődleges kiszolgálón megtalálható, és győződjön meg arról a bejelentkezések megfelelő engedélyek hello főadatbázisban, ha van ilyen. További információkért lásd: [SQL-adatbázis biztonsági katasztrófa utáni helyreállítás után](sql-database-geo-replication-security-config.md)
* A riasztási szabályok, amelyeket toobe frissített toomap toohello új elsődleges adatbázis kell azonosítania.
* A dokumentum hello naplózási konfiguráció hello aktuális elsődleges adatbázis
* Hajtsa végre a [vész-helyreállítási részletezési](sql-database-disaster-recovery-drills.md). egy georedundáns helyreállítás, a szolgáltatáskimaradás toosimulate törölheti vagy nevezze át a hello source adatbázis toocause alkalmazás csatlakozási hiba. az aktív georeplikáció kimaradás toosimulate, letilthatja hello webalkalmazás vagy virtuális gép csatlakoztatott toohello adatbázis vagy a feladatátvételi hello adatbázis toocause alkalmazás kapcsolathibái.

## <a name="when-tooinitiate-recovery"></a>Amikor tooinitiate helyreállítási
hello helyreállítási művelet hello alkalmazás hatással van. Hello SQL kapcsolati karakterlánc vagy a DNS-sel átirányítása módosítani kell, és állandó adatvesztést eredményezhet. Ezért el kell végezni csak a hosszabb, mint az alkalmazás helyreállítási idő célkitűzése valószínűleg toolast hello kimaradás esetén. Ha hello alkalmazás telepített tooproduction hello alkalmazás állapotának rendszeres figyelés végrehajtásához, és a következő adatokat, amelyek hello helyreállítási pontok tooassert indokolt hello használata:

1. Állandó csatlakozási hiba hello alkalmazás réteg toohello adatbázisból.
2. hello Azure-portálon kapcsolatos incidens riasztás széleskörű hatással van a hello régióban jelenik meg.
3. "csökkentett teljesítményű" Hello Azure SQL Database-kiszolgálóhoz van megjelölve.

Attól függően, hogy az alkalmazás tolerancia toodowntime és a lehetséges üzleti felelősség vegye figyelembe a következő helyreállítási beállítások hello.

Használjon hello [helyreállítható adatbázishibák beolvasása](https://msdn.microsoft.com/library/dn800985.aspx) (*LastAvailableBackupDate*) tooget hello legújabb georeplikált visszaállítási pontot.

## <a name="wait-for-service-recovery"></a>Várjon, amíg a szolgáltatás helyreállítás
hello Azure csapatok gondossággal toorestore szolgáltatás rendelkezésre állása, gyorsan lehető, de hello alapvető oka attól függően is igénybe vehet, órák vagy napok.  Ha az alkalmazás működését egyszerűen megvárhatja hello helyreállítási toocomplete jelentős állásidőt. Ebben az esetben a letöltés intézkedés nem szükséges. A szolgáltatás jelenlegi állapota hello tekintheti meg a [Azure az állapotjelző irányítópulthoz](https://azure.microsoft.com/status/). Hello régió hello helyreállítása után az alkalmazás rendelkezésre állásának visszaáll.

## <a name="fail-over-toogeo-replicated-secondary-database"></a>Feladatok átadása a másodlagos adatbázis toogeo replikált
Ha az alkalmazás állásidőt eredményezhet üzleti felelősség kell használni georeplikált adatbázisának vagy adatbázisainak az alkalmazásban. Ez lehetővé teszi a hello tooquickly visszaállítási rendelkezésre állásának egy esetleges leállás esetén egy másik régióban. Ismerje meg, hogyan túl[georeplikáció konfigurálása](sql-database-geo-replication-portal.md).

hello toorestore rendelkezésre állását database(s) meg kell tooinitiate hello feladatátvételi toohello georeplikált másodlagos hello támogatott módszerek egyikének használatával.

Hello útmutatók toofail keresztül tooa georeplikált másodlagos adatbázis a következő egyikét használja:

* [Feladatok átadása tooa georeplikált másodlagos Azure portál használatával](sql-database-geo-replication-portal.md)
* [Georeplikált másodlagos tooa átkapcsolás PowerShell használatával](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
* [Feladatok átadása tooa georeplikált másodlagos T-SQL használatával](sql-database-geo-replication-transact-sql.md)

## <a name="recover-using-geo-restore"></a>Georedundáns helyreállítás használatával állítsa helyre
Ha az alkalmazás állásidő üzleti felelősség eredményez [georedundáns helyreállítás](sql-database-recovery-using-backups.md) mint egy metódus toorecover az alkalmazás-adatbázisra. Hello adatbázisát a legújabb georedundáns biztonsági másolatból hoz létre.

## <a name="configure-your-database-after-recovery"></a>A helyreállítás után az adatbázis konfigurálása
Georeplikálási feladatátvételi vagy a kimaradás georedundáns helyreállítás toorecover használatakor győződjön meg arról, hogy hello kapcsolat toohello új adatbázisok helyesen van-e konfigurálva, hogy a normál alkalmazási függvény hello folytathatók. A feladatok tooget ellenőrzőlistáját a helyreállított adatbázis éles készen van.

### <a name="update-connection-strings"></a>A kapcsolati karakterláncok frissítése
Mivel a helyreállított adatbázis egy másik kiszolgálón legyen elhelyezve, meg kell tooupdate az alkalmazás kapcsolódási karakterlánc toopoint toothat kiszolgáló.

Kapcsolati karakterláncok módosításával kapcsolatos további információkért lásd: hello megfelelő fejlesztői nyelvét a [kapcsolattára](sql-database-libraries.md).

### <a name="configure-firewall-rules"></a>Tűzfalszabályok konfigurálása
Meg kell, hogy hello tűzfalszabályok konfigurálása kiszolgálón és hello adatbázis egyezés azokat eredetileg konfigurálták a hello elsődleges kiszolgáló és az elsődleges adatbázis toomake. További információkért lásd: [hogyan: tűzfal beállításainak konfigurálása (Azure SQL Database)](sql-database-configure-firewall-settings.md).

### <a name="configure-logins-and-database-users"></a>Bejelentkezések és adatbázis-felhasználók konfigurálása
Meg kell, hogy létezik-e az alkalmazás által használt összes hello bejelentkezések hello a kiszolgálón, amely üzemelteti a helyreállított adatbázis toomake. További információkért lásd: [biztonsági beállítások a georeplikációért](sql-database-geo-replication-security-config.md).

> [!NOTE]
> Állítsa be, és a kiszolgáló tűzfalszabályainak és bejelentkezések (és a rájuk vonatkozó engedélyek) Ellenőrizze a vész-helyreállítási részletezési során. A kiszolgálói szintű objektumok és azok konfigurációja nem érhetők el hello kimaradás során.
> 
> 

### <a name="setup-telemetry-alerts"></a>A telepítő telemetriai riasztások
Toomake arról, hogy a meglévő riasztási szabály beállítások frissített toomap toohello helyreállított adatbázis és hello másik kiszolgálóra van szüksége.

Adatbázis értesítési szabályokkal kapcsolatos további információkért lásd: [riasztás értesítések fogadása](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) és [követése szolgáltatásának állapota](../monitoring-and-diagnostics/insights-service-health.md).

### <a name="enable-auditing"></a>Naplózás engedélyezése
Ha a naplózás tooaccess szükség van az adatbázis tooenable kell naplózás hello adatbázis helyreállítása után. További információkért lásd: [adatbázis naplózásának](sql-database-auditing.md).

## <a name="next-steps"></a>Következő lépések
* tudnivalók Azure SQL adatbázis automatikus biztonsági mentés, toolearn lásd: [SQL-adatbázis automatikus biztonsági mentés](sql-database-automated-backups.md)
* toolearn kapcsolatos üzleti folytonossági tervezési és helyreállítási forgatókönyvek, lásd: [folytonosságának forgatókönyvek](sql-database-business-continuity.md)
* toolearn a helyreállításhoz, az automatikus biztonsági mentés használatával kapcsolatban lásd: [adatbázis visszaállítása biztonsági másolatból hello szolgáltatás által kezdeményezett](sql-database-recovery-using-backups.md)

