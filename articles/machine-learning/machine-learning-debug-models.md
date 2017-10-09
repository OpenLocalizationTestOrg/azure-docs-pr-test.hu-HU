---
title: aaaDebug az az Azure Machine Learning modellje |} Microsoft Docs
description: "Hogyan toodebug által visszaadott hibaüzenetek tanítási és pontszám modell Azure Machine Learning modulok."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 629dc45e-ac1e-4b7d-b120-08813dc448be
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bradsev;garye
ms.openlocfilehash: ee38ca8ce38d4fc7add5ba70c80ab9bb2ceaf1d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="debug-your-model-in-azure-machine-learning"></a>A modell hibáinak keresése az Azure Machine Learning rendszerben

Ez a cikk azt ismerteti, miért vagy a következő két hibák hello szembesülhet modell futtatásakor hello lehetséges okok:

* Hello [tanítási modell] [ train-model] modul hibát hoz létre. 
* Hello [Score Model] [ score-model] modul helytelen eredményt ad. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="train-model-module-produces-an-error"></a>Train Model-modul hibát hoz létre.

![image1](./media/machine-learning-debug-models/train_model-1.png)

Hello [tanítási modell] [ train-model] modul vár a két bemenet:

1. gépi tanulási modellt az Azure Machine Learning által biztosított modellek hello gyűjteményből hello típusa.
2. hello egy megadott címke oszlop, amely megadja a betanítási adatok hello változó toopredict (hello más oszlopok feltételezhetően toobe funkciók).

Ez a modul hibát jelez a következő esetekben hello hozhat létre:

1. hello címke oszlop helytelenül van megadva. Ez akkor fordulhat elő, ha egynél több oszlop van kijelölve, címke hello, vagy helytelen index van kiválasztva. Például hello második esetben akkor vonatkozik, ha egy oszlop indexe 30 csak 25 oszlopot egy bemeneti adatkészletet használják.

2. hello a dataset nem tartalmaz szolgáltatás oszlopot. Például ha hello bemeneti dataset adatkészletben csak egy oszlop, amely hello címke oszlop van megjelölve, nem lenne nem szolgáltatások mely toobuild hello modellt. Ebben az esetben hello [tanítási modell] [ train-model] modul hibát eredményez.

3. hello bemeneti adatkészletet (szolgáltatások vagy címke) végtelen értéket tartalmazza.

## <a name="score-model-module-produces-incorrect-results"></a>Score Model-modul helytelen eredményt ad.

![image2](./media/machine-learning-debug-models/train_test-2.png)

Egy tipikus képzési/tesztelési kísérletben a felügyelt tanítás során, hello [Split Data] [ split] modul két részre oszt hello eredeti adathalmazból: egyrészről használt tootrain hello modell, és egy rész szolgál tooscore milyen jól betanított modell hello hajt végre. hello betanított modell nem használt tooscore hello tesztadatok, mely után hello eredményei hello modell kiértékelt toodetermine hello pontosságát.

Hello [Score Model] [ score-model] modul két bemeneti értéket igényel:

1. A betanított modell kimenete hello [tanítási modell] [ train-model] modul.
2. Hello dataset eltérő pontozási dataset tootrain hello modellt használja.

Azt a lehetséges, hogy annak ellenére, hogy hello kísérlet sikeres, hello [Score Model] [ score-model] modul helytelen eredményt ad. Több forgatókönyvben okozhat a toohappen:

1. Hello megadott címke kategorikus, és egy regressziós modell betanítása hello adatokon, ha egy hibás kimeneti volna állítja hello [Score Model] [ score-model] modul. Ez azért van így, mert regressziós megköveteli a folyamatos válasz változó. Ebben az esetben a besorolási modell megfelelőbbek toouse lenne. 

2. Hasonlóképpen, ha egy besorolási modell betanítása egy adatkészlethez, hogy hello címke oszlopban szereplő számok lebegőpontos, azt nemkívánatos eredményeket hozhat. Ez azért van így, mert besorolás megköveteli, hogy az csak értékek tartományt egy véges, és általában némileg kis készleten osztályok diszkrét válasz változó.

3. Ha hello pontozási a dataset nem tartalmaz minden hello használt funkciók tootrain hello modellt, hello [Score Model] [ score-model] hibát eredményez.

4. Ha dataset pontozási hello sorában hiányzó értékre, vagy a szolgáltatások bármelyikéhez egy végtelen értéket tartalmaz, hello [Score Model] [ score-model] nem hoznak létre minden olyan kimeneti megfelelő toothat sor.

5. Hello [Score Model] [ score-model] készíthet azonos kimeneti adatkészlet pontozási hello összes sorát. Ez akkor fordulhat, például döntési erdők használó minta / Levélcsomópont minimális száma hello választása elérhető példák hello nagyobb toobe száma besorolás megkísérlése során.

<!-- Module References -->
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/

