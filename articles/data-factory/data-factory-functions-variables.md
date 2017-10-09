---
title: "aaaData gyári funkciók és rendszerváltozók |} Microsoft Docs"
description: "Azure Data Factory funkciók és rendszerváltozók listája"
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
services: data-factory
ms.assetid: b6b3c2ae-b0e8-4e28-90d8-daf20421660d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: d2936c2821797947bb37d9775226a6c19c4b8ab9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory---functions-and-system-variables"></a><span data-ttu-id="8b1ac-103">Az Azure Data Factory - funkciók és a rendszer változói</span><span class="sxs-lookup"><span data-stu-id="8b1ac-103">Azure Data Factory - Functions and System Variables</span></span>
<span data-ttu-id="8b1ac-104">Ez a cikk tájékoztatást ad azokról a funkciók és az Azure Data Factory által támogatott változók.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-104">This article provides information about functions and variables supported by Azure Data Factory.</span></span>

## <a name="data-factory-system-variables"></a><span data-ttu-id="8b1ac-105">Data Factory rendszerváltozók</span><span class="sxs-lookup"><span data-stu-id="8b1ac-105">Data Factory system variables</span></span>
| <span data-ttu-id="8b1ac-106">Változó neve</span><span class="sxs-lookup"><span data-stu-id="8b1ac-106">Variable Name</span></span> | <span data-ttu-id="8b1ac-107">Leírás</span><span class="sxs-lookup"><span data-stu-id="8b1ac-107">Description</span></span> | <span data-ttu-id="8b1ac-108">Objektum hatóköre</span><span class="sxs-lookup"><span data-stu-id="8b1ac-108">Object Scope</span></span> | <span data-ttu-id="8b1ac-109">JSON-hatókör és a használati esetek</span><span class="sxs-lookup"><span data-stu-id="8b1ac-109">JSON Scope and Use Cases</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="8b1ac-110">WindowStart</span><span class="sxs-lookup"><span data-stu-id="8b1ac-110">WindowStart</span></span> |<span data-ttu-id="8b1ac-111">Időtartam a jelenlegi művelet ablakban futtassa elindítása</span><span class="sxs-lookup"><span data-stu-id="8b1ac-111">Start of time interval for current activity run window</span></span> |<span data-ttu-id="8b1ac-112">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="8b1ac-112">activity</span></span> |<ol><li><span data-ttu-id="8b1ac-113">Adja meg a kijelölés lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-113">Specify data selection queries.</span></span> <span data-ttu-id="8b1ac-114">Olvassa el a hello összekötő cikk [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-114">See connector articles referenced in hello [Data Movement Activities](data-factory-data-movement-activities.md) article.</span></span></li> |
| <span data-ttu-id="8b1ac-115">WindowEnd</span><span class="sxs-lookup"><span data-stu-id="8b1ac-115">WindowEnd</span></span> |<span data-ttu-id="8b1ac-116">Időtartam a jelenlegi művelet ablakban futtassa vége</span><span class="sxs-lookup"><span data-stu-id="8b1ac-116">End of time interval for current activity run window</span></span> |<span data-ttu-id="8b1ac-117">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="8b1ac-117">activity</span></span> |<span data-ttu-id="8b1ac-118">ugyanaz, mint WindowStart.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-118">same as WindowStart.</span></span> |
| <span data-ttu-id="8b1ac-119">SliceStart</span><span class="sxs-lookup"><span data-stu-id="8b1ac-119">SliceStart</span></span> |<span data-ttu-id="8b1ac-120">Az adatszelet előállítását időintervallumát elindítása</span><span class="sxs-lookup"><span data-stu-id="8b1ac-120">Start of time interval for data  slice being produced</span></span> |<span data-ttu-id="8b1ac-121">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="8b1ac-121">activity</span></span><br/><span data-ttu-id="8b1ac-122">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="8b1ac-122">dataset</span></span> |<ol><li><span data-ttu-id="8b1ac-123">Adja meg a dinamikus mappák elérési útjaiban és fájlneveket dolgozva [Azure Blob](data-factory-azure-blob-connector.md) és [fájlrendszer adatkészletek](data-factory-onprem-file-system-connector.md).</span><span class="sxs-lookup"><span data-stu-id="8b1ac-123">Specify dynamic folder paths and file names while working with [Azure Blob](data-factory-azure-blob-connector.md) and [File System datasets](data-factory-onprem-file-system-connector.md).</span></span></li><li><span data-ttu-id="8b1ac-124">Adja meg a bemeneti függőségek a data factory funkciók a tevékenység bemenetei gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-124">Specify input dependencies with data factory functions in activity inputs collection.</span></span></li></ol> |
| <span data-ttu-id="8b1ac-125">SliceEnd</span><span class="sxs-lookup"><span data-stu-id="8b1ac-125">SliceEnd</span></span> |<span data-ttu-id="8b1ac-126">Aktuális adatszelet időintervallumát végét.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-126">End of time interval for current data slice.</span></span> |<span data-ttu-id="8b1ac-127">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="8b1ac-127">activity</span></span><br/><span data-ttu-id="8b1ac-128">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="8b1ac-128">dataset</span></span> |<span data-ttu-id="8b1ac-129">ugyanaz, mint SliceStart.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-129">same as SliceStart.</span></span> |

