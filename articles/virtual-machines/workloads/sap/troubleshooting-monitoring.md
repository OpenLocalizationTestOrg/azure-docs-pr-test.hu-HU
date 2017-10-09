---
title: "aaaTroubleshooting és a figyelés az SAP HANA (nagy példányok) Azure-on |} Microsoft Docs"
description: "Hibaelhárítás, és figyelje az SAP HANA egy Azure (nagy példány)."
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/01/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1f1cd35820e227fd99af495431cd4b826aa53600
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootroubleshoot-and-monitor-sap-hana-large-instances-on-azure"></a>Hogyan tootroubleshoot és a figyelő SAP HANA (nagy példány) az Azure-on


## <a name="monitoring-in-sap-hana-on-azure-large-instances"></a>Az SAP HANA Azure (nagy példányok) figyelése

Az Azure (nagy példányok) SAP HANA ugyanolyan helyzetet teremt, az egyéb IaaS-telepítésből – kell toomonitor milyen operációs rendszer hello és hello alkalmazás ezzel, és hogyan ezeket felhasználhatják a következő erőforrások hello:

- CPU
- Memory (Memória)
- Hálózati sávszélesség
- Lemezterület

Azure virtuális gépekkel van lehetősége, hogy hello erőforrás osztályok fent megnevezett szoftverre-e elegendő, és hogy ezek beolvasása elfogytak toofigure. Íme további információkhoz juthat az egyes hello különböző osztályú:

**Processzor-erőforrás-felhasználás:** hello arány SAP HANA elleni bizonyos munkaterhelések meghatározott érvényben levő toomake meg arról, hogy van elég CPU erőforrások elérhető toowork hello a memóriában tárolt adatok keresztül kell lennie. Mindazonáltal néhány esetben előfordulhat HANA igényel, ahol nagy mennyiségű Processzor toomissing indexek vagy hasonló problémák miatt a lekérdezések végrehajtása. Ez azt jelenti, célszerű figyelemmel kísérni hello HANA nagy példány egység, valamint a Processzor-erőforrások hello adott HANA szolgáltatások által használt CPU hálózatierőforrás-fogyasztás.

**Memória-felhasználás:** fontos toomonitor a HANA belül, valamint HANA kívül hello egységen van. Belül HANA hogyan hello adatok nem használ-e rendelés toostay belül szükséges SAP útmutatást méretezése hello memóriája kiosztott HANA figyelése. Szeretné a hello nagy példány szintű toomake, hogy további telepített nem-HANA szoftver nem túl sok memóriát használ, és ezért versenyeznek HANA-memória memória-felhasználás toomonitor.

**Hálózati sávszélességet:** hello Azure VNet átjáró korlátozott sávszélesség az adatok áthelyezése hello Azure virtuális hálózat, ezért hasznos lehet az összes fogadott toomonitor hello adatok hello Azure virtuális gépek a virtuális hálózat toofigure ki, hogy milyen közel a hello Azure toohello határain belül a kiválasztott SKU átjáró. Hello HANA nagy példány egység akkor győződjön meg logika toomonitor bejövő és kimenő hálózati forgalmat is, és adott idő alatt végrehajtott hello kötetek tookeep nyomon.

**Szabad lemezterület:** lemezterület-felhasználást általában növekszik adott idő alatt. Ennek számos oka lehet, de a legtöbb összes: adatmennyiség növekszik, a tranzakciónapló biztonsági mentései, nyomkövetési fájlokat tárolja, és a storage-pillanatfelvételekkel végrehajtása végrehajtását. Ezért fontos toomonitor lemezterület-használat és hello HANA nagy példány egységhez társított hello lemezterület felügyeletét.

## <a name="monitoring-and-troubleshooting-from-hana-side"></a>Figyelés és hibaelhárítás céljából HANA oldaláról

A sorrend tooeffectively elemzése a problémák kapcsolódó tooSAP HANA Azure (nagy példányokat), a hasznos toonarrow le hello okozza a problémát. SAP közzétette dokumentáció toohelp nagy mennyiségű meg.

Vonatkozó gyakran ismételt kérdéseket kapcsolódó tooSAP HANA teljesítmény található SAP megjegyzések következő hello:

- [SAP Megjegyzés #2222200 – gyakran ismételt kérdések: SAP HANA-hálózat](https://launchpad.support.sap.com/#/notes/2222200)
- [SAP Megjegyzés #2100040 – gyakran ismételt kérdések: SAP HANA Processzor](https://launchpad.support.sap.com/#/notes/0002100040)
- [SAP Megjegyzés #199997 – gyakran ismételt kérdések: SAP HANA memória](https://launchpad.support.sap.com/#/notes/2177064)
- [SAP Megjegyzés #200000 – gyakran ismételt kérdések: SAP HANA teljesítmény optimalizálása](https://launchpad.support.sap.com/#/notes/2000000)
- [SAP Megjegyzés #199930 – gyakran ismételt kérdések: SAP HANA i/o-elemzés](https://launchpad.support.sap.com/#/notes/1999930)
- [SAP Megjegyzés #2177064 – gyakran ismételt kérdések: SAP HANA-szolgáltatás újraindítása és összeomlik](https://launchpad.support.sap.com/#/notes/2177064)

**SAP HANA riasztások**

Első lépésként ellenőrizze a hello aktuális SAP HANA riasztások naplókat. Az SAP HANA Studio eszközben lépjen túl**felügyeleti konzol: riasztások: megjelenítése: az összes riasztás**. Ezen a lapon adott értékekre (szabad fizikai memória, a processzorkihasználtság stb.) hello beállítása a minimális és maximális küszöbértékek kívül eső összes SAP HANA riasztások jelennek meg. Alapértelmezés szerint legyenek ellenőrzések automatikus frissítése 15 percenként.

![Az SAP HANA Studióban, lépjen a konzol tooAdministration: riasztások: megjelenítése: az összes riasztás](./media/troubleshooting-monitoring/image1-show-alerts.png)

**PROCESSZOR**

Tooimproper küszöbérték beállítása miatt kiváltott riasztást a felbontása esetén tooreset toohello alapértelmezett érték vagy egyéb ésszerű küszöbértéket.

![Új toohello alapértelmezett értékét, vagy egyéb ésszerű küszöbértéket](./media/troubleshooting-monitoring/image2-cpu-utilization.png)

a következő figyelmeztetések hello jelezheti CPU erőforrás problémák:

- Gazdagép CPU-használat (riasztás 5)
- Legutóbbi mentésipont-művelet (riasztás 28)
- Mentési pont időtartama (riasztás 54)

Azt tapasztalhatja, hogy magas CPU-felhasználás a SAP HANA-adatbázisból származó hello következő meg:

- Riasztási 5 (gazdagép CPU-használat) jelenik meg, az aktuális és korábbi CPU-használat
- hello CPU-használat a hello áttekintés képernyő jelenik meg.

![CPU-használat megjelenő hello áttekintés képernyő](./media/troubleshooting-monitoring/image3-cpu-usage.png)

hello terhelés graph magas CPU-felhasználás, vagy a fogyasztás magas lehet, hogy megjelenítése a múltbeli hello:

![hello terhelés graph előfordulhat, hogy megjelenítése magas CPU-felhasználás, vagy magas fogyasztás hello elmúlt](./media/troubleshooting-monitoring/image4-load-graph.png)

Riasztás toohigh Processzor kihasználtsága miatt indított több oka is, de nem kizárólagosan okozhatja: bizonyos tranzakciókat, az adatok betöltése, a feladatok sokáig futnak az SQL-utasításokat vagy hibás lekérdezések teljesítményét (például a BW HANA a függő végrehajtása a kockák).

Tekintse meg a toohello [SAP HANA-hibaelhárítás: CPU kapcsolódó okoz, és a megoldások](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4f/bc915462db406aa2fe92b708b95189/content.htm?frameset=/en/db/6ca50424714af8b370960c04ce667b/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=46&amp;show_children=false) helyen részletes hibaelhárítási lépéseket.

**Operációs rendszer**

Az egyik legfontosabb hello ellenőrzi, hogy az SAP HANA Linux toomake meg arról, hogy a transzparens túl nagy lapok le vannak tiltva, lásd: [SAP Megjegyzés #2131662 – átlátszó túl nagy lapok (THP) SAP HANA-kiszolgálókon](https://launchpad.support.sap.com/#/notes/2131662).

- Ellenőrizheti, hogy engedélyezve vannak-e átlátszó túl nagy lapok hello Linux parancs a következő használatával: **/sys/kernel/mm/transparent macskaeledel\_hugepage/engedélyezve**
- Ha _mindig_ kapcsos zárójelek között, az alábbi, az azt jelenti, hogy engedélyezve vannak-e a hello átlátszó túl nagy lapok: [mindig] madvise soha nem; Ha _soha nem_ szimpla zárójelek között, az alábbi, az azt jelenti, hogy hello átlátszó Túl nagy lapok le vannak tiltva: mindig madvise [soha nem]

Linux parancs a következő hello semmi sem kell visszaadnia: **rpm - qa |} grep ulimit.** Ha úgy tűnik, _ulimit_ van telepítve, távolítsa el azonnal.

**Memória**

Előfordulhat, hogy figyelje meg, hogy hello összeg hello SAP HANA által lefoglalt memória adatbázisa a vártnál nagyobb. a következő figyelmeztetések hello magas memóriahasználatban kapcsolatos hibákat jelzik:

- Állomás fizikai memória használata (riasztás 1.)
- A névkiszolgáló (riasztás 12) memóriahasználata
- Teljes memóriahasználatát oszlop tárolási táblák (riasztás 40)
- Memóriahasználat (riasztás 43) szolgáltatások
- Memóriahasználat fő tárolási oszlop tárolási táblák (riasztás 45)
- Futásidejű memóriaképek (riasztás 46)

Tekintse meg a toohello [SAP HANA-hibaelhárítás: memóriával kapcsolatos problémák](http://help.sap.com/saphelp_hanaplatform/helpdata/en/db/6ca50424714af8b370960c04ce667b/content.htm?frameset=/en/59/5eaa513dde43758b51378ab3315ebb/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=26&amp;show_children=false) helyen részletes hibaelhárítási lépéseket.

**Hálózat**

Tekintse meg a túl[SAP Megjegyzés #2081065 – hibaelhárítás SAP HANA hálózati](https://launchpad.support.sap.com/#/notes/2081065) , és végezze el a hello hálózati hibaelhárítási lépések az SAP Megjegyzés vehető fel.

1. Kiszolgáló és az ügyfél közötti üzenetváltások idő elemzése.
  A. Hello SQL-parancsfájl futtatása [ _HANA\_hálózati\_ügyfelek_](https://launchpad.support.sap.com/#/notes/1969700)_._
  
2. Elemezze a fürtök csomóponton belüli kommunikációjához kommunikációt.
  A. SQL-parancsfájl futtatása [ _HANA\_hálózati\_szolgáltatások_](https://launchpad.support.sap.com/#/notes/1969700)_._

3. Linux parancs **ifconfig** (hello kimeneti látható, ha a csomag veszteségeiért is megjelenhetnek).
4. Linux parancs **tcpdump parancsot**.

Használja továbbá hello nyílt forráskódú [IPERF](https://iperf.fr/) eszköz (vagy hasonlót) toomeasure alkalmazás valós hálózati teljesítmény.

Tekintse meg a toohello [SAP HANA-hibaelhárítás: hálózati teljesítmény és a kapcsolódási problémái vannak](http://help.sap.com/saphelp_hanaplatform/helpdata/en/a3/ccdff1aedc4720acb24ed8826938b6/content.htm?frameset=/en/dc/6ff98fa36541e997e4c719a632cbd8/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=142&amp;show_children=false) helyen részletes hibaelhárítási lépéseket.

**Tárolás**

Végfelhasználói szempontból egy alkalmazás (vagy hello rendszer egészének) nehézkesen futtatja, nem válaszol vagy is még úgy tűnik, toohang i/o-teljesítménnyel kapcsolatos problémák esetén. A hello **kötetek** lapon a SAP HANA Studio, hogy hello csatolt kötetek, és milyen kötetek minden szolgáltatás által használt.

![A SAP HANA Studio hello kötetek lapján látható hello csatolt kötetek, és milyen kötetek minden szolgáltatás által használt](./media/troubleshooting-monitoring/image5-volumes-tab-a.png)

Csatlakoztatott kötetek részleteit láthatja hello képernyő alsó részén hello hello kötetek, fájlok és az i/o-statisztikák például.

![Csatlakoztatott kötetek részleteit láthatja hello képernyő alsó részén hello hello kötetek, például fájlok és az i/o-statisztikák](./media/troubleshooting-monitoring/image6-volumes-tab-b.png)

Tekintse meg a toohello [SAP HANA-hibaelhárítás: i/o kapcsolódó okait és megoldásait](http://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/6ff98fa36541e997e4c719a632cbd8/content.htm?frameset=/en/47/4cb08a715c42fe9f7cc5efdc599959/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=55&amp;show_children=false) és [SAP HANA-hibaelhárítás: lemez kapcsolódó okait és megoldásait](http://help.sap.com/saphelp_hanaplatform/helpdata/en/47/4cb08a715c42fe9f7cc5efdc599959/content.htm?frameset=/en/44/3e1db4f73d42da859008df4f69e37a/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=53&amp;show_children=false) helyen részletes hibaelhárítási lépéseket.

**Diagnosztikai eszközök**

Hajtsa végre egy SAP HANA állapot-ellenőrzéssel keresztül HANA\_konfigurációs\_Minichecks. Ez az eszköz, amely kell már merült fel az SAP HANA Studio riasztásként elvégzésével kritikus technikai problémák adja vissza.

Tekintse meg a túl[SAP Megjegyzés #1969700 – SQL utasítás gyűjtemény SAP Hana](https://launchpad.support.sap.com/#/notes/1969700) , és töltse le a hello SQL Statements.zip fájl csatolt toothat megjegyzés. A .zip fájlt hello helyi merevlemezen tárolja.

Az SAP HANA Studio hello **Rendszerinformáció** lapra, kattintson a jobb gombbal a hello **neve** oszlop, és válassza ki **importálási SQL-utasítások**.

![Az SAP HANA Studio, a hello Rendszerinformáció lapon kattintson a jobb gombbal a hello neve oszlopban, és válassza ki a importálási SQL-utasítások](./media/troubleshooting-monitoring/image7-import-statements-a.png)

SELECT hello SQL Statements.zip fájlt helyileg tárolja, és importálja a megfelelő SQL-utasítások hello egy mappa. Ezen a ponton hello számos különböző diagnosztikai ellenőrzések az alábbi SQL-utasítások is futtatható.

Például tootest SAP HANA rendszer replikáció sávszélesség-követelményekkel, kattintson a jobb gombbal hello **sávszélesség** utasítás alapján **replikációs: sávszélesség** válassza ki **nyitott** SQL-konzolon.

hello teljes SQL utasítás megnyitja lehetővé tevő bemeneti paraméterek (módosítása szakaszát) toobe megváltozott, és végrehajthatók.

![hello teljes SQL-utasítás lehetővé tevő bemeneti paraméterek (módosítása szakaszát) megváltozott, és végrehajthatók toobe megnyitása](./media/troubleshooting-monitoring/image8-import-statements-b.png)

Egy másik példa: hello utasítások alapján a jobb gombbal kattint **replikációs: áttekintés**. Válassza ki **Execute** hello helyi menüben:

![Jobb gombbal kattint, a replikációs csoportban hello utasítások egy másik példa: áttekintése. Válassza ki a végrehajtás hello helyi menüből](./media/troubleshooting-monitoring/image9-import-statements-c.png)

Ennek eredményeként a hibaelhárítási információkat:

![Ennek eredményeképpen az adatokat, amelyek segítenek a hibaelhárításban](./media/troubleshooting-monitoring/image10-import-statements-d.png)

Hello azonos Hana\_konfigurációs\_Minichecks és az összes ellenőrzés _X_ jeleket a hello _C_ (kritikus) oszlopban.

A minta kimenete:

**HANA\_konfigurációs\_MiniChecks\_Rev102.01 + 1** az általános SAP HANA ellenőrzi.

![HANA\_konfigurációs\_MiniChecks\_Rev102.01 + 1. a általános SAP HANA-ellenőrzése](./media/troubleshooting-monitoring/image11-configuration-minichecks.png)

**HANA\_szolgáltatások\_áttekintése** mi SAP HANA futó szolgáltatások áttekintését.

![HANA\_szolgáltatások\_megtudhatja, mi SAP HANA futó szolgáltatások – áttekintés](./media/troubleshooting-monitoring/image12-services-overview.png)

**HANA\_szolgáltatások\_statisztika** SAP Hana szolgáltatás információkat (Processzor, memória, stb.).

![HANA\_szolgáltatások\_SAP Hana statisztika szolgáltatás információkat ](./media/troubleshooting-monitoring/image13-services-statistics.png)

**HANA\_konfigurációs\_áttekintése\_Rev110 +** hello SAP HANA-példány általános tájékoztatást.

![HANA\_konfigurációs\_áttekintése\_Rev110 + hello SAP HANA-példányon általános információk](./media/troubleshooting-monitoring/image14-configuration-overview.png)

**HANA\_konfigurációs\_paraméterek\_Rev70 +** toocheck SAP HANA-paraméterekhez.

![HANA\_konfigurációs\_paraméterek\_Rev70 + toocheck SAP HANA-paraméterek](./media/troubleshooting-monitoring/image15-configuration-parameters.png)

