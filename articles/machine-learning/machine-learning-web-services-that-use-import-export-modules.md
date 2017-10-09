---
title: "az Azure Machine Learning webszolgáltatások Import/Export adatok aaaUsing |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse adatok importálása és exportálása adatok modulok toosend hello és egy webszolgáltatás-bővítmény fogadhat adatokat."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondlaghaeian
editor: 
ms.assetid: 3a7ac351-ebd3-43a1-8c5d-18223903d08e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.author: v-donglo
ms.openlocfilehash: 176380259b15cb338ede61c7f28ba2296b35dd52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploying-azure-ml-web-services-that-use-data-import-and-data-export-modules"></a><span data-ttu-id="ea17f-103">Az Adatok importálása és az Adatok exportálása modult használó Azure ML webszolgáltatások üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="ea17f-103">Deploying Azure ML web services that use Data Import and Data Export modules</span></span>

<span data-ttu-id="ea17f-104">Amikor létrehoz egy prediktív kísérletté, általában hozzáadhat egy webszolgáltatás bemenete és a kimeneti.</span><span class="sxs-lookup"><span data-stu-id="ea17f-104">When you create a predictive experiment, you typically add a web service input and output.</span></span> <span data-ttu-id="ea17f-105">Hello kísérlet telepítésekor fogyasztók küldhet és fogadhat adatokat hello webszolgáltatás hello bemenetekhez és kimenetekhez keresztül.</span><span class="sxs-lookup"><span data-stu-id="ea17f-105">When you deploy hello experiment, consumers can send and receive data from hello web service through hello inputs and outputs.</span></span> <span data-ttu-id="ea17f-106">Egyes alkalmazások a fogyasztói adatoktól adatcsatornáról érhető el vagy már található egy külső adatforrásból, például az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="ea17f-106">For some applications, a consumer's data may be available from a data feed or already reside in an external data source such as Azure Blob storage.</span></span> <span data-ttu-id="ea17f-107">Ebben az esetben nincs szükségük olvasása és írása az adatokat, és a webszolgáltatás bemenetei és kimenetei.</span><span class="sxs-lookup"><span data-stu-id="ea17f-107">In these cases, they do not need read and write data using web service inputs and outputs.</span></span> <span data-ttu-id="ea17f-108">Ugyanakkor, ehelyett hello kötegelt végrehajtási szolgáltatás (BES) tooread adatokat használja az adatok importálása modullal hello adatforrásból és pontozási eredményei tooa különböző adatok helye az adatok exportálása modullal hello írni.</span><span class="sxs-lookup"><span data-stu-id="ea17f-108">They can, instead, use hello Batch Execution Service (BES) tooread data from hello data source using an Import Data module and write hello scoring results tooa different data location using an Export Data module.</span></span>

<span data-ttu-id="ea17f-109">hello adatok importálása és exportálása adatok modulok, lehet olvasni és toovarious az adatok írása például egy webes URL-Címéhez HTTP, a Hive-lekérdezések, egy Azure SQL-adatbázis, Azure Table storage, Azure Blob Storage tárolóban, a adatcsatorna révén helyét adja meg, vagy egy helyi SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="ea17f-109">hello Import Data and Export data modules, can read from and write toovarious data locations such as a Web URL via HTTP, a Hive Query, an Azure SQL database, Azure Table storage, Azure Blob storage, a Data Feed provide, or an on-premises SQL database.</span></span>

<span data-ttu-id="ea17f-110">Ez a témakör használ hello "minta 5: vonat, tesztelési, Evaluate bináris osztályozási: felnőtt Dataset" sample, és azt feltételezi, hogy hello dataset már be van töltve nevű censusdata Azure SQL-táblába.</span><span class="sxs-lookup"><span data-stu-id="ea17f-110">This topic uses hello "Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset" sample and assumes hello dataset has already been loaded into an Azure SQL table named censusdata.</span></span>

