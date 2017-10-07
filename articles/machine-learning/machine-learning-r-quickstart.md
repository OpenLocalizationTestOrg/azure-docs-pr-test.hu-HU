---
title: "az R nyelv a Machine Learning aaaQuickstart oktatóanyag |} Microsoft Docs"
description: "Az R programozási útmutató tooget használatának gyors hello R nyelv az Azure Machine Learning Studio toocreate előrejelzési megoldást használni."
keywords: "gyors üzembe helyezés, r nyelven, r programozási nyelv, r programozási útmutató"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 99a3a0fd-b359-481a-b236-66868deccd96
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: garye
ms.openlocfilehash: 9995f8728f4d7bf9a5c15412015e4cf769cdac96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-tutorial-for-hello-r-programming-language-for-azure-machine-learning"></a>Az hello R programozási nyelv az Azure Machine Learning gyors üzembe helyezési útmutató

<!-- Stephen F Elston, Ph.D. -->

## <a name="introduction"></a>Bevezetés
Gyors üzembe helyezési oktatóanyag segítségével gyorsan az Azure Machine Learning kiterjesztése az hello R programozási nyelv használatával indíthat el. Kövesse a programozási útmutató toocreate R, tesztelése és R kódban az Azure Machine Learning segítségével hajtható végre. Oktatóanyag munka, előrejelzési teljes körű megoldást hoz létre az Azure Machine Learning hello R nyelv használatával.  

Számos hatékony gépi tanulási és adatok adatkezelési modulok Microsoft Azure Machine Learning tartalmazza. hello hatékony R nyelv szerint hello nyelv franca Analytics leírása. Elemzés és az adatok kezelése az Azure Machine Learning boldogan, bővíthető R. használatával Ez a kombináció biztosít hello méretezhetőséget és a központi telepítés az Azure Machine Learning könnyű hello rugalmasságot és az r mély elemzés

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

### <a name="forecasting-and-hello-dataset"></a>Az előrejelzés és hello adatkészlet
Az előrejelzés a széles körben munkavállalók és elég hasznos lehet analitikai módszer. Közös közé határozza cikkek optimális készlet szintek, toopredicting makrogazdasági változók meghatározása előrejelzésére használja. Az előrejelzés idő adatsorozat modellekkel való általában történik.

Adatsorozat időadatok az adatai, amelyben hello értékek rendelkezik idő indexszel. hello idő index rendszeres, lehet, például minden hónap vagy percben, vagy szabálytalan. Egy idősorozat-modellbe idő adatsorozat adatokon alapul. hello R programozási nyelv egy rugalmas keretrendszer és a széles körű elemzés idő adatsorozat adatokat tartalmazza.

Az első lépések útmutató rendszer kell kaliforniai tejtermelésre használata és díjszabási adatok. Ezek az adatok több tejtermékek hello éles és hello ár tejzsír, egy teljesítményteszt hagyományos havi információt tartalmaz.

Ebben a cikkben, és az R parancsfájlokat használt hello adatok [itt letöltött][download]. Ezeket az adatokat eredetileg történt synthesized származó információk érhetők el a következő http://future.aae.wisc.edu/tab/production.html egyetemi a Wisconsin hello.

### <a name="organization"></a>Szervezet
Megtudhatja, hogyan toocreate, tesztelése és elemzés és az adatok R adatkezelési kód végrehajtása hello Azure Machine Learning környezetben, azt fogja előrehaladás számos lépésen.  

* Először néhány hello R nyelv használatával hello Azure Machine Learning Studio környezetben hello alapjait.
* Ezután azt előrehaladás toodiscussing i/o adatok, az R-kód és a grafikus hello Azure Machine Learning-környezetben különböző jellemzőinek megtekintését.
* A Microsoft fog majd összeállítani hello első megoldás részét képező az előrejelzési adatok tisztítása és átalakítása kódjának létrehozásával.
* Az előkészített adataink végezzük el hello korrelációk között az adathalmaz hello változók számos elemzését.
* Végül létre fogunk hozni egy határozza idő series előrejelzési modell tejtermelésre.

## <a id="mlstudio"></a>Együttműködhet az R nyelv a Machine Learning Studióban
Ez a szakasz végigvezeti néhány hello R programozási nyelv hello Machine Learning Studio környezetben való interakció alapjait. hello R nyelv biztosít egy hatékony eszköz testre szabott toocreate elemzés és az adatok adatkezelési modulok hello Azure Machine Learning környezeten belül.

Kis méretű Rstudióból toodevelop, tesztelése és hibakeresési R-kód fogja használni. Ez a kód vágjon és a beillesztési műveleteket egy [R-parancsfájl végrehajtása] [ execute-r-script] a Machine Learning Studio készen toorun modul.  

### <a name="hello-execute-r-script-module"></a>hello R-parancsfájl végrehajtása modul
A Machine Learning Studio belül R parancsfájlok belül hello [R-parancsfájl végrehajtása] [ execute-r-script] modul. Példa hello [R-parancsfájl végrehajtása] [ execute-r-script] modul a Machine Learning Studióban az 1. ábrán látható.

 ![R programozási nyelv: hello R-parancsfájl végrehajtása modul kiválasztva a Machine Learning Studióban][1]

*1. ábra. hello Machine Learning Studio környezet megjelenítő hello R-parancsfájl végrehajtása modul kiválasztva.*

TooFigure 1 utaló, vizsgáljuk meg néhány hello hello használata a Machine Learning Studio környezet legfontosabb részeit hello [R-parancsfájl végrehajtása] [ execute-r-script] modul.

* hello modulok hello kísérletben hello középső ablaktáblán jelennek meg.
* hello jobb oldali ablaktábla felső részén hello egy ablak tooview tartalmazza, és az R parancsfájlok szerkesztése.  
* jobb oldali hello alsó részén látható hello néhány tulajdonságát [R-parancsfájl végrehajtása][execute-r-script]. Hello hiba és a kimeneti naplók hello megfelelő tesztüzeméhez, ha a panel kattintva tekintheti meg.

Azt fogja, természetesen kell megvitatása hello [R-parancsfájl végrehajtása] [ execute-r-script] Ez a dokumentum többi hello nagyobb részletességgel.

Az összetett R funkciók használatakor javasolt, hogy szerkesztése, tesztelése és hibakeresése a Rstudióból. Csakúgy, mint bármely szoftverfejlesztői Növekményesen kiterjesztése a kódot, és kis egyszerű vizsgálati eseteknél az tesztelik azt. Majd Kivágás, és illessze be a funkciók hello R parancsfájl ablakának hello [R-parancsfájl végrehajtása] [ execute-r-script] modul. Ez a megközelítés lehetővé teszi tooharness egyaránt hello Rstudióból integrált fejlesztési környezeti (IDE) és hatványra emelésének Azure Machine Learning hello.  

#### <a name="execute-r-code"></a>R-kód végrehajtása
Hello bármely R-kód [R-parancsfájl végrehajtása] [ execute-r-script] modul végrehajtja a hello kísérlet hello kattintva futtatásakor **futtatása** gombra. Végrehajtás befejezése után megjelenik a négyzet hello [R-parancsfájl végrehajtása] [ execute-r-script] ikonra.

#### <a name="defensive-r-coding-for-azure-machine-learning"></a>Defenzív R kódolási Azure Machine Learning
Tegyük fel például, egy webszolgáltatás-bővítmény az R-kód fejlesztői Azure Machine Learning segítségével, meg kell mindenképpen terveznie módját a kód foglalkozni fog váratlan adatok bemeneti és a kivételek. toomaintain átláthatóság érdekében I nem szereplő nagy ellenőrzése vagy a legtöbb látható hello kódpéldák kivételkezelő hello módon. Azonban, a Folytatás I fogja diktálni funkciók számos példát R-hez tartozó kivételkezelő funkció használatával.  

