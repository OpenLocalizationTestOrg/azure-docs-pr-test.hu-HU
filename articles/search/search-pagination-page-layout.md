---
title: "aaaHow toopage keresési találatok az Azure Search |} Microsoft Docs"
description: "Az Azure Search, egy üzemeltetett felhőalapú keresőszolgáltatás, a Microsoft Azure tördelési."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
ms.assetid: a0a1d315-8624-4cdf-b38e-ba12569c6fcc
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 08/29/2016
ms.author: heidist
ms.openlocfilehash: e3abc1ca4d5994b0a77955379081a4fcfa5a7fa7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toopage-search-results-in-azure-search"></a>Hogyan toopage keresési eredmények az Azure Search
Ez a cikk hogyan toouse hello Azure Search szolgáltatás REST API tooimplement szokásos megoldások szabványos elemeit a keresési eredmények, például az érintett teljes, a dokumentum beolvasása, a rendezési sorrend és a navigációs nyújt útmutatást.

Az alábbiakban leírt minden esetben hello keresztül megadott lapokkal kapcsolatos beállításokat Ez hozzájárul az adatok és információk tooyour keresési eredmények oldalának [keresés a dokumentum](http://msdn.microsoft.com/library/azure/dn798927.aspx) küldött kérelmek tooyour Azure Search szolgáltatás. A kérelemnek tartalmaznia egy GET parancs, a elérési utat, és a lekérdezési paraméterek, amely tájékoztatja a mi vonatkozó kérelem hello szolgáltatást és a hogyan tooformulate hello válasz.

> [!NOTE]
> Kérés számos olyan elemeket, például egy URL-címe és az elérési út, HTTP-műveletet, `api-version`, és így tovább. Kivonatosan mutatja hello példák toohighlight csak hello szintaxis, amely megfelelő toopagination rövidített azt. Tekintse meg a hello [Azure Search szolgáltatás REST API](http://msdn.microsoft.com/library/azure/dn798935.aspx) dokumentációjában szintaxis található.
> 
> 

## <a name="total-hits-and-page-counts"></a>Találatok és a lap száma
Alapvető toovirtually megjelenítő hello összes, egy lekérdezés által visszaadott eredmények száma, és azokat, majd vissza kisebb csoportjai eredményezi, az összes keresési lapok.

![][1]

Az Azure Search hello használata `$count`, `$top`, és `$skip` paraméterek tooreturn ezeket az értékeket. hello alábbi példa azt mutatja, adja vissza a találatok összesen mintát kér `@OData.count`:

        GET /indexes/onlineCatalog/docs?$count=true

15 csoportokban dokumentumok beolvasása, és hello találatok, hello első lapon kezdődő is megjelenítése:

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

Eredmények paginating szükséges az `$top` és `$skip`, ahol `$top` Megadja, hány elemek tooreturn kötegekben, és `$skip` Megadja, hogy hány elemek tooskip. A hello a következő példában minden egyes lapon látható hello mellett 15 elemeket jelzi hello növekményes ezzel a művelettel a hello `$skip` paraméter.

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=15&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=30&$count=true

## <a name="layout"></a>Elrendezés
A keresési eredmények oldalát érdemes lehet tooshow miniatűr képére, mezők és a hivatkozás tooa teljes termék oldalát.

 ![][2]

Használja az Azure Search `$select` és a keresési parancs tooimplement a felhasználói élmény.

egy mozaik elrendezés mezők tooreturn:

        GET /indexes/ onlineCatalog/docs?search=*&$select=productName,imageFile,description,price,rating 

Képek és médiafájlok tárolására nincsenek közvetlenül kereshető, és egy másik tárolási platform, például az Azure Blob Storage tárolóban, tooreduce költségek kell tárolni. A hello index és a dokumentumok határozza meg a külső tartalom hello hello URL-címet tárol. Hello mező egy Képhivatkozás módon használhatja. hello URL-cím toohello lemezkép hello dokumentumban lehet.

Leírás lap tooretrieve egy **onClick** esemény, használjon [keresési dokumentum](http://msdn.microsoft.com/library/azure/dn798929.aspx) toopass hello dokumentum tooretrieve hello kulcsában. hello hello kulcs adattípusa `Edm.String`. Az ebben a példában is *246810*. 

        GET /indexes/onlineCatalog/docs/246810

## <a name="sort-by-relevance-rating-or-price"></a>Rendezési szempont relevanciájának, minősítés vagy ár
Sorrendjét gyakran toorelevance alapértelmezett, de közös toomake alternatív a rendezési sorrend azonnal elérhetők, hogy az ügyfelek is gyorsan átütemezésével a meglévő eredményeit azokat egy másik sorrend.

 ![][3]

Az Azure Search rendezés alapuló hello `$orderby` kifejezés indexeli, mezők`"Sortable": true.`

Relevanciájának erősen kapcsolódik a pontozási profil. Használhat hello alapértelmezett pontozási, amely támaszkodik szöveg elemzése és statisztikái toorank sorrendje minden eredmény elérése érdekében a további vagy erősebb megegyezik a toodocuments továbblép a kívánt keresőkifejezést a magasabb pontszámok.

Alternatív a rendezési sorrend társított általában **onClick** eseményeket, amelyek visszahívási tooa metódus, amely buildek hello rendezési sorrend. Például adja meg a page eleme:

 ![][4]

Fogad el bemenetként kijelölt hello rendezési beállítás, és ez a lehetőség társított hello feltétel rendezett listák metódus hozna létre.

 ![][5]

> [!NOTE]
> Hello alapértelmezett pontozási nem elegendő a számos forgatókönyv, de javasolt relevanciájának alapozva egyéni pontozási profil helyette. Egy egyéni relevanciaprofil lehetővé teszi egy tooboost elemek, amelyek több előnyös tooyour üzleti. Lásd: [a relevanciaprofil felvétele](http://msdn.microsoft.com/library/azure/dn798928.aspx) további információt. 
> 
> 

## <a name="faceted-navigation"></a>Jellemzőalapú navigáció
Keresési navigációs esetében gyakori, a találatokat tartalmazó lap, gyakran hello oldalán vagy a lap tetején található meg. Az Azure Search jellemzőalapú navigációs biztosít irányuló keresési előre definiált szűrők alapján. Lásd: [az Azure Search Jellemzőalapú navigációs](search-faceted-navigation.md) részleteiről.

## <a name="filters-at-hello-page-level"></a>Szűrők hello lap szinten
Ha a megoldás kialakításának részét dedikált keresési lapok az adott típusú tartalmakat (például egy online kereskedelmi alkalmazás részlegek hello tetején hello lapján felsorolt), egy kifejezést mellett is beszúrhat egy **onClick** esemény tooopen egy lap előszűrt állapotban. 

Egy szűrőt, vagy egy keresési kifejezés nélkül küldhet. Például hello következő kérelem szűrést márka neve csak a megfelelő azt dokumentumok ad vissza.

        GET /indexes/onlineCatalog/docs?$filter=brandname eq ‘Microsoft’ and category eq ‘Games’

Lásd: [dokumentumok keresése (Azure Search API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) kapcsolatos további információk `$filter` kifejezések.

## <a name="see-also"></a>Lásd még:
* [Az Azure Search szolgáltatás REST API](http://msdn.microsoft.com/library/azure/dn798935.aspx)
* [Indexművelet](http://msdn.microsoft.com/library/azure/dn798918.aspx)
* [A dokumentum műveletek](http://msdn.microsoft.com/library/azure/dn800962.aspx)
* [Videók és oktatóanyagok Azure Search kapcsolatos](search-video-demo-tutorial-list.md)
* [Az Azure Search jellemzőalapú navigáció](search-faceted-navigation.md)

<!--Image references-->
[1]: ./media/search-pagination-page-layout/Pages-1-Viewing1ofNResults.PNG
[2]: ./media/search-pagination-page-layout/Pages-2-Tiled.PNG
[3]: ./media/search-pagination-page-layout/Pages-3-SortBy.png
[4]: ./media/search-pagination-page-layout/Pages-4-SortbyRelevance.png
[5]: ./media/search-pagination-page-layout/Pages-5-BuildSort.png 
