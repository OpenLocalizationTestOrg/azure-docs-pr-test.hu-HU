---
title: aaaStream Analytics Data Lake Store kimeneti |} Microsoft Docs
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
ms.openlocfilehash: 183cf51edb2e49ac3e42257e67a8077b95777258
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="stream-analytics-data-lake-store-output"></a><span data-ttu-id="4ae9c-103">Stream Analytics Data Lake Store kimeneti</span><span class="sxs-lookup"><span data-stu-id="4ae9c-103">Stream Analytics Data Lake Store output</span></span>
<span data-ttu-id="4ae9c-104">Stream Analytics-feladatok támogatja több kimeneti módszerek közül az egyik egy [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/).</span><span class="sxs-lookup"><span data-stu-id="4ae9c-104">Stream Analytics jobs support several output methods, one being an [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/).</span></span> <span data-ttu-id="4ae9c-105">Az Azure Data Lake Store egy vállalati szintű, nagy kapacitású adattár a big data koncepción alapuló adatelemzési célokra.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-105">Azure Data Lake Store is an enterprise-wide hyper-scale repository for big data analytic workloads.</span></span> <span data-ttu-id="4ae9c-106">Data Lake Store lehetővé teszi a műveleti és felderítési jellegű bármilyen méretű, típusú és feldolgozási sebességű toostore adatok.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-106">Data Lake Store enables you toostore data of any size, type and ingestion speed for operational and exploratory analytics.</span></span>

## <a name="authorize-a-data-lake-store-account"></a><span data-ttu-id="4ae9c-107">Data Lake Store-fiók engedélyezése</span><span class="sxs-lookup"><span data-stu-id="4ae9c-107">Authorize a Data Lake Store account</span></span>
1. <span data-ttu-id="4ae9c-108">Data Lake Store az Azure-portálon hello kimenetként kiválasztásakor kérni fogja a meglévő Data Lake Store vagy toorequest tooauthorize használata toohello Data Lake Store elérése hello klasszikus portál eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-108">When Data Lake Store is selected as an output in hello Azure portal, you will be prompted tooauthorize use of your existing Data Lake Store or toorequest access toohello Data Lake Store via hello Classic Portal.</span></span>
   
   ![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-authorization.png)  
   
2. <span data-ttu-id="4ae9c-109">Ha már rendelkezik tooData Lake áruház eléréséhez kattintson a "Azonnali engedélyezése" és egy rövid ideig lap jelenik meg, amely jelzi, "Átirányítása tooauthorization".</span><span class="sxs-lookup"><span data-stu-id="4ae9c-109">If you already have access tooData Lake Store, click “Authorize Now” and for a brief time a page will pop up indicating “Redirecting tooauthorization”.</span></span> <span data-ttu-id="4ae9c-110">hello lap automatikusan bezárul, és lehetővé teszi tooconfigure hello Data Lake Store kimeneti hello lapok választhat.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-110">hello page will automatically close and you will be presented with hello page that would allow you tooconfigure hello Data Lake Store output.</span></span>

