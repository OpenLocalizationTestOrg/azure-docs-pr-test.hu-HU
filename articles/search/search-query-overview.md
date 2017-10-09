---
title: aaaQuery az Azure Search-Index |} Microsoft Docs
description: "Hozza létre az Azure search keresési lekérdezés, és használja a keresési paraméterek toofilter és rendezési keresési eredmények."
services: search
manager: jhubbard
documentationcenter: 
author: ashmaka
ms.assetid: 69205d7a-363f-4b92-a53f-6ca818a3d2c7
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 04/26/2017
ms.author: ashmaka
ms.openlocfilehash: 4a5ffffe179695fc09446760e21a738dd36c29b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="query-your-azure-search-index"></a>Az Azure Search-index lekérdezése
> [!div class="op_single_selector"]
> * [Áttekintés](search-query-overview.md)
> * [Portál](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

Amikor keresési kérelmeket tooAzure keresési, számos paraméterek hello tényleges szavak, amelyeket az alkalmazás hello keresőmezőbe együtt adható meg. A lekérdezési paraméterek lehetővé teszik tooachieve néhány hello mélyebb irányítását [teljes szöveges keresési élményt biztosít](search-lucene-query-architecture.md).

Az alábbiakban felsoroljuk, amely röviden ismerteti a gyakori használati módjai hello lekérdezési paraméterek az Azure Search. Lekérdezés-paraméterek és azok viselkedését teljes körét talál hello részletes hello lapok [REST API](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) és [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters#microsoft_azure_search_models_searchparameters#properties_summary).

## <a name="types-of-queries"></a>A lekérdezések típusai
Az Azure Search biztosít több beállítások toocreate rendkívül hatékony lekérdezést. hello két fő típusok használandó lekérdezés `search` és `filter`. A `search` lekérdezés az összes vagy több kifejezést keresi *kereshető* mezők az indexben, és működik, hello keresőmotort, például a Google vagy a Bing toowork teheti meg. A `filter` lekérdezés egy logikai kifejezés kiértékelését végzi el az index összes *filterable* (szűrhető) mezőjén. Eltérően `search` a lekérdezések `filter` lekérdezések megfelelő hello pontos tartalmát egy, ami azt jelenti, azok esetében az elemzőnek kis-és nagybetűket.

A keresések és a szűrések együtt vagy külön-külön is alkalmazhatók. Ha együtt használva, hello szűrő van alkalmazva az első toohello teljes indexe, és majd hello keresési hello eredmények hello szűrő hajtja végre. Szűrők ezért lehet egy hasznos módszer tooimprove lekérdezési teljesítményt, mivel csökkentik a keresési lekérdezés igények tooprocess hello dokumentumok hello beállítása.

hello szűrőkifejezéseket szintaxisa a következő hello részhalmazának [OData szűrő nyelvi](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search). Keresési lekérdezések használhatja bármelyik hello [egyszerűsített szintaxist](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search) vagy hello [Lucene lekérdezés szintaxisát](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search) alábbiak ismertetik.

### <a name="simple-query-syntax"></a>Egyszerű lekérdezési szintaxis
Hello [egyszerű lekérdezés szintaxisát](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search) hello alapértelmezett lekérdezési nyelv az Azure Search használt. hello egyszerű lekérdezés szintaxisát hello AND, OR, nem, kifejezést, utótag és sorrend operátorok közös keresési operátorokat számos támogat.

### <a name="lucene-query-syntax"></a>Lucene lekérdezési szintaxis
Hello [Lucene lekérdezés szintaxisát](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search) lehetővé teszi az széles körben fogják alkalmazni toouse hello és kifejező lekérdezési nyelv részeként [Apache Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html).

A lekérdezés szintaxisa használata lehetővé teszi a tooeasily eléréséhez a következő képességeket hello: [mező hatókörű lekérdezések](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_fields), [intelligens egyeztetésű keresési](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_fuzzy), [közelségi kapcsolat keresési](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_proximity), [ távon kiemelési](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_termboost), [reguláris kifejezés keresési](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_regex), [helyettesítő karakteres keresés](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_wildcard), [szintaxis – alapok](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_syntax), és [lekérdezi a használatával logikai operátorok](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_boolean).

## <a name="ordering-results"></a>Az eredmények rendezése
Keresési lekérdezés eredményeit fogadásakor kérheti, hogy az Azure Search szolgál értékeket egy adott mező szerint rendezett hello eredmények. Alapértelmezés szerint az Azure Search rendelések hello keresési eredmény származó minden egyes dokumentum keresési pontszám hello rangsorát [TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).

Ha azt szeretné, hogy az eredmények hello keresési pontszám-től eltérő érték szerint rendezett Azure Search tooreturn, használhatja a hello `orderby` keresési feltétel. Megadhatja a hello hello értékének `orderby` paraméter tooinclude mező nevét, és meghívja a toohello [ `geo.distance()` függvény](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search) térinformatikai értékeket. Minden egyes kifejezés követhetnek `asc` , hogy eredmények növekvő sorrendben, kért tooindicate és `desc` , hogy az eredmények csökkenő sorrendben kért tooindicate. hello alapértelmezett rangsorolási növekvő sorrend megadásához.

## <a name="paging"></a>Lapozás
Az Azure Search segítségével könnyen tooimplement a keresési eredmények lapozást. Hello segítségével `top` és `skip` paraméterek, akkor is zökkenőmentes keresési kérelmeket kibocsátó, amelyek lehetővé teszik, hogy könnyen engedélyezheti a helyes keresési felhasználói felületi eljárások kezelhető, rendezett alkészletek a tooreceive hello teljes beállítása a keresési eredmények. Ezen eredmények kisebb részhalmaza fogadásakor is kaphat dokumentumok hello száma a keresési eredmények hello teljes készletében.

Keresési eredmények hello cikkben lapozás kapcsolatban részletesebb [hogyan toopage keresési eredmények az Azure Search](search-pagination-page-layout.md).

## <a name="hit-highlighting"></a>Találatok kiemelése
Az Azure Search fogalmazás hello pontos keresés eredménye hello keresési lekérdezésnek megfelelő része legyen könnyen hello segítségével `highlight`, `highlightPreTag`, és `highlightPostTag` paraméterek. Megadhat, amely *kereshető* mezőket kell rendelkeznie az egyező szöveg megcélzó, valamint megadó karakterlánc tooappend toohello kezdő címkék és hello végét egyező szöveg Azure a keresés pontos hello adja vissza.

## <a name="try-out-query-syntax"></a>Lekérdezési szintaxis tesztelése

hello legjobb módja toounderstand szintaktikai eltérések van lekérdezések elküldése, és az eredmények megtekintésével.

+ Használjon [keresési ablak](search-explorer.md) a hello Azure-portálon. Üzembe helyezésével [hello minta index](search-get-started-portal.md), hello index lekérdezése az eszközökkel hello portálon percben.

+ Használjon [Fiddler](search-fiddler.md) vagy Chrome Postman toosubmit lekérdezések tooan index, hogy már feltöltött tooyour keresési szolgáltatást. Mindkét eszközök támogatják a többi hívások tooan HTTP-végpont. 
