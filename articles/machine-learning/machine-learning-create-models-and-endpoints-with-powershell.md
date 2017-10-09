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
# <a name="create-many-machine-learning-models-and-web-service-endpoints-from-one-experiment-using-powershell"></a><span data-ttu-id="c5c0c-103">Több Machine Learning-modellek és webszolgáltatás-végpont létrehozása egy kísérletből a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="c5c0c-103">Create many Machine Learning models and web service endpoints from one experiment using PowerShell</span></span>
<span data-ttu-id="c5c0c-104">Ez gyakori probléma machine learning: sok modellek, amelyek hello toocreate kívánt azonos képzési munkafolyamat és -felhasználási hello ugyanazt az algoritmust, de különböző képzési adatkészletek bemenetként rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-104">Here's a common machine learning problem: You want toocreate many models that have hello same training workflow and use hello same algorithm, but have different training datasets as input.</span></span> <span data-ttu-id="c5c0c-105">Ez a cikk bemutatja, hogyan toodo Ez az Azure Machine Learning Studio használatával csak egyetlen kísérlet méretekben.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-105">This article shows you how toodo this at scale in Azure Machine Learning Studio using just a single experiment.</span></span>

<span data-ttu-id="c5c0c-106">Például tegyük fel, a globális kerékpárt bérleti franchise vállalat tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-106">For example, let's say you own a global bike rental franchise business.</span></span> <span data-ttu-id="c5c0c-107">Azt szeretné, hogy toobuild regressziós modell toopredict hello bérleti igény szerinti történelmi adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-107">You want toobuild a regression model toopredict hello rental demand based on historic data.</span></span> <span data-ttu-id="c5c0c-108">1000 bérleti helyek közötti hello world rendelkezik, és a korábban összegyűjtött minden helyre, például a dátum, idő, időjárási fontos szolgáltatásokat tartalmaz, és a forgalom, amelyek adott tooeach hely dataset.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-108">You have 1,000 rental locations across hello world and you've collected a dataset for each location that includes important features such as date, time, weather, and traffic that are specific tooeach location.</span></span>

<span data-ttu-id="c5c0c-109">Egyszer minden hello adatkészletek egyesített verzióját használja az összes helyszínen modellje betanításához sikerült.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-109">You could train your model once using a merged version of all hello datasets across all locations.</span></span> <span data-ttu-id="c5c0c-110">A helyek mindegyikének egyedi környezetben, mert jobb megközelítés volna, de lehet tootrain a külön-külön használatával mindegyik helyen hello dataset regressziós modell.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-110">But because each of your locations has a unique environment, a better approach would be tootrain your regression model separately using hello dataset for each location.</span></span> <span data-ttu-id="c5c0c-111">Ily módon is beletelhet fiókok hello különböző tároló méretének, kötet, geográfiai, feltöltése, annak mobilbarát forgalom környezet minden betanított modell *stb*.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-111">That way, each trained model could take into account hello different store sizes, volume, geography, population, bike-friendly traffic environment, *etc.*.</span></span>

<span data-ttu-id="c5c0c-112">Hello ajánlott megközelítés, amely lehet, de nem szeretné, hogy az Azure Machine Learning toocreate 1000 képzési kísérleteket minden egy egyedi helyet jelölő.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-112">That may be hello best approach, but you don't want toocreate 1,000 training experiments in Azure Machine Learning with each one representing a unique location.</span></span> <span data-ttu-id="c5c0c-113">Amellett, hogy egy túlságosan feladat, célszerű is tűnik közérthető nem elég hatékony, mivel minden egyes kísérlet hello azonos összetevők hello képzési dataset kivételével az összes kellene lennie.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-113">Besides being an overwhelming task, it's also seems pretty inefficient since each experiment would have all hello same components except for hello training dataset.</span></span>

<span data-ttu-id="c5c0c-114">Szerencsére a Microsoft ennek segítségével végezheti hello [Azure Machine Learning átképezési API](machine-learning-retrain-models-programmatically.md) és automatizálása a hello feladatot [Azure Machine Learning PowerShell](machine-learning-powershell-module.md).</span><span class="sxs-lookup"><span data-stu-id="c5c0c-114">Fortunately, we can accomplish this by using hello [Azure Machine Learning retraining API](machine-learning-retrain-models-programmatically.md) and automating hello task with [Azure Machine Learning PowerShell](machine-learning-powershell-module.md).</span></span>

