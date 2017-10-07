---
title: "az Application Insights telemetria aaaContinuous exportálása |} Microsoft Docs"
description: "Exportálja a diagnosztikai és használati adatok toostorage a Microsoft Azure-ban, és töltse le innen."
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
ms.openlocfilehash: be9ed7e05922c1c8186df9ca4e642862adaa5fd0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="export-telemetry-from-application-insights"></a><span data-ttu-id="c8337-103">Az Application Insights telemetria exportálása</span><span class="sxs-lookup"><span data-stu-id="c8337-103">Export telemetry from Application Insights</span></span>
<span data-ttu-id="c8337-104">Szeretné, hogy tookeep a telemetriai adatok hello szabványos megőrzési időszaknál hosszabb ideig?</span><span class="sxs-lookup"><span data-stu-id="c8337-104">Want tookeep your telemetry for longer than hello standard retention period?</span></span> <span data-ttu-id="c8337-105">Vagy valamilyen speciális módon dolgozza fel?</span><span class="sxs-lookup"><span data-stu-id="c8337-105">Or process it in some specialized way?</span></span> <span data-ttu-id="c8337-106">A folyamatos exportálás ideális ehhez.</span><span class="sxs-lookup"><span data-stu-id="c8337-106">Continuous Export is ideal for this.</span></span> <span data-ttu-id="c8337-107">megjelenik a hello Application Insights portál hello események JSON formátumban a Microsoft Azure exportált toostorage lehet.</span><span class="sxs-lookup"><span data-stu-id="c8337-107">hello events you see in hello Application Insights portal can be exported toostorage in Microsoft Azure in JSON format.</span></span> <span data-ttu-id="c8337-108">Ott töltse le az adatok és írási függetlenül kódját, tooprocess kell azt.</span><span class="sxs-lookup"><span data-stu-id="c8337-108">From there you can download your data and write whatever code you need tooprocess it.</span></span>  

