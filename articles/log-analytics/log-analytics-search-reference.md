---
title: "Naplóelemzési aaaAzure keresési referencia |} Microsoft Docs"
description: "hello Naplóelemzési keresési hivatkozás hello keresési nyelvi ismerteti és hello általános lekérdezési szintaxis beállítások akkor hasznosak, ha az adatok a Keresés és szűrés kifejezések toohelp szűkítse a keresést."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: 402615a2-bed0-4831-ba69-53be49059718
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7478a1139b88a1ce76ebb7b76027a6ccd66f4f27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-search-reference"></a>A Naplóelemzési hivatkozás keresése

>[!NOTE]
> Ez a cikk ismerteti a napló keresések Naplóelemzési hello aktuális lekérdezési nyelv használatával.  Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), akkor túl olvassa el[nyelvi referencia az új nyelvi hello hello](https://go.microsoft.com/fwlink/?linkid=856079).

hello keresési nyelvi kapcsolatos következő útmutató szakaszban hello általános lekérdezési szintaxis elérhető lehetőségeket ismerteti akkor hasznosak, ha az adatok a Keresés és szűrés kifejezések toohelp szűkítse a keresést. Azt is bemutatja, hogy művelettel tootake hello adatok, a parancsok.

Keresés által visszaadott hello mezők olvashat, és hello facets, amelyek segítenek derítse fel még alaposabban hello az adatok hasonló kategóriáinak [keresőmezőt és dimenzió hivatkozási szakasz](#search-field-and-facet-reference).

## <a name="general-query-syntax"></a>Általános lekérdezés szintaxisa
hello általános lekérdezése szintaxisa a következő:

```
filterExpression | command1 | command2 …
```

hello szűrőkifejezés (`filterExpression`) határozza meg a "where" feltétel hello lekérdezés hello. hello parancsok hello lekérdezés eredményének toohello alkalmazni. Több parancs hello függőleges vonal (|) kell elválasztani.

### <a name="general-syntax-examples"></a>Általános szintaxispéldáival
Példák:

```
system
```

A lekérdezés által visszaadott eredmények hello szót tartalmazó *rendszer* minden mezőben, amely a teljes szöveges indexelés vagy keresési feltételeket.

> [!NOTE]
> Nem minden mező így indexeli ugyan, de általános szöveges mezők (például a leírásokat és nevek) általában hello.
>
>

```
system error
```

A lekérdezés által visszaadott eredmények hello szavakat tartalmazó *rendszer* és *hiba*.

```
system error | sort ManagementGroupName, TimeGenerated desc | top 10
```

A lekérdezés által visszaadott eredmények hello szavakat tartalmazó *rendszer* és *hiba*. Majd rendezése hello eredmények hello *ManagementGroupName* mezőjét (növekvő sorrendben), és a majd hello *TimeGenerated* mezőjét (csökkenő sorrendben). Csak hello első 10 eredmények szükséges.

> [!IMPORTANT]
> Az összes hello mezőnevek és hello hello karakterlánc és a szöveges mezők értékei a kis-és nagybetűket.
>
>

## <a name="filter-expressions"></a>Szűrési kifejezésekben
a következő alszakaszokat hello hello szűrőkifejezéseket ismertetik.

### <a name="string-literals"></a>A szövegkonstansok
Karakterlánc bármilyen karakterlánc nem ismeri fel a hello elemző kulcsszó vagy egy előre definiált adattípus (például szám vagy dátum).

Példák:

```
These all are string literals
```

A lekérdezési eredmények, amelyek tartalmazzák az összes öt szavak előfordulását keresi. tooperform összetett karakterlánc keresés – hello karakterlánckonstansnak idézőjelek közé. Példa:

```
"Windows Server"
```

Ez az csak a pontos egyezés az eredményeket ad vissza *Windows Server*.

### <a name="numbers"></a>Számok
hello elemző numerikus mezők hello decimális egész és lebegőpontos szám szintaxis támogatja.

Példák:

```
Type:Perf 0.5
```

```
HTTP 500
```

### <a name="dates-and-times"></a>Dátum és idő
Minden adat hello rendszerben rendelkezik egy *TimeGenerated* tulajdonságot, amelynek hello eredeti dátum és idő hello rekord jelenti. Bizonyos típusú adatok lehetnek a további dátumot és időt tartalmazó mezőiben (például *LastModified*).

ütemterv hello **diagram és az időt** választó az Azure Naplóelemzés (függően toohello aktuális lekérdezés futtatása) időbeli eredmények terjesztési jeleníti meg. Ez a hello alapul *TimeGenerated* mező. Dátum-és egy adott karakterlánc-formátum használható lekérdezések toorestrict hello lekérdezés tooa adott időkereten rendelkezik. Szintaxis toorefer toorelative (például "közötti intervallumok 3 nappal ezelőtt és 2 óra telt el") is használhatja.

hello a következők szintaxis érvényes formáinak a dátum és idő:

```
yyyy-mm-ddThh:mm:ss.dddZ
```

```
yyyy-mm-ddThh:mm:ss.ddd
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm
```

```
yyyy-mm-dd
```


Példa:

```
TimeGenerated:2013-10-01T12:20
```

hello előző parancs csak rekordokat adja vissza egy *TimeGenerated* pontosan 12:20 2013. októberi 1 értéket.

hello elemző is támogatja a hello mnemonikus dátum/idő értékeknek most. (Nem valószínű, hogy az így kapott eredmények, mert az adatok nem teszik, hogy gyors hello rendszeren keresztül.)

Ezekben a példákban a relatív és abszolút dátumok építőelemeket toouse. A következő három alszakaszokat hello, láthatja, hogyan toouse azokat több speciális szűrők, példákkal relatív dátumtartományok használó.

### <a name="datetime-math"></a>Matematikai dátum és idő
Hello dátum/idő matematikai operátorok toooffset használja, vagy a ciklikus hello dátum/idő érték, egyszerű dátum/idő számítások használatával.

Szintaxis:

```
datetime/unit
```

```
datetime[+|-]count unit
```

| Operátor | Leírás |
| --- | --- |
| / |Kerekítés dátum/idő toohello megadott egység. Ha például most / nap kerekít hello aktuális dátum és idő toomidnight hello az aktuális nap. |
| + vagy - |Eltolások dátum/idő hello által megadott egységek száma. Például most + 1 óra tolódnak hello jelenlegi dátum és idő egy órával előre. 2013-10-01T12:00-10 nap hello dátumérték tolódnak 10 nap által. |

Hello dátum/idő matematikai operátorok együtt is láncában találhatók. Példa:

```
NOW+1HOUR-10MONTHS/MINUTE
```

hello következő táblázatban hello támogatott dátum/idő egység.

| Dátum és idő egység | Leírás |
| --- | --- |
| ÉV, ÉV |Kerekítés toocurrent év vagy eltolások hello által megadott számú éven belülre esik. |
| HÓNAP, HÓNAP |Kerekítés toocurrent hónapban, vagy eltolások hello által megadott hónapok száma. |
| NAPJA, NAP, DÁTUM |Kerekítés toocurrent napja hello hónapban, vagy eltolások hello által megadott napok száma. |
| ÓRA, ÜZEMIDEJE (ÓRA) |Kerekítés toocurrent óra vagy eltolások hello által megadott órák száma. |
| PERC, A PERC |Kerekítés toocurrent perc vagy eltolások hello által megadott számú perc. |
| MÁSODIK, MÁSODPERCBEN |Második kerekít toocurrent, vagy tolódnak el a megadott hello másodpercek száma. |
| EZREDMÁSODPERCES, EZREDMÁSODPERC, MILLI, MILLIS |Kerekítés toocurrent ezredmásodperces vagy eltolások hello által megadott ideje (MS). |

### <a name="field-facets"></a>A mező facets
Mező értékkorlátozás használatával hello keresési feltétel mezőiről és a pontos értékeket is megadhat. Ez eltér "szabad szöveg" lekérdezések írásáról hello index egész különböző feltételek. Ezzel a módszerrel több előző példákban már láthatta. Az alábbiakban hello összetettebb példák.

**Szintaxis**

```
field:value
```

```
field=value
```

**Leírás**

A keresések hello mező hello megadott értéket. a literál karakterlánc, szám vagy dátumot és időpontot hello érték lehet.

Példa:

```
TimeGenerated:NOW
```

```
ObjectDisplayName:"server01.contoso.com"
```

```
SampleValue:0.3
```

**Szintaxis**

*a mező > értéke*

*a mező < érték*

*a mező > = érték*

*a mező < érték =*

*a mező! érték =*

**Leírás**

Itt összehasonlítást.

Példa:

```
TimeGenerated>NOW+2HOURS
```

**Szintaxis**

```
field:[from..to]
```

**Leírás**

Tartomány értékkorlátozás biztosít.

Példa:

```
TimeGenerated:[NOW..NOW+1DAY]
```

```
SampleValue:[0..2]
```

### <a name="in"></a>IN
Hello **IN** kulcsszó lehetővé teszi a tooselect értékek listájából. Attól függően, hogy hello szintaxisnak használ ez lehet egy egyszerű, adjon meg értékeket, vagy összesítést értékeinek listáját.

1. szintaxis:

```
field IN {value1,value2,value3,...}
```

Ez a szintaxis lehetővé teszi egy egyszerű lista összes értékei tooinclude.



Példák:

```
EventID IN {1201,1204,1210}
```

```
Computer IN {"srv01.contoso.com","srv02.contoso.com"}
```

2. szintaxis:

```
(Outer query) (Field toouse with inner query results) IN {Inner query | measure count() by (Field toosend tooouter query)} (rest  of outer query)  
```

Ez a szintaxis lehetővé teszi toocreate összesítést. Hello értékekből álló listát is majd hírcsatornát, hogy az adott összesítési be egy másik külső (elsődleges) keresési, amely megkeresi az adott értékkel rendelkező események. Ehhez kapcsos zárójelek hello belső keresési befoglaló és az eredményeket, a lehetséges értékek a hello külső keresési mező hello IN operátor használatával.

Belső lekérdezés példa: *jelenleg hiányzó biztonsági frissítéseket számítógépek* a hello összesítési lekérdezés a következő:

```
Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer
```    

hello utolsó lekérdezési megtalált *számítógépek jelenleg hiányzó biztonsági frissítéseket az összes Windows-események* hello következőhöz hasonló:

```
Type=Event Computer IN {Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer}
```

### <a name="contains"></a>Contains
Hello **Contains** kulcsszó lehetővé teszi a toofilter rekordok a megadott karakterlánc tartalmazó mezőt. Ez a nagybetűk különbözőnek számítanak, csak a karakterlánc típusú működik, és nem tartalmazhatnak egyetlen escape-karakter.

Szintaxis:

```
field:contains("string")
```

Példa:

```
Type:contains("Event")
```

Ez adja vissza, amely tartalmazza a hello karakterlánc típusú rekordok "Event". Példák **esemény**, **SecurityEvent**, és **ServiceFabricOperationEvent**.



### <a name="regular-expressions"></a>Reguláris kifejezések
Hello segítségével a reguláris kifejezéssel, mező a keresési feltételt is megadhat **Regex** kulcsszó. Hello szintaxis segítségével is reguláris kifejezések teljes leírását lásd: [reguláris kifejezések toofilter napló keresések használatát Naplóelemzési](log-analytics-log-searches-regex.md).

Szintaxis:

```
field:Regex("Regular Expression")
```

Példa:

```
Computer:Regex("^C.*")
```

### <a name="logical-operators"></a>Logikai operátorok
logikai operátorok hello támogatott nyelvek hello lekérdezés (*és*, *vagy*, és *nem*) és a C-stílusú (*&&*,  *||* , és *!*, illetve). Ezen operátorok használható kerek zárójeleket tartalmazhatnak toogroup.

Példák:

```
system OR error

```

```
Type:Alert AND NOT(Severity:1 OR ObjectId:"8066bbc0-9ec8-ca83-1edc-6f30d4779bcb8066bbc0-9ec8-ca83-1edc-6f30d4779bcb")
```

Hello logikai operátor hello legfelső szintű szűrő argumentumot, akkor kihagyhatja. Ebben az esetben hello és operátor feltételezi.

| Szűrőkifejezés | Egyenértékű túl|
| --- | --- |
| Rendszerhiba: |a rendszer és hiba |
| a rendszer "Windows Server" vagy Súlyosság: 1 |rendszer- és ("Windows Server" vagy Súlyosság: 1) |

### <a name="wildcarding"></a>Helyettesítő karakterek használata
hello lekérdezési nyelv támogatja a hello ( \* ) karakter túl határoz meg egy értéket a lekérdezés egy vagy több karaktert.

Példa:

 Az "SQL" minden számítógép található hello nevét, például a "Redmond-SQL".

```
Type=Event Computer=*SQL*
```

> [!NOTE]
> Jelenleg nem helyettesítő ajánlatok belül. Például üdvözlőüzenetére `"*This text*"` figyelembe veszi a hello (\*) literálként használt (\*) karaktert.


## <a name="commands"></a>Parancsok


hello parancsok hello lekérdezés által visszaadott eredmények toohello alkalmazni. Használjon hello cső karakter (|) tooapply egy parancs toohello eredmények lekérése. Több parancs hello függőleges vonal kell elválasztani.

> [!NOTE]
> Parancs nevek nagybetű vagy kisbetű, eltérően hello mezőneveket és hello adatokat is beírhatók.
>
>

### <a name="sort"></a>Rendezés
Szintaxis:

    sort field1 asc|desc, field2 asc|desc, …

Hello eredmények adott mezők szerint rendezi. hello növekvő/csökkenő utótag toosort hello eredmények növekvő vagy csökkenő sorrendben nem kötelező megadni. Ha ki van hagyva, hello *asc* rendezési sorrend feltételezi. A hello **TimeGenerated** mezőben *desc* rendezési sorrend feltételezi, így az eredményeket ad vissza hello legutóbbi először alapértelmezés szerint.

### <a name="toplimit"></a>Felső/korlátot
Szintaxis:

    top number


    limit number
Korlátok hello válasz toohello felső N eredmények.

Példa:

    Type:Alert errors detected | top 10

Vissza a megfelelő eredményeket felső 10 hello.

### <a name="skip"></a>Kihagyás
Szintaxis:

    skip number

Átugrása hello felsorolt eredmények száma.

Példa:

    Type:Alert errors detected | top 10 | skip 200

200 eredmény kezdődő felső 10 megfelelő eredményeket ad vissza.

### <a name="select"></a>Válassza ezt:
Szintaxis:

    select field1, field2, ...

Korlátozza az eredmények toohello mezők választja.

Példa:

    Type:Alert errors detected | select Name, Severity

Korlátok hello visszaadott eredmények mezők túl*neve* és *súlyossági*.

### <a name="measure"></a>mértékcsoport
Hello *mérték* parancs használt tooapply statisztikai függvények toohello nyers keresési eredmények között. Ez a nagyon hasznos tooget *csoportosítási* hello adatok nézetek. Hello mérték parancs használatakor a Log Analyticshez keresési összesített eredmények tábla jeleníti meg.

**Szintaxis:**

    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]] by groupField1 [, groupField2 [, groupField3]]  [interval interval]


    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]]  interval interval