Ha egy R kivételkezelést teljesebb kezelésére van szüksége, I javasoljuk, hogy olvassa el a megfelelő szakaszait hello hello könyv felsorolt Wickham által [B függelék – további olvasási](#appendixb).

#### <a name="debug-and-test-r-in-machine-learning-studio"></a>Hibakeresését és tesztelését R a Machine Learning Studióban
tooreiterate, javasolt tesztelése és hibakeresése az R-kód az Rstudióból kis méretű. Azonban, vannak esetek, ahol tootrack R kódproblémáknak az hello le kell [R-parancsfájl végrehajtása] [ execute-r-script] magát. Ezenkívül azt is célszerű toocheck az eredményeket a Machine Learning Studio.

Az R-kód és hello Azure Machine Learning platformon hello végrehajtási eredményének elsősorban megtalálható kimenetét. További információk a error.log fogja látni.  

Ha a hiba akkor fordul elő, a Machine Learning Studióban az R-kód futtatása során, az első lépések: error.log toolook kell lennie. Ez a fájl hasznos hiba üzenetek toohelp tudomásul veszi, és javítsa ki a hibát tartalmazza. tooview error.log, kattintson a **nézet hibanapló** a hello **tulajdonságok panelen** a hello [R-parancsfájl végrehajtása] [ execute-r-script] hello hibát tartalmazó.

Például az R-kód, nem definiált változó y, a következő hello futtattam egy [R-parancsfájl végrehajtása] [ execute-r-script] modul:

    x <- 1.0
    z <- x + y

Ez a kód nem sikerül tooexecute hibát eredményez. Kattintson a **nézet hibanapló** a hello **tulajdonságok panelen** előállított hello megjelenítési 2. ábrán látható.

  ![Felugró hibaüzenet][2]

*2. ábra. Előugró hibaüzenet.*

Úgy tűnik, igazolnia kell a kimenetét toosee hello R hibaüzenet toolook. Kattintson a hello [R-parancsfájl végrehajtása] [ execute-r-script] majd kattintson a hello **kimenetét megtekintése** hello cikk **tulajdonságok panelen** toohello jobbra. Egy új böngészőablakban nyílik meg, és a következő hello meg.

    [Critical]     Error: Error 0063: hello following error occurred during evaluation of R script:
    ---------- Start of error message from R ----------
    object 'y' not found


    object 'y' not found
    ----------- End of error message from R -----------

Ez a hibaüzenet nem meglepetések számát tartalmazza, és egyértelműen azonosítják a hello probléma.

az R bármely objektum tooinspect hello értéket, nyomtathatja ki ezen értékek toohello kimenetét fájlt. hello szabályok objektumot vizsgáló értékek a következők hello ugyanaz, mint egy interaktív R munkamenet. Például sorban adjon meg egy változónevet, hello objektum hello értéket nyomtatott toohello kimenetét fájl lesz.  

#### <a name="packages-in-machine-learning-studio"></a>A Machine Learning Studióban csomagok
Az Azure Machine Learning több mint 350 előre telepített R nyelvi csomagokat tartalmaz. Használhatja a következő kódot a hello hello [R-parancsfájl végrehajtása] [ execute-r-script] modul tooretrieve hello listája előre telepített csomagokat.

    data.set <- data.frame(installed.packages())
    maml.mapOutputPort("data.set")

Ez a kód utolsó sora hello hello pillanatban nem megismerése, olvassa el a. Ez a dokumentum többi hello nagymértékben ismertetik hello Azure Machine Learning környezet az R nyelv használatát.

### <a name="introduction-toorstudio"></a>Bevezetés tooRStudio
Rstudióból egy széles körben használt IDE az R. A Szerkesztés, a tesztelés és a hibakereséshez hello R-kód gyors üzembe helyezési útmutatóban használt egyes használom Rstudióból. Ha tesztelt, és készen áll, az R-kód egyszerűen kimásolni, majd a hello Rstudióból szerkesztő illessze be a Machine Learning Studio [R-parancsfájl végrehajtása] [ execute-r-script] modul.  

Ha nem rendelkezik hello R programozási nyelv a asztali gépen telepítve van, akkor tegye meg most javasolt. Ingyenes letöltést nyílt forráskódú R nyelv hello átfogó R archív hálózati (CRAN) webhelyen érhetők el: [http://www.r-project.org/](http://www.r-project.org/). Nincsenek elérhető a Windows, Mac OS és Linux/UNIX letöltések. Válassza ki a közeli tükrözött, és kövesse hello letöltési utasításokat. Ezenkívül a CRAN számos hasznos elemzés és az adatok adatkezelési csomagok olyan tartalmazza.

Ha új tooRStudio, töltse le és telepítse hello asztali verzióját. Rstudióból tölti le a Windows, Mac OS és Linux/UNIX http://www.rstudio.com/products/RStudio/: hello található. Kövesse az asztali gépen tooinstall Rstudióból megadott hello utasításokat.  

Egy bevezető oktatóanyag tooRStudio https://support.rstudio.com/hc/sections/200107586-Using-RStudio érhető el.

I néhány kiegészítő információt nyújt az Rstudióból használatával [függelék][appendixa].  

## <a id="scriptmodule"></a>Mindkét hello R-parancsfájl végrehajtása modul beolvasása
Ebben a szakaszban ismertetjük, hogyan elérhetővé adatok esetében bejövő és kimenő hello [R-parancsfájl végrehajtása] [ execute-r-script] modul. Hogyan toohandle különféle adatok olvasása-hello fog tanulmányozzák [R-parancsfájl végrehajtása] [ execute-r-script] modul.

hello teljes kód látható az ebben a szakaszban korábban letöltött zip-fájl hello van.

### <a name="load-and-check-data-in-machine-learning-studio"></a>Betölteni, és ellenőrizze az adatokat a Machine Learning Studióban
#### <a id="loading"></a>Hello adatkészlet betöltése
Először hello betöltésével **csdairydata.csv** Azure Machine Learning Studio fájlból.

* Indítsa el az Azure Machine Learning Studio környezetet.
* Kattintson a **+ új** hello, a képernyő, és válassza ki a bal alsó **Dataset**.
* Válassza ki **helyi fájlból**, majd **Tallózás** tooselect hello fájlt.
* Győződjön meg arról, hogy a kijelölt **általános CSV-fájl (.csv) fejlécű** hello adatkészlet hello típusként.
* Kattintson a hello pipára.
* Hello dataset feltöltése után megtekintheti az új adatkészlet hello hello kattintva **adatkészletek** fülre.  

#### <a name="create-an-experiment"></a>A kísérlet létrehozása
Most, hogy néhány adat a Machine Learning Studióban, igazolnia kell a toocreate kísérlet toodo hello elemzését.  

* Kattintson a **+ új** hello a bal alsó, és válassza ki **kísérlet**, majd **üres kísérlet**.
* Meg kísérletbe kiválasztásával, és módosítja, hello **kísérlet létre...**  cím hello oldal hello tetején. Például megváltoztathatja túl**hitelesítésszolgáltató tejtermék Analysis**.
* Hello hello kísérlet lap bal oldali, bontsa ki a **mentett adatkészletek**, majd **saját adatkészletek**. Megtekintheti az hello **cadairydata.csv** korábban feltöltött.
* A fogd és vidd hello **csdairydata.csv dataset** alakzatot hello kísérlet.
* A hello **keresési kísérletezhet elemek** hello felső részén hello bal oldali ablaktáblán típus mezőjében [R-parancsfájl végrehajtása][execute-r-script]. Látni fogja hello modul hello keresési listában jelennek meg.
* A fogd és vidd hello [R-parancsfájl végrehajtása] [ execute-r-script] modul alakzatot a sorát tartalmazza.  
* Csatlakozás hello hello kimenete **csdairydata.csv adatkészlet** toohello bal szélső bemeneti (**Dataset1**) a hello [R-parancsfájl végrehajtása][execute-r-script].
* **Ne felejtse el a "Mentés" tooclick!**  

Ezen a ponton a kísérletben hasonlóan kell kinéznie 3. ábra.

![a DataSet adatkészlet és R-parancsfájl végrehajtása modul kísérletezhet hello hitelesítésszolgáltató tejtermék elemzés][3]

*3. ábra. Hitelesítésszolgáltató tejtermék elemzés hello dataset és R-parancsfájl végrehajtása modul kísérletezhet.*

#### <a name="check-on-hello-data"></a>Hello adatok ellenőrzése
Most rá egy pillantást hello adatok azt töltött be a kísérletet. Hello kísérletben, kattintson a hello hello kimenete **cadairydata.csv dataset** válassza **megjelenítése**. 4. ábra hasonlót meg kell jelennie.  

![Hello cadairydata.csv dataset összegzése][4]

*4. ábra. Hello cadairydata.csv adatkészlet összegzését.*

Ebben a nézetben látható nagy mennyiségű hasznos információkat. Láthatja, hogy a dataset első több sornyi hello. Ha olyan oszlopot válasszon ki, hogy a hello statisztika szakasz hello oszlop további információkat jeleníti meg. Például hello szolgáltatástípus sor látható az Azure Machine Learning Studio hozzárendelt toohello oszlop milyen adatokat. Ehhez hasonló gyorsan át, akkor az a jó megerősítést ellenőrzés előtt súlyos munka toodo először.

### <a name="first-r-script"></a>Első R-parancsfájl
Az Azure Machine Learning Studióban hozzon létre egy egyszerű első R parancsfájl tooexperiment együtt. Létrehozta és tesztelte hello Rstudióból-parancsfájl a következő.  

    ## Only one of hello following two lines should be used
    ## If running in Machine Learning Studio, use hello first line with maml.mapInputPort()
    ## If in RStudio, use hello second line with read.csv()
    cadairydata <- maml.mapInputPort(1)
    # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
    str(cadairydata)
    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata)
    ## hello following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('cadairydata')

Most kell tootransfer a parancsfájl tooAzure Machine Learning Studio. Sikerült egyszerűen Kivágás, majd illessze be. Azonban ebben az esetben fogja átvinni az R-parancsfájl egy zip-fájl használatával.

### <a name="data-input-toohello-execute-r-script-module"></a>Adatok bemeneti toohello R-parancsfájl végrehajtása modul
Most rendelkezik egy pillantást hello bemenetek toohello [R-parancsfájl végrehajtása] [ execute-r-script] modul. Ebben a példában a Microsoft hello kaliforniai tejelő adatokat olvassa be hello [R-parancsfájl végrehajtása] [ execute-r-script] modul.  

Nincsenek hello három lehetséges bemeneteit [R-parancsfájl végrehajtása] [ execute-r-script] modul. Bármely egyikét vagy mindegyikét a bemenetek, attól függően, hogy az alkalmazás használhat. Akkor is, amelyek egyáltalán nem bemenetből fogad adatokat tökéletesen ésszerű toouse az R-parancsfájl.  

Vizsgáljuk meg mindegyik származó, a bal oldali tooright is. Az egyes hello bemenetek hello nevek helyezi el a kurzor fölé hello bemeneti és hello elemleírás tekintheti meg.  

#### <a name="script-bundle"></a>Parancsfájl-csomag
hello parancsfájl köteg bemeneti lehetővé teszi a toopass hello tartalmát egy zip-fájlba történő [R-parancsfájl végrehajtása] [ execute-r-script] modul. Hello parancsok tooread hello hello zip-fájl tartalmát az R-kód a következő egyikét használhatja.

    source("src/yourfile.R") # Reads a zipped R script
    load("src/yourData.rdata") # Reads a zipped R data file

> [!NOTE]
> Az Azure Machine Learning kezeli a hello zip fájlt, ha azok hello src / könyvtár, ezért meg kell, hogy a fájl nevét, a könyvtárnév tooprefix. Például ha hello zip hello fájlokat tartalmaz `yourfile.R` és `yourData.rdata` hello legfelső szintű hello zip, meg kellene cím ezek `src/yourfile.R` és `src/yourData.rdata` használatakor `source` és `load`.
> 
> 

Ismertettük már adathalmaz betöltése [hello adatkészlet betöltése](#loading). Miután létrehozta és tesztelte hello előző részben hello R-parancsfájl, a hello, a következő:

1. Mentse a hello R parancsfájlt a. R-fájl. A parancsfájl "simpleplot jelentkezem. R". Az alábbiakban hello tartalmát.
   
        ## Only one of hello following two lines should be used
        ## If running in Machine Learning Studio, use hello first line with maml.mapInputPort()
        ## If in RStudio, use hello second line with read.csv()
        cadairydata <- maml.mapInputPort(1)
        # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
        str(cadairydata)
        pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata)
        ## hello following line should be executed only when running in
        ## Azure Machine Learning Studio
        maml.mapOutputPort('cadairydata')
2. Hozzon létre egy zip fájlt, és másolja a parancsfájlt a zip-fájlba. A Windows hello fájl gombbal és válassza **küldése**, majd **tömörített mappa**. Ezzel létrehoz egy új zip-fájlt tartalmazó hello "simpleplot. R"fájl.
3. Adja hozzá a fájl toohello **adatkészletek** a Machine Learning Studióban hello típus megadása **zip**. Ekkor megjelenik az adatkészletek hello zip-fájl.
4. A fogd és vidd hello zip-fájl a **adatkészletek** alakzatot hello **ML Studio vászonra**.
5. Csatlakozás hello hello kimenete **zip-adatok** ikon toohello **parancsfájl köteg** hello a bemeneti [R-parancsfájl végrehajtása] [ execute-r-script] modul.
6. Típus hello `source()` függvény a zip fájlt hello kód ablakba a hello [R-parancsfájl végrehajtása] [ execute-r-script] modul. Abban az esetben, ha a beírt `source("src/simpleplot.R")`.  
7. Ellenőrizze, hogy **mentése**.

Ha ezeket a lépéseket befejeződött, hello [R-parancsfájl végrehajtása] [ execute-r-script] modul végrehajtja a hello R-parancsfájl hello zip-fájlban szereplő hello kísérlet futtatásakor. Ezen a ponton a kísérletben hasonlóan kell kinéznie 5. ábra.

![Kísérlet zip R-parancsfájl használatával][6]

*5. ábra. A kísérletben zip R-parancsfájl használatával.*

#### <a name="dataset1"></a>Dataset1
Adatok tooyour R kód téglalap alakú táblázatát átadhatók hello Dataset1 bemeneti használatával. Az egyszerű parancsprogram hello a `maml.mapInputPort(1)` függvény hello adatokat olvas %1 porton. Ezeket az adatokat, majd hozzá van rendelve a kódban tooa dataframe változónév. Az egyszerű parancsfájlban hello első kódsort hello hozzárendelés hajt végre.

    cadairydata <- maml.mapInputPort(1)

Hajtsa végre a kísérlet hello kattintva **futtatása** gombra. Hello végrehajtásának befejezése után kattintson a hello [R-parancsfájl végrehajtása] [ execute-r-script] modul, és kattintson **kimeneti napló megtekintése** hello tulajdonságai panelen. Egy új lapot ábrázoló hello hello kimenetét fájl tartalmát a böngésző meg kell jelennie. Amikor görgessen lefelé megtekintheti az alábbihoz hasonló hello.

    [ModuleOutput] InputDataStructure
    [ModuleOutput]
    [ModuleOutput] {
    [ModuleOutput]  "InputName":Dataset1
    [ModuleOutput]  "Rows":228
    [ModuleOutput]  "Cols":9
    [ModuleOutput]  "ColumnTypes":System.Int32,3,System.Double,5,System.String,1
    [ModuleOutput] }

Lefelé hello távolabb a lap hello oszlopokat, amelyek a következőhöz hello következő hasonlóan fog kinézni a további tájékoztatáshoz.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput]
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput]
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput]
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput]
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput]
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput]
    [ModuleOutput]  $ Month            : chr  "Jan" "Feb" "Mar" "Apr" ...
    [ModuleOutput]
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput]
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput]
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput]
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...

