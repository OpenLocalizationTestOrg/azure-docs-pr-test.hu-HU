---
title: "aaaScheduling és a Data Factory végrehajtási |} Microsoft Docs"
description: "Ismerje meg az Azure Data Factory alkalmazásmodell ütemezés és a végrehajtási aspektusait."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 088a83df-4d1b-4ac1-afb3-0787a9bd1ca5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 6114dd4896f5537c789c3b632fb90e501b694285
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="data-factory-scheduling-and-execution"></a><span data-ttu-id="e7d2f-103">Data Factory ütemezés és a végrehajtás</span><span class="sxs-lookup"><span data-stu-id="e7d2f-103">Data Factory scheduling and execution</span></span>
<span data-ttu-id="e7d2f-104">Ez a cikk ismerteti a hello ütemezés és a végrehajtási aspektusainak hello Azure Data Factory alkalmazásmodellt.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-104">This article explains hello scheduling and execution aspects of hello Azure Data Factory application model.</span></span> <span data-ttu-id="e7d2f-105">Ez a cikk feltételezi, hogy tudomásul veszi a Data Factory alkalmazás modell fogalmakat, beleértve a tevékenység, a folyamatok, a társított szolgáltatások és a adatkészletek alapjait.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-105">This article assumes that you understand basics of Data Factory application model concepts, including activity, pipelines, linked services, and datasets.</span></span> <span data-ttu-id="e7d2f-106">Azure Data Factory alapvető fogalmait tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="e7d2f-106">For basic concepts of Azure Data Factory, see hello following articles:</span></span>

* [<span data-ttu-id="e7d2f-107">Bevezetés tooData gyári</span><span class="sxs-lookup"><span data-stu-id="e7d2f-107">Introduction tooData Factory</span></span>](data-factory-introduction.md)
* [<span data-ttu-id="e7d2f-108">Folyamatok</span><span class="sxs-lookup"><span data-stu-id="e7d2f-108">Pipelines</span></span>](data-factory-create-pipelines.md)
* [<span data-ttu-id="e7d2f-109">Adatkészletek</span><span class="sxs-lookup"><span data-stu-id="e7d2f-109">Datasets</span></span>](data-factory-create-datasets.md) 

## <a name="start-and-end-times-of-pipeline"></a><span data-ttu-id="e7d2f-110">Feldolgozási folyamat kezdő és befejező időpontja</span><span class="sxs-lookup"><span data-stu-id="e7d2f-110">Start and end times of pipeline</span></span>
<span data-ttu-id="e7d2f-111">Egy folyamat aktív csak között a **start** idő és **end** idő.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-111">A pipeline is active only between its **start** time and **end** time.</span></span> <span data-ttu-id="e7d2f-112">Nem hajtotta végre, hello kezdete előtt vagy után hello befejezési időpontja.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-112">It is not executed before hello start time or after hello end time.</span></span> <span data-ttu-id="e7d2f-113">Hello csővezeték fel van függesztve, ha nem végre függetlenül a kezdő és záró idő.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-113">If hello pipeline is paused, it is not executed irrespective of its start and end time.</span></span> <span data-ttu-id="e7d2f-114">Az a folyamat toorun azt kell nem lehet szüneteltetni.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-114">For a pipeline toorun, it should not be paused.</span></span> <span data-ttu-id="e7d2f-115">Ezek a beállítások (kezdő, célból szünetel) található hello csővezeték definíciója:</span><span class="sxs-lookup"><span data-stu-id="e7d2f-115">You find these settings (start, end, paused) in hello pipeline definition:</span></span> 

```json
"start": "2017-04-01T08:00:00Z",
"end": "2017-04-01T11:00:00Z"
"isPaused": false
```

