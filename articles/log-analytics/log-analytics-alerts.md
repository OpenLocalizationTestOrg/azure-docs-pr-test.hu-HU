---
title: "az Azure Naplóelemzés aaaUnderstanding riasztások |} Microsoft Docs"
description: "Log Analytics riasztások határozza meg az OMS-adattárban lévő fontos adatokat és is proaktív értesítést küldenek, problémák vagy meg kíván hívni műveletek tooattempt toocorrect őket.  Ez a cikk ismerteti a hello különböző riasztási szabályok és hogyan vannak definiálva."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6cfd2a46-b6a2-4f79-a67b-08ce488f9a91
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: bwren
ms.openlocfilehash: bfa0a5aaeca81674e79a6d647f36d937efeeb439
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-alerts-in-log-analytics"></a>A Naplóelemzési riasztások ismertetése

Log Analytics riasztások határozza meg a Naplóelemzési tárházban fontos adatokat.  Ez a cikk részletesen ismerteti, hogyan riasztási szabályok Naplóelemzési munka, és ismerteti a különböző típusú riasztási szabályok hello különbségei.

A riasztási szabályok létrehozásának hello folyamatán tekintse meg a következő cikkek hello:

- Riasztási szabályok létrehozására [Azure-portálon](log-analytics-alerts-creating.md)
- Riasztási szabályok létrehozására [Resource Manager-sablon](../operations-management-suite/operations-management-suite-solutions-resources-searches-alerts.md)
- Riasztási szabályok létrehozására [REST API-n](log-analytics-api-alerts.md)


## <a name="alert-rules"></a>A riasztási szabályok

A riasztási szabályok, amelyek automatikusan futnak a napló keresések rendszeres időközönként riasztások jönnek létre.  Ha hello napló keresési eredmények hello adott feltételeknek, egy riasztás rekord jön létre.  hello szabály követően automatikusan futtathatja egy vagy több műveletek tooproactively hello riasztás értesíti vagy meg kíván hívni egy másik folyamat.  Különböző típusú riasztási szabályok használata különböző logika tooperform az elemzés.

![Log Analytics-riasztások](media/log-analytics-alerts/overview.png)

A riasztási szabályok következő adatok hello határozzák meg:

- **Naplófájl-keresési**.  hello lekérdezés, amely futtatja a minden alkalommal, amikor a riasztási szabály hello következik be.  Ez a lekérdezés által visszaadott hello rekordok használt toodetermine, hogy létrejön egy riasztás.
- **Időablak**.  Megadja a hello időtartomány hello lekérdezéshez.  hello lekérdezés csak azt jelzi, hogy hello ebben a tartományban jöttek létre aktuális időt adja vissza.  Ez bármilyen 5 perc és 24 óra közötti érték lehet. Például ha hello időt ablak van beállítva, too60 perc és hello lekérdezés futtatása: 1:15 előtti csak 12:15 előtti és 1:15 előtti között létrejövő rekordok ad vissza.
- **Gyakoriság**.  Itt adhatja meg, milyen gyakran hello lekérdezést kell futtatni. Bármely érték 5 perc és 24 óra közötti lehet. Hello időkerete-nál kisebb egyenlő tooor kell lennie.  Ha hello értéke nagyobb, mint hello időtartományt, azzal alatt elmulasztott rekordok kockáztatja ki.<br>Vegye figyelembe például egy olyan időkeretet, 30 perc és 60 perc gyakorisága.  Ha hello lekérdezés futtatása, 1:00, 12:30 és 1:00 PM rekordok adja vissza.  hello hello lekérdezés futna legközelebb 2:00 amikor meghaladná a 1:30 és 2:00 között rögzíti.  1:00 és 1:30 között létrejövő rekordok volna soha nem értékelhető ki.
- **Küszöbérték**.  hello hello napló keresési eredményei kiértékelt toodetermine hogy létrejöjjön egy riasztás.  hello küszöbértéke különböző hello különböző típusú riasztási szabályok.

A Naplóelemzési minden riasztási szabály a két típus egyike.  Ezek a típusok leírását hello szakaszok részletesen.

