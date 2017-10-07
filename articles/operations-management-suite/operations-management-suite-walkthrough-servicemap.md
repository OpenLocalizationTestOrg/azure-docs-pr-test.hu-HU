---
title: "Térkép megoldás aaaService önkiszolgáló programról ütemben elsajátítható bemutató |} Microsoft Docs"
description: "Szolgáltatástérkép egy megoldás az Operations Management Suite (OMS), amely automatikusan felderíti az alkalmazás-összetevők a Windows és Linux rendszerek és a maps hello szolgáltatások közötti kommunikáció.  Ez az egy önkiszolgáló elsajátítható bemutató, amely használatával a Service Map tooidentify végigvezeti és a webes alkalmazásban szimulált probléma diagnosztizálásához."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 9dc437b9-e83c-45da-917c-cb4f4d8d6333
ms.service: operations-management-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: bwren
ms.openlocfilehash: 13f26241cd55a9b35c07d6ca52760a968abffc64
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="operations-management-suite-oms-self-paced-demo---service-map"></a>Operations Management Suite (OMS) saját ütemben megtekinthető bemutató – Szolgáltatástérkép
Ez az egy önkiszolgáló elsajátítható bemutató, amely végigvezeti a hello segítségével [Szolgáltatástérkép megoldás](operations-management-suite-service-map.md) az Operations Management Suite (OMS) tooidentify és a webes alkalmazásban szimulált probléma diagnosztizálásához.  Szolgáltatástérkép automatikusan észleli a Windows és Linux rendszerek alkalmazás-összetevők és a maps hello szolgáltatások közötti kommunikáció.  Más OMS szolgáltatások tooassist által gyűjtött adatokat is összegzésével a teljesítményének elemzése és a problémák azonosításához.  Azt is ismertetjük [Log Analytics-e jelentkezni a keresések](../log-analytics/log-analytics-log-searches.md) toodrill le a rendelés tooidentify hello alapvető probléma összegyűjtött adatokon.


## <a name="scenario-description"></a>Forgatókönyv leírása
Csak egy értesítést, hogy hello SOFT Ügyfélportálra alkalmazás rendelkezik-e teljesítményproblémákat kapott.  hello rendelkezik mindössze adatait, hogy ezek a problémák körülbelül 4:00 am csendes-óceáni TÉLI ma elindult-e.  Nem minden hello összetevők teljesen biztos, hogy hello portál webes kiszolgálók egy csoportja nem függ.  

## <a name="components-and-features-used"></a>Használt összetevők és szolgáltatások
- [Szolgáltatástérkép-megoldás](operations-management-suite-service-map.md)
- [Log Analytics-naplókeresések](../log-analytics/log-analytics-log-searches.md)


## <a name="walk-through"></a>Útmutató

