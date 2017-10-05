---
title: "Az Azure Data Factory - JSON-Parancsprogramokról |} Microsoft Docs"
description: "JSON-sémákat biztosít a Data Factory-entitásokhoz."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 805106c0a5cdbff1f143f22a2ae59f6d2a0bf126
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-data-factory---json-scripting-reference"></a><span data-ttu-id="cc737-103">Az Azure Data Factory - JSON-Parancsprogramokról</span><span class="sxs-lookup"><span data-stu-id="cc737-103">Azure Data Factory - JSON Scripting Reference</span></span>
<span data-ttu-id="cc737-104">A cikkben JSON-sémákat és példák meghatározásához az Azure Data Factory entitások (adatcsatorna, tevékenység, adatkészlet és társított szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="cc737-104">This article provides JSON schemas and examples for defining Azure Data Factory entities (pipeline, activity, dataset, and linked service).</span></span>  

## <a name="pipeline"></a><span data-ttu-id="cc737-105">Folyamat</span><span class="sxs-lookup"><span data-stu-id="cc737-105">Pipeline</span></span> 
<span data-ttu-id="cc737-106">A magas szintű struktúra folyamat meghatározása a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="cc737-106">The high-level structure for a pipeline definition is as follows:</span></span> 

```json
{
  "name": "SamplePipeline",
  "properties": {
    "description": "Describe what pipeline does",
    "activities": [
    ],
    "start": "2016-07-12T00:00:00",
    "end": "2016-07-13T00:00:00"
  }
} 
```

<span data-ttu-id="cc737-107">Következő táblázat az adatcsatorna JSON-definícióból a tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="cc737-107">Following table describes the properties within the pipeline JSON definition:</span></span>

| <span data-ttu-id="cc737-108">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-108">Property</span></span> | <span data-ttu-id="cc737-109">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-109">Description</span></span> | <span data-ttu-id="cc737-110">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-110">Required</span></span>
-------- | ----------- | --------
| <span data-ttu-id="cc737-111">név</span><span class="sxs-lookup"><span data-stu-id="cc737-111">name</span></span> | <span data-ttu-id="cc737-112">A folyamat neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-112">Name of the pipeline.</span></span> <span data-ttu-id="cc737-113">Adjon meg egy nevet a műveletet jelenti, hogy a tevékenység vagy csővezeték van konfigurálva</span><span class="sxs-lookup"><span data-stu-id="cc737-113">Specify a name that represents the action that the activity or pipeline is configured to do</span></span><br/><ul><li><span data-ttu-id="cc737-114">Karakterek maximális száma: 260</span><span class="sxs-lookup"><span data-stu-id="cc737-114">Maximum number of characters: 260</span></span></li><li><span data-ttu-id="cc737-115">Számnak betűvel vagy aláhúzással (_) kell kezdődnie</span><span class="sxs-lookup"><span data-stu-id="cc737-115">Must start with a letter number, or an underscore (_)</span></span></li><li><span data-ttu-id="cc737-116">Következő karakterek nem engedélyezettek: ".", "+","?", "/", "<",">", "*", "%", "&", ":","\\"</span><span class="sxs-lookup"><span data-stu-id="cc737-116">Following characters are not allowed: “.”, “+”, “?”, “/”, “<”,”>”,”*”,”%”,”&”,”:”,”\\”</span></span></li></ul> |<span data-ttu-id="cc737-117">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-117">Yes</span></span> |
| <span data-ttu-id="cc737-118">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-118">description</span></span> |<span data-ttu-id="cc737-119">A tevékenység vagy csővezeték alkalmazott leíró szöveg</span><span class="sxs-lookup"><span data-stu-id="cc737-119">Text describing what the activity or pipeline is used for</span></span> | <span data-ttu-id="cc737-120">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-120">No</span></span> |
| <span data-ttu-id="cc737-121">tevékenységek</span><span class="sxs-lookup"><span data-stu-id="cc737-121">activities</span></span> | <span data-ttu-id="cc737-122">A tevékenységek listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="cc737-122">Contains a list of activities.</span></span> | <span data-ttu-id="cc737-123">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-123">Yes</span></span> |
| <span data-ttu-id="cc737-124">start</span><span class="sxs-lookup"><span data-stu-id="cc737-124">start</span></span> |<span data-ttu-id="cc737-125">Kezdő dátum-idő az adatcsatornához.</span><span class="sxs-lookup"><span data-stu-id="cc737-125">Start date-time for the pipeline.</span></span> <span data-ttu-id="cc737-126">Meg kell [ISO formátum](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="cc737-126">Must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="cc737-127">Például: 2014-10-14T16:32:41.</span><span class="sxs-lookup"><span data-stu-id="cc737-127">For example: 2014-10-14T16:32:41.</span></span> <br/><br/><span data-ttu-id="cc737-128">Akkor adja meg a helyi időt, például egy keleti TÉLI idő lehet.</span><span class="sxs-lookup"><span data-stu-id="cc737-128">It is possible to specify a local time, for example an EST time.</span></span> <span data-ttu-id="cc737-129">Példa: `2016-02-27T06:00:00**-05:00`, vagyis 6 AM becsült</span><span class="sxs-lookup"><span data-stu-id="cc737-129">Here is an example: `2016-02-27T06:00:00**-05:00`, which is 6 AM EST.</span></span><br/><br/><span data-ttu-id="cc737-130">A kezdő és záró tulajdonságok együtt adja meg az adatcsatorna aktív időszakát.</span><span class="sxs-lookup"><span data-stu-id="cc737-130">The start and end properties together specify active period for the pipeline.</span></span> <span data-ttu-id="cc737-131">Kimeneti szeletek csak előállítása és az aktív időszakban.</span><span class="sxs-lookup"><span data-stu-id="cc737-131">Output slices are only produced with in this active period.</span></span> |<span data-ttu-id="cc737-132">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-132">No</span></span><br/><br/><span data-ttu-id="cc737-133">Ha end tulajdonság értékét adja meg, meg kell adnia a kezdő tulajdonság értéke.</span><span class="sxs-lookup"><span data-stu-id="cc737-133">If you specify a value for the end property, you must specify value for the start property.</span></span><br/><br/><span data-ttu-id="cc737-134">A kezdő és befejező időpontja is lehet folyamatokat létrehozni üres.</span><span class="sxs-lookup"><span data-stu-id="cc737-134">The start and end times can both be empty to create a pipeline.</span></span> <span data-ttu-id="cc737-135">Állítsa az aktív időszakot futtatásához a tölcsér mindkét értéket meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="cc737-135">You must specify both values to set an active period for the pipeline to run.</span></span> <span data-ttu-id="cc737-136">Ha nem adja meg a kezdési és befejezési időpontjai folyamat létrehozásakor beállíthatja azokat később a Set-AzureRmDataFactoryPipelineActivePeriod parancsmaggal.</span><span class="sxs-lookup"><span data-stu-id="cc737-136">If you do not specify start and end times when creating a pipeline, you can set them using the Set-AzureRmDataFactoryPipelineActivePeriod cmdlet later.</span></span> |
| <span data-ttu-id="cc737-137">Vége</span><span class="sxs-lookup"><span data-stu-id="cc737-137">end</span></span> |<span data-ttu-id="cc737-138">Befejező dátum-idő az adatcsatorna.</span><span class="sxs-lookup"><span data-stu-id="cc737-138">End date-time for the pipeline.</span></span> <span data-ttu-id="cc737-139">Ha a megadott ISO-formátumban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="cc737-139">If specified must be in ISO format.</span></span> <span data-ttu-id="cc737-140">Például: 2014-10-14T17:32:41</span><span class="sxs-lookup"><span data-stu-id="cc737-140">For example: 2014-10-14T17:32:41</span></span> <br/><br/><span data-ttu-id="cc737-141">Akkor adja meg a helyi időt, például egy keleti TÉLI idő lehet.</span><span class="sxs-lookup"><span data-stu-id="cc737-141">It is possible to specify a local time, for example an EST time.</span></span> <span data-ttu-id="cc737-142">Példa: `2016-02-27T06:00:00**-05:00`, vagyis 6 AM becsült</span><span class="sxs-lookup"><span data-stu-id="cc737-142">Here is an example: `2016-02-27T06:00:00**-05:00`, which is 6 AM EST.</span></span><br/><br/><span data-ttu-id="cc737-143">A folyamat határozatlan idejű futásra, adja meg a 9999-09-09 end tulajdonság értékét.</span><span class="sxs-lookup"><span data-stu-id="cc737-143">To run the pipeline indefinitely, specify 9999-09-09 as the value for the end property.</span></span> |<span data-ttu-id="cc737-144">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-144">No</span></span> <br/><br/><span data-ttu-id="cc737-145">Ha a start tulajdonság értékét adja meg, meg kell adnia end tulajdonság értéke.</span><span class="sxs-lookup"><span data-stu-id="cc737-145">If you specify a value for the start property, you must specify value for the end property.</span></span><br/><br/><span data-ttu-id="cc737-146">Tekintse meg a megjegyzéseket az **start** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="cc737-146">See notes for the **start** property.</span></span> |
| <span data-ttu-id="cc737-147">isPaused</span><span class="sxs-lookup"><span data-stu-id="cc737-147">isPaused</span></span> |<span data-ttu-id="cc737-148">Ha a feldolgozási sor igaz értékre való beállítása nem működik.</span><span class="sxs-lookup"><span data-stu-id="cc737-148">If set to true the pipeline does not run.</span></span> <span data-ttu-id="cc737-149">Alapértelmezett érték = false.</span><span class="sxs-lookup"><span data-stu-id="cc737-149">Default value = false.</span></span> <span data-ttu-id="cc737-150">Ez a tulajdonság segítségével engedélyezheti vagy tilthatja le.</span><span class="sxs-lookup"><span data-stu-id="cc737-150">You can use this property to enable or disable.</span></span> |<span data-ttu-id="cc737-151">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-151">No</span></span> |
| <span data-ttu-id="cc737-152">pipelineMode</span><span class="sxs-lookup"><span data-stu-id="cc737-152">pipelineMode</span></span> |<span data-ttu-id="cc737-153">Az ütemezési módszer a következő feldolgozási sor fut.</span><span class="sxs-lookup"><span data-stu-id="cc737-153">The method for scheduling runs for the pipeline.</span></span> <span data-ttu-id="cc737-154">Két érték engedélyezett: (alapértelmezett), ütemezett alkalommal.</span><span class="sxs-lookup"><span data-stu-id="cc737-154">Allowed values are: scheduled (default), onetime.</span></span><br/><br/><span data-ttu-id="cc737-155">"Ütemezett" azt jelzi, hogy a folyamat az aktív időszak (kezdő és záró idő) alapján meghatározott időközönként.</span><span class="sxs-lookup"><span data-stu-id="cc737-155">‘Scheduled’ indicates that the pipeline runs at a specified time interval according to its active period (start and end time).</span></span> <span data-ttu-id="cc737-156">"Alkalommal" jelzi, hogy a folyamat csak egyszer fut-e.</span><span class="sxs-lookup"><span data-stu-id="cc737-156">‘Onetime’ indicates that the pipeline runs only once.</span></span> <span data-ttu-id="cc737-157">Létrehozását követően alkalommal adatcsatornák nem lehet módosítani/frissített jelenleg.</span><span class="sxs-lookup"><span data-stu-id="cc737-157">Onetime pipelines once created cannot be modified/updated currently.</span></span> <span data-ttu-id="cc737-158">Lásd: [Onetime csővezeték](data-factory-create-pipelines.md#onetime-pipeline) alkalommal beállítás vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="cc737-158">See [Onetime pipeline](data-factory-create-pipelines.md#onetime-pipeline) for details about onetime setting.</span></span> |<span data-ttu-id="cc737-159">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-159">No</span></span> |
| <span data-ttu-id="cc737-160">ExpirationTime</span><span class="sxs-lookup"><span data-stu-id="cc737-160">expirationTime</span></span> |<span data-ttu-id="cc737-161">Időtartam, amelyek a feldolgozási sor érvényes, és kiépített maradjon létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="cc737-161">Duration of time after creation for which the pipeline is valid and should remain provisioned.</span></span> <span data-ttu-id="cc737-162">Ha nem rendelkezik minden aktív sikertelen volt, vagy függőben lévő fut, a folyamat a rendszer automatikusan törli lejárati időpont után.</span><span class="sxs-lookup"><span data-stu-id="cc737-162">If it does not have any active, failed, or pending runs, the pipeline is deleted automatically once it reaches the expiration time.</span></span> |<span data-ttu-id="cc737-163">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-163">No</span></span> |


## <a name="activity"></a><span data-ttu-id="cc737-164">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="cc737-164">Activity</span></span> 
<span data-ttu-id="cc737-165">A magas szintű struktúra egy tevékenységhez, a folyamat definition (tevékenységek elem) belül a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="cc737-165">The high-level structure for an activity within a pipeline definition (activities element) is as follows:</span></span>

```json
{
    "name": "ActivityName",
    "description": "description", 
    "type": "<ActivityType>",
    "inputs":  "[]",
    "outputs":  "[]",
    "linkedServiceName": "MyLinkedService",
    "typeProperties":
    {

    },
    "policy":
    {
    }
    "scheduler":
    {
    }
}
```

<span data-ttu-id="cc737-166">A következő táblázat a tulajdonságokat a tevékenység JSON-definícióból ismertetik:</span><span class="sxs-lookup"><span data-stu-id="cc737-166">Following table describe the properties within the activity JSON definition:</span></span>

| <span data-ttu-id="cc737-167">Címke</span><span class="sxs-lookup"><span data-stu-id="cc737-167">Tag</span></span> | <span data-ttu-id="cc737-168">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-168">Description</span></span> | <span data-ttu-id="cc737-169">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-169">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-170">név</span><span class="sxs-lookup"><span data-stu-id="cc737-170">name</span></span> |<span data-ttu-id="cc737-171">A tevékenység nevét.</span><span class="sxs-lookup"><span data-stu-id="cc737-171">Name of the activity.</span></span> <span data-ttu-id="cc737-172">Adjon meg egy nevet a műveletet jelenti, hogy a tevékenység van konfigurálva</span><span class="sxs-lookup"><span data-stu-id="cc737-172">Specify a name that represents the action that the activity is configured to do</span></span><br/><ul><li><span data-ttu-id="cc737-173">Karakterek maximális száma: 260</span><span class="sxs-lookup"><span data-stu-id="cc737-173">Maximum number of characters: 260</span></span></li><li><span data-ttu-id="cc737-174">Számnak betűvel vagy aláhúzással (_) kell kezdődnie</span><span class="sxs-lookup"><span data-stu-id="cc737-174">Must start with a letter number, or an underscore (_)</span></span></li><li><span data-ttu-id="cc737-175">Következő karakterek nem engedélyezettek: ".", "+","?", "/", "<",">", "*", "%", "&", ":","\\"</span><span class="sxs-lookup"><span data-stu-id="cc737-175">Following characters are not allowed: “.”, “+”, “?”, “/”, “<”,”>”,”*”,”%”,”&”,”:”,”\\”</span></span></li></ul> |<span data-ttu-id="cc737-176">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-176">Yes</span></span> |
| <span data-ttu-id="cc737-177">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-177">description</span></span> |<span data-ttu-id="cc737-178">Mire használható a tevékenységet leíró szöveg.</span><span class="sxs-lookup"><span data-stu-id="cc737-178">Text describing what the activity is used for.</span></span> |<span data-ttu-id="cc737-179">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-179">Yes</span></span> |
| <span data-ttu-id="cc737-180">type</span><span class="sxs-lookup"><span data-stu-id="cc737-180">type</span></span> |<span data-ttu-id="cc737-181">Adja meg a tevékenység típusa.</span><span class="sxs-lookup"><span data-stu-id="cc737-181">Specifies the type of the activity.</span></span> <span data-ttu-id="cc737-182">Tekintse meg a [ADATTÁROLÓKHOZ](#data-stores) és [adatok ÁTALAKÍTÁSA tevékenységek](#data-transformation-activities) szakaszok a tevékenységek különböző típusú.</span><span class="sxs-lookup"><span data-stu-id="cc737-182">See the [DATA STORES](#data-stores) and [DATA TRANSFORMATION ACTIVITIES](#data-transformation-activities) sections for different types of activities.</span></span> |<span data-ttu-id="cc737-183">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-183">Yes</span></span> |
| <span data-ttu-id="cc737-184">Bemenetek</span><span class="sxs-lookup"><span data-stu-id="cc737-184">inputs</span></span> |<span data-ttu-id="cc737-185">A tevékenység által felhasznált bemeneti táblák</span><span class="sxs-lookup"><span data-stu-id="cc737-185">Input tables used by the activity</span></span><br/><br/>`// one input table`<br/>`"inputs":  [ { "name": "inputtable1"  } ],`<br/><br/>`// two input tables` <br/>`"inputs":  [ { "name": "inputtable1"  }, { "name": "inputtable2"  } ],` |<span data-ttu-id="cc737-186">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-186">Yes</span></span> |
| <span data-ttu-id="cc737-187">kimenetek</span><span class="sxs-lookup"><span data-stu-id="cc737-187">outputs</span></span> |<span data-ttu-id="cc737-188">A tevékenység által használt kimeneti táblák.</span><span class="sxs-lookup"><span data-stu-id="cc737-188">Output tables used by the activity.</span></span><br/><br/>`// one output table`<br/>`"outputs":  [ { "name": “outputtable1” } ],`<br/><br/>`//two output tables`<br/>`"outputs":  [ { "name": “outputtable1” }, { "name": “outputtable2” }  ],` |<span data-ttu-id="cc737-189">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-189">Yes</span></span> |
| <span data-ttu-id="cc737-190">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="cc737-190">linkedServiceName</span></span> |<span data-ttu-id="cc737-191">A tevékenység által használt társított szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-191">Name of the linked service used by the activity.</span></span> <br/><br/><span data-ttu-id="cc737-192">Egy tevékenység szükség lehet, hogy megadja a szükséges számítási környezet mutató társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="cc737-192">An activity may require that you specify the linked service that links to the required compute environment.</span></span> |<span data-ttu-id="cc737-193">HDInsight tevékenységek, az Azure Machine Learning tevékenységek és a tárolt eljárási tevékenység igen.</span><span class="sxs-lookup"><span data-stu-id="cc737-193">Yes for HDInsight activities, Azure Machine Learning activities, and Stored Procedure Activity.</span></span> <br/><br/><span data-ttu-id="cc737-194">Nem az összes többi</span><span class="sxs-lookup"><span data-stu-id="cc737-194">No for all others</span></span> |
| <span data-ttu-id="cc737-195">typeProperties</span><span class="sxs-lookup"><span data-stu-id="cc737-195">typeProperties</span></span> |<span data-ttu-id="cc737-196">A typeProperties szakaszban tulajdonságok attól függnek, hogy a tevékenység típusa.</span><span class="sxs-lookup"><span data-stu-id="cc737-196">Properties in the typeProperties section depend on type of the activity.</span></span> |<span data-ttu-id="cc737-197">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-197">No</span></span> |
| <span data-ttu-id="cc737-198">Házirend</span><span class="sxs-lookup"><span data-stu-id="cc737-198">policy</span></span> |<span data-ttu-id="cc737-199">A tevékenység a futásidejű működését befolyásoló házirendek.</span><span class="sxs-lookup"><span data-stu-id="cc737-199">Policies that affect the run-time behavior of the activity.</span></span> <span data-ttu-id="cc737-200">Ha nincs megadva, az alapértelmezett házirendek használhatók.</span><span class="sxs-lookup"><span data-stu-id="cc737-200">If it is not specified, default policies are used.</span></span> |<span data-ttu-id="cc737-201">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-201">No</span></span> |
| <span data-ttu-id="cc737-202">A Feladatütemező</span><span class="sxs-lookup"><span data-stu-id="cc737-202">scheduler</span></span> |<span data-ttu-id="cc737-203">"Feladatütemező" tulajdonság a tevékenység kívánt ütemezés meghatározására szolgál.</span><span class="sxs-lookup"><span data-stu-id="cc737-203">“scheduler” property is used to define desired scheduling for the activity.</span></span> <span data-ttu-id="cc737-204">A altulajdonságok ugyanazok, mint az a [availability tulajdonság DataSet adatkészletben](data-factory-create-datasets.md#dataset-availability).</span><span class="sxs-lookup"><span data-stu-id="cc737-204">Its subproperties are the same as the ones in the [availability property in a dataset](data-factory-create-datasets.md#dataset-availability).</span></span> |<span data-ttu-id="cc737-205">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-205">No</span></span> |

### <a name="policies"></a><span data-ttu-id="cc737-206">Házirendek</span><span class="sxs-lookup"><span data-stu-id="cc737-206">Policies</span></span>
<span data-ttu-id="cc737-207">A házirendek milyen hatással a futtatási viselkedés tevékenység, kifejezetten a szelet egy tábla feldolgozásakor.</span><span class="sxs-lookup"><span data-stu-id="cc737-207">Policies affect the run-time behavior of an activity, specifically when the slice of a table is processed.</span></span> <span data-ttu-id="cc737-208">A következő táblázat a részletesen.</span><span class="sxs-lookup"><span data-stu-id="cc737-208">The following table provides the details.</span></span>

| <span data-ttu-id="cc737-209">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-209">Property</span></span> | <span data-ttu-id="cc737-210">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-210">Permitted values</span></span> | <span data-ttu-id="cc737-211">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="cc737-211">Default Value</span></span> | <span data-ttu-id="cc737-212">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-212">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-213">Egyidejűségi</span><span class="sxs-lookup"><span data-stu-id="cc737-213">concurrency</span></span> |<span data-ttu-id="cc737-214">Egész szám</span><span class="sxs-lookup"><span data-stu-id="cc737-214">Integer</span></span> <br/><br/><span data-ttu-id="cc737-215">A maximális érték: 10</span><span class="sxs-lookup"><span data-stu-id="cc737-215">Max value: 10</span></span> |<span data-ttu-id="cc737-216">1</span><span class="sxs-lookup"><span data-stu-id="cc737-216">1</span></span> |<span data-ttu-id="cc737-217">A tevékenység egyidejű végrehajtások száma.</span><span class="sxs-lookup"><span data-stu-id="cc737-217">Number of concurrent executions of the activity.</span></span><br/><br/><span data-ttu-id="cc737-218">Meghatározza, hogy a párhuzamos tevékenység végrehajtások, amely akkor fordulhat elő, a másik szeletek számát.</span><span class="sxs-lookup"><span data-stu-id="cc737-218">It determines the number of parallel activity executions that can happen on different slices.</span></span> <span data-ttu-id="cc737-219">Például ha egy tevékenység kell végighaladnia rendelkezésre álló adatok, nagyobb feldolgozási értéke számos felgyorsítja az adatok feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="cc737-219">For example, if an activity needs to go through a large set of available data, having a larger concurrency value speeds up the data processing.</span></span> |
| <span data-ttu-id="cc737-220">executionPriorityOrder</span><span class="sxs-lookup"><span data-stu-id="cc737-220">executionPriorityOrder</span></span> |<span data-ttu-id="cc737-221">NewestFirst</span><span class="sxs-lookup"><span data-stu-id="cc737-221">NewestFirst</span></span><br/><br/><span data-ttu-id="cc737-222">OldestFirst</span><span class="sxs-lookup"><span data-stu-id="cc737-222">OldestFirst</span></span> |<span data-ttu-id="cc737-223">OldestFirst</span><span class="sxs-lookup"><span data-stu-id="cc737-223">OldestFirst</span></span> |<span data-ttu-id="cc737-224">Meghatározza, hogy feldolgozott adatszeletek sorrendje.</span><span class="sxs-lookup"><span data-stu-id="cc737-224">Determines the ordering of data slices that are being processed.</span></span><br/><br/><span data-ttu-id="cc737-225">Például ha 2 szeletek (du. 4: egy azonban és délután 5 óra egy másik tulajdonságnak), és mindkét végrehajtási függőben van.</span><span class="sxs-lookup"><span data-stu-id="cc737-225">For example, if you have 2 slices (one happening at 4pm, and another one at 5pm), and both are pending execution.</span></span> <span data-ttu-id="cc737-226">Ha a executionPriorityOrder NewestFirst kell, a szelet, délután 5 óra lesz elsőként feldolgozva.</span><span class="sxs-lookup"><span data-stu-id="cc737-226">If you set the executionPriorityOrder to be NewestFirst, the slice at 5 PM is processed first.</span></span> <span data-ttu-id="cc737-227">Hasonlóképpen ha OldestFIrst kell executionPriorityORder, majd a szelet du. 4: dolgoz fel.</span><span class="sxs-lookup"><span data-stu-id="cc737-227">Similarly if you set the executionPriorityORder to be OldestFIrst, then the slice at 4 PM is processed.</span></span> |
| <span data-ttu-id="cc737-228">Próbálja meg újra</span><span class="sxs-lookup"><span data-stu-id="cc737-228">retry</span></span> |<span data-ttu-id="cc737-229">Egész szám</span><span class="sxs-lookup"><span data-stu-id="cc737-229">Integer</span></span><br/><br/><span data-ttu-id="cc737-230">A maximális érték 10 is lehet.</span><span class="sxs-lookup"><span data-stu-id="cc737-230">Max value can be 10</span></span> |<span data-ttu-id="cc737-231">0</span><span class="sxs-lookup"><span data-stu-id="cc737-231">0</span></span> |<span data-ttu-id="cc737-232">Az adatok feldolgozása a szeletre vonatkozó hiba van megjelölve, mielőtt újrapróbálkozások száma.</span><span class="sxs-lookup"><span data-stu-id="cc737-232">Number of retries before the data processing for the slice is marked as Failure.</span></span> <span data-ttu-id="cc737-233">Egy adatszelet tevékenység végrehajtása a rendszer ismét megkísérli legfeljebb a megadott újrapróbálkozások maximális számát.</span><span class="sxs-lookup"><span data-stu-id="cc737-233">Activity execution for a data slice is retried up to the specified retry count.</span></span> <span data-ttu-id="cc737-234">Az újra gombra a lehető leghamarabb a meghibásodás után történik.</span><span class="sxs-lookup"><span data-stu-id="cc737-234">The retry is done as soon as possible after the failure.</span></span> |
| <span data-ttu-id="cc737-235">Időtúllépés</span><span class="sxs-lookup"><span data-stu-id="cc737-235">timeout</span></span> |<span data-ttu-id="cc737-236">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="cc737-236">TimeSpan</span></span> |<span data-ttu-id="cc737-237">00:00:00</span><span class="sxs-lookup"><span data-stu-id="cc737-237">00:00:00</span></span> |<span data-ttu-id="cc737-238">A tevékenység időkorlátja.</span><span class="sxs-lookup"><span data-stu-id="cc737-238">Timeout for the activity.</span></span> <span data-ttu-id="cc737-239">. Példa: 00:10:00 (azt jelenti, időtúllépés 10 perc)</span><span class="sxs-lookup"><span data-stu-id="cc737-239">Example: 00:10:00 (implies timeout 10 mins)</span></span><br/><br/><span data-ttu-id="cc737-240">Ha az érték nincs megadva vagy 0, az időtúllépési érték végtelen.</span><span class="sxs-lookup"><span data-stu-id="cc737-240">If a value is not specified or is 0, the timeout is infinite.</span></span><br/><br/><span data-ttu-id="cc737-241">A szelet adatok feldolgozási ideje meghaladja a időtúllépési értéket, ha azt megszakadt, és a rendszer megkísérli újra feldolgozását.</span><span class="sxs-lookup"><span data-stu-id="cc737-241">If the data processing time on a slice exceeds the timeout value, it is canceled, and the system attempts to retry the processing.</span></span> <span data-ttu-id="cc737-242">Az újrapróbálkozások száma attól függ, hogy az újra gombra tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="cc737-242">The number of retries depends on the retry property.</span></span> <span data-ttu-id="cc737-243">Időtúllépés történik, ha a beállítás időtúllépésbe került.</span><span class="sxs-lookup"><span data-stu-id="cc737-243">When timeout occurs, the status is set to TimedOut.</span></span> |
| <span data-ttu-id="cc737-244">Késleltetés</span><span class="sxs-lookup"><span data-stu-id="cc737-244">delay</span></span> |<span data-ttu-id="cc737-245">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="cc737-245">TimeSpan</span></span> |<span data-ttu-id="cc737-246">00:00:00</span><span class="sxs-lookup"><span data-stu-id="cc737-246">00:00:00</span></span> |<span data-ttu-id="cc737-247">Adja meg a késleltetés, elindul a szelet feldolgozásának előtt.</span><span class="sxs-lookup"><span data-stu-id="cc737-247">Specify the delay before data processing of the slice starts.</span></span><br/><br/><span data-ttu-id="cc737-248">Egy adatszelet tevékenység végrehajtása után a késleltetési idő legyen a várt végrehajtási ideje elmúlt elindult.</span><span class="sxs-lookup"><span data-stu-id="cc737-248">The execution of activity for a data slice is started after the Delay is past the expected execution time.</span></span><br/><br/><span data-ttu-id="cc737-249">. Példa: 00:10:00 (magában foglalja a késleltetést a 10 perc)</span><span class="sxs-lookup"><span data-stu-id="cc737-249">Example: 00:10:00 (implies delay of 10 mins)</span></span> |
| <span data-ttu-id="cc737-250">hosszú újrapróbálkozás</span><span class="sxs-lookup"><span data-stu-id="cc737-250">longRetry</span></span> |<span data-ttu-id="cc737-251">Egész szám</span><span class="sxs-lookup"><span data-stu-id="cc737-251">Integer</span></span><br/><br/><span data-ttu-id="cc737-252">A maximális érték: 10</span><span class="sxs-lookup"><span data-stu-id="cc737-252">Max value: 10</span></span> |<span data-ttu-id="cc737-253">1</span><span class="sxs-lookup"><span data-stu-id="cc737-253">1</span></span> |<span data-ttu-id="cc737-254">Sikertelen volt a szelet végrehajtása előtt hosszú újrapróbálkozási kísérletek száma.</span><span class="sxs-lookup"><span data-stu-id="cc737-254">The number of long retry attempts before the slice execution is failed.</span></span><br/><br/><span data-ttu-id="cc737-255">hosszú újrapróbálkozás kísérletek által longRetryInterval távolságban helyezkednek el.</span><span class="sxs-lookup"><span data-stu-id="cc737-255">longRetry attempts are spaced by longRetryInterval.</span></span> <span data-ttu-id="cc737-256">Ha meg kell adnia egy újrapróbálkozási kísérletek között eltelt idő, így hosszú újrapróbálkozás használja.</span><span class="sxs-lookup"><span data-stu-id="cc737-256">So if you need to specify a time between retry attempts, use longRetry.</span></span> <span data-ttu-id="cc737-257">Ha mind az újra gombra, és a hosszú újrapróbálkozás meg van adva, minden hosszú újrapróbálkozás kísérlet tartalmazza újrapróbálkozások és kísérletek maximális számát. Próbálkozzon újra * hosszú újrapróbálkozás.</span><span class="sxs-lookup"><span data-stu-id="cc737-257">If both Retry and longRetry are specified, each longRetry attempt includes Retry attempts and the max number of attempts is Retry * longRetry.</span></span><br/><br/><span data-ttu-id="cc737-258">Ha például a tevékenység-házirend van a következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="cc737-258">For example, if we have the following settings in the activity policy:</span></span><br/><span data-ttu-id="cc737-259">Próbálkozzon újra: 3</span><span class="sxs-lookup"><span data-stu-id="cc737-259">Retry: 3</span></span><br/><span data-ttu-id="cc737-260">hosszú újrapróbálkozás: 2. régiója</span><span class="sxs-lookup"><span data-stu-id="cc737-260">longRetry: 2</span></span><br/><span data-ttu-id="cc737-261">longRetryInterval: 01:00:00</span><span class="sxs-lookup"><span data-stu-id="cc737-261">longRetryInterval: 01:00:00</span></span><br/><br/><span data-ttu-id="cc737-262">Tegyük fel, nincs végrehajtandó csak egy szelet (állapot vár) és a tevékenység végrehajtása meghiúsul minden alkalommal.</span><span class="sxs-lookup"><span data-stu-id="cc737-262">Assume there is only one slice to execute (status is Waiting) and the activity execution fails every time.</span></span> <span data-ttu-id="cc737-263">Eredetileg nem lenne 3 egymást követő végrehajtási kísérletek.</span><span class="sxs-lookup"><span data-stu-id="cc737-263">Initially there would be 3 consecutive execution attempts.</span></span> <span data-ttu-id="cc737-264">A szelet állapota minden kísérlet után újra lehet.</span><span class="sxs-lookup"><span data-stu-id="cc737-264">After each attempt, the slice status would be Retry.</span></span> <span data-ttu-id="cc737-265">Miután először 3 kísérletet keresztül történik, a szelet állapota hosszú újrapróbálkozás lehet.</span><span class="sxs-lookup"><span data-stu-id="cc737-265">After first 3 attempts are over, the slice status would be LongRetry.</span></span><br/><br/><span data-ttu-id="cc737-266">Egy óra (Ez azt jelenti, hogy longRetryInteval tartozó érték) nem lenne a 3 egymást követő végrehajtási kísérletek egy másik készlet.</span><span class="sxs-lookup"><span data-stu-id="cc737-266">After an hour (that is, longRetryInteval’s value), there would be another set of 3 consecutive execution attempts.</span></span> <span data-ttu-id="cc737-267">Ezt követően a szelet állapota akkor sikertelen, és nincs további újrapróbálkozások volna kísérli meg a.</span><span class="sxs-lookup"><span data-stu-id="cc737-267">After that, the slice status would be Failed and no more retries would be attempted.</span></span> <span data-ttu-id="cc737-268">Ezért a teljes 6 történt kísérlet.</span><span class="sxs-lookup"><span data-stu-id="cc737-268">Hence overall 6 attempts were made.</span></span><br/><br/><span data-ttu-id="cc737-269">Ha bármely végrehajtása sikeres, a szelet állapota Kész és nincs további újrapróbálkozások próbált vannak.</span><span class="sxs-lookup"><span data-stu-id="cc737-269">If any execution succeeds, the slice status would be Ready and no more retries are attempted.</span></span><br/><br/><span data-ttu-id="cc737-270">hosszú újrapróbálkozás függő adatok nem determinisztikus időpontokban érkeznek vagy flaky akkor következik be, mely az adatfeldolgozás alatt a környezetben használható.</span><span class="sxs-lookup"><span data-stu-id="cc737-270">longRetry may be used in situations where dependent data arrives at non-deterministic times or the overall environment is flaky under which data processing occurs.</span></span> <span data-ttu-id="cc737-271">Ilyen esetekben újrapróbálkozások egymás után nem segíthet ezzel, és ezzel egy időszak után időt a kívánt kimeneti eredményez.</span><span class="sxs-lookup"><span data-stu-id="cc737-271">In such cases, doing retries one after another may not help and doing so after an interval of time results in the desired output.</span></span><br/><br/><span data-ttu-id="cc737-272">Járjon el a Word: nincs beállítva hosszú újrapróbálkozás vagy longRetryInterval magas értékeit.</span><span class="sxs-lookup"><span data-stu-id="cc737-272">Word of caution: do not set high values for longRetry or longRetryInterval.</span></span> <span data-ttu-id="cc737-273">Általában a magasabb értékkel rendszeres problémákkal utalnak.</span><span class="sxs-lookup"><span data-stu-id="cc737-273">Typically, higher values imply other systemic issues.</span></span> |
| <span data-ttu-id="cc737-274">longRetryInterval</span><span class="sxs-lookup"><span data-stu-id="cc737-274">longRetryInterval</span></span> |<span data-ttu-id="cc737-275">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="cc737-275">TimeSpan</span></span> |<span data-ttu-id="cc737-276">00:00:00</span><span class="sxs-lookup"><span data-stu-id="cc737-276">00:00:00</span></span> |<span data-ttu-id="cc737-277">Hosszú újrapróbálkozások közötti késleltetés</span><span class="sxs-lookup"><span data-stu-id="cc737-277">The delay between long retry attempts</span></span> |

### <a name="typeproperties-section"></a><span data-ttu-id="cc737-278">typeProperties szakasz</span><span class="sxs-lookup"><span data-stu-id="cc737-278">typeProperties section</span></span>
<span data-ttu-id="cc737-279">A typeProperties szakaszban nem egyezik, minden egyes tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="cc737-279">The typeProperties section is different for each activity.</span></span> <span data-ttu-id="cc737-280">Átalakítás tevékenységek rendelkezik csak a típus tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="cc737-280">Transformation activities have just the type properties.</span></span> <span data-ttu-id="cc737-281">Lásd: [adatok ÁTALAKÍTÁSA tevékenységek](#data-transformation-activities) szakasz ebben a cikkben egy folyamat átalakítása tevékenységek meghatározó JSON-minták.</span><span class="sxs-lookup"><span data-stu-id="cc737-281">See [DATA TRANSFORMATION ACTIVITIES](#data-transformation-activities) section in this article for JSON samples that define transformation activities in a pipeline.</span></span> 

<span data-ttu-id="cc737-282">**Másolási tevékenység** rendelkezik-e a typeProperties szakasz két alszakaszokat: **forrás** és **fogadó**.</span><span class="sxs-lookup"><span data-stu-id="cc737-282">**Copy activity** has two subsections in the typeProperties section: **source** and **sink**.</span></span> <span data-ttu-id="cc737-283">Lásd: [ADATTÁROLÓKHOZ](#data-stores) szakasz ebben a cikkben, amelyek bemutatják, hogyan használható az adatok JSON-minták tárolót, mint a forrás és fogadó, vagy a.</span><span class="sxs-lookup"><span data-stu-id="cc737-283">See [DATA STORES](#data-stores) section in this article for JSON samples that show how to use a data store as a source and/or sink.</span></span> 

### <a name="sample-copy-pipeline"></a><span data-ttu-id="cc737-284">A minta másolási folyamat</span><span class="sxs-lookup"><span data-stu-id="cc737-284">Sample copy pipeline</span></span>
<span data-ttu-id="cc737-285">A következő minta feldolgozási típusú egy tevékenység nincs **másolási** a a **tevékenységek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="cc737-285">In the following sample pipeline, there is one activity of type **Copy** in the **activities** section.</span></span> <span data-ttu-id="cc737-286">Ez a példa a [másolási tevékenység](data-factory-data-movement-activities.md) másol adatokat az Azure Blob storage Azure SQL-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="cc737-286">In this sample, the [Copy activity](data-factory-data-movement-activities.md) copies data from an Azure Blob storage to an Azure SQL database.</span></span> 

```json
{
  "name": "CopyPipeline",
  "properties": {
    "description": "Copy data from a blob to Azure SQL table",
    "activities": [
      {
        "name": "CopyFromBlobToSQL",
        "type": "Copy",
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
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink",
            "writeBatchSize": 10000,
            "writeBatchTimeout": "60:00:00"
          }
        },
        "Policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ],
    "start": "2016-07-12T00:00:00",
    "end": "2016-07-13T00:00:00"
  }
} 
```

<span data-ttu-id="cc737-287">Vegye figyelembe a következő szempontokat:</span><span class="sxs-lookup"><span data-stu-id="cc737-287">Note the following points:</span></span>

* <span data-ttu-id="cc737-288">A tevékenységek szakaszban csak egyetlen tevékenység van, amelynek a **típusa** **Copy** értékre van beállítva.</span><span class="sxs-lookup"><span data-stu-id="cc737-288">In the activities section, there is only one activity whose **type** is set to **Copy**.</span></span>
* <span data-ttu-id="cc737-289">A tevékenység bemenetének beállítása **InputDataset**, a kimeneté pedig **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="cc737-289">Input for the activity is set to **InputDataset** and output for the activity is set to **OutputDataset**.</span></span>
* <span data-ttu-id="cc737-290">A **typeProperties** szakaszban forrástípusként a **BlobSource**, fogadótípusként pedig az **SqlSink** érték van megadva.</span><span class="sxs-lookup"><span data-stu-id="cc737-290">In the **typeProperties** section, **BlobSource** is specified as the source type and **SqlSink** is specified as the sink type.</span></span>

<span data-ttu-id="cc737-291">Lásd: [ADATTÁROLÓKHOZ](#data-stores) szakasz ebben a cikkben, amelyek bemutatják, hogyan használható az adatok JSON-minták tárolót, mint a forrás és fogadó, vagy a.</span><span class="sxs-lookup"><span data-stu-id="cc737-291">See [DATA STORES](#data-stores) section in this article for JSON samples that show how to use a data store as a source and/or sink.</span></span>    

<span data-ttu-id="cc737-292">Ez az adatcsatorna létrehozásának részletes útmutatást lásd: [oktatóanyag: adatok másolása az Blob-tároló az SQL-adatbázis](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="cc737-292">For a complete walkthrough of creating this pipeline, see [Tutorial: Copy data from Blob Storage to SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span> 

### <a name="sample-transformation-pipeline"></a><span data-ttu-id="cc737-293">A minta átalakítási folyamat</span><span class="sxs-lookup"><span data-stu-id="cc737-293">Sample transformation pipeline</span></span>
<span data-ttu-id="cc737-294">A következő minta feldolgozási típusú egy tevékenység nincs **HDInsightHive** a a **tevékenységek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="cc737-294">In the following sample pipeline, there is one activity of type **HDInsightHive** in the **activities** section.</span></span> <span data-ttu-id="cc737-295">Ez a példa a [HDInsight Hive tevékenység](data-factory-hive-activity.md) átalakítja az adatokat a egy Azure Blob storage egy Azure HDInsight Hadoop-fürt Hive parancsfájl futtatásával.</span><span class="sxs-lookup"><span data-stu-id="cc737-295">In this sample, the [HDInsight Hive activity](data-factory-hive-activity.md) transforms data from an Azure Blob storage by running a Hive script file on an Azure HDInsight Hadoop cluster.</span></span> 

```json
{
    "name": "TransformPipeline",
    "properties": {
        "description": "My first Azure Data Factory pipeline",
        "activities": [
            {
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "AzureStorageLinkedService",
                    "defines": {
                        "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                        "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                    }
                },
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
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Month",
                    "interval": 1
                },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }
        ],
        "start": "2016-04-01T00:00:00",
        "end": "2016-04-02T00:00:00",
        "isPaused": false
    }
}
```

<span data-ttu-id="cc737-296">Vegye figyelembe a következő szempontokat:</span><span class="sxs-lookup"><span data-stu-id="cc737-296">Note the following points:</span></span> 

* <span data-ttu-id="cc737-297">A tevékenységek szakasz csak egy tevékenység nincs amelynek **típus** értéke **HDInsightHive**.</span><span class="sxs-lookup"><span data-stu-id="cc737-297">In the activities section, there is only one activity whose **type** is set to **HDInsightHive**.</span></span>
* <span data-ttu-id="cc737-298">A **partitionweblogs.hql** Hive-parancsfájl tárolása az Azure Storage-fiókban (az **AzureStorageLinkedService** nevű scriptLinkedService szolgáltatás által megadva), és az **adfgetstarted** tároló **script** mappájában történik.</span><span class="sxs-lookup"><span data-stu-id="cc737-298">The Hive script file, **partitionweblogs.hql**, is stored in the Azure storage account (specified by the scriptLinkedService, called **AzureStorageLinkedService**), and in **script** folder in the container **adfgetstarted**.</span></span>
* <span data-ttu-id="cc737-299">A **meghatározása** szakasz használatával adja meg a Hive értékként a hive-parancsfájl átadott futásidejű beállítások (például `${hiveconf:inputtable}`, `${hiveconf:partitionedtable}`).</span><span class="sxs-lookup"><span data-stu-id="cc737-299">The **defines** section is used to specify the runtime settings that are passed to the hive script as Hive configuration values (e.g `${hiveconf:inputtable}`, `${hiveconf:partitionedtable}`).</span></span>

<span data-ttu-id="cc737-300">Lásd: [adatok ÁTALAKÍTÁSA tevékenységek](#data-transformation-activities) szakasz ebben a cikkben egy folyamat átalakítása tevékenységek meghatározó JSON-minták.</span><span class="sxs-lookup"><span data-stu-id="cc737-300">See [DATA TRANSFORMATION ACTIVITIES](#data-transformation-activities) section in this article for JSON samples that define transformation activities in a pipeline.</span></span>

<span data-ttu-id="cc737-301">Ez az adatcsatorna létrehozásának részletes útmutatást lásd: [oktatóanyag: felépítheti első folyamatát Hadoop-fürt használatával adatfeldolgozásra történő](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="cc737-301">For a complete walkthrough of creating this pipeline, see [Tutorial: Build your first pipeline to process data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span> 

## <a name="linked-service"></a><span data-ttu-id="cc737-302">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-302">Linked service</span></span>
<span data-ttu-id="cc737-303">A magas szintű struktúra, a társított szolgáltatás definíciójának a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="cc737-303">The high-level structure for a linked service definition is as follows:</span></span>

```json
{
    "name": "<name of the linked service>",
    "properties": {
        "type": "<type of the linked service>",
        "typeProperties": {
        }
    }
}
```

<span data-ttu-id="cc737-304">A következő táblázat a tulajdonságokat a tevékenység JSON-definícióból ismertetik:</span><span class="sxs-lookup"><span data-stu-id="cc737-304">Following table describe the properties within the activity JSON definition:</span></span>

| <span data-ttu-id="cc737-305">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-305">Property</span></span> | <span data-ttu-id="cc737-306">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-306">Description</span></span> | <span data-ttu-id="cc737-307">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-307">Required</span></span> |
| -------- | ----------- | -------- | 
| <span data-ttu-id="cc737-308">név</span><span class="sxs-lookup"><span data-stu-id="cc737-308">name</span></span> | <span data-ttu-id="cc737-309">A társított szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-309">Name of the linked service.</span></span> | <span data-ttu-id="cc737-310">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-310">Yes</span></span> | 
| <span data-ttu-id="cc737-311">Tulajdonságok - típus</span><span class="sxs-lookup"><span data-stu-id="cc737-311">properties - type</span></span> | <span data-ttu-id="cc737-312">A társított szolgáltatás típusa.</span><span class="sxs-lookup"><span data-stu-id="cc737-312">Type of the linked service.</span></span> <span data-ttu-id="cc737-313">Például: az Azure Storage, Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="cc737-313">For example: Azure Storage, Azure SQL Database.</span></span> |
| <span data-ttu-id="cc737-314">typeProperties</span><span class="sxs-lookup"><span data-stu-id="cc737-314">typeProperties</span></span> | <span data-ttu-id="cc737-315">A typeProperties szakasz különböző minden adattároló vagy számítási környezet elemeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="cc737-315">The typeProperties section has elements that are different for each data store or compute environment.</span></span> <span data-ttu-id="cc737-316">Lásd: [adattárolókhoz](#datastores) szakasz az összes adat tárolására társított szolgáltatások és [számítási környezetek](#compute-environments) a számítás kapcsolódó szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-316">See [data stores](#datastores) section for all the data store linked services and [compute environments](#compute-environments) for all the compute linked services</span></span> |   

## <a name="dataset"></a><span data-ttu-id="cc737-317">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-317">Dataset</span></span> 
<span data-ttu-id="cc737-318">Az Azure Data Factory dataset a következők:</span><span class="sxs-lookup"><span data-stu-id="cc737-318">A dataset in Azure Data Factory is defined as follows:</span></span>

```json
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "external": <boolean flag to indicate external data. only for input datasets>,
        "linkedServiceName": "<Name of the linked service that refers to a data store.>",
        "structure": [
            {
                "name": "<Name of the column>",
                "type": "<Name of the type>"
            }
        ],
        "typeProperties": {
            "<type specific property>": "<value>",
            "<type specific property 2>": "<value 2>",
        },
        "availability": {
            "frequency": "<Specifies the time unit for data slice production. Supported frequency: Minute, Hour, Day, Week, Month>",
            "interval": "<Specifies the interval within the defined frequency. For example, frequency set to 'Hour' and interval set to 1 indicates that new data slices should be produced hourly>"
        },
       "policy":
        {      
        }
    }
}
```

<span data-ttu-id="cc737-319">A következő táblázat ismerteti a fenti JSON-tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="cc737-319">The following table describes properties in the above JSON:</span></span>   

| <span data-ttu-id="cc737-320">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-320">Property</span></span> | <span data-ttu-id="cc737-321">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-321">Description</span></span> | <span data-ttu-id="cc737-322">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-322">Required</span></span> | <span data-ttu-id="cc737-323">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="cc737-323">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-324">név</span><span class="sxs-lookup"><span data-stu-id="cc737-324">name</span></span> | <span data-ttu-id="cc737-325">A DataSet adatkészlet neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-325">Name of the dataset.</span></span> <span data-ttu-id="cc737-326">Lásd: [Azure Data Factory - elnevezési szabályait](data-factory-naming-rules.md) elnevezési szabályait.</span><span class="sxs-lookup"><span data-stu-id="cc737-326">See [Azure Data Factory - Naming rules](data-factory-naming-rules.md) for naming rules.</span></span> |<span data-ttu-id="cc737-327">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-327">Yes</span></span> |<span data-ttu-id="cc737-328">NA</span><span class="sxs-lookup"><span data-stu-id="cc737-328">NA</span></span> |
| <span data-ttu-id="cc737-329">type</span><span class="sxs-lookup"><span data-stu-id="cc737-329">type</span></span> | <span data-ttu-id="cc737-330">A dataset típusa.</span><span class="sxs-lookup"><span data-stu-id="cc737-330">Type of the dataset.</span></span> <span data-ttu-id="cc737-331">Meg kell adni az Azure Data Factory által támogatott típusok egyikét (például: AzureBlob, AzureSqlTable).</span><span class="sxs-lookup"><span data-stu-id="cc737-331">Specify one of the types supported by Azure Data Factory (for example: AzureBlob, AzureSqlTable).</span></span> <span data-ttu-id="cc737-332">Lásd: [ADATTÁROLÓKHOZ](#data-stores) szakasz az adatok áruházak és a Data Factory által támogatott adatkészlet-típusok.</span><span class="sxs-lookup"><span data-stu-id="cc737-332">See [DATA STORES](#data-stores) section for all the data stores and dataset types supported by Data Factory.</span></span> | 
| <span data-ttu-id="cc737-333">struktúra</span><span class="sxs-lookup"><span data-stu-id="cc737-333">structure</span></span> | <span data-ttu-id="cc737-334">Az adatkészlet sémája.</span><span class="sxs-lookup"><span data-stu-id="cc737-334">Schema of the dataset.</span></span> <span data-ttu-id="cc737-335">Tartalmaz oszlopok, azok típusok, stb.</span><span class="sxs-lookup"><span data-stu-id="cc737-335">It contains columns, their types, etc.</span></span> | <span data-ttu-id="cc737-336">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-336">No</span></span> |<span data-ttu-id="cc737-337">NA</span><span class="sxs-lookup"><span data-stu-id="cc737-337">NA</span></span> |
| <span data-ttu-id="cc737-338">typeProperties</span><span class="sxs-lookup"><span data-stu-id="cc737-338">typeProperties</span></span> | <span data-ttu-id="cc737-339">A kiválasztott típus megfelelő tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="cc737-339">Properties corresponding to the selected type.</span></span> <span data-ttu-id="cc737-340">Lásd: [ADATTÁROLÓKHOZ](#data-stores) szakaszban a támogatott típusok és azok tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="cc737-340">See [DATA STORES](#data-stores) section for supported types and their properties.</span></span> |<span data-ttu-id="cc737-341">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-341">Yes</span></span> |<span data-ttu-id="cc737-342">NA</span><span class="sxs-lookup"><span data-stu-id="cc737-342">NA</span></span> |
| <span data-ttu-id="cc737-343">external</span><span class="sxs-lookup"><span data-stu-id="cc737-343">external</span></span> | <span data-ttu-id="cc737-344">Logikai jelző, amely adja meg, hogy a data factory-folyamathoz explicit módon létrehozott adatkészlet vagy nem.</span><span class="sxs-lookup"><span data-stu-id="cc737-344">Boolean flag to specify whether a dataset is explicitly produced by a data factory pipeline or not.</span></span> |<span data-ttu-id="cc737-345">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-345">No</span></span> |<span data-ttu-id="cc737-346">hamis</span><span class="sxs-lookup"><span data-stu-id="cc737-346">false</span></span> |
| <span data-ttu-id="cc737-347">rendelkezésre állás</span><span class="sxs-lookup"><span data-stu-id="cc737-347">availability</span></span> | <span data-ttu-id="cc737-348">A feldolgozási időszakában vagy az adatkészlet üzemi slicing modell határoz meg.</span><span class="sxs-lookup"><span data-stu-id="cc737-348">Defines the processing window or the slicing model for the dataset production.</span></span> <span data-ttu-id="cc737-349">Az adatkészlet modell felosztás a részletekért lásd: [ütemezés és a végrehajtás](data-factory-scheduling-and-execution.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-349">For details on the dataset slicing model, see [Scheduling and Execution](data-factory-scheduling-and-execution.md) article.</span></span> |<span data-ttu-id="cc737-350">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-350">Yes</span></span> |<span data-ttu-id="cc737-351">NA</span><span class="sxs-lookup"><span data-stu-id="cc737-351">NA</span></span> |
| <span data-ttu-id="cc737-352">Házirend</span><span class="sxs-lookup"><span data-stu-id="cc737-352">policy</span></span> |<span data-ttu-id="cc737-353">Meghatározza a feltételek vagy a feltétellel, hogy a dataset szeletek teljesítenie kell.</span><span class="sxs-lookup"><span data-stu-id="cc737-353">Defines the criteria or the condition that the dataset slices must fulfill.</span></span> <br/><br/><span data-ttu-id="cc737-354">További információkért lásd: [Dataset házirend](#Policy) szakasz.</span><span class="sxs-lookup"><span data-stu-id="cc737-354">For details, see [Dataset Policy](#Policy) section.</span></span> |<span data-ttu-id="cc737-355">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-355">No</span></span> |<span data-ttu-id="cc737-356">NA</span><span class="sxs-lookup"><span data-stu-id="cc737-356">NA</span></span> |

<span data-ttu-id="cc737-357">Minden egyes oszlopának a **struktúra** a szakasz a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="cc737-357">Each column in the **structure** section contains the following properties:</span></span>

| <span data-ttu-id="cc737-358">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-358">Property</span></span> | <span data-ttu-id="cc737-359">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-359">Description</span></span> | <span data-ttu-id="cc737-360">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-360">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-361">név</span><span class="sxs-lookup"><span data-stu-id="cc737-361">name</span></span> |<span data-ttu-id="cc737-362">Az oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-362">Name of the column.</span></span> |<span data-ttu-id="cc737-363">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-363">Yes</span></span> |
| <span data-ttu-id="cc737-364">type</span><span class="sxs-lookup"><span data-stu-id="cc737-364">type</span></span> |<span data-ttu-id="cc737-365">Az oszlop adattípusát.</span><span class="sxs-lookup"><span data-stu-id="cc737-365">Data type of the column.</span></span>  |<span data-ttu-id="cc737-366">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-366">No</span></span> |
| <span data-ttu-id="cc737-367">Kulturális környezet</span><span class="sxs-lookup"><span data-stu-id="cc737-367">culture</span></span> |<span data-ttu-id="cc737-368">.NET-alapú kulturális környezet lehet használni, ha a típus meg van adva, de a .NET-típus `Datetime` vagy `Datetimeoffset`.</span><span class="sxs-lookup"><span data-stu-id="cc737-368">.NET based culture to be used when type is specified and is .NET type `Datetime` or `Datetimeoffset`.</span></span> <span data-ttu-id="cc737-369">Alapértelmezett érték a `en-us`.</span><span class="sxs-lookup"><span data-stu-id="cc737-369">Default is `en-us`.</span></span> |<span data-ttu-id="cc737-370">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-370">No</span></span> |
| <span data-ttu-id="cc737-371">Formátumban</span><span class="sxs-lookup"><span data-stu-id="cc737-371">format</span></span> |<span data-ttu-id="cc737-372">Formázó karakterlánc, használandó típus van megadva, és a .NET-típus `Datetime` vagy `Datetimeoffset`.</span><span class="sxs-lookup"><span data-stu-id="cc737-372">Format string to be used when type is specified and is .NET type `Datetime` or `Datetimeoffset`.</span></span> |<span data-ttu-id="cc737-373">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-373">No</span></span> |

<span data-ttu-id="cc737-374">A következő példában a dataset adatkészletben három oszlopot `slicetimestamp`, `projectname`, és `pageviews` és típus: karakterlánc, karakterlánc és tizedes kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="cc737-374">In the following example, the dataset has three columns `slicetimestamp`, `projectname`, and `pageviews` and they are of type: String, String, and Decimal respectively.</span></span>

```json
structure:  
[
    { "name": "slicetimestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

<span data-ttu-id="cc737-375">A következő táblázat ismerteti a használható tulajdonságok a **rendelkezésre állási** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-375">The following table describes properties you can use in the **availability** section:</span></span>

| <span data-ttu-id="cc737-376">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-376">Property</span></span> | <span data-ttu-id="cc737-377">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-377">Description</span></span> | <span data-ttu-id="cc737-378">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-378">Required</span></span> | <span data-ttu-id="cc737-379">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="cc737-379">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-380">gyakoriság</span><span class="sxs-lookup"><span data-stu-id="cc737-380">frequency</span></span> |<span data-ttu-id="cc737-381">Megadja a dataset szelet üzemi időegységét.</span><span class="sxs-lookup"><span data-stu-id="cc737-381">Specifies the time unit for dataset slice production.</span></span><br/><br/><span data-ttu-id="cc737-382"><b>Támogatott gyakoriság</b>: perc, óra, nap, hét, hónap</span><span class="sxs-lookup"><span data-stu-id="cc737-382"><b>Supported frequency</b>: Minute, Hour, Day, Week, Month</span></span> |<span data-ttu-id="cc737-383">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-383">Yes</span></span> |<span data-ttu-id="cc737-384">NA</span><span class="sxs-lookup"><span data-stu-id="cc737-384">NA</span></span> |
| <span data-ttu-id="cc737-385">időköz</span><span class="sxs-lookup"><span data-stu-id="cc737-385">interval</span></span> |<span data-ttu-id="cc737-386">Megadja egy szorzóval gyakoriság</span><span class="sxs-lookup"><span data-stu-id="cc737-386">Specifies a multiplier for frequency</span></span><br/><br/><span data-ttu-id="cc737-387">"X időköz" határozza meg, milyen gyakran a szelet jön létre.</span><span class="sxs-lookup"><span data-stu-id="cc737-387">”Frequency x interval” determines how often the slice is produced.</span></span><br/><br/><span data-ttu-id="cc737-388">Ha módosítania kell a adatkészlet kell szeletelhetők óránként, beállíthatja <b>gyakorisága</b> való <b>óra</b>, és <b>időköz</b> való <b>1</b>.</span><span class="sxs-lookup"><span data-stu-id="cc737-388">If you need the dataset to be sliced on an hourly basis, you set <b>Frequency</b> to <b>Hour</b>, and <b>interval</b> to <b>1</b>.</span></span><br/><br/><span data-ttu-id="cc737-389"><b>Megjegyzés:</b>: perces gyakoriságot ad meg, ha azt javasoljuk, hogy beállította az intervallum nem lehet kisebb, mint 15</span><span class="sxs-lookup"><span data-stu-id="cc737-389"><b>Note</b>: If you specify Frequency as Minute, we recommend that you set the interval to no less than 15</span></span> |<span data-ttu-id="cc737-390">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-390">Yes</span></span> |<span data-ttu-id="cc737-391">NA</span><span class="sxs-lookup"><span data-stu-id="cc737-391">NA</span></span> |
| <span data-ttu-id="cc737-392">stílus</span><span class="sxs-lookup"><span data-stu-id="cc737-392">style</span></span> |<span data-ttu-id="cc737-393">Meghatározza, hogy a szelet akkor a rendszer az időköz kezdő/végén.</span><span class="sxs-lookup"><span data-stu-id="cc737-393">Specifies whether the slice should be produced at the start/end of the interval.</span></span><ul><li><span data-ttu-id="cc737-394">StartOfInterval</span><span class="sxs-lookup"><span data-stu-id="cc737-394">StartOfInterval</span></span></li><li><span data-ttu-id="cc737-395">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="cc737-395">EndOfInterval</span></span></li></ul><br/><br/><span data-ttu-id="cc737-396">Ha gyakoriságának beállítása hónapot és stílus EndOfInterval beállítása, a szelet hónap utolsó napján elő.</span><span class="sxs-lookup"><span data-stu-id="cc737-396">If Frequency is set to Month and style is set to EndOfInterval, the slice is produced on the last day of month.</span></span> <span data-ttu-id="cc737-397">Ha a stílus StartOfInterval van megadva, a szelet hónap első napján elő.</span><span class="sxs-lookup"><span data-stu-id="cc737-397">If the style is set to StartOfInterval, the slice is produced on the first day of month.</span></span><br/><br/><span data-ttu-id="cc737-398">Ha nap gyakoriságának beállítása és stílus EndOfInterval beállítása, a szelet elő az elmúlt órában a nap.</span><span class="sxs-lookup"><span data-stu-id="cc737-398">If Frequency is set to Day and style is set to EndOfInterval, the slice is produced in the last hour of the day.</span></span><br/><br/><span data-ttu-id="cc737-399">Ha gyakoriságának beállítása óra és stílus EndOfInterval beállítása, a szelet elő az órát végén.</span><span class="sxs-lookup"><span data-stu-id="cc737-399">If Frequency is set to Hour and style is set to EndOfInterval, the slice is produced at the end of the hour.</span></span> <span data-ttu-id="cc737-400">Például egy adatszelethez du. 1 – 2 óra időszakban, a szelet hozzák 2 du..</span><span class="sxs-lookup"><span data-stu-id="cc737-400">For example, for a slice for 1 PM – 2 PM period, the slice is produced at 2 PM.</span></span> |<span data-ttu-id="cc737-401">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-401">No</span></span> |<span data-ttu-id="cc737-402">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="cc737-402">EndOfInterval</span></span> |
| <span data-ttu-id="cc737-403">anchorDateTime</span><span class="sxs-lookup"><span data-stu-id="cc737-403">anchorDateTime</span></span> |<span data-ttu-id="cc737-404">Határozza meg az idő az ütemező által használt adatkészlet szelet határok számítási abszolút pozíciója.</span><span class="sxs-lookup"><span data-stu-id="cc737-404">Defines the absolute position in time used by scheduler to compute dataset slice boundaries.</span></span> <br/><br/><span data-ttu-id="cc737-405"><b>Megjegyzés:</b>: Ha a AnchorDateTime részekből dátum, amelyek részletesebben, mint a gyakorisága, akkor a részletesebb részek figyelmen kívül lesznek hagyva.</span><span class="sxs-lookup"><span data-stu-id="cc737-405"><b>Note</b>: If the AnchorDateTime has date parts that are more granular than the frequency then the more granular parts are ignored.</span></span> <br/><br/><span data-ttu-id="cc737-406">Például ha a <b>időköz</b> van <b>óránkénti</b> (gyakoriság: óra és időköz: 1) és a <b>AnchorDateTime</b> tartalmaz <b>percet és másodpercet</b>a <b>percet és másodpercet</b> a AnchorDateTime részei a rendszer figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="cc737-406">For example, if the <b>interval</b> is <b>hourly</b> (frequency: hour and interval: 1) and the <b>AnchorDateTime</b> contains <b>minutes and seconds</b> then the <b>minutes and seconds</b> parts of the AnchorDateTime are ignored.</span></span> |<span data-ttu-id="cc737-407">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-407">No</span></span> |<span data-ttu-id="cc737-408">01/01/0001</span><span class="sxs-lookup"><span data-stu-id="cc737-408">01/01/0001</span></span> |
| <span data-ttu-id="cc737-409">Az offset</span><span class="sxs-lookup"><span data-stu-id="cc737-409">offset</span></span> |<span data-ttu-id="cc737-410">TimeSpan érték, amely a kezdő és a záró összes adatkészlet szeletek vette.</span><span class="sxs-lookup"><span data-stu-id="cc737-410">Timespan by which the start and end of all dataset slices are shifted.</span></span> <br/><br/><span data-ttu-id="cc737-411"><b>Megjegyzés:</b>: Ha anchorDateTime és az offset is meg van adva, az eredmény a kombinált shift-e.</span><span class="sxs-lookup"><span data-stu-id="cc737-411"><b>Note</b>: If both anchorDateTime and offset are specified, the result is the combined shift.</span></span> |<span data-ttu-id="cc737-412">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-412">No</span></span> |<span data-ttu-id="cc737-413">NA</span><span class="sxs-lookup"><span data-stu-id="cc737-413">NA</span></span> |

<span data-ttu-id="cc737-414">A következő rendelkezésre állási szakasz Megadja, hogy a kimeneti adatkészlet előállított óránként (vagy) bemeneti adatkészlet óránkénti áll rendelkezésre:</span><span class="sxs-lookup"><span data-stu-id="cc737-414">The following availability section specifies that the output dataset is either produced hourly (or) input dataset is available hourly:</span></span>

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 1    
}
```

<span data-ttu-id="cc737-415">A **házirend** az adatkészlet-definícióban szakaszban határozza meg, a feltételek vagy a feltétellel, hogy a dataset szeletek teljesítenie kell.</span><span class="sxs-lookup"><span data-stu-id="cc737-415">The **policy** section in dataset definition defines the criteria or the condition that the dataset slices must fulfill.</span></span>

| <span data-ttu-id="cc737-416">Házirend neve</span><span class="sxs-lookup"><span data-stu-id="cc737-416">Policy Name</span></span> | <span data-ttu-id="cc737-417">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-417">Description</span></span> | <span data-ttu-id="cc737-418">Vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="cc737-418">Applied To</span></span> | <span data-ttu-id="cc737-419">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-419">Required</span></span> | <span data-ttu-id="cc737-420">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="cc737-420">Default</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="cc737-421">minimumSizeMB</span><span class="sxs-lookup"><span data-stu-id="cc737-421">minimumSizeMB</span></span> |<span data-ttu-id="cc737-422">Azt ellenőrzi, hogy az adatokat egy **Azure blob** megfelel a minimális méretét (megabájtban).</span><span class="sxs-lookup"><span data-stu-id="cc737-422">Validates that the data in an **Azure blob** meets the minimum size requirements (in megabytes).</span></span> |<span data-ttu-id="cc737-423">Azure-blob</span><span class="sxs-lookup"><span data-stu-id="cc737-423">Azure Blob</span></span> |<span data-ttu-id="cc737-424">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-424">No</span></span> |<span data-ttu-id="cc737-425">NA</span><span class="sxs-lookup"><span data-stu-id="cc737-425">NA</span></span> |
| <span data-ttu-id="cc737-426">minimumRows</span><span class="sxs-lookup"><span data-stu-id="cc737-426">minimumRows</span></span> |<span data-ttu-id="cc737-427">Azt ellenőrzi, hogy az adatokat egy **Azure SQL adatbázis** vagy egy **Azure-tábla** a sorok legkisebb számát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="cc737-427">Validates that the data in an **Azure SQL database** or an **Azure table** contains the minimum number of rows.</span></span> |<ul><li><span data-ttu-id="cc737-428">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="cc737-428">Azure SQL Database</span></span></li><li><span data-ttu-id="cc737-429">Azure-tábla</span><span class="sxs-lookup"><span data-stu-id="cc737-429">Azure Table</span></span></li></ul> |<span data-ttu-id="cc737-430">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-430">No</span></span> |<span data-ttu-id="cc737-431">NA</span><span class="sxs-lookup"><span data-stu-id="cc737-431">NA</span></span> |

<span data-ttu-id="cc737-432">**Példa**</span><span class="sxs-lookup"><span data-stu-id="cc737-432">**Example:**</span></span>

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

<span data-ttu-id="cc737-433">Azure Data Factory hozzák alatt álló adatkészlet, kivéve azt állapotúként kell megjelölni **külső**.</span><span class="sxs-lookup"><span data-stu-id="cc737-433">Unless a dataset is being produced by Azure Data Factory, it should be marked as **external**.</span></span> <span data-ttu-id="cc737-434">Ez a beállítás általában vonatkozik a bemenet az adatcsatorna első tevékenység, kivéve, ha a tevékenység vagy csővezeték-láncolás használatban van.</span><span class="sxs-lookup"><span data-stu-id="cc737-434">This setting generally applies to the inputs of first activity in a pipeline unless activity or pipeline chaining is being used.</span></span>

| <span data-ttu-id="cc737-435">Név</span><span class="sxs-lookup"><span data-stu-id="cc737-435">Name</span></span> | <span data-ttu-id="cc737-436">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-436">Description</span></span> | <span data-ttu-id="cc737-437">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-437">Required</span></span> | <span data-ttu-id="cc737-438">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="cc737-438">Default Value</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-439">dataDelay</span><span class="sxs-lookup"><span data-stu-id="cc737-439">dataDelay</span></span> |<span data-ttu-id="cc737-440">A külső adatokat az adott szelet rendelkezésre állásának az ellenőrzését késleltetési idő.</span><span class="sxs-lookup"><span data-stu-id="cc737-440">Time to delay the check on the availability of the external data for the given slice.</span></span> <span data-ttu-id="cc737-441">Például ha óránkénti érhető el az adatokat, ellenőrizze, hogy a külső adatok elérhetők legyenek és a megfelelő szelet készen áll az dataDelay használatával késleltethető.</span><span class="sxs-lookup"><span data-stu-id="cc737-441">For example, if the data is available hourly, the check to see the external data is available and the corresponding slice is Ready can be delayed by using dataDelay.</span></span><br/><br/><span data-ttu-id="cc737-442">Csak a jelenlegi időpont vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="cc737-442">Only applies to the present time.</span></span>  <span data-ttu-id="cc737-443">Például ha 1:00 PM azonnal, és az értéke 10 perc, az érvényesítési kezdődik, 1:10 óra.</span><span class="sxs-lookup"><span data-stu-id="cc737-443">For example, if it is 1:00 PM right now and this value is 10 minutes, the validation starts at 1:10 PM.</span></span><br/><br/><span data-ttu-id="cc737-444">Ez a beállítás nincs hatással a szeletek a múltban (szelet befejezési időpontja + dataDelay szeletek < most) dolgoznak fel késedelem nélkül.</span><span class="sxs-lookup"><span data-stu-id="cc737-444">This setting does not affect slices in the past (slices with Slice End Time + dataDelay < Now) are processed without any delay.</span></span><br/><br/><span data-ttu-id="cc737-445">Idő nagyobb, mint 23:59 óra kell megadni, használja a `day.hours:minutes:seconds` formátumban.</span><span class="sxs-lookup"><span data-stu-id="cc737-445">Time greater than 23:59 hours need to specified using the `day.hours:minutes:seconds` format.</span></span> <span data-ttu-id="cc737-446">Például adja meg a 24 órát, ne használja 24:00:00. Ehelyett használjon 1.00:00:00.</span><span class="sxs-lookup"><span data-stu-id="cc737-446">For example, to specify 24 hours, don't use 24:00:00; instead, use 1.00:00:00.</span></span> <span data-ttu-id="cc737-447">Ha 24:00:00 használja, akkor a rendszer 24 napos (24.00:00:00).</span><span class="sxs-lookup"><span data-stu-id="cc737-447">If you use 24:00:00, it is treated as 24 days (24.00:00:00).</span></span> <span data-ttu-id="cc737-448">1 nap és 4 óra adja meg 1:04:00:00.</span><span class="sxs-lookup"><span data-stu-id="cc737-448">For 1 day and 4 hours, specify 1:04:00:00.</span></span> |<span data-ttu-id="cc737-449">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-449">No</span></span> |<span data-ttu-id="cc737-450">0</span><span class="sxs-lookup"><span data-stu-id="cc737-450">0</span></span> |
| <span data-ttu-id="cc737-451">RetryInterval</span><span class="sxs-lookup"><span data-stu-id="cc737-451">retryInterval</span></span> |<span data-ttu-id="cc737-452">A várakozási idő közötti hiba, a következő kísérlet próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="cc737-452">The wait time between a failure and the next retry attempt.</span></span> <span data-ttu-id="cc737-453">Ha egy try sikertelen, a következő kísérlet retryInterval utánra esik.</span><span class="sxs-lookup"><span data-stu-id="cc737-453">If a try fails, the next try is after retryInterval.</span></span> <br/><br/><span data-ttu-id="cc737-454">Ha 1:00 PM most, az első lépések az első próbálkozás.</span><span class="sxs-lookup"><span data-stu-id="cc737-454">If it is 1:00 PM right now, we begin the first try.</span></span> <span data-ttu-id="cc737-455">Ha az első ellenőrzési ellenőrzés időtartam 1 perc és a művelet sikertelen volt, a következő újrapróbálkozási jelenleg 1:00 + 1 perc (időtartam) + 1 perces (újrapróbálkozási időköz) = 1:02 PM.</span><span class="sxs-lookup"><span data-stu-id="cc737-455">If the duration to complete the first validation check is 1 minute and the operation failed, the next retry is at 1:00 + 1 min (duration) + 1 min (retry interval) = 1:02 PM.</span></span> <br/><br/><span data-ttu-id="cc737-456">A múltban szeletek nincs késleltetés.</span><span class="sxs-lookup"><span data-stu-id="cc737-456">For slices in the past, there is no delay.</span></span> <span data-ttu-id="cc737-457">Az újrapróbálkozási azonnal történik.</span><span class="sxs-lookup"><span data-stu-id="cc737-457">The retry happens immediately.</span></span> |<span data-ttu-id="cc737-458">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-458">No</span></span> |<span data-ttu-id="cc737-459">00:01:00 (1 perc)</span><span class="sxs-lookup"><span data-stu-id="cc737-459">00:01:00 (1 minute)</span></span> |
| <span data-ttu-id="cc737-460">retryTimeout</span><span class="sxs-lookup"><span data-stu-id="cc737-460">retryTimeout</span></span> |<span data-ttu-id="cc737-461">Az egyes újrapróbálkozások időkorlátját.</span><span class="sxs-lookup"><span data-stu-id="cc737-461">The timeout for each retry attempt.</span></span><br/><br/><span data-ttu-id="cc737-462">Ha ez a tulajdonság 10 percre van beállítva, az érvényesítési kell elvégezni, 10 percen belül.</span><span class="sxs-lookup"><span data-stu-id="cc737-462">If this property is set to 10 minutes, the validation needs to be completed within 10 minutes.</span></span> <span data-ttu-id="cc737-463">Ha az érvényesítés végrehajtásához 10 percnél hosszabb ideig tart, az ismételt próbálkozás túllépi az időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="cc737-463">If it takes longer than 10 minutes to perform the validation, the retry times out.</span></span><br/><br/><span data-ttu-id="cc737-464">Ha az érvényesítéshez bármilyen kísérlet időkorlátja lejár, a szelet időtúllépésbe került van megjelölve.</span><span class="sxs-lookup"><span data-stu-id="cc737-464">If all attempts for the validation times out, the slice is marked as TimedOut.</span></span> |<span data-ttu-id="cc737-465">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-465">No</span></span> |<span data-ttu-id="cc737-466">00:10:00 (10 perc)</span><span class="sxs-lookup"><span data-stu-id="cc737-466">00:10:00 (10 minutes)</span></span> |
| <span data-ttu-id="cc737-467">maximumRetry</span><span class="sxs-lookup"><span data-stu-id="cc737-467">maximumRetry</span></span> |<span data-ttu-id="cc737-468">Ellenőrizze, hogy elérhető-e a külső adatok futtatásainak száma.</span><span class="sxs-lookup"><span data-stu-id="cc737-468">Number of times to check for the availability of the external data.</span></span> <span data-ttu-id="cc737-469">A megengedett maximális értéke 10.</span><span class="sxs-lookup"><span data-stu-id="cc737-469">The allowed maximum value is 10.</span></span> |<span data-ttu-id="cc737-470">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-470">No</span></span> |<span data-ttu-id="cc737-471">3</span><span class="sxs-lookup"><span data-stu-id="cc737-471">3</span></span> |


## <a name="data-stores"></a><span data-ttu-id="cc737-472">ADATTÁROLÓ</span><span class="sxs-lookup"><span data-stu-id="cc737-472">DATA STORES</span></span>
<span data-ttu-id="cc737-473">A [társított szolgáltatás](#linked-service) szakaszban megadott JSON-elemek szerepelnek, amelyek közösek az összes eszköztípuson összekapcsolt szolgáltatások leírása.</span><span class="sxs-lookup"><span data-stu-id="cc737-473">The [Linked service](#linked-service) section provided descriptions for JSON elements that are common to all types of linked services.</span></span> <span data-ttu-id="cc737-474">Ez a szakasz ismerteti a JSON-elemek szerepelnek, mindegyik adattár jellemző.</span><span class="sxs-lookup"><span data-stu-id="cc737-474">This section provides details about JSON elements that are specific to each data store.</span></span>

<span data-ttu-id="cc737-475">A [Dataset](#dataset) JSON-elemek szerepelnek, amelyek közösek az összes eszköztípuson adatkészletek a megadott szakasz leírását.</span><span class="sxs-lookup"><span data-stu-id="cc737-475">The [Dataset](#dataset) section provided descriptions for JSON elements that are common to all types of datasets.</span></span> <span data-ttu-id="cc737-476">Ez a szakasz ismerteti a JSON-elemek szerepelnek, mindegyik adattár jellemző.</span><span class="sxs-lookup"><span data-stu-id="cc737-476">This section provides details about JSON elements that are specific to each data store.</span></span>

<span data-ttu-id="cc737-477">A [tevékenység](#activity) JSON-elemek szerepelnek, amelyek közösek az összes eszköztípuson tevékenységek leírásainak szakaszát.</span><span class="sxs-lookup"><span data-stu-id="cc737-477">The [Activity](#activity) section provided descriptions for JSON elements that are common to all types of activities.</span></span> <span data-ttu-id="cc737-478">Ez a szakasz ismerteti a JSON-elemek szerepelnek, amelyek minden adattár kötődnek, mint a másolási tevékenység során a forrás/fogadó használata esetén.</span><span class="sxs-lookup"><span data-stu-id="cc737-478">This section provides details about JSON elements that are specific to each data store when it is used as a source/sink in a copy activity.</span></span>  

<span data-ttu-id="cc737-479">A tároló érdekli a JSON-sémák társított szolgáltatás, a DataSet adatkészlet és a másolási tevékenység során a forrás/fogadó megjelenítéséhez a hivatkozásra kattintva.</span><span class="sxs-lookup"><span data-stu-id="cc737-479">Click the link for the store you are interested in to see the JSON schemas for linked service, dataset, and the source/sink for the copy activity.</span></span>

| <span data-ttu-id="cc737-480">Kategória</span><span class="sxs-lookup"><span data-stu-id="cc737-480">Category</span></span> | <span data-ttu-id="cc737-481">Adattár</span><span class="sxs-lookup"><span data-stu-id="cc737-481">Data store</span></span> 
|:--- |:--- |
| <span data-ttu-id="cc737-482">**Azure**</span><span class="sxs-lookup"><span data-stu-id="cc737-482">**Azure**</span></span> |[<span data-ttu-id="cc737-483">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="cc737-483">Azure Blob storage</span></span>](#azure-blob-storage) |
| &nbsp; |[<span data-ttu-id="cc737-484">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="cc737-484">Azure Data Lake Store</span></span>](#azure-datalake-store) |
| &nbsp; |[<span data-ttu-id="cc737-485">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="cc737-485">Azure Cosmos DB</span></span>](#azure-cosmos-db) |
| &nbsp; |[<span data-ttu-id="cc737-486">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="cc737-486">Azure SQL Database</span></span>](#azure-sql-database) |
| &nbsp; |[<span data-ttu-id="cc737-487">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="cc737-487">Azure SQL Data Warehouse</span></span>](#azure-sql-data-warehouse) |
| &nbsp; |[<span data-ttu-id="cc737-488">Azure Search</span><span class="sxs-lookup"><span data-stu-id="cc737-488">Azure Search</span></span>](#azure-search) |
| &nbsp; |[<span data-ttu-id="cc737-489">Azure Table storage</span><span class="sxs-lookup"><span data-stu-id="cc737-489">Azure Table storage</span></span>](#azure-table-storage) |
| <span data-ttu-id="cc737-490">**Adatbázisok**</span><span class="sxs-lookup"><span data-stu-id="cc737-490">**Databases**</span></span> |[<span data-ttu-id="cc737-491">Amazon Redshift</span><span class="sxs-lookup"><span data-stu-id="cc737-491">Amazon Redshift</span></span>](#amazon-redshift) |
| &nbsp; |[<span data-ttu-id="cc737-492">IBM DB2</span><span class="sxs-lookup"><span data-stu-id="cc737-492">IBM DB2</span></span>](#ibm-db2) |
| &nbsp; |[<span data-ttu-id="cc737-493">MySQL</span><span class="sxs-lookup"><span data-stu-id="cc737-493">MySQL</span></span>](#mysql) |
| &nbsp; |[<span data-ttu-id="cc737-494">Oracle</span><span class="sxs-lookup"><span data-stu-id="cc737-494">Oracle</span></span>](#oracle) |
| &nbsp; |[<span data-ttu-id="cc737-495">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="cc737-495">PostgreSQL</span></span>](#postgresql) |
| &nbsp; |[<span data-ttu-id="cc737-496">SAP Business Warehouse</span><span class="sxs-lookup"><span data-stu-id="cc737-496">SAP Business Warehouse</span></span>](#sap-business-warehouse) |
| &nbsp; |[<span data-ttu-id="cc737-497">SAP HANA</span><span class="sxs-lookup"><span data-stu-id="cc737-497">SAP HANA</span></span>](#sap-hana) |
| &nbsp; |[<span data-ttu-id="cc737-498">SQL Server</span><span class="sxs-lookup"><span data-stu-id="cc737-498">SQL Server</span></span>](#sql-server) |
| &nbsp; |[<span data-ttu-id="cc737-499">Sybase</span><span class="sxs-lookup"><span data-stu-id="cc737-499">Sybase</span></span>](#sybase) |
| &nbsp; |[<span data-ttu-id="cc737-500">Teradata</span><span class="sxs-lookup"><span data-stu-id="cc737-500">Teradata</span></span>](#teradata) |
| <span data-ttu-id="cc737-501">**NoSQL**</span><span class="sxs-lookup"><span data-stu-id="cc737-501">**NoSQL**</span></span> |[<span data-ttu-id="cc737-502">Cassandra</span><span class="sxs-lookup"><span data-stu-id="cc737-502">Cassandra</span></span>](#cassandra) |
| &nbsp; |[<span data-ttu-id="cc737-503">MongoDB</span><span class="sxs-lookup"><span data-stu-id="cc737-503">MongoDB</span></span>](#mongodb) |
| <span data-ttu-id="cc737-504">**Fájl**</span><span class="sxs-lookup"><span data-stu-id="cc737-504">**File**</span></span> |[<span data-ttu-id="cc737-505">Amazon S3</span><span class="sxs-lookup"><span data-stu-id="cc737-505">Amazon S3</span></span>](#amazon-s3) |
| &nbsp; |[<span data-ttu-id="cc737-506">Fájlrendszer</span><span class="sxs-lookup"><span data-stu-id="cc737-506">File System</span></span>](#file-system) |
| &nbsp; |[<span data-ttu-id="cc737-507">FTP</span><span class="sxs-lookup"><span data-stu-id="cc737-507">FTP</span></span>](#ftp) |
| &nbsp; |[<span data-ttu-id="cc737-508">HDFS</span><span class="sxs-lookup"><span data-stu-id="cc737-508">HDFS</span></span>](#hdfs) |
| &nbsp; |[<span data-ttu-id="cc737-509">SFTP</span><span class="sxs-lookup"><span data-stu-id="cc737-509">SFTP</span></span>](#sftp) |
| <span data-ttu-id="cc737-510">**Egyéb**</span><span class="sxs-lookup"><span data-stu-id="cc737-510">**Others**</span></span> |[<span data-ttu-id="cc737-511">HTTP</span><span class="sxs-lookup"><span data-stu-id="cc737-511">HTTP</span></span>](#http) |
| &nbsp; |[<span data-ttu-id="cc737-512">OData</span><span class="sxs-lookup"><span data-stu-id="cc737-512">OData</span></span>](#odata) |
| &nbsp; |[<span data-ttu-id="cc737-513">ODBC</span><span class="sxs-lookup"><span data-stu-id="cc737-513">ODBC</span></span>](#odbc) |
| &nbsp; |[<span data-ttu-id="cc737-514">Salesforce</span><span class="sxs-lookup"><span data-stu-id="cc737-514">Salesforce</span></span>](#salesforce) |
| &nbsp; |[<span data-ttu-id="cc737-515">Webes tábla</span><span class="sxs-lookup"><span data-stu-id="cc737-515">Web Table</span></span>](#web-table) |

## <a name="azure-blob-storage"></a><span data-ttu-id="cc737-516">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="cc737-516">Azure Blob Storage</span></span>

### <a name="linked-service"></a><span data-ttu-id="cc737-517">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-517">Linked service</span></span>
<span data-ttu-id="cc737-518">Társított szolgáltatások két típusa van: Azure Storage társított szolgáltatásnak, és a társított szolgáltatásnak Azure Storage SAS.</span><span class="sxs-lookup"><span data-stu-id="cc737-518">There are two types of linked services: Azure Storage linked service and Azure Storage SAS linked service.</span></span>

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="cc737-519">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="cc737-519">Azure Storage Linked Service</span></span>
<span data-ttu-id="cc737-520">A data factory használatával az Azure storage-fiók összekapcsolása a **fiókkulcs**, hozzon létre egy Azure Storage társított szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="cc737-520">To link your Azure storage account to a data factory by using the **account key**, create an Azure Storage linked service.</span></span> <span data-ttu-id="cc737-521">Meghatározásához az Azure Storage társított szolgáltatásnak, és állítsa a **típus** a társított szolgáltatás **AzureStorage**.</span><span class="sxs-lookup"><span data-stu-id="cc737-521">To define an Azure Storage linked service, set the **type** of the linked service to **AzureStorage**.</span></span> <span data-ttu-id="cc737-522">Ezután megadhatja tulajdonságait a következő a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-522">Then, you can specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="cc737-523">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-523">Property</span></span> | <span data-ttu-id="cc737-524">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-524">Description</span></span> | <span data-ttu-id="cc737-525">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-525">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="cc737-526">connectionString</span><span class="sxs-lookup"><span data-stu-id="cc737-526">connectionString</span></span> |<span data-ttu-id="cc737-527">Adja meg a connectionString tulajdonság az Azure storage való kapcsolódáshoz szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="cc737-527">Specify information needed to connect to Azure storage for the connectionString property.</span></span> |<span data-ttu-id="cc737-528">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-528">Yes</span></span> |

##### <a name="example"></a><span data-ttu-id="cc737-529">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-529">Example</span></span>  

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

#### <a name="azure-storage-sas-linked-service"></a><span data-ttu-id="cc737-530">Az Azure Storage SAS társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="cc737-530">Azure Storage SAS Linked Service</span></span>
<span data-ttu-id="cc737-531">A társított Azure Storage SAS-szolgáltatás lehetővé teszi egy Azure Storage-fiók összekapcsolása egy Azure data factory egy közös hozzáférésű Jogosultságkód (SAS) használatával.</span><span class="sxs-lookup"><span data-stu-id="cc737-531">The Azure Storage SAS linked service allows you to link an Azure Storage Account to an Azure data factory by using a Shared Access Signature (SAS).</span></span> <span data-ttu-id="cc737-532">Az adat-előállítóban minden/specifikus erőforrások (blobtárolóban /) a tárolóban lévő korlátozott/időhöz kötött hozzáférést biztosít.</span><span class="sxs-lookup"><span data-stu-id="cc737-532">It provides the data factory with restricted/time-bound access to all/specific resources (blob/container) in the storage.</span></span> <span data-ttu-id="cc737-533">Az Azure storage-fiók összekapcsolása egy adat-előállító közös hozzáférésű Jogosultságkód használatával, a társított Azure Storage SAS-szolgáltatás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="cc737-533">To link your Azure storage account to a data factory by using Shared Access Signature, create an Azure Storage SAS linked service.</span></span> <span data-ttu-id="cc737-534">Egy Azure Storage SAS meghatározásához társított szolgáltatás, állítsa be a **típus** a társított szolgáltatás **AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="cc737-534">To define an Azure Storage SAS linked service, set the **type** of the linked service to **AzureStorageSas**.</span></span> <span data-ttu-id="cc737-535">Ezután megadhatja tulajdonságait a következő a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-535">Then, you can specify following properties in the **typeProperties** section:</span></span>   

| <span data-ttu-id="cc737-536">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-536">Property</span></span> | <span data-ttu-id="cc737-537">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-537">Description</span></span> | <span data-ttu-id="cc737-538">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-538">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="cc737-539">sasUri</span><span class="sxs-lookup"><span data-stu-id="cc737-539">sasUri</span></span> |<span data-ttu-id="cc737-540">Adja meg a megosztott hozzáférési aláírást URI az Azure Storage-erőforrások, például a blob, -tároló vagy tábla.</span><span class="sxs-lookup"><span data-stu-id="cc737-540">Specify Shared Access Signature URI to the Azure Storage resources such as blob, container, or table.</span></span> |<span data-ttu-id="cc737-541">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-541">Yes</span></span> |

##### <a name="example"></a><span data-ttu-id="cc737-542">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-542">Example</span></span>

```json
{  
    "name": "StorageSasLinkedService",  
    "properties": {  
        "type": "AzureStorageSas",  
        "typeProperties": {  
            "sasUri": "<storageUri>?<sasToken>"   
        }  
    }  
}  
```

<span data-ttu-id="cc737-543">További információ a következő összekapcsolt szolgáltatások: [Azure Blob Storage összekötő](data-factory-azure-blob-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-543">For more information about these linked services, see [Azure Blob Storage connector](data-factory-azure-blob-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="cc737-544">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-544">Dataset</span></span>
<span data-ttu-id="cc737-545">Adja meg az Azure-Blob adatkészletet, állítsa be a **típus** a DataSet **AzureBlob**.</span><span class="sxs-lookup"><span data-stu-id="cc737-545">To define an Azure Blob dataset, set the **type** of the dataset to **AzureBlob**.</span></span> <span data-ttu-id="cc737-546">Ezt követően adja meg a következő Azure Blob tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-546">Then, specify the following Azure Blob specific properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="cc737-547">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-547">Property</span></span> | <span data-ttu-id="cc737-548">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-548">Description</span></span> | <span data-ttu-id="cc737-549">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-549">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-550">folderPath</span><span class="sxs-lookup"><span data-stu-id="cc737-550">folderPath</span></span> |<span data-ttu-id="cc737-551">A tároló és a blob-tároló mappa elérési útja.</span><span class="sxs-lookup"><span data-stu-id="cc737-551">Path to the container and folder in the blob storage.</span></span> <span data-ttu-id="cc737-552">Példa: myblobcontainer\myblobfolder\\</span><span class="sxs-lookup"><span data-stu-id="cc737-552">Example: myblobcontainer\myblobfolder\\</span></span> |<span data-ttu-id="cc737-553">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-553">Yes</span></span> |
| <span data-ttu-id="cc737-554">fileName</span><span class="sxs-lookup"><span data-stu-id="cc737-554">fileName</span></span> |<span data-ttu-id="cc737-555">A blob neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-555">Name of the blob.</span></span> <span data-ttu-id="cc737-556">Fájlnév nem, kötelező, és a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="cc737-556">fileName is optional and case-sensitive.</span></span><br/><br/><span data-ttu-id="cc737-557">Ha megad egy fájlnevet, a tevékenység (például a Másolás) működik a megadott Blob.</span><span class="sxs-lookup"><span data-stu-id="cc737-557">If you specify a filename, the activity (including Copy) works on the specific Blob.</span></span><br/><br/><span data-ttu-id="cc737-558">Ha nincs megadva fájlnév, másolása összes BLOB bemeneti adatkészlet folderPath tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="cc737-558">When fileName is not specified, Copy includes all Blobs in the folderPath for input dataset.</span></span><br/><br/><span data-ttu-id="cc737-559">Ha nincs megadva fájlnév egy kimeneti adatkészletet, a létrehozott fájl nevét a következő lenne ebben a formátumban: adatok. <Guid>.txt (például:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="cc737-559">When fileName is not specified for an output dataset, the name of the generated file would be in the following this format: Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="cc737-560">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-560">No</span></span> |
| <span data-ttu-id="cc737-561">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="cc737-561">partitionedBy</span></span> |<span data-ttu-id="cc737-562">partitionedBy egy nem kötelező tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="cc737-562">partitionedBy is an optional property.</span></span> <span data-ttu-id="cc737-563">Használhatja a dinamikus folderPath és fájlnevét idő adatsorozat adatok megadása.</span><span class="sxs-lookup"><span data-stu-id="cc737-563">You can use it to specify a dynamic folderPath and filename for time series data.</span></span> <span data-ttu-id="cc737-564">Például folderPath is paraméteres adatok óránkénti.</span><span class="sxs-lookup"><span data-stu-id="cc737-564">For example, folderPath can be parameterized for every hour of data.</span></span> |<span data-ttu-id="cc737-565">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-565">No</span></span> |
| <span data-ttu-id="cc737-566">Formátumban</span><span class="sxs-lookup"><span data-stu-id="cc737-566">format</span></span> | <span data-ttu-id="cc737-567">A következő formátumban típusok támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="cc737-567">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="cc737-568">Állítsa be a **típus** tulajdonság a formátuma a következő értékek egyikét.</span><span class="sxs-lookup"><span data-stu-id="cc737-568">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="cc737-569">További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="cc737-569">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="cc737-570">Ha azt szeretné, hogy **másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a Formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban.</span><span class="sxs-lookup"><span data-stu-id="cc737-570">If you want to **copy files as-is** between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="cc737-571">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-571">No</span></span> |
| <span data-ttu-id="cc737-572">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="cc737-572">compression</span></span> | <span data-ttu-id="cc737-573">Adja meg a típus és az adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="cc737-573">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="cc737-574">Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="cc737-574">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="cc737-575">Támogatott szintek a következők: **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="cc737-575">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="cc737-576">További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="cc737-576">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="cc737-577">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-577">No</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-578">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-578">Example</span></span>

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "input.log",
            "folderPath": "adfgetstarted/inputdata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
 ```


<span data-ttu-id="cc737-579">További információkért lásd: [Azure Blob összekötő](data-factory-azure-blob-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-579">For more information, see [Azure Blob connector](data-factory-azure-blob-connector.md#dataset-properties) article.</span></span>

### <a name="blobsource-in-copy-activity"></a><span data-ttu-id="cc737-580">A másolási tevékenység BlobSource</span><span class="sxs-lookup"><span data-stu-id="cc737-580">BlobSource in Copy Activity</span></span>
<span data-ttu-id="cc737-581">Adatok másolása az Azure Blob-tárolóból, állítsa be a **adatforrástípust** a másolási tevékenység **BlobSource**, és adja meg a következő tulajdonságokat a ** forrás ** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-581">If you are copying data from an Azure Blob Storage, set the **source type** of the copy activity to **BlobSource**, and specify following properties in the **source **section:</span></span>

| <span data-ttu-id="cc737-582">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-582">Property</span></span> | <span data-ttu-id="cc737-583">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-583">Description</span></span> | <span data-ttu-id="cc737-584">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-584">Allowed values</span></span> | <span data-ttu-id="cc737-585">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-585">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-586">Rekurzív</span><span class="sxs-lookup"><span data-stu-id="cc737-586">recursive</span></span> |<span data-ttu-id="cc737-587">Azt jelzi, hogy az adatok olvasható rekurzív módon az almappák vagy csak a megadott mappát.</span><span class="sxs-lookup"><span data-stu-id="cc737-587">Indicates whether the data is read recursively from the sub folders or only from the specified folder.</span></span> |<span data-ttu-id="cc737-588">TRUE hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="cc737-588">True (default value), False</span></span> |<span data-ttu-id="cc737-589">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-589">No</span></span> |

#### <a name="example-blobsource"></a><span data-ttu-id="cc737-590">Példa: BlobSource **</span><span class="sxs-lookup"><span data-stu-id="cc737-590">Example: BlobSource**</span></span>
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureSqlOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "SqlSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
### <a name="blobsink-in-copy-activity"></a><span data-ttu-id="cc737-591">A másolási tevékenység BlobSink</span><span class="sxs-lookup"><span data-stu-id="cc737-591">BlobSink in Copy Activity</span></span>
<span data-ttu-id="cc737-592">Adatok másolása az Azure Blob Storage, állítsa be a **típus gyűjtése** a másolási tevékenység **BlobSink**, és adja meg a következő tulajdonságokat a **fogadó** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-592">If you are copying data to an Azure Blob Storage, set the **sink type** of the copy activity to **BlobSink**, and specify following properties in the **sink** section:</span></span>

| <span data-ttu-id="cc737-593">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-593">Property</span></span> | <span data-ttu-id="cc737-594">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-594">Description</span></span> | <span data-ttu-id="cc737-595">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-595">Allowed values</span></span> | <span data-ttu-id="cc737-596">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-596">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-597">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="cc737-597">copyBehavior</span></span> |<span data-ttu-id="cc737-598">Másolás viselkedését határozza meg, ha az adatforrás BlobSource vagy a fájlrendszer.</span><span class="sxs-lookup"><span data-stu-id="cc737-598">Defines the copy behavior when the source is BlobSource or FileSystem.</span></span> |<span data-ttu-id="cc737-599"><b>PreserveHierarchy</b>: őrzi meg a fájl hierarchia a célmappában.</span><span class="sxs-lookup"><span data-stu-id="cc737-599"><b>PreserveHierarchy</b>: preserves the file hierarchy in the target folder.</span></span> <span data-ttu-id="cc737-600">A következő forrásfájl forrásmappához relatív elérési a relatív elérési út a cél-fájlját és a célmappa megegyezik.</span><span class="sxs-lookup"><span data-stu-id="cc737-600">The relative path of source file to source folder is identical to the relative path of target file to target folder.</span></span><br/><br/><span data-ttu-id="cc737-601"><b>FlattenHierarchy</b>: a forrásmappából a fájlok a célmappában első szintjét is.</span><span class="sxs-lookup"><span data-stu-id="cc737-601"><b>FlattenHierarchy</b>: all files from the source folder are in the first level of target folder.</span></span> <span data-ttu-id="cc737-602">A fájlok céljaként automatikusan létrehozott nevet adni.</span><span class="sxs-lookup"><span data-stu-id="cc737-602">The target files have auto generated name.</span></span> <br/><br/><span data-ttu-id="cc737-603"><b>Mergefiles típusú (alapértelmezett):</b> egyesíti a forrásmappából egy fájl összes fájlt.</span><span class="sxs-lookup"><span data-stu-id="cc737-603"><b>MergeFiles (default):</b> merges all files from the source folder to one file.</span></span> <span data-ttu-id="cc737-604">Ha a fájl/Blob neve meg van adva, az egyesített neve legyen a megadott név; Ellenkező esetben lenne automatikusan létrehozott fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="cc737-604">If the File/Blob Name is specified, the merged file name would be the specified name; otherwise, would be auto-generated file name.</span></span> |<span data-ttu-id="cc737-605">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-605">No</span></span> |

#### <a name="example-blobsink"></a><span data-ttu-id="cc737-606">Példa: BlobSink</span><span class="sxs-lookup"><span data-stu-id="cc737-606">Example: BlobSink</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSQLInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="cc737-607">További információkért lásd: [Azure Blob összekötő](data-factory-azure-blob-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-607">For more information, see [Azure Blob connector](data-factory-azure-blob-connector.md#copy-activity-properties) article.</span></span> 

## <a name="azure-data-lake-store"></a><span data-ttu-id="cc737-608">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="cc737-608">Azure Data Lake Store</span></span>

### <a name="linked-service"></a><span data-ttu-id="cc737-609">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-609">Linked service</span></span>
<span data-ttu-id="cc737-610">Egy Azure Data Lake Store meghatározásához társított szolgáltatás, a társított szolgáltatás típusának beállítása **AzureDataLakeStore**, és adja meg a következő tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-610">To define an Azure Data Lake Store linked service, set the type of the linked service to **AzureDataLakeStore**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="cc737-611">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-611">Property</span></span> | <span data-ttu-id="cc737-612">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-612">Description</span></span> | <span data-ttu-id="cc737-613">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-613">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="cc737-614">type</span><span class="sxs-lookup"><span data-stu-id="cc737-614">type</span></span> | <span data-ttu-id="cc737-615">A type tulajdonságot kell beállítani: **AzureDataLakeStore**</span><span class="sxs-lookup"><span data-stu-id="cc737-615">The type property must be set to: **AzureDataLakeStore**</span></span> | <span data-ttu-id="cc737-616">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-616">Yes</span></span> |
| <span data-ttu-id="cc737-617">dataLakeStoreUri</span><span class="sxs-lookup"><span data-stu-id="cc737-617">dataLakeStoreUri</span></span> | <span data-ttu-id="cc737-618">Adja meg az Azure Data Lake Store-fiók adatait.</span><span class="sxs-lookup"><span data-stu-id="cc737-618">Specify information about the Azure Data Lake Store account.</span></span> <span data-ttu-id="cc737-619">A következő formátumban: `https://[accountname].azuredatalakestore.net/webhdfs/v1` vagy `adl://[accountname].azuredatalakestore.net/`.</span><span class="sxs-lookup"><span data-stu-id="cc737-619">It is in the following format: `https://[accountname].azuredatalakestore.net/webhdfs/v1` or `adl://[accountname].azuredatalakestore.net/`.</span></span> | <span data-ttu-id="cc737-620">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-620">Yes</span></span> |
| <span data-ttu-id="cc737-621">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="cc737-621">subscriptionId</span></span> | <span data-ttu-id="cc737-622">Azure-előfizetése azonosítóját, amelyhez a Data Lake Store tartozik.</span><span class="sxs-lookup"><span data-stu-id="cc737-622">Azure subscription Id to which Data Lake Store belongs.</span></span> | <span data-ttu-id="cc737-623">A fogadó szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-623">Required for sink</span></span> |
| <span data-ttu-id="cc737-624">erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="cc737-624">resourceGroupName</span></span> | <span data-ttu-id="cc737-625">Azure erőforráscsoport-név, amely a Data Lake Store tartozik.</span><span class="sxs-lookup"><span data-stu-id="cc737-625">Azure resource group name to which Data Lake Store belongs.</span></span> | <span data-ttu-id="cc737-626">A fogadó szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-626">Required for sink</span></span> |
| <span data-ttu-id="cc737-627">servicePrincipalId</span><span class="sxs-lookup"><span data-stu-id="cc737-627">servicePrincipalId</span></span> | <span data-ttu-id="cc737-628">Adja meg az alkalmazás ügyfél-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="cc737-628">Specify the application's client ID.</span></span> | <span data-ttu-id="cc737-629">Igen (a szolgáltatás egyszerű hitelesítés)</span><span class="sxs-lookup"><span data-stu-id="cc737-629">Yes (for service principal authentication)</span></span> |
| <span data-ttu-id="cc737-630">servicePrincipalKey</span><span class="sxs-lookup"><span data-stu-id="cc737-630">servicePrincipalKey</span></span> | <span data-ttu-id="cc737-631">Adja meg az alkalmazás kulcsot.</span><span class="sxs-lookup"><span data-stu-id="cc737-631">Specify the application's key.</span></span> | <span data-ttu-id="cc737-632">Igen (a szolgáltatás egyszerű hitelesítés)</span><span class="sxs-lookup"><span data-stu-id="cc737-632">Yes (for service principal authentication)</span></span> |
| <span data-ttu-id="cc737-633">Bérlői</span><span class="sxs-lookup"><span data-stu-id="cc737-633">tenant</span></span> | <span data-ttu-id="cc737-634">Adja meg a bérlői adatokat (tartomány nevét vagy a bérlő azonosító) alatt az alkalmazás található.</span><span class="sxs-lookup"><span data-stu-id="cc737-634">Specify the tenant information (domain name or tenant ID) under which your application resides.</span></span> <span data-ttu-id="cc737-635">Azt az Azure-portál jobb felső sarkában az egér rámutató által kérheti le.</span><span class="sxs-lookup"><span data-stu-id="cc737-635">You can retrieve it by hovering the mouse in the top-right corner of the Azure portal.</span></span> | <span data-ttu-id="cc737-636">Igen (a szolgáltatás egyszerű hitelesítés)</span><span class="sxs-lookup"><span data-stu-id="cc737-636">Yes (for service principal authentication)</span></span> |
| <span data-ttu-id="cc737-637">Engedélyezési</span><span class="sxs-lookup"><span data-stu-id="cc737-637">authorization</span></span> | <span data-ttu-id="cc737-638">Kattintson a **engedélyezés** gombra a **Data Factory Editor** , és írja be a hitelesítő adatok, amelyek az automatikusan létrehozott engedélyezési URL-címet rendel hozzá ehhez a tulajdonsághoz.</span><span class="sxs-lookup"><span data-stu-id="cc737-638">Click **Authorize** button in the **Data Factory Editor** and enter your credential that assigns the auto-generated authorization URL to this property.</span></span> | <span data-ttu-id="cc737-639">Igen (a felhasználói hitelesítő)</span><span class="sxs-lookup"><span data-stu-id="cc737-639">Yes (for user credential authentication)</span></span>|
| <span data-ttu-id="cc737-640">Munkamenet-azonosító</span><span class="sxs-lookup"><span data-stu-id="cc737-640">sessionId</span></span> | <span data-ttu-id="cc737-641">OAuth munkamenet-azonosító az OAuth hitelesítési munkamenetből.</span><span class="sxs-lookup"><span data-stu-id="cc737-641">OAuth session id from the OAuth authorization session.</span></span> <span data-ttu-id="cc737-642">Minden munkamenet-azonosító egyedi, és előfordulhat, hogy csak egyszer használható.</span><span class="sxs-lookup"><span data-stu-id="cc737-642">Each session id is unique and may only be used once.</span></span> <span data-ttu-id="cc737-643">Ez a beállítás automatikusan létrejön a Data Factory Editor használatakor.</span><span class="sxs-lookup"><span data-stu-id="cc737-643">This setting is automatically generated when you use Data Factory Editor.</span></span> | <span data-ttu-id="cc737-644">Igen (a felhasználói hitelesítő)</span><span class="sxs-lookup"><span data-stu-id="cc737-644">Yes (for user credential authentication)</span></span> |

#### <a name="example-using-service-principal-authentication"></a><span data-ttu-id="cc737-645">Példa: a szolgáltatás egyszerű hitelesítést használó</span><span class="sxs-lookup"><span data-stu-id="cc737-645">Example: using service principal authentication</span></span>
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info. Example: microsoft.onmicrosoft.com>"
        }
    }
}
```

#### <a name="example-using-user-credential-authentication"></a><span data-ttu-id="cc737-646">Példa: a felhasználói hitelesítő adatok hitelesítést használnak</span><span class="sxs-lookup"><span data-stu-id="cc737-646">Example: using user credential authentication</span></span>
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<session ID>",
            "authorization": "<authorization URL>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

<span data-ttu-id="cc737-647">További információkért lásd: [Azure Data Lake Store összekötő](data-factory-azure-datalake-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-647">For more information, see [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="cc737-648">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-648">Dataset</span></span>
<span data-ttu-id="cc737-649">Adja meg az Azure Data Lake Store adatkészlethez, állítsa be a **típus** a DataSet **AzureDataLakeStore**, és adja meg az alábbi tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-649">To define an Azure Data Lake Store dataset, set the **type** of the dataset to **AzureDataLakeStore**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="cc737-650">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-650">Property</span></span> | <span data-ttu-id="cc737-651">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-651">Description</span></span> | <span data-ttu-id="cc737-652">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-652">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="cc737-653">folderPath</span><span class="sxs-lookup"><span data-stu-id="cc737-653">folderPath</span></span> |<span data-ttu-id="cc737-654">A tároló és az Azure Data Lake mappa elérési útja tárolja.</span><span class="sxs-lookup"><span data-stu-id="cc737-654">Path to the container and folder in the Azure Data Lake store.</span></span> |<span data-ttu-id="cc737-655">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-655">Yes</span></span> |
| <span data-ttu-id="cc737-656">fileName</span><span class="sxs-lookup"><span data-stu-id="cc737-656">fileName</span></span> |<span data-ttu-id="cc737-657">A fájl az Azure Data Lake store nevét.</span><span class="sxs-lookup"><span data-stu-id="cc737-657">Name of the file in the Azure Data Lake store.</span></span> <span data-ttu-id="cc737-658">Fájlnév nem, kötelező, és a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="cc737-658">fileName is optional and case-sensitive.</span></span> <br/><br/><span data-ttu-id="cc737-659">Ha megad egy fájlnevet, a tevékenység (például a Másolás) működik az adott fájlt.</span><span class="sxs-lookup"><span data-stu-id="cc737-659">If you specify a filename, the activity (including Copy) works on the specific file.</span></span><br/><br/><span data-ttu-id="cc737-660">Ha nincs megadva fájlnév, példány bemeneti adatkészlet folderPath minden fájl tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="cc737-660">When fileName is not specified, Copy includes all files in the folderPath for input dataset.</span></span><br/><br/><span data-ttu-id="cc737-661">Ha nincs megadva fájlnév egy kimeneti adatkészletet, a létrehozott fájl nevét a következő lenne ebben a formátumban: adatok. <Guid>.txt (például:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="cc737-661">When fileName is not specified for an output dataset, the name of the generated file would be in the following this format: Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="cc737-662">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-662">No</span></span> |
| <span data-ttu-id="cc737-663">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="cc737-663">partitionedBy</span></span> |<span data-ttu-id="cc737-664">partitionedBy egy nem kötelező tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="cc737-664">partitionedBy is an optional property.</span></span> <span data-ttu-id="cc737-665">Használhatja a dinamikus folderPath és fájlnevét idő adatsorozat adatok megadása.</span><span class="sxs-lookup"><span data-stu-id="cc737-665">You can use it to specify a dynamic folderPath and filename for time series data.</span></span> <span data-ttu-id="cc737-666">Például folderPath is paraméteres adatok óránkénti.</span><span class="sxs-lookup"><span data-stu-id="cc737-666">For example, folderPath can be parameterized for every hour of data.</span></span> |<span data-ttu-id="cc737-667">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-667">No</span></span> |
| <span data-ttu-id="cc737-668">Formátumban</span><span class="sxs-lookup"><span data-stu-id="cc737-668">format</span></span> | <span data-ttu-id="cc737-669">A következő formátumban típusok támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="cc737-669">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="cc737-670">Állítsa be a **típus** tulajdonság a formátuma a következő értékek egyikét.</span><span class="sxs-lookup"><span data-stu-id="cc737-670">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="cc737-671">További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="cc737-671">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="cc737-672">Ha azt szeretné, hogy **másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a Formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban.</span><span class="sxs-lookup"><span data-stu-id="cc737-672">If you want to **copy files as-is** between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="cc737-673">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-673">No</span></span> |
| <span data-ttu-id="cc737-674">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="cc737-674">compression</span></span> | <span data-ttu-id="cc737-675">Adja meg a típus és az adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="cc737-675">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="cc737-676">Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="cc737-676">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="cc737-677">Támogatott szintek a következők: **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="cc737-677">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="cc737-678">További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="cc737-678">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="cc737-679">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-679">No</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-680">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-680">Example</span></span>
```json
{
    "name": "AzureDataLakeStoreInput",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/input/",
            "fileName": "SearchLog.tsv",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="cc737-681">További információkért lásd: [Azure Data Lake Store összekötő](data-factory-azure-datalake-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-681">For more information, see [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#dataset-properties) article.</span></span> 

### <a name="azure-data-lake-store-source-in-copy-activity"></a><span data-ttu-id="cc737-682">A másolási tevékenység az Azure Data Lake Store-forrás</span><span class="sxs-lookup"><span data-stu-id="cc737-682">Azure Data Lake Store Source in Copy Activity</span></span>
<span data-ttu-id="cc737-683">Adatok másolása az Azure Data Lake Store a, állítsa be a **adatforrástípust** a másolási tevékenység **AzureDataLakeStoreSource**, és adja meg a következő tulajdonságokat a **forrás**szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-683">If you are copying data from an Azure Data Lake Store, set the **source type** of the copy activity to **AzureDataLakeStoreSource**, and specify following properties in the **source** section:</span></span>

<span data-ttu-id="cc737-684">**AzureDataLakeStoreSource** támogatja a következő tulajdonságok **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-684">**AzureDataLakeStoreSource** supports the following properties **typeProperties** section:</span></span>

| <span data-ttu-id="cc737-685">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-685">Property</span></span> | <span data-ttu-id="cc737-686">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-686">Description</span></span> | <span data-ttu-id="cc737-687">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-687">Allowed values</span></span> | <span data-ttu-id="cc737-688">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-688">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-689">Rekurzív</span><span class="sxs-lookup"><span data-stu-id="cc737-689">recursive</span></span> |<span data-ttu-id="cc737-690">Azt jelzi, hogy az adatok olvasható rekurzív módon az almappák vagy csak a megadott mappát.</span><span class="sxs-lookup"><span data-stu-id="cc737-690">Indicates whether the data is read recursively from the sub folders or only from the specified folder.</span></span> |<span data-ttu-id="cc737-691">TRUE hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="cc737-691">True (default value), False</span></span> |<span data-ttu-id="cc737-692">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-692">No</span></span> |

#### <a name="example-azuredatalakestoresource"></a><span data-ttu-id="cc737-693">Példa: AzureDataLakeStoreSource</span><span class="sxs-lookup"><span data-stu-id="cc737-693">Example: AzureDataLakeStoreSource</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureDakeLaketoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureDataLakeStoreInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "AzureDataLakeStoreSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="cc737-694">További információkért lásd: [Azure Data Lake Store összekötő](data-factory-azure-datalake-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-694">For more information, see [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#copy-activity-properties) article.</span></span>

### <a name="azure-data-lake-store-sink-in-copy-activity"></a><span data-ttu-id="cc737-695">A másolási tevékenység az Azure Data Lake Store fogadó</span><span class="sxs-lookup"><span data-stu-id="cc737-695">Azure Data Lake Store Sink in Copy Activity</span></span>
<span data-ttu-id="cc737-696">Adatok másolása az Azure Data Lake Store, állítsa be a **típus gyűjtése** a másolási tevékenység **AzureDataLakeStoreSink**, és adja meg a következő tulajdonságokat a **fogadó** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-696">If you are copying data to an Azure Data Lake Store, set the **sink type** of the copy activity to **AzureDataLakeStoreSink**, and specify following properties in the **sink** section:</span></span>

| <span data-ttu-id="cc737-697">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-697">Property</span></span> | <span data-ttu-id="cc737-698">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-698">Description</span></span> | <span data-ttu-id="cc737-699">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-699">Allowed values</span></span> | <span data-ttu-id="cc737-700">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-700">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-701">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="cc737-701">copyBehavior</span></span> |<span data-ttu-id="cc737-702">Meghatározza a másolási viselkedését.</span><span class="sxs-lookup"><span data-stu-id="cc737-702">Specifies the copy behavior.</span></span> |<span data-ttu-id="cc737-703"><b>PreserveHierarchy</b>: őrzi meg a fájl hierarchia a célmappában.</span><span class="sxs-lookup"><span data-stu-id="cc737-703"><b>PreserveHierarchy</b>: preserves the file hierarchy in the target folder.</span></span> <span data-ttu-id="cc737-704">A következő forrásfájl forrásmappához relatív elérési a relatív elérési út a cél-fájlját és a célmappa megegyezik.</span><span class="sxs-lookup"><span data-stu-id="cc737-704">The relative path of source file to source folder is identical to the relative path of target file to target folder.</span></span><br/><br/><span data-ttu-id="cc737-705"><b>FlattenHierarchy</b>: minden fájl a forrásmappából az első szintű célmappában jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="cc737-705"><b>FlattenHierarchy</b>: all files from the source folder are created in the first level of target folder.</span></span> <span data-ttu-id="cc737-706">A cél fájlok jönnek létre automatikusan létrehozott névvel.</span><span class="sxs-lookup"><span data-stu-id="cc737-706">The target files are created with auto generated name.</span></span><br/><br/><span data-ttu-id="cc737-707"><b>Mergefiles típusú</b>: egy fájl összes fájlt a forrásmappából egyesíti.</span><span class="sxs-lookup"><span data-stu-id="cc737-707"><b>MergeFiles</b>: merges all files from the source folder to one file.</span></span> <span data-ttu-id="cc737-708">Ha a fájl/Blob neve meg van adva, az egyesített neve legyen a megadott név; Ellenkező esetben lenne automatikusan létrehozott fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="cc737-708">If the File/Blob Name is specified, the merged file name would be the specified name; otherwise, would be auto-generated file name.</span></span> |<span data-ttu-id="cc737-709">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-709">No</span></span> |

#### <a name="example-azuredatalakestoresink"></a><span data-ttu-id="cc737-710">Példa: AzureDataLakeStoreSink</span><span class="sxs-lookup"><span data-stu-id="cc737-710">Example: AzureDataLakeStoreSink</span></span>
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoDataLake",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureDataLakeStoreOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "AzureDataLakeStoreSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="cc737-711">További információkért lásd: [Azure Data Lake Store összekötő](data-factory-azure-datalake-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-711">For more information, see [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#copy-activity-properties) article.</span></span> 

## <a name="azure-cosmos-db"></a><span data-ttu-id="cc737-712">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="cc737-712">Azure Cosmos DB</span></span>  

### <a name="linked-service"></a><span data-ttu-id="cc737-713">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-713">Linked service</span></span>
<span data-ttu-id="cc737-714">Egy Azure Cosmos DB meghatározásához társított szolgáltatás, állítsa be a **típus** a társított szolgáltatás **DocumentDb**, és adja meg a következő tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-714">To define an Azure Cosmos DB linked service, set the **type** of the linked service to **DocumentDb**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="cc737-715">**Tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="cc737-715">**Property**</span></span> | <span data-ttu-id="cc737-716">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="cc737-716">**Description**</span></span> | <span data-ttu-id="cc737-717">**Szükséges**</span><span class="sxs-lookup"><span data-stu-id="cc737-717">**Required**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-718">connectionString</span><span class="sxs-lookup"><span data-stu-id="cc737-718">connectionString</span></span> |<span data-ttu-id="cc737-719">Adja meg Azure Cosmos DB adatbázishoz való kapcsolódáshoz szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="cc737-719">Specify information needed to connect to Azure Cosmos DB database.</span></span> |<span data-ttu-id="cc737-720">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-720">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-721">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-721">Example</span></span>

```json
{
    "name": "CosmosDBLinkedService",
    "properties": {
        "type": "DocumentDb",
        "typeProperties": {
            "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
    }
}
```
<span data-ttu-id="cc737-722">További információkért lásd: [Azure Cosmos DB összekötő](data-factory-azure-documentdb-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-722">For more information, see [Azure Cosmos DB connector](data-factory-azure-documentdb-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="cc737-723">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-723">Dataset</span></span>
<span data-ttu-id="cc737-724">Adja meg az Azure Cosmos DB adatkészlethez, állítsa be a **típus** a DataSet **DocumentDbCollection**, és adja meg az alábbi tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-724">To define an Azure Cosmos DB dataset, set the **type** of the dataset to **DocumentDbCollection**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="cc737-725">**Tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="cc737-725">**Property**</span></span> | <span data-ttu-id="cc737-726">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="cc737-726">**Description**</span></span> | <span data-ttu-id="cc737-727">**Szükséges**</span><span class="sxs-lookup"><span data-stu-id="cc737-727">**Required**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-728">CollectionName</span><span class="sxs-lookup"><span data-stu-id="cc737-728">collectionName</span></span> |<span data-ttu-id="cc737-729">Az Azure Cosmos DB gyűjtemény nevét.</span><span class="sxs-lookup"><span data-stu-id="cc737-729">Name of the Azure Cosmos DB collection.</span></span> |<span data-ttu-id="cc737-730">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-730">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-731">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-731">Example</span></span>

```json
{
    "name": "PersonCosmosDBTable",
    "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "CosmosDBLinkedService",
        "typeProperties": {
            "collectionName": "Person"
        },
        "external": true,
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```
<span data-ttu-id="cc737-732">További információkért lásd: [Azure Cosmos DB összekötő](data-factory-azure-documentdb-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-732">For more information, see [Azure Cosmos DB connector](data-factory-azure-documentdb-connector.md#dataset-properties) article.</span></span>

### <a name="azure-cosmos-db-collection-source-in-copy-activity"></a><span data-ttu-id="cc737-733">A másolási tevékenység Azure Cosmos DB gyűjtemény forrás</span><span class="sxs-lookup"><span data-stu-id="cc737-733">Azure Cosmos DB Collection Source in Copy Activity</span></span>
<span data-ttu-id="cc737-734">Adatok másolása az Azure Cosmos adatbázisából, állítsa be a **adatforrástípust** a másolási tevékenység **DocumentDbCollectionSource**, és adja meg a következő tulajdonságokat a **forrás**szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-734">If you are copying data from an Azure Cosmos DB, set the **source type** of the copy activity to **DocumentDbCollectionSource**, and specify following properties in the **source** section:</span></span>


| <span data-ttu-id="cc737-735">**Tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="cc737-735">**Property**</span></span> | <span data-ttu-id="cc737-736">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="cc737-736">**Description**</span></span> | <span data-ttu-id="cc737-737">**Megengedett értékek**</span><span class="sxs-lookup"><span data-stu-id="cc737-737">**Allowed values**</span></span> | <span data-ttu-id="cc737-738">**Szükséges**</span><span class="sxs-lookup"><span data-stu-id="cc737-738">**Required**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-739">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="cc737-739">query</span></span> |<span data-ttu-id="cc737-740">Adja meg a lekérdezés adatainak olvasására.</span><span class="sxs-lookup"><span data-stu-id="cc737-740">Specify the query to read data.</span></span> |<span data-ttu-id="cc737-741">Lekérdezés-karakterlánc hossza Azure Cosmos DB által támogatott.</span><span class="sxs-lookup"><span data-stu-id="cc737-741">Query string supported by Azure Cosmos DB.</span></span> <br/><br/><span data-ttu-id="cc737-742">Példa:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"`</span><span class="sxs-lookup"><span data-stu-id="cc737-742">Example: `SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"`</span></span> |<span data-ttu-id="cc737-743">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-743">No</span></span> <br/><br/><span data-ttu-id="cc737-744">Ha nincs megadva, az SQL-utasítást, amely végrehajtja a rendszer:`select <columns defined in structure> from mycollection`</span><span class="sxs-lookup"><span data-stu-id="cc737-744">If not specified, the SQL statement that is executed: `select <columns defined in structure> from mycollection`</span></span> |
| <span data-ttu-id="cc737-745">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="cc737-745">nestingSeparator</span></span> |<span data-ttu-id="cc737-746">Jelzi, hogy a dokumentum van beágyazva speciális karakter</span><span class="sxs-lookup"><span data-stu-id="cc737-746">Special character to indicate that the document is nested</span></span> |<span data-ttu-id="cc737-747">Bármely karakter.</span><span class="sxs-lookup"><span data-stu-id="cc737-747">Any character.</span></span> <br/><br/><span data-ttu-id="cc737-748">Azure Cosmos-adatbázis egy NoSQL-tároló JSON-dokumentumok, amelyben beágyazott struktúrákat engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="cc737-748">Azure Cosmos DB is a NoSQL store for JSON documents, where nested structures are allowed.</span></span> <span data-ttu-id="cc737-749">Az Azure Data Factory lehetővé teszi, hogy a felhasználó nestingSeparator, amely használatával a hierarchia jelöléséhez "."</span><span class="sxs-lookup"><span data-stu-id="cc737-749">Azure Data Factory enables user to denote hierarchy via nestingSeparator, which is “.”</span></span> <span data-ttu-id="cc737-750">a fenti példákban.</span><span class="sxs-lookup"><span data-stu-id="cc737-750">in the above examples.</span></span> <span data-ttu-id="cc737-751">A elválasztóval, a másolási tevékenység hoz létre a "Name" objektum három gyermekek elemekkel először középső és az utolsó, "Name.First", "Name.Middle" és "Name.Last" tábla definíciójában.</span><span class="sxs-lookup"><span data-stu-id="cc737-751">With the separator, the copy activity will generate the “Name” object with three children elements First, Middle and Last, according to “Name.First”, “Name.Middle” and “Name.Last” in the table definition.</span></span> |<span data-ttu-id="cc737-752">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-752">No</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-753">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-753">Example</span></span>

```json
{
    "name": "DocDbToBlobPipeline",
    "properties": {
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "DocumentDbCollectionSource",
                    "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
                    "nestingSeparator": "."
                },
                "sink": {
                    "type": "BlobSink",
                    "blobWriterAddHeader": true,
                    "writeBatchSize": 1000,
                    "writeBatchTimeout": "00:00:59"
                }
            },
            "inputs": [{
                "name": "PersonCosmosDBTable"
            }],
            "outputs": [{
                "name": "PersonBlobTableOut"
            }],
            "policy": {
                "concurrency": 1
            },
            "name": "CopyFromCosmosDbToBlob"
        }],
        "start": "2016-04-01T00:00:00",
        "end": "2016-04-02T00:00:00"
    }
}
```

### <a name="azure-cosmos-db-collection-sink-in-copy-activity"></a><span data-ttu-id="cc737-754">Az Azure Cosmos DB gyűjtemény fogadó a másolási tevékenység</span><span class="sxs-lookup"><span data-stu-id="cc737-754">Azure Cosmos DB Collection Sink in Copy Activity</span></span>
<span data-ttu-id="cc737-755">Adatok másolása az Azure Cosmos Adatbázishoz, állítsa be a **típus gyűjtése** a másolási tevékenység **DocumentDbCollectionSink**, és adja meg a következő tulajdonságokat a **fogadó** szakasz :</span><span class="sxs-lookup"><span data-stu-id="cc737-755">If you are copying data to Azure Cosmos DB, set the **sink type** of the copy activity to **DocumentDbCollectionSink**, and specify following properties in the **sink** section:</span></span>

| <span data-ttu-id="cc737-756">**Tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="cc737-756">**Property**</span></span> | <span data-ttu-id="cc737-757">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="cc737-757">**Description**</span></span> | <span data-ttu-id="cc737-758">**Megengedett értékek**</span><span class="sxs-lookup"><span data-stu-id="cc737-758">**Allowed values**</span></span> | <span data-ttu-id="cc737-759">**Szükséges**</span><span class="sxs-lookup"><span data-stu-id="cc737-759">**Required**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-760">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="cc737-760">nestingSeparator</span></span> |<span data-ttu-id="cc737-761">A forrás oszlop nevét jelzi, hogy a beágyazott dokumentum egy különleges karakterek van szükség.</span><span class="sxs-lookup"><span data-stu-id="cc737-761">A special character in the source column name to indicate that nested document is needed.</span></span> <br/><br/><span data-ttu-id="cc737-762">Például fent: `Name.First` a kimeneti táblát hoz létre a következő JSON struktúrában a Cosmos DB dokumentumban:</span><span class="sxs-lookup"><span data-stu-id="cc737-762">For example above: `Name.First` in the output table produces the following JSON structure in the Cosmos DB document:</span></span><br/><br/><span data-ttu-id="cc737-763">"Name": {</span><span class="sxs-lookup"><span data-stu-id="cc737-763">"Name": {</span></span><br/>    <span data-ttu-id="cc737-764">"Első": "John"</span><span class="sxs-lookup"><span data-stu-id="cc737-764">"First": "John"</span></span><br/><span data-ttu-id="cc737-765">},</span><span class="sxs-lookup"><span data-stu-id="cc737-765">},</span></span> |<span data-ttu-id="cc737-766">A beágyazási szinteket elválasztó karakter.</span><span class="sxs-lookup"><span data-stu-id="cc737-766">Character that is used to separate nesting levels.</span></span><br/><br/><span data-ttu-id="cc737-767">Alapértelmezett érték `.` (pont).</span><span class="sxs-lookup"><span data-stu-id="cc737-767">Default value is `.` (dot).</span></span> |<span data-ttu-id="cc737-768">A beágyazási szinteket elválasztó karakter.</span><span class="sxs-lookup"><span data-stu-id="cc737-768">Character that is used to separate nesting levels.</span></span> <br/><br/><span data-ttu-id="cc737-769">Alapértelmezett érték `.` (pont).</span><span class="sxs-lookup"><span data-stu-id="cc737-769">Default value is `.` (dot).</span></span> |
| <span data-ttu-id="cc737-770">WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="cc737-770">writeBatchSize</span></span> |<span data-ttu-id="cc737-771">Dokumentumok létrehozásához Azure Cosmos DB szolgáltatás párhuzamos kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="cc737-771">Number of parallel requests to Azure Cosmos DB service to create documents.</span></span><br/><br/><span data-ttu-id="cc737-772">A teljesítmény úgy finomhangolhatja, e tulajdonság használatával vagy az Azure Cosmos Adatbázisból adatok másolásakor.</span><span class="sxs-lookup"><span data-stu-id="cc737-772">You can fine-tune the performance when copying data to/from Azure Cosmos DB by using this property.</span></span> <span data-ttu-id="cc737-773">A jobb teljesítmény számíthat, mivel több párhuzamos kérelem Azure Cosmos DB writeBatchSize növelésével.</span><span class="sxs-lookup"><span data-stu-id="cc737-773">You can expect a better performance when you increase writeBatchSize because more parallel requests to Azure Cosmos DB are sent.</span></span> <span data-ttu-id="cc737-774">Azonban kell elkerülheti a sávszélesség-szabályozás, amely képes throw a hibaüzenet a következő: "Ez nagy lekérő".</span><span class="sxs-lookup"><span data-stu-id="cc737-774">However you’ll need to avoid throttling that can throw the error message: "Request rate is large".</span></span><br/><br/><span data-ttu-id="cc737-775">Sávszélesség-szabályozás tényező, beleértve a dokumentumok, a dokumentumok számát méretét, indexelő házirend célgyűjteményt stb határoz meg. A másolási műveletek segítségével jobban gyűjtemény (például S3) rendelkezik a legtöbb átviteli sebesség érhető el (2500 kérés egység/másodperc).</span><span class="sxs-lookup"><span data-stu-id="cc737-775">Throttling is decided by a number of factors, including size of documents, number of terms in documents, indexing policy of target collection, etc. For copy operations, you can use a better collection (for example, S3) to have the most throughput available (2,500 request units/second).</span></span> |<span data-ttu-id="cc737-776">Egész szám</span><span class="sxs-lookup"><span data-stu-id="cc737-776">Integer</span></span> |<span data-ttu-id="cc737-777">Nem (alapértelmezett: 5)</span><span class="sxs-lookup"><span data-stu-id="cc737-777">No (default: 5)</span></span> |
| <span data-ttu-id="cc737-778">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="cc737-778">writeBatchTimeout</span></span> |<span data-ttu-id="cc737-779">Várakozási idő a művelet befejezését, mielőtt azt az időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="cc737-779">Wait time for the operation to complete before it times out.</span></span> |<span data-ttu-id="cc737-780">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="cc737-780">timespan</span></span><br/><br/> <span data-ttu-id="cc737-781">Példa: "00: 30:00" (30 perc).</span><span class="sxs-lookup"><span data-stu-id="cc737-781">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="cc737-782">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-782">No</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-783">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-783">Example</span></span>

```json
{
    "name": "BlobToDocDbPipeline",
    "properties": {
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "DocumentDbCollectionSink",
                    "nestingSeparator": ".",
                    "writeBatchSize": 2,
                    "writeBatchTimeout": "00:00:00"
                },
                "translator": {
                    "type": "TabularTranslator",
                    "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, Title: Title, Suffix: Suffix"
                }
            },
            "inputs": [{
                "name": "PersonBlobTableIn"
            }],
            "outputs": [{
                "name": "PersonCosmosDbTableOut"
            }],
            "policy": {
                "concurrency": 1
            },
            "name": "CopyFromBlobToCosmosDb"
        }],
        "start": "2016-04-14T00:00:00",
        "end": "2016-04-15T00:00:00"
    }
}
```

<span data-ttu-id="cc737-784">További információkért lásd: [Azure Cosmos DB összekötő](data-factory-azure-documentdb-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-784">For more information, see [Azure Cosmos DB connector](data-factory-azure-documentdb-connector.md#copy-activity-properties) article.</span></span>

## <a name="azure-sql-database"></a><span data-ttu-id="cc737-785">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="cc737-785">Azure SQL Database</span></span>

### <a name="linked-service"></a><span data-ttu-id="cc737-786">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-786">Linked service</span></span>
<span data-ttu-id="cc737-787">Egy Azure SQL Database meghatározásához társított szolgáltatás, állítsa be a **típus** a társított szolgáltatás **AzureSqlDatabase**, és adja meg a következő tulajdonságokat a **typeProperties** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-787">To define an Azure SQL Database linked service, set the **type** of the linked service to **AzureSqlDatabase**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="cc737-788">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-788">Property</span></span> | <span data-ttu-id="cc737-789">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-789">Description</span></span> | <span data-ttu-id="cc737-790">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-790">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-791">connectionString</span><span class="sxs-lookup"><span data-stu-id="cc737-791">connectionString</span></span> |<span data-ttu-id="cc737-792">Adja meg az Azure SQL Database-példányt a connectionString tulajdonság való kapcsolódáshoz szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="cc737-792">Specify information needed to connect to the Azure SQL Database instance for the connectionString property.</span></span> |<span data-ttu-id="cc737-793">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-793">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-794">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-794">Example</span></span>
```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

<span data-ttu-id="cc737-795">További információkért lásd: [Azure SQL-összekötő](data-factory-azure-sql-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-795">For more information, see [Azure SQL connector](data-factory-azure-sql-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="cc737-796">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-796">Dataset</span></span>
<span data-ttu-id="cc737-797">Adja meg az Azure SQL Database adatkészlethez, állítsa be a **típus** a DataSet **AzureSqlTable**, és adja meg az alábbi tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-797">To define an Azure SQL Database dataset, set the **type** of the dataset to **AzureSqlTable**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="cc737-798">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-798">Property</span></span> | <span data-ttu-id="cc737-799">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-799">Description</span></span> | <span data-ttu-id="cc737-800">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-800">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-801">tableName</span><span class="sxs-lookup"><span data-stu-id="cc737-801">tableName</span></span> |<span data-ttu-id="cc737-802">A tábla vagy nézet társított szolgáltatásnak Azure SQL Database-példány neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="cc737-802">Name of the table or view in the Azure SQL Database instance that linked service refers to.</span></span> |<span data-ttu-id="cc737-803">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-803">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-804">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-804">Example</span></span>

```json
{
    "name": "AzureSqlInput",
    "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
<span data-ttu-id="cc737-805">További információkért lásd: [Azure SQL-összekötő](data-factory-azure-sql-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-805">For more information, see [Azure SQL connector](data-factory-azure-sql-connector.md#dataset-properties) article.</span></span> 

### <a name="sql-source-in-copy-activity"></a><span data-ttu-id="cc737-806">A másolási tevékenység SQL-forrás</span><span class="sxs-lookup"><span data-stu-id="cc737-806">SQL Source in Copy Activity</span></span>
<span data-ttu-id="cc737-807">Adatok másolása az Azure SQL-adatbázisból, állítsa be a **adatforrástípust** a másolási tevékenység **SqlSource**, és adja meg a következő tulajdonságokat a **forrás** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-807">If you are copying data from an Azure SQL Database, set the **source type** of the copy activity to **SqlSource**, and specify following properties in the **source** section:</span></span>


| <span data-ttu-id="cc737-808">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-808">Property</span></span> | <span data-ttu-id="cc737-809">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-809">Description</span></span> | <span data-ttu-id="cc737-810">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-810">Allowed values</span></span> | <span data-ttu-id="cc737-811">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-811">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-812">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="cc737-812">sqlReaderQuery</span></span> |<span data-ttu-id="cc737-813">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="cc737-813">Use the custom query to read data.</span></span> |<span data-ttu-id="cc737-814">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="cc737-814">SQL query string.</span></span> <span data-ttu-id="cc737-815">Példa: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="cc737-815">Example: `select * from MyTable`.</span></span> |<span data-ttu-id="cc737-816">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-816">No</span></span> |
| <span data-ttu-id="cc737-817">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="cc737-817">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="cc737-818">A tárolt eljárás, amely adatokat olvas a forrástábla neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-818">Name of the stored procedure that reads data from the source table.</span></span> |<span data-ttu-id="cc737-819">A tárolt eljárás neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-819">Name of the stored procedure.</span></span> |<span data-ttu-id="cc737-820">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-820">No</span></span> |
| <span data-ttu-id="cc737-821">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="cc737-821">storedProcedureParameters</span></span> |<span data-ttu-id="cc737-822">A tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="cc737-822">Parameters for the stored procedure.</span></span> |<span data-ttu-id="cc737-823">A név/érték párok.</span><span class="sxs-lookup"><span data-stu-id="cc737-823">Name/value pairs.</span></span> <span data-ttu-id="cc737-824">Nevét és a kis-és a paraméterek meg kell egyeznie a nevek és a kis-és nagybetűhasználat a tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="cc737-824">Names and casing of parameters must match the names and casing of the stored procedure parameters.</span></span> |<span data-ttu-id="cc737-825">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-825">No</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-826">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-826">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSQLInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
<span data-ttu-id="cc737-827">További információkért lásd: [Azure SQL-összekötő](data-factory-azure-sql-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-827">For more information, see [Azure SQL connector](data-factory-azure-sql-connector.md#copy-activity-properties) article.</span></span> 

### <a name="sql-sink-in-copy-activity"></a><span data-ttu-id="cc737-828">A másolási tevékenység SQL fogadó</span><span class="sxs-lookup"><span data-stu-id="cc737-828">SQL Sink in Copy Activity</span></span>
<span data-ttu-id="cc737-829">Adatok másolása az Azure SQL Database, állítsa be a **típus gyűjtése** a másolási tevékenység **SqlSink**, és adja meg a következő tulajdonságokat a **fogadó** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-829">If you are copying data to Azure SQL Database, set the **sink type** of the copy activity to **SqlSink**, and specify following properties in the **sink** section:</span></span>

| <span data-ttu-id="cc737-830">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-830">Property</span></span> | <span data-ttu-id="cc737-831">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-831">Description</span></span> | <span data-ttu-id="cc737-832">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-832">Allowed values</span></span> | <span data-ttu-id="cc737-833">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-833">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-834">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="cc737-834">writeBatchTimeout</span></span> |<span data-ttu-id="cc737-835">Várakozási idő a kötegelt beszúrási művelet befejezését, mielőtt azt az időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="cc737-835">Wait time for the batch insert operation to complete before it times out.</span></span> |<span data-ttu-id="cc737-836">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="cc737-836">timespan</span></span><br/><br/> <span data-ttu-id="cc737-837">Példa: "00: 30:00" (30 perc).</span><span class="sxs-lookup"><span data-stu-id="cc737-837">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="cc737-838">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-838">No</span></span> |
| <span data-ttu-id="cc737-839">WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="cc737-839">writeBatchSize</span></span> |<span data-ttu-id="cc737-840">Szúr be az SQL-tábla adatokat, amikor a puffer mérete eléri writeBatchSize.</span><span class="sxs-lookup"><span data-stu-id="cc737-840">Inserts data into the SQL table when the buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="cc737-841">Egész szám (sorok száma)</span><span class="sxs-lookup"><span data-stu-id="cc737-841">Integer (number of rows)</span></span> |<span data-ttu-id="cc737-842">Nem (alapértelmezett: 10000)</span><span class="sxs-lookup"><span data-stu-id="cc737-842">No (default: 10000)</span></span> |
| <span data-ttu-id="cc737-843">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="cc737-843">sqlWriterCleanupScript</span></span> |<span data-ttu-id="cc737-844">Adja meg egy lekérdezést a másolási tevékenység végrehajtása úgy, hogy egy adott szelet adatait.</span><span class="sxs-lookup"><span data-stu-id="cc737-844">Specify a query for Copy Activity to execute such that data of a specific slice is cleaned up.</span></span> |<span data-ttu-id="cc737-845">A lekérdezési utasítást.</span><span class="sxs-lookup"><span data-stu-id="cc737-845">A query statement.</span></span> |<span data-ttu-id="cc737-846">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-846">No</span></span> |
| <span data-ttu-id="cc737-847">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="cc737-847">sliceIdentifierColumnName</span></span> |<span data-ttu-id="cc737-848">Adja meg a másolási tevékenység során automatikusan létrejön szelet azonosítóval, amely távolítja el az adatokat egy adott szelet, amikor futtassa újra a kitöltésének oszlop nevét.</span><span class="sxs-lookup"><span data-stu-id="cc737-848">Specify a column name for Copy Activity to fill with auto generated slice identifier, which is used to clean up data of a specific slice when rerun.</span></span> |<span data-ttu-id="cc737-849">Egy oszlop binary(32) adattípusú oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-849">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="cc737-850">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-850">No</span></span> |
| <span data-ttu-id="cc737-851">sqlWriterStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="cc737-851">sqlWriterStoredProcedureName</span></span> |<span data-ttu-id="cc737-852">A tárolt eljárás neve a cél táblázatba upserts (frissítés/Beszúrás) adatok.</span><span class="sxs-lookup"><span data-stu-id="cc737-852">Name of the stored procedure that upserts (updates/inserts) data into the target table.</span></span> |<span data-ttu-id="cc737-853">A tárolt eljárás neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-853">Name of the stored procedure.</span></span> |<span data-ttu-id="cc737-854">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-854">No</span></span> |
| <span data-ttu-id="cc737-855">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="cc737-855">storedProcedureParameters</span></span> |<span data-ttu-id="cc737-856">A tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="cc737-856">Parameters for the stored procedure.</span></span> |<span data-ttu-id="cc737-857">A név/érték párok.</span><span class="sxs-lookup"><span data-stu-id="cc737-857">Name/value pairs.</span></span> <span data-ttu-id="cc737-858">Nevét és a kis-és a paraméterek meg kell egyeznie a nevek és a kis-és nagybetűhasználat a tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="cc737-858">Names and casing of parameters must match the names and casing of the stored procedure parameters.</span></span> |<span data-ttu-id="cc737-859">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-859">No</span></span> |
| <span data-ttu-id="cc737-860">sqlWriterTableType</span><span class="sxs-lookup"><span data-stu-id="cc737-860">sqlWriterTableType</span></span> |<span data-ttu-id="cc737-861">Adjon meg egy tábla típus a következő tárolt eljárás használható.</span><span class="sxs-lookup"><span data-stu-id="cc737-861">Specify a table type name to be used in the stored procedure.</span></span> <span data-ttu-id="cc737-862">Másolási tevékenység elérhetővé teszi az adatok áthelyezése egy ideiglenes táblát, amely a táblatípus.</span><span class="sxs-lookup"><span data-stu-id="cc737-862">Copy activity makes the data being moved available in a temp table with this table type.</span></span> <span data-ttu-id="cc737-863">Tárolt eljárás kódot is majd egyesítheti az adatokat, a meglévő adatok másolásának.</span><span class="sxs-lookup"><span data-stu-id="cc737-863">Stored procedure code can then merge the data being copied with existing data.</span></span> |<span data-ttu-id="cc737-864">Egy tábla környezettípus nevét.</span><span class="sxs-lookup"><span data-stu-id="cc737-864">A table type name.</span></span> |<span data-ttu-id="cc737-865">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-865">No</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-866">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-866">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureSqlOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "blobColumnSeparators": ","
                },
                "sink": {
                    "type": "SqlSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="cc737-867">További információkért lásd: [Azure SQL-összekötő](data-factory-azure-sql-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-867">For more information, see [Azure SQL connector](data-factory-azure-sql-connector.md#copy-activity-properties) article.</span></span> 

## <a name="azure-sql-data-warehouse"></a><span data-ttu-id="cc737-868">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="cc737-868">Azure SQL Data Warehouse</span></span>

### <a name="linked-service"></a><span data-ttu-id="cc737-869">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-869">Linked service</span></span>
<span data-ttu-id="cc737-870">Azure SQL Data Warehouse meghatározásához társított szolgáltatás, állítsa be a **típus** a társított szolgáltatás **AzureSqlDW**, és adja meg a következő tulajdonságokat a **typeProperties** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-870">To define an Azure SQL Data Warehouse linked service, set the **type** of the linked service to **AzureSqlDW**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="cc737-871">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-871">Property</span></span> | <span data-ttu-id="cc737-872">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-872">Description</span></span> | <span data-ttu-id="cc737-873">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-873">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-874">connectionString</span><span class="sxs-lookup"><span data-stu-id="cc737-874">connectionString</span></span> |<span data-ttu-id="cc737-875">Adja meg a connectionString tulajdonság az Azure SQL Data Warehouse-példány való kapcsolódáshoz szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="cc737-875">Specify information needed to connect to the Azure SQL Data Warehouse instance for the connectionString property.</span></span> |<span data-ttu-id="cc737-876">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-876">Yes</span></span> |



#### <a name="example"></a><span data-ttu-id="cc737-877">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-877">Example</span></span>

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

<span data-ttu-id="cc737-878">További információkért lásd: [Azure SQL Data Warehouse-összekötő](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-878">For more information, see [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="cc737-879">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-879">Dataset</span></span>
<span data-ttu-id="cc737-880">Adja meg az Azure SQL Data Warehouse adatkészlethez, állítsa be a **típus** a DataSet **AzureSqlDWTable**, és adja meg az alábbi tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-880">To define an Azure SQL Data Warehouse dataset, set the **type** of the dataset to **AzureSqlDWTable**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="cc737-881">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-881">Property</span></span> | <span data-ttu-id="cc737-882">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-882">Description</span></span> | <span data-ttu-id="cc737-883">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-883">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-884">tableName</span><span class="sxs-lookup"><span data-stu-id="cc737-884">tableName</span></span> |<span data-ttu-id="cc737-885">A tábla vagy nézet, az Azure SQL Data Warehouse-adatbázis hivatkozik a társított szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-885">Name of the table or view in the Azure SQL Data Warehouse database that the linked service refers to.</span></span> |<span data-ttu-id="cc737-886">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-886">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-887">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-887">Example</span></span>

```json
{
    "name": "AzureSqlDWInput",
    "properties": {
    "type": "AzureSqlDWTable",
        "linkedServiceName": "AzureSqlDWLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="cc737-888">További információkért lásd: [Azure SQL Data Warehouse-összekötő](data-factory-azure-sql-data-warehouse-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-888">For more information, see [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#dataset-properties) article.</span></span> 

### <a name="sql-dw-source-in-copy-activity"></a><span data-ttu-id="cc737-889">A másolási tevékenység SQL DW-forrás</span><span class="sxs-lookup"><span data-stu-id="cc737-889">SQL DW Source in Copy Activity</span></span>
<span data-ttu-id="cc737-890">Adatok másolása az Azure SQL Data Warehouse, állítsa be a **adatforrástípust** a másolási tevékenység **SqlDWSource**, és adja meg a következő tulajdonságokat a **forrás** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-890">If you are copying data from Azure SQL Data Warehouse, set the **source type** of the copy activity to **SqlDWSource**, and specify following properties in the **source** section:</span></span>


| <span data-ttu-id="cc737-891">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-891">Property</span></span> | <span data-ttu-id="cc737-892">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-892">Description</span></span> | <span data-ttu-id="cc737-893">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-893">Allowed values</span></span> | <span data-ttu-id="cc737-894">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-894">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-895">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="cc737-895">sqlReaderQuery</span></span> |<span data-ttu-id="cc737-896">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="cc737-896">Use the custom query to read data.</span></span> |<span data-ttu-id="cc737-897">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="cc737-897">SQL query string.</span></span> <span data-ttu-id="cc737-898">Például: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="cc737-898">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="cc737-899">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-899">No</span></span> |
| <span data-ttu-id="cc737-900">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="cc737-900">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="cc737-901">A tárolt eljárás, amely adatokat olvas a forrástábla neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-901">Name of the stored procedure that reads data from the source table.</span></span> |<span data-ttu-id="cc737-902">A tárolt eljárás neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-902">Name of the stored procedure.</span></span> |<span data-ttu-id="cc737-903">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-903">No</span></span> |
| <span data-ttu-id="cc737-904">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="cc737-904">storedProcedureParameters</span></span> |<span data-ttu-id="cc737-905">A tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="cc737-905">Parameters for the stored procedure.</span></span> |<span data-ttu-id="cc737-906">A név/érték párok.</span><span class="sxs-lookup"><span data-stu-id="cc737-906">Name/value pairs.</span></span> <span data-ttu-id="cc737-907">Nevét és a kis-és a paraméterek meg kell egyeznie a nevek és a kis-és nagybetűhasználat a tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="cc737-907">Names and casing of parameters must match the names and casing of the stored procedure parameters.</span></span> |<span data-ttu-id="cc737-908">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-908">No</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-909">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-909">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLDWtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSqlDWInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlDWSource",
                    "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="cc737-910">További információkért lásd: [Azure SQL Data Warehouse-összekötő](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-910">For more information, see [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) article.</span></span> 

### <a name="sql-dw-sink-in-copy-activity"></a><span data-ttu-id="cc737-911">A másolási tevékenység SQL DW fogadó</span><span class="sxs-lookup"><span data-stu-id="cc737-911">SQL DW Sink in Copy Activity</span></span>
<span data-ttu-id="cc737-912">Adatok másolása az Azure SQL Data Warehouse, állítsa be a **típus gyűjtése** a másolási tevékenység **SqlDWSink**, és adja meg a következő tulajdonságokat a **fogadó** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-912">If you are copying data to Azure SQL Data Warehouse, set the **sink type** of the copy activity to **SqlDWSink**, and specify following properties in the **sink** section:</span></span>

| <span data-ttu-id="cc737-913">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-913">Property</span></span> | <span data-ttu-id="cc737-914">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-914">Description</span></span> | <span data-ttu-id="cc737-915">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-915">Allowed values</span></span> | <span data-ttu-id="cc737-916">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-916">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-917">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="cc737-917">sqlWriterCleanupScript</span></span> |<span data-ttu-id="cc737-918">Adja meg egy lekérdezést a másolási tevékenység végrehajtása úgy, hogy egy adott szelet adatait.</span><span class="sxs-lookup"><span data-stu-id="cc737-918">Specify a query for Copy Activity to execute such that data of a specific slice is cleaned up.</span></span> |<span data-ttu-id="cc737-919">A lekérdezési utasítást.</span><span class="sxs-lookup"><span data-stu-id="cc737-919">A query statement.</span></span> |<span data-ttu-id="cc737-920">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-920">No</span></span> |
| <span data-ttu-id="cc737-921">allowPolyBase</span><span class="sxs-lookup"><span data-stu-id="cc737-921">allowPolyBase</span></span> |<span data-ttu-id="cc737-922">Azt jelzi, hogy (ha alkalmazható), a PolyBase használata helyett BULKINSERT mechanizmus.</span><span class="sxs-lookup"><span data-stu-id="cc737-922">Indicates whether to use PolyBase (when applicable) instead of BULKINSERT mechanism.</span></span> <br/><br/> <span data-ttu-id="cc737-923">**Az ajánlott módszer az adatok betöltése az SQL Data Warehouse PolyBase a használata.**</span><span class="sxs-lookup"><span data-stu-id="cc737-923">**Using PolyBase is the recommended way to load data into SQL Data Warehouse.**</span></span> |<span data-ttu-id="cc737-924">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="cc737-924">True</span></span> <br/><span data-ttu-id="cc737-925">Hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="cc737-925">False (default)</span></span> |<span data-ttu-id="cc737-926">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-926">No</span></span> |
| <span data-ttu-id="cc737-927">kapcsolódó polyBaseSettings</span><span class="sxs-lookup"><span data-stu-id="cc737-927">polyBaseSettings</span></span> |<span data-ttu-id="cc737-928">Egy csoport, amely tulajdonságok megadott, amikor a **allowPolybase** tulajdonsága **igaz**.</span><span class="sxs-lookup"><span data-stu-id="cc737-928">A group of properties that can be specified when the **allowPolybase** property is set to **true**.</span></span> |&nbsp; |<span data-ttu-id="cc737-929">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-929">No</span></span> |
| <span data-ttu-id="cc737-930">rejectValue</span><span class="sxs-lookup"><span data-stu-id="cc737-930">rejectValue</span></span> |<span data-ttu-id="cc737-931">Megadja a szám vagy is el kell utasítani, mielőtt a lekérdezés nem sikerült sorokat százalékát.</span><span class="sxs-lookup"><span data-stu-id="cc737-931">Specifies the number or percentage of rows that can be rejected before the query fails.</span></span> <br/><br/><span data-ttu-id="cc737-932">További információ a PolyBase utasítsa el a beállítások elemre a **argumentumok** szakasza [külső tábla létrehozása (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) témakör.</span><span class="sxs-lookup"><span data-stu-id="cc737-932">Learn more about the PolyBase’s reject options in the **Arguments** section of [CREATE EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) topic.</span></span> |<span data-ttu-id="cc737-933">0 (alapértelmezés), 1, 2...</span><span class="sxs-lookup"><span data-stu-id="cc737-933">0 (default), 1, 2, …</span></span> |<span data-ttu-id="cc737-934">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-934">No</span></span> |
| <span data-ttu-id="cc737-935">rejectType</span><span class="sxs-lookup"><span data-stu-id="cc737-935">rejectType</span></span> |<span data-ttu-id="cc737-936">Határozza meg, hogy a rejectValue beállítás konstans értéket vagy százalékában van megadva.</span><span class="sxs-lookup"><span data-stu-id="cc737-936">Specifies whether the rejectValue option is specified as a literal value or a percentage.</span></span> |<span data-ttu-id="cc737-937">Érték (alapértelmezett), százalékos aránya</span><span class="sxs-lookup"><span data-stu-id="cc737-937">Value (default), Percentage</span></span> |<span data-ttu-id="cc737-938">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-938">No</span></span> |
| <span data-ttu-id="cc737-939">rejectSampleValue</span><span class="sxs-lookup"><span data-stu-id="cc737-939">rejectSampleValue</span></span> |<span data-ttu-id="cc737-940">Mielőtt a PolyBase újraszámítja a visszautasított sorok százalékát beolvasandó sorok számát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="cc737-940">Determines the number of rows to retrieve before the PolyBase recalculates the percentage of rejected rows.</span></span> |<span data-ttu-id="cc737-941">1, 2, …</span><span class="sxs-lookup"><span data-stu-id="cc737-941">1, 2, …</span></span> |<span data-ttu-id="cc737-942">Igen, ha **rejectType** van **százalékos aránya**</span><span class="sxs-lookup"><span data-stu-id="cc737-942">Yes, if **rejectType** is **percentage**</span></span> |
| <span data-ttu-id="cc737-943">useTypeDefault</span><span class="sxs-lookup"><span data-stu-id="cc737-943">useTypeDefault</span></span> |<span data-ttu-id="cc737-944">Megadja, hogyan legyen kezelve tagolt szövegfájlok a hiányzó értékeket, amikor a PolyBase kér le adatokat a szövegfájlból.</span><span class="sxs-lookup"><span data-stu-id="cc737-944">Specifies how to handle missing values in delimited text files when PolyBase retrieves data from the text file.</span></span><br/><br/><span data-ttu-id="cc737-945">Ezt a tulajdonságot, az argumentumok ismertető részben olvashat [létrehozása külső FÁJLFORMÁTUM (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).</span><span class="sxs-lookup"><span data-stu-id="cc737-945">Learn more about this property from the Arguments section in [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).</span></span> |<span data-ttu-id="cc737-946">IGAZ, hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="cc737-946">True, False (default)</span></span> |<span data-ttu-id="cc737-947">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-947">No</span></span> |
| <span data-ttu-id="cc737-948">WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="cc737-948">writeBatchSize</span></span> |<span data-ttu-id="cc737-949">Adatok beszúrása a SQL táblázatba, amikor a puffer mérete eléri writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="cc737-949">Inserts data into the SQL table when the buffer size reaches writeBatchSize</span></span> |<span data-ttu-id="cc737-950">Egész szám (sorok száma)</span><span class="sxs-lookup"><span data-stu-id="cc737-950">Integer (number of rows)</span></span> |<span data-ttu-id="cc737-951">Nem (alapértelmezett: 10000)</span><span class="sxs-lookup"><span data-stu-id="cc737-951">No (default: 10000)</span></span> |
| <span data-ttu-id="cc737-952">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="cc737-952">writeBatchTimeout</span></span> |<span data-ttu-id="cc737-953">Várakozási idő a kötegelt beszúrási művelet befejezését, mielőtt azt az időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="cc737-953">Wait time for the batch insert operation to complete before it times out.</span></span> |<span data-ttu-id="cc737-954">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="cc737-954">timespan</span></span><br/><br/> <span data-ttu-id="cc737-955">Példa: "00: 30:00" (30 perc).</span><span class="sxs-lookup"><span data-stu-id="cc737-955">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="cc737-956">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-956">No</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-957">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-957">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQLDW",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureSqlDWOutput"
            }],
            "typeProperties": {
                "source": {
                "type": "BlobSource",
                    "blobColumnSeparators": ","
                },
                "sink": {
                    "type": "SqlDWSink",
                    "allowPolyBase": true
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="cc737-958">További információkért lásd: [Azure SQL Data Warehouse-összekötő](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-958">For more information, see [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) article.</span></span> 

## <a name="azure-search"></a><span data-ttu-id="cc737-959">Azure Search</span><span class="sxs-lookup"><span data-stu-id="cc737-959">Azure Search</span></span>

### <a name="linked-service"></a><span data-ttu-id="cc737-960">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-960">Linked service</span></span>
<span data-ttu-id="cc737-961">Meghatározásához az Azure Search társított szolgáltatás, állítsa be a **típus** a társított szolgáltatás **AzureSearch**, és adja meg a következő tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-961">To define an Azure Search linked service, set the **type** of the linked service to **AzureSearch**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="cc737-962">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-962">Property</span></span> | <span data-ttu-id="cc737-963">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-963">Description</span></span> | <span data-ttu-id="cc737-964">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-964">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="cc737-965">URL-címe</span><span class="sxs-lookup"><span data-stu-id="cc737-965">url</span></span> | <span data-ttu-id="cc737-966">Az Azure Search szolgáltatás URL-címe.</span><span class="sxs-lookup"><span data-stu-id="cc737-966">URL for the Azure Search service.</span></span> | <span data-ttu-id="cc737-967">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-967">Yes</span></span> |
| <span data-ttu-id="cc737-968">kulcs</span><span class="sxs-lookup"><span data-stu-id="cc737-968">key</span></span> | <span data-ttu-id="cc737-969">Az Azure Search szolgáltatás adminisztrációs kulcsot.</span><span class="sxs-lookup"><span data-stu-id="cc737-969">Admin key for the Azure Search service.</span></span> | <span data-ttu-id="cc737-970">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-970">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-971">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-971">Example</span></span>

```json
{
    "name": "AzureSearchLinkedService",
    "properties": {
        "type": "AzureSearch",
        "typeProperties": {
            "url": "https://<service>.search.windows.net",
            "key": "<AdminKey>"
        }
    }
}
```

<span data-ttu-id="cc737-972">További információkért lásd: [Azure Search összekötő](data-factory-azure-search-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-972">For more information, see [Azure Search connector](data-factory-azure-search-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="cc737-973">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-973">Dataset</span></span>
<span data-ttu-id="cc737-974">Adja meg az Azure Search adatkészlethez, állítsa be a **típus** a DataSet **AzureSearchIndex**, és adja meg az alábbi tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-974">To define an Azure Search dataset, set the **type** of the dataset to **AzureSearchIndex**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="cc737-975">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-975">Property</span></span> | <span data-ttu-id="cc737-976">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-976">Description</span></span> | <span data-ttu-id="cc737-977">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-977">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="cc737-978">type</span><span class="sxs-lookup"><span data-stu-id="cc737-978">type</span></span> | <span data-ttu-id="cc737-979">A type tulajdonságot meg kell **AzureSearchIndex**.</span><span class="sxs-lookup"><span data-stu-id="cc737-979">The type property must be set to **AzureSearchIndex**.</span></span>| <span data-ttu-id="cc737-980">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-980">Yes</span></span> |
| <span data-ttu-id="cc737-981">indexName</span><span class="sxs-lookup"><span data-stu-id="cc737-981">indexName</span></span> | <span data-ttu-id="cc737-982">Az Azure Search-index neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-982">Name of the Azure Search index.</span></span> <span data-ttu-id="cc737-983">Adat-előállító nem hoz létre az indexet.</span><span class="sxs-lookup"><span data-stu-id="cc737-983">Data Factory does not create the index.</span></span> <span data-ttu-id="cc737-984">Az index az Azure Search léteznie kell.</span><span class="sxs-lookup"><span data-stu-id="cc737-984">The index must exist in Azure Search.</span></span> | <span data-ttu-id="cc737-985">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-985">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-986">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-986">Example</span></span>

```json
{
    "name": "AzureSearchIndexDataset",
    "properties": {
        "type": "AzureSearchIndex",
        "linkedServiceName": "AzureSearchLinkedService",
        "typeProperties": {
            "indexName": "products"
        },
        "availability": {
            "frequency": "Minute",
            "interval": 15
        }
    }
}
```

<span data-ttu-id="cc737-987">További információkért lásd: [Azure Search összekötő](data-factory-azure-search-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-987">For more information, see [Azure Search connector](data-factory-azure-search-connector.md#dataset-properties) article.</span></span>

### <a name="azure-search-index-sink-in-copy-activity"></a><span data-ttu-id="cc737-988">A másolási tevékenység az Azure Search-Index fogadó</span><span class="sxs-lookup"><span data-stu-id="cc737-988">Azure Search Index Sink in Copy Activity</span></span>
<span data-ttu-id="cc737-989">Adatok másolása az Azure Search-index, állítsa be a **típus gyűjtése** a másolási tevékenység **AzureSearchIndexSink**, és adja meg a következő tulajdonságokat a **fogadó** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-989">If you are copying data to an Azure Search index, set the **sink type** of the copy activity to **AzureSearchIndexSink**, and specify following properties in the **sink** section:</span></span>

| <span data-ttu-id="cc737-990">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-990">Property</span></span> | <span data-ttu-id="cc737-991">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-991">Description</span></span> | <span data-ttu-id="cc737-992">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-992">Allowed values</span></span> | <span data-ttu-id="cc737-993">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-993">Required</span></span> |
| -------- | ----------- | -------------- | -------- |
| <span data-ttu-id="cc737-994">WriteBehavior</span><span class="sxs-lookup"><span data-stu-id="cc737-994">WriteBehavior</span></span> | <span data-ttu-id="cc737-995">Megadja, hogy egyesíteni vagy cserélje le, ha az index már létezik egy dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="cc737-995">Specifies whether to merge or replace when a document already exists in the index.</span></span> | <span data-ttu-id="cc737-996">Egyesítés (alapértelmezett)</span><span class="sxs-lookup"><span data-stu-id="cc737-996">Merge (default)</span></span><br/><span data-ttu-id="cc737-997">Feltöltés</span><span class="sxs-lookup"><span data-stu-id="cc737-997">Upload</span></span>| <span data-ttu-id="cc737-998">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-998">No</span></span> |
| <span data-ttu-id="cc737-999">WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="cc737-999">WriteBatchSize</span></span> | <span data-ttu-id="cc737-1000">Amikor a puffer mérete eléri writeBatchSize feltölti az adatok be az Azure Search-index.</span><span class="sxs-lookup"><span data-stu-id="cc737-1000">Uploads data into the Azure Search index when the buffer size reaches writeBatchSize.</span></span> | <span data-ttu-id="cc737-1001">1-1 000.</span><span class="sxs-lookup"><span data-stu-id="cc737-1001">1 to 1,000.</span></span> <span data-ttu-id="cc737-1002">Alapértelmezett érték 1000.</span><span class="sxs-lookup"><span data-stu-id="cc737-1002">Default value is 1000.</span></span> | <span data-ttu-id="cc737-1003">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1003">No</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1004">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1004">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "SqlServertoAzureSearchIndex",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": " SqlServerInput"
            }],
            "outputs": [{
                "name": "AzureSearchIndexDataset"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "AzureSearchIndexSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="cc737-1005">További információkért lásd: [Azure Search összekötő](data-factory-azure-search-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1005">For more information, see [Azure Search connector](data-factory-azure-search-connector.md#copy-activity-properties) article.</span></span>

## <a name="azure-table-storage"></a><span data-ttu-id="cc737-1006">Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="cc737-1006">Azure Table Storage</span></span>

### <a name="linked-service"></a><span data-ttu-id="cc737-1007">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-1007">Linked service</span></span>
<span data-ttu-id="cc737-1008">Társított szolgáltatások két típusa van: Azure Storage társított szolgáltatásnak, és a társított szolgáltatásnak Azure Storage SAS.</span><span class="sxs-lookup"><span data-stu-id="cc737-1008">There are two types of linked services: Azure Storage linked service and Azure Storage SAS linked service.</span></span>

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="cc737-1009">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="cc737-1009">Azure Storage Linked Service</span></span>
<span data-ttu-id="cc737-1010">A data factory használatával az Azure storage-fiók összekapcsolása a **fiókkulcs**, hozzon létre egy Azure Storage társított szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="cc737-1010">To link your Azure storage account to a data factory by using the **account key**, create an Azure Storage linked service.</span></span> <span data-ttu-id="cc737-1011">Meghatározásához az Azure Storage társított szolgáltatásnak, és állítsa a **típus** a társított szolgáltatás **AzureStorage**.</span><span class="sxs-lookup"><span data-stu-id="cc737-1011">To define an Azure Storage linked service, set the **type** of the linked service to **AzureStorage**.</span></span> <span data-ttu-id="cc737-1012">Ezután megadhatja tulajdonságait a következő a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1012">Then, you can specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="cc737-1013">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1013">Property</span></span> | <span data-ttu-id="cc737-1014">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1014">Description</span></span> | <span data-ttu-id="cc737-1015">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1015">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="cc737-1016">type</span><span class="sxs-lookup"><span data-stu-id="cc737-1016">type</span></span> |<span data-ttu-id="cc737-1017">A type tulajdonságot kell beállítani: **AzureStorage**</span><span class="sxs-lookup"><span data-stu-id="cc737-1017">The type property must be set to: **AzureStorage**</span></span> |<span data-ttu-id="cc737-1018">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1018">Yes</span></span> |
| <span data-ttu-id="cc737-1019">connectionString</span><span class="sxs-lookup"><span data-stu-id="cc737-1019">connectionString</span></span> |<span data-ttu-id="cc737-1020">Adja meg a connectionString tulajdonság az Azure storage való kapcsolódáshoz szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="cc737-1020">Specify information needed to connect to Azure storage for the connectionString property.</span></span> |<span data-ttu-id="cc737-1021">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1021">Yes</span></span> |

<span data-ttu-id="cc737-1022">**Példa**</span><span class="sxs-lookup"><span data-stu-id="cc737-1022">**Example:**</span></span>  

```json
{  
    "name": "StorageLinkedService",  
    "properties": {  
        "type": "AzureStorage",  
        "typeProperties": {  
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"  
        }  
    }  
}  
```

#### <a name="azure-storage-sas-linked-service"></a><span data-ttu-id="cc737-1023">Az Azure Storage SAS társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="cc737-1023">Azure Storage SAS Linked Service</span></span>
<span data-ttu-id="cc737-1024">A társított Azure Storage SAS-szolgáltatás lehetővé teszi egy Azure Storage-fiók összekapcsolása egy Azure data factory egy közös hozzáférésű Jogosultságkód (SAS) használatával.</span><span class="sxs-lookup"><span data-stu-id="cc737-1024">The Azure Storage SAS linked service allows you to link an Azure Storage Account to an Azure data factory by using a Shared Access Signature (SAS).</span></span> <span data-ttu-id="cc737-1025">Az adat-előállítóban minden/specifikus erőforrások (blobtárolóban /) a tárolóban lévő korlátozott/időhöz kötött hozzáférést biztosít.</span><span class="sxs-lookup"><span data-stu-id="cc737-1025">It provides the data factory with restricted/time-bound access to all/specific resources (blob/container) in the storage.</span></span> <span data-ttu-id="cc737-1026">Az Azure storage-fiók összekapcsolása egy adat-előállító közös hozzáférésű Jogosultságkód használatával, a társított Azure Storage SAS-szolgáltatás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="cc737-1026">To link your Azure storage account to a data factory by using Shared Access Signature, create an Azure Storage SAS linked service.</span></span> <span data-ttu-id="cc737-1027">Egy Azure Storage SAS meghatározásához társított szolgáltatás, állítsa be a **típus** a társított szolgáltatás **AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="cc737-1027">To define an Azure Storage SAS linked service, set the **type** of the linked service to **AzureStorageSas**.</span></span> <span data-ttu-id="cc737-1028">Ezután megadhatja tulajdonságait a következő a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1028">Then, you can specify following properties in the **typeProperties** section:</span></span>   

| <span data-ttu-id="cc737-1029">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1029">Property</span></span> | <span data-ttu-id="cc737-1030">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1030">Description</span></span> | <span data-ttu-id="cc737-1031">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1031">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="cc737-1032">type</span><span class="sxs-lookup"><span data-stu-id="cc737-1032">type</span></span> |<span data-ttu-id="cc737-1033">A type tulajdonságot kell beállítani: **AzureStorageSas**</span><span class="sxs-lookup"><span data-stu-id="cc737-1033">The type property must be set to: **AzureStorageSas**</span></span> |<span data-ttu-id="cc737-1034">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1034">Yes</span></span> |
| <span data-ttu-id="cc737-1035">sasUri</span><span class="sxs-lookup"><span data-stu-id="cc737-1035">sasUri</span></span> |<span data-ttu-id="cc737-1036">Adja meg a megosztott hozzáférési aláírást URI az Azure Storage-erőforrások, például a blob, -tároló vagy tábla.</span><span class="sxs-lookup"><span data-stu-id="cc737-1036">Specify Shared Access Signature URI to the Azure Storage resources such as blob, container, or table.</span></span> |<span data-ttu-id="cc737-1037">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1037">Yes</span></span> |

<span data-ttu-id="cc737-1038">**Példa**</span><span class="sxs-lookup"><span data-stu-id="cc737-1038">**Example:**</span></span>

```json
{  
    "name": "StorageSasLinkedService",  
    "properties": {  
        "type": "AzureStorageSas",  
        "typeProperties": {  
            "sasUri": "<storageUri>?<sasToken>"   
        }  
    }  
}  
```

<span data-ttu-id="cc737-1039">További információ a következő összekapcsolt szolgáltatások: [Azure Table Storage összekötő](data-factory-azure-table-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1039">For more information about these linked services, see [Azure Table Storage connector](data-factory-azure-table-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="cc737-1040">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-1040">Dataset</span></span>
<span data-ttu-id="cc737-1041">Adja meg az Azure Table-adatkészlet, állítsa be a **típus** a DataSet **AzureTable**, és adja meg az alábbi tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1041">To define an Azure Table dataset, set the **type** of the dataset to **AzureTable**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="cc737-1042">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1042">Property</span></span> | <span data-ttu-id="cc737-1043">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1043">Description</span></span> | <span data-ttu-id="cc737-1044">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1044">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-1045">tableName</span><span class="sxs-lookup"><span data-stu-id="cc737-1045">tableName</span></span> |<span data-ttu-id="cc737-1046">Az az Azure tábla adatbázispéldány táblájának, amelyre a társított szolgáltatás neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="cc737-1046">Name of the table in the Azure Table Database instance that linked service refers to.</span></span> |<span data-ttu-id="cc737-1047">Igen.</span><span class="sxs-lookup"><span data-stu-id="cc737-1047">Yes.</span></span> <span data-ttu-id="cc737-1048">Amikor egy Táblanév egy azureTableSourceQuery nélkül van megadva, a tábla összes rekordot a cél lesz másolva.</span><span class="sxs-lookup"><span data-stu-id="cc737-1048">When a tableName is specified without an azureTableSourceQuery, all records from the table are copied to the destination.</span></span> <span data-ttu-id="cc737-1049">Ha egy azureTableSourceQuery is meg van adva, a cél a táblázatból, amely eleget tesz a lekérdezés rekordok lesz másolva.</span><span class="sxs-lookup"><span data-stu-id="cc737-1049">If an azureTableSourceQuery is also specified, records from the table that satisfies the query are copied to the destination.</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1050">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1050">Example</span></span>

```json
{
    "name": "AzureTableInput",
    "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="cc737-1051">További információ a következő összekapcsolt szolgáltatások: [Azure Table Storage összekötő](data-factory-azure-table-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1051">For more information about these linked services, see [Azure Table Storage connector](data-factory-azure-table-connector.md#dataset-properties) article.</span></span> 

### <a name="azure-table-source-in-copy-activity"></a><span data-ttu-id="cc737-1052">A másolási tevékenység az Azure tábla forrása</span><span class="sxs-lookup"><span data-stu-id="cc737-1052">Azure Table Source in Copy Activity</span></span>
<span data-ttu-id="cc737-1053">Adatok másolása az Azure Table Storage, állítsa be a **adatforrástípust** a másolási tevékenység **AzureTableSource**, és adja meg a következő tulajdonságokat a **forrás** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1053">If you are copying data from Azure Table Storage, set the **source type** of the copy activity to **AzureTableSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="cc737-1054">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1054">Property</span></span> | <span data-ttu-id="cc737-1055">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1055">Description</span></span> | <span data-ttu-id="cc737-1056">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-1056">Allowed values</span></span> | <span data-ttu-id="cc737-1057">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1057">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-1058">azureTableSourceQuery</span><span class="sxs-lookup"><span data-stu-id="cc737-1058">azureTableSourceQuery</span></span> |<span data-ttu-id="cc737-1059">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="cc737-1059">Use the custom query to read data.</span></span> |<span data-ttu-id="cc737-1060">Azure-tábla lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="cc737-1060">Azure table query string.</span></span> <span data-ttu-id="cc737-1061">Példák a következő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="cc737-1061">See examples in the next section.</span></span> |<span data-ttu-id="cc737-1062">Nem.</span><span class="sxs-lookup"><span data-stu-id="cc737-1062">No.</span></span> <span data-ttu-id="cc737-1063">Amikor egy Táblanév egy azureTableSourceQuery nélkül van megadva, a tábla összes rekordot a cél lesz másolva.</span><span class="sxs-lookup"><span data-stu-id="cc737-1063">When a tableName is specified without an azureTableSourceQuery, all records from the table are copied to the destination.</span></span> <span data-ttu-id="cc737-1064">Ha egy azureTableSourceQuery is meg van adva, a cél a táblázatból, amely eleget tesz a lekérdezés rekordok lesz másolva.</span><span class="sxs-lookup"><span data-stu-id="cc737-1064">If an azureTableSourceQuery is also specified, records from the table that satisfies the query are copied to the destination.</span></span> |
| <span data-ttu-id="cc737-1065">azureTableSourceIgnoreTableNotFound</span><span class="sxs-lookup"><span data-stu-id="cc737-1065">azureTableSourceIgnoreTableNotFound</span></span> |<span data-ttu-id="cc737-1066">Azt jelzi, hogy a tábla kivétel swallow nem létezik.</span><span class="sxs-lookup"><span data-stu-id="cc737-1066">Indicate whether swallow the exception of table not exist.</span></span> |<span data-ttu-id="cc737-1067">IGAZ</span><span class="sxs-lookup"><span data-stu-id="cc737-1067">TRUE</span></span><br/><span data-ttu-id="cc737-1068">HAMIS</span><span class="sxs-lookup"><span data-stu-id="cc737-1068">FALSE</span></span> |<span data-ttu-id="cc737-1069">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1069">No</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1070">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1070">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureTabletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureTableInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "AzureTableSource",
                    "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="cc737-1071">További információ a következő összekapcsolt szolgáltatások: [Azure Table Storage összekötő](data-factory-azure-table-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1071">For more information about these linked services, see [Azure Table Storage connector](data-factory-azure-table-connector.md#copy-activity-properties) article.</span></span> 

### <a name="azure-table-sink-in-copy-activity"></a><span data-ttu-id="cc737-1072">A másolási tevékenység az Azure tábla fogadó</span><span class="sxs-lookup"><span data-stu-id="cc737-1072">Azure Table Sink in Copy Activity</span></span>
<span data-ttu-id="cc737-1073">Adatok másolása az Azure Table Storage, állítsa be a **típus gyűjtése** a másolási tevékenység **AzureTableSink**, és adja meg a következő tulajdonságokat a **fogadó** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1073">If you are copying data to Azure Table Storage, set the **sink type** of the copy activity to **AzureTableSink**, and specify following properties in the **sink** section:</span></span>

| <span data-ttu-id="cc737-1074">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1074">Property</span></span> | <span data-ttu-id="cc737-1075">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1075">Description</span></span> | <span data-ttu-id="cc737-1076">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-1076">Allowed values</span></span> | <span data-ttu-id="cc737-1077">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1077">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-1078">azureTableDefaultPartitionKeyValue</span><span class="sxs-lookup"><span data-stu-id="cc737-1078">azureTableDefaultPartitionKeyValue</span></span> |<span data-ttu-id="cc737-1079">Alapértelmezett partíció kulcs értékét, amely a fogadó által használható.</span><span class="sxs-lookup"><span data-stu-id="cc737-1079">Default partition key value that can be used by the sink.</span></span> |<span data-ttu-id="cc737-1080">Egy karakterlánc-érték.</span><span class="sxs-lookup"><span data-stu-id="cc737-1080">A string value.</span></span> |<span data-ttu-id="cc737-1081">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1081">No</span></span> |
| <span data-ttu-id="cc737-1082">azureTablePartitionKeyName</span><span class="sxs-lookup"><span data-stu-id="cc737-1082">azureTablePartitionKeyName</span></span> |<span data-ttu-id="cc737-1083">Adja meg az oszlop, amelynek értékeket fogja használni, mint partíciókulcsok nevét.</span><span class="sxs-lookup"><span data-stu-id="cc737-1083">Specify name of the column whose values are used as partition keys.</span></span> <span data-ttu-id="cc737-1084">Ha nincs megadva, a partíciós kulcs AzureTableDefaultPartitionKeyValue lesz.</span><span class="sxs-lookup"><span data-stu-id="cc737-1084">If not specified, AzureTableDefaultPartitionKeyValue is used as the partition key.</span></span> |<span data-ttu-id="cc737-1085">Egy oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1085">A column name.</span></span> |<span data-ttu-id="cc737-1086">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1086">No</span></span> |
| <span data-ttu-id="cc737-1087">azureTableRowKeyName</span><span class="sxs-lookup"><span data-stu-id="cc737-1087">azureTableRowKeyName</span></span> |<span data-ttu-id="cc737-1088">Adja meg az oszlop, amelynek oszlop értékeit sor kulcsaként vannak használatban.</span><span class="sxs-lookup"><span data-stu-id="cc737-1088">Specify name of the column whose column values are used as row key.</span></span> <span data-ttu-id="cc737-1089">Ha nincs megadva, minden egyes sorára használjon a GUID Azonosítót.</span><span class="sxs-lookup"><span data-stu-id="cc737-1089">If not specified, use a GUID for each row.</span></span> |<span data-ttu-id="cc737-1090">Egy oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1090">A column name.</span></span> |<span data-ttu-id="cc737-1091">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1091">No</span></span> |
| <span data-ttu-id="cc737-1092">azureTableInsertType</span><span class="sxs-lookup"><span data-stu-id="cc737-1092">azureTableInsertType</span></span> |<span data-ttu-id="cc737-1093">A mód lehet adatokat beszúrni az Azure-tábla.</span><span class="sxs-lookup"><span data-stu-id="cc737-1093">The mode to insert data into Azure table.</span></span><br/><br/><span data-ttu-id="cc737-1094">Ez a tulajdonság szabja meg, hogy rendelkeznek-e a meglévő sorokat a táblában az egyező partíció-és sorkulcsok cseréje vagy egyesített értékükre.</span><span class="sxs-lookup"><span data-stu-id="cc737-1094">This property controls whether existing rows in the output table with matching partition and row keys have their values replaced or merged.</span></span> <br/><br/><span data-ttu-id="cc737-1095">Ezeket a beállításokat (lemezegyesítési és -csere) működése, lásd: [Insert vagy az egyesítéses entitás](https://msdn.microsoft.com/library/azure/hh452241.aspx) és [Insert vagy az entitás cseréje](https://msdn.microsoft.com/library/azure/hh452242.aspx) témaköröket.</span><span class="sxs-lookup"><span data-stu-id="cc737-1095">To learn about how these settings (merge and replace) work, see [Insert or Merge Entity](https://msdn.microsoft.com/library/azure/hh452241.aspx) and [Insert or Replace Entity](https://msdn.microsoft.com/library/azure/hh452242.aspx) topics.</span></span> <br/><br> <span data-ttu-id="cc737-1096">Ez a beállítás a sor szintjén, a táblázatok szintjén nem vonatkozik, és sem a lehetőség törli a kimeneti táblához, amely nem szerepel a bemeneti sorokat.</span><span class="sxs-lookup"><span data-stu-id="cc737-1096">This setting applies at the row level, not the table level, and neither option deletes rows in the output table that do not exist in the input.</span></span> |<span data-ttu-id="cc737-1097">Egyesítés (alapértelmezett)</span><span class="sxs-lookup"><span data-stu-id="cc737-1097">merge (default)</span></span><br/><span data-ttu-id="cc737-1098">cserélje le</span><span class="sxs-lookup"><span data-stu-id="cc737-1098">replace</span></span> |<span data-ttu-id="cc737-1099">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1099">No</span></span> |
| <span data-ttu-id="cc737-1100">WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="cc737-1100">writeBatchSize</span></span> |<span data-ttu-id="cc737-1101">Amikor writeBatchSize vagy writeBatchTimeout találati adatok beillesztése az Azure-tábla.</span><span class="sxs-lookup"><span data-stu-id="cc737-1101">Inserts data into the Azure table when the writeBatchSize or writeBatchTimeout is hit.</span></span> |<span data-ttu-id="cc737-1102">Egész szám (sorok száma)</span><span class="sxs-lookup"><span data-stu-id="cc737-1102">Integer (number of rows)</span></span> |<span data-ttu-id="cc737-1103">Nem (alapértelmezett: 10000)</span><span class="sxs-lookup"><span data-stu-id="cc737-1103">No (default: 10000)</span></span> |
| <span data-ttu-id="cc737-1104">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="cc737-1104">writeBatchTimeout</span></span> |<span data-ttu-id="cc737-1105">Adatok szúr be az Azure-táblázatra, ha a writeBatchSize vagy writeBatchTimeout találati</span><span class="sxs-lookup"><span data-stu-id="cc737-1105">Inserts data into the Azure table when the writeBatchSize or writeBatchTimeout is hit</span></span> |<span data-ttu-id="cc737-1106">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="cc737-1106">timespan</span></span><br/><br/><span data-ttu-id="cc737-1107">Példa: "00: 20:00" (20 perc)</span><span class="sxs-lookup"><span data-stu-id="cc737-1107">Example: “00:20:00” (20 minutes)</span></span> |<span data-ttu-id="cc737-1108">Nem (alapértelmezett tároló ügyfél alapértelmezett időtúllépési érték 90 másodperc)</span><span class="sxs-lookup"><span data-stu-id="cc737-1108">No (Default to storage client default timeout value 90 sec)</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1109">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1109">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoTable",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureTableOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "AzureTableSink",
                    "writeBatchSize": 100,
                    "writeBatchTimeout": "01:00:00"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
<span data-ttu-id="cc737-1110">További információ a következő összekapcsolt szolgáltatások: [Azure Table Storage összekötő](data-factory-azure-table-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1110">For more information about these linked services, see [Azure Table Storage connector](data-factory-azure-table-connector.md#copy-activity-properties) article.</span></span> 

## <a name="amazon-redshift"></a><span data-ttu-id="cc737-1111">Amazon RedShift</span><span class="sxs-lookup"><span data-stu-id="cc737-1111">Amazon RedShift</span></span>

### <a name="linked-service"></a><span data-ttu-id="cc737-1112">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-1112">Linked service</span></span>
<span data-ttu-id="cc737-1113">Az Amazon Redshift meghatározásához társított szolgáltatás, állítsa be a **típus** a társított szolgáltatás **AmazonRedshift**, és adja meg a következő tulajdonságokat a **typeProperties** szakasz :</span><span class="sxs-lookup"><span data-stu-id="cc737-1113">To define an Amazon Redshift linked service, set the **type** of the linked service to **AmazonRedshift**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="cc737-1114">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1114">Property</span></span> | <span data-ttu-id="cc737-1115">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1115">Description</span></span> | <span data-ttu-id="cc737-1116">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1116">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-1117">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="cc737-1117">server</span></span> |<span data-ttu-id="cc737-1118">Kiszolgáló IP-címét vagy állomásnevét kiszolgálónevét az Amazon Redshift.</span><span class="sxs-lookup"><span data-stu-id="cc737-1118">IP address or host name of the Amazon Redshift server.</span></span> |<span data-ttu-id="cc737-1119">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1119">Yes</span></span> |
| <span data-ttu-id="cc737-1120">port</span><span class="sxs-lookup"><span data-stu-id="cc737-1120">port</span></span> |<span data-ttu-id="cc737-1121">A TCP-portot, amelyen az Amazon Redshift kiszolgáló ügyfélkapcsolatokat száma.</span><span class="sxs-lookup"><span data-stu-id="cc737-1121">The number of the TCP port that the Amazon Redshift server uses to listen for client connections.</span></span> |<span data-ttu-id="cc737-1122">Nem, alapértelmezett érték: 5439</span><span class="sxs-lookup"><span data-stu-id="cc737-1122">No, default value: 5439</span></span> |
| <span data-ttu-id="cc737-1123">adatbázis</span><span class="sxs-lookup"><span data-stu-id="cc737-1123">database</span></span> |<span data-ttu-id="cc737-1124">Az Amazon Redshift adatbázis nevét.</span><span class="sxs-lookup"><span data-stu-id="cc737-1124">Name of the Amazon Redshift database.</span></span> |<span data-ttu-id="cc737-1125">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1125">Yes</span></span> |
| <span data-ttu-id="cc737-1126">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="cc737-1126">username</span></span> |<span data-ttu-id="cc737-1127">Felhasználó, aki hozzáfér az adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1127">Name of user who has access to the database.</span></span> |<span data-ttu-id="cc737-1128">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1128">Yes</span></span> |
| <span data-ttu-id="cc737-1129">jelszó</span><span class="sxs-lookup"><span data-stu-id="cc737-1129">password</span></span> |<span data-ttu-id="cc737-1130">A felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="cc737-1130">Password for the user account.</span></span> |<span data-ttu-id="cc737-1131">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1131">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1132">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1132">Example</span></span>

```json
{
    "name": "AmazonRedshiftLinkedService",
    "properties": {
        "type": "AmazonRedshift",
        "typeProperties": {
            "server": "<Amazon Redshift host name or IP address>",
            "port": 5439,
            "database": "<database name>",
            "username": "user",
            "password": "password"
        }
    }
}
```

<span data-ttu-id="cc737-1133">További információkért lásd: [Amazon Redshift összekötő](#data-factory-amazon-redshift-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1133">For more information, see [Amazon Redshift connector](#data-factory-amazon-redshift-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="cc737-1134">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-1134">Dataset</span></span>
<span data-ttu-id="cc737-1135">Adja meg az Amazon Redshift adatkészlethez, állítsa be a **típus** a DataSet **RelationalTable**, és adja meg az alábbi tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1135">To define an Amazon Redshift dataset, set the **type** of the dataset to **RelationalTable**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="cc737-1136">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1136">Property</span></span> | <span data-ttu-id="cc737-1137">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1137">Description</span></span> | <span data-ttu-id="cc737-1138">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1138">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-1139">tableName</span><span class="sxs-lookup"><span data-stu-id="cc737-1139">tableName</span></span> |<span data-ttu-id="cc737-1140">A tábla az Amazon Redshift adatbázisban, amelyre a társított szolgáltatás neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="cc737-1140">Name of the table in the Amazon Redshift database that linked service refers to.</span></span> |<span data-ttu-id="cc737-1141">Nem (Ha **lekérdezés** a **RelationalSource** van megadva)</span><span class="sxs-lookup"><span data-stu-id="cc737-1141">No (if **query** of **RelationalSource** is specified)</span></span> |


#### <a name="example"></a><span data-ttu-id="cc737-1142">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1142">Example</span></span>

```json
{
    "name": "AmazonRedshiftInputDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "AmazonRedshiftLinkedService",
        "typeProperties": {
            "tableName": "<Table name>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
<span data-ttu-id="cc737-1143">További információkért lásd: [Amazon Redshift összekötő](#data-factory-amazon-redshift-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1143">For more information, see [Amazon Redshift connector](#data-factory-amazon-redshift-connector.md#dataset-properties) article.</span></span>

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="cc737-1144">A másolási tevékenység relációs adatforrás</span><span class="sxs-lookup"><span data-stu-id="cc737-1144">Relational Source in Copy Activity</span></span> 
<span data-ttu-id="cc737-1145">Adatok másolása az Amazon Redshift, állítsa be a **adatforrástípust** a másolási tevékenység **RelationalSource**, és adja meg a következő tulajdonságokat a **forrás** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1145">If you are copying data from Amazon Redshift, set the **source type** of the copy activity to **RelationalSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="cc737-1146">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1146">Property</span></span> | <span data-ttu-id="cc737-1147">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1147">Description</span></span> | <span data-ttu-id="cc737-1148">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-1148">Allowed values</span></span> | <span data-ttu-id="cc737-1149">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1149">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-1150">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="cc737-1150">query</span></span> |<span data-ttu-id="cc737-1151">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="cc737-1151">Use the custom query to read data.</span></span> |<span data-ttu-id="cc737-1152">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="cc737-1152">SQL query string.</span></span> <span data-ttu-id="cc737-1153">Például: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="cc737-1153">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="cc737-1154">Nem (Ha **tableName** a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="cc737-1154">No (if **tableName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1155">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1155">Example</span></span>

```json
{
    "name": "CopyAmazonRedshiftToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "AmazonRedshiftInputDataset"
            }],
            "outputs": [{
                "name": "AzureBlobOutputDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "AmazonRedshiftToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```
<span data-ttu-id="cc737-1156">További információkért lásd: [Amazon Redshift összekötő](#data-factory-amazon-redshift-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1156">For more information, see [Amazon Redshift connector](#data-factory-amazon-redshift-connector.md#copy-activity-properties) article.</span></span>

## <a name="ibm-db2"></a><span data-ttu-id="cc737-1157">IBM DB2</span><span class="sxs-lookup"><span data-stu-id="cc737-1157">IBM DB2</span></span>

### <a name="linked-service"></a><span data-ttu-id="cc737-1158">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-1158">Linked service</span></span>
<span data-ttu-id="cc737-1159">Az IBM DB2 meghatározásához társított szolgáltatás, állítsa be a **típus** a társított szolgáltatás **OnPremisesDB2**, és adja meg a következő tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1159">To define an IBM DB2 linked service, set the **type** of the linked service to **OnPremisesDB2**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="cc737-1160">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1160">Property</span></span> | <span data-ttu-id="cc737-1161">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1161">Description</span></span> | <span data-ttu-id="cc737-1162">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1162">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-1163">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="cc737-1163">server</span></span> |<span data-ttu-id="cc737-1164">A DB2-kiszolgáló neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1164">Name of the DB2 server.</span></span> |<span data-ttu-id="cc737-1165">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1165">Yes</span></span> |
| <span data-ttu-id="cc737-1166">adatbázis</span><span class="sxs-lookup"><span data-stu-id="cc737-1166">database</span></span> |<span data-ttu-id="cc737-1167">Neve a DB2-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="cc737-1167">Name of the DB2 database.</span></span> |<span data-ttu-id="cc737-1168">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1168">Yes</span></span> |
| <span data-ttu-id="cc737-1169">Séma</span><span class="sxs-lookup"><span data-stu-id="cc737-1169">schema</span></span> |<span data-ttu-id="cc737-1170">Az adatbázisban séma neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1170">Name of the schema in the database.</span></span> <span data-ttu-id="cc737-1171">A séma neve a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="cc737-1171">The schema name is case-sensitive.</span></span> |<span data-ttu-id="cc737-1172">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1172">No</span></span> |
| <span data-ttu-id="cc737-1173">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="cc737-1173">authenticationType</span></span> |<span data-ttu-id="cc737-1174">A DB2-adatbázishoz való kapcsolódáshoz használt hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="cc737-1174">Type of authentication used to connect to the DB2 database.</span></span> <span data-ttu-id="cc737-1175">Lehetséges értékek a következők: névtelen, alapszintű és a Windows.</span><span class="sxs-lookup"><span data-stu-id="cc737-1175">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="cc737-1176">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1176">Yes</span></span> |
| <span data-ttu-id="cc737-1177">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="cc737-1177">username</span></span> |<span data-ttu-id="cc737-1178">Adja meg a felhasználónevet Basic vagy Windows-hitelesítés használata.</span><span class="sxs-lookup"><span data-stu-id="cc737-1178">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="cc737-1179">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1179">No</span></span> |
| <span data-ttu-id="cc737-1180">jelszó</span><span class="sxs-lookup"><span data-stu-id="cc737-1180">password</span></span> |<span data-ttu-id="cc737-1181">Adja meg a felhasználónévhez megadott felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="cc737-1181">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="cc737-1182">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1182">No</span></span> |
| <span data-ttu-id="cc737-1183">gatewayName</span><span class="sxs-lookup"><span data-stu-id="cc737-1183">gatewayName</span></span> |<span data-ttu-id="cc737-1184">Az átjáró, amely használatával a Data Factory szolgáltatásnak csatlakoznia a helyszíni DB2-adatbázishoz való kapcsolódáshoz neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1184">Name of the gateway that the Data Factory service should use to connect to the on-premises DB2 database.</span></span> |<span data-ttu-id="cc737-1185">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1185">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1186">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1186">Example</span></span>
```json
{
    "name": "OnPremDb2LinkedService",
    "properties": {
        "type": "OnPremisesDb2",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```
<span data-ttu-id="cc737-1187">További információkért lásd: [IBM DB2-összekötő](#data-factory-onprem-db2-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1187">For more information, see [IBM DB2 connector](#data-factory-onprem-db2-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="cc737-1188">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-1188">Dataset</span></span>
<span data-ttu-id="cc737-1189">Adja meg a DB2 adatkészletet, állítsa be a **típus** a DataSet **RelationalTable**, és adja meg az alábbi tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1189">To define a DB2 dataset, set the **type** of the dataset to **RelationalTable**, and specify the following properties in the **typeProperties** section:</span></span>

| <span data-ttu-id="cc737-1190">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1190">Property</span></span> | <span data-ttu-id="cc737-1191">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1191">Description</span></span> | <span data-ttu-id="cc737-1192">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1192">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-1193">tableName</span><span class="sxs-lookup"><span data-stu-id="cc737-1193">tableName</span></span> |<span data-ttu-id="cc737-1194">A tábla a DB2-adatbázishoz példány, amelyre a társított szolgáltatás neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="cc737-1194">Name of the table in the DB2 Database instance that linked service refers to.</span></span> <span data-ttu-id="cc737-1195">A táblanév a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="cc737-1195">The tableName is case-sensitive.</span></span> |<span data-ttu-id="cc737-1196">Nem (Ha **lekérdezés** a **RelationalSource** van megadva)</span><span class="sxs-lookup"><span data-stu-id="cc737-1196">No (if **query** of **RelationalSource** is specified)</span></span> 

#### <a name="example"></a><span data-ttu-id="cc737-1197">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1197">Example</span></span>
```json
{
    "name": "Db2DataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremDb2LinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="cc737-1198">További információkért lásd: [IBM DB2-összekötő](#data-factory-onprem-db2-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1198">For more information, see [IBM DB2 connector](#data-factory-onprem-db2-connector.md#dataset-properties) article.</span></span>

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="cc737-1199">A másolási tevékenység relációs adatforrás</span><span class="sxs-lookup"><span data-stu-id="cc737-1199">Relational Source in Copy Activity</span></span>
<span data-ttu-id="cc737-1200">Adatok másolása az IBM DB2, állítsa be a **adatforrástípust** a másolási tevékenység **RelationalSource**, és adja meg a következő tulajdonságokat a **forrás** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1200">If you are copying data from IBM DB2, set the **source type** of the copy activity to **RelationalSource**, and specify following properties in the **source** section:</span></span>


| <span data-ttu-id="cc737-1201">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1201">Property</span></span> | <span data-ttu-id="cc737-1202">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1202">Description</span></span> | <span data-ttu-id="cc737-1203">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-1203">Allowed values</span></span> | <span data-ttu-id="cc737-1204">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1204">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-1205">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="cc737-1205">query</span></span> |<span data-ttu-id="cc737-1206">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="cc737-1206">Use the custom query to read data.</span></span> |<span data-ttu-id="cc737-1207">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="cc737-1207">SQL query string.</span></span> <span data-ttu-id="cc737-1208">Például: `"query": "select * from "MySchema"."MyTable""`.</span><span class="sxs-lookup"><span data-stu-id="cc737-1208">For example: `"query": "select * from "MySchema"."MyTable""`.</span></span> |<span data-ttu-id="cc737-1209">Nem (Ha **tableName** a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="cc737-1209">No (if **tableName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1210">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1210">Example</span></span>
```json
{
    "name": "CopyDb2ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "select * from \"Orders\""
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "inputs": [{
                "name": "Db2DataSet"
            }],
            "outputs": [{
                "name": "AzureBlobDb2DataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "Db2ToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```
<span data-ttu-id="cc737-1211">További információkért lásd: [IBM DB2-összekötő](#data-factory-onprem-db2-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1211">For more information, see [IBM DB2 connector](#data-factory-onprem-db2-connector.md#copy-activity-properties) article.</span></span>

## <a name="mysql"></a><span data-ttu-id="cc737-1212">MySQL</span><span class="sxs-lookup"><span data-stu-id="cc737-1212">MySQL</span></span>

### <a name="linked-service"></a><span data-ttu-id="cc737-1213">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-1213">Linked service</span></span>
<span data-ttu-id="cc737-1214">Egy MySQL meghatározásához társított szolgáltatás, állítsa be a **típus** a társított szolgáltatás **OnPremisesMySql**, és adja meg a következő tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1214">To define a MySQL linked service, set the **type** of the linked service to **OnPremisesMySql**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="cc737-1215">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1215">Property</span></span> | <span data-ttu-id="cc737-1216">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1216">Description</span></span> | <span data-ttu-id="cc737-1217">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1217">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-1218">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="cc737-1218">server</span></span> |<span data-ttu-id="cc737-1219">A MySQL-kiszolgáló neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1219">Name of the MySQL server.</span></span> |<span data-ttu-id="cc737-1220">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1220">Yes</span></span> |
| <span data-ttu-id="cc737-1221">adatbázis</span><span class="sxs-lookup"><span data-stu-id="cc737-1221">database</span></span> |<span data-ttu-id="cc737-1222">A MySQL-adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1222">Name of the MySQL database.</span></span> |<span data-ttu-id="cc737-1223">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1223">Yes</span></span> |
| <span data-ttu-id="cc737-1224">Séma</span><span class="sxs-lookup"><span data-stu-id="cc737-1224">schema</span></span> |<span data-ttu-id="cc737-1225">Az adatbázisban séma neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1225">Name of the schema in the database.</span></span> |<span data-ttu-id="cc737-1226">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1226">No</span></span> |
| <span data-ttu-id="cc737-1227">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="cc737-1227">authenticationType</span></span> |<span data-ttu-id="cc737-1228">A MySQL-adatbázishoz való kapcsolódáshoz használt hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="cc737-1228">Type of authentication used to connect to the MySQL database.</span></span> <span data-ttu-id="cc737-1229">Lehetséges értékek a következők: `Basic`.</span><span class="sxs-lookup"><span data-stu-id="cc737-1229">Possible values are: `Basic`.</span></span> |<span data-ttu-id="cc737-1230">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1230">Yes</span></span> |
| <span data-ttu-id="cc737-1231">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="cc737-1231">username</span></span> |<span data-ttu-id="cc737-1232">Adja meg a felhasználónevet a MySQL-adatbázishoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="cc737-1232">Specify user name to connect to the MySQL database.</span></span> |<span data-ttu-id="cc737-1233">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1233">Yes</span></span> |
| <span data-ttu-id="cc737-1234">jelszó</span><span class="sxs-lookup"><span data-stu-id="cc737-1234">password</span></span> |<span data-ttu-id="cc737-1235">Adja meg a megadott felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="cc737-1235">Specify password for the user account you specified.</span></span> |<span data-ttu-id="cc737-1236">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1236">Yes</span></span> |
| <span data-ttu-id="cc737-1237">gatewayName</span><span class="sxs-lookup"><span data-stu-id="cc737-1237">gatewayName</span></span> |<span data-ttu-id="cc737-1238">Az átjáró, amely a Data Factory szolgáltatásnak csatlakoznia való kapcsolódáshoz a helyszíni MySQL-adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1238">Name of the gateway that the Data Factory service should use to connect to the on-premises MySQL database.</span></span> |<span data-ttu-id="cc737-1239">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1239">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1240">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1240">Example</span></span>

```json
{
    "name": "OnPremMySqlLinkedService",
    "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
            "server": "<server name>",
            "database": "<database name>",
            "schema": "<schema name>",
            "authenticationType": "<authentication type>",
            "userName": "<user name>",
            "password": "<password>",
            "gatewayName": "<gateway>"
        }
    }
}
```

<span data-ttu-id="cc737-1241">További információkért lásd: [MySQL összekötő](data-factory-onprem-mysql-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1241">For more information, see [MySQL connector](data-factory-onprem-mysql-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="cc737-1242">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-1242">Dataset</span></span>
<span data-ttu-id="cc737-1243">Adja meg a MySQL adatkészletet, állítsa be a **típus** a DataSet **RelationalTable**, és adja meg az alábbi tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1243">To define a MySQL dataset, set the **type** of the dataset to **RelationalTable**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="cc737-1244">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1244">Property</span></span> | <span data-ttu-id="cc737-1245">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1245">Description</span></span> | <span data-ttu-id="cc737-1246">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1246">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-1247">tableName</span><span class="sxs-lookup"><span data-stu-id="cc737-1247">tableName</span></span> |<span data-ttu-id="cc737-1248">A tábla a MySQL-adatbázis-példány, amelyre a társított szolgáltatás neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="cc737-1248">Name of the table in the MySQL Database instance that linked service refers to.</span></span> |<span data-ttu-id="cc737-1249">Nem (Ha **lekérdezés** a **RelationalSource** van megadva)</span><span class="sxs-lookup"><span data-stu-id="cc737-1249">No (if **query** of **RelationalSource** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1250">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1250">Example</span></span>

```json
{
    "name": "MySqlDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremMySqlLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
<span data-ttu-id="cc737-1251">További információkért lásd: [MySQL összekötő](data-factory-onprem-mysql-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1251">For more information, see [MySQL connector](data-factory-onprem-mysql-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="cc737-1252">A másolási tevékenység relációs adatforrás</span><span class="sxs-lookup"><span data-stu-id="cc737-1252">Relational Source in Copy Activity</span></span>
<span data-ttu-id="cc737-1253">Adatok másolása a MySQL-adatbázis, állítsa be a **adatforrástípust** a másolási tevékenység **RelationalSource**, és adja meg a következő tulajdonságokat a **forrás** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1253">If you are copying data from a MySQL database, set the **source type** of the copy activity to **RelationalSource**, and specify following properties in the **source** section:</span></span>


| <span data-ttu-id="cc737-1254">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1254">Property</span></span> | <span data-ttu-id="cc737-1255">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1255">Description</span></span> | <span data-ttu-id="cc737-1256">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-1256">Allowed values</span></span> | <span data-ttu-id="cc737-1257">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1257">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-1258">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="cc737-1258">query</span></span> |<span data-ttu-id="cc737-1259">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="cc737-1259">Use the custom query to read data.</span></span> |<span data-ttu-id="cc737-1260">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="cc737-1260">SQL query string.</span></span> <span data-ttu-id="cc737-1261">Például: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="cc737-1261">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="cc737-1262">Nem (Ha **tableName** a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="cc737-1262">No (if **tableName** of **dataset** is specified)</span></span> |


#### <a name="example"></a><span data-ttu-id="cc737-1263">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1263">Example</span></span>
```json
{
    "name": "CopyMySqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "MySqlDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobMySqlDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "MySqlToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

<span data-ttu-id="cc737-1264">További információkért lásd: [MySQL összekötő](data-factory-onprem-mysql-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1264">For more information, see [MySQL connector](data-factory-onprem-mysql-connector.md#copy-activity-properties) article.</span></span> 

## <a name="oracle"></a><span data-ttu-id="cc737-1265">Oracle</span><span class="sxs-lookup"><span data-stu-id="cc737-1265">Oracle</span></span> 

### <a name="linked-service"></a><span data-ttu-id="cc737-1266">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-1266">Linked service</span></span>
<span data-ttu-id="cc737-1267">Az Oracle meghatározásához társított szolgáltatás, állítsa be a **típus** a társított szolgáltatás **OnPremisesOracle**, és adja meg a következő tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1267">To define an Oracle linked service, set the **type** of the linked service to **OnPremisesOracle**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="cc737-1268">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1268">Property</span></span> | <span data-ttu-id="cc737-1269">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1269">Description</span></span> | <span data-ttu-id="cc737-1270">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1270">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-1271">driverType</span><span class="sxs-lookup"><span data-stu-id="cc737-1271">driverType</span></span> | <span data-ttu-id="cc737-1272">Adja meg, melyik illesztőprogram használatával másolja az adatokat, vagy Oracle-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="cc737-1272">Specify which driver to use to copy data from/to Oracle Database.</span></span> <span data-ttu-id="cc737-1273">Két érték engedélyezett **Microsoft** vagy **ODP** (alapértelmezett).</span><span class="sxs-lookup"><span data-stu-id="cc737-1273">Allowed values are **Microsoft** or **ODP** (default).</span></span> <span data-ttu-id="cc737-1274">Lásd: [verziójától és a telepítés támogatott](#supported-versions-and-installation) illesztőprogram adatai szakaszban.</span><span class="sxs-lookup"><span data-stu-id="cc737-1274">See [Supported version and installation](#supported-versions-and-installation) section on driver details.</span></span> | <span data-ttu-id="cc737-1275">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1275">No</span></span> |
| <span data-ttu-id="cc737-1276">connectionString</span><span class="sxs-lookup"><span data-stu-id="cc737-1276">connectionString</span></span> | <span data-ttu-id="cc737-1277">Adja meg az Oracle adatbázispéldányt a connectionString tulajdonság való kapcsolódáshoz szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="cc737-1277">Specify information needed to connect to the Oracle Database instance for the connectionString property.</span></span> | <span data-ttu-id="cc737-1278">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1278">Yes</span></span> |
| <span data-ttu-id="cc737-1279">gatewayName</span><span class="sxs-lookup"><span data-stu-id="cc737-1279">gatewayName</span></span> | <span data-ttu-id="cc737-1280">Azon átjáró neve, amely a helyszíni Oracle-kiszolgálóhoz való csatlakozáshoz használt</span><span class="sxs-lookup"><span data-stu-id="cc737-1280">Name of the gateway that that is used to connect to the on-premises Oracle server</span></span> |<span data-ttu-id="cc737-1281">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1281">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1282">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1282">Example</span></span>
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

<span data-ttu-id="cc737-1283">További információkért lásd: [Oracle összekötő](data-factory-onprem-oracle-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1283">For more information, see [Oracle connector](data-factory-onprem-oracle-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="cc737-1284">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-1284">Dataset</span></span>
<span data-ttu-id="cc737-1285">Adja meg az Oracle-adatkészlet, állítsa be a **típus** a DataSet **OracleTable**, és adja meg az alábbi tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1285">To define an Oracle dataset, set the **type** of the dataset to **OracleTable**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="cc737-1286">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1286">Property</span></span> | <span data-ttu-id="cc737-1287">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1287">Description</span></span> | <span data-ttu-id="cc737-1288">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1288">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-1289">tableName</span><span class="sxs-lookup"><span data-stu-id="cc737-1289">tableName</span></span> |<span data-ttu-id="cc737-1290">A tábla az Oracle-adatbázishoz, amely hivatkozik a társított szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1290">Name of the table in the Oracle Database that the linked service refers to.</span></span> |<span data-ttu-id="cc737-1291">Nem (Ha **oracleReaderQuery** a **OracleSource** van megadva)</span><span class="sxs-lookup"><span data-stu-id="cc737-1291">No (if **oracleReaderQuery** of **OracleSource** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1292">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1292">Example</span></span>

```json
{
    "name": "OracleInput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "offset": "01:00:00",
            "interval": "1",
            "anchorDateTime": "2016-02-27T12:00:00",
            "frequency": "Hour"
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
<span data-ttu-id="cc737-1293">További információkért lásd: [Oracle összekötő](data-factory-onprem-oracle-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1293">For more information, see [Oracle connector](data-factory-onprem-oracle-connector.md#dataset-properties) article.</span></span>

### <a name="oracle-source-in-copy-activity"></a><span data-ttu-id="cc737-1294">A másolási tevékenység Oracle-forrás</span><span class="sxs-lookup"><span data-stu-id="cc737-1294">Oracle Source in Copy Activity</span></span>
<span data-ttu-id="cc737-1295">Adatok másolása az Oracle-adatbázishoz, állítsa be a **adatforrástípust** a másolási tevékenység **OracleSource**, és adja meg a következő tulajdonságokat a **forrás** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1295">If you are copying data from an Oracle database, set the **source type** of the copy activity to **OracleSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="cc737-1296">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1296">Property</span></span> | <span data-ttu-id="cc737-1297">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1297">Description</span></span> | <span data-ttu-id="cc737-1298">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-1298">Allowed values</span></span> | <span data-ttu-id="cc737-1299">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1299">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-1300">oracleReaderQuery</span><span class="sxs-lookup"><span data-stu-id="cc737-1300">oracleReaderQuery</span></span> |<span data-ttu-id="cc737-1301">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="cc737-1301">Use the custom query to read data.</span></span> |<span data-ttu-id="cc737-1302">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="cc737-1302">SQL query string.</span></span> <span data-ttu-id="cc737-1303">Például:`select * from MyTable`</span><span class="sxs-lookup"><span data-stu-id="cc737-1303">For example: `select * from MyTable`</span></span> <br/><br/><span data-ttu-id="cc737-1304">Ha nincs megadva, az SQL-utasítást, amely végrehajtja a rendszer:`select * from MyTable`</span><span class="sxs-lookup"><span data-stu-id="cc737-1304">If not specified, the SQL statement that is executed: `select * from MyTable`</span></span> |<span data-ttu-id="cc737-1305">Nem (Ha **tableName** a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="cc737-1305">No (if **tableName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1306">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1306">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "OracletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": " OracleInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "OracleSource",
                    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
            "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="cc737-1307">További információkért lásd: [Oracle összekötő](data-factory-onprem-oracle-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1307">For more information, see [Oracle connector](data-factory-onprem-oracle-connector.md#copy-activity-properties) article.</span></span>

### <a name="oracle-sink-in-copy-activity"></a><span data-ttu-id="cc737-1308">A másolási tevékenység Oracle fogadó</span><span class="sxs-lookup"><span data-stu-id="cc737-1308">Oracle Sink in Copy Activity</span></span>
<span data-ttu-id="cc737-1309">Adatok másolása az Oracle-adatbázishoz am, állítsa be a **típus gyűjtése** a másolási tevékenység **OracleSink**, és adja meg a következő tulajdonságokat a **fogadó** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1309">If you are copying data to am Oracle database, set the **sink type** of the copy activity to **OracleSink**, and specify following properties in the **sink** section:</span></span>

| <span data-ttu-id="cc737-1310">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1310">Property</span></span> | <span data-ttu-id="cc737-1311">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1311">Description</span></span> | <span data-ttu-id="cc737-1312">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-1312">Allowed values</span></span> | <span data-ttu-id="cc737-1313">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1313">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-1314">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="cc737-1314">writeBatchTimeout</span></span> |<span data-ttu-id="cc737-1315">Várakozási idő a kötegelt beszúrási művelet befejezését, mielőtt azt az időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="cc737-1315">Wait time for the batch insert operation to complete before it times out.</span></span> |<span data-ttu-id="cc737-1316">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="cc737-1316">timespan</span></span><br/><br/> <span data-ttu-id="cc737-1317">. Példa: 00:30:00 (30 perc).</span><span class="sxs-lookup"><span data-stu-id="cc737-1317">Example: 00:30:00 (30 minutes).</span></span> |<span data-ttu-id="cc737-1318">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1318">No</span></span> |
| <span data-ttu-id="cc737-1319">WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="cc737-1319">writeBatchSize</span></span> |<span data-ttu-id="cc737-1320">Szúr be az SQL-tábla adatokat, amikor a puffer mérete eléri writeBatchSize.</span><span class="sxs-lookup"><span data-stu-id="cc737-1320">Inserts data into the SQL table when the buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="cc737-1321">Egész szám (sorok száma)</span><span class="sxs-lookup"><span data-stu-id="cc737-1321">Integer (number of rows)</span></span> |<span data-ttu-id="cc737-1322">Nem (alapértelmezett: 100)</span><span class="sxs-lookup"><span data-stu-id="cc737-1322">No (default: 100)</span></span> |
| <span data-ttu-id="cc737-1323">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="cc737-1323">sqlWriterCleanupScript</span></span> |<span data-ttu-id="cc737-1324">Adja meg egy lekérdezést a másolási tevékenység végrehajtása úgy, hogy egy adott szelet adatait.</span><span class="sxs-lookup"><span data-stu-id="cc737-1324">Specify a query for Copy Activity to execute such that data of a specific slice is cleaned up.</span></span> |<span data-ttu-id="cc737-1325">A lekérdezési utasítást.</span><span class="sxs-lookup"><span data-stu-id="cc737-1325">A query statement.</span></span> |<span data-ttu-id="cc737-1326">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1326">No</span></span> |
| <span data-ttu-id="cc737-1327">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="cc737-1327">sliceIdentifierColumnName</span></span> |<span data-ttu-id="cc737-1328">Adja meg a másolási tevékenység során automatikusan létrejön szelet azonosító, amely segítségével távolítja el az adatokat egy adott szelet, amikor futtassa újra a töltse ki az oszlopnevet.</span><span class="sxs-lookup"><span data-stu-id="cc737-1328">Specify column name for Copy Activity to fill with auto generated slice identifier, which is used to clean up data of a specific slice when rerun.</span></span> |<span data-ttu-id="cc737-1329">Egy oszlop binary(32) adattípusú oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1329">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="cc737-1330">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1330">No</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1331">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1331">Example</span></span>
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-05T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoOracle",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "OracleOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "OracleSink"
                }
            },
            "scheduler": {
                "frequency": "Day",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
<span data-ttu-id="cc737-1332">További információkért lásd: [Oracle összekötő](data-factory-onprem-oracle-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1332">For more information, see [Oracle connector](data-factory-onprem-oracle-connector.md#copy-activity-properties) article.</span></span>

## <a name="postgresql"></a><span data-ttu-id="cc737-1333">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="cc737-1333">PostgreSQL</span></span>

### <a name="linked-service"></a><span data-ttu-id="cc737-1334">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-1334">Linked service</span></span>
<span data-ttu-id="cc737-1335">Egy PostgreSQL meghatározásához társított szolgáltatás, állítsa be a **típus** a társított szolgáltatás **OnPremisesPostgreSql**, és adja meg a következő tulajdonságokat a **typeProperties** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1335">To define a PostgreSQL linked service, set the **type** of the linked service to **OnPremisesPostgreSql**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="cc737-1336">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1336">Property</span></span> | <span data-ttu-id="cc737-1337">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1337">Description</span></span> | <span data-ttu-id="cc737-1338">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1338">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-1339">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="cc737-1339">server</span></span> |<span data-ttu-id="cc737-1340">A PostgreSQL-kiszolgáló neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1340">Name of the PostgreSQL server.</span></span> |<span data-ttu-id="cc737-1341">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1341">Yes</span></span> |
| <span data-ttu-id="cc737-1342">adatbázis</span><span class="sxs-lookup"><span data-stu-id="cc737-1342">database</span></span> |<span data-ttu-id="cc737-1343">A PostgreSQL-adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1343">Name of the PostgreSQL database.</span></span> |<span data-ttu-id="cc737-1344">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1344">Yes</span></span> |
| <span data-ttu-id="cc737-1345">Séma</span><span class="sxs-lookup"><span data-stu-id="cc737-1345">schema</span></span> |<span data-ttu-id="cc737-1346">Az adatbázisban séma neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1346">Name of the schema in the database.</span></span> <span data-ttu-id="cc737-1347">A séma neve a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="cc737-1347">The schema name is case-sensitive.</span></span> |<span data-ttu-id="cc737-1348">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1348">No</span></span> |
| <span data-ttu-id="cc737-1349">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="cc737-1349">authenticationType</span></span> |<span data-ttu-id="cc737-1350">A PostgreSQL-adatbázishoz való kapcsolódáshoz használt hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="cc737-1350">Type of authentication used to connect to the PostgreSQL database.</span></span> <span data-ttu-id="cc737-1351">Lehetséges értékek a következők: névtelen, alapszintű és a Windows.</span><span class="sxs-lookup"><span data-stu-id="cc737-1351">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="cc737-1352">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1352">Yes</span></span> |
| <span data-ttu-id="cc737-1353">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="cc737-1353">username</span></span> |<span data-ttu-id="cc737-1354">Adja meg a felhasználónevet Basic vagy Windows-hitelesítés használata.</span><span class="sxs-lookup"><span data-stu-id="cc737-1354">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="cc737-1355">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1355">No</span></span> |
| <span data-ttu-id="cc737-1356">jelszó</span><span class="sxs-lookup"><span data-stu-id="cc737-1356">password</span></span> |<span data-ttu-id="cc737-1357">Adja meg a felhasználónévhez megadott felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="cc737-1357">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="cc737-1358">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1358">No</span></span> |
| <span data-ttu-id="cc737-1359">gatewayName</span><span class="sxs-lookup"><span data-stu-id="cc737-1359">gatewayName</span></span> |<span data-ttu-id="cc737-1360">Az átjáró, amely használatával a Data Factory szolgáltatásnak csatlakoznia a helyszíni PostgreSQL-adatbázishoz való kapcsolódáshoz neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1360">Name of the gateway that the Data Factory service should use to connect to the on-premises PostgreSQL database.</span></span> |<span data-ttu-id="cc737-1361">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1361">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1362">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1362">Example</span></span>

```json
{
    "name": "OnPremPostgreSqlLinkedService",
    "properties": {
        "type": "OnPremisesPostgreSql",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```
<span data-ttu-id="cc737-1363">További információkért lásd: [PostgreSQL összekötő](data-factory-onprem-postgresql-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1363">For more information, see [PostgreSQL connector](data-factory-onprem-postgresql-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="cc737-1364">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-1364">Dataset</span></span>
<span data-ttu-id="cc737-1365">Adja meg egy PostgreSQL-adatkészlet, állítsa be a **típus** a DataSet **RelationalTable**, adja meg a következő tulajdonságokat és a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1365">To define a PostgreSQL dataset, set the **type** of the dataset to **RelationalTable**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="cc737-1366">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1366">Property</span></span> | <span data-ttu-id="cc737-1367">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1367">Description</span></span> | <span data-ttu-id="cc737-1368">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1368">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-1369">tableName</span><span class="sxs-lookup"><span data-stu-id="cc737-1369">tableName</span></span> |<span data-ttu-id="cc737-1370">A tábla PostgreSQL-adatbázispéldány, amelyre a társított szolgáltatás neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="cc737-1370">Name of the table in the PostgreSQL Database instance that linked service refers to.</span></span> <span data-ttu-id="cc737-1371">A táblanév a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="cc737-1371">The tableName is case-sensitive.</span></span> |<span data-ttu-id="cc737-1372">Nem (Ha **lekérdezés** a **RelationalSource** van megadva)</span><span class="sxs-lookup"><span data-stu-id="cc737-1372">No (if **query** of **RelationalSource** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1373">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1373">Example</span></span>
```json
{
    "name": "PostgreSqlDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremPostgreSqlLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
<span data-ttu-id="cc737-1374">További információkért lásd: [PostgreSQL összekötő](data-factory-onprem-postgresql-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1374">For more information, see [PostgreSQL connector](data-factory-onprem-postgresql-connector.md#dataset-properties) article.</span></span>

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="cc737-1375">A másolási tevékenység relációs adatforrás</span><span class="sxs-lookup"><span data-stu-id="cc737-1375">Relational Source in Copy Activity</span></span>
<span data-ttu-id="cc737-1376">Adatok másolása egy PostgreSQL-adatbázisból, állítsa be a **adatforrástípust** a másolási tevékenység **RelationalSource**, és adja meg a következő tulajdonságokat a **forrás** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1376">If you are copying data from a PostgreSQL database, set the **source type** of the copy activity to **RelationalSource**, and specify following properties in the **source** section:</span></span>


| <span data-ttu-id="cc737-1377">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1377">Property</span></span> | <span data-ttu-id="cc737-1378">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1378">Description</span></span> | <span data-ttu-id="cc737-1379">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-1379">Allowed values</span></span> | <span data-ttu-id="cc737-1380">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1380">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-1381">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="cc737-1381">query</span></span> |<span data-ttu-id="cc737-1382">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="cc737-1382">Use the custom query to read data.</span></span> |<span data-ttu-id="cc737-1383">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="cc737-1383">SQL query string.</span></span> <span data-ttu-id="cc737-1384">Például: "query": "Válasszon * a \"MySchema\".\" MyTable\"".</span><span class="sxs-lookup"><span data-stu-id="cc737-1384">For example: "query": "select * from \"MySchema\".\"MyTable\"".</span></span> |<span data-ttu-id="cc737-1385">Nem (Ha **tableName** a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="cc737-1385">No (if **tableName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1386">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1386">Example</span></span>

```json
{
    "name": "CopyPostgreSqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "select * from \"public\".\"usstates\""
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "inputs": [{
                "name": "PostgreSqlDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobPostgreSqlDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "PostgreSqlToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

<span data-ttu-id="cc737-1387">További információkért lásd: [PostgreSQL összekötő](data-factory-onprem-postgresql-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1387">For more information, see [PostgreSQL connector](data-factory-onprem-postgresql-connector.md#copy-activity-properties) article.</span></span>

## <a name="sap-business-warehouse"></a><span data-ttu-id="cc737-1388">SAP Business Warehouse</span><span class="sxs-lookup"><span data-stu-id="cc737-1388">SAP Business Warehouse</span></span>


### <a name="linked-service"></a><span data-ttu-id="cc737-1389">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-1389">Linked service</span></span>
<span data-ttu-id="cc737-1390">Egy SAP Business Warehouse (BW) meghatározásához társított szolgáltatás, állítsa be a **típus** a társított szolgáltatás **SapBw**, és adja meg a következő tulajdonságokat a **typeProperties** szakasz :</span><span class="sxs-lookup"><span data-stu-id="cc737-1390">To define a SAP Business Warehouse (BW) linked service, set the **type** of the linked service to **SapBw**, and specify following properties in the **typeProperties** section:</span></span>  

<span data-ttu-id="cc737-1391">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1391">Property</span></span> | <span data-ttu-id="cc737-1392">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1392">Description</span></span> | <span data-ttu-id="cc737-1393">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-1393">Allowed values</span></span> | <span data-ttu-id="cc737-1394">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1394">Required</span></span>
-------- | ----------- | -------------- | --------
<span data-ttu-id="cc737-1395">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="cc737-1395">server</span></span> | <span data-ttu-id="cc737-1396">A kiszolgálóra az SAP BW-példány neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1396">Name of the server on which the SAP BW instance resides.</span></span> | <span data-ttu-id="cc737-1397">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cc737-1397">string</span></span> | <span data-ttu-id="cc737-1398">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1398">Yes</span></span>
<span data-ttu-id="cc737-1399">systemNumber</span><span class="sxs-lookup"><span data-stu-id="cc737-1399">systemNumber</span></span> | <span data-ttu-id="cc737-1400">Az SAP BW rendszer rendszer száma.</span><span class="sxs-lookup"><span data-stu-id="cc737-1400">System number of the SAP BW system.</span></span> | <span data-ttu-id="cc737-1401">Kétjegyű tizedes tört karakterláncból.</span><span class="sxs-lookup"><span data-stu-id="cc737-1401">Two-digit decimal number represented as a string.</span></span> | <span data-ttu-id="cc737-1402">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1402">Yes</span></span>
<span data-ttu-id="cc737-1403">clientId</span><span class="sxs-lookup"><span data-stu-id="cc737-1403">clientId</span></span> | <span data-ttu-id="cc737-1404">Az ügyfél számára a SAP W rendszer ügyfél-azonosítója.</span><span class="sxs-lookup"><span data-stu-id="cc737-1404">Client ID of the client in the SAP W system.</span></span> | <span data-ttu-id="cc737-1405">Három számjegyből tizedes tört karakterláncból.</span><span class="sxs-lookup"><span data-stu-id="cc737-1405">Three-digit decimal number represented as a string.</span></span> | <span data-ttu-id="cc737-1406">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1406">Yes</span></span>
<span data-ttu-id="cc737-1407">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="cc737-1407">username</span></span> | <span data-ttu-id="cc737-1408">Az SAP-kiszolgálóhoz hozzáféréssel rendelkező felhasználó neve</span><span class="sxs-lookup"><span data-stu-id="cc737-1408">Name of the user who has access to the SAP server</span></span> | <span data-ttu-id="cc737-1409">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cc737-1409">string</span></span> | <span data-ttu-id="cc737-1410">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1410">Yes</span></span>
<span data-ttu-id="cc737-1411">jelszó</span><span class="sxs-lookup"><span data-stu-id="cc737-1411">password</span></span> | <span data-ttu-id="cc737-1412">A felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="cc737-1412">Password for the user.</span></span> | <span data-ttu-id="cc737-1413">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cc737-1413">string</span></span> | <span data-ttu-id="cc737-1414">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1414">Yes</span></span>
<span data-ttu-id="cc737-1415">gatewayName</span><span class="sxs-lookup"><span data-stu-id="cc737-1415">gatewayName</span></span> | <span data-ttu-id="cc737-1416">Az átjáró, amely a Data Factory szolgáltatásnak csatlakoznia a helyszíni SAP BW-példányra neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1416">Name of the gateway that the Data Factory service should use to connect to the on-premises SAP BW instance.</span></span> | <span data-ttu-id="cc737-1417">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cc737-1417">string</span></span> | <span data-ttu-id="cc737-1418">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1418">Yes</span></span>
<span data-ttu-id="cc737-1419">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="cc737-1419">encryptedCredential</span></span> | <span data-ttu-id="cc737-1420">A titkosított hitelesítő adatokban karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="cc737-1420">The encrypted credential string.</span></span> | <span data-ttu-id="cc737-1421">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cc737-1421">string</span></span> | <span data-ttu-id="cc737-1422">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1422">No</span></span>

#### <a name="example"></a><span data-ttu-id="cc737-1423">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1423">Example</span></span>

```json
{
    "name": "SapBwLinkedService",
    "properties": {
        "type": "SapBw",
        "typeProperties": {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

<span data-ttu-id="cc737-1424">További információkért lásd: [SAP Business Warehouse-összekötő](data-factory-sap-business-warehouse-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1424">For more information, see [SAP Business Warehouse connector](data-factory-sap-business-warehouse-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="cc737-1425">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-1425">Dataset</span></span>
<span data-ttu-id="cc737-1426">Adja meg egy SAP BW dataset, állítsa be a **típus** a DataSet **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="cc737-1426">To define a SAP BW dataset, set the **type** of the dataset to **RelationalTable**.</span></span> <span data-ttu-id="cc737-1427">Nincsenek az SAP BW adatkészlet típusú támogatott típusra vonatkozó tulajdonságok **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="cc737-1427">There are no type-specific properties supported for the SAP BW dataset of type **RelationalTable**.</span></span>  

#### <a name="example"></a><span data-ttu-id="cc737-1428">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1428">Example</span></span>

```json
{
    "name": "SapBwDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapBwLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
<span data-ttu-id="cc737-1429">További információkért lásd: [SAP Business Warehouse-összekötő](data-factory-sap-business-warehouse-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1429">For more information, see [SAP Business Warehouse connector](data-factory-sap-business-warehouse-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="cc737-1430">A másolási tevékenység relációs adatforrás</span><span class="sxs-lookup"><span data-stu-id="cc737-1430">Relational Source in Copy Activity</span></span>
<span data-ttu-id="cc737-1431">Adatok másolása az SAP Business Warehouse, állítsa be a **adatforrástípust** a másolási tevékenység **RelationalSource**, és adja meg a következő tulajdonságokat a **forrás** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1431">If you are copying data from SAP Business Warehouse, set the **source type** of the copy activity to **RelationalSource**, and specify following properties in the **source** section:</span></span>


| <span data-ttu-id="cc737-1432">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1432">Property</span></span> | <span data-ttu-id="cc737-1433">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1433">Description</span></span> | <span data-ttu-id="cc737-1434">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-1434">Allowed values</span></span> | <span data-ttu-id="cc737-1435">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1435">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-1436">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="cc737-1436">query</span></span> | <span data-ttu-id="cc737-1437">Meghatározza az MDX-lekérdezés adatokat olvasni az SAP BW-példány.</span><span class="sxs-lookup"><span data-stu-id="cc737-1437">Specifies the MDX query to read data from the SAP BW instance.</span></span> | <span data-ttu-id="cc737-1438">MDX-lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="cc737-1438">MDX query.</span></span> | <span data-ttu-id="cc737-1439">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1439">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1440">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1440">Example</span></span>

```json
{
    "name": "CopySapBwToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "<MDX query for SAP BW>"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "SapBwDataset"
            }],
            "outputs": [{
                "name": "AzureBlobDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "SapBwToBlob"
        }],
        "start": "2017-03-01T18:00:00",
        "end": "2017-03-01T19:00:00"
    }
}
```

<span data-ttu-id="cc737-1441">További információkért lásd: [SAP Business Warehouse-összekötő](data-factory-sap-business-warehouse-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1441">For more information, see [SAP Business Warehouse connector](data-factory-sap-business-warehouse-connector.md#copy-activity-properties) article.</span></span> 

## <a name="sap-hana"></a><span data-ttu-id="cc737-1442">SAP HANA</span><span class="sxs-lookup"><span data-stu-id="cc737-1442">SAP HANA</span></span>

### <a name="linked-service"></a><span data-ttu-id="cc737-1443">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-1443">Linked service</span></span>
<span data-ttu-id="cc737-1444">Egy SAP HANA meghatározásához társított szolgáltatás, állítsa be a **típus** a társított szolgáltatás **SapHana**, és adja meg a következő tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1444">To define a SAP HANA linked service, set the **type** of the linked service to **SapHana**, and specify following properties in the **typeProperties** section:</span></span>  

<span data-ttu-id="cc737-1445">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1445">Property</span></span> | <span data-ttu-id="cc737-1446">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1446">Description</span></span> | <span data-ttu-id="cc737-1447">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-1447">Allowed values</span></span> | <span data-ttu-id="cc737-1448">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1448">Required</span></span>
-------- | ----------- | -------------- | --------
<span data-ttu-id="cc737-1449">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="cc737-1449">server</span></span> | <span data-ttu-id="cc737-1450">A kiszolgálóra az SAP HANA-példány neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1450">Name of the server on which the SAP HANA instance resides.</span></span> <span data-ttu-id="cc737-1451">Ha a kiszolgáló egy testreszabott portot használ, adja meg a `server:port`.</span><span class="sxs-lookup"><span data-stu-id="cc737-1451">If your server is using a customized port, specify `server:port`.</span></span> | <span data-ttu-id="cc737-1452">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cc737-1452">string</span></span> | <span data-ttu-id="cc737-1453">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1453">Yes</span></span>
<span data-ttu-id="cc737-1454">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="cc737-1454">authenticationType</span></span> | <span data-ttu-id="cc737-1455">Hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="cc737-1455">Type of authentication.</span></span> | <span data-ttu-id="cc737-1456">Karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="cc737-1456">string.</span></span> <span data-ttu-id="cc737-1457">"Basic" vagy "Windows"</span><span class="sxs-lookup"><span data-stu-id="cc737-1457">"Basic" or "Windows"</span></span> | <span data-ttu-id="cc737-1458">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1458">Yes</span></span> 
<span data-ttu-id="cc737-1459">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="cc737-1459">username</span></span> | <span data-ttu-id="cc737-1460">Az SAP-kiszolgálóhoz hozzáféréssel rendelkező felhasználó neve</span><span class="sxs-lookup"><span data-stu-id="cc737-1460">Name of the user who has access to the SAP server</span></span> | <span data-ttu-id="cc737-1461">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cc737-1461">string</span></span> | <span data-ttu-id="cc737-1462">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1462">Yes</span></span>
<span data-ttu-id="cc737-1463">jelszó</span><span class="sxs-lookup"><span data-stu-id="cc737-1463">password</span></span> | <span data-ttu-id="cc737-1464">A felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="cc737-1464">Password for the user.</span></span> | <span data-ttu-id="cc737-1465">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cc737-1465">string</span></span> | <span data-ttu-id="cc737-1466">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1466">Yes</span></span>
<span data-ttu-id="cc737-1467">gatewayName</span><span class="sxs-lookup"><span data-stu-id="cc737-1467">gatewayName</span></span> | <span data-ttu-id="cc737-1468">Az átjáró, amely a Data Factory szolgáltatásnak csatlakoznia való kapcsolódáshoz a helyszíni SAP HANA-példány neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1468">Name of the gateway that the Data Factory service should use to connect to the on-premises SAP HANA instance.</span></span> | <span data-ttu-id="cc737-1469">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cc737-1469">string</span></span> | <span data-ttu-id="cc737-1470">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1470">Yes</span></span>
<span data-ttu-id="cc737-1471">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="cc737-1471">encryptedCredential</span></span> | <span data-ttu-id="cc737-1472">A titkosított hitelesítő adatokban karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="cc737-1472">The encrypted credential string.</span></span> | <span data-ttu-id="cc737-1473">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cc737-1473">string</span></span> | <span data-ttu-id="cc737-1474">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1474">No</span></span>

#### <a name="example"></a><span data-ttu-id="cc737-1475">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1475">Example</span></span>

```json
{
    "name": "SapHanaLinkedService",
    "properties": {
        "type": "SapHana",
        "typeProperties": {
            "server": "<server name>",
            "authenticationType": "<Basic, or Windows>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}

```
<span data-ttu-id="cc737-1476">További információkért lásd: [SAP HANA-összekötő](data-factory-sap-hana-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1476">For more information, see [SAP HANA connector](data-factory-sap-hana-connector.md#linked-service-properties) article.</span></span>
 
### <a name="dataset"></a><span data-ttu-id="cc737-1477">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-1477">Dataset</span></span>
<span data-ttu-id="cc737-1478">Adja meg egy SAP HANA-adatkészlet, állítsa be a **típus** a DataSet **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="cc737-1478">To define a SAP HANA dataset, set the **type** of the dataset to **RelationalTable**.</span></span> <span data-ttu-id="cc737-1479">Nincsenek az SAP HANA-adatkészlet típusú támogatott típusra vonatkozó tulajdonságok **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="cc737-1479">There are no type-specific properties supported for the SAP HANA dataset of type **RelationalTable**.</span></span> 

#### <a name="example"></a><span data-ttu-id="cc737-1480">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1480">Example</span></span>

```json
{
    "name": "SapHanaDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapHanaLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
<span data-ttu-id="cc737-1481">További információkért lásd: [SAP HANA-összekötő](data-factory-sap-hana-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1481">For more information, see [SAP HANA connector](data-factory-sap-hana-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="cc737-1482">A másolási tevékenység relációs adatforrás</span><span class="sxs-lookup"><span data-stu-id="cc737-1482">Relational Source in Copy Activity</span></span>
<span data-ttu-id="cc737-1483">Adatok másolása egy SAP HANA adattárból, állítsa be a **adatforrástípust** a másolási tevékenység **RelationalSource**, és adja meg a következő tulajdonságokat a **forrás** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1483">If you are copying data from a SAP HANA data store, set the **source type** of the copy activity to **RelationalSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="cc737-1484">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1484">Property</span></span> | <span data-ttu-id="cc737-1485">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1485">Description</span></span> | <span data-ttu-id="cc737-1486">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-1486">Allowed values</span></span> | <span data-ttu-id="cc737-1487">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1487">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-1488">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="cc737-1488">query</span></span> | <span data-ttu-id="cc737-1489">Adja meg az adatokat olvasni az SAP HANA-példány az SQL-lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="cc737-1489">Specifies the SQL query to read data from the SAP HANA instance.</span></span> | <span data-ttu-id="cc737-1490">SQL-lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="cc737-1490">SQL query.</span></span> | <span data-ttu-id="cc737-1491">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1491">Yes</span></span> |


#### <a name="example"></a><span data-ttu-id="cc737-1492">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1492">Example</span></span>


```json
{
    "name": "CopySapHanaToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "<SQL Query for HANA>"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "SapHanaDataset"
            }],
            "outputs": [{
                "name": "AzureBlobDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "SapHanaToBlob"
        }],
        "start": "2017-03-01T18:00:00",
        "end": "2017-03-01T19:00:00"
    }
}
```

<span data-ttu-id="cc737-1493">További információkért lásd: [SAP HANA-összekötő](data-factory-sap-hana-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1493">For more information, see [SAP HANA connector](data-factory-sap-hana-connector.md#copy-activity-properties) article.</span></span>


## <a name="sql-server"></a><span data-ttu-id="cc737-1494">SQL Server</span><span class="sxs-lookup"><span data-stu-id="cc737-1494">SQL Server</span></span>

### <a name="linked-service"></a><span data-ttu-id="cc737-1495">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-1495">Linked service</span></span>
<span data-ttu-id="cc737-1496">Típusú társított szolgáltatás létrehozása **OnPremisesSqlServer** a helyszíni SQL Server adatbázis összekapcsolása egy adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="cc737-1496">You create a linked service of type **OnPremisesSqlServer** to link an on-premises SQL Server database to a data factory.</span></span> <span data-ttu-id="cc737-1497">A következő táblázat a JSON-elemek szerepelnek a helyszíni SQL Server kapcsolódó szolgáltatásra vonatkozó leírást.</span><span class="sxs-lookup"><span data-stu-id="cc737-1497">The following table provides description for JSON elements specific to on-premises SQL Server linked service.</span></span>

<span data-ttu-id="cc737-1498">A következő táblázat a JSON-elemek szerepelnek kapcsolódó SQL Server-szolgáltatásra vonatkozó leírást.</span><span class="sxs-lookup"><span data-stu-id="cc737-1498">The following table provides description for JSON elements specific to SQL Server linked service.</span></span>

| <span data-ttu-id="cc737-1499">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1499">Property</span></span> | <span data-ttu-id="cc737-1500">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1500">Description</span></span> | <span data-ttu-id="cc737-1501">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1501">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-1502">type</span><span class="sxs-lookup"><span data-stu-id="cc737-1502">type</span></span> |<span data-ttu-id="cc737-1503">A type tulajdonságot kell megadni: **OnPremisesSqlServer**.</span><span class="sxs-lookup"><span data-stu-id="cc737-1503">The type property should be set to: **OnPremisesSqlServer**.</span></span> |<span data-ttu-id="cc737-1504">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1504">Yes</span></span> |
| <span data-ttu-id="cc737-1505">connectionString</span><span class="sxs-lookup"><span data-stu-id="cc737-1505">connectionString</span></span> |<span data-ttu-id="cc737-1506">Az SQL-hitelesítéssel vagy a Windows-hitelesítés a helyszíni SQL Server adatbázishoz való kapcsolódáshoz szükséges connectionString információkat adják meg.</span><span class="sxs-lookup"><span data-stu-id="cc737-1506">Specify connectionString information needed to connect to the on-premises SQL Server database using either SQL authentication or Windows authentication.</span></span> |<span data-ttu-id="cc737-1507">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1507">Yes</span></span> |
| <span data-ttu-id="cc737-1508">gatewayName</span><span class="sxs-lookup"><span data-stu-id="cc737-1508">gatewayName</span></span> |<span data-ttu-id="cc737-1509">Neve az átjáró, amely a Data Factory szolgáltatásnak csatlakoznia kell a helyszíni SQL Server adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="cc737-1509">Name of the gateway that the Data Factory service should use to connect to the on-premises SQL Server database.</span></span> |<span data-ttu-id="cc737-1510">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1510">Yes</span></span> |
| <span data-ttu-id="cc737-1511">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="cc737-1511">username</span></span> |<span data-ttu-id="cc737-1512">Adja meg a felhasználónevet, ha a Windows-hitelesítést használ.</span><span class="sxs-lookup"><span data-stu-id="cc737-1512">Specify user name if you are using Windows Authentication.</span></span> <span data-ttu-id="cc737-1513">Példa: **tartománynév\\felhasználónév**.</span><span class="sxs-lookup"><span data-stu-id="cc737-1513">Example: **domainname\\username**.</span></span> |<span data-ttu-id="cc737-1514">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1514">No</span></span> |
| <span data-ttu-id="cc737-1515">jelszó</span><span class="sxs-lookup"><span data-stu-id="cc737-1515">password</span></span> |<span data-ttu-id="cc737-1516">Adja meg a felhasználónévhez megadott felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="cc737-1516">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="cc737-1517">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1517">No</span></span> |

<span data-ttu-id="cc737-1518">Hitelesítő adatok használatával titkosíthatja az **New-AzureRmDataFactoryEncryptValue** parancsmag és a következő példában látható módon használhatja őket a kapcsolódási karakterláncban (**EncryptedCredential** tulajdonság):</span><span class="sxs-lookup"><span data-stu-id="cc737-1518">You can encrypt credentials using the **New-AzureRmDataFactoryEncryptValue** cmdlet and use them in the connection string as shown in the following example (**EncryptedCredential** property):</span></span>  

```json
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```


#### <a name="example-json-for-using-sql-authentication"></a><span data-ttu-id="cc737-1519">Példa: JSON az SQL-hitelesítéssel</span><span class="sxs-lookup"><span data-stu-id="cc737-1519">Example: JSON for using SQL Authentication</span></span>

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
#### <a name="example-json-for-using-windows-authentication"></a><span data-ttu-id="cc737-1520">Példa: JSON a Windows-hitelesítés használatával</span><span class="sxs-lookup"><span data-stu-id="cc737-1520">Example: JSON for using Windows Authentication</span></span>

<span data-ttu-id="cc737-1521">Ha a felhasználónév és jelszó van adva, átjáró őket a megszemélyesítéshez használ a megadott felhasználói fióknak a helyi SQL Server-adatbázishoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="cc737-1521">If username and password are specified, gateway uses them to impersonate the specified user account to connect to the on-premises SQL Server database.</span></span> <span data-ttu-id="cc737-1522">Ellenkező esetben átjáró csatlakozik az SQL Server közvetlenül az átjáró (az indítási fiókjához) biztonsági környezetében.</span><span class="sxs-lookup"><span data-stu-id="cc737-1522">Otherwise, gateway connects to the SQL Server directly with the security context of Gateway (its startup account).</span></span>

```json
{
    "Name": " MyOnPremisesSQLDB",
    "Properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
            "username": "<domain\\username>",
            "password": "<password>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

<span data-ttu-id="cc737-1523">További információkért lásd: [SQL Server-összekötő](data-factory-sqlserver-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1523">For more information, see [SQL Server connector](data-factory-sqlserver-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="cc737-1524">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-1524">Dataset</span></span>
<span data-ttu-id="cc737-1525">Adja meg az SQL Server dataset, állítsa be a **típus** a DataSet **SqlServerTable**, és adja meg az alábbi tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1525">To define a SQL Server dataset, set the **type** of the dataset to **SqlServerTable**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="cc737-1526">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1526">Property</span></span> | <span data-ttu-id="cc737-1527">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1527">Description</span></span> | <span data-ttu-id="cc737-1528">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1528">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-1529">tableName</span><span class="sxs-lookup"><span data-stu-id="cc737-1529">tableName</span></span> |<span data-ttu-id="cc737-1530">A tábla vagy nézet, amelyre a társított szolgáltatás SQL Server adatbázis-példány neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="cc737-1530">Name of the table or view in the SQL Server Database instance that linked service refers to.</span></span> |<span data-ttu-id="cc737-1531">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1531">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1532">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1532">Example</span></span>
```json
{
    "name": "SqlServerInput",
    "properties": {
        "type": "SqlServerTable",
        "linkedServiceName": "SqlServerLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="cc737-1533">További információkért lásd: [SQL Server-összekötő](data-factory-sqlserver-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1533">For more information, see [SQL Server connector](data-factory-sqlserver-connector.md#dataset-properties) article.</span></span> 

### <a name="sql-source-in-copy-activity"></a><span data-ttu-id="cc737-1534">A másolási tevékenység SQL-forrás</span><span class="sxs-lookup"><span data-stu-id="cc737-1534">Sql Source in Copy Activity</span></span>
<span data-ttu-id="cc737-1535">Adatok másolása egy SQL Server-adatbázisból, állítsa be a **adatforrástípust** a másolási tevékenység **SqlSource**, és adja meg a következő tulajdonságokat a **forrás** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1535">If you are copying data from a SQL Server database, set the **source type** of the copy activity to **SqlSource**, and specify following properties in the **source** section:</span></span>


| <span data-ttu-id="cc737-1536">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1536">Property</span></span> | <span data-ttu-id="cc737-1537">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1537">Description</span></span> | <span data-ttu-id="cc737-1538">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-1538">Allowed values</span></span> | <span data-ttu-id="cc737-1539">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1539">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-1540">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="cc737-1540">sqlReaderQuery</span></span> |<span data-ttu-id="cc737-1541">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="cc737-1541">Use the custom query to read data.</span></span> |<span data-ttu-id="cc737-1542">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="cc737-1542">SQL query string.</span></span> <span data-ttu-id="cc737-1543">Például: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="cc737-1543">For example: `select * from MyTable`.</span></span> <span data-ttu-id="cc737-1544">Előfordulhat, hogy a bemeneti adatkészlet által hivatkozott adatbázishoz több táblát is hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="cc737-1544">May reference multiple tables from the database referenced by the input dataset.</span></span> <span data-ttu-id="cc737-1545">Ha nincs megadva, az SQL-utasítás végrehajtott: táblanév kiválaszthatja.</span><span class="sxs-lookup"><span data-stu-id="cc737-1545">If not specified, the SQL statement that is executed: select from MyTable.</span></span> |<span data-ttu-id="cc737-1546">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1546">No</span></span> |
| <span data-ttu-id="cc737-1547">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="cc737-1547">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="cc737-1548">A tárolt eljárás, amely adatokat olvas a forrástábla neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1548">Name of the stored procedure that reads data from the source table.</span></span> |<span data-ttu-id="cc737-1549">A tárolt eljárás neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1549">Name of the stored procedure.</span></span> |<span data-ttu-id="cc737-1550">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1550">No</span></span> |
| <span data-ttu-id="cc737-1551">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="cc737-1551">storedProcedureParameters</span></span> |<span data-ttu-id="cc737-1552">A tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="cc737-1552">Parameters for the stored procedure.</span></span> |<span data-ttu-id="cc737-1553">A név/érték párok.</span><span class="sxs-lookup"><span data-stu-id="cc737-1553">Name/value pairs.</span></span> <span data-ttu-id="cc737-1554">Nevét és a kis-és a paraméterek meg kell egyeznie a nevek és a kis-és nagybetűhasználat a tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="cc737-1554">Names and casing of parameters must match the names and casing of the stored procedure parameters.</span></span> |<span data-ttu-id="cc737-1555">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1555">No</span></span> |

<span data-ttu-id="cc737-1556">Ha a **sqlReaderQuery** van megadva a SqlSource, a másolási tevékenység során ez a lekérdezés fut az SQL Server-adatbázis forrás az adatok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="cc737-1556">If the **sqlReaderQuery** is specified for the SqlSource, the Copy Activity runs this query against the SQL Server Database source to get the data.</span></span>

<span data-ttu-id="cc737-1557">Másik lehetőségként megadhat tárolt eljárás megadásával a **sqlReaderStoredProcedureName** és **storedProcedureParameters** (Ha a tárolt eljárás paraméterek fogadja el).</span><span class="sxs-lookup"><span data-stu-id="cc737-1557">Alternatively, you can specify a stored procedure by specifying the **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if the stored procedure takes parameters).</span></span>

<span data-ttu-id="cc737-1558">Ha nem ad meg sqlReaderQuery vagy sqlReaderStoredProcedureName, struktúra szakaszában meghatározott oszlopokat válassza futtatni az SQL Server adatbázis-lekérdezés összeállításához használt.</span><span class="sxs-lookup"><span data-stu-id="cc737-1558">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, the columns defined in the structure section are used to build a select query to run against the SQL Server Database.</span></span> <span data-ttu-id="cc737-1559">Az adatkészlet-definícióban nem rendelkezik a struktúra, ha minden kiválasztott oszlop. a táblából.</span><span class="sxs-lookup"><span data-stu-id="cc737-1559">If the dataset definition does not have the structure, all columns are selected from the table.</span></span>

> [!NOTE]
> <span data-ttu-id="cc737-1560">Amikor **sqlReaderStoredProcedureName**, továbbra is meg kell adnia egy értéket a **tableName** az adatkészlet JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="cc737-1560">When you use **sqlReaderStoredProcedureName**, you still need to specify a value for the **tableName** property in the dataset JSON.</span></span> <span data-ttu-id="cc737-1561">Nincs érvényesítést hajt végre ezt a táblázatot, ha van.</span><span class="sxs-lookup"><span data-stu-id="cc737-1561">There are no validations performed against this table though.</span></span>


#### <a name="example"></a><span data-ttu-id="cc737-1562">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1562">Example</span></span>
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "SqlServertoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": " SqlServerInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="cc737-1563">Ebben a példában **sqlReaderQuery** a SqlSource van megadva.</span><span class="sxs-lookup"><span data-stu-id="cc737-1563">In this example, **sqlReaderQuery** is specified for the SqlSource.</span></span> <span data-ttu-id="cc737-1564">A másolási tevékenység során ez a lekérdezés fut az SQL Server-adatbázis forrás az adatok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="cc737-1564">The Copy Activity runs this query against the SQL Server Database source to get the data.</span></span> <span data-ttu-id="cc737-1565">Másik lehetőségként megadhat tárolt eljárás megadásával a **sqlReaderStoredProcedureName** és **storedProcedureParameters** (Ha a tárolt eljárás paraméterek fogadja el).</span><span class="sxs-lookup"><span data-stu-id="cc737-1565">Alternatively, you can specify a stored procedure by specifying the **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if the stored procedure takes parameters).</span></span> <span data-ttu-id="cc737-1566">A sqlReaderQuery hivatkozhat több táblák az adatbázisban a következő bemeneti adatkészlet hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="cc737-1566">The sqlReaderQuery can reference multiple tables within the database referenced by the input dataset.</span></span> <span data-ttu-id="cc737-1567">Nincs korlátozva csak a tábla a dataset tableName typeProperty állítja be.</span><span class="sxs-lookup"><span data-stu-id="cc737-1567">It is not limited to only the table set as the dataset's tableName typeProperty.</span></span>

<span data-ttu-id="cc737-1568">Ha nem ad meg sqlReaderQuery vagy sqlReaderStoredProcedureName, struktúra szakaszában meghatározott oszlopokat válassza futtatni az SQL Server adatbázis-lekérdezés összeállításához használt.</span><span class="sxs-lookup"><span data-stu-id="cc737-1568">If you do not specify sqlReaderQuery or sqlReaderStoredProcedureName, the columns defined in the structure section are used to build a select query to run against the SQL Server Database.</span></span> <span data-ttu-id="cc737-1569">Az adatkészlet-definícióban nem rendelkezik a struktúra, ha minden kiválasztott oszlop. a táblából.</span><span class="sxs-lookup"><span data-stu-id="cc737-1569">If the dataset definition does not have the structure, all columns are selected from the table.</span></span>

<span data-ttu-id="cc737-1570">További információkért lásd: [SQL Server-összekötő](data-factory-sqlserver-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1570">For more information, see [SQL Server connector](data-factory-sqlserver-connector.md#copy-activity-properties) article.</span></span> 

### <a name="sql-sink-in-copy-activity"></a><span data-ttu-id="cc737-1571">A másolási tevékenység SQL fogadó</span><span class="sxs-lookup"><span data-stu-id="cc737-1571">Sql Sink in Copy Activity</span></span>
<span data-ttu-id="cc737-1572">Adatok másolása az SQL Server-adatbázishoz, állítsa be a **típus gyűjtése** a másolási tevékenység **SqlSink**, és adja meg a következő tulajdonságokat a **fogadó** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1572">If you are copying data to a SQL Server database, set the **sink type** of the copy activity to **SqlSink**, and specify following properties in the **sink** section:</span></span>

| <span data-ttu-id="cc737-1573">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1573">Property</span></span> | <span data-ttu-id="cc737-1574">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1574">Description</span></span> | <span data-ttu-id="cc737-1575">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-1575">Allowed values</span></span> | <span data-ttu-id="cc737-1576">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1576">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-1577">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="cc737-1577">writeBatchTimeout</span></span> |<span data-ttu-id="cc737-1578">Várakozási idő a kötegelt beszúrási művelet befejezését, mielőtt azt az időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="cc737-1578">Wait time for the batch insert operation to complete before it times out.</span></span> |<span data-ttu-id="cc737-1579">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="cc737-1579">timespan</span></span><br/><br/> <span data-ttu-id="cc737-1580">Példa: "00: 30:00" (30 perc).</span><span class="sxs-lookup"><span data-stu-id="cc737-1580">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="cc737-1581">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1581">No</span></span> |
| <span data-ttu-id="cc737-1582">WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="cc737-1582">writeBatchSize</span></span> |<span data-ttu-id="cc737-1583">Szúr be az SQL-tábla adatokat, amikor a puffer mérete eléri writeBatchSize.</span><span class="sxs-lookup"><span data-stu-id="cc737-1583">Inserts data into the SQL table when the buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="cc737-1584">Egész szám (sorok száma)</span><span class="sxs-lookup"><span data-stu-id="cc737-1584">Integer (number of rows)</span></span> |<span data-ttu-id="cc737-1585">Nem (alapértelmezett: 10000)</span><span class="sxs-lookup"><span data-stu-id="cc737-1585">No (default: 10000)</span></span> |
| <span data-ttu-id="cc737-1586">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="cc737-1586">sqlWriterCleanupScript</span></span> |<span data-ttu-id="cc737-1587">Adja meg a lekérdezést úgy, hogy egy adott szelet adatait végrehajtásához másolási tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="cc737-1587">Specify query for Copy Activity to execute such that data of a specific slice is cleaned up.</span></span> <span data-ttu-id="cc737-1588">További információkért lásd: [ismételhetőség](#repeatability-during-copy) szakasz.</span><span class="sxs-lookup"><span data-stu-id="cc737-1588">For more information, see [repeatability](#repeatability-during-copy) section.</span></span> |<span data-ttu-id="cc737-1589">A lekérdezési utasítást.</span><span class="sxs-lookup"><span data-stu-id="cc737-1589">A query statement.</span></span> |<span data-ttu-id="cc737-1590">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1590">No</span></span> |
| <span data-ttu-id="cc737-1591">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="cc737-1591">sliceIdentifierColumnName</span></span> |<span data-ttu-id="cc737-1592">Adja meg a másolási tevékenység során automatikusan létrejön szelet azonosító, amely segítségével távolítja el az adatokat egy adott szelet, amikor futtassa újra a töltse ki az oszlopnevet.</span><span class="sxs-lookup"><span data-stu-id="cc737-1592">Specify column name for Copy Activity to fill with auto generated slice identifier, which is used to clean up data of a specific slice when rerun.</span></span> <span data-ttu-id="cc737-1593">További információkért lásd: [ismételhetőség](#repeatability-during-copy) szakasz.</span><span class="sxs-lookup"><span data-stu-id="cc737-1593">For more information, see [repeatability](#repeatability-during-copy) section.</span></span> |<span data-ttu-id="cc737-1594">Egy oszlop binary(32) adattípusú oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1594">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="cc737-1595">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1595">No</span></span> |
| <span data-ttu-id="cc737-1596">sqlWriterStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="cc737-1596">sqlWriterStoredProcedureName</span></span> |<span data-ttu-id="cc737-1597">A tárolt eljárás neve a cél táblázatba upserts (frissítés/Beszúrás) adatok.</span><span class="sxs-lookup"><span data-stu-id="cc737-1597">Name of the stored procedure that upserts (updates/inserts) data into the target table.</span></span> |<span data-ttu-id="cc737-1598">A tárolt eljárás neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1598">Name of the stored procedure.</span></span> |<span data-ttu-id="cc737-1599">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1599">No</span></span> |
| <span data-ttu-id="cc737-1600">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="cc737-1600">storedProcedureParameters</span></span> |<span data-ttu-id="cc737-1601">A tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="cc737-1601">Parameters for the stored procedure.</span></span> |<span data-ttu-id="cc737-1602">A név/érték párok.</span><span class="sxs-lookup"><span data-stu-id="cc737-1602">Name/value pairs.</span></span> <span data-ttu-id="cc737-1603">Nevét és a kis-és a paraméterek meg kell egyeznie a nevek és a kis-és nagybetűhasználat a tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="cc737-1603">Names and casing of parameters must match the names and casing of the stored procedure parameters.</span></span> |<span data-ttu-id="cc737-1604">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1604">No</span></span> |
| <span data-ttu-id="cc737-1605">sqlWriterTableType</span><span class="sxs-lookup"><span data-stu-id="cc737-1605">sqlWriterTableType</span></span> |<span data-ttu-id="cc737-1606">Adja meg a tárolt eljárásban használandó tábla neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1606">Specify table type name to be used in the stored procedure.</span></span> <span data-ttu-id="cc737-1607">Másolási tevékenység elérhetővé teszi az adatok áthelyezése egy ideiglenes táblát, amely a táblatípus.</span><span class="sxs-lookup"><span data-stu-id="cc737-1607">Copy activity makes the data being moved available in a temp table with this table type.</span></span> <span data-ttu-id="cc737-1608">Tárolt eljárás kódot is majd egyesítheti az adatokat, a meglévő adatok másolásának.</span><span class="sxs-lookup"><span data-stu-id="cc737-1608">Stored procedure code can then merge the data being copied with existing data.</span></span> |<span data-ttu-id="cc737-1609">Egy tábla környezettípus nevét.</span><span class="sxs-lookup"><span data-stu-id="cc737-1609">A table type name.</span></span> |<span data-ttu-id="cc737-1610">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1610">No</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1611">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1611">Example</span></span>
<span data-ttu-id="cc737-1612">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="cc737-1612">The pipeline contains a Copy Activity that is configured to use these input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="cc737-1613">Az adatcsatorna JSON-definícióból a **forrás** típusúra **BlobSource** és **fogadó** típusúra **SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="cc737-1613">In the pipeline JSON definition, the **source** type is set to **BlobSource** and **sink** type is set to **SqlSink**.</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": " SqlServerOutput "
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "blobColumnSeparators": ","
                },
                "sink": {
                    "type": "SqlSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="cc737-1614">További információkért lásd: [SQL Server-összekötő](data-factory-sqlserver-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1614">For more information, see [SQL Server connector](data-factory-sqlserver-connector.md#copy-activity-properties) article.</span></span> 

## <a name="sybase"></a><span data-ttu-id="cc737-1615">Sybase</span><span class="sxs-lookup"><span data-stu-id="cc737-1615">Sybase</span></span>

### <a name="linked-service"></a><span data-ttu-id="cc737-1616">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-1616">Linked service</span></span>
<span data-ttu-id="cc737-1617">Adja meg a Sybase társított a szolgáltatás, állítsa be a **típus** a társított szolgáltatás **OnPremisesSybase**, és adja meg a következő tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1617">To define a Sybase linked service, set the **type** of the linked service to **OnPremisesSybase**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="cc737-1618">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1618">Property</span></span> | <span data-ttu-id="cc737-1619">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1619">Description</span></span> | <span data-ttu-id="cc737-1620">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1620">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-1621">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="cc737-1621">server</span></span> |<span data-ttu-id="cc737-1622">A Sybase-kiszolgáló neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1622">Name of the Sybase server.</span></span> |<span data-ttu-id="cc737-1623">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1623">Yes</span></span> |
| <span data-ttu-id="cc737-1624">adatbázis</span><span class="sxs-lookup"><span data-stu-id="cc737-1624">database</span></span> |<span data-ttu-id="cc737-1625">Neve a Sybase-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="cc737-1625">Name of the Sybase database.</span></span> |<span data-ttu-id="cc737-1626">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1626">Yes</span></span> |
| <span data-ttu-id="cc737-1627">Séma</span><span class="sxs-lookup"><span data-stu-id="cc737-1627">schema</span></span> |<span data-ttu-id="cc737-1628">Az adatbázisban séma neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1628">Name of the schema in the database.</span></span> |<span data-ttu-id="cc737-1629">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1629">No</span></span> |
| <span data-ttu-id="cc737-1630">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="cc737-1630">authenticationType</span></span> |<span data-ttu-id="cc737-1631">A Sybase-adatbázishoz való kapcsolódáshoz használt hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="cc737-1631">Type of authentication used to connect to the Sybase database.</span></span> <span data-ttu-id="cc737-1632">Lehetséges értékek a következők: névtelen, alapszintű és a Windows.</span><span class="sxs-lookup"><span data-stu-id="cc737-1632">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="cc737-1633">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1633">Yes</span></span> |
| <span data-ttu-id="cc737-1634">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="cc737-1634">username</span></span> |<span data-ttu-id="cc737-1635">Adja meg a felhasználónevet Basic vagy Windows-hitelesítés használata.</span><span class="sxs-lookup"><span data-stu-id="cc737-1635">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="cc737-1636">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1636">No</span></span> |
| <span data-ttu-id="cc737-1637">jelszó</span><span class="sxs-lookup"><span data-stu-id="cc737-1637">password</span></span> |<span data-ttu-id="cc737-1638">Adja meg a felhasználónévhez megadott felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="cc737-1638">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="cc737-1639">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1639">No</span></span> |
| <span data-ttu-id="cc737-1640">gatewayName</span><span class="sxs-lookup"><span data-stu-id="cc737-1640">gatewayName</span></span> |<span data-ttu-id="cc737-1641">Az átjáró, amely használatával a Data Factory szolgáltatásnak csatlakoznia a helyszíni Sybase-adatbázishoz való kapcsolódáshoz neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1641">Name of the gateway that the Data Factory service should use to connect to the on-premises Sybase database.</span></span> |<span data-ttu-id="cc737-1642">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1642">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1643">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1643">Example</span></span>
```json
{
    "name": "OnPremSybaseLinkedService",
    "properties": {
        "type": "OnPremisesSybase",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```

<span data-ttu-id="cc737-1644">További információkért lásd: [Sybase összekötő](data-factory-onprem-sybase-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1644">For more information, see [Sybase connector](data-factory-onprem-sybase-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="cc737-1645">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-1645">Dataset</span></span>
<span data-ttu-id="cc737-1646">Adja meg a Sybase adatkészletet, állítsa be a **típus** a DataSet **RelationalTable**, és adja meg az alábbi tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1646">To define a Sybase dataset, set the **type** of the dataset to **RelationalTable**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="cc737-1647">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1647">Property</span></span> | <span data-ttu-id="cc737-1648">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1648">Description</span></span> | <span data-ttu-id="cc737-1649">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1649">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-1650">tableName</span><span class="sxs-lookup"><span data-stu-id="cc737-1650">tableName</span></span> |<span data-ttu-id="cc737-1651">A tábla a Sybase-adatbázishoz példány, amelyre a társított szolgáltatás neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="cc737-1651">Name of the table in the Sybase Database instance that linked service refers to.</span></span> |<span data-ttu-id="cc737-1652">Nem (Ha **lekérdezés** a **RelationalSource** van megadva)</span><span class="sxs-lookup"><span data-stu-id="cc737-1652">No (if **query** of **RelationalSource** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1653">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1653">Example</span></span>

```json
{
    "name": "SybaseDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremSybaseLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="cc737-1654">További információkért lásd: [Sybase összekötő](data-factory-onprem-sybase-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1654">For more information, see [Sybase connector](data-factory-onprem-sybase-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="cc737-1655">A másolási tevékenység relációs adatforrás</span><span class="sxs-lookup"><span data-stu-id="cc737-1655">Relational Source in Copy Activity</span></span>
<span data-ttu-id="cc737-1656">Adatok másolása egy Sybase-adatbázisból, állítsa be a **adatforrástípust** a másolási tevékenység **RelationalSource**, és adja meg a következő tulajdonságokat a **forrás** szakasz :</span><span class="sxs-lookup"><span data-stu-id="cc737-1656">If you are copying data from a Sybase database, set the **source type** of the copy activity to **RelationalSource**, and specify following properties in the **source** section:</span></span>


| <span data-ttu-id="cc737-1657">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1657">Property</span></span> | <span data-ttu-id="cc737-1658">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1658">Description</span></span> | <span data-ttu-id="cc737-1659">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-1659">Allowed values</span></span> | <span data-ttu-id="cc737-1660">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1660">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-1661">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="cc737-1661">query</span></span> |<span data-ttu-id="cc737-1662">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="cc737-1662">Use the custom query to read data.</span></span> |<span data-ttu-id="cc737-1663">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="cc737-1663">SQL query string.</span></span> <span data-ttu-id="cc737-1664">Például: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="cc737-1664">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="cc737-1665">Nem (Ha **tableName** a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="cc737-1665">No (if **tableName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1666">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1666">Example</span></span>

```json
{
    "name": "CopySybaseToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "select * from DBA.Orders"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "inputs": [{
                "name": "SybaseDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobSybaseDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "SybaseToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

<span data-ttu-id="cc737-1667">További információkért lásd: [Sybase összekötő](data-factory-onprem-sybase-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1667">For more information, see [Sybase connector](data-factory-onprem-sybase-connector.md#copy-activity-properties) article.</span></span>

## <a name="teradata"></a><span data-ttu-id="cc737-1668">Teradata</span><span class="sxs-lookup"><span data-stu-id="cc737-1668">Teradata</span></span>

### <a name="linked-service"></a><span data-ttu-id="cc737-1669">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-1669">Linked service</span></span>
<span data-ttu-id="cc737-1670">Adja meg a teradata rendszerhez társított a szolgáltatás, állítsa be a **típus** a társított szolgáltatás **OnPremisesTeradata**, és adja meg a következő tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1670">To define a Teradata linked service, set the **type** of the linked service to **OnPremisesTeradata**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="cc737-1671">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1671">Property</span></span> | <span data-ttu-id="cc737-1672">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1672">Description</span></span> | <span data-ttu-id="cc737-1673">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1673">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-1674">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="cc737-1674">server</span></span> |<span data-ttu-id="cc737-1675">A Teradata-kiszolgáló neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1675">Name of the Teradata server.</span></span> |<span data-ttu-id="cc737-1676">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1676">Yes</span></span> |
| <span data-ttu-id="cc737-1677">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="cc737-1677">authenticationType</span></span> |<span data-ttu-id="cc737-1678">A Teradata-adatbázishoz való kapcsolódáshoz használt hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="cc737-1678">Type of authentication used to connect to the Teradata database.</span></span> <span data-ttu-id="cc737-1679">Lehetséges értékek a következők: névtelen, alapszintű és a Windows.</span><span class="sxs-lookup"><span data-stu-id="cc737-1679">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="cc737-1680">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1680">Yes</span></span> |
| <span data-ttu-id="cc737-1681">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="cc737-1681">username</span></span> |<span data-ttu-id="cc737-1682">Adja meg a felhasználónevet Basic vagy Windows-hitelesítés használata.</span><span class="sxs-lookup"><span data-stu-id="cc737-1682">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="cc737-1683">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1683">No</span></span> |
| <span data-ttu-id="cc737-1684">jelszó</span><span class="sxs-lookup"><span data-stu-id="cc737-1684">password</span></span> |<span data-ttu-id="cc737-1685">Adja meg a felhasználónévhez megadott felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="cc737-1685">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="cc737-1686">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1686">No</span></span> |
| <span data-ttu-id="cc737-1687">gatewayName</span><span class="sxs-lookup"><span data-stu-id="cc737-1687">gatewayName</span></span> |<span data-ttu-id="cc737-1688">Az átjáró, amely használatával a Data Factory szolgáltatásnak csatlakoznia a helyszíni Teradata-adatbázishoz való kapcsolódáshoz neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1688">Name of the gateway that the Data Factory service should use to connect to the on-premises Teradata database.</span></span> |<span data-ttu-id="cc737-1689">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1689">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1690">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1690">Example</span></span>
```json
{
    "name": "OnPremTeradataLinkedService",
    "properties": {
        "type": "OnPremisesTeradata",
        "typeProperties": {
            "server": "<server>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```

<span data-ttu-id="cc737-1691">További információkért lásd: [Teradata összekötő](data-factory-onprem-teradata-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1691">For more information, see [Teradata connector](data-factory-onprem-teradata-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="cc737-1692">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-1692">Dataset</span></span>
<span data-ttu-id="cc737-1693">Adja meg a Teradata-Blob adatkészletet, állítsa be a **típus** a DataSet **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="cc737-1693">To define a Teradata Blob dataset, set the **type** of the dataset to **RelationalTable**.</span></span> <span data-ttu-id="cc737-1694">Jelenleg nem támogatott a Teradata-adatkészlet tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="cc737-1694">Currently, there are no type properties supported for the Teradata dataset.</span></span> 

#### <a name="example"></a><span data-ttu-id="cc737-1695">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1695">Example</span></span>
```json
{
    "name": "TeradataDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremTeradataLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="cc737-1696">További információkért lásd: [Teradata összekötő](data-factory-onprem-teradata-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1696">For more information, see [Teradata connector](data-factory-onprem-teradata-connector.md#dataset-properties) article.</span></span>

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="cc737-1697">A másolási tevékenység relációs adatforrás</span><span class="sxs-lookup"><span data-stu-id="cc737-1697">Relational Source in Copy Activity</span></span>
<span data-ttu-id="cc737-1698">Adatok másolása egy Teradata-adatbázisból, állítsa be a **adatforrástípust** a másolási tevékenység **RelationalSource**, és adja meg a következő tulajdonságokat a **forrás** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1698">If you are copying data from a Teradata database, set the **source type** of the copy activity to **RelationalSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="cc737-1699">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1699">Property</span></span> | <span data-ttu-id="cc737-1700">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1700">Description</span></span> | <span data-ttu-id="cc737-1701">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-1701">Allowed values</span></span> | <span data-ttu-id="cc737-1702">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1702">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-1703">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="cc737-1703">query</span></span> |<span data-ttu-id="cc737-1704">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="cc737-1704">Use the custom query to read data.</span></span> |<span data-ttu-id="cc737-1705">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="cc737-1705">SQL query string.</span></span> <span data-ttu-id="cc737-1706">Például: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="cc737-1706">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="cc737-1707">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1707">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1708">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1708">Example</span></span>

```json
{
    "name": "CopyTeradataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', SliceStart, SliceEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "TeradataDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobTeradataDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "TeradataToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "isPaused": false
    }
}
```

<span data-ttu-id="cc737-1709">További információkért lásd: [Teradata összekötő](data-factory-onprem-teradata-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1709">For more information, see [Teradata connector](data-factory-onprem-teradata-connector.md#copy-activity-properties) article.</span></span>

## <a name="cassandra"></a><span data-ttu-id="cc737-1710">Cassandra</span><span class="sxs-lookup"><span data-stu-id="cc737-1710">Cassandra</span></span>


### <a name="linked-service"></a><span data-ttu-id="cc737-1711">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-1711">Linked service</span></span>
<span data-ttu-id="cc737-1712">Kapcsolódó Cassandra szolgáltatás definiálásához, állítsa be a **típus** a társított szolgáltatás **OnPremisesCassandra**, és adja meg a következő tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1712">To define a Cassandra linked service, set the **type** of the linked service to **OnPremisesCassandra**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="cc737-1713">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1713">Property</span></span> | <span data-ttu-id="cc737-1714">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1714">Description</span></span> | <span data-ttu-id="cc737-1715">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1715">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-1716">állomás</span><span class="sxs-lookup"><span data-stu-id="cc737-1716">host</span></span> |<span data-ttu-id="cc737-1717">Egy vagy több IP-címek vagy Cassandra kiszolgálók állomás nevét.</span><span class="sxs-lookup"><span data-stu-id="cc737-1717">One or more IP addresses or host names of Cassandra servers.</span></span><br/><br/><span data-ttu-id="cc737-1718">IP-címek vagy állomásnevek kiszolgálókhoz való kapcsolódáshoz összes egyidejűleg vesszővel tagolt listáját adja meg.</span><span class="sxs-lookup"><span data-stu-id="cc737-1718">Specify a comma-separated list of IP addresses or host names to connect to all servers concurrently.</span></span> |<span data-ttu-id="cc737-1719">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1719">Yes</span></span> |
| <span data-ttu-id="cc737-1720">port</span><span class="sxs-lookup"><span data-stu-id="cc737-1720">port</span></span> |<span data-ttu-id="cc737-1721">A TCP-portot, amelyen a Cassandra kiszolgáló ügyfélkapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="cc737-1721">The TCP port that the Cassandra server uses to listen for client connections.</span></span> |<span data-ttu-id="cc737-1722">Nem, alapértelmezett érték: 9042</span><span class="sxs-lookup"><span data-stu-id="cc737-1722">No, default value: 9042</span></span> |
| <span data-ttu-id="cc737-1723">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="cc737-1723">authenticationType</span></span> |<span data-ttu-id="cc737-1724">Basic vagy Anonymous</span><span class="sxs-lookup"><span data-stu-id="cc737-1724">Basic, or Anonymous</span></span> |<span data-ttu-id="cc737-1725">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1725">Yes</span></span> |
| <span data-ttu-id="cc737-1726">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="cc737-1726">username</span></span> |<span data-ttu-id="cc737-1727">Adja meg a felhasználói fiók felhasználónevét.</span><span class="sxs-lookup"><span data-stu-id="cc737-1727">Specify user name for the user account.</span></span> |<span data-ttu-id="cc737-1728">Igen, ha authenticationType beállítása alapszintű.</span><span class="sxs-lookup"><span data-stu-id="cc737-1728">Yes, if authenticationType is set to Basic.</span></span> |
| <span data-ttu-id="cc737-1729">jelszó</span><span class="sxs-lookup"><span data-stu-id="cc737-1729">password</span></span> |<span data-ttu-id="cc737-1730">Adja meg a felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="cc737-1730">Specify password for the user account.</span></span> |<span data-ttu-id="cc737-1731">Igen, ha authenticationType beállítása alapszintű.</span><span class="sxs-lookup"><span data-stu-id="cc737-1731">Yes, if authenticationType is set to Basic.</span></span> |
| <span data-ttu-id="cc737-1732">gatewayName</span><span class="sxs-lookup"><span data-stu-id="cc737-1732">gatewayName</span></span> |<span data-ttu-id="cc737-1733">A helyszíni Cassandra adatbázishoz való csatlakozáshoz használt átjáró neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1733">The name of the gateway that is used to connect to the on-premises Cassandra database.</span></span> |<span data-ttu-id="cc737-1734">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1734">Yes</span></span> |
| <span data-ttu-id="cc737-1735">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="cc737-1735">encryptedCredential</span></span> |<span data-ttu-id="cc737-1736">Az átjáró által titkosított hitelesítő.</span><span class="sxs-lookup"><span data-stu-id="cc737-1736">Credential encrypted by the gateway.</span></span> |<span data-ttu-id="cc737-1737">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1737">No</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1738">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1738">Example</span></span>

```json
{
    "name": "CassandraLinkedService",
    "properties": {
        "type": "OnPremisesCassandra",
        "typeProperties": {
            "authenticationType": "Basic",
            "host": "<cassandra server name or IP address>",
            "port": 9042,
            "username": "user",
            "password": "password",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

<span data-ttu-id="cc737-1739">További információkért lásd: [Cassandra összekötő](data-factory-onprem-cassandra-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1739">For more information, see [Cassandra connector](data-factory-onprem-cassandra-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="cc737-1740">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-1740">Dataset</span></span>
<span data-ttu-id="cc737-1741">Cassandra dataset határozza meg, állítsa be a **típus** a DataSet **CassandraTable**, és adja meg az alábbi tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1741">To define a Cassandra dataset, set the **type** of the dataset to **CassandraTable**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="cc737-1742">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1742">Property</span></span> | <span data-ttu-id="cc737-1743">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1743">Description</span></span> | <span data-ttu-id="cc737-1744">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1744">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-1745">kulcstérértesítések használatával</span><span class="sxs-lookup"><span data-stu-id="cc737-1745">keyspace</span></span> |<span data-ttu-id="cc737-1746">Kulcstérértesítések használatával vagy séma Cassandra adatbázis nevét.</span><span class="sxs-lookup"><span data-stu-id="cc737-1746">Name of the keyspace or schema in Cassandra database.</span></span> |<span data-ttu-id="cc737-1747">Igen (Ha **lekérdezés** a **CassandraSource** nincs definiálva).</span><span class="sxs-lookup"><span data-stu-id="cc737-1747">Yes (If **query** for **CassandraSource** is not defined).</span></span> |
| <span data-ttu-id="cc737-1748">tableName</span><span class="sxs-lookup"><span data-stu-id="cc737-1748">tableName</span></span> |<span data-ttu-id="cc737-1749">A tábla Cassandra adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1749">Name of the table in Cassandra database.</span></span> |<span data-ttu-id="cc737-1750">Igen (Ha **lekérdezés** a **CassandraSource** nincs definiálva).</span><span class="sxs-lookup"><span data-stu-id="cc737-1750">Yes (If **query** for **CassandraSource** is not defined).</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1751">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1751">Example</span></span>

```json
{
    "name": "CassandraInput",
    "properties": {
        "linkedServiceName": "CassandraLinkedService",
        "type": "CassandraTable",
        "typeProperties": {
            "tableName": "mytable",
            "keySpace": "<key space>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="cc737-1752">További információkért lásd: [Cassandra összekötő](data-factory-onprem-cassandra-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1752">For more information, see [Cassandra connector](data-factory-onprem-cassandra-connector.md#dataset-properties) article.</span></span> 

### <a name="cassandra-source-in-copy-activity"></a><span data-ttu-id="cc737-1753">A másolási tevékenység Cassandra forrás</span><span class="sxs-lookup"><span data-stu-id="cc737-1753">Cassandra Source in Copy Activity</span></span>
<span data-ttu-id="cc737-1754">Adatok másolása az Cassandra, állítsa be a **adatforrástípust** a másolási tevékenység **CassandraSource**, és adja meg a következő tulajdonságokat a **forrás** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1754">If you are copying data from Cassandra, set the **source type** of the copy activity to **CassandraSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="cc737-1755">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1755">Property</span></span> | <span data-ttu-id="cc737-1756">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1756">Description</span></span> | <span data-ttu-id="cc737-1757">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-1757">Allowed values</span></span> | <span data-ttu-id="cc737-1758">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1758">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-1759">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="cc737-1759">query</span></span> |<span data-ttu-id="cc737-1760">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="cc737-1760">Use the custom query to read data.</span></span> |<span data-ttu-id="cc737-1761">SQL-92 vagy CQL lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="cc737-1761">SQL-92 query or CQL query.</span></span> <span data-ttu-id="cc737-1762">Lásd: [CQL hivatkozás](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html).</span><span class="sxs-lookup"><span data-stu-id="cc737-1762">See [CQL reference](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html).</span></span> <br/><br/><span data-ttu-id="cc737-1763">SQL-lekérdezés használata esetén adja meg a **kulcstérértesítések használatával name.table neve** a lekérdezni kívánt táblázat képviseli.</span><span class="sxs-lookup"><span data-stu-id="cc737-1763">When using SQL query, specify **keyspace name.table name** to represent the table you want to query.</span></span> |<span data-ttu-id="cc737-1764">Nem (ha van megadva a tableName és a dataset kulcstérértesítések használatával).</span><span class="sxs-lookup"><span data-stu-id="cc737-1764">No (if tableName and keyspace on dataset are defined).</span></span> |
| <span data-ttu-id="cc737-1765">consistencyLevel</span><span class="sxs-lookup"><span data-stu-id="cc737-1765">consistencyLevel</span></span> |<span data-ttu-id="cc737-1766">A konzisztencia szint határozza meg, hány replikák adatok visszatér az ügyfélalkalmazás egy olvasási kérést kell válaszolnia.</span><span class="sxs-lookup"><span data-stu-id="cc737-1766">The consistency level specifies how many replicas must respond to a read request before returning data to the client application.</span></span> <span data-ttu-id="cc737-1767">Cassandra ellenőrzi a megadott számú replikákat az adatok a olvasási kérelem teljesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="cc737-1767">Cassandra checks the specified number of replicas for data to satisfy the read request.</span></span> |<span data-ttu-id="cc737-1768">EGY, KETTŐ, HÁROM, KVÓRUM, AZ ÖSSZES, LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE.</span><span class="sxs-lookup"><span data-stu-id="cc737-1768">ONE, TWO, THREE, QUORUM, ALL, LOCAL_QUORUM, EACH_QUORUM, LOCAL_ONE.</span></span> <span data-ttu-id="cc737-1769">Lásd: [konfigurálása az adatok konzisztenciájának](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="cc737-1769">See [Configuring data consistency](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) for details.</span></span> |<span data-ttu-id="cc737-1770">Nem.</span><span class="sxs-lookup"><span data-stu-id="cc737-1770">No.</span></span> <span data-ttu-id="cc737-1771">Alapértelmezett érték: egyet.</span><span class="sxs-lookup"><span data-stu-id="cc737-1771">Default value is ONE.</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1772">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1772">Example</span></span>
  
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "CassandraToAzureBlob",
            "description": "Copy from Cassandra to an Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "CassandraInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "CassandraSource",
                    "query": "select id, firstname, lastname from mykeyspace.mytable"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="cc737-1773">További információkért lásd: [Cassandra összekötő](data-factory-onprem-cassandra-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-1773">For more information, see [Cassandra connector](data-factory-onprem-cassandra-connector.md#copy-activity-properties) article.</span></span>

## <a name="mongodb"></a><span data-ttu-id="cc737-1774">MongoDB</span><span class="sxs-lookup"><span data-stu-id="cc737-1774">MongoDB</span></span>

### <a name="linked-service"></a><span data-ttu-id="cc737-1775">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-1775">Linked service</span></span>
<span data-ttu-id="cc737-1776">A MongoDB meghatározásához társított szolgáltatás, állítsa be a **típus** a társított szolgáltatás **OnPremisesMongoDB**, és adja meg a következő tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1776">To define a MongoDB linked service, set the **type** of the linked service to **OnPremisesMongoDB**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="cc737-1777">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1777">Property</span></span> | <span data-ttu-id="cc737-1778">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1778">Description</span></span> | <span data-ttu-id="cc737-1779">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1779">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-1780">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="cc737-1780">server</span></span> |<span data-ttu-id="cc737-1781">Kiszolgáló IP-címét vagy állomásnevét kiszolgálónevét a mongodb-Protokolltámogatással.</span><span class="sxs-lookup"><span data-stu-id="cc737-1781">IP address or host name of the MongoDB server.</span></span> |<span data-ttu-id="cc737-1782">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1782">Yes</span></span> |
| <span data-ttu-id="cc737-1783">port</span><span class="sxs-lookup"><span data-stu-id="cc737-1783">port</span></span> |<span data-ttu-id="cc737-1784">A MongoDB-kiszolgálóhoz a kapcsolatok figyelésére használt TCP portot.</span><span class="sxs-lookup"><span data-stu-id="cc737-1784">TCP port that the MongoDB server uses to listen for client connections.</span></span> |<span data-ttu-id="cc737-1785">Nem kötelező, alapértelmezett érték: 27017</span><span class="sxs-lookup"><span data-stu-id="cc737-1785">Optional, default value: 27017</span></span> |
| <span data-ttu-id="cc737-1786">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="cc737-1786">authenticationType</span></span> |<span data-ttu-id="cc737-1787">Alapszintű, vagy névtelen.</span><span class="sxs-lookup"><span data-stu-id="cc737-1787">Basic, or Anonymous.</span></span> |<span data-ttu-id="cc737-1788">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1788">Yes</span></span> |
| <span data-ttu-id="cc737-1789">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="cc737-1789">username</span></span> |<span data-ttu-id="cc737-1790">Felhasználói fiók MongoDB eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="cc737-1790">User account to access MongoDB.</span></span> |<span data-ttu-id="cc737-1791">Igen (Ha alapszintű hitelesítést használ).</span><span class="sxs-lookup"><span data-stu-id="cc737-1791">Yes (if basic authentication is used).</span></span> |
| <span data-ttu-id="cc737-1792">jelszó</span><span class="sxs-lookup"><span data-stu-id="cc737-1792">password</span></span> |<span data-ttu-id="cc737-1793">A felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="cc737-1793">Password for the user.</span></span> |<span data-ttu-id="cc737-1794">Igen (Ha alapszintű hitelesítést használ).</span><span class="sxs-lookup"><span data-stu-id="cc737-1794">Yes (if basic authentication is used).</span></span> |
| <span data-ttu-id="cc737-1795">authSource</span><span class="sxs-lookup"><span data-stu-id="cc737-1795">authSource</span></span> |<span data-ttu-id="cc737-1796">A MongoDB-adatbázist, amely a hitelesítő adatok kereséséhez használni kívánt nevét.</span><span class="sxs-lookup"><span data-stu-id="cc737-1796">Name of the MongoDB database that you want to use to check your credentials for authentication.</span></span> |<span data-ttu-id="cc737-1797">Választható (Ha alapszintű hitelesítést használ).</span><span class="sxs-lookup"><span data-stu-id="cc737-1797">Optional (if basic authentication is used).</span></span> <span data-ttu-id="cc737-1798">alapértelmezett: a rendszergazdai fiókot és a databaseName tulajdonsággal megadott adatbázis használ.</span><span class="sxs-lookup"><span data-stu-id="cc737-1798">default: uses the admin account and the database specified using databaseName property.</span></span> |
| <span data-ttu-id="cc737-1799">DatabaseName</span><span class="sxs-lookup"><span data-stu-id="cc737-1799">databaseName</span></span> |<span data-ttu-id="cc737-1800">A MongoDB-adatbázist, amely az elérni kívánt nevét.</span><span class="sxs-lookup"><span data-stu-id="cc737-1800">Name of the MongoDB database that you want to access.</span></span> |<span data-ttu-id="cc737-1801">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1801">Yes</span></span> |
| <span data-ttu-id="cc737-1802">gatewayName</span><span class="sxs-lookup"><span data-stu-id="cc737-1802">gatewayName</span></span> |<span data-ttu-id="cc737-1803">Az átjáró, aki hozzáfér az adattár neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1803">Name of the gateway that accesses the data store.</span></span> |<span data-ttu-id="cc737-1804">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1804">Yes</span></span> |
| <span data-ttu-id="cc737-1805">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="cc737-1805">encryptedCredential</span></span> |<span data-ttu-id="cc737-1806">A hitelesítőadat-átjáró által titkosított.</span><span class="sxs-lookup"><span data-stu-id="cc737-1806">Credential encrypted by gateway.</span></span> |<span data-ttu-id="cc737-1807">Optional</span><span class="sxs-lookup"><span data-stu-id="cc737-1807">Optional</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1808">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1808">Example</span></span>

```json
{
    "name": "OnPremisesMongoDbLinkedService",
    "properties": {
        "type": "OnPremisesMongoDb",
        "typeProperties": {
            "authenticationType": "<Basic or Anonymous>",
            "server": "< The IP address or host name of the MongoDB server >",
            "port": "<The number of the TCP port that the MongoDB server uses to listen for client connections.>",
            "username": "<username>",
            "password": "<password>",
            "authSource": "< The database that you want to use to check your credentials for authentication. >",
            "databaseName": "<database name>",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

<span data-ttu-id="cc737-1809">További információkért lásd: [MongoDB összekötő cikk](data-factory-on-premises-mongodb-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="cc737-1809">For more information, see [MongoDB connector article](data-factory-on-premises-mongodb-connector.md#linked-service-properties)</span></span>

### <a name="dataset"></a><span data-ttu-id="cc737-1810">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-1810">Dataset</span></span>
<span data-ttu-id="cc737-1811">Adja meg a MongoDB adatkészletet, állítsa be a **típus** a DataSet **MongoDbCollection**, és adja meg az alábbi tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1811">To define a MongoDB dataset, set the **type** of the dataset to **MongoDbCollection**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="cc737-1812">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1812">Property</span></span> | <span data-ttu-id="cc737-1813">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1813">Description</span></span> | <span data-ttu-id="cc737-1814">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1814">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-1815">CollectionName</span><span class="sxs-lookup"><span data-stu-id="cc737-1815">collectionName</span></span> |<span data-ttu-id="cc737-1816">A MongoDB-adatbázist a gyűjtemény nevét.</span><span class="sxs-lookup"><span data-stu-id="cc737-1816">Name of the collection in MongoDB database.</span></span> |<span data-ttu-id="cc737-1817">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1817">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1818">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1818">Example</span></span>

```json
{
    "name": "MongoDbInputDataset",
    "properties": {
        "type": "MongoDbCollection",
        "linkedServiceName": "OnPremisesMongoDbLinkedService",
        "typeProperties": {
            "collectionName": "<Collection name>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

<span data-ttu-id="cc737-1819">További információkért lásd: [MongoDB összekötő cikk](data-factory-on-premises-mongodb-connector.md#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="cc737-1819">For more information, see [MongoDB connector article](data-factory-on-premises-mongodb-connector.md#dataset-properties)</span></span>

#### <a name="mongodb-source-in-copy-activity"></a><span data-ttu-id="cc737-1820">A másolási tevékenység MongoDB-forrás</span><span class="sxs-lookup"><span data-stu-id="cc737-1820">MongoDB Source in Copy Activity</span></span>
<span data-ttu-id="cc737-1821">Adatok másolása a MongoDB, állítsa be a **adatforrástípust** a másolási tevékenység **MongoDbSource**, és adja meg a következő tulajdonságokat a **forrás** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1821">If you are copying data from MongoDB, set the **source type** of the copy activity to **MongoDbSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="cc737-1822">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1822">Property</span></span> | <span data-ttu-id="cc737-1823">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1823">Description</span></span> | <span data-ttu-id="cc737-1824">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-1824">Allowed values</span></span> | <span data-ttu-id="cc737-1825">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1825">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-1826">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="cc737-1826">query</span></span> |<span data-ttu-id="cc737-1827">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="cc737-1827">Use the custom query to read data.</span></span> |<span data-ttu-id="cc737-1828">SQL-92 lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="cc737-1828">SQL-92 query string.</span></span> <span data-ttu-id="cc737-1829">Például: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="cc737-1829">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="cc737-1830">Nem (Ha **collectionName** a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="cc737-1830">No (if **collectionName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1831">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1831">Example</span></span>

```json
{
    "name": "CopyMongoDBToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "MongoDbSource",
                    "query": "select * from MyTable"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "MongoDbInputDataset"
            }],
            "outputs": [{
                "name": "AzureBlobOutputDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "MongoDBToAzureBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

<span data-ttu-id="cc737-1832">További információkért lásd: [MongoDB összekötő cikk](data-factory-on-premises-mongodb-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="cc737-1832">For more information, see [MongoDB connector article](data-factory-on-premises-mongodb-connector.md#copy-activity-properties)</span></span>

## <a name="amazon-s3"></a><span data-ttu-id="cc737-1833">Amazon S3</span><span class="sxs-lookup"><span data-stu-id="cc737-1833">Amazon S3</span></span>


### <a name="linked-service"></a><span data-ttu-id="cc737-1834">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-1834">Linked service</span></span>
<span data-ttu-id="cc737-1835">Az Amazon S3 meghatározásához társított szolgáltatás, állítsa be a **típus** a társított szolgáltatás **AwsAccessKey**, és adja meg a következő tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1835">To define an Amazon S3 linked service, set the **type** of the linked service to **AwsAccessKey**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="cc737-1836">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1836">Property</span></span> | <span data-ttu-id="cc737-1837">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1837">Description</span></span> | <span data-ttu-id="cc737-1838">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-1838">Allowed values</span></span> | <span data-ttu-id="cc737-1839">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1839">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-1840">accessKeyID</span><span class="sxs-lookup"><span data-stu-id="cc737-1840">accessKeyID</span></span> |<span data-ttu-id="cc737-1841">A titkos hívóbetű azonosítója.</span><span class="sxs-lookup"><span data-stu-id="cc737-1841">ID of the secret access key.</span></span> |<span data-ttu-id="cc737-1842">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cc737-1842">string</span></span> |<span data-ttu-id="cc737-1843">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1843">Yes</span></span> |
| <span data-ttu-id="cc737-1844">secretAccessKey</span><span class="sxs-lookup"><span data-stu-id="cc737-1844">secretAccessKey</span></span> |<span data-ttu-id="cc737-1845">A titkos hívóbetű magát.</span><span class="sxs-lookup"><span data-stu-id="cc737-1845">The secret access key itself.</span></span> |<span data-ttu-id="cc737-1846">Titkosított titkos karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cc737-1846">Encrypted secret string</span></span> |<span data-ttu-id="cc737-1847">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1847">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-1848">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1848">Example</span></span>
```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

<span data-ttu-id="cc737-1849">További információkért lásd: [Amazon S3 összekötő cikk](data-factory-amazon-simple-storage-service-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="cc737-1849">For more information, see [Amazon S3 connector article](data-factory-amazon-simple-storage-service-connector.md#linked-service-properties).</span></span>

### <a name="dataset"></a><span data-ttu-id="cc737-1850">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-1850">Dataset</span></span>
<span data-ttu-id="cc737-1851">Adja meg az Amazon S3 adatkészlethez, állítsa be a **típus** a DataSet **AmazonS3**, és adja meg az alábbi tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1851">To define an Amazon S3 dataset, set the **type** of the dataset to **AmazonS3**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="cc737-1852">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1852">Property</span></span> | <span data-ttu-id="cc737-1853">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1853">Description</span></span> | <span data-ttu-id="cc737-1854">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-1854">Allowed values</span></span> | <span data-ttu-id="cc737-1855">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1855">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-1856">bucketName</span><span class="sxs-lookup"><span data-stu-id="cc737-1856">bucketName</span></span> |<span data-ttu-id="cc737-1857">S3 gyűjtő neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-1857">The S3 bucket name.</span></span> |<span data-ttu-id="cc737-1858">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cc737-1858">String</span></span> |<span data-ttu-id="cc737-1859">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1859">Yes</span></span> |
| <span data-ttu-id="cc737-1860">kulcs</span><span class="sxs-lookup"><span data-stu-id="cc737-1860">key</span></span> |<span data-ttu-id="cc737-1861">S3 objektum kulcsa.</span><span class="sxs-lookup"><span data-stu-id="cc737-1861">The S3 object key.</span></span> |<span data-ttu-id="cc737-1862">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cc737-1862">String</span></span> |<span data-ttu-id="cc737-1863">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1863">No</span></span> |
| <span data-ttu-id="cc737-1864">előtag</span><span class="sxs-lookup"><span data-stu-id="cc737-1864">prefix</span></span> |<span data-ttu-id="cc737-1865">S3 objektum kulcshoz előtag.</span><span class="sxs-lookup"><span data-stu-id="cc737-1865">Prefix for the S3 object key.</span></span> <span data-ttu-id="cc737-1866">Kiválasztott objektumok, amelynek kulcsait a előtaggal kezdődik.</span><span class="sxs-lookup"><span data-stu-id="cc737-1866">Objects whose keys start with this prefix are selected.</span></span> <span data-ttu-id="cc737-1867">Érvényes, csak ha kulcsa üres.</span><span class="sxs-lookup"><span data-stu-id="cc737-1867">Applies only when key is empty.</span></span> |<span data-ttu-id="cc737-1868">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cc737-1868">String</span></span> |<span data-ttu-id="cc737-1869">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1869">No</span></span> |
| <span data-ttu-id="cc737-1870">Verzió</span><span class="sxs-lookup"><span data-stu-id="cc737-1870">version</span></span> |<span data-ttu-id="cc737-1871">Ha engedélyezve van a S3 versioning S3 objektum verziója.</span><span class="sxs-lookup"><span data-stu-id="cc737-1871">The version of S3 object if S3 versioning is enabled.</span></span> |<span data-ttu-id="cc737-1872">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cc737-1872">String</span></span> |<span data-ttu-id="cc737-1873">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1873">No</span></span> |
| <span data-ttu-id="cc737-1874">Formátumban</span><span class="sxs-lookup"><span data-stu-id="cc737-1874">format</span></span> | <span data-ttu-id="cc737-1875">A következő formátumban típusok támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="cc737-1875">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="cc737-1876">Állítsa be a **típus** tulajdonság a formátuma a következő értékek egyikét.</span><span class="sxs-lookup"><span data-stu-id="cc737-1876">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="cc737-1877">További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="cc737-1877">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="cc737-1878">Ha azt szeretné, hogy **másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a Formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban.</span><span class="sxs-lookup"><span data-stu-id="cc737-1878">If you want to **copy files as-is** between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="cc737-1879">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1879">No</span></span> | |
| <span data-ttu-id="cc737-1880">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="cc737-1880">compression</span></span> | <span data-ttu-id="cc737-1881">Adja meg a típus és az adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="cc737-1881">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="cc737-1882">Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="cc737-1882">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="cc737-1883">A támogatott szintek a következők: **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="cc737-1883">The supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="cc737-1884">További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="cc737-1884">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="cc737-1885">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1885">No</span></span> | |


> [!NOTE]
> <span data-ttu-id="cc737-1886">bucketName + kulcs elérési útja a S3 objektum, ahol a gyűjtő S3 objektumok a legfelső szintű tárolója, és a kulcs S3 objektum teljes elérési útja.</span><span class="sxs-lookup"><span data-stu-id="cc737-1886">bucketName + key specifies the location of the S3 object where bucket is the root container for S3 objects and key is the full path to S3 object.</span></span>

#### <a name="example-sample-dataset-with-prefix"></a><span data-ttu-id="cc737-1887">Példa: Minta-adatkészleteken előtaggal</span><span class="sxs-lookup"><span data-stu-id="cc737-1887">Example: Sample dataset with prefix</span></span>

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "prefix": "testFolder/test",
            "bucketName": "<S3 bucket name>",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
#### <a name="example-sample-data-set-with-version"></a><span data-ttu-id="cc737-1888">Példa: Mintát adatkészletre (verziójával)</span><span class="sxs-lookup"><span data-stu-id="cc737-1888">Example: Sample data set (with version)</span></span>

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "key": "testFolder/test.orc",
            "bucketName": "<S3 bucket name>",
            "version": "XXXXXXXXXczm0CJajYkHf0_k6LhBmkcL",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

#### <a name="example-dynamic-paths-for-s3"></a><span data-ttu-id="cc737-1889">Példa: A dinamikus útvonalak S3</span><span class="sxs-lookup"><span data-stu-id="cc737-1889">Example: Dynamic paths for S3</span></span>
<span data-ttu-id="cc737-1890">A minta rögzített értékeket használjuk az Amazon S3 adatkészlet kulcs és bucketName tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="cc737-1890">In the sample, we use fixed values for key and bucketName properties in the Amazon S3 dataset.</span></span>

```json
"key": "testFolder/test.orc",
"bucketName": "<S3 bucket name>",
```

<span data-ttu-id="cc737-1891">A kulcs és a futási időben dinamikusan bucketName kiszámításához rendszerváltozók SliceStart például a Data Factory lehet.</span><span class="sxs-lookup"><span data-stu-id="cc737-1891">You can have Data Factory calculate the key and bucketName dynamically at runtime by using system variables such as SliceStart.</span></span>

```json
"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"
```

<span data-ttu-id="cc737-1892">Az Amazon S3 adatkészlethez előtag tulajdonságának azonos teheti meg.</span><span class="sxs-lookup"><span data-stu-id="cc737-1892">You can do the same for the prefix property of an Amazon S3 dataset.</span></span> <span data-ttu-id="cc737-1893">Lásd: [adat-előállító funkciók és rendszerváltozók](data-factory-functions-variables.md) támogatott funkciók és változók listáját.</span><span class="sxs-lookup"><span data-stu-id="cc737-1893">See [Data Factory functions and system variables](data-factory-functions-variables.md) for a list of supported functions and variables.</span></span>

<span data-ttu-id="cc737-1894">További információkért lásd: [Amazon S3 összekötő cikk](data-factory-amazon-simple-storage-service-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="cc737-1894">For more information, see [Amazon S3 connector article](data-factory-amazon-simple-storage-service-connector.md#dataset-properties).</span></span>

### <a name="file-system-source-in-copy-activity"></a><span data-ttu-id="cc737-1895">A másolási tevékenység rendszer forrás</span><span class="sxs-lookup"><span data-stu-id="cc737-1895">File System Source in Copy Activity</span></span>
<span data-ttu-id="cc737-1896">Adatok másolása az Amazon S3, állítsa be a **adatforrástípust** a másolási tevékenység **FileSystemSource**, és adja meg a következő tulajdonságokat a **forrás** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1896">If you are copying data from Amazon S3, set the **source type** of the copy activity to **FileSystemSource**, and specify following properties in the **source** section:</span></span>


| <span data-ttu-id="cc737-1897">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1897">Property</span></span> | <span data-ttu-id="cc737-1898">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1898">Description</span></span> | <span data-ttu-id="cc737-1899">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-1899">Allowed values</span></span> | <span data-ttu-id="cc737-1900">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1900">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-1901">Rekurzív</span><span class="sxs-lookup"><span data-stu-id="cc737-1901">recursive</span></span> |<span data-ttu-id="cc737-1902">Határozza meg, hogy a rekurzív módon listában S3 objektumok a könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="cc737-1902">Specifies whether to recursively list S3 objects under the directory.</span></span> |<span data-ttu-id="cc737-1903">Igaz/hamis</span><span class="sxs-lookup"><span data-stu-id="cc737-1903">true/false</span></span> |<span data-ttu-id="cc737-1904">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1904">No</span></span> |


#### <a name="example"></a><span data-ttu-id="cc737-1905">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1905">Example</span></span>


```json
{
    "name": "CopyAmazonS3ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource",
                    "recursive": true
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "AmazonS3InputDataset"
            }],
            "outputs": [{
                "name": "AzureBlobOutputDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "AmazonS3ToBlob"
        }],
        "start": "2016-08-08T18:00:00",
        "end": "2016-08-08T19:00:00"
    }
}
```

<span data-ttu-id="cc737-1906">További információkért lásd: [Amazon S3 összekötő cikk](data-factory-amazon-simple-storage-service-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="cc737-1906">For more information, see [Amazon S3 connector article](data-factory-amazon-simple-storage-service-connector.md#copy-activity-properties).</span></span>

## <a name="file-system"></a><span data-ttu-id="cc737-1907">Fájlrendszer</span><span class="sxs-lookup"><span data-stu-id="cc737-1907">File System</span></span>


### <a name="linked-service"></a><span data-ttu-id="cc737-1908">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-1908">Linked service</span></span>
<span data-ttu-id="cc737-1909">Egy helyszíni fájlrendszer hozzákapcsolhatja egy az Azure data factory, a **a helyi fájlkiszolgáló** társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="cc737-1909">You can link an on-premises file system to an Azure data factory with the **On-Premises File Server** linked service.</span></span> <span data-ttu-id="cc737-1910">A következő táblázat ismerteti, amelyek a helyszíni fájl kiszolgálóhoz kapcsolódó szolgáltatásra vonatkozó JSON-elemek szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="cc737-1910">The following table provides descriptions for JSON elements that are specific to the On-Premises File Server linked service.</span></span>

| <span data-ttu-id="cc737-1911">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1911">Property</span></span> | <span data-ttu-id="cc737-1912">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1912">Description</span></span> | <span data-ttu-id="cc737-1913">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1913">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-1914">type</span><span class="sxs-lookup"><span data-stu-id="cc737-1914">type</span></span> |<span data-ttu-id="cc737-1915">Győződjön meg arról, hogy a type tulajdonság értéke **OnPremisesFileServer**.</span><span class="sxs-lookup"><span data-stu-id="cc737-1915">Ensure that the type property is set to **OnPremisesFileServer**.</span></span> |<span data-ttu-id="cc737-1916">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1916">Yes</span></span> |
| <span data-ttu-id="cc737-1917">állomás</span><span class="sxs-lookup"><span data-stu-id="cc737-1917">host</span></span> |<span data-ttu-id="cc737-1918">Adja meg a legfelső szintű mappa elérési útját, amelyet szeretne másolni.</span><span class="sxs-lookup"><span data-stu-id="cc737-1918">Specifies the root path of the folder that you want to copy.</span></span> <span data-ttu-id="cc737-1919">Az escape-karakter használata "\" a speciális karakterek a karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="cc737-1919">Use the escape character ‘ \ ’ for special characters in the string.</span></span> <span data-ttu-id="cc737-1920">Lásd: [minta kapcsolódó szolgáltatás és az adatkészlet-definíciók](#sample-linked-service-and-dataset-definitions) példákat.</span><span class="sxs-lookup"><span data-stu-id="cc737-1920">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span> |<span data-ttu-id="cc737-1921">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1921">Yes</span></span> |
| <span data-ttu-id="cc737-1922">felhasználói azonosítóját</span><span class="sxs-lookup"><span data-stu-id="cc737-1922">userid</span></span> |<span data-ttu-id="cc737-1923">Adja meg a felhasználó, aki hozzáfér a kiszolgáló Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="cc737-1923">Specify the ID of the user who has access to the server.</span></span> |<span data-ttu-id="cc737-1924">Nem (Ha úgy dönt, hogy encryptedCredential)</span><span class="sxs-lookup"><span data-stu-id="cc737-1924">No (if you choose encryptedCredential)</span></span> |
| <span data-ttu-id="cc737-1925">jelszó</span><span class="sxs-lookup"><span data-stu-id="cc737-1925">password</span></span> |<span data-ttu-id="cc737-1926">Adja meg a felhasználó (userid) jelszavát.</span><span class="sxs-lookup"><span data-stu-id="cc737-1926">Specify the password for the user (userid).</span></span> |<span data-ttu-id="cc737-1927">Nem (Ha úgy dönt, hogy encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="cc737-1927">No (if you choose encryptedCredential</span></span> |
| <span data-ttu-id="cc737-1928">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="cc737-1928">encryptedCredential</span></span> |<span data-ttu-id="cc737-1929">Adja meg a titkosított hitelesítő adatokat kaphat a New-AzureRmDataFactoryEncryptValue parancsmag futtatásával.</span><span class="sxs-lookup"><span data-stu-id="cc737-1929">Specify the encrypted credentials that you can get by running the New-AzureRmDataFactoryEncryptValue cmdlet.</span></span> |<span data-ttu-id="cc737-1930">Nem (Ha úgy dönt, hogy adja meg a felhasználói azonosítót és jelszót a szövegként)</span><span class="sxs-lookup"><span data-stu-id="cc737-1930">No (if you choose to specify userid and password in plain text)</span></span> |
| <span data-ttu-id="cc737-1931">gatewayName</span><span class="sxs-lookup"><span data-stu-id="cc737-1931">gatewayName</span></span> |<span data-ttu-id="cc737-1932">Megadja a Data Factory kell csatlakozni a helyi fájlkiszolgálón használó átjáró nevét.</span><span class="sxs-lookup"><span data-stu-id="cc737-1932">Specifies the name of the gateway that Data Factory should use to connect to the on-premises file server.</span></span> |<span data-ttu-id="cc737-1933">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1933">Yes</span></span> |

#### <a name="sample-folder-path-definitions"></a><span data-ttu-id="cc737-1934">A minta mappa elérési útja definíciók</span><span class="sxs-lookup"><span data-stu-id="cc737-1934">Sample folder path definitions</span></span> 
| <span data-ttu-id="cc737-1935">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="cc737-1935">Scenario</span></span> | <span data-ttu-id="cc737-1936">A társított szolgáltatás definíciójának üzemeltetéséhez</span><span class="sxs-lookup"><span data-stu-id="cc737-1936">Host in linked service definition</span></span> | <span data-ttu-id="cc737-1937">Az adatkészlet-definícióban folderPath</span><span class="sxs-lookup"><span data-stu-id="cc737-1937">folderPath in dataset definition</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-1938">Az adatkezelési átjáró gépen helyi mappában:</span><span class="sxs-lookup"><span data-stu-id="cc737-1938">Local folder on Data Management Gateway machine:</span></span> <br/><br/><span data-ttu-id="cc737-1939">Példák: D:\\ \* vagy D:\folder\subfolder\\*</span><span class="sxs-lookup"><span data-stu-id="cc737-1939">Examples: D:\\\* or D:\folder\subfolder\\*</span></span> |<span data-ttu-id="cc737-1940">D:\\ \\ (az adatok felügyeleti átjáró 2.0-s és újabb verziók)</span><span class="sxs-lookup"><span data-stu-id="cc737-1940">D:\\\\ (for Data Management Gateway 2.0 and later versions)</span></span> <br/><br/> <span data-ttu-id="cc737-1941">a localhost (korábbi verzióihoz mint adatok felügyeleti átjáró 2.0-s)</span><span class="sxs-lookup"><span data-stu-id="cc737-1941">localhost (for earlier versions than Data Management Gateway 2.0)</span></span> |<span data-ttu-id="cc737-1942">. \\ \\ vagy mappa\\\\almappa (az adatok felügyeleti átjáró 2.0-s és újabb verziók)</span><span class="sxs-lookup"><span data-stu-id="cc737-1942">.\\\\ or folder\\\\subfolder (for Data Management Gateway 2.0 and later versions)</span></span> <br/><br/><span data-ttu-id="cc737-1943">D:\\ \\ vagy D:\\\\mappa\\\\almappa (az átjáró verziója alatt 2.0-s)</span><span class="sxs-lookup"><span data-stu-id="cc737-1943">D:\\\\ or D:\\\\folder\\\\subfolder (for gateway version below 2.0)</span></span> |
| <span data-ttu-id="cc737-1944">Távoli megosztott mappa:</span><span class="sxs-lookup"><span data-stu-id="cc737-1944">Remote shared folder:</span></span> <br/><br/><span data-ttu-id="cc737-1945">Példák: \\ \\myserver\\megosztása\\ \* vagy \\ \\myserver\\megosztása\\mappa\\almappa\\*</span><span class="sxs-lookup"><span data-stu-id="cc737-1945">Examples: \\\\myserver\\share\\\* or \\\\myserver\\share\\folder\\subfolder\\*</span></span> |<span data-ttu-id="cc737-1946">\\\\\\\\myserver\\\\megosztása</span><span class="sxs-lookup"><span data-stu-id="cc737-1946">\\\\\\\\myserver\\\\share</span></span> |<span data-ttu-id="cc737-1947">. \\ \\ vagy mappa\\\\almappa</span><span class="sxs-lookup"><span data-stu-id="cc737-1947">.\\\\ or folder\\\\subfolder</span></span> |


#### <a name="example-using-username-and-password-in-plain-text"></a><span data-ttu-id="cc737-1948">Példa: Felhasználónév és jelszó használatával egyszerű szöveges</span><span class="sxs-lookup"><span data-stu-id="cc737-1948">Example: Using username and password in plain text</span></span>

```json
{
    "Name": "OnPremisesFileServerLinkedService",
    "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
            "host": "\\\\Contosogame-Asia",
            "userid": "Admin",
            "password": "123456",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-using-encryptedcredential"></a><span data-ttu-id="cc737-1949">Példa: Encryptedcredential használatával</span><span class="sxs-lookup"><span data-stu-id="cc737-1949">Example: Using encryptedcredential</span></span>

```json
{
    "Name": " OnPremisesFileServerLinkedService ",
    "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
            "host": "D:\\",
            "encryptedCredential": "WFuIGlzIGRpc3Rpbmd1aXNoZWQsIG5vdCBvbmx5IGJ5xxxxxxxxxxxxxxxxx",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

<span data-ttu-id="cc737-1950">További információkért lásd: [fájlrendszer összekötő cikk](data-factory-onprem-file-system-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="cc737-1950">For more information, see [File System connector article](data-factory-onprem-file-system-connector.md#linked-service-properties).</span></span>

### <a name="dataset"></a><span data-ttu-id="cc737-1951">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-1951">Dataset</span></span>
<span data-ttu-id="cc737-1952">Adja meg a fájlrendszer adatkészletet, állítsa be a **típus** a DataSet **fájlmegosztási**, adja meg a következő tulajdonságokat és a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1952">To define a File System dataset, set the **type** of the dataset to **FileShare**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="cc737-1953">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1953">Property</span></span> | <span data-ttu-id="cc737-1954">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1954">Description</span></span> | <span data-ttu-id="cc737-1955">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1955">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-1956">folderPath</span><span class="sxs-lookup"><span data-stu-id="cc737-1956">folderPath</span></span> |<span data-ttu-id="cc737-1957">Adja meg a részleges elérési útja a mappához.</span><span class="sxs-lookup"><span data-stu-id="cc737-1957">Specifies the subpath to the folder.</span></span> <span data-ttu-id="cc737-1958">Az escape-karakter használata "\" a speciális karakterek a karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="cc737-1958">Use the escape character ‘\’ for special characters in the string.</span></span> <span data-ttu-id="cc737-1959">Lásd: [minta kapcsolódó szolgáltatás és az adatkészlet-definíciók](#sample-linked-service-and-dataset-definitions) példákat.</span><span class="sxs-lookup"><span data-stu-id="cc737-1959">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="cc737-1960">Ez a tulajdonság a kombinálhatja **partitionBy** szeretné, hogy a mappa elérési utak alapján szelet kezdő és záró dátum-idő.</span><span class="sxs-lookup"><span data-stu-id="cc737-1960">You can combine this property with **partitionBy** to have folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="cc737-1961">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-1961">Yes</span></span> |
| <span data-ttu-id="cc737-1962">fileName</span><span class="sxs-lookup"><span data-stu-id="cc737-1962">fileName</span></span> |<span data-ttu-id="cc737-1963">Adja meg a fájl nevét a **folderPath** Ha azt szeretné, hogy a tábla egy adott fájlra a mappában.</span><span class="sxs-lookup"><span data-stu-id="cc737-1963">Specify the name of the file in the **folderPath** if you want the table to refer to a specific file in the folder.</span></span> <span data-ttu-id="cc737-1964">Ha nem ad meg ehhez a tulajdonsághoz értéket, a tábla a mappában lévő összes fájlt mutat.</span><span class="sxs-lookup"><span data-stu-id="cc737-1964">If you do not specify any value for this property, the table points to all files in the folder.</span></span><br/><br/><span data-ttu-id="cc737-1965">Nincs megadva fájlnév egy kimeneti adatkészletet, a létrehozott fájl neve esetén a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="cc737-1965">When fileName is not specified for an output dataset, the name of the generated file is in the following format:</span></span> <br/><br/><span data-ttu-id="cc737-1966">`Data.<Guid>.txt`(Példa: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span><span class="sxs-lookup"><span data-stu-id="cc737-1966">`Data.<Guid>.txt` (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span></span> |<span data-ttu-id="cc737-1967">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1967">No</span></span> |
| <span data-ttu-id="cc737-1968">fileFilter</span><span class="sxs-lookup"><span data-stu-id="cc737-1968">fileFilter</span></span> |<span data-ttu-id="cc737-1969">Adjon meg egy szűrőt, amely minden fájl helyett a fájlok Tárolónév részhalmazának kiválasztására szolgál.</span><span class="sxs-lookup"><span data-stu-id="cc737-1969">Specify a filter to be used to select a subset of files in the folderPath rather than all files.</span></span> <br/><br/><span data-ttu-id="cc737-1970">Két érték engedélyezett: `*` (több karaktert) és `?` (egyetlen karakter).</span><span class="sxs-lookup"><span data-stu-id="cc737-1970">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="cc737-1971">1. példa: "fileFilter": "* .log"</span><span class="sxs-lookup"><span data-stu-id="cc737-1971">Example 1: "fileFilter": "*.log"</span></span><br/><span data-ttu-id="cc737-1972">2. példa: "fileFilter": 2016 - 1-?. txt"</span><span class="sxs-lookup"><span data-stu-id="cc737-1972">Example 2: "fileFilter": 2016-1-?.txt"</span></span><br/><br/><span data-ttu-id="cc737-1973">Vegye figyelembe, hogy fileFilter egy bemeneti fájlmegosztási adatkészlet esetében alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="cc737-1973">Note that fileFilter is applicable for an input FileShare dataset.</span></span> |<span data-ttu-id="cc737-1974">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1974">No</span></span> |
| <span data-ttu-id="cc737-1975">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="cc737-1975">partitionedBy</span></span> |<span data-ttu-id="cc737-1976">PartitionedBy segítségével adjon meg egy dinamikus folderPath/fájlnevet idősorozat adatok.</span><span class="sxs-lookup"><span data-stu-id="cc737-1976">You can use partitionedBy to specify a dynamic folderPath/fileName for time-series data.</span></span> <span data-ttu-id="cc737-1977">Példa: az adatok óránkénti paraméteres folderPath.</span><span class="sxs-lookup"><span data-stu-id="cc737-1977">An example is folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="cc737-1978">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1978">No</span></span> |
| <span data-ttu-id="cc737-1979">Formátumban</span><span class="sxs-lookup"><span data-stu-id="cc737-1979">format</span></span> | <span data-ttu-id="cc737-1980">A következő formátumban típusok támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="cc737-1980">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="cc737-1981">Állítsa be a **típus** tulajdonság a formátuma a következő értékek egyikét.</span><span class="sxs-lookup"><span data-stu-id="cc737-1981">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="cc737-1982">További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="cc737-1982">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="cc737-1983">Ha azt szeretné, hogy **másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a Formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban.</span><span class="sxs-lookup"><span data-stu-id="cc737-1983">If you want to **copy files as-is** between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="cc737-1984">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1984">No</span></span> |
| <span data-ttu-id="cc737-1985">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="cc737-1985">compression</span></span> | <span data-ttu-id="cc737-1986">Adja meg a típus és az adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="cc737-1986">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="cc737-1987">Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**; és a támogatott szintek a következők: **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="cc737-1987">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**; and supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="cc737-1988">Lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="cc737-1988">see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="cc737-1989">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-1989">No</span></span> |

> [!NOTE]
> <span data-ttu-id="cc737-1990">Nem használható egyszerre fájlnév és fileFilter.</span><span class="sxs-lookup"><span data-stu-id="cc737-1990">You cannot use fileName and fileFilter simultaneously.</span></span>

#### <a name="example"></a><span data-ttu-id="cc737-1991">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-1991">Example</span></span>

```json
{
    "name": "OnpremisesFileSystemInput",
    "properties": {
        "type": " FileShare",
        "linkedServiceName": " OnPremisesFileServerLinkedService ",
        "typeProperties": {
            "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
            "fileName": "{Hour}.csv",
            "partitionedBy": [{
                "name": "Year",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                        "format": "yyyy"
                }
            }, {
                "name": "Month",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "MM"
                }
            }, {
                "name": "Day",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "dd"
                }
            }, {
                "name": "Hour",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "HH"
                }
            }]
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="cc737-1992">További információkért lásd: [fájlrendszer összekötő cikk](data-factory-onprem-file-system-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="cc737-1992">For more information, see [File System connector article](data-factory-onprem-file-system-connector.md#dataset-properties).</span></span>

### <a name="file-system-source-in-copy-activity"></a><span data-ttu-id="cc737-1993">A másolási tevékenység rendszer forrás</span><span class="sxs-lookup"><span data-stu-id="cc737-1993">File System Source in Copy Activity</span></span>
<span data-ttu-id="cc737-1994">Adatok másolása a fájlrendszerből, állítsa be a **adatforrástípust** a másolási tevékenység **FileSystemSource**, és adja meg a következő tulajdonságokat a **forrás** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-1994">If you are copying data from File System, set the **source type** of the copy activity to **FileSystemSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="cc737-1995">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-1995">Property</span></span> | <span data-ttu-id="cc737-1996">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-1996">Description</span></span> | <span data-ttu-id="cc737-1997">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-1997">Allowed values</span></span> | <span data-ttu-id="cc737-1998">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-1998">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-1999">Rekurzív</span><span class="sxs-lookup"><span data-stu-id="cc737-1999">recursive</span></span> |<span data-ttu-id="cc737-2000">Azt jelzi, hogy az adatok olvasható rekurzív módon az almappákat, illetve csak a megadott mappát.</span><span class="sxs-lookup"><span data-stu-id="cc737-2000">Indicates whether the data is read recursively from the subfolders or only from the specified folder.</span></span> |<span data-ttu-id="cc737-2001">IGAZ, hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="cc737-2001">True, False (default)</span></span> |<span data-ttu-id="cc737-2002">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2002">No</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-2003">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2003">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2015-06-01T18:00:00",
        "end": "2015-06-01T19:00:00",
        "description": "Pipeline for copy activity",
        "activities": [{
            "name": "OnpremisesFileSystemtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "OnpremisesFileSystemInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
            "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
<span data-ttu-id="cc737-2004">További információkért lásd: [fájlrendszer összekötő cikk](data-factory-onprem-file-system-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="cc737-2004">For more information, see [File System connector article](data-factory-onprem-file-system-connector.md#copy-activity-properties).</span></span>

### <a name="file-system-sink-in-copy-activity"></a><span data-ttu-id="cc737-2005">A másolási tevékenység gyűjtése fájlrendszer</span><span class="sxs-lookup"><span data-stu-id="cc737-2005">File System Sink in Copy Activity</span></span>
<span data-ttu-id="cc737-2006">Adatok másolása fájlrendszerre, állítsa be a **típus gyűjtése** a másolási tevékenység **FileSystemSink**, és adja meg a következő tulajdonságokat a **fogadó** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-2006">If you are copying data to File System, set the **sink type** of the copy activity to **FileSystemSink**, and specify following properties in the **sink** section:</span></span>

| <span data-ttu-id="cc737-2007">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2007">Property</span></span> | <span data-ttu-id="cc737-2008">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2008">Description</span></span> | <span data-ttu-id="cc737-2009">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-2009">Allowed values</span></span> | <span data-ttu-id="cc737-2010">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2010">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-2011">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="cc737-2011">copyBehavior</span></span> |<span data-ttu-id="cc737-2012">Másolás viselkedését határozza meg, ha az adatforrás BlobSource vagy a fájlrendszer.</span><span class="sxs-lookup"><span data-stu-id="cc737-2012">Defines the copy behavior when the source is BlobSource or FileSystem.</span></span> |<span data-ttu-id="cc737-2013">**PreserveHierarchy:** őrzi meg a fájl hierarchia a célmappában.</span><span class="sxs-lookup"><span data-stu-id="cc737-2013">**PreserveHierarchy:** Preserves the file hierarchy in the target folder.</span></span> <span data-ttu-id="cc737-2014">Ez azt jelenti, hogy a forrásfájl, a forrásmappához relatív elérési út ugyanaz, mint a relatív a cél elérési útja a célként megadott mappába.</span><span class="sxs-lookup"><span data-stu-id="cc737-2014">That is, the relative path of the source file to the source folder is the same as the relative path of the target file to the target folder.</span></span><br/><br/><span data-ttu-id="cc737-2015">**FlattenHierarchy:** minden fájl a forrásmappából az első szintű célmappában jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="cc737-2015">**FlattenHierarchy:** All files from the source folder are created in the first level of target folder.</span></span> <span data-ttu-id="cc737-2016">A cél fájlok jönnek létre automatikusan létrehozott névvel.</span><span class="sxs-lookup"><span data-stu-id="cc737-2016">The target files are created with an autogenerated name.</span></span><br/><br/><span data-ttu-id="cc737-2017">**Mergefiles típusú:** egyesíti a forrásmappából egy fájl összes fájlt.</span><span class="sxs-lookup"><span data-stu-id="cc737-2017">**MergeFiles:** Merges all files from the source folder to one file.</span></span> <span data-ttu-id="cc737-2018">Ha a fájl neve/blob neve meg van adva, az egyesített fájlnév a megadott név.</span><span class="sxs-lookup"><span data-stu-id="cc737-2018">If the file name/blob name is specified, the merged file name is the specified name.</span></span> <span data-ttu-id="cc737-2019">Ellenkező esetben egy automatikusan létrehozott nevét.</span><span class="sxs-lookup"><span data-stu-id="cc737-2019">Otherwise, it is an auto-generated file name.</span></span> |<span data-ttu-id="cc737-2020">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2020">No</span></span> |
<span data-ttu-id="cc737-2021">automatikus-</span><span class="sxs-lookup"><span data-stu-id="cc737-2021">auto-</span></span>

#### <a name="example"></a><span data-ttu-id="cc737-2022">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2022">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2015-06-01T18:00:00",
        "end": "2015-06-01T20:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLtoOnPremisesFile",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSQLInput"
            }],
            "outputs": [{
                "name": "OnpremisesFileSystemOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "FileSystemSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 3,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="cc737-2023">További információkért lásd: [fájlrendszer összekötő cikk](data-factory-onprem-file-system-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="cc737-2023">For more information, see [File System connector article](data-factory-onprem-file-system-connector.md#copy-activity-properties).</span></span>

## <a name="ftp"></a><span data-ttu-id="cc737-2024">FTP</span><span class="sxs-lookup"><span data-stu-id="cc737-2024">FTP</span></span>

### <a name="linked-service"></a><span data-ttu-id="cc737-2025">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-2025">Linked service</span></span>
<span data-ttu-id="cc737-2026">Egy FTP meghatározásához társított szolgáltatás, állítsa be a **típus** a társított szolgáltatás **FTP-kiszolgáló**, és adja meg a következő tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-2026">To define an FTP linked service, set the **type** of the linked service to **FtpServer**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="cc737-2027">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2027">Property</span></span> | <span data-ttu-id="cc737-2028">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2028">Description</span></span> | <span data-ttu-id="cc737-2029">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2029">Required</span></span> | <span data-ttu-id="cc737-2030">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="cc737-2030">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-2031">állomás</span><span class="sxs-lookup"><span data-stu-id="cc737-2031">host</span></span> |<span data-ttu-id="cc737-2032">Az FTP-kiszolgáló neve vagy IP-cím</span><span class="sxs-lookup"><span data-stu-id="cc737-2032">Name or IP address of the FTP Server</span></span> |<span data-ttu-id="cc737-2033">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2033">Yes</span></span> |&nbsp; |
| <span data-ttu-id="cc737-2034">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="cc737-2034">authenticationType</span></span> |<span data-ttu-id="cc737-2035">Adja meg a hitelesítés típusa</span><span class="sxs-lookup"><span data-stu-id="cc737-2035">Specify authentication type</span></span> |<span data-ttu-id="cc737-2036">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2036">Yes</span></span> |<span data-ttu-id="cc737-2037">Alapszintű, a névtelen</span><span class="sxs-lookup"><span data-stu-id="cc737-2037">Basic, Anonymous</span></span> |
| <span data-ttu-id="cc737-2038">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="cc737-2038">username</span></span> |<span data-ttu-id="cc737-2039">Az FTP-kiszolgálóhoz hozzáféréssel rendelkező felhasználó</span><span class="sxs-lookup"><span data-stu-id="cc737-2039">User who has access to the FTP server</span></span> |<span data-ttu-id="cc737-2040">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2040">No</span></span> |&nbsp; |
| <span data-ttu-id="cc737-2041">jelszó</span><span class="sxs-lookup"><span data-stu-id="cc737-2041">password</span></span> |<span data-ttu-id="cc737-2042">A felhasználó (felhasználónév) jelszavát</span><span class="sxs-lookup"><span data-stu-id="cc737-2042">Password for the user (username)</span></span> |<span data-ttu-id="cc737-2043">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2043">No</span></span> |&nbsp; |
| <span data-ttu-id="cc737-2044">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="cc737-2044">encryptedCredential</span></span> |<span data-ttu-id="cc737-2045">Az FTP-kiszolgáló eléréséhez titkosított hitelesítő adatokat</span><span class="sxs-lookup"><span data-stu-id="cc737-2045">Encrypted credential to access the FTP server</span></span> |<span data-ttu-id="cc737-2046">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2046">No</span></span> |&nbsp; |
| <span data-ttu-id="cc737-2047">gatewayName</span><span class="sxs-lookup"><span data-stu-id="cc737-2047">gatewayName</span></span> |<span data-ttu-id="cc737-2048">A helyszíni FTP-kiszolgálóhoz csatlakozni az adatkezelési átjáró átjáró neve</span><span class="sxs-lookup"><span data-stu-id="cc737-2048">Name of the Data Management Gateway gateway to connect to an on-premises FTP server</span></span> |<span data-ttu-id="cc737-2049">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2049">No</span></span> |&nbsp; |
| <span data-ttu-id="cc737-2050">port</span><span class="sxs-lookup"><span data-stu-id="cc737-2050">port</span></span> |<span data-ttu-id="cc737-2051">Port, amelyet az FTP-kiszolgáló figyel</span><span class="sxs-lookup"><span data-stu-id="cc737-2051">Port on which the FTP server is listening</span></span> |<span data-ttu-id="cc737-2052">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2052">No</span></span> |<span data-ttu-id="cc737-2053">21</span><span class="sxs-lookup"><span data-stu-id="cc737-2053">21</span></span> |
| <span data-ttu-id="cc737-2054">enableSsl</span><span class="sxs-lookup"><span data-stu-id="cc737-2054">enableSsl</span></span> |<span data-ttu-id="cc737-2055">Adja meg, hogy a TLS/SSL csatornán keresztül FTP használata</span><span class="sxs-lookup"><span data-stu-id="cc737-2055">Specify whether to use FTP over SSL/TLS channel</span></span> |<span data-ttu-id="cc737-2056">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2056">No</span></span> |<span data-ttu-id="cc737-2057">Igaz</span><span class="sxs-lookup"><span data-stu-id="cc737-2057">true</span></span> |
| <span data-ttu-id="cc737-2058">enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="cc737-2058">enableServerCertificateValidation</span></span> |<span data-ttu-id="cc737-2059">Adja meg, hogy engedélyezi az FTP-használ, a TLS/SSL csatornán keresztül kiszolgálói SSL-tanúsítvány hitelesítése</span><span class="sxs-lookup"><span data-stu-id="cc737-2059">Specify whether to enable server SSL certificate validation when using FTP over SSL/TLS channel</span></span> |<span data-ttu-id="cc737-2060">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2060">No</span></span> |<span data-ttu-id="cc737-2061">Igaz</span><span class="sxs-lookup"><span data-stu-id="cc737-2061">true</span></span> |

#### <a name="example-using-anonymous-authentication"></a><span data-ttu-id="cc737-2062">Példa: A névtelen hitelesítést használó</span><span class="sxs-lookup"><span data-stu-id="cc737-2062">Example: Using Anonymous authentication</span></span>

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
            "typeProperties": {
            "authenticationType": "Anonymous",
            "host": "myftpserver.com"
        }
    }
}
```

#### <a name="example-using-username-and-password-in-plain-text-for-basic-authentication"></a><span data-ttu-id="cc737-2063">Példa: Használatával felhasználónevet és jelszót egyszerű szövegként az egyszerű hitelesítés</span><span class="sxs-lookup"><span data-stu-id="cc737-2063">Example: Using username and password in plain text for basic authentication</span></span>

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
        }
    }
}
```

#### <a name="example-using-port-enablessl-enableservercertificatevalidation"></a><span data-ttu-id="cc737-2064">Példa: Port, enableSsl, enableServerCertificateValidation használatával</span><span class="sxs-lookup"><span data-stu-id="cc737-2064">Example: Using port, enableSsl, enableServerCertificateValidation</span></span>

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",    
            "username": "Admin",
            "password": "123456",
            "port": "21",
            "enableSsl": true,
            "enableServerCertificateValidation": true
        }
    }
}
```

#### <a name="example-using-encryptedcredential-for-authentication-and-gateway"></a><span data-ttu-id="cc737-2065">Példa: EncryptedCredential használata a hitelesítéshez és az átjáró</span><span class="sxs-lookup"><span data-stu-id="cc737-2065">Example: Using encryptedCredential for authentication and gateway</span></span>

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "gatewayName": "<onpremgateway>"
        }
      }
}
```

<span data-ttu-id="cc737-2066">További információkért lásd: [FTP-összekötő](data-factory-ftp-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2066">For more information, see [FTP connector](data-factory-ftp-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="cc737-2067">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-2067">Dataset</span></span>
<span data-ttu-id="cc737-2068">Adja meg az FTP-adatkészlet, állítsa be a **típus** a DataSet **fájlmegosztási**, és adja meg az alábbi tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-2068">To define an FTP dataset, set the **type** of the dataset to **FileShare**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="cc737-2069">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2069">Property</span></span> | <span data-ttu-id="cc737-2070">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2070">Description</span></span> | <span data-ttu-id="cc737-2071">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2071">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-2072">folderPath</span><span class="sxs-lookup"><span data-stu-id="cc737-2072">folderPath</span></span> |<span data-ttu-id="cc737-2073">Sub mappa elérési útját.</span><span class="sxs-lookup"><span data-stu-id="cc737-2073">Sub path to the folder.</span></span> <span data-ttu-id="cc737-2074">Használja az escape-karakter "\" a speciális karakterek a karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="cc737-2074">Use escape character ‘ \ ’ for special characters in the string.</span></span> <span data-ttu-id="cc737-2075">Lásd: [minta kapcsolódó szolgáltatás és az adatkészlet-definíciók](#sample-linked-service-and-dataset-definitions) példákat.</span><span class="sxs-lookup"><span data-stu-id="cc737-2075">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="cc737-2076">Ez a tulajdonság a kombinálhatja **partitionBy** szeretné, hogy a mappa elérési utak alapján szelet kezdő és záró dátum-idő.</span><span class="sxs-lookup"><span data-stu-id="cc737-2076">You can combine this property with **partitionBy** to have folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="cc737-2077">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2077">Yes</span></span> 
| <span data-ttu-id="cc737-2078">fileName</span><span class="sxs-lookup"><span data-stu-id="cc737-2078">fileName</span></span> |<span data-ttu-id="cc737-2079">Adja meg a fájl nevét a **folderPath** Ha azt szeretné, hogy a tábla egy adott fájlra a mappában.</span><span class="sxs-lookup"><span data-stu-id="cc737-2079">Specify the name of the file in the **folderPath** if you want the table to refer to a specific file in the folder.</span></span> <span data-ttu-id="cc737-2080">Ha nem ad meg ehhez a tulajdonsághoz értéket, a tábla a mappában lévő összes fájlt mutat.</span><span class="sxs-lookup"><span data-stu-id="cc737-2080">If you do not specify any value for this property, the table points to all files in the folder.</span></span><br/><br/><span data-ttu-id="cc737-2081">Ha nincs megadva fájlnév egy kimeneti adatkészletet, a létrehozott fájl nevét a következő lenne ebben a formátumban:</span><span class="sxs-lookup"><span data-stu-id="cc737-2081">When fileName is not specified for an output dataset, the name of the generated file would be in the following this format:</span></span> <br/><br/><span data-ttu-id="cc737-2082">Adatok. <Guid>.txt (Példa: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="cc737-2082">Data.<Guid>.txt (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="cc737-2083">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2083">No</span></span> |
| <span data-ttu-id="cc737-2084">fileFilter</span><span class="sxs-lookup"><span data-stu-id="cc737-2084">fileFilter</span></span> |<span data-ttu-id="cc737-2085">Adjon meg egy szűrőt, amely minden fájl helyett a fájlok Tárolónév részhalmazának kiválasztására szolgál.</span><span class="sxs-lookup"><span data-stu-id="cc737-2085">Specify a filter to be used to select a subset of files in the folderPath rather than all files.</span></span><br/><br/><span data-ttu-id="cc737-2086">Két érték engedélyezett: `*` (több karaktert) és `?` (egyetlen karakter).</span><span class="sxs-lookup"><span data-stu-id="cc737-2086">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="cc737-2087">1. példa:`"fileFilter": "*.log"`</span><span class="sxs-lookup"><span data-stu-id="cc737-2087">Examples 1: `"fileFilter": "*.log"`</span></span><br/><span data-ttu-id="cc737-2088">2. példa:`"fileFilter": 2016-1-?.txt"`</span><span class="sxs-lookup"><span data-stu-id="cc737-2088">Example 2: `"fileFilter": 2016-1-?.txt"`</span></span><br/><br/> <span data-ttu-id="cc737-2089">fileFilter is alkalmazható egy bemeneti fájlmegosztási az adatkészlethez.</span><span class="sxs-lookup"><span data-stu-id="cc737-2089">fileFilter is applicable for an input FileShare dataset.</span></span> <span data-ttu-id="cc737-2090">Ez a tulajdonság a HDFS nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="cc737-2090">This property is not supported with HDFS.</span></span> |<span data-ttu-id="cc737-2091">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2091">No</span></span> |
| <span data-ttu-id="cc737-2092">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="cc737-2092">partitionedBy</span></span> |<span data-ttu-id="cc737-2093">Adjon meg egy dinamikus folderPath idő adatsor fájlnevét partitionedBy használható.</span><span class="sxs-lookup"><span data-stu-id="cc737-2093">partitionedBy can be used to specify a dynamic folderPath, filename for time series data.</span></span> <span data-ttu-id="cc737-2094">Például folderPath adatok óránkénti paraméteres.</span><span class="sxs-lookup"><span data-stu-id="cc737-2094">For example, folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="cc737-2095">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2095">No</span></span> |
| <span data-ttu-id="cc737-2096">Formátumban</span><span class="sxs-lookup"><span data-stu-id="cc737-2096">format</span></span> | <span data-ttu-id="cc737-2097">A következő formátumban típusok támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2097">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="cc737-2098">Állítsa be a **típus** tulajdonság a formátuma a következő értékek egyikét.</span><span class="sxs-lookup"><span data-stu-id="cc737-2098">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="cc737-2099">További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="cc737-2099">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="cc737-2100">Ha azt szeretné, hogy **másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a Formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban.</span><span class="sxs-lookup"><span data-stu-id="cc737-2100">If you want to **copy files as-is** between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="cc737-2101">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2101">No</span></span> |
| <span data-ttu-id="cc737-2102">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="cc737-2102">compression</span></span> | <span data-ttu-id="cc737-2103">Adja meg a típus és az adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="cc737-2103">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="cc737-2104">Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**; és a támogatott szintek a következők: **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2104">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**; and supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="cc737-2105">További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="cc737-2105">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="cc737-2106">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2106">No</span></span> |
| <span data-ttu-id="cc737-2107">useBinaryTransfer</span><span class="sxs-lookup"><span data-stu-id="cc737-2107">useBinaryTransfer</span></span> |<span data-ttu-id="cc737-2108">Adja meg, hogy a bináris átviteli mód használata.</span><span class="sxs-lookup"><span data-stu-id="cc737-2108">Specify whether use Binary transfer mode.</span></span> <span data-ttu-id="cc737-2109">A bináris mód és a hamis értéket ASCII igaz.</span><span class="sxs-lookup"><span data-stu-id="cc737-2109">True for binary mode and false ASCII.</span></span> <span data-ttu-id="cc737-2110">Alapértelmezett érték: igaz.</span><span class="sxs-lookup"><span data-stu-id="cc737-2110">Default value: True.</span></span> <span data-ttu-id="cc737-2111">A tulajdonság csak akkor használható, típusú társított kapcsolódószolgáltatás-típus esetén: FTP-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="cc737-2111">This property can only be used when associated linked service type is of type: FtpServer.</span></span> |<span data-ttu-id="cc737-2112">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2112">No</span></span> |

> [!NOTE]
> <span data-ttu-id="cc737-2113">fájlnév és fileFilter nem használható egyszerre.</span><span class="sxs-lookup"><span data-stu-id="cc737-2113">filename and fileFilter cannot be used simultaneously.</span></span>

#### <a name="example"></a><span data-ttu-id="cc737-2114">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2114">Example</span></span>

```json
{
    "name": "FTPFileInput",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "FTPLinkedService",
        "typeProperties": {
            "folderPath": "<path to shared folder>",
            "fileName": "test.csv",
            "useBinaryTransfer": true
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="cc737-2115">További információkért lásd: [FTP-összekötő](data-factory-ftp-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2115">For more information, see [FTP connector](data-factory-ftp-connector.md#dataset-properties) article.</span></span>

### <a name="file-system-source-in-copy-activity"></a><span data-ttu-id="cc737-2116">A másolási tevékenység rendszer forrás</span><span class="sxs-lookup"><span data-stu-id="cc737-2116">File System Source in Copy Activity</span></span>
<span data-ttu-id="cc737-2117">Adatok másolása az FTP-kiszolgálóról, állítsa be a **adatforrástípust** a másolási tevékenység **FileSystemSource**, és adja meg a következő tulajdonságokat a **forrás** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-2117">If you are copying data from an FTP server, set the **source type** of the copy activity to **FileSystemSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="cc737-2118">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2118">Property</span></span> | <span data-ttu-id="cc737-2119">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2119">Description</span></span> | <span data-ttu-id="cc737-2120">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-2120">Allowed values</span></span> | <span data-ttu-id="cc737-2121">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2121">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-2122">Rekurzív</span><span class="sxs-lookup"><span data-stu-id="cc737-2122">recursive</span></span> |<span data-ttu-id="cc737-2123">Azt jelzi, hogy az adatok olvasható rekurzív módon az almappák vagy csak a megadott mappát.</span><span class="sxs-lookup"><span data-stu-id="cc737-2123">Indicates whether the data is read recursively from the sub folders or only from the specified folder.</span></span> |<span data-ttu-id="cc737-2124">IGAZ, hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="cc737-2124">True, False (default)</span></span> |<span data-ttu-id="cc737-2125">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2125">No</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-2126">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2126">Example</span></span>

```json
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "FTPToBlobCopy",
            "inputs": [{
                "name": "FtpFileInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2016-08-24T18:00:00",
        "end": "2016-08-24T19:00:00"
    }
}
```

<span data-ttu-id="cc737-2127">További információkért lásd: [FTP-összekötő](data-factory-ftp-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2127">For more information, see [FTP connector](data-factory-ftp-connector.md#copy-activity-properties) article.</span></span>


## <a name="hdfs"></a><span data-ttu-id="cc737-2128">HDFS</span><span class="sxs-lookup"><span data-stu-id="cc737-2128">HDFS</span></span>

### <a name="linked-service"></a><span data-ttu-id="cc737-2129">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-2129">Linked service</span></span>
<span data-ttu-id="cc737-2130">A HDFS meghatározásához társított szolgáltatás, állítsa be a **típus** a társított szolgáltatás **Hdfs**, és adja meg a következő tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-2130">To define a HDFS linked service, set the **type** of the linked service to **Hdfs**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="cc737-2131">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2131">Property</span></span> | <span data-ttu-id="cc737-2132">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2132">Description</span></span> | <span data-ttu-id="cc737-2133">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2133">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-2134">type</span><span class="sxs-lookup"><span data-stu-id="cc737-2134">type</span></span> |<span data-ttu-id="cc737-2135">A type tulajdonságot kell beállítani: **Hdfs**</span><span class="sxs-lookup"><span data-stu-id="cc737-2135">The type property must be set to: **Hdfs**</span></span> |<span data-ttu-id="cc737-2136">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2136">Yes</span></span> |
| <span data-ttu-id="cc737-2137">URL-cím</span><span class="sxs-lookup"><span data-stu-id="cc737-2137">Url</span></span> |<span data-ttu-id="cc737-2138">A HDFS URL-címe</span><span class="sxs-lookup"><span data-stu-id="cc737-2138">URL to the HDFS</span></span> |<span data-ttu-id="cc737-2139">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2139">Yes</span></span> |
| <span data-ttu-id="cc737-2140">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="cc737-2140">authenticationType</span></span> |<span data-ttu-id="cc737-2141">Névtelen, vagy a Windows.</span><span class="sxs-lookup"><span data-stu-id="cc737-2141">Anonymous, or Windows.</span></span> <br><br> <span data-ttu-id="cc737-2142">Használandó **Kerberos-hitelesítés** HDFS-összekötőhöz, tekintse meg [ebben a szakaszban](#use-kerberos-authentication-for-hdfs-connector) ennek megfelelően a helyszíni környezet beállítása.</span><span class="sxs-lookup"><span data-stu-id="cc737-2142">To use **Kerberos authentication** for HDFS connector, refer to [this section](#use-kerberos-authentication-for-hdfs-connector) to set up your on-premises environment accordingly.</span></span> |<span data-ttu-id="cc737-2143">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2143">Yes</span></span> |
| <span data-ttu-id="cc737-2144">Felhasználónév</span><span class="sxs-lookup"><span data-stu-id="cc737-2144">userName</span></span> |<span data-ttu-id="cc737-2145">Felhasználónév a Windows-hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="cc737-2145">Username for Windows authentication.</span></span> |<span data-ttu-id="cc737-2146">Igen (a Windows-hitelesítés)</span><span class="sxs-lookup"><span data-stu-id="cc737-2146">Yes (for Windows Authentication)</span></span> |
| <span data-ttu-id="cc737-2147">jelszó</span><span class="sxs-lookup"><span data-stu-id="cc737-2147">password</span></span> |<span data-ttu-id="cc737-2148">A Windows-hitelesítés jelszót.</span><span class="sxs-lookup"><span data-stu-id="cc737-2148">Password for Windows authentication.</span></span> |<span data-ttu-id="cc737-2149">Igen (a Windows-hitelesítés)</span><span class="sxs-lookup"><span data-stu-id="cc737-2149">Yes (for Windows Authentication)</span></span> |
| <span data-ttu-id="cc737-2150">gatewayName</span><span class="sxs-lookup"><span data-stu-id="cc737-2150">gatewayName</span></span> |<span data-ttu-id="cc737-2151">Az átjáró, amely a Data Factory szolgáltatásnak csatlakoznia a HDFS a neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-2151">Name of the gateway that the Data Factory service should use to connect to the HDFS.</span></span> |<span data-ttu-id="cc737-2152">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2152">Yes</span></span> |
| <span data-ttu-id="cc737-2153">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="cc737-2153">encryptedCredential</span></span> |<span data-ttu-id="cc737-2154">[Új AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) kimenetét a hozzáférési hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="cc737-2154">[New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) output of the access credential.</span></span> |<span data-ttu-id="cc737-2155">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2155">No</span></span> |

#### <a name="example-using-anonymous-authentication"></a><span data-ttu-id="cc737-2156">Példa: A névtelen hitelesítést használó</span><span class="sxs-lookup"><span data-stu-id="cc737-2156">Example: Using Anonymous authentication</span></span>

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "authenticationType": "Anonymous",
            "userName": "hadoop",
            "url": "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-using-windows-authentication"></a><span data-ttu-id="cc737-2157">Példa: A Windows-hitelesítés használatával</span><span class="sxs-lookup"><span data-stu-id="cc737-2157">Example: Using Windows authentication</span></span>

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "authenticationType": "Windows",
            "userName": "Administrator",
            "password": "password",
            "url": "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

<span data-ttu-id="cc737-2158">További információkért lásd: [HDFS összekötő](#data-factory-hdfs-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2158">For more information, see [HDFS connector](#data-factory-hdfs-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="cc737-2159">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-2159">Dataset</span></span>
<span data-ttu-id="cc737-2160">Adja meg a HDFS adatkészletet, állítsa be a **típus** a DataSet **fájlmegosztási**, és adja meg az alábbi tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-2160">To define a HDFS dataset, set the **type** of the dataset to **FileShare**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="cc737-2161">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2161">Property</span></span> | <span data-ttu-id="cc737-2162">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2162">Description</span></span> | <span data-ttu-id="cc737-2163">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2163">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-2164">folderPath</span><span class="sxs-lookup"><span data-stu-id="cc737-2164">folderPath</span></span> |<span data-ttu-id="cc737-2165">A mappa elérési útját.</span><span class="sxs-lookup"><span data-stu-id="cc737-2165">Path to the folder.</span></span> <span data-ttu-id="cc737-2166">Példa:`myfolder`</span><span class="sxs-lookup"><span data-stu-id="cc737-2166">Example: `myfolder`</span></span><br/><br/><span data-ttu-id="cc737-2167">Használja az escape-karakter "\" a speciális karakterek a karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="cc737-2167">Use escape character ‘ \ ’ for special characters in the string.</span></span> <span data-ttu-id="cc737-2168">Például: folder\subfolder, adja meg a mappa\\\\almappa és d:\samplefolder, adja meg a d:\\\\mappába.</span><span class="sxs-lookup"><span data-stu-id="cc737-2168">For example: for folder\subfolder, specify folder\\\\subfolder and for d:\samplefolder, specify d:\\\\samplefolder.</span></span><br/><br/><span data-ttu-id="cc737-2169">Ez a tulajdonság a kombinálhatja **partitionBy** szeretné, hogy a mappa elérési utak alapján szelet kezdő és záró dátum-idő.</span><span class="sxs-lookup"><span data-stu-id="cc737-2169">You can combine this property with **partitionBy** to have folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="cc737-2170">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2170">Yes</span></span> |
| <span data-ttu-id="cc737-2171">fileName</span><span class="sxs-lookup"><span data-stu-id="cc737-2171">fileName</span></span> |<span data-ttu-id="cc737-2172">Adja meg a fájl nevét a **folderPath** Ha azt szeretné, hogy a tábla egy adott fájlra a mappában.</span><span class="sxs-lookup"><span data-stu-id="cc737-2172">Specify the name of the file in the **folderPath** if you want the table to refer to a specific file in the folder.</span></span> <span data-ttu-id="cc737-2173">Ha nem ad meg ehhez a tulajdonsághoz értéket, a tábla a mappában lévő összes fájlt mutat.</span><span class="sxs-lookup"><span data-stu-id="cc737-2173">If you do not specify any value for this property, the table points to all files in the folder.</span></span><br/><br/><span data-ttu-id="cc737-2174">Ha nincs megadva fájlnév egy kimeneti adatkészletet, a létrehozott fájl nevét a következő lenne ebben a formátumban:</span><span class="sxs-lookup"><span data-stu-id="cc737-2174">When fileName is not specified for an output dataset, the name of the generated file would be in the following this format:</span></span> <br/><br/><span data-ttu-id="cc737-2175">Adatok. <Guid>.txt (például:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="cc737-2175">Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="cc737-2176">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2176">No</span></span> |
| <span data-ttu-id="cc737-2177">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="cc737-2177">partitionedBy</span></span> |<span data-ttu-id="cc737-2178">Adjon meg egy dinamikus folderPath idő adatsor fájlnevét partitionedBy használható.</span><span class="sxs-lookup"><span data-stu-id="cc737-2178">partitionedBy can be used to specify a dynamic folderPath, filename for time series data.</span></span> <span data-ttu-id="cc737-2179">Példa: folderPath adatok óránkénti paraméteres.</span><span class="sxs-lookup"><span data-stu-id="cc737-2179">Example: folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="cc737-2180">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2180">No</span></span> |
| <span data-ttu-id="cc737-2181">Formátumban</span><span class="sxs-lookup"><span data-stu-id="cc737-2181">format</span></span> | <span data-ttu-id="cc737-2182">A következő formátumban típusok támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2182">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="cc737-2183">Állítsa be a **típus** tulajdonság a formátuma a következő értékek egyikét.</span><span class="sxs-lookup"><span data-stu-id="cc737-2183">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="cc737-2184">További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="cc737-2184">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="cc737-2185">Ha azt szeretné, hogy **másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a Formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban.</span><span class="sxs-lookup"><span data-stu-id="cc737-2185">If you want to **copy files as-is** between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="cc737-2186">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2186">No</span></span> |
| <span data-ttu-id="cc737-2187">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="cc737-2187">compression</span></span> | <span data-ttu-id="cc737-2188">Adja meg a típus és az adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="cc737-2188">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="cc737-2189">Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2189">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="cc737-2190">Támogatott szintek a következők: **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2190">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="cc737-2191">További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="cc737-2191">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="cc737-2192">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2192">No</span></span> |

> [!NOTE]
> <span data-ttu-id="cc737-2193">fájlnév és fileFilter nem használható egyszerre.</span><span class="sxs-lookup"><span data-stu-id="cc737-2193">filename and fileFilter cannot be used simultaneously.</span></span>

#### <a name="example"></a><span data-ttu-id="cc737-2194">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2194">Example</span></span>

```json
{
    "name": "InputDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "HDFSLinkedService",
        "typeProperties": {
            "folderPath": "DataTransfer/UnitTest/"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="cc737-2195">További információkért lásd: [HDFS összekötő](#data-factory-hdfs-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2195">For more information, see [HDFS connector](#data-factory-hdfs-connector.md#dataset-properties) article.</span></span> 

### <a name="file-system-source-in-copy-activity"></a><span data-ttu-id="cc737-2196">A másolási tevékenység rendszer forrás</span><span class="sxs-lookup"><span data-stu-id="cc737-2196">File System Source in Copy Activity</span></span>
<span data-ttu-id="cc737-2197">Adatok másolása a HDFS, állítsa be a **adatforrástípust** a másolási tevékenység **FileSystemSource**, és adja meg a következő tulajdonságokat a **forrás** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-2197">If you are copying data from HDFS, set the **source type** of the copy activity to **FileSystemSource**, and specify following properties in the **source** section:</span></span>

<span data-ttu-id="cc737-2198">**FileSystemSource** támogatja a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="cc737-2198">**FileSystemSource** supports the following properties:</span></span>

| <span data-ttu-id="cc737-2199">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2199">Property</span></span> | <span data-ttu-id="cc737-2200">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2200">Description</span></span> | <span data-ttu-id="cc737-2201">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-2201">Allowed values</span></span> | <span data-ttu-id="cc737-2202">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2202">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-2203">Rekurzív</span><span class="sxs-lookup"><span data-stu-id="cc737-2203">recursive</span></span> |<span data-ttu-id="cc737-2204">Azt jelzi, hogy az adatok olvasható rekurzív módon az almappák vagy csak a megadott mappát.</span><span class="sxs-lookup"><span data-stu-id="cc737-2204">Indicates whether the data is read recursively from the sub folders or only from the specified folder.</span></span> |<span data-ttu-id="cc737-2205">IGAZ, hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="cc737-2205">True, False (default)</span></span> |<span data-ttu-id="cc737-2206">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2206">No</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-2207">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2207">Example</span></span>

```json
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "HdfsToBlobCopy",
            "inputs": [{
                "name": "InputDataset"
            }],
            "outputs": [{
                "name": "OutputDataset"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

<span data-ttu-id="cc737-2208">További információkért lásd: [HDFS összekötő](#data-factory-hdfs-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2208">For more information, see [HDFS connector](#data-factory-hdfs-connector.md#copy-activity-properties) article.</span></span>

## <a name="sftp"></a><span data-ttu-id="cc737-2209">SFTP</span><span class="sxs-lookup"><span data-stu-id="cc737-2209">SFTP</span></span>


### <a name="linked-service"></a><span data-ttu-id="cc737-2210">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-2210">Linked service</span></span>
<span data-ttu-id="cc737-2211">Az SFTP meghatározásához társított szolgáltatás, állítsa be a **típus** a társított szolgáltatás **Sftp**, és adja meg a következő tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-2211">To define an SFTP linked service, set the **type** of the linked service to **Sftp**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="cc737-2212">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2212">Property</span></span> | <span data-ttu-id="cc737-2213">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2213">Description</span></span> | <span data-ttu-id="cc737-2214">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2214">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-2215">állomás</span><span class="sxs-lookup"><span data-stu-id="cc737-2215">host</span></span> | <span data-ttu-id="cc737-2216">Az SFTP-kiszolgáló neve vagy IP-címét.</span><span class="sxs-lookup"><span data-stu-id="cc737-2216">Name or IP address of the SFTP server.</span></span> |<span data-ttu-id="cc737-2217">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2217">Yes</span></span> |
| <span data-ttu-id="cc737-2218">port</span><span class="sxs-lookup"><span data-stu-id="cc737-2218">port</span></span> |<span data-ttu-id="cc737-2219">Port, amelyen az SFTP kiszolgáló figyel.</span><span class="sxs-lookup"><span data-stu-id="cc737-2219">Port on which the SFTP server is listening.</span></span> <span data-ttu-id="cc737-2220">Az alapértelmezett érték: 21.</span><span class="sxs-lookup"><span data-stu-id="cc737-2220">The default value is: 21</span></span> |<span data-ttu-id="cc737-2221">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2221">No</span></span> |
| <span data-ttu-id="cc737-2222">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="cc737-2222">authenticationType</span></span> |<span data-ttu-id="cc737-2223">Adja meg a hitelesítés típusát.</span><span class="sxs-lookup"><span data-stu-id="cc737-2223">Specify authentication type.</span></span> <span data-ttu-id="cc737-2224">Megengedett értékek: **alapvető**, **SshPublicKey**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2224">Allowed values: **Basic**, **SshPublicKey**.</span></span> <br><br> <span data-ttu-id="cc737-2225">Tekintse meg [használja az egyszerű hitelesítés](#using-basic-authentication) és [használatával SSH nyilvános kulcsos hitelesítés](#using-ssh-public-key-authentication) további tulajdonságokat és JSON-minták szakasz.</span><span class="sxs-lookup"><span data-stu-id="cc737-2225">Refer to [Using basic authentication](#using-basic-authentication) and [Using SSH public key authentication](#using-ssh-public-key-authentication) sections on more properties and JSON samples respectively.</span></span> |<span data-ttu-id="cc737-2226">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2226">Yes</span></span> |
| <span data-ttu-id="cc737-2227">skipHostKeyValidation</span><span class="sxs-lookup"><span data-stu-id="cc737-2227">skipHostKeyValidation</span></span> | <span data-ttu-id="cc737-2228">Adja meg, hogy a gazdagép kulcs ellenőrzésének kihagyására.</span><span class="sxs-lookup"><span data-stu-id="cc737-2228">Specify whether to skip host key validation.</span></span> | <span data-ttu-id="cc737-2229">Nem.</span><span class="sxs-lookup"><span data-stu-id="cc737-2229">No.</span></span> <span data-ttu-id="cc737-2230">Az alapértelmezett érték: hamis</span><span class="sxs-lookup"><span data-stu-id="cc737-2230">The default value: false</span></span> |
| <span data-ttu-id="cc737-2231">hostKeyFingerprint</span><span class="sxs-lookup"><span data-stu-id="cc737-2231">hostKeyFingerprint</span></span> | <span data-ttu-id="cc737-2232">Adja meg a gazdagép kulcs az ujjlenyomat.</span><span class="sxs-lookup"><span data-stu-id="cc737-2232">Specify the finger print of the host key.</span></span> | <span data-ttu-id="cc737-2233">Igen, ha a `skipHostKeyValidation` hamis értékre van állítva.</span><span class="sxs-lookup"><span data-stu-id="cc737-2233">Yes if the `skipHostKeyValidation` is set to false.</span></span>  |
| <span data-ttu-id="cc737-2234">gatewayName</span><span class="sxs-lookup"><span data-stu-id="cc737-2234">gatewayName</span></span> |<span data-ttu-id="cc737-2235">Az adatkezelési átjáró egy helyszíni SFTP-kiszolgálóhoz való csatlakozáshoz neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-2235">Name of the Data Management Gateway to connect to an on-premises SFTP server.</span></span> | <span data-ttu-id="cc737-2236">Igen, ha az adatok másolása egy helyszíni SFTP-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="cc737-2236">Yes if copying data from an on-premises SFTP server.</span></span> |
| <span data-ttu-id="cc737-2237">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="cc737-2237">encryptedCredential</span></span> | <span data-ttu-id="cc737-2238">Titkosított hitelesítő adatokat a SFTP-kiszolgálóhoz való hozzáféréshez.</span><span class="sxs-lookup"><span data-stu-id="cc737-2238">Encrypted credential to access the SFTP server.</span></span> <span data-ttu-id="cc737-2239">Automatikusan létrehozott Ha megadja az egyszerű hitelesítés (felhasználónév + jelszó) vagy az SshPublicKey hitelesítési (felhasználónév + titkos kulcs elérési útja vagy tartalom) másolása varázsló vagy a ClickOnce előugró párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="cc737-2239">Auto-generated when you specify basic authentication (username + password) or SshPublicKey authentication (username + private key path or content) in copy wizard or the ClickOnce popup dialog.</span></span> | <span data-ttu-id="cc737-2240">Nem.</span><span class="sxs-lookup"><span data-stu-id="cc737-2240">No.</span></span> <span data-ttu-id="cc737-2241">Csak akkor, ha az adatok másolása egy helyszíni SFTP kiszolgáló alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="cc737-2241">Apply only when copying data from an on-premises SFTP server.</span></span> |

#### <a name="example-using-basic-authentication"></a><span data-ttu-id="cc737-2242">Például: Alapszintű hitelesítést használ</span><span class="sxs-lookup"><span data-stu-id="cc737-2242">Example: Using basic authentication</span></span>

<span data-ttu-id="cc737-2243">Egyszerű hitelesítést használ, állítsa be `authenticationType` , `Basic`, és adja meg az SFTP összekötő általános néhányat a meglévők közül az utolsó szakaszban bemutatott mellett az alábbi tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="cc737-2243">To use basic authentication, set `authenticationType` as `Basic`, and specify the following properties besides the SFTP connector generic ones introduced in the last section:</span></span>

| <span data-ttu-id="cc737-2244">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2244">Property</span></span> | <span data-ttu-id="cc737-2245">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2245">Description</span></span> | <span data-ttu-id="cc737-2246">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2246">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-2247">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="cc737-2247">username</span></span> | <span data-ttu-id="cc737-2248">Felhasználó, aki hozzáfér az SFTP-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="cc737-2248">User who has access to the SFTP server.</span></span> |<span data-ttu-id="cc737-2249">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2249">Yes</span></span> |
| <span data-ttu-id="cc737-2250">jelszó</span><span class="sxs-lookup"><span data-stu-id="cc737-2250">password</span></span> | <span data-ttu-id="cc737-2251">A felhasználó (felhasználónév) jelszavát.</span><span class="sxs-lookup"><span data-stu-id="cc737-2251">Password for the user (username).</span></span> | <span data-ttu-id="cc737-2252">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2252">Yes</span></span> |

```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<SFTP server name or IP address>",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "password": "xxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-basic-authentication-with-encrypted-credential"></a><span data-ttu-id="cc737-2253">Példa: Egyszerű hitelesítést a titkosított hitelesítő adat **</span><span class="sxs-lookup"><span data-stu-id="cc737-2253">Example: Basic authentication with encrypted credential**</span></span>

```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<FTP server name or IP address>",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="using-ssh-public-key-authentication"></a><span data-ttu-id="cc737-2254">SSH nyilvános kulcsos hitelesítés használatával: **</span><span class="sxs-lookup"><span data-stu-id="cc737-2254">Using SSH public key authentication:**</span></span>

<span data-ttu-id="cc737-2255">Egyszerű hitelesítést használ, állítsa be `authenticationType` , `SshPublicKey`, és adja meg az SFTP összekötő általános néhányat a meglévők közül az utolsó szakaszban bemutatott mellett az alábbi tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="cc737-2255">To use basic authentication, set `authenticationType` as `SshPublicKey`, and specify the following properties besides the SFTP connector generic ones introduced in the last section:</span></span>

| <span data-ttu-id="cc737-2256">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2256">Property</span></span> | <span data-ttu-id="cc737-2257">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2257">Description</span></span> | <span data-ttu-id="cc737-2258">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2258">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-2259">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="cc737-2259">username</span></span> |<span data-ttu-id="cc737-2260">SFTP-kiszolgálóhoz hozzáféréssel rendelkező felhasználó</span><span class="sxs-lookup"><span data-stu-id="cc737-2260">User who has access to the SFTP server</span></span> |<span data-ttu-id="cc737-2261">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2261">Yes</span></span> |
| <span data-ttu-id="cc737-2262">privateKeyPath</span><span class="sxs-lookup"><span data-stu-id="cc737-2262">privateKeyPath</span></span> | <span data-ttu-id="cc737-2263">Adjon meg abszolút elérési útját a titkos kulcs fájlját, hogy az átjáró férhetnek hozzá.</span><span class="sxs-lookup"><span data-stu-id="cc737-2263">Specify absolute path to the private key file that gateway can access.</span></span> | <span data-ttu-id="cc737-2264">Adja meg a `privateKeyPath` vagy `privateKeyContent`.</span><span class="sxs-lookup"><span data-stu-id="cc737-2264">Specify either the `privateKeyPath` or `privateKeyContent`.</span></span> <br><br> <span data-ttu-id="cc737-2265">Csak akkor, ha az adatok másolása egy helyszíni SFTP kiszolgáló alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="cc737-2265">Apply only when copying data from an on-premises SFTP server.</span></span> |
| <span data-ttu-id="cc737-2266">privateKeyContent</span><span class="sxs-lookup"><span data-stu-id="cc737-2266">privateKeyContent</span></span> | <span data-ttu-id="cc737-2267">A titkos kulcs tartalmát, mivel a szerializált karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="cc737-2267">A serialized string of the private key content.</span></span> <span data-ttu-id="cc737-2268">A varázsló a titkos kulcsfájl olvashatja, és automatikusan bontsa ki a titkos kulcs tartalmát.</span><span class="sxs-lookup"><span data-stu-id="cc737-2268">The Copy Wizard can read the private key file and extract the private key content automatically.</span></span> <span data-ttu-id="cc737-2269">Ha minden egyéb eszköz/SDK használja, használja a privateKeyPath tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="cc737-2269">If you are using any other tool/SDK, use the privateKeyPath property instead.</span></span> | <span data-ttu-id="cc737-2270">Adja meg a `privateKeyPath` vagy `privateKeyContent`.</span><span class="sxs-lookup"><span data-stu-id="cc737-2270">Specify either the `privateKeyPath` or `privateKeyContent`.</span></span> |
| <span data-ttu-id="cc737-2271">hozzáférési kód</span><span class="sxs-lookup"><span data-stu-id="cc737-2271">passPhrase</span></span> | <span data-ttu-id="cc737-2272">Adja meg a pass kifejezést/jelszót a titkos kulcs visszafejtésére, ha a kulcs fájlját egy hozzáférési kódot védi.</span><span class="sxs-lookup"><span data-stu-id="cc737-2272">Specify the pass phrase/password to decrypt the private key if the key file is protected by a pass phrase.</span></span> | <span data-ttu-id="cc737-2273">Igen, ha a titkos kulcsfájl védik a hozzáférési kód.</span><span class="sxs-lookup"><span data-stu-id="cc737-2273">Yes if the private key file is protected by a pass phrase.</span></span> |

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyPath",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<FTP server name or IP address>",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyPath": "D:\\privatekey_openssh",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true,
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-sshpublickey-authentication-using-private-key-content"></a><span data-ttu-id="cc737-2274">Példa: Az SshPublicKey hitelesítés titkos kulcs tartalom **</span><span class="sxs-lookup"><span data-stu-id="cc737-2274">Example: SshPublicKey authentication using private key content**</span></span>

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyContent",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver.westus.cloudapp.azure.com",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyContent": "<base64 string of the private key content>",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true
        }
    }
}
```

<span data-ttu-id="cc737-2275">További információkért lásd: [SFTP összekötő](data-factory-sftp-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2275">For more information, see [SFTP connector](data-factory-sftp-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="cc737-2276">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-2276">Dataset</span></span>
<span data-ttu-id="cc737-2277">Adja meg az SFTP adatkészlethez, állítsa be a **típus** a DataSet **fájlmegosztási**, és adja meg az alábbi tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-2277">To define an SFTP dataset, set the **type** of the dataset to **FileShare**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="cc737-2278">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2278">Property</span></span> | <span data-ttu-id="cc737-2279">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2279">Description</span></span> | <span data-ttu-id="cc737-2280">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2280">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-2281">folderPath</span><span class="sxs-lookup"><span data-stu-id="cc737-2281">folderPath</span></span> |<span data-ttu-id="cc737-2282">Sub mappa elérési útját.</span><span class="sxs-lookup"><span data-stu-id="cc737-2282">Sub path to the folder.</span></span> <span data-ttu-id="cc737-2283">Használja az escape-karakter "\" a speciális karakterek a karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="cc737-2283">Use escape character ‘ \ ’ for special characters in the string.</span></span> <span data-ttu-id="cc737-2284">Lásd: [minta kapcsolódó szolgáltatás és az adatkészlet-definíciók](#sample-linked-service-and-dataset-definitions) példákat.</span><span class="sxs-lookup"><span data-stu-id="cc737-2284">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="cc737-2285">Ez a tulajdonság a kombinálhatja **partitionBy** szeretné, hogy a mappa elérési utak alapján szelet kezdő és záró dátum-idő.</span><span class="sxs-lookup"><span data-stu-id="cc737-2285">You can combine this property with **partitionBy** to have folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="cc737-2286">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2286">Yes</span></span> |
| <span data-ttu-id="cc737-2287">fileName</span><span class="sxs-lookup"><span data-stu-id="cc737-2287">fileName</span></span> |<span data-ttu-id="cc737-2288">Adja meg a fájl nevét a **folderPath** Ha azt szeretné, hogy a tábla egy adott fájlra a mappában.</span><span class="sxs-lookup"><span data-stu-id="cc737-2288">Specify the name of the file in the **folderPath** if you want the table to refer to a specific file in the folder.</span></span> <span data-ttu-id="cc737-2289">Ha nem ad meg ehhez a tulajdonsághoz értéket, a tábla a mappában lévő összes fájlt mutat.</span><span class="sxs-lookup"><span data-stu-id="cc737-2289">If you do not specify any value for this property, the table points to all files in the folder.</span></span><br/><br/><span data-ttu-id="cc737-2290">Ha nincs megadva fájlnév egy kimeneti adatkészletet, a létrehozott fájl nevét a következő lenne ebben a formátumban:</span><span class="sxs-lookup"><span data-stu-id="cc737-2290">When fileName is not specified for an output dataset, the name of the generated file would be in the following this format:</span></span> <br/><br/><span data-ttu-id="cc737-2291">Adatok. <Guid>.txt (Példa: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="cc737-2291">Data.<Guid>.txt (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="cc737-2292">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2292">No</span></span> |
| <span data-ttu-id="cc737-2293">fileFilter</span><span class="sxs-lookup"><span data-stu-id="cc737-2293">fileFilter</span></span> |<span data-ttu-id="cc737-2294">Adjon meg egy szűrőt, amely minden fájl helyett a fájlok Tárolónév részhalmazának kiválasztására szolgál.</span><span class="sxs-lookup"><span data-stu-id="cc737-2294">Specify a filter to be used to select a subset of files in the folderPath rather than all files.</span></span><br/><br/><span data-ttu-id="cc737-2295">Két érték engedélyezett: `*` (több karaktert) és `?` (egyetlen karakter).</span><span class="sxs-lookup"><span data-stu-id="cc737-2295">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="cc737-2296">1. példa:`"fileFilter": "*.log"`</span><span class="sxs-lookup"><span data-stu-id="cc737-2296">Examples 1: `"fileFilter": "*.log"`</span></span><br/><span data-ttu-id="cc737-2297">2. példa:`"fileFilter": 2016-1-?.txt"`</span><span class="sxs-lookup"><span data-stu-id="cc737-2297">Example 2: `"fileFilter": 2016-1-?.txt"`</span></span><br/><br/> <span data-ttu-id="cc737-2298">fileFilter is alkalmazható egy bemeneti fájlmegosztási az adatkészlethez.</span><span class="sxs-lookup"><span data-stu-id="cc737-2298">fileFilter is applicable for an input FileShare dataset.</span></span> <span data-ttu-id="cc737-2299">Ez a tulajdonság a HDFS nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="cc737-2299">This property is not supported with HDFS.</span></span> |<span data-ttu-id="cc737-2300">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2300">No</span></span> |
| <span data-ttu-id="cc737-2301">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="cc737-2301">partitionedBy</span></span> |<span data-ttu-id="cc737-2302">Adjon meg egy dinamikus folderPath idő adatsor fájlnevét partitionedBy használható.</span><span class="sxs-lookup"><span data-stu-id="cc737-2302">partitionedBy can be used to specify a dynamic folderPath, filename for time series data.</span></span> <span data-ttu-id="cc737-2303">Például folderPath adatok óránkénti paraméteres.</span><span class="sxs-lookup"><span data-stu-id="cc737-2303">For example, folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="cc737-2304">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2304">No</span></span> |
| <span data-ttu-id="cc737-2305">Formátumban</span><span class="sxs-lookup"><span data-stu-id="cc737-2305">format</span></span> | <span data-ttu-id="cc737-2306">A következő formátumban típusok támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2306">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="cc737-2307">Állítsa be a **típus** tulajdonság a formátuma a következő értékek egyikét.</span><span class="sxs-lookup"><span data-stu-id="cc737-2307">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="cc737-2308">További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="cc737-2308">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="cc737-2309">Ha azt szeretné, hogy **másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a Formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban.</span><span class="sxs-lookup"><span data-stu-id="cc737-2309">If you want to **copy files as-is** between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="cc737-2310">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2310">No</span></span> |
| <span data-ttu-id="cc737-2311">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="cc737-2311">compression</span></span> | <span data-ttu-id="cc737-2312">Adja meg a típus és az adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="cc737-2312">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="cc737-2313">Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2313">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="cc737-2314">Támogatott szintek a következők: **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2314">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="cc737-2315">További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="cc737-2315">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="cc737-2316">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2316">No</span></span> |
| <span data-ttu-id="cc737-2317">useBinaryTransfer</span><span class="sxs-lookup"><span data-stu-id="cc737-2317">useBinaryTransfer</span></span> |<span data-ttu-id="cc737-2318">Adja meg, hogy a bináris átviteli mód használata.</span><span class="sxs-lookup"><span data-stu-id="cc737-2318">Specify whether use Binary transfer mode.</span></span> <span data-ttu-id="cc737-2319">A bináris mód és a hamis értéket ASCII igaz.</span><span class="sxs-lookup"><span data-stu-id="cc737-2319">True for binary mode and false ASCII.</span></span> <span data-ttu-id="cc737-2320">Alapértelmezett érték: igaz.</span><span class="sxs-lookup"><span data-stu-id="cc737-2320">Default value: True.</span></span> <span data-ttu-id="cc737-2321">A tulajdonság csak akkor használható, típusú társított kapcsolódószolgáltatás-típus esetén: FTP-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="cc737-2321">This property can only be used when associated linked service type is of type: FtpServer.</span></span> |<span data-ttu-id="cc737-2322">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2322">No</span></span> |

> [!NOTE]
> <span data-ttu-id="cc737-2323">fájlnév és fileFilter nem használható egyszerre.</span><span class="sxs-lookup"><span data-stu-id="cc737-2323">filename and fileFilter cannot be used simultaneously.</span></span>

#### <a name="example"></a><span data-ttu-id="cc737-2324">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2324">Example</span></span>

```json
{
    "name": "SFTPFileInput",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "SftpLinkedService",
        "typeProperties": {
            "folderPath": "<path to shared folder>",
            "fileName": "test.csv"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="cc737-2325">További információkért lásd: [SFTP összekötő](data-factory-sftp-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2325">For more information, see [SFTP connector](data-factory-sftp-connector.md#dataset-properties) article.</span></span> 

### <a name="file-system-source-in-copy-activity"></a><span data-ttu-id="cc737-2326">A másolási tevékenység rendszer forrás</span><span class="sxs-lookup"><span data-stu-id="cc737-2326">File System Source in Copy Activity</span></span>
<span data-ttu-id="cc737-2327">SFTP forrásból származó adatok másolása, állítsa be a **adatforrástípust** a másolási tevékenység **FileSystemSource**, és adja meg a következő tulajdonságokat a **forrás** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-2327">If you are copying data from an SFTP source, set the **source type** of the copy activity to **FileSystemSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="cc737-2328">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2328">Property</span></span> | <span data-ttu-id="cc737-2329">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2329">Description</span></span> | <span data-ttu-id="cc737-2330">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-2330">Allowed values</span></span> | <span data-ttu-id="cc737-2331">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2331">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-2332">Rekurzív</span><span class="sxs-lookup"><span data-stu-id="cc737-2332">recursive</span></span> |<span data-ttu-id="cc737-2333">Azt jelzi, hogy az adatok olvasható rekurzív módon az almappák vagy csak a megadott mappát.</span><span class="sxs-lookup"><span data-stu-id="cc737-2333">Indicates whether the data is read recursively from the sub folders or only from the specified folder.</span></span> |<span data-ttu-id="cc737-2334">IGAZ, hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="cc737-2334">True, False (default)</span></span> |<span data-ttu-id="cc737-2335">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2335">No</span></span> |



#### <a name="example"></a><span data-ttu-id="cc737-2336">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2336">Example</span></span>

```json
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "SFTPToBlobCopy",
            "inputs": [{
                "name": "SFTPFileInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2017-02-20T18:00:00",
        "end": "2017-02-20T19:00:00"
    }
}
```

<span data-ttu-id="cc737-2337">További információkért lásd: [SFTP összekötő](data-factory-sftp-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2337">For more information, see [SFTP connector](data-factory-sftp-connector.md#copy-activity-properties) article.</span></span>


## <a name="http"></a><span data-ttu-id="cc737-2338">HTTP</span><span class="sxs-lookup"><span data-stu-id="cc737-2338">HTTP</span></span>

### <a name="linked-service"></a><span data-ttu-id="cc737-2339">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-2339">Linked service</span></span>
<span data-ttu-id="cc737-2340">Egy HTTP meghatározásához társított szolgáltatás, állítsa be a **típus** a társított szolgáltatás **Http**, és adja meg a következő tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-2340">To define a HTTP linked service, set the **type** of the linked service to **Http**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="cc737-2341">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2341">Property</span></span> | <span data-ttu-id="cc737-2342">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2342">Description</span></span> | <span data-ttu-id="cc737-2343">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2343">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-2344">URL-címe</span><span class="sxs-lookup"><span data-stu-id="cc737-2344">url</span></span> | <span data-ttu-id="cc737-2345">A webkiszolgáló alap URL-címe</span><span class="sxs-lookup"><span data-stu-id="cc737-2345">Base URL to the Web Server</span></span> | <span data-ttu-id="cc737-2346">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2346">Yes</span></span> |
| <span data-ttu-id="cc737-2347">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="cc737-2347">authenticationType</span></span> | <span data-ttu-id="cc737-2348">Megadja a hitelesítési típus.</span><span class="sxs-lookup"><span data-stu-id="cc737-2348">Specifies the authentication type.</span></span> <span data-ttu-id="cc737-2349">Két érték engedélyezett: **névtelen**, **alapvető**, **kivonatoló**, **Windows**, **ClientCertificate**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2349">Allowed values are: **Anonymous**, **Basic**, **Digest**, **Windows**, **ClientCertificate**.</span></span> <br><br> <span data-ttu-id="cc737-2350">Tekintse meg a további tulajdonságok és adott hitelesítési típusok JSON-példák a táblázat alatti részek kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="cc737-2350">Refer to sections below this table on more properties and JSON samples for those authentication types respectively.</span></span> | <span data-ttu-id="cc737-2351">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2351">Yes</span></span> |
| <span data-ttu-id="cc737-2352">enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="cc737-2352">enableServerCertificateValidation</span></span> | <span data-ttu-id="cc737-2353">Adja meg, hogy a kiszolgálói SSL-tanúsítvány hitelesítése engedélyezése, ha a forrás HTTPS webkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="cc737-2353">Specify whether to enable server SSL certificate validation if source is HTTPS Web Server</span></span> | <span data-ttu-id="cc737-2354">Nem, alapértelmezett érték true</span><span class="sxs-lookup"><span data-stu-id="cc737-2354">No, default is true</span></span> |
| <span data-ttu-id="cc737-2355">gatewayName</span><span class="sxs-lookup"><span data-stu-id="cc737-2355">gatewayName</span></span> | <span data-ttu-id="cc737-2356">Neve az adatkezelési átjáró HTTP a helyszíni adatforráshoz kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="cc737-2356">Name of the Data Management Gateway to connect to an on-premises HTTP source.</span></span> | <span data-ttu-id="cc737-2357">Igen, ha a helyszíni HTTP forrásból származó adatok másolása.</span><span class="sxs-lookup"><span data-stu-id="cc737-2357">Yes if copying data from an on-premises HTTP source.</span></span> |
| <span data-ttu-id="cc737-2358">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="cc737-2358">encryptedCredential</span></span> | <span data-ttu-id="cc737-2359">Titkosított hitelesítő adatokat a HTTP-végpont elérésére.</span><span class="sxs-lookup"><span data-stu-id="cc737-2359">Encrypted credential to access the HTTP endpoint.</span></span> <span data-ttu-id="cc737-2360">Automatikusan létrehozott másolása varázsló vagy a ClickOnce felugró párbeszédpanel a hitelesítő adatok konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="cc737-2360">Auto-generated when you configure the authentication information in copy wizard or the ClickOnce popup dialog.</span></span> | <span data-ttu-id="cc737-2361">Nem.</span><span class="sxs-lookup"><span data-stu-id="cc737-2361">No.</span></span> <span data-ttu-id="cc737-2362">Csak akkor, ha az adatok másolása helyi HTTP-kiszolgáló alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="cc737-2362">Apply only when copying data from an on-premises HTTP server.</span></span> |

#### <a name="example-using-basic-digest-or-windows-authentication"></a><span data-ttu-id="cc737-2363">Példa: Basic, a kivonatoló vagy a Windows-hitelesítés használatával</span><span class="sxs-lookup"><span data-stu-id="cc737-2363">Example: Using Basic, Digest, or Windows authentication</span></span>
<span data-ttu-id="cc737-2364">Állítsa be `authenticationType` , `Basic`, `Digest`, vagy `Windows`, és adja meg a HTTP-összekötő fent bevezetett általános ők mellett az alábbi tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="cc737-2364">Set `authenticationType` as `Basic`, `Digest`, or `Windows`, and specify the following properties besides the HTTP connector generic ones introduced above:</span></span>

| <span data-ttu-id="cc737-2365">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2365">Property</span></span> | <span data-ttu-id="cc737-2366">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2366">Description</span></span> | <span data-ttu-id="cc737-2367">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2367">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-2368">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="cc737-2368">username</span></span> | <span data-ttu-id="cc737-2369">Felhasználónév a HTTP-végpont elérésére.</span><span class="sxs-lookup"><span data-stu-id="cc737-2369">Username to access the HTTP endpoint.</span></span> | <span data-ttu-id="cc737-2370">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2370">Yes</span></span> |
| <span data-ttu-id="cc737-2371">jelszó</span><span class="sxs-lookup"><span data-stu-id="cc737-2371">password</span></span> | <span data-ttu-id="cc737-2372">A felhasználó (felhasználónév) jelszavát.</span><span class="sxs-lookup"><span data-stu-id="cc737-2372">Password for the user (username).</span></span> | <span data-ttu-id="cc737-2373">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2373">Yes</span></span> |

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "Http",
        "typeProperties": {
            "authenticationType": "basic",
            "url": "https://en.wikipedia.org/wiki/",
            "userName": "user name",
            "password": "password"
        }
    }
}
```

#### <a name="example-using-clientcertificate-authentication"></a><span data-ttu-id="cc737-2374">Példa: ClientCertificate hitelesítés használatával</span><span class="sxs-lookup"><span data-stu-id="cc737-2374">Example: Using ClientCertificate authentication</span></span>

<span data-ttu-id="cc737-2375">Egyszerű hitelesítést használ, állítsa be `authenticationType` , `ClientCertificate`, és adja meg a HTTP-összekötő fent bevezetett általános ők mellett az alábbi tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="cc737-2375">To use basic authentication, set `authenticationType` as `ClientCertificate`, and specify the following properties besides the HTTP connector generic ones introduced above:</span></span>

| <span data-ttu-id="cc737-2376">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2376">Property</span></span> | <span data-ttu-id="cc737-2377">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2377">Description</span></span> | <span data-ttu-id="cc737-2378">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2378">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-2379">embeddedCertData</span><span class="sxs-lookup"><span data-stu-id="cc737-2379">embeddedCertData</span></span> | <span data-ttu-id="cc737-2380">A személyes információcseréhez kapcsolódó (PFX) fájl bináris adatok Base64-kódolású tartalmát.</span><span class="sxs-lookup"><span data-stu-id="cc737-2380">The Base64-encoded contents of binary data of the Personal Information Exchange (PFX) file.</span></span> | <span data-ttu-id="cc737-2381">Adja meg a `embeddedCertData` vagy `certThumbprint`.</span><span class="sxs-lookup"><span data-stu-id="cc737-2381">Specify either the `embeddedCertData` or `certThumbprint`.</span></span> |
| <span data-ttu-id="cc737-2382">CertThumbprint</span><span class="sxs-lookup"><span data-stu-id="cc737-2382">certThumbprint</span></span> | <span data-ttu-id="cc737-2383">A tanúsítványtároló átjáró számítógépre telepített tanúsítvány ujjlenyomatát.</span><span class="sxs-lookup"><span data-stu-id="cc737-2383">The thumbprint of the certificate that was installed on your gateway machine’s cert store.</span></span> <span data-ttu-id="cc737-2384">Csak akkor, ha a helyszíni HTTP forrásból származó adat másolása alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="cc737-2384">Apply only when copying data from an on-premises HTTP source.</span></span> | <span data-ttu-id="cc737-2385">Adja meg a `embeddedCertData` vagy `certThumbprint`.</span><span class="sxs-lookup"><span data-stu-id="cc737-2385">Specify either the `embeddedCertData` or `certThumbprint`.</span></span> |
| <span data-ttu-id="cc737-2386">jelszó</span><span class="sxs-lookup"><span data-stu-id="cc737-2386">password</span></span> | <span data-ttu-id="cc737-2387">A tanúsítványhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="cc737-2387">Password associated with the certificate.</span></span> | <span data-ttu-id="cc737-2388">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2388">No</span></span> |

<span data-ttu-id="cc737-2389">Ha `certThumbprint` hitelesítés és a tanúsítvány telepítése a helyi számítógép személyes tanúsítványokat tartalmazó tárolójában kell az olvasási engedélyt az átjárószolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="cc737-2389">If you use `certThumbprint` for authentication and the certificate is installed in the personal store of the local computer, you need to grant the read permission to the gateway service:</span></span>

1. <span data-ttu-id="cc737-2390">Indítsa el a Microsoft Management Console (MMC).</span><span class="sxs-lookup"><span data-stu-id="cc737-2390">Launch Microsoft Management Console (MMC).</span></span> <span data-ttu-id="cc737-2391">Adja hozzá a **tanúsítványok** beépülő modul, amelynek célpontja a **helyi számítógép**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2391">Add the **Certificates** snap-in that targets the **Local Computer**.</span></span>
2. <span data-ttu-id="cc737-2392">Bontsa ki a **tanúsítványok**, **személyes**, és kattintson a **tanúsítványok**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2392">Expand **Certificates**, **Personal**, and click **Certificates**.</span></span>
3. <span data-ttu-id="cc737-2393">Kattintson a jobb gombbal a tanúsítványt a személyes tárolóba, és válassza ki **feladataival**->**titkos kulcsok kezelése...**</span><span class="sxs-lookup"><span data-stu-id="cc737-2393">Right-click the certificate from the personal store, and select **All Tasks**->**Manage Private Keys...**</span></span>
3. <span data-ttu-id="cc737-2394">Az a **biztonsági** lapon maradva adja hozzá a felhasználói fiók, amely alatt az adatkezelési átjáró gazdaszolgáltatása fut az olvasási joggal rendelkező tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="cc737-2394">On the **Security** tab, add the user account under which Data Management Gateway Host Service is running with the read access to the certificate.</span></span>  

<span data-ttu-id="cc737-2395">**Példa: ügyfél-tanúsítványt használ:** ez kapcsolódó szolgáltatás hivatkozások a data factory egy helyszíni HTTP-webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="cc737-2395">**Example: using client certificate:** This linked service links your data factory to an on-premises HTTP web server.</span></span> <span data-ttu-id="cc737-2396">Az adatkezelési átjáró telepítve a számítógépen telepített ügyféltanúsítványt használ.</span><span class="sxs-lookup"><span data-stu-id="cc737-2396">It uses a client certificate that is installed on the machine with Data Management Gateway installed.</span></span>

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "Http",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "certThumbprint": "thumbprint of certificate",
            "gatewayName": "gateway name"
        }
    }
}
```

#### <a name="example-using-client-certificate-in-a-file"></a><span data-ttu-id="cc737-2397">Példa: ügyfél-tanúsítványt használ egy fájlban</span><span class="sxs-lookup"><span data-stu-id="cc737-2397">Example: using client certificate in a file</span></span>
<span data-ttu-id="cc737-2398">A kapcsolódó szolgáltatás hivatkozások a data factory egy helyszíni HTTP-webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="cc737-2398">This linked service links your data factory to an on-premises HTTP web server.</span></span> <span data-ttu-id="cc737-2399">Az adatkezelési átjáró telepítése egy ügyfél tanúsítványfájlt, a gép használ.</span><span class="sxs-lookup"><span data-stu-id="cc737-2399">It uses a client certificate file on the machine with Data Management Gateway installed.</span></span>

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "Http",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "embeddedCertData": "base64 encoded cert data",
            "password": "password of cert"
        }
    }
}
```

<span data-ttu-id="cc737-2400">További információkért lásd: [HTTP összekötő](data-factory-http-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2400">For more information, see [HTTP connector](data-factory-http-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="cc737-2401">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-2401">Dataset</span></span>
<span data-ttu-id="cc737-2402">Adja meg a HTTP-adatkészletet, állítsa be a **típus** a DataSet **Http**, és adja meg az alábbi tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-2402">To define a HTTP dataset, set the **type** of the dataset to **Http**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="cc737-2403">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2403">Property</span></span> | <span data-ttu-id="cc737-2404">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2404">Description</span></span> | <span data-ttu-id="cc737-2405">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2405">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="cc737-2406">relativeUrl</span><span class="sxs-lookup"><span data-stu-id="cc737-2406">relativeUrl</span></span> | <span data-ttu-id="cc737-2407">Az erőforrás adatokat tartalmazó relatív URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="cc737-2407">A relative URL to the resource that contains the data.</span></span> <span data-ttu-id="cc737-2408">Ha nincs megadva, csak a megadott URL-cím a társított szolgáltatás definíciójának használja.</span><span class="sxs-lookup"><span data-stu-id="cc737-2408">When path is not specified, only the URL specified in the linked service definition is used.</span></span> <br><br> <span data-ttu-id="cc737-2409">Dinamikus URL-cím létrehozásához használható [adat-előállító funkciók és rendszerváltozók](data-factory-functions-variables.md), például: `"relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)"`.</span><span class="sxs-lookup"><span data-stu-id="cc737-2409">To construct dynamic URL, you can use [Data Factory functions and system variables](data-factory-functions-variables.md), Example: `"relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)"`.</span></span> | <span data-ttu-id="cc737-2410">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2410">No</span></span> |
| <span data-ttu-id="cc737-2411">requestMethod</span><span class="sxs-lookup"><span data-stu-id="cc737-2411">requestMethod</span></span> | <span data-ttu-id="cc737-2412">HTTP-metódus.</span><span class="sxs-lookup"><span data-stu-id="cc737-2412">Http method.</span></span> <span data-ttu-id="cc737-2413">Két érték engedélyezett **beolvasása** vagy **POST**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2413">Allowed values are **GET** or **POST**.</span></span> | <span data-ttu-id="cc737-2414">Nem.</span><span class="sxs-lookup"><span data-stu-id="cc737-2414">No.</span></span> <span data-ttu-id="cc737-2415">Alapértelmezett érték a `GET`.</span><span class="sxs-lookup"><span data-stu-id="cc737-2415">Default is `GET`.</span></span> |
| <span data-ttu-id="cc737-2416">additionalHeaders</span><span class="sxs-lookup"><span data-stu-id="cc737-2416">additionalHeaders</span></span> | <span data-ttu-id="cc737-2417">További HTTP-kérelemfejlécekben.</span><span class="sxs-lookup"><span data-stu-id="cc737-2417">Additional HTTP request headers.</span></span> | <span data-ttu-id="cc737-2418">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2418">No</span></span> |
| <span data-ttu-id="cc737-2419">requestBody</span><span class="sxs-lookup"><span data-stu-id="cc737-2419">requestBody</span></span> | <span data-ttu-id="cc737-2420">A HTTP-kérelmek törzsében.</span><span class="sxs-lookup"><span data-stu-id="cc737-2420">Body for HTTP request.</span></span> | <span data-ttu-id="cc737-2421">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2421">No</span></span> |
| <span data-ttu-id="cc737-2422">Formátumban</span><span class="sxs-lookup"><span data-stu-id="cc737-2422">format</span></span> | <span data-ttu-id="cc737-2423">Ha azt szeretné, hogy egyszerűen **lekérik az adatokat, HTTP-végpont-van** nélkül elemzés azt, hagyja ki a formátumot beállítások.</span><span class="sxs-lookup"><span data-stu-id="cc737-2423">If you want to simply **retrieve the data from HTTP endpoint as-is** without parsing it, skip this format settings.</span></span> <br><br> <span data-ttu-id="cc737-2424">Ha azt szeretné, a HTTP-válasz tartalom elemzése során másolása, a következő formátumban típusok támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2424">If you want to parse the HTTP response content during copy, the following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="cc737-2425">További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="cc737-2425">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> |<span data-ttu-id="cc737-2426">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2426">No</span></span> |
| <span data-ttu-id="cc737-2427">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="cc737-2427">compression</span></span> | <span data-ttu-id="cc737-2428">Adja meg a típus és az adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="cc737-2428">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="cc737-2429">Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2429">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="cc737-2430">Támogatott szintek a következők: **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2430">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="cc737-2431">További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="cc737-2431">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="cc737-2432">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2432">No</span></span> |

#### <a name="example-using-the-get-default-method"></a><span data-ttu-id="cc737-2433">Példa: a GET (alapértelmezett) metódussal</span><span class="sxs-lookup"><span data-stu-id="cc737-2433">Example: using the GET (default) method</span></span>

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "XXX/test.xml",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

#### <a name="example-using-the-post-method"></a><span data-ttu-id="cc737-2434">Példa: a POST metódussal</span><span class="sxs-lookup"><span data-stu-id="cc737-2434">Example: using the POST method</span></span>

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "/XXX/test.xml",
            "requestMethod": "Post",
            "requestBody": "body for POST HTTP request"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```
<span data-ttu-id="cc737-2435">További információkért lásd: [HTTP összekötő](data-factory-http-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2435">For more information, see [HTTP connector](data-factory-http-connector.md#dataset-properties) article.</span></span>

### <a name="http-source-in-copy-activity"></a><span data-ttu-id="cc737-2436">A másolási tevékenység HTTP-forrás</span><span class="sxs-lookup"><span data-stu-id="cc737-2436">HTTP Source in Copy Activity</span></span>
<span data-ttu-id="cc737-2437">Adatok másolása egy HTTP-bejegyzéseket, állítsa be a **adatforrástípust** a másolási tevékenység **HttpSource**, és adja meg a következő tulajdonságokat a **forrás** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-2437">If you are copying data from a HTTP source, set the **source type** of the copy activity to **HttpSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="cc737-2438">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2438">Property</span></span> | <span data-ttu-id="cc737-2439">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2439">Description</span></span> | <span data-ttu-id="cc737-2440">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2440">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="cc737-2441">httpRequestTimeout</span><span class="sxs-lookup"><span data-stu-id="cc737-2441">httpRequestTimeout</span></span> | <span data-ttu-id="cc737-2442">Időtúllépés (időtartam) válaszol a HTTP-kérelem.</span><span class="sxs-lookup"><span data-stu-id="cc737-2442">The timeout (TimeSpan) for the HTTP request to get a response.</span></span> <span data-ttu-id="cc737-2443">Az időtúllépés is válaszol, nem lehet olvasni a válasz adatokat időtúllépés.</span><span class="sxs-lookup"><span data-stu-id="cc737-2443">It is the timeout to get a response, not the timeout to read response data.</span></span> | <span data-ttu-id="cc737-2444">Nem.</span><span class="sxs-lookup"><span data-stu-id="cc737-2444">No.</span></span> <span data-ttu-id="cc737-2445">Alapértelmezett érték: 00:01:40</span><span class="sxs-lookup"><span data-stu-id="cc737-2445">Default value: 00:01:40</span></span> |


#### <a name="example"></a><span data-ttu-id="cc737-2446">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2446">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "HttpSourceToAzureBlob",
            "description": "Copy from an HTTP source to an Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "HttpSourceDataInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "HttpSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="cc737-2447">További információkért lásd: [HTTP összekötő](data-factory-http-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2447">For more information, see [HTTP connector](data-factory-http-connector.md#copy-activity-properties) article.</span></span>

## <a name="odata"></a><span data-ttu-id="cc737-2448">OData</span><span class="sxs-lookup"><span data-stu-id="cc737-2448">OData</span></span>

### <a name="linked-service"></a><span data-ttu-id="cc737-2449">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-2449">Linked service</span></span>
<span data-ttu-id="cc737-2450">Egy OData meghatározásához társított szolgáltatás, állítsa be a **típus** a társított szolgáltatás **OData**, és adja meg a következő tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-2450">To define an OData linked service, set the **type** of the linked service to **OData**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="cc737-2451">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2451">Property</span></span> | <span data-ttu-id="cc737-2452">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2452">Description</span></span> | <span data-ttu-id="cc737-2453">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2453">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-2454">URL-címe</span><span class="sxs-lookup"><span data-stu-id="cc737-2454">url</span></span> |<span data-ttu-id="cc737-2455">Az OData szolgáltatási URL-címét.</span><span class="sxs-lookup"><span data-stu-id="cc737-2455">Url of the OData service.</span></span> |<span data-ttu-id="cc737-2456">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2456">Yes</span></span> |
| <span data-ttu-id="cc737-2457">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="cc737-2457">authenticationType</span></span> |<span data-ttu-id="cc737-2458">Az OData-forrásra való kapcsolódáshoz használt hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="cc737-2458">Type of authentication used to connect to the OData source.</span></span> <br/><br/> <span data-ttu-id="cc737-2459">A felhőbeli OData a lehetséges értékek: névtelen, alapszintű és OAuth (Megjegyzés: jelenleg csak Azure Data Factory támogatási Azure Active Directory-alapú OAuth).</span><span class="sxs-lookup"><span data-stu-id="cc737-2459">For cloud OData, possible values are Anonymous, Basic, and OAuth (note Azure Data Factory currently only support Azure Active Directory based OAuth).</span></span> <br/><br/> <span data-ttu-id="cc737-2460">A helyszíni OData lehetséges értékei a névtelen, alapszintű és a Windows.</span><span class="sxs-lookup"><span data-stu-id="cc737-2460">For on-premises OData, possible values are Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="cc737-2461">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2461">Yes</span></span> |
| <span data-ttu-id="cc737-2462">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="cc737-2462">username</span></span> |<span data-ttu-id="cc737-2463">Ha egyszerű hitelesítést használ, adja meg a felhasználónevet.</span><span class="sxs-lookup"><span data-stu-id="cc737-2463">Specify user name if you are using Basic authentication.</span></span> |<span data-ttu-id="cc737-2464">Igen (Ha csak az egyszerű hitelesítés használata esetén)</span><span class="sxs-lookup"><span data-stu-id="cc737-2464">Yes (only if you are using Basic authentication)</span></span> |
| <span data-ttu-id="cc737-2465">jelszó</span><span class="sxs-lookup"><span data-stu-id="cc737-2465">password</span></span> |<span data-ttu-id="cc737-2466">Adja meg a felhasználónévhez megadott felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="cc737-2466">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="cc737-2467">Igen (Ha csak az egyszerű hitelesítés használata esetén)</span><span class="sxs-lookup"><span data-stu-id="cc737-2467">Yes (only if you are using Basic authentication)</span></span> |
| <span data-ttu-id="cc737-2468">authorizedCredential</span><span class="sxs-lookup"><span data-stu-id="cc737-2468">authorizedCredential</span></span> |<span data-ttu-id="cc737-2469">Ha OAuth használ, kattintson a **engedélyezés** gombra a Data Factory másolása varázsló vagy a szerkesztőben, majd adja meg a hitelesítő adatok, akkor ez a tulajdonság értékének lesz automatikusan generált.</span><span class="sxs-lookup"><span data-stu-id="cc737-2469">If you are using OAuth, click **Authorize** button in the Data Factory Copy Wizard or Editor and enter your credential, then the value of this property will be auto-generated.</span></span> |<span data-ttu-id="cc737-2470">Igen (csak ha OAuth-hitelesítés használata esetén)</span><span class="sxs-lookup"><span data-stu-id="cc737-2470">Yes (only if you are using OAuth authentication)</span></span> |
| <span data-ttu-id="cc737-2471">gatewayName</span><span class="sxs-lookup"><span data-stu-id="cc737-2471">gatewayName</span></span> |<span data-ttu-id="cc737-2472">Az átjáró, amely a Data Factory szolgáltatásnak csatlakoznia a helyszíni OData-szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-2472">Name of the gateway that the Data Factory service should use to connect to the on-premises OData service.</span></span> <span data-ttu-id="cc737-2473">Csak adja meg, ha a másolt adatokat a helyszíni OData-forrásra.</span><span class="sxs-lookup"><span data-stu-id="cc737-2473">Specify only if you are copying data from on-prem OData source.</span></span> |<span data-ttu-id="cc737-2474">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2474">No</span></span> |

#### <a name="example---using-basic-authentication"></a><span data-ttu-id="cc737-2475">Példa – egyszerű hitelesítés használata</span><span class="sxs-lookup"><span data-stu-id="cc737-2475">Example - Using Basic authentication</span></span>
```json
{
    "name": "inputLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Basic",
            "username": "username",
            "password": "password"
        }
    }
}
```

#### <a name="example---using-anonymous-authentication"></a><span data-ttu-id="cc737-2476">Példa - névtelen hitelesítés használatával</span><span class="sxs-lookup"><span data-stu-id="cc737-2476">Example - Using Anonymous authentication</span></span>

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
        }
    }
}
```

#### <a name="example---using-windows-authentication-accessing-on-premises-odata-source"></a><span data-ttu-id="cc737-2477">Példa - használatával Windows hitelesítés használata a helyszíni OData-forrásra</span><span class="sxs-lookup"><span data-stu-id="cc737-2477">Example - Using Windows authentication accessing on-premises OData source</span></span>

```json
{
    "name": "inputLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "<endpoint of on-premises OData source, for example, Dynamics CRM>",
            "authenticationType": "Windows",
            "username": "domain\\user",
            "password": "password",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example---using-oauth-authentication-accessing-cloud-odata-source"></a><span data-ttu-id="cc737-2478">Példa - felhő OData-forrásra elérése OAuth-hitelesítés használatával</span><span class="sxs-lookup"><span data-stu-id="cc737-2478">Example - Using OAuth authentication accessing cloud OData source</span></span>
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "<endpoint of cloud OData source, for example, https://<tenant>.crm.dynamics.com/XRMServices/2011/OrganizationData.svc>",
            "authenticationType": "OAuth",
            "authorizedCredential": "<auto generated by clicking the Authorize button on UI>"
        }
    }
}
```

<span data-ttu-id="cc737-2479">További információkért lásd: [OData összekötő](data-factory-odata-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2479">For more information, see [OData connector](data-factory-odata-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="cc737-2480">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-2480">Dataset</span></span>
<span data-ttu-id="cc737-2481">Adja meg egy OData-adatkészlet, állítsa be a **típus** a DataSet **ODataResource**, és adja meg az alábbi tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-2481">To define an OData dataset, set the **type** of the dataset to **ODataResource**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="cc737-2482">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2482">Property</span></span> | <span data-ttu-id="cc737-2483">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2483">Description</span></span> | <span data-ttu-id="cc737-2484">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2484">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-2485">Elérési út</span><span class="sxs-lookup"><span data-stu-id="cc737-2485">path</span></span> |<span data-ttu-id="cc737-2486">Az OData-erőforrás elérési útja</span><span class="sxs-lookup"><span data-stu-id="cc737-2486">Path to the OData resource</span></span> |<span data-ttu-id="cc737-2487">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2487">No</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-2488">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2488">Example</span></span>

```json
{
    "name": "ODataDataset",
    "properties": {
        "type": "ODataResource",
        "typeProperties": {
            "path": "Products"
        },
        "linkedServiceName": "ODataLinkedService",
        "structure": [],
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
        }
    }
}
```

<span data-ttu-id="cc737-2489">További információkért lásd: [OData összekötő](data-factory-odata-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2489">For more information, see [OData connector](data-factory-odata-connector.md#dataset-properties) article.</span></span>

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="cc737-2490">A másolási tevékenység relációs adatforrás</span><span class="sxs-lookup"><span data-stu-id="cc737-2490">Relational Source in Copy Activity</span></span>
<span data-ttu-id="cc737-2491">OData forrásból származó adatok másolása, állítsa be a **adatforrástípust** a másolási tevékenység **RelationalSource**, és adja meg a következő tulajdonságokat a **forrás** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-2491">If you are copying data from an OData source, set the **source type** of the copy activity to **RelationalSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="cc737-2492">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2492">Property</span></span> | <span data-ttu-id="cc737-2493">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2493">Description</span></span> | <span data-ttu-id="cc737-2494">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2494">Example</span></span> | <span data-ttu-id="cc737-2495">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2495">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-2496">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="cc737-2496">query</span></span> |<span data-ttu-id="cc737-2497">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="cc737-2497">Use the custom query to read data.</span></span> |<span data-ttu-id="cc737-2498">"? $select neve, leírása és $top = = 5"</span><span class="sxs-lookup"><span data-stu-id="cc737-2498">"?$select=Name, Description&$top=5"</span></span> |<span data-ttu-id="cc737-2499">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2499">No</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-2500">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2500">Example</span></span>

```json
{
    "name": "CopyODataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "?$select=Name, Description&$top=5"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "ODataDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobODataDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "ODataToBlob"
        }],
        "start": "2017-02-01T18:00:00",
        "end": "2017-02-03T19:00:00"
    }
}
```

<span data-ttu-id="cc737-2501">További információkért lásd: [OData összekötő](data-factory-odata-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2501">For more information, see [OData connector](data-factory-odata-connector.md#copy-activity-properties) article.</span></span>


## <a name="odbc"></a><span data-ttu-id="cc737-2502">ODBC</span><span class="sxs-lookup"><span data-stu-id="cc737-2502">ODBC</span></span>


### <a name="linked-service"></a><span data-ttu-id="cc737-2503">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-2503">Linked service</span></span>
<span data-ttu-id="cc737-2504">Az ODBC meghatározásához társított szolgáltatás, állítsa be a **típus** a társított szolgáltatás **OnPremisesOdbc**, és adja meg a következő tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-2504">To define an ODBC linked service, set the **type** of the linked service to **OnPremisesOdbc**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="cc737-2505">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2505">Property</span></span> | <span data-ttu-id="cc737-2506">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2506">Description</span></span> | <span data-ttu-id="cc737-2507">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2507">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-2508">connectionString</span><span class="sxs-lookup"><span data-stu-id="cc737-2508">connectionString</span></span> |<span data-ttu-id="cc737-2509">A kapcsolati karakterlánc és egy opcionális titkosított hitelesítő adat nem hozzáférési hitelesítő adatok része.</span><span class="sxs-lookup"><span data-stu-id="cc737-2509">The non-access credential portion of the connection string and an optional encrypted credential.</span></span> <span data-ttu-id="cc737-2510">Példák az alábbi szakaszokban található.</span><span class="sxs-lookup"><span data-stu-id="cc737-2510">See examples in the following sections.</span></span> |<span data-ttu-id="cc737-2511">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2511">Yes</span></span> |
| <span data-ttu-id="cc737-2512">hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="cc737-2512">credential</span></span> |<span data-ttu-id="cc737-2513">A hozzáférési hitelesítő adatok része illesztőprogram-specifikus tulajdonság-érték formátumban megadott kapcsolódási karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="cc737-2513">The access credential portion of the connection string specified in driver-specific property-value format.</span></span> <span data-ttu-id="cc737-2514">Példa: "Uid =<user ID>; Pwd =<password>; RefreshToken =<secret refresh token>; ".</span><span class="sxs-lookup"><span data-stu-id="cc737-2514">Example: “Uid=<user ID>;Pwd=<password>;RefreshToken=<secret refresh token>;”.</span></span> |<span data-ttu-id="cc737-2515">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2515">No</span></span> |
| <span data-ttu-id="cc737-2516">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="cc737-2516">authenticationType</span></span> |<span data-ttu-id="cc737-2517">Az ODBC-adattár eléréséhez használt hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="cc737-2517">Type of authentication used to connect to the ODBC data store.</span></span> <span data-ttu-id="cc737-2518">Lehetséges értékek a következők: névtelen és alapvető.</span><span class="sxs-lookup"><span data-stu-id="cc737-2518">Possible values are: Anonymous and Basic.</span></span> |<span data-ttu-id="cc737-2519">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2519">Yes</span></span> |
| <span data-ttu-id="cc737-2520">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="cc737-2520">username</span></span> |<span data-ttu-id="cc737-2521">Ha egyszerű hitelesítést használ, adja meg a felhasználónevet.</span><span class="sxs-lookup"><span data-stu-id="cc737-2521">Specify user name if you are using Basic authentication.</span></span> |<span data-ttu-id="cc737-2522">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2522">No</span></span> |
| <span data-ttu-id="cc737-2523">jelszó</span><span class="sxs-lookup"><span data-stu-id="cc737-2523">password</span></span> |<span data-ttu-id="cc737-2524">Adja meg a felhasználónévhez megadott felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="cc737-2524">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="cc737-2525">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2525">No</span></span> |
| <span data-ttu-id="cc737-2526">gatewayName</span><span class="sxs-lookup"><span data-stu-id="cc737-2526">gatewayName</span></span> |<span data-ttu-id="cc737-2527">Az átjáró, amely a Data Factory szolgáltatásnak csatlakoznia az ODBC-adattárolóhoz neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-2527">Name of the gateway that the Data Factory service should use to connect to the ODBC data store.</span></span> |<span data-ttu-id="cc737-2528">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2528">Yes</span></span> |

#### <a name="example---using-basic-authentication"></a><span data-ttu-id="cc737-2529">Példa – egyszerű hitelesítés használata</span><span class="sxs-lookup"><span data-stu-id="cc737-2529">Example - Using Basic authentication</span></span>

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "OnPremisesOdbc",
        "typeProperties": {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;",
            "userName": "username",
            "password": "password",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```
#### <a name="example---using-basic-authentication-with-encrypted-credentials"></a><span data-ttu-id="cc737-2530">Példa – egyszerű hitelesítés használata a titkosított hitelesítő adatokat</span><span class="sxs-lookup"><span data-stu-id="cc737-2530">Example - Using Basic authentication with encrypted credentials</span></span>
<span data-ttu-id="cc737-2531">A hitelesítő adatokat titkosíthatja a [New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) (Azure PowerShell 1.0-ás verziója) parancsmag vagy [New-AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (az Azure PowerShell 0.9 vagy régebbi verzió).</span><span class="sxs-lookup"><span data-stu-id="cc737-2531">You can encrypt the credentials using the [New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) (1.0 version of Azure PowerShell) cmdlet or [New-AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0.9 or earlier version of the Azure PowerShell).</span></span>  

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "OnPremisesOdbc",
        "typeProperties": {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=myserver.database.windows.net; Database=TestDatabase;;EncryptedCredential=eyJDb25uZWN0...........................",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-using-anonymous-authentication"></a><span data-ttu-id="cc737-2532">Példa: A névtelen hitelesítést használó</span><span class="sxs-lookup"><span data-stu-id="cc737-2532">Example: Using Anonymous authentication</span></span>

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "OnPremisesOdbc",
        "typeProperties": {
            "authenticationType": "Anonymous",
            "connectionString": "Driver={SQL Server};Server={servername}.database.windows.net; Database=TestDatabase;",
            "credential": "UID={uid};PWD={pwd}",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

<span data-ttu-id="cc737-2533">További információkért lásd: [ODBC összekötő](data-factory-odbc-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2533">For more information, see [ODBC connector](data-factory-odbc-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="cc737-2534">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-2534">Dataset</span></span>
<span data-ttu-id="cc737-2535">Adja meg egy ODBC-adatkészlet, állítsa be a **típus** a DataSet **RelationalTable**, és adja meg a következő tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-2535">To define an ODBC dataset, set the **type** of the dataset to **RelationalTable**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="cc737-2536">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2536">Property</span></span> | <span data-ttu-id="cc737-2537">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2537">Description</span></span> | <span data-ttu-id="cc737-2538">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2538">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-2539">tableName</span><span class="sxs-lookup"><span data-stu-id="cc737-2539">tableName</span></span> |<span data-ttu-id="cc737-2540">Az ODBC-tárolóban a tábla neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-2540">Name of the table in the ODBC data store.</span></span> |<span data-ttu-id="cc737-2541">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2541">Yes</span></span> |


#### <a name="example"></a><span data-ttu-id="cc737-2542">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2542">Example</span></span>

```json
{
    "name": "ODBCDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "ODBCLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="cc737-2543">További információkért lásd: [ODBC összekötő](data-factory-odbc-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2543">For more information, see [ODBC connector](data-factory-odbc-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="cc737-2544">A másolási tevékenység relációs adatforrás</span><span class="sxs-lookup"><span data-stu-id="cc737-2544">Relational Source in Copy Activity</span></span>
<span data-ttu-id="cc737-2545">Adatok másolása egy ODBC adattárból, állítsa be a **adatforrástípust** a másolási tevékenység **RelationalSource**, és adja meg a következő tulajdonságokat a **forrás** szakasz :</span><span class="sxs-lookup"><span data-stu-id="cc737-2545">If you are copying data from an ODBC data store, set the **source type** of the copy activity to **RelationalSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="cc737-2546">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2546">Property</span></span> | <span data-ttu-id="cc737-2547">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2547">Description</span></span> | <span data-ttu-id="cc737-2548">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-2548">Allowed values</span></span> | <span data-ttu-id="cc737-2549">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2549">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-2550">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="cc737-2550">query</span></span> |<span data-ttu-id="cc737-2551">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="cc737-2551">Use the custom query to read data.</span></span> |<span data-ttu-id="cc737-2552">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="cc737-2552">SQL query string.</span></span> <span data-ttu-id="cc737-2553">Például: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="cc737-2553">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="cc737-2554">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2554">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-2555">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2555">Example</span></span>

```json
{
    "name": "CopyODBCToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "OdbcDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobOdbcDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "OdbcToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
``` 

<span data-ttu-id="cc737-2556">További információkért lásd: [ODBC összekötő](data-factory-odbc-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2556">For more information, see [ODBC connector](data-factory-odbc-connector.md#copy-activity-properties) article.</span></span>

## <a name="salesforce"></a><span data-ttu-id="cc737-2557">Salesforce</span><span class="sxs-lookup"><span data-stu-id="cc737-2557">Salesforce</span></span>


### <a name="linked-service"></a><span data-ttu-id="cc737-2558">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-2558">Linked service</span></span>
<span data-ttu-id="cc737-2559">A Salesforce meghatározásához társított szolgáltatás, állítsa be a **típus** a társított szolgáltatás **Salesforce**, és adja meg a következő tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-2559">To define a Salesforce linked service, set the **type** of the linked service to **Salesforce**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="cc737-2560">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2560">Property</span></span> | <span data-ttu-id="cc737-2561">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2561">Description</span></span> | <span data-ttu-id="cc737-2562">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2562">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-2563">environmentUrl</span><span class="sxs-lookup"><span data-stu-id="cc737-2563">environmentUrl</span></span> | <span data-ttu-id="cc737-2564">Adja meg az URL-címet a Salesforce-példány.</span><span class="sxs-lookup"><span data-stu-id="cc737-2564">Specify the URL of Salesforce instance.</span></span> <br><br> <span data-ttu-id="cc737-2565">-Alapértelmezett érték a "https://login.salesforce.com".</span><span class="sxs-lookup"><span data-stu-id="cc737-2565">- Default is "https://login.salesforce.com".</span></span> <br> <span data-ttu-id="cc737-2566">-Adatok másolása az védőfal, adja meg a "https://test.salesforce.com".</span><span class="sxs-lookup"><span data-stu-id="cc737-2566">- To copy data from sandbox, specify "https://test.salesforce.com".</span></span> <br> <span data-ttu-id="cc737-2567">-Adatok másolása az egyéni tartományt, adja meg, például "https://[domain].my.salesforce.com".</span><span class="sxs-lookup"><span data-stu-id="cc737-2567">- To copy data from custom domain, specify, for example, "https://[domain].my.salesforce.com".</span></span> |<span data-ttu-id="cc737-2568">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2568">No</span></span> |
| <span data-ttu-id="cc737-2569">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="cc737-2569">username</span></span> |<span data-ttu-id="cc737-2570">Adja meg a felhasználói fiók felhasználói nevét.</span><span class="sxs-lookup"><span data-stu-id="cc737-2570">Specify a user name for the user account.</span></span> |<span data-ttu-id="cc737-2571">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2571">Yes</span></span> |
| <span data-ttu-id="cc737-2572">jelszó</span><span class="sxs-lookup"><span data-stu-id="cc737-2572">password</span></span> |<span data-ttu-id="cc737-2573">Adja meg a felhasználói fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="cc737-2573">Specify a password for the user account.</span></span> |<span data-ttu-id="cc737-2574">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2574">Yes</span></span> |
| <span data-ttu-id="cc737-2575">securityToken</span><span class="sxs-lookup"><span data-stu-id="cc737-2575">securityToken</span></span> |<span data-ttu-id="cc737-2576">Adja meg a felhasználói fiók biztonsági jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="cc737-2576">Specify a security token for the user account.</span></span> <span data-ttu-id="cc737-2577">Lásd: [biztonsági jogkivonatának beszerzéséhez](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) útmutatást, ha alaphelyzetbe állítása/get egy biztonsági jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="cc737-2577">See [Get security token](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) for instructions on how to reset/get a security token.</span></span> <span data-ttu-id="cc737-2578">Általános biztonsági jogkivonatokat kapcsolatos további tudnivalókért lásd: [biztonsági és az API-t](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).</span><span class="sxs-lookup"><span data-stu-id="cc737-2578">To learn about security tokens in general, see [Security and the API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).</span></span> |<span data-ttu-id="cc737-2579">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2579">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-2580">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2580">Example</span></span>

```json
{
    "name": "SalesforceLinkedService",
    "properties": {
        "type": "Salesforce",
        "typeProperties": {
            "username": "<user name>",
            "password": "<password>",
            "securityToken": "<security token>"
        }
    }
}
```

<span data-ttu-id="cc737-2581">További információkért lásd: [Salesforce összekötő](data-factory-salesforce-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2581">For more information, see [Salesforce connector](data-factory-salesforce-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="cc737-2582">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-2582">Dataset</span></span>
<span data-ttu-id="cc737-2583">Adja meg a Salesforce-adatkészlet, állítsa be a **típus** a DataSet **RelationalTable**, és adja meg az alábbi tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-2583">To define a Salesforce dataset, set the **type** of the dataset to **RelationalTable**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="cc737-2584">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2584">Property</span></span> | <span data-ttu-id="cc737-2585">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2585">Description</span></span> | <span data-ttu-id="cc737-2586">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2586">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-2587">tableName</span><span class="sxs-lookup"><span data-stu-id="cc737-2587">tableName</span></span> |<span data-ttu-id="cc737-2588">A Salesforce-ban a tábla neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-2588">Name of the table in Salesforce.</span></span> |<span data-ttu-id="cc737-2589">Nem (Ha egy **lekérdezés** a **RelationalSource** van megadva)</span><span class="sxs-lookup"><span data-stu-id="cc737-2589">No (if a **query** of **RelationalSource** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-2590">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2590">Example</span></span>

```json
{
    "name": "SalesforceInput",
    "properties": {
        "linkedServiceName": "SalesforceLinkedService",
        "type": "RelationalTable",
        "typeProperties": {
            "tableName": "AllDataType__c"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="cc737-2591">További információkért lásd: [Salesforce összekötő](data-factory-salesforce-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2591">For more information, see [Salesforce connector](data-factory-salesforce-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="cc737-2592">A másolási tevékenység relációs adatforrás</span><span class="sxs-lookup"><span data-stu-id="cc737-2592">Relational Source in Copy Activity</span></span>
<span data-ttu-id="cc737-2593">Adatok másolása a Salesforce, állítsa be a **adatforrástípust** a másolási tevékenység **RelationalSource**, és adja meg a következő tulajdonságokat a **forrás** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-2593">If you are copying data from Salesforce, set the **source type** of the copy activity to **RelationalSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="cc737-2594">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2594">Property</span></span> | <span data-ttu-id="cc737-2595">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2595">Description</span></span> | <span data-ttu-id="cc737-2596">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="cc737-2596">Allowed values</span></span> | <span data-ttu-id="cc737-2597">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2597">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc737-2598">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="cc737-2598">query</span></span> |<span data-ttu-id="cc737-2599">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="cc737-2599">Use the custom query to read data.</span></span> |<span data-ttu-id="cc737-2600">Egy SQL-92 lekérdezés vagy [Salesforce objektum Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="cc737-2600">A SQL-92 query or [Salesforce Object Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) query.</span></span> <span data-ttu-id="cc737-2601">Például: `select * from MyTable__c`.</span><span class="sxs-lookup"><span data-stu-id="cc737-2601">For example:  `select * from MyTable__c`.</span></span> |<span data-ttu-id="cc737-2602">Nem (Ha a **tableName** , a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="cc737-2602">No (if the **tableName** of the **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-2603">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2603">Example</span></span>  



```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "SalesforceToAzureBlob",
            "description": "Copy from Salesforce to an Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "SalesforceInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "SELECT Id, Col_AutoNumber__c, Col_Checkbox__c, Col_Currency__c, Col_Date__c, Col_DateTime__c, Col_Email__c, Col_Number__c, Col_Percent__c, Col_Phone__c, Col_Picklist__c, Col_Picklist_MultiSelect__c, Col_Text__c, Col_Text_Area__c, Col_Text_AreaLong__c, Col_Text_AreaRich__c, Col_URL__c, Col_Text_Encrypt__c, Col_Lookup__c FROM AllDataType__c"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

> [!IMPORTANT]
> <span data-ttu-id="cc737-2604">Az API neve "__c" részét bármilyen egyéni objektum szükséges.</span><span class="sxs-lookup"><span data-stu-id="cc737-2604">The "__c" part of the API Name is needed for any custom object.</span></span>

<span data-ttu-id="cc737-2605">További információkért lásd: [Salesforce összekötő](data-factory-salesforce-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2605">For more information, see [Salesforce connector](data-factory-salesforce-connector.md#copy-activity-properties) article.</span></span> 

## <a name="web-data"></a><span data-ttu-id="cc737-2606">Webes adatok</span><span class="sxs-lookup"><span data-stu-id="cc737-2606">Web Data</span></span> 

### <a name="linked-service"></a><span data-ttu-id="cc737-2607">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-2607">Linked service</span></span>
<span data-ttu-id="cc737-2608">Egy webes meghatározásához társított szolgáltatás, állítsa be a **típus** a társított szolgáltatás **webes**, és adja meg a következő tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-2608">To define a Web linked service, set the **type** of the linked service to **Web**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="cc737-2609">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2609">Property</span></span> | <span data-ttu-id="cc737-2610">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2610">Description</span></span> | <span data-ttu-id="cc737-2611">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2611">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-2612">URL-cím</span><span class="sxs-lookup"><span data-stu-id="cc737-2612">Url</span></span> |<span data-ttu-id="cc737-2613">A webes forrás URL-címe</span><span class="sxs-lookup"><span data-stu-id="cc737-2613">URL to the Web source</span></span> |<span data-ttu-id="cc737-2614">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2614">Yes</span></span> |
| <span data-ttu-id="cc737-2615">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="cc737-2615">authenticationType</span></span> |<span data-ttu-id="cc737-2616">Névtelen.</span><span class="sxs-lookup"><span data-stu-id="cc737-2616">Anonymous.</span></span> |<span data-ttu-id="cc737-2617">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2617">Yes</span></span> |
 

#### <a name="example"></a><span data-ttu-id="cc737-2618">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2618">Example</span></span>


```json
{
    "name": "web",
    "properties": {
        "type": "Web",
        "typeProperties": {
            "authenticationType": "Anonymous",
            "url": "https://en.wikipedia.org/wiki/"
        }
    }
}
```

<span data-ttu-id="cc737-2619">További információkért lásd: [webes tábla összekötő](data-factory-web-table-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2619">For more information, see [Web Table connector](data-factory-web-table-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="cc737-2620">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cc737-2620">Dataset</span></span>
<span data-ttu-id="cc737-2621">Adja meg a webes adatkészletet, állítsa be a **típus** a DataSet **Webtábla**, és adja meg az alábbi tulajdonságokat a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-2621">To define a Web dataset, set the **type** of the dataset to **WebTable**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="cc737-2622">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2622">Property</span></span> | <span data-ttu-id="cc737-2623">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2623">Description</span></span> | <span data-ttu-id="cc737-2624">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2624">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="cc737-2625">type</span><span class="sxs-lookup"><span data-stu-id="cc737-2625">type</span></span> |<span data-ttu-id="cc737-2626">A dataset típusa.</span><span class="sxs-lookup"><span data-stu-id="cc737-2626">type of the dataset.</span></span> <span data-ttu-id="cc737-2627">meg kell **Webtábla**</span><span class="sxs-lookup"><span data-stu-id="cc737-2627">must be set to **WebTable**</span></span> |<span data-ttu-id="cc737-2628">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2628">Yes</span></span> |
| <span data-ttu-id="cc737-2629">Elérési út</span><span class="sxs-lookup"><span data-stu-id="cc737-2629">path</span></span> |<span data-ttu-id="cc737-2630">Az erőforrás, amely tartalmazza a tábla relatív URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="cc737-2630">A relative URL to the resource that contains the table.</span></span> |<span data-ttu-id="cc737-2631">Nem.</span><span class="sxs-lookup"><span data-stu-id="cc737-2631">No.</span></span> <span data-ttu-id="cc737-2632">Ha nincs megadva, csak a megadott URL-cím a társított szolgáltatás definíciójának használja.</span><span class="sxs-lookup"><span data-stu-id="cc737-2632">When path is not specified, only the URL specified in the linked service definition is used.</span></span> |
| <span data-ttu-id="cc737-2633">Index</span><span class="sxs-lookup"><span data-stu-id="cc737-2633">index</span></span> |<span data-ttu-id="cc737-2634">Annak az erőforrás a táblának az indexe.</span><span class="sxs-lookup"><span data-stu-id="cc737-2634">The index of the table in the resource.</span></span> <span data-ttu-id="cc737-2635">Lásd: [Get index egy tábla egy HTML-lapon](#get-index-of-a-table-in-an-html-page) szakasz lépéseit egy tábla indexének első HTML-lapon.</span><span class="sxs-lookup"><span data-stu-id="cc737-2635">See [Get index of a table in an HTML page](#get-index-of-a-table-in-an-html-page) section for steps to getting index of a table in an HTML page.</span></span> |<span data-ttu-id="cc737-2636">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2636">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="cc737-2637">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2637">Example</span></span>

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="cc737-2638">További információkért lásd: [webes tábla összekötő](data-factory-web-table-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2638">For more information, see [Web Table connector](data-factory-web-table-connector.md#dataset-properties) article.</span></span> 

### <a name="web-source-in-copy-activity"></a><span data-ttu-id="cc737-2639">A másolási tevékenység webes forrás</span><span class="sxs-lookup"><span data-stu-id="cc737-2639">Web Source in Copy Activity</span></span>
<span data-ttu-id="cc737-2640">Adatok másolása egy webes táblából, állítsa be a **adatforrástípust** a másolási tevékenység **WebSource**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2640">If you are copying data from a web table, set the **source type** of the copy activity to **WebSource**.</span></span> <span data-ttu-id="cc737-2641">Ha a forrás, a másolási tevékenység jelenleg típusú **WebSource**, további tulajdonságok nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="cc737-2641">Currently, when the source in copy activity is of type **WebSource**, no additional properties are supported.</span></span>

#### <a name="example"></a><span data-ttu-id="cc737-2642">Példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2642">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "WebTableToAzureBlob",
            "description": "Copy from a Web table to an Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "WebTableInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "WebSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="cc737-2643">További információkért lásd: [webes tábla összekötő](data-factory-web-table-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2643">For more information, see [Web Table connector](data-factory-web-table-connector.md#copy-activity-properties) article.</span></span> 

## <a name="compute-environments"></a><span data-ttu-id="cc737-2644">SZÁMÍTÁSI KÖRNYEZETEK</span><span class="sxs-lookup"><span data-stu-id="cc737-2644">COMPUTE ENVIRONMENTS</span></span>
<span data-ttu-id="cc737-2645">Az alábbi táblázat a Data Factory és az átalakítási tevékenységek ezeken futó által támogatott számítási környezeteket.</span><span class="sxs-lookup"><span data-stu-id="cc737-2645">The following table lists the compute environments supported by Data Factory and the transformation activities that can run on them.</span></span> <span data-ttu-id="cc737-2646">A számítási érdekli a JSON-sémák csatolni egy adat-előállító kapcsolódó szolgáltatás megjelenítéséhez a hivatkozásra kattintva.</span><span class="sxs-lookup"><span data-stu-id="cc737-2646">Click the link for the compute you are interested in to see the JSON schemas for linked service to link it to a data factory.</span></span> 

| <span data-ttu-id="cc737-2647">Számítási környezet</span><span class="sxs-lookup"><span data-stu-id="cc737-2647">Compute environment</span></span> | <span data-ttu-id="cc737-2648">Tevékenységek</span><span class="sxs-lookup"><span data-stu-id="cc737-2648">Activities</span></span> |
| --- | --- |
| <span data-ttu-id="cc737-2649">[Igény szerinti HDInsight-fürt](#on-demand-azure-hdinsight-cluster) vagy [saját HDInsight-fürt](#existing-azure-hdinsight-cluster)</span><span class="sxs-lookup"><span data-stu-id="cc737-2649">[On-demand HDInsight cluster](#on-demand-azure-hdinsight-cluster) or [your own HDInsight cluster](#existing-azure-hdinsight-cluster)</span></span> |<span data-ttu-id="cc737-2650">[.NET egyéni tevékenység](#net-custom-activity), [tevékenység Hive](#hdinsight-hive-activity), [tevékenység sertésfelmérés] (hdinsight-pig-tevékenység # [MapReduce művelethez](#hdinsight-mapreduce-activity), [Hadoop-Stream tevékenység](#hdinsight-streaming-activityd), [ A Spark-tevékenység](#hdinsight-spark-activity)</span><span class="sxs-lookup"><span data-stu-id="cc737-2650">[.NET custom activity](#net-custom-activity), [Hive activity](#hdinsight-hive-activity), [Pig activity](#hdinsight-pig-activity, [MapReduce activity](#hdinsight-mapreduce-activity), [Hadoop streaming activity](#hdinsight-streaming-activityd), [Spark activity](#hdinsight-spark-activity)</span></span> |
| [<span data-ttu-id="cc737-2651">Az Azure Batch</span><span class="sxs-lookup"><span data-stu-id="cc737-2651">Azure Batch</span></span>](#azure-batch) |[<span data-ttu-id="cc737-2652">.NET egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="cc737-2652">.NET custom activity</span></span>](#net-custom-activity) |
| [<span data-ttu-id="cc737-2653">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="cc737-2653">Azure Machine Learning</span></span>](#azure-machine-learning) | <span data-ttu-id="cc737-2654">[Számítógép-kötegelt végrehajtási tevékenység tanulási](#machine-learning-batch-execution-activity), [gép tanulási Update-Erőforrástevékenység](#machine-learning-update-resource-activity)</span><span class="sxs-lookup"><span data-stu-id="cc737-2654">[Machine Learning Batch Execution Activity](#machine-learning-batch-execution-activity), [Machine Learning Update Resource Activity](#machine-learning-update-resource-activity)</span></span> |
| [<span data-ttu-id="cc737-2655">Az Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="cc737-2655">Azure Data Lake Analytics</span></span>](#azure-data-lake-analytics) |[<span data-ttu-id="cc737-2656">Data Lake Analytics U-SQL</span><span class="sxs-lookup"><span data-stu-id="cc737-2656">Data Lake Analytics U-SQL</span></span>](#data-lake-analytics-u-sql-activity) |
| <span data-ttu-id="cc737-2657">[Az Azure SQL adatbázis](#azure-sql-database-1), [Azure SQL Data Warehouse](#azure-sql-data-warehouse-1), [SQL Server](#sql-server-1)</span><span class="sxs-lookup"><span data-stu-id="cc737-2657">[Azure SQL Database](#azure-sql-database-1), [Azure SQL Data Warehouse](#azure-sql-data-warehouse-1), [SQL Server](#sql-server-1)</span></span> |[<span data-ttu-id="cc737-2658">Tárolt eljárás</span><span class="sxs-lookup"><span data-stu-id="cc737-2658">Stored Procedure</span></span>](#stored-procedure-activity) |

## <a name="on-demand-azure-hdinsight-cluster"></a><span data-ttu-id="cc737-2659">Igény szerinti Azure HDInsight-fürt</span><span class="sxs-lookup"><span data-stu-id="cc737-2659">On-demand Azure HDInsight cluster</span></span>
<span data-ttu-id="cc737-2660">Az Azure Data Factory szolgáltatásnak automatikusan hozhat létre egy Windows/Linux-alapú igény szerinti HDInsight-fürt adatfeldolgozásra történő.</span><span class="sxs-lookup"><span data-stu-id="cc737-2660">The Azure Data Factory service can automatically create a Windows/Linux-based on-demand HDInsight cluster to process data.</span></span> <span data-ttu-id="cc737-2661">A fürt létrehozása a tárfiók (linkedServiceName tulajdonságot a JSON-ban) a fürthöz tartozó ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="cc737-2661">The cluster is created in the same region as the storage account (linkedServiceName property in the JSON) associated with the cluster.</span></span> <span data-ttu-id="cc737-2662">Futtathatja a következő átalakító tevékenységek szolgáltatásnak: [.NET egyéni tevékenység](#net-custom-activity), [tevékenység Hive](#hdinsight-hive-activity), [tevékenység sertésfelmérés] (hdinsight-pig-tevékenység # [MapReduce művelethez ](#hdinsight-mapreduce-activity), [Hadoop-Stream tevékenység](#hdinsight-streaming-activityd), [tevékenység Spark](#hdinsight-spark-activity).</span><span class="sxs-lookup"><span data-stu-id="cc737-2662">You can run the following transformation activities on this linked service: [.NET custom activity](#net-custom-activity), [Hive activity](#hdinsight-hive-activity), [Pig activity](#hdinsight-pig-activity, [MapReduce activity](#hdinsight-mapreduce-activity), [Hadoop streaming activity](#hdinsight-streaming-activityd), [Spark activity](#hdinsight-spark-activity).</span></span> 

### <a name="linked-service"></a><span data-ttu-id="cc737-2663">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-2663">Linked service</span></span> 
<span data-ttu-id="cc737-2664">A következő táblázat ismerteti az igény szerinti HDInsight társított szolgáltatásnak Azure JSON-definícióból használt tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="cc737-2664">The following table provides descriptions for the properties used in the Azure JSON definition of an on-demand HDInsight linked service.</span></span>

| <span data-ttu-id="cc737-2665">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2665">Property</span></span> | <span data-ttu-id="cc737-2666">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2666">Description</span></span> | <span data-ttu-id="cc737-2667">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2667">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-2668">type</span><span class="sxs-lookup"><span data-stu-id="cc737-2668">type</span></span> |<span data-ttu-id="cc737-2669">A type tulajdonságot kell megadni **HDInsightOnDemand**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2669">The type property should be set to **HDInsightOnDemand**.</span></span> |<span data-ttu-id="cc737-2670">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2670">Yes</span></span> |
| <span data-ttu-id="cc737-2671">Nagyobbnak</span><span class="sxs-lookup"><span data-stu-id="cc737-2671">clusterSize</span></span> |<span data-ttu-id="cc737-2672">A fürt munkavégző/adatok csomópontok száma.</span><span class="sxs-lookup"><span data-stu-id="cc737-2672">Number of worker/data nodes in the cluster.</span></span> <span data-ttu-id="cc737-2673">A HDInsight-fürt együtt ez a tulajdonság a megadott munkavégző csomópontok száma 2 átjárócsomópontokkal hozza létre.</span><span class="sxs-lookup"><span data-stu-id="cc737-2673">The HDInsight cluster is created with 2 head nodes along with the number of worker nodes you specify for this property.</span></span> <span data-ttu-id="cc737-2674">A csomópontok egy 4 munkavégző csomópontot tartalmazó fürtben veszi 24 mag, 4 mag, rendelkező standard, D3 méretű vannak (4\*a munkavégző csomópontokról, valamint 2 processzormag, 4 = 16\*az átjárócsomópontokkal processzormag, 4 = 8).</span><span class="sxs-lookup"><span data-stu-id="cc737-2674">The nodes are of size Standard_D3 that has 4 cores, so a 4 worker node cluster takes 24 cores (4\*4 = 16 cores for worker nodes, plus 2\*4 = 8 cores for head nodes).</span></span> <span data-ttu-id="cc737-2675">Lásd: [hdinsight létrehozása Linux-alapú Hadoop-fürtök](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) a standard, D3 réteg vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="cc737-2675">See [Create Linux-based Hadoop clusters in HDInsight](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) for details about the Standard_D3 tier.</span></span> |<span data-ttu-id="cc737-2676">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2676">Yes</span></span> |
| <span data-ttu-id="cc737-2677">a TimeToLive tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2677">timetolive</span></span> |<span data-ttu-id="cc737-2678">A megengedett üresjárati idő az igény szerinti HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="cc737-2678">The allowed idle time for the on-demand HDInsight cluster.</span></span> <span data-ttu-id="cc737-2679">Meghatározza, mennyi ideig az igény szerinti HDInsight-fürt aktív marad egy tevékenység fut, ha nincsenek a fürt más aktív feladatok befejezése után.</span><span class="sxs-lookup"><span data-stu-id="cc737-2679">Specifies how long the on-demand HDInsight cluster stays alive after completion of an activity run if there are no other active jobs in the cluster.</span></span><br/><br/><span data-ttu-id="cc737-2680">Például ha egy tevékenység futott 6 percig tart, és az élettartam értéke 5 perc, a fürt marad, a figyelő életben 5 perc, a 6 percnél feldolgozásának a tevékenység futtatása után.</span><span class="sxs-lookup"><span data-stu-id="cc737-2680">For example, if an activity run takes 6 minutes and timetolive is set to 5 minutes, the cluster stays alive for 5 minutes after the 6 minutes of processing the activity run.</span></span> <span data-ttu-id="cc737-2681">Ha egy másik tevékenységfuttatási 6 percnél időkeretet, dolgoz fel ugyanabban a fürtben.</span><span class="sxs-lookup"><span data-stu-id="cc737-2681">If another activity run is executed with the 6 minutes window, it is processed by the same cluster.</span></span><br/><br/><span data-ttu-id="cc737-2682">Igény szerinti HDInsight fürtök létrehozásával egy (igénybe vehet) drága művelet, ezt a beállítást, mint egy adat-előállító teljesítményének javításával újból felhasználja az igény szerinti HDInsight-fürtök által szükséges Igen használja.</span><span class="sxs-lookup"><span data-stu-id="cc737-2682">Creating an on-demand HDInsight cluster is an expensive operation (could take a while), so use this setting as needed to improve performance of a data factory by reusing an on-demand HDInsight cluster.</span></span><br/><br/><span data-ttu-id="cc737-2683">A TimeToLive tulajdonság értékét 0-ra állítja be, ha törölni a fürtöt, amint a tevékenység futtatása feldolgozott.</span><span class="sxs-lookup"><span data-stu-id="cc737-2683">If you set timetolive value to 0, the cluster is deleted as soon as the activity run in processed.</span></span> <span data-ttu-id="cc737-2684">Másrészről Ha a magas érték, a fürt felfüggesztheti üresjárati feleslegesen magas költségeket eredményez.</span><span class="sxs-lookup"><span data-stu-id="cc737-2684">On the other hand, if you set a high value, the cluster may stay idle unnecessarily resulting in high costs.</span></span> <span data-ttu-id="cc737-2685">Ezért fontos, hogy beállította-e a megfelelő értéket a igényei szerint.</span><span class="sxs-lookup"><span data-stu-id="cc737-2685">Therefore, it is important that you set the appropriate value based on your needs.</span></span><br/><br/><span data-ttu-id="cc737-2686">Több folyamatok is használ az igény szerinti HDInsight-fürt ugyanazt a példányát, ha a timetolive tulajdonság értékének megfelelően van beállítva.</span><span class="sxs-lookup"><span data-stu-id="cc737-2686">Multiple pipelines can share the same instance of the on-demand HDInsight cluster if the timetolive property value is appropriately set</span></span> |<span data-ttu-id="cc737-2687">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2687">Yes</span></span> |
| <span data-ttu-id="cc737-2688">Verzió</span><span class="sxs-lookup"><span data-stu-id="cc737-2688">version</span></span> |<span data-ttu-id="cc737-2689">A HDInsight-fürt verziószáma.</span><span class="sxs-lookup"><span data-stu-id="cc737-2689">Version of the HDInsight cluster.</span></span> <span data-ttu-id="cc737-2690">További információkért lásd: [HDInsight-verziókról támogatott az Azure Data Factory](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory).</span><span class="sxs-lookup"><span data-stu-id="cc737-2690">For details, see [supported HDInsight versions in Azure Data Factory](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory).</span></span> |<span data-ttu-id="cc737-2691">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2691">No</span></span> |
| <span data-ttu-id="cc737-2692">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="cc737-2692">linkedServiceName</span></span> |<span data-ttu-id="cc737-2693">Az Azure tárolás társított szolgáltatásának történő tárolására és feldolgozására adatok az igény szerinti fürt által használható.</span><span class="sxs-lookup"><span data-stu-id="cc737-2693">Azure Storage linked service to be used by the on-demand cluster for storing and processing data.</span></span> <p><span data-ttu-id="cc737-2694">Jelenleg nem hozható létre, amely egy Azure Data Lake Store használ a tárolási igény szerinti HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="cc737-2694">Currently, you cannot create an on-demand HDInsight cluster that uses an Azure Data Lake Store as the storage.</span></span> <span data-ttu-id="cc737-2695">Ha szeretné tárolni az eredményadatok a HDInsight-feldolgozás alatt álló egy Azure Data Lake Store-ból, a másolási tevékenység segítségével az adatok másolása az Azure Blob Storage-ból az Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="cc737-2695">If you want to store the result data from HDInsight processing in an Azure Data Lake Store, use a Copy Activity to copy the data from the Azure Blob Storage to the Azure Data Lake Store.</span></span></p>  | <span data-ttu-id="cc737-2696">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2696">Yes</span></span> |
| <span data-ttu-id="cc737-2697">additionalLinkedServiceNames</span><span class="sxs-lookup"><span data-stu-id="cc737-2697">additionalLinkedServiceNames</span></span> |<span data-ttu-id="cc737-2698">Adja meg a további tárfiókok a HDInsight a társított szolgáltatás, hogy a Data Factory szolgáltatásnak is regisztrálja őket az Ön nevében.</span><span class="sxs-lookup"><span data-stu-id="cc737-2698">Specifies additional storage accounts for the HDInsight linked service so that the Data Factory service can register them on your behalf.</span></span> |<span data-ttu-id="cc737-2699">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2699">No</span></span> |
| <span data-ttu-id="cc737-2700">osType</span><span class="sxs-lookup"><span data-stu-id="cc737-2700">osType</span></span> |<span data-ttu-id="cc737-2701">Az operációs rendszer típusát.</span><span class="sxs-lookup"><span data-stu-id="cc737-2701">Type of operating system.</span></span> <span data-ttu-id="cc737-2702">Két érték engedélyezett: (alapértelmezett) Windows és Linux</span><span class="sxs-lookup"><span data-stu-id="cc737-2702">Allowed values are: Windows (default) and Linux</span></span> |<span data-ttu-id="cc737-2703">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2703">No</span></span> |
| <span data-ttu-id="cc737-2704">hcatalogLinkedServiceName</span><span class="sxs-lookup"><span data-stu-id="cc737-2704">hcatalogLinkedServiceName</span></span> |<span data-ttu-id="cc737-2705">A név az Azure SQL társított szolgáltatásnak, mutasson az HCatalog-adatbázisra.</span><span class="sxs-lookup"><span data-stu-id="cc737-2705">The name of Azure SQL linked service that point to the HCatalog database.</span></span> <span data-ttu-id="cc737-2706">Az igény szerinti HDInsight-fürt létrehozása az Azure SQL database segítségével a metaadattárhoz.</span><span class="sxs-lookup"><span data-stu-id="cc737-2706">The on-demand HDInsight cluster is created by using the Azure SQL database as the metastore.</span></span> |<span data-ttu-id="cc737-2707">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2707">No</span></span> |

### <a name="json-example"></a><span data-ttu-id="cc737-2708">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2708">JSON example</span></span>
<span data-ttu-id="cc737-2709">A következő JSON igény kapcsolódó HDInsight Linux-alapú szolgáltatás határozza meg.</span><span class="sxs-lookup"><span data-stu-id="cc737-2709">The following JSON defines a Linux-based on-demand HDInsight linked service.</span></span> <span data-ttu-id="cc737-2710">A Data Factory szolgáltatásnak automatikusan létrehoz egy **Linux-alapú** HDInsight-fürt adatok szelet feldolgozása közben.</span><span class="sxs-lookup"><span data-stu-id="cc737-2710">The Data Factory service automatically creates a **Linux-based** HDInsight cluster when processing a data slice.</span></span> 

```json
{
    "name": "HDInsightOnDemandLinkedService",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "StorageLinkedService"
        }
    }
}
```

<span data-ttu-id="cc737-2711">További információkért lásd: [összekapcsolt szolgáltatások számítási](data-factory-compute-linked-services.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2711">For more information, see [Compute linked services](data-factory-compute-linked-services.md) article.</span></span> 

## <a name="existing-azure-hdinsight-cluster"></a><span data-ttu-id="cc737-2712">Meglévő Azure HDInsight-fürt</span><span class="sxs-lookup"><span data-stu-id="cc737-2712">Existing Azure HDInsight cluster</span></span>
<span data-ttu-id="cc737-2713">Létrehozhat saját HDInsight-fürt regisztrálni a Data Factory kapcsolt Azure HDInsight szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="cc737-2713">You can create an Azure HDInsight linked service to register your own HDInsight cluster with Data Factory.</span></span> <span data-ttu-id="cc737-2714">Futtathatja a következő adatok átalakítása tevékenységek szolgáltatásnak: [.NET egyéni tevékenység](#net-custom-activity), [tevékenység Hive](#hdinsight-hive-activity), [tevékenység sertésfelmérés] (hdinsight-pig-tevékenység # [MapReduce tevékenység](#hdinsight-mapreduce-activity), [Hadoop-Stream tevékenység](#hdinsight-streaming-activityd), [tevékenység Spark](#hdinsight-spark-activity).</span><span class="sxs-lookup"><span data-stu-id="cc737-2714">You can run the following data transformation activities on this linked service: [.NET custom activity](#net-custom-activity), [Hive activity](#hdinsight-hive-activity), [Pig activity](#hdinsight-pig-activity, [MapReduce activity](#hdinsight-mapreduce-activity), [Hadoop streaming activity](#hdinsight-streaming-activityd), [Spark activity](#hdinsight-spark-activity).</span></span> 

### <a name="linked-service"></a><span data-ttu-id="cc737-2715">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-2715">Linked service</span></span>
<span data-ttu-id="cc737-2716">A következő táblázat ismerteti az Azure HDInsight társított szolgáltatásnak Azure JSON-definícióból használt tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="cc737-2716">The following table provides descriptions for the properties used in the Azure JSON definition of an Azure HDInsight linked service.</span></span>

| <span data-ttu-id="cc737-2717">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2717">Property</span></span> | <span data-ttu-id="cc737-2718">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2718">Description</span></span> | <span data-ttu-id="cc737-2719">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2719">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-2720">type</span><span class="sxs-lookup"><span data-stu-id="cc737-2720">type</span></span> |<span data-ttu-id="cc737-2721">A type tulajdonságot kell megadni **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2721">The type property should be set to **HDInsight**.</span></span> |<span data-ttu-id="cc737-2722">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2722">Yes</span></span> |
| <span data-ttu-id="cc737-2723">clusterUri</span><span class="sxs-lookup"><span data-stu-id="cc737-2723">clusterUri</span></span> |<span data-ttu-id="cc737-2724">Az URI-je a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="cc737-2724">The URI of the HDInsight cluster.</span></span> |<span data-ttu-id="cc737-2725">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2725">Yes</span></span> |
| <span data-ttu-id="cc737-2726">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="cc737-2726">username</span></span> |<span data-ttu-id="cc737-2727">Adja meg a felhasználó egy meglévő HDInsight-fürtre való kapcsolódáshoz használt nevét.</span><span class="sxs-lookup"><span data-stu-id="cc737-2727">Specify the name of the user to be used to connect to an existing HDInsight cluster.</span></span> |<span data-ttu-id="cc737-2728">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2728">Yes</span></span> |
| <span data-ttu-id="cc737-2729">jelszó</span><span class="sxs-lookup"><span data-stu-id="cc737-2729">password</span></span> |<span data-ttu-id="cc737-2730">Adja meg a felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="cc737-2730">Specify password for the user account.</span></span> |<span data-ttu-id="cc737-2731">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2731">Yes</span></span> |
| <span data-ttu-id="cc737-2732">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="cc737-2732">linkedServiceName</span></span> | <span data-ttu-id="cc737-2733">Az Azure blob storage a HDInsight-fürt által használt az Azure Storage társított szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-2733">Name of the Azure Storage linked service that refers to the Azure blob storage used by the HDInsight cluster.</span></span> <p><span data-ttu-id="cc737-2734">Jelenleg nem adhat meg egy Azure Data Lake Store társított szolgáltatás ehhez a tulajdonsághoz.</span><span class="sxs-lookup"><span data-stu-id="cc737-2734">Currently, you cannot specify an Azure Data Lake Store linked service for this property.</span></span> <span data-ttu-id="cc737-2735">Ha a HDInsight-fürt hozzáférése van a Data Lake Store adatokat az Azure Data Lake Store előfordulhat, hogy elérhető Hive/Pig-parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="cc737-2735">You may access data in the Azure Data Lake Store from Hive/Pig scripts if the HDInsight cluster has access to the Data Lake Store.</span></span> </p>  |<span data-ttu-id="cc737-2736">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2736">Yes</span></span> |

<span data-ttu-id="cc737-2737">A HDInsight-fürtök támogatott verzióit lásd: [HDInsight-verziókról támogatott](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory).</span><span class="sxs-lookup"><span data-stu-id="cc737-2737">For versions of HDInsight clusters supported, see [supported HDInsight versions](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory).</span></span> 

#### <a name="json-example"></a><span data-ttu-id="cc737-2738">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2738">JSON example</span></span>

```json
{
    "name": "HDInsightLinkedService",
    "properties": {
        "type": "HDInsight",
        "typeProperties": {
            "clusterUri": " https://<hdinsightclustername>.azurehdinsight.net/",
            "userName": "admin",
            "password": "<password>",
            "linkedServiceName": "MyHDInsightStoragelinkedService"
        }
    }
}
```

## <a name="azure-batch"></a><span data-ttu-id="cc737-2739">Azure Batch</span><span class="sxs-lookup"><span data-stu-id="cc737-2739">Azure Batch</span></span>
<span data-ttu-id="cc737-2740">A data Factory létrehozhat egy csatolt Azure Batch szolgáltatás regisztrálása virtuális gépek (VM) kötegelt készletét.</span><span class="sxs-lookup"><span data-stu-id="cc737-2740">You can create an Azure Batch linked service to register a Batch pool of virtual machines (VMs) with a data factory.</span></span> <span data-ttu-id="cc737-2741">.NET-egyéni tevékenységek Azure Batch vagy Azure HDInsight segítségével is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="cc737-2741">You can run .NET custom activities using either Azure Batch or Azure HDInsight.</span></span> <span data-ttu-id="cc737-2742">Futtathatja a [.NET egyéni tevékenység](#net-custom-activity) a társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="cc737-2742">You can run a [.NET custom activity](#net-custom-activity) on this linked service.</span></span> 

### <a name="linked-service"></a><span data-ttu-id="cc737-2743">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-2743">Linked service</span></span>
<span data-ttu-id="cc737-2744">A következő táblázat ismerteti az Azure Batch társított szolgáltatásnak Azure JSON definíciójában használt tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="cc737-2744">The following table provides descriptions for the properties used in the Azure JSON definition of an Azure Batch linked service.</span></span>

| <span data-ttu-id="cc737-2745">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2745">Property</span></span> | <span data-ttu-id="cc737-2746">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2746">Description</span></span> | <span data-ttu-id="cc737-2747">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2747">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-2748">type</span><span class="sxs-lookup"><span data-stu-id="cc737-2748">type</span></span> |<span data-ttu-id="cc737-2749">A type tulajdonságot kell megadni **AzureBatch**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2749">The type property should be set to **AzureBatch**.</span></span> |<span data-ttu-id="cc737-2750">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2750">Yes</span></span> |
| <span data-ttu-id="cc737-2751">Fióknév</span><span class="sxs-lookup"><span data-stu-id="cc737-2751">accountName</span></span> |<span data-ttu-id="cc737-2752">Az Azure Batch-fiók neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-2752">Name of the Azure Batch account.</span></span> |<span data-ttu-id="cc737-2753">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2753">Yes</span></span> |
| <span data-ttu-id="cc737-2754">accessKey</span><span class="sxs-lookup"><span data-stu-id="cc737-2754">accessKey</span></span> |<span data-ttu-id="cc737-2755">Az Azure Batch-fiók elérési kulcsának.</span><span class="sxs-lookup"><span data-stu-id="cc737-2755">Access key for the Azure Batch account.</span></span> |<span data-ttu-id="cc737-2756">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2756">Yes</span></span> |
| <span data-ttu-id="cc737-2757">poolName</span><span class="sxs-lookup"><span data-stu-id="cc737-2757">poolName</span></span> |<span data-ttu-id="cc737-2758">A virtuálisgép-készlet neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-2758">Name of the pool of virtual machines.</span></span> |<span data-ttu-id="cc737-2759">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2759">Yes</span></span> |
| <span data-ttu-id="cc737-2760">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="cc737-2760">linkedServiceName</span></span> |<span data-ttu-id="cc737-2761">Neve az Azure Storage társított szolgáltatásnak a kapcsolódó Azure-köteg szolgáltatással kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="cc737-2761">Name of the Azure Storage linked service associated with this Azure Batch linked service.</span></span> <span data-ttu-id="cc737-2762">A társított szolgáltatás átmeneti fájlok kell futnia a tevékenység és a tevékenység végrehajtási naplók tárolására szolgál.</span><span class="sxs-lookup"><span data-stu-id="cc737-2762">This linked service is used for staging files required to run the activity and storing the activity execution logs.</span></span> |<span data-ttu-id="cc737-2763">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2763">Yes</span></span> |


#### <a name="json-example"></a><span data-ttu-id="cc737-2764">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2764">JSON example</span></span>

```json
{
    "name": "AzureBatchLinkedService",
    "properties": {
        "type": "AzureBatch",
        "typeProperties": {
            "accountName": "<Azure Batch account name>",
            "accessKey": "<Azure Batch account key>",
            "poolName": "<Azure Batch pool name>",
            "linkedServiceName": "<Specify associated storage linked service reference here>"
        }
    }
}
```

## <a name="azure-machine-learning"></a><span data-ttu-id="cc737-2765">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="cc737-2765">Azure Machine Learning</span></span>
<span data-ttu-id="cc737-2766">A Machine Learning kötegelt pontozási végpont a data Factory regisztrálni Azure Machine Learning társított szolgáltatás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="cc737-2766">You create an Azure Machine Learning linked service to register a Machine Learning batch scoring endpoint with a data factory.</span></span> <span data-ttu-id="cc737-2767">A társított szolgáltatás futtatható ez két adatok átalakítása tevékenységek: [Machine Learning kötegelt végrehajtási tevékenység](#machine-learning-batch-execution-activity), [Machine Learning Update-Erőforrástevékenység](#machine-learning-update-resource-activity).</span><span class="sxs-lookup"><span data-stu-id="cc737-2767">Two data transformation activities that can run on this linked service: [Machine Learning Batch Execution Activity](#machine-learning-batch-execution-activity), [Machine Learning Update Resource Activity](#machine-learning-update-resource-activity).</span></span> 

### <a name="linked-service"></a><span data-ttu-id="cc737-2768">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-2768">Linked service</span></span>
<span data-ttu-id="cc737-2769">A következő táblázat ismerteti az Azure Machine Learning társított szolgáltatásnak Azure JSON-definícióból használt tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="cc737-2769">The following table provides descriptions for the properties used in the Azure JSON definition of an Azure Machine Learning linked service.</span></span>

| <span data-ttu-id="cc737-2770">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2770">Property</span></span> | <span data-ttu-id="cc737-2771">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2771">Description</span></span> | <span data-ttu-id="cc737-2772">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2772">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-2773">Típus</span><span class="sxs-lookup"><span data-stu-id="cc737-2773">Type</span></span> |<span data-ttu-id="cc737-2774">A type tulajdonságot kell megadni: **AzureML**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2774">The type property should be set to: **AzureML**.</span></span> |<span data-ttu-id="cc737-2775">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2775">Yes</span></span> |
| <span data-ttu-id="cc737-2776">mlEndpoint</span><span class="sxs-lookup"><span data-stu-id="cc737-2776">mlEndpoint</span></span> |<span data-ttu-id="cc737-2777">A kötegelt pontozás URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="cc737-2777">The batch scoring URL.</span></span> |<span data-ttu-id="cc737-2778">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2778">Yes</span></span> |
| <span data-ttu-id="cc737-2779">apiKey</span><span class="sxs-lookup"><span data-stu-id="cc737-2779">apiKey</span></span> |<span data-ttu-id="cc737-2780">A közzétett munkaterület-modell API.</span><span class="sxs-lookup"><span data-stu-id="cc737-2780">The published workspace model’s API.</span></span> |<span data-ttu-id="cc737-2781">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2781">Yes</span></span> |

#### <a name="json-example"></a><span data-ttu-id="cc737-2782">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2782">JSON example</span></span>

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://[batch scoring endpoint]/jobs",
            "apiKey": "<apikey>"
        }
    }
}
```

## <a name="azure-data-lake-analytics"></a><span data-ttu-id="cc737-2783">Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="cc737-2783">Azure Data Lake Analytics</span></span>
<span data-ttu-id="cc737-2784">Létrehozhat egy **Azure Data Lake Analytics** társított szolgáltatás az Azure Data Lake Analytics csatolásához számítási egy az Azure data factory szolgáltatás használata előtt a [Data Lake Analytics U-SQL-tevékenység](data-factory-usql-activity.md) egy folyamaton belül.</span><span class="sxs-lookup"><span data-stu-id="cc737-2784">You create an **Azure Data Lake Analytics** linked service to link an Azure Data Lake Analytics compute service to an Azure data factory before using the [Data Lake Analytics U-SQL activity](data-factory-usql-activity.md) in a pipeline.</span></span>

### <a name="linked-service"></a><span data-ttu-id="cc737-2785">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-2785">Linked service</span></span>

<span data-ttu-id="cc737-2786">A következő táblázat ismerteti az Azure Data Lake Analytics társított szolgáltatás JSON-definícióból használt tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="cc737-2786">The following table provides descriptions for the properties used in the JSON definition of an Azure Data Lake Analytics linked service.</span></span> 

| <span data-ttu-id="cc737-2787">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2787">Property</span></span> | <span data-ttu-id="cc737-2788">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2788">Description</span></span> | <span data-ttu-id="cc737-2789">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2789">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-2790">Típus</span><span class="sxs-lookup"><span data-stu-id="cc737-2790">Type</span></span> |<span data-ttu-id="cc737-2791">A type tulajdonságot kell megadni: **AzureDataLakeAnalytics**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2791">The type property should be set to: **AzureDataLakeAnalytics**.</span></span> |<span data-ttu-id="cc737-2792">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2792">Yes</span></span> |
| <span data-ttu-id="cc737-2793">Fióknév</span><span class="sxs-lookup"><span data-stu-id="cc737-2793">accountName</span></span> |<span data-ttu-id="cc737-2794">Az Azure Data Lake Analytics-fiók neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-2794">Azure Data Lake Analytics Account Name.</span></span> |<span data-ttu-id="cc737-2795">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2795">Yes</span></span> |
| <span data-ttu-id="cc737-2796">datalakeanalyticsuri paraméter</span><span class="sxs-lookup"><span data-stu-id="cc737-2796">dataLakeAnalyticsUri</span></span> |<span data-ttu-id="cc737-2797">Az Azure Data Lake Analytics URI.</span><span class="sxs-lookup"><span data-stu-id="cc737-2797">Azure Data Lake Analytics URI.</span></span> |<span data-ttu-id="cc737-2798">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2798">No</span></span> |
| <span data-ttu-id="cc737-2799">Engedélyezési</span><span class="sxs-lookup"><span data-stu-id="cc737-2799">authorization</span></span> |<span data-ttu-id="cc737-2800">Engedélyezési kód automatikusan beolvassa kattintás után **engedélyezés** a Data Factory Editor gombra, és az OAuth-bejelentkezés befejezése.</span><span class="sxs-lookup"><span data-stu-id="cc737-2800">Authorization code is automatically retrieved after clicking **Authorize** button in the Data Factory Editor and completing the OAuth login.</span></span> |<span data-ttu-id="cc737-2801">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2801">Yes</span></span> |
| <span data-ttu-id="cc737-2802">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="cc737-2802">subscriptionId</span></span> |<span data-ttu-id="cc737-2803">Az Azure előfizetés-azonosító</span><span class="sxs-lookup"><span data-stu-id="cc737-2803">Azure subscription id</span></span> |<span data-ttu-id="cc737-2804">Nem (Ha nincs megadva, a data factory-előfizetése szerepel).</span><span class="sxs-lookup"><span data-stu-id="cc737-2804">No (If not specified, subscription of the data factory is used).</span></span> |
| <span data-ttu-id="cc737-2805">erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="cc737-2805">resourceGroupName</span></span> |<span data-ttu-id="cc737-2806">Azure erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="cc737-2806">Azure resource group name</span></span> |<span data-ttu-id="cc737-2807">Nem (Ha nincs megadva, az adat-előállító erőforráscsoport szerepel).</span><span class="sxs-lookup"><span data-stu-id="cc737-2807">No (If not specified, resource group of the data factory is used).</span></span> |
| <span data-ttu-id="cc737-2808">Munkamenet-azonosító</span><span class="sxs-lookup"><span data-stu-id="cc737-2808">sessionId</span></span> |<span data-ttu-id="cc737-2809">munkamenet-azonosító az OAuth hitelesítési munkamenetből.</span><span class="sxs-lookup"><span data-stu-id="cc737-2809">session id from the OAuth authorization session.</span></span> <span data-ttu-id="cc737-2810">Minden munkamenet-azonosító egyedi, és előfordulhat, hogy csak egyszer használható.</span><span class="sxs-lookup"><span data-stu-id="cc737-2810">Each session id is unique and may only be used once.</span></span> <span data-ttu-id="cc737-2811">A Data Factory Editor használata esetén ezt az Azonosítót jön létre automatikusan.</span><span class="sxs-lookup"><span data-stu-id="cc737-2811">When you use the Data Factory Editor, this ID is auto-generated.</span></span> |<span data-ttu-id="cc737-2812">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2812">Yes</span></span> |


#### <a name="json-example"></a><span data-ttu-id="cc737-2813">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2813">JSON example</span></span>
<span data-ttu-id="cc737-2814">Az alábbi példa biztosít az Azure Data Lake Analytics társított szolgáltatás JSON-definícióból.</span><span class="sxs-lookup"><span data-stu-id="cc737-2814">The following example provides JSON definition for an Azure Data Lake Analytics linked service.</span></span>

```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "<account name>",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>",
            "subscriptionId": "<subscription id>",
            "resourceGroupName": "<resource group name>"
        }
    }
}
```

## <a name="azure-sql-database"></a><span data-ttu-id="cc737-2815">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="cc737-2815">Azure SQL Database</span></span>
<span data-ttu-id="cc737-2816">Hozzon létre egy Azure SQL társított szolgáltatást, és együtt használja a [tárolt eljárási tevékenység](#stored-procedure-activity) meghívni a Data Factory-folyamat a tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="cc737-2816">You create an Azure SQL linked service and use it with the [Stored Procedure Activity](#stored-procedure-activity) to invoke a stored procedure from a Data Factory pipeline.</span></span> 

### <a name="linked-service"></a><span data-ttu-id="cc737-2817">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-2817">Linked service</span></span>
<span data-ttu-id="cc737-2818">Egy Azure SQL Database meghatározásához társított szolgáltatás, állítsa be a **típus** a társított szolgáltatás **AzureSqlDatabase**, és adja meg a következő tulajdonságokat a **typeProperties** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-2818">To define an Azure SQL Database linked service, set the **type** of the linked service to **AzureSqlDatabase**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="cc737-2819">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2819">Property</span></span> | <span data-ttu-id="cc737-2820">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2820">Description</span></span> | <span data-ttu-id="cc737-2821">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2821">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-2822">connectionString</span><span class="sxs-lookup"><span data-stu-id="cc737-2822">connectionString</span></span> |<span data-ttu-id="cc737-2823">Adja meg az Azure SQL Database-példányt a connectionString tulajdonság való kapcsolódáshoz szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="cc737-2823">Specify information needed to connect to the Azure SQL Database instance for the connectionString property.</span></span> |<span data-ttu-id="cc737-2824">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2824">Yes</span></span> |

#### <a name="json-example"></a><span data-ttu-id="cc737-2825">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2825">JSON example</span></span>

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

<span data-ttu-id="cc737-2826">Lásd: [Azure SQL-összekötő](data-factory-azure-sql-connector.md#linked-service-properties) szóló cikkben olvashat a szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="cc737-2826">See [Azure SQL Connector](data-factory-azure-sql-connector.md#linked-service-properties) article for details about this linked service.</span></span>

## <a name="azure-sql-data-warehouse"></a><span data-ttu-id="cc737-2827">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="cc737-2827">Azure SQL Data Warehouse</span></span>
<span data-ttu-id="cc737-2828">Hozzon létre egy Azure SQL Data Warehouse társított szolgáltatást, és együtt használja a [tárolt eljárási tevékenység](data-factory-stored-proc-activity.md) meghívni a Data Factory-folyamat a tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="cc737-2828">You create an Azure SQL Data Warehouse linked service and use it with the [Stored Procedure Activity](data-factory-stored-proc-activity.md) to invoke a stored procedure from a Data Factory pipeline.</span></span> 

### <a name="linked-service"></a><span data-ttu-id="cc737-2829">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-2829">Linked service</span></span>
<span data-ttu-id="cc737-2830">Azure SQL Data Warehouse meghatározásához társított szolgáltatás, állítsa be a **típus** a társított szolgáltatás **AzureSqlDW**, és adja meg a következő tulajdonságokat a **typeProperties** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="cc737-2830">To define an Azure SQL Data Warehouse linked service, set the **type** of the linked service to **AzureSqlDW**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="cc737-2831">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2831">Property</span></span> | <span data-ttu-id="cc737-2832">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2832">Description</span></span> | <span data-ttu-id="cc737-2833">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2833">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-2834">connectionString</span><span class="sxs-lookup"><span data-stu-id="cc737-2834">connectionString</span></span> |<span data-ttu-id="cc737-2835">Adja meg a connectionString tulajdonság az Azure SQL Data Warehouse-példány való kapcsolódáshoz szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="cc737-2835">Specify information needed to connect to the Azure SQL Data Warehouse instance for the connectionString property.</span></span> |<span data-ttu-id="cc737-2836">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2836">Yes</span></span> |

#### <a name="json-example"></a><span data-ttu-id="cc737-2837">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2837">JSON example</span></span>

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

<span data-ttu-id="cc737-2838">További információkért lásd: [Azure SQL Data Warehouse-összekötő](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2838">For more information, see [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) article.</span></span> 

## <a name="sql-server"></a><span data-ttu-id="cc737-2839">SQL Server</span><span class="sxs-lookup"><span data-stu-id="cc737-2839">SQL Server</span></span> 
<span data-ttu-id="cc737-2840">Hozzon létre csatolt SQL Server szolgáltatást, és együtt használja a [tárolt eljárási tevékenység](data-factory-stored-proc-activity.md) meghívni a Data Factory-folyamat a tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="cc737-2840">You create a SQL Server linked service and use it with the [Stored Procedure Activity](data-factory-stored-proc-activity.md) to invoke a stored procedure from a Data Factory pipeline.</span></span> 

### <a name="linked-service"></a><span data-ttu-id="cc737-2841">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cc737-2841">Linked service</span></span>
<span data-ttu-id="cc737-2842">Típusú társított szolgáltatás létrehozása **OnPremisesSqlServer** a helyszíni SQL Server adatbázis összekapcsolása egy adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="cc737-2842">You create a linked service of type **OnPremisesSqlServer** to link an on-premises SQL Server database to a data factory.</span></span> <span data-ttu-id="cc737-2843">A következő táblázat a JSON-elemek szerepelnek a helyszíni SQL Server kapcsolódó szolgáltatásra vonatkozó leírást.</span><span class="sxs-lookup"><span data-stu-id="cc737-2843">The following table provides description for JSON elements specific to on-premises SQL Server linked service.</span></span>

<span data-ttu-id="cc737-2844">A következő táblázat a JSON-elemek szerepelnek kapcsolódó SQL Server-szolgáltatásra vonatkozó leírást.</span><span class="sxs-lookup"><span data-stu-id="cc737-2844">The following table provides description for JSON elements specific to SQL Server linked service.</span></span>

| <span data-ttu-id="cc737-2845">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2845">Property</span></span> | <span data-ttu-id="cc737-2846">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2846">Description</span></span> | <span data-ttu-id="cc737-2847">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2847">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-2848">type</span><span class="sxs-lookup"><span data-stu-id="cc737-2848">type</span></span> |<span data-ttu-id="cc737-2849">A type tulajdonságot kell megadni: **OnPremisesSqlServer**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2849">The type property should be set to: **OnPremisesSqlServer**.</span></span> |<span data-ttu-id="cc737-2850">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2850">Yes</span></span> |
| <span data-ttu-id="cc737-2851">connectionString</span><span class="sxs-lookup"><span data-stu-id="cc737-2851">connectionString</span></span> |<span data-ttu-id="cc737-2852">Az SQL-hitelesítéssel vagy a Windows-hitelesítés a helyszíni SQL Server adatbázishoz való kapcsolódáshoz szükséges connectionString információkat adják meg.</span><span class="sxs-lookup"><span data-stu-id="cc737-2852">Specify connectionString information needed to connect to the on-premises SQL Server database using either SQL authentication or Windows authentication.</span></span> |<span data-ttu-id="cc737-2853">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2853">Yes</span></span> |
| <span data-ttu-id="cc737-2854">gatewayName</span><span class="sxs-lookup"><span data-stu-id="cc737-2854">gatewayName</span></span> |<span data-ttu-id="cc737-2855">Neve az átjáró, amely a Data Factory szolgáltatásnak csatlakoznia kell a helyszíni SQL Server adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="cc737-2855">Name of the gateway that the Data Factory service should use to connect to the on-premises SQL Server database.</span></span> |<span data-ttu-id="cc737-2856">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2856">Yes</span></span> |
| <span data-ttu-id="cc737-2857">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="cc737-2857">username</span></span> |<span data-ttu-id="cc737-2858">Adja meg a felhasználónevet, ha a Windows-hitelesítést használ.</span><span class="sxs-lookup"><span data-stu-id="cc737-2858">Specify user name if you are using Windows Authentication.</span></span> <span data-ttu-id="cc737-2859">Példa: **tartománynév\\felhasználónév**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2859">Example: **domainname\\username**.</span></span> |<span data-ttu-id="cc737-2860">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2860">No</span></span> |
| <span data-ttu-id="cc737-2861">jelszó</span><span class="sxs-lookup"><span data-stu-id="cc737-2861">password</span></span> |<span data-ttu-id="cc737-2862">Adja meg a felhasználónévhez megadott felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="cc737-2862">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="cc737-2863">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2863">No</span></span> |

<span data-ttu-id="cc737-2864">Hitelesítő adatok használatával titkosíthatja az **New-AzureRmDataFactoryEncryptValue** parancsmag és a következő példában látható módon használhatja őket a kapcsolódási karakterláncban (**EncryptedCredential** tulajdonság):</span><span class="sxs-lookup"><span data-stu-id="cc737-2864">You can encrypt credentials using the **New-AzureRmDataFactoryEncryptValue** cmdlet and use them in the connection string as shown in the following example (**EncryptedCredential** property):</span></span>  

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```


#### <a name="example-json-for-using-sql-authentication"></a><span data-ttu-id="cc737-2865">Példa: JSON az SQL-hitelesítéssel</span><span class="sxs-lookup"><span data-stu-id="cc737-2865">Example: JSON for using SQL Authentication</span></span>

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
#### <a name="example-json-for-using-windows-authentication"></a><span data-ttu-id="cc737-2866">Példa: JSON a Windows-hitelesítés használatával</span><span class="sxs-lookup"><span data-stu-id="cc737-2866">Example: JSON for using Windows Authentication</span></span>

<span data-ttu-id="cc737-2867">Ha a felhasználónév és jelszó van adva, átjáró őket a megszemélyesítéshez használ a megadott felhasználói fióknak a helyi SQL Server-adatbázishoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="cc737-2867">If username and password are specified, gateway uses them to impersonate the specified user account to connect to the on-premises SQL Server database.</span></span> <span data-ttu-id="cc737-2868">Ellenkező esetben átjáró csatlakozik az SQL Server közvetlenül az átjáró (az indítási fiókjához) biztonsági környezetében.</span><span class="sxs-lookup"><span data-stu-id="cc737-2868">Otherwise, gateway connects to the SQL Server directly with the security context of Gateway (its startup account).</span></span>

```json
{
    "Name": " MyOnPremisesSQLDB",
    "Properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
            "username": "<domain\\username>",
            "password": "<password>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

<span data-ttu-id="cc737-2869">További információkért lásd: [SQL Server-összekötő](data-factory-sqlserver-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2869">For more information, see [SQL Server connector](data-factory-sqlserver-connector.md#linked-service-properties) article.</span></span>

## <a name="data-transformation-activities"></a><span data-ttu-id="cc737-2870">ADATOK ÁTALAKÍTÁSA TEVÉKENYSÉGEK</span><span class="sxs-lookup"><span data-stu-id="cc737-2870">DATA TRANSFORMATION ACTIVITIES</span></span>

<span data-ttu-id="cc737-2871">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="cc737-2871">Activity</span></span> | <span data-ttu-id="cc737-2872">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2872">Description</span></span>
-------- | -----------
[<span data-ttu-id="cc737-2873">HDInsight Hive tevékenység</span><span class="sxs-lookup"><span data-stu-id="cc737-2873">HDInsight Hive activity</span></span>](#hdinsight-hive-activity) | <span data-ttu-id="cc737-2874">A HDInsight Hive tevékenység a Data Factory-folyamat saját Hive-lekérdezéseket vagy igény szerinti Windows/Linux-alapú HDInsight-fürt hajt végre.</span><span class="sxs-lookup"><span data-stu-id="cc737-2874">The HDInsight Hive activity in a Data Factory pipeline executes Hive queries on your own or on-demand Windows/Linux-based HDInsight cluster.</span></span> 
[<span data-ttu-id="cc737-2875">HDInsight Pig tevékenység</span><span class="sxs-lookup"><span data-stu-id="cc737-2875">HDInsight Pig activity</span></span>](#hdinsight-pig-activity) | <span data-ttu-id="cc737-2876">A HDInsight Pig tevékenység a Data Factory-folyamat saját Pig lekérdezések vagy igény szerinti Windows/Linux-alapú HDInsight-fürt hajt végre.</span><span class="sxs-lookup"><span data-stu-id="cc737-2876">The HDInsight Pig activity in a Data Factory pipeline executes Pig queries on your own or on-demand Windows/Linux-based HDInsight cluster.</span></span>
[<span data-ttu-id="cc737-2877">HDInsight MapReduce-tevékenység</span><span class="sxs-lookup"><span data-stu-id="cc737-2877">HDInsight MapReduce Activity</span></span>](#hdinsight-mapreduce-activity) | <span data-ttu-id="cc737-2878">A HDInsight MapReduce művelethez a Data Factory-folyamat saját MapReduce programok vagy igény szerinti Windows/Linux-alapú HDInsight-fürt hajt végre.</span><span class="sxs-lookup"><span data-stu-id="cc737-2878">The HDInsight MapReduce activity in a Data Factory pipeline executes MapReduce programs on your own or on-demand Windows/Linux-based HDInsight cluster.</span></span>
[<span data-ttu-id="cc737-2879">HDInsight Streaming-tevékenység</span><span class="sxs-lookup"><span data-stu-id="cc737-2879">HDInsight Streaming Activity</span></span>](#hdinsight-streaming-activity) | <span data-ttu-id="cc737-2880">A HDInsight Streamelési tevékenységben a Data Factory-folyamat saját Hadoop Streamelési programok vagy igény szerinti Windows/Linux-alapú HDInsight-fürt hajt végre.</span><span class="sxs-lookup"><span data-stu-id="cc737-2880">The HDInsight Streaming Activity in a Data Factory pipeline executes Hadoop Streaming programs on your own or on-demand Windows/Linux-based HDInsight cluster.</span></span>
[<span data-ttu-id="cc737-2881">HDInsight Spark-tevékenység</span><span class="sxs-lookup"><span data-stu-id="cc737-2881">HDInsight Spark Activity</span></span>](#hdinsight-spark-activity) | <span data-ttu-id="cc737-2882">A HDInsight Spark tevékenység a Data Factory-folyamat saját HDInsight-fürt végrehajtja a Spark programokat.</span><span class="sxs-lookup"><span data-stu-id="cc737-2882">The HDInsight Spark activity in a Data Factory pipeline executes Spark programs on your own HDInsight cluster.</span></span> 
[<span data-ttu-id="cc737-2883">Machine Learning kötegelt végrehajtási tevékenység</span><span class="sxs-lookup"><span data-stu-id="cc737-2883">Machine Learning Batch Execution Activity</span></span>](#machine-learning-batch-execution-activity) | <span data-ttu-id="cc737-2884">Az Azure Data Factory lehetővé teszi, hogy könnyen létrehozhat egy közzétett Azure Machine Learning webszolgáltatás prediktív elemzési használó folyamatok.</span><span class="sxs-lookup"><span data-stu-id="cc737-2884">Azure Data Factory enables you to easily create pipelines that use a published Azure Machine Learning web service for predictive analytics.</span></span> <span data-ttu-id="cc737-2885">A kötegelt végrehajtási tevékenység használja az Azure Data Factory-folyamathoz, hívhat meg a Machine Learning webszolgáltatás előrejelzéseket készítsen a kötegben lévő adatokat.</span><span class="sxs-lookup"><span data-stu-id="cc737-2885">Using the Batch Execution Activity in an Azure Data Factory pipeline, you can invoke a Machine Learning web service to make predictions on the data in batch.</span></span> 
[<span data-ttu-id="cc737-2886">Machine Learning Update-erőforrástevékenység</span><span class="sxs-lookup"><span data-stu-id="cc737-2886">Machine Learning Update Resource Activity</span></span>](#machine-learning-update-resource-activity) | <span data-ttu-id="cc737-2887">Az idő múlásával a Machine Learning scoring-kísérletek a prediktív modellek kell kell retrained új bemeneti adatkészletek használata.</span><span class="sxs-lookup"><span data-stu-id="cc737-2887">Over time, the predictive models in the Machine Learning scoring experiments need to be retrained using new input datasets.</span></span> <span data-ttu-id="cc737-2888">Miután elkészült, az átképezési, retrained gépi tanulási modell a pontozási webszolgáltatás frissíteni kívánt.</span><span class="sxs-lookup"><span data-stu-id="cc737-2888">After you are done with retraining, you want to update the scoring web service with the retrained Machine Learning model.</span></span> <span data-ttu-id="cc737-2889">A frissítés erőforrás tevékenység segítségével a webszolgáltatás frissítenie az újonnan betanított modell.</span><span class="sxs-lookup"><span data-stu-id="cc737-2889">You can use the Update Resource Activity to update the web service with the newly trained model.</span></span>
[<span data-ttu-id="cc737-2890">Tárolt eljárási tevékenység</span><span class="sxs-lookup"><span data-stu-id="cc737-2890">Stored Procedure Activity</span></span>](#stored-procedure-activity) | <span data-ttu-id="cc737-2891">Segítségével a tárolt eljárási tevékenység a Data Factory-folyamat a következő adatokat tárolja egyikében tárolt eljárás hívása: Azure SQL Database, Azure SQL Data Warehouse szolgáltatásban a vállalat SQL Server-adatbázis vagy egy Azure virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="cc737-2891">You can use the Stored Procedure activity in a Data Factory pipeline to invoke a stored procedure in one of the following data stores: Azure SQL Database, Azure SQL Data Warehouse, SQL Server Database in your enterprise or an Azure VM.</span></span> 
[<span data-ttu-id="cc737-2892">Data Lake Analytics U-SQL-tevékenység</span><span class="sxs-lookup"><span data-stu-id="cc737-2892">Data Lake Analytics U-SQL activity</span></span>](#data-lake-analytics-u-sql-activity) | <span data-ttu-id="cc737-2893">Data Lake Analytics U-SQL-tevékenység egy Azure Data Lake Analytics-fürt U-SQL parancsfájlt futtat.</span><span class="sxs-lookup"><span data-stu-id="cc737-2893">Data Lake Analytics U-SQL Activity runs a U-SQL script on an Azure Data Lake Analytics cluster.</span></span>  
[<span data-ttu-id="cc737-2894">.NET egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="cc737-2894">.NET custom activity</span></span>](#net-custom-activity) | <span data-ttu-id="cc737-2895">Ha úgy, hogy nem támogatja a Data Factory adatok átalakítása van szüksége, hozzon létre egy egyéni tevékenység saját adatokat feldolgozó logika, és használja a tevékenységet a feldolgozási.</span><span class="sxs-lookup"><span data-stu-id="cc737-2895">If you need to transform data in a way that is not supported by Data Factory, you can create a custom activity with your own data processing logic and use the activity in the pipeline.</span></span> <span data-ttu-id="cc737-2896">Az egyéni .NET tevékenység segítségével futtatja, az Azure Batch szolgáltatás vagy az Azure HDInsight-fürtöt is konfigurálhat.</span><span class="sxs-lookup"><span data-stu-id="cc737-2896">You can configure the custom .NET activity to run using either an Azure Batch service or an Azure HDInsight cluster.</span></span> 

     
## <a name="hdinsight-hive-activity"></a><span data-ttu-id="cc737-2897">HDInsight Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="cc737-2897">HDInsight Hive Activity</span></span>
<span data-ttu-id="cc737-2898">A következő tulajdonságok is megadhatók a Hive tevékenység JSON-definícióban.</span><span class="sxs-lookup"><span data-stu-id="cc737-2898">You can specify the following properties in a Hive Activity JSON definition.</span></span> <span data-ttu-id="cc737-2899">A type tulajdonságot a tevékenységnek kell lennie: **HDInsightHive**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2899">The type property for the activity must be: **HDInsightHive**.</span></span> <span data-ttu-id="cc737-2900">Létre kell hoznia egy HDInsight kapcsolódó szolgáltatás először és értékeként adja meg a nevét, a **linkedServiceName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="cc737-2900">You must create a HDInsight linked service first and specify the name of it as a value for the **linkedServiceName** property.</span></span> <span data-ttu-id="cc737-2901">A következő tulajdonságok támogatottak a **typeProperties** szakasz tevékenység HDInsightHive beállításakor:</span><span class="sxs-lookup"><span data-stu-id="cc737-2901">The following properties are supported in the **typeProperties** section when you set the type of activity to HDInsightHive:</span></span>

| <span data-ttu-id="cc737-2902">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2902">Property</span></span> | <span data-ttu-id="cc737-2903">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2903">Description</span></span> | <span data-ttu-id="cc737-2904">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2904">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-2905">Parancsfájl</span><span class="sxs-lookup"><span data-stu-id="cc737-2905">script</span></span> |<span data-ttu-id="cc737-2906">Adja meg a Hive parancsfájl beágyazott</span><span class="sxs-lookup"><span data-stu-id="cc737-2906">Specify the Hive script inline</span></span> |<span data-ttu-id="cc737-2907">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2907">No</span></span> |
| <span data-ttu-id="cc737-2908">parancsfájl elérési útja</span><span class="sxs-lookup"><span data-stu-id="cc737-2908">script path</span></span> |<span data-ttu-id="cc737-2909">A Hive-parancsfájl az Azure blob Storage tárolóban tárolja, és adja meg a fájl elérési útját.</span><span class="sxs-lookup"><span data-stu-id="cc737-2909">Store the Hive script in an Azure blob storage and provide the path to the file.</span></span> <span data-ttu-id="cc737-2910">Használja a "script" vagy "scriptPath" tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="cc737-2910">Use 'script' or 'scriptPath' property.</span></span> <span data-ttu-id="cc737-2911">Mindkettő nem használható együtt.</span><span class="sxs-lookup"><span data-stu-id="cc737-2911">Both cannot be used together.</span></span> <span data-ttu-id="cc737-2912">A fájlnév pedig kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="cc737-2912">The file name is case-sensitive.</span></span> |<span data-ttu-id="cc737-2913">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2913">No</span></span> |
| <span data-ttu-id="cc737-2914">határozza meg</span><span class="sxs-lookup"><span data-stu-id="cc737-2914">defines</span></span> |<span data-ttu-id="cc737-2915">Kulcs/érték párok paraméterek meghatározni a Hive-parancsfájl segítségével történő "hiveconf" belül hivatkozik</span><span class="sxs-lookup"><span data-stu-id="cc737-2915">Specify parameters as key/value pairs for referencing within the Hive script using 'hiveconf'</span></span> |<span data-ttu-id="cc737-2916">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2916">No</span></span> |

<span data-ttu-id="cc737-2917">A típus tulajdonságokat. a struktúra tevékenység vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="cc737-2917">These type properties are specific to the Hive Activity.</span></span> <span data-ttu-id="cc737-2918">Az összes tevékenység egyéb tulajdonságok (kívül a typeProperties szakaszban) támogatottak.</span><span class="sxs-lookup"><span data-stu-id="cc737-2918">Other properties (outside the typeProperties section) are supported for all activities.</span></span>   

### <a name="json-example"></a><span data-ttu-id="cc737-2919">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2919">JSON example</span></span>
<span data-ttu-id="cc737-2920">A következő JSON egy HDInsight Hive tevékenységet egy folyamaton belül határozza meg.</span><span class="sxs-lookup"><span data-stu-id="cc737-2920">The following JSON defines a HDInsight Hive activity in a pipeline.</span></span>  

```json
{
    "name": "Hive Activity",
    "description": "description",
    "type": "HDInsightHive",
    "inputs": [
      {
        "name": "input tables"
      }
    ],
    "outputs": [
      {
        "name": "output tables"
      }
    ],
    "linkedServiceName": "MyHDInsightLinkedService",
    "typeProperties": {
      "script": "Hive script",
      "scriptPath": "<pathtotheHivescriptfileinAzureblobstorage>",
      "defines": {
        "param1": "param1Value"
      }
    },
   "scheduler": {
      "frequency": "Day",
      "interval": 1
    }
}
```

<span data-ttu-id="cc737-2921">További információkért lásd: [Hive tevékenység](data-factory-hive-activity.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2921">For more information, see [Hive Activity](data-factory-hive-activity.md) article.</span></span> 

## <a name="hdinsight-pig-activity"></a><span data-ttu-id="cc737-2922">HDInsight Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="cc737-2922">HDInsight Pig Activity</span></span>
<span data-ttu-id="cc737-2923">A következő tulajdonságok is megadhatók a Pig tevékenység JSON-definícióban.</span><span class="sxs-lookup"><span data-stu-id="cc737-2923">You can specify the following properties in a Pig Activity JSON definition.</span></span> <span data-ttu-id="cc737-2924">A type tulajdonságot a tevékenységnek kell lennie: **HDInsightPig**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2924">The type property for the activity must be: **HDInsightPig**.</span></span> <span data-ttu-id="cc737-2925">Létre kell hoznia egy HDInsight kapcsolódó szolgáltatás először és értékeként adja meg a nevét, a **linkedServiceName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="cc737-2925">You must create a HDInsight linked service first and specify the name of it as a value for the **linkedServiceName** property.</span></span> <span data-ttu-id="cc737-2926">A következő tulajdonságok támogatottak a **typeProperties** szakasz tevékenység HDInsightPig beállításakor:</span><span class="sxs-lookup"><span data-stu-id="cc737-2926">The following properties are supported in the **typeProperties** section when you set the type of activity to HDInsightPig:</span></span> 

| <span data-ttu-id="cc737-2927">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2927">Property</span></span> | <span data-ttu-id="cc737-2928">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2928">Description</span></span> | <span data-ttu-id="cc737-2929">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2929">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-2930">Parancsfájl</span><span class="sxs-lookup"><span data-stu-id="cc737-2930">script</span></span> |<span data-ttu-id="cc737-2931">Adja meg a Pig-parancsprogram beágyazott</span><span class="sxs-lookup"><span data-stu-id="cc737-2931">Specify the Pig script inline</span></span> |<span data-ttu-id="cc737-2932">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2932">No</span></span> |
| <span data-ttu-id="cc737-2933">parancsfájl elérési útja</span><span class="sxs-lookup"><span data-stu-id="cc737-2933">script path</span></span> |<span data-ttu-id="cc737-2934">A Pig-parancsprogram egy Azure blob Storage tárolóban tárolja, és adja meg a fájl elérési útját.</span><span class="sxs-lookup"><span data-stu-id="cc737-2934">Store the Pig script in an Azure blob storage and provide the path to the file.</span></span> <span data-ttu-id="cc737-2935">Használja a "script" vagy "scriptPath" tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="cc737-2935">Use 'script' or 'scriptPath' property.</span></span> <span data-ttu-id="cc737-2936">Mindkettő nem használható együtt.</span><span class="sxs-lookup"><span data-stu-id="cc737-2936">Both cannot be used together.</span></span> <span data-ttu-id="cc737-2937">A fájlnév pedig kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="cc737-2937">The file name is case-sensitive.</span></span> |<span data-ttu-id="cc737-2938">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2938">No</span></span> |
| <span data-ttu-id="cc737-2939">határozza meg</span><span class="sxs-lookup"><span data-stu-id="cc737-2939">defines</span></span> |<span data-ttu-id="cc737-2940">Kulcs/érték párok paraméterek meghatározni a Pig-parancsprogram belül hivatkozik</span><span class="sxs-lookup"><span data-stu-id="cc737-2940">Specify parameters as key/value pairs for referencing within the Pig script</span></span> |<span data-ttu-id="cc737-2941">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2941">No</span></span> |

<span data-ttu-id="cc737-2942">A típus tulajdonságokat. a Pig tevékenység vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="cc737-2942">These type properties are specific to the Pig Activity.</span></span> <span data-ttu-id="cc737-2943">Az összes tevékenység egyéb tulajdonságok (kívül a typeProperties szakaszban) támogatottak.</span><span class="sxs-lookup"><span data-stu-id="cc737-2943">Other properties (outside the typeProperties section) are supported for all activities.</span></span>   

### <a name="json-example"></a><span data-ttu-id="cc737-2944">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2944">JSON example</span></span>

```json
{
    "name": "HiveActivitySamplePipeline",
      "properties": {
    "activities": [
        {
            "name": "Pig Activity",
            "description": "description",
            "type": "HDInsightPig",
            "inputs": [
                  {
                    "name": "input tables"
                  }
            ],
            "outputs": [
                  {
                    "name": "output tables"
                  }
            ],
            "linkedServiceName": "MyHDInsightLinkedService",
            "typeProperties": {
                  "script": "Pig script",
                  "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
                  "defines": {
                    "param1": "param1Value"
                  }
            },
               "scheduler": {
                  "frequency": "Day",
                  "interval": 1
            }
          }
    ]
  }
}
```

<span data-ttu-id="cc737-2945">További információkért lásd: [Pig tevékenység](#data-factory-pig-activity.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2945">For more information, see [Pig Activity](#data-factory-pig-activity.md) article.</span></span> 

## <a name="hdinsight-mapreduce-activity"></a><span data-ttu-id="cc737-2946">HDInsight MapReduce-tevékenység</span><span class="sxs-lookup"><span data-stu-id="cc737-2946">HDInsight MapReduce Activity</span></span>
<span data-ttu-id="cc737-2947">A következő tulajdonságok is megadhatók a MapReduce tevékenység JSON-definícióban.</span><span class="sxs-lookup"><span data-stu-id="cc737-2947">You can specify the following properties in a MapReduce Activity JSON definition.</span></span> <span data-ttu-id="cc737-2948">A type tulajdonságot a tevékenységnek kell lennie: **HDInsightMapReduce**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2948">The type property for the activity must be: **HDInsightMapReduce**.</span></span> <span data-ttu-id="cc737-2949">Létre kell hoznia egy HDInsight kapcsolódó szolgáltatás először és értékeként adja meg a nevét, a **linkedServiceName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="cc737-2949">You must create a HDInsight linked service first and specify the name of it as a value for the **linkedServiceName** property.</span></span> <span data-ttu-id="cc737-2950">A következő tulajdonságok támogatottak a **typeProperties** szakasz tevékenység HDInsightMapReduce beállításakor:</span><span class="sxs-lookup"><span data-stu-id="cc737-2950">The following properties are supported in the **typeProperties** section when you set the type of activity to HDInsightMapReduce:</span></span> 

| <span data-ttu-id="cc737-2951">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2951">Property</span></span> | <span data-ttu-id="cc737-2952">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2952">Description</span></span> | <span data-ttu-id="cc737-2953">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-2953">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-2954">jarLinkedService</span><span class="sxs-lookup"><span data-stu-id="cc737-2954">jarLinkedService</span></span> | <span data-ttu-id="cc737-2955">Az Azure Storage a JAR-fájlt tartalmazó a társított szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-2955">Name of the linked service for the Azure Storage that contains the JAR file.</span></span> | <span data-ttu-id="cc737-2956">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2956">Yes</span></span> |
| <span data-ttu-id="cc737-2957">jarfilepath tulajdonságot</span><span class="sxs-lookup"><span data-stu-id="cc737-2957">jarFilePath</span></span> | <span data-ttu-id="cc737-2958">Az Azure Storage a JAR-fájl elérési útja.</span><span class="sxs-lookup"><span data-stu-id="cc737-2958">Path to the JAR file in the Azure Storage.</span></span> | <span data-ttu-id="cc737-2959">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2959">Yes</span></span> | 
| <span data-ttu-id="cc737-2960">Osztálynév</span><span class="sxs-lookup"><span data-stu-id="cc737-2960">className</span></span> | <span data-ttu-id="cc737-2961">Neve a fő osztály a JAR-fájlra.</span><span class="sxs-lookup"><span data-stu-id="cc737-2961">Name of the main class in the JAR file.</span></span> | <span data-ttu-id="cc737-2962">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-2962">Yes</span></span> | 
| <span data-ttu-id="cc737-2963">Argumentumok</span><span class="sxs-lookup"><span data-stu-id="cc737-2963">arguments</span></span> | <span data-ttu-id="cc737-2964">A MapReduce program argumentumai vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="cc737-2964">A list of comma-separated arguments for the MapReduce program.</span></span> <span data-ttu-id="cc737-2965">Futásidőben, néhány további argumentumok látja (például: mapreduce.job.tags) a a MapReduce keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="cc737-2965">At runtime, you see a few extra arguments (for example: mapreduce.job.tags) from the MapReduce framework.</span></span> <span data-ttu-id="cc737-2966">A MapReduce argumentumok argumentumok megkülönböztetéséhez érdemes beállítás és value elemeit is argumentumként a következő példában látható módon (- s,--bemeneti,--kimeneti stb., amelyet közvetlenül az értékeik lehetőség áll)</span><span class="sxs-lookup"><span data-stu-id="cc737-2966">To differentiate your arguments with the MapReduce arguments, consider using both option and value as arguments as shown in the following example (-s, --input, --output etc., are options immediately followed by their values)</span></span> | <span data-ttu-id="cc737-2967">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-2967">No</span></span> | 

### <a name="json-example"></a><span data-ttu-id="cc737-2968">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="cc737-2968">JSON example</span></span>

```json
{
    "name": "MahoutMapReduceSamplePipeline",
    "properties": {
        "description": "Sample Pipeline to Run a Mahout Custom Map Reduce Jar. This job calculates an Item Similarity Matrix to determine the similarity between two items",
        "activities": [
            {
                "type": "HDInsightMapReduce",
                "typeProperties": {
                    "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                    "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                    "jarLinkedService": "StorageLinkedService",
                    "arguments": ["-s", "SIMILARITY_LOGLIKELIHOOD", "--input", "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input", "--output", "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/", "--maxSimilaritiesPerItem", "500", "--tempDir", "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"]
                },
                "inputs": [
                    {
                        "name": "MahoutInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "MahoutOutput"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "MahoutActivity",
                "description": "Custom Map Reduce to generate Mahout result",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-01-03T00:00:00",
        "end": "2017-01-04T00:00:00"
    }
}
```

<span data-ttu-id="cc737-2969">További információkért lásd: [MapReduce művelethez](data-factory-map-reduce.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-2969">For more information, see [MapReduce Activity](data-factory-map-reduce.md) article.</span></span> 

## <a name="hdinsight-streaming-activity"></a><span data-ttu-id="cc737-2970">HDInsight Streaming-tevékenység</span><span class="sxs-lookup"><span data-stu-id="cc737-2970">HDInsight Streaming Activity</span></span>
<span data-ttu-id="cc737-2971">A következő tulajdonságok is megadhatók a Hadoop Streamelési tevékenység JSON-definícióban.</span><span class="sxs-lookup"><span data-stu-id="cc737-2971">You can specify the following properties in a Hadoop Streaming Activity JSON definition.</span></span> <span data-ttu-id="cc737-2972">A type tulajdonságot a tevékenységnek kell lennie: **HDInsightStreaming**.</span><span class="sxs-lookup"><span data-stu-id="cc737-2972">The type property for the activity must be: **HDInsightStreaming**.</span></span> <span data-ttu-id="cc737-2973">Létre kell hoznia egy HDInsight kapcsolódó szolgáltatás először és értékeként adja meg a nevét, a **linkedServiceName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="cc737-2973">You must create a HDInsight linked service first and specify the name of it as a value for the **linkedServiceName** property.</span></span> <span data-ttu-id="cc737-2974">A következő tulajdonságok támogatottak a **typeProperties** szakasz tevékenység HDInsightStreaming beállításakor:</span><span class="sxs-lookup"><span data-stu-id="cc737-2974">The following properties are supported in the **typeProperties** section when you set the type of activity to HDInsightStreaming:</span></span> 

| <span data-ttu-id="cc737-2975">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-2975">Property</span></span> | <span data-ttu-id="cc737-2976">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-2976">Description</span></span> | 
| --- | --- |
| <span data-ttu-id="cc737-2977">eseményleképező</span><span class="sxs-lookup"><span data-stu-id="cc737-2977">mapper</span></span> | <span data-ttu-id="cc737-2978">A végrehajtható hozzárendelő neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-2978">Name of the mapper executable.</span></span> <span data-ttu-id="cc737-2979">A példában a cat.exe végrehajtható leképező.</span><span class="sxs-lookup"><span data-stu-id="cc737-2979">In the example, cat.exe is the mapper executable.</span></span>| 
| <span data-ttu-id="cc737-2980">Nyomáscsökkentő</span><span class="sxs-lookup"><span data-stu-id="cc737-2980">reducer</span></span> | <span data-ttu-id="cc737-2981">A végrehajtható nyomáscsökkentő neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-2981">Name of the reducer executable.</span></span> <span data-ttu-id="cc737-2982">A példában a wc.exe végrehajtható nyomáscsökkentő.</span><span class="sxs-lookup"><span data-stu-id="cc737-2982">In the example, wc.exe is the reducer executable.</span></span> | 
| <span data-ttu-id="cc737-2983">Bemeneti</span><span class="sxs-lookup"><span data-stu-id="cc737-2983">input</span></span> | <span data-ttu-id="cc737-2984">A leképező bemeneti (beleértve a hely) fájl.</span><span class="sxs-lookup"><span data-stu-id="cc737-2984">Input file (including location) for the mapper.</span></span> <span data-ttu-id="cc737-2985">A példában: "wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample a blob-tároló, például/data/Gutenberg az a mappa, pedig davinci.txt a blob.</span><span class="sxs-lookup"><span data-stu-id="cc737-2985">In the example: "wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample is the blob container, example/data/Gutenberg is the folder, and davinci.txt is the blob.</span></span> |
| <span data-ttu-id="cc737-2986">Kimeneti</span><span class="sxs-lookup"><span data-stu-id="cc737-2986">output</span></span> | <span data-ttu-id="cc737-2987">Kimeneti fájl (beleértve) a nyomáscsökkentő.</span><span class="sxs-lookup"><span data-stu-id="cc737-2987">Output file (including location) for the reducer.</span></span> <span data-ttu-id="cc737-2988">Az adatfolyam-Hadoop-feladat eredményének írása ehhez a tulajdonsághoz megadott helyre.</span><span class="sxs-lookup"><span data-stu-id="cc737-2988">The output of the Hadoop Streaming job is written to the location specified for this property.</span></span> |
| <span data-ttu-id="cc737-2989">filePaths</span><span class="sxs-lookup"><span data-stu-id="cc737-2989">filePaths</span></span> | <span data-ttu-id="cc737-2990">Elérési utak a hozzárendelést és nyomáscsökkentő végrehajtható fájlok számára.</span><span class="sxs-lookup"><span data-stu-id="cc737-2990">Paths for the mapper and reducer executables.</span></span> <span data-ttu-id="cc737-2991">A példában: "adfsample/example/apps/wc.exe" adfsample a blob-tároló, például/alkalmazások az a mappa, pedig wc.exe a végrehajtható fájl.</span><span class="sxs-lookup"><span data-stu-id="cc737-2991">In the example: "adfsample/example/apps/wc.exe", adfsample is the blob container, example/apps is the folder, and wc.exe is the executable.</span></span> | 
| <span data-ttu-id="cc737-2992">fileLinkedService</span><span class="sxs-lookup"><span data-stu-id="cc737-2992">fileLinkedService</span></span> | <span data-ttu-id="cc737-2993">Az Azure tárolás társított szolgáltatása, amely az Azure storage filePaths szakaszában megadott fájlt tartalmazó jelöli.</span><span class="sxs-lookup"><span data-stu-id="cc737-2993">Azure Storage linked service that represents the Azure storage that contains the files specified in the filePaths section.</span></span> | 
| <span data-ttu-id="cc737-2994">Argumentumok</span><span class="sxs-lookup"><span data-stu-id="cc737-2994">arguments</span></span> | <span data-ttu-id="cc737-2995">A MapReduce program argumentumai vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="cc737-2995">A list of comma-separated arguments for the MapReduce program.</span></span> <span data-ttu-id="cc737-2996">Futásidőben, néhány további argumentumok látja (például: mapreduce.job.tags) a a MapReduce keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="cc737-2996">At runtime, you see a few extra arguments (for example: mapreduce.job.tags) from the MapReduce framework.</span></span> <span data-ttu-id="cc737-2997">A MapReduce argumentumok argumentumok megkülönböztetéséhez érdemes beállítás és value elemeit is argumentumként a következő példában látható módon (- s,--bemeneti,--kimeneti stb., amelyet közvetlenül az értékeik lehetőség áll)</span><span class="sxs-lookup"><span data-stu-id="cc737-2997">To differentiate your arguments with the MapReduce arguments, consider using both option and value as arguments as shown in the following example (-s, --input, --output etc., are options immediately followed by their values)</span></span> | 
| <span data-ttu-id="cc737-2998">getDebugInfo</span><span class="sxs-lookup"><span data-stu-id="cc737-2998">getDebugInfo</span></span> | <span data-ttu-id="cc737-2999">Választható eleme.</span><span class="sxs-lookup"><span data-stu-id="cc737-2999">An optional element.</span></span> <span data-ttu-id="cc737-3000">Hiba-ra van állítva, a naplók csak az hiba lesznek letöltve.</span><span class="sxs-lookup"><span data-stu-id="cc737-3000">When it is set to Failure, the logs are downloaded only on failure.</span></span> <span data-ttu-id="cc737-3001">Ha ezt a beállítást az összes, a naplók mindig letöltődnek függetlenül a végrehajtási állapotot.</span><span class="sxs-lookup"><span data-stu-id="cc737-3001">When it is set to All, logs are always downloaded irrespective of the execution status.</span></span> | 

> [!NOTE]
> <span data-ttu-id="cc737-3002">A Hadoop Streamelési tevékenységben egy kimeneti adatkészletet kell megadni a **kimenete** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="cc737-3002">You must specify an output dataset for the Hadoop Streaming Activity for the **outputs** property.</span></span> <span data-ttu-id="cc737-3003">Ez az adatkészlet csak egy üres adatkészlet szükséges ahhoz, hogy az adatcsatorna ütemezés (óránként, naponta, stb.) a meghajtó is lehet.</span><span class="sxs-lookup"><span data-stu-id="cc737-3003">This dataset can be just a dummy dataset that is required to drive the pipeline schedule (hourly, daily, etc.).</span></span> <span data-ttu-id="cc737-3004">A tevékenység bemeneti nem veszi, kihagyhatja, ha egy bemeneti adatkészlet a tevékenység megadása a **bemenetek** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="cc737-3004">If the activity doesn't take an input, you can skip specifying an input dataset for the activity for the **inputs** property.</span></span>  

## <a name="json-example"></a><span data-ttu-id="cc737-3005">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="cc737-3005">JSON example</span></span>

```json
{
    "name": "HadoopStreamingPipeline",
    "properties": {
        "description": "Hadoop Streaming Demo",
        "activities": [
            {
                "type": "HDInsightStreaming",
                "typeProperties": {
                    "mapper": "cat.exe",
                    "reducer": "wc.exe",
                    "input": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                    "output": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                    "filePaths": ["<nameofthecluster>/example/apps/wc.exe","<nameofthecluster>/example/apps/cat.exe"],
                    "fileLinkedService": "StorageLinkedService",
                    "getDebugInfo": "Failure"
                },
                "outputs": [
                    {
                        "name": "StreamingOutputDataset"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "RunHadoopStreamingJob",
                "description": "Run a Hadoop streaming job",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2014-01-04T00:00:00",
        "end": "2014-01-05T00:00:00"
    }
}
```

<span data-ttu-id="cc737-3006">További információkért lásd: [Hadoop Streamelési tevékenységben](data-factory-hadoop-streaming-activity.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-3006">For more information, see [Hadoop Streaming Activity](data-factory-hadoop-streaming-activity.md) article.</span></span> 

## <a name="hdinsight-spark-activity"></a><span data-ttu-id="cc737-3007">HDInsight Spark-tevékenység</span><span class="sxs-lookup"><span data-stu-id="cc737-3007">HDInsight Spark Activity</span></span>
<span data-ttu-id="cc737-3008">A következő tulajdonságok is megadhatók a Spark tevékenység JSON-definícióban.</span><span class="sxs-lookup"><span data-stu-id="cc737-3008">You can specify the following properties in a Spark Activity JSON definition.</span></span> <span data-ttu-id="cc737-3009">A type tulajdonságot a tevékenységnek kell lennie: **HDInsightSpark**.</span><span class="sxs-lookup"><span data-stu-id="cc737-3009">The type property for the activity must be: **HDInsightSpark**.</span></span> <span data-ttu-id="cc737-3010">Létre kell hoznia egy HDInsight kapcsolódó szolgáltatás először és értékeként adja meg a nevét, a **linkedServiceName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="cc737-3010">You must create a HDInsight linked service first and specify the name of it as a value for the **linkedServiceName** property.</span></span> <span data-ttu-id="cc737-3011">A következő tulajdonságok támogatottak a **typeProperties** szakasz tevékenység HDInsightSpark beállításakor:</span><span class="sxs-lookup"><span data-stu-id="cc737-3011">The following properties are supported in the **typeProperties** section when you set the type of activity to HDInsightSpark:</span></span> 

| <span data-ttu-id="cc737-3012">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-3012">Property</span></span> | <span data-ttu-id="cc737-3013">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-3013">Description</span></span> | <span data-ttu-id="cc737-3014">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-3014">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="cc737-3015">rootPath</span><span class="sxs-lookup"><span data-stu-id="cc737-3015">rootPath</span></span> | <span data-ttu-id="cc737-3016">Az Azure Blob-tároló és a Spark-fájlt tartalmazó mappát.</span><span class="sxs-lookup"><span data-stu-id="cc737-3016">The Azure Blob container and folder that contains the Spark file.</span></span> <span data-ttu-id="cc737-3017">A fájlnév pedig kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="cc737-3017">The file name is case-sensitive.</span></span> | <span data-ttu-id="cc737-3018">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-3018">Yes</span></span> |
| <span data-ttu-id="cc737-3019">entryFilePath</span><span class="sxs-lookup"><span data-stu-id="cc737-3019">entryFilePath</span></span> | <span data-ttu-id="cc737-3020">A gyökérmappában található azon a Spark kódcsomag relatív elérési útja.</span><span class="sxs-lookup"><span data-stu-id="cc737-3020">Relative path to the root folder of the Spark code/package.</span></span> | <span data-ttu-id="cc737-3021">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-3021">Yes</span></span> |
| <span data-ttu-id="cc737-3022">Osztálynév</span><span class="sxs-lookup"><span data-stu-id="cc737-3022">className</span></span> | <span data-ttu-id="cc737-3023">Az alkalmazás Java/Spark fő osztály</span><span class="sxs-lookup"><span data-stu-id="cc737-3023">Application's Java/Spark main class</span></span> | <span data-ttu-id="cc737-3024">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-3024">No</span></span> | 
| <span data-ttu-id="cc737-3025">Argumentumok</span><span class="sxs-lookup"><span data-stu-id="cc737-3025">arguments</span></span> | <span data-ttu-id="cc737-3026">A Spark program parancssori argumentumokat listáját.</span><span class="sxs-lookup"><span data-stu-id="cc737-3026">A list of command-line arguments to the Spark program.</span></span> | <span data-ttu-id="cc737-3027">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-3027">No</span></span> | 
| <span data-ttu-id="cc737-3028">proxyUser</span><span class="sxs-lookup"><span data-stu-id="cc737-3028">proxyUser</span></span> | <span data-ttu-id="cc737-3029">A Spark program végrehajtásának megszemélyesíteni a felhasználói fiók</span><span class="sxs-lookup"><span data-stu-id="cc737-3029">The user account to impersonate to execute the Spark program</span></span> | <span data-ttu-id="cc737-3030">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-3030">No</span></span> | 
| <span data-ttu-id="cc737-3031">sparkConfig</span><span class="sxs-lookup"><span data-stu-id="cc737-3031">sparkConfig</span></span> | <span data-ttu-id="cc737-3032">Spark konfigurációs tulajdonságaiban.</span><span class="sxs-lookup"><span data-stu-id="cc737-3032">Spark configuration properties.</span></span> | <span data-ttu-id="cc737-3033">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-3033">No</span></span> | 
| <span data-ttu-id="cc737-3034">getDebugInfo</span><span class="sxs-lookup"><span data-stu-id="cc737-3034">getDebugInfo</span></span> | <span data-ttu-id="cc737-3035">Itt adhatja meg, amikor a Spark naplófájlok kerülnek a HDInsight-fürt által használt Azure storage (vagy) leírt módon sparkJobLinkedService.</span><span class="sxs-lookup"><span data-stu-id="cc737-3035">Specifies when the Spark log files are copied to the Azure storage used by HDInsight cluster (or) specified by sparkJobLinkedService.</span></span> <span data-ttu-id="cc737-3036">Megengedett értékek: None, mindig, vagy sikertelen.</span><span class="sxs-lookup"><span data-stu-id="cc737-3036">Allowed values: None, Always, or Failure.</span></span> <span data-ttu-id="cc737-3037">Alapértelmezett érték: nincs.</span><span class="sxs-lookup"><span data-stu-id="cc737-3037">Default value: None.</span></span> | <span data-ttu-id="cc737-3038">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-3038">No</span></span> | 
| <span data-ttu-id="cc737-3039">sparkJobLinkedService</span><span class="sxs-lookup"><span data-stu-id="cc737-3039">sparkJobLinkedService</span></span> | <span data-ttu-id="cc737-3040">Az Azure Storage társított szolgáltatás, amely tárolja a Spark feladat fájl, a függőségeket és a naplókat.</span><span class="sxs-lookup"><span data-stu-id="cc737-3040">The Azure Storage linked service that holds the Spark job file, dependencies, and logs.</span></span>  <span data-ttu-id="cc737-3041">Ha nem ad meg egy értéket ehhez a tulajdonsághoz, a HDInsight-fürthöz társított tárolót használja a rendszer.</span><span class="sxs-lookup"><span data-stu-id="cc737-3041">If you do not specify a value for this property, the storage associated with HDInsight cluster is used.</span></span> | <span data-ttu-id="cc737-3042">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-3042">No</span></span> |

### <a name="json-example"></a><span data-ttu-id="cc737-3043">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="cc737-3043">JSON example</span></span>

```json
{
    "name": "SparkPipeline",
    "properties": {
        "activities": [
            {
                "type": "HDInsightSpark",
                "typeProperties": {
                    "rootPath": "adfspark\\pyFiles",
                    "entryFilePath": "test.py",
                    "getDebugInfo": "Always"
                },
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ],
                "name": "MySparkActivity",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-02-05T00:00:00",
        "end": "2017-02-06T00:00:00"
    }
}
```
<span data-ttu-id="cc737-3044">Vegye figyelembe a következő szempontokat:</span><span class="sxs-lookup"><span data-stu-id="cc737-3044">Note the following points:</span></span> 

- <span data-ttu-id="cc737-3045">A **típus** tulajdonsága **HDInsightSpark**.</span><span class="sxs-lookup"><span data-stu-id="cc737-3045">The **type** property is set to **HDInsightSpark**.</span></span>
- <span data-ttu-id="cc737-3046">A **rootPath** értéke **adfspark\\pyFiles** ahol adfspark az Azure Blob-tárolóba, pyFiles pedig az adott tároló finom mappát.</span><span class="sxs-lookup"><span data-stu-id="cc737-3046">The **rootPath** is set to **adfspark\\pyFiles** where adfspark is the Azure Blob container and pyFiles is fine folder in that container.</span></span> <span data-ttu-id="cc737-3047">Ebben a példában az Azure Blob Storage lesz a Spark-fürt társított.</span><span class="sxs-lookup"><span data-stu-id="cc737-3047">In this example, the Azure Blob Storage is the one that is associated with the Spark cluster.</span></span> <span data-ttu-id="cc737-3048">Feltöltheti a fájlt egy másik Azure-tárhelyre.</span><span class="sxs-lookup"><span data-stu-id="cc737-3048">You can upload the file to a different Azure Storage.</span></span> <span data-ttu-id="cc737-3049">Ha így tesz, hozzon létre egy Azure tárolás társított szolgáltatása, hogy a storage-fiók összekapcsolása az adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="cc737-3049">If you do so, create an Azure Storage linked service to link that storage account to the data factory.</span></span> <span data-ttu-id="cc737-3050">Ezt követően adja meg a társított szolgáltatás neve értékeként a **sparkJobLinkedService** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="cc737-3050">Then, specify the name of the linked service as a value for the **sparkJobLinkedService** property.</span></span> <span data-ttu-id="cc737-3051">Lásd: [Spark-tevékenység tulajdonságai](#spark-activity-properties) Ez a tulajdonság és az egyéb Spark tevékenység által támogatott tulajdonságokról.</span><span class="sxs-lookup"><span data-stu-id="cc737-3051">See [Spark Activity properties](#spark-activity-properties) for details about this property and other properties supported by the Spark Activity.</span></span>
- <span data-ttu-id="cc737-3052">A **entryFilePath** értékre van állítva a **test.py**, vagyis a python-fájl.</span><span class="sxs-lookup"><span data-stu-id="cc737-3052">The **entryFilePath** is set to the **test.py**, which is the python file.</span></span> 
- <span data-ttu-id="cc737-3053">A **getDebugInfo** tulajdonsága **mindig**, ami azt jelenti, a naplófájlok helyét a rendszer mindig generált (sikeres vagy sikertelen).</span><span class="sxs-lookup"><span data-stu-id="cc737-3053">The **getDebugInfo** property is set to **Always**, which means the log files are always generated (success or failure).</span></span>  

    > [!IMPORTANT]
    > <span data-ttu-id="cc737-3054">Azt javasoljuk, hogy nincs megadva ehhez a tulajdonsághoz mindig az éles környezetben kivéve, ha a probléma hibaelhárítást.</span><span class="sxs-lookup"><span data-stu-id="cc737-3054">We recommend that you do not set this property to Always in a production environment unless you are troubleshooting an issue.</span></span> 
- <span data-ttu-id="cc737-3055">A **kimenete** szakasz tartalmaz egy kimeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="cc737-3055">The **outputs** section has one output dataset.</span></span> <span data-ttu-id="cc737-3056">Meg kell adnia egy kimeneti adatkészletet, még akkor is, ha a spark program nem ad kimenetet.</span><span class="sxs-lookup"><span data-stu-id="cc737-3056">You must specify an output dataset even if the spark program does not produce any output.</span></span> <span data-ttu-id="cc737-3057">A kimeneti adatkészlet meghajtók az ütemezés a következő feldolgozási sor (óránként, naponta, stb.).</span><span class="sxs-lookup"><span data-stu-id="cc737-3057">The output dataset drives the schedule for the pipeline (hourly, daily, etc.).</span></span>

<span data-ttu-id="cc737-3058">A tevékenység kapcsolatos további információkért lásd: [Spark tevékenység](data-factory-spark.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-3058">For more information about the activity, see [Spark Activity](data-factory-spark.md) article.</span></span>  

## <a name="machine-learning-batch-execution-activity"></a><span data-ttu-id="cc737-3059">Machine Learning kötegelt végrehajtási tevékenység</span><span class="sxs-lookup"><span data-stu-id="cc737-3059">Machine Learning Batch Execution Activity</span></span>
<span data-ttu-id="cc737-3060">A következő tulajdonságok is megadhatók az Azure ML kötegelt végrehajtási tevékenység JSON-definícióban.</span><span class="sxs-lookup"><span data-stu-id="cc737-3060">You can specify the following properties in a Azure ML Batch Execution Activity JSON definition.</span></span> <span data-ttu-id="cc737-3061">A type tulajdonságot a tevékenységnek kell lennie: **AzureMLBatchExecution**.</span><span class="sxs-lookup"><span data-stu-id="cc737-3061">The type property for the activity must be: **AzureMLBatchExecution**.</span></span> <span data-ttu-id="cc737-3062">Létre kell hoznia az Azure Machine Learning-szolgáltatásnak először és értékeként adja meg a nevét, a **linkedServiceName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="cc737-3062">You must create a Azure Machine Learning linked service first and specify the name of it as a value for the **linkedServiceName** property.</span></span> <span data-ttu-id="cc737-3063">A következő tulajdonságok támogatottak a **typeProperties** szakasz tevékenység AzureMLBatchExecution beállításakor:</span><span class="sxs-lookup"><span data-stu-id="cc737-3063">The following properties are supported in the **typeProperties** section when you set the type of activity to AzureMLBatchExecution:</span></span>

<span data-ttu-id="cc737-3064">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-3064">Property</span></span> | <span data-ttu-id="cc737-3065">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-3065">Description</span></span> | <span data-ttu-id="cc737-3066">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-3066">Required</span></span> 
-------- | ----------- | --------
<span data-ttu-id="cc737-3067">Típus</span><span class="sxs-lookup"><span data-stu-id="cc737-3067">webServiceInput</span></span> | <span data-ttu-id="cc737-3068">Az adatkészlet átadását az Azure gépi tanulás webszolgáltatás bemeneteként.</span><span class="sxs-lookup"><span data-stu-id="cc737-3068">The dataset to be passed as an input for the Azure ML web service.</span></span> <span data-ttu-id="cc737-3069">Ez az adatkészlet a tevékenység bemenetei is kell szerepelnie.</span><span class="sxs-lookup"><span data-stu-id="cc737-3069">This dataset must also be included in the inputs for the activity.</span></span> |<span data-ttu-id="cc737-3070">Használja a típus vagy webServiceInputs.</span><span class="sxs-lookup"><span data-stu-id="cc737-3070">Use either webServiceInput or webServiceInputs.</span></span> | 
<span data-ttu-id="cc737-3071">webServiceInputs</span><span class="sxs-lookup"><span data-stu-id="cc737-3071">webServiceInputs</span></span> | <span data-ttu-id="cc737-3072">Adja meg az Azure gépi tanulás webszolgáltatás bemeneteként átadni adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="cc737-3072">Specify datasets to be passed as inputs for the Azure ML web service.</span></span> <span data-ttu-id="cc737-3073">A webszolgáltatás több bemeneti adatokat fogad, ha a webServiceInputs tulajdonsággal típus tulajdonság használata helyett.</span><span class="sxs-lookup"><span data-stu-id="cc737-3073">If the web service takes multiple inputs, use the webServiceInputs property instead of using the webServiceInput property.</span></span> <span data-ttu-id="cc737-3074">Által hivatkozott adatkészletek a **webServiceInputs** is szerepelnie kell a tevékenység **bemenetek**.</span><span class="sxs-lookup"><span data-stu-id="cc737-3074">Datasets that are referenced by the **webServiceInputs** must also be included in the Activity **inputs**.</span></span> | <span data-ttu-id="cc737-3075">Használja a típus vagy webServiceInputs.</span><span class="sxs-lookup"><span data-stu-id="cc737-3075">Use either webServiceInput or webServiceInputs.</span></span> | 
<span data-ttu-id="cc737-3076">webServiceOutputs</span><span class="sxs-lookup"><span data-stu-id="cc737-3076">webServiceOutputs</span></span> | <span data-ttu-id="cc737-3077">Az Azure ML web service kimeneti rendelt adathalmazokat.</span><span class="sxs-lookup"><span data-stu-id="cc737-3077">The datasets that are assigned as outputs for the Azure ML web service.</span></span> <span data-ttu-id="cc737-3078">A webszolgáltatás kimeneti adatait jeleníti meg az ehhez az adatkészlethez.</span><span class="sxs-lookup"><span data-stu-id="cc737-3078">The web service returns output data in this dataset.</span></span> | <span data-ttu-id="cc737-3079">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-3079">Yes</span></span> | 
<span data-ttu-id="cc737-3080">globalParameters</span><span class="sxs-lookup"><span data-stu-id="cc737-3080">globalParameters</span></span> | <span data-ttu-id="cc737-3081">Ez a szakasz a webszolgáltatás-paramétereket értékeket megadni.</span><span class="sxs-lookup"><span data-stu-id="cc737-3081">Specify values for the web service parameters in this section.</span></span> | <span data-ttu-id="cc737-3082">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-3082">No</span></span> | 

### <a name="json-example"></a><span data-ttu-id="cc737-3083">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="cc737-3083">JSON example</span></span>
<span data-ttu-id="cc737-3084">Ebben a példában a tevékenység rendelkezik az adatkészlet **MLSqlInput** bemenetként és **MLSqlOutput** kimeneteként.</span><span class="sxs-lookup"><span data-stu-id="cc737-3084">In this example, the activity has the dataset **MLSqlInput** as input and **MLSqlOutput** as the output.</span></span> <span data-ttu-id="cc737-3085">A **MLSqlInput** átadása a webszolgáltatás által bemeneteként használja a **típus** JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="cc737-3085">The **MLSqlInput** is passed as an input to the web service by using the **webServiceInput** JSON property.</span></span> <span data-ttu-id="cc737-3086">A **MLSqlOutput** objektumnak átadott kimenetként a webszolgáltatás által használ a **webServiceOutputs** JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="cc737-3086">The **MLSqlOutput** is passed as an output to the Web service by using the **webServiceOutputs** JSON property.</span></span> 

```json
{
   "name": "MLWithSqlReaderSqlWriter",
   "properties": {
      "description": "Azure ML model with sql azure reader/writer",
      "activities": [{
         "name": "MLSqlReaderSqlWriterActivity",
         "type": "AzureMLBatchExecution",
         "description": "test",
         "inputs": [ { "name": "MLSqlInput" }],
         "outputs": [ { "name": "MLSqlOutput" } ],
         "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
         "typeProperties":
         {
            "webServiceInput": "MLSqlInput",
            "webServiceOutputs": {
               "output1": "MLSqlOutput"
            },
            "globalParameters": {
               "Database server name": "<myserver>.database.windows.net",
               "Database name": "<database>",
               "Server user account name": "<user name>",
               "Server user account password": "<password>"
            }              
         },
         "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
         }
      }],
      "start": "2016-02-13T00:00:00",
       "end": "2016-02-14T00:00:00"
   }
}
```

<span data-ttu-id="cc737-3087">A JSON példában a telepített Azure Machine Learning Web service használ egy olvasó és író modul adatokat az vagy egy Azure SQL-adatbázis írható/olvasható.</span><span class="sxs-lookup"><span data-stu-id="cc737-3087">In the JSON example, the deployed Azure Machine Learning Web service uses a reader and a writer module to read/write data from/to an Azure SQL Database.</span></span> <span data-ttu-id="cc737-3088">Ez a webszolgáltatás mutatja meg a következő négy paraméterek: adatbázis-kiszolgáló neve, a adatbázis neve, a kiszolgáló felhasználói fiók nevét és a kiszolgáló felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="cc737-3088">This Web service exposes the following four parameters:  Database server name, Database name, Server user account name, and Server user account password.</span></span>

> [!NOTE]
> <span data-ttu-id="cc737-3089">Csak be- és kimenetekkel a AzureMLBatchExecution tevékenység argumentumként átadhatók paraméterek a webszolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="cc737-3089">Only inputs and outputs of the AzureMLBatchExecution activity can be passed as parameters to the Web service.</span></span> <span data-ttu-id="cc737-3090">Például a fenti JSON-részlet MLSqlInput a AzureMLBatchExecution tevékenység, amelyet a rendszer továbbad a webszolgáltatás bemeneteként típus paraméteren keresztül bemenete.</span><span class="sxs-lookup"><span data-stu-id="cc737-3090">For example, in the above JSON snippet, MLSqlInput is an input to the AzureMLBatchExecution activity, which is passed as an input to the Web service via webServiceInput parameter.</span></span>

## <a name="machine-learning-update-resource-activity"></a><span data-ttu-id="cc737-3091">Machine Learning Update-erőforrástevékenység</span><span class="sxs-lookup"><span data-stu-id="cc737-3091">Machine Learning Update Resource Activity</span></span>
<span data-ttu-id="cc737-3092">A következő tulajdonságok is megadhatók az Azure ML Update erőforrás tevékenység JSON-definícióban.</span><span class="sxs-lookup"><span data-stu-id="cc737-3092">You can specify the following properties in a Azure ML Update Resource Activity JSON definition.</span></span> <span data-ttu-id="cc737-3093">A type tulajdonságot a tevékenységnek kell lennie: **AzureMLUpdateResource**.</span><span class="sxs-lookup"><span data-stu-id="cc737-3093">The type property for the activity must be: **AzureMLUpdateResource**.</span></span> <span data-ttu-id="cc737-3094">Létre kell hoznia az Azure Machine Learning-szolgáltatásnak először és értékeként adja meg a nevét, a **linkedServiceName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="cc737-3094">You must create a Azure Machine Learning linked service first and specify the name of it as a value for the **linkedServiceName** property.</span></span> <span data-ttu-id="cc737-3095">A következő tulajdonságok támogatottak a **typeProperties** szakasz AzureMLUpdateResource tevékenység beállításakor:</span><span class="sxs-lookup"><span data-stu-id="cc737-3095">The following properties are supported in the **typeProperties** section when you set the type of activity to AzureMLUpdateResource:</span></span>

<span data-ttu-id="cc737-3096">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-3096">Property</span></span> | <span data-ttu-id="cc737-3097">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-3097">Description</span></span> | <span data-ttu-id="cc737-3098">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-3098">Required</span></span> 
-------- | ----------- | --------
<span data-ttu-id="cc737-3099">trainedModelName</span><span class="sxs-lookup"><span data-stu-id="cc737-3099">trainedModelName</span></span> | <span data-ttu-id="cc737-3100">A retrained modell neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-3100">Name of the retrained model.</span></span> | <span data-ttu-id="cc737-3101">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-3101">Yes</span></span> |  
<span data-ttu-id="cc737-3102">trainedModelDatasetName</span><span class="sxs-lookup"><span data-stu-id="cc737-3102">trainedModelDatasetName</span></span> | <span data-ttu-id="cc737-3103">A DataSet adatkészlet mutat a megőrzési művelet által visszaadott iLearner-fájlt.</span><span class="sxs-lookup"><span data-stu-id="cc737-3103">Dataset pointing to the iLearner file returned by the retraining operation.</span></span> | <span data-ttu-id="cc737-3104">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-3104">Yes</span></span> | 

### <a name="json-example"></a><span data-ttu-id="cc737-3105">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="cc737-3105">JSON example</span></span>
<span data-ttu-id="cc737-3106">A folyamat két tevékenység rendelkezik: **AzureMLBatchExecution** és **AzureMLUpdateResource**.</span><span class="sxs-lookup"><span data-stu-id="cc737-3106">The pipeline has two activities: **AzureMLBatchExecution** and **AzureMLUpdateResource**.</span></span> <span data-ttu-id="cc737-3107">Az Azure ML kötegelt végrehajtási tevékenység lekéri a tanítási adatokat bemeneti adatként, és egy kimenetként iLearner-fájlt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="cc737-3107">The Azure ML Batch Execution activity takes the training data as input and produces an iLearner file as an output.</span></span> <span data-ttu-id="cc737-3108">A tevékenység hív meg, a képzési webszolgáltatás (a tanítási kísérletet webszolgáltatásként kitett) a bemeneti betanítási adatok, és megkapja a webservice ilearner-fájlt.</span><span class="sxs-lookup"><span data-stu-id="cc737-3108">The activity invokes the training web service (training experiment exposed as a web service) with the input training data and receives the ilearner file from the webservice.</span></span> <span data-ttu-id="cc737-3109">A placeholderBlob csak az Azure Data Factory szolgáltatásnak a feldolgozási sor futtatásához szükséges üres kimeneti adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="cc737-3109">The placeholderBlob is just a dummy output dataset that is required by the Azure Data Factory service to run the pipeline.</span></span>


```json
{
    "name": "pipeline",
    "properties": {
        "activities": [
            {
                "name": "retraining",
                "type": "AzureMLBatchExecution",
                "inputs": [
                    {
                        "name": "trainingData"
                    }
                ],
                "outputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "typeProperties": {
                    "webServiceInput": "trainingData",
                    "webServiceOutputs": {
                        "output1": "trainedModelBlob"
                    }              
                 },
                "linkedServiceName": "trainingEndpoint",
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            },
            {
                "type": "AzureMLUpdateResource",
                "typeProperties": {
                    "trainedModelName": "trained model",
                    "trainedModelDatasetName" :  "trainedModelBlob"
                },
                "inputs": [{ "name": "trainedModelBlob" }],
                "outputs": [{ "name": "placeholderBlob" }],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "AzureML Update Resource",
                "linkedServiceName": "updatableScoringEndpoint2"
            }
        ],
        "start": "2016-02-13T00:00:00",
        "end": "2016-02-14T00:00:00"
    }
}
```

## <a name="data-lake-analytics-u-sql-activity"></a><span data-ttu-id="cc737-3110">Data Lake Analytics U-SQL-tevékenység</span><span class="sxs-lookup"><span data-stu-id="cc737-3110">Data Lake Analytics U-SQL Activity</span></span>
<span data-ttu-id="cc737-3111">A következő tulajdonságok is megadhatók a U-SQL tevékenység JSON-definícióban.</span><span class="sxs-lookup"><span data-stu-id="cc737-3111">You can specify the following properties in a U-SQL Activity JSON definition.</span></span> <span data-ttu-id="cc737-3112">A type tulajdonságot a tevékenységnek kell lennie: **DataLakeAnalyticsU-SQL**.</span><span class="sxs-lookup"><span data-stu-id="cc737-3112">The type property for the activity must be: **DataLakeAnalyticsU-SQL**.</span></span> <span data-ttu-id="cc737-3113">Létre kell hoznia egy Azure Data Lake Analytics kapcsolódó szolgáltatás és értékeként adja meg a nevét, a **linkedServiceName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="cc737-3113">You must create an Azure Data Lake Analytics linked service and specify the name of it as a value for the **linkedServiceName** property.</span></span> <span data-ttu-id="cc737-3114">A következő tulajdonságok támogatottak a **typeProperties** szakasz tevékenység DataLakeAnalyticsU-SQL beállításakor:</span><span class="sxs-lookup"><span data-stu-id="cc737-3114">The following properties are supported in the **typeProperties** section when you set the type of activity to DataLakeAnalyticsU-SQL:</span></span> 

| <span data-ttu-id="cc737-3115">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-3115">Property</span></span> | <span data-ttu-id="cc737-3116">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-3116">Description</span></span> | <span data-ttu-id="cc737-3117">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-3117">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="cc737-3118">scriptPath</span><span class="sxs-lookup"><span data-stu-id="cc737-3118">scriptPath</span></span> |<span data-ttu-id="cc737-3119">A U-SQL parancsfájlt tartalmazó mappa elérési útja.</span><span class="sxs-lookup"><span data-stu-id="cc737-3119">Path to folder that contains the U-SQL script.</span></span> <span data-ttu-id="cc737-3120">A fájl neve nem kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="cc737-3120">Name of the file is case-sensitive.</span></span> |<span data-ttu-id="cc737-3121">Nem (Ha a parancsfájl használata)</span><span class="sxs-lookup"><span data-stu-id="cc737-3121">No (if you use script)</span></span> |
| <span data-ttu-id="cc737-3122">scriptLinkedService</span><span class="sxs-lookup"><span data-stu-id="cc737-3122">scriptLinkedService</span></span> |<span data-ttu-id="cc737-3123">Kapcsolódó szolgáltatás, amely a tárolóban, amely tartalmazza az adat-előállító parancsfájl</span><span class="sxs-lookup"><span data-stu-id="cc737-3123">Linked service that links the storage that contains the script to the data factory</span></span> |<span data-ttu-id="cc737-3124">Nem (Ha a parancsfájl használata)</span><span class="sxs-lookup"><span data-stu-id="cc737-3124">No (if you use script)</span></span> |
| <span data-ttu-id="cc737-3125">Parancsfájl</span><span class="sxs-lookup"><span data-stu-id="cc737-3125">script</span></span> |<span data-ttu-id="cc737-3126">Adja meg a beágyazott parancsfájlja scriptPath és a scriptLinkedService megadása helyett.</span><span class="sxs-lookup"><span data-stu-id="cc737-3126">Specify inline script instead of specifying scriptPath and scriptLinkedService.</span></span> <span data-ttu-id="cc737-3127">Például: "script": "Adatbázis létrehozása test".</span><span class="sxs-lookup"><span data-stu-id="cc737-3127">For example: "script": "CREATE DATABASE test".</span></span> |<span data-ttu-id="cc737-3128">Nem (Ha a ScriptPath tulajdonságot is és a scriptLinkedService használ)</span><span class="sxs-lookup"><span data-stu-id="cc737-3128">No (if you use scriptPath and scriptLinkedService)</span></span> |
| <span data-ttu-id="cc737-3129">degreeOfParallelism</span><span class="sxs-lookup"><span data-stu-id="cc737-3129">degreeOfParallelism</span></span> |<span data-ttu-id="cc737-3130">A feladat futtatásához egyidejűleg használt csomópontok maximális száma.</span><span class="sxs-lookup"><span data-stu-id="cc737-3130">The maximum number of nodes simultaneously used to run the job.</span></span> |<span data-ttu-id="cc737-3131">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-3131">No</span></span> |
| <span data-ttu-id="cc737-3132">Prioritás</span><span class="sxs-lookup"><span data-stu-id="cc737-3132">priority</span></span> |<span data-ttu-id="cc737-3133">Azt határozza meg, melyet futtatni kíván szereplő várólistáján szereplő feladatok közül melyeket.</span><span class="sxs-lookup"><span data-stu-id="cc737-3133">Determines which jobs out of all that are queued should be selected to run first.</span></span> <span data-ttu-id="cc737-3134">Az alacsonyabb a szám, annál magasabb a prioritás.</span><span class="sxs-lookup"><span data-stu-id="cc737-3134">The lower the number, the higher the priority.</span></span> |<span data-ttu-id="cc737-3135">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-3135">No</span></span> |
| <span data-ttu-id="cc737-3136">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="cc737-3136">parameters</span></span> |<span data-ttu-id="cc737-3137">A U-SQL parancsfájl paraméterek</span><span class="sxs-lookup"><span data-stu-id="cc737-3137">Parameters for the U-SQL script</span></span> |<span data-ttu-id="cc737-3138">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-3138">No</span></span> |

### <a name="json-example"></a><span data-ttu-id="cc737-3139">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="cc737-3139">JSON example</span></span>

```json
{
    "name": "ComputeEventsByRegionPipeline",
    "properties": {
        "description": "This pipeline computes events for en-gb locale and date less than Feb 19, 2012.",
        "activities": 
        [
            {
                "type": "DataLakeAnalyticsU-SQL",
                "typeProperties": {
                    "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                    "scriptLinkedService": "StorageLinkedService",
                    "degreeOfParallelism": 3,
                    "priority": 100,
                    "parameters": {
                        "in": "/datalake/input/SearchLog.tsv",
                        "out": "/datalake/output/Result.tsv"
                    }
                },
                "inputs": [
                    {
                        "name": "DataLakeTable"
                    }
                ],
                "outputs": 
                [
                    {
                        "name": "EventsByRegionTable"
                    }
                ],
                "policy": {
                    "timeout": "06:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "EventsByRegion",
                "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
            }
        ],
        "start": "2015-08-08T00:00:00",
        "end": "2015-08-08T01:00:00",
        "isPaused": false
    }
}
```

<span data-ttu-id="cc737-3140">További információkért lásd: [Data Lake Analytics U-SQL tevékenység](data-factory-usql-activity.md).</span><span class="sxs-lookup"><span data-stu-id="cc737-3140">For more information, see [Data Lake Analytics U-SQL Activity](data-factory-usql-activity.md).</span></span> 

## <a name="stored-procedure-activity"></a><span data-ttu-id="cc737-3141">Tárolt eljárási tevékenység</span><span class="sxs-lookup"><span data-stu-id="cc737-3141">Stored Procedure Activity</span></span>
<span data-ttu-id="cc737-3142">A következő tulajdonságok is megadhatók a tárolt eljárás tevékenység JSON-definícióban.</span><span class="sxs-lookup"><span data-stu-id="cc737-3142">You can specify the following properties in a Stored Procedure Activity JSON definition.</span></span> <span data-ttu-id="cc737-3143">A type tulajdonságot a tevékenységnek kell lennie: **SqlServerStoredProcedure**.</span><span class="sxs-lookup"><span data-stu-id="cc737-3143">The type property for the activity must be: **SqlServerStoredProcedure**.</span></span> <span data-ttu-id="cc737-3144">Létre kell hoznia egy, a következő összekapcsolt szolgáltatások, és adja meg a társított szolgáltatás neve értékének a **linkedServiceName** tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="cc737-3144">You must create an one of the following linked services and specify the name of the linked service as a value for the **linkedServiceName** property:</span></span>

- <span data-ttu-id="cc737-3145">SQL Server</span><span class="sxs-lookup"><span data-stu-id="cc737-3145">SQL Server</span></span> 
- <span data-ttu-id="cc737-3146">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="cc737-3146">Azure SQL Database</span></span>
- <span data-ttu-id="cc737-3147">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="cc737-3147">Azure SQL Data Warehouse</span></span>

<span data-ttu-id="cc737-3148">A következő tulajdonságok támogatottak a **typeProperties** szakasz tevékenység SqlServerStoredProcedure beállításakor:</span><span class="sxs-lookup"><span data-stu-id="cc737-3148">The following properties are supported in the **typeProperties** section when you set the type of activity to SqlServerStoredProcedure:</span></span>

| <span data-ttu-id="cc737-3149">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-3149">Property</span></span> | <span data-ttu-id="cc737-3150">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-3150">Description</span></span> | <span data-ttu-id="cc737-3151">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-3151">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc737-3152">storedProcedureName</span><span class="sxs-lookup"><span data-stu-id="cc737-3152">storedProcedureName</span></span> |<span data-ttu-id="cc737-3153">Adja meg a tárolt eljárás neve a Azure SQL database vagy az Azure SQL Data Warehouse, amely a társított szolgáltatás, amely a bemeneti tábla használja.</span><span class="sxs-lookup"><span data-stu-id="cc737-3153">Specify the name of the stored procedure in the Azure SQL database or Azure SQL Data Warehouse that is represented by the linked service that the output table uses.</span></span> |<span data-ttu-id="cc737-3154">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-3154">Yes</span></span> |
| <span data-ttu-id="cc737-3155">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="cc737-3155">storedProcedureParameters</span></span> |<span data-ttu-id="cc737-3156">Adja meg a tárolt eljárás paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="cc737-3156">Specify values for stored procedure parameters.</span></span> <span data-ttu-id="cc737-3157">Ha az egyik paraméter null értéket átadni van szüksége, használja a szintaxist: "param1": (összes kisbetű) NULL értékű.</span><span class="sxs-lookup"><span data-stu-id="cc737-3157">If you need to pass null for a parameter, use the syntax: "param1": null (all lower case).</span></span> <span data-ttu-id="cc737-3158">Tekintse meg az alábbi minta tájékozódhat az e tulajdonság használatával.</span><span class="sxs-lookup"><span data-stu-id="cc737-3158">See the following sample to learn about using this property.</span></span> |<span data-ttu-id="cc737-3159">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-3159">No</span></span> |

<span data-ttu-id="cc737-3160">Ha megad egy bemeneti adatkészlet, elérhetőnek kell lennie (a "Kész" állapotú) a tárolt eljárás tevékenység futtatásához.</span><span class="sxs-lookup"><span data-stu-id="cc737-3160">If you do specify an input dataset, it must be available (in ‘Ready’ status) for the stored procedure activity to run.</span></span> <span data-ttu-id="cc737-3161">A bemeneti adatkészletet a tárolt eljárás nem használható paraméterként.</span><span class="sxs-lookup"><span data-stu-id="cc737-3161">The input dataset cannot be consumed in the stored procedure as a parameter.</span></span> <span data-ttu-id="cc737-3162">Csak a tárolt eljárási tevékenység megkezdése előtt ellenőrizze, a függőség szolgál.</span><span class="sxs-lookup"><span data-stu-id="cc737-3162">It is only used to check the dependency before starting the stored procedure activity.</span></span> <span data-ttu-id="cc737-3163">Meg kell adnia egy tárolt eljárás tevékenység egy kimeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="cc737-3163">You must specify an output dataset for a stored procedure activity.</span></span> 

<span data-ttu-id="cc737-3164">Kimeneti adatkészlet határozza meg a **ütemezés** a tárolt eljárás tevékenység (óránként, heti, havi, stb.).</span><span class="sxs-lookup"><span data-stu-id="cc737-3164">Output dataset specifies the **schedule** for the stored procedure activity (hourly, weekly, monthly, etc.).</span></span> <span data-ttu-id="cc737-3165">A kimeneti adatkészlet kell használnia egy **társított szolgáltatás** , amely egy Azure SQL Database vagy az Azure SQL Data Warehouse vagy a tárolt eljárás futtatásához használni kívánt SQL Server-adatbázis hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="cc737-3165">The output dataset must use a **linked service** that refers to an Azure SQL Database or an Azure SQL Data Warehouse or a SQL Server Database in which you want the stored procedure to run.</span></span> <span data-ttu-id="cc737-3166">A kimeneti adatkészlet felelt meg a tárolt eljárás eredményét a későbbi feldolgozásra, amelyet egy másik tevékenység módon működhetnek ([tevékenységek láncolás](data-factory-scheduling-and-execution.md##multiple-activities-in-a-pipeline)) a feldolgozási.</span><span class="sxs-lookup"><span data-stu-id="cc737-3166">The output dataset can serve as a way to pass the result of the stored procedure for subsequent processing by another activity ([chaining activities](data-factory-scheduling-and-execution.md##multiple-activities-in-a-pipeline)) in the pipeline.</span></span> <span data-ttu-id="cc737-3167">Azonban adat-előállító nem automatikusan kiírhatja a kimenetet tárolt eljárás ehhez a DataSet adatkészlethez.</span><span class="sxs-lookup"><span data-stu-id="cc737-3167">However, Data Factory does not automatically write the output of a stored procedure to this dataset.</span></span> <span data-ttu-id="cc737-3168">A tárolt eljárás, amely egy SQL táblázat, amely a kimeneti adatkészlet mutat ír.</span><span class="sxs-lookup"><span data-stu-id="cc737-3168">It is the stored procedure that writes to a SQL table that the output dataset points to.</span></span> <span data-ttu-id="cc737-3169">Bizonyos esetekben a kimeneti adatkészlet lehet egy **dummy dataset**, amellyel csak a tárolt eljárási tevékenység fut ütemezése.</span><span class="sxs-lookup"><span data-stu-id="cc737-3169">In some cases, the output dataset can be a **dummy dataset**, which is used only to specify the schedule for running the stored procedure activity.</span></span>  

### <a name="json-example"></a><span data-ttu-id="cc737-3170">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="cc737-3170">JSON example</span></span>

```json
{
    "name": "SprocActivitySamplePipeline",
    "properties": {
        "activities": [
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_sample",
                    "storedProcedureParameters": {
                        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                    }
                },
                "outputs": [{ "name": "sprocsampleout" }],
                "name": "SprocActivitySample"
            }
        ],
         "start": "2016-08-02T00:00:00",
         "end": "2016-08-02T05:00:00",
        "isPaused": false
    }
}
```

<span data-ttu-id="cc737-3171">További információkért lásd: [tárolt eljárási tevékenység](data-factory-stored-proc-activity.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-3171">For more information, see [Stored Procedure Activity](data-factory-stored-proc-activity.md) article.</span></span> 

## <a name="net-custom-activity"></a><span data-ttu-id="cc737-3172">.NET egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="cc737-3172">.NET custom activity</span></span>
<span data-ttu-id="cc737-3173">A következő tulajdonságok is megadhatók a .NET egyéni tevékenység JSON-definícióból.</span><span class="sxs-lookup"><span data-stu-id="cc737-3173">You can specify the following properties in a .NET custom activity JSON definition.</span></span> <span data-ttu-id="cc737-3174">A type tulajdonságot a tevékenységnek kell lennie: **DotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="cc737-3174">The type property for the activity must be: **DotNetActivity**.</span></span> <span data-ttu-id="cc737-3175">Létre kell hoznia egy Azure HDInsight kapcsolódó szolgáltatás, vagy egy társított Azure Batch szolgáltatásra, és adja meg a társított szolgáltatás neve értékének a **linkedServiceName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="cc737-3175">You must create an Azure HDInsight linked service or an Azure Batch linked service, and specify the name of the linked service as a value for the **linkedServiceName** property.</span></span> <span data-ttu-id="cc737-3176">A következő tulajdonságok támogatottak a **typeProperties** szakasz tevékenység DotNetActivity beállításakor:</span><span class="sxs-lookup"><span data-stu-id="cc737-3176">The following properties are supported in the **typeProperties** section when you set the type of activity to DotNetActivity:</span></span>
 
| <span data-ttu-id="cc737-3177">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc737-3177">Property</span></span> | <span data-ttu-id="cc737-3178">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc737-3178">Description</span></span> | <span data-ttu-id="cc737-3179">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc737-3179">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="cc737-3180">AssemblyName</span><span class="sxs-lookup"><span data-stu-id="cc737-3180">AssemblyName</span></span> | <span data-ttu-id="cc737-3181">A szerelvény neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-3181">Name of the assembly.</span></span> <span data-ttu-id="cc737-3182">A példában van: **MyDotnetActivity.dll**.</span><span class="sxs-lookup"><span data-stu-id="cc737-3182">In the example, it is: **MyDotnetActivity.dll**.</span></span> | <span data-ttu-id="cc737-3183">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-3183">Yes</span></span> |
| <span data-ttu-id="cc737-3184">Belépési pont</span><span class="sxs-lookup"><span data-stu-id="cc737-3184">EntryPoint</span></span> |<span data-ttu-id="cc737-3185">Az IDotNetActivity illesztőfelületet megvalósító osztály neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-3185">Name of the class that implements the IDotNetActivity interface.</span></span> <span data-ttu-id="cc737-3186">A példában van: **MyDotNetActivityNS.MyDotNetActivity** ahol MyDotNetActivityNS az a névtér pedig MyDotNetActivity az osztály.</span><span class="sxs-lookup"><span data-stu-id="cc737-3186">In the example, it is: **MyDotNetActivityNS.MyDotNetActivity** where MyDotNetActivityNS is the namespace and MyDotNetActivity is the class.</span></span>  | <span data-ttu-id="cc737-3187">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-3187">Yes</span></span> | 
| <span data-ttu-id="cc737-3188">PackageLinkedService</span><span class="sxs-lookup"><span data-stu-id="cc737-3188">PackageLinkedService</span></span> | <span data-ttu-id="cc737-3189">Az Azure Storage társított szolgáltatás mutat, a blob-tároló, amely tartalmazza az egyéni tevékenység zip-fájl neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-3189">Name of the Azure Storage linked service that points to the blob storage that contains the custom activity zip file.</span></span> <span data-ttu-id="cc737-3190">A példában van: **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="cc737-3190">In the example, it is: **AzureStorageLinkedService**.</span></span>| <span data-ttu-id="cc737-3191">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-3191">Yes</span></span> |
| <span data-ttu-id="cc737-3192">PackageFile</span><span class="sxs-lookup"><span data-stu-id="cc737-3192">PackageFile</span></span> | <span data-ttu-id="cc737-3193">A zip-fájl neve.</span><span class="sxs-lookup"><span data-stu-id="cc737-3193">Name of the zip file.</span></span> <span data-ttu-id="cc737-3194">A példában van: **customactivitycontainer/MyDotNetActivity.zip**.</span><span class="sxs-lookup"><span data-stu-id="cc737-3194">In the example, it is: **customactivitycontainer/MyDotNetActivity.zip**.</span></span> | <span data-ttu-id="cc737-3195">Igen</span><span class="sxs-lookup"><span data-stu-id="cc737-3195">Yes</span></span> |
| <span data-ttu-id="cc737-3196">Az ExtendedProperties</span><span class="sxs-lookup"><span data-stu-id="cc737-3196">extendedProperties</span></span> | <span data-ttu-id="cc737-3197">A kiterjesztett tulajdonságok, amely meghatározza, és át a .NET-kódot.</span><span class="sxs-lookup"><span data-stu-id="cc737-3197">Extended properties that you can define and pass on to the .NET code.</span></span> <span data-ttu-id="cc737-3198">Ebben a példában a **SliceStart** változó értéke a SliceStart rendszerváltozó alapuló értéket.</span><span class="sxs-lookup"><span data-stu-id="cc737-3198">In this example, the **SliceStart** variable is set to a value based on the SliceStart system variable.</span></span> | <span data-ttu-id="cc737-3199">Nem</span><span class="sxs-lookup"><span data-stu-id="cc737-3199">No</span></span> | 

### <a name="json-example"></a><span data-ttu-id="cc737-3200">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="cc737-3200">JSON example</span></span>

```json
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
    "start": "2016-11-16T00:00:00",
    "end": "2016-11-16T05:00:00",
    "isPaused": false
  }
}
```

<span data-ttu-id="cc737-3201">Részletes információkért lásd: [egyéni tevékenységeket felhasználni a Data Factory](data-factory-use-custom-activities.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc737-3201">For detailed information, see [Use custom activities in Data Factory](data-factory-use-custom-activities.md) article.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="cc737-3202">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cc737-3202">Next Steps</span></span>
<span data-ttu-id="cc737-3203">Tekintse meg az alábbi oktatóanyagok:</span><span class="sxs-lookup"><span data-stu-id="cc737-3203">See the following tutorials:</span></span> 

- [<span data-ttu-id="cc737-3204">Oktatóanyag: hozzon létre egy folyamatot a másolási tevékenység</span><span class="sxs-lookup"><span data-stu-id="cc737-3204">Tutorial: create a pipeline with a copy activity</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [<span data-ttu-id="cc737-3205">Oktatóanyag: hozzon létre egy folyamatot egy hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="cc737-3205">Tutorial: create a pipeline with a hive activity</span></span>](data-factory-build-your-first-pipeline-using-editor.md)