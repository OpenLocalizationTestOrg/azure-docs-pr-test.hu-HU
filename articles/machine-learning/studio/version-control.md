---
title: "Az Azure gépi tanulás ALM |} Microsoft Docs"
description: "Alkalmazás-életciklus kezelésének ajánlott eljárások az Azure Machine Learning Studióban alkalmazása"
keywords: "Az Azure ML, életciklus Alkalmazáskezelés verziókezelést ALM, AML,"
services: machine-learning
documentationcenter: 
author: hning86
manager: jhubbard
editor: cgronlun
ms.assetid: 1be6577d-f2c7-425b-b6b9-d5038e52b395
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2016
ms.author: haining
ms.openlocfilehash: 9d1fcc761115c64fafb811d6ca1c2389babfdc15
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/11/2017
---
# <a name="application-lifecycle-management-in-azure-machine-learning-studio"></a>Alkalmazás-életciklus kezelésének az Azure Machine Learning Studióban
Az Azure Machine Learning Studio egy olyan eszköz, a machine learning kísérleteket, amely az Azure felhőalapú platform vannak operationalized fejleszt. Ez például a Visual Studio IDE és méretezhető felhőalapú szolgáltatás egyesített egyetlen platformra. Is használhatja az szabványos alkalmazás életciklusa Management (ALM) eljárásait, versioning különböző eszközök automatikus végrehajtás és üzembe helyezés, az Azure Machine Learning Studio. A cikk ismerteti az egyes lehetőségek és szempontok.

## <a name="versioning-experiment"></a>Versioning kísérlet
Két módon ajánlott verzióra a kísérletek. Támaszkodjon beépített futtatási előzményei, vagy a kísérlet JavaScript Object Notation (JSON) formátumban exportálja és külsőleg kezelheti. Mindkét megközelítés az előnyei és hátrányai tartalmaz.

### <a name="experiment-snapshots-using-run-history"></a>Kísérlet a pillanatképek futtatása előzmények használata
Az Azure Machine Learning Studióban tanulási kísérlet, minden alkalommal, amikor kattint, a végrehajtási modell a **futtatása** kísérlet szerkesztőben a kísérlet egy módosítható pillanatkép gomb elküldve a Feladatütemező a. Ebben a listában, a pillanatképek kattintva megtekintheti a **futtatása előzmények** gombra a parancssávon az a kísérlet szövegszerkesztő nézetben.

![Futtatási előzmények gomb](./media/version-control/runhistory.png)

Ezután nyissa meg a pillanatkép a kísérletet a kísérlet, valamint a pillanatkép küldött időben nevére kattintanak zárolt módban készült. Figyelje meg, hogy a listában, amely jelöli az aktuális kísérletet, csak az első elem szerkeszthető állapotban van-e. Figyelje meg, hogy minden pillanatkép lehet különböző állapotát is, valamint államok befejeződött (a részleges futtatása), sikertelen, beleértve sikertelen (a részleges futtatása), vagy vázlatszintű.

![Futtatási előzmények lista](./media/version-control/runhistorylist.png)

Miután már meg van nyitva, a pillanatkép kísérlet elmentse egy új kísérletet, és módosíthatja azt. Ha a kísérlet pillanatkép erőforrásokat, például a betanított modell, -átalakítási, és adatkészlet frissített verzióit tartalmazza, a pillanatkép az eredeti verzió hivatkozásainak megtartja a során létrehozott pillanatképen. A zárolt pillanatkép elmentse egy új kísérletet, ha az Azure Machine Learning Studio ezen eszközök újabb verzióra meglétét észleli, és automatikusan frissíti őket az új kísérlet.

Ha törli a kísérletet, törlődnek, hogy a kísérlet összes pillanatképet.

