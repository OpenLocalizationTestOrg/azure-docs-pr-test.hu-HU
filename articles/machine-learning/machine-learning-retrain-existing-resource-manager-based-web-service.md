---
title: "egy meglévő prediktív aaaRetrain webszolgáltatás |} Microsoft Docs"
description: "Ismerje meg, hogyan tooretrain modell és a frissítés hello web service toouse hello újonnan betanított modell az Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: cc4c26a2-5672-4255-a767-cfd971e46775
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: v-donglo
ms.openlocfilehash: fb0760d0a2adc34fc5f3df1ae41bdac075f91bf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="retrain-an-existing-predictive-web-service"></a><span data-ttu-id="ae6a0-103">Meglévő prediktív webszolgáltatás újratanítása</span><span class="sxs-lookup"><span data-stu-id="ae6a0-103">Retrain an existing predictive web service</span></span>
<span data-ttu-id="ae6a0-104">Ez a dokumentum ismerteti a folyamatot a következő forgatókönyv hello átképezési hello:</span><span class="sxs-lookup"><span data-stu-id="ae6a0-104">This document describes hello retraining process for hello following scenario:</span></span>

* <span data-ttu-id="ae6a0-105">Rendelkezik egy tanítási kísérletet, és egy prediktív kísérletté központilag telepített operationalized webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-105">You have a training experiment and a predictive experiment that you have deployed as an operationalized web service.</span></span>
* <span data-ttu-id="ae6a0-106">Új adatokat, hogy a prediktív web service toouse tooperform a pontozási rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-106">You have new data that you want your predictive web service toouse tooperform its scoring.</span></span>

> [!NOTE] 
> <span data-ttu-id="ae6a0-107">toodeploy egy új webszolgáltatás-bővítmény, megfelelő engedélyekkel kell rendelkeznie a hello előfizetés toowhich meg hello webes szolgáltatás telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-107">toodeploy a New web service you must have sufficient permissions in hello subscription toowhich you deploying hello web service.</span></span> <span data-ttu-id="ae6a0-108">További információ: [egy webszolgáltatás-bővítmény hello Azure Machine Learning webszolgáltatások portálon kezelheti](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="ae6a0-108">For more information see, [Manage a Web service using hello Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="ae6a0-109">A meglévő webszolgáltatás és kísérletek verziótól kezdődően kell toofollow ezeket a lépéseket:</span><span class="sxs-lookup"><span data-stu-id="ae6a0-109">Starting with your existing web service and experiments, you need toofollow these steps:</span></span>

1. <span data-ttu-id="ae6a0-110">Frissítési hello modell.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-110">Update hello model.</span></span>
   1. <span data-ttu-id="ae6a0-111">A webszolgáltatás bemenetei és kimenetei a tanítási kísérletet tooallow módosítása.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-111">Modify your training experiment tooallow for web service inputs and outputs.</span></span>
   2. <span data-ttu-id="ae6a0-112">Hello tanítási kísérletet egy megőrzési webszolgáltatás-bővítmény telepítése.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-112">Deploy hello training experiment as a retraining web service.</span></span>
   3. <span data-ttu-id="ae6a0-113">Hello tanítási kísérletet kötegelt végrehajtási szolgáltatás (BES) tooretrain hello modellt használja.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-113">Use hello training experiment's Batch Execution Service (BES) tooretrain hello model.</span></span>