Összesíti az eredményeket hello *csoportosítási mező*, és összesítve hello mérték értékek alapján számítja ki *aggregatedField*.

| Mérték statisztikai függvény | Leírás |
| --- | --- |
| *aggregateFunction* |hello neve hello összesítő függvény (kis-és nagybetűket). a következő aggregátumfüggvények hello támogatottak: száma, MAX, MIN, SUM, AVG, szórás, COUNTDISTINCT, percentilis ## vagy PCT ## (## 1 és 99 közötti érték). |
| *aggregatedField* |hello mezője, amely összesítve van folyamatban. Ez a mező esetén hello összesítő függvény a nem kötelező, de vannak a toobe egy meglévő számmező SUM, MAX, MIN, AVG, szórás, percentilis ## vagy PCT ## (## 1 és 99 közötti érték). hello aggregatedField is lehet bármely hello **bővítése** támogatott függvényeket. |
| *fieldAlias* |(választható) alias hello hello számított összesített értéket. Nincs megadva, hogy van-e a hello mezőnév **AggregatedValue**. |
| *csoportosítási mező* |hello hello mező neve, amely hello eredményhalmaz szerint vannak csoportosítva. |
| *Időköz* |hello alatt az időtartam alatt, hello formátumban:**nnnNAME**. **nnn**az hello pozitív egész szám. **NÉV** hello időköz neve. Támogatott intervallumnál nevek kis-és nagybetűket, és a következőket tartalmazzák: másodperc [S], [S] EZREDMÁSODPERCES perc [S], [S] ÓRÁBAN nap [S], [S] hónap és év [S]. |

