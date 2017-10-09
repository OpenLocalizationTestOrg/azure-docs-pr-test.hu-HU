---
title: "az IoT Suite aaaAzure csatlakoztatott gyári áttekintése |} Microsoft Docs"
description: "Hello Azure IoT Suite leírása előre konfigurált gyári megoldás csatlakoztatva."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: dobett
ms.openlocfilehash: 929b5ed41ef7d82c9b4480d02aa3f0db38931919
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-connected-factory-preconfigured-solution"></a>Ismerkedés a csatlakoztatott hello előre konfigurált gyári megoldás

Az Azure IoT Suite [előre konfigurált megoldások] [ lnk-preconfigured-solutions] több Azure IoT szolgáltatások toodeliver végpontok közötti megoldások, amelyek megvalósítják az IoT elterjedt üzleti forgatókönyvek kombinálni. Hello *csatlakoztatott gyári* előkonfigurált megoldás csatlakozik tooand figyelők az ipari eszközök. Használhatja a hello megoldás tooanalyze hello adatfolyamban az adatok az eszközök és a toodrive működési hatékonyság és a nyereségességre nézve.

Az oktatóanyag bemutatja, hogyan tooprovision hello csatlakoztatva gyári előre konfigurált megoldás. Azt is bemutatja, hogyan hello alapvető szolgáltatások előre konfigurált hello megoldás. Érheti el ezeket a funkciókat számos hello megoldás *irányítópult* , előre konfigurált hello megoldás részeként telepíti:

![Az előre konfigurált csatlakoztatott gyár megoldás irányítópultja][img-cf-home]

toocomplete ebben az oktatóanyagban aktív Azure-előfizetés szükséges.

> [!NOTE]
> Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információ: [Ingyenes Azure-fiók létrehozása][lnk_free_trial].
> 
> 

## <a name="provision-hello-solution"></a>Kiépítés hello megoldás

1. Jelentkezzen be Azure-fiók hitelesítő adataival tooazureiotsuite.com, és kattintson a "**+**" toocreate megoldást.
2. Kattintson a **válasszon** a hello **csatlakoztatott gyári** csempére.
3. Adja meg a **Megoldásnevet** az előre konfigurált csatlakoztatott gyár megoldáshoz.
4. Jelölje be hello **előfizetés** és **régió** kívánt toouse tooprovision hello megoldás.
5. Kattintson a **megoldás létrehozása** toobegin hello létesítésének folyamatát kell használnia. Ez a folyamat általában több percet toorun időt vesz igénybe.

### <a name="while-you-wait-for-hello-provisioning-process-toocomplete"></a>A kiépítési folyamat toocomplete hello várakozás közben

1. Kattintson a megoldás a hello csempe **kiépítési** állapotát.
2. Értesítés hello **állapotok kiépítés** , Azure-szolgáltatások vannak telepítve az Azure-előfizetéshez.
3. Miután kiépítése befejeződött, hello állapotmódosítások túl**készen**.
4. A megoldás hello jobb oldali ablaktáblában hello csempe toosee hello Részletek gombra.

> [!NOTE]
> Ha hibát tapasztal előre konfigurált hello megoldás telepítésének, tekintse át a [hello azureiotsuite.com hely engedélyeinek] [ lnk-permissions] és hello [csatlakoztatott gyári gyakran ismételt kérdések](iot-suite-faq-cf.md). Ha hello problémák továbbra is fennáll, hozzon létre szolgáltatásjegyet a hello [portal][lnk-portal].