> [!NOTE]
> <span data-ttu-id="8b1ac-130">Jelenleg adat-előállító megköveteli, hogy hello ütemezhet a megadott hello tevékenység pontosan egyezik a megadott hello kimeneti adatkészlet rendelkezésre állását hello ütemezése.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-130">Currently data factory requires that hello schedule specified in hello activity exactly matches hello schedule specified in availability of hello output dataset.</span></span> <span data-ttu-id="8b1ac-131">Ezért WindowStart, a WindowEnd, és a SliceStart és a SliceEnd mindig leképezése toohello ugyanaz az időszak és egy kimeneti szelet időpont.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-131">Therefore, WindowStart, WindowEnd, and SliceStart and SliceEnd always map toohello same time period and a single output slice.</span></span>
> 

### <a name="example-for-using-a-system-variable"></a><span data-ttu-id="8b1ac-132">Példa a rendszer változó segítségével</span><span class="sxs-lookup"><span data-stu-id="8b1ac-132">Example for using a system variable</span></span>
<span data-ttu-id="8b1ac-133">A következő példa, év, hónap, nap és idején hello a **SliceStart** ki kell olvasni a által használt külön változók **folderPath** és **Fájlnév** tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-133">In hello following example, year, month, day, and time of **SliceStart** are extracted into separate variables that are used by **folderPath** and **fileName** properties.</span></span>

```json
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```

## <a name="data-factory-functions"></a><span data-ttu-id="8b1ac-134">Data Factory-funkciók</span><span class="sxs-lookup"><span data-stu-id="8b1ac-134">Data Factory functions</span></span>
<span data-ttu-id="8b1ac-135">Használható funkciók mellett rendszerváltozók adat-előállítóban hello a következő célra:</span><span class="sxs-lookup"><span data-stu-id="8b1ac-135">You can use functions in data factory along with system variables for hello following purposes:</span></span>

