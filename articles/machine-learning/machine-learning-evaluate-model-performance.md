---
title: "a Machine Learning aaaEvaluate modell teljesítmény |} Microsoft Docs"
description: "Ismerteti, hogyan tooevaluate modell Azure Machine Learning teljesítményét."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 5dc5348a-4488-4536-99eb-ff105be9b160
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: bradsev;garye
ms.openlocfilehash: 03477368758dbb13aa6f54c5d27fb215615d1f9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooevaluate-model-performance-in-azure-machine-learning"></a>Hogyan tooevaluate modell-teljesítmény az Azure gépi tanulás
Ez a cikk bemutatja, hogyan tooevaluate hello az Azure Machine Learning Studióban modell teljesítményétől, a rövid ismerteti hello voltak elérhetők metrikák ehhez a feladathoz. Felügyelt tanítás során három gyakori forgatókönyveket mutatnak be: 

* Regressziós
* bináris osztályozás 
* multiclass besorolás

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Hello teljesítmény-modell kiértékelése az egyik hello core szakaszból hello adatok tudományos folyamat során. Azt jelzi, hogy hogyan sikeres hello (előrejelzés) pontozási a DataSet adatkészlet már egy betanított modell. 

Az Azure Machine Learning modell kiértékelése két a fő machine learning modulok keresztül támogatja: [modell kiértékelése] [ evaluate-model] és [kereszt modell][cross-validate-model]. Ezek a modulok lehetővé teszik toosee hogyan hajtja végre a modell számos általánosan használt gépi tanulási és a statisztika metrikák tekintetében.

## <a name="evaluation-vs-cross-validation"></a>Kiértékelési vs. Kereszt-ellenőrzési
Kiértékelési és keresztellenőrzési a szokásos módon toomeasure hello a modell teljesítményétől. Vizsgálja meg, vagy más modellek az összehasonlításhoz értékelési metrikák mindkettő készítése.

[Értékelje ki a modell] [ evaluate-model] pontozott dataset bemeneti (vagy 2 2 különböző modell teljesítményétől toocompare hello milyen esetekben) vár. Ez azt jelenti, hogy szüksége tootrain a modellben hello [tanítási modell] [ train-model] néhány adathalmaz hello segítségével a modul, és később előrejelzéseket [Score Model] [ score-model] modul, mielőtt kiértékelheti az hello eredmények. hello értékelési alapuló címkék/valószínűség együtt hello igaz címkéket, amelyek mindegyike által hello pontozni hello [Score Model] [ score-model] modul.

Másik lehetőségként keresztellenőrzési tooperform a vonat pontszám kiértékelése műveletek száma (10 modellrészt) automatikusan különböző alkészletek hello bemeneti adatokat is használhatja. hello bemeneti adatok 10 részre, ahol egy tesztelési számára fenntartva, és más 9 képzési hello van osztva. Ez a folyamat ismétlődik 10-szer és hello értékelési-metrikák program átlagolja. Ez segít meghatározni, hogy mennyire modell volna generalize toonew adatkészletek. Hello [kereszt modell] [ cross-validate-model] modul fogad az kellő modell és néhány feliratú dataset és -kimenetek hello kiértékelésének eredménye az egyes hello 10 modellrészt, továbbá toohello átlagosan eredmények.

A következő részekben hello, rendszer egyszerű regressziós és besorolási modellek létrehozása és alkalmazásproblémák teljesítményét, mindkét hello [modell kiértékelése] [ evaluate-model] és hello [kereszt Modell] [ cross-validate-model] modulok.

## <a name="evaluating-a-regression-model"></a>Egy regressziós modell kiértékelése
Tegyük fel, azt szeretnénk, ha toopredict egy autó árának az egyes szolgáltatások, például a dimenziók, lóerő, motor specifikációk és így tovább. A tipikus regressziós probléma, ha a célváltozót hello (*ár*) egy folyamatos numerikus érték. Egy egyszerű lineáris regressziós modellt, hogy hello szolgáltatást a megadott értékek egy bizonyos autó is előre jelezni, hogy autó árának hello fér azt. A regressziós modell használható tooscore hello azt képezni a DataSet adatkészlethez. Ha igazolnia kell hello összes hello autók előre jelzett árak, azt meg tudja határozni, hello teljesítmény hello modell mennyire hello előrejelzéseket eltérnek hello a tényleges díjak átlagosan megtekintésével. tooillustrate, hello használjuk *Automobile price data (Raw) dataset* hello elérhető **mentett adatkészletek** szakaszban az Azure Machine Learning Studióban.

