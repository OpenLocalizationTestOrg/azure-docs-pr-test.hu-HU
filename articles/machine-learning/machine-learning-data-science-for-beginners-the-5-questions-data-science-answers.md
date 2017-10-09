---
title: "aaaThe 5 adattudomány kérdésekre - Adattudomány kezdőknek - Azure Machine Learning |} Microsoft Docs"
description: "Adattudomány kezdők érték útmutatást ad az alapvető fogalmait 5 rövid videók, a hello 5 kérdések adatok tudományos válaszok kezdve. Az Azure gépi tanulás."
keywords: "Ennek során adattudomány, adatok tudományos kezdők, adattudomány kezdők, adatok tudományos alapjai, tudományos kérdésekre, adatok tudományos videó, adatok tudományos bemutatása"
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: a01f93ee-01eb-4afe-abbd-cfa035c119b0
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: cgronlun
ms.openlocfilehash: 6de0ca22556771a3044520d9ccc6771c3de78771
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="data-science-for-beginners-video-1-hello-5-questions-data-science-answers"></a>Videó 1 kezdőknek Adattudomány: hello 5 kérdésekre adatok tudományos válaszok
A gyors bevezetés toodata tudományos az beszerzése *Adattudomány kezdőknek* a egy felső adatok tudósok az öt rövid videók. Videók olyan alapvető, de hasznos, hogy érdekli, ennek során adattudomány vagy adatszakértőkön használata.

Ez a videó első kérdésekhez, amely meg tudja válaszolni, adattudomány hello típusú tárgya. tooget hello hatékonyabb működtetését hello adatsorozat, tekintse meg azokat. [Nyissa meg a videók toohello listája](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Shows/SupervisionNotRequired/8/player]
>
>

## <a name="other-videos-in-this-series"></a>A sorozat többi videók
*Adattudomány kezdőknek* egy gyors bevezetés toodata tudományos teljes körülbelül 25 percig tart. Tekintse meg az összes öt videók:

* 1. Videó: hello 5 kérdésekre adatok tudományos válaszok
* 2. Videó: [adattudomány készen áll az adatok?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 perc 56 másodperc)*
* 3. Videó: [tegyen fel kérdést a válasz adatokkal](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 perc 17 másodperc)*
* 4. Videó: [választ egyszerű modell előrejelzése](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 perc 42 másodperc)*
* 5. Videó: [mások munkahelyi toodo adattudomány másolása](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 perc 18 másodperc)*

## <a name="transcript-hello-5-questions-data-science-answers"></a>A Beszélgetés szövegének: hello 5 kérdésekre adatok tudományos válaszok
szia! Üdvözli a toohello videósorozat *Adattudomány kezdőknek*.

Adattudomány ijesztő lehet, ezért I kell bevezetni hello alapjai itt egyenleteket vagy egy zsargon programozási számítógép nélkül.

A első videóban lesz döntésről bővebben a "hello 5 kérdések adatok tudományos válaszokat."

Adattudomány számokat használ, és a nevét (más néven a kategóriák vagy címkék) toopredict válaszok tooquestions.

Akkor lehet, hogy lepje meg, de *adott adatok tudományos válaszok csak öt kérdések nincsenek*:

* Ez A vagy B van?
* Az Ez furcsának?
* Mennyi – vagy – hány?
* Hogyan rendszerezett Ez?
* Mit tegyek mellett?

Ezeket a kérdéseket a machine learning módszerek algoritmusok nevű külön termékcsalád melléket.

Akkor hasznos, mint egy receptet algoritmust és az adatok hello összetevőként toothink. Egy algoritmus megtudhatja, hogyan toocombine és vegyes hello rendelés tooget adatok választ. Számítógépek vannak, például egy keverőgép. A legtöbb hello rögzített munkahelyi hello algoritmus tehetik meg, és tehetik az lefut.

## <a name="question-1-is-this-a-or-b-uses-classification-algorithms"></a>1. kérdés: Az ebben A vagy B? besorolási algoritmusokat használ
Kezdjük hello kérdés: Ez A vagy B?

![Besorolási algoritmusok: Ez A vagy B?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/classification-algorithms.png)

A család két osztályú osztályozás nevezik.

Célszerű minden kérdés, amelynek csak két lehetséges válaszok esetében.

Példa:

* Sikertelen lesz, a kulcsszava hello a következő 1000 miles: Igen vagy nem?
* Ami további ügyfelek elérését: $5-ráta vagy 25 %-os kedvezményes?

Ez a kérdés is lehet rephrased tooinclude több mint két lehetőség közül választhat: Ez A vagy B vagy C vagy D, stb.?  Ez a lehetőség multiclass besorolási és annak hasznos, ha több – vagy több ezer volt – lehetséges válaszokat. Multiclass besorolást választ hello nagy valószínűséggel.

