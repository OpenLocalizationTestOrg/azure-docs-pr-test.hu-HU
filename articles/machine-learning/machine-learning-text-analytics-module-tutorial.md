---
title: "aaaCreate szöveg elemzési modelljei az Azure Machine Learning Studióban |} Microsoft Docs"
description: "Hogyan toocreate szövegelemzések modellek az Azure Machine Learning Studio a szöveg előfeldolgozása, N-g vagy a szolgáltatáskivonatolás modulok használata"
services: machine-learning
documentationcenter: 
author: rastala
manager: jhubbard
editor: 
ms.assetid: 08cd6723-3ae6-4e99-a924-e650942e461b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: roastala
ms.openlocfilehash: e3799f37ba54bb2ec8815ecf5ed34e145ffb20e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-text-analytics-models-in-azure-machine-learning-studio"></a>Szövegelemzési modellek létrehozása az Azure Machine Learning Studio használatával
Azure Machine Learning toobuild használja, és azok szöveg elemzési modelljeit. Ezek a modellek segítségével, például dokumentum besorolását vagy a céggel kapcsolatos véleményeket elemzés előforduló problémák megoldásához.

Az a szöveg elemzési kísérletet üzembe helyezése rendszerint:

1. Tiszta és a szöveg dataset előfeldolgozása
2. Bontsa ki a numerikus szolgáltatás vektorok Előfeldolgozott szövegből
3. Tanítási osztályozási vagy regressziós modell
4. Pontszám és hello modell ellenőrzése
5. Hello modell tooproduction telepítése

Ebben az oktatóanyagban elsajátíthatja, ezeket a lépéseket, azt végezze el a céggel kapcsolatos véleményeket elemzési modellek Amazon könyv értékelést adatkészlet (lásd a tanulmány "adatainak, Bollywood, elterjedése-Box és Blenders: tartomány kiigazítása véleményeket besorolási" John Blitzer, be van jelölve Dredze és Fernando Pereira; Társítás számítási Linguistics (ACL), a 2007.) Ez az adatkészlet felülvizsgálati pontszámok (1-2 vagy 4-5) és a szabad formátumú szöveget áll. hello célja toopredict hello felülvizsgálati pontszám: alacsony (1 - 2) vagy nagy (4-5).

Ebben az oktatóanyagban a Cortana Intelligence Gallery kísérletek található:

[Könyv értékelést előrejelzése](https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-1)

[Könyv értékelést - prediktív Kísérletté előrejelzése](https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-Predictive-Experiment-1)

