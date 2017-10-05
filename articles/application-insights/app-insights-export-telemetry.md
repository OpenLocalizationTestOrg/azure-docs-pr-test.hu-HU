---
title: "Az Application Insights telemetria a folyamatos exportálás |} Microsoft Docs"
description: "Diagnosztikai és használati adatok exportálása Microsoft Azure-tárhelyre, és töltse le innen."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 5b859200-b484-4c98-9d9f-929713f1030c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: bwren
ms.openlocfilehash: 6ac3bda5101593b5ca66b4c9035e2fdac9d1e833
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="export-telemetry-from-application-insights"></a><span data-ttu-id="15a12-103">Az Application Insights telemetria exportálása</span><span class="sxs-lookup"><span data-stu-id="15a12-103">Export telemetry from Application Insights</span></span>
<span data-ttu-id="15a12-104">Szeretné megőrizni a telemetriai adat hosszabb, mint a szokásos megőrzési időszakot?</span><span class="sxs-lookup"><span data-stu-id="15a12-104">Want to keep your telemetry for longer than the standard retention period?</span></span> <span data-ttu-id="15a12-105">Vagy valamilyen speciális módon dolgozza fel?</span><span class="sxs-lookup"><span data-stu-id="15a12-105">Or process it in some specialized way?</span></span> <span data-ttu-id="15a12-106">A folyamatos exportálás ideális ehhez.</span><span class="sxs-lookup"><span data-stu-id="15a12-106">Continuous Export is ideal for this.</span></span> <span data-ttu-id="15a12-107">Az eseményeket az Application Insights portáljáról látható JSON formátumban a Microsoft Azure storage exportálhatja.</span><span class="sxs-lookup"><span data-stu-id="15a12-107">The events you see in the Application Insights portal can be exported to storage in Microsoft Azure in JSON format.</span></span> <span data-ttu-id="15a12-108">Ott töltse le az adatok és bármit kódját, írási kell feldolgozni azt.</span><span class="sxs-lookup"><span data-stu-id="15a12-108">From there you can download your data and write whatever code you need to process it.</span></span>  