hello intervallum-beállítás csak akkor használható dátum/idő csoport mezőket (például *TimeGenerated* és *TimeCreated*). Jelenleg ez nem kényszeríti ki hello szolgáltatást, de nélküli dátum/idő toohello háttérben átadott mező futásidejű hibát okoz. Hello sémaérvényesítés megadásuk hello szolgáltatás API elutasítja a lekérdezéseket, amelyekkel a dátum/idő nélkül mezők időköz összesítés céljából. aktuális hello *mérték* implementációja időköz csoportosítási támogat minden összesítő függvény.

Ha hello BY záradék meg van adva, de időközt (mint a második szintaxis) van megadva, hello *TimeGenerated* alapértelmezés szerint feltételezett mező.

Példák:

**1. példa**

    Type:Alert | measure count() as Count by ObjectId

Csoportok hello riasztások szerint *ObjectID*, és kiszámítja az egyes riasztások hello számát. hello összesített értéket adja vissza a rendszer hello *száma* mező (másodlagos).

**2. példa**

    Type:Alert | measure count() interval 1HOUR

Hello segítségével csoportok hello az 1 órás időközönként riasztások *TimeGenerated* mező, és minden időszakban riasztások hello számát adja vissza.

**3. példa**

    Type:Alert | measure count() as AlertsPerHour interval 1HOUR

Azonos hello előző példaként, de egy összesített mező aliasnév (*AlertsPerHour*).

**4. példa**

    * | mérték count() TimeCreated időköz 5DAYS által

Csoportosítja hello eredményeket 5 napos időközönként hello segítségével *TimeCreated* mező, és minden időköz eredmények hello számát adja vissza.

**5. példa**

    Type:Alert | measure max(Severity) by WorkflowName

Csoportok riasztások hello munkaterhelés név szerint, és értéket ad vissza hello minden munkafolyamat maximális riasztási fontossági értékét.

**6. példa**

    Type:Alert | measure min(Severity) by WorkflowName

Ugyanaz, mint hello előző példa, de hello *min* függvény összesíteni.

**7 – példa**

    Type:Perf | measure avg(CounterValue) by Computer

Számítógép által telj csoportok, és kiszámítja hello átlagos (átlag).

**8 – példa**

    Type:Perf | measure sum(CounterValue) by Computer

Ugyanaz, mint az előző példában hello, de használja *sum*.

**Példa 9**

    Type:Perf | measure stddev(CounterValue) by Computer

Ugyanaz, mint az előző példában hello, de használja *stddev*.

**10 – példa**

    Type:Perf | measure percentile70(CounterValue) by Computer

Ugyanaz, mint az előző példában hello, de használja *percentile70*.

**11 – példa**

    Type:Perf | measure pct70(CounterValue) by Computer

Ugyanaz, mint az előző példában hello, de használja *pct70*. Vegye figyelembe, hogy *PCT ##* aliasneve csak *PERCENTILIS ##* függvény.

**Példa 12**

    Type:Perf | measure avg(CounterValue) by Computer, CounterName

A Teljesítményfigyelő először csoportosítása számítógép majd CounterName, és kiszámítja hello átlagos (átlag).

**13 – példa**

    Type:Alert | measure count() as Count by WorkflowName | sort Count desc | top 5

Lekérdezi a hello hello riasztások maximális száma öt legfontosabb munkafolyamatok.

**14 – példa**

    * | mérték countdistinct(Computer) típus szerint

Megjeleníti az egyes reporting egyedi számítógépek hello számát.

**15 – példa**

    * | mérték countdistinct(Computer) időköz 1 óra

Megjeleníti a minden órában jelentett egyedi számítógépek hello számát.

**16 – példa**

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total” | measure avg(CounterValue) by Computer Interval 1HOUR
```

Csoportosítja a számítógép a processzoridő %, és óránként hello átlagát adja vissza.

**17 – példa**

    Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES

W3CIISLog eljárás alapján csoportosítja, és adja vissza hello maximális 5 percenként.

**18 – példa**

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer Interval 1HOUR
```

Csoportok %-ban a processzoron számítógép, és vissza hello minimális, átlagos, 75. percentilis és maximális óránkénti.

**19 – példa**

```
Type:Perf CounterName=”% Processor Time”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer, InstanceName Interval 1HOUR
```

Csoportok processzoridő % először számítógép majd a példány nevét, és vissza hello minimális, átlagos, 75. percentilis és maximális óránkénti.

