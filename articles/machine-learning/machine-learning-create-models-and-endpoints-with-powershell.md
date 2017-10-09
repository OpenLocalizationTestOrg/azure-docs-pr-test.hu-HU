---
title: "aaaCreate egy kísérlet több modellek |} Microsoft Docs"
description: "Szolgáltatásvégpontok hello a webes és PowerShell toocreate több gépi tanulási modellek használja ugyanazt az algoritmust, de különböző képzési adatkészletek."
services: machine-learning
documentationcenter: 
author: hning86
manager: jhubbard
editor: cgronlun
ms.assetid: 1076b8eb-5a0d-4ac5-8601-8654d9be229f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: garye;haining
ms.openlocfilehash: 4a258a8ab26395d4169a058520151c860e16e169
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-many-machine-learning-models-and-web-service-endpoints-from-one-experiment-using-powershell"></a>Több Machine Learning-modellek és webszolgáltatás-végpont létrehozása egy kísérletből a PowerShell használatával
Ez gyakori probléma machine learning: sok modellek, amelyek hello toocreate kívánt azonos képzési munkafolyamat és -felhasználási hello ugyanazt az algoritmust, de különböző képzési adatkészletek bemenetként rendelkezik. Ez a cikk bemutatja, hogyan toodo Ez az Azure Machine Learning Studio használatával csak egyetlen kísérlet méretekben.

Például tegyük fel, a globális kerékpárt bérleti franchise vállalat tulajdonosa. Azt szeretné, hogy toobuild regressziós modell toopredict hello bérleti igény szerinti történelmi adatok alapján. 1000 bérleti helyek közötti hello world rendelkezik, és a korábban összegyűjtött minden helyre, például a dátum, idő, időjárási fontos szolgáltatásokat tartalmaz, és a forgalom, amelyek adott tooeach hely dataset.

Egyszer minden hello adatkészletek egyesített verzióját használja az összes helyszínen modellje betanításához sikerült. A helyek mindegyikének egyedi környezetben, mert jobb megközelítés volna, de lehet tootrain a külön-külön használatával mindegyik helyen hello dataset regressziós modell. Ily módon is beletelhet fiókok hello különböző tároló méretének, kötet, geográfiai, feltöltése, annak mobilbarát forgalom környezet minden betanított modell *stb*.

Hello ajánlott megközelítés, amely lehet, de nem szeretné, hogy az Azure Machine Learning toocreate 1000 képzési kísérleteket minden egy egyedi helyet jelölő. Amellett, hogy egy túlságosan feladat, célszerű is tűnik közérthető nem elég hatékony, mivel minden egyes kísérlet hello azonos összetevők hello képzési dataset kivételével az összes kellene lennie.

Szerencsére a Microsoft ennek segítségével végezheti hello [Azure Machine Learning átképezési API](machine-learning-retrain-models-programmatically.md) és automatizálása a hello feladatot [Azure Machine Learning PowerShell](machine-learning-powershell-module.md).

> [!NOTE]
> a minta gyorsabbá toomake, azt fogja csökkentse 1000 too10 helyeket hello számát. De a hello azonos alapelvek és eljárások too1, 000 helyét. hello egyetlen különbség, hogy ha azt szeretné, hogy az 1000 adatkészletek tootrain érdemes hello a következő PowerShell-parancsfájlok párhuzamosan futó toothink. Hogyan, amely nem ebben a cikkben, de hello terjed toodo található példák PowerShell többszálas hello Internet.  
> 
> 