Ezekkel az eredményekkel többnyire 228 megfigyelések és hello dataframe 9 oszlopai várt módon vannak. Hello oszlopnevek, hello R adattípus és egy minta oszlopok láthatja.

> [!NOTE]
> Az azonos nyomtatásra érhető kényelmesen hello hello R eszköz kimenete [R-parancsfájl végrehajtása] [ execute-r-script] modul. Hello hello kimenetének ismertetik [R-parancsfájl végrehajtása] [ execute-r-script] modul hello a következő szakaszban.  
> 
> 

#### <a name="dataset2"></a>Dataset2
hello hello Dataset2 bemeneti működése azonos toothat a Dataset1. Használja a bemeneti átviheti az adatok egy második téglalap alakú tábla az R-kódba. hello függvény `maml.mapInputPort(2)`, 2 hello argumentummal van használt toopass ezeket az adatokat.  

### <a name="execute-r-script-outputs"></a>R-parancsfájl kimenetek végrehajtása
#### <a name="output-a-dataframe"></a>Egy dataframe kimeneti
A kimenetre küldheti hello tartalmát egy R dataframe hello eredmény Dataset1 porton keresztül téglalap alakú táblázatként hello segítségével `maml.mapOutputPort()` függvény. Az egyszerű R-parancsfájl ez hajtható végre a következő sor hello.

    maml.mapOutputPort('cadairydata')

Futó hello kísérletet követően kattintson a hello eredmény Dataset1 kimeneti portra, és kattintson a **Visualize**. 6. ábra hasonlót meg kell jelennie.

![hello képi megjelenítés hello kibocsátásának hello kaliforniai tejelő adatok][7]

*6. ábra. hello képi megjelenítés hello kaliforniai tejelő adatok hello kimenetét.*

A kimeneti azonos toohello bemeneti keres, pontosan úgy várt.  

### <a name="r-device-output"></a>R eszköz kimeneti
Eszköz kimenete hello hello [R-parancsfájl végrehajtása] [ execute-r-script] modul üzenetek és grafikus kimeneti tartalmazza. Mindkét standard kimenet és a standard hiba R üzenetei toohello R eszköz kimeneti portjával.  

tooview hello R eszköz kimeneti, kattintson a hello porton, majd a **Visualize**. Hello standard kimenet és a standard hiba a hello R-parancsfájl a 7. ábrán látható.

![Standard kimenet és a standard hiba a hello R eszköz port][8]

*7. ábra. Standard kimenet és a standard hiba a hello R eszköz port.*

Azt lefelé görgetéshez használható tekintse meg a 8. ábrán az R-parancsfájl kimenete hello grafikus.  

![Hello R eszköz port grafikus kimenete][9]

*8. ábra. A grafikus hello R eszköz port kimenetét.*  

## <a id="filtering"></a>Adatok szűrése és átalakítás
Ebben a szakaszban végezzük el a néhány alapvető adatok szűrése és a hello kaliforniai tejelő adatok átalakítási műveletek. Ez a szakasz hello végéig azt van adatok formátuma nem megfelelő az elemzési modell készítéséhez.  

Pontosabban, ez a szakasz azt feladatokat kell elvégezni több közös adatok tisztítása és átalakítás: Adja meg a szűrést dataframes, hozzáadása új számított oszlop, az átalakítási és átalakítások értéket. A háttérben segítséget valós problémákat észlelt számos változata hello foglalkozik.

Ez a szakasz hello teljes R kód hello korábban letöltött zip-fájlban szereplő érhető el.

### <a name="type-transformations"></a>Típus átalakítások
Most, hogy a Microsoft hello kaliforniai tejelő adatok elolvashatják a hello R kódra a hello [R-parancsfájl végrehajtása] [ execute-r-script] modul, igazolnia kell, hogy hello adatok hello oszlopban rendelkezik szánt hello típusát és formátumát tooensure.  