<span data-ttu-id="15a12-109">Használja a folyamatos exportálás járhat egy kell külön fizetni.</span><span class="sxs-lookup"><span data-stu-id="15a12-109">Using Continuous Export may incur an additional charge.</span></span> <span data-ttu-id="15a12-110">Ellenőrizze a [fizetési modell](http://azure.microsoft.com/pricing/details/application-insights/).</span><span class="sxs-lookup"><span data-stu-id="15a12-110">Check your [pricing model](http://azure.microsoft.com/pricing/details/application-insights/).</span></span>

<span data-ttu-id="15a12-111">A folyamatos exportálás beállítása előtt van néhány más-érdemes figyelembe venni:</span><span class="sxs-lookup"><span data-stu-id="15a12-111">Before you set up continuous export, there are some alternatives you might want to consider:</span></span>

* <span data-ttu-id="15a12-112">A metrikák vagy keresési panel tetején az Exportálás gomb lehetővé teszi a táblák átviteli, és Excel-táblázatba diagramokat.</span><span class="sxs-lookup"><span data-stu-id="15a12-112">The Export button at the top of a metrics or search blade lets you transfer tables and charts to an Excel spreadsheet.</span></span>

* <span data-ttu-id="15a12-113">[Elemzés](app-insights-analytics.md) hatékony lekérdezésnyelvet biztosít a telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="15a12-113">[Analytics](app-insights-analytics.md) provides a powerful query language for telemetry.</span></span> <span data-ttu-id="15a12-114">Exportálhatja eredmények.</span><span class="sxs-lookup"><span data-stu-id="15a12-114">It can also export results.</span></span>
* <span data-ttu-id="15a12-115">Ha a keresett [az adatokba a Power BI](app-insights-export-power-bi.md), azt teheti a folyamatos exportálás használata nélkül.</span><span class="sxs-lookup"><span data-stu-id="15a12-115">If you're looking to [explore your data in Power BI](app-insights-export-power-bi.md), you can do that without using Continuous Export.</span></span>
* <span data-ttu-id="15a12-116">A [adatelérési REST API](https://dev.applicationinsights.io/) programozott hozzáférést a telemetriai adatok.</span><span class="sxs-lookup"><span data-stu-id="15a12-116">The [Data access REST API](https://dev.applicationinsights.io/) lets you access your telemetry programmatically.</span></span>

<span data-ttu-id="15a12-117">A folyamatos exportálás nem másolta az adatok (Ha azt is marad mindaddig, amíg tetszés) tárhelyre, továbbra is rendelkezésre áll az Application Insightsban a szokásos [megőrzési időszak](app-insights-data-retention-privacy.md).</span><span class="sxs-lookup"><span data-stu-id="15a12-117">After Continuous Export copies your data to storage (where it can stay for as long as you like), it's still available in Application Insights for the usual [retention period](app-insights-data-retention-privacy.md).</span></span>

## <span data-ttu-id="15a12-118"><a name="setup"></a>A folyamatos exportálás létrehozása</span><span class="sxs-lookup"><span data-stu-id="15a12-118"><a name="setup"></a> Create a Continuous Export</span></span>
1. <span data-ttu-id="15a12-119">Az Application Insights-erőforrást az alkalmazáshoz, nyissa meg a folyamatos exportálás, és válassza a **Hozzáadás**:</span><span class="sxs-lookup"><span data-stu-id="15a12-119">In the Application Insights resource for your app, open Continuous Export and choose **Add**:</span></span>

    ![Görgessen le, majd kattintson a folyamatos exportálás](./media/app-insights-export-telemetry/01-export.png)

2. <span data-ttu-id="15a12-121">Válassza ki az exportálni kívánt adattípusok telemetriai adatok.</span><span class="sxs-lookup"><span data-stu-id="15a12-121">Choose the telemetry data types you want to export.</span></span>

3. <span data-ttu-id="15a12-122">Hozzon létre vagy válasszon egy [Azure storage-fiók](../storage/common/storage-introduction.md) hol szeretné tárolni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="15a12-122">Create or select an [Azure storage account](../storage/common/storage-introduction.md) where you want to store the data.</span></span>

    > [!Warning]
    > <span data-ttu-id="15a12-123">Alapértelmezés szerint a tárolóhely állítja be az Application Insights-erőforrás és ugyanabban a földrajzi régióban.</span><span class="sxs-lookup"><span data-stu-id="15a12-123">By default, the storage location will be set to the same geographical region as your Application Insights resource.</span></span> <span data-ttu-id="15a12-124">Ha egy másik régióban vannak tárolva, az átviteli további költségekkel járhat.</span><span class="sxs-lookup"><span data-stu-id="15a12-124">If you store in a different region, you may incur transfer charges.</span></span>

    ![Kattintson a Hozzáadás gombra, exportálás cél tárfiók, és majd létrehoz egy új vagy egy meglévő tároló kiválasztása](./media/app-insights-export-telemetry/02-add.png)

4. <span data-ttu-id="15a12-126">Hozzon létre vagy jelöljön ki egy tárolót a tárolóban:</span><span class="sxs-lookup"><span data-stu-id="15a12-126">Create or select a container in the storage:</span></span>

    ![Válasszon eseménytípust kattintson](./media/app-insights-export-telemetry/create-container.png)

<span data-ttu-id="15a12-128">Ha létrehozta az exportálás, fog kezdődik.</span><span class="sxs-lookup"><span data-stu-id="15a12-128">Once you've created your export, it starts going.</span></span> <span data-ttu-id="15a12-129">Miután létrehozta az Exportálás érkező adatok csak kap.</span><span class="sxs-lookup"><span data-stu-id="15a12-129">You only get data that arrives after you create the export.</span></span>

<span data-ttu-id="15a12-130">Körülbelül egy óra alatt jelenik meg adat tárolására előtt várnia lehet.</span><span class="sxs-lookup"><span data-stu-id="15a12-130">There can be a delay of about an hour before data appears in the storage.</span></span>

### <a name="to-edit-continuous-export"></a><span data-ttu-id="15a12-131">A folyamatos exportálás szerkesztése</span><span class="sxs-lookup"><span data-stu-id="15a12-131">To edit continuous export</span></span>

<span data-ttu-id="15a12-132">Ha módosítani szeretné a eseménytípusok később, az Exportálás módosításával:</span><span class="sxs-lookup"><span data-stu-id="15a12-132">If you want to change the event types later, just edit the export:</span></span>

![Válasszon eseménytípust kattintson](./media/app-insights-export-telemetry/05-edit.png)

### <a name="to-stop-continuous-export"></a><span data-ttu-id="15a12-134">A folyamatos Exportálás leállítása</span><span class="sxs-lookup"><span data-stu-id="15a12-134">To stop continuous export</span></span>

<span data-ttu-id="15a12-135">Az Exportálás leállításához kattintson a letiltása.</span><span class="sxs-lookup"><span data-stu-id="15a12-135">To stop the export, click Disable.</span></span> <span data-ttu-id="15a12-136">Engedélyezze újra kattint, az Exportálás újraindul az új adatokat.</span><span class="sxs-lookup"><span data-stu-id="15a12-136">When you click Enable again, the export will restart with new data.</span></span> <span data-ttu-id="15a12-137">Az Exportálás le lett tiltva, amíg a portálon érkező adatokat nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="15a12-137">You won't get the data that arrived in the portal while export was disabled.</span></span>

<span data-ttu-id="15a12-138">Véglegesen állítsa le az exportálást, törölje azt.</span><span class="sxs-lookup"><span data-stu-id="15a12-138">To stop the export permanently, delete it.</span></span> <span data-ttu-id="15a12-139">Így nem törlése az adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="15a12-139">Doing so doesn't delete your data from storage.</span></span>

### <a name="cant-add-or-change-an-export"></a><span data-ttu-id="15a12-140">Nem adható hozzá, vagy módosítsa az export?</span><span class="sxs-lookup"><span data-stu-id="15a12-140">Can't add or change an export?</span></span>
* <span data-ttu-id="15a12-141">Adja hozzá, vagy módosítsa a kivitel, tulajdonos, közreműködő vagy Application Insights közreműködői jogokkal kell.</span><span class="sxs-lookup"><span data-stu-id="15a12-141">To add or change exports, you need Owner, Contributor or Application Insights Contributor access rights.</span></span> <span data-ttu-id="15a12-142">[További tudnivalók a szerepkörök][roles].</span><span class="sxs-lookup"><span data-stu-id="15a12-142">[Learn about roles][roles].</span></span>

## <span data-ttu-id="15a12-143"><a name="analyze"></a>Milyen eseményeket kapott?</span><span class="sxs-lookup"><span data-stu-id="15a12-143"><a name="analyze"></a> What events do you get?</span></span>
<span data-ttu-id="15a12-144">Az exportált adatok a nyers telemetriaadatok az alkalmazásból érkező, azzal a különbséggel, hogy az ügyfél IP-címről érkező helyadatok, amelyek Microsoft jelenleg felvenni.</span><span class="sxs-lookup"><span data-stu-id="15a12-144">The exported data is the raw telemetry we receive from your application, except that we add location data which we calculate from the client IP address.</span></span>

<span data-ttu-id="15a12-145">Adatok, amelyek szerint el lett vetve [mintavételi](app-insights-sampling.md) nem szerepel az exportált adatokat.</span><span class="sxs-lookup"><span data-stu-id="15a12-145">Data that has been discarded by [sampling](app-insights-sampling.md) is not included in the exported data.</span></span>

<span data-ttu-id="15a12-146">Más számított metrikák nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="15a12-146">Other calculated metrics are not included.</span></span> <span data-ttu-id="15a12-147">Például azt átlagos CPU-felhasználása ne exportálja, de a nyers telemetriaadatok, amelyből az átlagos számított exportálása.</span><span class="sxs-lookup"><span data-stu-id="15a12-147">For example, we don't export average CPU utilisation, but we do export the raw telemetry from which the average is computed.</span></span>

<span data-ttu-id="15a12-148">Az adatokat is tartalmaz, az eredmények közül [webteszt rendelkezésre állási](app-insights-monitor-web-app-availability.md) , amely meg van adva.</span><span class="sxs-lookup"><span data-stu-id="15a12-148">The data also includes the results of any [availability web tests](app-insights-monitor-web-app-availability.md) that you have set up.</span></span>

> [!NOTE]
> <span data-ttu-id="15a12-149">**Mintavételi.**</span><span class="sxs-lookup"><span data-stu-id="15a12-149">**Sampling.**</span></span> <span data-ttu-id="15a12-150">Az alkalmazás nagy mennyiségű adatot küld, ha a mintavételi szolgáltatás működik, és csak töredéke alatt a generált telemetriai adatot küldeni.</span><span class="sxs-lookup"><span data-stu-id="15a12-150">If your application sends a lot of data, the sampling feature may operate and send only a fraction of the generated telemetry.</span></span> [<span data-ttu-id="15a12-151">További tudnivalók a mintavételezésről.</span><span class="sxs-lookup"><span data-stu-id="15a12-151">Learn more about sampling.</span></span>](app-insights-sampling.md)
>
>

## <span data-ttu-id="15a12-152"><a name="get"></a>Vizsgálja meg az adatok</span><span class="sxs-lookup"><span data-stu-id="15a12-152"><a name="get"></a> Inspect the data</span></span>
<span data-ttu-id="15a12-153">A tárolási közvetlenül a portálon vizsgálhatja meg.</span><span class="sxs-lookup"><span data-stu-id="15a12-153">You can inspect the storage directly in the portal.</span></span> <span data-ttu-id="15a12-154">Kattintson a **Tallózás**, válassza ki a tárfiók, és nyisson **tárolók**.</span><span class="sxs-lookup"><span data-stu-id="15a12-154">Click **Browse**, select your storage account, and then open **Containers**.</span></span>

<span data-ttu-id="15a12-155">Vizsgálja meg az Azure storage a Visual Studio, nyissa meg a **nézet**, **Cloud Explorer**.</span><span class="sxs-lookup"><span data-stu-id="15a12-155">To inspect Azure storage in Visual Studio, open **View**, **Cloud Explorer**.</span></span> <span data-ttu-id="15a12-156">(Ha még nem rendelkezik az adott parancs, szeretné-e az Azure SDK telepítése: Nyissa meg a **új projekt** párbeszédpanelen bontsa ki a Visual C# / felhőalapú, és válassza **Microsoft Azure SDK for .NET**.)</span><span class="sxs-lookup"><span data-stu-id="15a12-156">(If you don't have that menu command, you need to install the Azure SDK: Open the **New Project** dialog, expand Visual C#/Cloud and choose **Get Microsoft Azure SDK for .NET**.)</span></span>

<span data-ttu-id="15a12-157">A blob-tároló megnyitásakor látni fogja a blob-fájlokat a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="15a12-157">When you open your blob store, you'll see a container with a set of blob files.</span></span> <span data-ttu-id="15a12-158">Az egyes fájlok származtatva a Application Insights-erőforrás nevét, a instrumentation kulcs, a telemetria-típus vagy dátum/idő URI.</span><span class="sxs-lookup"><span data-stu-id="15a12-158">The URI of each file derived from your Application Insights resource name, its instrumentation key, telemetry-type/date/time.</span></span> <span data-ttu-id="15a12-159">(Az erőforrás neve kisbetű szerepelhet, és a instrumentation kulcs kihagyja a kötőjel.)</span><span class="sxs-lookup"><span data-stu-id="15a12-159">(The resource name is all lowercase, and the instrumentation key omits dashes.)</span></span>

![Vizsgálja meg a megfelelő eszköz blob tárolóba](./media/app-insights-export-telemetry/04-data.png)

<span data-ttu-id="15a12-161">A dátum és idő UTC és amikor a telemetria - tárolójában lett letétbe nem hozta létre idejét.</span><span class="sxs-lookup"><span data-stu-id="15a12-161">The date and time are UTC and are when the telemetry was deposited in the store - not the time it was generated.</span></span> <span data-ttu-id="15a12-162">Így ha az adatok letöltése kódot ír, áthelyezheti azt lineárisan keresztül az adatok.</span><span class="sxs-lookup"><span data-stu-id="15a12-162">So if you write code to download the data, it can move linearly through the data.</span></span>

<span data-ttu-id="15a12-163">Az elérési utat a következő:</span><span class="sxs-lookup"><span data-stu-id="15a12-163">Here's the form of the path:</span></span>

    $"{applicationName}_{instrumentationKey}/{type}/{blobDeliveryTimeUtc:yyyy-MM-dd}/{ blobDeliveryTimeUtc:HH}/{blobId}_{blobCreationTimeUtc:yyyyMMdd_HHmmss}.blob"

<span data-ttu-id="15a12-164">Ha</span><span class="sxs-lookup"><span data-stu-id="15a12-164">Where</span></span>

* <span data-ttu-id="15a12-165">`blobCreationTimeUtc`van a belső blob létrehozásának időpontja átmeneti tárolási</span><span class="sxs-lookup"><span data-stu-id="15a12-165">`blobCreationTimeUtc` is time when blob was created in the internal staging storage</span></span>
* <span data-ttu-id="15a12-166">`blobDeliveryTimeUtc`az az idő, amikor blob lett másolva a exportálási cél</span><span class="sxs-lookup"><span data-stu-id="15a12-166">`blobDeliveryTimeUtc` is the time when blob is copied to the export destination storage</span></span>

## <span data-ttu-id="15a12-167"><a name="format"></a>Az adatformátum</span><span class="sxs-lookup"><span data-stu-id="15a12-167"><a name="format"></a> Data format</span></span>
* <span data-ttu-id="15a12-168">Minden egyes blob egy szövegfájlt, amely több tartalmazza az "\n'-separated sorokat.</span><span class="sxs-lookup"><span data-stu-id="15a12-168">Each blob is a text file that contains multiple '\n'-separated rows.</span></span> <span data-ttu-id="15a12-169">A telemetriai adatok feldolgozása körülbelül fél perc időszakra tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="15a12-169">It contains the telemetry processed over a time period of roughly half a minute.</span></span>
* <span data-ttu-id="15a12-170">Minden egyes sorára például egy kérelem vagy a lap nézet telemetriai adatpont jelöli.</span><span class="sxs-lookup"><span data-stu-id="15a12-170">Each row represents a telemetry data point such as a request or page view.</span></span>
* <span data-ttu-id="15a12-171">Minden egyes sorára nem formázott JSON-dokumentumhoz.</span><span class="sxs-lookup"><span data-stu-id="15a12-171">Each row is an unformatted JSON document.</span></span> <span data-ttu-id="15a12-172">Elhelyezkedik, és azt a stare, nyissa meg a Visual Studio és válassza a Szerkesztés, speciális formátumú fájlba:</span><span class="sxs-lookup"><span data-stu-id="15a12-172">If you want to sit and stare at it, open it in Visual Studio and choose Edit, Advanced, Format File:</span></span>

![A megfelelő eszköz telemetriai adatok megtekintése](./media/app-insights-export-telemetry/06-json.png)

<span data-ttu-id="15a12-174">Idő időtartamok vannak ütés, ahol a 10 000-re ölések = 1 MS.</span><span class="sxs-lookup"><span data-stu-id="15a12-174">Time durations are in ticks, where 10 000 ticks = 1ms.</span></span> <span data-ttu-id="15a12-175">Ezek az értékek például a kérést küldeni a böngészőből, 3ms azt, és a lap a böngészőben feldolgozni 1.8s 1 MS idő megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="15a12-175">For example, these values show a time of 1ms to send a request from the browser, 3ms to receive it, and 1.8s to process the page in the browser:</span></span>

    "sendRequest": {"value": 10000.0},
    "receiveRequest": {"value": 30000.0},
    "clientProcess": {"value": 17970000.0}

[<span data-ttu-id="15a12-176">A részletes adatok modell útmutató a tulajdonság típusát és értékét.</span><span class="sxs-lookup"><span data-stu-id="15a12-176">Detailed data model reference for the property types and values.</span></span>](app-insights-export-data-model.md)

## <a name="processing-the-data"></a><span data-ttu-id="15a12-177">Az adatok feldolgozása</span><span class="sxs-lookup"><span data-stu-id="15a12-177">Processing the data</span></span>
<span data-ttu-id="15a12-178">Egy kis méretű írhat néhány kódot egymástól az adatok lekérésére, olvassa el, egy számolótáblába, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="15a12-178">On a small scale, you can write some code to pull apart your data, read it into a spreadsheet, and so on.</span></span> <span data-ttu-id="15a12-179">Példa:</span><span class="sxs-lookup"><span data-stu-id="15a12-179">For example:</span></span>

    private IEnumerable<T> DeserializeMany<T>(string folderName)
    {
      var files = Directory.EnumerateFiles(folderName, "*.blob", SearchOption.AllDirectories);
      foreach (var file in files)
      {
         using (var fileReader = File.OpenText(file))
         {
            string fileContent = fileReader.ReadToEnd();
            IEnumerable<string> entities = fileContent.Split('\n').Where(s => !string.IsNullOrWhiteSpace(s));
            foreach (var entity in entities)
            {
                yield return JsonConvert.DeserializeObject<T>(entity);
            }
         }
      }
    }

<span data-ttu-id="15a12-180">A nagyobb kódminta, lásd: [feldolgozói szerepkörök használatával][exportasa].</span><span class="sxs-lookup"><span data-stu-id="15a12-180">For a larger code sample, see [using a worker role][exportasa].</span></span>

## <span data-ttu-id="15a12-181"><a name="delete"></a>A régi adatok törlése</span><span class="sxs-lookup"><span data-stu-id="15a12-181"><a name="delete"></a>Delete your old data</span></span>
<span data-ttu-id="15a12-182">Vegye figyelembe, hogy Ön felelősséggel tartozik a tárolási kapacitás kezelése és a régi adatok törlése, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="15a12-182">Please note that you are responsible for managing your storage capacity and deleting the old data if necessary.</span></span>

## <a name="if-you-regenerate-your-storage-key"></a><span data-ttu-id="15a12-183">Ha a tárolási kulcs újragenerálása...</span><span class="sxs-lookup"><span data-stu-id="15a12-183">If you regenerate your storage key...</span></span>
<span data-ttu-id="15a12-184">Ha módosítja a kulcsot a tároló, a folyamatos exportálás nem fog tovább működni.</span><span class="sxs-lookup"><span data-stu-id="15a12-184">If you change the key to your storage, continuous export will stop working.</span></span> <span data-ttu-id="15a12-185">Az Azure-fiókjába, megjelenik egy értesítés.</span><span class="sxs-lookup"><span data-stu-id="15a12-185">You'll see a notification in your Azure account.</span></span>

<span data-ttu-id="15a12-186">A folyamatos exportálás panel megnyitásához, és az Exportálás szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="15a12-186">Open the Continuous Export blade and edit your export.</span></span> <span data-ttu-id="15a12-187">Az Exportálás cél szerkesztése, de ne változtassa meg a kiválasztott ugyanazt a tárhelyet.</span><span class="sxs-lookup"><span data-stu-id="15a12-187">Edit the Export Destination, but just leave the same storage selected.</span></span> <span data-ttu-id="15a12-188">Kattintson az OK gombra a megerősítéshez.</span><span class="sxs-lookup"><span data-stu-id="15a12-188">Click OK to confirm.</span></span>

![Szerkesztés a folyamatos exportálás, és ezekkel a cél exportálása.](./media/app-insights-export-telemetry/07-resetstore.png)

<span data-ttu-id="15a12-190">A folyamatos exportálás újraindul.</span><span class="sxs-lookup"><span data-stu-id="15a12-190">The continuous export will restart.</span></span>

## <a name="export-samples"></a><span data-ttu-id="15a12-191">Exportálás minták</span><span class="sxs-lookup"><span data-stu-id="15a12-191">Export samples</span></span>

* <span data-ttu-id="15a12-192">[A Stream Analytics használ SQL exportálása][exportasa]</span><span class="sxs-lookup"><span data-stu-id="15a12-192">[Export to SQL using Stream Analytics][exportasa]</span></span>
* [<span data-ttu-id="15a12-193">Stream Analytics minta 2</span><span class="sxs-lookup"><span data-stu-id="15a12-193">Stream Analytics sample 2</span></span>](app-insights-export-stream-analytics.md)

<span data-ttu-id="15a12-194">Nagyobb skálán, érdemes lehet [HDInsight](https://azure.microsoft.com/services/hdinsight/) -Hadoop-fürtök a felhőben.</span><span class="sxs-lookup"><span data-stu-id="15a12-194">On larger scales, consider [HDInsight](https://azure.microsoft.com/services/hdinsight/) - Hadoop clusters in the cloud.</span></span> <span data-ttu-id="15a12-195">HDInsight számos technológiák kezelése, és a big Data típusú adatok elemzésére, és használhat fel az Application Insights exportált adatokat.</span><span class="sxs-lookup"><span data-stu-id="15a12-195">HDInsight provides a variety of technologies for managing and analyzing big data, and you could use it to process data that has been exported from Application Insights.</span></span>

## <a name="q--a"></a><span data-ttu-id="15a12-196">Kérdések és válaszok</span><span class="sxs-lookup"><span data-stu-id="15a12-196">Q & A</span></span>
* <span data-ttu-id="15a12-197">*Azonban az összes kívánt egyszeri letölteni egy diagram.*</span><span class="sxs-lookup"><span data-stu-id="15a12-197">*But all I want is a one-time download of a chart.*</span></span>  

    <span data-ttu-id="15a12-198">Igen, akkor teheti meg.</span><span class="sxs-lookup"><span data-stu-id="15a12-198">Yes, you can do that.</span></span> <span data-ttu-id="15a12-199">Kattintson a panel tetején **adatok exportálása**.</span><span class="sxs-lookup"><span data-stu-id="15a12-199">At the top of the blade, click **Export Data**.</span></span>
* <span data-ttu-id="15a12-200">*Exportálás beállítva, de nincs adat a saját tárolóban.*</span><span class="sxs-lookup"><span data-stu-id="15a12-200">*I set up an export, but there's no data in my store.*</span></span>

    <span data-ttu-id="15a12-201">Az Application Insights kapta meg összes telemetriai adat az alkalmazás óta állít be az Exportálás?</span><span class="sxs-lookup"><span data-stu-id="15a12-201">Did Application Insights receive any telemetry from your app since you set up the export?</span></span> <span data-ttu-id="15a12-202">Csak kap új adatokat.</span><span class="sxs-lookup"><span data-stu-id="15a12-202">You'll only receive new data.</span></span>
* <span data-ttu-id="15a12-203">*I exportálás beállítása próbált, de a rendszer megtagadta a hozzáférést*</span><span class="sxs-lookup"><span data-stu-id="15a12-203">*I tried to set up an export, but was denied access*</span></span>

    <span data-ttu-id="15a12-204">Ha a fiók a szervezete tulajdonában van, akkor a tulajdonos vagy közreműködő szerepkörrel rendelkező személyek csoportok tagjai.</span><span class="sxs-lookup"><span data-stu-id="15a12-204">If the account is owned by your organization, you have to be a member of the owners or contributors groups.</span></span>
* <span data-ttu-id="15a12-205">*Is exportálásakor rögtön saját helyszíni tárolási?*</span><span class="sxs-lookup"><span data-stu-id="15a12-205">*Can I export straight to my own on-premises store?*</span></span>

    <span data-ttu-id="15a12-206">Nem, sajnos.</span><span class="sxs-lookup"><span data-stu-id="15a12-206">No, sorry.</span></span> <span data-ttu-id="15a12-207">Az Exportálás motor jelenleg csak akkor működik az Azure storage most.</span><span class="sxs-lookup"><span data-stu-id="15a12-207">Our export engine currently only works with Azure storage at this time.</span></span>  
* <span data-ttu-id="15a12-208">*A saját tárolóban lévő adatok bármely korlátozva van?*</span><span class="sxs-lookup"><span data-stu-id="15a12-208">*Is there any limit to the amount of data you put in my store?*</span></span>

    <span data-ttu-id="15a12-209">Nem.</span><span class="sxs-lookup"><span data-stu-id="15a12-209">No.</span></span> <span data-ttu-id="15a12-210">Azt fogja tartsa kérdez le adatokat exportálás törléséig.</span><span class="sxs-lookup"><span data-stu-id="15a12-210">We'll keep pushing data in until you delete the export.</span></span> <span data-ttu-id="15a12-211">Ha azt a külső korlátozhatja a blob-tároló találati, de ez közérthető hatalmas lesz leállítása.</span><span class="sxs-lookup"><span data-stu-id="15a12-211">We'll stop if we hit the outer limits for blob storage, but that's pretty huge.</span></span> <span data-ttu-id="15a12-212">Ez legfeljebb lesz vezérlését mennyi tárhelyre, használja.</span><span class="sxs-lookup"><span data-stu-id="15a12-212">It's up to you to control how much storage you use.</span></span>  
* <span data-ttu-id="15a12-213">*Hány blobok kell lásd a tárolóban lévő?*</span><span class="sxs-lookup"><span data-stu-id="15a12-213">*How many blobs should I see in the storage?*</span></span>

  * <span data-ttu-id="15a12-214">Minden egyes exportálandó kiválasztott adattípus használatához új blob jön létre percenként (ha érhetők el adatok).</span><span class="sxs-lookup"><span data-stu-id="15a12-214">For every data type you selected to export, a new blob is created every minute (if data is available).</span></span>
  * <span data-ttu-id="15a12-215">Emellett nagy forgalmú alkalmazások esetén további partíció egységek foglal le.</span><span class="sxs-lookup"><span data-stu-id="15a12-215">In addition, for applications with high traffic, additional partition units are allocated.</span></span> <span data-ttu-id="15a12-216">Ebben az esetben tárolóegységekhez hoz létre egy blobot percenként.</span><span class="sxs-lookup"><span data-stu-id="15a12-216">In this case each unit creates a blob every minute.</span></span>
* <span data-ttu-id="15a12-217">*Lehet, a kulcs újragenerálása a tárhely, vagy a tároló neve megváltozott, és az exportálás nem működik.*</span><span class="sxs-lookup"><span data-stu-id="15a12-217">*I regenerated the key to my storage or changed the name of the container, and now the export doesn't work.*</span></span>

    <span data-ttu-id="15a12-218">Az Exportálás módosításával, és nyissa meg az Exportálás cél panelt.</span><span class="sxs-lookup"><span data-stu-id="15a12-218">Edit the export and open the export destination blade.</span></span> <span data-ttu-id="15a12-219">Hagyja a választott azt korábban ugyanazt a tárhelyet, és kattintson az OK gombra a megerősítéshez.</span><span class="sxs-lookup"><span data-stu-id="15a12-219">Leave the same storage selected as before, and click OK to confirm.</span></span> <span data-ttu-id="15a12-220">Exportálás újraindul.</span><span class="sxs-lookup"><span data-stu-id="15a12-220">Export will restart.</span></span> <span data-ttu-id="15a12-221">Ha módosította az elmúlt néhány napon belül, adatok nem vesznek el.</span><span class="sxs-lookup"><span data-stu-id="15a12-221">If the change was within the past few days, you won't lose data.</span></span>
* <span data-ttu-id="15a12-222">*Az exportálás is szüneteltetése?*</span><span class="sxs-lookup"><span data-stu-id="15a12-222">*Can I pause the export?*</span></span>

    <span data-ttu-id="15a12-223">Igen.</span><span class="sxs-lookup"><span data-stu-id="15a12-223">Yes.</span></span> <span data-ttu-id="15a12-224">Kattintson a letiltása.</span><span class="sxs-lookup"><span data-stu-id="15a12-224">Click Disable.</span></span>

## <a name="code-samples"></a><span data-ttu-id="15a12-225">Kódminták</span><span class="sxs-lookup"><span data-stu-id="15a12-225">Code samples</span></span>

* [<span data-ttu-id="15a12-226">Stream Analytics-minta</span><span class="sxs-lookup"><span data-stu-id="15a12-226">Stream Analytics sample</span></span>](app-insights-export-stream-analytics.md)
* <span data-ttu-id="15a12-227">[A Stream Analytics használ SQL exportálása][exportasa]</span><span class="sxs-lookup"><span data-stu-id="15a12-227">[Export to SQL using Stream Analytics][exportasa]</span></span>
* [<span data-ttu-id="15a12-228">A részletes adatok modell útmutató a tulajdonság típusát és értékét.</span><span class="sxs-lookup"><span data-stu-id="15a12-228">Detailed data model reference for the property types and values.</span></span>](app-insights-export-data-model.md)

<!--Link references-->

[exportasa]: app-insights-code-sample-export-sql-stream-analytics.md
[roles]: app-insights-resources-roles-access-control.md
