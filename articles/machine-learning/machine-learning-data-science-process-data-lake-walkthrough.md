---
title: "Az Azure Data Lake méretezhető Adattudomány: egy végpontok közötti forgatókönyv |} Microsoft Docs"
description: "Hogyan toouse Azure Data Lake toodo adatok feltárása és bináris osztályozás a dataset feladatok."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 91a8207f-1e57-4570-b7fc-7c5fa858ffeb
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: bradsev;weig
ms.openlocfilehash: 8b05457ae7045a7aaed350a7502469f2247161e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scalable-data-science-with-azure-data-lake-an-end-to-end-walkthrough"></a><span data-ttu-id="fda28-103">Az Azure Data Lake méretezhető Adattudomány: egy végpont forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="fda28-103">Scalable Data Science with Azure Data Lake: An end-to-end Walkthrough</span></span>
<span data-ttu-id="fda28-104">Ez a bemutató ismerteti, hogyan toouse Azure Data Lake toodo adatok feltárása és a bináris osztályozási feladatok hello NYC mintában taxiköltség út és a jegy ára dataset toopredict tipp egy jegy ára fizeti lesz-e.</span><span class="sxs-lookup"><span data-stu-id="fda28-104">This walkthrough shows how toouse Azure Data Lake toodo data exploration and binary classification tasks on a sample of hello NYC taxi trip and fare dataset toopredict whether or not a tip will be paid by a fare.</span></span> <span data-ttu-id="fda28-105">Az végigvezeti hello a hello [Team adatok tudományos folyamat](http://aka.ms/datascienceprocess), végpontok, az adatok megszerzése toomodel képzési, és egy webszolgáltatás, amelyet hello modell közzéteszi toohello telepítését.</span><span class="sxs-lookup"><span data-stu-id="fda28-105">It walks you through hello steps of hello [Team Data Science Process](http://aka.ms/datascienceprocess), end-to-end, from data acquisition toomodel training, and then toohello deployment of a web service that publishes hello model.</span></span>

### <a name="azure-data-lake-analytics"></a><span data-ttu-id="fda28-106">Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="fda28-106">Azure Data Lake Analytics</span></span>
<span data-ttu-id="fda28-107">Hello [Microsoft Azure Data Lake](https://azure.microsoft.com/solutions/data-lake/) szükséges toomake összes hello képességekkel rendelkezik egyszerűen az adatok kutatók toostore adatok bármely méretét, az alakzat és a sebesség és a tooconduct adatfeldolgozás, speciális elemzés és a machine learning modellezési magas méretezhetőség költséghatékony módon.</span><span class="sxs-lookup"><span data-stu-id="fda28-107">hello [Microsoft Azure Data Lake](https://azure.microsoft.com/solutions/data-lake/) has all hello capabilities required toomake it easy for data scientists toostore data of any size, shape and speed, and tooconduct data processing, advanced analytics, and machine learning modeling with high scalability in a cost-effective way.</span></span>   <span data-ttu-id="fda28-108">Kell fizetnie a feladatonként, csak akkor, ha ténylegesen feldolgozott adatokat.</span><span class="sxs-lookup"><span data-stu-id="fda28-108">You pay on a per-job basis, only when data is actually being processed.</span></span> <span data-ttu-id="fda28-109">Az Azure Data Lake Analytics U-SQL, az, hogy színátmenet hello deklaratív természetét a C# tooprovide méretezhető hello kifejezőerejével SQL nyelvű elosztott lekérdezés funkció.</span><span class="sxs-lookup"><span data-stu-id="fda28-109">Azure Data Lake Analytics includes U-SQL, a language that blends hello declarative nature of SQL with hello expressive power of C# tooprovide scalable distributed query capability.</span></span> <span data-ttu-id="fda28-110">Tooprocess lehetővé teszi a séma alkalmazásával olvasható, a strukturálatlan adatok beszúrása egyéni logika és a felhasználó által definiált működik (UDF), és tartalmazza a bővítési tooenable finom nyomtatott szabályozhatják, hogyan tooexecute méretekben.</span><span class="sxs-lookup"><span data-stu-id="fda28-110">It enables you tooprocess unstructured data by applying schema on read, insert custom logic and user defined functions (UDFs), and includes extensibility tooenable fine grained control over how tooexecute at scale.</span></span> <span data-ttu-id="fda28-111">toolearn hello tervezési alapvetően mögött U-SQL, kapcsolatos további információkért lásd: [Visual Studio által írt blogbejegyzés](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).</span><span class="sxs-lookup"><span data-stu-id="fda28-111">toolearn more about hello design philosophy behind U-SQL, see [Visual Studio blog post](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).</span></span>

<span data-ttu-id="fda28-112">A Data Lake Analytics kulcseleme a Cortana Analytics Suite csomagnak is, emellett az Azure SQL Data Warehouse, a Power BI és a Data Factory szolgáltatásokkal is együttműködik.</span><span class="sxs-lookup"><span data-stu-id="fda28-112">Data Lake Analytics is also a key part of Cortana Analytics Suite and works with Azure SQL Data Warehouse, Power BI, and Data Factory.</span></span> <span data-ttu-id="fda28-113">Ez lehetővé teszi egy teljes felhőalapú big Data típusú adatok és a speciális elemzési platformot.</span><span class="sxs-lookup"><span data-stu-id="fda28-113">This gives you a complete cloud big data and advanced analytics platform.</span></span>

<span data-ttu-id="fda28-114">Ez a forgatókönyv megkezdése toocomplete hello szükséges feladatok Data Lake Analytics hello adatok tudományos folyamat alkotó, és milyen erőforrásokat és hello Előfeltételek ismertetésével tooinstall őket.</span><span class="sxs-lookup"><span data-stu-id="fda28-114">This walkthrough begins by describing hello prerequisites and resources that are needed toocomplete hello tasks with Data Lake Analytics that form hello data science process and how tooinstall them.</span></span> <span data-ttu-id="fda28-115">Ezután U-SQL használatával hello adatfeldolgozási lépéseit sorolja fel, és arra a következtetésre jut jelenít meg hogyan toouse Python és az Azure Machine Learning Studio toobuild struktúra és üzembe prediktív modelleket hello.</span><span class="sxs-lookup"><span data-stu-id="fda28-115">Then it outlines hello data processing steps using U-SQL and concludes by showing how toouse Python and Hive with Azure Machine Learning Studio toobuild and deploy hello predictive models.</span></span> 

### <a name="u-sql-and-visual-studio"></a><span data-ttu-id="fda28-116">U-SQL és a Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fda28-116">U-SQL and Visual Studio</span></span>
<span data-ttu-id="fda28-117">Ez a forgatókönyv azt javasolja, hogy a Visual Studio tooedit U-SQL parancsfájlok tooprocess hello adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="fda28-117">This walkthrough recommends using Visual Studio tooedit U-SQL scripts tooprocess hello dataset.</span></span> <span data-ttu-id="fda28-118">hello U-SQL-parancsfájlok ismerteti, és egy különálló fájlban megadott.</span><span class="sxs-lookup"><span data-stu-id="fda28-118">hello U-SQL scripts are described here and provided in a separate file.</span></span> <span data-ttu-id="fda28-119">hello folyamat választásával dolgozhat fel, felfedezése és hello az adatok mintavétele tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="fda28-119">hello process includes ingesting, exploring, and sampling hello data.</span></span> <span data-ttu-id="fda28-120">Azt is bemutatja, hogyan toorun egy U-SQL parancsfájl-alapú hello Azure-portál a feladat.</span><span class="sxs-lookup"><span data-stu-id="fda28-120">It also shows how toorun a U-SQL scripted job from hello Azure portal.</span></span> <span data-ttu-id="fda28-121">Hive táblák hello adatok egy társított HDInsight fürt toofacilitate hello épület és a központi telepítés egy bináris besorolási modell az Azure Machine Learning Studióban jön létre.</span><span class="sxs-lookup"><span data-stu-id="fda28-121">Hive tables are created for hello data in an associated HDInsight cluster toofacilitate hello building and deployment of a binary classification model in Azure Machine Learning Studio.</span></span>  

### <a name="python"></a><span data-ttu-id="fda28-122">Python</span><span class="sxs-lookup"><span data-stu-id="fda28-122">Python</span></span>
<span data-ttu-id="fda28-123">Ez a forgatókönyv is egy szakaszt tartalmaz, amely bemutatja, hogyan toobuild és központi telepítése a Python használata az Azure Machine Learning Studio prediktív modellt.</span><span class="sxs-lookup"><span data-stu-id="fda28-123">This walkthrough also contains a section that shows how toobuild and deploy a predictive model using Python with Azure Machine Learning Studio.</span></span>  <span data-ttu-id="fda28-124">Jupyter notebook hello Python parancsfájlokkal, ezeket a lépéseket, a folyamat a nyújtunk.</span><span class="sxs-lookup"><span data-stu-id="fda28-124">We provide a Jupyter notebook with hello Python scripts for these steps in this process.</span></span> <span data-ttu-id="fda28-125">hello notebook néhány további szolgáltatás mérnöki lépéseit, valamint a például a multiclass besorolást és a modellezési továbbá toohello bináris osztályozási modell itt vázolt regressziós modell konstrukció kódot tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="fda28-125">hello notebook includes code for some additional feature engineering steps and models construction such as multiclass classification and regression modeling in addition toohello binary classification model outlined here.</span></span> <span data-ttu-id="fda28-126">hello regressziós feladat érték toopredict hello hello tipp más tip szolgáltatások alapján.</span><span class="sxs-lookup"><span data-stu-id="fda28-126">hello regression task is toopredict hello amount of hello tip based on other tip features.</span></span> 

### <a name="azure-machine-learning"></a><span data-ttu-id="fda28-127">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="fda28-127">Azure Machine Learning</span></span>
<span data-ttu-id="fda28-128">Az Azure Machine Learning Studio használt toobuild és üzembe prediktív modelleket hello.</span><span class="sxs-lookup"><span data-stu-id="fda28-128">Azure Machine Learning Studio is used toobuild and deploy hello predictive models.</span></span> <span data-ttu-id="fda28-129">Ebben az esetben két módszer használatával: első Python-parancsfájlok majd Hive táblák HDInsight (Hadoop) fürtökön.</span><span class="sxs-lookup"><span data-stu-id="fda28-129">This is done using two approaches: first with Python scripts and then with Hive tables on an HDInsight (Hadoop) cluster.</span></span>

### <a name="scripts"></a><span data-ttu-id="fda28-130">Parancsprogramok</span><span class="sxs-lookup"><span data-stu-id="fda28-130">Scripts</span></span>
<span data-ttu-id="fda28-131">A bemutatóban szereplő eljárásokat csak hello egyszerű lépéseket.</span><span class="sxs-lookup"><span data-stu-id="fda28-131">Only hello principal steps are outlined in this walkthrough.</span></span> <span data-ttu-id="fda28-132">Letöltheti a teljes hello **U-SQL parancsfájl** és **Jupyter Notebook** a [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).</span><span class="sxs-lookup"><span data-stu-id="fda28-132">You can download hello full **U-SQL script** and **Jupyter Notebook** from [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fda28-133">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fda28-133">Prerequisites</span></span>
<span data-ttu-id="fda28-134">Ezek a témakörök megkezdése előtt hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="fda28-134">Before you begin these topics, you must have hello following:</span></span>

* <span data-ttu-id="fda28-135">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="fda28-135">An Azure subscription.</span></span> <span data-ttu-id="fda28-136">Ha még nem rendelkezik egy, lásd: [beolvasása az Azure ingyenes próbaverzió](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="fda28-136">If you do not already have one, see [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="fda28-137">[Ajánlott] A Visual Studio 2013 vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="fda28-137">[Recommended] Visual Studio 2013 or later.</span></span> <span data-ttu-id="fda28-138">Ha még nem rendelkezik ilyen verziójú telepítve, akkor egy ingyenes közösségi verziója letölthető a [Visual Studio Community](https://www.visualstudio.com/vs/community/).</span><span class="sxs-lookup"><span data-stu-id="fda28-138">If you do not already have one of these versions installed, you can download a free Community version from [Visual Studio Community](https://www.visualstudio.com/vs/community/).</span></span>

> [!NOTE]
> <span data-ttu-id="fda28-139">Visual Studio helyett hello Azure Portal toosubmit Azure Data Lake-lekérdezéseket is használhatja.</span><span class="sxs-lookup"><span data-stu-id="fda28-139">Instead of Visual Studio, you can also use hello Azure Portal toosubmit Azure Data Lake queries.</span></span> <span data-ttu-id="fda28-140">Hogyan toodo, a Visual Studio- és kilépéskor egyaránt hello portal hello szakaszában jelenik meg a Microsoft szükséges műveleteket **feldolgozni az adatokat az U-SQL**.</span><span class="sxs-lookup"><span data-stu-id="fda28-140">We will provide instructions on how toodo so both with Visual Studio and on hello portal in hello section titled **Process data with U-SQL**.</span></span> 
> 
> 


## <a name="prepare-data-science-environment-for-azure-data-lake"></a><span data-ttu-id="fda28-141">Azure Data Lake data tudományos környezet előkészítése</span><span class="sxs-lookup"><span data-stu-id="fda28-141">Prepare data science environment for Azure Data Lake</span></span>
<span data-ttu-id="fda28-142">tooprepare hello adatok tudományos környezetben ennél a bemutatónál hozza létre a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="fda28-142">tooprepare hello data science environment for this walkthrough, create hello following resources:</span></span>

* <span data-ttu-id="fda28-143">Az Azure Data Lake Store-(ADLS)</span><span class="sxs-lookup"><span data-stu-id="fda28-143">Azure Data Lake Store (ADLS)</span></span> 
* <span data-ttu-id="fda28-144">Az Azure Data Lake Analytics (ADLA)</span><span class="sxs-lookup"><span data-stu-id="fda28-144">Azure Data Lake Analytics (ADLA)</span></span>
* <span data-ttu-id="fda28-145">Az Azure Blob storage-fiók</span><span class="sxs-lookup"><span data-stu-id="fda28-145">Azure Blob storage account</span></span>
* <span data-ttu-id="fda28-146">Azure Machine Learning Studio-fiók</span><span class="sxs-lookup"><span data-stu-id="fda28-146">Azure Machine Learning Studio account</span></span>
* <span data-ttu-id="fda28-147">Az Azure Data Lake Tools for Visual Studio (ajánlott)</span><span class="sxs-lookup"><span data-stu-id="fda28-147">Azure Data Lake Tools for Visual Studio (Recommended)</span></span>

<span data-ttu-id="fda28-148">Ez a szakasz útmutatás toocreate minden erőforrásainak.</span><span class="sxs-lookup"><span data-stu-id="fda28-148">This section provides instructions on how toocreate each of these resources.</span></span> <span data-ttu-id="fda28-149">Ha úgy dönt, toouse Hive táblák Azure Machine Learning segítségével, Python, a modell toobuild helyett tooprovision (Hadoop) HDInsight-fürtök is szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="fda28-149">If you choose toouse Hive tables with Azure Machine Learning, instead of Python, toobuild a model,you will also need tooprovision an HDInsight (Hadoop) cluster.</span></span> <span data-ttu-id="fda28-150">Az alternatív eljárás hello megfelelő részben leírt.</span><span class="sxs-lookup"><span data-stu-id="fda28-150">This alternative procedure in described in hello appropriate section below.</span></span>


> [!NOTE]
> <span data-ttu-id="fda28-151">Hello **Azure Data Lake Store** is létrehozható, vagy külön-külön vagy hello létrehozásakor **Azure Data Lake Analytics** hello alapértelmezett tárolóként.</span><span class="sxs-lookup"><span data-stu-id="fda28-151">hello **Azure Data Lake Store** can be created either separately or when you create hello **Azure Data Lake Analytics** as hello default storage.</span></span> <span data-ttu-id="fda28-152">Ezek külön-külön az alábbi erőforrások mindegyikének létrehozására vonatkozó utasításokat hivatkozott, de hello Data Lake-tárfiókot kell nem kell külön hoz létre.</span><span class="sxs-lookup"><span data-stu-id="fda28-152">Instructions are referenced for creating each of these resources separately below, but hello Data Lake storage account need not be created separately.</span></span>
>
> 

### <a name="create-an-azure-data-lake-store"></a><span data-ttu-id="fda28-153">Hozzon létre egy Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="fda28-153">Create an Azure Data Lake Store</span></span>


<span data-ttu-id="fda28-154">Hozzon létre egy ADLS a hello [Azure Portal](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fda28-154">Create an ADLS from hello [Azure Portal](http://portal.azure.com).</span></span> <span data-ttu-id="fda28-155">További információkért lásd: [HDInsight-fürtök létrehozása az Azure portál használatával a Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="fda28-155">For details, see [Create an HDInsight cluster with Data Lake Store using Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span> <span data-ttu-id="fda28-156">Meg arról, hogy tooset hello a fürt AAD-identitása hello fel kell **DataSource** panelen található hello **opcionális konfigurációs** panel leírt van.</span><span class="sxs-lookup"><span data-stu-id="fda28-156">Be sure tooset up hello Cluster AAD Identity in hello **DataSource** blade of hello **Optional Configuration** blade described there.</span></span> 

 ![3](./media/machine-learning-data-science-process-data-lake-walkthrough/3-create-ADLS.PNG)

### <a name="create-an-azure-data-lake-analytics-account"></a><span data-ttu-id="fda28-158">Azure Data Lake Analytics-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="fda28-158">Create an Azure Data Lake Analytics account</span></span>
<span data-ttu-id="fda28-159">ADLA-fiók létrehozása a hello [Azure Portal](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fda28-159">Create an ADLA account from hello [Azure Portal](http://portal.azure.com).</span></span> <span data-ttu-id="fda28-160">További információkért lásd: [oktatóanyag: Ismerkedés az Azure Data Lake Analytics Azure portál használatával](../data-lake-analytics/data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="fda28-160">For details, see [Tutorial: get started with Azure Data Lake Analytics using Azure Portal](../data-lake-analytics/data-lake-analytics-get-started-portal.md).</span></span> 

 ![4](./media/machine-learning-data-science-process-data-lake-walkthrough/4-create-ADLA-new.PNG)

### <a name="create-an-azure-blob-storage-account"></a><span data-ttu-id="fda28-162">Egy Azure Blob storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="fda28-162">Create an Azure Blob storage account</span></span>
<span data-ttu-id="fda28-163">Egy Azure Blob storage-fiók létrehozása hello [Azure Portal](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fda28-163">Create an Azure Blob storage account from hello [Azure Portal](http://portal.azure.com).</span></span> <span data-ttu-id="fda28-164">További információkért lásd: hello hozzon létre egy tárfiókot című [tudnivalók az Azure storage-fiókok](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="fda28-164">For details, see hello Create a storage account section in [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>

 ![5](./media/machine-learning-data-science-process-data-lake-walkthrough/5-Create-Azure-Blob.PNG)

### <a name="set-up-an-azure-machine-learning-studio-account"></a><span data-ttu-id="fda28-166">Az Azure Machine Learning Studio fiók beállítása</span><span class="sxs-lookup"><span data-stu-id="fda28-166">Set up an Azure Machine Learning Studio account</span></span>
<span data-ttu-id="fda28-167">Jelentkezzen be- / az Azure Machine Learning Studio a hello [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) lap.</span><span class="sxs-lookup"><span data-stu-id="fda28-167">Sign up/into Azure Machine Learning Studio from hello [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) page.</span></span> <span data-ttu-id="fda28-168">Kattintson a hello **az induláshoz** gombra, majd válassza a "Szabad munkaterület" vagy "Szabványos munkaterület".</span><span class="sxs-lookup"><span data-stu-id="fda28-168">Click on hello **Get started now** button and then choose a "Free Workspace" or "Standard Workspace".</span></span> <span data-ttu-id="fda28-169">Ezt követően az Azure ML Studio képes toocreate kísérletek fogja.</span><span class="sxs-lookup"><span data-stu-id="fda28-169">After this you will be able toocreate experiments in Azure ML Studio.</span></span>  

### <a name="install-azure-data-lake-tools-recommended"></a><span data-ttu-id="fda28-170">Telepítheti az Azure Data Lake Tools [ajánlott]</span><span class="sxs-lookup"><span data-stu-id="fda28-170">Install Azure Data Lake Tools [Recommended]</span></span>
<span data-ttu-id="fda28-171">Azure Data Lake Tools telepítése a Visual Studio verziójának [Azure Data Lake Tools for Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span><span class="sxs-lookup"><span data-stu-id="fda28-171">Install Azure Data Lake Tools for your version of Visual Studio from [Azure Data Lake Tools for Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span></span>

 ![6](./media/machine-learning-data-science-process-data-lake-walkthrough/6-install-ADL-tools-VS.PNG)

<span data-ttu-id="fda28-173">Hello telepítés sikeres befejezése után nyissa meg a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fda28-173">After hello installation finishes successfully, open up Visual Studio.</span></span> <span data-ttu-id="fda28-174">Hello Data Lake lapon hello menü hello felső kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="fda28-174">You should see hello Data Lake tab hello menu at hello top.</span></span> <span data-ttu-id="fda28-175">Az Azure-erőforrások meg kell jelennie a hello bal oldali panelen történő bejelentkezéskor be Azure-fiókjába.</span><span class="sxs-lookup"><span data-stu-id="fda28-175">Your Azure resources should appear in hello left panel when you sign into your Azure account.</span></span>

 ![7](./media/machine-learning-data-science-process-data-lake-walkthrough/7-install-ADL-tools-VS-done.PNG)

## <a name="hello-nyc-taxi-trips-dataset"></a><span data-ttu-id="fda28-177">hello NYC Taxi Utazgatással adatkészlet</span><span class="sxs-lookup"><span data-stu-id="fda28-177">hello NYC Taxi Trips dataset</span></span>
<span data-ttu-id="fda28-178">hello itt használtuk adatkészlet nyilvánosan elérhető dataset – hello [NYC Taxi Utazgatással dataset](http://www.andresmh.com/nyctaxitrips/).</span><span class="sxs-lookup"><span data-stu-id="fda28-178">hello data set we used here is a publicly available dataset -- hello [NYC Taxi Trips dataset](http://www.andresmh.com/nyctaxitrips/).</span></span> <span data-ttu-id="fda28-179">hello NYC Taxi út adatok körülbelül 20GB tömörített CSV-fájlok (~ 48GB tömörítetlen) áll, több mint 173 millió rögzítése egyéni utak és hello turistajegyek esetében minden út kifizette.</span><span class="sxs-lookup"><span data-stu-id="fda28-179">hello NYC Taxi Trip data consists of about 20GB of compressed CSV files (~48GB uncompressed), recording more than 173 million individual trips and hello fares paid for each trip.</span></span> <span data-ttu-id="fda28-180">Minden út rekord hello felvétel és Gyűjtőtár helyeket tartalmaz, és időpontokat, anonimizált adatokon alapul (illesztőprogram) licencszám ellophatja, és hello medallion (taxi tartozó egyedi azonosító) száma.</span><span class="sxs-lookup"><span data-stu-id="fda28-180">Each trip record includes hello pickup and drop-off locations and times, anonymized hack (driver's) license number, and hello medallion (taxi’s unique id) number.</span></span> <span data-ttu-id="fda28-181">hello adatok hello évben 2013 hozzá van rendelve minden való adatváltások számát, és megtalálható a következő két adatkészletet havonta hello:</span><span class="sxs-lookup"><span data-stu-id="fda28-181">hello data covers all trips in hello year 2013 and is provided in hello following two datasets for each month:</span></span>

* <span data-ttu-id="fda28-182">hello "trip_data" CSV út részleteit, például a utasok, a felvételi és dropoff pontok, út időtartama és út hossza tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="fda28-182">hello 'trip_data' CSV contains trip details, such as number of passengers, pickup and dropoff points, trip duration, and trip length.</span></span> <span data-ttu-id="fda28-183">Íme néhány példa rekordok:</span><span class="sxs-lookup"><span data-stu-id="fda28-183">Here are a few sample records:</span></span>
  
       medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count, trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
       89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
* <span data-ttu-id="fda28-184">hello "trip_fare" CSV hello jegy ára minden út, például a fizetési módot, jegy ára összeg, emelt díjas és, tippeket és autópályadíjakra, és kifizetett hello teljes összeg kifizette részleteit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="fda28-184">hello 'trip_fare' CSV contains details of hello fare paid for each trip, such as payment type, fare amount, surcharge and taxes, tips and tolls, and hello total amount paid.</span></span> <span data-ttu-id="fda28-185">Íme néhány példa rekordok:</span><span class="sxs-lookup"><span data-stu-id="fda28-185">Here are a few sample records:</span></span>
  
       medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
       89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

<span data-ttu-id="fda28-186">hello egyedi kulcs toojoin út\_adatok és út\_jegy ára tevődnek össze a következő három mezőt hello: medallion, rejthetők el\_licenc és a felvételi\_datetime.</span><span class="sxs-lookup"><span data-stu-id="fda28-186">hello unique key toojoin trip\_data and trip\_fare is composed of hello following three fields: medallion, hack\_license and pickup\_datetime.</span></span> <span data-ttu-id="fda28-187">egy nyilvános Azure storage-blob hello nyers CSV-fájlok elérhetők.</span><span class="sxs-lookup"><span data-stu-id="fda28-187">hello raw CSV files can be accessed from a public Azure storage blob.</span></span> <span data-ttu-id="fda28-188">U-SQL parancsfájl hello ezt az összekapcsolást érték a hello [út és a jegy ára csatlakozás](#join) szakasz.</span><span class="sxs-lookup"><span data-stu-id="fda28-188">hello U-SQL script for this join is in hello [Join trip and fare tables](#join) section.</span></span>

## <a name="process-data-with-u-sql"></a><span data-ttu-id="fda28-189">A U-SQL adatok feldolgozása</span><span class="sxs-lookup"><span data-stu-id="fda28-189">Process data with U-SQL</span></span>
<span data-ttu-id="fda28-190">hello adatfeldolgozási ebben a szakaszban ismertetett feladatok választásával dolgozhat fel, minőségi ellenőrzése, felfedezése és hello az adatok mintavétele.</span><span class="sxs-lookup"><span data-stu-id="fda28-190">hello data processing tasks illustrated in this section include ingesting, checking quality, exploring, and sampling hello data.</span></span> <span data-ttu-id="fda28-191">Is megmutatjuk, hogyan toojoin út és a jegy ára tábla.</span><span class="sxs-lookup"><span data-stu-id="fda28-191">We also show how toojoin trip and fare tables.</span></span> <span data-ttu-id="fda28-192">hello végső szakasz futtatási bemutatja a U-SQL parancsfájl feladatot hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="fda28-192">hello final section shows run a U-SQL scripted job from hello Azure portal.</span></span> <span data-ttu-id="fda28-193">Az alábbiakban hivatkozások tooeach alszakasz:</span><span class="sxs-lookup"><span data-stu-id="fda28-193">Here are links tooeach subsection:</span></span>

* [<span data-ttu-id="fda28-194">Adatfeldolgozást: nyilvános blob adatainak olvasása</span><span class="sxs-lookup"><span data-stu-id="fda28-194">Data ingestion: read in data from public blob</span></span>](#ingest)
* [<span data-ttu-id="fda28-195">Adatminőségi ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="fda28-195">Data quality checks</span></span>](#quality)
* [<span data-ttu-id="fda28-196">Az adatok feltárása</span><span class="sxs-lookup"><span data-stu-id="fda28-196">Data exploration</span></span>](#explore)
* [<span data-ttu-id="fda28-197">Csatlakozás út és a jegy ára</span><span class="sxs-lookup"><span data-stu-id="fda28-197">Join trip and fare tables</span></span>](#join)
* [<span data-ttu-id="fda28-198">Adat-mintavételezésre</span><span class="sxs-lookup"><span data-stu-id="fda28-198">Data sampling</span></span>](#sample)
* [<span data-ttu-id="fda28-199">U-SQL feladatok futtatása</span><span class="sxs-lookup"><span data-stu-id="fda28-199">Run U-SQL jobs</span></span>](#run)

<span data-ttu-id="fda28-200">hello U-SQL-parancsfájlok ismerteti, és egy különálló fájlban megadott.</span><span class="sxs-lookup"><span data-stu-id="fda28-200">hello U-SQL scripts are described here and provided in a separate file.</span></span> <span data-ttu-id="fda28-201">Letöltheti a teljes hello **U-SQL-parancsfájlok** a [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).</span><span class="sxs-lookup"><span data-stu-id="fda28-201">You can download hello full **U-SQL scripts** from [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).</span></span>

<span data-ttu-id="fda28-202">tooexecute U-SQL, nyissa meg a Visual Studio, kattintson a **fájl--> Új projekt-->**, válassza a **U-SQL projekt**, a nevet, és mentse tooa mappa.</span><span class="sxs-lookup"><span data-stu-id="fda28-202">tooexecute U-SQL, Open Visual Studio, click **File --> New --> Project**, choose **U-SQL Project**, name and save it tooa folder.</span></span>

![8](./media/machine-learning-data-science-process-data-lake-walkthrough/8-create-USQL-project.PNG)

> [!NOTE]
> <span data-ttu-id="fda28-204">Már lehetséges toouse hello Azure Portal tooexecute U-SQL Visual Studio helyett.</span><span class="sxs-lookup"><span data-stu-id="fda28-204">It is possible toouse hello Azure Portal tooexecute U-SQL instead of Visual Studio.</span></span> <span data-ttu-id="fda28-205">Keresse meg az Azure Data Lake Analytics-erőforrás toohello hello portálon, és közvetlenül, a következő ábra hello illusztrált lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="fda28-205">You can navigate toohello Azure Data Lake Analytics resource on hello portal and submit queries directly as illustrated in hello following figure.</span></span>
> 
> 

![9](./media/machine-learning-data-science-process-data-lake-walkthrough/9-portal-submit-job.PNG)

### <span data-ttu-id="fda28-207"><a name="ingest"></a>Adatfeldolgozást: A nyilvános blob adatainak olvasása</span><span class="sxs-lookup"><span data-stu-id="fda28-207"><a name="ingest"></a>Data Ingestion: Read in data from public blob</span></span>
<span data-ttu-id="fda28-208">hello adatok Azure blob hello hello helyét hivatkozott  **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name**  és használatával kiolvasható **Extractors.Csv()**.</span><span class="sxs-lookup"><span data-stu-id="fda28-208">hello location of hello data in hello Azure blob is referenced as **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name** and can be extracted using **Extractors.Csv()**.</span></span> <span data-ttu-id="fda28-209">Helyettesítse a saját tároló nevének és a tárfiók nevét az alábbi parancsfájlok a container_name@blob_storage_account_name hello wasb címét.</span><span class="sxs-lookup"><span data-stu-id="fda28-209">Substitute your own container name and storage account name in following scripts for container_name@blob_storage_account_name in hello wasb address.</span></span> <span data-ttu-id="fda28-210">Mivel ugyanazt a formátumot a hello fájlneveket, használhatjuk **út\_data_ {\*\}.csv** tooread összes 12 út fájl.</span><span class="sxs-lookup"><span data-stu-id="fda28-210">Since hello file names are in same format, we can use **trip\_data_{\*\}.csv** tooread in all 12 trip files.</span></span> 

    ///Read in Trip data
    @trip0 =
        EXTRACT 
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string
    // This is reading 12 trip data from blob
    FROM "wasb://container_name@blob_storage_account_name.blob.core.windows.net/nyctaxitrip/trip_data_{*}.csv"
    USING Extractors.Csv();

<span data-ttu-id="fda28-211">Mivel hello első sorában fejlécek, azt kell tooremove hello fejlécek, és oszloptípus átalakítása megfelelő néhányat a meglévők közül.</span><span class="sxs-lookup"><span data-stu-id="fda28-211">Since there are headers in hello first row, we need tooremove hello headers and change column types into appropriate ones.</span></span> <span data-ttu-id="fda28-212">Azt a mentés elvégezhető feldolgozott hello tooAzure Data Lake Storage használatával végzett **swebhdfs://data_lake_storage_name.azuredatalakestorage.net/folder_name/file_name**_ vagy tooAzure Blob-tároló fiók használatával  **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name** .</span><span class="sxs-lookup"><span data-stu-id="fda28-212">We can either save hello processed data tooAzure Data Lake Storage using **swebhdfs://data_lake_storage_name.azuredatalakestorage.net/folder_name/file_name**_ or tooAzure Blob storage account using  **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name**.</span></span> 

    // change data types
    @trip =
        SELECT 
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        DateTime.Parse(pickup_datetime) AS pickup_datetime,
        DateTime.Parse(dropoff_datetime) AS dropoff_datetime,
        Int32.Parse(passenger_count) AS passenger_count,
        Double.Parse(trip_time_in_secs) AS trip_time_in_secs,
        Double.Parse(trip_distance) AS trip_distance,
        (pickup_longitude==string.Empty ? 0: float.Parse(pickup_longitude)) AS pickup_longitude,
        (pickup_latitude==string.Empty ? 0: float.Parse(pickup_latitude)) AS pickup_latitude,
        (dropoff_longitude==string.Empty ? 0: float.Parse(dropoff_longitude)) AS dropoff_longitude,
        (dropoff_latitude==string.Empty ? 0: float.Parse(dropoff_latitude)) AS dropoff_latitude
    FROM @trip0
    WHERE medallion != "medallion";

    ////output data tooADL
    OUTPUT @trip   
    too"swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_trip.csv"
    USING Outputters.Csv(); 

    ////Output data tooblob
    OUTPUT @trip   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_trip.csv"
    USING Outputters.Csv();  

<span data-ttu-id="fda28-213">Hasonlóképpen azt olvasási hello jegy ára adathalmaz.</span><span class="sxs-lookup"><span data-stu-id="fda28-213">Similarly we can read in hello fare data sets.</span></span> <span data-ttu-id="fda28-214">Kattintson a jobb gombbal az Azure Data Lake Store, választhatja az adatok toolook **Azure Portal adatkezelő-->** vagy **Fájlkezelőben** Visual studióban.</span><span class="sxs-lookup"><span data-stu-id="fda28-214">Right click Azure Data Lake Store, you can choose toolook at your data in **Azure Portal --> Data Explorer** or **File Explorer** within Visual Studio.</span></span> 

 ![10](./media/machine-learning-data-science-process-data-lake-walkthrough/10-data-in-ADL-VS.PNG)

 ![11](./media/machine-learning-data-science-process-data-lake-walkthrough/11-data-in-ADL.PNG)

### <span data-ttu-id="fda28-217"><a name="quality"></a>Adatminőségi ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="fda28-217"><a name="quality"></a>Data quality checks</span></span>
<span data-ttu-id="fda28-218">Miután út és a jegy ára az olvasott, adatminőségi ellenőrzése a következő módon hello végezhető.</span><span class="sxs-lookup"><span data-stu-id="fda28-218">After trip and fare tables have been read in, data quality checks can be done in hello following way.</span></span> <span data-ttu-id="fda28-219">CSV-fájlok eredő hello kimeneti tooAzure Blob-tároló vagy az Azure Data Lake Store lehet.</span><span class="sxs-lookup"><span data-stu-id="fda28-219">hello resulting CSV files can be output tooAzure Blob storage or Azure Data Lake Store.</span></span> 

<span data-ttu-id="fda28-220">Keresse meg medallions hello száma és medallions egyedi száma:</span><span class="sxs-lookup"><span data-stu-id="fda28-220">Find hello number of medallions and unique number of medallions:</span></span>

    ///check hello number of medallions and unique number of medallions
    @trip2 =
        SELECT
        medallion,
        vendor_id,
        pickup_datetime.Month AS pickup_month
        FROM @trip;

    @ex_1 =
        SELECT
        pickup_month, 
        COUNT(medallion) AS cnt_medallion,
        COUNT(DISTINCT(medallion)) AS unique_medallion
        FROM @trip2
        GROUP BY pickup_month;
        OUTPUT @ex_1   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_1.csv"
    USING Outputters.Csv(); 

<span data-ttu-id="fda28-221">Keresse meg azokat, amelyek 100-nál több utazgatással kellett medallions:</span><span class="sxs-lookup"><span data-stu-id="fda28-221">Find those medallions that had more than 100 trips:</span></span>

    ///find those medallions that had more than 100 trips
    @ex_2 =
        SELECT medallion,
               COUNT(medallion) AS cnt_medallion
        FROM @trip2
        //where pickup_datetime >= "2013-01-01t00:00:00.0000000" and pickup_datetime <= "2013-04-01t00:00:00.0000000"
        GROUP BY medallion
        HAVING COUNT(medallion) > 100;
        OUTPUT @ex_2   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_2.csv"
    USING Outputters.Csv(); 

<span data-ttu-id="fda28-222">Található érvénytelen rekordokat pickup_longitude tekintetében:</span><span class="sxs-lookup"><span data-stu-id="fda28-222">Find those invalid records in terms of pickup_longitude:</span></span>

    ///find those invalid records in terms of pickup_longitude
    @ex_3 =
        SELECT COUNT(medallion) AS cnt_invalid_pickup_longitude
        FROM @trip
        WHERE
        pickup_longitude <- 90 OR pickup_longitude > 90;
        OUTPUT @ex_3   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_3.csv"
    USING Outputters.Csv(); 

<span data-ttu-id="fda28-223">Az egyes változók hiányzó értékeinek megkeresése:</span><span class="sxs-lookup"><span data-stu-id="fda28-223">Find missing values for some variables:</span></span>

    //check missing values
    @res =
        SELECT *,
               (medallion == null? 1 : 0) AS missing_medallion
        FROM @trip;

    @trip_summary6 =
        SELECT 
            vendor_id,
        SUM(missing_medallion) AS medallion_empty, 
        COUNT(medallion) AS medallion_total,
        COUNT(DISTINCT(medallion)) AS medallion_total_unique  
        FROM @res
        GROUP BY vendor_id;
    OUTPUT @trip_summary6
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_16.csv"
    USING Outputters.Csv();



### <span data-ttu-id="fda28-224"><a name="explore"></a>Az adatok feltárása</span><span class="sxs-lookup"><span data-stu-id="fda28-224"><a name="explore"></a>Data exploration</span></span>
<span data-ttu-id="fda28-225">Tehetünk ennek néhány adatok feltárása tooget hello adatok jobb megértése.</span><span class="sxs-lookup"><span data-stu-id="fda28-225">We can do some data exploration tooget a better understanding of hello data.</span></span>

<span data-ttu-id="fda28-226">Hello terjesztési Formabontó és nem Formabontó utak keresése:</span><span class="sxs-lookup"><span data-stu-id="fda28-226">Find hello distribution of tipped and non-tipped trips:</span></span>

    ///tipped vs. not tipped distribution
    @tip_or_not =
        SELECT *,
               (tip_amount > 0 ? 1: 0) AS tipped
        FROM @fare;

    @ex_4 =
        SELECT tipped,
               COUNT(*) AS tip_freq
        FROM @tip_or_not
        GROUP BY tipped;
        OUTPUT @ex_4   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_4.csv"
    USING Outputters.Csv(); 

<span data-ttu-id="fda28-227">Tipp összeg lezárási értékekkel hello terjesztési található: 0,5,10 és 20 dollár.</span><span class="sxs-lookup"><span data-stu-id="fda28-227">Find hello distribution of tip amount with cut-off values: 0,5,10,and 20 dollars.</span></span>

    //tip class/range distribution
    @tip_class =
        SELECT *,
               (tip_amount >20? 4: (tip_amount >10? 3:(tip_amount >5 ? 2:(tip_amount > 0 ? 1: 0)))) AS tip_class
        FROM @fare;
    @ex_5 =
        SELECT tip_class,
               COUNT(*) AS tip_freq
        FROM @tip_class
        GROUP BY tip_class;
        OUTPUT @ex_5   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_5.csv"
    USING Outputters.Csv(); 

<span data-ttu-id="fda28-228">Keresse meg a út távolság alapvető statisztikai adatait tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="fda28-228">Find basic statistics of trip distance:</span></span>

    // find basic statistics for trip_distance
    @trip_summary4 =
        SELECT 
            vendor_id,
            COUNT(*) AS cnt_row,
            MIN(trip_distance) AS min_trip_distance,
            MAX(trip_distance) AS max_trip_distance,
            AVG(trip_distance) AS avg_trip_distance 
        FROM @trip
        GROUP BY vendor_id;
    OUTPUT @trip_summary4
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_14.csv"
    USING Outputters.Csv();

<span data-ttu-id="fda28-229">Hello százalékos érték út távolság keresése:</span><span class="sxs-lookup"><span data-stu-id="fda28-229">Find hello percentiles of trip distance:</span></span>

    // find percentiles of trip_distance
    @trip_summary3 =
        SELECT DISTINCT vendor_id AS vendor,
                        PERCENTILE_DISC(0.25) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.5) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.75) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc
        FROM @trip;
       // group by vendor_id;
    OUTPUT @trip_summary3
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_13.csv"
    USING Outputters.Csv(); 


### <span data-ttu-id="fda28-230"><a name="join"></a>Csatlakozás út és a jegy ára</span><span class="sxs-lookup"><span data-stu-id="fda28-230"><a name="join"></a>Join trip and fare tables</span></span>
<span data-ttu-id="fda28-231">Út és a jegy ára medallion, hack_license és pickup_time társíthatók.</span><span class="sxs-lookup"><span data-stu-id="fda28-231">Trip and fare tables can be joined by medallion, hack_license, and pickup_time.</span></span>

    //join trip and fare table

    @model_data_full =
    SELECT t.*, 
    f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
    (f.tip_amount > 0 ? 1: 0) AS tipped,
    (f.tip_amount >20? 4: (f.tip_amount >10? 3:(f.tip_amount >5 ? 2:(f.tip_amount > 0 ? 1: 0)))) AS tip_class
    FROM @trip AS t JOIN  @fare AS f
    ON   (t.medallion == f.medallion AND t.hack_license == f.hack_license AND t.pickup_datetime == f.pickup_datetime)
    WHERE   (pickup_longitude != 0 AND dropoff_longitude != 0 );

    //// output tooblob
    OUTPUT @model_data_full   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 

    ////output data tooADL
    OUTPUT @model_data_full   
    too"swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 


<span data-ttu-id="fda28-232">Az egyes utas száma kiszámítása hello rekordok, átlagos tipp összeg, tipp összeg varianciáját, százalékos aránya Formabontó való adatváltások számát.</span><span class="sxs-lookup"><span data-stu-id="fda28-232">For each level of passenger count, calculate hello number of records, average tip amount, variance of tip amount, percentage of tipped trips.</span></span>

    // contigency table
    @trip_summary8 =
        SELECT passenger_count,
               COUNT(*) AS cnt,
               AVG(tip_amount) AS avg_tip_amount,
               VAR(tip_amount) AS var_tip_amount,
               SUM(tipped) AS cnt_tipped,
               (float)SUM(tipped)/COUNT(*) AS pct_tipped
        FROM @model_data_full
        GROUP BY passenger_count;
        OUTPUT @trip_summary8
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_17.csv"
    USING Outputters.Csv();


### <span data-ttu-id="fda28-233"><a name="sample"></a>Adat-mintavételezésre</span><span class="sxs-lookup"><span data-stu-id="fda28-233"><a name="sample"></a>Data sampling</span></span>
<span data-ttu-id="fda28-234">Először igazolnia véletlenszerűen választ ki 0,1 % hello adatok hello illesztett táblából:</span><span class="sxs-lookup"><span data-stu-id="fda28-234">First we randomly select 0.1% of hello data from hello joined table:</span></span>

    //random select 1/1000 data for modeling purpose
    @addrownumberres_randomsample =
    SELECT *,
            ROW_NUMBER() OVER() AS rownum
    FROM @model_data_full;

    @model_data_random_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_randomsample
    WHERE rownum % 1000 == 0;

    OUTPUT @model_data_random_sample_1_1000   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_random_1_1000.csv"
    USING Outputters.Csv(); 

<span data-ttu-id="fda28-235">Majd végezzük rétegzett mintavételi bináris változó tip_class szerint:</span><span class="sxs-lookup"><span data-stu-id="fda28-235">Then we do stratified sampling by binary variable tip_class:</span></span>

    //stratified random select 1/1000 data for modeling purpose
    @addrownumberres_stratifiedsample =
    SELECT *,
            ROW_NUMBER() OVER(PARTITION BY tip_class) AS rownum
    FROM @model_data_full;

    @model_data_stratified_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_stratifiedsample
    WHERE rownum % 1000 == 0;
    //// output tooblob
    OUTPUT @model_data_stratified_sample_1_1000   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 
    ////output data tooADL
    OUTPUT @model_data_stratified_sample_1_1000   
    too"swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 


### <span data-ttu-id="fda28-236"><a name="run"></a>U-SQL feladatok futtatása</span><span class="sxs-lookup"><span data-stu-id="fda28-236"><a name="run"></a>Run U-SQL jobs</span></span>
<span data-ttu-id="fda28-237">Amikor befejezte a U-SQL-parancsfájlok szerkesztésével, elküldheti azokat toohello server az Azure Data Lake Analytics-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="fda28-237">When you finish editing U-SQL scripts, you can submit them toohello server using your Azure Data Lake Analytics account.</span></span> <span data-ttu-id="fda28-238">Kattintson a **Data Lake**, **feladat elküldése**, jelölje be a **Analytics-fiók**, válassza ki **párhuzamossági**, és kattintson a **küldje el a következőt** gombra.</span><span class="sxs-lookup"><span data-stu-id="fda28-238">Click **Data Lake**, **Submit Job**, select your **Analytics Account**, choose **Parallelism**, and click **Submit** button.</span></span>  

 ![12](./media/machine-learning-data-science-process-data-lake-walkthrough/12-submit-USQL.PNG)

<span data-ttu-id="fda28-240">Amikor hello feladat sikeresen megfelelést, a feladat állapotának hello jelenik meg a Visual Studio figyelésre.</span><span class="sxs-lookup"><span data-stu-id="fda28-240">When hello job is complied successfully, hello status of your job will be displayed in Visual Studio for monitoring.</span></span> <span data-ttu-id="fda28-241">Után hello feladat befejezése után történik, akkor még a visszajátszás hello feladat végrehajtási folyamatának, és megtudhatja, hello bottleneck lépéseket tooimprove a feladat hatékonyságát.</span><span class="sxs-lookup"><span data-stu-id="fda28-241">After hello job finishes running, you can even replay hello job execution process and find out hello bottleneck steps tooimprove your job efficiency.</span></span> <span data-ttu-id="fda28-242">Portál toocheck hello a U-SQL feladatok állapotának tooAzure is választhatja.</span><span class="sxs-lookup"><span data-stu-id="fda28-242">You can also go tooAzure Portal toocheck hello status of your U-SQL jobs.</span></span>

 ![13](./media/machine-learning-data-science-process-data-lake-walkthrough/13-USQL-running-v2.PNG)

 ![14](./media/machine-learning-data-science-process-data-lake-walkthrough/14-USQL-jobs-portal.PNG)

<span data-ttu-id="fda28-245">Most már ellenőrizheti a hello kimeneti fájlok az Azure Blob-tároló vagy Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="fda28-245">Now you can check hello output files in either Azure Blob storage or Azure Portal.</span></span> <span data-ttu-id="fda28-246">A következő lépésben hello modellezési műanyaggal rétegezett hello mintaadatok azt fogja használni.</span><span class="sxs-lookup"><span data-stu-id="fda28-246">We will use hello stratified sample data for our modeling in hello next step.</span></span>

 ![15](./media/machine-learning-data-science-process-data-lake-walkthrough/15-U-SQL-output-csv.PNG)

 ![16](./media/machine-learning-data-science-process-data-lake-walkthrough/16-U-SQL-output-csv-portal.PNG)

## <a name="build-and-deploy-models-in-azure-machine-learning"></a><span data-ttu-id="fda28-249">Hozza létre és telepítheti az Azure Machine Learning modellek</span><span class="sxs-lookup"><span data-stu-id="fda28-249">Build and deploy models in Azure Machine Learning</span></span>
<span data-ttu-id="fda28-250">Bemutatjuk, két lehetőség érhető el az Ön toopull adatokat az Azure Machine Learning toobuild és</span><span class="sxs-lookup"><span data-stu-id="fda28-250">We demonstrate two options available for you toopull data into Azure Machine Learning toobuild and</span></span> 

* <span data-ttu-id="fda28-251">Hello első lehetőség, használhatja az Azure Blob tooan írt mintát hello adatokat (a hello **adat-mintavételezésre** fenti lépést) használ a Python toobuild és központi telepítése az Azure Machine Learning modellek.</span><span class="sxs-lookup"><span data-stu-id="fda28-251">In hello first option, you use hello sampled data that has been written tooan Azure Blob (in hello **Data sampling** step above) and use Python toobuild and deploy models from Azure Machine Learning.</span></span> 
* <span data-ttu-id="fda28-252">A második lehetőség hello adatait hello Azure Data Lake közvetlenül a Hive-lekérdezések használatával.</span><span class="sxs-lookup"><span data-stu-id="fda28-252">In hello second option, you query hello data in Azure Data Lake directly using a Hive query.</span></span> <span data-ttu-id="fda28-253">Ez a beállítás megköveteli, hogy hozzon létre egy új HDInsight-fürtöt, vagy használja egy meglévő HDInsight-fürtre, ahol hello Hive táblák pont toohello NY Taxi adatokat az Azure Data Lake Storage.</span><span class="sxs-lookup"><span data-stu-id="fda28-253">This option requires that you create a new HDInsight cluster or use an existing HDInsight cluster where hello Hive tables point toohello NY Taxi data in Azure Data Lake Storage.</span></span>  <span data-ttu-id="fda28-254">Mindkét ezek a beállítások az alábbi arról lesz szó.</span><span class="sxs-lookup"><span data-stu-id="fda28-254">We discuss both these options below.</span></span> 

## <a name="option-1-use-python-toobuild-and-deploy-machine-learning-models"></a><span data-ttu-id="fda28-255">1. lehetőség: Használata Python toobuild és központi telepítése a machine learning modellek</span><span class="sxs-lookup"><span data-stu-id="fda28-255">Option 1: Use Python toobuild and deploy machine learning models</span></span>
<span data-ttu-id="fda28-256">toobuild és machine learning modellek pythonos környezetekben, a rendszer a Jupyter Notebook létrehozása, a helyi számítógépen vagy az Azure Machine Learning Studióban.</span><span class="sxs-lookup"><span data-stu-id="fda28-256">toobuild and deploy machine learning models using Python, create a Jupyter Notebook on your local machine or in Azure Machine Learning Studio.</span></span> <span data-ttu-id="fda28-257">hello Jupyter Notebook megadott [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough) tartalmaz hello teljes kód tooexplore, adatok, a szolgáltatás mérnöki csapathoz, a modellezési és a központi telepítés megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="fda28-257">hello Jupyter Notebook  provided on [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough) contains hello full code tooexplore, visualize data, feature engineering, modeling and deployment.</span></span> <span data-ttu-id="fda28-258">Ez a cikk megmutatjuk, ebben az esetben a hello modellezési és a központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="fda28-258">In this article, we show just hello modeling and deployment.</span></span> 

### <a name="import-python-libraries"></a><span data-ttu-id="fda28-259">Python könyvtárak importálása</span><span class="sxs-lookup"><span data-stu-id="fda28-259">Import Python libraries</span></span>
<span data-ttu-id="fda28-260">A sorrend toorun hello minta Jupyter Notebook vagy hello Python-parancsfájl, hello következő Python csomagok szükségesek.</span><span class="sxs-lookup"><span data-stu-id="fda28-260">In order toorun hello sample Jupyter Notebook or hello Python script file, hello following Python packages are needed.</span></span> <span data-ttu-id="fda28-261">Hello AzureML Notebook szolgáltatást használja, ha ezeket a csomagokat előre telepített törölték.</span><span class="sxs-lookup"><span data-stu-id="fda28-261">If you are using hello AzureML Notebook service, these packages have been pre-installed.</span></span>

    import pandas as pd
    from pandas import Series, DataFrame
    import numpy as np
    import matplotlib.pyplot as plt
    from time import time
    import pyodbc
    import os
    from azure.storage.blob import BlobService
    import tables
    import time
    import zipfile
    import random
    import sklearn
    from sklearn.linear_model import LogisticRegression
    from sklearn.cross_validation import train_test_split
    from sklearn import metrics
    from __future__ import division
    from sklearn import linear_model
    from azureml import services


### <a name="read-in-hello-data-from-blob"></a><span data-ttu-id="fda28-262">A blob adatait hello olvasása</span><span class="sxs-lookup"><span data-stu-id="fda28-262">Read in hello data from blob</span></span>
* <span data-ttu-id="fda28-263">Kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="fda28-263">Connection String</span></span>   
  
        CONTAINERNAME = 'test1'
        STORAGEACCOUNTNAME = 'XXXXXXXXX'
        STORAGEACCOUNTKEY = 'YYYYYYYYYYYYYYYYYYYYYYYYYYYY'
        BLOBNAME = 'demo_ex_9_stratified_1_1000_copy.csv'
        blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
* <span data-ttu-id="fda28-264">Szövegként olvasása</span><span class="sxs-lookup"><span data-stu-id="fda28-264">Read in as text</span></span>
  
        t1 = time.time()
        data = blob_service.get_blob_to_text(CONTAINERNAME,BLOBNAME).split("\n")
        t2 = time.time()
        print(("It takes %s seconds tooread in "+BLOBNAME) % (t2 - t1))
  
  ![17](./media/machine-learning-data-science-process-data-lake-walkthrough/17-python_readin_csv.PNG)    
* <span data-ttu-id="fda28-266">Oszlop neveket adhat hozzá és különálló oszlopok</span><span class="sxs-lookup"><span data-stu-id="fda28-266">Add column names and separate columns</span></span>
  
        colnames = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime',
        'passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'payment_type', 'fare_amount', 'surcharge', 'mta_tax', 'tolls_amount',  'total_amount', 'tip_amount', 'tipped', 'tip_class', 'rownum']
        df1 = pd.DataFrame([sub.split(",") for sub in data], columns = colnames)
* <span data-ttu-id="fda28-267">Egyes oszlopok toonumeric módosítása</span><span class="sxs-lookup"><span data-stu-id="fda28-267">Change some columns toonumeric</span></span>
  
        cols_2_float = ['trip_time_in_secs','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'fare_amount', 'surcharge','mta_tax','tolls_amount','total_amount','tip_amount', 'passenger_count','trip_distance'
        ,'tipped','tip_class','rownum']
        for col in cols_2_float:
            df1[col] = df1[col].astype(float)

### <a name="build-machine-learning-models"></a><span data-ttu-id="fda28-268">Machine learning modellek létrehozása</span><span class="sxs-lookup"><span data-stu-id="fda28-268">Build machine learning models</span></span>
<span data-ttu-id="fda28-269">Itt azt hozzon létre egy bináris osztályozási modell toopredict, hogy egy út Formabontó van, vagy nem.</span><span class="sxs-lookup"><span data-stu-id="fda28-269">Here we build a binary classification model toopredict whether a trip is tipped or not.</span></span> <span data-ttu-id="fda28-270">A Jupyter Notebook hello található egyéb két modell: multiclass besorolási és regressziós modell.</span><span class="sxs-lookup"><span data-stu-id="fda28-270">In hello Jupyter Notebook you can find other two models: multiclass classification, and regression models.</span></span>

* <span data-ttu-id="fda28-271">Először igazolnia kell toocreate dummy változók scikit használt-modellek tudnivalók</span><span class="sxs-lookup"><span data-stu-id="fda28-271">First we need toocreate dummy variables that can be used in scikit-learn models</span></span>
  
        df1_payment_type_dummy = pd.get_dummies(df1['payment_type'], prefix='payment_type_dummy')
        df1_vendor_id_dummy = pd.get_dummies(df1['vendor_id'], prefix='vendor_id_dummy')
* <span data-ttu-id="fda28-272">Adatok kerete hello modellezési létrehozása</span><span class="sxs-lookup"><span data-stu-id="fda28-272">Create data frame for hello modeling</span></span>
  
        cols_to_keep = ['tipped', 'trip_distance', 'passenger_count']
        data = df1[cols_to_keep].join([df1_payment_type_dummy,df1_vendor_id_dummy])
  
        X = data.iloc[:,1:]
        Y = data.tipped
* <span data-ttu-id="fda28-273">Modell betanítására és tesztelésére 60-40 felosztása</span><span class="sxs-lookup"><span data-stu-id="fda28-273">Training and testing 60-40 split</span></span>
  
        X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.4, random_state=0)
* <span data-ttu-id="fda28-274">A gyakorlókészlethez logisztikai regresszió</span><span class="sxs-lookup"><span data-stu-id="fda28-274">Logistic Regression in training set</span></span>
  
        model = LogisticRegression()
        logit_fit = model.fit(X_train, Y_train)
        print ('Coefficients: \n', logit_fit.coef_)
        Y_train_pred = logit_fit.predict(X_train)
  
       ![c1](./media/machine-learning-data-science-process-data-lake-walkthrough/c1-py-logit-coefficient.PNG)
* <span data-ttu-id="fda28-275">Tesztelési adatkészlet pontozása</span><span class="sxs-lookup"><span data-stu-id="fda28-275">Score testing data set</span></span>
  
        Y_test_pred = logit_fit.predict(X_test)
* <span data-ttu-id="fda28-276">Értékelési-metrikák kiszámítása</span><span class="sxs-lookup"><span data-stu-id="fda28-276">Calculate Evaluation metrics</span></span>
  
        fpr_train, tpr_train, thresholds_train = metrics.roc_curve(Y_train, Y_train_pred)
        print fpr_train, tpr_train, thresholds_train
  
        fpr_test, tpr_test, thresholds_test = metrics.roc_curve(Y_test, Y_test_pred) 
        print fpr_test, tpr_test, thresholds_test
  
        #AUC
        print metrics.auc(fpr_train,tpr_train)
        print metrics.auc(fpr_test,tpr_test)
  
        #Confusion Matrix
        print metrics.confusion_matrix(Y_train,Y_train_pred)
        print metrics.confusion_matrix(Y_test,Y_test_pred)
  
       ![c2](./media/machine-learning-data-science-process-data-lake-walkthrough/c2-py-logit-evaluation.PNG)

### <a name="build-web-service-api-and-consume-it-in-python"></a><span data-ttu-id="fda28-277">Webszolgáltatási API létrehozása és a Python szokásokra is</span><span class="sxs-lookup"><span data-stu-id="fda28-277">Build Web Service API and consume it in Python</span></span>
<span data-ttu-id="fda28-278">Szeretnénk toooperationalize hello gépi tanulási modell, miután be lett építve.</span><span class="sxs-lookup"><span data-stu-id="fda28-278">We want toooperationalize hello machine learning model after it has been built.</span></span> <span data-ttu-id="fda28-279">Hello bináris logisztikai modell itt példaként használjuk.</span><span class="sxs-lookup"><span data-stu-id="fda28-279">Here we use hello binary logistic model as an example.</span></span> <span data-ttu-id="fda28-280">Győződjön meg arról, hogy hello scikit-további 0.15.1 verziója a helyi gépen.</span><span class="sxs-lookup"><span data-stu-id="fda28-280">Make sure hello scikit-learn version in your local machine is 0.15.1.</span></span> <span data-ttu-id="fda28-281">A tooworry nincs Azure ML studio szolgáltatás használatakor.</span><span class="sxs-lookup"><span data-stu-id="fda28-281">You don't have tooworry about this if you use Azure ML studio service.</span></span>

* <span data-ttu-id="fda28-282">A munkaterület hitelesítő adatait az Azure ML studio beállítások megkeresése</span><span class="sxs-lookup"><span data-stu-id="fda28-282">Find your workspace credentials from Azure ML studio settings.</span></span> <span data-ttu-id="fda28-283">Az Azure Machine Learning Studióban, kattintson a **beállítások** --> **neve** --> **engedélyezési jogkivonatok**.</span><span class="sxs-lookup"><span data-stu-id="fda28-283">In Azure Machine Learning Studio, click **Settings** --> **Name** --> **Authorization Tokens**.</span></span> 
  
    ![C3](./media/machine-learning-data-science-process-data-lake-walkthrough/c3-workspace-id.PNG)

        workspaceid = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'
        auth_token = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'

* <span data-ttu-id="fda28-285">Hozzon létre a webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="fda28-285">Create Web Service</span></span>
  
        @services.publish(workspaceid, auth_token) 
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float, payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(int) #0, or 1
        def predictNYCTAXI(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            inputArray = [trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH, payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS]
            return logit_fit.predict(inputArray)
* <span data-ttu-id="fda28-286">Webes szolgáltatás hitelesítő adatainak lekérése</span><span class="sxs-lookup"><span data-stu-id="fda28-286">Get web service credentials</span></span>
  
        url = predictNYCTAXI.service.url
        api_key =  predictNYCTAXI.service.api_key
  
        print url
        print api_key
  
        @services.service(url, api_key)
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float,payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(float)
        def NYCTAXIPredictor(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            pass
* <span data-ttu-id="fda28-287">Webszolgáltatási API hívása.</span><span class="sxs-lookup"><span data-stu-id="fda28-287">Call Web service API.</span></span> <span data-ttu-id="fda28-288">Hogy toowait hello előző lépés után 5-10 másodperc.</span><span class="sxs-lookup"><span data-stu-id="fda28-288">You have toowait 5-10 seconds after hello previous step.</span></span>
  
        NYCTAXIPredictor(1,2,1,0,0,0,0,0,1)
  
       ![c4](./media/machine-learning-data-science-process-data-lake-walkthrough/c4-call-API.PNG)

## <a name="option-2-create-and-deploy-models-directly-in-azure-machine-learning"></a><span data-ttu-id="fda28-289">2. lehetőség: Hozzon létre, és közvetlenül az Azure Machine Learning modellek telepítése</span><span class="sxs-lookup"><span data-stu-id="fda28-289">Option 2: Create and deploy models directly in Azure Machine Learning</span></span>
<span data-ttu-id="fda28-290">Az Azure Machine Learning Studio képes adatokat közvetlenül olvassák be az Azure Data Lake Store majd használt toocreate kell és modellek.</span><span class="sxs-lookup"><span data-stu-id="fda28-290">Azure Machine Learning Studio can read data directly from Azure Data Lake Store and then be used toocreate and deploy models.</span></span> <span data-ttu-id="fda28-291">Ez a módszer olyan Hive táblát, amely a hello Azure Data Lake Store használja.</span><span class="sxs-lookup"><span data-stu-id="fda28-291">This approach uses a Hive table that points at hello Azure Data Lake Store.</span></span> <span data-ttu-id="fda28-292">Ehhez szükséges, hogy egy külön Azure HDInsight-fürt üzembe, mely hello Hive tábla létre van hozva.</span><span class="sxs-lookup"><span data-stu-id="fda28-292">This requires that a separate Azure HDInsight cluster be provisioned, on which hello Hive table is created.</span></span> <span data-ttu-id="fda28-293">a következő szakaszok megjelenítése hogyan hello toodo ez.</span><span class="sxs-lookup"><span data-stu-id="fda28-293">hello following sections show how toodo this.</span></span> 

### <a name="create-an-hdinsight-linux-cluster"></a><span data-ttu-id="fda28-294">HDInsight Linux-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="fda28-294">Create an HDInsight Linux Cluster</span></span>
<span data-ttu-id="fda28-295">HDInsight-fürtök (Linux) készíteni hello [Azure Portal](http://portal.azure.com). További információkért lásd: hello **HDInsight-fürtök létrehozása a hozzáférés tooAzure Data Lake Store** szakasz [HDInsight-fürtök létrehozása az Azure portál használatával a Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="fda28-295">Create an HDInsight Cluster (Linux) from hello [Azure Portal](http://portal.azure.com).For details, see hello **Create an HDInsight cluster with access tooAzure Data Lake Store** section in [Create an HDInsight cluster with Data Lake Store using Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

 ![18](./media/machine-learning-data-science-process-data-lake-walkthrough/18-create_HDI_cluster.PNG)

### <a name="create-hive-table-in-hdinsight"></a><span data-ttu-id="fda28-297">Hdinsight Hive tábla létrehozásához</span><span class="sxs-lookup"><span data-stu-id="fda28-297">Create Hive table in HDInsight</span></span>
<span data-ttu-id="fda28-298">Most már tudjuk létrehozni a Hive táblák toobe hello HDInsight-fürtjéhez hello adataihoz az Azure Data Lake Store hello előző lépésben az Azure Machine Learning Studióban használt.</span><span class="sxs-lookup"><span data-stu-id="fda28-298">Now we create Hive tables toobe used in Azure Machine Learning Studio in hello HDInsight cluster using hello data stored in Azure Data Lake Store in hello previous step.</span></span> <span data-ttu-id="fda28-299">Nyissa meg toohello most hozott létre HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="fda28-299">Go toohello HDInsight cluster just created.</span></span> <span data-ttu-id="fda28-300">Kattintson a **beállítások** --> **tulajdonságok** --> **fürt AAD-identitása** --> **ADLS-hozzáférés**, Ellenőrizze, hogy az Azure Data Lake Store-fiók fel van véve hello lista olvasási, írási és végrehajtási jogokat.</span><span class="sxs-lookup"><span data-stu-id="fda28-300">Click **Settings** --> **Properties** --> **Cluster AAD Identity** --> **ADLS Access**, make sure your Azure Data Lake Store account is added in hello list with read, write and execute rights.</span></span> 

 ![19](./media/machine-learning-data-science-process-data-lake-walkthrough/19-HDI-cluster-add-ADLS.PNG)

<span data-ttu-id="fda28-302">Kattintson a **irányítópult** következő toohello **beállítások** gombra, és egy ablakban jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="fda28-302">Then click **Dashboard** next toohello **Settings** button and a window will pop up.</span></span> <span data-ttu-id="fda28-303">Kattintson a **Hive View** hello jobb felső sarkában hello lapot, és a következő látható: hello **Lekérdezésszerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="fda28-303">Click **Hive View** in hello upper right corner of hello page and you will see hello **Query Editor**.</span></span>

 ![20](./media/machine-learning-data-science-process-data-lake-walkthrough/20-HDI-dashboard.PNG)

 ![21](./media/machine-learning-data-science-process-data-lake-walkthrough/21-Hive-Query-Editor-v2.PNG)

<span data-ttu-id="fda28-306">Illessze be a következő Hive parancsfájlok toocreate hello egy tábla.</span><span class="sxs-lookup"><span data-stu-id="fda28-306">Paste in hello following Hive scripts toocreate a table.</span></span> <span data-ttu-id="fda28-307">hello adatforrás helye a ily módon az Azure Data Lake Store-dokumentáció: **adl://data_lake_store_name.azuredatalakestore.net:443/mappanév/Fájlnév**.</span><span class="sxs-lookup"><span data-stu-id="fda28-307">hello location of data source is in Azure Data Lake Store reference in this way: **adl://data_lake_store_name.azuredatalakestore.net:443/folder_name/file_name**.</span></span>

    CREATE EXTERNAL TABLE nyc_stratified_sample
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string,
      payment_type string,
      fare_amount string,
      surcharge string,
      mta_tax string,
      tolls_amount string,
      total_amount string,
      tip_amount string,
      tipped string,
      tip_class string,
      rownum string
      )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    LOCATION 'adl://data_lake_storage_name.azuredatalakestore.net:443/nyctaxi_folder/demo_ex_9_stratified_1_1000_copy.csv';


<span data-ttu-id="fda28-308">Hello lekérdezés végeztével ilyen hello eredmény jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="fda28-308">When hello query finishes running, you will see hello results like this:</span></span>

 ![22](./media/machine-learning-data-science-process-data-lake-walkthrough/22-Hive-Query-results.PNG)

### <a name="build-and-deploy-models-in-azure-machine-learning-studio"></a><span data-ttu-id="fda28-310">Hozza létre és telepítheti az Azure Machine Learning Studióban modellek</span><span class="sxs-lookup"><span data-stu-id="fda28-310">Build and deploy models in Azure Machine Learning Studio</span></span>
<span data-ttu-id="fda28-311">Azt most már készen áll a toobuild, és telepítsék a modell, amely képes-e a tipp van fizetős Azure Machine Learning segítségével.</span><span class="sxs-lookup"><span data-stu-id="fda28-311">We are now ready toobuild and deploy a model that predicts whether or not a tip is paid with Azure Machine Learning.</span></span> <span data-ttu-id="fda28-312">hello rétegzett mintaadatok kész, a bináris osztályozási használt toobe (tipp vagy sem) a probléma.</span><span class="sxs-lookup"><span data-stu-id="fda28-312">hello stratified sample data is ready toobe used in this binary classification (tip or not) problem.</span></span> <span data-ttu-id="fda28-313">hello multiclass besorolásával (tip_class) prediktív modellek és regressziós (tip_amount) is a beépített és az Azure Machine Learning Studio telepített, de itt csak megmutatjuk, hogyan hello bináris osztályozási modell toohandle hello nagybetűk használata.</span><span class="sxs-lookup"><span data-stu-id="fda28-313">hello predictive models using multiclass classification (tip_class) and regression (tip_amount) can also be built and deployed with Azure Machine Learning Studio, but here we only show how toohandle hello case using hello binary classification model.</span></span>

1. <span data-ttu-id="fda28-314">Hello adatok beolvasása az Azure ml hello segítségével **és adatokat importálhat** modul hello elérhető **bemeneti és kimeneti** szakasz.</span><span class="sxs-lookup"><span data-stu-id="fda28-314">Get hello data into Azure ML using hello **Import Data** module, available in hello **Data Input and Output** section.</span></span> <span data-ttu-id="fda28-315">További információkért lásd: hello [és adatokat importálhat modul](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) referencialapja.</span><span class="sxs-lookup"><span data-stu-id="fda28-315">For more information, see hello [Import Data module](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) reference page.</span></span>
2. <span data-ttu-id="fda28-316">Válassza ki **Hive-lekérdezések** , hello **adatforrás** a hello **tulajdonságok** panel.</span><span class="sxs-lookup"><span data-stu-id="fda28-316">Select **Hive Query** as hello **Data source** in hello **Properties** panel.</span></span>
3. <span data-ttu-id="fda28-317">A következő Hive parancsfájl hello Beillesztés hello **adatbázis-lekérdezés Hive** szerkesztő</span><span class="sxs-lookup"><span data-stu-id="fda28-317">Paste hello following Hive script in hello **Hive database query** editor</span></span>
   
        select * from nyc_stratified_sample;
4. <span data-ttu-id="fda28-318">Adja meg a hello URI a HDInsight-fürt (Ez található az Azure portálon), a Hadoop hitelesítő adatokat, a hely a kimeneti adatok és az Azure storage fiók nevét vagy kulcstároló neve.</span><span class="sxs-lookup"><span data-stu-id="fda28-318">Enter hello URI of HDInsight cluster (this can be found in Azure Portal), Hadoop credentials, location of output data, and Azure storage account name/key/container name.</span></span>
   
   ![23](./media/machine-learning-data-science-process-data-lake-walkthrough/23-reader-module-v3.PNG)  

<span data-ttu-id="fda28-320">Egy bináris osztályozási kísérletet, adatok beolvasása a Hive tábla példa hello az alábbi ábra az.</span><span class="sxs-lookup"><span data-stu-id="fda28-320">An example of a binary classification experiment reading data from Hive table is shown in hello figure below.</span></span>

 ![24](./media/machine-learning-data-science-process-data-lake-walkthrough/24-AML-exp.PNG)

<span data-ttu-id="fda28-322">Hello kísérlet létrehozása után kattintson **webes szolgáltatások beállítása** --> **prediktív webszolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="fda28-322">After hello experiment is created, click  **Set Up Web Service** --> **Predictive Web Service**</span></span>

 ![25](./media/machine-learning-data-science-process-data-lake-walkthrough/25-AML-exp-deploy.PNG)

<span data-ttu-id="fda28-324">Automatikusan létrehozott futtatási hello pontozási kísérlet, a Befejezés után kattintson **webes szolgáltatás telepítése**</span><span class="sxs-lookup"><span data-stu-id="fda28-324">Run hello automatically created scoring experiment, when it finishes, click **Deploy Web Service**</span></span>

 ![26](./media/machine-learning-data-science-process-data-lake-walkthrough/26-AML-exp-deploy-web.PNG)

<span data-ttu-id="fda28-326">hello webes szolgáltatás irányítópultját hamarosan fog megjelenni:</span><span class="sxs-lookup"><span data-stu-id="fda28-326">hello web service dashboard will be displayed shortly:</span></span>

 ![27](./media/machine-learning-data-science-process-data-lake-walkthrough/27-AML-web-api.PNG)

## <a name="summary"></a><span data-ttu-id="fda28-328">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="fda28-328">Summary</span></span>
<span data-ttu-id="fda28-329">Ez a forgatókönyv végrehajtásával létrehozott egy adatok tudományos környezet méretezhető végpontok közötti megoldások Azure Data Lake készítéséhez.</span><span class="sxs-lookup"><span data-stu-id="fda28-329">By completing this walkthrough you have created a data science environment for building scalable end-to-end solutions in Azure Data Lake.</span></span> <span data-ttu-id="fda28-330">Ebben a környezetben használt tooanalyze véve a hello kanonikus lépéseit az adatgyűjtést keresztül modell betanítási adatok tudományos folyamat, hello a nagy nyilvános adatbázisból lett, és hogy hello toohello telepítését majd modell webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="fda28-330">This environment was used tooanalyze a large public dataset, taking it through hello canonical steps of hello Data Science Process, from data acquisition through model training, and then toohello deployment of hello model as a web service.</span></span> <span data-ttu-id="fda28-331">U-SQL használt tooprocess volt, és fel az hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="fda28-331">U-SQL was used tooprocess, explore and sample hello data.</span></span> <span data-ttu-id="fda28-332">Python és a Hive az Azure Machine Learning Studio toobuild alkalmazott, és üzembe prediktív modelleket.</span><span class="sxs-lookup"><span data-stu-id="fda28-332">Python and Hive were used with Azure Machine Learning Studio toobuild and deploy predictive models.</span></span>

## <a name="whats-next"></a><span data-ttu-id="fda28-333">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="fda28-333">What's next?</span></span>
<span data-ttu-id="fda28-334">a képzési hello a [Team adatok tudományos folyamat (TDSP)](http://aka.ms/datascienceprocess) leíró egyes hivatkozások tootopics advanced analytics folyamat hello lépést biztosítja.</span><span class="sxs-lookup"><span data-stu-id="fda28-334">hello learning path for the [Team Data Science Process (TDSP)](http://aka.ms/datascienceprocess) provides links tootopics describing each step in hello advanced analytics process.</span></span> <span data-ttu-id="fda28-335">Nincsenek felsorolva hello a forgatókönyvek több [Team adatok tudományos folyamat forgatókönyvek](data-science-process-walkthroughs.md) hogyan lapon adott showcase toouse erőforrások és szolgáltatások a prediktív elemzés különféle helyzetekben:</span><span class="sxs-lookup"><span data-stu-id="fda28-335">There are a series of walkthroughs itemized on hello [Team Data Science Process walkthroughs](data-science-process-walkthroughs.md) page that showcase how toouse resources and services in various predictive analytics scenarios:</span></span>

* [<span data-ttu-id="fda28-336">hello Team adatok tudományos folyamat működés közben: az SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="fda28-336">hello Team Data Science Process in action: using SQL Data Warehouse</span></span>](machine-learning-data-science-process-sqldw-walkthrough.md)
* [<span data-ttu-id="fda28-337">hello Team adatok tudományos folyamat működés közben: HDInsight Hadoop-fürtök használata</span><span class="sxs-lookup"><span data-stu-id="fda28-337">hello Team Data Science Process in action: using HDInsight Hadoop clusters</span></span>](machine-learning-data-science-process-hive-walkthrough.md)
* [<span data-ttu-id="fda28-338">hello csapat az tudományos folyamata: SQL Server használata</span><span class="sxs-lookup"><span data-stu-id="fda28-338">hello Team Data Science Process: using SQL Server</span></span>](machine-learning-data-science-process-sql-walkthrough.md)
* [<span data-ttu-id="fda28-339">Az Azure HDInsight Spark hello adatok tudományos folyamat használatának áttekintése</span><span class="sxs-lookup"><span data-stu-id="fda28-339">Overview of hello Data Science Process using Spark on Azure HDInsight</span></span>](machine-learning-data-science-spark-overview.md)