R dinamikusan gépelt nyelven, ami azt jelenti, hogy az adattípusokat egy tooanother szükség szerint a rendszer kényszerített. atomi adattípusok hello R numerikus, logikai és karaktert tartalmaz. hello tényező típus használt toocompactly kategorikus adatok tárolása. Adattípusok sokkal további információk találhatók hello hivatkozások [B függelék – további információ](#appendixb).

Táblázatos adatolvasás a külső forrásból származó R, esetén mindig egy jó ötlet toocheck hello származó típusok hello oszlopok. Érdemes lehet egy oszlophoz Típusjelző karakter, de sok esetben ez fog megjelenni tényezőként vagy fordítva. Más esetekben egy oszlopot úgy gondolja, hogy legyen numerikus által képviselt karakter adatok, például az 1,23, lebegőpontos helyett a "1,23" pont számát.  

Szerencsére is könnyen tooconvert egy típus tooanother, mindaddig, amíg lesz lehetséges. Például "Nevada" konvertálása nem egy numerikus érték, de akkor átalakíthatja tooa tényező (kategorikus változó). Másik példaként egy numerikus 1 átalakíthatja a következő karaktert: "1" vagy egy tényező.  

hello ezek az átalakítások bármelyikét szintaxisa a következő egyszerű: `as.datatype()`. A típuskonverziós hello következők.

* `as.numeric()`
* `as.character()`
* `as.logical()`
* `as.factor()`

Azt az előző szakaszban hello bemeneti hello oszlopok adattípusai hello megnézi: az összes oszlop sem típusú numerikus, "Month", amelynek a típuskarakter feliratú hello oszlopok kivételével. Most a tooa tényező átalakítani, és hello teszteredmények.  

Hello scatterplot mátrix létrehozza és hozzáadja egy sor hello "Month" oszlop tooa tényező konvertálása hello sor rendelkezik törölve A kísérletemben I fog csak kivágása és hello R-kód beillesztése hello kód ablakának hello [R-parancsfájl végrehajtása] [ execute-r-script] modul. Is képes hello zip-fájl frissítése, és töltse fel a Machine Learning Studio tooAzure, de ez számos lépést időt vesz igénybe.  

    ## Only one of hello following two lines should be used
    ## If running in Machine Learning Studio, use hello first line with maml.mapInputPort()
    ## If in RStudio, use hello second line with read.csv()
    cadairydata <- maml.mapInputPort(1)
    # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
    ## Ensure hello coding is consistent and convert column tooa factor
    cadairydata$Month <- as.factor(cadairydata$Month)
    str(cadairydata) # Check hello result
    ## hello following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('cadairydata')

Most futtassa ezt a kódot, és tekintse meg az R-parancsfájl hello hello kimeneti naplót. hello vonatkozó adatokat hello naplóból 9. ábra jelenik meg.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 14 levels "Apr","April",..: 6 5 9 1 11 8 7 3 14 13 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

*9. ábra. Hello dataframe tényező változóhoz összegzését.*

hello típus hónaphoz most üzenetnek kell megjelennie "**keresztüli 14 szintek rendelkező tényező**". Ez az probléma, mivel nincsenek hello évben csak 12 hónapig. Ellenőrizheti, hogy a típus hello toosee a **Visualize** hello eredmény Dataset port az "**Categorical**".

hello probléma az, hogy hello "Month" oszlop nem lett a kódolt rendszeresen. Bizonyos esetekben egy hónap. április nevezik, és más, ápr rövidítéseket tartalmaz. A Microsoft hello karakterlánc too3 karakterek segítségével megoldhatja a problémát. hello kódsort most következőhöz hasonló hello:

    ## Ensure hello coding is consistent and convert column tooa factor
    cadairydata$Month <- as.factor(substr(cadairydata$Month, 1, 3))

Futtassa újra a hello kísérletet, és tekintse meg hello kimeneti naplót. hello várt eredmény 10. ábrán láthatók.  

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

*10. ábra. Megfelelő számú tényező szintek hello dataframe összegzését.*

A tényező változó most már rendelkezik a szükséges hello 12 szintek.

### <a name="basic-data-frame-filtering"></a>Alapvető adatok keret szűrése
R dataframes hatékony szűrési lehetőségeket nyújtanak. Adatkészletek sorok vagy oszlopok logikai szűrők használatával lehet részlegesen beágyazott. Sok esetben összetett szűrési feltételeket lesz szükség. hello sorszámra [B függelék – további információ](#appendixb) dataframes szűrés kiterjedt példái tartalmaz.  

Van egy bittel szűrése az adathalmaz azt kell végeznie. Hello cadairydata dataframe hello oszlopai tekinti meg, két felesleges oszlopok látják. hello első oszlop csak egy sor számát, amely nem nagyon hasznos tartalmazza. hello második oszlopban Year.Month, redundáns információkat tartalmaz. Azt is könnyen használatával zárhatja ki ezeket az oszlopokat a következő R-kód hello.

> [!NOTE]
> A most az ebben a szakaszban I ugyanúgy jelennek meg hello hello a hozzáadott kód [R-parancsfájl végrehajtása] [ execute-r-script] modul. Minden új sor veszek fel **előtt** hello `str()` függvény. Használhatom a függvény tooverify az eredmények az Azure Machine Learning Studióban.
> 
> 

A következő sor toomy R kód hello hello hozzáadok [R-parancsfájl végrehajtása] [ execute-r-script] modul.

    # Remove two columns we do not need
    cadairydata <- cadairydata[, c(-1, -2)]

Futtassa a következő kódot a kísérletben, és ellenőrizze a hello kimeneti naplóból hello eredménye. Ezekkel az eredményekkel 11. ábra mutatja be.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  7 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

*11. ábra. A két oszlop hello dataframe összefoglalása eltávolítani.*

Jó hírünk van! A Microsoft hello várt eredmény érhető el.

### <a name="add-a-new-column"></a>Egy olyan új oszlop hozzáadása
toocreate idő adatsorozat modellek lesz kényelmes toohave egy oszlopot tartalmazó hello hónap hello idősorozat hello indítása óta. Létrehozunk egy olyan új oszlop "Month.Count".

toohelp rendszerezése létre fogunk hozni az első egyszerű függvény hello kód `num.month()`. Fog majd érvénybe lépni a függvény toocreate hello dataframe új oszlopát. hello új kódot a következőképpen történik.

    ## Create a new column with hello month count
    ## Function toofind hello number of months from hello first
    ## month of hello time series
    num.month <- function(Year, Month) {
      ## Find hello starting year
      min.year  <- min(Year)

      ## Compute hello number of months from hello start of hello time series
      12 * (Year - min.year) + Month - 1
    }

    ## Compute hello new column for hello dataframe
    cadairydata$Month.Count <- num.month(cadairydata$Year, cadairydata$Month.Number)

Most frissíteni hello kísérlet futtatása, és hello kimeneti napló tooview hello eredmények használja. Ezekkel az eredményekkel 12. ábra mutatja be.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

*12. ábra. Hello dataframe hello további oszlop összegzése.*

Úgy tűnik, minden működik. Új oszlop hello hello várt értékei a dataframe a van.

### <a name="value-transformations"></a>Érték átalakítása
Ebben a szakaszban végezzük el néhány egyszerű átalakítások hello értékek egyes a dataframe hello oszlopait. hello R nyelv majdnem tetszőleges érték átalakítások támogatja. hello sorszámra [B függelék – további olvasási](#appendixb) kiterjedt példákat tartalmaz.

Ha megnézi a dataframe hello összegzéseinek hello értékeket kell valami páratlan itt. Kaliforniai előállított tej-nál több jégkrémje? Nem, természetesen ez nincs értelme, mivel EV, mint a tény lehet, hogy nem mindannyian toosome jégkrémje lovers. hello egységek eltérnek. hello ár egységekbe, az font, tej van egységekbe, az 1 millió egység USA font, jégkrémje 1000 egységekbe gallon VELÜNK, pedig túró egységekbe, az USA 1000 font. Ha jégkrémje tömege / gallonra készül 6.5 font, azt is könnyen hello szorzás tooconvert ezeket az értékeket úgy, hogy azok mind 1000 font egyenlő egységekben.

Az előrejelzési modell azt használjon tényezőt modellt a trendek és a határozza ezeket az adatokat. Napló-átalakítás lehetővé teszi egy lineáris modell, hogy az a folyamat toouse. Azt is érvényesek lesznek hello napló átalakítása a hello olyan funkciókat, amikor a hello szorzója van érvényben.

A következő kód hello, I határozza meg egy új funkció `log.transform()`, és alkalmazza azt hello numerikus értékeket tartalmazó toohello sorok. hello R `Map()` függvény használt tooapply hello `log.transform()` függvény toohello kijelölt hello dataframe oszlopait. `Map()`túl hasonlít`apply()` , de lehetővé teszi, hogy a argumentumok toohello függvény egynél több listáját. Vegye figyelembe, hogy tényezők listájának megadja az hello második argumentum toohello `log.transform()` függvény. Hello `na.omit()` kicsit tisztítás tooensure, nem találtunk hiányzik vagy nem definiált értékek hello dataframe függvény használatos.

    log.transform <- function(invec, multiplier = 1) {
      ## Function for hello transformation, which is hello log
      ## of hello input value times a multiplier

      warningmessages <- c("ERROR: Non-numeric argument encountered in function log.transform",
                           "ERROR: Arguments toofunction log.transform must be greate than zero",
                           "ERROR: Aggurment multiplier toofuncition log.transform must be a scaler",
                           "ERROR: Invalid time seies value encountered in function log.transform"
                           )

      ## Check hello input arguments
      if(!is.numeric(invec) | !is.numeric(multiplier)) {warning(warningmessages[1]); return(NA)}  
      if(any(invec < 0.0) | any(multiplier < 0.0)) {warning(warningmessages[2]); return(NA)}
      if(length(multiplier) != 1) {{warning(warningmessages[3]); return(NA)}}

      ## Wrap hello multiplication in tryCatch
      ## If there is an exception, print hello warningmessage to
      ## standard error and return NA
      tryCatch(log(multiplier * invec),
               error = function(e){warning(warningmessages[4]); NA})
    }


    ## Apply hello transformation function toohello 4 columns
    ## of hello dataframe with production data
    multipliers  <- list(1.0, 6.5, 1000.0, 1000.0)
    cadairydata[, 4:7] <- Map(log.transform, cadairydata[, 4:7], multipliers)

    ## Get rid of any rows with NA values
    cadairydata <- na.omit(cadairydata)  

Nincs elég egy bit azonban a hello `log.transform()` függvény. Ez a kód a legtöbb hello argumentumok kapcsolatos esetleges problémák keresése vagy kivételek, továbbra is hello számítások során felmerülő foglalkoznak. Ez a kód csak néhány sornyi ténylegesen hello számításokat.

hello hello defenzív programozás célja tooprevent hello sikertelen volt-e, amely megakadályozza a folyamatos feldolgozási egy funkcióval. Lehet, hogy a hosszan futó elemzési hirtelen hibát igen kellemetlen, a felhasználók számára. Ebben az esetben alapértelmezett visszatérési értékek ki kell választani, amely korlátozza az tooavoid toodownstream feldolgozási sérülést. Egy üzenet egyben előállított tooalert felhasználók, amelynek valami hibás.

Ha még nem használt toodefensive programozásba a R, ez a kód egy kicsit túlságosan tűnhet. I haladhat végig hello fő lépést:

1. Négy üzenetek vektor van definiálva. Ezek az üzenetek nem használt toocommunicate tájékoztatásra hello lehetséges hibákat és kivételeket, amik akkor léphetnek fel ezzel a kóddal.
2. NA érték minden egyes esetben vissza Nincsenek sok egyéb lehetőségeket, amelyekkel kevesebb hatásai lehetnek. I visszaadhatja a vektoros nullából álló, illetve ha hello eredeti bemeneti vektor, például.
3. A hello argumentumok toohello függvény ellenőrzéseket futtatja. Minden esetben a rendszer hibát észlel, ha ki egy alapértelmezett értéket adja vissza, és üzenet hello hozzák `warning()` függvény. Használok `warning()` helyett `stop()` , ez utóbbi hello megszünteti a végrehajtási, pontosan milyen próbálok tooavoid. Vegye figyelembe, hogy I írt ezt a kódot eljárási Style, ahogy a gyorsítás esetében a rendszer valószínűleg összetett és homályos működési megközelítést.
4. hello napló számítások csomagolni vannak `tryCatch()` , hogy a kivételek nem okoz egy hirtelen halt tooprocessing. Nélkül `tryCatch()` R funkciók eredményt stopjelzést, viszont csak, amelyek miatt a legtöbb hiba.

Az R-kód végrehajtása a kísérletben, és kinyomtatott hello egy pillantást kimeneti hello kimenetét fájlban. Ekkor hello négy oszlopai naplózásához hello hello át legyenek-e értékeket is, ahogy az 13. ábra.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

*13. ábra. Hello összefoglalása hello dataframe szereplő értékek átalakítva.*

Hello értékek átalakítása látható. Most tejtermelés nagy mértékben meghaladja minden más tejtermék éles, hogy azt most nézi a Logaritmikus skála tárolóról.

Ezen a ponton az adatok törlődnek, és készen áll az egyes modellezési azt. Hello képi megjelenítés eredménye Dataset a kimeneti hello összegzése megnézi a [R-parancsfájl végrehajtása] [ execute-r-script] modul, látni fogja hello "Month" oszlop: "Categorical" 12 egyedi értékekkel újra, ugyanúgy, mint a akartunk .

## <a id="timeseries"></a>Idő adatsorozat objektumok és a korrelációs elemzés
Ebben a szakaszban rendszer R idő néhány alapvető adatsorozat objektumok felfedezése és elemezheti hello korrelációk néhány hello változók között. Célunk toooutput egy dataframe hello páros korrelációs információkat tartalmazó, több késedelmes jelentések.

Ez a szakasz hello teljes R kód hello korábban letöltött zip-fájl van.

### <a name="time-series-objects-in-r"></a>Az R idő adatsorozat objektumok
Már említettük, az idő a adatsorokat idő indexelik adatértékek sorozata. R idő adatsorozat objektumok használt toocreate és hello idő index kezelése. Nincsenek több előnyeit toousing idő adatsorozat objektumok. Idő adatsorozat objektumok mentes, hello számos részletével hello idő index sorozatértékek hello objektum ágyazott kezelése. Ezenkívül idő adatsorozat objektumok lehetővé teszik a toouse hello sok időt adatsorozat módszerek ábrázolásához, nyomtatási, modellezési stb.

hello POSIXct idő adatsorozat osztály általában arra használják, és viszonylag egyszerű. Az idő adatsorozat osztály hello kezdetét hello epoch, 1970. január 1. a időt méri. Ebben a példában használjuk POSIXct idő adatsorozat objektumok. Más széles körben használt R idő adatsorozat objektum osztályok itt és xts, bővíthető idősor tartalmazza.
<!-- Additional information on R time series objects is provided in hello references in Section 5.7. [commenting because this section doesn't exist, even in hello original] -->

### <a name="time-series-object-example"></a>Idő adatsorozat objektum – példa
Lássunk neki a fenti példában. Húzza a **új** [R-parancsfájl végrehajtása] [ execute-r-script] modult a kísérletvászonra. Csatlakozás hello eredmény Dataset1 kimeneti port hello meglévő [R-parancsfájl végrehajtása] [ execute-r-script] modul toohello Dataset1 adjon meg új hello portja [R-parancsfájl végrehajtása] [ execute-r-script] modul.

Ahogyan hello első példák, mint azt hello például bizonyos időpontokban I megjeleníti a folyamat csak hello növekményes további kódsorokat R lépésről lépésre.  

#### <a name="reading-hello-dataframe"></a>Hello dataframe olvasása
Első lépésként tegyük egy dataframe olvasása, és győződjön meg arról, hogy a Microsoft hello várt eredményekhez juthat. hello alábbira tegyen hello feladat.

    # Comment hello following if using RStudio
    cadairydata <- maml.mapInputPort(1)
    str(cadairydata) # Check hello results

Most futtassa a hello kísérlet. hello napló hello új R-parancsfájl végrehajtása alakzat 14. ábra hasonlóan kell kinéznie.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...

*14. ábra. Hello dataframe hello R-parancsfájl végrehajtása modul összegzését.*

Ezek az adatok hello várt típusok és formátumban van. Vegye figyelembe, hogy hello "Month" oszlop típusa tényező, és rendelkezik hello várt szintek száma.

#### <a name="creating-a-time-series-object"></a>Idő adatsorozat objektum létrehozása
Egy alkalommal adatsorozat objektum tooour dataframe igazolnia kell tooadd. Hello aktuális kód cseréje hello következő, az osztály POSIXct egy új oszlopot ad hozzá.

    # Comment hello following if using RStudio
    cadairydata <- maml.mapInputPort(1)

    ## Create a new column as a POSIXct object
    Sys.setenv(TZ = "PST8PDT")
    cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

    str(cadairydata) # Check hello results

Most a naplóban hello. 15. ábra kell hasonlítania.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Time             : POSIXct, format: "1995-01-01" "1995-02-01" ...

*15. ábra. Hello dataframe idő adatsorozat objektum összegzését.*

Az összefoglaló hello láthatja, hogy hello új oszlop valójában osztály POSIXct.

### <a name="exploring-and-transforming-hello-data"></a>Fel és hello adatok átalakítása
Most felfedezheti hello változók DataSet adatkészletben. Egy scatterplot mátrixban egy jó módszer tooproduce gyorsan át. Vagyok helyettesíteni a hello `str()` függvényt a következő sor hello hello előző R kódot.

    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata, main = "Pairwise Scatterplots of dairy time series")

Futtassa a következő kódot, és tekintse meg, mi történik. hello R eszköz port bemutatni hello rajzot 16. ábra hasonlóan kell kinéznie.

![A kijelölt változók Scatterplot mátrix][17]

*16. ábra. A kijelölt változók mátrix Scatterplot.*

Van néhány odd-looking struktúra hello kapcsolatok változókhoz között. Lehet, hogy ez akkor merül fel hello adatok a trendek és hello tényt, hogy azt nem rendelkezik szabványosított hello változók.

### <a name="correlation-analysis"></a>Korrelációs elemzés
tooperform korrelációs elemzési tooboth kell deszerializálni trend, és szabványosítására hello változók. Sikerült egyszerűen használjuk hello R `scale()` függvénynek, amely az adatközpontok, mind az méretezi a változókat. Ez a funkció is előfordulhat, hogy gyorsabban futnak. Azonban tooshow kívánt, az r defenzív programing példa

Hello `ts.detrend()` függvény alább látható mindkét ezeket a műveleteket hajt végre. hello következő két sornyi kód deszerializálni trend hello adatok és majd szabványosítására hello értékeket.

    ts.detrend <- function(ts, Time, min.length = 3){
      ## Function toode-trend and standardize a time series

      ## Define some messages if they are NULL  
      messages <- c('ERROR: ts.detrend requires arguments ts and Time toohave hello same length',
                    'ERROR: ts.detrend requires argument ts toobe of type numeric',
                    paste('WARNING: ts.detrend has encountered a time series with length less than', as.character(min.length)),
                    'ERROR: ts.detrend has encountered a Time argument not of class POSIXct',
                    'ERROR: Detrend regression has failed in ts.detrend',
                    'ERROR: Exception occurred in ts.detrend while standardizing time series in function ts.detrend'
      )
      # Create a vector of zeros tooreturn as a default in some cases
      zerovec  <- rep(length(ts), 0.0)

      # hello input arguments are not of hello same length, return ts and quit
      if(length(Time) != length(ts)) {warning(messages[1]); return(ts)}

      # If hello ts is not numeric, just return a zero vector and quit
      if(!is.numeric(ts)) {warning(messages[2]); return(zerovec)}

      # If hello ts is too short, just return it and quit
      if((ts.length <- length(ts)) < min.length) {warning(messages[3]); return(ts)}

      ## Check that hello Time variable is of class POSIXct
      if(class(cadairydata$Time)[[1]] != "POSIXct") {warning(messages[4]); return(ts)}

      ## De-trend hello time series by using a linear model
      ts.frame  <- data.frame(ts = ts, Time = Time)
      tryCatch({ts <- ts - fitted(lm(ts ~ Time, data = ts.frame))},
               error = function(e){warning(messages[5]); zerovec})

      tryCatch( {stdev <- sqrt(sum((ts - mean(ts))^2))/(ts.length - 1)
                 ts <- ts/stdev},
                error = function(e){warning(messages[6]); zerovec})

      ts
    }  
    ## Apply hello detrend.ts function toohello variables of interest
    df.detrend <- data.frame(lapply(cadairydata[, 4:7], ts.detrend, cadairydata$Time))

    ## Plot hello results toolook at hello relationships
    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = df.detrend, main = "Pairwise Scatterplots of detrended standardized time series")

Nincs elég egy bit azonban a hello `ts.detrend()` függvény. Ez a kód a legtöbb hello argumentumok kapcsolatos esetleges problémák keresése vagy kivételek, továbbra is hello számítások során felmerülő foglalkoznak. Ez a kód csak néhány sornyi ténylegesen hello számításokat.

Rendelkezik már tárgyalt defenzív programozás a példát [átalakítások érték](#valuetransformations). Mindkét számítási blokkok csomagolni vannak `tryCatch()`. A hibák logika tooreturn hello eredeti bemeneti vektoros lehetővé teszi, és más esetekben nullák vektor visszatérés.  

Vegye figyelembe, hogy hello deszerializálni trendek használt lineáris regresszió egy idő adatsorozat regressziós. hello előrejelzőjének változó egy idő adatsorozat objektum.  

Egyszer `ts.detrend()` definiált érvénybe lépni, a dataframe érdeklődik toohello változókat. Igazolnia kell kényszerítési hello listájában által létrehozott `lapply()` toodata dataframe használatával `as.data.frame()`. Defenzív aspektusainak miatt `ts.detrend()`, egy hello változók nem akadályozza meg a hiba tooprocess javítsa hello feldolgozása mások számára.  

hello végső kódsort páros scatterplot hoz létre. Miután hello R-kód, hello scatterplot hello eredményei láthatók 17. ábra.

![Vonja trendszerű és szabványos idősorozat páros scatterplot][18]

*17. ábra. Páros scatterplot deszerializálni trendszerű és szabványos idősorozat.*

Ezek a 16 megjelenített eredmények toothose összehasonlíthatja. Hello trend eltávolítva és szabványosított, változók hello sokkal kevesebb szerkezetben vannak ezek a változók hello kapcsolatai látható.

hello kód toocompute hello korrelációk R FTB objektumként a következőképpen történik.

    ## A function toocompute pairwise correlations from a
    ## list of time series value vectors
    pair.cor <- function(pair.ind, ts.list, lag.max = 1, plot = FALSE){
      ccf(ts.list[[pair.ind[1]]], ts.list[[pair.ind[2]]], lag.max = lag.max, plot = plot)
    }

    ## A list of hello pairwise indices
    corpairs <- list(c(1,2), c(1,3), c(1,4), c(2,3), c(2,4), c(3,4))

    ## Compute hello list of ccf objects
    cadairycorrelations <- lapply(corpairs, pair.cor, df.detrend)  

    cadairycorrelations

Ezt a kódot futtató hoz létre a 18 látható hello napló.

    [ModuleOutput] Loading objects:
    [ModuleOutput]   port1
    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] [[1]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]    -1     0     1 
    [ModuleOutput] 0.148 0.358 0.317 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[2]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.395 -0.186 -0.238 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[3]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.059 -0.089 -0.127 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[4]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]    -1     0     1 
    [ModuleOutput] 0.140 0.294 0.293 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[5]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.002 -0.074 -0.124 

*18. ábra. FTB listája hello páros korrelációs elemzés a következő helyről objektumokat.*

Minden egyes lag korrelációs értéke van. A következő korrelációs értékek nincs elég nagy toobe jelentős. Azt ezért be, hogy azt az egyes minta egymástól függetlenül is.

### <a name="output-a-dataframe"></a>Egy dataframe kimeneti
Az R FTB objektumok listájának azt rendelkezik számított hello páros korrelációk. Ez megadja bit típust egy probléma hello eredmény adathalmaz kimeneti portjával valóban egy dataframe van szüksége. További hello FTB objektum pedig egy lista, és szeretnénk csak hello értékek hello első eleme ebben a listában, a hello hello korrelációk különböző késedelmes jelentések.

hello követő kód kivonatok hello lag értékek FTB objektumok, amelyek maguk is listák hello listája.

    df.correlations <- data.frame(do.call(rbind, lapply(cadairycorrelations, '[[', 1)))

    c.names <- c("correlation pair", "-1 lag", "0 lag", "+1 lag")
    r.names  <- c("Corr Cot Cheese - Ice Cream",
                  "Corr Cot Cheese - Milk Prod",
                  "Corr Cot Cheese - Fat Price",
                  "Corr Ice Cream - Mik Prod",
                  "Corr Ice Cream - Fat Price",
                  "Corr Milk Prod - Fat Price")

    ## Build a dataframe with hello row names column and the
    ## correlation data frame and assign hello column names
    outframe <- cbind(r.names, df.correlations)
    colnames(outframe) <- c.names
    outframe


    ## WARNING!
    ## hello following line works only in Azure Machine Learning
    ## When running in RStudio, this code will result in an error
    #maml.mapOutputPort('outframe')

hello első sor a kód egy kicsit legbonyolultabb, és a rövid segítenek megismerni. Hello ki belül dolgozó tudunk hello következő:

1. hello "**[[**"hello argumentummal operator"**1**" választja ki a hello késedelmes jelentések hello hello FTB objektum-lista első eleme a korrelációk vektorát hello.
2. Hello `do.call()` függvény vonatkozik hello `rbind()` keresztül hello lista elemeinek hello függvény által `lapply()`.
3. Hello `data.frame()` függvény típusú értékké konvertál által előállított hello eredmény `do.call()` tooa dataframe.

Vegye figyelembe, hogy hello sor nevek hello dataframe oszlopában. Így megőrzi hello sor neveket, ha azok hello kimenete [R-parancsfájl végrehajtása][execute-r-script].

Hello kód futtatásával hozza létre a 19. ábra hello kimenettel amikor I **Visualize** hello kimeneti: hello eredmény Dataset port. hello sor nevek rendeltetésszerű hello első oszlopban szerepelnek.

![Hello korrelációs elemzési eredmények kimenete][20]

*19. ábra. Az eredmények hello korrelációs elemzés kimenetét.*

## <a id="seasonalforecasting"></a>Idő adatsorozat példa: határozza előrejelzés
Az adatok mostantól elemzés alkalmas űrlapon, és nincsenek hello változók között nincs jelentős korrelációk azt észlelte. Most lépés, és hozzon létre egy idősorozat-modell előrejelzési. A modell segítségével azt fogja előrejelzési kaliforniai tejtermelésre hello 2013 számított 12 hónapon.

Az előrejelzési modell két összetevőt, a trend összetevőt és határozza lesz. hello teljes előrejelzés a két összetevő hello szorzatát. Az ilyen típusú modell tényezőt modell néven ismert. hello alternatíva a következő: egy additívak modellt. A napló átalakítása toohello változók iránt, így tractable az elemzés azt már telepítették.

Ez a szakasz hello teljes R kód hello korábban letöltött zip-fájl van.

### <a name="creating-hello-dataframe-for-analysis"></a>Hello dataframe elemzés létrehozása
Először vegyen fel egy **új** [R-parancsfájl végrehajtása] [ execute-r-script] modul tooyour kísérlet. Csatlakozás hello **eredmény Dataset** hello meglévő kimeneti [R-parancsfájl végrehajtása] [ execute-r-script] modul toohello **Dataset1** hello új modul bemeneti. hello eredmény hasonlóan kell kinéznie. ábra 20.

![hello kísérletezhet hello hozzáadott új R-parancsfájl végrehajtása modul][21]

*20. ábra. hello hello hozzáadott új R-parancsfájl végrehajtása modul kísérletezhet.*

Mint hello korrelációs elemzése azt imént befejeződött, a szükséges tooadd adatsorozat objektum POSIXct idő oszlop. a következő kód hello fog ehhez csak.

    # If running in Machine Learning Studio, uncomment hello first line with maml.mapInputPort()
    cadairydata <- maml.mapInputPort(1)

    ## Create a new column as a POSIXct object
    Sys.setenv(TZ = "PST8PDT")
    cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

    str(cadairydata)

Futtassa a következő kódot, és nézze meg hello naplót. hello eredmény 21. ábrát hasonlóan kell kinéznie.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Time             : POSIXct, format: "1995-01-01" "1995-02-01" ...

*21. ábra. Hello dataframe összegzését.*

Az eredményt, vagy folyamatban, készen áll a toostart az elemzés.

### <a name="create-a-training-dataset"></a>Hozzon létre egy képzési adatkészlet
Az összeállított hello dataframe kell toocreate képzési dataset. Ezeket az adatokat fog tartalmaznak minden hello megfigyelések kivételével hello utolsó 12 hello év 2013, amelyek a tesztelési adatkészletnél. hello következő alkészletek hello dataframe code, és létrehozza a hello tejelő éles üzemi pontjának és ár változók előkészítésére. I majd függvényében hello négy éles üzemi pontjának és ár változókat létrehozni. Egy névtelen funkció használt toodefine néhány szabályozva az információhasználat a rajzolási, és majd ismétlés hello hello listája más két argumentumot `Map()`. Ha vannak-e végezni, amely egy a hurok kellene rendelkeznie működött itt, hogy helyesek. Azonban mivel R I vagyok jeleníti meg a működési megközelítés működési nyelvet.

    cadairytrain <- cadairydata[1:216, ]

    Ylabs  <- list("Log CA Cotage Cheese Production, 1000s lb",
                   "Log CA Ice Cream Production, 1000s lb",
                   "Log CA Milk Production 1000s lb",
                   "Log North CA Milk Milk Fat Price per 1000 lb")

    Map(function(y, Ylabs){plot(cadairytrain$Time, y, xlab = "Time", ylab = Ylabs, type = "l")}, cadairytrain[, 4:7], Ylabs)

A time series sorozatát tevékenységtérkép 22. ábra látható hello R eszköz kimenetből hello hello kód futtatásával hozza létre. Vegye figyelembe, hogy hello időtengelye egységekbe, dátumok, adatsorozat megrajzolásához metódus hello időt töltött előnyeit.

![Első idő adatsorozat előkészítésére kaliforniai tejelő éles üzemi pontjának és ár adatok](./media/machine-learning-r-quickstart/unnamed-chunk-161.png)

![Második idő adatsorozat előkészítésére kaliforniai tejelő éles üzemi pontjának és ár adatok](./media/machine-learning-r-quickstart/unnamed-chunk-162.png)

![Az idő adatsorozat előkészítésére kaliforniai tejelő éles üzemi pontjának és ár adatok külső](./media/machine-learning-r-quickstart/unnamed-chunk-163.png)

![Az idő adatsorozat előkészítésére kaliforniai tejelő éles üzemi pontjának és ár adatok negyedik](./media/machine-learning-r-quickstart/unnamed-chunk-164.png)

*22. ábra. Idő adatsorozat előkészítésére kaliforniai tejtermelésre és adatokat.*

### <a name="a-trend-model"></a>A trend modell
Egy idő adatsorozat objektum, és hogy megtekintette hello adatok létrehozása tooconstruct hello kaliforniai tej termelési adatok trend modellt kezdjük. Azt a idő adatsorozat regressziós végezheti el. Azonban származik egyértelműen hello rajzot, hogy azt fogja kell több mint egy görbét és intercept tooaccurately modell hello betanítási adatok trendet megfigyelt hello.

Megadott hello kis léptékű hello adatok, I elkészíti az Rstudióból trend hello modell és majd kivágása és illessze be hello eredményül kapott modell Azure Machine Learning. Az ilyen típusú interaktív elemzések elvégzéséhez interaktív környezetet biztosít az Rstudióból.

Első kísérlet, mint a polinom regressziós rendelkező too3 fel fog meg. Nincs túlzott illeszkedő modellek az ilyen típusú valós veszélye. Ezért akkor ajánlott tooavoid magas rendelés feltételeit. Hello `I()` hello tartalmának értelmezése funkció meggátolja a rövid nevének (értelmezi hello tartalma ","), és lehetővé teszi a egy regressziós egyenlet toowrite szó értelmezett függvényt.

    milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^2) + I(Month.Count^3), data = cadairytrain)
    summary(milk.lm)

