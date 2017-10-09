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
# <a name="get-started-with-batch-sdk-for-nodejs"></a>Ismerkedés a Node.js-hez készült Batch SDK-val

> [!div class="op_single_selector"]
> * [.NET](batch-dotnet-get-started.md)
> * [Python](batch-python-tutorial.md)
> * [Node.js](batch-nodejs-get-started.md)
>
>

A Node.js használatával kötegelt ügyfél kialakításának hello alapvető [Azure Batch Node.js SDK](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/). Részletes, lépésekre osztott módon ismerkedhet meg a Batch-alkalmazásokhoz tartozó forgatókönyvekkel és azok a Node.js-ügyfél használatával történő beállításával.  

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk a Node.js és a Linux gyakorlati ismeretét feltételezi. Azt is feltételezi, hogy rendelkezik-e az Azure-fiók hozzáférési jogok toocreate Batch- és tárolási szolgáltatások telepítése.

Azt javasoljuk, hogy olvasási [Azure Batch műszaki áttekintés](batch-technical-overview.md) előtt halad át a hello lépéseket ebben a cikkben leírt.

## <a name="hello-tutorial-scenario"></a>hello útmutató forgatókönyv
Ossza meg velünk megértéséhez hello kötegelt munkafolyamat forgatókönyv. Kell egy egyszerű-parancsfájl, amely letölti a Python minden csv egy Azure Blob storage tárolók fájljainak és tooJSON alakítja át. tooprocess több tárolási fiók tárolók párhuzamosan, telepíthetünk egy Azure kötegelt hello parancsfájl.

## <a name="azure-batch-architecture"></a>Azure Batch-architektúra
hello következő ábra szemlélteti azt hogyan hello Python-parancsfájl segítségével történő Azure Batch és a Node.js-ügyfelet is méretezhető.

![Azure Batch-forgatókönyv](./media/batch-nodejs-get-started/BatchScenario.png)

hello node.js ügyfél telepíti egy kötegelt (viszonylag részletesen később) előkészítő feladat és feladatokhoz attól függően, hogy a tárfiók hello tárolók hello száma. Hello github-tárházban hello parancsfájlokat is letölthető.

