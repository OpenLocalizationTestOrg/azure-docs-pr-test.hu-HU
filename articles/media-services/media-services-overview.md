---
title: "aaaAzure Media Services áttekintése |} Microsoft Docs"
description: "Ezen témakör áttekintést nyújt az Azure Media Services szolgáltatásairól"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 7a5e9723-c379-446b-b4d6-d0e41bd7d31f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/04/2017
ms.author: juliako;anilmur
ms.openlocfilehash: 81f9f4d9ff75effea30c10fd09449e9d2025f377
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-media-services-overview"></a><span data-ttu-id="63245-103">Az Azure Media Services áttekintése</span><span class="sxs-lookup"><span data-stu-id="63245-103">Azure Media Services overview</span></span> 

<span data-ttu-id="63245-104">Microsoft Azure Media Services szolgáltatás felhőalapú egy bővíthető platform, amely lehetővé teszi a fejlesztők toobuild méretezhető médiafelügyeleti és -továbbítási alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="63245-104">Microsoft Azure Media Services is an extensible cloud-based platform that enables developers toobuild scalable media management and delivery applications.</span></span> <span data-ttu-id="63245-105">A Media Services alapjai a REST API-k, amelyek lehetővé teszik toosecurely feltöltési, tárolásához, kódolása és video- vagy tartalom csomag mind igény szerinti és élő adatfolyam-továbbítási kézbesítési toovarious ügyfelek (például TV, számítógépek és mobileszközök).</span><span class="sxs-lookup"><span data-stu-id="63245-105">Media Services is based on REST APIs that enable you toosecurely upload, store, encode, and package video or audio content for both on-demand and live streaming delivery toovarious clients (for example, TV, PC, and mobile devices).</span></span>

<span data-ttu-id="63245-106">A Media Services használatával teljes, átfogó munkafolyamatokat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="63245-106">You can build end-to-end workflows using entirely Media Services.</span></span> <span data-ttu-id="63245-107">Másik lehetőségként toouse külső összetevőket a munkafolyamatok bizonyos részeihez.</span><span class="sxs-lookup"><span data-stu-id="63245-107">You can also choose toouse third-party components for some parts of your workflow.</span></span> <span data-ttu-id="63245-108">Például egy harmadik féltől származó kódolóval végezhet kódolást.</span><span class="sxs-lookup"><span data-stu-id="63245-108">For example, encode using a third-party encoder.</span></span> <span data-ttu-id="63245-109">Ezután a Media Services használatával intézheti a feltöltést, a védelmet, a csomagolást és a továbbítást.</span><span class="sxs-lookup"><span data-stu-id="63245-109">Then, upload, protect, package, deliver using Media Services.</span></span>

<span data-ttu-id="63245-110">Válassza ki a toostream élő tartalom, vagy tartalom igény biztosíthat.</span><span class="sxs-lookup"><span data-stu-id="63245-110">You can choose toostream your content live or deliver content on-demand.</span></span> <span data-ttu-id="63245-111">hello témakör ezenkívül hivatkozásokat is tartalmaz, tooother kapcsolódó témakörökre.</span><span class="sxs-lookup"><span data-stu-id="63245-111">hello topic also links tooother relevant topics.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="63245-112">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="63245-112">Media Services learning paths</span></span>
<span data-ttu-id="63245-113">Az AMS képzési terveket itt tekintheti meg:</span><span class="sxs-lookup"><span data-stu-id="63245-113">You can view AMS learning paths here:</span></span>

* [<span data-ttu-id="63245-114">AMS élő adatfolyam-továbbítási munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="63245-114">AMS Live Streaming Workflow</span></span>](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-live/)
* [<span data-ttu-id="63245-115">AMS igényalapú streaming-munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="63245-115">AMS on-Demand Streaming Workflow</span></span>](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)

## <a name="prerequisites"></a><span data-ttu-id="63245-116">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="63245-116">Prerequisites</span></span>

<span data-ttu-id="63245-117">az Azure Media Services toostart hello következő szükséges:</span><span class="sxs-lookup"><span data-stu-id="63245-117">toostart using Azure Media Services, you should have hello following:</span></span>