Ezt követően hello következő.

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^2) + I(Month.Count^3),
    ##     data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.12667 -0.02730  0.00236  0.02943  0.10586
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)       6.33e+00   1.45e-01   43.60   <2e-16 ***
    ## Time              1.63e-09   1.72e-10    9.47   <2e-16 ***
    ## I(Month.Count^2) -1.71e-06   4.89e-06   -0.35    0.726
    ## I(Month.Count^3) -3.24e-08   1.49e-08   -2.17    0.031 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0418 on 212 degrees of freedom
    ## Multiple R-squared:  0.941,    Adjusted R-squared:  0.94
    ## F-statistic: 1.12e+03 on 3 and 212 DF,  p-value: <2e-16

P értékek (Pr (> |} t |})) szöveget a kimenetben, láthatja, hogy hello squared kifejezés nem mindig jelentős. Hello használom `update()` Ez a modell által figyelmen kívül hagyása hello négyzet kifejezés toomodify működik.

    milk.lm <- update(milk.lm, . ~ . - I(Month.Count^2))
    summary(milk.lm)

Ezt követően hello következő.

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^3), data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.12597 -0.02659  0.00185  0.02963  0.10696
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)       6.38e+00   4.07e-02   156.6   <2e-16 ***
    ## Time              1.57e-09   4.32e-11    36.3   <2e-16 ***
    ## I(Month.Count^3) -3.76e-08   2.50e-09   -15.1   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0417 on 213 degrees of freedom
    ## Multiple R-squared:  0.941,  Adjusted R-squared:  0.94
    ## F-statistic: 1.69e+03 on 2 and 213 DF,  p-value: <2e-16