> [!NOTE]
> <span data-ttu-id="c5c0c-115">a minta gyorsabbá toomake, azt fogja csökkentse 1000 too10 helyeket hello számát.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-115">toomake our sample run faster, we'll reduce hello number of locations from 1,000 too10.</span></span> <span data-ttu-id="c5c0c-116">De a hello azonos alapelvek és eljárások too1, 000 helyét.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-116">But hello same principles and procedures apply too1,000 locations.</span></span> <span data-ttu-id="c5c0c-117">hello egyetlen különbség, hogy ha azt szeretné, hogy az 1000 adatkészletek tootrain érdemes hello a következő PowerShell-parancsfájlok párhuzamosan futó toothink.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-117">hello only difference is that if you want tootrain from 1,000 datasets you probably want toothink of running hello following PowerShell scripts in parallel.</span></span> <span data-ttu-id="c5c0c-118">Hogyan, amely nem ebben a cikkben, de hello terjed toodo található példák PowerShell többszálas hello Internet.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-118">How toodo that is beyond hello scope of this article, but you can find examples of PowerShell multi-threading on hello Internet.</span></span>  
> 
> 

## <a name="set-up-hello-training-experiment"></a><span data-ttu-id="c5c0c-119">Hello tanítási kísérletet beállítása</span><span class="sxs-lookup"><span data-stu-id="c5c0c-119">Set up hello training experiment</span></span>
<span data-ttu-id="c5c0c-120">Példa toouse fogjuk [tanítási kísérletet](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) , amely már a hello nagyságú [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="c5c0c-120">We're going toouse an example [training experiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) that we've already created in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="c5c0c-121">Nyissa meg az ehhez a kísérlethez a [Azure Machine Learning Studio](https://studio.azureml.net) munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-121">Open this experiment in your [Azure Machine Learning Studio](https://studio.azureml.net) workspace.</span></span>

> [!NOTE]
> <span data-ttu-id="c5c0c-122">Rendelés toofollow együtt ebben a példában érdemes lehet toouse szabad munkaterület helyett egy szabványos munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-122">In order toofollow along with this example, you may want toouse a standard workspace rather than a free workspace.</span></span> <span data-ttu-id="c5c0c-123">Jelenleg csak létrehozunk egy végpont minden ügyfél - 10-végpontok – összesen, és a szabványos munkaterület, amely esetén, mivel szabad munkaterület korlátozott too3 végpontok.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-123">We'll be creating one endpoint for each customer - for a total of 10 endpoints - and that will require a standard workspace since a free workspace is limited too3 endpoints.</span></span> <span data-ttu-id="c5c0c-124">Ha csak egy ingyenes munkaterület, csak módosítása hello parancsfájlok csak 3 helyeket tooallow alatt.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-124">If you only have a free workspace, just modify hello scripts below tooallow for only 3 locations.</span></span>
> 
> 

<span data-ttu-id="c5c0c-125">hello kísérlet használ egy **és adatokat importálhat** modul tooimport hello képzési dataset *customer001.csv* az Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-125">hello experiment uses an **Import Data** module tooimport hello training dataset *customer001.csv* from an Azure storage account.</span></span> <span data-ttu-id="c5c0c-126">Tegyük fel, azt tanítási adathalmazt gyűjtött összes kerékpárt bérleti helyét, és tárolja őket azonos blob-tárolási hely közötti fájlnévvel hello *rentalloc001.csv* túl*rentalloc10.csv* .</span><span class="sxs-lookup"><span data-stu-id="c5c0c-126">Let's assume we have collected training datasets from all bike rental locations and stored them in hello same blob storage location with file names ranging from *rentalloc001.csv* too*rentalloc10.csv*.</span></span>

![Kép](./media/machine-learning-create-models-and-endpoints-with-powershell/reader-module.png)

<span data-ttu-id="c5c0c-128">Vegye figyelembe, hogy egy **webes szolgáltatás kimeneti** modul hozzáadott toohello **tanítási modell** modul.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-128">Note that a **Web Service Output** module has been added toohello **Train Model** module.</span></span>
<span data-ttu-id="c5c0c-129">Ehhez a kísérlethez webszolgáltatásként telepítésekor kimenet társított hello végpont hello formátumban .ilearner fájl visszaállítja az hello betanított modell.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-129">When this experiment is deployed as a web service, hello endpoint associated with that output will return hello trained model in hello format of a .ilearner file.</span></span>

<span data-ttu-id="c5c0c-130">Vegye figyelembe azt is, hogy beállítjuk a webszolgáltatási paraméter hello URL-címhez, hogy hello **és adatokat importálhat** modul használja.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-130">Also note that we set up a web service parameter for hello URL that hello **Import Data** module uses.</span></span> <span data-ttu-id="c5c0c-131">Ez lehetővé teszi toouse hello paraméter toospecify egyedi képzési adatkészletek tootrain hello modell mindegyik helyen.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-131">This allows us toouse hello parameter toospecify individual training datasets tootrain hello model for each location.</span></span>
<span data-ttu-id="c5c0c-132">Egyéb módon azt sikerült ezt, például egy SQL-lekérdezést használ egy webes szolgáltatás paraméter tooget egy SQL Azure-adatbázis adatait, vagy egyszerűen használja egy **webszolgáltatás bemenetét** modul toopass egy adatkészlet toohello a webes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-132">There are other ways we could have done this, such as using a SQL query with a web service parameter tooget data from a SQL Azure database, or simply using a  **Web Service Input** module toopass in a dataset toohello web service.</span></span>

![Kép](./media/machine-learning-create-models-and-endpoints-with-powershell/web-service-output.png)

<span data-ttu-id="c5c0c-134">Most most futtatni a tanítási kísérletet hello alapértelmezett értékével *rental001.csv* , a képzési dataset hello.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-134">Now, let's run this training experiment using hello default value *rental001.csv* as hello training dataset.</span></span> <span data-ttu-id="c5c0c-135">Ha hello hello kimenete **Evaluate** modul (hello kimeneti kattintson, és válassza **Visualize**), láthatja azt lekérése decent teljesítményétől *AUC* = 0.91.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-135">If you view hello output of hello **Evaluate** module (click hello output and select **Visualize**), you can see we get a decent performance of *AUC* = 0.91.</span></span> <span data-ttu-id="c5c0c-136">Ezen a ponton még készen toodeploy egy webszolgáltatás-bővítmény kívül a tanítási kísérletet.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-136">At this point, we're ready toodeploy a web service out of this training experiment.</span></span>

## <a name="deploy-hello-training-and-scoring-web-services"></a><span data-ttu-id="c5c0c-137">Hello képzési és webszolgáltatások pontozási központi telepítése</span><span class="sxs-lookup"><span data-stu-id="c5c0c-137">Deploy hello training and scoring web services</span></span>
<span data-ttu-id="c5c0c-138">toodeploy hello betanítása webszolgáltatás, kattintson a hello **webes szolgáltatások beállítása** hello kísérletvászon alatt gombra, majd az **webes szolgáltatás telepítése**.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-138">toodeploy hello training web service, click hello **Set Up Web Service** button below hello experiment canvas and select **Deploy Web Service**.</span></span> <span data-ttu-id="c5c0c-139">Hívja meg a webszolgáltatás "" kerékpárt bérleti képzési".</span><span class="sxs-lookup"><span data-stu-id="c5c0c-139">Call this web service ""Bike Rental Training".</span></span>

<span data-ttu-id="c5c0c-140">Most létre kell toodeploy hello pontozási webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-140">Now we need toodeploy hello scoring web service.</span></span>
<span data-ttu-id="c5c0c-141">toodo, azt kattintva **webes szolgáltatások beállítása** alatt hello vászonra, és válassza ki **prediktív webszolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-141">toodo this, we can click **Set Up Web Service** below hello canvas and select **Predictive Web Service**.</span></span> <span data-ttu-id="c5c0c-142">Ezzel létrehoz egy pontozási kísérletet.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-142">This creates a scoring experiment.</span></span>
<span data-ttu-id="c5c0c-143">Szükség lesz toomake néhány kisebb mértékben toomake webszolgáltatásként, akkor működni, például a hello címke oszlop "számláló" eltávolítását hello bemeneti adatai és hello kimeneti tooonly hello példányazonosító és hello megfelelő előre jelzett érték.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-143">We'll need toomake a few minor adjustments toomake it work as a web service, such as removing hello label column "cnt" from hello input data and limiting hello output tooonly hello instance id and hello corresponding predicted value.</span></span>

<span data-ttu-id="c5c0c-144">toosave saját magának, amelyek működnek, egyszerűen nyissa meg hello [prediktív kísérletté](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) a hello gyűjteménye, amelyek már elő van készítve.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-144">toosave yourself that work, you can simply open hello [predictive experiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) in hello Gallery that's already been prepared.</span></span>

<span data-ttu-id="c5c0c-145">toodeploy hello webszolgáltatás, futtassa a hello prediktív kísérletté, majd kattintson a hello **webes szolgáltatás telepítése** vásznon a hello gombra.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-145">toodeploy hello web service, run hello predictive experiment, then click hello **Deploy Web Service** button below hello canvas.</span></span> <span data-ttu-id="c5c0c-146">Webszolgáltatás "Kerékpárt bérleti pontozási" pontozási neve hello ".</span><span class="sxs-lookup"><span data-stu-id="c5c0c-146">Name hello scoring web service "Bike Rental Scoring"".</span></span>

## <a name="create-10-identical-web-service-endpoints-with-powershell"></a><span data-ttu-id="c5c0c-147">10 azonos webszolgáltatás-végpontok létrehozása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="c5c0c-147">Create 10 identical web service endpoints with PowerShell</span></span>
<span data-ttu-id="c5c0c-148">Ez a webszolgáltatás tartalmaz egy alapértelmezett végpont.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-148">This web service comes with a default endpoint.</span></span> <span data-ttu-id="c5c0c-149">De még nincs hello alapértelmezett végpont az érdekelt óta nem lehet frissíteni.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-149">But we're not as interested in hello default endpoint since it can't be updated.</span></span> <span data-ttu-id="c5c0c-150">Mit kell toodo toocreate 10 további végpontokat, mindegyik helyen egy.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-150">What we need toodo is toocreate 10 additional endpoints, one for each location.</span></span> <span data-ttu-id="c5c0c-151">A PowerShell-lel azt fogja erre.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-151">We'll do this with PowerShell.</span></span>

<span data-ttu-id="c5c0c-152">Először beállítjuk a PowerShell-környezetben:</span><span class="sxs-lookup"><span data-stu-id="c5c0c-152">First, we set up our PowerShell environment:</span></span>

    Import-Module .\AzureMLPS.dll
    # Assume hello default configuration file exists and is properly set toopoint toohello valid Workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

<span data-ttu-id="c5c0c-153">Ezután futtassa a következő PowerShell-paranccsal hello:</span><span class="sxs-lookup"><span data-stu-id="c5c0c-153">Then, run hello following PowerShell command:</span></span>

    # Create 10 endpoints on hello scoring web service.
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpointName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

<span data-ttu-id="c5c0c-154">Most már 10 végpontok létrehoztunk Önnek, és minden bennük hello azonos betanított modell betanítása a *customer001.csv*.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-154">Now we've created 10 endpoints and they all contain hello same trained model trained on *customer001.csv*.</span></span> <span data-ttu-id="c5c0c-155">Azokat a hello Azure felügyeleti portálján tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-155">You can view them in hello Azure Management Portal.</span></span>

![Kép](./media/machine-learning-create-models-and-endpoints-with-powershell/created-endpoints.png)

## <a name="update-hello-endpoints-toouse-separate-training-datasets-using-powershell"></a><span data-ttu-id="c5c0c-157">Frissítse a hello végpontok toouse külön tanítási adathalmazt PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="c5c0c-157">Update hello endpoints toouse separate training datasets using PowerShell</span></span>
<span data-ttu-id="c5c0c-158">hello következő lépésre tooupdate hello végpontok minden ügyfélnél egyedi adatokat a egyedileg betanítása modellekkel.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-158">hello next step is tooupdate hello endpoints with models uniquely trained on each customer's individual data.</span></span> <span data-ttu-id="c5c0c-159">Azonban először meg kell tooproduce ezek a modellek az hello **kerékpárt bérleti képzési** webes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-159">But first we need tooproduce these models from hello **Bike Rental Training** web service.</span></span> <span data-ttu-id="c5c0c-160">Ugorjunk vissza toohello **kerékpárt bérleti képzési** webes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-160">Let's go back toohello **Bike Rental Training** web service.</span></span> <span data-ttu-id="c5c0c-161">A BES végpont 10-szer adatkészletekkel 10 különböző képzési rendelés tooproduce 10 különböző modellek igazolnia kell toocall.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-161">We need toocall its BES endpoint 10 times with 10 different training datasets in order tooproduce 10 different models.</span></span> <span data-ttu-id="c5c0c-162">Hello fogjuk használni **InovkeAmlWebServiceBESEndpoint** PowerShell parancsmag toodo ez.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-162">We'll use hello **InovkeAmlWebServiceBESEndpoint** PowerShell cmdlet toodo this.</span></span>

<span data-ttu-id="c5c0c-163">Is szüksége lesz tooprovide hitelesítő adatokat a blob storage-fiók be `$configContent`, nevezetesen: hello mezők `AccountName`, `AccountKey` és `RelativeLocation`.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-163">You will also need tooprovide credentials for your blob storage account into `$configContent`, namely, at hello fields `AccountName`, `AccountKey` and `RelativeLocation`.</span></span> <span data-ttu-id="c5c0c-164">Hello `AccountName` egyike lehet a számítógépfiók-nevét, mint a hello **klasszikus Azure felügyeleti portálon** (*tárolási* lap).</span><span class="sxs-lookup"><span data-stu-id="c5c0c-164">hello `AccountName` can be one of your account names, as seen in hello **Classic Azure Management Portal** (*Storage* tab).</span></span> <span data-ttu-id="c5c0c-165">A tárfiók kattintva a `AccountKey` lenyomásával hello található **elérési kulcsok kezelése** hello alsó és hello Másolás gombra *elsődleges elérési kulcsot*.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-165">Once you click on a storage account, its `AccountKey` can be found by pressing hello **Manage Access Keys** button at hello bottom and copying hello *Primary Access Key*.</span></span> <span data-ttu-id="c5c0c-166">Hello `RelativeLocation` az hello elérési útja relatív tooyour tároló új modell tárolásához.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-166">hello `RelativeLocation` is hello path relative tooyour storage where a new model will be stored.</span></span> <span data-ttu-id="c5c0c-167">Például hello elérési `hai/retrain/bike_rental/` hello parancsfájlban pontok tooa tároló alatt `hai`, és `/retrain/bike_rental/` találhatók.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-167">For instance, hello path `hai/retrain/bike_rental/` in hello script below points tooa container named `hai`, and `/retrain/bike_rental/` are subfolders.</span></span> <span data-ttu-id="c5c0c-168">Jelenleg nem hozható létre almappákat hello portálon felhasználói felületén keresztül, de nincsenek [több Azure Tártallózók](../storage/common/storage-explorers.md) , amelyek lehetővé teszik toodo stb.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-168">Currently, you cannot create subfolders through hello portal UI, but there are [several Azure Storage Explorers](../storage/common/storage-explorers.md) that allow you toodo so.</span></span> <span data-ttu-id="c5c0c-169">Javasoljuk, hogy hozzon létre egy új tároló a tárolási toostore hello az új betanított modellek (.ilearner fájlok) az alábbiak szerint: a tároló lapon kattintson a hello **Hozzáadás** hello alsó gombra, és adjon neki nevet `retrain`.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-169">It is recommended that you create a new container in your storage toostore hello new trained models (.ilearner files) as follows: from your storage page, click on hello **Add** button at hello bottom and name it `retrain`.</span></span> <span data-ttu-id="c5c0c-170">Összefoglalva, a szükséges módosításokat hello toohello az alábbi parancsfájl említett adat túl`AccountName`, `AccountKey` és `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).</span><span class="sxs-lookup"><span data-stu-id="c5c0c-170">In summary, hello necassary changes toohello script below pertain too`AccountName`, `AccountKey` and `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).</span></span>

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
> <span data-ttu-id="c5c0c-171">hello BES végpont hello csak akkor támogatja a mód ehhez a művelethez.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-171">hello BES endpoint is hello only supported mode for this operation.</span></span> <span data-ttu-id="c5c0c-172">Betanított modellek előállító RR-EKET nem használható.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-172">RRS cannot be used for producing trained models.</span></span>
> 
> 

<span data-ttu-id="c5c0c-173">10 különböző BES feladat konfigurációs json-fájlokat, létrehozása helyett a fent látható azt dinamikusan hozzon létre helyette hello config karakterlánc, és toohello hírcsatorna azt *jobConfigString* hello paramétere  **InvokeAmlWebServceBESEndpoint** parancsmaggal, mert nincs valóban nincs szükség tookeep egy másolatot a lemezen.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-173">As you can see above, instead of constructing 10 different BES job configuration json files, we dynamically create hello config string instead and feed it toohello *jobConfigString* parameter of hello **InvokeAmlWebServceBESEndpoint** cmdlet, since there is really no need tookeep a copy on disk.</span></span>

<span data-ttu-id="c5c0c-174">Ha minden megfelelően működik, egy kis idő után kell megjelennie 10 .ilearner fájlok, a *model001.ilearner* túl*model010.ilearner*, az Azure-tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-174">If everything goes well, after a while you should see 10 .ilearner files, from *model001.ilearner* too*model010.ilearner*, in your Azure storage account.</span></span> <span data-ttu-id="c5c0c-175">Most már készen áll a tooupdate a rendszer a 10 pontozási webes szolgáltatás végpontjait ezek a modellek hello segítségével a **javítás-AmlWebServiceEndpoint** PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-175">Now we're ready tooupdate our 10 scoring web service endpoints with these models using hello **Patch-AmlWebServiceEndpoint** PowerShell cmdlet.</span></span> <span data-ttu-id="c5c0c-176">Ne felejtse el újra, hogy a Microsoft csak javítás a programozott módon korábban létrehozott hello nem alapértelmezett végpontok.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-176">Remember again that we can only patch hello non-default endpoints we programmatically created earlier.</span></span>

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

<span data-ttu-id="c5c0c-177">Ez meglehetősen gyorsan fusson.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-177">This should run fairly quickly.</span></span> <span data-ttu-id="c5c0c-178">Hello végrehajtásának befejezése után fog sikeresen létrehoztunk 10 prediktív webszolgáltatási végpontok, egy egyedi módon betanítása hello dataset adott tooa bérleti helyükre, minden olyan egyetlen tanítási kísérletet a betanított modell tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-178">When hello execution finishes, we'll have successfully created 10 predictive web service endpoints, each containing a trained model uniquely trained on hello dataset specific tooa rental location, all from a single training experiment.</span></span> <span data-ttu-id="c5c0c-179">tooverify, próbálja meg ezeket a végpontokat hello segítségével hívja **InvokeAmlWebServiceRRSEndpoint** parancsmagot, hogy biztosít számukra a hello azonos bemeneti adatai, és toosee különböző előrejelzés eredmények számíthat óta hello modellek különböző képzési készletekkel képezni.</span><span class="sxs-lookup"><span data-stu-id="c5c0c-179">tooverify this, you can try calling these endpoints using hello **InvokeAmlWebServiceRRSEndpoint** cmdlet, providing them with hello same input data, and you should expect toosee different prediction results since hello models are trained with different training sets.</span></span>

## <a name="full-powershell-script"></a><span data-ttu-id="c5c0c-180">Teljes PowerShell-parancsfájl</span><span class="sxs-lookup"><span data-stu-id="c5c0c-180">Full PowerShell script</span></span>
<span data-ttu-id="c5c0c-181">Hello teljes forráskód hello listája itt található:</span><span class="sxs-lookup"><span data-stu-id="c5c0c-181">Here's hello listing of hello full source code:</span></span>

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
