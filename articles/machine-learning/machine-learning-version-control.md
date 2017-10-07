---
title: az Azure Machine Learning aaaALM |} Microsoft Docs
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
ms.openlocfilehash: 99470ff72fea7ab59d9d44f3fded7b9dd49a38c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-lifecycle-management-in-azure-machine-learning-studio"></a>Alkalmazás-életciklus kezelésének az Azure Machine Learning Studióban
Az Azure Machine Learning Studio egy olyan eszköz, a machine learning kísérleteket, amelyek operationalized hello Azure cloud platform fejlesztéséhez. Például a Visual Studio IDE környezetben, és egy egységes platform egyesítve méretezhető felhőalapú szolgáltatás hello van. Is használhatja az szabványos alkalmazás életciklusa Management (ALM) eljárásait, versioning különböző eszközök tooautomated végrehajtási és központi telepítését, az Azure Machine Learning Studio. A cikk ismerteti az egyes hello-beállítások és módszerek.

## <a name="versioning-experiment"></a>Versioning kísérlet
Nincsenek ajánlott módja két tooversion a kísérletek. Támaszkodjon beépített futtatási előzményei, vagy hello kísérlet JavaScript Object Notation (JSON) formátumban exportálja és külsőleg kezelheti. Mindkét megközelítés az előnyei és hátrányai tartalmaz.

### <a name="experiment-snapshots-using-run-history"></a>Kísérlet a pillanatképek futtatása előzmények használata
Hello végrehajtási modell hello Azure Machine Learning Studióban tanulási kísérlet, minden alkalommal, amikor hello kattint a **futtatása** hello kísérlet szerkesztő, egy nem módosítható pillanatkép hello kísérlet gomb akkor elküldött toohello Feladatütemező. Ebben a listában, a pillanatképek hello kattintva megtekintheti **futtatása előzmények** hello parancssáv hello kísérlet szerkesztő nézetben gombjára.

![Futtatási előzmények gomb](media/machine-learning-version-control/runhistory.png)

Akkor is, majd hello kísérlet: hello idő hello kísérlet hello nevére kattintva zárolt módban megnyitott hello pillanatkép elküldött toorun volt, és hello pillanatkép készítése. Figyelje meg, hogy csak az első elem hello lista, amely hello aktuális kísérlet jelöli, hello szerkeszthető állapotban van. Figyelje meg, hogy minden pillanatkép lehet különböző állapotát is, valamint államok befejeződött (a részleges futtatása), sikertelen, beleértve sikertelen (a részleges futtatása), vagy vázlatszintű.

![Futtatási előzmények lista](media/machine-learning-version-control/runhistorylist.png)

Miután megnyitotta, hello pillanatkép kísérlet elmentse egy új kísérletet, és módosíthatja azt. Ha a kísérlet pillanatkép erőforrásokat, például a betanított modell, -átalakítási, és adatkészlet frissített verzióit tartalmazza, hello pillanatkép hello hivatkozások toohello eredeti verzió megtartja a hello pillanatkép készítése. Ha zárolva hello menti, új kísérlet létrehozása, pillanatkép Azure Machine Learning Studio ezen eszközök újabb verzióra hello meglétét észleli, és automatikusan frissítése hello új kísérletben.

Ha törli a hello kísérlet, a kísérlet összes pillanatképet törlődnek.