1. <span data-ttu-id="8b1ac-136">Kijelölés lekérdezések megadása (hello által hivatkozott összekötő cikkekben talál [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-136">Specifying data selection queries (see connector articles referenced by hello [Data Movement Activities](data-factory-data-movement-activities.md) article.</span></span>
   
   <span data-ttu-id="8b1ac-137">a data factory függvény szintaxis tooinvoke hello:  **$$ <function>**  kijelölés lekérdezések és egyéb tulajdonságok hello tevékenység és adathalmazokat.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-137">hello syntax tooinvoke a data factory function is: **$$<function>** for data selection queries and other properties in hello activity and datasets.</span></span>  
2. <span data-ttu-id="8b1ac-138">Bemeneti függőségek megadása a data factory funkciók a tevékenység bemenetei gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-138">Specifying input dependencies with data factory functions in activity inputs collection.</span></span>
   
    <span data-ttu-id="8b1ac-139">$$ nem szükséges bemeneti függőségi kifejezés megadásával.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-139">$$ is not needed for specifying input dependency expressions.</span></span>     

<span data-ttu-id="8b1ac-140">A következő minta hello **sqlReaderQuery** tulajdonság egy JSON-fájlban hello által visszaadott tooa értéket kapja `Text.Format` függvény.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-140">In hello following sample, **sqlReaderQuery** property in a JSON file is assigned tooa value returned by hello `Text.Format` function.</span></span> <span data-ttu-id="8b1ac-141">Ez a minta is használja a rendszer nevű változó **WindowStart**, amely hello tevékenységfuttatási ablak hello kezdési időt jelenti.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-141">This sample also uses a system variable named **WindowStart**, which represents hello start time of hello activity run window.</span></span>

```json
{
    "Type": "SqlSource",
    "sqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
}
```

<span data-ttu-id="8b1ac-142">Lásd: [egyéni dátum és idő formátumú karakterláncok](https://msdn.microsoft.com/library/8kb3ddd4.aspx) foglalkozó témakör használható különböző formázási beállítások (például: éééé és pi).</span><span class="sxs-lookup"><span data-stu-id="8b1ac-142">See [Custom Date and Time Format Strings](https://msdn.microsoft.com/library/8kb3ddd4.aspx) topic that describes different formatting options you can use (for example: ay vs. yyyy).</span></span> 

### <a name="functions"></a><span data-ttu-id="8b1ac-143">Functions</span><span class="sxs-lookup"><span data-stu-id="8b1ac-143">Functions</span></span>
<span data-ttu-id="8b1ac-144">a következő táblák hello Azure Data Factory összes hello függvény listában:</span><span class="sxs-lookup"><span data-stu-id="8b1ac-144">hello following tables list all hello functions in Azure Data Factory:</span></span>

| <span data-ttu-id="8b1ac-145">Kategória</span><span class="sxs-lookup"><span data-stu-id="8b1ac-145">Category</span></span> | <span data-ttu-id="8b1ac-146">Függvény</span><span class="sxs-lookup"><span data-stu-id="8b1ac-146">Function</span></span> | <span data-ttu-id="8b1ac-147">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="8b1ac-147">Parameters</span></span> | <span data-ttu-id="8b1ac-148">Leírás</span><span class="sxs-lookup"><span data-stu-id="8b1ac-148">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="8b1ac-149">Time</span><span class="sxs-lookup"><span data-stu-id="8b1ac-149">Time</span></span> |<span data-ttu-id="8b1ac-150">AddHours(X,Y)</span><span class="sxs-lookup"><span data-stu-id="8b1ac-150">AddHours(X,Y)</span></span> |<span data-ttu-id="8b1ac-151">X: dátum és idő</span><span class="sxs-lookup"><span data-stu-id="8b1ac-151">X: DateTime</span></span> <br/><br/><span data-ttu-id="8b1ac-152">Y int</span><span class="sxs-lookup"><span data-stu-id="8b1ac-152">Y: int</span></span> |<span data-ttu-id="8b1ac-153">Hozzáadja a megadott idő X Y óra toohello.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-153">Adds Y hours toohello given time X.</span></span> <br/><br/><span data-ttu-id="8b1ac-154">Példa:`9/5/2013 12:00:00 PM + 2 hours = 9/5/2013 2:00:00 PM`</span><span class="sxs-lookup"><span data-stu-id="8b1ac-154">Example: `9/5/2013 12:00:00 PM + 2 hours = 9/5/2013 2:00:00 PM`</span></span> |
| <span data-ttu-id="8b1ac-155">Time</span><span class="sxs-lookup"><span data-stu-id="8b1ac-155">Time</span></span> |<span data-ttu-id="8b1ac-156">AddMinutes(X,Y)</span><span class="sxs-lookup"><span data-stu-id="8b1ac-156">AddMinutes(X,Y)</span></span> |<span data-ttu-id="8b1ac-157">X: dátum és idő</span><span class="sxs-lookup"><span data-stu-id="8b1ac-157">X: DateTime</span></span> <br/><br/><span data-ttu-id="8b1ac-158">Y int</span><span class="sxs-lookup"><span data-stu-id="8b1ac-158">Y: int</span></span> |<span data-ttu-id="8b1ac-159">Y perc tooX hozzáadja.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-159">Adds Y minutes tooX.</span></span><br/><br/><span data-ttu-id="8b1ac-160">Példa:`9/15/2013 12: 00:00 PM + 15 minutes = 9/15/2013 12: 15:00 PM`</span><span class="sxs-lookup"><span data-stu-id="8b1ac-160">Example: `9/15/2013 12: 00:00 PM + 15 minutes = 9/15/2013 12: 15:00 PM`</span></span> |
| <span data-ttu-id="8b1ac-161">Time</span><span class="sxs-lookup"><span data-stu-id="8b1ac-161">Time</span></span> |<span data-ttu-id="8b1ac-162">StartOfHour(X)</span><span class="sxs-lookup"><span data-stu-id="8b1ac-162">StartOfHour(X)</span></span> |<span data-ttu-id="8b1ac-163">X: dátum és idő</span><span class="sxs-lookup"><span data-stu-id="8b1ac-163">X: Datetime</span></span> |<span data-ttu-id="8b1ac-164">Lekérdezi a hello kezdési idő hello óráig hello óra összetevőjét X képviseli.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-164">Gets hello starting time for hello hour represented by hello hour component of X.</span></span> <br/><br/><span data-ttu-id="8b1ac-165">Példa:`StartOfHour of 9/15/2013 05: 10:23 PM is 9/15/2013 05: 00:00 PM`</span><span class="sxs-lookup"><span data-stu-id="8b1ac-165">Example: `StartOfHour of 9/15/2013 05: 10:23 PM is 9/15/2013 05: 00:00 PM`</span></span> |
| <span data-ttu-id="8b1ac-166">Dátum</span><span class="sxs-lookup"><span data-stu-id="8b1ac-166">Date</span></span> |<span data-ttu-id="8b1ac-167">AddDays(X,Y)</span><span class="sxs-lookup"><span data-stu-id="8b1ac-167">AddDays(X,Y)</span></span> |<span data-ttu-id="8b1ac-168">X: dátum és idő</span><span class="sxs-lookup"><span data-stu-id="8b1ac-168">X: DateTime</span></span><br/><br/><span data-ttu-id="8b1ac-169">Y int</span><span class="sxs-lookup"><span data-stu-id="8b1ac-169">Y: int</span></span> |<span data-ttu-id="8b1ac-170">Y nap tooX hozzáadja.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-170">Adds Y days tooX.</span></span> <br/><br/><span data-ttu-id="8b1ac-171">Példa: 9/15/2013 12:00:00 PM + 2 nap = 9/17/2013 12:00:00 PM.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-171">Example: 9/15/2013 12:00:00 PM + 2 days = 9/17/2013 12:00:00 PM.</span></span><br/><br/><span data-ttu-id="8b1ac-172">Nap túl kivonás Y megadása egy negatív számot.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-172">You can subtract days too by specifying Y as a negative number.</span></span><br/><br/><span data-ttu-id="8b1ac-173">Példa: `9/15/2013 12:00:00 PM - 2 days = 9/13/2013 12:00:00 PM`.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-173">Example: `9/15/2013 12:00:00 PM - 2 days = 9/13/2013 12:00:00 PM`.</span></span> |
| <span data-ttu-id="8b1ac-174">Dátum</span><span class="sxs-lookup"><span data-stu-id="8b1ac-174">Date</span></span> |<span data-ttu-id="8b1ac-175">AddMonths(X,Y)</span><span class="sxs-lookup"><span data-stu-id="8b1ac-175">AddMonths(X,Y)</span></span> |<span data-ttu-id="8b1ac-176">X: dátum és idő</span><span class="sxs-lookup"><span data-stu-id="8b1ac-176">X: DateTime</span></span><br/><br/><span data-ttu-id="8b1ac-177">Y int</span><span class="sxs-lookup"><span data-stu-id="8b1ac-177">Y: int</span></span> |<span data-ttu-id="8b1ac-178">Y hónap tooX hozzáadja.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-178">Adds Y months tooX.</span></span><br/><br/><span data-ttu-id="8b1ac-179">`Example: 9/15/2013 12:00:00 PM + 1 month = 10/15/2013 12:00:00 PM`.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-179">`Example: 9/15/2013 12:00:00 PM + 1 month = 10/15/2013 12:00:00 PM`.</span></span><br/><br/><span data-ttu-id="8b1ac-180">Hónap túl kivonás Y megadása egy negatív számot.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-180">You can subtract months too by specifying Y as a negative number.</span></span><br/><br/><span data-ttu-id="8b1ac-181">Példa: `9/15/2013 12:00:00 PM - 1 month = 8/15/2013 12:00:00 PM`.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-181">Example: `9/15/2013 12:00:00 PM - 1 month = 8/15/2013 12:00:00 PM`.</span></span>|
| <span data-ttu-id="8b1ac-182">Dátum</span><span class="sxs-lookup"><span data-stu-id="8b1ac-182">Date</span></span> |<span data-ttu-id="8b1ac-183">AddQuarters(X,Y)</span><span class="sxs-lookup"><span data-stu-id="8b1ac-183">AddQuarters(X,Y)</span></span> |<span data-ttu-id="8b1ac-184">X: dátum és idő</span><span class="sxs-lookup"><span data-stu-id="8b1ac-184">X: DateTime</span></span> <br/><br/><span data-ttu-id="8b1ac-185">Y int</span><span class="sxs-lookup"><span data-stu-id="8b1ac-185">Y: int</span></span> |<span data-ttu-id="8b1ac-186">Hozzáadja a Y * 3 hónapos tooX.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-186">Adds Y * 3 months tooX.</span></span><br/><br/><span data-ttu-id="8b1ac-187">Példa:`9/15/2013 12:00:00 PM + 1 quarter = 12/15/2013 12:00:00 PM`</span><span class="sxs-lookup"><span data-stu-id="8b1ac-187">Example: `9/15/2013 12:00:00 PM + 1 quarter = 12/15/2013 12:00:00 PM`</span></span> |
| <span data-ttu-id="8b1ac-188">Dátum</span><span class="sxs-lookup"><span data-stu-id="8b1ac-188">Date</span></span> |<span data-ttu-id="8b1ac-189">AddWeeks(X,Y)</span><span class="sxs-lookup"><span data-stu-id="8b1ac-189">AddWeeks(X,Y)</span></span> |<span data-ttu-id="8b1ac-190">X: dátum és idő</span><span class="sxs-lookup"><span data-stu-id="8b1ac-190">X: DateTime</span></span><br/><br/><span data-ttu-id="8b1ac-191">Y int</span><span class="sxs-lookup"><span data-stu-id="8b1ac-191">Y: int</span></span> |<span data-ttu-id="8b1ac-192">Hozzáadja a Y * tooX 7 nap</span><span class="sxs-lookup"><span data-stu-id="8b1ac-192">Adds Y * 7 days tooX</span></span><br/><br/><span data-ttu-id="8b1ac-193">Példa: 9/15/2013 12:00:00 PM + 1 hét = 9/22-es/2013 12:00:00 PM</span><span class="sxs-lookup"><span data-stu-id="8b1ac-193">Example: 9/15/2013 12:00:00 PM + 1 week = 9/22/2013 12:00:00 PM</span></span><br/><br/><span data-ttu-id="8b1ac-194">Hét túl kivonás Y megadása egy negatív számot.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-194">You can subtract weeks too by specifying Y as a negative number.</span></span><br/><br/><span data-ttu-id="8b1ac-195">Példa: `9/15/2013 12:00:00 PM - 1 week = 9/7/2013 12:00:00 PM`.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-195">Example: `9/15/2013 12:00:00 PM - 1 week = 9/7/2013 12:00:00 PM`.</span></span> |
| <span data-ttu-id="8b1ac-196">Dátum</span><span class="sxs-lookup"><span data-stu-id="8b1ac-196">Date</span></span> |<span data-ttu-id="8b1ac-197">AddYears(X,Y)</span><span class="sxs-lookup"><span data-stu-id="8b1ac-197">AddYears(X,Y)</span></span> |<span data-ttu-id="8b1ac-198">X: dátum és idő</span><span class="sxs-lookup"><span data-stu-id="8b1ac-198">X: DateTime</span></span><br/><br/><span data-ttu-id="8b1ac-199">Y int</span><span class="sxs-lookup"><span data-stu-id="8b1ac-199">Y: int</span></span> |<span data-ttu-id="8b1ac-200">Y év tooX hozzáadja.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-200">Adds Y years tooX.</span></span><br/><br/>`Example: 9/15/2013 12:00:00 PM + 1 year = 9/15/2014 12:00:00 PM`<br/><br/><span data-ttu-id="8b1ac-201">Év túl kivonás Y megadása egy negatív számot.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-201">You can subtract years too by specifying Y as a negative number.</span></span><br/><br/><span data-ttu-id="8b1ac-202">Példa: `9/15/2013 12:00:00 PM - 1 year = 9/15/2012 12:00:00 PM`.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-202">Example: `9/15/2013 12:00:00 PM - 1 year = 9/15/2012 12:00:00 PM`.</span></span> |
| <span data-ttu-id="8b1ac-203">Dátum</span><span class="sxs-lookup"><span data-stu-id="8b1ac-203">Date</span></span> |<span data-ttu-id="8b1ac-204">Day(X)</span><span class="sxs-lookup"><span data-stu-id="8b1ac-204">Day(X)</span></span> |<span data-ttu-id="8b1ac-205">X: dátum és idő</span><span class="sxs-lookup"><span data-stu-id="8b1ac-205">X: DateTime</span></span> |<span data-ttu-id="8b1ac-206">Lekérdezi a hello nap összetevőjének megállapítása X.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-206">Gets hello day component of X.</span></span><br/><br/><span data-ttu-id="8b1ac-207">Példa: `Day of 9/15/2013 12:00:00 PM is 9`.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-207">Example: `Day of 9/15/2013 12:00:00 PM is 9`.</span></span> |
| <span data-ttu-id="8b1ac-208">Dátum</span><span class="sxs-lookup"><span data-stu-id="8b1ac-208">Date</span></span> |<span data-ttu-id="8b1ac-209">DayOfWeek(X)</span><span class="sxs-lookup"><span data-stu-id="8b1ac-209">DayOfWeek(X)</span></span> |<span data-ttu-id="8b1ac-210">X: dátum és idő</span><span class="sxs-lookup"><span data-stu-id="8b1ac-210">X: DateTime</span></span> |<span data-ttu-id="8b1ac-211">Lekérdezi a hello nap, hét összetevőjének X.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-211">Gets hello day of week component of X.</span></span><br/><br/><span data-ttu-id="8b1ac-212">Példa: `DayOfWeek of 9/15/2013 12:00:00 PM is Sunday`.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-212">Example: `DayOfWeek of 9/15/2013 12:00:00 PM is Sunday`.</span></span> |
| <span data-ttu-id="8b1ac-213">Dátum</span><span class="sxs-lookup"><span data-stu-id="8b1ac-213">Date</span></span> |<span data-ttu-id="8b1ac-214">DayOfYear(X)</span><span class="sxs-lookup"><span data-stu-id="8b1ac-214">DayOfYear(X)</span></span> |<span data-ttu-id="8b1ac-215">X: dátum és idő</span><span class="sxs-lookup"><span data-stu-id="8b1ac-215">X: DateTime</span></span> |<span data-ttu-id="8b1ac-216">Hello év összetevőjét X által képviselt hello évben hello nap lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-216">Gets hello day in hello year represented by hello year component of X.</span></span><br/><br/><span data-ttu-id="8b1ac-217">Példák:</span><span class="sxs-lookup"><span data-stu-id="8b1ac-217">Examples:</span></span><br/>`12/1/2015: day 335 of 2015`<br/>`12/31/2015: day 365 of 2015`<br/>`12/31/2016: day 366 of 2016 (Leap Year)` |
| <span data-ttu-id="8b1ac-218">Dátum</span><span class="sxs-lookup"><span data-stu-id="8b1ac-218">Date</span></span> |<span data-ttu-id="8b1ac-219">DaysInMonth(X)</span><span class="sxs-lookup"><span data-stu-id="8b1ac-219">DaysInMonth(X)</span></span> |<span data-ttu-id="8b1ac-220">X: dátum és idő</span><span class="sxs-lookup"><span data-stu-id="8b1ac-220">X: DateTime</span></span> |<span data-ttu-id="8b1ac-221">Hello hónap összetevőjét X paraméter által képviselt hello hónap napjainak hello lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-221">Gets hello days in hello month represented by hello month component of parameter X.</span></span><br/><br/><span data-ttu-id="8b1ac-222">Példa: `DaysInMonth of 9/15/2013 are 30 since there are 30 days in hello September month`.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-222">Example: `DaysInMonth of 9/15/2013 are 30 since there are 30 days in hello September month`.</span></span> |
| <span data-ttu-id="8b1ac-223">Dátum</span><span class="sxs-lookup"><span data-stu-id="8b1ac-223">Date</span></span> |<span data-ttu-id="8b1ac-224">EndOfDay(X)</span><span class="sxs-lookup"><span data-stu-id="8b1ac-224">EndOfDay(X)</span></span> |<span data-ttu-id="8b1ac-225">X: dátum és idő</span><span class="sxs-lookup"><span data-stu-id="8b1ac-225">X: DateTime</span></span> |<span data-ttu-id="8b1ac-226">Hello dátum idejű hello napja (nap összetevőjének), X hello végét jelölő lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-226">Gets hello date-time that represents hello end of hello day (day component) of X.</span></span><br/><br/><span data-ttu-id="8b1ac-227">Példa: `EndOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 11:59:59 PM`.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-227">Example: `EndOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 11:59:59 PM`.</span></span> |
| <span data-ttu-id="8b1ac-228">Dátum</span><span class="sxs-lookup"><span data-stu-id="8b1ac-228">Date</span></span> |<span data-ttu-id="8b1ac-229">EndOfMonth(X)</span><span class="sxs-lookup"><span data-stu-id="8b1ac-229">EndOfMonth(X)</span></span> |<span data-ttu-id="8b1ac-230">X: dátum és idő</span><span class="sxs-lookup"><span data-stu-id="8b1ac-230">X: DateTime</span></span> |<span data-ttu-id="8b1ac-231">Lekérdezi a hónap összetevőjét X paraméter által képviselt hello hónap hello végét.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-231">Gets hello end of hello month represented by month component of parameter X.</span></span> <br/><br/><span data-ttu-id="8b1ac-232">Példa: `EndOfMonth of 9/15/2013 05:10:23 PM is 9/30/2013 11:59:59 PM` (dátuma és időpontja hello szeptember a hónap végét jelölő)</span><span class="sxs-lookup"><span data-stu-id="8b1ac-232">Example: `EndOfMonth of 9/15/2013 05:10:23 PM is 9/30/2013 11:59:59 PM` (date time that represents hello end of September month)</span></span> |
| <span data-ttu-id="8b1ac-233">Dátum</span><span class="sxs-lookup"><span data-stu-id="8b1ac-233">Date</span></span> |<span data-ttu-id="8b1ac-234">StartOfDay(X)</span><span class="sxs-lookup"><span data-stu-id="8b1ac-234">StartOfDay(X)</span></span> |<span data-ttu-id="8b1ac-235">X: dátum és idő</span><span class="sxs-lookup"><span data-stu-id="8b1ac-235">X: DateTime</span></span> |<span data-ttu-id="8b1ac-236">Hello start hello nap összetevőjének megállapítása X paraméter által képviselt hello nap lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-236">Gets hello start of hello day represented by hello day component of parameter X.</span></span><br/><br/><span data-ttu-id="8b1ac-237">Példa: `StartOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 12:00:00 AM`.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-237">Example: `StartOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 12:00:00 AM`.</span></span> |
| <span data-ttu-id="8b1ac-238">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="8b1ac-238">DateTime</span></span> |<span data-ttu-id="8b1ac-239">FROM(X)</span><span class="sxs-lookup"><span data-stu-id="8b1ac-239">From(X)</span></span> |<span data-ttu-id="8b1ac-240">X: karakterlánc</span><span class="sxs-lookup"><span data-stu-id="8b1ac-240">X: String</span></span> |<span data-ttu-id="8b1ac-241">X tooa karakterlánc elemzése időpontja.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-241">Parse string X tooa date time.</span></span> |
| <span data-ttu-id="8b1ac-242">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="8b1ac-242">DateTime</span></span> |<span data-ttu-id="8b1ac-243">Ticks(X)</span><span class="sxs-lookup"><span data-stu-id="8b1ac-243">Ticks(X)</span></span> |<span data-ttu-id="8b1ac-244">X: dátum és idő</span><span class="sxs-lookup"><span data-stu-id="8b1ac-244">X: DateTime</span></span> |<span data-ttu-id="8b1ac-245">Lekérdezi a hello ticks X hello paraméter tulajdonsága. Egy osztásjelek 100 nanoszekundumban egyenlő.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-245">Gets hello ticks property of hello parameter X. One tick equals 100 nanoseconds.</span></span> <span data-ttu-id="8b1ac-246">Ez a tulajdonság értékének hello ticks, 1 januárt, 12:00:00 éjfél óta eltelt 0001 hello számát jelöli.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-246">hello value of this property represents hello number of ticks that have elapsed since 12:00:00 midnight, January 1, 0001.</span></span> |
| <span data-ttu-id="8b1ac-247">Szöveg</span><span class="sxs-lookup"><span data-stu-id="8b1ac-247">Text</span></span> |<span data-ttu-id="8b1ac-248">Format(X)</span><span class="sxs-lookup"><span data-stu-id="8b1ac-248">Format(X)</span></span> |<span data-ttu-id="8b1ac-249">X: karakterlánc-változóvá</span><span class="sxs-lookup"><span data-stu-id="8b1ac-249">X: String variable</span></span> |<span data-ttu-id="8b1ac-250">Formátumok hello szöveg (használata `\\'` kombinációja tooescape `'` karakter).</span><span class="sxs-lookup"><span data-stu-id="8b1ac-250">Formats hello text (use `\\'` combination tooescape `'` character).</span></span>|

> [!IMPORTANT]
> <span data-ttu-id="8b1ac-251">Egy másik függvényen belül függvény használatakor nem kell toouse  **$$**  hello belső függvény előtag.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-251">When using a function within another function, you do not need toouse **$$** prefix for hello inner function.</span></span> <span data-ttu-id="8b1ac-252">Például: $$Text.Format ("PartitionKey eq \\" my_pkey_filter_value\\"és a RowKey ge \\" {0: éééé-hh-nn óó: pp:}\\", Time.AddHours (SliceStart, -6)).</span><span class="sxs-lookup"><span data-stu-id="8b1ac-252">For example: $$Text.Format('PartitionKey eq \\'my_pkey_filter_value\\' and RowKey ge \\'{0: yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(SliceStart, -6)).</span></span> <span data-ttu-id="8b1ac-253">Figyelje meg, hogy a jelen példában  **$$**  előtag nem használják-e hello **Time.AddHours** függvény.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-253">In this example, notice that **$$** prefix is not used for hello **Time.AddHours** function.</span></span> 

#### <a name="example"></a><span data-ttu-id="8b1ac-254">Példa</span><span class="sxs-lookup"><span data-stu-id="8b1ac-254">Example</span></span>
<span data-ttu-id="8b1ac-255">Hello a következő példa, valamint bemeneti és kimeneti paraméterek hello Hive tevékenység határozzák meg hello segítségével `Text.Format` függvény és SliceStart rendszer változó.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-255">In hello following example, input and output parameters for hello Hive activity are determined by using hello `Text.Format` function and SliceStart system variable.</span></span> 

```json  
{
    "name": "HiveActivitySamplePipeline",
        "properties": {
    "activities": [
            {
            "name": "HiveActivitySample",
            "type": "HDInsightHive",
            "inputs": [
                    {
                    "name": "HiveSampleIn"
                    }
            ],
            "outputs": [
                    {
                    "name": "HiveSampleOut"
                }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "typeproperties": {
                    "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                    "scriptLinkedService": "StorageLinkedService",
                    "defines": {
                        "Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)",
                        "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                }
            }
            }
    ]
    }
}
```

### <a name="example-2"></a><span data-ttu-id="8b1ac-256">2. példa</span><span class="sxs-lookup"><span data-stu-id="8b1ac-256">Example 2</span></span>

<span data-ttu-id="8b1ac-257">A következő példa hello hello DateTime paraméter hello tárolt eljárási tevékenység szöveg hello segítségével határozza meg.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-257">In hello following example, hello DateTime parameter for hello Stored Procedure Activity is determined by using hello Text.</span></span> <span data-ttu-id="8b1ac-258">Függvény formázása és hello SliceStart változó.</span><span class="sxs-lookup"><span data-stu-id="8b1ac-258">Format function and hello SliceStart variable.</span></span> 

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
                "outputs": [
                    {
                        "name": "sprocsampleout"
                    }
                ],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "SprocActivitySample"
            }
        ],
            "start": "2016-08-02T00:00:00Z",
            "end": "2016-08-02T05:00:00Z",
        "isPaused": false
    }
}
```

### <a name="example-3"></a><span data-ttu-id="8b1ac-259">3. példa</span><span class="sxs-lookup"><span data-stu-id="8b1ac-259">Example 3</span></span>
<span data-ttu-id="8b1ac-260">tooread adatok előző nap hello SliceStart, által jelölt nap helyett használja a hello napokHozzaadasa függvényt, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="8b1ac-260">tooread data from previous day instead of day represented by hello SliceStart, use hello AddDays function as shown in hello following example:</span></span> 

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-01-01T08:00:00",
        "end": "2017-01-01T11:00:00",
        "description": "hive activity",
        "activities": [
            {
                "name": "SampleHiveActivity",
                "inputs": [
                    {
                        "name": "MyAzureBlobInput",
                        "startTime": "Date.AddDays(SliceStart, -1)",
                        "endTime": "Date.AddDays(SliceEnd, -1)"
                    }
                ],
                "outputs": [
                    {
                        "name": "MyAzureBlobOutput"
                    }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adftutorial\\hivequery.hql",
                    "scriptLinkedService": "StorageLinkedService",
                    "defines": {
                        "Year": "$$Text.Format('{0:yyyy}',WindowsStart)",
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

<span data-ttu-id="8b1ac-261">Lásd: [egyéni dátum és idő formátumú karakterláncok](https://msdn.microsoft.com/library/8kb3ddd4.aspx) foglalkozó témakör használható különböző formázási beállítások (például: nn és éééé).</span><span class="sxs-lookup"><span data-stu-id="8b1ac-261">See [Custom Date and Time Format Strings](https://msdn.microsoft.com/library/8kb3ddd4.aspx) topic that describes different formatting options you can use (for example: yy vs. yyyy).</span></span> 

