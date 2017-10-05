---
title: "Az Azure Cosmos DB rendelkezés átviteli |} Microsoft Docs"
description: "Tudnivalók az Azure Cosmos DB containsers, gyűjtemények, diagramokat és táblák kiosztott átviteli sebesség."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: f98def7f-f012-4592-be03-f6fa185e1b1e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2017
ms.author: mimig
ms.openlocfilehash: d541bb19ba7e5ecb44c9fe91b1e232d4d9c2170e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="set-throughput-for-azure-cosmos-db-containers"></a><span data-ttu-id="d1ec3-103">Az Azure Cosmos DB tárolókat átviteli beállítása</span><span class="sxs-lookup"><span data-stu-id="d1ec3-103">Set throughput for Azure Cosmos DB containers</span></span>

<span data-ttu-id="d1ec3-104">Átviteli sebesség az az Azure Cosmos DB tárolókat az Azure portálon vagy az ügyfél SDK-k segítségével állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="d1ec3-104">You can set throughput for your Azure Cosmos DB containers in the Azure portal or by using the client SDKs.</span></span> 

<span data-ttu-id="d1ec3-105">A következő táblázat felsorolja a rendelkezésre álló tárolók az átviteli sebesség:</span><span class="sxs-lookup"><span data-stu-id="d1ec3-105">The following table lists the throughput available for containers:</span></span>

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><span data-ttu-id="d1ec3-106"><strong>Az egypartíciós tároló</strong></span><span class="sxs-lookup"><span data-stu-id="d1ec3-106"><strong>Single Partition Container</strong></span></span></p></td>
            <td valign="top"><p><span data-ttu-id="d1ec3-107"><strong>Particionált tároló</strong></span><span class="sxs-lookup"><span data-stu-id="d1ec3-107"><strong>Partitioned Container</strong></span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="d1ec3-108">Minimális átviteli sebesség</span><span class="sxs-lookup"><span data-stu-id="d1ec3-108">Minimum Throughput</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="d1ec3-109">400 kérelem egység / másodperc</span><span class="sxs-lookup"><span data-stu-id="d1ec3-109">400 request units per second</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="d1ec3-110">2500 kérést egység / másodperc</span><span class="sxs-lookup"><span data-stu-id="d1ec3-110">2,500 request units per second</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="d1ec3-111">Maximális átviteli sebesség</span><span class="sxs-lookup"><span data-stu-id="d1ec3-111">Maximum Throughput</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="d1ec3-112">10 000 kérelemegység / másodperc</span><span class="sxs-lookup"><span data-stu-id="d1ec3-112">10,000 request units per second</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="d1ec3-113">Korlátlan</span><span class="sxs-lookup"><span data-stu-id="d1ec3-113">Unlimited</span></span></p></td>
        </tr>
    </tbody>
</table>

## <a name="to-set-the-throughput-by-using-the-azure-portal"></a><span data-ttu-id="d1ec3-114">Az átviteli sebesség beállítása az Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="d1ec3-114">To set the throughput by using the Azure portal</span></span>

1. <span data-ttu-id="d1ec3-115">Egy új ablakban nyissa meg a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d1ec3-115">In a new window, open the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="d1ec3-116">A bal oldali sávon kattintson **Azure Cosmos DB**, vagy kattintson a **több szolgáltatások** alsó, majd görgessen a **adatbázisok**, és kattintson a **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="d1ec3-116">On the left bar, click **Azure Cosmos DB**, or click **More Services** at the bottom, then scroll to **Databases**, and then click **Azure Cosmos DB**.</span></span>
3. <span data-ttu-id="d1ec3-117">Válassza ki a Cosmos DB fiókját.</span><span class="sxs-lookup"><span data-stu-id="d1ec3-117">Select your Cosmos DB account.</span></span>
4. <span data-ttu-id="d1ec3-118">Kattintson az új ablakban **adatok kezelővel (előzetes verzió)** a navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="d1ec3-118">In the new window, click **Data Explorer (Preview)** in the navigation menu.</span></span>
5. <span data-ttu-id="d1ec3-119">Az új ablakban bontsa ki az adatbázis és a tároló majd **skála & beállítások**.</span><span class="sxs-lookup"><span data-stu-id="d1ec3-119">In the new window, expand your database and container and then click **Scale & Settings**.</span></span>
6. <span data-ttu-id="d1ec3-120">Az új ablakban írja be az új átviteli értéket a **átviteli** gombra, majd **mentése**.</span><span class="sxs-lookup"><span data-stu-id="d1ec3-120">In the new window, type the new throughput value in the **Throughput** box, and then click **Save**.</span></span>

