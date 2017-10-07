---
title: "az Azure Naplóelemzés napló keresések aaaFind adatok |} Microsoft Docs"
description: "Napló keresések toocombine lehetővé teszi, és a számítógép adatainak a környezetben több forrásból."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: 0d7b6712-1722-423b-a60f-05389cde3625
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: bwren
ms.openlocfilehash: 1161857a0027f05726492417362cb24a8fe21ef8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="find-data-using-log-searches-in-log-analytics"></a>Napló keresések használatát Naplóelemzési adatok megkeresése

>[!NOTE]
> Ez a cikk ismerteti a napló keresések Naplóelemzési hello aktuális lekérdezési nyelv használatával.  Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), akkor túl olvassa el[ismertetése napló keres a Log Analyticshez (új)](log-analytics-log-search-new.md).


A hello fő a Naplóelemzési hello napló keresése szolgáltatás, amely lehetővé teszi a toocombine és a számítógép adatainak a környezetben több forrásból. Megoldások is vannak kapcsolva, akkor metrikák átalakítani egy adott probléma terület körül napló keresési toobring által.

Hello keresési oldalon is létrehozhat egy lekérdezést, és majd kereséskor szűrheti hello eredmények értékkorlátozás vezérlők használatával. Az eredmények fejlett lekérdezési tootransform, a szűrő és a jelentés is létrehozhat.

Általános naplófájl-keresési lekérdezések a legtöbb megoldás oldalon jelennek meg. Teljes hello OMS-konzolon kattintson a csempéket, vagy a részletezéshez tooother elemeknél tooview adatait hello elem által naplófájl-keresési.

Ebben az oktatóanyagban végigvesszük példák toocover összes hello alapjai naplófájl-keresési használatakor.

Azt a lesz egyszerű, gyakorlati példák kezdődnie, és majd kialakítható őket, hogy gyakorlati használati esetek toouse hello szintaxis tooextract hello insights módját hello adatokból ismeretét kaphat.

Beállította a keresési technikák ismeri, áttekintheti a hello [Naplóelemzési jelentkezzen keresési hivatkozás](log-analytics-search-reference.md).

## <a name="use-basic-filters"></a>Basic-szűrők használata
hello először thing tooknow része hello első keresési lekérdezés, minden "|}" függőleges vonal karaktert, mindig van olyan *szűrő*. Az eltolásokat tekintheti, mint a TSQL--WHERE záradék meghatározza, hogy *mi* hello OMS-adattár kívüli adatok toopull részhalmaza. Keresés hello adattár tárgya nagymértékben hello kívánt adatokat tooextract, ezért ezt a természetes, hogy a lekérdezés hello WHERE záradék kellene kezdődnie hello jellemzőinek megadása.

hello legalapvetőbb szűrők is használhatja a következők *kulcsszavak*, például az "error" vagy "timeout" vagy a számítógép nevét. Ezek a lekérdezéstípusok egyszerű általában térjen vissza a különböző alakzatok hello belül azonos set vezethet. Ennek az az oka Naplóelemzési rendelkezik különböző *típusok* adatok hello rendszerben.

### <a name="tooconduct-a-simple-search"></a>egy egyszerű keresés tooconduct
1. Az OMS-portálon hello kattintson **naplófájl-keresési**.  
    ![keresési csempe](./media/log-analytics-log-searches/oms-overview-log-search.png)
2. Hello lekérdezés mezőbe, írja be a `error` majd **keresési**.  
    ![keresési hiba](./media/log-analytics-log-searches/oms-search-error.png)  
    Például hello lekérdezést `error` hello az alábbi képen visszaadott 100 000 **esemény** (napló Management által összegyűjtött) rekordok 18 **ConfigurationAlert** (által előállított rekordokat konfiguráció Értékelés) és 12 **konfigurációváltozás** (rögzített változások követése hello) rögzíti.   
    ![keresési eredmények](./media/log-analytics-log-searches/oms-search-results01.png)  

Ezek a szűrők csak valóban típusok/objektumosztályokon. *Típus* csak egy címkét, vagy egy tulajdonság vagy karakterlánc/neve/kategória, csatolt tooa adat. Egyes dokumentumok hello rendszer címkével rendelkeznek, **típusa: ConfigurationAlert** néhány címkével rendelkeznek, és **típusa: telj**, vagy **esemény típusa:**, és így tovább. Minden keresési eredmény, a dokumentum, a rekord vagy a belépési jeleníti meg az összes hello nyers tulajdonságok és értékeik minden egyes adatok, és használhatja ezeket a mező nevét toospecify hello szűrő Ha azt szeretné, hogy tooretrieve csak hello rekordok ahol hello mező rendelkezik, hogy adott érték.

*Típus* csupán az olyan mezője, amely rendelkezik az összes rekord, eltér nem bármely más mezőre. Ez létrejött hello mezőben hello értéke alapján. Hogy a rekord a különböző formájára lesz. Mellékesen **típus telj =**, vagy **típus = esemény** egyben hello szintaxis toolearn tooquery szüksége az ügynökteljesítmény-adatokat és eseményeket.

A kettőspont (:) vagy egyenlőségjellel (=) is használhatja, hello mező neve után, és hello érték előtt. **Esemény típusa:** és **típus esemény =** egyenértékűek a jelentéssel, dönthet úgy hello inkább stílusát.