## <a name="set-up-hello-training-experiment"></a>Hello tanítási kísérletet beállítása
Példa toouse fogjuk [tanítási kísérletet](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) , amely már a hello nagyságú [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Nyissa meg az ehhez a kísérlethez a [Azure Machine Learning Studio](https://studio.azureml.net) munkaterületen.

> [!NOTE]
> Rendelés toofollow együtt ebben a példában érdemes lehet toouse szabad munkaterület helyett egy szabványos munkaterületen. Jelenleg csak létrehozunk egy végpont minden ügyfél - 10-végpontok – összesen, és a szabványos munkaterület, amely esetén, mivel szabad munkaterület korlátozott too3 végpontok. Ha csak egy ingyenes munkaterület, csak módosítása hello parancsfájlok csak 3 helyeket tooallow alatt.
> 
> 

hello kísérlet használ egy **és adatokat importálhat** modul tooimport hello képzési dataset *customer001.csv* az Azure storage-fiók. Tegyük fel, azt tanítási adathalmazt gyűjtött összes kerékpárt bérleti helyét, és tárolja őket azonos blob-tárolási hely közötti fájlnévvel hello *rentalloc001.csv* túl*rentalloc10.csv* .

![Kép](./media/machine-learning-create-models-and-endpoints-with-powershell/reader-module.png)

Vegye figyelembe, hogy egy **webes szolgáltatás kimeneti** modul hozzáadott toohello **tanítási modell** modul.
Ehhez a kísérlethez webszolgáltatásként telepítésekor kimenet társított hello végpont hello formátumban .ilearner fájl visszaállítja az hello betanított modell.

Vegye figyelembe azt is, hogy beállítjuk a webszolgáltatási paraméter hello URL-címhez, hogy hello **és adatokat importálhat** modul használja. Ez lehetővé teszi toouse hello paraméter toospecify egyedi képzési adatkészletek tootrain hello modell mindegyik helyen.
Egyéb módon azt sikerült ezt, például egy SQL-lekérdezést használ egy webes szolgáltatás paraméter tooget egy SQL Azure-adatbázis adatait, vagy egyszerűen használja egy **webszolgáltatás bemenetét** modul toopass egy adatkészlet toohello a webes szolgáltatás.

![Kép](./media/machine-learning-create-models-and-endpoints-with-powershell/web-service-output.png)

Most most futtatni a tanítási kísérletet hello alapértelmezett értékével *rental001.csv* , a képzési dataset hello. Ha hello hello kimenete **Evaluate** modul (hello kimeneti kattintson, és válassza **Visualize**), láthatja azt lekérése decent teljesítményétől *AUC* = 0.91. Ezen a ponton még készen toodeploy egy webszolgáltatás-bővítmény kívül a tanítási kísérletet.

## <a name="deploy-hello-training-and-scoring-web-services"></a>Hello képzési és webszolgáltatások pontozási központi telepítése
toodeploy hello betanítása webszolgáltatás, kattintson a hello **webes szolgáltatások beállítása** hello kísérletvászon alatt gombra, majd az **webes szolgáltatás telepítése**. Hívja meg a webszolgáltatás "" kerékpárt bérleti képzési".

Most létre kell toodeploy hello pontozási webszolgáltatáshoz.
toodo, azt kattintva **webes szolgáltatások beállítása** alatt hello vászonra, és válassza ki **prediktív webszolgáltatás**. Ezzel létrehoz egy pontozási kísérletet.
Szükség lesz toomake néhány kisebb mértékben toomake webszolgáltatásként, akkor működni, például a hello címke oszlop "számláló" eltávolítását hello bemeneti adatai és hello kimeneti tooonly hello példányazonosító és hello megfelelő előre jelzett érték.

toosave saját magának, amelyek működnek, egyszerűen nyissa meg hello [prediktív kísérletté](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) a hello gyűjteménye, amelyek már elő van készítve.

toodeploy hello webszolgáltatás, futtassa a hello prediktív kísérletté, majd kattintson a hello **webes szolgáltatás telepítése** vásznon a hello gombra. Webszolgáltatás "Kerékpárt bérleti pontozási" pontozási neve hello ".

## <a name="create-10-identical-web-service-endpoints-with-powershell"></a>10 azonos webszolgáltatás-végpontok létrehozása a PowerShell használatával
Ez a webszolgáltatás tartalmaz egy alapértelmezett végpont. De még nincs hello alapértelmezett végpont az érdekelt óta nem lehet frissíteni. Mit kell toodo toocreate 10 további végpontokat, mindegyik helyen egy. A PowerShell-lel azt fogja erre.

Először beállítjuk a PowerShell-környezetben:

    Import-Module .\AzureMLPS.dll
    # Assume hello default configuration file exists and is properly set toopoint toohello valid Workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

Ezután futtassa a következő PowerShell-paranccsal hello:

    # Create 10 endpoints on hello scoring web service.
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpointName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

Most már 10 végpontok létrehoztunk Önnek, és minden bennük hello azonos betanított modell betanítása a *customer001.csv*. Azokat a hello Azure felügyeleti portálján tekintheti meg.

![Kép](./media/machine-learning-create-models-and-endpoints-with-powershell/created-endpoints.png)

## <a name="update-hello-endpoints-toouse-separate-training-datasets-using-powershell"></a>Frissítse a hello végpontok toouse külön tanítási adathalmazt PowerShell használatával
hello következő lépésre tooupdate hello végpontok minden ügyfélnél egyedi adatokat a egyedileg betanítása modellekkel. Azonban először meg kell tooproduce ezek a modellek az hello **kerékpárt bérleti képzési** webes szolgáltatás. Ugorjunk vissza toohello **kerékpárt bérleti képzési** webes szolgáltatás. A BES végpont 10-szer adatkészletekkel 10 különböző képzési rendelés tooproduce 10 különböző modellek igazolnia kell toocall. Hello fogjuk használni **InovkeAmlWebServiceBESEndpoint** PowerShell parancsmag toodo ez.

Is szüksége lesz tooprovide hitelesítő adatokat a blob storage-fiók be `$configContent`, nevezetesen: hello mezők `AccountName`, `AccountKey` és `RelativeLocation`. Hello `AccountName` egyike lehet a számítógépfiók-nevét, mint a hello **klasszikus Azure felügyeleti portálon** (*tárolási* lap). A tárfiók kattintva a `AccountKey` lenyomásával hello található **elérési kulcsok kezelése** hello alsó és hello Másolás gombra *elsődleges elérési kulcsot*. Hello `RelativeLocation` az hello elérési útja relatív tooyour tároló új modell tárolásához. Például hello elérési `hai/retrain/bike_rental/` hello parancsfájlban pontok tooa tároló alatt `hai`, és `/retrain/bike_rental/` találhatók. Jelenleg nem hozható létre almappákat hello portálon felhasználói felületén keresztül, de nincsenek [több Azure Tártallózók](../storage/common/storage-explorers.md) , amelyek lehetővé teszik toodo stb. Javasoljuk, hogy hozzon létre egy új tároló a tárolási toostore hello az új betanított modellek (.ilearner fájlok) az alábbiak szerint: a tároló lapon kattintson a hello **Hozzáadás** hello alsó gombra, és adjon neki nevet `retrain`. Összefoglalva, a szükséges módosításokat hello toohello az alábbi parancsfájl említett adat túl`AccountName`, `AccountKey` és `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).

    # Invoke hello retraining API 10 times
    # This is hello default (and hello only) endpoint on hello training web service
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

> [!NOTE]
> hello BES végpont hello csak akkor támogatja a mód ehhez a művelethez. Betanított modellek előállító RR-EKET nem használható.
> 
> 

10 különböző BES feladat konfigurációs json-fájlokat, létrehozása helyett a fent látható azt dinamikusan hozzon létre helyette hello config karakterlánc, és toohello hírcsatorna azt *jobConfigString* hello paramétere  **InvokeAmlWebServceBESEndpoint** parancsmaggal, mert nincs valóban nincs szükség tookeep egy másolatot a lemezen.

Ha minden megfelelően működik, egy kis idő után kell megjelennie 10 .ilearner fájlok, a *model001.ilearner* túl*model010.ilearner*, az Azure-tárfiókot. Most már készen áll a tooupdate a rendszer a 10 pontozási webes szolgáltatás végpontjait ezek a modellek hello segítségével a **javítás-AmlWebServiceEndpoint** PowerShell-parancsmagot. Ne felejtse el újra, hogy a Microsoft csak javítás a programozott módon korábban létrehozott hello nem alapértelmezett végpontok.

    # Patch hello 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '<my_blob_sas_token>'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }

Ez meglehetősen gyorsan fusson. Hello végrehajtásának befejezése után fog sikeresen létrehoztunk 10 prediktív webszolgáltatási végpontok, egy egyedi módon betanítása hello dataset adott tooa bérleti helyükre, minden olyan egyetlen tanítási kísérletet a betanított modell tartalmazó. tooverify, próbálja meg ezeket a végpontokat hello segítségével hívja **InvokeAmlWebServiceRRSEndpoint** parancsmagot, hogy biztosít számukra a hello azonos bemeneti adatai, és toosee különböző előrejelzés eredmények számíthat óta hello modellek különböző képzési készletekkel képezni.

## <a name="full-powershell-script"></a>Teljes PowerShell-parancsfájl
Hello teljes forráskód hello listája itt található:

    Import-Module .\AzureMLPS.dll
    # Assume hello default configuration file exists and properly set toopoint toohello valid workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

    # Create 10 endpoints on hello scoring web service
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpontName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

    # Invoke hello retraining API 10 times tooproduce 10 regression models in .ilearner format
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

    # Patch hello 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '?test'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }
