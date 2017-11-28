---
title: "egyéni tevékenységeket aaaUse egy Azure Data Factory-folyamat"
description: "Megtudhatja, hogyan toocreate egyéni tevékenységeket és az Azure Data Factory-folyamat használja őket."
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
ms.openlocfilehash: 23e33727b2160541ab40938ffd911fdd484b3daa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-custom-activities-in-an-azure-data-factory-pipeline"></a><span data-ttu-id="823a3-103">Egyéni tevékenységek használata Azure Data Factory-folyamatban</span><span class="sxs-lookup"><span data-stu-id="823a3-103">Use custom activities in an Azure Data Factory pipeline</span></span>

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="823a3-104">Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="823a3-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="823a3-105">A Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="823a3-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="823a3-106">MapReduce művelethez</span><span class="sxs-lookup"><span data-stu-id="823a3-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="823a3-107">Hadoop Streamelési tevékenységben</span><span class="sxs-lookup"><span data-stu-id="823a3-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="823a3-108">A Spark-tevékenység</span><span class="sxs-lookup"><span data-stu-id="823a3-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="823a3-109">Machine Learning kötegelt végrehajtási tevékenység</span><span class="sxs-lookup"><span data-stu-id="823a3-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="823a3-110">Machine Learning Update-erőforrástevékenység</span><span class="sxs-lookup"><span data-stu-id="823a3-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="823a3-111">Tárolt eljárási tevékenység</span><span class="sxs-lookup"><span data-stu-id="823a3-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="823a3-112">Data Lake Analytics U-SQL-tevékenység</span><span class="sxs-lookup"><span data-stu-id="823a3-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="823a3-113">.NET egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="823a3-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="823a3-114">Egy Azure Data Factory-folyamathoz használható tevékenységeknek két típusa van.</span><span class="sxs-lookup"><span data-stu-id="823a3-114">There are two types of activities that you can use in an Azure Data Factory pipeline.</span></span>