Ezért, ha hello írja be a Teljesítményfigyelő = rekordok rendelkezik egy "CounterName" nevű mező, majd írhat egy lekérdezést színű `Type=Perf CounterName="% Processor Time"`.

Adja meg csak hello teljesítményadatokat hello teljesítményszámláló neve "kihasználtsága (%) esetén.

### <a name="toosearch-for-processor-time-performance-data"></a>toosearch processzor idő teljesítményadatokhoz.
* Hello keresési lekérdezés mezőbe írja be a`Type=Perf CounterName="% Processor Time"`

Akkor is pontosabb és használni **InstanceName = _ "Összesen"** hello lekérdezés, amely Windows teljesítményszámláló. Igény szerint kiválaszthatja egy dimenzió, és egy másik **mezőérték:**. hello szűrő automatikusan kerül tooyour szűrő hello lekérdezés sávon. Ez a kép a következő hello látható. Azt mutatja, amelyben tooclick tooadd **InstanceName: "_Total"** toohello lekérdezés semmit beírása nélkül.

![keresési dimenzió](./media/log-analytics-log-searches/oms-search-facet.png)

A lekérdezés most válik.`Type=Perf CounterName="% Processor Time" InstanceName="_Total"`

Ebben a példában nincs toospecify **típus telj =** tooget toothis eredménye. Mert CounterName és példánynév hello mező csak a típusú rekord = telj, hello lekérdezés esetén a ugyanazokat az eredményeket elég konkrétan fogalmaz ahhoz tooreturn hello mert hosszabb, előző egy hello:

```
CounterName="% Processor Time" InstanceName="_Total"
```

Ennek az az oka, hogy minden hello szűrők hello lekérdezésben értékelődnek *és* egymással. Gyakorlatilag hello további mezők toohello feltételek hozzáadása, kap, adott és kifinomultabb eredmények.

Például hello lekérdezés `Type=Event EventLog="Windows PowerShell"` túl megegyezik`Type=Event AND EventLog="Windows PowerShell"`. Bejelentkezett és hello Windows PowerShell eseménynaplójából gyűjtött összes eseményt adja vissza. Ha ad hozzá egy szűrőt többször ugyanazon dimenzió, majd hello probléma a csak formai –, előfordulhat, hogy megzavarhatják hello keresősáv, de azt is ad vissza hello ugyanazokat az eredményeket mivel hello implicit és operátor mindig van hello ismételten kiválasztásával.

Explicit módon a NOT operátor használatával egyszerűen megfordíthatja hello implicit és operátor. Példa:

`Type:Event NOT(EventLog:"Windows PowerShell")`vagy azzal egyenértékű `Type=Event EventLog!="Windows PowerShell"` összes esemény visszaadására további naplófájlokat, amelyek nincsenek hello Windows PowerShell napló.

Vagy más logikai operátor használhatja például a "Vagy". hello következő lekérdezés rekordokat az eseménynaplóban mely hello vagy alkalmazás vagy a rendszer nem adja vissza.

```
EventLog=Application OR EventLog=System
```

Hello fent lekérdezés használata, kaphat bejegyzések mindkét naplók hello azonos set vezethet.

Azonban ha hello vagy távozó hello implicit és helyben távolít el, majd hello következő lekérdezés még nem hoz eredmények mert tooBOTH naplók tartozó eseménynapló-bejegyzés nem létezik. Minden egyes eseménynapló-bejegyzés készült tooonly hello két naplók egyikébe.

```
EventLog=Application EventLog=System
```


## <a name="use-additional-filters"></a>További szűrők használata
hello következő lekérdezés visszaadja 2 eseménynaplóiban keresse meg az összes elküldött adatok hello számítógép bejegyzéseket.

```
EventLog=Application OR EventLog=System
```

![keresési eredmények](./media/log-analytics-log-searches/oms-search-results03.png)

Hello mezők vagy szűrők egyikének kiválasztásával szűkítheti hello lekérdezés tooa adott számítógép, csak egyesekből kivételével. hello eredményül kapott lekérdezés hello következő hasonló lesz.

```
EventLog=Application OR EventLog=System Computer=SERVER1.contoso.com
```

Ez az egyenértékű toohello következő hello miatt implicit és művelet.

```
EventLog=Application OR EventLog=System AND Computer=SERVER1.contoso.com
```

Minden egyes lekérdezés explicit sorrend hello értékeli. Megjegyzés: hello zárójelek közé foglalva.

```
(EventLog=Application OR EventLog=System) AND Computer=SERVER1.contoso.com
```

Hasonlóan hello Eseménynapló mezőben is visszaállíthatja az adatokat csak az adott számítógépek hozzáadásával vagy. Példa:

```
(EventLog=Application OR EventLog=System) AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com OR Computer=SERVER3.contoso.com)
```

Ehhez hasonlóan a következő lekérdezés return hello **% CPU-idő** hello kijelölt csak a két számítógép.

```
CounterName="% Processor Time"  AND InstanceName="_Total" AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com)
```

### <a name="field-types"></a>Mezőtípusok
Szűrők létrehozásakor meg kell ismernie a hello különbségek különböző típusú mezők napló keresés által visszaadott használata.

**Kereshető mezők** kék keresési eredmények megjelenítése.  A keresési feltételek adott toohello mezőben hello alábbi használhatja a kereshető mezők:

```
Type: Event EventLevelName: "Error"
Type: SecurityEvent Computer:Contains("contoso.com")
Type: Event EventLevelName IN {"Error","Warning"}
```