Ez a következőhöz jobb. Hello feltételek összes jelentős. Azonban a hello 2e-16 érték az alapértelmezett érték, és nem kell venni súlyosan túl.  

Megerősítést tesztként most Meggyőződünk hello kaliforniai tejtermelésre adatok ábrázolása idő adatsorozat hello trend görbéjű látható. A következő kódot az Azure Machine Learning hello hello hozzáadott [R-parancsfájl végrehajtása] [ execute-r-script] modell (nem Rstudióból) toocreate hello modell, és tegye rajzot. 23. ábra hello eredmény jelenik meg.

    milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^3), data = cadairytrain)

    plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
    lines(cadairytrain$Time, predict(milk.lm, cadairytrain), lty = 2, col = 2)

![Kaliforniai tej termelési adatok trend modellel látható](./media/machine-learning-r-quickstart/unnamed-chunk-18.png)

*23. ábra. Kaliforniai tej termelési adatok trend modellel látható.*

Úgy tűnik, hello trend modell hello adatok viszonylag jól illeszkedik. További úgy nem tűnik toobe igazolása túlzott méretezés, például a következő hello modell görbe wiggles páratlan.  

### <a name="seasonal-model"></a>Határozza modell
Az aktuális trend modell azt kell toopush és hello határozza hatások közé tartozik. Hello év hónapját hello hello lineáris modell toocapture hello havonta készülő érvényben dummy változóként használjuk. Vegye figyelembe, hogy amikor egy modell tényező változók bevezetéséhez, hello intercept nem kell számolni. Ha nem ezt teszi, hello képlet túlzott megadott és R eldobja hello egyik tényező szükséges, de tartsa hello intercept kifejezés.

