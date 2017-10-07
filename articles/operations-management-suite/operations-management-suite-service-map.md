---
title: "az Operations Management Suite Szolgáltatástérkép megoldás aaaUse hello |} Microsoft Docs"
description: "Szolgáltatástérkép Operations Management Suite megoldás, amely automatikusan felderíti az alkalmazás-összetevők a Windows és Linux rendszerek és a maps hello szolgáltatások közötti kommunikáció. Ez a cikk a Service Map telepítése a környezetben, és használja azt a különféle forgatókönyvekhez, amik részletesen."
services: operations-management-suite
documentationcenter: 
author: daveirwin1
manager: jwhit
editor: tysonn
ms.assetid: 3ceb84cc-32d7-4a7a-a916-8858ef70c0bd
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/22/2016
ms.author: daseidma;bwren;dairwin
ms.openlocfilehash: f7c209182c9171cc520192ac13ca4d85174081b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-service-map-solution-in-operations-management-suite"></a>Az Operations Management Suite hello Szolgáltatástérkép megoldás használja
Szolgáltatástérkép automatikusan észleli a Windows és Linux rendszerek alkalmazás-összetevők és a maps hello szolgáltatások közötti kommunikáció. Szolgáltatástérkép, használatával megtekintheti a kiszolgálók, amelyek Ön szerint egyik hello módon: összekapcsolt rendszerekhez, hogy a kritikus szolgáltatásokhoz. Szolgáltatástérkép jeleníti meg a kiszolgálók, folyamatok és portok közötti kapcsolatok között bármely TCP-csatlakoztatott architektúra az eltérő szükség konfigurálásra hello ügynököt telepíteni.

Ez a cikk ismerteti a Szolgáltatástérkép használatával hello részleteit. Szolgáltatástérkép és bevezetési ügynökök konfigurálásával kapcsolatos további információkért lásd: [Operations Management Suite megoldás konfigurálása a Service Map](operations-management-suite-service-map-configure.md).


## <a name="use-cases-make-your-it-processes-dependency-aware"></a>Eseteinek: Ellenőrizze az informatikai feldolgozza a fürttámogató függőség

### <a name="discovery"></a>Felderítés
Szolgáltatástérkép függőségek közös hivatkozás térképet automatikusan létrehozza a kiszolgálón, a folyamatok és a harmadik féltől származó szolgáltatással. Észleli, és hozzárendeli az összes TCP-függőség, jelzés nélküli kapcsolatokat, a távoli külső rendszerek függ, és a függőségek tootraditional sötét területet a hálózaton, például az Active Directory azonosító. Szolgáltatástérkép deríti fel, hogy a felügyeleti rendszer próbált toomake, hibás hálózati kapcsolatok gondoskodik a potenciális server hibás konfigurációja, a szolgáltatáskimaradás és a hálózati problémák azonosításához.

### <a name="incident-management"></a>Incidenskezelés
Szolgáltatástérkép segít, hogy bemutatja, hogyan kapcsolódnak a rendszerek, és egymást érintő hello munka bizonytalanságát probléma elkülönítési megszüntetéséhez. Ezenkívül tooidentifying sikertelen kapcsolatok, hanem a helytelenül konfigurált terheléselosztók, kritikus szolgáltatások meglepő vagy túlzott terhelése azonosítása és támadó ügyfelek, például a fejlesztői gépek tooproduction rendszerek van szó. Az Operations Management Suite módosítása követési integrált munkafolyamatok használatával is megtekinthető e egy háttér-gép vagy szolgáltatás-megváltoztatási esemény ismerteti az incidensek alapvető oka hello.

### <a name="migration-assurance"></a>Áttelepítési megbízhatósági
Szolgáltatástérkép használatával hatékonyan megtervezése programot, egyre gyorsabban jelennek meg, és a segítségével győződjön meg arról, hogy semmi sem hátrahagyott és jelzés nélküli nem kiesések Azure áttelepítések ellenőrzése. Felderítése rendszerek minden egymástól kölcsönösen függnek, hogy szükség toomigrate együtt, mérje fel a Rendszerkonfiguráció és a kapacitás, és adja meg, hogy a futó rendszer van még kiszolgáló felhasználók vagy jelöltségét ellenőrző az áttelepítési helyett leszerelése. Hello lépés befejezése után ellenőrizheti, hogy az ügyfél a terhelési és identitás tooverify, amely tesztrendszerek, és az ügyfelek kapcsolódik. Az alhálózat tervezési és a tűzfal-meghatározások problémák vannak, ha a Service Map maps sikertelen kapcsolatok pontját kapcsolatot igénylő rendszerek toohello.

