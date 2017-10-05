---
title: "Az Azure Media Services áttekintése | Microsoft Docs"
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
ms.openlocfilehash: 2a175aed40b9775d9a4de6877eb3467b43819568
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-media-services-overview"></a><span data-ttu-id="b2159-103">Az Azure Media Services áttekintése</span><span class="sxs-lookup"><span data-stu-id="b2159-103">Azure Media Services overview</span></span> 

<span data-ttu-id="b2159-104">Microsoft Azure Media Services egy bővíthető, felhőalapú platform, amely lehetővé teszi a fejlesztők számára méretezhető médiafelügyeleti és -továbbítási alkalmazások létrehozását.</span><span class="sxs-lookup"><span data-stu-id="b2159-104">Microsoft Azure Media Services is an extensible cloud-based platform that enables developers to build scalable media management and delivery applications.</span></span> <span data-ttu-id="b2159-105">A Media Services alapjai a REST API-k, amelyek lehetővé teszik különböző videó- és audiotartalmak feltöltését, tárolását, kódolását és becsomagolását, majd igény szerinti és élő adatfolyamként történő továbbítását különböző ügyfelek részére (például tévékészülékekre, számítógépekre és mobileszközökre).</span><span class="sxs-lookup"><span data-stu-id="b2159-105">Media Services is based on REST APIs that enable you to securely upload, store, encode, and package video or audio content for both on-demand and live streaming delivery to various clients (for example, TV, PC, and mobile devices).</span></span>

<span data-ttu-id="b2159-106">A Media Services használatával teljes, átfogó munkafolyamatokat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="b2159-106">You can build end-to-end workflows using entirely Media Services.</span></span> <span data-ttu-id="b2159-107">A munkafolyamatok bizonyos részeihez harmadik féltől származó összetevőket is szabadon felhasználhat.</span><span class="sxs-lookup"><span data-stu-id="b2159-107">You can also choose to use third-party components for some parts of your workflow.</span></span> <span data-ttu-id="b2159-108">Például egy harmadik féltől származó kódolóval végezhet kódolást.</span><span class="sxs-lookup"><span data-stu-id="b2159-108">For example, encode using a third-party encoder.</span></span> <span data-ttu-id="b2159-109">Ezután a Media Services használatával intézheti a feltöltést, a védelmet, a csomagolást és a továbbítást.</span><span class="sxs-lookup"><span data-stu-id="b2159-109">Then, upload, protect, package, deliver using Media Services.</span></span>

<span data-ttu-id="b2159-110">A tartalmakat továbbíthatja streamként, illetve az ügyfelek igénye szerint is.</span><span class="sxs-lookup"><span data-stu-id="b2159-110">You can choose to stream your content live or deliver content on-demand.</span></span> <span data-ttu-id="b2159-111">Emellett a témakör hivatkozásokat is tartalmaz egyéb, kapcsolódó témakörökre.</span><span class="sxs-lookup"><span data-stu-id="b2159-111">The topic also links to other relevant topics.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="b2159-112">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="b2159-112">Media Services learning paths</span></span>
<span data-ttu-id="b2159-113">Az AMS képzési terveket itt tekintheti meg:</span><span class="sxs-lookup"><span data-stu-id="b2159-113">You can view AMS learning paths here:</span></span>

* [<span data-ttu-id="b2159-114">AMS élő adatfolyam-továbbítási munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="b2159-114">AMS Live Streaming Workflow</span></span>](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-live/)
* [<span data-ttu-id="b2159-115">AMS igényalapú streaming-munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="b2159-115">AMS on-Demand Streaming Workflow</span></span>](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)

## <a name="prerequisites"></a><span data-ttu-id="b2159-116">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b2159-116">Prerequisites</span></span>

<span data-ttu-id="b2159-117">Az Azure Media Services használatának megkezdéséhez rendelkeznie kell a következőkkel:</span><span class="sxs-lookup"><span data-stu-id="b2159-117">To start using Azure Media Services, you should have the following:</span></span>

