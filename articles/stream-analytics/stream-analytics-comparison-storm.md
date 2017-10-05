---
title: "Elemzés platformok: Stream Analytics alatt futó Apache Storm összehasonlítása |} Microsoft Docs"
description: "Útmutató a felhőalapú analytics platform kiválasztása az Apache Storm összehasonlítás Stream Analytics segítségével beolvasása. Szolgáltatások és különbségek megértése."
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
ms.openlocfilehash: 97044cb5d7b0b3fcb3b85328df618a265bc59b61
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="choosing-a-streaming-analytics-platform-comparing-apache-storm-and-azure-stream-analytics"></a><span data-ttu-id="36768-105">A folyamatos átviteli analytics platform kiválasztása: az Apache Storm és az Azure Stream Analytics összehasonlítása</span><span class="sxs-lookup"><span data-stu-id="36768-105">Choosing a streaming analytics platform: comparing Apache Storm and Azure Stream Analytics</span></span>
<span data-ttu-id="36768-106">Azure adatfolyam-továbbítási adatok elemzése több megoldást nyújt: [Azure Streaming Analytics](https://docs.microsoft.com/azure/stream-analytics/) és [Azure HDInsight alatt futó Apache Storm](https://azure.microsoft.com/services/hdinsight/apache-storm/).</span><span class="sxs-lookup"><span data-stu-id="36768-106">Azure provides multiple solutions for analyzing streaming data: [Azure Streaming Analytics](https://docs.microsoft.com/azure/stream-analytics/) and [Apache Storm on Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-storm/).</span></span> <span data-ttu-id="36768-107">Mindkét analytics platform előnyei a PaaS megoldást.</span><span class="sxs-lookup"><span data-stu-id="36768-107">Both analytics platforms provide the benefits of a PaaS solution.</span></span> <span data-ttu-id="36768-108">Azonban a platformok jelentős eltérések képességeit, valamint hogyan konfigurálhatja és kezelheti azokat.</span><span class="sxs-lookup"><span data-stu-id="36768-108">But the platforms have some significant differences in their capabilities as well as in how you configure and manage them.</span></span> 

<span data-ttu-id="36768-109">Ez a cikk ismerteti a szolgáltatások segítségével válassza ki az Apache Storm és Azure Stream Analytics felhő analytics platformként között egymás melletti összehasonlítása.</span><span class="sxs-lookup"><span data-stu-id="36768-109">This article provides a side-by-side comparison of features to help you choose between Apache Storm and Azure Stream Analytics as a cloud analytics platform.</span></span> 

## <a name="general-features"></a><span data-ttu-id="36768-110">Általános szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="36768-110">General features</span></span>

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-111">
                    <strong> </strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-111">
                    <strong> </strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p><span data-ttu-id="36768-112">
                    <strong>Az Azure Stream Analytics</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-112">
                    <strong>Azure Stream Analytics</strong>
                </span></span></p>
            </td>
            <td width="246" valign="top">
                <p><span data-ttu-id="36768-113">
                    <strong>A HDInsight alatt futó Apache Storm</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-113">
                    <strong>Apache Storm on HDInsight</strong>
                </span></span></p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-114">
                    <strong>Nyílt forráskódú?</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-114">
                    <strong>Open source?</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="36768-115">Nem.</span><span class="sxs-lookup"><span data-stu-id="36768-115">No.</span></span> <span data-ttu-id="36768-116">Az Azure Stream Analytics a Microsoft védett kínál.</span><span class="sxs-lookup"><span data-stu-id="36768-116">Azure Stream Analytics is a Microsoft proprietary offering.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="36768-117">Igen.</span><span class="sxs-lookup"><span data-stu-id="36768-117">Yes.</span></span> <span data-ttu-id="36768-118">Apache Storm egy olyan licenccel rendelkező Apache technológia.</span><span class="sxs-lookup"><span data-stu-id="36768-118">Apache Storm is an Apache licensed technology.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-119">
                    <strong>A Microsoft támogatási?</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-119">
                    <strong>Microsoft support?</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="36768-120">Igen</span><span class="sxs-lookup"><span data-stu-id="36768-120">Yes</span></span> </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="36768-121">Igen</span><span class="sxs-lookup"><span data-stu-id="36768-121">Yes</span></span> </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-122">
                    <strong>Hardverkövetelmények</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-122">
                    <strong>Hardware requirements</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="36768-123">nincs.</span><span class="sxs-lookup"><span data-stu-id="36768-123">None.</span></span> <span data-ttu-id="36768-124">Az Azure Stream Analytics egy olyan Azure-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="36768-124">Azure Stream Analytics is an Azure Service.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="36768-125">nincs.</span><span class="sxs-lookup"><span data-stu-id="36768-125">None.</span></span> <span data-ttu-id="36768-126">Apache Storm egy olyan Azure-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="36768-126">Apache Storm is an Azure Service.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-127">
                    <strong>Legfelső szintű egység</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-127">
                    <strong>Top-level unit</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="36768-128">Felhasználók telepítheti és figyelheti a folyamatos átviteli feladat.</span><span class="sxs-lookup"><span data-stu-id="36768-128">Users deploy and monitor streaming jobs.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="36768-129">Felhasználók központi telepítése, és figyelje az egész fürt, amelyek több Storm feladatok, valamint egyéb munkaterhelések (beleértve a kötegelt) tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="36768-129">Users deploy and monitor a whole cluster, which can host multiple Storm jobs as well as other workloads (including batch).</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-130">
                    <strong>Tarifacsomag</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-130">
                    <strong>Pricing</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="36768-131">Áron által feldolgozott adatokat és a szükséges, hogy fut-e a feladat óránként streamelési egységek számát.</span><span class="sxs-lookup"><span data-stu-id="36768-131">Priced by volume of data processed and the number of streaming units required per hour that the job is running.</span></span> 
                </p>
                    <p><span data-ttu-id="36768-132">További információkért lásd: <a href="http://azure.microsoft.com/pricing/details/stream-analytics/">Stream Analytics Díjszabásáról</a>.</span><span class="sxs-lookup"><span data-stu-id="36768-132">For more information, see <a href="http://azure.microsoft.com/pricing/details/stream-analytics/">Stream Analytics Pricing</a>.</span></span></p>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="36768-133">A vásárlás fürt-alapú, és fel van töltve, a rendszer fut a fürtön, független telepített feladatok ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="36768-133">The unit of purchase is cluster-based, and is charged based on the time the cluster is running, independent of jobs deployed.</span></span>
                </p>
                <p>
<span data-ttu-id="36768-134">További információkért lásd: <a href="http://azure.microsoft.com/pricing/details/hdinsight/">HDInsight árképzési</a>.</span><span class="sxs-lookup"><span data-stu-id="36768-134">For more information, see <a href="http://azure.microsoft.com/pricing/details/hdinsight/">HDInsight pricing</a>.</span></span>
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="authoring"></a><span data-ttu-id="36768-135">Szerzői</span><span class="sxs-lookup"><span data-stu-id="36768-135">Authoring</span></span>

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-136">
                    <strong> </strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-136">
                    <strong> </strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p><span data-ttu-id="36768-137">
                    <strong>Az Azure Stream Analytics</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-137">
                    <strong>Azure Stream Analytics</strong>
                </span></span></p>
            </td>
            <td width="246" valign="top">
                <p><span data-ttu-id="36768-138">
                    <strong>A HDInsight alatt futó Apache Storm</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-138">
                    <strong>Apache Storm on HDInsight</strong>
                </span></span></p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-139">
                    <strong>Képességek: SQL DSL?</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-139">
                    <strong>Capabilities: SQL DSL?</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="36768-140">Igen.</span><span class="sxs-lookup"><span data-stu-id="36768-140">Yes.</span></span> <span data-ttu-id="36768-141">A Stream Analytics egy SQL-szerű nyelv nyújt átalakítások létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="36768-141">Stream Analytics provides a SQL-like language for creating transformations.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="36768-142">Nem.</span><span class="sxs-lookup"><span data-stu-id="36768-142">No.</span></span> <span data-ttu-id="36768-143">Felhasználók írhat kódot a Java vagy C# vagy Trident az API-kkal.</span><span class="sxs-lookup"><span data-stu-id="36768-143">Users write code in Java or C#, or use Trident APIs.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-144">
                    <strong>Képességek: Az ideiglenes operátorok?</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-144">
                    <strong>Capabilities: Temporal operators?</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="36768-145">Ablakos összesítéseket és az időalapú illesztéseket alapértelmezés szerint támogatott.</span><span class="sxs-lookup"><span data-stu-id="36768-145">Windowed aggregates and temporal joins are supported by default.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="36768-146">Az ideiglenes operátorok a felhasználónak kell végrehajtania.</span><span class="sxs-lookup"><span data-stu-id="36768-146">Temporal operators must be implemented by the user.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-147">
                    <strong>Fejlesztési felület</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-147">
                    <strong>Development experience</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="36768-148">Felhasználók létrehozása, hibakeresési, és figyelje a feladatokat az Azure-portálon keresztül mintaadatok származó élő adatfolyam használata.</span><span class="sxs-lookup"><span data-stu-id="36768-148">Users can create, debug, and monitor jobs through the Azure portal, using sample data derived from a live stream.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="36768-149">Felhasználók a .NET fejlesztés, hibakeresését és figyelése a Visual Studio alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="36768-149">Users using .NET can develop, debug, and monitor through Visual Studio.</span></span> <span data-ttu-id="36768-150">Java vagy más nyelvű felhasználók használhatják az általuk választott IDE.</span><span class="sxs-lookup"><span data-stu-id="36768-150">Users using Java or other languages can use the IDE of their choice.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-151">
                    <strong>Hibaelhárítási támogatás</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-151">
                    <strong>Debugging support</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="36768-152">Alapszintű állapotának és műveleteinek feladatnaplóit elérhetők segítségére hibakeresésben.</span><span class="sxs-lookup"><span data-stu-id="36768-152">Basic job status and operations logs are available to help debug.</span></span> <span data-ttu-id="36768-153">A Stream Analytics jelenleg megadásakor a felhasználók nem adja meg a tartalmat, vagy mekkora a tartalom megtalálható a naplók (azaz részletes üzemmód).</span><span class="sxs-lookup"><span data-stu-id="36768-153">Stream Analytics currently does not let users specify what content or how much content is included in the logs (i.e., verbose mode).</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="36768-154">Részletes naplózási érhetők el.</span><span class="sxs-lookup"><span data-stu-id="36768-154">Detailed logs are available.</span></span> <span data-ttu-id="36768-155">A felhasználók naplók a Visual Studio vagy jelentkezik be a fürt és a naplók elérése közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="36768-155">Users can access logs in Visual Studio or by logging in to the cluster and accessing the logs directly.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-156">
                    <strong>A bővítési egyéni kód használatával?</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-156">
                    <strong>Extensibility using custom code?</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="36768-157">Részlegesen támogatja a JavaScript felhasználó által megadott függvények rendelkező.</span><span class="sxs-lookup"><span data-stu-id="36768-157">Partially support with JavaScript UDFs.</span></span> <span data-ttu-id="36768-158">További információkért lásd: <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-javascript-user-defined-functions">JavaScript UDF integrációs</a>.</span><span class="sxs-lookup"><span data-stu-id="36768-158">For more information, see <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-javascript-user-defined-functions">JavaScript UDF integration</a>.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="36768-159">Igen.</span><span class="sxs-lookup"><span data-stu-id="36768-159">Yes.</span></span> <span data-ttu-id="36768-160">Felhasználók is egyéni kód írását, a C#, Java, vagy bármely más, a Storm támogatott nyelven.</span><span class="sxs-lookup"><span data-stu-id="36768-160">Users can write custom code in C#, Java, or any other language supported on Storm.</span></span>
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="data-sources-inputs-and-outputs"></a><span data-ttu-id="36768-161">Adatforrások (bemeneti) és -kimenetek</span><span class="sxs-lookup"><span data-stu-id="36768-161">Data sources (inputs) and outputs</span></span> ##

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-162">
                    <strong> </strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-162">
                    <strong> </strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p><span data-ttu-id="36768-163">
                    <strong>Az Azure Stream Analytics</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-163">
                    <strong>Azure Stream Analytics</strong>
                </span></span></p>
            </td>
            <td width="246" valign="top">
                <p><span data-ttu-id="36768-164">
                    <strong>A HDInsight alatt futó Apache Storm</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-164">
                    <strong>Apache Storm on HDInsight</strong>
                </span></span></p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-165">
                 <strong>Bemeneti adatforrások</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-165">
                 <strong>Input data sources</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p><span data-ttu-id="36768-166">Az Azure Event Hubs és az Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="36768-166">Azure Event Hubs and Azure Blob storage.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="36768-167">Összekötők Azure Event Hubs, Azure Service Bus, Kafka és több érhetők el.</span><span class="sxs-lookup"><span data-stu-id="36768-167">Connectors are available for Azure Event Hubs, Azure Service Bus, Kafka, and more.</span></span> <span data-ttu-id="36768-168">Felhasználók hozhatnak létre további összekötők egyéni kód használatával.</span><span class="sxs-lookup"><span data-stu-id="36768-168">Users can create additional connectors using custom code.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-169">
                    <strong>A bemeneti adatok formátumok</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-169">
                    <strong>Input data formats</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="36768-170">Az Avro, JSON, CSV</span><span class="sxs-lookup"><span data-stu-id="36768-170">Avro, JSON, CSV</span></span> </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="36768-171">Tetszőleges méretű egyéni kód használatával a felhasználók is végrehajthatják.</span><span class="sxs-lookup"><span data-stu-id="36768-171">Users can implement any format using custom code.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-172">
                    <strong>Kimenetek</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-172">
                    <strong>Outputs</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="36768-173">A folyamatos átviteli feladatnak rendelkezhet több kimenet.</span><span class="sxs-lookup"><span data-stu-id="36768-173">A streaming job can have multiple outputs.</span></span> <span data-ttu-id="36768-174">Támogatott kimenetek az Azure Event Hubs, Azure Blob storage, Azure Table storage, Azure SQL Database és Power bi-ban.</span><span class="sxs-lookup"><span data-stu-id="36768-174">Supported outputs are Azure Event Hubs, Azure Blob storage, Azure Table storage, Azure SQL DB, and Power BI.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="36768-175">Storm sok kimenetek támogatja a topológiában, és minden egyes kimeneti lehet alárendelt feldolgozási egyéni logikát.</span><span class="sxs-lookup"><span data-stu-id="36768-175">Storm supports many outputs in a topology, and each output can have custom logic for downstream processing.</span></span> <span data-ttu-id="36768-176">A Storm összekötők Power bi-ban, az Azure Event Hubs, Azure Blob storage, Azure Cosmos DB, SQL és HBase magában foglalja.</span><span class="sxs-lookup"><span data-stu-id="36768-176">Storm includes connectors for Power BI, Azure Event Hubs, Azure Blob storage, Azure Cosmos DB, SQL, and HBase.</span></span> <span data-ttu-id="36768-177">Felhasználók hozhatnak létre további összekötők egyéni kód használatával.</span><span class="sxs-lookup"><span data-stu-id="36768-177">Users can create additional connectors using custom code.</span></span>    
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-178">
                    <strong>Adatok kódolási formátum</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-178">
                    <strong>Data-encoding formats</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="36768-179">Adatok UTF-8 használatával kell formázni.</span><span class="sxs-lookup"><span data-stu-id="36768-179">Data must be formatted using UTF-8.</span></span>
                </p>
            </td>   
            <td width="246" valign="top">
                <p>
<span data-ttu-id="36768-180">Felhasználók bármilyen egyéni kód használatával adatok kódolási formátum is létrehozható.</span><span class="sxs-lookup"><span data-stu-id="36768-180">Users can implement any data encoding format using custom code.</span></span>
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="management-and-operations"></a><span data-ttu-id="36768-181">Felügyelete és műveletei</span><span class="sxs-lookup"><span data-stu-id="36768-181">Management and operations</span></span> ##

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-182">
                    <strong> </strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-182">
                    <strong> </strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p><span data-ttu-id="36768-183">
                    <strong>Az Azure Stream Analytics</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-183">
                    <strong>Azure Stream Analytics</strong>
                </span></span></p>
            </td>
            <td width="246" valign="top">
                <p><span data-ttu-id="36768-184">
                    <strong>A HDInsight alatt futó Apache Storm</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-184">
                    <strong>Apache Storm on HDInsight</strong>
                </span></span></p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-185">
                    <strong>Feladat telepítési modell</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-185">
                    <strong>Job deployment model</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="36768-186">Azure-portálon, a PowerShell és a REST API-k.</span><span class="sxs-lookup"><span data-stu-id="36768-186">Azure portal, PowerShell, and REST APIs.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="36768-187">Azure-portálon, a PowerShell, a Visual Studio és a REST API-k.</span><span class="sxs-lookup"><span data-stu-id="36768-187">Azure portal, PowerShell, Visual Studio, and REST APIs.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-188">
                    <strong>Monitoring (statisztikák, számlálók, stb.)</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-188">
                    <strong>Monitoring (stats, counters, etc.)</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="36768-189">Figyelése Azure portál és a REST API-k segítségével történik.</span><span class="sxs-lookup"><span data-stu-id="36768-189">Monitoring is implemented using Azure portal and REST APIs.</span></span> <span data-ttu-id="36768-190">Felhasználók is konfigurálhatja az Azure riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="36768-190">Users can also configure Azure alerts.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="36768-191">Figyelés a Storm felhasználói felület és a REST API-k segítségével történik.</span><span class="sxs-lookup"><span data-stu-id="36768-191">Monitoring is implemented using the Storm UI and REST APIs.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-192">
                    <strong>Méretezhetőség</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-192">
                    <strong>Scalability</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="36768-193">Méretezhetőség minden folyamatos átviteli egységek (SUs) számát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="36768-193">Scalability is determined by the number of Streaming Units (SUs) for each job.</span></span> <span data-ttu-id="36768-194">Folyamatos átviteli egységenként legfeljebb 1 MB/másodperc, a maximális 50 egységek dolgozza fel.</span><span class="sxs-lookup"><span data-stu-id="36768-194">Each Streaming Unit processes up to 1 MB/second, with a maximum 50 units.</span></span> <span data-ttu-id="36768-195">További információkért lásd: <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-scale-jobs">átviteli sebesség növelése méretezési</a>.</span><span class="sxs-lookup"><span data-stu-id="36768-195">For more information, see <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-scale-jobs">Scale to increase throughput</a>.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="36768-196">Méretezhetőség a HDInsight alatt futó Storm-fürt a csomópontok száma határozza meg.</span><span class="sxs-lookup"><span data-stu-id="36768-196">Scalability is determined by the number of nodes in the HDInsight Storm cluster.</span></span> <span data-ttu-id="36768-197">A felhasználó Azure kvóta határozza meg a csomópontok száma a felső korlátot.</span><span class="sxs-lookup"><span data-stu-id="36768-197">The top limit on the number of nodes is defined by the user's Azure quota.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-198">
                    <strong>Adatok feldolgozási korlátok</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-198">
                    <strong>Data processing limits</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="36768-199">A felhasználók növelheti az adatok feldolgozása vagy növelésével vagy csökkentésével a Streaming Units számát és a felső korlátja 1 GB/s optimalizálni a költségeket.</span><span class="sxs-lookup"><span data-stu-id="36768-199">Users can increase data processing or optimize costs by increasing or decreasing the number of Streaming Units, with an upper limit of 1 GB/second.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="36768-200">Felhasználók méretezheti fürtméret felfelé vagy lefelé.</span><span class="sxs-lookup"><span data-stu-id="36768-200">Users can scale cluster size up or down.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-201">
                    <strong>Állítsa le vagy folytatása</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-201">
                    <strong>Stop/Resume</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="36768-202">Állítsa le, és indítsa újra az utolsó helyen leállt.</span><span class="sxs-lookup"><span data-stu-id="36768-202">Stop and resume from last place stopped.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="36768-203">Legutóbbi folytatni helyezze leállított vízjel alapján.</span><span class="sxs-lookup"><span data-stu-id="36768-203">Stop and resume from last place stopped based on a watermark.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-204">
                    <strong>Frissítési szolgáltatás és -keretrendszer</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-204">
                    <strong>Service and framework update</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="36768-205">Automatikus javítás állásidő nélkül.</span><span class="sxs-lookup"><span data-stu-id="36768-205">Automatic patching with no downtime.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="36768-206">Automatikus javítás állásidő nélkül.</span><span class="sxs-lookup"><span data-stu-id="36768-206">Automatic patching with no downtime.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-207">
                    <strong>Az üzletmenet folytonossága garantált SLA-k a magas rendelkezésre álló szolgáltatáson keresztül</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-207">
                    <strong>Business continuity through a Highly Available Service with guaranteed SLAs</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <ul>
                <li><span data-ttu-id="36768-208">Szolgáltatásiszint-szerződésben garantált 99,9 %-os hasznos üzemidő</span><span class="sxs-lookup"><span data-stu-id="36768-208">SLA of 99.9% uptime</span></span></li>
                <li><span data-ttu-id="36768-209">A hibák automatikus helyreállítás</span><span class="sxs-lookup"><span data-stu-id="36768-209">Auto-recovery from failures</span></span></li>
                <li><span data-ttu-id="36768-210">Állapot-nyilvántartó az ideiglenes operátorok beépített helyreállítása</span><span class="sxs-lookup"><span data-stu-id="36768-210">Built-in recovery of stateful temporal operators</span></span></li>
                </ul>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="36768-211">Szolgáltatásiszint-szerződésben garantált 99,9 %-os üzemidőt a Storm-fürt.</span><span class="sxs-lookup"><span data-stu-id="36768-211">SLA of 99.9% uptime of the Storm cluster.</span></span> 
                </p>
                <p>
<span data-ttu-id="36768-212">Apache Storm egy hibatűrő adatfolyam platform.</span><span class="sxs-lookup"><span data-stu-id="36768-212">Apache Storm is a fault-tolerant streaming platform.</span></span> <span data-ttu-id="36768-213">Azonban a felhasználó felelőssége, hogy győződjön meg arról, hogy a folyamatos átviteli feladat futtatása folyamatos.</span><span class="sxs-lookup"><span data-stu-id="36768-213">However, it is the user's responsibility to ensure that streaming jobs run uninterrupted.</span></span>
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="advanced-features"></a><span data-ttu-id="36768-214">Speciális funkciók</span><span class="sxs-lookup"><span data-stu-id="36768-214">Advanced features</span></span> ##

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-215">
                    <strong> </strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-215">
                    <strong> </strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p><span data-ttu-id="36768-216">
                    <strong>Az Azure Stream Analytics</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-216">
                    <strong>Azure Stream Analytics</strong>
                </span></span></p>
            </td>
            <td width="246" valign="top">
                <p><span data-ttu-id="36768-217">
                    <strong>A HDInsight alatt futó Apache Storm</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-217">
                    <strong>Apache Storm on HDInsight</strong>
                </span></span></p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-218">
                    <strong>Késő érkezés és soron eseménykezelésnek</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-218">
                    <strong>Late arrival and out-of-order event handling</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="36768-219">Beépített konfigurálható házirendek események sorrendjének módosításához, dobja el az események, vagy állítsa be az esemény időpontja.</span><span class="sxs-lookup"><span data-stu-id="36768-219">Built-in configurable policies can reorder events, drop events, or adjust event time.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="36768-220">Felhasználók logika ebben a forgatókönyvben kezeléséhez meg kell valósítania.</span><span class="sxs-lookup"><span data-stu-id="36768-220">Users must implement logic to handle this scenario.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-221">
                    <strong>Referenciaadatok</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-221">
                    <strong>Reference data</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="36768-222">Az Azure Blob storage egy legfeljebb 100 MB memória gyorsítótárának referenciaadatok érhető el.</span><span class="sxs-lookup"><span data-stu-id="36768-222">Reference data is available from Azure Blob storage with a maximum of 100 MB of in-memory cache.</span></span> <span data-ttu-id="36768-223">Hivatkozás az adatok a szolgáltatás frissítése.</span><span class="sxs-lookup"><span data-stu-id="36768-223">Reference data is refreshed by the service.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="36768-224">Nem határoz meg az adatok mérete.</span><span class="sxs-lookup"><span data-stu-id="36768-224">No limits on data size.</span></span> <span data-ttu-id="36768-225">Összekötők HBase, az Azure Cosmos adatbázis, az SQL Server és az Azure érhetők el.</span><span class="sxs-lookup"><span data-stu-id="36768-225">Connectors are available for HBase, Azure Cosmos DB, SQL Server, and Azure.</span></span> <span data-ttu-id="36768-226">Felhasználók hozhatnak létre további összekötők egyéni kód használatával.</span><span class="sxs-lookup"><span data-stu-id="36768-226">Users can create additional connectors using custom code.</span></span> <span data-ttu-id="36768-227">Referenciaadatok frissíteni kell az egyéni kód használatával.</span><span class="sxs-lookup"><span data-stu-id="36768-227">Reference data must be refreshed using custom code.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="36768-228">
                    <strong>Integráció a gépi tanulás</strong>
                </span><span class="sxs-lookup"><span data-stu-id="36768-228">
                    <strong>Integration with Machine Learning</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="36768-229">Közzétett Azure Machine Learning modellek konfigurálhatók funkciók feladat létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="36768-229">Published Azure Machine Learning models can be configured as functions during job creation.</span></span> <span data-ttu-id="36768-230">További információkért lásd: <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-scale-with-machine-learning-functions">skálája a Machine Learning funkciók</a>.</span><span class="sxs-lookup"><span data-stu-id="36768-230">For more information, see <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-scale-with-machine-learning-functions">Scale for Machine Learning functions</a>.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="36768-231">A Storm Boltokhoz keresztül elérhető.</span><span class="sxs-lookup"><span data-stu-id="36768-231">Available through Storm Bolts.</span></span>
                </p>
            </td>
        </tr>
    </tbody>
</table>