### <a name="business-continuity"></a>Az üzletmenet folytonossága
Ha Azure Site Recovery használ, és kell hello helyreállítási feladatütemezési alkalmazás környezetre Szolgáltatástérkép meghatározása súgó automatikusan láthatja, hogyan rendszerek támaszkodnak egymással, hogy megbízható-e a helyreállítási terv tooensure. Egy kritikus fontosságú kiszolgáló vagy csoport kiválasztása, és tekintse meg az ügyfeleknek, azonosíthatja melyik előtér-rendszerek toorecover után hello kiszolgáló visszaállítása, és elérhető. Ezzel szemben kritikus fontosságú kiszolgálók háttér-függőségek megtekintésével azonosíthatja, mely rendszerek toorecover a fókusz rendszerek visszaállítása előtt.

### <a name="patch-management"></a>A javítások
Szolgáltatástérkép hello Operations Management Suite rendszer frissítések értékelését használatát fokozza a jelenít meg, amelyek egyéb csoportok és a kiszolgálók a szolgáltatástól függenek, így akkor is értesítést küldhet nekik előre le a rendszer a javítás végrehajtása előtt. Szolgáltatástérkép is javítja a javítás-kezelés az Operations Management Suite azáltal, hogy bemutatja, hogy a szolgáltatások elérhető és megfelelően csatlakoztatott után lett, és újraindul.


## <a name="mapping-overview"></a>Leképezési áttekintése
Szolgáltatástérkép ügynökök hello kiszolgálón, ahol azok van telepítve, és részleteit hello bejövő és kimenő kapcsolatok az egyes folyamatokhoz tartozó összes TCP-csatlakoztatott folyamatokról adatainak összegyűjtése. Hello hello bal oldali ablaktábla listájában kiválaszthatja a gépeket vagy csoportok Szolgáltatástérkép ügynökök toovisualize függősége a megadott időtartomány keresztül. Gép függőségi képezi le egy adott gép helyezi a hangsúlyt, és azok megjelenítése, hello gépeire közvetlen TCP-ügyfelek vagy kiszolgálók gép.  Gép maps kiszolgálók és a Függőségek megjelenítése.

![Szolgáltatástérkép áttekintése](media/oms-service-map/service-map-overview.png)

Gépek hello térkép tooshow hello futó folyamatok aktív hálózati kapcsolatokkal hello kijelölt időtartományban bővíthető. Bővített tooshow folyamat részletei a Szolgáltatástérkép ügynökkel távoli számítógép esetén csak a hello fókusz gép kommunikáló folyamatok jelennek meg. ügynök nélküli előtér-gépek hello fókusz géppé csatlakozó hello száma fel van tüntetve hello bal oldalán található hello folyamatok való csatlakozáshoz. Ha hello fókusz gépen van egy kapcsolat tooa háttér-számítógép, amelyen nincs ügynök, hello háttér-kiszolgáló egy olyan kiszolgálócsoportot Port szerepel, és egyéb kapcsolatok toohello azonos port száma.

Alapértelmezés szerint a Service Map maps megjelenítése hello függőségi adatokat az elmúlt 30 percben. Hello bal felső hello használható vezérlők használatával kérdezheti le maps korábbi idő porttartományok tooone óra tooshow hogyan függőségek kikeresi az hello be (például incidens vagy előtt változott meg valami) múltbeli. Szolgáltatástérkép tárolja a fizetős munkaterületek 30 napig, és a munkaterületek 7 napban.

## <a name="status-badges-and-border-coloring"></a>Állapot jelvények és szegély színezés
Az egyes kiszolgálók hello leképezés alsó hello lehet hello server állapotinformációról igényinek állapot jelvények listáját. hello jelvények jelzi, hogy néhány információt hello kiszolgáló hello Operations Management Suite megoldás Integrációk egyikéből. Egy jelvény kattintva viszi közvetlenül hello állapot részleteit toohello hello jobb oldali ablaktáblán. hello jelenleg elérhető állapot jelvények tartalmaz riasztások, a ügyfélszolgálatához, a módosítások, a biztonsági és a frissítések.

Attól függően, hogy hello állapot jelvények hello súlyossága gép csomópont szegélyek színes piros (kritikus), sárga (figyelmeztetés), vagy kék (tájékoztató). hello szín hello legsúlyosabb károkat okozó állapotát hello állapot jelvények bármelyikének mutatja be. Szürke szegélyt megadni a csomópont, amelynek nincs Állapotjelzők jelzi.