2. <span data-ttu-id="ae6a0-114">Hello Azure Machine Learning PowerShell parancsmagok tooupdate hello prediktív kísérletté használja.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-114">Use hello Azure Machine Learning PowerShell cmdlets tooupdate hello predictive experiment.</span></span>
   1. <span data-ttu-id="ae6a0-115">Jelentkezzen be Azure Resource Manager fiók tooyour.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-115">Sign in tooyour Azure Resource Manager account.</span></span>
   2. <span data-ttu-id="ae6a0-116">Hello webszolgáltatás-definíciójának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-116">Get hello web service definition.</span></span>
   3. <span data-ttu-id="ae6a0-117">Exportálja a hello webszolgáltatás-definíciójának JSON-ként.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-117">Export hello web service definition as JSON.</span></span>
   4. <span data-ttu-id="ae6a0-118">Frissítés hello hivatkozás toohello ilearner blob hello JSON.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-118">Update hello reference toohello ilearner blob in hello JSON.</span></span>
   5. <span data-ttu-id="ae6a0-119">Hello JSON importálnia kell a webszolgáltatás-definíciójának.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-119">Import hello JSON into a web service definition.</span></span>
   6. <span data-ttu-id="ae6a0-120">Hello webszolgáltatás frissítése egy új webszolgáltatás-definíciójának.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-120">Update hello web service with a new web service definition.</span></span>

## <a name="deploy-hello-training-experiment"></a><span data-ttu-id="ae6a0-121">Hello tanítási kísérletet telepítése</span><span class="sxs-lookup"><span data-stu-id="ae6a0-121">Deploy hello training experiment</span></span>
<span data-ttu-id="ae6a0-122">toodeploy hello tanítási kísérletet megőrzési webszolgáltatásként, hozzá kell adnia a webszolgáltatás bemenetei és kimenetei toohello szolgáltatásmodellt.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-122">toodeploy hello training experiment as a retraining web service, you must add web service inputs and outputs toohello model.</span></span> <span data-ttu-id="ae6a0-123">Csatlakozzon egy *webes szolgáltatás kimeneti* modul toohello kísérlet  *[tanítási modell] [ train-model]*  hello kísérlet képzési modul, engedélyezése tooproduce új betanított modell, amely a prediktív kísérletté is használhatja.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-123">By connecting a *Web Service Output* module toohello experiment's *[Train Model][train-model]* module, you enable hello training experiment tooproduce a new trained model that you can use in your predictive experiment.</span></span> <span data-ttu-id="ae6a0-124">Ha rendelkezik egy *modell kiértékelése* modul, webes szolgáltatás kimeneti tooget hello kiértékelésének eredménye kimenetként is csatolható.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-124">If you have an *Evaluate Model* module, you can also attach web service output tooget hello evaluation results as output.</span></span>

<span data-ttu-id="ae6a0-125">tooupdate a tanítási kísérletet:</span><span class="sxs-lookup"><span data-stu-id="ae6a0-125">tooupdate your training experiment:</span></span>

1. <span data-ttu-id="ae6a0-126">Csatlakozás egy *Web Service bemeneti* bemeneti modul tooyour adatokat (például egy *Clean Missing Data* modul).</span><span class="sxs-lookup"><span data-stu-id="ae6a0-126">Connect a *Web Service Input* module tooyour data input (for example, a *Clean Missing Data* module).</span></span> <span data-ttu-id="ae6a0-127">Általában azt szeretné, hogy a bemeneti adatok feldolgozása a tooensure hello ugyanaz, mint az eredeti betanítási adatok.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-127">Typically, you want tooensure that your input data is processed in hello same way as your original training data.</span></span>
2. <span data-ttu-id="ae6a0-128">Csatlakozás egy *webes szolgáltatás kimeneti* modul toohello kimenetét a *tanítási modell* modul.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-128">Connect a *Web Service Output* module toohello output of your *Train Model* module.</span></span>
3. <span data-ttu-id="ae6a0-129">Ha rendelkezik egy *modell kiértékelése* modul, és szeretné, hogy toooutput hello kiértékelésének eredménye, csatlakozzon a *webes szolgáltatás kimeneti* modul toohello kimenetét a *modell kiértékelése* a modul.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-129">If you have an *Evaluate Model* module and you want toooutput hello evaluation results, connect a *Web Service Output* module toohello output of your *Evaluate Model* module.</span></span>

<span data-ttu-id="ae6a0-130">Futtassa a kísérletet.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-130">Run your experiment.</span></span>

<span data-ttu-id="ae6a0-131">A következő hello tanítási kísérletet kell telepítenie egy webszolgáltatás, amely létrehozza a modell betanítását és modell kiértékelésének eredménye.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-131">Next, you must deploy hello training experiment as a web service that produces a trained model and model evaluation results.</span></span>  

