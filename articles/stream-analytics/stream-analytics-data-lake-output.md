---
title: Stream Analytics Data Lake Store kimeneti |} Microsoft Docs
description: "Hitelesítési és engedélyezési egy Azure Data Lake store a Stream Analytics-feladatok konfigurálása"
keywords: 
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: ea5baafa-0054-4c70-973a-6a3a8c6eaffc
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 3d867df3ef875d5cc41de418c3d1d269ff751fda
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="stream-analytics-data-lake-store-output"></a><span data-ttu-id="90d5b-103">Stream Analytics Data Lake Store kimeneti</span><span class="sxs-lookup"><span data-stu-id="90d5b-103">Stream Analytics Data Lake Store output</span></span>
<span data-ttu-id="90d5b-104">Stream Analytics-feladatok támogatja több kimeneti módszerek közül az egyik egy [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/).</span><span class="sxs-lookup"><span data-stu-id="90d5b-104">Stream Analytics jobs support several output methods, one being an [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/).</span></span> <span data-ttu-id="90d5b-105">Az Azure Data Lake Store egy vállalati szintű, nagy kapacitású adattár a big data koncepción alapuló adatelemzési célokra.</span><span class="sxs-lookup"><span data-stu-id="90d5b-105">Azure Data Lake Store is an enterprise-wide hyper-scale repository for big data analytic workloads.</span></span> <span data-ttu-id="90d5b-106">Data Lake Store lehetővé teszi, hogy a műveleti és felderítési jellegű bármilyen méretű, típusú és feldolgozási sebességű adatok.</span><span class="sxs-lookup"><span data-stu-id="90d5b-106">Data Lake Store enables you to store data of any size, type and ingestion speed for operational and exploratory analytics.</span></span>

## <a name="authorize-a-data-lake-store-account"></a><span data-ttu-id="90d5b-107">Data Lake Store-fiók engedélyezése</span><span class="sxs-lookup"><span data-stu-id="90d5b-107">Authorize a Data Lake Store account</span></span>
1. <span data-ttu-id="90d5b-108">Data Lake Store az Azure portálon kimenetként kiválasztásakor kérni fogja a meglévő Data Lake Store használatának engedélyezéséhez, vagy kérjen hozzáférést a Data Lake store a klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="90d5b-108">When Data Lake Store is selected as an output in the Azure portal, you will be prompted to authorize use of your existing Data Lake Store or to request access to the Data Lake Store via the Classic Portal.</span></span>
   
   ![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-authorization.png)  
   
2. <span data-ttu-id="90d5b-109">Ha már van hozzáférésük Data Lake Store-ba, kattintson a "Most engedélyezése", és egy rövid ideig lap jelenik meg "Engedély átirányítása" jelző.</span><span class="sxs-lookup"><span data-stu-id="90d5b-109">If you already have access to Data Lake Store, click “Authorize Now” and for a brief time a page will pop up indicating “Redirecting to authorization”.</span></span> <span data-ttu-id="90d5b-110">A lap automatikusan bezárul, és lehetővé teszi a Data Lake Store kimeneti konfigurálása lap jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="90d5b-110">The page will automatically close and you will be presented with the page that would allow you to configure the Data Lake Store output.</span></span>

