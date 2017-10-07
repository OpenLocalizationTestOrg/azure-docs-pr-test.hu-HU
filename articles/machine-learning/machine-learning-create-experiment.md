---
title: "aaaA egyszerű kísérletben a Machine Learning Studióban |} Microsoft Docs"
description: "Ez a Machine Learning-oktatóanyag egy egyszerű adatelemezési kísérletet mutat be. Egy regressziós algoritmus segítségével egy autó árára hello azt fogja előre jelezni."
keywords: "kísérlet,lineáris regresszió,machine learning-algoritmusok,machine learning-oktatóanyag,prediktív modellezési technikák,adatelemzési kísérlet"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: b6176bb2-3bb6-4ebf-84d1-3598ee6e01c6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: fb215851d380acf7d0f4934a426283369f9c4ccb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="machine-learning-tutorial-create-your-first-data-science-experiment-in-azure-machine-learning-studio"></a>Machine Learning-oktatóanyag: Az első adatelemzési kísérlet létrehozása az Azure Machine Learning Studióban

Ha korábban még soha nem használta az **Azure Machine Learning Studiót**, ez az oktatóanyag Önnek szól.

Ebben az oktatóanyagban hogyan végigvesszük toouse Studio hello az első alkalom toocreate tartalmazó gépi tanulási kísérlet. hello kísérlet egy elemzési modell, amely képes különböző változók, például a márka és a műszaki adatok alapján autók árát hello tesztelni.