## <a name="step-1-clean-and-preprocess-text-dataset"></a>1. lépés: Törlése és a szöveg dataset előfeldolgozása
Igény szerinti felosztásával hello felülvizsgálati pontszámok kategorikus alacsony és magas gyűjtők tooformulate hello problémát, két osztályú osztályozás megkezdjük hello kísérlet. Használjuk [szerkesztése metaadatok](https://msdn.microsoft.com/library/azure/dn905986.aspx) és [Kategorikus Csoportértékek](https://msdn.microsoft.com/library/azure/dn906014.aspx) modulok.

![Címke létrehozása](./media/machine-learning-text-analytics-module-tutorial/create-label.png)

Ezt követően azt tiszta hello szöveg használatával [előfeldolgozása szöveg](https://msdn.microsoft.com/library/azure/mt762915.aspx) modul. hello zaj hello adatkészlet hello tisztítás csökkenti, megkeresi hello legfontosabb funkciókról, és javíthatja a Súgó hello hello végső modell pontosságát. Tudjuk eltávolítani szavak - gyakori szavakat, például "a" vagy "a" - és számok, különleges karaktereket, ismétlődő karaktereket, e-mail címek és URL-címeket. Azt is hello szöveg toolowercase konvertálni, lemmatize hello szavak és mondat határokat, majd eldobni észlelése "|||" szimbólum Előfeldolgozott szövegben.

![Szöveg előfeldolgozása](./media/machine-learning-text-analytics-module-tutorial/preprocess-text.png)

Mi történik, ha azt szeretné, toouse szavak egyéni listáját? Is át kell neki adni a nem kötelező bemeneti adatként. Is egyéni C# szintaxis reguláris kifejezés tooreplace karakterláncrészletek használja, és része a beszédfelismerés szavak eltávolításához: parancsokat, műveletek és melléknevek.

Hello előfeldolgozása befejezése után a Microsoft hello adatok felosztása tanítási, és tesztelje a beállítása.

## <a name="step-2-extract-numeric-feature-vectors-from-pre-processed-text"></a>2. lépés: Numerikus szolgáltatás vektorok kinyerése Előfeldolgozott szöveg
szöveges adatok modellt toobuild, általában rendelkeznek tooconvert szabad formátumú szöveg numerikus szolgáltatás veszélyének. A jelen példában használjuk [N-Gramot szolgáltatások bontsa ki a szöveg](https://msdn.microsoft.com/library/azure/mt762916.aspx) modul tootransform hello szöveg toosuch adatformátum. Ez a modul szóköz elválasztott szavakat oszlopot vesz igénybe, és kiszámítja szóból álló dictionary, vagy az N-g, szereplő szavakkal a dataset. Ezt követően álló hány alkalommal minden szó, vagy N-gramot, rekordokban levő jelenik meg, és létrehozza a szolgáltatás vektorok azoktól, amelyeket száma. Ebben az oktatóanyagban hivatott N-gramot mérete too2, így a szolgáltatás vektorok közé tartozik a önálló szavak és két további szavak kombinációi.

![Bontsa ki az N-g](./media/machine-learning-text-analytics-module-tutorial/extract-ngrams.png)

TF érvénybe lépni * tooN-gramot súlyozási IDF (kifejezés gyakoriság inverz dokumentum gyakoriság) száma. Ez a megközelítés hozzáadja súly szó gyakran jelennek meg egyetlen rekordot, de ritka hello teljes adatkészlet között. Egyéb lehetőségek a következők bináris, TF, és súlyú diagramot.

Ilyen szöveg funkciók általában magas granularitása van. Például ha a corpus 100 000 egyedi szót, igényel a funkció kellene 100 000 dimenziók, vagy több N-g használata esetén. hello kibontása N-Gramot szolgáltatások modul lehetővé teszi beállítások tooreduce hello granularitása készlete. Választhatja a rövid és hosszú, vagy túl ritka vagy túl gyakori toohave jelentős prediktív értéket tooexclude szavakat. Az oktatóanyag azt jelennek meg az 5-nél kevesebb rekordok vagy a több mint 80 %-a rekordok N g zárhatja ki.

Szolgáltatás kiválasztása tooselect csak azokat a szolgáltatásokat, amelyek leginkább hello korrelált is használhatja az előrejelzés célhelye. KHI szolgáltatás kijelölés tooselect 1000 szolgáltatások használjuk. A kijelölt szavak vagy N-g hello szóhasználatának hello jobb oldali kimeneti kivonat N-g modul kattintva megtekintheti.

Egy másik módszert toousing kibontása N-Gramot szolgáltatások, mint a Szolgáltatáskivonatolás modul is használhatja. Vegye figyelembe azonban, hogy [Szolgáltatáskivonatolás](https://msdn.microsoft.com/library/azure/dn906018.aspx) nem rendelkezik beépített kijelölés funkciói vagy TF * IDF súlyú.

## <a name="step-3-train-classification-or-regression-model"></a>3. lépés: Besorolási vagy regressziós modell betanításához.
Most hello szöveg lett átalakított toonumeric szolgáltatás oszlopok. hello dataset továbbra is tartalmaz az előző lépésben, karakterlánc típusú oszlopokra, ezért Select Columns a Dataset tooexclude használjuk őket.

Majd használjon [két osztályú logisztikai regresszió](https://msdn.microsoft.com/library/azure/dn905994.aspx) toopredict a cél: magas vagy alacsony felülvizsgálati pontszámot. Ezen a ponton hello szöveg analytics probléma átalakítása rendszeres besorolás hiba. Eszközeivel hello Azure Machine Learning tooimprove hello modellben érhető el. Például kísérletezhet különböző osztályozó toofind hogyan pontos eredményeket adjon, vagy tooimprove hello pontossága hangolása hyperparameter használja ki.

![Tanítási és pontszám](./media/machine-learning-text-analytics-module-tutorial/scoring-text.png)

## <a name="step-4-score-and-validate-hello-model"></a>4. lépés: Pontszám és hello modell ellenőrzése
Hogyan ellenőrizze akkor hello betanított modell? Azt pontozása azt hello tesztelési adatkészletnél ellen, és értékelje ki a hello pontosságát. Hello modell megtanulta, azonban hello szóhasználatának N-g és a tanúsítványsúly hello képzési adatkészletből. Ezért azt használandó adott szóhasználatának és azokat a súlyok kibontása tesztadatok, szolgáltatásait annál ellenezte toocreating hello szóhasználatának. Ezért azt kibontása N-Gramot szolgáltatások modul toohello hello kísérlet fiókirodai pontozási hozzáadása hello kimeneti szóhasználatának csatlakoztatja a képzés ág és hello szóhasználatának mód beállítása csak tooread. Azt is hello N-g gyakoriság szerinti szűrés beállítása hello minimális too1 példány és a maximális too100 % letiltása, és kikapcsolni hello szolgáltatás kiválasztása.

Után hello szöveges oszlop adatok teszt átalakítva toonumeric szolgáltatás oszlopok, azt az előző lépésben oszlop, például a képzés fiókirodai hello karakterlánc zárhatja ki. Ezután használja Score Model-modul toomake előrejelzéseket és a modell kiértékelése modul tooevaluate hello pontosságát.

## <a name="step-5-deploy-hello-model-tooproduction"></a>5. lépés: Hello modell tooproduction telepítése
hello modell tooproduction majdnem kész toobe telepítve. Webszolgáltatás telepítésekor szabad formátumú karaktersorozat bemenetként vesz igénybe, és vissza előrejelzéshez "magas" vagy "alacsony". Megtudta hello N-gramot szóhasználatának tootransform hello szöveg toofeatures használja, és logisztikai regresszió modell toomake azokat a funkciókat az előrejelzéshez képezni. 

tooset hello prediktív kísérletté be, azt először elmentse hello N-gramot szóhasználatának adatkészletet, és hello hello képzési fiókirodai hello kísérlet logisztikai regresszió modell betanítása. Ezt követően elmentjük használja a "Mentés másként" toocreate hello kísérlet egy kísérlet graph prediktív kísérleti fázisú funkciókat. Hello vegyes adatmodult és hello képzési ág nem eltávolítani hello kísérlet. A Microsoft csatlakoztassa korábban mentett hello N-gramot szóhasználatának és a modell tooExtract N-Gramot szolgáltatások és a Score Model modulok, illetve. Azt is hello modell kiértékelése modul eltávolítása.

Azt a beszúrandó oszlopok válassza ki az adatkészlethez modul előfeldolgozása szöveg modul tooremove hello címke oszlop elé, és törölje a pontszám "Append pontszám oszlop toodataset" lehetőséget. Ily módon hello webszolgáltatás nem kér hello címke toopredict próbál, és nem jeleníti meg hello bemeneti szolgáltatások válaszként.

![Prediktív Kísérletté](./media/machine-learning-text-analytics-module-tutorial/predictive-text.png)

Most van egy kísérlet, amely egy webszolgáltatás közzétett és neve a kérés-válasz vagy kötegelt végrehajtási API-k használatával.

## <a name="next-steps"></a>Következő lépések
Tudnivalók a szöveg analytics modulok [az MSDN dokumentációjában tájékozódhat](https://msdn.microsoft.com/library/azure/dn905886.aspx).

