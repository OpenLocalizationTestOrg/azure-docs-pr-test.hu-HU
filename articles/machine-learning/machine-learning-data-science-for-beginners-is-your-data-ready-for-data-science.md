---
title: "az adatok és készen áll a adattudomány aaaIs? Az adatok kiértékelése - Azure Machine Learning |} Microsoft Docs"
description: "Ismerje meg, hogy készen áll a adatok tudományos adatok toobe hello 4 feltételeit. Videó 2 kezdőknek Adattudomány konkrét példák toohelp alapvető adatok próbaverziójának rendelkezik."
keywords: "a vonatkozó adatok adatok kiértékelése, adatok, adatok feltételek, készen áll a adatok előkészítése"
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: d502062c-da70-4b21-9054-0bfd9902612e
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: cgronlun
ms.openlocfilehash: ef6bb680ace771537157dbdd50a4ccce0a3eb7ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="is-your-data-ready-for-data-science"></a>Készen állnak adatai az elemzésre?
## <a name="video-2-data-science-for-beginners-series"></a>2. Videó: Adattudomány kezdők sorozat
Megtudhatja, hogyan tooevaluate meg arról, hogy készen áll a adattudomány alapvető feltételek toobe megfelel-e az adatok toomake.

tooget hello hatékonyabb működtetését hello adatsorozat, tekintse meg azokat. [Nyissa meg a videók toohello listája](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Shows/SupervisionNotRequired/9/player]
>
>

## <a name="other-videos-in-this-series"></a>A sorozat többi videók
*Adattudomány kezdőknek* van egy gyors bevezetés toodata tudományos az öt rövid videók.

* 1. Videó: [hello 5 kérdésekre adatok tudományos válaszok](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 perc 14 másodperc)*
* 2. Videó: Az adatok készen áll a adattudomány?
* 3. Videó: [tegyen fel kérdést a válasz adatokkal](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 perc 17 másodperc)*
* 4. Videó: [választ egyszerű modell előrejelzése](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 perc 42 másodperc)*
* 5. Videó: [mások munkahelyi toodo adattudomány másolása](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 perc 18 másodperc)*

## <a name="transcript-is-your-data-ready-for-data-science"></a>A Beszélgetés szövegének: Az adatok készen áll a adattudomány?
Üdvözli a túl "az adatok készen áll a adattudomány?" második videó hello sorozat hello *Adattudomány kezdőknek*.  

Mielőtt adattudomány adhat meg a kívánt válaszokat hello telepítette, toogive azt az egyes kiváló minőségű raw anyagok toowork. Csakúgy, mint egy pizzaszósz végez, hello a kiindulási pont, jobb hello összetevők hello jobban hello végső termék. 

## <a name="criteria-for-data"></a>Adatok feltételeit
Igen adattudomány hello esetben van néhány toopull együtt szeretnénk összetevők.

Igazolnia kell, amely adatokat:

* Megfelelő
* Csatlakoztatva
* A pontos
* A elég toowork

## <a name="is-your-data-relevant"></a>Fontos az adatokat?
Ezért hello első összetevő - ellenőriznünk kell a szükséges adatokat.

![A vonatkozó adatokat, és irreleváns adatok - adatok kiértékelése](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/relevant-and-irrelevant-data.png)

Hello tábla hello bal oldalon tekintse meg. Azt kívül Boston sávok hét személyek teljesül, a vér alkohol szint, hello piros Sox batting átlagos az utolsó játékban és hello ár legközelebbi kényelmi tároló hello tej mérni.

Ezek az összes tökéletesen jogos adatai. Az egyetlen hiba, nincs jelentősége. Ezek a számok között nem nyilvánvaló kapcsolat áll fenn. Ha a megadott, i. meg hello tej és hello piros Sox batting átlagos aktuális árlista, semmilyen módon nem lehetett kitalálni a saját vér alkohol tartalom.

Most nézze hello tábla jobb oldali hello. Most azt mérni minden egyes személy szervezet tömeges és leltározott hello számát italok már rendelkeztek.  minden egyes sorára hello számok egyéb releváns tooeach áll. I akitől a szervezet háttértár és hello száma már le kellett Margaritas, ha akkor teheti a saját vér cikkekből alkohol tartalom.