<span data-ttu-id="ae6a0-132">A kísérletvászonra hello hello alján kattintson **webes szolgáltatások beállítása**, majd válassza ki **[Új] webes szolgáltatás telepítése**.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-132">At hello bottom of hello experiment canvas, click **Set Up Web Service**, and then select **Deploy Web Service [New]**.</span></span> <span data-ttu-id="ae6a0-133">hello Azure Machine Learning webszolgáltatások portál megnyitja toohello **webes szolgáltatás telepítése** lap.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-133">hello Azure Machine Learning Web Services portal opens toohello **Deploy Web Service** page.</span></span> <span data-ttu-id="ae6a0-134">Adjon meg egy nevet a webszolgáltatáshoz, fizetési csomag kiválasztása, és kattintson **telepítés**.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-134">Type a name for your web service, choose a payment plan, and then click **Deploy**.</span></span> <span data-ttu-id="ae6a0-135">Hello kötegelt végrehajtási metódus csak a betanított modellek létrehozásához használható.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-135">You can only use hello Batch Execution method for creating trained models.</span></span>

## <a name="retrain-hello-model-with-new-data-by-using-bes"></a><span data-ttu-id="ae6a0-136">Az új adatokat hello modell újratanítása BES használatával</span><span class="sxs-lookup"><span data-stu-id="ae6a0-136">Retrain hello model with new data by using BES</span></span>
<span data-ttu-id="ae6a0-137">Ebben a példában a C# toocreate hello alkalmazás átképezési használunk.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-137">For this example, we're using C# toocreate hello retraining application.</span></span> <span data-ttu-id="ae6a0-138">Python vagy R minta kód tooaccomplish ezt a feladatot is használjon.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-138">You can also use Python or R sample code tooaccomplish this task.</span></span>

<span data-ttu-id="ae6a0-139">toocall hello átképezési API-kat:</span><span class="sxs-lookup"><span data-stu-id="ae6a0-139">toocall hello retraining APIs:</span></span>

1. <span data-ttu-id="ae6a0-140">Hozzon létre egy C# konzolalkalmazást a Visual Studio: **új** > **projekt** > **Visual C#** > **klasszikus Windows asztal** > **Konzolalkalmazás (.NET-keretrendszer)**.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-140">Create a C# console application in Visual Studio: **New** > **Project** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.</span></span>
2. <span data-ttu-id="ae6a0-141">Jelentkezzen be toohello a Machine Learning webszolgáltatások portálon.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-141">Sign in toohello Machine Learning Web Services portal.</span></span>
3. <span data-ttu-id="ae6a0-142">Kattintson a hello webszolgáltatás, amelyet dolgozunk.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-142">Click hello web service that you're working with.</span></span>
4. <span data-ttu-id="ae6a0-143">Kattintson a **felhasználásához**.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-143">Click **Consume**.</span></span>
5. <span data-ttu-id="ae6a0-144">Hello hello alján **felhasználás** lap hello **mintakód** kattintson **kötegelt**.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-144">At hello bottom of hello **Consume** page, in hello **Sample Code** section, click **Batch**.</span></span>
6. <span data-ttu-id="ae6a0-145">A kötegelt végrehajtás hello C# mintakód másolja, majd illessze be hello Program.cs fájlra.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-145">Copy hello sample C# code for batch execution and paste it into hello Program.cs file.</span></span> <span data-ttu-id="ae6a0-146">Győződjön meg arról, hogy hello névtér nem módosulnak.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-146">Make sure that hello namespace remains intact.</span></span>

