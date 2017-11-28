---
title: aaaImport az adatok tooAnalytics Azure Application insightsban |} Microsoft Docs
description: "Az alkalmazás telemetriai statikus adatok toojoin importálni, vagy importáljon egy külön adatok adatfolyam tooquery az Analytics."
services: application-insights
keywords: "Nyissa meg a séma, az adatok importálása"
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: bwren
ms.openlocfilehash: 7a3a3c9155adc1885dd366ddb13dda80bb894adb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="import-data-into-analytics"></a><span data-ttu-id="ad626-104">Adatok importálása elemzés</span><span class="sxs-lookup"><span data-stu-id="ad626-104">Import data into Analytics</span></span>

<span data-ttu-id="ad626-105">Az adatok importálása [Analytics](app-insights-analytics.md), vagy toojoin együtt [Application Insights](app-insights-overview.md) telemetriai az alkalmazásból, vagy úgy, hogy egy külön adatfolyamként elemzése.</span><span class="sxs-lookup"><span data-stu-id="ad626-105">Import any tabular data into [Analytics](app-insights-analytics.md), either toojoin it with [Application Insights](app-insights-overview.md) telemetry from your app, or so that you can analyze it as a separate stream.</span></span> <span data-ttu-id="ad626-106">Elemzés egy hatékony lekérdezési nyelv jól alkalmazható tooanalyzing telemetriai a nagy mennyiségű időbélyegzővel adatfolyamokat.</span><span class="sxs-lookup"><span data-stu-id="ad626-106">Analytics is a powerful query language well-suited tooanalyzing high-volume timestamped streams of telemetry.</span></span>

<span data-ttu-id="ad626-107">Adatok importálhatja a Analytics saját sémáját használja.</span><span class="sxs-lookup"><span data-stu-id="ad626-107">You can import data into Analytics using your own schema.</span></span> <span data-ttu-id="ad626-108">Toouse hello szabványos Application Insights sémák például kérelem vagy a nyomkövetés nem tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ad626-108">It doesn't have toouse hello standard Application Insights schemas such as request or trace.</span></span>

<span data-ttu-id="ad626-109">Importálhatja, JSON vagy Adatforrásnézet (elválasztó tagolt - vesszővel, pontosvesszővel válassza el vagy lapon) fájlok.</span><span class="sxs-lookup"><span data-stu-id="ad626-109">You can import JSON or DSV (delimiter-separated values - comma, semicolon or tab) files.</span></span>

<span data-ttu-id="ad626-110">Nincsenek három olyan helyzetekben, ahol tooAnalytics importálása hasznos:</span><span class="sxs-lookup"><span data-stu-id="ad626-110">There are three situations where importing tooAnalytics is useful:</span></span>

* <span data-ttu-id="ad626-111">**Alkalmazás telemetriai csatlakozik.**</span><span class="sxs-lookup"><span data-stu-id="ad626-111">**Join with app telemetry.**</span></span> <span data-ttu-id="ad626-112">Például a táblázat, amely leképezhető URL-címeket a webhely toomore olvasható lap címének sikerült importálni.</span><span class="sxs-lookup"><span data-stu-id="ad626-112">For example, you could import a table that maps URLs from your website toomore readable page titles.</span></span> <span data-ttu-id="ad626-113">Elemzés létrehozhat egy irányítópult diagram jelentés hello tíz legnépszerűbb lap a webhelyen.</span><span class="sxs-lookup"><span data-stu-id="ad626-113">In Analytics, you can create a dashboard chart report that shows hello ten most popular pages in your website.</span></span> <span data-ttu-id="ad626-114">Most megmutatja hello lap címek hello URL-címei helyett.</span><span class="sxs-lookup"><span data-stu-id="ad626-114">Now it can show hello page titles instead of hello URLs.</span></span>
* <span data-ttu-id="ad626-115">**A telemetriai összefüggéseket** más adatforrások, például a hálózati forgalom, a kiszolgáló adatait, vagy a CDN-naplófájljai.</span><span class="sxs-lookup"><span data-stu-id="ad626-115">**Correlate your application telemetry** with other sources such as network traffic, server data, or CDN log files.</span></span>
* <span data-ttu-id="ad626-116">**Elemzés tooa külön adatfolyam alkalmazni.**</span><span class="sxs-lookup"><span data-stu-id="ad626-116">**Apply Analytics tooa separate data stream.**</span></span> <span data-ttu-id="ad626-117">Application Insights Analytics egy olyan erőteljes eszköz, amely ritka, időbélyegzővel adatfolyamok - SQL-re mint javulás megfelelően működik.</span><span class="sxs-lookup"><span data-stu-id="ad626-117">Application Insights Analytics is a powerful tool, that works well with sparse, timestamped streams - much better than SQL in many cases.</span></span> <span data-ttu-id="ad626-118">Ha ilyen adatfolyam más forrásból, elemezheti az Analytics.</span><span class="sxs-lookup"><span data-stu-id="ad626-118">If you have such a stream from some other source, you can analyze it with Analytics.</span></span>

