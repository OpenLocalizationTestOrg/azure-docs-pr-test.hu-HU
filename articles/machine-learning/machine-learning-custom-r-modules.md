---
title: "Egyéni R modul létrehozása az Azure Machine Learning aaaAuthor |} Microsoft Docs"
description: "Gyors üzembe helyezési egyéni R modul az Azure Machine Learning-szerzésre vonatkozó információ."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 6cbc628a-7e60-42ce-9f90-20aaea7ba630
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 03/24/2017
ms.author: bradsev;ankarlof
ms.openlocfilehash: 8007c2abe20a4ab990f38b6d09bc4e6834ad2082
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="author-custom-r-modules-in-azure-machine-learning"></a>Egyéni R-modul létrehozása az Azure Machine Learningben
Ez a témakör ismerteti, hogyan tooauthor és központi telepítése az Azure Machine Learning egy egyéni R modult. Egyéni R modul vannak, és milyen fájlok használt toodefine ismerteti azokat. Azt mutatja be, hogyan tooconstruct hello fájlok, amelyek meghatározzák egy modult, és hogyan tooregister hello modul a Machine Learning-munkaterület központi telepítéséhez. hello elemek és attribútumok hello definíciójában hello egyéni modult használja majd ismerteti részletesen. Hogyan toouse kiegészítő funkciók és a fájlok és a több kimenet van is ismertet. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="what-is-a-custom-r-module"></a>Mi az, hogy egy egyéni R modult?
A **egyéni modul** egy felhasználó által definiált modul, amely lehet feltöltve tooyour munkaterület és az Azure Machine Learning kísérlet részeként végrehajtani. A **egyéni R modul** egy egyéni modul, amely végrehajtja a felhasználó által definiált R függvény. **R** statisztikai számítások és grafikus statisztikusok és az adatok kutatók által algoritmusok megvalósításának széles körben használt programozási nyelv. R jelenleg hello nyelvi további nyelveket a jövőbeli kiadások van ütemezve. az egyéni modulok, de támogatja a támogatott.

Az egyéni modulok rendelkezik **első osztályú állapot** az Azure Machine Learning hello értelemben, hogy azok csakúgy, mint bármely más modul használható. Az egyéb modulok, közzétett kísérletek vagy a képi megjelenítések végrehajthatók. Hello modul által megvalósított hello algoritmus szabályozhatják, a használt bemeneti és kimeneti portok toobe hello, hello modellezési paramétereket és egyéb különböző futásidejű viselkedések. A kísérlet, amely tartalmazza az egyéni modulok az egyszerű is a Cortana Intelligence Gallery hello tehetők közzé.

## <a name="files-in-a-custom-r-module"></a>Egy egyéni R modul fájlok
Egy egyéni R modul egy .zip fájlt, amely legalább két fájlt tartalmaz határozzák meg:

* A **forrásfájl** , amely megvalósítja hello modul által elérhetővé tett hello R függvény
* Egy **XML-definíciós fájljának** , amely leírja, hogy hello egyéni modul felület

További kiegészítő fájlok hello .zip fájlt, amely elérhető az hello egyéni modult is megtalálhatók. Ez a beállítás hello ismertet **argumentumok** hello útmutató szakaszban része **hello XML-definíciós fájljának elemeinek** hello gyors üzembe helyezési példában a következő.