![Állapot jelvények](media/oms-service-map/status-badges.png)

## <a name="machine-groups"></a>Számítógép-csoportok
Gép csoportok lehetővé teszik, hogy Ön toosee maps része a több kiszolgáló, nem csak egy hívjuk fel egy leképezést egy többrétegű alkalmazást vagy a kiszolgáló fürt összes hello tagja.

Felhasználók válassza ki, melyik kiszolgálókat együtt egy csoporthoz tartozik, és válassza a hello csoport nevét.  Ezután válassza ki a tooview hello csoportban található összes folyamatok és kapcsolatok, vagy megtekintheti csak hello folyamatok és kapcsolatok toohello közvetlenül kapcsolódó egyéb hello csoport tagjai.

![Számítógép-csoport](media/oms-service-map/machine-group.png)

### <a name="creating-a-machine-group"></a>Számítógép-csoport létrehozása
toocreate egy csoportot, válassza hello gép vagy gépek kívánt hello gépeken listán, és kattintson **toogroup hozzáadása**.

![Csoport létrehozása](media/oms-service-map/machine-groups-create.png)

Itt választhat **hozzon létre új** , és nevezze el hello csoport.

![Csoport neve](media/oms-service-map/machine-groups-name.png)

>[!NOTE]
>Gép csoportok jelenleg korlátozott too10 kiszolgálók, de tervezzük tooincrease ezt a határt hamarosan.

### <a name="viewing-a-group"></a>Egy csoport megtekintése
Néhány csoport létrehozása után is megtekintheti őket hello csoportok fülre.

![Csoportok lap](media/oms-service-map/machine-groups-tab.png)

Ezután válassza ki a számítógép csoport hello neve tooview hello hozzárendelése.
![Gép](media/oms-service-map/machine-group.png) hello gépek toohello csoporthoz tartozó eljárásokat a fehér hello a térképen.

Növekvő hello csoport hello gép csoportot alkotó hello gépekhez felsorolja.

![Gép gépek](media/oms-service-map/machine-groups-machines.png)

### <a name="filter-by-processes"></a>Folyamatok szűrése
Hello térképnézet folyamatok és a kapcsolatokat megjelenítő hello csoport közötti váltás, és csak hello azokon, közvetlenül a gép toohello vonatkoznak.  az alapértelmezett nézet hello tooshow az összes folyamat.  Hello nézet hello térkép felett hello szűrő ikonra kattintva módosíthatja.

![Szűrő csoport](media/oms-service-map/machine-groups-filter.png)

Ha **minden folyamat** van kijelölve, hello térkép megjeleníti minden folyamat és kapcsolatok az egyes gépek hello abban hello csoport.

![Minden számítógép csoport dolgozza fel.](media/oms-service-map/machine-groups-all.png)

Hello nézet csak tooshow módosításakor **csoport csatlakoztatva folyamatok**, hello térkép fog maradt le tooonly ezen folyamatok és a kapcsolatokat, amelyek közvetlenül csatlakoztatott tooother gépek hello csoportjában egyszerűsített nézetek létrehozásával.

![Számítógép csoport szűrve folyamatok](media/oms-service-map/machine-groups-filtered.png)
 
### <a name="adding-machines-tooa-group"></a>Gépek tooa hozzáadása csoporthoz
tooadd gépek tooan meglévő csoporthoz, ellenőrizze a hello mezők szeretne, és kattintson a Tovább toohello gépek **toogroup hozzáadása**.  Ezt követően válassza a kívánt tooadd hello gépek hello csoport.
 
### <a name="removing-machines-from-a-group"></a>Gép eltávolítása egy csoportból
Hello csoportok listában megadott csoportokat bontsa ki a hello csoport neve toolist hello gépek hello számítógép csoportjához.  Kattintson a hello három pont menü következő toohello gépen szeretné, hogy tooremove, és válassza a **eltávolítása**.

![Gép eltávolítása a csoportból](media/oms-service-map/machine-groups-remove.png)

### <a name="removing-or-renaming-a-group"></a>Eltávolítása, vagy egy csoport átnevezése
Kattintson a hello három pont menü következő toohello csoport nevére a hello listájából.

![Számítógép csoport menü](media/oms-service-map/machine-groups-menu.png)


