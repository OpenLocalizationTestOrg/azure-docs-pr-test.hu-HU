---
title: "aaaRetrain Machine Learning modellek szoftveres átképezése |} Microsoft Docs"
description: "Ismerje meg, hogyan tooprogrammatically újratanítása a modell és a frissítés hello webes toouse hello újonnan betanított modell az Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: raymondlaghaeian
manager: jhubbard
editor: cgronlun
ms.assetid: 7ae4f977-e6bf-4d04-9dde-28a66ce7b664
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: raymondl;garye;v-donglo
ms.openlocfilehash: edbb64c08f7d9edf3c76e23e0cc7e14c0125d697
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="retrain-machine-learning-models-programmatically"></a><span data-ttu-id="f9652-103">Azure Machine Learning-modellek szoftveres átképezése</span><span class="sxs-lookup"><span data-stu-id="f9652-103">Retrain Machine Learning models programmatically</span></span>
<span data-ttu-id="f9652-104">Ez a forgatókönyv megtudhatja, hogyan tooprogrammatically működik az Azure Machine Learning webszolgáltatás C# és hello Machine Learning kötegelt végrehajtási szolgáltatás használatával.</span><span class="sxs-lookup"><span data-stu-id="f9652-104">In this walkthrough, you will learn how tooprogrammatically retrain an Azure Machine Learning Web Service using C# and hello Machine Learning Batch Execution service.</span></span>

<span data-ttu-id="f9652-105">Hello modell rendelkezik retrained, a következő forgatókönyvek hello megjelenni hogyan tooupdate hello modell a prediktív webszolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="f9652-105">Once you have retrained hello model, hello following walkthroughs show how tooupdate hello model in your predictive web service:</span></span>