<span data-ttu-id="ae6a0-147">Adja hozzá a hello Microsoft.AspNet.WebApi.Client, NuGet-csomagot a hello megjegyzések.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-147">Add hello NuGet package Microsoft.AspNet.WebApi.Client, as specified in hello comments.</span></span> <span data-ttu-id="ae6a0-148">tooadd hello hivatkozás tooMicrosoft.WindowsAzure.Storage.dll, először meg kell tooinstall hello [ügyféloldali kódtára a Azure Storage szolgáltatás](https://www.nuget.org/packages/WindowsAzure.Storage).</span><span class="sxs-lookup"><span data-stu-id="ae6a0-148">tooadd hello reference tooMicrosoft.WindowsAzure.Storage.dll, you might first need tooinstall hello [client library for Azure Storage services](https://www.nuget.org/packages/WindowsAzure.Storage).</span></span>

<span data-ttu-id="ae6a0-149">hello alábbi képernyőfelvételen látható hello **felhasználás** hello Azure Machine Learning webszolgáltatások lapjára.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-149">hello following screenshot shows hello **Consume** page in hello Azure Machine Learning Web Services portal.</span></span>

![Lap felhasználása][1]

### <a name="update-hello-apikey-declaration"></a><span data-ttu-id="ae6a0-151">Hello apikey nyilatkozat frissítése</span><span class="sxs-lookup"><span data-stu-id="ae6a0-151">Update hello apikey declaration</span></span>
<span data-ttu-id="ae6a0-152">Keresse meg a hello **apikey** deklarációjában:</span><span class="sxs-lookup"><span data-stu-id="ae6a0-152">Locate hello **apikey** declaration:</span></span>

    const string apiKey = "abc123"; // Replace this with hello API key for hello web service

<span data-ttu-id="ae6a0-153">A hello **alapvető fogyasztási adatai** hello szakasza **felhasználás** lapon hello elsődleges kulcs keresse meg és másolja azt toohello **apikey** nyilatkozatot.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-153">In hello **Basic consumption info** section of hello **Consume** page, locate hello primary key and copy it toohello **apikey** declaration.</span></span>

### <a name="update-hello-azure-storage-information"></a><span data-ttu-id="ae6a0-154">Hello Azure Storage-adatainak módosítása</span><span class="sxs-lookup"><span data-stu-id="ae6a0-154">Update hello Azure Storage information</span></span>
<span data-ttu-id="ae6a0-155">hello BES mintakód feltölt egy fájlt egy helyi meghajtó (például "C:\temp\CensusIpnput.csv") tooAzure tárolási, folyamatokat engedélyez, és írja a hello eredmények hátsó tooAzure tárolási.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-155">hello BES sample code uploads a file from a local drive (for example, "C:\temp\CensusIpnput.csv") tooAzure Storage, processes it, and writes hello results back tooAzure Storage.</span></span>  

<span data-ttu-id="ae6a0-156">tooupdate hello Azure tárolással kapcsolatos, le kell kérni a tárfiók a klasszikus Azure portálon hello nevét, a kulcs és a tároló információkat, majd a frissítés hello correspondi a kísérlet futtatása után hello eredő hello tárfiók munkafolyamat hasonló toohello következő legyen:</span><span class="sxs-lookup"><span data-stu-id="ae6a0-156">tooupdate hello Azure Storage information, you must retrieve hello storage account name, key, and container information for your storage account from hello Azure classic portal, and then update hello correspondi After running your experiment, hello resulting workflow should be similar toohello following:</span></span>

![Eredményül kapott munkafolyamat futtatása után][4]<span data-ttu-id="ae6a0-158">hello kód ng értékek.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-158">ng values in hello code.</span></span>

