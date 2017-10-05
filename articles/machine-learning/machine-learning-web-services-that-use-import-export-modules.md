---
title: "Importálási/exportálási adatok használatának Azure Machine Learning webszolgáltatások |} Microsoft Docs"
description: "Ismerje meg, hogyan használható az adatok importálása és exportálása adatok modulok adatokat küldeni és fogadni webszolgáltatásból."
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
ms.openlocfilehash: 123c8c2b1c5bae268b2a61c185743f2c3920175e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="deploying-azure-ml-web-services-that-use-data-import-and-data-export-modules"></a><span data-ttu-id="f3299-103">Az Adatok importálása és az Adatok exportálása modult használó Azure ML webszolgáltatások üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="f3299-103">Deploying Azure ML web services that use Data Import and Data Export modules</span></span>

<span data-ttu-id="f3299-104">Amikor létrehoz egy prediktív kísérletté, általában hozzáadhat egy webszolgáltatás bemenete és a kimeneti.</span><span class="sxs-lookup"><span data-stu-id="f3299-104">When you create a predictive experiment, you typically add a web service input and output.</span></span> <span data-ttu-id="f3299-105">A kísérlet telepítésekor fogyasztók küldhet és fogadhat adatokat a webszolgáltatás a be- és kimenetekkel keresztül.</span><span class="sxs-lookup"><span data-stu-id="f3299-105">When you deploy the experiment, consumers can send and receive data from the web service through the inputs and outputs.</span></span> <span data-ttu-id="f3299-106">Egyes alkalmazások a fogyasztói adatoktól adatcsatornáról érhető el vagy már található egy külső adatforrásból, például az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="f3299-106">For some applications, a consumer's data may be available from a data feed or already reside in an external data source such as Azure Blob storage.</span></span> <span data-ttu-id="f3299-107">Ebben az esetben nincs szükségük olvasása és írása az adatokat, és a webszolgáltatás bemenetei és kimenetei.</span><span class="sxs-lookup"><span data-stu-id="f3299-107">In these cases, they do not need read and write data using web service inputs and outputs.</span></span> <span data-ttu-id="f3299-108">Ugyanakkor, ehelyett a kötegelt végrehajtási szolgáltatás (BES) segítségével adatokat olvasni az adatforrást, és adatokat importálhat-modullal és a pontozási eredményeinek írni egy másik adatok helye az adatok exportálása modullal.</span><span class="sxs-lookup"><span data-stu-id="f3299-108">They can, instead, use the Batch Execution Service (BES) to read data from the data source using an Import Data module and write the scoring results to a different data location using an Export Data module.</span></span>

<span data-ttu-id="f3299-109">Az adatok importálása és exportálása adatok modulok, olvasni és írni a különböző adatokat, például egy webes URL-Címéhez HTTP, a Hive-lekérdezések, egy Azure SQL-adatbázis, Azure Table storage, Azure Blob Storage tárolóban, a adatcsatorna révén helyét adja meg, vagy egy helyi SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="f3299-109">The Import Data and Export data modules, can read from and write to various data locations such as a Web URL via HTTP, a Hive Query, an Azure SQL database, Azure Table storage, Azure Blob storage, a Data Feed provide, or an on-premises SQL database.</span></span>

<span data-ttu-id="f3299-110">Ez a témakör használja a "minta 5: vonat, tesztelési, Evaluate bináris osztályozási: felnőtt Dataset" sample, és azt feltételezi, hogy a dataset már be van töltve nevű censusdata Azure SQL-táblába.</span><span class="sxs-lookup"><span data-stu-id="f3299-110">This topic uses the "Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset" sample and assumes the dataset has already been loaded into an Azure SQL table named censusdata.</span></span>

