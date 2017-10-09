---
title: "az Azure Search aaaData feltöltés |} Microsoft Docs"
description: Ismerje meg, hogyan tooupload adatok tooan az Azure Search index.
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
editor: 
tags: 
ms.assetid: aa8d47c1-4ae6-4209-a8ce-48d5a9474707
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: ashmaka
ms.openlocfilehash: a95eae94f72c1d0926804ff7e1152f21773fcabf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-data-tooazure-search"></a>Töltse fel az adatok tooAzure keresése
> [!div class="op_single_selector"]
> * [Áttekintés](search-what-is-data-import.md)
> * [.NET](search-import-data-dotnet.md)
> * [REST](search-import-data-rest-api.md)
> 
> 

Nincsenek két módon toopopulate egy index ugyanezzel az adatokat. hello első lehetőség az adatok manuális küldését hello Azure Search használatával hello indexbe [REST API](search-import-data-rest-api.md) vagy [.NET SDK](search-import-data-dotnet.md). hello második lehetőség van túl[egy támogatott adatforrásra pont](search-indexer-overview.md) tooyour index, és automatikusan hello adatok bekérésére Azure Search segítségével.

## <a name="push-data-tooan-index"></a>Leküldéses az tooan indexe
Ez a megközelítés hivatkozik az adatok tooAzure keresési toomake küldése tooprogrammatically elérhetővé a kereséshez. Az alkalmazások (például operations toobe dinamikus készlet adatbázisok szinkronban kell keresni) nagyon alacsony késési követelményekkel rendelkező hello leküldési modelljével egyetlen választása esetén.

Használhatja a hello [REST API](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents) vagy [.NET SDK](search-import-data-dotnet.md) toopush adatok tooan index. Jelenleg nincs hello portálon előtte adatot eszköz támogatása.

Ez a módszer nem rugalmasabb, mint hello lekéréses modell, mert is tölthet fel dokumentumokat, külön-külön és kötegek (mentése kötegelt vagy 16 MB too1000, korlát átlépése következik be előbb). hello leküldési modelljével is lehetővé teszi tooupload dokumentumok tooAzure keresési függetlenül adatai.

Azure Search azt értelmezni tudja hello adatformátum JSON, és hello dataset található összes dokumentum rendelkeznie kell a sémát indexeli definiált toofields megfeleltetni mezőt. 

## <a name="pull-data-into-an-index"></a>Adatok lekérése indexbe
hello lekéréses modell bejárásokat egy támogatott adatforrásra, és automatikusan feltölti a hello adatok az indexbe. Az Azure Search szolgáltatásban ez a képesség az *indexelőkön* keresztül valósul meg, ami jelenleg a [Blob Storage](search-howto-indexing-azure-blob-storage.md), a [Table Storage](search-howto-indexing-azure-tables.md), az [Azure Cosmos DB](http://aka.ms/documentdb-search-indexer), az [Azure SQL Database és az Azure virtuális gépekhez készült SQL Server](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) számára érhető el. 

Indexelők egy index tooa adatforrás csatlakoztatása (általában egy tábla, nézet vagy ezzel egyenértékű struktúra), és képezze le a forrás mezők tooequivalent mezők hello index. Végrehajtásakor hello sorhalmaz automatikusan átalakított tooJSON és betölti a megadott index hello. Összes indexelő támogatja az ütemezést, így megadható, hogy milyen gyakran hello adatok toobe frissíteni. A legtöbb indexelők adja meg a változások követését, ha az adatforrás hello támogatja. Változások követése és a törlések tooexisting dokumentumok továbbá toorecognizing új dokumentumok, indexelők eltávolítása hello kell tooactively az indexben hello adatok kezelésére. 

Az indexelő funkció ki van téve, a hello [Azure-portálon](search-import-data-portal.md), hello [REST API](/rest/api/searchservice/Indexer-operations), és hello [.NET SDK](/dotnet/api/microsoft.azure.search.indexersoperations). 

Egy előny toousing hello portál, hogy Azure Search általában generálhat egy alapértelmezett sémát indexeli az Ön által hello forrás adatkészlet hello metaadatait. Amíg hello index dolgoz fel, melyik hello után csak a séma módosításokat engedélyezett azok, amelyek nem igényelnek újraindexelés hello létrehozott index módosíthatja. Ha azt szeretné, hogy toomake hatás hello módosítások közvetlenül hello séma, kellene toorebuild hello index. 

Miután elkészült hello index, használhatja **keresési ablak** a hello portál parancssáv ellenőrzési lépésben.

## <a name="query-an-index-using-search-explorer"></a>Index lekérdezése a Keresési ablak használatával

A gyors tooperform hello dokumentumfeltöltés előzetes ellenőrzése toouse **keresési ablak** hello portálon. hello explorer lehetővé teszi egy index lekérdezése nélkül toowrite összes kódot. hello keresési élményt biztosít a alapul alapértelmezett beállítások, például hello [egyszerű Szintaxishiba](/rest/api/searchservice/simple-query-syntax-in-azure-search) és az alapértelmezett [searchMode lekérdezésparaméter](/rest/api/searchservice/search-documents). Eredményeinek JSON-ban, hogy a teljes dokumentum hello vizsgálhatja meg.

> [!TIP]
> Számos [Azure Search-Kódminták](https://github.com/Azure-Samples/?utf8=%E2%9C%93&query=search) közé tartozik a beágyazott vagy azonnal elérhetők legyenek adatkészletek ajánlat egy egyszerűen tooget elindult. hello portal minta indexelő és adatforrás ("realestate-us-minta" nevű) kis ingatlan dataset álló is biztosít. Hello előre konfigurált indexelő hello minta az adatforrásban való futtatásakor az index létrehozása és lekérdezhető keresési ablak, vagy írhat kódot dokumentumok betöltését.
