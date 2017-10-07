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
# <a name="query-an-azure-search-index-using-search-explorer-in-hello-azure-portal"></a>Keresési ablak használatát hello Azure portál Azure Search-index lekérdezése
> [!div class="op_single_selector"]
> * [Áttekintés](search-query-overview.md)
> * [Portál](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

Ez a cikk bemutatja, hogyan tooquery az Azure Search index használatával **keresési ablak** a hello Azure-portálon. Keresési ablak toosubmit egyszerű vagy teljes Lucene lekérdezési karakterláncok tooany meglévő index használhatja a szolgáltatást.

## <a name="open-hello-service-dashboard"></a>Nyissa meg hello szolgáltatás irányítópult
1. Kattintson a **összes erőforrás** hello Ugrás sávon a bal oldalán található hello hello [Azure-portálon](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).
2. Válassza ki az Azure Search szolgáltatást.

## <a name="select-an-index"></a>Index kiválasztása

Jelölje be hello indexet a hello toosearch **indexek** csempére.

   ![](./media/search-explorer/pick-index.png)

## <a name="open-search-explorer"></a>A keresési ablak megnyitása

Kattintson a keresési ablak csempére tooslide nyitott hello keresősáv hello és az eredmények ablaktábláján.

   ![](./media/search-explorer/search-explorer-tile.png)

## <a name="start-searching"></a>Indítson keresést

Hello keresési ablak használatakor is megadhat [lekérdezési paramétert](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) tooformulate hello lekérdezés.

1. A **Lekérdezési karakterlánc** területen írjon be egy lekérdezést, majd kattintson a **Keresés** gombra. 

   hello lekérdezési karakterlánc van automatikusan elemzi hello megfelelő kérelem URL-cím toosubmit hello Azure Search REST API egy HTTP-kérelmet.   
   
   Használhat bármilyen érvényes egyszerű vagy teljes Lucene lekérdezési szintaxis toocreate hello kérelmet. Hello `*` karakter egyenértékű tooan üres vagy nincs megadva keresési, amely visszaadja az összes dokumentumot nem adott sorrendben.

2. A **eredmények**, azonos toohello hasznos visszaadott lekérdezés eredménye nyers JSON jelenjenek meg, a HTTP-válasz törzsében programozott módon kérelmek kiállításához.

   ![](./media/search-explorer/search-bar.png)

## <a name="next-steps"></a>Következő lépések

a következő erőforrások hello adja meg a további lekérdezési szintaxissal kapcsolatos információk és a példákat.

 + [Egyszerű lekérdezési szintaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 
 + [Lucene lekérdezési szintaxis](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 + [Lucene lekérdezési szintaxis példái](https://docs.microsoft.com/azure/search/search-query-lucene-examples) 
 + [OData szűrési kifejezés szintaxisa](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) 