## <a name="create-the-training-experiment"></a><span data-ttu-id="f3299-111">A képzési kísérlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3299-111">Create the training experiment</span></span>
<span data-ttu-id="f3299-112">Amikor megnyitja a "minta 5: vonat, tesztelési, Evaluate bináris osztályozási: felnőtt Dataset" mintát használ a minta bináris osztályozási felnőtt nyilvántartásba bevétel adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="f3299-112">When you open the "Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset" sample it uses the sample Adult Census Income Binary Classification dataset.</span></span> <span data-ttu-id="f3299-113">És a kísérlet vászonra az alábbi képen hasonlóan fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="f3299-113">And the experiment in the canvas will look similar to the following image:</span></span>

![A kísérlet a kezdeti konfigurálását.](./media/machine-learning-web-services-that-use-import-export-modules/initial-look-of-experiment.png)

<span data-ttu-id="f3299-115">Az adatokat olvasni az Azure SQL táblázat:</span><span class="sxs-lookup"><span data-stu-id="f3299-115">To read the data from the Azure SQL table:</span></span>

1. <span data-ttu-id="f3299-116">Az adatkészlet-modul törlése.</span><span class="sxs-lookup"><span data-stu-id="f3299-116">Delete the dataset module.</span></span>
2. <span data-ttu-id="f3299-117">Összetevők a keresőmezőbe írja be a importálása.</span><span class="sxs-lookup"><span data-stu-id="f3299-117">In the components search box, type import.</span></span>
3. <span data-ttu-id="f3299-118">Az eredmények listájában hozzá egy *és adatokat importálhat* modult a kísérletvászonra.</span><span class="sxs-lookup"><span data-stu-id="f3299-118">From the results list, add an *Import Data* module to the experiment canvas.</span></span>
4. <span data-ttu-id="f3299-119">Csatlakozás kimenetét a *és adatokat importálhat* modul bemeneti, a *Clean Missing Data* modul.</span><span class="sxs-lookup"><span data-stu-id="f3299-119">Connect output of the *Import Data* module the input of the *Clean Missing Data* module.</span></span>
5. <span data-ttu-id="f3299-120">A Tulajdonságok panelen, jelölje ki **Azure SQL Database** a a **adatforrás** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="f3299-120">In properties pane, select **Azure SQL Database** in the **Data Source** dropdown.</span></span>
6. <span data-ttu-id="f3299-121">Az a **adatbázis-kiszolgáló neve**, **adatbázisnév**, **felhasználónév**, és **jelszó** mezőkben adja meg a megfelelő adatokat a az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="f3299-121">In the **Database server name**, **Database name**, **User name**, and **Password** fields, enter the appropriate information for your database.</span></span>
7. <span data-ttu-id="f3299-122">Az adatbázis lekérdezés mezőben adja meg a következő lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="f3299-122">In the Database query field, enter the following query.</span></span>
   
     <span data-ttu-id="f3299-123">Válassza ki a [kora]</span><span class="sxs-lookup"><span data-stu-id="f3299-123">select [age],</span></span>
   
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
     <span data-ttu-id="f3299-124">a dbo.censusdata;</span><span class="sxs-lookup"><span data-stu-id="f3299-124">from dbo.censusdata;</span></span>
8. <span data-ttu-id="f3299-125">Kattintson a kísérletvászon alján **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="f3299-125">At the bottom of the experiment canvas, click **Run**.</span></span>

## <a name="create-the-predictive-experiment"></a><span data-ttu-id="f3299-126">A prediktív kísérlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3299-126">Create the predictive experiment</span></span>
<span data-ttu-id="f3299-127">Ezután a prediktív kísérletté, ahol a webszolgáltatás telepítése beállítása.</span><span class="sxs-lookup"><span data-stu-id="f3299-127">Next you set up the predictive experiment from which you deploy your web service.</span></span>

