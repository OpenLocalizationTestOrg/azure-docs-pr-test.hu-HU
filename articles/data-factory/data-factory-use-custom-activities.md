---
title: "Egyéni tevékenységek használata Azure Data Factory-folyamatban"
description: "Megtudhatja, hogyan hozzon létre egyéni tevékenységeket, és használja őket az Azure Data Factory-folyamathoz."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 8dd7ba14-15d2-4fd9-9ada-0b2c684327e9
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: spelluru
ms.openlocfilehash: f3d265f31cb653d32076747e586383d67bbccc41
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-custom-activities-in-an-azure-data-factory-pipeline"></a><span data-ttu-id="82742-103">Egyéni tevékenységek használata Azure Data Factory-folyamatban</span><span class="sxs-lookup"><span data-stu-id="82742-103">Use custom activities in an Azure Data Factory pipeline</span></span>

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="82742-104">Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="82742-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="82742-105">A Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="82742-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="82742-106">MapReduce művelethez</span><span class="sxs-lookup"><span data-stu-id="82742-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="82742-107">Hadoop Streamelési tevékenységben</span><span class="sxs-lookup"><span data-stu-id="82742-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="82742-108">A Spark-tevékenység</span><span class="sxs-lookup"><span data-stu-id="82742-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="82742-109">Machine Learning kötegelt végrehajtási tevékenység</span><span class="sxs-lookup"><span data-stu-id="82742-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="82742-110">Machine Learning Update-erőforrástevékenység</span><span class="sxs-lookup"><span data-stu-id="82742-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="82742-111">Tárolt eljárási tevékenység</span><span class="sxs-lookup"><span data-stu-id="82742-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="82742-112">Data Lake Analytics U-SQL-tevékenység</span><span class="sxs-lookup"><span data-stu-id="82742-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="82742-113">.NET egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="82742-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="82742-114">Egy Azure Data Factory-folyamathoz használható tevékenységeknek két típusa van.</span><span class="sxs-lookup"><span data-stu-id="82742-114">There are two types of activities that you can use in an Azure Data Factory pipeline.</span></span>