**20 – példa**

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | measure max(product(CounterValue,60)) as MaxDWPerMin by InstanceName Interval 1HOUR
```

Kiszámítja a hello maximális Lemezírások / perc minden lemez a számítógépen.

### <a name="where"></a>Ha
Szintaxis:

```
**where** AggregatedValue>20
```

Csak akkor használható után egy *mérték* parancs toofurther szűrő hello összesített értéket annak az eredménye, hogy hello *mérték* aggregátumfüggvény által előállított.

Példák:

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) as MAXCPU by Computer

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) by Computer | where (AggregatedValue>50 and AggregatedValue<90)



### <a name="dedup"></a>A Deduplikáció
Szintaxis:

    Dedup FieldName

Minden egyedi értéket a megadott mező hello található hello első dokumentumát adja vissza.

Példa:

    Type=Event | Dedup EventID | sort TimeGenerated DESC

Ez a példa egy esemény (hello utolsó esemény) / EventID adja vissza.

### <a name="join"></a>Csatlakozás
Illesztések hello két lekérdezést tooform egyetlen eredményeit set vezethet.  Hello ismertetett többféle illesztési típust támogatja hajtsa végre a táblában.

| Összekapcsolás típusa | Leírás |
|:--|:--|
| belső | Mindkét lekérdezések csak rögzíti a megfelelő érték visszaadása. |
| külső | Mindkét lekérdezések összes rekordját adja vissza.  |
| balra  | Bal oldali lekérdezés és az adatokat a megfelelő lekérdezés összes rekordját adja vissza. |


- Illesztések jelenleg nem támogatja lekérdezések hello tartalmazó **IN** kulcsszót, hello **mérték** parancs vagy hello **bővítése** parancsot, ha azt célozza hello jobb lekérdezésből származó mezőt.
- Csak egy mező jelenleg szerepel a csatlakozzon.
- Egyetlen keresés nem tartozhat egynél több illesztési.

**Szintaxis**

```
<left-query> | JOIN <join-type> <left-query-field-name> (<right-query>) <right-query-field-name>
```

**Példák**

tooillustrate hello különböző csatlakozási típusok, fontolja meg a hello szívverés számítógépenként MyBackup_CL nevű egyéni napló gyűjtött való csatlakozás adattípust.  Ezek eltérőek a következő adatok hello rendelkezik.

`Type = MyBackup_CL`

| TimeGenerated | Computer | LastBackupStatus |
|:---|:---|:---|
| 4/20/2017 01:26:32.137 AM | Srv01.contoso.com | Sikeres |
| 4/20/2017 02:13:12.381 AM | srv02.contoso.com | Sikeres |
| 4/20/2017 02:13:12.381 AM | srv03.contoso.com | Hiba |

`Type = Hearbeat`(Csak megjelenjen egy része)

| TimeGenerated | Computer | ComputerIP |
|:---|:---|:---|
| 4/21/2017 12:01:34.482 PM | Srv01.contoso.com | 10.10.100.1 |
| 4/21/2017 12:02:21.916 PM | srv02.contoso.com | 10.10.100.2 |
| 4/21/2017 12:01:47.373 PM | srv04.contoso.com | 10.10.100.4 |

#### <a name="inner-join"></a>belső illesztés

`Type=MyBackup_CL | join inner Computer (Type=Heartbeat) Computer`

A következő rekordok Ha hello számítógép mező megegyezik a mindkét adattípusok hello adja vissza.

| Computer| TimeGenerated | LastBackupStatus | TimeGenerated_joined | ComputerIP_joined | Type_joined |
|:---|:---|:---|:---|:---|:---|
| Srv01.contoso.com | 4/20/2017 01:26:32.137 AM | Sikeres | 4/21/2017 12:01:34.482 PM | 10.10.100.1 | Szívverés |
| srv02.contoso.com | 4/20/2017 02:13:12.381 AM | Sikeres | 4/21/2017 12:02:21.916 PM | 10.10.100.2 | Szívverés |


#### <a name="outer-join"></a>külső illesztés

`Type=MyBackup_CL | join outer Computer (Type=Heartbeat) Computer`

A következő rekordok mindkét adattípusok a hello adja vissza.

| Computer| TimeGenerated | LastBackupStatus | TimeGenerated_joined | ComputerIP_joined | Type_joined |
|:---|:---|:---|:---|:---|:---|
| Srv01.contoso.com | 4/20/2017 01:26:32.137 AM | Sikeres  | 4/21/2017 12:01:34.482 PM | 10.10.100.1 | Szívverés |
| srv02.contoso.com | 4/20/2017 02:14:12.381 AM | Sikeres  | 4/21/2017 12:02:21.916 PM | 10.10.100.2 | Szívverés |
| srv03.contoso.com | 4/20/2017 01:33:35.974 AM | Hiba  | 4/21/2017 12:01:47.373 PM | | |
| srv04.contoso.com |                           |          | 4/21/2017 12:01:47.373 PM | 10.10.100.2 | Szívverés |



#### <a name="left-join"></a>bal oldali illesztés

`Type=MyBackup_CL | join left Computer (Type=Heartbeat) Computer`

A MyBackup_CL a szívverés a megfelelő mezőket a következő rekordok hello adja vissza.

| Computer| TimeGenerated | LastBackupStatus | TimeGenerated_joined | ComputerIP_joined | Type_joined |
|:---|:---|:---|:---|:---|:---|
| Srv01.contoso.com | 4/20/2017 01:26:32.137 AM | Sikeres | 4/21/2017 12:01:34.482 PM | 10.10.100.1 | Szívverés |
| srv02.contoso.com | 4/20/2017 02:13:12.381 AM | Sikeres | 4/21/2017 12:02:21.916 PM | 10.10.100.2 | Szívverés |
| srv03.contoso.com | 4/20/2017 02:13:12.381 AM | Hiba | | | |


### <a name="extend"></a>Bővíthető
Lehetővé teszi a toocreate futásidejű mezők lekérdezésekben. Vegye figyelembe, hogy futásidőben mezők hello mérték parancs tooperform összesítés nem használható.

**1. példa**

    Type=SQLAssessmentRecommendation | Extend product(RecommendationScore, RecommendationWeight) AS RecommendationWeightedScore
Jeleníti meg súlyozott SQL-kiértékelés ajánlásai javaslat pontszámot.

**2. példa**

    Type=Perf CounterName="Private Bytes" | EXTEND div(CounterValue,1024) AS KBs | Select CounterValue,Computer,KBs
A számláló értéke mutatja KB bájt helyett.

**3. példa**

    Type=WireData | EXTEND scale(TotalBytes,0,100) AS ScaledTotalBytes | Select ScaledTotalBytes,TotalBytes | SORT TotalBytes DESC
Méretezik hello WireData TotalBytes értékét, úgy, hogy minden eredmény 0 és 100 közötti legyenek.

**4. példa**

```
Type=Perf CounterName="% Processor Time" | EXTEND if(map(CounterValue,0,50,0,1),"HIGH","LOW") as UTILIZATION
```
A Teljesítményfigyelő számlálók értékeit kisebb, mint 50 %-a alacsony, és más, magas címkéket.

**5. példa**

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | Extend product(CounterValue,60) as DWPerMin| measure max(DWPerMin) by InstanceName Interval 1HOUR
```
Kiszámítja a hello maximális Lemezírások / perc minden lemez a számítógépen.

**Támogatott funkciók**