1. <span data-ttu-id="f3299-128">A kísérlet vászon alján kattintson **webes szolgáltatások beállítása** válassza **prediktív webszolgáltatás [ajánlott]**.</span><span class="sxs-lookup"><span data-stu-id="f3299-128">At the bottom of the experiment canvas, click **Set Up Web Service** and select **Predictive Web Service [Recommended]**.</span></span>
2. <span data-ttu-id="f3299-129">Távolítsa el a *webszolgáltatás bemenetét* és *webes szolgáltatás kimeneti modulok* a prediktív kísérlet.</span><span class="sxs-lookup"><span data-stu-id="f3299-129">Remove the *Web Service Input* and *Web Service Output modules* from the predictive experiment.</span></span> 
3. <span data-ttu-id="f3299-130">Összetevők keresőmezőbe írja be az exportálás.</span><span class="sxs-lookup"><span data-stu-id="f3299-130">In the components search box, type export.</span></span>
4. <span data-ttu-id="f3299-131">Az eredmények listájában hozzá egy *adatok exportálása* modult a kísérletvászonra.</span><span class="sxs-lookup"><span data-stu-id="f3299-131">From the results list, add an *Export Data* module to the experiment canvas.</span></span>
5. <span data-ttu-id="f3299-132">Csatlakozás kimenetét a *Score Model* modul bemeneti, a *adatok exportálása* modul.</span><span class="sxs-lookup"><span data-stu-id="f3299-132">Connect output of the *Score Model* module the input of the *Export Data* module.</span></span> 
6. <span data-ttu-id="f3299-133">A Tulajdonságok panelen, jelölje ki **Azure SQL Database** meg az adatok cél legördülő listában.</span><span class="sxs-lookup"><span data-stu-id="f3299-133">In properties pane, select **Azure SQL Database** in the data destination dropdown.</span></span>
7. <span data-ttu-id="f3299-134">Az a **adatbázis-kiszolgáló neve**, **adatbázisnév**, **kiszolgáló felhasználói fiók nevét**, és **kiszolgáló felhasználói fiók jelszavát** mezőkben adja meg a az adatbázis megfelelő adatokat.</span><span class="sxs-lookup"><span data-stu-id="f3299-134">In the **Database server name**, **Database name**, **Server user account name**, and **Server user account password** fields, enter the appropriate information for your database.</span></span>
8. <span data-ttu-id="f3299-135">Az a **vesszővel tagolt listája menteni oszlopok** mezőbe írja be a pontozott címkék.</span><span class="sxs-lookup"><span data-stu-id="f3299-135">In the **Comma separated list of columns to be saved** field, type Scored Labels.</span></span>
9. <span data-ttu-id="f3299-136">Az a **tábla neve adatmező**, írja be a dbo. ScoredLabels.</span><span class="sxs-lookup"><span data-stu-id="f3299-136">In the **Data table name field**, type dbo.ScoredLabels.</span></span> <span data-ttu-id="f3299-137">Ha a tábla nem létezik, létrejön a kísérlet fut, vagy a webes szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="f3299-137">If the table does not exist, it is created when the experiment is run or the web service is called.</span></span>
10. <span data-ttu-id="f3299-138">Az a **vesszővel tagolt listája datatable oszlopok** mezőbe írja be a ScoredLabels.</span><span class="sxs-lookup"><span data-stu-id="f3299-138">In the **Comma separated list of datatable columns** field, type ScoredLabels.</span></span>

<span data-ttu-id="f3299-139">Ha egy alkalmazás, amely behívja a végső webszolgáltatás ír, érdemes lehet adjon meg egy másik bemeneti lekérdezés vagy a célként megadott táblája futási időben.</span><span class="sxs-lookup"><span data-stu-id="f3299-139">When you write an application that calls the final web service, you may want to specify a different input query or destination table at run time.</span></span> <span data-ttu-id="f3299-140">Konfigurálja ezeket bemenetekhez és kimenetekhez, a webszolgáltatás-paramétereket szolgáltatás használatával állítsa be a *és adatokat importálhat* modul *adatforrás* tulajdonság és a *adatok exportálása* mód adatait cél tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="f3299-140">To configure these inputs and outputs, use the Web Service Parameters feature to set the *Import Data* module *Data source* property and the *Export Data* mode data destination property.</span></span>  <span data-ttu-id="f3299-141">A webszolgáltatás-paramétereket további információkért lásd: a [AzureML webszolgáltatás-paramétereket bejegyzés](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) a Cortana Intelligence és a Machine Learning Blog.</span><span class="sxs-lookup"><span data-stu-id="f3299-141">For more information on Web Service Parameters, see the [AzureML Web Service Parameters entry](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) on the Cortana Intelligence and Machine Learning Blog.</span></span>

