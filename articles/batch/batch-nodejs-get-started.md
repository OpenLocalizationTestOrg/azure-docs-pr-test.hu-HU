---
title: "aaaTutorial - használata hello Azure Batch ügyféloldali kódtára a Node.js |} Microsoft Docs"
description: "Ismerje meg az Azure Batch hello alapvető fogalmait, és egy egyszerű Node.js segítségével megoldás kiépítését."
services: batch
author: shwetams
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: nodejs
ms.topic: hero-article
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: shwetams
ms.openlocfilehash: d2b0ecbe764e7100affd7b02839aef3077b073cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-batch-sdk-for-nodejs"></a><span data-ttu-id="da9cf-103">Ismerkedés a Node.js-hez készült Batch SDK-val</span><span class="sxs-lookup"><span data-stu-id="da9cf-103">Get started with Batch SDK for Node.js</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="da9cf-104">.NET</span><span class="sxs-lookup"><span data-stu-id="da9cf-104">.NET</span></span>](batch-dotnet-get-started.md)
> * [<span data-ttu-id="da9cf-105">Python</span><span class="sxs-lookup"><span data-stu-id="da9cf-105">Python</span></span>](batch-python-tutorial.md)
> * [<span data-ttu-id="da9cf-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="da9cf-106">Node.js</span></span>](batch-nodejs-get-started.md)
>
>

<span data-ttu-id="da9cf-107">A Node.js használatával kötegelt ügyfél kialakításának hello alapvető [Azure Batch Node.js SDK](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/).</span><span class="sxs-lookup"><span data-stu-id="da9cf-107">Learn hello basics of building a Batch client in Node.js using [Azure Batch Node.js SDK](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/).</span></span> <span data-ttu-id="da9cf-108">Részletes, lépésekre osztott módon ismerkedhet meg a Batch-alkalmazásokhoz tartozó forgatókönyvekkel és azok a Node.js-ügyfél használatával történő beállításával.</span><span class="sxs-lookup"><span data-stu-id="da9cf-108">We take a step by step approach of understanding a scenario for a batch application and then setting it up using a Node.js client.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="da9cf-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="da9cf-109">Prerequisites</span></span>
<span data-ttu-id="da9cf-110">Ez a cikk a Node.js és a Linux gyakorlati ismeretét feltételezi.</span><span class="sxs-lookup"><span data-stu-id="da9cf-110">This article assumes that you have a working knowledge of Node.js and familiarity with Linux.</span></span> <span data-ttu-id="da9cf-111">Azt is feltételezi, hogy rendelkezik-e az Azure-fiók hozzáférési jogok toocreate Batch- és tárolási szolgáltatások telepítése.</span><span class="sxs-lookup"><span data-stu-id="da9cf-111">It also assumes that you have an Azure account setup with access rights toocreate Batch and Storage services.</span></span>

<span data-ttu-id="da9cf-112">Azt javasoljuk, hogy olvasási [Azure Batch műszaki áttekintés](batch-technical-overview.md) előtt halad át a hello lépéseket ebben a cikkben leírt.</span><span class="sxs-lookup"><span data-stu-id="da9cf-112">We recommend reading [Azure Batch Technical Overview](batch-technical-overview.md) before you go through hello steps outlined this article.</span></span>

## <a name="hello-tutorial-scenario"></a><span data-ttu-id="da9cf-113">hello útmutató forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="da9cf-113">hello tutorial scenario</span></span>
<span data-ttu-id="da9cf-114">Ossza meg velünk megértéséhez hello kötegelt munkafolyamat forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="da9cf-114">Let us understand hello batch workflow scenario.</span></span> <span data-ttu-id="da9cf-115">Kell egy egyszerű-parancsfájl, amely letölti a Python minden csv egy Azure Blob storage tárolók fájljainak és tooJSON alakítja át.</span><span class="sxs-lookup"><span data-stu-id="da9cf-115">We have a simple script written in Python that downloads all csv files from an Azure Blob storage container and converts them tooJSON.</span></span> <span data-ttu-id="da9cf-116">tooprocess több tárolási fiók tárolók párhuzamosan, telepíthetünk egy Azure kötegelt hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="da9cf-116">tooprocess multiple storage account containers in parallel, we can deploy hello script as an Azure Batch job.</span></span>

## <a name="azure-batch-architecture"></a><span data-ttu-id="da9cf-117">Azure Batch-architektúra</span><span class="sxs-lookup"><span data-stu-id="da9cf-117">Azure Batch Architecture</span></span>
<span data-ttu-id="da9cf-118">hello következő ábra szemlélteti azt hogyan hello Python-parancsfájl segítségével történő Azure Batch és a Node.js-ügyfelet is méretezhető.</span><span class="sxs-lookup"><span data-stu-id="da9cf-118">hello following diagram depicts how we can scale hello Python script using Azure Batch and a Node.js client.</span></span>

![Azure Batch-forgatókönyv](./media/batch-nodejs-get-started/BatchScenario.png)

<span data-ttu-id="da9cf-120">hello node.js ügyfél telepíti egy kötegelt (viszonylag részletesen később) előkészítő feladat és feladatokhoz attól függően, hogy a tárfiók hello tárolók hello száma.</span><span class="sxs-lookup"><span data-stu-id="da9cf-120">hello node.js client deploys a batch job with a preparation task (explained in detail later) and a set of tasks depending on hello number of containers in hello storage account.</span></span> <span data-ttu-id="da9cf-121">Hello github-tárházban hello parancsfájlokat is letölthető.</span><span class="sxs-lookup"><span data-stu-id="da9cf-121">You can download hello scripts from hello github repository.</span></span>

* [<span data-ttu-id="da9cf-122">Node.js-ügyfél</span><span class="sxs-lookup"><span data-stu-id="da9cf-122">Node.js client</span></span>](https://github.com/Azure/azure-batch-samples/blob/master/Node.js/GettingStarted/nodejs_batch_client_sample.js)
* [<span data-ttu-id="da9cf-123">Előkészítési tevékenységhez kapcsolódó héjszkriptek</span><span class="sxs-lookup"><span data-stu-id="da9cf-123">Preparation task shell scripts</span></span>](https://github.com/Azure/azure-batch-samples/blob/master/Node.js/GettingStarted/startup_prereq.sh)
* [<span data-ttu-id="da9cf-124">Python csv tooJSON processzor</span><span class="sxs-lookup"><span data-stu-id="da9cf-124">Python csv tooJSON processor</span></span>](https://github.com/Azure/azure-batch-samples/blob/master/Node.js/GettingStarted/processcsv.py)

> [!TIP]
> <span data-ttu-id="da9cf-125">hello Node.js ügyfél hello hivatkozásban megadott nem tartalmaz adott kód toobe telepített Azure függvény alkalmazásként.</span><span class="sxs-lookup"><span data-stu-id="da9cf-125">hello Node.js client in hello link specified does not contain specific code toobe deployed as an Azure function app.</span></span> <span data-ttu-id="da9cf-126">Olvassa el a következő hivatkozások utasításokat toocreate egy toohello.</span><span class="sxs-lookup"><span data-stu-id="da9cf-126">You can refer toohello following links for instructions toocreate one.</span></span>
> - [<span data-ttu-id="da9cf-127">Függvényalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="da9cf-127">Create function app</span></span>](../azure-functions/functions-create-first-azure-function.md)
> - [<span data-ttu-id="da9cf-128">Időzítő által aktivált függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="da9cf-128">Create timer trigger function</span></span>](../azure-functions/functions-bindings-timer.md)
>
>

## <a name="build-hello-application"></a><span data-ttu-id="da9cf-129">Hello alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="da9cf-129">Build hello application</span></span>

<span data-ttu-id="da9cf-130">Most, ossza meg velünk hello eljárást követve lépésről lépésre történő kialakításának hello Node.js ügyfél:</span><span class="sxs-lookup"><span data-stu-id="da9cf-130">Now, let us follow hello process step by step into building hello Node.js client:</span></span>

### <a name="step-1-install-azure-batch-sdk"></a><span data-ttu-id="da9cf-131">1. lépés: Az Azure Batch SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="da9cf-131">Step 1: Install Azure Batch SDK</span></span>

<span data-ttu-id="da9cf-132">Azure Batch SDK for Node.js használatával hello npm telepítési parancs telepítése.</span><span class="sxs-lookup"><span data-stu-id="da9cf-132">You can install Azure Batch SDK for Node.js using hello npm install command.</span></span>

`npm install azure-batch`

<span data-ttu-id="da9cf-133">Ez a parancs hello azure-köteg csomópont SDK legújabb verzióját telepíti.</span><span class="sxs-lookup"><span data-stu-id="da9cf-133">This command installs hello latest version of azure-batch node SDK.</span></span>

>[!Tip]
> <span data-ttu-id="da9cf-134">Egy Azure-függvény alkalmazásban elvégezheti a túl "Kudu konzol" hello Azure függvény a beállítások lapon toorun hello npm telepítési parancsaival.</span><span class="sxs-lookup"><span data-stu-id="da9cf-134">In an Azure Function app, you can go too"Kudu Console" in hello Azure function's Settings tab toorun hello npm install commands.</span></span> <span data-ttu-id="da9cf-135">Az az eset tooinstall Azure Batch SDK for Node.js.</span><span class="sxs-lookup"><span data-stu-id="da9cf-135">In this case tooinstall Azure Batch SDK for Node.js.</span></span>
>
>

### <a name="step-2-create-an-azure-batch-account"></a><span data-ttu-id="da9cf-136">2. lépés: Azure Batch-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="da9cf-136">Step 2: Create an Azure Batch account</span></span>

<span data-ttu-id="da9cf-137">Létrehozhat a hello [Azure-portálon](batch-account-create-portal.md) vagy a parancssorból ([Powershell](batch-powershell-cmdlets-get-started.md) /[Azure cli](https://docs.microsoft.com/cli/azure/overview)).</span><span class="sxs-lookup"><span data-stu-id="da9cf-137">You can create it from hello [Azure portal](batch-account-create-portal.md) or from command line ([Powershell](batch-powershell-cmdlets-get-started.md) /[Azure cli](https://docs.microsoft.com/cli/azure/overview)).</span></span>

<span data-ttu-id="da9cf-138">Az alábbiakban hello parancsok toocreate egy Azure parancssori felületen keresztül.</span><span class="sxs-lookup"><span data-stu-id="da9cf-138">Following are hello commands toocreate one through Azure CLI.</span></span>

<span data-ttu-id="da9cf-139">Hozzon létre egy erőforráscsoportot, kihagyhatja ezt a lépést, ha már rendelkezik egy kívánt toocreate hello Batch-fiókhoz:</span><span class="sxs-lookup"><span data-stu-id="da9cf-139">Create a Resource Group, skip this step if you already have one where you want toocreate hello Batch Account:</span></span>

`az group create -n "<resource-group-name>" -l "<location>"`

<span data-ttu-id="da9cf-140">A következő lépés az Azure Batch-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="da9cf-140">Next, create an Azure Batch account.</span></span>

`az batch account create -l "<location>"  -g "<resource-group-name>" -n "<batch-account-name>"`

<span data-ttu-id="da9cf-141">Minden egyes Batch-fiók megfelelő hozzáférési kulcsokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="da9cf-141">Each Batch account has its corresponding access keys.</span></span> <span data-ttu-id="da9cf-142">Ezeket a kulcsokat olyan szükséges toocreate további erőforrások az Azure batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="da9cf-142">These keys are needed toocreate further resources in Azure batch account.</span></span> <span data-ttu-id="da9cf-143">Az éles környezetben célszerű van toouse Azure Key Vault toostore ezeket a kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="da9cf-143">A good practice for production environment is toouse Azure Key Vault toostore these keys.</span></span> <span data-ttu-id="da9cf-144">Ezután létrehozhat egy szolgáltatás egyszerű hello alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="da9cf-144">You can then create a Service principal for hello application.</span></span> <span data-ttu-id="da9cf-145">Ehhez a szolgáltatásalkalmazáshoz egyszerű hello segítségével hozhat létre az OAuth jogkivonat tooaccess kulcsok hello kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="da9cf-145">Using this service principal hello application can create an OAuth token tooaccess keys from hello key vault.</span></span>

`az batch account keys list -g "<resource-group-name>" -n "<batch-account-name>"`

<span data-ttu-id="da9cf-146">Másolja, és a későbbi lépésekben hello használt hello kulcs toobe tárolja.</span><span class="sxs-lookup"><span data-stu-id="da9cf-146">Copy and store hello key toobe used in hello subsequent steps.</span></span>

### <a name="step-3-create-an-azure-batch-service-client"></a><span data-ttu-id="da9cf-147">3. lépés: Azure Batch-szolgáltatásügyfél létrehozása</span><span class="sxs-lookup"><span data-stu-id="da9cf-147">Step 3: Create an Azure Batch service client</span></span>
<span data-ttu-id="da9cf-148">Következő kódrészletet először importálja az hello azure-köteg Node.js modult, és létrehozza a Batch szolgáltatás ügyfél.</span><span class="sxs-lookup"><span data-stu-id="da9cf-148">Following code snippet first imports hello azure-batch Node.js module and then creates a Batch Service client.</span></span> <span data-ttu-id="da9cf-149">Toofirst kell hozzon létre egy SharedKeyCredentials objektum hello hello előző lépésben másolt kötegelt a fiókkulcsot.</span><span class="sxs-lookup"><span data-stu-id="da9cf-149">You need toofirst create a SharedKeyCredentials object with hello Batch account key copied from hello previous step.</span></span>

```nodejs
// Initializing Azure Batch variables

var batch = require('azure-batch');

var accountName = '<azure-batch-account-name>';

var accountKey = '<account-key-downloaded>';

var accountUrl = '<account-url>'

// Create Batch credentials object using account name and account key

var credentials = new batch.SharedKeyCredentials(accountName,accountKey);

// Create Batch service client

var batch_client = new batch.ServiceClient(credentials,accountUrl);

```

<span data-ttu-id="da9cf-150">hello Azure Batch URI hello Azure-portálon hello áttekintése lapján található.</span><span class="sxs-lookup"><span data-stu-id="da9cf-150">hello Azure Batch URI can be found in hello Overview tab of hello Azure portal.</span></span> <span data-ttu-id="da9cf-151">Hello formátum van:</span><span class="sxs-lookup"><span data-stu-id="da9cf-151">It is of hello format:</span></span>

`https://accountname.location.batch.azure.com`

<span data-ttu-id="da9cf-152">Tekintse meg a toohello képernyőkép:</span><span class="sxs-lookup"><span data-stu-id="da9cf-152">Refer toohello screenshot:</span></span>

![Azure Batch URI](./media/batch-nodejs-get-started/azurebatchuri.png)



### <a name="step-4-create-an-azure-batch-pool"></a><span data-ttu-id="da9cf-154">4. lépés: Azure Batch-készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="da9cf-154">Step 4: Create an Azure Batch pool</span></span>
<span data-ttu-id="da9cf-155">Az Azure Batch-készlet több virtuális gépből áll (ezek Batch-csomópontokként is ismertek).</span><span class="sxs-lookup"><span data-stu-id="da9cf-155">An Azure Batch pool consists of multiple VMs (also known as Batch Nodes).</span></span> <span data-ttu-id="da9cf-156">Azure Batch szolgáltatás telepíti ezeket a csomópontokat hello feladatokat, és kezeli őket.</span><span class="sxs-lookup"><span data-stu-id="da9cf-156">Azure Batch service deploys hello tasks on these nodes and manages them.</span></span> <span data-ttu-id="da9cf-157">A következő konfigurációs paraméterek, a készlet hello adhat meg.</span><span class="sxs-lookup"><span data-stu-id="da9cf-157">You can define hello following configuration parameters for your pool.</span></span>

* <span data-ttu-id="da9cf-158">A virtuális gép rendszerképének típusa</span><span class="sxs-lookup"><span data-stu-id="da9cf-158">Type of Virtual Machine image</span></span>
* <span data-ttu-id="da9cf-159">A virtuálisgép-csomópontok métere</span><span class="sxs-lookup"><span data-stu-id="da9cf-159">Size of Virtual Machine nodes</span></span>
* <span data-ttu-id="da9cf-160">A virtuálisgép-csomópontok száma</span><span class="sxs-lookup"><span data-stu-id="da9cf-160">Number of Virtual Machine nodes</span></span>

> [!Tip]
> <span data-ttu-id="da9cf-161">hello méretét és a virtuális gép csomópontok száma nagy mértékben függ hello szeretné a toorun párhuzamosan is maga hello feladat feladatok száma.</span><span class="sxs-lookup"><span data-stu-id="da9cf-161">hello size and number of Virtual Machine nodes largely depend on hello number of tasks you want toorun in parallel and also hello task itself.</span></span> <span data-ttu-id="da9cf-162">Ajánlott a tesztelés toodetermine hello ideális számát és méretét.</span><span class="sxs-lookup"><span data-stu-id="da9cf-162">We recommend testing toodetermine hello ideal number and size.</span></span>
>
>

<span data-ttu-id="da9cf-163">hello következő kódrészletet hello konfigurációs paraméter-objektumokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="da9cf-163">hello following code snippet creates hello configuration parameter objects.</span></span>

```nodejs
// Creating Image reference configuration for Ubuntu Linux VM
var imgRef = {publisher:"Canonical",offer:"UbuntuServer",sku:"14.04.2-LTS",version:"latest"}

// Creating hello VM configuration object with hello SKUID
var vmconfig = {imageReference:imgRef,nodeAgentSKUId:"batch.node.ubuntu 14.04"}

// Setting hello VM size tooStandard F4
var vmSize = "STANDARD_F4"

//Setting number of VMs in hello pool too4
var numVMs = 4
```

> [!Tip]
> <span data-ttu-id="da9cf-164">Azure Batch és az SKU-azonosítók elérhető Linux Virtuálisgép-rendszerképek hello listájáért lásd: [virtuálisgép-rendszerképek listája](batch-linux-nodes.md#list-of-virtual-machine-images).</span><span class="sxs-lookup"><span data-stu-id="da9cf-164">For hello list of Linux VM images available for Azure Batch and their SKU IDs, see [List of virtual machine images](batch-linux-nodes.md#list-of-virtual-machine-images).</span></span>
>
>

<span data-ttu-id="da9cf-165">Miután hello készlet definiált, hello Azure Batch-készlet hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="da9cf-165">Once hello pool configuration is defined, you can create hello Azure Batch pool.</span></span> <span data-ttu-id="da9cf-166">hello kötegparancsban készletet hoz létre az Azure virtuális gép csomópontok, és felkészítheti őket toobe készen tooreceive feladatok tooexecute.</span><span class="sxs-lookup"><span data-stu-id="da9cf-166">hello Batch pool command creates Azure Virtual Machine nodes and prepares them toobe ready tooreceive tasks tooexecute.</span></span> <span data-ttu-id="da9cf-167">Mindegyik készletnek egyedi azonosítóval kell rendelkeznie a következő lépésekben történő hivatkozáshoz.</span><span class="sxs-lookup"><span data-stu-id="da9cf-167">Each pool should have a unique ID for reference in subsequent steps.</span></span>

<span data-ttu-id="da9cf-168">a következő kódrészletet hello hoz létre az Azure Batch-készlet.</span><span class="sxs-lookup"><span data-stu-id="da9cf-168">hello following code snippet creates an Azure Batch pool.</span></span>

```nodejs
// Create a unique Azure Batch pool ID
var poolid = "pool" + customerDetails.customerid;
var poolConfig = {id:poolid, displayName:poolid,vmSize:vmSize,virtualMachineConfiguration:vmconfig,targetDedicatedComputeNodes:numVms,enableAutoScale:false };
// Creating hello Pool for hello specific customer
var pool = batch_client.pool.add(poolConfig,function(error,result){
    if(error!=null){console.log(error.response)};
});
```

<span data-ttu-id="da9cf-169">Hello állapotának hello készlet létrehozása, és ügyeljen arra, hogy hello állapota "aktív" folytassa a feladat toothat készlet elküldése előtt.</span><span class="sxs-lookup"><span data-stu-id="da9cf-169">You can check hello status of hello pool created and ensure that hello state is in "active" before going ahead with submission of a Job toothat pool.</span></span>

```nodejs
var cloudPool = batch_client.pool.get(poolid,function(error,result,request,response){
        if(error == null)
        {

            if(result.state == "active")
            {
                console.log("Pool is active");
            }
        }
        else
        {
            if(error.statusCode==404)
            {
                console.log("Pool not found yet returned 404...");    

            }
            else
            {
                console.log("Error occurred while retrieving pool data");
            }
        }
        });
```

<span data-ttu-id="da9cf-170">Az alábbiakban látható egy minta eredményobjektum hello pool.get függvény által visszaadott.</span><span class="sxs-lookup"><span data-stu-id="da9cf-170">Following is a sample result object returned by hello pool.get function.</span></span>

```
{ id: 'processcsv_201721152',
  displayName: 'processcsv_201721152',
  url: 'https://<batch-account-name>.centralus.batch.azure.com/pools/processcsv_201721152',
  eTag: '<eTag>',
  lastModified: 2017-03-27T10:28:02.398Z,
  creationTime: 2017-03-27T10:28:02.398Z,
  state: 'active',
  stateTransitionTime: 2017-03-27T10:28:02.398Z,
  allocationState: 'resizing',
  allocationStateTransitionTime: 2017-03-27T10:28:02.398Z,
  vmSize: 'standard_a1',
  virtualMachineConfiguration:
   { imageReference:
      { publisher: 'Canonical',
        offer: 'UbuntuServer',
        sku: '14.04.2-LTS',
        version: 'latest' },
     nodeAgentSKUId: 'batch.node.ubuntu 14.04' },
  resizeTimeout:
   { [Number: 900000]
     _milliseconds: 900000,
     _days: 0,
     _months: 0,
     _data:
      { milliseconds: 0,
        seconds: 0,
        minutes: 15,
        hours: 0,
        days: 0,
        months: 0,
        years: 0 },
     _locale:
      Locale {
        _calendar: [Object],
        _longDateFormat: [Object],
        _invalidDate: 'Invalid date',
        ordinal: [Function: ordinal],
        _ordinalParse: /\d{1,2}(th|st|nd|rd)/,
        _relativeTime: [Object],
        _months: [Object],
        _monthsShort: [Object],
        _week: [Object],
        _weekdays: [Object],
        _weekdaysMin: [Object],
        _weekdaysShort: [Object],
        _meridiemParse: /[ap]\.?m?\.?/i,
        _abbr: 'en',
        _config: [Object],
        _ordinalParseLenient: /\d{1,2}(th|st|nd|rd)|\d{1,2}/ } },
  currentDedicated: 0,
  targetDedicated: 4,
  enableAutoScale: false,
  enableInterNodeCommunication: false,
  maxTasksPerNode: 1,
  taskSchedulingPolicy: { nodeFillType: 'Spread' } }
```


### <a name="step-4-submit-an-azure-batch-job"></a><span data-ttu-id="da9cf-171">4. lépés: Azure Batch-feladat elküldése</span><span class="sxs-lookup"><span data-stu-id="da9cf-171">Step 4: Submit an Azure Batch job</span></span>
<span data-ttu-id="da9cf-172">Az Azure Batch-feladatok hasonló feladatok logikai csoportjai.</span><span class="sxs-lookup"><span data-stu-id="da9cf-172">An Azure Batch job is a logical group of similar tasks.</span></span> <span data-ttu-id="da9cf-173">A mi esetünkben esetében "Folyamat csv tooJSON."</span><span class="sxs-lookup"><span data-stu-id="da9cf-173">In our scenario, it is "Process csv tooJSON."</span></span> <span data-ttu-id="da9cf-174">Minden egyes itt szereplő feladat képes az Azure Storage-tárolókban lévő CSV-fájlok feldolgozására.</span><span class="sxs-lookup"><span data-stu-id="da9cf-174">Each task here could be processing csv files present in each Azure Storage container.</span></span>

<span data-ttu-id="da9cf-175">Ezek a feladatok párhuzamosan futnak, és telepített hello Azure Batch szolgáltatás által összehangolva. több csomópont között.</span><span class="sxs-lookup"><span data-stu-id="da9cf-175">These tasks would run in parallel and deployed across multiple nodes, orchestrated by hello Azure Batch service.</span></span>

> [!Tip]
> <span data-ttu-id="da9cf-176">Használhatja a hello [maxTasksPerNode](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Pool.html#add) tulajdonság toospecify feladatok maximális száma, amelyek egyetlen csomóponton egyidejűleg is futtathatók.</span><span class="sxs-lookup"><span data-stu-id="da9cf-176">You can use hello [maxTasksPerNode](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Pool.html#add) property toospecify maximum number of tasks that can run concurrently on a single node.</span></span>
>
>

#### <a name="preparation-task"></a><span data-ttu-id="da9cf-177">Előkészítő feladat</span><span class="sxs-lookup"><span data-stu-id="da9cf-177">Preparation task</span></span>

<span data-ttu-id="da9cf-178">hello VM csomópontja létre üres Ubuntu csomópontok.</span><span class="sxs-lookup"><span data-stu-id="da9cf-178">hello VM nodes created are blank Ubuntu nodes.</span></span> <span data-ttu-id="da9cf-179">Programok tooinstall gyakran, előfeltételként szükséges.</span><span class="sxs-lookup"><span data-stu-id="da9cf-179">Often, you need tooinstall a set of programs as prerequisites.</span></span>
<span data-ttu-id="da9cf-180">Linux-csomópontok általában egy héjparancsfájlt is hello Előfeltételek hello tényleges feladatok futtatása előtt kell is.</span><span class="sxs-lookup"><span data-stu-id="da9cf-180">Typically, for Linux nodes you can have a shell script that installs hello prerequisites before hello actual tasks run.</span></span> <span data-ttu-id="da9cf-181">Ez azonban bármilyen programozható végrehajtható fájl lehet.</span><span class="sxs-lookup"><span data-stu-id="da9cf-181">However it could be any programmable executable.</span></span>
<span data-ttu-id="da9cf-182">Hello [rendszerhéj-parancsfájl](https://github.com/shwetams/azure-batchclient-sample-nodejs/blob/master/startup_prereq.sh) ebben a példában telepíti a Python-pip és hello Azure Storage szolgáltatás SDK Python.</span><span class="sxs-lookup"><span data-stu-id="da9cf-182">hello [shell script](https://github.com/shwetams/azure-batchclient-sample-nodejs/blob/master/startup_prereq.sh) in this example installs Python-pip and hello Azure Storage SDK for Python.</span></span>

<span data-ttu-id="da9cf-183">Egy Azure Storage-fiók hello parancsfájl feltöltése, és a SAS URI tooaccess hello parancsfájl létrehozása.</span><span class="sxs-lookup"><span data-stu-id="da9cf-183">You can upload hello script on an Azure Storage Account and generate a SAS URI tooaccess hello script.</span></span> <span data-ttu-id="da9cf-184">Ez a folyamat hello Azure Storage Node.js SDK használatával is automatikus.</span><span class="sxs-lookup"><span data-stu-id="da9cf-184">This process can also be automated using hello Azure Storage Node.js SDK.</span></span>

> [!Tip]
> <span data-ttu-id="da9cf-185">Csak csomópontján hello VM fusson a feladat előkészítése feladat amikor hello adott feladatot kell toorun.</span><span class="sxs-lookup"><span data-stu-id="da9cf-185">A preparation task for a job runs only on hello VM nodes where hello specific task needs toorun.</span></span> <span data-ttu-id="da9cf-186">Ha azt szeretné, hogy a rajta futó feladatok hello függetlenül összes csomópontján telepítve Előfeltételek toobe, használhatja a hello [startTask](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Pool.html#add) tulajdonság a készlet hozzáadása közben.</span><span class="sxs-lookup"><span data-stu-id="da9cf-186">If you want prerequisites toobe installed on all nodes irrespective of hello tasks that run on it, you can use hello [startTask](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Pool.html#add) property while adding a pool.</span></span> <span data-ttu-id="da9cf-187">Előkészítő feladat meghatározása referenciaként a következő hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="da9cf-187">You can use hello following preparation task definition for reference.</span></span>
>
>

<span data-ttu-id="da9cf-188">Előkészítő feladat végzett leadásakor hello Azure kötegelt van megadva.</span><span class="sxs-lookup"><span data-stu-id="da9cf-188">A preparation task is specified during hello submission of Azure Batch job.</span></span> <span data-ttu-id="da9cf-189">Következő előkészítő feladat konfigurációs paraméterek vannak hello:</span><span class="sxs-lookup"><span data-stu-id="da9cf-189">Following are hello preparation task configuration parameters:</span></span>

* <span data-ttu-id="da9cf-190">**Azonosító**: hello előkészítése tevékenység egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="da9cf-190">**ID**: A unique identifier for hello preparation task</span></span>
* <span data-ttu-id="da9cf-191">**commandLine**: parancssor tooexecute hello végrehajtható feladat</span><span class="sxs-lookup"><span data-stu-id="da9cf-191">**commandLine**: Command line tooexecute hello task executable</span></span>
* <span data-ttu-id="da9cf-192">**resourceFiles**: a fájlok részleteit objektumokból álló tömb le ez a feladat toorun toobe szükséges.</span><span class="sxs-lookup"><span data-stu-id="da9cf-192">**resourceFiles**: Array of objects that provide details of files needed toobe downloaded for this task toorun.</span></span>  <span data-ttu-id="da9cf-193">Ennek beállítási lehetőségei a következők:</span><span class="sxs-lookup"><span data-stu-id="da9cf-193">Following are its options</span></span>
    - <span data-ttu-id="da9cf-194">blobSource: hello SAS URI-jának hello fájl</span><span class="sxs-lookup"><span data-stu-id="da9cf-194">blobSource: hello SAS URI of hello file</span></span>
    - <span data-ttu-id="da9cf-195">fájl elérési útja: helyi elérési út toodownload és hello fájl mentése</span><span class="sxs-lookup"><span data-stu-id="da9cf-195">filePath: Local path toodownload and save hello file</span></span>
    - <span data-ttu-id="da9cf-196">fileMode: Csak Linux-csomópontokra vonatkozik, a fileMode formátuma oktális, alapértelmezett értéke 0770.</span><span class="sxs-lookup"><span data-stu-id="da9cf-196">fileMode: Only applicable for Linux nodes, fileMode is in octal format with a default value of 0770</span></span>
* <span data-ttu-id="da9cf-197">**waitForSuccess**: Ha set tootrue, hello feladat nem futtatható előkészítő feladat hibáihoz</span><span class="sxs-lookup"><span data-stu-id="da9cf-197">**waitForSuccess**: If set tootrue, hello task does not run on preparation task failures</span></span>
* <span data-ttu-id="da9cf-198">**runElevated**: állítsa be tootrue emelt szintű jogosultságokkal szükséges toorun hello feladat esetén.</span><span class="sxs-lookup"><span data-stu-id="da9cf-198">**runElevated**: Set it tootrue if elevated privileges are needed toorun hello task.</span></span>

<span data-ttu-id="da9cf-199">Következő kódrészletet hello előkészítő feladat parancsfájl konfigurációs minta mutatja:</span><span class="sxs-lookup"><span data-stu-id="da9cf-199">Following code snippet shows hello preparation task script configuration sample:</span></span>

```nodejs
var job_prep_task_config = {id:"installprereq",commandLine:"sudo sh startup_prereq.sh > startup.log",resourceFiles:[{'blobSource':'Blob SAS URI','filePath':'startup_prereq.sh'}],waitForSuccess:true,runElevated:true}
```

<span data-ttu-id="da9cf-200">Ha nincs telepítve a feladatok toorun Előfeltételek toobe, kihagyhatja hello előkészítő feladatok.</span><span class="sxs-lookup"><span data-stu-id="da9cf-200">If there are no prerequisites toobe installed for your tasks toorun, you can skip hello preparation tasks.</span></span> <span data-ttu-id="da9cf-201">Az alábbi kód „CSV-fájlok feldolgozása” megjelenített névvel ellátott feladatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="da9cf-201">Following code creates a job with display name "process csv files."</span></span>

 ```nodejs
 // Setting up Batch pool configuration
 var pool_config = {poolId:poolid}
 // Setting up Job configuration along with preparation task
 var jobId = "processcsvjob"
 var job_config = {id:jobId,displayName:"process csv files",jobPreparationTask:job_prep_task_config,poolInfo:pool_config}
 // Adding Azure batch job toohello pool
 var job = batch_client.job.add(job_config,function(error,result){
     if(error != null)
     {
         console.log("Error submitting job : " + error.response);
     }});
```


### <a name="step-5-submit-azure-batch-tasks-for-a-job"></a><span data-ttu-id="da9cf-202">5. lépés: Azure Batch-feladatok elküldése egy feladathoz</span><span class="sxs-lookup"><span data-stu-id="da9cf-202">Step 5: Submit Azure Batch tasks for a job</span></span>

<span data-ttu-id="da9cf-203">A CSV-feldolgozási feladat létrehozását követően hozzunk létre tevékenységeket ehhez a feladathoz.</span><span class="sxs-lookup"><span data-stu-id="da9cf-203">Now that our process csv job is created, let us create tasks for that job.</span></span> <span data-ttu-id="da9cf-204">Ha tudunk négy tárolók, tudunk toocreate négy feladatok, minden egyes tároló egyik.</span><span class="sxs-lookup"><span data-stu-id="da9cf-204">Assuming we have four containers, we have toocreate four tasks, one for each container.</span></span>

<span data-ttu-id="da9cf-205">Ha úgy tekintünk hello [Python-parancsfájl](https://github.com/shwetams/azure-batchclient-sample-nodejs/blob/master/processcsv.py), két paramétert fogad:</span><span class="sxs-lookup"><span data-stu-id="da9cf-205">If we look at hello [Python script](https://github.com/shwetams/azure-batchclient-sample-nodejs/blob/master/processcsv.py), it accepts two parameters:</span></span>

* <span data-ttu-id="da9cf-206">tároló neve: hello tárolási tároló toodownload fájlok</span><span class="sxs-lookup"><span data-stu-id="da9cf-206">container name: hello Storage container toodownload files from</span></span>
* <span data-ttu-id="da9cf-207">pattern: A fájlnév-minta választható paramétere.</span><span class="sxs-lookup"><span data-stu-id="da9cf-207">pattern: An optional parameter of file name pattern</span></span>

<span data-ttu-id="da9cf-208">Ha tudunk négy tárolók "con1", "con2", "con3", "con4" következő kódot feladatok toohello az Azure batch feladat "folyamat fürt megosztott kötetei szolgáltatás" korábban létrehozott elküldése jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="da9cf-208">Assuming we have four containers "con1", "con2", "con3","con4" following code shows submitting for tasks toohello Azure batch job "process csv" we created earlier.</span></span>

```nodejs
// storing container names in an array
var container_list = ["con1","con2","con3","con4"]
    container_list.forEach(function(val,index){           

           var container_name = val;
           var taskID = container_name + "_process";
           var task_config = {id:taskID,displayName:'process csv in ' + container_name,commandLine:'python processcsv.py --container ' + container_name,resourceFiles:[{'blobSource':'<blob SAS URI>','filePath':'processcsv.py'}]}
           var task = batch_client.task.add(poolid,task_config,function(error,result){
                if(error != null)
                {
                    console.log(error.response);     
                }
                else
                {
                    console.log("Task for container : " + container_name + "submitted successfully");
                }



           });

    });
```

<span data-ttu-id="da9cf-209">hello kód több feladatok toohello készletet ad hozzá.</span><span class="sxs-lookup"><span data-stu-id="da9cf-209">hello code adds multiple tasks toohello pool.</span></span> <span data-ttu-id="da9cf-210">És minden hello feladatok végrehajtása hello készlet létrehozott virtuális gépek egyik csomópontján.</span><span class="sxs-lookup"><span data-stu-id="da9cf-210">And each of hello tasks is executed on a node in hello pool of VMs created.</span></span> <span data-ttu-id="da9cf-211">Hello feladatok száma meghaladja a hello virtuális gépek számát a készlet vagy hello maxTasksPerNode tulajdonság, ha a hello feladatok Várjon, amíg a csomópont szeretné elérhetővé tenni.</span><span class="sxs-lookup"><span data-stu-id="da9cf-211">If hello number of tasks exceeds hello number of VMs in a pool or hello maxTasksPerNode property, hello tasks wait until a node is made available.</span></span> <span data-ttu-id="da9cf-212">Ennek vezénylését az Azure Batch automatikusan elvégzi.</span><span class="sxs-lookup"><span data-stu-id="da9cf-212">This orchestration is handled by Azure Batch automatically.</span></span>

<span data-ttu-id="da9cf-213">hello portál rendelkezik részletes nézeteket hello feladatok és a feladatok állapota.</span><span class="sxs-lookup"><span data-stu-id="da9cf-213">hello portal has detailed views on hello tasks and job statuses.</span></span> <span data-ttu-id="da9cf-214">Is hello a listában, és lekérése funkciók hello Azure csomópont SDK-t.</span><span class="sxs-lookup"><span data-stu-id="da9cf-214">You can also use hello list and get functions in hello Azure Node SDK.</span></span> <span data-ttu-id="da9cf-215">Részletek szerepelnek hello dokumentáció [hivatkozás](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Job.html).</span><span class="sxs-lookup"><span data-stu-id="da9cf-215">Details are provided in hello documentation [link](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Job.html).</span></span>

## <a name="next-steps"></a><span data-ttu-id="da9cf-216">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="da9cf-216">Next steps</span></span>

- <span data-ttu-id="da9cf-217">Felülvizsgálati hello [áttekintés az Azure Batch funkcióinak](batch-api-basics.md) cikk, amely ajánlott, ha még új toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="da9cf-217">Review hello [Overview of Azure Batch features](batch-api-basics.md) article, which we recommend if you're new toohello service.</span></span>
- <span data-ttu-id="da9cf-218">Lásd: hello [kötegelt Node.js hivatkozás](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/) tooexplore hello kötegelt API.</span><span class="sxs-lookup"><span data-stu-id="da9cf-218">See hello [Batch Node.js reference](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/) tooexplore hello Batch API.</span></span>