1. <span data-ttu-id="ae6a0-159">Jelentkezzen be toohello a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-159">Sign in toohello Azure classic portal.</span></span>
2. <span data-ttu-id="ae6a0-160">Kattintson a bal oldali oszlopban hello **tárolási**.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-160">In hello left navigation column, click **Storage**.</span></span>
3. <span data-ttu-id="ae6a0-161">Hello listában tárfiókok jelölje be egy toostore hello retrained modell.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-161">From hello list of storage accounts, select one toostore hello retrained model.</span></span>
4. <span data-ttu-id="ae6a0-162">Hello a hello lap alján, kattintson **elérési kulcsok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-162">At hello bottom of hello page, click **Manage Access Keys**.</span></span>
5. <span data-ttu-id="ae6a0-163">Másolja ki és mentse a hello **elsődleges elérési kulcsot** és Bezárás hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-163">Copy and save hello **Primary Access Key** and close hello dialog.</span></span>
6. <span data-ttu-id="ae6a0-164">Hello hello oldal tetején, kattintson a **tárolók**.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-164">At hello top of hello page, click **Containers**.</span></span>
7. <span data-ttu-id="ae6a0-165">Egy meglévő tárolóhoz, válassza ki vagy hozzon létre egy újat, és mentse hello nevét.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-165">Select an existing container, or create a new one and save hello name.</span></span>

<span data-ttu-id="ae6a0-166">Keresse meg a hello *StorageAccountName*, *StorageAccountKey*, és *StorageContainerName* nyilatkozatok, és a frissítés hello értékek mentett hello klasszikus portálon .</span><span class="sxs-lookup"><span data-stu-id="ae6a0-166">Locate hello *StorageAccountName*, *StorageAccountKey*, and *StorageContainerName* declarations, and update hello values that you saved from hello classic portal.</span></span>

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

<span data-ttu-id="ae6a0-167">Is gondoskodnia kell arról, hogy hello bemeneti fájl áll rendelkezésre a hello kódban megadott hello helyen.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-167">You also must ensure that hello input file is available at hello location that you specify in hello code.</span></span>

### <a name="specify-hello-output-location"></a><span data-ttu-id="ae6a0-168">Hello kimeneti helyének megadása</span><span class="sxs-lookup"><span data-stu-id="ae6a0-168">Specify hello output location</span></span>
<span data-ttu-id="ae6a0-169">A kérelem hasznos hello hello kimeneti hely megadása esetén a megadott hello fájl kiterjesztése hello *RelativeLocation* kell megadni, `ilearner`.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-169">When you specify hello output location in hello Request Payload, hello extension of hello file that is specified in *RelativeLocation* must be specified as `ilearner`.</span></span> <span data-ttu-id="ae6a0-170">Tekintse meg a következő példa hello:</span><span class="sxs-lookup"><span data-stu-id="ae6a0-170">See hello following example:</span></span>

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with hello location you want toouse for your output file and a valid file extension (usually .csv for scoring results or .ilearner for trained models)*/
            }
        },

<span data-ttu-id="ae6a0-171">hello az alábbiakban látható egy példa kimenet átképezési: ![kimeneti Átképezési][6]</span><span class="sxs-lookup"><span data-stu-id="ae6a0-171">hello following is an example of retraining output: ![Retraining output][6]</span></span>

## <a name="evaluate-hello-retraining-results"></a><span data-ttu-id="ae6a0-172">Eredmények átképezési hello kiértékelése</span><span class="sxs-lookup"><span data-stu-id="ae6a0-172">Evaluate hello retraining results</span></span>
<span data-ttu-id="ae6a0-173">Hello alkalmazás futtatásakor hello kimenete hello URL-cím és a megosztott hozzáférési aláírásokkal jogkivonat, amelyek a szükséges tooaccess hello kiértékelésének eredménye.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-173">When you run hello application, hello output includes hello URL and shared access signatures token that are necessary tooaccess hello evaluation results.</span></span>

<span data-ttu-id="ae6a0-174">Megtekintheti a hello teljesítménymérési eredményeket hello retrained modell hello kombinálásával *BaseLocation*, *RelativeLocation*, és *SasBlobToken* hello kimeneti eredmények a *output2* (ahogy az az előző kimeneti kép átképezési hello szerepel), és be hello teljes URL-CÍMÉT a böngésző címsorában hello.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-174">You can see hello performance results of hello retrained model by combining hello *BaseLocation*, *RelativeLocation*, and *SasBlobToken* from hello output results for *output2* (as shown in hello preceding retraining output image) and pasting hello complete URL into hello browser address bar.</span></span>  

