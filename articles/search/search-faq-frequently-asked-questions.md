---
title: "Azure Search kapcsolatos gyakori kérdések (GYIK) |} Microsoft Docs"
description: "A Microsoft Azure Search szolgáltatással kapcsolatos gyakori kérdésekre adott válaszok"
services: search
author: HeidiSteen
manager: jhubbard
ms.service: search
ms.technology: search
ms.topic: article
ms.date: 08/03/2017
ms.author: heidist
ms.openlocfilehash: 02d5fac8cf9067ec544668f306fe49b805b3d164
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-search---frequently-asked-questions-faq"></a>Az Azure Search - gyakran ismételt kérdések (GYIK)
 
 Fogalmakat, a kódot és az Azure Search kapcsolatos forgatókönyvek gyakran feltett kérdésekre adott válaszok.

## <a name="platform"></a>Platform

### <a name="how-is-azure-search-different-from-full-text-search-in-my-dbms"></a>Miben különbözik Azure Search teljes szöveges keresés a célrendszerben?

Az Azure Search támogatja több adatforrást, [nyelvi elemzés különböző nyelvű](https://docs.microsoft.com/rest/api/searchservice/language-support), [érdekes és szokatlan adatok bemenetek egyéni elemzési](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search), keressen a sorrendet megadó vezérlők [pontozási profilok](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index), és a felhasználói élmény szolgáltatásait, például typeahead, a találatok kiemelése és a jellemzőalapú navigáció. Egyéb szolgáltatásokkal, például a szinonimák és gazdag lekérdezési szintaxis is tartalmaz, de azokat, amelyek általában nem lehetővé tevő szolgáltatások.

### <a name="what-is-the-difference-between-azure-search-and-elasticsearch"></a>Mi az Azure Search és Elasticsearch közötti különbség?

Keresési technológiák összehasonlításakor ügyfelek gyakran kérje meg a részletekért hogyan összehasonlítja az Azure Search Elasticsearch. Felhasználók Azure Search Elasticsearch keresztül a kereséshez alkalmazások általában megtenni, mert a legfontosabb feladatok könnyebb hajtottunk vagy más Microsoft-technológiák beépített integrálva van szükségük:

+ Az Azure Search egy teljes körűen felügyelt felhőszolgáltatás 99,9 %-os garantált szolgáltatási szintek (SLA) esetén (olvasási hozzáféréssel 2-replikával, írható-olvasható 3 replikák) megfelelő redundanciával kiépítve.
+ A Microsoft [természetes nyelvű processzorok](https://docs.microsoft.com/rest/api/searchservice/language-support) élvonalbeli inguistic elemzés kínálnak.  
+ [Az Azure keresési indexelő](search-indexer-overview.md) által bejárható különböző Azure-adatforrással kezdeti és a növekményes indexeléshez.
+ Ha a lekérdezés vagy kötetek indexelő ingadozását gyors válasz van szüksége, használhatja [csúszkával](search-manage.md#scale-up-or-down) az Azure portál, vagy futtassa egy [PowerShell-parancsfájl](search-manage-powershell.md), shard felügyeleti megkerülésével közvetlenül.  
+ [Pontozási és hangolási szolgáltatások](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) adja meg az eszközöket befolyásoló keresési sorrendet megadó pontszámok túl önmagában keresőmotor biztosíthat. 

### <a name="can-i-pause-azure-search-service-and-stop-billing"></a>Azure Search szolgáltatás szüneteltetése és számlázási leállítása?

A szolgáltatás nem függeszthető fel. Számítási és tárolási erőforrásokat a kizárólagos használatra a szolgáltatás létrehozásakor. Nincs lehetőség felszabadítása és VISSZAIGÉNYLÉSE ezen erőforrások igény. 

## <a name="indexing-operations"></a>Indexelési műveletek

### <a name="backup-and-restore-or-download-and-move-indexes-or-index-snapshots"></a>Biztonsági mentése és visszaállítása (vagy töltse le és áthelyezése) indexek vagy index pillanatképeket?

Bár [beolvasása az index definícióját](https://docs.microsoft.com/rest/api/searchservice/get-index) bármikor nincs index kinyerési, pillanatképet, vagy biztonsági mentési-visszaállítási funkció le egy *feltöltve* egy helyi rendszerre, a felhőben futó index vagy Helyezze át egy másik Azure Search szolgáltatás. 

Indexek beépített és tölti be a kódot, amely írási, és csak a felhőben Azure Search futtassa. Általában a felhasználóknak, akik index áthelyezése egy másik szolgáltatás ehhez új végpont használandó kódjukat szerkesztésével, és futtassa a indexelő. Ha azt szeretné, hogy a pillanatkép vagy biztonsági mentését egy index, leadott szavazattal [User Voice](https://feedback.azure.com/forums/263029-azure-search/suggestions/8021610-backup-snapshot-of-index).

### <a name="can-i-index-from-sql-database-replicas-applies-to-azure-sql-database-indexershttpsdocsmicrosoftcomazuresearchsearch-howto-connecting-azure-sql-database-to-azure-search-using-indexers"></a>Képes vagyok felületindexét SQL adatbázis-replikák (vonatkozik [Azure SQL Database indexelők](https://docs.microsoft.com/azure/search/search-howto-connecting-azure-sql-database-to-azure-search-using-indexers))

 Nincsenek az elsődleges vagy másodlagos replikák használatára vonatkozó korlátozások adatforrásként teljesen új index létrehozásakor. Az index frissítése (a megváltozott rekordokat alapján) a növekményes frissítések azonban csak az elsődleges másodpéldány. Ez a követelmény SQL-adatbázis, mely garanciák csak elsődleges replikára változott a változáskövetés származik. Ha másodlagos replikákat használ egy index frissítési munkaterhelés, nincs kap az összes adat garancia.

## <a name="search-operations"></a>Keresési műveletek

### <a name="can-i-search-across-multiple-indexes"></a>Is kereshető több index között?

Nem, ez a művelet nem támogatott. Keresési mindig egyetlen index hatókörét.

### <a name="can-i-restrict-search-corpus-access-by-user-identity"></a>Is keresési corpus hozzáférés is korlátozása felhasználó identitása szerint?

Az Azure Search nem rendelkezik felhasználói adat-hozzáférési szerepkör alapú biztonsági modellt. A hitelesítés az vagy teljes körű jogosultságokkal vagy a szolgáltatás szinten csak olvasható. Egyes ügyfelek alapján megoldások megvalósítását [ `$filter` keresési feltétel](https://docs.microsoft.com/rest/api/searchservice/search-documents), de áthidaló legjobb. Ha azt szeretné, hogy ez a funkció, leadott szavazattal [User Voice](https://feedback.azure.com/forums/263029-azure-search/category/86074-security).

### <a name="why-are-there-zero-matches-on-terms-i-know-to-be-valid"></a>Miért van nulla megegyezik tudható, hogy érvényes igényei szerint?

A leggyakoribb eset nem fogják, hogy minden egyes lekérdezés típusa különböző keresési viselkedések és nyelvi elemzések szintű támogatja-e. Teljes szöveges keresés, ami az elsődleges munkaterhelés, a nyelvi elemzési fázis, a legfelső szintű űrlap le feltételeket megsérti tartalmazza. Ezen jellemzője lekérdezés elemzése a szélesebb körű nettó keresztül lehetséges találatok kerül, mert tokenekre kifejezés Variant típusú adatok nagyobb számának egyeznie kell.

Ha indításakor [más lekérdezéstípusok](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) (helyettesítő keresési, intelligens egyeztetésű keresési, közelségi kapcsolat keresési, javaslatok, többek között), nincs nyelvi elemzés van. Feltételek kerülnek, a lekérdezés fa-van. 

### <a name="why-is-the-search-rank-a-constant-or-equal-score-of-10-for-every-hit"></a>Miért van a keresési besorolás 1.0 minden nyomja le az állandó vagy egyenlő pontszámot?

Alapértelmezés szerint keresési eredmények alapján kell kiértékelni a [statisztikai tulajdonságainak megfelelő feltételek](search-lucene-query-architecture.md#stage-4-scoring), és rendezett magastól alacsonyig az eredményben. Azonban néhány lekérdezés (helyettesítő, előtag, regex) típusú mindig hozzájárulhatnak a a teljes dokumentum pontszám állandó pontszámot. Ez a működésmód szándékos. Az Azure Search szab találat lekérdezés bővítése keresztül anélkül, hogy befolyásolná a rangsorolási foglalandó az eredményeket, hogy állandó pontszámot. 

Tegyük fel, hogy a "bemutató *" helyettesítő karakteres keresés bemeneti megfelel a "bemutatók", "tourettes" és "tourmaline" hoz létre. Ezekkel az eredményekkel jellegéből, nincs mód ésszerűen következtetési forrásaként, mely feltételek értékesebb, mint a többire. Ezért azt figyelmen kívül hagyása kifejezés gyakoriságot amikor pontozási eredményei típusok helyettesítő, előtag és regex lekérdezésekben. Egy részleges bemeneti keresési eredmény felé potenciálisan váratlan egyezések időeltérés elkerülése érdekében állandó pontszámot kap.

## <a name="design-patterns"></a>Tervezési minták

### <a name="what-is-the-best-approach-for-implementing-localized-search"></a>Honosított keresés végrehajtásához a legjobb módszer mekkora?

A legtöbb ügyfél dedikált mezők kiválasztása a tartalmazó gyűjteményen ismét ugyanazt az indexet a különböző területi beállításokhoz (nyelv) támogatására. Területibeállítás-specifikus mezők hozzárendelése egy megfelelő analyzer lehetővé teszik. A Microsoft francia Analyzer például hozzárendelése egy francia karakterláncokat tartalmazó mezőt. Emellett leegyszerűsíti az szűrést. Ha tudja, hogy egy lekérdezést lehet kezdeményezni fr-fr oldalon, korlátozhatja a keresési eredmények ebbe a mezőbe. Vagy hozzon létre egy [pontozási profil](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) ahhoz, hogy megkapja a mező további relatív súly.

## <a name="next-steps"></a>Következő lépések

Egy hiányzó szolgáltatást és funkciót a kérdése van? A funkció a kérelem a [User Voice webhely](https://feedback.azure.com/forums/263029-azure-search).

## <a name="see-also"></a>Lásd még:

 [StackOverflow: Az Azure Search](https://stackoverflow.com/questions/tagged/azure-search)   
 [Hogyan teljes szöveges keresés az Azure Search működik](search-lucene-query-architecture.md)  
 [Mi az az Azure Search?](search-what-is-azure-search.md)

 