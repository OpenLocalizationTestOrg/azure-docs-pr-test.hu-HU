---
title: "aaaHow toochoose gépi tanulási algoritmusok |} Microsoft Docs"
description: "Hogyan toochoose Azure Machine Learning algoritmusok használata a fürtszolgáltatás felügyelt és nem felügyelt tanítás, osztályozási vagy regressziós kísérleteket."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: a3b23d7f-f083-49c4-b6b1-3911cd69f1b4
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/25/2017
ms.author: garye
ms.openlocfilehash: 367b2278acc2435f27f9d24ead8199db58aca283
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toochoose-algorithms-for-microsoft-azure-machine-learning"></a>Hogyan toochoose algoritmus a Microsoft Azure Machine Learning szolgáltatáshoz
hello válaszolja toohello kérdést "milyen machine learning algoritmus érdemes használni?" mindig kapcsolva "Attól függ." Ez függ hello mérete, a minőségi és a hello adatok jellegét. Attól függ, amit meg akar toodo hello választ. Ez attól függ, hogy hogyan hello matematikai hello algoritmus hello számítógépet, hogy az utasítások lett lefordítva. Annak függvénye, és mennyi idővel rendelkezik. Még a legtöbb észlelt hello adatszakértőkön nem sikerült megállapítani, mely algoritmus hajtja végre a legjobb előtt őket.

## <a name="hello-machine-learning-algorithm-cheat-sheet"></a>hello Machine Learning algoritmus Cheat lap
Hello **Microsoft Azure Machine Learning algoritmus Cheat lap** úgy dönt, hogy hello jobb segítséget nyújt a gép a prediktív elemzési megoldások könyvtárból hello Microsoft Azure Machine Learning-algoritmusok tanulási algoritmus.
Ez a cikk végigvezeti toouse azt.

> [!NOTE]
> toodownload hello Adatlap és mentén kövesse az ebben a cikkben túl Ugrás[gépi tanulási algoritmus-adatlap a Microsoft Azure Machine Learning Studióban](machine-learning-algorithm-cheat-sheet.md).
> 
> 

Ez adatlap egy nagyon adott célközönséget van tartva: egy kezdete adatok tudósok undergraduate szintű machine Learning segítségével, az Azure Machine Learning Studióban, az algoritmus toostart toochoose közben. Ez azt jelenti, hogy néhány általánosítást és oversimplifications teszi, de biztonságos irányba mutat. Azt is jelenti, hogy vannak-e nagy mennyiségű algoritmusok itt nem látható. Növekedésével Azure Machine Learning tooencompass rendelkezésre álló metódusok több teljes készletét, fel kell venni őket.

Ezek a javaslatok még lefordított visszajelzések és tippek sok adatszakértőkön és machine learning szakértői. Jelenleg nem fogadja el a licencfeltételeket a mindent, de már próbáltam tooharmonize a vélemények nyers együttműködési be. A legtöbb hello utasítások érjen véget kezdődhet "Előfeltétel..."

### <a name="how-toouse-hello-cheat-sheet"></a>Hogyan toouse hello cheat lap
Hello elérési útját és algoritmus címkék hello diagram olvassa el "a  *&lt;elérési útját címke&gt;*, használjon  *&lt;algoritmus&gt;*." Például "a *sebesség*, használjon *két osztály logisztikai regresszió*." Egyes esetekben több fiókirodai vonatkozik.
Egyes esetekben tökéletes egyikük sem. Ezt a tervezett toobe szabály az USB-a javaslatok még, így nem kell aggódni pontos alatt.
Több adatszakértőkön I volt szó, az említett módon, hogy hello nagyon ajánlott algoritmus tootry csak arról, hogy hello az összes.