<span data-ttu-id="ae6a0-175">Hello eredmények toodetermine akkor megvizsgálja, hogy hello újonnan betanított modell is elegendő egy meglévő tooreplace hello hajt végre.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-175">Examine hello results toodetermine whether hello newly trained model performs well enough tooreplace hello existing one.</span></span>

<span data-ttu-id="ae6a0-176">Másolás hello *BaseLocation*, *RelativeLocation*, és *SasBlobToken* hello kimeneti eredmények közül.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-176">Copy hello *BaseLocation*, *RelativeLocation*, and *SasBlobToken* from hello output results.</span></span>

## <a name="retrain-hello-web-service"></a><span data-ttu-id="ae6a0-177">Hello webszolgáltatás újratanítása</span><span class="sxs-lookup"><span data-stu-id="ae6a0-177">Retrain hello web service</span></span>
<span data-ttu-id="ae6a0-178">Ha újratanítása egy új webszolgáltatás-bővítmény, hello prediktív webes szolgáltatás definíciós tooreference hello új betanított modell frissítenie.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-178">When you retrain a new web service, you update hello predictive web service definition tooreference hello new trained model.</span></span> <span data-ttu-id="ae6a0-179">hello webszolgáltatás-definíciójának hello betanított modell hello webszolgáltatás belső másolatát, és nem módosítható közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-179">hello web service definition is an internal representation of hello trained model of hello web service and is not directly modifiable.</span></span> <span data-ttu-id="ae6a0-180">Győződjön meg arról, hogy vannak-e hello webszolgáltatás-definíciójának beolvasása a prediktív kísérletté és nem a tanítási kísérletet.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-180">Make sure that you are retrieving hello web service definition for your predictive experiment and not your training experiment.</span></span>

