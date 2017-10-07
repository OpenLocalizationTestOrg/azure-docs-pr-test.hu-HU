---
title: "az Azure Machine Learning modell aaaHow válik egy webszolgáltatás-bővítmény |} Microsoft Docs"
description: "Hogyan az Azure Machine Learning modell történik a fejlesztői kísérletezhet tooan hello idejéről áttekintést operationalized webszolgáltatáshoz."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 25e0c025-f8b0-44ab-beaf-d0f2d485eb91
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: 2bbe09fa8fa22662cf8ec4a8b6249d23c87c8ddf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-a-machine-learning-model-progresses-from-an-experiment-tooan-operationalized-web-service"></a>Hogyan egy gépi tanulási modell történik, egy kísérlet tooan a operationalized webszolgáltatás
Az Azure Machine Learning Studio biztosít egy interaktív vászonra, amely lehetővé teszi toodevelop, futtassa, tesztelése és többször egy ***kísérletezhet*** képviselő prediktív elemzési modellek. Nincsenek modullistából, amelyek számos:

* A bemeneti adatok kísérletbe
* Kezelhetők hello adatok
* A gépi tanulási algoritmusok használata modell betanításához
* Pontszám hello modell
* Hello eredmények értékelése
* Kimeneti végső értékek

Ha elégedett a kísérletet, telepítheti azt egy ***klasszikus Azure Machine Learning webszolgáltatás*** vagy egy ***új Azure Machine Learning webszolgáltatás*** , hogy a felhasználók új adatok elküldi és hátsó eredményeket.

A cikkben amelyben tudatjuk a felhasználókkal hello idejéről hogyan a gépi tanulási modell időtartamára egy fejlesztési kísérlet tooan a operationalized webes szolgáltatás áttekintését.

