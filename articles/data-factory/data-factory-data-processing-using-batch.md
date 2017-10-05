---
title: "Nagy méretű adatkészletekhez adat-előállító és kötegelt feldolgozni |} Microsoft Docs"
description: "Ismerteti, hogyan lehet feldolgozni egy Azure Data Factory-folyamat túl nagy adatmennyiségek Azure kötegelt párhuzamos feldolgozási képességének használatával."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 688b964b-51d0-4faa-91a7-26c7e3150868
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: spelluru
ms.openlocfilehash: 9defbf7a6a515740fa3b3cb1c67a2f5f9d9baa01
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="process-large-scale-datasets-using-data-factory-and-batch"></a><span data-ttu-id="3c4b6-103">Nagyméretű adatkészletek feldolgozása a Data Factory és a Batch használatával</span><span class="sxs-lookup"><span data-stu-id="3c4b6-103">Process large-scale datasets using Data Factory and Batch</span></span>
<span data-ttu-id="3c4b6-104">Ez a cikk ismerteti a minta megoldás, amely helyezi át, és feldolgozza a nagy méretű adatkészletek automatikus és ütemezett módon architektúrát.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-104">This article describes an architecture of a sample solution that moves and processes large-scale datasets in an automatic and scheduled manner.</span></span> <span data-ttu-id="3c4b6-105">Egy végpont forgatókönyv használatával az Azure Data Factory és az Azure Batch megoldás megvalósításának is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-105">It also provides an end-to-end walkthrough to implement the solution using Azure Data Factory and Azure Batch.</span></span>

