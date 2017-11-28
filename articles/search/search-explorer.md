---
title: "AAA \"lekérdezheti az indexét (portál – Azure Search) |} Microsoft dokumentumok\""
description: "Keresési lekérdezés küldése hello Azure portál keresési ablakában ki."
services: search
manager: jhubbard
documentationcenter: 
author: ashmaka
ms.assetid: 8e524188-73a7-44db-9e64-ae8bf66b05d3
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 07/10/2017
ms.author: ashmaka
ms.openlocfilehash: 56bab3ef8a66eeb053fbbeb6d322acb6824fb34b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="query-an-azure-search-index-using-search-explorer-in-hello-azure-portal"></a><span data-ttu-id="fe129-103">Keresési ablak használatát hello Azure portál Azure Search-index lekérdezése</span><span class="sxs-lookup"><span data-stu-id="fe129-103">Query an Azure Search index using Search Explorer in hello Azure Portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fe129-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="fe129-104">Overview</span></span>](search-query-overview.md)
> * [<span data-ttu-id="fe129-105">Portál</span><span class="sxs-lookup"><span data-stu-id="fe129-105">Portal</span></span>](search-explorer.md)
> * [<span data-ttu-id="fe129-106">.NET</span><span class="sxs-lookup"><span data-stu-id="fe129-106">.NET</span></span>](search-query-dotnet.md)
> * [<span data-ttu-id="fe129-107">REST</span><span class="sxs-lookup"><span data-stu-id="fe129-107">REST</span></span>](search-query-rest-api.md)
> 
> 

<span data-ttu-id="fe129-108">Ez a cikk bemutatja, hogyan tooquery az Azure Search index használatával **keresési ablak** a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="fe129-108">This article shows you how tooquery an Azure Search index using **Search Explorer** in hello Azure portal.</span></span> <span data-ttu-id="fe129-109">Keresési ablak toosubmit egyszerű vagy teljes Lucene lekérdezési karakterláncok tooany meglévő index használhatja a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="fe129-109">You can use Search Explorer toosubmit simple or full Lucene query strings tooany existing index in your service.</span></span>

## <a name="open-hello-service-dashboard"></a><span data-ttu-id="fe129-110">Nyissa meg hello szolgáltatás irányítópult</span><span class="sxs-lookup"><span data-stu-id="fe129-110">Open hello service dashboard</span></span>
1. <span data-ttu-id="fe129-111">Kattintson a **összes erőforrás** hello Ugrás sávon a bal oldalán található hello hello [Azure-portálon](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).</span><span class="sxs-lookup"><span data-stu-id="fe129-111">Click **All resources** in hello jump bar on hello left side of hello [Azure portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).</span></span>
2. <span data-ttu-id="fe129-112">Válassza ki az Azure Search szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="fe129-112">Select your Azure Search service.</span></span>

## <a name="select-an-index"></a><span data-ttu-id="fe129-113">Index kiválasztása</span><span class="sxs-lookup"><span data-stu-id="fe129-113">Select an index</span></span>

<span data-ttu-id="fe129-114">Jelölje be hello indexet a hello toosearch **indexek** csempére.</span><span class="sxs-lookup"><span data-stu-id="fe129-114">Select hello index you would like toosearch from hello **Indexes** tile.</span></span>

   ![](./media/search-explorer/pick-index.png)

## <a name="open-search-explorer"></a><span data-ttu-id="fe129-115">A keresési ablak megnyitása</span><span class="sxs-lookup"><span data-stu-id="fe129-115">Open Search Explorer</span></span>

<span data-ttu-id="fe129-116">Kattintson a keresési ablak csempére tooslide nyitott hello keresősáv hello és az eredmények ablaktábláján.</span><span class="sxs-lookup"><span data-stu-id="fe129-116">Click on hello Search Explorer tile tooslide open hello search bar and results pane.</span></span>

   ![](./media/search-explorer/search-explorer-tile.png)

## <a name="start-searching"></a><span data-ttu-id="fe129-117">Indítson keresést</span><span class="sxs-lookup"><span data-stu-id="fe129-117">Start searching</span></span>

<span data-ttu-id="fe129-118">Hello keresési ablak használatakor is megadhat [lekérdezési paramétert](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) tooformulate hello lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="fe129-118">When using hello Search Explorer, you can specify [query parameters](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) tooformulate hello query.</span></span>

1. <span data-ttu-id="fe129-119">A **Lekérdezési karakterlánc** területen írjon be egy lekérdezést, majd kattintson a **Keresés** gombra.</span><span class="sxs-lookup"><span data-stu-id="fe129-119">In **Query string**, type a query and then press **Search**.</span></span> 

   <span data-ttu-id="fe129-120">hello lekérdezési karakterlánc van automatikusan elemzi hello megfelelő kérelem URL-cím toosubmit hello Azure Search REST API egy HTTP-kérelmet.</span><span class="sxs-lookup"><span data-stu-id="fe129-120">hello query string is automatically parsed into hello proper request URL toosubmit a HTTP request against hello Azure Search REST API.</span></span>   
   
   <span data-ttu-id="fe129-121">Használhat bármilyen érvényes egyszerű vagy teljes Lucene lekérdezési szintaxis toocreate hello kérelmet.</span><span class="sxs-lookup"><span data-stu-id="fe129-121">You can use any valid simple or full Lucene query syntax toocreate hello request.</span></span> <span data-ttu-id="fe129-122">Hello `*` karakter egyenértékű tooan üres vagy nincs megadva keresési, amely visszaadja az összes dokumentumot nem adott sorrendben.</span><span class="sxs-lookup"><span data-stu-id="fe129-122">hello `*` character is equivalent tooan empty or unspecified search that returns all documents in no particular order.</span></span>

2. <span data-ttu-id="fe129-123">A **eredmények**, azonos toohello hasznos visszaadott lekérdezés eredménye nyers JSON jelenjenek meg, a HTTP-válasz törzsében programozott módon kérelmek kiállításához.</span><span class="sxs-lookup"><span data-stu-id="fe129-123">In  **Results**, query results are presented in raw JSON, identical toohello payload returned in an HTTP Response Body when issuing requests programmatically.</span></span>

   ![](./media/search-explorer/search-bar.png)

## <a name="next-steps"></a><span data-ttu-id="fe129-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fe129-124">Next steps</span></span>

<span data-ttu-id="fe129-125">a következő erőforrások hello adja meg a további lekérdezési szintaxissal kapcsolatos információk és a példákat.</span><span class="sxs-lookup"><span data-stu-id="fe129-125">hello following resources provide additional query syntax information and examples.</span></span>

 + [<span data-ttu-id="fe129-126">Egyszerű lekérdezési szintaxis</span><span class="sxs-lookup"><span data-stu-id="fe129-126">Simple query syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 
 + [<span data-ttu-id="fe129-127">Lucene lekérdezési szintaxis</span><span class="sxs-lookup"><span data-stu-id="fe129-127">Lucene query syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 + [<span data-ttu-id="fe129-128">Lucene lekérdezési szintaxis példái</span><span class="sxs-lookup"><span data-stu-id="fe129-128">Lucene query syntax examples</span></span>](https://docs.microsoft.com/azure/search/search-query-lucene-examples) 
 + [<span data-ttu-id="fe129-129">OData szűrési kifejezés szintaxisa</span><span class="sxs-lookup"><span data-stu-id="fe129-129">OData Filter expression syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) 