### <a name="1-connect-toohello-oms-experience-center"></a>1. Csatlakozás toohello OMS élmény Center
Ez a lépésein végighaladva használja hello [Operations Management Suite élmény Center](https://experience.mms.microsoft.com/) mintaadatokkal teljes OMS környezetet biztosít, amely. Indítsa el ezt a hivatkozást követve, adja meg az adatokat, és válassza a hello **betekintést és az elemzések** forgatókönyv.


### <a name="2-start-service-map"></a>2. Szolgáltatástérkép indítása
Indítsa el a hello Szolgáltatástérkép megoldás hello kattintva **Szolgáltatástérkép** csempére.

![Szolgáltatástérkép csempe](media/operations-management-suite-walkthrough-servicemap/tile.png)

hello Szolgáltatástérkép konzol jelenik meg.  Hello a bal oldali ablaktáblán a környezetben hello Szolgáltatástérkép-ügynököt futtató számítógépek listáját.  Hello számítógép, amelyet az tooview a listából válasszon ki.

![Számítógépek listája](media/operations-management-suite-walkthrough-servicemap/computer-list.png)


### <a name="3-view-computer"></a>3. Számítógép megtekintése
Tudjuk, hogy hello webkiszolgálók nevezzük AcmeWFE001 és AcmeWFE002, így ez tűnik, hogy a megfelelő helyen toostart.  Kattintson az **AcmeWFE001** kiszolgálóra.  Ez megjeleníti a AcmeWFE001 hello-leképezés és az összes függősége.  Láthatja, hogy mely folyamatok futnak a hello kijelölt számítógépen, és amely külső szolgáltatásokat tudnak kommunikálni.

![Webkiszolgáló](media/operations-management-suite-walkthrough-servicemap/web-server.png)

A rendszer részletesebben szeretnének foglalkozni az internetes hello teljesítmény alkalmazás, kattintson a hello **AcmeAppPool (IIS-alkalmazáskészlet)** folyamat.  Ez a folyamat hello részleteit jeleníti meg, és kiemeli a függőségeit.  

![Alkalmazáskészlet](media/operations-management-suite-walkthrough-servicemap/app-pool.png)


### <a name="4-change-time-window"></a>4. Időtartomány módosítása

A Microsoft hallgassa meg, hogy tekintse meg a következő mi zajlik akkori 4:00 órakor indítása most hello probléma. Kattintson a **időtartomány** , és módosítsa a hello idő too4: 00 csendes-óceáni TÉLI de (megtartásához hello aktuális, és beállíthatja a helyi időzóna) 20 perces időtartamra.

![Időválasztó](./media/operations-management-suite-walkthrough-servicemap/time-picker.png)


### <a name="5-view-alert"></a>5. Riasztás megtekintése

Most már látható, hogy hello **acmetomcat** függőség van, megjelenik egy figyelmeztetés, hogy az a lehetséges problémákról.  Kattintson a figyelmeztető ikonra hello **acmetomcat** tooshow hello hello riasztás részletei.  Láthatjuk, hogy a processzor kihasználtsága kritikus, és a riasztást kibontva további részleteket tudhatunk meg.  Valószínűleg ez a lassú teljesítmény oka. 

![Riasztás](./media/operations-management-suite-walkthrough-servicemap/alert.png)


### <a name="6-view-performance"></a>6. Teljesítmény megtekintése

Vizsgáljuk meg közelebbről az **acmetomcat** elemet.  Kattintson a hello felső sarkában található **acmetomcat** válassza ki **terhelés Server térkép** tooshow hello információk és a függőségek ezen a számítógépen. Azt is majd megismerhetők valamivel több e teljesítmény számlálók tooverify a lehetséges.  Jelölje be hello **teljesítmény** lapon toodisplay hello [Naplóelemzési által gyűjtött teljesítményszámlálók](../log-analytics/log-analytics-data-sources-performance-counters.md) hello időtartomány keresztül.  Láthatja, hogy elvégezzük a beállításokat a hello processzor és memória rendszeres teljesítményt.

![Teljesítmény](./media/operations-management-suite-walkthrough-servicemap/performance.png)


### <a name="7-view-change-tracking"></a>7. Változáskövetés megtekintése
Lássuk, mi okozza a magas kihasználtságot.  Kattintson a hello **összegzés** fülre.  Ez lehetővé teszi, hogy OMS, mint a hello számítógép rendelkezik gyűjtött információkat nem sikerült, kapcsolatok, a kritikus riasztások és a szoftver változásainak.  Már kibontható-szakaszok érdekes friss információkat, és bővítheti egyéb szakaszok tooinspect információkat tartalmaz.


Ha a **Változáskövetés** nincs megnyitva, bontsa ki.  Ez azt jelenti, hogy hello által gyűjtött adatok [változáskövetési megoldás](../log-analytics/log-analytics-change-tracking.md).  Úgy tűnik, hogy az adott időtartományban változás történt a szoftverekben.  Kattintson a **szoftver** tooget részleteit.  A biztonsági mentési folyamat hozzá lett adva toohello gép után 4:00-kor, így ez toobe hello hibát okozó hello túlzott erőforrások feldolgozottként jelenik meg.

![Változások követése](./media/operations-management-suite-walkthrough-servicemap/change-tracking.png)



### <a name="8-view-details-in-log-search"></a>8. Részletek megtekintése a naplókeresésben
További ellenőrizni tudja a hello megtekintésével részletes hello Naplóelemzési tárházban összegyűjtött teljesítményadatok.  Kattintson a hello **riasztások** újra lapon majd hello egyikét a **magas CPU** riasztásokat.  Kattintson a **Megjelenítés a naplókeresésben** lehetőségre.  Ekkor megnyílik a hello naplófájl-keresési ablak, ahol hajthat végre [keresések jelentkezzen](../log-analytics/log-analytics-log-searches.md) elleni hello tárházban tárolt összes adatot.  Szolgáltatástérkép már kitöltötte a queriy nekünk tooretrieve hello riasztás azt kíváncsiak vagyunk.  

![Naplókeresés](./media/operations-management-suite-walkthrough-servicemap/log-search.png)


### <a name="9-open-saved-search"></a>9. Mentett keresés megnyitása
Nézzük meg, ha azt néhány további információkhoz juthat a riasztást kiváltó hello teljesítménygyűjtés szerezni, és ellenőrizze a lehetséges, hogy hello problémák okozta-e biztonsági mentési folyamat.  Hello időtartomány túl módosítása**6 óra**.  Kattintson a **Kedvencek** és mentett toohello keres görgetve **Szolgáltatástérkép**.  Ezek olyan lekérdezések, amelyeket külön ehhez az elemzéshez hozunk létre.  Kattintson az **Első 5 folyamat processzorhasználat alapján – acmetomcat** elemre.

![Mentett keresés](./media/operations-management-suite-walkthrough-servicemap/saved-search.png)


A lekérdezés által visszaadott hello listájának első 5 folyamat, amely a processzor túlzottan **acmetomcat**.  Hello lekérdezés tooget egy bevezető toohello lekérdezési nyelv napló kereséshez használt vizsgálhatja meg.  Ha más számítógépeken hello folyamatok iránt érdeklődik, ezt az információt hello lekérdezés tooretrieve volt módosítani.

Ebben az esetben láthatja, hogy hello biztonsági mentési folyamat következetesen fel hello alkalmazások kiszolgálói CPU készül 60 %-át.  Egyértelmű, hogy ez az új folyamat a felelős a teljesítményproblémákért.  A megoldás nyilvánvalóan lenne tooremove Ez új biztonsági mentési szoftverrel hello alkalmazáskiszolgáló ki.  Azt a sikerült ténylegesen használja ki a Szükségeskonfiguráció-konfiguráló (DSC) Azure Automation-toodefine házirendek, amelyek biztosítják, hogy ez a folyamat soha nem fut ezen kritikus rendszerek.


## <a name="summary-points"></a>Összefoglaló pontok
- A [Szolgáltatástérkép](operations-management-suite-service-map.md) megjeleníti a teljes alkalmazás nézetét, még akkor is, ha nem ismeri az összes kiszolgálóját és függőségét.
- Szolgáltatástérkép felfed adatok más OMS megoldások toohelp által összegyűjtött, problémák azonosításához az alkalmazás és az alapjául szolgáló infrastruktúra.
- [A keresések jelentkezzen](../log-analytics/log-analytics-log-searches.md) oszthatja toodrill le hello Naplóelemzési tárházban összegyűjtött adatokat.    

## <a name="next-steps"></a>Következő lépések
- Tudjon meg többet a [Szolgáltatástérképről](operations-management-suite-service-map.md).
- A Szolgáltatástérkép [üzembe helyezése és konfigurálása](operations-management-suite-service-map-configure.md).
- Tudjon meg többet a [Log Analyticsről](../log-analytics/log-analytics-overview.md), amely szükséges a Szolgáltatástérkép használatához, és amely az ügynökök által gyűjtött működési adatokat tárolja.