## <a name="quickstart-example-define-package-and-register-a-custom-r-module"></a>Gyors üzembe helyezési példa: határozza meg, a csomagot, majd egy egyéni R modult regisztrálni
Ez a példa bemutatja, hogyan tooconstruct hello egy egyéni R modult szükséges fájlokat, a csomagolni egy zip-fájl, és a Machine Learning-munkaterület hello modul regisztrálása. hello például zip csomag és a minta-fájlok letölthető [letöltése CustomAddRows.zip fájl](http://go.microsoft.com/fwlink/?LinkID=524916&clcid=0x409).

## <a name="hello-source-file"></a>hello forrásfájl
Vegye figyelembe a hello példa egy **egyéni hozzáadása sorok** modul, amely módosítja a hello szabványos végrehajtásának hello **sorok hozzáadása** modul használt tooconcatenate sorát (megfigyelések) két adatkészletet (adatkeretek). hello standard **sorok hozzáadása** modul hozzáfűzi hello sor végének hello második bemeneti adatkészlet toohello hello első bemeneti adatkészletet hello segítségével `rbind` algoritmus. testre szabott hello `CustomAddRows` függvény hasonlóképpen fogad el két adatkészletet, de egy logikai swap paraméter további bemenetként is fogad. Ha hello swap paraméter értéke túl**hamis**, akkor adja vissza hello azonos adatkészlet, szabványos implementációjától hello. De ha hello swap paraméter **igaz**, hello függvény első bemeneti adatkészlet toohello végét hello második dataset sorát helyette hozzáfűzi. hello CustomAddRows.R hello R hello végrehajtásának tartalmazó fájl `CustomAddRows` hello által elérhetővé tett függvény **egyéni hozzáadása sorok** modul rendelkezik a következő R-kód hello.

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) 
    {
        if (swap)
        {
            return (rbind(dataset2, dataset1));
        }
        else
        {
            return (rbind(dataset1, dataset2));
        } 
    } 

### <a name="hello-xml-definition-file"></a>hello XML-definíciós fájlt
tooexpose ez `CustomAddRows` függvényt egy Azure Machine Learning-modul, egy XML-definíciós fájljának, létre kell hozni toospecify hogyan hello **egyéni hozzáadása sorok** modul kell kinézete és viselkedése. 

    <!-- Defined a module using an R Script -->
    <Module name="Custom Add Rows">
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset tooanother. Dataset 2 is concatenated tooDataset 1 when Swap is FALSE, and vice versa when Swap is TRUE.</Description>

    <!-- Specify hello base language, script file and R function toouse for this module. -->        
        <Language name="R" 
         sourceFile="CustomAddRows.R" 
         entryPoint="CustomAddRows" />  

    <!-- Define module input and output ports -->
    <!-- Note: hello values of hello id attributes in hello Input and Arg elements must match hello parameter names in hello R Function CustomAddRows defined in CustomAddRows.R. -->
        <Ports>
            <Input id="dataset1" name="Dataset 1" type="DataTable">
                <Description>First input dataset</Description>
            </Input>
            <Input id="dataset2" name="Dataset 2" type="DataTable">
                <Description>Second input dataset</Description>
            </Input>
            <Output id="dataset" name="Dataset" type="DataTable">
                <Description>hello combined dataset</Description>
            </Output>
        </Ports>

    <!-- Define module parameters -->
        <Arguments>
            <Arg id="swap" name="Swap" type="bool" >
                <Description>Swap input datasets.</Description>
            </Arg>
        </Arguments>
    </Module>


Kritikus toonote, amely hello értékének hello **azonosító** hello attribútumait **bemeneti** és **Arg** hello XML-fájl elemeinek meg kell egyeznie a hello függvény paramétereinek nevére hello R hello CustomAddRows.R fájlban pontosan kód: (*dataset1*, *dataset2*, és *swap* hello példában). Hasonlóképpen, a hello értékének hello **belépési pont** hello attribútumának **nyelvi** elem pontosan meg kell egyeznie a hello R-parancsfájl hello függvény hello neve: (*CustomAddRows* hello példában). 

Ezzel szemben hello **azonosító** hello attribútuma **kimeneti** elem nem felel meg a hello R-parancsfájl tooany változókat. Ha több kimenet szükség, egyszerűen listáját adja vissza hello R függvényből elhelyezett eredményekkel *hello az ugyanabban a sorrendben* , **kimenetek** hello XML-fájlban deklarált elemeket.

### <a name="package-and-register-hello-module"></a>Csomag és a nyilvántartás hello modul
Mentés másként két fájlt *CustomAddRows.R* és *CustomAddRows.xml* és majd zip hello két fájlt együtt történő egy *CustomAddRows.zip* fájlt.

