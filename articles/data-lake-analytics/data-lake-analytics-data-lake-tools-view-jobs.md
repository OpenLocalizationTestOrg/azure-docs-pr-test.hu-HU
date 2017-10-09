---
title: "aaaUse feladat böngésző és a feladat tekintse meg az Azure Data Lake Analytics-feladatok |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse feladat böngésző és az Azure Data Lake Analytics-feladatok feladat megtekintése. "
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: bdf27b4d-6f58-4093-ab83-4fa3a99b5650
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/02/2017
ms.author: jgao
ms.openlocfilehash: c45e618426808349ca380b1bcfaefd4c947ce7ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-job-browser-and-job-view-for-azure-data-lake-analytics-jobs"></a>Feladat böngésző és a feladatok használja az Azure Data lake Analytics-feladatok
hello Azure Data Lake Analytics szolgáltatás archívumokat küldött feladatok egy [lekérdezéstár](#query-store). Ebből a cikkből megismerheti, hogyan toouse feladat böngésző és a feladat tekintse meg az Azure Data Lake Tools for Visual Studio toofind hello korábbi sikertelen feladat-információkat. 

Alapértelmezés szerint a Data Lake Analytics szolgáltatás hello hello feladatok archiválja 30 napig. hello lejárati idővel konfigurálásával testre szabott hello elévülési szabályzatának hello Azure-portálon is konfigurálható. Csak akkor tudja tooaccess hello feladatinformációkat lejárata után. 

## <a name="prerequisites"></a>Előfeltételek
Lásd: [Data Lake Tools for Visual Studio Előfeltételek](data-lake-analytics-data-lake-tools-get-started.md#prerequisites).

## <a name="open-hello-job-browser"></a>Nyissa meg a feladat böngésző hello
Hozzáférés hello feladat böngésző keresztül **Server Explorer > Azure > Data Lake Analytics > feladatok** a Visual Studióban.  Hello feladat böngészőt használ, egy Data Lake Analytics-fiók hello lekérdezéstár érheti el. Feladat böngésző megjeleníti a Lekérdezéstár hello balra, a alapvető feladatok adatait, valamint hello jobb megjelenítése a feladat megtekintése részletes feladat adatait.

## <a name="job-view"></a>Feladatok megtekintése
Sikertelen feladat látható hello a feladatok részletes információk megtekintése. egy feladat tooopen kattintson duplán egy feladatot, a feladat böngésző hello, vagy megnyitásához hello Data Lake menüjében kattintson feladat megtekintése. Meg kell jelennie egy párbeszédpanel, feltöltve hello feladat URL-címet.

![A Data Lake Visual Studio-projekt böngésző eszközei](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view.png)

Feladat-nézet tartalmazza:

* Feladat összegzése
  
    Frissítse a feladatok hello toosee hello futó feladatok a legfrissebb információkat.
  
  * Feladat állapota (diagramhoz):
    
      A feladatállapot hello Projekt fázisok ismerteti:
    
      ![Az Azure Data Lake Analytics-feladat fázisok állapota](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-phases.png)
    
    * Előkészítése: A parancsfájl toohello felhőalapú, fordítása, és hello parancsfájl hello fordítási szolgáltatás használatakor optimalizálására töltse fel.
    * A várólistára: Feladatok várólistára helyezett savó elegendő erőforrást vár, vagy hello feladatok haladhatja meg a fiók korlátozás / egyidejű feladatok maximális hello. hello prioritási beállítás meghatározza, hogy hello feladatütemezési aszinkron feladatok - hello hello kevesebb, hello hello elsőbbséget.
    * Futó: hello feladat ténylegesen fut a Data Lake Analytics-fiók.
    * Véglegesítése: hello feladat befejeződik (például véglegesítése hello fájl).
      
      hello feladat minden fázisban sikertelen lehet. Például fordítási hibák hello előkészítése fázis, a hello várakozik fázisban időtúllépést és a végrehajtási hibák a hello futó fázis stb.
  * Alapvető információk
    
      hello alapvető feladatinformációkat hello hello feladat összegzése panel alsó részén látható.
    
      ![Az Azure Data Lake Analytics-feladat fázisok állapota](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-info.png)
    
    * Feladat eredmény: Sikeres vagy sikertelen volt. hello feladat minden fázisban sikertelenek lehetnek.
    * Teljes időtartam: Fali idő (időtartam) elküldésekor időt és befejezési időpont között.
    * Számítási teljes időtartam: minden csomópont-végrehajtási idő hello összege, érdemes lehet, mint hello idő hello feladatot kell végrehajtania a csak egy-egy csúcsának. Tekintse meg a tooTotal csúcsban toofind csúcspont további információt.
    * Küldje el/kezdő/záró idő: hello idő, amikor megkapja az hello Data Lake Analytics szolgáltatás a feladat elküldése/indítása toorun hello feladat/végpontok hello feladat sikeres volt-e.
    * Fordítási/várakozik/fut: Valós idő telik hello előkészítése/várakozik/fut fázis során.
    * : Hello Data Lake Analytics fiókok hello feladat futtatásához használt.
    * Szerző: hello hello feladat küldő felhasználó valós személy vagy a system fiók lehet.
    * Prioritás: hello hello feladat prioritása. hello hello kevesebb, hello hello elsőbbséget. Csak hello sorozatát hello feladatokat hello sorban van hatással. A magasabb prioritású virtuális gép beállítása nem futó feladatok megelőzik az.
    * Párhuzamossági: hello kért egyidejű Azure Data Lake Analytics egységre is kiterjed (ADLAUs), más néven csúcsban maximális számát. Jelenleg egy-egy csúcsának egyenlő tooone VM két virtuális core és 6 GB RAM, bár ez sikerült frissíteni a jövőben a Data Lake Analytics frissíti.
    * Bal oldali bájtok: Bájtok kell toobe feldolgozása következik, amíg hello feladat befejeződik.
    * Olvasás/írt bájtok: bájt lett olvasása/írása hello feladat indulása óta.
    * Teljes csúcsban: hello feladat van felosztva hány darab munka, minden egyes munkákat csúcspont nevezik. Ezt az értéket ismerteti, hogy hány darab munka hello feladat áll. Csúcspont egységként folyamat alapvetően, más néven Azure Data Lake Analytics egység (ADLAU), érdemes lehet, és a párhuzamosság csúcsban is futtatható. 
    * Befejeződött/futó/nem sikerült: hello befejeződött/futó/sikertelen csúcsban száma. Csúcsban tooboth felhasználói kód és a rendszer hibák miatt sikertelen lehet, de hello rendszer újrapróbálkozások sikertelenek csúcsban automatikusan néhány alkalommal. Hello csúcspont továbbra is sikertelen újrapróbálkozás után, ha hello teljes feladat sikertelen lesz.
* A Feladatgrafikon
  
    A U-SQL parancsfájl hello programot a bemeneti adatok toooutput adatok átalakítása jelöli. hello parancsprogram fordítása, és optimalizált tooa fizikai végrehajtási terv hello előkészítése fázisában. A Feladatgrafikon tooshow hello fizikai végrehajtási terv.  a következő diagram hello hello folyamatát mutatja be:
  
    ![Az Azure Data Lake Analytics-feladat fázisok állapota](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-logical-to-physical-plan.png)
  
    Egy feladat hány darab munka van felosztva. Minden egyes munkákat csúcspont nevezik. hello csúcsban Super csúcspont (más néven előkészítés) szerint csoportosítva, és mint Feladatgrafikon ábrázolt. hello zöld szakasz felirat a hello feladatgrafikon hello szakaszból megjelenítése.
  
    Minden csomópont egy szakaszban tesz a hello azonos típusú együttműködve hello különböző részeinek azonos adatokat. Például ha egy TB adatot tartalmazó fájlt, és azt olvasásakor csúcsban több száz, azok éppen olvas a rendszer. Ezek a csúcsban hello vannak csoportosítva azonos szakasz és azonos módon működik különböző részein ugyanazon bemeneti fájl.
  
  * <a name="state-information"></a>Szakasz információkat
    
      Egy adott szakaszban számok hello metaadattábla láthatók.
    
      ![Azure Data Lake Analytics feladat graph szakasz](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-stage.png)
    
    * SV1 Bontsa ki: egy-egy számot, hello művelet módszerrel nevű szakaszban hello nevét.
    * 84 csúcsban: hello csúcsban teljes száma ebben a szakaszban. hello. ábra azt jelzi, hogy hány darab munka oszlik meg ebben a szakaszban.
    * 12.90 s/csúcspont: hello csúcspont átlagos végrehajtási idő ebben a szakaszban. Ez a szám összege (minden csomópontra végrehajtási idő) kiszámítása / (teljes csúcspont száma). Tehát ha párhuzamossági végrehajtott összes hello csúcsban rendelheti, hello teljes szakasz 12.90 befejeződött s. Azt is jelenti, ha a munkahelyi hello összes ebben a szakaszban a Feladattervek történik, hello költség lenne #vertices * átlagos idő.
    * írt 850,895 sorok: Ebben a szakaszban írt sorainak száma összesen.
    * R/W: adatmennyiség bájtban ebben a szakaszban olvasható/Written.
    * Színek: Színek hello szakasz tooindicate másik csomópont állapota is használja.
      
      * Zöld állapot azt jelzi, hogy hello csúcspont van sikeres volt.
      * Narancssárga azt jelzi, hogy hello csúcspont a rendszer ismét megkísérli. újra megpróbálja hello csúcspont sikertelen volt, de a rendszer ismét megkísérli automatikusan, és sikeresen hello rendszer, és az általános hello fázis sikeresen befejeződött. Ha hello csúcspont újra megpróbálja, de továbbra is sikertelen, hello szín bekapcsolása piros és hello teljes feladat sikertelen volt.
      * Vörös állapot azt jelzi sikertelen, ami azt jelenti, hogy egy bizonyos csúcspont volt néhány alkalommal újra hello rendszer azonban továbbra is sikertelen. Ebben a forgatókönyvben hello teljes feladat toofail okoz.
      * Kék azt jelenti, hogy egy bizonyos csúcspont fut-e.
      * Fehér jelzi hello csúcspont vár. hello csúcspont lehet várakozási toobe ütemezett, miután egy ADLAU elérhetővé válik, vagy az lehet, hogy lehet vár, mert a bemeneti adatok nem áll készen.
      
      Hello. szakaszra vonatkozóan további részleteket az egérmutatót egy állapot szerint rámutató által találja meg:
      
      ![Az Azure Data Lake Analytics-feladat graph szakasz részletei](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-stage-details.png)
  * Csúcsban: Ismerteti hello csúcsban részletek, például összesen hány csúcsban készültek el, hogy hány csúcsban-e a sikertelen volt, vagy még fut/Várakozás stb.
  * Adatok olvasása határokon/belüli pod: fájlokat és adatokat tárolódnak több három munkaállomás-csoporttal elosztott fájlrendszerben. Itt hello értéket írja le, mennyi adatot olvasott a hello pod azonos vagy eltérő közötti pod.
  * Teljes compute idő: hello sum minden csúcspont végrehajtási idejének hello szakaszban is fontolóra veheti azt, a csak egy-egy csúcsának végrehajtása hello időbe telne ha összes hello szakaszában működik.
  * Adatok és a sorok írhatók/olvashatók: azt jelzi, mennyi adat- vagy sor olvasása/írása, vagy toobe kell olvasni.
  * Csúcspont olvasása sikertelen: ismerteti, hogy hány csúcsban adatolvasási során nem sikerült.
  * Ismétlődő csúcspont elveti: Ha csúcspont működése túl lassú, előfordulhat, hogy hello rendszer ütemezze, több csúcsban toorun hello azonos munkákat. Reductant csúcsban elvesznek, amennyiben az egyik hello csúcsban sikeresen befejeződik. Ismétlődő csúcspont elveti lévő hello szakaszában adatismétlődések, elveti a rekordok hello száma.
  * Csomópont által okozott hibával találkozzanak: hello csúcspont sikeres volt, de beszerzése futtassa újra a később toosome okok miatt. Például ha az alárendelt csomópont köztes bemeneti adatok megszakad, a program kéri hello csúcsának fölérendelt toorerun.
  * Ütemezés végrehajtások csúcspont: hello csúcsban ütemezett hello teljes ideje.
  * Minimális és átlagos/maximális csúcspont beolvasott adatok: minden csúcspont minimális/átlagos/legfeljebb hello adatokat olvasni.
  * Időtartam: hello valós idő fázist időt vesz igénybe, kell tooload profil toosee ezt az értéket.
  * Feladat visszajátszása
    
      A Data Lake Analytics feladat fut, és archívumok hello hello feladatok, például amikor hello csúcsban indulnak el, információkat futtató csúcsban leállt, sikertelen, és hogyan azokat újra vannak, stb. Hello információk automatikusan hello lekérdezéstár bejelentkezett és a feladat profil tárolja. Letöltheti a hello "Profil betöltése" feladat nézetben feladat profilhoz, és megtekintheti a hello feladat visszajátszása hello feladat profil letöltése után.
    
      Feladat visszajátszása egy epitome képi megjelenítés, mi történt hello fürtben. Segít tekintse meg a feladat végrehajtásának menetét, és vizuálisan észlelheti a teljesítményanomáliákat és a szűk keresztmetszeteket nagyon rövid időn belül (kevesebb mint 30s általában).
  * Feladat tűz térkép megjelenítési 
    
      Feladat Hőtérkép hello megjelenítési legördülő Feladatgrafikon keresztül lehet kiválasztani. 
    
      ![Azure Data Lake Analytics feladat graph halommemória térkép megjelenítési](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-heat-map-display.png)
    
      Azt illusztrálja, hello i/o, időpont és átviteli hőtérkép feladat, amelyen keresztül található, ahol hello feladat túlnyomó hello idő, vagy a feladatot egy i/o-határ feladat, és így tovább.
    
      ![Azure Data Lake Analytics feladat graph halommemória térkép – példa](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-heat-map-example.png)
    
    * Folyamatban: hello feladat végrehajtásának menetét, kapcsolatban információk [szakasz információkat](#stage-information).
    * Az adatok olvasása/írása: hello hőtérkép teljes adatok az egyes fázisokban olvasása/írása.
    * Számítási idő: hello hőtérkép Sum (minden csomópontra végrehajtási idő), érdemes lehet ez, hogyan hosszú időt vesz igénybe Ha minden munkahelyi hello szakaszban csak 1 csúcspont végrehajtásához.
    * Csomópontonkénti átlagos végrehajtási idő: hello hőtérkép a Sum (minden csomópontra végrehajtási idő) / (csúcspont számát). Ami azt jelenti, hogy párhuzamossági végrehajtott összes hello csúcsban rendelheti, ha hello teljes szakasza a időkereten belül megtörténik.
    * Bemeneti/kimeneti átviteli sebesség: bemeneti/kimeneti sebességét, minden szakasza hello hőtérkép, ellenőrizheti a feladat esetén egy kötött i/o-feladat, ezáltal.
* Metaadat-művelet
  
    Néhány metaadatok műveleteinek elvégzéséhez a U-SQL parancsfájlt, mint például az adatbázis létrehozása, dobja el a tábla, stb. Ezek a műveletek fordítás után jelennek meg a metaadat-művelet. Akkor lehet, hogy keresés helyességi feltételek, entitásokat hozhatnak létre, húzza ide a entitások.
  
    ![Az Azure Data Lake Analytics feladat megtekintése metaadat-művelet](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view-metadata-operations.png)
* Állapotelőzmények
  
    hello State History is formájában jelenik meg a feladat összegzése, de a további részletekért itt. Részletes hello található adatokat például amikor hello feladat készen áll a testreszabásra, várakoznia, indulása, befejeződött. Is megtalálhatja hányszor hello feladat le van fordítva (hello CcsAttempts: 1), ha van ténylegesen hello feladat elküldött toohello fürt (részletes hello: feladat toocluster terjesztéséhez) stb.
  
    ![Az Azure Data Lake Analytics feladat megtekintése állapot előzményeinek](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view-state-history.png)
* Diagnosztika
  
    hello eszköz automatikusan diagnosztizálja a feladat végrehajtása. Riasztásokat fog kapni, amikor az egyes hibák vagy teljesítményproblémák a feladatok. Vegye figyelembe, hogy kell-e toodownload profil tooget teljes körű információkat itt. 
  
    ![Az Azure Data Lake Analytics feladat megtekintése diagnosztika](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view-diagnostics.png)
  
  * Figyelmeztetés: Riasztást jeleníti meg itt a fordító figyelmeztetést jelenít meg. Kattintson "x hiba/hibák" hivatkozás toohave további részleteket hello riasztás megjelenése után.
  * Futtassa a túl hosszú csúcspont: Ha bármilyen csúcspont elfogy idő (azaz 5 óra), problémák itt található.
  * Erőforrás-használat: Ha mint kell lefoglalni további vagy nem elég párhuzamossági problémák itt található. Is Erőforrás kihasználtsága toosee további részletekért kattintson, és hajtsa végre a lehetőségelemzések forgatókönyvek toofind jobb erőforrás-elosztás (További részletekért lásd a jelen útmutató).
  * Memória ellenőrzése: Ha bármely csúcspont 5 GB-nál több memóriát használ, problémák itt található. Feladat végrehajtását is beolvasása következtében leállt rendszer rendszerre vonatkozó korlátozás-nál több memóriát használ.

## <a name="job-detail"></a>Feladat részletei
Feladat részletei látható hello hello feladat, beleértve a parancsfájl, erőforrások és a Vertex végrehajtási nézetet részletes információkat.

![Azure Data Lake Analytics-feladat részletei](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-details.png)

* Szkript
  
    U-SQL parancsfájl hello feladat hello hello lekérdezéstár tárolja. Hello eredeti U-SQL parancsfájl megtekintése, és küldje el újra, ha szükséges.
* Erőforrások
  
    Hello feladat fordítási kimenetek hello lekérdezés tárolni forrásanyagok segítségével megtalálhatja. Például "algebra.xml", amely használt tooshow hello Feladatgrafikon regisztrált hello szerelvények stb. Itt találja.
* Vertex végrehajtási nézetet
  
    Csúcsban végrehajtási részleteit mutatja. hello feladat profil archiválja minden csúcspont végrehajtási napló, például az összes adat olvasása/írása, runtime, állapot, stb. Ebben a nézetben keresztül kaphat további részleteket a módját a feladat futott-e. További információkért lásd: [használata hello Vertex végrehajtási nézetet a Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).

## <a name="next-steps"></a>Következő lépések
* toolog diagnosztikai információkat, lásd: [diagnosztikai naplók elérése az Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md)
* egy összetettebb lekérdezés toosee lásd [elemzés webhely naplózza az Azure Data Lake Analytics használatával](data-lake-analytics-analyze-weblogs.md).
* toouse vertex végrehajtási nézetet, lásd: [használata hello Vertex végrehajtási nézetet a Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md)