## <a name="sign-in-tooazure-resource-manager"></a><span data-ttu-id="ae6a0-181">Jelentkezzen be tooAzure erőforrás-kezelő</span><span class="sxs-lookup"><span data-stu-id="ae6a0-181">Sign in tooAzure Resource Manager</span></span>
<span data-ttu-id="ae6a0-182">Akkor először be kell jelentkeznie az Azure-hello PowerShell környezetben a fiók tooyour hello segítségével [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-182">You must first sign in tooyour Azure account from within hello PowerShell environment by using hello [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span></span>

## <a name="get-hello-web-service-definition-object"></a><span data-ttu-id="ae6a0-183">Hello webszolgáltatás-definíciójának objektum</span><span class="sxs-lookup"><span data-stu-id="ae6a0-183">Get hello Web Service Definition object</span></span>
<span data-ttu-id="ae6a0-184">Következő lépésként hozza hello webszolgáltatás-definíciójának objektum hívó hello [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-184">Next, get hello Web Service Definition object by calling hello [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span></span>

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

<span data-ttu-id="ae6a0-185">toodetermine hello erőforráscsoport neve egy meglévő webszolgáltatás hello Get-AzureRmMlWebService parancsmag nélkül paraméterek toodisplay hello webes szolgáltatások futtatása az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-185">toodetermine hello resource group name of an existing web service, run hello Get-AzureRmMlWebService cmdlet without any parameters toodisplay hello web services in your subscription.</span></span> <span data-ttu-id="ae6a0-186">Keresse meg a hello webes szolgáltatás, és tekintse meg a webes szolgáltatás azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-186">Locate hello web service, and then look at its web service ID.</span></span> <span data-ttu-id="ae6a0-187">hello erőforráscsoport hello neve nem a hello azonosítója hello eleme után hello *resourceGroups* elemet.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-187">hello name of hello resource group is hello fourth element in hello ID, just after hello *resourceGroups* element.</span></span> <span data-ttu-id="ae6a0-188">A következő példa hello hello erőforráscsoport neve alapértelmezett-MachineLearning-SouthCentralUS.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-188">In hello following example, hello resource group name is Default-MachineLearning-SouthCentralUS.</span></span>

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

<span data-ttu-id="ae6a0-189">Másik lehetőségként toodetermine hello erőforráscsoport-név egy létező webszolgáltatás, jelentkezzen be Azure Machine Learning webszolgáltatások portal toohello.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-189">Alternatively, toodetermine hello resource group name of an existing web service, sign in toohello Azure Machine Learning Web Services portal.</span></span> <span data-ttu-id="ae6a0-190">Válassza ki a hello webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-190">Select hello web service.</span></span> <span data-ttu-id="ae6a0-191">hello erőforráscsoport neve hello webszolgáltatás, hello URL-CÍMÉT hello ötödik eleme után hello *resourceGroups* elemet.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-191">hello resource group name is hello fifth element of hello URL of hello web service, just after hello *resourceGroups* element.</span></span> <span data-ttu-id="ae6a0-192">A következő példa hello hello erőforráscsoport neve alapértelmezett-MachineLearning-SouthCentralUS.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-192">In hello following example, hello resource group name is Default-MachineLearning-SouthCentralUS.</span></span>

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-hello-web-service-definition-object-as-json"></a><span data-ttu-id="ae6a0-193">JSON-ként hello webszolgáltatás-definíciójának objektum exportálása</span><span class="sxs-lookup"><span data-stu-id="ae6a0-193">Export hello Web Service Definition object as JSON</span></span>
<span data-ttu-id="ae6a0-194">hello betanított modell toouse hello toomodify hello definíciója újonnan modell betanítása, előbb használnia kell hello [Export-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) parancsmag tooexport azt tooa JSON-formátumú fájlt.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-194">toomodify hello definition of hello trained model toouse hello newly trained model, you must first use hello [Export-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) cmdlet tooexport it tooa JSON-format file.</span></span>

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-hello-reference-toohello-ilearner-blob"></a><span data-ttu-id="ae6a0-195">Hello hivatkozás toohello ilearner blob frissítése</span><span class="sxs-lookup"><span data-stu-id="ae6a0-195">Update hello reference toohello ilearner blob</span></span>
<span data-ttu-id="ae6a0-196">Az eszközök, a hello keresse meg a hello [betanított modell], frissítés hello *uri* hello érték *locationInfo* hello hello ilearner BLOB URI-csomópont.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-196">In hello assets, locate hello [trained model], update hello *uri* value in hello *locationInfo* node with hello URI of hello ilearner blob.</span></span> <span data-ttu-id="ae6a0-197">hello URI létrejön hello kombinálásával *BaseLocation* és hello *RelativeLocation* a BES megőrzési hívás hello hello kimenetét.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-197">hello URI is generated by combining hello *BaseLocation* and hello *RelativeLocation* from hello output of hello BES retraining call.</span></span>

     "asset3": {
        "name": "Retrain Sample [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-hello-json-into-a-web-service-definition-object"></a><span data-ttu-id="ae6a0-198">A webszolgáltatás-definíciójának objektum hello JSON importálása</span><span class="sxs-lookup"><span data-stu-id="ae6a0-198">Import hello JSON into a Web Service Definition object</span></span>
<span data-ttu-id="ae6a0-199">Hello kell használnia [Import-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) parancsmag tooconvert hello JSON-fájl módosító vissza objektummá egy webszolgáltatás-definíciójának használható tooupdate hello predicative kísérlet.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-199">You must use hello [Import-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) cmdlet tooconvert hello modified JSON file back into a Web Service Definition object that you can use tooupdate hello predicative experiment.</span></span>

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-hello-web-service"></a><span data-ttu-id="ae6a0-200">Hello webes szolgáltatás frissítése</span><span class="sxs-lookup"><span data-stu-id="ae6a0-200">Update hello web service</span></span>
<span data-ttu-id="ae6a0-201">Végül, használja a hello [frissítés-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) parancsmag tooupdate hello prediktív kísérletté.</span><span class="sxs-lookup"><span data-stu-id="ae6a0-201">Finally, use hello [Update-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) cmdlet tooupdate hello predictive experiment.</span></span>

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

[1]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-consume-page.png
[4]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE04.png
[6]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE06.png

<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