a Machine Learning munkaterületen, a Machine Learning Studio hello lépjen tooyour munkaterületén kattintson hello tooregister **+ új** hello alján gombra, majd válassza a **modul ZIP-csomag a ->** tooupload új hello **egyéni hozzáadása sorok** modul.

![Zip feltöltése](./media/machine-learning-custom-r-modules/upload-from-zip-package.png)

Hello **egyéni hozzáadása sorok** modul mostantól készen áll a toobe érhetők el a Machine Learning kísérleteket.

## <a name="elements-in-hello-xml-definition-file"></a>XML-definíciós fájl hello elemei
### <a name="module-elements"></a>A modul elemei
Hello **modul** elem használt toodefine egy egyéni modul hello XML-fájlban. Több modul adható meg egy XML-fájl több **modul** elemek. A munkaterület minden modulja egy egyedi névvel kell rendelkeznie. Egy egyéni modult regisztrálni azonos nevet, egy meglévő egyéni modulként hello és hello meglévő modul lecseréli hello újat. Azonban az egyéni modulok lehet hello azonos nevet, egy meglévő Azure Machine Learning-modul regisztrálva. Ha igen, azok megjelennek hello **egyéni** hello modulpalettán kategóriáját.

    <Module name="Custom Add Rows" isDeterministic="false"> 
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset tooanother...</Description>/> 


Hello belül **modul** elem, két további választható elem adhat meg:

* egy **tulajdonos** hello modulba beágyazott elem  
* egy **leírás** elem, amely tartalmazza a szöveg, amely gyors súgó hello modul jelenik meg, és ha mutat a Machine Learning felhasználói felület hello hello modul.

Karakterek korlátok hello modul elemekben szabályokat:

* hello értékének hello **neve** hello attribútum **modul** elem legfeljebb 64 karakternél hosszabb. 
* hello tartalmának hello **leírás** elem legfeljebb 128 karakter hosszúságú.
* hello tartalmának hello **tulajdonos** elem legfeljebb 32 karakter hosszúságú.

Lehet, hogy egy modul eredmények determinisztikus vagy nondeterministic.* * alapértelmezés szerint, minden modul minősülnek toobe determinisztikus. Ez azt jelenti, hogy a megadott bemeneti paraméterek és egy nem változó készletét, hello modul kell visszaadnia hello azonos eredmények eacRAND vagy egy functionh futtatáskor. Ez a viselkedés, Azure Machine Learning Studio csak Újrafuttatja determinisztikus, ha a paraméter jelölésű modulok vagy hello bemeneti adatok változásairól. Gyorsítótárazott hello eredményeket ad vissza a kísérletek sokkal gyorsabb végrehajtását is tartalmaz.

Nincsenek determinált, például VÉL vagy az aktuális dátum vagy idő hello függvény funkciókat. Ha a modul determinált függvényt tartalmaz, megadhatja, hello modult nem determinisztikus által választható beállítás hello **isDeterministic** túl attribútum**hamis**. Ez biztosítja, hogy hello modult akkor fut újra, amikor hello kísérlet fut, akkor is, ha hello modul bemeneti és a paraméterek nem változtak. 

### <a name="language-definition"></a>Nyelv meghatározása
Hello **nyelvi** az XML-definíciós fájljának eleme használt toospecify hello egyéni modul nyelv. R jelenleg csak a támogatott nyelvi hello. hello értékének hello **sourceFile** attribútum hello R-fájl hello függvény toocall hello modul futtatásakor hello nevének kell lennie. Ez a fájl hello zip-csomagját részének kell lennie. hello értékének hello **entryPoint** attribútum meghívott hello függvény hello nevét, és meg kell egyeznie egy érvényes ellátott függvényt hívják hello forrásfájl.

    <Language name="R" sourceFile="CustomAddRows.R" entryPoint="CustomAddRows" />


