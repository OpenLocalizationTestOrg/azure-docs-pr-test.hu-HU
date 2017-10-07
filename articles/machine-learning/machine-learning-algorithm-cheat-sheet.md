---
title: "tanulási algoritmus-Adatlap aaaMachine |} Microsoft Docs"
description: "Egy nyomtatható gépi tanulási algoritmus-Adatlap segítségével válassza ki a prediktív modell hello jobb algoritmusa az Azure Machine Learning Studióban."
keywords: "algoritmus-Adatlap, Adatlap, gépi tanulási algoritmus"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: e1dc31ec-1acb-463f-ba77-de565d4ddf4d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: garye
ms.openlocfilehash: 2bedd77bfc65128a90c6128743016415dd8609d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="machine-learning-algorithm-cheat-sheet-for-microsoft-azure-machine-learning-studio"></a>Gyorsreferencia a Machine Learning-algoritmusokhoz a Microsoft Azure Machine Learning Studióban
Hello **Microsoft Azure Machine Learning algoritmus Cheat lap** segít kiválasztani hello jobb egy prediktív elemzési modell algoritmusa.

[Az Azure Machine Learning Studio](https://studio.azureml.net/) rendelkezik a hello algoritmusok hatalmas katalógusában ***regressziós***, ***besorolás***, ***Fürtszolgáltatás***, és ***anomáliadetektálás*** családok. Egy tervezett tooaddress machine learning probléma más típusú.

## <a name="download-machine-learning-algorithm-cheat-sheet"></a>Letöltés: Machine learning algoritmus-Adatlap
**Töltse le a hello Adatlap itt: [Machine Learning algoritmus Cheat lap (11 x 17.)](http://download.microsoft.com/download/A/6/1/A613E11E-8F9C-424A-B99D-65344785C288/microsoft-machine-learning-algorithm-cheat-sheet-v6.pdf)**

![Machine Learning algoritmus-Adatlap: megtudhatja, hogyan toochoose egy gépi tanulási algoritmus.][cheat-sheet]

[cheat-sheet]: ./media/machine-learning-algorithm-cheat-sheet/machine-learning-algorithm-cheat-sheet-small_v_0_6-01.png

Töltse le, és nyomtassa ki tabloid mérete tookeep a Machine Learning algoritmus Cheat lap hello kéznél legyen, és a get-help egy algoritmus kiválasztása.

> [!NOTE]
> Hello cikke [hogyan toochoose algoritmusok használata a Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md) egy részletes útmutató toousing az e Adatlap.
> 
> 

## <a name="more-help-with-algorithms"></a>További segítséget itt találhat algoritmusok
* Ez Adatlap használatával hello megfelelő algoritmus, valamint a gépi tanulási algoritmusok és hogyan által használt különböző típusú hello mélyebb döntéseken kiválasztására vonatkozó útmutatásért lásd: [hogyan toochoose algoritmusok használata a Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md).
* Letölthető infographic, amely leírja a algoritmusok, és a példákat mutat be, lásd: [letölthető Infographic: gépi tanulási algoritmus példákkal alapjai](machine-learning-basics-infographic-with-algorithm-examples.md).
* Az összes hello gépi tanulási algoritmusok a Machine Learning Studióban elérhető kategória szerint listájáért lásd: [inicializálása modell] [ initialize-model] hello Machine Learning Studio algoritmus és a modul segítségével.
* Teljes betűrendben listáját az algoritmusok és a modulok a Machine Learning Studióban, tekintse meg a [a Machine Learning Studio moduljai A-Z listában] [ a-z-list] a Machine Learning Studio algoritmus és a modul segítségével.
* Lásd a toodownload és nyomtatási ábrázoló diagram, amely áttekintést nyújt a Machine Learning Studio funkcióiról hello [Azure Machine Learning Studio képességeit áttekintő diagram](machine-learning-studio-overview-diagram.md).

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="notes-and-terminology-definitions-for-hello-machine-learning-algorithm-cheat-sheet"></a>Megjegyzések és a hello gépi tanulási algoritmus terminológiai definíciók cheat lap

* hello javaslatok érhető el az algoritmus-Adatlap hozzávetőleges szabályok-az-görgetőgomb. Néhány hajlított, és néhány is lehet flagrantly betartva. Ez a tervezett toosuggest kiindulási pontot. Afraid toorun nem lehet egy head-to-head verseny több algoritmusok a adatai között. Nincs egyszerűen nem helyettesíti a hello alapelvek minden algoritmus megismeréséhez, és az adatokat létrehozó hello rendszer ismertetése.

* Minden gépi tanulási algoritmus rendelkezik saját stílus vagy *induktív időeltérés*. Egy adott probléma lehet, hogy több algoritmusok megfelelő, és egy algoritmus lehet, mint a többire jobban megfelelnek. De nem mindig lehetséges tooknow előzetesen amely hello legjobb térkihasználás érdekében. Ilyen esetekben több algoritmusok szereplő együtt hello Adatlap. Megfelelő stratégiát volna tootry egy algoritmust, és hello eredmények még nem kielégítő, ha próbálja hello mások. Íme egy példa a hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/) egy kísérlet, amely több algoritmusok elleni hello ugyanazon adatokhoz és összehasonlítja hello eredmények: [több osztály osztályozó összehasonlítása: felismerés betűs ](http://gallery.cortanaintelligence.com/Details/a635502fc98b402a890efe21cec65b92).

* Gépi tanulás három fő kategóriába sorolhatók: **felügyelt tanítással**, **felügyelt tanítást**, és **megerősítése tanulási**.

  * A **felügyelt tanítással**, minden adatpontnál feliratú vagy kategória vagy egyik fontos értékére.  Példa kategorikus címke van hozzárendelése "cat" vagy "kutya" kép.  Példa egy érték címkéje: hello pénztári ár használt autót társított. hello felügyelt tanítás során célja toostudy számos feliratú példák hasonló, és toobe kapcsolatos későbbi adatpontok képes toomake előrejelzéseket. Új fényképek például megfelelő állat vagy a pontos pénztári árak tooother hozzárendelése használt autók hello azonosítja. Ez olyan népszerű és hasznos a gépi tanulás. Tanulási algoritmus kivételével az összes hello Azure Machine Learning modulok felügyelete [K-közép Fürtszolgáltatás][k-means-clustering].

  * A **felügyelt tanítást**, adatpontok hozzájuk társított címkéket nem rendelkezik. Ehelyett hello felügyeletlen tanulási algoritmus célja van néhány módon vagy toodescribe tooorganize hello adatok struktúrája. Ez azt csoportosításával azt fürtök esetén a K-közép hasonlóan, vagy megtekint összetett adatokat úgy, hogy egyszerűbb különböző módokat kereséséhez.

  * A **megerősítése tanulási**, hello algoritmus művelet lekérdezi toochoose válasz tooeach adatpont. Általános gyakorlatként javasolt a robotics, ahol hello érzékelő szivattyútelepek érzékelőinek adatai egy helyen időben készlete adatpont és hello algoritmus ki kell választania a következő művelet hello robot. Egyúttal természetes az eszközök internetes hálózatát alkalmazások alkalmas. hello tanulási algoritmus is kap egy rövid időn belül, mennyire jó hello döntési jelző lett ellenszolgáltatás jel. Ennek alapján, hello algoritmus módosítja annak biztosításával kapcsolatos stratégia rendelés tooachieve hello legmagasabb ellenszolgáltatás. Jelenleg nincsenek nem tanulási algoritmus modulok Azure ml megerősítése.

* **Bayes-féle módszerek** hello feltételezésen statisztikailag független adatpontok ellenőrizze. Ez azt jelenti, hogy az egy-egy unmodeled variancia hello predikátumban másokkal, ez azt jelenti, hogy nem meghatározható. Például ha rögzíti hello adatok hello hány percig, amíg hello következő subway vonat megérkeznek, két mérések naponta egymástól függetlenek statisztikailag. Azonban két mérések egy percet egymástól függetlenek nem statisztikailag – hello valamelyik értéke magas prediktív más hello hello értékének.

* **Súlyozott döntési fa regressziós** kihasználja a szolgáltatás hozzon létre vagy szolgáltatások közötti kommunikáció. Némileg prediktív hello értékének egy másik, hogy azt jelenti, hogy, a megadott adatokat, majd hello egy funkció értéke. Például a magas vagy alacsony hőmérséklet adatok napi, hello alacsony hőmérséklet tudatában hello napon lehetővé teszi egy ésszerű becslés toomake: hello magas. hello két szolgáltatás lévő hello információt már némileg redundáns.

* Adatok besorolása több mint két kategóriába sorolhatók végezhető vagy egy eredendően több osztály osztályozó, vagy csoportjának egyesítésével képzett két osztályú osztályozó be egy **ensemble**. Hello ensemble módszert használja, nincs külön két osztályú besorolás minden osztályához – hello adatok mindegyik elválasztja a két kategóriába sorolhatók: "Ez az osztály" és "nem ez az osztály." Ezek a besorolások majd hello megfelelő hozzárendelés hello adatpont szavazhatnak. Ez a működési elv hello mögött [egyik-vs-összes Multiclass][one-vs-all-multiclass].

* Tegyük fel, számos módszer, beleértve a logisztikai regresszió és hello Bayes pontozó gépet **lineáris osztály határok**. Ez azt jelenti, hogy azok azt feltételezik, hogy hello osztályok közötti határokat körülbelül egyenes vonalak vagy (a hyperplanes hello általános eset). Gyakran ez a tulajdonság hello adatok után tooseparate próbált, de azt Ön nem tudja csak olyan dolog, amire általában is amelyeket ismernie kell előzetesen megjelenítése. Ha hello osztály határok nagyon szabálytalan, a döntési fák odatapadjon, jungles döntési, támogatja a vektoros gépek vagy a Neurális hálózatokat.

* Neurális hálózatokat használható kategorikus változók hozzon létre egy **típusú változó** kategórián, amely too1 azokban az esetekben, ahol hello kategória vonatkozik, ahol nem 0.


<!-- This is how you can embed a link in an image in HTML. Don't know how toodo this in markdown.
<a href="http://download.microsoft.com/download/A/6/1/A613E11E-8F9C-424A-B99D-65344785C288/microsoft-machine-learning-algorithm-cheat-sheet.pdf">
<img src="C:\Users\garye\azure-docs-pr\articles\media\machine-learning-algorithm-cheat-sheet\cheat-sheet-small.png">
</a>
-->

<!-- Module References -->
[a-z-list]: https://msdn.microsoft.com/library/azure/dn906033.aspx
[initialize-model]: https://msdn.microsoft.com/library/azure/0c67013c-bfbc-428b-87f3-f552d8dd41f6/
[k-means-clustering]: https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/
[one-vs-all-multiclass]: https://msdn.microsoft.com/library/azure/7191efae-b4b1-4d03-a6f8-7205f87be664/
