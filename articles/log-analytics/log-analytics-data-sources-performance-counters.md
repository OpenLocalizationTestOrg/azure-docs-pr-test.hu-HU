---
title: "aaaCollect és elemzése az Azure Naplóelemzés teljesítményszámlálók |} Microsoft Docs"
description: "Teljesítményszámlálók Naplóelemzési tooanalyze teljesítmény szerint összegyűjtése a Windows és Linux-ügynökök.  Ez a cikk ismerteti, hogyan teljesítmény tooconfigure gyűjteménye a Windows és Linux-ügynökök, azok részleteit hello OMS-tárház tárolódnak, és hogyan teljesítményszámlálók tooanalyze az OMS-portálon hello őket."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 20e145e4-2ace-4cd9-b252-71fb4f94099e
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/12/2017
ms.author: magoedte
ms.openlocfilehash: 30146fecf8db1d8851b89fdb970f757bbb24abf1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="windows-and-linux-performance-data-sources-in-log-analytics"></a>A Naplóelemzési Windows és Linux teljesítmény adatforrások
A Windows és Linux teljesítményszámlálók hello hardverösszetevők, operációs rendszerek és alkalmazások teljesítményének betekintést.  A Naplóelemzési össze tudják gyűjteni a teljesítményszámlálók gyakori elemzésre közel valós idejű (NRT) hozzáadása tooaggregating teljesítményadatok hosszabb távú elemzéshez és jelentéskészítéshez.

![Teljesítményszámlálók](media/log-analytics-data-sources-performance-counters/overview.png)