### <a name="ports"></a>Portok
hello egy egyéni modul bemeneti és kimeneti port meg van határozva a hello gyermekelemei **portok** hello XML-definíciós fájljának szakasza. Ezek az elemek sorrendjét hello hello elrendezés tapasztalt (UX) felhasználók határozza meg. hello első gyermekét **bemeneti** vagy **kimeneti** hello felsorolt **portok** hello XML-fájl eleme lesz hello bal szélső bemeneti portját a Machine Learning UX hello
Adja meg mindegyik, és előfordulhat, hogy a kimeneti portra egy nem kötelező **leírás** hello szöveg látható, ha hello egérmutatót rámutat hello port a Machine Learning felhasználói felület hello megadó gyermekelemet.

**Szabályok portok**:

* Maximális száma **bemeneti és kimeneti portok** minden 8 van.

### <a name="input-elements"></a>Bemeneti elemei
A bemeneti portok lehetővé teszik toopass adatok tooyour R függvény és munkaterületen. Hello **adattípusok** , amely a bemeneti portok a következők támogatottak: 

**A DataTable:** ehhez a típushoz lett átadva tooyour R függvény bemeneti egy data.frame. Tulajdonképpen bármely típusa (például a CSV-fájlok vagy a ARFF fájlok), és hogy Machine Learning által támogatott kompatibilisek-e **DataTable** konvertált tooa data.frame automatikusan vannak. 

        <Input id="dataset1" name="Input 1" type="DataTable" isOptional="false">
            <Description>Input Dataset 1</Description>
           </Input>

Hello **azonosító** társított minden egyes attribútum **DataTable** bemeneti porthoz egyedi értéknek kell tartoznia, és ezt az értéket meg kell egyeznie a megfelelő nevű az R-függvény paramétere.
Nem kötelező **DataTable** nem átadott kísérlet a bemeneti portok hello értéke lehet **NULL** átadott toohello R függvény és választható zip portok figyelmen kívül lesznek hagyva hello bemeneti nincs csatlakoztatva. Hello **isOptional** attribútum nem kötelező megadni mindkét hello **DataTable** és **Zip-** , és meg kell adnia *hamis* alapértelmezés szerint.

**Zip:** egyéni modulok fogad el bemenetként egy zip-fájlt. A bemeneti van csomagolva, a függvény hello R működő könyvtárba

        <Input id="zippedData" name="Zip Input" type="Zip" IsOptional="false">
            <Description>Zip files toobe extracted toohello R working directory.</Description>
           </Input>

Az egyéni R modul hello azonosító egy Zip-porthoz tartozik toomatch paraméter hello R függvény. Ez azért, mert hello zip-fájl automatikusan kibontott toohello R munkakönyvtárát.

**Bemeneti szabályok:**

* hello értékének hello **azonosító** hello attribútumának **bemeneti** elemnek kell lennie egy érvényes R változónevet.
* hello értékének hello **azonosító** hello attribútumának **bemeneti** elem nem lehet hosszabb 64 karakternél.
* hello értékének hello **neve** hello attribútumának **bemeneti** elem nem lehet hosszabb 64 karakternél.
* hello tartalmának hello **leírás** elem nem lehet hosszabb 128 karakternél
* hello értékének hello **típus** hello attribútumának **bemeneti** elemnek kell lennie *Zip-* vagy *DataTable*.
* hello értékének hello **isOptional** hello attribútumának **bemeneti** elem nincs szükség (és *hamis* alapértelmezés szerint ha nincs megadva); de ha van megadva, kelllennie*igaz* vagy *hamis*.

### <a name="output-elements"></a>Kimeneti elemei
**Standard kimeneti portot:** kimeneti portjait csatlakoztatott toohello visszatérési értékek a R függvényből, amelyek ezután felhasználhatók a további modulokat. *A DataTable* hello csak a standard kimeneti port típus jelenleg támogatott. (Támogatása *tanulókkal* és *átalakítja* kapja.) A *DataTable* output típusúként van definiálva:

    <Output id="dataset" name="Dataset" type="DataTable">
        <Description>Combined dataset</Description>
    </Output>

