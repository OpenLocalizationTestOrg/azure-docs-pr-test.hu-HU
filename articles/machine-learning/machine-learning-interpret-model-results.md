---
title: "Gépi tanulási modell eredményez aaaInterpret |} Microsoft Docs"
description: "Hogyan toochoose hello optimális paraméter be algoritmust használ, és pontszám modell kimeneti megjelenítése."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 6230e5ab-a5c0-4c21-a061-47675ba3342c
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 52161b1aa5ff3e7a63fc4b1bfb7c5e354eabcc50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="interpret-model-results-in-azure-machine-learning"></a>Az Azure Machine Learning modell eredmények értelmezését
Ez a témakör azt ismerteti, hogyan toovisualize és az Azure Machine Learning Studióban előrejelzés eredmények értelmezéséhez. Miután a modellek betanítása és előrejelzéseket utasítást ("pontozni hello modell") történik, toounderstand kell, és hello előrejelzés eredmény értelmezhetők.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Gépi tanulási modellek az Azure Machine Learning négy fő típusú léteznek:

* Besorolás
* Fürtszolgáltatás
* Regressziós
* Ajánló rendszerek

Ezek a modellek fölött előrejelzés használt hello modulok a következők:

* [Pontszám modell] [ score-model] besorolás és regressziós modul
* [Rendelje hozzá a tooClusters] [ assign-to-clusters] modul a fürtszolgáltatás
* [Pontszám Matchbox ajánló] [ score-matchbox-recommender] javaslat rendszerekhez

Ez a dokumentum azt ismerteti, hogyan toointerpret előrejelzés jár-e az egyes modulok. Ezek a modulok áttekintését lásd: [hogyan toochoose paraméterek toooptimize a algoritmusok az Azure Machine Learning](machine-learning-algorithm-parameters-optimize.md).

Ez a témakör megoldja az előrejelzés értelmezése, de nem a modell kiértékelése. További információ tooevaluate a modell, lásd: [hogyan tooevaluate modell-teljesítmény az Azure Machine Learning](machine-learning-evaluate-model-performance.md).

Ha új tooAzure gépi tanulási és kell létrehozni egy egyszerű kísérlet tooget lépések segítségével, lásd: [egy egyszerű kísérlet létrehozása az Azure Machine Learning Studio](machine-learning-create-experiment.md) az Azure Machine Learning Studióban.

## <a name="classification"></a>Besorolás
Két alkategóriái besorolás problémák:

* Csak két osztály (két osztályú vagy bináris osztályozás) kapcsolatos problémák
* Több mint két osztály (több osztályú osztályozás) kapcsolatos problémák

Az Azure Machine Learning különböző modulok toodeal az összes ilyen típusú besorolás rendelkezik, de az előrejelzés eredmények értelmezéséhez hello módszerei hasonlóak.

### <a name="two-class-classification"></a>Két osztályú osztályozás
**Példa kísérlet**

