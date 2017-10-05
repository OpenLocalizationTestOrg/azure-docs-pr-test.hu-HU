---
title: "SQL-adatbázis vészhelyreállítás |} Microsoft Docs"
description: "Megtudhatja, hogyan regionális adatközpontban szolgáltatáskimaradás vagy az Azure SQL Database aktív georeplikáció, georedundáns helyreállítás képességek sikertelen egy adatbázis helyreállítása."
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
ms.openlocfilehash: e33f69bf04b32a31aae3c311c41aa44e4da5016a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="restore-an-azure-sql-database-or-failover-to-a-secondary"></a>Egy Azure SQL Database vagy feladatátvételi visszaállításához a másodlagos
Az Azure SQL-adatbázis helyreállításához a kimaradás a következő szolgáltatásokat biztosítja:

* [Aktív georeplikáció](sql-database-geo-replication-overview.md)
* [Georedundáns helyreállítás](sql-database-recovery-using-backups.md#point-in-time-restore)

Üzleti folytonossági forgatókönyvek és a szolgáltatások, ezek a forgatókönyvek támogatása, lásd: [az üzletmenet folytonossága](sql-database-business-continuity.md).

### <a name="prepare-for-the-event-of-an-outage"></a>Az esemény a nem tervezett kimaradás előkészítése
A sikeres a helyreállítás egy másik adatterülethez aktív georeplikáció vagy georedundáns biztonsági mentések használatával, elő kell készíteni egy kiszolgáló egy másik adatközpontban szolgáltatáskimaradás új elsődleges kiszolgálóként való amennyiben szükséges, valamint jól meghatározott lépései dokumentált, és tesztelt, zökkenőmentes helyreállítási merülhetnek fel. Előkészítő lépések a következők:

* A logikai kiszolgáló egy másik régióban legyen, az új elsődleges kiszolgáló azonosítására szolgál. Az aktív georeplikáció ez lesz, és lehet, hogy mindegyik másodlagos kiszolgáló legalább egy. Georedundáns helyreállítás, a rendszer általában lehet a kiszolgáló a [párosított régió](../best-practices-availability-paired-regions.md) a régió, ahol az adatbázis is található.
* Azonosítsa, és opcionálisan, a kiszolgálószintű tűzfal szabályok definiálása a felhasználók az új elsődleges adatbázis eléréséhez szükséges.
* Határozza meg, hogyan kívánja irányítsa át a felhasználókat az új elsődleges kiszolgáló, például kapcsolati karakterláncok módosításával, vagy ha megváltoztatja a DNS-bejegyzéseket.
* Határozza meg, és másik lehetőségként létrehozhatja a bejelentkezések szerepel a fő adatbázist az új elsődleges kiszolgáló, és győződjön meg arról, a bejelentkezési adatok rendelkezik megfelelő engedélyekkel a master adatbázisban, ha van ilyen. További információkért lásd: [SQL-adatbázis biztonsági katasztrófa utáni helyreállítás után](sql-database-geo-replication-security-config.md)
* A riasztási szabályok, amelyeket frissítenie kell az új elsődleges adatbázis hozzárendelése kell azonosítani.
* Dokumentálja az aktuális elsődleges adatbázis naplózási konfiguráció
* Hajtsa végre a [vész-helyreállítási részletezési](sql-database-disaster-recovery-drills.md). A georedundáns helyreállítás kimaradás szimulálása, törölheti vagy nevezze át a forrásadatbázis alkalmazás csatlakozási hiba okozza. Az aktív georeplikáció kimaradás szimulálása, letilthatja a webalkalmazáshoz vagy a virtuális gép csatlakozik az adatbázis vagy az adatbázis feladatátvétel alkalmazás csatlakozási hibákhoz vezethet.

## <a name="when-to-initiate-recovery"></a>Mikor érdemes elindítani a helyreállítást
A helyreállítási művelet hatással van az alkalmazás. Az SQL kapcsolati karakterlánc vagy a DNS-sel átirányítása módosítani kell, és állandó adatvesztést eredményezhet. Ezért el kell végezni csak az utolsó tovább, mint az alkalmazás helyreállítási idő célkitűzése valószínűleg a szolgáltatáskimaradás esetén. Ha az alkalmazás központi telepítése éles az alkalmazás állapotának rendszeres figyelés végrehajtásához, és, hogy a helyreállítási indokolt-e az állítás használja a következő adatpontokhoz:

1. Állandó csatlakozási hiba az alkalmazás rétegtől az adatbázisba.
2. Az Azure-portálon kapcsolatos incidens riasztás széleskörű hatással van a régióban jelenik meg.
3. Csökkentett teljesítményű az Azure SQL adatbázis-kiszolgáló van megjelölve.

Attól függően, hogy az alkalmazás tűréshatár és a lehetséges üzleti felelősséget vegye figyelembe a következő helyreállítási beállítások.

Használja a [helyreállítható adatbázishibák beolvasása](https://msdn.microsoft.com/library/dn800985.aspx) (*LastAvailableBackupDate*) a legújabb georeplikált visszaállítási pontok eléréséhez.

## <a name="wait-for-service-recovery"></a>Várjon, amíg a szolgáltatás helyreállítás
Az Azure-csoportok munkát gondossággal állítsa vissza a szolgáltatás rendelkezésre állása gyorsan lehető, de attól függően, hogy a legfelső szintű oka az órák vagy napok vehet igénybe.  Ha az alkalmazás működését jelentős állásidőt egyszerűen megvárhatja a helyreállítás végrehajtásához. Ebben az esetben a letöltés intézkedés nem szükséges. A szolgáltatás jelenlegi állapota tekintheti meg a [Azure az állapotjelző irányítópulthoz](https://azure.microsoft.com/status/). A régió a helyreállítás után az alkalmazás rendelkezésre állásának visszaáll.

## <a name="fail-over-to-geo-replicated-secondary-database"></a>Feladatok átadása a georeplikált másodlagos adatbázis
Ha az alkalmazás állásidőt eredményezhet üzleti felelősség kell használni georeplikált adatbázisának vagy adatbázisainak az alkalmazásban. Ez lehetővé teszi az alkalmazás egy másik régióban egy esetleges leállás esetén rendelkezésre állási gyors visszaállításához. Megtudhatja, hogyan [georeplikáció konfigurálása](sql-database-geo-replication-portal.md).

Kezdeményezze a feladatátvételt, a georeplikált másodlagos a támogatott módszerek egyikének használatával kell az adatbázis(ok) rendelkezésre állását visszaállítására.

Georeplikált másodlagos adatbázishoz feladatátvételt használja az alábbi útmutatókban egyikét:

* [Feladatok átadása a georeplikált másodlagos Azure portál használatával](sql-database-geo-replication-portal.md)
* [Feladatok átadása a georeplikált másodlagos PowerShell használatával](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
* [Feladatok átadása a T-SQL használatával georeplikált másodlagos](sql-database-geo-replication-transact-sql.md)

## <a name="recover-using-geo-restore"></a>Georedundáns helyreállítás használatával állítsa helyre
Ha az alkalmazás állásidő üzleti felelősség eredményez [georedundáns helyreállítás](sql-database-recovery-using-backups.md) az alkalmazás-adatbázisok helyreállítása módszerként. A legújabb georedundáns biztonsági másolatból létrehoz egy másolatot az adatbázisról.

## <a name="configure-your-database-after-recovery"></a>A helyreállítás után az adatbázis konfigurálása
Ha georeplikáció feladatátvételi vagy georedundáns helyreállítás kimaradás helyreállítás használ, győződjön meg arról, hogy a kapcsolatot az új adatbázisok helyesen van-e konfigurálva, hogy a normál alkalmazási függvény folytathatók. Ez az ellenőrzőlista a helyreállított adatbázis éles kész állapotba hozásához feladatok.

### <a name="update-connection-strings"></a>A kapcsolati karakterláncok frissítése
A helyreállított adatbázis legyen elhelyezve egy másik kiszolgálón, mert kell frissíteni az alkalmazás kapcsolati karakterláncot, hogy adott kiszolgálóra mutasson.

Kapcsolati karakterláncok módosításával kapcsolatos további információkért tekintse meg a megfelelő fejlesztői nyelvét a [kapcsolattára](sql-database-libraries.md).

### <a name="configure-firewall-rules"></a>Tűzfalszabályok konfigurálása
Győződjön meg arról, hogy a tűzfal szabályaiban konfigurált kiszolgálón és az adatbázis pontosan megegyeznek az elsődleges kiszolgáló és az elsődleges adatbázis be lett állítva kell. További információkért lásd: [hogyan: tűzfal beállításainak konfigurálása (Azure SQL Database)](sql-database-configure-firewall-settings.md).

### <a name="configure-logins-and-database-users"></a>Bejelentkezések és adatbázis-felhasználók konfigurálása
Győződjön meg arról, hogy az alkalmazás által használt bejelentkezési adatok léteznek-e a kiszolgálón, amely üzemelteti a helyreállított adatbázis kell. További információkért lásd: [biztonsági beállítások a georeplikációért](sql-database-geo-replication-security-config.md).

> [!NOTE]
> Állítsa be, és a kiszolgáló tűzfalszabályainak és bejelentkezések (és a rájuk vonatkozó engedélyek) Ellenőrizze a vész-helyreállítási részletezési során. A kiszolgálói szintű objektumok és azok konfigurációja nem érhetők el a szolgáltatáskimaradás elhárítása során.
> 
> 

### <a name="setup-telemetry-alerts"></a>A telepítő telemetriai riasztások
Győződjön meg arról, hogy a meglévő riasztási szabály beállítások hozzárendelése a helyreállított adatbázis és a másik kiszolgáló frissítését kell.

Adatbázis értesítési szabályokkal kapcsolatos további információkért lásd: [riasztás értesítések fogadása](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) és [követése szolgáltatásának állapota](../monitoring-and-diagnostics/insights-service-health.md).

### <a name="enable-auditing"></a>Naplózás engedélyezése
Ha az adatbázis eléréséhez szükség van a naplózás szüksége az adatbázis helyreállítása után a naplózás engedélyezéséhez. További információkért lásd: [adatbázis naplózásának](sql-database-auditing.md).

## <a name="next-steps"></a>Következő lépések
* További tudnivalók az Azure SQL adatbázis automatikus biztonsági mentés című [SQL-adatbázis automatikus biztonsági mentés](sql-database-automated-backups.md)
* Üzleti folytonosság tervezési és helyreállítási forgatókönyvekkel kapcsolatos további tudnivalókért lásd: [folytonosságának forgatókönyvek](sql-database-business-continuity.md)
* A helyreállítás automatikus biztonsági mentés használatával kapcsolatos további tudnivalókért lásd: [adatbázis visszaállítása a szolgáltatás által kezdeményezett biztonsági másolatból](sql-database-recovery-using-backups.md)

