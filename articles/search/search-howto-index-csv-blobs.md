---
title: "az Azure Search-indexelőt, blob blobok aaaIndexing CSV |} Microsoft Docs"
description: Ismerje meg, hogyan tooindex CSV-blobok az Azure Search
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: ed3c9cff-1946-4af2-a05a-5e0b3d61eb25
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 12/15/2016
ms.author: eugenesh
ms.openlocfilehash: f2b1ce824e62c5b3f6155c6e88887897cf1a8eae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>Az Azure Search-indexelőt, blob CSV blobok indexelő
Alapértelmezés szerint [blob Azure Search-indexelőt](search-howto-indexing-azure-blob-storage.md) elemez szöveg blobok szöveg egyetlen adattömb jelölik. Azonban a CSV-adatokat tartalmazó BLOB, érdemes tootreat hello blob minden sora egy különálló dokumentumként. A megadott hello például a következő tagolt szöveg: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

érdemes tooparse azt a 2-dokumentumok esetében minden egyes tartalmazó "id", "datePublished" és "címkék" mezőket.

Ebben a cikkben megtudhatja, hogyan tooparse CSV-blobok az Azure Search blob indexelőt. 

> [!IMPORTANT]
> Ez a funkció jelenleg előzetes verzió. Csak a hello REST API verziójával **2015-02-28-előzetes**. Adjon ne feledje, az előzetes API-k tesztelésében és értékelésében készült, és nem használható üzemi környezetben. 
> 
> 

## <a name="setting-up-csv-indexing"></a>Fürt megosztott kötetei szolgáltatás indexelő beállítása
tooindex CSV blobot definíció létrehozása vagy módosítása egy indexelő a hello `delimitedText` elemzése mód:  

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

További részleteket a hello indexelő API létrehozása, tekintse meg [létrehozása indexelő](search-api-indexers-2015-02-28-preview.md#create-indexer).

`firstLineContainsHeaders`azt jelzi, hogy minden egyes blob hello első (kötelező) sort tartalmaz-e a fejlécek.
Blobok nem tartalmaznak egy kezdeti fejlécsort, ha hello fejlécek hello indexelő konfigurációban kell megadni: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

Jelenleg az egyetlen hello UTF-8 kódolást támogatja. Emellett csak hello vesszővel `','` , hello elválasztó karakter támogatott. Ha segítségre más kódolások vagy elválasztó karaktert, tudassa velünk, a [a UserVoice webhelyén](https://feedback.azure.com/forums/263029-azure-search).

> [!IMPORTANT]
> Hello tagolt szöveges módot, az Azure Search elemzés használata esetén azt feltételezi, hogy az adatforrás összes BLOB lesz-e a fürt megosztott kötetei szolgáltatás. Ha toosupport CSV vegyesen van szüksége, és ugyanazt a hello blobok nem CSV-adatforrás, adjon ossza meg velünk a [a UserVoice webhelyén](https://feedback.azure.com/forums/263029-azure-search).
> 
> 

## <a name="request-examples"></a>Példák
Ez minden üzembe együtt, az alábbiakban hello teljes adattartalmat példák. 

Adatforrás: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Az indexelő:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-search-better"></a>Segítsen az Azure Search továbbfejlesztésében
Ha a szolgáltatás-kérelmek vagy ötleteket javításai, lépjen kapcsolatba a toous a [UserVoice webhelyén](https://feedback.azure.com/forums/263029-azure-search/).