## <a name="configuring-performance-counters"></a>Teljesítményszámlálók konfigurálása
Teljesítményszámlálók az OMS-portálon hello a hello konfigurálása [Naplóelemzés beállításai adatok menüben](log-analytics-data-sources.md#configuring-data-sources).

Amikor először konfigurálja a Windows vagy Linux teljesítményszámlálóinak új OMS-munkaterület lehetősége van a hello beállítás tooquickly létrehozása néhány általános jellegű számlálót.  A jelölőnégyzet következő tooeach szerepelnek.  Győződjön meg arról, hogy a létrehozni kívánt tooinitially számlálókat a rendszer ellenőrzi, és kattintson a **Hozzáadás hello kijelölt teljesítményszámlálók**.

Windows-teljesítményszámlálókat kiválaszthatja az egyes teljesítményszámlálókhoz bizonyos példányainak. Linux teljesítményszámlálókkal az Ön által az egyes számlálók hello példányának tooall gyermek számlálók hello szülő számláló vonatkozik. hello következő táblázatban hello közös példányok elérhető tooboth Linux és a Windows a teljesítményszámlálókat.

| Példány neve | Leírás |
| --- | --- |
| \_Összesen |Hello példányainak száma |
| \* |Minden példány |
| (/ &#124; / var) |Példány neve megegyezik: / vagy /var |

### <a name="windows-performance-counters"></a>Windows-teljesítményszámlálók

![Konfigurálja a Windows-teljesítményszámlálók](media/log-analytics-data-sources-performance-counters/configure-windows.png)

Ez az eljárás tooadd egy új Windows-teljesítmény számláló toocollect kövesse.

1. Hello típusnév hello számláló hello szövegmezőben hello formátumban *objektum (példány) \counter*.  Amikor elkezdi beírni, lehetősége lesz egyező listáját általános jellegű számlálót.  Kiválaszthat egy számláló vagy hello listából, vagy írja be egy saját.  A számláló az összes példányát adja vissza megadásával *object\counter*.  

    Elnevezett példányok az SQL Server teljesítményszámlálói összegyűjtésekor összes nevű példány számlálók útmutató *MSSQL$* hello példány hello neve követ.  Toocollect hello napló gyorsítótári találati aránya teljesítményszámláló az összes adatbázis teljesítményobjektum hello az elnevezett SQL-adatbázisok példány INST2, adja meg például `MSSQL$INST2:Databases(*)\Log Cache Hit Ratio`.

2. Kattintson a  **+**  vagy nyomja le az ENTER **Enter** tooadd hello Számlálólista toohello.
3. Számláló hozzáadása, ha a 10 másodperces hello alapértelmezett használ a **mintavételi időköze**.  Módosíthatja a tooa nagyobb az értéke be too1800 másodperc (30 perc), ha azt szeretné, hogy tooreduce hello tárolási követelményeinek hello összegyűjtött teljesítményadatok.
4. Amikor elkészült a számlálók hozzáadását, kattintson a hello **mentése** hello képernyő toosave hello konfigurációs hello tetején gombra.

### <a name="linux-performance-counters"></a>Linux-teljesítményszámlálók

![Linux-teljesítményszámlálók konfigurálása](media/log-analytics-data-sources-performance-counters/configure-linux.png)

Ez az eljárás tooadd egy új Linux teljesítmény számláló toocollect kövesse.

1. Alapértelmezés szerint az összes konfigurációs módosításhoz automatikusan vannak leküldött tooall ügynökök.  Linux-ügynökök, a konfigurációs fájl toohello Fluentd adatgyűjtő küldött.  Ha a toomodify ezt a fájlt manuálisan minden egyes Linux-ügynök, majd törölje a jelet hello mezőben *alábbi konfigurációs toomy Linux számítógépek alkalmaz* és kövesse az alábbi hello útmutatást.
2. Hello típusnév hello számláló hello szövegmezőben hello formátumban *objektum (példány) \counter*.  Amikor elkezdi beírni, lehetősége lesz egyező listáját általános jellegű számlálót.  Kiválaszthat egy számláló vagy hello listából, vagy írja be egy saját.  
3. Kattintson a  **+**  vagy nyomja le az ENTER **Enter** tooadd hello számláló toohello listája más hello objektum számlálói.
4. Egy objektum használatra számlálók hello azonos **mintavételi időköze**.  hello alapértelmezett érték 10 másodperc.  Ha azt szeretné, hogy tooreduce hello tárolási követelményeinek hello összegyűjtött teljesítményadatok módosítja tooa nagyobb az értéke be too1800 másodperc (30 perc).
5. Amikor elkészült a számlálók hozzáadását, kattintson a hello **mentése** hello képernyő toosave hello konfigurációs hello tetején gombra.

#### <a name="configure-linux-performance-counters-in-configuration-file"></a>A konfigurációs fájlban Linux teljesítményszámlálók konfigurálása
Linux teljesítményszámlálóit hello OMS-portálon konfigurálására, lehetősége van hello szerkesztési hello Linux-ügynök konfigurációs fájlokat.  Teljesítmény-mérőszámok toocollect hello konfiguráció által vezérelt **/etc/opt/microsoft/omsagent/\<munkaterület azonosítója\>/conf/omsagent.conf**.

Minden objektumot, vagy a teljesítmény-mérőszámok toocollect kategóriáját definiálni kell egy hello konfigurációs fájlban `<source>` elemet. hello szintaxisa az alábbi hello mintát követi.

    <source>
      type oms_omi  
      object_name "Processor"
      instance_regex ".*"
      counter_name_regex ".*"
      interval 30s
    </source>


Ebben az elemben hello paraméterek hello a következő táblázat ismerteti.

| Paraméterek | Leírás |
|:--|:--|
| objektum\_neve | Hello gyűjtemény objektum nevét. |
| példány\_regex |  A *reguláris kifejezés* mely példányok toocollect meghatározása. érték hello: `.*` határozza meg az összes példányát. csak hello toocollect processzor metrikáját \_teljes példányt kell megadni `_Total`. a toocollect folyamatmetrikák csak hello crond vagy sshd példány, meg kell megadni: "(crond\|sshd) ". |
| a számláló\_neve\_regex | A *reguláris kifejezés* mely számlálói (hello objektum) toocollect meghatározása. toocollect számlálók hello objektumhoz, adja meg: `.*`. toocollect terület számlálók csak felcserélése hello memória objektumhoz tartozó, például kell megadni:`.+Swap.+` |
| interval | A gyakoriság, mely hello objektum számlálók összegyűjtése. |


hello következő táblázatban hello objektumok és számlálók megadása hello konfigurációs fájlban.  Nincsenek további számlálók bizonyos alkalmazások leírtak [Linux Log Analytics-alkalmazások a teljesítményszámlálók adatainak összegyűjtése](log-analytics-data-sources-linux-applications.md).

| Objektum neve | Számláló neve |
|:--|:--|
| Logikai lemez | % Szabad Inode-OK |
| Logikai lemez | % Szabad terület |
| Logikai lemez | Foglalt Inode-OK % |
| Logikai lemez | Foglalt hely % |
| Logikai lemez | Lemez sebessége olvasott bájt/mp |
| Logikai lemez | Lemezolvasások/mp |
| Logikai lemez | Átvitel/mp |
| Logikai lemez | Lemez írási bájtok/s |
| Logikai lemez | Lemezírás/mp |
| Logikai lemez | Szabad hely MB-ban |
| Logikai lemez | Logikai lemez bájtok/s |
| Memory (Memória) | Rendelkezésre álló memória %-ban |
| Memory (Memória) | Rendelkezésre álló Lapozóterület % |
| Memory (Memória) | Foglalt memória % |
| Memory (Memória) | Foglalt Lapozóterület % |
| Memory (Memória) | Rendelkezésre álló memória |
| Memory (Memória) | Rendelkezésre álló Lapozóterület |
| Memory (Memória) | Olvasott lap/mp |
| Memory (Memória) | Írt lap/mp |
| Memory (Memória) | Lap/mp |
| Memory (Memória) | Használt memória (MB) Lapozóterület |
| Memory (Memória) | Használt memória (MB) |
| Network (Hálózat) | Küldött bájtok teljes száma |
| Network (Hálózat) | Fogadott bájtok száma összesen |
| Network (Hálózat) | Az összes bájt |
| Network (Hálózat) | Továbbított csomagok száma összesen |
| Network (Hálózat) | Fogadott csomagok száma összesen |
| Network (Hálózat) | A Rx teljes hibák |
| Network (Hálózat) | Teljes Tx hibák |
| Network (Hálózat) | Teljes ütközések |
| Fizikai lemez | Átlagos Lemez mp/Olvasás |
| Fizikai lemez | Átlagos Lemez mp/átvitel |
| Fizikai lemez | Átlagos Lemez mp/írás |
| Fizikai lemez | Fizikai lemez bájtok/s |
| Folyamat | A PCT kiemelt idő |
| Folyamat | A PCT felhasználói módú használatának aránya |
| Folyamat | Használt memória KB |
| Folyamat | Megosztott virtuális memória |
| Processzor | DPC idő % |
| Processzor | Inaktivitási idő % |
| Processzor | Megszakítási idő % |
| Processzor | IO várakozási idő % |
| Processzor | Feladatok futtatásával töltött idő %-át |
| Processzor | % Védett módú használatának aránya |
| Processzor | Processzor kihasználtsága |
| Processzor | Felhasználói idő % |
| Rendszer | Szabad fizikai memória |
| Rendszer | Szabad hely a Lapozófájlokban |
| Rendszer | Virtuális memória |
| Rendszer | Folyamatok |
| Rendszer | A Lapozófájlokban tárolt adatok mérete |
| Rendszer | Hasznos üzemidő |
| Rendszer | Felhasználók |


Az alábbiakban olvashatja a metrikák hello alapértelmezett konfigurációja.

    <source>
      type oms_omi
      object_name "Physical Disk"
      instance_regex ".*"
      counter_name_regex ".*"
      interval 5m
    </source>

    <source>
      type oms_omi
      object_name "Logical Disk"
      instance_regex ".*
      counter_name_regex ".*"
      interval 5m
    </source>

    <source>
      type oms_omi
      object_name "Processor"
      instance_regex ".*
      counter_name_regex ".*"
      interval 30s
    </source>

    <source>
      type oms_omi
      object_name "Memory"
      instance_regex ".*"
      counter_name_regex ".*"
      interval 30s
    </source>

## <a name="data-collection"></a>Adatgyűjtés
A Naplóelemzési megadott teljesítményszámlálók gyűjti. a megadott minta időközönként minden számláló telepített rendelkező ügynököknek.  hello adatok nem összesített értéket, és hello nyers adatok érhető el az összes naplófájl-keresési nézetben az OMS-előfizetés által megadott hello idejére.

## <a name="performance-record-properties"></a>Teljesítmény rekord tulajdonságai
Teljesítmény rekordok típusa lehet **telj** és a következő táblázat hello hello jellemzőkkel rendelkezik.

| Tulajdonság | Leírás |
|:--- |:--- |
| Computer |A számítógép, amely esemény hello összegyűjtött. |
| CounterName |Hello teljesítményszámláló neve |
| Számláló_elérési_útja |Hello formában hello számláló elérési útját \\ \\ \<számítógép >\\objektum(példány)\\számláló. |
| Ellenértéknek |Hello számláló numerikus értéket. |
| Példánynév |Hello esemény példány nevét.  Üres, ha egyetlen példánya. |
| ObjectName |Hello teljesítményobjektum neve |
| SourceSystem |Az összegyűjtött ügynök hello adatok típusát. <br><br>OpsManager – Windows-ügynök, vagy közvetlen kapcsolódás vagy SCOM <br> Linux – az összes Linux-ügynökök  <br> AzureStorage – az Azure Diagnostics |
| TimeGenerated |Dátum és idő hello adatok lett mintát. |

## <a name="sizing-estimates"></a>Méretezési becslése
 Egy durva becslést gyűjtemény egy adott számláló 10 másodperces időközönként körülbelül 1 MB naponkénti egy példány.  A következő képlet hello megbecsülheti egy adott számláló hello tárolási követelményeinek.

    1 MB x (number of counters) x (number of agents) x (number of instances)

## <a name="log-searches-with-performance-records"></a>Teljesítmény rekordot tartalmazó napló-keresések
hello következő táblázat példákat különböző teljesítmény lehívása napló kereséseket.

| Lekérdezés | Leírás |
|:--- |:--- |
| Típus telj = |Minden teljesítményadat |
| Típus = telj számítógép = "Sajátgép" |Egy adott számítógép minden teljesítményadat |
| Típus telj CounterName = "Lemezvárólista jelenlegi hossza" = |Egy adott számláló minden teljesítményadat |
| Típus telj = (ObjectName processzor =) CounterName = "kihasználtsága (%)" példánynév = _Total &#124; a mérték, számítógépenként AVGCPU Avg(Average) |Minden átlagos CPU-felhasználás |
| Típus telj = (CounterName = "kihasználtsága (%)") &#124;  Számítógép által a mérték max(Max) |Minden maximális CPU-felhasználás |
| Típus = telj ObjectName logikai lemez CounterName = "Az aktuális lemez várólista hossza" számítógép = = "MyComputerName" &#124; a mérték által példánynév Avg(Average) |Egy adott számítógép minden hello példányára átlagos aktuális lemez-várólista hossza |
| Típus = telj CounterName = "DiskTransfers/mp" &#124; Számítógép által a mérték percentile95(Average) |95. percentilis az átvitel/mp minden |
| Típus telj CounterName = "kihasználtsága (%)" példánynév = = "_Total" &#124; mérheti avg(CounterValue) számítógép időköze 1 óra |CPU-használat minden óránkénti átlaga |
| Típus telj számítógép = "Sajátgép" CounterName = % * példánynév = = _Total &#124; mérték percentile70(CounterValue) CounterName időköze 1 óra |Egy adott számítógépen minden % százalékos számláló óránkénti 70 százalékos érték |
| Típus telj CounterName = "kihasználtsága (%)" példánynév = = "_Total" (számítógép = "Sajátgép") &#124; mérheti min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) számítógép időköze 1 óra |Óránkénti átlag, minimális, maximális és 75-PERCENTILIS CPU-használat egy adott számítógépen |
| Típus = telj ObjectName = "MSSQL$ INST2: adatbázisok" példánynév fő = | Minden teljesítményadat hello adatbázis teljesítményének hello főadatbázis hello SQL Server-példány INST2 nevű objektum.  

>[!NOTE]
> Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), majd a fenti lekérdezések hello megváltozna toohello következő.

> | Lekérdezés | Leírás |
|:--- |:--- |
| A Teljesítményfigyelő |Minden teljesítményadat |
| A Teljesítményfigyelő &#124; Ha számítógép == "Sajátgép" |Egy adott számítógép minden teljesítményadat |
| A Teljesítményfigyelő &#124; Ha CounterName == "Lemezvárólista jelenlegi hossza" |Egy adott számláló minden teljesítményadat |
| A Teljesítményfigyelő &#124; Ha ObjectName == "Processzor" és a CounterName == "kihasználtsága (%)" és a példánynév == "_Total" &#124; AVGCPU összefoglalója = avg(Average) számítógépenként |Minden átlagos CPU-felhasználás |
| A Teljesítményfigyelő &#124; Ha CounterName == "kihasználtsága (%)" &#124; AggregatedValue összefoglalója = max(Max) számítógépenként |Minden maximális CPU-felhasználás |
| A Teljesítményfigyelő &#124; Ha ObjectName == "Logikai lemez" és a CounterName == "Lemezvárólista jelenlegi hossza" és a számítógép == "MyComputerName" &#124; AggregatedValue összefoglalója által példánynév avg(Average) = |Egy adott számítógép minden hello példányára átlagos aktuális lemez-várólista hossza |
| A Teljesítményfigyelő &#124; Ha CounterName == "DiskTransfers/mp" &#124; AggregatedValue összefoglalója = (átlagos, 95) PERCENTILIS számítógépenként |95. percentilis az átvitel/mp minden |
| A Teljesítményfigyelő &#124; Ha CounterName == "kihasználtsága (%)" és a példánynév == "_Total" &#124; AggregatedValue összefoglalója bin (TimeGenerated, 1 óra), a számítógép által avg(CounterValue) = |CPU-használat minden óránkénti átlaga |
| A Teljesítményfigyelő &#124; Ha számítógép == "Sajátgép" és a CounterName startswith_cs "%" és a példánynév == "_Total" &#124; AggregatedValue összefoglalója (ellenértéknek, 70) PERCENTILIS szerint bin (TimeGenerated, 1 óra), a CounterName = | Egy adott számítógépen minden % százalékos számláló óránkénti 70 százalékos érték |
| A Teljesítményfigyelő &#124; Ha CounterName == "kihasználtsága (%)" és a példánynév == "_Total" és a számítógép == "Sajátgép" &#124; ["min(CounterValue)"] összefoglalója min(CounterValue), = ["avg(CounterValue)"] avg(CounterValue), = ["percentile75(CounterValue)"] PERCENTILIS (ellenértéknek, 75), = ["max(CounterValue)"] bin (TimeGenerated, 1 óra), a számítógép által max(CounterValue) = |Óránkénti átlag, minimális, maximális és 75-PERCENTILIS CPU-használat egy adott számítógépen |
| A Teljesítményfigyelő &#124; Ha ObjectName == "MSSQL$ INST2: adatbázisok" és a példánynév == "master" | Minden teljesítményadat hello adatbázis teljesítményének hello főadatbázis hello SQL Server-példány INST2 nevű objektum.  

## <a name="viewing-performance-data"></a>Teljesítményadatok
Teljesítményadatok napló keresése futtatásakor hello **lista** nézet alapértelmezés szerint megjelenik.  tooview hello adatok grafikus képernyő, kattintson a **metrikák**.  Részletes grafikus nézetének, kattintson a hello  **+**  következő tooa számláló.  

![Összecsukott metrikák megtekintése](media/log-analytics-data-sources-performance-counters/metricscollapsed.png)

teljesítményadatok tooaggregate napló keresés, lásd: [igény metrika összevonásának és a képi megjelenítés OMS](http://blogs.technet.microsoft.com/msoms/2016/02/26/on-demand-metric-aggregation-and-visualization-in-oms/).


## <a name="next-steps"></a>Következő lépések
* [A teljesítményszámlálók adatainak összegyűjtése Linux alkalmazásokból](log-analytics-data-sources-linux-applications.md) többek között a MySQL és az Apache HTTP Server.
* További tudnivalók [keresések jelentkezzen](log-analytics-log-searches.md) tooanalyze hello adatokat gyűjteni az adatforrások és megoldásokat.  
* Összegyűjtött adatok exportálása túl[Power BI](log-analytics-powerbi.md) további képi megjelenítések és elemzésére.