- <span data-ttu-id="82742-115">[Adatok mozgása tevékenységek](data-factory-data-movement-activities.md) közötti áthelyezése [támogatott forrás és a fogadó adattárolókhoz](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="82742-115">[Data Movement Activities](data-factory-data-movement-activities.md) to move data between [supported source and sink data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span>
- <span data-ttu-id="82742-116">[Adatok átalakítása tevékenységek](data-factory-data-transformation-activities.md) adatok átalakítására a számítási szolgáltatásokat, például Azure HDInsight, az Azure Batch és az Azure Machine Learning segítségével.</span><span class="sxs-lookup"><span data-stu-id="82742-116">[Data Transformation Activities](data-factory-data-transformation-activities.md) to transform data using compute services such as Azure HDInsight, Azure Batch, and Azure Machine Learning.</span></span> 

<span data-ttu-id="82742-117">Adatok áthelyezése az adat-előállító nem támogatja az adattárat, hozzon létre egy **egyéni tevékenység** a saját adatok mozgása logika és használja a tevékenység egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="82742-117">To move data to/from a data store that Data Factory does not support, create a **custom activity** with your own data movement logic and use the activity in a pipeline.</span></span> <span data-ttu-id="82742-118">Hasonlóképpen átalakító/folyamat számára adatokat úgy, hogy a Data Factory nem támogatja, egyéni tevékenység létrehozása a saját adatok átalakítása logikával, és használja a tevékenységet egy folyamaton belül.</span><span class="sxs-lookup"><span data-stu-id="82742-118">Similarly, to transform/process data in a way that isn't supported by Data Factory, create a custom activity with your own data transformation logic and use the activity in a pipeline.</span></span> 

<span data-ttu-id="82742-119">Konfigurálhat egy egyéni tevékenység segítségével futtatja egy **Azure Batch** készlete virtuális gépek vagy a Windows-alapú **Azure HDInsight** fürt.</span><span class="sxs-lookup"><span data-stu-id="82742-119">You can configure a custom activity to run on an **Azure Batch** pool of virtual machines or a Windows-based **Azure HDInsight** cluster.</span></span> <span data-ttu-id="82742-120">Azure Batch használatakor csak egy meglévő Azure Batch-készlet is használhatja.</span><span class="sxs-lookup"><span data-stu-id="82742-120">When using Azure Batch, you can use only an existing Azure Batch pool.</span></span> <span data-ttu-id="82742-121">Mivel a HDInsight használata esetén használható egy meglévő HDInsight-fürtre vagy olyan fürt, amely automatikusan jön létre, igény szerinti futásidőben.</span><span class="sxs-lookup"><span data-stu-id="82742-121">Whereas, when using HDInsight, you can use an existing HDInsight cluster or a cluster that is automatically created for you on-demand at runtime.</span></span>  

<span data-ttu-id="82742-122">A következő forgatókönyv részletesen bemutatja egy egyéni .NET tevékenység létrehozásáért és az egyéni tevékenység egy folyamaton belül.</span><span class="sxs-lookup"><span data-stu-id="82742-122">The following walkthrough provides step-by-step instructions for creating a custom .NET activity and using the custom activity in a pipeline.</span></span> <span data-ttu-id="82742-123">A forgatókönyv egy **Azure Batch** társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="82742-123">The walkthrough uses an **Azure Batch** linked service.</span></span> <span data-ttu-id="82742-124">Egy Azure HDInsight használata társított szolgáltatás helyette, típusú társított szolgáltatás létrehozása **HDInsight** (egyéni HDInsight-fürt) vagy **HDInsightOnDemand** (Data Factory létrehoz egy HDInsight-fürt igény szerinti).</span><span class="sxs-lookup"><span data-stu-id="82742-124">To use an Azure HDInsight linked service instead, you create a linked service of type **HDInsight** (your own HDInsight cluster) or **HDInsightOnDemand** (Data Factory creates an HDInsight cluster on-demand).</span></span> <span data-ttu-id="82742-125">Ezt követően konfigurálja a kapcsolódó HDInsight szolgáltatásokat egyéni tevékenység.</span><span class="sxs-lookup"><span data-stu-id="82742-125">Then, configure custom activity to use the HDInsight linked service.</span></span> <span data-ttu-id="82742-126">Lásd: [használata Azure HDInsight összekapcsolt szolgáltatások](#use-hdinsight-compute-service) című szakaszban talál információt az egyéni tevékenység futtatásához Azure HDInsight eszközzel.</span><span class="sxs-lookup"><span data-stu-id="82742-126">See [Use Azure HDInsight linked services](#use-hdinsight-compute-service) section for details on using Azure HDInsight to run the custom activity.</span></span>

> [!IMPORTANT]
> - <span data-ttu-id="82742-127">Az egyéni .NET-tevékenységek futtatásához csak a Windows-alapú HDInsight-fürtökön.</span><span class="sxs-lookup"><span data-stu-id="82742-127">The custom .NET activities run only on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="82742-128">Ez a korlátozás a megoldás, hogy a térkép csökkentése tevékenység használja egyéni Java-kódot futtathatnak egy Linux-alapú HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="82742-128">A workaround for this limitation is to use the Map Reduce Activity to run custom Java code on a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="82742-129">Egy másik lehetőség, hogy a virtuális gépek Azure Batch-készlet használja egy HDInsight-fürt használata helyett egyéni tevékenységek futtatásához.</span><span class="sxs-lookup"><span data-stu-id="82742-129">Another option is to use an Azure Batch pool of VMs to run custom activities instead of using a HDInsight cluster.</span></span>
> - <span data-ttu-id="82742-130">Nincs lehetőség egyéni tevékenység az adatkezelési átjárót a helyszíni adatforrások eléréséhez használható.</span><span class="sxs-lookup"><span data-stu-id="82742-130">It is not possible to use a Data Management Gateway from a custom activity to access on-premises data sources.</span></span> <span data-ttu-id="82742-131">Jelenleg [az adatkezelési átjáró](data-factory-data-management-gateway.md) csak a másolási tevékenység és a tárolt eljárási tevékenység támogatja az adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="82742-131">Currently, [Data Management Gateway](data-factory-data-management-gateway.md) supports only the copy activity and stored procedure activity in Data Factory.</span></span>   

## <a name="walkthrough-create-a-custom-activity"></a><span data-ttu-id="82742-132">Forgatókönyv: egyéni tevékenység létrehozása</span><span class="sxs-lookup"><span data-stu-id="82742-132">Walkthrough: create a custom activity</span></span>
### <a name="prerequisites"></a><span data-ttu-id="82742-133">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="82742-133">Prerequisites</span></span>
* <span data-ttu-id="82742-134">A Visual Studio 2012/2013 vagy 2015</span><span class="sxs-lookup"><span data-stu-id="82742-134">Visual Studio 2012/2013/2015</span></span>
* <span data-ttu-id="82742-135">Az [Azure .NET SDK](https://azure.microsoft.com/downloads/) letöltése és telepítése.</span><span class="sxs-lookup"><span data-stu-id="82742-135">Download and install [Azure .NET SDK](https://azure.microsoft.com/downloads/)</span></span>

### <a name="azure-batch-prerequisites"></a><span data-ttu-id="82742-136">Azure Batch-Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="82742-136">Azure Batch prerequisites</span></span>
<span data-ttu-id="82742-137">A forgatókönyv futtatása az egyéni .NET-tevékenységek használata az Azure Batch számítási erőforrásként.</span><span class="sxs-lookup"><span data-stu-id="82742-137">In the walkthrough, you run your custom .NET activities using Azure Batch as a compute resource.</span></span> <span data-ttu-id="82742-138">**Az Azure Batch** van egy platform szolgáltatás a felügyeleti teendők központjaként párhuzamosan futó és nagy teljesítményű számítástechnikai (HPC) alkalmazások hatékonyan a felhőben.</span><span class="sxs-lookup"><span data-stu-id="82742-138">**Azure Batch** is a platform service for running large-scale parallel and high-performance computing (HPC) applications efficiently in the cloud.</span></span> <span data-ttu-id="82742-139">Az Azure Batch ütemezi a számítási igényű munkát egy felügyelt futtathatnak **virtuális gépek**, és képes automatikusan méretezési számítási erőforrásokat a feladatok igényeinek.</span><span class="sxs-lookup"><span data-stu-id="82742-139">Azure Batch schedules compute-intensive work to run on a managed **collection of virtual machines**, and can automatically scale compute resources to meet the needs of your jobs.</span></span> <span data-ttu-id="82742-140">Lásd: [Azure Batch alapjai] [ batch-technical-overview] cikk részletes áttekintés az Azure Batch szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="82742-140">See [Azure Batch basics][batch-technical-overview] article for a detailed overview of the Azure Batch service.</span></span>

<span data-ttu-id="82742-141">Az oktatóanyag az Azure Batch-fiók létrehozása virtuális gépek készletét.</span><span class="sxs-lookup"><span data-stu-id="82742-141">For the tutorial, create an Azure Batch account with a pool of VMs.</span></span> <span data-ttu-id="82742-142">A lépések a következők:</span><span class="sxs-lookup"><span data-stu-id="82742-142">Here are the steps:</span></span>

1. <span data-ttu-id="82742-143">Hozzon létre egy **Azure Batch-fiók** használatával a [Azure-portálon](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="82742-143">Create an **Azure Batch account** using the [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="82742-144">Lásd: [létrehozása és kezelése az Azure Batch-fiók] [ batch-create-account] miként.</span><span class="sxs-lookup"><span data-stu-id="82742-144">See [Create and manage an Azure Batch account][batch-create-account] article for instructions.</span></span>
2. <span data-ttu-id="82742-145">Jegyezze fel az Azure Batch-fiók neve, fiókkulcs, URI és az alkalmazáskészlet neve.</span><span class="sxs-lookup"><span data-stu-id="82742-145">Note down the Azure Batch account name, account key, URI, and pool name.</span></span> <span data-ttu-id="82742-146">Már szükség csatolt Azure Batch szolgáltatás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="82742-146">You need them to create an Azure Batch linked service.</span></span>
    1. <span data-ttu-id="82742-147">A kezdőlapon Azure Batch-fiókhoz, megjelenik egy **URL-cím** a következő formátumban: `https://myaccount.westus.batch.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="82742-147">On the home page for Azure Batch account, you see a **URL** in the following format: `https://myaccount.westus.batch.azure.com`.</span></span> <span data-ttu-id="82742-148">Ebben a példában **myaccount** az Azure Batch-fiók neve.</span><span class="sxs-lookup"><span data-stu-id="82742-148">In this example, **myaccount** is the name of the Azure Batch account.</span></span> <span data-ttu-id="82742-149">A társított szolgáltatás definíciójának segítségével URI megadása nélkül a fiók nevét az URL-címet.</span><span class="sxs-lookup"><span data-stu-id="82742-149">URI you use in the linked service definition is the URL without the name of the account.</span></span> <span data-ttu-id="82742-150">Például: `https://<region>.batch.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="82742-150">For example: `https://<region>.batch.azure.com`.</span></span>
    2. <span data-ttu-id="82742-151">Kattintson a **kulcsok** a bal oldali menüben, és másolja a **elsődleges ELÉRÉSI kulcsot**.</span><span class="sxs-lookup"><span data-stu-id="82742-151">Click **Keys** on the left menu, and copy the **PRIMARY ACCESS KEY**.</span></span>
    3. <span data-ttu-id="82742-152">Egy létező alkalmazáskészlet használatához kattintson **készletek** a menüben, és jegyezze fel a **azonosító** a készlet.</span><span class="sxs-lookup"><span data-stu-id="82742-152">To use an existing pool, click **Pools** on the menu, and note down the **ID** of the pool.</span></span> <span data-ttu-id="82742-153">Ha egy meglévő készlet nem rendelkezik, helyezze át a következő lépéssel.</span><span class="sxs-lookup"><span data-stu-id="82742-153">If you don't have an existing pool, move to the next step.</span></span>     
2. <span data-ttu-id="82742-154">Hozzon létre egy **Azure Batch-készlet**.</span><span class="sxs-lookup"><span data-stu-id="82742-154">Create an **Azure Batch pool**.</span></span>

   1. <span data-ttu-id="82742-155">Az a [Azure-portálon](https://portal.azure.com), kattintson a **Tallózás** a bal oldali menüben, majd kattintson a **Batch-fiókok**.</span><span class="sxs-lookup"><span data-stu-id="82742-155">In the [Azure portal](https://portal.azure.com), click **Browse** in the left menu, and click **Batch Accounts**.</span></span>
   2. <span data-ttu-id="82742-156">Válassza ki az Azure Batch-fiók megnyitása a **Batch-fiók** panelen.</span><span class="sxs-lookup"><span data-stu-id="82742-156">Select your Azure Batch account to open the **Batch Account** blade.</span></span>
   3. <span data-ttu-id="82742-157">Kattintson a **készletek** csempére.</span><span class="sxs-lookup"><span data-stu-id="82742-157">Click **Pools** tile.</span></span>
   4. <span data-ttu-id="82742-158">Az a **készletek** panelen kattintson a Hozzáadás gombra az eszköztáron a készlet hozzáadása gombra.</span><span class="sxs-lookup"><span data-stu-id="82742-158">In the **Pools** blade, click Add button on the toolbar to add a pool.</span></span>
      1. <span data-ttu-id="82742-159">Adja meg a készlet (alkalmazáskészlet azonosítója) Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="82742-159">Enter an ID for the pool (Pool ID).</span></span> <span data-ttu-id="82742-160">Megjegyzés: a **a készlet Azonosítóját**; kell azt a Data Factory megoldás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="82742-160">Note the **ID of the pool**; you need it when creating the Data Factory solution.</span></span>
      2. <span data-ttu-id="82742-161">Adja meg **Windows Server 2012 R2** az operációsrendszer-családot beállítás.</span><span class="sxs-lookup"><span data-stu-id="82742-161">Specify **Windows Server 2012 R2** for the Operating System Family setting.</span></span>
      3. <span data-ttu-id="82742-162">Válassza ki a **csomóponti tarifacsomagot**.</span><span class="sxs-lookup"><span data-stu-id="82742-162">Select a **node pricing tier**.</span></span>
      4. <span data-ttu-id="82742-163">Adja meg **2** , értékét a **cél dedikált** beállítást.</span><span class="sxs-lookup"><span data-stu-id="82742-163">Enter **2** as value for the **Target Dedicated** setting.</span></span>
      5. <span data-ttu-id="82742-164">Adja meg **2** , értékét a **csomópontonkénti tevékenységek maximális** beállítást.</span><span class="sxs-lookup"><span data-stu-id="82742-164">Enter **2** as value for the **Max tasks per node** setting.</span></span>
   5. <span data-ttu-id="82742-165">A készlet létrehozásához kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="82742-165">Click **OK** to create the pool.</span></span>
   6. <span data-ttu-id="82742-166">Jegyezze fel a **azonosító** a készlet.</span><span class="sxs-lookup"><span data-stu-id="82742-166">Note down the **ID** of the pool.</span></span> 



### <a name="high-level-steps"></a><span data-ttu-id="82742-167">Magas szintű lépései</span><span class="sxs-lookup"><span data-stu-id="82742-167">High-level steps</span></span>
<span data-ttu-id="82742-168">Ez a forgatókönyv részeként elvégezhető két magas szintű lépései a következők:</span><span class="sxs-lookup"><span data-stu-id="82742-168">Here are the two high-level steps you perform as part of this walkthrough:</span></span> 

1. <span data-ttu-id="82742-169">Egyszerű adat-átalakítási/feldolgozó logika tartalmazó egyéni tevékenység létrehozása.</span><span class="sxs-lookup"><span data-stu-id="82742-169">Create a custom activity that contains simple data transformation/processing logic.</span></span>
2. <span data-ttu-id="82742-170">Hozzon létre egy Azure data factory egy folyamatot, amely az egyéni tevékenység használja.</span><span class="sxs-lookup"><span data-stu-id="82742-170">Create an Azure data factory with a pipeline that uses the custom activity.</span></span>

### <a name="create-a-custom-activity"></a><span data-ttu-id="82742-171">Egyéni tevékenység létrehozása</span><span class="sxs-lookup"><span data-stu-id="82742-171">Create a custom activity</span></span>
<span data-ttu-id="82742-172">.NET egyéni tevékenység, hozzon létre egy **.NET Class Library** egy osztály, amely megvalósítja az, hogy a projekt **IDotNetActivity** felületet.</span><span class="sxs-lookup"><span data-stu-id="82742-172">To create a .NET custom activity, create a **.NET Class Library** project with a class that implements that **IDotNetActivity** interface.</span></span> <span data-ttu-id="82742-173">Ez az interfész csak egy metódusa: [Execute](https://msdn.microsoft.com/library/azure/mt603945.aspx) , és az aláírása:</span><span class="sxs-lookup"><span data-stu-id="82742-173">This interface has only one method: [Execute](https://msdn.microsoft.com/library/azure/mt603945.aspx) and its signature is:</span></span>

```csharp
public IDictionary<string, string> Execute(
        IEnumerable<LinkedService> linkedServices,
        IEnumerable<Dataset> datasets,
        Activity activity,
        IActivityLogger logger)
```


<span data-ttu-id="82742-174">A metódus négy paramétereket fogadja:</span><span class="sxs-lookup"><span data-stu-id="82742-174">The method takes four parameters:</span></span>

- <span data-ttu-id="82742-175">**linkedServices**.</span><span class="sxs-lookup"><span data-stu-id="82742-175">**linkedServices**.</span></span> <span data-ttu-id="82742-176">Ez a tulajdonság egy-egy adattároló kapcsolódó szolgáltatások bemeneti/kimeneti adatkészletek a tevékenység által hivatkozott enumerálható lista.</span><span class="sxs-lookup"><span data-stu-id="82742-176">This property is an enumerable list of Data Store linked services referenced by input/output datasets for the activity.</span></span>   
- <span data-ttu-id="82742-177">**adatkészletek**.</span><span class="sxs-lookup"><span data-stu-id="82742-177">**datasets**.</span></span> <span data-ttu-id="82742-178">Ez a tulajdonság egy bemeneti/kimeneti adatkészletek tevékenység enumerálható listája.</span><span class="sxs-lookup"><span data-stu-id="82742-178">This property is an enumerable list of input/output datasets for the activity.</span></span> <span data-ttu-id="82742-179">A helyek és a bemeneti és kimeneti adatkészletek által megadott sémák használhatja ezt a paramétert.</span><span class="sxs-lookup"><span data-stu-id="82742-179">You can use this parameter to get the locations and schemas defined by input and output datasets.</span></span>
- <span data-ttu-id="82742-180">**tevékenység**.</span><span class="sxs-lookup"><span data-stu-id="82742-180">**activity**.</span></span> <span data-ttu-id="82742-181">Ez a tulajdonság adja meg az aktuális tevékenység.</span><span class="sxs-lookup"><span data-stu-id="82742-181">This property represents the current activity.</span></span> <span data-ttu-id="82742-182">Az egyéni tevékenység társított kiterjesztett tulajdonságok eléréséhez használható.</span><span class="sxs-lookup"><span data-stu-id="82742-182">It can be used to access extended properties associated with the custom activity.</span></span> <span data-ttu-id="82742-183">Lásd: [további tulajdonságok hozzáférés](#access-extended-properties) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="82742-183">See [Access extended properties](#access-extended-properties) for details.</span></span>
- <span data-ttu-id="82742-184">**naplózó**.</span><span class="sxs-lookup"><span data-stu-id="82742-184">**logger**.</span></span> <span data-ttu-id="82742-185">Ez az objektum lehetővé teszi a felhasználó napló a következő feldolgozási sor az adott felület hibakeresési megjegyzések írását.</span><span class="sxs-lookup"><span data-stu-id="82742-185">This object lets you write debug comments that surface in the user log for the pipeline.</span></span>

<span data-ttu-id="82742-186">A metódus visszaadja a szótár részére láncolni egyéni tevékenységek együtt a jövőben használható.</span><span class="sxs-lookup"><span data-stu-id="82742-186">The method returns a dictionary that can be used to chain custom activities together in the future.</span></span> <span data-ttu-id="82742-187">Ez a funkció még nem használható, így egy üres szótár visszaadásának metódus.</span><span class="sxs-lookup"><span data-stu-id="82742-187">This feature is not implemented yet, so return an empty dictionary from the method.</span></span>  

### <a name="procedure"></a><span data-ttu-id="82742-188">Eljárás</span><span class="sxs-lookup"><span data-stu-id="82742-188">Procedure</span></span>
1. <span data-ttu-id="82742-189">Hozzon létre egy **.NET Class Library** projekt.</span><span class="sxs-lookup"><span data-stu-id="82742-189">Create a **.NET Class Library** project.</span></span>
   <ol type="a">
     <li><span data-ttu-id="82742-190">Indítsa el <b>Visual Studio 2017</b> vagy <b>Visual Studio 2015-öt</b> vagy <b>Visual Studio 2013</b> vagy <b>Visual Studio 2012</b>.</span><span class="sxs-lookup"><span data-stu-id="82742-190">Launch <b>Visual Studio 2017</b> or <b>Visual Studio 2015</b> or <b>Visual Studio 2013</b> or <b>Visual Studio 2012</b>.</span></span></li>
     <li><span data-ttu-id="82742-191">Kattintson a <b>File</b> (Fájl) menüre, mutasson a <b>New</b> (Új) elemre, és kattintson a <b>Project</b> (Projekt) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="82742-191">Click <b>File</b>, point to <b>New</b>, and click <b>Project</b>.</span></span></li>
     <li><span data-ttu-id="82742-192">Bontsa ki a <b>Sablonok</b> lehetőséget, és válassza a <b>Visual C#</b> lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="82742-192">Expand <b>Templates</b>, and select <b>Visual C#</b>.</span></span> <span data-ttu-id="82742-193">Ebben a bemutatóban használhat C#, de bármilyen .NET nyelvi használhatja az egyéni tevékenység fejlesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="82742-193">In this walkthrough, you use C#, but you can use any .NET language to develop the custom activity.</span></span></li>
     <li><span data-ttu-id="82742-194">Válassza ki <b>Class Library</b> a jobb oldali projekttípusok közül.</span><span class="sxs-lookup"><span data-stu-id="82742-194">Select <b>Class Library</b> from the list of project types on the right.</span></span> <span data-ttu-id="82742-195">VS 2017, válassza a <b>Class Library (.NET-keretrendszer)</b> </span><span class="sxs-lookup"><span data-stu-id="82742-195">In VS 2017, choose <b>Class Library (.NET Framework)</b> </span></span></li>
     <li><span data-ttu-id="82742-196">Adja meg <b>MyDotNetActivity</b> a a <b>neve</b>.</span><span class="sxs-lookup"><span data-stu-id="82742-196">Enter <b>MyDotNetActivity</b> for the <b>Name</b>.</span></span></li>
     <li><span data-ttu-id="82742-197">Válassza ki <b>C:\ADFGetStarted</b> a a <b>hely</b>.</span><span class="sxs-lookup"><span data-stu-id="82742-197">Select <b>C:\ADFGetStarted</b> for the <b>Location</b>.</span></span></li>
     <li><span data-ttu-id="82742-198">A projekt létrehozásához kattintson az <b>OK</b> gombra.</span><span class="sxs-lookup"><span data-stu-id="82742-198">Click <b>OK</b> to create the project.</span></span></li>
   </ol><span data-ttu-id="82742-199">
2.Kattintson a **eszközök**, mutasson a **NuGet-Csomagkezelő**, és kattintson a **Csomagkezelő konzol**.</span><span class="sxs-lookup"><span data-stu-id="82742-199">
2. Click **Tools**, point to **NuGet Package Manager**, and click **Package Manager Console**.</span></span>
<span data-ttu-id="82742-200">3.</span><span class="sxs-lookup"><span data-stu-id="82742-200">3.</span></span> <span data-ttu-id="82742-201">A Package Manager Console hajtható végre a következő parancs futtatásával importálja **Microsoft.Azure.Management.DataFactories**.</span><span class="sxs-lookup"><span data-stu-id="82742-201">In the Package Manager Console, execute the following command to import **Microsoft.Azure.Management.DataFactories**.</span></span>

    ```PowerShell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. <span data-ttu-id="82742-202">Importálás a **Azure Storage** NuGet-csomagot a projekthez.</span><span class="sxs-lookup"><span data-stu-id="82742-202">Import the **Azure Storage** NuGet package in to the project.</span></span>

    ```PowerShell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="82742-203">Data Factory szolgáltatás indítója windowsazure.Storage kifejezésre 4.3 verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="82742-203">Data Factory service launcher requires the 4.3 version of WindowsAzure.Storage.</span></span> <span data-ttu-id="82742-204">Ha Azure Storage szerelvény újabb verziójára történő hivatkozás az egyéni tevékenység projektben, hibaüzenet jelenik meg a tevékenység végrehajtásakor.</span><span class="sxs-lookup"><span data-stu-id="82742-204">If you add a reference to a later version of Azure Storage assembly in your custom activity project, you see an error when the activity executes.</span></span> <span data-ttu-id="82742-205">A hiba elhárításához lásd: [Appdomain elkülönítési](#appdomain-isolation) szakasz.</span><span class="sxs-lookup"><span data-stu-id="82742-205">To resolve the error, see [Appdomain isolation](#appdomain-isolation) section.</span></span> 
5. <span data-ttu-id="82742-206">Adja hozzá a következő **használatával** utasítást, hogy a forrásfájl, a projektben.</span><span class="sxs-lookup"><span data-stu-id="82742-206">Add the following **using** statements to the source file in the project.</span></span>

    ```csharp

    // Comment these lines if using VS 2017
    using System.IO;
    using System.Globalization;
    using System.Diagnostics;
    using System.Linq;
    // --------------------

    // Comment these lines if using <= VS 2015
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    // ---------------------

    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Runtime;

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```
6. <span data-ttu-id="82742-207">Módosítsa a nevét a **névtér** való **MyDotNetActivityNS**.</span><span class="sxs-lookup"><span data-stu-id="82742-207">Change the name of the **namespace** to **MyDotNetActivityNS**.</span></span>

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. <span data-ttu-id="82742-208">Változtassa meg az osztály nevét **MyDotNetActivity** és a Származtatás a **IDotNetActivity** csatoló, ahogy az a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="82742-208">Change the name of the class to **MyDotNetActivity** and derive it from the **IDotNetActivity** interface as shown in the following code snippet:</span></span>

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. <span data-ttu-id="82742-209">Megvalósítása (Hozzáadás) a **Execute** metódusában a **IDotNetActivity** a csatoló a **MyDotNetActivity** osztályhoz, és másolja az alábbi példakód a metódust.</span><span class="sxs-lookup"><span data-stu-id="82742-209">Implement (Add) the **Execute** method of the **IDotNetActivity** interface to the **MyDotNetActivity** class and copy the following sample code to the method.</span></span>

    <span data-ttu-id="82742-210">Az alábbi minta előfordulási a keresési kifejezés ("Microsoft") található, az minden egyes blob egy adatszelet társított.</span><span class="sxs-lookup"><span data-stu-id="82742-210">The following sample counts the number of occurrences of the search term (“Microsoft”) in each blob associated with a data slice.</span></span>

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
        // get extended properties defined in activity JSON definition
        // (for example: SliceStart)
        DotNetActivity dotNetActivity = (DotNetActivity)activity.TypeProperties;
        string sliceStartString = dotNetActivity.ExtendedProperties["SliceStart"];
    
        // to log information, use the logger object
        // log all extended properties            
        IDictionary<string, string> extendedProperties = dotNetActivity.ExtendedProperties;
        logger.Write("Logging extended properties if any...");
        foreach (KeyValuePair<string, string> entry in extendedProperties)
        {
            logger.Write("<key:{0}> <value:{1}>", entry.Key, entry.Value);
        }
    
        // linked service for input and output data stores
        // in this example, same storage is used for both input/output
        AzureStorageLinkedService inputLinkedService;

        // get the input dataset
        Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
    
        // declare variables to hold type properties of input/output datasets
        AzureBlobDataset inputTypeProperties, outputTypeProperties;
        
        // get type properties from the dataset object
        inputTypeProperties = inputDataset.Properties.TypeProperties as AzureBlobDataset;
    
        // log linked services passed in linkedServices parameter
        // you will see two linked services of type: AzureStorage
        // one for input dataset and the other for output dataset 
        foreach (LinkedService ls in linkedServices)
            logger.Write("linkedService.Name {0}", ls.Name);
    
        // get the first Azure Storate linked service from linkedServices object
        // using First method instead of Single since we are using the same
        // Azure Storage linked service for input and output.
        inputLinkedService = linkedServices.First(
            linkedService =>
            linkedService.Name ==
            inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
            as AzureStorageLinkedService;
    
        // get the connection string in the linked service
        string connectionString = inputLinkedService.ConnectionString;
    
        // get the folder path from the input dataset definition
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
               // with the data slice. definition of the method is shown in the next step.
    
            output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
        } while (continuationToken != null);
    
        // get the output dataset using the name of the dataset matched to a name in the Activity output collection.
        Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);

        // get type properties for the output dataset
        outputTypeProperties = outputDataset.Properties.TypeProperties as AzureBlobDataset;
    
        // get the folder path from the output dataset definition
        folderPath = GetFolderPath(outputDataset);

        // log the output folder path   
        logger.Write("Writing blob to the folder: {0}", folderPath);
    
        // create a storage object for the output blob.
        CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
        // write the name of the file.
        Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    
        // log the output file name
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
9. <span data-ttu-id="82742-211">Adja hozzá a következő segédmódszereket:</span><span class="sxs-lookup"><span data-stu-id="82742-211">Add the following helper methods:</span></span> 

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

        // get type properties of the dataset   
        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
        if (blobDataset == null)
        {
            return null;
        }
    
        // return the folder path found in the type properties
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
    
        // get type properties of the dataset
        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
        if (blobDataset == null)
        {
            return null;
        }
    
        // return the blob/file name in the type properties
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

    <span data-ttu-id="82742-212">A GetFolderPath metódus visszaadja az elérési út a mappába, amely a DataSet adatkészlet mutat, és a GetFileName metódus a/fájlját, hogy az adatkészlet nevét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="82742-212">The GetFolderPath method returns the path to the folder that the dataset points to and the GetFileName method returns the name of the blob/file that the dataset points to.</span></span> <span data-ttu-id="82742-213">Ha Ön havefolderPath meghatározása változókkal például {Year}, {Month}, {Day} stb., a módszer eloszlás, ez a karakterlánc nem törli őket futásidejű értékekkel.</span><span class="sxs-lookup"><span data-stu-id="82742-213">If you havefolderPath defines using variables such as {Year}, {Month}, {Day} etc., the method returns the string as it is without replacing them with runtime values.</span></span> <span data-ttu-id="82742-214">Lásd: [további tulajdonságok hozzáférés](#access-extended-properties) című szakaszban talál információt elérése SliceStart, SliceEnd, stb.</span><span class="sxs-lookup"><span data-stu-id="82742-214">See [Access extended properties](#access-extended-properties) section for details on accessing SliceStart, SliceEnd, etc.</span></span>    

    ```JSON
    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "adftutorial/inputfolder/",
    ```

    <span data-ttu-id="82742-215">A Calculate metódus kulcsszó a bemeneti fájlok (a mappában található a bináris objektumok) a Microsoft-példányok száma számítja ki.</span><span class="sxs-lookup"><span data-stu-id="82742-215">The Calculate method calculates the number of instances of keyword Microsoft in the input files (blobs in the folder).</span></span> <span data-ttu-id="82742-216">A keresési kifejezés ("Microsoft") nem változtatható a kódban.</span><span class="sxs-lookup"><span data-stu-id="82742-216">The search term (“Microsoft”) is hard-coded in the code.</span></span>
10. <span data-ttu-id="82742-217">Fordítsa le a projektet.</span><span class="sxs-lookup"><span data-stu-id="82742-217">Compile the project.</span></span> <span data-ttu-id="82742-218">Kattintson a **Build** a menüre, majd a **megoldás fordítása**.</span><span class="sxs-lookup"><span data-stu-id="82742-218">Click **Build** from the menu and click **Build Solution**.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="82742-219">4.5.2-es verzió .NET-keretrendszer célkeretrendszerként a projekthez: kattintson a jobb gombbal a projektre, majd kattintson **tulajdonságok** a megcélzott keretrendszer beállításához.</span><span class="sxs-lookup"><span data-stu-id="82742-219">Set 4.5.2 version of .NET Framework as the target framework for your project: right-click the project, and click **Properties** to set the target framework.</span></span> <span data-ttu-id="82742-220">Adat-előállító nem támogatja a lefordított elleni .NET-keretrendszer verziója 4.5.2 később egyéni tevékenységeket.</span><span class="sxs-lookup"><span data-stu-id="82742-220">Data Factory does not support custom activities compiled against .NET Framework versions later than 4.5.2.</span></span>

11. <span data-ttu-id="82742-221">Indítsa el **Windows Explorer**, és navigáljon a **bin\debug** vagy **bin\release** mappa build típusától függően.</span><span class="sxs-lookup"><span data-stu-id="82742-221">Launch **Windows Explorer**, and navigate to **bin\debug** or **bin\release** folder depending on the type of build.</span></span>
12. <span data-ttu-id="82742-222">Hozzon létre egy zip fájlt **MyDotNetActivity.zip** , amely tartalmazza a bináris fájlok a <project folder>\bin\Debug mappába.</span><span class="sxs-lookup"><span data-stu-id="82742-222">Create a zip file **MyDotNetActivity.zip** that contains all the binaries in the <project folder>\bin\Debug folder.</span></span> <span data-ttu-id="82742-223">Tartalmazza a **MyDotNetActivity.pdb** fájlt úgy, hogy be további részletekről, mint a sor száma, amelyek a probléma oka, hogy hiba történt a forráskód.</span><span class="sxs-lookup"><span data-stu-id="82742-223">Include the **MyDotNetActivity.pdb** file so that you get additional details such as line number in the source code that caused the issue if there was a failure.</span></span> 

    > [!IMPORTANT]
    > <span data-ttu-id="82742-224">Az egyéni tevékenység zip-fájljában lévő összes fájlnak a **legfelső szinten** kell lennie, almappák nélkül.</span><span class="sxs-lookup"><span data-stu-id="82742-224">All the files in the zip file for the custom activity must be at the **top level** with no sub folders.</span></span>

    ![Bináris kimeneti fájlok](./media/data-factory-use-custom-activities/Binaries.png)
14. <span data-ttu-id="82742-226">Hozzon létre egy blob-tároló nevű **customactivitycontainer** Ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="82742-226">Create a blob container named **customactivitycontainer** if it does not already exist.</span></span> 
15. <span data-ttu-id="82742-227">A customactivitycontainer egy blobot, töltse fel a MyDotNetActivity.zip egy **általános célú** AzureStorageLinkedService által hivatkozott Azure blob storage (nem közbeni/ritkán Blob-tároló).</span><span class="sxs-lookup"><span data-stu-id="82742-227">Upload MyDotNetActivity.zip as a blob to the customactivitycontainer in a **general-purpose** Azure blob storage (not hot/cool Blob storage) that is referred by AzureStorageLinkedService.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="82742-228">Ha a .NET tevékenység projekt hozzáadása egy megoldást a Visual Studio Data Factory projektet tartalmaz, és adjon hozzá egy hivatkozást a Data Factory-alkalmazás projekt .NET tevékenység projekthez, nem kell hajtsa végre az utolsó két lépést a zip manuális létrehozása fájl, majd ismét feltölteni az általános célú Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="82742-228">If you add this .NET activity project to a solution in Visual Studio that contains a Data Factory project, and add a reference to .NET activity project from the Data Factory application project, you do not need to perform the last two steps of manually creating the zip file and uploading it to the general-purpose Azure blob storage.</span></span> <span data-ttu-id="82742-229">Ha közzéteszi a Visual Studio használatával a Data Factory entitások, ezeket a lépéseket automatikusan történik a közzétételi folyamat.</span><span class="sxs-lookup"><span data-stu-id="82742-229">When you publish Data Factory entities using Visual Studio, these steps are automatically done by the publishing process.</span></span> <span data-ttu-id="82742-230">További információkért lásd: [adat-előállító projektre a Visual Studio](#data-factory-project-in-visual-studio) szakasz.</span><span class="sxs-lookup"><span data-stu-id="82742-230">For more information, see [Data Factory project in Visual Studio](#data-factory-project-in-visual-studio) section.</span></span>

## <a name="create-a-pipeline-with-custom-activity"></a><span data-ttu-id="82742-231">Hozzon létre egy folyamatot egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="82742-231">Create a pipeline with custom activity</span></span>
<span data-ttu-id="82742-232">Egyéni tevékenység létrehozása és egy blob-tárolóba, a bináris fájljait a zip-fájl feltöltése a **általános célú** Azure Storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="82742-232">You have created a custom activity and uploaded the zip file with binaries to a blob container in a **general-purpose** Azure Storage Account.</span></span> <span data-ttu-id="82742-233">Ebben a szakaszban hoz létre egy Azure data factory egy folyamatot, amely az egyéni tevékenység használja.</span><span class="sxs-lookup"><span data-stu-id="82742-233">In this section, you create an Azure data factory with a pipeline that uses the custom activity.</span></span>

<span data-ttu-id="82742-234">Az egyéni tevékenység bemeneti adatkészletet blobok (fájlok) a customactivityinput mappában a blob Storage tárolóban adftutorial tároló jelöli.</span><span class="sxs-lookup"><span data-stu-id="82742-234">The input dataset for the custom activity represents blobs (files) in the customactivityinput folder of adftutorial container in the blob storage.</span></span> <span data-ttu-id="82742-235">A kimeneti adatkészlet a tevékenység kimeneti BLOB adftutorial tároló a blob Storage tárolóban customactivityoutput mappában jelöli.</span><span class="sxs-lookup"><span data-stu-id="82742-235">The output dataset for the activity represents output blobs in the customactivityoutput folder of adftutorial container in the blob storage.</span></span>

<span data-ttu-id="82742-236">Hozzon létre **file.txt** a következő tartalommal rendelkező fájlt, és töltse fel a **customactivityinput** mappában található a **adftutorial** tároló.</span><span class="sxs-lookup"><span data-stu-id="82742-236">Create **file.txt** file with the following content and upload it to **customactivityinput** folder of the **adftutorial** container.</span></span> <span data-ttu-id="82742-237">A adftutorial tároló létrehozása, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="82742-237">Create the adftutorial container if it does not exist already.</span></span> 

```
test custom activity Microsoft test custom activity Microsoft
```

<span data-ttu-id="82742-238">A bemeneti mappa felel meg az Azure Data Factory szelet akkor is, ha a mappa két vagy több fájlt.</span><span class="sxs-lookup"><span data-stu-id="82742-238">The input folder corresponds to a slice in Azure Data Factory even if the folder has two or more files.</span></span> <span data-ttu-id="82742-239">A folyamat minden szelet feldolgozása után az egyéni tevékenység az, hogy a szelet bemeneti mappa összes blobjának telepítéseket.</span><span class="sxs-lookup"><span data-stu-id="82742-239">When each slice is processed by the pipeline, the custom activity iterates through all the blobs in the input folder for that slice.</span></span>

<span data-ttu-id="82742-240">Egy kimeneti fájlt a adftutorial\customactivityoutput mappában (azonos számú blobot a bemeneti mappában) egy vagy több sort tartalmazó jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="82742-240">You see one output file with in the adftutorial\customactivityoutput folder with one or more lines (same as number of blobs in the input folder):</span></span>

```
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2016-11-16-00/file.txt.
```


<span data-ttu-id="82742-241">A jelen szakaszban végrehajtandó lépések a következők:</span><span class="sxs-lookup"><span data-stu-id="82742-241">Here are the steps you perform in this section:</span></span>

1. <span data-ttu-id="82742-242">Hozzon létre egy **adat-előállító**.</span><span class="sxs-lookup"><span data-stu-id="82742-242">Create a **data factory**.</span></span>
2. <span data-ttu-id="82742-243">Hozzon létre **összekapcsolt szolgáltatások** a virtuális gépek az Azure Batch-készlet, amely az egyéni tevékenység fut, és az Azure Storage a bemeneti/kimeneti BLOB tároló.</span><span class="sxs-lookup"><span data-stu-id="82742-243">Create **Linked services** for the Azure Batch pool of VMs on which the custom activity runs and the Azure Storage that holds the input/output blobs.</span></span>
3. <span data-ttu-id="82742-244">Hozzon létre a bemeneti és kimeneti **adatkészletek** , amelyek bemeneti és az egyéni tevékenység jelölik.</span><span class="sxs-lookup"><span data-stu-id="82742-244">Create input and output **datasets** that represent input and output of the custom activity.</span></span>
4. <span data-ttu-id="82742-245">Hozzon létre egy **csővezeték** , amely használja az egyéni tevékenység.</span><span class="sxs-lookup"><span data-stu-id="82742-245">Create a **pipeline** that uses the custom activity.</span></span>

> [!NOTE]
> <span data-ttu-id="82742-246">Hozzon létre a **file.txt** és feltöltése egy blob-tárolóba, ha még nem tette meg.</span><span class="sxs-lookup"><span data-stu-id="82742-246">Create the **file.txt** and upload it to a blob container if you haven't already done so.</span></span> <span data-ttu-id="82742-247">Lásd az előző részben található útmutatást.</span><span class="sxs-lookup"><span data-stu-id="82742-247">See instructions in the preceding section.</span></span>   

### <a name="step-1-create-the-data-factory"></a><span data-ttu-id="82742-248">1. lépés: Az adat-előállító létrehozása</span><span class="sxs-lookup"><span data-stu-id="82742-248">Step 1: Create the data factory</span></span>
1. <span data-ttu-id="82742-249">Az Azure-portálon való bejelentkezés után hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="82742-249">After logging in to the Azure portal, do the following steps:</span></span>
   1. <span data-ttu-id="82742-250">A bal oldali menüben kattintson a **NEW** (ÚJ) elemre.</span><span class="sxs-lookup"><span data-stu-id="82742-250">Click **NEW** on the left menu.</span></span>
   2. <span data-ttu-id="82742-251">Kattintson a **adatok + analitika** a a **új** panelen.</span><span class="sxs-lookup"><span data-stu-id="82742-251">Click **Data + Analytics** in the **New** blade.</span></span>
   3. <span data-ttu-id="82742-252">Kattintson a **Data Factory** elemre a **Data analytics** (Adatelemzés) panelen.</span><span class="sxs-lookup"><span data-stu-id="82742-252">Click **Data Factory** on the **Data analytics** blade.</span></span>
   
    ![Új Azure Data Factory menü](media/data-factory-use-custom-activities/new-azure-data-factory-menu.png)
2. <span data-ttu-id="82742-254">Az a **új adat-előállító** panelen adjon meg **CustomActivityFactory** nevét.</span><span class="sxs-lookup"><span data-stu-id="82742-254">In the **New data factory** blade, enter **CustomActivityFactory** for the Name.</span></span> <span data-ttu-id="82742-255">Az Azure data factory nevének globálisan egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="82742-255">The name of the Azure data factory must be globally unique.</span></span> <span data-ttu-id="82742-256">Ha a hibaüzenetet kapja: **nem érhető el adat-előállító "CustomActivityFactory"**, változtassa meg a data factory nevét (például **yournameCustomActivityFactory**), és próbálja meg újra létrehozni.</span><span class="sxs-lookup"><span data-stu-id="82742-256">If you receive the error: **Data factory name “CustomActivityFactory” is not available**, change the name of the data factory (for example, **yournameCustomActivityFactory**) and try creating again.</span></span>

    ![Új Azure Data Factory panel](media/data-factory-use-custom-activities/new-azure-data-factory-blade.png)
3. <span data-ttu-id="82742-258">Kattintson a **ERŐFORRÁSCSOPORT-név**, és válasszon ki egy meglévő erőforráscsoportot, vagy hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="82742-258">Click **RESOURCE GROUP NAME**, and select an existing resource group or create a resource group.</span></span>
4. <span data-ttu-id="82742-259">Ellenőrizze, hogy használja a megfelelő **előfizetés** és **régió** hol szeretné létrehozni az adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="82742-259">Verify that you are using the correct **subscription** and **region** where you want the data factory to be created.</span></span>
5. <span data-ttu-id="82742-260">Kattintson a **Create** (Létrehozás) elemre a **New data factory** (Új data factory) panelen.</span><span class="sxs-lookup"><span data-stu-id="82742-260">Click **Create** on the **New data factory** blade.</span></span>
6. <span data-ttu-id="82742-261">Megjelenik a data factory létrehozása a **irányítópult** az Azure portálról.</span><span class="sxs-lookup"><span data-stu-id="82742-261">You see the data factory being created in the **Dashboard** of the Azure portal.</span></span>
7. <span data-ttu-id="82742-262">Után az adat-előállító létrehozása sikerült, tekintse meg a Data Factory panel, amelyen az adat-előállítóban tartalmát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="82742-262">After the data factory has been created successfully, you see the Data Factory blade, which shows you the contents of the data factory.</span></span>
    
    ![A Data Factory panel](media/data-factory-use-custom-activities/data-factory-blade.png)

### <a name="step-2-create-linked-services"></a><span data-ttu-id="82742-264">2. lépés: A társított szolgáltatások létrehozásához</span><span class="sxs-lookup"><span data-stu-id="82742-264">Step 2: Create linked services</span></span>
<span data-ttu-id="82742-265">A társított szolgáltatások adattárakat vagy számítási szolgáltatásokat társítanak az Azure data factoryhez.</span><span class="sxs-lookup"><span data-stu-id="82742-265">Linked services link data stores or compute services to an Azure data factory.</span></span> <span data-ttu-id="82742-266">Ebben a lépésben kapcsolhat az Azure Storage-fiók és az Azure Batch-fiók a data factory.</span><span class="sxs-lookup"><span data-stu-id="82742-266">In this step, you link your Azure Storage account and Azure Batch account to your data factory.</span></span>

#### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="82742-267">Azure Storage társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="82742-267">Create Azure Storage linked service</span></span>
1. <span data-ttu-id="82742-268">Kattintson a **Szerző és központi telepítése** csempét a **adat-előállító** paneljén **CustomActivityFactory**.</span><span class="sxs-lookup"><span data-stu-id="82742-268">Click the **Author and deploy** tile on the **DATA FACTORY** blade for **CustomActivityFactory**.</span></span> <span data-ttu-id="82742-269">A Data Factory Editor láthatja.</span><span class="sxs-lookup"><span data-stu-id="82742-269">You see the Data Factory Editor.</span></span>
2. <span data-ttu-id="82742-270">Kattintson a **az új adattároló** a parancs megnyitásához, és válassza a **az Azure storage**.</span><span class="sxs-lookup"><span data-stu-id="82742-270">Click **New data store** on the command bar and choose **Azure storage**.</span></span> <span data-ttu-id="82742-271">A szerkesztőben megjelenik az Azure Storage társított szolgáltatás létrehozására szolgáló JSON-parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="82742-271">You should see the JSON script for creating an Azure Storage linked service in the editor.</span></span>
    
    ![Az új adattároló - Azure Storage](media/data-factory-use-custom-activities/new-data-store-menu.png)
3. <span data-ttu-id="82742-273">Cserélje le `<accountname>` az Azure storage-fiók nevére és `<accountkey>` az Azure storage-fiókjának elérési kulcsával.</span><span class="sxs-lookup"><span data-stu-id="82742-273">Replace `<accountname>` with name of your Azure storage account and `<accountkey>` with access key of the Azure storage account.</span></span> <span data-ttu-id="82742-274">A tárelérési kulcs lekérésével kapcsolatos információk: [Tárelérési kulcsok megtekintése, másolása és újragenerálása](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="82742-274">To learn how to get your storage access key, see [View, copy and regenerate storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>

    ![Az Azure Storage szolgáltatás tetszését](media/data-factory-use-custom-activities/azure-storage-linked-service.png)
4. <span data-ttu-id="82742-276">A társított szolgáltatás üzembe helyezéséhez kattintson a parancssáv **Deploy** (Üzembe helyezés) elemére.</span><span class="sxs-lookup"><span data-stu-id="82742-276">Click **Deploy** on the command bar to deploy the linked service.</span></span>

#### <a name="create-azure-batch-linked-service"></a><span data-ttu-id="82742-277">Csatolt Azure Batch szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="82742-277">Create Azure Batch linked service</span></span>
1. <span data-ttu-id="82742-278">Kattintson a Data Factory Editor **... További** a parancssávon kattintson **új számítási**, majd válassza ki **Azure Batch** a menüből.</span><span class="sxs-lookup"><span data-stu-id="82742-278">In the Data Factory Editor, click **... More** on the command bar, click **New compute**, and then select **Azure Batch** from the menu.</span></span>

    ![Új számítási - Azure Batch](media/data-factory-use-custom-activities/new-azure-compute-batch.png)
2. <span data-ttu-id="82742-280">A következő módosításokat a JSON-parancsfájlba:</span><span class="sxs-lookup"><span data-stu-id="82742-280">Make the following changes to the JSON script:</span></span>

   1. <span data-ttu-id="82742-281">Adja meg az Azure Batch-fiók nevét a **accountName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="82742-281">Specify Azure Batch account name for the **accountName** property.</span></span> <span data-ttu-id="82742-282">A **URL-cím** a a **Azure Batch-fiók panelen** a következő formátumban: `http://accountname.region.batch.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="82742-282">The **URL** from the **Azure Batch account blade** is in the following format: `http://accountname.region.batch.azure.com`.</span></span> <span data-ttu-id="82742-283">Az a **batchUri** tulajdonság a JSON-ban, el kell távolítania `accountname.` az URL-címet, és használja a `accountname` a a `accountName` JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="82742-283">For the **batchUri** property in the JSON, you need to remove `accountname.` from the URL and use the `accountname` for the `accountName` JSON property.</span></span>
   2. <span data-ttu-id="82742-284">Adja meg a szükséges Azure Batch-fiók kulcsot a **accessKey** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="82742-284">Specify the Azure Batch account key for the **accessKey** property.</span></span>
   3. <span data-ttu-id="82742-285">Adja meg a létrehozott alkalmazáskészlet nevét előfeltételei részeként a **poolName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="82742-285">Specify the name of the pool you created as part of prerequisites for the **poolName** property.</span></span> <span data-ttu-id="82742-286">A készlet neve helyett a készlet Azonosítóját is megadható.</span><span class="sxs-lookup"><span data-stu-id="82742-286">You can also specify the ID of the pool instead of the name of the pool.</span></span>
   4. <span data-ttu-id="82742-287">Adja meg az Azure Batch URI Azonosítót a **batchUri** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="82742-287">Specify Azure Batch URI for the **batchUri** property.</span></span> <span data-ttu-id="82742-288">Példa: `https://westus.batch.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="82742-288">Example: `https://westus.batch.azure.com`.</span></span>  
   5. <span data-ttu-id="82742-289">Adja meg a **AzureStorageLinkedService** a a **linkedServiceName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="82742-289">Specify the **AzureStorageLinkedService** for the **linkedServiceName** property.</span></span>

        ```json
        {
         "name": "AzureBatchLinkedService",
         "properties": {
           "type": "AzureBatch",
           "typeProperties": {
             "accountName": "myazurebatchaccount",
             "batchUri": "https://westus.batch.azure.com",
             "accessKey": "<yourbatchaccountkey>",
             "poolName": "myazurebatchpool",
             "linkedServiceName": "AzureStorageLinkedService"
           }
         }
        }
        ```

       <span data-ttu-id="82742-290">Az a **poolName** tulajdonság, azt is megadhatja a tárolókészlet neve helyett a készlet Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="82742-290">For the **poolName** property, you can also specify the ID of the pool instead of the name of the pool.</span></span>

      > [!IMPORTANT]
      > <span data-ttu-id="82742-291">A Data Factory szolgáltatásnak nem támogatja egy igény szerinti lehetőséget az Azure hdinsight azonban nem.</span><span class="sxs-lookup"><span data-stu-id="82742-291">The Data Factory service does not support an on-demand option for Azure Batch as it does for HDInsight.</span></span> <span data-ttu-id="82742-292">Csak a saját Azure Batch-készlet használhatja az Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="82742-292">You can only use your own Azure Batch pool in an Azure data factory.</span></span>   
    

### <a name="step-3-create-datasets"></a><span data-ttu-id="82742-293">3. lépés: Adatkészletek létrehozása</span><span class="sxs-lookup"><span data-stu-id="82742-293">Step 3: Create datasets</span></span>
<span data-ttu-id="82742-294">Ebben a lépésben hoz létre a bemeneti és kimeneti adatok adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="82742-294">In this step, you create datasets to represent input and output data.</span></span>

#### <a name="create-input-dataset"></a><span data-ttu-id="82742-295">Bemeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="82742-295">Create input dataset</span></span>
1. <span data-ttu-id="82742-296">A Data Factory **szerkesztőjében** kattintson a **... További** a parancssávon kattintson **új adatkészlet**, majd válassza ki **Azure Blob Storage tárolóban** a legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="82742-296">In the **Editor** for the Data Factory, click **... More** on the command bar, click **New dataset**, and then select **Azure Blob storage** from the drop-down menu.</span></span>
2. <span data-ttu-id="82742-297">A jobb oldali JSON cserélje le a következő JSON kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="82742-297">Replace the JSON in the right pane with the following JSON snippet:</span></span>

    ```json
    {
     "name": "InputDataset",
     "properties": {
         "type": "AzureBlob",
         "linkedServiceName": "AzureStorageLinkedService",
         "typeProperties": {
             "folderPath": "adftutorial/customactivityinput/",
             "format": {
                 "type": "TextFormat"
             }
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

   <span data-ttu-id="82742-298">Ez a forgatókönyv a kezdő időpont későbbi részében hozzon létre egy folyamatot: 2016-11-16T00:00:00Z és záró idő: 2016-11-16T05:00:00Z.</span><span class="sxs-lookup"><span data-stu-id="82742-298">You create a pipeline later in this walkthrough with start time: 2016-11-16T00:00:00Z and end time: 2016-11-16T05:00:00Z.</span></span> <span data-ttu-id="82742-299">Ütemezett eredményezett adatokat óránként, így öt bemeneti/kimeneti szeletek (közötti **00**: 00:00 -> **05**: 00:00).</span><span class="sxs-lookup"><span data-stu-id="82742-299">It is scheduled to produce data hourly, so there are five input/output slices (between **00**:00:00 -> **05**:00:00).</span></span>

   <span data-ttu-id="82742-300">A **gyakoriság** és **időköz** az bemeneti adatkészlet állítsa **óra** és **1**, ami azt jelenti, hogy a bemeneti szelet elérhető óránként.</span><span class="sxs-lookup"><span data-stu-id="82742-300">The **frequency** and **interval** for the input dataset is set to **Hour** and **1**, which means that the input slice is available hourly.</span></span> <span data-ttu-id="82742-301">Az ebben a példában a fájl (file.txt) a intputfolder is.</span><span class="sxs-lookup"><span data-stu-id="82742-301">In this sample, it is the same file (file.txt) in the intputfolder.</span></span>

   <span data-ttu-id="82742-302">Az alábbiakban a minden szelet, amely a fenti JSON-részlet SliceStart rendszer változó által képviselt kezdési idejét.</span><span class="sxs-lookup"><span data-stu-id="82742-302">Here are the start times for each slice, which is represented by SliceStart system variable in the above JSON snippet.</span></span>
3. <span data-ttu-id="82742-303">Kattintson a **telepítés** létrehozása és telepítése az eszköztáron a **InputDataset**.</span><span class="sxs-lookup"><span data-stu-id="82742-303">Click **Deploy** on the toolbar to create and deploy the **InputDataset**.</span></span> <span data-ttu-id="82742-304">Győződjön meg arról, hogy a szerkesztő címsorában megjelenik a **TABLE CREATED SUCCESSFULLY** (A TÁBLA SIKERESEN LÉTREJÖTT) üzenet.</span><span class="sxs-lookup"><span data-stu-id="82742-304">Confirm that you see the **TABLE CREATED SUCCESSFULLY** message on the title bar of the Editor.</span></span>

#### <a name="create-an-output-dataset"></a><span data-ttu-id="82742-305">Kimeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="82742-305">Create an output dataset</span></span>
1. <span data-ttu-id="82742-306">Az a **Data Factory editor**, kattintson a **... További** a parancssávon kattintson **új adatkészlet**, majd válassza ki **Azure Blob Storage tárolóban**.</span><span class="sxs-lookup"><span data-stu-id="82742-306">In the **Data Factory editor**, click **... More** on the command bar, click **New dataset**, and then select **Azure Blob storage**.</span></span>
2. <span data-ttu-id="82742-307">A JSON-parancsfájl, a jobb oldali cserélje le a következő JSON-parancsfájl:</span><span class="sxs-lookup"><span data-stu-id="82742-307">Replace the JSON script in the right pane with the following JSON script:</span></span>

    ```JSON
    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "{slice}.txt",
                "folderPath": "adftutorial/customactivityoutput/",
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

     <span data-ttu-id="82742-308">Kimeneti hely **adftutorial/customactivityoutput/** és kimeneti fájlnév: éééé-HH-NN-HH.txt, ha éééé-hh-nn ÓÓ az év, hónap, dátum és az éppen létrehozott szelet óra.</span><span class="sxs-lookup"><span data-stu-id="82742-308">Output location is **adftutorial/customactivityoutput/** and output file name is yyyy-MM-dd-HH.txt where yyyy-MM-dd-HH is the year, month, date, and hour of the slice being produced.</span></span> <span data-ttu-id="82742-309">Lásd: [fejlesztői leírás] [ adf-developer-reference] részleteiről.</span><span class="sxs-lookup"><span data-stu-id="82742-309">See [Developer Reference][adf-developer-reference] for details.</span></span>

    <span data-ttu-id="82742-310">Egy kimeneti blob/fájl az egyes bemeneti szeletek jön létre.</span><span class="sxs-lookup"><span data-stu-id="82742-310">An output blob/file is generated for each input slice.</span></span> <span data-ttu-id="82742-311">Ez hogyan kimeneti fájl neve az egyes szeletek.</span><span class="sxs-lookup"><span data-stu-id="82742-311">Here is how an output file is named for each slice.</span></span> <span data-ttu-id="82742-312">A kimeneti fájlok akkor jönnek létre, egy kimeneti mappában: **adftutorial\customactivityoutput**.</span><span class="sxs-lookup"><span data-stu-id="82742-312">All the output files are generated in one output folder: **adftutorial\customactivityoutput**.</span></span>

   | <span data-ttu-id="82742-313">Szelet</span><span class="sxs-lookup"><span data-stu-id="82742-313">Slice</span></span> | <span data-ttu-id="82742-314">Kezdési idő</span><span class="sxs-lookup"><span data-stu-id="82742-314">Start time</span></span> | <span data-ttu-id="82742-315">Kimeneti fájlja</span><span class="sxs-lookup"><span data-stu-id="82742-315">Output file</span></span> |
   |:--- |:--- |:--- |
   | <span data-ttu-id="82742-316">1</span><span class="sxs-lookup"><span data-stu-id="82742-316">1</span></span> |<span data-ttu-id="82742-317">2016-11-16T00:00:00</span><span class="sxs-lookup"><span data-stu-id="82742-317">2016-11-16T00:00:00</span></span> |<span data-ttu-id="82742-318">2016-11-16-00.txt</span><span class="sxs-lookup"><span data-stu-id="82742-318">2016-11-16-00.txt</span></span> |
   | <span data-ttu-id="82742-319">2</span><span class="sxs-lookup"><span data-stu-id="82742-319">2</span></span> |<span data-ttu-id="82742-320">2016-11-16T01:00:00</span><span class="sxs-lookup"><span data-stu-id="82742-320">2016-11-16T01:00:00</span></span> |<span data-ttu-id="82742-321">2016-11-16-01.txt</span><span class="sxs-lookup"><span data-stu-id="82742-321">2016-11-16-01.txt</span></span> |
   | <span data-ttu-id="82742-322">3</span><span class="sxs-lookup"><span data-stu-id="82742-322">3</span></span> |<span data-ttu-id="82742-323">2016-11-16T02:00:00</span><span class="sxs-lookup"><span data-stu-id="82742-323">2016-11-16T02:00:00</span></span> |<span data-ttu-id="82742-324">2016-11-16-02.txt</span><span class="sxs-lookup"><span data-stu-id="82742-324">2016-11-16-02.txt</span></span> |
   | <span data-ttu-id="82742-325">4</span><span class="sxs-lookup"><span data-stu-id="82742-325">4</span></span> |<span data-ttu-id="82742-326">2016-11-16T03:00:00</span><span class="sxs-lookup"><span data-stu-id="82742-326">2016-11-16T03:00:00</span></span> |<span data-ttu-id="82742-327">2016-11-16-03.txt</span><span class="sxs-lookup"><span data-stu-id="82742-327">2016-11-16-03.txt</span></span> |
   | <span data-ttu-id="82742-328">5</span><span class="sxs-lookup"><span data-stu-id="82742-328">5</span></span> |<span data-ttu-id="82742-329">2016-11-16T04:00:00</span><span class="sxs-lookup"><span data-stu-id="82742-329">2016-11-16T04:00:00</span></span> |<span data-ttu-id="82742-330">2016-11-16-04.txt</span><span class="sxs-lookup"><span data-stu-id="82742-330">2016-11-16-04.txt</span></span> |

    <span data-ttu-id="82742-331">Ne feledje, hogy egy bemeneti mappában lévő összes fájlt a fent említett kezdési idő feltüntetésével szelet részét.</span><span class="sxs-lookup"><span data-stu-id="82742-331">Remember that all the files in an input folder are part of a slice with the start times mentioned above.</span></span> <span data-ttu-id="82742-332">A szelet feldolgozása után az egyéni tevékenység minden fájl megvizsgálja, és hozza létre a keresési kifejezés ("Microsoft") előfordulásainak száma a kimeneti fájl egy sorban.</span><span class="sxs-lookup"><span data-stu-id="82742-332">When this slice is processed, the custom activity scans through each file and produces a line in the output file with the number of occurrences of search term (“Microsoft”).</span></span> <span data-ttu-id="82742-333">Ha a inputfolder három fájlok, vannak-e három sort a kimeneti fájl az egyes óránkénti szeletek: 2016-11-16:01:00:00.txt 2016-11-16-00.txt, stb.</span><span class="sxs-lookup"><span data-stu-id="82742-333">If there are three files in the inputfolder, there are three lines in the output file for each hourly slice: 2016-11-16-00.txt, 2016-11-16:01:00:00.txt, etc.</span></span>
3. <span data-ttu-id="82742-334">Központi telepítése a **OutputDataset**, kattintson a **telepítés** a parancssávon.</span><span class="sxs-lookup"><span data-stu-id="82742-334">To deploy the **OutputDataset**, click **Deploy** on the command bar.</span></span>

### <a name="create-and-run-a-pipeline-that-uses-the-custom-activity"></a><span data-ttu-id="82742-335">Hozzon létre és futtasson egy folyamatot, amely használja az egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="82742-335">Create and run a pipeline that uses the custom activity</span></span>
1. <span data-ttu-id="82742-336">Kattintson a Data Factory Editor **... További**, majd válassza ki **új adatcsatorna** a parancssávon.</span><span class="sxs-lookup"><span data-stu-id="82742-336">In the Data Factory Editor, click **... More**, and then select **New pipeline** on the command bar.</span></span> 
2. <span data-ttu-id="82742-337">A jobb oldali JSON cserélje le a következő JSON-parancsfájl:</span><span class="sxs-lookup"><span data-stu-id="82742-337">Replace the JSON in the right pane with the following JSON script:</span></span>

    ```JSON
    {
      "name": "ADFTutorialPipelineCustom",
      "properties": {
        "description": "Use custom activity",
        "activities": [
          {
            "Name": "MyDotNetActivity",
            "Type": "DotNetActivity",
            "Inputs": [
              {
                "Name": "InputDataset"
              }
            ],
            "Outputs": [
              {
                "Name": "OutputDataset"
              }
            ],
            "LinkedServiceName": "AzureBatchLinkedService",
            "typeProperties": {
              "AssemblyName": "MyDotNetActivity.dll",
              "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
              "PackageLinkedService": "AzureStorageLinkedService",
              "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
              "extendedProperties": {
                "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
              }
            },
            "Policy": {
              "Concurrency": 2,
              "ExecutionPriorityOrder": "OldestFirst",
              "Retry": 3,
              "Timeout": "00:30:00",
              "Delay": "00:00:00"
            }
          }
        ],
        "start": "2016-11-16T00:00:00Z",
        "end": "2016-11-16T05:00:00Z",
        "isPaused": false
      }
    }
    ```

    <span data-ttu-id="82742-338">Vegye figyelembe a következő szempontokat:</span><span class="sxs-lookup"><span data-stu-id="82742-338">Note the following points:</span></span>

   * <span data-ttu-id="82742-339">**Egyidejűségi** értéke **2** , hogy két szeletek párhuzamosan dolgozza fel az Azure Batch-készletben 2 virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="82742-339">**Concurrency** is set to **2** so that two slices are processed in parallel by 2 VMs in the Azure Batch pool.</span></span>
   * <span data-ttu-id="82742-340">Egy tevékenység szerepel a tevékenységek szakasz és a típusa: **DotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="82742-340">There is one activity in the activities section and it is of type: **DotNetActivity**.</span></span>
   * <span data-ttu-id="82742-341">**AssemblyName** értéke a dll-fájl neve: **MyDotnetActivity.dll**.</span><span class="sxs-lookup"><span data-stu-id="82742-341">**AssemblyName** is set to the name of the DLL: **MyDotnetActivity.dll**.</span></span>
   * <span data-ttu-id="82742-342">**EntryPoint** értéke **MyDotNetActivityNS.MyDotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="82742-342">**EntryPoint** is set to **MyDotNetActivityNS.MyDotNetActivity**.</span></span>
   * <span data-ttu-id="82742-343">**PackageLinkedService** értéke **AzureStorageLinkedService** a blob-tároló, amely tartalmazza az egyéni tevékenység zip-fájl mutat.</span><span class="sxs-lookup"><span data-stu-id="82742-343">**PackageLinkedService** is set to **AzureStorageLinkedService** that points to the blob storage that contains the custom activity zip file.</span></span> <span data-ttu-id="82742-344">Különböző Azure Storage-fiókok használatakor a bemeneti/kimeneti fájlok és az egyéni tevékenység zip-fájlt hoz létre egy másik Azure tárolás társított szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="82742-344">If you are using different Azure Storage accounts for input/output files and the custom activity zip file, you create another Azure Storage linked service.</span></span> <span data-ttu-id="82742-345">Ez a cikk feltételezi, hogy a azonos Azure Storage-fiókot használja.</span><span class="sxs-lookup"><span data-stu-id="82742-345">This article assumes that you are using the same Azure Storage account.</span></span>
   * <span data-ttu-id="82742-346">**PackageFile** értéke **customactivitycontainer/MyDotNetActivity.zip**.</span><span class="sxs-lookup"><span data-stu-id="82742-346">**PackageFile** is set to **customactivitycontainer/MyDotNetActivity.zip**.</span></span> <span data-ttu-id="82742-347">A a formátumban: containerforthezip/nameofthezip.zip.</span><span class="sxs-lookup"><span data-stu-id="82742-347">It is in the format: containerforthezip/nameofthezip.zip.</span></span>
   * <span data-ttu-id="82742-348">Az egyéni tevékenység veszi **InputDataset** bemenetként és **OutputDataset** output típusúként.</span><span class="sxs-lookup"><span data-stu-id="82742-348">The custom activity takes **InputDataset** as input and **OutputDataset** as output.</span></span>
   * <span data-ttu-id="82742-349">A linkedServiceName tulajdonságot az egyéni tevékenység mutat a **AzureBatchLinkedService**, amely közli az Azure Data Factory, amely az egyéni tevékenység az Azure Batch virtuális gépek futtatásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="82742-349">The linkedServiceName property of the custom activity points to the **AzureBatchLinkedService**, which tells Azure Data Factory that the custom activity needs to run on Azure Batch VMs.</span></span>
   * <span data-ttu-id="82742-350">**isPaused** tulajdonsága **hamis** alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="82742-350">**isPaused** property is set to **false** by default.</span></span> <span data-ttu-id="82742-351">A folyamat a rendszer azonnal futtatja ebben a példában a szeletek indítsa el a régebbi, mert.</span><span class="sxs-lookup"><span data-stu-id="82742-351">The pipeline runs immediately in this example because the slices start in the past.</span></span> <span data-ttu-id="82742-352">Ez a tulajdonság igaz értékre a feldolgozási sor szüneteltetése, és állítsa vissza újraindítására hamis beállíthatja.</span><span class="sxs-lookup"><span data-stu-id="82742-352">You can set this property to true to pause the pipeline and set it back to false to restart.</span></span>
   * <span data-ttu-id="82742-353">A **start** idő és **end** idők **öt** egymástól óra és szeletek előállítása hourly, így öt szeletek hozzák létre a folyamatot.</span><span class="sxs-lookup"><span data-stu-id="82742-353">The **start** time and **end** times are **five** hours apart and slices are produced hourly, so five slices are produced by the pipeline.</span></span>
3. <span data-ttu-id="82742-354">Kattintson újra az adatcsatornát, **telepítés** a parancssávon.</span><span class="sxs-lookup"><span data-stu-id="82742-354">To deploy the pipeline, click **Deploy** on the command bar.</span></span>

### <a name="monitor-the-pipeline"></a><span data-ttu-id="82742-355">A folyamat figyelése</span><span class="sxs-lookup"><span data-stu-id="82742-355">Monitor the pipeline</span></span>
1. <span data-ttu-id="82742-356">A Data Factory panelen az Azure portálon kattintson **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="82742-356">In the Data Factory blade in the Azure portal, click **Diagram**.</span></span>

    ![Diagram csempe](./media/data-factory-use-custom-activities/DataFactoryBlade.png)
2. <span data-ttu-id="82742-358">A Diagram nézetben kattintson a OutputDataset.</span><span class="sxs-lookup"><span data-stu-id="82742-358">In the Diagram View, now click the OutputDataset.</span></span>

    ![Diagramnézet](./media/data-factory-use-custom-activities/diagram.png)
3. <span data-ttu-id="82742-360">Megtekintheti, hogy az öt kimeneti szeletek kész állapotban van-e.</span><span class="sxs-lookup"><span data-stu-id="82742-360">You should see that the five output slices are in the Ready state.</span></span> <span data-ttu-id="82742-361">Ha nem üzemkész állapotban, hogy még nem készült még.</span><span class="sxs-lookup"><span data-stu-id="82742-361">If they are not in the Ready state, they haven't been produced yet.</span></span> 

   ![Kimeneti szeletek](./media/data-factory-use-custom-activities/OutputSlices.png)
4. <span data-ttu-id="82742-363">Győződjön meg arról, hogy a kimeneti fájlok akkor jönnek létre, a blob Storage tárolóban lévő a **adftutorial** tároló.</span><span class="sxs-lookup"><span data-stu-id="82742-363">Verify that the output files are generated in the blob storage in the **adftutorial** container.</span></span>

   ![egyéni tevékenység kimenetét][image-data-factory-ouput-from-custom-activity]
5. <span data-ttu-id="82742-365">A kimeneti fájl megnyitásakor, a következő kimeneti hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="82742-365">If you open the output file, you should see the output similar to the following output:</span></span>

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2016-11-16-00/file.txt.
    ```
6. <span data-ttu-id="82742-366">Használja a [Azure-portálon] [ azure-preview-portal] vagy az Azure PowerShell-parancsmagok segítségével figyelheti az adat-előállító, folyamatok és adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="82742-366">Use the [Azure portal][azure-preview-portal] or Azure PowerShell cmdlets to monitor your data factory, pipelines, and data sets.</span></span> <span data-ttu-id="82742-367">Láthatja, hogy az üzenetek a **ActivityLogger** a naplók (kifejezetten felhasználói-0.log) tölthet le a portál vagy parancsmagok használatával az egyéni tevékenység a kódban.</span><span class="sxs-lookup"><span data-stu-id="82742-367">You can see messages from the **ActivityLogger** in the code for the custom activity in the logs (specifically user-0.log) that you can download from the portal or using cmdlets.</span></span>

   ![az egyéni tevékenység naplók letöltése][image-data-factory-download-logs-from-custom-activity]

<span data-ttu-id="82742-369">Lásd: [figyelő és folyamatok kezelése](data-factory-monitor-manage-pipelines.md) adathalmazok és adatcsatornák figyelemmel kísérésére részletes leírást.</span><span class="sxs-lookup"><span data-stu-id="82742-369">See [Monitor and Manage Pipelines](data-factory-monitor-manage-pipelines.md) for detailed steps for monitoring datasets and pipelines.</span></span>      

## <a name="data-factory-project-in-visual-studio"></a><span data-ttu-id="82742-370">Data Factory projektre a Visual Studióban</span><span class="sxs-lookup"><span data-stu-id="82742-370">Data Factory project in Visual Studio</span></span>  
<span data-ttu-id="82742-371">Hozzon létre, és tegye közzé a Data Factory entitásokat Visual Studio használatával Azure portál használata helyett.</span><span class="sxs-lookup"><span data-stu-id="82742-371">You can create and publish Data Factory entities by using Visual Studio instead of using Azure portal.</span></span> <span data-ttu-id="82742-372">Részletes információkat létrehozása és közzététele a Data Factory entitások Visual Studio használatával, lásd: [felépítheti első folyamatát Visual Studio használatával](data-factory-build-your-first-pipeline-using-vs.md) és [adatok másolása az Azure Blob az Azure SQL](data-factory-copy-activity-tutorial-using-visual-studio.md) cikkek.</span><span class="sxs-lookup"><span data-stu-id="82742-372">For detailed information about creating and publishing Data Factory entities by using Visual Studio, See [Build your first pipeline using Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) and [Copy data from Azure Blob to Azure SQL](data-factory-copy-activity-tutorial-using-visual-studio.md) articles.</span></span>

<span data-ttu-id="82742-373">Adat-előállító projekt létrehozása a Visual Studio, hajtsa végre az alábbi kiegészítő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="82742-373">Do the following additional steps if you are creating Data Factory project in Visual Studio:</span></span>
 
1. <span data-ttu-id="82742-374">Adja hozzá a Data Factory projektet a Visual Studio megoldás, amely az egyéni tevékenység projektet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="82742-374">Add the Data Factory project to the Visual Studio solution that contains the custom activity project.</span></span> 
2. <span data-ttu-id="82742-375">Adjon hozzá egy hivatkozást a .NET tevékenység projektre a Data Factory projektből.</span><span class="sxs-lookup"><span data-stu-id="82742-375">Add a reference to the .NET activity project from the Data Factory project.</span></span> <span data-ttu-id="82742-376">Kattintson jobb gombbal a Data Factory projekt **Hozzáadás**, és kattintson a **hivatkozás**.</span><span class="sxs-lookup"><span data-stu-id="82742-376">Right-click Data Factory project, point to **Add**, and then click **Reference**.</span></span> 
3. <span data-ttu-id="82742-377">Az a **hivatkozás hozzáadása** párbeszédpanelen jelölje ki a **MyDotNetActivity** projektre, majd kattintson az **OK**.</span><span class="sxs-lookup"><span data-stu-id="82742-377">In the **Add Reference** dialog box, select the **MyDotNetActivity** project, and click **OK**.</span></span>
4. <span data-ttu-id="82742-378">Hozza létre, és a megoldás közzététele.</span><span class="sxs-lookup"><span data-stu-id="82742-378">Build and publish the solution.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="82742-379">Ha közzéteszi a Data Factory entitások, automatikusan létrejön egy zip-fájlt, és blobtárolóba feltöltött: customactivitycontainer.</span><span class="sxs-lookup"><span data-stu-id="82742-379">When you publish Data Factory entities, a zip file is automatically created for you and is uploaded to the blob container: customactivitycontainer.</span></span> <span data-ttu-id="82742-380">Ha a blob-tároló nem létezik, automatikusan létrejön túl.</span><span class="sxs-lookup"><span data-stu-id="82742-380">If the blob container does not exist, it is automatically created too.</span></span>  


## <a name="data-factory-and-batch-integration"></a><span data-ttu-id="82742-381">Adat-előállító és kötegelt integrációja</span><span class="sxs-lookup"><span data-stu-id="82742-381">Data Factory and Batch integration</span></span>
<span data-ttu-id="82742-382">A Data Factory szolgáltatásnak hoz létre egy feladatot az Azure Batch nevű: **adf-poolname: feladat-xxx**.</span><span class="sxs-lookup"><span data-stu-id="82742-382">The Data Factory service creates a job in Azure Batch with the name: **adf-poolname: job-xxx**.</span></span> <span data-ttu-id="82742-383">Kattintson a **feladatok** a bal oldali menüből.</span><span class="sxs-lookup"><span data-stu-id="82742-383">Click **Jobs** from the left menu.</span></span> 

![Az Azure Data Factory - kötegelt feladatok](media/data-factory-use-custom-activities/data-factory-batch-jobs.png)

<span data-ttu-id="82742-385">Egy feladat minden egyes tevékenység futtatásához a szelet jön létre.</span><span class="sxs-lookup"><span data-stu-id="82742-385">A task is created for each activity run of a slice.</span></span> <span data-ttu-id="82742-386">Ha készen áll a feldolgozásra öt szeletek, ez a feladat öt feladatok jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="82742-386">If there are five slices ready to be processed, five tasks are created in this job.</span></span> <span data-ttu-id="82742-387">A Batch-készlet több számítási csomópontok szerepelnek, ha két vagy több szeletek párhuzamosan futtatható.</span><span class="sxs-lookup"><span data-stu-id="82742-387">If there are multiple compute nodes in the Batch pool, two or more slices can run in parallel.</span></span> <span data-ttu-id="82742-388">Ha egyes számítási csomópontjain maximális feladatok értéke > 1, az azonos számítási futó egynél több szelet is lehet.</span><span class="sxs-lookup"><span data-stu-id="82742-388">If the maximum tasks per compute node is set to > 1, you can also have more than one slice running on the same compute.</span></span>

![Az Azure Data Factory - kötegelt munka feladatai](media/data-factory-use-custom-activities/data-factory-batch-job-tasks.png)

<span data-ttu-id="82742-390">A következő ábra bemutatja az Azure Data Factory és kötegelt feladatok közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="82742-390">The following diagram illustrates the relationship between Azure Data Factory and Batch tasks.</span></span>

![Adat-előállító & kötegelt](./media/data-factory-use-custom-activities/DataFactoryAndBatch.png)

## <a name="troubleshoot-failures"></a><span data-ttu-id="82742-392">Hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="82742-392">Troubleshoot failures</span></span>
<span data-ttu-id="82742-393">Néhány alapvető technikák áll:</span><span class="sxs-lookup"><span data-stu-id="82742-393">Troubleshooting consists of a few basic techniques:</span></span>

1. <span data-ttu-id="82742-394">Ha hibaüzenet jelenik meg, előfordulhat, hogy használni egy gyakran használt adatok/ritkán blob-tároló egy általános célú Azure blob storage használata helyett.</span><span class="sxs-lookup"><span data-stu-id="82742-394">If you see the following error, you may be using a Hot/Cool blob storage instead of using a general-purpose Azure blob storage.</span></span> <span data-ttu-id="82742-395">A zip-fájlt feltölteni egy **általános célú Azure Storage-fiók**.</span><span class="sxs-lookup"><span data-stu-id="82742-395">Upload the zip file to a **general-purpose Azure Storage Account**.</span></span> 
 
    ```
    Error in Activity: Job encountered scheduling error. Code: BlobDownloadMiscError Category: ServerError Message: Miscellaneous error encountered while downloading one of the specified Azure Blob(s).
    ``` 
2. <span data-ttu-id="82742-396">Ha a következő hibát látja, ellenőrizze, hogy az osztály a CS-fájl neve megegyezik a megadott név a **EntryPoint** az adatcsatorna JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="82742-396">If you see the following error, confirm that the name of the class in the CS file matches the name you specified for the **EntryPoint** property in the pipeline JSON.</span></span> <span data-ttu-id="82742-397">A forgatókönyv osztály neve van: MyDotNetActivity, illetve a belépési pont a JSON-ban: MyDotNetActivityNS. **MyDotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="82742-397">In the walkthrough, name of the class is: MyDotNetActivity, and the EntryPoint in the JSON is: MyDotNetActivityNS.**MyDotNetActivity**.</span></span>

    ```
    MyDotNetActivity assembly does not exist or doesn't implement the type Microsoft.DataFactories.Runtime.IDotNetActivity properly
    ```

   <span data-ttu-id="82742-398">Ha felelnek meg, ellenőrizze, hogy a bináris fájlok a a **gyökérmappa** zip-fájlban.</span><span class="sxs-lookup"><span data-stu-id="82742-398">If the names do match, confirm that all the binaries are in the **root folder** of the zip file.</span></span> <span data-ttu-id="82742-399">Ez azt jelenti, hogy a zip-fájl megnyitásakor, megtekintheti az összes fájl a gyökérmappában található, és nem minden almappa.</span><span class="sxs-lookup"><span data-stu-id="82742-399">That is, when you open the zip file, you should see all the files in the root folder, not in any sub folders.</span></span>   
3. <span data-ttu-id="82742-400">Ha nincs beállítva a bemeneti szelet **készen**, győződjön meg arról, hogy helyesen-e a bemeneti gyökérmappa-szerkezetében és **file.txt** szerepel a bemeneti mappákat.</span><span class="sxs-lookup"><span data-stu-id="82742-400">If the input slice is not set to **Ready**, confirm that the input folder structure is correct and **file.txt** exists in the input folders.</span></span>
3. <span data-ttu-id="82742-401">Az a **Execute** metódus az egyéni tevékenység, használja a **IActivityLogger** objektum, amely segít a problémák elhárításához adatok naplózására.</span><span class="sxs-lookup"><span data-stu-id="82742-401">In the **Execute** method of your custom activity, use the **IActivityLogger** object to log information that helps you troubleshoot issues.</span></span> <span data-ttu-id="82742-402">A felhasználó naplófájlok megjelennek a naplózott üzeneteket (egy vagy több fájlt nevű: felhasználó-0.log, felhasználó-1.log, felhasználó-2.log, stb.).</span><span class="sxs-lookup"><span data-stu-id="82742-402">The logged messages show up in the user log files (one or more files named: user-0.log, user-1.log, user-2.log, etc.).</span></span>

   <span data-ttu-id="82742-403">Az a **OutputDataset** panelen a szelet megtekintéséhez kattintson a **ADATSZELET** , hogy a szelet paneljét.</span><span class="sxs-lookup"><span data-stu-id="82742-403">In the **OutputDataset** blade, click the slice to see the **DATA SLICE** blade for that slice.</span></span> <span data-ttu-id="82742-404">Látni **tevékenységek** az adott szeletek.</span><span class="sxs-lookup"><span data-stu-id="82742-404">You see **activity runs** for that slice.</span></span> <span data-ttu-id="82742-405">Meg kell jelennie egy tevékenység Futtatás újrapróbálása.</span><span class="sxs-lookup"><span data-stu-id="82742-405">You should see one activity run for the slice.</span></span> <span data-ttu-id="82742-406">Kattintson a Futtatás parancsra a parancssávon, ha egy másik, azonos szeletre vonatkozó tevékenységfuttatási is elindítható.</span><span class="sxs-lookup"><span data-stu-id="82742-406">If you click Run in the command bar, you can start another activity run for the same slice.</span></span>

   <span data-ttu-id="82742-407">Ha a tevékenység futtatása gombra kattint, megjelenik a **tevékenység futtatása részletei** naplófájlok listáját tartalmazó panel.</span><span class="sxs-lookup"><span data-stu-id="82742-407">When you click the activity run, you see the **ACTIVITY RUN DETAILS** blade with a list of log files.</span></span> <span data-ttu-id="82742-408">Megjelenik a user_0.log fájlban naplózott üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="82742-408">You see logged messages in the user_0.log file.</span></span> <span data-ttu-id="82742-409">Ha hiba történik, megjelenik az három tevékenység fut oka az újrapróbálkozások maximális számát a feldolgozási sor/tevékenységben JSON 3 értékre van beállítva.</span><span class="sxs-lookup"><span data-stu-id="82742-409">When an error occurs, you see three activity runs because the retry count is set to 3 in the pipeline/activity JSON.</span></span> <span data-ttu-id="82742-410">Amikor a tevékenység futtatása gombra kattint, láthatja a hiba elhárítása érdekében tekintse át a naplófájlokat.</span><span class="sxs-lookup"><span data-stu-id="82742-410">When you click the activity run, you see the log files that you can review to troubleshoot the error.</span></span>

   <span data-ttu-id="82742-411">A naplófájlok, kattintson a **felhasználói-0.log**.</span><span class="sxs-lookup"><span data-stu-id="82742-411">In the list of log files, click the **user-0.log**.</span></span> <span data-ttu-id="82742-412">A jobb oldali panelen a következők használatával eredményeit a **IActivityLogger.Write** metódust.</span><span class="sxs-lookup"><span data-stu-id="82742-412">In the right panel are the results of using the **IActivityLogger.Write** method.</span></span> <span data-ttu-id="82742-413">Ha az összes üzenet nem látható, ellenőrizze, hogy van-e további naplófájlokat nevű: user_1.log, user_2.log stb. Ellenkező esetben a kód nem sikerült az utolsó üzenet bejelentkezést követően.</span><span class="sxs-lookup"><span data-stu-id="82742-413">If you don't see all messages, check if you have more log files named: user_1.log, user_2.log etc. Otherwise, the code may have failed after the last logged message.</span></span>

   <span data-ttu-id="82742-414">Ezenkívül ellenőrizze **rendszer-0.log** rendszer hibaüzeneteket és kivételeket.</span><span class="sxs-lookup"><span data-stu-id="82742-414">In addition, check **system-0.log** for any system error messages and exceptions.</span></span>
4. <span data-ttu-id="82742-415">Tartalmazza a **PDB** , hogy a hiba részletei információkat, mint a zip-fájlban szereplő fájl **hívási verem** Ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="82742-415">Include the **PDB** file in the zip file so that the error details have information such as **call stack** when an error occurs.</span></span>
5. <span data-ttu-id="82742-416">Az egyéni tevékenység zip-fájljában lévő összes fájlnak a **legfelső szinten** kell lennie, almappák nélkül.</span><span class="sxs-lookup"><span data-stu-id="82742-416">All the files in the zip file for the custom activity must be at the **top level** with no sub folders.</span></span>
6. <span data-ttu-id="82742-417">Győződjön meg arról, hogy a **assemblyName** (MyDotNetActivity.dll) **entryPoint**(MyDotNetActivityNS.MyDotNetActivity) **packageFile** (customactivitycontainer / MyDotNetActivity.zip), és **packageLinkedService** (kell mutatnia a **általános célú**Azure blob-tároló, amely a zip-fájl tartalmazza) helyes az értékük értékre van beállítva.</span><span class="sxs-lookup"><span data-stu-id="82742-417">Ensure that the **assemblyName** (MyDotNetActivity.dll), **entryPoint**(MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer/MyDotNetActivity.zip), and **packageLinkedService** (should point to the **general-purpose**Azure blob storage that contains the zip file) are set to correct values.</span></span>
7. <span data-ttu-id="82742-418">Ha kijavított egy hibát, és újra fel szeretné dolgozni a szeletet, kattintson a jobb gombbal a szeletre az **OutputDataset** panelen, és kattintson a **Futtatás** parancsra.</span><span class="sxs-lookup"><span data-stu-id="82742-418">If you fixed an error and want to reprocess the slice, right-click the slice in the **OutputDataset** blade and click **Run**.</span></span>
8. <span data-ttu-id="82742-419">Ha a következő hibát látja, használja az Azure Storage csomag verziója > 4.3.0.</span><span class="sxs-lookup"><span data-stu-id="82742-419">If you see the following error, you are using the Azure Storage package of version > 4.3.0.</span></span> <span data-ttu-id="82742-420">Data Factory szolgáltatás indítója windowsazure.Storage kifejezésre 4.3 verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="82742-420">Data Factory service launcher requires the 4.3 version of WindowsAzure.Storage.</span></span> <span data-ttu-id="82742-421">Lásd: [Appdomain elkülönítési](#appdomain-isolation) résznél a munkahelyi körül, ha az Azure Storage szerelvény újabb verzióját kell használnia.</span><span class="sxs-lookup"><span data-stu-id="82742-421">See [Appdomain isolation](#appdomain-isolation) section for a work-around if you must use the later version of Azure Storage assembly.</span></span> 

    ```
    Error in Activity: Unknown error in module: System.Reflection.TargetInvocationException: Exception has been thrown by the target of an invocation. ---> System.TypeLoadException: Could not load type 'Microsoft.WindowsAzure.Storage.Blob.CloudBlob' from assembly 'Microsoft.WindowsAzure.Storage, Version=4.3.0.0, Culture=neutral, 
    ```

    <span data-ttu-id="82742-422">Ha a 4.3.0 használható Azure Storage csomag verziója, távolítsa el a meglévő hivatkozást verzió > 4.3.0 az Azure Storage-csomaghoz.</span><span class="sxs-lookup"><span data-stu-id="82742-422">If you can use the 4.3.0 version of Azure Storage package, remove the existing reference to Azure Storage package of version > 4.3.0.</span></span> <span data-ttu-id="82742-423">Ezután futtassa a következő parancsot a NuGet-Csomagkezelő konzolról.</span><span class="sxs-lookup"><span data-stu-id="82742-423">Then, run the following command from NuGet Package Manager Console.</span></span> 

    ```PowerShell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    <span data-ttu-id="82742-424">A projekt felépítése.</span><span class="sxs-lookup"><span data-stu-id="82742-424">Build the project.</span></span> <span data-ttu-id="82742-425">A verzió > 4.3.0 Azure.Storage szerelvény a bin\Debug mappa törlése.</span><span class="sxs-lookup"><span data-stu-id="82742-425">Delete Azure.Storage assembly of version > 4.3.0 from the bin\Debug folder.</span></span> <span data-ttu-id="82742-426">Hozzon létre egy zip-fájl bináris fájljait és a PDB-fájl.</span><span class="sxs-lookup"><span data-stu-id="82742-426">Create a zip file with binaries and the PDB file.</span></span> <span data-ttu-id="82742-427">Lecseréli a régi zip-fájl erre a blob-tárolóban (customactivitycontainer).</span><span class="sxs-lookup"><span data-stu-id="82742-427">Replace the old zip file with this one in the blob container (customactivitycontainer).</span></span> <span data-ttu-id="82742-428">Futtassa újból a szeletet, melyeknél nem sikerült (kattintson a jobb gombbal a szelet, majd kattintson a Futtatás).</span><span class="sxs-lookup"><span data-stu-id="82742-428">Rerun the slices that failed (right-click slice, and click Run).</span></span>   
8. <span data-ttu-id="82742-429">Az egyéni tevékenység nem használja a **app.config** fájlt a csomagból.</span><span class="sxs-lookup"><span data-stu-id="82742-429">The custom activity does not use the **app.config** file from your package.</span></span> <span data-ttu-id="82742-430">Ezért ha a kódot a kapcsolati karakterláncok a konfigurációs fájlból olvassa be, nem működik futásidőben.</span><span class="sxs-lookup"><span data-stu-id="82742-430">Therefore, if your code reads any connection strings from the configuration file, it does not work at runtime.</span></span> <span data-ttu-id="82742-431">Az ajánlott eljárás az Azure Batch használata esetén a titkos kulcsainak tárolásához egy **Azure KeyVault**, egyszerű tanúsítvány-alapú szolgáltatás használatával megvédheti a **keyvault**, és az Azure Batch tanúsítvány terjesztése készlet.</span><span class="sxs-lookup"><span data-stu-id="82742-431">The best practice when using Azure Batch is to hold any secrets in an **Azure KeyVault**, use a certificate-based service principal to protect the **keyvault**, and distribute the certificate to Azure Batch pool.</span></span> <span data-ttu-id="82742-432">A .NET egyéni tevékenysége ezután elérheti a titkos kulcsokat a kulcstartóból a futtatáskor.</span><span class="sxs-lookup"><span data-stu-id="82742-432">The .NET custom activity then can access secrets from the KeyVault at runtime.</span></span> <span data-ttu-id="82742-433">Ez a megoldás egy általános megoldás, és minden olyan titkos kulcsot, nem csak a kapcsolati karakterlánc típusú méretezheti.</span><span class="sxs-lookup"><span data-stu-id="82742-433">This solution is a generic solution and can scale to any type of secret, not just connection string.</span></span>

   <span data-ttu-id="82742-434">Van egy egyszerűbb megoldást (de nem ajánlott): hozhat létre egy **Azure SQL társított szolgáltatásnak** kapcsolatikarakterlánc-beállításokat, hozzon létre egy adatkészlet által használt társított szolgáltatás, valamint az adatkészlet láncolt, egy üres bemeneti adatkészlet a Egyéni .NET tevékenység.</span><span class="sxs-lookup"><span data-stu-id="82742-434">There is an easier workaround (but not a best practice): you can create an **Azure SQL linked service** with connection string settings, create a dataset that uses the linked service, and chain the dataset as a dummy input dataset to the custom .NET activity.</span></span> <span data-ttu-id="82742-435">A társított szolgáltatás kapcsolati karakterlánc az egyéni tevékenység kódban érheti el.</span><span class="sxs-lookup"><span data-stu-id="82742-435">You can then access the linked service's connection string in the custom activity code.</span></span>  

## <a name="update-custom-activity"></a><span data-ttu-id="82742-436">Egyéni tevékenység frissítése</span><span class="sxs-lookup"><span data-stu-id="82742-436">Update custom activity</span></span>
<span data-ttu-id="82742-437">Ha frissíti a kódot az egyéni tevékenység, összeállítani, és a blob Storage új bináris fájlokat tartalmazó zip-fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="82742-437">If you update the code for the custom activity, build it, and upload the zip file that contains new binaries to the blob storage.</span></span>

## <a name="appdomain-isolation"></a><span data-ttu-id="82742-438">Az alkalmazástartomány elkülönítési</span><span class="sxs-lookup"><span data-stu-id="82742-438">Appdomain isolation</span></span>
<span data-ttu-id="82742-439">Lásd: [Cross AppDomain minta](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) , amely bemutatja, hogyan hozzon létre egy egyéni tevékenységet, amely nem korlátozza a Data Factory indítója által használt szerelvény verzióra történő váltás (Példa: windowsazure.Storage kifejezésre v4.3.0, Newtonsoft.Json v6.0.x, stb.).</span><span class="sxs-lookup"><span data-stu-id="82742-439">See [Cross AppDomain Sample](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) that shows you how to create a custom activity that is not constrained to assembly versions used by the Data Factory launcher (example: WindowsAzure.Storage v4.3.0, Newtonsoft.Json v6.0.x, etc.).</span></span>

## <a name="access-extended-properties"></a><span data-ttu-id="82742-440">További tulajdonságok hozzáférés</span><span class="sxs-lookup"><span data-stu-id="82742-440">Access extended properties</span></span>
<span data-ttu-id="82742-441">A tevékenység JSON-ban, a következő mintában látható módon a kiterjesztett tulajdonságok deklarálhatnak:</span><span class="sxs-lookup"><span data-stu-id="82742-441">You can declare extended properties in the activity JSON as shown in the following sample:</span></span>

```JSON
"typeProperties": {
  "AssemblyName": "MyDotNetActivity.dll",
  "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
  "PackageLinkedService": "AzureStorageLinkedService",
  "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
  "extendedProperties": {
    "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))",
    "DataFactoryName": "CustomActivityFactory"
  }
},
```


<span data-ttu-id="82742-442">A példában két további tulajdonságainak vannak: **SliceStart** és **DataFactoryName**.</span><span class="sxs-lookup"><span data-stu-id="82742-442">In the example, there are two extended properties: **SliceStart** and **DataFactoryName**.</span></span> <span data-ttu-id="82742-443">SliceStart értékét a SliceStart rendszerváltozó alapul.</span><span class="sxs-lookup"><span data-stu-id="82742-443">The value for SliceStart is based on the SliceStart system variable.</span></span> <span data-ttu-id="82742-444">Lásd: [rendszerváltozók](data-factory-functions-variables.md) támogatott rendszerváltozók listáját.</span><span class="sxs-lookup"><span data-stu-id="82742-444">See [System Variables](data-factory-functions-variables.md) for a list of supported system variables.</span></span> <span data-ttu-id="82742-445">DataFactoryName értéke nem módosítható CustomActivityFactory.</span><span class="sxs-lookup"><span data-stu-id="82742-445">The value for DataFactoryName is hard-coded to CustomActivityFactory.</span></span>

<span data-ttu-id="82742-446">További tulajdonságok a eléréséhez a **Execute** metódust, az alábbi kód hasonló használható kódot:</span><span class="sxs-lookup"><span data-stu-id="82742-446">To access these extended properties in the **Execute** method, use code similar to the following code:</span></span>

```csharp
// to get extended properties (for example: SliceStart)
DotNetActivity dotNetActivity = (DotNetActivity)activity.TypeProperties;
string sliceStartString = dotNetActivity.ExtendedProperties["SliceStart"];

// to log all extended properties                               
IDictionary<string, string> extendedProperties = dotNetActivity.ExtendedProperties;
logger.Write("Logging extended properties if any...");
foreach (KeyValuePair<string, string> entry in extendedProperties)
{
    logger.Write("<key:{0}> <value:{1}>", entry.Key, entry.Value);
}
```

## <a name="auto-scaling-of-azure-batch"></a><span data-ttu-id="82742-447">Az Azure Batch automatikus skálázás</span><span class="sxs-lookup"><span data-stu-id="82742-447">Auto-scaling of Azure Batch</span></span>
<span data-ttu-id="82742-448">Az Azure Batch-készlet is létrehozhat **automatikus skálázás** szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="82742-448">You can also create an Azure Batch pool with **autoscale** feature.</span></span> <span data-ttu-id="82742-449">Például létrehozhatja az azure batch-készlet 0 dedikált virtuális gépek és az automatikus skálázás képlet függőben lévő feladatok száma alapján.</span><span class="sxs-lookup"><span data-stu-id="82742-449">For example, you could create an azure batch pool with 0 dedicated VMs and an autoscale formula based on the number of pending tasks.</span></span> 

<span data-ttu-id="82742-450">A minta képlet itt éri el a következő viselkedés: a készlet létrehozásakor 1 virtuális gép kezdődik.</span><span class="sxs-lookup"><span data-stu-id="82742-450">The sample formula here achieves the following behavior: When the pool is initially created, it starts with 1 VM.</span></span> <span data-ttu-id="82742-451">$PendingTasks metrika feladatok száma definiálja a futó + (aszinkron) aktív állapotban.</span><span class="sxs-lookup"><span data-stu-id="82742-451">$PendingTasks metric defines the number of tasks in running + active (queued) state.</span></span>  <span data-ttu-id="82742-452">A képlet átlagos száma függőben lévő feladatok megkeresi az elmúlt 180 másodperc alatt, és ennek megfelelően állítja be TargetDedicated.</span><span class="sxs-lookup"><span data-stu-id="82742-452">The formula finds the average number of pending tasks in the last 180 seconds and sets TargetDedicated accordingly.</span></span> <span data-ttu-id="82742-453">Biztosítja, hogy TargetDedicated soha nem túllép 25 virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="82742-453">It ensures that TargetDedicated never goes beyond 25 VMs.</span></span> <span data-ttu-id="82742-454">Igen új feladatok nyújtják, készlet automatikusan növekszik és feladatok befejezését, mint a virtuális gépek válik a szabad egyenként és az automatikus skálázás zsugorítja a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="82742-454">So, as new tasks are submitted, pool automatically grows and as tasks complete, VMs become free one by one and the autoscaling shrinks those VMs.</span></span> <span data-ttu-id="82742-455">startingNumberOfVMs és maxNumberofVMs az igényeinek megfelelően kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="82742-455">startingNumberOfVMs and maxNumberofVMs can be adjusted to your needs.</span></span>

<span data-ttu-id="82742-456">Automatikus skálázás képlet:</span><span class="sxs-lookup"><span data-stu-id="82742-456">Autoscale formula:</span></span>

``` 
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
```

<span data-ttu-id="82742-457">Lásd: [automatikus méretezési számítási csomópontok az Azure Batch-készlet](../batch/batch-automatic-scaling.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="82742-457">See [Automatically scale compute nodes in an Azure Batch pool](../batch/batch-automatic-scaling.md) for details.</span></span>

<span data-ttu-id="82742-458">Ha az alkalmazáskészlet által használt alapértelmezett [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), a Batch szolgáltatás a virtuális gép előkészítése az egyéni tevékenység futtatása előtt 15 – 30 percet vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="82742-458">If the pool is using the default [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), the Batch service could take 15-30 minutes to prepare the VM before running the custom activity.</span></span>  <span data-ttu-id="82742-459">Ha a készlet egy másik autoScaleEvaluationInterval használ, a Batch szolgáltatás autoScaleEvaluationInterval + 10 percet is beletelhet.</span><span class="sxs-lookup"><span data-stu-id="82742-459">If the pool is using a different autoScaleEvaluationInterval, the Batch service could take autoScaleEvaluationInterval + 10 minutes.</span></span>

## <a name="use-hdinsight-compute-service"></a><span data-ttu-id="82742-460">HDInsight számítási szolgáltatás használata</span><span class="sxs-lookup"><span data-stu-id="82742-460">Use HDInsight compute service</span></span>
<span data-ttu-id="82742-461">A forgatókönyv Azure Batch számítási az egyéni tevékenység futtatásához használt.</span><span class="sxs-lookup"><span data-stu-id="82742-461">In the walkthrough, you used Azure Batch compute to run the custom activity.</span></span> <span data-ttu-id="82742-462">Is használhatja a saját Windows-alapú HDInsight-fürt vagy adat-előállító igény szerinti Windows-alapú HDInsight-fürtöt, és az egyéni tevékenység, futtassa a HDInsight-fürt rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="82742-462">You can also use your own Windows-based HDInsight cluster or have Data Factory create an on-demand Windows-based HDInsight cluster and have the custom activity run on the HDInsight cluster.</span></span> <span data-ttu-id="82742-463">Az alábbiakban a magas szintű lépései a HDInsight-fürtöt használ.</span><span class="sxs-lookup"><span data-stu-id="82742-463">Here are the high-level steps for using an HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="82742-464">Az egyéni .NET-tevékenységek futtatásához csak a Windows-alapú HDInsight-fürtökön.</span><span class="sxs-lookup"><span data-stu-id="82742-464">The custom .NET activities run only on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="82742-465">Ez a korlátozás a megoldás, hogy a térkép csökkentése tevékenység használja egyéni Java-kódot futtathatnak egy Linux-alapú HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="82742-465">A workaround for this limitation is to use the Map Reduce Activity to run custom Java code on a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="82742-466">Egy másik lehetőség, hogy a virtuális gépek Azure Batch-készlet használja egy HDInsight-fürt használata helyett egyéni tevékenységek futtatásához.</span><span class="sxs-lookup"><span data-stu-id="82742-466">Another option is to use an Azure Batch pool of VMs to run custom activities instead of using a HDInsight cluster.</span></span>
 

1. <span data-ttu-id="82742-467">Hozzon létre egy Azure HDInsight társított szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="82742-467">Create an Azure HDInsight linked service.</span></span>   
2. <span data-ttu-id="82742-468">Használja a HDInsight társított szolgáltatás helyett **AzureBatchLinkedService** az adatcsatorna JSON-NÁ.</span><span class="sxs-lookup"><span data-stu-id="82742-468">Use HDInsight linked service in place of **AzureBatchLinkedService** in the pipeline JSON.</span></span>

<span data-ttu-id="82742-469">Ha azt szeretné, akkor a forgatókönyv teszteléséhez, módosítsa **start** és **end** alkalommal fordult elő a tölcsér, hogy az Azure HDInsight szolgáltatással a forgatókönyv teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="82742-469">If you want to test it with the walkthrough, change **start** and **end** times for the pipeline so that you can test the scenario with the Azure HDInsight service.</span></span>

#### <a name="create-azure-hdinsight-linked-service"></a><span data-ttu-id="82742-470">Azure HDInsight társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="82742-470">Create Azure HDInsight linked service</span></span>
<span data-ttu-id="82742-471">Az Azure Data Factory szolgáltatásnak az igény szerinti fürt létrehozását támogatja, és használja úgy eredményezett kimeneti adatokat bemenet feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="82742-471">The Azure Data Factory service supports creation of an on-demand cluster and use it to process input to produce output data.</span></span> <span data-ttu-id="82742-472">Használhatja a saját fürt azonos végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="82742-472">You can also use your own cluster to perform the same.</span></span> <span data-ttu-id="82742-473">Igény szerinti HDInsight-fürt használata esetén a fürt minden egyes szeletre vonatkozó végrehajtásakor létrejön.</span><span class="sxs-lookup"><span data-stu-id="82742-473">When you use on-demand HDInsight cluster, a cluster gets created for each slice.</span></span> <span data-ttu-id="82742-474">Mivel a saját HDInsight-fürtöt használ, ha a szelet feldolgozása azonnal készen áll a fürt.</span><span class="sxs-lookup"><span data-stu-id="82742-474">Whereas, if you use your own HDInsight cluster, the cluster is ready to process the slice immediately.</span></span> <span data-ttu-id="82742-475">Ezért igény-fürtöt használ, akkor előfordulhat, hogy nem jelennek meg kimeneti adatokat leggyorsabban saját fürt használata esetén.</span><span class="sxs-lookup"><span data-stu-id="82742-475">Therefore, when you use on-demand cluster, you may not see the output data as quickly as when you use your own cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="82742-476">Futásidőben a .NET tevékenység példánya fut. csak a HDInsight fürt; egy munkavégző csomópont több csomópont futtathatnak nem lehet méretezni.</span><span class="sxs-lookup"><span data-stu-id="82742-476">At runtime, an instance of a .NET activity runs only on one worker node in the HDInsight cluster; it cannot be scaled to run on multiple nodes.</span></span> <span data-ttu-id="82742-477">A HDInsight-fürt csomópontjai párhuzamosan .NET tevékenység több példánya is futtathatók.</span><span class="sxs-lookup"><span data-stu-id="82742-477">Multiple instances of .NET activity can run in parallel on different nodes of the HDInsight cluster.</span></span>
>
>

##### <a name="to-use-an-on-demand-hdinsight-cluster"></a><span data-ttu-id="82742-478">Igény szerinti HDInsight-fürt használatára</span><span class="sxs-lookup"><span data-stu-id="82742-478">To use an on-demand HDInsight cluster</span></span>
1. <span data-ttu-id="82742-479">Az a **Azure-portálon**, kattintson a **fejlesztés és üzembe helyezés** a Data Factory kezdőlapján.</span><span class="sxs-lookup"><span data-stu-id="82742-479">In the **Azure portal**, click **Author and Deploy** in the Data Factory home page.</span></span>
2. <span data-ttu-id="82742-480">Kattintson a Data Factory Editor **új számítási** a parancssávon, és válassza ki a **igény szerinti HDInsight-fürt** a menüből.</span><span class="sxs-lookup"><span data-stu-id="82742-480">In the Data Factory Editor, click **New compute** from the command bar and select **On-demand HDInsight cluster** from the menu.</span></span>
3. <span data-ttu-id="82742-481">A következő módosításokat a JSON-parancsfájlba:</span><span class="sxs-lookup"><span data-stu-id="82742-481">Make the following changes to the JSON script:</span></span>

   1. <span data-ttu-id="82742-482">Az a **nagyobbnak** tulajdonság, adja meg a HDInsight-fürt méretét.</span><span class="sxs-lookup"><span data-stu-id="82742-482">For the **clusterSize** property, specify the size of the HDInsight cluster.</span></span>
   2. <span data-ttu-id="82742-483">Az a **timeToLive** tulajdonság, adja meg, mennyi ideig az ügyfél üresjáratban lehet, mielőtt az törlődni fog.</span><span class="sxs-lookup"><span data-stu-id="82742-483">For the **timeToLive** property, specify how long the customer can be idle before it is deleted.</span></span>
   3. <span data-ttu-id="82742-484">Az a **verzió** tulajdonság, adja meg a használni kívánt HDInsight-verzió.</span><span class="sxs-lookup"><span data-stu-id="82742-484">For the **version** property, specify the HDInsight version you want to use.</span></span> <span data-ttu-id="82742-485">Ha kizárja a ezt a tulajdonságot, a legújabb verziót használja.</span><span class="sxs-lookup"><span data-stu-id="82742-485">If you exclude this property, the latest version is used.</span></span>  
   4. <span data-ttu-id="82742-486">Az a **linkedServiceName**, adja meg **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="82742-486">For the **linkedServiceName**, specify **AzureStorageLinkedService**.</span></span>

        ```JSON
        {
           "name": "HDInsightOnDemandLinkedService",
           "properties": {
               "type": "HDInsightOnDemand",
               "typeProperties": {
                   "clusterSize": 4,
                   "timeToLive": "00:05:00",
                   "osType": "Windows",
                   "linkedServiceName": "AzureStorageLinkedService",
               }
           }
        }
        ```

    > [!IMPORTANT]
    > <span data-ttu-id="82742-487">Az egyéni .NET-tevékenységek futtatásához csak a Windows-alapú HDInsight-fürtökön.</span><span class="sxs-lookup"><span data-stu-id="82742-487">The custom .NET activities run only on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="82742-488">Ez a korlátozás a megoldás, hogy a térkép csökkentése tevékenység használja egyéni Java-kódot futtathatnak egy Linux-alapú HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="82742-488">A workaround for this limitation is to use the Map Reduce Activity to run custom Java code on a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="82742-489">Egy másik lehetőség, hogy a virtuális gépek Azure Batch-készlet használja egy HDInsight-fürt használata helyett egyéni tevékenységek futtatásához.</span><span class="sxs-lookup"><span data-stu-id="82742-489">Another option is to use an Azure Batch pool of VMs to run custom activities instead of using a HDInsight cluster.</span></span>

4. <span data-ttu-id="82742-490">A társított szolgáltatás üzembe helyezéséhez kattintson a parancssáv **Deploy** (Üzembe helyezés) elemére.</span><span class="sxs-lookup"><span data-stu-id="82742-490">Click **Deploy** on the command bar to deploy the linked service.</span></span>

##### <a name="to-use-your-own-hdinsight-cluster"></a><span data-ttu-id="82742-491">A saját HDInsight-fürt használatára:</span><span class="sxs-lookup"><span data-stu-id="82742-491">To use your own HDInsight cluster:</span></span>
1. <span data-ttu-id="82742-492">Az a **Azure-portálon**, kattintson a **fejlesztés és üzembe helyezés** a Data Factory kezdőlapján.</span><span class="sxs-lookup"><span data-stu-id="82742-492">In the **Azure portal**, click **Author and Deploy** in the Data Factory home page.</span></span>
2. <span data-ttu-id="82742-493">Az a **Data Factory Editor**, kattintson a **új számítási** a parancssávon, és válassza ki a **HDInsight-fürt** a menüből.</span><span class="sxs-lookup"><span data-stu-id="82742-493">In the **Data Factory Editor**, click **New compute** from the command bar and select **HDInsight cluster** from the menu.</span></span>
3. <span data-ttu-id="82742-494">A következő módosításokat a JSON-parancsfájlba:</span><span class="sxs-lookup"><span data-stu-id="82742-494">Make the following changes to the JSON script:</span></span>

   1. <span data-ttu-id="82742-495">Az a **clusterUri** tulajdonság, a HDInsight meg az URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="82742-495">For the **clusterUri** property, enter the URL for your HDInsight.</span></span> <span data-ttu-id="82742-496">Például: https://<clustername>.azurehdinsight.net/</span><span class="sxs-lookup"><span data-stu-id="82742-496">For example: https://<clustername>.azurehdinsight.net/</span></span>     
   2. <span data-ttu-id="82742-497">Az a **felhasználónév** tulajdonság, írja be a felhasználónevet, aki hozzáférhet a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="82742-497">For the **UserName** property, enter the user name who has access to the HDInsight cluster.</span></span>
   3. <span data-ttu-id="82742-498">Az a **jelszó** tulajdonság, írja be a felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="82742-498">For the **Password** property, enter the password for the user.</span></span>
   4. <span data-ttu-id="82742-499">Az a **LinkedServiceName** tulajdonság, adja meg **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="82742-499">For the **LinkedServiceName** property, enter **AzureStorageLinkedService**.</span></span>
4. <span data-ttu-id="82742-500">A társított szolgáltatás üzembe helyezéséhez kattintson a parancssáv **Deploy** (Üzembe helyezés) elemére.</span><span class="sxs-lookup"><span data-stu-id="82742-500">Click **Deploy** on the command bar to deploy the linked service.</span></span>

<span data-ttu-id="82742-501">Lásd: [összekapcsolt szolgáltatások számítási](data-factory-compute-linked-services.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="82742-501">See [Compute linked services](data-factory-compute-linked-services.md) for details.</span></span>

<span data-ttu-id="82742-502">Az a **JSON-feldolgozási folyamat**, HDInsight használata (igény vagy a saját) társított szolgáltatáshoz:</span><span class="sxs-lookup"><span data-stu-id="82742-502">In the **pipeline JSON**, use HDInsight (on-demand or your own) linked service:</span></span>

```JSON
{
  "name": "ADFTutorialPipelineCustom",
  "properties": {
    "description": "Use custom activity",
    "activities": [
      {
        "Name": "MyDotNetActivity",
        "Type": "DotNetActivity",
        "Inputs": [
          {
            "Name": "InputDataset"
          }
        ],
        "Outputs": [
          {
            "Name": "OutputDataset"
          }
        ],
        "LinkedServiceName": "HDInsightOnDemandLinkedService",
        "typeProperties": {
          "AssemblyName": "MyDotNetActivity.dll",
          "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
          "PackageLinkedService": "AzureStorageLinkedService",
          "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
          "extendedProperties": {
            "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
          }
        },
        "Policy": {
          "Concurrency": 2,
          "ExecutionPriorityOrder": "OldestFirst",
          "Retry": 3,
          "Timeout": "00:30:00",
          "Delay": "00:00:00"
        }
      }
    ],
    "start": "2016-11-16T00:00:00Z",
    "end": "2016-11-16T05:00:00Z",
    "isPaused": false
  }
}
```

## <a name="create-a-custom-activity-by-using-net-sdk"></a><span data-ttu-id="82742-503">Egyéni tevékenység létrehozása .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="82742-503">Create a custom activity by using .NET SDK</span></span>
<span data-ttu-id="82742-504">A forgatókönyv ebben a cikkben akkor hozzon létre egy adat-előállító egy folyamatot, amely az egyéni tevékenység használ az Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="82742-504">In the walkthrough in this article, you create a data factory with a pipeline that uses the custom activity by using the Azure portal.</span></span> <span data-ttu-id="82742-505">A következő kód bemutatja, hogyan helyette .NET SDK használatával a data factory létrehozása.</span><span class="sxs-lookup"><span data-stu-id="82742-505">The following code shows you how to create the data factory by using .NET SDK instead.</span></span> <span data-ttu-id="82742-506">Adatcsatornák programozott módon létrehozásához SDK használatával kapcsolatos további részleteket megtalálja a [.NET API használatával hozzon létre egy folyamatot másolási tevékenység](data-factory-copy-activity-tutorial-using-dotnet-api.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="82742-506">You can find more details about using SDK to programmatically create pipelines in the [create a pipeline with copy activity by using .NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md) article.</span></span> 

```csharp
using System;
using System.Configuration;
using System.Collections.ObjectModel;
using System.Threading;
using System.Threading.Tasks;

using Microsoft.Azure;
using Microsoft.Azure.Management.DataFactories;
using Microsoft.Azure.Management.DataFactories.Models;
using Microsoft.Azure.Management.DataFactories.Common.Models;

using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System.Collections.Generic;

namespace DataFactoryAPITestApp
{
    class Program
    {
        static void Main(string[] args)
        {
            // create data factory management client

            // TODO: replace ADFTutorialResourceGroup with the name of your resource group.
            string resourceGroupName = "ADFTutorialResourceGroup";

            // TODO: replace APITutorialFactory with a name that is globally unique. For example: APITutorialFactory04212017
            string dataFactoryName = "APITutorialFactory";

            TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
                ConfigurationManager.AppSettings["SubscriptionId"],
                GetAuthorizationHeader().Result);

            Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

            DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);

            Console.WriteLine("Creating a data factory");
            client.DataFactories.CreateOrUpdate(resourceGroupName,
                new DataFactoryCreateOrUpdateParameters()
                {
                    DataFactory = new DataFactory()
                    {
                        Name = dataFactoryName,
                        Location = "westus",
                        Properties = new DataFactoryProperties()
                    }
                }
            );

            // create a linked service for input data store: Azure Storage
            Console.WriteLine("Creating Azure Storage linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureStorageLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            // TODO: Replace <accountname> and <accountkey> with name and key of your Azure Storage account.
                            new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>")
                        )
                    }
                }
            );

            // create a linked service for output data store: Azure SQL Database
            Console.WriteLine("Creating Azure Batch linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureBatchLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            // TODO: replace <batchaccountname> and <yourbatchaccountkey> with name and key of your Azure Batch account
                            new AzureBatchLinkedService("<batchaccountname>", "https://westus.batch.azure.com", "<yourbatchaccountkey>", "myazurebatchpool", "AzureStorageLinkedService")
                        )
                    }
                }
            );

            // create input and output datasets
            Console.WriteLine("Creating input and output datasets");
            string Dataset_Source = "InputDataset";
            string Dataset_Destination = "OutputDataset";

            Console.WriteLine("Creating input dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,

                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Source,
                        Properties = new DatasetProperties()
                        {
                            LinkedServiceName = "AzureStorageLinkedService",
                            TypeProperties = new AzureBlobDataset()
                            {
                                FolderPath = "adftutorial/customactivityinput/",
                                Format = new TextFormat()
                            },
                            External = true,
                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },

                            Policy = new Policy() { }
                        }
                    }
                });

            Console.WriteLine("Creating output dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Destination,
                        Properties = new DatasetProperties()
                        {
                            LinkedServiceName = "AzureStorageLinkedService",
                            TypeProperties = new AzureBlobDataset()
                            {
                                FileName = "{slice}.txt",
                                FolderPath = "adftutorial/customactivityoutput/",
                                PartitionedBy = new List<Partition>()
                                {
                                    new Partition()
                                    {
                                        Name = "slice",
                                        Value = new DateTimePartitionValue()
                                        {
                                            Date = "SliceStart",
                                            Format = "yyyy-MM-dd-HH"
                                        }
                                    }
                                }
                            },
                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },
                        }
                    }
                });

            Console.WriteLine("Creating a custom activity pipeline");
            DateTime PipelineActivePeriodStartTime = new DateTime(2017, 3, 9, 0, 0, 0, 0, DateTimeKind.Utc);
            DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
            string PipelineName = "ADFTutorialPipelineCustom";

            client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new PipelineCreateOrUpdateParameters()
                {
                    Pipeline = new Pipeline()
                    {
                        Name = PipelineName,
                        Properties = new PipelineProperties()
                        {
                            Description = "Use custom activity",

                            // Initial value for pipeline's active period. With this, you won't need to set slice status
                            Start = PipelineActivePeriodStartTime,
                            End = PipelineActivePeriodEndTime,
                            IsPaused = false,

                            Activities = new List<Activity>()
                            {
                                new Activity()
                                {
                                    Name = "MyDotNetActivity",
                                    Inputs = new List<ActivityInput>()
                                    {
                                        new ActivityInput() {
                                            Name = Dataset_Source
                                        }
                                    },
                                    Outputs = new List<ActivityOutput>()
                                    {
                                        new ActivityOutput()
                                        {
                                            Name = Dataset_Destination
                                        }
                                    },
                                    LinkedServiceName = "AzureBatchLinkedService",
                                    TypeProperties = new DotNetActivity()
                                    {
                                        AssemblyName = "MyDotNetActivity.dll",
                                        EntryPoint = "MyDotNetActivityNS.MyDotNetActivity",
                                        PackageLinkedService = "AzureStorageLinkedService",
                                        PackageFile = "customactivitycontainer/MyDotNetActivity.zip",
                                        ExtendedProperties = new Dictionary<string, string>()
                                        {
                                            { "SliceStart", "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"}
                                        }
                                    },
                                    Policy = new ActivityPolicy()
                                    {
                                        Concurrency = 2,
                                        ExecutionPriorityOrder = "OldestFirst",
                                        Retry = 3,
                                        Timeout = new TimeSpan(0,0,30,0),
                                        Delay = new TimeSpan()
                                    }
                                }
                            }
                        }
                    }
                });
        }

        public static async Task<string> GetAuthorizationHeader()
        {
            AuthenticationContext context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);
            ClientCredential credential = new ClientCredential(
                ConfigurationManager.AppSettings["ApplicationId"],
                ConfigurationManager.AppSettings["Password"]);
            AuthenticationResult result = await context.AcquireTokenAsync(
                resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                clientCredential: credential);

            if (result != null)
                return result.AccessToken;

            throw new InvalidOperationException("Failed to acquire token");
        }
    }
}
```

## <a name="debug-custom-activity-in-visual-studio"></a><span data-ttu-id="82742-507">A Visual Studio egyéni tevékenység hibakeresése</span><span class="sxs-lookup"><span data-stu-id="82742-507">Debug custom activity in Visual Studio</span></span>
<span data-ttu-id="82742-508">A [Azure Data Factory - helyi környezetben](https://github.com/gbrueckl/Azure.DataFactory.LocalEnvironment) mintát a Githubon tartalmaz olyan eszköz, amely lehetővé teszi a Visual Studio .NET tevékenységét egyéni hibakereséséhez.</span><span class="sxs-lookup"><span data-stu-id="82742-508">The [Azure Data Factory - local environment](https://github.com/gbrueckl/Azure.DataFactory.LocalEnvironment) sample on GitHub includes a tool that allows you to debug custom .NET activities within Visual Studio.</span></span>  


## <a name="sample-custom-activities-on-github"></a><span data-ttu-id="82742-509">Egyéni tevékenységek mintát a Githubon</span><span class="sxs-lookup"><span data-stu-id="82742-509">Sample custom activities on GitHub</span></span>
| <span data-ttu-id="82742-510">Minta</span><span class="sxs-lookup"><span data-stu-id="82742-510">Sample</span></span> | <span data-ttu-id="82742-511">Milyen egyéni tevékenység does</span><span class="sxs-lookup"><span data-stu-id="82742-511">What custom activity does</span></span> |
| --- | --- |
| <span data-ttu-id="82742-512">[HTTP-adatok letöltő](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample).</span><span class="sxs-lookup"><span data-stu-id="82742-512">[HTTP Data Downloader](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample).</span></span> |<span data-ttu-id="82742-513">Letölti a adatokat a HTTP-végponttal Azure Blob Storage használatával egyéni tevékenység C# adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="82742-513">Downloads data from an HTTP Endpoint to Azure Blob Storage using custom C# Activity in Data Factory.</span></span> |
| [<span data-ttu-id="82742-514">Twitter véleményeket elemzési minta</span><span class="sxs-lookup"><span data-stu-id="82742-514">Twitter Sentiment Analysis sample</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |<span data-ttu-id="82742-515">Meghívja az Azure ML modellje és do véleményeket elemzése, pontozási, előrejelzés stb.</span><span class="sxs-lookup"><span data-stu-id="82742-515">Invokes an Azure ML model and do sentiment analysis, scoring, prediction etc.</span></span> |
| <span data-ttu-id="82742-516">[R-parancsfájl futtatása](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).</span><span class="sxs-lookup"><span data-stu-id="82742-516">[Run R Script](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).</span></span> |<span data-ttu-id="82742-517">Meghívja az R-parancsfájl RScript.exe futtatásával a HDInsight-fürtre, amelyen már a R telepítve van rajta.</span><span class="sxs-lookup"><span data-stu-id="82742-517">Invokes R script by running RScript.exe on your HDInsight cluster that already has R Installed on it.</span></span> |
| [<span data-ttu-id="82742-518">Kereszt-AppDomain .NET tevékenység</span><span class="sxs-lookup"><span data-stu-id="82742-518">Cross AppDomain .NET Activity</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) |<span data-ttu-id="82742-519">Használja azokat, használja a Data Factory indítója szerelvény különböző verziójú</span><span class="sxs-lookup"><span data-stu-id="82742-519">Uses different assembly versions from ones used by the Data Factory launcher</span></span> |
| [<span data-ttu-id="82742-520">Újból feldolgozza a modellt az Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="82742-520">Reprocess a model in Azure Analysis Services</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/AzureAnalysisServicesProcessSample) |  <span data-ttu-id="82742-521">Feldolgozza a modellt az Azure Analysis Servicesben.</span><span class="sxs-lookup"><span data-stu-id="82742-521">Reprocesses a model in Azure Analysis Services.</span></span> |

[batch-net-library]: ../batch/batch-dotnet-get-started.md
[batch-create-account]: ../batch/batch-account-create-portal.md
[batch-technical-overview]: ../batch/batch-technical-overview.md
[batch-get-started]: ../batch/batch-dotnet-get-started.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[data-factory-introduction]: data-factory-introduction.md
[azure-powershell-install]: https://github.com/Azure/azure-sdk-tools/releases


[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456

[new-azure-batch-account]: https://msdn.microsoft.com/library/mt125880.aspx
[new-azure-batch-pool]: https://msdn.microsoft.com/library/mt125936.aspx
[azure-batch-blog]: http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx

[nuget-package]: http://go.microsoft.com/fwlink/?LinkId=517478
[adf-developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[azure-preview-portal]: https://portal.azure.com/

[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[hivewalkthrough]: data-factory-data-transformation-activities.md

[image-data-factory-ouput-from-custom-activity]: ./media/data-factory-use-custom-activities/OutputFilesFromCustomActivity.png

[image-data-factory-download-logs-from-custom-activity]: ./media/data-factory-use-custom-activities/DownloadLogsFromCustomActivity.png