**Szöveg kereshető mezőt szabad** jelennek meg a keresési eredmények között szürke.  Azokat a keresési feltételek adott toohello mező például a kereshető mezők nem használható.  Csak keresett között például hello következő minden mező a lekérdezés végrehajtása során.

```
"Error"
Type: Event "Exception"
```


### <a name="boolean-operators"></a>Logikai operátorok
A dátum és idő és a numerikus, kereshet értékek *nagyobb, mint*, *kisebb mint*, és *kisebb vagy egyenlő, mint*. Használhatja például a egyszerű operátorok >, <>, =, < =,! = hello lekérdezés keresősávban.

Egy adott eseménynaplóból is kereshet egy adott időszakra vonatkozóan. Például hello utolsó 24 órában kifejezett mnemonikus kifejezés a következő hello.

```
EventLog=System TimeGenerated>NOW-24HOURS
```


#### <a name="toosearch-using-a-boolean-operator"></a>egy logikai operátorral toosearch
* Hello keresési lekérdezés mezőbe írja be a`EventLog=System TimeGenerated>NOW-24HOURS`  
    ![a logikai keresése](./media/log-analytics-log-searches/oms-search-boolean.png)

Bár szabályozhatja grafikusan hello alatt az időtartam alatt, és a legtöbb alkalommal szükség lehet, hogy nincsenek előnyeit tooincluding közvetlenül a hello lekérdezési idő szűrő toodo. Például a nagyszerű ahol mindegyik mozaiknál függetlenül hello hello idő lehet felülbírálni az irányítópultok *globális* idő választó hello irányítópult-oldalon. További információkért lásd: [idő kérdések irányítópulton](http://cloudadministrator.wordpress.com/2014/10/19/system-center-advisor-restarted-time-matters-in-dashboard-part-6/).

Amikor idő szerinti szűrését, tartsa szem előtt eredmények lekérése hello *metszetének* hello a két időtartamot-: hello hello OMS-portálon (S1) szerepel, és hello hello lekérdezés (S2) szerepel.

![metszetének](./media/log-analytics-log-searches/oms-search-intersection.png)

Ez azt jelenti, ha hello időszakok nem intersect, például az hello OMS-portálon kiválasztására **ezen a héten** és hello lekérdezésben, ahol megadhatja a **múlt héten**, majd nincs nincs metszetének, és hogy nem bármely kapni.

Összehasonlító operátorok hello TimeGenerated mező használja más helyzetekben is hasznosak. Például a numerikus mezőket.

Ha például adott, konfigurációs Assessment riasztások rendelkezik-e a következő fontossági értékek hello:

* 0 = információk
* 1 = figyelmeztetés
* 2 = kritikus

A figyelmeztetési és a kritikus riasztások lekérdezése, és szintén kizárhatja a tájékoztató állók közül. a következő lekérdezés hello:

```
Type=ConfigurationAlert  Severity>=1
```


Tartomány-lekérdezéseket is használhatja. Ez azt jelenti, hogy képes-e biztosítani hello elején és végén értékek tartományán sorrendje egy sorozatban. Például akkor, ha az Operations Manager eseménynaplójában hello események adott hello EventID lesz nagyobb vagy egyenlő too2100, de nem nagyobb, mint 2199, akkor a következő lekérdezés hello alakítanák vissza őket.

```
Type=Event EventLog="Operations Manager" EventID:[2100..2199]
```


> [!NOTE]
> hello tartományt kell használnia szintaxisa hello kettőspont (:) mező: érték elválasztó és *nem* hello egyenlőségjel (=). Hello alsó és felső hello tartomány végéig tegye szögletes zárójelbe, és válassza el őket két pontot (.).
>
>

## <a name="manipulate-search-results"></a>Kezelheti a keresési eredmények
Amikor adatokat keresünk, lesz toorefine szeretné, hogy a keresési lekérdezés, és ellenőrzése alatt tartja a hello eredmények megfelelő szintű. Eredmények beolvasásakor parancsok tootransform alkalmazhatja azokat.

Log Analytics-keresések parancsok *kell* hello függőleges függőleges vonal (|) után hajtsa végre. Szűrő mindig kell egy lekérdezési karakterlánc hello első része. Határozza meg a hello adatkészlet dolgozunk, és ezt követően "átadja" az eredmények a parancsban. Hello cső tooadd további parancsokat használhatja. Ez a lazán hasonló toohello Windows PowerShell kimenetátirányítási mechanizmusával.

Általában hello Naplóelemzési keresési nyelvi megpróbál toofollow PowerShell stílus és irányelveket toomake informatikai hasonló toohello informatikai szakemberek és tooease hello tanulási görbére.

Parancsok a műveletek nevük legyen, így könnyen állapítható meg, ezek.  

### <a name="sort"></a>Rendezés
hello rendezési parancs lehetővé teszi toodefine hello rendezési sorrend szerint egy vagy több mező. Akkor is, ha, alapértelmezés szerint nem használ, a rendszer érvényesíti egyszerre csökkenő sorrendben. hello legutóbbi eredmények mindig a keresési eredmények hello tetején találhatók. Ez azt jelenti, hogy a Keresés az futtatásakor `Type=Event EventID=1234` mi valóban végrehajtása meg van:

```
Type=Event EventID=1234 **| Sort TimeGenerated desc**
```

Ennek oka az, hello típusú ismeri a naplókban lévő felhasználói felület. Például a Windows Eseménynapló hello.

A rendezési toochange hello módon eredményeinek is használhatja. hello következő példák azt szemléltetik, hogyan működik.

```
Type=Event EventID=1234 | Sort TimeGenerated asc
```

```
Type=Event EventID=1234 | Sort Computer asc
```

```
Type=Event EventID=1234 | Sort Computer asc,TimeGenerated desc
```


hello egyszerű a fenti példákban láthat parancsok működése – hello alakját visszaadott szűrő hello hello eredmények változik.

### <a name="limit-and-top"></a>Korlátozás és a felső
Egy másik kevésbé ismert parancs korlátozás. A határ egy PowerShell-szerű művelet. A határ funkcionálisan azonos toohello TOP parancsot. hello következő lekérdezések vissza hello ugyanazokat az eredményeket.

```
Type=Event EventID=600 | Limit 1
```

```
Type=Event EventID=600 | Top 1
```


#### <a name="toosearch-using-top"></a>toosearch top használata
* Hello keresési lekérdezés mezőbe írja be a`Type=Event EventID=600 | Top 1`   
    ![keresés felső](./media/log-analytics-log-searches/oms-search-top.png)

A fenti hello kép, nincsenek 358 ezer rekordokat EventID = 600. mezők, értékkorlátozás és balra mindig hello eredményének megjelenítése információ hello szűrő hello *hello szűrő része által* hello lekérdezés, mielőtt bármilyen függőleges vonal hello része. Hello **eredmények** ablaktábla csak eredményt adja vissza, hello legutóbbi 1, mert hello példaparancs alakú és hello eredmények átalakítva.

### <a name="select"></a>Válassza ezt:
hello kijelölési parancs Select-Object PowerShell viselkedik. Szűrt eredmények, amelyek nem rendelkeznek az összes azok eredeti tulajdonságait adja vissza. Ehelyett csak az olyan hello tulajdonság, melyet kiválasztva.

#### <a name="toorun-a-search-using-hello-select-command"></a>a kívánt hello kijelölési parancs használatával toorun
1. Írja be a keresésbe, `Type=Event` majd **keresési**.
2. Kattintson a **+ megjelenítése további** valamelyik hello eredmények tooview eredmények hello hello tulajdonságok rendelkezik.
3. Válassza ki olyanokat is explicit módon, és hello lekérdezés módosítások túl`Type=Event | Select Computer,EventID,RenderedDescription`.  
    ![keresési kiválasztása](./media/log-analytics-log-searches/oms-search-select.png)

Ez a parancs akkor különösen hasznos, ha szeretné, hogy toocontrol keresési kimeneti, és válassza ki az igazán fontos adatok csak hello részei a feltárási, amely gyakran nem hello teljes rekord. Ez akkor hasznos, ha különböző típusú rekord *néhány* általános tulajdonságait, de nem *összes* a Tulajdonságok megegyeznek. A, kimenete, amely több természetes néz ki egy táblát, vagy jól használhatók tooa CSV-fájlba exportálni, és majd massaged az Excel programban.

## <a name="use-hello-measure-command"></a>Hello mérték parancs használata
MÉRTÉK egyike hello legsokoldalúbb parancsok, Log Analyticshez keresi. Lehetővé teszi tooapply statisztikai *funkciók* tooyour adatok és összesítési eredményeket egy mező szerint csoportosítva. Nincsenek több statisztikai függvények, amelyek mérték támogatja.

### <a name="measure-count"></a>Mérték count()
az első statisztikai függvény toowork hello, és egy hello legegyszerűbb toounderstand hello *count()* függvény.

Például az összes keresési lekérdezés eredménye `Type=Event`, más néven a keresési eredmények oldalának balra hello értékkorlátozás szűrők megjelenítése. hello szűrők megjelenítése a hello eredmények elérése érdekében a megadott mező szerint értékek terjesztési hello keresés végrehajtása.

![keresési mérték száma](./media/log-analytics-log-searches/oms-search-measure-count01.png)

Például a hello felett akkor jelenik meg hello **számítógép** mező, és azt mutatja, hogy eseményeiben hello szinte 739 ezer hello eredményeket, nincs hello 68 egyedi és különböző értékeket **számítógép** a rekordok mező. hello csempe csak látható hello felső 5, amelyek hello leggyakoribb 5 írt értékeket a hello **számítógép** mezők), ezt a mezőt a megadott értéket tartalmazó dokumentumok hello száma szerint rendezi sorba. Hello ábrán láthatja, hogy – között olyan szinte 369 ezer eseményeket – 90 ezer hello OpsInsights04.contoso.com számítógép 83 ezer hello DB03.contoso.com számítógépről származó, és így tovább.