## <a name="create-hello-training-experiment"></a><span data-ttu-id="ea17f-111">Hello képzési kísérlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="ea17f-111">Create hello training experiment</span></span>
<span data-ttu-id="ea17f-112">Hello megnyitásakor "minta 5: vonat, tesztelési, Evaluate bináris osztályozási: felnőtt Dataset" mintát azt használja hello minta bináris osztályozási felnőtt nyilvántartásba bevétel adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="ea17f-112">When you open hello "Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset" sample it uses hello sample Adult Census Income Binary Classification dataset.</span></span> <span data-ttu-id="ea17f-113">És hello vásznon a hello kísérlet a következő kép hasonló toohello fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="ea17f-113">And hello experiment in hello canvas will look similar toohello following image:</span></span>

![Kezdeti konfigurációs hello kísérlet.](./media/machine-learning-web-services-that-use-import-export-modules/initial-look-of-experiment.png)

<span data-ttu-id="ea17f-115">hello Azure SQL-tábla tooread hello adatait:</span><span class="sxs-lookup"><span data-stu-id="ea17f-115">tooread hello data from hello Azure SQL table:</span></span>

1. <span data-ttu-id="ea17f-116">Hello dataset modul törlése.</span><span class="sxs-lookup"><span data-stu-id="ea17f-116">Delete hello dataset module.</span></span>
2. <span data-ttu-id="ea17f-117">Hello összetevők keresési mezőbe írja be az importálási.</span><span class="sxs-lookup"><span data-stu-id="ea17f-117">In hello components search box, type import.</span></span>
3. <span data-ttu-id="ea17f-118">Hello eredménylistában hozzá egy *és adatokat importálhat* modul toohello kísérlet vászonra.</span><span class="sxs-lookup"><span data-stu-id="ea17f-118">From hello results list, add an *Import Data* module toohello experiment canvas.</span></span>
4. <span data-ttu-id="ea17f-119">Csatlakozás hello kimenete *és adatokat importálhat* modul hello bemeneti hello *Clean Missing Data* modul.</span><span class="sxs-lookup"><span data-stu-id="ea17f-119">Connect output of hello *Import Data* module hello input of hello *Clean Missing Data* module.</span></span>
5. <span data-ttu-id="ea17f-120">A Tulajdonságok panelen, jelölje ki **Azure SQL Database** a hello **adatforrás** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="ea17f-120">In properties pane, select **Azure SQL Database** in hello **Data Source** dropdown.</span></span>
6. <span data-ttu-id="ea17f-121">A hello **adatbázis-kiszolgáló neve**, **adatbázisnév**, **felhasználónév**, és **jelszó** mezők, adja meg a megfelelő információkat hello az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="ea17f-121">In hello **Database server name**, **Database name**, **User name**, and **Password** fields, enter hello appropriate information for your database.</span></span>
7. <span data-ttu-id="ea17f-122">Hello adatbázis-lekérdezés mezőjébe írja be a következő lekérdezés hello.</span><span class="sxs-lookup"><span data-stu-id="ea17f-122">In hello Database query field, enter hello following query.</span></span>
   
     <span data-ttu-id="ea17f-123">Válassza ki a [kora]</span><span class="sxs-lookup"><span data-stu-id="ea17f-123">select [age],</span></span>
   
        [workclass],
        [fnlwgt],
        [education],
        [education-num],
        [marital-status],
        [occupation],
        [relationship],
        [race],
        [sex],
        [capital-gain],
        [capital-loss],
        [hours-per-week],
        [native-country],
        [income]
     <span data-ttu-id="ea17f-124">a dbo.censusdata;</span><span class="sxs-lookup"><span data-stu-id="ea17f-124">from dbo.censusdata;</span></span>
8. <span data-ttu-id="ea17f-125">A kísérletvászonra hello hello alján kattintson **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="ea17f-125">At hello bottom of hello experiment canvas, click **Run**.</span></span>

## <a name="create-hello-predictive-experiment"></a><span data-ttu-id="ea17f-126">Hello prediktív kísérlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="ea17f-126">Create hello predictive experiment</span></span>
<span data-ttu-id="ea17f-127">Ezután hello prediktív kísérletté, ahol a webszolgáltatás telepítése beállítása.</span><span class="sxs-lookup"><span data-stu-id="ea17f-127">Next you set up hello predictive experiment from which you deploy your web service.</span></span>

