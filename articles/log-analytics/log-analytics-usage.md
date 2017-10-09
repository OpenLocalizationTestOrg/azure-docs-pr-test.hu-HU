---
title: "Naplóelemzési aaaAnalyze adatok felhasználásának |} Microsoft Docs"
description: "Naplóelemzési tooview mennyi adatot küldött toohello Naplóelemzés szolgáltatás, és ezért nagy mennyiségű adatok küldési hibáinak elhárítása hello használati irányítópult használja."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 74d0adcb-4dc2-425e-8b62-c65537cef270
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/21/2017
ms.author: magoedte
ms.openlocfilehash: c30373dd6edbe3ff900fbebc865575fee61ce14c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-data-usage-in-log-analytics"></a>Az adathasználat elemzése a Log Analyticsben
A Naplóelemzési összegyűjtött adatok, amelyek számítógépek küldi hello adatok és a különböző típusú hello küldött adatok mennyisége hello kapcsolatos tartalmaz.  Használjon hello **Analytics Naplóhasználatra** irányítópult toosee hello adatok mennyisége toohello Naplóelemzés szolgáltatás küldött. hello irányítópulton látható mennyi adatot gyűjt minden megoldás, és mennyi adatot a számítógépek üzenetet küld.

## <a name="understand-hello-usage-dashboard"></a>Hello használati irányítópult ismertetése
Hello **Naplóelemzési használati** irányítópulton megjelenített hello a következő információkat:

- Adatmennyiség
    - Adatmennyiség az idő függvényében (az aktuális időtartomány alapján)
    - Adatmennyiség megoldásonként
    - Adatok, amelyek nincsenek számítógéphez rendelve
- Számítógépek
    - Adatokat küldő számítógépek
    - Számítógépek, amelyek nem küldtek adatokat az elmúlt 24 órában
- Ajánlatok
    - Insight- és Analytics-csomópontok
    - Automation and Control-csomópontok
    - Biztonsági csomópontok
- Teljesítmény
    - Toocollect és index adatok igénybe vett idő
- Lekérdezések listája

![A Használat irányítópult](./media/log-analytics-usage/usage-dashboard01.png)

### <a name="toowork-with-usage-data"></a>a használati adatok toowork
1. Ha még nem tette meg, jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) használata az Azure-előfizetéshez.
2. A hello **Hub** menüben kattintson a **további szolgáltatások** hello az erőforrások listájához, írja be a **Naplóelemzési**. Írja be megkezdése előtt, hello szűrők megjelenítése a megadott feltételeknek. Kattintson a **Log Analytics** elemre.  
    ![Azure-központ](./media/log-analytics-usage/hub.png)
3. Hello **Naplóelemzési** irányítópult a munkaterületek listáját jeleníti meg. Jelöljön ki egy munkaterületet.
4. A hello *munkaterület* irányítópultján kattintson **Naplóelemzési használati**.
5. A hello **Analytics Naplóhasználatra** irányítópultján kattintson **idő: utolsó 24 óra** toochange hello alatt az időtartam alatt.  
    ![időintervallum](./media/log-analytics-usage/time.png)
6. Nézet hello használati kategória paneleken területek megjelenítő kíváncsiak vagyunk. Válasszon egy panel, és kattintson a bármely elemére a részletesebben tooview [naplófájl-keresési](log-analytics-log-searches.md).  
    ![példa adathasználati panelre](./media/log-analytics-usage/blade.png)
7. Tekintse meg az irányítópult hello napló keresése hello eredmények hello keresési eredményei.  
    ![példa használati panelre a naplókeresésben](./media/log-analytics-usage/usage-log-search.png)

## <a name="create-an-alert-when-data-collection-is-higher-than-expected"></a>Riasztás létrehozása, amikor az adatgyűjtés szintje a vártnál magasabb
Ez a szakasz ismerteti, hogyan toocreate riasztás, ha:
- Az adatmennyiség meghalad egy megadott mennyiséget.
- Adatmennyiség előre jelzett tooexceed egy megadott időtartamig.

