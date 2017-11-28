---
title: "aaaAzure Data Factory - JSON-parancsfájl-kezelési referencia |} Microsoft Docs"
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
ms.openlocfilehash: 813fd752bb0ecb1b513d022b9f302325105dac31
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory---json-scripting-reference"></a><span data-ttu-id="b4da5-103">Az Azure Data Factory - JSON-Parancsprogramokról</span><span class="sxs-lookup"><span data-stu-id="b4da5-103">Azure Data Factory - JSON Scripting Reference</span></span>
<span data-ttu-id="b4da5-104">A cikkben JSON-sémákat és példák meghatározásához az Azure Data Factory entitások (adatcsatorna, tevékenység, adatkészlet és társított szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="b4da5-104">This article provides JSON schemas and examples for defining Azure Data Factory entities (pipeline, activity, dataset, and linked service).</span></span>  

## <a name="pipeline"></a><span data-ttu-id="b4da5-105">Folyamat</span><span class="sxs-lookup"><span data-stu-id="b4da5-105">Pipeline</span></span> 
<span data-ttu-id="b4da5-106">hello magas szintű struktúra folyamat meghatározása a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="b4da5-106">hello high-level structure for a pipeline definition is as follows:</span></span> 

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

<span data-ttu-id="b4da5-107">Következő táblázat hello adatcsatorna JSON-definícióból hello tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="b4da5-107">Following table describes hello properties within hello pipeline JSON definition:</span></span>

| <span data-ttu-id="b4da5-108">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-108">Property</span></span> | <span data-ttu-id="b4da5-109">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-109">Description</span></span> | <span data-ttu-id="b4da5-110">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-110">Required</span></span>
-------- | ----------- | --------
| <span data-ttu-id="b4da5-111">név</span><span class="sxs-lookup"><span data-stu-id="b4da5-111">name</span></span> | <span data-ttu-id="b4da5-112">Hello folyamat nevét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-112">Name of hello pipeline.</span></span> <span data-ttu-id="b4da5-113">Adjon meg egy nevet, amely jelöli, hogy a tevékenység hello hello műveletet, vagy csővezeték konfigurált toodo</span><span class="sxs-lookup"><span data-stu-id="b4da5-113">Specify a name that represents hello action that hello activity or pipeline is configured toodo</span></span><br/><ul><li><span data-ttu-id="b4da5-114">A karakterek maximális száma: 260</span><span class="sxs-lookup"><span data-stu-id="b4da5-114">Maximum number of characters: 260</span></span></li><li><span data-ttu-id="b4da5-115">Betűvel, számmal vagy aláhúzásjellel (_) kell kezdődnie</span><span class="sxs-lookup"><span data-stu-id="b4da5-115">Must start with a letter number, or an underscore (_)</span></span></li><li><span data-ttu-id="b4da5-116">Következő karakterek nem engedélyezettek: ".", "+","?", "/", "<",">", "*", "%", "&", ":","\\"</span><span class="sxs-lookup"><span data-stu-id="b4da5-116">Following characters are not allowed: “.”, “+”, “?”, “/”, “<”,”>”,”*”,”%”,”&”,”:”,”\\”</span></span></li></ul> |<span data-ttu-id="b4da5-117">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-117">Yes</span></span> |
| <span data-ttu-id="b4da5-118">leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-118">description</span></span> |<span data-ttu-id="b4da5-119">Milyen hello tevékenység vagy csővezeték leíró szöveg</span><span class="sxs-lookup"><span data-stu-id="b4da5-119">Text describing what hello activity or pipeline is used for</span></span> | <span data-ttu-id="b4da5-120">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-120">No</span></span> |
| <span data-ttu-id="b4da5-121">tevékenységek</span><span class="sxs-lookup"><span data-stu-id="b4da5-121">activities</span></span> | <span data-ttu-id="b4da5-122">A tevékenységek listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b4da5-122">Contains a list of activities.</span></span> | <span data-ttu-id="b4da5-123">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-123">Yes</span></span> |
| <span data-ttu-id="b4da5-124">start</span><span class="sxs-lookup"><span data-stu-id="b4da5-124">start</span></span> |<span data-ttu-id="b4da5-125">Kezdő dátum-idő hello adatcsatorna.</span><span class="sxs-lookup"><span data-stu-id="b4da5-125">Start date-time for hello pipeline.</span></span> <span data-ttu-id="b4da5-126">Meg kell [ISO formátum](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="b4da5-126">Must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="b4da5-127">Például: 2014-10-14T16:32:41.</span><span class="sxs-lookup"><span data-stu-id="b4da5-127">For example: 2014-10-14T16:32:41.</span></span> <br/><br/><span data-ttu-id="b4da5-128">Már lehetséges toospecify a helyi időt, például egy keleti TÉLI idő.</span><span class="sxs-lookup"><span data-stu-id="b4da5-128">It is possible toospecify a local time, for example an EST time.</span></span> <span data-ttu-id="b4da5-129">Példa: `2016-02-27T06:00:00**-05:00`, vagyis 6 AM becsült</span><span class="sxs-lookup"><span data-stu-id="b4da5-129">Here is an example: `2016-02-27T06:00:00**-05:00`, which is 6 AM EST.</span></span><br/><br/><span data-ttu-id="b4da5-130">hello kezdő és záró együtt adjon hello adatcsatorna aktív időszakát.</span><span class="sxs-lookup"><span data-stu-id="b4da5-130">hello start and end properties together specify active period for hello pipeline.</span></span> <span data-ttu-id="b4da5-131">Kimeneti szeletek csak előállítása és az aktív időszakban.</span><span class="sxs-lookup"><span data-stu-id="b4da5-131">Output slices are only produced with in this active period.</span></span> |<span data-ttu-id="b4da5-132">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-132">No</span></span><br/><br/><span data-ttu-id="b4da5-133">Ha hello end tulajdonság értékét adja meg, meg kell adnia hello start tulajdonság értéke.</span><span class="sxs-lookup"><span data-stu-id="b4da5-133">If you specify a value for hello end property, you must specify value for hello start property.</span></span><br/><br/><span data-ttu-id="b4da5-134">hello indításának és befejezésének idejét is lehet üres toocreate folyamat.</span><span class="sxs-lookup"><span data-stu-id="b4da5-134">hello start and end times can both be empty toocreate a pipeline.</span></span> <span data-ttu-id="b4da5-135">Mindkét értéket meg kell adni az aktív időszak hello csővezeték toorun tooset.</span><span class="sxs-lookup"><span data-stu-id="b4da5-135">You must specify both values tooset an active period for hello pipeline toorun.</span></span> <span data-ttu-id="b4da5-136">Ha nem adja meg a kezdési és befejezési időpontjai folyamat létrehozásakor beállíthatja azokat később hello Set-AzureRmDataFactoryPipelineActivePeriod parancsmag használatával.</span><span class="sxs-lookup"><span data-stu-id="b4da5-136">If you do not specify start and end times when creating a pipeline, you can set them using hello Set-AzureRmDataFactoryPipelineActivePeriod cmdlet later.</span></span> |
| <span data-ttu-id="b4da5-137">Vége</span><span class="sxs-lookup"><span data-stu-id="b4da5-137">end</span></span> |<span data-ttu-id="b4da5-138">Befejező dátum idejű hello adatcsatorna.</span><span class="sxs-lookup"><span data-stu-id="b4da5-138">End date-time for hello pipeline.</span></span> <span data-ttu-id="b4da5-139">Ha a megadott ISO-formátumban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b4da5-139">If specified must be in ISO format.</span></span> <span data-ttu-id="b4da5-140">Például: 2014-10-14T17:32:41</span><span class="sxs-lookup"><span data-stu-id="b4da5-140">For example: 2014-10-14T17:32:41</span></span> <br/><br/><span data-ttu-id="b4da5-141">Már lehetséges toospecify a helyi időt, például egy keleti TÉLI idő.</span><span class="sxs-lookup"><span data-stu-id="b4da5-141">It is possible toospecify a local time, for example an EST time.</span></span> <span data-ttu-id="b4da5-142">Példa: `2016-02-27T06:00:00**-05:00`, vagyis 6 AM becsült</span><span class="sxs-lookup"><span data-stu-id="b4da5-142">Here is an example: `2016-02-27T06:00:00**-05:00`, which is 6 AM EST.</span></span><br/><br/><span data-ttu-id="b4da5-143">toorun hello folyamat határozatlan ideig, adja meg 9999-09-09 hello hello end tulajdonság értékét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-143">toorun hello pipeline indefinitely, specify 9999-09-09 as hello value for hello end property.</span></span> |<span data-ttu-id="b4da5-144">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-144">No</span></span> <br/><br/><span data-ttu-id="b4da5-145">Ha hello start tulajdonság értékét adja meg, meg kell adnia hello end tulajdonság értéke.</span><span class="sxs-lookup"><span data-stu-id="b4da5-145">If you specify a value for hello start property, you must specify value for hello end property.</span></span><br/><br/><span data-ttu-id="b4da5-146">Lásd: a Megjegyzések a hello **start** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b4da5-146">See notes for hello **start** property.</span></span> |
| <span data-ttu-id="b4da5-147">isPaused</span><span class="sxs-lookup"><span data-stu-id="b4da5-147">isPaused</span></span> |<span data-ttu-id="b4da5-148">Ha a készlet tootrue hello folyamat nem futtatható.</span><span class="sxs-lookup"><span data-stu-id="b4da5-148">If set tootrue hello pipeline does not run.</span></span> <span data-ttu-id="b4da5-149">Alapértelmezett érték = false.</span><span class="sxs-lookup"><span data-stu-id="b4da5-149">Default value = false.</span></span> <span data-ttu-id="b4da5-150">Ez a tulajdonság tooenable használja, vagy tiltsa le.</span><span class="sxs-lookup"><span data-stu-id="b4da5-150">You can use this property tooenable or disable.</span></span> |<span data-ttu-id="b4da5-151">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-151">No</span></span> |
| <span data-ttu-id="b4da5-152">pipelineMode</span><span class="sxs-lookup"><span data-stu-id="b4da5-152">pipelineMode</span></span> |<span data-ttu-id="b4da5-153">hello ütemezési módszer hello adatcsatorna fut.</span><span class="sxs-lookup"><span data-stu-id="b4da5-153">hello method for scheduling runs for hello pipeline.</span></span> <span data-ttu-id="b4da5-154">Két érték engedélyezett: (alapértelmezett), ütemezett alkalommal.</span><span class="sxs-lookup"><span data-stu-id="b4da5-154">Allowed values are: scheduled (default), onetime.</span></span><br/><br/><span data-ttu-id="b4da5-155">"Ütemezett" azt jelzi, hogy hello adatcsatorna tooits aktív időszaka (kezdő és záró idő) alapján meghatározott időközönként.</span><span class="sxs-lookup"><span data-stu-id="b4da5-155">‘Scheduled’ indicates that hello pipeline runs at a specified time interval according tooits active period (start and end time).</span></span> <span data-ttu-id="b4da5-156">"Alkalommal" azt jelzi, hogy hello folyamat csak egyszer fut le.</span><span class="sxs-lookup"><span data-stu-id="b4da5-156">‘Onetime’ indicates that hello pipeline runs only once.</span></span> <span data-ttu-id="b4da5-157">Létrehozását követően alkalommal adatcsatornák nem lehet módosítani/frissített jelenleg.</span><span class="sxs-lookup"><span data-stu-id="b4da5-157">Onetime pipelines once created cannot be modified/updated currently.</span></span> <span data-ttu-id="b4da5-158">Lásd: [Onetime csővezeték](data-factory-create-pipelines.md#onetime-pipeline) alkalommal beállítás vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="b4da5-158">See [Onetime pipeline](data-factory-create-pipelines.md#onetime-pipeline) for details about onetime setting.</span></span> |<span data-ttu-id="b4da5-159">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-159">No</span></span> |
| <span data-ttu-id="b4da5-160">ExpirationTime</span><span class="sxs-lookup"><span data-stu-id="b4da5-160">expirationTime</span></span> |<span data-ttu-id="b4da5-161">Időtartam, mely hello a feldolgozási sorban lévő érvényes és kiépített maradjon létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="b4da5-161">Duration of time after creation for which hello pipeline is valid and should remain provisioned.</span></span> <span data-ttu-id="b4da5-162">Nincs minden aktív sikertelen volt, vagy fut, függőben lévő hello feldolgozási sor törlése esetén automatikusan egyszer eléri a hello lejárati ideje.</span><span class="sxs-lookup"><span data-stu-id="b4da5-162">If it does not have any active, failed, or pending runs, hello pipeline is deleted automatically once it reaches hello expiration time.</span></span> |<span data-ttu-id="b4da5-163">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-163">No</span></span> |


## <a name="activity"></a><span data-ttu-id="b4da5-164">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4da5-164">Activity</span></span> 
<span data-ttu-id="b4da5-165">Az adatcsatorna definícióját (tevékenységek elem) tevékenységet hello magas szintű struktúráját a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="b4da5-165">hello high-level structure for an activity within a pipeline definition (activities element) is as follows:</span></span>

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

<span data-ttu-id="b4da5-166">A következő táblázat azt mutatják be hello tevékenység JSON-definícióból hello tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="b4da5-166">Following table describe hello properties within hello activity JSON definition:</span></span>

| <span data-ttu-id="b4da5-167">Címke</span><span class="sxs-lookup"><span data-stu-id="b4da5-167">Tag</span></span> | <span data-ttu-id="b4da5-168">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-168">Description</span></span> | <span data-ttu-id="b4da5-169">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-169">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-170">név</span><span class="sxs-lookup"><span data-stu-id="b4da5-170">name</span></span> |<span data-ttu-id="b4da5-171">Hello tevékenység nevét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-171">Name of hello activity.</span></span> <span data-ttu-id="b4da5-172">Adja meg egy nevet, hogy hello tevékenység hello műveletet képviselő úgy toodo</span><span class="sxs-lookup"><span data-stu-id="b4da5-172">Specify a name that represents hello action that hello activity is configured toodo</span></span><br/><ul><li><span data-ttu-id="b4da5-173">A karakterek maximális száma: 260</span><span class="sxs-lookup"><span data-stu-id="b4da5-173">Maximum number of characters: 260</span></span></li><li><span data-ttu-id="b4da5-174">Betűvel, számmal vagy aláhúzásjellel (_) kell kezdődnie</span><span class="sxs-lookup"><span data-stu-id="b4da5-174">Must start with a letter number, or an underscore (_)</span></span></li><li><span data-ttu-id="b4da5-175">Következő karakterek nem engedélyezettek: ".", "+","?", "/", "<",">", "*", "%", "&", ":","\\"</span><span class="sxs-lookup"><span data-stu-id="b4da5-175">Following characters are not allowed: “.”, “+”, “?”, “/”, “<”,”>”,”*”,”%”,”&”,”:”,”\\”</span></span></li></ul> |<span data-ttu-id="b4da5-176">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-176">Yes</span></span> |
| <span data-ttu-id="b4da5-177">leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-177">description</span></span> |<span data-ttu-id="b4da5-178">Milyen hello tevékenységet leíró szöveg használható.</span><span class="sxs-lookup"><span data-stu-id="b4da5-178">Text describing what hello activity is used for.</span></span> |<span data-ttu-id="b4da5-179">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-179">Yes</span></span> |
| <span data-ttu-id="b4da5-180">type</span><span class="sxs-lookup"><span data-stu-id="b4da5-180">type</span></span> |<span data-ttu-id="b4da5-181">Hello tevékenység hello típusát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="b4da5-181">Specifies hello type of hello activity.</span></span> <span data-ttu-id="b4da5-182">Lásd: hello [ADATTÁROLÓKHOZ](#data-stores) és [adatok ÁTALAKÍTÁSA tevékenységek](#data-transformation-activities) szakaszok a tevékenységek különböző típusú.</span><span class="sxs-lookup"><span data-stu-id="b4da5-182">See hello [DATA STORES](#data-stores) and [DATA TRANSFORMATION ACTIVITIES](#data-transformation-activities) sections for different types of activities.</span></span> |<span data-ttu-id="b4da5-183">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-183">Yes</span></span> |
| <span data-ttu-id="b4da5-184">Bemenetek</span><span class="sxs-lookup"><span data-stu-id="b4da5-184">inputs</span></span> |<span data-ttu-id="b4da5-185">Hello tevékenység által felhasznált bemeneti táblák</span><span class="sxs-lookup"><span data-stu-id="b4da5-185">Input tables used by hello activity</span></span><br/><br/>`// one input table`<br/>`"inputs":  [ { "name": "inputtable1"  } ],`<br/><br/>`// two input tables` <br/>`"inputs":  [ { "name": "inputtable1"  }, { "name": "inputtable2"  } ],` |<span data-ttu-id="b4da5-186">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-186">Yes</span></span> |
| <span data-ttu-id="b4da5-187">kimenetek</span><span class="sxs-lookup"><span data-stu-id="b4da5-187">outputs</span></span> |<span data-ttu-id="b4da5-188">Hello tevékenység által használt kimeneti táblák.</span><span class="sxs-lookup"><span data-stu-id="b4da5-188">Output tables used by hello activity.</span></span><br/><br/>`// one output table`<br/>`"outputs":  [ { "name": “outputtable1” } ],`<br/><br/>`//two output tables`<br/>`"outputs":  [ { "name": “outputtable1” }, { "name": “outputtable2” }  ],` |<span data-ttu-id="b4da5-189">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-189">Yes</span></span> |
| <span data-ttu-id="b4da5-190">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="b4da5-190">linkedServiceName</span></span> |<span data-ttu-id="b4da5-191">Hello tevékenység által használt hello társított szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-191">Name of hello linked service used by hello activity.</span></span> <br/><br/><span data-ttu-id="b4da5-192">Egy tevékenység lehet szükség, hogy megadja a kapcsolódó hello szolgáltatást, amely a toohello szükséges számítási környezet.</span><span class="sxs-lookup"><span data-stu-id="b4da5-192">An activity may require that you specify hello linked service that links toohello required compute environment.</span></span> |<span data-ttu-id="b4da5-193">HDInsight tevékenységek, az Azure Machine Learning tevékenységek és a tárolt eljárási tevékenység igen.</span><span class="sxs-lookup"><span data-stu-id="b4da5-193">Yes for HDInsight activities, Azure Machine Learning activities, and Stored Procedure Activity.</span></span> <br/><br/><span data-ttu-id="b4da5-194">Minden egyéb esetében: nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-194">No for all others</span></span> |
| <span data-ttu-id="b4da5-195">typeProperties</span><span class="sxs-lookup"><span data-stu-id="b4da5-195">typeProperties</span></span> |<span data-ttu-id="b4da5-196">Hello typeProperties szakaszban tulajdonságok attól függnek, hogy hello tevékenység típusa.</span><span class="sxs-lookup"><span data-stu-id="b4da5-196">Properties in hello typeProperties section depend on type of hello activity.</span></span> |<span data-ttu-id="b4da5-197">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-197">No</span></span> |
| <span data-ttu-id="b4da5-198">szabályzat</span><span class="sxs-lookup"><span data-stu-id="b4da5-198">policy</span></span> |<span data-ttu-id="b4da5-199">Hello tevékenység hello futásidejű működését befolyásoló házirendek.</span><span class="sxs-lookup"><span data-stu-id="b4da5-199">Policies that affect hello run-time behavior of hello activity.</span></span> <span data-ttu-id="b4da5-200">Ha nincs megadva, az alapértelmezett házirendek használhatók.</span><span class="sxs-lookup"><span data-stu-id="b4da5-200">If it is not specified, default policies are used.</span></span> |<span data-ttu-id="b4da5-201">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-201">No</span></span> |
| <span data-ttu-id="b4da5-202">A Feladatütemező</span><span class="sxs-lookup"><span data-stu-id="b4da5-202">scheduler</span></span> |<span data-ttu-id="b4da5-203">"Feladatütemező" tulajdonság hello tevékenység ütemezés használt toodefine szükséges.</span><span class="sxs-lookup"><span data-stu-id="b4da5-203">“scheduler” property is used toodefine desired scheduling for hello activity.</span></span> <span data-ttu-id="b4da5-204">A altulajdonságok vannak hello megegyeznek a hello azokat a hello [availability tulajdonság DataSet adatkészletben](data-factory-create-datasets.md#dataset-availability).</span><span class="sxs-lookup"><span data-stu-id="b4da5-204">Its subproperties are hello same as hello ones in hello [availability property in a dataset](data-factory-create-datasets.md#dataset-availability).</span></span> |<span data-ttu-id="b4da5-205">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-205">No</span></span> |

### <a name="policies"></a><span data-ttu-id="b4da5-206">Házirendek</span><span class="sxs-lookup"><span data-stu-id="b4da5-206">Policies</span></span>
<span data-ttu-id="b4da5-207">A házirendek milyen hatással hello futtatási viselkedés tevékenység, kifejezetten egy tábla hello szelet feldolgozásakor.</span><span class="sxs-lookup"><span data-stu-id="b4da5-207">Policies affect hello run-time behavior of an activity, specifically when hello slice of a table is processed.</span></span> <span data-ttu-id="b4da5-208">a következő táblázat hello hello részletes adatokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="b4da5-208">hello following table provides hello details.</span></span>

| <span data-ttu-id="b4da5-209">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-209">Property</span></span> | <span data-ttu-id="b4da5-210">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-210">Permitted values</span></span> | <span data-ttu-id="b4da5-211">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="b4da5-211">Default Value</span></span> | <span data-ttu-id="b4da5-212">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-212">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-213">Egyidejűségi</span><span class="sxs-lookup"><span data-stu-id="b4da5-213">concurrency</span></span> |<span data-ttu-id="b4da5-214">Egész szám</span><span class="sxs-lookup"><span data-stu-id="b4da5-214">Integer</span></span> <br/><br/><span data-ttu-id="b4da5-215">A maximális érték: 10</span><span class="sxs-lookup"><span data-stu-id="b4da5-215">Max value: 10</span></span> |<span data-ttu-id="b4da5-216">1</span><span class="sxs-lookup"><span data-stu-id="b4da5-216">1</span></span> |<span data-ttu-id="b4da5-217">Egyidejű végrehajtások hello tevékenység száma.</span><span class="sxs-lookup"><span data-stu-id="b4da5-217">Number of concurrent executions of hello activity.</span></span><br/><br/><span data-ttu-id="b4da5-218">Meghatározza, hogy hello száma párhuzamos tevékenység végrehajtások, amely akkor fordulhat elő, a másik szeletek.</span><span class="sxs-lookup"><span data-stu-id="b4da5-218">It determines hello number of parallel activity executions that can happen on different slices.</span></span> <span data-ttu-id="b4da5-219">Például ha egy tevékenység toogo keresztül elérhető adatokat, nagyobb feldolgozási értéke számos felgyorsítja a hello adatok feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="b4da5-219">For example, if an activity needs toogo through a large set of available data, having a larger concurrency value speeds up hello data processing.</span></span> |
| <span data-ttu-id="b4da5-220">executionPriorityOrder</span><span class="sxs-lookup"><span data-stu-id="b4da5-220">executionPriorityOrder</span></span> |<span data-ttu-id="b4da5-221">NewestFirst</span><span class="sxs-lookup"><span data-stu-id="b4da5-221">NewestFirst</span></span><br/><br/><span data-ttu-id="b4da5-222">OldestFirst</span><span class="sxs-lookup"><span data-stu-id="b4da5-222">OldestFirst</span></span> |<span data-ttu-id="b4da5-223">OldestFirst</span><span class="sxs-lookup"><span data-stu-id="b4da5-223">OldestFirst</span></span> |<span data-ttu-id="b4da5-224">Meghatározza, hogy hello sorrendje feldolgozott adatszeletek.</span><span class="sxs-lookup"><span data-stu-id="b4da5-224">Determines hello ordering of data slices that are being processed.</span></span><br/><br/><span data-ttu-id="b4da5-225">Például ha 2 szeletek (du. 4: egy azonban és délután 5 óra egy másik tulajdonságnak), és mindkét végrehajtási függőben van.</span><span class="sxs-lookup"><span data-stu-id="b4da5-225">For example, if you have 2 slices (one happening at 4pm, and another one at 5pm), and both are pending execution.</span></span> <span data-ttu-id="b4da5-226">Ha hello executionPriorityOrder toobe NewestFirst, hello szelet, délután 5 óra lesz elsőként feldolgozva.</span><span class="sxs-lookup"><span data-stu-id="b4da5-226">If you set hello executionPriorityOrder toobe NewestFirst, hello slice at 5 PM is processed first.</span></span> <span data-ttu-id="b4da5-227">Hasonlóképpen ha hello executionPriorityORder toobe OldestFIrst, majd du. 4: hello szelet feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="b4da5-227">Similarly if you set hello executionPriorityORder toobe OldestFIrst, then hello slice at 4 PM is processed.</span></span> |
| <span data-ttu-id="b4da5-228">retry</span><span class="sxs-lookup"><span data-stu-id="b4da5-228">retry</span></span> |<span data-ttu-id="b4da5-229">Egész szám</span><span class="sxs-lookup"><span data-stu-id="b4da5-229">Integer</span></span><br/><br/><span data-ttu-id="b4da5-230">A maximális érték 10 is lehet.</span><span class="sxs-lookup"><span data-stu-id="b4da5-230">Max value can be 10</span></span> |<span data-ttu-id="b4da5-231">0</span><span class="sxs-lookup"><span data-stu-id="b4da5-231">0</span></span> |<span data-ttu-id="b4da5-232">Hello adatfeldolgozási hello adatszelethez előtt újrapróbálkozások száma hiba van megjelölve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-232">Number of retries before hello data processing for hello slice is marked as Failure.</span></span> <span data-ttu-id="b4da5-233">Egy adatszelet tevékenység végrehajtása a rendszer ismét megkísérli megadott toohello mentése újrapróbálkozások száma.</span><span class="sxs-lookup"><span data-stu-id="b4da5-233">Activity execution for a data slice is retried up toohello specified retry count.</span></span> <span data-ttu-id="b4da5-234">hello újrapróbálkozási minél hamarabb hello meghibásodás után történik.</span><span class="sxs-lookup"><span data-stu-id="b4da5-234">hello retry is done as soon as possible after hello failure.</span></span> |
| <span data-ttu-id="b4da5-235">timeout</span><span class="sxs-lookup"><span data-stu-id="b4da5-235">timeout</span></span> |<span data-ttu-id="b4da5-236">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="b4da5-236">TimeSpan</span></span> |<span data-ttu-id="b4da5-237">00:00:00</span><span class="sxs-lookup"><span data-stu-id="b4da5-237">00:00:00</span></span> |<span data-ttu-id="b4da5-238">Hello tevékenység időkorlátját.</span><span class="sxs-lookup"><span data-stu-id="b4da5-238">Timeout for hello activity.</span></span> <span data-ttu-id="b4da5-239">. Példa: 00:10:00 (azt jelenti, időtúllépés 10 perc)</span><span class="sxs-lookup"><span data-stu-id="b4da5-239">Example: 00:10:00 (implies timeout 10 mins)</span></span><br/><br/><span data-ttu-id="b4da5-240">Ha az érték nincs megadva vagy 0, hello időtúllépési érték végtelen.</span><span class="sxs-lookup"><span data-stu-id="b4da5-240">If a value is not specified or is 0, hello timeout is infinite.</span></span><br/><br/><span data-ttu-id="b4da5-241">Szelet hello adatok feldolgozási ideje meghaladja a hello időtúllépési értéket, ha azt megszakadt, és hello rendszer próbál tooretry hello feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="b4da5-241">If hello data processing time on a slice exceeds hello timeout value, it is canceled, and hello system attempts tooretry hello processing.</span></span> <span data-ttu-id="b4da5-242">Az újrapróbálkozások számát hello hello újrapróbálkozási tulajdonság függ.</span><span class="sxs-lookup"><span data-stu-id="b4da5-242">hello number of retries depends on hello retry property.</span></span> <span data-ttu-id="b4da5-243">Időtúllépés esetén hello beállítás tooTimedOut.</span><span class="sxs-lookup"><span data-stu-id="b4da5-243">When timeout occurs, hello status is set tooTimedOut.</span></span> |
| <span data-ttu-id="b4da5-244">Késleltetés</span><span class="sxs-lookup"><span data-stu-id="b4da5-244">delay</span></span> |<span data-ttu-id="b4da5-245">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="b4da5-245">TimeSpan</span></span> |<span data-ttu-id="b4da5-246">00:00:00</span><span class="sxs-lookup"><span data-stu-id="b4da5-246">00:00:00</span></span> |<span data-ttu-id="b4da5-247">Adja meg a hello késleltetés hello szelet elindítja az adatok feldolgozása előtt.</span><span class="sxs-lookup"><span data-stu-id="b4da5-247">Specify hello delay before data processing of hello slice starts.</span></span><br/><br/><span data-ttu-id="b4da5-248">egy adatszelet tevékenységet hello végrehajtása után hello késleltetés hello várt végrehajtási ideje elmúlt elindult.</span><span class="sxs-lookup"><span data-stu-id="b4da5-248">hello execution of activity for a data slice is started after hello Delay is past hello expected execution time.</span></span><br/><br/><span data-ttu-id="b4da5-249">. Példa: 00:10:00 (magában foglalja a késleltetést a 10 perc)</span><span class="sxs-lookup"><span data-stu-id="b4da5-249">Example: 00:10:00 (implies delay of 10 mins)</span></span> |
| <span data-ttu-id="b4da5-250">hosszú újrapróbálkozás</span><span class="sxs-lookup"><span data-stu-id="b4da5-250">longRetry</span></span> |<span data-ttu-id="b4da5-251">Egész szám</span><span class="sxs-lookup"><span data-stu-id="b4da5-251">Integer</span></span><br/><br/><span data-ttu-id="b4da5-252">A maximális érték: 10</span><span class="sxs-lookup"><span data-stu-id="b4da5-252">Max value: 10</span></span> |<span data-ttu-id="b4da5-253">1</span><span class="sxs-lookup"><span data-stu-id="b4da5-253">1</span></span> |<span data-ttu-id="b4da5-254">hosszú újrapróbálkozások számát hello tett kísérletet, mielőtt hello szelet végrehajtása sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="b4da5-254">hello number of long retry attempts before hello slice execution is failed.</span></span><br/><br/><span data-ttu-id="b4da5-255">hosszú újrapróbálkozás kísérletek által longRetryInterval távolságban helyezkednek el.</span><span class="sxs-lookup"><span data-stu-id="b4da5-255">longRetry attempts are spaced by longRetryInterval.</span></span> <span data-ttu-id="b4da5-256">Ha toospecify kell egy újrapróbálkozási kísérletek között eltelt idő, így hosszú újrapróbálkozás használja.</span><span class="sxs-lookup"><span data-stu-id="b4da5-256">So if you need toospecify a time between retry attempts, use longRetry.</span></span> <span data-ttu-id="b4da5-257">Ha mind az újra gombra, és a hosszú újrapróbálkozás meg van adva, minden hosszú újrapróbálkozás kísérlet tartalmazza az ismételt kísérletek számát, és hello kísérletek maximális száma újrapróbálkozási * hosszú újrapróbálkozás.</span><span class="sxs-lookup"><span data-stu-id="b4da5-257">If both Retry and longRetry are specified, each longRetry attempt includes Retry attempts and hello max number of attempts is Retry * longRetry.</span></span><br/><br/><span data-ttu-id="b4da5-258">Ha például tudunk hello hello tevékenység házirend beállításai a következő:</span><span class="sxs-lookup"><span data-stu-id="b4da5-258">For example, if we have hello following settings in hello activity policy:</span></span><br/><span data-ttu-id="b4da5-259">Próbálkozzon újra: 3</span><span class="sxs-lookup"><span data-stu-id="b4da5-259">Retry: 3</span></span><br/><span data-ttu-id="b4da5-260">hosszú újrapróbálkozás: 2. régiója</span><span class="sxs-lookup"><span data-stu-id="b4da5-260">longRetry: 2</span></span><br/><span data-ttu-id="b4da5-261">longRetryInterval: 01:00:00</span><span class="sxs-lookup"><span data-stu-id="b4da5-261">longRetryInterval: 01:00:00</span></span><br/><br/><span data-ttu-id="b4da5-262">Feltételezik, hogy csak egy szelet tooexecute (állapot vár) hello tevékenység végrehajtási minden olyan alkalommal sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="b4da5-262">Assume there is only one slice tooexecute (status is Waiting) and hello activity execution fails every time.</span></span> <span data-ttu-id="b4da5-263">Eredetileg nem lenne 3 egymást követő végrehajtási kísérletek.</span><span class="sxs-lookup"><span data-stu-id="b4da5-263">Initially there would be 3 consecutive execution attempts.</span></span> <span data-ttu-id="b4da5-264">Minden kísérlet után hello szelet állapota lenne az újra gombra.</span><span class="sxs-lookup"><span data-stu-id="b4da5-264">After each attempt, hello slice status would be Retry.</span></span> <span data-ttu-id="b4da5-265">Miután először 3 kísérletet: keresztül, hello szelet állapota hosszú újrapróbálkozás lesz.</span><span class="sxs-lookup"><span data-stu-id="b4da5-265">After first 3 attempts are over, hello slice status would be LongRetry.</span></span><br/><br/><span data-ttu-id="b4da5-266">Egy óra (Ez azt jelenti, hogy longRetryInteval tartozó érték) nem lenne a 3 egymást követő végrehajtási kísérletek egy másik készlet.</span><span class="sxs-lookup"><span data-stu-id="b4da5-266">After an hour (that is, longRetryInteval’s value), there would be another set of 3 consecutive execution attempts.</span></span> <span data-ttu-id="b4da5-267">Ezt követően hello szelet állapota akkor sikertelen, és nincs további újrapróbálkozások volna kísérli meg.</span><span class="sxs-lookup"><span data-stu-id="b4da5-267">After that, hello slice status would be Failed and no more retries would be attempted.</span></span> <span data-ttu-id="b4da5-268">Ezért a teljes 6 történt kísérlet.</span><span class="sxs-lookup"><span data-stu-id="b4da5-268">Hence overall 6 attempts were made.</span></span><br/><br/><span data-ttu-id="b4da5-269">Ha bármely végrehajtása sikeres, hello szelet állapota Kész és nincs további újrapróbálkozások próbált vannak.</span><span class="sxs-lookup"><span data-stu-id="b4da5-269">If any execution succeeds, hello slice status would be Ready and no more retries are attempted.</span></span><br/><br/><span data-ttu-id="b4da5-270">hosszú újrapróbálkozás függő adatok nem determinisztikus időpontokban érkeznek vagy hello teljes környezete alapján akkor következik be, mely az adatfeldolgozás flaky használható.</span><span class="sxs-lookup"><span data-stu-id="b4da5-270">longRetry may be used in situations where dependent data arrives at non-deterministic times or hello overall environment is flaky under which data processing occurs.</span></span> <span data-ttu-id="b4da5-271">Ezekben az esetekben nem újrapróbálkozások egymás után ez segíthet, és kimeneti így időt eredményez hello időköz után szükséges.</span><span class="sxs-lookup"><span data-stu-id="b4da5-271">In such cases, doing retries one after another may not help and doing so after an interval of time results in hello desired output.</span></span><br/><br/><span data-ttu-id="b4da5-272">Járjon el a Word: nincs beállítva hosszú újrapróbálkozás vagy longRetryInterval magas értékeit.</span><span class="sxs-lookup"><span data-stu-id="b4da5-272">Word of caution: do not set high values for longRetry or longRetryInterval.</span></span> <span data-ttu-id="b4da5-273">Általában a magasabb értékkel rendszeres problémákkal utalnak.</span><span class="sxs-lookup"><span data-stu-id="b4da5-273">Typically, higher values imply other systemic issues.</span></span> |
| <span data-ttu-id="b4da5-274">longRetryInterval</span><span class="sxs-lookup"><span data-stu-id="b4da5-274">longRetryInterval</span></span> |<span data-ttu-id="b4da5-275">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="b4da5-275">TimeSpan</span></span> |<span data-ttu-id="b4da5-276">00:00:00</span><span class="sxs-lookup"><span data-stu-id="b4da5-276">00:00:00</span></span> |<span data-ttu-id="b4da5-277">hello kísérletek hosszú újrapróbálkozási kísérletek között eltelt idő</span><span class="sxs-lookup"><span data-stu-id="b4da5-277">hello delay between long retry attempts</span></span> |

### <a name="typeproperties-section"></a><span data-ttu-id="b4da5-278">typeProperties szakasz</span><span class="sxs-lookup"><span data-stu-id="b4da5-278">typeProperties section</span></span>
<span data-ttu-id="b4da5-279">hello typeProperties szakaszban nem egyezik, minden egyes tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="b4da5-279">hello typeProperties section is different for each activity.</span></span> <span data-ttu-id="b4da5-280">Átalakítás tevékenység csak a hello típus tulajdonságokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b4da5-280">Transformation activities have just hello type properties.</span></span> <span data-ttu-id="b4da5-281">Lásd: [adatok ÁTALAKÍTÁSA tevékenységek](#data-transformation-activities) szakasz ebben a cikkben egy folyamat átalakítása tevékenységek meghatározó JSON-minták.</span><span class="sxs-lookup"><span data-stu-id="b4da5-281">See [DATA TRANSFORMATION ACTIVITIES](#data-transformation-activities) section in this article for JSON samples that define transformation activities in a pipeline.</span></span> 

<span data-ttu-id="b4da5-282">**Másolási tevékenység** rendelkezik-e két alszakaszokat hello typeProperties szakasz: **forrás** és **fogadó**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-282">**Copy activity** has two subsections in hello typeProperties section: **source** and **sink**.</span></span> <span data-ttu-id="b4da5-283">Lásd: [ADATTÁROLÓKHOZ](#data-stores) szakasz ebben a cikkben a JSON-minták, hogy hogyan toouse egy adatok tárolót, mint a forrás és fogadó vagy megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="b4da5-283">See [DATA STORES](#data-stores) section in this article for JSON samples that show how toouse a data store as a source and/or sink.</span></span> 

### <a name="sample-copy-pipeline"></a><span data-ttu-id="b4da5-284">Minta másolási folyamat</span><span class="sxs-lookup"><span data-stu-id="b4da5-284">Sample copy pipeline</span></span>
<span data-ttu-id="b4da5-285">A következő minta csővezeték hello, nincs típusú egy tevékenység **másolása** a hello **tevékenységek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="b4da5-285">In hello following sample pipeline, there is one activity of type **Copy** in hello **activities** section.</span></span> <span data-ttu-id="b4da5-286">Ez a példa hello [másolási tevékenység](data-factory-data-movement-activities.md) másol adatokat az Azure Blob storage tooan Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b4da5-286">In this sample, hello [Copy activity](data-factory-data-movement-activities.md) copies data from an Azure Blob storage tooan Azure SQL database.</span></span> 

```json
{
  "name": "CopyPipeline",
  "properties": {
    "description": "Copy data from a blob tooAzure SQL table",
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

<span data-ttu-id="b4da5-287">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="b4da5-287">Note hello following points:</span></span>

* <span data-ttu-id="b4da5-288">Hello tevékenységek szakaszban csak egy tevékenység nincs amelynek **típus** értéke túl**másolási**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-288">In hello activities section, there is only one activity whose **type** is set too**Copy**.</span></span>
* <span data-ttu-id="b4da5-289">Adjon meg a hello tevékenység értéke túl**InputDataset** és hello tevékenység túl van-e állítva a kimeneti**OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-289">Input for hello activity is set too**InputDataset** and output for hello activity is set too**OutputDataset**.</span></span>
* <span data-ttu-id="b4da5-290">A hello **typeProperties** szakaszban **BlobSource** hello forrás típusaként van megadva, és **SqlSink** hello a fogadó típusa van megadva.</span><span class="sxs-lookup"><span data-stu-id="b4da5-290">In hello **typeProperties** section, **BlobSource** is specified as hello source type and **SqlSink** is specified as hello sink type.</span></span>

<span data-ttu-id="b4da5-291">Lásd: [ADATTÁROLÓKHOZ](#data-stores) szakasz ebben a cikkben a JSON-minták, hogy hogyan toouse egy adatok tárolót, mint a forrás és fogadó vagy megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="b4da5-291">See [DATA STORES](#data-stores) section in this article for JSON samples that show how toouse a data store as a source and/or sink.</span></span>    

<span data-ttu-id="b4da5-292">Ez az adatcsatorna létrehozásának részletes útmutatást lásd: [oktatóanyag: adatok másolása a Blob Storage tooSQL adatbázis](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="b4da5-292">For a complete walkthrough of creating this pipeline, see [Tutorial: Copy data from Blob Storage tooSQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span> 

### <a name="sample-transformation-pipeline"></a><span data-ttu-id="b4da5-293">Minta átalakítási folyamat</span><span class="sxs-lookup"><span data-stu-id="b4da5-293">Sample transformation pipeline</span></span>
<span data-ttu-id="b4da5-294">A következő minta csővezeték hello, nincs típusú egy tevékenység **HDInsightHive** a hello **tevékenységek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="b4da5-294">In hello following sample pipeline, there is one activity of type **HDInsightHive** in hello **activities** section.</span></span> <span data-ttu-id="b4da5-295">Ez a példa hello [HDInsight Hive tevékenység](data-factory-hive-activity.md) átalakítja az adatokat a egy Azure Blob storage egy Azure HDInsight Hadoop-fürt Hive parancsfájl futtatásával.</span><span class="sxs-lookup"><span data-stu-id="b4da5-295">In this sample, hello [HDInsight Hive activity](data-factory-hive-activity.md) transforms data from an Azure Blob storage by running a Hive script file on an Azure HDInsight Hadoop cluster.</span></span> 

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

<span data-ttu-id="b4da5-296">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="b4da5-296">Note hello following points:</span></span> 

* <span data-ttu-id="b4da5-297">Hello tevékenységek szakaszban csak egy tevékenység nincs amelynek **típus** értéke túl**HDInsightHive**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-297">In hello activities section, there is only one activity whose **type** is set too**HDInsightHive**.</span></span>
* <span data-ttu-id="b4da5-298">hello Hive parancsfájl, **partitionweblogs.hql**, hello Azure storage-fiók tárolva van (hello scriptLinkedService nevű által megadott **AzureStorageLinkedService**), majd a  **parancsfájl** hello tároló mappa **adfgetstarted**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-298">hello Hive script file, **partitionweblogs.hql**, is stored in hello Azure storage account (specified by hello scriptLinkedService, called **AzureStorageLinkedService**), and in **script** folder in hello container **adfgetstarted**.</span></span>
* <span data-ttu-id="b4da5-299">Hello **meghatározása** szakaszban használt toospecify hello futtatási beállítások Hive értékként toohello hive parancsfájl átadott (például `${hiveconf:inputtable}`, `${hiveconf:partitionedtable}`).</span><span class="sxs-lookup"><span data-stu-id="b4da5-299">hello **defines** section is used toospecify hello runtime settings that are passed toohello hive script as Hive configuration values (e.g `${hiveconf:inputtable}`, `${hiveconf:partitionedtable}`).</span></span>

<span data-ttu-id="b4da5-300">Lásd: [adatok ÁTALAKÍTÁSA tevékenységek](#data-transformation-activities) szakasz ebben a cikkben egy folyamat átalakítása tevékenységek meghatározó JSON-minták.</span><span class="sxs-lookup"><span data-stu-id="b4da5-300">See [DATA TRANSFORMATION ACTIVITIES](#data-transformation-activities) section in this article for JSON samples that define transformation activities in a pipeline.</span></span>

<span data-ttu-id="b4da5-301">Ez az adatcsatorna létrehozásának részletes útmutatást lásd: [oktatóanyag: az első adatcsatorna tooprocess adatok Hadoop-fürt létrehozása](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="b4da5-301">For a complete walkthrough of creating this pipeline, see [Tutorial: Build your first pipeline tooprocess data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span> 

## <a name="linked-service"></a><span data-ttu-id="b4da5-302">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-302">Linked service</span></span>
<span data-ttu-id="b4da5-303">a társított szolgáltatás definíciójának hello magas szintű struktúrája a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="b4da5-303">hello high-level structure for a linked service definition is as follows:</span></span>

```json
{
    "name": "<name of hello linked service>",
    "properties": {
        "type": "<type of hello linked service>",
        "typeProperties": {
        }
    }
}
```

<span data-ttu-id="b4da5-304">A következő táblázat azt mutatják be hello tevékenység JSON-definícióból hello tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="b4da5-304">Following table describe hello properties within hello activity JSON definition:</span></span>

| <span data-ttu-id="b4da5-305">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-305">Property</span></span> | <span data-ttu-id="b4da5-306">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-306">Description</span></span> | <span data-ttu-id="b4da5-307">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-307">Required</span></span> |
| -------- | ----------- | -------- | 
| <span data-ttu-id="b4da5-308">név</span><span class="sxs-lookup"><span data-stu-id="b4da5-308">name</span></span> | <span data-ttu-id="b4da5-309">Hello társított szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-309">Name of hello linked service.</span></span> | <span data-ttu-id="b4da5-310">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-310">Yes</span></span> | 
| <span data-ttu-id="b4da5-311">Tulajdonságok - típus</span><span class="sxs-lookup"><span data-stu-id="b4da5-311">properties - type</span></span> | <span data-ttu-id="b4da5-312">Hello típusú társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b4da5-312">Type of hello linked service.</span></span> <span data-ttu-id="b4da5-313">Például: az Azure Storage, Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="b4da5-313">For example: Azure Storage, Azure SQL Database.</span></span> |
| <span data-ttu-id="b4da5-314">typeProperties</span><span class="sxs-lookup"><span data-stu-id="b4da5-314">typeProperties</span></span> | <span data-ttu-id="b4da5-315">hello typeProperties szakasz különböző minden adattároló vagy számítási környezet elemeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b4da5-315">hello typeProperties section has elements that are different for each data store or compute environment.</span></span> <span data-ttu-id="b4da5-316">Lásd: [adattárolókhoz](#datastores) szakasz az összes hello adattároló társított szolgáltatások és [számítási környezetek](#compute-environments) összes hello a számítási összekapcsolt szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-316">See [data stores](#datastores) section for all hello data store linked services and [compute environments](#compute-environments) for all hello compute linked services</span></span> |   

## <a name="dataset"></a><span data-ttu-id="b4da5-317">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-317">Dataset</span></span> 
<span data-ttu-id="b4da5-318">Az Azure Data Factory dataset a következők:</span><span class="sxs-lookup"><span data-stu-id="b4da5-318">A dataset in Azure Data Factory is defined as follows:</span></span>

```json
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "external": <boolean flag tooindicate external data. only for input datasets>,
        "linkedServiceName": "<Name of hello linked service that refers tooa data store.>",
        "structure": [
            {
                "name": "<Name of hello column>",
                "type": "<Name of hello type>"
            }
        ],
        "typeProperties": {
            "<type specific property>": "<value>",
            "<type specific property 2>": "<value 2>",
        },
        "availability": {
            "frequency": "<Specifies hello time unit for data slice production. Supported frequency: Minute, Hour, Day, Week, Month>",
            "interval": "<Specifies hello interval within hello defined frequency. For example, frequency set too'Hour' and interval set too1 indicates that new data slices should be produced hourly>"
        },
       "policy":
        {      
        }
    }
}
```

<span data-ttu-id="b4da5-319">a következő táblázat hello hello fent JSON-tulajdonságokat ismerteti:</span><span class="sxs-lookup"><span data-stu-id="b4da5-319">hello following table describes properties in hello above JSON:</span></span>   

| <span data-ttu-id="b4da5-320">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-320">Property</span></span> | <span data-ttu-id="b4da5-321">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-321">Description</span></span> | <span data-ttu-id="b4da5-322">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-322">Required</span></span> | <span data-ttu-id="b4da5-323">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="b4da5-323">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-324">név</span><span class="sxs-lookup"><span data-stu-id="b4da5-324">name</span></span> | <span data-ttu-id="b4da5-325">Hello DataSet adatkészlet neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-325">Name of hello dataset.</span></span> <span data-ttu-id="b4da5-326">Lásd: [Azure Data Factory - elnevezési szabályait](data-factory-naming-rules.md) elnevezési szabályait.</span><span class="sxs-lookup"><span data-stu-id="b4da5-326">See [Azure Data Factory - Naming rules](data-factory-naming-rules.md) for naming rules.</span></span> |<span data-ttu-id="b4da5-327">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-327">Yes</span></span> |<span data-ttu-id="b4da5-328">NA</span><span class="sxs-lookup"><span data-stu-id="b4da5-328">NA</span></span> |
| <span data-ttu-id="b4da5-329">type</span><span class="sxs-lookup"><span data-stu-id="b4da5-329">type</span></span> | <span data-ttu-id="b4da5-330">Hello dataset típusa.</span><span class="sxs-lookup"><span data-stu-id="b4da5-330">Type of hello dataset.</span></span> <span data-ttu-id="b4da5-331">Adjon meg egy Azure Data Factory által támogatott hello típusok (például: AzureBlob, AzureSqlTable).</span><span class="sxs-lookup"><span data-stu-id="b4da5-331">Specify one of hello types supported by Azure Data Factory (for example: AzureBlob, AzureSqlTable).</span></span> <span data-ttu-id="b4da5-332">Lásd: [ADATTÁROLÓKHOZ](#data-stores) szakasz az összes hello adattárolókhoz és a Data Factory által támogatott adatkészlet-típusok.</span><span class="sxs-lookup"><span data-stu-id="b4da5-332">See [DATA STORES](#data-stores) section for all hello data stores and dataset types supported by Data Factory.</span></span> | 
| <span data-ttu-id="b4da5-333">struktúra</span><span class="sxs-lookup"><span data-stu-id="b4da5-333">structure</span></span> | <span data-ttu-id="b4da5-334">Hello adatkészlet sémája.</span><span class="sxs-lookup"><span data-stu-id="b4da5-334">Schema of hello dataset.</span></span> <span data-ttu-id="b4da5-335">Tartalmaz oszlopok, azok típusok, stb.</span><span class="sxs-lookup"><span data-stu-id="b4da5-335">It contains columns, their types, etc.</span></span> | <span data-ttu-id="b4da5-336">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-336">No</span></span> |<span data-ttu-id="b4da5-337">NA</span><span class="sxs-lookup"><span data-stu-id="b4da5-337">NA</span></span> |
| <span data-ttu-id="b4da5-338">typeProperties</span><span class="sxs-lookup"><span data-stu-id="b4da5-338">typeProperties</span></span> | <span data-ttu-id="b4da5-339">Tulajdonságok toohello megfelelő típust választotta.</span><span class="sxs-lookup"><span data-stu-id="b4da5-339">Properties corresponding toohello selected type.</span></span> <span data-ttu-id="b4da5-340">Lásd: [ADATTÁROLÓKHOZ](#data-stores) szakaszban a támogatott típusok és azok tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="b4da5-340">See [DATA STORES](#data-stores) section for supported types and their properties.</span></span> |<span data-ttu-id="b4da5-341">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-341">Yes</span></span> |<span data-ttu-id="b4da5-342">NA</span><span class="sxs-lookup"><span data-stu-id="b4da5-342">NA</span></span> |
| <span data-ttu-id="b4da5-343">external</span><span class="sxs-lookup"><span data-stu-id="b4da5-343">external</span></span> | <span data-ttu-id="b4da5-344">Logikai jelző toospecify, hogy a DataSet adatkészlet explicit módon hozzák a data factory-folyamat vagy nem.</span><span class="sxs-lookup"><span data-stu-id="b4da5-344">Boolean flag toospecify whether a dataset is explicitly produced by a data factory pipeline or not.</span></span> |<span data-ttu-id="b4da5-345">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-345">No</span></span> |<span data-ttu-id="b4da5-346">hamis</span><span class="sxs-lookup"><span data-stu-id="b4da5-346">false</span></span> |
| <span data-ttu-id="b4da5-347">rendelkezésre állás</span><span class="sxs-lookup"><span data-stu-id="b4da5-347">availability</span></span> | <span data-ttu-id="b4da5-348">Feldolgozási időszakát, vagy a felosztás hello dataset üzemi modell hello hello határozza meg.</span><span class="sxs-lookup"><span data-stu-id="b4da5-348">Defines hello processing window or hello slicing model for hello dataset production.</span></span> <span data-ttu-id="b4da5-349">További részletek a felosztás modell hello adatkészlet: [ütemezés és a végrehajtás](data-factory-scheduling-and-execution.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-349">For details on hello dataset slicing model, see [Scheduling and Execution](data-factory-scheduling-and-execution.md) article.</span></span> |<span data-ttu-id="b4da5-350">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-350">Yes</span></span> |<span data-ttu-id="b4da5-351">NA</span><span class="sxs-lookup"><span data-stu-id="b4da5-351">NA</span></span> |
| <span data-ttu-id="b4da5-352">szabályzat</span><span class="sxs-lookup"><span data-stu-id="b4da5-352">policy</span></span> |<span data-ttu-id="b4da5-353">Hello feltételek vagy hello dataset szeletek kell néhány előfeltételnek hello feltételt határoz meg.</span><span class="sxs-lookup"><span data-stu-id="b4da5-353">Defines hello criteria or hello condition that hello dataset slices must fulfill.</span></span> <br/><br/><span data-ttu-id="b4da5-354">További információkért lásd: [Dataset házirend](#Policy) szakasz.</span><span class="sxs-lookup"><span data-stu-id="b4da5-354">For details, see [Dataset Policy](#Policy) section.</span></span> |<span data-ttu-id="b4da5-355">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-355">No</span></span> |<span data-ttu-id="b4da5-356">NA</span><span class="sxs-lookup"><span data-stu-id="b4da5-356">NA</span></span> |

<span data-ttu-id="b4da5-357">Minden egyes oszlopának hello **struktúra** szakasz hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="b4da5-357">Each column in hello **structure** section contains hello following properties:</span></span>

| <span data-ttu-id="b4da5-358">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-358">Property</span></span> | <span data-ttu-id="b4da5-359">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-359">Description</span></span> | <span data-ttu-id="b4da5-360">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-360">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-361">név</span><span class="sxs-lookup"><span data-stu-id="b4da5-361">name</span></span> |<span data-ttu-id="b4da5-362">Hello oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-362">Name of hello column.</span></span> |<span data-ttu-id="b4da5-363">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-363">Yes</span></span> |
| <span data-ttu-id="b4da5-364">type</span><span class="sxs-lookup"><span data-stu-id="b4da5-364">type</span></span> |<span data-ttu-id="b4da5-365">Hello oszlop adattípusát.</span><span class="sxs-lookup"><span data-stu-id="b4da5-365">Data type of hello column.</span></span>  |<span data-ttu-id="b4da5-366">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-366">No</span></span> |
| <span data-ttu-id="b4da5-367">Kulturális környezet</span><span class="sxs-lookup"><span data-stu-id="b4da5-367">culture</span></span> |<span data-ttu-id="b4da5-368">.NET-alapú kulturális környezet toobe használható, ha a típus meg van adva, de a .NET-típus `Datetime` vagy `Datetimeoffset`.</span><span class="sxs-lookup"><span data-stu-id="b4da5-368">.NET based culture toobe used when type is specified and is .NET type `Datetime` or `Datetimeoffset`.</span></span> <span data-ttu-id="b4da5-369">Alapértelmezett érték a `en-us`.</span><span class="sxs-lookup"><span data-stu-id="b4da5-369">Default is `en-us`.</span></span> |<span data-ttu-id="b4da5-370">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-370">No</span></span> |
| <span data-ttu-id="b4da5-371">Formátumban</span><span class="sxs-lookup"><span data-stu-id="b4da5-371">format</span></span> |<span data-ttu-id="b4da5-372">Formázza használható, ha a típus meg van adva, de .NET típusú karakterlánc toobe `Datetime` vagy `Datetimeoffset`.</span><span class="sxs-lookup"><span data-stu-id="b4da5-372">Format string toobe used when type is specified and is .NET type `Datetime` or `Datetimeoffset`.</span></span> |<span data-ttu-id="b4da5-373">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-373">No</span></span> |

<span data-ttu-id="b4da5-374">A következő példa hello, hello dataset adatkészletben három oszlopot `slicetimestamp`, `projectname`, és `pageviews` és típus: karakterlánc, karakterlánc és decimális rendre.</span><span class="sxs-lookup"><span data-stu-id="b4da5-374">In hello following example, hello dataset has three columns `slicetimestamp`, `projectname`, and `pageviews` and they are of type: String, String, and Decimal respectively.</span></span>

```json
structure:  
[
    { "name": "slicetimestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

<span data-ttu-id="b4da5-375">hello következő táblázat ismerteti a hello használható tulajdonságok **rendelkezésre állási** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-375">hello following table describes properties you can use in hello **availability** section:</span></span>

| <span data-ttu-id="b4da5-376">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-376">Property</span></span> | <span data-ttu-id="b4da5-377">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-377">Description</span></span> | <span data-ttu-id="b4da5-378">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-378">Required</span></span> | <span data-ttu-id="b4da5-379">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="b4da5-379">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-380">frequency</span><span class="sxs-lookup"><span data-stu-id="b4da5-380">frequency</span></span> |<span data-ttu-id="b4da5-381">Megadja a dataset szelet üzemi hello időegységét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-381">Specifies hello time unit for dataset slice production.</span></span><br/><br/><span data-ttu-id="b4da5-382"><b>Támogatott gyakoriság</b>: perc, óra, nap, hét, hónap</span><span class="sxs-lookup"><span data-stu-id="b4da5-382"><b>Supported frequency</b>: Minute, Hour, Day, Week, Month</span></span> |<span data-ttu-id="b4da5-383">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-383">Yes</span></span> |<span data-ttu-id="b4da5-384">NA</span><span class="sxs-lookup"><span data-stu-id="b4da5-384">NA</span></span> |
| <span data-ttu-id="b4da5-385">interval</span><span class="sxs-lookup"><span data-stu-id="b4da5-385">interval</span></span> |<span data-ttu-id="b4da5-386">Megadja egy szorzóval gyakoriság</span><span class="sxs-lookup"><span data-stu-id="b4da5-386">Specifies a multiplier for frequency</span></span><br/><br/><span data-ttu-id="b4da5-387">"X időköz" határozza meg, milyen gyakran hello szelet jön létre.</span><span class="sxs-lookup"><span data-stu-id="b4da5-387">”Frequency x interval” determines how often hello slice is produced.</span></span><br/><br/><span data-ttu-id="b4da5-388">Ha dataset toobe szeletelhetők óránként kell hello, akkor be <b>gyakoriság</b> túl<b>óra</b>, és <b>időköz</b> túl<b>1</b>.</span><span class="sxs-lookup"><span data-stu-id="b4da5-388">If you need hello dataset toobe sliced on an hourly basis, you set <b>Frequency</b> too<b>Hour</b>, and <b>interval</b> too<b>1</b>.</span></span><br/><br/><span data-ttu-id="b4da5-389"><b>Megjegyzés:</b>: perces gyakoriságot ad meg, ha azt javasoljuk, hogy állítsa a 15-nál kisebb hello időköz toono</span><span class="sxs-lookup"><span data-stu-id="b4da5-389"><b>Note</b>: If you specify Frequency as Minute, we recommend that you set hello interval toono less than 15</span></span> |<span data-ttu-id="b4da5-390">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-390">Yes</span></span> |<span data-ttu-id="b4da5-391">NA</span><span class="sxs-lookup"><span data-stu-id="b4da5-391">NA</span></span> |
| <span data-ttu-id="b4da5-392">stílus</span><span class="sxs-lookup"><span data-stu-id="b4da5-392">style</span></span> |<span data-ttu-id="b4da5-393">Meghatározza, hogy kell-e hello szelet előállított hello kezdő/záró hello időköz.</span><span class="sxs-lookup"><span data-stu-id="b4da5-393">Specifies whether hello slice should be produced at hello start/end of hello interval.</span></span><ul><li><span data-ttu-id="b4da5-394">StartOfInterval</span><span class="sxs-lookup"><span data-stu-id="b4da5-394">StartOfInterval</span></span></li><li><span data-ttu-id="b4da5-395">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="b4da5-395">EndOfInterval</span></span></li></ul><br/><br/><span data-ttu-id="b4da5-396">Ha gyakoriságának beállítása tooMonth és stílus tooEndOfInterval van beállítva, hello szelet a hónap utolsó napján hello elő.</span><span class="sxs-lookup"><span data-stu-id="b4da5-396">If Frequency is set tooMonth and style is set tooEndOfInterval, hello slice is produced on hello last day of month.</span></span> <span data-ttu-id="b4da5-397">Ha hello stílus be van állítva tooStartOfInterval, hello szelet elő hello a hónap első napján.</span><span class="sxs-lookup"><span data-stu-id="b4da5-397">If hello style is set tooStartOfInterval, hello slice is produced on hello first day of month.</span></span><br/><br/><span data-ttu-id="b4da5-398">Ha gyakoriságának beállítása tooDay és stílus tooEndOfInterval van beállítva, hello szelet elő az elmúlt egy órában hello nap hello.</span><span class="sxs-lookup"><span data-stu-id="b4da5-398">If Frequency is set tooDay and style is set tooEndOfInterval, hello slice is produced in hello last hour of hello day.</span></span><br/><br/><span data-ttu-id="b4da5-399">Ha gyakoriság tooHour és stílus tooEndOfInterval van beállítva, hello szelet hello végének hello keletkezik.</span><span class="sxs-lookup"><span data-stu-id="b4da5-399">If Frequency is set tooHour and style is set tooEndOfInterval, hello slice is produced at hello end of hello hour.</span></span> <span data-ttu-id="b4da5-400">Például a szelet du. 1 – 2 óra időszakban, a hello szelet hozzák 2 du..</span><span class="sxs-lookup"><span data-stu-id="b4da5-400">For example, for a slice for 1 PM – 2 PM period, hello slice is produced at 2 PM.</span></span> |<span data-ttu-id="b4da5-401">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-401">No</span></span> |<span data-ttu-id="b4da5-402">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="b4da5-402">EndOfInterval</span></span> |
| <span data-ttu-id="b4da5-403">anchorDateTime</span><span class="sxs-lookup"><span data-stu-id="b4da5-403">anchorDateTime</span></span> |<span data-ttu-id="b4da5-404">Hello abszolút pozíciója a ennyi másodpercig használta a Feladatütemező toocompute dataset szelet határok meghatározása.</span><span class="sxs-lookup"><span data-stu-id="b4da5-404">Defines hello absolute position in time used by scheduler toocompute dataset slice boundaries.</span></span> <br/><br/><span data-ttu-id="b4da5-405"><b>Megjegyzés:</b>: Ha hello AnchorDateTime részekből dátum, amelyek részletesebben, mint a hello gyakorisága, akkor hello részletesebb részek figyelmen kívül lesznek hagyva.</span><span class="sxs-lookup"><span data-stu-id="b4da5-405"><b>Note</b>: If hello AnchorDateTime has date parts that are more granular than hello frequency then hello more granular parts are ignored.</span></span> <br/><br/><span data-ttu-id="b4da5-406">Például, ha hello <b>időköz</b> van <b>óránkénti</b> (gyakoriság: óra és időköz: 1) és hello <b>AnchorDateTime</b> tartalmaz <b>percet és másodpercet</b>majd hello <b>percet és másodpercet</b> hello AnchorDateTime részei a rendszer figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="b4da5-406">For example, if hello <b>interval</b> is <b>hourly</b> (frequency: hour and interval: 1) and hello <b>AnchorDateTime</b> contains <b>minutes and seconds</b> then hello <b>minutes and seconds</b> parts of hello AnchorDateTime are ignored.</span></span> |<span data-ttu-id="b4da5-407">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-407">No</span></span> |<span data-ttu-id="b4da5-408">01/01/0001</span><span class="sxs-lookup"><span data-stu-id="b4da5-408">01/01/0001</span></span> |
| <span data-ttu-id="b4da5-409">Az offset</span><span class="sxs-lookup"><span data-stu-id="b4da5-409">offset</span></span> |<span data-ttu-id="b4da5-410">TimeSpan érték, mely hello által kezdő és záró összes adatkészlet szeletek vette.</span><span class="sxs-lookup"><span data-stu-id="b4da5-410">Timespan by which hello start and end of all dataset slices are shifted.</span></span> <br/><br/><span data-ttu-id="b4da5-411"><b>Megjegyzés:</b>: Ha anchorDateTime és az offset is meg van adva, hello eredménye kombinált hello shift.</span><span class="sxs-lookup"><span data-stu-id="b4da5-411"><b>Note</b>: If both anchorDateTime and offset are specified, hello result is hello combined shift.</span></span> |<span data-ttu-id="b4da5-412">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-412">No</span></span> |<span data-ttu-id="b4da5-413">NA</span><span class="sxs-lookup"><span data-stu-id="b4da5-413">NA</span></span> |

<span data-ttu-id="b4da5-414">a következő rendelkezésre állással kapcsolatos szakaszának hello Megadja, hogy adott hello kimeneti adatkészlet előállított óránként (vagy) bemeneti adatkészlet óránkénti áll rendelkezésre:</span><span class="sxs-lookup"><span data-stu-id="b4da5-414">hello following availability section specifies that hello output dataset is either produced hourly (or) input dataset is available hourly:</span></span>

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 1    
}
```

<span data-ttu-id="b4da5-415">Hello **házirend** hello feltételek rész az adatkészlet-definícióban vagy hello feltétellel, hogy a dataset szeletek hello teljesítenie kell.</span><span class="sxs-lookup"><span data-stu-id="b4da5-415">hello **policy** section in dataset definition defines hello criteria or hello condition that hello dataset slices must fulfill.</span></span>

| <span data-ttu-id="b4da5-416">Házirend neve</span><span class="sxs-lookup"><span data-stu-id="b4da5-416">Policy Name</span></span> | <span data-ttu-id="b4da5-417">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-417">Description</span></span> | <span data-ttu-id="b4da5-418">Alkalmazott túl</span><span class="sxs-lookup"><span data-stu-id="b4da5-418">Applied too</span></span>| <span data-ttu-id="b4da5-419">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-419">Required</span></span> | <span data-ttu-id="b4da5-420">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="b4da5-420">Default</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="b4da5-421">minimumSizeMB</span><span class="sxs-lookup"><span data-stu-id="b4da5-421">minimumSizeMB</span></span> |<span data-ttu-id="b4da5-422">Ellenőrzi, hogy hello adatokat egy **Azure blob** megfelel-e hello (megabájtban) a minimális méret követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="b4da5-422">Validates that hello data in an **Azure blob** meets hello minimum size requirements (in megabytes).</span></span> |<span data-ttu-id="b4da5-423">Azure-blob</span><span class="sxs-lookup"><span data-stu-id="b4da5-423">Azure Blob</span></span> |<span data-ttu-id="b4da5-424">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-424">No</span></span> |<span data-ttu-id="b4da5-425">NA</span><span class="sxs-lookup"><span data-stu-id="b4da5-425">NA</span></span> |
| <span data-ttu-id="b4da5-426">minimumRows</span><span class="sxs-lookup"><span data-stu-id="b4da5-426">minimumRows</span></span> |<span data-ttu-id="b4da5-427">Ellenőrzi, hogy hello adatokat egy **Azure SQL adatbázis** vagy egy **Azure-tábla** hello a sorok legkisebb számát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b4da5-427">Validates that hello data in an **Azure SQL database** or an **Azure table** contains hello minimum number of rows.</span></span> |<ul><li><span data-ttu-id="b4da5-428">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="b4da5-428">Azure SQL Database</span></span></li><li><span data-ttu-id="b4da5-429">Azure-tábla</span><span class="sxs-lookup"><span data-stu-id="b4da5-429">Azure Table</span></span></li></ul> |<span data-ttu-id="b4da5-430">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-430">No</span></span> |<span data-ttu-id="b4da5-431">NA</span><span class="sxs-lookup"><span data-stu-id="b4da5-431">NA</span></span> |

<span data-ttu-id="b4da5-432">**Példa**</span><span class="sxs-lookup"><span data-stu-id="b4da5-432">**Example:**</span></span>

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

<span data-ttu-id="b4da5-433">Azure Data Factory hozzák alatt álló adatkészlet, kivéve azt állapotúként kell megjelölni **külső**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-433">Unless a dataset is being produced by Azure Data Factory, it should be marked as **external**.</span></span> <span data-ttu-id="b4da5-434">Ez a beállítás toohello bemenet az adatcsatorna első tevékenység általában vonatkozik, kivéve, ha a tevékenység vagy csővezeték-láncolás használatban van.</span><span class="sxs-lookup"><span data-stu-id="b4da5-434">This setting generally applies toohello inputs of first activity in a pipeline unless activity or pipeline chaining is being used.</span></span>

| <span data-ttu-id="b4da5-435">Név</span><span class="sxs-lookup"><span data-stu-id="b4da5-435">Name</span></span> | <span data-ttu-id="b4da5-436">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-436">Description</span></span> | <span data-ttu-id="b4da5-437">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-437">Required</span></span> | <span data-ttu-id="b4da5-438">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="b4da5-438">Default Value</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-439">dataDelay</span><span class="sxs-lookup"><span data-stu-id="b4da5-439">dataDelay</span></span> |<span data-ttu-id="b4da5-440">Idő toodelay hello ellenőrzése hello külső adatok számára megadott szelet hello hello rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="b4da5-440">Time toodelay hello check on hello availability of hello external data for hello given slice.</span></span> <span data-ttu-id="b4da5-441">Például ha hello érhetők el adatok óránkénti, hello ellenőrzés toosee hello külső adatok elérhetők legyenek és hello megfelelő szelet készen felszámítása dataDelay használatával.</span><span class="sxs-lookup"><span data-stu-id="b4da5-441">For example, if hello data is available hourly, hello check toosee hello external data is available and hello corresponding slice is Ready can be delayed by using dataDelay.</span></span><br/><br/><span data-ttu-id="b4da5-442">Csak érvényes toohello jelenlegi idő.</span><span class="sxs-lookup"><span data-stu-id="b4da5-442">Only applies toohello present time.</span></span>  <span data-ttu-id="b4da5-443">Például ha 1:00 PM azonnal, és az értéke 10 perc hello érvényesítési kezdődik, 1:10 óra.</span><span class="sxs-lookup"><span data-stu-id="b4da5-443">For example, if it is 1:00 PM right now and this value is 10 minutes, hello validation starts at 1:10 PM.</span></span><br/><br/><span data-ttu-id="b4da5-444">Ez a beállítás nem befolyásolja az elmúlt hello szeletek (szelet befejezési időpontja + dataDelay szeletek < most) dolgoznak fel késedelem nélkül.</span><span class="sxs-lookup"><span data-stu-id="b4da5-444">This setting does not affect slices in hello past (slices with Slice End Time + dataDelay < Now) are processed without any delay.</span></span><br/><br/><span data-ttu-id="b4da5-445">23:59 óra kell hello segítségével toospecified hosszabb idő `day.hours:minutes:seconds` formátumban.</span><span class="sxs-lookup"><span data-stu-id="b4da5-445">Time greater than 23:59 hours need toospecified using hello `day.hours:minutes:seconds` format.</span></span> <span data-ttu-id="b4da5-446">Például toospecify 24 órát, ne használjon 24:00:00. Ehelyett használjon 1.00:00:00.</span><span class="sxs-lookup"><span data-stu-id="b4da5-446">For example, toospecify 24 hours, don't use 24:00:00; instead, use 1.00:00:00.</span></span> <span data-ttu-id="b4da5-447">Ha 24:00:00 használja, akkor a rendszer 24 napos (24.00:00:00).</span><span class="sxs-lookup"><span data-stu-id="b4da5-447">If you use 24:00:00, it is treated as 24 days (24.00:00:00).</span></span> <span data-ttu-id="b4da5-448">1 nap és 4 óra adja meg 1:04:00:00.</span><span class="sxs-lookup"><span data-stu-id="b4da5-448">For 1 day and 4 hours, specify 1:04:00:00.</span></span> |<span data-ttu-id="b4da5-449">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-449">No</span></span> |<span data-ttu-id="b4da5-450">0</span><span class="sxs-lookup"><span data-stu-id="b4da5-450">0</span></span> |
| <span data-ttu-id="b4da5-451">RetryInterval</span><span class="sxs-lookup"><span data-stu-id="b4da5-451">retryInterval</span></span> |<span data-ttu-id="b4da5-452">hello várakozási idő a hibákhoz és hello következő újrapróbálkozások között.</span><span class="sxs-lookup"><span data-stu-id="b4da5-452">hello wait time between a failure and hello next retry attempt.</span></span> <span data-ttu-id="b4da5-453">Ha nem sikerül egy try, hello következő kísérlet retryInterval utánra esik.</span><span class="sxs-lookup"><span data-stu-id="b4da5-453">If a try fails, hello next try is after retryInterval.</span></span> <br/><br/><span data-ttu-id="b4da5-454">Ha 1:00 PM most, az első lépések hello első próbálkozás.</span><span class="sxs-lookup"><span data-stu-id="b4da5-454">If it is 1:00 PM right now, we begin hello first try.</span></span> <span data-ttu-id="b4da5-455">Hello időtartama toocomplete hello első eredetiség ellenőrzésének 1 perc és hello művelet végrehajtása sikertelen volt, hello legközelebbi újrapróbálkozás akkor 1:00 + 1 perc (időtartam) + (újrapróbálkozási időköz) 1 perc = 1:02 PM.</span><span class="sxs-lookup"><span data-stu-id="b4da5-455">If hello duration toocomplete hello first validation check is 1 minute and hello operation failed, hello next retry is at 1:00 + 1 min (duration) + 1 min (retry interval) = 1:02 PM.</span></span> <br/><br/><span data-ttu-id="b4da5-456">Az elmúlt hello szeletek nincs késleltetés.</span><span class="sxs-lookup"><span data-stu-id="b4da5-456">For slices in hello past, there is no delay.</span></span> <span data-ttu-id="b4da5-457">hello újrapróbálkozási azonnal történik.</span><span class="sxs-lookup"><span data-stu-id="b4da5-457">hello retry happens immediately.</span></span> |<span data-ttu-id="b4da5-458">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-458">No</span></span> |<span data-ttu-id="b4da5-459">00:01:00 (1 perc)</span><span class="sxs-lookup"><span data-stu-id="b4da5-459">00:01:00 (1 minute)</span></span> |
| <span data-ttu-id="b4da5-460">retryTimeout</span><span class="sxs-lookup"><span data-stu-id="b4da5-460">retryTimeout</span></span> |<span data-ttu-id="b4da5-461">hello időkorlátjának újrapróbálkozási kísérletei.</span><span class="sxs-lookup"><span data-stu-id="b4da5-461">hello timeout for each retry attempt.</span></span><br/><br/><span data-ttu-id="b4da5-462">Ha ez a tulajdonság értéke too10 perc hello érvényesítési igények toobe 10 percen belül fejeződött be.</span><span class="sxs-lookup"><span data-stu-id="b4da5-462">If this property is set too10 minutes, hello validation needs toobe completed within 10 minutes.</span></span> <span data-ttu-id="b4da5-463">Ha hosszabb ideig tart, mint 10 percig tooperform hello érvényesítési, hello időtúllépést próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="b4da5-463">If it takes longer than 10 minutes tooperform hello validation, hello retry times out.</span></span><br/><br/><span data-ttu-id="b4da5-464">Ha bármilyen kísérlet hello érvényesítéshez időkorlátja lejár, hello szelet időtúllépésbe került van megjelölve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-464">If all attempts for hello validation times out, hello slice is marked as TimedOut.</span></span> |<span data-ttu-id="b4da5-465">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-465">No</span></span> |<span data-ttu-id="b4da5-466">00:10:00 (10 perc)</span><span class="sxs-lookup"><span data-stu-id="b4da5-466">00:10:00 (10 minutes)</span></span> |
| <span data-ttu-id="b4da5-467">maximumRetry</span><span class="sxs-lookup"><span data-stu-id="b4da5-467">maximumRetry</span></span> |<span data-ttu-id="b4da5-468">Számos alkalommal fordult elő az toocheck a hello hello külső adatok rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="b4da5-468">Number of times toocheck for hello availability of hello external data.</span></span> <span data-ttu-id="b4da5-469">hello engedélyezett maximális értéke 10.</span><span class="sxs-lookup"><span data-stu-id="b4da5-469">hello allowed maximum value is 10.</span></span> |<span data-ttu-id="b4da5-470">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-470">No</span></span> |<span data-ttu-id="b4da5-471">3</span><span class="sxs-lookup"><span data-stu-id="b4da5-471">3</span></span> |


## <a name="data-stores"></a><span data-ttu-id="b4da5-472">ADATTÁROLÓ</span><span class="sxs-lookup"><span data-stu-id="b4da5-472">DATA STORES</span></span>
<span data-ttu-id="b4da5-473">Hello [társított szolgáltatás](#linked-service) szakaszban megadott leírásainak összekapcsolt szolgáltatások gyakori tooall típusa JSON-elemek szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="b4da5-473">hello [Linked service](#linked-service) section provided descriptions for JSON elements that are common tooall types of linked services.</span></span> <span data-ttu-id="b4da5-474">Ez a szakasz ismerteti a JSON-elemek szerepelnek, amelyek adott tooeach adattár részleteit.</span><span class="sxs-lookup"><span data-stu-id="b4da5-474">This section provides details about JSON elements that are specific tooeach data store.</span></span>

<span data-ttu-id="b4da5-475">Hello [Dataset](#dataset) szakaszban megadott JSON-elemek szerepelnek, amelyek közös tooall fajta adatkészlet leírása.</span><span class="sxs-lookup"><span data-stu-id="b4da5-475">hello [Dataset](#dataset) section provided descriptions for JSON elements that are common tooall types of datasets.</span></span> <span data-ttu-id="b4da5-476">Ez a szakasz ismerteti a JSON-elemek szerepelnek, amelyek adott tooeach adattár részleteit.</span><span class="sxs-lookup"><span data-stu-id="b4da5-476">This section provides details about JSON elements that are specific tooeach data store.</span></span>

<span data-ttu-id="b4da5-477">Hello [tevékenység](#activity) JSON-elemek szerepelnek, amelyek közös tooall típusú tevékenységek leírásainak szakaszát.</span><span class="sxs-lookup"><span data-stu-id="b4da5-477">hello [Activity](#activity) section provided descriptions for JSON elements that are common tooall types of activities.</span></span> <span data-ttu-id="b4da5-478">Ez a szakasz ismerteti, amelyek adott tooeach adattár, mikor kell azokat alkalmazni, a másolási tevékenység során a forrás/fogadó JSON-elemek szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="b4da5-478">This section provides details about JSON elements that are specific tooeach data store when it is used as a source/sink in a copy activity.</span></span>  

<span data-ttu-id="b4da5-479">Megtudhatja, hogyan készítheti toosee hello JSON sémák kapcsolódó szolgáltatás, adatkészlet és forrás/fogadó hello másolási tevékenységhez hello hello tároló hello hivatkozásra kattintva.</span><span class="sxs-lookup"><span data-stu-id="b4da5-479">Click hello link for hello store you are interested in toosee hello JSON schemas for linked service, dataset, and hello source/sink for hello copy activity.</span></span>

| <span data-ttu-id="b4da5-480">Kategória</span><span class="sxs-lookup"><span data-stu-id="b4da5-480">Category</span></span> | <span data-ttu-id="b4da5-481">Adattár</span><span class="sxs-lookup"><span data-stu-id="b4da5-481">Data store</span></span> 
|:--- |:--- |
| <span data-ttu-id="b4da5-482">**Azure**</span><span class="sxs-lookup"><span data-stu-id="b4da5-482">**Azure**</span></span> |[<span data-ttu-id="b4da5-483">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="b4da5-483">Azure Blob storage</span></span>](#azure-blob-storage) |
| &nbsp; |[<span data-ttu-id="b4da5-484">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="b4da5-484">Azure Data Lake Store</span></span>](#azure-datalake-store) |
| &nbsp; |[<span data-ttu-id="b4da5-485">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="b4da5-485">Azure Cosmos DB</span></span>](#azure-cosmos-db) |
| &nbsp; |[<span data-ttu-id="b4da5-486">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="b4da5-486">Azure SQL Database</span></span>](#azure-sql-database) |
| &nbsp; |[<span data-ttu-id="b4da5-487">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="b4da5-487">Azure SQL Data Warehouse</span></span>](#azure-sql-data-warehouse) |
| &nbsp; |[<span data-ttu-id="b4da5-488">Azure Search</span><span class="sxs-lookup"><span data-stu-id="b4da5-488">Azure Search</span></span>](#azure-search) |
| &nbsp; |[<span data-ttu-id="b4da5-489">Azure Table storage</span><span class="sxs-lookup"><span data-stu-id="b4da5-489">Azure Table storage</span></span>](#azure-table-storage) |
| <span data-ttu-id="b4da5-490">**Adatbázisok**</span><span class="sxs-lookup"><span data-stu-id="b4da5-490">**Databases**</span></span> |[<span data-ttu-id="b4da5-491">Amazon Redshift</span><span class="sxs-lookup"><span data-stu-id="b4da5-491">Amazon Redshift</span></span>](#amazon-redshift) |
| &nbsp; |[<span data-ttu-id="b4da5-492">IBM DB2</span><span class="sxs-lookup"><span data-stu-id="b4da5-492">IBM DB2</span></span>](#ibm-db2) |
| &nbsp; |[<span data-ttu-id="b4da5-493">MySQL</span><span class="sxs-lookup"><span data-stu-id="b4da5-493">MySQL</span></span>](#mysql) |
| &nbsp; |[<span data-ttu-id="b4da5-494">Oracle</span><span class="sxs-lookup"><span data-stu-id="b4da5-494">Oracle</span></span>](#oracle) |
| &nbsp; |[<span data-ttu-id="b4da5-495">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="b4da5-495">PostgreSQL</span></span>](#postgresql) |
| &nbsp; |[<span data-ttu-id="b4da5-496">SAP Business Warehouse</span><span class="sxs-lookup"><span data-stu-id="b4da5-496">SAP Business Warehouse</span></span>](#sap-business-warehouse) |
| &nbsp; |[<span data-ttu-id="b4da5-497">SAP HANA</span><span class="sxs-lookup"><span data-stu-id="b4da5-497">SAP HANA</span></span>](#sap-hana) |
| &nbsp; |[<span data-ttu-id="b4da5-498">SQL Server</span><span class="sxs-lookup"><span data-stu-id="b4da5-498">SQL Server</span></span>](#sql-server) |
| &nbsp; |[<span data-ttu-id="b4da5-499">Sybase</span><span class="sxs-lookup"><span data-stu-id="b4da5-499">Sybase</span></span>](#sybase) |
| &nbsp; |[<span data-ttu-id="b4da5-500">Teradata</span><span class="sxs-lookup"><span data-stu-id="b4da5-500">Teradata</span></span>](#teradata) |
| <span data-ttu-id="b4da5-501">**NoSQL**</span><span class="sxs-lookup"><span data-stu-id="b4da5-501">**NoSQL**</span></span> |[<span data-ttu-id="b4da5-502">Cassandra</span><span class="sxs-lookup"><span data-stu-id="b4da5-502">Cassandra</span></span>](#cassandra) |
| &nbsp; |[<span data-ttu-id="b4da5-503">MongoDB</span><span class="sxs-lookup"><span data-stu-id="b4da5-503">MongoDB</span></span>](#mongodb) |
| <span data-ttu-id="b4da5-504">**Fájl**</span><span class="sxs-lookup"><span data-stu-id="b4da5-504">**File**</span></span> |[<span data-ttu-id="b4da5-505">Amazon S3</span><span class="sxs-lookup"><span data-stu-id="b4da5-505">Amazon S3</span></span>](#amazon-s3) |
| &nbsp; |[<span data-ttu-id="b4da5-506">Fájlrendszer</span><span class="sxs-lookup"><span data-stu-id="b4da5-506">File System</span></span>](#file-system) |
| &nbsp; |[<span data-ttu-id="b4da5-507">FTP</span><span class="sxs-lookup"><span data-stu-id="b4da5-507">FTP</span></span>](#ftp) |
| &nbsp; |[<span data-ttu-id="b4da5-508">HDFS</span><span class="sxs-lookup"><span data-stu-id="b4da5-508">HDFS</span></span>](#hdfs) |
| &nbsp; |[<span data-ttu-id="b4da5-509">SFTP</span><span class="sxs-lookup"><span data-stu-id="b4da5-509">SFTP</span></span>](#sftp) |
| <span data-ttu-id="b4da5-510">**Egyéb**</span><span class="sxs-lookup"><span data-stu-id="b4da5-510">**Others**</span></span> |[<span data-ttu-id="b4da5-511">HTTP</span><span class="sxs-lookup"><span data-stu-id="b4da5-511">HTTP</span></span>](#http) |
| &nbsp; |[<span data-ttu-id="b4da5-512">OData</span><span class="sxs-lookup"><span data-stu-id="b4da5-512">OData</span></span>](#odata) |
| &nbsp; |[<span data-ttu-id="b4da5-513">ODBC</span><span class="sxs-lookup"><span data-stu-id="b4da5-513">ODBC</span></span>](#odbc) |
| &nbsp; |[<span data-ttu-id="b4da5-514">Salesforce</span><span class="sxs-lookup"><span data-stu-id="b4da5-514">Salesforce</span></span>](#salesforce) |
| &nbsp; |[<span data-ttu-id="b4da5-515">Webes tábla</span><span class="sxs-lookup"><span data-stu-id="b4da5-515">Web Table</span></span>](#web-table) |

## <a name="azure-blob-storage"></a><span data-ttu-id="b4da5-516">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="b4da5-516">Azure Blob Storage</span></span>

### <a name="linked-service"></a><span data-ttu-id="b4da5-517">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-517">Linked service</span></span>
<span data-ttu-id="b4da5-518">Társított szolgáltatások két típusa van: Azure Storage társított szolgáltatásnak, és a társított szolgáltatásnak Azure Storage SAS.</span><span class="sxs-lookup"><span data-stu-id="b4da5-518">There are two types of linked services: Azure Storage linked service and Azure Storage SAS linked service.</span></span>

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="b4da5-519">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b4da5-519">Azure Storage Linked Service</span></span>
<span data-ttu-id="b4da5-520">toolink a az Azure storage-tooa data factory hello segítségével **fiókkulcs**, hozzon létre egy Azure Storage társított szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b4da5-520">toolink your Azure storage account tooa data factory by using hello **account key**, create an Azure Storage linked service.</span></span> <span data-ttu-id="b4da5-521">toodefine egy Azure Storage társított szolgáltatásnak, set hello **típus** hello a társított szolgáltatás túl**AzureStorage**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-521">toodefine an Azure Storage linked service, set hello **type** of hello linked service too**AzureStorage**.</span></span> <span data-ttu-id="b4da5-522">Ezután megadhatja tulajdonságok hello a következő **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-522">Then, you can specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="b4da5-523">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-523">Property</span></span> | <span data-ttu-id="b4da5-524">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-524">Description</span></span> | <span data-ttu-id="b4da5-525">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-525">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="b4da5-526">connectionString</span><span class="sxs-lookup"><span data-stu-id="b4da5-526">connectionString</span></span> |<span data-ttu-id="b4da5-527">Adjon meg információt tooconnect tooAzure tárolási hello connectionString tulajdonság szükséges.</span><span class="sxs-lookup"><span data-stu-id="b4da5-527">Specify information needed tooconnect tooAzure storage for hello connectionString property.</span></span> |<span data-ttu-id="b4da5-528">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-528">Yes</span></span> |

##### <a name="example"></a><span data-ttu-id="b4da5-529">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-529">Example</span></span>  

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

#### <a name="azure-storage-sas-linked-service"></a><span data-ttu-id="b4da5-530">Az Azure Storage SAS társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b4da5-530">Azure Storage SAS Linked Service</span></span>
<span data-ttu-id="b4da5-531">hello Azure Storage SAS kapcsolódó szolgáltatás lehetővé teszi egy Azure Storage-fiók tooan az Azure data factory toolink egy közös hozzáférésű Jogosultságkód (SAS) használatával.</span><span class="sxs-lookup"><span data-stu-id="b4da5-531">hello Azure Storage SAS linked service allows you toolink an Azure Storage Account tooan Azure data factory by using a Shared Access Signature (SAS).</span></span> <span data-ttu-id="b4da5-532">Ez hozzáférést biztosít a hello adat-előállító korlátozott/időhöz kötött tooall/specifikus erőforrások (blobtárolóban /) hello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="b4da5-532">It provides hello data factory with restricted/time-bound access tooall/specific resources (blob/container) in hello storage.</span></span> <span data-ttu-id="b4da5-533">toolink a az Azure storage-tooa data factory használatával a közös hozzáférésű Jogosultságkód, Azure Storage SAS társított szolgáltatás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b4da5-533">toolink your Azure storage account tooa data factory by using Shared Access Signature, create an Azure Storage SAS linked service.</span></span> <span data-ttu-id="b4da5-534">egy Azure Storage SAS toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-534">toodefine an Azure Storage SAS linked service, set hello **type** of hello linked service too**AzureStorageSas**.</span></span> <span data-ttu-id="b4da5-535">Ezután megadhatja tulajdonságok hello a következő **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-535">Then, you can specify following properties in hello **typeProperties** section:</span></span>   

| <span data-ttu-id="b4da5-536">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-536">Property</span></span> | <span data-ttu-id="b4da5-537">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-537">Description</span></span> | <span data-ttu-id="b4da5-538">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-538">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="b4da5-539">sasUri</span><span class="sxs-lookup"><span data-stu-id="b4da5-539">sasUri</span></span> |<span data-ttu-id="b4da5-540">Adja meg a megosztott hozzáférési aláírást URI toohello Azure tárolási erőforrások, például blob, a tároló vagy a tábla.</span><span class="sxs-lookup"><span data-stu-id="b4da5-540">Specify Shared Access Signature URI toohello Azure Storage resources such as blob, container, or table.</span></span> |<span data-ttu-id="b4da5-541">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-541">Yes</span></span> |

##### <a name="example"></a><span data-ttu-id="b4da5-542">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-542">Example</span></span>

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

<span data-ttu-id="b4da5-543">További információ a következő összekapcsolt szolgáltatások: [Azure Blob Storage összekötő](data-factory-azure-blob-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-543">For more information about these linked services, see [Azure Blob Storage connector](data-factory-azure-blob-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="b4da5-544">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-544">Dataset</span></span>
<span data-ttu-id="b4da5-545">egy Azure Blob-adathalmazra toodefine set hello **típus** hello adatkészlet túl**AzureBlob**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-545">toodefine an Azure Blob dataset, set hello **type** of hello dataset too**AzureBlob**.</span></span> <span data-ttu-id="b4da5-546">Ezt követően adja a következő hello Azure Blob tulajdonságokat hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-546">Then, specify hello following Azure Blob specific properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="b4da5-547">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-547">Property</span></span> | <span data-ttu-id="b4da5-548">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-548">Description</span></span> | <span data-ttu-id="b4da5-549">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-549">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-550">folderPath</span><span class="sxs-lookup"><span data-stu-id="b4da5-550">folderPath</span></span> |<span data-ttu-id="b4da5-551">Elérési út toohello tároló és mappa hello blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="b4da5-551">Path toohello container and folder in hello blob storage.</span></span> <span data-ttu-id="b4da5-552">Példa: myblobcontainer\myblobfolder\\</span><span class="sxs-lookup"><span data-stu-id="b4da5-552">Example: myblobcontainer\myblobfolder\\</span></span> |<span data-ttu-id="b4da5-553">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-553">Yes</span></span> |
| <span data-ttu-id="b4da5-554">fileName</span><span class="sxs-lookup"><span data-stu-id="b4da5-554">fileName</span></span> |<span data-ttu-id="b4da5-555">Hello blob neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-555">Name of hello blob.</span></span> <span data-ttu-id="b4da5-556">Fájlnév nem, kötelező, és a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="b4da5-556">fileName is optional and case-sensitive.</span></span><br/><br/><span data-ttu-id="b4da5-557">Ha meg kell adnia egy fájlnevet, hello (például a Másolás) tevékenység works hello adott Blob.</span><span class="sxs-lookup"><span data-stu-id="b4da5-557">If you specify a filename, hello activity (including Copy) works on hello specific Blob.</span></span><br/><br/><span data-ttu-id="b4da5-558">Ha nincs megadva fájlnév, másolása összes BLOB bemeneti adatkészlet hello folderPath tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b4da5-558">When fileName is not specified, Copy includes all Blobs in hello folderPath for input dataset.</span></span><br/><br/><span data-ttu-id="b4da5-559">Ha nincs megadva fájlnév egy kimeneti adatkészletet, hello hello létrehozott fájl lenne a következő formátumban hello: adatok. <Guid>.txt (például:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="b4da5-559">When fileName is not specified for an output dataset, hello name of hello generated file would be in hello following this format: Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="b4da5-560">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-560">No</span></span> |
| <span data-ttu-id="b4da5-561">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="b4da5-561">partitionedBy</span></span> |<span data-ttu-id="b4da5-562">partitionedBy egy nem kötelező tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b4da5-562">partitionedBy is an optional property.</span></span> <span data-ttu-id="b4da5-563">Használhatja a dinamikus folderPath toospecify és a fájlnév idő adatsorozat adatok.</span><span class="sxs-lookup"><span data-stu-id="b4da5-563">You can use it toospecify a dynamic folderPath and filename for time series data.</span></span> <span data-ttu-id="b4da5-564">Például folderPath is paraméteres adatok óránkénti.</span><span class="sxs-lookup"><span data-stu-id="b4da5-564">For example, folderPath can be parameterized for every hour of data.</span></span> |<span data-ttu-id="b4da5-565">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-565">No</span></span> |
| <span data-ttu-id="b4da5-566">Formátumban</span><span class="sxs-lookup"><span data-stu-id="b4da5-566">format</span></span> | <span data-ttu-id="b4da5-567">a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-567">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="b4da5-568">Set hello **típus** tulajdonság alapján formátum tooone ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="b4da5-568">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="b4da5-569">További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="b4da5-569">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="b4da5-570">Ha azt szeretné, túl**másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a hello formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban.</span><span class="sxs-lookup"><span data-stu-id="b4da5-570">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="b4da5-571">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-571">No</span></span> |
| <span data-ttu-id="b4da5-572">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="b4da5-572">compression</span></span> | <span data-ttu-id="b4da5-573">Adja meg a hello típusát és hello adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-573">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="b4da5-574">Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-574">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="b4da5-575">Támogatott szintek a következők: **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-575">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="b4da5-576">További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="b4da5-576">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="b4da5-577">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-577">No</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-578">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-578">Example</span></span>

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


<span data-ttu-id="b4da5-579">További információkért lásd: [Azure Blob összekötő](data-factory-azure-blob-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-579">For more information, see [Azure Blob connector](data-factory-azure-blob-connector.md#dataset-properties) article.</span></span>

### <a name="blobsource-in-copy-activity"></a><span data-ttu-id="b4da5-580">A másolási tevékenység BlobSource</span><span class="sxs-lookup"><span data-stu-id="b4da5-580">BlobSource in Copy Activity</span></span>
<span data-ttu-id="b4da5-581">Adatok másolása az Azure Blob-tárolóból, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**BlobSource**, és adja meg a következő tulajdonságok hello ** forrás ** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-581">If you are copying data from an Azure Blob Storage, set hello **source type** of hello copy activity too**BlobSource**, and specify following properties in hello **source **section:</span></span>

| <span data-ttu-id="b4da5-582">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-582">Property</span></span> | <span data-ttu-id="b4da5-583">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-583">Description</span></span> | <span data-ttu-id="b4da5-584">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-584">Allowed values</span></span> | <span data-ttu-id="b4da5-585">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-585">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-586">Rekurzív</span><span class="sxs-lookup"><span data-stu-id="b4da5-586">recursive</span></span> |<span data-ttu-id="b4da5-587">Azt jelzi, hogy hello adatolvasás rekurzív módon hello almappák vagy csak a megadott mappa hello.</span><span class="sxs-lookup"><span data-stu-id="b4da5-587">Indicates whether hello data is read recursively from hello sub folders or only from hello specified folder.</span></span> |<span data-ttu-id="b4da5-588">TRUE hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="b4da5-588">True (default value), False</span></span> |<span data-ttu-id="b4da5-589">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-589">No</span></span> |

#### <a name="example-blobsource"></a><span data-ttu-id="b4da5-590">Példa: BlobSource **</span><span class="sxs-lookup"><span data-stu-id="b4da5-590">Example: BlobSource**</span></span>
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
### <a name="blobsink-in-copy-activity"></a><span data-ttu-id="b4da5-591">A másolási tevékenység BlobSink</span><span class="sxs-lookup"><span data-stu-id="b4da5-591">BlobSink in Copy Activity</span></span>
<span data-ttu-id="b4da5-592">Adatok tooan Azure Blob Storage másolása, állítsa be a hello **típus gyűjtése** hello a másolási tevékenység túl**BlobSink**, és adja meg a következő tulajdonságok hello **fogadó** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-592">If you are copying data tooan Azure Blob Storage, set hello **sink type** of hello copy activity too**BlobSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="b4da5-593">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-593">Property</span></span> | <span data-ttu-id="b4da5-594">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-594">Description</span></span> | <span data-ttu-id="b4da5-595">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-595">Allowed values</span></span> | <span data-ttu-id="b4da5-596">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-596">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-597">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="b4da5-597">copyBehavior</span></span> |<span data-ttu-id="b4da5-598">Hello másolási viselkedését határozza meg, ha hello adatforrás BlobSource vagy a fájlrendszer.</span><span class="sxs-lookup"><span data-stu-id="b4da5-598">Defines hello copy behavior when hello source is BlobSource or FileSystem.</span></span> |<span data-ttu-id="b4da5-599"><b>PreserveHierarchy</b>: megtartja hello fájl hierarchia hello célmappában.</span><span class="sxs-lookup"><span data-stu-id="b4da5-599"><b>PreserveHierarchy</b>: preserves hello file hierarchy in hello target folder.</span></span> <span data-ttu-id="b4da5-600">hello relatív fájl toosource forrásmappa elérési út azonos toohello fájl tootarget célmappa relatív elérési útját.</span><span class="sxs-lookup"><span data-stu-id="b4da5-600">hello relative path of source file toosource folder is identical toohello relative path of target file tootarget folder.</span></span><br/><br/><span data-ttu-id="b4da5-601"><b>FlattenHierarchy</b>: összes fájl hello forrásmappából a hello első szintű tároló mappa.</span><span class="sxs-lookup"><span data-stu-id="b4da5-601"><b>FlattenHierarchy</b>: all files from hello source folder are in hello first level of target folder.</span></span> <span data-ttu-id="b4da5-602">hello fájljaira automatikusan létrehozott nevet adni.</span><span class="sxs-lookup"><span data-stu-id="b4da5-602">hello target files have auto generated name.</span></span> <br/><br/><span data-ttu-id="b4da5-603"><b>Mergefiles típusú (alapértelmezett):</b> forrásfájlból hello mappa tooone minden fájl egyesíti.</span><span class="sxs-lookup"><span data-stu-id="b4da5-603"><b>MergeFiles (default):</b> merges all files from hello source folder tooone file.</span></span> <span data-ttu-id="b4da5-604">Ha hello fájl/Blob neve meg van adva, egyesített hello neve legyen hello megadott név; Ellenkező esetben lenne automatikusan létrehozott fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-604">If hello File/Blob Name is specified, hello merged file name would be hello specified name; otherwise, would be auto-generated file name.</span></span> |<span data-ttu-id="b4da5-605">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-605">No</span></span> |

#### <a name="example-blobsink"></a><span data-ttu-id="b4da5-606">Példa: BlobSink</span><span class="sxs-lookup"><span data-stu-id="b4da5-606">Example: BlobSink</span></span>

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

<span data-ttu-id="b4da5-607">További információkért lásd: [Azure Blob összekötő](data-factory-azure-blob-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-607">For more information, see [Azure Blob connector](data-factory-azure-blob-connector.md#copy-activity-properties) article.</span></span> 

## <a name="azure-data-lake-store"></a><span data-ttu-id="b4da5-608">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="b4da5-608">Azure Data Lake Store</span></span>

### <a name="linked-service"></a><span data-ttu-id="b4da5-609">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-609">Linked service</span></span>
<span data-ttu-id="b4da5-610">egy Azure Data Lake Store kapcsolódó szolgáltatás toodefine, hello beállítása hello típusú társított szolgáltatás túl**AzureDataLakeStore**, és adja meg a következő tulajdonságok hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-610">toodefine an Azure Data Lake Store linked service, set hello type of hello linked service too**AzureDataLakeStore**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="b4da5-611">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-611">Property</span></span> | <span data-ttu-id="b4da5-612">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-612">Description</span></span> | <span data-ttu-id="b4da5-613">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-613">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="b4da5-614">type</span><span class="sxs-lookup"><span data-stu-id="b4da5-614">type</span></span> | <span data-ttu-id="b4da5-615">hello type tulajdonságot kell beállítani: **AzureDataLakeStore**</span><span class="sxs-lookup"><span data-stu-id="b4da5-615">hello type property must be set to: **AzureDataLakeStore**</span></span> | <span data-ttu-id="b4da5-616">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-616">Yes</span></span> |
| <span data-ttu-id="b4da5-617">dataLakeStoreUri</span><span class="sxs-lookup"><span data-stu-id="b4da5-617">dataLakeStoreUri</span></span> | <span data-ttu-id="b4da5-618">Hello Azure Data Lake Store-fiók adatainak megadása.</span><span class="sxs-lookup"><span data-stu-id="b4da5-618">Specify information about hello Azure Data Lake Store account.</span></span> <span data-ttu-id="b4da5-619">Az hello a következő formátumban: `https://[accountname].azuredatalakestore.net/webhdfs/v1` vagy `adl://[accountname].azuredatalakestore.net/`.</span><span class="sxs-lookup"><span data-stu-id="b4da5-619">It is in hello following format: `https://[accountname].azuredatalakestore.net/webhdfs/v1` or `adl://[accountname].azuredatalakestore.net/`.</span></span> | <span data-ttu-id="b4da5-620">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-620">Yes</span></span> |
| <span data-ttu-id="b4da5-621">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="b4da5-621">subscriptionId</span></span> | <span data-ttu-id="b4da5-622">Azure-előfizetés azonosítója toowhich Data Lake Store tartozik.</span><span class="sxs-lookup"><span data-stu-id="b4da5-622">Azure subscription Id toowhich Data Lake Store belongs.</span></span> | <span data-ttu-id="b4da5-623">A fogadó szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-623">Required for sink</span></span> |
| <span data-ttu-id="b4da5-624">erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="b4da5-624">resourceGroupName</span></span> | <span data-ttu-id="b4da5-625">Azure-erőforrás csoport neve toowhich Data Lake Store tartozik.</span><span class="sxs-lookup"><span data-stu-id="b4da5-625">Azure resource group name toowhich Data Lake Store belongs.</span></span> | <span data-ttu-id="b4da5-626">A fogadó szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-626">Required for sink</span></span> |
| <span data-ttu-id="b4da5-627">servicePrincipalId</span><span class="sxs-lookup"><span data-stu-id="b4da5-627">servicePrincipalId</span></span> | <span data-ttu-id="b4da5-628">Adja meg a hello alkalmazás ügyfél-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="b4da5-628">Specify hello application's client ID.</span></span> | <span data-ttu-id="b4da5-629">Igen (a szolgáltatás egyszerű hitelesítés)</span><span class="sxs-lookup"><span data-stu-id="b4da5-629">Yes (for service principal authentication)</span></span> |
| <span data-ttu-id="b4da5-630">servicePrincipalKey</span><span class="sxs-lookup"><span data-stu-id="b4da5-630">servicePrincipalKey</span></span> | <span data-ttu-id="b4da5-631">Adja meg a hello kulcsát.</span><span class="sxs-lookup"><span data-stu-id="b4da5-631">Specify hello application's key.</span></span> | <span data-ttu-id="b4da5-632">Igen (a szolgáltatás egyszerű hitelesítés)</span><span class="sxs-lookup"><span data-stu-id="b4da5-632">Yes (for service principal authentication)</span></span> |
| <span data-ttu-id="b4da5-633">Bérlői</span><span class="sxs-lookup"><span data-stu-id="b4da5-633">tenant</span></span> | <span data-ttu-id="b4da5-634">Adja meg a hello bérlői adatokat (tartomány nevét vagy a bérlő azonosító) alatt az alkalmazás található.</span><span class="sxs-lookup"><span data-stu-id="b4da5-634">Specify hello tenant information (domain name or tenant ID) under which your application resides.</span></span> <span data-ttu-id="b4da5-635">Ez által rámutató hello egér hello Azure-portál jobb felső sarkában hello kérheti le.</span><span class="sxs-lookup"><span data-stu-id="b4da5-635">You can retrieve it by hovering hello mouse in hello top-right corner of hello Azure portal.</span></span> | <span data-ttu-id="b4da5-636">Igen (a szolgáltatás egyszerű hitelesítés)</span><span class="sxs-lookup"><span data-stu-id="b4da5-636">Yes (for service principal authentication)</span></span> |
| <span data-ttu-id="b4da5-637">Engedélyezési</span><span class="sxs-lookup"><span data-stu-id="b4da5-637">authorization</span></span> | <span data-ttu-id="b4da5-638">Kattintson a **engedélyezés** hello gombjára **Data Factory Editor** , és írja be a hitelesítő adat, amelyet hozzárendel hello automatikusan létrehozott engedélyezési URL-cím toothis tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b4da5-638">Click **Authorize** button in hello **Data Factory Editor** and enter your credential that assigns hello auto-generated authorization URL toothis property.</span></span> | <span data-ttu-id="b4da5-639">Igen (a felhasználói hitelesítő)</span><span class="sxs-lookup"><span data-stu-id="b4da5-639">Yes (for user credential authentication)</span></span>|
| <span data-ttu-id="b4da5-640">Munkamenet-azonosító</span><span class="sxs-lookup"><span data-stu-id="b4da5-640">sessionId</span></span> | <span data-ttu-id="b4da5-641">OAuth munkamenet-azonosító hello OAuth hitelesítési munkamenetből.</span><span class="sxs-lookup"><span data-stu-id="b4da5-641">OAuth session id from hello OAuth authorization session.</span></span> <span data-ttu-id="b4da5-642">Minden munkamenet-azonosító egyedi, és előfordulhat, hogy csak egyszer használható.</span><span class="sxs-lookup"><span data-stu-id="b4da5-642">Each session id is unique and may only be used once.</span></span> <span data-ttu-id="b4da5-643">Ez a beállítás automatikusan létrejön a Data Factory Editor használatakor.</span><span class="sxs-lookup"><span data-stu-id="b4da5-643">This setting is automatically generated when you use Data Factory Editor.</span></span> | <span data-ttu-id="b4da5-644">Igen (a felhasználói hitelesítő)</span><span class="sxs-lookup"><span data-stu-id="b4da5-644">Yes (for user credential authentication)</span></span> |

#### <a name="example-using-service-principal-authentication"></a><span data-ttu-id="b4da5-645">Példa: a szolgáltatás egyszerű hitelesítést használó</span><span class="sxs-lookup"><span data-stu-id="b4da5-645">Example: using service principal authentication</span></span>
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

#### <a name="example-using-user-credential-authentication"></a><span data-ttu-id="b4da5-646">Példa: a felhasználói hitelesítő adatok hitelesítést használnak</span><span class="sxs-lookup"><span data-stu-id="b4da5-646">Example: using user credential authentication</span></span>
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

<span data-ttu-id="b4da5-647">További információkért lásd: [Azure Data Lake Store összekötő](data-factory-azure-datalake-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-647">For more information, see [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="b4da5-648">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-648">Dataset</span></span>
<span data-ttu-id="b4da5-649">toodefine egy Azure Data Lake Store adatkészlet set hello **típus** hello adatkészlet túl**AzureDataLakeStore**, és adja meg a következő tulajdonságai a hello hello **typeProperties**szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-649">toodefine an Azure Data Lake Store dataset, set hello **type** of hello dataset too**AzureDataLakeStore**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="b4da5-650">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-650">Property</span></span> | <span data-ttu-id="b4da5-651">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-651">Description</span></span> | <span data-ttu-id="b4da5-652">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-652">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="b4da5-653">folderPath</span><span class="sxs-lookup"><span data-stu-id="b4da5-653">folderPath</span></span> |<span data-ttu-id="b4da5-654">Elérési út toohello tároló és mappa hello Azure Data Lake tárolja.</span><span class="sxs-lookup"><span data-stu-id="b4da5-654">Path toohello container and folder in hello Azure Data Lake store.</span></span> |<span data-ttu-id="b4da5-655">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-655">Yes</span></span> |
| <span data-ttu-id="b4da5-656">fileName</span><span class="sxs-lookup"><span data-stu-id="b4da5-656">fileName</span></span> |<span data-ttu-id="b4da5-657">Hello fájl hello Azure Data Lake store nevét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-657">Name of hello file in hello Azure Data Lake store.</span></span> <span data-ttu-id="b4da5-658">Fájlnév nem, kötelező, és a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="b4da5-658">fileName is optional and case-sensitive.</span></span> <br/><br/><span data-ttu-id="b4da5-659">Ha megad egy fájlnevet, hello tevékenység (például a Másolás) működik-e az hello adott fájlt.</span><span class="sxs-lookup"><span data-stu-id="b4da5-659">If you specify a filename, hello activity (including Copy) works on hello specific file.</span></span><br/><br/><span data-ttu-id="b4da5-660">Ha nincs megadva fájlnév, másolása hello folderPath bemeneti adatkészlet összes fájl tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b4da5-660">When fileName is not specified, Copy includes all files in hello folderPath for input dataset.</span></span><br/><br/><span data-ttu-id="b4da5-661">Ha nincs megadva fájlnév egy kimeneti adatkészletet, hello hello létrehozott fájl lenne a következő formátumban hello: adatok. <Guid>.txt (például:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="b4da5-661">When fileName is not specified for an output dataset, hello name of hello generated file would be in hello following this format: Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="b4da5-662">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-662">No</span></span> |
| <span data-ttu-id="b4da5-663">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="b4da5-663">partitionedBy</span></span> |<span data-ttu-id="b4da5-664">partitionedBy egy nem kötelező tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b4da5-664">partitionedBy is an optional property.</span></span> <span data-ttu-id="b4da5-665">Használhatja a dinamikus folderPath toospecify és a fájlnév idő adatsorozat adatok.</span><span class="sxs-lookup"><span data-stu-id="b4da5-665">You can use it toospecify a dynamic folderPath and filename for time series data.</span></span> <span data-ttu-id="b4da5-666">Például folderPath is paraméteres adatok óránkénti.</span><span class="sxs-lookup"><span data-stu-id="b4da5-666">For example, folderPath can be parameterized for every hour of data.</span></span> |<span data-ttu-id="b4da5-667">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-667">No</span></span> |
| <span data-ttu-id="b4da5-668">Formátumban</span><span class="sxs-lookup"><span data-stu-id="b4da5-668">format</span></span> | <span data-ttu-id="b4da5-669">a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-669">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="b4da5-670">Set hello **típus** tulajdonság alapján formátum tooone ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="b4da5-670">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="b4da5-671">További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="b4da5-671">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="b4da5-672">Ha azt szeretné, túl**másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a hello formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban.</span><span class="sxs-lookup"><span data-stu-id="b4da5-672">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="b4da5-673">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-673">No</span></span> |
| <span data-ttu-id="b4da5-674">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="b4da5-674">compression</span></span> | <span data-ttu-id="b4da5-675">Adja meg a hello típusát és hello adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-675">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="b4da5-676">Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-676">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="b4da5-677">Támogatott szintek a következők: **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-677">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="b4da5-678">További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="b4da5-678">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="b4da5-679">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-679">No</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-680">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-680">Example</span></span>
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

<span data-ttu-id="b4da5-681">További információkért lásd: [Azure Data Lake Store összekötő](data-factory-azure-datalake-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-681">For more information, see [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#dataset-properties) article.</span></span> 

### <a name="azure-data-lake-store-source-in-copy-activity"></a><span data-ttu-id="b4da5-682">A másolási tevékenység az Azure Data Lake Store-forrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-682">Azure Data Lake Store Source in Copy Activity</span></span>
<span data-ttu-id="b4da5-683">Adatok másolása az Azure Data Lake Store a, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**AzureDataLakeStoreSource**, és adja meg a következő tulajdonságok hello **forrás**  szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-683">If you are copying data from an Azure Data Lake Store, set hello **source type** of hello copy activity too**AzureDataLakeStoreSource**, and specify following properties in hello **source** section:</span></span>

<span data-ttu-id="b4da5-684">**AzureDataLakeStoreSource** támogatja a következő tulajdonságai hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-684">**AzureDataLakeStoreSource** supports hello following properties **typeProperties** section:</span></span>

| <span data-ttu-id="b4da5-685">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-685">Property</span></span> | <span data-ttu-id="b4da5-686">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-686">Description</span></span> | <span data-ttu-id="b4da5-687">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-687">Allowed values</span></span> | <span data-ttu-id="b4da5-688">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-688">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-689">Rekurzív</span><span class="sxs-lookup"><span data-stu-id="b4da5-689">recursive</span></span> |<span data-ttu-id="b4da5-690">Azt jelzi, hogy hello adatolvasás rekurzív módon hello almappák vagy csak a megadott mappa hello.</span><span class="sxs-lookup"><span data-stu-id="b4da5-690">Indicates whether hello data is read recursively from hello sub folders or only from hello specified folder.</span></span> |<span data-ttu-id="b4da5-691">TRUE hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="b4da5-691">True (default value), False</span></span> |<span data-ttu-id="b4da5-692">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-692">No</span></span> |

#### <a name="example-azuredatalakestoresource"></a><span data-ttu-id="b4da5-693">Példa: AzureDataLakeStoreSource</span><span class="sxs-lookup"><span data-stu-id="b4da5-693">Example: AzureDataLakeStoreSource</span></span>

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

<span data-ttu-id="b4da5-694">További információkért lásd: [Azure Data Lake Store összekötő](data-factory-azure-datalake-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-694">For more information, see [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#copy-activity-properties) article.</span></span>

### <a name="azure-data-lake-store-sink-in-copy-activity"></a><span data-ttu-id="b4da5-695">A másolási tevékenység az Azure Data Lake Store fogadó</span><span class="sxs-lookup"><span data-stu-id="b4da5-695">Azure Data Lake Store Sink in Copy Activity</span></span>
<span data-ttu-id="b4da5-696">Adatok tooan Azure Data Lake Store másolása, állítsa be a hello **típus gyűjtése** hello a másolási tevékenység túl**AzureDataLakeStoreSink**, és adja meg a következő tulajdonságok hello **fogadó**szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-696">If you are copying data tooan Azure Data Lake Store, set hello **sink type** of hello copy activity too**AzureDataLakeStoreSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="b4da5-697">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-697">Property</span></span> | <span data-ttu-id="b4da5-698">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-698">Description</span></span> | <span data-ttu-id="b4da5-699">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-699">Allowed values</span></span> | <span data-ttu-id="b4da5-700">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-700">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-701">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="b4da5-701">copyBehavior</span></span> |<span data-ttu-id="b4da5-702">Meghatározza a hello másolási viselkedését.</span><span class="sxs-lookup"><span data-stu-id="b4da5-702">Specifies hello copy behavior.</span></span> |<span data-ttu-id="b4da5-703"><b>PreserveHierarchy</b>: megtartja hello fájl hierarchia hello célmappában.</span><span class="sxs-lookup"><span data-stu-id="b4da5-703"><b>PreserveHierarchy</b>: preserves hello file hierarchy in hello target folder.</span></span> <span data-ttu-id="b4da5-704">hello relatív fájl toosource forrásmappa elérési út azonos toohello fájl tootarget célmappa relatív elérési útját.</span><span class="sxs-lookup"><span data-stu-id="b4da5-704">hello relative path of source file toosource folder is identical toohello relative path of target file tootarget folder.</span></span><br/><br/><span data-ttu-id="b4da5-705"><b>FlattenHierarchy</b>: hello forrásmappából minden fájl első szintű hello célmappában jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="b4da5-705"><b>FlattenHierarchy</b>: all files from hello source folder are created in hello first level of target folder.</span></span> <span data-ttu-id="b4da5-706">hello fájljaira jönnek létre automatikusan létrehozott névvel.</span><span class="sxs-lookup"><span data-stu-id="b4da5-706">hello target files are created with auto generated name.</span></span><br/><br/><span data-ttu-id="b4da5-707"><b>Mergefiles típusú</b>: összes fájl forrásfájlból hello mappa tooone egyesíti.</span><span class="sxs-lookup"><span data-stu-id="b4da5-707"><b>MergeFiles</b>: merges all files from hello source folder tooone file.</span></span> <span data-ttu-id="b4da5-708">Ha hello fájl/Blob neve meg van adva, egyesített hello neve legyen hello megadott név; Ellenkező esetben lenne automatikusan létrehozott fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-708">If hello File/Blob Name is specified, hello merged file name would be hello specified name; otherwise, would be auto-generated file name.</span></span> |<span data-ttu-id="b4da5-709">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-709">No</span></span> |

#### <a name="example-azuredatalakestoresink"></a><span data-ttu-id="b4da5-710">Példa: AzureDataLakeStoreSink</span><span class="sxs-lookup"><span data-stu-id="b4da5-710">Example: AzureDataLakeStoreSink</span></span>
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

<span data-ttu-id="b4da5-711">További információkért lásd: [Azure Data Lake Store összekötő](data-factory-azure-datalake-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-711">For more information, see [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#copy-activity-properties) article.</span></span> 

## <a name="azure-cosmos-db"></a><span data-ttu-id="b4da5-712">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="b4da5-712">Azure Cosmos DB</span></span>  

### <a name="linked-service"></a><span data-ttu-id="b4da5-713">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-713">Linked service</span></span>
<span data-ttu-id="b4da5-714">egy Azure Cosmos DB toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**DocumentDb**, és adja meg a következő tulajdonságok hello **typeProperties** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-714">toodefine an Azure Cosmos DB linked service, set hello **type** of hello linked service too**DocumentDb**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="b4da5-715">**Tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="b4da5-715">**Property**</span></span> | <span data-ttu-id="b4da5-716">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="b4da5-716">**Description**</span></span> | <span data-ttu-id="b4da5-717">**Szükséges**</span><span class="sxs-lookup"><span data-stu-id="b4da5-717">**Required**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-718">connectionString</span><span class="sxs-lookup"><span data-stu-id="b4da5-718">connectionString</span></span> |<span data-ttu-id="b4da5-719">Adja meg a szükséges adatok tooconnect tooAzure Cosmos DB adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="b4da5-719">Specify information needed tooconnect tooAzure Cosmos DB database.</span></span> |<span data-ttu-id="b4da5-720">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-720">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-721">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-721">Example</span></span>

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
<span data-ttu-id="b4da5-722">További információkért lásd: [Azure Cosmos DB összekötő](data-factory-azure-documentdb-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-722">For more information, see [Azure Cosmos DB connector](data-factory-azure-documentdb-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="b4da5-723">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-723">Dataset</span></span>
<span data-ttu-id="b4da5-724">toodefine egy Azure Cosmos DB adatkészlet set hello **típus** hello adatkészlet túl**DocumentDbCollection**, és adja meg a következő tulajdonságai a hello hello **typeProperties** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-724">toodefine an Azure Cosmos DB dataset, set hello **type** of hello dataset too**DocumentDbCollection**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="b4da5-725">**Tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="b4da5-725">**Property**</span></span> | <span data-ttu-id="b4da5-726">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="b4da5-726">**Description**</span></span> | <span data-ttu-id="b4da5-727">**Szükséges**</span><span class="sxs-lookup"><span data-stu-id="b4da5-727">**Required**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-728">CollectionName</span><span class="sxs-lookup"><span data-stu-id="b4da5-728">collectionName</span></span> |<span data-ttu-id="b4da5-729">Hello Azure Cosmos DB gyűjtemény neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-729">Name of hello Azure Cosmos DB collection.</span></span> |<span data-ttu-id="b4da5-730">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-730">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-731">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-731">Example</span></span>

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
<span data-ttu-id="b4da5-732">További információkért lásd: [Azure Cosmos DB összekötő](data-factory-azure-documentdb-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-732">For more information, see [Azure Cosmos DB connector](data-factory-azure-documentdb-connector.md#dataset-properties) article.</span></span>

### <a name="azure-cosmos-db-collection-source-in-copy-activity"></a><span data-ttu-id="b4da5-733">A másolási tevékenység Azure Cosmos DB gyűjtemény forrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-733">Azure Cosmos DB Collection Source in Copy Activity</span></span>
<span data-ttu-id="b4da5-734">Adatok másolása az Azure Cosmos adatbázisából, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**DocumentDbCollectionSource**, és adja meg a következő tulajdonságok hello **forrás** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-734">If you are copying data from an Azure Cosmos DB, set hello **source type** of hello copy activity too**DocumentDbCollectionSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="b4da5-735">**Tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="b4da5-735">**Property**</span></span> | <span data-ttu-id="b4da5-736">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="b4da5-736">**Description**</span></span> | <span data-ttu-id="b4da5-737">**Megengedett értékek**</span><span class="sxs-lookup"><span data-stu-id="b4da5-737">**Allowed values**</span></span> | <span data-ttu-id="b4da5-738">**Szükséges**</span><span class="sxs-lookup"><span data-stu-id="b4da5-738">**Required**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-739">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="b4da5-739">query</span></span> |<span data-ttu-id="b4da5-740">Adja meg a hello tooread adatait kérdezi le.</span><span class="sxs-lookup"><span data-stu-id="b4da5-740">Specify hello query tooread data.</span></span> |<span data-ttu-id="b4da5-741">Lekérdezés-karakterlánc hossza Azure Cosmos DB által támogatott.</span><span class="sxs-lookup"><span data-stu-id="b4da5-741">Query string supported by Azure Cosmos DB.</span></span> <br/><br/><span data-ttu-id="b4da5-742">Példa:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"`</span><span class="sxs-lookup"><span data-stu-id="b4da5-742">Example: `SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"`</span></span> |<span data-ttu-id="b4da5-743">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-743">No</span></span> <br/><br/><span data-ttu-id="b4da5-744">Ha nincs megadva, hello végrehajtott SQL-utasítást:`select <columns defined in structure> from mycollection`</span><span class="sxs-lookup"><span data-stu-id="b4da5-744">If not specified, hello SQL statement that is executed: `select <columns defined in structure> from mycollection`</span></span> |
| <span data-ttu-id="b4da5-745">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="b4da5-745">nestingSeparator</span></span> |<span data-ttu-id="b4da5-746">Speciális karakter tooindicate, hogy a dokumentum hello van beágyazva.</span><span class="sxs-lookup"><span data-stu-id="b4da5-746">Special character tooindicate that hello document is nested</span></span> |<span data-ttu-id="b4da5-747">Bármely karakter.</span><span class="sxs-lookup"><span data-stu-id="b4da5-747">Any character.</span></span> <br/><br/><span data-ttu-id="b4da5-748">Azure Cosmos-adatbázis egy NoSQL-tároló JSON-dokumentumok, amelyben beágyazott struktúrákat engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="b4da5-748">Azure Cosmos DB is a NoSQL store for JSON documents, where nested structures are allowed.</span></span> <span data-ttu-id="b4da5-749">Az Azure Data Factory lehetővé teszi, hogy a felhasználó toodenote hierarchia keresztül nestingSeparator, amely "."</span><span class="sxs-lookup"><span data-stu-id="b4da5-749">Azure Data Factory enables user toodenote hierarchy via nestingSeparator, which is “.”</span></span> <span data-ttu-id="b4da5-750">a fenti példák hello.</span><span class="sxs-lookup"><span data-stu-id="b4da5-750">in hello above examples.</span></span> <span data-ttu-id="b4da5-751">Hello elválasztóval hello másolási tevékenység hello "Name" objektum három gyermekek elemekkel hozza létre első, középső és utolsó, függően too"Name.First", "Name.Middle" és "Name.Last" hello a tábla megadása.</span><span class="sxs-lookup"><span data-stu-id="b4da5-751">With hello separator, hello copy activity will generate hello “Name” object with three children elements First, Middle and Last, according too“Name.First”, “Name.Middle” and “Name.Last” in hello table definition.</span></span> |<span data-ttu-id="b4da5-752">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-752">No</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-753">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-753">Example</span></span>

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

### <a name="azure-cosmos-db-collection-sink-in-copy-activity"></a><span data-ttu-id="b4da5-754">Az Azure Cosmos DB gyűjtemény fogadó a másolási tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4da5-754">Azure Cosmos DB Collection Sink in Copy Activity</span></span>
<span data-ttu-id="b4da5-755">Adatok tooAzure Cosmos DB másolása, állítsa be a hello **típus gyűjtése** hello a másolási tevékenység túl**DocumentDbCollectionSink**, és adja meg a következő tulajdonságok hello **fogadó**szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-755">If you are copying data tooAzure Cosmos DB, set hello **sink type** of hello copy activity too**DocumentDbCollectionSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="b4da5-756">**Tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="b4da5-756">**Property**</span></span> | <span data-ttu-id="b4da5-757">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="b4da5-757">**Description**</span></span> | <span data-ttu-id="b4da5-758">**Megengedett értékek**</span><span class="sxs-lookup"><span data-stu-id="b4da5-758">**Allowed values**</span></span> | <span data-ttu-id="b4da5-759">**Szükséges**</span><span class="sxs-lookup"><span data-stu-id="b4da5-759">**Required**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-760">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="b4da5-760">nestingSeparator</span></span> |<span data-ttu-id="b4da5-761">A különleges karakterek hello forrás oszlop neve tooindicate, amely a beágyazott dokumentumok van szükség.</span><span class="sxs-lookup"><span data-stu-id="b4da5-761">A special character in hello source column name tooindicate that nested document is needed.</span></span> <br/><br/><span data-ttu-id="b4da5-762">Például fent: `Name.First` hello kimeneti táblát hoz létre hello JSON struktúrában hello Cosmos DB dokumentumban a következő:</span><span class="sxs-lookup"><span data-stu-id="b4da5-762">For example above: `Name.First` in hello output table produces hello following JSON structure in hello Cosmos DB document:</span></span><br/><br/><span data-ttu-id="b4da5-763">"Name": {</span><span class="sxs-lookup"><span data-stu-id="b4da5-763">"Name": {</span></span><br/>    <span data-ttu-id="b4da5-764">"Első": "John"</span><span class="sxs-lookup"><span data-stu-id="b4da5-764">"First": "John"</span></span><br/><span data-ttu-id="b4da5-765">},</span><span class="sxs-lookup"><span data-stu-id="b4da5-765">},</span></span> |<span data-ttu-id="b4da5-766">Az karakter, amely használt tooseparate beágyazási szinttel.</span><span class="sxs-lookup"><span data-stu-id="b4da5-766">Character that is used tooseparate nesting levels.</span></span><br/><br/><span data-ttu-id="b4da5-767">Alapértelmezett érték `.` (pont).</span><span class="sxs-lookup"><span data-stu-id="b4da5-767">Default value is `.` (dot).</span></span> |<span data-ttu-id="b4da5-768">Az karakter, amely használt tooseparate beágyazási szinttel.</span><span class="sxs-lookup"><span data-stu-id="b4da5-768">Character that is used tooseparate nesting levels.</span></span> <br/><br/><span data-ttu-id="b4da5-769">Alapértelmezett érték `.` (pont).</span><span class="sxs-lookup"><span data-stu-id="b4da5-769">Default value is `.` (dot).</span></span> |
| <span data-ttu-id="b4da5-770">WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="b4da5-770">writeBatchSize</span></span> |<span data-ttu-id="b4da5-771">A lekérdezések tooAzure Cosmos DB toocreate dokumentumok párhuzamos száma.</span><span class="sxs-lookup"><span data-stu-id="b4da5-771">Number of parallel requests tooAzure Cosmos DB service toocreate documents.</span></span><br/><br/><span data-ttu-id="b4da5-772">Hello teljesítmény úgy finomhangolhatja, e tulajdonság használatával vagy az Azure Cosmos Adatbázisból adatok másolásakor.</span><span class="sxs-lookup"><span data-stu-id="b4da5-772">You can fine-tune hello performance when copying data to/from Azure Cosmos DB by using this property.</span></span> <span data-ttu-id="b4da5-773">A jobb teljesítmény számíthat, mivel a rendszer további kérelmeket párhuzamos tooAzure Cosmos DB elküldi a writeBatchSize növelésével.</span><span class="sxs-lookup"><span data-stu-id="b4da5-773">You can expect a better performance when you increase writeBatchSize because more parallel requests tooAzure Cosmos DB are sent.</span></span> <span data-ttu-id="b4da5-774">Azonban szüksége lesz, amelyek sávszélesség-szabályozás tooavoid is throw hello hibaüzenet: "Ez nagy lekérő".</span><span class="sxs-lookup"><span data-stu-id="b4da5-774">However you’ll need tooavoid throttling that can throw hello error message: "Request rate is large".</span></span><br/><br/><span data-ttu-id="b4da5-775">Sávszélesség-szabályozás tényező, beleértve a dokumentumok, a dokumentumok számát méretét, indexelő házirend célgyűjteményt stb határoz meg. A másolási műveletek, használhat egy jobb gyűjtemény (például S3) toohave hello legtöbb átviteli sebesség érhető el (2500 kérés egység/másodperc).</span><span class="sxs-lookup"><span data-stu-id="b4da5-775">Throttling is decided by a number of factors, including size of documents, number of terms in documents, indexing policy of target collection, etc. For copy operations, you can use a better collection (for example, S3) toohave hello most throughput available (2,500 request units/second).</span></span> |<span data-ttu-id="b4da5-776">Egész szám</span><span class="sxs-lookup"><span data-stu-id="b4da5-776">Integer</span></span> |<span data-ttu-id="b4da5-777">Nem (alapértelmezett: 5)</span><span class="sxs-lookup"><span data-stu-id="b4da5-777">No (default: 5)</span></span> |
| <span data-ttu-id="b4da5-778">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="b4da5-778">writeBatchTimeout</span></span> |<span data-ttu-id="b4da5-779">Várnia kell az hello művelet toocomplete előtt azt az időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="b4da5-779">Wait time for hello operation toocomplete before it times out.</span></span> |<span data-ttu-id="b4da5-780">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="b4da5-780">timespan</span></span><br/><br/> <span data-ttu-id="b4da5-781">Példa: "00: 30:00" (30 perc).</span><span class="sxs-lookup"><span data-stu-id="b4da5-781">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="b4da5-782">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-782">No</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-783">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-783">Example</span></span>

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
                    "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, title: aaaTitle, Suffix: Suffix"
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

<span data-ttu-id="b4da5-784">További információkért lásd: [Azure Cosmos DB összekötő](data-factory-azure-documentdb-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-784">For more information, see [Azure Cosmos DB connector](data-factory-azure-documentdb-connector.md#copy-activity-properties) article.</span></span>

## <a name="azure-sql-database"></a><span data-ttu-id="b4da5-785">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="b4da5-785">Azure SQL Database</span></span>

### <a name="linked-service"></a><span data-ttu-id="b4da5-786">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-786">Linked service</span></span>
<span data-ttu-id="b4da5-787">egy Azure SQL Database toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**AzureSqlDatabase**, és adja meg a következő tulajdonságok hello **typeProperties**szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-787">toodefine an Azure SQL Database linked service, set hello **type** of hello linked service too**AzureSqlDatabase**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="b4da5-788">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-788">Property</span></span> | <span data-ttu-id="b4da5-789">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-789">Description</span></span> | <span data-ttu-id="b4da5-790">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-790">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-791">connectionString</span><span class="sxs-lookup"><span data-stu-id="b4da5-791">connectionString</span></span> |<span data-ttu-id="b4da5-792">Adjon meg információt hello connectionString tulajdonság szükséges tooconnect toohello Azure SQL Database-példányt.</span><span class="sxs-lookup"><span data-stu-id="b4da5-792">Specify information needed tooconnect toohello Azure SQL Database instance for hello connectionString property.</span></span> |<span data-ttu-id="b4da5-793">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-793">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-794">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-794">Example</span></span>
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

<span data-ttu-id="b4da5-795">További információkért lásd: [Azure SQL-összekötő](data-factory-azure-sql-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-795">For more information, see [Azure SQL connector](data-factory-azure-sql-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="b4da5-796">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-796">Dataset</span></span>
<span data-ttu-id="b4da5-797">toodefine egy Azure SQL Database adatkészlet set hello **típus** hello adatkészlet túl**AzureSqlTable**, és adja meg a következő tulajdonságai a hello hello **typeProperties** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-797">toodefine an Azure SQL Database dataset, set hello **type** of hello dataset too**AzureSqlTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="b4da5-798">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-798">Property</span></span> | <span data-ttu-id="b4da5-799">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-799">Description</span></span> | <span data-ttu-id="b4da5-800">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-800">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-801">tableName</span><span class="sxs-lookup"><span data-stu-id="b4da5-801">tableName</span></span> |<span data-ttu-id="b4da5-802">Hello tábla vagy nézet hello Azure SQL Database-példányt, amelyre a társított szolgáltatás neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="b4da5-802">Name of hello table or view in hello Azure SQL Database instance that linked service refers to.</span></span> |<span data-ttu-id="b4da5-803">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-803">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-804">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-804">Example</span></span>

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
<span data-ttu-id="b4da5-805">További információkért lásd: [Azure SQL-összekötő](data-factory-azure-sql-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-805">For more information, see [Azure SQL connector](data-factory-azure-sql-connector.md#dataset-properties) article.</span></span> 

### <a name="sql-source-in-copy-activity"></a><span data-ttu-id="b4da5-806">A másolási tevékenység SQL-forrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-806">SQL Source in Copy Activity</span></span>
<span data-ttu-id="b4da5-807">Adatok másolása az Azure SQL-adatbázisból, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**SqlSource**, és adja meg a következő tulajdonságok hello **forrás** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-807">If you are copying data from an Azure SQL Database, set hello **source type** of hello copy activity too**SqlSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="b4da5-808">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-808">Property</span></span> | <span data-ttu-id="b4da5-809">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-809">Description</span></span> | <span data-ttu-id="b4da5-810">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-810">Allowed values</span></span> | <span data-ttu-id="b4da5-811">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-811">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-812">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="b4da5-812">sqlReaderQuery</span></span> |<span data-ttu-id="b4da5-813">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="b4da5-813">Use hello custom query tooread data.</span></span> |<span data-ttu-id="b4da5-814">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="b4da5-814">SQL query string.</span></span> <span data-ttu-id="b4da5-815">Példa: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="b4da5-815">Example: `select * from MyTable`.</span></span> |<span data-ttu-id="b4da5-816">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-816">No</span></span> |
| <span data-ttu-id="b4da5-817">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="b4da5-817">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="b4da5-818">Hello neve tárolt eljárást, amely hello forrástábla olvassa be az adatokat.</span><span class="sxs-lookup"><span data-stu-id="b4da5-818">Name of hello stored procedure that reads data from hello source table.</span></span> |<span data-ttu-id="b4da5-819">Hello neve tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="b4da5-819">Name of hello stored procedure.</span></span> |<span data-ttu-id="b4da5-820">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-820">No</span></span> |
| <span data-ttu-id="b4da5-821">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="b4da5-821">storedProcedureParameters</span></span> |<span data-ttu-id="b4da5-822">Hello paramétereinek tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="b4da5-822">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="b4da5-823">A név/érték párok.</span><span class="sxs-lookup"><span data-stu-id="b4da5-823">Name/value pairs.</span></span> <span data-ttu-id="b4da5-824">Nevek és a kis-és paraméterek meg kell egyeznie hello nevét és a kis-és nagybetűhasználat hello tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="b4da5-824">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="b4da5-825">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-825">No</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-826">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-826">Example</span></span>

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
<span data-ttu-id="b4da5-827">További információkért lásd: [Azure SQL-összekötő](data-factory-azure-sql-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-827">For more information, see [Azure SQL connector](data-factory-azure-sql-connector.md#copy-activity-properties) article.</span></span> 

### <a name="sql-sink-in-copy-activity"></a><span data-ttu-id="b4da5-828">A másolási tevékenység SQL fogadó</span><span class="sxs-lookup"><span data-stu-id="b4da5-828">SQL Sink in Copy Activity</span></span>
<span data-ttu-id="b4da5-829">Adatok tooAzure SQL-adatbázis másolása, állítsa be a hello **típus gyűjtése** hello a másolási tevékenység túl**SqlSink**, és adja meg a következő tulajdonságok hello **fogadó** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-829">If you are copying data tooAzure SQL Database, set hello **sink type** of hello copy activity too**SqlSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="b4da5-830">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-830">Property</span></span> | <span data-ttu-id="b4da5-831">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-831">Description</span></span> | <span data-ttu-id="b4da5-832">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-832">Allowed values</span></span> | <span data-ttu-id="b4da5-833">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-833">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-834">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="b4da5-834">writeBatchTimeout</span></span> |<span data-ttu-id="b4da5-835">Várnia kell az hello kötegelt beszúrási művelet toocomplete előtt azt az időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="b4da5-835">Wait time for hello batch insert operation toocomplete before it times out.</span></span> |<span data-ttu-id="b4da5-836">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="b4da5-836">timespan</span></span><br/><br/> <span data-ttu-id="b4da5-837">Példa: "00: 30:00" (30 perc).</span><span class="sxs-lookup"><span data-stu-id="b4da5-837">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="b4da5-838">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-838">No</span></span> |
| <span data-ttu-id="b4da5-839">WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="b4da5-839">writeBatchSize</span></span> |<span data-ttu-id="b4da5-840">Amikor hello puffer mérete eléri writeBatchSize adatok beillesztése hello SQL táblázat.</span><span class="sxs-lookup"><span data-stu-id="b4da5-840">Inserts data into hello SQL table when hello buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="b4da5-841">Egész szám (sorok száma)</span><span class="sxs-lookup"><span data-stu-id="b4da5-841">Integer (number of rows)</span></span> |<span data-ttu-id="b4da5-842">Nem (alapértelmezett: 10000)</span><span class="sxs-lookup"><span data-stu-id="b4da5-842">No (default: 10000)</span></span> |
| <span data-ttu-id="b4da5-843">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="b4da5-843">sqlWriterCleanupScript</span></span> |<span data-ttu-id="b4da5-844">Adja meg a másolási tevékenység tooexecute vonatkozó lekérdezést úgy, hogy egy adott szelet adatait.</span><span class="sxs-lookup"><span data-stu-id="b4da5-844">Specify a query for Copy Activity tooexecute such that data of a specific slice is cleaned up.</span></span> |<span data-ttu-id="b4da5-845">A lekérdezési utasítást.</span><span class="sxs-lookup"><span data-stu-id="b4da5-845">A query statement.</span></span> |<span data-ttu-id="b4da5-846">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-846">No</span></span> |
| <span data-ttu-id="b4da5-847">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="b4da5-847">sliceIdentifierColumnName</span></span> |<span data-ttu-id="b4da5-848">Adja meg oszlop nevét, a másolási tevékenység toofill szelet azonosító automatikusan létrejön, vagyis amikor futtassa újra a megadott szelet adatainak használt tooclean.</span><span class="sxs-lookup"><span data-stu-id="b4da5-848">Specify a column name for Copy Activity toofill with auto generated slice identifier, which is used tooclean up data of a specific slice when rerun.</span></span> |<span data-ttu-id="b4da5-849">Egy oszlop binary(32) adattípusú oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-849">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="b4da5-850">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-850">No</span></span> |
| <span data-ttu-id="b4da5-851">sqlWriterStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="b4da5-851">sqlWriterStoredProcedureName</span></span> |<span data-ttu-id="b4da5-852">Hello nevét (frissítés/Beszúrás) upserts adatok tárolt eljárás hello cél táblába.</span><span class="sxs-lookup"><span data-stu-id="b4da5-852">Name of hello stored procedure that upserts (updates/inserts) data into hello target table.</span></span> |<span data-ttu-id="b4da5-853">Hello neve tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="b4da5-853">Name of hello stored procedure.</span></span> |<span data-ttu-id="b4da5-854">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-854">No</span></span> |
| <span data-ttu-id="b4da5-855">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="b4da5-855">storedProcedureParameters</span></span> |<span data-ttu-id="b4da5-856">Hello paramétereinek tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="b4da5-856">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="b4da5-857">A név/érték párok.</span><span class="sxs-lookup"><span data-stu-id="b4da5-857">Name/value pairs.</span></span> <span data-ttu-id="b4da5-858">Nevek és a kis-és paraméterek meg kell egyeznie hello nevét és a kis-és nagybetűhasználat hello tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="b4da5-858">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="b4da5-859">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-859">No</span></span> |
| <span data-ttu-id="b4da5-860">sqlWriterTableType</span><span class="sxs-lookup"><span data-stu-id="b4da5-860">sqlWriterTableType</span></span> |<span data-ttu-id="b4da5-861">Adjon meg egy tábla Típus neve toobe hello tárolt eljárásban használt.</span><span class="sxs-lookup"><span data-stu-id="b4da5-861">Specify a table type name toobe used in hello stored procedure.</span></span> <span data-ttu-id="b4da5-862">Másolási tevékenység elérhetővé teszi hello adatok éppen áthelyezik egy ideiglenes táblát, amely a táblatípus.</span><span class="sxs-lookup"><span data-stu-id="b4da5-862">Copy activity makes hello data being moved available in a temp table with this table type.</span></span> <span data-ttu-id="b4da5-863">Tárolt eljárás kód majd egyesítheti a meglévő adatok másolásának hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="b4da5-863">Stored procedure code can then merge hello data being copied with existing data.</span></span> |<span data-ttu-id="b4da5-864">Egy tábla környezettípus nevét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-864">A table type name.</span></span> |<span data-ttu-id="b4da5-865">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-865">No</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-866">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-866">Example</span></span>

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

<span data-ttu-id="b4da5-867">További információkért lásd: [Azure SQL-összekötő](data-factory-azure-sql-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-867">For more information, see [Azure SQL connector](data-factory-azure-sql-connector.md#copy-activity-properties) article.</span></span> 

## <a name="azure-sql-data-warehouse"></a><span data-ttu-id="b4da5-868">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="b4da5-868">Azure SQL Data Warehouse</span></span>

### <a name="linked-service"></a><span data-ttu-id="b4da5-869">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-869">Linked service</span></span>
<span data-ttu-id="b4da5-870">Azure SQL Data Warehouse toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**AzureSqlDW**, és adja meg a következő tulajdonságok hello **typeProperties**szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-870">toodefine an Azure SQL Data Warehouse linked service, set hello **type** of hello linked service too**AzureSqlDW**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="b4da5-871">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-871">Property</span></span> | <span data-ttu-id="b4da5-872">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-872">Description</span></span> | <span data-ttu-id="b4da5-873">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-873">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-874">connectionString</span><span class="sxs-lookup"><span data-stu-id="b4da5-874">connectionString</span></span> |<span data-ttu-id="b4da5-875">Adjon meg információt hello connectionString tulajdonság szükséges tooconnect toohello Azure SQL Data Warehouse-példányhoz.</span><span class="sxs-lookup"><span data-stu-id="b4da5-875">Specify information needed tooconnect toohello Azure SQL Data Warehouse instance for hello connectionString property.</span></span> |<span data-ttu-id="b4da5-876">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-876">Yes</span></span> |



#### <a name="example"></a><span data-ttu-id="b4da5-877">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-877">Example</span></span>

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

<span data-ttu-id="b4da5-878">További információkért lásd: [Azure SQL Data Warehouse-összekötő](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-878">For more information, see [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="b4da5-879">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-879">Dataset</span></span>
<span data-ttu-id="b4da5-880">toodefine egy Azure SQL Data Warehouse adatkészlet set hello **típus** hello adatkészlet túl**AzureSqlDWTable**, és adja meg a következő tulajdonságai a hello hello **typeProperties**szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-880">toodefine an Azure SQL Data Warehouse dataset, set hello **type** of hello dataset too**AzureSqlDWTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="b4da5-881">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-881">Property</span></span> | <span data-ttu-id="b4da5-882">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-882">Description</span></span> | <span data-ttu-id="b4da5-883">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-883">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-884">tableName</span><span class="sxs-lookup"><span data-stu-id="b4da5-884">tableName</span></span> |<span data-ttu-id="b4da5-885">Hello tábla vagy nézet hello Azure SQL Data Warehouse-adatbázis, amely a társított szolgáltatás hello nevére hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="b4da5-885">Name of hello table or view in hello Azure SQL Data Warehouse database that hello linked service refers to.</span></span> |<span data-ttu-id="b4da5-886">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-886">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-887">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-887">Example</span></span>

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

<span data-ttu-id="b4da5-888">További információkért lásd: [Azure SQL Data Warehouse-összekötő](data-factory-azure-sql-data-warehouse-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-888">For more information, see [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#dataset-properties) article.</span></span> 

### <a name="sql-dw-source-in-copy-activity"></a><span data-ttu-id="b4da5-889">A másolási tevékenység SQL DW-forrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-889">SQL DW Source in Copy Activity</span></span>
<span data-ttu-id="b4da5-890">Adatok másolása az Azure SQL Data Warehouse, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**SqlDWSource**, és adja meg a következő tulajdonságok hello **forrás**szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-890">If you are copying data from Azure SQL Data Warehouse, set hello **source type** of hello copy activity too**SqlDWSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="b4da5-891">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-891">Property</span></span> | <span data-ttu-id="b4da5-892">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-892">Description</span></span> | <span data-ttu-id="b4da5-893">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-893">Allowed values</span></span> | <span data-ttu-id="b4da5-894">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-894">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-895">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="b4da5-895">sqlReaderQuery</span></span> |<span data-ttu-id="b4da5-896">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="b4da5-896">Use hello custom query tooread data.</span></span> |<span data-ttu-id="b4da5-897">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="b4da5-897">SQL query string.</span></span> <span data-ttu-id="b4da5-898">Például: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="b4da5-898">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="b4da5-899">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-899">No</span></span> |
| <span data-ttu-id="b4da5-900">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="b4da5-900">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="b4da5-901">Hello neve tárolt eljárást, amely hello forrástábla olvassa be az adatokat.</span><span class="sxs-lookup"><span data-stu-id="b4da5-901">Name of hello stored procedure that reads data from hello source table.</span></span> |<span data-ttu-id="b4da5-902">Hello neve tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="b4da5-902">Name of hello stored procedure.</span></span> |<span data-ttu-id="b4da5-903">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-903">No</span></span> |
| <span data-ttu-id="b4da5-904">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="b4da5-904">storedProcedureParameters</span></span> |<span data-ttu-id="b4da5-905">Hello paramétereinek tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="b4da5-905">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="b4da5-906">A név/érték párok.</span><span class="sxs-lookup"><span data-stu-id="b4da5-906">Name/value pairs.</span></span> <span data-ttu-id="b4da5-907">Nevek és a kis-és paraméterek meg kell egyeznie hello nevét és a kis-és nagybetűhasználat hello tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="b4da5-907">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="b4da5-908">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-908">No</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-909">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-909">Example</span></span>

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

<span data-ttu-id="b4da5-910">További információkért lásd: [Azure SQL Data Warehouse-összekötő](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-910">For more information, see [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) article.</span></span> 

### <a name="sql-dw-sink-in-copy-activity"></a><span data-ttu-id="b4da5-911">A másolási tevékenység SQL DW fogadó</span><span class="sxs-lookup"><span data-stu-id="b4da5-911">SQL DW Sink in Copy Activity</span></span>
<span data-ttu-id="b4da5-912">Adatok tooAzure SQL Data Warehouse másolása, állítsa be a hello **típus gyűjtése** hello a másolási tevékenység túl**SqlDWSink**, és adja meg a következő tulajdonságok hello **fogadó** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-912">If you are copying data tooAzure SQL Data Warehouse, set hello **sink type** of hello copy activity too**SqlDWSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="b4da5-913">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-913">Property</span></span> | <span data-ttu-id="b4da5-914">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-914">Description</span></span> | <span data-ttu-id="b4da5-915">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-915">Allowed values</span></span> | <span data-ttu-id="b4da5-916">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-916">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-917">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="b4da5-917">sqlWriterCleanupScript</span></span> |<span data-ttu-id="b4da5-918">Adja meg a másolási tevékenység tooexecute vonatkozó lekérdezést úgy, hogy egy adott szelet adatait.</span><span class="sxs-lookup"><span data-stu-id="b4da5-918">Specify a query for Copy Activity tooexecute such that data of a specific slice is cleaned up.</span></span> |<span data-ttu-id="b4da5-919">A lekérdezési utasítást.</span><span class="sxs-lookup"><span data-stu-id="b4da5-919">A query statement.</span></span> |<span data-ttu-id="b4da5-920">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-920">No</span></span> |
| <span data-ttu-id="b4da5-921">allowPolyBase</span><span class="sxs-lookup"><span data-stu-id="b4da5-921">allowPolyBase</span></span> |<span data-ttu-id="b4da5-922">Azt jelzi, hogy (ha alkalmazható) PolyBase toouse BULKINSERT mechanizmus helyett.</span><span class="sxs-lookup"><span data-stu-id="b4da5-922">Indicates whether toouse PolyBase (when applicable) instead of BULKINSERT mechanism.</span></span> <br/><br/> <span data-ttu-id="b4da5-923">**A PolyBase használata javasolt módja tooload adatokat az SQL Data Warehouse hello.**</span><span class="sxs-lookup"><span data-stu-id="b4da5-923">**Using PolyBase is hello recommended way tooload data into SQL Data Warehouse.**</span></span> |<span data-ttu-id="b4da5-924">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="b4da5-924">True</span></span> <br/><span data-ttu-id="b4da5-925">Hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="b4da5-925">False (default)</span></span> |<span data-ttu-id="b4da5-926">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-926">No</span></span> |
| <span data-ttu-id="b4da5-927">kapcsolódó polyBaseSettings</span><span class="sxs-lookup"><span data-stu-id="b4da5-927">polyBaseSettings</span></span> |<span data-ttu-id="b4da5-928">Egy csoport lehet megadni, ha hello tulajdonságok **allowPolybase** tulajdonsága túl**igaz**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-928">A group of properties that can be specified when hello **allowPolybase** property is set too**true**.</span></span> |&nbsp; |<span data-ttu-id="b4da5-929">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-929">No</span></span> |
| <span data-ttu-id="b4da5-930">rejectValue</span><span class="sxs-lookup"><span data-stu-id="b4da5-930">rejectValue</span></span> |<span data-ttu-id="b4da5-931">Megadja a hello számát vagy a sorok, amelyekre el lehet utasítani, mielőtt hello lekérdezés nem sikerült százalékát.</span><span class="sxs-lookup"><span data-stu-id="b4da5-931">Specifies hello number or percentage of rows that can be rejected before hello query fails.</span></span> <br/><br/><span data-ttu-id="b4da5-932">Bővebben a hello PolyBase elutasítása hello lehetőségeit további **argumentumok** szakasza [külső tábla létrehozása (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) témakör.</span><span class="sxs-lookup"><span data-stu-id="b4da5-932">Learn more about hello PolyBase’s reject options in hello **Arguments** section of [CREATE EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) topic.</span></span> |<span data-ttu-id="b4da5-933">0 (alapértelmezés), 1, 2...</span><span class="sxs-lookup"><span data-stu-id="b4da5-933">0 (default), 1, 2, …</span></span> |<span data-ttu-id="b4da5-934">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-934">No</span></span> |
| <span data-ttu-id="b4da5-935">rejectType</span><span class="sxs-lookup"><span data-stu-id="b4da5-935">rejectType</span></span> |<span data-ttu-id="b4da5-936">Meghatározza, hogy hello rejectValue beállítás konstans értéket vagy százalékában van megadva.</span><span class="sxs-lookup"><span data-stu-id="b4da5-936">Specifies whether hello rejectValue option is specified as a literal value or a percentage.</span></span> |<span data-ttu-id="b4da5-937">Érték (alapértelmezett), százalékos aránya</span><span class="sxs-lookup"><span data-stu-id="b4da5-937">Value (default), Percentage</span></span> |<span data-ttu-id="b4da5-938">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-938">No</span></span> |
| <span data-ttu-id="b4da5-939">rejectSampleValue</span><span class="sxs-lookup"><span data-stu-id="b4da5-939">rejectSampleValue</span></span> |<span data-ttu-id="b4da5-940">Határozza meg, hogy hello sorok tooretrieve előtt hello PolyBase újraszámítja a visszautasított sorok hello százalékát.</span><span class="sxs-lookup"><span data-stu-id="b4da5-940">Determines hello number of rows tooretrieve before hello PolyBase recalculates hello percentage of rejected rows.</span></span> |<span data-ttu-id="b4da5-941">1, 2, …</span><span class="sxs-lookup"><span data-stu-id="b4da5-941">1, 2, …</span></span> |<span data-ttu-id="b4da5-942">Igen, ha **rejectType** van **százalékos aránya**</span><span class="sxs-lookup"><span data-stu-id="b4da5-942">Yes, if **rejectType** is **percentage**</span></span> |
| <span data-ttu-id="b4da5-943">useTypeDefault</span><span class="sxs-lookup"><span data-stu-id="b4da5-943">useTypeDefault</span></span> |<span data-ttu-id="b4da5-944">Itt adhatja meg, hogyan értékek a hiányzó toohandle tagolt-e szövegfájlok amikor PolyBase hello szövegfájlból kér le adatokat.</span><span class="sxs-lookup"><span data-stu-id="b4da5-944">Specifies how toohandle missing values in delimited text files when PolyBase retrieves data from hello text file.</span></span><br/><br/><span data-ttu-id="b4da5-945">Ezt a tulajdonságot hello argumentumok című szakaszában olvashat [létrehozása külső FÁJLFORMÁTUM (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).</span><span class="sxs-lookup"><span data-stu-id="b4da5-945">Learn more about this property from hello Arguments section in [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).</span></span> |<span data-ttu-id="b4da5-946">IGAZ, hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="b4da5-946">True, False (default)</span></span> |<span data-ttu-id="b4da5-947">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-947">No</span></span> |
| <span data-ttu-id="b4da5-948">WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="b4da5-948">writeBatchSize</span></span> |<span data-ttu-id="b4da5-949">Adatok beszúrása hello SQL táblázatba, amikor hello puffer mérete eléri writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="b4da5-949">Inserts data into hello SQL table when hello buffer size reaches writeBatchSize</span></span> |<span data-ttu-id="b4da5-950">Egész szám (sorok száma)</span><span class="sxs-lookup"><span data-stu-id="b4da5-950">Integer (number of rows)</span></span> |<span data-ttu-id="b4da5-951">Nem (alapértelmezett: 10000)</span><span class="sxs-lookup"><span data-stu-id="b4da5-951">No (default: 10000)</span></span> |
| <span data-ttu-id="b4da5-952">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="b4da5-952">writeBatchTimeout</span></span> |<span data-ttu-id="b4da5-953">Várnia kell az hello kötegelt beszúrási művelet toocomplete előtt azt az időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="b4da5-953">Wait time for hello batch insert operation toocomplete before it times out.</span></span> |<span data-ttu-id="b4da5-954">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="b4da5-954">timespan</span></span><br/><br/> <span data-ttu-id="b4da5-955">Példa: "00: 30:00" (30 perc).</span><span class="sxs-lookup"><span data-stu-id="b4da5-955">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="b4da5-956">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-956">No</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-957">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-957">Example</span></span>

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

<span data-ttu-id="b4da5-958">További információkért lásd: [Azure SQL Data Warehouse-összekötő](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-958">For more information, see [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) article.</span></span> 

## <a name="azure-search"></a><span data-ttu-id="b4da5-959">Azure Search</span><span class="sxs-lookup"><span data-stu-id="b4da5-959">Azure Search</span></span>

### <a name="linked-service"></a><span data-ttu-id="b4da5-960">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-960">Linked service</span></span>
<span data-ttu-id="b4da5-961">az Azure Search toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**AzureSearch**, és adja meg a következő tulajdonságok hello **typeProperties** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-961">toodefine an Azure Search linked service, set hello **type** of hello linked service too**AzureSearch**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="b4da5-962">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-962">Property</span></span> | <span data-ttu-id="b4da5-963">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-963">Description</span></span> | <span data-ttu-id="b4da5-964">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-964">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="b4da5-965">URL-címe</span><span class="sxs-lookup"><span data-stu-id="b4da5-965">url</span></span> | <span data-ttu-id="b4da5-966">Hello Azure Search szolgáltatás URL-címe.</span><span class="sxs-lookup"><span data-stu-id="b4da5-966">URL for hello Azure Search service.</span></span> | <span data-ttu-id="b4da5-967">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-967">Yes</span></span> |
| <span data-ttu-id="b4da5-968">kulcs</span><span class="sxs-lookup"><span data-stu-id="b4da5-968">key</span></span> | <span data-ttu-id="b4da5-969">Adminisztrátori kulcsot a hello Azure Search szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b4da5-969">Admin key for hello Azure Search service.</span></span> | <span data-ttu-id="b4da5-970">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-970">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-971">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-971">Example</span></span>

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

<span data-ttu-id="b4da5-972">További információkért lásd: [Azure Search összekötő](data-factory-azure-search-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-972">For more information, see [Azure Search connector](data-factory-azure-search-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="b4da5-973">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-973">Dataset</span></span>
<span data-ttu-id="b4da5-974">toodefine egy Azure Search adatkészlet set hello **típus** hello adatkészlet túl**AzureSearchIndex**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz :</span><span class="sxs-lookup"><span data-stu-id="b4da5-974">toodefine an Azure Search dataset, set hello **type** of hello dataset too**AzureSearchIndex**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="b4da5-975">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-975">Property</span></span> | <span data-ttu-id="b4da5-976">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-976">Description</span></span> | <span data-ttu-id="b4da5-977">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-977">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="b4da5-978">type</span><span class="sxs-lookup"><span data-stu-id="b4da5-978">type</span></span> | <span data-ttu-id="b4da5-979">hello type tulajdonság túl be kell állítani**AzureSearchIndex**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-979">hello type property must be set too**AzureSearchIndex**.</span></span>| <span data-ttu-id="b4da5-980">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-980">Yes</span></span> |
| <span data-ttu-id="b4da5-981">indexName</span><span class="sxs-lookup"><span data-stu-id="b4da5-981">indexName</span></span> | <span data-ttu-id="b4da5-982">Hello Azure Search-index neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-982">Name of hello Azure Search index.</span></span> <span data-ttu-id="b4da5-983">Adat-előállító nem hoz létre hello index.</span><span class="sxs-lookup"><span data-stu-id="b4da5-983">Data Factory does not create hello index.</span></span> <span data-ttu-id="b4da5-984">az Azure Search léteznie kell a hello index.</span><span class="sxs-lookup"><span data-stu-id="b4da5-984">hello index must exist in Azure Search.</span></span> | <span data-ttu-id="b4da5-985">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-985">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-986">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-986">Example</span></span>

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

<span data-ttu-id="b4da5-987">További információkért lásd: [Azure Search összekötő](data-factory-azure-search-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-987">For more information, see [Azure Search connector](data-factory-azure-search-connector.md#dataset-properties) article.</span></span>

### <a name="azure-search-index-sink-in-copy-activity"></a><span data-ttu-id="b4da5-988">A másolási tevékenység az Azure Search-Index fogadó</span><span class="sxs-lookup"><span data-stu-id="b4da5-988">Azure Search Index Sink in Copy Activity</span></span>
<span data-ttu-id="b4da5-989">Adatok tooan Azure Search-index másolása, állítsa be a hello **típus gyűjtése** hello a másolási tevékenység túl**AzureSearchIndexSink**, és adja meg a következő tulajdonságok hello **fogadó**szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-989">If you are copying data tooan Azure Search index, set hello **sink type** of hello copy activity too**AzureSearchIndexSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="b4da5-990">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-990">Property</span></span> | <span data-ttu-id="b4da5-991">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-991">Description</span></span> | <span data-ttu-id="b4da5-992">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-992">Allowed values</span></span> | <span data-ttu-id="b4da5-993">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-993">Required</span></span> |
| -------- | ----------- | -------------- | -------- |
| <span data-ttu-id="b4da5-994">WriteBehavior</span><span class="sxs-lookup"><span data-stu-id="b4da5-994">WriteBehavior</span></span> | <span data-ttu-id="b4da5-995">Meghatározza, hogy toomerge vagy cserélje le a dokumentum már létezik hello index.</span><span class="sxs-lookup"><span data-stu-id="b4da5-995">Specifies whether toomerge or replace when a document already exists in hello index.</span></span> | <span data-ttu-id="b4da5-996">Egyesítés (alapértelmezett)</span><span class="sxs-lookup"><span data-stu-id="b4da5-996">Merge (default)</span></span><br/><span data-ttu-id="b4da5-997">Feltöltés</span><span class="sxs-lookup"><span data-stu-id="b4da5-997">Upload</span></span>| <span data-ttu-id="b4da5-998">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-998">No</span></span> |
| <span data-ttu-id="b4da5-999">WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="b4da5-999">WriteBatchSize</span></span> | <span data-ttu-id="b4da5-1000">Fájlfeltöltések hello Azure Search-index az adatokat, amikor hello puffer mérete eléri writeBatchSize.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1000">Uploads data into hello Azure Search index when hello buffer size reaches writeBatchSize.</span></span> | <span data-ttu-id="b4da5-1001">1 too1 000.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1001">1 too1,000.</span></span> <span data-ttu-id="b4da5-1002">Alapértelmezett érték 1000.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1002">Default value is 1000.</span></span> | <span data-ttu-id="b4da5-1003">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1003">No</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1004">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1004">Example</span></span>

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

<span data-ttu-id="b4da5-1005">További információkért lásd: [Azure Search összekötő](data-factory-azure-search-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1005">For more information, see [Azure Search connector](data-factory-azure-search-connector.md#copy-activity-properties) article.</span></span>

## <a name="azure-table-storage"></a><span data-ttu-id="b4da5-1006">Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="b4da5-1006">Azure Table Storage</span></span>

### <a name="linked-service"></a><span data-ttu-id="b4da5-1007">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-1007">Linked service</span></span>
<span data-ttu-id="b4da5-1008">Társított szolgáltatások két típusa van: Azure Storage társított szolgáltatásnak, és a társított szolgáltatásnak Azure Storage SAS.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1008">There are two types of linked services: Azure Storage linked service and Azure Storage SAS linked service.</span></span>

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="b4da5-1009">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1009">Azure Storage Linked Service</span></span>
<span data-ttu-id="b4da5-1010">toolink a az Azure storage-tooa data factory hello segítségével **fiókkulcs**, hozzon létre egy Azure Storage társított szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1010">toolink your Azure storage account tooa data factory by using hello **account key**, create an Azure Storage linked service.</span></span> <span data-ttu-id="b4da5-1011">toodefine egy Azure Storage társított szolgáltatásnak, set hello **típus** hello a társított szolgáltatás túl**AzureStorage**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1011">toodefine an Azure Storage linked service, set hello **type** of hello linked service too**AzureStorage**.</span></span> <span data-ttu-id="b4da5-1012">Ezután megadhatja tulajdonságok hello a következő **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1012">Then, you can specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="b4da5-1013">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1013">Property</span></span> | <span data-ttu-id="b4da5-1014">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1014">Description</span></span> | <span data-ttu-id="b4da5-1015">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1015">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="b4da5-1016">type</span><span class="sxs-lookup"><span data-stu-id="b4da5-1016">type</span></span> |<span data-ttu-id="b4da5-1017">hello type tulajdonságot kell beállítani: **AzureStorage**</span><span class="sxs-lookup"><span data-stu-id="b4da5-1017">hello type property must be set to: **AzureStorage**</span></span> |<span data-ttu-id="b4da5-1018">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1018">Yes</span></span> |
| <span data-ttu-id="b4da5-1019">connectionString</span><span class="sxs-lookup"><span data-stu-id="b4da5-1019">connectionString</span></span> |<span data-ttu-id="b4da5-1020">Adjon meg információt tooconnect tooAzure tárolási hello connectionString tulajdonság szükséges.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1020">Specify information needed tooconnect tooAzure storage for hello connectionString property.</span></span> |<span data-ttu-id="b4da5-1021">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1021">Yes</span></span> |

<span data-ttu-id="b4da5-1022">**Példa**</span><span class="sxs-lookup"><span data-stu-id="b4da5-1022">**Example:**</span></span>  

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

#### <a name="azure-storage-sas-linked-service"></a><span data-ttu-id="b4da5-1023">Az Azure Storage SAS társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1023">Azure Storage SAS Linked Service</span></span>
<span data-ttu-id="b4da5-1024">hello Azure Storage SAS kapcsolódó szolgáltatás lehetővé teszi egy Azure Storage-fiók tooan az Azure data factory toolink egy közös hozzáférésű Jogosultságkód (SAS) használatával.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1024">hello Azure Storage SAS linked service allows you toolink an Azure Storage Account tooan Azure data factory by using a Shared Access Signature (SAS).</span></span> <span data-ttu-id="b4da5-1025">Ez hozzáférést biztosít a hello adat-előállító korlátozott/időhöz kötött tooall/specifikus erőforrások (blobtárolóban /) hello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1025">It provides hello data factory with restricted/time-bound access tooall/specific resources (blob/container) in hello storage.</span></span> <span data-ttu-id="b4da5-1026">toolink a az Azure storage-tooa data factory használatával a közös hozzáférésű Jogosultságkód, Azure Storage SAS társított szolgáltatás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1026">toolink your Azure storage account tooa data factory by using Shared Access Signature, create an Azure Storage SAS linked service.</span></span> <span data-ttu-id="b4da5-1027">egy Azure Storage SAS toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1027">toodefine an Azure Storage SAS linked service, set hello **type** of hello linked service too**AzureStorageSas**.</span></span> <span data-ttu-id="b4da5-1028">Ezután megadhatja tulajdonságok hello a következő **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1028">Then, you can specify following properties in hello **typeProperties** section:</span></span>   

| <span data-ttu-id="b4da5-1029">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1029">Property</span></span> | <span data-ttu-id="b4da5-1030">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1030">Description</span></span> | <span data-ttu-id="b4da5-1031">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1031">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="b4da5-1032">type</span><span class="sxs-lookup"><span data-stu-id="b4da5-1032">type</span></span> |<span data-ttu-id="b4da5-1033">hello type tulajdonságot kell beállítani: **AzureStorageSas**</span><span class="sxs-lookup"><span data-stu-id="b4da5-1033">hello type property must be set to: **AzureStorageSas**</span></span> |<span data-ttu-id="b4da5-1034">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1034">Yes</span></span> |
| <span data-ttu-id="b4da5-1035">sasUri</span><span class="sxs-lookup"><span data-stu-id="b4da5-1035">sasUri</span></span> |<span data-ttu-id="b4da5-1036">Adja meg a megosztott hozzáférési aláírást URI toohello Azure tárolási erőforrások, például blob, a tároló vagy a tábla.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1036">Specify Shared Access Signature URI toohello Azure Storage resources such as blob, container, or table.</span></span> |<span data-ttu-id="b4da5-1037">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1037">Yes</span></span> |

<span data-ttu-id="b4da5-1038">**Példa**</span><span class="sxs-lookup"><span data-stu-id="b4da5-1038">**Example:**</span></span>

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

<span data-ttu-id="b4da5-1039">További információ a következő összekapcsolt szolgáltatások: [Azure Table Storage összekötő](data-factory-azure-table-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1039">For more information about these linked services, see [Azure Table Storage connector](data-factory-azure-table-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="b4da5-1040">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-1040">Dataset</span></span>
<span data-ttu-id="b4da5-1041">toodefine az Azure tábla adatkészlethez set hello **típus** hello adatkészlet túl**AzureTable**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1041">toodefine an Azure Table dataset, set hello **type** of hello dataset too**AzureTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="b4da5-1042">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1042">Property</span></span> | <span data-ttu-id="b4da5-1043">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1043">Description</span></span> | <span data-ttu-id="b4da5-1044">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1044">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-1045">tableName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1045">tableName</span></span> |<span data-ttu-id="b4da5-1046">Hello hello Azure tábla adatbázispéldány táblájának, amelyre a társított szolgáltatás neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1046">Name of hello table in hello Azure Table Database instance that linked service refers to.</span></span> |<span data-ttu-id="b4da5-1047">Igen.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1047">Yes.</span></span> <span data-ttu-id="b4da5-1048">Egy Táblanév egy azureTableSourceQuery nélkül van megadva, a hello tábla összes rekordot másolt toohello cél.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1048">When a tableName is specified without an azureTableSourceQuery, all records from hello table are copied toohello destination.</span></span> <span data-ttu-id="b4da5-1049">Ha egy azureTableSourceQuery is meg van adva, a rekordok hello táblázatból, amely eleget tesz a hello lekérdezés olyan másolt toohello cél.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1049">If an azureTableSourceQuery is also specified, records from hello table that satisfies hello query are copied toohello destination.</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1050">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1050">Example</span></span>

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

<span data-ttu-id="b4da5-1051">További információ a következő összekapcsolt szolgáltatások: [Azure Table Storage összekötő](data-factory-azure-table-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1051">For more information about these linked services, see [Azure Table Storage connector](data-factory-azure-table-connector.md#dataset-properties) article.</span></span> 

### <a name="azure-table-source-in-copy-activity"></a><span data-ttu-id="b4da5-1052">A másolási tevékenység az Azure tábla forrása</span><span class="sxs-lookup"><span data-stu-id="b4da5-1052">Azure Table Source in Copy Activity</span></span>
<span data-ttu-id="b4da5-1053">Adatok másolása az Azure Table Storage, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**AzureTableSource**, és adja meg a következő tulajdonságok hello **forrás**szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1053">If you are copying data from Azure Table Storage, set hello **source type** of hello copy activity too**AzureTableSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="b4da5-1054">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1054">Property</span></span> | <span data-ttu-id="b4da5-1055">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1055">Description</span></span> | <span data-ttu-id="b4da5-1056">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-1056">Allowed values</span></span> | <span data-ttu-id="b4da5-1057">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1057">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-1058">azureTableSourceQuery</span><span class="sxs-lookup"><span data-stu-id="b4da5-1058">azureTableSourceQuery</span></span> |<span data-ttu-id="b4da5-1059">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1059">Use hello custom query tooread data.</span></span> |<span data-ttu-id="b4da5-1060">Azure-tábla lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1060">Azure table query string.</span></span> <span data-ttu-id="b4da5-1061">Példák a következő szakaszban hello.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1061">See examples in hello next section.</span></span> |<span data-ttu-id="b4da5-1062">Nem.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1062">No.</span></span> <span data-ttu-id="b4da5-1063">Egy Táblanév egy azureTableSourceQuery nélkül van megadva, a hello tábla összes rekordot másolt toohello cél.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1063">When a tableName is specified without an azureTableSourceQuery, all records from hello table are copied toohello destination.</span></span> <span data-ttu-id="b4da5-1064">Ha egy azureTableSourceQuery is meg van adva, a rekordok hello táblázatból, amely eleget tesz a hello lekérdezés olyan másolt toohello cél.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1064">If an azureTableSourceQuery is also specified, records from hello table that satisfies hello query are copied toohello destination.</span></span> |
| <span data-ttu-id="b4da5-1065">azureTableSourceIgnoreTableNotFound</span><span class="sxs-lookup"><span data-stu-id="b4da5-1065">azureTableSourceIgnoreTableNotFound</span></span> |<span data-ttu-id="b4da5-1066">Azt jelzi, hogy swallow hello kivétel tábla nem létezik.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1066">Indicate whether swallow hello exception of table not exist.</span></span> |<span data-ttu-id="b4da5-1067">IGAZ</span><span class="sxs-lookup"><span data-stu-id="b4da5-1067">TRUE</span></span><br/><span data-ttu-id="b4da5-1068">HAMIS</span><span class="sxs-lookup"><span data-stu-id="b4da5-1068">FALSE</span></span> |<span data-ttu-id="b4da5-1069">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1069">No</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1070">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1070">Example</span></span>

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

<span data-ttu-id="b4da5-1071">További információ a következő összekapcsolt szolgáltatások: [Azure Table Storage összekötő](data-factory-azure-table-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1071">For more information about these linked services, see [Azure Table Storage connector](data-factory-azure-table-connector.md#copy-activity-properties) article.</span></span> 

### <a name="azure-table-sink-in-copy-activity"></a><span data-ttu-id="b4da5-1072">A másolási tevékenység az Azure tábla fogadó</span><span class="sxs-lookup"><span data-stu-id="b4da5-1072">Azure Table Sink in Copy Activity</span></span>
<span data-ttu-id="b4da5-1073">A Table Storage adatok tooAzure másolása, állítsa be a hello **típus gyűjtése** hello a másolási tevékenység túl**AzureTableSink**, és adja meg a következő tulajdonságok hello **fogadó** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1073">If you are copying data tooAzure Table Storage, set hello **sink type** of hello copy activity too**AzureTableSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="b4da5-1074">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1074">Property</span></span> | <span data-ttu-id="b4da5-1075">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1075">Description</span></span> | <span data-ttu-id="b4da5-1076">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-1076">Allowed values</span></span> | <span data-ttu-id="b4da5-1077">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1077">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-1078">azureTableDefaultPartitionKeyValue</span><span class="sxs-lookup"><span data-stu-id="b4da5-1078">azureTableDefaultPartitionKeyValue</span></span> |<span data-ttu-id="b4da5-1079">Alapértelmezett partíció kulcsérték hello a fogadó által használható.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1079">Default partition key value that can be used by hello sink.</span></span> |<span data-ttu-id="b4da5-1080">Egy karakterlánc-érték.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1080">A string value.</span></span> |<span data-ttu-id="b4da5-1081">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1081">No</span></span> |
| <span data-ttu-id="b4da5-1082">azureTablePartitionKeyName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1082">azureTablePartitionKeyName</span></span> |<span data-ttu-id="b4da5-1083">Adja meg, amelynek értékeket fogja használni, mint partíciókulcsok hello oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1083">Specify name of hello column whose values are used as partition keys.</span></span> <span data-ttu-id="b4da5-1084">Ha nincs megadva, AzureTableDefaultPartitionKeyValue hello partíciókulcs lesz.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1084">If not specified, AzureTableDefaultPartitionKeyValue is used as hello partition key.</span></span> |<span data-ttu-id="b4da5-1085">Egy oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1085">A column name.</span></span> |<span data-ttu-id="b4da5-1086">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1086">No</span></span> |
| <span data-ttu-id="b4da5-1087">azureTableRowKeyName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1087">azureTableRowKeyName</span></span> |<span data-ttu-id="b4da5-1088">Adja meg, amelynek oszlop értékeit kulcsként sor hello oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1088">Specify name of hello column whose column values are used as row key.</span></span> <span data-ttu-id="b4da5-1089">Ha nincs megadva, minden egyes sorára használjon a GUID Azonosítót.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1089">If not specified, use a GUID for each row.</span></span> |<span data-ttu-id="b4da5-1090">Egy oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1090">A column name.</span></span> |<span data-ttu-id="b4da5-1091">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1091">No</span></span> |
| <span data-ttu-id="b4da5-1092">azureTableInsertType</span><span class="sxs-lookup"><span data-stu-id="b4da5-1092">azureTableInsertType</span></span> |<span data-ttu-id="b4da5-1093">hello mód tooinsert adatait az Azure táblájába.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1093">hello mode tooinsert data into Azure table.</span></span><br/><br/><span data-ttu-id="b4da5-1094">Ez a tulajdonság szabja meg, hogy meglévő hello kimeneti táblát, amely megfelelő a partíció-és sorkulcsok sora cseréje vagy egyesített értékükre.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1094">This property controls whether existing rows in hello output table with matching partition and row keys have their values replaced or merged.</span></span> <br/><br/><span data-ttu-id="b4da5-1095">toolearn arról, hogyan működnek ezek a beállítások (lemezegyesítési és -csere), lásd: [Insert vagy az egyesítéses entitás](https://msdn.microsoft.com/library/azure/hh452241.aspx) és [Insert vagy az entitás cseréje](https://msdn.microsoft.com/library/azure/hh452242.aspx) témaköröket.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1095">toolearn about how these settings (merge and replace) work, see [Insert or Merge Entity](https://msdn.microsoft.com/library/azure/hh452241.aspx) and [Insert or Replace Entity](https://msdn.microsoft.com/library/azure/hh452242.aspx) topics.</span></span> <br/><br> <span data-ttu-id="b4da5-1096">Ez a beállítás érvényes szinten hello sor nem hello táblaszintű, és sem a lehetőség törli a nem létező hello bemeneti hello kimeneti tábla sorainak.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1096">This setting applies at hello row level, not hello table level, and neither option deletes rows in hello output table that do not exist in hello input.</span></span> |<span data-ttu-id="b4da5-1097">Egyesítés (alapértelmezett)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1097">merge (default)</span></span><br/><span data-ttu-id="b4da5-1098">cserélje le</span><span class="sxs-lookup"><span data-stu-id="b4da5-1098">replace</span></span> |<span data-ttu-id="b4da5-1099">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1099">No</span></span> |
| <span data-ttu-id="b4da5-1100">WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="b4da5-1100">writeBatchSize</span></span> |<span data-ttu-id="b4da5-1101">Amikor hello writeBatchSize vagy writeBatchTimeout találati adatok beillesztése hello Azure-tábla.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1101">Inserts data into hello Azure table when hello writeBatchSize or writeBatchTimeout is hit.</span></span> |<span data-ttu-id="b4da5-1102">Egész szám (sorok száma)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1102">Integer (number of rows)</span></span> |<span data-ttu-id="b4da5-1103">Nem (alapértelmezett: 10000)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1103">No (default: 10000)</span></span> |
| <span data-ttu-id="b4da5-1104">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="b4da5-1104">writeBatchTimeout</span></span> |<span data-ttu-id="b4da5-1105">Adatbeszúrást hello Azure-tábla amikor hello writeBatchSize vagy writeBatchTimeout találati</span><span class="sxs-lookup"><span data-stu-id="b4da5-1105">Inserts data into hello Azure table when hello writeBatchSize or writeBatchTimeout is hit</span></span> |<span data-ttu-id="b4da5-1106">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="b4da5-1106">timespan</span></span><br/><br/><span data-ttu-id="b4da5-1107">Példa: "00: 20:00" (20 perc)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1107">Example: “00:20:00” (20 minutes)</span></span> |<span data-ttu-id="b4da5-1108">Nem (alapértelmezett toostorage ügyfél alapértelmezett időtúllépési érték 90 másodperc)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1108">No (Default toostorage client default timeout value 90 sec)</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1109">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1109">Example</span></span>

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
<span data-ttu-id="b4da5-1110">További információ a következő összekapcsolt szolgáltatások: [Azure Table Storage összekötő](data-factory-azure-table-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1110">For more information about these linked services, see [Azure Table Storage connector](data-factory-azure-table-connector.md#copy-activity-properties) article.</span></span> 

## <a name="amazon-redshift"></a><span data-ttu-id="b4da5-1111">Amazon RedShift</span><span class="sxs-lookup"><span data-stu-id="b4da5-1111">Amazon RedShift</span></span>

### <a name="linked-service"></a><span data-ttu-id="b4da5-1112">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-1112">Linked service</span></span>
<span data-ttu-id="b4da5-1113">az Amazon Redshift toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**AmazonRedshift**, és adja meg a következő tulajdonságok hello **typeProperties**szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1113">toodefine an Amazon Redshift linked service, set hello **type** of hello linked service too**AmazonRedshift**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="b4da5-1114">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1114">Property</span></span> | <span data-ttu-id="b4da5-1115">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1115">Description</span></span> | <span data-ttu-id="b4da5-1116">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1116">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-1117">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="b4da5-1117">server</span></span> |<span data-ttu-id="b4da5-1118">Kiszolgáló IP-címét vagy állomásnevét kiszolgálónevét hello Amazon Redshift.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1118">IP address or host name of hello Amazon Redshift server.</span></span> |<span data-ttu-id="b4da5-1119">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1119">Yes</span></span> |
| <span data-ttu-id="b4da5-1120">port</span><span class="sxs-lookup"><span data-stu-id="b4da5-1120">port</span></span> |<span data-ttu-id="b4da5-1121">Amazon Redshift server hello hello TCP port száma hello toolisten ügyfél-kommunikációhoz használ.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1121">hello number of hello TCP port that hello Amazon Redshift server uses toolisten for client connections.</span></span> |<span data-ttu-id="b4da5-1122">Nem, alapértelmezett érték: 5439</span><span class="sxs-lookup"><span data-stu-id="b4da5-1122">No, default value: 5439</span></span> |
| <span data-ttu-id="b4da5-1123">adatbázis</span><span class="sxs-lookup"><span data-stu-id="b4da5-1123">database</span></span> |<span data-ttu-id="b4da5-1124">Hello Amazon Redshift adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1124">Name of hello Amazon Redshift database.</span></span> |<span data-ttu-id="b4da5-1125">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1125">Yes</span></span> |
| <span data-ttu-id="b4da5-1126">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="b4da5-1126">username</span></span> |<span data-ttu-id="b4da5-1127">Hozzáférés toohello adatbázis-felhasználó nevét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1127">Name of user who has access toohello database.</span></span> |<span data-ttu-id="b4da5-1128">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1128">Yes</span></span> |
| <span data-ttu-id="b4da5-1129">jelszó</span><span class="sxs-lookup"><span data-stu-id="b4da5-1129">password</span></span> |<span data-ttu-id="b4da5-1130">Hello felhasználói fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1130">Password for hello user account.</span></span> |<span data-ttu-id="b4da5-1131">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1131">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1132">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1132">Example</span></span>

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

<span data-ttu-id="b4da5-1133">További információkért lásd: [Amazon Redshift összekötő](#data-factory-amazon-redshift-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1133">For more information, see [Amazon Redshift connector](#data-factory-amazon-redshift-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="b4da5-1134">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-1134">Dataset</span></span>
<span data-ttu-id="b4da5-1135">toodefine az Amazon Redshift adatkészlethez set hello **típus** hello adatkészlet túl**RelationalTable**, és adja meg a következő tulajdonságai a hello hello **typeProperties** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1135">toodefine an Amazon Redshift dataset, set hello **type** of hello dataset too**RelationalTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="b4da5-1136">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1136">Property</span></span> | <span data-ttu-id="b4da5-1137">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1137">Description</span></span> | <span data-ttu-id="b4da5-1138">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1138">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-1139">tableName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1139">tableName</span></span> |<span data-ttu-id="b4da5-1140">Hello tábla hello Amazon Redshift adatbázis, amelyre a társított szolgáltatás neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1140">Name of hello table in hello Amazon Redshift database that linked service refers to.</span></span> |<span data-ttu-id="b4da5-1141">Nem (Ha **lekérdezés** a **RelationalSource** van megadva)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1141">No (if **query** of **RelationalSource** is specified)</span></span> |


#### <a name="example"></a><span data-ttu-id="b4da5-1142">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1142">Example</span></span>

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
<span data-ttu-id="b4da5-1143">További információkért lásd: [Amazon Redshift összekötő](#data-factory-amazon-redshift-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1143">For more information, see [Amazon Redshift connector](#data-factory-amazon-redshift-connector.md#dataset-properties) article.</span></span>

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="b4da5-1144">A másolási tevékenység relációs adatforrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1144">Relational Source in Copy Activity</span></span> 
<span data-ttu-id="b4da5-1145">Adatok másolása az Amazon Redshift, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**RelationalSource**, és adja meg a következő tulajdonságok hello **forrás** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1145">If you are copying data from Amazon Redshift, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="b4da5-1146">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1146">Property</span></span> | <span data-ttu-id="b4da5-1147">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1147">Description</span></span> | <span data-ttu-id="b4da5-1148">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-1148">Allowed values</span></span> | <span data-ttu-id="b4da5-1149">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1149">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-1150">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="b4da5-1150">query</span></span> |<span data-ttu-id="b4da5-1151">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1151">Use hello custom query tooread data.</span></span> |<span data-ttu-id="b4da5-1152">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1152">SQL query string.</span></span> <span data-ttu-id="b4da5-1153">Például: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1153">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="b4da5-1154">Nem (Ha **tableName** a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1154">No (if **tableName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1155">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1155">Example</span></span>

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
<span data-ttu-id="b4da5-1156">További információkért lásd: [Amazon Redshift összekötő](#data-factory-amazon-redshift-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1156">For more information, see [Amazon Redshift connector](#data-factory-amazon-redshift-connector.md#copy-activity-properties) article.</span></span>

## <a name="ibm-db2"></a><span data-ttu-id="b4da5-1157">IBM DB2</span><span class="sxs-lookup"><span data-stu-id="b4da5-1157">IBM DB2</span></span>

### <a name="linked-service"></a><span data-ttu-id="b4da5-1158">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-1158">Linked service</span></span>
<span data-ttu-id="b4da5-1159">az IBM DB2 toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**OnPremisesDB2**, és adja meg a következő tulajdonságok hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1159">toodefine an IBM DB2 linked service, set hello **type** of hello linked service too**OnPremisesDB2**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="b4da5-1160">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1160">Property</span></span> | <span data-ttu-id="b4da5-1161">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1161">Description</span></span> | <span data-ttu-id="b4da5-1162">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1162">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-1163">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="b4da5-1163">server</span></span> |<span data-ttu-id="b4da5-1164">Hello DB2-kiszolgáló neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1164">Name of hello DB2 server.</span></span> |<span data-ttu-id="b4da5-1165">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1165">Yes</span></span> |
| <span data-ttu-id="b4da5-1166">adatbázis</span><span class="sxs-lookup"><span data-stu-id="b4da5-1166">database</span></span> |<span data-ttu-id="b4da5-1167">Hello DB2-adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1167">Name of hello DB2 database.</span></span> |<span data-ttu-id="b4da5-1168">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1168">Yes</span></span> |
| <span data-ttu-id="b4da5-1169">Séma</span><span class="sxs-lookup"><span data-stu-id="b4da5-1169">schema</span></span> |<span data-ttu-id="b4da5-1170">Hello séma hello adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1170">Name of hello schema in hello database.</span></span> <span data-ttu-id="b4da5-1171">hello sémanév a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1171">hello schema name is case-sensitive.</span></span> |<span data-ttu-id="b4da5-1172">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1172">No</span></span> |
| <span data-ttu-id="b4da5-1173">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="b4da5-1173">authenticationType</span></span> |<span data-ttu-id="b4da5-1174">Tooconnect toohello DB2-adatbázishoz használt hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1174">Type of authentication used tooconnect toohello DB2 database.</span></span> <span data-ttu-id="b4da5-1175">Lehetséges értékek a következők: névtelen, alapszintű és a Windows.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1175">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="b4da5-1176">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1176">Yes</span></span> |
| <span data-ttu-id="b4da5-1177">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="b4da5-1177">username</span></span> |<span data-ttu-id="b4da5-1178">Adja meg a felhasználónevet Basic vagy Windows-hitelesítés használata.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1178">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="b4da5-1179">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1179">No</span></span> |
| <span data-ttu-id="b4da5-1180">jelszó</span><span class="sxs-lookup"><span data-stu-id="b4da5-1180">password</span></span> |<span data-ttu-id="b4da5-1181">Adja meg a megadott felhasználónévhez hello hello felhasználói fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1181">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="b4da5-1182">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1182">No</span></span> |
| <span data-ttu-id="b4da5-1183">gatewayName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1183">gatewayName</span></span> |<span data-ttu-id="b4da5-1184">Hello átjáró, amely a Data Factory szolgáltatásnak hello nevét használja tooconnect toohello helyszíni DB2-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1184">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises DB2 database.</span></span> |<span data-ttu-id="b4da5-1185">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1185">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1186">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1186">Example</span></span>
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
<span data-ttu-id="b4da5-1187">További információkért lásd: [IBM DB2-összekötő](#data-factory-onprem-db2-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1187">For more information, see [IBM DB2 connector](#data-factory-onprem-db2-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="b4da5-1188">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-1188">Dataset</span></span>
<span data-ttu-id="b4da5-1189">toodefine DB2 dataset, set hello **típus** hello adatkészlet túl**RelationalTable**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1189">toodefine a DB2 dataset, set hello **type** of hello dataset too**RelationalTable**, and specify hello following properties in hello **typeProperties** section:</span></span>

| <span data-ttu-id="b4da5-1190">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1190">Property</span></span> | <span data-ttu-id="b4da5-1191">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1191">Description</span></span> | <span data-ttu-id="b4da5-1192">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1192">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-1193">tableName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1193">tableName</span></span> |<span data-ttu-id="b4da5-1194">Hello hello DB2 adatbázispéldány táblájának, amelyre a társított szolgáltatás neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1194">Name of hello table in hello DB2 Database instance that linked service refers to.</span></span> <span data-ttu-id="b4da5-1195">hello táblanév a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1195">hello tableName is case-sensitive.</span></span> |<span data-ttu-id="b4da5-1196">Nem (Ha **lekérdezés** a **RelationalSource** van megadva)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1196">No (if **query** of **RelationalSource** is specified)</span></span> 

#### <a name="example"></a><span data-ttu-id="b4da5-1197">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1197">Example</span></span>
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

<span data-ttu-id="b4da5-1198">További információkért lásd: [IBM DB2-összekötő](#data-factory-onprem-db2-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1198">For more information, see [IBM DB2 connector](#data-factory-onprem-db2-connector.md#dataset-properties) article.</span></span>

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="b4da5-1199">A másolási tevékenység relációs adatforrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1199">Relational Source in Copy Activity</span></span>
<span data-ttu-id="b4da5-1200">Adatok másolása az IBM DB2, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**RelationalSource**, és adja meg a következő tulajdonságok hello **forrás** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1200">If you are copying data from IBM DB2, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="b4da5-1201">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1201">Property</span></span> | <span data-ttu-id="b4da5-1202">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1202">Description</span></span> | <span data-ttu-id="b4da5-1203">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-1203">Allowed values</span></span> | <span data-ttu-id="b4da5-1204">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1204">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-1205">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="b4da5-1205">query</span></span> |<span data-ttu-id="b4da5-1206">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1206">Use hello custom query tooread data.</span></span> |<span data-ttu-id="b4da5-1207">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1207">SQL query string.</span></span> <span data-ttu-id="b4da5-1208">Például: `"query": "select * from "MySchema"."MyTable""`.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1208">For example: `"query": "select * from "MySchema"."MyTable""`.</span></span> |<span data-ttu-id="b4da5-1209">Nem (Ha **tableName** a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1209">No (if **tableName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1210">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1210">Example</span></span>
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
<span data-ttu-id="b4da5-1211">További információkért lásd: [IBM DB2-összekötő](#data-factory-onprem-db2-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1211">For more information, see [IBM DB2 connector](#data-factory-onprem-db2-connector.md#copy-activity-properties) article.</span></span>

## <a name="mysql"></a><span data-ttu-id="b4da5-1212">MySQL</span><span class="sxs-lookup"><span data-stu-id="b4da5-1212">MySQL</span></span>

### <a name="linked-service"></a><span data-ttu-id="b4da5-1213">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-1213">Linked service</span></span>
<span data-ttu-id="b4da5-1214">egy MySQL toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**OnPremisesMySql**, és adja meg a következő tulajdonságok hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1214">toodefine a MySQL linked service, set hello **type** of hello linked service too**OnPremisesMySql**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="b4da5-1215">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1215">Property</span></span> | <span data-ttu-id="b4da5-1216">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1216">Description</span></span> | <span data-ttu-id="b4da5-1217">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1217">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-1218">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="b4da5-1218">server</span></span> |<span data-ttu-id="b4da5-1219">Hello MySQL kiszolgáló neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1219">Name of hello MySQL server.</span></span> |<span data-ttu-id="b4da5-1220">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1220">Yes</span></span> |
| <span data-ttu-id="b4da5-1221">adatbázis</span><span class="sxs-lookup"><span data-stu-id="b4da5-1221">database</span></span> |<span data-ttu-id="b4da5-1222">Hello MySQL-adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1222">Name of hello MySQL database.</span></span> |<span data-ttu-id="b4da5-1223">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1223">Yes</span></span> |
| <span data-ttu-id="b4da5-1224">Séma</span><span class="sxs-lookup"><span data-stu-id="b4da5-1224">schema</span></span> |<span data-ttu-id="b4da5-1225">Hello séma hello adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1225">Name of hello schema in hello database.</span></span> |<span data-ttu-id="b4da5-1226">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1226">No</span></span> |
| <span data-ttu-id="b4da5-1227">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="b4da5-1227">authenticationType</span></span> |<span data-ttu-id="b4da5-1228">Tooconnect toohello MySQL-adatbázis használt hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1228">Type of authentication used tooconnect toohello MySQL database.</span></span> <span data-ttu-id="b4da5-1229">Lehetséges értékek a következők: `Basic`.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1229">Possible values are: `Basic`.</span></span> |<span data-ttu-id="b4da5-1230">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1230">Yes</span></span> |
| <span data-ttu-id="b4da5-1231">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="b4da5-1231">username</span></span> |<span data-ttu-id="b4da5-1232">Adja meg a felhasználó nevét tooconnect toohello MySQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1232">Specify user name tooconnect toohello MySQL database.</span></span> |<span data-ttu-id="b4da5-1233">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1233">Yes</span></span> |
| <span data-ttu-id="b4da5-1234">jelszó</span><span class="sxs-lookup"><span data-stu-id="b4da5-1234">password</span></span> |<span data-ttu-id="b4da5-1235">Adja meg a megadott hello felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1235">Specify password for hello user account you specified.</span></span> |<span data-ttu-id="b4da5-1236">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1236">Yes</span></span> |
| <span data-ttu-id="b4da5-1237">gatewayName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1237">gatewayName</span></span> |<span data-ttu-id="b4da5-1238">Hello átjáró, amely a Data Factory szolgáltatásnak hello nevét kell tooconnect toohello helyszíni MySQL-adatbázis használata.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1238">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises MySQL database.</span></span> |<span data-ttu-id="b4da5-1239">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1239">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1240">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1240">Example</span></span>

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

<span data-ttu-id="b4da5-1241">További információkért lásd: [MySQL összekötő](data-factory-onprem-mysql-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1241">For more information, see [MySQL connector](data-factory-onprem-mysql-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="b4da5-1242">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-1242">Dataset</span></span>
<span data-ttu-id="b4da5-1243">toodefine egy MySQL dataset set hello **típus** hello adatkészlet túl**RelationalTable**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1243">toodefine a MySQL dataset, set hello **type** of hello dataset too**RelationalTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="b4da5-1244">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1244">Property</span></span> | <span data-ttu-id="b4da5-1245">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1245">Description</span></span> | <span data-ttu-id="b4da5-1246">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1246">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-1247">tableName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1247">tableName</span></span> |<span data-ttu-id="b4da5-1248">Hello tábla hello MySQL-adatbázis-példány, amelyre a társított szolgáltatás neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1248">Name of hello table in hello MySQL Database instance that linked service refers to.</span></span> |<span data-ttu-id="b4da5-1249">Nem (Ha **lekérdezés** a **RelationalSource** van megadva)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1249">No (if **query** of **RelationalSource** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1250">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1250">Example</span></span>

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
<span data-ttu-id="b4da5-1251">További információkért lásd: [MySQL összekötő](data-factory-onprem-mysql-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1251">For more information, see [MySQL connector](data-factory-onprem-mysql-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="b4da5-1252">A másolási tevékenység relációs adatforrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1252">Relational Source in Copy Activity</span></span>
<span data-ttu-id="b4da5-1253">Adatok másolása a MySQL-adatbázis, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**RelationalSource**, és adja meg a következő tulajdonságok hello **forrás** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1253">If you are copying data from a MySQL database, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="b4da5-1254">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1254">Property</span></span> | <span data-ttu-id="b4da5-1255">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1255">Description</span></span> | <span data-ttu-id="b4da5-1256">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-1256">Allowed values</span></span> | <span data-ttu-id="b4da5-1257">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1257">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-1258">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="b4da5-1258">query</span></span> |<span data-ttu-id="b4da5-1259">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1259">Use hello custom query tooread data.</span></span> |<span data-ttu-id="b4da5-1260">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1260">SQL query string.</span></span> <span data-ttu-id="b4da5-1261">Például: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1261">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="b4da5-1262">Nem (Ha **tableName** a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1262">No (if **tableName** of **dataset** is specified)</span></span> |


#### <a name="example"></a><span data-ttu-id="b4da5-1263">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1263">Example</span></span>
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

<span data-ttu-id="b4da5-1264">További információkért lásd: [MySQL összekötő](data-factory-onprem-mysql-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1264">For more information, see [MySQL connector](data-factory-onprem-mysql-connector.md#copy-activity-properties) article.</span></span> 

## <a name="oracle"></a><span data-ttu-id="b4da5-1265">Oracle</span><span class="sxs-lookup"><span data-stu-id="b4da5-1265">Oracle</span></span> 

### <a name="linked-service"></a><span data-ttu-id="b4da5-1266">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-1266">Linked service</span></span>
<span data-ttu-id="b4da5-1267">az Oracle toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**OnPremisesOracle**, és adja meg a következő tulajdonságok hello **typeProperties** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1267">toodefine an Oracle linked service, set hello **type** of hello linked service too**OnPremisesOracle**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="b4da5-1268">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1268">Property</span></span> | <span data-ttu-id="b4da5-1269">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1269">Description</span></span> | <span data-ttu-id="b4da5-1270">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1270">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-1271">driverType</span><span class="sxs-lookup"><span data-stu-id="b4da5-1271">driverType</span></span> | <span data-ttu-id="b4da5-1272">Adja meg, mely illesztőprogram toouse toocopy adatait / tooOracle adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1272">Specify which driver toouse toocopy data from/tooOracle Database.</span></span> <span data-ttu-id="b4da5-1273">Két érték engedélyezett **Microsoft** vagy **ODP** (alapértelmezett).</span><span class="sxs-lookup"><span data-stu-id="b4da5-1273">Allowed values are **Microsoft** or **ODP** (default).</span></span> <span data-ttu-id="b4da5-1274">Lásd: [verziójától és a telepítés támogatott](#supported-versions-and-installation) illesztőprogram adatai szakaszban.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1274">See [Supported version and installation](#supported-versions-and-installation) section on driver details.</span></span> | <span data-ttu-id="b4da5-1275">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1275">No</span></span> |
| <span data-ttu-id="b4da5-1276">connectionString</span><span class="sxs-lookup"><span data-stu-id="b4da5-1276">connectionString</span></span> | <span data-ttu-id="b4da5-1277">Adjon meg információt tooconnect toohello Oracle adatbázispéldány hello connectionString tulajdonság szükséges.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1277">Specify information needed tooconnect toohello Oracle Database instance for hello connectionString property.</span></span> | <span data-ttu-id="b4da5-1278">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1278">Yes</span></span> |
| <span data-ttu-id="b4da5-1279">gatewayName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1279">gatewayName</span></span> | <span data-ttu-id="b4da5-1280">Hello átjáró, amely használt tooconnect toohello helyszíni Oracle-kiszolgáló neve</span><span class="sxs-lookup"><span data-stu-id="b4da5-1280">Name of hello gateway that that is used tooconnect toohello on-premises Oracle server</span></span> |<span data-ttu-id="b4da5-1281">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1281">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1282">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1282">Example</span></span>
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

<span data-ttu-id="b4da5-1283">További információkért lásd: [Oracle összekötő](data-factory-onprem-oracle-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1283">For more information, see [Oracle connector](data-factory-onprem-oracle-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="b4da5-1284">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-1284">Dataset</span></span>
<span data-ttu-id="b4da5-1285">toodefine az Oracle adatkészlethez set hello **típus** hello adatkészlet túl**OracleTable**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1285">toodefine an Oracle dataset, set hello **type** of hello dataset too**OracleTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="b4da5-1286">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1286">Property</span></span> | <span data-ttu-id="b4da5-1287">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1287">Description</span></span> | <span data-ttu-id="b4da5-1288">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1288">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-1289">tableName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1289">tableName</span></span> |<span data-ttu-id="b4da5-1290">Oracle-adatbázishoz kapcsolódó szolgáltatás hello hello hello tábla neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1290">Name of hello table in hello Oracle Database that hello linked service refers to.</span></span> |<span data-ttu-id="b4da5-1291">Nem (Ha **oracleReaderQuery** a **OracleSource** van megadva)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1291">No (if **oracleReaderQuery** of **OracleSource** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1292">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1292">Example</span></span>

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
<span data-ttu-id="b4da5-1293">További információkért lásd: [Oracle összekötő](data-factory-onprem-oracle-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1293">For more information, see [Oracle connector](data-factory-onprem-oracle-connector.md#dataset-properties) article.</span></span>

### <a name="oracle-source-in-copy-activity"></a><span data-ttu-id="b4da5-1294">A másolási tevékenység Oracle-forrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1294">Oracle Source in Copy Activity</span></span>
<span data-ttu-id="b4da5-1295">Adatok másolása az Oracle-adatbázishoz, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**OracleSource**, és adja meg a következő tulajdonságok hello **forrás** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1295">If you are copying data from an Oracle database, set hello **source type** of hello copy activity too**OracleSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="b4da5-1296">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1296">Property</span></span> | <span data-ttu-id="b4da5-1297">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1297">Description</span></span> | <span data-ttu-id="b4da5-1298">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-1298">Allowed values</span></span> | <span data-ttu-id="b4da5-1299">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1299">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-1300">oracleReaderQuery</span><span class="sxs-lookup"><span data-stu-id="b4da5-1300">oracleReaderQuery</span></span> |<span data-ttu-id="b4da5-1301">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1301">Use hello custom query tooread data.</span></span> |<span data-ttu-id="b4da5-1302">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1302">SQL query string.</span></span> <span data-ttu-id="b4da5-1303">Például:`select * from MyTable`</span><span class="sxs-lookup"><span data-stu-id="b4da5-1303">For example: `select * from MyTable`</span></span> <br/><br/><span data-ttu-id="b4da5-1304">Ha nincs megadva, hello végrehajtott SQL-utasítást:`select * from MyTable`</span><span class="sxs-lookup"><span data-stu-id="b4da5-1304">If not specified, hello SQL statement that is executed: `select * from MyTable`</span></span> |<span data-ttu-id="b4da5-1305">Nem (Ha **tableName** a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1305">No (if **tableName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1306">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1306">Example</span></span>

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

<span data-ttu-id="b4da5-1307">További információkért lásd: [Oracle összekötő](data-factory-onprem-oracle-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1307">For more information, see [Oracle connector](data-factory-onprem-oracle-connector.md#copy-activity-properties) article.</span></span>

### <a name="oracle-sink-in-copy-activity"></a><span data-ttu-id="b4da5-1308">A másolási tevékenység Oracle fogadó</span><span class="sxs-lookup"><span data-stu-id="b4da5-1308">Oracle Sink in Copy Activity</span></span>
<span data-ttu-id="b4da5-1309">Adatok tooam Oracle adatbázis másolása, állítsa be a hello **típus gyűjtése** hello a másolási tevékenység túl**OracleSink**, és adja meg a következő tulajdonságok hello **fogadó** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1309">If you are copying data tooam Oracle database, set hello **sink type** of hello copy activity too**OracleSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="b4da5-1310">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1310">Property</span></span> | <span data-ttu-id="b4da5-1311">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1311">Description</span></span> | <span data-ttu-id="b4da5-1312">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-1312">Allowed values</span></span> | <span data-ttu-id="b4da5-1313">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1313">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-1314">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="b4da5-1314">writeBatchTimeout</span></span> |<span data-ttu-id="b4da5-1315">Várnia kell az hello kötegelt beszúrási művelet toocomplete előtt azt az időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1315">Wait time for hello batch insert operation toocomplete before it times out.</span></span> |<span data-ttu-id="b4da5-1316">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="b4da5-1316">timespan</span></span><br/><br/> <span data-ttu-id="b4da5-1317">. Példa: 00:30:00 (30 perc).</span><span class="sxs-lookup"><span data-stu-id="b4da5-1317">Example: 00:30:00 (30 minutes).</span></span> |<span data-ttu-id="b4da5-1318">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1318">No</span></span> |
| <span data-ttu-id="b4da5-1319">WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="b4da5-1319">writeBatchSize</span></span> |<span data-ttu-id="b4da5-1320">Amikor hello puffer mérete eléri writeBatchSize adatok beillesztése hello SQL táblázat.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1320">Inserts data into hello SQL table when hello buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="b4da5-1321">Egész szám (sorok száma)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1321">Integer (number of rows)</span></span> |<span data-ttu-id="b4da5-1322">Nem (alapértelmezett: 100)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1322">No (default: 100)</span></span> |
| <span data-ttu-id="b4da5-1323">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="b4da5-1323">sqlWriterCleanupScript</span></span> |<span data-ttu-id="b4da5-1324">Adja meg a másolási tevékenység tooexecute vonatkozó lekérdezést úgy, hogy egy adott szelet adatait.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1324">Specify a query for Copy Activity tooexecute such that data of a specific slice is cleaned up.</span></span> |<span data-ttu-id="b4da5-1325">A lekérdezési utasítást.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1325">A query statement.</span></span> |<span data-ttu-id="b4da5-1326">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1326">No</span></span> |
| <span data-ttu-id="b4da5-1327">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1327">sliceIdentifierColumnName</span></span> |<span data-ttu-id="b4da5-1328">Adja meg, a másolási tevékenység toofill oszlopnév automatikusan létrejön szelet azonosítóval, amely adatokat egy adott szelet, amikor futtassa újra a használt tooclean.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1328">Specify column name for Copy Activity toofill with auto generated slice identifier, which is used tooclean up data of a specific slice when rerun.</span></span> |<span data-ttu-id="b4da5-1329">Egy oszlop binary(32) adattípusú oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1329">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="b4da5-1330">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1330">No</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1331">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1331">Example</span></span>
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
<span data-ttu-id="b4da5-1332">További információkért lásd: [Oracle összekötő](data-factory-onprem-oracle-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1332">For more information, see [Oracle connector](data-factory-onprem-oracle-connector.md#copy-activity-properties) article.</span></span>

## <a name="postgresql"></a><span data-ttu-id="b4da5-1333">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="b4da5-1333">PostgreSQL</span></span>

### <a name="linked-service"></a><span data-ttu-id="b4da5-1334">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-1334">Linked service</span></span>
<span data-ttu-id="b4da5-1335">egy PostgreSQL toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**OnPremisesPostgreSql**, és adja meg a következő tulajdonságok hello **typeProperties**szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1335">toodefine a PostgreSQL linked service, set hello **type** of hello linked service too**OnPremisesPostgreSql**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="b4da5-1336">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1336">Property</span></span> | <span data-ttu-id="b4da5-1337">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1337">Description</span></span> | <span data-ttu-id="b4da5-1338">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1338">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-1339">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="b4da5-1339">server</span></span> |<span data-ttu-id="b4da5-1340">Hello PostgreSQL-kiszolgáló neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1340">Name of hello PostgreSQL server.</span></span> |<span data-ttu-id="b4da5-1341">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1341">Yes</span></span> |
| <span data-ttu-id="b4da5-1342">adatbázis</span><span class="sxs-lookup"><span data-stu-id="b4da5-1342">database</span></span> |<span data-ttu-id="b4da5-1343">Hello PostgreSQL-adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1343">Name of hello PostgreSQL database.</span></span> |<span data-ttu-id="b4da5-1344">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1344">Yes</span></span> |
| <span data-ttu-id="b4da5-1345">Séma</span><span class="sxs-lookup"><span data-stu-id="b4da5-1345">schema</span></span> |<span data-ttu-id="b4da5-1346">Hello séma hello adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1346">Name of hello schema in hello database.</span></span> <span data-ttu-id="b4da5-1347">hello sémanév a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1347">hello schema name is case-sensitive.</span></span> |<span data-ttu-id="b4da5-1348">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1348">No</span></span> |
| <span data-ttu-id="b4da5-1349">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="b4da5-1349">authenticationType</span></span> |<span data-ttu-id="b4da5-1350">Tooconnect toohello PostgreSQL-adatbázishoz használt hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1350">Type of authentication used tooconnect toohello PostgreSQL database.</span></span> <span data-ttu-id="b4da5-1351">Lehetséges értékek a következők: névtelen, alapszintű és a Windows.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1351">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="b4da5-1352">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1352">Yes</span></span> |
| <span data-ttu-id="b4da5-1353">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="b4da5-1353">username</span></span> |<span data-ttu-id="b4da5-1354">Adja meg a felhasználónevet Basic vagy Windows-hitelesítés használata.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1354">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="b4da5-1355">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1355">No</span></span> |
| <span data-ttu-id="b4da5-1356">jelszó</span><span class="sxs-lookup"><span data-stu-id="b4da5-1356">password</span></span> |<span data-ttu-id="b4da5-1357">Adja meg a megadott felhasználónévhez hello hello felhasználói fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1357">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="b4da5-1358">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1358">No</span></span> |
| <span data-ttu-id="b4da5-1359">gatewayName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1359">gatewayName</span></span> |<span data-ttu-id="b4da5-1360">Hello átjáró, amely a Data Factory szolgáltatásnak hello nevét használja tooconnect toohello a helyszíni PostgreSQL-adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1360">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises PostgreSQL database.</span></span> |<span data-ttu-id="b4da5-1361">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1361">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1362">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1362">Example</span></span>

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
<span data-ttu-id="b4da5-1363">További információkért lásd: [PostgreSQL összekötő](data-factory-onprem-postgresql-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1363">For more information, see [PostgreSQL connector](data-factory-onprem-postgresql-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="b4da5-1364">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-1364">Dataset</span></span>
<span data-ttu-id="b4da5-1365">toodefine egy PostgreSQL dataset set hello **típus** hello adatkészlet túl**RelationalTable**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1365">toodefine a PostgreSQL dataset, set hello **type** of hello dataset too**RelationalTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="b4da5-1366">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1366">Property</span></span> | <span data-ttu-id="b4da5-1367">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1367">Description</span></span> | <span data-ttu-id="b4da5-1368">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1368">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-1369">tableName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1369">tableName</span></span> |<span data-ttu-id="b4da5-1370">Hello tábla hello PostgreSQL-adatbázispéldányt, amely a társított szolgáltatás neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1370">Name of hello table in hello PostgreSQL Database instance that linked service refers to.</span></span> <span data-ttu-id="b4da5-1371">hello táblanév a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1371">hello tableName is case-sensitive.</span></span> |<span data-ttu-id="b4da5-1372">Nem (Ha **lekérdezés** a **RelationalSource** van megadva)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1372">No (if **query** of **RelationalSource** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1373">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1373">Example</span></span>
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
<span data-ttu-id="b4da5-1374">További információkért lásd: [PostgreSQL összekötő](data-factory-onprem-postgresql-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1374">For more information, see [PostgreSQL connector](data-factory-onprem-postgresql-connector.md#dataset-properties) article.</span></span>

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="b4da5-1375">A másolási tevékenység relációs adatforrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1375">Relational Source in Copy Activity</span></span>
<span data-ttu-id="b4da5-1376">Adatok másolása egy PostgreSQL-adatbázisból, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**RelationalSource**, és adja meg a következő tulajdonságok hello **forrás**szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1376">If you are copying data from a PostgreSQL database, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="b4da5-1377">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1377">Property</span></span> | <span data-ttu-id="b4da5-1378">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1378">Description</span></span> | <span data-ttu-id="b4da5-1379">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-1379">Allowed values</span></span> | <span data-ttu-id="b4da5-1380">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1380">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-1381">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="b4da5-1381">query</span></span> |<span data-ttu-id="b4da5-1382">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1382">Use hello custom query tooread data.</span></span> |<span data-ttu-id="b4da5-1383">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1383">SQL query string.</span></span> <span data-ttu-id="b4da5-1384">Például: "query": "Válasszon * a \"MySchema\".\" MyTable\"".</span><span class="sxs-lookup"><span data-stu-id="b4da5-1384">For example: "query": "select * from \"MySchema\".\"MyTable\"".</span></span> |<span data-ttu-id="b4da5-1385">Nem (Ha **tableName** a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1385">No (if **tableName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1386">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1386">Example</span></span>

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

<span data-ttu-id="b4da5-1387">További információkért lásd: [PostgreSQL összekötő](data-factory-onprem-postgresql-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1387">For more information, see [PostgreSQL connector](data-factory-onprem-postgresql-connector.md#copy-activity-properties) article.</span></span>

## <a name="sap-business-warehouse"></a><span data-ttu-id="b4da5-1388">SAP Business Warehouse</span><span class="sxs-lookup"><span data-stu-id="b4da5-1388">SAP Business Warehouse</span></span>


### <a name="linked-service"></a><span data-ttu-id="b4da5-1389">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-1389">Linked service</span></span>
<span data-ttu-id="b4da5-1390">egy SAP Business Warehouse (BW) toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**SapBw**, és adja meg a következő tulajdonságok hello **typeProperties**szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1390">toodefine a SAP Business Warehouse (BW) linked service, set hello **type** of hello linked service too**SapBw**, and specify following properties in hello **typeProperties** section:</span></span>  

<span data-ttu-id="b4da5-1391">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1391">Property</span></span> | <span data-ttu-id="b4da5-1392">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1392">Description</span></span> | <span data-ttu-id="b4da5-1393">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-1393">Allowed values</span></span> | <span data-ttu-id="b4da5-1394">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1394">Required</span></span>
-------- | ----------- | -------------- | --------
<span data-ttu-id="b4da5-1395">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="b4da5-1395">server</span></span> | <span data-ttu-id="b4da5-1396">Hello kiszolgálóra mely hello SAP BW példány neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1396">Name of hello server on which hello SAP BW instance resides.</span></span> | <span data-ttu-id="b4da5-1397">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b4da5-1397">string</span></span> | <span data-ttu-id="b4da5-1398">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1398">Yes</span></span>
<span data-ttu-id="b4da5-1399">systemNumber</span><span class="sxs-lookup"><span data-stu-id="b4da5-1399">systemNumber</span></span> | <span data-ttu-id="b4da5-1400">Hello SAP BW rendszer rendszer száma.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1400">System number of hello SAP BW system.</span></span> | <span data-ttu-id="b4da5-1401">Kétjegyű tizedes tört karakterláncból.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1401">Two-digit decimal number represented as a string.</span></span> | <span data-ttu-id="b4da5-1402">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1402">Yes</span></span>
<span data-ttu-id="b4da5-1403">clientId</span><span class="sxs-lookup"><span data-stu-id="b4da5-1403">clientId</span></span> | <span data-ttu-id="b4da5-1404">Hello ügyfél hello SAP W rendszer ügyfél-azonosító.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1404">Client ID of hello client in hello SAP W system.</span></span> | <span data-ttu-id="b4da5-1405">Három számjegyből tizedes tört karakterláncból.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1405">Three-digit decimal number represented as a string.</span></span> | <span data-ttu-id="b4da5-1406">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1406">Yes</span></span>
<span data-ttu-id="b4da5-1407">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="b4da5-1407">username</span></span> | <span data-ttu-id="b4da5-1408">Hello toohello SAP-kiszolgálóhoz hozzáféréssel rendelkező felhasználó neve</span><span class="sxs-lookup"><span data-stu-id="b4da5-1408">Name of hello user who has access toohello SAP server</span></span> | <span data-ttu-id="b4da5-1409">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b4da5-1409">string</span></span> | <span data-ttu-id="b4da5-1410">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1410">Yes</span></span>
<span data-ttu-id="b4da5-1411">jelszó</span><span class="sxs-lookup"><span data-stu-id="b4da5-1411">password</span></span> | <span data-ttu-id="b4da5-1412">Hello felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1412">Password for hello user.</span></span> | <span data-ttu-id="b4da5-1413">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b4da5-1413">string</span></span> | <span data-ttu-id="b4da5-1414">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1414">Yes</span></span>
<span data-ttu-id="b4da5-1415">gatewayName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1415">gatewayName</span></span> | <span data-ttu-id="b4da5-1416">Hello átjáró, amely a Data Factory szolgáltatásnak hello neve tooconnect toohello helyszíni SAP BW példányát kell használnia.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1416">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises SAP BW instance.</span></span> | <span data-ttu-id="b4da5-1417">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b4da5-1417">string</span></span> | <span data-ttu-id="b4da5-1418">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1418">Yes</span></span>
<span data-ttu-id="b4da5-1419">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="b4da5-1419">encryptedCredential</span></span> | <span data-ttu-id="b4da5-1420">hello titkosított hitelesítő adat karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1420">hello encrypted credential string.</span></span> | <span data-ttu-id="b4da5-1421">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b4da5-1421">string</span></span> | <span data-ttu-id="b4da5-1422">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1422">No</span></span>

#### <a name="example"></a><span data-ttu-id="b4da5-1423">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1423">Example</span></span>

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

<span data-ttu-id="b4da5-1424">További információkért lásd: [SAP Business Warehouse-összekötő](data-factory-sap-business-warehouse-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1424">For more information, see [SAP Business Warehouse connector](data-factory-sap-business-warehouse-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="b4da5-1425">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-1425">Dataset</span></span>
<span data-ttu-id="b4da5-1426">toodefine egy SAP BW dataset set hello **típus** hello adatkészlet túl**RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1426">toodefine a SAP BW dataset, set hello **type** of hello dataset too**RelationalTable**.</span></span> <span data-ttu-id="b4da5-1427">Nincsenek hello SAP BW adatkészlet típusú támogatott típusra vonatkozó tulajdonságok **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1427">There are no type-specific properties supported for hello SAP BW dataset of type **RelationalTable**.</span></span>  

#### <a name="example"></a><span data-ttu-id="b4da5-1428">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1428">Example</span></span>

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
<span data-ttu-id="b4da5-1429">További információkért lásd: [SAP Business Warehouse-összekötő](data-factory-sap-business-warehouse-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1429">For more information, see [SAP Business Warehouse connector](data-factory-sap-business-warehouse-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="b4da5-1430">A másolási tevékenység relációs adatforrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1430">Relational Source in Copy Activity</span></span>
<span data-ttu-id="b4da5-1431">Adatok másolása az SAP Business Warehouse, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**RelationalSource**, és adja meg a következő tulajdonságok hello **forrás**szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1431">If you are copying data from SAP Business Warehouse, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="b4da5-1432">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1432">Property</span></span> | <span data-ttu-id="b4da5-1433">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1433">Description</span></span> | <span data-ttu-id="b4da5-1434">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-1434">Allowed values</span></span> | <span data-ttu-id="b4da5-1435">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1435">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-1436">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="b4da5-1436">query</span></span> | <span data-ttu-id="b4da5-1437">Megadja a hello MDX tooread adatait hello SAP BW-példányból.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1437">Specifies hello MDX query tooread data from hello SAP BW instance.</span></span> | <span data-ttu-id="b4da5-1438">MDX-lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1438">MDX query.</span></span> | <span data-ttu-id="b4da5-1439">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1439">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1440">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1440">Example</span></span>

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

<span data-ttu-id="b4da5-1441">További információkért lásd: [SAP Business Warehouse-összekötő](data-factory-sap-business-warehouse-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1441">For more information, see [SAP Business Warehouse connector](data-factory-sap-business-warehouse-connector.md#copy-activity-properties) article.</span></span> 

## <a name="sap-hana"></a><span data-ttu-id="b4da5-1442">SAP HANA</span><span class="sxs-lookup"><span data-stu-id="b4da5-1442">SAP HANA</span></span>

### <a name="linked-service"></a><span data-ttu-id="b4da5-1443">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-1443">Linked service</span></span>
<span data-ttu-id="b4da5-1444">egy SAP HANA toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**SapHana**, és adja meg a következő tulajdonságok hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1444">toodefine a SAP HANA linked service, set hello **type** of hello linked service too**SapHana**, and specify following properties in hello **typeProperties** section:</span></span>  

<span data-ttu-id="b4da5-1445">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1445">Property</span></span> | <span data-ttu-id="b4da5-1446">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1446">Description</span></span> | <span data-ttu-id="b4da5-1447">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-1447">Allowed values</span></span> | <span data-ttu-id="b4da5-1448">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1448">Required</span></span>
-------- | ----------- | -------------- | --------
<span data-ttu-id="b4da5-1449">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="b4da5-1449">server</span></span> | <span data-ttu-id="b4da5-1450">Hello kiszolgálóra mely hello SAP HANA példány neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1450">Name of hello server on which hello SAP HANA instance resides.</span></span> <span data-ttu-id="b4da5-1451">Ha a kiszolgáló egy testreszabott portot használ, adja meg a `server:port`.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1451">If your server is using a customized port, specify `server:port`.</span></span> | <span data-ttu-id="b4da5-1452">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b4da5-1452">string</span></span> | <span data-ttu-id="b4da5-1453">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1453">Yes</span></span>
<span data-ttu-id="b4da5-1454">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="b4da5-1454">authenticationType</span></span> | <span data-ttu-id="b4da5-1455">Hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1455">Type of authentication.</span></span> | <span data-ttu-id="b4da5-1456">Karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1456">string.</span></span> <span data-ttu-id="b4da5-1457">"Basic" vagy "Windows"</span><span class="sxs-lookup"><span data-stu-id="b4da5-1457">"Basic" or "Windows"</span></span> | <span data-ttu-id="b4da5-1458">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1458">Yes</span></span> 
<span data-ttu-id="b4da5-1459">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="b4da5-1459">username</span></span> | <span data-ttu-id="b4da5-1460">Hello toohello SAP-kiszolgálóhoz hozzáféréssel rendelkező felhasználó neve</span><span class="sxs-lookup"><span data-stu-id="b4da5-1460">Name of hello user who has access toohello SAP server</span></span> | <span data-ttu-id="b4da5-1461">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b4da5-1461">string</span></span> | <span data-ttu-id="b4da5-1462">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1462">Yes</span></span>
<span data-ttu-id="b4da5-1463">jelszó</span><span class="sxs-lookup"><span data-stu-id="b4da5-1463">password</span></span> | <span data-ttu-id="b4da5-1464">Hello felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1464">Password for hello user.</span></span> | <span data-ttu-id="b4da5-1465">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b4da5-1465">string</span></span> | <span data-ttu-id="b4da5-1466">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1466">Yes</span></span>
<span data-ttu-id="b4da5-1467">gatewayName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1467">gatewayName</span></span> | <span data-ttu-id="b4da5-1468">Hello átjáró, amely a Data Factory szolgáltatásnak hello neve tooconnect toohello helyszíni SAP HANA-példányt kell használnia.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1468">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises SAP HANA instance.</span></span> | <span data-ttu-id="b4da5-1469">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b4da5-1469">string</span></span> | <span data-ttu-id="b4da5-1470">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1470">Yes</span></span>
<span data-ttu-id="b4da5-1471">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="b4da5-1471">encryptedCredential</span></span> | <span data-ttu-id="b4da5-1472">hello titkosított hitelesítő adat karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1472">hello encrypted credential string.</span></span> | <span data-ttu-id="b4da5-1473">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b4da5-1473">string</span></span> | <span data-ttu-id="b4da5-1474">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1474">No</span></span>

#### <a name="example"></a><span data-ttu-id="b4da5-1475">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1475">Example</span></span>

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
<span data-ttu-id="b4da5-1476">További információkért lásd: [SAP HANA-összekötő](data-factory-sap-hana-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1476">For more information, see [SAP HANA connector](data-factory-sap-hana-connector.md#linked-service-properties) article.</span></span>
 
### <a name="dataset"></a><span data-ttu-id="b4da5-1477">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-1477">Dataset</span></span>
<span data-ttu-id="b4da5-1478">toodefine egy SAP HANA dataset set hello **típus** hello adatkészlet túl**RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1478">toodefine a SAP HANA dataset, set hello **type** of hello dataset too**RelationalTable**.</span></span> <span data-ttu-id="b4da5-1479">Nincsenek hello SAP HANA-adatkészlet típusú támogatott típusra vonatkozó tulajdonságok **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1479">There are no type-specific properties supported for hello SAP HANA dataset of type **RelationalTable**.</span></span> 

#### <a name="example"></a><span data-ttu-id="b4da5-1480">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1480">Example</span></span>

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
<span data-ttu-id="b4da5-1481">További információkért lásd: [SAP HANA-összekötő](data-factory-sap-hana-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1481">For more information, see [SAP HANA connector](data-factory-sap-hana-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="b4da5-1482">A másolási tevékenység relációs adatforrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1482">Relational Source in Copy Activity</span></span>
<span data-ttu-id="b4da5-1483">Adatok másolása egy SAP HANA adattárból, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**RelationalSource**, és adja meg a következő tulajdonságok hello **forrás**szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1483">If you are copying data from a SAP HANA data store, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="b4da5-1484">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1484">Property</span></span> | <span data-ttu-id="b4da5-1485">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1485">Description</span></span> | <span data-ttu-id="b4da5-1486">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-1486">Allowed values</span></span> | <span data-ttu-id="b4da5-1487">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1487">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-1488">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="b4da5-1488">query</span></span> | <span data-ttu-id="b4da5-1489">Megadja a hello SQL lekérdezés tooread adatait hello SAP HANA-példányból.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1489">Specifies hello SQL query tooread data from hello SAP HANA instance.</span></span> | <span data-ttu-id="b4da5-1490">SQL-lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1490">SQL query.</span></span> | <span data-ttu-id="b4da5-1491">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1491">Yes</span></span> |


#### <a name="example"></a><span data-ttu-id="b4da5-1492">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1492">Example</span></span>


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

<span data-ttu-id="b4da5-1493">További információkért lásd: [SAP HANA-összekötő](data-factory-sap-hana-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1493">For more information, see [SAP HANA connector](data-factory-sap-hana-connector.md#copy-activity-properties) article.</span></span>


## <a name="sql-server"></a><span data-ttu-id="b4da5-1494">SQL Server</span><span class="sxs-lookup"><span data-stu-id="b4da5-1494">SQL Server</span></span>

### <a name="linked-service"></a><span data-ttu-id="b4da5-1495">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-1495">Linked service</span></span>
<span data-ttu-id="b4da5-1496">Típusú társított szolgáltatás létrehozása **OnPremisesSqlServer** toolink egy helyi SQL Server adatbázis tooa adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1496">You create a linked service of type **OnPremisesSqlServer** toolink an on-premises SQL Server database tooa data factory.</span></span> <span data-ttu-id="b4da5-1497">a következő táblázat hello biztosít JSON elemek adott tooon-hez kapcsolódó SQL Server szolgáltatás leírását.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1497">hello following table provides description for JSON elements specific tooon-premises SQL Server linked service.</span></span>

<span data-ttu-id="b4da5-1498">a következő táblázat hello biztosít JSON-elemek adott tooSQL csatolt kiszolgáló szolgáltatás leírását.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1498">hello following table provides description for JSON elements specific tooSQL Server linked service.</span></span>

| <span data-ttu-id="b4da5-1499">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1499">Property</span></span> | <span data-ttu-id="b4da5-1500">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1500">Description</span></span> | <span data-ttu-id="b4da5-1501">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1501">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-1502">type</span><span class="sxs-lookup"><span data-stu-id="b4da5-1502">type</span></span> |<span data-ttu-id="b4da5-1503">hello tulajdonságra kell megadni: **OnPremisesSqlServer**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1503">hello type property should be set to: **OnPremisesSqlServer**.</span></span> |<span data-ttu-id="b4da5-1504">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1504">Yes</span></span> |
| <span data-ttu-id="b4da5-1505">connectionString</span><span class="sxs-lookup"><span data-stu-id="b4da5-1505">connectionString</span></span> |<span data-ttu-id="b4da5-1506">Adja meg a connectionString információ tooconnect toohello a helyszíni SQL Server-adatbázis SQL-hitelesítéssel vagy a Windows-hitelesítés szükséges.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1506">Specify connectionString information needed tooconnect toohello on-premises SQL Server database using either SQL authentication or Windows authentication.</span></span> |<span data-ttu-id="b4da5-1507">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1507">Yes</span></span> |
| <span data-ttu-id="b4da5-1508">gatewayName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1508">gatewayName</span></span> |<span data-ttu-id="b4da5-1509">Hello átjáró, amely a Data Factory szolgáltatásnak hello neve tooconnect toohello a helyszíni SQL Server-adatbázist használja.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1509">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises SQL Server database.</span></span> |<span data-ttu-id="b4da5-1510">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1510">Yes</span></span> |
| <span data-ttu-id="b4da5-1511">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="b4da5-1511">username</span></span> |<span data-ttu-id="b4da5-1512">Adja meg a felhasználónevet, ha a Windows-hitelesítést használ.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1512">Specify user name if you are using Windows Authentication.</span></span> <span data-ttu-id="b4da5-1513">Példa: **tartománynév\\felhasználónév**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1513">Example: **domainname\\username**.</span></span> |<span data-ttu-id="b4da5-1514">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1514">No</span></span> |
| <span data-ttu-id="b4da5-1515">jelszó</span><span class="sxs-lookup"><span data-stu-id="b4da5-1515">password</span></span> |<span data-ttu-id="b4da5-1516">Adja meg a megadott felhasználónévhez hello hello felhasználói fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1516">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="b4da5-1517">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1517">No</span></span> |

<span data-ttu-id="b4da5-1518">Hitelesítő adatok hello segítségével titkosíthatja **New-AzureRmDataFactoryEncryptValue** parancsmag, amelyekkel hello kapcsolati karakterlánc, ahogy az alábbi példa hello (**EncryptedCredential** tulajdonság):</span><span class="sxs-lookup"><span data-stu-id="b4da5-1518">You can encrypt credentials using hello **New-AzureRmDataFactoryEncryptValue** cmdlet and use them in hello connection string as shown in hello following example (**EncryptedCredential** property):</span></span>  

```json
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```


#### <a name="example-json-for-using-sql-authentication"></a><span data-ttu-id="b4da5-1519">Példa: JSON az SQL-hitelesítéssel</span><span class="sxs-lookup"><span data-stu-id="b4da5-1519">Example: JSON for using SQL Authentication</span></span>

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
#### <a name="example-json-for-using-windows-authentication"></a><span data-ttu-id="b4da5-1520">Példa: JSON a Windows-hitelesítés használatával</span><span class="sxs-lookup"><span data-stu-id="b4da5-1520">Example: JSON for using Windows Authentication</span></span>

<span data-ttu-id="b4da5-1521">Ha a felhasználónév és jelszó van adva, átjáró használja őket tooimpersonate hello megadott felhasználói fiók tooconnect toohello a helyszíni SQL Server-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1521">If username and password are specified, gateway uses them tooimpersonate hello specified user account tooconnect toohello on-premises SQL Server database.</span></span> <span data-ttu-id="b4da5-1522">Ellenkező esetben átjáró csatlakozik toohello SQL Server közvetlenül (az indítási fiókjához) átjáró hello biztonsági környezetében.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1522">Otherwise, gateway connects toohello SQL Server directly with hello security context of Gateway (its startup account).</span></span>

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

<span data-ttu-id="b4da5-1523">További információkért lásd: [SQL Server-összekötő](data-factory-sqlserver-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1523">For more information, see [SQL Server connector](data-factory-sqlserver-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="b4da5-1524">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-1524">Dataset</span></span>
<span data-ttu-id="b4da5-1525">toodefine egy SQL Server dataset set hello **típus** hello adatkészlet túl**SqlServerTable**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1525">toodefine a SQL Server dataset, set hello **type** of hello dataset too**SqlServerTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="b4da5-1526">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1526">Property</span></span> | <span data-ttu-id="b4da5-1527">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1527">Description</span></span> | <span data-ttu-id="b4da5-1528">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1528">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-1529">tableName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1529">tableName</span></span> |<span data-ttu-id="b4da5-1530">Hello tábla vagy nézet hello SQL Server-adatbázispéldány, amelyre a társított szolgáltatás neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1530">Name of hello table or view in hello SQL Server Database instance that linked service refers to.</span></span> |<span data-ttu-id="b4da5-1531">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1531">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1532">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1532">Example</span></span>
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

<span data-ttu-id="b4da5-1533">További információkért lásd: [SQL Server-összekötő](data-factory-sqlserver-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1533">For more information, see [SQL Server connector](data-factory-sqlserver-connector.md#dataset-properties) article.</span></span> 

### <a name="sql-source-in-copy-activity"></a><span data-ttu-id="b4da5-1534">A másolási tevékenység SQL-forrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1534">Sql Source in Copy Activity</span></span>
<span data-ttu-id="b4da5-1535">Adatok másolása egy SQL Server-adatbázisból, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**SqlSource**, és adja meg a következő tulajdonságok hello **forrás** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1535">If you are copying data from a SQL Server database, set hello **source type** of hello copy activity too**SqlSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="b4da5-1536">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1536">Property</span></span> | <span data-ttu-id="b4da5-1537">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1537">Description</span></span> | <span data-ttu-id="b4da5-1538">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-1538">Allowed values</span></span> | <span data-ttu-id="b4da5-1539">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1539">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-1540">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="b4da5-1540">sqlReaderQuery</span></span> |<span data-ttu-id="b4da5-1541">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1541">Use hello custom query tooread data.</span></span> |<span data-ttu-id="b4da5-1542">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1542">SQL query string.</span></span> <span data-ttu-id="b4da5-1543">Például: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1543">For example: `select * from MyTable`.</span></span> <span data-ttu-id="b4da5-1544">Hivatkozhatnak hello bemeneti adatkészlet által hivatkozott hello adatbázisból táblákat.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1544">May reference multiple tables from hello database referenced by hello input dataset.</span></span> <span data-ttu-id="b4da5-1545">Ha nincs megadva, az SQL-utasítás végrehajtása hello: táblanév kiválaszthatja.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1545">If not specified, hello SQL statement that is executed: select from MyTable.</span></span> |<span data-ttu-id="b4da5-1546">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1546">No</span></span> |
| <span data-ttu-id="b4da5-1547">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1547">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="b4da5-1548">Hello neve tárolt eljárást, amely hello forrástábla olvassa be az adatokat.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1548">Name of hello stored procedure that reads data from hello source table.</span></span> |<span data-ttu-id="b4da5-1549">Hello neve tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1549">Name of hello stored procedure.</span></span> |<span data-ttu-id="b4da5-1550">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1550">No</span></span> |
| <span data-ttu-id="b4da5-1551">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="b4da5-1551">storedProcedureParameters</span></span> |<span data-ttu-id="b4da5-1552">Hello paramétereinek tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1552">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="b4da5-1553">A név/érték párok.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1553">Name/value pairs.</span></span> <span data-ttu-id="b4da5-1554">Nevek és a kis-és paraméterek meg kell egyeznie hello nevét és a kis-és nagybetűhasználat hello tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1554">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="b4da5-1555">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1555">No</span></span> |

<span data-ttu-id="b4da5-1556">Ha hello **sqlReaderQuery** megadott hello SqlSource, hello másolási tevékenység során ez a lekérdezés futtatása hello SQL Server-adatbázis forrás tooget hello adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1556">If hello **sqlReaderQuery** is specified for hello SqlSource, hello Copy Activity runs this query against hello SQL Server Database source tooget hello data.</span></span>

<span data-ttu-id="b4da5-1557">Másik lehetőségként megadhat tárolt eljárás hello megadásával **sqlReaderStoredProcedureName** és **storedProcedureParameters** (ha hello tárolt eljárás paraméterek fogadja el).</span><span class="sxs-lookup"><span data-stu-id="b4da5-1557">Alternatively, you can specify a stored procedure by specifying hello **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if hello stored procedure takes parameters).</span></span>

<span data-ttu-id="b4da5-1558">Ha nem ad meg sqlReaderQuery vagy sqlReaderStoredProcedureName, hello struktúra szakaszban meghatározott hello oszlopok használt toobuild a select lekérdezés toorun elleni hello SQL Server-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1558">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, hello columns defined in hello structure section are used toobuild a select query toorun against hello SQL Server Database.</span></span> <span data-ttu-id="b4da5-1559">Hello adatkészlet definíciója nem rendelkezik hello struktúra, ha minden kiválasztott oszlop. a hello táblából.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1559">If hello dataset definition does not have hello structure, all columns are selected from hello table.</span></span>

> [!NOTE]
> <span data-ttu-id="b4da5-1560">Használata esetén **sqlReaderStoredProcedureName**, továbbra is szükséges toospecify értéket hello **tableName** hello adatkészlet JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1560">When you use **sqlReaderStoredProcedureName**, you still need toospecify a value for hello **tableName** property in hello dataset JSON.</span></span> <span data-ttu-id="b4da5-1561">Nincs érvényesítést hajt végre ezt a táblázatot, ha van.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1561">There are no validations performed against this table though.</span></span>


#### <a name="example"></a><span data-ttu-id="b4da5-1562">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1562">Example</span></span>
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

<span data-ttu-id="b4da5-1563">Ebben a példában **sqlReaderQuery** hello SqlSource van megadva.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1563">In this example, **sqlReaderQuery** is specified for hello SqlSource.</span></span> <span data-ttu-id="b4da5-1564">hello másolási tevékenység fut ez a lekérdezés hello hello tooget SQL Server-adatbázis forrásadatot.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1564">hello Copy Activity runs this query against hello SQL Server Database source tooget hello data.</span></span> <span data-ttu-id="b4da5-1565">Másik lehetőségként megadhat tárolt eljárás hello megadásával **sqlReaderStoredProcedureName** és **storedProcedureParameters** (ha hello tárolt eljárás paraméterek fogadja el).</span><span class="sxs-lookup"><span data-stu-id="b4da5-1565">Alternatively, you can specify a stored procedure by specifying hello **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if hello stored procedure takes parameters).</span></span> <span data-ttu-id="b4da5-1566">hello sqlReaderQuery hello bemeneti adatkészlet által hivatkozott hello adatbázison belül több táblát is hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1566">hello sqlReaderQuery can reference multiple tables within hello database referenced by hello input dataset.</span></span> <span data-ttu-id="b4da5-1567">Már nem korlátozott tooonly hello tábla adatkészlet tableName typeProperty hello beállítani.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1567">It is not limited tooonly hello table set as hello dataset's tableName typeProperty.</span></span>

<span data-ttu-id="b4da5-1568">Ha nem ad meg sqlReaderQuery vagy sqlReaderStoredProcedureName, hello struktúra szakaszban meghatározott hello oszlopok használt toobuild a select lekérdezés toorun elleni hello SQL Server-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1568">If you do not specify sqlReaderQuery or sqlReaderStoredProcedureName, hello columns defined in hello structure section are used toobuild a select query toorun against hello SQL Server Database.</span></span> <span data-ttu-id="b4da5-1569">Hello adatkészlet definíciója nem rendelkezik hello struktúra, ha minden kiválasztott oszlop. a hello táblából.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1569">If hello dataset definition does not have hello structure, all columns are selected from hello table.</span></span>

<span data-ttu-id="b4da5-1570">További információkért lásd: [SQL Server-összekötő](data-factory-sqlserver-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1570">For more information, see [SQL Server connector](data-factory-sqlserver-connector.md#copy-activity-properties) article.</span></span> 

### <a name="sql-sink-in-copy-activity"></a><span data-ttu-id="b4da5-1571">A másolási tevékenység SQL fogadó</span><span class="sxs-lookup"><span data-stu-id="b4da5-1571">Sql Sink in Copy Activity</span></span>
<span data-ttu-id="b4da5-1572">Adatok tooa SQL Server-adatbázis másolása, állítsa be a hello **típus gyűjtése** hello a másolási tevékenység túl**SqlSink**, és adja meg a következő tulajdonságok hello **fogadó** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1572">If you are copying data tooa SQL Server database, set hello **sink type** of hello copy activity too**SqlSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="b4da5-1573">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1573">Property</span></span> | <span data-ttu-id="b4da5-1574">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1574">Description</span></span> | <span data-ttu-id="b4da5-1575">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-1575">Allowed values</span></span> | <span data-ttu-id="b4da5-1576">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1576">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-1577">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="b4da5-1577">writeBatchTimeout</span></span> |<span data-ttu-id="b4da5-1578">Várnia kell az hello kötegelt beszúrási művelet toocomplete előtt azt az időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1578">Wait time for hello batch insert operation toocomplete before it times out.</span></span> |<span data-ttu-id="b4da5-1579">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="b4da5-1579">timespan</span></span><br/><br/> <span data-ttu-id="b4da5-1580">Példa: "00: 30:00" (30 perc).</span><span class="sxs-lookup"><span data-stu-id="b4da5-1580">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="b4da5-1581">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1581">No</span></span> |
| <span data-ttu-id="b4da5-1582">WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="b4da5-1582">writeBatchSize</span></span> |<span data-ttu-id="b4da5-1583">Amikor hello puffer mérete eléri writeBatchSize adatok beillesztése hello SQL táblázat.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1583">Inserts data into hello SQL table when hello buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="b4da5-1584">Egész szám (sorok száma)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1584">Integer (number of rows)</span></span> |<span data-ttu-id="b4da5-1585">Nem (alapértelmezett: 10000)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1585">No (default: 10000)</span></span> |
| <span data-ttu-id="b4da5-1586">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="b4da5-1586">sqlWriterCleanupScript</span></span> |<span data-ttu-id="b4da5-1587">Adja meg a másolási tevékenység tooexecute lekérdezést, úgy, hogy egy adott szelet adatait.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1587">Specify query for Copy Activity tooexecute such that data of a specific slice is cleaned up.</span></span> <span data-ttu-id="b4da5-1588">További információkért lásd: [ismételhetőség](#repeatability-during-copy) szakasz.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1588">For more information, see [repeatability](#repeatability-during-copy) section.</span></span> |<span data-ttu-id="b4da5-1589">A lekérdezési utasítást.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1589">A query statement.</span></span> |<span data-ttu-id="b4da5-1590">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1590">No</span></span> |
| <span data-ttu-id="b4da5-1591">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1591">sliceIdentifierColumnName</span></span> |<span data-ttu-id="b4da5-1592">Adja meg, a másolási tevékenység toofill oszlopnév automatikusan létrejön szelet azonosítóval, amely adatokat egy adott szelet, amikor futtassa újra a használt tooclean.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1592">Specify column name for Copy Activity toofill with auto generated slice identifier, which is used tooclean up data of a specific slice when rerun.</span></span> <span data-ttu-id="b4da5-1593">További információkért lásd: [ismételhetőség](#repeatability-during-copy) szakasz.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1593">For more information, see [repeatability](#repeatability-during-copy) section.</span></span> |<span data-ttu-id="b4da5-1594">Egy oszlop binary(32) adattípusú oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1594">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="b4da5-1595">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1595">No</span></span> |
| <span data-ttu-id="b4da5-1596">sqlWriterStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1596">sqlWriterStoredProcedureName</span></span> |<span data-ttu-id="b4da5-1597">Hello nevét (frissítés/Beszúrás) upserts adatok tárolt eljárás hello cél táblába.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1597">Name of hello stored procedure that upserts (updates/inserts) data into hello target table.</span></span> |<span data-ttu-id="b4da5-1598">Hello neve tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1598">Name of hello stored procedure.</span></span> |<span data-ttu-id="b4da5-1599">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1599">No</span></span> |
| <span data-ttu-id="b4da5-1600">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="b4da5-1600">storedProcedureParameters</span></span> |<span data-ttu-id="b4da5-1601">Hello paramétereinek tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1601">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="b4da5-1602">A név/érték párok.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1602">Name/value pairs.</span></span> <span data-ttu-id="b4da5-1603">Nevek és a kis-és paraméterek meg kell egyeznie hello nevét és a kis-és nagybetűhasználat hello tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1603">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="b4da5-1604">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1604">No</span></span> |
| <span data-ttu-id="b4da5-1605">sqlWriterTableType</span><span class="sxs-lookup"><span data-stu-id="b4da5-1605">sqlWriterTableType</span></span> |<span data-ttu-id="b4da5-1606">Adja meg a tábla Típus neve toobe hello tárolt eljárásban használt.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1606">Specify table type name toobe used in hello stored procedure.</span></span> <span data-ttu-id="b4da5-1607">Másolási tevékenység elérhetővé teszi hello adatok éppen áthelyezik egy ideiglenes táblát, amely a táblatípus.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1607">Copy activity makes hello data being moved available in a temp table with this table type.</span></span> <span data-ttu-id="b4da5-1608">Tárolt eljárás kód majd egyesítheti a meglévő adatok másolásának hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1608">Stored procedure code can then merge hello data being copied with existing data.</span></span> |<span data-ttu-id="b4da5-1609">Egy tábla környezettípus nevét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1609">A table type name.</span></span> |<span data-ttu-id="b4da5-1610">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1610">No</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1611">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1611">Example</span></span>
<span data-ttu-id="b4da5-1612">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse ezen bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1612">hello pipeline contains a Copy Activity that is configured toouse these input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="b4da5-1613">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**BlobSource** és **fogadó** típusuk értéke túl**SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1613">In hello pipeline JSON definition, hello **source** type is set too**BlobSource** and **sink** type is set too**SqlSink**.</span></span>

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

<span data-ttu-id="b4da5-1614">További információkért lásd: [SQL Server-összekötő](data-factory-sqlserver-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1614">For more information, see [SQL Server connector](data-factory-sqlserver-connector.md#copy-activity-properties) article.</span></span> 

## <a name="sybase"></a><span data-ttu-id="b4da5-1615">Sybase</span><span class="sxs-lookup"><span data-stu-id="b4da5-1615">Sybase</span></span>

### <a name="linked-service"></a><span data-ttu-id="b4da5-1616">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-1616">Linked service</span></span>
<span data-ttu-id="b4da5-1617">egy Sybase toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**OnPremisesSybase**, és adja meg a következő tulajdonságok hello **typeProperties** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1617">toodefine a Sybase linked service, set hello **type** of hello linked service too**OnPremisesSybase**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="b4da5-1618">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1618">Property</span></span> | <span data-ttu-id="b4da5-1619">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1619">Description</span></span> | <span data-ttu-id="b4da5-1620">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1620">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-1621">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="b4da5-1621">server</span></span> |<span data-ttu-id="b4da5-1622">Hello Sybase-kiszolgáló neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1622">Name of hello Sybase server.</span></span> |<span data-ttu-id="b4da5-1623">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1623">Yes</span></span> |
| <span data-ttu-id="b4da5-1624">adatbázis</span><span class="sxs-lookup"><span data-stu-id="b4da5-1624">database</span></span> |<span data-ttu-id="b4da5-1625">Hello Sybase-adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1625">Name of hello Sybase database.</span></span> |<span data-ttu-id="b4da5-1626">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1626">Yes</span></span> |
| <span data-ttu-id="b4da5-1627">Séma</span><span class="sxs-lookup"><span data-stu-id="b4da5-1627">schema</span></span> |<span data-ttu-id="b4da5-1628">Hello séma hello adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1628">Name of hello schema in hello database.</span></span> |<span data-ttu-id="b4da5-1629">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1629">No</span></span> |
| <span data-ttu-id="b4da5-1630">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="b4da5-1630">authenticationType</span></span> |<span data-ttu-id="b4da5-1631">Tooconnect toohello Sybase-adatbázishoz használt hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1631">Type of authentication used tooconnect toohello Sybase database.</span></span> <span data-ttu-id="b4da5-1632">Lehetséges értékek a következők: névtelen, alapszintű és a Windows.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1632">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="b4da5-1633">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1633">Yes</span></span> |
| <span data-ttu-id="b4da5-1634">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="b4da5-1634">username</span></span> |<span data-ttu-id="b4da5-1635">Adja meg a felhasználónevet Basic vagy Windows-hitelesítés használata.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1635">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="b4da5-1636">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1636">No</span></span> |
| <span data-ttu-id="b4da5-1637">jelszó</span><span class="sxs-lookup"><span data-stu-id="b4da5-1637">password</span></span> |<span data-ttu-id="b4da5-1638">Adja meg a megadott felhasználónévhez hello hello felhasználói fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1638">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="b4da5-1639">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1639">No</span></span> |
| <span data-ttu-id="b4da5-1640">gatewayName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1640">gatewayName</span></span> |<span data-ttu-id="b4da5-1641">Hello átjáró, amely a Data Factory szolgáltatásnak hello nevét használja tooconnect toohello helyszíni Sybase-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1641">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises Sybase database.</span></span> |<span data-ttu-id="b4da5-1642">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1642">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1643">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1643">Example</span></span>
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

<span data-ttu-id="b4da5-1644">További információkért lásd: [Sybase összekötő](data-factory-onprem-sybase-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1644">For more information, see [Sybase connector](data-factory-onprem-sybase-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="b4da5-1645">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-1645">Dataset</span></span>
<span data-ttu-id="b4da5-1646">toodefine egy Sybase dataset set hello **típus** hello adatkészlet túl**RelationalTable**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1646">toodefine a Sybase dataset, set hello **type** of hello dataset too**RelationalTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="b4da5-1647">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1647">Property</span></span> | <span data-ttu-id="b4da5-1648">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1648">Description</span></span> | <span data-ttu-id="b4da5-1649">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1649">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-1650">tableName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1650">tableName</span></span> |<span data-ttu-id="b4da5-1651">Hello tábla hello Sybase-adatbázispéldányt, amely a társított szolgáltatás neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1651">Name of hello table in hello Sybase Database instance that linked service refers to.</span></span> |<span data-ttu-id="b4da5-1652">Nem (Ha **lekérdezés** a **RelationalSource** van megadva)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1652">No (if **query** of **RelationalSource** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1653">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1653">Example</span></span>

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

<span data-ttu-id="b4da5-1654">További információkért lásd: [Sybase összekötő](data-factory-onprem-sybase-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1654">For more information, see [Sybase connector](data-factory-onprem-sybase-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="b4da5-1655">A másolási tevékenység relációs adatforrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1655">Relational Source in Copy Activity</span></span>
<span data-ttu-id="b4da5-1656">Adatok másolása egy Sybase-adatbázisból, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**RelationalSource**, és adja meg a következő tulajdonságok hello **forrás** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1656">If you are copying data from a Sybase database, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="b4da5-1657">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1657">Property</span></span> | <span data-ttu-id="b4da5-1658">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1658">Description</span></span> | <span data-ttu-id="b4da5-1659">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-1659">Allowed values</span></span> | <span data-ttu-id="b4da5-1660">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1660">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-1661">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="b4da5-1661">query</span></span> |<span data-ttu-id="b4da5-1662">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1662">Use hello custom query tooread data.</span></span> |<span data-ttu-id="b4da5-1663">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1663">SQL query string.</span></span> <span data-ttu-id="b4da5-1664">Például: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1664">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="b4da5-1665">Nem (Ha **tableName** a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1665">No (if **tableName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1666">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1666">Example</span></span>

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

<span data-ttu-id="b4da5-1667">További információkért lásd: [Sybase összekötő](data-factory-onprem-sybase-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1667">For more information, see [Sybase connector](data-factory-onprem-sybase-connector.md#copy-activity-properties) article.</span></span>

## <a name="teradata"></a><span data-ttu-id="b4da5-1668">Teradata</span><span class="sxs-lookup"><span data-stu-id="b4da5-1668">Teradata</span></span>

### <a name="linked-service"></a><span data-ttu-id="b4da5-1669">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-1669">Linked service</span></span>
<span data-ttu-id="b4da5-1670">a Teradata toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**OnPremisesTeradata**, és adja meg a következő tulajdonságok hello **typeProperties** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1670">toodefine a Teradata linked service, set hello **type** of hello linked service too**OnPremisesTeradata**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="b4da5-1671">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1671">Property</span></span> | <span data-ttu-id="b4da5-1672">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1672">Description</span></span> | <span data-ttu-id="b4da5-1673">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1673">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-1674">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="b4da5-1674">server</span></span> |<span data-ttu-id="b4da5-1675">Hello Teradata-kiszolgáló neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1675">Name of hello Teradata server.</span></span> |<span data-ttu-id="b4da5-1676">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1676">Yes</span></span> |
| <span data-ttu-id="b4da5-1677">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="b4da5-1677">authenticationType</span></span> |<span data-ttu-id="b4da5-1678">Tooconnect toohello Teradata-adatbázishoz használt hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1678">Type of authentication used tooconnect toohello Teradata database.</span></span> <span data-ttu-id="b4da5-1679">Lehetséges értékek a következők: névtelen, alapszintű és a Windows.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1679">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="b4da5-1680">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1680">Yes</span></span> |
| <span data-ttu-id="b4da5-1681">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="b4da5-1681">username</span></span> |<span data-ttu-id="b4da5-1682">Adja meg a felhasználónevet Basic vagy Windows-hitelesítés használata.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1682">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="b4da5-1683">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1683">No</span></span> |
| <span data-ttu-id="b4da5-1684">jelszó</span><span class="sxs-lookup"><span data-stu-id="b4da5-1684">password</span></span> |<span data-ttu-id="b4da5-1685">Adja meg a megadott felhasználónévhez hello hello felhasználói fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1685">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="b4da5-1686">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1686">No</span></span> |
| <span data-ttu-id="b4da5-1687">gatewayName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1687">gatewayName</span></span> |<span data-ttu-id="b4da5-1688">Hello átjáró, amely a Data Factory szolgáltatásnak hello nevét használja tooconnect toohello a helyszíni Teradata-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1688">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises Teradata database.</span></span> |<span data-ttu-id="b4da5-1689">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1689">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1690">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1690">Example</span></span>
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

<span data-ttu-id="b4da5-1691">További információkért lásd: [Teradata összekötő](data-factory-onprem-teradata-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1691">For more information, see [Teradata connector](data-factory-onprem-teradata-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="b4da5-1692">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-1692">Dataset</span></span>
<span data-ttu-id="b4da5-1693">toodefine egy Teradata-Blob adatkészletet set hello **típus** hello adatkészlet túl**RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1693">toodefine a Teradata Blob dataset, set hello **type** of hello dataset too**RelationalTable**.</span></span> <span data-ttu-id="b4da5-1694">Jelenleg nem támogatott hello Teradata adatkészlet tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1694">Currently, there are no type properties supported for hello Teradata dataset.</span></span> 

#### <a name="example"></a><span data-ttu-id="b4da5-1695">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1695">Example</span></span>
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

<span data-ttu-id="b4da5-1696">További információkért lásd: [Teradata összekötő](data-factory-onprem-teradata-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1696">For more information, see [Teradata connector](data-factory-onprem-teradata-connector.md#dataset-properties) article.</span></span>

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="b4da5-1697">A másolási tevékenység relációs adatforrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1697">Relational Source in Copy Activity</span></span>
<span data-ttu-id="b4da5-1698">Adatok másolása egy Teradata-adatbázisból, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**RelationalSource**, és adja meg a következő tulajdonságok hello **forrás**szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1698">If you are copying data from a Teradata database, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="b4da5-1699">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1699">Property</span></span> | <span data-ttu-id="b4da5-1700">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1700">Description</span></span> | <span data-ttu-id="b4da5-1701">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-1701">Allowed values</span></span> | <span data-ttu-id="b4da5-1702">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1702">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-1703">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="b4da5-1703">query</span></span> |<span data-ttu-id="b4da5-1704">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1704">Use hello custom query tooread data.</span></span> |<span data-ttu-id="b4da5-1705">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1705">SQL query string.</span></span> <span data-ttu-id="b4da5-1706">Például: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1706">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="b4da5-1707">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1707">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1708">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1708">Example</span></span>

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

<span data-ttu-id="b4da5-1709">További információkért lásd: [Teradata összekötő](data-factory-onprem-teradata-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1709">For more information, see [Teradata connector](data-factory-onprem-teradata-connector.md#copy-activity-properties) article.</span></span>

## <a name="cassandra"></a><span data-ttu-id="b4da5-1710">Cassandra</span><span class="sxs-lookup"><span data-stu-id="b4da5-1710">Cassandra</span></span>


### <a name="linked-service"></a><span data-ttu-id="b4da5-1711">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-1711">Linked service</span></span>
<span data-ttu-id="b4da5-1712">egy Cassandra toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**OnPremisesCassandra**, és adja meg a következő tulajdonságok hello **typeProperties** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1712">toodefine a Cassandra linked service, set hello **type** of hello linked service too**OnPremisesCassandra**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="b4da5-1713">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1713">Property</span></span> | <span data-ttu-id="b4da5-1714">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1714">Description</span></span> | <span data-ttu-id="b4da5-1715">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1715">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-1716">állomás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1716">host</span></span> |<span data-ttu-id="b4da5-1717">Egy vagy több IP-címek vagy Cassandra kiszolgálók állomás nevét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1717">One or more IP addresses or host names of Cassandra servers.</span></span><br/><br/><span data-ttu-id="b4da5-1718">IP-címek vagy nevek tooconnect tooall kiszolgálók vesszővel tagolt listáját adja meg a egyidejűleg.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1718">Specify a comma-separated list of IP addresses or host names tooconnect tooall servers concurrently.</span></span> |<span data-ttu-id="b4da5-1719">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1719">Yes</span></span> |
| <span data-ttu-id="b4da5-1720">port</span><span class="sxs-lookup"><span data-stu-id="b4da5-1720">port</span></span> |<span data-ttu-id="b4da5-1721">hello hello Cassandra server TCP-port toolisten ügyfél-kommunikációhoz használ.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1721">hello TCP port that hello Cassandra server uses toolisten for client connections.</span></span> |<span data-ttu-id="b4da5-1722">Nem, alapértelmezett érték: 9042</span><span class="sxs-lookup"><span data-stu-id="b4da5-1722">No, default value: 9042</span></span> |
| <span data-ttu-id="b4da5-1723">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="b4da5-1723">authenticationType</span></span> |<span data-ttu-id="b4da5-1724">Basic vagy Anonymous</span><span class="sxs-lookup"><span data-stu-id="b4da5-1724">Basic, or Anonymous</span></span> |<span data-ttu-id="b4da5-1725">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1725">Yes</span></span> |
| <span data-ttu-id="b4da5-1726">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="b4da5-1726">username</span></span> |<span data-ttu-id="b4da5-1727">Adja meg a hello felhasználói fiókhoz tartozó felhasználónevet.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1727">Specify user name for hello user account.</span></span> |<span data-ttu-id="b4da5-1728">Igen, ha authenticationType tooBasic van beállítva.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1728">Yes, if authenticationType is set tooBasic.</span></span> |
| <span data-ttu-id="b4da5-1729">jelszó</span><span class="sxs-lookup"><span data-stu-id="b4da5-1729">password</span></span> |<span data-ttu-id="b4da5-1730">Adja meg a hello felhasználói fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1730">Specify password for hello user account.</span></span> |<span data-ttu-id="b4da5-1731">Igen, ha authenticationType tooBasic van beállítva.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1731">Yes, if authenticationType is set tooBasic.</span></span> |
| <span data-ttu-id="b4da5-1732">gatewayName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1732">gatewayName</span></span> |<span data-ttu-id="b4da5-1733">hello hello átjáró, amely használt tooconnect toohello helyszíni Cassandra adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1733">hello name of hello gateway that is used tooconnect toohello on-premises Cassandra database.</span></span> |<span data-ttu-id="b4da5-1734">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1734">Yes</span></span> |
| <span data-ttu-id="b4da5-1735">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="b4da5-1735">encryptedCredential</span></span> |<span data-ttu-id="b4da5-1736">A hitelesítő adatok hello átjáró titkosítja.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1736">Credential encrypted by hello gateway.</span></span> |<span data-ttu-id="b4da5-1737">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1737">No</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1738">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1738">Example</span></span>

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

<span data-ttu-id="b4da5-1739">További információkért lásd: [Cassandra összekötő](data-factory-onprem-cassandra-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1739">For more information, see [Cassandra connector](data-factory-onprem-cassandra-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="b4da5-1740">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-1740">Dataset</span></span>
<span data-ttu-id="b4da5-1741">toodefine Cassandra dataset, set hello **típus** hello adatkészlet túl**CassandraTable**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1741">toodefine a Cassandra dataset, set hello **type** of hello dataset too**CassandraTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="b4da5-1742">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1742">Property</span></span> | <span data-ttu-id="b4da5-1743">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1743">Description</span></span> | <span data-ttu-id="b4da5-1744">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1744">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-1745">kulcstérértesítések használatával</span><span class="sxs-lookup"><span data-stu-id="b4da5-1745">keyspace</span></span> |<span data-ttu-id="b4da5-1746">Hello kulcstérértesítések használatával vagy a séma Cassandra adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1746">Name of hello keyspace or schema in Cassandra database.</span></span> |<span data-ttu-id="b4da5-1747">Igen (Ha **lekérdezés** a **CassandraSource** nincs definiálva).</span><span class="sxs-lookup"><span data-stu-id="b4da5-1747">Yes (If **query** for **CassandraSource** is not defined).</span></span> |
| <span data-ttu-id="b4da5-1748">tableName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1748">tableName</span></span> |<span data-ttu-id="b4da5-1749">Hello tábla Cassandra adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1749">Name of hello table in Cassandra database.</span></span> |<span data-ttu-id="b4da5-1750">Igen (Ha **lekérdezés** a **CassandraSource** nincs definiálva).</span><span class="sxs-lookup"><span data-stu-id="b4da5-1750">Yes (If **query** for **CassandraSource** is not defined).</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1751">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1751">Example</span></span>

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

<span data-ttu-id="b4da5-1752">További információkért lásd: [Cassandra összekötő](data-factory-onprem-cassandra-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1752">For more information, see [Cassandra connector](data-factory-onprem-cassandra-connector.md#dataset-properties) article.</span></span> 

### <a name="cassandra-source-in-copy-activity"></a><span data-ttu-id="b4da5-1753">A másolási tevékenység Cassandra forrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1753">Cassandra Source in Copy Activity</span></span>
<span data-ttu-id="b4da5-1754">Adatok másolása az Cassandra, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**CassandraSource**, és adja meg a következő tulajdonságok hello **forrás** szakasz :</span><span class="sxs-lookup"><span data-stu-id="b4da5-1754">If you are copying data from Cassandra, set hello **source type** of hello copy activity too**CassandraSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="b4da5-1755">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1755">Property</span></span> | <span data-ttu-id="b4da5-1756">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1756">Description</span></span> | <span data-ttu-id="b4da5-1757">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-1757">Allowed values</span></span> | <span data-ttu-id="b4da5-1758">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1758">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-1759">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="b4da5-1759">query</span></span> |<span data-ttu-id="b4da5-1760">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1760">Use hello custom query tooread data.</span></span> |<span data-ttu-id="b4da5-1761">SQL-92 vagy CQL lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1761">SQL-92 query or CQL query.</span></span> <span data-ttu-id="b4da5-1762">Lásd: [CQL hivatkozás](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html).</span><span class="sxs-lookup"><span data-stu-id="b4da5-1762">See [CQL reference](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html).</span></span> <br/><br/><span data-ttu-id="b4da5-1763">SQL-lekérdezés használata esetén adja meg a **kulcstérértesítések használatával name.table neve** tooquery kívánt toorepresent hello tábla.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1763">When using SQL query, specify **keyspace name.table name** toorepresent hello table you want tooquery.</span></span> |<span data-ttu-id="b4da5-1764">Nem (ha van megadva a tableName és a dataset kulcstérértesítések használatával).</span><span class="sxs-lookup"><span data-stu-id="b4da5-1764">No (if tableName and keyspace on dataset are defined).</span></span> |
| <span data-ttu-id="b4da5-1765">consistencyLevel</span><span class="sxs-lookup"><span data-stu-id="b4da5-1765">consistencyLevel</span></span> |<span data-ttu-id="b4da5-1766">hello konzisztencia szint határozza meg, hány replikák tooa olvasási kérelem kell válaszolnia kell a visszatérésre adatok toohello ügyfélalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1766">hello consistency level specifies how many replicas must respond tooa read request before returning data toohello client application.</span></span> <span data-ttu-id="b4da5-1767">Cassandra ellenőrzések hello replikák megadott számú, az adatok toosatisfy hello olvasni a kérelmet.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1767">Cassandra checks hello specified number of replicas for data toosatisfy hello read request.</span></span> |<span data-ttu-id="b4da5-1768">EGY, KETTŐ, HÁROM, KVÓRUM, AZ ÖSSZES, LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1768">ONE, TWO, THREE, QUORUM, ALL, LOCAL_QUORUM, EACH_QUORUM, LOCAL_ONE.</span></span> <span data-ttu-id="b4da5-1769">Lásd: [konfigurálása az adatok konzisztenciájának](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1769">See [Configuring data consistency](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) for details.</span></span> |<span data-ttu-id="b4da5-1770">Nem.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1770">No.</span></span> <span data-ttu-id="b4da5-1771">Alapértelmezett érték: egyet.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1771">Default value is ONE.</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1772">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1772">Example</span></span>
  
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "CassandraToAzureBlob",
            "description": "Copy from Cassandra tooan Azure blob",
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

<span data-ttu-id="b4da5-1773">További információkért lásd: [Cassandra összekötő](data-factory-onprem-cassandra-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1773">For more information, see [Cassandra connector](data-factory-onprem-cassandra-connector.md#copy-activity-properties) article.</span></span>

## <a name="mongodb"></a><span data-ttu-id="b4da5-1774">MongoDB</span><span class="sxs-lookup"><span data-stu-id="b4da5-1774">MongoDB</span></span>

### <a name="linked-service"></a><span data-ttu-id="b4da5-1775">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-1775">Linked service</span></span>
<span data-ttu-id="b4da5-1776">a MongoDB toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**OnPremisesMongoDB**, és adja meg a következő tulajdonságok hello **typeProperties** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1776">toodefine a MongoDB linked service, set hello **type** of hello linked service too**OnPremisesMongoDB**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="b4da5-1777">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1777">Property</span></span> | <span data-ttu-id="b4da5-1778">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1778">Description</span></span> | <span data-ttu-id="b4da5-1779">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1779">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-1780">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="b4da5-1780">server</span></span> |<span data-ttu-id="b4da5-1781">Kiszolgáló IP-címét vagy állomásnevét kiszolgálónevét hello MongoDB.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1781">IP address or host name of hello MongoDB server.</span></span> |<span data-ttu-id="b4da5-1782">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1782">Yes</span></span> |
| <span data-ttu-id="b4da5-1783">port</span><span class="sxs-lookup"><span data-stu-id="b4da5-1783">port</span></span> |<span data-ttu-id="b4da5-1784">TCP-portot, amely a MongoDB-kiszolgálóhoz hello toolisten ügyfél-kommunikációhoz használ.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1784">TCP port that hello MongoDB server uses toolisten for client connections.</span></span> |<span data-ttu-id="b4da5-1785">Nem kötelező, alapértelmezett érték: 27017</span><span class="sxs-lookup"><span data-stu-id="b4da5-1785">Optional, default value: 27017</span></span> |
| <span data-ttu-id="b4da5-1786">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="b4da5-1786">authenticationType</span></span> |<span data-ttu-id="b4da5-1787">Alapszintű, vagy névtelen.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1787">Basic, or Anonymous.</span></span> |<span data-ttu-id="b4da5-1788">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1788">Yes</span></span> |
| <span data-ttu-id="b4da5-1789">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="b4da5-1789">username</span></span> |<span data-ttu-id="b4da5-1790">Felhasználói fiók MongoDB tooaccess.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1790">User account tooaccess MongoDB.</span></span> |<span data-ttu-id="b4da5-1791">Igen (Ha alapszintű hitelesítést használ).</span><span class="sxs-lookup"><span data-stu-id="b4da5-1791">Yes (if basic authentication is used).</span></span> |
| <span data-ttu-id="b4da5-1792">jelszó</span><span class="sxs-lookup"><span data-stu-id="b4da5-1792">password</span></span> |<span data-ttu-id="b4da5-1793">Hello felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1793">Password for hello user.</span></span> |<span data-ttu-id="b4da5-1794">Igen (Ha alapszintű hitelesítést használ).</span><span class="sxs-lookup"><span data-stu-id="b4da5-1794">Yes (if basic authentication is used).</span></span> |
| <span data-ttu-id="b4da5-1795">authSource</span><span class="sxs-lookup"><span data-stu-id="b4da5-1795">authSource</span></span> |<span data-ttu-id="b4da5-1796">Hello MongoDB-adatbázist, amelyet toouse toocheck a hitelesítő adatok hitelesítéshez neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1796">Name of hello MongoDB database that you want toouse toocheck your credentials for authentication.</span></span> |<span data-ttu-id="b4da5-1797">Választható (Ha alapszintű hitelesítést használ).</span><span class="sxs-lookup"><span data-stu-id="b4da5-1797">Optional (if basic authentication is used).</span></span> <span data-ttu-id="b4da5-1798">alapértelmezett: hello rendszergazdai fiókot és a databaseName tulajdonsággal megadott hello adatbázis használ.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1798">default: uses hello admin account and hello database specified using databaseName property.</span></span> |
| <span data-ttu-id="b4da5-1799">DatabaseName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1799">databaseName</span></span> |<span data-ttu-id="b4da5-1800">Hello MongoDB-adatbázist, amelyet az tooaccess neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1800">Name of hello MongoDB database that you want tooaccess.</span></span> |<span data-ttu-id="b4da5-1801">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1801">Yes</span></span> |
| <span data-ttu-id="b4da5-1802">gatewayName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1802">gatewayName</span></span> |<span data-ttu-id="b4da5-1803">Hello adattár hozzáférő hello átjáró neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1803">Name of hello gateway that accesses hello data store.</span></span> |<span data-ttu-id="b4da5-1804">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1804">Yes</span></span> |
| <span data-ttu-id="b4da5-1805">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="b4da5-1805">encryptedCredential</span></span> |<span data-ttu-id="b4da5-1806">A hitelesítőadat-átjáró által titkosított.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1806">Credential encrypted by gateway.</span></span> |<span data-ttu-id="b4da5-1807">Optional</span><span class="sxs-lookup"><span data-stu-id="b4da5-1807">Optional</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1808">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1808">Example</span></span>

```json
{
    "name": "OnPremisesMongoDbLinkedService",
    "properties": {
        "type": "OnPremisesMongoDb",
        "typeProperties": {
            "authenticationType": "<Basic or Anonymous>",
            "server": "< hello IP address or host name of hello MongoDB server >",
            "port": "<hello number of hello TCP port that hello MongoDB server uses toolisten for client connections.>",
            "username": "<username>",
            "password": "<password>",
            "authSource": "< hello database that you want toouse toocheck your credentials for authentication. >",
            "databaseName": "<database name>",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

<span data-ttu-id="b4da5-1809">További információkért lásd: [MongoDB összekötő cikk](data-factory-on-premises-mongodb-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1809">For more information, see [MongoDB connector article](data-factory-on-premises-mongodb-connector.md#linked-service-properties)</span></span>

### <a name="dataset"></a><span data-ttu-id="b4da5-1810">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-1810">Dataset</span></span>
<span data-ttu-id="b4da5-1811">toodefine a MongoDB dataset set hello **típus** hello adatkészlet túl**MongoDbCollection**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1811">toodefine a MongoDB dataset, set hello **type** of hello dataset too**MongoDbCollection**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="b4da5-1812">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1812">Property</span></span> | <span data-ttu-id="b4da5-1813">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1813">Description</span></span> | <span data-ttu-id="b4da5-1814">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1814">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-1815">CollectionName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1815">collectionName</span></span> |<span data-ttu-id="b4da5-1816">A MongoDB adatbázis hello gyűjtemény nevét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1816">Name of hello collection in MongoDB database.</span></span> |<span data-ttu-id="b4da5-1817">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1817">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1818">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1818">Example</span></span>

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

<span data-ttu-id="b4da5-1819">További információkért lásd: [MongoDB összekötő cikk](data-factory-on-premises-mongodb-connector.md#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1819">For more information, see [MongoDB connector article](data-factory-on-premises-mongodb-connector.md#dataset-properties)</span></span>

#### <a name="mongodb-source-in-copy-activity"></a><span data-ttu-id="b4da5-1820">A másolási tevékenység MongoDB-forrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1820">MongoDB Source in Copy Activity</span></span>
<span data-ttu-id="b4da5-1821">Adatok másolása a MongoDB, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**MongoDbSource**, és adja meg a következő tulajdonságok hello **forrás** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1821">If you are copying data from MongoDB, set hello **source type** of hello copy activity too**MongoDbSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="b4da5-1822">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1822">Property</span></span> | <span data-ttu-id="b4da5-1823">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1823">Description</span></span> | <span data-ttu-id="b4da5-1824">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-1824">Allowed values</span></span> | <span data-ttu-id="b4da5-1825">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1825">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-1826">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="b4da5-1826">query</span></span> |<span data-ttu-id="b4da5-1827">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1827">Use hello custom query tooread data.</span></span> |<span data-ttu-id="b4da5-1828">SQL-92 lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1828">SQL-92 query string.</span></span> <span data-ttu-id="b4da5-1829">Például: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1829">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="b4da5-1830">Nem (Ha **collectionName** a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1830">No (if **collectionName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1831">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1831">Example</span></span>

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

<span data-ttu-id="b4da5-1832">További információkért lásd: [MongoDB összekötő cikk](data-factory-on-premises-mongodb-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1832">For more information, see [MongoDB connector article](data-factory-on-premises-mongodb-connector.md#copy-activity-properties)</span></span>

## <a name="amazon-s3"></a><span data-ttu-id="b4da5-1833">Amazon S3</span><span class="sxs-lookup"><span data-stu-id="b4da5-1833">Amazon S3</span></span>


### <a name="linked-service"></a><span data-ttu-id="b4da5-1834">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-1834">Linked service</span></span>
<span data-ttu-id="b4da5-1835">az Amazon S3 toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**AwsAccessKey**, és adja meg a következő tulajdonságok hello **typeProperties** szakasz :</span><span class="sxs-lookup"><span data-stu-id="b4da5-1835">toodefine an Amazon S3 linked service, set hello **type** of hello linked service too**AwsAccessKey**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="b4da5-1836">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1836">Property</span></span> | <span data-ttu-id="b4da5-1837">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1837">Description</span></span> | <span data-ttu-id="b4da5-1838">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-1838">Allowed values</span></span> | <span data-ttu-id="b4da5-1839">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1839">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-1840">accessKeyID</span><span class="sxs-lookup"><span data-stu-id="b4da5-1840">accessKeyID</span></span> |<span data-ttu-id="b4da5-1841">Hello titkos hívóbetű azonosítója.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1841">ID of hello secret access key.</span></span> |<span data-ttu-id="b4da5-1842">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b4da5-1842">string</span></span> |<span data-ttu-id="b4da5-1843">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1843">Yes</span></span> |
| <span data-ttu-id="b4da5-1844">secretAccessKey</span><span class="sxs-lookup"><span data-stu-id="b4da5-1844">secretAccessKey</span></span> |<span data-ttu-id="b4da5-1845">hello titkos hívóbetű magát.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1845">hello secret access key itself.</span></span> |<span data-ttu-id="b4da5-1846">Titkosított titkos karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b4da5-1846">Encrypted secret string</span></span> |<span data-ttu-id="b4da5-1847">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1847">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-1848">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1848">Example</span></span>
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

<span data-ttu-id="b4da5-1849">További információkért lásd: [Amazon S3 összekötő cikk](data-factory-amazon-simple-storage-service-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="b4da5-1849">For more information, see [Amazon S3 connector article](data-factory-amazon-simple-storage-service-connector.md#linked-service-properties).</span></span>

### <a name="dataset"></a><span data-ttu-id="b4da5-1850">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-1850">Dataset</span></span>
<span data-ttu-id="b4da5-1851">az Amazon S3 toodefine dataset, set hello **típus** hello adatkészlet túl**AmazonS3**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1851">toodefine an Amazon S3 dataset, set hello **type** of hello dataset too**AmazonS3**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="b4da5-1852">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1852">Property</span></span> | <span data-ttu-id="b4da5-1853">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1853">Description</span></span> | <span data-ttu-id="b4da5-1854">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-1854">Allowed values</span></span> | <span data-ttu-id="b4da5-1855">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1855">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-1856">bucketName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1856">bucketName</span></span> |<span data-ttu-id="b4da5-1857">hello S3 gyűjtő neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1857">hello S3 bucket name.</span></span> |<span data-ttu-id="b4da5-1858">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b4da5-1858">String</span></span> |<span data-ttu-id="b4da5-1859">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1859">Yes</span></span> |
| <span data-ttu-id="b4da5-1860">kulcs</span><span class="sxs-lookup"><span data-stu-id="b4da5-1860">key</span></span> |<span data-ttu-id="b4da5-1861">hello S3 objektum kulcsának.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1861">hello S3 object key.</span></span> |<span data-ttu-id="b4da5-1862">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b4da5-1862">String</span></span> |<span data-ttu-id="b4da5-1863">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1863">No</span></span> |
| <span data-ttu-id="b4da5-1864">előtag</span><span class="sxs-lookup"><span data-stu-id="b4da5-1864">prefix</span></span> |<span data-ttu-id="b4da5-1865">Hello S3 Objektumkulcs előtagját.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1865">Prefix for hello S3 object key.</span></span> <span data-ttu-id="b4da5-1866">Kiválasztott objektumok, amelynek kulcsait a előtaggal kezdődik.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1866">Objects whose keys start with this prefix are selected.</span></span> <span data-ttu-id="b4da5-1867">Érvényes, csak ha kulcsa üres.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1867">Applies only when key is empty.</span></span> |<span data-ttu-id="b4da5-1868">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b4da5-1868">String</span></span> |<span data-ttu-id="b4da5-1869">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1869">No</span></span> |
| <span data-ttu-id="b4da5-1870">Verzió</span><span class="sxs-lookup"><span data-stu-id="b4da5-1870">version</span></span> |<span data-ttu-id="b4da5-1871">Ha engedélyezve van a S3 versioning S3 objektum hello verziója.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1871">hello version of S3 object if S3 versioning is enabled.</span></span> |<span data-ttu-id="b4da5-1872">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b4da5-1872">String</span></span> |<span data-ttu-id="b4da5-1873">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1873">No</span></span> |
| <span data-ttu-id="b4da5-1874">Formátumban</span><span class="sxs-lookup"><span data-stu-id="b4da5-1874">format</span></span> | <span data-ttu-id="b4da5-1875">a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1875">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="b4da5-1876">Set hello **típus** tulajdonság alapján formátum tooone ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1876">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="b4da5-1877">További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1877">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="b4da5-1878">Ha azt szeretné, túl**másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a hello formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1878">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="b4da5-1879">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1879">No</span></span> | |
| <span data-ttu-id="b4da5-1880">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="b4da5-1880">compression</span></span> | <span data-ttu-id="b4da5-1881">Adja meg a hello típusát és hello adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1881">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="b4da5-1882">Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1882">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="b4da5-1883">hello támogatott szintek a következők: **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1883">hello supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="b4da5-1884">További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="b4da5-1884">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="b4da5-1885">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1885">No</span></span> | |


> [!NOTE]
> <span data-ttu-id="b4da5-1886">bucketName + kulcs hello S3 objektum, ahol a gyűjtő S3 objektumok hello legfelső szintű tárolója, és a kulcs hello teljes elérési útja tooS3 objektum hello helyét adja meg.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1886">bucketName + key specifies hello location of hello S3 object where bucket is hello root container for S3 objects and key is hello full path tooS3 object.</span></span>

#### <a name="example-sample-dataset-with-prefix"></a><span data-ttu-id="b4da5-1887">Példa: Minta-adatkészleteken előtaggal</span><span class="sxs-lookup"><span data-stu-id="b4da5-1887">Example: Sample dataset with prefix</span></span>

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
#### <a name="example-sample-data-set-with-version"></a><span data-ttu-id="b4da5-1888">Példa: Mintát adatkészletre (verziójával)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1888">Example: Sample data set (with version)</span></span>

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

#### <a name="example-dynamic-paths-for-s3"></a><span data-ttu-id="b4da5-1889">Példa: A dinamikus útvonalak S3</span><span class="sxs-lookup"><span data-stu-id="b4da5-1889">Example: Dynamic paths for S3</span></span>
<span data-ttu-id="b4da5-1890">Hello minta rögzített értékeket használjuk kulcs és bucketName tulajdonságok hello Amazon S3 adatkészletben.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1890">In hello sample, we use fixed values for key and bucketName properties in hello Amazon S3 dataset.</span></span>

```json
"key": "testFolder/test.orc",
"bucketName": "<S3 bucket name>",
```

<span data-ttu-id="b4da5-1891">Hello kulcs, és dinamikusan futásidőben bucketName kiszámításához rendszerváltozók SliceStart például a Data Factory lehet.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1891">You can have Data Factory calculate hello key and bucketName dynamically at runtime by using system variables such as SliceStart.</span></span>

```json
"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"
```

<span data-ttu-id="b4da5-1892">Mindent hello azonos hello előtag tulajdonsága az Amazon S3 adatkészlethez.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1892">You can do hello same for hello prefix property of an Amazon S3 dataset.</span></span> <span data-ttu-id="b4da5-1893">Lásd: [adat-előállító funkciók és rendszerváltozók](data-factory-functions-variables.md) támogatott funkciók és változók listáját.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1893">See [Data Factory functions and system variables](data-factory-functions-variables.md) for a list of supported functions and variables.</span></span>

<span data-ttu-id="b4da5-1894">További információkért lásd: [Amazon S3 összekötő cikk](data-factory-amazon-simple-storage-service-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="b4da5-1894">For more information, see [Amazon S3 connector article](data-factory-amazon-simple-storage-service-connector.md#dataset-properties).</span></span>

### <a name="file-system-source-in-copy-activity"></a><span data-ttu-id="b4da5-1895">A másolási tevékenység rendszer forrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1895">File System Source in Copy Activity</span></span>
<span data-ttu-id="b4da5-1896">Adatok másolása az Amazon S3, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**FileSystemSource**, és adja meg a következő tulajdonságok hello **forrás** szakasz :</span><span class="sxs-lookup"><span data-stu-id="b4da5-1896">If you are copying data from Amazon S3, set hello **source type** of hello copy activity too**FileSystemSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="b4da5-1897">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1897">Property</span></span> | <span data-ttu-id="b4da5-1898">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1898">Description</span></span> | <span data-ttu-id="b4da5-1899">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-1899">Allowed values</span></span> | <span data-ttu-id="b4da5-1900">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1900">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-1901">Rekurzív</span><span class="sxs-lookup"><span data-stu-id="b4da5-1901">recursive</span></span> |<span data-ttu-id="b4da5-1902">Meghatározza, hogy toorecursively lista S3 objektumokat hello könyvtára alatt tárolja.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1902">Specifies whether toorecursively list S3 objects under hello directory.</span></span> |<span data-ttu-id="b4da5-1903">Igaz/hamis</span><span class="sxs-lookup"><span data-stu-id="b4da5-1903">true/false</span></span> |<span data-ttu-id="b4da5-1904">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1904">No</span></span> |


#### <a name="example"></a><span data-ttu-id="b4da5-1905">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1905">Example</span></span>


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

<span data-ttu-id="b4da5-1906">További információkért lásd: [Amazon S3 összekötő cikk](data-factory-amazon-simple-storage-service-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="b4da5-1906">For more information, see [Amazon S3 connector article](data-factory-amazon-simple-storage-service-connector.md#copy-activity-properties).</span></span>

## <a name="file-system"></a><span data-ttu-id="b4da5-1907">Fájlrendszer</span><span class="sxs-lookup"><span data-stu-id="b4da5-1907">File System</span></span>


### <a name="linked-service"></a><span data-ttu-id="b4da5-1908">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-1908">Linked service</span></span>
<span data-ttu-id="b4da5-1909">Egy helyi fájl rendszer tooan az Azure data factory hello társíthatja **a helyi fájlkiszolgáló** társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1909">You can link an on-premises file system tooan Azure data factory with hello **On-Premises File Server** linked service.</span></span> <span data-ttu-id="b4da5-1910">hello a következő táblázat ismerteti, amelyek adott toohello a helyi fájlkiszolgáló társított szolgáltatás JSON-elemek szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1910">hello following table provides descriptions for JSON elements that are specific toohello On-Premises File Server linked service.</span></span>

| <span data-ttu-id="b4da5-1911">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1911">Property</span></span> | <span data-ttu-id="b4da5-1912">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1912">Description</span></span> | <span data-ttu-id="b4da5-1913">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1913">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-1914">type</span><span class="sxs-lookup"><span data-stu-id="b4da5-1914">type</span></span> |<span data-ttu-id="b4da5-1915">Győződjön meg arról, hogy hello típusú tulajdonsága túl**OnPremisesFileServer**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1915">Ensure that hello type property is set too**OnPremisesFileServer**.</span></span> |<span data-ttu-id="b4da5-1916">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1916">Yes</span></span> |
| <span data-ttu-id="b4da5-1917">állomás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1917">host</span></span> |<span data-ttu-id="b4da5-1918">Hello legfelső szintű megjeleníteni kívánt toocopy hello mappa elérési útját adja meg.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1918">Specifies hello root path of hello folder that you want toocopy.</span></span> <span data-ttu-id="b4da5-1919">Hello escape-karakter használata "\" hello karakterlánc speciális karakter.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1919">Use hello escape character ‘ \ ’ for special characters in hello string.</span></span> <span data-ttu-id="b4da5-1920">Lásd: [minta kapcsolódó szolgáltatás és az adatkészlet-definíciók](#sample-linked-service-and-dataset-definitions) példákat.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1920">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span> |<span data-ttu-id="b4da5-1921">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1921">Yes</span></span> |
| <span data-ttu-id="b4da5-1922">felhasználói azonosítóját</span><span class="sxs-lookup"><span data-stu-id="b4da5-1922">userid</span></span> |<span data-ttu-id="b4da5-1923">Adja meg a toohello kiszolgáló hello felhasználó hello Azonosítót.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1923">Specify hello ID of hello user who has access toohello server.</span></span> |<span data-ttu-id="b4da5-1924">Nem (Ha úgy dönt, hogy encryptedCredential)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1924">No (if you choose encryptedCredential)</span></span> |
| <span data-ttu-id="b4da5-1925">jelszó</span><span class="sxs-lookup"><span data-stu-id="b4da5-1925">password</span></span> |<span data-ttu-id="b4da5-1926">Adja meg a hello jelszó hello (userid).</span><span class="sxs-lookup"><span data-stu-id="b4da5-1926">Specify hello password for hello user (userid).</span></span> |<span data-ttu-id="b4da5-1927">Nem (Ha úgy dönt, hogy encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="b4da5-1927">No (if you choose encryptedCredential</span></span> |
| <span data-ttu-id="b4da5-1928">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="b4da5-1928">encryptedCredential</span></span> |<span data-ttu-id="b4da5-1929">Adja meg a hello titkosított hitelesítő adatokat, amelyek a parancsmagot a New-AzureRmDataFactoryEncryptValue hello kaphat.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1929">Specify hello encrypted credentials that you can get by running hello New-AzureRmDataFactoryEncryptValue cmdlet.</span></span> |<span data-ttu-id="b4da5-1930">Nem (Ha úgy dönt, toospecify felhasználói azonosítót és jelszót a szövegként)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1930">No (if you choose toospecify userid and password in plain text)</span></span> |
| <span data-ttu-id="b4da5-1931">gatewayName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1931">gatewayName</span></span> |<span data-ttu-id="b4da5-1932">Megadja, hogy a Data Factory kell használnia tooconnect toohello a helyi fájlkiszolgáló hello átjáró hello nevét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1932">Specifies hello name of hello gateway that Data Factory should use tooconnect toohello on-premises file server.</span></span> |<span data-ttu-id="b4da5-1933">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1933">Yes</span></span> |

#### <a name="sample-folder-path-definitions"></a><span data-ttu-id="b4da5-1934">A minta mappa elérési útja definíciók</span><span class="sxs-lookup"><span data-stu-id="b4da5-1934">Sample folder path definitions</span></span> 
| <span data-ttu-id="b4da5-1935">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="b4da5-1935">Scenario</span></span> | <span data-ttu-id="b4da5-1936">A társított szolgáltatás definíciójának üzemeltetéséhez</span><span class="sxs-lookup"><span data-stu-id="b4da5-1936">Host in linked service definition</span></span> | <span data-ttu-id="b4da5-1937">Az adatkészlet-definícióban folderPath</span><span class="sxs-lookup"><span data-stu-id="b4da5-1937">folderPath in dataset definition</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-1938">Az adatkezelési átjáró gépen helyi mappában:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1938">Local folder on Data Management Gateway machine:</span></span> <br/><br/><span data-ttu-id="b4da5-1939">Példák: D:\\ \* vagy D:\folder\subfolder\\*</span><span class="sxs-lookup"><span data-stu-id="b4da5-1939">Examples: D:\\\* or D:\folder\subfolder\\*</span></span> |<span data-ttu-id="b4da5-1940">D:\\ \\ (az adatok felügyeleti átjáró 2.0-s és újabb verziók)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1940">D:\\\\ (for Data Management Gateway 2.0 and later versions)</span></span> <br/><br/> <span data-ttu-id="b4da5-1941">a localhost (korábbi verzióihoz mint adatok felügyeleti átjáró 2.0-s)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1941">localhost (for earlier versions than Data Management Gateway 2.0)</span></span> |<span data-ttu-id="b4da5-1942">. \\ \\ vagy mappa\\\\almappa (az adatok felügyeleti átjáró 2.0-s és újabb verziók)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1942">.\\\\ or folder\\\\subfolder (for Data Management Gateway 2.0 and later versions)</span></span> <br/><br/><span data-ttu-id="b4da5-1943">D:\\ \\ vagy D:\\\\mappa\\\\almappa (az átjáró verziója alatt 2.0-s)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1943">D:\\\\ or D:\\\\folder\\\\subfolder (for gateway version below 2.0)</span></span> |
| <span data-ttu-id="b4da5-1944">Távoli megosztott mappa:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1944">Remote shared folder:</span></span> <br/><br/><span data-ttu-id="b4da5-1945">Példák: \\ \\myserver\\megosztása\\ \* vagy \\ \\myserver\\megosztása\\mappa\\almappa\\*</span><span class="sxs-lookup"><span data-stu-id="b4da5-1945">Examples: \\\\myserver\\share\\\* or \\\\myserver\\share\\folder\\subfolder\\*</span></span> |<span data-ttu-id="b4da5-1946">\\\\\\\\myserver\\\\megosztása</span><span class="sxs-lookup"><span data-stu-id="b4da5-1946">\\\\\\\\myserver\\\\share</span></span> |<span data-ttu-id="b4da5-1947">. \\ \\ vagy mappa\\\\almappa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1947">.\\\\ or folder\\\\subfolder</span></span> |


#### <a name="example-using-username-and-password-in-plain-text"></a><span data-ttu-id="b4da5-1948">Példa: Felhasználónév és jelszó használatával egyszerű szöveges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1948">Example: Using username and password in plain text</span></span>

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

#### <a name="example-using-encryptedcredential"></a><span data-ttu-id="b4da5-1949">Példa: Encryptedcredential használatával</span><span class="sxs-lookup"><span data-stu-id="b4da5-1949">Example: Using encryptedcredential</span></span>

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

<span data-ttu-id="b4da5-1950">További információkért lásd: [fájlrendszer összekötő cikk](data-factory-onprem-file-system-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="b4da5-1950">For more information, see [File System connector article](data-factory-onprem-file-system-connector.md#linked-service-properties).</span></span>

### <a name="dataset"></a><span data-ttu-id="b4da5-1951">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-1951">Dataset</span></span>
<span data-ttu-id="b4da5-1952">toodefine egy fájlrendszer dataset set hello **típus** hello adatkészlet túl**fájlmegosztási**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1952">toodefine a File System dataset, set hello **type** of hello dataset too**FileShare**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="b4da5-1953">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1953">Property</span></span> | <span data-ttu-id="b4da5-1954">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1954">Description</span></span> | <span data-ttu-id="b4da5-1955">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1955">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-1956">folderPath</span><span class="sxs-lookup"><span data-stu-id="b4da5-1956">folderPath</span></span> |<span data-ttu-id="b4da5-1957">Hello részleges toohello mappáját adja meg.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1957">Specifies hello subpath toohello folder.</span></span> <span data-ttu-id="b4da5-1958">Hello escape-karakter használata "\" hello karakterlánc speciális karakter.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1958">Use hello escape character ‘\’ for special characters in hello string.</span></span> <span data-ttu-id="b4da5-1959">Lásd: [minta kapcsolódó szolgáltatás és az adatkészlet-definíciók](#sample-linked-service-and-dataset-definitions) példákat.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1959">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="b4da5-1960">Ez a tulajdonság a kombinálhatja **partitionBy** toohave mappák elérési útjaiban szelet alapján kezdő és záró dátum-idő.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1960">You can combine this property with **partitionBy** toohave folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="b4da5-1961">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-1961">Yes</span></span> |
| <span data-ttu-id="b4da5-1962">fileName</span><span class="sxs-lookup"><span data-stu-id="b4da5-1962">fileName</span></span> |<span data-ttu-id="b4da5-1963">Meg kell adnia hello fájl hello nevet hello **folderPath** Ha azt szeretné, hogy hello tábla toorefer tooa adott fájl hello mappában.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1963">Specify hello name of hello file in hello **folderPath** if you want hello table toorefer tooa specific file in hello folder.</span></span> <span data-ttu-id="b4da5-1964">Ha nem ad meg semmilyen értéket ehhez a tulajdonsághoz, hello tábla mutat tooall fájlok hello mappában.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1964">If you do not specify any value for this property, hello table points tooall files in hello folder.</span></span><br/><br/><span data-ttu-id="b4da5-1965">Ha nincs megadva fájlnév egy kimeneti adatkészletet, hello név hello létrehozott fájl van hello formátuma a következő:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1965">When fileName is not specified for an output dataset, hello name of hello generated file is in hello following format:</span></span> <br/><br/><span data-ttu-id="b4da5-1966">`Data.<Guid>.txt`(Példa: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span><span class="sxs-lookup"><span data-stu-id="b4da5-1966">`Data.<Guid>.txt` (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span></span> |<span data-ttu-id="b4da5-1967">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1967">No</span></span> |
| <span data-ttu-id="b4da5-1968">fileFilter</span><span class="sxs-lookup"><span data-stu-id="b4da5-1968">fileFilter</span></span> |<span data-ttu-id="b4da5-1969">Adja meg a szűrő toobe tooselect használja, minden fájl helyett a hello folderPath fájlok egy részét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1969">Specify a filter toobe used tooselect a subset of files in hello folderPath rather than all files.</span></span> <br/><br/><span data-ttu-id="b4da5-1970">Két érték engedélyezett: `*` (több karaktert) és `?` (egyetlen karakter).</span><span class="sxs-lookup"><span data-stu-id="b4da5-1970">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="b4da5-1971">1. példa: "fileFilter": "* .log"</span><span class="sxs-lookup"><span data-stu-id="b4da5-1971">Example 1: "fileFilter": "*.log"</span></span><br/><span data-ttu-id="b4da5-1972">2. példa: "fileFilter": 2016 - 1-?. txt"</span><span class="sxs-lookup"><span data-stu-id="b4da5-1972">Example 2: "fileFilter": 2016-1-?.txt"</span></span><br/><br/><span data-ttu-id="b4da5-1973">Vegye figyelembe, hogy fileFilter egy bemeneti fájlmegosztási adatkészlet esetében alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1973">Note that fileFilter is applicable for an input FileShare dataset.</span></span> |<span data-ttu-id="b4da5-1974">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1974">No</span></span> |
| <span data-ttu-id="b4da5-1975">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="b4da5-1975">partitionedBy</span></span> |<span data-ttu-id="b4da5-1976">Az idősorozat adatok partitionedBy toospecify dinamikus folderPath/fileName is használhatja.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1976">You can use partitionedBy toospecify a dynamic folderPath/fileName for time-series data.</span></span> <span data-ttu-id="b4da5-1977">Példa: az adatok óránkénti paraméteres folderPath.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1977">An example is folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="b4da5-1978">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1978">No</span></span> |
| <span data-ttu-id="b4da5-1979">Formátumban</span><span class="sxs-lookup"><span data-stu-id="b4da5-1979">format</span></span> | <span data-ttu-id="b4da5-1980">a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1980">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="b4da5-1981">Set hello **típus** tulajdonság alapján formátum tooone ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1981">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="b4da5-1982">További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1982">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="b4da5-1983">Ha azt szeretné, túl**másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a hello formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1983">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="b4da5-1984">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1984">No</span></span> |
| <span data-ttu-id="b4da5-1985">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="b4da5-1985">compression</span></span> | <span data-ttu-id="b4da5-1986">Adja meg a hello típusát és hello adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1986">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="b4da5-1987">Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**; és a támogatott szintek a következők: **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1987">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**; and supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="b4da5-1988">Lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="b4da5-1988">see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="b4da5-1989">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-1989">No</span></span> |

> [!NOTE]
> <span data-ttu-id="b4da5-1990">Nem használható egyszerre fájlnév és fileFilter.</span><span class="sxs-lookup"><span data-stu-id="b4da5-1990">You cannot use fileName and fileFilter simultaneously.</span></span>

#### <a name="example"></a><span data-ttu-id="b4da5-1991">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-1991">Example</span></span>

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

<span data-ttu-id="b4da5-1992">További információkért lásd: [fájlrendszer összekötő cikk](data-factory-onprem-file-system-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="b4da5-1992">For more information, see [File System connector article](data-factory-onprem-file-system-connector.md#dataset-properties).</span></span>

### <a name="file-system-source-in-copy-activity"></a><span data-ttu-id="b4da5-1993">A másolási tevékenység rendszer forrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1993">File System Source in Copy Activity</span></span>
<span data-ttu-id="b4da5-1994">Adatok másolása a fájlrendszerből, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**FileSystemSource**, és adja meg a következő tulajdonságok hello **forrás** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-1994">If you are copying data from File System, set hello **source type** of hello copy activity too**FileSystemSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="b4da5-1995">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-1995">Property</span></span> | <span data-ttu-id="b4da5-1996">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-1996">Description</span></span> | <span data-ttu-id="b4da5-1997">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-1997">Allowed values</span></span> | <span data-ttu-id="b4da5-1998">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-1998">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-1999">Rekurzív</span><span class="sxs-lookup"><span data-stu-id="b4da5-1999">recursive</span></span> |<span data-ttu-id="b4da5-2000">Azt jelzi, hogy hello adatolvasás rekurzív módon hello almappák vagy csak a megadott mappa hello.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2000">Indicates whether hello data is read recursively from hello subfolders or only from hello specified folder.</span></span> |<span data-ttu-id="b4da5-2001">IGAZ, hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="b4da5-2001">True, False (default)</span></span> |<span data-ttu-id="b4da5-2002">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2002">No</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-2003">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2003">Example</span></span>

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
<span data-ttu-id="b4da5-2004">További információkért lásd: [fájlrendszer összekötő cikk](data-factory-onprem-file-system-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="b4da5-2004">For more information, see [File System connector article](data-factory-onprem-file-system-connector.md#copy-activity-properties).</span></span>

### <a name="file-system-sink-in-copy-activity"></a><span data-ttu-id="b4da5-2005">A másolási tevékenység gyűjtése fájlrendszer</span><span class="sxs-lookup"><span data-stu-id="b4da5-2005">File System Sink in Copy Activity</span></span>
<span data-ttu-id="b4da5-2006">Adatok tooFile rendszer másolása, állítsa be a hello **típus gyűjtése** hello a másolási tevékenység túl**FileSystemSink**, és adja meg a következő tulajdonságok hello **fogadó** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2006">If you are copying data tooFile System, set hello **sink type** of hello copy activity too**FileSystemSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="b4da5-2007">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2007">Property</span></span> | <span data-ttu-id="b4da5-2008">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2008">Description</span></span> | <span data-ttu-id="b4da5-2009">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-2009">Allowed values</span></span> | <span data-ttu-id="b4da5-2010">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2010">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-2011">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="b4da5-2011">copyBehavior</span></span> |<span data-ttu-id="b4da5-2012">Hello másolási viselkedését határozza meg, ha hello adatforrás BlobSource vagy a fájlrendszer.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2012">Defines hello copy behavior when hello source is BlobSource or FileSystem.</span></span> |<span data-ttu-id="b4da5-2013">**PreserveHierarchy:** hello fájl hierarchia hello célmappában megőrzi.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2013">**PreserveHierarchy:** Preserves hello file hierarchy in hello target folder.</span></span> <span data-ttu-id="b4da5-2014">Ez azt jelenti, hogy van hello ugyanaz, mint a hello relatív elérési útja hello cél fájl toohello célmappa hello hello forrás fájl toohello forrásmappa relatív elérési út.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2014">That is, hello relative path of hello source file toohello source folder is hello same as hello relative path of hello target file toohello target folder.</span></span><br/><br/><span data-ttu-id="b4da5-2015">**FlattenHierarchy:** hello forrásmappából minden fájl első szintű hello célmappában jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2015">**FlattenHierarchy:** All files from hello source folder are created in hello first level of target folder.</span></span> <span data-ttu-id="b4da5-2016">hello fájljaira jönnek létre automatikusan létrehozott névvel.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2016">hello target files are created with an autogenerated name.</span></span><br/><br/><span data-ttu-id="b4da5-2017">**Mergefiles típusú:** forrásfájlból hello mappa tooone minden fájl egyesíti.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2017">**MergeFiles:** Merges all files from hello source folder tooone file.</span></span> <span data-ttu-id="b4da5-2018">Ha hello fájl neve/blob neve meg van adva, a hello egyesített fájl neve az hello megadott név.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2018">If hello file name/blob name is specified, hello merged file name is hello specified name.</span></span> <span data-ttu-id="b4da5-2019">Ellenkező esetben egy automatikusan létrehozott nevét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2019">Otherwise, it is an auto-generated file name.</span></span> |<span data-ttu-id="b4da5-2020">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2020">No</span></span> |
<span data-ttu-id="b4da5-2021">automatikus-</span><span class="sxs-lookup"><span data-stu-id="b4da5-2021">auto-</span></span>

#### <a name="example"></a><span data-ttu-id="b4da5-2022">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2022">Example</span></span>

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

<span data-ttu-id="b4da5-2023">További információkért lásd: [fájlrendszer összekötő cikk](data-factory-onprem-file-system-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="b4da5-2023">For more information, see [File System connector article](data-factory-onprem-file-system-connector.md#copy-activity-properties).</span></span>

## <a name="ftp"></a><span data-ttu-id="b4da5-2024">FTP</span><span class="sxs-lookup"><span data-stu-id="b4da5-2024">FTP</span></span>

### <a name="linked-service"></a><span data-ttu-id="b4da5-2025">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-2025">Linked service</span></span>
<span data-ttu-id="b4da5-2026">az FTP toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**FTP-kiszolgáló**, és adja meg a következő tulajdonságok hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2026">toodefine an FTP linked service, set hello **type** of hello linked service too**FtpServer**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="b4da5-2027">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2027">Property</span></span> | <span data-ttu-id="b4da5-2028">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2028">Description</span></span> | <span data-ttu-id="b4da5-2029">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2029">Required</span></span> | <span data-ttu-id="b4da5-2030">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="b4da5-2030">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-2031">állomás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2031">host</span></span> |<span data-ttu-id="b4da5-2032">Hello FTP-kiszolgáló neve vagy IP-címe</span><span class="sxs-lookup"><span data-stu-id="b4da5-2032">Name or IP address of hello FTP Server</span></span> |<span data-ttu-id="b4da5-2033">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2033">Yes</span></span> |&nbsp; |
| <span data-ttu-id="b4da5-2034">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="b4da5-2034">authenticationType</span></span> |<span data-ttu-id="b4da5-2035">Adja meg a hitelesítés típusa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2035">Specify authentication type</span></span> |<span data-ttu-id="b4da5-2036">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2036">Yes</span></span> |<span data-ttu-id="b4da5-2037">Alapszintű, a névtelen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2037">Basic, Anonymous</span></span> |
| <span data-ttu-id="b4da5-2038">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="b4da5-2038">username</span></span> |<span data-ttu-id="b4da5-2039">Felhasználó, aki rendelkezik hozzáférési toohello FTP-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="b4da5-2039">User who has access toohello FTP server</span></span> |<span data-ttu-id="b4da5-2040">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2040">No</span></span> |&nbsp; |
| <span data-ttu-id="b4da5-2041">jelszó</span><span class="sxs-lookup"><span data-stu-id="b4da5-2041">password</span></span> |<span data-ttu-id="b4da5-2042">Jelszó hello (felhasználónév)</span><span class="sxs-lookup"><span data-stu-id="b4da5-2042">Password for hello user (username)</span></span> |<span data-ttu-id="b4da5-2043">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2043">No</span></span> |&nbsp; |
| <span data-ttu-id="b4da5-2044">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="b4da5-2044">encryptedCredential</span></span> |<span data-ttu-id="b4da5-2045">Titkosított hitelesítő adatokban tooaccess hello FTP-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="b4da5-2045">Encrypted credential tooaccess hello FTP server</span></span> |<span data-ttu-id="b4da5-2046">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2046">No</span></span> |&nbsp; |
| <span data-ttu-id="b4da5-2047">gatewayName</span><span class="sxs-lookup"><span data-stu-id="b4da5-2047">gatewayName</span></span> |<span data-ttu-id="b4da5-2048">Hello az adatkezelési átjáró átjáró tooconnect tooan nevét a helyszíni FTP-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="b4da5-2048">Name of hello Data Management Gateway gateway tooconnect tooan on-premises FTP server</span></span> |<span data-ttu-id="b4da5-2049">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2049">No</span></span> |&nbsp; |
| <span data-ttu-id="b4da5-2050">port</span><span class="sxs-lookup"><span data-stu-id="b4da5-2050">port</span></span> |<span data-ttu-id="b4da5-2051">Mely hello FTP-kiszolgáló figyel port</span><span class="sxs-lookup"><span data-stu-id="b4da5-2051">Port on which hello FTP server is listening</span></span> |<span data-ttu-id="b4da5-2052">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2052">No</span></span> |<span data-ttu-id="b4da5-2053">21</span><span class="sxs-lookup"><span data-stu-id="b4da5-2053">21</span></span> |
| <span data-ttu-id="b4da5-2054">enableSsl</span><span class="sxs-lookup"><span data-stu-id="b4da5-2054">enableSsl</span></span> |<span data-ttu-id="b4da5-2055">Adja meg, hogy toouse SSL/TLS-csatorna feletti FTP-e</span><span class="sxs-lookup"><span data-stu-id="b4da5-2055">Specify whether toouse FTP over SSL/TLS channel</span></span> |<span data-ttu-id="b4da5-2056">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2056">No</span></span> |<span data-ttu-id="b4da5-2057">Igaz</span><span class="sxs-lookup"><span data-stu-id="b4da5-2057">true</span></span> |
| <span data-ttu-id="b4da5-2058">enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="b4da5-2058">enableServerCertificateValidation</span></span> |<span data-ttu-id="b4da5-2059">Adja meg, hogy a tooenable server SSL FTP használata a TLS/SSL csatornán keresztül tanúsítvány érvényesítése</span><span class="sxs-lookup"><span data-stu-id="b4da5-2059">Specify whether tooenable server SSL certificate validation when using FTP over SSL/TLS channel</span></span> |<span data-ttu-id="b4da5-2060">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2060">No</span></span> |<span data-ttu-id="b4da5-2061">Igaz</span><span class="sxs-lookup"><span data-stu-id="b4da5-2061">true</span></span> |

#### <a name="example-using-anonymous-authentication"></a><span data-ttu-id="b4da5-2062">Példa: A névtelen hitelesítést használó</span><span class="sxs-lookup"><span data-stu-id="b4da5-2062">Example: Using Anonymous authentication</span></span>

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

#### <a name="example-using-username-and-password-in-plain-text-for-basic-authentication"></a><span data-ttu-id="b4da5-2063">Példa: Használatával felhasználónevet és jelszót egyszerű szövegként az egyszerű hitelesítés</span><span class="sxs-lookup"><span data-stu-id="b4da5-2063">Example: Using username and password in plain text for basic authentication</span></span>

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

#### <a name="example-using-port-enablessl-enableservercertificatevalidation"></a><span data-ttu-id="b4da5-2064">Példa: Port, enableSsl, enableServerCertificateValidation használatával</span><span class="sxs-lookup"><span data-stu-id="b4da5-2064">Example: Using port, enableSsl, enableServerCertificateValidation</span></span>

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

#### <a name="example-using-encryptedcredential-for-authentication-and-gateway"></a><span data-ttu-id="b4da5-2065">Példa: EncryptedCredential használata a hitelesítéshez és az átjáró</span><span class="sxs-lookup"><span data-stu-id="b4da5-2065">Example: Using encryptedCredential for authentication and gateway</span></span>

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

<span data-ttu-id="b4da5-2066">További információkért lásd: [FTP-összekötő](data-factory-ftp-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2066">For more information, see [FTP connector](data-factory-ftp-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="b4da5-2067">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-2067">Dataset</span></span>
<span data-ttu-id="b4da5-2068">toodefine egy FTP-adatkészlet, set hello **típus** hello adatkészlet túl**fájlmegosztási**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2068">toodefine an FTP dataset, set hello **type** of hello dataset too**FileShare**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="b4da5-2069">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2069">Property</span></span> | <span data-ttu-id="b4da5-2070">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2070">Description</span></span> | <span data-ttu-id="b4da5-2071">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2071">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-2072">folderPath</span><span class="sxs-lookup"><span data-stu-id="b4da5-2072">folderPath</span></span> |<span data-ttu-id="b4da5-2073">Elérési út toohello almappa.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2073">Sub path toohello folder.</span></span> <span data-ttu-id="b4da5-2074">Használja az escape-karakter "\" hello karakterlánc speciális karakter.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2074">Use escape character ‘ \ ’ for special characters in hello string.</span></span> <span data-ttu-id="b4da5-2075">Lásd: [minta kapcsolódó szolgáltatás és az adatkészlet-definíciók](#sample-linked-service-and-dataset-definitions) példákat.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2075">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="b4da5-2076">Ez a tulajdonság a kombinálhatja **partitionBy** toohave mappák elérési útjaiban szelet alapján kezdő és záró dátum-idő.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2076">You can combine this property with **partitionBy** toohave folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="b4da5-2077">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2077">Yes</span></span> 
| <span data-ttu-id="b4da5-2078">fileName</span><span class="sxs-lookup"><span data-stu-id="b4da5-2078">fileName</span></span> |<span data-ttu-id="b4da5-2079">Meg kell adnia hello fájl hello nevet hello **folderPath** Ha azt szeretné, hogy hello tábla toorefer tooa adott fájl hello mappában.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2079">Specify hello name of hello file in hello **folderPath** if you want hello table toorefer tooa specific file in hello folder.</span></span> <span data-ttu-id="b4da5-2080">Ha nem ad meg semmilyen értéket ehhez a tulajdonsághoz, hello tábla mutat tooall fájlok hello mappában.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2080">If you do not specify any value for this property, hello table points tooall files in hello folder.</span></span><br/><br/><span data-ttu-id="b4da5-2081">Ha nincs megadva fájlnév egy kimeneti adatkészletet, hello hello létrehozott fájl neve a következő formátumban hello lenne:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2081">When fileName is not specified for an output dataset, hello name of hello generated file would be in hello following this format:</span></span> <br/><br/><span data-ttu-id="b4da5-2082">Adatok. <Guid>.txt (Példa: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="b4da5-2082">Data.<Guid>.txt (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="b4da5-2083">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2083">No</span></span> |
| <span data-ttu-id="b4da5-2084">fileFilter</span><span class="sxs-lookup"><span data-stu-id="b4da5-2084">fileFilter</span></span> |<span data-ttu-id="b4da5-2085">Adja meg a szűrő toobe tooselect használja, minden fájl helyett a hello folderPath fájlok egy részét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2085">Specify a filter toobe used tooselect a subset of files in hello folderPath rather than all files.</span></span><br/><br/><span data-ttu-id="b4da5-2086">Két érték engedélyezett: `*` (több karaktert) és `?` (egyetlen karakter).</span><span class="sxs-lookup"><span data-stu-id="b4da5-2086">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="b4da5-2087">1. példa:`"fileFilter": "*.log"`</span><span class="sxs-lookup"><span data-stu-id="b4da5-2087">Examples 1: `"fileFilter": "*.log"`</span></span><br/><span data-ttu-id="b4da5-2088">2. példa:`"fileFilter": 2016-1-?.txt"`</span><span class="sxs-lookup"><span data-stu-id="b4da5-2088">Example 2: `"fileFilter": 2016-1-?.txt"`</span></span><br/><br/> <span data-ttu-id="b4da5-2089">fileFilter is alkalmazható egy bemeneti fájlmegosztási az adatkészlethez.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2089">fileFilter is applicable for an input FileShare dataset.</span></span> <span data-ttu-id="b4da5-2090">Ez a tulajdonság a HDFS nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2090">This property is not supported with HDFS.</span></span> |<span data-ttu-id="b4da5-2091">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2091">No</span></span> |
| <span data-ttu-id="b4da5-2092">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="b4da5-2092">partitionedBy</span></span> |<span data-ttu-id="b4da5-2093">partitionedBy lehet használt toospecify egy dinamikus folderPath időadatok adatsorozat nevét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2093">partitionedBy can be used toospecify a dynamic folderPath, filename for time series data.</span></span> <span data-ttu-id="b4da5-2094">Például folderPath adatok óránkénti paraméteres.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2094">For example, folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="b4da5-2095">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2095">No</span></span> |
| <span data-ttu-id="b4da5-2096">Formátumban</span><span class="sxs-lookup"><span data-stu-id="b4da5-2096">format</span></span> | <span data-ttu-id="b4da5-2097">a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2097">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="b4da5-2098">Set hello **típus** tulajdonság alapján formátum tooone ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2098">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="b4da5-2099">További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2099">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="b4da5-2100">Ha azt szeretné, túl**másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a hello formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2100">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="b4da5-2101">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2101">No</span></span> |
| <span data-ttu-id="b4da5-2102">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="b4da5-2102">compression</span></span> | <span data-ttu-id="b4da5-2103">Adja meg a hello típusát és hello adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2103">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="b4da5-2104">Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**; és a támogatott szintek a következők: **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2104">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**; and supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="b4da5-2105">További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="b4da5-2105">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="b4da5-2106">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2106">No</span></span> |
| <span data-ttu-id="b4da5-2107">useBinaryTransfer</span><span class="sxs-lookup"><span data-stu-id="b4da5-2107">useBinaryTransfer</span></span> |<span data-ttu-id="b4da5-2108">Adja meg, hogy a bináris átviteli mód használata.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2108">Specify whether use Binary transfer mode.</span></span> <span data-ttu-id="b4da5-2109">A bináris mód és a hamis értéket ASCII igaz.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2109">True for binary mode and false ASCII.</span></span> <span data-ttu-id="b4da5-2110">Alapértelmezett érték: igaz.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2110">Default value: True.</span></span> <span data-ttu-id="b4da5-2111">A tulajdonság csak akkor használható, típusú társított kapcsolódószolgáltatás-típus esetén: FTP-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2111">This property can only be used when associated linked service type is of type: FtpServer.</span></span> |<span data-ttu-id="b4da5-2112">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2112">No</span></span> |

> [!NOTE]
> <span data-ttu-id="b4da5-2113">fájlnév és fileFilter nem használható egyszerre.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2113">filename and fileFilter cannot be used simultaneously.</span></span>

#### <a name="example"></a><span data-ttu-id="b4da5-2114">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2114">Example</span></span>

```json
{
    "name": "FTPFileInput",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "FTPLinkedService",
        "typeProperties": {
            "folderPath": "<path tooshared folder>",
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

<span data-ttu-id="b4da5-2115">További információkért lásd: [FTP-összekötő](data-factory-ftp-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2115">For more information, see [FTP connector](data-factory-ftp-connector.md#dataset-properties) article.</span></span>

### <a name="file-system-source-in-copy-activity"></a><span data-ttu-id="b4da5-2116">A másolási tevékenység rendszer forrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2116">File System Source in Copy Activity</span></span>
<span data-ttu-id="b4da5-2117">Adatok másolása az FTP-kiszolgálóról, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**FileSystemSource**, és adja meg a következő tulajdonságok hello **forrás** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2117">If you are copying data from an FTP server, set hello **source type** of hello copy activity too**FileSystemSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="b4da5-2118">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2118">Property</span></span> | <span data-ttu-id="b4da5-2119">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2119">Description</span></span> | <span data-ttu-id="b4da5-2120">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-2120">Allowed values</span></span> | <span data-ttu-id="b4da5-2121">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2121">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-2122">Rekurzív</span><span class="sxs-lookup"><span data-stu-id="b4da5-2122">recursive</span></span> |<span data-ttu-id="b4da5-2123">Azt jelzi, hogy hello adatolvasás rekurzív módon hello almappák vagy csak a megadott mappa hello.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2123">Indicates whether hello data is read recursively from hello sub folders or only from hello specified folder.</span></span> |<span data-ttu-id="b4da5-2124">IGAZ, hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="b4da5-2124">True, False (default)</span></span> |<span data-ttu-id="b4da5-2125">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2125">No</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-2126">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2126">Example</span></span>

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

<span data-ttu-id="b4da5-2127">További információkért lásd: [FTP-összekötő](data-factory-ftp-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2127">For more information, see [FTP connector](data-factory-ftp-connector.md#copy-activity-properties) article.</span></span>


## <a name="hdfs"></a><span data-ttu-id="b4da5-2128">HDFS</span><span class="sxs-lookup"><span data-stu-id="b4da5-2128">HDFS</span></span>

### <a name="linked-service"></a><span data-ttu-id="b4da5-2129">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-2129">Linked service</span></span>
<span data-ttu-id="b4da5-2130">a HDFS toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**Hdfs**, és adja meg a következő tulajdonságok hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2130">toodefine a HDFS linked service, set hello **type** of hello linked service too**Hdfs**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="b4da5-2131">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2131">Property</span></span> | <span data-ttu-id="b4da5-2132">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2132">Description</span></span> | <span data-ttu-id="b4da5-2133">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2133">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-2134">type</span><span class="sxs-lookup"><span data-stu-id="b4da5-2134">type</span></span> |<span data-ttu-id="b4da5-2135">hello type tulajdonságot kell beállítani: **Hdfs**</span><span class="sxs-lookup"><span data-stu-id="b4da5-2135">hello type property must be set to: **Hdfs**</span></span> |<span data-ttu-id="b4da5-2136">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2136">Yes</span></span> |
| <span data-ttu-id="b4da5-2137">URL-cím</span><span class="sxs-lookup"><span data-stu-id="b4da5-2137">Url</span></span> |<span data-ttu-id="b4da5-2138">URL-cím toohello HDFS</span><span class="sxs-lookup"><span data-stu-id="b4da5-2138">URL toohello HDFS</span></span> |<span data-ttu-id="b4da5-2139">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2139">Yes</span></span> |
| <span data-ttu-id="b4da5-2140">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="b4da5-2140">authenticationType</span></span> |<span data-ttu-id="b4da5-2141">Névtelen, vagy a Windows.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2141">Anonymous, or Windows.</span></span> <br><br> <span data-ttu-id="b4da5-2142">toouse **Kerberos-hitelesítés** HDFS-összekötőhöz, tekintse meg túl[ebben a szakaszban](#use-kerberos-authentication-for-hdfs-connector) a helyszíni környezet tooset ennek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2142">toouse **Kerberos authentication** for HDFS connector, refer too[this section](#use-kerberos-authentication-for-hdfs-connector) tooset up your on-premises environment accordingly.</span></span> |<span data-ttu-id="b4da5-2143">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2143">Yes</span></span> |
| <span data-ttu-id="b4da5-2144">Felhasználónév</span><span class="sxs-lookup"><span data-stu-id="b4da5-2144">userName</span></span> |<span data-ttu-id="b4da5-2145">Felhasználónév a Windows-hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2145">Username for Windows authentication.</span></span> |<span data-ttu-id="b4da5-2146">Igen (a Windows-hitelesítés)</span><span class="sxs-lookup"><span data-stu-id="b4da5-2146">Yes (for Windows Authentication)</span></span> |
| <span data-ttu-id="b4da5-2147">jelszó</span><span class="sxs-lookup"><span data-stu-id="b4da5-2147">password</span></span> |<span data-ttu-id="b4da5-2148">A Windows-hitelesítés jelszót.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2148">Password for Windows authentication.</span></span> |<span data-ttu-id="b4da5-2149">Igen (a Windows-hitelesítés)</span><span class="sxs-lookup"><span data-stu-id="b4da5-2149">Yes (for Windows Authentication)</span></span> |
| <span data-ttu-id="b4da5-2150">gatewayName</span><span class="sxs-lookup"><span data-stu-id="b4da5-2150">gatewayName</span></span> |<span data-ttu-id="b4da5-2151">Hello átjáró, amely a Data Factory szolgáltatásnak hello neve tooconnect toohello HDFS kell használnia.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2151">Name of hello gateway that hello Data Factory service should use tooconnect toohello HDFS.</span></span> |<span data-ttu-id="b4da5-2152">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2152">Yes</span></span> |
| <span data-ttu-id="b4da5-2153">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="b4da5-2153">encryptedCredential</span></span> |<span data-ttu-id="b4da5-2154">[Új AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) hello hozzáférési hitelesítő adatok kimenetét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2154">[New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) output of hello access credential.</span></span> |<span data-ttu-id="b4da5-2155">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2155">No</span></span> |

#### <a name="example-using-anonymous-authentication"></a><span data-ttu-id="b4da5-2156">Példa: A névtelen hitelesítést használó</span><span class="sxs-lookup"><span data-stu-id="b4da5-2156">Example: Using Anonymous authentication</span></span>

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

#### <a name="example-using-windows-authentication"></a><span data-ttu-id="b4da5-2157">Példa: A Windows-hitelesítés használatával</span><span class="sxs-lookup"><span data-stu-id="b4da5-2157">Example: Using Windows authentication</span></span>

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

<span data-ttu-id="b4da5-2158">További információkért lásd: [HDFS összekötő](#data-factory-hdfs-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2158">For more information, see [HDFS connector](#data-factory-hdfs-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="b4da5-2159">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-2159">Dataset</span></span>
<span data-ttu-id="b4da5-2160">toodefine a HDFS dataset set hello **típus** hello adatkészlet túl**fájlmegosztási**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2160">toodefine a HDFS dataset, set hello **type** of hello dataset too**FileShare**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="b4da5-2161">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2161">Property</span></span> | <span data-ttu-id="b4da5-2162">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2162">Description</span></span> | <span data-ttu-id="b4da5-2163">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2163">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-2164">folderPath</span><span class="sxs-lookup"><span data-stu-id="b4da5-2164">folderPath</span></span> |<span data-ttu-id="b4da5-2165">Toohello mappa elérési útja.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2165">Path toohello folder.</span></span> <span data-ttu-id="b4da5-2166">Példa:`myfolder`</span><span class="sxs-lookup"><span data-stu-id="b4da5-2166">Example: `myfolder`</span></span><br/><br/><span data-ttu-id="b4da5-2167">Használja az escape-karakter "\" hello karakterlánc speciális karakter.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2167">Use escape character ‘ \ ’ for special characters in hello string.</span></span> <span data-ttu-id="b4da5-2168">Például: folder\subfolder, adja meg a mappa\\\\almappa és d:\samplefolder, adja meg a d:\\\\mappába.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2168">For example: for folder\subfolder, specify folder\\\\subfolder and for d:\samplefolder, specify d:\\\\samplefolder.</span></span><br/><br/><span data-ttu-id="b4da5-2169">Ez a tulajdonság a kombinálhatja **partitionBy** toohave mappák elérési útjaiban szelet alapján kezdő és záró dátum-idő.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2169">You can combine this property with **partitionBy** toohave folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="b4da5-2170">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2170">Yes</span></span> |
| <span data-ttu-id="b4da5-2171">fileName</span><span class="sxs-lookup"><span data-stu-id="b4da5-2171">fileName</span></span> |<span data-ttu-id="b4da5-2172">Meg kell adnia hello fájl hello nevet hello **folderPath** Ha azt szeretné, hogy hello tábla toorefer tooa adott fájl hello mappában.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2172">Specify hello name of hello file in hello **folderPath** if you want hello table toorefer tooa specific file in hello folder.</span></span> <span data-ttu-id="b4da5-2173">Ha nem ad meg semmilyen értéket ehhez a tulajdonsághoz, hello tábla mutat tooall fájlok hello mappában.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2173">If you do not specify any value for this property, hello table points tooall files in hello folder.</span></span><br/><br/><span data-ttu-id="b4da5-2174">Ha nincs megadva fájlnév egy kimeneti adatkészletet, hello hello létrehozott fájl neve a következő formátumban hello lenne:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2174">When fileName is not specified for an output dataset, hello name of hello generated file would be in hello following this format:</span></span> <br/><br/><span data-ttu-id="b4da5-2175">Adatok. <Guid>.txt (például:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="b4da5-2175">Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="b4da5-2176">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2176">No</span></span> |
| <span data-ttu-id="b4da5-2177">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="b4da5-2177">partitionedBy</span></span> |<span data-ttu-id="b4da5-2178">partitionedBy lehet használt toospecify egy dinamikus folderPath időadatok adatsorozat nevét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2178">partitionedBy can be used toospecify a dynamic folderPath, filename for time series data.</span></span> <span data-ttu-id="b4da5-2179">Példa: folderPath adatok óránkénti paraméteres.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2179">Example: folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="b4da5-2180">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2180">No</span></span> |
| <span data-ttu-id="b4da5-2181">Formátumban</span><span class="sxs-lookup"><span data-stu-id="b4da5-2181">format</span></span> | <span data-ttu-id="b4da5-2182">a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2182">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="b4da5-2183">Set hello **típus** tulajdonság alapján formátum tooone ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2183">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="b4da5-2184">További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2184">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="b4da5-2185">Ha azt szeretné, túl**másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a hello formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2185">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="b4da5-2186">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2186">No</span></span> |
| <span data-ttu-id="b4da5-2187">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="b4da5-2187">compression</span></span> | <span data-ttu-id="b4da5-2188">Adja meg a hello típusát és hello adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2188">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="b4da5-2189">Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2189">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="b4da5-2190">Támogatott szintek a következők: **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2190">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="b4da5-2191">További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="b4da5-2191">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="b4da5-2192">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2192">No</span></span> |

> [!NOTE]
> <span data-ttu-id="b4da5-2193">fájlnév és fileFilter nem használható egyszerre.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2193">filename and fileFilter cannot be used simultaneously.</span></span>

#### <a name="example"></a><span data-ttu-id="b4da5-2194">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2194">Example</span></span>

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

<span data-ttu-id="b4da5-2195">További információkért lásd: [HDFS összekötő](#data-factory-hdfs-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2195">For more information, see [HDFS connector](#data-factory-hdfs-connector.md#dataset-properties) article.</span></span> 

### <a name="file-system-source-in-copy-activity"></a><span data-ttu-id="b4da5-2196">A másolási tevékenység rendszer forrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2196">File System Source in Copy Activity</span></span>
<span data-ttu-id="b4da5-2197">Adatok másolása a HDFS, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**FileSystemSource**, és adja meg a következő tulajdonságok hello **forrás** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2197">If you are copying data from HDFS, set hello **source type** of hello copy activity too**FileSystemSource**, and specify following properties in hello **source** section:</span></span>

<span data-ttu-id="b4da5-2198">**FileSystemSource** következő tulajdonságai hello támogatja:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2198">**FileSystemSource** supports hello following properties:</span></span>

| <span data-ttu-id="b4da5-2199">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2199">Property</span></span> | <span data-ttu-id="b4da5-2200">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2200">Description</span></span> | <span data-ttu-id="b4da5-2201">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-2201">Allowed values</span></span> | <span data-ttu-id="b4da5-2202">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2202">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-2203">Rekurzív</span><span class="sxs-lookup"><span data-stu-id="b4da5-2203">recursive</span></span> |<span data-ttu-id="b4da5-2204">Azt jelzi, hogy hello adatolvasás rekurzív módon hello almappák vagy csak a megadott mappa hello.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2204">Indicates whether hello data is read recursively from hello sub folders or only from hello specified folder.</span></span> |<span data-ttu-id="b4da5-2205">IGAZ, hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="b4da5-2205">True, False (default)</span></span> |<span data-ttu-id="b4da5-2206">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2206">No</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-2207">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2207">Example</span></span>

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

<span data-ttu-id="b4da5-2208">További információkért lásd: [HDFS összekötő](#data-factory-hdfs-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2208">For more information, see [HDFS connector](#data-factory-hdfs-connector.md#copy-activity-properties) article.</span></span>

## <a name="sftp"></a><span data-ttu-id="b4da5-2209">SFTP</span><span class="sxs-lookup"><span data-stu-id="b4da5-2209">SFTP</span></span>


### <a name="linked-service"></a><span data-ttu-id="b4da5-2210">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-2210">Linked service</span></span>
<span data-ttu-id="b4da5-2211">az SFTP toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**Sftp**, és adja meg a következő tulajdonságok hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2211">toodefine an SFTP linked service, set hello **type** of hello linked service too**Sftp**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="b4da5-2212">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2212">Property</span></span> | <span data-ttu-id="b4da5-2213">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2213">Description</span></span> | <span data-ttu-id="b4da5-2214">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2214">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-2215">állomás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2215">host</span></span> | <span data-ttu-id="b4da5-2216">Hello SFTP kiszolgáló neve vagy IP-címét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2216">Name or IP address of hello SFTP server.</span></span> |<span data-ttu-id="b4da5-2217">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2217">Yes</span></span> |
| <span data-ttu-id="b4da5-2218">port</span><span class="sxs-lookup"><span data-stu-id="b4da5-2218">port</span></span> |<span data-ttu-id="b4da5-2219">Az port, mely hello SFTP kiszolgáló figyel.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2219">Port on which hello SFTP server is listening.</span></span> <span data-ttu-id="b4da5-2220">hello alapértelmezett érték: 21.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2220">hello default value is: 21</span></span> |<span data-ttu-id="b4da5-2221">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2221">No</span></span> |
| <span data-ttu-id="b4da5-2222">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="b4da5-2222">authenticationType</span></span> |<span data-ttu-id="b4da5-2223">Adja meg a hitelesítés típusát.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2223">Specify authentication type.</span></span> <span data-ttu-id="b4da5-2224">Megengedett értékek: **alapvető**, **SshPublicKey**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2224">Allowed values: **Basic**, **SshPublicKey**.</span></span> <br><br> <span data-ttu-id="b4da5-2225">Tekintse meg a túl[használja az egyszerű hitelesítés](#using-basic-authentication) és [használatával SSH nyilvános kulcsos hitelesítés](#using-ssh-public-key-authentication) további tulajdonságokat és JSON-minták szakasz.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2225">Refer too[Using basic authentication](#using-basic-authentication) and [Using SSH public key authentication](#using-ssh-public-key-authentication) sections on more properties and JSON samples respectively.</span></span> |<span data-ttu-id="b4da5-2226">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2226">Yes</span></span> |
| <span data-ttu-id="b4da5-2227">skipHostKeyValidation</span><span class="sxs-lookup"><span data-stu-id="b4da5-2227">skipHostKeyValidation</span></span> | <span data-ttu-id="b4da5-2228">Adja meg, hogy tooskip gazdagép kulcs érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2228">Specify whether tooskip host key validation.</span></span> | <span data-ttu-id="b4da5-2229">Nem.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2229">No.</span></span> <span data-ttu-id="b4da5-2230">alapértelmezett érték hello: hamis</span><span class="sxs-lookup"><span data-stu-id="b4da5-2230">hello default value: false</span></span> |
| <span data-ttu-id="b4da5-2231">hostKeyFingerprint</span><span class="sxs-lookup"><span data-stu-id="b4da5-2231">hostKeyFingerprint</span></span> | <span data-ttu-id="b4da5-2232">Adja meg a hello ujjlenyomat hello állomás kulcs.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2232">Specify hello finger print of hello host key.</span></span> | <span data-ttu-id="b4da5-2233">Igen, ha a hello `skipHostKeyValidation` toofalse van beállítva.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2233">Yes if hello `skipHostKeyValidation` is set toofalse.</span></span>  |
| <span data-ttu-id="b4da5-2234">gatewayName</span><span class="sxs-lookup"><span data-stu-id="b4da5-2234">gatewayName</span></span> |<span data-ttu-id="b4da5-2235">Hello az adatkezelési átjáró tooconnect tooan nevét a helyszíni SFTP kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2235">Name of hello Data Management Gateway tooconnect tooan on-premises SFTP server.</span></span> | <span data-ttu-id="b4da5-2236">Igen, ha az adatok másolása egy helyszíni SFTP-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2236">Yes if copying data from an on-premises SFTP server.</span></span> |
| <span data-ttu-id="b4da5-2237">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="b4da5-2237">encryptedCredential</span></span> | <span data-ttu-id="b4da5-2238">Titkosított hitelesítő adatokban tooaccess hello SFTP kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2238">Encrypted credential tooaccess hello SFTP server.</span></span> <span data-ttu-id="b4da5-2239">Automatikusan létrehozott Ha megadja az egyszerű hitelesítés (felhasználónév + jelszó) vagy az SshPublicKey hitelesítési (felhasználónév + titkos kulcs elérési útja vagy tartalom) másolása varázsló vagy a hello ClickOnce előugró párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2239">Auto-generated when you specify basic authentication (username + password) or SshPublicKey authentication (username + private key path or content) in copy wizard or hello ClickOnce popup dialog.</span></span> | <span data-ttu-id="b4da5-2240">Nem.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2240">No.</span></span> <span data-ttu-id="b4da5-2241">Csak akkor, ha az adatok másolása egy helyszíni SFTP kiszolgáló alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2241">Apply only when copying data from an on-premises SFTP server.</span></span> |

#### <a name="example-using-basic-authentication"></a><span data-ttu-id="b4da5-2242">Például: Alapszintű hitelesítést használ</span><span class="sxs-lookup"><span data-stu-id="b4da5-2242">Example: Using basic authentication</span></span>

<span data-ttu-id="b4da5-2243">Alapszintű hitelesítés toouse, állítsa be `authenticationType` , `Basic`, és adja meg a következő tulajdonságai módosításokon kívül SFTP összekötő hello utolsó szakaszában bevezetett általános ők hello hello:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2243">toouse basic authentication, set `authenticationType` as `Basic`, and specify hello following properties besides hello SFTP connector generic ones introduced in hello last section:</span></span>

| <span data-ttu-id="b4da5-2244">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2244">Property</span></span> | <span data-ttu-id="b4da5-2245">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2245">Description</span></span> | <span data-ttu-id="b4da5-2246">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2246">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-2247">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="b4da5-2247">username</span></span> | <span data-ttu-id="b4da5-2248">Az felhasználó, aki rendelkezik toohello SFTP kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2248">User who has access toohello SFTP server.</span></span> |<span data-ttu-id="b4da5-2249">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2249">Yes</span></span> |
| <span data-ttu-id="b4da5-2250">jelszó</span><span class="sxs-lookup"><span data-stu-id="b4da5-2250">password</span></span> | <span data-ttu-id="b4da5-2251">(Felhasználónév) hello felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2251">Password for hello user (username).</span></span> | <span data-ttu-id="b4da5-2252">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2252">Yes</span></span> |

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

#### <a name="example-basic-authentication-with-encrypted-credential"></a><span data-ttu-id="b4da5-2253">Példa: Egyszerű hitelesítést a titkosított hitelesítő adat **</span><span class="sxs-lookup"><span data-stu-id="b4da5-2253">Example: Basic authentication with encrypted credential**</span></span>

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

#### <a name="using-ssh-public-key-authentication"></a><span data-ttu-id="b4da5-2254">SSH nyilvános kulcsos hitelesítés használatával: **</span><span class="sxs-lookup"><span data-stu-id="b4da5-2254">Using SSH public key authentication:**</span></span>

<span data-ttu-id="b4da5-2255">Alapszintű hitelesítés toouse, állítsa be `authenticationType` , `SshPublicKey`, és adja meg a következő tulajdonságai módosításokon kívül SFTP összekötő hello utolsó szakaszában bevezetett általános ők hello hello:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2255">toouse basic authentication, set `authenticationType` as `SshPublicKey`, and specify hello following properties besides hello SFTP connector generic ones introduced in hello last section:</span></span>

| <span data-ttu-id="b4da5-2256">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2256">Property</span></span> | <span data-ttu-id="b4da5-2257">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2257">Description</span></span> | <span data-ttu-id="b4da5-2258">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2258">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-2259">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="b4da5-2259">username</span></span> |<span data-ttu-id="b4da5-2260">Felhasználó, aki rendelkezik toohello SFTP kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="b4da5-2260">User who has access toohello SFTP server</span></span> |<span data-ttu-id="b4da5-2261">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2261">Yes</span></span> |
| <span data-ttu-id="b4da5-2262">privateKeyPath</span><span class="sxs-lookup"><span data-stu-id="b4da5-2262">privateKeyPath</span></span> | <span data-ttu-id="b4da5-2263">Adjon meg abszolút elérési út toohello titkos kulcsot tartalmazó fájlt, hogy az átjáró férhetnek hozzá.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2263">Specify absolute path toohello private key file that gateway can access.</span></span> | <span data-ttu-id="b4da5-2264">Adja meg vagy hello `privateKeyPath` vagy `privateKeyContent`.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2264">Specify either hello `privateKeyPath` or `privateKeyContent`.</span></span> <br><br> <span data-ttu-id="b4da5-2265">Csak akkor, ha az adatok másolása egy helyszíni SFTP kiszolgáló alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2265">Apply only when copying data from an on-premises SFTP server.</span></span> |
| <span data-ttu-id="b4da5-2266">privateKeyContent</span><span class="sxs-lookup"><span data-stu-id="b4da5-2266">privateKeyContent</span></span> | <span data-ttu-id="b4da5-2267">Hello titkos kulcs tartalmát, mivel a szerializált karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2267">A serialized string of hello private key content.</span></span> <span data-ttu-id="b4da5-2268">hello másolása varázsló olvashatja hello titkos kulcsot tartalmazó fájlt, és bontsa ki a titkos kulcs tartalmát hello automatikusan.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2268">hello Copy Wizard can read hello private key file and extract hello private key content automatically.</span></span> <span data-ttu-id="b4da5-2269">Minden egyéb eszköz/SDK használatakor használja hello privateKeyPath tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2269">If you are using any other tool/SDK, use hello privateKeyPath property instead.</span></span> | <span data-ttu-id="b4da5-2270">Adja meg vagy hello `privateKeyPath` vagy `privateKeyContent`.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2270">Specify either hello `privateKeyPath` or `privateKeyContent`.</span></span> |
| <span data-ttu-id="b4da5-2271">hozzáférési kód</span><span class="sxs-lookup"><span data-stu-id="b4da5-2271">passPhrase</span></span> | <span data-ttu-id="b4da5-2272">Adja meg hello hozzáférési kódot vagy jelszót toodecrypt hello titkos kulcsot, ha hello kulcsfájl védi egy hozzáférési kódot.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2272">Specify hello pass phrase/password toodecrypt hello private key if hello key file is protected by a pass phrase.</span></span> | <span data-ttu-id="b4da5-2273">Igen, ha a hozzáférési kód hello titkos kulcsot tartalmazó fájlt védi.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2273">Yes if hello private key file is protected by a pass phrase.</span></span> |

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

#### <a name="example-sshpublickey-authentication-using-private-key-content"></a><span data-ttu-id="b4da5-2274">Példa: Az SshPublicKey hitelesítés titkos kulcs tartalom **</span><span class="sxs-lookup"><span data-stu-id="b4da5-2274">Example: SshPublicKey authentication using private key content**</span></span>

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
            "privateKeyContent": "<base64 string of hello private key content>",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true
        }
    }
}
```

<span data-ttu-id="b4da5-2275">További információkért lásd: [SFTP összekötő](data-factory-sftp-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2275">For more information, see [SFTP connector](data-factory-sftp-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="b4da5-2276">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-2276">Dataset</span></span>
<span data-ttu-id="b4da5-2277">toodefine az SFTP adatkészlethez set hello **típus** hello adatkészlet túl**fájlmegosztási**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2277">toodefine an SFTP dataset, set hello **type** of hello dataset too**FileShare**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="b4da5-2278">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2278">Property</span></span> | <span data-ttu-id="b4da5-2279">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2279">Description</span></span> | <span data-ttu-id="b4da5-2280">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2280">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-2281">folderPath</span><span class="sxs-lookup"><span data-stu-id="b4da5-2281">folderPath</span></span> |<span data-ttu-id="b4da5-2282">Elérési út toohello almappa.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2282">Sub path toohello folder.</span></span> <span data-ttu-id="b4da5-2283">Használja az escape-karakter "\" hello karakterlánc speciális karakter.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2283">Use escape character ‘ \ ’ for special characters in hello string.</span></span> <span data-ttu-id="b4da5-2284">Lásd: [minta kapcsolódó szolgáltatás és az adatkészlet-definíciók](#sample-linked-service-and-dataset-definitions) példákat.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2284">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="b4da5-2285">Ez a tulajdonság a kombinálhatja **partitionBy** toohave mappák elérési útjaiban szelet alapján kezdő és záró dátum-idő.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2285">You can combine this property with **partitionBy** toohave folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="b4da5-2286">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2286">Yes</span></span> |
| <span data-ttu-id="b4da5-2287">fileName</span><span class="sxs-lookup"><span data-stu-id="b4da5-2287">fileName</span></span> |<span data-ttu-id="b4da5-2288">Meg kell adnia hello fájl hello nevet hello **folderPath** Ha azt szeretné, hogy hello tábla toorefer tooa adott fájl hello mappában.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2288">Specify hello name of hello file in hello **folderPath** if you want hello table toorefer tooa specific file in hello folder.</span></span> <span data-ttu-id="b4da5-2289">Ha nem ad meg semmilyen értéket ehhez a tulajdonsághoz, hello tábla mutat tooall fájlok hello mappában.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2289">If you do not specify any value for this property, hello table points tooall files in hello folder.</span></span><br/><br/><span data-ttu-id="b4da5-2290">Ha nincs megadva fájlnév egy kimeneti adatkészletet, hello hello létrehozott fájl neve a következő formátumban hello lenne:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2290">When fileName is not specified for an output dataset, hello name of hello generated file would be in hello following this format:</span></span> <br/><br/><span data-ttu-id="b4da5-2291">Adatok. <Guid>.txt (Példa: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="b4da5-2291">Data.<Guid>.txt (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="b4da5-2292">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2292">No</span></span> |
| <span data-ttu-id="b4da5-2293">fileFilter</span><span class="sxs-lookup"><span data-stu-id="b4da5-2293">fileFilter</span></span> |<span data-ttu-id="b4da5-2294">Adja meg a szűrő toobe tooselect használja, minden fájl helyett a hello folderPath fájlok egy részét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2294">Specify a filter toobe used tooselect a subset of files in hello folderPath rather than all files.</span></span><br/><br/><span data-ttu-id="b4da5-2295">Két érték engedélyezett: `*` (több karaktert) és `?` (egyetlen karakter).</span><span class="sxs-lookup"><span data-stu-id="b4da5-2295">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="b4da5-2296">1. példa:`"fileFilter": "*.log"`</span><span class="sxs-lookup"><span data-stu-id="b4da5-2296">Examples 1: `"fileFilter": "*.log"`</span></span><br/><span data-ttu-id="b4da5-2297">2. példa:`"fileFilter": 2016-1-?.txt"`</span><span class="sxs-lookup"><span data-stu-id="b4da5-2297">Example 2: `"fileFilter": 2016-1-?.txt"`</span></span><br/><br/> <span data-ttu-id="b4da5-2298">fileFilter is alkalmazható egy bemeneti fájlmegosztási az adatkészlethez.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2298">fileFilter is applicable for an input FileShare dataset.</span></span> <span data-ttu-id="b4da5-2299">Ez a tulajdonság a HDFS nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2299">This property is not supported with HDFS.</span></span> |<span data-ttu-id="b4da5-2300">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2300">No</span></span> |
| <span data-ttu-id="b4da5-2301">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="b4da5-2301">partitionedBy</span></span> |<span data-ttu-id="b4da5-2302">partitionedBy lehet használt toospecify egy dinamikus folderPath időadatok adatsorozat nevét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2302">partitionedBy can be used toospecify a dynamic folderPath, filename for time series data.</span></span> <span data-ttu-id="b4da5-2303">Például folderPath adatok óránkénti paraméteres.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2303">For example, folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="b4da5-2304">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2304">No</span></span> |
| <span data-ttu-id="b4da5-2305">Formátumban</span><span class="sxs-lookup"><span data-stu-id="b4da5-2305">format</span></span> | <span data-ttu-id="b4da5-2306">a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2306">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="b4da5-2307">Set hello **típus** tulajdonság alapján formátum tooone ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2307">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="b4da5-2308">További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2308">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="b4da5-2309">Ha azt szeretné, túl**másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a hello formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2309">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="b4da5-2310">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2310">No</span></span> |
| <span data-ttu-id="b4da5-2311">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="b4da5-2311">compression</span></span> | <span data-ttu-id="b4da5-2312">Adja meg a hello típusát és hello adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2312">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="b4da5-2313">Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2313">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="b4da5-2314">Támogatott szintek a következők: **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2314">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="b4da5-2315">További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="b4da5-2315">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="b4da5-2316">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2316">No</span></span> |
| <span data-ttu-id="b4da5-2317">useBinaryTransfer</span><span class="sxs-lookup"><span data-stu-id="b4da5-2317">useBinaryTransfer</span></span> |<span data-ttu-id="b4da5-2318">Adja meg, hogy a bináris átviteli mód használata.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2318">Specify whether use Binary transfer mode.</span></span> <span data-ttu-id="b4da5-2319">A bináris mód és a hamis értéket ASCII igaz.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2319">True for binary mode and false ASCII.</span></span> <span data-ttu-id="b4da5-2320">Alapértelmezett érték: igaz.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2320">Default value: True.</span></span> <span data-ttu-id="b4da5-2321">A tulajdonság csak akkor használható, típusú társított kapcsolódószolgáltatás-típus esetén: FTP-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2321">This property can only be used when associated linked service type is of type: FtpServer.</span></span> |<span data-ttu-id="b4da5-2322">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2322">No</span></span> |

> [!NOTE]
> <span data-ttu-id="b4da5-2323">fájlnév és fileFilter nem használható egyszerre.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2323">filename and fileFilter cannot be used simultaneously.</span></span>

#### <a name="example"></a><span data-ttu-id="b4da5-2324">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2324">Example</span></span>

```json
{
    "name": "SFTPFileInput",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "SftpLinkedService",
        "typeProperties": {
            "folderPath": "<path tooshared folder>",
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

<span data-ttu-id="b4da5-2325">További információkért lásd: [SFTP összekötő](data-factory-sftp-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2325">For more information, see [SFTP connector](data-factory-sftp-connector.md#dataset-properties) article.</span></span> 

### <a name="file-system-source-in-copy-activity"></a><span data-ttu-id="b4da5-2326">A másolási tevékenység rendszer forrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2326">File System Source in Copy Activity</span></span>
<span data-ttu-id="b4da5-2327">SFTP forrásból származó adatok másolása, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**FileSystemSource**, és adja meg a következő tulajdonságok hello **forrás** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2327">If you are copying data from an SFTP source, set hello **source type** of hello copy activity too**FileSystemSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="b4da5-2328">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2328">Property</span></span> | <span data-ttu-id="b4da5-2329">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2329">Description</span></span> | <span data-ttu-id="b4da5-2330">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-2330">Allowed values</span></span> | <span data-ttu-id="b4da5-2331">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2331">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-2332">Rekurzív</span><span class="sxs-lookup"><span data-stu-id="b4da5-2332">recursive</span></span> |<span data-ttu-id="b4da5-2333">Azt jelzi, hogy hello adatolvasás rekurzív módon hello almappák vagy csak a megadott mappa hello.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2333">Indicates whether hello data is read recursively from hello sub folders or only from hello specified folder.</span></span> |<span data-ttu-id="b4da5-2334">IGAZ, hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="b4da5-2334">True, False (default)</span></span> |<span data-ttu-id="b4da5-2335">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2335">No</span></span> |



#### <a name="example"></a><span data-ttu-id="b4da5-2336">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2336">Example</span></span>

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

<span data-ttu-id="b4da5-2337">További információkért lásd: [SFTP összekötő](data-factory-sftp-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2337">For more information, see [SFTP connector](data-factory-sftp-connector.md#copy-activity-properties) article.</span></span>


## <a name="http"></a><span data-ttu-id="b4da5-2338">HTTP</span><span class="sxs-lookup"><span data-stu-id="b4da5-2338">HTTP</span></span>

### <a name="linked-service"></a><span data-ttu-id="b4da5-2339">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-2339">Linked service</span></span>
<span data-ttu-id="b4da5-2340">a HTTP toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**Http**, és adja meg a következő tulajdonságok hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2340">toodefine a HTTP linked service, set hello **type** of hello linked service too**Http**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="b4da5-2341">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2341">Property</span></span> | <span data-ttu-id="b4da5-2342">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2342">Description</span></span> | <span data-ttu-id="b4da5-2343">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2343">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-2344">URL-címe</span><span class="sxs-lookup"><span data-stu-id="b4da5-2344">url</span></span> | <span data-ttu-id="b4da5-2345">Alap URL-cím toohello webkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="b4da5-2345">Base URL toohello Web Server</span></span> | <span data-ttu-id="b4da5-2346">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2346">Yes</span></span> |
| <span data-ttu-id="b4da5-2347">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="b4da5-2347">authenticationType</span></span> | <span data-ttu-id="b4da5-2348">Hello hitelesítés típusát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2348">Specifies hello authentication type.</span></span> <span data-ttu-id="b4da5-2349">Két érték engedélyezett: **névtelen**, **alapvető**, **kivonatoló**, **Windows**, **ClientCertificate**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2349">Allowed values are: **Anonymous**, **Basic**, **Digest**, **Windows**, **ClientCertificate**.</span></span> <br><br> <span data-ttu-id="b4da5-2350">További tulajdonságokat és JSON-minták a táblázat alatti toosections rendre adott hitelesítési típusok olvassa.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2350">Refer toosections below this table on more properties and JSON samples for those authentication types respectively.</span></span> | <span data-ttu-id="b4da5-2351">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2351">Yes</span></span> |
| <span data-ttu-id="b4da5-2352">enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="b4da5-2352">enableServerCertificateValidation</span></span> | <span data-ttu-id="b4da5-2353">Adja meg, hogy tooenable server SSL tanúsítvány érvényesítése, ha forrás HTTPS webkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="b4da5-2353">Specify whether tooenable server SSL certificate validation if source is HTTPS Web Server</span></span> | <span data-ttu-id="b4da5-2354">Nem, alapértelmezett érték true</span><span class="sxs-lookup"><span data-stu-id="b4da5-2354">No, default is true</span></span> |
| <span data-ttu-id="b4da5-2355">gatewayName</span><span class="sxs-lookup"><span data-stu-id="b4da5-2355">gatewayName</span></span> | <span data-ttu-id="b4da5-2356">Hello az adatkezelési átjáró tooconnect tooan nevét a helyszíni HTTP-forrás.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2356">Name of hello Data Management Gateway tooconnect tooan on-premises HTTP source.</span></span> | <span data-ttu-id="b4da5-2357">Igen, ha a helyszíni HTTP forrásból származó adatok másolása.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2357">Yes if copying data from an on-premises HTTP source.</span></span> |
| <span data-ttu-id="b4da5-2358">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="b4da5-2358">encryptedCredential</span></span> | <span data-ttu-id="b4da5-2359">Titkosított hitelesítő adatokban tooaccess hello HTTP-végpont.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2359">Encrypted credential tooaccess hello HTTP endpoint.</span></span> <span data-ttu-id="b4da5-2360">Automatikusan létrehozott hello hitelesítési adatok másolása varázsló vagy a hello ClickOnce előugró párbeszédpanelen konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2360">Auto-generated when you configure hello authentication information in copy wizard or hello ClickOnce popup dialog.</span></span> | <span data-ttu-id="b4da5-2361">Nem.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2361">No.</span></span> <span data-ttu-id="b4da5-2362">Csak akkor, ha az adatok másolása helyi HTTP-kiszolgáló alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2362">Apply only when copying data from an on-premises HTTP server.</span></span> |

#### <a name="example-using-basic-digest-or-windows-authentication"></a><span data-ttu-id="b4da5-2363">Példa: Basic, a kivonatoló vagy a Windows-hitelesítés használatával</span><span class="sxs-lookup"><span data-stu-id="b4da5-2363">Example: Using Basic, Digest, or Windows authentication</span></span>
<span data-ttu-id="b4da5-2364">Állítsa be `authenticationType` , `Basic`, `Digest`, vagy `Windows`, és adja meg a következő tulajdonságai módosításokon kívül HTTP összekötő általános azokat, a fenti bevezetett hello hello:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2364">Set `authenticationType` as `Basic`, `Digest`, or `Windows`, and specify hello following properties besides hello HTTP connector generic ones introduced above:</span></span>

| <span data-ttu-id="b4da5-2365">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2365">Property</span></span> | <span data-ttu-id="b4da5-2366">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2366">Description</span></span> | <span data-ttu-id="b4da5-2367">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2367">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-2368">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="b4da5-2368">username</span></span> | <span data-ttu-id="b4da5-2369">Felhasználónév tooaccess hello HTTP-végpont.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2369">Username tooaccess hello HTTP endpoint.</span></span> | <span data-ttu-id="b4da5-2370">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2370">Yes</span></span> |
| <span data-ttu-id="b4da5-2371">jelszó</span><span class="sxs-lookup"><span data-stu-id="b4da5-2371">password</span></span> | <span data-ttu-id="b4da5-2372">(Felhasználónév) hello felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2372">Password for hello user (username).</span></span> | <span data-ttu-id="b4da5-2373">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2373">Yes</span></span> |

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

#### <a name="example-using-clientcertificate-authentication"></a><span data-ttu-id="b4da5-2374">Példa: ClientCertificate hitelesítés használatával</span><span class="sxs-lookup"><span data-stu-id="b4da5-2374">Example: Using ClientCertificate authentication</span></span>

<span data-ttu-id="b4da5-2375">Alapszintű hitelesítés toouse, állítsa be `authenticationType` , `ClientCertificate`, és adja meg a következő tulajdonságai módosításokon kívül HTTP összekötő általános azokat, a fenti bevezetett hello hello:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2375">toouse basic authentication, set `authenticationType` as `ClientCertificate`, and specify hello following properties besides hello HTTP connector generic ones introduced above:</span></span>

| <span data-ttu-id="b4da5-2376">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2376">Property</span></span> | <span data-ttu-id="b4da5-2377">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2377">Description</span></span> | <span data-ttu-id="b4da5-2378">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2378">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-2379">embeddedCertData</span><span class="sxs-lookup"><span data-stu-id="b4da5-2379">embeddedCertData</span></span> | <span data-ttu-id="b4da5-2380">hello Base64-kódolású tartalmak a bináris adatok hello személyes információcseréhez kapcsolódó (PFX) fájl.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2380">hello Base64-encoded contents of binary data of hello Personal Information Exchange (PFX) file.</span></span> | <span data-ttu-id="b4da5-2381">Adja meg vagy hello `embeddedCertData` vagy `certThumbprint`.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2381">Specify either hello `embeddedCertData` or `certThumbprint`.</span></span> |
| <span data-ttu-id="b4da5-2382">CertThumbprint</span><span class="sxs-lookup"><span data-stu-id="b4da5-2382">certThumbprint</span></span> | <span data-ttu-id="b4da5-2383">hello telepítve lett az átjáró gépen tanúsítványtároló hello tanúsítvány ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2383">hello thumbprint of hello certificate that was installed on your gateway machine’s cert store.</span></span> <span data-ttu-id="b4da5-2384">Csak akkor, ha a helyszíni HTTP forrásból származó adat másolása alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2384">Apply only when copying data from an on-premises HTTP source.</span></span> | <span data-ttu-id="b4da5-2385">Adja meg vagy hello `embeddedCertData` vagy `certThumbprint`.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2385">Specify either hello `embeddedCertData` or `certThumbprint`.</span></span> |
| <span data-ttu-id="b4da5-2386">jelszó</span><span class="sxs-lookup"><span data-stu-id="b4da5-2386">password</span></span> | <span data-ttu-id="b4da5-2387">Hello tanúsítványhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2387">Password associated with hello certificate.</span></span> | <span data-ttu-id="b4da5-2388">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2388">No</span></span> |

<span data-ttu-id="b4da5-2389">Ha `certThumbprint` hitelesítési és hello tanúsítvány telepítve van a hello hello helyi számítógép személyes tárolójában, kell toogrant hello olvasási hozzáférést toohello átjáró szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2389">If you use `certThumbprint` for authentication and hello certificate is installed in hello personal store of hello local computer, you need toogrant hello read permission toohello gateway service:</span></span>

1. <span data-ttu-id="b4da5-2390">Indítsa el a Microsoft Management Console (MMC).</span><span class="sxs-lookup"><span data-stu-id="b4da5-2390">Launch Microsoft Management Console (MMC).</span></span> <span data-ttu-id="b4da5-2391">Adja hozzá a hello **tanúsítványok** beépülő modul a célok hello **helyi számítógép**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2391">Add hello **Certificates** snap-in that targets hello **Local Computer**.</span></span>
2. <span data-ttu-id="b4da5-2392">Bontsa ki a **tanúsítványok**, **személyes**, és kattintson a **tanúsítványok**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2392">Expand **Certificates**, **Personal**, and click **Certificates**.</span></span>
3. <span data-ttu-id="b4da5-2393">Kattintson a jobb gombbal a hello tanúsítványt hello személyes tárolójában, és válassza ki **feladataival**->**titkos kulcsok kezelése...**</span><span class="sxs-lookup"><span data-stu-id="b4da5-2393">Right-click hello certificate from hello personal store, and select **All Tasks**->**Manage Private Keys...**</span></span>
3. <span data-ttu-id="b4da5-2394">A hello **biztonsági** lapon maradva adja hozzá a hello felhasználói fiók alatt az adatkezelési átjáró gazdaszolgáltatása fut. hello olvasási hozzáférés toohello tanúsítvánnyal.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2394">On hello **Security** tab, add hello user account under which Data Management Gateway Host Service is running with hello read access toohello certificate.</span></span>  

<span data-ttu-id="b4da5-2395">**Példa: ügyfél-tanúsítványt használ:** a társított szolgáltatás hivatkozások a data factory tooan helyszíni HTTP webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2395">**Example: using client certificate:** This linked service links your data factory tooan on-premises HTTP web server.</span></span> <span data-ttu-id="b4da5-2396">Az adatkezelési átjáró telepített hello gépen telepített ügyféltanúsítványt használ.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2396">It uses a client certificate that is installed on hello machine with Data Management Gateway installed.</span></span>

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

#### <a name="example-using-client-certificate-in-a-file"></a><span data-ttu-id="b4da5-2397">Példa: ügyfél-tanúsítványt használ egy fájlban</span><span class="sxs-lookup"><span data-stu-id="b4da5-2397">Example: using client certificate in a file</span></span>
<span data-ttu-id="b4da5-2398">Ez a data factory tooan helyszíni HTTP webkiszolgáló kapcsolódó szolgáltatás hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2398">This linked service links your data factory tooan on-premises HTTP web server.</span></span> <span data-ttu-id="b4da5-2399">Az adatkezelési átjáró telepítése egy ügyfél tanúsítványfájl hello gépen használ.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2399">It uses a client certificate file on hello machine with Data Management Gateway installed.</span></span>

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

<span data-ttu-id="b4da5-2400">További információkért lásd: [HTTP összekötő](data-factory-http-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2400">For more information, see [HTTP connector](data-factory-http-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="b4da5-2401">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-2401">Dataset</span></span>
<span data-ttu-id="b4da5-2402">toodefine egy HTTP-adatkészlet, set hello **típus** hello adatkészlet túl**Http**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2402">toodefine a HTTP dataset, set hello **type** of hello dataset too**Http**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="b4da5-2403">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2403">Property</span></span> | <span data-ttu-id="b4da5-2404">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2404">Description</span></span> | <span data-ttu-id="b4da5-2405">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2405">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="b4da5-2406">relativeUrl</span><span class="sxs-lookup"><span data-stu-id="b4da5-2406">relativeUrl</span></span> | <span data-ttu-id="b4da5-2407">Relatív URL-cím toohello erőforrás hello adatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2407">A relative URL toohello resource that contains hello data.</span></span> <span data-ttu-id="b4da5-2408">Ha nincs megadva elérési út, kapcsolódó hello szolgáltatásdefinícióban megadott csak hello URL szolgál.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2408">When path is not specified, only hello URL specified in hello linked service definition is used.</span></span> <br><br> <span data-ttu-id="b4da5-2409">tooconstruct dinamikus URL-címe, használhat [adat-előállító funkciók és rendszerváltozók](data-factory-functions-variables.md), például: `"relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)"`.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2409">tooconstruct dynamic URL, you can use [Data Factory functions and system variables](data-factory-functions-variables.md), Example: `"relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)"`.</span></span> | <span data-ttu-id="b4da5-2410">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2410">No</span></span> |
| <span data-ttu-id="b4da5-2411">requestMethod</span><span class="sxs-lookup"><span data-stu-id="b4da5-2411">requestMethod</span></span> | <span data-ttu-id="b4da5-2412">HTTP-metódus.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2412">Http method.</span></span> <span data-ttu-id="b4da5-2413">Két érték engedélyezett **beolvasása** vagy **POST**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2413">Allowed values are **GET** or **POST**.</span></span> | <span data-ttu-id="b4da5-2414">Nem.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2414">No.</span></span> <span data-ttu-id="b4da5-2415">Alapértelmezett érték a `GET`.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2415">Default is `GET`.</span></span> |
| <span data-ttu-id="b4da5-2416">additionalHeaders</span><span class="sxs-lookup"><span data-stu-id="b4da5-2416">additionalHeaders</span></span> | <span data-ttu-id="b4da5-2417">További HTTP-kérelemfejlécekben.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2417">Additional HTTP request headers.</span></span> | <span data-ttu-id="b4da5-2418">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2418">No</span></span> |
| <span data-ttu-id="b4da5-2419">requestBody</span><span class="sxs-lookup"><span data-stu-id="b4da5-2419">requestBody</span></span> | <span data-ttu-id="b4da5-2420">A HTTP-kérelmek törzsében.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2420">Body for HTTP request.</span></span> | <span data-ttu-id="b4da5-2421">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2421">No</span></span> |
| <span data-ttu-id="b4da5-2422">Formátumban</span><span class="sxs-lookup"><span data-stu-id="b4da5-2422">format</span></span> | <span data-ttu-id="b4da5-2423">Ha azt szeretné, hogy toosimply **hello adatainak lekérése, a HTTP-végpont-van** nélkül elemzés azt, hagyja ki a formátumot beállítások.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2423">If you want toosimply **retrieve hello data from HTTP endpoint as-is** without parsing it, skip this format settings.</span></span> <br><br> <span data-ttu-id="b4da5-2424">Ha szeretné tooparse hello HTTP-válasz tartalom másolása során, a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2424">If you want tooparse hello HTTP response content during copy, hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="b4da5-2425">További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2425">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> |<span data-ttu-id="b4da5-2426">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2426">No</span></span> |
| <span data-ttu-id="b4da5-2427">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="b4da5-2427">compression</span></span> | <span data-ttu-id="b4da5-2428">Adja meg a hello típusát és hello adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2428">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="b4da5-2429">Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2429">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="b4da5-2430">Támogatott szintek a következők: **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2430">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="b4da5-2431">További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="b4da5-2431">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="b4da5-2432">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2432">No</span></span> |

#### <a name="example-using-hello-get-default-method"></a><span data-ttu-id="b4da5-2433">Példa: hello (alapértelmezett) GET metódussal</span><span class="sxs-lookup"><span data-stu-id="b4da5-2433">Example: using hello GET (default) method</span></span>

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

#### <a name="example-using-hello-post-method"></a><span data-ttu-id="b4da5-2434">Példa: hello POST metódussal</span><span class="sxs-lookup"><span data-stu-id="b4da5-2434">Example: using hello POST method</span></span>

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
<span data-ttu-id="b4da5-2435">További információkért lásd: [HTTP összekötő](data-factory-http-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2435">For more information, see [HTTP connector](data-factory-http-connector.md#dataset-properties) article.</span></span>

### <a name="http-source-in-copy-activity"></a><span data-ttu-id="b4da5-2436">A másolási tevékenység HTTP-forrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2436">HTTP Source in Copy Activity</span></span>
<span data-ttu-id="b4da5-2437">Adatok másolása egy HTTP-bejegyzéseket, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**HttpSource**, és adja meg a következő tulajdonságok hello **forrás** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2437">If you are copying data from a HTTP source, set hello **source type** of hello copy activity too**HttpSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="b4da5-2438">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2438">Property</span></span> | <span data-ttu-id="b4da5-2439">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2439">Description</span></span> | <span data-ttu-id="b4da5-2440">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2440">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="b4da5-2441">httpRequestTimeout</span><span class="sxs-lookup"><span data-stu-id="b4da5-2441">httpRequestTimeout</span></span> | <span data-ttu-id="b4da5-2442">hello hello HTTP kérelem tooget választ (időtartam) időkorlátját.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2442">hello timeout (TimeSpan) for hello HTTP request tooget a response.</span></span> <span data-ttu-id="b4da5-2443">Hello időtúllépés tooget választ, hello időtúllépés tooread érkezett válasz adatait is.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2443">It is hello timeout tooget a response, not hello timeout tooread response data.</span></span> | <span data-ttu-id="b4da5-2444">Nem.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2444">No.</span></span> <span data-ttu-id="b4da5-2445">Alapértelmezett érték: 00:01:40</span><span class="sxs-lookup"><span data-stu-id="b4da5-2445">Default value: 00:01:40</span></span> |


#### <a name="example"></a><span data-ttu-id="b4da5-2446">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2446">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "HttpSourceToAzureBlob",
            "description": "Copy from an HTTP source tooan Azure blob",
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

<span data-ttu-id="b4da5-2447">További információkért lásd: [HTTP összekötő](data-factory-http-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2447">For more information, see [HTTP connector](data-factory-http-connector.md#copy-activity-properties) article.</span></span>

## <a name="odata"></a><span data-ttu-id="b4da5-2448">OData</span><span class="sxs-lookup"><span data-stu-id="b4da5-2448">OData</span></span>

### <a name="linked-service"></a><span data-ttu-id="b4da5-2449">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-2449">Linked service</span></span>
<span data-ttu-id="b4da5-2450">egy OData toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**OData**, és adja meg a következő tulajdonságok hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2450">toodefine an OData linked service, set hello **type** of hello linked service too**OData**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="b4da5-2451">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2451">Property</span></span> | <span data-ttu-id="b4da5-2452">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2452">Description</span></span> | <span data-ttu-id="b4da5-2453">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2453">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-2454">URL-címe</span><span class="sxs-lookup"><span data-stu-id="b4da5-2454">url</span></span> |<span data-ttu-id="b4da5-2455">Hello OData-szolgáltatás URL-címét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2455">Url of hello OData service.</span></span> |<span data-ttu-id="b4da5-2456">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2456">Yes</span></span> |
| <span data-ttu-id="b4da5-2457">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="b4da5-2457">authenticationType</span></span> |<span data-ttu-id="b4da5-2458">Tooconnect toohello OData-forrásra használt hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2458">Type of authentication used tooconnect toohello OData source.</span></span> <br/><br/> <span data-ttu-id="b4da5-2459">A felhőbeli OData a lehetséges értékek: névtelen, alapszintű és OAuth (Megjegyzés: jelenleg csak Azure Data Factory támogatási Azure Active Directory-alapú OAuth).</span><span class="sxs-lookup"><span data-stu-id="b4da5-2459">For cloud OData, possible values are Anonymous, Basic, and OAuth (note Azure Data Factory currently only support Azure Active Directory based OAuth).</span></span> <br/><br/> <span data-ttu-id="b4da5-2460">A helyszíni OData lehetséges értékei a névtelen, alapszintű és a Windows.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2460">For on-premises OData, possible values are Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="b4da5-2461">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2461">Yes</span></span> |
| <span data-ttu-id="b4da5-2462">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="b4da5-2462">username</span></span> |<span data-ttu-id="b4da5-2463">Ha egyszerű hitelesítést használ, adja meg a felhasználónevet.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2463">Specify user name if you are using Basic authentication.</span></span> |<span data-ttu-id="b4da5-2464">Igen (Ha csak az egyszerű hitelesítés használata esetén)</span><span class="sxs-lookup"><span data-stu-id="b4da5-2464">Yes (only if you are using Basic authentication)</span></span> |
| <span data-ttu-id="b4da5-2465">jelszó</span><span class="sxs-lookup"><span data-stu-id="b4da5-2465">password</span></span> |<span data-ttu-id="b4da5-2466">Adja meg a megadott felhasználónévhez hello hello felhasználói fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2466">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="b4da5-2467">Igen (Ha csak az egyszerű hitelesítés használata esetén)</span><span class="sxs-lookup"><span data-stu-id="b4da5-2467">Yes (only if you are using Basic authentication)</span></span> |
| <span data-ttu-id="b4da5-2468">authorizedCredential</span><span class="sxs-lookup"><span data-stu-id="b4da5-2468">authorizedCredential</span></span> |<span data-ttu-id="b4da5-2469">Ha OAuth használ, kattintson a **engedélyezés** hello Data Factory másolása varázsló vagy a szerkesztő gombra, és adja meg a hitelesítő adatok, akkor a tulajdonság értékének hello lesz automatikusan generált.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2469">If you are using OAuth, click **Authorize** button in hello Data Factory Copy Wizard or Editor and enter your credential, then hello value of this property will be auto-generated.</span></span> |<span data-ttu-id="b4da5-2470">Igen (csak ha OAuth-hitelesítés használata esetén)</span><span class="sxs-lookup"><span data-stu-id="b4da5-2470">Yes (only if you are using OAuth authentication)</span></span> |
| <span data-ttu-id="b4da5-2471">gatewayName</span><span class="sxs-lookup"><span data-stu-id="b4da5-2471">gatewayName</span></span> |<span data-ttu-id="b4da5-2472">Hello átjáró, amely a Data Factory szolgáltatásnak hello nevét használja tooconnect toohello helyszíni OData-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2472">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises OData service.</span></span> <span data-ttu-id="b4da5-2473">Csak adja meg, ha a másolt adatokat a helyszíni OData-forrásra.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2473">Specify only if you are copying data from on-prem OData source.</span></span> |<span data-ttu-id="b4da5-2474">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2474">No</span></span> |

#### <a name="example---using-basic-authentication"></a><span data-ttu-id="b4da5-2475">Példa – egyszerű hitelesítés használata</span><span class="sxs-lookup"><span data-stu-id="b4da5-2475">Example - Using Basic authentication</span></span>
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

#### <a name="example---using-anonymous-authentication"></a><span data-ttu-id="b4da5-2476">Példa - névtelen hitelesítés használatával</span><span class="sxs-lookup"><span data-stu-id="b4da5-2476">Example - Using Anonymous authentication</span></span>

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

#### <a name="example---using-windows-authentication-accessing-on-premises-odata-source"></a><span data-ttu-id="b4da5-2477">Példa - használatával Windows hitelesítés használata a helyszíni OData-forrásra</span><span class="sxs-lookup"><span data-stu-id="b4da5-2477">Example - Using Windows authentication accessing on-premises OData source</span></span>

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

#### <a name="example---using-oauth-authentication-accessing-cloud-odata-source"></a><span data-ttu-id="b4da5-2478">Példa - felhő OData-forrásra elérése OAuth-hitelesítés használatával</span><span class="sxs-lookup"><span data-stu-id="b4da5-2478">Example - Using OAuth authentication accessing cloud OData source</span></span>
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
            "authorizedCredential": "<auto generated by clicking hello Authorize button on UI>"
        }
    }
}
```

<span data-ttu-id="b4da5-2479">További információkért lásd: [OData összekötő](data-factory-odata-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2479">For more information, see [OData connector](data-factory-odata-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="b4da5-2480">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-2480">Dataset</span></span>
<span data-ttu-id="b4da5-2481">toodefine egy OData adatkészlet set hello **típus** hello adatkészlet túl**ODataResource**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2481">toodefine an OData dataset, set hello **type** of hello dataset too**ODataResource**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="b4da5-2482">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2482">Property</span></span> | <span data-ttu-id="b4da5-2483">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2483">Description</span></span> | <span data-ttu-id="b4da5-2484">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2484">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-2485">Elérési út</span><span class="sxs-lookup"><span data-stu-id="b4da5-2485">path</span></span> |<span data-ttu-id="b4da5-2486">Elérési út toohello OData-erőforrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2486">Path toohello OData resource</span></span> |<span data-ttu-id="b4da5-2487">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2487">No</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-2488">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2488">Example</span></span>

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

<span data-ttu-id="b4da5-2489">További információkért lásd: [OData összekötő](data-factory-odata-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2489">For more information, see [OData connector](data-factory-odata-connector.md#dataset-properties) article.</span></span>

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="b4da5-2490">A másolási tevékenység relációs adatforrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2490">Relational Source in Copy Activity</span></span>
<span data-ttu-id="b4da5-2491">OData forrásból származó adatok másolása, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**RelationalSource**, és adja meg a következő tulajdonságok hello **forrás** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2491">If you are copying data from an OData source, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="b4da5-2492">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2492">Property</span></span> | <span data-ttu-id="b4da5-2493">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2493">Description</span></span> | <span data-ttu-id="b4da5-2494">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2494">Example</span></span> | <span data-ttu-id="b4da5-2495">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2495">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-2496">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="b4da5-2496">query</span></span> |<span data-ttu-id="b4da5-2497">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2497">Use hello custom query tooread data.</span></span> |<span data-ttu-id="b4da5-2498">"? $select neve, leírása és $top = = 5"</span><span class="sxs-lookup"><span data-stu-id="b4da5-2498">"?$select=Name, Description&$top=5"</span></span> |<span data-ttu-id="b4da5-2499">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2499">No</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-2500">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2500">Example</span></span>

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

<span data-ttu-id="b4da5-2501">További információkért lásd: [OData összekötő](data-factory-odata-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2501">For more information, see [OData connector](data-factory-odata-connector.md#copy-activity-properties) article.</span></span>


## <a name="odbc"></a><span data-ttu-id="b4da5-2502">ODBC</span><span class="sxs-lookup"><span data-stu-id="b4da5-2502">ODBC</span></span>


### <a name="linked-service"></a><span data-ttu-id="b4da5-2503">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-2503">Linked service</span></span>
<span data-ttu-id="b4da5-2504">az ODBC toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**OnPremisesOdbc**, és adja meg a következő tulajdonságok hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2504">toodefine an ODBC linked service, set hello **type** of hello linked service too**OnPremisesOdbc**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="b4da5-2505">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2505">Property</span></span> | <span data-ttu-id="b4da5-2506">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2506">Description</span></span> | <span data-ttu-id="b4da5-2507">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2507">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-2508">connectionString</span><span class="sxs-lookup"><span data-stu-id="b4da5-2508">connectionString</span></span> |<span data-ttu-id="b4da5-2509">hello nem hozzáférési hitelesítő adatok része hello kapcsolati karakterláncot, és egy nem kötelező hitelesítő adat titkosítva.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2509">hello non-access credential portion of hello connection string and an optional encrypted credential.</span></span> <span data-ttu-id="b4da5-2510">Példák a következő részekben hello.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2510">See examples in hello following sections.</span></span> |<span data-ttu-id="b4da5-2511">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2511">Yes</span></span> |
| <span data-ttu-id="b4da5-2512">hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="b4da5-2512">credential</span></span> |<span data-ttu-id="b4da5-2513">hello hozzáférési hitelesítő adatok része, illesztőprogram-specifikus tulajdonság-érték formátumban megadott hello kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2513">hello access credential portion of hello connection string specified in driver-specific property-value format.</span></span> <span data-ttu-id="b4da5-2514">Példa: "Uid =<user ID>; Pwd =<password>; RefreshToken =<secret refresh token>; ".</span><span class="sxs-lookup"><span data-stu-id="b4da5-2514">Example: “Uid=<user ID>;Pwd=<password>;RefreshToken=<secret refresh token>;”.</span></span> |<span data-ttu-id="b4da5-2515">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2515">No</span></span> |
| <span data-ttu-id="b4da5-2516">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="b4da5-2516">authenticationType</span></span> |<span data-ttu-id="b4da5-2517">Tooconnect toohello ODBC adattár használt hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2517">Type of authentication used tooconnect toohello ODBC data store.</span></span> <span data-ttu-id="b4da5-2518">Lehetséges értékek a következők: névtelen és alapvető.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2518">Possible values are: Anonymous and Basic.</span></span> |<span data-ttu-id="b4da5-2519">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2519">Yes</span></span> |
| <span data-ttu-id="b4da5-2520">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="b4da5-2520">username</span></span> |<span data-ttu-id="b4da5-2521">Ha egyszerű hitelesítést használ, adja meg a felhasználónevet.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2521">Specify user name if you are using Basic authentication.</span></span> |<span data-ttu-id="b4da5-2522">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2522">No</span></span> |
| <span data-ttu-id="b4da5-2523">jelszó</span><span class="sxs-lookup"><span data-stu-id="b4da5-2523">password</span></span> |<span data-ttu-id="b4da5-2524">Adja meg a megadott felhasználónévhez hello hello felhasználói fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2524">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="b4da5-2525">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2525">No</span></span> |
| <span data-ttu-id="b4da5-2526">gatewayName</span><span class="sxs-lookup"><span data-stu-id="b4da5-2526">gatewayName</span></span> |<span data-ttu-id="b4da5-2527">Hello átjáró, amely a Data Factory szolgáltatásnak hello neve tooconnect toohello ODBC adattárat kell használnia.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2527">Name of hello gateway that hello Data Factory service should use tooconnect toohello ODBC data store.</span></span> |<span data-ttu-id="b4da5-2528">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2528">Yes</span></span> |

#### <a name="example---using-basic-authentication"></a><span data-ttu-id="b4da5-2529">Példa – egyszerű hitelesítés használata</span><span class="sxs-lookup"><span data-stu-id="b4da5-2529">Example - Using Basic authentication</span></span>

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
#### <a name="example---using-basic-authentication-with-encrypted-credentials"></a><span data-ttu-id="b4da5-2530">Példa – egyszerű hitelesítés használata a titkosított hitelesítő adatokat</span><span class="sxs-lookup"><span data-stu-id="b4da5-2530">Example - Using Basic authentication with encrypted credentials</span></span>
<span data-ttu-id="b4da5-2531">Hello hitelesítő adatok hello segítségével titkosíthatja [New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) (Azure PowerShell 1.0-ás verziója) parancsmag vagy [New-AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0.9 vagy korábbi verziójú hello Az Azure PowerShell).</span><span class="sxs-lookup"><span data-stu-id="b4da5-2531">You can encrypt hello credentials using hello [New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) (1.0 version of Azure PowerShell) cmdlet or [New-AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0.9 or earlier version of hello Azure PowerShell).</span></span>  

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

#### <a name="example-using-anonymous-authentication"></a><span data-ttu-id="b4da5-2532">Példa: A névtelen hitelesítést használó</span><span class="sxs-lookup"><span data-stu-id="b4da5-2532">Example: Using Anonymous authentication</span></span>

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

<span data-ttu-id="b4da5-2533">További információkért lásd: [ODBC összekötő](data-factory-odbc-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2533">For more information, see [ODBC connector](data-factory-odbc-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="b4da5-2534">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-2534">Dataset</span></span>
<span data-ttu-id="b4da5-2535">toodefine egy ODBC adatkészlet set hello **típus** hello adatkészlet túl**RelationalTable**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2535">toodefine an ODBC dataset, set hello **type** of hello dataset too**RelationalTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="b4da5-2536">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2536">Property</span></span> | <span data-ttu-id="b4da5-2537">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2537">Description</span></span> | <span data-ttu-id="b4da5-2538">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2538">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-2539">tableName</span><span class="sxs-lookup"><span data-stu-id="b4da5-2539">tableName</span></span> |<span data-ttu-id="b4da5-2540">Hello ODBC adattár hello tábla neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2540">Name of hello table in hello ODBC data store.</span></span> |<span data-ttu-id="b4da5-2541">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2541">Yes</span></span> |


#### <a name="example"></a><span data-ttu-id="b4da5-2542">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2542">Example</span></span>

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

<span data-ttu-id="b4da5-2543">További információkért lásd: [ODBC összekötő](data-factory-odbc-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2543">For more information, see [ODBC connector](data-factory-odbc-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="b4da5-2544">A másolási tevékenység relációs adatforrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2544">Relational Source in Copy Activity</span></span>
<span data-ttu-id="b4da5-2545">Adatok másolása egy ODBC adattárból, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**RelationalSource**, és adja meg a következő tulajdonságok hello **forrás** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2545">If you are copying data from an ODBC data store, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="b4da5-2546">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2546">Property</span></span> | <span data-ttu-id="b4da5-2547">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2547">Description</span></span> | <span data-ttu-id="b4da5-2548">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-2548">Allowed values</span></span> | <span data-ttu-id="b4da5-2549">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2549">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-2550">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="b4da5-2550">query</span></span> |<span data-ttu-id="b4da5-2551">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2551">Use hello custom query tooread data.</span></span> |<span data-ttu-id="b4da5-2552">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2552">SQL query string.</span></span> <span data-ttu-id="b4da5-2553">Például: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2553">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="b4da5-2554">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2554">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-2555">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2555">Example</span></span>

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

<span data-ttu-id="b4da5-2556">További információkért lásd: [ODBC összekötő](data-factory-odbc-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2556">For more information, see [ODBC connector](data-factory-odbc-connector.md#copy-activity-properties) article.</span></span>

## <a name="salesforce"></a><span data-ttu-id="b4da5-2557">Salesforce</span><span class="sxs-lookup"><span data-stu-id="b4da5-2557">Salesforce</span></span>


### <a name="linked-service"></a><span data-ttu-id="b4da5-2558">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-2558">Linked service</span></span>
<span data-ttu-id="b4da5-2559">a Salesforce toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**Salesforce**, és adja meg a következő tulajdonságok hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2559">toodefine a Salesforce linked service, set hello **type** of hello linked service too**Salesforce**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="b4da5-2560">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2560">Property</span></span> | <span data-ttu-id="b4da5-2561">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2561">Description</span></span> | <span data-ttu-id="b4da5-2562">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2562">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-2563">environmentUrl</span><span class="sxs-lookup"><span data-stu-id="b4da5-2563">environmentUrl</span></span> | <span data-ttu-id="b4da5-2564">Adja meg a hello URL-címet a Salesforce-példány.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2564">Specify hello URL of Salesforce instance.</span></span> <br><br> <span data-ttu-id="b4da5-2565">-Alapértelmezett érték a "https://login.salesforce.com".</span><span class="sxs-lookup"><span data-stu-id="b4da5-2565">- Default is "https://login.salesforce.com".</span></span> <br> <span data-ttu-id="b4da5-2566">-toocopy adatok védőfalak, adja meg a "https://test.salesforce.com".</span><span class="sxs-lookup"><span data-stu-id="b4da5-2566">- toocopy data from sandbox, specify "https://test.salesforce.com".</span></span> <br> <span data-ttu-id="b4da5-2567">-toocopy adatait egyéni tartományt, adja meg, például "https://[domain].my.salesforce.com".</span><span class="sxs-lookup"><span data-stu-id="b4da5-2567">- toocopy data from custom domain, specify, for example, "https://[domain].my.salesforce.com".</span></span> |<span data-ttu-id="b4da5-2568">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2568">No</span></span> |
| <span data-ttu-id="b4da5-2569">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="b4da5-2569">username</span></span> |<span data-ttu-id="b4da5-2570">Adjon meg egy felhasználónevet hello felhasználói fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2570">Specify a user name for hello user account.</span></span> |<span data-ttu-id="b4da5-2571">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2571">Yes</span></span> |
| <span data-ttu-id="b4da5-2572">jelszó</span><span class="sxs-lookup"><span data-stu-id="b4da5-2572">password</span></span> |<span data-ttu-id="b4da5-2573">Adjon meg egy hello felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2573">Specify a password for hello user account.</span></span> |<span data-ttu-id="b4da5-2574">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2574">Yes</span></span> |
| <span data-ttu-id="b4da5-2575">securityToken</span><span class="sxs-lookup"><span data-stu-id="b4da5-2575">securityToken</span></span> |<span data-ttu-id="b4da5-2576">Adjon meg egy biztonsági jogkivonatot hello felhasználói fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2576">Specify a security token for hello user account.</span></span> <span data-ttu-id="b4da5-2577">Lásd: [biztonsági jogkivonatának beszerzéséhez](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) útmutatást tooreset/get egy biztonsági jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2577">See [Get security token](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) for instructions on how tooreset/get a security token.</span></span> <span data-ttu-id="b4da5-2578">biztonsági jogkivonatok kapcsolatos toolearn általában, lásd: [biztonsági és hello API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).</span><span class="sxs-lookup"><span data-stu-id="b4da5-2578">toolearn about security tokens in general, see [Security and hello API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).</span></span> |<span data-ttu-id="b4da5-2579">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2579">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-2580">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2580">Example</span></span>

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

<span data-ttu-id="b4da5-2581">További információkért lásd: [Salesforce összekötő](data-factory-salesforce-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2581">For more information, see [Salesforce connector](data-factory-salesforce-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="b4da5-2582">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-2582">Dataset</span></span>
<span data-ttu-id="b4da5-2583">toodefine a Salesforce-adatkészlet set hello **típus** hello adatkészlet túl**RelationalTable**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2583">toodefine a Salesforce dataset, set hello **type** of hello dataset too**RelationalTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="b4da5-2584">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2584">Property</span></span> | <span data-ttu-id="b4da5-2585">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2585">Description</span></span> | <span data-ttu-id="b4da5-2586">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2586">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-2587">tableName</span><span class="sxs-lookup"><span data-stu-id="b4da5-2587">tableName</span></span> |<span data-ttu-id="b4da5-2588">A Salesforce-ban hello tábla neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2588">Name of hello table in Salesforce.</span></span> |<span data-ttu-id="b4da5-2589">Nem (Ha egy **lekérdezés** a **RelationalSource** van megadva)</span><span class="sxs-lookup"><span data-stu-id="b4da5-2589">No (if a **query** of **RelationalSource** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-2590">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2590">Example</span></span>

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

<span data-ttu-id="b4da5-2591">További információkért lásd: [Salesforce összekötő](data-factory-salesforce-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2591">For more information, see [Salesforce connector](data-factory-salesforce-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="b4da5-2592">A másolási tevékenység relációs adatforrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2592">Relational Source in Copy Activity</span></span>
<span data-ttu-id="b4da5-2593">Adatok másolása a Salesforce, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**RelationalSource**, és adja meg a következő tulajdonságok hello **forrás** a szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2593">If you are copying data from Salesforce, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="b4da5-2594">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2594">Property</span></span> | <span data-ttu-id="b4da5-2595">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2595">Description</span></span> | <span data-ttu-id="b4da5-2596">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b4da5-2596">Allowed values</span></span> | <span data-ttu-id="b4da5-2597">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2597">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b4da5-2598">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="b4da5-2598">query</span></span> |<span data-ttu-id="b4da5-2599">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2599">Use hello custom query tooread data.</span></span> |<span data-ttu-id="b4da5-2600">Egy SQL-92 lekérdezés vagy [Salesforce objektum Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2600">A SQL-92 query or [Salesforce Object Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) query.</span></span> <span data-ttu-id="b4da5-2601">Például: `select * from MyTable__c`.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2601">For example:  `select * from MyTable__c`.</span></span> |<span data-ttu-id="b4da5-2602">Nem (ha hello **tableName** a hello **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="b4da5-2602">No (if hello **tableName** of hello **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-2603">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2603">Example</span></span>  



```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "SalesforceToAzureBlob",
            "description": "Copy from Salesforce tooan Azure blob",
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
> <span data-ttu-id="b4da5-2604">hello API neve "__c" részét hello bármilyen egyéni objektum szükséges.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2604">hello "__c" part of hello API Name is needed for any custom object.</span></span>

<span data-ttu-id="b4da5-2605">További információkért lásd: [Salesforce összekötő](data-factory-salesforce-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2605">For more information, see [Salesforce connector](data-factory-salesforce-connector.md#copy-activity-properties) article.</span></span> 

## <a name="web-data"></a><span data-ttu-id="b4da5-2606">Webes adatok</span><span class="sxs-lookup"><span data-stu-id="b4da5-2606">Web Data</span></span> 

### <a name="linked-service"></a><span data-ttu-id="b4da5-2607">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-2607">Linked service</span></span>
<span data-ttu-id="b4da5-2608">a webes toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**webes**, és adja meg a következő tulajdonságok hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2608">toodefine a Web linked service, set hello **type** of hello linked service too**Web**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="b4da5-2609">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2609">Property</span></span> | <span data-ttu-id="b4da5-2610">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2610">Description</span></span> | <span data-ttu-id="b4da5-2611">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2611">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-2612">URL-cím</span><span class="sxs-lookup"><span data-stu-id="b4da5-2612">Url</span></span> |<span data-ttu-id="b4da5-2613">URL-cím toohello webes forrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2613">URL toohello Web source</span></span> |<span data-ttu-id="b4da5-2614">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2614">Yes</span></span> |
| <span data-ttu-id="b4da5-2615">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="b4da5-2615">authenticationType</span></span> |<span data-ttu-id="b4da5-2616">Névtelen.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2616">Anonymous.</span></span> |<span data-ttu-id="b4da5-2617">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2617">Yes</span></span> |
 

#### <a name="example"></a><span data-ttu-id="b4da5-2618">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2618">Example</span></span>


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

<span data-ttu-id="b4da5-2619">További információkért lásd: [webes tábla összekötő](data-factory-web-table-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2619">For more information, see [Web Table connector](data-factory-web-table-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="b4da5-2620">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b4da5-2620">Dataset</span></span>
<span data-ttu-id="b4da5-2621">toodefine egy webes dataset set hello **típus** hello adatkészlet túl**Webtábla**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2621">toodefine a Web dataset, set hello **type** of hello dataset too**WebTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="b4da5-2622">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2622">Property</span></span> | <span data-ttu-id="b4da5-2623">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2623">Description</span></span> | <span data-ttu-id="b4da5-2624">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2624">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="b4da5-2625">type</span><span class="sxs-lookup"><span data-stu-id="b4da5-2625">type</span></span> |<span data-ttu-id="b4da5-2626">Hello dataset típusa.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2626">type of hello dataset.</span></span> <span data-ttu-id="b4da5-2627">be kell állítani túl**Webtábla**</span><span class="sxs-lookup"><span data-stu-id="b4da5-2627">must be set too**WebTable**</span></span> |<span data-ttu-id="b4da5-2628">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2628">Yes</span></span> |
| <span data-ttu-id="b4da5-2629">Elérési út</span><span class="sxs-lookup"><span data-stu-id="b4da5-2629">path</span></span> |<span data-ttu-id="b4da5-2630">Relatív URL-cím toohello erőforrás hello táblát tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2630">A relative URL toohello resource that contains hello table.</span></span> |<span data-ttu-id="b4da5-2631">Nem.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2631">No.</span></span> <span data-ttu-id="b4da5-2632">Ha nincs megadva elérési út, kapcsolódó hello szolgáltatásdefinícióban megadott csak hello URL szolgál.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2632">When path is not specified, only hello URL specified in hello linked service definition is used.</span></span> |
| <span data-ttu-id="b4da5-2633">Index</span><span class="sxs-lookup"><span data-stu-id="b4da5-2633">index</span></span> |<span data-ttu-id="b4da5-2634">hello tábla hello erőforrás hello indexe.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2634">hello index of hello table in hello resource.</span></span> <span data-ttu-id="b4da5-2635">Lásd: [Get index egy tábla egy HTML-lapon](#get-index-of-a-table-in-an-html-page) szakasz lépéseit toogetting index egy tábla egy HTML-lapon.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2635">See [Get index of a table in an HTML page](#get-index-of-a-table-in-an-html-page) section for steps toogetting index of a table in an HTML page.</span></span> |<span data-ttu-id="b4da5-2636">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2636">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="b4da5-2637">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2637">Example</span></span>

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

<span data-ttu-id="b4da5-2638">További információkért lásd: [webes tábla összekötő](data-factory-web-table-connector.md#dataset-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2638">For more information, see [Web Table connector](data-factory-web-table-connector.md#dataset-properties) article.</span></span> 

### <a name="web-source-in-copy-activity"></a><span data-ttu-id="b4da5-2639">A másolási tevékenység webes forrás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2639">Web Source in Copy Activity</span></span>
<span data-ttu-id="b4da5-2640">Adatok másolása egy webes táblából, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**WebSource**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2640">If you are copying data from a web table, set hello **source type** of hello copy activity too**WebSource**.</span></span> <span data-ttu-id="b4da5-2641">Ha a másolási tevékenység hello forrás jelenleg típusú **WebSource**, további tulajdonságok nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2641">Currently, when hello source in copy activity is of type **WebSource**, no additional properties are supported.</span></span>

#### <a name="example"></a><span data-ttu-id="b4da5-2642">Példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2642">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "WebTableToAzureBlob",
            "description": "Copy from a Web table tooan Azure blob",
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

<span data-ttu-id="b4da5-2643">További információkért lásd: [webes tábla összekötő](data-factory-web-table-connector.md#copy-activity-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2643">For more information, see [Web Table connector](data-factory-web-table-connector.md#copy-activity-properties) article.</span></span> 

## <a name="compute-environments"></a><span data-ttu-id="b4da5-2644">SZÁMÍTÁSI KÖRNYEZETEK</span><span class="sxs-lookup"><span data-stu-id="b4da5-2644">COMPUTE ENVIRONMENTS</span></span>
<span data-ttu-id="b4da5-2645">hello következő táblázatban hello számítási környezetek futtathat rajtuk adat-előállító és hello átalakítása tevékenységek által támogatott.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2645">hello following table lists hello compute environments supported by Data Factory and hello transformation activities that can run on them.</span></span> <span data-ttu-id="b4da5-2646">Ön hello számítási hello hivatkozásra kattintva érdeklődik toosee hello JSON-sémák társított szolgáltatás toolink azt tooa adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2646">Click hello link for hello compute you are interested in toosee hello JSON schemas for linked service toolink it tooa data factory.</span></span> 

| <span data-ttu-id="b4da5-2647">Számítási környezet</span><span class="sxs-lookup"><span data-stu-id="b4da5-2647">Compute environment</span></span> | <span data-ttu-id="b4da5-2648">Tevékenységek</span><span class="sxs-lookup"><span data-stu-id="b4da5-2648">Activities</span></span> |
| --- | --- |
| <span data-ttu-id="b4da5-2649">[Igény szerinti HDInsight-fürt](#on-demand-azure-hdinsight-cluster) vagy [saját HDInsight-fürt](#existing-azure-hdinsight-cluster)</span><span class="sxs-lookup"><span data-stu-id="b4da5-2649">[On-demand HDInsight cluster](#on-demand-azure-hdinsight-cluster) or [your own HDInsight cluster](#existing-azure-hdinsight-cluster)</span></span> |<span data-ttu-id="b4da5-2650">[.NET egyéni tevékenység](#net-custom-activity), [tevékenység Hive](#hdinsight-hive-activity), [tevékenység sertésfelmérés] (hdinsight-pig-tevékenység # [MapReduce művelethez](#hdinsight-mapreduce-activity), [Hadoop-Stream tevékenység](#hdinsight-streaming-activityd), [ A Spark-tevékenység](#hdinsight-spark-activity)</span><span class="sxs-lookup"><span data-stu-id="b4da5-2650">[.NET custom activity](#net-custom-activity), [Hive activity](#hdinsight-hive-activity), [Pig activity](#hdinsight-pig-activity, [MapReduce activity](#hdinsight-mapreduce-activity), [Hadoop streaming activity](#hdinsight-streaming-activityd), [Spark activity](#hdinsight-spark-activity)</span></span> |
| [<span data-ttu-id="b4da5-2651">Az Azure Batch</span><span class="sxs-lookup"><span data-stu-id="b4da5-2651">Azure Batch</span></span>](#azure-batch) |[<span data-ttu-id="b4da5-2652">.NET egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4da5-2652">.NET custom activity</span></span>](#net-custom-activity) |
| [<span data-ttu-id="b4da5-2653">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="b4da5-2653">Azure Machine Learning</span></span>](#azure-machine-learning) | <span data-ttu-id="b4da5-2654">[Számítógép-kötegelt végrehajtási tevékenység tanulási](#machine-learning-batch-execution-activity), [gép tanulási Update-Erőforrástevékenység](#machine-learning-update-resource-activity)</span><span class="sxs-lookup"><span data-stu-id="b4da5-2654">[Machine Learning Batch Execution Activity](#machine-learning-batch-execution-activity), [Machine Learning Update Resource Activity](#machine-learning-update-resource-activity)</span></span> |
| [<span data-ttu-id="b4da5-2655">Az Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="b4da5-2655">Azure Data Lake Analytics</span></span>](#azure-data-lake-analytics) |[<span data-ttu-id="b4da5-2656">Data Lake Analytics U-SQL</span><span class="sxs-lookup"><span data-stu-id="b4da5-2656">Data Lake Analytics U-SQL</span></span>](#data-lake-analytics-u-sql-activity) |
| <span data-ttu-id="b4da5-2657">[Az Azure SQL adatbázis](#azure-sql-database-1), [Azure SQL Data Warehouse](#azure-sql-data-warehouse-1), [SQL Server](#sql-server-1)</span><span class="sxs-lookup"><span data-stu-id="b4da5-2657">[Azure SQL Database](#azure-sql-database-1), [Azure SQL Data Warehouse](#azure-sql-data-warehouse-1), [SQL Server](#sql-server-1)</span></span> |[<span data-ttu-id="b4da5-2658">Tárolt eljárás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2658">Stored Procedure</span></span>](#stored-procedure-activity) |

## <a name="on-demand-azure-hdinsight-cluster"></a><span data-ttu-id="b4da5-2659">Igény szerinti Azure HDInsight-fürt</span><span class="sxs-lookup"><span data-stu-id="b4da5-2659">On-demand Azure HDInsight cluster</span></span>
<span data-ttu-id="b4da5-2660">hello Azure Data Factory szolgáltatásnak automatikusan hozhat létre egy Windows/Linux-alapú igény szerinti HDInsight fürt tooprocess adatok.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2660">hello Azure Data Factory service can automatically create a Windows/Linux-based on-demand HDInsight cluster tooprocess data.</span></span> <span data-ttu-id="b4da5-2661">hello fürt hello és hello (hello JSON linkedServiceName tulajdonsága) storage-fiók ugyanabban a régióban társított hello fürt jön létre.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2661">hello cluster is created in hello same region as hello storage account (linkedServiceName property in hello JSON) associated with hello cluster.</span></span> <span data-ttu-id="b4da5-2662">Átalakítás tevékenységek a szolgáltatásnak a következő hello futtatása: [.NET egyéni tevékenység](#net-custom-activity), [tevékenység Hive](#hdinsight-hive-activity), [tevékenység sertésfelmérés] (hdinsight-pig-tevékenység # [MapReduce művelethez ](#hdinsight-mapreduce-activity), [Hadoop-Stream tevékenység](#hdinsight-streaming-activityd), [tevékenység Spark](#hdinsight-spark-activity).</span><span class="sxs-lookup"><span data-stu-id="b4da5-2662">You can run hello following transformation activities on this linked service: [.NET custom activity](#net-custom-activity), [Hive activity](#hdinsight-hive-activity), [Pig activity](#hdinsight-pig-activity, [MapReduce activity](#hdinsight-mapreduce-activity), [Hadoop streaming activity](#hdinsight-streaming-activityd), [Spark activity](#hdinsight-spark-activity).</span></span> 

### <a name="linked-service"></a><span data-ttu-id="b4da5-2663">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-2663">Linked service</span></span> 
<span data-ttu-id="b4da5-2664">a következő táblázat hello hello Azure JSON-definícióból igény szerinti HDInsight kapcsolódó szolgáltatási használt hello tulajdonságok ismerteti.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2664">hello following table provides descriptions for hello properties used in hello Azure JSON definition of an on-demand HDInsight linked service.</span></span>

| <span data-ttu-id="b4da5-2665">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2665">Property</span></span> | <span data-ttu-id="b4da5-2666">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2666">Description</span></span> | <span data-ttu-id="b4da5-2667">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2667">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-2668">type</span><span class="sxs-lookup"><span data-stu-id="b4da5-2668">type</span></span> |<span data-ttu-id="b4da5-2669">hello típusú tulajdonságot kell beállítani, túl**HDInsightOnDemand**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2669">hello type property should be set too**HDInsightOnDemand**.</span></span> |<span data-ttu-id="b4da5-2670">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2670">Yes</span></span> |
| <span data-ttu-id="b4da5-2671">Nagyobbnak</span><span class="sxs-lookup"><span data-stu-id="b4da5-2671">clusterSize</span></span> |<span data-ttu-id="b4da5-2672">Hello fürt/adatok csomópontok száma.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2672">Number of worker/data nodes in hello cluster.</span></span> <span data-ttu-id="b4da5-2673">Ez a tulajdonság a megadott munkavégző csomópontokhoz hello száma együtt 2 átjárócsomópontokkal hello HDInsight-fürt jön létre.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2673">hello HDInsight cluster is created with 2 head nodes along with hello number of worker nodes you specify for this property.</span></span> <span data-ttu-id="b4da5-2674">hello csomópontra van, amely nem rendelkezik 4 mag, így egy 4 munkavégző csomópontot tartalmazó fürtben veszi 24 mag standard, D3 méretű (4\*a munkavégző csomópontokról, valamint 2 processzormag, 4 = 16\*az átjárócsomópontokkal processzormag, 4 = 8).</span><span class="sxs-lookup"><span data-stu-id="b4da5-2674">hello nodes are of size Standard_D3 that has 4 cores, so a 4 worker node cluster takes 24 cores (4\*4 = 16 cores for worker nodes, plus 2\*4 = 8 cores for head nodes).</span></span> <span data-ttu-id="b4da5-2675">Lásd: [hdinsight létrehozása Linux-alapú Hadoop-fürtök](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) hello standard, D3 réteg vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2675">See [Create Linux-based Hadoop clusters in HDInsight](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) for details about hello Standard_D3 tier.</span></span> |<span data-ttu-id="b4da5-2676">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2676">Yes</span></span> |
| <span data-ttu-id="b4da5-2677">a TimeToLive tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2677">timetolive</span></span> |<span data-ttu-id="b4da5-2678">hello megengedett üresjárati idő hello igény szerinti HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2678">hello allowed idle time for hello on-demand HDInsight cluster.</span></span> <span data-ttu-id="b4da5-2679">Meghatározza, mennyi ideig hello igény szerinti HDInsight-fürt aktív marad a művelet végrehajtása, ha nincsenek más aktív feladatok hello fürt tevékenység befejezését követően.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2679">Specifies how long hello on-demand HDInsight cluster stays alive after completion of an activity run if there are no other active jobs in hello cluster.</span></span><br/><br/><span data-ttu-id="b4da5-2680">Például ha egy tevékenységfuttatási fogad 6 percnél és timetolive nem beállítása too5 perc, hello fürt marad a 5 perc után hello életben hello tevékenységfuttatási feldolgozásának 6 percnél.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2680">For example, if an activity run takes 6 minutes and timetolive is set too5 minutes, hello cluster stays alive for 5 minutes after hello 6 minutes of processing hello activity run.</span></span> <span data-ttu-id="b4da5-2681">Ha egy másik tevékenységfuttatási hello 6 percnél ablak, hello által feldolgozott ugyanabban a fürtben.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2681">If another activity run is executed with hello 6 minutes window, it is processed by hello same cluster.</span></span><br/><br/><span data-ttu-id="b4da5-2682">Igény szerinti HDInsight-fürtök létrehozása során költséges (igénybe vehet), így használják ezt a beállítást egy adat-előállító szükséges tooimprove teljesítményének újból felhasználja az igény szerinti HDInsight-fürtök által.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2682">Creating an on-demand HDInsight cluster is an expensive operation (could take a while), so use this setting as needed tooimprove performance of a data factory by reusing an on-demand HDInsight cluster.</span></span><br/><br/><span data-ttu-id="b4da5-2683">Ha timetolive érték too0, amint hello tevékenység fut feldolgozott hello-fürtök törlése.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2683">If you set timetolive value too0, hello cluster is deleted as soon as hello activity run in processed.</span></span> <span data-ttu-id="b4da5-2684">A hello ugyanakkor, ha a magas érték, hello fürt felfüggesztheti üresjárati feleslegesen magas költségeket eredményez.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2684">On hello other hand, if you set a high value, hello cluster may stay idle unnecessarily resulting in high costs.</span></span> <span data-ttu-id="b4da5-2685">Ezért fontos, hogy állítsa a igények alapján hello megfelelő értékre.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2685">Therefore, it is important that you set hello appropriate value based on your needs.</span></span><br/><br/><span data-ttu-id="b4da5-2686">Több folyamatok megoszthatja hello hello igény szerinti HDInsight-fürt ugyanazt a példányát, ha hello timetolive tulajdonság értékének megfelelően van beállítva.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2686">Multiple pipelines can share hello same instance of hello on-demand HDInsight cluster if hello timetolive property value is appropriately set</span></span> |<span data-ttu-id="b4da5-2687">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2687">Yes</span></span> |
| <span data-ttu-id="b4da5-2688">Verzió</span><span class="sxs-lookup"><span data-stu-id="b4da5-2688">version</span></span> |<span data-ttu-id="b4da5-2689">Hello HDInsight-fürt verziószáma.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2689">Version of hello HDInsight cluster.</span></span> <span data-ttu-id="b4da5-2690">További információkért lásd: [HDInsight-verziókról támogatott az Azure Data Factory](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory).</span><span class="sxs-lookup"><span data-stu-id="b4da5-2690">For details, see [supported HDInsight versions in Azure Data Factory](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory).</span></span> |<span data-ttu-id="b4da5-2691">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2691">No</span></span> |
| <span data-ttu-id="b4da5-2692">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="b4da5-2692">linkedServiceName</span></span> |<span data-ttu-id="b4da5-2693">Az Azure Storage társított szolgáltatás toobe történő tárolására és feldolgozására adatok hello igény fürt által használt.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2693">Azure Storage linked service toobe used by hello on-demand cluster for storing and processing data.</span></span> <p><span data-ttu-id="b4da5-2694">Jelenleg nem hozható létre, amely egy Azure Data Lake Store hello tárolóként használ igény szerinti HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2694">Currently, you cannot create an on-demand HDInsight cluster that uses an Azure Data Lake Store as hello storage.</span></span> <span data-ttu-id="b4da5-2695">Ha azt szeretné, hogy toostore hello ezért az adatok a HDInsight-feldolgozás alatt álló egy Azure Data Lake Store-ból, a hello Azure Blob Storage toohello Azure Data Lake Store másolási tevékenység toocopy hello adatait használják.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2695">If you want toostore hello result data from HDInsight processing in an Azure Data Lake Store, use a Copy Activity toocopy hello data from hello Azure Blob Storage toohello Azure Data Lake Store.</span></span></p>  | <span data-ttu-id="b4da5-2696">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2696">Yes</span></span> |
| <span data-ttu-id="b4da5-2697">additionalLinkedServiceNames</span><span class="sxs-lookup"><span data-stu-id="b4da5-2697">additionalLinkedServiceNames</span></span> |<span data-ttu-id="b4da5-2698">Adja meg a további tárfiókok hello HDInsight a társított szolgáltatás, így hello Data Factory szolgáltatásnak is regisztrálja őket az Ön nevében.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2698">Specifies additional storage accounts for hello HDInsight linked service so that hello Data Factory service can register them on your behalf.</span></span> |<span data-ttu-id="b4da5-2699">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2699">No</span></span> |
| <span data-ttu-id="b4da5-2700">osType</span><span class="sxs-lookup"><span data-stu-id="b4da5-2700">osType</span></span> |<span data-ttu-id="b4da5-2701">Az operációs rendszer típusát.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2701">Type of operating system.</span></span> <span data-ttu-id="b4da5-2702">Két érték engedélyezett: (alapértelmezett) Windows és Linux</span><span class="sxs-lookup"><span data-stu-id="b4da5-2702">Allowed values are: Windows (default) and Linux</span></span> |<span data-ttu-id="b4da5-2703">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2703">No</span></span> |
| <span data-ttu-id="b4da5-2704">hcatalogLinkedServiceName</span><span class="sxs-lookup"><span data-stu-id="b4da5-2704">hcatalogLinkedServiceName</span></span> |<span data-ttu-id="b4da5-2705">Azure SQL társított hello neve szolgáltatási pont toohello HCatalog adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2705">hello name of Azure SQL linked service that point toohello HCatalog database.</span></span> <span data-ttu-id="b4da5-2706">hello igény szerinti HDInsight-fürt létrehozása hello metaadattárhoz hello Azure SQL database segítségével.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2706">hello on-demand HDInsight cluster is created by using hello Azure SQL database as hello metastore.</span></span> |<span data-ttu-id="b4da5-2707">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2707">No</span></span> |

### <a name="json-example"></a><span data-ttu-id="b4da5-2708">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2708">JSON example</span></span>
<span data-ttu-id="b4da5-2709">a következő JSON hello igény kapcsolódó HDInsight Linux-alapú szolgáltatás határozza meg.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2709">hello following JSON defines a Linux-based on-demand HDInsight linked service.</span></span> <span data-ttu-id="b4da5-2710">hello Data Factory szolgáltatásnak automatikusan létrehoz egy **Linux-alapú** HDInsight-fürt adatok szelet feldolgozása közben.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2710">hello Data Factory service automatically creates a **Linux-based** HDInsight cluster when processing a data slice.</span></span> 

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

<span data-ttu-id="b4da5-2711">További információkért lásd: [összekapcsolt szolgáltatások számítási](data-factory-compute-linked-services.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2711">For more information, see [Compute linked services](data-factory-compute-linked-services.md) article.</span></span> 

## <a name="existing-azure-hdinsight-cluster"></a><span data-ttu-id="b4da5-2712">Meglévő Azure HDInsight-fürt</span><span class="sxs-lookup"><span data-stu-id="b4da5-2712">Existing Azure HDInsight cluster</span></span>
<span data-ttu-id="b4da5-2713">A Data Factory egy Azure HDInsight kapcsolódó szolgáltatás tooregister saját HDInsight-fürtöt hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2713">You can create an Azure HDInsight linked service tooregister your own HDInsight cluster with Data Factory.</span></span> <span data-ttu-id="b4da5-2714">Adatok átalakítása tevékenységek a szolgáltatásnak a következő hello is futtathatja: [.NET egyéni tevékenység](#net-custom-activity), [tevékenység Hive](#hdinsight-hive-activity), [tevékenység sertésfelmérés] (hdinsight-pig-tevékenység # [MapReduce tevékenység](#hdinsight-mapreduce-activity), [Hadoop-Stream tevékenység](#hdinsight-streaming-activityd), [tevékenység Spark](#hdinsight-spark-activity).</span><span class="sxs-lookup"><span data-stu-id="b4da5-2714">You can run hello following data transformation activities on this linked service: [.NET custom activity](#net-custom-activity), [Hive activity](#hdinsight-hive-activity), [Pig activity](#hdinsight-pig-activity, [MapReduce activity](#hdinsight-mapreduce-activity), [Hadoop streaming activity](#hdinsight-streaming-activityd), [Spark activity](#hdinsight-spark-activity).</span></span> 

### <a name="linked-service"></a><span data-ttu-id="b4da5-2715">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-2715">Linked service</span></span>
<span data-ttu-id="b4da5-2716">hello a következő táblázat ismerteti az Azure HDInsight társított szolgáltatásnak Azure JSON-definícióból hello használt hello tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2716">hello following table provides descriptions for hello properties used in hello Azure JSON definition of an Azure HDInsight linked service.</span></span>

| <span data-ttu-id="b4da5-2717">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2717">Property</span></span> | <span data-ttu-id="b4da5-2718">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2718">Description</span></span> | <span data-ttu-id="b4da5-2719">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2719">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-2720">type</span><span class="sxs-lookup"><span data-stu-id="b4da5-2720">type</span></span> |<span data-ttu-id="b4da5-2721">hello típusú tulajdonságot kell beállítani, túl**HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2721">hello type property should be set too**HDInsight**.</span></span> |<span data-ttu-id="b4da5-2722">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2722">Yes</span></span> |
| <span data-ttu-id="b4da5-2723">clusterUri</span><span class="sxs-lookup"><span data-stu-id="b4da5-2723">clusterUri</span></span> |<span data-ttu-id="b4da5-2724">hello hello HDInsight-fürt URI.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2724">hello URI of hello HDInsight cluster.</span></span> |<span data-ttu-id="b4da5-2725">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2725">Yes</span></span> |
| <span data-ttu-id="b4da5-2726">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="b4da5-2726">username</span></span> |<span data-ttu-id="b4da5-2727">Adja meg felhasználói toobe hello hello nevét használja tooconnect tooan meglévő HDInsight-fürtre.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2727">Specify hello name of hello user toobe used tooconnect tooan existing HDInsight cluster.</span></span> |<span data-ttu-id="b4da5-2728">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2728">Yes</span></span> |
| <span data-ttu-id="b4da5-2729">jelszó</span><span class="sxs-lookup"><span data-stu-id="b4da5-2729">password</span></span> |<span data-ttu-id="b4da5-2730">Adja meg a hello felhasználói fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2730">Specify password for hello user account.</span></span> |<span data-ttu-id="b4da5-2731">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2731">Yes</span></span> |
| <span data-ttu-id="b4da5-2732">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="b4da5-2732">linkedServiceName</span></span> | <span data-ttu-id="b4da5-2733">Hello toohello Azure blob storage Azure Storage társított szolgáltatás neve hello által használt HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2733">Name of hello Azure Storage linked service that refers toohello Azure blob storage used by hello HDInsight cluster.</span></span> <p><span data-ttu-id="b4da5-2734">Jelenleg nem adhat meg egy Azure Data Lake Store társított szolgáltatás ehhez a tulajdonsághoz.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2734">Currently, you cannot specify an Azure Data Lake Store linked service for this property.</span></span> <span data-ttu-id="b4da5-2735">Előfordulhat, hogy éri hello Azure Data Lake Store a Hive/Pig-parancsfájlok hozzáférés toohello Data Lake Store hello HDInsight-fürt-e.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2735">You may access data in hello Azure Data Lake Store from Hive/Pig scripts if hello HDInsight cluster has access toohello Data Lake Store.</span></span> </p>  |<span data-ttu-id="b4da5-2736">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2736">Yes</span></span> |

<span data-ttu-id="b4da5-2737">A HDInsight-fürtök támogatott verzióit lásd: [HDInsight-verziókról támogatott](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory).</span><span class="sxs-lookup"><span data-stu-id="b4da5-2737">For versions of HDInsight clusters supported, see [supported HDInsight versions](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory).</span></span> 

#### <a name="json-example"></a><span data-ttu-id="b4da5-2738">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2738">JSON example</span></span>

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

## <a name="azure-batch"></a><span data-ttu-id="b4da5-2739">Azure Batch</span><span class="sxs-lookup"><span data-stu-id="b4da5-2739">Azure Batch</span></span>
<span data-ttu-id="b4da5-2740">A data Factory egy csatolt Azure Batch szolgáltatás tooregister Batch-készlet a virtuális gépek (VM) hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2740">You can create an Azure Batch linked service tooregister a Batch pool of virtual machines (VMs) with a data factory.</span></span> <span data-ttu-id="b4da5-2741">.NET-egyéni tevékenységek Azure Batch vagy Azure HDInsight segítségével is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2741">You can run .NET custom activities using either Azure Batch or Azure HDInsight.</span></span> <span data-ttu-id="b4da5-2742">Futtathatja a [.NET egyéni tevékenység](#net-custom-activity) a társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2742">You can run a [.NET custom activity](#net-custom-activity) on this linked service.</span></span> 

### <a name="linked-service"></a><span data-ttu-id="b4da5-2743">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-2743">Linked service</span></span>
<span data-ttu-id="b4da5-2744">a következő táblázat hello hello Azure JSON-definícióból egy csatolt Azure Batch szolgáltatás használt hello tulajdonságok ismerteti.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2744">hello following table provides descriptions for hello properties used in hello Azure JSON definition of an Azure Batch linked service.</span></span>

| <span data-ttu-id="b4da5-2745">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2745">Property</span></span> | <span data-ttu-id="b4da5-2746">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2746">Description</span></span> | <span data-ttu-id="b4da5-2747">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2747">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-2748">type</span><span class="sxs-lookup"><span data-stu-id="b4da5-2748">type</span></span> |<span data-ttu-id="b4da5-2749">hello típusú tulajdonságot kell beállítani, túl**AzureBatch**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2749">hello type property should be set too**AzureBatch**.</span></span> |<span data-ttu-id="b4da5-2750">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2750">Yes</span></span> |
| <span data-ttu-id="b4da5-2751">Fióknév</span><span class="sxs-lookup"><span data-stu-id="b4da5-2751">accountName</span></span> |<span data-ttu-id="b4da5-2752">Hello Azure Batch-fiók neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2752">Name of hello Azure Batch account.</span></span> |<span data-ttu-id="b4da5-2753">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2753">Yes</span></span> |
| <span data-ttu-id="b4da5-2754">accessKey</span><span class="sxs-lookup"><span data-stu-id="b4da5-2754">accessKey</span></span> |<span data-ttu-id="b4da5-2755">Hello Azure Batch-fiók elérési kulcsának.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2755">Access key for hello Azure Batch account.</span></span> |<span data-ttu-id="b4da5-2756">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2756">Yes</span></span> |
| <span data-ttu-id="b4da5-2757">poolName</span><span class="sxs-lookup"><span data-stu-id="b4da5-2757">poolName</span></span> |<span data-ttu-id="b4da5-2758">A virtuális gépek hello készlet neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2758">Name of hello pool of virtual machines.</span></span> |<span data-ttu-id="b4da5-2759">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2759">Yes</span></span> |
| <span data-ttu-id="b4da5-2760">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="b4da5-2760">linkedServiceName</span></span> |<span data-ttu-id="b4da5-2761">Hello a kapcsolódó Azure Batch-szolgáltatás társított Azure Storage társított szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2761">Name of hello Azure Storage linked service associated with this Azure Batch linked service.</span></span> <span data-ttu-id="b4da5-2762">A társított szolgáltatás szolgál átmeneti fájlok toorun hello tevékenység és a hello tevékenység végrehajtási naplók tárolásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2762">This linked service is used for staging files required toorun hello activity and storing hello activity execution logs.</span></span> |<span data-ttu-id="b4da5-2763">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2763">Yes</span></span> |


#### <a name="json-example"></a><span data-ttu-id="b4da5-2764">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2764">JSON example</span></span>

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

## <a name="azure-machine-learning"></a><span data-ttu-id="b4da5-2765">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="b4da5-2765">Azure Machine Learning</span></span>
<span data-ttu-id="b4da5-2766">A Machine Learning kötegelt pontozási végpont a data Factory létrehozása az Azure Machine Learning kapcsolódó szolgáltatás tooregister.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2766">You create an Azure Machine Learning linked service tooregister a Machine Learning batch scoring endpoint with a data factory.</span></span> <span data-ttu-id="b4da5-2767">A társított szolgáltatás futtatható ez két adatok átalakítása tevékenységek: [Machine Learning kötegelt végrehajtási tevékenység](#machine-learning-batch-execution-activity), [Machine Learning Update-Erőforrástevékenység](#machine-learning-update-resource-activity).</span><span class="sxs-lookup"><span data-stu-id="b4da5-2767">Two data transformation activities that can run on this linked service: [Machine Learning Batch Execution Activity](#machine-learning-batch-execution-activity), [Machine Learning Update Resource Activity](#machine-learning-update-resource-activity).</span></span> 

### <a name="linked-service"></a><span data-ttu-id="b4da5-2768">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-2768">Linked service</span></span>
<span data-ttu-id="b4da5-2769">hello a következő táblázat ismerteti az Azure Machine Learning társított szolgáltatásnak Azure JSON-definícióból hello használt hello tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2769">hello following table provides descriptions for hello properties used in hello Azure JSON definition of an Azure Machine Learning linked service.</span></span>

| <span data-ttu-id="b4da5-2770">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2770">Property</span></span> | <span data-ttu-id="b4da5-2771">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2771">Description</span></span> | <span data-ttu-id="b4da5-2772">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2772">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-2773">Típus</span><span class="sxs-lookup"><span data-stu-id="b4da5-2773">Type</span></span> |<span data-ttu-id="b4da5-2774">hello tulajdonságra kell megadni: **AzureML**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2774">hello type property should be set to: **AzureML**.</span></span> |<span data-ttu-id="b4da5-2775">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2775">Yes</span></span> |
| <span data-ttu-id="b4da5-2776">mlEndpoint</span><span class="sxs-lookup"><span data-stu-id="b4da5-2776">mlEndpoint</span></span> |<span data-ttu-id="b4da5-2777">hello kötegelt pontozás URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2777">hello batch scoring URL.</span></span> |<span data-ttu-id="b4da5-2778">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2778">Yes</span></span> |
| <span data-ttu-id="b4da5-2779">apiKey</span><span class="sxs-lookup"><span data-stu-id="b4da5-2779">apiKey</span></span> |<span data-ttu-id="b4da5-2780">hello közzétett munkaterület-modell API.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2780">hello published workspace model’s API.</span></span> |<span data-ttu-id="b4da5-2781">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2781">Yes</span></span> |

#### <a name="json-example"></a><span data-ttu-id="b4da5-2782">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2782">JSON example</span></span>

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

## <a name="azure-data-lake-analytics"></a><span data-ttu-id="b4da5-2783">Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="b4da5-2783">Azure Data Lake Analytics</span></span>
<span data-ttu-id="b4da5-2784">Létrehozhat egy **Azure Data Lake Analytics** szolgáltatás toolink egy Azure Data Lake Analytics számítási szolgáltatás tooan az Azure data factory kapcsolt hello használata előtt [Data Lake Analytics U-SQL-tevékenység](data-factory-usql-activity.md) egy folyamaton belül .</span><span class="sxs-lookup"><span data-stu-id="b4da5-2784">You create an **Azure Data Lake Analytics** linked service toolink an Azure Data Lake Analytics compute service tooan Azure data factory before using hello [Data Lake Analytics U-SQL activity](data-factory-usql-activity.md) in a pipeline.</span></span>

### <a name="linked-service"></a><span data-ttu-id="b4da5-2785">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-2785">Linked service</span></span>

<span data-ttu-id="b4da5-2786">hello a következő táblázat ismerteti az Azure Data Lake Analytics társított szolgáltatás JSON-definícióból hello használt hello tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2786">hello following table provides descriptions for hello properties used in hello JSON definition of an Azure Data Lake Analytics linked service.</span></span> 

| <span data-ttu-id="b4da5-2787">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2787">Property</span></span> | <span data-ttu-id="b4da5-2788">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2788">Description</span></span> | <span data-ttu-id="b4da5-2789">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2789">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-2790">Típus</span><span class="sxs-lookup"><span data-stu-id="b4da5-2790">Type</span></span> |<span data-ttu-id="b4da5-2791">hello tulajdonságra kell megadni: **AzureDataLakeAnalytics**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2791">hello type property should be set to: **AzureDataLakeAnalytics**.</span></span> |<span data-ttu-id="b4da5-2792">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2792">Yes</span></span> |
| <span data-ttu-id="b4da5-2793">Fióknév</span><span class="sxs-lookup"><span data-stu-id="b4da5-2793">accountName</span></span> |<span data-ttu-id="b4da5-2794">Az Azure Data Lake Analytics-fiók neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2794">Azure Data Lake Analytics Account Name.</span></span> |<span data-ttu-id="b4da5-2795">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2795">Yes</span></span> |
| <span data-ttu-id="b4da5-2796">datalakeanalyticsuri paraméter</span><span class="sxs-lookup"><span data-stu-id="b4da5-2796">dataLakeAnalyticsUri</span></span> |<span data-ttu-id="b4da5-2797">Az Azure Data Lake Analytics URI.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2797">Azure Data Lake Analytics URI.</span></span> |<span data-ttu-id="b4da5-2798">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2798">No</span></span> |
| <span data-ttu-id="b4da5-2799">Engedélyezési</span><span class="sxs-lookup"><span data-stu-id="b4da5-2799">authorization</span></span> |<span data-ttu-id="b4da5-2800">Engedélyezési kód automatikusan beolvassa kattintás után **engedélyezés** gombra a Data Factory Editor hello és épp hello OAuth-bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2800">Authorization code is automatically retrieved after clicking **Authorize** button in hello Data Factory Editor and completing hello OAuth login.</span></span> |<span data-ttu-id="b4da5-2801">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2801">Yes</span></span> |
| <span data-ttu-id="b4da5-2802">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="b4da5-2802">subscriptionId</span></span> |<span data-ttu-id="b4da5-2803">Az Azure előfizetés-azonosító</span><span class="sxs-lookup"><span data-stu-id="b4da5-2803">Azure subscription id</span></span> |<span data-ttu-id="b4da5-2804">Nem (Ha nincs megadva, az adat-előállító használt hello előfizetés).</span><span class="sxs-lookup"><span data-stu-id="b4da5-2804">No (If not specified, subscription of hello data factory is used).</span></span> |
| <span data-ttu-id="b4da5-2805">erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="b4da5-2805">resourceGroupName</span></span> |<span data-ttu-id="b4da5-2806">Azure erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="b4da5-2806">Azure resource group name</span></span> |<span data-ttu-id="b4da5-2807">Nem (Ha nincs megadva, az adat-előállító használt hello erőforráscsoport).</span><span class="sxs-lookup"><span data-stu-id="b4da5-2807">No (If not specified, resource group of hello data factory is used).</span></span> |
| <span data-ttu-id="b4da5-2808">Munkamenet-azonosító</span><span class="sxs-lookup"><span data-stu-id="b4da5-2808">sessionId</span></span> |<span data-ttu-id="b4da5-2809">munkamenet-azonosító hello OAuth hitelesítési munkamenetből.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2809">session id from hello OAuth authorization session.</span></span> <span data-ttu-id="b4da5-2810">Minden munkamenet-azonosító egyedi, és előfordulhat, hogy csak egyszer használható.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2810">Each session id is unique and may only be used once.</span></span> <span data-ttu-id="b4da5-2811">Data Factory Editor hello használata esetén ezt az Azonosítót jön létre automatikusan.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2811">When you use hello Data Factory Editor, this ID is auto-generated.</span></span> |<span data-ttu-id="b4da5-2812">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2812">Yes</span></span> |


#### <a name="json-example"></a><span data-ttu-id="b4da5-2813">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2813">JSON example</span></span>
<span data-ttu-id="b4da5-2814">a következő példa hello biztosít az Azure Data Lake Analytics társított szolgáltatás JSON-definícióból.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2814">hello following example provides JSON definition for an Azure Data Lake Analytics linked service.</span></span>

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

## <a name="azure-sql-database"></a><span data-ttu-id="b4da5-2815">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="b4da5-2815">Azure SQL Database</span></span>
<span data-ttu-id="b4da5-2816">Hozzon létre egy Azure SQL társított szolgáltatást, és használatához a hello [tárolt eljárási tevékenység](#stored-procedure-activity) tooinvoke a Data Factory-folyamathoz tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2816">You create an Azure SQL linked service and use it with hello [Stored Procedure Activity](#stored-procedure-activity) tooinvoke a stored procedure from a Data Factory pipeline.</span></span> 

### <a name="linked-service"></a><span data-ttu-id="b4da5-2817">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-2817">Linked service</span></span>
<span data-ttu-id="b4da5-2818">egy Azure SQL Database toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**AzureSqlDatabase**, és adja meg a következő tulajdonságok hello **typeProperties**szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2818">toodefine an Azure SQL Database linked service, set hello **type** of hello linked service too**AzureSqlDatabase**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="b4da5-2819">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2819">Property</span></span> | <span data-ttu-id="b4da5-2820">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2820">Description</span></span> | <span data-ttu-id="b4da5-2821">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2821">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-2822">connectionString</span><span class="sxs-lookup"><span data-stu-id="b4da5-2822">connectionString</span></span> |<span data-ttu-id="b4da5-2823">Adjon meg információt hello connectionString tulajdonság szükséges tooconnect toohello Azure SQL Database-példányt.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2823">Specify information needed tooconnect toohello Azure SQL Database instance for hello connectionString property.</span></span> |<span data-ttu-id="b4da5-2824">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2824">Yes</span></span> |

#### <a name="json-example"></a><span data-ttu-id="b4da5-2825">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2825">JSON example</span></span>

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

<span data-ttu-id="b4da5-2826">Lásd: [Azure SQL-összekötő](data-factory-azure-sql-connector.md#linked-service-properties) szóló cikkben olvashat a szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2826">See [Azure SQL Connector](data-factory-azure-sql-connector.md#linked-service-properties) article for details about this linked service.</span></span>

## <a name="azure-sql-data-warehouse"></a><span data-ttu-id="b4da5-2827">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="b4da5-2827">Azure SQL Data Warehouse</span></span>
<span data-ttu-id="b4da5-2828">Azure SQL Data Warehouse társított szolgáltatás létrehozása és használatához a hello [tárolt eljárási tevékenység](data-factory-stored-proc-activity.md) tooinvoke a Data Factory-folyamathoz tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2828">You create an Azure SQL Data Warehouse linked service and use it with hello [Stored Procedure Activity](data-factory-stored-proc-activity.md) tooinvoke a stored procedure from a Data Factory pipeline.</span></span> 

### <a name="linked-service"></a><span data-ttu-id="b4da5-2829">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-2829">Linked service</span></span>
<span data-ttu-id="b4da5-2830">Azure SQL Data Warehouse toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**AzureSqlDW**, és adja meg a következő tulajdonságok hello **typeProperties**szakasz:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2830">toodefine an Azure SQL Data Warehouse linked service, set hello **type** of hello linked service too**AzureSqlDW**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="b4da5-2831">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2831">Property</span></span> | <span data-ttu-id="b4da5-2832">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2832">Description</span></span> | <span data-ttu-id="b4da5-2833">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2833">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-2834">connectionString</span><span class="sxs-lookup"><span data-stu-id="b4da5-2834">connectionString</span></span> |<span data-ttu-id="b4da5-2835">Adjon meg információt hello connectionString tulajdonság szükséges tooconnect toohello Azure SQL Data Warehouse-példányhoz.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2835">Specify information needed tooconnect toohello Azure SQL Data Warehouse instance for hello connectionString property.</span></span> |<span data-ttu-id="b4da5-2836">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2836">Yes</span></span> |

#### <a name="json-example"></a><span data-ttu-id="b4da5-2837">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2837">JSON example</span></span>

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

<span data-ttu-id="b4da5-2838">További információkért lásd: [Azure SQL Data Warehouse-összekötő](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2838">For more information, see [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) article.</span></span> 

## <a name="sql-server"></a><span data-ttu-id="b4da5-2839">SQL Server</span><span class="sxs-lookup"><span data-stu-id="b4da5-2839">SQL Server</span></span> 
<span data-ttu-id="b4da5-2840">Hozzon létre csatolt SQL Server szolgáltatást, és használatához a hello [tárolt eljárási tevékenység](data-factory-stored-proc-activity.md) tooinvoke a Data Factory-folyamathoz tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2840">You create a SQL Server linked service and use it with hello [Stored Procedure Activity](data-factory-stored-proc-activity.md) tooinvoke a stored procedure from a Data Factory pipeline.</span></span> 

### <a name="linked-service"></a><span data-ttu-id="b4da5-2841">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4da5-2841">Linked service</span></span>
<span data-ttu-id="b4da5-2842">Típusú társított szolgáltatás létrehozása **OnPremisesSqlServer** toolink egy helyi SQL Server adatbázis tooa adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2842">You create a linked service of type **OnPremisesSqlServer** toolink an on-premises SQL Server database tooa data factory.</span></span> <span data-ttu-id="b4da5-2843">a következő táblázat hello biztosít JSON elemek adott tooon-hez kapcsolódó SQL Server szolgáltatás leírását.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2843">hello following table provides description for JSON elements specific tooon-premises SQL Server linked service.</span></span>

<span data-ttu-id="b4da5-2844">a következő táblázat hello biztosít JSON-elemek adott tooSQL csatolt kiszolgáló szolgáltatás leírását.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2844">hello following table provides description for JSON elements specific tooSQL Server linked service.</span></span>

| <span data-ttu-id="b4da5-2845">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2845">Property</span></span> | <span data-ttu-id="b4da5-2846">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2846">Description</span></span> | <span data-ttu-id="b4da5-2847">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2847">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-2848">type</span><span class="sxs-lookup"><span data-stu-id="b4da5-2848">type</span></span> |<span data-ttu-id="b4da5-2849">hello tulajdonságra kell megadni: **OnPremisesSqlServer**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2849">hello type property should be set to: **OnPremisesSqlServer**.</span></span> |<span data-ttu-id="b4da5-2850">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2850">Yes</span></span> |
| <span data-ttu-id="b4da5-2851">connectionString</span><span class="sxs-lookup"><span data-stu-id="b4da5-2851">connectionString</span></span> |<span data-ttu-id="b4da5-2852">Adja meg a connectionString információ tooconnect toohello a helyszíni SQL Server-adatbázis SQL-hitelesítéssel vagy a Windows-hitelesítés szükséges.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2852">Specify connectionString information needed tooconnect toohello on-premises SQL Server database using either SQL authentication or Windows authentication.</span></span> |<span data-ttu-id="b4da5-2853">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2853">Yes</span></span> |
| <span data-ttu-id="b4da5-2854">gatewayName</span><span class="sxs-lookup"><span data-stu-id="b4da5-2854">gatewayName</span></span> |<span data-ttu-id="b4da5-2855">Hello átjáró, amely a Data Factory szolgáltatásnak hello neve tooconnect toohello a helyszíni SQL Server-adatbázist használja.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2855">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises SQL Server database.</span></span> |<span data-ttu-id="b4da5-2856">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2856">Yes</span></span> |
| <span data-ttu-id="b4da5-2857">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="b4da5-2857">username</span></span> |<span data-ttu-id="b4da5-2858">Adja meg a felhasználónevet, ha a Windows-hitelesítést használ.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2858">Specify user name if you are using Windows Authentication.</span></span> <span data-ttu-id="b4da5-2859">Példa: **tartománynév\\felhasználónév**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2859">Example: **domainname\\username**.</span></span> |<span data-ttu-id="b4da5-2860">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2860">No</span></span> |
| <span data-ttu-id="b4da5-2861">jelszó</span><span class="sxs-lookup"><span data-stu-id="b4da5-2861">password</span></span> |<span data-ttu-id="b4da5-2862">Adja meg a megadott felhasználónévhez hello hello felhasználói fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2862">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="b4da5-2863">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2863">No</span></span> |

<span data-ttu-id="b4da5-2864">Hitelesítő adatok hello segítségével titkosíthatja **New-AzureRmDataFactoryEncryptValue** parancsmag, amelyekkel hello kapcsolati karakterlánc, ahogy az alábbi példa hello (**EncryptedCredential** tulajdonság):</span><span class="sxs-lookup"><span data-stu-id="b4da5-2864">You can encrypt credentials using hello **New-AzureRmDataFactoryEncryptValue** cmdlet and use them in hello connection string as shown in hello following example (**EncryptedCredential** property):</span></span>  

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```


#### <a name="example-json-for-using-sql-authentication"></a><span data-ttu-id="b4da5-2865">Példa: JSON az SQL-hitelesítéssel</span><span class="sxs-lookup"><span data-stu-id="b4da5-2865">Example: JSON for using SQL Authentication</span></span>

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
#### <a name="example-json-for-using-windows-authentication"></a><span data-ttu-id="b4da5-2866">Példa: JSON a Windows-hitelesítés használatával</span><span class="sxs-lookup"><span data-stu-id="b4da5-2866">Example: JSON for using Windows Authentication</span></span>

<span data-ttu-id="b4da5-2867">Ha a felhasználónév és jelszó van adva, átjáró használja őket tooimpersonate hello megadott felhasználói fiók tooconnect toohello a helyszíni SQL Server-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2867">If username and password are specified, gateway uses them tooimpersonate hello specified user account tooconnect toohello on-premises SQL Server database.</span></span> <span data-ttu-id="b4da5-2868">Ellenkező esetben átjáró csatlakozik toohello SQL Server közvetlenül (az indítási fiókjához) átjáró hello biztonsági környezetében.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2868">Otherwise, gateway connects toohello SQL Server directly with hello security context of Gateway (its startup account).</span></span>

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

<span data-ttu-id="b4da5-2869">További információkért lásd: [SQL Server-összekötő](data-factory-sqlserver-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2869">For more information, see [SQL Server connector](data-factory-sqlserver-connector.md#linked-service-properties) article.</span></span>

## <a name="data-transformation-activities"></a><span data-ttu-id="b4da5-2870">ADATOK ÁTALAKÍTÁSA TEVÉKENYSÉGEK</span><span class="sxs-lookup"><span data-stu-id="b4da5-2870">DATA TRANSFORMATION ACTIVITIES</span></span>

<span data-ttu-id="b4da5-2871">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4da5-2871">Activity</span></span> | <span data-ttu-id="b4da5-2872">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2872">Description</span></span>
-------- | -----------
[<span data-ttu-id="b4da5-2873">HDInsight Hive tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4da5-2873">HDInsight Hive activity</span></span>](#hdinsight-hive-activity) | <span data-ttu-id="b4da5-2874">a Data Factory-folyamathoz HDInsight Hive tevékenység hello Hive-lekérdezéseket a saját vagy igény szerinti Windows/Linux-alapú HDInsight-fürt hajt végre.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2874">hello HDInsight Hive activity in a Data Factory pipeline executes Hive queries on your own or on-demand Windows/Linux-based HDInsight cluster.</span></span> 
[<span data-ttu-id="b4da5-2875">HDInsight Pig tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4da5-2875">HDInsight Pig activity</span></span>](#hdinsight-pig-activity) | <span data-ttu-id="b4da5-2876">a Data Factory-folyamathoz HDInsight Pig tevékenység hello Pig lekérdezések saját vagy igény szerinti Windows/Linux-alapú HDInsight-fürt hajt végre.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2876">hello HDInsight Pig activity in a Data Factory pipeline executes Pig queries on your own or on-demand Windows/Linux-based HDInsight cluster.</span></span>
[<span data-ttu-id="b4da5-2877">HDInsight MapReduce-tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4da5-2877">HDInsight MapReduce Activity</span></span>](#hdinsight-mapreduce-activity) | <span data-ttu-id="b4da5-2878">hello HDInsight MapReduce művelethez a Data Factory-folyamat saját MapReduce programok vagy igény szerinti Windows/Linux-alapú HDInsight-fürt hajt végre.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2878">hello HDInsight MapReduce activity in a Data Factory pipeline executes MapReduce programs on your own or on-demand Windows/Linux-based HDInsight cluster.</span></span>
[<span data-ttu-id="b4da5-2879">HDInsight Streaming-tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4da5-2879">HDInsight Streaming Activity</span></span>](#hdinsight-streaming-activity) | <span data-ttu-id="b4da5-2880">hello Streamelési tevékenységben HDInsight a Data Factory-folyamat saját Hadoop Streamelési programok vagy igény szerinti Windows/Linux-alapú HDInsight-fürt hajt végre.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2880">hello HDInsight Streaming Activity in a Data Factory pipeline executes Hadoop Streaming programs on your own or on-demand Windows/Linux-based HDInsight cluster.</span></span>
[<span data-ttu-id="b4da5-2881">HDInsight Spark-tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4da5-2881">HDInsight Spark Activity</span></span>](#hdinsight-spark-activity) | <span data-ttu-id="b4da5-2882">a Data Factory-folyamathoz HDInsight Spark tevékenység hello Spark programok saját HDInsight-fürt hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2882">hello HDInsight Spark activity in a Data Factory pipeline executes Spark programs on your own HDInsight cluster.</span></span> 
[<span data-ttu-id="b4da5-2883">Machine Learning kötegelt végrehajtási tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4da5-2883">Machine Learning Batch Execution Activity</span></span>](#machine-learning-batch-execution-activity) | <span data-ttu-id="b4da5-2884">Az Azure Data Factory lehetővé teszi, hogy Ön tooeasily létrehozása folyamatok, amelyek egy közzétett Azure Machine Learning a prediktív elemzés webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2884">Azure Data Factory enables you tooeasily create pipelines that use a published Azure Machine Learning web service for predictive analytics.</span></span> <span data-ttu-id="b4da5-2885">Egy Azure Data Factory-folyamat a kötegelt végrehajtási tevékenység hello használ, a Machine Learning web toomake előrejelzések kötegben hello adatokon hívhat meg.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2885">Using hello Batch Execution Activity in an Azure Data Factory pipeline, you can invoke a Machine Learning web service toomake predictions on hello data in batch.</span></span> 
[<span data-ttu-id="b4da5-2886">Machine Learning Update-erőforrástevékenység</span><span class="sxs-lookup"><span data-stu-id="b4da5-2886">Machine Learning Update Resource Activity</span></span>](#machine-learning-update-resource-activity) | <span data-ttu-id="b4da5-2887">Idővel hello saját prediktív modelljeit hello pontozási kísérletek kell toobe retrained új Machine Learning bemeneti adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2887">Over time, hello predictive models in hello Machine Learning scoring experiments need toobe retrained using new input datasets.</span></span> <span data-ttu-id="b4da5-2888">Miután elkészült, az átképezési, kívánt hello webszolgáltatás pontozási tooupdate hello retrained gépi tanulási modell.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2888">After you are done with retraining, you want tooupdate hello scoring web service with hello retrained Machine Learning model.</span></span> <span data-ttu-id="b4da5-2889">Hello Update-Erőforrástevékenység tooupdate hello webszolgáltatás újonnan betanítása hello modell használható.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2889">You can use hello Update Resource Activity tooupdate hello web service with hello newly trained model.</span></span>
[<span data-ttu-id="b4da5-2890">Tárolt eljárási tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4da5-2890">Stored Procedure Activity</span></span>](#stored-procedure-activity) | <span data-ttu-id="b4da5-2891">Hello tárolt eljárási tevékenység használható egy adat-előállító adatcsatorna tooinvoke valamelyik hello adattárolókhoz a következő tárolt eljárást: Azure SQL Database, Azure SQL Data Warehouse szolgáltatásban a vállalat SQL Server-adatbázis vagy egy Azure virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2891">You can use hello Stored Procedure activity in a Data Factory pipeline tooinvoke a stored procedure in one of hello following data stores: Azure SQL Database, Azure SQL Data Warehouse, SQL Server Database in your enterprise or an Azure VM.</span></span> 
[<span data-ttu-id="b4da5-2892">Data Lake Analytics U-SQL-tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4da5-2892">Data Lake Analytics U-SQL activity</span></span>](#data-lake-analytics-u-sql-activity) | <span data-ttu-id="b4da5-2893">Data Lake Analytics U-SQL-tevékenység egy Azure Data Lake Analytics-fürt U-SQL parancsfájlt futtat.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2893">Data Lake Analytics U-SQL Activity runs a U-SQL script on an Azure Data Lake Analytics cluster.</span></span>  
[<span data-ttu-id="b4da5-2894">.NET egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4da5-2894">.NET custom activity</span></span>](#net-custom-activity) | <span data-ttu-id="b4da5-2895">Ha tootransform adatokat oly módon, amely nem támogatja a Data Factory van szüksége, hozzon létre egy egyéni tevékenység saját adatokat feldolgozó logika, és hello tevékenység hello folyamat használja.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2895">If you need tootransform data in a way that is not supported by Data Factory, you can create a custom activity with your own data processing logic and use hello activity in hello pipeline.</span></span> <span data-ttu-id="b4da5-2896">Hello egyéni .NET tevékenység toorun az Azure Batch szolgáltatás vagy az Azure HDInsight-fürtöt is konfigurálhat.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2896">You can configure hello custom .NET activity toorun using either an Azure Batch service or an Azure HDInsight cluster.</span></span> 

     
## <a name="hdinsight-hive-activity"></a><span data-ttu-id="b4da5-2897">HDInsight Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4da5-2897">HDInsight Hive Activity</span></span>
<span data-ttu-id="b4da5-2898">A következő tulajdonságok a tevékenység JSON Hive definícióban hello adhatja meg.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2898">You can specify hello following properties in a Hive Activity JSON definition.</span></span> <span data-ttu-id="b4da5-2899">hello type tulajdonság hello tevékenységnek kell lennie: **HDInsightHive**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2899">hello type property for hello activity must be: **HDInsightHive**.</span></span> <span data-ttu-id="b4da5-2900">Létre kell hoznia egy HDInsight kapcsolódó szolgáltatás először és hello értékeként adja meg a hello nevét az **linkedServiceName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2900">You must create a HDInsight linked service first and specify hello name of it as a value for hello **linkedServiceName** property.</span></span> <span data-ttu-id="b4da5-2901">hello következő tulajdonságok támogatottak hello **typeProperties** szakasz hello típusú tevékenység tooHDInsightHive beállításakor:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2901">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooHDInsightHive:</span></span>

| <span data-ttu-id="b4da5-2902">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2902">Property</span></span> | <span data-ttu-id="b4da5-2903">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2903">Description</span></span> | <span data-ttu-id="b4da5-2904">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2904">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-2905">Parancsfájl</span><span class="sxs-lookup"><span data-stu-id="b4da5-2905">script</span></span> |<span data-ttu-id="b4da5-2906">Adja meg a hello Hive parancsfájl beágyazott</span><span class="sxs-lookup"><span data-stu-id="b4da5-2906">Specify hello Hive script inline</span></span> |<span data-ttu-id="b4da5-2907">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2907">No</span></span> |
| <span data-ttu-id="b4da5-2908">parancsfájl elérési útja</span><span class="sxs-lookup"><span data-stu-id="b4da5-2908">script path</span></span> |<span data-ttu-id="b4da5-2909">Tároló hello Hive parancsprogram egy Azure blob Storage tárolóban, és adja meg a hello elérési toohello fájlt.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2909">Store hello Hive script in an Azure blob storage and provide hello path toohello file.</span></span> <span data-ttu-id="b4da5-2910">Használja a "script" vagy "scriptPath" tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2910">Use 'script' or 'scriptPath' property.</span></span> <span data-ttu-id="b4da5-2911">Mindkettő nem használható együtt.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2911">Both cannot be used together.</span></span> <span data-ttu-id="b4da5-2912">hello fájlnév a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2912">hello file name is case-sensitive.</span></span> |<span data-ttu-id="b4da5-2913">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2913">No</span></span> |
| <span data-ttu-id="b4da5-2914">határozza meg</span><span class="sxs-lookup"><span data-stu-id="b4da5-2914">defines</span></span> |<span data-ttu-id="b4da5-2915">Kulcs/érték párok paraméterek meghatározni belül hello Hive parancsfájl segítségével történő "hiveconf" hivatkozik</span><span class="sxs-lookup"><span data-stu-id="b4da5-2915">Specify parameters as key/value pairs for referencing within hello Hive script using 'hiveconf'</span></span> |<span data-ttu-id="b4da5-2916">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2916">No</span></span> |

<span data-ttu-id="b4da5-2917">Adott toohello Hive tevékenység rendszer így ezeket a típus tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2917">These type properties are specific toohello Hive Activity.</span></span> <span data-ttu-id="b4da5-2918">Az összes tevékenység egyéb tulajdonságok (kívül hello typeProperties szakaszban) támogatottak.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2918">Other properties (outside hello typeProperties section) are supported for all activities.</span></span>   

### <a name="json-example"></a><span data-ttu-id="b4da5-2919">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2919">JSON example</span></span>
<span data-ttu-id="b4da5-2920">a következő JSON hello egy HDInsight Hive tevékenységet egy folyamaton belül határozza meg.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2920">hello following JSON defines a HDInsight Hive activity in a pipeline.</span></span>  

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

<span data-ttu-id="b4da5-2921">További információkért lásd: [Hive tevékenység](data-factory-hive-activity.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2921">For more information, see [Hive Activity](data-factory-hive-activity.md) article.</span></span> 

## <a name="hdinsight-pig-activity"></a><span data-ttu-id="b4da5-2922">HDInsight Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4da5-2922">HDInsight Pig Activity</span></span>
<span data-ttu-id="b4da5-2923">A következő tulajdonságok a Pig tevékenység JSON-definícióban hello adhatja meg.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2923">You can specify hello following properties in a Pig Activity JSON definition.</span></span> <span data-ttu-id="b4da5-2924">hello type tulajdonság hello tevékenységnek kell lennie: **HDInsightPig**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2924">hello type property for hello activity must be: **HDInsightPig**.</span></span> <span data-ttu-id="b4da5-2925">Létre kell hoznia egy HDInsight kapcsolódó szolgáltatás először és hello értékeként adja meg a hello nevét az **linkedServiceName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2925">You must create a HDInsight linked service first and specify hello name of it as a value for hello **linkedServiceName** property.</span></span> <span data-ttu-id="b4da5-2926">hello következő tulajdonságok támogatottak hello **typeProperties** szakasz hello típusú tevékenység tooHDInsightPig beállításakor:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2926">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooHDInsightPig:</span></span> 

| <span data-ttu-id="b4da5-2927">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2927">Property</span></span> | <span data-ttu-id="b4da5-2928">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2928">Description</span></span> | <span data-ttu-id="b4da5-2929">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2929">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-2930">Parancsfájl</span><span class="sxs-lookup"><span data-stu-id="b4da5-2930">script</span></span> |<span data-ttu-id="b4da5-2931">Adja meg a Pig-parancsprogram beágyazott hello</span><span class="sxs-lookup"><span data-stu-id="b4da5-2931">Specify hello Pig script inline</span></span> |<span data-ttu-id="b4da5-2932">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2932">No</span></span> |
| <span data-ttu-id="b4da5-2933">parancsfájl elérési útja</span><span class="sxs-lookup"><span data-stu-id="b4da5-2933">script path</span></span> |<span data-ttu-id="b4da5-2934">Hello Pig-parancsprogram egy Azure blob Storage tárolóban tárolja, és adja meg a hello elérési toohello fájlt.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2934">Store hello Pig script in an Azure blob storage and provide hello path toohello file.</span></span> <span data-ttu-id="b4da5-2935">Használja a "script" vagy "scriptPath" tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2935">Use 'script' or 'scriptPath' property.</span></span> <span data-ttu-id="b4da5-2936">Mindkettő nem használható együtt.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2936">Both cannot be used together.</span></span> <span data-ttu-id="b4da5-2937">hello fájlnév a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2937">hello file name is case-sensitive.</span></span> |<span data-ttu-id="b4da5-2938">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2938">No</span></span> |
| <span data-ttu-id="b4da5-2939">határozza meg</span><span class="sxs-lookup"><span data-stu-id="b4da5-2939">defines</span></span> |<span data-ttu-id="b4da5-2940">Kulcs/érték párok paraméterek meghatározni hivatkozó belül hello Pig-parancsprogram</span><span class="sxs-lookup"><span data-stu-id="b4da5-2940">Specify parameters as key/value pairs for referencing within hello Pig script</span></span> |<span data-ttu-id="b4da5-2941">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2941">No</span></span> |

<span data-ttu-id="b4da5-2942">Adott toohello Pig tevékenység rendszer így ezeket a típus tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2942">These type properties are specific toohello Pig Activity.</span></span> <span data-ttu-id="b4da5-2943">Az összes tevékenység egyéb tulajdonságok (kívül hello typeProperties szakaszban) támogatottak.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2943">Other properties (outside hello typeProperties section) are supported for all activities.</span></span>   

### <a name="json-example"></a><span data-ttu-id="b4da5-2944">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2944">JSON example</span></span>

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

<span data-ttu-id="b4da5-2945">További információkért lásd: [Pig tevékenység](#data-factory-pig-activity.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2945">For more information, see [Pig Activity](#data-factory-pig-activity.md) article.</span></span> 

## <a name="hdinsight-mapreduce-activity"></a><span data-ttu-id="b4da5-2946">HDInsight MapReduce-tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4da5-2946">HDInsight MapReduce Activity</span></span>
<span data-ttu-id="b4da5-2947">A következő tulajdonságok MapReduce tevékenység JSON-definícióban hello adhatja meg.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2947">You can specify hello following properties in a MapReduce Activity JSON definition.</span></span> <span data-ttu-id="b4da5-2948">hello type tulajdonság hello tevékenységnek kell lennie: **HDInsightMapReduce**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2948">hello type property for hello activity must be: **HDInsightMapReduce**.</span></span> <span data-ttu-id="b4da5-2949">Létre kell hoznia egy HDInsight kapcsolódó szolgáltatás először és hello értékeként adja meg a hello nevét az **linkedServiceName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2949">You must create a HDInsight linked service first and specify hello name of it as a value for hello **linkedServiceName** property.</span></span> <span data-ttu-id="b4da5-2950">hello következő tulajdonságok támogatottak hello **typeProperties** szakasz hello típusú tevékenység tooHDInsightMapReduce beállításakor:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2950">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooHDInsightMapReduce:</span></span> 

| <span data-ttu-id="b4da5-2951">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2951">Property</span></span> | <span data-ttu-id="b4da5-2952">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2952">Description</span></span> | <span data-ttu-id="b4da5-2953">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-2953">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-2954">jarLinkedService</span><span class="sxs-lookup"><span data-stu-id="b4da5-2954">jarLinkedService</span></span> | <span data-ttu-id="b4da5-2955">Hello nevét csatolt Azure Storage hello JAR-fájlt tartalmazó hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2955">Name of hello linked service for hello Azure Storage that contains hello JAR file.</span></span> | <span data-ttu-id="b4da5-2956">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2956">Yes</span></span> |
| <span data-ttu-id="b4da5-2957">jarfilepath tulajdonságot</span><span class="sxs-lookup"><span data-stu-id="b4da5-2957">jarFilePath</span></span> | <span data-ttu-id="b4da5-2958">Elérési út toohello JAR-fájlra a hello Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2958">Path toohello JAR file in hello Azure Storage.</span></span> | <span data-ttu-id="b4da5-2959">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2959">Yes</span></span> | 
| <span data-ttu-id="b4da5-2960">Osztálynév</span><span class="sxs-lookup"><span data-stu-id="b4da5-2960">className</span></span> | <span data-ttu-id="b4da5-2961">Hello fő osztály hello JAR-fájlra a neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2961">Name of hello main class in hello JAR file.</span></span> | <span data-ttu-id="b4da5-2962">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-2962">Yes</span></span> | 
| <span data-ttu-id="b4da5-2963">Argumentumok</span><span class="sxs-lookup"><span data-stu-id="b4da5-2963">arguments</span></span> | <span data-ttu-id="b4da5-2964">Vesszővel elválasztott argumentumokat hello MapReduce program listáját.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2964">A list of comma-separated arguments for hello MapReduce program.</span></span> <span data-ttu-id="b4da5-2965">Futásidőben, néhány további argumentumok látja (például: mapreduce.job.tags) a hello MapReduce keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2965">At runtime, you see a few extra arguments (for example: mapreduce.job.tags) from hello MapReduce framework.</span></span> <span data-ttu-id="b4da5-2966">toodifferentiate az argumentumok hello MapReduce argumentumokkal, érdemes lehetőség és value elemeit is argumentumként látható módon a következő példa hello (- s,--bemeneti,--kimeneti stb., amelyet közvetlenül az értékeik lehetőség áll)</span><span class="sxs-lookup"><span data-stu-id="b4da5-2966">toodifferentiate your arguments with hello MapReduce arguments, consider using both option and value as arguments as shown in hello following example (-s, --input, --output etc., are options immediately followed by their values)</span></span> | <span data-ttu-id="b4da5-2967">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-2967">No</span></span> | 

### <a name="json-example"></a><span data-ttu-id="b4da5-2968">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-2968">JSON example</span></span>

```json
{
    "name": "MahoutMapReduceSamplePipeline",
    "properties": {
        "description": "Sample Pipeline tooRun a Mahout Custom Map Reduce Jar. This job calculates an Item Similarity Matrix toodetermine hello similarity between two items",
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
                "description": "Custom Map Reduce toogenerate Mahout result",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-01-03T00:00:00",
        "end": "2017-01-04T00:00:00"
    }
}
```

<span data-ttu-id="b4da5-2969">További információkért lásd: [MapReduce művelethez](data-factory-map-reduce.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2969">For more information, see [MapReduce Activity](data-factory-map-reduce.md) article.</span></span> 

## <a name="hdinsight-streaming-activity"></a><span data-ttu-id="b4da5-2970">HDInsight Streaming-tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4da5-2970">HDInsight Streaming Activity</span></span>
<span data-ttu-id="b4da5-2971">A következő tulajdonságok a Hadoop Streamelési tevékenység JSON-definícióban hello adhatja meg.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2971">You can specify hello following properties in a Hadoop Streaming Activity JSON definition.</span></span> <span data-ttu-id="b4da5-2972">hello type tulajdonság hello tevékenységnek kell lennie: **HDInsightStreaming**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2972">hello type property for hello activity must be: **HDInsightStreaming**.</span></span> <span data-ttu-id="b4da5-2973">Létre kell hoznia egy HDInsight kapcsolódó szolgáltatás először és hello értékeként adja meg a hello nevét az **linkedServiceName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2973">You must create a HDInsight linked service first and specify hello name of it as a value for hello **linkedServiceName** property.</span></span> <span data-ttu-id="b4da5-2974">hello következő tulajdonságok támogatottak hello **typeProperties** szakasz hello típusú tevékenység tooHDInsightStreaming beállításakor:</span><span class="sxs-lookup"><span data-stu-id="b4da5-2974">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooHDInsightStreaming:</span></span> 

| <span data-ttu-id="b4da5-2975">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-2975">Property</span></span> | <span data-ttu-id="b4da5-2976">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-2976">Description</span></span> | 
| --- | --- |
| <span data-ttu-id="b4da5-2977">eseményleképező</span><span class="sxs-lookup"><span data-stu-id="b4da5-2977">mapper</span></span> | <span data-ttu-id="b4da5-2978">Hello leképező végrehajtható fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2978">Name of hello mapper executable.</span></span> <span data-ttu-id="b4da5-2979">Hello példában cat.exe: hello leképező végrehajtható.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2979">In hello example, cat.exe is hello mapper executable.</span></span>| 
| <span data-ttu-id="b4da5-2980">Nyomáscsökkentő</span><span class="sxs-lookup"><span data-stu-id="b4da5-2980">reducer</span></span> | <span data-ttu-id="b4da5-2981">Hello nyomáscsökkentő végrehajtható fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2981">Name of hello reducer executable.</span></span> <span data-ttu-id="b4da5-2982">Hello példában wc.exe: hello nyomáscsökkentő végrehajtható.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2982">In hello example, wc.exe is hello reducer executable.</span></span> | 
| <span data-ttu-id="b4da5-2983">Bemeneti</span><span class="sxs-lookup"><span data-stu-id="b4da5-2983">input</span></span> | <span data-ttu-id="b4da5-2984">Hello leképező bemeneti (beleértve a hely) fájl.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2984">Input file (including location) for hello mapper.</span></span> <span data-ttu-id="b4da5-2985">Hello példa: "wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample hello blob tároló, például/data/Gutenberg hello mappában, pedig davinci.txt hello blob.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2985">In hello example: "wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample is hello blob container, example/data/Gutenberg is hello folder, and davinci.txt is hello blob.</span></span> |
| <span data-ttu-id="b4da5-2986">Kimeneti</span><span class="sxs-lookup"><span data-stu-id="b4da5-2986">output</span></span> | <span data-ttu-id="b4da5-2987">Kimeneti fájl (beleértve a hely) hello nyomáscsökkentő.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2987">Output file (including location) for hello reducer.</span></span> <span data-ttu-id="b4da5-2988">hello Hadoop adatfolyam-feladat eredményének hello toohello helyére a tulajdonság írása.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2988">hello output of hello Hadoop Streaming job is written toohello location specified for this property.</span></span> |
| <span data-ttu-id="b4da5-2989">filePaths</span><span class="sxs-lookup"><span data-stu-id="b4da5-2989">filePaths</span></span> | <span data-ttu-id="b4da5-2990">Elérési utak hello hozzárendelést és nyomáscsökkentő végrehajtható fájlok számára.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2990">Paths for hello mapper and reducer executables.</span></span> <span data-ttu-id="b4da5-2991">Hello példa: "adfsample/example/apps/wc.exe" adfsample egy hello blob tároló, például/alkalmazások hello mappa, pedig wc.exe hello végrehajtható.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2991">In hello example: "adfsample/example/apps/wc.exe", adfsample is hello blob container, example/apps is hello folder, and wc.exe is hello executable.</span></span> | 
| <span data-ttu-id="b4da5-2992">fileLinkedService</span><span class="sxs-lookup"><span data-stu-id="b4da5-2992">fileLinkedService</span></span> | <span data-ttu-id="b4da5-2993">Az Azure tárolás társított szolgáltatása, amely az Azure storage hello filePaths szakaszában megadott hello fájlokat tartalmazó hello jelöli.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2993">Azure Storage linked service that represents hello Azure storage that contains hello files specified in hello filePaths section.</span></span> | 
| <span data-ttu-id="b4da5-2994">Argumentumok</span><span class="sxs-lookup"><span data-stu-id="b4da5-2994">arguments</span></span> | <span data-ttu-id="b4da5-2995">Vesszővel elválasztott argumentumokat hello MapReduce program listáját.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2995">A list of comma-separated arguments for hello MapReduce program.</span></span> <span data-ttu-id="b4da5-2996">Futásidőben, néhány további argumentumok látja (például: mapreduce.job.tags) a hello MapReduce keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2996">At runtime, you see a few extra arguments (for example: mapreduce.job.tags) from hello MapReduce framework.</span></span> <span data-ttu-id="b4da5-2997">toodifferentiate az argumentumok hello MapReduce argumentumokkal, érdemes lehetőség és value elemeit is argumentumként látható módon a következő példa hello (- s,--bemeneti,--kimeneti stb., amelyet közvetlenül az értékeik lehetőség áll)</span><span class="sxs-lookup"><span data-stu-id="b4da5-2997">toodifferentiate your arguments with hello MapReduce arguments, consider using both option and value as arguments as shown in hello following example (-s, --input, --output etc., are options immediately followed by their values)</span></span> | 
| <span data-ttu-id="b4da5-2998">getDebugInfo</span><span class="sxs-lookup"><span data-stu-id="b4da5-2998">getDebugInfo</span></span> | <span data-ttu-id="b4da5-2999">Választható eleme.</span><span class="sxs-lookup"><span data-stu-id="b4da5-2999">An optional element.</span></span> <span data-ttu-id="b4da5-3000">Ha ezt a beállítást tooFailure, hello naplók csak az hiba lesznek letöltve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3000">When it is set tooFailure, hello logs are downloaded only on failure.</span></span> <span data-ttu-id="b4da5-3001">Ha ezt a beállítást tooAll, naplók mindig letöltődnek függetlenül hello végrehajtási állapotát.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3001">When it is set tooAll, logs are always downloaded irrespective of hello execution status.</span></span> | 

> [!NOTE]
> <span data-ttu-id="b4da5-3002">Meg kell adnia egy kimeneti adatkészletet hello a Hadoop Streamelési tevékenységben hello **kimenete** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3002">You must specify an output dataset for hello Hadoop Streaming Activity for hello **outputs** property.</span></span> <span data-ttu-id="b4da5-3003">Ez az adatkészlet csak egy üres adathalmazt, amelyet szükséges toodrive hello csővezeték ütemezés (óránként, naponta, stb.) lehet.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3003">This dataset can be just a dummy dataset that is required toodrive hello pipeline schedule (hourly, daily, etc.).</span></span> <span data-ttu-id="b4da5-3004">Hello tevékenység bemeneti nem veszi, kihagyhatja, ha egy bemeneti adatkészlet hello tevékenység a hello megadó **bemenetek** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3004">If hello activity doesn't take an input, you can skip specifying an input dataset for hello activity for hello **inputs** property.</span></span>  

## <a name="json-example"></a><span data-ttu-id="b4da5-3005">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-3005">JSON example</span></span>

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

<span data-ttu-id="b4da5-3006">További információkért lásd: [Hadoop Streamelési tevékenységben](data-factory-hadoop-streaming-activity.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3006">For more information, see [Hadoop Streaming Activity](data-factory-hadoop-streaming-activity.md) article.</span></span> 

## <a name="hdinsight-spark-activity"></a><span data-ttu-id="b4da5-3007">HDInsight Spark-tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4da5-3007">HDInsight Spark Activity</span></span>
<span data-ttu-id="b4da5-3008">A következő tulajdonságok Spark tevékenység JSON-definícióban hello adhatja meg.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3008">You can specify hello following properties in a Spark Activity JSON definition.</span></span> <span data-ttu-id="b4da5-3009">hello type tulajdonság hello tevékenységnek kell lennie: **HDInsightSpark**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3009">hello type property for hello activity must be: **HDInsightSpark**.</span></span> <span data-ttu-id="b4da5-3010">Létre kell hoznia egy HDInsight kapcsolódó szolgáltatás először és hello értékeként adja meg a hello nevét az **linkedServiceName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3010">You must create a HDInsight linked service first and specify hello name of it as a value for hello **linkedServiceName** property.</span></span> <span data-ttu-id="b4da5-3011">hello következő tulajdonságok támogatottak hello **typeProperties** szakasz hello típusú tevékenység tooHDInsightSpark beállításakor:</span><span class="sxs-lookup"><span data-stu-id="b4da5-3011">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooHDInsightSpark:</span></span> 

| <span data-ttu-id="b4da5-3012">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-3012">Property</span></span> | <span data-ttu-id="b4da5-3013">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-3013">Description</span></span> | <span data-ttu-id="b4da5-3014">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-3014">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="b4da5-3015">rootPath</span><span class="sxs-lookup"><span data-stu-id="b4da5-3015">rootPath</span></span> | <span data-ttu-id="b4da5-3016">hello Azure blobtároló és hello Spark fájlt tartalmazó mappát.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3016">hello Azure Blob container and folder that contains hello Spark file.</span></span> <span data-ttu-id="b4da5-3017">hello fájlnév a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3017">hello file name is case-sensitive.</span></span> | <span data-ttu-id="b4da5-3018">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-3018">Yes</span></span> |
| <span data-ttu-id="b4da5-3019">entryFilePath</span><span class="sxs-lookup"><span data-stu-id="b4da5-3019">entryFilePath</span></span> | <span data-ttu-id="b4da5-3020">Relatív elérési út toohello gyökérmappájában hello Spark kódcsomag.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3020">Relative path toohello root folder of hello Spark code/package.</span></span> | <span data-ttu-id="b4da5-3021">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-3021">Yes</span></span> |
| <span data-ttu-id="b4da5-3022">Osztálynév</span><span class="sxs-lookup"><span data-stu-id="b4da5-3022">className</span></span> | <span data-ttu-id="b4da5-3023">Az alkalmazás Java/Spark fő osztály</span><span class="sxs-lookup"><span data-stu-id="b4da5-3023">Application's Java/Spark main class</span></span> | <span data-ttu-id="b4da5-3024">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-3024">No</span></span> | 
| <span data-ttu-id="b4da5-3025">Argumentumok</span><span class="sxs-lookup"><span data-stu-id="b4da5-3025">arguments</span></span> | <span data-ttu-id="b4da5-3026">Parancssori argumentumok toohello Spark program listáját.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3026">A list of command-line arguments toohello Spark program.</span></span> | <span data-ttu-id="b4da5-3027">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-3027">No</span></span> | 
| <span data-ttu-id="b4da5-3028">proxyUser</span><span class="sxs-lookup"><span data-stu-id="b4da5-3028">proxyUser</span></span> | <span data-ttu-id="b4da5-3029">hello felhasználói fiók tooimpersonate tooexecute hello Spark program</span><span class="sxs-lookup"><span data-stu-id="b4da5-3029">hello user account tooimpersonate tooexecute hello Spark program</span></span> | <span data-ttu-id="b4da5-3030">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-3030">No</span></span> | 
| <span data-ttu-id="b4da5-3031">sparkConfig</span><span class="sxs-lookup"><span data-stu-id="b4da5-3031">sparkConfig</span></span> | <span data-ttu-id="b4da5-3032">Spark konfigurációs tulajdonságaiban.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3032">Spark configuration properties.</span></span> | <span data-ttu-id="b4da5-3033">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-3033">No</span></span> | 
| <span data-ttu-id="b4da5-3034">getDebugInfo</span><span class="sxs-lookup"><span data-stu-id="b4da5-3034">getDebugInfo</span></span> | <span data-ttu-id="b4da5-3035">Itt adhatja meg, amikor hello Spark naplófájlok másolt toohello az Azure storage HDInsight-fürt által használt (vagy) leírt módon sparkJobLinkedService.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3035">Specifies when hello Spark log files are copied toohello Azure storage used by HDInsight cluster (or) specified by sparkJobLinkedService.</span></span> <span data-ttu-id="b4da5-3036">Megengedett értékek: None, mindig, vagy sikertelen.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3036">Allowed values: None, Always, or Failure.</span></span> <span data-ttu-id="b4da5-3037">Alapértelmezett érték: nincs.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3037">Default value: None.</span></span> | <span data-ttu-id="b4da5-3038">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-3038">No</span></span> | 
| <span data-ttu-id="b4da5-3039">sparkJobLinkedService</span><span class="sxs-lookup"><span data-stu-id="b4da5-3039">sparkJobLinkedService</span></span> | <span data-ttu-id="b4da5-3040">hello Azure tárolás társított szolgáltatása, amely tárolja a hello Spark feladat fájl, a függőségeket és a naplókat.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3040">hello Azure Storage linked service that holds hello Spark job file, dependencies, and logs.</span></span>  <span data-ttu-id="b4da5-3041">Ha nem ad meg egy értéket ehhez a tulajdonsághoz, HDInsight-fürthöz társított hello tárolót használja a rendszer.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3041">If you do not specify a value for this property, hello storage associated with HDInsight cluster is used.</span></span> | <span data-ttu-id="b4da5-3042">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-3042">No</span></span> |

### <a name="json-example"></a><span data-ttu-id="b4da5-3043">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-3043">JSON example</span></span>

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
<span data-ttu-id="b4da5-3044">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="b4da5-3044">Note hello following points:</span></span> 

- <span data-ttu-id="b4da5-3045">Hello **típus** tulajdonsága túl**HDInsightSpark**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3045">hello **type** property is set too**HDInsightSpark**.</span></span>
- <span data-ttu-id="b4da5-3046">Hello **rootPath** értéke túl**adfspark\\pyFiles** ahol adfspark hello Azure Blob-tároló és pyFiles pedig az adott tároló finom mappát.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3046">hello **rootPath** is set too**adfspark\\pyFiles** where adfspark is hello Azure Blob container and pyFiles is fine folder in that container.</span></span> <span data-ttu-id="b4da5-3047">Ebben a példában a hello Azure Blob Storage egy Spark-fürt hello társított hello.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3047">In this example, hello Azure Blob Storage is hello one that is associated with hello Spark cluster.</span></span> <span data-ttu-id="b4da5-3048">Feltöltheti a hello fájl tooa különböző Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3048">You can upload hello file tooa different Azure Storage.</span></span> <span data-ttu-id="b4da5-3049">Ha így tesz, hozzon létre egy Azure Storage társított szolgáltatás toolink adott tárolási fiók toohello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3049">If you do so, create an Azure Storage linked service toolink that storage account toohello data factory.</span></span> <span data-ttu-id="b4da5-3050">Ezt követően adja a hello társított szolgáltatás neve hello hello értékeként **sparkJobLinkedService** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3050">Then, specify hello name of hello linked service as a value for hello **sparkJobLinkedService** property.</span></span> <span data-ttu-id="b4da5-3051">Lásd: [Spark-tevékenység tulajdonságai](#spark-activity-properties) Ez a tulajdonság és az egyéb hello Spark tevékenység által támogatott tulajdonságokról.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3051">See [Spark Activity properties](#spark-activity-properties) for details about this property and other properties supported by hello Spark Activity.</span></span>
- <span data-ttu-id="b4da5-3052">Hello **entryFilePath** értéke toohello **test.py**, vagyis hello python-fájl.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3052">hello **entryFilePath** is set toohello **test.py**, which is hello python file.</span></span> 
- <span data-ttu-id="b4da5-3053">Hello **getDebugInfo** tulajdonsága túl**mindig**, ami azt jelenti, hogy hello naplófájlok helyét a rendszer mindig generált (sikeres vagy sikertelen).</span><span class="sxs-lookup"><span data-stu-id="b4da5-3053">hello **getDebugInfo** property is set too**Always**, which means hello log files are always generated (success or failure).</span></span>  

    > [!IMPORTANT]
    > <span data-ttu-id="b4da5-3054">Azt javasoljuk, hogy nincs megadva a tulajdonság tooAlways éles környezetben kivéve, ha a probléma hibaelhárítást.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3054">We recommend that you do not set this property tooAlways in a production environment unless you are troubleshooting an issue.</span></span> 
- <span data-ttu-id="b4da5-3055">Hello **kimenete** szakasz tartalmaz egy kimeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3055">hello **outputs** section has one output dataset.</span></span> <span data-ttu-id="b4da5-3056">Meg kell adnia egy kimeneti adatkészletet, még akkor is, ha hello spark program nem ad kimenetet.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3056">You must specify an output dataset even if hello spark program does not produce any output.</span></span> <span data-ttu-id="b4da5-3057">hello kimeneti adatkészlet meghajtók hello ütemezés hello adatcsatorna (óránként, naponta, stb.).</span><span class="sxs-lookup"><span data-stu-id="b4da5-3057">hello output dataset drives hello schedule for hello pipeline (hourly, daily, etc.).</span></span>

<span data-ttu-id="b4da5-3058">Hello tevékenységgel kapcsolatos további információkért lásd: [Spark tevékenység](data-factory-spark.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3058">For more information about hello activity, see [Spark Activity](data-factory-spark.md) article.</span></span>  

## <a name="machine-learning-batch-execution-activity"></a><span data-ttu-id="b4da5-3059">Machine Learning kötegelt végrehajtási tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4da5-3059">Machine Learning Batch Execution Activity</span></span>
<span data-ttu-id="b4da5-3060">A következő tulajdonságok az Azure ML kötegelt végrehajtási tevékenység JSON-definícióban hello adhatja meg.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3060">You can specify hello following properties in a Azure ML Batch Execution Activity JSON definition.</span></span> <span data-ttu-id="b4da5-3061">hello type tulajdonság hello tevékenységnek kell lennie: **AzureMLBatchExecution**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3061">hello type property for hello activity must be: **AzureMLBatchExecution**.</span></span> <span data-ttu-id="b4da5-3062">Létre kell hoznia az Azure Machine Learning-szolgáltatásnak először és adja meg azt hello nevét hello értékeként **linkedServiceName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3062">You must create a Azure Machine Learning linked service first and specify hello name of it as a value for hello **linkedServiceName** property.</span></span> <span data-ttu-id="b4da5-3063">hello következő tulajdonságok támogatottak hello **typeProperties** szakasz hello típusú tevékenység tooAzureMLBatchExecution beállításakor:</span><span class="sxs-lookup"><span data-stu-id="b4da5-3063">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooAzureMLBatchExecution:</span></span>

<span data-ttu-id="b4da5-3064">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-3064">Property</span></span> | <span data-ttu-id="b4da5-3065">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-3065">Description</span></span> | <span data-ttu-id="b4da5-3066">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-3066">Required</span></span> 
-------- | ----------- | --------
<span data-ttu-id="b4da5-3067">Típus</span><span class="sxs-lookup"><span data-stu-id="b4da5-3067">webServiceInput</span></span> | <span data-ttu-id="b4da5-3068">hello dataset toobe át az Azure ML web service hello bemenetként.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3068">hello dataset toobe passed as an input for hello Azure ML web service.</span></span> <span data-ttu-id="b4da5-3069">Ez az adatkészlet hello bemenetek hello tevékenység is kell szerepelnie.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3069">This dataset must also be included in hello inputs for hello activity.</span></span> |<span data-ttu-id="b4da5-3070">Használja a típus vagy webServiceInputs.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3070">Use either webServiceInput or webServiceInputs.</span></span> | 
<span data-ttu-id="b4da5-3071">webServiceInputs</span><span class="sxs-lookup"><span data-stu-id="b4da5-3071">webServiceInputs</span></span> | <span data-ttu-id="b4da5-3072">Adja meg az Azure ML web service hello bemeneteként átadott adatkészletek toobe.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3072">Specify datasets toobe passed as inputs for hello Azure ML web service.</span></span> <span data-ttu-id="b4da5-3073">Ha hello webszolgáltatás több bemeneti adatokat fogad, tulajdonsággal hello webServiceInputs hello típus tulajdonság használata helyett.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3073">If hello web service takes multiple inputs, use hello webServiceInputs property instead of using hello webServiceInput property.</span></span> <span data-ttu-id="b4da5-3074">Hello által hivatkozott adatkészletek **webServiceInputs** is szerepelnie kell hello tevékenység **bemenetek**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3074">Datasets that are referenced by hello **webServiceInputs** must also be included in hello Activity **inputs**.</span></span> | <span data-ttu-id="b4da5-3075">Használja a típus vagy webServiceInputs.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3075">Use either webServiceInput or webServiceInputs.</span></span> | 
<span data-ttu-id="b4da5-3076">webServiceOutputs</span><span class="sxs-lookup"><span data-stu-id="b4da5-3076">webServiceOutputs</span></span> | <span data-ttu-id="b4da5-3077">hello adathalmaz kimeneti hello Azure ML web service rendelt.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3077">hello datasets that are assigned as outputs for hello Azure ML web service.</span></span> <span data-ttu-id="b4da5-3078">hello webszolgáltatás kimeneti adatokat adja vissza ehhez az adatkészlethez.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3078">hello web service returns output data in this dataset.</span></span> | <span data-ttu-id="b4da5-3079">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-3079">Yes</span></span> | 
<span data-ttu-id="b4da5-3080">globalParameters</span><span class="sxs-lookup"><span data-stu-id="b4da5-3080">globalParameters</span></span> | <span data-ttu-id="b4da5-3081">Ez a szakasz hello webszolgáltatás-paramétereket értékeket megadni.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3081">Specify values for hello web service parameters in this section.</span></span> | <span data-ttu-id="b4da5-3082">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-3082">No</span></span> | 

### <a name="json-example"></a><span data-ttu-id="b4da5-3083">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-3083">JSON example</span></span>
<span data-ttu-id="b4da5-3084">Ebben a példában hello tevékenység rendelkezik hello dataset **MLSqlInput** bemenetként és **MLSqlOutput** hello output típusúként.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3084">In this example, hello activity has hello dataset **MLSqlInput** as input and **MLSqlOutput** as hello output.</span></span> <span data-ttu-id="b4da5-3085">Hello **MLSqlInput** átadása egy bemeneti toohello webszolgáltatásként által hello segítségével **típus** JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3085">hello **MLSqlInput** is passed as an input toohello web service by using hello **webServiceInput** JSON property.</span></span> <span data-ttu-id="b4da5-3086">Hello **MLSqlOutput** átadása egy kimeneti toohello webszolgáltatás által hello segítségével **webServiceOutputs** JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3086">hello **MLSqlOutput** is passed as an output toohello Web service by using hello **webServiceOutputs** JSON property.</span></span> 

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

<span data-ttu-id="b4da5-3087">Hello JSON példában hello telepítette az Azure Machine Learning Web szolgáltatás használja az olvasót, és egy író modul tooread/adatok írása az / tooan Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3087">In hello JSON example, hello deployed Azure Machine Learning Web service uses a reader and a writer module tooread/write data from/tooan Azure SQL Database.</span></span> <span data-ttu-id="b4da5-3088">Ez a webszolgáltatás mutatja meg a következő négy paraméterek hello: adatbázis-kiszolgáló neve, a adatbázis neve, a kiszolgáló felhasználói fiók nevét és a kiszolgáló felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3088">This Web service exposes hello following four parameters:  Database server name, Database name, Server user account name, and Server user account password.</span></span>

> [!NOTE]
> <span data-ttu-id="b4da5-3089">Csak be- és kimenetekkel hello AzureMLBatchExecution tevékenység paraméterek toohello webszolgáltatás argumentumként átadhatók.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3089">Only inputs and outputs of hello AzureMLBatchExecution activity can be passed as parameters toohello Web service.</span></span> <span data-ttu-id="b4da5-3090">A fenti JSON részlet hello, például a MLSqlInput egy bemeneti toohello AzureMLBatchExecution tevékenység, mint egy bemeneti toohello webszolgáltatás átadott típus paraméteren keresztül.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3090">For example, in hello above JSON snippet, MLSqlInput is an input toohello AzureMLBatchExecution activity, which is passed as an input toohello Web service via webServiceInput parameter.</span></span>

## <a name="machine-learning-update-resource-activity"></a><span data-ttu-id="b4da5-3091">Machine Learning Update-erőforrástevékenység</span><span class="sxs-lookup"><span data-stu-id="b4da5-3091">Machine Learning Update Resource Activity</span></span>
<span data-ttu-id="b4da5-3092">A következő tulajdonságok az Azure ML Update erőforrás tevékenység JSON-definícióban hello adhatja meg.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3092">You can specify hello following properties in a Azure ML Update Resource Activity JSON definition.</span></span> <span data-ttu-id="b4da5-3093">hello type tulajdonság hello tevékenységnek kell lennie: **AzureMLUpdateResource**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3093">hello type property for hello activity must be: **AzureMLUpdateResource**.</span></span> <span data-ttu-id="b4da5-3094">Létre kell hoznia az Azure Machine Learning-szolgáltatásnak először és adja meg azt hello nevét hello értékeként **linkedServiceName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3094">You must create a Azure Machine Learning linked service first and specify hello name of it as a value for hello **linkedServiceName** property.</span></span> <span data-ttu-id="b4da5-3095">hello következő tulajdonságok támogatottak hello **typeProperties** szakasz hello típusú tevékenység tooAzureMLUpdateResource beállításakor:</span><span class="sxs-lookup"><span data-stu-id="b4da5-3095">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooAzureMLUpdateResource:</span></span>

<span data-ttu-id="b4da5-3096">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-3096">Property</span></span> | <span data-ttu-id="b4da5-3097">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-3097">Description</span></span> | <span data-ttu-id="b4da5-3098">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-3098">Required</span></span> 
-------- | ----------- | --------
<span data-ttu-id="b4da5-3099">trainedModelName</span><span class="sxs-lookup"><span data-stu-id="b4da5-3099">trainedModelName</span></span> | <span data-ttu-id="b4da5-3100">Hello neve retrained modell.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3100">Name of hello retrained model.</span></span> | <span data-ttu-id="b4da5-3101">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-3101">Yes</span></span> |  
<span data-ttu-id="b4da5-3102">trainedModelDatasetName</span><span class="sxs-lookup"><span data-stu-id="b4da5-3102">trainedModelDatasetName</span></span> | <span data-ttu-id="b4da5-3103">Adatkészlet mutat toohello iLearner-fájlt hello átképezési művelet által visszaadott.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3103">Dataset pointing toohello iLearner file returned by hello retraining operation.</span></span> | <span data-ttu-id="b4da5-3104">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-3104">Yes</span></span> | 

### <a name="json-example"></a><span data-ttu-id="b4da5-3105">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-3105">JSON example</span></span>
<span data-ttu-id="b4da5-3106">hello folyamat két tevékenység rendelkezik: **AzureMLBatchExecution** és **AzureMLUpdateResource**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3106">hello pipeline has two activities: **AzureMLBatchExecution** and **AzureMLUpdateResource**.</span></span> <span data-ttu-id="b4da5-3107">hello Azure ML kötegelt végrehajtási tevékenység hello betanítási adatok bemenetként vesz igénybe, és egy kimenetként iLearner-fájlt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3107">hello Azure ML Batch Execution activity takes hello training data as input and produces an iLearner file as an output.</span></span> <span data-ttu-id="b4da5-3108">hello tevékenység hello képzési webszolgáltatás (a tanítási kísérletet webszolgáltatásként kitett) hív hello bevitellel betanítási adatok, és hello ilearner-fájlt kapott hello webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3108">hello activity invokes hello training web service (training experiment exposed as a web service) with hello input training data and receives hello ilearner file from hello webservice.</span></span> <span data-ttu-id="b4da5-3109">hello placeholderBlob csak hello Azure Data Factory szolgáltatás toorun hello-folyamat által igényelt üres kimeneti adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3109">hello placeholderBlob is just a dummy output dataset that is required by hello Azure Data Factory service toorun hello pipeline.</span></span>


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

## <a name="data-lake-analytics-u-sql-activity"></a><span data-ttu-id="b4da5-3110">Data Lake Analytics U-SQL-tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4da5-3110">Data Lake Analytics U-SQL Activity</span></span>
<span data-ttu-id="b4da5-3111">A következő tulajdonságok a U-SQL tevékenység JSON-definícióban hello adhatja meg.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3111">You can specify hello following properties in a U-SQL Activity JSON definition.</span></span> <span data-ttu-id="b4da5-3112">hello type tulajdonság hello tevékenységnek kell lennie: **DataLakeAnalyticsU-SQL**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3112">hello type property for hello activity must be: **DataLakeAnalyticsU-SQL**.</span></span> <span data-ttu-id="b4da5-3113">Létre kell hoznia egy Azure Data Lake Analytics kapcsolódó szolgáltatás és adja meg azt hello nevét hello értékeként **linkedServiceName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3113">You must create an Azure Data Lake Analytics linked service and specify hello name of it as a value for hello **linkedServiceName** property.</span></span> <span data-ttu-id="b4da5-3114">hello következő tulajdonságok támogatottak hello **typeProperties** szakasz hello típusú tevékenység tooDataLakeAnalyticsU-SQL beállításakor:</span><span class="sxs-lookup"><span data-stu-id="b4da5-3114">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooDataLakeAnalyticsU-SQL:</span></span> 

| <span data-ttu-id="b4da5-3115">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-3115">Property</span></span> | <span data-ttu-id="b4da5-3116">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-3116">Description</span></span> | <span data-ttu-id="b4da5-3117">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-3117">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="b4da5-3118">scriptPath</span><span class="sxs-lookup"><span data-stu-id="b4da5-3118">scriptPath</span></span> |<span data-ttu-id="b4da5-3119">Elérési út toofolder hello U-SQL parancsfájlt tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3119">Path toofolder that contains hello U-SQL script.</span></span> <span data-ttu-id="b4da5-3120">Hello fájl neve nem kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3120">Name of hello file is case-sensitive.</span></span> |<span data-ttu-id="b4da5-3121">Nem (Ha a parancsfájl használata)</span><span class="sxs-lookup"><span data-stu-id="b4da5-3121">No (if you use script)</span></span> |
| <span data-ttu-id="b4da5-3122">scriptLinkedService</span><span class="sxs-lookup"><span data-stu-id="b4da5-3122">scriptLinkedService</span></span> |<span data-ttu-id="b4da5-3123">Kapcsolódó szolgáltatás, amely a hello parancsfájl toohello adat-előállító tartalmazó hello tároló</span><span class="sxs-lookup"><span data-stu-id="b4da5-3123">Linked service that links hello storage that contains hello script toohello data factory</span></span> |<span data-ttu-id="b4da5-3124">Nem (Ha a parancsfájl használata)</span><span class="sxs-lookup"><span data-stu-id="b4da5-3124">No (if you use script)</span></span> |
| <span data-ttu-id="b4da5-3125">Parancsfájl</span><span class="sxs-lookup"><span data-stu-id="b4da5-3125">script</span></span> |<span data-ttu-id="b4da5-3126">Adja meg a beágyazott parancsfájlja scriptPath és a scriptLinkedService megadása helyett.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3126">Specify inline script instead of specifying scriptPath and scriptLinkedService.</span></span> <span data-ttu-id="b4da5-3127">Például: "script": "Adatbázis létrehozása test".</span><span class="sxs-lookup"><span data-stu-id="b4da5-3127">For example: "script": "CREATE DATABASE test".</span></span> |<span data-ttu-id="b4da5-3128">Nem (Ha a ScriptPath tulajdonságot is és a scriptLinkedService használ)</span><span class="sxs-lookup"><span data-stu-id="b4da5-3128">No (if you use scriptPath and scriptLinkedService)</span></span> |
| <span data-ttu-id="b4da5-3129">degreeOfParallelism</span><span class="sxs-lookup"><span data-stu-id="b4da5-3129">degreeOfParallelism</span></span> |<span data-ttu-id="b4da5-3130">csomópontok maximális száma hello egyidejűleg használt toorun hello feladat.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3130">hello maximum number of nodes simultaneously used toorun hello job.</span></span> |<span data-ttu-id="b4da5-3131">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-3131">No</span></span> |
| <span data-ttu-id="b4da5-3132">Prioritás</span><span class="sxs-lookup"><span data-stu-id="b4da5-3132">priority</span></span> |<span data-ttu-id="b4da5-3133">Meghatározza, hogy szereplő várólistáján szereplő feladatok közül kell lennie a kijelölt toorun először.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3133">Determines which jobs out of all that are queued should be selected toorun first.</span></span> <span data-ttu-id="b4da5-3134">hello hello kevesebb, hello hello elsőbbséget.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3134">hello lower hello number, hello higher hello priority.</span></span> |<span data-ttu-id="b4da5-3135">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-3135">No</span></span> |
| <span data-ttu-id="b4da5-3136">paraméterek</span><span class="sxs-lookup"><span data-stu-id="b4da5-3136">parameters</span></span> |<span data-ttu-id="b4da5-3137">Hello U-SQL parancsfájl paramétereinek</span><span class="sxs-lookup"><span data-stu-id="b4da5-3137">Parameters for hello U-SQL script</span></span> |<span data-ttu-id="b4da5-3138">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-3138">No</span></span> |

### <a name="json-example"></a><span data-ttu-id="b4da5-3139">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-3139">JSON example</span></span>

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

<span data-ttu-id="b4da5-3140">További információkért lásd: [Data Lake Analytics U-SQL tevékenység](data-factory-usql-activity.md).</span><span class="sxs-lookup"><span data-stu-id="b4da5-3140">For more information, see [Data Lake Analytics U-SQL Activity](data-factory-usql-activity.md).</span></span> 

## <a name="stored-procedure-activity"></a><span data-ttu-id="b4da5-3141">Tárolt eljárási tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4da5-3141">Stored Procedure Activity</span></span>
<span data-ttu-id="b4da5-3142">A következő tárolt eljárás tevékenység JSON-definícióban tulajdonságok hello adhatja meg.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3142">You can specify hello following properties in a Stored Procedure Activity JSON definition.</span></span> <span data-ttu-id="b4da5-3143">hello type tulajdonság hello tevékenységnek kell lennie: **SqlServerStoredProcedure**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3143">hello type property for hello activity must be: **SqlServerStoredProcedure**.</span></span> <span data-ttu-id="b4da5-3144">Létre kell hoznia egy valamelyik a következő összekapcsolt szolgáltatások hello és hello értékeként adja meg a kapcsolódó hello szolgáltatás hello neve **linkedServiceName** tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="b4da5-3144">You must create an one of hello following linked services and specify hello name of hello linked service as a value for hello **linkedServiceName** property:</span></span>

- <span data-ttu-id="b4da5-3145">SQL Server</span><span class="sxs-lookup"><span data-stu-id="b4da5-3145">SQL Server</span></span> 
- <span data-ttu-id="b4da5-3146">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="b4da5-3146">Azure SQL Database</span></span>
- <span data-ttu-id="b4da5-3147">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="b4da5-3147">Azure SQL Data Warehouse</span></span>

<span data-ttu-id="b4da5-3148">hello következő tulajdonságok támogatottak hello **typeProperties** szakasz hello típusú tevékenység tooSqlServerStoredProcedure beállításakor:</span><span class="sxs-lookup"><span data-stu-id="b4da5-3148">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooSqlServerStoredProcedure:</span></span>

| <span data-ttu-id="b4da5-3149">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-3149">Property</span></span> | <span data-ttu-id="b4da5-3150">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-3150">Description</span></span> | <span data-ttu-id="b4da5-3151">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-3151">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4da5-3152">storedProcedureName</span><span class="sxs-lookup"><span data-stu-id="b4da5-3152">storedProcedureName</span></span> |<span data-ttu-id="b4da5-3153">Hello Azure SQL database vagy az Azure SQL Data Warehouse hello kapcsolódó szolgáltatás, amely a kimeneti tábla által használt hello által képviselt hello hello tárolt eljárás nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3153">Specify hello name of hello stored procedure in hello Azure SQL database or Azure SQL Data Warehouse that is represented by hello linked service that hello output table uses.</span></span> |<span data-ttu-id="b4da5-3154">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-3154">Yes</span></span> |
| <span data-ttu-id="b4da5-3155">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="b4da5-3155">storedProcedureParameters</span></span> |<span data-ttu-id="b4da5-3156">Adja meg a tárolt eljárás paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3156">Specify values for stored procedure parameters.</span></span> <span data-ttu-id="b4da5-3157">Ha toopass null egy paraméter, szintaxissal hello: "param1": (összes kisbetű) NULL értékű.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3157">If you need toopass null for a parameter, use hello syntax: "param1": null (all lower case).</span></span> <span data-ttu-id="b4da5-3158">Tekintse meg a következő minta toolearn e tulajdonság használatával kapcsolatos hello.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3158">See hello following sample toolearn about using this property.</span></span> |<span data-ttu-id="b4da5-3159">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-3159">No</span></span> |

<span data-ttu-id="b4da5-3160">Ha megad egy bemeneti adatkészlet, elérhetőnek kell lennie (a "Kész" állapotú) hello a tárolt eljárás tevékenység toorun.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3160">If you do specify an input dataset, it must be available (in ‘Ready’ status) for hello stored procedure activity toorun.</span></span> <span data-ttu-id="b4da5-3161">hello bemeneti adatkészlet hello tárolt eljárás nem használható paraméterként.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3161">hello input dataset cannot be consumed in hello stored procedure as a parameter.</span></span> <span data-ttu-id="b4da5-3162">Csak a felhasznált toocheck hello függőségi kezdési hello tárolt eljárási tevékenység előtt.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3162">It is only used toocheck hello dependency before starting hello stored procedure activity.</span></span> <span data-ttu-id="b4da5-3163">Meg kell adnia egy tárolt eljárás tevékenység egy kimeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3163">You must specify an output dataset for a stored procedure activity.</span></span> 

<span data-ttu-id="b4da5-3164">Kimeneti adatkészlet megadja hello **ütemezés** hello a tárolt eljárási tevékenység (óránként, heti, havi, stb.).</span><span class="sxs-lookup"><span data-stu-id="b4da5-3164">Output dataset specifies hello **schedule** for hello stored procedure activity (hourly, weekly, monthly, etc.).</span></span> <span data-ttu-id="b4da5-3165">hello kimeneti adatkészlet kell használnia egy **társított szolgáltatás** , amely hivatkozik tooan Azure SQL Database vagy az Azure SQL Data Warehouse vagy a használni kívánt tárolt eljárás toorun hello SQL Server-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3165">hello output dataset must use a **linked service** that refers tooan Azure SQL Database or an Azure SQL Data Warehouse or a SQL Server Database in which you want hello stored procedure toorun.</span></span> <span data-ttu-id="b4da5-3166">hello kimeneti adatkészlet ki tud szolgálni hello tárolt eljárás későbbi feldolgozásra módon toopass hello eredményként egy másik tevékenység ([tevékenységek láncolás](data-factory-scheduling-and-execution.md##multiple-activities-in-a-pipeline)) hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3166">hello output dataset can serve as a way toopass hello result of hello stored procedure for subsequent processing by another activity ([chaining activities](data-factory-scheduling-and-execution.md##multiple-activities-in-a-pipeline)) in hello pipeline.</span></span> <span data-ttu-id="b4da5-3167">Adat-előállító azonban nem a hello kimeneti a tárolt eljárás toothis DataSet adatkészlet automatikusan írnia.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3167">However, Data Factory does not automatically write hello output of a stored procedure toothis dataset.</span></span> <span data-ttu-id="b4da5-3168">Hello tárolt eljárás írások tooa SQL táblát, amely hello kimeneti adatkészlet mutat.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3168">It is hello stored procedure that writes tooa SQL table that hello output dataset points to.</span></span> <span data-ttu-id="b4da5-3169">Bizonyos esetekben hello kimeneti adatkészlet lehet egy **dummy dataset**, amellyel csak hello futtatási toospecify hello ütemezésének tárolt eljárási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3169">In some cases, hello output dataset can be a **dummy dataset**, which is used only toospecify hello schedule for running hello stored procedure activity.</span></span>  

### <a name="json-example"></a><span data-ttu-id="b4da5-3170">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-3170">JSON example</span></span>

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

<span data-ttu-id="b4da5-3171">További információkért lásd: [tárolt eljárási tevékenység](data-factory-stored-proc-activity.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3171">For more information, see [Stored Procedure Activity](data-factory-stored-proc-activity.md) article.</span></span> 

## <a name="net-custom-activity"></a><span data-ttu-id="b4da5-3172">.NET egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4da5-3172">.NET custom activity</span></span>
<span data-ttu-id="b4da5-3173">A következő tulajdonságok .NET egyéni tevékenység JSON-definícióból hello adhatja meg.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3173">You can specify hello following properties in a .NET custom activity JSON definition.</span></span> <span data-ttu-id="b4da5-3174">hello type tulajdonság hello tevékenységnek kell lennie: **DotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3174">hello type property for hello activity must be: **DotNetActivity**.</span></span> <span data-ttu-id="b4da5-3175">Létre kell hoznia egy Azure HDInsight kapcsolódó szolgáltatás, vagy egy társított Azure Batch szolgáltatás, és adja meg a hello társított szolgáltatás neve hello hello értéket **linkedServiceName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3175">You must create an Azure HDInsight linked service or an Azure Batch linked service, and specify hello name of hello linked service as a value for hello **linkedServiceName** property.</span></span> <span data-ttu-id="b4da5-3176">hello következő tulajdonságok támogatottak hello **typeProperties** szakasz hello típusú tevékenység tooDotNetActivity beállításakor:</span><span class="sxs-lookup"><span data-stu-id="b4da5-3176">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooDotNetActivity:</span></span>
 
| <span data-ttu-id="b4da5-3177">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4da5-3177">Property</span></span> | <span data-ttu-id="b4da5-3178">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4da5-3178">Description</span></span> | <span data-ttu-id="b4da5-3179">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b4da5-3179">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="b4da5-3180">AssemblyName</span><span class="sxs-lookup"><span data-stu-id="b4da5-3180">AssemblyName</span></span> | <span data-ttu-id="b4da5-3181">Hello szerelvény neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3181">Name of hello assembly.</span></span> <span data-ttu-id="b4da5-3182">Hello például, hogy a rendszer: **MyDotnetActivity.dll**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3182">In hello example, it is: **MyDotnetActivity.dll**.</span></span> | <span data-ttu-id="b4da5-3183">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-3183">Yes</span></span> |
| <span data-ttu-id="b4da5-3184">Belépési pont</span><span class="sxs-lookup"><span data-stu-id="b4da5-3184">EntryPoint</span></span> |<span data-ttu-id="b4da5-3185">Hello IDotNetActivity felületet megvalósító hello osztály neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3185">Name of hello class that implements hello IDotNetActivity interface.</span></span> <span data-ttu-id="b4da5-3186">Hello például, hogy a rendszer: **MyDotNetActivityNS.MyDotNetActivity** ahol a MyDotNetActivityNS hello névtere, ezért a MyDotNetActivity hello osztály.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3186">In hello example, it is: **MyDotNetActivityNS.MyDotNetActivity** where MyDotNetActivityNS is hello namespace and MyDotNetActivity is hello class.</span></span>  | <span data-ttu-id="b4da5-3187">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-3187">Yes</span></span> | 
| <span data-ttu-id="b4da5-3188">PackageLinkedService</span><span class="sxs-lookup"><span data-stu-id="b4da5-3188">PackageLinkedService</span></span> | <span data-ttu-id="b4da5-3189">Hello Azure Storage társított szolgáltatás mutat, toohello blob-tároló, amely tartalmazza a hello egyéni tevékenység zip-fájl neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3189">Name of hello Azure Storage linked service that points toohello blob storage that contains hello custom activity zip file.</span></span> <span data-ttu-id="b4da5-3190">Hello például, hogy a rendszer: **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3190">In hello example, it is: **AzureStorageLinkedService**.</span></span>| <span data-ttu-id="b4da5-3191">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-3191">Yes</span></span> |
| <span data-ttu-id="b4da5-3192">PackageFile</span><span class="sxs-lookup"><span data-stu-id="b4da5-3192">PackageFile</span></span> | <span data-ttu-id="b4da5-3193">Hello zip-fájl neve.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3193">Name of hello zip file.</span></span> <span data-ttu-id="b4da5-3194">Hello például, hogy a rendszer: **customactivitycontainer/MyDotNetActivity.zip**.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3194">In hello example, it is: **customactivitycontainer/MyDotNetActivity.zip**.</span></span> | <span data-ttu-id="b4da5-3195">Igen</span><span class="sxs-lookup"><span data-stu-id="b4da5-3195">Yes</span></span> |
| <span data-ttu-id="b4da5-3196">Az ExtendedProperties</span><span class="sxs-lookup"><span data-stu-id="b4da5-3196">extendedProperties</span></span> | <span data-ttu-id="b4da5-3197">A kiterjesztett tulajdonságok, amely meghatározza, és adja át a toohello .NET-kódot.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3197">Extended properties that you can define and pass on toohello .NET code.</span></span> <span data-ttu-id="b4da5-3198">Ebben a példában hello **SliceStart** változó értéke alapján hello SliceStart rendszerváltozó tooa érték.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3198">In this example, hello **SliceStart** variable is set tooa value based on hello SliceStart system variable.</span></span> | <span data-ttu-id="b4da5-3199">Nem</span><span class="sxs-lookup"><span data-stu-id="b4da5-3199">No</span></span> | 

### <a name="json-example"></a><span data-ttu-id="b4da5-3200">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="b4da5-3200">JSON example</span></span>

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

<span data-ttu-id="b4da5-3201">Részletes információkért lásd: [egyéni tevékenységeket felhasználni a Data Factory](data-factory-use-custom-activities.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4da5-3201">For detailed information, see [Use custom activities in Data Factory](data-factory-use-custom-activities.md) article.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="b4da5-3202">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b4da5-3202">Next Steps</span></span>
<span data-ttu-id="b4da5-3203">Tekintse meg a következő oktatóanyagok hello:</span><span class="sxs-lookup"><span data-stu-id="b4da5-3203">See hello following tutorials:</span></span> 

- [<span data-ttu-id="b4da5-3204">Oktatóanyag: hozzon létre egy folyamatot a másolási tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4da5-3204">Tutorial: create a pipeline with a copy activity</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [<span data-ttu-id="b4da5-3205">Oktatóanyag: hozzon létre egy folyamatot egy hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4da5-3205">Tutorial: create a pipeline with a hive activity</span></span>](data-factory-build-your-first-pipeline-using-editor.md)