<span data-ttu-id="3c4b6-106">Ez a cikk nem hosszabb, mint a szokásos cikk, mert a forgatókönyv egy teljes mintát megoldás tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-106">This article is longer than our typical article because it contains a walkthrough of an entire sample solution.</span></span> <span data-ttu-id="3c4b6-107">Ha még nem ismeri a kötegelt és a Data Factory megismerheti ezeket a szolgáltatásokat, és hogyan működnek együtt.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-107">If you are new to Batch and Data Factory, you can learn about these services and how they work together.</span></span> <span data-ttu-id="3c4b6-108">Ha tudja, valamit a szolgáltatásokkal kapcsolatos, és vannak designing/újratervezni a megoldás, akkor előfordulhat, hogy összpontosítson, csak a a [architektúra szakasz](#architecture-of-sample-solution) a cikk egy prototípus vagy megoldás fejlesztői, is érdemes lehet próbálja ki a részletes utasításokat és a [forgatókönyv](#implementation-of-sample-solution).</span><span class="sxs-lookup"><span data-stu-id="3c4b6-108">If you know something about the services and are designing/architecting a solution, you may focus just on the [architecture section](#architecture-of-sample-solution) of the article and if you are developing a prototype or a solution, you may also want to try out step-by-step instructions in the [walkthrough](#implementation-of-sample-solution).</span></span> <span data-ttu-id="3c4b6-109">A Microsoft hívhat meg a tartalmat, és használatának kapcsolatos megjegyzéseit.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-109">We invite your comments about this content and how you use it.</span></span>

<span data-ttu-id="3c4b6-110">Először hogyan adat-előállító és a Batch szolgáltatás segítik a felhőben nagy adatkészletek feldolgozással vizsgáljuk meg.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-110">First, let's look at how Data Factory and Batch services can help with processing large datasets in the cloud.</span></span>     

## <a name="why-azure-batch"></a><span data-ttu-id="3c4b6-111">Miért érdemes az Azure Batch?</span><span class="sxs-lookup"><span data-stu-id="3c4b6-111">Why Azure Batch?</span></span>
<span data-ttu-id="3c4b6-112">Az Azure Batch lehetővé teszi, hogy hatékonyan futtasson nagyméretű párhuzamos és nagy teljesítményű feldolgozási (HPC) alkalmazásokat a felhőben.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-112">Azure Batch enables you to run large-scale parallel and high-performance computing (HPC) applications efficiently in the cloud.</span></span> <span data-ttu-id="3c4b6-113">A platformszolgáltatás számításigényes munkák futtatását ütemezi virtuális gépek felügyelt gyűjteményében, és automatikusan képes méretezni a számítási erőforrásokat a feladatok igényeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-113">It's a platform service that schedules compute-intensive work to run on a managed collection of virtual machines, and can automatically scale compute resources to meet the needs of your jobs.</span></span>

<span data-ttu-id="3c4b6-114">A Batch szolgáltatással Azure számítási erőforrásokat határoz meg az alkalmazások párhuzamos és méretezhető futtatásához.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-114">With the Batch service, you define Azure compute resources to execute your applications in parallel, and at scale.</span></span> <span data-ttu-id="3c4b6-115">Igény szerinti vagy ütemezett feladatokat futtathat, és nem kell manuálisan létrehoznia, konfigurálnia és felügyelnie a HPC-fürtöket, az egyes virtuális gépeket, virtuális hálózatokat vagy az összetett feladat- és tevékenységütemezési infrastruktúrát.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-115">You can run on-demand or scheduled jobs, and you don't need to manually create, configure, and manage an HPC cluster, individual virtual machines, virtual networks, or a complex job and task scheduling infrastructure.</span></span>

<span data-ttu-id="3c4b6-116">Lásd az alábbi cikkeket, ha nem ismeri az Azure Batch, segít megérteni a megoldás, a cikkben leírt architektúra/végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-116">See the following articles if you are not familiar with Azure Batch as it helps with understanding the architecture/implementation of the solution described in this article.</span></span>   

* [<span data-ttu-id="3c4b6-117">Az Azure Batch alapjai</span><span class="sxs-lookup"><span data-stu-id="3c4b6-117">Basics of Azure Batch</span></span>](../batch/batch-technical-overview.md)
* [<span data-ttu-id="3c4b6-118">A Batch funkcióinak áttekintése</span><span class="sxs-lookup"><span data-stu-id="3c4b6-118">Batch feature overview</span></span>](../batch/batch-api-basics.md)

<span data-ttu-id="3c4b6-119">(választható) Azure Batch kapcsolatos további tudnivalókért tekintse meg a [Azure Batch képzési terv](https://azure.microsoft.com/documentation/learning-paths/batch/).</span><span class="sxs-lookup"><span data-stu-id="3c4b6-119">(optional) To learn more about Azure Batch, see the [Learning path for Azure Batch](https://azure.microsoft.com/documentation/learning-paths/batch/).</span></span>

## <a name="why-azure-data-factory"></a><span data-ttu-id="3c4b6-120">Miért érdemes az Azure Data Factory?</span><span class="sxs-lookup"><span data-stu-id="3c4b6-120">Why Azure Data Factory?</span></span>
<span data-ttu-id="3c4b6-121">A Data Factory egy felhőalapú adatintegrációs szolgáltatás, amellyel előkészíthető és automatizálható az adatok továbbítása és átalakítása.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-121">Data Factory is a cloud-based data integration service that orchestrates and automates the movement and transformation of data.</span></span> <span data-ttu-id="3c4b6-122">A Data Factory szolgáltatással kezelt adatok folyamatok, amelyek tárolt adatok mozgatása a helyszíni és felhőalapú adattároló központosított adattárolóhoz hozhat létre (például: Azure Blob Storage), és a folyamat vagy átalakítási adatok szolgáltatásokat, például Azure HDInsight és az Azure Machine Learning használatával.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-122">Using the Data Factory service, you can create managed data pipelines that move data from on-premises and cloud data stores to a centralized data store (for example: Azure Blob Storage), and process/transform data using services such as Azure HDInsight and Azure Machine Learning.</span></span> <span data-ttu-id="3c4b6-123">Beütemezhet adatok folyamatok ütemezett módon (óránkénti, napi, heti, stb.) és a figyelő futtatásához, és kezelheti azokat egy pillanat alatt azonosíthatja a problémákat, és hajtsa végre a műveletet.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-123">You can also schedule data pipelines to run in a scheduled manner (hourly, daily, weekly, etc.) and monitor and manage them at a glance to identify issues and take action.</span></span>

<span data-ttu-id="3c4b6-124">Lásd az alábbi cikkeket, ha nem ismeri az Azure Data Factoryvel, segít megérteni a megoldás, a cikkben leírt architektúra/végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-124">See the following articles if you are not familiar with Azure Data Factory as it helps with understanding the architecture/implementation of the solution described in this article.</span></span>  

* [<span data-ttu-id="3c4b6-125">Az Azure Data Factory bevezetése</span><span class="sxs-lookup"><span data-stu-id="3c4b6-125">Introduction of Azure Data Factory</span></span>](data-factory-introduction.md)
* [<span data-ttu-id="3c4b6-126">Felépítheti első folyamatát adatok</span><span class="sxs-lookup"><span data-stu-id="3c4b6-126">Build your first data pipeline</span></span>](data-factory-build-your-first-pipeline.md)   

<span data-ttu-id="3c4b6-127">(választható) Azure Data Factory kapcsolatos további tudnivalókért tekintse meg a [az Azure Data Factory képzési terv](https://azure.microsoft.com/documentation/learning-paths/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="3c4b6-127">(optional) To learn more about Azure Data Factory, see the [Learning path for Azure Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/).</span></span>

## <a name="data-factory-and-batch-together"></a><span data-ttu-id="3c4b6-128">Adat-előállító és kötegelt együtt</span><span class="sxs-lookup"><span data-stu-id="3c4b6-128">Data Factory and Batch together</span></span>
<span data-ttu-id="3c4b6-129">Adat-előállító beépített tevékenységeket magában foglalja például a másolási tevékenység során történő másolására/áthelyezésére adatok cél adattároló forrás adattároló és a Hadoop-fürtök (HDInsight) használata az Azure-on adatfeldolgozásra történő Hive tevékenység.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-129">Data Factory includes built-in activities such as Copy Activity to copy/move data from a source data store to a destination data store and Hive Activity to process data using Hadoop clusters (HDInsight) on Azure.</span></span> <span data-ttu-id="3c4b6-130">Lásd: [adatok átalakítása tevékenységek](data-factory-data-transformation-activities.md) támogatott átalakítása tevékenységek listáját.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-130">See [Data Transformation Activities](data-factory-data-transformation-activities.md) for a list of supported transformation activities.</span></span>

<span data-ttu-id="3c4b6-131">Lehetővé teszi egyéni .NET tevékenység áthelyezése vagy dolgoz fel adatokat a saját logikával, és ezek a tevékenységek futtatásához Azure HDInsight-fürtöt, vagy a virtuális gépek Azure Batch-készlet létrehozását.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-131">It also allows you to create custom .NET activities to move or process data with your own logic and run these activities on an Azure HDInsight cluster or on an Azure Batch pool of VMs.</span></span> <span data-ttu-id="3c4b6-132">Azure Batch használatakor konfigurálhatja az automatikus méretezése a készletet (hozzáadása vagy eltávolítása a virtuális gépek, a munkaterhelés alapján), adja meg a képlet alapján.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-132">When you use Azure Batch, you can configure the pool to auto-scale (add or remove VMs based on the workload) based on a formula you provide.</span></span>     

## <a name="architecture-of-sample-solution"></a><span data-ttu-id="3c4b6-133">A minta megoldás architektúrája</span><span class="sxs-lookup"><span data-stu-id="3c4b6-133">Architecture of sample solution</span></span>
<span data-ttu-id="3c4b6-134">Annak ellenére, hogy ebben a cikkben leírt architektúra a legegyszerűbb megoldás az, az összetett forgatókönyvek esetében például a pénzügyi szolgáltatásokat, kép feldolgozási és megjelenítési és genomikus elemzés modellezési kockázat kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-134">Even though the architecture described in this article is for a simple solution, it is relevant to complex scenarios such as risk modeling by financial services, image processing and rendering, and genomic analysis.</span></span>

<span data-ttu-id="3c4b6-135">Az ábrán látható, 1) hogyan koordinálja a Data Factory adatmozgatást és a feldolgozás és a 2.) hogyan Azure Batch dolgozza fel az adatok párhuzamos módon.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-135">The diagram illustrates 1) how Data Factory orchestrates data movement and processing and 2) how Azure Batch processes the data in a parallel manner.</span></span> <span data-ttu-id="3c4b6-136">Letölthető és kinyomtatható a az egyszerűbb használat (11 x 17.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-136">Download and print the diagram for easy reference (11 x 17 in.</span></span> <span data-ttu-id="3c4b6-137">vagy A3 méret): [Azure Batch és a Data Factory használatával HPC és az orchestration](http://go.microsoft.com/fwlink/?LinkId=717686).</span><span class="sxs-lookup"><span data-stu-id="3c4b6-137">or A3 size): [HPC and data orchestration using Azure Batch and Data Factory](http://go.microsoft.com/fwlink/?LinkId=717686).</span></span>

<span data-ttu-id="3c4b6-138">[![A nagyméretű adatok feldolgozása diagramja](./media/data-factory-data-processing-using-batch/image1.png)](http://go.microsoft.com/fwlink/?LinkId=717686)</span><span class="sxs-lookup"><span data-stu-id="3c4b6-138">[![Large-scale data processing diagram](./media/data-factory-data-processing-using-batch/image1.png)](http://go.microsoft.com/fwlink/?LinkId=717686)</span></span>

<span data-ttu-id="3c4b6-139">Az alábbi lista ismerteti a folyamat alapvető lépéseit.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-139">The following list provides the basic steps of the process.</span></span> <span data-ttu-id="3c4b6-140">A megoldás tartalmaz a kódot és magyarázataik a végpont megoldás kiépítését.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-140">The solution includes code and explanations to build the end-to-end solution.</span></span>

1. <span data-ttu-id="3c4b6-141">**Konfigurálja az Azure Batch számítási csomópontok (VM) készletét**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-141">**Configure Azure Batch with a pool of compute nodes (VMs)**.</span></span> <span data-ttu-id="3c4b6-142">A csomópontok száma és mérete az egyes csomópontok is megadhat.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-142">You can specify the number of nodes and size of each node.</span></span>
2. <span data-ttu-id="3c4b6-143">**Hozzon létre egy Azure Data Factory példányt** , amelyek megfelelnek az Azure blob storage, Azure Batch számítási szolgáltatás, bemeneti adatokat és egy munkafolyamat/folyamat olyan tevékenységet, amely áthelyezése és az adatok átalakítása entitások van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-143">**Create an Azure Data Factory instance** that is configured with entities that represent Azure blob storage, Azure Batch compute service, input/output data, and a workflow/pipeline with activities that move and transform data.</span></span>
3. <span data-ttu-id="3c4b6-144">**Hozzon létre egy egyéni .NET tevékenység a Data Factory-folyamathoz**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-144">**Create a custom .NET activity in the Data Factory pipeline**.</span></span> <span data-ttu-id="3c4b6-145">A tevékenység a felhasználói kód, amely fut az Azure Batch-készlet.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-145">The activity is your user code that runs on the Azure Batch pool.</span></span>
4. <span data-ttu-id="3c4b6-146">**A bemeneti adatok nagy mennyiségű tárolót, mint az Azure-tárfiókba blobok**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-146">**Store large amounts of input data as blobs in Azure storage**.</span></span> <span data-ttu-id="3c4b6-147">Adatok osztóval időszeletekre logikai (általában idő).</span><span class="sxs-lookup"><span data-stu-id="3c4b6-147">Data is divided into logical slices (usually by time).</span></span>
5. <span data-ttu-id="3c4b6-148">**Adat-előállító másolja át a feldolgozott adatok párhuzamosan** a másodlagos helyre.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-148">**Data Factory copies data that is processed in parallel** to the secondary location.</span></span>
6. <span data-ttu-id="3c4b6-149">**Adat-előállító futtatja a kötegelt lefoglalta a készlet használatával egyéni tevékenységet**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-149">**Data Factory runs the custom activity using the pool allocated by Batch**.</span></span> <span data-ttu-id="3c4b6-150">Adat-előállító egyidejűleg futtatható tevékenységek.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-150">Data Factory can run activities concurrently.</span></span> <span data-ttu-id="3c4b6-151">Minden tevékenység adatok szelet dolgozza fel.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-151">Each activity processes a slice of data.</span></span> <span data-ttu-id="3c4b6-152">Az eredményeket az Azure storage tárolja.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-152">The results are stored in Azure storage.</span></span>
7. <span data-ttu-id="3c4b6-153">**Adat-előállító harmadik helyre helyezi át a végső eredmények**, vagy terjesztési keresztül egy alkalmazást, vagy más eszközökkel további feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-153">**Data Factory moves the final results to a third location**, either for distribution via an app, or for further processing by other tools.</span></span>

## <a name="implementation-of-sample-solution"></a><span data-ttu-id="3c4b6-154">A minta-megoldás megvalósítása</span><span class="sxs-lookup"><span data-stu-id="3c4b6-154">Implementation of sample solution</span></span>
<span data-ttu-id="3c4b6-155">A minta megoldás szándékosan egyszerű és bemutatják a Data Factory és kötegelt adatkészletek feldolgozni együtt használja.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-155">The sample solution is intentionally simple and is to show you how to use Data Factory and Batch together to process datasets.</span></span> <span data-ttu-id="3c4b6-156">A megoldás egyszerűen megszámlálása egy keresési kifejezés ("Microsoft") előfordulását a bemeneti fájlok idősor rendezve.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-156">The solution simply counts the number of occurrences of a search term (“Microsoft”) in input files organized in a time series.</span></span> <span data-ttu-id="3c4b6-157">A számát, hogy a kimeneti fájlok kiírja azt.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-157">It outputs the count to output files.</span></span>

<span data-ttu-id="3c4b6-158">**Idő**: Ha ismeri az Azure Data Factory és kötegelt alapjait, és végrehajtotta az alábbi előfeltételek, azt becsléséhez a megoldás 1 – 2 órát vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-158">**Time**: If you are familiar with basics of Azure, Data Factory, and Batch, and have completed the prerequisites listed below, we estimate this solution takes 1-2 hours to complete.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="3c4b6-159">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3c4b6-159">Prerequisites</span></span>
#### <a name="azure-subscription"></a><span data-ttu-id="3c4b6-160">Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="3c4b6-160">Azure subscription</span></span>
<span data-ttu-id="3c4b6-161">Ha nem rendelkezik Azure-előfizetéssel, létrehozhat egy ingyenes próbafiók néhány perc alatt.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-161">If you don't have an Azure subscription, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="3c4b6-162">Lásd: [ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3c4b6-162">See [Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

#### <a name="azure-storage-account"></a><span data-ttu-id="3c4b6-163">Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="3c4b6-163">Azure storage account</span></span>
<span data-ttu-id="3c4b6-164">Egy Azure storage-fiókot használ az adatok tárolása az oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-164">You use an Azure storage account for storing the data in this tutorial.</span></span> <span data-ttu-id="3c4b6-165">Ha egy Azure storage-fiók nem rendelkezik, tekintse meg [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="3c4b6-165">If you don't have an Azure storage account, see [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span></span> <span data-ttu-id="3c4b6-166">A megoldást használja a blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-166">The sample solution uses blob storage.</span></span>

#### <a name="azure-batch-account"></a><span data-ttu-id="3c4b6-167">Azure Batch-fiók</span><span class="sxs-lookup"><span data-stu-id="3c4b6-167">Azure Batch account</span></span>
<span data-ttu-id="3c4b6-168">Hozzon létre egy Azure Batch fiók használata a [Azure-portálon](http://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="3c4b6-168">Create an Azure Batch account using the [Azure portal](http://manage.windowsazure.com/).</span></span> <span data-ttu-id="3c4b6-169">Lásd: [létrehozása és kezelése az Azure Batch-fiók](../batch/batch-account-create-portal.md).</span><span class="sxs-lookup"><span data-stu-id="3c4b6-169">See [Create and manage an Azure Batch account](../batch/batch-account-create-portal.md).</span></span> <span data-ttu-id="3c4b6-170">Megjegyzés: az Azure Batch nevét és a fiók kulcsot.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-170">Note the Azure Batch account name and account key.</span></span> <span data-ttu-id="3c4b6-171">Is [New-AzureRmBatchAccount](https://msdn.microsoft.com/library/mt603749.aspx) parancsmaggal hozhat létre Azure Batch-fiók.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-171">You can also use [New-AzureRmBatchAccount](https://msdn.microsoft.com/library/mt603749.aspx) cmdlet to create an Azure Batch account.</span></span> <span data-ttu-id="3c4b6-172">Lásd: [Ismerkedés az Azure Batch PowerShell-parancsmagok](../batch/batch-powershell-cmdlets-get-started.md) részletes útmutató a parancsmag használatával.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-172">See [Get started with Azure Batch PowerShell cmdlets](../batch/batch-powershell-cmdlets-get-started.md) for detailed instructions on using this cmdlet.</span></span>

<span data-ttu-id="3c4b6-173">A minta megoldás adatfeldolgozásra történő Azure Batch (közvetve keresztül egy Azure Data Factory-folyamathoz) használ a számítási csomópontok (a virtuális gépek felügyelt gyűjteménye) készlete egy párhuzamos módon.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-173">The sample solution uses Azure Batch (indirectly via an Azure Data Factory pipeline) to process data in a parallel manner on a pool of compute nodes (a managed collection of virtual machines).</span></span>

#### <a name="azure-batch-pool-of-virtual-machines-vms"></a><span data-ttu-id="3c4b6-174">A virtuális gépek (VM) az Azure Batch-készlet</span><span class="sxs-lookup"><span data-stu-id="3c4b6-174">Azure Batch pool of virtual machines (VMs)</span></span>
<span data-ttu-id="3c4b6-175">Hozzon létre egy **Azure Batch-készlet** legalább 2-es számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-175">Create an **Azure Batch pool** with at least 2 compute nodes.</span></span>

1. <span data-ttu-id="3c4b6-176">Az a [Azure-portálon](https://portal.azure.com), kattintson a **Tallózás** a bal oldali menüben, majd kattintson a **Batch-fiókok**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-176">In the [Azure portal](https://portal.azure.com), click **Browse** in the left menu, and click **Batch Accounts**.</span></span>
2. <span data-ttu-id="3c4b6-177">Válassza ki az Azure Batch-fiók megnyitása a **Batch-fiók** panelen.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-177">Select your Azure Batch account to open the **Batch Account** blade.</span></span>
3. <span data-ttu-id="3c4b6-178">Kattintson a **készletek** csempére.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-178">Click **Pools** tile.</span></span>
4. <span data-ttu-id="3c4b6-179">Az a **készletek** panelen kattintson a Hozzáadás gombra az eszköztáron a készlet hozzáadása gombra.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-179">In the **Pools** blade, click Add button on the toolbar to add a pool.</span></span>
   1. <span data-ttu-id="3c4b6-180">Adja meg a készlet Azonosítóját (**alkalmazáskészlet-azonosító**).</span><span class="sxs-lookup"><span data-stu-id="3c4b6-180">Enter an ID for the pool (**Pool ID**).</span></span> <span data-ttu-id="3c4b6-181">Megjegyzés: a **a készlet Azonosítóját**; kell azt a Data Factory megoldás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-181">Note the **ID of the pool**; you need it when creating the Data Factory solution.</span></span>
   2. <span data-ttu-id="3c4b6-182">Adja meg **Windows Server 2012 R2** az operációsrendszer-családot beállítás.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-182">Specify **Windows Server 2012 R2** for the Operating System Family setting.</span></span>
   3. <span data-ttu-id="3c4b6-183">Válassza ki a **csomóponti tarifacsomagot**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-183">Select a **node pricing tier**.</span></span>
   4. <span data-ttu-id="3c4b6-184">Adja meg **2** , értékét a **cél dedikált** beállítást.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-184">Enter **2** as value for the **Target Dedicated** setting.</span></span>
   5. <span data-ttu-id="3c4b6-185">Adja meg **2** , értékét a **csomópontonkénti tevékenységek maximális** beállítást.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-185">Enter **2** as value for the **Max tasks per node** setting.</span></span>
   6. <span data-ttu-id="3c4b6-186">A készlet létrehozásához kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-186">Click **OK** to create the pool.</span></span>

#### <a name="azure-storage-explorer"></a><span data-ttu-id="3c4b6-187">Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="3c4b6-187">Azure Storage Explorer</span></span>
<span data-ttu-id="3c4b6-188">[Az Azure Storage Explorer 6 (eszköz)](https://azurestorageexplorer.codeplex.com/) vagy [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) (a ClumsyLeaf szoftver).</span><span class="sxs-lookup"><span data-stu-id="3c4b6-188">[Azure Storage Explorer 6 (tool)](https://azurestorageexplorer.codeplex.com/) or [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) (from ClumsyLeaf Software).</span></span> <span data-ttu-id="3c4b6-189">Ezek az eszközök használata Kérem, és módosítsa az adatokat az Azure Storage-projektek a felhőben üzemeltetett alkalmazások naplófájlokkal együtt.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-189">You use these tools for inspecting and altering the data in your Azure Storage projects including the logs of your cloud-hosted applications.</span></span>

1. <span data-ttu-id="3c4b6-190">Hozzon létre egy nevű tárolót **mycontainer** privát hozzáférést (nincs névtelen hozzáférés)</span><span class="sxs-lookup"><span data-stu-id="3c4b6-190">Create a container named **mycontainer** with private access (no anonymous access)</span></span>
2. <span data-ttu-id="3c4b6-191">Ha használ **CloudXplorer**, hozzon létre mappákban és almappáiban az alábbi szerkezettel:</span><span class="sxs-lookup"><span data-stu-id="3c4b6-191">If you are using **CloudXplorer**, create folders and subfolders with the following structure:</span></span>

   ![](./media/data-factory-data-processing-using-batch/image3.png)

   <span data-ttu-id="3c4b6-192">`Inputfolder`és `outputfolder` a legfelső szintű mappák `mycontainer`.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-192">`Inputfolder` and `outputfolder` are top-level folders in `mycontainer`.</span></span> <span data-ttu-id="3c4b6-193">A `inputfolder` almappái dátum-időbélyegeket (éééé-hh-nn-ÓÓ) rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-193">The `inputfolder` has subfolders with date-time stamps (YYYY-MM-DD-HH).</span></span>

   <span data-ttu-id="3c4b6-194">Ha használ **Azure Tártallózó**, a következő lépéssel kell névvel rendelkező fájlok feltöltése: `inputfolder/2015-11-16-00/file.txt`, `inputfolder/2015-11-16-01/file.txt` és így tovább.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-194">If you are using **Azure Storage Explorer**, in the next step, you need to upload files with names: `inputfolder/2015-11-16-00/file.txt`, `inputfolder/2015-11-16-01/file.txt` and so on.</span></span> <span data-ttu-id="3c4b6-195">Ez a lépés automatikusan a mappákat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-195">This step automatically creates the folders.</span></span>
3. <span data-ttu-id="3c4b6-196">Hozzon létre egy szövegfájlt **file.txt** a számítógépre, amelyen a kulcsszó tartalmú **Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-196">Create a text file **file.txt** on your machine with content that has the keyword **Microsoft**.</span></span> <span data-ttu-id="3c4b6-197">Például: "egyéni tevékenység Microsoft teszt egyéni tevékenység Microsoft teszt".</span><span class="sxs-lookup"><span data-stu-id="3c4b6-197">For example: “test custom activity Microsoft test custom activity Microsoft”.</span></span>
4. <span data-ttu-id="3c4b6-198">A fájl feltöltése az Azure blob storage a következő bemeneti mappákhoz.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-198">Upload the file to the following input folders in Azure blob storage.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image4.png)

   <span data-ttu-id="3c4b6-199">Ha használ **Azure Tártallózó**, a fájl feltöltése **file.txt** való **mycontainer**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-199">If you are using **Azure Storage Explorer**, upload the file **file.txt** to **mycontainer**.</span></span> <span data-ttu-id="3c4b6-200">Kattintson a **másolási** másolatot készít a blob az eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-200">Click **Copy** on the toolbar to create a copy of the blob.</span></span> <span data-ttu-id="3c4b6-201">Az a **másolási Blob** párbeszédpanelen módosítsa a **cél blob neve** való `inputfolder/2015-11-16-00/file.txt`.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-201">In the **Copy Blob** dialog box, change the **destination blob name** to `inputfolder/2015-11-16-00/file.txt`.</span></span> <span data-ttu-id="3c4b6-202">Ismételje meg ezt a lépést létrehozása `inputfolder/2015-11-16-01/file.txt`, `inputfolder/2015-11-16-02/file.txt`, `inputfolder/2015-11-16-03/file.txt`, `inputfolder/2015-11-16-04/file.txt` és így tovább.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-202">Repeat this step to create `inputfolder/2015-11-16-01/file.txt`, `inputfolder/2015-11-16-02/file.txt`, `inputfolder/2015-11-16-03/file.txt`, `inputfolder/2015-11-16-04/file.txt` and so on.</span></span> <span data-ttu-id="3c4b6-203">Ez a művelet automatikusan létrehozza a mappákat.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-203">This action automatically creates the folders.</span></span>
5. <span data-ttu-id="3c4b6-204">Hozzon létre egy másik tárolót: `customactivitycontainer`.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-204">Create another container named: `customactivitycontainer`.</span></span> <span data-ttu-id="3c4b6-205">Az egyéni tevékenység zip fájlt feltölteni a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-205">You upload the custom activity zip file to this container.</span></span>

#### <a name="visual-studio"></a><span data-ttu-id="3c4b6-206">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c4b6-206">Visual Studio</span></span>
<span data-ttu-id="3c4b6-207">Telepítse a Microsoft Visual Studio 2012 vagy újabb a egyéni kötegelt tevékenység használható a Data Factory megoldás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-207">Install Microsoft Visual Studio 2012 or later to create the custom Batch activity to be used in the Data Factory solution.</span></span>

### <a name="high-level-steps-to-create-the-solution"></a><span data-ttu-id="3c4b6-208">Magas szintű lépései a megoldás létrehozása</span><span class="sxs-lookup"><span data-stu-id="3c4b6-208">High-level steps to create the solution</span></span>
1. <span data-ttu-id="3c4b6-209">Hozzon létre egy egyéni tevékenység, amely az adatok feldolgozása logikáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-209">Create a custom activity that contains the data processing logic.</span></span>
2. <span data-ttu-id="3c4b6-210">Hozzon létre egy Azure data factory használja az egyéni tevékenység:</span><span class="sxs-lookup"><span data-stu-id="3c4b6-210">Create an Azure data factory that uses the custom activity:</span></span>

### <a name="create-the-custom-activity"></a><span data-ttu-id="3c4b6-211">Az egyéni tevékenység létrehozása</span><span class="sxs-lookup"><span data-stu-id="3c4b6-211">Create the custom activity</span></span>
<span data-ttu-id="3c4b6-212">A Data Factory egyéni tevékenység Ez a minta-megoldás kulcsfontosságú.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-212">The Data Factory custom activity is the heart of this sample solution.</span></span> <span data-ttu-id="3c4b6-213">A minta megoldás Azure Batch az egyéni tevékenység futtatásához használ.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-213">The sample solution uses Azure Batch to run the custom activity.</span></span> <span data-ttu-id="3c4b6-214">Lásd: [egyéni tevékenységeket használni egy Azure Data Factory-folyamathoz](data-factory-use-custom-activities.md) fejlesztése egyéni tevékenységeket, és használhatja őket az Azure Data Factory folyamatok alapvető információ megadása.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-214">See [Use custom activities in an Azure Data Factory pipeline](data-factory-use-custom-activities.md) for the basic information to develop custom activities and use them in Azure Data Factory pipelines.</span></span>

<span data-ttu-id="3c4b6-215">Egy Azure Data Factory-folyamathoz használható .NET egyéni tevékenységek létrehozásához szüksége a **.NET Class Library** egy osztály, amely megvalósítja az, hogy a projekt **IDotNetActivity** felületet.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-215">To create a .NET custom activity that you can use in an Azure Data Factory pipeline, you need to create a **.NET Class Library** project with a class that implements that **IDotNetActivity** interface.</span></span> <span data-ttu-id="3c4b6-216">Ez az interfész csak egy metódusa: **Execute**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-216">This interface has only one method: **Execute**.</span></span> <span data-ttu-id="3c4b6-217">Ez a metódus aláírása:</span><span class="sxs-lookup"><span data-stu-id="3c4b6-217">Here is the signature of the method:</span></span>

```csharp
public IDictionary<string, string> Execute(
            IEnumerable<LinkedService> linkedServices,
            IEnumerable<Dataset> datasets,
            Activity activity,
            IActivityLogger logger)
```

<span data-ttu-id="3c4b6-218">A metódushoz meg kell ismernie néhány kulcsfontosságú összetevők.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-218">The method has a few key components that you need to understand.</span></span>

* <span data-ttu-id="3c4b6-219">A metódus négy paramétereket fogadja:</span><span class="sxs-lookup"><span data-stu-id="3c4b6-219">The method takes four parameters:</span></span>

  1. <span data-ttu-id="3c4b6-220">**linkedServices**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-220">**linkedServices**.</span></span> <span data-ttu-id="3c4b6-221">Egy enumerálható listája, amely a bemeneti/kimeneti adatforrások összekapcsolt szolgáltatások (például: Azure Blob Storage) az adat-előállító.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-221">An enumerable list of linked services that link input/output data sources (for example: Azure Blob Storage) to the data factory.</span></span> <span data-ttu-id="3c4b6-222">Ez a példa nincs Azure Storage használt bemeneti és kimeneti típusú csak egy társított szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-222">In this sample, there is only one linked service of type Azure Storage used for both input and output.</span></span>
  2. <span data-ttu-id="3c4b6-223">**adatkészletek**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-223">**datasets**.</span></span> <span data-ttu-id="3c4b6-224">A rendszer egy enumerálható állnak.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-224">This is an enumerable list of datasets.</span></span> <span data-ttu-id="3c4b6-225">A helyek és a bemeneti és kimeneti adatkészletek által megadott sémák használhatja ezt a paramétert.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-225">You can use this parameter to get the locations and schemas defined by input and output datasets.</span></span>
  3. <span data-ttu-id="3c4b6-226">**tevékenység**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-226">**activity**.</span></span> <span data-ttu-id="3c4b6-227">Ez a paraméter az aktuális entitásnak számítási – ebben az esetben az Azure Batch szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-227">This parameter represents the current compute entity - in this case, an Azure Batch service.</span></span>
  4. <span data-ttu-id="3c4b6-228">**naplózó**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-228">**logger**.</span></span> <span data-ttu-id="3c4b6-229">A naplózó lehetővé teszi az adatcsatorna "User" bejelentkezési az adott felület hibakeresési megjegyzések írását.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-229">The logger lets you write debug comments that surface as the “User” log for the pipeline.</span></span>
* <span data-ttu-id="3c4b6-230">A metódus visszaadja a szótár részére láncolni egyéni tevékenységek együtt a jövőben használható.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-230">The method returns a dictionary that can be used to chain custom activities together in the future.</span></span> <span data-ttu-id="3c4b6-231">Ez a funkció még nem használható, így egy üres szótár visszaadásának metódus.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-231">This feature is not implemented yet, so return an empty dictionary from the method.</span></span>

#### <a name="procedure-create-the-custom-activity"></a><span data-ttu-id="3c4b6-232">Az eljárás: Az egyéni tevékenység létrehozása</span><span class="sxs-lookup"><span data-stu-id="3c4b6-232">Procedure: Create the custom activity</span></span>
1. <span data-ttu-id="3c4b6-233">A Visual Studio .NET Class Library projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="3c4b6-233">Create a .NET Class Library project in Visual Studio.</span></span>

   1. <span data-ttu-id="3c4b6-234">Indítsa el **Visual Studio 2012**/**2013 vagy 2015**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-234">Launch **Visual Studio 2012**/**2013/2015**.</span></span>
   2. <span data-ttu-id="3c4b6-235">Kattintson a **File** (Fájl) menüre, mutasson a **New** (Új) elemre, és kattintson a **Project** (Projekt) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-235">Click **File**, point to **New**, and click **Project**.</span></span>
   3. <span data-ttu-id="3c4b6-236">Bontsa ki a **sablonok**, és válassza ki **Visual C\#**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-236">Expand **Templates**, and select **Visual C\#**.</span></span> <span data-ttu-id="3c4b6-237">Ebben a bemutatóban használhat C\#, de bármely .NET nyelvi használhatja az egyéni tevékenység fejlesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-237">In this walkthrough, you use C\#, but you can use any .NET language to develop the custom activity.</span></span>
   4. <span data-ttu-id="3c4b6-238">Válassza ki **Class Library** a jobb oldali projekttípusok közül.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-238">Select **Class Library** from the list of project types on the right.</span></span>
   5. <span data-ttu-id="3c4b6-239">Adja meg **MyDotNetActivity** a a **neve**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-239">Enter **MyDotNetActivity** for the **Name**.</span></span>
   6. <span data-ttu-id="3c4b6-240">Válassza ki **C:\\ADF** a a **hely**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-240">Select **C:\\ADF** for the **Location**.</span></span> <span data-ttu-id="3c4b6-241">A mappa létrehozása **ADF** Ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-241">Create the folder **ADF** if it does not exist.</span></span>
   7. <span data-ttu-id="3c4b6-242">A projekt létrehozásához kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-242">Click **OK** to create the project.</span></span>
2. <span data-ttu-id="3c4b6-243">Kattintson az **Eszközök** elemre, mutasson a **NuGet Package Manager** (NuGet-csomagkezelő) lehetőségre, majd kattintson a **Package Manager Console** (Csomagkezelő konzol) elemre.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-243">Click **Tools**, point to **NuGet Package Manager**, and click **Package Manager Console**.</span></span>
3. <span data-ttu-id="3c4b6-244">Az a **Csomagkezelő konzol**, hajtsa végre a következő parancsot importálandó **Microsoft.Azure.Management.DataFactories**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-244">In the **Package Manager Console**, execute the following command to import **Microsoft.Azure.Management.DataFactories**.</span></span>

    ```powershell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. <span data-ttu-id="3c4b6-245">Importálás a **Azure Storage** NuGet-csomagot a projekthez.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-245">Import the **Azure Storage** NuGet package in to the project.</span></span> <span data-ttu-id="3c4b6-246">Mivel ez a példa a Blob storage API kell ezt a csomagot.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-246">You need this package because you use the Blob storage API in this sample.</span></span>

    ```powershell
    Install-Package Azure.Storage
    ```
5. <span data-ttu-id="3c4b6-247">Adja hozzá a következő **használatával** irányelvek a forrásfájl, a projektben.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-247">Add the following **using** directives to the source file in the project.</span></span>

    ```csharp
    using System.IO;
    using System.Globalization;
    using System.Diagnostics;
    using System.Linq;
    
    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Runtime;
    
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```
6. <span data-ttu-id="3c4b6-248">Módosítsa a nevét a **névtér** való **MyDotNetActivityNS**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-248">Change the name of the **namespace** to **MyDotNetActivityNS**.</span></span>

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. <span data-ttu-id="3c4b6-249">Változtassa meg az osztály nevét **MyDotNetActivity** és a Származtatás a **IDotNetActivity** csatoló alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-249">Change the name of the class to **MyDotNetActivity** and derive it from the **IDotNetActivity** interface as shown below.</span></span>

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. <span data-ttu-id="3c4b6-250">Megvalósítása (Hozzáadás) a **Execute** metódusában a **IDotNetActivity** a csatoló a **MyDotNetActivity** osztályhoz, és másolja az alábbi példakód a metódust.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-250">Implement (Add) the **Execute** method of the **IDotNetActivity** interface to the **MyDotNetActivity** class and copy the following sample code to the method.</span></span> <span data-ttu-id="3c4b6-251">Tekintse meg a [metódus végrehajtása](#execute-method) szakasz a metódusban használt logika ismertetése.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-251">See the [Execute Method](#execute-method) section for explanation for the logic used in this method.</span></span>

    ```csharp
    /// <summary>
    /// Execute method is the only method of IDotNetActivity interface you must implement.
    /// In this sample, the method invokes the Calculate method to perform the core logic.  
    /// </summary>
    public IDictionary<string, string> Execute(
       IEnumerable<LinkedService> linkedServices,
       IEnumerable<Dataset> datasets,
       Activity activity,
       IActivityLogger logger)
    {
    
       // declare types for input and output data stores
       AzureStorageLinkedService inputLinkedService;
    
       Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
    
       foreach (LinkedService ls in linkedServices)
           logger.Write("linkedService.Name {0}", ls.Name);
    
       // using First method instead of Single since we are using the same
       // Azure Storage linked service for input and output.
       inputLinkedService = linkedServices.First(
           linkedService =>
           linkedService.Name ==
           inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
           as AzureStorageLinkedService;
    
       string connectionString = inputLinkedService.ConnectionString; // To create an input storage client.
       string folderPath = GetFolderPath(inputDataset);
       string output = string.Empty; // for use later.
    
       // create storage client for input. Pass the connection string.
       CloudStorageAccount inputStorageAccount = CloudStorageAccount.Parse(connectionString);
       CloudBlobClient inputClient = inputStorageAccount.CreateCloudBlobClient();
    
       // initialize the continuation token before using it in the do-while loop.
       BlobContinuationToken continuationToken = null;
       do
       {   // get the list of input blobs from the input storage client object.
           BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
                                    true,
                                    BlobListingDetails.Metadata,
                                    null,
                                    continuationToken,
                                    null,
                                    null);
    
           // Calculate method returns the number of occurrences of
           // the search term (“Microsoft”) in each blob associated
           // with the data slice.
           //
           // definition of the method is shown in the next step.
           output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
       } while (continuationToken != null);
    
       // get the output dataset using the name of the dataset matched to a name in the Activity output collection.
       Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    
       folderPath = GetFolderPath(outputDataset);
    
       logger.Write("Writing blob to the folder: {0}", folderPath);
    
       // create a storage object for the output blob.
       CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
       // write the name of the file.
       Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    
       logger.Write("output blob URI: {0}", outputBlobUri.ToString());
       // create a blob and upload the output text.
       CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
       logger.Write("Writing {0} to the output blob", output);
       outputBlob.UploadText(output);
    
       // The dictionary can be used to chain custom activities together in the future.
       // This feature is not implemented yet, so just return an empty dictionary.
       return new Dictionary<string, string>();
    }
    ```
9. <span data-ttu-id="3c4b6-252">A következő segédmódszereket adhat hozzá az osztályhoz.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-252">Add the following helper methods to the class.</span></span> <span data-ttu-id="3c4b6-253">Ezek a módszerek hívják a **Execute** metódust.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-253">These methods are invoked by the **Execute** method.</span></span> <span data-ttu-id="3c4b6-254">A legfontosabb a **Calculate** metódus elkülöníti a kódot, amely minden egyes blob telepítéseket.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-254">Most importantly, the **Calculate** method isolates the code that iterates through each blob.</span></span>

    ```csharp
    /// <summary>
    /// Gets the folderPath value from the input/output dataset.
    /// </summary>
    private static string GetFolderPath(Dataset dataArtifact)
    {
       if (dataArtifact == null || dataArtifact.Properties == null)
       {
           return null;
       }
    
       AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
       if (blobDataset == null)
       {
           return null;
       }
    
       return blobDataset.FolderPath;
    }
    
    /// <summary>
    /// Gets the fileName value from the input/output dataset.
    /// </summary>
    
    private static string GetFileName(Dataset dataArtifact)
    {
       if (dataArtifact == null || dataArtifact.Properties == null)
       {
           return null;
       }
    
       AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
       if (blobDataset == null)
       {
           return null;
       }
    
       return blobDataset.FileName;
    }
    
    /// <summary>
    /// Iterates through each blob (file) in the folder, counts the number of instances of search term in the file,
    /// and prepares the output text that is written to the output blob.
    /// </summary>
    
    public static string Calculate(BlobResultSegment Bresult, IActivityLogger logger, string folderPath, ref BlobContinuationToken token, string searchTerm)
    {
       string output = string.Empty;
       logger.Write("number of blobs found: {0}", Bresult.Results.Count<IListBlobItem>());
       foreach (IListBlobItem listBlobItem in Bresult.Results)
       {
           CloudBlockBlob inputBlob = listBlobItem as CloudBlockBlob;
           if ((inputBlob != null) && (inputBlob.Name.IndexOf("$$$.$$$") == -1))
           {
               string blobText = inputBlob.DownloadText(Encoding.ASCII, null, null, null);
               logger.Write("input blob text: {0}", blobText);
               string[] source = blobText.Split(new char[] { '.', '?', '!', ' ', ';', ':', ',' }, StringSplitOptions.RemoveEmptyEntries);
               var matchQuery = from word in source
                                where word.ToLowerInvariant() == searchTerm.ToLowerInvariant()
                                select word;
               int wordCount = matchQuery.Count();
               output += string.Format("{0} occurrences(s) of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
           }
       }
       return output;
    }
    ```
    <span data-ttu-id="3c4b6-255">A **GetFolderPath** metódus visszaadja az elérési út a mappába, amely a DataSet adatkészlet mutat, és a **GetFileName** módszer a/fájlját, hogy az adatkészlet nevét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-255">The **GetFolderPath** method returns the path to the folder that the dataset points to and the **GetFileName** method returns the name of the blob/file that the dataset points to.</span></span>

    ```csharp

    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
    ```

    <span data-ttu-id="3c4b6-256">A **Calculate** metódus kiszámítja a kulcsszó-példányok száma **Microsoft** a bemeneti fájlok (BLOB a mappában található).</span><span class="sxs-lookup"><span data-stu-id="3c4b6-256">The **Calculate** method calculates the number of instances of keyword **Microsoft** in the input files (blobs in the folder).</span></span> <span data-ttu-id="3c4b6-257">A keresési kifejezés ("Microsoft") nem változtatható a kódban.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-257">The search term (“Microsoft”) is hard-coded in the code.</span></span>

1. <span data-ttu-id="3c4b6-258">Fordítsa le a projektet.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-258">Compile the project.</span></span> <span data-ttu-id="3c4b6-259">Kattintson a **Build** a menüre, majd a **megoldás fordítása**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-259">Click **Build** from the menu and click **Build Solution**.</span></span>
2. <span data-ttu-id="3c4b6-260">Indítsa el **Windows Explorer**, és keresse meg **bin\\debug** vagy **bin\\kiadási** mappa build típusától függően.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-260">Launch **Windows Explorer**, and navigate to **bin\\debug** or **bin\\release** folder depending on the type of build.</span></span>
3. <span data-ttu-id="3c4b6-261">Hozzon létre egy zip fájlt **MyDotNetActivity.zip** , amely tartalmazza a bináris fájlok a  **\\bin\\Debug** mappát.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-261">Create a zip file **MyDotNetActivity.zip** that contains all the binaries in the **\\bin\\Debug** folder.</span></span> <span data-ttu-id="3c4b6-262">Érdemes lehet a MyDotNetActivity tartalmazza. **pdb** fájlt úgy, hogy be további részletekről, mint a sor számának a forráskódban, hogy a problémát az okozza, ha hiba lép fel.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-262">You may want to include the MyDotNetActivity.**pdb** file so that you get additional details such as line number in the source code that caused the issue when a failure occurs.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image5.png)
4. <span data-ttu-id="3c4b6-263">Töltse fel **MyDotNetActivity.zip** mint a blobtárolót egy blobot: `customactivitycontainer` az Azure blob-tároló, amely a **StorageLinkedService** társított a szolgáltatás a  **ADFTutorialDataFactory** használja.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-263">Upload **MyDotNetActivity.zip** as a blob to the blob container: `customactivitycontainer` in the Azure blob storage that the **StorageLinkedService** linked service in the **ADFTutorialDataFactory** uses.</span></span> <span data-ttu-id="3c4b6-264">A blob-tároló létrehozása `customactivitycontainer` Ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-264">Create the blob container `customactivitycontainer` if it does not already exist.</span></span>

#### <a name="execute-method"></a><span data-ttu-id="3c4b6-265">Metódus végrehajtása</span><span class="sxs-lookup"><span data-stu-id="3c4b6-265">Execute method</span></span>
<span data-ttu-id="3c4b6-266">Ez a témakör további részleteket és megjegyzéseket fűzhet az Execute metódus kódja.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-266">This section provides more details and notes about the code in the Execute method.</span></span>

1. <span data-ttu-id="3c4b6-267">A tagok iteráció a bemeneti gyűjteményben találhatók a [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) névtér.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-267">The members for iterating through the input collection are found in the [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) namespace.</span></span> <span data-ttu-id="3c4b6-268">A blob gyűjtemény iteráció kell használni a **BlobContinuationToken** osztály.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-268">Iterating through the blob collection requires using the **BlobContinuationToken** class.</span></span> <span data-ttu-id="3c4b6-269">Lényegében kell használnia a do-ciklus az a jogkivonatot a rendszer a hurokból való kilépés közben.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-269">In essence, you must use a do-while loop with the token as the mechanism for exiting the loop.</span></span> <span data-ttu-id="3c4b6-270">További információkért lásd: [használata a .NET-Blob-tároló](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="3c4b6-270">For more information, see [How to use Blob storage from .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="3c4b6-271">Alapszintű hurkot itt jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="3c4b6-271">A basic loop is shown here:</span></span>

    ```csharp
    // Initialize the continuation token.
    BlobContinuationToken continuationToken = null;
    do
    {
    // Get the list of input blobs from the input storage client object.
    BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
    
                         true,
                                   BlobListingDetails.Metadata,
                                   null,
                                   continuationToken,
                                   null,
                                   null);
    // Return a string derived from parsing each blob.
    
     output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
    } while (continuationToken != null);

    ```
   <span data-ttu-id="3c4b6-272">A dokumentációban a [ListBlobsSegmented](https://msdn.microsoft.com/library/jj717596.aspx) részletes metódust.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-272">See the documentation for the [ListBlobsSegmented](https://msdn.microsoft.com/library/jj717596.aspx) method for details.</span></span>
2. <span data-ttu-id="3c4b6-273">A használatához, áttekintve blobok készletét logikailag kódját a ne belül kerül-ciklus során.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-273">The code for working through the set of blobs logically goes within the do-while loop.</span></span> <span data-ttu-id="3c4b6-274">Az a **Execute** metódust, ne-közben hurok nevű metódus a bináris objektumok listáját adja át **Calculate**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-274">In the **Execute** method, the do-while loop passes the list of blobs to a method named **Calculate**.</span></span> <span data-ttu-id="3c4b6-275">A metódus visszaadja egy karakterlánc-változóvá nevű **kimeneti** , amely rendelkezik a szegmensben lévő összes blobjának keresztül többször eredménye.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-275">The method returns a string variable named **output** that is the result of having iterated through all the blobs in the segment.</span></span>

   <span data-ttu-id="3c4b6-276">A keresési kifejezés előfordulási adja vissza (**Microsoft**) átadott található a **Calculate** metódus.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-276">It returns the number of occurrences of the search term (**Microsoft**) in the blob passed to the **Calculate** method.</span></span>

    ```csharp
    output += string.Format("{0} occurrences of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
    ```
3. <span data-ttu-id="3c4b6-277">Egyszer a **Calculate** metódus végzett munka, azt kell írni új blob.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-277">Once the **Calculate** method has done the work, it must be written to a new blob.</span></span> <span data-ttu-id="3c4b6-278">Így a blobok feldolgozása minden készlete esetében egy új blob írhatók az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-278">So for every set of blobs processed, a new blob can be written with the results.</span></span> <span data-ttu-id="3c4b6-279">Egy új blob írni először található a kimeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-279">To write to a new blob, first find the output dataset.</span></span>

    ```csharp
    // Get the output dataset using the name of the dataset matched to a name in the Activity output collection.
    Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    ```
4. <span data-ttu-id="3c4b6-280">A kódot is meghívja egy segédmetódust: **GetFolderPath** lehet lekérni a mappa elérési útját (a tároló nevét).</span><span class="sxs-lookup"><span data-stu-id="3c4b6-280">The code also calls a helper method: **GetFolderPath** to retrieve the folder path (the storage container name).</span></span>

    ```csharp
    folderPath = GetFolderPath(outputDataset);
    ```
   <span data-ttu-id="3c4b6-281">A **GetFolderPath** árnyékot a DataSet adatkészlet-objektum egy AzureBlobDataSet, amelynek nevű FolderPath tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-281">The **GetFolderPath** casts the DataSet object to an AzureBlobDataSet, which has a property named FolderPath.</span></span>

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FolderPath;
    ```
5. <span data-ttu-id="3c4b6-282">A kód hívások a **GetFileName** metódusának segítéségével lekérheti a fájlnév (blob neve).</span><span class="sxs-lookup"><span data-stu-id="3c4b6-282">The code calls the **GetFileName** method to retrieve the file name (blob name).</span></span> <span data-ttu-id="3c4b6-283">A kód hasonlít a fenti kód segítségével kérheti le a mappa elérési útját.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-283">The code is similar to the above code to get the folder path.</span></span>

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FileName;
    ```
6. <span data-ttu-id="3c4b6-284">Egy URI-objektum létrehozása a fájl nevét írja.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-284">The name of the file is written by creating a URI object.</span></span> <span data-ttu-id="3c4b6-285">Az URI konstruktor használja a **BlobEndpoint** tulajdonság vissza a tároló neve.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-285">The URI constructor uses the **BlobEndpoint** property to return the container name.</span></span> <span data-ttu-id="3c4b6-286">A mappa elérési útját és nevét a kimeneti blob URI összeállításához kerülnek.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-286">The folder path and file name are added to construct the output blob URI.</span></span>  

    ```csharp
    // Write the name of the file.
    Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    ```
7. <span data-ttu-id="3c4b6-287">A fájl neve van írva, és most lehet írni a kimeneti karakterláncot, a **Calculate** egy új blob módszert:</span><span class="sxs-lookup"><span data-stu-id="3c4b6-287">The name of the file has been written and now you can write the output string from the **Calculate** method to a new blob:</span></span>

    ```csharp
    // Create a blob and upload the output text.
    CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
    logger.Write("Writing {0} to the output blob", output);
    outputBlob.UploadText(output);
    ```

### <a name="create-the-data-factory"></a><span data-ttu-id="3c4b6-288">A data factory létrehozása</span><span class="sxs-lookup"><span data-stu-id="3c4b6-288">Create the data factory</span></span>
<span data-ttu-id="3c4b6-289">Az a [az egyéni tevékenység létrehozása](#create-the-custom-activity) szakaszban létrehozott egyéni tevékenység és a zip-fájl bináris fájljait és a PDB-fájl feltöltése egy Azure blob-tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-289">In the [Create the custom activity](#create-the-custom-activity) section, you created a custom activity and uploaded the zip file with binaries and the PDB file to an Azure blob container.</span></span> <span data-ttu-id="3c4b6-290">Ebben a szakaszban hoz létre egy Azure **adat-előállító** rendelkező egy **csővezeték** , amely használja a **egyéni tevékenység**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-290">In this section, you create an Azure **data factory** with a **pipeline** that uses the **custom activity**.</span></span>

<span data-ttu-id="3c4b6-291">Az egyéni tevékenység bemeneti adatkészletet jelenti. a blobot (fájlok) a bemeneti mappában (`mycontainer\\inputfolder`) a blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-291">The input dataset for the custom activity represents the blobs (files) in the input folder (`mycontainer\\inputfolder`) in blob storage.</span></span> <span data-ttu-id="3c4b6-292">A kimeneti adatkészlet a tevékenység jelenti. a kimeneti blobot, amely a kimeneti mappa (`mycontainer\\outputfolder`) a blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-292">The output dataset for the activity represents the output blobs in the output folder (`mycontainer\\outputfolder`) in blob storage.</span></span>

<span data-ttu-id="3c4b6-293">A bemeneti mappák el egy vagy több fájlokat:</span><span class="sxs-lookup"><span data-stu-id="3c4b6-293">Drop one or more files in the input folders:</span></span>

```
mycontainer -\> inputfolder
    2015-11-16-00
    2015-11-16-01
    2015-11-16-02
    2015-11-16-03
    2015-11-16-04
```

<span data-ttu-id="3c4b6-294">Például dobja el egy fájl (file.txt) a következő tartalommal mindegyik mappákat.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-294">For example, drop one file (file.txt) with the following content into each of the folders.</span></span>

```
test custom activity Microsoft test custom activity Microsoft
```

<span data-ttu-id="3c4b6-295">Minden egyes bemeneti mappa felel meg az Azure Data Factory szelet akkor is, ha a mappa 2 vagy több fájlt.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-295">Each input folder corresponds to a slice in Azure Data Factory even if the folder has 2 or more files.</span></span> <span data-ttu-id="3c4b6-296">A folyamat minden szelet feldolgozása után az egyéni tevékenység az, hogy a szelet bemeneti mappa összes blobjának telepítéseket.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-296">When each slice is processed by the pipeline, the custom activity iterates through all the blobs in the input folder for that slice.</span></span>

<span data-ttu-id="3c4b6-297">Megjelenik a kimeneti fájlok öt ugyanahhoz a tartalomhoz.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-297">You see five output files with the same content.</span></span> <span data-ttu-id="3c4b6-298">Például a kimeneti fájl nem dolgozza fel a 2015-11-16-00 mappában található a fájl tartalma a következő:</span><span class="sxs-lookup"><span data-stu-id="3c4b6-298">For example, the output file from processing the file in the 2015-11-16-00 folder has the following content:</span></span>

```
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
```

<span data-ttu-id="3c4b6-299">Ha több fájl (file.txt, fájl2.ref fájllal, file3.txt) rendelkező ugyanahhoz a tartalomhoz a bemeneti mappába dobja el, tekintse meg a következő tartalmat a kimeneti fájl.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-299">If you drop multiple files (file.txt, file2.txt, file3.txt) with the same content to the input folder, you see the following content in the output file.</span></span> <span data-ttu-id="3c4b6-300">Minden mappa (2015-11-16-00, stb.) megfelel-e ez a példa szelet annak ellenére, hogy a mappa több bemeneti fájlokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-300">Each folder (2015-11-16-00, etc.) corresponds to a slice in this sample even though the folder has multiple input files.</span></span>

```csharp
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file2.txt.
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file3.txt.
```

<span data-ttu-id="3c4b6-301">A kimeneti fájl három sort, egyet a mappában, a szelet (2015-11-16-00) társított minden egyes bemeneti fájl (blob) rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-301">The output file has three lines now, one for each input file (blob) in the folder associated with the slice (2015-11-16-00).</span></span>

<span data-ttu-id="3c4b6-302">Egy feladat minden egyes tevékenységfuttatási jön létre.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-302">A task is created for each activity run.</span></span> <span data-ttu-id="3c4b6-303">Ez a példa nincs csak egy tevékenység folyamatban.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-303">In this sample, there is only one activity in the pipeline.</span></span> <span data-ttu-id="3c4b6-304">A szelet feldolgozása a folyamat, az egyéni tevékenység fut, a szelet feldolgozása az Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-304">When a slice is processed by the pipeline, the custom activity runs on Azure Batch to process the slice.</span></span> <span data-ttu-id="3c4b6-305">Nincsenek öt szeletek (minden szelet rendelkezhet több bináris objektumok vagy fájl), mert nincsenek Azure Batch létrehozott öt feladatok.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-305">Since there are five slices (each slice can have multiple blobs or file), there are five tasks created in Azure Batch.</span></span> <span data-ttu-id="3c4b6-306">Kötegelt feladatként fut, esetén ténylegesen fut-e egyéni tevékenység.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-306">When a task runs on Batch, it is actually the custom activity that is running.</span></span>

<span data-ttu-id="3c4b6-307">A következő forgatókönyv további részleteket biztosít.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-307">The following walkthrough provides additional details.</span></span>

#### <a name="step-1-create-the-data-factory"></a><span data-ttu-id="3c4b6-308">1. lépés: Az adat-előállító létrehozása</span><span class="sxs-lookup"><span data-stu-id="3c4b6-308">Step 1: Create the data factory</span></span>
1. <span data-ttu-id="3c4b6-309">Való bejelentkezés után a [Azure-portálon](https://portal.azure.com/), hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3c4b6-309">After logging in to the [Azure portal](https://portal.azure.com/), do the following steps:</span></span>

   1. <span data-ttu-id="3c4b6-310">A bal oldali menüben kattintson a **NEW** (ÚJ) elemre.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-310">Click **NEW** on the left menu.</span></span>
   2. <span data-ttu-id="3c4b6-311">Kattintson a **adatok + analitika** a a **új** panelen.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-311">Click **Data + Analytics** in the **New** blade.</span></span>
   3. <span data-ttu-id="3c4b6-312">Kattintson a **Data Factory** elemre a **Data analytics** (Adatelemzés) panelen.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-312">Click **Data Factory** on the **Data analytics** blade.</span></span>
2. <span data-ttu-id="3c4b6-313">Az a **új adat-előállító** panelen adjon meg **CustomActivityFactory** nevét.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-313">In the **New data factory** blade, enter **CustomActivityFactory** for the Name.</span></span> <span data-ttu-id="3c4b6-314">Az Azure data factory nevének globálisan egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-314">The name of the Azure data factory must be globally unique.</span></span> <span data-ttu-id="3c4b6-315">Ha a hibaüzenetet kapja: **nem érhető el adat-előállító "CustomActivityFactory"**, változtassa meg a data factory nevét (például **yournameCustomActivityFactory**), és próbálja meg újra létrehozni.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-315">If you receive the error: **Data factory name “CustomActivityFactory” is not available**, change the name of the data factory (for example, **yournameCustomActivityFactory**) and try creating again.</span></span>
3. <span data-ttu-id="3c4b6-316">Kattintson a **ERŐFORRÁSCSOPORT-név**, és válasszon ki egy meglévő erőforráscsoportot, vagy hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-316">Click **RESOURCE GROUP NAME**, and select an existing resource group or create a resource group.</span></span>
4. <span data-ttu-id="3c4b6-317">Győződjön meg arról, hogy a megfelelő előfizetés és a régióban, amelyben szeretné létrehozni az adat-előállítóban használja.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-317">Verify that you are using the correct subscription and region where you want the data factory to be created.</span></span>
5. <span data-ttu-id="3c4b6-318">Kattintson a **Create** (Létrehozás) elemre a **New data factory** (Új data factory) panelen.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-318">Click **Create** on the **New data factory** blade.</span></span>
6. <span data-ttu-id="3c4b6-319">Megjelenik a data factory létrehozása a **irányítópult** az Azure portálról.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-319">You see the data factory being created in the **Dashboard** of the Azure portal.</span></span>
7. <span data-ttu-id="3c4b6-320">A data factory sikeres létrehozása után megjelenik a data factory oldal, amely megjeleníti a data factory tartalmát.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-320">After the data factory has been created successfully, you see the data factory page, which shows you the contents of the data factory.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image6.png)

#### <a name="step-2-create-linked-services"></a><span data-ttu-id="3c4b6-321">2. lépés: A társított szolgáltatások létrehozásához</span><span class="sxs-lookup"><span data-stu-id="3c4b6-321">Step 2: Create linked services</span></span>
<span data-ttu-id="3c4b6-322">A társított szolgáltatások adattárakat vagy számítási szolgáltatásokat társítanak az Azure data factoryhez.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-322">Linked services link data stores or compute services to an Azure data factory.</span></span> <span data-ttu-id="3c4b6-323">Ebben a lépésben hivatkozásra a **Azure Storage** fiók és **Azure Batch** a data factory fiókot.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-323">In this step, you link your **Azure Storage** account and **Azure Batch** account to your data factory.</span></span>

#### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="3c4b6-324">Azure Storage társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="3c4b6-324">Create Azure Storage linked service</span></span>
1. <span data-ttu-id="3c4b6-325">Kattintson a **Szerző és központi telepítése** csempét a **adat-előállító** paneljén **CustomActivityFactory**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-325">Click the **Author and deploy** tile on the **DATA FACTORY** blade for **CustomActivityFactory**.</span></span> <span data-ttu-id="3c4b6-326">A Data Factory Editor láthatja.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-326">You see the Data Factory Editor.</span></span>
2. <span data-ttu-id="3c4b6-327">Kattintson a **az új adattároló** a parancs megnyitásához, és válassza a **az Azure storage.**</span><span class="sxs-lookup"><span data-stu-id="3c4b6-327">Click **New data store** on the command bar and choose **Azure storage.**</span></span> <span data-ttu-id="3c4b6-328">A szerkesztőben megjelenik az Azure Storage társított szolgáltatás létrehozására szolgáló JSON-parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-328">You should see the JSON script for creating an Azure Storage linked service in the editor.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image7.png)

3. <span data-ttu-id="3c4b6-329">Az **account name** kifejezést cserélje az Azure Storage-fiókja nevére, az **account key** kifejezést pedig az Azure Storage-fiók kulcsára.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-329">Replace **account name** with the name of your Azure storage account and **account key** with the access key of the Azure storage account.</span></span> <span data-ttu-id="3c4b6-330">A tárelérési kulcs lekérésével kapcsolatos információk: [Tárelérési kulcsok megtekintése, másolása és újragenerálása](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="3c4b6-330">To learn how to get your storage access key, see [View, copy and regenerate storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>

4. <span data-ttu-id="3c4b6-331">A társított szolgáltatás üzembe helyezéséhez kattintson a parancssáv **Deploy** (Üzembe helyezés) elemére.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-331">Click **Deploy** on the command bar to deploy the linked service.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image8.png)

#### <a name="create-azure-batch-linked-service"></a><span data-ttu-id="3c4b6-332">Csatolt Azure Batch szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="3c4b6-332">Create Azure Batch linked service</span></span>
<span data-ttu-id="3c4b6-333">Ebben a lépésben a társított szolgáltatás létrehozása a **Azure Batch** a Data Factory egyéni tevékenység futtatásához használt fiók.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-333">In this step, you create a linked service for your **Azure Batch** account that is used to run the Data Factory custom activity.</span></span>

1. <span data-ttu-id="3c4b6-334">Kattintson a **új számítási** a parancs megnyitásához, és válassza a **Azure Batch.**</span><span class="sxs-lookup"><span data-stu-id="3c4b6-334">Click **New compute** on the command bar and choose **Azure Batch.**</span></span> <span data-ttu-id="3c4b6-335">A JSON-parancsfájl egy csatolt Azure Batch szolgáltatás létrehozásához a szerkesztőben kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-335">You should see the JSON script for creating an Azure Batch linked service in the editor.</span></span>
2. <span data-ttu-id="3c4b6-336">A JSON-parancsfájl:</span><span class="sxs-lookup"><span data-stu-id="3c4b6-336">In the JSON script:</span></span>

   1. <span data-ttu-id="3c4b6-337">Cserélje le **fióknév** az Azure Batch-fiók nevével.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-337">Replace **account name** with the name of your Azure Batch account.</span></span>
   2. <span data-ttu-id="3c4b6-338">Cserélje le **hozzáférési kulcs** az Azure Batch-fiók hozzáférési kulcsával.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-338">Replace **access key** with the access key of the Azure Batch account.</span></span>
   3. <span data-ttu-id="3c4b6-339">Adja meg a készlet Azonosítóját a **poolName** tulajdonság**.**</span><span class="sxs-lookup"><span data-stu-id="3c4b6-339">Enter the ID of the pool for the **poolName** property**.**</span></span> <span data-ttu-id="3c4b6-340">Ehhez a tulajdonsághoz adja meg vagy készlet nevét vagy tárolókészlet azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-340">For this property, you can specify either pool name or pool ID.</span></span>
   4. <span data-ttu-id="3c4b6-341">Adja meg a kötegelt URI esetében a **batchUri** JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-341">Enter the batch URI for the **batchUri** JSON property.</span></span>

      > [!IMPORTANT]
      > <span data-ttu-id="3c4b6-342">A **URL-cím** a a **Azure Batch-fiók panelen** a következő formátumban: \<accountname\>.\< régió\>. batch.azure.com. Az a **batchUri** tulajdonság a JSON-ban, kell **távolítsa el a "accountname."**</span><span class="sxs-lookup"><span data-stu-id="3c4b6-342">The **URL** from the **Azure Batch account blade** is in the following format: \<accountname\>.\<region\>.batch.azure.com. For the **batchUri** property in the JSON, you need to **remove "accountname."**</span></span> <span data-ttu-id="3c4b6-343">az URL-címből.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-343">from the URL.</span></span> <span data-ttu-id="3c4b6-344">Példa: `"batchUri": "https://eastus.batch.azure.com"`.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-344">Example: `"batchUri": "https://eastus.batch.azure.com"`.</span></span>
      >
      >

      ![](./media/data-factory-data-processing-using-batch/image9.png)

      <span data-ttu-id="3c4b6-345">Az a **poolName** tulajdonság, azt is megadhatja a tárolókészlet neve helyett a készlet Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-345">For the **poolName** property, you can also specify the ID of the pool instead of the name of the pool.</span></span>

      > [!NOTE]
      > <span data-ttu-id="3c4b6-346">A Data Factory szolgáltatásnak nem támogatja egy igény szerinti lehetőséget az Azure hdinsight azonban nem.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-346">The Data Factory service does not support an on-demand option for Azure Batch as it does for HDInsight.</span></span> <span data-ttu-id="3c4b6-347">Csak a saját Azure Batch-készlet használhatja az Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-347">You can only use your own Azure Batch pool in an Azure data factory.</span></span>
      >
      >
   5. <span data-ttu-id="3c4b6-348">Adja meg **StorageLinkedService** a a **linkedServiceName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-348">Specify **StorageLinkedService** for the **linkedServiceName** property.</span></span> <span data-ttu-id="3c4b6-349">Az előző lépésben létrehozott szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-349">You created this linked service in the previous step.</span></span> <span data-ttu-id="3c4b6-350">A tárolót használja a rendszer egy átmeneti területre, fájlok és a naplókat.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-350">This storage is used as a staging area for files and logs.</span></span>
3. <span data-ttu-id="3c4b6-351">A társított szolgáltatás üzembe helyezéséhez kattintson a parancssáv **Deploy** (Üzembe helyezés) elemére.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-351">Click **Deploy** on the command bar to deploy the linked service.</span></span>

#### <a name="step-3-create-datasets"></a><span data-ttu-id="3c4b6-352">3. lépés: Adatkészletek létrehozása</span><span class="sxs-lookup"><span data-stu-id="3c4b6-352">Step 3: Create datasets</span></span>
<span data-ttu-id="3c4b6-353">Ebben a lépésben hoz létre a bemeneti és kimeneti adatok adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-353">In this step, you create datasets to represent input and output data.</span></span>

#### <a name="create-input-dataset"></a><span data-ttu-id="3c4b6-354">Bemeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="3c4b6-354">Create input dataset</span></span>
1. <span data-ttu-id="3c4b6-355">Az a **szerkesztő** a Data Factory kattintson **új adatkészlet** gomb az eszköztáron és a kattintson **Azure Blob Storage tárolóban** a legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-355">In the **Editor** for the Data Factory, click **New dataset** button on the toolbar and click **Azure Blob storage** from the drop-down menu.</span></span>
2. <span data-ttu-id="3c4b6-356">A jobb oldali JSON cserélje le a következő JSON kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="3c4b6-356">Replace the JSON in the right pane with the following JSON snippet:</span></span>

    ```json
    {
       "name": "InputDataset",
       "properties": {
           "type": "AzureBlob",
           "linkedServiceName": "AzureStorageLinkedService",
           "typeProperties": {
               "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
               "format": {
                   "type": "TextFormat"
               },
               "partitionedBy": [
                   {
                       "name": "Year",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "yyyy"
                       }
                   },
                   {
                       "name": "Month",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "MM"
                       }
                   },
                   {
                       "name": "Day",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "dd"
                       }
                   },
                   {
                       "name": "Hour",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "HH"
                       }
                   }
               ]
           },
           "availability": {
               "frequency": "Hour",
               "interval": 1
           },
           "external": true,
           "policy": {}
       }
    }
    ```

    <span data-ttu-id="3c4b6-357">Ez a forgatókönyv a kezdő időpont későbbi részében hozzon létre egy folyamatot: 2015-11-16T00:00:00Z és záró idő: 2015-11-16T05:00:00Z.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-357">You create a pipeline later in this walkthrough with start time: 2015-11-16T00:00:00Z and end time: 2015-11-16T05:00:00Z.</span></span> <span data-ttu-id="3c4b6-358">Adatok ütemezett **óránkénti**, így 5 bemeneti/kimeneti szeletek (közötti **00**: 00:00 -\> **05**: 00:00).</span><span class="sxs-lookup"><span data-stu-id="3c4b6-358">It is scheduled to produce data **hourly**, so there are 5 input/output slices (between **00**:00:00 -\> **05**:00:00).</span></span>

    <span data-ttu-id="3c4b6-359">A **gyakoriság** és **időköz** az bemeneti adatkészlet állítsa **óra** és **1**, ami azt jelenti, hogy a bemeneti szelet elérhető óránként.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-359">The **frequency** and **interval** for the input dataset is set to **Hour** and **1**, which means that the input slice is available hourly.</span></span>

    <span data-ttu-id="3c4b6-360">Az alábbiakban az indítási idejének az egyes szeletek, ami által képviselt **SliceStart** a fenti JSON-részlet rendszer változót.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-360">Here are the start times for each slice, which is represented by **SliceStart** system variable in the above JSON snippet.</span></span>

    | <span data-ttu-id="3c4b6-361">**Szelet**</span><span class="sxs-lookup"><span data-stu-id="3c4b6-361">**Slice**</span></span> | <span data-ttu-id="3c4b6-362">**Kezdési idő**</span><span class="sxs-lookup"><span data-stu-id="3c4b6-362">**Start time**</span></span>          |
    |-----------|-------------------------|
    | <span data-ttu-id="3c4b6-363">1</span><span class="sxs-lookup"><span data-stu-id="3c4b6-363">1</span></span>         | <span data-ttu-id="3c4b6-364">2015-11-16T**00**: 00:00</span><span class="sxs-lookup"><span data-stu-id="3c4b6-364">2015-11-16T**00**:00:00</span></span> |
    | <span data-ttu-id="3c4b6-365">2</span><span class="sxs-lookup"><span data-stu-id="3c4b6-365">2</span></span>         | <span data-ttu-id="3c4b6-366">2015-11-16T**01**: 00:00</span><span class="sxs-lookup"><span data-stu-id="3c4b6-366">2015-11-16T**01**:00:00</span></span> |
    | <span data-ttu-id="3c4b6-367">3</span><span class="sxs-lookup"><span data-stu-id="3c4b6-367">3</span></span>         | <span data-ttu-id="3c4b6-368">2015-11-16T**02**: 00:00</span><span class="sxs-lookup"><span data-stu-id="3c4b6-368">2015-11-16T**02**:00:00</span></span> |
    | <span data-ttu-id="3c4b6-369">4</span><span class="sxs-lookup"><span data-stu-id="3c4b6-369">4</span></span>         | <span data-ttu-id="3c4b6-370">2015-11-16T**03**: 00:00</span><span class="sxs-lookup"><span data-stu-id="3c4b6-370">2015-11-16T**03**:00:00</span></span> |
    | <span data-ttu-id="3c4b6-371">5</span><span class="sxs-lookup"><span data-stu-id="3c4b6-371">5</span></span>         | <span data-ttu-id="3c4b6-372">2015-11-16T**04**: 00:00</span><span class="sxs-lookup"><span data-stu-id="3c4b6-372">2015-11-16T**04**:00:00</span></span> |

    <span data-ttu-id="3c4b6-373">A **folderPath** számítja ki a szelet kezdési idő az év, hónap, nap és óra része (**SliceStart**).</span><span class="sxs-lookup"><span data-stu-id="3c4b6-373">The **folderPath** is calculated by using the year, month, day, and hour part of the slice start time (**SliceStart**).</span></span> <span data-ttu-id="3c4b6-374">Ezért ez hogyan egy bemeneti mappa szelet van leképezve.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-374">Therefore, here is how an input folder is mapped to a slice.</span></span>

    | <span data-ttu-id="3c4b6-375">**Szelet**</span><span class="sxs-lookup"><span data-stu-id="3c4b6-375">**Slice**</span></span> | <span data-ttu-id="3c4b6-376">**Kezdési idő**</span><span class="sxs-lookup"><span data-stu-id="3c4b6-376">**Start time**</span></span>          | <span data-ttu-id="3c4b6-377">**Bemeneti mappa**</span><span class="sxs-lookup"><span data-stu-id="3c4b6-377">**Input folder**</span></span>  |
    |-----------|-------------------------|-------------------|
    | <span data-ttu-id="3c4b6-378">1</span><span class="sxs-lookup"><span data-stu-id="3c4b6-378">1</span></span>         | <span data-ttu-id="3c4b6-379">2015-11-16T**00**: 00:00</span><span class="sxs-lookup"><span data-stu-id="3c4b6-379">2015-11-16T**00**:00:00</span></span> | <span data-ttu-id="3c4b6-380">2015-11-16-**00**</span><span class="sxs-lookup"><span data-stu-id="3c4b6-380">2015-11-16-**00**</span></span> |
    | <span data-ttu-id="3c4b6-381">2</span><span class="sxs-lookup"><span data-stu-id="3c4b6-381">2</span></span>         | <span data-ttu-id="3c4b6-382">2015-11-16T**01**: 00:00</span><span class="sxs-lookup"><span data-stu-id="3c4b6-382">2015-11-16T**01**:00:00</span></span> | <span data-ttu-id="3c4b6-383">2015-11-16-**01**</span><span class="sxs-lookup"><span data-stu-id="3c4b6-383">2015-11-16-**01**</span></span> |
    | <span data-ttu-id="3c4b6-384">3</span><span class="sxs-lookup"><span data-stu-id="3c4b6-384">3</span></span>         | <span data-ttu-id="3c4b6-385">2015-11-16T**02**: 00:00</span><span class="sxs-lookup"><span data-stu-id="3c4b6-385">2015-11-16T**02**:00:00</span></span> | <span data-ttu-id="3c4b6-386">2015-11-16-**02**</span><span class="sxs-lookup"><span data-stu-id="3c4b6-386">2015-11-16-**02**</span></span> |
    | <span data-ttu-id="3c4b6-387">4</span><span class="sxs-lookup"><span data-stu-id="3c4b6-387">4</span></span>         | <span data-ttu-id="3c4b6-388">2015-11-16T**03**: 00:00</span><span class="sxs-lookup"><span data-stu-id="3c4b6-388">2015-11-16T**03**:00:00</span></span> | <span data-ttu-id="3c4b6-389">2015-11-16-**03**</span><span class="sxs-lookup"><span data-stu-id="3c4b6-389">2015-11-16-**03**</span></span> |
    | <span data-ttu-id="3c4b6-390">5</span><span class="sxs-lookup"><span data-stu-id="3c4b6-390">5</span></span>         | <span data-ttu-id="3c4b6-391">2015-11-16T**04**: 00:00</span><span class="sxs-lookup"><span data-stu-id="3c4b6-391">2015-11-16T**04**:00:00</span></span> | <span data-ttu-id="3c4b6-392">2015-11-16-**04**</span><span class="sxs-lookup"><span data-stu-id="3c4b6-392">2015-11-16-**04**</span></span> |

1. <span data-ttu-id="3c4b6-393">Kattintson a **telepítés** létrehozása és telepítése az eszköztáron a **InputDataset** tábla.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-393">Click **Deploy** on the toolbar to create and deploy the **InputDataset** table.</span></span>

#### <a name="create-output-dataset"></a><span data-ttu-id="3c4b6-394">Kimeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="3c4b6-394">Create output dataset</span></span>
<span data-ttu-id="3c4b6-395">Ebben a lépésben létrehoz egy másik dataset a kimeneti adatok AzureBlob típusú.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-395">In this step, you create another dataset of type AzureBlob to represent the output data.</span></span>

1. <span data-ttu-id="3c4b6-396">Az a **szerkesztő** a Data Factory kattintson **új adatkészlet** gomb az eszköztáron és a kattintson **Azure Blob Storage tárolóban** a legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-396">In the **Editor** for the Data Factory, click **New dataset** button on the toolbar and click **Azure Blob storage** from the drop-down menu.</span></span>
2. <span data-ttu-id="3c4b6-397">A jobb oldali JSON cserélje le a következő JSON kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="3c4b6-397">Replace the JSON in the right pane with the following JSON snippet:</span></span>

    ```json
    {
       "name": "OutputDataset",
       "properties": {
           "type": "AzureBlob",
           "linkedServiceName": "AzureStorageLinkedService",
           "typeProperties": {
               "fileName": "{slice}.txt",
               "folderPath": "mycontainer/outputfolder",
               "partitionedBy": [
                   {
                       "name": "slice",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "yyyy-MM-dd-HH"
                       }
                   }
               ]
           },
           "availability": {
               "frequency": "Hour",
               "interval": 1
           }
       }
    }
    ```

    <span data-ttu-id="3c4b6-398">Egy kimeneti blob/fájl az egyes bemeneti szeletek jön létre.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-398">An output blob/file is generated for each input slice.</span></span> <span data-ttu-id="3c4b6-399">Ez hogyan kimeneti fájl neve az egyes szeletek.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-399">Here is how an output file is named for each slice.</span></span> <span data-ttu-id="3c4b6-400">A kimeneti fájlok akkor jönnek létre, egy kimeneti mappában: `mycontainer\\outputfolder`.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-400">All the output files are generated in one output folder: `mycontainer\\outputfolder`.</span></span>

    | <span data-ttu-id="3c4b6-401">**Szelet**</span><span class="sxs-lookup"><span data-stu-id="3c4b6-401">**Slice**</span></span> | <span data-ttu-id="3c4b6-402">**Kezdési idő**</span><span class="sxs-lookup"><span data-stu-id="3c4b6-402">**Start time**</span></span>          | <span data-ttu-id="3c4b6-403">**Kimeneti fájlja**</span><span class="sxs-lookup"><span data-stu-id="3c4b6-403">**Output file**</span></span>       |
    |-----------|-------------------------|-----------------------|
    | <span data-ttu-id="3c4b6-404">1</span><span class="sxs-lookup"><span data-stu-id="3c4b6-404">1</span></span>         | <span data-ttu-id="3c4b6-405">2015-11-16T**00**: 00:00</span><span class="sxs-lookup"><span data-stu-id="3c4b6-405">2015-11-16T**00**:00:00</span></span> | <span data-ttu-id="3c4b6-406">2015-11-16 -**00. txt**</span><span class="sxs-lookup"><span data-stu-id="3c4b6-406">2015-11-16-**00.txt**</span></span> |
    | <span data-ttu-id="3c4b6-407">2</span><span class="sxs-lookup"><span data-stu-id="3c4b6-407">2</span></span>         | <span data-ttu-id="3c4b6-408">2015-11-16T**01**: 00:00</span><span class="sxs-lookup"><span data-stu-id="3c4b6-408">2015-11-16T**01**:00:00</span></span> | <span data-ttu-id="3c4b6-409">2015-11-16 -**01. txt**</span><span class="sxs-lookup"><span data-stu-id="3c4b6-409">2015-11-16-**01.txt**</span></span> |
    | <span data-ttu-id="3c4b6-410">3</span><span class="sxs-lookup"><span data-stu-id="3c4b6-410">3</span></span>         | <span data-ttu-id="3c4b6-411">2015-11-16T**02**: 00:00</span><span class="sxs-lookup"><span data-stu-id="3c4b6-411">2015-11-16T**02**:00:00</span></span> | <span data-ttu-id="3c4b6-412">2015-11-16 -**02. txt**</span><span class="sxs-lookup"><span data-stu-id="3c4b6-412">2015-11-16-**02.txt**</span></span> |
    | <span data-ttu-id="3c4b6-413">4</span><span class="sxs-lookup"><span data-stu-id="3c4b6-413">4</span></span>         | <span data-ttu-id="3c4b6-414">2015-11-16T**03**: 00:00</span><span class="sxs-lookup"><span data-stu-id="3c4b6-414">2015-11-16T**03**:00:00</span></span> | <span data-ttu-id="3c4b6-415">2015-11-16 -**03. txt**</span><span class="sxs-lookup"><span data-stu-id="3c4b6-415">2015-11-16-**03.txt**</span></span> |
    | <span data-ttu-id="3c4b6-416">5</span><span class="sxs-lookup"><span data-stu-id="3c4b6-416">5</span></span>         | <span data-ttu-id="3c4b6-417">2015-11-16T**04**: 00:00</span><span class="sxs-lookup"><span data-stu-id="3c4b6-417">2015-11-16T**04**:00:00</span></span> | <span data-ttu-id="3c4b6-418">2015-11-16 -**04. txt**</span><span class="sxs-lookup"><span data-stu-id="3c4b6-418">2015-11-16-**04.txt**</span></span> |

    <span data-ttu-id="3c4b6-419">Ne feledje, hogy egy bemeneti mappában lévő összes fájl (például: 2015-11-16-00) részei a szelet, amelynél a kezdő időpont: 2015-11-16-00.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-419">Remember that all the files in an input folder (for example: 2015-11-16-00) are part of a slice with the start time: 2015-11-16-00.</span></span> <span data-ttu-id="3c4b6-420">A szelet feldolgozása után az egyéni tevékenység minden fájl megvizsgálja, és hozza létre a keresési kifejezés ("Microsoft") előfordulásainak száma a kimeneti fájl egy sorban.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-420">When this slice is processed, the custom activity scans through each file and produces a line in the output file with the number of occurrences of search term (“Microsoft”).</span></span> <span data-ttu-id="3c4b6-421">Ha a mappa 2015-11-16-00 három fájlok, vannak-e három sort a kimeneti fájl: 2015-11-16-00.txt.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-421">If there are three files in the folder 2015-11-16-00, there are three lines in the output file: 2015-11-16-00.txt.</span></span>

1. <span data-ttu-id="3c4b6-422">Kattintson a **telepítés** létrehozása és telepítése az eszköztáron a **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-422">Click **Deploy** on the toolbar to create and deploy the **OutputDataset**.</span></span>

#### <a name="step-4-create-and-run-the-pipeline-with-custom-activity"></a><span data-ttu-id="3c4b6-423">4. lépés: Hozzon létre, és futtatjuk a folyamatot az egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="3c4b6-423">Step 4: Create and run the pipeline with custom activity</span></span>
<span data-ttu-id="3c4b6-424">Ebben a lépésben létrehoz egy folyamatot egy tevékenységet, a korábban létrehozott egyéni tevékenység.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-424">In this step, you create a pipeline with one activity, the custom activity you created earlier.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3c4b6-425">Ha még nem töltötte fel a **file.txt** a blob-tároló mappákat a felhasználótól, ehhez a folyamat létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-425">If you haven't uploaded the **file.txt** to input folders in the blob container, do so before creating the pipeline.</span></span> <span data-ttu-id="3c4b6-426">A **isPaused** tulajdonsága hamis az adatcsatorna JSON-NÁ, így a folyamat a rendszer azonnal futtatja a **start** dátuma a múltban van.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-426">The **isPaused** property is set to false in the pipeline JSON, so the pipeline runs immediately as the **start** date is in the past.</span></span>
>
>

1. <span data-ttu-id="3c4b6-427">Kattintson a Data Factory Editor **új adatcsatorna** a parancssávon.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-427">In the Data Factory Editor, click **New pipeline** on the command bar.</span></span> <span data-ttu-id="3c4b6-428">Ha nem látja a parancsot, kattintson a **... Három (pont)**  látja.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-428">If you do not see the command, click **... (Ellipsis)** to see it.</span></span>
2. <span data-ttu-id="3c4b6-429">A jobb oldali JSON cserélje le a következő JSON-parancsfájl:</span><span class="sxs-lookup"><span data-stu-id="3c4b6-429">Replace the JSON in the right pane with the following JSON script:</span></span>

    ```json
    {
       "name": "PipelineCustom",
       "properties": {
           "description": "Use custom activity",
           "activities": [
               {
                   "type": "DotNetActivity",
                   "typeProperties": {
                       "assemblyName": "MyDotNetActivity.dll",
                       "entryPoint": "MyDotNetActivityNS.MyDotNetActivity",
                       "packageLinkedService": "AzureStorageLinkedService",
                       "packageFile": "customactivitycontainer/MyDotNetActivity.zip"
                   },
                   "inputs": [
                       {
                           "name": "InputDataset"
                       }
                   ],
                   "outputs": [
                       {
                           "name": "OutputDataset"
                       }
                   ],
                   "policy": {
                       "timeout": "00:30:00",
                       "concurrency": 5,
                       "retry": 3
                   },
                   "scheduler": {
                       "frequency": "Hour",
                       "interval": 1
                   },
                   "name": "MyDotNetActivity",
                   "linkedServiceName": "AzureBatchLinkedService"
               }
           ],
           "start": "2015-11-16T00:00:00Z",
           "end": "2015-11-16T05:00:00Z",
           "isPaused": false
      }
    }
    ```
   <span data-ttu-id="3c4b6-430">Vegye figyelembe a következő szempontokat:</span><span class="sxs-lookup"><span data-stu-id="3c4b6-430">Note the following points:</span></span>

   * <span data-ttu-id="3c4b6-431">Csak egy tevékenység van folyamatban, és típusa, amely nem: **DotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-431">There is only one activity in the pipeline and that is of type: **DotNetActivity**.</span></span>
   * <span data-ttu-id="3c4b6-432">**AssemblyName** értéke a dll-fájl neve: **MyDotNetActivity.dll**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-432">**AssemblyName** is set to the name of the DLL: **MyDotNetActivity.dll**.</span></span>
   * <span data-ttu-id="3c4b6-433">**EntryPoint** értéke **MyDotNetActivityNS.MyDotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-433">**EntryPoint** is set to **MyDotNetActivityNS.MyDotNetActivity**.</span></span> <span data-ttu-id="3c4b6-434">Alapvetően van \<névtér\>.\< osztálynév\> a kódban.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-434">It is basically \<namespace\>.\<classname\> in your code.</span></span>
   * <span data-ttu-id="3c4b6-435">**PackageLinkedService** értéke **StorageLinkedService** a blob-tároló, amely tartalmazza az egyéni tevékenység zip-fájl mutat.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-435">**PackageLinkedService** is set to **StorageLinkedService** that points to the blob storage that contains the custom activity zip file.</span></span> <span data-ttu-id="3c4b6-436">Különböző Azure Storage-fiókok használatakor a bemeneti/kimeneti fájlok és az egyéni tevékenység zip-fájlt kell létrehoznia egy másik Azure tárolás társított szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-436">If you are using different Azure Storage accounts for input/output files and the custom activity zip file, you have to create another Azure Storage linked service.</span></span> <span data-ttu-id="3c4b6-437">Ez a cikk feltételezi, hogy a azonos Azure Storage-fiókot használja.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-437">This article assumes that you are using the same Azure Storage account.</span></span>
   * <span data-ttu-id="3c4b6-438">**PackageFile** értéke **customactivitycontainer/MyDotNetActivity.zip**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-438">**PackageFile** is set to **customactivitycontainer/MyDotNetActivity.zip**.</span></span> <span data-ttu-id="3c4b6-439">A a formátumban: \<containerforthezip\>/\<nameofthezip.zip\>.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-439">It is in the format: \<containerforthezip\>/\<nameofthezip.zip\>.</span></span>
   * <span data-ttu-id="3c4b6-440">Az egyéni tevékenység veszi **InputDataset** bemenetként és **OutputDataset** output típusúként.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-440">The custom activity takes **InputDataset** as input and **OutputDataset** as output.</span></span>
   * <span data-ttu-id="3c4b6-441">A **linkedServiceName** tulajdonság az egyéni tevékenység mutat, a **AzureBatchLinkedService**, amely közli az Azure Data Factory, amely az egyéni tevékenység Azure Batch futtatásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-441">The **linkedServiceName** property of the custom activity points to the **AzureBatchLinkedService**, which tells Azure Data Factory that the custom activity needs to run on Azure Batch.</span></span>
   * <span data-ttu-id="3c4b6-442">A **egyidejűségi** beállítás fontos.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-442">The **concurrency** setting is important.</span></span> <span data-ttu-id="3c4b6-443">Ha használja az alapértelmezett érték, amely 1, még akkor is, ha 2 vagy több számítási csomópontok az Azure Batch-készlet, a rendszer feldolgozása egymás után.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-443">If you use the default value, which is 1, even if you have 2 or more compute nodes in the Azure Batch pool, the slices are processed one after another.</span></span> <span data-ttu-id="3c4b6-444">Ezért nem készítésének Azure kötegelt párhuzamos feldolgozási képességének kihasználása.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-444">Therefore, you are not taking advantage of the parallel processing capability of Azure Batch.</span></span> <span data-ttu-id="3c4b6-445">Ha **egyidejűségi** magasabb értékre, mondja ki 2, az azt jelenti, hogy a két szeletek (felel meg az Azure Batch két feladatot) tudja feldolgozni az egy időben, ebben az esetben, mind a virtuális gépek az Azure Batch készlet ki van használva.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-445">If you set **concurrency** to a higher value, say 2, it means that two slices (corresponds to two tasks in Azure Batch) can be processed at the same time, in which case, both the VMs in the Azure Batch pool are utilized.</span></span> <span data-ttu-id="3c4b6-446">Ezért állítsa be megfelelően az egyidejű tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-446">Therefore, set the concurrency property appropriately.</span></span>
   * <span data-ttu-id="3c4b6-447">Alapértelmezés szerint egyetlen virtuális gép (szelet) csak egy feladat végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-447">Only one task (slice) is executed on a VM at any point by default.</span></span> <span data-ttu-id="3c4b6-448">Az oka, hogy a alapértelmezés szerint a **virtuális gépenként feladatok maximális** az Azure Batch-készlet 1 értékre van állítva.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-448">The reason is that, by default, the **Maximum tasks per VM** is set to 1 for an Azure Batch pool.</span></span> <span data-ttu-id="3c4b6-449">Előfeltételek részeként létrehozott készlet e tulajdonsága 2, így a két adat-előállító szeletek futtathat a virtuális gép egyszerre.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-449">As part of prerequisites, you created a pool with this property set to 2, so two Data Factory slices can be running on a VM at the same time.</span></span>

    -   <span data-ttu-id="3c4b6-450">**isPaused** tulajdonsága hamis értékre van beállítva, alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-450">**isPaused** property is set to false by default.</span></span> <span data-ttu-id="3c4b6-451">A folyamat a rendszer azonnal futtatja ebben a példában a szeletek indítsa el a régebbi, mert.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-451">The pipeline runs immediately in this example because the slices start in the past.</span></span> <span data-ttu-id="3c4b6-452">Ez a tulajdonság igaz értékre a feldolgozási sor szüneteltetése, és állítsa vissza újraindítására hamis beállíthatja.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-452">You can set this property to true to pause the pipeline and set it back to false to restart.</span></span>

    -   <span data-ttu-id="3c4b6-453">A **start** idő és **end** alkalommal öt órát egymástól, szeletek előállított óránkénti, így öt szeletek hozzák létre a folyamatot.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-453">The **start** time and **end** times are five hours apart and slices are produced hourly, so five slices are produced by the pipeline.</span></span>

1. <span data-ttu-id="3c4b6-454">A folyamat üzembe helyezéséhez kattintson a parancssáv **Deploy** (Üzembe helyezés) elemére.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-454">Click **Deploy** on the command bar to deploy the pipeline.</span></span>

#### <a name="step-5-test-the-pipeline"></a><span data-ttu-id="3c4b6-455">5. lépés: A folyamat tesztelése</span><span class="sxs-lookup"><span data-stu-id="3c4b6-455">Step 5: Test the pipeline</span></span>
<span data-ttu-id="3c4b6-456">Ebben a lépésben a folyamat a bemeneti mappákba fájlok ejtésével tesztelése.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-456">In this step, you test the pipeline by dropping files into the input folders.</span></span> <span data-ttu-id="3c4b6-457">Kezdjük azokkal a folyamat tesztelése egy bemeneti mappa minden egy fájlba.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-457">Let’s start with testing the pipeline with one file per one input folder.</span></span>

1. <span data-ttu-id="3c4b6-458">A Data Factory panelen az Azure portálon kattintson **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-458">In the Data Factory blade in the Azure portal, click **Diagram**.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image10.png)
2. <span data-ttu-id="3c4b6-459">A diagram nézetben kattintson duplán a bemeneti adatkészlet: **InputDataset**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-459">In the diagram view, double-click input dataset: **InputDataset**.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image11.png)
3. <span data-ttu-id="3c4b6-460">Megjelenik a **InputDataset** mind az öt panelről szeletek készen áll.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-460">You should see the **InputDataset** blade with all five slices ready.</span></span> <span data-ttu-id="3c4b6-461">Figyelje meg a **SZELET kezdete** és **SZELET BEFEJEZÉSÉNEK** az egyes szeletek.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-461">Notice the **SLICE START TIME** and **SLICE END TIME** for each slice.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image12.png)
4. <span data-ttu-id="3c4b6-462">Az a **diagramnézet**, ekkor kattintson a **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-462">In the **Diagram View**, now click **OutputDataset**.</span></span>
5. <span data-ttu-id="3c4b6-463">Megtekintheti, hogy a az öt kimeneti szeletek kész állapotban vannak, ha azok már előállítani.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-463">You should see that the five output slices are in the Ready state if they have already been produced.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image13.png)
6. <span data-ttu-id="3c4b6-464">Az Azure portál segítségével megtekintheti a **feladatok** társított a **szeletek** , és ellenőrizze, milyen VM minden szelet futott.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-464">Use Azure portal to view the **tasks** associated with the **slices** and see what VM each slice ran on.</span></span> <span data-ttu-id="3c4b6-465">Lásd: [adat-előállító és kötegelt integrációs](#data-factory-and-batch-integration) című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-465">See [Data Factory and Batch integration](#data-factory-and-batch-integration) section for details.</span></span>
7. <span data-ttu-id="3c4b6-466">A kimenő fájlok kell megjelennie a `outputfolder` a `mycontainer` az Azure blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-466">You should see the output files in the `outputfolder` of `mycontainer` in your Azure blob storage.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image15.png)

   <span data-ttu-id="3c4b6-467">Meg kell jelennie egy, az egyes bemeneti szeletek öt kimeneti fájlokat.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-467">You should see five output files, one for each input slice.</span></span> <span data-ttu-id="3c4b6-468">A kimeneti fájl minden egyes rendelkeznie kell a következő kimeneti hasonló tartalom:</span><span class="sxs-lookup"><span data-stu-id="3c4b6-468">Each of the output file should have content similar to the following output:</span></span>

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
    ```
   <span data-ttu-id="3c4b6-469">A következő ábra bemutatja, hogyan képezi le a Data Factory szeletek Azure Batch feladatai számára.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-469">The following diagram illustrates how the Data Factory slices map to tasks in Azure Batch.</span></span> <span data-ttu-id="3c4b6-470">Ebben a példában a szelet csak egy futtató rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-470">In this example, a slice has only one run.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image16.png)
8. <span data-ttu-id="3c4b6-471">Most próbáljuk meg egy mappában több fájlokkal.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-471">Now, let’s try with multiple files in a folder.</span></span> <span data-ttu-id="3c4b6-472">Fájlok létrehozása: **fájl2.ref fájllal**, **file3.txt**, **file4.txt**, és **file5.txt** az ugyanahhoz a tartalomhoz, mint a mappában file.txt: **2015-11-06-01**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-472">Create files: **file2.txt**, **file3.txt**, **file4.txt**, and **file5.txt** with the same content as in file.txt in the folder: **2015-11-06-01**.</span></span>
9. <span data-ttu-id="3c4b6-473">A kimeneti mappában **törlése** a kimeneti fájl: **2015-11-16-01.txt**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-473">In the output folder, **delete** the output file: **2015-11-16-01.txt**.</span></span>
10. <span data-ttu-id="3c4b6-474">Most, a a **OutputDataset** panelen kattintson a jobb gombbal a szeletre **SZELET Kezdés időpontja** beállítása **11/16/2015 01:00:00-kor**, és kattintson **futtassa** újrafuttatása/újra-process a szeletet a.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-474">Now, in the **OutputDataset** blade, right-click the slice with **SLICE START TIME** set to **11/16/2015 01:00:00 AM**, and click **Run** to rerun/re-process the slice.</span></span> <span data-ttu-id="3c4b6-475">A szelet, öt fájlokat tartalmaz egy fájl helyett.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-475">Now, the slice has five files instead of one file.</span></span>

    ![](./media/data-factory-data-processing-using-batch/image17.png)
11. <span data-ttu-id="3c4b6-476">Után futtatja a szelet állapota pedig **készen**, ellenőrizze a tartalmat a kimeneti fájl a szeletre vonatkozó (**2015-11-16-01.txt**) található a `outputfolder` a `mycontainer` a blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-476">After the slice runs and its status is **Ready**, verify the content in the output file for this slice (**2015-11-16-01.txt**) in the `outputfolder` of `mycontainer` in your blob storage.</span></span> <span data-ttu-id="3c4b6-477">Egy sor minden fájlhoz a szelet kell lennie.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-477">There should be a line for each file of the slice.</span></span>

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file2.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file3.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file4.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file5.txt.
    ```

> [!NOTE]
> <span data-ttu-id="3c4b6-478">Ha, nem törölte a a kimeneti fájl 2015-11-16-01.txt előtt öt bemeneti fájlokkal, látható az előző szelet futni, hanem egy sor és az aktuális szelet futni, hanem öt sorban.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-478">If you did not delete the output file 2015-11-16-01.txt before trying with five input files, you see one line from the previous slice run and five lines from the current slice run.</span></span> <span data-ttu-id="3c4b6-479">Alapértelmezés szerint a rendszer hozzáfűzi a tartalom kimeneti fájl, ha már létezik.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-479">By default, the content is appended to output file if it already exists.</span></span>
>
>

#### <a name="data-factory-and-batch-integration"></a><span data-ttu-id="3c4b6-480">Adat-előállító és kötegelt integrációja</span><span class="sxs-lookup"><span data-stu-id="3c4b6-480">Data Factory and Batch integration</span></span>
<span data-ttu-id="3c4b6-481">A Data Factory szolgáltatásnak hoz létre egy feladatot az Azure Batch nevű: `adf-poolname:job-xxx`.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-481">The Data Factory service creates a job in Azure Batch with the name: `adf-poolname:job-xxx`.</span></span>

![Az Azure Data Factory - kötegelt feladatok](media/data-factory-data-processing-using-batch/data-factory-batch-jobs.png)

<span data-ttu-id="3c4b6-483">A feladat egy feladat minden egyes tevékenység futtatásához a szelet jön létre.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-483">A task in the job is created for each activity run of a slice.</span></span> <span data-ttu-id="3c4b6-484">Ha készen áll a feldolgozásra 10 szeletek, a feladat 10 feladatok jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-484">If there are 10 slices ready to be processed, 10 tasks are created in the job.</span></span> <span data-ttu-id="3c4b6-485">A párhuzamosan futó, ha több számítási csomópontok a készlet több szelet lehet.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-485">You can have more than one slice running in parallel if you have multiple compute nodes in the pool.</span></span> <span data-ttu-id="3c4b6-486">Ha a maximális tevékenységek maximális száma a számítási csomópont értéke > 1, az azonos számítási futó egynél több szelet lehet.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-486">If the maximum tasks per compute node is set to > 1, there can be more than one slice running on the same compute.</span></span>

<span data-ttu-id="3c4b6-487">Ebben a példában nincsenek öt szeletek, így öt feladatok Azure kötegben.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-487">In this example, there are five slices, so five tasks in Azure Batch.</span></span> <span data-ttu-id="3c4b6-488">Az a **egyidejűségi** beállítása **5** az adatcsatorna JSON-t Azure Data Factory és **virtuális gépenként feladatok maximális** beállítása **2** az Azure Batch-készlet **2** virtuális gépeket, a feladatok futtatása gyors (Ellenőrizze a kezdő és záró időpontjának feladatok).</span><span class="sxs-lookup"><span data-stu-id="3c4b6-488">With the **concurrency** set to **5** in the pipeline JSON in Azure Data Factory and **Maximum tasks per VM** set to **2** in Azure Batch pool with **2** VMs, the tasks runs fast (check start and end times for tasks).</span></span>

<span data-ttu-id="3c4b6-489">A portál segítségével fogja megtekinteni a kötegelt és a társított feladatokat a **szeletek** , és ellenőrizze, milyen VM minden szelet futott.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-489">Use the portal to view the Batch job and its tasks that are associated with the **slices** and see what VM each slice ran on.</span></span>

![Az Azure Data Factory - kötegelt munka feladatai](media/data-factory-data-processing-using-batch/data-factory-batch-job-tasks.png)

### <a name="debug-the-pipeline"></a><span data-ttu-id="3c4b6-491">A folyamat hibakeresése</span><span class="sxs-lookup"><span data-stu-id="3c4b6-491">Debug the pipeline</span></span>
<span data-ttu-id="3c4b6-492">A hibakeresés néhány alapvető technikából áll:</span><span class="sxs-lookup"><span data-stu-id="3c4b6-492">Debugging consists of a few basic techniques:</span></span>

1. <span data-ttu-id="3c4b6-493">Ha nincs beállítva a bemeneti szelet **készen**, győződjön meg arról, hogy a bemeneti gyökérmappa-szerkezetében helyességéről, valamint file.txt szerepel a bemeneti mappákat.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-493">If the input slice is not set to **Ready**, confirm that the input folder structure is correct and file.txt exists in the input folders.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image3.png)
2. <span data-ttu-id="3c4b6-494">Az a **Execute** metódus az egyéni tevékenység, használja a **IActivityLogger** objektum, amely segít a problémák elhárításához adatok naplózására.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-494">In the **Execute** method of your custom activity, use the **IActivityLogger** object to log information that helps you troubleshoot issues.</span></span> <span data-ttu-id="3c4b6-495">A naplózott üzenet jelenik meg a felhasználó\_0. naplófájlt.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-495">The logged messages show up in the user\_0.log file.</span></span>

   <span data-ttu-id="3c4b6-496">Az a **OutputDataset** panelen a szelet megtekintéséhez kattintson a **ADATSZELET** , hogy a szelet paneljét.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-496">In the **OutputDataset** blade, click the slice to see the **DATA SLICE** blade for that slice.</span></span> <span data-ttu-id="3c4b6-497">Látni **tevékenységek** az adott szeletek.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-497">You see **activity runs** for that slice.</span></span> <span data-ttu-id="3c4b6-498">Meg kell jelennie egy tevékenység Futtatás újrapróbálása.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-498">You should see one activity run for the slice.</span></span> <span data-ttu-id="3c4b6-499">Ha **futtatása** a parancssávon, akkor kezdhet a azonos szelet futtassa egy másik tevékenység.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-499">If you click **Run** in the command bar, you can start another activity run for the same slice.</span></span>

   <span data-ttu-id="3c4b6-500">Ha a tevékenység futtatása gombra kattint, megjelenik a **tevékenység futtatása részletei** naplófájlok listáját tartalmazó panel.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-500">When you click the activity run, you see the **ACTIVITY RUN DETAILS** blade with a list of log files.</span></span> <span data-ttu-id="3c4b6-501">A naplózott üzeneteket látja a **felhasználói\_0. napló** fájlt.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-501">You see logged messages in the **user\_0.log** file.</span></span> <span data-ttu-id="3c4b6-502">Ha hiba történik, megjelenik az három tevékenység fut oka az újrapróbálkozások maximális számát a feldolgozási sor/tevékenységben JSON 3 értékre van beállítva.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-502">When an error occurs, you see three activity runs because the retry count is set to 3 in the pipeline/activity JSON.</span></span> <span data-ttu-id="3c4b6-503">Amikor a tevékenység futtatása gombra kattint, láthatja a hiba elhárítása érdekében tekintse át a naplófájlokat.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-503">When you click the activity run, you see the log files that you can review to troubleshoot the error.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image18.png)

   <span data-ttu-id="3c4b6-504">A naplófájlok, kattintson a **felhasználói-0.log**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-504">In the list of log files, click the **user-0.log**.</span></span> <span data-ttu-id="3c4b6-505">A jobb oldali panelen a következők használatával eredményeit a **IActivityLogger.Write** metódust.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-505">In the right panel are the results of using the **IActivityLogger.Write** method.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image19.png)

   <span data-ttu-id="3c4b6-506">Ellenőrizze a rendszer-0.log rendszer hibaüzeneteket és kivételeket.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-506">Check system-0.log for any system error messages and exceptions.</span></span>

    ```
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Loading assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Creating an instance of MyDotNetActivityNS.MyDotNetActivity from assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Executing Module
    
    Trace\_T\_D\_12/6/2015 1:43:38 AM\_T\_D\_\_T\_D\_Information\_T\_D\_0\_T\_D\_Activity e3817da0-d843-4c5c-85c6-40ba7424dce2 finished successfully
    ```
3. <span data-ttu-id="3c4b6-507">Tartalmazza a **PDB** , hogy a hiba részletei információkat, mint a zip-fájlban szereplő fájl **hívási verem** Ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-507">Include the **PDB** file in the zip file so that the error details have information such as **call stack** when an error occurs.</span></span>
4. <span data-ttu-id="3c4b6-508">Az egyéni tevékenység zip-fájlban található összes fájl el kell érnie a **felső szintű** egyetlen almappát.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-508">All the files in the zip file for the custom activity must be at the **top level** with no subfolders.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image20.png)
5. <span data-ttu-id="3c4b6-509">Győződjön meg arról, hogy a **assemblyName** (MyDotNetActivity.dll) **belépési pont** (MyDotNetActivityNS.MyDotNetActivity) **packageFile** (customactivitycontainer/MyDotNetActivity.zip), és **packageLinkedService** (kell mutatnia, amely a zip-fájl tartalmazza az Azure blob-tároló) helyes az értékük értékre van beállítva.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-509">Ensure that the **assemblyName** (MyDotNetActivity.dll), **entryPoint** (MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer/MyDotNetActivity.zip), and **packageLinkedService** (should point to the Azure blob storage that contains the zip file) are set to correct values.</span></span>
6. <span data-ttu-id="3c4b6-510">Ha kijavított egy hibát, és újra fel szeretné dolgozni a szeletet, kattintson a jobb gombbal a szeletre az **OutputDataset** panelen, és kattintson a **Futtatás** parancsra.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-510">If you fixed an error and want to reprocess the slice, right-click the slice in the **OutputDataset** blade and click **Run**.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image21.png)

   > [!NOTE]
   > <span data-ttu-id="3c4b6-511">Megjelenik egy **tároló** az Azure Blob Storage nevű: `adfjobs`.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-511">You see a **container** in your Azure Blob storage named: `adfjobs`.</span></span> <span data-ttu-id="3c4b6-512">Ez a tároló nem törli automatikusan a rendszer, de biztonságos törlése után végzett vizsgálat a megoldás.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-512">This container is not automatically deleted, but you can safely delete it after you are done testing the solution.</span></span> <span data-ttu-id="3c4b6-513">Hasonlóképpen, a Data Factory megoldást hoz létre egy Azure Batch **feladat** nevű: `adf-\<pool ID/name\>:job-0000000001`.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-513">Similarly, the Data Factory solution creates an Azure Batch **job** named: `adf-\<pool ID/name\>:job-0000000001`.</span></span> <span data-ttu-id="3c4b6-514">Ez a feladat után ha szeretné tesztelni a megoldás törölhető.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-514">You can delete this job after you test the solution if you like.</span></span>
   >
   >
7. <span data-ttu-id="3c4b6-515">Az egyéni tevékenység nem használja a **app.config** fájlt a csomagból.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-515">The custom activity does not use the **app.config** file from your package.</span></span> <span data-ttu-id="3c4b6-516">Ezért ha a kódot a kapcsolati karakterláncok a konfigurációs fájlból olvassa be, nem működik futásidőben.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-516">Therefore, if your code reads any connection strings from the configuration file, it does not work at runtime.</span></span> <span data-ttu-id="3c4b6-517">Az ajánlott eljárás az Azure Batch használata esetén a titkos kulcsainak tárolásához egy **Azure KeyVault**, egyszerű tanúsítvány-alapú szolgáltatás használható a keyvault védelmére, és az Azure Batch-készlet tanúsítvány terjesztése.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-517">The best practice when using Azure Batch is to hold any secrets in an **Azure KeyVault**, use a certificate-based service principal to protect the keyvault, and distribute the certificate to Azure Batch pool.</span></span> <span data-ttu-id="3c4b6-518">A .NET egyéni tevékenysége ezután elérheti a titkos kulcsokat a kulcstartóból a futtatáskor.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-518">The .NET custom activity then can access secrets from the KeyVault at runtime.</span></span> <span data-ttu-id="3c4b6-519">Ez a megoldás általános egy, és minden olyan titkos kulcsot, nem csak a kapcsolati karakterlánc típusú méretezheti.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-519">This solution is a generic one and can scale to any type of secret, not just connection string.</span></span>

    <span data-ttu-id="3c4b6-520">Van egy egyszerűbb megoldást (de nem ajánlott): hozhat létre egy **Azure SQL társított szolgáltatásnak** kapcsolatikarakterlánc-beállításokat, hozzon létre egy adatkészlet által használt társított szolgáltatás, valamint az adatkészlet láncolt, egy üres bemeneti adatkészlet a Egyéni .NET tevékenység.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-520">There is an easier workaround (but not a best practice): you can create an **Azure SQL linked service** with connection string settings, create a dataset that uses the linked service, and chain the dataset as a dummy input dataset to the custom .NET activity.</span></span> <span data-ttu-id="3c4b6-521">A társított szolgáltatás kapcsolati karakterlánc az egyéni tevékenység kódban érhető el, és azt kell működnek az futásidőben.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-521">You can then access the linked service's connection string in the custom activity code and it should work fine at runtime.</span></span>  

#### <a name="extend-the-sample"></a><span data-ttu-id="3c4b6-522">A minta kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="3c4b6-522">Extend the sample</span></span>
<span data-ttu-id="3c4b6-523">Ez a minta tudhat meg többet az Azure Data Factory és az Azure Batch-szolgáltatások bővítése.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-523">You can extend this sample to learn more about Azure Data Factory and Azure Batch features.</span></span> <span data-ttu-id="3c4b6-524">Például egy eltérő időtartományt szeletek feldolgozni, hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3c4b6-524">For example, to process slices in a different time range, do the following steps:</span></span>

1. <span data-ttu-id="3c4b6-525">Adja hozzá a következő almappákhoz a `inputfolder`: 2015-11-16-05, 2015-11-16-06 201-11-16-07, 2011-11-16-08, 2015-11-16-09 és helyet adjon meg a mappákban lévő fájlok.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-525">Add the following subfolders in the `inputfolder`: 2015-11-16-05, 2015-11-16-06, 201-11-16-07, 2011-11-16-08, 2015-11-16-09 and place input files in those folders.</span></span> <span data-ttu-id="3c4b6-526">Módosítsa a folyamat befejezésének `2015-11-16T05:00:00Z` való `2015-11-16T10:00:00Z`.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-526">Change the end time for the pipeline from `2015-11-16T05:00:00Z` to `2015-11-16T10:00:00Z`.</span></span> <span data-ttu-id="3c4b6-527">Az a **diagramnézet**, kattintson duplán a **InputDataset**, és győződjön meg arról, hogy a bemeneti szeletek készen áll-e.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-527">In the **Diagram View**, double-click the **InputDataset**, and confirm that the input slices are ready.</span></span> <span data-ttu-id="3c4b6-528">Kattintson duplán a **OuptutDataset** kimeneti szeletek állapotának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-528">Double-click **OuptutDataset** to see the state of output slices.</span></span> <span data-ttu-id="3c4b6-529">Ha üzemkész állapotban vannak, ellenőrizze a kimeneti mappa kimeneti fájlok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-529">If they are in Ready state, check the output folder for the output files.</span></span>
2. <span data-ttu-id="3c4b6-530">Növeli vagy csökkenti a **egyidejűségi** beállítás tudni, hogy milyen hatással van a teljesítmény, a megoldás, különösen akkor fordul elő, az Azure Batch feldolgozását.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-530">Increase or decrease the **concurrency** setting to understand how it affects the performance of your solution, especially the processing that occurs on Azure Batch.</span></span> <span data-ttu-id="3c4b6-531">(Lásd a 4. lépés: hozzon létre, és futtassa a folyamat hosszabb a **egyidejűségi** beállítás.)</span><span class="sxs-lookup"><span data-stu-id="3c4b6-531">(See Step 4: Create and run the pipeline for more on the **concurrency** setting.)</span></span>
3. <span data-ttu-id="3c4b6-532">Hozzon létre egy alkalmazáskészlet magasabb/alacsonyabb **virtuális gépenként feladatok maximális**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-532">Create a pool with higher/lower **Maximum tasks per VM**.</span></span> <span data-ttu-id="3c4b6-533">A létrehozott új készletet használ, frissítse a kapcsolódó Azure Batch szolgáltatás a Data Factory-megoldásban.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-533">To use the new pool you created, update the Azure Batch linked service in the Data Factory solution.</span></span> <span data-ttu-id="3c4b6-534">(Lásd a 4. lépés: hozzon létre, és futtassa a folyamat hosszabb a **virtuális gépenként feladatok maximális** beállítás.)</span><span class="sxs-lookup"><span data-stu-id="3c4b6-534">(See Step 4: Create and run the pipeline for more on the **Maximum tasks per VM** setting.)</span></span>
4. <span data-ttu-id="3c4b6-535">Az Azure Batch-készlet létrehozása **automatikus skálázás** szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-535">Create an Azure Batch pool with **autoscale** feature.</span></span> <span data-ttu-id="3c4b6-536">A dinamikusan állítható lesz az feldolgozási kapacitása révén az alkalmazás által használt Azure Batch-készlet számítási csomópontjainak automatikus skálázás.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-536">Automatically scaling compute nodes in an Azure Batch pool is the dynamic adjustment of processing power used by your application.</span></span> 

    <span data-ttu-id="3c4b6-537">A minta képlet itt éri el a következő viselkedés: a készlet létrehozásakor 1 virtuális gép kezdődik.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-537">The sample formula here achieves the following behavior: When the pool is initially created, it starts with 1 VM.</span></span> <span data-ttu-id="3c4b6-538">$PendingTasks metrika feladatok száma definiálja a futó + (aszinkron) aktív állapotban.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-538">$PendingTasks metric defines the number of tasks in running + active (queued) state.</span></span>  <span data-ttu-id="3c4b6-539">A képlet átlagos száma függőben lévő feladatok megkeresi az elmúlt 180 másodperc alatt, és ennek megfelelően állítja be TargetDedicated.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-539">The formula finds the average number of pending tasks in the last 180 seconds and sets TargetDedicated accordingly.</span></span> <span data-ttu-id="3c4b6-540">Biztosítja, hogy TargetDedicated soha nem túllép 25 virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-540">It ensures that TargetDedicated never goes beyond 25 VMs.</span></span> <span data-ttu-id="3c4b6-541">Igen új feladatok nyújtják, készlet automatikusan növekszik és feladatok befejezését, mint a virtuális gépek válik a szabad egyenként és az automatikus skálázás zsugorítja a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-541">So, as new tasks are submitted, pool automatically grows and as tasks complete, VMs become free one by one and the autoscaling shrinks those VMs.</span></span> <span data-ttu-id="3c4b6-542">startingNumberOfVMs és maxNumberofVMs az igényeinek megfelelően kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-542">startingNumberOfVMs and maxNumberofVMs can be adjusted to your needs.</span></span>
 
    <span data-ttu-id="3c4b6-543">Automatikus skálázás képlet:</span><span class="sxs-lookup"><span data-stu-id="3c4b6-543">Autoscale formula:</span></span>

    ``` 
    startingNumberOfVMs = 1;
    maxNumberofVMs = 25;
    pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
    pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
    $TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
    ```

   <span data-ttu-id="3c4b6-544">Lásd: [automatikus méretezési számítási csomópontok az Azure Batch-készlet](../batch/batch-automatic-scaling.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-544">See [Automatically scale compute nodes in an Azure Batch pool](../batch/batch-automatic-scaling.md) for details.</span></span>

   <span data-ttu-id="3c4b6-545">Ha az alkalmazáskészlet által használt alapértelmezett [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), a Batch szolgáltatás a virtuális gép előkészítése az egyéni tevékenység futtatása előtt 15 – 30 percet vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-545">If the pool is using the default [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), the Batch service could take 15-30 minutes to prepare the VM before running the custom activity.</span></span>  <span data-ttu-id="3c4b6-546">Ha a készlet egy másik autoScaleEvaluationInterval használ, a Batch szolgáltatás autoScaleEvaluationInterval + 10 percet is beletelhet.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-546">If the pool is using a different autoScaleEvaluationInterval, the Batch service could take autoScaleEvaluationInterval + 10 minutes.</span></span>
5. <span data-ttu-id="3c4b6-547">A minta a megoldásban a **Execute** metódus meghívja a **Calculate** módszer, amely feldolgozza a egy bemeneti adatszelet egy kimeneti adatszelet létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-547">In the sample solution, the **Execute** method invokes the **Calculate** method that processes an input data slice to produce an output data slice.</span></span> <span data-ttu-id="3c4b6-548">A bemeneti adatok feldolgozásához, és cserélje le a Calculate metódus hívásához a az Execute metódus a metódus hívásakor a saját metódus lehet írni.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-548">You can write your own method to process input data and replace the Calculate method call in the Execute method with a call to your method.</span></span>

### <a name="next-steps-consume-the-data"></a><span data-ttu-id="3c4b6-549">További lépések: az adatokat</span><span class="sxs-lookup"><span data-stu-id="3c4b6-549">Next steps: Consume the data</span></span>
<span data-ttu-id="3c4b6-550">Adatok feldolgozása után felhasználhat az online eszközökkel, például a **Microsoft Power BI**.</span><span class="sxs-lookup"><span data-stu-id="3c4b6-550">After you process data, you can consume it with online tools like **Microsoft Power BI**.</span></span> <span data-ttu-id="3c4b6-551">Az alábbiakban a hivatkozásokat, amelyekkel jobban megértheti a Power bi-ban, és hogy miképpen lehet vele az Azure-ban:</span><span class="sxs-lookup"><span data-stu-id="3c4b6-551">Here are links to help you understand Power BI and how to use it in Azure:</span></span>

* [<span data-ttu-id="3c4b6-552">A Power BI dataset felfedezés</span><span class="sxs-lookup"><span data-stu-id="3c4b6-552">Explore a dataset in Power BI</span></span>](https://powerbi.microsoft.com/documentation/powerbi-service-get-data/)
* [<span data-ttu-id="3c4b6-553">Bevezetés a Power BI Desktop használatába</span><span class="sxs-lookup"><span data-stu-id="3c4b6-553">Getting started with the Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/)
* [<span data-ttu-id="3c4b6-554">Adatok frissítése a Power bi-ban</span><span class="sxs-lookup"><span data-stu-id="3c4b6-554">Refresh data in Power BI</span></span>](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/)
* [<span data-ttu-id="3c4b6-555">Azure és a Power BI – alapvető – áttekintés</span><span class="sxs-lookup"><span data-stu-id="3c4b6-555">Azure and Power BI - basic overview</span></span>](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)

## <a name="references"></a><span data-ttu-id="3c4b6-556">Referencia</span><span class="sxs-lookup"><span data-stu-id="3c4b6-556">References</span></span>
* [<span data-ttu-id="3c4b6-557">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="3c4b6-557">Azure Data Factory</span></span>](https://azure.microsoft.com/documentation/services/data-factory/)

  * [<span data-ttu-id="3c4b6-558">Bevezetés az Azure Data Factory szolgáltatásnak</span><span class="sxs-lookup"><span data-stu-id="3c4b6-558">Introduction to Azure Data Factory service</span></span>](data-factory-introduction.md)
  * [<span data-ttu-id="3c4b6-559">Ismerkedés az Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="3c4b6-559">Get started with Azure Data Factory</span></span>](data-factory-build-your-first-pipeline.md)
  * [<span data-ttu-id="3c4b6-560">Egyéni tevékenységek használata Azure Data Factory-folyamatban</span><span class="sxs-lookup"><span data-stu-id="3c4b6-560">Use custom activities in an Azure Data Factory pipeline</span></span>](data-factory-use-custom-activities.md)
* [<span data-ttu-id="3c4b6-561">Az Azure Batch</span><span class="sxs-lookup"><span data-stu-id="3c4b6-561">Azure Batch</span></span>](https://azure.microsoft.com/documentation/services/batch/)

  * [<span data-ttu-id="3c4b6-562">Az Azure Batch alapjai</span><span class="sxs-lookup"><span data-stu-id="3c4b6-562">Basics of Azure Batch</span></span>](../batch/batch-technical-overview.md)
  * [<span data-ttu-id="3c4b6-563">Azure Batch funkcióinak áttekintése</span><span class="sxs-lookup"><span data-stu-id="3c4b6-563">Overview of Azure Batch features</span></span>](../batch/batch-api-basics.md)
  * [<span data-ttu-id="3c4b6-564">Létrehozása és kezelése az Azure portál Azure Batch-fiók</span><span class="sxs-lookup"><span data-stu-id="3c4b6-564">Create and manage Azure Batch account in the Azure portal</span></span>](../batch/batch-account-create-portal.md)
  * [<span data-ttu-id="3c4b6-565">Ismerkedés az Azure Batch Library .NET</span><span class="sxs-lookup"><span data-stu-id="3c4b6-565">Get started with Azure Batch Library .NET</span></span>](../batch/batch-dotnet-get-started.md)

[batch-explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch-explorer-walkthrough]: http://blogs.technet.com/b/windowshpc/archive/2015/01/20/azure-batch-explorer-sample-walkthrough.aspx
