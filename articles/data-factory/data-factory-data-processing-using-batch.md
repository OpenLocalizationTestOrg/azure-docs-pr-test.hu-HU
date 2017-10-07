---
title: "használatával a Data Factory és kötegelt aaaProcess nagy méretű adatkészletek |} Microsoft Docs"
description: "Ismerteti, hogyan tooprocess hatalmas mennyiségű adatokat az Azure Data Factory-feldolgozási folyamat Azure kötegelt párhuzamos feldolgozási képességének használatával."
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
ms.openlocfilehash: 6788f02de555d2e9d6588cc990a39043866d7e97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="process-large-scale-datasets-using-data-factory-and-batch"></a><span data-ttu-id="ccf50-103">Nagyméretű adatkészletek feldolgozása a Data Factory és a Batch használatával</span><span class="sxs-lookup"><span data-stu-id="ccf50-103">Process large-scale datasets using Data Factory and Batch</span></span>
<span data-ttu-id="ccf50-104">Ez a cikk ismerteti a minta megoldás, amely helyezi át, és feldolgozza a nagy méretű adatkészletek automatikus és ütemezett módon architektúrát.</span><span class="sxs-lookup"><span data-stu-id="ccf50-104">This article describes an architecture of a sample solution that moves and processes large-scale datasets in an automatic and scheduled manner.</span></span> <span data-ttu-id="ccf50-105">Egy Azure Data Factory és az Azure Batch-végpontok közötti forgatókönyv tooimplement hello megoldás is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ccf50-105">It also provides an end-to-end walkthrough tooimplement hello solution using Azure Data Factory and Azure Batch.</span></span>

<span data-ttu-id="ccf50-106">Ez a cikk nem hosszabb, mint a szokásos cikk, mert a forgatókönyv egy teljes mintát megoldás tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ccf50-106">This article is longer than our typical article because it contains a walkthrough of an entire sample solution.</span></span> <span data-ttu-id="ccf50-107">Ha új tooBatch és a Data Factory megismerheti ezeket a szolgáltatásokat, és hogyan működnek együtt.</span><span class="sxs-lookup"><span data-stu-id="ccf50-107">If you are new tooBatch and Data Factory, you can learn about these services and how they work together.</span></span> <span data-ttu-id="ccf50-108">Ha tudja, valami hello szolgáltatásokra vonatkozó és vannak designing/újratervezni a megoldás, előfordulhat, hogy koncentrálhat, csak hello [architektúra szakasz](#architecture-of-sample-solution) hello cikket, és ha egy prototípus vagy megoldás fejleszt, akkor is érdemes lehet tootry részletes útmutatás a hello [forgatókönyv](#implementation-of-sample-solution).</span><span class="sxs-lookup"><span data-stu-id="ccf50-108">If you know something about hello services and are designing/architecting a solution, you may focus just on hello [architecture section](#architecture-of-sample-solution) of hello article and if you are developing a prototype or a solution, you may also want tootry out step-by-step instructions in hello [walkthrough](#implementation-of-sample-solution).</span></span> <span data-ttu-id="ccf50-109">A Microsoft hívhat meg a tartalmat, és használatának kapcsolatos megjegyzéseit.</span><span class="sxs-lookup"><span data-stu-id="ccf50-109">We invite your comments about this content and how you use it.</span></span>

<span data-ttu-id="ccf50-110">Első lépésként nézzük hogyan adat-előállító és a Batch szolgáltatás nagy adatkészletek hello felhőben feldolgozással segítik.</span><span class="sxs-lookup"><span data-stu-id="ccf50-110">First, let's look at how Data Factory and Batch services can help with processing large datasets in hello cloud.</span></span>     

## <a name="why-azure-batch"></a><span data-ttu-id="ccf50-111">Miért érdemes az Azure Batch?</span><span class="sxs-lookup"><span data-stu-id="ccf50-111">Why Azure Batch?</span></span>
<span data-ttu-id="ccf50-112">Az Azure Batch lehetővé teszi toorun nagyméretű párhuzamos és nagy teljesítményű számítástechnikai (HPC) alkalmazások hatékonyan hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="ccf50-112">Azure Batch enables you toorun large-scale parallel and high-performance computing (HPC) applications efficiently in hello cloud.</span></span> <span data-ttu-id="ccf50-113">Egy felügyelt szervezendő virtuális gépek számítási igényű munkahelyi toorun ütemezését végző platform szolgáltatás, és képes automatikusan méretezési számítási erőforrások toomeet hello igényeinek a feladatok.</span><span class="sxs-lookup"><span data-stu-id="ccf50-113">It's a platform service that schedules compute-intensive work toorun on a managed collection of virtual machines, and can automatically scale compute resources toomeet hello needs of your jobs.</span></span>

<span data-ttu-id="ccf50-114">A Batch szolgáltatás hello megadhatja az Azure számítási erőforrások tooexecute az alkalmazások párhuzamosan, és léptékű.</span><span class="sxs-lookup"><span data-stu-id="ccf50-114">With hello Batch service, you define Azure compute resources tooexecute your applications in parallel, and at scale.</span></span> <span data-ttu-id="ccf50-115">Futtatható azonnali vagy ütemezett feladatok, és nem kell toomanually létrehozása, konfigurálása és kezelése a HPC-fürt, az egyes virtuális gépek, virtuális hálózatok vagy egy összetett feladat és feladat ütemezési infrastruktúra.</span><span class="sxs-lookup"><span data-stu-id="ccf50-115">You can run on-demand or scheduled jobs, and you don't need toomanually create, configure, and manage an HPC cluster, individual virtual machines, virtual networks, or a complex job and task scheduling infrastructure.</span></span>

<span data-ttu-id="ccf50-116">Tekintse meg a következő cikkeket, ha nem ismeri az Azure Batch, segít megérteni, hello hello megoldás cikkben leírt architektúra/végrehajtásának hello.</span><span class="sxs-lookup"><span data-stu-id="ccf50-116">See hello following articles if you are not familiar with Azure Batch as it helps with understanding hello architecture/implementation of hello solution described in this article.</span></span>   

* [<span data-ttu-id="ccf50-117">Az Azure Batch alapjai</span><span class="sxs-lookup"><span data-stu-id="ccf50-117">Basics of Azure Batch</span></span>](../batch/batch-technical-overview.md)
* [<span data-ttu-id="ccf50-118">A Batch funkcióinak áttekintése</span><span class="sxs-lookup"><span data-stu-id="ccf50-118">Batch feature overview</span></span>](../batch/batch-api-basics.md)