| Függvény | Leírás | Szintaxispéldáival |
| --- | --- | --- |
| ABS |Hello abszolút értékét adja vissza hello megadott értéket vagy függvény. |`abs(x)` <br> `abs(-5)` |
| ARCCOS |Arkusz koszinusz az értékre, vagy egy olyan függvényt ad vissza. |`acos(x)` |
| és |Igaz értéket ad eredményként, csak ha összes operandus tootrue kiértékelése. |`and(not(exists(popularity)),exists(price))` |
| ARCSIN |Arkusz szinusz az értékre, vagy egy olyan függvényt ad vissza. |`asin(x)` |
| Atan |Arkusz tangens az értékre, vagy egy olyan függvényt ad vissza. |`atan(x)` |
| ARCTAN2 |Hello átalakítás hello téglalap alakú koordinátának x, y koordinátáit-toopolar eredő hello szöget adja vissza. |`atan2(x,y)` |
| cbrt |Adatkocka gyökere. |`cbrt(x)` |
| ceil |Tooan egész szám felfelé kerekítése a. |`ceil(x)`  <br> `ceil(5.6)`6 beolvasása |
| Cos |Az szög koszinuszát adja vissza. |`cos(x)` |
| COSH |A szög koszinusz hiperbolikuszát adja vissza. |`cosh(x)` |
| def |Alapértelmezett rövidítése. Vissza a mező "mező értékének" hello. Ha hello mező nem létezik, a megadott hello alapértelmezett értéket ad vissza, és hello első értékét adja eredményül adott: `exists()==true`. |`def(rating,5)`. A def() függvény adja vissza a minősítése hello, vagy ha minősítés nélküli van megadva a hello dokumentum adja vissza az 5. <br> `def(myfield, 1.0)`az egyenértékű túl`if(exists(myfield),myfield,1.0)`. |
| fok |Radiánban megadott szög toodegrees alakítja. |`deg(x)` |
| DIV |`div(x,y)`osztja x, y által. |`div(1,y)` <br> `div(sum(x,100),max(y,1))` |
| ELOSZLÁS |Két veszélyének, (pont) n dimenziós térben hello távolság visszaadása. Hello power, valamint két vagy több, ValueSource példányát veszi, és kiszámítja hello távolságát hello két veszélyének. Minden egyes ValueSource számnak kell lennie. Az átadott ValueSource példányok páros számú kell lennie, és hello metódus feltételezi, hogy hello első félig képviselő hello első vektor, és hello második fele ismereti képviselő hello második vektoros. |`dist(2, x, y, 0, 0)`Kiszámítja a hello Euclidean közötti távolság szerint (0,0) és (x, y) minden dokumentumhoz. <br> `dist(1, x, y, 0, 0)`Kiszámítja a hello Manhattan (taxicab) távolságát (0,0) és (x, y) minden dokumentumhoz. <br> `dist(2,,x,y,z,0,0,0)`(0,0,0) Euclidean távolságát és (x, y, z) minden dokumentumhoz.<br>`dist(1,x,y,z,e,f,g)`Manhattan közötti távolság (x, y, z) és (e, f, g), ahol minden levél-e a mező nevét. |
| létezik-e |Értéket ad vissza IGAZ, ha bármelyik tag hello mező létezik. |`exists(author)`Bármely dokumentum, amely hello "Szerző" mező értéke IGAZ értéket ad vissza.<br>`exists(query(price:5.00))`Eredménye IGAZ, ha a megfelelő "price", "5,00". |
| Exp |Számát adja vissza Euler kiváltott toopower x. |`exp(x)` |
| Emelet |Tooan egész szám lefelé kerekítése a. |`floor(x)`  <br> `floor(5.6)`5 beolvasása |
| hypo |Sqrt(sum(pow(x,2),pow(y,2))) köztes túlcsordulás vagy alulcsordulás történt nélkül adja vissza. |`hypo(x,y)`  <br> ` |
| Ha |Feltételes függvény lekérdezéseket tesz lehetővé. A `if(test,value1,value2)`, tesztelési vagy hivatkozik tooa logikai érték vagy kifejezés, amely egy logikai (IGAZ vagy hamis) értéket ad vissza. `value1`hello értéket ad vissza hello függvény Ha a vizsgálat igaz értéket adja eredményül. `value2`hello értéket ad vissza hello függvény Ha a vizsgálat hamis adja eredményül. Egy kifejezés lehet bármely funkció, amely a logikai értékek kimenete. Egy függvény numerikus értékeket, amelyben esetértéket 0 hamis kerül értelmezésre, vagy visszaadó karakterláncok, mely esetben üres karakterláncot a értelmezi hamis értéket ad vissza is lehet. |`if(termfreq(cat,'electronics'),popularity,42)`Ez a függvény minden egyes dokumentum toosee ellenőrzi, hogy a hello kifejezés "electronics" hello cat mezőben tartalmaz. Ha, majd hello hello népszerűségét mező értéket adja vissza. Ellenkező esetben a 42 hello értéket adja vissza. |
| lineáris |Megvalósítja `m*x+c`, ahol m és c állandók és a x egy tetszőleges függvényben. Ez az egyenértékű túl`sum(product(m,x),c)`, de nagyobb jelentőséggel hatékony, mint egy funkcióval megvalósuló. |`linear(x,m,c) linear(x,2,4)`adja vissza`2*x+4` |
| ln |Beolvasása hello természetes naplóját hello megadott függvény. |`ln(x)` |
| Napló |Beolvasása hello napló hello alap 10 megadott függvény. |`log(x)   log(sum(x,100))` |
| térkép |A Maps bármely egy bemeneti függvény x minimális és maximális, a határokat is beleértve toohello megadott cél belül eső értékeket. hello argumentumok minimális és maximális állandónak kell lennie. hello argumentumok cél- és alapértelmezett lehet állandók vagy funkciók. Ha x hello értéke nem tartozik a minimális és maximális között, majd az x vagy hello értéket adja vissza, vagy egy alapértelmezett értéket adja vissza, ha meg van adva az 5. argumentuma. |`map(x,min,max,target) map(x,0,0,1)`Módosítja a 0 too1 az értékeket. Ez az alapértelmezett 0 értékek kezelése hasznos lehet.<br> `map(x,min,max,target,default)    map(x,0,100,1,-1)`Módosítja az 0 és 100 too1, és minden egyéb értékek túl-1 közötti értékeket.<br>  `map(x,0,100,sum(x,599),docfreq(text,solr))`Minden olyan értéket 0 és 100 toox + 599 és minden egyéb értékek toofrequency hello kifejezés "solr" hello mező szöveg megváltozik. |
| maximális |Értéket ad vissza a legnagyobb numerikus értéket több beágyazott függvények vagy az állandókat, mint szerepkör argumentumokban megadott hello: `max(x,y,...)`. hello max függvény is hasznos lehet a "bottoming" egy másik függvényt, vagy néhány mező megadott konstans.  Használjon hello `field(myfield,max)` szintaxis egy többértékű mező hello maximális értékét kijelöléséhez. |`max(myfield,myotherfield,0)` |
| perc |Vissza az állandók, mint szerepkör argumentumokban megadott több beágyazott függvények legkisebb numerikus értékének hello: `min(x,y,...)`. hello min függvénnyel is hasznos lehet az állandó használatával függvényt a "felsőé" biztosít. Használjon hello `field(myfield,min)` szintaxis egy többértékű mező hello minimális értékét kijelöléséhez. |`min(myfield,myotherfield,0)` |
| MOD |Kiszámítja a hello modulus hello függvény x hello függvény y által. |`mod(1,x)` <br> `mod(sum(x,100), max(y,1))` |
| MS |Az argumentumok között különbséget ezredmásodperc adja vissza. Dátumok olyan Unix- vagy POSIX relatív toohello idő epoch, éjféli, 1970. január 1 UTC. Argumentumok indexelt TrieDateField, vagy állandó dátum alapján dátum matematikai hello neve vagy most is. `ms()`az egyenértékű túl`ms(NOW)`, mert hello epoch idő ezredmásodpercben. `ms(a)`hello epoch hello argumentum jelölő óta ezredmásodperc hello számát adja eredményül. `ms(a,b)`számot ad vissza, hello ideje (MS), hogy a b előtt következik be, amely `a - b`. |`ms(NOW/DAY)`<br>`ms(2000-01-01T00:00:00Z)`<br>`ms(mydatefield)`<br>`ms(NOW,mydatefield)`<br>`ms(mydatefield,2000-01-01T00:00:00Z)`<br>`ms(datefield1,datefield2)` |
| nem |hello logikailag negated hello értékének függvény burkolva. |`not(exists(author))`IGAZ, csak ha `exists(author)` értéke "false". |
| vagy |Logikai vagy műveletet. |`or(value1,value2)`Értéke TRUE, ha bármelyik value1 és value2 értéke true. |
| Pow |A megadott alap toohello megadott vált ki hello power. `pow(x,y)`y x toohello hatványa riasztást. |`pow(x,y)`<br>`pow(x,log(y))`<br>`pow(x,0.5)`ugyanaz, mint sqrt hello. |
| A termék |Beolvasása hello több értékek vagy funkciókat, amelyeket egy vesszővel elválasztott listában. `mul(...)`használható aliasként ennél a függvénynél. |`product(x,y,...)`<br>`product(x,2)`<br>`product(x,y)`<br>`mul(x,y)` |
| recip |Kölcsönös végzi a `recip(x,m,a,b)` végrehajtási `a/(m*x+b)`, ahol m, a, b állandók és x az önkényesen összetett függvényeket. Ha a és b egyenlő és x > = 0, a függvény, amely elutasítja azokat a növekszik x 1 maximális értéke. Hello értékének növelésével a és b együtt eredményeket egy hello teljes függvény tooa mozgása laposabb hello görbének. Ezek a Tulajdonságok hozzárendelheti ezt az ideális függvény kiemelése legfrissebb, ha x `rord(datefield)`. |`recip(myfield,m,a,b)`<br>`recip(rord(creationDate),1,1000,1000)` |
| RAD |Fok tooradians alakítja. |`rad(x)` |
| Nyomtatás |A legközelebbi egész számra kerekít toohello. |`rint(x)`  <br> `rint(5.6)`6 beolvasása |
| EG |Egy szög szinuszát adja vissza. |`sin(x)` |
| SINH |A szög szinusz hiperbolikuszát adja vissza. |`sinh(x)` |
| Skála |Hello függvény x értékek méretezik, például azok hello közé eső megadott minTarget és maxTarget határokat is beleértve. hello jelenlegi implementációja áthaladó összes hello függvény értékek tooobtain hello min és max, így kiválaszthatja a megfelelő méretezés hello. hello jelenlegi implementációja nem különbözteti meg, amikor dokumentumok törölték, vagy dokumentumok érték nélküli. Ezekben az esetekben 0.0 értékeket használ. Ez azt jelenti, hogy ha általában az összes nagyobb, mint a 0,0 érték, egy is továbbra is végül 0,0, minimális érték toomap a hello. Ezekben az esetekben, egy megfelelő `map()` függvény használható a megoldás toochange 0,0 tooa értéknek hello valós közé, ahogy az itt látható:`scale(map(x,0,0,5),1,2)` |`scale(x,minTarget,maxTarget)`<br>`scale(x,1,2)`Skála hello, az x értékek, úgy, hogy az összes értékei 1 és 2 között lehet. |
| Sqrt |Hello négyzetgyökét adja vissza a hello megadott értéket vagy függvény. |`sqrt(x)`<br>`sqrt(100)`<br>`sqrt(sum(x,100))` |
| strdist |Két karakterlánc hello távolságát számítja ki. Hello Lucene helyesírás-ellenőrző futtatásához, StringDistance felületet használja, és támogatja az összes hello megvalósítások az adott csomagban érhető el. Alkalmazások tooplug is lehetővé teszi a saját, Solr tartozó erőforrás keresztül betöltése képességeit. strdist veszi `(string1, string2, distance measure)`. Távolság mérték lehetséges értékei:<ul><li>jw: Jaro-Winkler</li><li>Szerkesztés: Levenstein vagy Szerkesztés távolság</li><li>ngram: hello NGramDistance, ha meg van adva, opcionálisan teljen hello ngram mérete túl. Alapértelmezett érték a 2.</li><li>FQN: Teljesen minősített osztály egy hello StringDistance illesztőfelület megvalósítása nevét. Nem-arg konstruktorral kell rendelkezniük.</li></ul> |`strdist("SOLR",id,edit)` |
| Sub |Visszaadja az x-y `sub(x,y)`. |`sub(myfield,myfield2)`<br>`sub(100,sqrt(myfield))` |
| Sum |Beolvasása hello összege több értékek vagy funkciókat, amelyeket egy vesszővel elválasztott listában. `add(...)`Előfordulhat, hogy használható aliasként ennél a függvénynél. |`sum(x,y,...)`<br>`sum(x,1)`<br>`sum(x,y)`<br>`sum(sqrt(x),log(y),z,0.5)`<br>`add(x,y)` |
| termfreq |Hello kifejezés megjelenik a dokumentum hello mező hányszor hello számát adja vissza. |termfreq(Text,'memory') |
| Tan |Az szög tangensét adja vissza. |`tan(x)` |
| TANH |A szög tangens hiperbolikuszát adja vissza. |`tanh(x)` |

## <a name="search-field-and-facet-reference"></a>Keresési mező, és a dimenzió hivatkozási
Naplófájl-keresési toofind adatainak használatakor eredményekben különböző mező és értékkorlátozást. Hello adatok egy részét nem jelenhet meg nagyon leíró. A következő információk toohelp hello eredmények tisztában hello használata.

| Mező | Keresés típusa | Leírás |
| --- | --- | --- |
| A TenantId |Összes |Használt toopartition adatokat. |
| TimeGenerated |Összes |Toodrive hello ütemterv timeselectors (keresési és egyéb képernyők) használt. Azt jelöli, amikor hello adat jött létre (általában a hello ügynök). hello idő ISO-formátumban van kifejezve, és mindig UTC. Hello esetében, amelyek a meglévő instrumentation (Ez azt jelenti, hogy a napló események) alapuló Ez a művelet rendszerint hello valós időben adott hello bejegyzés/sor/naplóbejegyzés naplózott. Egyes hello más típusokat, amelyek a felügyeleti csomagok segítségével vagy hello felhőben (például javaslatok vagy riasztások) előállított hello valami más időt jelöli. Ez az hello idő, amikor az új adat pillanatképet a konfigurációról valamiféle az összegyűjtött, vagy a javaslat riasztások előállítása azt alapján. |
| Eseményazonosító |Esemény |Eseményazonosító hello Windows-eseménynaplóban találhatók. |
| Az eseménynaplóban talál |Esemény |Ahol a hello esemény naplózva lett a Windows eseménynaplójában. |
| EventLevelName |Esemény |Kritikus vagy figyelmeztetési/információk/sikeres |
| EventLevel |Esemény |A kritikus vagy figyelmeztetési/információk/sikeres számérték (inkább EventLevelName könnyebb/olvashatóbb lekérdezések). |
| SourceSystem |Összes |Ha hello adatok származik (a mód toohello szolgáltatás csatolása). Ilyen például a Microsoft System Center Operations Manager és az Azure Storage. |
| ObjectName |PerfHourly |Windows teljesítményobjektum nevét. |
| Példánynév |PerfHourly |Windows teljesítményszámláló példány neve. |
| CounteName |PerfHourly |Windows teljesítményszámláló neve. |
| ObjectDisplayName |PerfHourly, ConfigurationAlert, ConfigurationObject, ConfigurationObjectProperty |A teljesítményadat-gyűjtési szabály az Operations Manager által megcélzott hello objektum megjelenített neve. Operational Insights által felderített hello objektum hello megjelenítendő nevét is lehet, vagy mely hello elleni riasztást. |
| RootObjectName |PerfHourly, ConfigurationAlert, ConfigurationObject, ConfigurationObjectProperty |Hello szülő hello szülő (a double birtokosi kapcsolat) egy teljesítménygyűjtési szabály az Operations Manager által megcélzott hello objektum megjelenített neve. Operational Insights által felderített hello objektum hello megjelenítendő nevét is lehet, vagy mely hello elleni riasztást. |
| Computer |A legtöbb |Számítógép neve, amely hello adatok tartozik. |
| Eszköznév |ProtectionStatus |Számítógép neve hello adatok túl tartozik (ugyanaz, mint a "Számítógép"). |
| DetectionId |ProtectionStatus | |
| ThreatStatusRank |ProtectionStatus |Fenyegetés állapot dimenziószáma hello fenyegetés állapot interfésztípus numerikus megjelölése. Hasonló tooHTTP válaszkódot hello rangsorolási rendelkezik hello számok között lévő hézagokat (ezért nem fenyegetések nem 150 és nem 100 vagy 0), hely tooadd új állapotok hagyja. Fenyegetések és védelmi állapota összesítését, a hello célja tooshow hello legrosszabb állapota, amely a számítógép hello során hello kijelölt időszakban történt. hello számok rangsorolja hello állapotot váltani, így meggyőződhet hello legmagasabb számú hello rekordhoz. |
| ThreatStatus |ProtectionStatus |A maps 1:1 ThreatStatusRank ThreatStatus, leírása. |
| TypeofProtection |ProtectionStatus |Kártevőirtó termék hello számítógép az észlelt: none, a Microsoft kártevő-eltávolító eszköz, a Forefront, és így tovább. |
| ScanDate |ProtectionStatus | |
| SourceHealthServiceId |ProtectionStatus, RequiredUpdate |A számítógép ügynök Állapotfigyelő szolgáltatás azonosítója. |
| HealthServiceId |A legtöbb |A számítógép ügynök Állapotfigyelő szolgáltatás azonosítója. |
| ManagementGroupName |A legtöbb |Az ügynököt az Operations Manager csatlakoztatott felügyeleti csoport neve. Ellenkező esetben is null vagy üres. |
| Objektumtípus |ConfigurationObject |Írja be (ahogy az Operations Manager felügyeleti csomag/osztálya) ehhez az objektumhoz, Log Analyticshez konfigurációelemzés által felderített. |
| UpdateTitle |RequiredUpdate |Hello frissítés, amely nem található telepített neve. |
| PublishDate |RequiredUpdate |Ha a Microsoft Update hello frissítés közzététele. |
| Kiszolgáló |RequiredUpdate |Számítógép neve hello adatok túl tartozik (ugyanaz, mint a "Számítógép"). |
| Product |RequiredUpdate |Terméket, amely a frissítés hello vonatkozik. |
| UpdateClassification |RequiredUpdate |Frissítés (például kumulatív frissítést vagy szervizcsomagot) típusú. |
| KBID |RequiredUpdate |KB cikk azonosítója, amely leírja a bevált gyakorlat vagy frissítés. |
| WorkflowName |ConfigurationAlert |Hello szabályt vagy figyelőt, előállított hello riasztás neve. |
| Súlyosság |ConfigurationAlert |Hello riasztás súlyossága. |
| Prioritás |ConfigurationAlert |Hello riasztás prioritását. |
| IsMonitorAlert |ConfigurationAlert |Ez a riasztás generálja (true) figyelőt vagy szabályt (hamis)? |
| AlertParameters |ConfigurationAlert |XML-hello Naplóelemzési riasztás hello paraméterekkel. |
| Környezet |ConfigurationAlert |XML-hello kontextusú hello Naplóelemzési riasztás. |
| Számítási feladat |ConfigurationAlert |Technológia vagy terhelést, riasztás hello hivatkozik. |
| AdvisorWorkload |Ajánlás |Technológia vagy javaslat hello munkaterhelés hivatkozik. |
| Leírás |ConfigurationAlert |Riasztás leírása (rövid). |
| DaysSinceLastUpdate |UpdateAgent |Hány nappal ezelőtt (a rekordok relatív tooTimeGenerated) volt az ügynök telepítése minden frissítés a Windows Server Update Service (WSUS) vagy a Microsoft Update? |
| DaysSinceLastUpdateBucket |UpdateAgent |DaysSinceLastUpdate, az idő gyűjtők, hogy mennyivel számítógép utolsó telepített bármilyen frissítést a WSUS vagy a Microsoft Update szolgáltatással a kategorizálási alapján. |
| AutomaticUpdateEnabled |UpdateAgent |Automatikus frissítés ellenőrzése engedélyezve van-e az ügynökön? |
| AutomaticUpdateValue |UpdateAgent |Automatikus frissítés ellenőrzési set tooautomatically letöltéséhez és telepítéséhez, csak töltse le, vagy csak ellenőrizze? |
| WindowsUpdateAgentVersion |UpdateAgent |A Microsoft Update-ügynök hello verziószáma. |
| WSUSServer |UpdateAgent |Melyik WSUS-kiszolgáló a Windows update agent célzott? |
| OSVersion |UpdateAgent |Hello operációs rendszer fut a Windows update agent verziószáma. |
| Név |A javaslat, ConfigurationObjectProperty |Neve vagy címe hello javaslat, vagy a Naplóelemzési konfigurációelemzés hello tulajdonság neve. |
| Érték |ConfigurationObjectProperty |A Naplóelemzési konfigurációelemzés tulajdonság értéke. |
| KBLink |Ajánlás |URL-cím toohello KB cikk ismerteti, hogy az ajánlott eljárás vagy a frissítés. |
| RecommendationStatus |Ajánlás |Javaslatok hello közé tartoznak, néhány típusú rekordok frissített, a most felvett toohello search-indexet beolvasni. Ez az állapot változik-e hello javaslat aktív/megnyitása, vagy ha Log Analyticshez észleli, hogy le van oldva. |
| RenderedDescription |Esemény |Megjelenített (feltöltött paraméterekkel újrafelhasznált szöveg) egy Windows esemény leírása. |
| ParameterXml |Esemény |XML-hello adatok szakaszában egy Windows-esemény (az eredményhalmaznak megfelelően Eseménynapló) hello paraméterekkel. |
| EventData |Esemény |XML-re teljes adatszakasza hello Windows-esemény (az eredményhalmaznak megfelelően Eseménynapló). |
| Forrás |Esemény |Hello eseményt létrehozó eseménynapló-forrás. |
| EventCategory |Esemény |Hello esemény, közvetlenül a Windows Eseménynapló hello kategóriáját. |
| Felhasználónév |Esemény |A felhasználónév hello Windows-esemény (általában NT AUTHORITY\LOCALSYSTEM). |
| SampleValue |PerfHourly |Hello óránkénti összesítéssel teljesítményszámláló átlagos értékét. |
| Perc |PerfHourly |Minimális érték hello óránkénti időszakban a teljesítmény számláló óránkénti összesítés. |
| Maximum |PerfHourly |Hello óránkénti időszakban a teljesítmény számláló óránkénti összesítés maximális értéket. |
| Percentile95 |PerfHourly |hello 95. percentilis hello óránkénti időtartam alatt a teljesítmény számláló óránkénti összesítés. |
| SampleCount |PerfHourly |Rekord hány nyers teljesítményszámláló-minták a óránkénti volt használt tooproduce összesíteni. |
| Fenyegetések |ProtectionStatus |Kártevő szoftver található neve. |
| StorageAccount |W3CIISLog |Az Azure tárfióknapló hello lett beolvasva. |
| AzureDeploymentID |W3CIISLog |Hello felhőalapú szolgáltatás hello bejelentkezési azonosítója az Azure-telepítés tartozik. |
| Szerepkör |W3CIISLog |Hello Azure cloud service hello napló szerepkör tartozik. |
| RoleInstance |W3CIISLog |A napló hello Azure szerepkört hello RoleInstance tartozik. |
| sSiteName |W3CIISLog |IIS-webhely napló hello tartozik too(metabase notation); s-sitename mező hello hello eredeti napló. |
| sComputerName |W3CIISLog |s-computername mező hello hello eredeti napló. |
| a sIP |W3CIISLog |Kiszolgáló IP cím hello HTTP-kérelem küldtünk. az ip-s mező hello hello eredeti napló. |
| csMethod |W3CIISLog |HTTP-metódus (például GET vagy POST) hello ügyfél hello HTTP-kérelem által használt. hello cs-method hello eredeti naplóban. |
| cIP |W3CIISLog |Ügyfél IP cím hello HTTP-kérelem érkezett. ügyfél IP-címe mező hello hello eredeti napló. |
| csUserAgent |W3CIISLog |Hello ügyfél által megadott HTTP-ügynök (böngészőben vagy más módon). hello cs-felhasználói ügynök hello eredeti naplóban. |
| scStatus |W3CIISLog |Hello server toohello ügyfél által visszaadott HTTP-állapotkód: (például 200/403-as/500). hello cs-állapot hello eredeti naplóban. |
| TimeTaken |W3CIISLog |Mennyi ideig (ezredmásodpercben), hogy hello kérelem toocomplete vett igénybe. hello timetaken mező hello eredeti napló. |
| csUriStem |W3CIISLog |Relatív URI-Azonosítót (gazdagép címe, amely van, / keresése), amely a kért. cs-lévő egyedi azonosítóhoz mező hello hello eredeti napló. |
| csUriQuery |W3CIISLog |URI-lekérdezésre. URI-lekérdezések szükségesek csak dinamikus lapok, például az ASP-lapok, így ez a mező általában statikus lapok kötőjellel tartalmazza. |
| sPort |W3CIISLog |Kiszolgálóport, amely HTTP-kérelem hello túl lett elküldve (és, hogy az IIS figyeli, mivel azt felvételre azt). |
| csUserName |W3CIISLog |Hitelesített felhasználó nevét, ha hello kérés hitelesített, és nem névtelen. |
| csVersion |W3CIISLog |(Például HTTP/1.1) hello kérésben használt HTTP protokoll verziója. |
| csCookie |W3CIISLog |A cookie-adatok. |
| csReferer |W3CIISLog |Ez hello a felhasználó utoljára felkeresett helyet. Ez a hely szolgáltatta a hivatkozást toohello aktuális hely. |
| csHost |W3CIISLog |Állomásfejléc (például www.mysite.com), amely a kért. |
| scSubStatus |W3CIISLog |A részállapot hibakód. |
| scWin32Status |W3CIISLog |Windows állapotkódját. |
| csBytes |W3CIISLog |Hello ügyfél toohello kiszolgálóról hello kérelemben küldött bájtok. |
| scBytes |W3CIISLog |Vissza hello válaszként hello server toohello Client visszaadott bájtok száma. |
| ConfigChangeType |Konfigurációváltozás |Olyan változást (például WindowsServices/szoftverfrissítési). |
| ChangeCategory |Konfigurációváltozás |Hello változás (módosított/hozzáadott/eltávolítva) típusa. |
| SoftwareType |Konfigurációváltozás |(Frissítés/alkalmazás) szoftver típusát. |
| SoftwareName |Konfigurációváltozás |Hello szoftver (csak a megfelelő toosoftware változásokat) neve. |
| Közzétevő |Konfigurációváltozás |Szállítói, aki közzéteszi hello szoftver (csak a megfelelő toosoftware változásokat). |
| SvcChangeType |Konfigurációváltozás |Olyan változást, amely a Windows-szolgáltatás (állam/StartupType/útvonal/szolgáltatásfiók) lett alkalmazva. Ez egy csak a megfelelő tooWindows szolgáltatást érintő változások. |
| SvcDisplayName |Konfigurációváltozás |Módosított hello szolgáltatás megjelenített neve. |
| SvcName |Konfigurációváltozás |Módosított hello szolgáltatás neve. |
| SvcState |Konfigurációváltozás |Hello szolgáltatás új (aktuális) állapotát. |
| SvcPreviousState |Konfigurációváltozás |Az előző ismert hello szolgáltatás (csak akkor érvényes, ha a szolgáltatás állapota megváltozott) állapotát. |
| SvcStartupType |Konfigurációváltozás |A szolgáltatás indítási típusa. |
| SvcPreviousStartupType |Konfigurációváltozás |Előző szolgáltatás indítási típusa (csak akkor érvényes, ha a szolgáltatás indítási típusának megváltoztatása). |
| SvcAccount |Konfigurációváltozás |A szolgáltatásfiók. |
| SvcPreviousAccount |Konfigurációváltozás |Előző szolgáltatásfiók (csak akkor érvényes, ha megváltozott a szolgáltatásfiók). |
| SvcPath |Konfigurációváltozás |Elérési út toohello végrehajtható hello Windows-szolgáltatás. |
| SvcPreviousPath |Konfigurációváltozás |Előző elérési útja hello végrehajtható fájlt a következő hello (csak akkor érvényes, ha megváltoztatta) Windows-szolgáltatás. |
| SvcDescription |Konfigurációváltozás |Hello szolgáltatás leírása. |
| Előző |Konfigurációváltozás |A szoftver (telepített/nem telepített/előző verzió) korábbi állapotába. |
| Aktuális |Konfigurációváltozás |Ez a szoftver (verzió: telepítve vagy nem telepített/current) aktuális állapotát. |

## <a name="next-steps"></a>Következő lépések
További információt a naplófájl-keresések feldolgozásához:

* Ismerkedjen meg [keresések jelentkezzen](log-analytics-log-searches.md) tooview részletes megoldások által összegyűjtött adatokat.
* Használjon [Naplóelemzési az egyéni mezők](log-analytics-custom-fields.md) tooextend napló kereséseket.
