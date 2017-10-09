---
title: aaaWhat az Azure Machine Learning Studio? | Microsoft Docs
description: "Az Azure ML Studio áttekintése. Az Azure ML Studio olyan, egérrel kezelhető eszköz, amellyel egy használatra kész algoritmus- és modultárból gyorsan felépíthetők a modellek."
keywords: azure machine learning,azure ml, ml studio
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: e65c8fe1-7991-4a2a-86ef-fd80a7a06269
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: a25f2d9e75d088a37930de98240c6e14c9567fa4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-machine-learning-studio"></a>Mi az Azure Machine Learning Studio?
A Microsoft Azure Machine Learning Studio egy olyan együttműködést támogató, egérrel húzási és elengedési eszköz használhatja toobuild, tesztelését és rendszerbe adataihoz prediktív elemzési megoldások. A Machine Learning Studio a modelleket webszolgáltatásként teszi közzé, amelyeket az egyéni alkalmazások vagy az Excel és más üzletiintelligencia-eszközök egyszerűen felhasználhatnak.

A Machine Learning Studio találkozási pontot biztosít az adatelemzés, a prediktív elemzés, a felhőerőforrások és az Ön adatai számára.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="hello-machine-learning-studio-interactive-workspace"></a>hello Machine Learning Studio interaktív munkaterülete
a prediktív elemzési modellek toodevelop, általában használja adatok egy vagy több forrásból, átalakítás és elemzése adatokat veszünk különböző adatkezelési és statisztikai függvények és az eredmények eredményhalmazt. Az ilyen modellek fejlesztése iteratív folyamat. Addig módosítjuk hello különböző függvényeket és paramétereket, az eredményeket addig közelítjük, amíg úgy nem véljük, hogy rendelkezik-e a betanított, hatékony modellel rendelkezünk.

**Az Azure Machine Learning Studio** által biztosított egy interaktív, grafikus munkaterületet tooeasily létrehozása, tesztelése és a prediktív elemzési modellek többször. Ön fogd és vidd ***adatkészletek*** és elemzések ***modulok*** egy interaktív vászonra csatlakoztatja őket együtt tooform egy ***kísérletezhet***, amely futtatja a Machine Learning Studio. a modell tervezésével tooiterate, akkor hello kísérletet, mentse el szükség esetén szerkessze, és indítsa újra. Ha készen áll, alakíthatja a ***tanítási kísérletet*** tooa ***prediktív kísérletté***, és tegye közzé azt egy ***webszolgáltatás*** , hogy elérhető legyen a modell mások számára.

Nincs szükség programozásra, csupán grafikus összekapcsolására a prediktív elemzési modell az adathalmazokat és modulokat tooconstruct van.

> [!TIP]
> Lásd a toodownload és nyomtatási ábrázoló diagram, amely áttekintést nyújt a Machine Learning Studio funkcióiról hello [Azure Machine Learning Studio képességeit áttekintő diagram](machine-learning-studio-overview-diagram.md).
> 
> 

![Az Azure ML Studio diagramja: kísérletek létrehozása, adatok beolvasása számos forrásból, pontozott adatok és modellek írása.][ml-studio-overview]