## <a name="role-icons"></a>Szerepkör ikon
Bizonyos folyamatok szolgálnak ki adott szerepkörök gépek: webalkalmazás-kiszolgálók, alkalmazás-kiszolgálókat, adatbázis, és így tovább. Szolgáltatástérkép jelzi a folyamat, és a gép mezők szerepkör ikonok toohelp azonosítása egy pillanat alatt hello szerepkör egy folyamat vagy a kiszolgálón betöltött szerepe.

| Szerepkör ikon | Leírás |
|:--|:--|
| ![Webkiszolgáló](media/oms-service-map/role-web-server.png) | Webkiszolgáló |
| ![Alkalmazáskiszolgáló](media/oms-service-map/role-application-server.png) | Alkalmazáskiszolgáló |
| ![Adatbázis-kiszolgáló](media/oms-service-map/role-database.png) | Adatbázis-kiszolgáló |
| ![LDAP-kiszolgáló](media/oms-service-map/role-ldap.png) | LDAP-kiszolgáló |
| ![SMB-kiszolgálón](media/oms-service-map/role-smb.png) | SMB-kiszolgálón |

![Szerepkör ikon](media/oms-service-map/role-icons.png)


## <a name="failed-connections"></a>Nem sikerült kapcsolatok
Nem sikerült kapcsolatok megjelennek-e Szolgáltatástérkép leképezi a folyamatok és a számítógépek, piros szaggatott vonal jelzi, hogy az ügyfélrendszer tooreach meghiúsul a folyamatot vagy port. Nem sikerült kapcsolatot a rendszer a telepített Service Map ügynökkel küld jelentést hello egy tett kísérlet során nem sikerült hello kapcsolat esetén is, hogy a rendszer. Szolgáltatástérkép betartásával tooestablish teljesítő TCP-szoftvercsatornák a kapcsolat a folyamat méri. Ezt a hibát okozhatja egy tűzfal, a helytelen konfiguráció hello ügyfél vagy kiszolgáló, vagy a távoli szolgáltatás nem volt elérhető.

![Nem sikerült kapcsolatok](media/oms-service-map/failed-connections.png)

Hibás kapcsolatok elősegítheti a hibaelhárítást, áttelepítési érvényesítési, biztonsági elemzés és a teljes architectural understanding ismertetése. Hibás kapcsolatok néha ártalmatlan, de gyakran pontok közvetlenül tooa probléma, például egy feladatátvételi környezetet hirtelen kiderül, hogy nem érhető el, vagy két alkalmazásrétegek nem tootalk éppen egy felhőalapú áttelepítése után.

## <a name="client-groups"></a>Az ügyfélcsoportok
Az ügyfélcsoportok hello térképen jelölőnégyzetéből, amelyek megfelelnek az ügyfél gépek, amelyeken nincs függőségi ügynökök. Egyetlen ügyfél csoport hello ügyfelek egy adott folyamat vagy a számítógép jelenti.

![Az ügyfélcsoportok](media/oms-service-map/client-groups.png)

toosee hello IP-címek hello kiszolgálók ügyfél-csoportjában válassza hello csoport. hello csoport hello tartalmát hello szereplő **ügyfél tulajdonságai** ablaktáblán.

![Ügyfél csoport tulajdonságai](media/oms-service-map/client-group-properties.png)

## <a name="server-port-groups"></a>Kiszolgálóport csoportok
Kiszolgálóport csoportok jelölőnégyzetéből, amelyek megfelelnek a kiszolgálókon, amelyeken nincs függőségi ügynökök server portok. hello mezőben hello kiszolgálóport és hello több kiszolgálót kapcsolatok toothat port számát tartalmazza. Bontsa ki a hello mezőben toosee hello egyes kiszolgálók és a kapcsolatokat. Ha csak egy kiszolgáló hello mezőben, hello nevét vagy IP-cím szerepel.

![Kiszolgálóport csoportok](media/oms-service-map/server-port-groups.png)

## <a name="context-menu"></a>Helyi menü
Hello három ponttal (…) hello tetején kattintson jobb bármely kiszolgáló hello helyi menüjére, hogy a kiszolgáló jeleníti meg.

![Nem sikerült kapcsolatok](media/oms-service-map/context-menu.png)

### <a name="load-server-map"></a>Betöltési server térkép
Kattintson a **terhelés Server térkép** tooa új leképezés hello kijelölt kiszolgálón, a hello új fókusz gép viszi.

### <a name="show-self-links"></a>Megjeleníti az önhivatkozások
Kattintson a **megjelenítése Self-Links** újrarajzolja hello server csomóponton, így azokat az önhivatkozások, amelyeket kezdődnek és zárulnak folyamatok belül hello kiszolgáló TCP-kapcsolatok. Ha erre az önhivatkozások is hello menü parancs végrehajtása túl is látható,**elrejtése Self-Links**, így kikapcsolhatja azokat.