* <span data-ttu-id="63245-118">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="63245-118">An Azure account.</span></span> <span data-ttu-id="63245-119">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="63245-119">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="63245-120">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="63245-120">For details, see [Azure Free Trial](https://azure.microsoft.com).</span></span>
* <span data-ttu-id="63245-121">Egy Azure Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="63245-121">An Azure Media Services account.</span></span> <span data-ttu-id="63245-122">További információ: [Fiók létrehozása](media-services-portal-create-account.md)</span><span class="sxs-lookup"><span data-stu-id="63245-122">For more information, see [Create Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="63245-123">(Választható:) A fejlesztési környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="63245-123">(Optional) Set up development environment.</span></span> <span data-ttu-id="63245-124">Válassza ki, hogy a .NET vagy a REST API fejlesztési környezetet kívánja-e használni.</span><span class="sxs-lookup"><span data-stu-id="63245-124">Choose .NET or REST API for your development environment.</span></span> <span data-ttu-id="63245-125">További információ: [Környezet beállítása](media-services-dotnet-how-to-use.md)</span><span class="sxs-lookup"><span data-stu-id="63245-125">For more information, see [Set up environment](media-services-dotnet-how-to-use.md).</span></span>

    <span data-ttu-id="63245-126">Emellett ismerje meg, hogyan túl[tooAMS API programozott módon való kapcsolódás](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="63245-126">Also, learn how too[connect  programmatically tooAMS API](media-services-use-aad-auth-to-access-ams-api.md).</span></span>
* <span data-ttu-id="63245-127">Elindított állapotú standard vagy prémium szintű streamvégpont.</span><span class="sxs-lookup"><span data-stu-id="63245-127">A standard or premium streaming endpoint in started state.</span></span>  <span data-ttu-id="63245-128">További információ: [Streamvégpontok felügyelete](media-services-portal-manage-streaming-endpoints.md)</span><span class="sxs-lookup"><span data-stu-id="63245-128">For more information, see [Managing streaming endpoints](media-services-portal-manage-streaming-endpoints.md)</span></span>

## <a name="sdks-and-tools"></a><span data-ttu-id="63245-129">SDK-k és eszközök</span><span class="sxs-lookup"><span data-stu-id="63245-129">SDKs and tools</span></span>

<span data-ttu-id="63245-130">Media Services-megoldások toobuild használhatja:</span><span class="sxs-lookup"><span data-stu-id="63245-130">toobuild Media Services solutions, you can use:</span></span>

* [<span data-ttu-id="63245-131">Media Services REST API</span><span class="sxs-lookup"><span data-stu-id="63245-131">Media Services REST API</span></span>](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference)
* <span data-ttu-id="63245-132">Hello elérhető ügyfél SDK-k közül:</span><span class="sxs-lookup"><span data-stu-id="63245-132">One of hello available client SDKs:</span></span>
    * <span data-ttu-id="63245-133">[.NET-keretrendszerhez készült Azure Media Services SDK](https://github.com/Azure/azure-sdk-for-media-services),</span><span class="sxs-lookup"><span data-stu-id="63245-133">[Azure Media Services SDK for .NET](https://github.com/Azure/azure-sdk-for-media-services),</span></span>
    * <span data-ttu-id="63245-134">[Javához készült Azure SDK](https://github.com/Azure/azure-sdk-for-java),</span><span class="sxs-lookup"><span data-stu-id="63245-134">[Azure SDK for Java](https://github.com/Azure/azure-sdk-for-java),</span></span>
    * <span data-ttu-id="63245-135">[Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php),</span><span class="sxs-lookup"><span data-stu-id="63245-135">[Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php),</span></span>
    * <span data-ttu-id="63245-136">[Node.js-hez készült Azure Media Services](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (Ez a Node.js SDK nem Microsoft által készített verziója.</span><span class="sxs-lookup"><span data-stu-id="63245-136">[Azure Media Services for Node.js](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (This is a non-Microsoft version of a Node.js SDK.</span></span> <span data-ttu-id="63245-137">Ez karbantartását egy Közösség végzi, és jelenleg nem rendelkezik a 100 %-ában használhassák az hello AMS API-k).</span><span class="sxs-lookup"><span data-stu-id="63245-137">It is maintained by a community and currently does not have a 100% coverage of hello AMS APIs).</span></span>
* <span data-ttu-id="63245-138">Meglévő eszközök:</span><span class="sxs-lookup"><span data-stu-id="63245-138">Existing tools:</span></span>
    * [<span data-ttu-id="63245-139">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="63245-139">Azure portal</span></span>](https://portal.azure.com/)
    * <span data-ttu-id="63245-140">[Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Az Azure Media Services Explorer (AMSE) egy Winforms/C#-alkalmazás Windows rendszerre)</span><span class="sxs-lookup"><span data-stu-id="63245-140">[Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Explorer (AMSE) is a Winforms/C# application for Windows)</span></span>

## <a name="concepts-and-overview"></a><span data-ttu-id="63245-141">Alapfogalmak és áttekintés</span><span class="sxs-lookup"><span data-stu-id="63245-141">Concepts and overview</span></span>
<span data-ttu-id="63245-142">Az Azure Media Services alapfogalmaiért lásd: [Fogalmak](media-services-concepts.md)</span><span class="sxs-lookup"><span data-stu-id="63245-142">For Azure Media Services concepts, see [Concepts](media-services-concepts.md).</span></span>

<span data-ttu-id="63245-143">Egy hogyan-tooseries tooall hello fő összetevőit Azure Media Services szolgáltatásban, lásd: [Azure Media Services részletes oktatóprogramjai](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series).</span><span class="sxs-lookup"><span data-stu-id="63245-143">For a how-tooseries that introduces you tooall hello main components of Azure Media Services, see [Azure Media Services Step-by-Step tutorials](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series).</span></span> <span data-ttu-id="63245-144">Az adatsorozat fogalmak áttekintést rendelkezik, és hello AMSE eszköz toodemonstrate AMS feladatok használ.</span><span class="sxs-lookup"><span data-stu-id="63245-144">This series has a great overview of concepts and it uses hello AMSE tool toodemonstrate AMS tasks.</span></span> <span data-ttu-id="63245-145">Az AMSE eszköz egy Windows-eszköz.</span><span class="sxs-lookup"><span data-stu-id="63245-145">AMSE tool is a Windows tool.</span></span> <span data-ttu-id="63245-146">Az eszköz támogatja a legtöbb érhet el programozott módon való hello feladatot [.NET-keretrendszerhez készült AMS SDK](https://github.com/Azure/azure-sdk-for-media-services), [Javához készült Azure SDK](https://github.com/Azure/azure-sdk-for-java), vagy [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php).</span><span class="sxs-lookup"><span data-stu-id="63245-146">This tool supports most of hello tasks you can achieve programmatically with [AMS SDK for .NET](https://github.com/Azure/azure-sdk-for-media-services), [Azure SDK for Java](https://github.com/Azure/azure-sdk-for-java), or  [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php).</span></span>

## <a name="supported-scenarios-and-availability-of-media-services-across-data-centers"></a><span data-ttu-id="63245-147">Támogatott helyzetek és a Media Services rendelkezésre állása az egyes adatközpontokban</span><span class="sxs-lookup"><span data-stu-id="63245-147">Supported scenarios and availability of Media Services across data centers</span></span>

<span data-ttu-id="63245-148">Részletes információkért lásd az [AMS-forgatókönyvek és a funkciók és szolgáltatások az egyes adatközpontokban történő rendelkezésre állását](scenarios-and-availability.md) ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="63245-148">For detailed information, see [AMS scenarios and availability of features and services across data centers](scenarios-and-availability.md).</span></span>

## <a name="service-level-agreement-sla"></a><span data-ttu-id="63245-149">Szolgáltatói szerződés (SLA)</span><span class="sxs-lookup"><span data-stu-id="63245-149">Service Level Agreement (SLA)</span></span>

* <span data-ttu-id="63245-150">A Media Services kódolási funkciójához szükséges REST API-tranzakciókhoz 99,9%-os rendelkezésre állást garantálunk.</span><span class="sxs-lookup"><span data-stu-id="63245-150">For Media Services Encoding, we guarantee 99.9% availability of REST API transactions.</span></span>
* <span data-ttu-id="63245-151">A streamelési funkciók szolgáltatási kéréseinek sikeres kiszolgálására 99,9%-os rendelkezésre állást garantálunk a létező médiatartalmak esetében egy standard vagy prémium szintű streamvégpont megvásárlásakor.</span><span class="sxs-lookup"><span data-stu-id="63245-151">For Streaming, we will successfully service requests with a 99.9% availability guarantee for existing media content when a standard or premium streaming endpoint is purchased.</span></span>
* <span data-ttu-id="63245-152">Az élő csatornák esetében garantáljuk, hogy fut a csatornák lesz külső kapcsolatot hello idő legalább 99,9 %.</span><span class="sxs-lookup"><span data-stu-id="63245-152">For Live Channels, we guarantee that running Channels will have external connectivity at least 99.9% of hello time.</span></span>
* <span data-ttu-id="63245-153">A Content Protection esetében garantáljuk, hogy ában sikeresen teljesítjük kulcs kérelmek legalább 99,9 %-os hello idő.</span><span class="sxs-lookup"><span data-stu-id="63245-153">For Content Protection, we guarantee that we will successfully fulfill key requests at least 99.9% of hello time.</span></span>
* <span data-ttu-id="63245-154">Az indexelő esetében sikeresen szolgálja, hogy az egy kódolási feldolgozott indexelési kéréseket hello idő 99,9 % egység.</span><span class="sxs-lookup"><span data-stu-id="63245-154">For Indexer, we will successfully service Indexer Task requests processed with an Encoding Reserved Unit 99.9% of hello time.</span></span>

<span data-ttu-id="63245-155">További információ: [Microsoft Azure SLA](https://azure.microsoft.com/support/legal/sla/)</span><span class="sxs-lookup"><span data-stu-id="63245-155">For more information, see [Microsoft Azure SLA](https://azure.microsoft.com/support/legal/sla/).</span></span>

<span data-ttu-id="63245-156">Rendelkezésre állás biztosításához az adatközpontok kapcsolatos információkért lásd: hello [Avaiability](scenarios-and-availability.md#availability) szakasz.</span><span class="sxs-lookup"><span data-stu-id="63245-156">For information about availability in datacenters, see hello [Avaiability](scenarios-and-availability.md#availability) section.</span></span>

## <a name="support"></a><span data-ttu-id="63245-157">Támogatás</span><span class="sxs-lookup"><span data-stu-id="63245-157">Support</span></span>

<span data-ttu-id="63245-158">[Az Azure-támogatási szolgáltatások](https://azure.microsoft.com/support/options/) támogatási lehetőségeket biztosítanak az Azure szolgáltatásaival, köztük a Media Services szolgáltatással kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="63245-158">[Azure Support](https://azure.microsoft.com/support/options/) provides support options for Azure, including Media Services.</span></span>

## <a name="provide-feedback"></a><span data-ttu-id="63245-159">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="63245-159">Provide feedback</span></span>

[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