### <a name="creating-hello-experiment"></a>Hello kísérlet létrehozása
Adja hozzá a következő modulok tooyour munkaterület az Azure Machine Learning Studióban hello:

* Autó price data (Raw)
* [Lineáris regressziós][linear-regression]
* [Tanítási modell][train-model]
* [Pontszám modell][score-model]
* [Modell kiértékelése][evaluate-model]

Csatlakozás hello portok, ahogy alább az 1. ábra és a set hello címke oszlopa hello [tanítási modell] [ train-model] modul túl*ár*.

![Egy regressziós modell kiértékelése](media/machine-learning-evaluate-model-performance/1.png)

1. ábra. Egy regressziós modell kiértékelése.

### <a name="inspecting-hello-evaluation-results"></a>Kérem hello kiértékelésének eredménye
Futó hello kísérletet követően kattinthat a hello kimeneti portjára hello [modell kiértékelése] [ evaluate-model] modul, és válassza ki *Visualize* toosee hello kiértékelésének eredménye. hello érhető el értékelési-metrikák a regressziós modellek a következők: *Mean Absolute Error*, *Root Mean Absolute Error*, *Relative Absolute Error*,  *Relatív négyzet hiba*, és hello *Coefficient of Determination*.

hello kifejezés "error" Itt jelöli hello különbségének hello előre jelzett érték és hello igaz értéket. hello abszolút értéke vagy a különbség négyzetes hello általában számított toocapture hello teljes hello hello különbségének minden példányára, hibát nagysága előre jelezni, és IGAZ érték negatív, egyes esetekben lehet. hello hiba metrikák hello prediktív teljesítmény szempontjából hello közepét eltérése a előrejelzéseket hello valódi értékek regressziós modell mérését. Alacsonyabb hibaértékek jelenti azt hello modell pontosabb az előrejelzés során. Egy általános hiba a metrika a 0 azt jelenti, hogy a modell hello hello adatok tökéletesen megfelel.

hello együttható, amely más néven R-négyzet, mérési mennyire illik legjobban a hello modell hello adatok szabványos módja is. Is értelmezhető hello modell magyarázza variation hello részét. Egy nagyobb arányban célszerűbb ebben az esetben, ahol az 1 azt jelzi, tökéletes.

![Lineáris regressziós értékelési-metrikák](media/machine-learning-evaluate-model-performance/2.png)

2. ábra Lineáris regressziós értékelési-metrikák.

### <a name="using-cross-validation"></a>Közötti érvényesítése
A korábban említett ismételt képzési végezheti el, pontozási és értékelések használatával automatikusan hello [kereszt modell] [ cross-validate-model] modul. Ebben az esetben szüksége a DataSet adatkészlet olyan kellő modellt, és egy [kereszt modell] [ cross-validate-model] modul (lásd az alábbi ábrát). Megjegyzés: tooset hello címke oszlop túl kell*ár* a hello [kereszt modell] [ cross-validate-model] modulhoz tartozó tulajdonságok.

![Egy regressziós modell kereszt-ellenőrzése](media/machine-learning-evaluate-model-performance/3.png)

3. ábra. Kereszt-ellenőrzése egy regressziós modellt.

Futó hello kísérletet követően vizsgálhatja hello kiértékelésének eredménye kattintson a jobb oldali kimeneti portjára hello hello [kereszt modell] [ cross-validate-model] modul. Ezzel a funkcióval hello metrikák részletes nézete törzsének (modellészből), és hello átlagosan eredményei az hello metrikák (4. ábra).

![Kereszt-ellenőrzési eredmények regressziós modell](media/machine-learning-evaluate-model-performance/4.png)

4. ábra. Kereszt-ellenőrzési eredmények regressziós modell.