<span data-ttu-id="4ae9c-111">Nem feliratkozott a Data Lake Store, esetén hajtsa végre a hello "Feliratkozás most" hivatkozás tooinitiate hello kérelem, vagy hajtsa végre a hello [elindított útmutatást](../data-lake-store/data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4ae9c-111">If you have not signed up for Data Lake Store, you can follow hello “Sign up now” link tooinitiate hello request, or follow hello [get started instructions](../data-lake-store/data-lake-store-get-started-portal.md).</span></span>

## <a name="configure-hello-data-lake-store-output-properties"></a><span data-ttu-id="4ae9c-112">Hello Data Lake Store kimeneti tulajdonságainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4ae9c-112">Configure hello Data Lake Store output properties</span></span>
<span data-ttu-id="4ae9c-113">Miután hitelesített hello Data Lake Store-fiók, a Data Lake Store kimeneti állíthatja be a hello tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-113">Once you have hello Data Lake Store account authenticated, you can configure hello properties for your Data Lake Store output.</span></span> <span data-ttu-id="4ae9c-114">az alábbi táblázat hello tulajdonságnevek és azok leírása tooconfigure a Data Lake Store kimeneti hello listája.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-114">hello table below is hello list of property names and their description tooconfigure your Data Lake Store output.</span></span>

<table>
<tbody>
<tr>
<td><span data-ttu-id="4ae9c-115"><B>TULAJDONSÁG NEVE</B></span><span class="sxs-lookup"><span data-stu-id="4ae9c-115"><B>PROPERTY NAME</B></span></span></td>
<td><span data-ttu-id="4ae9c-116"><B>LEÍRÁS</B></span><span class="sxs-lookup"><span data-stu-id="4ae9c-116"><B>DESCRIPTION</B></span></span></td>
</tr>
<tr>
<td><span data-ttu-id="4ae9c-117">A kimeneti Alias</span><span class="sxs-lookup"><span data-stu-id="4ae9c-117">Output Alias</span></span></td>
<td><span data-ttu-id="4ae9c-118">Ez az egy rövid nevet használt a lekérdezések toodirect hello lekérdezés kimeneti toothis Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-118">This is a friendly name used in queries toodirect hello query output toothis Data Lake Store.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="4ae9c-119">Data Lake Store-fiók</span><span class="sxs-lookup"><span data-stu-id="4ae9c-119">Data Lake Store Account</span></span></td>
<td><span data-ttu-id="4ae9c-120">ahol a kimeneti küldi hello tárfiók hello neve.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-120">hello name of hello storage account where you are sending your output.</span></span> <span data-ttu-id="4ae9c-121">Választhat hello bejelentkezett felhasználó hozzáféréssel rendelkezik Data Lake Store-fiókok listájával.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-121">You will be presented with a list of Data Lake Store accounts  hello logged in user has access to.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="4ae9c-122">Elérési út előtag mintája [<I>választható</I>]</span><span class="sxs-lookup"><span data-stu-id="4ae9c-122">Path Prefix Pattern [<I>optional</I>]</span></span></td>
<td><span data-ttu-id="4ae9c-123">fájl elérési útja toowrite hello hello található a fájl megadott Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-123">hello file path used toowrite your files within hello specified Data Lake Store Account.</span></span> <BR><span data-ttu-id="4ae9c-124">a {date}, {time}</span><span class="sxs-lookup"><span data-stu-id="4ae9c-124">{date}, {time}</span></span><BR><span data-ttu-id="4ae9c-125">1. példa: mappa1/logs / {date} / {time}</span><span class="sxs-lookup"><span data-stu-id="4ae9c-125">Example 1: folder1/logs/{date}/{time}</span></span><BR><span data-ttu-id="4ae9c-126">2. példa: mappa1/logs / {date}</span><span class="sxs-lookup"><span data-stu-id="4ae9c-126">Example 2: folder1/logs/{date}</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="4ae9c-127">Dátum formátumban [<I>választható</I>]</span><span class="sxs-lookup"><span data-stu-id="4ae9c-127">Date Format [<I>optional</I>]</span></span></td>
<td><span data-ttu-id="4ae9c-128">Hello dátumtokent hello előtag elérési út használata esetén, amelyben a fájlok vannak rendszerezve hello dátumformátum választhatja meg.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-128">If hello date token is used in hello prefix path, you can select hello date format in which your files are organized.</span></span> <span data-ttu-id="4ae9c-129">. Példa: Éééé/hh/nn</span><span class="sxs-lookup"><span data-stu-id="4ae9c-129">Example: YYYY/MM/DD</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="4ae9c-130">Idő formátumot [<I>választható</I>]</span><span class="sxs-lookup"><span data-stu-id="4ae9c-130">Time Format [<I>optional</I>]</span></span></td>
<td><span data-ttu-id="4ae9c-131">Ha hello idő jogkivonat hello előtag elérési útja, adja meg a hello idő formátumban, amelyben a fájlok vannak rendezve.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-131">If hello time token is used in hello prefix path, specify hello time format in which your files are organized.</span></span> <span data-ttu-id="4ae9c-132">Jelenleg csak a támogatott hello értéke HH.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-132">Currently hello only supported value is HH.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="4ae9c-133">Esemény szerializálási formátum</span><span class="sxs-lookup"><span data-stu-id="4ae9c-133">Event Serialization Format</span></span></td>
<td><span data-ttu-id="4ae9c-134">A kimeneti adatok szerializálási formátum.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-134">Serialization format for output data.</span></span> <span data-ttu-id="4ae9c-135">JSON, CSV és az avro-hoz támogatott.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-135">JSON, CSV, and Avro are supported.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="4ae9c-136">Encoding</span><span class="sxs-lookup"><span data-stu-id="4ae9c-136">Encoding</span></span></td>
<td><span data-ttu-id="4ae9c-137">Ha a fürt megosztott kötetei szolgáltatás- vagy JSON formátumú, kódolással meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-137">If CSV or JSON format, an encoding must be specified.</span></span> <span data-ttu-id="4ae9c-138">Az UTF-8 hello csak akkor támogatja a kódolási formátum jelenleg.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-138">UTF-8 is hello only supported encoding format at this time.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="4ae9c-139">Elválasztó</span><span class="sxs-lookup"><span data-stu-id="4ae9c-139">Delimiter</span></span></td>
<td><span data-ttu-id="4ae9c-140">Csak a fürt megosztott kötetei szolgáltatás szerializálási alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-140">Only applicable for CSV serialization.</span></span> <span data-ttu-id="4ae9c-141">A Stream Analytics számos általánosan használt elválasztó karaktert támogatja a CSV-adatok szerializálása során.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-141">Stream Analytics supports a number of common delimiters for serializing CSV data.</span></span> <span data-ttu-id="4ae9c-142">Támogatott értékei vesszővel, a pontosvesszővel válassza el, a terület, a lapon és a függőleges vonal.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-142">Supported values are comma, semicolon, space, tab and vertical bar.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="4ae9c-143">Formátumban</span><span class="sxs-lookup"><span data-stu-id="4ae9c-143">Format</span></span></td>
<td><span data-ttu-id="4ae9c-144">Csak a JSON-szerializálás alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-144">Only applicable for JSON serialization.</span></span> <span data-ttu-id="4ae9c-145">Sorral elválasztott beállítás megadja, hogy hello kimeneti azzal, hogy minden JSON-objektum sortöréssel elválasztva kell formázni.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-145">Line separated specifies that hello output will be formatted by having each JSON object separated by a new line.</span></span> <span data-ttu-id="4ae9c-146">A tömb határozza meg, hogy hello kimeneti lesznek formázva, egy JSON-objektumok tömbjét.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-146">Array specifies that hello output will be formatted as an array of JSON objects.</span></span></td>
</tr>
</tbody>
</table>

## <a name="renew-data-lake-store-authorization"></a><span data-ttu-id="4ae9c-147">Újítsa meg a Data Lake Store engedélyezési</span><span class="sxs-lookup"><span data-stu-id="4ae9c-147">Renew Data Lake Store Authorization</span></span>
<span data-ttu-id="4ae9c-148">Jelenleg egy korlátozás amikor a hello hitelesítésére szolgáló jogkivonat toobe vonatkozó minden Data Lake Store kimenet 90 naponta manuálisan frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-148">Currently, there is a limitation where hello authentication token needs toobe manually refreshed every 90 days for all jobs with Data Lake Store output.</span></span> <span data-ttu-id="4ae9c-149">Konfigurálnia kell toore-hitelesítéséhez a Data Lake Store-fiókot, ha sikeresen módosította jelszavát, mert a feladat meg lett létrehozva, vagy utolsó hitelesítése.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-149">You will also need toore-authenticate your Data Lake Store account if you have changed your password since your job was created or last authenticated.</span></span> <span data-ttu-id="4ae9c-150">A probléma tünete nem feladatkiemenetét és újbóli engedélyezése szükségességét jelző hello műveletnaplók a hiba.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-150">A symptom of this issue is no job output and an error in hello Operation Logs indicating need for re-authorization.</span></span>

<span data-ttu-id="4ae9c-151">tooresolve probléma leállítja a futó feladatot, és nyissa meg a kimeneti tooyour Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-151">tooresolve this issue, stop your running job and go tooyour Data Lake Store output.</span></span> <span data-ttu-id="4ae9c-152">Hello "Megújítási engedélyezési" hivatkozásra, és egy rövid ideig lap jelenik meg, amely jelzi, "Átirányítása tooauthorization...".</span><span class="sxs-lookup"><span data-stu-id="4ae9c-152">Click hello “Renew authorization” link, and for a brief time a page will pop up indicating “Redirecting tooauthorization..”.</span></span> <span data-ttu-id="4ae9c-153">hello lap automatikusan bezárul, és ha sikeres, fogja jelezni, "Engedélyezési sikeresen megújítva".</span><span class="sxs-lookup"><span data-stu-id="4ae9c-153">hello page will automatically close and if successful, will indicate “Authorization has been successfully renewed”.</span></span> <span data-ttu-id="4ae9c-154">Ezután alján hello hello tooclick "Mentés" kell, és indítsa újra a feladatot hello feladat utolsó befejezési időpontja tooavoid adatvesztés lépne.</span><span class="sxs-lookup"><span data-stu-id="4ae9c-154">You then need tooclick “Save” at hello bottom of hello page, and can proceed by restarting your job from hello Last Stopped Time tooavoid data loss.</span></span>

![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-renew-authorization.png)