- <span data-ttu-id="823a3-115">[Adatok mozgása tevékenységek](data-factory-data-movement-activities.md) toomove adatok között [támogatott forrás és a fogadó adattárolókhoz](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="823a3-115">[Data Movement Activities](data-factory-data-movement-activities.md) toomove data between [supported source and sink data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span>
- <span data-ttu-id="823a3-116">[Adatok átalakítása tevékenységek](data-factory-data-transformation-activities.md) tootransform-adatokat, például az Azure HDInsight, az Azure Batch és az Azure Machine Learning szolgáltatás számítási.</span><span class="sxs-lookup"><span data-stu-id="823a3-116">[Data Transformation Activities](data-factory-data-transformation-activities.md) tootransform data using compute services such as Azure HDInsight, Azure Batch, and Azure Machine Learning.</span></span> 

<span data-ttu-id="823a3-117">toomove adatok/egy adattárból, amely nem támogatja a Data Factory, hozzon létre egy **egyéni tevékenység** saját adatok mozgása logika és -felhasználási hello tevékenységet egy folyamaton belül.</span><span class="sxs-lookup"><span data-stu-id="823a3-117">toomove data to/from a data store that Data Factory does not support, create a **custom activity** with your own data movement logic and use hello activity in a pipeline.</span></span> <span data-ttu-id="823a3-118">Hasonlóképpen tootransform/folyamat adatokat úgy, hogy a Data Factory nem támogatja egyéni tevékenység létrehozása a saját adatok átalakítása logikával, és hello tevékenység a folyamat.</span><span class="sxs-lookup"><span data-stu-id="823a3-118">Similarly, tootransform/process data in a way that isn't supported by Data Factory, create a custom activity with your own data transformation logic and use hello activity in a pipeline.</span></span> 

<span data-ttu-id="823a3-119">Egy egyéni tevékenység toorun konfigurálható egy **Azure Batch** készlete virtuális gépek vagy a Windows-alapú **Azure HDInsight** fürt.</span><span class="sxs-lookup"><span data-stu-id="823a3-119">You can configure a custom activity toorun on an **Azure Batch** pool of virtual machines or a Windows-based **Azure HDInsight** cluster.</span></span> <span data-ttu-id="823a3-120">Azure Batch használatakor csak egy meglévő Azure Batch-készlet is használhatja.</span><span class="sxs-lookup"><span data-stu-id="823a3-120">When using Azure Batch, you can use only an existing Azure Batch pool.</span></span> <span data-ttu-id="823a3-121">Mivel a HDInsight használata esetén használható egy meglévő HDInsight-fürtre vagy olyan fürt, amely automatikusan jön létre, igény szerinti futásidőben.</span><span class="sxs-lookup"><span data-stu-id="823a3-121">Whereas, when using HDInsight, you can use an existing HDInsight cluster or a cluster that is automatically created for you on-demand at runtime.</span></span>  

<span data-ttu-id="823a3-122">hello következő forgatókönyv részletesen bemutatja egy egyéni .NET tevékenység létrehozásáért és hello egyéni tevékenység egy folyamaton belül.</span><span class="sxs-lookup"><span data-stu-id="823a3-122">hello following walkthrough provides step-by-step instructions for creating a custom .NET activity and using hello custom activity in a pipeline.</span></span> <span data-ttu-id="823a3-123">hello forgatókönyv egy **Azure Batch** társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="823a3-123">hello walkthrough uses an **Azure Batch** linked service.</span></span> <span data-ttu-id="823a3-124">inkább a társított szolgáltatásnak az Azure HDInsight toouse, típusú társított szolgáltatás létrehozása **HDInsight** (egyéni HDInsight-fürt) vagy **HDInsightOnDemand** (Data Factory létrehoz egy HDInsight-fürt igény szerinti).</span><span class="sxs-lookup"><span data-stu-id="823a3-124">toouse an Azure HDInsight linked service instead, you create a linked service of type **HDInsight** (your own HDInsight cluster) or **HDInsightOnDemand** (Data Factory creates an HDInsight cluster on-demand).</span></span> <span data-ttu-id="823a3-125">Ezt követően konfigurálja az egyéni tevékenység toouse hello HDInsight társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="823a3-125">Then, configure custom activity toouse hello HDInsight linked service.</span></span> <span data-ttu-id="823a3-126">Lásd: [használata Azure HDInsight összekapcsolt szolgáltatások](#use-hdinsight-compute-service) című szakaszban talál információt az Azure HDInsight toorun hello egyéni tevékenység.</span><span class="sxs-lookup"><span data-stu-id="823a3-126">See [Use Azure HDInsight linked services](#use-hdinsight-compute-service) section for details on using Azure HDInsight toorun hello custom activity.</span></span>

> [!IMPORTANT]
> - <span data-ttu-id="823a3-127">hello egyéni .NET tevékenységek futtatásához csak a Windows-alapú HDInsight-fürtökön.</span><span class="sxs-lookup"><span data-stu-id="823a3-127">hello custom .NET activities run only on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="823a3-128">Ez a korlátozás az áthidaló toouse hello térkép csökkentése tevékenység toorun egyéni Java-kód egy Linux-alapú HDInsight-fürtön.</span><span class="sxs-lookup"><span data-stu-id="823a3-128">A workaround for this limitation is toouse hello Map Reduce Activity toorun custom Java code on a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="823a3-129">Egy másik lehetőség a virtuális gépek toorun egyéni tevékenységeket a HDInsight-fürt használata helyett az Azure Batch-készlet toouse.</span><span class="sxs-lookup"><span data-stu-id="823a3-129">Another option is toouse an Azure Batch pool of VMs toorun custom activities instead of using a HDInsight cluster.</span></span>
> - <span data-ttu-id="823a3-130">Már nem lehetséges toouse egy egyéni tevékenység tooaccess a helyszíni adatforrások az adatkezelési átjárót.</span><span class="sxs-lookup"><span data-stu-id="823a3-130">It is not possible toouse a Data Management Gateway from a custom activity tooaccess on-premises data sources.</span></span> <span data-ttu-id="823a3-131">Jelenleg [az adatkezelési átjáró](data-factory-data-management-gateway.md) csak a hello másolási tevékenység és a tárolt eljárási tevékenység támogatja az adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="823a3-131">Currently, [Data Management Gateway](data-factory-data-management-gateway.md) supports only hello copy activity and stored procedure activity in Data Factory.</span></span>   

## <a name="walkthrough-create-a-custom-activity"></a><span data-ttu-id="823a3-132">Forgatókönyv: egyéni tevékenység létrehozása</span><span class="sxs-lookup"><span data-stu-id="823a3-132">Walkthrough: create a custom activity</span></span>
### <a name="prerequisites"></a><span data-ttu-id="823a3-133">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="823a3-133">Prerequisites</span></span>
* <span data-ttu-id="823a3-134">A Visual Studio 2012/2013 vagy 2015</span><span class="sxs-lookup"><span data-stu-id="823a3-134">Visual Studio 2012/2013/2015</span></span>
* <span data-ttu-id="823a3-135">Az [Azure .NET SDK](https://azure.microsoft.com/downloads/) letöltése és telepítése.</span><span class="sxs-lookup"><span data-stu-id="823a3-135">Download and install [Azure .NET SDK](https://azure.microsoft.com/downloads/)</span></span>

### <a name="azure-batch-prerequisites"></a><span data-ttu-id="823a3-136">Azure Batch-Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="823a3-136">Azure Batch prerequisites</span></span>
<span data-ttu-id="823a3-137">Hello forgatókönyv futtatása az egyéni .NET-tevékenységek használata az Azure Batch számítási erőforrásként.</span><span class="sxs-lookup"><span data-stu-id="823a3-137">In hello walkthrough, you run your custom .NET activities using Azure Batch as a compute resource.</span></span> <span data-ttu-id="823a3-138">**Az Azure Batch** van egy, a nagyméretű párhuzamosan futó szolgáltatást és nagy teljesítményű számítástechnikai (HPC) alkalmazások hatékonyan hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="823a3-138">**Azure Batch** is a platform service for running large-scale parallel and high-performance computing (HPC) applications efficiently in hello cloud.</span></span> <span data-ttu-id="823a3-139">Az Azure Batch számítási igényű munkahelyi toorun a egy felügyelt ütemezi **virtuális gépek**, és képes automatikusan méretezési számítási erőforrások toomeet hello igényeinek feladatot.</span><span class="sxs-lookup"><span data-stu-id="823a3-139">Azure Batch schedules compute-intensive work toorun on a managed **collection of virtual machines**, and can automatically scale compute resources toomeet hello needs of your jobs.</span></span> <span data-ttu-id="823a3-140">Lásd: [Azure Batch alapjai] [ batch-technical-overview] hello Azure Batch szolgáltatás részletes áttekintés cikkében.</span><span class="sxs-lookup"><span data-stu-id="823a3-140">See [Azure Batch basics][batch-technical-overview] article for a detailed overview of hello Azure Batch service.</span></span>

<span data-ttu-id="823a3-141">Hello az oktatóanyagban az Azure Batch-fiók létrehozása virtuális gépek készletét.</span><span class="sxs-lookup"><span data-stu-id="823a3-141">For hello tutorial, create an Azure Batch account with a pool of VMs.</span></span> <span data-ttu-id="823a3-142">Az alábbiakban hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="823a3-142">Here are hello steps:</span></span>

1. <span data-ttu-id="823a3-143">Hozzon létre egy **Azure Batch-fiók** hello segítségével [Azure-portálon](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="823a3-143">Create an **Azure Batch account** using hello [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="823a3-144">Lásd: [létrehozása és kezelése az Azure Batch-fiók] [ batch-create-account] miként.</span><span class="sxs-lookup"><span data-stu-id="823a3-144">See [Create and manage an Azure Batch account][batch-create-account] article for instructions.</span></span>
2. <span data-ttu-id="823a3-145">Jegyezze fel hello Azure Batch-fiók neve, a fiókkulcsot, URI és az alkalmazáskészlet neve.</span><span class="sxs-lookup"><span data-stu-id="823a3-145">Note down hello Azure Batch account name, account key, URI, and pool name.</span></span> <span data-ttu-id="823a3-146">Egy csatolt Azure Batch szolgáltatás toocreate kell őket.</span><span class="sxs-lookup"><span data-stu-id="823a3-146">You need them toocreate an Azure Batch linked service.</span></span>
    1. <span data-ttu-id="823a3-147">Kezdőlapon hello Azure Batch-fiókhoz, megjelenik egy **URL-cím** a formátum a következő hello: `https://myaccount.westus.batch.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="823a3-147">On hello home page for Azure Batch account, you see a **URL** in hello following format: `https://myaccount.westus.batch.azure.com`.</span></span> <span data-ttu-id="823a3-148">Ebben a példában **myaccount** hello hello Azure Batch-fiók neve.</span><span class="sxs-lookup"><span data-stu-id="823a3-148">In this example, **myaccount** is hello name of hello Azure Batch account.</span></span> <span data-ttu-id="823a3-149">Használhatja a kapcsolódó hello szolgáltatásdefiníció URI megadása hello URL-cím hello fiók hello név nélkül.</span><span class="sxs-lookup"><span data-stu-id="823a3-149">URI you use in hello linked service definition is hello URL without hello name of hello account.</span></span> <span data-ttu-id="823a3-150">Például: `https://<region>.batch.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="823a3-150">For example: `https://<region>.batch.azure.com`.</span></span>
    2. <span data-ttu-id="823a3-151">Kattintson a **kulcsok** hello bal oldali menüben, és az másolási hello **elsődleges ELÉRÉSI kulcsot**.</span><span class="sxs-lookup"><span data-stu-id="823a3-151">Click **Keys** on hello left menu, and copy hello **PRIMARY ACCESS KEY**.</span></span>
    3. <span data-ttu-id="823a3-152">Kattintson egy meglévő készlet toouse **készletek** hello menüben, és jegyezze fel a hello **azonosító** hello készlet.</span><span class="sxs-lookup"><span data-stu-id="823a3-152">toouse an existing pool, click **Pools** on hello menu, and note down hello **ID** of hello pool.</span></span> <span data-ttu-id="823a3-153">Ha egy meglévő készlet nem rendelkezik, helyezze át a toohello következő lépésre.</span><span class="sxs-lookup"><span data-stu-id="823a3-153">If you don't have an existing pool, move toohello next step.</span></span>     
2. <span data-ttu-id="823a3-154">Hozzon létre egy **Azure Batch-készlet**.</span><span class="sxs-lookup"><span data-stu-id="823a3-154">Create an **Azure Batch pool**.</span></span>

   1. <span data-ttu-id="823a3-155">A hello [Azure-portálon](https://portal.azure.com), kattintson a **Tallózás** a bal oldali menü hello, és kattintson a **Batch-fiókok**.</span><span class="sxs-lookup"><span data-stu-id="823a3-155">In hello [Azure portal](https://portal.azure.com), click **Browse** in hello left menu, and click **Batch Accounts**.</span></span>
   2. <span data-ttu-id="823a3-156">Válassza ki az Azure Batch-fiók tooopen hello **Batch-fiók** panelen.</span><span class="sxs-lookup"><span data-stu-id="823a3-156">Select your Azure Batch account tooopen hello **Batch Account** blade.</span></span>
   3. <span data-ttu-id="823a3-157">Kattintson a **készletek** csempére.</span><span class="sxs-lookup"><span data-stu-id="823a3-157">Click **Pools** tile.</span></span>
   4. <span data-ttu-id="823a3-158">A hello **készletek** panelen hello eszköztár tooadd a készlet hozzáadása gombra.</span><span class="sxs-lookup"><span data-stu-id="823a3-158">In hello **Pools** blade, click Add button on hello toolbar tooadd a pool.</span></span>
      1. <span data-ttu-id="823a3-159">Adja meg az azonosító hello csoport (alkalmazáskészlet azonosítója).</span><span class="sxs-lookup"><span data-stu-id="823a3-159">Enter an ID for hello pool (Pool ID).</span></span> <span data-ttu-id="823a3-160">Megjegyzés: hello **hello készlet azonosítója**; amikor hello adat-előállító megoldás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="823a3-160">Note hello **ID of hello pool**; you need it when creating hello Data Factory solution.</span></span>
      2. <span data-ttu-id="823a3-161">Adja meg **Windows Server 2012 R2** hello operációsrendszer-családot beállítás.</span><span class="sxs-lookup"><span data-stu-id="823a3-161">Specify **Windows Server 2012 R2** for hello Operating System Family setting.</span></span>
      3. <span data-ttu-id="823a3-162">Válassza ki a **csomóponti tarifacsomagot**.</span><span class="sxs-lookup"><span data-stu-id="823a3-162">Select a **node pricing tier**.</span></span>
      4. <span data-ttu-id="823a3-163">Adja meg **2** hello értékként **cél dedikált** beállítást.</span><span class="sxs-lookup"><span data-stu-id="823a3-163">Enter **2** as value for hello **Target Dedicated** setting.</span></span>
      5. <span data-ttu-id="823a3-164">Adja meg **2** hello értékként **csomópontonkénti tevékenységek maximális** beállítást.</span><span class="sxs-lookup"><span data-stu-id="823a3-164">Enter **2** as value for hello **Max tasks per node** setting.</span></span>
   5. <span data-ttu-id="823a3-165">Kattintson a **OK** toocreate hello készlet.</span><span class="sxs-lookup"><span data-stu-id="823a3-165">Click **OK** toocreate hello pool.</span></span>
   6. <span data-ttu-id="823a3-166">Jegyezze fel a hello **azonosító** hello készlet.</span><span class="sxs-lookup"><span data-stu-id="823a3-166">Note down hello **ID** of hello pool.</span></span> 



### <a name="high-level-steps"></a><span data-ttu-id="823a3-167">Magas szintű lépései</span><span class="sxs-lookup"><span data-stu-id="823a3-167">High-level steps</span></span>
<span data-ttu-id="823a3-168">Az alábbiakban a két magas szintű lépései hello elvégezhető ez a forgatókönyv részeként:</span><span class="sxs-lookup"><span data-stu-id="823a3-168">Here are hello two high-level steps you perform as part of this walkthrough:</span></span> 

1. <span data-ttu-id="823a3-169">Egyszerű adat-átalakítási/feldolgozó logika tartalmazó egyéni tevékenység létrehozása.</span><span class="sxs-lookup"><span data-stu-id="823a3-169">Create a custom activity that contains simple data transformation/processing logic.</span></span>
2. <span data-ttu-id="823a3-170">Hozzon létre egy Azure data factory egy folyamatot, amely hello egyéni tevékenység használja.</span><span class="sxs-lookup"><span data-stu-id="823a3-170">Create an Azure data factory with a pipeline that uses hello custom activity.</span></span>

### <a name="create-a-custom-activity"></a><span data-ttu-id="823a3-171">Egyéni tevékenység létrehozása</span><span class="sxs-lookup"><span data-stu-id="823a3-171">Create a custom activity</span></span>
<span data-ttu-id="823a3-172">toocreate .NET egyéni tevékenység, hozzon létre egy **.NET Class Library** egy osztály, amely megvalósítja az, hogy a projekt **IDotNetActivity** felületet.</span><span class="sxs-lookup"><span data-stu-id="823a3-172">toocreate a .NET custom activity, create a **.NET Class Library** project with a class that implements that **IDotNetActivity** interface.</span></span> <span data-ttu-id="823a3-173">Ez az interfész csak egy metódusa: [Execute](https://msdn.microsoft.com/library/azure/mt603945.aspx) , és az aláírása:</span><span class="sxs-lookup"><span data-stu-id="823a3-173">This interface has only one method: [Execute](https://msdn.microsoft.com/library/azure/mt603945.aspx) and its signature is:</span></span>

```csharp
public IDictionary<string, string> Execute(
        IEnumerable<LinkedService> linkedServices,
        IEnumerable<Dataset> datasets,
        Activity activity,
        IActivityLogger logger)
```


<span data-ttu-id="823a3-174">hello metódus négy paramétereket fogadja:</span><span class="sxs-lookup"><span data-stu-id="823a3-174">hello method takes four parameters:</span></span>

- <span data-ttu-id="823a3-175">**linkedServices**.</span><span class="sxs-lookup"><span data-stu-id="823a3-175">**linkedServices**.</span></span> <span data-ttu-id="823a3-176">Ez a tulajdonság egy-egy adattároló kapcsolódó szolgáltatások bemeneti/kimeneti adatkészletek hello tevékenység által hivatkozott enumerálható lista.</span><span class="sxs-lookup"><span data-stu-id="823a3-176">This property is an enumerable list of Data Store linked services referenced by input/output datasets for hello activity.</span></span>   
- <span data-ttu-id="823a3-177">**adatkészletek**.</span><span class="sxs-lookup"><span data-stu-id="823a3-177">**datasets**.</span></span> <span data-ttu-id="823a3-178">Ez a tulajdonság egy bemeneti/kimeneti adatkészletek hello tevékenység enumerálható listája.</span><span class="sxs-lookup"><span data-stu-id="823a3-178">This property is an enumerable list of input/output datasets for hello activity.</span></span> <span data-ttu-id="823a3-179">A paraméter tooget hello helyek és -sémákat határozzák meg a bemeneti és kimeneti adathalmazokat is használhatja.</span><span class="sxs-lookup"><span data-stu-id="823a3-179">You can use this parameter tooget hello locations and schemas defined by input and output datasets.</span></span>
- <span data-ttu-id="823a3-180">**tevékenység**.</span><span class="sxs-lookup"><span data-stu-id="823a3-180">**activity**.</span></span> <span data-ttu-id="823a3-181">Ez a tulajdonság aktuális tevékenység hello jelöli.</span><span class="sxs-lookup"><span data-stu-id="823a3-181">This property represents hello current activity.</span></span> <span data-ttu-id="823a3-182">További tulajdonságok hello egyéni tevékenység társított használt tooaccess lehet.</span><span class="sxs-lookup"><span data-stu-id="823a3-182">It can be used tooaccess extended properties associated with hello custom activity.</span></span> <span data-ttu-id="823a3-183">Lásd: [további tulajdonságok hozzáférés](#access-extended-properties) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="823a3-183">See [Access extended properties](#access-extended-properties) for details.</span></span>
- <span data-ttu-id="823a3-184">**naplózó**.</span><span class="sxs-lookup"><span data-stu-id="823a3-184">**logger**.</span></span> <span data-ttu-id="823a3-185">Ez az objektum lehetővé teszi a felhasználó bejelentkezése hello hello adatcsatorna az adott felület hibakeresési megjegyzések írását.</span><span class="sxs-lookup"><span data-stu-id="823a3-185">This object lets you write debug comments that surface in hello user log for hello pipeline.</span></span>

<span data-ttu-id="823a3-186">hello metódus, amely lehet használt toochain egyéni tevékenységek együtt hello jövőben dictionary adja vissza.</span><span class="sxs-lookup"><span data-stu-id="823a3-186">hello method returns a dictionary that can be used toochain custom activities together in hello future.</span></span> <span data-ttu-id="823a3-187">Ez a funkció még nem használható, így egy üres szótár visszaadásának hello metódus.</span><span class="sxs-lookup"><span data-stu-id="823a3-187">This feature is not implemented yet, so return an empty dictionary from hello method.</span></span>  

### <a name="procedure"></a><span data-ttu-id="823a3-188">Eljárás</span><span class="sxs-lookup"><span data-stu-id="823a3-188">Procedure</span></span>
1. <span data-ttu-id="823a3-189">Hozzon létre egy **.NET Class Library** projekt.</span><span class="sxs-lookup"><span data-stu-id="823a3-189">Create a **.NET Class Library** project.</span></span>
   <ol type="a">
     <li><span data-ttu-id="823a3-190">Indítsa el <b>Visual Studio 2017</b> vagy <b>Visual Studio 2015-öt</b> vagy <b>Visual Studio 2013</b> vagy <b>Visual Studio 2012</b>.</span><span class="sxs-lookup"><span data-stu-id="823a3-190">Launch <b>Visual Studio 2017</b> or <b>Visual Studio 2015</b> or <b>Visual Studio 2013</b> or <b>Visual Studio 2012</b>.</span></span></li>
     <li><span data-ttu-id="823a3-191">Kattintson a <b>fájl</b>, pont túl<b>új</b>, és kattintson a <b>projekt</b>.</span><span class="sxs-lookup"><span data-stu-id="823a3-191">Click <b>File</b>, point too<b>New</b>, and click <b>Project</b>.</span></span></li>
     <li><span data-ttu-id="823a3-192">Bontsa ki a <b>Sablonok</b> lehetőséget, és válassza a <b>Visual C#</b> lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="823a3-192">Expand <b>Templates</b>, and select <b>Visual C#</b>.</span></span> <span data-ttu-id="823a3-193">Ebben a bemutatóban használhat C#, de használhatja a .NET nyelvi toodevelop hello egyéni tevékenység.</span><span class="sxs-lookup"><span data-stu-id="823a3-193">In this walkthrough, you use C#, but you can use any .NET language toodevelop hello custom activity.</span></span></li>
     <li><span data-ttu-id="823a3-194">Válassza ki <b>Class Library</b> jobb hello projekttípusok hello listája.</span><span class="sxs-lookup"><span data-stu-id="823a3-194">Select <b>Class Library</b> from hello list of project types on hello right.</span></span> <span data-ttu-id="823a3-195">VS 2017, válassza a <b>Class Library (.NET-keretrendszer)</b> </span><span class="sxs-lookup"><span data-stu-id="823a3-195">In VS 2017, choose <b>Class Library (.NET Framework)</b> </span></span></li>
     <li><span data-ttu-id="823a3-196">Adja meg <b>MyDotNetActivity</b> a hello <b>neve</b>.</span><span class="sxs-lookup"><span data-stu-id="823a3-196">Enter <b>MyDotNetActivity</b> for hello <b>Name</b>.</span></span></li>
     <li><span data-ttu-id="823a3-197">Válassza ki <b>C:\ADFGetStarted</b> a hello <b>hely</b>.</span><span class="sxs-lookup"><span data-stu-id="823a3-197">Select <b>C:\ADFGetStarted</b> for hello <b>Location</b>.</span></span></li>
     <li><span data-ttu-id="823a3-198">Kattintson a <b>OK</b> toocreate hello projekt.</span><span class="sxs-lookup"><span data-stu-id="823a3-198">Click <b>OK</b> toocreate hello project.</span></span></li>
   </ol><span data-ttu-id="823a3-199">
2.Kattintson a **eszközök**, pont túl**NuGet-Csomagkezelő**, és kattintson a **Csomagkezelő konzol**.</span><span class="sxs-lookup"><span data-stu-id="823a3-199">
2. Click **Tools**, point too**NuGet Package Manager**, and click **Package Manager Console**.</span></span>
<span data-ttu-id="823a3-200">3.</span><span class="sxs-lookup"><span data-stu-id="823a3-200">3.</span></span> <span data-ttu-id="823a3-201">A Csomagkezelő konzol hello, hajtsa végre a következő parancs tooimport hello **Microsoft.Azure.Management.DataFactories**.</span><span class="sxs-lookup"><span data-stu-id="823a3-201">In hello Package Manager Console, execute hello following command tooimport **Microsoft.Azure.Management.DataFactories**.</span></span>

    ```PowerShell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. <span data-ttu-id="823a3-202">Importálás hello **Azure Storage** toohello projekt NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="823a3-202">Import hello **Azure Storage** NuGet package in toohello project.</span></span>

    ```PowerShell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="823a3-203">Data Factory szolgáltatás indítója windowsazure.Storage kifejezésre hello 4.3 verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="823a3-203">Data Factory service launcher requires hello 4.3 version of WindowsAzure.Storage.</span></span> <span data-ttu-id="823a3-204">Ha egy hivatkozás tooa az egyéni tevékenység projektben Azure Storage szerelvény újabb verziója, hibaüzenet jelenik meg hello tevékenység végrehajtásakor.</span><span class="sxs-lookup"><span data-stu-id="823a3-204">If you add a reference tooa later version of Azure Storage assembly in your custom activity project, you see an error when hello activity executes.</span></span> <span data-ttu-id="823a3-205">tooresolve hello hiba, lásd: [Appdomain elkülönítési](#appdomain-isolation) szakasz.</span><span class="sxs-lookup"><span data-stu-id="823a3-205">tooresolve hello error, see [Appdomain isolation](#appdomain-isolation) section.</span></span> 
5. <span data-ttu-id="823a3-206">Adja hozzá a következő hello **használatával** utasítások toohello forrásfájl hello projektben.</span><span class="sxs-lookup"><span data-stu-id="823a3-206">Add hello following **using** statements toohello source file in hello project.</span></span>

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
6. <span data-ttu-id="823a3-207">Hello nevének módosítása a hello **névtér** túl**MyDotNetActivityNS**.</span><span class="sxs-lookup"><span data-stu-id="823a3-207">Change hello name of hello **namespace** too**MyDotNetActivityNS**.</span></span>

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. <span data-ttu-id="823a3-208">Hello osztály hello nevének módosítása túl**MyDotNetActivity** , és amelyek hello **IDotNetActivity** csatoló, ahogy az a következő kódrészletet hello:</span><span class="sxs-lookup"><span data-stu-id="823a3-208">Change hello name of hello class too**MyDotNetActivity** and derive it from hello **IDotNetActivity** interface as shown in hello following code snippet:</span></span>

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. <span data-ttu-id="823a3-209">Megvalósítása (Hozzáadás) hello **Execute** hello metódusában **IDotNetActivity** toohello csatoló **MyDotNetActivity** minta kód toohello metódus a következő osztály és példány hello.</span><span class="sxs-lookup"><span data-stu-id="823a3-209">Implement (Add) hello **Execute** method of hello **IDotNetActivity** interface toohello **MyDotNetActivity** class and copy hello following sample code toohello method.</span></span>

    <span data-ttu-id="823a3-210">hello következő minta található hello hello keresési kifejezés ("Microsoft") előfordulásainak száma az adatok szelet társított minden egyes blob.</span><span class="sxs-lookup"><span data-stu-id="823a3-210">hello following sample counts hello number of occurrences of hello search term (“Microsoft”) in each blob associated with a data slice.</span></span>

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
        // get extended properties defined in activity JSON definition
        // (for example: SliceStart)
        DotNetActivity dotNetActivity = (DotNetActivity)activity.TypeProperties;
        string sliceStartString = dotNetActivity.ExtendedProperties["SliceStart"];
    
        // toolog information, use hello logger object
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

        // get hello input dataset
        Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
    
        // declare variables toohold type properties of input/output datasets
        AzureBlobDataset inputTypeProperties, outputTypeProperties;
        
        // get type properties from hello dataset object
        inputTypeProperties = inputDataset.Properties.TypeProperties as AzureBlobDataset;
    
        // log linked services passed in linkedServices parameter
        // you will see two linked services of type: AzureStorage
        // one for input dataset and hello other for output dataset 
        foreach (LinkedService ls in linkedServices)
            logger.Write("linkedService.Name {0}", ls.Name);
    
        // get hello first Azure Storate linked service from linkedServices object
        // using First method instead of Single since we are using hello same
        // Azure Storage linked service for input and output.
        inputLinkedService = linkedServices.First(
            linkedService =>
            linkedService.Name ==
            inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
            as AzureStorageLinkedService;
    
        // get hello connection string in hello linked service
        string connectionString = inputLinkedService.ConnectionString;
    
        // get hello folder path from hello input dataset definition
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
               // with hello data slice. definition of hello method is shown in hello next step.
    
            output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
        } while (continuationToken != null);
    
        // get hello output dataset using hello name of hello dataset matched tooa name in hello Activity output collection.
        Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);

        // get type properties for hello output dataset
        outputTypeProperties = outputDataset.Properties.TypeProperties as AzureBlobDataset;
    
        // get hello folder path from hello output dataset definition
        folderPath = GetFolderPath(outputDataset);

        // log hello output folder path 
        logger.Write("Writing blob toohello folder: {0}", folderPath);
    
        // create a storage object for hello output blob.
        CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
        // write hello name of hello file.
        Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    
        // log hello output file name
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
9. <span data-ttu-id="823a3-211">Adja hozzá a következő segédmódszereket hello:</span><span class="sxs-lookup"><span data-stu-id="823a3-211">Add hello following helper methods:</span></span> 

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

        // get type properties of hello dataset 
        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
        if (blobDataset == null)
        {
            return null;
        }
    
        // return hello folder path found in hello type properties
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
    
        // get type properties of hello dataset
        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
        if (blobDataset == null)
        {
            return null;
        }
    
        // return hello blob/file name in hello type properties
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

    <span data-ttu-id="823a3-212">hello GetFolderPath metódus beolvasása hello elérési toohello mappa hello dataset tooand hello GetFileName metódus értéket ad vissza hello hello blob/fájl nevét, amely a DataSet adatkészlet mutat hello mutat.</span><span class="sxs-lookup"><span data-stu-id="823a3-212">hello GetFolderPath method returns hello path toohello folder that hello dataset points tooand hello GetFileName method returns hello name of hello blob/file that hello dataset points to.</span></span> <span data-ttu-id="823a3-213">Ha Ön havefolderPath meghatározása változókkal például {Year}, {Month}, {Day} stb., hello metódus beolvasása hello karakterlánc, mert az nem törli őket futásidejű értékekkel.</span><span class="sxs-lookup"><span data-stu-id="823a3-213">If you havefolderPath defines using variables such as {Year}, {Month}, {Day} etc., hello method returns hello string as it is without replacing them with runtime values.</span></span> <span data-ttu-id="823a3-214">Lásd: [további tulajdonságok hozzáférés](#access-extended-properties) című szakaszban talál információt elérése SliceStart, SliceEnd, stb.</span><span class="sxs-lookup"><span data-stu-id="823a3-214">See [Access extended properties](#access-extended-properties) section for details on accessing SliceStart, SliceEnd, etc.</span></span>    

    ```JSON
    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "adftutorial/inputfolder/",
    ```

    <span data-ttu-id="823a3-215">hello Calculate metódus kulcsszó Microsoft hello bemeneti fájlok (BLOB hello mappában) a példányok száma hello számítja ki.</span><span class="sxs-lookup"><span data-stu-id="823a3-215">hello Calculate method calculates hello number of instances of keyword Microsoft in hello input files (blobs in hello folder).</span></span> <span data-ttu-id="823a3-216">hello keresési kifejezés ("Microsoft") nem változtatható hello kódban.</span><span class="sxs-lookup"><span data-stu-id="823a3-216">hello search term (“Microsoft”) is hard-coded in hello code.</span></span>
10. <span data-ttu-id="823a3-217">Fordítsa le hello projektet.</span><span class="sxs-lookup"><span data-stu-id="823a3-217">Compile hello project.</span></span> <span data-ttu-id="823a3-218">Kattintson a **Build** hello menüre, majd a **megoldás fordítása**.</span><span class="sxs-lookup"><span data-stu-id="823a3-218">Click **Build** from hello menu and click **Build Solution**.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="823a3-219">Hello célkeretrendszer a projekthez, .NET-keretrendszer 4.5.2-es set verziója: kattintson a jobb gombbal a projekt hello, és kattintson a **tulajdonságok** tooset hello célkeretrendszer.</span><span class="sxs-lookup"><span data-stu-id="823a3-219">Set 4.5.2 version of .NET Framework as hello target framework for your project: right-click hello project, and click **Properties** tooset hello target framework.</span></span> <span data-ttu-id="823a3-220">Adat-előállító nem támogatja a lefordított elleni .NET-keretrendszer verziója 4.5.2 később egyéni tevékenységeket.</span><span class="sxs-lookup"><span data-stu-id="823a3-220">Data Factory does not support custom activities compiled against .NET Framework versions later than 4.5.2.</span></span>

11. <span data-ttu-id="823a3-221">Indítsa el **Windows Explorer**, és keresse meg a túl**bin\debug** vagy **bin\release** mappa build hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="823a3-221">Launch **Windows Explorer**, and navigate too**bin\debug** or **bin\release** folder depending on hello type of build.</span></span>
12. <span data-ttu-id="823a3-222">Hozzon létre egy zip fájlt **MyDotNetActivity.zip** hello összes hello bináris fájlokat tartalmazó <project folder>\bin\Debug mappába.</span><span class="sxs-lookup"><span data-stu-id="823a3-222">Create a zip file **MyDotNetActivity.zip** that contains all hello binaries in hello <project folder>\bin\Debug folder.</span></span> <span data-ttu-id="823a3-223">Hello tartalmaznak **MyDotNetActivity.pdb** fájlt úgy, hogy be további részletekről, mint a sor számának hello forráskódban, ha hiba történt a hello hibát okozó.</span><span class="sxs-lookup"><span data-stu-id="823a3-223">Include hello **MyDotNetActivity.pdb** file so that you get additional details such as line number in hello source code that caused hello issue if there was a failure.</span></span> 

    > [!IMPORTANT]
    > <span data-ttu-id="823a3-224">Az egyéni tevékenység hello el kell érnie hello fájlok hello zip-fájlban szereplő összes hello **felső szintű** nem sub mappák.</span><span class="sxs-lookup"><span data-stu-id="823a3-224">All hello files in hello zip file for hello custom activity must be at hello **top level** with no sub folders.</span></span>

    ![Bináris kimeneti fájlok](./media/data-factory-use-custom-activities/Binaries.png)
14. <span data-ttu-id="823a3-226">Hozzon létre egy blob-tároló nevű **customactivitycontainer** Ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="823a3-226">Create a blob container named **customactivitycontainer** if it does not already exist.</span></span> 
15. <span data-ttu-id="823a3-227">MyDotNetActivity.zip feltöltése a blob toohello customactivitycontainer a, egy **általános célú** AzureStorageLinkedService által hivatkozott Azure blob storage (nem közbeni/ritkán Blob-tároló).</span><span class="sxs-lookup"><span data-stu-id="823a3-227">Upload MyDotNetActivity.zip as a blob toohello customactivitycontainer in a **general-purpose** Azure blob storage (not hot/cool Blob storage) that is referred by AzureStorageLinkedService.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="823a3-228">Ha a .NET tevékenység projekt tooa megoldás hozzáadása a Visual Studio Data Factory projektet tartalmaz, és adja hozzá egy hivatkozást too.NET tevékenység projekt hello adat-előállító projektet, nem kell tooperform hello utolsó két lépést manuális létrehozása hello zip-fájlt, majd ismét feltölteni a toohello általános célú Azure blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="823a3-228">If you add this .NET activity project tooa solution in Visual Studio that contains a Data Factory project, and add a reference too.NET activity project from hello Data Factory application project, you do not need tooperform hello last two steps of manually creating hello zip file and uploading it toohello general-purpose Azure blob storage.</span></span> <span data-ttu-id="823a3-229">Ha közzéteszi a Visual Studio használatával a Data Factory entitások, ezeket a lépéseket automatikusan végzett hello közzétételi folyamat.</span><span class="sxs-lookup"><span data-stu-id="823a3-229">When you publish Data Factory entities using Visual Studio, these steps are automatically done by hello publishing process.</span></span> <span data-ttu-id="823a3-230">További információkért lásd: [adat-előállító projektre a Visual Studio](#data-factory-project-in-visual-studio) szakasz.</span><span class="sxs-lookup"><span data-stu-id="823a3-230">For more information, see [Data Factory project in Visual Studio](#data-factory-project-in-visual-studio) section.</span></span>

## <a name="create-a-pipeline-with-custom-activity"></a><span data-ttu-id="823a3-231">Hozzon létre egy folyamatot egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="823a3-231">Create a pipeline with custom activity</span></span>
<span data-ttu-id="823a3-232">Létrehozott egy egyéni tevékenység és a bináris fájlok tooa blob tároló a hello zip-fájl feltöltése a **általános célú** Azure Storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="823a3-232">You have created a custom activity and uploaded hello zip file with binaries tooa blob container in a **general-purpose** Azure Storage Account.</span></span> <span data-ttu-id="823a3-233">Ebben a szakaszban hoz létre egy Azure data factory egy folyamatot, amely hello egyéni tevékenység használja.</span><span class="sxs-lookup"><span data-stu-id="823a3-233">In this section, you create an Azure data factory with a pipeline that uses hello custom activity.</span></span>

<span data-ttu-id="823a3-234">hello bemeneti adatkészlet hello egyéni tevékenység hello customactivityinput mappában adftutorial tároló hello blob Storage tárolóban található blobok (fájlok) jelöli.</span><span class="sxs-lookup"><span data-stu-id="823a3-234">hello input dataset for hello custom activity represents blobs (files) in hello customactivityinput folder of adftutorial container in hello blob storage.</span></span> <span data-ttu-id="823a3-235">hello kimeneti adatkészlet hello tevékenység kimeneti BLOB adftutorial tároló hello blob Storage tárolóban hello customactivityoutput mappában jelöli.</span><span class="sxs-lookup"><span data-stu-id="823a3-235">hello output dataset for hello activity represents output blobs in hello customactivityoutput folder of adftutorial container in hello blob storage.</span></span>

<span data-ttu-id="823a3-236">Hozzon létre **file.txt** fájl a következő tartalmat, és töltse fel az túl hello**customactivityinput** mappában található hello **adftutorial** tároló.</span><span class="sxs-lookup"><span data-stu-id="823a3-236">Create **file.txt** file with hello following content and upload it too**customactivityinput** folder of hello **adftutorial** container.</span></span> <span data-ttu-id="823a3-237">Hello adftutorial tároló létrehozása, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="823a3-237">Create hello adftutorial container if it does not exist already.</span></span> 

```
test custom activity Microsoft test custom activity Microsoft
```

<span data-ttu-id="823a3-238">hello bemeneti mappa felel meg az Azure Data Factory tooa szelet, akkor is, ha hello mappa két vagy több fájlt.</span><span class="sxs-lookup"><span data-stu-id="823a3-238">hello input folder corresponds tooa slice in Azure Data Factory even if hello folder has two or more files.</span></span> <span data-ttu-id="823a3-239">Minden szelet feldolgozása hello folyamat után hello egyéni tevékenység hello bemeneti mappában, hogy a szeletre vonatkozó összes hello BLOB telepítéseket.</span><span class="sxs-lookup"><span data-stu-id="823a3-239">When each slice is processed by hello pipeline, hello custom activity iterates through all hello blobs in hello input folder for that slice.</span></span>

<span data-ttu-id="823a3-240">Egy kimeneti fájl hello adftutorial\customactivityoutput mappában (azonos számú BLOB hello bemeneti mappában) egy vagy több sort tartalmazó jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="823a3-240">You see one output file with in hello adftutorial\customactivityoutput folder with one or more lines (same as number of blobs in hello input folder):</span></span>

```
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2016-11-16-00/file.txt.
```


<span data-ttu-id="823a3-241">Az alábbiakban a jelen szakaszban végrehajtandó hello lépések:</span><span class="sxs-lookup"><span data-stu-id="823a3-241">Here are hello steps you perform in this section:</span></span>

1. <span data-ttu-id="823a3-242">Hozzon létre egy **adat-előállító**.</span><span class="sxs-lookup"><span data-stu-id="823a3-242">Create a **data factory**.</span></span>
2. <span data-ttu-id="823a3-243">Hozzon létre **összekapcsolt szolgáltatások** a virtuális gépek melyik hello az egyéni tevékenység lefut, és hello bemeneti/kimeneti BLOB tároló Azure Storage hello hello Azure Batch-készlet.</span><span class="sxs-lookup"><span data-stu-id="823a3-243">Create **Linked services** for hello Azure Batch pool of VMs on which hello custom activity runs and hello Azure Storage that holds hello input/output blobs.</span></span>
3. <span data-ttu-id="823a3-244">Hozzon létre a bemeneti és kimeneti **adatkészletek** , amelyek megfelelnek a bemeneti és kimeneti hello egyéni tevékenység.</span><span class="sxs-lookup"><span data-stu-id="823a3-244">Create input and output **datasets** that represent input and output of hello custom activity.</span></span>
4. <span data-ttu-id="823a3-245">Hozzon létre egy **csővezeték** használó hello egyéni tevékenység.</span><span class="sxs-lookup"><span data-stu-id="823a3-245">Create a **pipeline** that uses hello custom activity.</span></span>

> [!NOTE]
> <span data-ttu-id="823a3-246">Hozzon létre hello **file.txt** , és töltse fel tooa blob tároló Ha még nem tette meg.</span><span class="sxs-lookup"><span data-stu-id="823a3-246">Create hello **file.txt** and upload it tooa blob container if you haven't already done so.</span></span> <span data-ttu-id="823a3-247">A szakasz fenti hello tudnivalókat.</span><span class="sxs-lookup"><span data-stu-id="823a3-247">See instructions in hello preceding section.</span></span>   

### <a name="step-1-create-hello-data-factory"></a><span data-ttu-id="823a3-248">1. lépés: Hello adat-előállító létrehozása</span><span class="sxs-lookup"><span data-stu-id="823a3-248">Step 1: Create hello data factory</span></span>
1. <span data-ttu-id="823a3-249">Az hello a bejelentkezés után toohello Azure-portálon, a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="823a3-249">After logging in toohello Azure portal, do hello following steps:</span></span>
   1. <span data-ttu-id="823a3-250">Kattintson a **új** hello bal oldali menüben.</span><span class="sxs-lookup"><span data-stu-id="823a3-250">Click **NEW** on hello left menu.</span></span>
   2. <span data-ttu-id="823a3-251">Kattintson a **adatok + analitika** a hello **új** panelen.</span><span class="sxs-lookup"><span data-stu-id="823a3-251">Click **Data + Analytics** in hello **New** blade.</span></span>
   3. <span data-ttu-id="823a3-252">Kattintson a **adat-előállító** a hello **adatelemzés** panelen.</span><span class="sxs-lookup"><span data-stu-id="823a3-252">Click **Data Factory** on hello **Data analytics** blade.</span></span>
   
    ![Új Azure Data Factory menü](media/data-factory-use-custom-activities/new-azure-data-factory-menu.png)
2. <span data-ttu-id="823a3-254">A hello **új adat-előállító** panelen adja meg **CustomActivityFactory** a hello nevét.</span><span class="sxs-lookup"><span data-stu-id="823a3-254">In hello **New data factory** blade, enter **CustomActivityFactory** for hello Name.</span></span> <span data-ttu-id="823a3-255">az Azure data factory hello hello nevének globálisan egyedi kell lennie.</span><span class="sxs-lookup"><span data-stu-id="823a3-255">hello name of hello Azure data factory must be globally unique.</span></span> <span data-ttu-id="823a3-256">Ha hello hibaüzenetet kapja: **nem érhető el adat-előállító "CustomActivityFactory"**, adat-előállító hello hello nevének módosítása (például **yournameCustomActivityFactory**), és próbálja meg létrehozni újra.</span><span class="sxs-lookup"><span data-stu-id="823a3-256">If you receive hello error: **Data factory name “CustomActivityFactory” is not available**, change hello name of hello data factory (for example, **yournameCustomActivityFactory**) and try creating again.</span></span>

    ![Új Azure Data Factory panel](media/data-factory-use-custom-activities/new-azure-data-factory-blade.png)
3. <span data-ttu-id="823a3-258">Kattintson a **ERŐFORRÁSCSOPORT-név**, és válasszon ki egy meglévő erőforráscsoportot, vagy hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="823a3-258">Click **RESOURCE GROUP NAME**, and select an existing resource group or create a resource group.</span></span>
4. <span data-ttu-id="823a3-259">Győződjön meg arról, hogy helyes-e hello használunk **előfizetés** és **régió** hello data factory toobe létrehozni kívánt.</span><span class="sxs-lookup"><span data-stu-id="823a3-259">Verify that you are using hello correct **subscription** and **region** where you want hello data factory toobe created.</span></span>
5. <span data-ttu-id="823a3-260">Kattintson a **létrehozása** a hello **új adat-előállító** panelen.</span><span class="sxs-lookup"><span data-stu-id="823a3-260">Click **Create** on hello **New data factory** blade.</span></span>
6. <span data-ttu-id="823a3-261">Hello adat-előállító létrehozása hello látja **irányítópult** a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="823a3-261">You see hello data factory being created in hello **Dashboard** of hello Azure portal.</span></span>
7. <span data-ttu-id="823a3-262">Miután hello adat-előállító létrehozása sikerült, hello adat-előállító panel, amely jelzi, hogy látja hello hello adat-előállító tartalmát.</span><span class="sxs-lookup"><span data-stu-id="823a3-262">After hello data factory has been created successfully, you see hello Data Factory blade, which shows you hello contents of hello data factory.</span></span>
    
    ![A Data Factory panel](media/data-factory-use-custom-activities/data-factory-blade.png)

### <a name="step-2-create-linked-services"></a><span data-ttu-id="823a3-264">2. lépés: A társított szolgáltatások létrehozásához</span><span class="sxs-lookup"><span data-stu-id="823a3-264">Step 2: Create linked services</span></span>
<span data-ttu-id="823a3-265">Összekapcsolt szolgáltatások adattárolókhoz hivatkozásra, vagy a szolgáltatások tooan az Azure data factory számítási.</span><span class="sxs-lookup"><span data-stu-id="823a3-265">Linked services link data stores or compute services tooan Azure data factory.</span></span> <span data-ttu-id="823a3-266">Ebben a lépésben csatolja a az Azure Storage-fiók és az Azure Batch tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="823a3-266">In this step, you link your Azure Storage account and Azure Batch account tooyour data factory.</span></span>

#### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="823a3-267">Azure Storage társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="823a3-267">Create Azure Storage linked service</span></span>
1. <span data-ttu-id="823a3-268">Kattintson a hello **Szerző és központi telepítése** hello csempét **DATA FACTORY** paneljén **CustomActivityFactory**.</span><span class="sxs-lookup"><span data-stu-id="823a3-268">Click hello **Author and deploy** tile on hello **DATA FACTORY** blade for **CustomActivityFactory**.</span></span> <span data-ttu-id="823a3-269">Megjelenik a Data Factory Editor hello.</span><span class="sxs-lookup"><span data-stu-id="823a3-269">You see hello Data Factory Editor.</span></span>
2. <span data-ttu-id="823a3-270">Kattintson a **az új adattároló** a hello parancssávon, és válassza a **az Azure storage**.</span><span class="sxs-lookup"><span data-stu-id="823a3-270">Click **New data store** on hello command bar and choose **Azure storage**.</span></span> <span data-ttu-id="823a3-271">Megtekintheti az hello JSON-parancsfájl létrehozásához egy Azure Storage társított szolgáltatásnak hello-szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="823a3-271">You should see hello JSON script for creating an Azure Storage linked service in hello editor.</span></span>
    
    ![Az új adattároló - Azure Storage](media/data-factory-use-custom-activities/new-data-store-menu.png)
3. <span data-ttu-id="823a3-273">Cserélje le `<accountname>` az Azure storage-fiók nevére és `<accountkey>` hello Azure storage-fiók hozzáférési kulcsával.</span><span class="sxs-lookup"><span data-stu-id="823a3-273">Replace `<accountname>` with name of your Azure storage account and `<accountkey>` with access key of hello Azure storage account.</span></span> <span data-ttu-id="823a3-274">toolearn hogyan tooget tárhelyét férnek hozzá, tekintse meg [megtekintése, másolása és újragenerálása tárolási hívóbetűk](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="823a3-274">toolearn how tooget your storage access key, see [View, copy and regenerate storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>

    ![Az Azure Storage szolgáltatás tetszését](media/data-factory-use-custom-activities/azure-storage-linked-service.png)
4. <span data-ttu-id="823a3-276">Kattintson a **telepítés** a hello parancssávon toodeploy kapcsolódó hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="823a3-276">Click **Deploy** on hello command bar toodeploy hello linked service.</span></span>

#### <a name="create-azure-batch-linked-service"></a><span data-ttu-id="823a3-277">Csatolt Azure Batch szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="823a3-277">Create Azure Batch linked service</span></span>
1. <span data-ttu-id="823a3-278">A Data Factory Editor hello, kattintson **... További** hello parancssávon kattintson **új számítási**, majd válassza ki **Azure Batch** hello menüből.</span><span class="sxs-lookup"><span data-stu-id="823a3-278">In hello Data Factory Editor, click **... More** on hello command bar, click **New compute**, and then select **Azure Batch** from hello menu.</span></span>

    ![Új számítási - Azure Batch](media/data-factory-use-custom-activities/new-azure-compute-batch.png)
2. <span data-ttu-id="823a3-280">Hajtsa végre a következő módosításokat toohello JSON-parancsfájl hello:</span><span class="sxs-lookup"><span data-stu-id="823a3-280">Make hello following changes toohello JSON script:</span></span>

   1. <span data-ttu-id="823a3-281">Adja meg az Azure Batch-fiók nevét a hello **accountName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="823a3-281">Specify Azure Batch account name for hello **accountName** property.</span></span> <span data-ttu-id="823a3-282">Hello **URL-cím** a hello **Azure Batch-fiók panelen** hello formátuma a következő szerepel: `http://accountname.region.batch.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="823a3-282">hello **URL** from hello **Azure Batch account blade** is in hello following format: `http://accountname.region.batch.azure.com`.</span></span> <span data-ttu-id="823a3-283">A hello **batchUri** tulajdonság hello JSON, tooremove kell `accountname.` a hello URL-cím és -felhasználási hello `accountname` a hello `accountName` JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="823a3-283">For hello **batchUri** property in hello JSON, you need tooremove `accountname.` from hello URL and use hello `accountname` for hello `accountName` JSON property.</span></span>
   2. <span data-ttu-id="823a3-284">Adja meg az Azure Batch-fiók kulcsot hello a hello **accessKey** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="823a3-284">Specify hello Azure Batch account key for hello **accessKey** property.</span></span>
   3. <span data-ttu-id="823a3-285">Adja meg a létrehozott hello készlet neve hello hello előfeltételei részeként **poolName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="823a3-285">Specify hello name of hello pool you created as part of prerequisites for hello **poolName** property.</span></span> <span data-ttu-id="823a3-286">Hello készlet hello hello neve helyett hello azonosító is megadható.</span><span class="sxs-lookup"><span data-stu-id="823a3-286">You can also specify hello ID of hello pool instead of hello name of hello pool.</span></span>
   4. <span data-ttu-id="823a3-287">Azure Batch URI megadása hello **batchUri** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="823a3-287">Specify Azure Batch URI for hello **batchUri** property.</span></span> <span data-ttu-id="823a3-288">Példa: `https://westus.batch.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="823a3-288">Example: `https://westus.batch.azure.com`.</span></span>  
   5. <span data-ttu-id="823a3-289">Adja meg a hello **AzureStorageLinkedService** a hello **linkedServiceName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="823a3-289">Specify hello **AzureStorageLinkedService** for hello **linkedServiceName** property.</span></span>

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

       <span data-ttu-id="823a3-290">A hello **poolName** tulajdonság, azt is megadhatja, hello azonosító hello készlet hello hello neve helyett.</span><span class="sxs-lookup"><span data-stu-id="823a3-290">For hello **poolName** property, you can also specify hello ID of hello pool instead of hello name of hello pool.</span></span>

      > [!IMPORTANT]
      > <span data-ttu-id="823a3-291">hello Data Factory szolgáltatásnak nem támogatja egy igény szerinti beállítást az Azure hdinsight azonban nem.</span><span class="sxs-lookup"><span data-stu-id="823a3-291">hello Data Factory service does not support an on-demand option for Azure Batch as it does for HDInsight.</span></span> <span data-ttu-id="823a3-292">Csak a saját Azure Batch-készlet használhatja az Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="823a3-292">You can only use your own Azure Batch pool in an Azure data factory.</span></span>   
    

### <a name="step-3-create-datasets"></a><span data-ttu-id="823a3-293">3. lépés: Adatkészletek létrehozása</span><span class="sxs-lookup"><span data-stu-id="823a3-293">Step 3: Create datasets</span></span>
<span data-ttu-id="823a3-294">Ebben a lépésben létrehozni adatkészletek toorepresent bemeneti és kimeneti adatai.</span><span class="sxs-lookup"><span data-stu-id="823a3-294">In this step, you create datasets toorepresent input and output data.</span></span>

#### <a name="create-input-dataset"></a><span data-ttu-id="823a3-295">Bemeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="823a3-295">Create input dataset</span></span>
1. <span data-ttu-id="823a3-296">A hello **szerkesztő** hello adat-előállítót, kattintson a **... További** hello parancssávon kattintson **új adatkészlet**, majd válassza ki **Azure Blob Storage tárolóban** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="823a3-296">In hello **Editor** for hello Data Factory, click **... More** on hello command bar, click **New dataset**, and then select **Azure Blob storage** from hello drop-down menu.</span></span>
2. <span data-ttu-id="823a3-297">Cserélje le a következő JSON részlet hello hello JSON hello jobb oldali ablaktáblában:</span><span class="sxs-lookup"><span data-stu-id="823a3-297">Replace hello JSON in hello right pane with hello following JSON snippet:</span></span>

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

   <span data-ttu-id="823a3-298">Ez a forgatókönyv a kezdő időpont későbbi részében hozzon létre egy folyamatot: 2016-11-16T00:00:00Z és záró idő: 2016-11-16T05:00:00Z.</span><span class="sxs-lookup"><span data-stu-id="823a3-298">You create a pipeline later in this walkthrough with start time: 2016-11-16T00:00:00Z and end time: 2016-11-16T05:00:00Z.</span></span> <span data-ttu-id="823a3-299">Már ütemezett tooproduce adatok óránkénti, így öt bemeneti/kimeneti szeletek (közötti **00**: 00:00 -> **05**: 00:00).</span><span class="sxs-lookup"><span data-stu-id="823a3-299">It is scheduled tooproduce data hourly, so there are five input/output slices (between **00**:00:00 -> **05**:00:00).</span></span>

   <span data-ttu-id="823a3-300">Hello **gyakoriság** és **időköz** hello bemeneti adatkészlet túl van-e állítva a**óra** és **1**, ami azt jelenti, hogy hello bemeneti szelet érhető el óránként.</span><span class="sxs-lookup"><span data-stu-id="823a3-300">hello **frequency** and **interval** for hello input dataset is set too**Hour** and **1**, which means that hello input slice is available hourly.</span></span> <span data-ttu-id="823a3-301">Az ebben a példában is azonos hello hello intputfolder (file.txt) fájlt.</span><span class="sxs-lookup"><span data-stu-id="823a3-301">In this sample, it is hello same file (file.txt) in hello intputfolder.</span></span>

   <span data-ttu-id="823a3-302">Az alábbiakban minden szelet, amely a fenti JSON részlet hello SliceStart rendszerváltozó által képviselt hello kezdési idejét.</span><span class="sxs-lookup"><span data-stu-id="823a3-302">Here are hello start times for each slice, which is represented by SliceStart system variable in hello above JSON snippet.</span></span>
3. <span data-ttu-id="823a3-303">Kattintson a **telepítés** eszköztár toocreate hello és központi telepítése hello **InputDataset**.</span><span class="sxs-lookup"><span data-stu-id="823a3-303">Click **Deploy** on hello toolbar toocreate and deploy hello **InputDataset**.</span></span> <span data-ttu-id="823a3-304">Ellenőrizze, hogy látható-e hello **tábla sikeresen LÉTREHOZVA** hello címsorában hello szerkesztő-üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="823a3-304">Confirm that you see hello **TABLE CREATED SUCCESSFULLY** message on hello title bar of hello Editor.</span></span>

#### <a name="create-an-output-dataset"></a><span data-ttu-id="823a3-305">Kimeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="823a3-305">Create an output dataset</span></span>
1. <span data-ttu-id="823a3-306">A hello **Data Factory editor**, kattintson a **... További** hello parancssávon kattintson **új adatkészlet**, majd válassza ki **Azure Blob Storage tárolóban**.</span><span class="sxs-lookup"><span data-stu-id="823a3-306">In hello **Data Factory editor**, click **... More** on hello command bar, click **New dataset**, and then select **Azure Blob storage**.</span></span>
2. <span data-ttu-id="823a3-307">Cserélje le a JSON-parancsfájl hello hello jobb oldali hello JSON-parancsfájl a következő:</span><span class="sxs-lookup"><span data-stu-id="823a3-307">Replace hello JSON script in hello right pane with hello following JSON script:</span></span>

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

     <span data-ttu-id="823a3-308">Kimeneti hely **adftutorial/customactivityoutput/** és kimeneti fájlnév: éééé-HH-NN-HH.txt, ha éééé-hh-nn óó hello év, hónap, dátum és óra alatt előállított hello szelet.</span><span class="sxs-lookup"><span data-stu-id="823a3-308">Output location is **adftutorial/customactivityoutput/** and output file name is yyyy-MM-dd-HH.txt where yyyy-MM-dd-HH is hello year, month, date, and hour of hello slice being produced.</span></span> <span data-ttu-id="823a3-309">Lásd: [fejlesztői leírás] [ adf-developer-reference] részleteiről.</span><span class="sxs-lookup"><span data-stu-id="823a3-309">See [Developer Reference][adf-developer-reference] for details.</span></span>

    <span data-ttu-id="823a3-310">Egy kimeneti blob/fájl az egyes bemeneti szeletek jön létre.</span><span class="sxs-lookup"><span data-stu-id="823a3-310">An output blob/file is generated for each input slice.</span></span> <span data-ttu-id="823a3-311">Ez hogyan kimeneti fájl neve az egyes szeletek.</span><span class="sxs-lookup"><span data-stu-id="823a3-311">Here is how an output file is named for each slice.</span></span> <span data-ttu-id="823a3-312">Minden hello kimeneti fájlok akkor jönnek létre, egy kimeneti mappában: **adftutorial\customactivityoutput**.</span><span class="sxs-lookup"><span data-stu-id="823a3-312">All hello output files are generated in one output folder: **adftutorial\customactivityoutput**.</span></span>

   | <span data-ttu-id="823a3-313">Szelet</span><span class="sxs-lookup"><span data-stu-id="823a3-313">Slice</span></span> | <span data-ttu-id="823a3-314">Kezdési idő</span><span class="sxs-lookup"><span data-stu-id="823a3-314">Start time</span></span> | <span data-ttu-id="823a3-315">Kimeneti fájlja</span><span class="sxs-lookup"><span data-stu-id="823a3-315">Output file</span></span> |
   |:--- |:--- |:--- |
   | <span data-ttu-id="823a3-316">1</span><span class="sxs-lookup"><span data-stu-id="823a3-316">1</span></span> |<span data-ttu-id="823a3-317">2016-11-16T00:00:00</span><span class="sxs-lookup"><span data-stu-id="823a3-317">2016-11-16T00:00:00</span></span> |<span data-ttu-id="823a3-318">2016-11-16-00.txt</span><span class="sxs-lookup"><span data-stu-id="823a3-318">2016-11-16-00.txt</span></span> |
   | <span data-ttu-id="823a3-319">2</span><span class="sxs-lookup"><span data-stu-id="823a3-319">2</span></span> |<span data-ttu-id="823a3-320">2016-11-16T01:00:00</span><span class="sxs-lookup"><span data-stu-id="823a3-320">2016-11-16T01:00:00</span></span> |<span data-ttu-id="823a3-321">2016-11-16-01.txt</span><span class="sxs-lookup"><span data-stu-id="823a3-321">2016-11-16-01.txt</span></span> |
   | <span data-ttu-id="823a3-322">3</span><span class="sxs-lookup"><span data-stu-id="823a3-322">3</span></span> |<span data-ttu-id="823a3-323">2016-11-16T02:00:00</span><span class="sxs-lookup"><span data-stu-id="823a3-323">2016-11-16T02:00:00</span></span> |<span data-ttu-id="823a3-324">2016-11-16-02.txt</span><span class="sxs-lookup"><span data-stu-id="823a3-324">2016-11-16-02.txt</span></span> |
   | <span data-ttu-id="823a3-325">4</span><span class="sxs-lookup"><span data-stu-id="823a3-325">4</span></span> |<span data-ttu-id="823a3-326">2016-11-16T03:00:00</span><span class="sxs-lookup"><span data-stu-id="823a3-326">2016-11-16T03:00:00</span></span> |<span data-ttu-id="823a3-327">2016-11-16-03.txt</span><span class="sxs-lookup"><span data-stu-id="823a3-327">2016-11-16-03.txt</span></span> |
   | <span data-ttu-id="823a3-328">5</span><span class="sxs-lookup"><span data-stu-id="823a3-328">5</span></span> |<span data-ttu-id="823a3-329">2016-11-16T04:00:00</span><span class="sxs-lookup"><span data-stu-id="823a3-329">2016-11-16T04:00:00</span></span> |<span data-ttu-id="823a3-330">2016-11-16-04.txt</span><span class="sxs-lookup"><span data-stu-id="823a3-330">2016-11-16-04.txt</span></span> |

    <span data-ttu-id="823a3-331">Ne feledje, hogy egy bemeneti mappában lévő összes hello fájlt a fent említett hello indítási idejének szelet részét.</span><span class="sxs-lookup"><span data-stu-id="823a3-331">Remember that all hello files in an input folder are part of a slice with hello start times mentioned above.</span></span> <span data-ttu-id="823a3-332">A szelet feldolgozása után hello egyéni tevékenység minden egyes fájl vizsgálja, és hozza létre a keresési kifejezés ("Microsoft") előfordulásainak száma hello hello kimeneti fájlban egy sor.</span><span class="sxs-lookup"><span data-stu-id="823a3-332">When this slice is processed, hello custom activity scans through each file and produces a line in hello output file with hello number of occurrences of search term (“Microsoft”).</span></span> <span data-ttu-id="823a3-333">Ha hello inputfolder három fájlok, vannak-e három sort hello kimeneti fájl az egyes óránkénti szeletek: 2016-11-16:01:00:00.txt 2016-11-16-00.txt, stb.</span><span class="sxs-lookup"><span data-stu-id="823a3-333">If there are three files in hello inputfolder, there are three lines in hello output file for each hourly slice: 2016-11-16-00.txt, 2016-11-16:01:00:00.txt, etc.</span></span>
3. <span data-ttu-id="823a3-334">toodeploy hello **OutputDataset**, kattintson a **telepítés** hello parancssávon.</span><span class="sxs-lookup"><span data-stu-id="823a3-334">toodeploy hello **OutputDataset**, click **Deploy** on hello command bar.</span></span>

### <a name="create-and-run-a-pipeline-that-uses-hello-custom-activity"></a><span data-ttu-id="823a3-335">Hozzon létre és futtasson egy folyamatot, amely hello egyéni tevékenység használja</span><span class="sxs-lookup"><span data-stu-id="823a3-335">Create and run a pipeline that uses hello custom activity</span></span>
1. <span data-ttu-id="823a3-336">A Data Factory Editor hello, kattintson **... További**, majd válassza ki **új adatcsatorna** hello parancssávon.</span><span class="sxs-lookup"><span data-stu-id="823a3-336">In hello Data Factory Editor, click **... More**, and then select **New pipeline** on hello command bar.</span></span> 
2. <span data-ttu-id="823a3-337">Cserélje le a JSON-parancsfájl a következő hello hello JSON hello jobb oldali ablaktáblában:</span><span class="sxs-lookup"><span data-stu-id="823a3-337">Replace hello JSON in hello right pane with hello following JSON script:</span></span>

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

    <span data-ttu-id="823a3-338">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="823a3-338">Note hello following points:</span></span>

   * <span data-ttu-id="823a3-339">**Egyidejűségi** értéke túl**2** , hogy két szeletek párhuzamosan dolgozza fel a hello Azure Batch-készlet 2 virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="823a3-339">**Concurrency** is set too**2** so that two slices are processed in parallel by 2 VMs in hello Azure Batch pool.</span></span>
   * <span data-ttu-id="823a3-340">Egy tevékenység hello tevékenységek szakaszban van, és a típusa: **DotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="823a3-340">There is one activity in hello activities section and it is of type: **DotNetActivity**.</span></span>
   * <span data-ttu-id="823a3-341">**AssemblyName** hello DLL toohello neve van beállítva: **MyDotnetActivity.dll**.</span><span class="sxs-lookup"><span data-stu-id="823a3-341">**AssemblyName** is set toohello name of hello DLL: **MyDotnetActivity.dll**.</span></span>
   * <span data-ttu-id="823a3-342">**EntryPoint** értéke túl**MyDotNetActivityNS.MyDotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="823a3-342">**EntryPoint** is set too**MyDotNetActivityNS.MyDotNetActivity**.</span></span>
   * <span data-ttu-id="823a3-343">**PackageLinkedService** értéke túl**AzureStorageLinkedService** toohello blob-tároló hello egyéni tevékenység zip fájlt tartalmazó mutat.</span><span class="sxs-lookup"><span data-stu-id="823a3-343">**PackageLinkedService** is set too**AzureStorageLinkedService** that points toohello blob storage that contains hello custom activity zip file.</span></span> <span data-ttu-id="823a3-344">Ha használ különböző Azure Storage-fiókok a bemeneti/kimeneti fájlok és hello egyéni tevékenység zip-fájl, létrehozhat egy másik Azure tárolás társított szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="823a3-344">If you are using different Azure Storage accounts for input/output files and hello custom activity zip file, you create another Azure Storage linked service.</span></span> <span data-ttu-id="823a3-345">Ez a cikk feltételezi, hogy a hello azonos Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="823a3-345">This article assumes that you are using hello same Azure Storage account.</span></span>
   * <span data-ttu-id="823a3-346">**PackageFile** értéke túl**customactivitycontainer/MyDotNetActivity.zip**.</span><span class="sxs-lookup"><span data-stu-id="823a3-346">**PackageFile** is set too**customactivitycontainer/MyDotNetActivity.zip**.</span></span> <span data-ttu-id="823a3-347">A hello formátumban: containerforthezip/nameofthezip.zip.</span><span class="sxs-lookup"><span data-stu-id="823a3-347">It is in hello format: containerforthezip/nameofthezip.zip.</span></span>
   * <span data-ttu-id="823a3-348">hello egyéni tevékenység veszi **InputDataset** bemenetként és **OutputDataset** output típusúként.</span><span class="sxs-lookup"><span data-stu-id="823a3-348">hello custom activity takes **InputDataset** as input and **OutputDataset** as output.</span></span>
   * <span data-ttu-id="823a3-349">hello linkedServiceName tulajdonsága hello egyéni tevékenység mutat toohello **AzureBatchLinkedService**, amely közli az Azure Data Factory hello egyéni tevékenységre kell toorun Azure Batch virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="823a3-349">hello linkedServiceName property of hello custom activity points toohello **AzureBatchLinkedService**, which tells Azure Data Factory that hello custom activity needs toorun on Azure Batch VMs.</span></span>
   * <span data-ttu-id="823a3-350">**isPaused** tulajdonsága túl**hamis** alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="823a3-350">**isPaused** property is set too**false** by default.</span></span> <span data-ttu-id="823a3-351">hello csővezeték azonnal futtatja ebben a példában, mert hello szeletek indítsa el a korábbi hello.</span><span class="sxs-lookup"><span data-stu-id="823a3-351">hello pipeline runs immediately in this example because hello slices start in hello past.</span></span> <span data-ttu-id="823a3-352">Ez a tulajdonság tootrue toopause hello folyamat beállítása, és állítsa be úgy a hátsó toofalse toorestart.</span><span class="sxs-lookup"><span data-stu-id="823a3-352">You can set this property tootrue toopause hello pipeline and set it back toofalse toorestart.</span></span>
   * <span data-ttu-id="823a3-353">Hello **start** idő és **end** idők **öt** egymástól óra és szeletek előállítása hourly, így öt szeletek hello csővezeték hozzák létre.</span><span class="sxs-lookup"><span data-stu-id="823a3-353">hello **start** time and **end** times are **five** hours apart and slices are produced hourly, so five slices are produced by hello pipeline.</span></span>
3. <span data-ttu-id="823a3-354">toodeploy hello sorban, kattintson a **telepítés** hello parancssávon.</span><span class="sxs-lookup"><span data-stu-id="823a3-354">toodeploy hello pipeline, click **Deploy** on hello command bar.</span></span>

### <a name="monitor-hello-pipeline"></a><span data-ttu-id="823a3-355">A figyelő hello folyamat</span><span class="sxs-lookup"><span data-stu-id="823a3-355">Monitor hello pipeline</span></span>
1. <span data-ttu-id="823a3-356">Hello a Data Factory panelen hello Azure-portálon, kattintson a **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="823a3-356">In hello Data Factory blade in hello Azure portal, click **Diagram**.</span></span>

    ![Diagram csempe](./media/data-factory-use-custom-activities/DataFactoryBlade.png)
2. <span data-ttu-id="823a3-358">A Diagram nézet hello Ekkor kattintson a hello OutputDataset.</span><span class="sxs-lookup"><span data-stu-id="823a3-358">In hello Diagram View, now click hello OutputDataset.</span></span>

    ![Diagramnézet](./media/data-factory-use-custom-activities/diagram.png)
3. <span data-ttu-id="823a3-360">Meg kell jelennie, hogy hello öt kimeneti szeletek hello kész állapotban van-e.</span><span class="sxs-lookup"><span data-stu-id="823a3-360">You should see that hello five output slices are in hello Ready state.</span></span> <span data-ttu-id="823a3-361">Ha nincsenek hello üzemkész állapotban, hogy még nem készült még.</span><span class="sxs-lookup"><span data-stu-id="823a3-361">If they are not in hello Ready state, they haven't been produced yet.</span></span> 

   ![Kimeneti szeletek](./media/data-factory-use-custom-activities/OutputSlices.png)
4. <span data-ttu-id="823a3-363">Győződjön meg arról, hogy a hello kimeneti fájlok legyenek létrehozva hello blob Storage tárolóban lévő hello **adftutorial** tároló.</span><span class="sxs-lookup"><span data-stu-id="823a3-363">Verify that hello output files are generated in hello blob storage in hello **adftutorial** container.</span></span>

   ![egyéni tevékenység kimenetét][image-data-factory-ouput-from-custom-activity]
5. <span data-ttu-id="823a3-365">Hello kimeneti fájl megnyitásakor, láthatja a hello kimeneti hasonló toohello kimenete a következő:</span><span class="sxs-lookup"><span data-stu-id="823a3-365">If you open hello output file, you should see hello output similar toohello following output:</span></span>

    ```
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2016-11-16-00/file.txt.
    ```
6. <span data-ttu-id="823a3-366">Használjon hello [Azure-portálon] [ azure-preview-portal] vagy az Azure PowerShell-parancsmagok toomonitor a data factory, folyamatok és adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="823a3-366">Use hello [Azure portal][azure-preview-portal] or Azure PowerShell cmdlets toomonitor your data factory, pipelines, and data sets.</span></span> <span data-ttu-id="823a3-367">Hello üzeneteit látható **ActivityLogger** hello kódban hello egyéni tevékenység hello-naplók (kifejezetten felhasználói-0.log) tölthető le: hello portál vagy parancsmagok használatával.</span><span class="sxs-lookup"><span data-stu-id="823a3-367">You can see messages from hello **ActivityLogger** in hello code for hello custom activity in hello logs (specifically user-0.log) that you can download from hello portal or using cmdlets.</span></span>

   ![az egyéni tevékenység naplók letöltése][image-data-factory-download-logs-from-custom-activity]

<span data-ttu-id="823a3-369">Lásd: [figyelő és folyamatok kezelése](data-factory-monitor-manage-pipelines.md) adathalmazok és adatcsatornák figyelemmel kísérésére részletes leírást.</span><span class="sxs-lookup"><span data-stu-id="823a3-369">See [Monitor and Manage Pipelines](data-factory-monitor-manage-pipelines.md) for detailed steps for monitoring datasets and pipelines.</span></span>      

## <a name="data-factory-project-in-visual-studio"></a><span data-ttu-id="823a3-370">Data Factory projektre a Visual Studióban</span><span class="sxs-lookup"><span data-stu-id="823a3-370">Data Factory project in Visual Studio</span></span>  
<span data-ttu-id="823a3-371">Hozzon létre, és tegye közzé a Data Factory entitásokat Visual Studio használatával Azure portál használata helyett.</span><span class="sxs-lookup"><span data-stu-id="823a3-371">You can create and publish Data Factory entities by using Visual Studio instead of using Azure portal.</span></span> <span data-ttu-id="823a3-372">Részletes információkat létrehozása és közzététele a Data Factory entitások Visual Studio használatával, lásd: [felépítheti első folyamatát Visual Studio használatával](data-factory-build-your-first-pipeline-using-vs.md) és [adatok másolása az Azure Blob tooAzure SQL](data-factory-copy-activity-tutorial-using-visual-studio.md) cikkek.</span><span class="sxs-lookup"><span data-stu-id="823a3-372">For detailed information about creating and publishing Data Factory entities by using Visual Studio, See [Build your first pipeline using Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) and [Copy data from Azure Blob tooAzure SQL](data-factory-copy-activity-tutorial-using-visual-studio.md) articles.</span></span>

<span data-ttu-id="823a3-373">További lépések Data Factory-projekt létrehozása a Visual Studio hello:</span><span class="sxs-lookup"><span data-stu-id="823a3-373">Do hello following additional steps if you are creating Data Factory project in Visual Studio:</span></span>
 
1. <span data-ttu-id="823a3-374">Adja hozzá a hello adat-előállító projekt toohello Visual Studio megoldás, amely hello egyéni tevékenység projekt tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="823a3-374">Add hello Data Factory project toohello Visual Studio solution that contains hello custom activity project.</span></span> 
2. <span data-ttu-id="823a3-375">Adja hozzá egy hivatkozást toohello .NET tevékenység projekt hello adat-előállító projekt.</span><span class="sxs-lookup"><span data-stu-id="823a3-375">Add a reference toohello .NET activity project from hello Data Factory project.</span></span> <span data-ttu-id="823a3-376">Jobb gombbal az adat-előállító projekt túl**Hozzáadás**, és kattintson a **hivatkozás**.</span><span class="sxs-lookup"><span data-stu-id="823a3-376">Right-click Data Factory project, point too**Add**, and then click **Reference**.</span></span> 
3. <span data-ttu-id="823a3-377">A hello **hivatkozás hozzáadása** párbeszédpanel megnyitásához, jelölje be hello **MyDotNetActivity** projektre, majd kattintson az **OK**.</span><span class="sxs-lookup"><span data-stu-id="823a3-377">In hello **Add Reference** dialog box, select hello **MyDotNetActivity** project, and click **OK**.</span></span>
4. <span data-ttu-id="823a3-378">Hozza létre és hello megoldás közzététele.</span><span class="sxs-lookup"><span data-stu-id="823a3-378">Build and publish hello solution.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="823a3-379">Ha közzéteszi a Data Factory entitások, automatikusan létrejön egy zip-fájlt, és feltöltött toohello blob tároló: customactivitycontainer.</span><span class="sxs-lookup"><span data-stu-id="823a3-379">When you publish Data Factory entities, a zip file is automatically created for you and is uploaded toohello blob container: customactivitycontainer.</span></span> <span data-ttu-id="823a3-380">Ha hello blob tároló nem létezik, automatikusan létrejön túl.</span><span class="sxs-lookup"><span data-stu-id="823a3-380">If hello blob container does not exist, it is automatically created too.</span></span>  


## <a name="data-factory-and-batch-integration"></a><span data-ttu-id="823a3-381">Adat-előállító és kötegelt integrációja</span><span class="sxs-lookup"><span data-stu-id="823a3-381">Data Factory and Batch integration</span></span>
<span data-ttu-id="823a3-382">hello Data Factory szolgáltatásnak hoz létre egy feladatot az Azure Batch névvel hello: **adf-poolname: feladat-xxx**.</span><span class="sxs-lookup"><span data-stu-id="823a3-382">hello Data Factory service creates a job in Azure Batch with hello name: **adf-poolname: job-xxx**.</span></span> <span data-ttu-id="823a3-383">Kattintson a **feladatok** hello bal oldali menüből.</span><span class="sxs-lookup"><span data-stu-id="823a3-383">Click **Jobs** from hello left menu.</span></span> 

![Az Azure Data Factory - kötegelt feladatok](media/data-factory-use-custom-activities/data-factory-batch-jobs.png)

<span data-ttu-id="823a3-385">Egy feladat minden egyes tevékenység futtatásához a szelet jön létre.</span><span class="sxs-lookup"><span data-stu-id="823a3-385">A task is created for each activity run of a slice.</span></span> <span data-ttu-id="823a3-386">Ha öt szeletek készen toobe feldolgozott, ez a feladat öt feladatok jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="823a3-386">If there are five slices ready toobe processed, five tasks are created in this job.</span></span> <span data-ttu-id="823a3-387">Ha több számítási csomópontok szerepelnek hello Batch-készlet, két vagy több szeletek párhuzamosan is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="823a3-387">If there are multiple compute nodes in hello Batch pool, two or more slices can run in parallel.</span></span> <span data-ttu-id="823a3-388">Ha hello maximális tevékenységek maximális száma számítási csomópont értéke túl > 1 is lehet hello futó egynél több szelet azonos számítási.</span><span class="sxs-lookup"><span data-stu-id="823a3-388">If hello maximum tasks per compute node is set too> 1, you can also have more than one slice running on hello same compute.</span></span>

![Az Azure Data Factory - kötegelt munka feladatai](media/data-factory-use-custom-activities/data-factory-batch-job-tasks.png)

<span data-ttu-id="823a3-390">a következő diagram hello Azure Data Factory és kötegelt feladatok hello kapcsolatát mutatja be.</span><span class="sxs-lookup"><span data-stu-id="823a3-390">hello following diagram illustrates hello relationship between Azure Data Factory and Batch tasks.</span></span>

![Adat-előállító & kötegelt](./media/data-factory-use-custom-activities/DataFactoryAndBatch.png)

## <a name="troubleshoot-failures"></a><span data-ttu-id="823a3-392">Hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="823a3-392">Troubleshoot failures</span></span>
<span data-ttu-id="823a3-393">Néhány alapvető technikák áll:</span><span class="sxs-lookup"><span data-stu-id="823a3-393">Troubleshooting consists of a few basic techniques:</span></span>

1. <span data-ttu-id="823a3-394">Ha megjelenik a következő hiba hello, használhat egy gyakran használt adatok/ritkán blob-tároló egy általános célú Azure blob storage használata helyett.</span><span class="sxs-lookup"><span data-stu-id="823a3-394">If you see hello following error, you may be using a Hot/Cool blob storage instead of using a general-purpose Azure blob storage.</span></span> <span data-ttu-id="823a3-395">Töltse fel a zip-fájl tooa hello **általános célú Azure Storage-fiók**.</span><span class="sxs-lookup"><span data-stu-id="823a3-395">Upload hello zip file tooa **general-purpose Azure Storage Account**.</span></span> 
 
    ```
    Error in Activity: Job encountered scheduling error. Code: BlobDownloadMiscError Category: ServerError Message: Miscellaneous error encountered while downloading one of hello specified Azure Blob(s).
    ``` 
2. <span data-ttu-id="823a3-396">Ha megjelenik a következő hiba hello, győződjön meg arról, hogy a hello CS egyezések hello fájlnév hello megadott hello osztály hello neve **EntryPoint** hello adatcsatorna JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="823a3-396">If you see hello following error, confirm that hello name of hello class in hello CS file matches hello name you specified for hello **EntryPoint** property in hello pipeline JSON.</span></span> <span data-ttu-id="823a3-397">Hello forgatókönyv hello osztály neve van: MyDotNetActivity, illetve a hello JSON EntryPoint hello: MyDotNetActivityNS. **MyDotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="823a3-397">In hello walkthrough, name of hello class is: MyDotNetActivity, and hello EntryPoint in hello JSON is: MyDotNetActivityNS.**MyDotNetActivity**.</span></span>

    ```
    MyDotNetActivity assembly does not exist or doesn't implement hello type Microsoft.DataFactories.Runtime.IDotNetActivity properly
    ```

   <span data-ttu-id="823a3-398">Ha hello nevei egyeznek, ellenőrizze, hogy minden hello bináris fájlok a hello **gyökérmappa** hello zip-fájl.</span><span class="sxs-lookup"><span data-stu-id="823a3-398">If hello names do match, confirm that all hello binaries are in hello **root folder** of hello zip file.</span></span> <span data-ttu-id="823a3-399">Ez azt jelenti, hogy hello zip-fájl megnyitásakor, megtekintheti az összes hello fájlok hello gyökérmappában, és nem minden almappa.</span><span class="sxs-lookup"><span data-stu-id="823a3-399">That is, when you open hello zip file, you should see all hello files in hello root folder, not in any sub folders.</span></span>   
3. <span data-ttu-id="823a3-400">Ha hello bemeneti szelet értéke túl**készen**, ellenőrizze a bemeneti mappaszerkezet hello és **file.txt** hello bemeneti mappák szerepel.</span><span class="sxs-lookup"><span data-stu-id="823a3-400">If hello input slice is not set too**Ready**, confirm that hello input folder structure is correct and **file.txt** exists in hello input folders.</span></span>
3. <span data-ttu-id="823a3-401">A hello **Execute** az egyéni tevékenység használja hello metódusában **IActivityLogger** toolog objektuminformáció, amely segít a problémák elhárításához.</span><span class="sxs-lookup"><span data-stu-id="823a3-401">In hello **Execute** method of your custom activity, use hello **IActivityLogger** object toolog information that helps you troubleshoot issues.</span></span> <span data-ttu-id="823a3-402">hello felhasználói naplófájlok megjelennek a naplóba köszönőüzenetei (egy vagy több fájlt nevű: felhasználó-0.log, felhasználó-1.log, felhasználó-2.log, stb.).</span><span class="sxs-lookup"><span data-stu-id="823a3-402">hello logged messages show up in hello user log files (one or more files named: user-0.log, user-1.log, user-2.log, etc.).</span></span>

   <span data-ttu-id="823a3-403">A hello **OutputDataset** panelen hello szelet toosee hello kattintson **ADATSZELET** , hogy a szelet paneljét.</span><span class="sxs-lookup"><span data-stu-id="823a3-403">In hello **OutputDataset** blade, click hello slice toosee hello **DATA SLICE** blade for that slice.</span></span> <span data-ttu-id="823a3-404">Látni **tevékenységek** az adott szeletek.</span><span class="sxs-lookup"><span data-stu-id="823a3-404">You see **activity runs** for that slice.</span></span> <span data-ttu-id="823a3-405">Futtassa a hello szelet tevékenységenként kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="823a3-405">You should see one activity run for hello slice.</span></span> <span data-ttu-id="823a3-406">Hello parancs sávon kattintson a Futtatás, ha egy másik tevékenységgel, futtassa a hello elindíthatja azonos szelet.</span><span class="sxs-lookup"><span data-stu-id="823a3-406">If you click Run in hello command bar, you can start another activity run for hello same slice.</span></span>

   <span data-ttu-id="823a3-407">Ha hello tevékenységfuttatási gombra kattint, megjelenik az hello **tevékenység futtatása részletei** naplófájlok listáját tartalmazó panel.</span><span class="sxs-lookup"><span data-stu-id="823a3-407">When you click hello activity run, you see hello **ACTIVITY RUN DETAILS** blade with a list of log files.</span></span> <span data-ttu-id="823a3-408">Megjelenik a naplózott üzeneteket hello user_0.log fájlban.</span><span class="sxs-lookup"><span data-stu-id="823a3-408">You see logged messages in hello user_0.log file.</span></span> <span data-ttu-id="823a3-409">Ha hiba történik, megjelenik az három tevékenység fut mert hello újrapróbálkozások száma too3 hello csővezeték/tevékenységben JSON.</span><span class="sxs-lookup"><span data-stu-id="823a3-409">When an error occurs, you see three activity runs because hello retry count is set too3 in hello pipeline/activity JSON.</span></span> <span data-ttu-id="823a3-410">Ha hello tevékenységfuttatási gombra kattint, megjelenik az hello naplófájlokat, hogy áttekintheti tootroubleshoot hello hiba.</span><span class="sxs-lookup"><span data-stu-id="823a3-410">When you click hello activity run, you see hello log files that you can review tootroubleshoot hello error.</span></span>

   <span data-ttu-id="823a3-411">A naplófájlok hello listájában kattintson a hello **felhasználói-0.log**.</span><span class="sxs-lookup"><span data-stu-id="823a3-411">In hello list of log files, click hello **user-0.log**.</span></span> <span data-ttu-id="823a3-412">Jobb oldali panelen hello vannak hello segítségével hello eredményeit **IActivityLogger.Write** metódust.</span><span class="sxs-lookup"><span data-stu-id="823a3-412">In hello right panel are hello results of using hello **IActivityLogger.Write** method.</span></span> <span data-ttu-id="823a3-413">Ha az összes üzenet nem látható, ellenőrizze, hogy van-e további naplófájlokat nevű: user_1.log, user_2.log stb. Ellenkező esetben hello kód nem sikerült hello utolsó üzenet bejelentkezést követően.</span><span class="sxs-lookup"><span data-stu-id="823a3-413">If you don't see all messages, check if you have more log files named: user_1.log, user_2.log etc. Otherwise, hello code may have failed after hello last logged message.</span></span>

   <span data-ttu-id="823a3-414">Ezenkívül ellenőrizze **rendszer-0.log** rendszer hibaüzeneteket és kivételeket.</span><span class="sxs-lookup"><span data-stu-id="823a3-414">In addition, check **system-0.log** for any system error messages and exceptions.</span></span>
4. <span data-ttu-id="823a3-415">Hello tartalmaznak **PDB** hello zip-fájlban szereplő fájlt, hogy a hello hiba legutolsó részletes adatai információ például **hívási verem** Ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="823a3-415">Include hello **PDB** file in hello zip file so that hello error details have information such as **call stack** when an error occurs.</span></span>
5. <span data-ttu-id="823a3-416">Az egyéni tevékenység hello el kell érnie hello fájlok hello zip-fájlban szereplő összes hello **felső szintű** nem sub mappák.</span><span class="sxs-lookup"><span data-stu-id="823a3-416">All hello files in hello zip file for hello custom activity must be at hello **top level** with no sub folders.</span></span>
6. <span data-ttu-id="823a3-417">Győződjön meg arról, hogy hello **assemblyName** (MyDotNetActivity.dll) **entryPoint**(MyDotNetActivityNS.MyDotNetActivity) **packageFile** (customactivitycontainer / MyDotNetActivity.zip), és **packageLinkedService** (kell mutatnia toohello **általános célú**, amely hello zip-fájl tartalmazza az Azure blob-tároló) toocorrect értékek vannak beállítva.</span><span class="sxs-lookup"><span data-stu-id="823a3-417">Ensure that hello **assemblyName** (MyDotNetActivity.dll), **entryPoint**(MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer/MyDotNetActivity.zip), and **packageLinkedService** (should point toohello **general-purpose**Azure blob storage that contains hello zip file) are set toocorrect values.</span></span>
7. <span data-ttu-id="823a3-418">Ha rögzített egy hiba és a kívánt tooreprocess hello szelet, kattintson a jobb gombbal a hello hello szelet **OutputDataset** panel megnyitásához, és kattintson **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="823a3-418">If you fixed an error and want tooreprocess hello slice, right-click hello slice in hello **OutputDataset** blade and click **Run**.</span></span>
8. <span data-ttu-id="823a3-419">Ha megjelenik a következő hiba hello, hello Azure Storage csomag verziója > 4.3.0 használ.</span><span class="sxs-lookup"><span data-stu-id="823a3-419">If you see hello following error, you are using hello Azure Storage package of version > 4.3.0.</span></span> <span data-ttu-id="823a3-420">Data Factory szolgáltatás indítója windowsazure.Storage kifejezésre hello 4.3 verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="823a3-420">Data Factory service launcher requires hello 4.3 version of WindowsAzure.Storage.</span></span> <span data-ttu-id="823a3-421">Lásd: [Appdomain elkülönítési](#appdomain-isolation) szakasz egy munkahelyi körül hello használata Azure Storage-szerelvény újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="823a3-421">See [Appdomain isolation](#appdomain-isolation) section for a work-around if you must use hello later version of Azure Storage assembly.</span></span> 

    ```
    Error in Activity: Unknown error in module: System.Reflection.TargetInvocationException: Exception has been thrown by hello target of an invocation. ---> System.TypeLoadException: Could not load type 'Microsoft.WindowsAzure.Storage.Blob.CloudBlob' from assembly 'Microsoft.WindowsAzure.Storage, Version=4.3.0.0, Culture=neutral, 
    ```

    <span data-ttu-id="823a3-422">Azure Storage csomag hello 4.3.0 verziója használható, ha hello meglévő hivatkozás tooAzure tárolási csomag eltávolításához a verzió > 4.3.0.</span><span class="sxs-lookup"><span data-stu-id="823a3-422">If you can use hello 4.3.0 version of Azure Storage package, remove hello existing reference tooAzure Storage package of version > 4.3.0.</span></span> <span data-ttu-id="823a3-423">Ezután futtassa a következő parancsot a NuGet-Csomagkezelő konzolról hello.</span><span class="sxs-lookup"><span data-stu-id="823a3-423">Then, run hello following command from NuGet Package Manager Console.</span></span> 

    ```PowerShell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    <span data-ttu-id="823a3-424">Hello projekt felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="823a3-424">Build hello project.</span></span> <span data-ttu-id="823a3-425">A verzió > 4.3.0 Azure.Storage szerelvény hello bin\Debug mappa törlése.</span><span class="sxs-lookup"><span data-stu-id="823a3-425">Delete Azure.Storage assembly of version > 4.3.0 from hello bin\Debug folder.</span></span> <span data-ttu-id="823a3-426">Hozzon létre egy zip-fájl bináris fájljait és hello PDB-fájl.</span><span class="sxs-lookup"><span data-stu-id="823a3-426">Create a zip file with binaries and hello PDB file.</span></span> <span data-ttu-id="823a3-427">Cserélje le a régi zip-fájl hello hello blob-tárolóban (customactivitycontainer) erre.</span><span class="sxs-lookup"><span data-stu-id="823a3-427">Replace hello old zip file with this one in hello blob container (customactivitycontainer).</span></span> <span data-ttu-id="823a3-428">Futtassa újra a műveletet hello szeletek, melyeknél nem sikerült (kattintson a jobb gombbal a szelet, majd kattintson a Futtatás).</span><span class="sxs-lookup"><span data-stu-id="823a3-428">Rerun hello slices that failed (right-click slice, and click Run).</span></span>   
8. <span data-ttu-id="823a3-429">hello egyéni tevékenység nem használja a hello **app.config** fájlt a csomagból.</span><span class="sxs-lookup"><span data-stu-id="823a3-429">hello custom activity does not use hello **app.config** file from your package.</span></span> <span data-ttu-id="823a3-430">Ezért ha a kód a kapcsolati karakterláncok hello konfigurációs fájlból olvassa be, nem működik futásidőben.</span><span class="sxs-lookup"><span data-stu-id="823a3-430">Therefore, if your code reads any connection strings from hello configuration file, it does not work at runtime.</span></span> <span data-ttu-id="823a3-431">hello célszerű esetén használja az Azure Batch toohold bármely titkos kulcsainak egy **Azure KeyVault**, használja a tanúsítvány alapú szolgáltatás egyszerű tooprotect hello **keyvault**, és hello tanúsítvány terjesztése tooAzure Batch-készlet.</span><span class="sxs-lookup"><span data-stu-id="823a3-431">hello best practice when using Azure Batch is toohold any secrets in an **Azure KeyVault**, use a certificate-based service principal tooprotect hello **keyvault**, and distribute hello certificate tooAzure Batch pool.</span></span> <span data-ttu-id="823a3-432">hello .NET egyéni tevékenység majd el titkok hello KeyVault futásidőben.</span><span class="sxs-lookup"><span data-stu-id="823a3-432">hello .NET custom activity then can access secrets from hello KeyVault at runtime.</span></span> <span data-ttu-id="823a3-433">Ez a megoldás egy általános megoldás, és titkos kulcsot, nem csak a kapcsolati karakterlánc típusú tooany méretezheti.</span><span class="sxs-lookup"><span data-stu-id="823a3-433">This solution is a generic solution and can scale tooany type of secret, not just connection string.</span></span>

   <span data-ttu-id="823a3-434">Van egy egyszerűbb megoldást (de nem ajánlott): hozhat létre egy **Azure SQL társított szolgáltatásnak** rendelkező kapcsolatikarakterlánc-beállításokat, hozzon létre, hogy használ hello társított szolgáltatás, és típusú bemeneti adatkészletként lánc hello dataset adatkészlet Egyéni .NET tevékenység toohello.</span><span class="sxs-lookup"><span data-stu-id="823a3-434">There is an easier workaround (but not a best practice): you can create an **Azure SQL linked service** with connection string settings, create a dataset that uses hello linked service, and chain hello dataset as a dummy input dataset toohello custom .NET activity.</span></span> <span data-ttu-id="823a3-435">Ezután a hozzáférés hello kapcsolódó szolgáltatás kapcsolati karakterlánc hello egyéni tevékenység kódban.</span><span class="sxs-lookup"><span data-stu-id="823a3-435">You can then access hello linked service's connection string in hello custom activity code.</span></span>  

## <a name="update-custom-activity"></a><span data-ttu-id="823a3-436">Egyéni tevékenység frissítése</span><span class="sxs-lookup"><span data-stu-id="823a3-436">Update custom activity</span></span>
<span data-ttu-id="823a3-437">Hello kód hello egyéni tevékenység frissítésekor összeállítani, és új bináris toohello blob-tároló tartalmazó hello zip-fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="823a3-437">If you update hello code for hello custom activity, build it, and upload hello zip file that contains new binaries toohello blob storage.</span></span>

## <a name="appdomain-isolation"></a><span data-ttu-id="823a3-438">Az alkalmazástartomány elkülönítési</span><span class="sxs-lookup"><span data-stu-id="823a3-438">Appdomain isolation</span></span>
<span data-ttu-id="823a3-439">Lásd: [Cross AppDomain minta](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) , amely bemutatja, hogyan toocreate egyéni tevékenység, amely nem korlátozott hello adat-előállító indítója által használt tooassembly verziók (Példa: windowsazure.Storage kifejezésre v4.3.0, Newtonsoft.Json v6.0.x, stb.).</span><span class="sxs-lookup"><span data-stu-id="823a3-439">See [Cross AppDomain Sample](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) that shows you how toocreate a custom activity that is not constrained tooassembly versions used by hello Data Factory launcher (example: WindowsAzure.Storage v4.3.0, Newtonsoft.Json v6.0.x, etc.).</span></span>

## <a name="access-extended-properties"></a><span data-ttu-id="823a3-440">További tulajdonságok hozzáférés</span><span class="sxs-lookup"><span data-stu-id="823a3-440">Access extended properties</span></span>
<span data-ttu-id="823a3-441">Kiterjesztett tulajdonságok deklarálhatnak hello tevékenység JSON látható hello minta a következő módon:</span><span class="sxs-lookup"><span data-stu-id="823a3-441">You can declare extended properties in hello activity JSON as shown in hello following sample:</span></span>

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


<span data-ttu-id="823a3-442">Hello példában két további tulajdonságainak vannak: **SliceStart** és **DataFactoryName**.</span><span class="sxs-lookup"><span data-stu-id="823a3-442">In hello example, there are two extended properties: **SliceStart** and **DataFactoryName**.</span></span> <span data-ttu-id="823a3-443">hello értéke SliceStart hello SliceStart rendszerváltozó alapul.</span><span class="sxs-lookup"><span data-stu-id="823a3-443">hello value for SliceStart is based on hello SliceStart system variable.</span></span> <span data-ttu-id="823a3-444">Lásd: [rendszerváltozók](data-factory-functions-variables.md) támogatott rendszerváltozók listáját.</span><span class="sxs-lookup"><span data-stu-id="823a3-444">See [System Variables](data-factory-functions-variables.md) for a list of supported system variables.</span></span> <span data-ttu-id="823a3-445">hello DataFactoryName értéke kódolt tooCustomActivityFactory.</span><span class="sxs-lookup"><span data-stu-id="823a3-445">hello value for DataFactoryName is hard-coded tooCustomActivityFactory.</span></span>

<span data-ttu-id="823a3-446">tooaccess ezek kiterjesztett tulajdonságok hello **Execute** metódus használata kód hasonló toohello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="823a3-446">tooaccess these extended properties in hello **Execute** method, use code similar toohello following code:</span></span>

```csharp
// tooget extended properties (for example: SliceStart)
DotNetActivity dotNetActivity = (DotNetActivity)activity.TypeProperties;
string sliceStartString = dotNetActivity.ExtendedProperties["SliceStart"];

// toolog all extended properties                               
IDictionary<string, string> extendedProperties = dotNetActivity.ExtendedProperties;
logger.Write("Logging extended properties if any...");
foreach (KeyValuePair<string, string> entry in extendedProperties)
{
    logger.Write("<key:{0}> <value:{1}>", entry.Key, entry.Value);
}
```

## <a name="auto-scaling-of-azure-batch"></a><span data-ttu-id="823a3-447">Az Azure Batch automatikus skálázás</span><span class="sxs-lookup"><span data-stu-id="823a3-447">Auto-scaling of Azure Batch</span></span>
<span data-ttu-id="823a3-448">Az Azure Batch-készlet is létrehozhat **automatikus skálázás** szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="823a3-448">You can also create an Azure Batch pool with **autoscale** feature.</span></span> <span data-ttu-id="823a3-449">0 dedikált virtuális gépek és az automatikus skálázási képlet alapján a függőben lévő feladatok száma hello segítségével például létrehozhat egy azure batch-készlet.</span><span class="sxs-lookup"><span data-stu-id="823a3-449">For example, you could create an azure batch pool with 0 dedicated VMs and an autoscale formula based on hello number of pending tasks.</span></span> 

<span data-ttu-id="823a3-450">Itt hello minta képlet éri el a következő viselkedés hello: hello készlet létrehozásakor 1 virtuális gép kezdődik.</span><span class="sxs-lookup"><span data-stu-id="823a3-450">hello sample formula here achieves hello following behavior: When hello pool is initially created, it starts with 1 VM.</span></span> <span data-ttu-id="823a3-451">$PendingTasks metrika meghatározza hello feladatok futtatásának + (aszinkron) aktív állapotban.</span><span class="sxs-lookup"><span data-stu-id="823a3-451">$PendingTasks metric defines hello number of tasks in running + active (queued) state.</span></span>  <span data-ttu-id="823a3-452">hello képlet hello átlagos száma függőben lévő feladatok keresi hello az elmúlt 180 másodperc alatt, és ennek megfelelően állítja be TargetDedicated.</span><span class="sxs-lookup"><span data-stu-id="823a3-452">hello formula finds hello average number of pending tasks in hello last 180 seconds and sets TargetDedicated accordingly.</span></span> <span data-ttu-id="823a3-453">Biztosítja, hogy TargetDedicated soha nem túllép 25 virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="823a3-453">It ensures that TargetDedicated never goes beyond 25 VMs.</span></span> <span data-ttu-id="823a3-454">Igen új feladatok nyújtják, készlet automatikusan növekszik és feladatok befejezését, mint a virtuális gépek válik a szabad egyenként és hello automatikus skálázás zsugorítja a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="823a3-454">So, as new tasks are submitted, pool automatically grows and as tasks complete, VMs become free one by one and hello autoscaling shrinks those VMs.</span></span> <span data-ttu-id="823a3-455">módosított tooyour igényei lehetnek startingNumberOfVMs és maxNumberofVMs.</span><span class="sxs-lookup"><span data-stu-id="823a3-455">startingNumberOfVMs and maxNumberofVMs can be adjusted tooyour needs.</span></span>

<span data-ttu-id="823a3-456">Automatikus skálázás képlet:</span><span class="sxs-lookup"><span data-stu-id="823a3-456">Autoscale formula:</span></span>

``` 
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
```

<span data-ttu-id="823a3-457">Lásd: [automatikus méretezési számítási csomópontok az Azure Batch-készlet](../batch/batch-automatic-scaling.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="823a3-457">See [Automatically scale compute nodes in an Azure Batch pool](../batch/batch-automatic-scaling.md) for details.</span></span>

<span data-ttu-id="823a3-458">Ha hello készlet hello alapértelmezett [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), hello Batch szolgáltatás 15-30 perces tooprepare hello VM is beletelhet hello egyéni tevékenység futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="823a3-458">If hello pool is using hello default [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), hello Batch service could take 15-30 minutes tooprepare hello VM before running hello custom activity.</span></span>  <span data-ttu-id="823a3-459">Ha hello készletet használ egy másik autoScaleEvaluationInterval, hello Batch szolgáltatás autoScaleEvaluationInterval + 10 percet is beletelhet.</span><span class="sxs-lookup"><span data-stu-id="823a3-459">If hello pool is using a different autoScaleEvaluationInterval, hello Batch service could take autoScaleEvaluationInterval + 10 minutes.</span></span>

## <a name="use-hdinsight-compute-service"></a><span data-ttu-id="823a3-460">HDInsight számítási szolgáltatás használata</span><span class="sxs-lookup"><span data-stu-id="823a3-460">Use HDInsight compute service</span></span>
<span data-ttu-id="823a3-461">Hello forgatókönyv használt Azure Batch számítási toorun hello egyéni tevékenység.</span><span class="sxs-lookup"><span data-stu-id="823a3-461">In hello walkthrough, you used Azure Batch compute toorun hello custom activity.</span></span> <span data-ttu-id="823a3-462">Is használhatja a saját Windows-alapú HDInsight-fürt vagy adat-előállító igény szerinti Windows-alapú HDInsight-fürtöt, és futtassa a HDInsight-fürt hello hello egyéni tevékenység rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="823a3-462">You can also use your own Windows-based HDInsight cluster or have Data Factory create an on-demand Windows-based HDInsight cluster and have hello custom activity run on hello HDInsight cluster.</span></span> <span data-ttu-id="823a3-463">Az alábbiakban hello magas szintű lépései a HDInsight-fürtöt használ.</span><span class="sxs-lookup"><span data-stu-id="823a3-463">Here are hello high-level steps for using an HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="823a3-464">hello egyéni .NET tevékenységek futtatásához csak a Windows-alapú HDInsight-fürtökön.</span><span class="sxs-lookup"><span data-stu-id="823a3-464">hello custom .NET activities run only on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="823a3-465">Ez a korlátozás az áthidaló toouse hello térkép csökkentése tevékenység toorun egyéni Java-kód egy Linux-alapú HDInsight-fürtön.</span><span class="sxs-lookup"><span data-stu-id="823a3-465">A workaround for this limitation is toouse hello Map Reduce Activity toorun custom Java code on a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="823a3-466">Egy másik lehetőség a virtuális gépek toorun egyéni tevékenységeket a HDInsight-fürt használata helyett az Azure Batch-készlet toouse.</span><span class="sxs-lookup"><span data-stu-id="823a3-466">Another option is toouse an Azure Batch pool of VMs toorun custom activities instead of using a HDInsight cluster.</span></span>
 

1. <span data-ttu-id="823a3-467">Hozzon létre egy Azure HDInsight társított szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="823a3-467">Create an Azure HDInsight linked service.</span></span>   
2. <span data-ttu-id="823a3-468">Használja a HDInsight társított szolgáltatás helyett **AzureBatchLinkedService** hello a csővezeték-JSON.</span><span class="sxs-lookup"><span data-stu-id="823a3-468">Use HDInsight linked service in place of **AzureBatchLinkedService** in hello pipeline JSON.</span></span>

<span data-ttu-id="823a3-469">Ha azt szeretné, hogy tootest hello forgatókönyv, módosítsa a **start** és **end** alkalommal hello adatcsatorna, így hello Azure HDInsight szolgáltatás hello forgatókönyv teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="823a3-469">If you want tootest it with hello walkthrough, change **start** and **end** times for hello pipeline so that you can test hello scenario with hello Azure HDInsight service.</span></span>

#### <a name="create-azure-hdinsight-linked-service"></a><span data-ttu-id="823a3-470">Azure HDInsight társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="823a3-470">Create Azure HDInsight linked service</span></span>
<span data-ttu-id="823a3-471">hello Azure Data Factory szolgáltatásnak az igény szerinti fürt létrehozását támogatja, és használja úgy tooprocess bemeneti tooproduce kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="823a3-471">hello Azure Data Factory service supports creation of an on-demand cluster and use it tooprocess input tooproduce output data.</span></span> <span data-ttu-id="823a3-472">Használhatja a saját fürt tooperform hello azonos.</span><span class="sxs-lookup"><span data-stu-id="823a3-472">You can also use your own cluster tooperform hello same.</span></span> <span data-ttu-id="823a3-473">Igény szerinti HDInsight-fürt használata esetén a fürt minden egyes szeletre vonatkozó végrehajtásakor létrejön.</span><span class="sxs-lookup"><span data-stu-id="823a3-473">When you use on-demand HDInsight cluster, a cluster gets created for each slice.</span></span> <span data-ttu-id="823a3-474">Mivel a saját HDInsight-fürtöt használ, ha készen áll a fürt hello tooprocess azonnal hello szelet.</span><span class="sxs-lookup"><span data-stu-id="823a3-474">Whereas, if you use your own HDInsight cluster, hello cluster is ready tooprocess hello slice immediately.</span></span> <span data-ttu-id="823a3-475">Ezért ha igény-fürtöt használ, nem jelenhet meg hello kimeneti adatok leggyorsabban saját fürt használatakor.</span><span class="sxs-lookup"><span data-stu-id="823a3-475">Therefore, when you use on-demand cluster, you may not see hello output data as quickly as when you use your own cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="823a3-476">Futásidőben a .NET tevékenység példányának csak a HDInsight-fürt hello; egy munkavégző csomópontja fut nem lehet több csomóponton méretezett toorun.</span><span class="sxs-lookup"><span data-stu-id="823a3-476">At runtime, an instance of a .NET activity runs only on one worker node in hello HDInsight cluster; it cannot be scaled toorun on multiple nodes.</span></span> <span data-ttu-id="823a3-477">.NET tevékenység több példánya hello HDInsight-fürt csomópontjai párhuzamosan is futtathatók.</span><span class="sxs-lookup"><span data-stu-id="823a3-477">Multiple instances of .NET activity can run in parallel on different nodes of hello HDInsight cluster.</span></span>
>
>

##### <a name="toouse-an-on-demand-hdinsight-cluster"></a><span data-ttu-id="823a3-478">igény szerinti HDInsight-fürtök toouse</span><span class="sxs-lookup"><span data-stu-id="823a3-478">toouse an on-demand HDInsight cluster</span></span>
1. <span data-ttu-id="823a3-479">A hello **Azure-portálon**, kattintson a **fejlesztés és üzembe helyezés** a hello adat-előállító kezdőlapján.</span><span class="sxs-lookup"><span data-stu-id="823a3-479">In hello **Azure portal**, click **Author and Deploy** in hello Data Factory home page.</span></span>
2. <span data-ttu-id="823a3-480">A Data Factory Editor hello, kattintson **új számítási** hello parancs menüsoron, majd válassza a **igény szerinti HDInsight-fürt** hello menüből.</span><span class="sxs-lookup"><span data-stu-id="823a3-480">In hello Data Factory Editor, click **New compute** from hello command bar and select **On-demand HDInsight cluster** from hello menu.</span></span>
3. <span data-ttu-id="823a3-481">Hajtsa végre a következő módosításokat toohello JSON-parancsfájl hello:</span><span class="sxs-lookup"><span data-stu-id="823a3-481">Make hello following changes toohello JSON script:</span></span>

   1. <span data-ttu-id="823a3-482">A hello **nagyobbnak** tulajdonság, adja meg a HDInsight-fürt hello hello méretét.</span><span class="sxs-lookup"><span data-stu-id="823a3-482">For hello **clusterSize** property, specify hello size of hello HDInsight cluster.</span></span>
   2. <span data-ttu-id="823a3-483">A hello **timeToLive** tulajdonság, adja meg, hogy mennyi ideig hello ügyfél üresjáratban lehet, mielőtt törli.</span><span class="sxs-lookup"><span data-stu-id="823a3-483">For hello **timeToLive** property, specify how long hello customer can be idle before it is deleted.</span></span>
   3. <span data-ttu-id="823a3-484">A hello **verzió** tulajdonság, adja meg a kívánt toouse hello HDInsight-verzió.</span><span class="sxs-lookup"><span data-stu-id="823a3-484">For hello **version** property, specify hello HDInsight version you want toouse.</span></span> <span data-ttu-id="823a3-485">Ha kizárja a ezt a tulajdonságot, hello legújabb verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="823a3-485">If you exclude this property, hello latest version is used.</span></span>  
   4. <span data-ttu-id="823a3-486">A hello **linkedServiceName**, adja meg **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="823a3-486">For hello **linkedServiceName**, specify **AzureStorageLinkedService**.</span></span>

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
    > <span data-ttu-id="823a3-487">hello egyéni .NET tevékenységek futtatásához csak a Windows-alapú HDInsight-fürtökön.</span><span class="sxs-lookup"><span data-stu-id="823a3-487">hello custom .NET activities run only on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="823a3-488">Ez a korlátozás az áthidaló toouse hello térkép csökkentése tevékenység toorun egyéni Java-kód egy Linux-alapú HDInsight-fürtön.</span><span class="sxs-lookup"><span data-stu-id="823a3-488">A workaround for this limitation is toouse hello Map Reduce Activity toorun custom Java code on a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="823a3-489">Egy másik lehetőség a virtuális gépek toorun egyéni tevékenységeket a HDInsight-fürt használata helyett az Azure Batch-készlet toouse.</span><span class="sxs-lookup"><span data-stu-id="823a3-489">Another option is toouse an Azure Batch pool of VMs toorun custom activities instead of using a HDInsight cluster.</span></span>

4. <span data-ttu-id="823a3-490">Kattintson a **telepítés** a hello parancssávon toodeploy kapcsolódó hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="823a3-490">Click **Deploy** on hello command bar toodeploy hello linked service.</span></span>

##### <a name="toouse-your-own-hdinsight-cluster"></a><span data-ttu-id="823a3-491">toouse saját HDInsight-fürt:</span><span class="sxs-lookup"><span data-stu-id="823a3-491">toouse your own HDInsight cluster:</span></span>
1. <span data-ttu-id="823a3-492">A hello **Azure-portálon**, kattintson a **fejlesztés és üzembe helyezés** a hello adat-előállító kezdőlapján.</span><span class="sxs-lookup"><span data-stu-id="823a3-492">In hello **Azure portal**, click **Author and Deploy** in hello Data Factory home page.</span></span>
2. <span data-ttu-id="823a3-493">A hello **Data Factory Editor**, kattintson a **új számítási** hello parancs menüsoron, majd válassza a **HDInsight-fürt** hello menüből.</span><span class="sxs-lookup"><span data-stu-id="823a3-493">In hello **Data Factory Editor**, click **New compute** from hello command bar and select **HDInsight cluster** from hello menu.</span></span>
3. <span data-ttu-id="823a3-494">Hajtsa végre a következő módosításokat toohello JSON-parancsfájl hello:</span><span class="sxs-lookup"><span data-stu-id="823a3-494">Make hello following changes toohello JSON script:</span></span>

   1. <span data-ttu-id="823a3-495">A hello **clusterUri** tulajdonság, a HDInsight meg hello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="823a3-495">For hello **clusterUri** property, enter hello URL for your HDInsight.</span></span> <span data-ttu-id="823a3-496">Például: https://<clustername>.azurehdinsight.net/</span><span class="sxs-lookup"><span data-stu-id="823a3-496">For example: https://<clustername>.azurehdinsight.net/</span></span>     
   2. <span data-ttu-id="823a3-497">A hello **felhasználónév** tulajdonság, adja meg a hozzáférés toohello HDInsight-fürt rendelkező hello felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="823a3-497">For hello **UserName** property, enter hello user name who has access toohello HDInsight cluster.</span></span>
   3. <span data-ttu-id="823a3-498">A hello **jelszó** tulajdonság, írja be a hello jelszót hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="823a3-498">For hello **Password** property, enter hello password for hello user.</span></span>
   4. <span data-ttu-id="823a3-499">A hello **LinkedServiceName** tulajdonság, adja meg **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="823a3-499">For hello **LinkedServiceName** property, enter **AzureStorageLinkedService**.</span></span>
4. <span data-ttu-id="823a3-500">Kattintson a **telepítés** a hello parancssávon toodeploy kapcsolódó hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="823a3-500">Click **Deploy** on hello command bar toodeploy hello linked service.</span></span>

<span data-ttu-id="823a3-501">Lásd: [összekapcsolt szolgáltatások számítási](data-factory-compute-linked-services.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="823a3-501">See [Compute linked services](data-factory-compute-linked-services.md) for details.</span></span>

<span data-ttu-id="823a3-502">A hello **JSON-feldolgozási folyamat**, HDInsight használata (igény vagy a saját) társított szolgáltatáshoz:</span><span class="sxs-lookup"><span data-stu-id="823a3-502">In hello **pipeline JSON**, use HDInsight (on-demand or your own) linked service:</span></span>

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

## <a name="create-a-custom-activity-by-using-net-sdk"></a><span data-ttu-id="823a3-503">Egyéni tevékenység létrehozása .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="823a3-503">Create a custom activity by using .NET SDK</span></span>
<span data-ttu-id="823a3-504">Hello forgatókönyv ebben a cikkben akkor hozzon létre egy adat-előállító egy folyamatot, amely hello egyéni tevékenység használ hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="823a3-504">In hello walkthrough in this article, you create a data factory with a pipeline that uses hello custom activity by using hello Azure portal.</span></span> <span data-ttu-id="823a3-505">hello a következő kód bemutatja, hogyan toocreate hello helyette .NET SDK használatával adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="823a3-505">hello following code shows you how toocreate hello data factory by using .NET SDK instead.</span></span> <span data-ttu-id="823a3-506">SDK tooprogrammatically használatával kapcsolatos további részletekért hozzon létre adatcsatornák hello található [.NET API használatával hozzon létre egy folyamatot másolási tevékenység](data-factory-copy-activity-tutorial-using-dotnet-api.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="823a3-506">You can find more details about using SDK tooprogrammatically create pipelines in hello [create a pipeline with copy activity by using .NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md) article.</span></span> 

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

            // TODO: replace ADFTutorialResourceGroup with hello name of your resource group.
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

                            // Initial value for pipeline's active period. With this, you won't need tooset slice status
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

            throw new InvalidOperationException("Failed tooacquire token");
        }
    }
}
```

## <a name="debug-custom-activity-in-visual-studio"></a><span data-ttu-id="823a3-507">A Visual Studio egyéni tevékenység hibakeresése</span><span class="sxs-lookup"><span data-stu-id="823a3-507">Debug custom activity in Visual Studio</span></span>
<span data-ttu-id="823a3-508">Hello [Azure Data Factory - helyi környezetben](https://github.com/gbrueckl/Azure.DataFactory.LocalEnvironment) mintát a Githubon tartalmaz olyan eszköz, amely lehetővé teszi a toodebug egyéni .NET tevékenységeket a Visual studióban.</span><span class="sxs-lookup"><span data-stu-id="823a3-508">hello [Azure Data Factory - local environment](https://github.com/gbrueckl/Azure.DataFactory.LocalEnvironment) sample on GitHub includes a tool that allows you toodebug custom .NET activities within Visual Studio.</span></span>  


## <a name="sample-custom-activities-on-github"></a><span data-ttu-id="823a3-509">Egyéni tevékenységek mintát a Githubon</span><span class="sxs-lookup"><span data-stu-id="823a3-509">Sample custom activities on GitHub</span></span>
| <span data-ttu-id="823a3-510">Minta</span><span class="sxs-lookup"><span data-stu-id="823a3-510">Sample</span></span> | <span data-ttu-id="823a3-511">Milyen egyéni tevékenység does</span><span class="sxs-lookup"><span data-stu-id="823a3-511">What custom activity does</span></span> |
| --- | --- |
| <span data-ttu-id="823a3-512">[HTTP-adatok letöltő](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample).</span><span class="sxs-lookup"><span data-stu-id="823a3-512">[HTTP Data Downloader](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample).</span></span> |<span data-ttu-id="823a3-513">Letölti az adatokat a egy HTTP-végpont tooAzure egyéni tevékenység C# használatával a Data Factory Blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="823a3-513">Downloads data from an HTTP Endpoint tooAzure Blob Storage using custom C# Activity in Data Factory.</span></span> |
| [<span data-ttu-id="823a3-514">Twitter véleményeket elemzési minta</span><span class="sxs-lookup"><span data-stu-id="823a3-514">Twitter Sentiment Analysis sample</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |<span data-ttu-id="823a3-515">Meghívja az Azure ML modellje és do véleményeket elemzése, pontozási, előrejelzés stb.</span><span class="sxs-lookup"><span data-stu-id="823a3-515">Invokes an Azure ML model and do sentiment analysis, scoring, prediction etc.</span></span> |
| <span data-ttu-id="823a3-516">[R-parancsfájl futtatása](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).</span><span class="sxs-lookup"><span data-stu-id="823a3-516">[Run R Script](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).</span></span> |<span data-ttu-id="823a3-517">Meghívja az R-parancsfájl RScript.exe futtatásával a HDInsight-fürtre, amelyen már a R telepítve van rajta.</span><span class="sxs-lookup"><span data-stu-id="823a3-517">Invokes R script by running RScript.exe on your HDInsight cluster that already has R Installed on it.</span></span> |
| [<span data-ttu-id="823a3-518">Kereszt-AppDomain .NET tevékenység</span><span class="sxs-lookup"><span data-stu-id="823a3-518">Cross AppDomain .NET Activity</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) |<span data-ttu-id="823a3-519">Használja azokat, hello adat-előállító indítója által használt verziókat különböző szerelvény</span><span class="sxs-lookup"><span data-stu-id="823a3-519">Uses different assembly versions from ones used by hello Data Factory launcher</span></span> |
| [<span data-ttu-id="823a3-520">Újból feldolgozza a modellt az Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="823a3-520">Reprocess a model in Azure Analysis Services</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/AzureAnalysisServicesProcessSample) |  <span data-ttu-id="823a3-521">Feldolgozza a modellt az Azure Analysis Servicesben.</span><span class="sxs-lookup"><span data-stu-id="823a3-521">Reprocesses a model in Azure Analysis Services.</span></span> |

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
