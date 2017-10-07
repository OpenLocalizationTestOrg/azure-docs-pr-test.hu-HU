---
title: "aaaAzure Fenyegetésészlelés speciális |} Microsoft Docs"
description: "Ismerje meg a azonosító adatok védelmét és képességeit."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: TomSh
ms.openlocfilehash: 63e2c541fd4ce2c571af9d5845c9a9bd4b03b2a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-advanced-threat-detection"></a>Az Azure fejlett fenyegetések észlelése
## <a name="introduction"></a>Bevezetés

### <a name="overview"></a>Áttekintés

A Microsoft kifejlesztette tanulmányok, biztonsági áttekintése, ajánlott eljárásokat és ellenőrzőlistákat tooassist Azure sorozata ügyfeleinek hello különböző biztonsági lehetőségeinek és környező hello Azure platformra. hello témakörök tartomány körének és mélysége, és rendszeresen frissülnek. Ez a dokumentum a sorozat része a következő absztrakt szakasz hello foglalja össze.

### <a name="azure-platform"></a>Az Azure Platform

Azure egy nyilvános felhőszolgáltatási platform, amely támogatja az operációs rendszerek, programozási nyelvek, keretrendszerek, eszközök, adatbázisok és eszközök hello legszélesebb kiválasztása.
Hello következő programozási nyelveket támogatja:
-   Futtassa a Linux-tárolók Docker-integráció.
-   JavaScript, Python, .NET, PHP, Java és Node.js-alkalmazások létrehozása
-   Build vissza-végpontok iOS, Android és Windows eszközökhöz.

Nyilvános Azure felhőszolgáltatások hello azonos technológiák több millió fejlesztők és informatikai szakemberek számára már alapulnak, és megbízható támogatja.

Tooa nyilvános felhőbe a szervezet végzi az áttelepítést, amikor a szervezet felelős tooprotect az adatok, és adja meg a biztonsági és irányítási hello rendszer körül.

Azure infrastruktúrája hello létesítmény tooapplications felhasználók millióit egyidejűleg üzemeltetéséhez a készült, és amelyen vállalatok számára is saját igényeihez biztonsági megbízható alaprendszert biztosít. Azure az beállítások tooconfigure széles köréről biztosít, és szabja testre a biztonsági toomeet hello az alkalmazások telepítésének követelményeinek. Ez a dokumentum segít az elvárásoknak.

### <a name="abstract"></a>Absztrakt

Azure Active Directory, Azure Operations Management Suite (OMS) és az Azure Security Center, például az advanced threat észlelési funkció használatával a beépített Microsoft Azure-ajánlatok. Ez a témakörgyűjtemény a biztonsági szolgáltatásokat és képességeket biztosít egy gyors és egyszerű módot toounderstand, mi történik az Azure-környezetekhez belül.

Ez a dokumentum végigvezeti "Microsoft Azure megközelítések" hello fenyegetés biztonsági rés diagnosztikai felé, és hello rosszindulatú tevékenységek kiszolgálók és más Azure-erőforrások elleni megcélzott társított hello kockázat elemzése. Ezzel a megoldással azonosítási tooidentify hello módszerek és biztonsági rés kezelésének optimalizált megoldások hello Azure platformon és ügyfélkapcsolati biztonsági szolgáltatásokat és technológiákat.

Ez a dokumentum elsősorban az Azure platformon és -vezérlők ügyfélkapcsolati hello technológia, és nem kísérli meg tooaddress SLA-k, árképzési modellt és DevOps eljárásokkal kapcsolatos szempontok.

## <a name="azure-active-directory-identity-protection"></a>Azure Active Directory Identity Protection

![Azure Active Directory Identity Protection](./media/azure-threat-detection/azure-threat-detection-fig1.png)