<span data-ttu-id="ad626-119">Küldő tooyour adatforrás is könnyen.</span><span class="sxs-lookup"><span data-stu-id="ad626-119">Sending data tooyour data source is easy.</span></span> 

1. <span data-ttu-id="ad626-120">(Egy alkalommal) Az adatok "adatforrás" hello séma határozza meg.</span><span class="sxs-lookup"><span data-stu-id="ad626-120">(One time) Define hello schema of your data in a 'data source'.</span></span>
2. <span data-ttu-id="ad626-121">(Rendszeres) Töltse fel az adattárolás tooAzure, és hívja hello REST API toonotify, amely új adatok adatfeldolgozást vár.</span><span class="sxs-lookup"><span data-stu-id="ad626-121">(Periodically) Upload your data tooAzure storage, and call hello REST API toonotify us that new data is waiting for ingestion.</span></span> <span data-ttu-id="ad626-122">Néhány percen belül hello adatok érhető el elemzés lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="ad626-122">Within a few minutes, hello data is available for query in Analytics.</span></span>

<span data-ttu-id="ad626-123">hello hello feltöltés gyakoriságát határozza meg, és hogy milyen gyorsan kívánja, hogy az adatok toobe lekérdezhető.</span><span class="sxs-lookup"><span data-stu-id="ad626-123">hello frequency of hello upload is defined by you and how fast would you like your data toobe available for queries.</span></span> <span data-ttu-id="ad626-124">Nagyobb méretű adattömböket, de nem lehet nagyobb, mint 1 GB-os hatékonyabb tooupload adatok is.</span><span class="sxs-lookup"><span data-stu-id="ad626-124">It is more efficient tooupload data in larger chunks, but not larger than 1GB.</span></span>

