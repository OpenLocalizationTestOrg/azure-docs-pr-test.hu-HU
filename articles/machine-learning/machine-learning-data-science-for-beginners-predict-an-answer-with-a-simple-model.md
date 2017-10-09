---
title: "egyszerű regressziós modell - Azure Machine Learning választ aaaPredict |} Microsoft Docs"
description: "Hogyan toocreate egy egyszerű regressziós modell toopredict Adattudomány videó 4 kezdőknek ár. Egy lineáris regressziós cél adatokat tartalmazza."
keywords: "a modell, egyszerű modell, árának előrejelzése, egyszerű regressziós modell létrehozása"
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: a28f1fab-e2d8-4663-aa7d-ca3530c8b525
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: cgronlun
ms.openlocfilehash: d4270c2237c33b7e898b78a08b292bc9d62e49ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="predict-an-answer-with-a-simple-model"></a>Válasz előrejelzése egy egyszerű modell segítségével
## <a name="video-4-data-science-for-beginners-series"></a>4. Videó: Adattudomány kezdők sorozat
Ismerje meg, hogyan toocreate egy egyszerű regressziós modell toopredict hello egy rombusz a videó 4 kezdőknek Adattudomány árát. Azt fogja megrajzolásához egy regressziós modell cél adatokkal.

tooget hello hatékonyabb működtetését hello adatsorozat, tekintse meg azokat. [Nyissa meg a videók toohello listája](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/data-science-for-beginners-series-predict-an-answer-with-a-simple-model/player]
>
>

## <a name="other-videos-in-this-series"></a>A sorozat többi videók
*Adattudomány kezdőknek* van egy gyors bevezetés toodata tudományos az öt rövid videók.

* 1. Videó: [hello 5 kérdésekre adatok tudományos válaszok](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 perc 14 másodperc)*
* 2. Videó: [adattudomány készen áll az adatok?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 perc 56 másodperc)*
* 3. Videó: [tegyen fel kérdést a válasz adatokkal](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 perc 17 másodperc)*
* 4. Videó: Egyszerű modell választ előrejelzése
* 5. Videó: [mások munkahelyi toodo adattudomány másolása](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 perc 18 másodperc)*

## <a name="transcript-predict-an-answer-with-a-simple-model"></a>A Beszélgetés szövegének: Egyszerű modell választ előrejelzése
Üdvözli a negyedik videó hello "Adatok tudományos a kezdők" Adatsorozat toohello. Ezt a telepítést azt bemutatjuk egy egyszerű modell létrehozása, ellenőrizze az előrejelzéshez.

A *modell* egy egyszerűsített szövegegység az adatok körül forog. I fogjuk I jelenti.

## <a name="collect-relevant-accurate-connected-enough-data"></a>Megfelelő, a pontos, csatlakoztatott, elég adatok gyűjtése
Tegyük fel például, egy rombusz kívánt tooshop. Az egy 1.35 beszúrási rombusz beállítással nőivarú toomy nagyszülő tartozó gyűrű van, és tooget egy meghatározni, hogy a rendszer mennyibe kívánt. Szeretnék egy Jegyzettömb és toll hello ékszereket tárolóba, és szeretnék írja le az összes hello rombusz – hello esetben és akkor karát mennyi mérjük hello ár. Hello első rombusz - fennállt 1.01 karát és $7,366 kezdve.

Most keresztül halad, és tegye meg ezt az összes hello más rombusz – hello tároló.

![Oszlopok rombusz adatok](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/diamond-data.png)

Figyelje meg, hogy a kiválasztott két oszlopot tartalmaz. Mindegyik oszlopnak van egy másik attribútum - a súlyozás karát és ár - és minden egyes sorára egyetlen rombusz jelölő egyetlen adatpont.

Ténylegesen létrehoztunk Önnek egy kis adatkészlet ide - tábla. Figyelje meg, hogy megfelel-e a minőségi feltételek:

* hello adatok **vonatkozó** -súly mindenképpen kapcsolódó tooprice
* Rendelkezik **pontos** -azt ellenőrizni hello árak, amelyek azt írja le
* Rendelkezik **csatlakoztatott** -nincsenek ezeket az oszlopokat az üres szóközök nélkül
* És mivel megtanulhatja, rendelkezik **elég** adatok tooanswer a kérdés

## <a name="ask-a-sharp-question"></a>Éles kérdés
Most azt fogja jelent a kérdés éles úgy: "mennyi lesz költség toobuy egy 1.35 beszúrási rombusz?"

A lista nem rendelkezik egy 1.35 beszúrási rombusz, hogy lesz toouse hello többi részének az adatok tooget egy válasz toohello kérdést.

## <a name="plot-hello-existing-data"></a>Hello meglévő adatok ábrázolása
hello elsőként azt kell végeznie az rajzolása vízszintes számát, nevű tengely toochart hello súlyt. hello hello súlyok tartománya 0 too2, így azt fogja rajzolása, amely hozzá van rendelve, amely a tartomány és az egyes fele beszúrási ticks helyezze.

Lesz ezután azt egy függőleges tengely toorecord hello ár megrajzolásához, és csatlakoztassa toohello súly vízszintes tengely. Ezzel egység dolláros lesz. Most van koordináta tengelyek készlete.

![Súly és az ár](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/weight-and-price-axes.png)