<span data-ttu-id="c8337-109">Használja a folyamatos exportálás járhat egy kell külön fizetni.</span><span class="sxs-lookup"><span data-stu-id="c8337-109">Using Continuous Export may incur an additional charge.</span></span> <span data-ttu-id="c8337-110">Ellenőrizze a [fizetési modell](http://azure.microsoft.com/pricing/details/application-insights/).</span><span class="sxs-lookup"><span data-stu-id="c8337-110">Check your [pricing model](http://azure.microsoft.com/pricing/details/application-insights/).</span></span>

<span data-ttu-id="c8337-111">A folyamatos exportálás beállítása előtt van néhány más-érdemes tooconsider:</span><span class="sxs-lookup"><span data-stu-id="c8337-111">Before you set up continuous export, there are some alternatives you might want tooconsider:</span></span>

* <span data-ttu-id="c8337-112">egy metrikák vagy keresési panel felső hello hello Exportálás gomb lehetővé teszi átviteli táblázatokat és diagramokat tooan Excel-táblázat.</span><span class="sxs-lookup"><span data-stu-id="c8337-112">hello Export button at hello top of a metrics or search blade lets you transfer tables and charts tooan Excel spreadsheet.</span></span>

* <span data-ttu-id="c8337-113">[Elemzés](app-insights-analytics.md) hatékony lekérdezésnyelvet biztosít a telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="c8337-113">[Analytics](app-insights-analytics.md) provides a powerful query language for telemetry.</span></span> <span data-ttu-id="c8337-114">Exportálhatja eredmények.</span><span class="sxs-lookup"><span data-stu-id="c8337-114">It can also export results.</span></span>
* <span data-ttu-id="c8337-115">Ha túl[az adatokba a Power BI](app-insights-export-power-bi.md), azt teheti a folyamatos exportálás használata nélkül.</span><span class="sxs-lookup"><span data-stu-id="c8337-115">If you're looking too[explore your data in Power BI](app-insights-export-power-bi.md), you can do that without using Continuous Export.</span></span>
* <span data-ttu-id="c8337-116">Hello [adatelérési REST API](https://dev.applicationinsights.io/) programozott hozzáférést a telemetriai adatok.</span><span class="sxs-lookup"><span data-stu-id="c8337-116">hello [Data access REST API](https://dev.applicationinsights.io/) lets you access your telemetry programmatically.</span></span>

<span data-ttu-id="c8337-117">A folyamatos exportálás nem másolta az adatokat toostorage (amennyiben azt is marad mindaddig, amíg tetszés), akkor továbbra is használható az Application Insightsban szokásos hello [megőrzési időszak](app-insights-data-retention-privacy.md).</span><span class="sxs-lookup"><span data-stu-id="c8337-117">After Continuous Export copies your data toostorage (where it can stay for as long as you like), it's still available in Application Insights for hello usual [retention period](app-insights-data-retention-privacy.md).</span></span>

## <span data-ttu-id="c8337-118"><a name="setup"></a>A folyamatos exportálás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8337-118"><a name="setup"></a> Create a Continuous Export</span></span>
1. <span data-ttu-id="c8337-119">A hello Application Insights-erőforrást az alkalmazáshoz, nyissa meg a folyamatos exportálás, és válassza **Hozzáadás**:</span><span class="sxs-lookup"><span data-stu-id="c8337-119">In hello Application Insights resource for your app, open Continuous Export and choose **Add**:</span></span>

    ![Görgessen le, majd kattintson a folyamatos exportálás](./media/app-insights-export-telemetry/01-export.png)

2. <span data-ttu-id="c8337-121">Válassza ki a hello telemetriai adattípusok tooexport szeretné.</span><span class="sxs-lookup"><span data-stu-id="c8337-121">Choose hello telemetry data types you want tooexport.</span></span>

3. <span data-ttu-id="c8337-122">Hozzon létre vagy válasszon egy [Azure storage-fiók](../storage/common/storage-introduction.md) ahová toostore hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="c8337-122">Create or select an [Azure storage account](../storage/common/storage-introduction.md) where you want toostore hello data.</span></span>

    > [!Warning]
    > <span data-ttu-id="c8337-123">Alapértelmezés szerint hello tárolási helye lesz beállítva, toohello és az Application Insights-erőforrás ugyanabban a földrajzi régióban.</span><span class="sxs-lookup"><span data-stu-id="c8337-123">By default, hello storage location will be set toohello same geographical region as your Application Insights resource.</span></span> <span data-ttu-id="c8337-124">Ha egy másik régióban vannak tárolva, az átviteli további költségekkel járhat.</span><span class="sxs-lookup"><span data-stu-id="c8337-124">If you store in a different region, you may incur transfer charges.</span></span>

    ![Kattintson a Hozzáadás gombra, exportálás cél tárfiók, és majd létrehoz egy új vagy egy meglévő tároló kiválasztása](./media/app-insights-export-telemetry/02-add.png)

4. <span data-ttu-id="c8337-126">Hozzon létre, vagy válasszon egy tároló hello tárolóban:</span><span class="sxs-lookup"><span data-stu-id="c8337-126">Create or select a container in hello storage:</span></span>

    ![Válasszon eseménytípust kattintson](./media/app-insights-export-telemetry/create-container.png)

<span data-ttu-id="c8337-128">Ha létrehozta az exportálás, fog kezdődik.</span><span class="sxs-lookup"><span data-stu-id="c8337-128">Once you've created your export, it starts going.</span></span> <span data-ttu-id="c8337-129">Csak get hello exportálási létrehozása után érkező adatokat.</span><span class="sxs-lookup"><span data-stu-id="c8337-129">You only get data that arrives after you create hello export.</span></span>

<span data-ttu-id="c8337-130">Körülbelül egy óra alatt jelenik meg adat hello tárolási előtt késéssel is járhat.</span><span class="sxs-lookup"><span data-stu-id="c8337-130">There can be a delay of about an hour before data appears in hello storage.</span></span>

### <a name="tooedit-continuous-export"></a><span data-ttu-id="c8337-131">a folyamatos exportálás tooedit</span><span class="sxs-lookup"><span data-stu-id="c8337-131">tooedit continuous export</span></span>

<span data-ttu-id="c8337-132">Ha azt szeretné, hogy toochange hello eseménytípusok később, hello exportálás módosításával:</span><span class="sxs-lookup"><span data-stu-id="c8337-132">If you want toochange hello event types later, just edit hello export:</span></span>

![Válasszon eseménytípust kattintson](./media/app-insights-export-telemetry/05-edit.png)

### <a name="toostop-continuous-export"></a><span data-ttu-id="c8337-134">a folyamatos exportálás toostop</span><span class="sxs-lookup"><span data-stu-id="c8337-134">toostop continuous export</span></span>

<span data-ttu-id="c8337-135">toostop hello exportálás kattintson letiltása.</span><span class="sxs-lookup"><span data-stu-id="c8337-135">toostop hello export, click Disable.</span></span> <span data-ttu-id="c8337-136">Engedélyezze újra gombra, a rendszer újraindítja a hello exportálási új adatokkal.</span><span class="sxs-lookup"><span data-stu-id="c8337-136">When you click Enable again, hello export will restart with new data.</span></span> <span data-ttu-id="c8337-137">Hello adatok érkező hello portálon exportálás le lett tiltva, amíg nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="c8337-137">You won't get hello data that arrived in hello portal while export was disabled.</span></span>

<span data-ttu-id="c8337-138">toostop hello exportálási véglegesen, törölje a parancsikont.</span><span class="sxs-lookup"><span data-stu-id="c8337-138">toostop hello export permanently, delete it.</span></span> <span data-ttu-id="c8337-139">Így nem törlése az adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="c8337-139">Doing so doesn't delete your data from storage.</span></span>

### <a name="cant-add-or-change-an-export"></a><span data-ttu-id="c8337-140">Nem adható hozzá, vagy módosítsa az export?</span><span class="sxs-lookup"><span data-stu-id="c8337-140">Can't add or change an export?</span></span>
* <span data-ttu-id="c8337-141">tooadd, vagy módosítsa azokat exportálja, tulajdonos, közreműködő vagy Application Insights közreműködői hozzáférési jogosultságok szükségesek.</span><span class="sxs-lookup"><span data-stu-id="c8337-141">tooadd or change exports, you need Owner, Contributor or Application Insights Contributor access rights.</span></span> <span data-ttu-id="c8337-142">[További tudnivalók a szerepkörök][roles].</span><span class="sxs-lookup"><span data-stu-id="c8337-142">[Learn about roles][roles].</span></span>

## <span data-ttu-id="c8337-143"><a name="analyze"></a>Milyen eseményeket kapott?</span><span class="sxs-lookup"><span data-stu-id="c8337-143"><a name="analyze"></a> What events do you get?</span></span>
<span data-ttu-id="c8337-144">hello exportált adatok hello nyers telemetriaadatok az alkalmazásból érkező azzal a különbséggel, hogy a jelenleg a helyadatok, amelyek Microsoft hello ügyfél IP-címről érkező felvenni.</span><span class="sxs-lookup"><span data-stu-id="c8337-144">hello exported data is hello raw telemetry we receive from your application, except that we add location data which we calculate from hello client IP address.</span></span>

<span data-ttu-id="c8337-145">Adatok, amelyek szerint el lett vetve [mintavételi](app-insights-sampling.md) hello exportált adatok nem szerepel.</span><span class="sxs-lookup"><span data-stu-id="c8337-145">Data that has been discarded by [sampling](app-insights-sampling.md) is not included in hello exported data.</span></span>

<span data-ttu-id="c8337-146">Más számított metrikák nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="c8337-146">Other calculated metrics are not included.</span></span> <span data-ttu-id="c8337-147">Például azt átlagos CPU-felhasználása ne exportálja, de exportálása hello nyers telemetriaadatok, amelyből hello átlagának kiszámítása történik.</span><span class="sxs-lookup"><span data-stu-id="c8337-147">For example, we don't export average CPU utilisation, but we do export hello raw telemetry from which hello average is computed.</span></span>

<span data-ttu-id="c8337-148">hello adatokat is tartalmaz hello eredmények bármely [webteszt rendelkezésre állási](app-insights-monitor-web-app-availability.md) , amely meg van adva.</span><span class="sxs-lookup"><span data-stu-id="c8337-148">hello data also includes hello results of any [availability web tests](app-insights-monitor-web-app-availability.md) that you have set up.</span></span>

> [!NOTE]
> <span data-ttu-id="c8337-149">**Mintavételi.**</span><span class="sxs-lookup"><span data-stu-id="c8337-149">**Sampling.**</span></span> <span data-ttu-id="c8337-150">Az alkalmazás nagy mennyiségű adatot küld, ha hello mintavételi szolgáltatás működik, és csak töredéke alatt létrehozott hello telemetriai adatot küldeni.</span><span class="sxs-lookup"><span data-stu-id="c8337-150">If your application sends a lot of data, hello sampling feature may operate and send only a fraction of hello generated telemetry.</span></span> [<span data-ttu-id="c8337-151">További tudnivalók a mintavételezésről.</span><span class="sxs-lookup"><span data-stu-id="c8337-151">Learn more about sampling.</span></span>](app-insights-sampling.md)
>
>

## <span data-ttu-id="c8337-152"><a name="get"></a>Hello adatok vizsgálata</span><span class="sxs-lookup"><span data-stu-id="c8337-152"><a name="get"></a> Inspect hello data</span></span>
<span data-ttu-id="c8337-153">Közvetlenül a hello portál hello tárolási vizsgálhatja meg.</span><span class="sxs-lookup"><span data-stu-id="c8337-153">You can inspect hello storage directly in hello portal.</span></span> <span data-ttu-id="c8337-154">Kattintson a **Tallózás**, válassza ki a tárfiók, és nyisson **tárolók**.</span><span class="sxs-lookup"><span data-stu-id="c8337-154">Click **Browse**, select your storage account, and then open **Containers**.</span></span>

<span data-ttu-id="c8337-155">Nyissa meg a Visual studióban, az Azure storage tooinspect **nézet**, **Cloud Explorer**.</span><span class="sxs-lookup"><span data-stu-id="c8337-155">tooinspect Azure storage in Visual Studio, open **View**, **Cloud Explorer**.</span></span> <span data-ttu-id="c8337-156">(Még nem rendelkezik az adott parancs, ha szüksége van-e tooinstall hello Azure SDK: nyitott hello **új projekt** párbeszédpanelen bontsa ki a Visual C# / felhőalapú, és válassza **Microsoft Azure SDK for .NET**.)</span><span class="sxs-lookup"><span data-stu-id="c8337-156">(If you don't have that menu command, you need tooinstall hello Azure SDK: Open hello **New Project** dialog, expand Visual C#/Cloud and choose **Get Microsoft Azure SDK for .NET**.)</span></span>

<span data-ttu-id="c8337-157">A blob-tároló megnyitásakor látni fogja a blob-fájlokat a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="c8337-157">When you open your blob store, you'll see a container with a set of blob files.</span></span> <span data-ttu-id="c8337-158">az egyes fájlok URI hello az Application Insights-erőforrás nevét, a instrumentation kulcs, a telemetria-típus vagy dátum/idő származik.</span><span class="sxs-lookup"><span data-stu-id="c8337-158">hello URI of each file derived from your Application Insights resource name, its instrumentation key, telemetry-type/date/time.</span></span> <span data-ttu-id="c8337-159">(hello erőforrásnév kisbetű szerepelhet, és hello instrumentation kulcs kihagyja a kötőjel.)</span><span class="sxs-lookup"><span data-stu-id="c8337-159">(hello resource name is all lowercase, and hello instrumentation key omits dashes.)</span></span>

![Vizsgálja meg a megfelelő eszköz hello blob tároló](./media/app-insights-export-telemetry/04-data.png)

<span data-ttu-id="c8337-161">hello dátuma és időpontja (UTC) és amikor hello telemetriai lett letétbe hello tárolójában – hello idő nem hozta létre.</span><span class="sxs-lookup"><span data-stu-id="c8337-161">hello date and time are UTC and are when hello telemetry was deposited in hello store - not hello time it was generated.</span></span> <span data-ttu-id="c8337-162">Így ha kód toodownload hello adatok írása, áthelyezheti azt lineárisan hello adatok között.</span><span class="sxs-lookup"><span data-stu-id="c8337-162">So if you write code toodownload hello data, it can move linearly through hello data.</span></span>

<span data-ttu-id="c8337-163">Hello űrlap hello elérési út a következő:</span><span class="sxs-lookup"><span data-stu-id="c8337-163">Here's hello form of hello path:</span></span>

    $"{applicationName}_{instrumentationKey}/{type}/{blobDeliveryTimeUtc:yyyy-MM-dd}/{ blobDeliveryTimeUtc:HH}/{blobId}_{blobCreationTimeUtc:yyyyMMdd_HHmmss}.blob"

<span data-ttu-id="c8337-164">Ha</span><span class="sxs-lookup"><span data-stu-id="c8337-164">Where</span></span>

* <span data-ttu-id="c8337-165">`blobCreationTimeUtc`van blob létrehozásának időpontja a hello belső átmeneti tárolási</span><span class="sxs-lookup"><span data-stu-id="c8337-165">`blobCreationTimeUtc` is time when blob was created in hello internal staging storage</span></span>
* <span data-ttu-id="c8337-166">`blobDeliveryTimeUtc`hello időpontot, amikor blob másolt toohello exportálási destination tároló van</span><span class="sxs-lookup"><span data-stu-id="c8337-166">`blobDeliveryTimeUtc` is hello time when blob is copied toohello export destination storage</span></span>

## <span data-ttu-id="c8337-167"><a name="format"></a>Az adatformátum</span><span class="sxs-lookup"><span data-stu-id="c8337-167"><a name="format"></a> Data format</span></span>
* <span data-ttu-id="c8337-168">Minden egyes blob egy szövegfájlt, amely több tartalmazza az "\n'-separated sorokat.</span><span class="sxs-lookup"><span data-stu-id="c8337-168">Each blob is a text file that contains multiple '\n'-separated rows.</span></span> <span data-ttu-id="c8337-169">Körülbelül fél perc időszakra feldolgozott hello telemetriai adatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c8337-169">It contains hello telemetry processed over a time period of roughly half a minute.</span></span>
* <span data-ttu-id="c8337-170">Minden egyes sorára például egy kérelem vagy a lap nézet telemetriai adatpont jelöli.</span><span class="sxs-lookup"><span data-stu-id="c8337-170">Each row represents a telemetry data point such as a request or page view.</span></span>
* <span data-ttu-id="c8337-171">Minden egyes sorára nem formázott JSON-dokumentumhoz.</span><span class="sxs-lookup"><span data-stu-id="c8337-171">Each row is an unformatted JSON document.</span></span> <span data-ttu-id="c8337-172">Ha toosit és, hogy stare, nyissa meg a Visual Studio és az válassza szerkesztése, a speciális formátumú fájlba:</span><span class="sxs-lookup"><span data-stu-id="c8337-172">If you want toosit and stare at it, open it in Visual Studio and choose Edit, Advanced, Format File:</span></span>

![Nézet hello telemetriai megfelelő eszközzel](./media/app-insights-export-telemetry/06-json.png)

<span data-ttu-id="c8337-174">Idő időtartamok vannak ütés, ahol a 10 000-re ölések = 1 MS.</span><span class="sxs-lookup"><span data-stu-id="c8337-174">Time durations are in ticks, where 10 000 ticks = 1ms.</span></span> <span data-ttu-id="c8337-175">Például ezek az értékek megjelenítése a idő 1 MS toosend hello böngészőből 3ms kérelmet tooreceive és 1.8s tooprocess hello lapon hello böngészőben:</span><span class="sxs-lookup"><span data-stu-id="c8337-175">For example, these values show a time of 1ms toosend a request from hello browser, 3ms tooreceive it, and 1.8s tooprocess hello page in hello browser:</span></span>

    "sendRequest": {"value": 10000.0},
    "receiveRequest": {"value": 30000.0},
    "clientProcess": {"value": 17970000.0}

[<span data-ttu-id="c8337-176">Részletes adatok modellhez tartozó referencia hello Tulajdonságtípusok és értékeket.</span><span class="sxs-lookup"><span data-stu-id="c8337-176">Detailed data model reference for hello property types and values.</span></span>](app-insights-export-data-model.md)

## <a name="processing-hello-data"></a><span data-ttu-id="c8337-177">Hello adatainak feldolgozása</span><span class="sxs-lookup"><span data-stu-id="c8337-177">Processing hello data</span></span>
<span data-ttu-id="c8337-178">Kis méretű az adatok írása az egyes kód toopull egymástól, olvassa el, egy számolótáblába, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="c8337-178">On a small scale, you can write some code toopull apart your data, read it into a spreadsheet, and so on.</span></span> <span data-ttu-id="c8337-179">Példa:</span><span class="sxs-lookup"><span data-stu-id="c8337-179">For example:</span></span>

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

<span data-ttu-id="c8337-180">A nagyobb kódminta, lásd: [feldolgozói szerepkörök használatával][exportasa].</span><span class="sxs-lookup"><span data-stu-id="c8337-180">For a larger code sample, see [using a worker role][exportasa].</span></span>

## <span data-ttu-id="c8337-181"><a name="delete"></a>A régi adatok törlése</span><span class="sxs-lookup"><span data-stu-id="c8337-181"><a name="delete"></a>Delete your old data</span></span>
<span data-ttu-id="c8337-182">Vegye figyelembe, hogy Ön felelősséggel tartozik a tárolási kapacitás kezelése és hello régi adatok törlése, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="c8337-182">Please note that you are responsible for managing your storage capacity and deleting hello old data if necessary.</span></span>

## <a name="if-you-regenerate-your-storage-key"></a><span data-ttu-id="c8337-183">Ha a tárolási kulcs újragenerálása...</span><span class="sxs-lookup"><span data-stu-id="c8337-183">If you regenerate your storage key...</span></span>
<span data-ttu-id="c8337-184">Hello kulcs tooyour tárolási módosítása esetén a folyamatos exportálás nem fog tovább működni.</span><span class="sxs-lookup"><span data-stu-id="c8337-184">If you change hello key tooyour storage, continuous export will stop working.</span></span> <span data-ttu-id="c8337-185">Az Azure-fiókjába, megjelenik egy értesítés.</span><span class="sxs-lookup"><span data-stu-id="c8337-185">You'll see a notification in your Azure account.</span></span>

<span data-ttu-id="c8337-186">Hello a folyamatos exportálás panel megnyitásához, és az Exportálás szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="c8337-186">Open hello Continuous Export blade and edit your export.</span></span> <span data-ttu-id="c8337-187">Hello exportálása cél szerkesztése, de ne változtassa meg ugyanazt a tárhelyet kijelölt hello.</span><span class="sxs-lookup"><span data-stu-id="c8337-187">Edit hello Export Destination, but just leave hello same storage selected.</span></span> <span data-ttu-id="c8337-188">Kattintson az OK tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="c8337-188">Click OK tooconfirm.</span></span>

![Szerkesztés hello folyamatos exportálása, nyissa meg, majd zárja be, ezekkel az exportálási cél.](./media/app-insights-export-telemetry/07-resetstore.png)

<span data-ttu-id="c8337-190">a folyamatos exportálás hello újraindul.</span><span class="sxs-lookup"><span data-stu-id="c8337-190">hello continuous export will restart.</span></span>

## <a name="export-samples"></a><span data-ttu-id="c8337-191">Exportálás minták</span><span class="sxs-lookup"><span data-stu-id="c8337-191">Export samples</span></span>

* <span data-ttu-id="c8337-192">[Használja a Stream Analytics tooSQL exportálása][exportasa]</span><span class="sxs-lookup"><span data-stu-id="c8337-192">[Export tooSQL using Stream Analytics][exportasa]</span></span>
* [<span data-ttu-id="c8337-193">Stream Analytics minta 2</span><span class="sxs-lookup"><span data-stu-id="c8337-193">Stream Analytics sample 2</span></span>](app-insights-export-stream-analytics.md)

<span data-ttu-id="c8337-194">Nagyobb skálán, érdemes lehet [HDInsight](https://azure.microsoft.com/services/hdinsight/) -Hadoop-fürtök hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="c8337-194">On larger scales, consider [HDInsight](https://azure.microsoft.com/services/hdinsight/) - Hadoop clusters in hello cloud.</span></span> <span data-ttu-id="c8337-195">HDInsight számos technológiák kezelése, és a big Data típusú adatok elemzésére, és használhatja azt az Application Insights exportált tooprocess adatokat.</span><span class="sxs-lookup"><span data-stu-id="c8337-195">HDInsight provides a variety of technologies for managing and analyzing big data, and you could use it tooprocess data that has been exported from Application Insights.</span></span>

## <a name="q--a"></a><span data-ttu-id="c8337-196">Kérdések és válaszok</span><span class="sxs-lookup"><span data-stu-id="c8337-196">Q & A</span></span>
* <span data-ttu-id="c8337-197">*Azonban az összes kívánt egyszeri letölteni egy diagram.*</span><span class="sxs-lookup"><span data-stu-id="c8337-197">*But all I want is a one-time download of a chart.*</span></span>  

    <span data-ttu-id="c8337-198">Igen, akkor teheti meg.</span><span class="sxs-lookup"><span data-stu-id="c8337-198">Yes, you can do that.</span></span> <span data-ttu-id="c8337-199">Hello hello panel felső részén kattintson **adatok exportálása**.</span><span class="sxs-lookup"><span data-stu-id="c8337-199">At hello top of hello blade, click **Export Data**.</span></span>
* <span data-ttu-id="c8337-200">*Exportálás beállítva, de nincs adat a saját tárolóban.*</span><span class="sxs-lookup"><span data-stu-id="c8337-200">*I set up an export, but there's no data in my store.*</span></span>

    <span data-ttu-id="c8337-201">Az Application Insights kapta meg összes telemetriai adat az alkalmazás óta hello exportálás beállítása?</span><span class="sxs-lookup"><span data-stu-id="c8337-201">Did Application Insights receive any telemetry from your app since you set up hello export?</span></span> <span data-ttu-id="c8337-202">Csak kap új adatokat.</span><span class="sxs-lookup"><span data-stu-id="c8337-202">You'll only receive new data.</span></span>
* <span data-ttu-id="c8337-203">*I tooset Exportálás mentése próbált, de a rendszer megtagadta a hozzáférést*</span><span class="sxs-lookup"><span data-stu-id="c8337-203">*I tried tooset up an export, but was denied access*</span></span>

    <span data-ttu-id="c8337-204">Ha hello fiókot a szervezete tulajdonában van, akkor toobe hello tulajdonos vagy közreműködő szerepkörrel rendelkező személyek csoport tagja.</span><span class="sxs-lookup"><span data-stu-id="c8337-204">If hello account is owned by your organization, you have toobe a member of hello owners or contributors groups.</span></span>
* <span data-ttu-id="c8337-205">*Exportálhatja a saját helyszíni tárolási egyenes toomy?*</span><span class="sxs-lookup"><span data-stu-id="c8337-205">*Can I export straight toomy own on-premises store?*</span></span>

    <span data-ttu-id="c8337-206">Nem, sajnos.</span><span class="sxs-lookup"><span data-stu-id="c8337-206">No, sorry.</span></span> <span data-ttu-id="c8337-207">Az Exportálás motor jelenleg csak akkor működik az Azure storage most.</span><span class="sxs-lookup"><span data-stu-id="c8337-207">Our export engine currently only works with Azure storage at this time.</span></span>  
* <span data-ttu-id="c8337-208">*Bármely korlát toohello mennyiségű adat kerüljön a saját tárolóban van?*</span><span class="sxs-lookup"><span data-stu-id="c8337-208">*Is there any limit toohello amount of data you put in my store?*</span></span>

    <span data-ttu-id="c8337-209">Nem.</span><span class="sxs-lookup"><span data-stu-id="c8337-209">No.</span></span> <span data-ttu-id="c8337-210">Azt fogja tartani kérdez le adatokat hello exportálási törléséig.</span><span class="sxs-lookup"><span data-stu-id="c8337-210">We'll keep pushing data in until you delete hello export.</span></span> <span data-ttu-id="c8337-211">Ha azt találati hello külső korlátozhatja a blob Storage tárolóban, de ez közérthető hatalmas lesz leállítása.</span><span class="sxs-lookup"><span data-stu-id="c8337-211">We'll stop if we hit hello outer limits for blob storage, but that's pretty huge.</span></span> <span data-ttu-id="c8337-212">Az működik tooyou toocontrol mennyi tárhelyre használja.</span><span class="sxs-lookup"><span data-stu-id="c8337-212">It's up tooyou toocontrol how much storage you use.</span></span>  
* <span data-ttu-id="c8337-213">*Hány blobok kell lásd hello Storage?*</span><span class="sxs-lookup"><span data-stu-id="c8337-213">*How many blobs should I see in hello storage?*</span></span>

  * <span data-ttu-id="c8337-214">Az összes adattípus kijelölt tooexport, egy új blob létrejön percenként (ha érhetők el adatok).</span><span class="sxs-lookup"><span data-stu-id="c8337-214">For every data type you selected tooexport, a new blob is created every minute (if data is available).</span></span>
  * <span data-ttu-id="c8337-215">Emellett nagy forgalmú alkalmazások esetén további partíció egységek foglal le.</span><span class="sxs-lookup"><span data-stu-id="c8337-215">In addition, for applications with high traffic, additional partition units are allocated.</span></span> <span data-ttu-id="c8337-216">Ebben az esetben tárolóegységekhez hoz létre egy blobot percenként.</span><span class="sxs-lookup"><span data-stu-id="c8337-216">In this case each unit creates a blob every minute.</span></span>
* <span data-ttu-id="c8337-217">*I hello kulcs toomy tárolási újragenerálása vagy hello hello tároló neve megváltozott, és hello exportálás nem működik.*</span><span class="sxs-lookup"><span data-stu-id="c8337-217">*I regenerated hello key toomy storage or changed hello name of hello container, and now hello export doesn't work.*</span></span>

    <span data-ttu-id="c8337-218">Hello exportálás módosításával és hello exportálási cél panel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="c8337-218">Edit hello export and open hello export destination blade.</span></span> <span data-ttu-id="c8337-219">Ugyanazt a tárhelyet választott azt korábban hello hagyja, és kattintson az OK tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="c8337-219">Leave hello same storage selected as before, and click OK tooconfirm.</span></span> <span data-ttu-id="c8337-220">Exportálás újraindul.</span><span class="sxs-lookup"><span data-stu-id="c8337-220">Export will restart.</span></span> <span data-ttu-id="c8337-221">Ha hello módosítása néhány napok hello belül volt, adatok nem vesznek el.</span><span class="sxs-lookup"><span data-stu-id="c8337-221">If hello change was within hello past few days, you won't lose data.</span></span>
* <span data-ttu-id="c8337-222">*Szüneteltetheti hello exportálás is?*</span><span class="sxs-lookup"><span data-stu-id="c8337-222">*Can I pause hello export?*</span></span>

    <span data-ttu-id="c8337-223">Igen.</span><span class="sxs-lookup"><span data-stu-id="c8337-223">Yes.</span></span> <span data-ttu-id="c8337-224">Kattintson a letiltása.</span><span class="sxs-lookup"><span data-stu-id="c8337-224">Click Disable.</span></span>

## <a name="code-samples"></a><span data-ttu-id="c8337-225">Kódminták</span><span class="sxs-lookup"><span data-stu-id="c8337-225">Code samples</span></span>

* [<span data-ttu-id="c8337-226">Stream Analytics-minta</span><span class="sxs-lookup"><span data-stu-id="c8337-226">Stream Analytics sample</span></span>](app-insights-export-stream-analytics.md)
* <span data-ttu-id="c8337-227">[Használja a Stream Analytics tooSQL exportálása][exportasa]</span><span class="sxs-lookup"><span data-stu-id="c8337-227">[Export tooSQL using Stream Analytics][exportasa]</span></span>
* [<span data-ttu-id="c8337-228">Részletes adatok modellhez tartozó referencia hello Tulajdonságtípusok és értékeket.</span><span class="sxs-lookup"><span data-stu-id="c8337-228">Detailed data model reference for hello property types and values.</span></span>](app-insights-export-data-model.md)

<!--Link references-->

[exportasa]: app-insights-code-sample-export-sql-stream-analytics.md
[roles]: app-insights-resources-roles-access-control.md