<a id="set-throughput-sdk"></a>

## <a name="to-set-the-throughput-by-using-the-documentdb-api-for-net"></a><span data-ttu-id="d1ec3-121">Az átviteli sebesség beállítása a .NET-hez a DocumentDB API használatával</span><span class="sxs-lookup"><span data-stu-id="d1ec3-121">To set the throughput by using the DocumentDB API for .NET</span></span>

```C#
//Fetch the resource to be updated
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set the throughput to the new value, for example 12,000 request units per second
offer = new OfferV2(offer, 12000);

//Now persist these changes to the database by replacing the original resource
await client.ReplaceOfferAsync(offer);
```

## <a name="throughput-faq"></a><span data-ttu-id="d1ec3-122">Átviteli – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="d1ec3-122">Throughput FAQ</span></span>

<span data-ttu-id="d1ec3-123">**I állíthatja az átviteli sebesség kisebb, mint 400 RU/mp?**</span><span class="sxs-lookup"><span data-stu-id="d1ec3-123">**Can I set my throughput to less than 400 RU/s?**</span></span>

<span data-ttu-id="d1ec3-124">400 RU/mp a minimális átviteli sebesség érhető el a Cosmos DB az egypartíciós gyűjtemények (2500 RU/mp a particionált gyűjtemények minimális).</span><span class="sxs-lookup"><span data-stu-id="d1ec3-124">400 RU/s is the minimum throughput available on Cosmos DB single partition collections (2500 RU/s is the minimum for partitioned collections).</span></span> <span data-ttu-id="d1ec3-125">Kérelem egységek 100 RU/mp intervallumok belül vannak beállítva, de az átviteli sebesség nem állítható be 100 RU/mp vagy bármely érték kisebb, mint a 400 RU/mp.</span><span class="sxs-lookup"><span data-stu-id="d1ec3-125">Request units are set in 100 RU/s intervals, but throughput cannot be set to 100 RU/s or any value smaller than 400 RU/s.</span></span> <span data-ttu-id="d1ec3-126">Ha fejlesztéséhez és teszteléséhez Cosmos DB költséghatékony módszert keres, akkor használhatja az ingyenes [Azure Cosmos DB emulátor](local-emulator.md), amely központilag telepíthető helyileg ingyenesen.</span><span class="sxs-lookup"><span data-stu-id="d1ec3-126">If you're looking for a cost effective method to develop and test Cosmos DB, you can use the free [Azure Cosmos DB Emulator](local-emulator.md), which you can deploy locally at no cost.</span></span> 

<span data-ttu-id="d1ec3-127">**Hogyan állíthatom be a MongoDB API-jával througput?**</span><span class="sxs-lookup"><span data-stu-id="d1ec3-127">**How do I set througput using the MongoDB API?**</span></span>

<span data-ttu-id="d1ec3-128">Nincs átviteli sebesség beállításához MongoDB API kiterjesztés nélkül.</span><span class="sxs-lookup"><span data-stu-id="d1ec3-128">There's no MongoDB API extension to set throughput.</span></span> <span data-ttu-id="d1ec3-129">A javaslat, hogy a DocumentDB API-t használó, ahogy az [az átviteli sebesség beállítása a .NET-hez a DocumentDB API használatával](#set-throughput-sdk).</span><span class="sxs-lookup"><span data-stu-id="d1ec3-129">The recommendation is to use the DocumentDB API, as shown in [To set the throughput by using the DocumentDB API for .NET](#set-throughput-sdk).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d1ec3-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d1ec3-130">Next steps</span></span>

<span data-ttu-id="d1ec3-131">Üzembe helyezési és folyamatos bolygónk-méretezéssel Cosmos DB, kapcsolatos további információkért lásd: [particionálás és méretezést Cosmos DB,](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="d1ec3-131">To learn more about provisioning and going planet-scale with Cosmos DB, see [Partitioning and scaling with Cosmos DB](partition-data.md).</span></span>