## <a name="computer-summary"></a>A számítógép összefoglaló
Hello **gép összegzés** ablaktáblán egy kiszolgáló operációs rendszer, függőség számát és az Operations Management Suite megoldásait adatait áttekintését tartalmazza. Ezen adatok közé tartoznak a teljesítménymutatók, szolgáltatásjegyek ügyfélszolgálati, változások követését, biztonsági és frissítéseket.

![Gép összefoglalás ablaktábla](media/oms-service-map/machine-summary.png)

## <a name="computer-and-process-properties"></a>Számítógép és a folyamat tulajdonságai
Amikor a Service Map térkép, kiválaszthatja a gépek és a folyamatok toogain további környezet adnak a tulajdonságaikról. Gépek DNS-neve, IPv4 címek, CPU és memória kapacitás, virtuális gép típusa, operációs rendszer és verzió, a időt és az Operations Management Suite és a Service Map ügynökök hello azonosítók újraindítás utolsó kapcsolatos adatok megadása.

![Számítógép-tulajdonságok panelen](media/oms-service-map/machine-properties.png)

Folyamat részletes adatainak összegyűjtése futó folyamatok, beleértve a folyamat neve, folyamat leírása, felhasználónév és (Windowson) tartomány, vállalat neve, termék neve, termék verziója, munkakönyvtár, parancssori és folyamat metaadatainak operációs rendszer kezdési időpontja.

![Folyamat tulajdonságok panelen](media/oms-service-map/process-properties.png)

Hello **folyamat összegzése** ablaktábla hello folyamat kapcsolatot, beleértve annak kötött portok, a bejövő és kimenő kapcsolatok további információkkal szolgál, és a kapcsolatok nem sikerült.

![Folyamat összefoglalás ablaktábla](media/oms-service-map/process-summary.png)

## <a name="operations-management-suite-alerts-integration"></a>Az Operations Management Suite riasztások integráció
Szolgáltatástérkép integrálódik az Operations Management Suite riasztások indította tooshow riasztások hello kijelölt kiszolgálón kiválasztott hello időtartományba esik. hello server megjelenít egy ikont, ha nincsenek az aktuális riasztások és hello **gép riasztások** ablaktábla listázza a hello riasztásokat.

![Gép riasztások panelen](media/oms-service-map/machine-alerts.png)

tooenable Szolgáltatástérkép toodisplay vonatkozó riasztásokat, amely egy adott számítógép következik be riasztási szabályt létrehozni. toocreate megfelelő riasztások:
- Lehet például egy záradék toogroup számítógép (például **számítógép időköze 1 perces**).
- Válassza ki a tooalert metrika mérési alapján.

![Riasztási konfigurációja](media/oms-service-map/alert-configuration.png)


## <a name="operations-management-suite-log-events-integration"></a>Operations Management Suite naplózási események integráció
Szolgáltatástérkép integrálható a keresési napló tooshow hello kiválasztott kiszolgálóhoz tartozó összes elérhető napló események számát hello kijelölt időtartományban. Esemény száma toojump tooLog keresési hello lista bármely sorára kattintson, és hello egyedi aktiválási naplót eseményeket.

![Gép naplóeseményeket ablaktábla](media/oms-service-map/log-events.png)

