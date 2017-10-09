---
title: "aaaAzure DocumentDB .NET módosítás hírcsatorna processzor SDK & erőforrások |} Microsoft Docs"
description: "Tudnivalók az hello módosítás hírcsatorna processzor API és az SDK, beleértve a kiadási dátum, a használatból való kivonást dátum és a DocumentDB .NET módosítás hírcsatorna processzor SDK hello verziói között végrehajtott módosítások."
services: cosmos-db
documentationcenter: .net
author: ealsur
manager: kirillg
editor: mimig1
ms.assetid: f2dd9438-8879-4f74-bb6c-e1efc2cd0157
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/14/2017
ms.author: maquaran
ms.openlocfilehash: 7c001cc77f41c01445fb53328e9d99fd3d312c58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="documentdb-net-change-feed-processor-sdk-download-and-release-notes"></a><span data-ttu-id="0547f-103">A DocumentDB .NET módosítás adatcsatorna processzor SDK: Töltse le és a kibocsátási megjegyzések</span><span class="sxs-lookup"><span data-stu-id="0547f-103">DocumentDB .NET Change Feed Processor SDK: Download and release notes</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0547f-104">.NET</span><span class="sxs-lookup"><span data-stu-id="0547f-104">.NET</span></span>](documentdb-sdk-dotnet.md)
> * [<span data-ttu-id="0547f-105">.NET-módosítás adatcsatorna</span><span class="sxs-lookup"><span data-stu-id="0547f-105">.NET Change Feed</span></span>](documentdb-sdk-dotnet-changefeed.md)
> * [<span data-ttu-id="0547f-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="0547f-106">.NET Core</span></span>](documentdb-sdk-dotnet-core.md)
> * [<span data-ttu-id="0547f-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="0547f-107">Node.js</span></span>](documentdb-sdk-node.md)
> * [<span data-ttu-id="0547f-108">Java</span><span class="sxs-lookup"><span data-stu-id="0547f-108">Java</span></span>](documentdb-sdk-java.md)
> * [<span data-ttu-id="0547f-109">Python</span><span class="sxs-lookup"><span data-stu-id="0547f-109">Python</span></span>](documentdb-sdk-python.md)
> * [<span data-ttu-id="0547f-110">REST</span><span class="sxs-lookup"><span data-stu-id="0547f-110">REST</span></span>](https://docs.microsoft.com/rest/api/documentdb/)
> * [<span data-ttu-id="0547f-111">REST erőforrás-szolgáltató</span><span class="sxs-lookup"><span data-stu-id="0547f-111">REST Resource Provider</span></span>](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [<span data-ttu-id="0547f-112">SQL</span><span class="sxs-lookup"><span data-stu-id="0547f-112">SQL</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td><span data-ttu-id="0547f-113">**SDK letöltése**</span><span class="sxs-lookup"><span data-stu-id="0547f-113">**SDK download**</span></span></td><td>[<span data-ttu-id="0547f-114">NuGet</span><span class="sxs-lookup"><span data-stu-id="0547f-114">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)</td></tr>

<tr><td><span data-ttu-id="0547f-115">**API-JÁNAK dokumentációja**</span><span class="sxs-lookup"><span data-stu-id="0547f-115">**API documentation**</span></span></td><td>[<span data-ttu-id="0547f-116">Adatcsatorna processzor könyvtár API-referenciadokumentáció módosítása</span><span class="sxs-lookup"><span data-stu-id="0547f-116">Change Feed Processor library API reference documentation</span></span>](/dotnet/api/microsoft.azure.documents.changefeedprocessor?view=azure-dotnet)</td></tr>

<tr><td><span data-ttu-id="0547f-117">**Első lépések**</span><span class="sxs-lookup"><span data-stu-id="0547f-117">**Get started**</span></span></td><td>[<span data-ttu-id="0547f-118">Ismerkedés a DocumentDB módosítás hírcsatorna processzor .NET SDK hello</span><span class="sxs-lookup"><span data-stu-id="0547f-118">Get started with hello DocumentDB Change Feed Processor .NET SDK</span></span>](change-feed.md)</td></tr>

<tr><td><span data-ttu-id="0547f-119">**Aktuális támogatott keretrendszer**</span><span class="sxs-lookup"><span data-stu-id="0547f-119">**Current supported framework**</span></span></td><td>[<span data-ttu-id="0547f-120">Microsoft .NET-keretrendszer 4.5</span><span class="sxs-lookup"><span data-stu-id="0547f-120">Microsoft .NET Framework 4.5</span></span>](https://www.microsoft.com/download/details.aspx?id=30653)</td></tr>
</table></br>

## <a name="release-notes"></a><span data-ttu-id="0547f-121">Kibocsátási megjegyzések</span><span class="sxs-lookup"><span data-stu-id="0547f-121">Release notes</span></span>

### <a name="a-name110110"></a><span data-ttu-id="0547f-122"><a name="1.1.0"/>1.1.0</span><span class="sxs-lookup"><span data-stu-id="0547f-122"><a name="1.1.0"/>1.1.0</span></span>
* <span data-ttu-id="0547f-123">A metódus tooobtain módosítás hírcsatorna hello feldolgozása fennmaradó munkahelyi toobe becsült hozzá.</span><span class="sxs-lookup"><span data-stu-id="0547f-123">Added a method tooobtain an estimate of remaining work toobe processed in hello Change Feed.</span></span>
* <span data-ttu-id="0547f-124">Kompatibilis [DocumentDB .NET SDK](documentdb-sdk-dotnet.md) 1.13.2 verzió vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="0547f-124">Compatible with [DocumentDB .NET SDK](documentdb-sdk-dotnet.md) versions 1.13.2 and above.</span></span>

### <a name="a-name100100"></a><span data-ttu-id="0547f-125"><a name="1.0.0"/>1.0.0</span><span class="sxs-lookup"><span data-stu-id="0547f-125"><a name="1.0.0"/>1.0.0</span></span>
* <span data-ttu-id="0547f-126">GA SDK</span><span class="sxs-lookup"><span data-stu-id="0547f-126">GA SDK</span></span>
* <span data-ttu-id="0547f-127">Kompatibilis [DocumentDB .NET SDK](documentdb-sdk-dotnet.md) verziók 1.14.1 vagy régebbi verzió.</span><span class="sxs-lookup"><span data-stu-id="0547f-127">Compatible with [DocumentDB .NET SDK](documentdb-sdk-dotnet.md) versions 1.14.1 and below.</span></span>

## <a name="release--retirement-dates"></a><span data-ttu-id="0547f-128">Kiadás & használatból való kivonást dátumok</span><span class="sxs-lookup"><span data-stu-id="0547f-128">Release & Retirement dates</span></span>
<span data-ttu-id="0547f-129">Microsoft legalább értesítést küldenek **12 hónapon keresztül** előre kivonása az SDK-t rendelés toosmooth hello átmenet tooa vagy újabb támogatott verzióra.</span><span class="sxs-lookup"><span data-stu-id="0547f-129">Microsoft will provide notification at least **12 months** in advance of retiring an SDK in order toosmooth hello transition tooa newer/supported version.</span></span>

<span data-ttu-id="0547f-130">Új szolgáltatásait és funkcióit és optimalizálás csak hozzáadott toohello aktuális SDK-t, így javasolt, hogy Ön mindig frissítési toohello SDK legújabb lehető leghamarabb.</span><span class="sxs-lookup"><span data-stu-id="0547f-130">New features and functionality and optimizations are only added toohello current SDK, as such it is recommended that you always upgrade toohello latest SDK version as early as possible.</span></span> 

<span data-ttu-id="0547f-131">A kérelem tooCosmos DB kivont SDK használatával a program elutasítja hello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="0547f-131">Any request tooCosmos DB using a retired SDK will be rejected by hello service.</span></span>

<br/>

| <span data-ttu-id="0547f-132">Verzió</span><span class="sxs-lookup"><span data-stu-id="0547f-132">Version</span></span> | <span data-ttu-id="0547f-133">Kiadás dátuma</span><span class="sxs-lookup"><span data-stu-id="0547f-133">Release Date</span></span> | <span data-ttu-id="0547f-134">Kivezetési dátum</span><span class="sxs-lookup"><span data-stu-id="0547f-134">Retirement Date</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="0547f-135">1.1.0</span><span class="sxs-lookup"><span data-stu-id="0547f-135">1.1.0</span></span>](#1.1.0) |<span data-ttu-id="0547f-136">2017. augusztus 13.</span><span class="sxs-lookup"><span data-stu-id="0547f-136">August 13, 2017</span></span> |--- |
| [<span data-ttu-id="0547f-137">1.0.0</span><span class="sxs-lookup"><span data-stu-id="0547f-137">1.0.0</span></span>](#1.0.0) |<span data-ttu-id="0547f-138">2017. július 07.</span><span class="sxs-lookup"><span data-stu-id="0547f-138">July 07, 2017</span></span> |--- |


## <a name="faq"></a><span data-ttu-id="0547f-139">GYIK</span><span class="sxs-lookup"><span data-stu-id="0547f-139">FAQ</span></span>
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a><span data-ttu-id="0547f-140">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="0547f-140">See also</span></span>
<span data-ttu-id="0547f-141">toolearn Cosmos DB kapcsolatos további információkért lásd: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) szolgáltatás lapján.</span><span class="sxs-lookup"><span data-stu-id="0547f-141">toolearn more about Cosmos DB, see [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service page.</span></span> 