<span data-ttu-id="90d5b-111">Ha nem feliratkozott a Data Lake Store, kövesse a "Feliratkozás most" hivatkozásra kattintva kezdeményezheti a kérelmet, vagy hajtsa végre a [elindított útmutatást](../data-lake-store/data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="90d5b-111">If you have not signed up for Data Lake Store, you can follow the “Sign up now” link to initiate the request, or follow the [get started instructions](../data-lake-store/data-lake-store-get-started-portal.md).</span></span>

## <a name="configure-the-data-lake-store-output-properties"></a><span data-ttu-id="90d5b-112">A Data Lake Store kimeneti tulajdonságainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="90d5b-112">Configure the Data Lake Store output properties</span></span>
<span data-ttu-id="90d5b-113">Miután a Data Lake Store-fiók hitelesítését, a Data Lake Store kimeneti megadhatja a tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="90d5b-113">Once you have the Data Lake Store account authenticated, you can configure the properties for your Data Lake Store output.</span></span> <span data-ttu-id="90d5b-114">Az alábbi táblázat pedig a tulajdonság nevét és azok leírását a Data Lake Store kimeneti konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="90d5b-114">The table below is the list of property names and their description to configure your Data Lake Store output.</span></span>

<table>
<tbody>
<tr>
<td><span data-ttu-id="90d5b-115"><B>TULAJDONSÁG NEVE</B></span><span class="sxs-lookup"><span data-stu-id="90d5b-115"><B>PROPERTY NAME</B></span></span></td>
<td><span data-ttu-id="90d5b-116"><B>LEÍRÁS</B></span><span class="sxs-lookup"><span data-stu-id="90d5b-116"><B>DESCRIPTION</B></span></span></td>
</tr>
<tr>
<td><span data-ttu-id="90d5b-117">A kimeneti Alias</span><span class="sxs-lookup"><span data-stu-id="90d5b-117">Output Alias</span></span></td>
<td><span data-ttu-id="90d5b-118">Ez az a lekérdezés kimenete a Data Lake Store a lekérdezésekben használt rövid nevét.</span><span class="sxs-lookup"><span data-stu-id="90d5b-118">This is a friendly name used in queries to direct the query output to this Data Lake Store.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="90d5b-119">Data Lake Store-fiók</span><span class="sxs-lookup"><span data-stu-id="90d5b-119">Data Lake Store Account</span></span></td>
<td><span data-ttu-id="90d5b-120">A tárfiók, ahol küldendő a kimeneti neve.</span><span class="sxs-lookup"><span data-stu-id="90d5b-120">The name of the storage account where you are sending your output.</span></span> <span data-ttu-id="90d5b-121">Meg fog jelenni a bejelentkezett felhasználó hozzáféréssel rendelkezik Data Lake Store-fiókok listáját.</span><span class="sxs-lookup"><span data-stu-id="90d5b-121">You will be presented with a list of Data Lake Store accounts  the logged in user has access to.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="90d5b-122">Elérési út előtag mintája [<I>választható</I>]</span><span class="sxs-lookup"><span data-stu-id="90d5b-122">Path Prefix Pattern [<I>optional</I>]</span></span></td>
<td><span data-ttu-id="90d5b-123">A megadott Data Lake Store-fiók található a fájl írásához használt elérési.</span><span class="sxs-lookup"><span data-stu-id="90d5b-123">The file path used to write your files within the specified Data Lake Store Account.</span></span> <BR><span data-ttu-id="90d5b-124">a {date}, {time}</span><span class="sxs-lookup"><span data-stu-id="90d5b-124">{date}, {time}</span></span><BR><span data-ttu-id="90d5b-125">1. példa: mappa1/logs / {date} / {time}</span><span class="sxs-lookup"><span data-stu-id="90d5b-125">Example 1: folder1/logs/{date}/{time}</span></span><BR><span data-ttu-id="90d5b-126">2. példa: mappa1/logs / {date}</span><span class="sxs-lookup"><span data-stu-id="90d5b-126">Example 2: folder1/logs/{date}</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="90d5b-127">Dátum formátumban [<I>választható</I>]</span><span class="sxs-lookup"><span data-stu-id="90d5b-127">Date Format [<I>optional</I>]</span></span></td>
<td><span data-ttu-id="90d5b-128">Ha a dátum jogkivonat a előtag elérési útját, válassza a dátumformátum, amelyben a fájlok vannak rendezve.</span><span class="sxs-lookup"><span data-stu-id="90d5b-128">If the date token is used in the prefix path, you can select the date format in which your files are organized.</span></span> <span data-ttu-id="90d5b-129">. Példa: Éééé/hh/nn</span><span class="sxs-lookup"><span data-stu-id="90d5b-129">Example: YYYY/MM/DD</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="90d5b-130">Idő formátumot [<I>választható</I>]</span><span class="sxs-lookup"><span data-stu-id="90d5b-130">Time Format [<I>optional</I>]</span></span></td>
<td><span data-ttu-id="90d5b-131">Ha a idő jogkivonat előtag elérési, adja meg az időformátum, amelyben a fájlok vannak rendezve.</span><span class="sxs-lookup"><span data-stu-id="90d5b-131">If the time token is used in the prefix path, specify the time format in which your files are organized.</span></span> <span data-ttu-id="90d5b-132">Jelenleg az egyetlen támogatott érték HH.</span><span class="sxs-lookup"><span data-stu-id="90d5b-132">Currently the only supported value is HH.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="90d5b-133">Esemény szerializálási formátum</span><span class="sxs-lookup"><span data-stu-id="90d5b-133">Event Serialization Format</span></span></td>
<td><span data-ttu-id="90d5b-134">A kimeneti adatok szerializálási formátum.</span><span class="sxs-lookup"><span data-stu-id="90d5b-134">Serialization format for output data.</span></span> <span data-ttu-id="90d5b-135">JSON, CSV és az avro-hoz támogatott.</span><span class="sxs-lookup"><span data-stu-id="90d5b-135">JSON, CSV, and Avro are supported.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="90d5b-136">Encoding</span><span class="sxs-lookup"><span data-stu-id="90d5b-136">Encoding</span></span></td>
<td><span data-ttu-id="90d5b-137">Ha a fürt megosztott kötetei szolgáltatás- vagy JSON formátumú, kódolással meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="90d5b-137">If CSV or JSON format, an encoding must be specified.</span></span> <span data-ttu-id="90d5b-138">Jelenleg az UTF-8 az egyetlen támogatott kódolási formátum.</span><span class="sxs-lookup"><span data-stu-id="90d5b-138">UTF-8 is the only supported encoding format at this time.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="90d5b-139">Elválasztó</span><span class="sxs-lookup"><span data-stu-id="90d5b-139">Delimiter</span></span></td>
<td><span data-ttu-id="90d5b-140">Csak a fürt megosztott kötetei szolgáltatás szerializálási alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="90d5b-140">Only applicable for CSV serialization.</span></span> <span data-ttu-id="90d5b-141">A Stream Analytics számos általánosan használt elválasztó karaktert támogatja a CSV-adatok szerializálása során.</span><span class="sxs-lookup"><span data-stu-id="90d5b-141">Stream Analytics supports a number of common delimiters for serializing CSV data.</span></span> <span data-ttu-id="90d5b-142">Támogatott értékei vesszővel, a pontosvesszővel válassza el, a terület, a lapon és a függőleges vonal.</span><span class="sxs-lookup"><span data-stu-id="90d5b-142">Supported values are comma, semicolon, space, tab and vertical bar.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="90d5b-143">Formátumban</span><span class="sxs-lookup"><span data-stu-id="90d5b-143">Format</span></span></td>
<td><span data-ttu-id="90d5b-144">Csak a JSON-szerializálás alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="90d5b-144">Only applicable for JSON serialization.</span></span> <span data-ttu-id="90d5b-145">Sorral elválasztott beállítás megadja, hogy a kimeneti azzal, hogy minden JSON-objektum sortöréssel elválasztva kell formázni.</span><span class="sxs-lookup"><span data-stu-id="90d5b-145">Line separated specifies that the output will be formatted by having each JSON object separated by a new line.</span></span> <span data-ttu-id="90d5b-146">A tömb határozza meg, hogy a kimeneti lesznek formázva, egy JSON-objektumok tömbjét.</span><span class="sxs-lookup"><span data-stu-id="90d5b-146">Array specifies that the output will be formatted as an array of JSON objects.</span></span></td>
</tr>
</tbody>
</table>

## <a name="renew-data-lake-store-authorization"></a><span data-ttu-id="90d5b-147">Újítsa meg a Data Lake Store engedélyezési</span><span class="sxs-lookup"><span data-stu-id="90d5b-147">Renew Data Lake Store Authorization</span></span>
<span data-ttu-id="90d5b-148">Jelenleg egy korlátozás amikor a hitelesítési jogkivonat kell manuálisan frissíteni kell a Data Lake Store kimenet összes feladat 90 naponta.</span><span class="sxs-lookup"><span data-stu-id="90d5b-148">Currently, there is a limitation where the authentication token needs to be manually refreshed every 90 days for all jobs with Data Lake Store output.</span></span> <span data-ttu-id="90d5b-149">Szüksége lesz is, ha sikeresen módosította jelszavát, mert a feladat meg lett létrehozva, vagy utolsó hitelesített újra hitelesíteni a Data Lake Store-fiókját.</span><span class="sxs-lookup"><span data-stu-id="90d5b-149">You will also need to re-authenticate your Data Lake Store account if you have changed your password since your job was created or last authenticated.</span></span> <span data-ttu-id="90d5b-150">A probléma tünete nincs feladat kimenet és a művelet újbóli engedélyezése szükségességét jelző naplókban hiba.</span><span class="sxs-lookup"><span data-stu-id="90d5b-150">A symptom of this issue is no job output and an error in the Operation Logs indicating need for re-authorization.</span></span>

<span data-ttu-id="90d5b-151">A probléma megoldásához állítsa le a futó feladat, és nyissa meg a Data Lake Store a kimenetbe.</span><span class="sxs-lookup"><span data-stu-id="90d5b-151">To resolve this issue, stop your running job and go to your Data Lake Store output.</span></span> <span data-ttu-id="90d5b-152">Kattintson az "Újra a portálon" hivatkozásra, és egy rövid ideig lap jelenik meg "Engedély átirányítása..." jelző.</span><span class="sxs-lookup"><span data-stu-id="90d5b-152">Click the “Renew authorization” link, and for a brief time a page will pop up indicating “Redirecting to authorization..”.</span></span> <span data-ttu-id="90d5b-153">A lap automatikusan bezáródik, és ha sikeres, fogja jelezni, "Engedélyezési sikeresen megújítva".</span><span class="sxs-lookup"><span data-stu-id="90d5b-153">The page will automatically close and if successful, will indicate “Authorization has been successfully renewed”.</span></span> <span data-ttu-id="90d5b-154">Később, majd az oldal alján kattintson a "Mentés" kell, és indítsa újra a feladatot a feladat utolsó befejezési időpontja adatvesztés elkerülése érdekében csak ezután folytatható.</span><span class="sxs-lookup"><span data-stu-id="90d5b-154">You then need to click “Save” at the bottom of the page, and can proceed by restarting your job from the Last Stopped Time to avoid data loss.</span></span>

![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-renew-authorization.png)