A Log Analytics[-riasztások](log-analytics-alerts-creating.md) keresési lekérdezéseket használnak. hello következő lekérdezés van eredménye 100 GB-nál több adatgyűjtés hello utolsó 24 órában esetén:

`Type=Usage QuantityUnit=MBytes IsBillable=true | measure sum(div(Quantity,1024)) as DataGB by Type | where DataGB > 100`

hello következő lekérdezés használja egy egyszerű képlet toopredict Ha több mint 100 GB adatot küld egy napon belül: 

`Type=Usage QuantityUnit=MBytes IsBillable=true | measure sum(div(mul(Quantity,8),1024)) as EstimatedGB by Type | where EstimatedGB > 100`

tooalert másik kötetet, módosítás hello 100 hello lekérdezések toohello számú tooalert kívánt GB.

Ismertetett hello lépésekkel [riasztási szabályt létrehozni](log-analytics-alerts-creating.md#create-an-alert-rule) toobe adatok gyűjtése a vártnál nagyobb értesítést.

Amikor hello első lekérdezés – hello riasztás létrehozása, ha több mint 100 GB adat 24 órában, állítsa be a:
- **Név** túl*24 órában 100 GB-nál nagyobb adatmennyiség*
- **Súlyossági** túl*figyelmeztetés*
- **Keresési lekérdezés** túl`Type=Usage QuantityUnit=MBytes IsBillable=true | measure sum(div(Quantity,1024)) as DataGB by Type | where DataGB > 100`
- **Időablak** túl*24 óra*.
- **Riasztási gyakoriságot** toobe egy órával óta hello használati adatok csak óránként egyszer frissíti.
- **Riasztás alapján** toobe *eredmények száma*
- **Találatok száma** toobe *nagyobb, mint 0*

Ismertetett hello lépésekkel [műveletek tooalert szabályok hozzáadása](log-analytics-alerts-actions.md) egy e-mail, a webhook vagy a runbook műveletet a riasztási szabály hello konfigurálása.

Ha hello második lekérdezés – hello riasztás létrehozása során azt előre jelezni fogja a több mint 100 GB adat 24 órában beállítása a:
- **Név** túl*adatmennyiség várható 100 GB-nál toogreater 24 órában*
- **Súlyossági** túl*figyelmeztetés*
- **Keresési lekérdezés** túl`Type=Usage QuantityUnit=MBytes IsBillable=true | measure sum(div(mul(Quantity,8),1024)) as EstimatedGB by Type | where EstimatedGB > 100`
- **Időablak** túl*3 óra*.
- **Riasztási gyakoriságot** toobe egy órával óta hello használati adatok csak óránként egyszer frissíti.
- **Riasztás alapján** toobe *eredmények száma*
- **Találatok száma** toobe *nagyobb, mint 0*

Riasztás megjelenésekor kövesse a következő szakasz tootroubleshoot miért használati a vártnál nagyobb hello hello lépéseket.

## <a name="troubleshooting-why-usage-is-higher-than-expected"></a>A vártnál magasabb szintű használatot okozó hibák elhárítása
hello használati Irányítópult segítségével tooidentify miért használati (és a költségeket, ezért) nagyobb, mint a várt.

A magasabb szintű használatot a következők okozhatják:
- Küldési tooLog Analytics vártnál több adat
- Több csomópontot várt adatok tooLog Analytics küldése

### <a name="check-if-there-is-more-data-than-expected"></a>Annak ellenőrzése, hogy a vártnál több adatot küld-e a rendszer 
Két fő szakasz hello használati lap, amelyek segítenek azonosítani, hogy mi okozza a legtöbb adatok toobe gyűjtött hello van.

Hello *adatmennyiség időbeli* diagram hello teljes küldött adatok mennyiségét jeleníti meg, és küld hello számítógépek hello legtöbb adatot. hello diagram hello felső lehetővé teszi toosee, ha az összesített adatok használatának növekvő, állandó vagy csökkenő maradt. a számítógépek listáját hello adatküldés hello legtöbb hello 10 olyan számítógépet jeleníti meg.

Hello *megoldás adatmennyiség* diagram hello minden megoldás és a legtöbb adatokat küldi hello hello megoldások küldött adatok mennyiségét mutatja. hello diagram hello felső hello teljes mennyisége idővel minden megoldás által küldött adatokat jeleníti meg. Ezek az információk lehetővé teszi tooidentify e megoldás további adatait, az adatok, illetve kisebb adatok azonos összeg időbeli hello küld. hello megoldások jeleníti adatküldés hello legtöbb hello 10 megoldások. 

Ezen a két diagramon megjelenik az összes adat. Néhány adat számlázható, mások pedig ingyenesek. toofocus csak adatokon, amely számlázható, módosítsa a hello keresés lap tooinclude hello lekérdezést `IsBillable=true`.  

![adatmennyiség-diagramok](./media/log-analytics-usage/log-analytics-usage-data-volume.png)

Nézze meg hello *adatmennyiség időbeli* diagram. toosee hello megoldások és adattípusok, amelyek küldi hello legtöbb adatokat egy adott számítógépen kattintson a hello hello számítógép nevét. Kattintson az első számítógép hello hello lista hello neve.

A következő képernyőkép hello, hello *napló felügyeleti / telj* adattípus küldi hello hello legtöbb adataival. 

![adatmennyiség egy számítógép esetében](./media/log-analytics-usage/log-analytics-usage-data-volume-computer.png)

Ezután térjen vissza toohello *használati* irányítópult és hello pillantást *megoldás adatmennyiség* diagram. toosee hello számítógépek adatküldés hello legtöbb megoldás, kattintson a hello megoldás hello listában hello nevét. Kattintson a hello első megoldás hello listában hello nevét. 

A következő képernyőkép hello, az megerősíti, hogy hello *acmetomcat* számítógép küldi hello hello napló-kezelési megoldás a legtöbb adatot.

![adatmennyiség egy megoldás esetében](./media/log-analytics-usage/log-analytics-usage-data-volume-solution.png)

Ha szükséges, hajtsa végre a további elemzés tooidentify nagy méretű köteteket tartozó megoldás vagy adatokat. Példák a lekérdezésekre:

+ **Biztonsági** megoldás
  - `Type=SecurityEvent | measure count() by EventID`
+ **Naplókezelési** megoldás
  - `Type=Usage Solution=LogManagement IsBillable=true | measure count() by DataType`
+ **Perf** adattípus
  - `Type=Perf | measure count() by CounterPath`
  - `Type=Perf | measure count() by CounterName`
+ **Esemény** adattípus
  - `Type=Event | measure count() by EventID`
  - `Type=Event | measure count() by EventLog, EventLevelName`
+ **Rendszernapló** adattípus
  - `Type=Syslog | measure count() by Facility, SeverityLevel`
  - `Type=Syslog | measure count() by ProcessName`
+ **AzureDiagnostics** adattípus
  - `Type=AzureDiagnostics | measure count() by ResourceProvider, ResourceId`

A következő lépéseket tooreduce hello kötet gyűjtött naplók hello használata:

| A nagy adatmennyiség forrása | Hogyan tooreduce adatmennyiség |
| -------------------------- | ------------------------- |
| Biztonsági események            | Válassza a [gyakori vagy minimális biztonsági események](https://blogs.technet.microsoft.com/msoms/2016/11/08/filter-the-security-events-the-oms-security-collects/) lehetőséget <br> Módosítsa a hello biztonsági naplózási házirend csak akkor szükséges, toocollect eseményeket. Különösen tekintse át a hello kell toocollect eseményei <br> - [szűrőplatform naplózása](https://technet.microsoft.com/library/dd772749(WS.10).aspx) <br> - [beállításjegyzék naplózása](https://docs.microsoft.com/windows/device-security/auditing/audit-registry)<br> - [fájlrendszer naplózása](https://docs.microsoft.com/windows/device-security/auditing/audit-file-system)<br> - [kernelobjektum naplózása](https://docs.microsoft.com/windows/device-security/auditing/audit-kernel-object)<br> - [leírókezelés naplózása](https://docs.microsoft.com/windows/device-security/auditing/audit-handle-manipulation)<br> - [cserélhető tároló naplózása](https://docs.microsoft.com/windows/device-security/auditing/audit-removable-storage) |
| Teljesítményszámlálók       | Módosítsa a [teljesítményszámlálók konfigurációját](log-analytics-data-sources-performance-counters.md): <br> -Gyűjtemény hello gyakoriságának csökkentése <br> – Csökkentse a teljesítményszámlálók számát |
| Eseménynaplók                 | Módosítsa az [eseménynaplók konfigurációját](log-analytics-data-sources-windows-events.md): <br> -Csökkentse az összegyűjtött Eseménynapló hello száma <br> – Csak a szükséges eseményszinteket gyűjtse. Ne gyűjtsön például *Tájékoztatás* szintű eseményeket |
| Rendszernapló                     | Módosítsa a [rendszernapló konfigurációját](log-analytics-data-sources-syslog.md): <br> -Hello számú gyűjtött csökkentése <br> – Csak a szükséges eseményszinteket gyűjtse. Ne gyűjtsön például *Tájékoztatás* vagy *Hibakeresés* szintű eseményeket |
| AzureDiagnostics           | Az erőforrásnapló-gyűjtés módosítása a következőre: <br> -Csökkentse erőforrások küldési naplók tooLog Analytics hello számát <br> – Csak a szükséges naplókat gyűjtse |
| Megoldás adatait hello megoldást nem igénylő számítógépek | Használjon [célcsoport-kezelési megoldás](../operations-management-suite/operations-management-suite-solution-targeting.md) szükséges számítógépcsoportok csak toocollect adatait. |

### <a name="check-if-there-are-more-nodes-than-expected"></a>Annak ellenőrzése, hogy a vártnál több csomópont küld-e adatokat
Ha a hello *csomópontonként (OMS)* IP-címek, akkor van szó, a csomópontok és a megoldások használata hello száma alapján. Láthatja, hogy hány csomópontok minden ajánlat használatban lévő hello *ajánlatok* hello használati irányítópult szakasza.

![A Használat irányítópult](./media/log-analytics-usage/log-analytics-usage-offerings.png)

Kattintson a **összes...**  tooview hello teljes listáját a számítógépek adatküldés hello kijelölt szolgáltatásokat.

Használjon [célcsoport-kezelési megoldás](../operations-management-suite/operations-management-suite-solution-targeting.md) szükséges számítógépcsoportok csak toocollect adatait.


## <a name="next-steps"></a>Következő lépések
* Lásd: [Log Analytics-e jelentkezni a keresések](log-analytics-log-searches.md) toolearn toouse hello a keresési nyelv. Keresési lekérdezések tooperform további elemzés hello használati adatok is használhatja.
* Ismertetett hello lépésekkel [riasztási szabályt létrehozni](log-analytics-alerts-creating.md#create-an-alert-rule) toobe értesíti, ha a keresési feltételek teljesülése esetén
* Használjon [célcsoport-kezelési megoldás](../operations-management-suite/operations-management-suite-solution-targeting.md) szükséges számítógépcsoportok csak toocollect adatait
* Válassza a [gyakori vagy minimális biztonsági események](https://blogs.technet.microsoft.com/msoms/2016/11/08/filter-the-security-events-the-oms-security-collects/) lehetőséget
* A [teljesítményszámlálók konfigurációjának](log-analytics-data-sources-performance-counters.md) módosítása
* Az [eseménynaplók konfigurációjának](log-analytics-data-sources-windows-events.md) módosítása
* A [rendszernapló konfigurációjának](log-analytics-data-sources-syslog.md) módosítása
