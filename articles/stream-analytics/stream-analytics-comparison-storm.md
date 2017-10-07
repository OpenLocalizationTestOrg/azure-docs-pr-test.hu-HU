---
title: "Elemzés platformok: Apache Storm összehasonlító tooStream Analytics |} Microsoft Docs"
description: "Útmutató a felhőalapú analytics platform kiválasztása az Apache Storm összehasonlító tooStream Analytics segítségével beolvasása. Szolgáltatások és különbségek megértése."
keywords: "elemzési platformot, analytics platform, elemzés felhőplatform, storm összehasonlítása"
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: b9aac017-9866-4d0a-b98f-6f03881e9339
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/27/2017
ms.author: samacha
ms.openlocfilehash: 5a0ec5b2439596f0da962f04b776472031660062
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="choosing-a-streaming-analytics-platform-comparing-apache-storm-and-azure-stream-analytics"></a><span data-ttu-id="d0ffd-105">A folyamatos átviteli analytics platform kiválasztása: az Apache Storm és az Azure Stream Analytics összehasonlítása</span><span class="sxs-lookup"><span data-stu-id="d0ffd-105">Choosing a streaming analytics platform: comparing Apache Storm and Azure Stream Analytics</span></span>
<span data-ttu-id="d0ffd-106">Azure adatfolyam-továbbítási adatok elemzése több megoldást nyújt: [Azure Streaming Analytics](https://docs.microsoft.com/azure/stream-analytics/) és [Azure HDInsight alatt futó Apache Storm](https://azure.microsoft.com/services/hdinsight/apache-storm/).</span><span class="sxs-lookup"><span data-stu-id="d0ffd-106">Azure provides multiple solutions for analyzing streaming data: [Azure Streaming Analytics](https://docs.microsoft.com/azure/stream-analytics/) and [Apache Storm on Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-storm/).</span></span> <span data-ttu-id="d0ffd-107">Mindkét analytics platform előnyei hello a PaaS megoldás.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-107">Both analytics platforms provide hello benefits of a PaaS solution.</span></span> <span data-ttu-id="d0ffd-108">De hello platformok jelentős eltérések képességeit, valamint hogyan konfigurálhatja és kezelheti azokat.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-108">But hello platforms have some significant differences in their capabilities as well as in how you configure and manage them.</span></span> 

<span data-ttu-id="d0ffd-109">Ez a cikk választhat alatt futó Apache Storm és Azure Stream Analytics felhő analytics platformként között szolgáltatások toohelp egymás melletti összehasonlítását tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-109">This article provides a side-by-side comparison of features toohelp you choose between Apache Storm and Azure Stream Analytics as a cloud analytics platform.</span></span> 

## <a name="general-features"></a><span data-ttu-id="d0ffd-110">Általános szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="d0ffd-110">General features</span></span>

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-111">
                    <strong> </strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-111">
                    <strong> </strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p><span data-ttu-id="d0ffd-112">
                    <strong>Az Azure Stream Analytics</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-112">
                    <strong>Azure Stream Analytics</strong>
                </span></span></p>
            </td>
            <td width="246" valign="top">
                <p><span data-ttu-id="d0ffd-113">
                    <strong>A HDInsight alatt futó Apache Storm</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-113">
                    <strong>Apache Storm on HDInsight</strong>
                </span></span></p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-114">
                    <strong>Nyílt forráskódú?</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-114">
                    <strong>Open source?</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="d0ffd-115">Nem.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-115">No.</span></span> <span data-ttu-id="d0ffd-116">Az Azure Stream Analytics a Microsoft védett kínál.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-116">Azure Stream Analytics is a Microsoft proprietary offering.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="d0ffd-117">Igen.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-117">Yes.</span></span> <span data-ttu-id="d0ffd-118">Apache Storm egy olyan licenccel rendelkező Apache technológia.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-118">Apache Storm is an Apache licensed technology.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-119">
                    <strong>A Microsoft támogatási?</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-119">
                    <strong>Microsoft support?</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="d0ffd-120">Igen</span><span class="sxs-lookup"><span data-stu-id="d0ffd-120">Yes</span></span> </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="d0ffd-121">Igen</span><span class="sxs-lookup"><span data-stu-id="d0ffd-121">Yes</span></span> </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-122">
                    <strong>Hardverkövetelmények</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-122">
                    <strong>Hardware requirements</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="d0ffd-123">nincs.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-123">None.</span></span> <span data-ttu-id="d0ffd-124">Az Azure Stream Analytics egy olyan Azure-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-124">Azure Stream Analytics is an Azure Service.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="d0ffd-125">nincs.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-125">None.</span></span> <span data-ttu-id="d0ffd-126">Apache Storm egy olyan Azure-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-126">Apache Storm is an Azure Service.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-127">
                    <strong>Legfelső szintű egység</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-127">
                    <strong>Top-level unit</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="d0ffd-128">Felhasználók telepítheti és figyelheti a folyamatos átviteli feladat.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-128">Users deploy and monitor streaming jobs.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="d0ffd-129">Felhasználók központi telepítése, és figyelje az egész fürt, amelyek több Storm feladatok, valamint egyéb munkaterhelések (beleértve a kötegelt) tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-129">Users deploy and monitor a whole cluster, which can host multiple Storm jobs as well as other workloads (including batch).</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-130">
                    <strong>Tarifacsomag</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-130">
                    <strong>Pricing</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="d0ffd-131">Által feldolgozott adatmennyiség áron és hello adatfolyam-továbbítási egységek száma óránként szükséges hello feladat fut-e.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-131">Priced by volume of data processed and hello number of streaming units required per hour that hello job is running.</span></span> 
                </p>
                    <p><span data-ttu-id="d0ffd-132">További információkért lásd: <a href="http://azure.microsoft.com/pricing/details/stream-analytics/">Stream Analytics Díjszabásáról</a>.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-132">For more information, see <a href="http://azure.microsoft.com/pricing/details/stream-analytics/">Stream Analytics Pricing</a>.</span></span></p>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="d0ffd-133">hello beszerzési fürt-alapú, és hello fürt fut, független telepített feladatok hello idővel feladata alapján.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-133">hello unit of purchase is cluster-based, and is charged based on hello time hello cluster is running, independent of jobs deployed.</span></span>
                </p>
                <p>
<span data-ttu-id="d0ffd-134">További információkért lásd: <a href="http://azure.microsoft.com/pricing/details/hdinsight/">HDInsight árképzési</a>.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-134">For more information, see <a href="http://azure.microsoft.com/pricing/details/hdinsight/">HDInsight pricing</a>.</span></span>
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="authoring"></a><span data-ttu-id="d0ffd-135">Szerzői</span><span class="sxs-lookup"><span data-stu-id="d0ffd-135">Authoring</span></span>

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-136">
                    <strong> </strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-136">
                    <strong> </strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p><span data-ttu-id="d0ffd-137">
                    <strong>Az Azure Stream Analytics</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-137">
                    <strong>Azure Stream Analytics</strong>
                </span></span></p>
            </td>
            <td width="246" valign="top">
                <p><span data-ttu-id="d0ffd-138">
                    <strong>A HDInsight alatt futó Apache Storm</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-138">
                    <strong>Apache Storm on HDInsight</strong>
                </span></span></p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-139">
                    <strong>Képességek: SQL DSL?</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-139">
                    <strong>Capabilities: SQL DSL?</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="d0ffd-140">Igen.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-140">Yes.</span></span> <span data-ttu-id="d0ffd-141">A Stream Analytics egy SQL-szerű nyelv nyújt átalakítások létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-141">Stream Analytics provides a SQL-like language for creating transformations.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="d0ffd-142">Nem.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-142">No.</span></span> <span data-ttu-id="d0ffd-143">Felhasználók írhat kódot a Java vagy C# vagy Trident az API-kkal.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-143">Users write code in Java or C#, or use Trident APIs.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-144">
                    <strong>Képességek: Az ideiglenes operátorok?</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-144">
                    <strong>Capabilities: Temporal operators?</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="d0ffd-145">Ablakos összesítéseket és az időalapú illesztéseket alapértelmezés szerint támogatott.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-145">Windowed aggregates and temporal joins are supported by default.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="d0ffd-146">Az ideiglenes operátorok hello felhasználónak kell végrehajtania.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-146">Temporal operators must be implemented by hello user.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-147">
                    <strong>Fejlesztési felület</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-147">
                    <strong>Development experience</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="d0ffd-148">Felhasználók létrehozása, hibakeresési, és figyelje a feladatokat a hello Azure-portálon keresztül mintaadatok származó élő adatfolyam használata.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-148">Users can create, debug, and monitor jobs through hello Azure portal, using sample data derived from a live stream.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="d0ffd-149">Felhasználók a .NET fejlesztés, hibakeresését és figyelése a Visual Studio alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-149">Users using .NET can develop, debug, and monitor through Visual Studio.</span></span> <span data-ttu-id="d0ffd-150">Java vagy egyéb nyelvek felhasználók használhatják az általuk választott IDE hello.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-150">Users using Java or other languages can use hello IDE of their choice.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-151">
                    <strong>Hibaelhárítási támogatás</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-151">
                    <strong>Debugging support</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="d0ffd-152">Alapszintű állapotának és műveleteinek feladatnaplóit elérhető toohelp hibakeresési.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-152">Basic job status and operations logs are available toohelp debug.</span></span> <span data-ttu-id="d0ffd-153">A Stream Analytics jelenleg megadásakor a felhasználók nem adja meg a tartalmat, vagy mekkora a tartalom megtalálható hello naplók (azaz részletes üzemmód).</span><span class="sxs-lookup"><span data-stu-id="d0ffd-153">Stream Analytics currently does not let users specify what content or how much content is included in hello logs (i.e., verbose mode).</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="d0ffd-154">Részletes naplózási érhetők el.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-154">Detailed logs are available.</span></span> <span data-ttu-id="d0ffd-155">A felhasználók naplók a Visual Studio vagy toohello fürt naplózás és hello naplók elérése közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-155">Users can access logs in Visual Studio or by logging in toohello cluster and accessing hello logs directly.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-156">
                    <strong>A bővítési egyéni kód használatával?</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-156">
                    <strong>Extensibility using custom code?</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="d0ffd-157">Részlegesen támogatja a JavaScript felhasználó által megadott függvények rendelkező.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-157">Partially support with JavaScript UDFs.</span></span> <span data-ttu-id="d0ffd-158">További információkért lásd: <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-javascript-user-defined-functions">JavaScript UDF integrációs</a>.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-158">For more information, see <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-javascript-user-defined-functions">JavaScript UDF integration</a>.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="d0ffd-159">Igen.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-159">Yes.</span></span> <span data-ttu-id="d0ffd-160">Felhasználók is egyéni kód írását, a C#, Java, vagy bármely más, a Storm támogatott nyelven.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-160">Users can write custom code in C#, Java, or any other language supported on Storm.</span></span>
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="data-sources-inputs-and-outputs"></a><span data-ttu-id="d0ffd-161">Adatforrások (bemeneti) és -kimenetek</span><span class="sxs-lookup"><span data-stu-id="d0ffd-161">Data sources (inputs) and outputs</span></span> ##

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-162">
                    <strong> </strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-162">
                    <strong> </strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p><span data-ttu-id="d0ffd-163">
                    <strong>Az Azure Stream Analytics</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-163">
                    <strong>Azure Stream Analytics</strong>
                </span></span></p>
            </td>
            <td width="246" valign="top">
                <p><span data-ttu-id="d0ffd-164">
                    <strong>A HDInsight alatt futó Apache Storm</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-164">
                    <strong>Apache Storm on HDInsight</strong>
                </span></span></p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-165">
                 <strong>Bemeneti adatforrások</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-165">
                 <strong>Input data sources</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p><span data-ttu-id="d0ffd-166">Az Azure Event Hubs és az Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-166">Azure Event Hubs and Azure Blob storage.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="d0ffd-167">Összekötők Azure Event Hubs, Azure Service Bus, Kafka és több érhetők el.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-167">Connectors are available for Azure Event Hubs, Azure Service Bus, Kafka, and more.</span></span> <span data-ttu-id="d0ffd-168">Felhasználók hozhatnak létre további összekötők egyéni kód használatával.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-168">Users can create additional connectors using custom code.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-169">
                    <strong>A bemeneti adatok formátumok</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-169">
                    <strong>Input data formats</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="d0ffd-170">Az Avro, JSON, CSV</span><span class="sxs-lookup"><span data-stu-id="d0ffd-170">Avro, JSON, CSV</span></span> </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="d0ffd-171">Tetszőleges méretű egyéni kód használatával a felhasználók is végrehajthatják.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-171">Users can implement any format using custom code.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-172">
                    <strong>Kimenetek</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-172">
                    <strong>Outputs</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="d0ffd-173">A folyamatos átviteli feladatnak rendelkezhet több kimenet.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-173">A streaming job can have multiple outputs.</span></span> <span data-ttu-id="d0ffd-174">Támogatott kimenetek az Azure Event Hubs, Azure Blob storage, Azure Table storage, Azure SQL Database és Power bi-ban.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-174">Supported outputs are Azure Event Hubs, Azure Blob storage, Azure Table storage, Azure SQL DB, and Power BI.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="d0ffd-175">Storm sok kimenetek támogatja a topológiában, és minden egyes kimeneti lehet alárendelt feldolgozási egyéni logikát.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-175">Storm supports many outputs in a topology, and each output can have custom logic for downstream processing.</span></span> <span data-ttu-id="d0ffd-176">A Storm összekötők Power bi-ban, az Azure Event Hubs, Azure Blob storage, Azure Cosmos DB, SQL és HBase magában foglalja.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-176">Storm includes connectors for Power BI, Azure Event Hubs, Azure Blob storage, Azure Cosmos DB, SQL, and HBase.</span></span> <span data-ttu-id="d0ffd-177">Felhasználók hozhatnak létre további összekötők egyéni kód használatával.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-177">Users can create additional connectors using custom code.</span></span>    
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-178">
                    <strong>Adatok kódolási formátum</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-178">
                    <strong>Data-encoding formats</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="d0ffd-179">Adatok UTF-8 használatával kell formázni.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-179">Data must be formatted using UTF-8.</span></span>
                </p>
            </td>   
            <td width="246" valign="top">
                <p>
<span data-ttu-id="d0ffd-180">Felhasználók bármilyen egyéni kód használatával adatok kódolási formátum is létrehozható.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-180">Users can implement any data encoding format using custom code.</span></span>
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="management-and-operations"></a><span data-ttu-id="d0ffd-181">Felügyelete és műveletei</span><span class="sxs-lookup"><span data-stu-id="d0ffd-181">Management and operations</span></span> ##

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-182">
                    <strong> </strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-182">
                    <strong> </strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p><span data-ttu-id="d0ffd-183">
                    <strong>Az Azure Stream Analytics</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-183">
                    <strong>Azure Stream Analytics</strong>
                </span></span></p>
            </td>
            <td width="246" valign="top">
                <p><span data-ttu-id="d0ffd-184">
                    <strong>A HDInsight alatt futó Apache Storm</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-184">
                    <strong>Apache Storm on HDInsight</strong>
                </span></span></p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-185">
                    <strong>Feladat telepítési modell</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-185">
                    <strong>Job deployment model</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="d0ffd-186">Azure-portálon, a PowerShell és a REST API-k.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-186">Azure portal, PowerShell, and REST APIs.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="d0ffd-187">Azure-portálon, a PowerShell, a Visual Studio és a REST API-k.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-187">Azure portal, PowerShell, Visual Studio, and REST APIs.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-188">
                    <strong>Monitoring (statisztikák, számlálók, stb.)</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-188">
                    <strong>Monitoring (stats, counters, etc.)</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="d0ffd-189">Figyelése Azure portál és a REST API-k segítségével történik.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-189">Monitoring is implemented using Azure portal and REST APIs.</span></span> <span data-ttu-id="d0ffd-190">Felhasználók is konfigurálhatja az Azure riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-190">Users can also configure Azure alerts.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="d0ffd-191">Figyelési hello Storm felhasználói felülete és a REST API-k segítségével történik.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-191">Monitoring is implemented using hello Storm UI and REST APIs.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-192">
                    <strong>Méretezhetőség</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-192">
                    <strong>Scalability</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="d0ffd-193">Méretezhetőség hello minden folyamatos átviteli egységek (SUs) számát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-193">Scalability is determined by hello number of Streaming Units (SUs) for each job.</span></span> <span data-ttu-id="d0ffd-194">Minden Streaming Unit dolgozza fel a too1 mentése MB/másodperc, a maximális 50 egységek használatával.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-194">Each Streaming Unit processes up too1 MB/second, with a maximum 50 units.</span></span> <span data-ttu-id="d0ffd-195">További információkért lásd: <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-scale-jobs">méretezési tooincrease átviteli</a>.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-195">For more information, see <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-scale-jobs">Scale tooincrease throughput</a>.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="d0ffd-196">Méretezhetőség hello hello HDInsight alatt futó Storm-fürt a csomópontok száma határozza meg.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-196">Scalability is determined by hello number of nodes in hello HDInsight Storm cluster.</span></span> <span data-ttu-id="d0ffd-197">hello felső korlátot hello csomópontok hello felhasználói Azure kvóta határozzák meg.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-197">hello top limit on hello number of nodes is defined by hello user's Azure quota.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-198">
                    <strong>Adatok feldolgozási korlátok</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-198">
                    <strong>Data processing limits</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="d0ffd-199">Felhasználók növelheti az adatok feldolgozása vagy optimalizálni a költségeket növelésével vagy csökkentésével hello és a felső korlátja 1 GB/s Streaming Units számát.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-199">Users can increase data processing or optimize costs by increasing or decreasing hello number of Streaming Units, with an upper limit of 1 GB/second.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="d0ffd-200">Felhasználók méretezheti fürtméret felfelé vagy lefelé.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-200">Users can scale cluster size up or down.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-201">
                    <strong>Állítsa le vagy folytatása</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-201">
                    <strong>Stop/Resume</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="d0ffd-202">Állítsa le, és indítsa újra az utolsó helyen leállt.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-202">Stop and resume from last place stopped.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="d0ffd-203">Legutóbbi folytatni helyezze leállított vízjel alapján.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-203">Stop and resume from last place stopped based on a watermark.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-204">
                    <strong>Frissítési szolgáltatás és -keretrendszer</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-204">
                    <strong>Service and framework update</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="d0ffd-205">Automatikus javítás állásidő nélkül.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-205">Automatic patching with no downtime.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="d0ffd-206">Automatikus javítás állásidő nélkül.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-206">Automatic patching with no downtime.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-207">
                    <strong>Az üzletmenet folytonossága garantált SLA-k a magas rendelkezésre álló szolgáltatáson keresztül</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-207">
                    <strong>Business continuity through a Highly Available Service with guaranteed SLAs</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <ul>
                <li><span data-ttu-id="d0ffd-208">Szolgáltatásiszint-szerződésben garantált 99,9 %-os hasznos üzemidő</span><span class="sxs-lookup"><span data-stu-id="d0ffd-208">SLA of 99.9% uptime</span></span></li>
                <li><span data-ttu-id="d0ffd-209">A hibák automatikus helyreállítás</span><span class="sxs-lookup"><span data-stu-id="d0ffd-209">Auto-recovery from failures</span></span></li>
                <li><span data-ttu-id="d0ffd-210">Állapot-nyilvántartó az ideiglenes operátorok beépített helyreállítása</span><span class="sxs-lookup"><span data-stu-id="d0ffd-210">Built-in recovery of stateful temporal operators</span></span></li>
                </ul>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="d0ffd-211">Szolgáltatásiszint-szerződésben garantált 99,9 %-os üzemidőt hello Storm-fürt.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-211">SLA of 99.9% uptime of hello Storm cluster.</span></span> 
                </p>
                <p>
<span data-ttu-id="d0ffd-212">Apache Storm egy hibatűrő adatfolyam platform.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-212">Apache Storm is a fault-tolerant streaming platform.</span></span> <span data-ttu-id="d0ffd-213">Azonban, hogy a folyamatos átviteli feladatok folyamatos Futtatás hello felhasználó felelőssége tooensure.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-213">However, it is hello user's responsibility tooensure that streaming jobs run uninterrupted.</span></span>
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="advanced-features"></a><span data-ttu-id="d0ffd-214">Speciális funkciók</span><span class="sxs-lookup"><span data-stu-id="d0ffd-214">Advanced features</span></span> ##

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-215">
                    <strong> </strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-215">
                    <strong> </strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p><span data-ttu-id="d0ffd-216">
                    <strong>Az Azure Stream Analytics</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-216">
                    <strong>Azure Stream Analytics</strong>
                </span></span></p>
            </td>
            <td width="246" valign="top">
                <p><span data-ttu-id="d0ffd-217">
                    <strong>A HDInsight alatt futó Apache Storm</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-217">
                    <strong>Apache Storm on HDInsight</strong>
                </span></span></p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-218">
                    <strong>Késő érkezés és soron eseménykezelésnek</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-218">
                    <strong>Late arrival and out-of-order event handling</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="d0ffd-219">Beépített konfigurálható házirendek események sorrendjének módosításához, dobja el az események, vagy állítsa be az esemény időpontja.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-219">Built-in configurable policies can reorder events, drop events, or adjust event time.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="d0ffd-220">Felhasználók meg kell valósítania logika toohandle ebben a forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-220">Users must implement logic toohandle this scenario.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-221">
                    <strong>Referenciaadatok</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-221">
                    <strong>Reference data</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="d0ffd-222">Az Azure Blob storage egy legfeljebb 100 MB memória gyorsítótárának referenciaadatok érhető el.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-222">Reference data is available from Azure Blob storage with a maximum of 100 MB of in-memory cache.</span></span> <span data-ttu-id="d0ffd-223">Referenciaadatok hello szolgáltatás által frissül.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-223">Reference data is refreshed by hello service.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="d0ffd-224">Nem határoz meg az adatok mérete.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-224">No limits on data size.</span></span> <span data-ttu-id="d0ffd-225">Összekötők HBase, az Azure Cosmos adatbázis, az SQL Server és az Azure érhetők el.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-225">Connectors are available for HBase, Azure Cosmos DB, SQL Server, and Azure.</span></span> <span data-ttu-id="d0ffd-226">Felhasználók hozhatnak létre további összekötők egyéni kód használatával.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-226">Users can create additional connectors using custom code.</span></span> <span data-ttu-id="d0ffd-227">Referenciaadatok frissíteni kell az egyéni kód használatával.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-227">Reference data must be refreshed using custom code.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="d0ffd-228">
                    <strong>Integráció a gépi tanulás</strong>
                </span><span class="sxs-lookup"><span data-stu-id="d0ffd-228">
                    <strong>Integration with Machine Learning</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="d0ffd-229">Közzétett Azure Machine Learning modellek konfigurálhatók funkciók feladat létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-229">Published Azure Machine Learning models can be configured as functions during job creation.</span></span> <span data-ttu-id="d0ffd-230">További információkért lásd: <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-scale-with-machine-learning-functions">skálája a Machine Learning funkciók</a>.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-230">For more information, see <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-scale-with-machine-learning-functions">Scale for Machine Learning functions</a>.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="d0ffd-231">A Storm Boltokhoz keresztül elérhető.</span><span class="sxs-lookup"><span data-stu-id="d0ffd-231">Available through Storm Bolts.</span></span>
                </p>
            </td>
        </tr>
    </tbody>
</table>
