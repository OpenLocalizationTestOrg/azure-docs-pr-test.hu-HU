---
title: "Index lekérdezése (portál – Azure Search) | Microsoft Docs"
description: "Keresési lekérdezés küldése az Azure portál Keresési ablakában."
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
ms.openlocfilehash: dd68d8ed073bf7b8666ddef35a2f1f84df690b4b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="query-an-azure-search-index-using-search-explorer-in-the-azure-portal"></a><span data-ttu-id="824cc-103">Azure Search-index lekérdezése a keresési ablakkal az Azure Portalon</span><span class="sxs-lookup"><span data-stu-id="824cc-103">Query an Azure Search index using Search Explorer in the Azure Portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="824cc-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="824cc-104">Overview</span></span>](search-query-overview.md)
> * [<span data-ttu-id="824cc-105">Portál</span><span class="sxs-lookup"><span data-stu-id="824cc-105">Portal</span></span>](search-explorer.md)
> * [<span data-ttu-id="824cc-106">.NET</span><span class="sxs-lookup"><span data-stu-id="824cc-106">.NET</span></span>](search-query-dotnet.md)
> * [<span data-ttu-id="824cc-107">REST</span><span class="sxs-lookup"><span data-stu-id="824cc-107">REST</span></span>](search-query-rest-api.md)
> 
> 

<span data-ttu-id="824cc-108">Ez a cikk bemutatja, hogyan kérdezhet le Azure Search-indexeket a **keresési ablakkal** az Azure Portalon.</span><span class="sxs-lookup"><span data-stu-id="824cc-108">This article shows you how to query an Azure Search index using **Search Explorer** in the Azure portal.</span></span> <span data-ttu-id="824cc-109">A keresési ablakkal egyszerű vagy teljes Lucene lekérdezési karakterláncokat küldhet el a szolgáltatásban lévő bármely indexnek.</span><span class="sxs-lookup"><span data-stu-id="824cc-109">You can use Search Explorer to submit simple or full Lucene query strings to any existing index in your service.</span></span>

## <a name="open-the-service-dashboard"></a><span data-ttu-id="824cc-110">A szolgáltatás irányítópultjának megnyitása</span><span class="sxs-lookup"><span data-stu-id="824cc-110">Open the service dashboard</span></span>
1. <span data-ttu-id="824cc-111">Az [Azure Portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) bal oldali gyorselérési sávján kattintson a **Minden erőforrás** elemre.</span><span class="sxs-lookup"><span data-stu-id="824cc-111">Click **All resources** in the jump bar on the left side of the [Azure portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).</span></span>
2. <span data-ttu-id="824cc-112">Válassza ki az Azure Search szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="824cc-112">Select your Azure Search service.</span></span>

## <a name="select-an-index"></a><span data-ttu-id="824cc-113">Index kiválasztása</span><span class="sxs-lookup"><span data-stu-id="824cc-113">Select an index</span></span>

<span data-ttu-id="824cc-114">Válassza ki az **Indexek** csempén azt az indexet, amelyből keresni kíván.</span><span class="sxs-lookup"><span data-stu-id="824cc-114">Select the index you would like to search from the **Indexes** tile.</span></span>

   ![](./media/search-explorer/pick-index.png)

## <a name="open-search-explorer"></a><span data-ttu-id="824cc-115">A keresési ablak megnyitása</span><span class="sxs-lookup"><span data-stu-id="824cc-115">Open Search Explorer</span></span>

<span data-ttu-id="824cc-116">Kattintson a Keresési ablak csempére a keresősáv és az eredményeket tartalmazó ablaktábla megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="824cc-116">Click on the Search Explorer tile to slide open the search bar and results pane.</span></span>

   ![](./media/search-explorer/search-explorer-tile.png)

## <a name="start-searching"></a><span data-ttu-id="824cc-117">Indítson keresést</span><span class="sxs-lookup"><span data-stu-id="824cc-117">Start searching</span></span>

<span data-ttu-id="824cc-118">A Keresési ablak használatakor [lekérdezési paraméterekkel](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) határozhatja meg a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="824cc-118">When using the Search Explorer, you can specify [query parameters](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) to formulate the query.</span></span>

1. <span data-ttu-id="824cc-119">A **Lekérdezési karakterlánc** területen írjon be egy lekérdezést, majd kattintson a **Keresés** gombra.</span><span class="sxs-lookup"><span data-stu-id="824cc-119">In **Query string**, type a query and then press **Search**.</span></span> 

   <span data-ttu-id="824cc-120">A rendszer automatikusan átalakítja a lekérdezési sztringet a megfelelő kérelmi URL-címmé, és HTTP-kérelmet küldjön az Azure Search REST API-nak.</span><span class="sxs-lookup"><span data-stu-id="824cc-120">The query string is automatically parsed into the proper request URL to submit a HTTP request against the Azure Search REST API.</span></span>   
   
   <span data-ttu-id="824cc-121">A kérelem létrehozásához bármilyen érvényes egyszerű vagy teljes Lucene lekérdezési szintaxist használhat.</span><span class="sxs-lookup"><span data-stu-id="824cc-121">You can use any valid simple or full Lucene query syntax to create the request.</span></span> <span data-ttu-id="824cc-122">A `*` karakter üres vagy nem meghatározott keresésnek felel meg, amely az összes dokumentumot visszaadja véletlenszerű sorrendben.</span><span class="sxs-lookup"><span data-stu-id="824cc-122">The `*` character is equivalent to an empty or unspecified search that returns all documents in no particular order.</span></span>

2. <span data-ttu-id="824cc-123">Az **Eredmények** területen a lekérdezési eredmények nyers JSON formátumban jelennek meg, hasonlóan a HTTP-választörzsekben visszaadott hasznos adatokhoz, amikor programozott módon ad ki kéréseket.</span><span class="sxs-lookup"><span data-stu-id="824cc-123">In  **Results**, query results are presented in raw JSON, identical to the payload returned in an HTTP Response Body when issuing requests programmatically.</span></span>

   ![](./media/search-explorer/search-bar.png)

## <a name="next-steps"></a><span data-ttu-id="824cc-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="824cc-124">Next steps</span></span>

<span data-ttu-id="824cc-125">Az alábbi forrásokban további tudnivalókat és példákat találhat a lekérdezési szintaxisokról.</span><span class="sxs-lookup"><span data-stu-id="824cc-125">The following resources provide additional query syntax information and examples.</span></span>

 + [<span data-ttu-id="824cc-126">Egyszerű lekérdezési szintaxis</span><span class="sxs-lookup"><span data-stu-id="824cc-126">Simple query syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 
 + [<span data-ttu-id="824cc-127">Lucene lekérdezési szintaxis</span><span class="sxs-lookup"><span data-stu-id="824cc-127">Lucene query syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 + [<span data-ttu-id="824cc-128">Lucene lekérdezési szintaxis példái</span><span class="sxs-lookup"><span data-stu-id="824cc-128">Lucene query syntax examples</span></span>](https://docs.microsoft.com/azure/search/search-query-lucene-examples) 
 + [<span data-ttu-id="824cc-129">OData szűrési kifejezés szintaxisa</span><span class="sxs-lookup"><span data-stu-id="824cc-129">OData Filter expression syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) 