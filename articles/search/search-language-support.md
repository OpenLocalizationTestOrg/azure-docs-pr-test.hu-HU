---
title: "Keresés több nyelv aaaAzure |} Microsoft Docs"
description: "Az Azure Search 56 nyelvet támogat, Lucene és természetes nyelvű feldolgozása technológia a Microsoft a nyelvi elemzőkkel kihasználva."
services: search
documentationcenter: 
author: yahnoosh
manager: pablocas
editor: 
ms.assetid: 55a00b44-804d-41bb-9c96-e6ea498616f5
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/23/2017
ms.author: jlembicz
ms.openlocfilehash: 9a2e567a82ee563521c12ea320f6c484a8e73f04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-index-for-documents-in-multiple-languages-in-azure-search"></a>Több nyelven is az Azure Search-dokumentumok index létrehozása
> [!div class="op_single_selector"]
>
> * [Portál](search-language-support.md)
> * [REST](https://msdn.microsoft.com/library/azure/dn879793.aspx)
> * [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.analyzername.aspx)
>
>

A nyelvi elemzőkkel unleashing hello power lehető legkönnyebben hello Indexdefiníció kereshető mező egy tulajdonságának beállításakor. Most ezt a lépést hello portálon teheti meg.

Az alábbiakban példaként bemutató képernyőképeket láthat hello Azure portál panel az Azure Search, amelyek lehetővé teszik a felhasználók toodefine a sémát indexeli. Ezen a panelen a felhasználók az összes hello mezők létrehozása és azok hello analyzer tulajdonság beállítása.

> [!IMPORTANT]
> Csak beállíthat egy nyelvi elemzőt során mező definícióját, és a egy új index létrehozását a hello szabad vagy egy új tooan meglévő mezőindex hozzáadásakor. Ellenőrizze, hogy teljesen meg minden attribútumokat, hello analyzer hello mező létrehozása során. Nem lehet képes tooedit hello attribútumok, vagy hello analyzer típusának módosítása után a módosítások mentéséhez.
>
>

## <a name="define-a-new-field-definition"></a>Új mező definíció megadása
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) és a keresési szolgáltatás nyitott hello szolgáltatás panelre.
2. Kattintson a **index hozzáadása** hello parancsban hello szolgáltatás irányítópult toostart hello tetején sávot új index, vagy nyisson meg egy meglévő index tooset új mező hozzáadása egy analyzer tooan meglévő-index.
3. hello mezők panel jelenik meg, hello séma hello index meghatározása lehetőségeit is ide hello Analyzer használt, egy nyelvi elemzőt kiválasztásakor.
4. A mezőket indítsa el mező definíciójának megadása hello adattípus kiválasztása és attribútumok toomark hello mezőben teljes szöveges keresési eredmények között, használható értékkorlátozás navigációs szerkezetben lekérhető, kereshető rendezhető, és a beállítása stb.
5. Mielőtt továbblép a következő mező toohello, nyissa meg a hello **Analyzer** fülre.

![][1]
*egy elemző tooselect kattintson hello Analyzer lapon hello mezők panelen*

## <a name="choose-an-analyzer"></a>Válasszon egy elemző eszköz
1. Görgessen toofind hello mező meghatározásakor.
2. Ha még nem kereshető, hello mezőt, hello jelölőnégyzet most toomark kattintson rá **kereshető**.
3. Kattintson a hello Analyzer terület toodisplay hello listája elérhető elemzőkkel.
4. Válassza ki a hello analyzer toouse szeretné.

![][2]
*Válasszon ki egy támogatott hello elemzőkkel az egyes mezők*

Alapértelmezés szerint az összes kereshető mezőt használja hello [szabványos Lucene analyzer](http://lucene.apache.org/core/4_10_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) Ez az nyelvi független. tooview hello teljes listáját támogatott elemzőkkel [nyelvi támogatás az Azure Search](https://msdn.microsoft.com/library/azure/dn879793.aspx).

Ha egy mező hello nyelvi elemzőt van jelölve, azt fogja használni minden egyes indexelési és keresési kérelemmel mező. Amikor a lekérdezés különböző lekérdezések használatával több mezőkkel, hello lekérdezés egymástól függetlenül általi feldolgozásának hello jobb elemzőkkel minden mező.

Sok webes és mobilalkalmazásokhoz kiszolgálására körül hello földgömb különböző nyelveken használó felhasználók. Már lehetséges toodefine index forgatókönyv esetén például ez hozzon létre egy mezőt, minden támogatott nyelven.

![][3]
*Minden támogatott nyelven Leírás mezőt Indexdefiníció*

Ha hello nyelvi hello ügynök lekérdezés kiadása ismert, a keresési kérelem lehet-e a hatókörön belüli tooa adott mező hello segítségével **searchFields** lekérdezési paraméter. hello következő lekérdezést kap, csak a Lengyel hello leírást szemben:

`https://[service name].search.windows.net/indexes/[index name]/docs?search=darmowy&searchFields=description_pl&api-version=2016-09-01`

Akkor is lekérdezheti az indexét hello portálról használatával **keresési ablak** toopaste az egy lekérdezésben hasonló toohello, amelyik fentebb is látható. Keresési ablak hello parancssáv hello szolgáltatás panelen a érhető el. Lásd: [hello portálon az Azure Search-index lekérdezése](search-explorer.md) részleteiről.

Néha hello nyelvi hello ügynök lekérdezés kiadása nem ismert, mely eset hello a lekérdezés adhatók ki minden mezőkkel egyidejűleg. Ha szükséges, egy bizonyos nyelvi eredményez preferencia definiálható [profilok pontozási](https://msdn.microsoft.com/library/azure/dn798928.aspx). Hello az alábbi példában a találat hello leírásában angolul lesz pontozni magasabb relatív toomatches, lengyel és francia:

    "scoringProfiles": [
      {
        "name": "englishFirst",
        "text": {
          "weights": { "description_en": 2 }
        }
      }
    ]

`https://[service name].search.windows.net/indexes/[index name]/docs?search=Microsoft&scoringProfile=englishFirst&api-version=2016-09-01`

Ha a .NET-fejlesztők, vegye figyelembe, hogy a nyelvi elemzőkkel hello segítségével konfigurálhatja [Azure Search .NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Search). hello legújabb kiadásának hello Microsoft nyelvi elemzőkkel is támogatását is magában foglalja.

<!-- Image References -->
[1]: ./media/search-language-support/AnalyzerTab.png
[2]: ./media/search-language-support/SelectAnalyzer.png
[3]: ./media/search-language-support/IndexDefinition.png