> [!NOTE]
> Nincsenek más módokon toodevelop és központi telepítése a machine learning modellek, de ez a cikk a Machine Learning Studio használatának összpontosít. Például hogyan toocreate r, a klasszikus prediktív webszolgáltatás hello ismertető blogbejegyzésben talál leírása tooread [Build & telepítése prediktív Web Apps használatával Rstudióból és Azure ML](http://blogs.technet.com/b/machinelearning/archive/2015/09/25/build-and-deploy-a-predictive-web-app-using-rstudio-and-azure-ml.aspx).
> 
> 

Míg az Azure Machine Learning Studio tervezett toohelp fejlesztésekor és telepítésekor egy *prediktív elemzési modellek*, lehetséges toouse Studio toodevelop a kísérlet, amely nem tartalmazza a prediktív elemzési modellek. Például egy kísérlet előfordulhat, hogy csak a bemeneti adatokat, és kezelése céljából, majd kimeneti hello eredmények. Csakúgy, mint egy prediktív elemzési kísérletet telepítheti a nem prediktív kísérletté webszolgáltatásként, de egy egyszerűbb folyamat mert hello kísérlet nem betanítása vagy egy gépi tanulási modell pontozása. Bár ez nem hello tipikus toouse Studio ily módon, azt fogja foglalja bele hello vitafórum, hogy egy teljes magyarázat Studio működését is felállításához.

## <a name="developing-and-deploying-a-predictive-web-service"></a>Fejlesztéséhez és telepítéséhez egy prediktív webszolgáltatás-bővítmény
Az alábbiakban hello fázisból áll, amely a szokásos megoldás fejlesztése és a Machine Learning Studio használatával telepítse a következő:

![Üzembe helyezési folyamat](media/machine-learning-model-progression-experiment-to-web-service/model-stages-from-experiment-to-web-service.png)

*1. ábra – egy tipikus prediktív elemzési modellek szakaszainak*

### <a name="hello-training-experiment"></a>hello tanítási kísérletet
Hello ***tanítási kísérletet*** hello kezdeti fázisa fejlesztése a webszolgáltatás a Machine Learning Studióban. hello hello tanítási kísérletet célja toogive, egy hely toodevelop tesztelése, többször, és végül a machine learning-modell betanításához. Akkor is még betanítása több modellek egyidejűleg hello legjobban megfelelő megoldást keres, de miután befejezte a kísérletezés, választania betanítása egyetlen modell, és kiszűri a hello rest hello kísérlet. Például egy prediktív elemzési kísérletet fejlődő, [a az Azure Machine Learning hitelkockázat értékelésére szolgáló prediktív elemzési megoldás fejlesztése](machine-learning-walkthrough-develop-predictive-solution.md).

### <a name="hello-predictive-experiment"></a>hello prediktív kísérletté
Miután a betanított modell a tanítási kísérletet a, kattintson **webes szolgáltatások beállítása** válassza **prediktív webszolgáltatás** Machine Learning Studio tooinitiate hello folyamat átalakítani a képzés tooa kísérletezhet ***prediktív kísérletté***. hello hello prediktív kísérletté célja van toouse a betanított modell tooscore új adatok, végül Azure webszolgáltatásként váljon operationalized hello célját.

A konvertáláshoz a lépéseket követve hello segítségével történik meg:

* Alakítsa át egyetlen modulba képzési használt modulok hello készletét, és mentse a betanított modell
* Nem kapcsolódó idegen modul kiküszöbölése tooscoring
* Vegye fel a bemeneti és kimeneti hello végleges webes szolgáltatás által használt portokról

Előfordulhat, hogy további módosításokat toomake tooget a prediktív kísérletté készen toodeploy webszolgáltatásként. Például ha azt szeretné, hogy hello Web service toooutput eredmények csak egy részét, hozzáadhat egy szűrési modult hello kimeneti port előtt.

Az átalakítási folyamat során nem távolítja el hello tanítási kísérletet. Ha hello folyamat befejeződik, két lap van-e a Studio: egy hello tanítási kísérletet, egy, a prediktív kísérletté hello. Ily módon, hogy módosíthassa toohello tanítási kísérletet hello prediktív kísérletté újraépítéséhez és a webszolgáltatás telepítése előtt. Vagy mentheti hello tanítási kísérletet toostart másolatát kísérletezhet folytatását.

> [!NOTE]
> Amikor rákattint **prediktív webszolgáltatás** indítja el az automatikus folyamat tooconvert a tanítási kísérletet tooa prediktív kísérletet, és a legtöbb esetben működik jól. Ha a tanítási kísérletet összetett (például, hogy együtt belépni képzési több elérési út), előfordulhat, hogy inkább toodo az átalakításhoz manuálisan. További információkért lásd: [hogyan tooprepare a modell Azure Machine Learning Studio-KözpontiTelepítés](machine-learning-convert-training-experiment-to-scoring-experiment.md).
> 
> 

### <a name="hello-web-service"></a>hello webszolgáltatás
Ha készen, hogy a prediktív kísérletté készen áll, a szolgáltatás vagy egy klasszikus webszolgáltatás telepítése vagy egy új webszolgáltatás-alapú Azure Resource Manager. toooperationalize telepítésével, mint a modell egy *klasszikus Machine Learning webszolgáltatás*, kattintson a **webes szolgáltatás telepítése** válassza **webes szolgáltatás telepítése [klasszikus]**. mint toodeploy *új Machine Learning webszolgáltatás*, kattintson a **webes szolgáltatás telepítése** válassza ki **[Új] webes szolgáltatás telepítése**. Felhasználók ezután hello webes szolgáltatás REST API használatával tooyour adatmodell küldhet és hátsó hello eredményeket. További információkért lásd: [hogyan tooconsume az Azure Machine Learning Web service](machine-learning-consume-web-services.md).

## <a name="hello-non-typical-case-creating-a-non-predictive-web-service"></a>nem általában ez a helyzet hello: nem prediktív webes szolgáltatás létrehozása
Ha kísérletbe nem végezzük prediktív elemzési modellt, majd nem szükséges toocreate egy tanítási kísérletet és pontozási kísérlet is – csak egy kísérlet van, és telepítheti azt egy webszolgáltatás. A Machine Learning Studio észleli, hogy tartalmaz-e a kísérletet a prediktív modell elemzésével hello modulok már használta.

Miután a kísérletről többször is, és elégedett azt:

1. Kattintson a **webes szolgáltatások beállítása** válassza **webszolgáltatás Átképezési** – a bemeneti és kimeneti csomópontokat ad hozzá automatikusan
2. Kattintson a **futtatása**
3. Kattintson a **webes szolgáltatás telepítése** válassza **webes szolgáltatás telepítése [klasszikus]** vagy **[Új] webes szolgáltatás telepítése** attól függően, hogy hello környezet toowhich toodeploy kívánt.

A webes szolgáltatás már telepítve van, és elérheti és hasonlóan egy prediktív webszolgáltatás-bővítmény kezeli azt.

## <a name="updating-your-web-service"></a>A webes szolgáltatás frissítése
Most, hogy a kísérletben webszolgáltatásként telepítése után, mi történik, ha szüksége tooupdate azt?

Mit kell függ tooupdate:

**Azt szeretné, hogy toochange hello bemenete vagy kimenete, vagy azt szeretné, hogy toomodify hogyan hello webszolgáltatás adatokat kezel**

Ha épp nem módosított hello modell, de csak változnak, hogyan kezeli a hello webszolgáltatás az adatokat, hello prediktív kísérletté szerkesztése, és kattintson a **webes szolgáltatás telepítése** válassza **webes szolgáltatás telepítése [klasszikus]** vagy **[Új] webes szolgáltatás telepítése** újra. hello webes szolgáltatás le van állítva, hello frissített prediktív kísérletté van telepítve, és hello webes szolgáltatás újraindul.

Példa: Tegyük fel, hogy a prediktív kísérletté hello teljes sor a hello bemeneti adatokat adja vissza eredményt előre jelezni. Dönthet úgy, hogy szeretné-e hello Web service toojust visszatérési hello eredménye. Így adhat hozzá egy **Projektoszlopok** hello prediktív kísérletté moduljának jobb hello kimeneti portra, mielőtt tooexclude oszlopok eltérő hello vezethet. Elemre **webes szolgáltatás telepítése** válassza **webes szolgáltatás telepítése [klasszikus]** vagy **[Új] webes szolgáltatás telepítése** újra, a webszolgáltatás hello frissül.

**Azt szeretné, hogy az új adatokat tooretrain hello modell**

Ha a gépi tanulási a modell tookeep kívánt, de azt szeretné, hogy tooretrain az új adatokkal, két lehetősége van:

1. **Hello modell újratanítása hello webes szolgáltatás futása közben** -Ha tooretrain a modell hello prediktív webes szolgáltatás futása közben, ehhez azáltal, hogy néhány módosítás toohello tanítási kísérletet toomake azt egy *** kísérlet átképezési***, majd telepítheti azt egy  ***megőrzési webes* szolgáltatás**. Útmutatást toodo a, lásd: [Machine Learning-modellek szoftveres](machine-learning-retrain-models-programmatically.md).
2. **Lépjen vissza toohello eredeti tanítási kísérletet, és a modellt használnak a különböző betanítási adatok toodevelop** – a prediktív kísérletté csatolt toohello webes szolgáltatás, de ezzel a módszerrel nem közvetlenül kapcsolódó hello tanítási kísérletet. Ha módosítja a hello eredeti tanítási kísérletet, és kattintson **webes szolgáltatások beállítása**, az létrehoz egy *új* prediktív kísérletezhet, amellyel telepítésekor létrehoz egy *új* webes a szolgáltatás. Csak hello eredeti webszolgáltatás nem frissíti.
   
   Ha toomodify hello tanítási kísérletet, nyissa meg azt, és kattintson **Mentés másként** toomake másolatát. Így marad, nem sérültek hello eredeti tanítási kísérletet, a prediktív kísérletté és a webes szolgáltatás. Létrehozhat egy új webszolgáltatás-bővítmény most a módosításokat. Hello ezután új webes szolgáltatás telepítése után e toostop előző webszolgáltatás hello, vagy újat hello mellett működése.

**Azt szeretné, hogy egy másik modellhez tootrain**

Ha azt szeretné, hogy a toomake módosítja tooyour eredeti prediktív kísérletté, például a különböző gépi tanulási algoritmus kiválasztása próbált egy másik képzési metódust, stb., akkor szüksége toofollow hello második eljárás a modell átképezési fent ismertetett: Nyissa meg a hello tanítási kísérletet, kattintson **Mentés másként** toomake másolatát, és le hello a modell fejleszt, hello prediktív kísérletté létrehozása és telepítése az új elérési utat, majd elindítására hello webszolgáltatáshoz. Ezzel létrehoz egy új független toohello egy-egy webszolgáltatás - eldöntheti, hogy mely futó egyik vagy mindkét, tookeep.

## <a name="next-steps"></a>Következő lépések
Hello folyamat fejlesztése és kísérlet a további részletekért lásd: a következő cikkek hello:

* hello kísérlet - átalakítás [hogyan tooprepare a modell Azure Machine Learning Studio-KözpontiTelepítés](machine-learning-convert-training-experiment-to-scoring-experiment.md)
* Deploying hello webszolgáltatás - [az Azure Machine Learning webszolgáltatás telepítése](machine-learning-publish-a-machine-learning-web-service.md)
* megőrzési hello modell - [Machine Learning-modellek szoftveres](machine-learning-retrain-models-programmatically.md)

A teljes folyamat hello című részben talál példákat:

* [Machine learning oktatóanyag: az első kísérlet létrehozása az Azure Machine Learning Studióban](machine-learning-create-experiment.md)
* [Forgatókönyv: A hitelkockázat értékelésére az Azure Machine Learning a prediktív elemzési megoldás fejlesztése](machine-learning-walkthrough-develop-predictive-solution.md)