Íme egy példa a hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/) egy kísérlet, amely több algoritmusok elleni hello ugyanazon adatokhoz és összehasonlítja hello eredmények: [több osztály osztályozó összehasonlítása: felismerés betűs ](http://gallery.cortanaintelligence.com/Details/a635502fc98b402a890efe21cec65b92).

> [!TIP]
> Lásd a toodownload és nyomtatási ábrázoló diagram, amely áttekintést nyújt a Machine Learning Studio funkcióiról hello [Azure Machine Learning Studio képességeit áttekintő diagram](machine-learning-studio-overview-diagram.md).
> 
> 

## <a name="flavors-of-machine-learning"></a>Változatban is elkészíti a gépi tanulás
### <a name="supervised"></a>Felügyelt
Felügyelt tanítás során algoritmusok előrejelzésekhez példák alapján. Például a korábbi készlet árak használt toohazard Találgatások jövőbeli áron lehet. Minden egyes példában képzési érdeklő hello értékkel lett címkézve – ebben az esetben hello árfolyam. Felügyelt tanulási algoritmus ezen érték címkék minták keresi. Használhat bármilyen vonatkozó információk – hello hét, hello szezon, hello vállalati pénzügyi adatokat, iparági hello típusú, zavaró geopolitikai események hello jelenléte hello napja – minden algoritmus minták különböző típusú, és. Miután hello algoritmus megtalálta hello legjobb mintát azt is, akkor használja, hogy a minta toomake előrejelzéseket címkézetlen tesztelési adatokat – jövő árak.

Felügyelt tanítás során olyan népszerű és hasznos a gépi tanulás. Egy kivétellel összes hello modult az Azure Machine Learning felügyelete tanulási algoritmus. Több adott típusú felügyelt tanítás során, amely az Azure Machine Learning belül vannak megadva: besorolás, a regresszióra és anomáliadetektálási észlelése.

* **Besorolási**. Hello adatok éppen használt toopredict egy kategóriát, amikor felügyelt tanítás során rövidítése besorolás. Ez helyzet hello "cat" vagy "kutya" képként lemezkép hozzárendelésekor. Ha csak két választási lehetőség, hívott **két osztályú** vagy **binomiális besorolás**. További kategóriájuk van, mint amikor hello nyer, hello NCAA március Madness sportverseny előrejelzésére, ha a probléma nevezik **több osztály besorolás**.
* **Regressziós**. Egy érték van folyamatban előre jelezni, csakúgy, mint a mennyisége, felügyelt tanítás során regressziós neve.
* **Anomáliadetektálás**. Egyes esetekben hello célja egyszerűen ritka tooidentify adatpontok. A csalások felderítését például minden nagyon szokatlan hitelkártya költségkeret minták a gyanús. hello lehetséges változata, számos, és ezért néhány, hogy a rendszer nem megvalósítható toolearn milyen csalárd tevékenység néz példák hello. A megközelítést anomáliadetektálás typedValue toosimply megtudhatja, milyen szokásos tevékenység néz (egy előzmények nem csalárd tranzakciókat használatával), és mindent, ami jelentősen eltérő azonosításához.

### <a name="unsupervised"></a>Felügyeletlen
Felügyeletlen tanulási adatpontok hozzájuk társított címkéket nem rendelkezhet. Ehelyett egy felügyeletlen hello célját tanulási algoritmus, hogy egyes módon vagy toodescribe hello adatok struktúrája szervezze. Ez azt csoportokba a csoport, vagy megtekint összetett adatokat úgy, hogy egyszerűbb vagy több szervezett különböző módokat kereséséhez.

### <a name="reinforcement-learning"></a>Learning megerősítése
Megerősítése szeretnének, lekérdezi a toochoose hello algoritmus művelet válasz tooeach adatpontban. hello tanulási algoritmus is kap egy rövid időn belül, mennyire jó hello döntési jelző lett ellenszolgáltatás jel.
Ennek alapján, hello algoritmus módosítja annak biztosításával kapcsolatos stratégia rendelés tooachieve hello legmagasabb ellenszolgáltatás. Jelenleg nincsenek nem tanulási algoritmus Azure Machine Learning modulok megerősítése. Megerősítése tanulás esetében gyakori, a robotics, ahol hello érzékelő szivattyútelepek érzékelőinek adatai egy helyen időben készlete adatpont és hello algoritmus ki kell választania a következő művelet hello robot. Akkor is természetes alkalmas, az eszközök internetes hálózata alkalmazások.

## <a name="considerations-when-choosing-an-algorithm"></a>Egy algoritmus kiválasztása szempontjai
### <a name="accuracy"></a>Pontosság
Első hello legpontosabb válasz lehetséges nem mindig szükséges.
Egyes esetekben közelítés számára, attól függően, hogy szeretné használni. Ha a hello esetben is képes toocut által jelentősen több hozzávetőleges metódusával váltani a feldolgozási ideje. Egy másik hozzávetőleges módszerek további előnye, hogy azok természetesen az egyes elkerülése érdekében [overfitting](https://youtu.be/DQWI1kvmwRg).

### <a name="training-time"></a>Képzési idő
hello modell jelentős mértékben platformjától függően algoritmusok perc vagy óra szükséges tootrain száma. Idő betanítása gyakran szorosan kötődik pontossága – egy általában kísérő egyéb hello. Emellett az egyes algoritmusok érzékenyebb toohello adatpontok száma mint mások.
Korlátozott idő esetén azt is meghajtó hello választott algoritmust, különösen nagyméretű hello adatkészlet esetén.

### <a name="linearity"></a>Lineáris
Ellenőrizze a nagy mennyiségű gépi tanulási algoritmusok lineáris használatát. Lineáris besorolás algoritmusok azt feltételezik, hogy osztályok választhatók el egymástól egyenes (vagy annak újabb dimenziós analóg). Ezek közé tartozik a logisztikai regresszió és vektoros gép támogatása (az Azure Machine Learning megvalósuló).
Lineáris regressziós algoritmus azt feltételezik, hogy a adatok trendek egyenes kövesse. Ilyen Előfeltevések nem alkalmasak olyan problémák, de a többi azok leállíthatja pontossága.

![Az osztály nem lineáris határ][1]

***Az osztály nem lineáris határ*** *-egy lineáris osztályozó algoritmus hagyatkoznia eredményeznének alacsony pontossága*

![Adatok lineáris trend][2]

***Egy lineáris trend adatok*** *-lineáris regressziós módszerrel hoz létre sok szükségesnél nagyobb hibák*

Annak ellenére, hogy a veszélyek lineáris algoritmusok népszerű támadások első sorként. Ezek általában toobe algorithmically gyorsan és egyszerűen kell még betanítani.

### <a name="number-of-parameters"></a>A paraméterek száma
Paraméterei hello forgatógombját egy adatok tudósok lekérdezi tooturn algoritmus beállítása során. Azok a számok, amelyek hatással vannak a hello algoritmus viselkedését, például a hibatűrés vagy ismétlési vagy a beállítások között hello algoritmus viselkedését változatának számát. hello képzési időt és hello algoritmus pontosságát néha lehet meglehetősen bizalmas toogetting csak hello megfelelő beállításokat. Sok paraméterekkel algoritmusok általában, hello legtöbb próbaverzió és a hiba toofind jó kombinációjából van szükségük.

Azt is megteheti, hogy van egy [paraméter abszolút](machine-learning-algorithm-parameters-optimize.md) modul letiltása az Azure Machine Learning, amely automatikusan megkísérli az összes paraméterkombinációk bármilyen részletességű választja. Ez nem egy kiváló módja toomake meg arról, hogy Ön már ölel hello paraméter terület, egy modell hello szükséges idő tootrain exponenciálisan hello számú paraméterrel növeli.

hello feje, hogy ha sok paraméter általában azt jelzi, hogy rendelkezik-e az algoritmus nagyobb rugalmasságot. Nagyon jó pontossága gyakran érhető el. A megadott paraméter-beállításainak megfelelő kombinációja hello található.

### <a name="number-of-features"></a>Szolgáltatások száma
Bizonyos típusú adatok esetén hello számos szolgáltatást lehet összehasonlított toohello nagyon nagy számú adatpontot tartalmaznak. Ez helyzet gyakran hello genetics vagy szöveges adatok. hello szolgáltatásokat nagyszámú le néhány tanulási algoritmusok, így képzési unfeasibly hosszú időt is bog. Támogatási vektoros gépek különösen kiválóan alkalmas toothis eset (lásd alább).

### <a name="special-cases"></a>Bizonyos esetekben
Néhány tanulási algoritmusok ellenőrizze hello adatok vagy a szükséges hello eredmények hello szerkezete adott feltételezéseket. Ha talál egyet, amely megfelel az igényeinek, azt tudhatja meg több eredményeket, több pontos előrejelzéseket vagy gyorsabb képzési.

| **Algoritmus** | **Pontosság** | **Képzési idő** | **Lineáris** | **Paraméterek** | **Megjegyzések** |
| --- |:---:|:---:|:---:|:---:| --- |
| **Két osztályú osztályozás** | | | | | |
| [logisztikai regresszió](https://msdn.microsoft.com/library/azure/dn905994.aspx) | |● |● |5 | |
| [döntési erdő](https://msdn.microsoft.com/library/azure/dn906008.aspx) |● |○ | |6 | |
| [döntési Dzsungel](https://msdn.microsoft.com/library/azure/dn905976.aspx) |● |○ | |6 |Alacsony memóriaigény |
| [súlyozott döntési fája](https://msdn.microsoft.com/library/azure/dn906025.aspx) |● |○ | |6 |Nagy memóriaigény |
| [Neurális hálózat](https://msdn.microsoft.com/library/azure/dn905947.aspx) |● | | |9 |[További testreszabási lehetőség.](http://go.microsoft.com/fwlink/?LinkId=402867) |
| [átlagolt perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx) |○ |○ |● |4 | |
| [támogatja a vektoros gép](https://msdn.microsoft.com/library/azure/dn905835.aspx) | |○ |● |5 |Jó nagy szolgáltatás beállítása |
| [helyileg mély támogatási vektoros gép](https://msdn.microsoft.com/library/azure/dn913070.aspx) |○ | | |8 |Jó nagy szolgáltatás beállítása |
| [Bayes' pontozó gépet](https://msdn.microsoft.com/library/azure/dn905930.aspx) | |○ |● |3 | |
| **Több osztály besorolás** | | | | | |
| [logisztikai regresszió](https://msdn.microsoft.com/library/azure/dn905853.aspx) | |● |● |5 | |
| [döntési erdő](https://msdn.microsoft.com/library/azure/dn906015.aspx) |● |○ | |6 | |
| [döntési Dzsungel](https://msdn.microsoft.com/library/azure/dn905963.aspx) |● |○ | |6 |Alacsony memóriaigény |
| [Neurális hálózat](https://msdn.microsoft.com/library/azure/dn906030.aspx) |● | | |9 |[További testreszabási lehetőség.](http://go.microsoft.com/fwlink/?LinkId=402867) |
| [egyik-v-összes](https://msdn.microsoft.com/library/azure/dn905887.aspx) |- |- |- |- |Tekintse meg a kiválasztott hello két osztályú módszer tulajdonságait |
| **Regressziós** | | | | | |
| [lineáris](https://msdn.microsoft.com/library/azure/dn905978.aspx) | |● |● |4 | |
| [Bayes-féle lineáris](https://msdn.microsoft.com/library/azure/dn906022.aspx) | |○ |● |2 | |
| [döntési erdő](https://msdn.microsoft.com/library/azure/dn905862.aspx) |● |○ | |6 | |
| [súlyozott döntési fája](https://msdn.microsoft.com/library/azure/dn905801.aspx) |● |○ | |5 |Nagy memóriaigény |
| [gyors erdő ki osztóérték](https://msdn.microsoft.com/library/azure/dn913093.aspx) |● |○ | |9 |Terjesztési pont előrejelzéseket helyett |
| [Neurális hálózat](https://msdn.microsoft.com/library/azure/dn905924.aspx) |● | | |9 |[További testreszabási lehetőség.](http://go.microsoft.com/fwlink/?LinkId=402867) |
| [A POISSON](https://msdn.microsoft.com/library/azure/dn905988.aspx) | | |● |5 |Technikailag napló lineáris. Előrejelzésére száma |
| [sorszám](https://msdn.microsoft.com/library/azure/dn906029.aspx) | | | |0 |Előrejelzésére dimenziószáma-rendezés |
| **Anomáliadetektálás** | | | | | |
| [támogatja a vektoros gép](https://msdn.microsoft.com/library/azure/dn913103.aspx) |○ |○ | |2 |Különösen hasznos nagy szolgáltatás beállítása |
| [PEM-alapú anomáliadetektálás](https://msdn.microsoft.com/library/azure/dn913102.aspx) | |○ |● |3 | |
| [K-közép](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/) | |○ |● |4 |A csoportosítási algoritmus |

**Algoritmus tulajdonságai:**

**●** -kiváló pontosságának, gyors képzési alkalommal és lineáris hello használatát jeleníti meg

**○** -jeleníti meg a helyes pontosságát és mérsékelt képzési alkalommal

## <a name="algorithm-notes"></a>Algoritmus megjegyzések
### <a name="linear-regression"></a>Lineáris regresszió
Ahogy korábban említettük [lineáris regressziós](https://msdn.microsoft.com/library/azure/dn905978.aspx) megfelelő sor (vagy vezérlősík vagy hyperplane) toohello adatkészlet. Egy workhorse, egyszerű és gyors, de lehet, hogy bizonyos problémák túlságosan simplistic.
Itt keressen egy [lineáris regressziós oktatóanyag](machine-learning-linear-regression-in-azure.md).

![Adatok lineáris trend][3]

***Adatok lineáris trend***

### <a name="logistic-regression"></a>Logisztikai regresszió
Bár a hello neve "regressziós" megtévesztően tartalmaz, logisztikai regresszió ténylegesen hatékony eszköz [két osztályú](https://msdn.microsoft.com/library/azure/dn905994.aspx) és [multiclass](https://msdn.microsoft.com/library/azure/dn905853.aspx) besorolás. Gyors és egyszerű. hello tényt, hogy használja s '-alakú görbe egyenes helyett egy természetes méretezési csoportokba adatok megosztásának teszi. Logisztikai regresszió által biztosított lineáris osztály határok, így használatánál, győződjön meg arról lineáris közelítéséről valami is él, az.

![Csak egyetlen szolgáltatás logisztikai regresszió tootwo szintű adatok][4]

***Csak egyetlen szolgáltatás egy logisztikai regresszió tootwo szintű adatok*** *-osztály határa az hello pont mely hello logisztikai csak legközelebbi tooboth osztályok*

### <a name="trees-forests-and-jungles"></a>Jungles, fák és erdők
Erdők döntési ([regressziós](https://msdn.microsoft.com/library/azure/dn905862.aspx), [két osztályú](https://msdn.microsoft.com/library/azure/dn906008.aspx), és [multiclass](https://msdn.microsoft.com/library/azure/dn906015.aspx)), jungles döntési ([két osztályú](https://msdn.microsoft.com/library/azure/dn905976.aspx) és [multiclass](https://msdn.microsoft.com/library/azure/dn905963.aspx)), és a súlyozott döntési fák ([regressziós](https://msdn.microsoft.com/library/azure/dn905801.aspx) és [két osztályú](https://msdn.microsoft.com/library/azure/dn906025.aspx)) alapulnak döntési fák eligazodást machine learning-fogalom. A döntési fák számos változata létezik, de minden tehetik az ugyanaz – hello szolgáltatás terület tovább olyan régiókban, főleg hello azonos címke. Ezek lehetnek régiók konzisztens kategória vagy állandó érték, attól függően, hogy osztályozási vagy regressziós végzi.

![Döntési fa alterületét adható meg a szolgáltatás][5]

***A döntési fa alterületét szolgáltatás adhatja többé-kevésbé egységes értékek területekre***

Szolgáltatás adhatja önkényesen kis régiók osztható meg, mert osztva finom elég toohave több adatpont régiónként könnyen tooimagine. Ez az overfitting szélsőséges példát. A sorrend tooavoid ez fák számos össze végrehajtott matematikai gondosan hello fák nem közötti kapcsolatot. hello "döntési erdőben" átlaga, amellyel elkerülhető a overfitting fát. Döntési erdők nagy mennyiségű memóriát használhat. Döntési jungles variant érték, amely akkor hello költségén a képzés némileg hosszabb ideig kevesebb memória.

Súlyozott döntési fák overfitting hányszor tovább lehet, és hogyan kevés adatpont engedélyezettek minden régióban korlátozásával elkerülése érdekében. Az algoritmus fák, sorozata, amelyek mindegyike megtanulja kiegyensúlyozása érdekében hello fa előtt által hátrahagyott hello hibát hoz létre. hello eredménye egy nagyon pontos tanuló, amely általában toouse nagy mennyiségű memóriával. Hello részletes műszaki leírásáért tekintse meg [Friedman tartozó eredeti papír](http://www-stat.stanford.edu/~jhf/ftp/trebst.pdf).

[Erdő ki osztóérték regressziós gyors](https://msdn.microsoft.com/library/azure/dn913093.aspx) hello különleges esetben, ha szeretné tudni nem csak hello tipikus (közepes) értéket egy régiót, hanem a kiosztás quantiles hello formájában hello adatainak a döntési fák mértékben eltérő változata.

### <a name="neural-networks-and-perceptrons"></a>Neurális hálózatokat és perceptrons
Neurális hálózatokat vannak agy által ihletett tanulási algoritmus kiterjedő [multiclass](https://msdn.microsoft.com/library/azure/dn906030.aspx), [két osztályú](https://msdn.microsoft.com/library/azure/dn905947.aspx), és [regressziós](https://msdn.microsoft.com/library/azure/dn905924.aspx) problémákat. Egy végtelen különböző származnak, de hello Neurális hálózatokat belül Azure Machine Learning-e minden olyan hello formája irányított aciklikus diagramjait. Ez azt jelenti, hogy bemeneti szolgáltatások továbbítja a rendszer előre (soha nem visszafelé haladva) rétegek sorozatát előtt kikapcsolta a kimenetek be. Minden egyes rétegben bemeneti adatokat különböző kombinációkban összesítve, és átadja hello következő réteg vannak súlyozott. A képes toolearn eredményez egyszerű számítási Ez a kombinációja osztály határokat és az adatok trendeket, látszólag kifinomult magic által. Az ilyen jellegű több rétegből hálózatok hello "mély tanulás" üzemanyag túl nagy reporting műszaki és tudományos hajtható végre.

A nagy teljesítményű nem tartalmaz szabad, azonban. Neurális hálózatokat is igénybe vehet egy hosszú ideig tootrain, különösen a szolgáltatások sok nagy méretű adatkészletekhez. Is rendelkeznek, mint a legtöbb algoritmusok, ami azt jelenti, hogy a paraméter abszolút bővíti hello képzési idő jelentős mértékben több paramétert.
És az adott overachievers túl kívánó[adja meg a saját hálózati struktúra](http://go.microsoft.com/fwlink/?LinkId=402867), a lehetséges a következők inexhaustible.

![Határok Neurális hálózatokat által ismert][6]
***hello határokat, amelyeket a Neurális hálózatokat összetett és szabálytalan lehet.***

Hello [két osztályú átlagosan perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx) Neurális hálózatokat válasz tooskyrocketing képzési alkalommal van. A hálózati struktúra, amely lineáris osztály határokat használja. Napjaink szabványokkal szinte egyszerű, de a abroncsnyomásmérők erős működik-e hosszú ideje, és elég kis toolearn gyorsan.

### <a name="svms"></a>SVMs
Támogatási vektoros gépek (SVMs) található, amely elválasztja az osztályok által, a lehető legszélesebb margó hello határ. Hello két osztály nem egyértelműen el kell választani, hello algoritmusok hello legjobb határ akkor is található. Mivel az Azure Machine Learning ír, hello [két osztályú SVM](https://msdn.microsoft.com/library/azure/dn905835.aspx) ezt a csak egy lineáris végzi. (A SVM beszéd használ egy lineáris kernel.) Ez biztosítja ugyanis e lineáris közelítés, akkor tudja toorun meglehetősen gyorsan. Ha a valóban helyezi van szolgáltatás-intenzív adatokkal, például szöveget vagy genomikus. Ezekben az esetekben SVMs gyorsabb és kevesebb overfitting mint a legtöbb más algoritmusok, továbbá toorequiring memória mennyisége mérsékelt képes tooseparate felosztva.

![Támogatási vektoros gép osztály határ][7]

***A szokásos támogatási vektoros gép osztály határ maximalizálja a két osztály elválasztó hello margó***

Egy másik termékkel, a Microsoft Research hello [két osztályú helyileg mély SVM](https://msdn.microsoft.com/library/azure/dn913070.aspx) nem lineáris változatát, amely megőrzi a legtöbb hello sebessége és a memória hatékonyságát hello lineáris verzió SVM van. Ideális esetben, ahol hello lineáris módszert nem biztosítják a elég pontos válaszok. hello fejlesztők megőrzi azt gyors bontásához hello probléma álló, lemezcsoport típusú kisebb lineáris SVM problémák be. Olvasási hello [teljes körű ismertetését](http://research.microsoft.com/um/people/manik/pubs/Jose13.pdf) hogyan azok lekért ki ez a trükk hello leírását.

Nem lineáris SVMs intelligens bővítményében hello [egy szintű SVM](https://msdn.microsoft.com/library/azure/dn913103.aspx) megrajzolja a határ, amely a teljes adatkészlet hello szorosan ismerteti. Akkor célszerű közüli. Bármely új adatokat sokkal adott határán kívül eső pontok elég szokatlan toobe megjegyezni.

### <a name="bayesian-methods"></a>Bayes-féle módszerek
Bayes-féle módszerek rendelkezik, célszerű lenne minőségi: azok overfitting elkerülése érdekében. Így tesznek azáltal, hogy néhány feltételezéseket előzetesen hello valószínűleg terjesztési hello válasz. Az ezt a módszert használja egy másik byproduct arról, hogy nagyon kevés paramétert. Az Azure Machine Learning rendelkezik mindkét Bayes algoritmus mindkét besorolási ([két osztályú Bayes pontozó gépet](https://msdn.microsoft.com/library/azure/dn905930.aspx)) és regressziós ([bayes-féle lineáris regressziós](https://msdn.microsoft.com/library/azure/dn906022.aspx)).
Vegye figyelembe, hogy ezek azt feltételezik, hogy az hello adatok felosztása vagy egyenes felel.

Egy korábbi megjegyzés, a Bayes' pont gépek készültek, a Microsoft Research. Néhány kivételesen gyönyörű elméleti munkahelyi hátulról rendelkeznek. hello érdekelt student irányított toohello [JMLR eredeti cikk](http://jmlr.org/papers/volume1/herbrich01a/herbrich01a.pdf) és egy [által Chris Bishop osztályon blog](http://blogs.technet.com/b/machinelearning/archive/2014/10/30/embracing-uncertainty-probabilistic-inference.aspx).

### <a name="specialized-algorithms"></a>Speciális algoritmusok
Ha a meghatározott célja esetleg számolva. Hello Azure Machine Learning gyűjtemény, belül vannak algoritmust, amelyet a szolgáltatástelepítések:

- Előrejelzés rangsorolja ([sorszám regressziós](https://msdn.microsoft.com/library/azure/dn906029.aspx)),
- előrejelzés száma ([Poisson regressziós](https://msdn.microsoft.com/library/azure/dn905988.aspx)),
- anomáliadetektálás (egy alapján [fő összetevőit elemzés](https://msdn.microsoft.com/library/azure/dn913102.aspx) és alapján egy [támogatási vektoros gép](https://msdn.microsoft.com/library/azure/dn913103.aspx)s)
- Fürtszolgáltatás ([K-közép](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/))

![PEM-alapú anomáliadetektálás][8]

***PEM-alapú anomáliadetektálás*** *-hello többsége hello adatok beleesik stereotypical terjesztési; el jelentősen eltérő, hogy a terjesztési pontok az olyan gyanús*

![Adatkészlet csoportosítja K-közép használatával][9]

***Adatkészlet szerint vannak csoportosítva K-közép-öt fürtök***

Is van a ruhaegyüttes [egyik-v-összes multiclass osztályozó](https://msdn.microsoft.com/library/azure/dn905887.aspx), mely oldaltörések hello N szintű osztályozási problémához N-1 két osztályú osztályozás problémák be. hello pontosságának, a képzési időt és a lineáris tulajdonságok hello két osztályú osztályozó használt határozza meg.

![Két osztályú osztályozó kombinált tooform három-osztály besorolás][10]

***Két osztályú osztályozó két egyesítése tooform három-osztály besorolás***

Az Azure Machine Learning is hozzáférést tooa hatékony gépi tanulás keretrendszer hello címe alatt [Vowpal Wabbit](https://msdn.microsoft.com/library/azure/8383eb49-c0a3-45db-95c8-eb56a1fef5bf).
VW szakmai kategorizálási itt, mert besorolás és a regressziós problémák megtudhatja, és akkor is megtudhatja, részben címke nélküli adatokból. Beállíthatja, toouse tanulási algoritmusok, adatvesztés funkciók és optimalizálási algoritmusok számos valamelyikét. A hatékony, párhuzamos és nagyon gyorsan toobe mentése szabad hello tervezték. Kevés a nyilvánvaló munkát ridiculously nagy szolgáltatáskészletek kezeli.
Megkezdődött, és a Microsoft Research által saját John Langford által vezetett, VW bejegyzés egy képletet egy készlet car algoritmusok mezőben. Nem minden probléma megfelelő VW, de ha saját, akkor érdemes lehet a tanulási görbére illesztőjén tooclimb. Azt is rendelkezésre áll, mint a [önálló nyílt forráskódú kód](https://github.com/JohnLangford/vowpal_wabbit) több nyelven is.

## <a name="more-help-with-algorithms"></a>További segítséget itt találhat algoritmusok
* Letölthető infographic, amely leírja a algoritmusok, és a példákat mutat be, lásd: [letölthető Infographic: gépi tanulási algoritmus példákkal alapjai](machine-learning-basics-infographic-with-algorithm-examples.md).
* Kategória az Azure Machine Learning Studióban elérhető összes hello gépi tanulási algoritmusok listájáért lásd: [inicializálása modell] [ initialize-model] hello Machine Learning Studio algoritmus és a modul segítségével.
* Teljes betűrendben listáját az algoritmusok és a modulok az Azure Machine Learning Studióban, tekintse meg a [a Machine Learning Studio moduljai A-Z listában] [ a-z-list] a Machine Learning Studio algoritmus és a modul segítségével.
* Lásd a toodownload és nyomtatási ábrázoló diagram, amely áttekintést nyújt az Azure Machine Learning Studio képességeinek hello [Azure Machine Learning Studio képességeit áttekintő diagram](machine-learning-studio-overview-diagram.md).


<!-- Reference links -->
[initialize-model]: https://msdn.microsoft.com/library/azure/dn905812.aspx
[a-z-list]: https://msdn.microsoft.com/library/azure/dn906033.aspx

<!-- Media -->

[1]: ./media/machine-learning-algorithm-choice/image1.png
[2]: ./media/machine-learning-algorithm-choice/image2.png
[3]: ./media/machine-learning-algorithm-choice/image3.png
[4]: ./media/machine-learning-algorithm-choice/image4.png
[5]: ./media/machine-learning-algorithm-choice/image5.png
[6]: ./media/machine-learning-algorithm-choice/image6.png
[7]: ./media/machine-learning-algorithm-choice/image7.png
[8]: ./media/machine-learning-algorithm-choice/image8.png
[9]: ./media/machine-learning-algorithm-choice/image9.png
[10]: ./media/machine-learning-algorithm-choice/image10.png