1. <span data-ttu-id="ea17f-128">A kísérletvászonra hello hello alján kattintson **webes szolgáltatások beállítása** válassza **prediktív webszolgáltatás [ajánlott]**.</span><span class="sxs-lookup"><span data-stu-id="ea17f-128">At hello bottom of hello experiment canvas, click **Set Up Web Service** and select **Predictive Web Service [Recommended]**.</span></span>
2. <span data-ttu-id="ea17f-129">Távolítsa el a hello *webszolgáltatás bemenetét* és *webes szolgáltatás kimeneti modulok* hello prediktív kísérlet.</span><span class="sxs-lookup"><span data-stu-id="ea17f-129">Remove hello *Web Service Input* and *Web Service Output modules* from hello predictive experiment.</span></span> 
3. <span data-ttu-id="ea17f-130">Hello összetevők keresési mezőbe írja be az exportálás.</span><span class="sxs-lookup"><span data-stu-id="ea17f-130">In hello components search box, type export.</span></span>
4. <span data-ttu-id="ea17f-131">Hello eredménylistában hozzá egy *adatok exportálása* modul toohello kísérlet vászonra.</span><span class="sxs-lookup"><span data-stu-id="ea17f-131">From hello results list, add an *Export Data* module toohello experiment canvas.</span></span>
5. <span data-ttu-id="ea17f-132">Csatlakozás hello kimenete *Score Model* modul hello bemeneti hello *adatok exportálása* modul.</span><span class="sxs-lookup"><span data-stu-id="ea17f-132">Connect output of hello *Score Model* module hello input of hello *Export Data* module.</span></span> 
6. <span data-ttu-id="ea17f-133">A Tulajdonságok panelen, jelölje ki **Azure SQL Database** hello adatok cél legördülő.</span><span class="sxs-lookup"><span data-stu-id="ea17f-133">In properties pane, select **Azure SQL Database** in hello data destination dropdown.</span></span>
7. <span data-ttu-id="ea17f-134">A hello **adatbázis-kiszolgáló neve**, **adatbázisnév**, **kiszolgáló felhasználói fiók nevét**, és **kiszolgáló felhasználói fiók jelszavát** mezőkben adja meg az adatbázis hello megfelelő adatokat.</span><span class="sxs-lookup"><span data-stu-id="ea17f-134">In hello **Database server name**, **Database name**, **Server user account name**, and **Server user account password** fields, enter hello appropriate information for your database.</span></span>
8. <span data-ttu-id="ea17f-135">A hello **vesszővel tagolt listája mentett oszlopok toobe** mezőbe írja be a pontozott címkék.</span><span class="sxs-lookup"><span data-stu-id="ea17f-135">In hello **Comma separated list of columns toobe saved** field, type Scored Labels.</span></span>
9. <span data-ttu-id="ea17f-136">A hello **tábla neve adatmező**, írja be a dbo. ScoredLabels.</span><span class="sxs-lookup"><span data-stu-id="ea17f-136">In hello **Data table name field**, type dbo.ScoredLabels.</span></span> <span data-ttu-id="ea17f-137">Hello tábla nem létezik, ha létrejön hello kísérlet futtatása vagy hello webszolgáltatás nevezik.</span><span class="sxs-lookup"><span data-stu-id="ea17f-137">If hello table does not exist, it is created when hello experiment is run or hello web service is called.</span></span>
10. <span data-ttu-id="ea17f-138">A hello **vesszővel tagolt listája datatable oszlopok** mezőbe írja be a ScoredLabels.</span><span class="sxs-lookup"><span data-stu-id="ea17f-138">In hello **Comma separated list of datatable columns** field, type ScoredLabels.</span></span>

<span data-ttu-id="ea17f-139">Egy alkalmazás írásakor, hogy a hívások hello végső webszolgáltatás, érdemes lehet toospecify egy másik bemeneti lekérdezés vagy a cél táblázatban futási időben.</span><span class="sxs-lookup"><span data-stu-id="ea17f-139">When you write an application that calls hello final web service, you may want toospecify a different input query or destination table at run time.</span></span> <span data-ttu-id="ea17f-140">tooconfigure ezek bemenetekhez és kimenetekhez, használja a hello webszolgáltatás-paramétereket szolgáltatás tooset hello *és adatokat importálhat* modul *adatforrás* tulajdonság és hello *adatok exportálása* mód adatok cél tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="ea17f-140">tooconfigure these inputs and outputs, use hello Web Service Parameters feature tooset hello *Import Data* module *Data source* property and hello *Export Data* mode data destination property.</span></span>  <span data-ttu-id="ea17f-141">A webszolgáltatás-paramétereket további információkért lásd: hello [AzureML webszolgáltatás-paramétereket bejegyzés](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) hello Cortana Intelligence és a Machine Learning Blog.</span><span class="sxs-lookup"><span data-stu-id="ea17f-141">For more information on Web Service Parameters, see hello [AzureML Web Service Parameters entry](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) on hello Cortana Intelligence and Machine Learning Blog.</span></span>