## <a name="evaluating-a-binary-classification-model"></a>A bináris osztályozási modell kiértékelése
A bináris osztályozási forgatókönyvek hello célváltozó rendelkezik csak két eredményekkel, például: {0, 1} vagy {hamis, igaz}, {negatív, pozitív}. Tegyük fel, lehetősége van a DataSet adatkészlet egyes felnőtt az alkalmazottak demográfiai és alkalmazási változók, és hogy megadnia toopredict hello bevétel szintjén, a bináris változó hello értékekkel {"< 50K =", "> 50-K"}. Más szóval hello negatív osztály jelöli hello alkalmazottak, akik kisebb vagy egyenlő, mint / év, és pozitív osztály jelenti. minden más alkalmazott hello too50K. Mint hello regressziós forgatókönyv esetén azt volna a modell betanításához, bizonyos adatok pontozása és hello eredmények értékelése. hello fő különbség itt az Azure Machine Learning kiszámítja metrikák és kimenetek hello választott. tooillustrate hello bevétel szintű előrejelzés forgatókönyv használjuk hello [felnőtt](http://archive.ics.uci.edu/ml/datasets/Adult) dataset toocreate az Azure Machine Learning kísérletek kifejlesztéséhez, és értékelje hello teljesítményét a két osztályú logisztikai regresszió egy gyakran használt bináris modell besorolás.

### <a name="creating-hello-experiment"></a>Hello kísérlet létrehozása
Adja hozzá a következő modulok tooyour munkaterület az Azure Machine Learning Studióban hello:

* Felnőtt nyilvántartásba bevétel bináris osztályozási adatkészlet
* [Két osztályú logisztikai regresszió][two-class-logistic-regression]
* [Tanítási modell][train-model]
* [Pontszám modell][score-model]
* [Modell kiértékelése][evaluate-model]

Csatlakozás hello portok, 5. ábra és hello címke oszlopkészlet hello az alábbiak szerint [tanítási modell] [ train-model] modul túl*bevétel*.

![A bináris osztályozási modell kiértékelése](media/machine-learning-evaluate-model-performance/5.png)

5. ábra. A bináris osztályozási modell kiértékelése.

### <a name="inspecting-hello-evaluation-results"></a>Kérem hello kiértékelésének eredménye
Futó hello kísérletet követően kattinthat a hello kimeneti portjára hello [modell kiértékelése] [ evaluate-model] modul, és válassza ki *Visualize* toosee hello kiértékelésének eredménye (a 7. ábrán). kiértékelési voltak elérhetők metrikák hello a bináris osztályozási modellek a következők: *pontossága*, *pontosság*, *visszahívása*, *F1 pontszám*, és *AUC*. Ezenkívül hello modul kimenete egy zavart mátrix igaz pozitív, téves negatív eredmények, a vakriasztások és true negatív hello számát, valamint *ROC*, *pontosság/visszaírási*, és *Növekedési* görbék.

Pontosság megfelelően minősített példányok egyszerűen hello hányadát. Akkor általában hello első metrika megnézzük besorolás kiértékelése során. Azonban ha hello Tesztadatok van tette (ahol a legtöbb hello példányok hello osztályok tooone tartozik), vagy több érdekli hello osztályok bármelyikét hello teljesítményére, a pontosság valóban nem rögzíti egy osztályozó hello hatékonyságát. Hello bevétel szintű besorolást esetben azt feltételezik, néhány adat, ahol 99 % hello példányok képviselő személyeket, akik kisebb vagy egyenlő, mint a tesztelni kívánt too50K évente. Már lehetséges tooachieve 0.99 pontossága előrejelzésére hello osztály által "< = 50-K" minden példány esetében. hello osztályozó ebben az esetben ez egy általános jó feladat toobe jelenik meg, de valójában, az nem tooclassify bármelyik hello nagyjövedelmű egyéni felhasználók számára (hello 1 %) megfelelően.

Éppen ezért hasznos toocompute további metrikák hello értékelési pontosabb aspektusainak rögzítő. Mielőtt a felhasználó ilyen metrikák részleteinek hello, fontos toounderstand hello zavart mátrix a bináris osztályozási értékeléskor. a hello gyakorlókészlethez címkék csak 2 lehetséges értékei, mely azt általában vehet igénybe hello osztály tekintse meg a tooas pozitív vagy negatív. hello pozitív és negatív példányok, amely egy osztályozó előrejelzi megfelelően nevezzük igaz figyelmeztetéséket (TP), és IGAZ negatív (TN), illetve. Hasonlóképpen hibásan besorolt hello példányok téves (pi) és a téves negatív eredmények (FN) nevezik. hello zavart mátrix egyszerűen minden ilyen 4 alá tartozó-példányok száma hello megjelenítő táblázat. Az Azure Machine Learning automatikusan eldönti, amely két osztály hello hello adatkészlet hello pozitív osztály. Ha hello osztály címkék logikai érték vagy egész számok, akkor hello "true" vagy "1" címkével ellátott példányok hello pozitív osztály vannak hozzárendelve. Ha hello címkék karakterláncok, mint hello hello bevétel dataset, hello címkék vannak elemei ábécésorrendben vannak, és hello első szintjét toobe hello negatív osztály van kiválasztva, amíg hello második szintű hello pozitív osztály.

![Bináris osztályozási zavart mátrix](media/machine-learning-evaluate-model-performance/6a.png)

6. ábra. Bináris osztályozási zavart mátrix.

Ha visszalép toohello bevétel osztályozási problémához, akkor szeretnénk tooask számos értékelési kérdést, hogy segítsen megérteni használt hello besorolás hello teljesítményét. Egy nagyon természetes kérdés: "hello egyéni felhasználók számára, akinek hello kívül modell az előre jelzett toobe megszerzéséhez > 50-K (TP + pi), hogy hány sorolták megfelelően (TP)?" Ez a kérdés is válaszolhatók hello megtekintésével **pontosság** hello modell, amely pozitív megfelelően besorolt hello aránya: TP/(TP+FP). Egy másik gyakori kérdést "hello magas kamatlábairól rendelkező alkalmazottakra bevétel kívül > 50-k (TP + FN), hány osztályozó volt hello besorolása megfelelően (TP)". Ez a ténylegesen hello **visszahívása**, vagy hello igaz pozitív aránya: hello osztályozó TP/(TP+FN). Bizonyára észrevette, hogy, hogy van-e egy nyilvánvaló kompromisszum pontosság és a visszaírási között. Például viszonylag kiegyensúlyozott dataset tekintve, amelyek többnyire pozitív példányok előrejelzi besorolás magas visszaírási kellene lennie, de számos téves eredményezve hello negatív példányok közül a ahelyett, hogy alacsony pontossága volna misclassified. Hogyan két metrikákat eltérő ábrázolása toosee, rákattinthat a hello "PONTOSSÁG/VISSZAÍRÁSI" görbe hello kiértékelési eredménye kimeneti lapon (a 7. ábra részét bal felső).

![Bináris osztályozási kiértékelésének eredménye](media/machine-learning-evaluate-model-performance/7.png)

7. ábra. Bináris osztályozási kiértékelésének eredménye.

Egy másik kapcsolatos gyakran használt metrika hello **F1 pontszám**, amely a pontosság és a visszaírási figyelembe veszi. Hello harmonikus középértéke 2 metrikákat, és mint ilyen számított: F1 = 2 (pontosság x visszaírási) / (pontosság + visszaírási). hello F1 pontszám egy jó módszer toosummarize hello értékelési egyetlen számos, de a rendszer mindig egy célszerű toolook pontosság és visszaírási együtt toobetter megérteni, hogyan viselkedik a besorolás.

Emellett egy vizsgálhatja meg hello igaz pozitív sebesség és a téves pozitív sebességet hello hello **fogadó működő jellemző (ROC)** görbét és megfelelő hello **terület szerint hello görbe (AUC)** érték. hello közelebb a görbe toohello felső sarokban balra, a hello jobb hello osztályozó teljesítmény (Ez azt jelenti maximalizálva hello igaz pozitív arány ugyanakkor minimalizálja a téves pozitív arány hello). Görbék, amelyek a Bezárás toohello átlós hello rajzot, toomake előrejelzéseket, amelyek az egyes osztályozó kapott eredmény zárja be az toorandom találgatás.

### <a name="using-cross-validation"></a>Közötti érvényesítése
Hello regressziós példa, ahogy azt végrehajtása keresztellenőrzési toorepeatedly vonat, pontozása és hello adatok különböző részhalmazai automatikusan kiértékeléséhez. Ehhez hasonlóan használhatjuk hello [kereszt modell] [ cross-validate-model] modul, egy kellő logisztikai regresszió modell és a DataSet adatkészlet. hello címke oszlop túl be kell állítani*bevétel* a hello [kereszt modell] [ cross-validate-model] modulhoz tartozó tulajdonságok. Miután hello kísérlet fut, és kattintson a jobb oldali hello kimeneti portjára hello [kereszt modell] [ cross-validate-model] láthatja modul, hello bináris osztályozási metrika értékei minden modellrészek, továbbá toohello és az egyes szórásnál. 

![A bináris osztályozási modellek közötti érvényesítése](media/machine-learning-evaluate-model-performance/8.png)

8. ábra. A bináris osztályozási modellek közötti ellenőrzése.

![Egy bináris osztályozó kereszt-ellenőrzési eredményeit](media/machine-learning-evaluate-model-performance/9.png)

9. ábra. Egy bináris osztályozó kereszt-ellenőrzési eredményeit.

## <a name="evaluating-a-multiclass-classification-model"></a>A Multiclass besorolási modell kiértékelése
A kísérletben használjuk népszerű hello [Iris](http://archive.ics.uci.edu/ml/datasets/Iris "Iris") 3 különböző típusú (osztály) hello iris gépek példányait tartalmazó adathalmaz. 4 szolgáltatás értékek vannak (sepal hossza/szélesség és hosszúság/szélesség szirom) minden példány esetében. Hello előző kísérletekben azt betanítása és tesztelt hello modellek segítségével hello azonos adatkészletek. Itt használjuk hello [Split Data] [ split] hello adatok részhalmaza modul toocreate 2 betanítása a hello először, és pontozni és értékelje ki a hello második. hello Iris adatkészlet érhető el nyilvánosan hello [UCI Machine Learning tárház](http://archive.ics.uci.edu/ml/index.html), és segítségével letölthető egy [adatok importálása] [ import-data] modul.

### <a name="creating-hello-experiment"></a>Hello kísérlet létrehozása
Adja hozzá a következő modulok tooyour munkaterület az Azure Machine Learning Studióban hello:

* [Adatok importálása][import-data]
* [Multiclass döntési erdő][multiclass-decision-forest]
* [Adatok][split]
* [Tanítási modell][train-model]
* [Pontszám modell][score-model]
* [Modell kiértékelése][evaluate-model]

Csatlakozás hello portok, a 10. ábra az alább látható módon.

Hello címke oszlop indexe hello beállítása [tanítási modell] [ train-model] modul too5. hello dataset adatkészletben nem fejlécsor, de tudjuk, hogy hello osztály feliratai hello ötödik oszlopban.

Kattintson a hello [és adatokat importálhat] [ import-data] modul és a set hello *adatforrás* tulajdonság túl*webes URL-Címéhez HTTP Protokollon keresztül*, és hello *URL-címe*  toohttp://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data.

Set hello azon részét, hello képzési használt példányok toobe [Split Data] [ split] modul (például 0,7).

![A Multiclass osztályozó kiértékelése](media/machine-learning-evaluate-model-performance/10.png)

10. ábra. A Multiclass osztályozó kiértékelése

### <a name="inspecting-hello-evaluation-results"></a>Kérem hello kiértékelésének eredménye
Hello kísérlet futtatása, majd kattintson a kimeneti portjára hello [modell kiértékelése][evaluate-model]. hello értékelési eredmények jelenjenek meg a félreértések mátrix hello űrlap ebben az esetben. hello mátrix tartalmazza a hello előre jelzett összes 3 osztály példányainak tényleges.

![Multiclass besorolási kiértékelésének eredménye](media/machine-learning-evaluate-model-performance/11.png)

11. ábra. Multiclass besorolási kiértékelésének eredménye.

### <a name="using-cross-validation"></a>Közötti érvényesítése
A korábban említett ismételt képzési végezheti el, pontozási és értékelések használatával automatikusan hello [kereszt modell] [ cross-validate-model] modul. A DataSet adatkészlet olyan kellő modellt kell és egy [kereszt modell] [ cross-validate-model] modul (lásd az alábbi ábrát). Újra kell-e tooset hello címke oszlopa hello [kereszt modell] [ cross-validate-model] modul (oszlopindex 5 ebben az esetben). Miután hello kísérlet fut, és kattintson a jobb hello kimeneti portjára hello [kereszt modell][cross-validate-model], hello metrika értékek vizsgálhatja meg az egyes részekre bontani, valamint és szórásnál hello. Itt látható hello adatok gyűjtése le hello hasonló toohello hello bináris osztályozási eset leírtaknak megfelelően. Vegye figyelembe azonban, hogy multiclass besorolásban számítási hello igaz pozitív vagy negatív, és a téves pozitív vagy negatív eredmények végezhető el egy osztály alapon számbavételi, nincs átfogó pozitív vagy negatív osztály. Például ha számítógépes hello pontosság vagy hello "Iris-setosa" osztály oda, feltételezzük, hogy ez az pozitív osztály hello és minden más negatív.

![A Multiclass besorolási modell kereszt-ellenőrzése](media/machine-learning-evaluate-model-performance/12.png)

12. ábra. Kereszt-ellenőrzése egy Multiclass besorolási modell.

![Kereszt-ellenőrzési eredmények Multiclass besorolási modell](media/machine-learning-evaluate-model-performance/13.png)

13. ábra. Kereszt-ellenőrzési eredmények Multiclass besorolási modell.

<!-- Module References -->
[cross-validate-model]: https://msdn.microsoft.com/library/azure/75fb875d-6b86-4d46-8bcc-74261ade5826/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[multiclass-decision-forest]: https://msdn.microsoft.com/library/azure/5e70108d-2e44-45d9-86e8-94f37c68fe86/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-logistic-regression]: https://msdn.microsoft.com/library/azure/b0fd7660-eeed-43c5-9487-20d9cc79ed5d/