## <a name="operations-management-suite-service-desk-integration"></a>Az Operations Management Suite szolgáltatás ügyfélszolgálati integráció
Szolgáltatástérkép integrációja hello IT Service Management-összekötő akkor automatikus, ha a két megoldás engedélyezve és konfigurálva az Operations Management Suite-munkaterülettel. a Service Map hello integrációs lett címkézve "Ügyfélszolgálatához." További információkért lásd: [ITSM munkaelemek IT Service Management-összekötő segítségével központilag kezelheti](https://docs.microsoft.com/azure/log-analytics/log-analytics-itsmc-overview).

Hello **gép ügyfélszolgálatához** ablaktábla listázza a kiválasztott hello időtartomány hello a kiválasztott kiszolgálóhoz tartozó összes informatikai szolgáltatások kezelésében eseményt. hello server megjelenít egy ikont, ha nincsenek aktuális elemek és hello gép ügyfélszolgálatához ablaktábla listázza azokat.

![Gép ügyfélszolgálatához ablaktábla](media/oms-service-map/service-desk.png)

Kattintson a csatlakoztatott ITSM megoldásban tooopen hello elem **nézet munkaelem**.

hello elem a napló keresési tooview hello részletekért kattintson **jelenjen meg a keresési napló**.


## <a name="operations-management-suite-change-tracking-integration"></a>Az Operations Management Suite változások követése integráció
Szolgáltatástérkép integráció a változások követése akkor automatikus, ha a két megoldás engedélyezve és konfigurálva az Operations Management Suite-munkaterülettel.

Hello **gép változások követése** ablaktábla listázza a összes módosítás, a legutóbbi hello először, valamint egy hivatkozást toodrill le tooLog keresse meg a további részletek.

![Gép változások követése ablaktábla](media/oms-service-map/change-tracking.png)

hello példánycsoportokat, amelyeket konfigurációváltozás esemény részletes nézet kijelölése után **megjelenítése a Naplóelemzési**.

![Konfigurációváltozás esemény](media/oms-service-map/configuration-change-event.png)


## <a name="operations-management-suite-performance-integration"></a>Az Operations Management Suite teljesítmény-integráció
Hello **gépek teljesítménye** ablaktáblán jelennek meg a standard teljesítménymutatók hello kijelölt kiszolgálóra vonatkozóan. hello mérőszámok közé tartozik a CPU kihasználtsága, memória-felhasználás, küldött és fogadott hálózati bájtok és hello felső folyamatok listáját által küldött és fogadott hálózati bájtok. tooget hello hálózati teljesítményadatokat kell is engedélyezte hello Operations Management Suite átviteli adatok 2.0 megoldás.

![Gép teljesítmény ablaktábla](media/oms-service-map/machine-performance.png)


## <a name="operations-management-suite-security-integration"></a>Az Operations Management Suite biztonsági integráció
Biztonsági és a naplózási szolgáltatás térkép integrációját akkor automatikus, ha a két megoldás engedélyezve és konfigurálva az Operations Management Suite-munkaterülettel.

Hello **gép biztonsági** ablaktábla megjeleníti azokat a hello Operations Management Suite biztonsági és hitelesítési megoldás hello kijelölt kiszolgáló adatait. hello ablaktábla listázza a függőben lévő biztonsági problémák hello kiszolgáló összegzését hello kijelölt időtartományban. Kattintson bármelyik hello biztonsági problémák csukja le a velük kapcsolatos részletek napló keresése.

![Számítógép biztonsági ablaktábla](media/oms-service-map/machine-security.png)


## <a name="operations-management-suite-updates-integration"></a>Az Operations Management Suite frissítések integráció
Szolgáltatástérkép integráció a frissítéskezelés akkor automatikus, ha a két megoldás engedélyezve és konfigurálva az Operations Management Suite-munkaterülettel.

Hello **Machine frissítések** ablaktábla hello Operations Management Suite frissítés felügyeleti megoldás hello kijelölt kiszolgáló adatait jeleníti meg. hello ablaktábla listázza a hiányzó frissítésekkel hello kiszolgáló összegzését hello kijelölt időtartományban.

![Gép változások követése ablaktábla](media/oms-service-map/machine-updates.png)

## <a name="log-analytics-records"></a>Log Analytics-rekordok
Szolgáltatástérkép számítógép és a folyamat leltáradatok érhető el [keresési](../log-analytics/log-analytics-log-searches.md) a Naplóelemzési. Az adatok tooscenarios, amely tartalmazza az áttelepítés megtervezése, kapacitáselemzési, felderítési és igény szerinti teljesítmény hibaelhárítási is alkalmazhatja.

Külön rekordot minden egyedi számítógép és a folyamat óránként jön létre, továbbá toohello rögzíti, amelyek egy folyamat vagy a számítógép indításakor vagy előre telepített tooService hozzárendelését. Ezeket a rekordokat a következő táblák hello hello jellemzőkkel rendelkezik. hello mezők és értékek hello ServiceMapComputer_CL események képezze le a gép erőforrás hello ServiceMap Azure Resource Manager API hello toofields. hello mezők és értékek hello ServiceMapProcess_CL események leképezése hello hello ServiceMap Azure Resource Manager API folyamat erőforrás toohello mezőit. hello ResourceName_s mező hello megfelelő Resource Manager szerinti erőforrás hello név mezője megegyezik. 

>[!NOTE]
>Szolgáltatástérkép szolgáltatások növekedésével a mezők kitöltése tulajdonos toochange.

Nincsenek belsőleg generált tulajdonságok tooidentify egyedi folyamatokat és az olyan számítógépek is használhatja:

- Számítógép: Használata ResourceId vagy ResourceName_s toouniquely azonosítsa a számítógépet az Operations Management Suite-munkaterülethez belül.
- Folyamat: Használata ResourceId toouniquely azonosítja a belül az Operations Management Suite-munkaterülethez. ResourceName_s egyedi hello gép mely hello folyamat fut. (MachineResourceName_s) hello környezeten belül 

Mivel több rekord is létezik egy adott folyamat és a számítógép egy megadott időtartományba esik, a lekérdezések visszaadhatják a hello egynél több rekordot ugyanazon a számítógépen vagy folyamat. tooinclude csak hello legutóbbi rekord hozzáadása "|} a deduplikáció ResourceId"toohello lekérdezés.

### <a name="servicemapcomputercl-records"></a>ServiceMapComputer_CL rekordok
Típusú rekordok *ServiceMapComputer_CL* van a Service Map ügynökkel kiszolgálók Hardverleltár-adatait. Ezeket a rekordokat a következő táblázat hello hello jellemzőkkel rendelkezik:

| Tulajdonság | Leírás |
|:--|:--|
| Típus | *ServiceMapComputer_CL* |
| SourceSystem | *OpsManager* |
| ResourceId | az egyik számítógépére hello munkaterület hello egyedi azonosítója |
| ResourceName_s | az egyik számítógépére hello munkaterület hello egyedi azonosítója |
| ComputerName_s | hello számítógépének FQDN-neve |
| Ipv4Addresses_s | Hello kiszolgáló IPv4-címek listája |
| Ipv6Addresses_s | Hello kiszolgáló IPv6-címek listája |
| DnsNames_s | DNS-nevek tömbje |
| OperatingSystemFamily_s | A Windows vagy Linux |
| OperatingSystemFullName_s | hello hello operációs rendszer teljes név  |
| Bitness_s | hello bitszámának hello machine (32 bites vagy 64 bites)  |
| PhysicalMemory_d | hello fizikai memória MB-ban |
| Cpus_d | hello processzorok száma |
| CpuSpeed_d | hello processzorsebesség MHz-ben|
| VirtualizationState_s | *ismeretlen*, *fizikai*, *virtuális*, *hipervizor* |
| VirtualMachineType_s | *Hyper-v*, *vmware*, és így tovább |
| VirtualMachineNativeMachineId_g | hello a hipervizor által hozzárendelt virtuális gép azonosítója |
| VirtualMachineName_s | virtuális gép hello hello neve |
| BootTime_t | hello rendszerindítás |



### <a name="servicemapprocesscl-type-records"></a>ServiceMapProcess_CL típusú rekordok
Típusú rekordok *ServiceMapProcess_CL* rendelkezik TCP kapcsolódó eljárások a Hardverleltár-adatait a Szolgáltatástérkép ügynökök kiszolgálókon. Ezeket a rekordokat a következő táblázat hello hello jellemzőkkel rendelkezik:

| Tulajdonság | Leírás |
|:--|:--|
| Típus | *ServiceMapProcess_CL* |
| SourceSystem | *OpsManager* |
| ResourceId | hello hello munkaterület belül a folyamat egyedi azonosítója |
| ResourceName_s | hello hello gépen, amelyen fut a folyamat egyedi azonosítója|
| MachineResourceName_s | hello gép hello erőforrás neve |
| ExecutableName_s | hello hello folyamat végrehajtható fájl nevét |
| StartTime_t | hello folyamat készlet kezdési ideje |
| FirstPid_d | hello első azonosítója (PID) hello folyamatkészletben |
| Description_s | hello folyamat leírása |
| CompanyName_s | hello hello vállalat neve |
| InternalName_s | hello belső neve |
| ProductName_s | hello hello termék neve |
| ProductVersion_s | hello termékverzió |
| FileVersion_s | hello fájlverzió |
| CommandLine_s | hello parancssor |
| ExecutablePath z | hello elérési toohello végrehajtható fájl |
| WorkingDirectory_s | hello munkakönyvtár |
| Felhasználónév | hello fiók alatt mely hello folyamat végrehajtása történik |
| UserDomain | hello tartomány alapján mely hello folyamat végrehajtása történik |


## <a name="sample-log-searches"></a>Naplókeresési minták

### <a name="list-all-known-machines"></a>Az összes ismert gép felsorolása
Típus = ServiceMapComputer_CL |} a deduplikáció ResourceId

