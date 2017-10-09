---
title: "az Azure Cosmos DB aaaProvision átviteli |} Microsoft Docs"
description: "Ismerje meg, hogyan tooset kiosztott átviteli sebesség a Azure Cosmos DB containsers, gyűjtemények, diagramokat és táblákat."
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
ms.openlocfilehash: c143f4aace466b7109168a50e2eb80ddeca6400e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-throughput-for-azure-cosmos-db-containers"></a><span data-ttu-id="edf38-103">Az Azure Cosmos DB tárolókat átviteli beállítása</span><span class="sxs-lookup"><span data-stu-id="edf38-103">Set throughput for Azure Cosmos DB containers</span></span>

<span data-ttu-id="edf38-104">Átviteli sebesség beállítása az Azure Cosmos DB tárolók hello Azure-portálon, vagy a hello ügyfél SDK-k.</span><span class="sxs-lookup"><span data-stu-id="edf38-104">You can set throughput for your Azure Cosmos DB containers in hello Azure portal or by using hello client SDKs.</span></span> 

<span data-ttu-id="edf38-105">hello a következő táblázat felsorolja a hello átviteli tárolók érhető el:</span><span class="sxs-lookup"><span data-stu-id="edf38-105">hello following table lists hello throughput available for containers:</span></span>

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><span data-ttu-id="edf38-106"><strong>Az egypartíciós tároló</strong></span><span class="sxs-lookup"><span data-stu-id="edf38-106"><strong>Single Partition Container</strong></span></span></p></td>
            <td valign="top"><p><span data-ttu-id="edf38-107"><strong>Particionált tároló</strong></span><span class="sxs-lookup"><span data-stu-id="edf38-107"><strong>Partitioned Container</strong></span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="edf38-108">Minimális átviteli sebesség</span><span class="sxs-lookup"><span data-stu-id="edf38-108">Minimum Throughput</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="edf38-109">400 kérelem egység / másodperc</span><span class="sxs-lookup"><span data-stu-id="edf38-109">400 request units per second</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="edf38-110">2500 kérést egység / másodperc</span><span class="sxs-lookup"><span data-stu-id="edf38-110">2,500 request units per second</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="edf38-111">Maximális átviteli sebesség</span><span class="sxs-lookup"><span data-stu-id="edf38-111">Maximum Throughput</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="edf38-112">10 000 kérelemegység / másodperc</span><span class="sxs-lookup"><span data-stu-id="edf38-112">10,000 request units per second</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="edf38-113">Korlátlan</span><span class="sxs-lookup"><span data-stu-id="edf38-113">Unlimited</span></span></p></td>
        </tr>
    </tbody>
</table>

## <a name="tooset-hello-throughput-by-using-hello-azure-portal"></a><span data-ttu-id="edf38-114">tooset hello átviteli hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="edf38-114">tooset hello throughput by using hello Azure portal</span></span>

1. <span data-ttu-id="edf38-115">Egy új ablakban nyissa meg a hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="edf38-115">In a new window, open hello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="edf38-116">Hello bal oldali sávon kattintson **Azure Cosmos DB**, vagy kattintson a **több szolgáltatások** hello lap alján, majd görgessen túl**adatbázisok**, és kattintson a **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="edf38-116">On hello left bar, click **Azure Cosmos DB**, or click **More Services** at hello bottom, then scroll too**Databases**, and then click **Azure Cosmos DB**.</span></span>
3. <span data-ttu-id="edf38-117">Válassza ki a Cosmos DB fiókját.</span><span class="sxs-lookup"><span data-stu-id="edf38-117">Select your Cosmos DB account.</span></span>
4. <span data-ttu-id="edf38-118">Hello új ablakban kattintson **adatok kezelővel (előzetes verzió)** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="edf38-118">In hello new window, click **Data Explorer (Preview)** in hello navigation menu.</span></span>
5. <span data-ttu-id="edf38-119">Hello új ablakában bontsa ki az adatbázis és a tároló, és kattintson a **skála & beállítások**.</span><span class="sxs-lookup"><span data-stu-id="edf38-119">In hello new window, expand your database and container and then click **Scale & Settings**.</span></span>
6. <span data-ttu-id="edf38-120">Hello új ablakban írja be az új átviteli értéknek hello hello **átviteli** gombra, majd **mentése**.</span><span class="sxs-lookup"><span data-stu-id="edf38-120">In hello new window, type hello new throughput value in hello **Throughput** box, and then click **Save**.</span></span>