Mi történik, ha azt szeretné, toosee összes érték, mivel hello csempe csak látható csak hello felső 5?

Ez milyen hello mérték paranccsal teheti meg hello count() függvénnyel. Ez a funkció nem használ paramétereket. Elég megadnia hello mezőre, amely szerint toogroup által – hello **számítógép** mezőben ebben az esetben:

`Type=Event | Measure count() by Computer`

![keresési mérték száma](./media/log-analytics-log-searches/oms-search-measure-count-computer.png)

Azonban **számítógép** csak használt mező *a* minden adat – nincsenek érintett relációs adatbázisok, és nincs nincs külön **számítógép** bárhol objektum. Csak az értékek hello *a* entitáshoz őket, és más jellemzői és szempontjai hello adatait – előállított adatok leírhatja hello ezért hello kifejezés *értékkorlátozás*. Azonban is csoportosíthatja más mezők szerint. Mivel hello mérték parancsban vannak adatcsatornán szinte 739 ezer események eredeti eredményeit hello is rendelkezik egy nevű mező **EventID**, alkalmazhat hello azonos módszerrel toogroup, hogy a mező szerint, és lekérése az eseményazonosító események száma:

```
Type=Event | Measure count() by EventID
```

Ha nem érdekli, amely tartalmaz egy adott értéket hello tényleges rekordszám, de ehelyett Ha kizárólag listája hello értékek magukat, hozzáadhat egy *válasszon* parancsot, és csak select hello első oszlop hello végén:

```
Type=Event | Measure count() by EventID | Select EventID
```

Ezután beolvasása több bonyolult, és előre a hello lekérdezésben hello eredmények rendezéséhez, vagy túl csak kattintson hello oszlopok hello rácsban.

```
Type=Event | Measure count() by EventID | Select EventID | Sort EventID asc
```

#### <a name="toosearch-using-measure-count"></a>count mérték használatával toosearch
* Hello keresési lekérdezés mezőbe írja be a`Type=Event | Measure count() by EventID`
* Hozzáfűzendő `| Select EventID` hello lekérdezés toohello végét.
* Végezetül hozzáfűzése `| Sort EventID asc` hello lekérdezés toohello végét.

Van néhány fontos tényezőt toonotice kiemelése:

Első lépésként hello látja eredményei nem hello eredeti nyers eredmények többé. Ehelyett azok összesített eredmények – tulajdonképpen az eredmények csoportok. Ez nem hiba, de tisztában kell lennie, hogy van-e interakció eltérő adatok alakzatból hello eredeti nyers lekérdezi hello parancsprogramok hello összesítési és statisztikai függvény eredményeként létrehozott nagyon különböző alakját.

Második, **számának mérésére** jelenleg értéket ad vissza csak hello top 100 különböző eredmény. Ez a korlátozás nem alkalmazható toohello más statisztikai függvények. Igen általában szüksége lesz egy pontosabb szűrő első toosearch toouse elemeket mérték count() alkalmazása előtt.

## <a name="use-hello-max-and-min-functions-with-hello-measure-command"></a>Hello max és min funkciók használata hello mérték parancs
Számos esetben ha **mérték Max()** és **mérték Min()** hasznosak. Azonban mivel minden egyes függvény ellentétes minden más, azt is bemutatják Max() és kísérletezhet Min() saját.

Biztonsági események kérdezze le, ha van egy **szint** tulajdonság, amely eltérőek lehetnek. Példa:

```
Type=SecurityEvent
```

![keresési mérték száma indítása](./media/log-analytics-log-searches/oms-search-measure-max01.png)

Ha azt szeretné, tooview hello legnagyobb értéke az összes hello biztonsági események egy közös számítógép hello Csoportosítás mezőben megadott használhatja

```
Type=ConfigurationAlert | Measure Max(Level) by Computer
```

![keresési mérték maximális számítógép](./media/log-analytics-log-searches/oms-search-measure-max02.png)

Jeleníti meg, amely hello számítógépek, amelyekre **szint** rekordok, azokat a többsége a legalább 8. szintű számos kellett egy szint 16.

```
Type=ConfigurationAlert | Measure Max(Severity) by Computer
```

![keresési mérték maximális idő számítógép jön létre.](./media/log-analytics-log-searches/oms-search-measure-max03.png)

Számok jól működik ez a funkció, de a DateTime mezők is működik. Hasznos toocheck hello az utolsó, vagy bármely adat legutóbbi időbélyegzője indexelt számára minden számítógéphez. Például: amikor hello legfrissebb biztonsági esemény történt az egyes gépek?

```
Type=ConfigurationChange | Measure Max(TimeGenerated) by Computer
```

## <a name="use-hello-avg-function-with-hello-measure-command"></a>Hello avg függvénnyel hello mérték paranccsal
hello Avg() mérték használt statisztikai funkció lehetővé teszi a toocalculate hello átlagos néhány mező értékét, és csoport eredmények hello ugyanazon vagy másik mező. Ez akkor hasznos, a különböző esetekben, például a teljesítmény.

Először foglalkozunk teljesítményadatokat. Vegye figyelembe, hogy jelenleg OMS összegyűjti a Windows és Linux rendszerű gépek teljesítményszámlálók.

a toosearch *összes* teljesítményadatokat, lehető legegyszerűbb lekérdezés hello van:

```
Type=Perf
```

![keresési avg indítása](./media/log-analytics-log-searches/oms-search-avg01.png)

hello feltűnik először is, hogy a Naplóelemzési elsajátíthatja, hogy három perspektívák: lista, amely jelzi, hogy szerinti hello tényleges rekordok mögött hello diagramok; Táblázatot, amely bemutatja a teljesítményszámláló-adatokat; a táblázatos nézet és metrikákat, amely mutatja a hello diagramok teljesítményszámlálók.

A fenti hello kép nincsenek két csoportja, amelyek jelzik, hello következő jelölt mezők:

* hello első set hello lekérdezésszűrő Windows teljesítményszámláló neve, a objektum neve és a példány nevét határozza meg. Valószínűleg leggyakrabban használni kívánt értékkorlátozás/szűrők hello mezőket
* **Ellenértéknek** hello tényleges hello számláló értéke van. Ebben a példában hello értéke *75*.
* **TimeGenerated** 12:51, 24 órás formátumban van.