<span data-ttu-id="f3299-142">A webszolgáltatás-paramétereket az importálási és a célként megadott táblája konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="f3299-142">To configure the Web Service Parameters for the import query and the destination table:</span></span>

1. <span data-ttu-id="f3299-143">A Tulajdonságok panelen az a *és adatokat importálhat* modult, kattintson az ikonra a bal felső sarkában a **adatbázis-lekérdezés** mezőben, majd válassza ki **webszolgáltatási paraméter beállítása**.</span><span class="sxs-lookup"><span data-stu-id="f3299-143">In the properties pane for the *Import Data* module, click the icon at the top right of the **Database query** field and select **Set as web service parameter**.</span></span>
2. <span data-ttu-id="f3299-144">A Tulajdonságok panelen az a *adatok exportálása* modult, kattintson az ikonra a bal felső sarkában a **adatok táblanév** mezőben, majd válassza ki **webszolgáltatási paraméter beállítása**.</span><span class="sxs-lookup"><span data-stu-id="f3299-144">In the properties pane for the *Export Data* module, click the icon at the top right of the **Data table name** field and select **Set as web service parameter**.</span></span>
3. <span data-ttu-id="f3299-145">Alján a *adatok exportálása* modul tulajdonságok panelen, a a **webszolgáltatás-paramétereket** szakaszt, kattintson az adatbázis-lekérdezés és lekérdezés nevezni.</span><span class="sxs-lookup"><span data-stu-id="f3299-145">At the bottom of the *Export Data* module properties pane, in the **Web Service Parameters** section, click Database query and rename it Query.</span></span>
4. <span data-ttu-id="f3299-146">Kattintson a **adatok táblanév** és adjon neki **tábla**.</span><span class="sxs-lookup"><span data-stu-id="f3299-146">Click **Data table name** and rename it **Table**.</span></span>

<span data-ttu-id="f3299-147">Amikor elkészült, a kísérlet a következő kép hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="f3299-147">When you are done, your experiment should look similar to the following image:</span></span>

![Kísérlet végső megjelenését.](./media/machine-learning-web-services-that-use-import-export-modules/experiment-with-import-data-added.png)

<span data-ttu-id="f3299-149">Most már telepítheti a kísérlet webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="f3299-149">Now you can deploy the experiment as a web service.</span></span>

## <a name="deploy-the-web-service"></a><span data-ttu-id="f3299-150">A webszolgáltatás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="f3299-150">Deploy the web service</span></span>
<span data-ttu-id="f3299-151">A klasszikus vagy az új webszolgáltatás telepítése.</span><span class="sxs-lookup"><span data-stu-id="f3299-151">You can deploy to either a Classic or New web service.</span></span>

### <a name="deploy-a-classic-web-service"></a><span data-ttu-id="f3299-152">Klasszikus webes szolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="f3299-152">Deploy a Classic Web Service</span></span>
<span data-ttu-id="f3299-153">Klasszikus webszolgáltatásként üzembe helyezését, és felhasználását, az alkalmazás létrehozása:</span><span class="sxs-lookup"><span data-stu-id="f3299-153">To deploy as a Classic Web Service and create an application to consume it:</span></span>