Példa egy két osztályú osztályozási problémához: iris virág hello besorolása. hello feladata tooclassify iris virág szolgáltatásaik alapján. hello Azure Machine Learning megadott Iris adatkészlet része a népszerű hello [Iris adatkészlet](http://en.wikipedia.org/wiki/Iris_flower_data_set) példányait tartalmazó csak két virágzik felhasználójától (0 és 1 osztály). Nincsenek minden virág (sepal hossza, sepal szélessége, szirom hossza és szirom szélessége) négy funkciói.

![Képernyőkép a iris kísérlet](./media/machine-learning-interpret-model-results/1.png)

1. ábra. IRIS két osztályú osztályozás probléma kísérlet

A kísérlet már elvégezte toosolve a probléma, az 1. ábrán látható módon. A két osztályú súlyozott döntési fa modell betanítása, és a program pontozza a mennyiségeket. Most jelenítheti meg hello előrejelzés eredményeinek hello [Score Model] [ score-model] hello kimeneti portjára hello kattintva modul [Score Model] [ score-model]modult, majd kattintson **Visualize**.

![Score model-modul](./media/machine-learning-interpret-model-results/1_1.png)

Ekkor megjelenik a hello pontozási eredményei a 2. ábrán látható módon.

![Iris két osztályú osztályozás kísérlet eredményei](./media/machine-learning-interpret-model-results/2.png)

2. ábra Két osztályú osztályozás pontszám modell eredményt megjelenítése

**Eredmények értelmezése**

Hello eredmények táblázatában hat oszlopok szerepelnek. hello bal négy oszlopot hello négy funkciók. hello jobb két oszlop, pontozott címkék és a program pontozza a mennyiségeket valószínűség hello előrejelzés eredmények. hello pontozza a mennyiségeket valószínűség oszlop látható hello valószínűsége, hogy a virág tartozik toohello pozitív osztály (osztály % 1). Például hello hello (0.028571) oszlopban, akkor 0.028571 valószínűsége, hogy az első virág hello tartozik tooClass 1 az első szám. Pontozott címkék oszlop látható hello hello osztály minden egyes virág előre jelezni. Ez a program pontozza a mennyiségeket valószínűség oszlop hello alapul. Ha egy virág valószínűségét pontozni hello 0,5 nagyobb, mint osztály 1 előre jelezni. Ellenkező esetben azt várhatóan mint osztály 0.

**Webes szolgáltatás-közzététel**

Miután hello előrejelzés eredmények megértettem, és hang ítélt, hello kísérlet tehetők közzé egy webszolgáltatás, hogy telepítse a különböző alkalmazások és a bármely új iris virág tooobtain osztály előrejelzéseket hívják. toolearn hogyan toochange egy pontozási kísérlet a kísérletek kifejlesztéséhez, és közzéteheti webszolgáltatásként, lásd: [hello Azure Machine Learning webszolgáltatás](machine-learning-walkthrough-5-publish-web-service.md). Ez az eljárás ismerteti a pontozási kísérlet a 3. ábrán látható módon.

![Képernyőkép a pontozási kísérlet](./media/machine-learning-interpret-model-results/3.png)

3. ábra. A pontozási hello iris két osztályú osztályozás probléma kísérlet

A továbbiakban létre kell tooset hello bemeneti és kimeneti hello webszolgáltatáshoz. hello bemeneti érték hello jobb oldali bemeneti portját a [Score Model][score-model], amely van hello Iris virág szolgáltatások bemenet. hello hello kimeneti választott attól függ, hogy érdekli hello az előre meghatározott osztály (pontozott címke), hello program pontozza a mennyiségeket valószínűség, vagy mindkettőt. Ebben a példában feltételezzük, hogy érdekli, mindkét. tooselect hello szükséges kimeneti oszlopok, használja a [oszlopok kiválasztása az adathalmaz] [ select-columns] modul. Kattintson a [oszlopok kiválasztása az adathalmaz][select-columns], kattintson a **Oszlopválasztás**, és válassza ki **pontozott címkék** és **program pontozza a mennyiségeket valószínűség**. Miután beállította a hello kimeneti portjára [oszlopok kiválasztása az adathalmaz] [ select-columns] és újra futtatni, meg kell készen toopublish hello pontozási kísérlet webszolgáltatásként kattintva **webhely közzététele SZOLGÁLTATÁS**. hello elkészült kísérletnek a következőképpen néz 4. ábra.

![hello iris két osztályú osztályozás kísérlet](./media/machine-learning-interpret-model-results/4.png)

4. ábra. Egy iris két osztályú osztályozási problémához végső pontozási kísérlet

Miután hello webszolgáltatás futtatja, és adja meg a teszt-példány néhány funkció érték, hello eredmény két számot adja vissza. hello első szám hello címke program pontozza a mennyiségeket, és hello második hello valószínűség program pontozza a mennyiségeket. A virág 0.9655 valószínűséggel várhatóan osztály 1.

![Teszt értelmezése pontszám modell](./media/machine-learning-interpret-model-results/4_1.png)

![A pontozási teszteredmények](./media/machine-learning-interpret-model-results/5.png)

5. ábra. Webes szolgáltatás eredmény iris két osztályú osztályozás

### <a name="multi-class-classification"></a>Több osztály besorolás
**Példa kísérlet**

A kísérletben multiclass besorolás példa betűnek-felismerési feladat végrehajtása. hello osztályozó kísérletek toopredict egy bizonyos betű (osztály) bizonyos kezű attribútumértékei alapján hello kézzel írott képek kinyert.

![Felismerés példa betűnek](./media/machine-learning-interpret-model-results/5_1.png)

Hello betanítási adatok nincsenek kézzel írott levél képek kinyert 16 szolgáltatásokat. hello 26 betűk a 26 osztályok alkotják. Ábra a kísérlet, amely a multiclass besorolással lesz betanítása modell levél használatát, és a hello előre jelezni azonos 6 mutat be a beállítást, a teszt adatkészletet be.

![Levél felismerés multiclass besorolás kísérlet](./media/machine-learning-interpret-model-results/6.png)

6. ábra. Levél felismerés multiclass besorolás probléma kísérlet

Hello hello eredményeinek megjelenítése [Score Model] [ score-model] hello kimeneti portjára kattintva modul [Score Model] [ score-model] modult, majd Kattintson a **Visualize**, láthatja a tartalmat a 7. ábrán látható módon.

![Pontszám modell eredmények](./media/machine-learning-interpret-model-results/7.png)

7. ábra. A többszörös osztály besorolást pontszám modell eredményeinek képi megjelenítése

**Eredmények értelmezése**

hello bal 16 oszlop felel meg a hello TesztKészlet hello szolgáltatás értékeit. hello oszlopok nevét, például a program pontozza a mennyiségeket valószínűség osztály "XX" vannak hasonlóan hello pontozza a mennyiségeket valószínűség oszlop hello két osztályú esetében. Ezek megjelenítése hello annak valószínűségét, hogy a megfelelő bejegyzés hello beleesik egy adott osztály. Például hello első bejegyzés, nincs 0.003571 valószínűsége, hogy a rendszer egy "A" 0.000451 annak valószínűségét, a "B", és így tovább. hello utolsó oszlop (Scored címkék) ugyanaz, mint a pontozott címkék hello két osztályú esetben van hello. A legnagyobb valószínűség pontozni, hello megfelelő bejegyzés az előre jelzett osztály hello hello hello osztály kiválasztva. Például hello első bejegyzés, hello program pontozza a mennyiségeket címke is, "F" mivel hello legnagyobb valószínűség toobe egy "F" (0.916995) rendelkezik.

**Webes szolgáltatás-közzététel**

Minden egyes belépési és hello valószínűségértékének inverzét hello program pontozza a mennyiségeket címke címke kiértékelni hello is beszerezheti. hello alapszintű logikája toofind hello legnagyobb valószínűség valószínűség pontozni összes hello között. toodo, toouse hello kell [R-parancsfájl végrehajtása] [ execute-r-script] modul. hello R-kód a 8. ábrán látható, és hello hello kísérlet eredménye megjelenik-e a 9. ábra.

![R-példakód](./media/machine-learning-interpret-model-results/8.png)

8. ábra. R-kód pontozott címkék és hello kibontott tartozó valószínűség hello címkék

![Kísérlet eredménye](./media/machine-learning-interpret-model-results/9.png)

9. ábra. Hello levél-felismerési multiclass osztályozási problémához végső pontozási kísérlet

Közzététele és hello webszolgáltatás futtatása, és adja meg az egyes bemeneti szolgáltatás értékeket, után hello visszaadott eredmény tűnik a 10. ábrán. A kézzel írott levél kibontott 16 funkcióival előre jelzett toobe "Ma" 0.9715 valószínűséggel.

![Pontszám modul értelmezése](./media/machine-learning-interpret-model-results/9_1.png)

![Tesztelési eredménye](./media/machine-learning-interpret-model-results/10.png)

10. ábra. Webes szolgáltatás eredmény multiclass besorolás

## <a name="regression"></a>Regressziós
Visszavonási besorolás problémák eltérnek. Egy osztályozási problémához, a kívánt toopredict diszkrét osztályok, melyik osztály például egy iris virág tartozik. Azonban, a következő példa regressziós probléma hello látható, folyamatosan változó, például egy autó árára hello toopredict próbált.

**Példa kísérlet**

A példa autó árának előrejelzése regressziós használni. Annak szolgáltatásait, beleértve ellenőrizze, a üzemanyag típusát, a törzs típusa és a meghajtó kerék alapján egy autó árára toopredict hello próbált. hello kísérlet 11. ábra jelenik meg.

![Autó árának regressziós kísérlet](./media/machine-learning-interpret-model-results/11.png)

11. ábra. Autó árának regressziós probléma kísérlet

Hello megjelenítése [Score Model] [ score-model] modul, a hello eredmény néz 12. ábra.

![Autó árának előrejelzése probléma pontozási eredményei](./media/machine-learning-interpret-model-results/12.png)

12. ábra. A pontozási eredménye hello autó árának előrejelzése probléma

**Eredmények értelmezése**

Pontozott címkék hello eredményoszlop a pontértéket a rendszer. hello számadatok hello előre jelzett minden autó árát.

**Webes szolgáltatás-közzététel**

Hello regressziós kísérlet be egy webszolgáltatás-bővítmény közzététele, és nevezze el a autó árának előrejelzése hello a megszokott módon hello két osztályú osztályozás-és nagybetűhasználattal.

![Kísérlet pontozás autó árának regressziós probléma](./media/machine-learning-interpret-model-results/13.png)

13. ábra. Kísérlet egy autó árának regressziós probléma pontozási

14. ábra eredmény tűnik hello webszolgáltatást futtató, hello adott vissza. a car hello előre jelzett árat $15,085.52.

![Értelmezése pontozási modul](./media/machine-learning-interpret-model-results/13_1.png)

![A modul pontozási eredményei](./media/machine-learning-interpret-model-results/14.png)

14. ábra. Webes szolgáltatás eredménye egy autó árának regressziós probléma

## <a name="clustering"></a>Fürtszolgáltatás
**Példa kísérlet**

Most használja hello Iris adatkészlet újra toobuild egy csoportosítási kísérletet. Itt szűrhet hello osztály címkék hello adathalmaz ki, hogy csak lehetőséggel rendelkezik, és nem használható csoportosítási. Ez iris nagybetűhasználattal, fürtök toobe két során hello képzési, ami azt jelenti, akkor fürtön hello virág, két osztályba hello számát adja meg. hello kísérlet 15. ábrán látható.

![IRIS fürtözési probléma kísérletezhet](./media/machine-learning-interpret-model-results/15.png)

15. ábra. IRIS fürtözési probléma kísérletezhet

Fürtszolgáltatás eltér, mert a besorolás a hello tanítási adathalmazt önmagában nem rendelkezik ground-igazság címkék. Fürtszolgáltatás csoportok hello betanítási készlet példányok különálló csoportokba. Hello képzési folyamat során a hello modell címkék szolgáltatásaik hello különbségei tanulás által hello bejegyzéseket. Ezt követően a betanított modell hello használható toofurther besorolása jövőbeli bejegyzéseket. Azt is fürtözési probléma belül hello eredményének két részből áll. hello első része van a címkézés hello tanítási adathalmazt és hello második van zárolásának hello betanított modell egy új adatkészlet.

hello hello eredmény első részét is lehet ábrázolt kattintson a bal oldali kimeneti portjára hello [Train-csoportosítási modellben] [ train-clustering-model] majd **Visualize**. hello képi megjelenítés 16. ábra jelenik meg.

![Fürtszolgáltatás eredménye](./media/machine-learning-interpret-model-results/16.png)

16. ábra. Fürtszolgáltatás hello tanítási adathalmazt eredménye megjelenítése

17. ábra hello második részre hello képzett csoportosító modell, az új bejegyzések fürtszolgáltatás hello eredménye látható.

![Fürtszolgáltatás eredmény megjelenítése](./media/machine-learning-interpret-model-results/17.png)

17. ábra. Csoportosítás eredménye egy új adatkészlet megjelenítése

**Eredmények értelmezése**

Bár hello eredmények hello két részből áll másik kísérlet szakaszból erednek, megtekintik hello azonos, és a hello értelmezi azonos módon. hello első négy oszlop olyan szolgáltatása. hello utolsó oszlop-hozzárendelés, hello előrejelzés eredménye. hello hozzárendelt bejegyzések hello ugyanannyi vannak előre jelezni toobe hello azonos fürt, ez azt jelenti, hogy a megosztás Hasonlóságok valamilyen módon (az ehhez a kísérlethez hello alapértelmezett Euclidean távolság metrika használja). Megadott fürtök toobe 2 hello számát, mert hozzárendelések hello bejegyzések tartalma, 0 vagy 1.

**Webes szolgáltatás-közzététel**

Kísérlet egy webszolgáltatás-bővítmény a fürtszolgáltatás hello közzététele, és nevezze el a fürtszolgáltatás előrejelzéseket hello azonos módon hello két osztályú osztályozás nagybetűhasználattal.

![Kísérlet pontozás iris fürtözési probléma](./media/machine-learning-interpret-model-results/18.png)

18. ábra. Kísérlet egy iris fürtözési probléma pontozási

Hello webszolgáltatás futtatását követően hello 19. ábra tűnik eredményt adott vissza. Ennél az előre jelzett toobe fürt 0.

![Teszt értelmezhetők pontozási modul](./media/machine-learning-interpret-model-results/18_1.png)

![A pontozási modul eredménye](./media/machine-learning-interpret-model-results/19.png)

19. ábra. Webes szolgáltatás eredmény iris két osztályú osztályozás

## <a name="recommender-system"></a>Ajánló rendszer
**Példa kísérlet**

Ajánló rendszerek esetén használhatja hello éttermi javaslat probléma példaként: is javasoljuk éttermekben, az ügyfelek a minősítési előzményei alapján. hello bemeneti adatok három részből áll:

* Az ügyfelektől éttermi minősítése
* A szolgáltatás ügyféladatok
* Éttermi szolgáltatás adatok

Számos módon teheti azt hello [vonat Matchbox ajánló] [ train-matchbox-recommender] az Azure Machine Learning modulban:

* Egy adott felhasználó és a cikk minősítését előrejelzése
* Javasoljuk, felhasználói elemek tooa
* Felhasználók kapcsolódó tooa felhasználói keresése
* Elemek kapcsolódó tooa megadott elem keresése

Választhat, hogy mit toodo hello hello négy beállítások való kiválasztással **ajánló előrejelzés jellegű** menü. Itt, végezze el az összes négy forgatókönyv.

![Matchbox ajánló](./media/machine-learning-interpret-model-results/19_1.png)

Egy tipikus Azure Machine Learning kísérletet ajánló rendszer esetén a következőképpen néz 20. ábra. További információ a hogyan toouse ezen rendszer ajánló modulok, lásd: [vonat matchbox ajánló] [ train-matchbox-recommender] és [pontszám matchbox ajánló] [ score-matchbox-recommender].

![Ajánló rendszer kísérlet](./media/machine-learning-interpret-model-results/20.png)

20. ábra. Ajánló rendszer kísérlet

**Eredmények értelmezése**

**Egy adott felhasználó és a cikk minősítését előrejelzése**

Kiválasztásával **minősítés előrejelzés** alatt **ajánló előrejelzés jellegű**, arra utasítja a hello ajánló rendszer toopredict hello egy adott felhasználó és a cikk értékelése. hello a képi megjelenítés hello [pontszám Matchbox ajánló] [ score-matchbox-recommender] kimeneti néz 21. ábra.

![Pontszám hello ajánló rendszer – előrejelzés minősítés eredménye](./media/machine-learning-interpret-model-results/21.png)

21. ábra. Hello pontszám eredmény hello ajánló rendszer--minősítés előrejelzés megjelenítése

hello első két oszlop olyan hello felhasználói-elem párok hello bemeneti adatok által biztosított. hello harmadik oszlop hello van egy olyan felhasználó, az egyes elem előre jelzett minősítése. Például hello első sorába vevő U1048 előre jelezni toorate éttermi 135026 2.

**Javasoljuk, felhasználói elemek tooa**

Kiválasztásával **elem ajánlás** alatt **ajánló előrejelzés jellegű**, akkor most kérése hello ajánló rendszer toorecommend elemek tooa felhasználói. hello utolsó paraméter toochoose ebben a forgatókönyvben az *ajánlott a kiválasztott elemeket*. a beállítás hello **az értékelés elemét (modell kiértékelése)** elsősorban a modell kiértékelése hello képzési folyamat során. Az előrejelzés szakaszra választjuk **az összes elemet**. hello a képi megjelenítés hello [pontszám Matchbox ajánló] [ score-matchbox-recommender] kimeneti néz 22. ábra.

![Ajánló rendszer--elem javaslat pontszám eredménye](./media/machine-learning-interpret-model-results/22.png)

22. ábra. Hello ajánló rendszer--elem javaslat pontszám eredményének megjelenítése

hello első hat hello oszlopok jelöli hello felhasználói azonosítók toorecommend elemeket, a megadott bemeneti adatok hello által biztosított. hello más öt oszlop felel meg a hello elemek javasolt toohello felhasználói relevanciájának csökkenő sorrendben. Például hello első sorába hello leginkább ajánlott éttermi ügyfél U1048 134986 135018, 134975, 135021 és 132862 követ.

**Felhasználók kapcsolódó tooa felhasználói keresése**

Kiválasztásával **kapcsolódó felhasználók** alatt **ajánló előrejelzés jellegű**, akkor most hello ajánló rendszer toofind kapcsolódó felhasználók tooa megadott felhasználót kéri. Kapcsolódó felhasználók hello felhasználókat, hasonló beállítások. hello utolsó paraméter toochoose ebben a forgatókönyvben az *kapcsolatos felhasználói kijelöléskor*. a beállítás hello **a felhasználókat, hogy besorolású elemét (modell kiértékelése)** elsősorban a modell kiértékelése hello képzési folyamat során. Válasszon **az összes felhasználó** az előrejelzés. szakaszra vonatkozóan. hello a képi megjelenítés hello [pontszám Matchbox ajánló] [ score-matchbox-recommender] kimeneti néz 23. ábra.

![Ajánló rendszer--kapcsolódó felhasználók pontszám eredménye](./media/machine-learning-interpret-model-results/23.png)

23. ábra. Hello ajánló rendszer--kapcsolódó felhasználók pontszám eredményeinek képi megjelenítése

hello először a megadott felhasználói azonosító szükséges toofind hello hat oszlopok megjelenítése hello kapcsolódó felhasználók, bemeneti adatokat által biztosított. hello más öt oszlopok tárolására hello előre jelzett kapcsolódó felhasználók hello felhasználó relevanciájának csökkenő sorrendben. Hello első sorában, például a hello legfontosabb felhasználói ügyfél U1048 U1051 U1066, U1044, U1017 és U1072 követ.

**Elemek kapcsolódó tooa megadott elem keresése**

Kiválasztásával **kapcsolódó elemek** alatt **ajánló előrejelzés jellegű**, arra utasítja a hello ajánló rendszer toofind kapcsolódó elemek tooa megadott elemével. Kapcsolódó elemek már azonos hello által kedvelt hello elemek valószínűleg toobe felhasználó. hello utolsó paraméter toochoose ebben a forgatókönyvben az *kapcsolódó elem kijelölés*. a beállítás hello **az értékelés elemét (modell kiértékelése)** elsősorban a modell kiértékelése hello képzési folyamat során. Választjuk **az összes elemet** az előrejelzés. szakaszra vonatkozóan. hello a képi megjelenítés hello [pontszám Matchbox ajánló] [ score-matchbox-recommender] kimeneti néz 24. ábra.

![Pontszám eredmény ajánló rendszer--kapcsolódó elemek](./media/machine-learning-interpret-model-results/24.png)

24. ábra. Pontszám modell eredményeinek hello ajánló rendszer--kapcsolódó elemek megjelenítése

hello hat oszlopok jelöli hello megadott azonosítónak szükséges első hello toofind kapcsolódó elemeket, bemeneti adatokat hello által biztosított. hello más öt oszlopok tárolására hello előre jelzett kapcsolódó elemek hello cikk relevanciájának tekintetében csökkenő sorrendben. Például hello első sorába hello legfontosabb elem 135026 elem 135074 135035, 132875, 135055 és 134992 követ.

**Webes szolgáltatás-közzététel**

hello folyamat ezen kísérleteket, web services tooget előrejelzéseket közzétételi hasonlít az egyes hello négy forgatókönyvek. Második forgatókönyv hello (ajánlott elemek tooa felhasználói) itt példaként vesszük. Kövesse ugyanezt az eljárást a másik három hello hello.

Mentése hello ajánló rendszerre a betanított modell betanítása és hello bemeneti adatok tooa szűrés egyetlen felhasználói azonosító oszlopot numerikusként kért, mint 25. ábra hello kísérlet a számítógéphez, és közzéteheti webszolgáltatásként.

![Kísérlet hello éttermi javaslat probléma pontozási](./media/machine-learning-interpret-model-results/25.png)

25. ábra. Kísérlet hello éttermi javaslat probléma pontozási

26. ábra eredmény tűnik hello webszolgáltatást futtató, hello adott vissza. felhasználó U1048 ajánlott éttermekben öt hello 134986, 135018, 134975, 135021 és 132862.

![Ajánló rendszerszolgáltatás minta](./media/machine-learning-interpret-model-results/25_1.png)

![A minta kísérlet eredménye](./media/machine-learning-interpret-model-results/26.png)

26. ábra. Webes szolgáltatás eredménye éttermi javaslat probléma

<!-- Module References -->
[assign-to-clusters]: https://msdn.microsoft.com/library/azure/eed3ee76-e8aa-46e6-907c-9ca767f5c114/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-matchbox-recommender]: https://msdn.microsoft.com/library/azure/55544522-9a10-44bd-884f-9a91a9cec2cd/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-clustering-model]: https://msdn.microsoft.com/library/azure/bb43c744-f7fa-41d0-ae67-74ae75da3ffd/
[train-matchbox-recommender]: https://msdn.microsoft.com/library/azure/fa4aa69d-2f1c-4ba4-ad5f-90ea3a515b4c/
