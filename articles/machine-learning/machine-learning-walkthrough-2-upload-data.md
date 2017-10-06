---
title: "2. lépés: A gépi tanulási kísérlet az adatok feltöltése |} Microsoft Docs"
description: "Hello 2. lépés fejlesztése egy prediktív megoldás forgatókönyv: feltöltés nyilvános adatok tárolása az Azure Machine Learning Studio."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 9f4bc52e-9919-4dea-90ea-5cf7cc506d85
ms.service: machine-learning
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 0ea21dcca2d0934ed06508560cf85cf31b48ce6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-2-upload-existing-data-into-an-azure-machine-learning-experiment"></a>Az útmutató 2. lépése: A meglévő adatok feltöltése egy Azure Machine Learning-kísérletbe
Ez az hello második lépése annak hello forgatókönyv [az Azure Machine Learning a prediktív elemzési megoldás fejlesztése](machine-learning-walkthrough-develop-predictive-solution.md)

1. [Machine Learning-munkaterület létrehozása](machine-learning-walkthrough-1-create-ml-workspace.md)
2. **Meglévő adatok feltöltése**
3. [Új kísérlet létrehozása](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Betanítása és kiértékelése hello modellek](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Hello webszolgáltatás telepítése](machine-learning-walkthrough-5-publish-web-service.md)
6. [Hozzáférés hello webszolgáltatás](machine-learning-walkthrough-6-access-web-service.md)

- - -
toodevelop a hitelkockázat kiszámításához a prediktív modellt, igazolnia kell, hogy azt tootrain használja, és tesztelje a hello modell adatokat. Ez a forgatókönyv az adattárból hello Egyediségi Irvine Machine Learning "UCI Statlog (német jóváírás adatok) adatkészlet" hello fogjuk használni. Megtalálja itt:  
<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://Archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)</a>

Hello fájlt fogjuk használni **german.data**. Töltse le a fájl tooyour helyi merevlemez-meghajtóról.  

Hello **german.data** dataset 20 változók sorokat tartalmaz-jóváírási 1000 múltbeli kérelmező esetében. 20 változókhoz képviselő hello dataset számos funkciót (hello *szolgáltatás vektoros*), amely lehetővé teszi azonosító jellemzőinek minden jóváírás kérelmező. Minden sor egy olyan további oszlop hello kérelmező számított hitelkockázat, a magas kockázatú alacsony hitelkockázat és 300 azonosítottak 700 kérelmezők jelöli.

hello UCI webhely hello attribútumok hello szolgáltatás vektor ezeket az adatokat ismerteti. Ez magában foglalja az információkat, jóváírás előzmények, állapota és a személyes adatokat. Az egyes kérelmező bináris minősítést volt adott, amely azt jelzi, hogy azok az alacsony vagy magas kockázatú követel. 

Az adatok tootrain egy prediktív elemzési modellt fogjuk használni. Azt, a modell kell kell tudni tooaccept a szolgáltatás vektor egy új egyedi, és hogy nem magas vagy alacsony hitelkockázat előrejelzése.  

Íme egy érdekes formája. mennyi költséggel Ha azt egy személy hitelkockázat misclassify hello hello adatkészletre hello UCI webhely megjegyzések a leírását.
Ha hello modell magas hitelkockázat előrejelzi a rendszer ténylegesen alacsony hitelkockázat, hello modell tett egy téves besorolás.
De hello fordított téves besorolás ötször költségesebb toohello egy olyan pénzügyi intézménynél: Ha hello modell alacsony hitelkockázat előrejelzi mások számára ténylegesen hitelkockázati kockázatot jelent.

Igen azt szeretnénk, ha tootrain tekinthetők, hogy ez utóbbi típusú téves besorolás hello költségét ötször magasabb, mint más módon hello misclassifying.
Egy egyszerű módon toodo ezt, ha a kísérletben hello modell betanítása van (ötször) azokra a bejegyzésekre, amelyek megfelelnek egy magas hitelkockázat rendelkező bármely személy másolásával. Ezt követően hello modell misclassifies valaki alacsony hitelkockázat, ha azok ténylegesen nagy kockázatot jelent, ha hello modell elvégzi, hogy ugyanazon téves besorolás ötször, egyszer minden ismétlődő. Ez növeli a hello képzési eredmények hibára hello költségét.


## <a name="convert-hello-dataset-format"></a>Átalakítás hello dataset formátumban
hello eredeti adathalmazból egy üres elválasztott formátumot használja. A Machine Learning Studio jobban működik egy vesszővel tagolt (CSV) fájl, így nem lesz átalakítás hello dataset azáltal, hogy szóközök vesszővel válassza el egymástól.  

Nincsenek számos módon tooconvert ezeket az adatokat. Egyirányú van hello a következő Windows PowerShell-parancs használatával:   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

Egy másik módja hello Unix csökkentésének parancs használatával:  

    sed 's/ /,/g' german.data > german.csv  

Mindkét esetben létrehoztunk egy vesszővel tagolt verzió hello adatok nevű fájlba **german.csv** , hogy a kísérletben is használhatók.

## <a name="upload-hello-dataset-toomachine-learning-studio"></a>Hello dataset tooMachine Learning Studio feltöltése
Hello adatok konvertált tooCSV formátum követően tooupload kell azt a Machine Learning Studióhoz. 

1. Nyissa meg hello Machine Learning Studio kezdőlap ([https://studio.azureml.net](https://studio.azureml.net)). 

2. Hello menüjét ![menü][1] hello ablak hello bal felső sarkában kattintson **Azure Machine Learning**, jelölje be **Studio**, és jelentkezzen be.

3. Kattintson a **+ új** hello ablak hello alján.

4. Válassza ki **DATASET**.

5. Válassza ki **helyi FÁJLBÓL**.

    ![A DataSet adatkészlet hozzáadása a helyi fájlból][2]

6. A hello **töltse fel az új adatkészlet** párbeszédpanel, kattintson a **Tallózás** és hello található **german.csv** létrehozott fájlt.

7. Adja meg a hello adatkészlet nevét. Ennél a bemutatónál neki "UCI német hitelkártya adatok".

8. Adattípus, válassza ki a **fejléc nélküli általános CSV-fájlt (. nh.csv)**.

9. Ha azt szeretné, adjon meg egy leírást.

10. Kattintson a hello **OK** pipára.  

    ![Hello dataset feltöltése][3]

Ez fájlfeltöltések hello adatokat is használhatók. a kísérlet a dataset modulba.

Kezelheti, hogy hello kattintva tooStudio feltöltött adathalmazok **ADATKÉSZLETEK** lapon toohello hello Studio ablak bal oldali.

![Adatkészletek kezelése][4]

Más típusú adatok kísérlet történő importálásával kapcsolatos további információkért lásd: [a betanítási adatok importálása az Azure Machine Learning Studio](machine-learning-data-science-import-data.md).

**Következő: [új kísérlet létrehozása](machine-learning-walkthrough-3-create-new-experiment.md)**

[1]: media/machine-learning-walkthrough-2-upload-data/menu.png
[2]: media/machine-learning-walkthrough-2-upload-data/add-dataset.png
[3]: media/machine-learning-walkthrough-2-upload-data/upload-dataset.png
[4]: media/machine-learning-walkthrough-2-upload-data/dataset-list.png
