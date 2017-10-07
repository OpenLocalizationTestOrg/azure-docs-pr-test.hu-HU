---
title: "aaaHow tooprepare a modell Azure Machine Learning Studio-KözpontiTelepítés |} Microsoft Docs"
description: "Hogyan tooprepare a betanított modell egy központi telepítés szolgáltatás átalakításával a Machine Learning Studio képzési kísérletezhet tooa prediktív kísérletté."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: eb943c45-541a-401d-844a-c3337de82da6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.author: garye
ms.openlocfilehash: d25bc68be63679a803bfc24a9e29e009a9263f5f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooprepare-your-model-for-deployment-in-azure-machine-learning-studio"></a>Hogyan tooprepare a modell Azure Machine Learning Studio-KözpontiTelepítés

Az Azure Machine Learning Studio által biztosított eszközök toodevelop egy prediktív elemzési modell kell, és ezután azok egy Azure webes szolgáltatás telepítésével hello meg.

toodo, használhatja a Studio toocreate kísérlet - nevű egy *tanítási kísérletet* – amelyen betanítása, pontozása, és szerkesztheti a modell. Ha már elégedett, a tanítási kísérletet tooa átalakításával beolvasni a modell készen toodeploy *prediktív kísérletté* , hogy van-e beállítva tooscore felhasználói adatokat.

Ez a folyamat egy példa látható [forgatókönyv: a hitelkockázat értékelésére az Azure Machine Learning a prediktív elemzési megoldás fejlesztése](machine-learning-walkthrough-develop-predictive-solution.md).

Ez a cikk egy részletes bemutatója hello részleteinek hogyan lekérdezi a egy tanítási kísérletet egy prediktív kísérletté alakítja át, és hogyan legyen telepítve központilag, hogy prediktív kísérletté vesz igénybe. Ezen adatok megismerni, áttekintheti, hogyan tooconfigure a telepített modell toomake hatékonyabb azt.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview"></a>Áttekintés 

a tanítási kísérletet tooa prediktív kísérletté átalakítani hello folyamat három lépést foglal magában:

1. Cserélje le a hello machine learning algoritmus modulok a betanított modell.
2. Trim hello kísérlet tooonly ilyen modulokhoz, pontozási szükséges. A tanítási kísérletet, amelyek szükségesek a képzési, de nem szükségesek, miután hello modell betanítása modulok számos tartalmaz.
3. Határozza meg, hogy a modell hello webes felhasználói adatokat fogad, és milyen adatok visszaadásához fog.

> [!TIP]
> A tanítási kísérletet, a képzési és a saját adatok használata a modell pontozása már megtörtént. Azonban Amennyiben telepített, felhasználók küld új tooyour adatmodell, és előrejelzéses eredményeket ad vissza. Igen a tanítási kísérletet tooa prediktív kísérletté tooget, és készen áll a központi telepítés konvertálásakor, tartsa szem előtt hogyan hello modell mások által használható.
> 
> 

## <a name="set-up-web-service-button"></a>Webes szolgáltatások beállítása gomb
A kísérlet futtatása után (kattintson **futtatása** hello kísérletvászonra hello alján), hello kattintson **webes szolgáltatások beállítása** gomb (válassza hello **prediktív webszolgáltatás** a beállítás). **Webes szolgáltatások beállítása** hajtja végre az Ön hello átalakítani a tanítási kísérletet tooa prediktív kísérletté három lépést:

1. A betanított modell hello mentése **betanított modellek** hello modulpalettán (hello kísérletvászonra toohello balra) szakasza. Kicseréli hello gépi tanulási algoritmus és [tanítási modell] [ train-model] modell betanítása mentett hello rendelkező modulok.
2. Elemzi a kísérletet, és eltávolítja a modult, amely egyértelműen használták csak képzési és már nem szükséges.
3. Beilleszti _webszolgáltatás bemenetét_ és _kimeneti_ modulok importálása a kísérlet során (ezek a modulok fogadja el, és térjen vissza a felhasználói adatok) alapértelmezett helyét.

Például hello következő kísérletezhet szerelvények két osztályú súlyozott döntési fa-modellben nyilvántartásba mintaadatok:

![Tanítási kísérletet][figure1]

a kísérletben hello modulok alapvetően négy különböző feladatokat látnak el:

![A modul funkciók][figure2]

A tanítási kísérletet tooa prediktív kísérletté alakításakor néhányat ezek a modulok már nem szükséges, vagy most más célt szolgál:

* **Adatok** -nem használják a minta adatkészlet adatainak hello pontozási – hello felhasználói hello webszolgáltatás fogja adni hello adatok toobe program pontozza a mennyiségeket. Ehhez az adatkészlethez, adattípusok, például a hello metaadatok azonban hello betanított modell használatával. Így tookeep hello adatkészlet hello prediktív kísérletté kell, hogy több a metaadatok.

* **Előkészítő** – attól függően, hogy ezek a modulok feltétlenül nem szükséges tooprocess hello bejövő adatok pontozó, benyújtott hello felhasználói adatokat. Hello **webes szolgáltatások beállítása** gomb nem touch ezek - toodecide van szüksége, hogy hogyan kívánja toohandle őket.
  
    Például ebben a példában hello minta-adatkészleteken lehet érték hiányzik, ezért a [Clean Missing Data] [ clean-missing-data] modul végre tudta hajtani belefoglalt toodeal velük. Hello minta-adatkészleteken is oszlopokat, amelyek nem szükséges tootrain hello modell. Ezért egy [Select Columns in Dataset] [ select-columns] modul végre tudta belefoglalt tooexclude ezeket a felesleges oszlopok hello adatfolyam. Ha pontozó küldött hello adatok keresztül hello webszolgáltatás nincs hiányzó értékeket, majd eltávolíthatja hello [Clean Missing Data] [ clean-missing-data] modul. Mivel azonban hello [Select Columns in Dataset] [ select-columns] modul segítségével hello oszlopok definiálására, hogy hello betanított modell vár az adatok, modult kell tooremain.

* **Vonat** -e modulokra használt tootrain hello modell. Amikor rákattint **webes szolgáltatások beállítása**, ezek a modulok egyetlen modult tartalmaz hello modell betanítása Ön helyett. Ez a modul mentett hello **betanított modellek** hello modulpalettán szakasza.

* **Pontszám** – ebben a példában a hello [Split Data] [ split] modul az használt toodivide hello adatfolyam Tesztadatok és betanítási adata. A prediktív kísérletté hello, azt még nem betanítása többé, így [Split Data] [ split] távolíthatja el. Hasonlóképpen, a második hello [Score Model] [ score-model] modul és hello [modell kiértékelése] [ evaluate-model] modul hello tesztből használt toocompare eredmény adatok, így ezek a modulok nincs szükség a prediktív hello kipróbálásához. hello fennmaradó [Score Model] [ score-model] modul, azonban az szükséges tooreturn keresztül hello webszolgáltatás pontszám eredményt.

Ez a példa megjelenésének kattintás után **webes szolgáltatások beállítása**:

![Prediktív kísérletté konvertálni][figure3]

hello dolgozott **webes szolgáltatások beállítása** lehet, hogy elegendő tooprepare a kísérlet toobe telepített webszolgáltatásként. Azonban érdemes lehet toodo néhány további feladata adott tooyour kísérlet.

### <a name="adjust-input-and-output-modules"></a>Módosítsa a bemeneti és kimeneti modulok
A tanítási kísérletet, a használt betanítási adatok, és majd volt néhány feldolgozási tooget hello hello gépi tanulási algoritmus szükséges, az űrlap adatait. Hello adatokat várt tooreceive hello webszolgáltatás keresztül nem lesz szükség a feldolgozás, ha megkerüléséhez azt: csatlakozás hello hello kimenete **webszolgáltatás bemeneti modul** tooa különböző modul a kísérletben. hello felhasználói adatok most érkeznek hello modellben ezen a helyen.

Alapértelmezés szerint például **webes szolgáltatások beállítása** visszahelyezi hello **webszolgáltatás bemenetét** modul az adatfolyam, ahogy az a fenti ábrán hello hello tetején. De azt kézzel is elhelyezheti hello **webszolgáltatás bemenetét** túli hello adatfeldolgozási modulok:

![Áthelyezése hello webszolgáltatás bemenetét][figure4]

hello bemeneti adatai megadott keresztül hello webszolgáltatás most továbbítani fogja közvetlenül a hello Score Model-modul bármely előfeldolgozása nélkül.

Hasonlóképpen, alapértelmezés szerint **webes szolgáltatások beállítása** visszahelyezi hello webszolgáltatás kimeneti modul az adatfolyam hello alján. Ebben a példában a hello webszolgáltatás visszaadható toohello felhasználói hello kimenete hello [Score Model] [ score-model] modult, amely hello teljes bemeneti adatok vektoros mellett pontozási eredményei hello is.
Azonban ha inkább tooreturn valami más, majd hozzá további modulok előtt hello **webes szolgáltatás kimeneti** modul. 

Például tooreturn csak hello pontozási eredményeinek és nem hello teljes vektort a bemeneti adatok hozzáadása egy [Select Columns in Dataset] [ select-columns] modul tooexclude kivételével az összes oszlop hello pontozási eredményei. Ezután lépjen a hello **webes szolgáltatás kimeneti** modul toohello kimenete hello [Select Columns in Dataset] [ select-columns] modul. hello kísérlet így néz ki:

![Hello webes szolgáltatás kimeneti áthelyezése][figure5]

### <a name="add-or-remove-additional-data-processing-modules"></a>Adja hozzá, vagy távolítsa el a további adatok feldolgozása modulok
Ha a kísérletben, amelyek biztosan pontozási során nem szükséges további modulok, ezek távolíthatja el. Például mert hello helyeztük **webszolgáltatás bemenetét** modul tooa pont hello adatfeldolgozási modulok, azt is eltávolítja hello [Clean Missing Data] [ clean-missing-data] modul hello prediktív kísérletté.

A prediktív kísérletté most néz ki:

![További modul eltávolítása][figure6]


### <a name="add-optional-web-service-parameters"></a>Adja hozzá a választható paraméterek: Web Service
Bizonyos esetekben érdemes lehet a webes szolgáltatás toochange a modulok hello működését tooallow hello felhasználói hello szolgáltatás elérésekor. *Webszolgáltatási paramétereket* toodo lehetővé teszi ezt.

Ilyenek például beállítását egy [és adatokat importálhat] [ import-data] modul hello hello felhasználója telepített webes szolgáltatás, megadhat egy másik adatforráshoz hello webszolgáltatás elérésekor. Vagy konfigurálása egy [adatok exportálása] [ export-data] modul, hogy egy másik cél adható meg.

Adja meg a webszolgáltatás-paramétereket, és rendelje hozzá őket egy vagy több modulja paramétert, és megadhatja, hogy azok kötelező vagy választható. hello felhasználói hello webszolgáltatás biztosítja a értékeket a paraméterek hello szolgáltatás érhető el, és ennek megfelelően módosítják hello modul műveletek.

További információt a milyen webszolgáltatás-paramétereket, és hogyan toouse, lásd: [használata Azure Machine Learning webszolgáltatás-paramétereket][webserviceparameters].

[webserviceparameters]: machine-learning-web-service-parameters.md


## <a name="deploy-hello-predictive-experiment-as-a-web-service"></a>Hello prediktív kísérletté egy webszolgáltatás-bővítmény telepítése
Most, hogy hello prediktív kísérletté megfelelően elő van készítve, telepítheti azt egy Azure webszolgáltatásként. Hello webszolgáltatás segítségével, is el lehet küldeni tooyour adatmodell, és hello modell visszatér az előrejelzés.

Hello teljes központi telepítés folyamatáról további információk: [az Azure Machine Learning webszolgáltatás telepítése][deploy]

[deploy]: machine-learning-publish-a-machine-learning-web-service.md


<!-- Images -->
[figure1]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure1.png
[figure2]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure2.png
[figure3]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure3.png
[figure4]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure4.png
[figure5]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure5.png
[figure6]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure6.png


<!-- Module References -->
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[export-data]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