1. <span data-ttu-id="f3299-154">A kísérlet vászon alján kattintson a Futtatás gombra.</span><span class="sxs-lookup"><span data-stu-id="f3299-154">At the bottom of the experiment canvas, click Run.</span></span>
2. <span data-ttu-id="f3299-155">Kattintson a Futtatás befejeződésekor **webes szolgáltatás telepítése** válassza **webes szolgáltatás telepítése [klasszikus]**.</span><span class="sxs-lookup"><span data-stu-id="f3299-155">When the run has completed, click **Deploy Web Service** and select **Deploy Web Service [Classic]**.</span></span>
3. <span data-ttu-id="f3299-156">A webes szolgáltatás irányítópultján keresse meg az API-kulcs.</span><span class="sxs-lookup"><span data-stu-id="f3299-156">On the web service dashboard, locate your API key.</span></span> <span data-ttu-id="f3299-157">Másolja ki és mentse későbbi használat céljából.</span><span class="sxs-lookup"><span data-stu-id="f3299-157">Copy and save it to use later.</span></span>
4. <span data-ttu-id="f3299-158">Az a **alapértelmezett végpont** table, kattintson a **kötegelt végrehajtási** nyissa meg az API-Súgóoldalát mutató hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="f3299-158">In the **Default Endpoint** table, click the **Batch Execution** link to open the API Help Page.</span></span>
5. <span data-ttu-id="f3299-159">A Visual Studio, hozzon létre egy C# konzolalkalmazást: **új** > **projekt** > **Visual C#** > **Windows Klasszikus asztal** > **Konzolalkalmazásból (.NET-keretrendszer)**.</span><span class="sxs-lookup"><span data-stu-id="f3299-159">In Visual Studio, create a C# console application: **New** > **Project** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.</span></span>
6. <span data-ttu-id="f3299-160">Az API segítségével lapon található a **mintakód** szakasz az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="f3299-160">On the API Help Page, find the **Sample Code** section at the bottom of the page.</span></span>
7. <span data-ttu-id="f3299-161">Másolja és illessze be a C# mintakód a Program.cs fájlba, és távolítson el minden hivatkozást a blob Storage.</span><span class="sxs-lookup"><span data-stu-id="f3299-161">Copy and paste the C# sample code into your Program.cs file, and remove all references to the blob storage.</span></span>
8. <span data-ttu-id="f3299-162">Frissítse az értéket a *apiKey* változó a korábban mentett API-kulccsal.</span><span class="sxs-lookup"><span data-stu-id="f3299-162">Update the value of the *apiKey* variable with the API key saved earlier.</span></span>
9. <span data-ttu-id="f3299-163">Keresse meg a kérelem nyilatkozat, és módosítsa a átadott webszolgáltatás-paramétereket a *és adatokat importálhat* és *adatok exportálása* modulok.</span><span class="sxs-lookup"><span data-stu-id="f3299-163">Locate the request declaration and update the values of Web Service Parameters that are passed to the *Import Data* and *Export Data* modules.</span></span> <span data-ttu-id="f3299-164">Ebben az esetben akkor az eredeti lekérdezéssel, de egy új táblázat nevének meghatározása.</span><span class="sxs-lookup"><span data-stu-id="f3299-164">In this case, you use the original query, but define a new table name.</span></span>
   
        var request = new BatchExecutionRequest() 
        {           
            GlobalParameters = new Dictionary<string, string>() {
                { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable2" },
            }
        };
10. <span data-ttu-id="f3299-165">Futtassa az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f3299-165">Run the application.</span></span> 

<span data-ttu-id="f3299-166">A Futtatás befejezésekor új tábla hozzáadódik a pontozási eredményeinek tartalmazó adatbázis.</span><span class="sxs-lookup"><span data-stu-id="f3299-166">On completion of the run, a new table is added to the database containing the scoring results.</span></span>

### <a name="deploy-a-new-web-service"></a><span data-ttu-id="f3299-167">Egy új webszolgáltatás-bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="f3299-167">Deploy a New Web Service</span></span>

> [!NOTE] 
> <span data-ttu-id="f3299-168">Egy új webszolgáltatás-bővítmény telepítése, megfelelő engedélyekkel kell rendelkeznie, amelyhez az előfizetést, a webszolgáltatás telepítése.</span><span class="sxs-lookup"><span data-stu-id="f3299-168">To deploy a New web service you must have sufficient permissions in the subscription to which you deploying the web service.</span></span> <span data-ttu-id="f3299-169">További információkért lásd: [kezelése az Azure Machine Learning webszolgáltatások portál használatával egy webszolgáltatás-bővítmény](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="f3299-169">For more information, see [Manage a Web service using the Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="f3299-170">Telepítsen egy új webszolgáltatás-bővítmény, és felhasználását, az alkalmazás létrehozása:</span><span class="sxs-lookup"><span data-stu-id="f3299-170">To deploy as a New Web Service and create an application to consume it:</span></span>

1. <span data-ttu-id="f3299-171">Kattintson a kísérletvászon alján **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="f3299-171">At the bottom of the experiment canvas, click **Run**.</span></span>
2. <span data-ttu-id="f3299-172">Kattintson a Futtatás befejeződésekor **webes szolgáltatás telepítése** válassza **[Új] webes szolgáltatás telepítése**.</span><span class="sxs-lookup"><span data-stu-id="f3299-172">When the run has completed, click **Deploy Web Service** and select **Deploy Web Service [New]**.</span></span>
3. <span data-ttu-id="f3299-173">Telepítése kísérlet lapon adjon meg egy nevet, a webes szolgáltatás, és válassza ki a tarifacsomagot, majd kattintson **telepítés**.</span><span class="sxs-lookup"><span data-stu-id="f3299-173">On the Deploy Experiment page, enter a name for your web service, and select a pricing plan, then click **Deploy**.</span></span>
4. <span data-ttu-id="f3299-174">Az a **gyors üzembe helyezés** kattintson **felhasználás**.</span><span class="sxs-lookup"><span data-stu-id="f3299-174">On the **Quickstart** page, click **Consume**.</span></span>
5. <span data-ttu-id="f3299-175">Az a **mintakód** kattintson **kötegelt**.</span><span class="sxs-lookup"><span data-stu-id="f3299-175">In the **Sample Code** section, click **Batch**.</span></span>
6. <span data-ttu-id="f3299-176">A Visual Studio, hozzon létre egy C# konzolalkalmazást: **új** > **projekt** > **Visual C#** > **Windows Klasszikus asztal** > **Konzolalkalmazásból (.NET-keretrendszer)**.</span><span class="sxs-lookup"><span data-stu-id="f3299-176">In Visual Studio, create a C# console application: **New** > **Project** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.</span></span>
7. <span data-ttu-id="f3299-177">Másolja és illessze be a C# mintakód a Program.cs fájl.</span><span class="sxs-lookup"><span data-stu-id="f3299-177">Copy and paste the C# sample code into your Program.cs file.</span></span>
8. <span data-ttu-id="f3299-178">Frissítse az értéket a *apiKey* változó, a **elsődleges kulcs** található, a **alapvető fogyasztási adatai** szakasz.</span><span class="sxs-lookup"><span data-stu-id="f3299-178">Update the value of the *apiKey* variable with the **Primary Key** located in the **Basic consumption info** section.</span></span>
9. <span data-ttu-id="f3299-179">Keresse meg a *scoreRequest* nyilatkozat és módosítsa a átadott webszolgáltatás-paramétereket a *és adatokat importálhat* és *adatok exportálása* modulok.</span><span class="sxs-lookup"><span data-stu-id="f3299-179">Locate the *scoreRequest* declaration and update the values of Web Service Parameters that are passed to the *Import Data* and *Export Data* modules.</span></span> <span data-ttu-id="f3299-180">Ebben az esetben akkor az eredeti lekérdezéssel, de egy új táblázat nevének meghatározása.</span><span class="sxs-lookup"><span data-stu-id="f3299-180">In this case, you use the original query, but define a new table name.</span></span>
   
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
10. <span data-ttu-id="f3299-181">Futtassa az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f3299-181">Run the application.</span></span> 