> [!NOTE]
> Az oktatóanyag bemutatja, hogyan toodrag és vidd modulok alakzatot kísérletbe, hello alapjait összekapcsolhatja őket, hello kísérlet futtatása, és nézze meg hello eredmények. Nem fogjuk toodiscuss hello általános témakör a gépi tanulás vagy hogyan tooselect és -felhasználási hello 100 + beépített algoritmusok és az adatok adatkezelési modulok Studio szerepel.
>
>Ha új toomachine tanulási, hello videósorozat [Adattudomány kezdőknek](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) előfordulhat, hogy a megfelelő hely toostart. A videósorozat egy nagyszerű bemutatása toomachine tanulási mindennapos és fogalmak.
>
>Ha jártas a gépi tanulás, de a Machine Learning Studióban, és hello gépi tanulási algoritmusok, tartalmaz további általános információt keres, az alábbiakban néhány jó erőforrások:
>
- [Mi a Machine Learning Studio?](machine-learning-what-is-ml-studio.md) - Ez a Studio magas szintű áttekintése.
- [Gépi tanulási algoritmus példákkal alapjai](machine-learning-basics-infographic-with-algorithm-examples.md) -e infographic akkor hasznos, ha azt szeretné, hogy további információk a Machine Learning Studióban található gépi tanulási algoritmusok különböző típusú hello toolearn.
- [Számítógép-tanulási útmutató](https://gallery.cortanaintelligence.com/Tutorial/Machine-Learning-Guide-1) -Ez az útmutató ismerteti hasonló fenti hello infographic, de interaktív formátumot.
- [Gépi tanulási algoritmus-adatlap](machine-learning-algorithm-cheat-sheet.md) és [hogyan toochoose algoritmusok használata a Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md) -a letölthető poszter és kísérő cikk tárgyalja részletesen hello Studio algoritmusok.
- [A Machine Learning Studio: Algoritmust és modult súgó](https://msdn.microsoft.com/library/azure/dn905974.aspx) -Ez az összes Studio modult, beleértve a gépi tanulási algoritmusok hello teljes referenciája

<!-- -->

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-does-machine-learning-studio-help"></a>Hogyan lehet a segítségére a Machine Learning Studio?

A Machine Learning Studio segítségével könnyen tooset energiájának prediktív modellezési módszereket a fogd és vidd modulok használata a kísérlet fel.

Az interaktív, vizuális munkaterület használatával az ***adathalmazokat*** és elemzési ***modulokat*** egy interaktív vászonra húzhatja. Együtt tooform csatlakoztassa őket egy ***kísérletezhet*** , amelyen futtatja, a Machine Learning Studióban.
Ön ***modell létrehozása***, ***hello modell betanításához***, és ***pontszám és tesztelési hello modell***.

Akkor is többször a modell tervezési hello kísérlet szerkesztése és is fut, amíg nem ad meg hello eredmények keres. Ha a modell elkészült, közzéteheti ***webszolgáltatásként***, így mások is küldhetnek bele adatokat és kaphatnak előjelzéseket.

## <a name="open-machine-learning-studio"></a>A Machine Learning Studio megnyitása

tooget lépések Studio, a go túl[https://studio.azureml.net](https://studio.azureml.net). Ha korábban már bejelentkezett a Machine Learning Studióba, kattintson a **Sign in** (Bejelentkezés) lehetőségre. Ellenkező esetben kattintson a **Sign up here** (Regisztráció itt) lehetőségre, majd válasszon az ingyenes vagy a fizetős lehetőség használata közül.

![Jelentkezzen be a Learning Studio tooMachine][sign-in-to-studio]
<br/>
***Jelentkezzen be a Learning Studio tooMachine***

## <a name="five-steps-toocreate-an-experiment"></a>A kísérlet öt lépése toocreate

A machine learning oktatóanyagban bemutatjuk öt alapvető lépéseket toobuild egy kísérletben a Machine Learning Studio toocreate, vonat, hajtsa végre, a modell pontozása:

- **Modell létrehozása**
    - [1. lépés: Az adatok beszerzése]
    - [2. lépés: Hello adatok előkészítése]
    - [3. lépés: A jellemzők meghatározása]
- **Hello tanítási**
    - [4. lépés: Tanulási algoritmus kiválasztása és alkalmazása]
- **Pontszám és tesztelési hello modell**
    - [5. lépés: Új autó árának előrejelzése]

[1. lépés: Az adatok beszerzése]: #step-1-get-data
[2. lépés: Hello adatok előkészítése]: #step-2-prepare-the-data
[3. lépés: A jellemzők meghatározása]: #step-3-define-features
[4. lépés: Tanulási algoritmus kiválasztása és alkalmazása]: #step-4-choose-and-apply-a-learning-algorithm
[5. lépés: Új autó árának előrejelzése]: #step-5-predict-new-automobile-prices

> [!TIP] 
> Egy működő hello található hello szereplő következő [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com). Nyissa meg túl**[az első adattudomány kísérletezhet - autó árának előrejelzése](https://gallery.cortanaintelligence.com/Experiment/Your-first-data-science-experiment-Automobile-price-prediction-1)**  kattintson **Megnyitás a Studióban** toodownload hello kísérletben a Machine Learning a másolatát Studio munkaterületen.


## <a name="step-1-get-data"></a>1. lépés: Az adatok beszerzése

hello elsőként tooperform gépi tanulás van szüksége az adatai.
A Machine Learning Studio számos mintaként használható adathalmazt tartalmaz, de számos más forrásból is importálhat adatokat. Ehhez a példához hello mintahalmazt fogjuk használni **Automobile price data (Raw)**, amely tartalmazza a munkaterületen.
Ebben az adathalmazban számos különböző autót bemutató bejegyzés szerepel. A bejegyzések számos adatot (például márka, típus, műszaki specifikációk, ár) tartalmaznak.

Ez hogyan tooget hello kísérletbe való adatkészlet.

1. Hozzon létre egy új kísérlet kattintva **+ új** hello hello Machine Learning Studio ablakának alsó részén, jelölje ki a **KÍSÉRLETEZHET**, majd válassza ki **üres kísérlet**.

2. hello kísérlet hello vásznon a hello tetején megjelenik egy alapértelmezett nevet kap. Válassza ki a szöveget, és adjon neki például értelmezhető, toosomething **autó árának előrejelzése**. hello névnek nem kell egyedi toobe.

    ![Nevezze át a hello kísérlet][rename-experiment]

2. hello kísérletvászonra toohello balra az adathalmazokat és modulokat tartalmazó paletta. Típus **automobile** hello keresőmezőbe paletta toofind hello adatkészlethez feliratú hello tetején **Automobile price data (Raw)**. Húzza a dataset toohello kísérlet vászonra.

    ![Hello autókat tartalmazó adathalmaz található, és húzza a kísérletvászonra hello][type-automobile]
    <br/>
    ***Hello autókat tartalmazó adathalmaz található, és húzza a kísérletvászonra hello***

toosee mi ezek az adatok úgy tűnik, hello kimeneti port hello hello autókat tartalmazó adathalmaz alsó részén kattintson, majd **Visualize**.

![Kattintson a hello kimeneti portra, és válassza a "Megjelenítés"][select-visualize]
<br/>
***Kattintson a hello kimeneti portra, és válassza a "Megjelenítés"***

> [!TIP]
> Adathalmazokat és modulokat bemeneti rendelkezik, és kimeneti portok kisméretű körök - hello felső bemeneti portok által képviselt kimeneti portot hello lap alján.
a folyamat az adatok kísérletbe toocreate, lesz csatlakozzon a kimeneti portra, egy modul tooan bemeneti port egy másik.
Bármikor kattintson a dataset vagy modul toosee hello kimeneti portjára hello adatok a következőképpen néz ezen a ponton a hello adatfolyam.

Minta DataSet adatkészletben minden példány autók sorként, és minden autó társított hello változók oszlopaiként. Egy adott automobile tartozó hello változók megadott, az oktatóanyagban módosítjuk tootry toopredict hello ár jobb szélső oszlop (oszlop 26, című "price").

![Hello adatok képi megjelenítési ablakot hello autó adatainak megtekintéséhez][visualize-auto-data]
<br/>
***Hello adatok képi megjelenítési ablakot hello autó adatainak megtekintéséhez***

Bezárás hello képi megjelenítési ablakot hello kattintva "**x**" hello jobb felső sarokban.

## <a name="step-2-prepare-hello-data"></a>2. lépés: Hello adatok előkészítése

Az adathalmazok elemzése előtt általában némi előfeldolgozás szükséges. Például észrevette, hogy hiányoznak az értékek hello oszlopok számos sorából hello. A hiányzó értékeket kell tisztítani, hello modell elemezni tudja hello adatok megfelelően toobe. Ebben az esetben törölni fogjuk azokat a sorokat, amelyekből értékek hiányoznak. Emellett hello **normalized-losses** oszlop nem rendelkezik a hiányzó értékeket, nagy részét, így igazolnia kell, hogy oszlop kizárásához hello modellből regisztrálását.

> [!TIP]
> Hiányzik a bemeneti adatok tisztítására szolgáló hello hello modulok többsége használatának előfeltétele.

Először adja hozzá azt az a modul, amely eltávolítja a hello **normalized-losses** oszlop teljesen, majd azt egy másik modul, amely eltávolítja az összes sort, amelyből adatok hiányoznak.

1. Típus **egy oszlopot válasszon ki** hello keresőmezőbe hello modul paletta toofind hello hello tetején [Select Columns in Dataset] [ select-columns] modult, majd húzza a kísérletvászonra toohello . Ezzel a modullal kiválaszthatjuk, melyik adatoszlopokat azt szeretné, hogy tooinclude, vagy éppen kizárni hello modell tooselect.

2. Csatlakozás hello kimeneti portjára hello **Automobile price data (Raw)** dataset toohello bemeneti portját hello [Select Columns in Dataset] [ select-columns] modul.

    ![Adja hozzá a hello "Válasszon oszlopok a Dataset" modul toohello kísérletvászonra, és csatlakoztassa][type-select-columns]
    <br/>
    ***Adja hozzá a hello "Válasszon oszlopok a Dataset" modul toohello kísérletvászonra, és csatlakoztassa***

3. Kattintson a hello [Select Columns in Dataset] [ select-columns] modul, és kattintson **Oszlopválasztás** a hello **tulajdonságok** ablaktáblán.

    - Hello bal oldalon kattintson **szabályokkal**
    - A **Begin With** (Kezdés a következővel) területen kattintson az **All columns** (Minden oszlop) lehetőségre. Ezzel megadja [Select Columns in Dataset] [ select-columns] toopass összes hello oszlopát (kivéve az ilyen oszlopokat kapcsolatos tooexclude el).
    - Hello legördülő listákat, válassza ki **kizárása** és **oszlopnevek**, és kattintson a hello szövegdobozba. Megjelenik az oszlopnevek listája. Válassza ki **normalized-losses**, és a hozzáadott toohello szövegmezőben.
    - Hello pipa (OK) gomb tooclose hello Oszlopválasztó (a jobb alsó hello) gombra.

    ![Indítsa el a hello Oszlopválasztó és hello "normalized-losses" oszlop kizárása][launch-column-selector]
    <br/>
    ***Indítsa el a hello Oszlopválasztó és hello "normalized-losses" oszlop kizárása***

    Most hello tulajdonságok panelen az **Select Columns in Dataset** azt jelzi, hogy azt fog továbbítani az összes oszlop hello adatkészletből kivéve **normalized-losses**.

    ![hello tulajdonságok panelen látható hello "normalized-losses" oszlop ki van zárva.][showing-excluded-column]
    <br/>
    ***hello tulajdonságok panelen látható hello "normalized-losses" oszlop ki van zárva.***

    > [!TIP]
    A Megjegyzés tooa modul duplán hello modul és a belépés szöveget adhat hozzá. Ez segít gyorsan áttekintse milyen hello modul a kísérletben végez műveletet. Ebben az esetben kattintson duplán a hello [Select Columns in Dataset] [ select-columns] modul és a típus hello Megjegyzés "Kizárási normalizált veszteségek."
    >
    >


    ![Kattintson duplán egy modul tooadd Megjegyzés][add-comment]
    <br/>
    ***Kattintson duplán egy modul tooadd Megjegyzés***

3. A csomóponthúzási hello [Clean Missing Data] [ clean-missing-data] modul toohello vászonra kísérletek kifejlesztéséhez, és csatlakoztassa toohello [Select Columns in Dataset] [ select-columns] modul. A hello **tulajdonságok** ablaktáblán válassza előbb **teljes sor eltávolítása** alatt **Cleaning mód**. Ezzel megadja [Clean Missing Data] [ clean-missing-data] tooclean hello adatait a hiányzó értékeket tartalmazó sorok eltávolítása. Kattintson duplán a hello modul, és írja be a hello megjegyzést "Hiányzó értéket tartalmazó sorok eltávolítása."

    ![Hello tisztítási mód beállítása túl "teljes sor eltávolítása" hello "Clean Missing Data" modul][set-remove-entire-row]
    <br/>
    ***Hello tisztítási mód beállítása túl "teljes sor eltávolítása" hello "Clean Missing Data" modul***

4. Futtassa a hello kísérlet kattintva **futtatása** alján hello hello.

    Hello kísérlet futása befejeződött, minden hello modul kell egy zöld pipa tooindicate, hogy sikeresen befejeződött. Figyelje meg is hello **végzett futó** állapot hello jobb felső sarokban.

![Után is fut, hello kísérlet kell kinéznie][early-experiment-run]
<br/>
***Után is fut, hello kísérlet kell kinéznie***

> [!TIP]
> Miért azt futott hello kísérlet most? Futó hello kísérletet, szükség szerint hello oszlopdefiníciók az adataink hello adatkészletből hello keresztül továbbítja [Select Columns in Dataset] [ select-columns] modul, és a hello [Clean Missing Data] [ clean-missing-data] modul. Ez azt jelenti, hogy a Microsoft connect túl modul[Clean Missing Data] [ clean-missing-data] ezt az információt is.

Összes azt fel toothis pont hello kísérletben megtette az tiszta hello adatai. Tooview tisztítani hello dataset, kattintson a bal oldali kimeneti portjára hello hello [Clean Missing Data] [ clean-missing-data] modul, és válassza ki **Visualize**. Figyelje meg, hogy hello **normalized-losses** oszlop már nem része, és nincsenek hiányzó értékek.

Most, hogy hello adatok tiszta, a rendszer készen áll a toospecify funkciók megismeréséhez azt módosítjuk toouse hello prediktív modellben.

## <a name="step-3-define-features"></a>3. lépés: A jellemzők meghatározása

A gépi tanulásban a *jellemzők* azok a külön-külön mérhető tulajdonságok, amelyekre kíváncsiak vagyunk. Adathalmazunk minden sora egy-egy autót képvisel, az oszlopok pedig az autók különböző jellemzőit tartalmazzák.

Egy jó funkcióinak a prediktív modellben szükséges kísérletezhet és hello problémával kapcsolatos Tudásbázis keresése kívánt toosolve. Egyes szolgáltatások akkor nagyobb, mint a többire hello cél előrejelzéséhez. Ráadásul egyes jellemzők erős korrelációban állnak más jellemzőkkel, és eltávolíthatók. Például város-mpg és highway-mpg szorosan kapcsolódnak, azt másikat, és távolítsa el más hello hello előrejelzés jelentős befolyásolása nélkül.

Létrehozzuk a modellt, amely az adathalmaz hello funkciók egy részét használja. Térjen vissza később és más jellemzőket, futtassa újra a hello kísérlet, és tekintse meg, ha jobb eredményeket kap. De toostart, próbáljon a következő funkciók hello:

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price


1. Húzzon egy újabb [Select Columns in Dataset] [ select-columns] modul toohello kísérlet vászonra. Csatlakozás a bal oldali kimeneti portjára hello hello [Clean Missing Data] [ clean-missing-data] modul toohello bemeneti hello [Select Columns in Dataset] [ select-columns] modul.

    ![Csatlakozás hello "Válasszon oszlopok a Dataset" modul toohello "Clean Missing Data" modul][connect-clean-to-select]
    <br/>
    ***Csatlakozás hello "Válasszon oszlopok a Dataset" modul toohello "Clean Missing Data" modul***

2. Kattintson duplán a hello modul, és írja be a "Szolgáltatások előrejelzés kiválasztása."

2. Kattintson a **Oszlopválasztás** a hello **tulajdonságok** ablaktáblán.

3. Kattintson a **With rules** (Szabályokkal) lehetőségre.

4. A **Begin With** (Kezdés a következővel) területen kattintson a **No columns** (Egyetlen oszlop sem) lehetőségre. Hello szűrő sorban válassza **Include** és **oszlopnevek** , és válassza ki az oszlopnevek listája hello szövegmezőben. Ezzel megadja hello modul toonot továbbítása (szolgáltatások) oszlopot, azzal a különbséggel, adtuk meg hello.

5. Hello pipa (OK) gombra.

    ![Válassza ki a hello (szolgáltatások) oszlopok tooinclude hello előrejelzés][select-columns-to-include]
    <br/>
    ***Válassza ki a hello (szolgáltatások) oszlopok tooinclude hello előrejelzés***

Ezzel létrehozza azt szeretnénk, hogy a tanulási algoritmus fogjuk használni, a következő lépésben hello toopass toohello csak hello szolgáltatásokat tartalmazó szűrt adatkészlet. Később visszatérhet ide, és más jellemzőkkel is elvégezheti az előrejelzést.

## <a name="step-4-choose-and-apply-a-learning-algorithm"></a>4. lépés: Tanulási algoritmus kiválasztása és alkalmazása

Most, hogy készen áll a hello adatokat, hozhat létre, a prediktív modell áll betanításra és tesztelésre. Az adatok tootrain hello modellt fogjuk használni, és majd teszteljük fogja hello modell toosee milyen közeli képes toopredict árak.
<!-- For now, don't worry about *why* we need tootrain and then test a model.-->

A *besorolás* és a *regresszió* két algoritmus, amelynek segítségével felügyelt gépi tanítás valósítható meg. Besoroláskor a válaszok előrejelzése megadott kategóriakészletből történik (például: színek (vörös, kék vagy zöld)). Regressziós használt toopredict több.

Azt szeretnénk, ha toopredict ár, amely egy számot, mert egy regressziós algoritmus fogjuk használni. Ebben a példában egy egyszerű *lineáris regressziós* modellt használunk.

> [!TIP]
> Ha azt szeretné, hogy több különböző típusú gépi tanulási algoritmusok toolearn, és ha toouse őket, Adattudomány hello kezdők sorozata, előfordulhat, hogy megtekinti hello első videó [hello öt kérdésekre adatok tudományos válaszok](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md). Hello infographic is előfordulhat, hogy megnézi [gépi tanulási algoritmus példákkal alapjai](machine-learning-basics-infographic-with-algorithm-examples.md), vagy tekintse meg a hello [gépi tanulási algoritmus-adatlap](machine-learning-algorithm-cheat-sheet.md).

Adjon neki a hello ár tartalmazó adatkészletet hello modell azt betanításához. hello modell hello adatokat, és keresse meg egy autó szolgáltatások és az ár közötti összefüggések keres. Majd teszteljük fogja hello modell - igazolnia kell adjon neki az autók el jártasak funkciókat, és tekintse meg, milyen pontossággal hello modell előre toopredicting hello ismert ár.

Hello modell betanítása és vizsgálja, hogy a külön modell betanítására és tesztelésére adatkészletek hello adatok rendezze az adatokat fogjuk használni.

1. Válassza ki, majd húzza a hello [Split Data] [ split] modul toohello vászonra kísérletek kifejlesztéséhez, és csatlakoztassa toohello utolsó [Select Columns in Dataset] [ select-columns] a modul.

2. Kattintson a hello [Split Data] [ split] modul tooselect azt. Hello található **hello sorok először kimeneti adatkészlet** (a hello **tulajdonságok** ablaktábla toohello jog a vásznon a hello), majd állítsa be too0.75. Ezzel a módszerrel kell felhasználása hello tootrain hello adatmodell 75 százalékát, és 25 százalékát tesztelési (később, kísérletezhet különböző százalékos használatával).

    ![Ossza fel azon részét, hello "Split Data" modul too0.75 set hello][set-split-data-percentage]
    <br/>
    ***Ossza fel azon részét, hello "Split Data" modul too0.75 set hello***

    > [!TIP]
    > Hello módosításával **véletlenszerű kezdőérték** paraméter, hozhat létre különböző véletlenszerűen kiválasztott mintákat a modell betanítására és tesztelésére. Ezzel a paraméterrel állítható hello összehangolása hello pszeudo véletlenszám-generátor kezdőértékét.

2. Hello kísérlet futtatása. Hello kísérlet futtatásakor hello [Select Columns in Dataset] [ select-columns] és [Split Data] [ split] modulok oszlop definíciók toohello adja át. a modulok a következőkben hozzáadott mellett.  

3. tooselect hello tanulási algoritmus, bontsa ki a hello **Machine Learning** hello modul paletta toohello balra kategóriájában hello a kísérletvászonra, és végül **inicializálása modell**. Itt számos modulkategória közül választhat, amelyek használt tooinitialize gépi tanulási algoritmusok lehetnek. Ehhez a kísérlethez válassza ki a hello [lineáris regressziós] [ linear-regression] alatt hello modul **regressziós** kategóriát, és húzza a kísérletvászonra toohello.
(Megtalálhat hello modul hello paletta keresőmezőjébe írja be a "linear regression".)

4. Keresse meg, és húzza hello [tanítási modell] [ train-model] modul toohello kísérlet vászonra. Csatlakozás hello hello kimenete [lineáris regressziós] [ linear-regression] toohello modul bal oldali bemeneti hello [tanítási modell] [ train-model] modul, és hello adatokat tartalmazó kimenetével (bal oldali port) a hello betanítása [Split Data] [ split] modul toohello jobb bemeneti hello [tanítási modell] [ train-model] a modul.

    ![Csatlakozás hello "Tanítási modell" modul tooboth hello "Linear Regression" és "Split Data" modulok][connect-train-model]
    <br/>
    ***Csatlakozás hello "Tanítási modell" modul tooboth hello "Linear Regression" és "Split Data" modulok***

5. Hello kattintson [tanítási modell] [ train-model] modult, kattintson a **Oszlopválasztás** a hello **tulajdonságok** ablaktáblán, és jelölje ki hello **ár** oszlop. Ez az, hogy a modell-e a további folyamatos toopredict hello érték.

    Hello választja **ár** hello Oszlopválasztó hello való áthelyezésével oszlopa **elérhető oszlopok** toohello listában **kijelölt oszlopok** listája.

    ![Válassza ki a hello ár oszlop hello "Tanítási modell" modul][select-price-column]
    <br/>
    ***Válassza ki a hello ár oszlop hello "Tanítási modell" modul***

6. Hello kísérlet futtatása.

Most már tudunk képzett regressziós modell, amely használt tooscore új autó adatok toomake ár előrejelzéseket lehet.

![Után fut, hello kísérlet most hasonlóan kell kinéznie Ez][second-experiment-run]
<br/>
***Után fut, hello kísérlet most hasonlóan kell kinéznie Ez***

## <a name="step-5-predict-new-automobile-prices"></a>5. lépés: Új autó árának előrejelzése

Most, hogy azt már betanítása hello-modellben adataink 75 százalékával, használhatjuk tooscore hello maradék 25 százalék hello adatok toosee mennyire működik.

1. Keresse meg, és húzza hello [Score Model] [ score-model] modul toohello kísérlet vászonra. Csatlakozás hello hello kimenete [tanítási modell] [ train-model] bal oldali bemeneti portját a modul toohello [Score Model][score-model]. Csatlakozás hello tesztelési adatokat tartalmazó kimenetével (jobb oldali portjával) a hello [Split Data] [ split] modul toohello jobb oldali bemeneti portját [Score Model][score-model].

    ![Csatlakozás hello "Score Model" modul tooboth hello "Tanítási modell" és "Split Data" modulok][connect-score-model]
    <br/>
    ***Csatlakozás hello "Score Model" modul tooboth hello "Tanítási modell" és "Split Data" modulok***

2. Hello kísérlet futtatásához és megtekintéséhez hello hello kimenete [Score Model] [ score-model] modul (hello kimeneti portjára kattintson [Score Model] [ score-model] és kiválasztása **Megjelenítése**). hello áron előre jelzett értékek és ismert értékeinek hello Tesztadatok hello hello kimenet megjelenítése.  

    ![Hello "Score Model" modul kimenete][score-model-output]
    <br/>
    ***Hello "Score Model" modul kimenete***

3. Végezetül teszteljük hello eredmények hello minőségét. Válassza ki, majd húzza a hello [modell kiértékelése] [ evaluate-model] modul toohello kísérlet vászonra, és csatlakozzon a hello hello kimenete [Score Model] [ score-model] bal oldali bemeneti modul toohello [modell kiértékelése][evaluate-model].

    > [!TIP]
    > Vannak két bemeneti portok hello [modell kiértékelése] [ evaluate-model] modul mert használt toocompare két modell párhuzamos lehet. Később, akkor egy másik algoritmus toohello kísérlet hozzáadhat és használhat [modell kiértékelése] [ evaluate-model] toosee melyik jobb eredményt ad.

4. Hello kísérlet futtatása.

hello tooview hello kimenete [modell kiértékelése] [ evaluate-model] modulban kattintson hello kimeneti portra, és válassza **Visualize**.

![Hello kísérleti fázisú funkciókat kiértékelésének eredménye][evaluation-results]
<br/>
***Hello kísérleti fázisú funkciókat kiértékelésének eredménye***

hello a következő statisztikák tekinthetők láthatók:

- **Mean Absolute Error** (MAE): hello abszolút eltérésének átlaga (egy *hiba* hello különbség hello előre meghatározott és tényleges értékkel hello).
- **Root Mean Squared hiba** (Gyökátlagos): hello négyzetgyökét hello tesztelési adathalmazon végzett előrejelzések eltéréseinek négyzetéből hello átlaga.
- **Relative Absolute Error**: hello abszolút eltérésének relatív toohello abszolút eltérés és az összes tényleges értékek átlaga hello tényleges értékek átlaga.
- **Relatív Squared hiba**: hello átlaga relatív négyzetes értékéhez toohello négyzet hello tényleges értékek és hello összes tényleges értékek átlaga közötti különbség.
- **Együttható**: néven is ismert hello **R-négyzet értéke**, jelezve, hogy mennyire illik legjobban a modell hello adatok statisztikai mérőszám azt.

Az egyes hello hiba statisztika, kisebb célszerűbb. A kisebb értékek azt jelzik, hogy hello előrejelzés közelebb hello tényleges értékek. A **Coefficient of Determination**, hello közelebb értéke pedig tooone (1.0) hello jobb hello előrejelzéseket.

## <a name="final-experiment"></a>Elkészült kísérlet

hello elkészült kísérletnek kell kinéznie:

![hello elkészült kísérletnek][complete-linear-regression-experiment]
<br/>
***hello elkészült kísérletnek***

## <a name="next-steps"></a>Következő lépések

Hello első machine learning oktatóanyag elvégzése után, és beállította kísérletét rendelkezik, továbbra is tooimprove hello modell, és majd telepítenie kell azt prediktív webszolgáltatásként.

- **Többször tootry tooimprove hello modell** – például hello funkcióra az előrejelzés a módosíthatja. Hello hello tulajdonságait módosíthatja vagy [lineáris regressziós] [ linear-regression] algoritmust, vagy próbálja meg egy teljesen más algoritmust is. Még egy időben tanulási algoritmusok tooyour kísérlet különböző gépi és hasonlítsa össze azokat két hello segítségével [modell kiértékelése] [ evaluate-model] modul.
Hogyan toocompare egyetlen kísérlet, a több modellek: példa [összehasonlítása Regresszort](https://gallery.cortanaintelligence.com/Experiment/Compare-Regressors-5) a hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).

    > [!TIP]
    > toocopy ismétlések kísérletbe, használjon hello **SAVE AS** alján hello hello gombra. Gombra kattintva megtekintheti az összes hello ismétlések kísérletbe **futtatása ELŐZMÉNYEINEK megtekintése** hello lap hello alján. További információ: [Kísérlet ismétléseinek kezelése az Azure Machine Learning Studióban][runhistory].

[runhistory]: machine-learning-manage-experiment-iterations.md

- **A prediktív webszolgáltatásként hello modell rendszerbe állítása** – Ha elégedett a modellel, telepítheti azt egy webes szolgáltatás toobe toopredict autó árának használt új adatok használatával. További információ: [Azure Machine Learning-webszolgáltatás üzembe helyezése][publish].

[publish]: machine-learning-publish-a-machine-learning-web-service.md

Szeretné, hogy további toolearn? Hello folyamat létrehozása, betanítási, pontozási és központi telepítése egy modell széles körű, és részletes útmutatást lásd: [prediktív megoldás kifejlesztése az Azure Machine Learning segítségével][walkthrough].

[walkthrough]: machine-learning-walkthrough-develop-predictive-solution.md

<!-- Images -->
[sign-in-to-studio]: ./media/machine-learning-create-experiment/sign-in-to-studio.png
[rename-experiment]: ./media/machine-learning-create-experiment/rename-experiment.png
[visualize-auto-data]:./media/machine-learning-create-experiment/visualize-auto-data.png
[select-visualize]: ./media/machine-learning-create-experiment/select-visualize.png
[showing-excluded-column]:./media/machine-learning-create-experiment/showing-excluded-column.png
[set-remove-entire-row]:./media/machine-learning-create-experiment/set-remove-entire-row.png
[early-experiment-run]:./media/machine-learning-create-experiment/early-experiment-run.png
[select-columns-to-include]:./media/machine-learning-create-experiment/select-columns-to-include.png
[second-experiment-run]:./media/machine-learning-create-experiment/second-experiment-run.png
[connect-score-model]:./media/machine-learning-create-experiment/connect-score-model.png
[evaluation-results]:./media/machine-learning-create-experiment/evaluation-results.png
[complete-linear-regression-experiment]:./media/machine-learning-create-experiment/complete-linear-regression-experiment.png

<!-- temporarily switching GIFs tooPNGs tooremove animation --> 
[type-automobile]:./media/machine-learning-create-experiment/type-automobile.png
[type-select-columns]:./media/machine-learning-create-experiment/type-select-columns.png
[launch-column-selector]:./media/machine-learning-create-experiment/launch-column-selector.png
[add-comment]:./media/machine-learning-create-experiment/add-comment.png
[connect-clean-to-select]:./media/machine-learning-create-experiment/connect-clean-to-select.png

[set-split-data-percentage]:./media/machine-learning-create-experiment/set-split-data-percentage.png

<!-- temporarily switching GIFs tooPNGs tooremove animation --> 
[connect-train-model]:./media/machine-learning-create-experiment/connect-train-model.png
[select-price-column]:./media/machine-learning-create-experiment/select-price-column.png

[score-model-output]:./media/machine-learning-create-experiment/score-model-output.png

<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