### <a name="exportimport-experiment-in-json-format"></a>Exportálás/importálás kísérlet JSON formátumban
A futtatási előzményei pillanatképek ne a kísérlet nem módosítható verziójú Azure Machine Learning Studio minden alkalommal, amikor futtatásához küldené el. Is helyi másolatot készíteni a kísérlet, és ellenőrizze a kedvenc vezérlő forrásrendszer, például a Team Foundation Server, és később hozza létre a helyi fájl kísérlet. Használhatja a [Azure Machine Learning PowerShell](http://aka.ms/amlps) parancsmagjaival [ *Export-AmlExperimentGraph* ](https://github.com/hning86/azuremlps#export-amlexperimentgraph) és [  *Importálás – AmlExperimentGraph* ](https://github.com/hning86/azuremlps#import-amlexperimentgraph) hajthatja végre, hogy.

A JSON-fájl nem lehet hivatkozni eszközök például a dataset vagy a modell betanítását munkaterületen kísérlet gráf képviselő szöveges alakot. A szerializált verzió, az eszköz nem tartalmaz. A JSON-dokumentum importálja vissza a munkaterületre kísérli meg, ha a hivatkozott eszközök már léteznie kell az ugyanabban a kísérletben hivatkozott azonosítók eszközt. Ellenkező esetben csak akkor érhessék el az importált kísérlet.

## <a name="versioning-trained-model"></a>Versioning betanított modell
Az Azure Machine Learning egy trained model olyan formátumra .iLearner fájlként ismert szerializált, és az Azure Blob storage-fiók a munkaterület társított tárolja. Egyik módja a .iLearner fájl példánya van a megőrzési API-n keresztül. [Ez a cikk](retrain-models-programmatically.md) azt ismerteti, hogyan működik, a megőrzési API-t. A magas szintű lépéseket:

1. Állítsa be a tanítási kísérletet.
2. Adjon hozzá egy webes szolgáltatás kimeneti portot a tanítási modell modulhoz, vagy a modul, amely létrehozza a betanított modell, például a modell Hyperparameter hangolására vagy R modell létrehozása.
3. Futtassa a tanítási kísérletet, és majd telepítse azt a modell betanítási webszolgáltatásként.
4. A BES végpont a képzés webes szolgáltatás hívása, és adja meg a kívánt .iLearner fájl nevét és a Blob tárfiókhely azt a rendszer hol tárolja.
5. Nem a a létrehozott .iLearner fájlt kér be az adatokat, a BES hívás befejeződése után.

Egy másik .iLearner fájl letöltéséhez módja a PowerShell-parancsmag segítségével keresztül [ *letöltési-AmlExperimentNodeOutput*](https://github.com/hning86/azuremlps#download-amlexperimentnodeoutput). Ez akkor lehet egyszerűbb, ha csak egy példánya a .iLearner fájl anélkül, hogy a modell programozott módon működik.

Miután a betanított modell tartalmazó .iLearner fájlt, majd a saját versioning stratégia is alkalmaz. Lehet, hogy a stratégia alkalmazása előtti/utótag elnevezési szabályokat alkalmaz, és csak a Blob Storage a .iLearner fájl hagyja, vagy másolása/importálása a verziókezelő rendszert.

A mentett .iLearner fájl használható pontozó telepített webszolgáltatásokon keresztül.

## <a name="versioning-web-service"></a>Versioning webszolgáltatás
Egy Azure Machine Learning kísérlet webszolgáltatások két típusú telepítése. A klasszikus webszolgáltatás szorosan párosított a kísérletet, valamint a munkaterületen. Az új webszolgáltatás használja az Azure Resource Manager keretrendszer, és már nem használja az eredeti kísérlet vagy a munkaterületen.

### <a name="classic-web-service"></a>Klasszikus webszolgáltatás
Klasszikus webszolgáltatás verzióra kihasználhatja a webes szolgáltatás végpont szerkezet. Íme egy tipikus folyamatot:

1. A prediktív kísérlet telepít egy új klasszikus webes szolgáltatás, amely tartalmaz egy alapértelmezett végpont.
2. Ep2, amely felfedi a kísérletben/betanítása modell verziószámának nevű új végpont létrehozásához.
3. Lépjen vissza, és frissítse a prediktív kísérletté és a betanított modell.
4. A prediktív kísérletté, amely frissíti az alapértelmezett végpont újratelepíti. Azonban ez nem módosítja ep2.
5. Létrehozhat további végpont nevű ep3, amely felfedi a betanított modell és a kísérlet új verziója.
6. Lépjen vissza, ha a 3, ha szükséges.

Adott idő alatt lehetséges, hogy az azonos webszolgáltatás sok végpontok. Minden egyes végpont a kísérlet, a betanított modell időpontban verzióját tartalmazó pont időponthoz kötött másolatot képviseli. Majd használható külső logikai meghatározni, melyik végponthoz hívására, vagyis hatékonyan kiválasztása a betanított modell pontozási futtatáshoz verzióját.

Sok azonos a webszolgáltatás végpontjainak is létrehozhat, és a végpontnak hasonló hatás eléréséhez a .iLearner fájl különböző verzióinak majd javítás. [Ez a cikk](create-models-and-endpoints-with-powershell.md) részletesebben ismerteti, hogyan hajthatja végre, hogy.

### <a name="new-web-service"></a>Új webszolgáltatás
Ha létrehoz egy új Azure Resource Manager-alapú webszolgáltatás-bővítmény, a végpont szerkezet már nem érhető el. Ehelyett hozhat létre webes szolgáltatás definíciós (WSD) fájlok, JSON formátumban, a prediktív kísérlet használatával a [Export-AmlWebServiceDefinitionFromExperiment](https://github.com/hning86/azuremlps#export-amlwebservicedefinitionfromexperiment) PowerShell-parancsmag segítségével, vagy a [ *Export-AzureRmMlWebservice* ](https://msdn.microsoft.com/library/azure/mt767935.aspx) telepített Resource Manager-alapú webszolgáltatás a PowerShell-parancsmag segítségével.

Miután az exportált WSD fájl- és szabályozhatja azt verziója, is telepítheti a WSD új webszolgáltatásként az egy másik web service-csomag egy másik Azure-régióban. Ellenőrizze, hogy a megfelelő tárolási fiók konfigurációját, valamint az új webes szolgáltatás csomag. Adjon meg Javítást a különböző .iLearner fájlok, a WSD-fájl módosítása és frissítése a betanított modell hivatkozik, és üzembe egy új webszolgáltatás-bővítmény.

## <a name="automate-experiment-execution-and-deployment"></a>Kísérlet végrehajtása és a telepítés automatizálása
ALM fontos eleme, végrehajtási és az alkalmazás telepítési folyamatának automatizálását tennie. Az Azure Machine Learning, ennek segítségével végezheti a [PowerShell modul](http://aka.ms/amlps). Íme egy példa, amely kapcsolódik a szabványos ALM végpont lépéseket automatikus végrehajtási vagy üzembe helyező folyamat használatával a [Azure Machine Learning Studio PowerShell modul](http://aka.ms/amlps). Az egyes lépések, melyekkel a lépés elvégzéséhez legalább egy PowerShell-parancsmagjaival van csatolva.

1. [Töltse fel a dataset](https://github.com/hning86/azuremlps#upload-amldataset).
2. Másolja a munkaterületen, a tanítási kísérletet egy [munkaterület](https://github.com/hning86/azuremlps#copy-amlexperiment) vagy [gyűjteménye](https://github.com/hning86/azuremlps#copy-amlexperimentfromgallery), vagy [importálása](https://github.com/hning86/azuremlps#import-amlexperimentgraph) egy [exportált](https://github.com/hning86/azuremlps#export-amlexperimentgraph) kísérlet a helyi a lemez.
3. [Az adatkészlet frissítéséhez](https://github.com/hning86/azuremlps#update-amlexperimentuserasset) a a tanítási kísérletet.
4. [Futtassa a tanítási kísérletet](https://github.com/hning86/azuremlps#start-amlexperiment).
5. [A betanított modell előléptetni](https://github.com/hning86/azuremlps#promote-amltrainedmodel).
6. [Másolja a prediktív kísérletté](https://github.com/hning86/azuremlps#copy-amlexperiment) a munkaterületre.
7. [Frissítse a betanított modell](https://github.com/hning86/azuremlps#update-amlexperimentuserasset) a prediktív kísérletben.
8. [Futtassa a prediktív kísérletté](https://github.com/hning86/azuremlps#start-amlexperiment).
9. [Egy webszolgáltatás-bővítmény telepítése](https://github.com/hning86/azuremlps#new-amlwebservice) a prediktív kísérlet.
10. A webszolgáltatás tesztelése [RRS](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) vagy [BES](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint) végpont.

## <a name="next-steps"></a>Következő lépések
* Töltse le a [Azure Machine Learning Studio PowerShell](http://aka.ms/amlps) modul és a kezdő a ALM feladatok automatizálására.
* Megtudhatja, hogyan [létrehozása és kezelése a nagy számú gépi tanulás modellek csak egyetlen kísérlet használatával](create-models-and-endpoints-with-powershell.md) PowerShell és az átképezési API segítségével.
* További információ [Azure Machine Learning webszolgáltatások üzembe helyezése](publish-a-machine-learning-web-service.md).
