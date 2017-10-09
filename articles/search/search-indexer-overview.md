---
title: az Azure Search aaaIndexers |} Microsoft Docs
description: "Azure SQL database, Azure Cosmos DB vagy az Azure storage tooextract kereshető adatokat bejárható és feltöltése az Azure Search-index."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 34a7694c-8fd9-46b1-8900-cefdd7236323
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: heidist
ms.openlocfilehash: 6a816252ec5d6032491a12651c05cb1fe77d3d1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="indexers-in-azure-search"></a>Indexelők az Azure Search szolgáltatásban
> [!div class="op_single_selector"]
>
> * [Áttekintés](search-indexer-overview.md)
> * [Portál](search-import-data-portal.md)
> * [Azure SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
> * [Azure Cosmos DB](search-howto-index-documentdb.md)
> * [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
> * [Azure Table Storage](search-howto-indexing-azure-tables.md)
>
>

Egy **indexelő** az Azure Search olyan kapcsolat, amely kereshető adatokat és a metaadatokat a külső adatforráshoz, és feltölti az index alapján mezők hozzárendelések hello index és az adatforrás között. Ez a megközelítés oka néha hivatkozott tooas egy lekéréses modell hello szolgáltatás kéri le az adatok anélkül, hogy toowrite leküldéses értesítések adatok tooan index összes kódot.

Az indexelő használnak, amikor egyetlen hello azt jelenti, hogy adatfeldolgozást, vagy az hello használata az indexelő feltöltését csupán néhány hello az indexében lévő mezőket tartalmazó módszerek egyfajta kombinációját alkalmazza.

Az indexelők futtatása történhet igény szerint vagy ismétlődő adatfrissítési ütemterv alapján, akár 15 perces gyakorisággal is. Az ennél gyakoribb frissítésekhez olyan leküldési modellre van szükség, amely egyszerre frissíti az adatokat az Azure Search szolgáltatásban és a külső adatforrásban is.

## <a name="approaches-for-creating-and-managing-indexers"></a>Indexelők létrehozásának és kezelésének módszerei
Az olyan, mindenki számára elérhető indexelők esetében, mint az Azure SQL vagy az Azure Cosmos DB, az indexelők létrehozása és kezelése a következő módszerekkel történhet:

* [Portál &gt; Adatok importálása varázsló ](search-get-started-portal.md)
* [Szolgáltatás REST API-ja](https://msdn.microsoft.com/library/azure/dn946891.aspx)
* [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.search.iindexersoperations.aspx)

## <a name="basic-configuration-steps"></a>Alapszintű konfigurációs lépések
Indexelő kínálhat egyedi toohello adatforrás funkciókat. Ezért az indexelő- vagy az adatforrás-konfiguráció egyes szempontjai az indexelő típusától függően változnak. Azonban az összes indexelő megosztása hello ugyanazon alapvető összetételűnek és követelményeit. Közös tooall indexelők tartoznak az alábbi lépéseket.

### <a name="step-1-create-an-index"></a>1. lépés: Index létrehozása
Az indexelő fog automatizálhatja az egyes feladatok kapcsolódó toodata adatfeldolgozást, de az index létrehozása nem közül. Előfeltételként olyan előre meghatározott indexre van szükség, amelynek mezői egyeznek a külső adatforrás mezőivel. További információk az indexek strukturálásáról: [Index létrehozása (Azure Search REST API)](https://msdn.microsoft.com/library/azure/dn798941.aspx).

### <a name="step-2-create-a-data-source"></a>2. lépés: Adatforrás létrehozása
Az indexelők olyan **adatforrásokból** kérnek le adatokat, amelyek például kapcsolati karakterláncokat tartalmaznak. Jelenleg a következő adatforrások hello támogatottak:

* [Azure SQL Database vagy SQL Server egy Azure virtuális gépen](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-documentdb.md)
* [Az Azure Blob storage](search-howto-indexing-azure-blob-storage.md), PDF, Office-dokumentumok, HTML vagy XML tooextract szöveg használt
* [Azure Table Storage](search-howto-indexing-azure-tables.md)

Adatforrások konfigurálva és felügyelni azokat, ami azt jelenti, egy adatforrás több indexelők tooload által használható egynél több index egyszerre használó hello indexelők függetlenül.

### <a name="step-3create-and-schedule-hello-indexer"></a>3. lépés: hozzon létre, és ütemezés szerinti hello indexelő
hello indexelő meghatározása egy olyan konstrukció hello index, az adatforrás és egy ütemezés szerint. Az indexelő hivatkozhat egy adatforrást egy másik szolgáltatástól, feltéve, hogy az adatforrás tartozik hello ugyanahhoz az előfizetéshez. További információk az indexelők strukturálásáról: [Indexelő létrehozása (Azure Search REST API)](https://msdn.microsoft.com/library/azure/dn946899.aspx).

## <a name="next-steps"></a>Következő lépések
Most, hogy alapszintű hello-érdemes, hello következő lépésre tooreview követelmények és feladatok adott tooeach adatforrás típusa.

* [Azure SQL Database vagy SQL Server egy Azure virtuális gépen](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-documentdb.md)
* [Az Azure Blob Storage](search-howto-indexing-azure-blob-storage.md), PDF, Office-dokumentumok, HTML vagy XML tooextract szöveg használt
* [Azure Table Storage](search-howto-indexing-azure-tables.md)
* [Az indexelő CSV blobok használatával hello Azure keresési Blob indexelő](search-howto-index-csv-blobs.md)
* [JSON-blobok indexelése az Azure Search Blob indexelőjével](search-howto-index-json-blobs.md)