Íme egy grafikonon hello metrikák nézetét.

![keresési avg indítása](./media/log-analytics-log-searches/oms-search-avg02.png)

Után hello telj rekord alakzat témakörben elolvasta, és hogy olvassa el, egyéb keresési technikák használhatja mérték Avg() tooaggregate ilyen típusú numerikus adatok.

Íme egy egyszerű példa:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" | Measure Avg(CounterValue) by Computer
```

![keresési avg samplevalue](./media/log-analytics-log-searches/oms-search-avg03.png)

Ebben a példában válassza a teljes CPU-idő teljesítményszámláló hello és átlagos számítógépen. Ha toonarrow le az eredményeket tooonly hello utolsó 6 óra, hello szűrő vezérlő használja, vagy az alábbiak szerint adja meg a lekérdezés:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer
```

### <a name="toosearch-using-hello-avg-function-with-hello-measure-command"></a>hello avg függvénnyel hello mérték paranccsal toosearch
* Hello keresési lekérdezés mezőbe írja be `Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer`.

Összesítő és a adatainak *keresztül* számítógépek. Tegyük fel, hogy rendelkezik-e a farm minden egyes csomópont esetén egy másikra egyenlő tooany valamilyen állomásoknak, és csak azonos típusú és a munka érdemes nagyjából átgondolni összes hello tegye. Az összes, egy nyissa meg az alábbi lekérdezést, és átlagok lekérése hello teljes farm hello számlálók sikerült. Az alábbi példa hello hello számítógépek kiválasztásával megkezdése:

```
Type=Perf AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Most, hogy hello géppel is rendelkezik, emellett csak szeretné tooselect két fő teljesítménymutatók (KPI): % CPU-használat és a % szabad lemezterület. Igen hello lekérdezés része lesz:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS
```

Most a számítógépek és a számlálók a következő példa hello adhat hozzá:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Mivel egy olyan speciális kijelölés, hello **Avg() mérjük** parancs térhet vissza hello átlagos nem a számítógépen, de hello farm keresztül egyszerűen: CounterName szerinti csoportosítás. Példa:

```
Type=Perf  InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03") | Measure Avg(CounterValue) by CounterName
```

Ez lehetővé teszi a környezet KPI-k néhány hasznos kompakt nézetét.

![keresési avg csoportosítás](./media/log-analytics-log-searches/oms-search-avg04.png)

Hello keresési lekérdezés irányítópult könnyen használható. Például képes hello keresési lekérdezés mentéséhez, és hozzon létre egy irányítópultot, nevű *webkiszolgáló Farm KPI-k*. További információ az irányítópultok, használata toolearn lásd: [egyéni irányítópult létrehozása a Naplóelemzési](log-analytics-dashboards.md).

![keresési avg irányítópult](./media/log-analytics-log-searches/oms-search-avg05.png)