Mivel kielégítő trend modell tudunk hello használhatjuk `update()` függvény tooadd hello új toohello meglévő modell feltételeket. -1 hello hello frissítés képletben hello intercept kifejezés esik. Folytatás az Rstudióból hello pillanatra:

    milk.lm2 <- update(milk.lm, . ~ . + Month - 1)
    summary(milk.lm2)

Ezt követően hello következő.

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^3) + Month - 1,
    ##     data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.06879 -0.01693  0.00346  0.01543  0.08726
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## Time              1.57e-09   2.72e-11    57.7   <2e-16 ***
    ## I(Month.Count^3) -3.74e-08   1.57e-09   -23.8   <2e-16 ***
    ## MonthApr          6.40e+00   2.63e-02   243.3   <2e-16 ***
    ## MonthAug          6.38e+00   2.63e-02   242.2   <2e-16 ***
    ## MonthDec          6.38e+00   2.64e-02   241.9   <2e-16 ***
    ## MonthFeb          6.31e+00   2.63e-02   240.1   <2e-16 ***
    ## MonthJan          6.39e+00   2.63e-02   243.1   <2e-16 ***
    ## MonthJul          6.39e+00   2.63e-02   242.6   <2e-16 ***
    ## MonthJun          6.38e+00   2.63e-02   242.4   <2e-16 ***
    ## MonthMar          6.42e+00   2.63e-02   244.2   <2e-16 ***
    ## MonthMay          6.43e+00   2.63e-02   244.3   <2e-16 ***
    ## MonthNov          6.34e+00   2.63e-02   240.6   <2e-16 ***
    ## MonthOct          6.37e+00   2.63e-02   241.8   <2e-16 ***
    ## MonthSep          6.34e+00   2.63e-02   240.6   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0263 on 202 degrees of freedom
    ## Multiple R-squared:     1,    Adjusted R-squared:     1
    ## F-statistic: 1.42e+06 on 14 and 202 DF,  p-value: <2e-16

Adott hello modell nem tartalmaz egy intercept kifejezés és 12 hónap jelentős tényező látható. Ez az pontosan mit akartunk toosee.

Ellenőrizze egy másik idő adatsorozat ábrázolása hello kaliforniai tejtermelésre adatok toosee hello határozza modell mennyire működik. A következő kódot az Azure Machine Learning hello hello hozzáadott [R-parancsfájl végrehajtása] [ execute-r-script] toocreate hello modell, és tegye rajzot.

    milk.lm2 <- lm(Milk.Prod ~ Time + I(Month.Count^3) + Month - 1, data = cadairytrain)

    plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
    lines(cadairytrain$Time, predict(milk.lm2, cadairytrain), lty = 2, col = 2)

Ez a kód futtatása az Azure Machine Learning hoz létre a hello rajzot 24. ábrán látható.

![Kaliforniai tejtermelés határozza hatások beleértve modell](./media/machine-learning-r-quickstart/unnamed-chunk-20.png)

*24. ábra. Kaliforniai tejtermelés határozza hatások beleértve modellt.*

hello illeszkedő toohello 24. ábrán látható adata ahelyett, hogy ezzel. Hello trendek és a hello határozza hatás (havi változat) ésszerű jelenik meg

A modell ellenőrzése most rendelkeznie hello például egy pillantást. hello következő kód kiszámítja a két modell az előre jelzett értékek hello hello például hello határozza modell kiszámítja és majd tevékenységtérkép ezek például hello betanítási adatok.

    ## Compute predictions from our models
    predict1  <- predict(milk.lm, cadairydata)
    predict2  <- predict(milk.lm2, cadairydata)

    ## Compute and plot hello residuals
    residuals <- cadairydata$Milk.Prod - predict2
    plot(cadairytrain$Time, residuals[1:216], xlab = "Time", ylab ="Residuals of Seasonal Model")

hello maradék rajzot 25. ábra jelenik meg.

![Például hello határozza modell hello betanítási adatok](./media/machine-learning-r-quickstart/unnamed-chunk-21.png)

*25. ábra. Például hello határozza modell hello betanítási adatok.*

Ezeket például hely ésszerű. Nem adott struktúra, kivéve hello 2008-2009 recesszió, amely a modell nem derül ki hello hatása van különösen jól.

25. ábrán látható hello rajzot bármely időfüggő minták észlelésére hello például a. hello explicit módszert is, amely használt hello például számítási és a hello rajzot időrendjét hello például helyezi. Ha a hello, ugyanakkor I kellett ábrázolt `milk.lm$residuals`, hello rajzot nem volna időrendjét.

Is `plot.lm()` tooproduce diagnosztikai előkészítésére sorozata.

    ## Show hello diagnostic plots for hello model
    plot(milk.lm2, ask = FALSE)

Ez a kód 26. ábrán látható diagnosztikai előkészítésére sorozata eredményez.

![Diagnosztikai kialakítása hello határozza modell az első](./media/machine-learning-r-quickstart/unnamed-chunk-221.png)

![Diagnosztikai előkészítésére hello határozza modell második](./media/machine-learning-r-quickstart/unnamed-chunk-222.png)

![A diagnosztikai előkészítésére hello határozza modell harmadik](./media/machine-learning-r-quickstart/unnamed-chunk-223.png)

![A diagnosztikai előkészítésére hello határozza modell negyedik](./media/machine-learning-r-quickstart/unnamed-chunk-224.png)

*26. ábra. Diagnosztikai tevékenységtérkép hello határozza modell.*

Ezek előkészítésére, de semmi nem azonosított néhány magas segíti elő pontjai toocause nagy gondot okoz. További láthatja a hello normál Q-Q rajzot, hogy hello például-e a Bezárás toonormally elosztott, egy fontos feltételezésen lineáris modellek esetén.

### <a name="forecasting-and-model-evaluation"></a>Előrejelzési és modell kiértékelése
Nincs elegendő egyetlen további lépésként toodo toocomplete példában. Azt toocompute előrejelzések kell, és mérheti hello hiba hello tényleges adatokkal szemben. Az előrejelzés lesznek hello 2013 számított 12 hónapon. Hiba az előrejelzési toohello tényleges adatokat, amelyek nem része az képzési adatkészletet intézkedésként számíthatja azt. Emellett azt is vetik össze a hello a betanítási adatok toohello 18 évnyi vizsgálati adatok 12 hónapon keresztül.  

