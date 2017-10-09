---
title: "a kérdés adatok választ ad - aaaAsk adatok tudományos problémák - Azure Machine Learning |} Microsoft Docs"
description: "Ismerje meg, hogyan tooformulate éles adattudomány kérdés Adattudomány kezdőknek a videó 3. Besorolás és regressziós kérdések összehasonlítását tartalmazza."
keywords: "adatok tudományos problémák tudományos kérdésekre, állítson össze kérdéseket, a regressziós kérdések, a besorolás kérdéseket, éles kérdés"
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: 5b9501e3-9964-417a-8ffc-8913103da77b
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: cgronlun
ms.openlocfilehash: 00c328f51e6d9ff6654b5966eb97d6762582f7e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="ask-a-question-you-can-answer-with-data"></a>Adatokkal megválaszolható kérdések megfogalmazása
## <a name="video-3-data-science-for-beginners-series"></a>3. Videó: Adattudomány kezdők sorozat
Megtudhatja, hogyan tooformulate Adattudomány videó 3 kezdőknek fel kérdést az adatok tudományos probléma. Ez a videó besorolási és regressziós algoritmus kérdések összehasonlítását tartalmazza.

tooget hello hatékonyabb működtetését hello adatsorozat, tekintse meg azokat. [Nyissa meg a videók toohello listája](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Data-science-for-beginners-Ask-a-question-you-can-answer-with-data/player]
>
>

## <a name="other-videos-in-this-series"></a>A sorozat többi videók
*Adattudomány kezdőknek* van egy gyors bevezetés toodata tudományos az öt rövid videók.

* 1. Videó: [hello 5 kérdésekre adatok tudományos válaszok](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 perc 14 másodperc)*
* 2. Videó: [adattudomány készen áll az adatok?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 perc 56 másodperc)*
* 3. Videó: Tegyen fel kérdést a válasz adatokkal
* 4. Videó: [választ egyszerű modell előrejelzése](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 perc 42 másodperc)*
* 5. Videó: [mások munkahelyi toodo adattudomány másolása](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 perc 18 másodperc)*

## <a name="transcript-ask-a-question-you-can-answer-with-data"></a>A Beszélgetés szövegének: Tegyen fel kérdést a válasz adatokkal
Üdvözli a toohello harmadik videó hello sorozat "Adattudomány kezdőknek."  

Ez egy, a tippek fog kapni olyan vonatkozókat egy kérdést, a válasz adatokkal.

Kaphat további kívül ezt a videót, ha először tekintse meg a sorozat korábbi videók két hello: "hello 5 kérdések adattudomány meg tudja válaszolni" és "adattudomány készen áll-e az adatok?"

## <a name="ask-a-sharp-question"></a>Éles kérdés
Hogyan adattudomány hello folyamata nevekkel (más néven a kategóriák vagy címkék) és számok toopredict egy válasz tooa kérdés túlmutatnak tárgyalt. Nem lehet egyszerűen bármelyik kérdés; azonban toobe rendelkezik egy *éles kérdést.*

Bizonytalan kérdés nem rendelkezik egy vagy több válaszok toobe. Egy éles kérdést kell.

Tegyük fel, a egy genie, akik bármely tegye fel kérdését kételyei vannak válaszolni fog magic lámpa talált. De mischievous genie, és ezután lesz próbálja toomake a válasz bizonytalan és zavaró, ezután a számítógépnél kaphat. Kívánt toopin őt le egy kérdést, hogy ő nem súgó, de mi közli, így záródó tooknow szeretné.

Ha tooask bizonytalan kérdést, például a "Mi a helyzet, a készlet toohappen?", a hello genie előfordulhat, hogy fogadja a hívást, a "hello ár változik". Ez egy truthful választ, de nincs nagyon hasznos.

De ha voltak-e egy éles kérdést, például a "Mi a készlet pénztári ár jövő héten?", hello genie nem súgó de adjon meg egy adott tooask fogadja a hívást, és előre jelezni eladási árat.

## <a name="examples-of-your-answer-target-data"></a>A válasz példái: célalkalmazás adatok
Amennyiben az a kérdés állítson össze, toosee ellenőrizheti, hogy az adatok hello válasz példái vannak.

Ha a kérdés "Mi a készlet pénztári ár lesz jövő héten?" majd kell, hogy az adatok tartalmaz hello árfolyam előzmények toomake.

Ha a kérdés "mely car a saját járműflotta folyamatos toofail először?" majd tudunk toomake meg arról, hogy az adatok előző hibákkal kapcsolatos információkat tartalmaz.

![Cél adatok - példák a válasz. Állítson össze egy adatok tudományos kérdést.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/target-data.png)

Ezekben a példákban válaszok target nevezzük. A célja, hogy mik kapcsolatos későbbi adatpontok toopredict próbált egy kategóriát vagy egy szám.

Ha még nem rendelkezik target adatokat, tooget néhány lesz szüksége. Ön nem fogja tudni tooanswer a kérdés nélkül.

## <a name="reformulate-your-question"></a>A kérdés újraszövegezése
Néha az a kérdés tooget hasznosabb válasz is megjegyzések átfogalmazása.

hello kérdés "az adatok pont A vagy B?" előrejelzi hello kategória (vagy a név vagy címke) valamit. tooanswer használjuk, egy *osztályozó algoritmus*.

hello kérdés "Mennyi?" vagy "Hány?" egy meghatározott előrejelzi. tooanswer azt használjuk a *regressziós algoritmus*.

toosee azt alakíthatja át ezeket, hogyan nézzük hello kérdéssel, "mely hírek szövegegység hello a legérdekesebb toothis olvasó?" Kéri sok lehetőségek – az egyetlen választási előrejelzése azaz "Nem ezt A vagy B vagy C vagy D?" - és egy osztályozó algoritmus használna.

Azonban ez a kérdés lehet könnyebb tooanswer, ha a megjegyzések átfogalmazása azt "hogyan érdekes az egyes szövegegység a a lista toothis olvasó?" Minden cikk biztosíthat most egy numerikus pontszám, amely majd könnyen tooidentify hello legmagasabb pontozási cikk. Ez az egy hello besorolás kérdés egy regressziós kérdéses fogalmazza vagy mennyi?

![A kérdés újraszövegezése. Besorolási kérdést vagy regressziós kérdést.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/classification-question-vs-regression-question.png)

Hogyan kérjen a kérdése a clue toowhich algoritmus tudhatja meg választ.

Látni fogja, hogy az egyes algoritmusok – például hello állók közül. a hírek szövegegység példában - családok szorosan kapcsolódnak. A kérdés toouse hello algoritmus, amely lehetővé teszi az Ön is újraszövegezése hello leghasznosabb fogadja a hívást.

De, legfontosabb kérje meg, hogy éles kérdés - adatokkal meg tudja válaszolni hello kérdést. Ellenőrizze, hogy rendelkezik a megfelelő adatok tooanswer hello és azt.

Néhány alapelvei egy kérdezzen adatokkal meg tudja válaszolni túlmutatnak tárgyalt.

Hello ki, hogy toocheck kell az "Adatok tudományos a kezdők" a Microsoft Azure Machine Learning más videók.

## <a name="next-steps"></a>Következő lépések
* [Próbálja meg egy első adatok tudományos kísérletben a Machine Learning Studio](machine-learning-create-experiment.md)
* [Egy bevezető tooMachine beszerzése a Microsoft Azure tanulási](machine-learning-what-is-machine-learning.md)