Az egyéni R modul kimenetek, a hello hello értékének **azonosító** attribútumnak nincs bármi toocorrespond hello R-parancsfájl, de ennek egyedinek kell lennie. Egy egy modul kimeneti hello visszatérési érték a hello R függvénynek kell lennie egy *data.frame*. A rendezés toooutput egynél több objektum támogatott adattípusú, hello megfelelő kimeneti portokat kell toobe hello XML-definíciós fájljának megadott és hello objektumok listáját adja vissza a toobe kell. hello kimeneti objektumok toooutput portok bal oldali tooright tükröző hello-listát adott vissza helyet hello objektumok hello ahhoz rendeli.

Például, ha azt szeretné, hogy toomodify hello **egyéni hozzáadása sorok** modul toooutput hello eredeti két adatkészletet, *dataset1* és *dataset2*, továbbá új toohello csatlakoztatva adatkészlet, *adatkészlet*, (sorrendje, a bal oldali tooright,: *adatkészlet*, *dataset1*, *dataset2*), majd adja meg a hello a kimeneti portok hello CustomAddRows.xml fájlban az alábbiak szerint:

    <Ports> 
        <Output id="dataset" name="Dataset Out" type="DataTable"> 
            <Description>New Dataset</Description> 
        </Output> 
        <Output id="dataset1_out" name="Dataset 1 Out" type="DataTable"> 
            <Description>First Dataset</Description> 
        </Output> 
        <Output id="dataset2_out" name="Dataset 2 Out" type="DataTable"> 
            <Description>Second Dataset</Description> 
        </Output> 
        <Input id="dataset1" name="Dataset 1" type="DataTable"> 
            <Description>First Input Table</Description>
        </Input> 
        <Input id="dataset2" name="Dataset 2" type="DataTable"> 
            <Description>Second Input Table</Description> 
        </Input> 
    </Ports> 


És a "CustomAddRows.R" hello megfelelő sorrendben listájaként hello objektumok listáját adja vissza:

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) { 
        if (swap) { dataset <- rbind(dataset2, dataset1)) } 
        else { dataset <- rbind(dataset1, dataset2)) 
        } 
    return (list(dataset, dataset1, dataset2)) 
    } 

**A képi megjelenítés kimeneti:** azt is megadhatja a kimeneti portra típusú *képi megjelenítés*, amely hello R grafikus eszköz és a konzol kimeneti hello kimenet megjelenítése. Ez a port nem hello R függvény kimeneti része, és nem ütközik más hello hello sorrendjének kimeneti port típusok. tooadd a képi megjelenítés port toohello egyéni modulokkal, adja hozzá egy **kimeneti** elem értéke az *képi megjelenítés* a saját **típus** attribútum:

    <Output id="deviceOutput" name="View Port" type="Visualization">
      <Description>View hello R console graphics device output.</Description>
    </Output>

**Kimeneti szabályok:**

* hello értékének hello **azonosító** hello attribútumának **kimeneti** elemnek kell lennie egy érvényes R változónevet.
* hello értékének hello **azonosító** hello attribútumának **kimeneti** elem nem lehet hosszabb 32 karakternél.
* hello értékének hello **neve** hello attribútumának **kimeneti** elem nem lehet hosszabb 64 karakternél.
* hello értékének hello **típus** hello attribútumának **kimeneti** elemnek kell lennie *képi megjelenítés*.