<span data-ttu-id="ccf50-119">(választható) toolearn Azure Batch kapcsolatos további információkért lásd: hello [Azure Batch képzési terv](https://azure.microsoft.com/documentation/learning-paths/batch/).</span><span class="sxs-lookup"><span data-stu-id="ccf50-119">(optional) toolearn more about Azure Batch, see hello [Learning path for Azure Batch](https://azure.microsoft.com/documentation/learning-paths/batch/).</span></span>

## <a name="why-azure-data-factory"></a><span data-ttu-id="ccf50-120">Miért érdemes az Azure Data Factory?</span><span class="sxs-lookup"><span data-stu-id="ccf50-120">Why Azure Data Factory?</span></span>
<span data-ttu-id="ccf50-121">Adat-előállító koordinálja és automatizálja hello mozgás és adatok átalakítása felhőalapú adatok integrációs szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="ccf50-121">Data Factory is a cloud-based data integration service that orchestrates and automates hello movement and transformation of data.</span></span> <span data-ttu-id="ccf50-122">Hello Data Factory szolgáltatásnak, hozhat létre, amely tárolt adatok mozgatása a helyszíni és felhőalapú tárolók tooa központosított adatokat tároló felügyelt adatok folyamatok (például: Azure Blob Storage), és a folyamat/átalakítás-adatokat, például az Azure HDInsight és az Azure szolgáltatások Gépi tanulás.</span><span class="sxs-lookup"><span data-stu-id="ccf50-122">Using hello Data Factory service, you can create managed data pipelines that move data from on-premises and cloud data stores tooa centralized data store (for example: Azure Blob Storage), and process/transform data using services such as Azure HDInsight and Azure Machine Learning.</span></span> <span data-ttu-id="ccf50-123">Is adatok folyamatok toorun ütemezett módon (óránkénti, napi, heti, stb.) és a figyelő ütemezése és kezelheti azokat egy pillanat alatt tooidentify problémákat, és hajtsa végre a műveletet.</span><span class="sxs-lookup"><span data-stu-id="ccf50-123">You can also schedule data pipelines toorun in a scheduled manner (hourly, daily, weekly, etc.) and monitor and manage them at a glance tooidentify issues and take action.</span></span>

<span data-ttu-id="ccf50-124">Tekintse meg a következő cikkeket, ha nem ismeri az Azure Data Factoryvel, segít megérteni, hello hello megoldás cikkben leírt architektúra/végrehajtásának hello.</span><span class="sxs-lookup"><span data-stu-id="ccf50-124">See hello following articles if you are not familiar with Azure Data Factory as it helps with understanding hello architecture/implementation of hello solution described in this article.</span></span>  

* [<span data-ttu-id="ccf50-125">Az Azure Data Factory bevezetése</span><span class="sxs-lookup"><span data-stu-id="ccf50-125">Introduction of Azure Data Factory</span></span>](data-factory-introduction.md)
* [<span data-ttu-id="ccf50-126">Felépítheti első folyamatát adatok</span><span class="sxs-lookup"><span data-stu-id="ccf50-126">Build your first data pipeline</span></span>](data-factory-build-your-first-pipeline.md)   

<span data-ttu-id="ccf50-127">További információ az Azure Data Factory (nem kötelező) toolearn lásd: hello [az Azure Data Factory képzési terv](https://azure.microsoft.com/documentation/learning-paths/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="ccf50-127">(optional) toolearn more about Azure Data Factory, see hello [Learning path for Azure Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/).</span></span>

## <a name="data-factory-and-batch-together"></a><span data-ttu-id="ccf50-128">Adat-előállító és kötegelt együtt</span><span class="sxs-lookup"><span data-stu-id="ccf50-128">Data Factory and Batch together</span></span>
<span data-ttu-id="ccf50-129">Adat-előállító beépített tevékenységeket magában foglalja, például a forrásadatok másolási tevékenység toocopy/move data tooa adatok céltár és Hive tevékenységek tooprocess adatait az Azure-on Hadoop-fürtök (HDInsight) használatával.</span><span class="sxs-lookup"><span data-stu-id="ccf50-129">Data Factory includes built-in activities such as Copy Activity toocopy/move data from a source data store tooa destination data store and Hive Activity tooprocess data using Hadoop clusters (HDInsight) on Azure.</span></span> <span data-ttu-id="ccf50-130">Lásd: [adatok átalakítása tevékenységek](data-factory-data-transformation-activities.md) támogatott átalakítása tevékenységek listáját.</span><span class="sxs-lookup"><span data-stu-id="ccf50-130">See [Data Transformation Activities](data-factory-data-transformation-activities.md) for a list of supported transformation activities.</span></span>

<span data-ttu-id="ccf50-131">Lehetővé teszi, hogy a hogy toocreate egyéni .NET tevékenység saját logikával toomove vagy folyamat adatok és a egy Azure HDInsight-fürt vagy a virtuális gépek Azure Batch-készlet futtassa le ezeket a tevékenységeket.</span><span class="sxs-lookup"><span data-stu-id="ccf50-131">It also allows you toocreate custom .NET activities toomove or process data with your own logic and run these activities on an Azure HDInsight cluster or on an Azure Batch pool of VMs.</span></span> <span data-ttu-id="ccf50-132">Azure Batch használata esetén konfigurálhatja hello készlet tooauto méretű (hozzáadása vagy eltávolítása a virtuális gépek hello munkaterhelés alapján), adja meg a képlet alapján.</span><span class="sxs-lookup"><span data-stu-id="ccf50-132">When you use Azure Batch, you can configure hello pool tooauto-scale (add or remove VMs based on hello workload) based on a formula you provide.</span></span>     

## <a name="architecture-of-sample-solution"></a><span data-ttu-id="ccf50-133">A minta megoldás architektúrája</span><span class="sxs-lookup"><span data-stu-id="ccf50-133">Architecture of sample solution</span></span>
<span data-ttu-id="ccf50-134">Annak ellenére, hogy ebben a cikkben leírt hello architektúra a legegyszerűbb megoldás az, akkor megfelelő toocomplex forgatókönyvek például a pénzügyi szolgáltatásokat, kép feldolgozási és megjelenítési és genomikus elemzés modellezési kockázat.</span><span class="sxs-lookup"><span data-stu-id="ccf50-134">Even though hello architecture described in this article is for a simple solution, it is relevant toocomplex scenarios such as risk modeling by financial services, image processing and rendering, and genomic analysis.</span></span>

<span data-ttu-id="ccf50-135">hello diagram azt ábrázolja, 1) hogyan koordinálja a Data Factory az adatmozgás, és feldolgozás és a 2.) hogyan dolgozza fel a Azure Batch hello adatok párhuzamos módon.</span><span class="sxs-lookup"><span data-stu-id="ccf50-135">hello diagram illustrates 1) how Data Factory orchestrates data movement and processing and 2) how Azure Batch processes hello data in a parallel manner.</span></span> <span data-ttu-id="ccf50-136">A letöltés és nyomtatási hello diagram az egyszerűbb használat (11 x 17.</span><span class="sxs-lookup"><span data-stu-id="ccf50-136">Download and print hello diagram for easy reference (11 x 17 in.</span></span> <span data-ttu-id="ccf50-137">vagy A3 méret): [Azure Batch és a Data Factory használatával HPC és az orchestration](http://go.microsoft.com/fwlink/?LinkId=717686).</span><span class="sxs-lookup"><span data-stu-id="ccf50-137">or A3 size): [HPC and data orchestration using Azure Batch and Data Factory](http://go.microsoft.com/fwlink/?LinkId=717686).</span></span>

<span data-ttu-id="ccf50-138">[![A nagyméretű adatok feldolgozása diagramja](./media/data-factory-data-processing-using-batch/image1.png)](http://go.microsoft.com/fwlink/?LinkId=717686)</span><span class="sxs-lookup"><span data-stu-id="ccf50-138">[![Large-scale data processing diagram](./media/data-factory-data-processing-using-batch/image1.png)](http://go.microsoft.com/fwlink/?LinkId=717686)</span></span>

<span data-ttu-id="ccf50-139">hello alábbi lépéseit hello alapszintű hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="ccf50-139">hello following list provides hello basic steps of hello process.</span></span> <span data-ttu-id="ccf50-140">hello megoldás kódot és magyarázatokat toobuild hello végpont megoldást tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ccf50-140">hello solution includes code and explanations toobuild hello end-to-end solution.</span></span>

1. <span data-ttu-id="ccf50-141">**Konfigurálja az Azure Batch számítási csomópontok (VM) készletét**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-141">**Configure Azure Batch with a pool of compute nodes (VMs)**.</span></span> <span data-ttu-id="ccf50-142">Hello csomópontok száma és mérete az egyes csomópontok is megadhat.</span><span class="sxs-lookup"><span data-stu-id="ccf50-142">You can specify hello number of nodes and size of each node.</span></span>
2. <span data-ttu-id="ccf50-143">**Hozzon létre egy Azure Data Factory példányt** , amelyek megfelelnek az Azure blob storage, Azure Batch számítási szolgáltatás, bemeneti adatokat és egy munkafolyamat/folyamat olyan tevékenységet, amely áthelyezése és az adatok átalakítása entitások van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="ccf50-143">**Create an Azure Data Factory instance** that is configured with entities that represent Azure blob storage, Azure Batch compute service, input/output data, and a workflow/pipeline with activities that move and transform data.</span></span>
3. <span data-ttu-id="ccf50-144">**Hozzon létre egy egyéni .NET tevékenység hello Data Factory-folyamathoz**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-144">**Create a custom .NET activity in hello Data Factory pipeline**.</span></span> <span data-ttu-id="ccf50-145">hello tevékenysége a hello Azure Batch-készlet futó felhasználói kód.</span><span class="sxs-lookup"><span data-stu-id="ccf50-145">hello activity is your user code that runs on hello Azure Batch pool.</span></span>
4. <span data-ttu-id="ccf50-146">**A bemeneti adatok nagy mennyiségű tárolót, mint az Azure-tárfiókba blobok**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-146">**Store large amounts of input data as blobs in Azure storage**.</span></span> <span data-ttu-id="ccf50-147">Adatok osztóval időszeletekre logikai (általában idő).</span><span class="sxs-lookup"><span data-stu-id="ccf50-147">Data is divided into logical slices (usually by time).</span></span>
5. <span data-ttu-id="ccf50-148">**Adat-előállító másolja át a feldolgozott adatok párhuzamosan** toohello másodlagos helyre.</span><span class="sxs-lookup"><span data-stu-id="ccf50-148">**Data Factory copies data that is processed in parallel** toohello secondary location.</span></span>
6. <span data-ttu-id="ccf50-149">**Adat-előállító futtatja le van foglalva kötegelt használja hello hello egyéni tevékenység**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-149">**Data Factory runs hello custom activity using hello pool allocated by Batch**.</span></span> <span data-ttu-id="ccf50-150">Adat-előállító egyidejűleg futtatható tevékenységek.</span><span class="sxs-lookup"><span data-stu-id="ccf50-150">Data Factory can run activities concurrently.</span></span> <span data-ttu-id="ccf50-151">Minden tevékenység adatok szelet dolgozza fel.</span><span class="sxs-lookup"><span data-stu-id="ccf50-151">Each activity processes a slice of data.</span></span> <span data-ttu-id="ccf50-152">hello eredmények az Azure storage tárolja.</span><span class="sxs-lookup"><span data-stu-id="ccf50-152">hello results are stored in Azure storage.</span></span>
7. <span data-ttu-id="ccf50-153">**Adat-előállító hello végleges eredmények tooa harmadik helyre helyezi át**, vagy terjesztési keresztül egy alkalmazást, vagy más eszközökkel további feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="ccf50-153">**Data Factory moves hello final results tooa third location**, either for distribution via an app, or for further processing by other tools.</span></span>

## <a name="implementation-of-sample-solution"></a><span data-ttu-id="ccf50-154">A minta-megoldás megvalósítása</span><span class="sxs-lookup"><span data-stu-id="ccf50-154">Implementation of sample solution</span></span>
<span data-ttu-id="ccf50-155">hello megoldást szándékosan egyszerű és tooshow, hogyan toouse adat-előállító és kötegelt együtt tooprocess adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="ccf50-155">hello sample solution is intentionally simple and is tooshow you how toouse Data Factory and Batch together tooprocess datasets.</span></span> <span data-ttu-id="ccf50-156">hello megoldás egyszerűen számolja hello egy keresési kifejezés ("Microsoft") előfordulását a bemeneti fájlok idősor rendezve.</span><span class="sxs-lookup"><span data-stu-id="ccf50-156">hello solution simply counts hello number of occurrences of a search term (“Microsoft”) in input files organized in a time series.</span></span> <span data-ttu-id="ccf50-157">Hello száma toooutput fájlok kiírja azt.</span><span class="sxs-lookup"><span data-stu-id="ccf50-157">It outputs hello count toooutput files.</span></span>

<span data-ttu-id="ccf50-158">**Idő**: Amennyiben az Azure Data Factory és kötegelt alapjainak megismerése, és rendelkezik befejezett hello Előfeltételek alább felsorolt, azt adja meg ehhez a megoldáshoz szükséges 1 – 2 óra toocomplete.</span><span class="sxs-lookup"><span data-stu-id="ccf50-158">**Time**: If you are familiar with basics of Azure, Data Factory, and Batch, and have completed hello prerequisites listed below, we estimate this solution takes 1-2 hours toocomplete.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="ccf50-159">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ccf50-159">Prerequisites</span></span>
#### <a name="azure-subscription"></a><span data-ttu-id="ccf50-160">Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="ccf50-160">Azure subscription</span></span>
<span data-ttu-id="ccf50-161">Ha nem rendelkezik Azure-előfizetéssel, létrehozhat egy ingyenes próbafiók néhány perc alatt.</span><span class="sxs-lookup"><span data-stu-id="ccf50-161">If you don't have an Azure subscription, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="ccf50-162">Lásd: [ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ccf50-162">See [Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

#### <a name="azure-storage-account"></a><span data-ttu-id="ccf50-163">Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="ccf50-163">Azure storage account</span></span>
<span data-ttu-id="ccf50-164">Egy Azure storage-fiók ebben az oktatóanyagban hello adatok tárolásához használja.</span><span class="sxs-lookup"><span data-stu-id="ccf50-164">You use an Azure storage account for storing hello data in this tutorial.</span></span> <span data-ttu-id="ccf50-165">Ha egy Azure storage-fiók nem rendelkezik, tekintse meg [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="ccf50-165">If you don't have an Azure storage account, see [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span></span> <span data-ttu-id="ccf50-166">hello megoldást használja a blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="ccf50-166">hello sample solution uses blob storage.</span></span>

#### <a name="azure-batch-account"></a><span data-ttu-id="ccf50-167">Azure Batch-fiók</span><span class="sxs-lookup"><span data-stu-id="ccf50-167">Azure Batch account</span></span>
<span data-ttu-id="ccf50-168">Hello használata Azure Batch-fiók létrehozása [Azure-portálon](http://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="ccf50-168">Create an Azure Batch account using hello [Azure portal](http://manage.windowsazure.com/).</span></span> <span data-ttu-id="ccf50-169">Lásd: [létrehozása és kezelése az Azure Batch-fiók](../batch/batch-account-create-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ccf50-169">See [Create and manage an Azure Batch account](../batch/batch-account-create-portal.md).</span></span> <span data-ttu-id="ccf50-170">Vegye figyelembe a hello Azure Batch nevét és a fiók kulcsot.</span><span class="sxs-lookup"><span data-stu-id="ccf50-170">Note hello Azure Batch account name and account key.</span></span> <span data-ttu-id="ccf50-171">Is [New-AzureRmBatchAccount](https://msdn.microsoft.com/library/mt603749.aspx) parancsmag toocreate Azure Batch-fiók.</span><span class="sxs-lookup"><span data-stu-id="ccf50-171">You can also use [New-AzureRmBatchAccount](https://msdn.microsoft.com/library/mt603749.aspx) cmdlet toocreate an Azure Batch account.</span></span> <span data-ttu-id="ccf50-172">Lásd: [Ismerkedés az Azure Batch PowerShell-parancsmagok](../batch/batch-powershell-cmdlets-get-started.md) részletes útmutató a parancsmag használatával.</span><span class="sxs-lookup"><span data-stu-id="ccf50-172">See [Get started with Azure Batch PowerShell cmdlets](../batch/batch-powershell-cmdlets-get-started.md) for detailed instructions on using this cmdlet.</span></span>

<span data-ttu-id="ccf50-173">hello megoldást Azure Batch (közvetve keresztül egy Azure Data Factory-folyamathoz) tooprocess adatokat a számítási csomópontok (a virtuális gépek felügyelt gyűjteménye) készlete egy párhuzamos módon használja.</span><span class="sxs-lookup"><span data-stu-id="ccf50-173">hello sample solution uses Azure Batch (indirectly via an Azure Data Factory pipeline) tooprocess data in a parallel manner on a pool of compute nodes (a managed collection of virtual machines).</span></span>

#### <a name="azure-batch-pool-of-virtual-machines-vms"></a><span data-ttu-id="ccf50-174">A virtuális gépek (VM) az Azure Batch-készlet</span><span class="sxs-lookup"><span data-stu-id="ccf50-174">Azure Batch pool of virtual machines (VMs)</span></span>
<span data-ttu-id="ccf50-175">Hozzon létre egy **Azure Batch-készlet** legalább 2-es számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="ccf50-175">Create an **Azure Batch pool** with at least 2 compute nodes.</span></span>

1. <span data-ttu-id="ccf50-176">A hello [Azure-portálon](https://portal.azure.com), kattintson a **Tallózás** a bal oldali menü hello, és kattintson a **Batch-fiókok**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-176">In hello [Azure portal](https://portal.azure.com), click **Browse** in hello left menu, and click **Batch Accounts**.</span></span>
2. <span data-ttu-id="ccf50-177">Válassza ki az Azure Batch-fiók tooopen hello **Batch-fiók** panelen.</span><span class="sxs-lookup"><span data-stu-id="ccf50-177">Select your Azure Batch account tooopen hello **Batch Account** blade.</span></span>
3. <span data-ttu-id="ccf50-178">Kattintson a **készletek** csempére.</span><span class="sxs-lookup"><span data-stu-id="ccf50-178">Click **Pools** tile.</span></span>
4. <span data-ttu-id="ccf50-179">A hello **készletek** panelen hello eszköztár tooadd a készlet hozzáadása gombra.</span><span class="sxs-lookup"><span data-stu-id="ccf50-179">In hello **Pools** blade, click Add button on hello toolbar tooadd a pool.</span></span>
   1. <span data-ttu-id="ccf50-180">Adjon meg egy Azonosítót hello készlet (**alkalmazáskészlet-azonosító**).</span><span class="sxs-lookup"><span data-stu-id="ccf50-180">Enter an ID for hello pool (**Pool ID**).</span></span> <span data-ttu-id="ccf50-181">Megjegyzés: hello **hello készlet azonosítója**; amikor hello adat-előállító megoldás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="ccf50-181">Note hello **ID of hello pool**; you need it when creating hello Data Factory solution.</span></span>
   2. <span data-ttu-id="ccf50-182">Adja meg **Windows Server 2012 R2** hello operációsrendszer-családot beállítás.</span><span class="sxs-lookup"><span data-stu-id="ccf50-182">Specify **Windows Server 2012 R2** for hello Operating System Family setting.</span></span>
   3. <span data-ttu-id="ccf50-183">Válassza ki a **csomóponti tarifacsomagot**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-183">Select a **node pricing tier**.</span></span>
   4. <span data-ttu-id="ccf50-184">Adja meg **2** hello értékként **cél dedikált** beállítást.</span><span class="sxs-lookup"><span data-stu-id="ccf50-184">Enter **2** as value for hello **Target Dedicated** setting.</span></span>
   5. <span data-ttu-id="ccf50-185">Adja meg **2** hello értékként **csomópontonkénti tevékenységek maximális** beállítást.</span><span class="sxs-lookup"><span data-stu-id="ccf50-185">Enter **2** as value for hello **Max tasks per node** setting.</span></span>
   6. <span data-ttu-id="ccf50-186">Kattintson a **OK** toocreate hello készlet.</span><span class="sxs-lookup"><span data-stu-id="ccf50-186">Click **OK** toocreate hello pool.</span></span>

#### <a name="azure-storage-explorer"></a><span data-ttu-id="ccf50-187">Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="ccf50-187">Azure Storage Explorer</span></span>
<span data-ttu-id="ccf50-188">[Az Azure Storage Explorer 6 (eszköz)](https://azurestorageexplorer.codeplex.com/) vagy [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) (a ClumsyLeaf szoftver).</span><span class="sxs-lookup"><span data-stu-id="ccf50-188">[Azure Storage Explorer 6 (tool)](https://azurestorageexplorer.codeplex.com/) or [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) (from ClumsyLeaf Software).</span></span> <span data-ttu-id="ccf50-189">Ezek az eszközök használata vizsgálatával, és a felhőben üzemeltetett alkalmazások hello naplófájlokkal együtt az Azure Storage-projektek hello adatainak módosítása.</span><span class="sxs-lookup"><span data-stu-id="ccf50-189">You use these tools for inspecting and altering hello data in your Azure Storage projects including hello logs of your cloud-hosted applications.</span></span>

1. <span data-ttu-id="ccf50-190">Hozzon létre egy nevű tárolót **mycontainer** privát hozzáférést (nincs névtelen hozzáférés)</span><span class="sxs-lookup"><span data-stu-id="ccf50-190">Create a container named **mycontainer** with private access (no anonymous access)</span></span>
2. <span data-ttu-id="ccf50-191">Ha használ **CloudXplorer**, mappák létrehozása, és az almappák hello struktúra a következő:</span><span class="sxs-lookup"><span data-stu-id="ccf50-191">If you are using **CloudXplorer**, create folders and subfolders with hello following structure:</span></span>

   ![](./media/data-factory-data-processing-using-batch/image3.png)

   <span data-ttu-id="ccf50-192">`Inputfolder`és `outputfolder` a legfelső szintű mappák `mycontainer`.</span><span class="sxs-lookup"><span data-stu-id="ccf50-192">`Inputfolder` and `outputfolder` are top-level folders in `mycontainer`.</span></span> <span data-ttu-id="ccf50-193">Hello `inputfolder` almappái dátum-időbélyegeket (éééé-hh-nn-ÓÓ) rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ccf50-193">hello `inputfolder` has subfolders with date-time stamps (YYYY-MM-DD-HH).</span></span>

   <span data-ttu-id="ccf50-194">Ha használ **Azure Tártallózó**, hello a következő lépésben szüksége tooupload névvel rendelkező fájlok: `inputfolder/2015-11-16-00/file.txt`, `inputfolder/2015-11-16-01/file.txt` és így tovább.</span><span class="sxs-lookup"><span data-stu-id="ccf50-194">If you are using **Azure Storage Explorer**, in hello next step, you need tooupload files with names: `inputfolder/2015-11-16-00/file.txt`, `inputfolder/2015-11-16-01/file.txt` and so on.</span></span> <span data-ttu-id="ccf50-195">Ez a lépés automatikusan hello mappákat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ccf50-195">This step automatically creates hello folders.</span></span>
3. <span data-ttu-id="ccf50-196">Hozzon létre egy szövegfájlt **file.txt** hello kulcsszó tartalmat a gépen **Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-196">Create a text file **file.txt** on your machine with content that has hello keyword **Microsoft**.</span></span> <span data-ttu-id="ccf50-197">Például: "egyéni tevékenység Microsoft teszt egyéni tevékenység Microsoft teszt".</span><span class="sxs-lookup"><span data-stu-id="ccf50-197">For example: “test custom activity Microsoft test custom activity Microsoft”.</span></span>
4. <span data-ttu-id="ccf50-198">Töltse fel a következő bemeneti mappák az Azure blob storage hello fájl toohello.</span><span class="sxs-lookup"><span data-stu-id="ccf50-198">Upload hello file toohello following input folders in Azure blob storage.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image4.png)

   <span data-ttu-id="ccf50-199">Ha használ **Azure Tártallózó**, hello-fájl feltöltése **file.txt** túl**mycontainer**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-199">If you are using **Azure Storage Explorer**, upload hello file **file.txt** too**mycontainer**.</span></span> <span data-ttu-id="ccf50-200">Kattintson a **másolási** a hello eszköztár toocreate hello blob egy példányát.</span><span class="sxs-lookup"><span data-stu-id="ccf50-200">Click **Copy** on hello toolbar toocreate a copy of hello blob.</span></span> <span data-ttu-id="ccf50-201">A hello **másolási Blob** párbeszédpanelen, a módosítás hello **cél blob neve** túl`inputfolder/2015-11-16-00/file.txt`.</span><span class="sxs-lookup"><span data-stu-id="ccf50-201">In hello **Copy Blob** dialog box, change hello **destination blob name** too`inputfolder/2015-11-16-00/file.txt`.</span></span> <span data-ttu-id="ccf50-202">Ismételje meg ezt a lépést toocreate `inputfolder/2015-11-16-01/file.txt`, `inputfolder/2015-11-16-02/file.txt`, `inputfolder/2015-11-16-03/file.txt`, `inputfolder/2015-11-16-04/file.txt` és így tovább.</span><span class="sxs-lookup"><span data-stu-id="ccf50-202">Repeat this step toocreate `inputfolder/2015-11-16-01/file.txt`, `inputfolder/2015-11-16-02/file.txt`, `inputfolder/2015-11-16-03/file.txt`, `inputfolder/2015-11-16-04/file.txt` and so on.</span></span> <span data-ttu-id="ccf50-203">Ez a művelet automatikusan hello mappákat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ccf50-203">This action automatically creates hello folders.</span></span>
5. <span data-ttu-id="ccf50-204">Hozzon létre egy másik tárolót: `customactivitycontainer`.</span><span class="sxs-lookup"><span data-stu-id="ccf50-204">Create another container named: `customactivitycontainer`.</span></span> <span data-ttu-id="ccf50-205">Hello egyéni zip fájlt toothis tároló-be feltölteni.</span><span class="sxs-lookup"><span data-stu-id="ccf50-205">You upload hello custom activity zip file toothis container.</span></span>

#### <a name="visual-studio"></a><span data-ttu-id="ccf50-206">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ccf50-206">Visual Studio</span></span>
<span data-ttu-id="ccf50-207">Telepítse a Microsoft Visual Studio 2012 vagy újabb toocreate hello egyéni kötegelt tevékenység toobe hello adat-előállító megoldás szerepel.</span><span class="sxs-lookup"><span data-stu-id="ccf50-207">Install Microsoft Visual Studio 2012 or later toocreate hello custom Batch activity toobe used in hello Data Factory solution.</span></span>

### <a name="high-level-steps-toocreate-hello-solution"></a><span data-ttu-id="ccf50-208">Magas szintű lépései toocreate hello megoldás</span><span class="sxs-lookup"><span data-stu-id="ccf50-208">High-level steps toocreate hello solution</span></span>
1. <span data-ttu-id="ccf50-209">Létrehozhat egy egyéni, amely hello adatfeldolgozási logikáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ccf50-209">Create a custom activity that contains hello data processing logic.</span></span>
2. <span data-ttu-id="ccf50-210">Hozzon létre egy az Azure data factory hello egyéni tevékenységet használó:</span><span class="sxs-lookup"><span data-stu-id="ccf50-210">Create an Azure data factory that uses hello custom activity:</span></span>

### <a name="create-hello-custom-activity"></a><span data-ttu-id="ccf50-211">Hello egyéni tevékenység létrehozása</span><span class="sxs-lookup"><span data-stu-id="ccf50-211">Create hello custom activity</span></span>
<span data-ttu-id="ccf50-212">Adat-előállító egyéni tevékenység hello hello lelke a megoldást.</span><span class="sxs-lookup"><span data-stu-id="ccf50-212">hello Data Factory custom activity is hello heart of this sample solution.</span></span> <span data-ttu-id="ccf50-213">hello minta megoldás az Azure Batch toorun hello egyéni tevékenység.</span><span class="sxs-lookup"><span data-stu-id="ccf50-213">hello sample solution uses Azure Batch toorun hello custom activity.</span></span> <span data-ttu-id="ccf50-214">Lásd: [egyéni tevékenységeket használni egy Azure Data Factory-folyamathoz](data-factory-use-custom-activities.md) őket az Azure Data Factory folyamatok hello alapvető információkat toodevelop egyéni tevékenységeket és használatát.</span><span class="sxs-lookup"><span data-stu-id="ccf50-214">See [Use custom activities in an Azure Data Factory pipeline](data-factory-use-custom-activities.md) for hello basic information toodevelop custom activities and use them in Azure Data Factory pipelines.</span></span>

<span data-ttu-id="ccf50-215">toocreate szüksége toocreate .NET egyéni tevékenység, amely egy Azure Data Factory-folyamathoz használható, egy **.NET Class Library** egy osztály, amely megvalósítja az, hogy a projekt **IDotNetActivity** felületet.</span><span class="sxs-lookup"><span data-stu-id="ccf50-215">toocreate a .NET custom activity that you can use in an Azure Data Factory pipeline, you need toocreate a **.NET Class Library** project with a class that implements that **IDotNetActivity** interface.</span></span> <span data-ttu-id="ccf50-216">Ez az interfész csak egy metódusa: **Execute**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-216">This interface has only one method: **Execute**.</span></span> <span data-ttu-id="ccf50-217">Hello metódus aláírása hello itt található:</span><span class="sxs-lookup"><span data-stu-id="ccf50-217">Here is hello signature of hello method:</span></span>

```csharp
public IDictionary<string, string> Execute(
            IEnumerable<LinkedService> linkedServices,
            IEnumerable<Dataset> datasets,
            Activity activity,
            IActivityLogger logger)
```

<span data-ttu-id="ccf50-218">hello metódusnak, hogy kell-e toounderstand néhány kulcsfontosságú összetevők.</span><span class="sxs-lookup"><span data-stu-id="ccf50-218">hello method has a few key components that you need toounderstand.</span></span>

* <span data-ttu-id="ccf50-219">hello metódus négy paramétereket fogadja:</span><span class="sxs-lookup"><span data-stu-id="ccf50-219">hello method takes four parameters:</span></span>

  1. <span data-ttu-id="ccf50-220">**linkedServices**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-220">**linkedServices**.</span></span> <span data-ttu-id="ccf50-221">Egy enumerálható listája, amely a bemeneti/kimeneti adatforrások összekapcsolt szolgáltatások (például: Azure Blob Storage) toohello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="ccf50-221">An enumerable list of linked services that link input/output data sources (for example: Azure Blob Storage) toohello data factory.</span></span> <span data-ttu-id="ccf50-222">Ez a példa nincs Azure Storage használt bemeneti és kimeneti típusú csak egy társított szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="ccf50-222">In this sample, there is only one linked service of type Azure Storage used for both input and output.</span></span>
  2. <span data-ttu-id="ccf50-223">**adatkészletek**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-223">**datasets**.</span></span> <span data-ttu-id="ccf50-224">A rendszer egy enumerálható állnak.</span><span class="sxs-lookup"><span data-stu-id="ccf50-224">This is an enumerable list of datasets.</span></span> <span data-ttu-id="ccf50-225">A paraméter tooget hello helyek és -sémákat határozzák meg a bemeneti és kimeneti adathalmazokat is használhatja.</span><span class="sxs-lookup"><span data-stu-id="ccf50-225">You can use this parameter tooget hello locations and schemas defined by input and output datasets.</span></span>
  3. <span data-ttu-id="ccf50-226">**tevékenység**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-226">**activity**.</span></span> <span data-ttu-id="ccf50-227">Ez a paraméter hello aktuális számítási entitás - ebben az esetben az Azure Batch szolgáltatás jelöli.</span><span class="sxs-lookup"><span data-stu-id="ccf50-227">This parameter represents hello current compute entity - in this case, an Azure Batch service.</span></span>
  4. <span data-ttu-id="ccf50-228">**naplózó**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-228">**logger**.</span></span> <span data-ttu-id="ccf50-229">hello naplózó lehetővé teszi, hogy írható hibakeresési megjegyzések adott felület hello "User" keresse meg a folyamat hello.</span><span class="sxs-lookup"><span data-stu-id="ccf50-229">hello logger lets you write debug comments that surface as hello “User” log for hello pipeline.</span></span>
* <span data-ttu-id="ccf50-230">hello metódus, amely lehet használt toochain egyéni tevékenységek együtt hello jövőben dictionary adja vissza.</span><span class="sxs-lookup"><span data-stu-id="ccf50-230">hello method returns a dictionary that can be used toochain custom activities together in hello future.</span></span> <span data-ttu-id="ccf50-231">Ez a funkció még nem használható, így egy üres szótár visszaadásának hello metódus.</span><span class="sxs-lookup"><span data-stu-id="ccf50-231">This feature is not implemented yet, so return an empty dictionary from hello method.</span></span>

#### <a name="procedure-create-hello-custom-activity"></a><span data-ttu-id="ccf50-232">Az eljárás: Hello egyéni tevékenység létrehozása</span><span class="sxs-lookup"><span data-stu-id="ccf50-232">Procedure: Create hello custom activity</span></span>
1. <span data-ttu-id="ccf50-233">A Visual Studio .NET Class Library projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="ccf50-233">Create a .NET Class Library project in Visual Studio.</span></span>

   1. <span data-ttu-id="ccf50-234">Indítsa el **Visual Studio 2012**/**2013 vagy 2015**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-234">Launch **Visual Studio 2012**/**2013/2015**.</span></span>
   2. <span data-ttu-id="ccf50-235">Kattintson a **fájl**, pont túl**új**, és kattintson a **projekt**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-235">Click **File**, point too**New**, and click **Project**.</span></span>
   3. <span data-ttu-id="ccf50-236">Bontsa ki a **sablonok**, és válassza ki **Visual C\#**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-236">Expand **Templates**, and select **Visual C\#**.</span></span> <span data-ttu-id="ccf50-237">Ebben a bemutatóban használhat C\#, de használhatja a .NET nyelvi toodevelop hello egyéni tevékenység.</span><span class="sxs-lookup"><span data-stu-id="ccf50-237">In this walkthrough, you use C\#, but you can use any .NET language toodevelop hello custom activity.</span></span>
   4. <span data-ttu-id="ccf50-238">Válassza ki **Class Library** jobb hello projekttípusok hello listája.</span><span class="sxs-lookup"><span data-stu-id="ccf50-238">Select **Class Library** from hello list of project types on hello right.</span></span>
   5. <span data-ttu-id="ccf50-239">Adja meg **MyDotNetActivity** a hello **neve**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-239">Enter **MyDotNetActivity** for hello **Name**.</span></span>
   6. <span data-ttu-id="ccf50-240">Válassza ki **C:\\ADF** a hello **hely**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-240">Select **C:\\ADF** for hello **Location**.</span></span> <span data-ttu-id="ccf50-241">Hozzon létre hello mappát **ADF** Ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="ccf50-241">Create hello folder **ADF** if it does not exist.</span></span>
   7. <span data-ttu-id="ccf50-242">Kattintson a **OK** toocreate hello projekt.</span><span class="sxs-lookup"><span data-stu-id="ccf50-242">Click **OK** toocreate hello project.</span></span>
2. <span data-ttu-id="ccf50-243">Kattintson a **eszközök**, pont túl**NuGet-Csomagkezelő**, és kattintson a **Csomagkezelő konzol**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-243">Click **Tools**, point too**NuGet Package Manager**, and click **Package Manager Console**.</span></span>
3. <span data-ttu-id="ccf50-244">A hello **Csomagkezelő konzol**, hajtsa végre a következő parancs tooimport hello **Microsoft.Azure.Management.DataFactories**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-244">In hello **Package Manager Console**, execute hello following command tooimport **Microsoft.Azure.Management.DataFactories**.</span></span>

    ```powershell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. <span data-ttu-id="ccf50-245">Importálás hello **Azure Storage** toohello projekt NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="ccf50-245">Import hello **Azure Storage** NuGet package in toohello project.</span></span> <span data-ttu-id="ccf50-246">Ön ezt a csomagot kell végrehajtani, mert ez a példa hello Blob storage API-t használja.</span><span class="sxs-lookup"><span data-stu-id="ccf50-246">You need this package because you use hello Blob storage API in this sample.</span></span>

    ```powershell
    Install-Package Azure.Storage
    ```
5. <span data-ttu-id="ccf50-247">Adja hozzá a következő hello **használatával** irányelvek toohello forrásfájl hello projektben.</span><span class="sxs-lookup"><span data-stu-id="ccf50-247">Add hello following **using** directives toohello source file in hello project.</span></span>

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
6. <span data-ttu-id="ccf50-248">Hello nevének módosítása a hello **névtér** túl**MyDotNetActivityNS**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-248">Change hello name of hello **namespace** too**MyDotNetActivityNS**.</span></span>

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. <span data-ttu-id="ccf50-249">Hello osztály hello nevének módosítása túl**MyDotNetActivity** , és amelyek hello **IDotNetActivity** csatoló alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="ccf50-249">Change hello name of hello class too**MyDotNetActivity** and derive it from hello **IDotNetActivity** interface as shown below.</span></span>

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. <span data-ttu-id="ccf50-250">Megvalósítása (Hozzáadás) hello **Execute** hello metódusában **IDotNetActivity** toohello csatoló **MyDotNetActivity** minta kód toohello metódus a következő osztály és példány hello.</span><span class="sxs-lookup"><span data-stu-id="ccf50-250">Implement (Add) hello **Execute** method of hello **IDotNetActivity** interface toohello **MyDotNetActivity** class and copy hello following sample code toohello method.</span></span> <span data-ttu-id="ccf50-251">Lásd: hello [metódus végrehajtása](#execute-method) magyarázat hello logika, az ezzel a módszerrel a szakaszát.</span><span class="sxs-lookup"><span data-stu-id="ccf50-251">See hello [Execute Method](#execute-method) section for explanation for hello logic used in this method.</span></span>

    ```csharp
    /// <summary>
    /// Execute method is hello only method of IDotNetActivity interface you must implement.
    /// In this sample, hello method invokes hello Calculate method tooperform hello core logic.  
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
    
       // using First method instead of Single since we are using hello same
       // Azure Storage linked service for input and output.
       inputLinkedService = linkedServices.First(
           linkedService =>
           linkedService.Name ==
           inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
           as AzureStorageLinkedService;
    
       string connectionString = inputLinkedService.ConnectionString; // toocreate an input storage client.
       string folderPath = GetFolderPath(inputDataset);
       string output = string.Empty; // for use later.
    
       // create storage client for input. Pass hello connection string.
       CloudStorageAccount inputStorageAccount = CloudStorageAccount.Parse(connectionString);
       CloudBlobClient inputClient = inputStorageAccount.CreateCloudBlobClient();
    
       // initialize hello continuation token before using it in hello do-while loop.
       BlobContinuationToken continuationToken = null;
       do
       {   // get hello list of input blobs from hello input storage client object.
           BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
                                    true,
                                    BlobListingDetails.Metadata,
                                    null,
                                    continuationToken,
                                    null,
                                    null);
    
           // Calculate method returns hello number of occurrences of
           // hello search term (“Microsoft”) in each blob associated
           // with hello data slice.
           //
           // definition of hello method is shown in hello next step.
           output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
       } while (continuationToken != null);
    
       // get hello output dataset using hello name of hello dataset matched tooa name in hello Activity output collection.
       Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    
       folderPath = GetFolderPath(outputDataset);
    
       logger.Write("Writing blob toohello folder: {0}", folderPath);
    
       // create a storage object for hello output blob.
       CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
       // write hello name of hello file.
       Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    
       logger.Write("output blob URI: {0}", outputBlobUri.ToString());
       // create a blob and upload hello output text.
       CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
       logger.Write("Writing {0} toohello output blob", output);
       outputBlob.UploadText(output);
    
       // hello dictionary can be used toochain custom activities together in hello future.
       // This feature is not implemented yet, so just return an empty dictionary.
       return new Dictionary<string, string>();
    }
    ```
9. <span data-ttu-id="ccf50-252">Adja hozzá a következő módszerek toohello segítőosztály hello.</span><span class="sxs-lookup"><span data-stu-id="ccf50-252">Add hello following helper methods toohello class.</span></span> <span data-ttu-id="ccf50-253">Ezek a módszerek hívják hello **Execute** metódust.</span><span class="sxs-lookup"><span data-stu-id="ccf50-253">These methods are invoked by hello **Execute** method.</span></span> <span data-ttu-id="ccf50-254">A legfontosabb, hello **Calculate** metódus elkülöníti hello kódot, amely minden egyes blob telepítéseket.</span><span class="sxs-lookup"><span data-stu-id="ccf50-254">Most importantly, hello **Calculate** method isolates hello code that iterates through each blob.</span></span>

    ```csharp
    /// <summary>
    /// Gets hello folderPath value from hello input/output dataset.
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
    /// Gets hello fileName value from hello input/output dataset.
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
    /// Iterates through each blob (file) in hello folder, counts hello number of instances of search term in hello file,
    /// and prepares hello output text that is written toohello output blob.
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
               output += string.Format("{0} occurrences(s) of hello search term \"{1}\" were found in hello file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
           }
       }
       return output;
    }
    ```
    <span data-ttu-id="ccf50-255">Hello **GetFolderPath** metódus visszaadja hello toohello mappára, hogy hello dataset pontok tooand hello **GetFileName** metódus hello/fájlját, hogy a DataSet adatkészlet mutat hello hello nevét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="ccf50-255">hello **GetFolderPath** method returns hello path toohello folder that hello dataset points tooand hello **GetFileName** method returns hello name of hello blob/file that hello dataset points to.</span></span>

    ```csharp

    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
    ```

    <span data-ttu-id="ccf50-256">Hello **Calculate** metódus kiszámítja hello több példányban kulcsszó **Microsoft** hello bemeneti fájlban (BLOB hello mappában).</span><span class="sxs-lookup"><span data-stu-id="ccf50-256">hello **Calculate** method calculates hello number of instances of keyword **Microsoft** in hello input files (blobs in hello folder).</span></span> <span data-ttu-id="ccf50-257">hello keresési kifejezés ("Microsoft") nem változtatható hello kódban.</span><span class="sxs-lookup"><span data-stu-id="ccf50-257">hello search term (“Microsoft”) is hard-coded in hello code.</span></span>

1. <span data-ttu-id="ccf50-258">Fordítsa le hello projektet.</span><span class="sxs-lookup"><span data-stu-id="ccf50-258">Compile hello project.</span></span> <span data-ttu-id="ccf50-259">Kattintson a **Build** hello menüre, majd a **megoldás fordítása**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-259">Click **Build** from hello menu and click **Build Solution**.</span></span>
2. <span data-ttu-id="ccf50-260">Indítsa el **Windows Explorer**, és keresse meg a túl**bin\\debug** vagy **bin\\kiadási** mappa build hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="ccf50-260">Launch **Windows Explorer**, and navigate too**bin\\debug** or **bin\\release** folder depending on hello type of build.</span></span>
3. <span data-ttu-id="ccf50-261">Hozzon létre egy zip fájlt **MyDotNetActivity.zip** hello összes hello bináris fájlokat tartalmazó ** \\bin\\Debug** mappát.</span><span class="sxs-lookup"><span data-stu-id="ccf50-261">Create a zip file **MyDotNetActivity.zip** that contains all hello binaries in hello **\\bin\\Debug** folder.</span></span> <span data-ttu-id="ccf50-262">Érdemes lehet tooinclude hello MyDotNetActivity. **pdb** fájlt úgy, hogy be további részletekről, mint a sor számának hello forráskód, amely hello problémát az okozza, ha hiba lép fel.</span><span class="sxs-lookup"><span data-stu-id="ccf50-262">You may want tooinclude hello MyDotNetActivity.**pdb** file so that you get additional details such as line number in hello source code that caused hello issue when a failure occurs.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image5.png)
4. <span data-ttu-id="ccf50-263">Töltse fel **MyDotNetActivity.zip** blob toohello blob tárolóként: `customactivitycontainer` a hello Azure blob-tároló adott hello **StorageLinkedService** társított szolgáltatásnak az hello ** ADFTutorialDataFactory** használja.</span><span class="sxs-lookup"><span data-stu-id="ccf50-263">Upload **MyDotNetActivity.zip** as a blob toohello blob container: `customactivitycontainer` in hello Azure blob storage that hello **StorageLinkedService** linked service in hello **ADFTutorialDataFactory** uses.</span></span> <span data-ttu-id="ccf50-264">Hello blob-tároló létrehozása `customactivitycontainer` Ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="ccf50-264">Create hello blob container `customactivitycontainer` if it does not already exist.</span></span>

#### <a name="execute-method"></a><span data-ttu-id="ccf50-265">Metódus végrehajtása</span><span class="sxs-lookup"><span data-stu-id="ccf50-265">Execute method</span></span>
<span data-ttu-id="ccf50-266">Ez a témakör további részleteket és megjegyzéseket hello kód az Execute metódus hello.</span><span class="sxs-lookup"><span data-stu-id="ccf50-266">This section provides more details and notes about hello code in hello Execute method.</span></span>

1. <span data-ttu-id="ccf50-267">hello tagjainak iteráció hello bemeneti gyűjteményben találhatók hello [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) névtér.</span><span class="sxs-lookup"><span data-stu-id="ccf50-267">hello members for iterating through hello input collection are found in hello [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) namespace.</span></span> <span data-ttu-id="ccf50-268">Hello blob gyűjtemény iteráció szükséges hello segítségével **BlobContinuationToken** osztály.</span><span class="sxs-lookup"><span data-stu-id="ccf50-268">Iterating through hello blob collection requires using hello **BlobContinuationToken** class.</span></span> <span data-ttu-id="ccf50-269">Lényegében kell használnia a do-hello jogkivonattal való kilépés hello hurok hello mechanizmusként hurok közben.</span><span class="sxs-lookup"><span data-stu-id="ccf50-269">In essence, you must use a do-while loop with hello token as hello mechanism for exiting hello loop.</span></span> <span data-ttu-id="ccf50-270">További információkért lásd: [hogyan toouse Blob storage a .NET-](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="ccf50-270">For more information, see [How toouse Blob storage from .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="ccf50-271">Alapszintű hurkot itt jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="ccf50-271">A basic loop is shown here:</span></span>

    ```csharp
    // Initialize hello continuation token.
    BlobContinuationToken continuationToken = null;
    do
    {
    // Get hello list of input blobs from hello input storage client object.
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
   <span data-ttu-id="ccf50-272">Hello hello dokumentációjában [ListBlobsSegmented](https://msdn.microsoft.com/library/jj717596.aspx) részletes metódust.</span><span class="sxs-lookup"><span data-stu-id="ccf50-272">See hello documentation for hello [ListBlobsSegmented](https://msdn.microsoft.com/library/jj717596.aspx) method for details.</span></span>
2. <span data-ttu-id="ccf50-273">hello kód a belül hello használatához, áttekintve blobok hello készlete logikailag kerül tegye-ciklus során.</span><span class="sxs-lookup"><span data-stu-id="ccf50-273">hello code for working through hello set of blobs logically goes within hello do-while loop.</span></span> <span data-ttu-id="ccf50-274">A hello **Execute** metódust, hello tegye-közben hurok továbbítja blobok hello listája nevű tooa metódust **Calculate**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-274">In hello **Execute** method, hello do-while loop passes hello list of blobs tooa method named **Calculate**.</span></span> <span data-ttu-id="ccf50-275">hello metódus visszaadja egy karakterlánc-változóvá nevű **kimeneti** hello szegmensben lévő összes hello BLOB keresztül többször is rendelkezik, amely hello eredménye.</span><span class="sxs-lookup"><span data-stu-id="ccf50-275">hello method returns a string variable named **output** that is hello result of having iterated through all hello blobs in hello segment.</span></span>

   <span data-ttu-id="ccf50-276">Hello keresési kifejezés előfordulását hello számát adja vissza (**Microsoft**) hello BLOB átadott toohello **Calculate** metódust.</span><span class="sxs-lookup"><span data-stu-id="ccf50-276">It returns hello number of occurrences of hello search term (**Microsoft**) in hello blob passed toohello **Calculate** method.</span></span>

    ```csharp
    output += string.Format("{0} occurrences of hello search term \"{1}\" were found in hello file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
    ```
3. <span data-ttu-id="ccf50-277">Egyszer hello **Calculate** metódus hello munkahelyi megtörtént, akkor úgy kell megírni, tooa új blob.</span><span class="sxs-lookup"><span data-stu-id="ccf50-277">Once hello **Calculate** method has done hello work, it must be written tooa new blob.</span></span> <span data-ttu-id="ccf50-278">Így a blobok feldolgozása minden készlete esetében egy új blob hello eredmények írhatók.</span><span class="sxs-lookup"><span data-stu-id="ccf50-278">So for every set of blobs processed, a new blob can be written with hello results.</span></span> <span data-ttu-id="ccf50-279">toowrite tooa új blob, első keresés hello kimeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="ccf50-279">toowrite tooa new blob, first find hello output dataset.</span></span>

    ```csharp
    // Get hello output dataset using hello name of hello dataset matched tooa name in hello Activity output collection.
    Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    ```
4. <span data-ttu-id="ccf50-280">hello kódot is meghívja egy segédmetódust: **GetFolderPath** tooretrieve hello mappa elérési útja (hello tárolási tároló neve).</span><span class="sxs-lookup"><span data-stu-id="ccf50-280">hello code also calls a helper method: **GetFolderPath** tooretrieve hello folder path (hello storage container name).</span></span>

    ```csharp
    folderPath = GetFolderPath(outputDataset);
    ```
   <span data-ttu-id="ccf50-281">Hello **GetFolderPath** kiterjesztések hello DataSet objektum tooan AzureBlobDataSet, amelynek FolderPath nevű tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="ccf50-281">hello **GetFolderPath** casts hello DataSet object tooan AzureBlobDataSet, which has a property named FolderPath.</span></span>

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FolderPath;
    ```
5. <span data-ttu-id="ccf50-282">hello kód hívások hello **GetFileName** metódus tooretrieve hello fájl neve (blob neve).</span><span class="sxs-lookup"><span data-stu-id="ccf50-282">hello code calls hello **GetFileName** method tooretrieve hello file name (blob name).</span></span> <span data-ttu-id="ccf50-283">hello kód hasonló toohello kód tooget hello mappa elérési útja felett.</span><span class="sxs-lookup"><span data-stu-id="ccf50-283">hello code is similar toohello above code tooget hello folder path.</span></span>

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FileName;
    ```
6. <span data-ttu-id="ccf50-284">egy URI-objektum létrehozása hello fájl hello nevét írja.</span><span class="sxs-lookup"><span data-stu-id="ccf50-284">hello name of hello file is written by creating a URI object.</span></span> <span data-ttu-id="ccf50-285">hello URI konstruktor használ hello **BlobEndpoint** tulajdonság tooreturn hello tároló neve.</span><span class="sxs-lookup"><span data-stu-id="ccf50-285">hello URI constructor uses hello **BlobEndpoint** property tooreturn hello container name.</span></span> <span data-ttu-id="ccf50-286">hello mappa elérési útját és nevét tooconstruct hello kimeneti blob URI kerülnek.</span><span class="sxs-lookup"><span data-stu-id="ccf50-286">hello folder path and file name are added tooconstruct hello output blob URI.</span></span>  

    ```csharp
    // Write hello name of hello file.
    Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    ```
7. <span data-ttu-id="ccf50-287">hello fájl hello neve van írva, és most írja a hello kimeneti karakterlánc érkező hello **Calculate** metódus tooa új blob:</span><span class="sxs-lookup"><span data-stu-id="ccf50-287">hello name of hello file has been written and now you can write hello output string from hello **Calculate** method tooa new blob:</span></span>

    ```csharp
    // Create a blob and upload hello output text.
    CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
    logger.Write("Writing {0} toohello output blob", output);
    outputBlob.UploadText(output);
    ```

### <a name="create-hello-data-factory"></a><span data-ttu-id="ccf50-288">Hello adat-előállító létrehozása</span><span class="sxs-lookup"><span data-stu-id="ccf50-288">Create hello data factory</span></span>
<span data-ttu-id="ccf50-289">A hello [hello egyéni tevékenység létrehozása](#create-the-custom-activity) szakaszban létrehozott egyéni tevékenység és a zip-fájl feltöltése hello bináris fájljait és a hello PDB tooan Azure blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="ccf50-289">In hello [Create hello custom activity](#create-the-custom-activity) section, you created a custom activity and uploaded hello zip file with binaries and hello PDB file tooan Azure blob container.</span></span> <span data-ttu-id="ccf50-290">Ebben a szakaszban hoz létre egy Azure **adat-előállító** rendelkező egy **csővezeték** hello használó **egyéni tevékenység**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-290">In this section, you create an Azure **data factory** with a **pipeline** that uses hello **custom activity**.</span></span>

<span data-ttu-id="ccf50-291">hello bemeneti adatkészletet, az egyéni tevékenység hello hello blobok (fájlok) jelöli hello bemeneti mappában (`mycontainer\\inputfolder`) a blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="ccf50-291">hello input dataset for hello custom activity represents hello blobs (files) in hello input folder (`mycontainer\\inputfolder`) in blob storage.</span></span> <span data-ttu-id="ccf50-292">hello kimeneti adatkészlet a hello tevékenység jelöli hello kimeneti BLOB hello kimeneti mappában (`mycontainer\\outputfolder`) a blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="ccf50-292">hello output dataset for hello activity represents hello output blobs in hello output folder (`mycontainer\\outputfolder`) in blob storage.</span></span>

<span data-ttu-id="ccf50-293">Egy vagy több fájl eldobási hello bemeneti mappák:</span><span class="sxs-lookup"><span data-stu-id="ccf50-293">Drop one or more files in hello input folders:</span></span>

```
mycontainer -\> inputfolder
    2015-11-16-00
    2015-11-16-01
    2015-11-16-02
    2015-11-16-03
    2015-11-16-04
```

<span data-ttu-id="ccf50-294">Például egy fájl (file.txt) dobható el, a következő tartalom mindegyik hello mappák hello.</span><span class="sxs-lookup"><span data-stu-id="ccf50-294">For example, drop one file (file.txt) with hello following content into each of hello folders.</span></span>

```
test custom activity Microsoft test custom activity Microsoft
```

<span data-ttu-id="ccf50-295">Egyes bemeneti mappa felel meg az Azure Data Factory tooa szelet, akkor is, ha hello mappa 2 vagy több fájlt.</span><span class="sxs-lookup"><span data-stu-id="ccf50-295">Each input folder corresponds tooa slice in Azure Data Factory even if hello folder has 2 or more files.</span></span> <span data-ttu-id="ccf50-296">Minden szelet feldolgozása hello folyamat után hello egyéni tevékenység hello bemeneti mappában, hogy a szeletre vonatkozó összes hello BLOB telepítéseket.</span><span class="sxs-lookup"><span data-stu-id="ccf50-296">When each slice is processed by hello pipeline, hello custom activity iterates through all hello blobs in hello input folder for that slice.</span></span>

<span data-ttu-id="ccf50-297">Láthatja, hogy öt kimeneti fájl, amelynek hello azonos tartalom.</span><span class="sxs-lookup"><span data-stu-id="ccf50-297">You see five output files with hello same content.</span></span> <span data-ttu-id="ccf50-298">Például a hello kimeneti fájl hello 2015-11-16-00 mappában hello fájl feldolgozása a tartalom a következő hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="ccf50-298">For example, hello output file from processing hello file in hello 2015-11-16-00 folder has hello following content:</span></span>

```
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file.txt.
```

<span data-ttu-id="ccf50-299">Ha több fájl (file.txt, fájl2.ref fájllal, file3.txt) eldobja a hello azonos tartalommappa toohello bemeneti úgy, hogy a következő hello kimeneti fájl tartalma hello.</span><span class="sxs-lookup"><span data-stu-id="ccf50-299">If you drop multiple files (file.txt, file2.txt, file3.txt) with hello same content toohello input folder, you see hello following content in hello output file.</span></span> <span data-ttu-id="ccf50-300">Minden mappa (2015-11-16-00, stb.) felel meg ez a példa tooa szelet, annak ellenére, hogy hello mappa több bemeneti fájlokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ccf50-300">Each folder (2015-11-16-00, etc.) corresponds tooa slice in this sample even though hello folder has multiple input files.</span></span>

```csharp
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file.txt.
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file2.txt.
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file3.txt.
```

<span data-ttu-id="ccf50-301">hello kimeneti fájl három sort, egy a hello szelet (2015-11-16-00) társított hello mappa minden a bemeneti fájl (blob) rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ccf50-301">hello output file has three lines now, one for each input file (blob) in hello folder associated with hello slice (2015-11-16-00).</span></span>

<span data-ttu-id="ccf50-302">Egy feladat minden egyes tevékenységfuttatási jön létre.</span><span class="sxs-lookup"><span data-stu-id="ccf50-302">A task is created for each activity run.</span></span> <span data-ttu-id="ccf50-303">Ez a példa nincs csak egy tevékenység hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="ccf50-303">In this sample, there is only one activity in hello pipeline.</span></span> <span data-ttu-id="ccf50-304">A szelet feldolgozása hello folyamat után hello egyéni tevékenység az Azure Batch tooprocess hello szelet fut.</span><span class="sxs-lookup"><span data-stu-id="ccf50-304">When a slice is processed by hello pipeline, hello custom activity runs on Azure Batch tooprocess hello slice.</span></span> <span data-ttu-id="ccf50-305">Nincsenek öt szeletek (minden szelet rendelkezhet több bináris objektumok vagy fájl), mert nincsenek Azure Batch létrehozott öt feladatok.</span><span class="sxs-lookup"><span data-stu-id="ccf50-305">Since there are five slices (each slice can have multiple blobs or file), there are five tasks created in Azure Batch.</span></span> <span data-ttu-id="ccf50-306">Kötegelt feladatként fut, esetén ténylegesen hello egyéni tevékenység fut-e.</span><span class="sxs-lookup"><span data-stu-id="ccf50-306">When a task runs on Batch, it is actually hello custom activity that is running.</span></span>

<span data-ttu-id="ccf50-307">a következő forgatókönyv hello további részleteket biztosít.</span><span class="sxs-lookup"><span data-stu-id="ccf50-307">hello following walkthrough provides additional details.</span></span>

#### <a name="step-1-create-hello-data-factory"></a><span data-ttu-id="ccf50-308">1. lépés: Hello adat-előállító létrehozása</span><span class="sxs-lookup"><span data-stu-id="ccf50-308">Step 1: Create hello data factory</span></span>
1. <span data-ttu-id="ccf50-309">Toohello a bejelentkezés után [Azure-portálon](https://portal.azure.com/), hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="ccf50-309">After logging in toohello [Azure portal](https://portal.azure.com/), do hello following steps:</span></span>

   1. <span data-ttu-id="ccf50-310">Kattintson a **új** hello bal oldali menüben.</span><span class="sxs-lookup"><span data-stu-id="ccf50-310">Click **NEW** on hello left menu.</span></span>
   2. <span data-ttu-id="ccf50-311">Kattintson a **adatok + analitika** a hello **új** panelen.</span><span class="sxs-lookup"><span data-stu-id="ccf50-311">Click **Data + Analytics** in hello **New** blade.</span></span>
   3. <span data-ttu-id="ccf50-312">Kattintson a **adat-előállító** a hello **adatelemzés** panelen.</span><span class="sxs-lookup"><span data-stu-id="ccf50-312">Click **Data Factory** on hello **Data analytics** blade.</span></span>
2. <span data-ttu-id="ccf50-313">A hello **új adat-előállító** panelen adja meg **CustomActivityFactory** a hello nevét.</span><span class="sxs-lookup"><span data-stu-id="ccf50-313">In hello **New data factory** blade, enter **CustomActivityFactory** for hello Name.</span></span> <span data-ttu-id="ccf50-314">az Azure data factory hello hello nevének globálisan egyedi kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ccf50-314">hello name of hello Azure data factory must be globally unique.</span></span> <span data-ttu-id="ccf50-315">Ha hello hibaüzenetet kapja: **nem érhető el adat-előállító "CustomActivityFactory"**, adat-előállító hello hello nevének módosítása (például **yournameCustomActivityFactory**), és próbálja meg létrehozni újra.</span><span class="sxs-lookup"><span data-stu-id="ccf50-315">If you receive hello error: **Data factory name “CustomActivityFactory” is not available**, change hello name of hello data factory (for example, **yournameCustomActivityFactory**) and try creating again.</span></span>
3. <span data-ttu-id="ccf50-316">Kattintson a **ERŐFORRÁSCSOPORT-név**, és válasszon ki egy meglévő erőforráscsoportot, vagy hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="ccf50-316">Click **RESOURCE GROUP NAME**, and select an existing resource group or create a resource group.</span></span>
4. <span data-ttu-id="ccf50-317">Győződjön meg arról, hogy megfelelő előfizetés hello és hello data factory toobe létrehozott eső használja.</span><span class="sxs-lookup"><span data-stu-id="ccf50-317">Verify that you are using hello correct subscription and region where you want hello data factory toobe created.</span></span>
5. <span data-ttu-id="ccf50-318">Kattintson a **létrehozása** a hello **új adat-előállító** panelen.</span><span class="sxs-lookup"><span data-stu-id="ccf50-318">Click **Create** on hello **New data factory** blade.</span></span>
6. <span data-ttu-id="ccf50-319">Hello adat-előállító létrehozása hello látja **irányítópult** a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="ccf50-319">You see hello data factory being created in hello **Dashboard** of hello Azure portal.</span></span>
7. <span data-ttu-id="ccf50-320">Miután hello adat-előállító létrehozása sikerült, oldal akkor jelenik meg hello adatok gyári, amely jelzi, hogy hello hello adat-előállító tartalmát.</span><span class="sxs-lookup"><span data-stu-id="ccf50-320">After hello data factory has been created successfully, you see hello data factory page, which shows you hello contents of hello data factory.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image6.png)

#### <a name="step-2-create-linked-services"></a><span data-ttu-id="ccf50-321">2. lépés: A társított szolgáltatások létrehozásához</span><span class="sxs-lookup"><span data-stu-id="ccf50-321">Step 2: Create linked services</span></span>
<span data-ttu-id="ccf50-322">Összekapcsolt szolgáltatások adattárolókhoz hivatkozásra, vagy a szolgáltatások tooan az Azure data factory számítási.</span><span class="sxs-lookup"><span data-stu-id="ccf50-322">Linked services link data stores or compute services tooan Azure data factory.</span></span> <span data-ttu-id="ccf50-323">Ebben a lépésben hivatkozásra a **Azure Storage** fiók és **Azure Batch** fiók tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="ccf50-323">In this step, you link your **Azure Storage** account and **Azure Batch** account tooyour data factory.</span></span>

#### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="ccf50-324">Azure Storage társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ccf50-324">Create Azure Storage linked service</span></span>
1. <span data-ttu-id="ccf50-325">Kattintson a hello **Szerző és központi telepítése** hello csempét **DATA FACTORY** paneljén **CustomActivityFactory**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-325">Click hello **Author and deploy** tile on hello **DATA FACTORY** blade for **CustomActivityFactory**.</span></span> <span data-ttu-id="ccf50-326">Megjelenik a Data Factory Editor hello.</span><span class="sxs-lookup"><span data-stu-id="ccf50-326">You see hello Data Factory Editor.</span></span>
2. <span data-ttu-id="ccf50-327">Kattintson a **az új adattároló** a hello parancssávon, és válassza a **az Azure storage.**</span><span class="sxs-lookup"><span data-stu-id="ccf50-327">Click **New data store** on hello command bar and choose **Azure storage.**</span></span> <span data-ttu-id="ccf50-328">Megtekintheti az hello JSON-parancsfájl létrehozásához egy Azure Storage társított szolgáltatásnak hello-szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="ccf50-328">You should see hello JSON script for creating an Azure Storage linked service in hello editor.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image7.png)

3. <span data-ttu-id="ccf50-329">Cserélje le **fióknév** hello nevű Azure-tárfiókot és **fiókkulcs** kulcsával hello hozzáférés hello Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="ccf50-329">Replace **account name** with hello name of your Azure storage account and **account key** with hello access key of hello Azure storage account.</span></span> <span data-ttu-id="ccf50-330">toolearn hogyan tooget tárhelyét férnek hozzá, tekintse meg [megtekintése, másolása és újragenerálása tárolási hívóbetűk](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="ccf50-330">toolearn how tooget your storage access key, see [View, copy and regenerate storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>

4. <span data-ttu-id="ccf50-331">Kattintson a **telepítés** a hello parancssávon toodeploy kapcsolódó hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="ccf50-331">Click **Deploy** on hello command bar toodeploy hello linked service.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image8.png)

#### <a name="create-azure-batch-linked-service"></a><span data-ttu-id="ccf50-332">Csatolt Azure Batch szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ccf50-332">Create Azure Batch linked service</span></span>
<span data-ttu-id="ccf50-333">Ebben a lépésben a társított szolgáltatás létrehozása a **Azure Batch** használt toorun hello adat-előállító egyéni tevékenység fiókkal.</span><span class="sxs-lookup"><span data-stu-id="ccf50-333">In this step, you create a linked service for your **Azure Batch** account that is used toorun hello Data Factory custom activity.</span></span>

1. <span data-ttu-id="ccf50-334">Kattintson a **új számítási** a hello parancssávon, és válassza a **Azure Batch.**</span><span class="sxs-lookup"><span data-stu-id="ccf50-334">Click **New compute** on hello command bar and choose **Azure Batch.**</span></span> <span data-ttu-id="ccf50-335">Megjelenik a JSON-parancsfájl létrehozásához az Azure Batch hello társított szolgáltatás hello-szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="ccf50-335">You should see hello JSON script for creating an Azure Batch linked service in hello editor.</span></span>
2. <span data-ttu-id="ccf50-336">A JSON-parancsfájl hello:</span><span class="sxs-lookup"><span data-stu-id="ccf50-336">In hello JSON script:</span></span>

   1. <span data-ttu-id="ccf50-337">Cserélje le **fióknév** hello nevet, az Azure Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="ccf50-337">Replace **account name** with hello name of your Azure Batch account.</span></span>
   2. <span data-ttu-id="ccf50-338">Cserélje le **hozzáférési kulcs** kulcsával hello hozzáférés hello Azure Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="ccf50-338">Replace **access key** with hello access key of hello Azure Batch account.</span></span>
   3. <span data-ttu-id="ccf50-339">Adjon meg hello Azonosítót hello készlet hello **poolName** tulajdonság**.**</span><span class="sxs-lookup"><span data-stu-id="ccf50-339">Enter hello ID of hello pool for hello **poolName** property**.**</span></span> <span data-ttu-id="ccf50-340">Ehhez a tulajdonsághoz adja meg vagy készlet nevét vagy tárolókészlet azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="ccf50-340">For this property, you can specify either pool name or pool ID.</span></span>
   4. <span data-ttu-id="ccf50-341">Adja meg a hello köteg URI hello **batchUri** JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="ccf50-341">Enter hello batch URI for hello **batchUri** JSON property.</span></span>

      > [!IMPORTANT]
      > <span data-ttu-id="ccf50-342">Hello **URL-cím** a hello **Azure Batch-fiók panelen** hello formátuma a következő szerepel: \<accountname\>.\< régió\>. batch.azure.com. A hello **batchUri** hello JSON tulajdonságot, kell túl**távolítsa el a "accountname."**</span><span class="sxs-lookup"><span data-stu-id="ccf50-342">hello **URL** from hello **Azure Batch account blade** is in hello following format: \<accountname\>.\<region\>.batch.azure.com. For hello **batchUri** property in hello JSON, you need too**remove "accountname."**</span></span> <span data-ttu-id="ccf50-343">a hello URL-CÍMRŐL.</span><span class="sxs-lookup"><span data-stu-id="ccf50-343">from hello URL.</span></span> <span data-ttu-id="ccf50-344">Példa: `"batchUri": "https://eastus.batch.azure.com"`.</span><span class="sxs-lookup"><span data-stu-id="ccf50-344">Example: `"batchUri": "https://eastus.batch.azure.com"`.</span></span>
      >
      >

      ![](./media/data-factory-data-processing-using-batch/image9.png)

      <span data-ttu-id="ccf50-345">A hello **poolName** tulajdonság, azt is megadhatja, hello azonosító hello készlet hello hello neve helyett.</span><span class="sxs-lookup"><span data-stu-id="ccf50-345">For hello **poolName** property, you can also specify hello ID of hello pool instead of hello name of hello pool.</span></span>

      > [!NOTE]
      > <span data-ttu-id="ccf50-346">hello Data Factory szolgáltatásnak nem támogatja egy igény szerinti beállítást az Azure hdinsight azonban nem.</span><span class="sxs-lookup"><span data-stu-id="ccf50-346">hello Data Factory service does not support an on-demand option for Azure Batch as it does for HDInsight.</span></span> <span data-ttu-id="ccf50-347">Csak a saját Azure Batch-készlet használhatja az Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="ccf50-347">You can only use your own Azure Batch pool in an Azure data factory.</span></span>
      >
      >
   5. <span data-ttu-id="ccf50-348">Adja meg **StorageLinkedService** a hello **linkedServiceName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="ccf50-348">Specify **StorageLinkedService** for hello **linkedServiceName** property.</span></span> <span data-ttu-id="ccf50-349">Hello előző lépésben létrehozott szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="ccf50-349">You created this linked service in hello previous step.</span></span> <span data-ttu-id="ccf50-350">A tárolót használja a rendszer egy átmeneti területre, fájlok és a naplókat.</span><span class="sxs-lookup"><span data-stu-id="ccf50-350">This storage is used as a staging area for files and logs.</span></span>
3. <span data-ttu-id="ccf50-351">Kattintson a **telepítés** a hello parancssávon toodeploy kapcsolódó hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="ccf50-351">Click **Deploy** on hello command bar toodeploy hello linked service.</span></span>

#### <a name="step-3-create-datasets"></a><span data-ttu-id="ccf50-352">3. lépés: Adatkészletek létrehozása</span><span class="sxs-lookup"><span data-stu-id="ccf50-352">Step 3: Create datasets</span></span>
<span data-ttu-id="ccf50-353">Ebben a lépésben létrehozni adatkészletek toorepresent bemeneti és kimeneti adatai.</span><span class="sxs-lookup"><span data-stu-id="ccf50-353">In this step, you create datasets toorepresent input and output data.</span></span>

#### <a name="create-input-dataset"></a><span data-ttu-id="ccf50-354">Bemeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="ccf50-354">Create input dataset</span></span>
1. <span data-ttu-id="ccf50-355">A hello **szerkesztő** hello adat-előállítót, kattintson **új adatkészlet** elemre, majd hello eszköztár gomb **Azure Blob Storage tárolóban** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="ccf50-355">In hello **Editor** for hello Data Factory, click **New dataset** button on hello toolbar and click **Azure Blob storage** from hello drop-down menu.</span></span>
2. <span data-ttu-id="ccf50-356">Cserélje le a következő JSON részlet hello hello JSON hello jobb oldali ablaktáblában:</span><span class="sxs-lookup"><span data-stu-id="ccf50-356">Replace hello JSON in hello right pane with hello following JSON snippet:</span></span>

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

    <span data-ttu-id="ccf50-357">Ez a forgatókönyv a kezdő időpont későbbi részében hozzon létre egy folyamatot: 2015-11-16T00:00:00Z és záró idő: 2015-11-16T05:00:00Z.</span><span class="sxs-lookup"><span data-stu-id="ccf50-357">You create a pipeline later in this walkthrough with start time: 2015-11-16T00:00:00Z and end time: 2015-11-16T05:00:00Z.</span></span> <span data-ttu-id="ccf50-358">Ütemezett tooproduce adatok **óránkénti**, így 5 bemeneti/kimeneti szeletek (közötti **00**: 00:00 -\> **05**: 00:00).</span><span class="sxs-lookup"><span data-stu-id="ccf50-358">It is scheduled tooproduce data **hourly**, so there are 5 input/output slices (between **00**:00:00 -\> **05**:00:00).</span></span>

    <span data-ttu-id="ccf50-359">Hello **gyakoriság** és **időköz** hello bemeneti adatkészlet túl van-e állítva a**óra** és **1**, ami azt jelenti, hogy hello bemeneti szelet érhető el óránként.</span><span class="sxs-lookup"><span data-stu-id="ccf50-359">hello **frequency** and **interval** for hello input dataset is set too**Hour** and **1**, which means that hello input slice is available hourly.</span></span>

    <span data-ttu-id="ccf50-360">Az alábbiakban hello indítási idejének az egyes szeletek, ami által képviselt **SliceStart** hello JSON részlet újabb rendszer változót.</span><span class="sxs-lookup"><span data-stu-id="ccf50-360">Here are hello start times for each slice, which is represented by **SliceStart** system variable in hello above JSON snippet.</span></span>

    | <span data-ttu-id="ccf50-361">**Szelet**</span><span class="sxs-lookup"><span data-stu-id="ccf50-361">**Slice**</span></span> | <span data-ttu-id="ccf50-362">**Kezdési idő**</span><span class="sxs-lookup"><span data-stu-id="ccf50-362">**Start time**</span></span>          |
    |-----------|-------------------------|
    | <span data-ttu-id="ccf50-363">1</span><span class="sxs-lookup"><span data-stu-id="ccf50-363">1</span></span>         | <span data-ttu-id="ccf50-364">2015-11-16T**00**: 00:00</span><span class="sxs-lookup"><span data-stu-id="ccf50-364">2015-11-16T**00**:00:00</span></span> |
    | <span data-ttu-id="ccf50-365">2</span><span class="sxs-lookup"><span data-stu-id="ccf50-365">2</span></span>         | <span data-ttu-id="ccf50-366">2015-11-16T**01**: 00:00</span><span class="sxs-lookup"><span data-stu-id="ccf50-366">2015-11-16T**01**:00:00</span></span> |
    | <span data-ttu-id="ccf50-367">3</span><span class="sxs-lookup"><span data-stu-id="ccf50-367">3</span></span>         | <span data-ttu-id="ccf50-368">2015-11-16T**02**: 00:00</span><span class="sxs-lookup"><span data-stu-id="ccf50-368">2015-11-16T**02**:00:00</span></span> |
    | <span data-ttu-id="ccf50-369">4</span><span class="sxs-lookup"><span data-stu-id="ccf50-369">4</span></span>         | <span data-ttu-id="ccf50-370">2015-11-16T**03**: 00:00</span><span class="sxs-lookup"><span data-stu-id="ccf50-370">2015-11-16T**03**:00:00</span></span> |
    | <span data-ttu-id="ccf50-371">5</span><span class="sxs-lookup"><span data-stu-id="ccf50-371">5</span></span>         | <span data-ttu-id="ccf50-372">2015-11-16T**04**: 00:00</span><span class="sxs-lookup"><span data-stu-id="ccf50-372">2015-11-16T**04**:00:00</span></span> |

    <span data-ttu-id="ccf50-373">Hello **folderPath** által hello év, hónap, nap és óra részét hello szelet kezdete (**SliceStart**).</span><span class="sxs-lookup"><span data-stu-id="ccf50-373">hello **folderPath** is calculated by using hello year, month, day, and hour part of hello slice start time (**SliceStart**).</span></span> <span data-ttu-id="ccf50-374">Itt ezért csatlakoztatott tooa szelet Mitől egy bemeneti mappa.</span><span class="sxs-lookup"><span data-stu-id="ccf50-374">Therefore, here is how an input folder is mapped tooa slice.</span></span>

    | <span data-ttu-id="ccf50-375">**Szelet**</span><span class="sxs-lookup"><span data-stu-id="ccf50-375">**Slice**</span></span> | <span data-ttu-id="ccf50-376">**Kezdési idő**</span><span class="sxs-lookup"><span data-stu-id="ccf50-376">**Start time**</span></span>          | <span data-ttu-id="ccf50-377">**Bemeneti mappa**</span><span class="sxs-lookup"><span data-stu-id="ccf50-377">**Input folder**</span></span>  |
    |-----------|-------------------------|-------------------|
    | <span data-ttu-id="ccf50-378">1</span><span class="sxs-lookup"><span data-stu-id="ccf50-378">1</span></span>         | <span data-ttu-id="ccf50-379">2015-11-16T**00**: 00:00</span><span class="sxs-lookup"><span data-stu-id="ccf50-379">2015-11-16T**00**:00:00</span></span> | <span data-ttu-id="ccf50-380">2015-11-16-**00**</span><span class="sxs-lookup"><span data-stu-id="ccf50-380">2015-11-16-**00**</span></span> |
    | <span data-ttu-id="ccf50-381">2</span><span class="sxs-lookup"><span data-stu-id="ccf50-381">2</span></span>         | <span data-ttu-id="ccf50-382">2015-11-16T**01**: 00:00</span><span class="sxs-lookup"><span data-stu-id="ccf50-382">2015-11-16T**01**:00:00</span></span> | <span data-ttu-id="ccf50-383">2015-11-16-**01**</span><span class="sxs-lookup"><span data-stu-id="ccf50-383">2015-11-16-**01**</span></span> |
    | <span data-ttu-id="ccf50-384">3</span><span class="sxs-lookup"><span data-stu-id="ccf50-384">3</span></span>         | <span data-ttu-id="ccf50-385">2015-11-16T**02**: 00:00</span><span class="sxs-lookup"><span data-stu-id="ccf50-385">2015-11-16T**02**:00:00</span></span> | <span data-ttu-id="ccf50-386">2015-11-16-**02**</span><span class="sxs-lookup"><span data-stu-id="ccf50-386">2015-11-16-**02**</span></span> |
    | <span data-ttu-id="ccf50-387">4</span><span class="sxs-lookup"><span data-stu-id="ccf50-387">4</span></span>         | <span data-ttu-id="ccf50-388">2015-11-16T**03**: 00:00</span><span class="sxs-lookup"><span data-stu-id="ccf50-388">2015-11-16T**03**:00:00</span></span> | <span data-ttu-id="ccf50-389">2015-11-16-**03**</span><span class="sxs-lookup"><span data-stu-id="ccf50-389">2015-11-16-**03**</span></span> |
    | <span data-ttu-id="ccf50-390">5</span><span class="sxs-lookup"><span data-stu-id="ccf50-390">5</span></span>         | <span data-ttu-id="ccf50-391">2015-11-16T**04**: 00:00</span><span class="sxs-lookup"><span data-stu-id="ccf50-391">2015-11-16T**04**:00:00</span></span> | <span data-ttu-id="ccf50-392">2015-11-16-**04**</span><span class="sxs-lookup"><span data-stu-id="ccf50-392">2015-11-16-**04**</span></span> |

1. <span data-ttu-id="ccf50-393">Kattintson a **telepítés** eszköztár toocreate hello és központi telepítése hello **InputDataset** tábla.</span><span class="sxs-lookup"><span data-stu-id="ccf50-393">Click **Deploy** on hello toolbar toocreate and deploy hello **InputDataset** table.</span></span>

#### <a name="create-output-dataset"></a><span data-ttu-id="ccf50-394">Kimeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="ccf50-394">Create output dataset</span></span>
<span data-ttu-id="ccf50-395">Ebben a lépésben egy másik dataset típusú AzureBlob toorepresent hello kimeneti adatokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ccf50-395">In this step, you create another dataset of type AzureBlob toorepresent hello output data.</span></span>

1. <span data-ttu-id="ccf50-396">A hello **szerkesztő** hello adat-előállítót, kattintson **új adatkészlet** elemre, majd hello eszköztár gomb **Azure Blob Storage tárolóban** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="ccf50-396">In hello **Editor** for hello Data Factory, click **New dataset** button on hello toolbar and click **Azure Blob storage** from hello drop-down menu.</span></span>
2. <span data-ttu-id="ccf50-397">Cserélje le a következő JSON részlet hello hello JSON hello jobb oldali ablaktáblában:</span><span class="sxs-lookup"><span data-stu-id="ccf50-397">Replace hello JSON in hello right pane with hello following JSON snippet:</span></span>

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

    <span data-ttu-id="ccf50-398">Egy kimeneti blob/fájl az egyes bemeneti szeletek jön létre.</span><span class="sxs-lookup"><span data-stu-id="ccf50-398">An output blob/file is generated for each input slice.</span></span> <span data-ttu-id="ccf50-399">Ez hogyan kimeneti fájl neve az egyes szeletek.</span><span class="sxs-lookup"><span data-stu-id="ccf50-399">Here is how an output file is named for each slice.</span></span> <span data-ttu-id="ccf50-400">Minden hello kimeneti fájlok akkor jönnek létre, egy kimeneti mappában: `mycontainer\\outputfolder`.</span><span class="sxs-lookup"><span data-stu-id="ccf50-400">All hello output files are generated in one output folder: `mycontainer\\outputfolder`.</span></span>

    | <span data-ttu-id="ccf50-401">**Szelet**</span><span class="sxs-lookup"><span data-stu-id="ccf50-401">**Slice**</span></span> | <span data-ttu-id="ccf50-402">**Kezdési idő**</span><span class="sxs-lookup"><span data-stu-id="ccf50-402">**Start time**</span></span>          | <span data-ttu-id="ccf50-403">**Kimeneti fájlja**</span><span class="sxs-lookup"><span data-stu-id="ccf50-403">**Output file**</span></span>       |
    |-----------|-------------------------|-----------------------|
    | <span data-ttu-id="ccf50-404">1</span><span class="sxs-lookup"><span data-stu-id="ccf50-404">1</span></span>         | <span data-ttu-id="ccf50-405">2015-11-16T**00**: 00:00</span><span class="sxs-lookup"><span data-stu-id="ccf50-405">2015-11-16T**00**:00:00</span></span> | <span data-ttu-id="ccf50-406">2015-11-16 -**00. txt**</span><span class="sxs-lookup"><span data-stu-id="ccf50-406">2015-11-16-**00.txt**</span></span> |
    | <span data-ttu-id="ccf50-407">2</span><span class="sxs-lookup"><span data-stu-id="ccf50-407">2</span></span>         | <span data-ttu-id="ccf50-408">2015-11-16T**01**: 00:00</span><span class="sxs-lookup"><span data-stu-id="ccf50-408">2015-11-16T**01**:00:00</span></span> | <span data-ttu-id="ccf50-409">2015-11-16 -**01. txt**</span><span class="sxs-lookup"><span data-stu-id="ccf50-409">2015-11-16-**01.txt**</span></span> |
    | <span data-ttu-id="ccf50-410">3</span><span class="sxs-lookup"><span data-stu-id="ccf50-410">3</span></span>         | <span data-ttu-id="ccf50-411">2015-11-16T**02**: 00:00</span><span class="sxs-lookup"><span data-stu-id="ccf50-411">2015-11-16T**02**:00:00</span></span> | <span data-ttu-id="ccf50-412">2015-11-16 -**02. txt**</span><span class="sxs-lookup"><span data-stu-id="ccf50-412">2015-11-16-**02.txt**</span></span> |
    | <span data-ttu-id="ccf50-413">4</span><span class="sxs-lookup"><span data-stu-id="ccf50-413">4</span></span>         | <span data-ttu-id="ccf50-414">2015-11-16T**03**: 00:00</span><span class="sxs-lookup"><span data-stu-id="ccf50-414">2015-11-16T**03**:00:00</span></span> | <span data-ttu-id="ccf50-415">2015-11-16 -**03. txt**</span><span class="sxs-lookup"><span data-stu-id="ccf50-415">2015-11-16-**03.txt**</span></span> |
    | <span data-ttu-id="ccf50-416">5</span><span class="sxs-lookup"><span data-stu-id="ccf50-416">5</span></span>         | <span data-ttu-id="ccf50-417">2015-11-16T**04**: 00:00</span><span class="sxs-lookup"><span data-stu-id="ccf50-417">2015-11-16T**04**:00:00</span></span> | <span data-ttu-id="ccf50-418">2015-11-16 -**04. txt**</span><span class="sxs-lookup"><span data-stu-id="ccf50-418">2015-11-16-**04.txt**</span></span> |

    <span data-ttu-id="ccf50-419">Ne feledje, hogy az összes hello egy bemeneti mappában lévő fájlok (például: 2015-11-16-00) hello kezdési időponttal szelet részét képezik: 2015-11-16-00.</span><span class="sxs-lookup"><span data-stu-id="ccf50-419">Remember that all hello files in an input folder (for example: 2015-11-16-00) are part of a slice with hello start time: 2015-11-16-00.</span></span> <span data-ttu-id="ccf50-420">A szelet feldolgozása után hello egyéni tevékenység minden egyes fájl vizsgálja, és hozza létre a keresési kifejezés ("Microsoft") előfordulásainak száma hello hello kimeneti fájlban egy sor.</span><span class="sxs-lookup"><span data-stu-id="ccf50-420">When this slice is processed, hello custom activity scans through each file and produces a line in hello output file with hello number of occurrences of search term (“Microsoft”).</span></span> <span data-ttu-id="ccf50-421">Ha nincsenek három fájlok 2015-11-16-00 hello mappában, vannak-e három sort hello kimeneti fájl: 2015-11-16-00.txt.</span><span class="sxs-lookup"><span data-stu-id="ccf50-421">If there are three files in hello folder 2015-11-16-00, there are three lines in hello output file: 2015-11-16-00.txt.</span></span>

1. <span data-ttu-id="ccf50-422">Kattintson a **telepítés** eszköztár toocreate hello és központi telepítése hello **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-422">Click **Deploy** on hello toolbar toocreate and deploy hello **OutputDataset**.</span></span>

#### <a name="step-4-create-and-run-hello-pipeline-with-custom-activity"></a><span data-ttu-id="ccf50-423">4. lépés: Hozzon létre, és futtassa a hello csővezeték egyéni tevékenységet</span><span class="sxs-lookup"><span data-stu-id="ccf50-423">Step 4: Create and run hello pipeline with custom activity</span></span>
<span data-ttu-id="ccf50-424">Ebben a lépésben létrehoz egy folyamatot egy tevékenységet, amelyet előzőleg létrehozott egyéni tevékenység hello.</span><span class="sxs-lookup"><span data-stu-id="ccf50-424">In this step, you create a pipeline with one activity, hello custom activity you created earlier.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ccf50-425">Ha még nem töltött fel hello **file.txt** tooinput mappák hello blob a tárolóban, ehhez hello folyamat létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="ccf50-425">If you haven't uploaded hello **file.txt** tooinput folders in hello blob container, do so before creating hello pipeline.</span></span> <span data-ttu-id="ccf50-426">Hello **isPaused** tulajdonsága toofalse hello adatcsatorna JSON-NÁ, így hello folyamat fut a azonnal hello **start** dátuma múltbeli hello van.</span><span class="sxs-lookup"><span data-stu-id="ccf50-426">hello **isPaused** property is set toofalse in hello pipeline JSON, so hello pipeline runs immediately as hello **start** date is in hello past.</span></span>
>
>

1. <span data-ttu-id="ccf50-427">A Data Factory Editor hello, kattintson **új adatcsatorna** hello parancssávon.</span><span class="sxs-lookup"><span data-stu-id="ccf50-427">In hello Data Factory Editor, click **New pipeline** on hello command bar.</span></span> <span data-ttu-id="ccf50-428">Ha nem látja hello parancsot, kattintson a **... Három (pont) ** toosee azt.</span><span class="sxs-lookup"><span data-stu-id="ccf50-428">If you do not see hello command, click **... (Ellipsis)** toosee it.</span></span>
2. <span data-ttu-id="ccf50-429">Cserélje le a JSON-parancsfájl a következő hello hello JSON hello jobb oldali ablaktáblában:</span><span class="sxs-lookup"><span data-stu-id="ccf50-429">Replace hello JSON in hello right pane with hello following JSON script:</span></span>

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
   <span data-ttu-id="ccf50-430">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="ccf50-430">Note hello following points:</span></span>

   * <span data-ttu-id="ccf50-431">Csak egy tevékenység hello feldolgozási és típusa, amely nem: **DotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-431">There is only one activity in hello pipeline and that is of type: **DotNetActivity**.</span></span>
   * <span data-ttu-id="ccf50-432">**AssemblyName** hello DLL toohello neve van beállítva: **MyDotNetActivity.dll**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-432">**AssemblyName** is set toohello name of hello DLL: **MyDotNetActivity.dll**.</span></span>
   * <span data-ttu-id="ccf50-433">**EntryPoint** értéke túl**MyDotNetActivityNS.MyDotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-433">**EntryPoint** is set too**MyDotNetActivityNS.MyDotNetActivity**.</span></span> <span data-ttu-id="ccf50-434">Alapvetően van \<névtér\>.\< osztálynév\> a kódban.</span><span class="sxs-lookup"><span data-stu-id="ccf50-434">It is basically \<namespace\>.\<classname\> in your code.</span></span>
   * <span data-ttu-id="ccf50-435">**PackageLinkedService** értéke túl**StorageLinkedService** toohello blob-tároló hello egyéni tevékenység zip fájlt tartalmazó mutat.</span><span class="sxs-lookup"><span data-stu-id="ccf50-435">**PackageLinkedService** is set too**StorageLinkedService** that points toohello blob storage that contains hello custom activity zip file.</span></span> <span data-ttu-id="ccf50-436">Ha különböző Azure Storage-fiókokat használ a bemeneti/kimeneti fájlok és hello egyéni tevékenység zip-fájl, akkor toocreate egy másik Azure Storage társított szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="ccf50-436">If you are using different Azure Storage accounts for input/output files and hello custom activity zip file, you have toocreate another Azure Storage linked service.</span></span> <span data-ttu-id="ccf50-437">Ez a cikk feltételezi, hogy a hello azonos Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="ccf50-437">This article assumes that you are using hello same Azure Storage account.</span></span>
   * <span data-ttu-id="ccf50-438">**PackageFile** értéke túl**customactivitycontainer/MyDotNetActivity.zip**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-438">**PackageFile** is set too**customactivitycontainer/MyDotNetActivity.zip**.</span></span> <span data-ttu-id="ccf50-439">A hello formátumban: \<containerforthezip\>/\<nameofthezip.zip\>.</span><span class="sxs-lookup"><span data-stu-id="ccf50-439">It is in hello format: \<containerforthezip\>/\<nameofthezip.zip\>.</span></span>
   * <span data-ttu-id="ccf50-440">hello egyéni tevékenység veszi **InputDataset** bemenetként és **OutputDataset** output típusúként.</span><span class="sxs-lookup"><span data-stu-id="ccf50-440">hello custom activity takes **InputDataset** as input and **OutputDataset** as output.</span></span>
   * <span data-ttu-id="ccf50-441">Hello **linkedServiceName** hello egyéni tevékenység tulajdonság mutat toohello **AzureBatchLinkedService**, amely közli az Azure Data Factory hello egyéni tevékenységre kell az Azure Batch toorun.</span><span class="sxs-lookup"><span data-stu-id="ccf50-441">hello **linkedServiceName** property of hello custom activity points toohello **AzureBatchLinkedService**, which tells Azure Data Factory that hello custom activity needs toorun on Azure Batch.</span></span>
   * <span data-ttu-id="ccf50-442">Hello **egyidejűségi** beállítás fontos.</span><span class="sxs-lookup"><span data-stu-id="ccf50-442">hello **concurrency** setting is important.</span></span> <span data-ttu-id="ccf50-443">Ha hello alapértelmezett érték, amely 1, még akkor is, ha 2 vagy több csomópontján hello Azure Batch-készlet számítási, hello szeletek feldolgozásának egymás után.</span><span class="sxs-lookup"><span data-stu-id="ccf50-443">If you use hello default value, which is 1, even if you have 2 or more compute nodes in hello Azure Batch pool, hello slices are processed one after another.</span></span> <span data-ttu-id="ccf50-444">Ezért nem készítésének Azure Batch hello párhuzamos feldolgozási képességének kihasználása.</span><span class="sxs-lookup"><span data-stu-id="ccf50-444">Therefore, you are not taking advantage of hello parallel processing capability of Azure Batch.</span></span> <span data-ttu-id="ccf50-445">Ha **egyidejűségi** tooa magasabb értéket, azaz 2, az azt jelenti, hogy a két szeletek (felel meg az Azure Batch tootwo feladatok) hello képes lehet feldolgozni azonos időben, ebben az esetben, mindkét hello virtuális gépeinek hello Azure Batch-készlet ki van használva.</span><span class="sxs-lookup"><span data-stu-id="ccf50-445">If you set **concurrency** tooa higher value, say 2, it means that two slices (corresponds tootwo tasks in Azure Batch) can be processed at hello same time, in which case, both hello VMs in hello Azure Batch pool are utilized.</span></span> <span data-ttu-id="ccf50-446">Ezért állítsa be megfelelően hello párhuzamossági tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="ccf50-446">Therefore, set hello concurrency property appropriately.</span></span>
   * <span data-ttu-id="ccf50-447">Alapértelmezés szerint egyetlen virtuális gép (szelet) csak egy feladat végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="ccf50-447">Only one task (slice) is executed on a VM at any point by default.</span></span> <span data-ttu-id="ccf50-448">hello oka, hogy alapértelmezés szerint hello **virtuális gépenként feladatok maximális** az Azure Batch-készlet too1 van beállítva.</span><span class="sxs-lookup"><span data-stu-id="ccf50-448">hello reason is that, by default, hello **Maximum tasks per VM** is set too1 for an Azure Batch pool.</span></span> <span data-ttu-id="ccf50-449">Előfeltételek részeként létrehozott készlet e tulajdonság beállítása too2, két adat-előállító szeletek futhat egy virtuális gép egyidejű hello a ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="ccf50-449">As part of prerequisites, you created a pool with this property set too2, so two Data Factory slices can be running on a VM at hello same time.</span></span>

    -   <span data-ttu-id="ccf50-450">**isPaused** tulajdonsága toofalse alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="ccf50-450">**isPaused** property is set toofalse by default.</span></span> <span data-ttu-id="ccf50-451">hello csővezeték azonnal futtatja ebben a példában, mert hello szeletek indítsa el a korábbi hello.</span><span class="sxs-lookup"><span data-stu-id="ccf50-451">hello pipeline runs immediately in this example because hello slices start in hello past.</span></span> <span data-ttu-id="ccf50-452">Ez a tulajdonság tootrue toopause hello folyamat beállítása, és állítsa be úgy a hátsó toofalse toorestart.</span><span class="sxs-lookup"><span data-stu-id="ccf50-452">You can set this property tootrue toopause hello pipeline and set it back toofalse toorestart.</span></span>

    -   <span data-ttu-id="ccf50-453">Hello **start** idő és **end** alkalommal öt órát egymástól, szeletek előállított hourly, így öt szeletek hello csővezeték hozzák létre.</span><span class="sxs-lookup"><span data-stu-id="ccf50-453">hello **start** time and **end** times are five hours apart and slices are produced hourly, so five slices are produced by hello pipeline.</span></span>

1. <span data-ttu-id="ccf50-454">Kattintson a **telepítés** a toodeploy hello csővezeték hello parancssávon.</span><span class="sxs-lookup"><span data-stu-id="ccf50-454">Click **Deploy** on hello command bar toodeploy hello pipeline.</span></span>

#### <a name="step-5-test-hello-pipeline"></a><span data-ttu-id="ccf50-455">5. lépés: Hello folyamat tesztelése</span><span class="sxs-lookup"><span data-stu-id="ccf50-455">Step 5: Test hello pipeline</span></span>
<span data-ttu-id="ccf50-456">Ebben a lépésben hello csővezeték hello bemeneti mappákba fájlok ejtésével tesztelése.</span><span class="sxs-lookup"><span data-stu-id="ccf50-456">In this step, you test hello pipeline by dropping files into hello input folders.</span></span> <span data-ttu-id="ccf50-457">Egy bemeneti mappa minden egy fájlba kezdjük tesztelési hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="ccf50-457">Let’s start with testing hello pipeline with one file per one input folder.</span></span>

1. <span data-ttu-id="ccf50-458">Hello a Data Factory panelen hello Azure-portálon, kattintson a **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-458">In hello Data Factory blade in hello Azure portal, click **Diagram**.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image10.png)
2. <span data-ttu-id="ccf50-459">Hello diagram nézetben kattintson duplán a bemeneti adatkészlet: **InputDataset**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-459">In hello diagram view, double-click input dataset: **InputDataset**.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image11.png)
3. <span data-ttu-id="ccf50-460">Megtekintheti az hello **InputDataset** mind az öt panelről szeletek készen áll.</span><span class="sxs-lookup"><span data-stu-id="ccf50-460">You should see hello **InputDataset** blade with all five slices ready.</span></span> <span data-ttu-id="ccf50-461">Értesítés hello **SZELET kezdete** és **SZELET BEFEJEZÉSÉNEK** az egyes szeletek.</span><span class="sxs-lookup"><span data-stu-id="ccf50-461">Notice hello **SLICE START TIME** and **SLICE END TIME** for each slice.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image12.png)
4. <span data-ttu-id="ccf50-462">A hello **diagramnézet**, ekkor kattintson a **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-462">In hello **Diagram View**, now click **OutputDataset**.</span></span>
5. <span data-ttu-id="ccf50-463">Meg kell jelennie, hogy a hello öt kimeneti szeletek hello kész állapotban van, ha azok már előállítani.</span><span class="sxs-lookup"><span data-stu-id="ccf50-463">You should see that hello five output slices are in hello Ready state if they have already been produced.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image13.png)
6. <span data-ttu-id="ccf50-464">Használja az Azure portál tooview hello **feladatok** hello társított **szeletek** , és ellenőrizze, milyen VM minden szelet futott.</span><span class="sxs-lookup"><span data-stu-id="ccf50-464">Use Azure portal tooview hello **tasks** associated with hello **slices** and see what VM each slice ran on.</span></span> <span data-ttu-id="ccf50-465">Lásd: [adat-előállító és kötegelt integrációs](#data-factory-and-batch-integration) című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="ccf50-465">See [Data Factory and Batch integration](#data-factory-and-batch-integration) section for details.</span></span>
7. <span data-ttu-id="ccf50-466">A kimeneti fájlok hello hello kell megjelennie `outputfolder` a `mycontainer` az Azure blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="ccf50-466">You should see hello output files in hello `outputfolder` of `mycontainer` in your Azure blob storage.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image15.png)

   <span data-ttu-id="ccf50-467">Meg kell jelennie egy, az egyes bemeneti szeletek öt kimeneti fájlokat.</span><span class="sxs-lookup"><span data-stu-id="ccf50-467">You should see five output files, one for each input slice.</span></span> <span data-ttu-id="ccf50-468">Egyes hello kimeneti fájl a következő kimeneti tartalom hasonló toohello kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="ccf50-468">Each of hello output file should have content similar toohello following output:</span></span>

    ```
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file.txt.
    ```
   <span data-ttu-id="ccf50-469">hello következő ábra bemutatja hogyan hello adat-előállító szeletek leképezése tootasks Azure kötegben.</span><span class="sxs-lookup"><span data-stu-id="ccf50-469">hello following diagram illustrates how hello Data Factory slices map tootasks in Azure Batch.</span></span> <span data-ttu-id="ccf50-470">Ebben a példában a szelet csak egy futtató rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ccf50-470">In this example, a slice has only one run.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image16.png)
8. <span data-ttu-id="ccf50-471">Most próbáljuk meg egy mappában több fájlokkal.</span><span class="sxs-lookup"><span data-stu-id="ccf50-471">Now, let’s try with multiple files in a folder.</span></span> <span data-ttu-id="ccf50-472">Fájlok létrehozása: **fájl2.ref fájllal**, **file3.txt**, **file4.txt**, és **file5.txt** hello az azonos tartalom hasonlóan file.txt hello mappában: **2015-11-06-01**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-472">Create files: **file2.txt**, **file3.txt**, **file4.txt**, and **file5.txt** with hello same content as in file.txt in hello folder: **2015-11-06-01**.</span></span>
9. <span data-ttu-id="ccf50-473">Hello kimeneti mappában **törlése** hello kimeneti fájl: **2015-11-16-01.txt**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-473">In hello output folder, **delete** hello output file: **2015-11-16-01.txt**.</span></span>
10. <span data-ttu-id="ccf50-474">Mostantól a hello **OutputDataset** panelen, kattintson a jobb gombbal hello szeletre **SZELET Kezdés időpontja** túl beállítása**11/16/2015 01:00:00-kor**, és kattintson a **futtassa**toorerun/újra-process hello szelet.</span><span class="sxs-lookup"><span data-stu-id="ccf50-474">Now, in hello **OutputDataset** blade, right-click hello slice with **SLICE START TIME** set too**11/16/2015 01:00:00 AM**, and click **Run** toorerun/re-process hello slice.</span></span> <span data-ttu-id="ccf50-475">Most hello szelet öt fájlokat tartalmaz egy fájl helyett.</span><span class="sxs-lookup"><span data-stu-id="ccf50-475">Now, hello slice has five files instead of one file.</span></span>

    ![](./media/data-factory-data-processing-using-batch/image17.png)
11. <span data-ttu-id="ccf50-476">Után hello szelet fut, és annak állapotát **készen áll a**, ellenőrizze a szeletre vonatkozó hello kimeneti fájlban hello tartalmat (**2015-11-16-01.txt**) a hello `outputfolder` a `mycontainer` a blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="ccf50-476">After hello slice runs and its status is **Ready**, verify hello content in hello output file for this slice (**2015-11-16-01.txt**) in hello `outputfolder` of `mycontainer` in your blob storage.</span></span> <span data-ttu-id="ccf50-477">Egy sort a fájl hello szelet kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ccf50-477">There should be a line for each file of hello slice.</span></span>

    ```
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file.txt.
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file2.txt.
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file3.txt.
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file4.txt.
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file5.txt.
    ```

> [!NOTE]
> <span data-ttu-id="ccf50-478">Ha, nem törölte a hello kimeneti fájl 2015-11-16-01.txt előtt öt bemeneti fájlokkal, látni egy sor hello előző szelet futtassa a és hello aktuális szelet futtassa az öt sorban.</span><span class="sxs-lookup"><span data-stu-id="ccf50-478">If you did not delete hello output file 2015-11-16-01.txt before trying with five input files, you see one line from hello previous slice run and five lines from hello current slice run.</span></span> <span data-ttu-id="ccf50-479">Alapértelmezés szerint hello tartalom hozzáfűzött toooutput fájlt is, ha már létezik.</span><span class="sxs-lookup"><span data-stu-id="ccf50-479">By default, hello content is appended toooutput file if it already exists.</span></span>
>
>

#### <a name="data-factory-and-batch-integration"></a><span data-ttu-id="ccf50-480">Adat-előállító és kötegelt integrációja</span><span class="sxs-lookup"><span data-stu-id="ccf50-480">Data Factory and Batch integration</span></span>
<span data-ttu-id="ccf50-481">hello Data Factory szolgáltatásnak hoz létre egy feladatot az Azure Batch névvel hello: `adf-poolname:job-xxx`.</span><span class="sxs-lookup"><span data-stu-id="ccf50-481">hello Data Factory service creates a job in Azure Batch with hello name: `adf-poolname:job-xxx`.</span></span>

![Az Azure Data Factory - kötegelt feladatok](media/data-factory-data-processing-using-batch/data-factory-batch-jobs.png)

<span data-ttu-id="ccf50-483">Egy feladat hello feladat minden egyes tevékenység futtatásához a szelet jön létre.</span><span class="sxs-lookup"><span data-stu-id="ccf50-483">A task in hello job is created for each activity run of a slice.</span></span> <span data-ttu-id="ccf50-484">Ha 10 szeletek készen toobe feldolgozott, 10 feladatok hello feladat jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="ccf50-484">If there are 10 slices ready toobe processed, 10 tasks are created in hello job.</span></span> <span data-ttu-id="ccf50-485">A párhuzamosan futó, ha több számítási csomópont hello készletben egynél több szelet lehet.</span><span class="sxs-lookup"><span data-stu-id="ccf50-485">You can have more than one slice running in parallel if you have multiple compute nodes in hello pool.</span></span> <span data-ttu-id="ccf50-486">Ha hello maximális tevékenységek maximális száma számítási csomópont értéke túl > 1 is lehet több mint egy szelet hello futó azonos számítási.</span><span class="sxs-lookup"><span data-stu-id="ccf50-486">If hello maximum tasks per compute node is set too> 1, there can be more than one slice running on hello same compute.</span></span>

<span data-ttu-id="ccf50-487">Ebben a példában nincsenek öt szeletek, így öt feladatok Azure kötegben.</span><span class="sxs-lookup"><span data-stu-id="ccf50-487">In this example, there are five slices, so five tasks in Azure Batch.</span></span> <span data-ttu-id="ccf50-488">A hello **egyidejűségi** túl beállítása**5** hello a csővezeték-JSON-t Azure Data Factory és **virtuális gépenként feladatok maximális** túl beállítása**2** az Azure Batch tárolókészlet **2** virtuális gépeket, hello feladatok futtatása gyors (Ellenőrizze a kezdő és záró időpontjának feladatok).</span><span class="sxs-lookup"><span data-stu-id="ccf50-488">With hello **concurrency** set too**5** in hello pipeline JSON in Azure Data Factory and **Maximum tasks per VM** set too**2** in Azure Batch pool with **2** VMs, hello tasks runs fast (check start and end times for tasks).</span></span>

<span data-ttu-id="ccf50-489">Hello portál tooview hello kötegelt és a társított hello feladatainak **szeletek** , és ellenőrizze, milyen VM minden szelet futott.</span><span class="sxs-lookup"><span data-stu-id="ccf50-489">Use hello portal tooview hello Batch job and its tasks that are associated with hello **slices** and see what VM each slice ran on.</span></span>

![Az Azure Data Factory - kötegelt munka feladatai](media/data-factory-data-processing-using-batch/data-factory-batch-job-tasks.png)

### <a name="debug-hello-pipeline"></a><span data-ttu-id="ccf50-491">Hello folyamat hibakeresése</span><span class="sxs-lookup"><span data-stu-id="ccf50-491">Debug hello pipeline</span></span>
<span data-ttu-id="ccf50-492">A hibakeresés néhány alapvető technikából áll:</span><span class="sxs-lookup"><span data-stu-id="ccf50-492">Debugging consists of a few basic techniques:</span></span>

1. <span data-ttu-id="ccf50-493">Ha nincs beállítva túl hello bemeneti szelet**készen**, győződjön meg arról, hogy hello bemeneti mappaszerkezet helyes-e, valamint file.txt hello bemeneti mappáiban.</span><span class="sxs-lookup"><span data-stu-id="ccf50-493">If hello input slice is not set too**Ready**, confirm that hello input folder structure is correct and file.txt exists in hello input folders.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image3.png)
2. <span data-ttu-id="ccf50-494">A hello **Execute** az egyéni tevékenység használja hello metódusában **IActivityLogger** toolog objektuminformáció, amely segít a problémák elhárításához.</span><span class="sxs-lookup"><span data-stu-id="ccf50-494">In hello **Execute** method of your custom activity, use hello **IActivityLogger** object toolog information that helps you troubleshoot issues.</span></span> <span data-ttu-id="ccf50-495">hello felhasználói megjelennek a naplóba köszönőüzenetei\_0. naplófájlt.</span><span class="sxs-lookup"><span data-stu-id="ccf50-495">hello logged messages show up in hello user\_0.log file.</span></span>

   <span data-ttu-id="ccf50-496">A hello **OutputDataset** panelen hello szelet toosee hello kattintson **ADATSZELET** , hogy a szelet paneljét.</span><span class="sxs-lookup"><span data-stu-id="ccf50-496">In hello **OutputDataset** blade, click hello slice toosee hello **DATA SLICE** blade for that slice.</span></span> <span data-ttu-id="ccf50-497">Látni **tevékenységek** az adott szeletek.</span><span class="sxs-lookup"><span data-stu-id="ccf50-497">You see **activity runs** for that slice.</span></span> <span data-ttu-id="ccf50-498">Futtassa a hello szelet tevékenységenként kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="ccf50-498">You should see one activity run for hello slice.</span></span> <span data-ttu-id="ccf50-499">Ha **futtatása** hello parancssávon, akkor kezdhet hello futtassa egy másik tevékenysége azonos szelet.</span><span class="sxs-lookup"><span data-stu-id="ccf50-499">If you click **Run** in hello command bar, you can start another activity run for hello same slice.</span></span>

   <span data-ttu-id="ccf50-500">Ha hello tevékenységfuttatási gombra kattint, megjelenik az hello **tevékenység futtatása részletei** naplófájlok listáját tartalmazó panel.</span><span class="sxs-lookup"><span data-stu-id="ccf50-500">When you click hello activity run, you see hello **ACTIVITY RUN DETAILS** blade with a list of log files.</span></span> <span data-ttu-id="ccf50-501">Hello a naplózott üzeneteket látja **felhasználói\_0. napló** fájlt.</span><span class="sxs-lookup"><span data-stu-id="ccf50-501">You see logged messages in hello **user\_0.log** file.</span></span> <span data-ttu-id="ccf50-502">Ha hiba történik, megjelenik az három tevékenység fut mert hello újrapróbálkozások száma too3 hello csővezeték/tevékenységben JSON.</span><span class="sxs-lookup"><span data-stu-id="ccf50-502">When an error occurs, you see three activity runs because hello retry count is set too3 in hello pipeline/activity JSON.</span></span> <span data-ttu-id="ccf50-503">Ha hello tevékenységfuttatási gombra kattint, megjelenik az hello naplófájlokat, hogy áttekintheti tootroubleshoot hello hiba.</span><span class="sxs-lookup"><span data-stu-id="ccf50-503">When you click hello activity run, you see hello log files that you can review tootroubleshoot hello error.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image18.png)

   <span data-ttu-id="ccf50-504">A naplófájlok hello listájában kattintson a hello **felhasználói-0.log**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-504">In hello list of log files, click hello **user-0.log**.</span></span> <span data-ttu-id="ccf50-505">Jobb oldali panelen hello vannak hello segítségével hello eredményeit **IActivityLogger.Write** metódust.</span><span class="sxs-lookup"><span data-stu-id="ccf50-505">In hello right panel are hello results of using hello **IActivityLogger.Write** method.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image19.png)

   <span data-ttu-id="ccf50-506">Ellenőrizze a rendszer-0.log rendszer hibaüzeneteket és kivételeket.</span><span class="sxs-lookup"><span data-stu-id="ccf50-506">Check system-0.log for any system error messages and exceptions.</span></span>

    ```
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Loading assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Creating an instance of MyDotNetActivityNS.MyDotNetActivity from assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Executing Module
    
    Trace\_T\_D\_12/6/2015 1:43:38 AM\_T\_D\_\_T\_D\_Information\_T\_D\_0\_T\_D\_Activity e3817da0-d843-4c5c-85c6-40ba7424dce2 finished successfully
    ```
3. <span data-ttu-id="ccf50-507">Hello tartalmaznak **PDB** hello zip-fájlban szereplő fájlt, hogy a hello hiba legutolsó részletes adatai információ például **hívási verem** Ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="ccf50-507">Include hello **PDB** file in hello zip file so that hello error details have information such as **call stack** when an error occurs.</span></span>
4. <span data-ttu-id="ccf50-508">Az egyéni tevékenység hello el kell érnie hello fájlok hello zip-fájlban szereplő összes hello **felső szintű** egyetlen almappát.</span><span class="sxs-lookup"><span data-stu-id="ccf50-508">All hello files in hello zip file for hello custom activity must be at hello **top level** with no subfolders.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image20.png)
5. <span data-ttu-id="ccf50-509">Győződjön meg arról, hogy hello **assemblyName** (MyDotNetActivity.dll) **entryPoint** (MyDotNetActivityNS.MyDotNetActivity) **packageFile** (customactivitycontainer / MyDotNetActivity.zip), és **packageLinkedService** (kell pont toohello Azure blob-tároló, amely tartalmazza a hello zip-fájl) toocorrect értékek vannak beállítva.</span><span class="sxs-lookup"><span data-stu-id="ccf50-509">Ensure that hello **assemblyName** (MyDotNetActivity.dll), **entryPoint** (MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer/MyDotNetActivity.zip), and **packageLinkedService** (should point toohello Azure blob storage that contains hello zip file) are set toocorrect values.</span></span>
6. <span data-ttu-id="ccf50-510">Ha rögzített egy hiba és a kívánt tooreprocess hello szelet, kattintson a jobb gombbal a hello hello szelet **OutputDataset** panel megnyitásához, és kattintson **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-510">If you fixed an error and want tooreprocess hello slice, right-click hello slice in hello **OutputDataset** blade and click **Run**.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image21.png)

   > [!NOTE]
   > <span data-ttu-id="ccf50-511">Megjelenik egy **tároló** az Azure Blob Storage nevű: `adfjobs`.</span><span class="sxs-lookup"><span data-stu-id="ccf50-511">You see a **container** in your Azure Blob storage named: `adfjobs`.</span></span> <span data-ttu-id="ccf50-512">Ez a tároló nem törli automatikusan a rendszer, de biztonságos törlése után végzett tesztelés hello megoldás.</span><span class="sxs-lookup"><span data-stu-id="ccf50-512">This container is not automatically deleted, but you can safely delete it after you are done testing hello solution.</span></span> <span data-ttu-id="ccf50-513">Ehhez hasonlóan hello adat-előállító megoldást hoz létre egy Azure Batch **feladat** nevű: `adf-\<pool ID/name\>:job-0000000001`.</span><span class="sxs-lookup"><span data-stu-id="ccf50-513">Similarly, hello Data Factory solution creates an Azure Batch **job** named: `adf-\<pool ID/name\>:job-0000000001`.</span></span> <span data-ttu-id="ccf50-514">Ez a feladat hello megoldás Ha szeretné tesztelése után törölheti.</span><span class="sxs-lookup"><span data-stu-id="ccf50-514">You can delete this job after you test hello solution if you like.</span></span>
   >
   >
7. <span data-ttu-id="ccf50-515">hello egyéni tevékenység nem használja a hello **app.config** fájlt a csomagból.</span><span class="sxs-lookup"><span data-stu-id="ccf50-515">hello custom activity does not use hello **app.config** file from your package.</span></span> <span data-ttu-id="ccf50-516">Ezért ha a kód a kapcsolati karakterláncok hello konfigurációs fájlból olvassa be, nem működik futásidőben.</span><span class="sxs-lookup"><span data-stu-id="ccf50-516">Therefore, if your code reads any connection strings from hello configuration file, it does not work at runtime.</span></span> <span data-ttu-id="ccf50-517">hello célszerű esetén használja az Azure Batch toohold bármely titkos kulcsainak egy **Azure KeyVault**használni egy tanúsítvány alapú szolgáltatás egyszerű tooprotect hello keyvault és terjesztése hello tanúsítvány tooAzure Batch-készlet.</span><span class="sxs-lookup"><span data-stu-id="ccf50-517">hello best practice when using Azure Batch is toohold any secrets in an **Azure KeyVault**, use a certificate-based service principal tooprotect hello keyvault, and distribute hello certificate tooAzure Batch pool.</span></span> <span data-ttu-id="ccf50-518">hello .NET egyéni tevékenység majd el titkok hello KeyVault futásidőben.</span><span class="sxs-lookup"><span data-stu-id="ccf50-518">hello .NET custom activity then can access secrets from hello KeyVault at runtime.</span></span> <span data-ttu-id="ccf50-519">Ez a megoldás általános egy és titkos kulcsot, nem csak a kapcsolati karakterlánc típusú tooany méretezhető.</span><span class="sxs-lookup"><span data-stu-id="ccf50-519">This solution is a generic one and can scale tooany type of secret, not just connection string.</span></span>

    <span data-ttu-id="ccf50-520">Van egy egyszerűbb megoldást (de nem ajánlott): hozhat létre egy **Azure SQL társított szolgáltatásnak** rendelkező kapcsolatikarakterlánc-beállításokat, hozzon létre, hogy használ hello társított szolgáltatás, és típusú bemeneti adatkészletként lánc hello dataset adatkészlet Egyéni .NET tevékenység toohello.</span><span class="sxs-lookup"><span data-stu-id="ccf50-520">There is an easier workaround (but not a best practice): you can create an **Azure SQL linked service** with connection string settings, create a dataset that uses hello linked service, and chain hello dataset as a dummy input dataset toohello custom .NET activity.</span></span> <span data-ttu-id="ccf50-521">Ezután a hozzáférés hello kapcsolódó szolgáltatás kapcsolati karakterláncot a hello egyéni tevékenység kódja és konfigurálva kell működnie futásidőben.</span><span class="sxs-lookup"><span data-stu-id="ccf50-521">You can then access hello linked service's connection string in hello custom activity code and it should work fine at runtime.</span></span>  

#### <a name="extend-hello-sample"></a><span data-ttu-id="ccf50-522">Hello minta kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="ccf50-522">Extend hello sample</span></span>
<span data-ttu-id="ccf50-523">Kiterjesztheti a minta toolearn további információk az Azure Data Factory és az Azure Batch-szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="ccf50-523">You can extend this sample toolearn more about Azure Data Factory and Azure Batch features.</span></span> <span data-ttu-id="ccf50-524">Például egy eltérő időtartományt tooprocess szeletek hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="ccf50-524">For example, tooprocess slices in a different time range, do hello following steps:</span></span>

1. <span data-ttu-id="ccf50-525">Adja hozzá a következő hello almappát hello `inputfolder`: 2015-11-16-05, 2015-11-16-06 201-11-16-07, 2011-11-16-08, 2015-11-16-09 és helyet adjon meg a mappákban lévő fájlok.</span><span class="sxs-lookup"><span data-stu-id="ccf50-525">Add hello following subfolders in hello `inputfolder`: 2015-11-16-05, 2015-11-16-06, 201-11-16-07, 2011-11-16-08, 2015-11-16-09 and place input files in those folders.</span></span> <span data-ttu-id="ccf50-526">Módosítsa a hello folyamat befejezésének hello `2015-11-16T05:00:00Z` túl`2015-11-16T10:00:00Z`.</span><span class="sxs-lookup"><span data-stu-id="ccf50-526">Change hello end time for hello pipeline from `2015-11-16T05:00:00Z` too`2015-11-16T10:00:00Z`.</span></span> <span data-ttu-id="ccf50-527">A hello **diagramnézet**, kattintson duplán a hello **InputDataset**, és győződjön meg arról, hogy készen áll-e hello bemeneti szeletek.</span><span class="sxs-lookup"><span data-stu-id="ccf50-527">In hello **Diagram View**, double-click hello **InputDataset**, and confirm that hello input slices are ready.</span></span> <span data-ttu-id="ccf50-528">Kattintson duplán a **OuptutDataset** kimeneti szeletek toosee hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="ccf50-528">Double-click **OuptutDataset** toosee hello state of output slices.</span></span> <span data-ttu-id="ccf50-529">Ha készen áll a állapotban vannak, ellenőrizze a hello kimeneti mappa hello kimeneti fájlok.</span><span class="sxs-lookup"><span data-stu-id="ccf50-529">If they are in Ready state, check hello output folder for hello output files.</span></span>
2. <span data-ttu-id="ccf50-530">Növeli vagy csökkenti a hello **egyidejűségi** beállítás toounderstand hogyan azt befolyásolja, hogy a megoldás hello teljesítményét különösen hello feldolgozása, amely az Azure Batch következik be.</span><span class="sxs-lookup"><span data-stu-id="ccf50-530">Increase or decrease hello **concurrency** setting toounderstand how it affects hello performance of your solution, especially hello processing that occurs on Azure Batch.</span></span> <span data-ttu-id="ccf50-531">(Lásd a 4. lépés: hozzon létre és futtasson hello csővezeték további hello **egyidejűségi** beállítás.)</span><span class="sxs-lookup"><span data-stu-id="ccf50-531">(See Step 4: Create and run hello pipeline for more on hello **concurrency** setting.)</span></span>
3. <span data-ttu-id="ccf50-532">Hozzon létre egy alkalmazáskészlet magasabb/alacsonyabb **virtuális gépenként feladatok maximális**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-532">Create a pool with higher/lower **Maximum tasks per VM**.</span></span> <span data-ttu-id="ccf50-533">toouse hello új készletet létrehozta, frissítés hello csatolt Azure Batch szolgáltatás hello Data Factory-megoldásban.</span><span class="sxs-lookup"><span data-stu-id="ccf50-533">toouse hello new pool you created, update hello Azure Batch linked service in hello Data Factory solution.</span></span> <span data-ttu-id="ccf50-534">(Lásd a 4. lépés: hozzon létre és futtasson hello csővezeték további hello **virtuális gépenként feladatok maximális** beállítás.)</span><span class="sxs-lookup"><span data-stu-id="ccf50-534">(See Step 4: Create and run hello pipeline for more on hello **Maximum tasks per VM** setting.)</span></span>
4. <span data-ttu-id="ccf50-535">Az Azure Batch-készlet létrehozása **automatikus skálázás** szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="ccf50-535">Create an Azure Batch pool with **autoscale** feature.</span></span> <span data-ttu-id="ccf50-536">Hello dinamikusan állítható lesz az feldolgozási kapacitása révén az alkalmazás által használt Azure Batch-készlet számítási csomópontjainak automatikus skálázás.</span><span class="sxs-lookup"><span data-stu-id="ccf50-536">Automatically scaling compute nodes in an Azure Batch pool is hello dynamic adjustment of processing power used by your application.</span></span> 

    <span data-ttu-id="ccf50-537">Itt hello minta képlet éri el a következő viselkedés hello: hello készlet létrehozásakor 1 virtuális gép kezdődik.</span><span class="sxs-lookup"><span data-stu-id="ccf50-537">hello sample formula here achieves hello following behavior: When hello pool is initially created, it starts with 1 VM.</span></span> <span data-ttu-id="ccf50-538">$PendingTasks metrika meghatározza hello feladatok futtatásának + (aszinkron) aktív állapotban.</span><span class="sxs-lookup"><span data-stu-id="ccf50-538">$PendingTasks metric defines hello number of tasks in running + active (queued) state.</span></span>  <span data-ttu-id="ccf50-539">hello képlet hello átlagos száma függőben lévő feladatok keresi hello az elmúlt 180 másodperc alatt, és ennek megfelelően állítja be TargetDedicated.</span><span class="sxs-lookup"><span data-stu-id="ccf50-539">hello formula finds hello average number of pending tasks in hello last 180 seconds and sets TargetDedicated accordingly.</span></span> <span data-ttu-id="ccf50-540">Biztosítja, hogy TargetDedicated soha nem túllép 25 virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="ccf50-540">It ensures that TargetDedicated never goes beyond 25 VMs.</span></span> <span data-ttu-id="ccf50-541">Igen új feladatok nyújtják, készlet automatikusan növekszik és feladatok befejezését, mint a virtuális gépek válik a szabad egyenként és hello automatikus skálázás zsugorítja a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="ccf50-541">So, as new tasks are submitted, pool automatically grows and as tasks complete, VMs become free one by one and hello autoscaling shrinks those VMs.</span></span> <span data-ttu-id="ccf50-542">módosított tooyour igényei lehetnek startingNumberOfVMs és maxNumberofVMs.</span><span class="sxs-lookup"><span data-stu-id="ccf50-542">startingNumberOfVMs and maxNumberofVMs can be adjusted tooyour needs.</span></span>
 
    <span data-ttu-id="ccf50-543">Automatikus skálázás képlet:</span><span class="sxs-lookup"><span data-stu-id="ccf50-543">Autoscale formula:</span></span>

    ``` 
    startingNumberOfVMs = 1;
    maxNumberofVMs = 25;
    pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
    pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
    $TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
    ```

   <span data-ttu-id="ccf50-544">Lásd: [automatikus méretezési számítási csomópontok az Azure Batch-készlet](../batch/batch-automatic-scaling.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="ccf50-544">See [Automatically scale compute nodes in an Azure Batch pool](../batch/batch-automatic-scaling.md) for details.</span></span>

   <span data-ttu-id="ccf50-545">Ha hello készlet hello alapértelmezett [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), hello Batch szolgáltatás 15-30 perces tooprepare hello VM is beletelhet hello egyéni tevékenység futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="ccf50-545">If hello pool is using hello default [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), hello Batch service could take 15-30 minutes tooprepare hello VM before running hello custom activity.</span></span>  <span data-ttu-id="ccf50-546">Ha hello készletet használ egy másik autoScaleEvaluationInterval, hello Batch szolgáltatás autoScaleEvaluationInterval + 10 percet is beletelhet.</span><span class="sxs-lookup"><span data-stu-id="ccf50-546">If hello pool is using a different autoScaleEvaluationInterval, hello Batch service could take autoScaleEvaluationInterval + 10 minutes.</span></span>
5. <span data-ttu-id="ccf50-547">A hello minta megoldás hello **Execute** metódus meghívja hello **Calculate** módszer, amely egy bemeneti adatok szelet tooproduce egy kimeneti adatszelet dolgozza fel.</span><span class="sxs-lookup"><span data-stu-id="ccf50-547">In hello sample solution, hello **Execute** method invokes hello **Calculate** method that processes an input data slice tooproduce an output data slice.</span></span> <span data-ttu-id="ccf50-548">A saját módszer tooprocess bemeneti adatai, és cserélje le hello Calculate metódus hívása az Execute metódus hello tooyour telefonhívás írhat.</span><span class="sxs-lookup"><span data-stu-id="ccf50-548">You can write your own method tooprocess input data and replace hello Calculate method call in hello Execute method with a call tooyour method.</span></span>

### <a name="next-steps-consume-hello-data"></a><span data-ttu-id="ccf50-549">További lépések: hello adatokat</span><span class="sxs-lookup"><span data-stu-id="ccf50-549">Next steps: Consume hello data</span></span>
<span data-ttu-id="ccf50-550">Adatok feldolgozása után felhasználhat az online eszközökkel, például a **Microsoft Power BI**.</span><span class="sxs-lookup"><span data-stu-id="ccf50-550">After you process data, you can consume it with online tools like **Microsoft Power BI**.</span></span> <span data-ttu-id="ccf50-551">Az alábbiakban a hivatkozások toohelp megismerte a Power bi-ban, és hogyan toouse azt az Azure-ban:</span><span class="sxs-lookup"><span data-stu-id="ccf50-551">Here are links toohelp you understand Power BI and how toouse it in Azure:</span></span>

* [<span data-ttu-id="ccf50-552">A Power BI dataset felfedezés</span><span class="sxs-lookup"><span data-stu-id="ccf50-552">Explore a dataset in Power BI</span></span>](https://powerbi.microsoft.com/documentation/powerbi-service-get-data/)
* [<span data-ttu-id="ccf50-553">Bevezetés a hello Power BI Desktop használatába</span><span class="sxs-lookup"><span data-stu-id="ccf50-553">Getting started with hello Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/)
* [<span data-ttu-id="ccf50-554">Adatok frissítése a Power bi-ban</span><span class="sxs-lookup"><span data-stu-id="ccf50-554">Refresh data in Power BI</span></span>](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/)
* [<span data-ttu-id="ccf50-555">Azure és a Power BI – alapvető – áttekintés</span><span class="sxs-lookup"><span data-stu-id="ccf50-555">Azure and Power BI - basic overview</span></span>](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)

## <a name="references"></a><span data-ttu-id="ccf50-556">Referencia</span><span class="sxs-lookup"><span data-stu-id="ccf50-556">References</span></span>
* [<span data-ttu-id="ccf50-557">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="ccf50-557">Azure Data Factory</span></span>](https://azure.microsoft.com/documentation/services/data-factory/)

  * [<span data-ttu-id="ccf50-558">Bevezetés tooAzure Data Factory szolgáltatásnak</span><span class="sxs-lookup"><span data-stu-id="ccf50-558">Introduction tooAzure Data Factory service</span></span>](data-factory-introduction.md)
  * [<span data-ttu-id="ccf50-559">Ismerkedés az Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="ccf50-559">Get started with Azure Data Factory</span></span>](data-factory-build-your-first-pipeline.md)
  * [<span data-ttu-id="ccf50-560">Egyéni tevékenységek használata Azure Data Factory-folyamatban</span><span class="sxs-lookup"><span data-stu-id="ccf50-560">Use custom activities in an Azure Data Factory pipeline</span></span>](data-factory-use-custom-activities.md)
* [<span data-ttu-id="ccf50-561">Az Azure Batch</span><span class="sxs-lookup"><span data-stu-id="ccf50-561">Azure Batch</span></span>](https://azure.microsoft.com/documentation/services/batch/)

  * [<span data-ttu-id="ccf50-562">Az Azure Batch alapjai</span><span class="sxs-lookup"><span data-stu-id="ccf50-562">Basics of Azure Batch</span></span>](../batch/batch-technical-overview.md)
  * [<span data-ttu-id="ccf50-563">Azure Batch funkcióinak áttekintése</span><span class="sxs-lookup"><span data-stu-id="ccf50-563">Overview of Azure Batch features</span></span>](../batch/batch-api-basics.md)
  * [<span data-ttu-id="ccf50-564">Létrehozása és kezelése az Azure Batch-fiók hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="ccf50-564">Create and manage Azure Batch account in hello Azure portal</span></span>](../batch/batch-account-create-portal.md)
  * [<span data-ttu-id="ccf50-565">Ismerkedés az Azure Batch Library .NET</span><span class="sxs-lookup"><span data-stu-id="ccf50-565">Get started with Azure Batch Library .NET</span></span>](../batch/batch-dotnet-get-started.md)

[batch-explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch-explorer-walkthrough]: http://blogs.technet.com/b/windowshpc/archive/2015/01/20/azure-batch-explorer-sample-walkthrough.aspx