A Microsoft következővel folyamatos tootake ezeket az adatokat, és kapcsolja be a *pont rajzot*. Ez a kiváló módja toovisualize numerikus adatkészletek.

Az első adatpont hello azt eyeball 1.01 karát mérve. Ezt követően: $7,366 vízszintes vonal eyeball azt. Megfelelnek-e, ha azt egy pont megrajzolásához. Az első rombusz jelképez.

Most azt halad át minden rombusz ezen a listán, és hello ugyanaz. A rendszer keresztül, ha ez az azt lekérése: pontokból álló, lemezcsoport, minden egyes rombusz egyet.

![Pontdiagram rajzot](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/scatter-plot.png)

## <a name="draw-hello-model-through-hello-data-points"></a>Hello modell hello adatpontokra megrajzolásához
Most hello pontok és squint tekinti meg, ha hello gyűjtemény néz fat, intelligens sor. Azt a jelölő igénybe, és egy egyenes vonalat rajta.

Egy sor rajzolási, által létrehozott egy *modell*. Használjon hello valós véve, és azt simplistic Rajzfilmes verziójának. Most hello Rajzfilmes megfelelő – hello sor összes hello adatpontok nem használható. Ez azonban hasznos egyszerűsítését.

![Lineáris regressziós egyenes](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/linear-regression-line.png)

hello tényt, hogy minden hello pontokból nem végighaladnia pontosan hello sor az OK gombra. Adatszakértőkön azt ismertetik, ennek kimondásával, hogy van-e hello modell -, amely hello sor - és minden pont van néhány *zaj* vagy *variancia* társítva. Az alapul szolgáló tökéletes kapcsolat hello van, és nincs majd hello kövecses, valós globális zaj és bizonytalanság hozzáadó.

Mivel tooanswer hello kérdés próbált *mennyi?* ezt nevezik a *regressziós*. Mivel használunk egyenes, és van egy *lineáris regressziós*.

## <a name="use-hello-model-toofind-hello-answer"></a>Hello modell toofind hello válaszfájl használata
A modell van, és megkérjük, a kérdés: mennyi lesz a 1.35 beszúrási rombusz költség?

tooanswer a kérdést, szem 1.35 karát azt és egy függőleges vonalat. Az áthalad hello modell sor, ha azt egy vízszintes vonal toohello dollár tengely eyeball. Találatok száma a jobb: 10 000-re. Elterjedése! Ez az hello válasz: egy 1.35 beszúrási rombusz körülbelül 10 000 $ költségek.

![Található hello válasz hello modell](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/find-the-answer.png)

## <a name="create-a-confidence-interval"></a>Hozzon létre egy megbízhatósági intervallum
Hogyan pontos a előrejelzése természetes toowonder. Hasznos tooknow e hello 1.35 beszúrási rombusz túl rendkívül szoros lesznek $ 10 000-re vagy magasabb vagy alacsonyabb nagy. toofigure a, most körül, amely tartalmazza a legtöbb hello pontokból hello regressziós egyenes boríték megrajzolásához. A boríték nevezik a *megbízhatósági*: a rendszer közérthető bizonyos abban, hogy az árak esik-e a boríték, mert az elmúlt hello többsége a őket. Ha hello 1.35 beszúrási sor áthalad hello teteje és alja hello borítékot azt is tárolt két vízszintesen több sort.

![Megbízhatósági intervallum](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/confidence-interval.png)

Most azt is ismertetés az vetett bizalmat időköz: magabiztosan azt mondani, hogy egy 1.35 beszúrási rombusz hello ára készül $ 10 000 -, de lehet akár csupán $8000 és lehet, mint $ 12 000.

## <a name="were-done-with-no-math-or-computers"></a>A Microsoft nem használja, nem matematikai vagy számítógépek
Azt adta milyen adatszakértőkön beolvasása fizetett toodo, és azt adta azt csak fel:

* Azt kéri, a kérdés, hogy azt az választ sikerült adatokkal
* Azt a beépített egy *modell* használatával *lineáris regressziós*
* A Microsoft tett egy *előrejelzés*, kész, de egy *megbízhatósági intervallum*

Matematikai vagy számítógépek toodo nem használjuk, és azt.

Ha további információt a Microsoft volt, mostantól például...

* hello hello rombusz a Kivágás
* szín változata (milyen pontossággal hello rombusz fehér toobeing)
* a hello rombusz befoglalások hello száma

.. helyőrzőkre azt kellett volna több oszlopot. Ebben az esetben a matematikai lesz hasznos. Ha több mint két oszlop, papíron rögzített toodraw pont. hello matematikai lehetővé teszi, hogy a sor vagy vezérlősík tooyour adatok nagyon szépen.

Is ha helyett csak néhány olyan gyémántok, le kellett két példányban vagy két millió, majd munka teheti sokkal gyorsabb, a számítógép.

Napjainkban túlmutatnak tárgyalt hogyan toodo lineáris regresszióját, és azt szeretné az adatok előrejelzéshez tenni.

Hello ki, hogy toocheck kell az "Adatok tudományos a kezdők" a Microsoft Azure Machine Learning más videók.

## <a name="next-steps"></a>Következő lépések
* [Próbálja meg egy első adatok tudományos kísérletben a Machine Learning Studio](machine-learning-create-experiment.md)
* [Egy bevezető tooMachine beszerzése a Microsoft Azure tanulási](machine-learning-what-is-machine-learning.md)