### <a name="arguments"></a>Argumentumok
További adatok átadhatók toohello R függvény keresztül modul paraméterei hello meghatározott **argumentumok** elemet. Hello modul kiválasztásakor hello jobb szélső tulajdonságok ablaktáblájában hello Machine Learning felhasználói felületén ezek a paraméterek jelennek meg. Az argumentumok hello támogatott típusok lehetnek, vagy létrehozhat egy egyéni számbavételi szükség esetén. Hasonló toohello **portok** elemek, **argumentumok** elemek lehet egy nem kötelező **leírás** elem, amely itt jelenik meg, ha hello egérrel hello szöveg keresztül hello paraméter neve.
Egy modul, például a defaultValue, minValue és maxValue választható tulajdonságok adhatók hozzá, az attribútumok tooa tooany argumentum **tulajdonságok** elemet. Érvényes tulajdonságainak hello **tulajdonságok** elem hello argumentum típusa attól függ, és a támogatott hello argumentumtípus hello a következő szakaszban ismertetjük. Hello argumentumokat **isOptional** tulajdonsága túl**"true"** nem igényelnek hello felhasználói tooenter értéket. Ha az érték nem szerepel a toohello argumentum, majd hello argumentum nem toohello belépési pont függvény lett átadva. Argumentumokat hello belépési pont függvény nem kötelező kell toobe hello függvény, explicit módon kezeli pl. hello belépési pont függvény definícióját a NULL alapértelmezett értéket rendelni hozzá. Egy nem kötelező argumentumában csak kényszeríti hello egyéb argumentum megkötések, azaz a min vagy max, ha hello felhasználó által megadott értéket.
Csakúgy, mint a be- és kimenetekkel, rendkívül fontos, hogy rendelkezik-e hello paraméterek hozzájuk rendelt egyedi azonosító érték. A gyors üzembe helyezési példa hello tartozó azonosító/paraméter volt *swap*.

### <a name="arg-element"></a>Arg elem
Egy modul paraméter van definiálva hello segítségével **Arg** hello gyermekeleme **argumentumok** hello XML-definíciós fájljának szakasza. A hello gyermekelemek a hello **portok** területen hello paraméterek rendelési hello **argumentumok** szakasz határozza meg a metódusban hello UX hello elrendezés hello paraméterek jelennek meg felülről lefelé hello azonos rendelés nincsenek meghatározva a felhasználói felület hello hello XML-fájlban. az itt felsorolt paramétereket támogatja a Machine Learning hello típusok. 

**int** – egész szám (32 bites) típusú paramétert.

    <Arg id="intValue1" name="Int Param" type="int">
        <Properties min="0" max="100" default="0" />
        <Description>Integer Parameter</Description>
    </Arg>


* *Választható tulajdonságok*: **min**, **maximális**, **alapértelmezett** és **isOptional**

**kettős** – dupla típusú paraméter.

    <Arg id="doubleValue1" name="Double Param" type="double">
        <Properties min="0.000" max="0.999" default="0.3" />
        <Description>Double Parameter</Description>
    </Arg>


* *Választható tulajdonságok*: **min**, **maximális**, **alapértelmezett** és **isOptional**

**logikai** – a logikai paraméter által a UX jelölőnégyzettel jelölt

    <Arg id="boolValue1" name="Boolean Param" type="bool">
        <Properties default="true" />
        <Description>Boolean Parameter</Description>
    </Arg>



* *Választható tulajdonságok*: **alapértelmezett** -hamis, ha nincs beállítva.

**karakterlánc**: szabványos karakterláncok

    <Arg id="stringValue1" name="My string Param" type="string">
        <Properties isOptional="true" />
        <Description>String Parameter 1</Description>
    </Arg>    

* *Választható tulajdonságok*: **alapértelmezett** és **isOptional**

**ColumnPicker**: egy oszlop kiválasztási paraméter. Ez a típus a hello UX egy Oszlopválasztó kezeli. Hello **tulajdonság** elem, amelyből oszlop van kijelölve, ahol hello port céltípus kell hello port használt ide toospecify hello azonosítóját *DataTable*. hello Oszlopválasztás hello eredményét toohello R függvény van át a kiválasztott hello oszlopnevek tartalmazó karakterláncok listáját. 

        <Arg id="colset" name="Column set" type="ColumnPicker">      
          <Properties portId="datasetIn1" allowedTypes="Numeric" default="NumericAll"/>
          <Description>Column set</Description>
        </Arg>