## <a name="question-2-is-this-weird-uses-anomaly-detection-algorithms"></a>2. kérdés: Ez furcsának van? az anomáliadetektálási észlelési algoritmusokat használ
hello következő kérdésre választ ad adattudomány van: Ez furcsa van? Egy anomáliadetektálás nevű család megválaszolja ezt a kérdést.

![Az anomáliadetektálási észlelési algoritmusokat: Ez furcsa van?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/anomaly-detection-algorithms.png)

Ha hitelkártyával, hogy már korábban anomáliadetektálás élvezte. A hitelkártya vállalati a beszerzési kombinációját, elemzi az, hogy azok riasztást küldhet, toopossible csalás. Költségek, amelyek "furcsának" lehet, ahol nem általában felületeknél áruházbeli vagy a vételi szokatlanul pricey elem beszerzési.

Ez a kérdés nagy a módon hasznos lehet. Például:

* Ha egy autó rendelkező alávetni, érdemes lehet a tooknow: a normál olvasása érte?
* Ha most figyelési hello internet, érdemes tooknow: ezt az üzenetet a hello van internet tipikus?

Anomáliadetektálás jelzők váratlan vagy szokatlan események vagy viselkedés. Keresik biztosít ahol toolook a problémákat.

## <a name="question-3-how-much-or-how-many-uses-regression-algorithms"></a>3. kérdés: Mennyi? vagy hogyan sok? regressziós algoritmusokat használ
Sokkal gépi tanulás is képes előrejelzése hello válasz tooHow? vagy hogyan sok? a kérdésre választ ad hello algoritmuscsaládot regressziós nevezik.

![Regressziós algoritmus: mennyi? vagy hogyan sok?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/regression-algorithms.png)

Regressziós algoritmus előrejelzésekhez numerikus, például:

* Következő kedd mi lesz hello hőmérséklet kell?  
* Mi lesz a negyedik negyedév értékesítési?

Annak elősegítése, válaszolja meg a kérdést, amely rákérdez a számára.

## <a name="question-4-how-is-this-organized-uses-clustering-algorithms"></a>4. kérdés: Hogyan ez rendszerezett? Fürtszolgáltatás algoritmusokat használ
Hello utolsó két kérdések most egy fejlettebb bit.

Néha szeretné toounderstand hello struktúra egy adatkészlet - hogyan ez rendszerezett? Ez a kérdés nincs példák, amelyek már ismeri a eredményekkel.

Nincsenek számos módon tootease kimenő adatok hello szerkezete. Egyik módszer van a fürtszolgáltatás. Az elválasztja az adatok természetes "halmozza," a könnyebb értelmezéséhez. Fürtözési, nincs egy megfelelő válasz.

![Fürtszolgáltatás algoritmusok: hogyan ez rendszerezett?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/clustering-algorithms.png)

Fürtszolgáltatás kérdések gyakori példák:

* Mely megjelenítők hello ugyanaz, mint filmek típusú?
* Nyomtató modellek sikertelen hello ugyanúgy?

Megismerni, adatok, akkor is jobban megismerni - és előrejelzése - viselkedéshez és események.  

## <a name="question-5-what-should-i-do-now-uses-reinforcement-learning-algorithms"></a>5. kérdés: Mit tegyek most? használja a tanulási algoritmus megerősítése
hello utolsó kérdést – Mi a teendő most? – családba tartozó nevű megerősítése tanulási algoritmust használ.

Megerősítése tanulási lett ösztönző által hogyan hello agyából patkányok és emberek toopunishment és előnyök válaszol-e. Ezek az algoritmusok ismerje meg az eredményekkel, és adja meg a következő művelet hello.

Megerősítése tanulás általában remekül beválik, ha automatikus rendszerek kis döntések emberi útmutatás nélkül toomake rengeteg rendelkező.

![Tanulási algoritmus megerősítése: Mit tegyek mellett?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/reinforcement-learning-algorithms.png)

Kérdéseket válaszolja meg a rendszer mindig kapcsolatos mit kell meghoznia – általában egy gép vagy egy robot. Például a következők:

* Ha egy házat hőmérséklet ellenőrzési rendszerének vagyok: hello hőmérséklet beállítása, vagy hagyja, ahol van?  
* Ha egy önálló vezetői autó vagyok: egy sárga világos féknek vagy felgyorsítani?  
* A robot vákuumot: vacuuming hagyja, vagy lépjen vissza a díjszabási állomás toohello?

Megerősítése tanulási algoritmusok, akkor lépjen, tanulási próbálkozást az adatgyűjtést.

Hogy az – hello 5 kérdések adattudomány választ ad.

## <a name="next-steps"></a>Következő lépések
* [Próbálja meg egy első adatok tudományos kísérletben a Machine Learning Studio](machine-learning-create-experiment.md)
* [Egy bevezető tooMachine beszerzése a Microsoft Azure tanulási](machine-learning-what-is-machine-learning.md)