## <a name="get-started-with-machine-learning-studio"></a>A Machine Learning Studio használatának első lépései
Amikor először beírja [Machine Learning Studio](https://studio.azureml.net) hello látja **Home** lap. Innen kiindulva megtekintheti a dokumentációt, valamint videókat, webes előadások és más hasznos forrásokat érhet el.

Hello bal felső menüben ![Menü](media/machine-learning-what-is-ml-studio/menu.png) amelyben számos lehetőséget fog látni.

### <a name="cortana-intelligence"></a>Cortana Intelligence
Kattintson a **Cortana Intelligence** és fog tenni toohello kezdőlapján hello [Cortana Intelligence Suite](https://www.microsoft.com/cloud-platform/cortana-intelligence-suite). hello Cortana Intelligence Suite egy teljes körűen felügyelt big Data típusú adatok, és az adatok analytics suite tootransform speciális intelligens működésbe. Lásd: hello Suite kezdőlap teljes dokumentációja, beleértve az ügyfél szövegek.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Itt, két lehetőség **Home**, hello lap eredeti állapotába, és **Studio**.

Kattintson a **Studio** és fog tenni toohello **Azure Machine Learning Studio**. Először meg kell adnia a Microsoft-fiókját, vagy a munkahelyi vagy iskolai fiókjával toosign. Miután bejelentkezett, megjelenik a következő lapokon hello bal oldali hello:

* **PROJEKTEK** – Az egyes projekteket alkotó kísérletek, adatkészletek, jegyzetek és egyéb erőforrások gyűjteményei
* **KÍSÉRLETEK** – A létrehozott és futtatott vagy vázlatként mentett kísérletek
* **WEBSZOLGÁLTATÁSOK** – A kísérletekből üzembe helyezett webszolgáltatások
* **NOTEBOOKOK** – A létrehozott Jupyter notebookok
* **ADATHALMAZOK** – A Studióba feltöltött adathalmazok
* **BETANÍTOTT MODELLEK** – A kísérletek során betanított és a Studio eszközbe mentett kísérletek
* **BEÁLLÍTÁSOK** -beállítások használható tooconfigure fiókja és -erőforrások gyűjteménye.

### <a name="gallery"></a>Katalógus
Kattintson a **gyűjtemény** és fog tenni toohello  **[Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/)**. hello gyűjtemény egy olyan hely, ahol az adatelemzők és fejlesztők közösségi megosztás hello Cortana Intelligence Suite összetevői használatával létrehozott megoldásokat.

Gyűjteményelem hello kapcsolatos további információkért lásd: [megosztást, és felderítik a Cortana Intelligence Gallery hello megoldások](machine-learning-gallery-how-to-use-contribute-publish.md).

## <a name="components-of-an-experiment"></a>A kísérletek összetevői
Egy adathalmazokból állnak, amelyek biztosítanak az adatok tooanalytical modulokat, amelyek tooconstruct prediktív elemzési modellek. Az érvényes kísérletek a következő jellemzőkkel rendelkeznek:

* hello kísérlet tartalmaz legalább egy adathalmazt és egy modult
* Lehet, hogy adatkészletek csatlakoztatott csak toomodules
* Lehet, hogy modulok csatlakoztatott tooeither adathalmazokhoz és más modulokhoz
* Az összes bemeneti portok modulok rendelkeznie kell néhány kapcsolat toohello adatok folyamata
* Minden modul összes szükséges paraméterét meg kell adni

A kísérleteket létrehozhatja nulláról, vagy egy meglévő mintakísérletet sablonként használva További információkért lásd: [Copy példa kísérleteket toocreate új gépi tanulási kísérletekhez](machine-learning-sample-experiments.md).

Egy egyszerű kísérlet létrehozására láthat példát az [Egyszerű kísérlet létrehozása az Azure Machine Learning Studio eszközben](machine-learning-create-experiment.md) című cikkben.

A prediktív elemzési megoldások létrehozásának részletesebb leírásáért tekintse meg a [Prediktív megoldás kifejlesztése az Azure Machine Learning segítségével](machine-learning-walkthrough-develop-predictive-solution.md) című cikket.

### <a name="datasets"></a>Adathalmazok
Az adathalmaz olyan ennyi ideig feltöltött tooMachine Learning Studióban, hogy a folyamat modellezési hello is használható. Számos mintaként használható adathalmazt megtalálhatóak a Machine Learning Studio a tooexperiment, és szükség esetén további adathalmazokat is feltölthet. Néhány példa a mintaadathalmazokra:

* **Különböző autók fogyasztási adatai** – Hengerszám, lóerő stb. alapján azonosított autók üzemanyag-fogyasztási adatai.
* **Mellrákkal kapcsolatos adatok** – Mellrák-diagnosztikai adatok.
* **Erdőtüzek adatai** – Az Északkelet-Portugáliában előfordult erdőtüzek kiterjedése.

A kísérlet létrehozása választhat adatkészletek rendelkezésre hello listája toohello maradt a vásznon a hello.

A Machine Learning Studio mintaadathalmazainak listájáért lásd: [hello mintaadatkészletek használata az Azure Machine Learning Studióban](machine-learning-use-sample-datasets.md).

### <a name="modules"></a>Modulok
A modulok az adatokon végezhető algoritmusok. A Machine Learning Studio számos modult adatok érkező funkciók tootraining, pontozási és ellenőrzési folyamatok közötti rendelkezik. Néhány példa a mellékelt modulokra:

* [Átalakítás tooARFF] [ convert-to-arff] -konvertálja a .NET szerializált adatkészlet-kapcsolat tooAttribute fájlformátumra (ARFF).
* [Compute Elementary Statistics][elementary-statistics] (Alapvető statisztikai számítások) – Alapvető statisztikai számítások, például átlag, szórás stb. kiszámítása.
* [Linear Regression][linear-regression] (Lineáris regresszió) – Online, grádiens módszeren alapuló lineáris regressziós modell létrehozása.
* [Score Model][score-model] (Pontszámmodell) – Betanított osztályozási vagy regressziós modell pontozása.

A kísérlet létrehozása választhat modullistából hello listája toohello maradt a vásznon a hello.  

A modul rendelkezhet, amelyeket felhasználhat tooconfigure hello belső algoritmusok paraméterek. Amikor kiválaszt egy modult a vásznon a hello, hello modul paraméterei megjelennek-e hello **tulajdonságok** ablaktábla toohello jog a vásznon a hello. Módosíthatja az adott ablaktábla tootune hello paraméterek a modell.

Való eligazodást segíti hello gazdag tára álló gépi tanulási algoritmusok érhető el, lásd: [hogyan toochoose algoritmusok használata a Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md).

## <a name="deploying-a-predictive-analytics-web-service"></a>A prediktív elemzési webszolgáltatások telepítése
Ha elkészült a prediktív elemzési modell, közvetlenül a Machine Learning Studio eszközből üzembe helyezheti webszolgáltatásként. A folyamattal kapcsolatos további információkért tekintse meg az [Azure Machine Learning webszolgáltatás üzembe helyezése](machine-learning-publish-a-machine-learning-web-service.md) című cikket.

[ml-studio-overview]:./media/machine-learning-what-is-ml-studio/azure-ml-studio-diagram.jpg

<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