* *Kötelező tulajdonságokban*: **portId** -megegyezik egy bemeneti elem azonosítója hello típusú *DataTable*.
* *Választható tulajdonságok*:
  
  * **allowedTypes** -szűrők hello oszlop típusokat, amelyek ki tudja választani a. Érvényes értékek a következők: 
    
    * Numerikus
    * Logikai érték
    * Kategorikus
    * Karakterlánc
    * Címke
    * Szolgáltatás
    * Pontszám
    * Összes
  * **alapértelmezett** -hello Oszlopválasztó kiválasztott érvényes alapértelmezett beállításokat tartalmazza: 
    
    * None
    * NumericFeature
    * NumericLabel
    * NumericScore
    * NumericAll
    * BooleanFeature
    * BooleanLabel
    * BooleanScore
    * BooleanAll
    * CategoricalFeature
    * CategoricalLabel
    * CategoricalScore
    * CategoricalAll
    * StringFeature
    * StringLabel
    * StringScore
    * StringAll
    * AllLabel
    * AllFeature
    * AllScore
    * Összes

**Legördülő lista**: egy felhasználó által megadott felsorolt (legördülő) listán. hello legördülő elemek hello belül vannak megadva **tulajdonságok** elem használatával egy **elem** elemet. Hello **azonosító** minden **elem** egyedinek kell lennie, és egy érvényes R változó. hello értékének hello **neve** , egy **elem** hello látható szöveg és a toohello R függvénynek átadott hello érték funkcionál.

    <Arg id="color" name="Color" type="DropDown">
      <Properties default="red">
        <Item id="red" name="Red Value"/>
        <Item id="green" name="Green Value"/>
        <Item id="blue" name="Blue Value"/>
      </Properties>
      <Description>Select a color.</Description>
    </Arg>    

* *Választható tulajdonságok*:
  * **alapértelmezett** – hello hello alapértelmezett tulajdonságot meg kell felelnie az azonosító értéke egy hello értéke **elem** elemek.

### <a name="auxiliary-files"></a>Külső fájlok
Minden olyan fájlt, amely az egyéni modul ZIP-fájlja kerül folyamatos toobe használható végrehajtási idő alatt. A jelen könyvtárstruktúrák megmaradnak. Ez azt jelenti, hogy fájl forrás works hello azonos helyileg, és az Azure Machine Learning végrehajtása. 

> [!NOTE]
> Figyelje meg, hogy a fájlok is kibontott too'src "directory, az összes elérési utat kell" src / "előtag.
> 
> 

Tegyük fel szeretné tooremove hello adatkészletből NAs a sorokat, és is távolítsa el az ismétlődő sorokat előtt ellenőrizze a CustomAddRows, és már leírt egy R függvény, amelyet, amely egy fájlban RemoveDupNARows.R:

    RemoveDupNARows <- function(dataFrame) {
        #Remove Duplicate Rows:
        dataFrame <- unique(dataFrame)
        #Remove Rows with NAs:
        finalDataFrame <- dataFrame[complete.cases(dataFrame),]
        return(finalDataFrame)
    }
Hello kiegészítő fájlt RemoveDupNARows.R hello CustomAddRows függvény is forrás:

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) {
        source("src/RemoveDupNARows.R")
            if (swap) { 
                dataset <- rbind(dataset2, dataset1))
             } else { 
                  dataset <- rbind(dataset1, dataset2)) 
             } 
        dataset <- removeDupNARows(dataset)
        return (dataset)
    }

Ezt követően töltse fel az "CustomAddRows.R", "CustomAddRows.xml" és "RemoveDupNARows.R" egy egyéni R modult tartalmazó zip-fájl.

## <a name="execution-environment"></a>Végrehajtási környezet
hello végrehajtási környezet hello R-parancsfájl által használt verziójával megegyező verzióra r hello hello **R-parancsfájl végrehajtása** modul is használható hello azonos és csomagok alapértelmezett. Hozzáadhat további R csomagok tooyour egyéni modult is belefoglalja ezeket hello egyéni modul zip-csomagját. Csak töltődnek be azokat az R-parancsfájl, mint az R környezetben. 

**Hello végrehajtási környezet korlátai** tartalmazza:

* Nem állandó fájlrendszer: több frissítési kísérletei során hello között nem maradnak meg fájlokat, ha az egyéni modul hello fut ugyanabban a modulban.
* Nincs hálózati hozzáférés

