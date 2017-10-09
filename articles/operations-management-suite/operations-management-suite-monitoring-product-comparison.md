---
title: "termék összehasonlítás figyelési aaaMicrosoft |} Microsoft Docs"
description: "A Microsoft Operations Management Suite (OMS), a Microsoft felhő alapú informatikai felügyeleti megoldás, amely segít a kezelése és védelme a helyszíni és felhőalapú infrastruktúra.  Ez a cikk hello különböző szolgáltatásait az OMS azonosítja, és hivatkozások tootheir részletes tartalmat."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: a63ca0ad-61f8-425d-a48c-d87ba518c104
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/27/2016
ms.author: bwren
ms.openlocfilehash: 61144a298fe73c35181070d552c41b96fc445097
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-monitoring-product-comparison"></a>A Microsoft figyelési termék összehasonlítása
Ez a cikk ismerteti a System Center Operations Manager (SCOM) és az Operations Management Suite (OMS) szolgáltatáshoz összehasonlítása az architektúra, hello logikája hogyan azok megfigyelje az erőforrásokat, és hogyan működnek hello adatelemzési azok gyűjtése.  Ez a toogive, egy alapvető ismeretekkel rendelkezik a relatív szintjeiről és különbségeket.  

## <a name="basic-architecture"></a>Alapszintű architektúrája
### <a name="system-center-operations-manager"></a>System Center Operations Manager
Minden SCOM ezeket az összetevőket az Adatközpont.  [Van telepített ügynök](http://technet.microsoft.com/library/hh551142.aspx) SCOM által kezelt Windows és Linux rendszerű gépekre.  Ügynökök csatlakozás túl[felügyeleti kiszolgálók](https://technet.microsoft.com/library/hh301922.aspx) amely kommunikálni hello SCOM adatbázisról és az adatraktárról.  Ügynökök tartomány hitelesítési tooconnect toomanagement kiszolgálók támaszkodnak.  Azokat a megbízható tartományon kívüli tanúsítvány hitelesítést végezni, vagy tooa csatlakozás [átjárókiszolgáló](https://technet.microsoft.com/library/hh212823.aspx).

SCOM két SQL-adatbázis, a működési adatok egyet, majd egy másik data warehouse toosupport jelentéskészítésre és adatelemzésre igényel.  A [jelentéskészítő kiszolgáló](https://technet.microsoft.com/library/hh298611.aspx) fut az SQL Reporting Services tooreport adatok hello adatraktárból. 

SCOM figyelheti a felhőalapú erőforrásokat, például a felügyeleti csomagok használatát termékek [Azure](https://www.microsoft.com/download/details.aspx?id=38414), [Office 365](https://www.microsoft.com/download/details.aspx?id=43708), és [AWS](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/AWSManagementPack.html).  A felügyeleti csomagok használata egy vagy több helyi ügynököt, proxyként felderítéséhez felhőalapú erőforrások és a munkafolyamatok toomeasure fut, a teljesítmény és rendelkezésre állás.  Proxy ügynökök is túl[hálózati eszközök figyelése](https://technet.microsoft.com/library/hh212935.aspx) és az egyéb külső erőforrásokhoz.

hello operatív konzol egy alkalmazás, amely a Windows hello felügyeleti kiszolgálók tooone csatlakozik, és lehetővé teszi, hogy a rendszergazda tooview hello és összegyűjtött adatok elemzése és hello SCOM-környezet konfigurálása.  Egy webalapú konzol minden olyan IIS-kiszolgálón az alábbiakon tárolható, és böngészőn keresztül adatelemzés biztosít.

![SCOM-architektúra](media/operations-management-suite-monitoring-product-comparison/scom-architecture.png)

### <a name="log-analytics"></a>Log Analytics
Legtöbb OMS-összetevők telepítése és kezelése minimális költségek és erőfeszítések felügyeleti hello Azure felhőben szerepelnek.  A Naplóelemzési által gyűjtött összes adat hello OMS-tárház tárolja.

A Naplóelemzési három forrásokból származó gyűjthet adatokat:

* Fizikai és virtuális gépek futnak a Windows és hello [Microsoft Monitoring Agent (MMA)](https://technet.microsoft.com/library/mt484108.aspx) vagy Linux- és hello [Operations Management Suite-ügynök Linux](https://technet.microsoft.com/library/mt622052.aspx).  Ezek a gépek lehetnek a helyszíni vagy a virtuális gépek az Azure rendszerben vagy egy másik felhőben.
* Az Azure Storage-fiók [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) Azure feldolgozói szerepkörét, a webes szerepkör vagy a virtuális gép által összegyűjtött adatok.
* [Kapcsolat tooa SCOM felügyeleti csoport](https://technet.microsoft.com/library/mt484104.aspx).  Ebben a konfigurációban hello ügynökök kommunikálni az SCOM felügyeleti kiszolgálóhoz, amely fájlmegosztásba hello adatok toohello SCOM-adatbázis, ahol majd biztosítását toohello OMS-adattár.
  A rendszergazdák összegyűjtött adatok elemzése és Naplóelemzési konfigurálása az Azure szolgáltatásban üzemeltetett, és minden böngészőből elérhető hello OMS-portálon.  Mobilalkalmazások tooaccess ezek az adatok érhetők el hello szabványos platformokon.

![Napló Analytics architektúrája](media/operations-management-suite-monitoring-product-comparison/log-analytics-architecture.png)

### <a name="integrating-scom-and-log-analytics"></a>SCOM és Naplóelemzési integrációja
Ha SCOM adatforrásaként használja a Naplóelemzési kihasználható hello a két terméknek egy hibrid környezet figyelését.  Konfigurálja a meglévő SCOM ügynökök keresztül hello operatív konzol toobe OMS kezeli, továbbá toocontinuing toorun felügyeleti csomagokat az SCOM.  
Egy csatlakoztatott SCOM felügyeleti csoport adatait a rendszer tooLog Analytics négy módszerek egyikének használatával:

* Események és teljesítményadatok hello ügynök által gyűjtött és tooSCOM kézbesíteni.  Felügyeleti kiszolgálók scom hello adatok tooLog Analytics adja át.
* Egyes események, például az IIS-napló és a biztonsági események továbbra is toobe kézbesíteni közvetlenül tooLog Analytics hello ügynöktől.
* Néhány megoldás biztosításához a további szoftverfrissítési toohello ügynök, vagy a követelmény, hogy szoftvert telepített toocollect további adatokat.  Ezeket az adatokat a rendszer általában küld közvetlenül tooLog elemzés.
* Néhány megoldás közvetlenül az SCOM felügyeleti kiszolgálókat, amelyek nem származik hello ügynök gyűjtenek adatokat.  Például hello [Riasztáskezelési megoldás](https://technet.microsoft.com/library/mt484092.aspx) riasztásokat gyűjtő az SCOM való létrehozás után.

## <a name="monitoring-logic"></a>Figyelési logika
SCOM és Naplóelemzési hasonló, az ügynökök gyűjtött adatok, de hogyan azok határozza meg, és valósítja meg az adatgyűjtésre a programot, és hogyan azok adatelemzés hello általuk összegyűjtött alapvető eltérések vannak.

### <a name="operations-manager"></a>Operations Manager
Figyelési logika SCOM meg van valósítva a [felügyeleti csomagok](https://technet.microsoft.com/library/hh457558.aspx) tartalmazó logikát összetevők toomonitor, ezek az összetevők állapotának hello mérési felderítéséhez és az adatok tooanalyze gyűjtéséhez.  Figyelési adatok más dolga, mint az esemény- vagy teljesítményszámláló gyűjtése, vagy azt egy parancsfájl megvalósított komplex logikai használhat.  Felügyeleti csomagok, amelyek tartalmaznak teljes figyelési érhetők el a különböző [Microsoft és harmadik féltől származó alkalmazások](http://go.microsoft.com/fwlink/?LinkId=82105) hozzáadása toohardware és a hálózati eszközökön.  Is [saját felügyeleti csomagok szerzői](http://aka.ms/mpauthor) egyéni alkalmazások.

Felügyeleti csomagok tartalmazhatnak több munkafolyamatot, hogy minden egyes például teljesítményszámláló mintavételi, hello szolgáltatás állapotának ellenőrzése vagy parancsprogram futtatása különböző figyelési funkciót hajt végre.  Minden munkafolyamat függetlenül fut, és határozza meg a saját eredményeket, például az adatbázis azt írni fogja a tooand e riasztást hoz létre. 

Ha szeretné felülbírálni az például hello gyakoriság futnak, ahol ítélik hiba hello küszöbérték munkafolyamat részletes adatait, és akkor generál hello riasztás hello súlyossága.  További funkciók is megadhatja a saját munkafolyamatokat hozzáadásával.

![Felülbírálások](media/operations-management-suite-monitoring-product-comparison/scom-overrides.png)

Felügyeleti csomagok hello Operations Manager-adatbázis telepítve van, és automatikusan elosztott tooagents kezelési kiszolgálók használatával.  Minden ügynök automatikusan töltse le a felügyeleti csomagok és munkafolyamatok megfelelő toohello alkalmazásokat telepített töltenek.  Hello ügynök által gyűjtött adatok beszúrásához hátsó toohello felügyeleti kiszolgáló kerülnek az hello SCOM adatbázis és a data warehouse-bA.  hello operatív konzol lehetővé teszi a tooview, és egyéni nézetek, irányítópultokat és jelentéseket hello felügyeleti csomag tartalmazza az adatok elemzéséhez.

a felügyeleti csomagok hello terjesztési mutatja be a következő diagram hello.

![Felügyeleti csomag folyamata](media/operations-management-suite-monitoring-product-comparison/scom-mpflow.png)

### <a name="log-analytics"></a>Log Analytics
#### <a name="event-and-performance-collection"></a>Esemény- és teljesítménynaplózás
A Naplóelemzési gyűjti össze az események és teljesítményszámlálók ügynök rendszerekről adatforrások, például Windows-Eseménynapló, az IIS-napló és a Syslog.  Feltételek, amelynek adatok hello Naplóelemzés portálján keresztül gyűjtött és majd hozza létre a naplófájl lekérdezések tooanalyze hello gyűjtött adatokat adhat meg.  Szabványos feltételek csoportja van definiálva az OMS-munkaterület létrehozásakor, és bizonyos alkalmazásokhoz további adatokat adhat meg. 

![A Naplóelemzési Eseménynapló meghatározása](media/operations-management-suite-monitoring-product-comparison/log-analytics-definedata.png)

Míg SCOM számos részletes-munkafolyamatok, amelyek általában a meghatározott feltételek adatok megadása és hello művelet, amely válaszként, Log Analyticshez rendelkezik adatgyűjtés az általános feltételek.  Napló lekérdezések és a megoldások biztosít több célként megadott feltételek elemzése és adott adatok hello felhőben működő után már.

#### <a name="solutions"></a>Megoldások
Megoldás további logikát biztosítja az adatok gyűjtése és elemzése.  Megoldások tooadd tooyour OMS előfizetés választhat hello megoldás gyűjteménye.

![Megoldások gyűjteménye](media/operations-management-suite-monitoring-product-comparison/log-analytics-solutiongallery.png)

Megoldás elsősorban az események és teljesítményszámlálók hello OMS-tárházban gyűjtött adatelemzési lehetőségeket biztosít hello felhőben futtatható.  Akkor is határozhatnak meg további adatok toobe gyűjteni, amelyek a napló lekérdezésekkel vagy hello OMS irányítópult hello megoldás által biztosított további felhasználói felület elemzése. 

Például hello [változáskövetési megoldás](https://technet.microsoft.com/library/mt484099.aspx) konfigurációs ügynök rendszereken módosításai, és írja az eseményeket toohello OMS elemzése a több grafikus nézetek, hogy összesítse a tárházból talált változást észlel.  Részletezhető le összegzett hello nézetből napló lekérdezések adott megjelenítési hello részletes hello megoldás által összegyűjtött adatokat.

Amíg kiválaszthatja, hogy milyen megoldások tooyour előfizetés hozzáadása, jelenleg nincs hello képességét toocreate saját megoldásokat.  Válassza ki a hello események és a teljesítmény számlálók toocollect, és a saját napló lekérdezések alapuló egyéni nézetek létrehozása.

figyelési logika Naplóelemzési hello a következő diagram hello foglalja össze.

![Napló elemzési megoldások folyamata](media/operations-management-suite-monitoring-product-comparison/log-analytics-solution-flow.png)

## <a name="health-monitoring"></a>Állapotfigyelés
### <a name="operations-manager"></a>Operations Manager
SCOM modell hello különböző összetevőket az alkalmazások, és adjon meg egy valós idejű állapotát az egyes.  Ez lehetővé teszi, akkor toonot csak nézet észlelt hibák és teljesítményét az idő múlásával, de a is toovalidate hello egy alkalmazás vagy a rendszer aktuális állapotát, és annak összetevői egy adott időpontban.  Hello időszakokat, amelyek egy adott alkalmazás támogatja, mert a hello állapotfigyelő motor scom is támogatja a szolgáltatási szint szerződés (SLA) elemzése és a jelentés az adott idő alatt alkalmazás rendelkezésre állásának hello.

Az alábbi hello nézet például hello valós idejű állapotát az SQL-adatbázis motorok SCOM által figyelt jeleníti meg.  hello állapotát egy hello adatbázis motorok hello-adatbázisok mindegyike esetében jelenik meg a hello alsó felére hello nézet.

![Állapot nézet](media/operations-management-suite-monitoring-product-comparison/scom-state-view.png)

egy adatbázis-kezelők hello állapotkezelő hello alább használt toodetermine hello figyelőt az általános állapota.  Ezek a figyelők hello SQL felügyeleti csomagban definiált, és minden SQL adatbázis motor SCOM által felderített futtatni.

![Az állapotkezelő](media/operations-management-suite-monitoring-product-comparison/scom-health-explorer.png)

Több rendszer-összetevőket kombinált toomeasure hello állapotát egy elosztott alkalmazás lehet.  Ez különösen hasznos, ha több elosztott összetevőket tartalmazó üzleti alkalmazások lehet.  A rendelkezésre állási hello alkalmazás modell, amely minden egyes összetevő hello állapotát méri, hogy az összesítő hozhat létre.

Az Active Directory több felügyeleti csomagot, amely egy modell tooanalyze biztosít az elosztott összetevőinek példája.  hello minta diagram alább látható hello állapotának hello általános környezet és hello kapcsolat erdők, tartományok és a tartományvezérlők között.  Ezeket az összetevőket tartalmaz alösszetevők, és több monitor hasonló toohello SQL fenti példa.

![SCOM-diagram nézet](media/operations-management-suite-monitoring-product-comparison/scom-diagram-view.png)

### <a name="log-analytics"></a>Log Analytics
OMS nem egy általános motor toomodel alkalmazásokat is, és mérheti a valós idejű állapotát.  Egyes megoldások felmérhetik hello gyűjtött adatok alapján adott szolgáltatások általános állapotát, és egyéni logika hello ügynök tooperform valós idejű elemzési telepítheti azokat.  Megoldások futtatja, az access toohello OMS-tárház hello felhőben, mert azok gyakran megadhatják mélyebb elemzés, mint a felügyeleti csomagok jellemzően történik. 

Például hello [AD értékelési és SQL-értékelési megoldás](https://technet.microsoft.com/library/mt484102.aspx) összegyűjtött adatok elemzése, és adja meg a besorolását hello környezet különböző szempontjairól.  Ez magában foglalja a javaslatok javításai, beállíthatja, hogy tooimprove hello rendelkezésre állásának és teljesítményének hello környezet.

![AD-értékelési megoldás](media/operations-management-suite-monitoring-product-comparison/log-analytics-ad-assessment.png)

## <a name="data-analysis"></a>Adatok elemzése
SCOM és a Naplóelemzési minden különböző szolgáltatások tooanalyze gyűjtött adatokat szolgáltat.  SCOM van nézeteket és irányítópultokat hello operatív konzol formátumok és jelentéseinek hello adatraktár táblázatos formában az adatokat a különböző adatok elemzéséhez.  A Naplóelemzési teljes napló lekérdezési nyelv és felületet biztosít a hello OMS-tárházban adatok elemzéséhez.  SCOM Naplóelemzési használja forrásként, ha hello tárház tartalmazza, hogy hello Naplóelemzési eszközök lehessen tooanalyze használt adatokat mindkét SCOM által összegyűjtött adatokat.

### <a name="operations-manager"></a>Operations Manager
#### <a name="views"></a>Nézetek
Nézetek az operatív konzol hello engedélyezése SCOM által gyűjtött, tooview különböző adattípusokkal különböző formátumokban, általában táblázatos események, riasztások, és állapotadatokat és vonaldiagramok teljesítményadatokhoz.  Nézetek minimális elemzést vagy hello adatok összevonása, de engedélyezi toofilter tooparticular feltételek szerint. 

![Nézetek](media/operations-management-suite-monitoring-product-comparison/scom-views.png)

Felügyeleti csomagok jellemzően hello alkalmazást vagy az általa figyelt system támogató több nézetet biztosít.  Ez az hello különböző objektumok, amely felderíti a felügyeleti csomag hello állapotnézeteit tartalmazza, az észlelt problémákat, és nézeteket teljesítmény számlálók riasztást.

Nézetek különösen alkalmasak elemzéséhez, beleértve a nyitott riasztások és a figyelt rendszerek és objektumok hello állapotát hello környezet hello aktuális állapotát.  Részletezve támogatása egy adott riasztás rendelés toodiagnose toodetailed esemény- vagy adatokat az alapvető ok. Hasonlóképpen megtekintheti hello teljesítmény- és állapot az alkalmazás tooassess különböző összetevőinek az aktuális állapotát.

#### <a name="dashboards"></a>Irányítópultok
Az operatív konzol elsősorban együttműködve hello irányítópultok hello ugyanazokat az adatokat, nézetek azonban több testre szabhatók, és részletesebb képi megjelenítések tartalmazhatnak.  Standard irányítópultok készlete érhetők el, hogy könnyen teste szabhatja a saját célokra.  Egy PowerShell-lekérdezés által visszaadott adatokat megjelenítő widgetet PowerShell is használható.

![Irányítópult](media/operations-management-suite-monitoring-product-comparison/scom-dashboard.png)

A fejlesztők rendelkezik hello képességét tooadd egyéni összetevők toodashboards tartoznak azok a felügyeleti csomagokban.  Ezek nagyon specializált tooa adott alkalmazásra, például az SQL management pack alább látható hello hello irányítópult lehet.  Ezt az irányítópultot is használható sablonként egyéni verzióihoz.

![SQL-irányítópult](media/operations-management-suite-monitoring-product-comparison/scom-sql-dashboard.png)

#### <a name="reports"></a>Jelentések
Jelentéseket a SCOM hello adatraktár táblázatos adatok elemzését.  Ugyanakkor nyomtatott és automatizált PDF, a fürt megosztott kötetei szolgáltatás és a Word különböző fájlformátumokban ütemezve.  Jelentések dolgoznak hello az adatraktár adataival különösen megfelelőek a hosszú távú folyamatok elemzése.

Felügyeleti csomagok egyéni jelentések általában biztosít egy adott alkalmazás.  A szalagtár általános jelentések testre szabható a saját alkalmazásai vagy alkalmi elemzések végrehajtását is választhat.

Az alábbiakban látható egy minta rendszerteljesítmény-jelentés hello Active Directory felügyeleti csomag által gyűjtött adatok jelennek meg.

![Jelentés](media/operations-management-suite-monitoring-product-comparison/scom-report.png)

### <a name="log-analytics"></a>Log Analytics
A Naplóelemzési rendelkezik egy [lekérdezési nyelv](https://technet.microsoft.com/library/mt484120.aspx) használható tooperform elemzés keresztül nélkül hello kell toocreate több alkalmazás adatait egy egyéni nézet vagy a jelentés.  OMS hello felhőben valósul meg, mert lekérdezések és adatelemzés teljesítményétől nincsenek tulajdonos tooany hardveres korlátokat, és gyorsan elemezheti a lekérdezések, beleértve a több millió rekordot. 

A Naplóelemzési lekérdezések akkor is egyéb funkciók hello alapját.  Lekérdezés mentése, az eredmények tooExcel exportálja, vagy automatikusan futtatása rendszeres időközönként, és az eredményeket az adott feltételeknek megfelelő riasztást hoz létre.  

![Napló lekérdezés folyamata](media/operations-management-suite-monitoring-product-comparison/log-analytics-query-flow.png)

Az alábbiakban van például Log Analytics-lekérdezés.  Ebben a példában a "started" hello nevében összes eseményt adott vissza, és esemény szerint csoportosítva azonosítóját.  hello felhasználó egyszerűen hello lekérdezés biztosít, és Naplóelemzési dinamikusan hoz létre a hello a felhasználói felület tooperform hello elemzése.  Bármely elem kiválasztásával hello listában visszaadható hello részletes eseményadatok.

![Napló-lekérdezés](media/operations-management-suite-monitoring-product-comparison/log-analytics-query.png)

Ezenkívül tooproviding alkalmi elemzés, Log Analyticshez lévő lekérdezések menthetők a jövőbeli használatra, és a hozzáadott tooyour [OMS irányítópult](http://technet.microsoft.com/library/mt484090.aspx) a hello a következő példában látható módon.

![OMS irányítópult](media/operations-management-suite-monitoring-product-comparison/log-analytics-dashboard.png)

## <a name="next-steps"></a>Következő lépések
* Telepítése [a System Center Operations Manager (SCOM)](https://technet.microsoft.com/library/hh205987.aspx).
* Regisztráljon [Naplóelemzési](https://azure.microsoft.com/documentation/services/log-analytics).  

