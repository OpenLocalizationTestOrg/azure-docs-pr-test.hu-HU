---
title: "aaaDetection képességek az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum segít az Azure Security Center az észlelési képességek működése toounderstand."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 4c5599cc-99a1-430f-895f-601615ef12a0
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: d8001cc2acdd0026bd9b3596bbdfec56f8874513
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-detection-capabilities"></a>Az Azure Security Center észlelési funkciói
Ez a dokumentum ismerteti az Azure Security Center speciális észlelési képességek, ami segít azonosítani a Microsoft Azure-erőforrások célzó aktív fenyegetéseket, és biztosít, amelyen hello kellett toorespond gyorsan.

> [!NOTE]
> Speciális észlelések hello Standard szint az Azure Security Center érhetők el. A 60 napos próbaverzió ingyenes. Frissíthet a hello hello árképzési szintjében kiválasztást [biztonsági házirend](security-center-policies.md). Látogasson el [Security Center lap](https://azure.microsoft.com/pricing/details/security-center/) árazással kapcsolatos további toolearn. 
> 
> 

## <a name="responding-tootodays-threats"></a>Válaszol tootoday tartozó fenyegetések
Jelentős változások történtek hello veszélyforrásainak tükrében megfigyelhető keresztül hello elmúlt 20 évben. Hello korábbi a vállalatok általában csak kellett tooworry kapcsolatos egyedi támadók, akik leginkább érdeklő megnéz "sikerült dolgukat" webhely felülírása. A mai támadók sokkal bonyolultabban és szervezettebben cselekszenek. Gyakran konkrét pénzügyi és stratégiai célokat követnek. További erőforrások elérhető toothem, ezenkívül, akkor előfordulhat, hogy támogathatók a számára állapotok vagy szervezett bűncselekménynek rendelkeznek.

Ez a megközelítés tooan egyedülálló szintű szakmai jelleg erősítése a hello támadó egyes holtversenyekben vezetett. Már nem a webhelyek megrongálása érdekli őket. Ezek érdeklődik ellophassák az adatokat, pénzügyi fiókjairól és más titkos adatokat – amelyek használhatnak most toogenerate pénzbeli hello piacon vagy tooleverage egy adott üzleti, politikai vagy katonai pozíciója. Még több vonatkozó mint ezeket a támadók számára a pénzügyi célkitűzéseit hello támadók megsértik a hálózatok toodo kárt tooinfrastructure és személyek számára.

Válasz, a szervezetek gyakran különböző pont megoldások, amelyek a hello vállalati peremhálózati vagy a végpontok védelme támadási aláírások megkeresésével központilag telepíteni. Ezek a megoldások általában toogenerate nagyszámú alacsony hűség riasztást, amelyek a biztonsági elemző tootriage igényelnek, és vizsgálja meg. A legtöbb szervezet nem rendelkezik hello idő és a szükséges szakértelem toorespond toothese riasztások – így sok szerint nyissa meg.  Eközben a támadók lett hatékonyabb a módszerek toosubvert sok aláírás-alapú védelmet és [toocloud környezetek igazítja](https://azure.microsoft.com/blog/detecting-threats-with-azure-security-center/). Új módszerek szükséges toomore gyorsan azonosíthatja a megjelenő fenyegetéseket, és gyorsítása felderítését és a válasz. 

## <a name="how-azure-security-center-detects-and-responds-toothreats"></a>Hogyan az Azure Security Center észleli, és kezeli a toothreats
A Microsoft biztonsági kutatói folyamatosan hello lookout fenyegetések esetén. Hozzáférés tooan kiterjedtnek beállítása a Microsoft globális jelenlét hello felhőben és helyszíni megosztjuk telemetriai adatot rendelkeznek. Ezen adatkészletek wide elérése és sokszínű gyűjteménye lehetővé teszi a Microsoft toodiscover új támadási mintákat és trendeket a helyszíni fogyasztói, valamint vállalati termékei, valamint az online szolgáltatások között. Ennek eredményeképpen a Security Center gyorsan tudja frissíteni az észlelési algoritmusait, igazodva a támadók újabb és egyre összetettebb biztonságirés-kihasználási megoldásaihoz. Ez a módszer segít lépést tartani a fenyegetések gyors ütemben növekvő körével. 

A Security Center fenyegetésészlelés működik, automatikusan az Azure-erőforrások, hello és hálózati összekapcsolt partnermegoldások a biztonsági információk begyűjtése. Ezt az információt, gyakran adatok az adatokat több forrásból tooidentify fenyegetések megvizsgálja. Biztonsági riasztások a Security Center kerülnek előrébb, valamint javaslatokat a hogyan tooremediate hello fenyegetést.

![A Security Center adatgyűjtése és adatábrázolása](./media/security-center-detection-capabilities/security-center-detection-capabilities-fig1.png)

A Security Center olyan fejlett biztonsági elemzéseket alkalmaz, amelyek messze túlmutatnak az aláírás-alapú megközelítéseken. A big Data típusú adatok eredményeket és [gépi tanulás](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) technológiák kihasználhatók tooevaluate események legyenek hello teljes felhőbeli háló – lenne lehetetlen tooidentify manuális módszer segítségével, és hello előrejelzésére szolgáló észlelése támadások alakulását. Ezek a biztonsági elemzések a következők: 

* **Integrált fenyegetésfelderítési adataival**: megkeresi az ismert ezeken a Microsoft termékeinek és szolgáltatásainak, globális fenyegetésfelderítési adataival, ami hello Microsoft Digital Crimes Unit (DCU), hello Microsoft biztonsági válasz Center (MSRC), és a külső hírcsatornák.
* **Viselkedéselemzés**: ismert mintákat toodiscover rosszindulatú viselkedést alkalmazza. 
* **Anomáliadetektálás**: statisztikai profilkészítési toobuild korábbi alapkonfigurációt használ. A létrehozott alapkonfigurációkat tooa potenciális támadási felület tartó eltéréseket riasztást küld.

### <a name="threat-intelligence"></a>Fenyegetésészlelési intelligencia
A Microsoft rendkívül nagy mennyiségű adattal rendelkezik a globális fenyegetésészlelési intelligencia keretein belül. Telemetria zajlik a több forrásból, például az Azure, Office 365, Microsoft CRM online, a Microsoft Dynamics AX, Outlook.com-os, MSN.com, hello Microsoft Digital Crimes Unit (DCU) és a Microsoft biztonsági válasz Center (MSRC). Kutatói is fenyegetés eszközintelligencia információkat kaphat, amelyek jelentős felhőszolgáltatók között megosztott és harmadik féltől származó toothreat eszközintelligencia hírcsatornák előfizet. Az Azure Security Center használhatja az információk tooalert ismert ezeken a toothreats meg. Néhány példa:

* **Kimenő kommunikáció tooa kártékony IP-cím**: kimenő forgalom tooa botnet vagy valószínűleg darknet ismert jelzi, hogy az erőforrás feltörték, és egy támadó azt tooexecute kísérlet a parancsokat, hogy rendszer vagy exfiltrate adatokon. Az Azure Security Center összehasonlítja a hálózati forgalom tooMicrosoft globális fenyegetésfelderítési adatbázis, és riasztást küld, ha azt észleli, hogy kommunikációs tooa kártékony IP-címet.

## <a name="behavioral-analytics"></a>Működés elemzése
Viselkedéselemzés a technika, amely elemzi, és összehasonlítja a tooa adatgyűjtés ismert minták. Ezek a minták azonban nem csak egyszerű aláírások. Azok a határoz meg, amelyek alkalmazott toomassive adatkészletek összetett gépi tanulási algoritmusok keresztül. Ezenkívül szakértő elemzők mélyrehatóan elemezték a kártékony működést a meghatározásukhoz. Az Azure Security Center viselkedéselemzés sérült tooidentify erőforrásokat a virtuális gép naplóinak, virtuális hálózati eszköznaplók, háló naplókat, összeomlási memóriaképek és egyéb forrásokból elemzése alapján használhatja. 

Emellett nincs korrelációban állnak más jelek toocheck bizonyíték a széles körű kampány támogatásához. A korrelációs konzisztensek az illetéktelen behatolásoknak meghatározott mutatók tooidentify események segítségével. Néhány példa:

* **Gyanús folyamatok végrehajtását**: támadók több technikák tooexecute kártevő szoftverek észlelése nélkül alkalmaz. Például egy támadó előfordulhat, hogy adjon kártevő hello jogos rendszerfájlok azonos néven, de ezeket a fájlokat egy másodlagos helyen helyezze el, használja a nagyon hasonló tooa jóindulatú fájl vagy maszk hello igaz fájlkiterjesztés. A Security Center modellek folyamatok működést és a figyelők feldolgozni végrehajtások toodetect kiugró ehhez hasonló helyzeteknek.  
* **Kártevők és kihasználásának kísérletek rejtett**: kifinomult kártevő szoftverek képesek tooevade hagyományos kártevőirtó termékek soha nem toodisk írása vagy szoftverösszetevőket lemezen tárolt titkosított.  Azonban ilyen kártevő észlelhető memória az elemzést követően használatával, hello kártevő kell hagyja a nyomkövetések rendelés toofunction memóriája. Ha szoftver összeomlik, összeomlási memóriaképet egy részéhez memória hello összeomlási hello helyreállításkor rögzíti.  Hello összeomlási memóriakép hello memóriája elemzésével, az Azure Security Center képesek észlelni technikák tooexploit biztonsági rések használt szoftver, bizalmas adatok eléréséhez, és rejtett megőrzésre az a fertőzött gép hello teljesítményére gyakorolt hatás nélkül a számítógépen.
* **Oldalirányú mozgás és a belső felderítés**: toopersist a sérült biztonságú hálózati, és keresse meg vagy betakarítás értékes adatok, a támadók gyakran kísérlik meg toomove könnyebben hello sérült a gép tooothers belül hello ugyanazon a hálózaton. A Security Center folyamat figyeli, és bejelentkezési tevékenységek sorrendje toodiscover kísérletek tooexpand egy támadó foothold hello hálózatokon, például távoli parancs végrehajtása hálózati probing, és a fiók enumerálása.
* **Rosszindulatú PowerShell-parancsfájlok**: PowerShell használatos támadók tooexecute kártékony kódokkal a cél virtuális gépek különböző célokra. A Security Center megvizsgálja a PowerShell tevékenységeit, hogy megtalálja a gyanús tevékenységek nyomait. 
* **Kimenő támadások**: a támadók gyakran cél hello segítségével ezen erőforrások toomount további támadások célja a felhőben található erőforrásokat. Feltört virtuális gépek, például lehet, hogy más virtuális gépek elleni használt toolaunch találgatásos támadások ellen, LEVÉLSZEMETET küld, vagy megnyitott portok vizsgálata, és egyéb eszközöket hello internet. Úgy, hogy alkalmazza a machine learning toonetwork forgalom, Security Center képes észlelni kimenő hálózati kommunikáció-nál nagyobb hello alapértelmezetté. Hello esetében a LEVÉLSZEMÉT, a Security Center is korrelációban szokatlan e-mail forgalom az Office 365 toodetermine eszközintelligencia hello mail valószínű, hogy nefarious vagy egy szabályos e-mail kampány hello eredményét.  

### <a name="anomaly-detection"></a>Rendellenességek észlelése
Az Azure Security Center is használja az anomáliadetektálási észlelési tooidentify fenyegetéseket. Ezzel szemben toobehavioral Analytics (ami függ a ismert mintákat származó nagy adatkészletek) anomáliadetektálás több "szabott", és alapkonfigurációk, amelyek adott tooyour központi telepítések összpontosít. Gépi tanulás alkalmazott toodetermine szokásos tevékenység a központi telepítés, és akkor szabályok által generált toodefine halegyedeket feltételek, amelyek biztonsági esemény jelenthet. Például:

* **Bejövő RDP/SSH találgatásos támadások**: az üzemelő példányokon lehetnek nagy forgalmú virtuális gépek, amelyeken nagy mennyiségű bejelentkezést hajtanak végre mindennap, és olyan virtuális gépek is, amelyeken nagyon kevés bejelentkezés történik, vagy egyáltalán nincs ilyen tevékenység. Az Azure Security Center határozza meg az alapkonfiguráció bejelentkezési tevékenységét a virtuális gépekhez, és használja a machine learning toodefine mi normál bejelentkezési tevékenység kívül esik. Ha bejelentkezések száma hello vagy hello bejelentkezéseket, vagy a mely hello bejelentkezések kért hello hely hello időpontot, vagy más bejelentkezéssel kapcsolatos jellemzők hello alaptervből jelentősen eltér, majd előfordulhat, hogy riasztást. Ebben az esetben is gépi tanulás alapján határozza meg, hogy mi számít szignifikáns eltérésnek.

## <a name="continuous-threat-intelligence-monitoring"></a>A fenyegetésekre vonatkozó intelligencia folyamatos figyelése
Az Azure Security Center biztonsági kutatási és adatok tudományos csoportok, amelyek folyamatosan figyelje a módosítások hello veszélyforrásainak tükrében megfigyelhető működik. Ez magában foglalja a következő kezdeményezések hello:

* **Fenyegetésekre vonatkozó intelligencia figyelése**: a fenyegetésekre vonatkozó intelligencia a meglévő és felmerülő fenyegetésekkel kapcsolatos mechanizmusokat, jelzőket, következtetéseket és műveleti tanácsokat foglal magában. Az információk megosztása hello biztonsági Közösségből származó és Microsoft folyamatosan figyeli a fenyegetés eszközintelligencia hírcsatornák belső és külső forrásból.
* **Jelmegosztás**: megosztjuk és elemezzük a Microsoft felhőbeli és helyszíni szolgáltatásokat, kiszolgálókat és végponti eszközök tartalmazó kiterjedt portfóliójához kapcsolódó biztonsági csoportok megfigyeléseit. 
* **A Microsoft biztonsági szakértői**: folyamatos kapcsolatot tartunk a Microsoft csapataival, amelyek tagjai meghatározott biztonsági szakterületekkel foglalkoznak, például igazságügyi szakértői tevékenységekkel és a webes támadások észlelésével.
* **Észlelési hangolása**: algoritmusok elleni valós felhasználói adatkészletek futnak, és biztonsági kutatói együttműködve ügyfelek toovalidate hello eredmények. TRUE és false figyelmeztetéséket használt toorefine gépi tanulási algoritmusok.

Új és továbbfejlesztett észlelési, amelyek révén kihasználhatja a azonnal követi ezeket az egyesített erőfeszítéseket – nincs az Ön tootake művelet.

## <a name="see-also"></a>Lásd még:
Ebben a dokumentumban megtanulta, hogyan működnek a tooAzure Security Center az észlelési képességek. További információ a Security Center toolearn hello következő lásd:

* [Útmutató az Azure Security Center tervezéséhez és működtetéséhez](security-center-planning-and-operations-guide.md)
* [Kezelése és reagálás toosecurity értesítések az Azure Security Centerben](security-center-managing-and-responding-alerts.md)
* [Biztonsági riasztások típus szerint az Azure Security Centerben](security-center-alerts-type.md)
* [Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.
* [Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.
* [Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
* [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) – Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.