<span data-ttu-id="e7d2f-116">További információ: ezek a Tulajdonságok [hozzon létre adatcsatornák](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-116">For more information these properties, see [create pipelines](data-factory-create-pipelines.md) article.</span></span> 


## <a name="specify-schedule-for-an-activity"></a><span data-ttu-id="e7d2f-117">Egy tevékenység ütemezése</span><span class="sxs-lookup"><span data-stu-id="e7d2f-117">Specify schedule for an activity</span></span>
<span data-ttu-id="e7d2f-118">Hello folyamat, amely végrehajtja a rendszer nincs.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-118">It is not hello pipeline that is executed.</span></span> <span data-ttu-id="e7d2f-119">Hello hello adatcsatorna tevékenységeinek, amelyek hello hello folyamat teljes környezetében.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-119">It is hello activities in hello pipeline that are executed in hello overall context of hello pipeline.</span></span> <span data-ttu-id="e7d2f-120">Hello segítségével is megadhat egy ismétlődő ütemezés megadása egy tevékenység **Feladatütemező** tevékenység JSON szakasza.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-120">You can specify a recurring schedule for an activity by using hello **scheduler** section of activity JSON.</span></span> <span data-ttu-id="e7d2f-121">Például ütemezheti egy tevékenység toorun óránkénti az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="e7d2f-121">For example, you can schedule an activity toorun hourly as follows:</span></span>  

```json
"scheduler": {
    "frequency": "Hour",
    "interval": 1
},
```

<span data-ttu-id="e7d2f-122">Hello a következő ábrán az látható, annak ütemezését, hogy egy tevékenység hoz létre egy sorozatát átfedésmentes windows hello-feldolgozási folyamat kezdési és befejezési időpontja.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-122">As shown in hello following diagram, specifying a schedule for an activity creates a series of tumbling windows with in hello pipeline start and end times.</span></span> <span data-ttu-id="e7d2f-123">Átfedésmentes windows rendszer rögzített méretű mozaikként, átfedés nélkül, összefüggő időközök sorozata.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-123">Tumbling windows are a series of fixed-size non-overlapping, contiguous time intervals.</span></span> <span data-ttu-id="e7d2f-124">A logikai átfedésmentes egy tevékenység hívjuk **tevékenység windows**.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-124">These logical tumbling windows for an activity are called **activity windows**.</span></span>

![Tevékenység Feladatütemező – példa](media/data-factory-scheduling-and-execution/scheduler-example.png)

<span data-ttu-id="e7d2f-126">Hello **Feladatütemező** tevékenység tulajdonsága nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-126">hello **scheduler** property for an activity is optional.</span></span> <span data-ttu-id="e7d2f-127">Ha megadja ezt a tulajdonságot, akkor meg kell egyeznie a hello tevékenység kimeneti adatkészlet hello definíciójában megadott hello ütemben történik.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-127">If you do specify this property, it must match hello cadence you specify in hello definition of output dataset for hello activity.</span></span> <span data-ttu-id="e7d2f-128">Kimeneti adatkészlet jelenleg milyen meghajtók hello ütemezést.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-128">Currently, output dataset is what drives hello schedule.</span></span> <span data-ttu-id="e7d2f-129">Ezért egy kimeneti adatkészlet kell létrehoznia, még akkor is, ha hello tevékenység nem ad kimenetet.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-129">Therefore, you must create an output dataset even if hello activity does not produce any output.</span></span> 

## <a name="specify-schedule-for-a-dataset"></a><span data-ttu-id="e7d2f-130">Adja meg a DataSet adatkészlet ütemezését</span><span class="sxs-lookup"><span data-stu-id="e7d2f-130">Specify schedule for a dataset</span></span>
<span data-ttu-id="e7d2f-131">A Data Factory-folyamat egy tevékenységének is igénybe vehet a nulla vagy több bemeneti **adatkészletek** , majd előállítanak egy vagy több kimeneti adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-131">An activity in a Data Factory pipeline can take zero or more input **datasets** and produce one or more output datasets.</span></span> <span data-ttu-id="e7d2f-132">Egy tevékenység megadhat hello ütemben történik, mely hello érhetők el a bemeneti adatok vagy hello kimeneti adatok hozzák használó hello **rendelkezésre állási** hello dataset definíciók című szakasza.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-132">For an activity, you can specify hello cadence at which hello input data is available or hello output data is produced by using hello **availability** section in hello dataset definitions.</span></span> 

<span data-ttu-id="e7d2f-133">**Gyakoriság** a hello **rendelkezésre állási** szakasz hello időegység határozza meg.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-133">**Frequency** in hello **availability** section specifies hello time unit.</span></span> <span data-ttu-id="e7d2f-134">hello két érték engedélyezett: a gyakoriság: perc, óra, nap, hét és hónap.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-134">hello allowed values for frequency are: Minute, Hour, Day, Week, and Month.</span></span> <span data-ttu-id="e7d2f-135">Hello **időköz** hello rendelkezésre állással kapcsolatos szakaszának a tulajdonság határozza meg a gyakoriság egy szorzóval.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-135">hello **interval** property in hello availability section specifies a multiplier for frequency.</span></span> <span data-ttu-id="e7d2f-136">Példa: Ha hello gyakoriságának beállítása tooDay, és egy kimeneti adatkészlet too1 időköz, hello kimeneti adatok naponta jön létre.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-136">For example: if hello frequency is set tooDay and interval is set too1 for an output dataset, hello output data is produced daily.</span></span> <span data-ttu-id="e7d2f-137">Perc gyakoriság hello ad meg, azt javasoljuk, hogy állítsa a hello időköz toono kisebb, mint 15.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-137">If you specify hello frequency as minute, we recommend that you set hello interval toono less than 15.</span></span> 

<span data-ttu-id="e7d2f-138">A következő példa hello, hello bemeneti adatok elérhető óránkénti és hello kimeneti adatok rendszer óránként készít adatszeletet (`"frequency": "Hour", "interval": 1`).</span><span class="sxs-lookup"><span data-stu-id="e7d2f-138">In hello following example, hello input data is available hourly and hello output data is produced hourly (`"frequency": "Hour", "interval": 1`).</span></span> 

<span data-ttu-id="e7d2f-139">**Bemeneti adatkészletet:**</span><span class="sxs-lookup"><span data-stu-id="e7d2f-139">**Input dataset:**</span></span> 

```json
{
    "name": "AzureSqlInput",
    "properties": {
        "published": false,
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
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


<span data-ttu-id="e7d2f-140">**Kimeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="e7d2f-140">**Output dataset**</span></span>

```json
{
    "name": "AzureBlobOutput",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mypath/{Year}/{Month}/{Day}/{Hour}",
            "format": {
                "type": "TextFormat"
            },
            "partitionedBy": [
                { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
                { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
                { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "HH" }}
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="e7d2f-141">Jelenleg **kimeneti adatkészlet meghajtók hello ütemezés**.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-141">Currently, **output dataset drives hello schedule**.</span></span> <span data-ttu-id="e7d2f-142">Más szóval hello kimeneti adatkészlet megadott hello ütemezése használt toorun futásidőben tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-142">In other words, hello schedule specified for hello output dataset is used toorun an activity at runtime.</span></span> <span data-ttu-id="e7d2f-143">Ezért egy kimeneti adatkészlet kell létrehoznia, még akkor is, ha hello tevékenység nem ad kimenetet.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-143">Therefore, you must create an output dataset even if hello activity does not produce any output.</span></span> <span data-ttu-id="e7d2f-144">Ha hello tevékenység egyetlen bemeneti nem veszi, kihagyhatja létrehozása hello bemeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-144">If hello activity doesn't take any input, you can skip creating hello input dataset.</span></span> 

<span data-ttu-id="e7d2f-145">Hello alábbi csővezeték-definíció, hello **Feladatütemező** tulajdonsága hello tevékenység használt toospecify ütemezését.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-145">In hello following pipeline definition, hello **scheduler** property is used toospecify schedule for hello activity.</span></span> <span data-ttu-id="e7d2f-146">Ez a tulajdonság nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-146">This property is optional.</span></span> <span data-ttu-id="e7d2f-147">Hello ütemezés hello tevékenység jelenleg hello kimeneti adatkészlet megadott hello ütemezése egyeznie kell.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-147">Currently, hello schedule for hello activity must match hello schedule specified for hello output dataset.</span></span>
 
```json
{
    "name": "SamplePipeline",
    "properties": {
        "description": "copy activity",
        "activities": [
            {
                "type": "Copy",
                "name": "AzureSQLtoBlob",
                "description": "copy activity",
                "typeProperties": {
                    "source": {
                        "type": "SqlSource",
                        "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 100000,
                        "writeBatchTimeout": "00:05:00"
                    }
                },
                "inputs": [
                    {
                        "name": "AzureSQLInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        ],
        "start": "2017-04-01T08:00:00Z",
        "end": "2017-04-01T11:00:00Z"
    }
}
```

<span data-ttu-id="e7d2f-148">Ebben a példában a hello tevékenység fut óránkénti hello közötti kezdési, és a befejezési időpontja hello folyamatának.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-148">In this example, hello activity runs hourly between hello start and end times of hello pipeline.</span></span> <span data-ttu-id="e7d2f-149">hello kimeneti adatok óránkénti állítanak elő háromórás windows (8 de - de, Reggel 9-10 Reggel 9, és 10 de - de).</span><span class="sxs-lookup"><span data-stu-id="e7d2f-149">hello output data is produced hourly for three-hour windows (8 AM - 9 AM, 9 AM - 10 AM, and 10 AM - 11 AM).</span></span> 

<span data-ttu-id="e7d2f-150">Felhasznált vagy egy futtatása tevékenység által létrehozott adatok tárolóegységekhez nevezik egy **adatszelet**.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-150">Each unit of data consumed or produced by an activity run is called a **data slice**.</span></span> <span data-ttu-id="e7d2f-151">a következő diagram hello egy bemeneti adatkészlet és egy kimeneti adatkészlet a tevékenység példáját mutatja be:</span><span class="sxs-lookup"><span data-stu-id="e7d2f-151">hello following diagram shows an example of an activity with one input dataset and one output dataset:</span></span> 

![Rendelkezésre állási Feladatütemező](./media/data-factory-scheduling-and-execution/availability-scheduler.png)

<span data-ttu-id="e7d2f-153">hello ábrán az látható hello óránkénti adatszeletek hello a bemeneti és kimeneti adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-153">hello diagram shows hello hourly data slices for hello input and output dataset.</span></span> <span data-ttu-id="e7d2f-154">hello ábrán látható, amely készen áll a feldolgozás három bemeneti szeletek.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-154">hello diagram shows three input slices that are ready for processing.</span></span> <span data-ttu-id="e7d2f-155">hello 10-11 AM tevékenység van folyamatban, hello 10-11 AM kimeneti szeletet előállító.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-155">hello 10-11 AM activity is in progress, producing hello 10-11 AM output slice.</span></span> 

<span data-ttu-id="e7d2f-156">Hello alatt az időtartam alatt hello aktuális szelet hello adatkészlet JSON társított változók használatával végezheti el: [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) és [SliceEnd](data-factory-functions-variables.md#data-factory-system-variables).</span><span class="sxs-lookup"><span data-stu-id="e7d2f-156">You can access hello time interval associated with hello current slice in hello dataset JSON by using variables: [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) and [SliceEnd](data-factory-functions-variables.md#data-factory-system-variables).</span></span> <span data-ttu-id="e7d2f-157">Hasonló módon érheti el egy tevékenység ablakban társított hello WindowStart és WindowEnd hello időintervallumban.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-157">Similarly, you can access hello time interval associated with an activity window by using hello WindowStart and WindowEnd.</span></span> <span data-ttu-id="e7d2f-158">hello ütemezés tevékenység hello ütemezés hello kimeneti adatkészlet hello tevékenységhez meg kell egyeznie.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-158">hello schedule of an activity must match hello schedule of hello output dataset for hello activity.</span></span> <span data-ttu-id="e7d2f-159">Ezért hello SliceStart és a SliceEnd értékek vannak hello azonos WindowStart és WindowEnd értékként kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-159">Therefore, hello SliceStart and SliceEnd values are hello same as WindowStart and WindowEnd values respectively.</span></span> <span data-ttu-id="e7d2f-160">Ezek a változók további információkért lásd: [adat-előállító funkciók és rendszerváltozók](data-factory-functions-variables.md#data-factory-system-variables) cikkeket.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-160">For more information on these variables, see [Data Factory functions and system variables](data-factory-functions-variables.md#data-factory-system-variables) articles.</span></span>  

<span data-ttu-id="e7d2f-161">Ezek a változók többféle célra használhatja a a tevékenység JSON-NÁ.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-161">You can use these variables for different purposes in your activity JSON.</span></span> <span data-ttu-id="e7d2f-162">Például használhatja őket adatsorozat időadatok képviselő bemeneti és kimeneti adatkészletek tooselect adatait (például: 8 óra too9 VAGYOK).</span><span class="sxs-lookup"><span data-stu-id="e7d2f-162">For example, you can use them tooselect data from input and output datasets representing time series data (for example: 8 AM too9 AM).</span></span> <span data-ttu-id="e7d2f-163">Ez a példa **WindowStart** és **WindowEnd** tooselect vonatkozó adatokat egy tevékenységhez futtatni, és másolja a megfelelő hello tooa blob **folderPath**.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-163">This example also uses **WindowStart** and **WindowEnd** tooselect relevant data for an activity run and copy it tooa blob with hello appropriate **folderPath**.</span></span> <span data-ttu-id="e7d2f-164">Hello **folderPath** óránként paraméteres toohave külön mappába van.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-164">hello **folderPath** is parameterized toohave a separate folder for every hour.</span></span>  

<span data-ttu-id="e7d2f-165">Példa megelőző hello, a megadott bemeneti és kimeneti adatkészletek hello ütemezése van hello azonos (óránként).</span><span class="sxs-lookup"><span data-stu-id="e7d2f-165">In hello preceding example, hello schedule specified for input and output datasets is hello same (hourly).</span></span> <span data-ttu-id="e7d2f-166">Ha hello bemeneti adatkészlet hello tevékenység elérhető különböző gyakorisággal mondja ki 15 percenként, a hello tevékenység, amely létrehozza a kimeneti adatkészletet továbbra is fut óránként, mert hello kimeneti adatkészlet milyen meghajtók hello tevékenységütemezést.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-166">If hello input dataset for hello activity is available at a different frequency, say every 15 minutes, hello activity that produces this output dataset still runs once an hour as hello output dataset is what drives hello activity schedule.</span></span> <span data-ttu-id="e7d2f-167">További információkért lásd: [eltérő gyakorisággal adatkészletek modell](#model-datasets-with-different-frequencies).</span><span class="sxs-lookup"><span data-stu-id="e7d2f-167">For more information, see [Model datasets with different frequencies](#model-datasets-with-different-frequencies).</span></span>

## <a name="dataset-availability-and-policies"></a><span data-ttu-id="e7d2f-168">Adatkészlet rendelkezésre állását és házirendek</span><span class="sxs-lookup"><span data-stu-id="e7d2f-168">Dataset availability and policies</span></span>
<span data-ttu-id="e7d2f-169">Amint láthatta hello használat gyakorisága és időköz tulajdonságokat hello rendelkezésre állással kapcsolatos szakaszának adatkészlet-definícióban.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-169">You have seen hello usage of frequency and interval properties in hello availability section of dataset definition.</span></span> <span data-ttu-id="e7d2f-170">Nincsenek néhány más tulajdonságok, amelyek hello ütemezés és a tevékenység végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-170">There are a few other properties that affect hello scheduling and execution of an activity.</span></span> 

### <a name="dataset-availability"></a><span data-ttu-id="e7d2f-171">Adatkészlet rendelkezésre állása</span><span class="sxs-lookup"><span data-stu-id="e7d2f-171">Dataset availability</span></span> 
<span data-ttu-id="e7d2f-172">hello következő táblázat ismerteti a hello használható tulajdonságok **rendelkezésre állási** szakasz:</span><span class="sxs-lookup"><span data-stu-id="e7d2f-172">hello following table describes properties you can use in hello **availability** section:</span></span>

| <span data-ttu-id="e7d2f-173">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="e7d2f-173">Property</span></span> | <span data-ttu-id="e7d2f-174">Leírás</span><span class="sxs-lookup"><span data-stu-id="e7d2f-174">Description</span></span> | <span data-ttu-id="e7d2f-175">Szükséges</span><span class="sxs-lookup"><span data-stu-id="e7d2f-175">Required</span></span> | <span data-ttu-id="e7d2f-176">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="e7d2f-176">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="e7d2f-177">frequency</span><span class="sxs-lookup"><span data-stu-id="e7d2f-177">frequency</span></span> |<span data-ttu-id="e7d2f-178">Megadja a dataset szelet üzemi hello időegységét.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-178">Specifies hello time unit for dataset slice production.</span></span><br/><br/><span data-ttu-id="e7d2f-179"><b>Támogatott gyakoriság</b>: perc, óra, nap, hét, hónap</span><span class="sxs-lookup"><span data-stu-id="e7d2f-179"><b>Supported frequency</b>: Minute, Hour, Day, Week, Month</span></span> |<span data-ttu-id="e7d2f-180">Igen</span><span class="sxs-lookup"><span data-stu-id="e7d2f-180">Yes</span></span> |<span data-ttu-id="e7d2f-181">NA</span><span class="sxs-lookup"><span data-stu-id="e7d2f-181">NA</span></span> |
| <span data-ttu-id="e7d2f-182">interval</span><span class="sxs-lookup"><span data-stu-id="e7d2f-182">interval</span></span> |<span data-ttu-id="e7d2f-183">Megadja egy szorzóval gyakoriság</span><span class="sxs-lookup"><span data-stu-id="e7d2f-183">Specifies a multiplier for frequency</span></span><br/><br/><span data-ttu-id="e7d2f-184">"X időköz" határozza meg, milyen gyakran hello szelet jön létre.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-184">”Frequency x interval” determines how often hello slice is produced.</span></span><br/><br/><span data-ttu-id="e7d2f-185">Ha dataset toobe szeletelhetők óránként kell hello, akkor be <b>gyakoriság</b> túl<b>óra</b>, és <b>időköz</b> túl<b>1</b>.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-185">If you need hello dataset toobe sliced on an hourly basis, you set <b>Frequency</b> too<b>Hour</b>, and <b>interval</b> too<b>1</b>.</span></span><br/><br/><span data-ttu-id="e7d2f-186"><b>Megjegyzés:</b>: perces gyakoriságot ad meg, ha azt javasoljuk, hogy állítsa a 15-nál kisebb hello időköz toono</span><span class="sxs-lookup"><span data-stu-id="e7d2f-186"><b>Note</b>: If you specify Frequency as Minute, we recommend that you set hello interval toono less than 15</span></span> |<span data-ttu-id="e7d2f-187">Igen</span><span class="sxs-lookup"><span data-stu-id="e7d2f-187">Yes</span></span> |<span data-ttu-id="e7d2f-188">NA</span><span class="sxs-lookup"><span data-stu-id="e7d2f-188">NA</span></span> |
| <span data-ttu-id="e7d2f-189">stílus</span><span class="sxs-lookup"><span data-stu-id="e7d2f-189">style</span></span> |<span data-ttu-id="e7d2f-190">Meghatározza, hogy kell-e hello szelet előállított hello kezdő/záró hello időköz.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-190">Specifies whether hello slice should be produced at hello start/end of hello interval.</span></span><ul><li><span data-ttu-id="e7d2f-191">StartOfInterval</span><span class="sxs-lookup"><span data-stu-id="e7d2f-191">StartOfInterval</span></span></li><li><span data-ttu-id="e7d2f-192">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="e7d2f-192">EndOfInterval</span></span></li></ul><br/><br/><span data-ttu-id="e7d2f-193">Ha gyakoriságának beállítása tooMonth és stílus tooEndOfInterval van beállítva, hello szelet a hónap utolsó napján hello elő.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-193">If Frequency is set tooMonth and style is set tooEndOfInterval, hello slice is produced on hello last day of month.</span></span> <span data-ttu-id="e7d2f-194">Ha hello stílus be van állítva tooStartOfInterval, hello szelet elő hello a hónap első napján.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-194">If hello style is set tooStartOfInterval, hello slice is produced on hello first day of month.</span></span><br/><br/><span data-ttu-id="e7d2f-195">Ha gyakoriságának beállítása tooDay és stílus tooEndOfInterval van beállítva, hello szelet elő az elmúlt egy órában hello nap hello.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-195">If Frequency is set tooDay and style is set tooEndOfInterval, hello slice is produced in hello last hour of hello day.</span></span><br/><br/><span data-ttu-id="e7d2f-196">Ha gyakoriság tooHour és stílus tooEndOfInterval van beállítva, hello szelet hello végének hello keletkezik.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-196">If Frequency is set tooHour and style is set tooEndOfInterval, hello slice is produced at hello end of hello hour.</span></span> <span data-ttu-id="e7d2f-197">Például a szelet du. 1 – 2 óra időszakban, a hello szelet hozzák 2 du..</span><span class="sxs-lookup"><span data-stu-id="e7d2f-197">For example, for a slice for 1 PM – 2 PM period, hello slice is produced at 2 PM.</span></span> |<span data-ttu-id="e7d2f-198">Nem</span><span class="sxs-lookup"><span data-stu-id="e7d2f-198">No</span></span> |<span data-ttu-id="e7d2f-199">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="e7d2f-199">EndOfInterval</span></span> |
| <span data-ttu-id="e7d2f-200">anchorDateTime</span><span class="sxs-lookup"><span data-stu-id="e7d2f-200">anchorDateTime</span></span> |<span data-ttu-id="e7d2f-201">Hello abszolút pozíciója a ennyi másodpercig használta a Feladatütemező toocompute dataset szelet határok meghatározása.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-201">Defines hello absolute position in time used by scheduler toocompute dataset slice boundaries.</span></span> <br/><br/><span data-ttu-id="e7d2f-202"><b>Megjegyzés:</b>: Ha hello AnchorDateTime részekből dátum, amelyek részletesebben, mint a hello gyakorisága, akkor hello részletesebb részek figyelmen kívül lesznek hagyva.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-202"><b>Note</b>: If hello AnchorDateTime has date parts that are more granular than hello frequency then hello more granular parts are ignored.</span></span> <br/><br/><span data-ttu-id="e7d2f-203">Például, ha hello <b>időköz</b> van <b>óránkénti</b> (gyakoriság: óra és időköz: 1) és hello <b>AnchorDateTime</b> tartalmaz <b>percet és másodpercet</b>, majd hello <b>percet és másodpercet</b> hello AnchorDateTime részei a rendszer figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-203">For example, if hello <b>interval</b> is <b>hourly</b> (frequency: hour and interval: 1) and hello <b>AnchorDateTime</b> contains <b>minutes and seconds</b>, then hello <b>minutes and seconds</b> parts of hello AnchorDateTime are ignored.</span></span> |<span data-ttu-id="e7d2f-204">Nem</span><span class="sxs-lookup"><span data-stu-id="e7d2f-204">No</span></span> |<span data-ttu-id="e7d2f-205">01/01/0001</span><span class="sxs-lookup"><span data-stu-id="e7d2f-205">01/01/0001</span></span> |
| <span data-ttu-id="e7d2f-206">Az offset</span><span class="sxs-lookup"><span data-stu-id="e7d2f-206">offset</span></span> |<span data-ttu-id="e7d2f-207">TimeSpan érték, mely hello által kezdő és záró összes adatkészlet szeletek vette.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-207">Timespan by which hello start and end of all dataset slices are shifted.</span></span> <br/><br/><span data-ttu-id="e7d2f-208"><b>Megjegyzés:</b>: Ha anchorDateTime és az offset is meg van adva, hello eredménye kombinált hello shift.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-208"><b>Note</b>: If both anchorDateTime and offset are specified, hello result is hello combined shift.</span></span> |<span data-ttu-id="e7d2f-209">Nem</span><span class="sxs-lookup"><span data-stu-id="e7d2f-209">No</span></span> |<span data-ttu-id="e7d2f-210">NA</span><span class="sxs-lookup"><span data-stu-id="e7d2f-210">NA</span></span> |

### <a name="offset-example"></a><span data-ttu-id="e7d2f-211">az eltolási – példa</span><span class="sxs-lookup"><span data-stu-id="e7d2f-211">offset example</span></span>
<span data-ttu-id="e7d2f-212">Alapértelmezés szerint naponta (`"frequency": "Day", "interval": 1`) szeletek kezdjék 12 óra UTC idő szerint (éjfél).</span><span class="sxs-lookup"><span data-stu-id="e7d2f-212">By default, daily (`"frequency": "Day", "interval": 1`) slices start at 12 AM UTC time (midnight).</span></span> <span data-ttu-id="e7d2f-213">Ha ehelyett hello kezdési idő toobe reggel 6 óra UTC idő szerint, be eltolásnál, ahogy az alábbi részlet hello hello:</span><span class="sxs-lookup"><span data-stu-id="e7d2f-213">If you want hello start time toobe 6 AM UTC time instead, set hello offset as shown in hello following snippet:</span></span> 

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
### <a name="anchordatetime-example"></a><span data-ttu-id="e7d2f-214">anchorDateTime – példa</span><span class="sxs-lookup"><span data-stu-id="e7d2f-214">anchorDateTime example</span></span>
<span data-ttu-id="e7d2f-215">A következő példa hello hello dataset 23 óránként jön létre.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-215">In hello following example, hello dataset is produced once every 23 hours.</span></span> <span data-ttu-id="e7d2f-216">hello első szelet elindítja időpontban hello hello anchorDateTime, melynek értéke túl által megadott`2017-04-19T08:00:00` (UTC idő).</span><span class="sxs-lookup"><span data-stu-id="e7d2f-216">hello first slice starts at hello time specified by hello anchorDateTime, which is set too`2017-04-19T08:00:00` (UTC time).</span></span>

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 23,    
    "anchorDateTime":"2017-04-19T08:00:00"    
}
```

### <a name="offsetstyle-example"></a><span data-ttu-id="e7d2f-217">Az offset/stílus – példa</span><span class="sxs-lookup"><span data-stu-id="e7d2f-217">offset/style Example</span></span>
<span data-ttu-id="e7d2f-218">hello következő dataset havi adatkészlet és a 3. minden hónap 8:00 órakor hozzák (`3.08:00:00`):</span><span class="sxs-lookup"><span data-stu-id="e7d2f-218">hello following dataset is a monthly dataset and is produced on 3rd of every month at 8:00 AM (`3.08:00:00`):</span></span>

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "offset": "3.08:00:00", 
    "style": "StartOfInterval"
}
```

### <a name="dataset-policy"></a><span data-ttu-id="e7d2f-219">A DataSet házirend</span><span class="sxs-lookup"><span data-stu-id="e7d2f-219">Dataset policy</span></span>
<span data-ttu-id="e7d2f-220">Egy adatkészlet tartozhat egy meghatározott érvényesség-ellenőrzési házirend, amely meghatározza, hogyan olyan szelet végrehajtással által létrehozott hello adatok is ellenőrizni kell, mielőtt azt készen áll a felhasználásra.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-220">A dataset can have a validation policy defined that specifies how hello data generated by a slice execution can be validated before it is ready for consumption.</span></span> <span data-ttu-id="e7d2f-221">Ilyen esetekben hello szelet végrehajtás befejezését követően hello kimeneti szelet állapota túl**Várakozás** rendelkező, a részállapot **érvényesítési**.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-221">In such cases, after hello slice has finished execution, hello output slice status is changed too**Waiting** with a substatus of **Validation**.</span></span> <span data-ttu-id="e7d2f-222">Hello szeletek ellenőrzését követően hello szelet állapota túl**készen**.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-222">After hello slices are validated, hello slice status changes too**Ready**.</span></span> <span data-ttu-id="e7d2f-223">Ha egy adatszelet készült, de nem felelt meg a hello érvényesítési, alsóbb rétegbeli szeletek a szelet függő tevékenység fut nincsenek feldolgozva.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-223">If a data slice has been produced but did not pass hello validation, activity runs for downstream slices that depend on this slice are not processed.</span></span> <span data-ttu-id="e7d2f-224">[Megfigyelés és kezelés folyamatok](data-factory-monitor-manage-pipelines.md) magában foglalja az adat-előállítóban adatszeletek különböző állapotok hello.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-224">[Monitor and manage pipelines](data-factory-monitor-manage-pipelines.md) covers hello various states of data slices in Data Factory.</span></span>

<span data-ttu-id="e7d2f-225">Hello **házirend** hello feltételek rész az adatkészlet-definícióban vagy hello feltétellel, hogy a dataset szeletek hello teljesítenie kell.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-225">hello **policy** section in dataset definition defines hello criteria or hello condition that hello dataset slices must fulfill.</span></span> <span data-ttu-id="e7d2f-226">hello következő táblázat ismerteti a hello használható tulajdonságok **házirend** szakasz:</span><span class="sxs-lookup"><span data-stu-id="e7d2f-226">hello following table describes properties you can use in hello **policy** section:</span></span>

| <span data-ttu-id="e7d2f-227">Házirend neve</span><span class="sxs-lookup"><span data-stu-id="e7d2f-227">Policy Name</span></span> | <span data-ttu-id="e7d2f-228">Leírás</span><span class="sxs-lookup"><span data-stu-id="e7d2f-228">Description</span></span> | <span data-ttu-id="e7d2f-229">Alkalmazott túl</span><span class="sxs-lookup"><span data-stu-id="e7d2f-229">Applied too</span></span>| <span data-ttu-id="e7d2f-230">Szükséges</span><span class="sxs-lookup"><span data-stu-id="e7d2f-230">Required</span></span> | <span data-ttu-id="e7d2f-231">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="e7d2f-231">Default</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="e7d2f-232">minimumSizeMB</span><span class="sxs-lookup"><span data-stu-id="e7d2f-232">minimumSizeMB</span></span> | <span data-ttu-id="e7d2f-233">Ellenőrzi, hogy hello adatokat egy **Azure blob** megfelel-e hello (megabájtban) a minimális méret követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-233">Validates that hello data in an **Azure blob** meets hello minimum size requirements (in megabytes).</span></span> |<span data-ttu-id="e7d2f-234">Azure-blob</span><span class="sxs-lookup"><span data-stu-id="e7d2f-234">Azure Blob</span></span> |<span data-ttu-id="e7d2f-235">Nem</span><span class="sxs-lookup"><span data-stu-id="e7d2f-235">No</span></span> |<span data-ttu-id="e7d2f-236">NA</span><span class="sxs-lookup"><span data-stu-id="e7d2f-236">NA</span></span> |
| <span data-ttu-id="e7d2f-237">minimumRows</span><span class="sxs-lookup"><span data-stu-id="e7d2f-237">minimumRows</span></span> | <span data-ttu-id="e7d2f-238">Ellenőrzi, hogy hello adatokat egy **Azure SQL adatbázis** vagy egy **Azure-tábla** hello a sorok legkisebb számát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-238">Validates that hello data in an **Azure SQL database** or an **Azure table** contains hello minimum number of rows.</span></span> |<ul><li><span data-ttu-id="e7d2f-239">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="e7d2f-239">Azure SQL Database</span></span></li><li><span data-ttu-id="e7d2f-240">Azure-tábla</span><span class="sxs-lookup"><span data-stu-id="e7d2f-240">Azure Table</span></span></li></ul> |<span data-ttu-id="e7d2f-241">Nem</span><span class="sxs-lookup"><span data-stu-id="e7d2f-241">No</span></span> |<span data-ttu-id="e7d2f-242">NA</span><span class="sxs-lookup"><span data-stu-id="e7d2f-242">NA</span></span> |

#### <a name="examples"></a><span data-ttu-id="e7d2f-243">Példák</span><span class="sxs-lookup"><span data-stu-id="e7d2f-243">Examples</span></span>
<span data-ttu-id="e7d2f-244">**minimumSizeMB:**</span><span class="sxs-lookup"><span data-stu-id="e7d2f-244">**minimumSizeMB:**</span></span>

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

<span data-ttu-id="e7d2f-245">**minimumRows**</span><span class="sxs-lookup"><span data-stu-id="e7d2f-245">**minimumRows**</span></span>

```json
"policy":
{
    "validation":
    {
        "minimumRows": 100
    }
}
```

<span data-ttu-id="e7d2f-246">Ezek a tulajdonságok és példák kapcsolatos további információkért lásd: [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-246">For more information about these properties and examples, see [Create datasets](data-factory-create-datasets.md) article.</span></span> 

## <a name="activity-policies"></a><span data-ttu-id="e7d2f-247">A tevékenység-szabályzatok</span><span class="sxs-lookup"><span data-stu-id="e7d2f-247">Activity policies</span></span>
<span data-ttu-id="e7d2f-248">A házirendek milyen hatással hello futtatási viselkedés tevékenység, kifejezetten egy tábla hello szelet feldolgozásakor.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-248">Policies affect hello run-time behavior of an activity, specifically when hello slice of a table is processed.</span></span> <span data-ttu-id="e7d2f-249">a következő táblázat hello hello részletes adatokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-249">hello following table provides hello details.</span></span>

| <span data-ttu-id="e7d2f-250">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="e7d2f-250">Property</span></span> | <span data-ttu-id="e7d2f-251">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="e7d2f-251">Permitted values</span></span> | <span data-ttu-id="e7d2f-252">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="e7d2f-252">Default Value</span></span> | <span data-ttu-id="e7d2f-253">Leírás</span><span class="sxs-lookup"><span data-stu-id="e7d2f-253">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="e7d2f-254">Egyidejűségi</span><span class="sxs-lookup"><span data-stu-id="e7d2f-254">concurrency</span></span> |<span data-ttu-id="e7d2f-255">Egész szám</span><span class="sxs-lookup"><span data-stu-id="e7d2f-255">Integer</span></span> <br/><br/><span data-ttu-id="e7d2f-256">A maximális érték: 10</span><span class="sxs-lookup"><span data-stu-id="e7d2f-256">Max value: 10</span></span> |<span data-ttu-id="e7d2f-257">1</span><span class="sxs-lookup"><span data-stu-id="e7d2f-257">1</span></span> |<span data-ttu-id="e7d2f-258">Egyidejű végrehajtások hello tevékenység száma.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-258">Number of concurrent executions of hello activity.</span></span><br/><br/><span data-ttu-id="e7d2f-259">Meghatározza, hogy hello száma párhuzamos tevékenység végrehajtások, amely akkor fordulhat elő, a másik szeletek.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-259">It determines hello number of parallel activity executions that can happen on different slices.</span></span> <span data-ttu-id="e7d2f-260">Például ha egy tevékenység toogo keresztül elérhető adatokat, nagyobb feldolgozási értéke számos felgyorsítja a hello adatok feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-260">For example, if an activity needs toogo through a large set of available data, having a larger concurrency value speeds up hello data processing.</span></span> |
| <span data-ttu-id="e7d2f-261">executionPriorityOrder</span><span class="sxs-lookup"><span data-stu-id="e7d2f-261">executionPriorityOrder</span></span> |<span data-ttu-id="e7d2f-262">NewestFirst</span><span class="sxs-lookup"><span data-stu-id="e7d2f-262">NewestFirst</span></span><br/><br/><span data-ttu-id="e7d2f-263">OldestFirst</span><span class="sxs-lookup"><span data-stu-id="e7d2f-263">OldestFirst</span></span> |<span data-ttu-id="e7d2f-264">OldestFirst</span><span class="sxs-lookup"><span data-stu-id="e7d2f-264">OldestFirst</span></span> |<span data-ttu-id="e7d2f-265">Meghatározza, hogy hello sorrendje feldolgozott adatszeletek.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-265">Determines hello ordering of data slices that are being processed.</span></span><br/><br/><span data-ttu-id="e7d2f-266">Például ha 2 szeletek (du. 4: egy azonban és délután 5 óra egy másik tulajdonságnak), és mindkét végrehajtási függőben van.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-266">For example, if you have 2 slices (one happening at 4pm, and another one at 5pm), and both are pending execution.</span></span> <span data-ttu-id="e7d2f-267">Ha hello executionPriorityOrder toobe NewestFirst, hello szelet, délután 5 óra lesz elsőként feldolgozva.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-267">If you set hello executionPriorityOrder toobe NewestFirst, hello slice at 5 PM is processed first.</span></span> <span data-ttu-id="e7d2f-268">Hasonlóképpen ha hello executionPriorityORder toobe OldestFIrst, majd du. 4: hello szelet feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-268">Similarly if you set hello executionPriorityORder toobe OldestFIrst, then hello slice at 4 PM is processed.</span></span> |
| <span data-ttu-id="e7d2f-269">retry</span><span class="sxs-lookup"><span data-stu-id="e7d2f-269">retry</span></span> |<span data-ttu-id="e7d2f-270">Egész szám</span><span class="sxs-lookup"><span data-stu-id="e7d2f-270">Integer</span></span><br/><br/><span data-ttu-id="e7d2f-271">A maximális érték 10 is lehet.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-271">Max value can be 10</span></span> |<span data-ttu-id="e7d2f-272">0</span><span class="sxs-lookup"><span data-stu-id="e7d2f-272">0</span></span> |<span data-ttu-id="e7d2f-273">Hello adatfeldolgozási hello adatszelethez előtt újrapróbálkozások száma hiba van megjelölve.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-273">Number of retries before hello data processing for hello slice is marked as Failure.</span></span> <span data-ttu-id="e7d2f-274">Egy adatszelet tevékenység végrehajtása a rendszer ismét megkísérli megadott toohello mentése újrapróbálkozások száma.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-274">Activity execution for a data slice is retried up toohello specified retry count.</span></span> <span data-ttu-id="e7d2f-275">hello újrapróbálkozási minél hamarabb hello meghibásodás után történik.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-275">hello retry is done as soon as possible after hello failure.</span></span> |
| <span data-ttu-id="e7d2f-276">timeout</span><span class="sxs-lookup"><span data-stu-id="e7d2f-276">timeout</span></span> |<span data-ttu-id="e7d2f-277">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="e7d2f-277">TimeSpan</span></span> |<span data-ttu-id="e7d2f-278">00:00:00</span><span class="sxs-lookup"><span data-stu-id="e7d2f-278">00:00:00</span></span> |<span data-ttu-id="e7d2f-279">Hello tevékenység időkorlátját.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-279">Timeout for hello activity.</span></span> <span data-ttu-id="e7d2f-280">. Példa: 00:10:00 (azt jelenti, időtúllépés 10 perc)</span><span class="sxs-lookup"><span data-stu-id="e7d2f-280">Example: 00:10:00 (implies timeout 10 mins)</span></span><br/><br/><span data-ttu-id="e7d2f-281">Ha az érték nincs megadva vagy 0, hello időtúllépési érték végtelen.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-281">If a value is not specified or is 0, hello timeout is infinite.</span></span><br/><br/><span data-ttu-id="e7d2f-282">Szelet hello adatok feldolgozási ideje meghaladja a hello időtúllépési értéket, ha azt megszakadt, és hello rendszer próbál tooretry hello feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-282">If hello data processing time on a slice exceeds hello timeout value, it is canceled, and hello system attempts tooretry hello processing.</span></span> <span data-ttu-id="e7d2f-283">Az újrapróbálkozások számát hello hello újrapróbálkozási tulajdonság függ.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-283">hello number of retries depends on hello retry property.</span></span> <span data-ttu-id="e7d2f-284">Időtúllépés esetén hello beállítás tooTimedOut.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-284">When timeout occurs, hello status is set tooTimedOut.</span></span> |
| <span data-ttu-id="e7d2f-285">Késleltetés</span><span class="sxs-lookup"><span data-stu-id="e7d2f-285">delay</span></span> |<span data-ttu-id="e7d2f-286">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="e7d2f-286">TimeSpan</span></span> |<span data-ttu-id="e7d2f-287">00:00:00</span><span class="sxs-lookup"><span data-stu-id="e7d2f-287">00:00:00</span></span> |<span data-ttu-id="e7d2f-288">Adja meg a hello késleltetés hello szelet elindítja az adatok feldolgozása előtt.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-288">Specify hello delay before data processing of hello slice starts.</span></span><br/><br/><span data-ttu-id="e7d2f-289">egy adatszelet tevékenységet hello végrehajtása után hello késleltetés hello várt végrehajtási ideje elmúlt elindult.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-289">hello execution of activity for a data slice is started after hello Delay is past hello expected execution time.</span></span><br/><br/><span data-ttu-id="e7d2f-290">. Példa: 00:10:00 (magában foglalja a késleltetést a 10 perc)</span><span class="sxs-lookup"><span data-stu-id="e7d2f-290">Example: 00:10:00 (implies delay of 10 mins)</span></span> |
| <span data-ttu-id="e7d2f-291">hosszú újrapróbálkozás</span><span class="sxs-lookup"><span data-stu-id="e7d2f-291">longRetry</span></span> |<span data-ttu-id="e7d2f-292">Egész szám</span><span class="sxs-lookup"><span data-stu-id="e7d2f-292">Integer</span></span><br/><br/><span data-ttu-id="e7d2f-293">A maximális érték: 10</span><span class="sxs-lookup"><span data-stu-id="e7d2f-293">Max value: 10</span></span> |<span data-ttu-id="e7d2f-294">1</span><span class="sxs-lookup"><span data-stu-id="e7d2f-294">1</span></span> |<span data-ttu-id="e7d2f-295">hosszú újrapróbálkozások számát hello tett kísérletet, mielőtt hello szelet végrehajtása sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-295">hello number of long retry attempts before hello slice execution is failed.</span></span><br/><br/><span data-ttu-id="e7d2f-296">hosszú újrapróbálkozás kísérletek által longRetryInterval távolságban helyezkednek el.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-296">longRetry attempts are spaced by longRetryInterval.</span></span> <span data-ttu-id="e7d2f-297">Ha toospecify kell egy újrapróbálkozási kísérletek között eltelt idő, így hosszú újrapróbálkozás használja.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-297">So if you need toospecify a time between retry attempts, use longRetry.</span></span> <span data-ttu-id="e7d2f-298">Ha mind az újra gombra, és a hosszú újrapróbálkozás meg van adva, minden hosszú újrapróbálkozás kísérlet tartalmazza az ismételt kísérletek számát, és hello kísérletek maximális száma újrapróbálkozási * hosszú újrapróbálkozás.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-298">If both Retry and longRetry are specified, each longRetry attempt includes Retry attempts and hello max number of attempts is Retry * longRetry.</span></span><br/><br/><span data-ttu-id="e7d2f-299">Ha például tudunk hello hello tevékenység házirend beállításai a következő:</span><span class="sxs-lookup"><span data-stu-id="e7d2f-299">For example, if we have hello following settings in hello activity policy:</span></span><br/><span data-ttu-id="e7d2f-300">Próbálkozzon újra: 3</span><span class="sxs-lookup"><span data-stu-id="e7d2f-300">Retry: 3</span></span><br/><span data-ttu-id="e7d2f-301">hosszú újrapróbálkozás: 2. régiója</span><span class="sxs-lookup"><span data-stu-id="e7d2f-301">longRetry: 2</span></span><br/><span data-ttu-id="e7d2f-302">longRetryInterval: 01:00:00</span><span class="sxs-lookup"><span data-stu-id="e7d2f-302">longRetryInterval: 01:00:00</span></span><br/><br/><span data-ttu-id="e7d2f-303">Feltételezik, hogy csak egy szelet tooexecute (állapot vár) hello tevékenység végrehajtási minden olyan alkalommal sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-303">Assume there is only one slice tooexecute (status is Waiting) and hello activity execution fails every time.</span></span> <span data-ttu-id="e7d2f-304">Eredetileg nem lenne 3 egymást követő végrehajtási kísérletek.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-304">Initially there would be 3 consecutive execution attempts.</span></span> <span data-ttu-id="e7d2f-305">Minden kísérlet után hello szelet állapota lenne az újra gombra.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-305">After each attempt, hello slice status would be Retry.</span></span> <span data-ttu-id="e7d2f-306">Miután először 3 kísérletet: keresztül, hello szelet állapota hosszú újrapróbálkozás lesz.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-306">After first 3 attempts are over, hello slice status would be LongRetry.</span></span><br/><br/><span data-ttu-id="e7d2f-307">Egy óra (Ez azt jelenti, hogy longRetryInteval tartozó érték) nem lenne a 3 egymást követő végrehajtási kísérletek egy másik készlet.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-307">After an hour (that is, longRetryInteval’s value), there would be another set of 3 consecutive execution attempts.</span></span> <span data-ttu-id="e7d2f-308">Ezt követően hello szelet állapota akkor sikertelen, és nincs további újrapróbálkozások volna kísérli meg.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-308">After that, hello slice status would be Failed and no more retries would be attempted.</span></span> <span data-ttu-id="e7d2f-309">Ezért a teljes 6 történt kísérlet.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-309">Hence overall 6 attempts were made.</span></span><br/><br/><span data-ttu-id="e7d2f-310">Ha bármely végrehajtása sikeres, hello szelet állapota Kész és nincs további újrapróbálkozások próbált vannak.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-310">If any execution succeeds, hello slice status would be Ready and no more retries are attempted.</span></span><br/><br/><span data-ttu-id="e7d2f-311">hosszú újrapróbálkozás függő adatok nem determinisztikus időpontokban érkeznek vagy hello teljes környezete alapján akkor következik be, mely az adatfeldolgozás flaky használható.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-311">longRetry may be used in situations where dependent data arrives at non-deterministic times or hello overall environment is flaky under which data processing occurs.</span></span> <span data-ttu-id="e7d2f-312">Ezekben az esetekben nem újrapróbálkozások egymás után ez segíthet, és kimeneti így időt eredményez hello időköz után szükséges.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-312">In such cases, doing retries one after another may not help and doing so after an interval of time results in hello desired output.</span></span><br/><br/><span data-ttu-id="e7d2f-313">Járjon el a Word: nincs beállítva hosszú újrapróbálkozás vagy longRetryInterval magas értékeit.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-313">Word of caution: do not set high values for longRetry or longRetryInterval.</span></span> <span data-ttu-id="e7d2f-314">Általában a magasabb értékkel rendszeres problémákkal utalnak.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-314">Typically, higher values imply other systemic issues.</span></span> |
| <span data-ttu-id="e7d2f-315">longRetryInterval</span><span class="sxs-lookup"><span data-stu-id="e7d2f-315">longRetryInterval</span></span> |<span data-ttu-id="e7d2f-316">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="e7d2f-316">TimeSpan</span></span> |<span data-ttu-id="e7d2f-317">00:00:00</span><span class="sxs-lookup"><span data-stu-id="e7d2f-317">00:00:00</span></span> |<span data-ttu-id="e7d2f-318">hello kísérletek hosszú újrapróbálkozási kísérletek között eltelt idő</span><span class="sxs-lookup"><span data-stu-id="e7d2f-318">hello delay between long retry attempts</span></span> |

<span data-ttu-id="e7d2f-319">További információkért lásd: [folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-319">For more information, see [Pipelines](data-factory-create-pipelines.md) article.</span></span> 

## <a name="parallel-processing-of-data-slices"></a><span data-ttu-id="e7d2f-320">Párhuzamos feldolgozás adatszeletek</span><span class="sxs-lookup"><span data-stu-id="e7d2f-320">Parallel processing of data slices</span></span>
<span data-ttu-id="e7d2f-321">Hello adatcsatorna hello kezdő dátuma múltbeli hello állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-321">You can set hello start date for hello pipeline in hello past.</span></span> <span data-ttu-id="e7d2f-322">Ha így tesz, a Data Factory automatikusan kiszámítja (hátsó kitöltés) múltbeli hello összes adatszeletek, és elkezdi őket.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-322">When you do so, Data Factory automatically calculates (back fills) all data slices in hello past and begins processing them.</span></span> <span data-ttu-id="e7d2f-323">Példa: Ha a kezdő dátum 2017-04-01 hoz létre egy folyamatot, és hello aktuális dátum 2017-04-10.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-323">For example: if you create a pipeline with start date 2017-04-01 and hello current date is 2017-04-10.</span></span> <span data-ttu-id="e7d2f-324">Ha hello ütemben történik a hello kimeneti adatkészlet naponta, majd a Data Factory indítása 2017-04-01-től az összes hello szelet feldolgozása too2017-04-09 azonnal mert hello kezdő dátum van hello múltbeli.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-324">If hello cadence of hello output dataset is daily, then Data Factory starts processing all hello slices from 2017-04-01 too2017-04-09 immediately because hello start date is in hello past.</span></span> <span data-ttu-id="e7d2f-325">2017-04-10-hello szelet nem lett feldolgozva még mert hello hello rendelkezésre állással kapcsolatos szakaszának a style tulajdonságának értéke EndOfInterval alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-325">hello slice from 2017-04-10 is not processed yet because hello value of style property in hello availability section is EndOfInterval by default.</span></span> <span data-ttu-id="e7d2f-326">hello legrégebbi szelet feldolgozása először hello alapértelmezett executionPriorityOrder értéke OldestFirst.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-326">hello oldest slice is processed first as hello default value of executionPriorityOrder is OldestFirst.</span></span> <span data-ttu-id="e7d2f-327">Hello style tulajdonságának ismertetését lásd: [adatkészlet rendelkezésre állási](#dataset-availability) szakasz.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-327">For a description of hello style property, see [dataset availability](#dataset-availability) section.</span></span> <span data-ttu-id="e7d2f-328">Hello executionPriorityOrder szakasz ismertetését lásd: hello [tevékenység szabályzatai](#activity-policies) szakasz.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-328">For a description of hello executionPriorityOrder section, see hello [activity policies](#activity-policies) section.</span></span> 

<span data-ttu-id="e7d2f-329">Adatok biztonsági kitöltött szeletek toobe által hello beállítása párhuzamosan is beállíthat **egyidejűségi** hello tulajdonság **házirend** hello tevékenység JSON szakasza.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-329">You can configure back-filled data slices toobe processed in parallel by setting hello **concurrency** property in hello **policy** section of hello activity JSON.</span></span> <span data-ttu-id="e7d2f-330">Ez a tulajdonság meghatározza, hogy hello száma párhuzamos tevékenység végrehajtások, amely akkor fordulhat elő, a másik szeletek.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-330">This property determines hello number of parallel activity executions that can happen on different slices.</span></span> <span data-ttu-id="e7d2f-331">hello alapértelmezett hello párhuzamossági tulajdonság értéke 1.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-331">hello default value for hello concurrency property is 1.</span></span> <span data-ttu-id="e7d2f-332">Ezért egy szelet feldolgozása egyszerre alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-332">Therefore, one slice is processed at a time by default.</span></span> <span data-ttu-id="e7d2f-333">hello maximális érték: 10.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-333">hello maximum value is 10.</span></span> <span data-ttu-id="e7d2f-334">Ha egy folyamatot kell toogo keresztül a rendelkezésre álló adatok, nagyobb feldolgozási értéke számos felgyorsítja a hello adatok feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-334">When a pipeline needs toogo through a large set of available data, having a larger concurrency value speeds up hello data processing.</span></span> 

## <a name="rerun-a-failed-data-slice"></a><span data-ttu-id="e7d2f-335">Futtassa újra a sikertelen adatszelet</span><span class="sxs-lookup"><span data-stu-id="e7d2f-335">Rerun a failed data slice</span></span>
<span data-ttu-id="e7d2f-336">Adatok szelet feldolgozása során hiba lép fel, ha azt megtudhatja, miért szelet hello feldolgozása nem sikerült, az Azure portál paneleken vagy a figyelő és App kezelése.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-336">When an error occurs while processing a data slice, you can find out why hello processing of a slice failed by using Azure portal blades or Monitor and Manage App.</span></span> <span data-ttu-id="e7d2f-337">Lásd: [figyelése és kezelése az Azure portál paneleken használatával folyamatok](data-factory-monitor-manage-pipelines.md) vagy [figyelés és a felügyeleti alkalmazás](data-factory-monitor-manage-app.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-337">See [Monitoring and managing pipelines using Azure portal blades](data-factory-monitor-manage-pipelines.md) or [Monitoring and Management app](data-factory-monitor-manage-app.md) for details.</span></span>

<span data-ttu-id="e7d2f-338">Vegye figyelembe a következő példának, amely mutatja a két tevékenység hello.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-338">Consider hello following example, which shows two activities.</span></span> <span data-ttu-id="e7d2f-339">Activity1 és 2 tevékenység.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-339">Activity1 and Activity 2.</span></span> <span data-ttu-id="e7d2f-340">Activity1 Dataset1 szelet használ fel, és Dataset2, amely runbook az Activity2 tooproduce hello végső Dataset szelet által felhasznált bemeneti adatokként a szelet eredményez.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-340">Activity1 consumes a slice of Dataset1 and produces a slice of Dataset2, which is consumed as an input by Activity2 tooproduce a slice of hello Final Dataset.</span></span>

![Hibás szeletet](./media/data-factory-scheduling-and-execution/failed-slice.png)

<span data-ttu-id="e7d2f-342">hello ábra azt mutatja, hogy három legutóbbi szeletek, kívül történt hiba előállító hello 9-10 de szelet Dataset2 a.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-342">hello diagram shows that out of three recent slices, there was a failure producing hello 9-10 AM slice for Dataset2.</span></span> <span data-ttu-id="e7d2f-343">Adat-előállító automatikusan nyomon követi a függőségi hello idő adatsorozat adatkészlethez.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-343">Data Factory automatically tracks dependency for hello time series dataset.</span></span> <span data-ttu-id="e7d2f-344">Emiatt nem indul el hello tevékenységfuttatási hello Reggel 9-10 alárendelt adatszelethez.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-344">As a result, it does not start hello activity run for hello 9-10 AM downstream slice.</span></span>

<span data-ttu-id="e7d2f-345">Data Factory figyelése és a felügyeleti eszközöket oszthatja toodrill hello diagnosztikai naplók hello hibás szeletet tooeasily keresés hello legfelső szintű hello probléma okozza, és javítsa ki azt.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-345">Data Factory monitoring and management tools allow you toodrill into hello diagnostic logs for hello failed slice tooeasily find hello root cause for hello issue and fix it.</span></span> <span data-ttu-id="e7d2f-346">Hello a probléma kijavítása után futtassa tooproduce hello hibás szeletet hello tevékenység könnyen megkezdheti.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-346">After you have fixed hello issue, you can easily start hello activity run tooproduce hello failed slice.</span></span> <span data-ttu-id="e7d2f-347">További információt a toorerun és állapotváltozási adat áramlik az adatok megismeréséhez, lásd: [figyelése és kezelése az Azure portál paneleken használatával folyamatok](data-factory-monitor-manage-pipelines.md) vagy [figyelés és a felügyeleti alkalmazás](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="e7d2f-347">For more information on how toorerun and understand state transitions for data slices, see [Monitoring and managing pipelines using Azure portal blades](data-factory-monitor-manage-pipelines.md) or [Monitoring and Management app](data-factory-monitor-manage-app.md).</span></span>

<span data-ttu-id="e7d2f-348">Után futtassa újra a hello 9-10 AM a szelet **Dataset2**, adat-előállító hello 9-10 de függő adatszelethez futtatnak hello végső dataset hello kezdődik.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-348">After you rerun hello 9-10 AM slice for **Dataset2**, Data Factory starts hello run for hello 9-10 AM dependent slice on hello final dataset.</span></span>

![Futtassa újra a sikertelen szelet](./media/data-factory-scheduling-and-execution/rerun-failed-slice.png)

## <a name="multiple-activities-in-a-pipeline"></a><span data-ttu-id="e7d2f-350">Több tevékenység egy adott folyamatban</span><span class="sxs-lookup"><span data-stu-id="e7d2f-350">Multiple activities in a pipeline</span></span>
<span data-ttu-id="e7d2f-351">Egy folyamathoz azonban több tevékenység is tartozhat.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-351">You can have more than one activity in a pipeline.</span></span> <span data-ttu-id="e7d2f-352">Ha több tevékenység rendelkezik egy folyamatot, és a hello tevékenység kimenete nem egy másik tevékenység bemeneti, hello tevékenységek párhuzamos futtathatja, ha készen áll a bemeneti adatok szeletek hello tevékenységekhez.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-352">If you have multiple activities in a pipeline and hello output of an activity is not an input of another activity, hello activities may run in parallel if input data slices for hello activities are ready.</span></span>

<span data-ttu-id="e7d2f-353">Hello kimeneti adatkészlet egy tevékenység beállítását hello bemeneti hello az adatkészlet többi tevékenység által láncolt lehessen két tevékenység (egy tevékenység egymás után).</span><span class="sxs-lookup"><span data-stu-id="e7d2f-353">You can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="e7d2f-354">hello tevékenységek hello lehetnek azonos vagy különböző kimenetátirányítási.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-354">hello activities can be in hello same pipeline or in different pipelines.</span></span> <span data-ttu-id="e7d2f-355">hello második tevékenység végrehajtása csak akkor, ha hello először egy futtatása sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-355">hello second activity executes only when hello first one finishes successfully.</span></span>

<span data-ttu-id="e7d2f-356">Vegyük példaként a következő esetet, ahol a folyamat két tevékenység rendelkezik hello:</span><span class="sxs-lookup"><span data-stu-id="e7d2f-356">For example, consider hello following case where a pipeline has two activities:</span></span>

1. <span data-ttu-id="e7d2f-357">Külső bemeneti adatkészlet D1, és az előállított kimeneti adatkészlet D2 igénylő tevékenységek A1.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-357">Activity A1 that requires external input dataset D1, and produces output dataset D2.</span></span>
2. <span data-ttu-id="e7d2f-358">A kimeneti adatkészlet D3 tevékenység A2 D2 adatkészletből beavatkozást igényel, és hozza létre.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-358">Activity A2 that requires input from dataset D2, and produces output dataset D3.</span></span>

<span data-ttu-id="e7d2f-359">Ez a forgatókönyv, tevékenységek A1 és A2 vannak a hello azonos a következő feldolgozási sorban.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-359">In this scenario, activities A1 and A2 are in hello same pipeline.</span></span> <span data-ttu-id="e7d2f-360">hello tevékenység A1 fut, amikor hello külső adatok érhető el, és ütemezett hello rendelkezésre állási gyakoriság éri el.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-360">hello activity A1 runs when hello external data is available and hello scheduled availability frequency is reached.</span></span> <span data-ttu-id="e7d2f-361">hello tevékenység A2 fut, amikor hello a D2 ütemezett szeletek rendelkezésre állására, és hello ütemezett rendelkezésre állása gyakoriság éri el.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-361">hello activity A2 runs when hello scheduled slices from D2 become available and hello scheduled availability frequency is reached.</span></span> <span data-ttu-id="e7d2f-362">Ha nincs hiba fordult elő egy adatkészlet D2 hello szeletek, A2 nem fut a, hogy a szelet amíg elérhetővé válik.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-362">If there is an error in one of hello slices in dataset D2, A2 does not run for that slice until it becomes available.</span></span>

<span data-ttu-id="e7d2f-363">Diagram nézet hello mindkét tevékenységeinek azonos jelenne meg a következő diagram hello hello:</span><span class="sxs-lookup"><span data-stu-id="e7d2f-363">hello Diagram view with both activities in hello same pipeline would look like hello following diagram:</span></span>

![Láncolás hello tevékenységek ugyanazt a következő feldolgozási sorban](./media/data-factory-scheduling-and-execution/chaining-one-pipeline.png)

<span data-ttu-id="e7d2f-365">A korábban említett hello tevékenységek a különböző folyamatok lehet.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-365">As mentioned earlier, hello activities could be in different pipelines.</span></span> <span data-ttu-id="e7d2f-366">Ilyen esetben a következő diagram hello hello diagramnézet jelenne meg:</span><span class="sxs-lookup"><span data-stu-id="e7d2f-366">In such a scenario, hello diagram view would look like hello following diagram:</span></span>

![Két kimenetátirányítási titkosításblokkoló tevékenységek](./media/data-factory-scheduling-and-execution/chaining-two-pipelines.png)

<span data-ttu-id="e7d2f-368">Lásd: hello [egymás után másolja](#copy-sequentially) szakasz hello függelékben példát.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-368">See hello [copy sequentially](#copy-sequentially) section in hello appendix for an example.</span></span>

## <a name="model-datasets-with-different-frequencies"></a><span data-ttu-id="e7d2f-369">Eltérő gyakorisággal modell adatkészletek</span><span class="sxs-lookup"><span data-stu-id="e7d2f-369">Model datasets with different frequencies</span></span>
<span data-ttu-id="e7d2f-370">Hello minták, a bemeneti és kimeneti adatkészletek és hello tevékenység ütemezési ablak a hello gyakoriságot volt hello azonos.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-370">In hello samples, hello frequencies for input and output datasets and hello activity schedule window were hello same.</span></span> <span data-ttu-id="e7d2f-371">Egyes esetekben szükséges hello képességét tooproduce kimeneti egy vagy több bemeneti hello gyakoriságát eltérő gyakorisággal.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-371">Some scenarios require hello ability tooproduce output at a frequency different than hello frequencies of one or more inputs.</span></span> <span data-ttu-id="e7d2f-372">Adat-előállító támogatja a modellezési forgatókönyvekben.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-372">Data Factory supports modeling these scenarios.</span></span>

### <a name="sample-1-produce-a-daily-output-report-for-input-data-that-is-available-every-hour"></a><span data-ttu-id="e7d2f-373">1. példa: A bemeneti adatok óránként napi kimeneti jelentést készít.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-373">Sample 1: Produce a daily output report for input data that is available every hour</span></span>
<span data-ttu-id="e7d2f-374">Fontolja meg egy olyan forgatókönyvet, amelyben van megadott mérési adatokat az érzékelők érhető el az Azure Blob storage óránként.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-374">Consider a scenario in which you have input measurement data from sensors available every hour in Azure Blob storage.</span></span> <span data-ttu-id="e7d2f-375">Hello nap tooproduce statisztikákról, például átlag, maximális és minimális napi összesítő jelentés kívánt [adat-előállító hive tevékenység](data-factory-hive-activity.md).</span><span class="sxs-lookup"><span data-stu-id="e7d2f-375">You want tooproduce a daily aggregate report with statistics such as mean, maximum, and minimum for hello day with [Data Factory hive activity](data-factory-hive-activity.md).</span></span>

<span data-ttu-id="e7d2f-376">Hogyan modellezhető ebben a forgatókönyvben a Data Factory itt található:</span><span class="sxs-lookup"><span data-stu-id="e7d2f-376">Here is how you can model this scenario with Data Factory:</span></span>

<span data-ttu-id="e7d2f-377">**Bemeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="e7d2f-377">**Input dataset**</span></span>

<span data-ttu-id="e7d2f-378">hello óránkénti bemeneti fájlok dobja a megadott nap hello hello mappájában.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-378">hello hourly input files are dropped in hello folder for hello given day.</span></span> <span data-ttu-id="e7d2f-379">Bemeneti rendelkezésre állásra van beállítva **óra** (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="e7d2f-379">Availability for input is set at **Hour** (frequency: Hour, interval: 1).</span></span>

```json
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
<span data-ttu-id="e7d2f-380">**Kimeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="e7d2f-380">**Output dataset**</span></span>

<span data-ttu-id="e7d2f-381">Egy kimeneti fájl hello nap mappában jön létre naponta.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-381">One output file is created every day in hello day's folder.</span></span> <span data-ttu-id="e7d2f-382">Kimeneti rendelkezésre állását nem állítják **nap** (gyakoriság: nap és időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="e7d2f-382">Availability of output is set at **Day** (frequency: Day and interval: 1).</span></span>

```json
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

<span data-ttu-id="e7d2f-383">**Tevékenység: struktúra egy feldolgozási soros tevékenység**</span><span class="sxs-lookup"><span data-stu-id="e7d2f-383">**Activity: hive activity in a pipeline**</span></span>

<span data-ttu-id="e7d2f-384">hello hive parancsfájl kap megfelelő hello *DateTime* hello használó paraméterek adatként **WindowStart** változó, ahogy az alábbi részlet hello.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-384">hello hive script receives hello appropriate *DateTime* information as parameters that use hello **WindowStart** variable as shown in hello following snippet.</span></span> <span data-ttu-id="e7d2f-385">hello hive parancsfájlokat használ a változó tooload hello adatok hello megfelelő mappából hello napon, és futtassa a hello összesítési toogenerate hello kimeneti.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-385">hello hive script uses this variable tooload hello data from hello correct folder for hello day and run hello aggregation toogenerate hello output.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-01-01T08:00:00",
    "end":"2015-01-01T11:00:00",
    "description":"hive activity",
    "activities": [
        {
            "name": "SampleHiveActivity",
            "inputs": [
                {
                    "name": "AzureBlobInput"
                }
            ],
            "outputs": [
                {
                    "name": "AzureBlobOutput"
                }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "type": "HDInsightHive",
            "typeProperties": {
                "scriptPath": "adftutorial\\hivequery.hql",
                "scriptLinkedService": "StorageLinkedService",
                "defines": {
                    "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                    "Month": "$$Text.Format('{0:MM}',WindowStart)",
                    "Day": "$$Text.Format('{0:dd}',WindowStart)"
                }
            },
            "scheduler": {
                "frequency": "Day",
                "interval": 1
            },            
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 2,
                "timeout": "01:00:00"
            }
         }
     ]
   }
}
```

<span data-ttu-id="e7d2f-386">hello alábbi ábrán látható hello forgatókönyv egy adat-függőség szempontjából.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-386">hello following diagram shows hello scenario from a data-dependency point of view.</span></span>

![Függőség](./media/data-factory-scheduling-and-execution/data-dependency.png)

<span data-ttu-id="e7d2f-388">hello kimeneti szelet esetén minden nap 24 óránként szeletek a egy bemeneti adatkészlet függ.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-388">hello output slice for every day depends on 24 hourly slices from an input dataset.</span></span> <span data-ttu-id="e7d2f-389">Data Factory kiszámítja ezeket a függőségeket, hogy tudja által automatikusan hello bemeneti adatszeletek, amely az hello azonos időszak szerint előállított kimeneti szelet toobe hello.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-389">Data Factory computes these dependencies automatically by figuring out hello input data slices that fall in hello same time period as hello output slice toobe produced.</span></span> <span data-ttu-id="e7d2f-390">Ha hello 24 bemeneti szeletek bármelyike nem érhető el, adat-előállító megvárja-e a hello bemeneti szelet toobe készen áll a hello napi tevékenységfuttatási megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-390">If any of hello 24 input slices is not available, Data Factory waits for hello input slice toobe ready before starting hello daily activity run.</span></span>

### <a name="sample-2-specify-dependency-with-expressions-and-data-factory-functions"></a><span data-ttu-id="e7d2f-391">2. példa: Adja meg a függőségi kifejezések és a Data Factory-funkciók</span><span class="sxs-lookup"><span data-stu-id="e7d2f-391">Sample 2: Specify dependency with expressions and Data Factory functions</span></span>
<span data-ttu-id="e7d2f-392">Mérlegeljük, egy másik helyzet.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-392">Let’s consider another scenario.</span></span> <span data-ttu-id="e7d2f-393">Tegyük fel, a hive tevékenység, amely feldolgozza a két bemeneti adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-393">Suppose you have a hive activity that processes two input datasets.</span></span> <span data-ttu-id="e7d2f-394">Egyik új adatokat naponta, de egyikük kap új adatokat minden héten.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-394">One of them has new data daily, but one of them gets new data every week.</span></span> <span data-ttu-id="e7d2f-395">Tegyük toodo illesztés hello két bemenet közötti, majd előállít egy kimenő minden nap.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-395">Suppose you wanted toodo a join across hello two inputs and produce an output every day.</span></span>

<span data-ttu-id="e7d2f-396">hello egyszerű módszert, amelyben Data Factory automatikusan adatok kimenő hello jobb bemeneti tooprocess szeletek egymáshoz igazításával toohello kimeneti adatok szelet időszak nem működik.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-396">hello simple approach in which Data Factory automatically figures out hello right input slices tooprocess by aligning toohello output data slice’s time period does not work.</span></span>

<span data-ttu-id="e7d2f-397">Meg kell adnia, hogy minden tevékenységfuttatási hello adat-előállító érdemes használni a múlt héten adatszelet hello heti bemeneti adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-397">You must specify that for every activity run, hello Data Factory should use last week’s data slice for hello weekly input dataset.</span></span> <span data-ttu-id="e7d2f-398">Használhatja az Azure Data Factory funkciók, ahogy az alábbi részlet tooimplement hello ezt a viselkedést.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-398">You use Azure Data Factory functions as shown in hello following snippet tooimplement this behavior.</span></span>

<span data-ttu-id="e7d2f-399">**Input1: Az Azure blob**</span><span class="sxs-lookup"><span data-stu-id="e7d2f-399">**Input1: Azure blob**</span></span>

<span data-ttu-id="e7d2f-400">hello első bemenet hello Azure blob alatt naponta frissül.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-400">hello first input is hello Azure blob being updated daily.</span></span>

```json
{
  "name": "AzureBlobInputDaily",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

<span data-ttu-id="e7d2f-401">**Input2: Az Azure blob**</span><span class="sxs-lookup"><span data-stu-id="e7d2f-401">**Input2: Azure blob**</span></span>

<span data-ttu-id="e7d2f-402">Input2 hello Azure blob hetente frissítése folyamatban.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-402">Input2 is hello Azure blob being updated weekly.</span></span>

```json
{
  "name": "AzureBlobInputWeekly",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 7
    }
  }
}
```

<span data-ttu-id="e7d2f-403">**Kimenete: Az Azure blob**</span><span class="sxs-lookup"><span data-stu-id="e7d2f-403">**Output: Azure blob**</span></span>

<span data-ttu-id="e7d2f-404">Egy kimeneti fájl létrejön naponta hello mappában hello nap.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-404">One output file is created every day in hello folder for hello day.</span></span> <span data-ttu-id="e7d2f-405">Kimeneti rendelkezésre állását értéke túl**nap** (gyakoriság: nap, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="e7d2f-405">Availability of output is set too**day** (frequency: Day, interval: 1).</span></span>

```json
{
  "name": "AzureBlobOutputDaily",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

<span data-ttu-id="e7d2f-406">**Tevékenység: struktúra egy feldolgozási soros tevékenység**</span><span class="sxs-lookup"><span data-stu-id="e7d2f-406">**Activity: hive activity in a pipeline**</span></span>

<span data-ttu-id="e7d2f-407">hello hive tevékenység hello két bemeneti fogad, és egy kimeneti szelet naponta eredményez.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-407">hello hive activity takes hello two inputs and produces an output slice every day.</span></span> <span data-ttu-id="e7d2f-408">Minden nap kimeneti szelet toodepend a következőképpen hello az előző hét bemeneti szelet heti bemeneti adhatja meg.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-408">You can specify every day’s output slice toodepend on hello previous week’s input slice for weekly input as follows.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-01-01T08:00:00",
    "end":"2015-01-01T11:00:00",
    "description":"hive activity",
    "activities": [
      {
        "name": "SampleHiveActivity",
        "inputs": [
          {
            "name": "AzureBlobInputDaily"
          },
          {
            "name": "AzureBlobInputWeekly",
            "startTime": "Date.AddDays(SliceStart, - Date.DayOfWeek(SliceStart))",
            "endTime": "Date.AddDays(SliceEnd,  -Date.DayOfWeek(SliceEnd))"  
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutputDaily"
          }
        ],
        "linkedServiceName": "HDInsightLinkedService",
        "type": "HDInsightHive",
        "typeProperties": {
          "scriptPath": "adftutorial\\hivequery.hql",
          "scriptLinkedService": "StorageLinkedService",
          "defines": {
            "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
            "Month": "$$Text.Format('{0:MM}',WindowStart)",
            "Day": "$$Text.Format('{0:dd}',WindowStart)"
          }
        },
        "scheduler": {
          "frequency": "Day",
          "interval": 1
        },            
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 2,  
          "timeout": "01:00:00"
        }
       }
     ]
   }
}
```

<span data-ttu-id="e7d2f-409">Lásd: [adat-előállító funkciók és rendszerváltozók](data-factory-functions-variables.md) funkciók és a Data Factory támogató rendszerváltozók listáját.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-409">See [Data Factory functions and system variables](data-factory-functions-variables.md) for a list of functions and system variables that Data Factory supports.</span></span>

## <a name="appendix"></a><span data-ttu-id="e7d2f-410">Függelék:</span><span class="sxs-lookup"><span data-stu-id="e7d2f-410">Appendix</span></span>

### <a name="example-copy-sequentially"></a><span data-ttu-id="e7d2f-411">Példa: egymás után másolása</span><span class="sxs-lookup"><span data-stu-id="e7d2f-411">Example: copy sequentially</span></span>
<span data-ttu-id="e7d2f-412">Azt az lehetséges toorun több másolási műveletek egymás után szekvenciális/rendezett módon.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-412">It is possible toorun multiple copy operations one after another in a sequential/ordered manner.</span></span> <span data-ttu-id="e7d2f-413">Például lehetséges, hogy két másolási folyamat (CopyActivity1 és CopyActivity2) a következő hello tevékenységek bemeneti adatok kimeneti adatkészletek:</span><span class="sxs-lookup"><span data-stu-id="e7d2f-413">For example, you might have two copy activities in a pipeline (CopyActivity1 and CopyActivity2) with hello following input data output datasets:</span></span>   

<span data-ttu-id="e7d2f-414">CopyActivity1</span><span class="sxs-lookup"><span data-stu-id="e7d2f-414">CopyActivity1</span></span>

<span data-ttu-id="e7d2f-415">Bemenet: Dataset.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-415">Input: Dataset.</span></span> <span data-ttu-id="e7d2f-416">Kimenet: Dataset2.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-416">Output: Dataset2.</span></span>

<span data-ttu-id="e7d2f-417">CopyActivity2</span><span class="sxs-lookup"><span data-stu-id="e7d2f-417">CopyActivity2</span></span>

<span data-ttu-id="e7d2f-418">Bemenet: Dataset2.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-418">Input: Dataset2.</span></span>  <span data-ttu-id="e7d2f-419">Kimenet: Dataset3.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-419">Output: Dataset3.</span></span>

<span data-ttu-id="e7d2f-420">CopyActivity2 fog futni, ha hello CopyActivity1 végrehajtása sikeresen befejeződött, és Dataset2 érhető el.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-420">CopyActivity2 would run only if hello CopyActivity1 has run successfully and Dataset2 is available.</span></span>

<span data-ttu-id="e7d2f-421">Hello minta adatcsatorna JSON itt található:</span><span class="sxs-lookup"><span data-stu-id="e7d2f-421">Here is hello sample pipeline JSON:</span></span>

```json
{
    "name": "ChainActivities",
    "properties": {
        "description": "Run activities in sequence",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset1"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob1ToBlob2",
                "description": "Copy data from a blob tooanother"
            },
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset3"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob2ToBlob3",
                "description": "Copy data from a blob tooanother"
            }
        ],
        "start": "2016-08-25T01:00:00Z",
        "end": "2016-08-25T01:00:00Z",
        "isPaused": false
    }
}
```

<span data-ttu-id="e7d2f-422">Figyelje meg, hogy hello példában hello kimeneti adatkészlet a hello első másolási tevékenység (Dataset2) van megadva hello második tevékenység bemeneti adatként.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-422">Notice that in hello example, hello output dataset of hello first copy activity (Dataset2) is specified as input for hello second activity.</span></span> <span data-ttu-id="e7d2f-423">Hello második tevékenység fut, ezért csak akkor, amikor készen áll a hello kimeneti adatkészlet hello első tevékenységből.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-423">Therefore, hello second activity runs only when hello output dataset from hello first activity is ready.</span></span>  

<span data-ttu-id="e7d2f-424">Hello példában CopyActivity2 egy másik bemeneti Dataset3, például rendelkezhet, de ad meg Dataset2 egy bemeneti tooCopyActivity2, így hello tevékenység CopyActivity1 befejezéséig nem működik.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-424">In hello example, CopyActivity2 can have a different input, such as Dataset3, but you specify Dataset2 as an input tooCopyActivity2, so hello activity does not run until CopyActivity1 finishes.</span></span> <span data-ttu-id="e7d2f-425">Példa:</span><span class="sxs-lookup"><span data-stu-id="e7d2f-425">For example:</span></span>

<span data-ttu-id="e7d2f-426">CopyActivity1</span><span class="sxs-lookup"><span data-stu-id="e7d2f-426">CopyActivity1</span></span>

<span data-ttu-id="e7d2f-427">Bemenet: Dataset1.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-427">Input: Dataset1.</span></span> <span data-ttu-id="e7d2f-428">Kimenet: Dataset2.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-428">Output: Dataset2.</span></span>

<span data-ttu-id="e7d2f-429">CopyActivity2</span><span class="sxs-lookup"><span data-stu-id="e7d2f-429">CopyActivity2</span></span>

<span data-ttu-id="e7d2f-430">Bemenetek: Dataset3, Dataset2.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-430">Inputs: Dataset3, Dataset2.</span></span> <span data-ttu-id="e7d2f-431">Kimenet: Dataset4.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-431">Output: Dataset4.</span></span>

```json
{
    "name": "ChainActivities",
    "properties": {
        "description": "Run activities in sequence",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset1"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlobToBlob",
                "description": "Copy data from a blob tooanother"
            },
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset3"
                    },
                    {
                        "name": "Dataset2"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset4"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob3ToBlob4",
                "description": "Copy data from a blob tooanother"
            }
        ],
        "start": "2017-04-25T01:00:00Z",
        "end": "2017-04-25T01:00:00Z",
        "isPaused": false
    }
}
```

<span data-ttu-id="e7d2f-432">Figyelje meg, hogy hello példában két bemeneti adatkészletek vannak megadva hello második másolási tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-432">Notice that in hello example, two input datasets are specified for hello second copy activity.</span></span> <span data-ttu-id="e7d2f-433">Ha több bemeneti adatok meg vannak adva, csak hello első bemeneti adatkészletet szolgál az adatok másolásának, de más adatkészletek függőségek használatosak.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-433">When multiple inputs are specified, only hello first input dataset is used for copying data, but other datasets are used as dependencies.</span></span> <span data-ttu-id="e7d2f-434">CopyActivity2 kezdenie csak azt követően hello következő feltételek teljesülnek:</span><span class="sxs-lookup"><span data-stu-id="e7d2f-434">CopyActivity2 would start only after hello following conditions are met:</span></span>

* <span data-ttu-id="e7d2f-435">CopyActivity1 sikeresen befejeződött, és Dataset2 érhető el.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-435">CopyActivity1 has successfully completed and Dataset2 is available.</span></span> <span data-ttu-id="e7d2f-436">Ez az adatkészlet nem használt adatok tooDataset4 másolásakor.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-436">This dataset is not used when copying data tooDataset4.</span></span> <span data-ttu-id="e7d2f-437">Csak működik ütemezési függőségei a CopyActivity2.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-437">It only acts as a scheduling dependency for CopyActivity2.</span></span>   
* <span data-ttu-id="e7d2f-438">Dataset3 érhető el.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-438">Dataset3 is available.</span></span> <span data-ttu-id="e7d2f-439">Ez az adatkészlet másolt toohello cél hello adatot jelöli.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-439">This dataset represents hello data that is copied toohello destination.</span></span> 