Metrikák számos használt toomeasure hello teljesítményét az idő adatsorozat modellek. A mi esetünkben használjuk hello négyzetes (RMS)-hiba. hello következő függvény kiszámítja hello RMS hiba két sorozat között.  

    RMS.error <- function(series1, series2, is.log = TRUE, min.length = 2){
      ## Function toocompute hello RMS error or difference between two
      ## series or vectors

      messages <- c("ERROR: Input arguments toofunction RMS.error of wrong type encountered",
                    "ERROR: Input vector toofunction RMS.error is too short",
                    "ERROR: Input vectors toofunction RMS.error must be of same length",
                    "WARNING: Funtion rms.error has received invald input time series.")

      ## Check hello arguments
      if(!is.numeric(series1) | !is.numeric(series2) | !is.logical(is.log) | !is.numeric(min.length)) {
        warning(messages[1])
        return(NA)}

      if(length(series1) < min.length) {
        warning(messages[2])
        return(NA)}

      if((length(series1) != length(series2))) {
           warning(messages[3])
        return(NA)}

      ## If is.log is TRUE exponentiate hello values, else just copy
      if(is.log) {
        tryCatch( {
          temp1 <- exp(series1)
          temp2 <- exp(series2) },
          error = function(e){warning(messages[4]); NA}
        )
      } else {
        temp1 <- series1
        temp2 <- series2
      }

     ## Compute predictions from our models
    predict1  <- predict(milk.lm, cadairydata)
    predict2  <- predict(milk.lm2, cadairydata)

    ## Compute hello RMS error in a dataframe
      tryCatch( {
        sqrt(sum((temp1 - temp2)^2) / length(temp1))},
        error = function(e){warning(messages[4]); NA})
    }

A hello `log.transform()` a tárgyalt funkció hello "Érték átalakítások" szakaszban, nincs elég nagy mennyiségű ellenőrzése és a kivétel helyreállítási hibakód ebben a függvényben. hello alapelvek alkalmazott vannak hello azonos. hello munkát csomagolni két helyen `tryCatch()`. Első lépésként hello idő adatsorokat exponentiated, mivel azt dolgozott hello naplók hello értékek. Második számított hello tényleges RMS hiba.  

Most egy függvény toomeasure hello RMS hiba felszerelve, hozza létre és egy dataframe hello RMS a hibákat tartalmazó kimeneti. Hello trend modell önmagában feltételek és a teljes modell hello határozza tényezők is. hello alábbira hello feladat használatával hello két lineáris modellek igazolnia kell kialakítani.

    ## Compute hello RMS error in a dataframe
    ## Include hello row names in hello first column so they will
    ## appear in hello output of hello Execute R Script
    RMS.df  <-  data.frame(
    rowNames = c("Trend Model", "Seasonal Model"),
      Traing = c(
      RMS.error(predict1[1:216], cadairydata$Milk.Prod[1:216]),
      RMS.error(predict2[1:216], cadairydata$Milk.Prod[1:216])),
      Forecast = c(
        RMS.error(predict1[217:228], cadairydata$Milk.Prod[217:228]),
        RMS.error(predict2[217:228], cadairydata$Milk.Prod[217:228]))
    )
    RMS.df

    ## hello following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('RMS.df')

Ezt a kódot futtató hoz létre a hello kimeneti hello eredmény adathalmaz kimeneti portjával 27. ábrán látható.

![RMS hibák hello modellek összehasonlítása][26]

*27. ábra. RMS hibák hello modellek összehasonlítása.*

Ezekkel az eredményekkel, az látható, hogy hello határozza tényezők toohello hozzáadása modell hello RMS hiba lényegesen csökkenti. Nem túl érdekes hello RMS hiba a betanítási adatok hello bites kisebb, mint hello előrejelzési.

## <a id="appendixa"></a>FÜGGELÉK: Útmutató tooRStudio
Rstudióból meglehetősen megfelelően legyen dokumentálva, így a függelék I fogja adni az egyes hivatkozások toohello hello Rstudióból dokumentáció tooget használatba kulcsfontosságú részeit.

1. Projektek létrehozása
   
   Rendezheti és az R-kód a projektek Rstudióból használatával kezelni. https://support.rstudio.com/hc/articles/200526207-Using-Projects projektek használó hello dokumentációjában találhatók.
   
   Kövesse az alábbi utasításokat, és hello R kódpéldák projekt létrehozása ebben a dokumentumban javasolt.  
2. Szerkesztés és az R-kód végrehajtása
   
   A szerkesztési és R-kód végrehajtása integrált környezetet biztosít az Rstudióból. Https://support.rstudio.com/hc/articles/200484448-Editing-and-Executing-Code dokumentációjában találhatók.
3. Hibakeresés
   
   Rstudióból hatékony hibakeresési képességeket tartalmazza. Ezek a szolgáltatások dokumentációja https://support.rstudio.com/hc/articles/200713843-Debugging-with-RStudio jelenleg.
   
   hello töréspont hibaelhárítási lehetőségek: https://support.rstudio.com/hc/articles/200534337-Breakpoint-Troubleshooting dokumentációja.

## <a id="appendixb"></a>B függelék: További információ
Ez magában foglalja az oktatóanyag hello alapjai programozási R, hogy mit kell toouse hello Azure Machine Learning Studio R nyelven. Ha nem ismeri a R, a CRAN két bevezetés érhetők el:

* R Emmanuel Paradis által kezdőknek egy remek toostart: http://cran.r-project.org/doc/contrib/Paradis-rdebuts_en.pdf.  
* Egy bevezető bemutató w-n Venables et. al. egy kicsit nagyobb mélysége, http://cran.r-project.org/doc/manuals/R-intro.html állapotba kerül.

Nincs olyan sok könyvek R, amelyik segíthet a kezdéshez. Íme néhány hasznos található:

* Kép az R programozási hello: A bemutató a statisztikai szoftver kialakítása Norman Matloff által esetén egy kiváló bemutatása tooprogramming az R.  
* R Cookbook által Paul Teetor nyújt egy probléma és a megoldás megközelítés toousing R.  
* A művelet által Robert Kabacoff R egy másik hasznos bevezető címjegyzék-alkalmazásával. hello kiegészítő gyors R webhely http://www.statmethods.net/ egy hasznos erőforrás.
* R Inferno Patrick égési által, amely foglalkozik a legbonyolultabb és nehezen méretezhetővé foglalkozó témakörök, amelyek is észlelt, amikor R. hello könyv programozási számos érdekes vicces könyv szabad http://www.burns-stat.com/documents/books/the-r-inferno/ érhető el.
* A részletes bemutatója a speciális témakörei R, van szükség egy pillantást hello könyv speciális R Hadley Wickham által. hello online változata annak könyvben megtalálható szabad http://adv-r.had.co.nz/.

A katalógus R idő adatsorozat csomagok hello CRAN feladat megtekintése az adatsorozat elemzés található: http://cran.r-project.org/web/views/TimeSeries.html. Adott időpont adatsorozat objektum csomagok olvashat olvassa el, hogy a csomag toohello dokumentációját.

hello könyv bevezető Time Series Paul Cowpertwait és Andrew Metcalfe r biztosít egy bevezető toousing R idő adatsorozat elemzés céljából. Számos további elméleti szövegek R példákat.

Néhány nagy internetes erőforrásokhoz:

* DataCamp: DataCamp útmutatást ad az R a videó megszerzett és kódolási gyakorlatok a böngésző hello kényelmét. Nincsenek interaktív oktatóanyagok hello legújabb R technikák és csomagok. Hello szabad interaktív R oktatóanyag veszik a https://www.datacamp.com/courses/introduction-to-r
* Útmutató, kezdeti lépések a R Programiz https://www.programiz.com/r-programming a
* A gyors R oktatóprogram által Tibor fekete Clarkson egyetemi http://www.cyclismo.org/tutorial/R/
* 60 + R erőforrások megtalálható a http://www.computerworld.com/article/2497464/business-intelligence-60-r-resources-to-improve-your-data-skills.html

<!--Image references-->
[1]: ./media/machine-learning-r-quickstart/fig1.png
[2]: ./media/machine-learning-r-quickstart/fig2.png
[3]: ./media/machine-learning-r-quickstart/fig3.png
[4]: ./media/machine-learning-r-quickstart/fig4.png
[5]: ./media/machine-learning-r-quickstart/fig5.png
[6]: ./media/machine-learning-r-quickstart/fig6.png
[7]: ./media/machine-learning-r-quickstart/fig7.png
[8]: ./media/machine-learning-r-quickstart/fig8.png
[9]: ./media/machine-learning-r-quickstart/fig9.png
[10]: ./media/machine-learning-r-quickstart/fig10.png
[11]: ./media/machine-learning-r-quickstart/fig11.png
[12]: ./media/machine-learning-r-quickstart/fig12.png
[13]: ./media/machine-learning-r-quickstart/fig13.png
[14]: ./media/machine-learning-r-quickstart/fig14.png
[15]: ./media/machine-learning-r-quickstart/fig15.png
[16]: ./media/machine-learning-r-quickstart/fig16.png
[17]: ./media/machine-learning-r-quickstart/fig17.png
[18]: ./media/machine-learning-r-quickstart/fig18.png
[19]: ./media/machine-learning-r-quickstart/fig19.png
[20]: ./media/machine-learning-r-quickstart/fig20.png
[21]: ./media/machine-learning-r-quickstart/fig21.png
[22]: ./media/machine-learning-r-quickstart/fig22.png

[26]: ./media/machine-learning-r-quickstart/fig26.png

<!--links-->
[appendixa]: #appendixa
[download]: https://azurebigdatatutorials.blob.core.windows.net/rquickstart/RFiles.zip


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