### <a name="list-hello-physical-memory-capacity-of-all-managed-computers"></a>Lista hello fizikai memória kapacitása minden felügyelt számítógéphez.
Típus = ServiceMapComputer_CL |} Válassza ki a PhysicalMemory_d, ComputerName_s |} A Deduplikáció ResourceId

### <a name="list-computer-name-dns-ip-and-os"></a>Számítógép neve, a DNS, az IP és a az operációs rendszer.
Típus = ServiceMapComputer_CL |} Válassza ki a ComputerName_s, OperatingSystemFullName_s, DnsNames_s, IPv4Addresses_s |} a deduplikáció ResourceId

### <a name="find-all-processes-with-sql-in-hello-command-line"></a>Az "sql" minden folyamat található hello parancssor
Típus = ServiceMapProcess_CL CommandLine_s = \*sql\* |} deduplikáció ResourceId

### <a name="find-a-machine-most-recent-record-by-resource-name"></a>Található (legutóbbi rekord) gép neve szerint
Típus = "m-4b9c93f9-bc37-46df-b43c-899ba829e07b" ServiceMapComputer_CL |} a deduplikáció ResourceId

### <a name="find-a-machine-most-recent-record-by-ip-address"></a>Található (legutóbbi rekord) gép IP-cím szerint
Típus = "10.229.243.232" ServiceMapComputer_CL |} a deduplikáció ResourceId