* <span data-ttu-id="b2159-118">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="b2159-118">An Azure account.</span></span> <span data-ttu-id="b2159-119">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="b2159-119">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="b2159-120">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="b2159-120">For details, see [Azure Free Trial](https://azure.microsoft.com).</span></span>
* <span data-ttu-id="b2159-121">Egy Azure Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="b2159-121">An Azure Media Services account.</span></span> <span data-ttu-id="b2159-122">További információ: [Fiók létrehozása](media-services-portal-create-account.md)</span><span class="sxs-lookup"><span data-stu-id="b2159-122">For more information, see [Create Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="b2159-123">(Választható:) A fejlesztési környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="b2159-123">(Optional) Set up development environment.</span></span> <span data-ttu-id="b2159-124">Válassza ki, hogy a .NET vagy a REST API fejlesztési környezetet kívánja-e használni.</span><span class="sxs-lookup"><span data-stu-id="b2159-124">Choose .NET or REST API for your development environment.</span></span> <span data-ttu-id="b2159-125">További információ: [Környezet beállítása](media-services-dotnet-how-to-use.md)</span><span class="sxs-lookup"><span data-stu-id="b2159-125">For more information, see [Set up environment](media-services-dotnet-how-to-use.md).</span></span>

    <span data-ttu-id="b2159-126">Emellett megismerheti az [AMS API-hoz való programozott csatlakozást](media-services-use-aad-auth-to-access-ams-api.md) is.</span><span class="sxs-lookup"><span data-stu-id="b2159-126">Also, learn how to [connect  programmatically to AMS API](media-services-use-aad-auth-to-access-ams-api.md).</span></span>
* <span data-ttu-id="b2159-127">Elindított állapotú standard vagy prémium szintű streamvégpont.</span><span class="sxs-lookup"><span data-stu-id="b2159-127">A standard or premium streaming endpoint in started state.</span></span>  <span data-ttu-id="b2159-128">További információ: [Streamvégpontok felügyelete](media-services-portal-manage-streaming-endpoints.md)</span><span class="sxs-lookup"><span data-stu-id="b2159-128">For more information, see [Managing streaming endpoints](media-services-portal-manage-streaming-endpoints.md)</span></span>

## <a name="sdks-and-tools"></a><span data-ttu-id="b2159-129">SDK-k és eszközök</span><span class="sxs-lookup"><span data-stu-id="b2159-129">SDKs and tools</span></span>

<span data-ttu-id="b2159-130">A Media Services-megoldások létrehozásához a következőket használhatja:</span><span class="sxs-lookup"><span data-stu-id="b2159-130">To build Media Services solutions, you can use:</span></span>

* [<span data-ttu-id="b2159-131">Media Services REST API</span><span class="sxs-lookup"><span data-stu-id="b2159-131">Media Services REST API</span></span>](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference)
* <span data-ttu-id="b2159-132">Az elérhető ügyféloldali SDK-k valamelyike:</span><span class="sxs-lookup"><span data-stu-id="b2159-132">One of the available client SDKs:</span></span>
    * <span data-ttu-id="b2159-133">[.NET-keretrendszerhez készült Azure Media Services SDK](https://github.com/Azure/azure-sdk-for-media-services),</span><span class="sxs-lookup"><span data-stu-id="b2159-133">[Azure Media Services SDK for .NET](https://github.com/Azure/azure-sdk-for-media-services),</span></span>
    * <span data-ttu-id="b2159-134">[Javához készült Azure SDK](https://github.com/Azure/azure-sdk-for-java),</span><span class="sxs-lookup"><span data-stu-id="b2159-134">[Azure SDK for Java](https://github.com/Azure/azure-sdk-for-java),</span></span>
    * <span data-ttu-id="b2159-135">[Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php),</span><span class="sxs-lookup"><span data-stu-id="b2159-135">[Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php),</span></span>
    * <span data-ttu-id="b2159-136">[Node.js-hez készült Azure Media Services](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (Ez a Node.js SDK nem Microsoft által készített verziója.</span><span class="sxs-lookup"><span data-stu-id="b2159-136">[Azure Media Services for Node.js](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (This is a non-Microsoft version of a Node.js SDK.</span></span> <span data-ttu-id="b2159-137">Ennek a karbantartását egy közösség végzi, és jelenleg nem fedi le 100%-osan az AMS API-k tartalmát.)</span><span class="sxs-lookup"><span data-stu-id="b2159-137">It is maintained by a community and currently does not have a 100% coverage of the AMS APIs).</span></span>
* <span data-ttu-id="b2159-138">Meglévő eszközök:</span><span class="sxs-lookup"><span data-stu-id="b2159-138">Existing tools:</span></span>
    * [<span data-ttu-id="b2159-139">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b2159-139">Azure portal</span></span>](https://portal.azure.com/)
    * <span data-ttu-id="b2159-140">[Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Az Azure Media Services Explorer (AMSE) egy Winforms/C#-alkalmazás Windows rendszerre)</span><span class="sxs-lookup"><span data-stu-id="b2159-140">[Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Explorer (AMSE) is a Winforms/C# application for Windows)</span></span>

## <a name="concepts-and-overview"></a><span data-ttu-id="b2159-141">Alapfogalmak és áttekintés</span><span class="sxs-lookup"><span data-stu-id="b2159-141">Concepts and overview</span></span>
<span data-ttu-id="b2159-142">Az Azure Media Services alapfogalmaiért lásd: [Fogalmak](media-services-concepts.md)</span><span class="sxs-lookup"><span data-stu-id="b2159-142">For Azure Media Services concepts, see [Concepts](media-services-concepts.md).</span></span>

<span data-ttu-id="b2159-143">Az Azure Media Services összes fő összetevőjét bemutató útmutató-sorozat: [Az Azure Media Services részletes oktatóprogramjai](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series)</span><span class="sxs-lookup"><span data-stu-id="b2159-143">For a how-to series that introduces you to all the main components of Azure Media Services, see [Azure Media Services Step-by-Step tutorials](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series).</span></span> <span data-ttu-id="b2159-144">Ez a sorozat átfogó áttekintést nyújt a fogalmakról, és az AMSE eszköz használatával mutatja be az AMS-feladatokat.</span><span class="sxs-lookup"><span data-stu-id="b2159-144">This series has a great overview of concepts and it uses the AMSE tool to demonstrate AMS tasks.</span></span> <span data-ttu-id="b2159-145">Az AMSE eszköz egy Windows-eszköz.</span><span class="sxs-lookup"><span data-stu-id="b2159-145">AMSE tool is a Windows tool.</span></span> <span data-ttu-id="b2159-146">Az eszköz támogatja a legtöbb olyan műveletet, amelyek a [.NET-keretrendszerhez készült AMS SDK](https://github.com/Azure/azure-sdk-for-media-services), a [Javához készült Azure SDK](https://github.com/Azure/azure-sdk-for-java) vagy az [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php) használatával programozás útján megvalósíthatók.</span><span class="sxs-lookup"><span data-stu-id="b2159-146">This tool supports most of the tasks you can achieve programmatically with [AMS SDK for .NET](https://github.com/Azure/azure-sdk-for-media-services), [Azure SDK for Java](https://github.com/Azure/azure-sdk-for-java), or  [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php).</span></span>

## <a name="supported-scenarios-and-availability-of-media-services-across-data-centers"></a><span data-ttu-id="b2159-147">Támogatott helyzetek és a Media Services rendelkezésre állása az egyes adatközpontokban</span><span class="sxs-lookup"><span data-stu-id="b2159-147">Supported scenarios and availability of Media Services across data centers</span></span>

<span data-ttu-id="b2159-148">Részletes információkért lásd az [AMS-forgatókönyvek és a funkciók és szolgáltatások az egyes adatközpontokban történő rendelkezésre állását](scenarios-and-availability.md) ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="b2159-148">For detailed information, see [AMS scenarios and availability of features and services across data centers](scenarios-and-availability.md).</span></span>

## <a name="service-level-agreement-sla"></a><span data-ttu-id="b2159-149">Szolgáltatói szerződés (SLA)</span><span class="sxs-lookup"><span data-stu-id="b2159-149">Service Level Agreement (SLA)</span></span>

* <span data-ttu-id="b2159-150">A Media Services kódolási funkciójához szükséges REST API-tranzakciókhoz 99,9%-os rendelkezésre állást garantálunk.</span><span class="sxs-lookup"><span data-stu-id="b2159-150">For Media Services Encoding, we guarantee 99.9% availability of REST API transactions.</span></span>
* <span data-ttu-id="b2159-151">A streamelési funkciók szolgáltatási kéréseinek sikeres kiszolgálására 99,9%-os rendelkezésre állást garantálunk a létező médiatartalmak esetében egy standard vagy prémium szintű streamvégpont megvásárlásakor.</span><span class="sxs-lookup"><span data-stu-id="b2159-151">For Streaming, we will successfully service requests with a 99.9% availability guarantee for existing media content when a standard or premium streaming endpoint is purchased.</span></span>
* <span data-ttu-id="b2159-152">Az élő csatornák esetében garantáljuk, hogy a futó csatornák az idő legalább 99,9%-ában elérhetők lesznek kívülről.</span><span class="sxs-lookup"><span data-stu-id="b2159-152">For Live Channels, we guarantee that running Channels will have external connectivity at least 99.9% of the time.</span></span>
* <span data-ttu-id="b2159-153">A Content Protection esetében garantáljuk, hogy a kulcskéréseket az idő 99,9%-ában sikeresen teljesítjük.</span><span class="sxs-lookup"><span data-stu-id="b2159-153">For Content Protection, we guarantee that we will successfully fulfill key requests at least 99.9% of the time.</span></span>
* <span data-ttu-id="b2159-154">Az indexelő esetében sikeresen, 99,9%-os rendelkezésre állási garanciával kiszolgáljuk az egy kódolási egységgel feldolgozott indexelési kéréseket.</span><span class="sxs-lookup"><span data-stu-id="b2159-154">For Indexer, we will successfully service Indexer Task requests processed with an Encoding Reserved Unit 99.9% of the time.</span></span>

<span data-ttu-id="b2159-155">További információ: [Microsoft Azure SLA](https://azure.microsoft.com/support/legal/sla/)</span><span class="sxs-lookup"><span data-stu-id="b2159-155">For more information, see [Microsoft Azure SLA](https://azure.microsoft.com/support/legal/sla/).</span></span>

<span data-ttu-id="b2159-156">Az adatközpontokban lévő rendelkezésre állásról információért lásd a [Rendelkezésre állás](scenarios-and-availability.md#availability) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="b2159-156">For information about availability in datacenters, see the [Avaiability](scenarios-and-availability.md#availability) section.</span></span>

## <a name="support"></a><span data-ttu-id="b2159-157">Támogatás</span><span class="sxs-lookup"><span data-stu-id="b2159-157">Support</span></span>

<span data-ttu-id="b2159-158">[Az Azure-támogatási szolgáltatások](https://azure.microsoft.com/support/options/) támogatási lehetőségeket biztosítanak az Azure szolgáltatásaival, köztük a Media Services szolgáltatással kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="b2159-158">[Azure Support](https://azure.microsoft.com/support/options/) provides support options for Azure, including Media Services.</span></span>

## <a name="provide-feedback"></a><span data-ttu-id="b2159-159">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="b2159-159">Provide feedback</span></span>

[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