<span data-ttu-id="ea17f-142">hello importálási lekérdezés és hello céltábla tooconfigure hello webszolgáltatás-paramétereket:</span><span class="sxs-lookup"><span data-stu-id="ea17f-142">tooconfigure hello Web Service Parameters for hello import query and hello destination table:</span></span>

1. <span data-ttu-id="ea17f-143">Hello tulajdonságok panelen az hello *és adatokat importálhat* modul, hello hello ikonra kattintson jobb oldalán hello felső **adatbázis-lekérdezés** mezőben, majd válassza ki **webszolgáltatási paraméter beállítása**.</span><span class="sxs-lookup"><span data-stu-id="ea17f-143">In hello properties pane for hello *Import Data* module, click hello icon at hello top right of hello **Database query** field and select **Set as web service parameter**.</span></span>
2. <span data-ttu-id="ea17f-144">Hello tulajdonságok panelen az hello *adatok exportálása* modul, hello hello ikonra kattintson jobb oldalán hello felső **adatok táblanév** mezőben, majd válassza ki **webszolgáltatási paraméter beállítása**.</span><span class="sxs-lookup"><span data-stu-id="ea17f-144">In hello properties pane for hello *Export Data* module, click hello icon at hello top right of hello **Data table name** field and select **Set as web service parameter**.</span></span>
3. <span data-ttu-id="ea17f-145">Hello hello alján *adatok exportálása* modul tulajdonságok paneljén, hello **webszolgáltatás-paramétereket** szakaszt, kattintson az adatbázis-lekérdezés és lekérdezés nevezni.</span><span class="sxs-lookup"><span data-stu-id="ea17f-145">At hello bottom of hello *Export Data* module properties pane, in hello **Web Service Parameters** section, click Database query and rename it Query.</span></span>
4. <span data-ttu-id="ea17f-146">Kattintson a **adatok táblanév** és adjon neki **tábla**.</span><span class="sxs-lookup"><span data-stu-id="ea17f-146">Click **Data table name** and rename it **Table**.</span></span>

<span data-ttu-id="ea17f-147">Amikor elkészült, a kísérlet a következő kép hasonló toohello kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="ea17f-147">When you are done, your experiment should look similar toohello following image:</span></span>

![Kísérlet végső megjelenését.](./media/machine-learning-web-services-that-use-import-export-modules/experiment-with-import-data-added.png)

<span data-ttu-id="ea17f-149">Most már telepítheti hello kísérlet webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="ea17f-149">Now you can deploy hello experiment as a web service.</span></span>

## <a name="deploy-hello-web-service"></a><span data-ttu-id="ea17f-150">Hello webszolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="ea17f-150">Deploy hello web service</span></span>
<span data-ttu-id="ea17f-151">Tooeither telepíthet egy klasszikus vagy az új webszolgáltatás-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="ea17f-151">You can deploy tooeither a Classic or New web service.</span></span>

### <a name="deploy-a-classic-web-service"></a><span data-ttu-id="ea17f-152">Klasszikus webes szolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="ea17f-152">Deploy a Classic Web Service</span></span>
<span data-ttu-id="ea17f-153">Klasszikus webszolgáltatásként toodeploy, és hozzon létre egy alkalmazás tooconsume azt:</span><span class="sxs-lookup"><span data-stu-id="ea17f-153">toodeploy as a Classic Web Service and create an application tooconsume it:</span></span>