Vannak-e részletek toosee teheti meg, amelyek nem jelennek meg a megoldáshoz? A [felhasználói visszajelzési webhelyen](https://feedback.azure.com/forums/321918-azure-iot) elküldheti a szolgáltatásokkal kapcsolatos javaslatait.

## <a name="scenario-overview"></a>Forgatókönyv áttekintése

Csatlakoztatott hello telepítésekor gyári előre konfigurált megoldás, azt az erőforrásokat, amelyek lehetővé teszik a toostep keresztül ipari forgatókönyve előre feltöltve. Ebben a forgatókönyvben számos előállítók csatlakoztatott toohello megoldás jelentés hello adatok értékek szükséges toocompute berendezések hatékonyságot (OEE) és fő teljesítménymutatók (KPI). hello következő szakaszok bemutatják, hogyan számára:

* Gyárakra, gyártósorokra, állomásokra vonatkozó OEE- és KPI-értékek figyelése
* Ezek az eszközök Azure idő adatsorozat Insights alapján generált hello telemetriai adatainak elemzése
* A riasztások toofix problémák működésre

Ebben a forgatókönyvben alapfunkciója, hogy távolról is végrehajtható, ezek a műveletek hello megoldás irányítópulton. Nem kell fizikai hozzáférés toohello eszközök.

## <a name="view-hello-solution-dashboard"></a>Nézet hello megoldás irányítópultja

hello megoldás irányítópultja toomanage telepített hello megoldás lehetővé teszi. Az irányítópult egy globális gyárkonfiguráció hierarchikus megjelenítése, amelyen megtekintheti például az OEE-ket és KPI-ket, és új csomópontokat tehet közzé a telemetriához és műveleti riasztásokhoz.

1. Hello kiépítés befejeztével, miután az előkonfigurált megoldás hello csempe jelzi **készen áll a**, válassza ki **indítsa el** tooopen a csatlakoztatott gyári megoldás portál új lapon.

    ![Indítsa el az előre konfigurált hello megoldás][img-launch-solution]

1. Alapértelmezés szerint hello megoldás portal mutatja hello *irányítópult*. toonavigate tooother területek hello portál hello menü hello bal oldalán található hello használata.

    ![Az előre konfigurált csatlakoztatott gyár megoldás irányítópultja][cf-img-menu]

hello irányítópult a következő információ hello jeleníti meg:

* A **gyári lista** hello állapot, a helyet és a jelenlegi üzemi konfiguráció hello megoldás megjelenítő panelen. Hello megoldás első futtatásakor számos szimulált eszköz. hello termelési sor szimuláció állnak három valós OPC EE kiszolgálók éles soronként, amelyek szimulált feladatokat végeznek megoszthatják az adatokat. OPC EE kapcsolatos további információkért lásd: hello [gyári gyakran ismételt kérdések csatlakoztatott](iot-suite-faq-cf.md).
* A **térkép** , hogy minden eszköz megjeleníti hello helye csatlakoztatva toohello megoldás. hello megoldás hello a Bing térképek API tooplot információt használhatja hello térképen. Ha az előfizetéshez engedélyezve van a Bing Maps Enterprise API használata, a rendszer automatikusan ezt használja. Ha nem, olvassa el a hello [gyakran ismételt kérdések] [ lnk-faq] toolearn hogyan toomake hello térkép dinamikus.
* Egy **Alerts** (Riasztások) panelt, amely riasztásokat jelenít meg, ha egy telemetria- vagy OEE/KPI-érték meghalad egy adott küszöbértéket.
* Egy **berendezések hatékonyságot** panel, amely azt mutatja be a teljes vállalati hello vagy hello gyári/éles hello OEE értékeit sor/állomás, a rendszer azért jelenítette. Ez az érték a hello állomás nézet toohello vállalati szinten összesíti. hello OEE. ábra és a bennük foglalt elemek további elemzése.
* **A fő teljesítménymutatók** panel, amely előállított egységek és a teljes vállalati hello vagy hello gyári/termelési sor által használt energia hello számát mutatja, /, állomás tekinti meg. Ezek az értékek egy állomás nézet toohello vállalati szint összesítése.

## <a name="view-factories"></a>Üzemek megtekintése

Hello *előállítók* panel mutat be, akkor hello összes hello előállítók hello megoldás, az állapot és az üzemi aktuális konfigurációs földrajzi elhelyezkedését. Hello helyek listából navigálhat toohello hello megoldás hierarchiában lévő többi szinten. hello hello lista sorai hivatkozások, amelyek az adott helyhez hello éles sorok részleteit. Akkor majd lehetséges toodrill hello termelési sor részleteinek és lefelé toohello állomás szint nézetében. Szűrő toohello listáját is alkalmazhat.

![Előre konfigurált csatlakoztatott gyár megoldás – gyárak][cf-img-factories] 

1. Hello **gyári panel** mutat be hello gyári lista ebben a megoldásban.

2. hello gyári kezdetben listája hat előállítók hozta létre hello létesítésének folyamatát kell használnia. Hozzáadhat további szimulált és a fizikai eszközök toohello megoldás.

3. a gyári tooview hello részleteit kattintson hello sorra hello gyári listában.

4. egy éles sor tooview hello részleteinek hello lista hello sorára kattintson.

5. tooview hello közzétett hello éles sor állomás OPC EE csomópontjára, kattintson a hello listában hello sort.

6. tooview részletekért hello állomás, az adott csomóponton hello lista hello sorára kattintson. Ez a művelet elindítása hello környezetben panel idő adatsorozat Insights megjelenítésekkel. Kattintson a diagramok toodo további elemzés hello idő adatsorozat Insights explorer környezetben.

## <a name="view-map"></a>Térkép megtekintése

Ha az előfizetés a Bing térképek API access toohello, hello *előállítók* térkép meg hello földrajzi hely és állapotát jeleníti meg az összes hello előállítók hello megoldásban. hello hely részleteinek, toodrill hello térképen hello helyek elemre.

![Előre konfigurált csatlakoztatott gyár megoldás – térkép][cf-img-map]

## <a name="view-alerts"></a>Riasztások megtekintése

Hello **riasztási** panelen láthatók miatt előállított riasztások tooa jelentett értékét vagy számított OEE/KPI értéke meghaladja a beállított küszöbértéket. A panel hello felvételekor hello állomás szint nézetében toohello globális nézet egyes szintjein riasztásokat jeleníti meg. hello riasztások hello riasztást, dátum, idő, helyét és előfordulási leírását tartalmazza. Akkor értékes következtetéseket vonhat hello idő adatsorozat Insights adatainak használatával hello riasztást generáló toohello adatokban. hello idő adatsorozat Insights adatok formájában jelenik meg a hello riasztásokat, ha alkalmazható. Ha Ön rendszergazda, alapértelmezett műveletek hajthatók végre hello riasztások többek között:

* Bezárás hello riasztás.
* Tudomásul hello riasztás.

Igény szerint összetettebb műveleteket is végrehajthat. Például a hello nyomás OPC EE munkaterület hello szerelvény, a következőket teheti:

* Támogatási információkat jeleníthet meg egy weboldalon egy új böngészőablakban.
* Enyhíteni hello riasztás hello okait hello eszközön egy OPC EE metódus meghívásával.
* Ne jelenjen meg többé hello alapértelmezett műveletek hello rendelkezésre állását.

    ![Előre konfigurált csatlakoztatott gyár megoldás – riasztások][cf-img-alerts]

> [!NOTE]
> Ezek a riasztások akkor jönnek létre által előre konfigurált hello megoldásban a konfigurációs fájlban megadott feltételeket. Ezek a szabályok riasztást generál, ha hello OEE vagy KPI számokat vagy OPC EE csomópont érték túllépte a beállított küszöbértéket.

1. Hello **riasztások panel** mutat be hello figyelmeztetéseket, ebben a megoldásban.

2. tooview hello részleteit a riasztást, kattintson a riasztások panelen hello hello riasztásra.

3. toofurther hello riasztási adatok elemzésére, kattintson a hello graph hello riasztási panel tooopen hello idő adatsorozat Insights explorer környezetben.

4. tooaddress hello riasztást, és számos műveletet hello riasztási panelen elérhető. Válassza ki a hello megfelelő beállítást, és kattintson a hello Akciógomb parancs végrehajtása.

## <a name="view-overall-equipment-efficiency"></a>A teljes eszközhatékonyság megtekintése

OEE díjszabás hello hatékonyságát hello gyártási folyamat egy kulcs éles kapcsolatos működési paraméterek használatával. OEE az iparági szabványos mérték szorzásával hello rendelkezésre állási sebessége, teljesítmény sebessége és minőségének: OEE = x x minőségi rendelkezésre állását.

![Előre konfigurált csatlakoztatott gyár megoldás – OEE][cf-img-oee]

1. tooview OEE bármely szint hello hierarchiában, keresse meg a toohello specifikus nézet van szüksége. hello panel hello OEE százalékos alkotó hello elemmel együtt hello OEE, ha a nézet jeleníti meg.

2. toofurther hello OEE bármely szint hello hierarchia adatok elemzése, kattintson hello OEE, rendelkezésre állási, teljesítmény vagy a minőségi százalékos aránya. A környezet panel jelenik meg, amelyen idő adatsorozat Insights energiaforrással rendelkező képi megjelenítéseket, amelyek az elmúlt egy óra utolsó 24 óra és az elmúlt 7 napban hello adatainak megjelenítése.

    ![Előre konfigurált csatlakoztatott gyár megoldás – TSI-vizualizáció][cf-img-tsi-visualization]

3. toofurther hello riasztási adatok elemzésére, hello graph hello riasztási panelen kattintson. Ez a művelet megnyitja hello idő adatsorozat Insights explorer környezetben.

    ![Előre konfigurált csatlakoztatott gyár megoldás – TSI Explorer][cf-img-tsi-explorer]

## <a name="view-key-performance-indicators"></a>Fő teljesítménymutatók megtekintése

hello megoldás két fő teljesítménymutatók, biztosít *óránként egységek* és *kWh-ban használt energia*.

![Előre konfigurált csatlakoztatott gyár megoldás – KPI][cf-img-kpi]

1. tooview egység / óra vagy energiát minden szint hello hierarchiában, keresse meg a toohello specifikus nézet van szüksége. hello egység / óra és energia hello panelen megjelenített használt.

2. tooanalyze egység / óra vagy energiát minden szint hello hierarchia további, kattintson a hello hello mérőműszer **a fő teljesítménymutatók** panel. A környezet panel hello utolsó óra utolsó 24 órában, és a legutóbbi 7 nap hello tooview adatok így már kapcsolva idő adatsorozat Insights megjelenítésekkel jelenik meg.

## <a name="scenario-review"></a>Forgatókönyv áttekintése

Ebben a forgatókönyvben a előállítók OEE és a KPI-k, szereplő értékek hello irányítópult figyeli. Majd használt idő adatsorozat Insights tooprovide további információt toohelp részletek további hello telemetriai adatokat a rendellenességeinek észlelésekor OEE és a KPI-k toohelp. A előállítók hello riasztási panel tooview problémákat is használt, és hello műveletek elérhető tooyou tooresolve hello riasztás használta.

## <a name="other-features"></a>Egyéb jellemzők

a következő szakaszok hello csatlakoztatott hello gyári megoldás néhány további szolgáltatásokat, nem ismerteti a fentebbi szituáció hello ismertetik.

## <a name="apply-filters"></a>Szűrők alkalmazása

1. Kattintson a hello **sávnyíl** toodisplay vagy hello gyári helyek vagy hello riasztások panelen rendelkezésre álló szűrők listája.

2. az Ön hello szűrők panel jelenik meg. 

    ![Előre konfigurált csatlakoztatott gyár megoldás – szűrők][cf-img-alert-filter]

3. Válassza ki a szükséges hello szűrő. Akkor is lehetséges tootype szabad szöveg hello szűrő mezőkbe.

4. hello majd szűrőhöz meg. hello irányítópulton keresztül a tölcsér hello előállítók jeleníti meg, és riasztást küld a táblák hello szűrő állapota is látható.

    ![Előre konfigurált csatlakoztatott gyár megoldás – szűrők][cf-img-alert-filter-funnel]

    > [!NOTE]
    > Aktív szűrő nem befolyásolja a megjelenő hello OEE és KPI értékek, csak szűri hello tartalmának listázása.

5. tooclear szűrőt, kattintson a hello tölcsér, és kattintson a szűrő hello szűrő környezetben panelen. szöveg hello **összes** hello előállítók jelenik meg, és riasztást küld a táblákat.

## <a name="browse-an-opc-ua-server"></a>OPC UA-kiszolgáló tallózása

Előre konfigurált hello megoldás telepítésekor automatikusan létesítsen keresztül hello megoldás böngésző tallózással szimulált OPC EE-kiszolgálók. Ezek a kiszolgálók *szimulált OPC UA-kiszolgálók*. Szimulált kiszolgálók könnyítheti meg tooexperiment előre konfigurált hello megoldással hello kell toodeploy valódi, fizikai kiszolgálók nélkül. Ha szeretné, hogy valós OPC EE-kiszolgálói toohello megoldás tooconnect, tekintse meg a hello [kapcsolni a OPC EE eszköz csatlakoztatva toohello előre konfigurált gyári megoldást] [ lnk-connect-cf] oktatóanyag.

1. Kattintson a hello **gyári ikon** hello irányítópult navigációs sávban.

    ![Előre konfigurált csatlakoztatott gyár megoldás – kiszolgálótallózó][cf-img-server-browser]

2. Hello előre konfigurált listából válasszon hello kiszolgálók közül. A lista mutatja meg az előre konfigurált hello megoldás telepített hello kiszolgálókat.

    ![Előre konfigurált csatlakoztatott gyár megoldás – kiválasztott kiszolgálók][cf-img-server-choice]

3. Kattintson a **Connect** (Csatlakozás) gombra. Megjelenik egy biztonsági párbeszédablak. Hello szimuláció, biztonságos tooclick **folytatása**

4. tooexpand bármelyik hello csomópontok hello kiszolgáló csomópontjára, kattintson rá. A telemetriaadatokat közzétevő csomópontokat egy pipa jelöli.

    ![Előre konfigurált csatlakoztatott gyár megoldás – kiszolgálófa][cf-img-server-tree]

5. Kattintson a jobb gombbal egy cikk tooread, írása, közzététele és hívható meg, hogy a csomópont. hello műveletek elérhető tooyou az engedélyeit, és hello attribútumok hello csomópont függ. hello olvasható beállítás toodisplays egy adott csomópont hello hello értéket megjelenítő környezetben panel. hello írható beállítást jeleníti meg a környezetben panel, ahol új értéket adhat meg. hello hívás lehetőséget választja, megnyílik egy csomópont be hello paraméterek hello hívásához.

## <a name="publish-a-node"></a>Csomópont közzététele

Ha megnyitja egy *szimulált OPC EE-kiszolgáló*, dönthet úgy is toopublish új csomópontok. Ezek a csomópontok hello megoldásban a hello telemetriai elemezheti. Ezek *OPC EE-kiszolgálók szimulált* könnyebben előre konfigurált hello megoldással egyszerűen tooexperiment valódi, fizikai eszközök telepítése nélkül.

1. Keresse meg, hogy kívánja-e toopublish OPC EE böngésző fa hello tooa csomópontja.

2. Kattintson a jobb gombbal hello csomópont.

3. Válassza a **Publish** (Közzététel) lehetőséget.

    ![A csatlakoztatott gyár közzétesz egy csomópontot][cf-img-publish-node]

4. A környezet panel jelenik meg, amely közli, hogy hello közzététele sikeresen befejeződött. hello csomópont hello állomás szint nézetében mellette jelölve jelenik meg.

    ![Előre konfigurált csatlakoztatott gyár megoldás – sikeres közzététel][cf-img-publish-success]

## <a name="command-and-control"></a>Parancs és vezérlés

hello csatlakoztatott gyári lehetővé teszi a parancsot, és az iparág eszközök közvetlenül a hello felhőből szabályozzák. Ez a szolgáltatás toorespond tooalerts hello eszköz állítja elő is használhatja. Például egy parancs toohello eszköz sikerült elküldeni a hello felhőből. Hello elérhető parancsok az található hello **StationCommands** hello OPC EE kiszolgálók böngésző fa csomópontja. Ebben a forgatókönyvben egy éles sor München hello szerelvény állomáson nyomás kiadás szelep nyissa meg. toouse hello parancs funkciói, kell lennie a hello **rendszergazda** hello szerepkör előre konfigurált megoldás üzembe helyezése.

1. Keresse meg a toohello **StationCommands** hello OPC EE böngésző fa csomópontja.

2. Válassza ki, hogy kívánja-e használatban hello parancsot.

3. Kattintson a jobb gombbal hello csomópont.

4. Válassza a **Call** (Hívás) parancsot.

    ![Előre konfigurált csatlakoztatott gyár megoldás – hívás parancs][cf-img-call-command]

5. A környezet panel jelenik meg, tájékoztat módszer toocall, és minden paraméter adatát kell alkalmazni.

6. Válassza a **Call** (Hívás) parancsot.

    ![Előre konfigurált csatlakoztatott gyár megoldás – hívás helyi panelje][cf-img-call-context]

7. hello környezetben panel egy frissített tooinform, hogy a metódus hívása hello sikeres volt. Hello hívás sikeres hello nyomás csomópont hello hívása miatt frissített hello érték beolvasásával ellenőrizheti.

    ![Előre konfigurált csatlakoztatott gyár megoldás – sikeres hívás][cf-img-call-success]


## <a name="behind-hello-scenes"></a>Hello háttérben

Amikor telepít egy előre beállított megoldás, hello központi telepítési folyamat több erőforrást hello kijelölt Azure-előfizetés hoz létre. Megtekintheti ezeket az erőforrásokat a hello Azure [portal][lnk-portal]. hello központi telepítési folyamat létrehoz egy **erőforráscsoport** előkonfigurált megoldást választja hello neve alapján néven:

![Hello Azure-portálon az előkonfigurált megoldás][img-cf-portal]

Megtekintheti a hello-beállítások az egyes erőforrások hello az erőforrások listájához a hello erőforráscsoport lehetőséget.

Előre konfigurált hello megoldás forráskódját hello is megtekintheti. hello előre konfigurált csatlakoztatott gyári megoldás forráskódját van hello [azure iot-csatlakoztatott-gyári] [ lnk-cfgithub] GitHub-tárházban:

Amikor elkészült, hogy törli előre konfigurált hello megoldás az Azure-előfizetéshez hello [azureiotsuite.com] [ lnk-azureiotsuite] hely. Ez a hely összes hello előre konfigurált hello megoldás létrehozása után kiépített erőforrások tooeasily törlése lehetővé teszi.

> [!NOTE]
> tooensure törlése minden kapcsolódó előre konfigurált toohello megoldás, törölje a hello [azureiotsuite.com] [ lnk-azureiotsuite] hely. Ne törölje az erőforráscsoportot hello hello portálon.

## <a name="next-steps"></a>Következő lépések

Most, hogy egy előre konfigurált működő megoldást telepítése után, továbbra is a következő cikkek hello olvasásával Ismerkedés az IoT Suite:

* [Előre konfigurált csatlakoztatott gyár megoldás – bemutató][lnk-rm-walkthrough]
* [Csatlakozás a toohello előre konfigurált csatlakoztatott gyári megoldás][lnk-connect-cf]
* [Engedélyek hello azureiotsuite.com webhelyen][lnk-permissions]

[img-cf-home]:media/iot-suite-connected-factory-overview/cf-dashboard.png
[img-launch-solution]: media/iot-suite-connected-factory-overview/launch-cf.png
[cf-img-menu]: media/iot-suite-connected-factory-overview/cf-dashboard-menu.png
[cf-img-factories]:media/iot-suite-connected-factory-overview/cf-dashboard-factory.png
[cf-img-map]:media/iot-suite-connected-factory-overview/cf-dashboard-map.png
[cf-img-alerts]:media/iot-suite-connected-factory-overview/cf-dashboard-alerts.png
[cf-img-oee]:media/iot-suite-connected-factory-overview/cf-dashboard-oee.png
[cf-img-kpi]:media/iot-suite-connected-factory-overview/cf-dashboard-kpi.png
[cf-img-tsi-visualization]:media/iot-suite-connected-factory-overview/cf-dashboard-tsi.png
[cf-img-tsi-explorer]:media/iot-suite-connected-factory-overview/tsi-explorer.png
[cf-img-server-browser]: media/iot-suite-connected-factory-overview/cf-dashboard-browser.png
[cf-img-server-choice]: media/iot-suite-connected-factory-overview/cf-select-server.png
[cf-img-server-tree]:media/iot-suite-connected-factory-overview/cf-server-tree.png
[cf-img-publish-node]:media/iot-suite-connected-factory-overview/cf-publish-node1.png
[cf-img-publish-success]:media/iot-suite-connected-factory-overview/cf-publish-success.png
[cf-img-call-command]:media/iot-suite-connected-factory-overview/cf-command-and-control-call.png
[cf-img-call-context]:media/iot-suite-connected-factory-overview/cf-command-and-control-call-button.png
[cf-img-call-success]:media/iot-suite-connected-factory-overview/cf-command-and-control-succeed.png
[img-cf-portal]:media/iot-suite-connected-factory-overview/cf-resource-group.png
[cf-img-alert-filter]:media/iot-suite-connected-factory-overview/cf-filter.png
[cf-img-alert-filter-funnel]:media/iot-suite-connected-factory-overview/cf-filter-funnel.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-portal]: http://portal.azure.com/
[lnk-cfgithub]: https://github.com/Azure/azure-iot-connected-factory
[lnk-rm-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[lnk-connect-cf]: iot-suite-connected-factory-gateway-deployment.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-faq]: iot-suite-faq.md