### <a name="use-hello-sum-function-with-hello-measure-command"></a>Hello sum függvénnyel hello mérték paranccsal
hello sum függvény hello mérték parancs hasonló tooother funkciókat. Láthatja, hogy hogyan toouse hello a sum függvénnyel kapcsolatos például [W3C IIS naplók keresése a Microsoft Azure Operational Insights](http://blogs.msdn.com/b/dmuscett/archive/2014/09/20/w3c-iis-logs-search-in-system-center-advisor-limited-preview.aspx).

Max() és Min() használható számok, időpontok és szöveges karakterláncot. A szöveges karakterláncok betűrend szerint rendezve jelennek és először első és utolsó.

Azonban nem használhat Sum() numerikus mezők csakis. Ez a tooAvg() is vonatkozik.

### <a name="use-hello-percentile-function-with-hello-measure-command"></a>Hello százalékfüggvény használata hello mérték parancs
hello százalékfüggvény hasonló tooAvg() és Sum() abban, hogy a numerikus mezők csak használható. Bármely érték között egy numerikus mezőben 1 too99 is használhatja. Is használhatja az mindkét **PERCENTILIS** és **pct** parancsok. Íme néhány példa:  

```
Type:Perf CounterName:"DiskTransers/sec" |measure percentile95(CurrentValue) by Computer
```
```
Type:Perf ObjectName=LogicalDisk CounterName="Current Disk Queue Length" Computer="MyComputerName" | measure pct65(CurrentValue) by InstanceName
```

## <a name="use-hello-where-command"></a>Hello használni, ahol parancsot
Amikor parancs szűrő hasonlóan működik, de hello adatcsatorna toofurther szűrő alkalmazható hello eredmények, amelyek mérték parancs – mint megakadályozását tooraw lekérdezés hello elején kiszűrt eredményeket gyártott összesíteni.

Példa:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer
```

Hozzáadhat egy másik cső "|}" karaktert és hello hol parancs tooonly hozzá a számítógépet, amelynek átlagos CPU 80 % fölötti a következő példa hello:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer | Where AVGCPU>80
```

Ha ismeri a Microsoft System Center – Operations Manager, az eltolásokat tekintheti hello adott parancsot a felügyeleti csomag feltételeknek. Ha hello példa olyan szabályt, hello lekérdezés első részét hello lenne hello adatforrás és hello ahol a parancs hello feltételészlelése lenne.

A csempe is használhatja a hello lekérdezés **saját irányítópult**, mint egy figyelő rendezi, amikor a számítógép processzort túlterheltek toosee. További információ az irányítópultokat, toolearn lásd [egyéni irányítópult létrehozása a Naplóelemzési](log-analytics-dashboards.md). Hozhat létre, és hello mobilalkalmazással irányítópulton. További információkért lásd: [OMS mobilalkalmazás ](http://www.windowsphone.com/en-us/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865). A kép a következő hello hello alsó két csempéin, látható hello figyelő listája jelenik meg, és egy számot. Alapvetően, mindig a kívánt hello számú toobe nulla és üres lista toobe hello. Ellenkező esetben a figyelmeztetési állapot azt jelzi. Szükség esetén használhatja tootake, mely számítógépek egy pillantást terhelés alatt áll.

![mobil irányítópult](./media/log-analytics-log-searches/oms-search-mobile.png)

## <a name="use-hello-in-operator"></a>Hello operátor használata
Hello *IN* operátor szerinti szűrése, valamint *NOT IN* lehetővé teszi a toouse subsearches, amelyek közé tartozik, amelynek argumentuma egy újabb keresést kereséseket. Kapcsos zárójelek {} belül egy másik található *elsődleges* vagy *külső* keresési. hello eredménye egy subsearch, gyakran különböző eredmények listája majd használt, elsődleges keresését argumentumaként.

Subsearches toomatch fájlcsoportokat a, hogy közvetlenül egy keresési kifejezést, de a keresési generálhatók az ismerteti nem használható. Például, ha érdekli a használatával egy keresési toofind származó összes eseményt *gépek, amelyekről biztonsági frissítések hiányoznak*, akkor szükséges, amely először azonosítja, amely egy subsearch toodesign *gépek, amelyekről biztonsági frissítések hiányoznak*  előtt események tartozó toothose állomások talál.

Igen, sikerült express *számítógépek jelenleg hiányzik a szükséges biztonsági frissítések* a következő lekérdezés hello:

```
Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer
```    

![A keresési példa](./media/log-analytics-log-searches/oms-search-in01-revised.png)

Miután hello lista, is használhatja hello keresési listaként egy belső keresési toofeed hello számítógépek egy külső (elsődleges)-keresés, amely kikeresi az események a számítógépeken. Ehhez kapcsos zárójelek hello belső keresési befoglaló és az eredményeket a lehetséges értékek a hello hello IN operátor használata külső keresési szűrő/mező szerint. hello lekérdezés hasonló lesz:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer}
```
![A keresési példa](./media/log-analytics-log-searches/oms-search-in02-revised.png)

Is használhatja a hello belső keresési, mert szűrő hello rendszer frissítések értékelését értesítés hello idő pillanatképet készít az összes számítógép 24 óránként. Hogy hello belső lekérdezés könnyű, és pontos naponta csak keresésével. hello külső keresési helyette használja hello időbeállítást hello felhasználói felületén, események lekérése hello legutóbbi 7 nap. Lásd: [logikai operátorok](#boolean-operators) idő operátorok további információt.

Mivel Ön hello belső keresési szűrő értékeként valóban csak használata hello eredmények hello külső egy, a továbbra is érvényesek lesznek a hello külső keresési parancsok. Például akkor továbbra is egy másik mérték paranccsal események fent csoport hello:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer} | measure count() by Source
```

![A keresési példa](./media/log-analytics-log-searches/oms-search-in03-revised.png)

Általában érdemes a belső lekérdezés tooexecute gyorsan mert Naplóelemzési Szolgáltatásoldali időtúllépések és is tooreturn eredmények kis mennyiségű. Belső lekérdezés hello további eredményeket ad vissza, ha hello eredménylista lekérdezi csonkolva lesz, ami okozhatja hello külső keresési tooreturn helytelen eredményeket.

Egy másik szabály hello belső keresés jelenleg kell tooprovide *összesített* eredmények. Más szóval tartalmaznia kell egy *mérték* parancs; be egy külső keresés jelenleg nem hírcsatorna nyers eredmények.

Emellett csak egy IN operátor lehet, és hello utolsó szűrő hello lekérdezésben kell lennie. Több IN operátor nem lehet, vagy szeretné – Ez lényegében megakadályozza, hogy több subsearches fut: hello fontos pont, hogy csak egy sub/belső a keresés minden külső keresési lehetőség.

Ezek a korlátozások mellett is IN lehetővé teszi számos különböző típusú korrelált keresések, illetve toodefine lehetővé teszi a számítógépek, a felhasználók és a fájlok – például valami hasonló toogroups bármilyen hello az adatokat tartalmaz. Az alábbiakban további példákat:

**Minden frissítés hiányzik a számítógépeken, ahol automatikus frissítési beállítás le van tiltva**

```
Type=Update UpdateState=Needed Optional=false Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual | measure count() by Computer} | measure count() by KBID
```

**Összes hiba esemény (ahol SQL Assessment futott =) SQL Server rendszert futtató számítógépek**

```
Type=Event EventLevelName=error Computer IN {Type=SQLAssessmentRecommendation | measure count() by Computer}
```

**Minden biztonsági esemény (ahol AD Assessment futott =) tartományvezérlő számítógépekről**

```
Type=SecurityEvent Computer IN { Type=ADAssessmentRecommendation | measure count() by Computer }
```

**Amely más fiókokat toohello bejelentkezett fiók BACONLAND\jochan hol jelentkezett be ugyanazon számítógép?**

```
Type=SecurityEvent EventID=4624   Account!="BACONLAND\\jochan" Computer IN { Type=SecurityEvent EventID=4624   Account="BACONLAND\\jochan" | measure count() by Computer } | measure count() by Account
```

## <a name="use-hello-distinct-command"></a>Hello különböző paranccsal
Hello nevet javasol, a parancs különböző értékből álló lista mező biztosít. Fontos, hogy érdekes egyszerű, de elég hasznos lehet. hello azonos érhető el az a mérték count() paranccsal, ahogy alább látható.

```
Type=Event | Measure count() by Computer
```

![KÜLÖNBÖZŐ keresési példa](./media/log-analytics-log-searches/oms-search-distinct01-revised.png)

Azonban ha érdekli az összes csak a különböző értékeket, és nem hello száma azt jelzi, hogy az adott értékek listáját, majd DISTINCT kimeneti tisztító és könnyebb tooread és nyújthatnak rövidebb szintaxisa alább látható módon.

```
Type=Event | Distinct Computer
```
![KÜLÖNBÖZŐ keresési példa](./media/log-analytics-log-searches/oms-search-distinct02-revised.png)

## <a name="use-hello-countdistinct-function-with-hello-measure-command"></a>Hello countdistinct függvény használata hello mérték parancs
hello countdistinct függvény megjeleníti az egyes csoportok különböző értékeket hello számát. Ez lehet például az egyes jelentett egyedi számítógépek számát toocount hello használja:

```
* | measure countdistinct(Computer) by Type
```

![OMS-countdistinct](./media/log-analytics-log-searches/oms-countdistinct.png)

## <a name="use-hello-measure-interval-command"></a>Hello mérték időköz paranccsal
A közel valós idejű teljesítményadat-gyűjtés, összegyűjtheti, és bármilyen teljesítményszámláló, a Naplóelemzési megjelenítése. Egyszerűen belépés hello lekérdezés **típusa: telj** számlálók és a Naplóelemzési környezetében lévő kiszolgálókat hello száma alapján metrika diagramjait ezer ad vissza. Az igény szerinti metrika összesítéssel, vessen egy pillantást hello általános metrikák magas szintű és részletes bemutatója részletesebb adatokká szükség van a környezetben.

Tegyük fel, amelyet meg szeretne tooknow hello átlagos CPU Mi az a számítógép között. Hello átlagos CPU minden számítógép megnézi nem hasznos lehet, mert az eredmények lehet, hogy első Görbített. további részleteinek toolook, fájlba is összevonhatja az eredmény egy kisebb idő ablak adattömböket, és keresse meg az idősor között a különböző dimenziókban. Például végezheti el hello óránkénti átlaga CPU-használata a számítógépek között az alábbiak szerint:

```
Type:Perf CounterName="% Processor Time" InstanceName="_Total" | measure avg(CounterValue) by Computer Interval 1HOUR
```

![átlagos időköz mérték](./media/log-analytics-log-searches/oms-measure-avg-interval.png)

Alapértelmezés szerint ezekkel az eredményekkel fog megjelenni a diagram interaktív sor több adatsort.  Ez a diagram támogatja az adatsorozat való átváltással (az y tengelyen rescaling), nagyításához, és rámutató.  hello tábla megjelenítési beállítás érhető el továbbra is hello nyers adatainak megtekintése, ha szükséges.

Más mezők szerint is csoportosíthatja. Ebben a példában I hello % számlálók egy adott számítógép, figyelve megállapítható kívánt tooknow Mi az, hogy minden számláló hello óránkénti 70 százalékos érték:

```
Type:Perf Computer=beefpatty4 CounterName=%* InstanceName=_Total | measure percentile70(CounterValue) by CounterName Interval 1HOUR
```
Egy dolog toonote, győződjön meg arról, hogy ezeket a lekérdezéseket nem korlátozott tooperformance számláló áll. Is alkalmazhatóak legyenek tooany metrikát. Ebben a példában I vagyok megnézi a W3C IIS-naplókba. Tooknow szeretném, mi az, hogy egy 5 perces időszakban az egyes kérelmek feldolgozásához szükséges hello maximális idő:

```
Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES
```

### <a name="use-multiple-aggregates-in-one-query"></a>Egy lekérdezést több aggregátumok használata
Megadhat egy mérték parancs több összesített záradékot.  Minden egyes egymástól függetlenül lehet aliasnevet.  Ha nincs megadva egy alias hello eredő mezőnév lehet-e a használt hello összesítő függvény (azaz "avg(CounterValue)" avg(CounterValue)) számára.

 ```
Type=WireData | measure avg(ReceivedBytes), avg(SentBytes) by Direction interval 1hour
```
![OMS-multiaggregates1](./media/log-analytics-log-searches/oms-multiaggregates1.png)

Egy másik példa:

 ```
* | measure countdistinct(Computer) as Computers, count() as TotalRecords by Type
```


## <a name="next-steps"></a>Következő lépések
Napló keresések kapcsolatos további információkért lásd:

* Használjon [Naplóelemzési egyéni mezők](log-analytics-custom-fields.md) tooextend napló kereséseket.
* Felülvizsgálati hello [Naplóelemzési jelentkezzen keresési hivatkozás](log-analytics-search-reference.md) tooview összes hello keresési mezők és facets Naplóelemzési érhető el.
