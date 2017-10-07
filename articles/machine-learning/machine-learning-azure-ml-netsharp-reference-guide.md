---
title: "aaaGuide toohello Net # Neurális hálózatokat nyelv |} Microsoft Docs"
description: "Hello Net # Neurális szintaxisának hálózatok nyelv, hogyan toocreate egy egyéni Neurális hálózat modell használatával Net # Microsoft Azure ml példákkal együtt"
services: machine-learning
documentationcenter: 
author: jeannt
manager: jhubbard
editor: cgronlun
ms.assetid: cfd1454b-47df-4745-b064-ce5f9b3be303
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: jeannt
ms.openlocfilehash: 3493247ecc39ca3a1382510ad520d7017159ff62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="guide-toonet-neural-network-specification-language-for-azure-machine-learning"></a>Az útmutató tooNet # Neurális hálózat nyelv az Azure gépi tanulás
## <a name="overview"></a>Áttekintés
NET #, amely használt toodefine Neurális hálózat architektúrák a Microsoft által kifejlesztett nyelvet. Használhatja a Net # a Microsoft Azure Machine Learning modulok Neurális hálózat.

<!-- This function doesn't currentlyappear in hello MicrosoftML documentation. If it is added in a future update, we can uncomment this text.

, or in hello `rxNeuralNetwork()` function in [MicrosoftML](https://msdn.microsoft.com/microsoft-r/microsoftml/microsoftml). 

-->

Ebből a cikkből megtudhatja, főbb fogalmait és kifejezéseit toodevelop egy egyéni Neurális hálózat szükséges: 

* Neurális hálózat követelményeket, és hogyan toodefine hello elsődleges összetevői
* hello szintaxisát és kulcsszavát hello Net # nyelvi specifikáció
* Net # használatával létrehozott egyéni Neurális hálózatokat példák 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="neural-network-basics"></a>Neurális hálózat alapjai
Neurális hálózat struktúra áll ***csomópontok*** , amely vannak rendszerezve ***rétegek***, és súlyozott ***kapcsolatok*** (vagy ***szélek***) között hello csomópontok. hello kapcsolatok irányt, és mindegyik kapcsolat egy ***forrás*** csomópont és a ***cél*** csomópont.  

Minden egyes ***trainable réteg*** (egy rejtett vagy egy kimeneti réteg) rendelkezik egy vagy több ***kapcsolat kötegek***. Egy kapcsolat köteg áll forrás réteg és a forrás réteg hello kapcsolódásának leírását. Egy adott csomag megosztáson található összes hello kapcsolat hello azonos ***forrás réteg*** és azonos hello ***célfájl layer***. Net # a kapcsolat nyalábot tartozó toohello köteg célfájl layer tekintendő.  

NET # számos használatát támogatja kapcsolat kötegek, amely lehetővé teszi testre szabhatja a hello módon bemenetek csatlakoztatott toohidden rétegek, és kiírja a csatlakoztatott toohello.   

hello alapértelmezett vagy a standard csomagot egy **teljes csomag**, melyik minden egyes csomópontján hello a forrás réteg: hello célfájl layer csatlakoztatott tooevery csomópontja.  

Ezenkívül Net # támogatja a következő speciális kapcsolat kötegek négyféle hello:  

* **Szűrt kötegek**. hello felhasználói hello réteg forráscsomópont és hello réteg célcsomópont hello helyét használatával adhatja meg a predikátum. Csomópontok hello predikátum értéke igaz, amikor csatlakozik.
* **Convolutional csomagok**. hello felhasználói adhatja meg a csomópontok kis környékeken hello forrás réteg. Minden csomópont hello célfájl layer csatlakoztatott tooone hálózatok hello forrás rétegben csomópontok.
* **Csomagok készletezését** és **válasz normalizálási kötegek**. Ezek a hasonló tooconvolutional csomagok adott hello felhasználói hello forrás rétegben csomópontok kis környékeken határozza meg. hello különbség, hogy hello súlyozását hello szélek az ezek a csomagok nincsenek trainable. Ehelyett egy előre definiált függvény alkalmazott toohello forráscsomópont értékek toodetermine hello cél csomópont értéke.  

Net # toodefine hello struktúra Neurális hálózat révén lehetséges toodefine összetett struktúrák például mély Neurális hálózatokat vagy a tetszőleges dimenziók, vagyis a adatok, például a lemezkép, hang-, vagy videó tooimprove learning convolutions.  

## <a name="supported-customizations"></a>Támogatott testreszabások
az Azure Machine Learning létrehozott Neurális hálózat modellek hello architektúrájának nagymértékben testreszabható Net # használatával. A következőket teheti:  

* A Rejtett réteg és vezérlési csomópontok száma hello létrehozása minden egyes rétegben.
* Adja meg, hogyan rétegek egyéb kapcsolódó toobe tooeach.
* Adja meg a Speciális kapcsolat struktúrák, például convolutions és a súlyozást csomagok megosztása.
* Adjon meg másik aktiválási funkciók.  

A hello specification nyelvi szintaxisát, lásd: [struktúra Specification](#Structure-specifications).  

Néhány általános gépi tanulási a feladatok az egyirányú (szimplex) toocomplex, a Neurális hálózatokat meghatározásának tekintse meg a [példák](#Examples-of-Net#-usage).  

## <a name="general-requirements"></a>Általános követelmények
* Pontosan egy kimeneti réteg legalább egy bemeneti réteg és nulla vagy több rejtett rétegben kell. 
* Minden egyes réteg csomópontok, tetszőleges dimenziók téglalap alakú tömbje fogalmilag rendezett rögzített számú rendelkezik. 
* Bemeneti rétegek társított képzett paraméter nélküli, és ahol példányadatokat belép hello hálózati hello pontot jelöl. 
* (Hello rejtett, és a kimeneti rétegek) trainable rétegek társított súlyok és elfogultság ismert képzett paraméterek. 
* hello forrás és cél csomópontok külön rétegekben kell lennie. 
* Kapcsolatok aciklikus; kell lennie. Ez azt jelenti nem lehet hátsó toohello kezdeti forráscsomópont vezető kapcsolatok láncolata.
* hello kimeneti réteg nem lehet kapcsolat köteg forrás réteget.  

## <a name="structure-specifications"></a>Struktúra specifikációk
Három szakaszból tevődik össze a Neurális hálózat struktúra specifikációval: hello **konstans deklarációjában**, hello **deklaráció réteg**, hello **kapcsolat deklaráció**. Szerepel továbbá egy nem kötelező **fájlmegosztás a nyilatkozatot** szakasz. hello szakaszok bármilyen sorrendben adható meg.  

## <a name="constant-declaration"></a>Konstans deklarációjában
Egy konstans deklarációjában nem kötelező megadni. A azt jelenti, hogy toodefine hello Neurális hálózat definíciójának máshol használt értékek biztosít. hello deklaráció utasítás áll egy egyenlőségjellel és egy érték kifejezést azonosítója.   

Például a következő utasítás hello meghatározása állandó **x**:  

    Const X = 28;  

toodefine két vagy több állandók egyidejűleg, tegye hello típusú azonosító neveket és értékeket kell használni, és pontosvesszővel válassza el egymástól elválasztani őket. Példa:  

    Const { X = 28; Y = 4; }  

minden egyes hozzárendelés kifejezés jobb oldalán hello lehet egy egész számot, egy valós szám, egy logikai érték (IGAZ vagy hamis) vagy egy kifejezésnek. Példa:  

    Const { X = 17 * 2; Y = true; }  

## <a name="layer-declaration"></a>Réteg nyilatkozat
hello réteg deklaráció szükség. Meghatározza a hello méretét és hello réteg, beleértve a kapcsolat kötegek és attribútumok forrását. hello hello réteg (bemeneti, rejtett vagy kimeneti) hello neve deklaráció utasítás kezdődik, hello dimenziók hello réteg (egy pozitív egész számok rekord) követ. Példa:  

    input Data auto;
    hidden Hidden[5,20] from Data all;
    output Result[2] from Hidden all;  

* hello termék hello dimenziók hello rétegben található csomópontok számának hello. Ebben a példában a rendszer két dimenzió [5,20], ami azt jelenti, hogy 100 csomópontok hello rétegben.
* hello rétegek deklarálható bármilyen sorrendben, egy kivétellel: Ha egynél több bemeneti réteg van definiálva, hello rendelés deklarálva van egyeznie kell az hello szolgáltatások hello bemeneti adatok sorrendjét.  

toospecify, amelyek egy rétegben található csomópontok számának hello automatikusan határozza meg, használjon hello **automatikus** kulcsszó. Hello **automatikus** kulcsszó különböző hatások, attól függően, hogy hello réteg van:  

* A bemeneti réteg nyilatkozat, a csomópontok száma hello néhány hello szolgáltatás hello bemeneti adatok.
* A Rejtett réteg deklarációjában csomópontok száma hello hello szám hello paraméter értéke által meghatározott **rejtett csomópontok száma**. 
* A kimeneti réteg nyilatkozat, a csomópontok száma hello két osztályú osztályozási, a regresszióra és multiclass besorolási kimeneti csomópontok száma egyenlő toohello 1 2.   

Például hello következő hálózatdefiníció lehetővé teszi, hogy automatikusan határozza meg az összes rétegek toobe hello mérete:  

    input Data auto;
    hidden Hidden auto from Data all;
    output Result auto from Hidden all;  


Egy olyan réteghez, trainable réteg nyilatkozatot (hello rejtett vagy kimeneti rétegek) hello kimeneti függvény (más néven az aktiválási függvény), amely alapértelmezés szerint használt érték túl választhatóan is**sigmoid** besorolási modell és **lineáris** regressziós modell. (Még akkor is, ha hello alapértelmezett használatához is explicit módon azt hello aktiválási függvény, jobb érthetőség kedvéért bizonyos igény.)

hello következő kimeneti-funkciók támogatottak:  

* sigmoid
* lineáris
* softmax
* rlinear
* Négyzetes
* Sqrt
* srlinear
* ABS
* TANH 
* brlinear  

Például a következő nyilatkozatot hello használ hello **softmax** függvény:  

    output Result [100] softmax from Hidden all;  

## <a name="connection-declaration"></a>Kapcsolat nyilatkozat
Hello trainable réteg meghatározása, után azonnal definiált hello rétegek közötti kapcsolatok kell deklarálnia. hello kapcsolat köteg deklaráció hello kulcsszóval kezdődik **a**, hello köteg forrás réteg és hello típusú kapcsolat köteg toocreate hello neve követ.   

Jelenleg kapcsolat csomagok öt típusú támogatottak:  

* **Teljes** kötegek hello kulcsszó által jelzett **összes**
* **Szűrt** kötegek hello kulcsszó által jelzett **ahol**, utána pedig a predikátum kifejezés
* **Convolutional** kötegek hello kulcsszó által jelzett **convolve**, utána pedig hello konvolúció attribútumok
* **Készletezését** kötegek hello kulcsszavak által jelzett **készlet maximális** vagy **készlet témakörök**
* **Válasz normalizálási** kötegek hello kulcsszó által jelzett **válasz alapértelmezetté**      

## <a name="full-bundles"></a>Teljes kötegek
A teljes kapcsolat nyalábot hello réteg tooeach forráscsomópont hello cél rétegben minden csomópontjáról kapcsolatot tartalmazza. Ez a hello alapértelmezett hálózati kapcsolat típusa.  

## <a name="filtered-bundles"></a>Szűrt kötegek
Köteg végrehajtása szűrt kapcsolaton keresztül specifikáció tartalmazza a predikátum, szintaktikailag, kifejezett jelentős, például a C# lambda kifejezésben. hello alábbi példa meghatározza, hogy két szűrt csomagokat:  

    input Pixels [10, 20];
    hidden ByRow[10, 12] from Pixels where (s,d) => s[0] == d[0];
    hidden ByCol[5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;  

* A predikátum hello *ByRow*, **s** indexet jelölő csomópontok hello bemeneti réteg, hello téglalap alakú tömbbe paraméter *képpont*, és **d**  indexet jelölő csomópontok hello rejtett réteg, hello tömbbe paraméter *ByRow*. mindkét típusú hello **s** és **d** hosszúságú két számokból álló rekordot van. Fogalmilag **s** keresztül az egész számok minden pár címtartományok *0 < = [0] s < 10* és *0 < = s[1] < 20*, és **d**  az egész számok, minden pár keresztül címtartományok *0 < = [0] d < 10* és *0 < = d[1] < 12*. 
* Hello hello predikátum kifejezés jobb oldali egy feltétel van. Ebben a példában minden egyes értékéhez **s** és **d** hello feltétel értéke igaz, hogy nincs-e a hello réteg csomópont toohello cél réteg forráscsomópont él. Ebből kifolyólag a szűrőkifejezés azt jelzi, hogy adott hello csomag tartalmaz egy kapcsolat által meghatározott hello csomópontról **s** toohello csomópont által meghatározott **d** minden olyan esetben, ahol a [0] s az egyenlő tood [0].  

Másik lehetőségként a szűrt köteg súlyok készlete is megadhat. hello értéke hello **súlyok** attribútumnak kell lennie egy rekordot a pontértékek hello köteg által definiált kapcsolatok hello számának megfelelő hosszúságú lebegőpontos. Alapértelmezés szerint a súlyok véletlenszerűen generált.  

Súlyértékeket hello cél csomópont indexe szerint vannak csoportosítva. Ez azt jelenti, hogy ha hello első célcsomópont csatlakozik tartott forrás csomópontok, először hello *K* hello elemeinek **súlyok** rekordot hello súlyok hello első cél csomópontján, index forrássorendben. Ugyanez vonatkozik, a fennmaradó cél csomópontok hello hello.  

Lehetséges toospecify súlyok közvetlenül, állandó értékek is. Például ha hello súlyok korábban megtanulta, megadhatja azokat ezen szintakszist használó állandóként:

    const Weights_1 = [0.0188045055, 0.130500451, ...]


## <a name="convolutional-bundles"></a>Convolutional kötegek
Amikor hello betanítási adatok struktúrája a homogén, a convolutional kapcsolatok olyan gyakran használt toolearn magas szintű funkciók hello adatok. Például kép, hang- vagy videó, térbeli vagy időbeli granularitása lehet az adatokat viszonylag egységes.  

Convolutional kötegek alkalmaz téglalap alakú **kernelek** , amely keresztül hello dimenziók vannak ütközik. Tulajdonképpen minden kernel a helyi környékeken, hivatkozott tooas alkalmazza a súlyok készletét **kernel-alkalmazásokra**. Minden egyes kernel alkalmazás megfelel tooa csomópont hello forrás rétegben hivatkozott tooas hello **központi csomópont**. a kernel hello súlyozását sok kapcsolatok között vannak megosztva. Convolutional a köteg, minden egyes kernel téglalap alakú, és minden kernel-alkalmazásokra hello azonos méretét.  

Convolutional csomagok a következő attribútumok hello támogatja:

**InputShape** hello granularitása hello forrás réteg hello alkalmazásában ezen convolutional csomag határozza meg. hello értéknek pozitív egész számok rekordot kell lennie. hello egész számok szorzatát hello hello forrás rétegben található csomópontok számának hello egyenlőnek kell lennie, de egyébként nem kell toomatch hello granularitása hello forrás réteg deklarált. a rekord hossza hello válik hello **aritása** hello convolutional köteg értékét. (Általában aritása egységfigyelőt toohello számú argumentum vagy a operandusok használata történt, amely egy olyan függvényt is igénybe vehet.)  

toodefine hello alakzat és hello mag, a helyek hello attribútumok használata **KernelShape**, **Stride**, **kitöltési**, **LowerPad**, és **UpperPad**:   

* **KernelShape**: (kötelező) meghatározza hello dimenzióinak, minden egyes kernel a hello convolutional köteg. hello értékének pozitív egész számok hello aritása hello köteg egyenlő hosszúságú rekordot kell lennie. Minden egyes összetevő a rekord nem nagyobbnak kell lennie megfelelő összetevője hello **InputShape**. 
* **STRIDE**: (opcionális) meghatározza hello méretű lépés, amely hello hello központi csomópontok közötti távolság szerint hello konvolúció (minden dimenzió egy lépésköz mérete), a késleltetett. hello értéknek pozitív egész számok hello köteg hello aritása hosszúságú rekordot kell lennie. Minden egyes összetevő a rekord nem nagyobbnak kell lennie megfelelő összetevője hello **KernelShape**. hello alapértelmezett értéke az összes összetevő egyenlő tooone rekordot. 
* **Megosztás**: (nem kötelező), minden egyes dimenziójának hello konvolúció megosztásának meghatározza hello súly. egyetlen logikai érték vagy egy logikai értékek hello köteg hello aritása hosszúságú rekord hello érték lehet. Egyetlen logikai érték kiterjesztett toobe hello megfelelő hosszúságú az összes összetevő rekordot egyenlő toohello megadott érték. hello alapértelmezett érték egy rekord, amely az összes IGAZ érték áll. 
* **MapCount**: (opcionális) meghatározza hello száma a szolgáltatás a hello convolutional köteg képezi le. hello érték egy pozitív egész szám vagy egy pozitív egész számok hello köteg hello aritása hosszúságú rekord lehet. Csak egyetlen egész értéket ki van bővítve toobe hello megfelelő hosszúságú a hello első összetevők egyenlő toohello egy rekord megadott érték és az összes többi összetevő egyenlő tooone hello. hello alapértelmezett érték: egyet. hello teljes száma a szolgáltatás maps hello termék hello összetevők hello rekord. hello összetevői között és az összes faktoring hello határozza meg, hogyan hello cél csomópontok hello szolgáltatás térkép értékek vannak csoportosítva. 
* **Súlyozás**: (opcionális) meghatározza hello kezdeti súlyok a hello köteg. hello értéknek kell lennie egy rekordot a pontértékek, amely hello számát kernelek alkalommal hello súlyok / kernel, a cikk későbbi részében meghatározott hosszúságú lebegőpontos. hello alapértelmezett súlyozás véletlenszerűen jönnek létre.  

Kitöltési,-folyamatban, egymást kölcsönösen kizáró hello tulajdonságok szabályozó tulajdonságok két csoportjára van:

* **Kitöltési**: (opcionális) megállapítja, hogy hello bemeneti kell hosszúságra használatával egy **alapértelmezett kitöltő séma**. hello érték lehet egy logikai érték, vagy egy rekord logikai értékek hello köteg hello aritása hosszúságú lehet. Egyetlen logikai érték kiterjesztett toobe hello megfelelő hosszúságú az összes összetevő rekordot egyenlő toohello megadott érték. Hello dimenzió értéke igaz, ha hello forrás lesz logikailag kiegészítve az adott dimenzióban nulla értékű cellákat toosupport kiegészítő rendszermag alkalmazásokkal, úgy, hogy központi csomópontja hello hello első és utolsó kernelek az adott dimenzióban hello első és utolsó csomópontja az adott dimenzióban hello forrás rétegben. Minden dimenzió "típusú" csomópontok száma hello van így, automatikusan határozza meg, toofit pontosan *(InputShape [d.] - 1) / Stride [d.] + 1* kernelek hosszúságra hello forrás rétegbe. Hello dimenzió értéke HAMIS, ha kernelek határozzák meg, hogy mindkét oldalon kimenő fennmaradó csomópontok száma hello hello hello azonos (felfelé tooa különbség az 1). hello alapértelmezett Ez az attribútum értéke az összes összetevő egyenlő tooFalse rekordot.
* **UpperPad** és **LowerPad**: (nem kötelező) adja meg nagyobb mértékben vezérelheti a kitöltési toouse hello mennyiségét. **Fontos:** ezek az attribútumok adható meg, ha, és csak akkor, ha hello **kitöltési** fenti tulajdonság ***nem*** definiálva. hello értékének egész rekordokat, amelyek hello aritása hello köteg hosszúságú kell lennie. Ha ezek az attribútumok meg van adva, "típusú" csomópontokat ad hozzá toohello alsó és felső végén minden egyes dimenziójának hello bemeneti réteg. hello csomópontok száma hozzáadott toohello alsó és felső végződik minden dimenzió határozza meg **LowerPad**[i] és **UpperPad**[i] osztályban. tooensure, hogy kernelek megfelelnek túl csak "tényleges" csomópont, és nem túl "típusú" csomópontok hello a következő feltételeknek kell teljesülniük:
  * Az egyes összetevők **LowerPad** KernelShape [d.] szigorúan kisebbnek kell lennie 2. 
  * Az egyes összetevők **UpperPad** nem nagyobbnak kell lennie [d.] KernelShape / 2. 
  * hello alapértelmezett attribútum értéke az összes összetevő egyenlő too0 rekordot. 

hello beállítás **kitöltési** = true lehetővé teszi, hogy mekkora térközt, tookeep hello "valós" bemeneti hello belül hello kernel "center" igény szerint. Egy kicsit hello kimeneti méretének kiszámításához hello matematikai értékre változik. Általában hello kimeneti mérete *D* számítja ki, hogy *D = (I - K) / S + 1*, ahol *I* hello bemeneti mérete *K* hello kernel mérete *S* van hello stride és  */*  szám (szám nullához) van. Ha UpperPad = [1, 1], hello bemenet mérete *I* tulajdonképpen 29, és így *D = (29-5) / 2 + 1 = 13*. Azonban, amikor **kitöltési** lényegében = true, *I* által kap bumped *K - 1*; ezért *D = ((28 + 4) - 5) / 2 + 1 = 27 / 2 + 1 = 13 + 1 = 14*. Az értékek megadásával **UpperPad** és **LowerPad** kitöltési mint Ön imént beállított hello több ellenőrzést le **kitöltési** = true.

Convolutional hálózatok és az alkalmazásokkal kapcsolatos további információkért lásd: ezek a cikkek:  

* [http://deeplearning.NET/Tutorial/lenet.HTML](http://deeplearning.net/tutorial/lenet.html)
* [http://Research.microsoft.com/Pubs/68920/icdar03.PDF](http://research.microsoft.com/pubs/68920/icdar03.pdf) 
* [http://People.csail.Mit.edu/jvb/Papers/cnn_tutorial.PDF](http://people.csail.mit.edu/jvb/papers/cnn_tutorial.pdf)  

## <a name="pooling-bundles"></a>Csomagok készletezése
A **köteg készletezését** vonatkozik geometriai hasonló tooconvolutional kapcsolatot, de előre definiált függvények toosource csomópont értékek tooderive hello cél csomópont értéket használ. Emiatt készletezésével kötegek rendelkezik (súlyok vagy elfogultság) trainable állapot nélküli. Összes kivéve convolutional attribútumok hello csomagok támogatási készletezését **megosztás**, **MapCount**, és **súlyok**.  

Általában nem lehetnek átfedők hello kernelek szomszédos készletezésével egységek foglalja össze. Minden dimenzió egyenlő tooKernelShape [d.] [d] Stride esetén kapott hello réteg hello hagyományos helyi készletezésével szintje, amely általában alkalmazottja convolutional Neurális hálózatokat. Minden egyes célcsomópont kiszámítja a maximális hello vagy a kernel hello forrás rétegben hello tevékenységeit hello közepét.  

a következő példa hello készletezésével köteg mutatja be: 

    hidden P1 [5, 12, 12]
      from C1 max pool {
        InputShape  = [ 5, 24, 24];
        KernelShape = [ 1,  2,  2];
        Stride      = [ 1,  2,  2];
      }  

* hello köteg hello aritása a következő: 3 (hello rekordokat hosszát hello **InputShape**, **KernelShape**, és **Stride**). 
* hello forrás rétegben csomópontok száma hello *5 * 24 * 24 = 2880*. 
* Ennek oka egy hagyományos helyi készletezésével réteg **KernelShape** és **Stride** egyenlő. 
* hello cél rétegben csomópontok száma hello *5 * 12 * 12 = 1440*.  

Egyesítési rétegek kapcsolatos további információkért lásd: ezek a cikkek:  

* [http://www.cs.Toronto.edu/~hinton/absps/imagenet.PDF](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf) (szakaszban 3.4)
* [http://cs.nyu.edu/~koray/publis/lecun-iscas-10.PDF](http://cs.nyu.edu/~koray/publis/lecun-iscas-10.pdf) 
* [http://cs.nyu.edu/~koray/publis/jarrett-iccv-09.PDF](http://cs.nyu.edu/~koray/publis/jarrett-iccv-09.pdf)

## <a name="response-normalization-bundles"></a>Válasz normalizálási kötegek
**Válasz normalizálási** egy helyi normalizálási rendszer Geoffrey Hinton, először bevezetett és mások hello dokumentum [ImageNet Classiﬁcation Convolutional Neurális hálózatokat a](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf). Válasz normalizálási használt tooaid általánosítása Neurális hálók. Amikor egy idegsejt folyamatban van az aktiválás nagyon magas szintű, egy helyi válasz normalizálási réteg mellőzi hello aktiválási szintű hello körülvevő idegsejtek csoportjának viselkedését. Ehhez három paraméterekkel (***α***, ***β***, és ***k***) és egy convolutional struktúra (vagy hálózatok alakzat). Minden idegsejt hello cél rétegben ***y*** felel meg a tooa idegsejt ***x*** hello forrás rétegben. aktiválási szintű hello ***y*** adja meg a következő képlet, hello ahol ***f*** hello aktiválási szint idegsejt, és ***Nx*** hello kernel (vagy hello tartalmazó hello beállítása a hello ablakban a idegsejtek csoportjának viselkedését ***x***), a következő convolutional struktúra hello által definiált:  

![][1]  

Válasz normalizálási csomagokat támogatja az összes hello convolutional attribútumokat kivételével **megosztás**, **MapCount**, és **súlyok**.  

* Ha hello kernel tartalmaz idegsejtek csoportjának viselkedését a hello azonos leképezni ***x***, hello normalizálási séma hivatkozott tooas **normalizálási képezi le azonos**. toodefine azonos leképezése hello első koordináta a rendszer **InputShape** 1 hello értékűnek kell lennie.
* Ha hello kernel tartalmaz idegsejtek csoportjának viselkedését a hello azonos térbeli helyzetben ***x***, de hello idegsejtek csoportjának viselkedését más maps, a hello normalizálási séma neve **keresztben leképezi a normalizálási**. Az ilyen típusú válasz normalizálási egy formája, amelyet oldalirányú gátló inspirálta hello típus van a valós idegsejtek csoportjának viselkedését, többek között a különböző térképeken számított idegsejt kimenetek nagy aktiválási szintjeinek verseny létrehozása valósítják meg. toodefine közötti leképezések normalizálási, hello első koordinátájának nagyobb, mint egy és maps hello száma nem lehet nagyobb egész számnak kell lennie, és hello rest hello koordinátának kell rendelkeznie a hello érték 1.  

Válasz normalizálási csomagok egy előre definiált függvény toosource csomópont értékek toodetermine hello cél csomópont érték alkalmazni, mert nincs trainable állapot (súlyok vagy elfogultság) rendelkeznek.   

**Riasztási**: hello csomópontok hello cél rétegben, amelyek központi csomópontja hello hello kernelek tooneurons felel meg. Például, ha a [d] KernelShape páratlan, majd *KernelShape [d.] / 2* toohello központi kernel csomópont felel meg. Ha *KernelShape [d.]* az még akkor is, hello központi csomópont *KernelShape [d.] / 2-1*. Ezért ha **kitöltési**[d.] értéke HAMIS, hello első és utolsó hello *KernelShape [d.] / 2* csomópontok nem rendelkeznek megfelelő csomópontokat hello cél rétegben. tooavoid ebben az esetben adja meg **kitöltési** mint [értéke true, true,..., igaz].  

Ezenkívül toohello négy attribútumok a korábban ismertetett válasz normalizálási csomagokat is a következő attribútumok támogatási hello:  

* **Alpha**: (kötelező) adja meg egy lebegőpontos érték túl megfelelő***α*** hello előző képletben. 
* **Beta**: (kötelező) adja meg egy lebegőpontos érték túl megfelelő***β*** hello előző képletben. 
* **Eltolás**: (nem kötelező) adja meg egy lebegőpontos érték túl megfelelő***k*** hello előző képletben. Alapértelmezés szerint too1.  

hello alábbi példa meghatározza, hogy a válasz normalizálási nyalábot, ezek az attribútumok használata:  

    hidden RN1 [5, 10, 10]
      from P1 response norm {
        InputShape  = [ 5, 12, 12];
        KernelShape = [ 1,  3,  3];
        Alpha = 0.001;
        Beta = 0.75;
      }  

* hello forrás réteg öt 12 x 12 1440 csomópontok összesítés aof léptékű maps tartalmazza. 
* hello értékének **KernelShape** jelzi, hogy ez egy azonos normalizálási térképréteg, 3 x 3 téglalap hello hálózatok esetén. 
* alapértelmezett értékének hello **kitöltési** értéke HAMIS, így hello célfájl layer tartoznak minden dimenzió csak 10 csomópontok. egy csomópont tooinclude hello cél rétegben, amely megfelel a tooevery csomópont hello forrás rétegben, vegye fel a kitöltési = [true, true, true]; és túl RN1 hello méretének módosítása [5, 12, 12].  

## <a name="share-declaration"></a>Fájlmegosztás a nyilatkozatot
NET # opcionálisan is támogatja a megosztott súlyozással több csomagokat. bármely két kötegek hello súlyok is lehet megosztani, ha azok struktúrák vannak hello azonos. szintaxis a következő hello meghatározza, hogy a megosztott súlyozással rendelkező csomagok:  

    share-declaration:
        share    {    layer-list    }
        share    {    bundle-list    }
       share    {    bias-list    }

    layer-list:
        layer-name    ,    layer-name
        layer-list    ,    layer-name

    bundle-list:
       bundle-spec    ,    bundle-spec
        bundle-list    ,    bundle-spec

    bundle-spec:
       layer-name    =>     layer-name

    bias-list:
        bias-spec    ,    bias-spec
        bias-list    ,    bias-spec

    bias-spec:
        1    =>    layer-name

    layer-name:
        identifier  

Például hello következő megosztás-adja meg hello réteg nevét, amely azt jelzi, hogy a súlyok és a elfogultság megosztott:  

    Const {
      InputSize = 37;
      HiddenSize = 50;
    }
    input {
      Data1 [InputSize];
      Data2 [InputSize];
    }
    hidden {
      H1 [HiddenSize] from Data1 all;
      H2 [HiddenSize] from Data2 all;
    }
    output Result [2] {
      from H1 all;
      from H2 all;
    }
    share { H1, H2 } // share both weights and biases  

* hello bemeneti szolgáltatások particionáltak két egyenlő méretű bemeneti rétegben. 
* rejtett hello rétegek majd számítási hello két bemeneti rétegek magasabb szintű funkciók. 
* hello megosztás-deklaráció azt jelenti, hogy *H1* és *H2* a hello ki kell számítani a a megfelelő bemenetei azonos módon.  

Azt is megteheti ez sikerült lehet megadni két külön megosztás-deklarációk az alábbiak szerint:  

    share { Data1 => H1, Data2 => H2 } // share weights  

<!-- -->

    share { 1 => H1, 1 => H2 } // share biases  

Hello rövid alak: / is használhatja, csak ha hello rétegek csomagban szerepel. Megosztás általában lehetséges csak azonos, ami azt jelenti, hogy rendelkezik-e azonos mérete, azonos convolutional geometriai, és így tovább hello hello vonatkozó struktúra esetén.  

## <a name="examples-of-net-usage"></a>Net # használati példák
Ez a témakör néhány példa arra, hogyan használhatja a Net # rejtett tooadd rétegek hello úgy, hogy a Rejtett réteg más rétegeiből kommunikál, és convolutional hálózatok felépítéséhez határozza meg.   

### <a name="define-a-simple-custom-neural-network-hello-world-example"></a>Adja meg egy egyszerű egyéni Neurális hálózat: "Hello, World" – példa
Ez egyszerű példa bemutatja, hogyan toocreate a Neurális hálózat modell, amely rendelkezik egy rejtett rétegben.  

    input Data auto;
    hidden H [200] from Data all;
    output Out [10] sigmoid from H all;  

hello példában látható módon néhány alapvető parancsok az alábbiak szerint:  

* hello első sor meghatározása hello bemeneti réteg (nevű *adatok*). Amikor az hello **automatikus** kulcsszóval, hello Neurális hálózat hello bemeneti példák automatikusan összes szolgáltatás-oszlopa tartalmazza. 
* hello második sor hello rejtett réteg hoz létre. hello neve *H* toohello rejtett rétegben, amely 200 csomópont van hozzárendelve. Ez a réteg teljesen csatlakoztatott toohello bemeneti réteg.
* hello harmadik sor hello kimeneti réteg határozza meg (nevű *O*), 10 kimeneti csomópontok, amely tartalmazza. Hello Neurális hálózat besorolást használják, ha van egy kimeneti csomópont / osztály. hello kulcsszó **sigmoid** azt jelzi, hogy hello kimeneti függvény alkalmazott toohello kimeneti réteg.   

### <a name="define-multiple-hidden-layers-computer-vision-example"></a>Adja meg a több rejtett réteg: számítógép átfogóan bemutató példa
hello következő példa bemutatja, hogyan toodefine egy kicsit bonyolultabb Neurális hálózat, a több egyéni rejtett réteg.  

    // Define hello input layers 
    input Pixels [10, 20];
    input MetaData [7];

    // Define hello first two hidden layers, using data only from hello Pixels input
    hidden ByRow [10, 12] from Pixels where (s,d) => s[0] == d[0];
    hidden ByCol [5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;

    // Define hello third hidden layer, which uses as source hello hidden layers ByRow and ByCol
    hidden Gather [100] 
    {
      from ByRow all;
      from ByCol all;
    }

    // Define hello output layer and its sources
    output Result [10]  
    {
      from Gather all;
      from MetaData all;
    }  

Ez a példa több hello Neurális hálózatokat specification nyelvnek olyan szolgáltatását mutatja be:  

* hello struktúrának van két bemeneti réteg *képpont* és *metaadatok*.
* Hello *képpont* réteg rétegeket célfájl, két kapcsolat csomagokat, a forrás-rétege *ByRow* és *ByCol*.
* rétegek hello *összegyűjtése* és *eredmény* cél rétegek több kapcsolat csomagokat a rendszer.
* hello kimeneti réteg, *eredmény*, a cél réteg két kapcsolat kötegek; egyet hello második szint cél rétegként rejtett (összefog), és a cél rétegként hello más hello bemeneti réteg (MetaData).
* a Rejtett réteg hello *ByRow* és *ByCol*, adja meg a szűrt kapcsolat predikátum kifejezések használatával. Pontosabban, hello csomópontja *ByRow* : [x, y] csatlakoztatott toohello csomópontjának van *képpont* , amelyek hello első index koordináta egyenlő toohello csomópont első összehangolják x. Ehhez hasonlóan hello csomópontja *ByCol: [x, y] _Pixels csatlakoztatott toohello csomópontjának van* hello második index koordináta egy második koordináta hello csomópont, amelyeken y.  

### <a name="define-a-convolutional-network-for-multiclass-classification-digit-recognition-example"></a>Adja meg a multiclass besorolási convolutional hálózati: számjegy felismerés – példa
a következő hálózati hello hello definíciója tervezett toorecognize számok, és azt szemlélteti, hogy néhány speciális technikák Neurális hálózat testreszabásához.  

    input Image [29, 29];
    hidden Conv1 [5, 13, 13] from Image convolve 
    {
       InputShape  = [29, 29];
       KernelShape = [ 5,  5];
       Stride      = [ 2,  2];
       MapCount    = 5;
    }
    hidden Conv2 [50, 5, 5]
    from Conv1 convolve 
    {
       InputShape  = [ 5, 13, 13];
       KernelShape = [ 1,  5,  5];
       Stride      = [ 1,  2,  2];
       Sharing     = [false, true, true];
       MapCount    = 10;
    }
    hidden Hid3 [100] from Conv2 all;
    output Digit [10] from Hid3 all;  


* hello struktúrának van egy bemeneti rétegben *kép*.
* hello kulcsszó **convolve** azt jelzi, hogy hello rétegek nevű *Conv1* és *Conv2* convolutional rétegek vannak. A réteg nyilatkozatok mindegyikének hello konvolúció attribútumok listája követi.
* nettó hello harmadik rejtett réteg, *Hid3*, amely teljes mértékben kapcsolódó toohello második rejtett rétegben, *Conv2*.
* hello kimeneti réteg, *számjegy*, csatlakoztatott csak toohello harmadik rejtett réteg, *Hid3*. hello kulcsszó **összes** jelzi, hogy hello kimeneti réteg teljesen kapcsolódik túl*Hid3*.
* hello hello konvolúció aritása három (hello rekordokat hosszát hello **InputShape**, **KernelShape**, **Stride**, és **megosztás**). 
* kernel / súlyok hello száma *1 + **KernelShape**\[0] * **KernelShape**\[1] * **KernelShape** \[2] = 1 + 1 * 5 * 5 = 26. Vagy 26 * 50 = 1300*.
* Kiszámolhatja, minden egyes rejtett rétegben hello csomópontok az alábbiak szerint:
  * **NodeCount**\[0] = (5 - 1) vagy 1 + 1 = 5.
  * **NodeCount**\[1] = (13-5) / 2 + 1 = 5. 
  * **NodeCount**\[2] (13-5) = / 2 + 1 = 5. 
* hello csomópontok száma kerülhet sor dimenzióinak, hello deklarált hello segítségével réteg, [50, 5, 5], az alábbiak szerint:  ***MapCount** * **NodeCount** \[ 0] * **NodeCount**\[1] * **NodeCount**\[2] = 10 * 5 * 5 * 5*
* Mivel **megosztás**[d.] értéke hamis csak *d == 0*, kernelek hello száma  ***MapCount** * **NodeCount** \[0] = 10 * 5 = 50*. 

## <a name="acknowledgements"></a>A nyugtázás
hello Net # nyelv Neurális hálózatokat hello architektúrájának testreszabásához fejlesztette ki Microsoft Shon Katzenberger (felelős mérnök, gépi tanulás) és ALEKSZEJ Kamenev (szoftver visszafejtés, a Microsoft Research). Ez belső használatra készült a gépi tanulási projektek és az alkalmazások közötti kép észlelési tootext elemzés. További információkért lásd: [Azure ml - bevezető tooNet # Neurális háló](http://blogs.technet.com/b/machinelearning/archive/2015/02/16/neural-nets-in-azure-ml-introduction-to-net.aspx)

[1]:./media/machine-learning-azure-ml-netsharp-reference-guide/formula_large.gif

