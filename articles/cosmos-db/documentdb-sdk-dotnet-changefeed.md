---
title: "Az Azure DocumentDB .NET SDK adatcsatorna processzor & erőforrások módosítása |} Microsoft Docs"
description: "Tudnivalók a módosítás hírcsatorna processzor API és SDK kiadási dátum, használatból való kivonást dátumok és a DocumentDB .NET módosítás adatcsatorna-processzor SDK verziói között végrehajtott módosításokat."
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
ms.openlocfilehash: 40c796bc5af1220c46950a6fac062ffdd243e59f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="documentdb-net-change-feed-processor-sdk-download-and-release-notes"></a><span data-ttu-id="4bf4f-103">A DocumentDB .NET módosítás adatcsatorna processzor SDK: Töltse le és a kibocsátási megjegyzések</span><span class="sxs-lookup"><span data-stu-id="4bf4f-103">DocumentDB .NET Change Feed Processor SDK: Download and release notes</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4bf4f-104">.NET</span><span class="sxs-lookup"><span data-stu-id="4bf4f-104">.NET</span></span>](documentdb-sdk-dotnet.md)
> * [<span data-ttu-id="4bf4f-105">.NET-módosítás adatcsatorna</span><span class="sxs-lookup"><span data-stu-id="4bf4f-105">.NET Change Feed</span></span>](documentdb-sdk-dotnet-changefeed.md)
> * [<span data-ttu-id="4bf4f-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="4bf4f-106">.NET Core</span></span>](documentdb-sdk-dotnet-core.md)
> * [<span data-ttu-id="4bf4f-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="4bf4f-107">Node.js</span></span>](documentdb-sdk-node.md)
> * [<span data-ttu-id="4bf4f-108">Java</span><span class="sxs-lookup"><span data-stu-id="4bf4f-108">Java</span></span>](documentdb-sdk-java.md)
> * [<span data-ttu-id="4bf4f-109">Python</span><span class="sxs-lookup"><span data-stu-id="4bf4f-109">Python</span></span>](documentdb-sdk-python.md)
> * [<span data-ttu-id="4bf4f-110">REST</span><span class="sxs-lookup"><span data-stu-id="4bf4f-110">REST</span></span>](https://docs.microsoft.com/rest/api/documentdb/)
> * [<span data-ttu-id="4bf4f-111">REST erőforrás-szolgáltató</span><span class="sxs-lookup"><span data-stu-id="4bf4f-111">REST Resource Provider</span></span>](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [<span data-ttu-id="4bf4f-112">SQL</span><span class="sxs-lookup"><span data-stu-id="4bf4f-112">SQL</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td><span data-ttu-id="4bf4f-113">**SDK letöltése**</span><span class="sxs-lookup"><span data-stu-id="4bf4f-113">**SDK download**</span></span></td><td>[<span data-ttu-id="4bf4f-114">NuGet</span><span class="sxs-lookup"><span data-stu-id="4bf4f-114">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)</td></tr>

<tr><td><span data-ttu-id="4bf4f-115">**API-JÁNAK dokumentációja**</span><span class="sxs-lookup"><span data-stu-id="4bf4f-115">**API documentation**</span></span></td><td>[<span data-ttu-id="4bf4f-116">Adatcsatorna processzor könyvtár API-referenciadokumentáció módosítása</span><span class="sxs-lookup"><span data-stu-id="4bf4f-116">Change Feed Processor library API reference documentation</span></span>](/dotnet/api/microsoft.azure.documents.changefeedprocessor?view=azure-dotnet)</td></tr>

<tr><td><span data-ttu-id="4bf4f-117">**Első lépések**</span><span class="sxs-lookup"><span data-stu-id="4bf4f-117">**Get started**</span></span></td><td>[<span data-ttu-id="4bf4f-118">A DocumentDB módosítás hírcsatorna processzor .NET SDK használatába</span><span class="sxs-lookup"><span data-stu-id="4bf4f-118">Get started with the DocumentDB Change Feed Processor .NET SDK</span></span>](change-feed.md)</td></tr>

<tr><td><span data-ttu-id="4bf4f-119">**Aktuális támogatott keretrendszer**</span><span class="sxs-lookup"><span data-stu-id="4bf4f-119">**Current supported framework**</span></span></td><td>[<span data-ttu-id="4bf4f-120">Microsoft .NET-keretrendszer 4.5</span><span class="sxs-lookup"><span data-stu-id="4bf4f-120">Microsoft .NET Framework 4.5</span></span>](https://www.microsoft.com/download/details.aspx?id=30653)</td></tr>
</table></br>

## <a name="release-notes"></a><span data-ttu-id="4bf4f-121">Kibocsátási megjegyzések</span><span class="sxs-lookup"><span data-stu-id="4bf4f-121">Release notes</span></span>

### <a name="a-name110110"></a><span data-ttu-id="4bf4f-122"><a name="1.1.0"/>1.1.0</span><span class="sxs-lookup"><span data-stu-id="4bf4f-122"><a name="1.1.0"/>1.1.0</span></span>
* <span data-ttu-id="4bf4f-123">A módosítás hírcsatorna feldolgozandó fennmaradó munka becsléséhez metódus hozzá.</span><span class="sxs-lookup"><span data-stu-id="4bf4f-123">Added a method to obtain an estimate of remaining work to be processed in the Change Feed.</span></span>
* <span data-ttu-id="4bf4f-124">Kompatibilis [DocumentDB .NET SDK](documentdb-sdk-dotnet.md) 1.13.2 verzió vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="4bf4f-124">Compatible with [DocumentDB .NET SDK](documentdb-sdk-dotnet.md) versions 1.13.2 and above.</span></span>

### <a name="a-name100100"></a><span data-ttu-id="4bf4f-125"><a name="1.0.0"/>1.0.0</span><span class="sxs-lookup"><span data-stu-id="4bf4f-125"><a name="1.0.0"/>1.0.0</span></span>
* <span data-ttu-id="4bf4f-126">GA SDK</span><span class="sxs-lookup"><span data-stu-id="4bf4f-126">GA SDK</span></span>
* <span data-ttu-id="4bf4f-127">Kompatibilis [DocumentDB .NET SDK](documentdb-sdk-dotnet.md) verziók 1.14.1 vagy régebbi verzió.</span><span class="sxs-lookup"><span data-stu-id="4bf4f-127">Compatible with [DocumentDB .NET SDK](documentdb-sdk-dotnet.md) versions 1.14.1 and below.</span></span>

## <a name="release--retirement-dates"></a><span data-ttu-id="4bf4f-128">Kiadás & használatból való kivonást dátumok</span><span class="sxs-lookup"><span data-stu-id="4bf4f-128">Release & Retirement dates</span></span>
<span data-ttu-id="4bf4f-129">Microsoft legalább értesítést küldenek **12 hónapon keresztül** SDK eltávolítása érdekében vagy újabb támogatott verzióra való áttérés előtt.</span><span class="sxs-lookup"><span data-stu-id="4bf4f-129">Microsoft will provide notification at least **12 months** in advance of retiring an SDK in order to smooth the transition to a newer/supported version.</span></span>

<span data-ttu-id="4bf4f-130">Új szolgáltatásait és funkcióit és optimalizálás csak hozzá az aktuális SDK, így javasoljuk, hogy mindig a legújabb SDK verzióra frissít legkorábban lehető.</span><span class="sxs-lookup"><span data-stu-id="4bf4f-130">New features and functionality and optimizations are only added to the current SDK, as such it is recommended that you always upgrade to the latest SDK version as early as possible.</span></span> 

<span data-ttu-id="4bf4f-131">A Cosmos DB kivont SDK használatával fog kell elutasította a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="4bf4f-131">Any request to Cosmos DB using a retired SDK will be rejected by the service.</span></span>

<br/>

| <span data-ttu-id="4bf4f-132">Verzió</span><span class="sxs-lookup"><span data-stu-id="4bf4f-132">Version</span></span> | <span data-ttu-id="4bf4f-133">Kiadás dátuma</span><span class="sxs-lookup"><span data-stu-id="4bf4f-133">Release Date</span></span> | <span data-ttu-id="4bf4f-134">Kivezetési dátum</span><span class="sxs-lookup"><span data-stu-id="4bf4f-134">Retirement Date</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="4bf4f-135">1.1.0</span><span class="sxs-lookup"><span data-stu-id="4bf4f-135">1.1.0</span></span>](#1.1.0) |<span data-ttu-id="4bf4f-136">2017. augusztus 13.</span><span class="sxs-lookup"><span data-stu-id="4bf4f-136">August 13, 2017</span></span> |--- |
| [<span data-ttu-id="4bf4f-137">1.0.0</span><span class="sxs-lookup"><span data-stu-id="4bf4f-137">1.0.0</span></span>](#1.0.0) |<span data-ttu-id="4bf4f-138">2017. július 07.</span><span class="sxs-lookup"><span data-stu-id="4bf4f-138">July 07, 2017</span></span> |--- |


## <a name="faq"></a><span data-ttu-id="4bf4f-139">GYIK</span><span class="sxs-lookup"><span data-stu-id="4bf4f-139">FAQ</span></span>
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a><span data-ttu-id="4bf4f-140">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="4bf4f-140">See also</span></span>
<span data-ttu-id="4bf4f-141">A Cosmos DB kapcsolatos további információkért lásd: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) szolgáltatás lapján.</span><span class="sxs-lookup"><span data-stu-id="4bf4f-141">To learn more about Cosmos DB, see [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service page.</span></span> 

