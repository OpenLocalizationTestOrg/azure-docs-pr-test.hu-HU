---
title: "aaaA prediktív megoldás a hitelkockázat kiszámításához a Machine Learning |} Microsoft Docs"
description: "Részletes útmutató, amely hogyan toocreate egy prediktív elemzési megoldás a hitelkockázat kockázatbecslés az Azure Machine Learning Studióban."
keywords: "hitelkockázat, prediktív elemzési megoldás,kockázatértékelés"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 43300854-a14e-4cd2-9bb1-c55c779e0e93
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 00ed39081e6952b8d03b37a8285d8dc3ddff2cb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a>Részletes útmutató: A hitelkockázat értékelésére szolgáló prediktív elemzési megoldás fejlesztése az Azure Machine Learning Studio használatával

Ebben a bemutatóban vesszük egy kiterjesztett nézze meg a Machine Learning Studio prediktív elemzési megoldás fejlesztése hello folyamatán. Azt a egyszerű modellezése a Machine Learning Studióban, és telepítheti az Azure Machine Learning webszolgáltatás, ahol hello modell is előrejelzésekhez új adatokkal. 

Az útmutatóban leírtak abból indulnak ki, hogy legalább egyszer használta már a Machine Learning Studiót, és hogy valamennyire tisztában van a gépi tanulás fogalmaival. Az útmutató azonban nem feltételezi, hogy a fent említett területeken szakértő lenne.

Ha soha nem használt **Azure Machine Learning Studio** előtt érdemes a hello oktatóanyagban toostart [létrehozása az Azure Machine Learning Studióban kísérletezhet az első adattudomány](machine-learning-create-experiment.md). Hogy az oktatóanyag végigvezeti a Machine Learning Studio hello az első alkalommal. Ez megjeleníti a alakzatot kísérletbe, hogyan toodrag és vidd modulok hello alapjait összekapcsolhatja őket, hello kísérlet futtatása, és hello eredmények tekintse meg. Egy másik eszköz, amelyek segíthetnek az első lépések egy diagram, amely áttekintést nyújt a Machine Learning Studio funkcióiról hello. Ezt a következő helyről töltheti le és nyomtathatja ki: [Az Azure Machine Learning Studio képességeit áttekintő diagram](machine-learning-studio-overview-diagram.md).
 
Ha új toohello mezőt a gépi tanulási általában, van egy videósorozat, amely hasznos tooyou lehet. Azt nevezzük [Adattudomány kezdőknek](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) és azt tudhatja meg egy nagyszerű bemutatása toomachine tanulási mindennapos és fogalmak.


[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
 

## <a name="hello-problem"></a>hello probléma

Tegyük fel, hogy egy személy hitelkockázatáról azok hitelkérelemben megadott hello adatok alapján toopredict van szüksége.  

A hitelkockázat-értékelés összetett probléma, de az útmutató kedvéért leegyszerűsítjük egy kicsit. Ezután ezt példaként használjuk annak bemutatására, hogyan hozható létre prediktív elemzési megoldás a Microsoft Azure Machine Learning segítségével. toodo az Azure Machine Learning Studio és a gépi tanulás webszolgáltatás használjuk.  

## <a name="hello-solution"></a>hello megoldás

Ebben a részletes útmutatóban nyilvánosan elérhető hitelkockázati adatokkal fogunk dolgozni, amelyek alapján kifejlesztünk és betanítunk egy prediktív modellt. Ezután azt modell rendszerbe állítása hello webszolgáltatásként, használat mások számára a hitelkockázat értékelésére.

toocreate a hitelkockázat-értékelési megoldás, hogy kövesse az alábbi lépéseket:  

1. [Machine Learning-munkaterület létrehozása](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Meglévő adatok feltöltése](machine-learning-walkthrough-2-upload-data.md)
3. [Kísérlet létrehozása](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Betanítása és kiértékelése hello modellek](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Hello webszolgáltatás telepítése](machine-learning-walkthrough-5-publish-web-service.md)
6. [Hello webes szolgáltatás](machine-learning-walkthrough-6-access-web-service.md)

> [!TIP] 
> Ez a forgatókönyv a hello hello kísérlet, amely azt fejlesztése a működő példány található [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com). Nyissa meg túl**[forgatókönyv - jóváírási kockázat előrejelzés](https://gallery.cortanaintelligence.com/Experiment/Walkthrough-Credit-risk-prediction-1)**  kattintson **Megnyitás a Studióban** toodownload hello kísérletben a Machine Learning Studio munkaterületre másolatát.
> 
> Ez a forgatókönyv hello mintakísérletet, egyszerűsített verzióján alapul [bináris osztályozás: Credit kockázat előrejelzés](http://go.microsoft.com/fwlink/?LinkID=525270)hello is elérhető, [gyűjtemény](http://gallery.cortanaintelligence.com/).