### <a name="exportimport-experiment-in-json-format"></a>Exportálás/importálás kísérlet JSON formátumban
Futtatás hello Jelentéselőzmények pillanatképeinek ne hello kísérlet nem módosítható verziójú Azure Machine Learning Studio minden egyes elküldött toorun. Is a hello kísérlet helyi másolatot mentenek és rendszerben tooyour kedvenc forrás, például a Team Foundation Server, jelölje be, és később a hozza létre a helyi fájl kísérlet. Használhatja a hello [Azure Machine Learning PowerShell](http://aka.ms/amlps) parancsmagjaival [ *Export-AmlExperimentGraph* ](https://github.com/hning86/azuremlps#export-amlexperimentgraph) és [  *Importálás – AmlExperimentGraph* ](https://github.com/hning86/azuremlps#import-amlexperimentgraph) tooaccomplish, amely.

hello JSON-fájl hello képviselő szöveges alakot diagramot, többek között lehet, hogy a hivatkozás tooassets hello munkaterületen például a dataset vagy a modell betanítását kipróbálásához. A szerializált verzió hello eszköz nem tartalmaz. Tooimport hello JSON-dokumentum hello workspace alkalmazásban kísérli meg, ha hivatkozott hello eszközök már léteznie kell a hello azonos eszköz hello kísérletben hivatkozott azonosítók. Ellenkező esetben nem fogja tudni tooaccess importált hello kísérlet.

## <a name="versioning-trained-model"></a>Versioning betanított modell
Az Azure Machine Learning egy trained model szerializált .iLearner fájlként ismert olyan formátumra, és hello Azure Blob storage-fiók hello munkaterület társított tárolja. Egyirányú tooget hello .iLearner fájlról átképezési API hello keresztül történik. [Ez a cikk](machine-learning-retrain-models-programmatically.md) azt ismerteti, hogyan működik a hello átképezési API. hello magas szintű lépéseket:

1. Állítsa be a tanítási kísérletet.
2. Vegye fel a webes szolgáltatás kimeneti port toohello tanítási modell modulhoz vagy hello modul, amely létrehozza a betanított modell hello, például a modell Hyperparameter hangolására vagy R modell létrehozása.
3. Futtassa a tanítási kísérletet, és majd telepítse azt a modell betanítási webszolgáltatásként.
4. Hello BES végpont hello képzési webszolgáltatás hívja, és adja meg a hello .iLearner kívánt fájl nevét és a Blob tárfiókhely azt a rendszer hol tárolja.
5. Betakarítás előállított hello .iLearner hello BES hívás után befejeződik.

Egy másik módja tooretrieve hello .iLearner van keresztül hello PowerShell-parancsmag segítségével [ *letöltési-AmlExperimentNodeOutput*](https://github.com/hning86/azuremlps#download-amlexperimentnodeoutput). Ez akkor lehet egyszerűbb, ha csak tooget hello .iLearner másolatát programozott módon fájl hello kell tooretrain hello modell nélkül.

Miután hello betanított modell tartalmazó hello .iLearner fájlt, majd alkalmazhat, a saját versioning stratégia. lehet, hogy hello stratégia alkalmazása előtti/utótag elnevezési szabályokat alkalmaz, és csak így hello .iLearner fájl a Blob Storage tárolóban, vagy másolása/importálása a verziókezelő rendszert.

hello mentett .iLearner fájl használható pontozó telepített webszolgáltatásokon keresztül.

## <a name="versioning-web-service"></a>Versioning webszolgáltatás
Egy Azure Machine Learning kísérlet webszolgáltatások két típusú telepítése. hello klasszikus webszolgáltatás szorosan hello kísérletet, valamint a hello munkaterület együtt használja. hello új webszolgáltatás hello Azure Resource Manager keretrendszert használja, és már nem használja az eredeti kísérlet hello vagy hello munkaterületen.

### <a name="classic-web-service"></a>Klasszikus webszolgáltatás
klasszikus webszolgáltatás tooversion, kihasználhatja a hello webes szolgáltatási végpont szerkezet. Íme egy tipikus folyamatot:

1. A prediktív kísérlet telepít egy új klasszikus webes szolgáltatás, amely tartalmaz egy alapértelmezett végpont.
2. Ep2, amely felfedi a hello verziószámának hello kísérlet/betanítása modell nevű új végpont létrehozásához.
3. Lépjen vissza, és frissítse a prediktív kísérletté és a betanított modell.
4. Újbóli hello prediktív kísérletté, amely hello alapértelmezett végpont frissíti. Azonban ez nem módosítja ep2.
5. Létrehozhat egy további ep3, amely felfedi a hello kísérletet, és a betanított modell új verziójának hello nevű végpontot.
6. Lépjen vissza toostep 3 szükség esetén.

Adott idő alatt, lehetséges, hogy sok végpontok a hello azonos a webes szolgáltatás. Minden egyes végpont hello betanított modell hello időpontban verzióját tartalmazó hello kísérlet pont időponthoz kötött másolatot képviseli. Mely végpont toocall, vagyis hatékonyan kiválasztása egy hello verziója hello pontozás futtatási modell betanítása külső logika toodetermine használhatja.

Sok azonos a webszolgáltatás végpontjainak is létrehozhat, és majd javítás hello .iLearner fájl toohello végpont tooachieve hasonló hatás különböző verziói. [Ez a cikk](machine-learning-create-models-and-endpoints-with-powershell.md) részletesebben ismerteti, hogyan tooaccomplish, amely.

### <a name="new-web-service"></a>Új webszolgáltatás
Ha létrehoz egy új Azure Resource Manager-alapú webszolgáltatás-bővítmény, hello végpont szerkezet már nem érhető el. Ehelyett hozhat létre webes szolgáltatás definíciós (WSD) fájlok, JSON formátumban, a prediktív kísérletté hello segítségével a [Export-AmlWebServiceDefinitionFromExperiment](https://github.com/hning86/azuremlps#export-amlwebservicedefinitionfromexperiment) PowerShell-parancsmag segítségével, vagy a hello [ *Export-AzureRmMlWebservice* ](https://msdn.microsoft.com/library/azure/mt767935.aspx) telepített Resource Manager-alapú webszolgáltatás a PowerShell-parancsmag segítségével.

Miután exportált hello WSD fájl és verzió szabályozhatja azt, akkor is telepítheti hello WSD új webszolgáltatásként az egy másik web service-csomag egy másik Azure-régióban. Ellenőrizze, hogy megadnia hello megfelelő tárfiók konfigurációs, valamint a hello új webes szolgáltatás terv azonosítója. a különböző .iLearner fájlok toopatch, módosíthatja hello WSD-fájlt, és frissítés hello helyhivatkozást hello képezni a modell, és üzembe egy új webszolgáltatás-bővítmény.

## <a name="automate-experiment-execution-and-deployment"></a>Kísérlet végrehajtása és a telepítés automatizálása
Egy fontos ALM célja toobe képes tooautomate hello végrehajtása és a központi telepítés hello alkalmazás. Az Azure Machine Learning, ez elvégezhető hello segítségével [PowerShell modul](http://aka.ms/amlps). Íme egy példa a végpontok közötti lépések, amelyek megfelelő tooa normál automatikus ALM végrehajtási vagy üzembe helyező folyamat hello segítségével [Azure Machine Learning Studio PowerShell modul](http://aka.ms/amlps). Minden lépés a csatolt tooone vagy további PowerShell parancsmagjait, melyekkel tooaccomplish. lépés.

1. [Töltse fel a dataset](https://github.com/hning86/azuremlps#upload-amldataset).
2. Másolja a tanítási kísérletet hello munkaterület egy [munkaterület](https://github.com/hning86/azuremlps#copy-amlexperiment) vagy [gyűjteménye](https://github.com/hning86/azuremlps#copy-amlexperimentfromgallery), vagy [importálása](https://github.com/hning86/azuremlps#import-amlexperimentgraph) egy [exportált](https://github.com/hning86/azuremlps#export-amlexperimentgraph) kísérletezhet a helyi lemez.
3. [Hello adatkészlet frissítése](https://github.com/hning86/azuremlps#update-amlexperimentuserasset) a hello tanítási kísérletet.
4. [Futtassa a hello tanítási kísérletet](https://github.com/hning86/azuremlps#start-amlexperiment).
5. [Hello betanított modell előléptetni](https://github.com/hning86/azuremlps#promote-amltrainedmodel).
6. [Másolja a prediktív kísérletté](https://github.com/hning86/azuremlps#copy-amlexperiment) hello munkaterületre.
7. [Frissítés hello betanított modell](https://github.com/hning86/azuremlps#update-amlexperimentuserasset) hello prediktív kísérletben.
8. [Futtassa a hello prediktív kísérletté](https://github.com/hning86/azuremlps#start-amlexperiment).
9. [Egy webszolgáltatás-bővítmény telepítése](https://github.com/hning86/azuremlps#new-amlwebservice) hello prediktív kísérlet.
10. Hello webszolgáltatás tesztelése [RRS](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) vagy [BES](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint) végpont.

## <a name="next-steps"></a>Következő lépések
* Töltse le a hello [Azure Machine Learning Studio PowerShell](http://aka.ms/amlps) modul és a start tooautomate a ALM feladatok.
* Ismerje meg, hogyan túl[létrehozása és kezelése a nagy számú gépi tanulás modellek csak egyetlen kísérlet használatával](machine-learning-create-models-and-endpoints-with-powershell.md) PowerShell és az átképezési API segítségével.
* További információ [Azure Machine Learning webszolgáltatások üzembe helyezése](machine-learning-publish-a-machine-learning-web-service.md).