* <span data-ttu-id="f9652-106">Ha telepítette a klasszikus webszolgáltatás hello Machine Learning webszolgáltatások portálon, lásd: [egy klasszikus webszolgáltatás újratanítása](machine-learning-retrain-a-classic-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="f9652-106">If you deployed a Classic web service in hello Machine Learning Web Services portal, see [Retrain a Classic web service](machine-learning-retrain-a-classic-web-service.md).</span></span> 
* <span data-ttu-id="f9652-107">Ha egy új webszolgáltatás-bővítmény központi telepítése, lásd: [egy új webszolgáltatás-bővítmény hello Machine Learning Management-parancsmagok használatával újratanítása](machine-learning-retrain-new-web-service-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="f9652-107">If you deployed a New web service, see [Retrain a New web service using hello Machine Learning Management cmdlets](machine-learning-retrain-new-web-service-using-powershell.md).</span></span>

<span data-ttu-id="f9652-108">Folyamat átképezési hello áttekintését lásd: [a gépi tanulási modellek újratanítása](machine-learning-retrain-machine-learning-model.md).</span><span class="sxs-lookup"><span data-stu-id="f9652-108">For an overview of hello retraining process, see [Retrain a Machine Learning Model](machine-learning-retrain-machine-learning-model.md).</span></span>

<span data-ttu-id="f9652-109">Ha azt szeretné toostart, mint a létező új Azure Resource Manager-alapú webszolgáltatás, lásd: [meglévő prediktív webszolgáltatás újratanítása](machine-learning-retrain-existing-resource-manager-based-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="f9652-109">If you want toostart with your existing New Azure Resource Manager based web service, see [Retrain an existing Predictive web service](machine-learning-retrain-existing-resource-manager-based-web-service.md).</span></span>

## <a name="create-a-training-experiment"></a><span data-ttu-id="f9652-110">Hozzon létre egy tanítási kísérletet</span><span class="sxs-lookup"><span data-stu-id="f9652-110">Create a training experiment</span></span>
<span data-ttu-id="f9652-111">Ehhez a példához használandó "minta 5: vonat, tesztelési, Evaluate bináris osztályozási: felnőtt Dataset" hello Microsoft Azure Machine Learning minták.</span><span class="sxs-lookup"><span data-stu-id="f9652-111">For this example, you will use "Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset" from hello Microsoft Azure Machine Learning samples.</span></span> 

<span data-ttu-id="f9652-112">toocreate hello kísérlet:</span><span class="sxs-lookup"><span data-stu-id="f9652-112">toocreate hello experiment:</span></span>

1. <span data-ttu-id="f9652-113">Jelentkezzen be Azure Machine Learning Studio tooMicrosoft.</span><span class="sxs-lookup"><span data-stu-id="f9652-113">Sign into tooMicrosoft Azure Machine Learning Studio.</span></span> 
2. <span data-ttu-id="f9652-114">A hello jobb alsó sarkában hello irányítópultot, kattintson **új**.</span><span class="sxs-lookup"><span data-stu-id="f9652-114">On hello bottom right corner of hello dashboard, click **New**.</span></span>
3. <span data-ttu-id="f9652-115">Hello Microsoft Samples jelölje ki a minta 5.</span><span class="sxs-lookup"><span data-stu-id="f9652-115">From hello Microsoft Samples, select Sample 5.</span></span>
4. <span data-ttu-id="f9652-116">toorename hello kísérlet hello tetején hello kísérletvászonra, válassza ki a hello kísérlet neve "minta 5: vonat, tesztelési, Evaluate bináris osztályozási: felnőtt Dataset".</span><span class="sxs-lookup"><span data-stu-id="f9652-116">toorename hello experiment, at hello top of hello experiment canvas, select hello experiment name "Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset".</span></span>
5. <span data-ttu-id="f9652-117">Típus nyilvántartásba modell.</span><span class="sxs-lookup"><span data-stu-id="f9652-117">Type Census Model.</span></span>
6. <span data-ttu-id="f9652-118">A kísérletvászonra hello hello alján kattintson **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="f9652-118">At hello bottom of hello experiment canvas, click **Run**.</span></span>
7. <span data-ttu-id="f9652-119">Kattintson a **Set Up webszolgáltatás** válassza **webszolgáltatás Átképezési**.</span><span class="sxs-lookup"><span data-stu-id="f9652-119">Click **Set Up web service** and select **Retraining web service**.</span></span> 

<span data-ttu-id="f9652-120">hello következő hello kezdeti kísérlet jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="f9652-120">hello following shows hello initial experiment.</span></span>
   
   ![Kezdeti kísérlet.][2]


## <a name="create-a-predictive-experiment-and-publish-as-a-web-service"></a><span data-ttu-id="f9652-122">A prediktív kísérletté létrehozása és egy webszolgáltatás közzététele</span><span class="sxs-lookup"><span data-stu-id="f9652-122">Create a predictive experiment and publish as a web service</span></span>
<span data-ttu-id="f9652-123">Ezután hozzon létre egy Predicative kísérletet.</span><span class="sxs-lookup"><span data-stu-id="f9652-123">Next you create a Predicative Experiment.</span></span>

1. <span data-ttu-id="f9652-124">A kísérletvászonra hello hello alján kattintson **webes szolgáltatások beállítása** válassza **prediktív webszolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="f9652-124">At hello bottom of hello experiment canvas, click **Set Up Web Service** and select **Predictive Web Service**.</span></span> <span data-ttu-id="f9652-125">Ez a modell betanítását hello modell menti, és webes szolgáltatás bemeneti és kimeneti modulok hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="f9652-125">This saves hello model as a Trained Model and adds web service Input and Output modules.</span></span> 
2. <span data-ttu-id="f9652-126">Kattintson a **Run** (Futtatás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="f9652-126">Click **Run**.</span></span> 
3. <span data-ttu-id="f9652-127">Miután hello kísérlet futása befejeződött, kattintson **webes szolgáltatás telepítése [klasszikus]** vagy **[Új] webes szolgáltatás telepítése**.</span><span class="sxs-lookup"><span data-stu-id="f9652-127">After hello experiment has finished running, click **Deploy Web Service [Classic]** or **Deploy Web Service [New]**.</span></span>

> [!NOTE] 
> <span data-ttu-id="f9652-128">toodeploy egy új webszolgáltatás-bővítmény, megfelelő engedélyekkel kell rendelkeznie a hello előfizetés toowhich meg hello webes szolgáltatás telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="f9652-128">toodeploy a New web service you must have sufficient permissions in hello subscription toowhich you deploying hello web service.</span></span> <span data-ttu-id="f9652-129">További információ: [egy webszolgáltatás-bővítmény hello Azure Machine Learning webszolgáltatások portálon kezelheti](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="f9652-129">For more information see, [Manage a Web service using hello Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

## <a name="deploy-hello-training-experiment-as-a-training-web-service"></a><span data-ttu-id="f9652-130">Hello tanítási kísérletet egy képzési webszolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="f9652-130">Deploy hello training experiment as a Training web service</span></span>
<span data-ttu-id="f9652-131">tooretrain hello betanított modell, ha már telepítette a hello tanítási kísérletet, amelyet létrehozott Retraining webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="f9652-131">tooretrain hello trained model, you must deploy hello training experiment that you created as a Retraining web service.</span></span> <span data-ttu-id="f9652-132">Ez a webszolgáltatás kell egy *webes szolgáltatás kimeneti* modul csatlakoztatott toohello  *[tanítási modell] [ train-model]*  modul, új toobe képes tooproduce betanított modellek.</span><span class="sxs-lookup"><span data-stu-id="f9652-132">This web service needs a *Web Service Output* module connected toohello *[Train Model][train-model]* module, toobe able tooproduce new trained models.</span></span>

1. <span data-ttu-id="f9652-133">tooreturn toohello tanítási kísérletet, hello kísérletek ikonra hello bal oldali ablaktáblán, majd kattintson a nyilvántartásba modell nevű hello kísérlet.</span><span class="sxs-lookup"><span data-stu-id="f9652-133">tooreturn toohello training experiment, click hello Experiments icon in hello left pane, then click hello experiment named Census Model.</span></span>  
2. <span data-ttu-id="f9652-134">Hello keresési kísérlet elemek keresési mezőbe írja be a webes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f9652-134">In hello Search Experiment Items search box, type web service.</span></span> 
3. <span data-ttu-id="f9652-135">Húzza a *webszolgáltatás bemenetét* alakzatot hello modul vászonra kísérletek kifejlesztéséhez, és csatlakozzon a kimeneti toohello *Clean Missing Data* modul.</span><span class="sxs-lookup"><span data-stu-id="f9652-135">Drag a *Web Service Input* module onto hello experiment canvas and connect its output toohello *Clean Missing Data* module.</span></span>  <span data-ttu-id="f9652-136">Ez biztosítja, hogy a megőrzési adatfeldolgozás hello azonos módon, mint az eredeti betanítási adata.</span><span class="sxs-lookup"><span data-stu-id="f9652-136">This ensures that your retraining data is processed hello same way as your original training data.</span></span>
4. <span data-ttu-id="f9652-137">Húzza a két *webszolgáltatás kimeneti* alakzatot hello modulok kísérlet vászonra.</span><span class="sxs-lookup"><span data-stu-id="f9652-137">Drag two *web service Output* modules onto hello experiment canvas.</span></span> <span data-ttu-id="f9652-138">Csatlakozás hello hello kimenete *tanítási modell* modul tooone és hello kimenete hello *modell kiértékelése* más modul toohello.</span><span class="sxs-lookup"><span data-stu-id="f9652-138">Connect hello output of hello *Train Model* module tooone and hello output of hello *Evaluate Model* module toohello other.</span></span> <span data-ttu-id="f9652-139">a webes szolgáltatás kimeneti hello **tanítási modell** biztosítanak számunkra hello új betanított modell.</span><span class="sxs-lookup"><span data-stu-id="f9652-139">hello web service output for **Train Model** gives us hello new trained model.</span></span> <span data-ttu-id="f9652-140">csatolt túl kimeneti hello**modell kiértékelése** adja vissza, hogy a modul kimenetneve, vagyis hello teljesítménymérési eredményeket.</span><span class="sxs-lookup"><span data-stu-id="f9652-140">hello output attached too**Evaluate Model** returns that module’s output, which is hello performance results.</span></span>
5. <span data-ttu-id="f9652-141">Kattintson a **Run** (Futtatás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="f9652-141">Click **Run**.</span></span> 

<span data-ttu-id="f9652-142">Ezután egy webszolgáltatás, amely létrehozza a modell betanítását és modell kiértékelésének eredménye hello tanítási kísérletet kell telepítenie.</span><span class="sxs-lookup"><span data-stu-id="f9652-142">Next you must deploy hello training experiment as a web service that produces a trained model and model evaluation results.</span></span> <span data-ttu-id="f9652-143">Ez a következő műveletek készletét függenek e dolgozik egy klasszikus webszolgáltatás-bővítmény vagy egy új webszolgáltatás-bővítmény tooaccomplish.</span><span class="sxs-lookup"><span data-stu-id="f9652-143">tooaccomplish this, your next set of actions are dependent on whether you are working with a Classic web service or a New web service.</span></span>  

<span data-ttu-id="f9652-144">**Klasszikus webszolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="f9652-144">**Classic web service**</span></span>

<span data-ttu-id="f9652-145">Hello kísérletvászonra hello alsó részén, kattintson a **webes szolgáltatások beállítása** válassza **webes szolgáltatás telepítése [klasszikus]**.</span><span class="sxs-lookup"><span data-stu-id="f9652-145">At hello bottom of hello experiment canvas, click **Set Up Web Service** and select **Deploy Web Service [Classic]**.</span></span> <span data-ttu-id="f9652-146">hello webszolgáltatás **irányítópult** jelenik meg, amely az API-kulcs és hello API hello súgólap a kötegelt végrehajtás.</span><span class="sxs-lookup"><span data-stu-id="f9652-146">hello Web Service **Dashboard** is displayed with hello API Key and hello API help page for Batch Execution.</span></span> <span data-ttu-id="f9652-147">Csak a kötegelt végrehajtási módszer hello betanított modellek létrehozásához használható.</span><span class="sxs-lookup"><span data-stu-id="f9652-147">Only hello Batch Execution method can be used for creating Trained Models.</span></span>

<span data-ttu-id="f9652-148">**Új webszolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="f9652-148">**New web service**</span></span>

<span data-ttu-id="f9652-149">A kísérletvászonra hello hello alján kattintson **webes szolgáltatások beállítása** válassza **[Új] webes szolgáltatás telepítése**.</span><span class="sxs-lookup"><span data-stu-id="f9652-149">At hello bottom of hello experiment canvas, click **Set Up Web Service** and select **Deploy Web Service [New]**.</span></span> <span data-ttu-id="f9652-150">hello webes szolgáltatás Azure Machine Learning webszolgáltatások portál megnyitja toohello telepítés webszolgáltatás oldalát.</span><span class="sxs-lookup"><span data-stu-id="f9652-150">hello Web Service Azure Machine Learning Web Services portal opens toohello Deploy web service page.</span></span> <span data-ttu-id="f9652-151">Adjon meg egy nevet a webszolgáltatáshoz és támogatási csomag kiválasztása, majd kattintson **telepítés**.</span><span class="sxs-lookup"><span data-stu-id="f9652-151">Type a name for your web service and choose a payment plan, then click **Deploy**.</span></span> <span data-ttu-id="f9652-152">Csak hello kötegelt végrehajtási módszer használható betanított modellek létrehozása</span><span class="sxs-lookup"><span data-stu-id="f9652-152">Only hello Batch Execution method can be used for creating Trained Models</span></span>

<span data-ttu-id="f9652-153">Mindkét esetben kísérlet tartalmaz befejezett futtatását, miután hello eredményül kapott munkafolyamat kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="f9652-153">In either case, after experiment has completed running, hello resulting workflow should look as follows:</span></span>

![Eredményül kapott munkafolyamat futtatása után.][4]



## <a name="retrain-hello-model-with-new-data-using-bes"></a><span data-ttu-id="f9652-155">Hello modell újratanítása BES használatával új adatokkal</span><span class="sxs-lookup"><span data-stu-id="f9652-155">Retrain hello model with new data using BES</span></span>
<span data-ttu-id="f9652-156">Ebben a példában a C# toocreate hello átképezési alkalmazás használ.</span><span class="sxs-lookup"><span data-stu-id="f9652-156">For this example, you are using C# toocreate hello retraining application.</span></span> <span data-ttu-id="f9652-157">Is használhatja hello Python vagy R minta kód tooaccomplish ezt a feladatot.</span><span class="sxs-lookup"><span data-stu-id="f9652-157">You can also use hello Python or R sample code tooaccomplish this task.</span></span>

<span data-ttu-id="f9652-158">toocall hello Átképezési API-kat:</span><span class="sxs-lookup"><span data-stu-id="f9652-158">toocall hello Retraining APIs:</span></span>

1. <span data-ttu-id="f9652-159">Hozzon létre egy C# konzolalkalmazást a Visual Studio: **új** > **projekt** > **Visual C#** > **klasszikus Windows asztal** > **Konzolalkalmazás (.NET-keretrendszer)**.</span><span class="sxs-lookup"><span data-stu-id="f9652-159">Create a C# console application in Visual Studio: **New** > **Project** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.</span></span>
2. <span data-ttu-id="f9652-160">Jelentkezzen be toohello a Machine Learning webszolgáltatás portálon.</span><span class="sxs-lookup"><span data-stu-id="f9652-160">Sign in toohello Machine Learning Web Service portal.</span></span>
3. <span data-ttu-id="f9652-161">Ha egy klasszikus webszolgáltatás dolgozik, kattintson a **klasszikus webszolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="f9652-161">If you are working with a Classic web service, click **Classic Web Services**.</span></span>
   1. <span data-ttu-id="f9652-162">Kattintson a hello webszolgáltatás dolgozik.</span><span class="sxs-lookup"><span data-stu-id="f9652-162">Click hello web service you are working with.</span></span>
   2. <span data-ttu-id="f9652-163">Kattintson a hello alapértelmezett végpont.</span><span class="sxs-lookup"><span data-stu-id="f9652-163">Click hello default endpoint.</span></span>
   3. <span data-ttu-id="f9652-164">Kattintson a **felhasználásához**.</span><span class="sxs-lookup"><span data-stu-id="f9652-164">Click **Consume**.</span></span>
   4. <span data-ttu-id="f9652-165">Hello hello alján **felhasználás** lap hello **mintakód** kattintson **kötegelt**.</span><span class="sxs-lookup"><span data-stu-id="f9652-165">At hello bottom of hello **Consume** page, in hello **Sample Code** section, click **Batch**.</span></span>
   5. <span data-ttu-id="f9652-166">Az eljárás 5 toostep továbbra is.</span><span class="sxs-lookup"><span data-stu-id="f9652-166">Continue toostep 5 of this procedure.</span></span>
4. <span data-ttu-id="f9652-167">Ha egy új webszolgáltatás-bővítmény dolgozik, kattintson a **webszolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="f9652-167">If you are working with a New web service, click **Web Services**.</span></span>
   1. <span data-ttu-id="f9652-168">Kattintson a hello webszolgáltatás dolgozik.</span><span class="sxs-lookup"><span data-stu-id="f9652-168">Click hello web service you are working with.</span></span>
   2. <span data-ttu-id="f9652-169">Kattintson a **felhasználásához**.</span><span class="sxs-lookup"><span data-stu-id="f9652-169">Click **Consume**.</span></span>
   3. <span data-ttu-id="f9652-170">Hello felhasználás lap hello hello alján **mintakód** kattintson **kötegelt**.</span><span class="sxs-lookup"><span data-stu-id="f9652-170">At hello bottom of hello Consume page, in hello **Sample Code** section, click **Batch**.</span></span>
5. <span data-ttu-id="f9652-171">Hello minta C#-kódban a kötegelt végrehajtás másolása és beillesztése hello Program.cs fájlhoz a meggyőződött arról, hogy hello névtér nem módosulnak.</span><span class="sxs-lookup"><span data-stu-id="f9652-171">Copy hello sample C# code for batch execution and paste it into hello Program.cs file, making sure hello namespace remains intact.</span></span>

<span data-ttu-id="f9652-172">Adja hozzá a hello Microsoft.AspNet.WebApi.Client Nuget-csomagot a hello megjegyzések.</span><span class="sxs-lookup"><span data-stu-id="f9652-172">Add hello Nuget package Microsoft.AspNet.WebApi.Client as specified in hello comments.</span></span> <span data-ttu-id="f9652-173">tooadd hello hivatkozás tooMicrosoft.WindowsAzure.Storage.dll, először meg kell tooinstall hello ügyféloldali kódtár a Microsoft Azure tárolási szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="f9652-173">tooadd hello reference tooMicrosoft.WindowsAzure.Storage.dll, you might first need tooinstall hello client library for Microsoft Azure storage services.</span></span> <span data-ttu-id="f9652-174">További információkért lásd: [Windows tárolószolgáltatások](https://www.nuget.org/packages/WindowsAzure.Storage).</span><span class="sxs-lookup"><span data-stu-id="f9652-174">For more information, see [Windows Storage Services](https://www.nuget.org/packages/WindowsAzure.Storage).</span></span>

### <a name="update-hello-apikey-declaration"></a><span data-ttu-id="f9652-175">Hello apikey nyilatkozat frissítése</span><span class="sxs-lookup"><span data-stu-id="f9652-175">Update hello apikey declaration</span></span>
<span data-ttu-id="f9652-176">Keresse meg a hello **apikey** nyilatkozatot.</span><span class="sxs-lookup"><span data-stu-id="f9652-176">Locate hello **apikey** declaration.</span></span>

    const string apiKey = "abc123"; // Replace this with hello API key for hello web service

<span data-ttu-id="f9652-177">A hello **alapvető fogyasztási adatai** hello szakasza **felhasználás** lapon hello elsődleges kulcs keresse meg és másolja azt toohello **apikey** nyilatkozatot.</span><span class="sxs-lookup"><span data-stu-id="f9652-177">In hello **Basic consumption info** section of hello **Consume** page, locate hello primary key and copy it toohello **apikey** declaration.</span></span>

### <a name="update-hello-azure-storage-information"></a><span data-ttu-id="f9652-178">Hello Azure Storage-adatainak módosítása</span><span class="sxs-lookup"><span data-stu-id="f9652-178">Update hello Azure Storage information</span></span>
<span data-ttu-id="f9652-179">hello BES mintakód feltölt egy fájlt egy helyi meghajtó (például "C:\temp\CensusIpnput.csv") tooAzure tárolási, folyamatokat engedélyez, és írja a hello eredmények hátsó tooAzure tárolási.</span><span class="sxs-lookup"><span data-stu-id="f9652-179">hello BES sample code uploads a file from a local drive (For example "C:\temp\CensusIpnput.csv") tooAzure Storage, processes it, and writes hello results back tooAzure Storage.</span></span>  

<span data-ttu-id="f9652-180">tooaccomplish ebben a feladatban le kell kérni a tárfiók hello klasszikus Azure portálon, és a megfelelő értékek hello kódban hello frissítse hello Tárfiók neve, a kulcs és a tároló adatait.</span><span class="sxs-lookup"><span data-stu-id="f9652-180">tooaccomplish this task, you must retrieve hello Storage account name, key, and container information for your Storage account from hello classic Azure portal and hello update corresponding values in hello code.</span></span> 

1. <span data-ttu-id="f9652-181">Jelentkezzen be a klasszikus Azure portálon toohello.</span><span class="sxs-lookup"><span data-stu-id="f9652-181">Sign in toohello classic Azure portal.</span></span>
2. <span data-ttu-id="f9652-182">Kattintson a bal oldali oszlopban hello **tárolási**.</span><span class="sxs-lookup"><span data-stu-id="f9652-182">In hello left navigation column, click **Storage**.</span></span>
3. <span data-ttu-id="f9652-183">Hello listában tárfiókok jelölje be egy toostore hello retrained modell.</span><span class="sxs-lookup"><span data-stu-id="f9652-183">From hello list of storage accounts, select one toostore hello retrained model.</span></span>
4. <span data-ttu-id="f9652-184">Hello a hello lap alján, kattintson **elérési kulcsok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="f9652-184">At hello bottom of hello page, click **Manage Access Keys**.</span></span>
5. <span data-ttu-id="f9652-185">Másolja ki és mentse a hello **elsődleges elérési kulcsot** és Bezárás hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f9652-185">Copy and save hello **Primary Access Key** and close hello dialog.</span></span> 
6. <span data-ttu-id="f9652-186">Hello hello oldal tetején, kattintson a **tárolók**.</span><span class="sxs-lookup"><span data-stu-id="f9652-186">At hello top of hello page, click **Containers**.</span></span>
7. <span data-ttu-id="f9652-187">Jelöljön ki egy meglévő tárolót, vagy hozzon létre egy újat, és mentse hello nevét.</span><span class="sxs-lookup"><span data-stu-id="f9652-187">Select an existing container or create a new one and save hello name.</span></span>

<span data-ttu-id="f9652-188">Keresse meg a hello *StorageAccountName*, *StorageAccountKey*, és *StorageContainerName* deklarációkat és mentette a hello frissítés hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="f9652-188">Locate hello *StorageAccountName*, *StorageAccountKey*, and *StorageContainerName* declarations and update hello values you saved from hello Azure portal.</span></span>

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure Storage Account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage Key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage Container name

<span data-ttu-id="f9652-189">Akkor is biztosítania kell hello bemeneti fájl elérhető hello hello kódban megadott helyen.</span><span class="sxs-lookup"><span data-stu-id="f9652-189">You also must ensure hello input file is available at hello location you specify in hello code.</span></span> 

### <a name="specify-hello-output-location"></a><span data-ttu-id="f9652-190">Hello kimeneti helyének megadása</span><span class="sxs-lookup"><span data-stu-id="f9652-190">Specify hello output location</span></span>
<span data-ttu-id="f9652-191">A kérelem hasznos hello hello kimeneti hely megadásakor hello megadott hello fájl kiterjesztése *RelativeLocation* ilearner kell megadni.</span><span class="sxs-lookup"><span data-stu-id="f9652-191">When specifying hello output location in hello Request Payload, hello extension of hello file specified in *RelativeLocation* must be specified as ilearner.</span></span> 

<span data-ttu-id="f9652-192">Tekintse meg a következő példa hello:</span><span class="sxs-lookup"><span data-stu-id="f9652-192">See hello following example:</span></span>

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with hello location you would like toouse for your output file, and valid file extension (usually .csv for scoring results, or .ilearner for trained models)*/
            }
        },

> [!NOTE]
> <span data-ttu-id="f9652-193">a kimeneti helyek hello nevének azokat, a forgatókönyv alapuló hello hello webes szolgáltatás kimeneti modulok hozzá hello eltérhet.</span><span class="sxs-lookup"><span data-stu-id="f9652-193">hello names of your output locations may be different from hello ones in this walkthrough based on hello order in which you added hello web service output modules.</span></span> <span data-ttu-id="f9652-194">A tanítási kísérletet két kimenettel állít be, mert hello eredmények tartalmazzák a Tartózkodásihely-adatok tárolási helyezni őket.</span><span class="sxs-lookup"><span data-stu-id="f9652-194">Since you set up this training experiment with two outputs, hello results include storage location information for both of them.</span></span>  
> 
> 

![Kimeneti átképezési][6]

<span data-ttu-id="f9652-196">4. ábra: A kimeneti Átképezési.</span><span class="sxs-lookup"><span data-stu-id="f9652-196">Diagram 4: Retraining output.</span></span>

## <a name="evaluate-hello-retraining-results"></a><span data-ttu-id="f9652-197">Hello Átképezési eredmények értékelése</span><span class="sxs-lookup"><span data-stu-id="f9652-197">Evaluate hello Retraining Results</span></span>
<span data-ttu-id="f9652-198">Hello alkalmazás futtatásakor hello kimeneti hello URL-címet tartalmazza, és a SAS-token szükséges tooaccess hello kiértékelésének eredménye.</span><span class="sxs-lookup"><span data-stu-id="f9652-198">When you run hello application, hello output includes hello URL and SAS token necessary tooaccess hello evaluation results.</span></span>

<span data-ttu-id="f9652-199">Megtekintheti a hello teljesítménymérési eredményeket hello retrained modell hello kombinálásával *BaseLocation*, *RelativeLocation*, és *SasBlobToken* hello kimeneti eredmények a *output2* (ahogy az az előző kimeneti kép átképezési hello szerepel), és be hello teljes URL-CÍMÉT a böngésző címsorában hello.</span><span class="sxs-lookup"><span data-stu-id="f9652-199">You can see hello performance results of hello retrained model by combining hello *BaseLocation*, *RelativeLocation*, and *SasBlobToken* from hello output results for *output2* (as shown in hello preceding retraining output image) and pasting hello complete URL in hello browser address bar.</span></span>  

<span data-ttu-id="f9652-200">Hello eredmények toodetermine akkor megvizsgálja, hogy hello újonnan betanított modell is elegendő egy meglévő tooreplace hello hajt végre.</span><span class="sxs-lookup"><span data-stu-id="f9652-200">Examine hello results toodetermine whether hello newly trained model performs well enough tooreplace hello existing one.</span></span>

<span data-ttu-id="f9652-201">Másolás hello *BaseLocation*, *RelativeLocation*, és *SasBlobToken* hello kimeneti eredmények, használhatja őket hello átképezési folyamat során.</span><span class="sxs-lookup"><span data-stu-id="f9652-201">Copy hello *BaseLocation*, *RelativeLocation*, and *SasBlobToken* from hello output results, you will use them during hello retraining process.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f9652-202">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f9652-202">Next steps</span></span>
<span data-ttu-id="f9652-203">Ha telepítette a hello prediktív webszolgáltatás kattintva **webes szolgáltatás telepítése [klasszikus]**, lásd: [egy klasszikus webszolgáltatás újratanítása](machine-learning-retrain-a-classic-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="f9652-203">If you deployed hello predictive web service by clicking **Deploy Web Service [Classic]**, see [Retrain a Classic web service](machine-learning-retrain-a-classic-web-service.md).</span></span>

<span data-ttu-id="f9652-204">Ha telepítette a hello prediktív webszolgáltatás kattintva **[Új] webes szolgáltatás telepítése**, lásd: [egy új webszolgáltatás-bővítmény hello Machine Learning Management-parancsmagok használatával újratanítása](machine-learning-retrain-new-web-service-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="f9652-204">If you deployed hello predictive web service by clicking **Deploy Web Service [New]**, see [Retrain a New web service using hello Machine Learning Management cmdlets](machine-learning-retrain-new-web-service-using-powershell.md).</span></span>

<!-- Retrain a New web service using hello Machine Learning Management REST API -->


[1]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE01.png
[2]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE02.png
[3]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE03.png
[4]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE04.png
[5]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE05.png
[6]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE06.png
[7]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE07.png


<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