> [!NOTE]
> <span data-ttu-id="ad626-125">*Nagy mennyiségű adatok források tooanalyze kapott?*</span><span class="sxs-lookup"><span data-stu-id="ad626-125">*Got lots of data sources tooanalyze?*</span></span> [<span data-ttu-id="ad626-126">*Érdemes lehet* logstash *tooship az Application Insights adatait.*</span><span class="sxs-lookup"><span data-stu-id="ad626-126">*Consider using* logstash *tooship your data into Application Insights.*</span></span>](https://github.com/Microsoft/logstash-output-application-insights)
> 

## <a name="before-you-start"></a><span data-ttu-id="ad626-127">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="ad626-127">Before you start</span></span>

<span data-ttu-id="ad626-128">A következők szükségesek:</span><span class="sxs-lookup"><span data-stu-id="ad626-128">You need:</span></span>

1. <span data-ttu-id="ad626-129">Az Application Insights-erőforrás a Microsoft Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="ad626-129">An Application Insights resource in Microsoft Azure.</span></span>

 * <span data-ttu-id="ad626-130">Ha azt szeretné, tooanalyze elkülönítve egyéb telemetriai adatait [hozzon létre egy új Application Insights-erőforrást](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="ad626-130">If you want tooanalyze your data separately from any other telemetry, [create a new Application Insights resource](app-insights-create-new-resource.md).</span></span>
 * <span data-ttu-id="ad626-131">Ha való csatlakozás vagy az alkalmazás már be van állítva az Application insights szolgáltatással telemetriai adatok összehasonlítása, majd használhatja hello erőforrást az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="ad626-131">If you're joining or comparing your data with telemetry from an app that is already set up with Application Insights, then you can use hello resource for that app.</span></span>
 * <span data-ttu-id="ad626-132">Közreműködő vagy tulajdonosi hozzáférés toothat erőforrás.</span><span class="sxs-lookup"><span data-stu-id="ad626-132">Contributor or owner access toothat resource.</span></span>
 
2. <span data-ttu-id="ad626-133">Az Azure storage.</span><span class="sxs-lookup"><span data-stu-id="ad626-133">Azure storage.</span></span> <span data-ttu-id="ad626-134">TooAzure storage-be feltölteni, és elemzés lekérdezi az adatok ott.</span><span class="sxs-lookup"><span data-stu-id="ad626-134">You upload tooAzure storage, and Analytics gets your data from there.</span></span> 

 * <span data-ttu-id="ad626-135">Javasoljuk, hogy hozzon létre egy dedikált tárfiókot, a blobok.</span><span class="sxs-lookup"><span data-stu-id="ad626-135">We recommend you create a dedicated storage account for your blobs.</span></span> <span data-ttu-id="ad626-136">Ha a blobok más folyamatokkal van megosztva, hosszabb ideig tart a folyamatok tooread a blobok.</span><span class="sxs-lookup"><span data-stu-id="ad626-136">If your blobs are shared with other processes, it takes longer for our processes tooread your blobs.</span></span>


## <a name="define-your-schema"></a><span data-ttu-id="ad626-137">A séma meghatározása</span><span class="sxs-lookup"><span data-stu-id="ad626-137">Define your schema</span></span>

<span data-ttu-id="ad626-138">Adatok importálása előtt meg kell adnia egy *adatforrás,* amely hello séma adatait adja meg.</span><span class="sxs-lookup"><span data-stu-id="ad626-138">Before you can import data, you must define a *data source,* which specifies hello schema of your data.</span></span>
<span data-ttu-id="ad626-139">Adatforrások too50 egyetlen Application Insights-erőforrás lehet</span><span class="sxs-lookup"><span data-stu-id="ad626-139">You can have up too50 data sources in a single Application Insights resource</span></span>

1. <span data-ttu-id="ad626-140">Hello adatforrás varázsló elindításához.</span><span class="sxs-lookup"><span data-stu-id="ad626-140">Start hello data source wizard.</span></span> <span data-ttu-id="ad626-141">Használja az "Új adatforrás hozzáadása" gombra.</span><span class="sxs-lookup"><span data-stu-id="ad626-141">Use "Add new data source" button.</span></span> <span data-ttu-id="ad626-142">Másik lehetőségként - kattintson a jobb felső sarkában található beállítások gombra, és kattintson "Az adatforrások" legördülő menüre.</span><span class="sxs-lookup"><span data-stu-id="ad626-142">Alternatively - click on settings button in right upper corner and choose "Data Sources" in dropdown menu.</span></span>

    ![Új adatforrás hozzáadása](./media/app-insights-analytics-import/add-new-data-source.png)

    <span data-ttu-id="ad626-144">Adja meg az új adatforrás nevét.</span><span class="sxs-lookup"><span data-stu-id="ad626-144">Provide a name for your new data source.</span></span>

2. <span data-ttu-id="ad626-145">Adja meg, hogy fel kell töltenie hello fájlok formátuma.</span><span class="sxs-lookup"><span data-stu-id="ad626-145">Define format of hello files that you will upload.</span></span>

    <span data-ttu-id="ad626-146">Manuálisan állítsa hello formátumot, vagy a minta-fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="ad626-146">You can either define hello format manually, or upload a sample file.</span></span>

    <span data-ttu-id="ad626-147">Ha hello adatok CSV formátumban, hello első sorának hello minta lehet oszlopfejléceket.</span><span class="sxs-lookup"><span data-stu-id="ad626-147">If hello data is in CSV format, hello first row of hello sample can be column headers.</span></span> <span data-ttu-id="ad626-148">Hello mező nevét a következő lépésben hello módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="ad626-148">You can change hello field names in hello next step.</span></span>

    <span data-ttu-id="ad626-149">hello minta tartalmaznia kell legalább 10 sorok vagy adatokat rögzíti.</span><span class="sxs-lookup"><span data-stu-id="ad626-149">hello sample should include at least 10 rows or records of data.</span></span>

    <span data-ttu-id="ad626-150">Oszlop vagy mező neve alfanumerikus nevek (nélkül szóköz vagy írásjel) kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="ad626-150">Column or field names should have alphanumeric names (without spaces or punctuation).</span></span>

    ![A minta-fájl feltöltése](./media/app-insights-analytics-import/sample-data-file.png)


3. <span data-ttu-id="ad626-152">Felülvizsgálati hello séma, amely varázsló hello kapott rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ad626-152">Review hello schema that hello wizard has got.</span></span> <span data-ttu-id="ad626-153">Ha következtetni hello típusok mintaként, esetleg újra kell tooadjust következtetni hello típusú hello oszlopok.</span><span class="sxs-lookup"><span data-stu-id="ad626-153">If it inferred hello types from a sample, you might need tooadjust hello inferred types of hello columns.</span></span>

    ![Tekintse át a hello következtetni séma](./media/app-insights-analytics-import/data-source-review-schema.png)

 * <span data-ttu-id="ad626-155">(Választható.) Töltse fel a sémadefiníciót.</span><span class="sxs-lookup"><span data-stu-id="ad626-155">(Optional.) Upload a schema definition.</span></span> <span data-ttu-id="ad626-156">Tekintse meg az alábbi hello formátumban.</span><span class="sxs-lookup"><span data-stu-id="ad626-156">See hello format below.</span></span>

 * <span data-ttu-id="ad626-157">Válassza ki az időbélyegző.</span><span class="sxs-lookup"><span data-stu-id="ad626-157">Select a Timestamp.</span></span> <span data-ttu-id="ad626-158">Minden adat Analytics kell rendelkezhetnek időbélyegmezővel.</span><span class="sxs-lookup"><span data-stu-id="ad626-158">All data in Analytics must have a timestamp field.</span></span> <span data-ttu-id="ad626-159">Típusúnak kell lennie `datetime`, de nincs beállítva a "időbélyeg" nevű toobe.</span><span class="sxs-lookup"><span data-stu-id="ad626-159">It must have type `datetime`, but it doesn't have toobe named 'timestamp'.</span></span> <span data-ttu-id="ad626-160">Ha az adatok egy dátum és idő ISO formátumú tartalmazó oszlop, válassza ezt a hello Timestamp típusú oszlop.</span><span class="sxs-lookup"><span data-stu-id="ad626-160">If your data has a column containing a date and time in ISO format, choose this as hello timestamp column.</span></span> <span data-ttu-id="ad626-161">Ellenkező esetben válassza a "Data a Beérkezett", és hello az importálási folyamat felveszi időbélyegmezővel.</span><span class="sxs-lookup"><span data-stu-id="ad626-161">Otherwise, choose "as data arrived", and hello import process will add a timestamp field.</span></span>

5. <span data-ttu-id="ad626-162">Hello adatforrás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ad626-162">Create hello data source.</span></span>

### <a name="schema-definition-file-format"></a><span data-ttu-id="ad626-163">Séma csomagdefiníciós fájlformátumról</span><span class="sxs-lookup"><span data-stu-id="ad626-163">Schema definition file format</span></span>

<span data-ttu-id="ad626-164">A felhasználói felületen hello séma szerkesztése, helyett betöltése hello sémadefiníciót fájlból.</span><span class="sxs-lookup"><span data-stu-id="ad626-164">Instead of editing hello schema in UI, you can load hello schema definition from a file.</span></span> <span data-ttu-id="ad626-165">hello schema definíció formátum a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="ad626-165">hello schema definition format is as follows:</span></span> 

<span data-ttu-id="ad626-166">Tagolt formátumú</span><span class="sxs-lookup"><span data-stu-id="ad626-166">Delimited format</span></span> 
```
[ 
    {"location": "0", "name": "RequestName", "type": "string"}, 
    {"location": "1", "name": "timestamp", "type": "datetime"}, 
    {"location": "2", "name": "IPAddress", "type": "string"} 
] 
```

<span data-ttu-id="ad626-167">JSON formátumban</span><span class="sxs-lookup"><span data-stu-id="ad626-167">JSON format</span></span> 
```
[ 
    {"location": "$.name", "name": "name", "type": "string"}, 
    {"location": "$.alias", "name": "alias", "type": "string"}, 
    {"location": "$.room", "name": "room", "type": "long"} 
]
```
 
<span data-ttu-id="ad626-168">Egyes oszlopok azonosíthatók hello helyét, nevét és típusát.</span><span class="sxs-lookup"><span data-stu-id="ad626-168">Each column is identified by hello location, name and type.</span></span> 

* <span data-ttu-id="ad626-169">Hely – tagolt fájl Format azt leképezve hello érték hello helyének.</span><span class="sxs-lookup"><span data-stu-id="ad626-169">Location – For delimited file format it is hello position of hello mapped value.</span></span> <span data-ttu-id="ad626-170">A JSON formátum esetén hello jpath leképezett hello kulcs.</span><span class="sxs-lookup"><span data-stu-id="ad626-170">For JSON format, it is hello jpath of hello mapped key.</span></span>
* <span data-ttu-id="ad626-171">Név – hello hello oszlop neve jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="ad626-171">Name – hello displayed name of hello column.</span></span>
* <span data-ttu-id="ad626-172">Írja be – hello adatok írja be annak az oszlopnak.</span><span class="sxs-lookup"><span data-stu-id="ad626-172">Type – hello data type of that column.</span></span>
 
<span data-ttu-id="ad626-173">Abban az esetben a mintaadatok lett megadva, és fájlformátumot csak, a hello sémadefiníciót kell az összes oszlop leképezése, és adja hozzá az új oszlopok hello végén.</span><span class="sxs-lookup"><span data-stu-id="ad626-173">In case a sample data was used and file format is delimited, hello schema definition must map all columns and add new columns at hello end.</span></span> 

<span data-ttu-id="ad626-174">JSON lehetővé teszi, hogy a részleges leképezést hello adatok, ezért hello sémadefiníciót JSON formátum nem rendelkezik toomap minden kulcsot, amely megtalálható a mintaadatokat.</span><span class="sxs-lookup"><span data-stu-id="ad626-174">JSON allows partial mapping of hello data, therefore hello schema definition of JSON format doesn’t have toomap every key which is found in a sample data.</span></span> <span data-ttu-id="ad626-175">Az oszlopokat, amelyek nem részei hello mintaadatok is leképezheti.</span><span class="sxs-lookup"><span data-stu-id="ad626-175">It can also map columns which are not part of hello sample data.</span></span> 

## <a name="import-data"></a><span data-ttu-id="ad626-176">Adatok importálása</span><span class="sxs-lookup"><span data-stu-id="ad626-176">Import data</span></span>

<span data-ttu-id="ad626-177">tooimport adatok tooAzure tárolási töltse fel, az access-kulcs létrehozása, és hajtsa végre az egy REST API-hívás.</span><span class="sxs-lookup"><span data-stu-id="ad626-177">tooimport data, you upload it tooAzure storage, create an access key for it, and then make a REST API call.</span></span>

![Új adatforrás hozzáadása](./media/app-insights-analytics-import/analytics-upload-process.png)

<span data-ttu-id="ad626-179">Hajtsa végre a folyamatot követve manuálisan hello, vagy állítsa be az automatikus rendszer-toodo, rendszeres időközönként.</span><span class="sxs-lookup"><span data-stu-id="ad626-179">You can perform hello following process manually, or set up an automated system toodo it at regular intervals.</span></span> <span data-ttu-id="ad626-180">Meg kell toofollow ezeket a lépéseket az egyes adatblokk kívánt tooimport.</span><span class="sxs-lookup"><span data-stu-id="ad626-180">You need toofollow these steps for each block of data you want tooimport.</span></span>

1. <span data-ttu-id="ad626-181">Fájlok feltöltése hello túl[Azure blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="ad626-181">Upload hello data too[Azure blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> 

 * <span data-ttu-id="ad626-182">Blobok tömörítetlen too1GB fel tetszőleges méretű lehet.</span><span class="sxs-lookup"><span data-stu-id="ad626-182">Blobs can be any size up too1GB uncompressed.</span></span> <span data-ttu-id="ad626-183">Nagyméretű bináris objektumok MB több száz alkalmasak a teljesítmény szempontjából.</span><span class="sxs-lookup"><span data-stu-id="ad626-183">Large blobs of hundreds of MB are ideal from a performance perspective.</span></span>
 * <span data-ttu-id="ad626-184">A Gzip tooimprove ideje és késés mellett hello adatok toobe lekérdezhetők a tömöríthetők.</span><span class="sxs-lookup"><span data-stu-id="ad626-184">You can compress it with Gzip tooimprove upload time and latency for hello data toobe available for query.</span></span> <span data-ttu-id="ad626-185">Használjon hello `.gz` fájlnév-kiterjesztés.</span><span class="sxs-lookup"><span data-stu-id="ad626-185">Use hello `.gz` filename extension.</span></span>
 * <span data-ttu-id="ad626-186">Ajánlott toouse külön tárfiókot erre a célra, tooavoid hívások a különböző szolgáltatások lelassulnak, ami a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="ad626-186">It's best toouse a separate storage account for this purpose, tooavoid calls from different services slowing performance.</span></span>
 * <span data-ttu-id="ad626-187">Adatküldés nagy gyakorisággal, minden néhány másodpercben esetén ajánlott toouse több mint egy tárfiókot, a teljesítmény szempontjából.</span><span class="sxs-lookup"><span data-stu-id="ad626-187">When sending data in high frequency, every few seconds, it is recommended toouse more than one storage account, for performance reasons.</span></span>

 
2. <span data-ttu-id="ad626-188">[Hozzon létre egy közös hozzáférésű Jogosultságkód kulcsot hello BLOB](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md).</span><span class="sxs-lookup"><span data-stu-id="ad626-188">[Create a Shared Access Signature key for hello blob](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md).</span></span> <span data-ttu-id="ad626-189">hello kulcsot kell egy olyan lejárati időt egy nap van, és olvasási hozzáférést biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="ad626-189">hello key should have an expiration period of one day and provide read access.</span></span>
3. <span data-ttu-id="ad626-190">Ellenőrizze egy REST-hívást toonotify adatok váró Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ad626-190">Make a REST call toonotify Application Insights that data is waiting.</span></span>

 * <span data-ttu-id="ad626-191">Végpont:`https://dc.services.visualstudio.com/v2/track`</span><span class="sxs-lookup"><span data-stu-id="ad626-191">Endpoint: `https://dc.services.visualstudio.com/v2/track`</span></span>
 * <span data-ttu-id="ad626-192">HTTP-metódus: POST</span><span class="sxs-lookup"><span data-stu-id="ad626-192">HTTP method: POST</span></span>
 * <span data-ttu-id="ad626-193">Hasznos:</span><span class="sxs-lookup"><span data-stu-id="ad626-193">Payload:</span></span>

```JSON

    {
       "data": {
            "baseType":"OpenSchemaData",
            "baseData":{
               "ver":"2",
               "blobSasUri":"<Blob URI with Shared Access Key>",
               "sourceName":"<Schema ID>",
               "sourceVersion":"1.0"
             }
       },
       "ver":1,
       "name":"Microsoft.ApplicationInsights.OpenSchema",
       "time":"<DateTime>",
       "iKey":"<instrumentation key>"
    }
```

<span data-ttu-id="ad626-194">hello helyőrzői:</span><span class="sxs-lookup"><span data-stu-id="ad626-194">hello placeholders are:</span></span>

* <span data-ttu-id="ad626-195">`Blob URI with Shared Access Key`: Kap a hello eljárás a kulcs létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="ad626-195">`Blob URI with Shared Access Key`: You get this from hello procedure for creating a key.</span></span> <span data-ttu-id="ad626-196">Adott toohello blob.</span><span class="sxs-lookup"><span data-stu-id="ad626-196">It is specific toohello blob.</span></span>
* <span data-ttu-id="ad626-197">`Schema ID`: a megadott séma létrehozott séma azonosító hello.</span><span class="sxs-lookup"><span data-stu-id="ad626-197">`Schema ID`: hello schema ID generated for your defined schema.</span></span> <span data-ttu-id="ad626-198">a blob hello adatokat meg kell felelniük toohello séma.</span><span class="sxs-lookup"><span data-stu-id="ad626-198">hello data in this blob should conform toohello schema.</span></span>
* <span data-ttu-id="ad626-199">`DateTime`: mely hello: küldés, hello ideje (UTC).</span><span class="sxs-lookup"><span data-stu-id="ad626-199">`DateTime`: hello time at which hello request is submitted, UTC.</span></span> <span data-ttu-id="ad626-200">Ezek a formátumok fogadunk: ISO8601 (például "2016-01-01 13:45:01"); RFC822 ("Sze, 14 Dec 16 14:57:01 + 0000"); RFC850 ("szerda, 14-Dec-16 14:57:00 UTC"); RFC1123 ("Sze, 14 Dec 2016 14:57:00 + 0000").</span><span class="sxs-lookup"><span data-stu-id="ad626-200">We accept these formats: ISO8601 (like "2016-01-01 13:45:01"); RFC822 ("Wed, 14 Dec 16 14:57:01 +0000"); RFC850 ("Wednesday, 14-Dec-16 14:57:00 UTC"); RFC1123 ("Wed, 14 Dec 2016 14:57:00 +0000").</span></span>
* <span data-ttu-id="ad626-201">`Instrumentation key`az Application Insights-erőforrás.</span><span class="sxs-lookup"><span data-stu-id="ad626-201">`Instrumentation key` of your Application Insights resource.</span></span>

<span data-ttu-id="ad626-202">hello érhetők el az adatok Analytics néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="ad626-202">hello data is available in Analytics after a few minutes.</span></span>

## <a name="error-responses"></a><span data-ttu-id="ad626-203">Hibaüzenetek</span><span class="sxs-lookup"><span data-stu-id="ad626-203">Error responses</span></span>

* <span data-ttu-id="ad626-204">**400 Hibás kérelem**: azt jelzi, hogy hello-kérések forgalma érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="ad626-204">**400 bad request**: indicates that hello request payload is invalid.</span></span> <span data-ttu-id="ad626-205">Részletek:</span><span class="sxs-lookup"><span data-stu-id="ad626-205">Check:</span></span>
 * <span data-ttu-id="ad626-206">Megfelelő instrumentation kulcs.</span><span class="sxs-lookup"><span data-stu-id="ad626-206">Correct instrumentation key.</span></span>
 * <span data-ttu-id="ad626-207">Érvényes időérték.</span><span class="sxs-lookup"><span data-stu-id="ad626-207">Valid time value.</span></span> <span data-ttu-id="ad626-208">Hello ideje most UTC Formátumban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ad626-208">It should be hello time now in UTC.</span></span>
 * <span data-ttu-id="ad626-209">JSON hello esemény megfelel toohello séma.</span><span class="sxs-lookup"><span data-stu-id="ad626-209">JSON of hello event conforms toohello schema.</span></span>
* <span data-ttu-id="ad626-210">**403 Tiltott**: elküldött hello blob nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="ad626-210">**403 Forbidden**: hello blob you've sent is not accessible.</span></span> <span data-ttu-id="ad626-211">Győződjön meg arról, hogy hello megosztott elérési kulcsával érvényes, és nem járt le.</span><span class="sxs-lookup"><span data-stu-id="ad626-211">Make sure that hello shared access key is valid and has not expired.</span></span>
* <span data-ttu-id="ad626-212">**Nem található 404**:</span><span class="sxs-lookup"><span data-stu-id="ad626-212">**404 Not Found**:</span></span>
 * <span data-ttu-id="ad626-213">hello blob nem létezik.</span><span class="sxs-lookup"><span data-stu-id="ad626-213">hello blob doesn't exist.</span></span>
 * <span data-ttu-id="ad626-214">hello sourceId nem megfelelő.</span><span class="sxs-lookup"><span data-stu-id="ad626-214">hello sourceId is wrong.</span></span>

<span data-ttu-id="ad626-215">További részletes információk a hello válasz hibaüzenet érhető el.</span><span class="sxs-lookup"><span data-stu-id="ad626-215">More detailed information is available in hello response error message.</span></span>


## <a name="sample-code"></a><span data-ttu-id="ad626-216">Mintakód</span><span class="sxs-lookup"><span data-stu-id="ad626-216">Sample code</span></span>

<span data-ttu-id="ad626-217">Ezt a kódot használja hello [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/9.0.1) NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="ad626-217">This code uses hello [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/9.0.1) NuGet package.</span></span>

### <a name="classes"></a><span data-ttu-id="ad626-218">Osztályok</span><span class="sxs-lookup"><span data-stu-id="ad626-218">Classes</span></span>

```C#
namespace IngestionClient 
{ 
    using System; 
    using Newtonsoft.Json; 

    public class AnalyticsDataSourceIngestionRequest 
    { 
        #region Members 
        private const string BaseDataRequiredVersion = "2"; 
        private const string RequestName = "Microsoft.ApplicationInsights.OpenSchema"; 
        #endregion Members 

        public AnalyticsDataSourceIngestionRequest(string ikey, string schemaId, string blobSasUri, int version = 1) 
        { 
            Ver = version; 
            IKey = ikey; 
            Data = new Data 
            { 
                BaseData = new BaseData 
                { 
                    Ver = BaseDataRequiredVersion, 
                    BlobSasUri = blobSasUri, 
                    SourceName = schemaId, 
                    SourceVersion = version.ToString() 
                } 
            }; 
        } 


        [JsonProperty("data")] 
        public Data Data { get; set; } 

        [JsonProperty("ver")] 
        public int Ver { get; set; } 

        [JsonProperty("name")] 
        public string Name { get { return RequestName; } } 

        [JsonProperty("time")] 
        public DateTime Time { get { return DateTime.UtcNow; } } 

        [JsonProperty("iKey")] 
        public string IKey { get; set; } 
    } 

    #region Internal Classes 

    public class Data 
    { 
        private const string DataBaseType = "OpenSchemaData"; 

        [JsonProperty("baseType")] 
        public string BaseType 
        { 
            get { return DataBaseType; } 
        } 

        [JsonProperty("baseData")] 
        public BaseData BaseData { get; set; } 
    } 


    public class BaseData 
    { 
        [JsonProperty("ver")] 
        public string Ver { get; set; } 

        [JsonProperty("blobSasUri")] 
        public string BlobSasUri { get; set; } 

        [JsonProperty("sourceName")] 
        public string SourceName { get; set; } 

        [JsonProperty("sourceVersion")] 
        public string SourceVersion { get; set; } 
    } 

    #endregion Internal Classes 
} 


namespace IngestionClient 
{ 
    using System; 
    using System.IO; 
    using System.Net; 
    using System.Text; 
    using System.Threading.Tasks; 
    using Newtonsoft.Json; 

    public class AnalyticsDataSourceClient 
    { 
        #region Members 
        private readonly Uri endpoint = new Uri("https://dc.services.visualstudio.com/v2/track"); 
        private const string RequestContentType = "application/json; charset=UTF-8"; 
        private const string RequestAccess = "application/json"; 
        #endregion Members 

        #region Public 

        public async Task<bool> RequestBlobIngestion(AnalyticsDataSourceIngestionRequest ingestionRequest) 
        { 
            HttpWebRequest request = WebRequest.CreateHttp(endpoint); 
            request.Method = WebRequestMethods.Http.Post; 
            request.ContentType = RequestContentType; 
            request.Accept = RequestAccess; 

            string notificationJson = Serialize(ingestionRequest); 
            byte[] notificationBytes = GetContentBytes(notificationJson); 
            request.ContentLength = notificationBytes.Length; 

            Stream requestStream = request.GetRequestStream(); 
            requestStream.Write(notificationBytes, 0, notificationBytes.Length); 
            requestStream.Close(); 

            try 
            { 
                using (var response = (HttpWebResponse)await request.GetResponseAsync())
                {
                    return response.StatusCode == HttpStatusCode.OK;
                }
            } 
            catch (WebException e) 
            { 
                HttpWebResponse httpResponse = e.Response as HttpWebResponse; 
                if (httpResponse != null) 
                { 
                    Console.WriteLine( 
                        "Ingestion request failed with status code: {0}. Error: {1}", 
                        httpResponse.StatusCode, 
                        httpResponse.StatusDescription); 
                    return false; 
                }
                throw; 
            } 
        } 
        #endregion Public 

        #region Private 
        private byte[] GetContentBytes(string content) 
        { 
            return Encoding.UTF8.GetBytes(content);
        } 


        private string Serialize(AnalyticsDataSourceIngestionRequest ingestionRequest) 
        { 
            return JsonConvert.SerializeObject(ingestionRequest); 
        } 
        #endregion Private 
    } 
} 
```

### <a name="ingest-data"></a><span data-ttu-id="ad626-219">Adatok kigyűjtése</span><span class="sxs-lookup"><span data-stu-id="ad626-219">Ingest data</span></span>

<span data-ttu-id="ad626-220">Ez a kód használható minden egyes blob.</span><span class="sxs-lookup"><span data-stu-id="ad626-220">Use this code for each blob.</span></span> 

```C#
   AnalyticsDataSourceClient client = new AnalyticsDataSourceClient(); 

   var ingestionRequest = new AnalyticsDataSourceIngestionRequest("iKey", "sourceId", "blobUrlWithSas"); 

   bool success = await client.RequestBlobIngestion(ingestionRequest);
```

## <a name="next-steps"></a><span data-ttu-id="ad626-221">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ad626-221">Next steps</span></span>

* [<span data-ttu-id="ad626-222">Log Analytics lekérdezési nyelv hello bemutatása</span><span class="sxs-lookup"><span data-stu-id="ad626-222">Tour of hello Log Analytics query language</span></span>](app-insights-analytics-tour.md)
* [<span data-ttu-id="ad626-223">Használjon *Logstash* toosend adatok tooApplication Insights</span><span class="sxs-lookup"><span data-stu-id="ad626-223">Use *Logstash* toosend data tooApplication Insights</span></span>](https://github.com/Microsoft/logstash-output-application-insights)
