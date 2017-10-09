---
title: "aaaAzure SQL-adatbázis – gyakori kérdések |} Microsoft Docs"
description: "Válaszok toocommon kérdések ügyfelek kérdezzen felhő adatbázisok és az Azure SQL Database, a Microsoft relációs adatbázis-kezelő rendszerének (RDBMS) és adatbázis hello felhőalapú szolgáltatásként."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 1da12abc-0646-43ba-b564-e3b049a6487f
ms.service: sql-database
ms.custom: reference
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 02/07/2017
ms.author: sashan;carlrab
ms.openlocfilehash: 04c02f96e6e91cf314221134ee0ef6d24217ef45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sql-database-faq"></a>SQL Database GYIK

## <a name="what-is-hello-current-version-of-sql-database"></a>Mi az az SQL-adatbázis jelenlegi verziója hello?
hello aktuális az SQL-adatbázis verziója 12-es verzió. Verzió V11 eltávolították.

## <a name="what-is-hello-sla-for-sql-database"></a>Mi az az SQL-adatbázis hello SLA-t?
Garantáljuk legalább hello idő ügyfelek 99,99 %-át lesz az egyetlen, akár a rugalmas Basic, Standard, vagy prémium szintű Microsoft Azure SQL Database és az internetes átjáró közötti kapcsolatot. További információkért lásd: [SLA](http://azure.microsoft.com/support/legal/sla/).

## <a name="how-do-i-reset-hello-password-for-hello-server-admin"></a>Hogyan alaphelyzetbe állítása hello kiszolgáló rendszergazdai jelszavát hello?
A hello [Azure-portálon](https://portal.azure.com) kattintson **SQL Server-kiszolgálók**, válassza ki a hello server hello listából, és kattintson **jelszó alaphelyzetbe állítása**.

## <a name="how-do-i-manage-databases-and-logins"></a>Hogyan kezelhető adatbázisok és bejelentkezések?
Lásd: [adatbázisok és bejelentkezések kezelése](sql-database-manage-logins.md).

## <a name="how-do-i-make-sure-only-authorized-ip-addresses-are-allowed-tooaccess-a-server"></a>Hogyan tehetem arról, hogy csak engedélyezett IP-címek engedélyezett tooaccess egy kiszolgálót?
Lásd: [hogyan: SQL Database tűzfalbeállításainak konfigurálása](sql-database-configure-firewall-settings.md).

## <a name="how-does-hello-usage-of-sql-database-show-up-on-my-bill"></a>SQL-adatbázis hello használati hogyan nem jelennek meg a saját számlázási?
SQL-adatbázis váltók a egy előre jelezhető óránkénti arány alapján hello szolgáltatásréteg + teljesítmény mind önálló adatbázisok és rugalmas készletenként felhasználható edtu-k szint. Tényleges használathoz számított és egyenletesen szerint óránként, a számlán törtek az óra lehet, hogy megjelenítése. Például ha egy adatbázist egy hónapban a 12 óra, a számlán 0,5 nap-használatát mutatja. Emellett szolgáltatásszintek + teljesítményszintet és készletenként felhasználható edtu-k nem működik ki a hello számlázási toomake azt könnyebb toosee hello hány adatbázist használt minden, az adott hónapban.

## <a name="what-if-a-single-database-is-active-for-less-than-an-hour-or-uses-a-higher-service-tier-for-less-than-an-hour"></a>Mi történik, ha egy önálló adatbázis aktív kisebb, mint egy óráig vagy magasabb szolgáltatásréteg kisebb, mint egy óráig használ?
Az adatbázis létezik használatával óránként hello legmagasabb szolgáltatásréteg + teljesítmény szinten használati vagy hello adatbázis volt-e aktív kisebb, mint egy óráig függetlenül, hogy órán belül alkalmazott kell fizetni. Például ha egy önálló adatbázis létrehozása, és törölje azt 5 percen belül a számlázási egy adatbázis óra díjat tükrözi. 

Példák

* Ha egy alapszintű adatbázis létrehozása, majd azonnal frissíteni tooStandard S1 van szó, a hello hello standard szintű, S1 díj első óra.
* Ha egy adatbázist frissítené 10:00 órakor alapvető tooPremium és reggel 1:35 frissítés befejezése a napot követő hello az 1:00 órakor kezdődő hello díjas díjakon 
* Ha Ön prémium tooBasic adatbázisának visszaminősítését 11:00 órakor 2:15 órakor befejeződik, majd hello adatbázis díjfizetéssel hello prémium díj 3:00 órakor, amíg elteltével hello alapvető ütemben számítjuk fel.

## <a name="how-does-elastic-pool-usage-show-up-on-my-bill-and-what-happens-when-i-change-edtus-per-pool"></a>Hogyan nem rugalmas készlet használati jelennek meg a számlázási és mi történik, amikor módosíthatók készletenként felhasználható edtu-k?
A rugalmas készlet díja megjelenítése fel a számlázási, rugalmas dtu-i (edtu-k) készletenként felhasználható edtu-k alatt látható hello lépésekben [árképzést ismertető oldalra hello](https://azure.microsoft.com/pricing/details/sql-database/). Nincs a rugalmas adatbázis-díjmentes. A készlet eDTU hello legmagasabb, függetlenül a használati vagy hello készlet volt-e aktív kisebb, mint egy óráig létezik óránként kell fizetni. 

Példák

* Ha egy Standard rugalmas készletet hoz létre a 200 edtu-k szerint 18, öt adatbázisok toohello alkalmazáskészlet hozzáadása van szó, a 200 edtu-k hello teljes órán keresztül, 11 órakor kezdődő keresztül hello fennmaradó hello nap.
* Naponta 2, reggel 5:05 adatbázis 1 50 edtu-k fel kezdődik, és állandó hello napon keresztül tárolja. Adatbázisok 2 – 5 ingadozik 0 és 80 edtu-k között. Hello nap alatt adja hozzá öt más adatbázisok használó különböző edtu-k hello nap folyamán. 2 nap terhelve, 200 eDTU teljes napi. 
* A nap 3: 00 órai egy másik 15 adatbázisok hozzáadása. Adatbázis-használat növeli a teljes hello nap toohello pont döntheti el, a 200 too400 hello készlet tooincrease edtu-k: 8:05-kor Díjak hello 200 eDTU szinten voltak érvényben csak 8 pm, és növeli a fennmaradó négyórás hello too400 edtu-k. 

## <a name="elastic-pool-billing-and-pricing-information"></a>A rugalmas készlet számlázási és díjszabási információkat
Rugalmas készletek óraalapú hello a következő jellemzőkkel:

* Rugalmas készlethez való létrehozás után lesz számlázva, akkor is, ha nincsenek adatbázisok hello készletben.
* Egy rugalmas készlet számlázása óránként történik. Ez az hello azonos mérési gyakoriság teljesítményszintet önálló adatbázisok esetében.
* Rugalmas készletek esetén új átméretezett tooa edtu-k, majd hello készlet száma nem számlázva toohello edtu-k új mennyisége szerint hello átméretezése művelet befejezéséig. Ez a következő hello azonos mintát önálló adatbázisok teljesítményszintjének hello változó állapotúként.
* rugalmas készletek ára hello hello készlet edtu-inak száma hello alapul. rugalmas készletek ára hello nem függ a hello számát és a rugalmas adatbázisok hello belül felhasználása.
* Árlista (a készlet edtu-inak száma) alapján számított x (eDTU / egységár).

eDTU egységár hello rugalmas készletek értéke magasabb, mint hello DTU egységárát hello egy önálló adatbázis ugyanazon a szolgáltatási rétegben. Részletes információ: [SQL Database – Díjszabás](https://azure.microsoft.com/pricing/details/sql-database/). 

toounderstand hello edtu-inak száma és a szolgáltatási rétegek, lásd: [SQL Database beállításai és teljesítménye](sql-database-service-tiers.md).

## <a name="how-does-hello-use-of-active-geo-replication-in-an-elastic-pool-show-up-on-my-bill"></a>Hogyan használja az aktív georeplikáció a rugalmas készlet megjelennek a számlán meg a hello?
Önálló adatbázisok használatával ellentétben [aktív georeplikáció](sql-database-geo-replication-overview.md) rugalmas adatbázisok nem közvetlen hatást számlázási.  Csak a hello edtu-k üzembe helyezve az egyes hello készletek (alkalmazáskészlet elsődleges és másodlagos készlet) van szó

## <a name="how-does-hello-use-of-hello-auditing-feature-impact-my-bill"></a>Hogyan hello használ a naplózási szolgáltatás hatás hello a számlázási?
Naplózás épül hello SQL Database szolgáltatáshoz a(z) nincs további költségeket, és elérhető tooBasic, Standard, Premium és prémium RS adatbázisok. Azonban toostore hello auditnaplókat, hello naplózása szolgáltatás által használt Azure Storage-fiók és a táblák és az Azure Storage üzenetsorokat sebességet hello a napló mérete alapján.

## <a name="how-do-i-find-hello-right-service-tier-and-performance-level-for-single-databases-and-elastic-pools"></a>Hogyan találhatom hello jobb és teljesítményszintet szolgáltatásszint önálló adatbázisok és rugalmas készletek?
Néhány eszközök elérhető tooyou van. 

* A helyszíni adatbázisokhoz, használja a hello [DTU méretezési advisor](http://dtucalculator.azurewebsites.net/) toorecommend hello adatbázisok és a dtu-k szükséges, és értékelje ki a rugalmas adatbáziskészletek.
* Ha egy önálló adatbázis volna rendelkezésre áll a készletbe, Azure intelligens motor rugalmas készletek javasolja, ha azt látja, hogy egy korábbi használati mintát, amely szükségessé teszi. Lásd: [figyelése és kezelése az Azure-portálon hello rugalmas készletek](sql-database-elastic-pool-manage-portal.md). Hogyan toodo hello matematikai saját kezűleg kapcsolatos részletekért lásd: [rugalmas készletek ára és teljesítménye szempontjai](sql-database-elastic-pool.md)
* toosee hogy toodial egy adatbázist kell felfelé vagy lefelé, lásd: [az önálló adatbázisok teljesítményének útmutatást](sql-database-performance-guidance.md).

## <a name="how-often-can-i-change-hello-service-tier-or-performance-level-of-a-single-database"></a>Milyen gyakran módosíthatom hello szolgáltatási szint vagy a teljesítmény egyetlen adatbázisra szint?
Módosíthatja a hello szolgáltatási réteg (között a Basic, Standard, Premium és prémium RS) vagy gyakran kívánt hello teljesítményszint szükséges egy szolgáltatási réteg (például S1 tooS2) belül. Korábbi adatbázisokhoz módosíthatja hello szolgáltatási szint vagy a teljesítmény szint összesen négy alkalommal egy 24 órás időszakban.

## <a name="how-often-can-i-adjust-hello-edtus-per-pool"></a>Milyen gyakran módosíthatja készletenként hello edtu-t?
Olyan gyakran ahányat csak szeretne.

## <a name="how-long-does-it-take-toochange-hello-service-tier-or-performance-level-of-a-single-database-or-move-a-database-in-and-out-of-an-elastic-pool"></a>Mennyi ideig nem azt toochange hello szolgáltatási szint vagy a teljesítmény szint egyetlen adatbázisra végrehajtása vagy a-adatbázis áthelyezése rugalmas készletek mindkét?
Adatbázis hello szolgáltatási szint módosítása és a bejövő és kimenő adatforgalma áthelyezése egy készlet szükséges hello adatbázis toobe hello platformon, mint a háttérművelet másolja. Változó hello szolgáltatásréteg hello hello adatbázisok méretétől függően néhány percet tooseveral órát vehet igénybe. Mindkét esetben hello adatbázisokat továbbra is online és elérhető hello áthelyezés során. Önálló adatbázisok módosításával kapcsolatos részletekért lásd: [módosítás hello szolgáltatási szint adatbázis](sql-database-service-tiers.md). 

## <a name="when-should-i-use-a-single-database-vs-elastic-databases"></a>Mikor kell használni egy önálló adatbázis és a rugalmas adatbázisok?
Általában rugalmas készletek készültek, a szokásos [szoftver,--szolgáltatás (SaaS) alkalmazásminta](sql-database-design-patterns-multi-tenancy-saas-applications.md), ahol van egy adatbázist használ az ügyfél vagy a bérlőt. Beszerzési önálló adatbázisok és a elhelyezésétől toomeet hello változót és a maximális követelheti meg az egyes adatbázisok gyakran nem költséghatékony. Az összevont kezelheti a hello készlet hello teljesítményéért, és a hello adatbázisok automatikusan méret felfelé és lefelé. Azure intelligens motor adatbázisok készlet azt javasolja, amikor a használati mód szükségessé teszi. További információkért lásd: [rugalmas készlet útmutatást](sql-database-elastic-pool.md).

## <a name="what-does-it-mean-toohave-up-too200-of-your-maximum-provisioned-database-storage-for-backup-storage"></a>Mit jelent a maximális kiosztott adatbázistár too200 %-át toohave biztonságimásolat-tároláshoz?
Biztonsági másolatok tárolásának az automatikus adatbázis biztonsági mentése, a használt társított hello tárhely [pontot-a--visszaállítás egy korábbi időpontra](sql-database-recovery-using-backups.md#point-in-time-restore) és [georedundáns helyreállítás](sql-database-recovery-using-backups.md#geo-restore). A Microsoft Azure SQL Database biztonsági másolatok tárolásának további költségek nélkül a maximális kiosztott adatbázis tárolási too200 % biztosít. Például ha egy szabványos DB példány kiosztott DB méretű 250 GB-os, rendelkezésre álló 500 GB-os biztonsági mentési tároló használatáért nem kell külön fizetni. Ha az adatbázis meghaladja a megadott biztonsági másolatok tárolásának hello, lépjen kapcsolatba az Azure támogatási tooreduce hello megőrzési időszak választhat, vagy hello extra biztonsági másolatok tárolásának számlázva standard írásvédett földrajzilag redundáns tárolás (RA-GRS) díj ellenében. RA-GRS számlázási további információkért lásd: Storage Díjszabásának részleteit.

## <a name="im-moving-from-webbusiness-toohello-new-service-tiers-what-do-i-need-tooknow"></a>I vagyok áthelyezést Web vagy Business toohello új szolgáltatási szinteket, mire van szükségem az tooknow?
Az Azure SQL Web és Business adatbázisokat most kivezettük. hello Basic, Standard, Premium, prémium szintű RS és rugalmas rétegek cserélje le a Web és Business adatbázisokat kivonása hello. 

## <a name="what-is-an-expected-replication-lag-when-geo-replicating-a-database-between-two-regions-within-hello-same-azure-geography"></a>Mi az egy várt replikációs késés, ha azonos földrajzi replikál egy adatbázis belül két régiók közötti hello Azure geográfiai?
Azt jelenleg támogatják az RPO öt másodpercben és hello replikációs késés lett kisebb, mint amikor hello földrajzi-másodlagos hello Azure ajánlott párosított régió van tárolva, és: hello azonos szolgáltatásréteget.

## <a name="what-is-an-expected-replication-lag-when-geo-secondary-is-created-in-hello-same-region-as-hello-primary-database"></a>Egy várt replikációs késés Újdonságok létrehozásakor földrajzi-másodlagos hello azonos hello elsődleges adatbáziséval megegyező régióban?
A tapasztalati adatai alapján, nincs túl sok intra-régiónként, illetve a régió közötti replikációs késés különbségének Azure ajánlott párosított régió hello használata esetén. 

## <a name="if-there-is-a-network-failure-between-two-regions-how-does-hello-retry-logic-work-when-geo-replication-is-set-up"></a>Ha két régiók közötti hálózati hiba, hello újrapróbálkozási logika működése georeplikáció beállításakor
Ha a kapcsolata megszakad, minden 10 másodperc toore próbálkozni-kapcsolatok létesítéséhez.

## <a name="what-can-i-do-tooguarantee-that-a-critical-change-on-hello-primary-database-is-replicated"></a>Mi a teendő, hogy a rendszer replikálja az elsődleges adatbázis hello kritikus módosítása tooguarantee?
hello földrajzi-másodlagos aszinkron replikájának, és azt nem tookeep elsődleges hello teljes szinkronban legyen. De a metódus tooforce szinkronizálási tooensure hello replikációt kritikus változások nyújtunk (például jelszó frissítések). Kényszerített szinkronizálási teljesítmény hatással van, mert a hívó szál hello blokkol, amíg a végrehajtott tranzakciók replikálódnak. További információkért lásd: [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx). 

## <a name="what-tools-are-available-toomonitor-hello-replication-lag-between-hello-primary-database-and-geo-secondary"></a>Mely eszközök elérhető toomonitor hello replikációs késés hello elsődleges és másodlagos földrajzi között?
Hello valós idejű replikációs késés hello elsődleges adatbázis és a földrajzi-másodlagos egy DMV keresztül elérhetővé kell tenni. További információkért lásd: [sys.dm_geo_replication_link_status](https://msdn.microsoft.com/library/mt575504.aspx).

## <a name="toomove-a-database-tooa-different-server-in-hello-same-subscription"></a>egy adatbázis tooa másik hello kiszolgálóján toomove ugyanahhoz az előfizetéshez
* A hello [Azure-portálon](https://portal.azure.com), kattintson a **SQL-adatbázisok**, hello listából válasszon ki egy adatbázist, és kattintson a **másolási**. Lásd: [Azure SQL-adatbázis másolása](sql-database-copy.md) további részletek.

## <a name="toomove-a-database-between-subscriptions"></a>az előfizetések közötti adatbázis toomove
* A hello [Azure-portálon](https://portal.azure.com), kattintson a **SQL Server-kiszolgálók** , és válassza a hello kiszolgáló, amelyen az adatbázis hello listából. Kattintson a **áthelyezése**, majd válassza ki a hello erőforrások toomove és hello előfizetés toomove való használni.