* [Node.js-ügyfél](https://github.com/Azure/azure-batch-samples/blob/master/Node.js/GettingStarted/nodejs_batch_client_sample.js)
* [Előkészítési tevékenységhez kapcsolódó héjszkriptek](https://github.com/Azure/azure-batch-samples/blob/master/Node.js/GettingStarted/startup_prereq.sh)
* [Python csv tooJSON processzor](https://github.com/Azure/azure-batch-samples/blob/master/Node.js/GettingStarted/processcsv.py)

> [!TIP]
> hello Node.js ügyfél hello hivatkozásban megadott nem tartalmaz adott kód toobe telepített Azure függvény alkalmazásként. Olvassa el a következő hivatkozások utasításokat toocreate egy toohello.
> - [Függvényalkalmazás létrehozása](../azure-functions/functions-create-first-azure-function.md)
> - [Időzítő által aktivált függvény létrehozása](../azure-functions/functions-bindings-timer.md)
>
>

## <a name="build-hello-application"></a>Hello alkalmazás létrehozása

Most, ossza meg velünk hello eljárást követve lépésről lépésre történő kialakításának hello Node.js ügyfél:

### <a name="step-1-install-azure-batch-sdk"></a>1. lépés: Az Azure Batch SDK telepítése

Azure Batch SDK for Node.js használatával hello npm telepítési parancs telepítése.

`npm install azure-batch`

Ez a parancs hello azure-köteg csomópont SDK legújabb verzióját telepíti.

>[!Tip]
> Egy Azure-függvény alkalmazásban elvégezheti a túl "Kudu konzol" hello Azure függvény a beállítások lapon toorun hello npm telepítési parancsaival. Az az eset tooinstall Azure Batch SDK for Node.js.
>
>

### <a name="step-2-create-an-azure-batch-account"></a>2. lépés: Azure Batch-fiók létrehozása

Létrehozhat a hello [Azure-portálon](batch-account-create-portal.md) vagy a parancssorból ([Powershell](batch-powershell-cmdlets-get-started.md) /[Azure cli](https://docs.microsoft.com/cli/azure/overview)).

Az alábbiakban hello parancsok toocreate egy Azure parancssori felületen keresztül.

Hozzon létre egy erőforráscsoportot, kihagyhatja ezt a lépést, ha már rendelkezik egy kívánt toocreate hello Batch-fiókhoz:

`az group create -n "<resource-group-name>" -l "<location>"`

A következő lépés az Azure Batch-fiók létrehozása.

`az batch account create -l "<location>"  -g "<resource-group-name>" -n "<batch-account-name>"`

Minden egyes Batch-fiók megfelelő hozzáférési kulcsokkal rendelkezik. Ezeket a kulcsokat olyan szükséges toocreate további erőforrások az Azure batch-fiókhoz. Az éles környezetben célszerű van toouse Azure Key Vault toostore ezeket a kulcsokat. Ezután létrehozhat egy szolgáltatás egyszerű hello alkalmazáshoz. Ehhez a szolgáltatásalkalmazáshoz egyszerű hello segítségével hozhat létre az OAuth jogkivonat tooaccess kulcsok hello kulcstároló.

`az batch account keys list -g "<resource-group-name>" -n "<batch-account-name>"`

Másolja, és a későbbi lépésekben hello használt hello kulcs toobe tárolja.

### <a name="step-3-create-an-azure-batch-service-client"></a>3. lépés: Azure Batch-szolgáltatásügyfél létrehozása
Következő kódrészletet először importálja az hello azure-köteg Node.js modult, és létrehozza a Batch szolgáltatás ügyfél. Toofirst kell hozzon létre egy SharedKeyCredentials objektum hello hello előző lépésben másolt kötegelt a fiókkulcsot.

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

hello Azure Batch URI hello Azure-portálon hello áttekintése lapján található. Hello formátum van:

`https://accountname.location.batch.azure.com`

Tekintse meg a toohello képernyőkép:

![Azure Batch URI](./media/batch-nodejs-get-started/azurebatchuri.png)



### <a name="step-4-create-an-azure-batch-pool"></a>4. lépés: Azure Batch-készlet létrehozása
Az Azure Batch-készlet több virtuális gépből áll (ezek Batch-csomópontokként is ismertek). Azure Batch szolgáltatás telepíti ezeket a csomópontokat hello feladatokat, és kezeli őket. A következő konfigurációs paraméterek, a készlet hello adhat meg.

* A virtuális gép rendszerképének típusa
* A virtuálisgép-csomópontok métere
* A virtuálisgép-csomópontok száma

> [!Tip]
> hello méretét és a virtuális gép csomópontok száma nagy mértékben függ hello szeretné a toorun párhuzamosan is maga hello feladat feladatok száma. Ajánlott a tesztelés toodetermine hello ideális számát és méretét.
>
>

hello következő kódrészletet hello konfigurációs paraméter-objektumokat hoz létre.

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
> Azure Batch és az SKU-azonosítók elérhető Linux Virtuálisgép-rendszerképek hello listájáért lásd: [virtuálisgép-rendszerképek listája](batch-linux-nodes.md#list-of-virtual-machine-images).
>
>

Miután hello készlet definiált, hello Azure Batch-készlet hozhatók létre. hello kötegparancsban készletet hoz létre az Azure virtuális gép csomópontok, és felkészítheti őket toobe készen tooreceive feladatok tooexecute. Mindegyik készletnek egyedi azonosítóval kell rendelkeznie a következő lépésekben történő hivatkozáshoz.

a következő kódrészletet hello hoz létre az Azure Batch-készlet.

```nodejs
// Create a unique Azure Batch pool ID
var poolid = "pool" + customerDetails.customerid;
var poolConfig = {id:poolid, displayName:poolid,vmSize:vmSize,virtualMachineConfiguration:vmconfig,targetDedicatedComputeNodes:numVms,enableAutoScale:false };
// Creating hello Pool for hello specific customer
var pool = batch_client.pool.add(poolConfig,function(error,result){
    if(error!=null){console.log(error.response)};
});
```

Hello állapotának hello készlet létrehozása, és ügyeljen arra, hogy hello állapota "aktív" folytassa a feladat toothat készlet elküldése előtt.

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

Az alábbiakban látható egy minta eredményobjektum hello pool.get függvény által visszaadott.

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


### <a name="step-4-submit-an-azure-batch-job"></a>4. lépés: Azure Batch-feladat elküldése
Az Azure Batch-feladatok hasonló feladatok logikai csoportjai. A mi esetünkben esetében "Folyamat csv tooJSON." Minden egyes itt szereplő feladat képes az Azure Storage-tárolókban lévő CSV-fájlok feldolgozására.

Ezek a feladatok párhuzamosan futnak, és telepített hello Azure Batch szolgáltatás által összehangolva. több csomópont között.

> [!Tip]
> Használhatja a hello [maxTasksPerNode](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Pool.html#add) tulajdonság toospecify feladatok maximális száma, amelyek egyetlen csomóponton egyidejűleg is futtathatók.
>
>

#### <a name="preparation-task"></a>Előkészítő feladat

hello VM csomópontja létre üres Ubuntu csomópontok. Programok tooinstall gyakran, előfeltételként szükséges.
Linux-csomópontok általában egy héjparancsfájlt is hello Előfeltételek hello tényleges feladatok futtatása előtt kell is. Ez azonban bármilyen programozható végrehajtható fájl lehet.
Hello [rendszerhéj-parancsfájl](https://github.com/shwetams/azure-batchclient-sample-nodejs/blob/master/startup_prereq.sh) ebben a példában telepíti a Python-pip és hello Azure Storage szolgáltatás SDK Python.

Egy Azure Storage-fiók hello parancsfájl feltöltése, és a SAS URI tooaccess hello parancsfájl létrehozása. Ez a folyamat hello Azure Storage Node.js SDK használatával is automatikus.

> [!Tip]
> Csak csomópontján hello VM fusson a feladat előkészítése feladat amikor hello adott feladatot kell toorun. Ha azt szeretné, hogy a rajta futó feladatok hello függetlenül összes csomópontján telepítve Előfeltételek toobe, használhatja a hello [startTask](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Pool.html#add) tulajdonság a készlet hozzáadása közben. Előkészítő feladat meghatározása referenciaként a következő hello is használhatja.
>
>

Előkészítő feladat végzett leadásakor hello Azure kötegelt van megadva. Következő előkészítő feladat konfigurációs paraméterek vannak hello:

* **Azonosító**: hello előkészítése tevékenység egyedi azonosítója
* **commandLine**: parancssor tooexecute hello végrehajtható feladat
* **resourceFiles**: a fájlok részleteit objektumokból álló tömb le ez a feladat toorun toobe szükséges.  Ennek beállítási lehetőségei a következők:
    - blobSource: hello SAS URI-jának hello fájl
    - fájl elérési útja: helyi elérési út toodownload és hello fájl mentése
    - fileMode: Csak Linux-csomópontokra vonatkozik, a fileMode formátuma oktális, alapértelmezett értéke 0770.
* **waitForSuccess**: Ha set tootrue, hello feladat nem futtatható előkészítő feladat hibáihoz
* **runElevated**: állítsa be tootrue emelt szintű jogosultságokkal szükséges toorun hello feladat esetén.

Következő kódrészletet hello előkészítő feladat parancsfájl konfigurációs minta mutatja:

```nodejs
var job_prep_task_config = {id:"installprereq",commandLine:"sudo sh startup_prereq.sh > startup.log",resourceFiles:[{'blobSource':'Blob SAS URI','filePath':'startup_prereq.sh'}],waitForSuccess:true,runElevated:true}
```

Ha nincs telepítve a feladatok toorun Előfeltételek toobe, kihagyhatja hello előkészítő feladatok. Az alábbi kód „CSV-fájlok feldolgozása” megjelenített névvel ellátott feladatot hoz létre.

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


### <a name="step-5-submit-azure-batch-tasks-for-a-job"></a>5. lépés: Azure Batch-feladatok elküldése egy feladathoz

A CSV-feldolgozási feladat létrehozását követően hozzunk létre tevékenységeket ehhez a feladathoz. Ha tudunk négy tárolók, tudunk toocreate négy feladatok, minden egyes tároló egyik.

Ha úgy tekintünk hello [Python-parancsfájl](https://github.com/shwetams/azure-batchclient-sample-nodejs/blob/master/processcsv.py), két paramétert fogad:

* tároló neve: hello tárolási tároló toodownload fájlok
* pattern: A fájlnév-minta választható paramétere.

Ha tudunk négy tárolók "con1", "con2", "con3", "con4" következő kódot feladatok toohello az Azure batch feladat "folyamat fürt megosztott kötetei szolgáltatás" korábban létrehozott elküldése jeleníti meg.

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

hello kód több feladatok toohello készletet ad hozzá. És minden hello feladatok végrehajtása hello készlet létrehozott virtuális gépek egyik csomópontján. Hello feladatok száma meghaladja a hello virtuális gépek számát a készlet vagy hello maxTasksPerNode tulajdonság, ha a hello feladatok Várjon, amíg a csomópont szeretné elérhetővé tenni. Ennek vezénylését az Azure Batch automatikusan elvégzi.

hello portál rendelkezik részletes nézeteket hello feladatok és a feladatok állapota. Is hello a listában, és lekérése funkciók hello Azure csomópont SDK-t. Részletek szerepelnek hello dokumentáció [hivatkozás](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Job.html).

## <a name="next-steps"></a>Következő lépések

- Felülvizsgálati hello [áttekintés az Azure Batch funkcióinak](batch-api-basics.md) cikk, amely ajánlott, ha még új toohello szolgáltatás.
- Lásd: hello [kötegelt Node.js hivatkozás](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/) tooexplore hello kötegelt API.