<a id="set-throughput-sdk"></a>

## <a name="tooset-hello-throughput-by-using-hello-documentdb-api-for-net"></a><span data-ttu-id="edf38-121">tooset hello átviteli hello DocumentDB API használatával a .NET-hez</span><span class="sxs-lookup"><span data-stu-id="edf38-121">tooset hello throughput by using hello DocumentDB API for .NET</span></span>

```C#
//Fetch hello resource toobe updated
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set hello throughput toohello new value, for example 12,000 request units per second
offer = new OfferV2(offer, 12000);

//Now persist these changes toohello database by replacing hello original resource
await client.ReplaceOfferAsync(offer);
```

## <a name="throughput-faq"></a><span data-ttu-id="edf38-122">Átviteli – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="edf38-122">Throughput FAQ</span></span>

<span data-ttu-id="edf38-123">**Beállíthatja a átviteli tooless, mint a 400 RU/mp?**</span><span class="sxs-lookup"><span data-stu-id="edf38-123">**Can I set my throughput tooless than 400 RU/s?**</span></span>

<span data-ttu-id="edf38-124">400 RU/mp hello minimális átviteli sebesség érhető el a Cosmos DB az egypartíciós gyűjtemények (2500 RU/mp a particionált gyűjtemények minimális hello).</span><span class="sxs-lookup"><span data-stu-id="edf38-124">400 RU/s is hello minimum throughput available on Cosmos DB single partition collections (2500 RU/s is hello minimum for partitioned collections).</span></span> <span data-ttu-id="edf38-125">Kérelem egységek 100 RU/mp intervallumok belül vannak beállítva, de az átviteli sebesség nem állítható be too100 RU/mp vagy bármely érték kisebb, mint a 400 RU/mp.</span><span class="sxs-lookup"><span data-stu-id="edf38-125">Request units are set in 100 RU/s intervals, but throughput cannot be set too100 RU/s or any value smaller than 400 RU/s.</span></span> <span data-ttu-id="edf38-126">Ha egy költséghatékony metódus toodevelop keres, és Cosmos DB tesztelni, használhatja-e szabad hello [Azure Cosmos DB emulátor](local-emulator.md), amely központilag telepíthető helyileg ingyenesen.</span><span class="sxs-lookup"><span data-stu-id="edf38-126">If you're looking for a cost effective method toodevelop and test Cosmos DB, you can use hello free [Azure Cosmos DB Emulator](local-emulator.md), which you can deploy locally at no cost.</span></span> 

<span data-ttu-id="edf38-127">**Hogyan állíthatom througput hello MongoDB API használatával?**</span><span class="sxs-lookup"><span data-stu-id="edf38-127">**How do I set througput using hello MongoDB API?**</span></span>

<span data-ttu-id="edf38-128">Nincs nincs MongoDB API bővítmény tooset átviteli sebességet.</span><span class="sxs-lookup"><span data-stu-id="edf38-128">There's no MongoDB API extension tooset throughput.</span></span> <span data-ttu-id="edf38-129">hello ajánljuk, toouse hello DocumentDB API, ahogy az [tooset hello átviteli hello DocumentDB API használatával a .NET-hez](#set-throughput-sdk).</span><span class="sxs-lookup"><span data-stu-id="edf38-129">hello recommendation is toouse hello DocumentDB API, as shown in [tooset hello throughput by using hello DocumentDB API for .NET](#set-throughput-sdk).</span></span>

## <a name="next-steps"></a><span data-ttu-id="edf38-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="edf38-130">Next steps</span></span>

<span data-ttu-id="edf38-131">További információ az üzembe helyezési és folyamatos bolygónk-méretezéssel Cosmos DB, toolearn lásd [particionálás és méretezést Cosmos DB,](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="edf38-131">toolearn more about provisioning and going planet-scale with Cosmos DB, see [Partitioning and scaling with Cosmos DB](partition-data.md).</span></span>