## <a name="do-you-have-connected-data"></a>Tegye csatlakozott adatokat?
hello tovább összetevő az csatlakoztatott adatai.

![Csatlakoztatott vagy leválasztott adat - adatok feltételek, készen áll az adatok](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/connected-vs-disconnected-data.png)

Íme néhány hamburgers hello minőségének vonatkozó adatokat: hőmérséklet patty súly és minősítési hello helyi étele újság a rács. De hello hézagok hello bal oldali hello táblázatban láthatja.

A legtöbb adatkészletek hiányzik néhány érték. Ehhez hasonló közös toohave lyuk és nincsenek módon toowork felhasználókat ezekbe a csoportokba. De ha túl sok hiányzik, az adatok kezdődik, például Svájc közötti sajt toolook.

Hello tábla hello bal oldali tekinti meg, ha van így sokkal hiányzó adatok mentése rögzített toocome bármilyen közötti kapcsolat a hőmérséklet és patty súly rács. Íme egy leválasztott adatok.

hello tábla a jobb oldali hello azonban megtelt, és kész - csatlakoztatott adatok példát.

## <a name="is-your-data-accurate"></a>Az adatok pontos van?
hello tovább kell összetevője pontosságát. Az alábbiakban négy cél, hogy szeretnénk toohit nyíllal.

![Hibás adatokat - adatok feltételek és pontos adatok](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/inaccurate-vs-accurate-data.png)

Tekintse meg hello cél hello jobb felső részén található. Jobb körül hello elem-macskaszem szoros csoportosítása van. Természetesen ez pontos. Furcsa viselkedése adattudomány hello nyelvén, a teljesítmény hello cél jobb oldali alá is figyelembe veszi a pontos.

Ha ki ezen nyilak hello közepére toomap, mutatunk be, hogy a rendszer rendkívül szoros toohello elem-macskaszem. hello nyilak helyezkednek el összes körül hello célként, így nem pontos most minősül, de azok még része a hello elem-macskaszem, így pontos most számít.

Tekintse meg most hello bal felső cél. Itt a nyilak találati nagyon közel együtt szoros csoportosítása. Pontos fontosságúak, de mivel hello center módon hello elem-macskaszem ki pontos fontosságúak. És a természetesen hello nyilak hello bal alsó cél pontatlan, mind a nem pontos. Ez archer további eljárás szükséges.

## <a name="do-you-have-enough-data-toowork-with"></a>Rendelkezik a elég adat toowork?
Végezetül összetevő #4 - toohave elegendő adatot kell.

![Elegendő vonatkozó adatok elemzési célú van? Az adatok kiértékelése](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/barely-enough-data.png)

Gondolja, hogy egy ecset stroke elem található a kifestési a tábla minden egyes adatpont. Ha csak egy része van, lehet, hogy hello kifestési közérthető intelligens – rögzített tootell mi is.

Ha néhány további ecset Strokes elemek hozzáadásához a kifestési tooget kevés élesebb kezdődik.

Ha alig elég vonások, látható csak elég toomake néhány széleskörű döntések. Ennyi az egész valahol toovisit előfordulhat, hogy kívánt? A jelek világos, amely a következőképpen néz tiszta vízjel – Igen, ahol fogom szabadságon van.

További adatok hozzáadása hello kép tisztább válik, és döntéseket lehet részletesebb. Most I is megtekinthetik hello a bal oldali banki hello három szállodák. Tudja, valóban tetszik hello hello előtérben hello architekturális funkcióit. I marad, a harmadik emelet hello.

Az adatokat, amelyek megfelelő, csatlakoztatott, pontos és elég, van néhány kiváló minőségű adattudomány kell toodo összes hello összetevők.

Kell, hogy toocheck hello ki más négy videók *Adattudomány kezdőknek* a Microsoft Azure Machine Learning.

## <a name="next-steps"></a>Következő lépések
* [Próbálja meg egy első adatok tudományos kísérletben a Machine Learning Studio](machine-learning-create-experiment.md)
* [Egy bevezető tooMachine beszerzése a Microsoft Azure tanulási](machine-learning-what-is-machine-learning.md)