[Az Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) hello szolgáltatása [Azure AD Premium P2](https://docs.microsoft.com/azure/active-directory/active-directory-editions) hello kockázati eseményekről és a potenciális biztonsági réseket, hogy az hatással lenne a szervezet áttekintést nyújt kiadásának identitások. Microsoft rendelkezik lett biztonságossá tétele a felhőalapú identitás egy évtizedben keresztül, és az Azure AD Identity Protection, a Microsoft, hogy így ezek azonos védelmi rendszerek elérhető tooenterprise ügyfelek. Azonosító adatok védelmét a meglévő Azure AD anomáliadetektálási észlelési keresztül elérhető szolgáltatásait használja [az Azure AD rendellenes Tevékenységjelentések](https://docs.microsoft.com/azure/active-directory/active-directory-view-access-usage-reports#anomalous-activity-reports), és vezet be az új kockázat típusait, amely valós idejű rendellenességek észlelését.

Azonosító adatok védelmét adaptív gépi tanulási algoritmusok és a heurisztikus toodetect rendellenességek észlelését és a kockázati eseményekről, amelyek azt jelzik, hogy identitás feltörték használja. Ezen adatok Identity Protection állít elő, jelentéseket és riasztásokat, amelyek lehetővé teszik a tooinvestigate a kockázati eseményekről és javítási vagy a megoldás megfelelő lépéseket.

De Azure Active Directory Identity Protection több mint egy figyelési és jelentéskészítési eszköz. Kockázati események alapján Identity Protection számítja ki a felhasználó kockázati szint minden felhasználóhoz, így már tooconfigure kockázati-alapú házirendek tooautomatically védelme hello identitások a szervezet.

A kockázat-alapú házirendek hozzáadása tooother [feltételes hozzáférés-vezérlést](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) Azure Active Directory által biztosított és [EMS](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access), automatikusan letilthatja vagy kínálnak, amely tartalmazza az adaptív szervizelési műveletek jelszó alaphelyzetbe állítását és a többtényezős hitelesítés kényszerítése.

### <a name="identity-protections-capabilities"></a>Identitás-megelőzési képességek

Az Azure Active Directory azonosító adatok védelmét a rendszer több mint egy figyelési és jelentéskészítési eszköz. tooprotect a szervezet identitásait, kockázati-alapú, toodetected problémákat automatikusan válaszol, amikor a rendszer elérte a megadott kockázati szint házirendeket konfigurálhat. Ezek a házirendek továbbá tooother feltételes hozzáférés-szabályozási Azure Active Directory és az EMS biztosítja, vagy automatikusan letilthatja vagy kezdeményezheti, többek között a jelszó alaphelyzetbe állítását és a többtényezős hitelesítés kényszerítése adaptív elvégzett szervizelési műveleteket.

Példák olyan hello módszereket, amelyekkel Azure Identity Protection a fiókok biztosításában és identitások tartalmazza:

[Annak ellenőrzése, kockázati eseményekről és kockázatos fiókok:](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection#detection)
-   Gépi tanulás és heurisztikus szabályok használatával hat kockázat eseménytípusok észlelése
-   Kockázati szintek felhasználói kiszámítása
-   Egyéni javaslatok tooimprove nyújtó általános biztonsági állapotát a biztonsági rések kiemelésével

[Vizsgáló kockázati események:](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection#investigation)
-   Értesítésküldési kockázati események
-   Megfelelő és környezetfüggő információk alapján kockázati események kivizsgálása
-   A munkafolyamatok alapvető tootrack vizsgálatok biztosítása
-   Így könnyen elérhetők tooremediation műveletek, például a jelszó alaphelyzetbe állítása

[Kockázati-alapú feltételes hozzáférési házirendek:](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection#risky-sign-ins)
-   Házirend toomitigate kockázatos bejelentkezések bejelentkezések blokkolja, vagy a multi-factor authentication kihívások szükségessé tételével.
-   Házirend tooblock vagy biztonságos kockázatos felhasználói fiókok
-   Házirend toorequire felhasználók tooregister a többtényezős hitelesítés

### <a name="azure-ad-privileged-identity-management-pim"></a>Az Azure AD Privileged Identity Management (PIM)

A [az Azure Active Directory (AD) Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure),

![Azure AD Privileged Identity Management](./media/azure-threat-detection/azure-threat-detection-fig2.png)

kezelése, szabályozása, és figyelje a hozzáférést a szervezeten belül. Ez magában foglalja az Azure AD hozzáférési tooresources és más Microsoft online szolgáltatások, például az Office 365-öt vagy a Microsoft Intune.

Az Azure AD Privileged Identity Management nyújt segítséget:

-   Figyelmeztetést kap, és jelentést az Azure AD-rendszergazdák és a "csak az időben" rendszergazdai hozzáférés tooMicrosoft Online szolgáltatások, például az Office 365 és az Intune-ban

-   Rendszergazda-hozzárendelések beolvasása a rendszergazdai hozzáférési műveleteiről és a változások

-   Értesítéskérés a hozzáférés tooa kiemelt szerepkörű

## <a name="microsoft-operations-management-suite-oms"></a>A Microsoft Operations Management Suite (OMS)

[A Microsoft Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) a Microsoft felhőalapú informatikai felügyeleti megoldás, amely segít a kezelése és védelme a helyszíni és felhőalapú infrastruktúra. Mivel az OMS felhőalapú szolgáltatás, gyorsan és az infrastruktúra-szolgáltatásokra fordított minimális mértékű befektetéssel üzembe helyezhető. Új biztonsági funkciók automatikusan érkeznek, a folyamatos karbantartási mentésekor, és frissítse a költségeket.

Ezenkívül tooproviding értékes szolgáltatások a saját, OMS-ben integrálható a System Center-összetevő például [System Center Operations Manager](https://blogs.technet.microsoft.com/cbernier/2013/10/23/monitoring-windows-azure-with-system-center-operations-manager-2012-get-me-started/) tooextend a meglévő biztonsági management beruházások hello be a felhő. A System Center és OMS hogyan tudnak együttműködni egy teljes hibrid felület tooprovide.

### <a name="holistic-security-and-compliance-posture"></a>Körű biztonsági és megfelelőségi állapotát

Hello [OMS biztonsági és naplózási irányítópult](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) nyújt átfogó képet kaphat a szervezet informatikai biztonságot beépített keresési lekérdezések a jelentős figyelmet igénylő problémák. hello biztonsági és naplózási irányítópult hello kezdőképernyő minden kapcsolódó toosecurity az OMS Szolgáltatáshoz. A számítógépek biztonsági állapotának hello magas szintű betekintést biztosít. Is hello képességét tooview összes események az elmúlt 24 óra, 7 nap hello vagy bármely más egyéni időkereten belül.

Gyorsan és egyszerűen tisztában OMS-irányítópultok súgó hello hello informatikai műveleteket, köztük a környezetében, az összes bármely környezete általános biztonsági állapotát: szoftver frissítések értékelését, kártevőirtó értékelési és referenciakonfigurációkat. Ezenkívül a biztonsági naplóadatokat könnyen hozzáférhető toostreamline hello biztonsági és megfelelőségi naplózási folyamatokat.

hello OMS biztonsági és naplózási irányítópult négy fő kategóriába vannak rendezve:

![Az OMS biztonsági és auditálási irányítópultja](./media/azure-threat-detection/azure-threat-detection-fig3.jpg)

-   **Biztonsági tartományok:** ezen a területen fogja tudni toofurther felfedezés biztonsági rekordok időbeli, kártevő assessment hozzáférni, felmérési és a hálózati biztonság, a identitás- és hozzáférés adatait, a számítógépek biztonsági események frissítse, és gyorsan hozzáférés tooAzure Security Center irányítópultjának.

-   **Jelentős problémák:** lehetővé teszi az olyan tooquickly aktív problémák hello számának meghatározása, és ezek a problémák súlyosságát hello.

-   **Észlelések (előzetes verzió):** lehetővé teszi a biztonsági riasztások megjelenítése, hogy elvégezze a erőforrásokon által tooidentify támadási mintákat.

-   **Fenyegetésfelderítési adataival:** teszi tooidentify támadási mintákat által kimenő kártékony IP-forgalom, hello rosszindulatú fenyegetés típusa és megjelenítő, ha ezeket az IP-címekről érkező kiszolgálók száma hello megjelenítése.

-   **Általános biztonsági lekérdezések:** ezt a lehetőséget biztosít a leggyakrabban használt biztonsági hello listáját, lekérdezi a használható toomonitor a környezetben. Ha valamelyik lekérdezések gombra kattint, hogy a lekérdezés eredményeit hello nyílik meg hello Search paneljét.

### <a name="insight-and-analytics"></a>Betekintések és elemzés
A hello központban [Naplóelemzési](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) hello OMS-tárházban, lévő hello Azure felhőben van.

![Betekintések és elemzés](./media/azure-threat-detection/azure-threat-detection-fig4.png)

Adatgyűjtés történő hello tárházba csatlakoztatott adatforrások konfigurálását az adatforrások és hozzáadását megoldások tooyour előfizetés.

![előfizetést](./media/azure-threat-detection/azure-threat-detection-fig5.png)

Az adatforrások és a megoldások egyes létrehoz különböző rekordtípusokat, saját tulajdonságokat rendelkeznie, de előfordulhat, hogy továbbra is elemezheti együtt lekérdezések toohello tárházban. Ez lehetővé teszi a különböző típusú adatok azonos eszközök és módszerek toowork különböző forrásokból gyűjtött toouse hello.


A Log Analyticshez való együttműködéshez a legtöbb hello OMS-portálon, amely minden böngésző fut, és hozzáférési tooconfiguration beállításokat és több eszközök tooanalyze és az összegyűjtött adatok act biztosít keresztül történik. Hello portálról, használhat [keresések jelentkezzen](https://docs.microsoft.com/azure/log-analytics/log-analytics-log-searches) , hozhat létre lekérdezéseket tooanalyze gyűjtött adatokat, ha [irányítópultok](https://docs.microsoft.com/azure/log-analytics/log-analytics-dashboards), amely testre szabhatja a legértékesebb keresések és grafikusnézetek[megoldások](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions), amelyek biztosítják, hogy további funkciók és -elemző eszközökkel.

![elemzésére szolgáló eszközöket](./media/azure-threat-detection/azure-threat-detection-fig6.png)

Megoldások funkcióit tooLog Analytics hozzáadása. Elsősorban hello felhőben futtatható és adja meg a hello OMS-tárházban gyűjtött adatok elemzése. Akkor is határozhatnak meg új rekordtípusokhoz toobe gyűjtött, a napló keresések vagy hello OMS irányítópult hello megoldás által biztosított további felhasználói felület elemzése.
hello biztonsági és naplózási példája a következő típusú megoldások.



### <a name="automation--control-alert-on-security-configuration-drifts"></a>Automatizálási & vezérlő: biztonsági beállítások a riasztás drifts

Azure Automation szolgáltatásbeli adminisztratív folyamatok, amelyek alapján a PowerShell és hello Azure felhőben futtatható runbookok automatizálja. A Runbookok is az adott kiszolgálón a helyi adatközponti toomanage helyi erőforrásokat az hajtható végre. Azure Automation szolgáltatásbeli konfiguráció kezelése a PowerShell DSC (célállapot-konfiguráció) biztosít.

![Azure Automation](./media/azure-threat-detection/azure-threat-detection-fig7.png)

Hozzon létre és Azure-ban üzemeltetett DSC-erőforrások kezelése és alkalmazza őket toocloud és a helyszíni rendszer toodefine és automatikusan azok konfigurálásának kényszerítésére vagy szerezhetők be jelentések az eltéréseket toohelp biztosítását, hogy a biztonsági beállításokkal házirend maradnak.

## <a name="azure-security-center"></a>Azure Security Center

Azure Security Center segítségével, az Azure-erőforrások védelme. Biztosít integrált biztonsági monitorozást és az Azure-előfizetések. Belül hello szolgáltatást le is tudja toodefine házirendek nem csak az Azure-előfizetések ellen is elleni [erőforráscsoportok](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal), így pontosabban is lehet.

![Azure Security Center](./media/azure-threat-detection/azure-threat-detection-fig8.png)

A Microsoft biztonsági kutatói folyamatosan hello lookout fenyegetések esetén. Hozzáférés tooan kiterjedtnek beállítása a Microsoft globális jelenlét hello felhőben és helyszíni megosztjuk telemetriai adatot rendelkeznek. Ezen adatkészletek wide elérése és sokszínű gyűjteménye lehetővé teszi a Microsoft toodiscover új támadási mintákat és trendeket a helyszíni fogyasztói, valamint vállalati termékei, valamint az online szolgáltatások között.

Ebből kifolyólag a Security Center képes gyorsan frissíti az észlelési algoritmusokat támadók kiadás új és egyre kifinomult biztonsági réseket. Ez a megközelítés segít a gyorsan változó fenyegetésekkel teli világában lépést tartani.

![Security Center](./media/azure-threat-detection/azure-threat-detection-fig9.jpg)

A Security Center fenyegetésészlelés működik, automatikusan az Azure-erőforrások, hello és hálózati összekapcsolt partnermegoldások a biztonsági információk begyűjtése.  Megvizsgálja ezt az információt, több forrásból tooidentify fenyegetések információk használatával történik.
Biztonsági riasztások a Security Center kerülnek előrébb, valamint javaslatokat a hogyan tooremediate hello fenyegetést.

A Security Center olyan fejlett biztonsági elemzéseket alkalmaz, amelyek messze túlmutatnak az aláírás-alapú megközelítéseken. A big Data típusú adatok eredményeket és [gépi tanulás](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) technológiák által használt tooevaluate események hello teljes felhőbeli háló – lenne lehetetlen tooidentify manuális módszer segítségével, és hello előrejelzésére szolgáló észlelése támadások alakulását. A biztonsági elemzés hello következőket tartalmazza.

### <a name="threat-intelligence"></a>Fenyegetések felderítése

A Microsoft rendkívül nagy mennyiségű adattal rendelkezik a globális fenyegetésészlelési intelligencia keretein belül.
A telemetriai adatok több forrásból, például az Azure, Office 365, Microsoft CRM online, a Microsoft Dynamics AX, Outlook.com-os, MSN.com, hello Microsoft Digital Crimes Unit (DCU) és a Microsoft biztonsági válasz Center (MSRC) zajló kommunikációról.

![Fenyegetések felderítése](./media/azure-threat-detection/azure-threat-detection-fig10.jpg)

Kutatói is fenyegetés eszközintelligencia információkat kaphat, amelyek jelentős felhőszolgáltatók között megosztott és harmadik féltől származó toothreat eszközintelligencia hírcsatornák előfizet. Az Azure Security Center használhatja az információk tooalert ismert ezeken a toothreats meg. Néhány példa:

-   **Energiagazdálkodási a Machine Learning - kifejlesztése hello** az Azure Security Center rendelkezik hozzáférési tooa hatalmas mennyiségű adatot felhő hálózati tevékenységről, amely lehet használni az Azure-környezetekhez célzó toodetect fenyegetéseket. Példa:

-   **Találgatásos kényszerített észlelések -** gépi tanulási használt toocreate távelérési kísérlet egy korábbi mintát, amely lehetővé teszi, hogy toodetect találgatásos támadások ellen SSH, RDP és az SQL-port.

-   **Kimenő DDoS és Botnet észlelési** -támadások célzó felhőalapú erőforrások közös célkitűzése van toouse hello számítási teljesítményt az ezen erőforrások tooexecute más támadásoknak.

-   **Új viselkedéssel összefüggő Analytics kiszolgálók és virtuális gépek -** után egy kiszolgáló vagy a virtuális gép biztonsága sérül, a támadók technikák tooexecute kártékony kódot, hogy a rendszer számos alkalmaz észlelési elkerülése, biztosítva az adatmegőrzési és így nem biztonsági vezérlők.

-   **Az Azure SQL adatbázis Fenyegetésészlelés -** Fenyegetésészlelés az Azure SQL Database, amely azonosítja az adatbázist érintő rendellenes tevékenységeket szokatlan és potenciálisan káros kísérletek tooaccess vagy biztonsági rés adatbázisok.

### <a name="behavioral-analytics"></a>Működés elemzése

Viselkedéselemzés a technika, amely elemzi, és összehasonlítja a tooa adatgyűjtés ismert minták. Ezek a minták azonban nem csak egyszerű aláírások. Azok a határoz meg, amelyek alkalmazott toomassive adatkészletek összetett gépi tanulási algoritmusok keresztül.

![Működés elemzése](./media/azure-threat-detection/azure-threat-detection-fig11.jpg)


Ezenkívül szakértő elemzők mélyrehatóan elemezték a kártékony működést a meghatározásukhoz. Az Azure Security Center viselkedéselemzés sérült tooidentify erőforrásokat a virtuális gép naplóinak, virtuális hálózati eszköznaplók, háló naplókat, összeomlási memóriaképek és egyéb forrásokból elemzése alapján használhatja.

Emellett nincs korrelációban állnak más jelek toocheck bizonyíték a széles körű kampány támogatásához. A korrelációs konzisztensek az illetéktelen behatolásoknak meghatározott mutatók tooidentify események segítségével.

Néhány példa:
-   **Gyanús folyamatok végrehajtását:** támadók több technikák tooexecute kártevő szoftverek észlelése nélkül alkalmaz. Például egy támadó előfordulhat, hogy adjon kártevő hello jogos rendszerfájlok azonos néven, de ezeket a fájlokat egy másodlagos helyen helyezze el, használja a nagyon hasonlít jóindulatú fájlból, illetve maszk hello igaz fájlkiterjesztés. A Security Center modellek folyamatok működést és a figyelők feldolgozni végrehajtások toodetect kiugró ehhez hasonló helyzeteknek.

-   **Kártevők és kihasználásának kísérletek rejtett:** kifinomult kártevők is foganatosításával hagyományos kártevőirtó termékek soha nem toodisk írása vagy szoftverösszetevőket lemezen tárolt titkosított. Azonban ilyen kártevő észlelhető használatával memória elemzés, hello kártevő kell hagyja a nyomkövetések memória toofunction. Ha szoftver összeomlik, összeomlási memóriaképet egy részéhez memória hello összeomlási hello helyreállításkor rögzíti. Hello összeomlási memóriakép hello memóriája elemzésével, az Azure Security Center képesek észlelni technikák tooexploit biztonsági rések használt szoftver, bizalmas adatok eléréséhez, és rejtett megőrzéséhez a sérült biztonságú gépekről hello teljesítményére gyakorolt hatás nélkül a számítógépen.

-   **Oldalirányú mozgás és a belső reconnaissance:** toopersist a sérült biztonságú hálózati, és keresse meg vagy betakarítás értékes adatok, a támadók gyakran kísérlik meg toomove könnyebben hello sérült a gép tooothers belül hello ugyanazon a hálózaton. A Security Center folyamat figyeli, és bejelentkezési tevékenységek toodiscover kísérletek tooexpand egy támadó foothold hello hálózatról, például távoli utasítás végrehajtása hálózati probing vagy fiók enumerálása.

-   **PowerShell-parancsfájlok rosszindulatú:** PowerShell támadók tooexecute kártékony kódot a cél virtuális gépek által használható a különböző célt. A Security Center megvizsgálja a PowerShell tevékenységeit, hogy megtalálja a gyanús tevékenységek nyomait.

-   **Kimenő támadások:** a támadók gyakran cél hello segítségével ezen erőforrások toomount további támadások célja a felhőben található erőforrásokat. Feltört virtuális gépek, például lehet, hogy más virtuális gépek elleni használt toolaunch találgatásos támadások ellen, LEVÉLSZEMETET küld, vagy megnyitott portok és egyéb eszközöket hello Internet vizsgálata. Úgy, hogy alkalmazza a machine learning toonetwork forgalom, Security Center képes észlelni kimenő hálózati kommunikáció-nál nagyobb hello alapértelmezetté. Ha a LEVÉLSZEMÉT, a Security Center is korrelációban szokatlan e-mail forgalom az Office 365 toodetermine intelligence, üdvözlő levelek valószínű, hogy nefarious vagy egy szabályos e-mail kampány hello eredményét.

### <a name="anomaly-detection"></a>Anomáliadetektálás

Az Azure Security Center is használja az anomáliadetektálási észlelési tooidentify fenyegetéseket. Ezzel szemben toobehavioral Analytics (ami függ a ismert mintákat származó nagy adatkészletek) anomáliadetektálás több "szabott", és alapkonfigurációk, amelyek adott tooyour központi telepítések összpontosít. Gépi tanulás alkalmazott toodetermine szokásos tevékenység a központi telepítés, és akkor szabályok által generált toodefine halegyedeket feltételek, amelyek biztonsági esemény jelenthet. Például:

-   **Bejövő RDP/SSH találgatásos támadások:** a kialakítás esetében előfordulhat, sok bejelentkezések foglalt virtuális gépek minden nap és egyéb rendelkező virtuális gépek néhány vagy minden bejelentkezések. Az Azure Security Center határozza meg az alapkonfiguráció bejelentkezési tevékenységét a virtuális gépekhez, és használja a machine learning toodefine hello normál bejelentkezési tevékenységek körül. Ha meghatározott hello együtt eltérés van a bejelentkezési kapcsolatos jellemzőit, akkor lehetséges, hogy riasztást. Ebben az esetben is gépi tanulás alapján határozza meg, hogy mi számít szignifikáns eltérésnek.

### <a name="continuous-threat-intelligence-monitoring"></a>Folyamatos Fenyegetésfelderítési adataival figyelése

Az Azure Security Center biztonsági kutatási és adatok tudományos csoportok teljes hello world, amely folyamatosan figyelje a módosítások hello veszélyforrásainak tükrében megfigyelhető, működik. Ez magában foglalja a következő kezdeményezések hello:

-   **Fenyegetés eszközintelligencia figyelési:** fenyegetés eszközintelligencia mechanizmusok, a mutatók, a gyakorolt hatása és a meglévő vagy megjelenő fenyegetéseket végrehajthatóként útmutatást tartalmaz. Az információk megosztása hello biztonsági Közösségből származó és Microsoft folyamatosan figyeli a fenyegetés eszközintelligencia hírcsatornák belső és külső forrásból.

-   **Jel megosztása:** információkat kaphat a felhőalapú és helyszíni szolgáltatásokat, a kiszolgálók és az ügyfél végponti eszközök széles körű kaphat a Microsoft biztonsági csoportjai megosztott és elemzése.

-   **Microsoft adatbiztonsági szakemberek:** folyamatban lévő engagement az karbantartó csoportok között Microsoft speciális biztonsági mezők, például a törvényszéki használhatók, és a webalkalmazás-támadás észlelése.

-   **Észlelési hangolása:** algoritmusok elleni valós felhasználói adatkészletek futnak, és biztonsági kutatói együttműködve ügyfelek toovalidate hello eredmények. TRUE és false figyelmeztetéséket használt toorefine gépi tanulási algoritmusok.

Új és továbbfejlesztett észlelési, amelyek révén kihasználhatja a azonnal követi ezeket az egyesített erőfeszítéseket – nincs az Ön tootake művelet.

## <a name="advanced-threat-detection-features---other-azure-services"></a>Az Advanced Threat szolgáltatásai – más Azure-szolgáltatásokkal

### <a name="virtual-machine-microsoft-antimalware"></a>Virtuális gép: Microsoft Antimalware

[A Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) Azure single-ügynök megoldás az alkalmazások és a bérlői környezetben van, tervezett toorun hello háttérben emberi beavatkozás nélkül. Az alkalmazás-munkaterhelések, vagy alapvető biztonságos--alapértelmezés szerint a hello igények alapján, vagy egyéni konfiguráció – beleértve a kártevőirtó-figyelés speciális védelem telepítése. Azure kártevőirtó az Azure virtuális gépek biztonsági megoldás, és automatikusan települ az összes Azure PaaS virtuális gépet.

**Azure toodeploy és a Microsoft Antimalware engedélyezése az alkalmazások képességei**

#### <a name="microsoft-antimalware-core-features"></a>A Microsoft Antimalware alapszolgáltatások

-   **A valós idejű védelem -** figyeli a Felhőszolgáltatások és virtuális gépek toodetect, és letiltja a kártevő szoftverek végrehajtásakor.

-   **Ütemezett ellenőrzés -** rendszeres időközönként célzott beolvasási toodetect kártevő szoftverek, beleértve, aktívan futó programok.

-   **Kártevő szoftver eltávolításának -** automatikusan hajt végre műveletet a kártevő, például törlése vagy a karanténba helyezés kártevő fájlok és a rosszindulatú beállításjegyzék-bejegyzések törlése.

-   **Aláírás frissítések -** automatikusan telepíti hello legújabb védelmi aláírások (vírus-definíciók) tooensure védelem egy előre meghatározott gyakoriságáról naprakész.

-   **Kártevőirtó motor frissítéseit -** automatikusan a frissítéseket a Microsoft Antimalware motor hello.

-   **Kártevőirtó Platform frissítések –** automatikusan a frissítéseket a Microsoft Antimalware platform hello.

-   **Aktív védelem -** telemetriai metaadatok jelentések fenyegetésről és gyanús erőforrások tooMicrosoft Azure tooensure gyors válasz toohello veszélyforrásainak tükrében megfigyelhető fejlődnek, illetve engedélyezni kívánja a valós idejű szinkron aláírás kézbesítési keresztül hello Microsoft Active Protection rendszer (MAPS).

-   **Jelentéskészítés - minták** biztosít, és minták jelentések toohello Microsoft Antimalware szolgáltatás toohelp pontosítsa hello szolgáltatást és a hibaelhárítási engedélyezése.

-   **Kizárások –** lehetővé teszi, hogy az alkalmazás és szolgáltatás-rendszergazdák tooconfigure bizonyos fájlokat, a folyamatokat, és a meghajtók tooexclude őket a védelmi és beolvasó teljesítménye és/vagy más meggondolásból.

-   **Eseménygyűjtés kártevőirtó -** hello kártevőirtó szolgáltatás állapota, a gyanús tevékenységek és a szervizelés műveleteit hello operációs rendszer-eseménynaplójában rögzíti, és összegyűjti a hello ügyfél Azure Storage figyelembe.

### <a name="azure-sql-database-threat-detection"></a>Az Azure SQL adatbázis fenyegetések észlelése

[Az Azure SQL adatbázis Fenyegetésészlelés](https://azure.microsoft.com/blog/azure-sql-database-threat-detection-your-built-in-security-expert/) egy új biztonsági az eszközintelligencia szolgáltatás épített hello Azure SQL Database szolgáltatásban. Megkerülő megoldásának végrehajtásához hello toolearn, profil óra és adatbázist érintő rendellenes tevékenységeket észleli, az Azure SQL adatbázis Fenyegetésészlelés azonosítja a potenciális fenyegetések toohello adatbázis.

Biztonsági tisztviselő vagy más kijelölt rendszergazdák kérheti le az azonnali értesítések adatbázis gyanús tevékenységekről azok bekövetkezésekor. Minden értesítés részletesen hello gyanús tevékenységekre, illetve hogyan toofurther vizsgálja meg, és hello fenyegetések mérséklésére javasolja.

Azure SQL adatbázis Fenyegetésészlelés jelenleg észleli a potenciális biztonsági réseket és az SQL injektálási támadások és a rendellenes adatbázis hozzáférési minták.

Fenyegetések észlelése e-mail értesítést kap, felhasználók bejegyzései képes toonavigate és nézet hello vonatkozó naplózási hello mélyhivatkozása használatát, amely megnyitja a naplózási viewer és/vagy előre beállított naplózási Excel-sablon, amely tartalmazza a megfelelő naplózási hello hello mail rekordok körül hello idő hello gyanús esemény toohello következő szerint:
-   Hello adatbázis-kiszolgáló hello adatbázist érintő rendellenes tevékenységeket a naplózási tároló

-   Naplózási toowrite hello Eseménynapló hello során használt vonatkozó naplózási tároló tábla

-   Naplózási hello az esemény akkor következik be, mert a következő óra hello rekordjait.

-   Naplózási rekordokat hasonló eseményazonosító hello időpontban hello esemény (néhány érzékelők esetén nem kötelező)

SQL adatbázis fenyegetés érzékelők hello a következő észlelési módszerek valamelyikével:

-   **Determinisztikus észlelést –** gyanús mintázatok (alapuló szabályok) észleli hello SQL ügyfél lekérdezések, amelyek megfelelnek az ismert támadásokat. Ez a módszer van magas felderítését és alacsony vakriasztás, azonban csak korlátozott érvényességének, mert a "atomi észlelések" hello kategóriába tartozik.

-   **Viselkedési észlelés –** rendellenes tevékenységet, amely korábban nem látott során hello utolsó 30 nap hello adatbázis rendellenes viselkedés hibák.  Egy példa az SQL client rendellenes tevékenységet egy csúcs sikertelen bejelentkezések/lekérdezések, nagy mennyiségű adatok kibontása közben, szokatlan kanonikus lekérdezések és ismeretlen IP használt címek tooaccess hello adatbázis is lehet.

### <a name="application-gateway-web-application-firewall"></a>Alkalmazás átjáró webalkalmazási tűzfal

[Webalkalmazási tűzfal](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-web-application-firewall) szolgáltatása [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-webapplicationfirewall-overview) , amely védelmet biztosít a standard Alkalmazásátjáró használó tooweb alkalmazások [Alkalmazásvezérlés kézbesítési](https://kemptechnologies.com/in/application-delivery-controllers) funkciók. Webalkalmazási tűzfal ezt hajtja végre őket a legtöbb hello elleni védelme [OWASP felső 10 közös webes biztonsági rések](https://www.owasp.org/index.php/Top_10_2010-Main)

![Alkalmazás átjáró webalkalmazás Firewally](./media/azure-threat-detection/azure-threat-detection-fig13.png)

-   SQL-injektálás elleni védelem

-   Webhelyek közötti, parancsprogramot alkalmazó támadások elleni védelem

-   Gyakori webes támadások (például parancsinjektálás, HTTP-kéréscsempészet, HTTP-válaszfelosztás és távolifájl-beszúrásos támadás) elleni védelem

-   HTTP protokoll megsértése elleni védelem

-   HTTP protokollanomáliák (például hiányzó gazdagép-felhasználói ügynök és Accept (Elfogadás) fejlécek) elleni védelem

-   Robotprogramok, webbejárók és képolvasók elleni védelem

-   A gyakori alkalmazás konfigurációs hibák (Ez azt jelenti, hogy Apache, az IIS, stb.) észlelése

A következő juttatás tooyou hello WAF konfigurál Application Gateway biztosítja:

-   A webes alkalmazás webes biztonsági rések és toobackend kód módosítása nélkül támadások védelméhez.

-   Több webkiszolgáló védelme hello alkalmazásokat azonos időben Alkalmazásátjáró mögött. Alkalmazásátjáró támogatja az üzemeltető too20 webhelyek védhető sikerült összes webes támadások elleni egyetlen átjáró mögötti be.

-   Az Application Gateway WAF-naplók által létrehozott valós idejű jelentés segítségével figyelheti a webalkalmazást a támadások tekintetében.

-   Bizonyos megfelelőségi vezérlők végpontok toobe WAF-megoldás által védett összes internetes igényelnek. Az engedélyezett webalkalmazási tűzfallal rendelkező Application Gateway használatával teljesítheti ezeket a megfelelőségi követelményeket.

### <a name="anomaly-detection--an-api-built-with-azure-machine-learning"></a>Anomáliadetektálás – az Azure Machine Learning-val készült API-k

Anomáliadetektálás az API-k, amelyek akkor hasznos, ha különböző típusú rendellenes minták észlelésére az az idő adatsorok Azure Machine Learning-val készült. hello API hozzárendel egy tooeach adatpont pontszám hello a time series, amely használható a riasztások generálásához, az anomáliadetektálási irányítópultjaink figyelése, vagy a hibajegyalapú rendszert való csatlakoztatásra.

Hello [Anomáliadetektálási észlelési API](https://docs.microsoft.com/azure/machine-learning/machine-learning-apps-anomaly-detection-api) rendellenességeket típusú idő adatsor a következő hello képes észlelni:

-   **Napra és esik:** például figyelésekor bejelentkezési hibák tooa szolgáltatás hello száma vagy egy elektronikus kereskedelmi webhely, a kivételek száma szokatlan igényeiben jelentkező vagy immerzióban nem jelzi a biztonsági támadások vagy szolgáltatás üzemzavarokhoz vezethet.

-   **Pozitív és negatív trendek:** memóriahasználat számítástechnikai figyelésekor például zsugorítását a szabad memória mérete, a potenciális memóriavesztés; szolgáltatás várólista hossza figyelésekor egy állandó növekvő tendenciát jelezheti alapul szolgáló szoftver probléma.

-   **Módosítások és a dinamikus értéktartományhoz változásainak szinten:** például szintje változik a szolgáltatási késéseket után a szolgáltatás frissítése vagy alacsonyabb szintű kivételek, miután érdekes toomonitor lehet.

hello machine learning-alapú API:

-   **Rugalmas és robusztus észlelési:** hello anomáliadetektálási észlelési modellek tooconfigure érzékenységi beállításai módosításának engedélyezése a felhasználók, és határozza és nem határozza adatkészlet között rendellenességek észlelését. A felhasználók módosíthatják hello észlelési modell toomake hello anomáliadetektálás API kevesebb vagy érzékenyebb függően tootheir kell. Ez azt jelentené hello észlelése több vagy kevesebb látható adatok rendelkező és anélküli határozza minták rendellenességeinek.

-   **Méretezhető és időben történő észlelése:** hello hagyományos módon szakértői tartomány Tudásbázis által beállított jelen küszöbökkel figyelési költséges és nem méretezhető toomillions dinamikusan változó adatkészletek. megjegyzett hello anomáliadetektálási észlelési modellek ezt az API-ban, és modellek automatikusan hangolt előzmény- és a valós idejű adatokból.

-   **Proaktív és végrehajthatóként észlelési:** lassú trend és szint módosítása észlelési közüli korai alkalmazhatók. hello korai rendellenes jelek észlelése használt toodirect emberek tooinvestigate lehetnek és hello problémás területek működjön.  Ezenkívül alapvető oka elemzési modellek és riasztási eszközök lehetnek a anomáliadetektálás API szolgáltatás felett.

hello anomáliadetektálás API egy eredményesebbé és hatékonyabbá teszi megoldás, mint szolgáltatás állapotát & figyelési, IoT, a teljesítmény figyelése és a hálózati forgalom figyelése KPI forgatókönyvek széles köre. Az alábbiakban néhány népszerű forgatókönyv, ahol ez az API lehet hasznos:
- Az informatikai részlegeknek képesnek kell az eszközök tootrack események, a hibakód, a napló és a teljesítmény (a Processzor, memória- és így tovább) időben.

-   Online kereskedelmi helyeken szeretné tootrack felhasználói tevékenységek, Lapmegtekintések, kattint, és így tovább.

-   Segédprogram vállalatok szeretné tootrack felhasználás vízjel, gáz, elektromosság kialakulása és egyéb erőforrásokhoz.

-   Szeretné, hogy létesítmény/épület szolgáltatások toomonitor hőmérséklet, nedvesség, forgalom és így tovább.

-   Az IoT/gyártók szeretné toouse érzékelőadatok idő adatsorozat toomonitor munkafolyamat, minőségét és így tovább.

-   Szolgáltatók, például a telefonos ügyfélszolgálatok kell toomonitor szolgáltatás igény szerinti trend, incidens kötet, várjon, amíg várólista hossza és így tovább.

-   Üzleti elemzés csoportokat szeretné, hogy toomonitor üzleti KPI-k (például az értékesítési kötet ügyfél hangulati elemek díjszabás) rendellenes adatátviteli valós időben.

### <a name="cloud-app-security"></a>A cloud App Security

[Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) hello Microsoft Cloud biztonsági verem kritikus összetevője. Egy átfogó megoldást, amely segít a szervezete teljes mértékben tootake hello ígéret felhőalapú alkalmazások áthelyezése, de az irányítást, a tevékenység jobb átláthatóságával keresztül nyomon. Emellett segít felhőalkalmazások kritikus adatainak védelmét hello növeléséhez.

Eszközöket, amelyek segítenek az árnyékinformatika informatikai kockázatfelmérés, a házirendek kényszerítését, tevékenységek vizsgálata, és veszélyek megakadályozása, amely a szervezet több biztonságosan haladhat toohello felhőbe a kritikus adatok felügyelete megőrzésével.

<table style="width:100%">
 <tr>
   <td>Ismertetők</td>
   <td>A Cloud App Security rendelkező informatikai árnyékinformatika. Hogy lássák felderíti az alkalmazások, tevékenységekhez, felhasználók, adatokat és fájlokat a felhőalapú környezetben. Csatlakoztatott külső alkalmazások felderítése tooyour felhő.</td>
 </tr>
 <tr>
   <td>Vizsgálja meg</td>
   <td>Vizsgálja meg a felhőalkalmazások felhő Törvényszéki eszközök toodeep részletesen a kockázatos alkalmazásokat, egyes felhasználók és a hálózati fájlokat. Minták található hello adatokat a felhőből gyűjtött. Jelentések toomonitor készítése a felhőben.</td>

 </tr>
 <tr>
   <td>vezérlő</td>
   <td>A kockázatnak a mérséklése úgy, hogy házirendek és a riasztások maximális vezérlő tooachieve hálózati felhőforgalom felett. A Cloud App Security toomigrate a felhasználók toosafe, engedélyezett felhőalkalmazás-alternatívákra használja.</td>

 </tr>
 <tr>
   <td>Védelem</td>
   <td>Használja a Cloud App Security toosanction alkalmazások, adatveszteség-megelőzési, hozzáféréssel és megosztása, illetve egyéni jelentések és értesítések.</td>

 </tr>
 <tr>
   <td>vezérlő</td>
   <td>A kockázatnak a mérséklése úgy, hogy házirendek és a riasztások maximális vezérlő tooachieve hálózati felhőforgalom felett. A Cloud App Security toomigrate a felhasználók toosafe, engedélyezett felhőalkalmazás-alternatívákra használja.</td>

 </tr>
</table>


![A cloud App Security](./media/azure-threat-detection/azure-threat-detection-fig14.png)

A cloud App Security integrálható a felhő által látható

-   A Cloud Discovery toomap használatával a felhőalapú környezet azonosítását és a szervezet által használt felhőalkalmazások hello.


-   Engedélyezése és tiltása a felhőben lévő alkalmazásokhoz.



-   Előnyeit-szolgáltató API-k, láthatósága és irányítása a csatlakoztatott alkalmazások könnyű telepítése alkalmazás-összekötők használatával.

-   Gondoskodik a folyamatos ellenőrzés-beállítást, és majd folyamatosan pontosabb beállításra, házirendekkel rendelkeznek.

A gyűjtött adatokat e forrásokból, a Cloud App Security kifinomult elemzést futtat hello adatokon. Ez azonnal tooanomalous tevékenységek figyelmeztet, és betekintést nyújt a részletes a felhőalapú környezetben. A Cloud App Security házirend konfigurálása és a tooprotect használni minden, a felhőalapú környezetben.

## <a name="third-party-atd-capabilities-through-azure-marketplace"></a>Az Azure piactér külső ATD képességeit

### <a name="web-application-firewall"></a>Web Application Firewall (Webalkalmazási tűzfal)

Webalkalmazási tűzfal megvizsgálja a bejövő webes forgalmat és blokkolja SQL-utasítások, többhelyes parancsfájlok, kártevő szoftverek feltöltések & DDoS alkalmazás és más a webalkalmazások irányuló támadások. Azt is ellenőrzi hello háttér-webkiszolgálók hello válaszainak az adatok adatvesztés-megelőzési (DLP). hello integrált access control motor hitelesítési, engedélyezési és nyilvántartási (AAA), amely lehetőséget nyújt a szervezetek erős hitelesítés és a felhasználói vezérlő lehetővé teszi a rendszergazdák toocreate részletes hozzáférés-vezérlési házirendeket.

**Emeli ki:**
-   Észleli és blokkolja a SQL alkalommal, idegen, kártevő szoftverek feltöltések, alkalmazás DDoS vagy bármely egyéb támadások elleni az alkalmazás.

-   Hitelesítési és hozzáférés-vezérlés.

-   Ellenőrzi a kimenő forgalom toodetect bizalmas adatokat, és a maszkolás módosítása vagy letiltja a szivárogjanak hello információkat.

-   Webes alkalmazás tartalmát, például a gyorsítótárazás, tömörítés és egyéb forgalom optimalizálása funkciókkal hello kézbesítését növekszik.

Az alábbiakban az Azure piactéren elérhető webalkalmazás tűzfalak példát:

[Webalkalmazási tűzfal barracuda, Brocade virtuális webalkalmazási tűzfal (Brocade vWAF), Imperva SecureSphere & hello ThreatSTOP IP tűzfal.](https://azure.microsoft.com/marketplace/partners/brocade_communications/brocade-virtual-web-application-firewall-templatevtmcluster/)

## <a name="next-steps"></a>Következő lépések

- [Az Azure Security Center észlelési képességei](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities)

Az Azure Security Center speciális észlelési képességek segítségével a Microsoft Azure-erőforrások célzó tooidentify aktív fenyegetéseket, és biztosít, amelyen hello kellett toorespond gyorsan.

- [Az Azure SQL adatbázis fenyegetések észlelése](https://azure.microsoft.com/blog/azure-sql-database-threat-detection-your-built-in-security-expert/)

Az Azure SQL adatbázis Fenyegetésészlelés segített a potenciális fenyegetések tootheir adatbázis kétségei cím.