1. <span data-ttu-id="ea17f-154">Hello kísérletvászonra hello alsó részén kattintson a Futtatás gombra.</span><span class="sxs-lookup"><span data-stu-id="ea17f-154">At hello bottom of hello experiment canvas, click Run.</span></span>
2. <span data-ttu-id="ea17f-155">Futtatás hello befejezése után kattintson **webes szolgáltatás telepítése** válassza **webes szolgáltatás telepítése [klasszikus]**.</span><span class="sxs-lookup"><span data-stu-id="ea17f-155">When hello run has completed, click **Deploy Web Service** and select **Deploy Web Service [Classic]**.</span></span>
3. <span data-ttu-id="ea17f-156">Hello webes szolgáltatás irányítópultját keresse meg az API-kulcs.</span><span class="sxs-lookup"><span data-stu-id="ea17f-156">On hello web service dashboard, locate your API key.</span></span> <span data-ttu-id="ea17f-157">Másolja ki és mentse toouse később.</span><span class="sxs-lookup"><span data-stu-id="ea17f-157">Copy and save it toouse later.</span></span>
4. <span data-ttu-id="ea17f-158">A hello **alapértelmezett végpont** table, kattintson a hello **kötegelt végrehajtási** hivatkozás tooopen hello API Súgóoldalát.</span><span class="sxs-lookup"><span data-stu-id="ea17f-158">In hello **Default Endpoint** table, click hello **Batch Execution** link tooopen hello API Help Page.</span></span>
5. <span data-ttu-id="ea17f-159">A Visual Studio, hozzon létre egy C# konzolalkalmazást: **új** > **projekt** > **Visual C#** > **Windows Klasszikus asztal** > **Konzolalkalmazásból (.NET-keretrendszer)**.</span><span class="sxs-lookup"><span data-stu-id="ea17f-159">In Visual Studio, create a C# console application: **New** > **Project** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.</span></span>
6. <span data-ttu-id="ea17f-160">Hello API Súgóoldalát, megtalálhatja hello **mintakód** alján hello hello szakasz.</span><span class="sxs-lookup"><span data-stu-id="ea17f-160">On hello API Help Page, find hello **Sample Code** section at hello bottom of hello page.</span></span>
7. <span data-ttu-id="ea17f-161">Másolja és hello C# mintakód illessze be a Program.cs fájlt, és távolítsa el az összes hivatkozás toohello blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="ea17f-161">Copy and paste hello C# sample code into your Program.cs file, and remove all references toohello blob storage.</span></span>
8. <span data-ttu-id="ea17f-162">Hello hello értékét *apiKey* változó korábban mentett hello API-kulccsal.</span><span class="sxs-lookup"><span data-stu-id="ea17f-162">Update hello value of hello *apiKey* variable with hello API key saved earlier.</span></span>
9. <span data-ttu-id="ea17f-163">Keresse meg a hello kérelem nyilatkozat és frissítés hello értékei toohello átadott webszolgáltatás-paramétereket *és adatokat importálhat* és *adatok exportálása* modulok.</span><span class="sxs-lookup"><span data-stu-id="ea17f-163">Locate hello request declaration and update hello values of Web Service Parameters that are passed toohello *Import Data* and *Export Data* modules.</span></span> <span data-ttu-id="ea17f-164">Ebben az esetben akkor hello eredeti lekérdezéssel, de egy új táblázat nevének meghatározása.</span><span class="sxs-lookup"><span data-stu-id="ea17f-164">In this case, you use hello original query, but define a new table name.</span></span>
   
        var request = new BatchExecutionRequest() 
        {           
            GlobalParameters = new Dictionary<string, string>() {
                { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable2" },
            }
        };
10. <span data-ttu-id="ea17f-165">Hello alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="ea17f-165">Run hello application.</span></span> 

<span data-ttu-id="ea17f-166">Futtatás hello befejezésekor egy új hozzá pontozási eredményei hello tartalmazó toohello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="ea17f-166">On completion of hello run, a new table is added toohello database containing hello scoring results.</span></span>

### <a name="deploy-a-new-web-service"></a><span data-ttu-id="ea17f-167">Egy új webszolgáltatás-bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="ea17f-167">Deploy a New Web Service</span></span>

> [!NOTE] 
> <span data-ttu-id="ea17f-168">toodeploy egy új webszolgáltatás-bővítmény, megfelelő engedélyekkel kell rendelkeznie a hello előfizetés toowhich meg hello webes szolgáltatás telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="ea17f-168">toodeploy a New web service you must have sufficient permissions in hello subscription toowhich you deploying hello web service.</span></span> <span data-ttu-id="ea17f-169">További információkért lásd: [egy webszolgáltatás-bővítmény hello Azure Machine Learning webszolgáltatások portálon kezelheti](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="ea17f-169">For more information, see [Manage a Web service using hello Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="ea17f-170">egy új webszolgáltatás toodeploy, és hozzon létre egy alkalmazás tooconsume azt:</span><span class="sxs-lookup"><span data-stu-id="ea17f-170">toodeploy as a New Web Service and create an application tooconsume it:</span></span>

1. <span data-ttu-id="ea17f-171">A kísérletvászonra hello hello alján kattintson **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="ea17f-171">At hello bottom of hello experiment canvas, click **Run**.</span></span>
2. <span data-ttu-id="ea17f-172">Futtatás hello befejezése után kattintson **webes szolgáltatás telepítése** válassza **[Új] webes szolgáltatás telepítése**.</span><span class="sxs-lookup"><span data-stu-id="ea17f-172">When hello run has completed, click **Deploy Web Service** and select **Deploy Web Service [New]**.</span></span>
3. <span data-ttu-id="ea17f-173">Hello telepítése kísérlet lapon adja meg egy nevet a webszolgáltatás, és válassza ki a tarifacsomagot, majd kattintson **telepítés**.</span><span class="sxs-lookup"><span data-stu-id="ea17f-173">On hello Deploy Experiment page, enter a name for your web service, and select a pricing plan, then click **Deploy**.</span></span>
4. <span data-ttu-id="ea17f-174">A hello **gyors üzembe helyezés** kattintson **felhasználás**.</span><span class="sxs-lookup"><span data-stu-id="ea17f-174">On hello **Quickstart** page, click **Consume**.</span></span>
5. <span data-ttu-id="ea17f-175">A hello **mintakód** kattintson **kötegelt**.</span><span class="sxs-lookup"><span data-stu-id="ea17f-175">In hello **Sample Code** section, click **Batch**.</span></span>
6. <span data-ttu-id="ea17f-176">A Visual Studio, hozzon létre egy C# konzolalkalmazást: **új** > **projekt** > **Visual C#** > **Windows Klasszikus asztal** > **Konzolalkalmazásból (.NET-keretrendszer)**.</span><span class="sxs-lookup"><span data-stu-id="ea17f-176">In Visual Studio, create a C# console application: **New** > **Project** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.</span></span>
7. <span data-ttu-id="ea17f-177">Másolással illessze be a C# mintakód hello a Program.cs fájlba.</span><span class="sxs-lookup"><span data-stu-id="ea17f-177">Copy and paste hello C# sample code into your Program.cs file.</span></span>
8. <span data-ttu-id="ea17f-178">Hello hello értékét *apiKey* változó, hello **elsődleges kulcs** hello található **alapvető fogyasztási adatai** szakasz.</span><span class="sxs-lookup"><span data-stu-id="ea17f-178">Update hello value of hello *apiKey* variable with hello **Primary Key** located in hello **Basic consumption info** section.</span></span>
9. <span data-ttu-id="ea17f-179">Keresse meg a hello *scoreRequest* toohello átadott webszolgáltatás-paramétereket nyilatkozat és a frissítés hello értékei *és adatokat importálhat* és *adatok exportálása* modulok.</span><span class="sxs-lookup"><span data-stu-id="ea17f-179">Locate hello *scoreRequest* declaration and update hello values of Web Service Parameters that are passed toohello *Import Data* and *Export Data* modules.</span></span> <span data-ttu-id="ea17f-180">Ebben az esetben akkor hello eredeti lekérdezéssel, de egy új táblázat nevének meghatározása.</span><span class="sxs-lookup"><span data-stu-id="ea17f-180">In this case, you use hello original query, but define a new table name.</span></span>
   
        var scoreRequest = new
        {       
            Inputs = new Dictionary<string, StringTable>()
            {
            },
            GlobalParameters = new Dictionary<string, string>() {
                { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable3" },
            }
        };
10. <span data-ttu-id="ea17f-181">Hello alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="ea17f-181">Run hello application.</span></span> 