- **[Találatok száma](#number-of-results-alert-rules)**. Egyetlen riasztás jön létre, amikor hello számú rekordok hello napló keresés által visszaadott meghaladja a megadott értéket.
- **[Metrika mérési](#metric-measurement-alert-rules)**.  Riasztás létre az egyes objektumok hello napló keresése a megadott küszöbértéket meghaladó értékek hello eredményeit.

hello különbségei riasztási szabályok típusai a következők:

- **Találatok száma** riasztási szabály mindig létrehoz egy riasztás rövid **metrika mérési** riasztási szabály minden objektum, amely meghaladja a küszöbértéket hello riasztást hoz létre.
- **Találatok száma** riasztási szabályok riasztást hoznak létre, amikor hello küszöbérték elérése esetén egy alkalommal. **Metrika mérési** riasztási szabályok riasztást létre hello küszöbérték túllépésekor bizonyos számú alkalommal keresztül egy adott időintervallumban.

## <a name="number-of-results-alert-rules"></a>Eredmények riasztási szabályok száma
**Találatok száma** riasztási szabályok egyetlen riasztás létrehozása, ha hello hello keresési lekérdezés által visszaadott rekordok több hello megadott küszöbértéket.

### <a name="threshold"></a>Küszöbérték
hello küszöbértéke egy **eredmények száma** riasztási szabály egyszerűen nagyobb vagy kisebb, mint egy adott értéket.  Hello hello napló keresés által visszaadott rekordok számát a feltételeknek megfelelő, ha riasztás jön létre.

### <a name="scenarios"></a>Forgatókönyvek

#### <a name="events"></a>Események
Riasztási szabály az ilyen típusú ideális munkavégzés az eseményekkel, például a Windows eseménynaplóiban keresse meg, a Syslog, és az egyéni naplózza.  Érdemes lehet toocreate riasztást, ha egy adott hibaesemény jön létre, vagy ha több hibaesemények belül egy adott időkerete jönnek létre.

tooalert egyszeri esemény, mint 0, és mindkét hello gyakorisága eredmények toogreater hello számának megadása, és a time ablak too5 perc.  Hello lekérdezés futó minden 5 perc és egy adott eseményhez hello utolsó idő hello lekérdezés futtatása óta létrehozott hello előfordulása ellenőrzése.  Hosszabb gyakorisága miatt késhet hello idő közötti hello esemény lesznek összegyűjtött és hello riasztás létrehozása folyamatban.

Egyes alkalmazások jelentkezhetnek be, amely nem feltétlenül riasztás alkalmi hiba.  Hello alkalmazás például újra hello hibaesemény létrehozott hello folyamatot és hello legközelebb majd sikeres.  Ebben az esetben kívánt toocreate hello riasztás kivéve, ha több esemény egy adott időpontban időszakban jönnek létre.  

Bizonyos esetekben érdemes lehet az toocreate riasztást hello hiányában egy eseményt.  A folyamat például előfordulhat, hogy jelentkezzen rendszeres eseményekhez tooindicate megfelelően működik.  Ha egy ilyen eseményt belül egy adott időkerete nem jelentkeznek, majd riasztást kell létrehozni.  Ebben az esetben állíthat hello küszöbértéke túl**1-nél kevesebb**.

#### <a name="performance-alerts"></a>Teljesítményével kapcsolatos riasztások
[Teljesítményadatok](log-analytics-data-sources-performance-counters.md) hello OMS-tárház hasonló tooevents rekordjait tárolja.  Ha azt szeretné tooalert, ha a teljesítményszámláló eléri az adott, majd a küszöbérték szerepeljenek hello lekérdezés.

Például ha tooalert, amikor hello processzor futtatja 90 %-át szeretné használni például hello hello küszöbértéke hello riasztási szabály az alábbi lekérdezés **0-nál nagyobb**.

    Type=Perf ObjectName=Processor CounterName="% Processor Time" CounterValue>90

Ha tooalert amikor hello processzor átlagosan több mint 90 %-át egy adott időtartomány, hello lekérdezést használja [parancs mérésére](log-analytics-search-reference.md#commands) hello következőre hello riasztási szabály hello küszöbértékét, például **nagyobb, mint 0** .

    Type=Perf ObjectName=Processor CounterName="% Processor Time" | measure avg(CounterValue) by Computer | where AggregatedValue>90

>[!NOTE]
> Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), majd a fenti lekérdezések hello megváltozna toohello következő:`Perf | where ObjectName=="Processor" and CounterName=="% Processor Time" and CounterValue>90`
> `Perf | where ObjectName=="Processor" and CounterName=="% Processor Time" | summarize avg(CounterValue) by Computer | where CounterValue>90`


## <a name="metric-measurement-alert-rules"></a>Metrika mérési riasztási szabályok

>[!NOTE]
> Riasztási szabályok metrika mérési jelenleg nyilvános előzetes verziójához.

**Metrika mérési** riasztási szabályok létrehozása minden objektum egy riasztást a megadott küszöbértéket meghaladó érték lekérdezésben.  Rendelkeznek-e a következő különböző eltérések a hello **eredmények száma** riasztási szabályok.

#### <a name="log-search"></a>Naplókeresés
Használhatja a lekérdezés egy **eredmények száma** riasztás szabályok nincsenek konkrét követelmények hello lekérdezés egy metrika mérési riasztási szabály.  Tartalmaznia kell egy [parancs mérésére](log-analytics-search-reference.md#commands) toogroup hello eredményeket egy adott mező. Ez a parancs a következő elemek hello tartalmaznia kell.

- **Összesítő függvény**.  Meghatározza, hogy hello számítási végzett, és esetleg egy számmező tooaggregate.  Például **count()** visszatér a rekordok száma hello hello lekérdezésben **avg(CounterValue)** hello időszakban hello átlagos hello ellenértéknek mező ad vissza.
- **Csoport mező**.  Az összesített érték egy rekord jön létre minden egyes példányánál ebben a mezőben, és a riasztás is generálható minden.  Például ha egy riasztás toogenerate számítógépenként, használhatja **számítógépenként**.   
- **Időköz**.  Hello időtartam, mely hello adatok összesített értéket határoz meg.  Például, ha a megadott **5minutes**, egy rekord létrehozott hello csoportmező, 5 perces időközönként hello riasztás megadott hello időszak alatt összesített értéket minden egyes példányánál.

#### <a name="threshold"></a>Küszöbérték
riasztási szabályok metrika mérési hello küszöbértéke összesítő érték és a behatolások határozzák meg.  Bármely adatpont hello napló keresése meghaladja ezt az értéket, ha a jövőben éri figyelembe.  Ha az összes objektum hello eredmények megszegése hello száma meghaladja a hello érték, akkor riasztást hoz létre, hogy az objektum.

#### <a name="example"></a>Példa
Fontolja meg egy olyan forgatókönyvet, ahol keresett riasztást, ha a számítógép számát processzorhasználat 90 %-os háromszor több mint 30 perc.  Riasztási szabály a következő adatok hello hozna létre.  

**Lekérdezés:** típus = telj ObjectName processzor CounterName = "kihasználtsága (%) = |} avg(CounterValue) mértékcsoport által számítógép időköz 5 perc<br>
**Időablak:** 30 perc<br>
**Riasztási gyakoriságot:** 5 perc<br>
**Értéket összesítő:** kiváló 90-nél<br>
**Eseményindító riasztás alapján:** összege nagyobb, mint 5 feltöri<br>

hello lekérdezés 5 perces időközönként hozna létre az egyes számítógépek átlagos értékét.  Ez a lekérdezés kell futna át 5 percenként összegyűjtött adatok keresztül hello előző 30 perc.  Mintaadatokat három számítógépek alább láthatók.

![A minta lekérdezés eredményei](media/log-analytics-alerts/metrics-measurement-sample-graph.png)

Ebben a példában külön riasztások létrehozott KSZLG02 és srv03 mivel azok megszegése hello 90 %-os küszöbértéket 3-szor hello időszak alatt.  Ha hello **eseményindító riasztás alapján:** túl módosított**folyamatos** riasztást volna létre srv03 csak a, mivel azt megszegése 3 egymást követő minták hello küszöbértékét.

## <a name="alert-records"></a>Riasztási rekordok
A Naplóelemzési riasztási szabályok által létrehozott riasztás rekordok rendelkezik egy **típus** a **riasztás** és egy **SourceSystem** a **OMS**.  A következő táblázat hello hello tulajdonságok rendelkeznek.

| Tulajdonság | Leírás |
|:--- |:--- |
| Típus |*Riasztás* |
| SourceSystem |*OMS* |
| *Objektum*  | [Metrika mérési riasztások](#metric-measurement-alert-rules) hello csoportmező tulajdonságának fog rendelkezni.  Például ha csoportok hello napló keresése a számítógépen hello riasztási rekord, kell hello számítógép hello nevű számítógép mező hello értékként.
| AlertName |Hello riasztás nevét. |
| AlertSeverity |Hello riasztás súlyossági szintje. |
| LinkToSearchResults |Hivatkozás tooLog Analytics napló keresési visszaadó hello rekordok hello riasztást létrehozó hello lekérdezésből. |
| Lekérdezés |Futtatott hello lekérdezés szövegét. |
| QueryExecutionEndTime |Hello időtartomány hello lekérdezés végét. |
| QueryExecutionStartTime |Hello időtartomány hello lekérdezés elindítása. |
| ThresholdOperator | Hello riasztási szabály által használt operátor. |
| ThresholdValue | Hello riasztási szabály által használt érték. |
| TimeGenerated |Dátum és idő hello riasztás létrejött. |

Más típusú hello által létrehozott riasztás rekordok léteznek [riasztás felügyeleti megoldás](log-analytics-solution-alert-management.md) és [Power BI exportálja](log-analytics-powerbi.md).  Ezek minden rendelkeznek egy **típus** a **riasztási** alapján megkülönböztetett, de azok **SourceSystem**.


## <a name="next-steps"></a>Következő lépések
* Hello telepítése [Riasztáskezelési megoldás](log-analytics-solution-alert-management.md) Naplóelemzési riasztások együtt létrehozott tooanalyze riasztások begyűjti a System Center Operations Manager.
* Tudjon meg többet az [keresések jelentkezzen](log-analytics-log-searches.md) , amely riasztást generál.
* A forgatókönyv a [konfigurálása egy webook](log-analytics-alerts-webhooks.md) a riasztási szabályt.  
* Megtudhatja, hogyan toowrite [az Azure Automation runbookjai](https://azure.microsoft.com/documentation/services/automation) riasztások által azonosított tooremediate problémák.