### <a name="list-all-known-processes-on-a-specified-machine"></a>A megadott számítógép összes ismert folyamat felsorolása
Típus ServiceMapProcess_CL MachineResourceName_s="m-4b9c93f9-bc37-46df-b43c-899ba829e07b =" |} a deduplikáció ResourceId

### <a name="list-all-computers-running-sql"></a>Megjeleníti az SQL futtató összes számítógépet
Típus ServiceMapComputer_CL ResourceName_s IN = {típus = ServiceMapProcess_CL \*sql\* |} Különböző MachineResourceName_s} |} a deduplikáció ResourceId |} Különböző ComputerName_s

### <a name="list-all-unique-product-versions-of-curl-in-my-datacenter"></a>A saját adatközpont curl összes egyedi termék verziójának felsorolása
Típus = ServiceMapProcess_CL ExecutableName_s = curl |} Különböző ProductVersion_s

### <a name="create-a-computer-group-of-all-computers-running-centos"></a>Hozzon létre egy számítógépcsoportot CentOS rendszerrel működő számítógépek
Típus = ServiceMapComputer_CL OperatingSystemFullName_s = \*CentOS\* |} Különböző ComputerName_s


## <a name="rest-api"></a>REST API
Minden hello server, a folyamat és a függőségi Szolgáltatástérkép adatai hello keresztül elérhető [szolgáltatás térkép REST API](https://docs.microsoft.com/rest/api/servicemap/).


## <a name="diagnostic-and-usage-data"></a>diagnosztikai és használati adatok
A Microsoft automatikusan használati és teljesítményadatokat gyűjt a Szolgáltatástérkép szolgáltatás hello használata. A Microsoft ezen adatok tooprovide használ, és hello minőségének, biztonsági és hello Szolgáltatástérkép szolgáltatás integritásának javítására. tooprovide pontos és hatékony hibaelhárítási képességei, hello adatok tartalmazzák a szoftverek, például az operációs rendszer és a verziója, a IP-cím, a DNS-nevét és a munkaállomás neve hello konfigurációs adatait. A Microsoft nem gyűjti, neveket, címeket és egyéb kapcsolattartási adatait.

Adatok gyűjtésével és használatával kapcsolatos további információkért lásd: hello [Microsoft Online Services adatvédelmi nyilatkozatát](https://go.microsoft.com/fwlink/?LinkId=512132).


## <a name="next-steps"></a>Következő lépések
További információ [keresések jelentkezzen](../log-analytics/log-analytics-log-searches.md) a Naplóelemzési tooretrieve Szolgáltatástérkép által összegyűjtött adatokat.


## <a name="troubleshooting"></a>Hibaelhárítás
Lásd: hello [hibaelhárítási szakaszában hello konfigurálása a Service Map dokumentum](operations-management-suite-service-map-configure.md#troubleshooting).


## <a name="feedback"></a>Visszajelzés
Van olyan visszajelzést az USA Szolgáltatástérkép és a jelen dokumentáció?  Látogasson el a [felhasználói véleményekkel foglalkozó weblapunkra](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), ahol felajánlja a szolgáltatások és arra való szavazás meglévő javaslatok be.
