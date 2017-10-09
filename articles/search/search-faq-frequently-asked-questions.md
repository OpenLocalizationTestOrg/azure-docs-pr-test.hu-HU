---
title: "kérdések (GYIK) kapcsolatos Azure Search aaaFrequently |} Microsoft Docs"
description: "A Microsoft Azure Search szolgáltatás toocommon kérdésekre adott válaszok"
services: search
author: HeidiSteen
manager: jhubbard
ms.service: search
ms.technology: search
ms.topic: article
ms.date: 08/03/2017
ms.author: heidist
ms.openlocfilehash: 2c573750600d80585b746bfce20d6c0f8df5a262
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-search---frequently-asked-questions-faq"></a>Az Azure Search - gyakran ismételt kérdések (GYIK)
 
 Megállapítja a válaszok toocommonly kérdések a fogalmakat, kapcsolódó keresési tooAzure kódot, és forgatókönyvek.

## <a name="platform"></a>Platform

### <a name="how-is-azure-search-different-from-full-text-search-in-my-dbms"></a>Miben különbözik Azure Search teljes szöveges keresés a célrendszerben?

Az Azure Search támogatja több adatforrást, [nyelvi elemzés különböző nyelvű](https://docs.microsoft.com/rest/api/searchservice/language-support), [érdekes és szokatlan adatok bemenetek egyéni elemzési](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search), keressen a sorrendet megadó vezérlők [pontozási profilok](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index), és a felhasználói élmény szolgáltatásait, például typeahead, a találatok kiemelése és a jellemzőalapú navigáció. Egyéb szolgáltatásokkal, például a szinonimák és gazdag lekérdezési szintaxis is tartalmaz, de azokat, amelyek általában nem lehetővé tevő szolgáltatások.

### <a name="what-is-hello-difference-between-azure-search-and-elasticsearch"></a>Mi az Azure Search és Elasticsearch hello különbségének?

Keresési technológiák összehasonlításakor ügyfelek gyakran kérje meg a részletekért hogyan összehasonlítja az Azure Search Elasticsearch. Felhasználók Azure Search Elasticsearch keresztül a kereséshez alkalmazások általában megtenni, mert a legfontosabb feladatok könnyebb hajtottunk vagy hello beépített integráció más Microsoft-technológiák a van szükségük:

+ Az Azure Search egy teljes körűen felügyelt felhőszolgáltatás 99,9 %-os garantált szolgáltatási szintek (SLA) esetén (olvasási hozzáféréssel 2-replikával, írható-olvasható 3 replikák) megfelelő redundanciával kiépítve.
+ A Microsoft [természetes nyelvű processzorok](https://docs.microsoft.com/rest/api/searchservice/language-support) élvonalbeli inguistic elemzés kínálnak.  
+ [Az Azure keresési indexelő](search-indexer-overview.md) által bejárható különböző Azure-adatforrással kezdeti és a növekményes indexeléshez.
+ Ha a lekérdezés vagy kötetek indexelő gyors válasz toofluctuations van szüksége, használhatja [csúszkával](search-manage.md#scale-up-or-down) hello Azure portál, vagy futtassa a egy [PowerShell-parancsfájl](search-manage-powershell.md), shard felügyeleti megkerülésével közvetlenül.  
+ [Pontozási és hangolási szolgáltatások](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) hello azt jelenti, hogy keresési sorrendet megadó pontszámok túl milyen hello keresőmotor önmagában biztosíthat befolyásoló biztosítanak. 

### <a name="can-i-pause-azure-search-service-and-stop-billing"></a>Azure Search szolgáltatás szüneteltetése és számlázási leállítása?

Hello szolgáltatást nem függeszthető fel. Számítási és tárolási erőforrásokat a kizárólagos használatra hello szolgáltatás létrehozásakor. Ez nem lehetséges toorelease és VISSZAIGÉNYLÉSE ezen erőforrások igény. 

## <a name="indexing-operations"></a>Indexelési műveletek

### <a name="backup-and-restore-or-download-and-move-indexes-or-index-snapshots"></a>Biztonsági mentése és visszaállítása (vagy töltse le és áthelyezése) indexek vagy index pillanatképeket?

Bár [beolvasása az index definícióját](https://docs.microsoft.com/rest/api/searchservice/get-index) bármikor nincs index kinyerési, pillanatképet, vagy biztonsági mentési-visszaállítási funkció le egy *feltöltve* index rendszerben futó hello felhő tooa helyi, vagy Azure Search szolgáltatás tooanother helyezze át. 

Indexek beépített és tölti be a kódot, amely írási, és csak az Azure Search hello felhőben futtatni. Általában a felhasználóknak, akik toomove index tooanother szolgáltatásnak ehhez a kód toouse új végpont szerkesztésével, és futtassa a indexelő. Ha szeretné, hello képességét tootake pillanatképet, vagy biztonsági mentését egy index, leadott szavazattal [User Voice](https://feedback.azure.com/forums/263029-azure-search/suggestions/8021610-backup-snapshot-of-index).

### <a name="can-i-index-from-sql-database-replicas-applies-tooazure-sql-database-indexershttpsdocsmicrosoftcomazuresearchsearch-howto-connecting-azure-sql-database-to-azure-search-using-indexers"></a>Képes vagyok felületindexét SQL adatbázis-replikák (túl vonatkozik[Azure SQL Database indexelők](https://docs.microsoft.com/azure/search/search-howto-connecting-azure-sql-database-to-azure-search-using-indexers))

 Nincsenek korlátozások hello használja az elsődleges vagy másodlagos replikák adatforrásként teljesen új index létrehozásakor. Az index frissítése (a megváltozott rekordokat alapján) a növekményes frissítések azonban csak hello elsődleges replika. Ez a követelmény SQL-adatbázis, mely garanciák csak elsődleges replikára változott a változáskövetés származik. Ha másodlagos replikákat használ egy index frissítési munkaterhelés, nincs kap hello adatok garancia.

## <a name="search-operations"></a>Keresési műveletek

### <a name="can-i-search-across-multiple-indexes"></a>Is kereshető több index között?

Nem, ez a művelet nem támogatott. A művelet mindig egyetlen hatókörön belüli tooa index.

### <a name="can-i-restrict-search-corpus-access-by-user-identity"></a>Is keresési corpus hozzáférés is korlátozása felhasználó identitása szerint?

Az Azure Search nem rendelkezik felhasználói adat-hozzáférési szerepkör alapú biztonsági modellt. A hitelesítés az vagy teljes körű jogosultságokkal vagy hello szolgáltatás szinten csak olvasható. Egyes ügyfelek alapján megoldások megvalósítását [ `$filter` keresési feltétel](https://docs.microsoft.com/rest/api/searchservice/search-documents), de áthidaló legjobb. Ha azt szeretné, hogy ez a funkció, leadott szavazattal [User Voice](https://feedback.azure.com/forums/263029-azure-search/category/86074-security).

### <a name="why-are-there-zero-matches-on-terms-i-know-toobe-valid"></a>Miért van nulla megegyezik tudható, hogy érvényes toobe igényei szerint?

hello leggyakoribb eset nem fogják, hogy minden egyes lekérdezés típusa különböző keresési viselkedések és nyelvi elemzések szintű támogatja-e. Teljes szöveges keresés, ami hello meghatározó munkaterhelés, a nyelvi elemzési fázis tooroot űrlapok vonatkozó feltételek megszüntető tartalmazza. Elemzése a lekérdezés ezen része a szélesebb körű nettó keresztül lehetséges találatok kerül, mert hello tokenekre kifejezés számának egyeznie kell a nagyobb Variant adattípusban.

Ha indításakor [más lekérdezéstípusok](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) (helyettesítő keresési, intelligens egyeztetésű keresési, közelségi kapcsolat keresési, javaslatok, többek között), nincs nyelvi elemzés van. Feltételek kerülnek toohello lekérdezés fa,-van. 

### <a name="why-is-hello-search-rank-a-constant-or-equal-score-of-10-for-every-hit"></a>Miért van hello keresési besorolás 1.0 minden nyomja le az állandó vagy egyenlő pontszámot?

Alapértelmezés szerint keresési eredmények kell kiértékelni alapján a hello [statisztikai tulajdonságainak megfelelő feltételek](search-lucene-query-architecture.md#stage-4-scoring), és magas toolow rendezett hello eredményhalmazban. Azonban néhány lekérdezés (helyettesítő, előtag, regex) típusú mindig hozzájárul az állandó pontszám toohello összesített pontszám dokumentum. Ez a működésmód szándékos. Az Azure Search ró állandó pontozása tooallow találat keresztül lekérdezés bővítése toobe hello eredmények szereplő hello rangsorolási befolyásolása nélkül. 

Tegyük fel, hogy a "bemutató *" helyettesítő karakteres keresés bemeneti megfelel a "bemutatók", "tourettes" és "tourmaline" hoz létre. Hello jellegéből ezekkel az eredményekkel, nincs mód tooreasonably következtethető ki, mely feltételek értékesebb, mint a többire. Ezért azt figyelmen kívül hagyása kifejezés gyakoriságot amikor pontozási eredményei típusok helyettesítő, előtag és regex lekérdezésekben. Egy részleges bemeneti keresési eredmény kap egy állandó pontozása tooavoid eltérés felé potenciálisan váratlan megegyezik.

## <a name="design-patterns"></a>Tervezési minták

### <a name="what-is-hello-best-approach-for-implementing-localized-search"></a>Mi az a legjobb módszer hello honosított keresés végrehajtásához?

A legtöbb ügyfél a gyűjtemény dedikált mezők kiválasztása a toosupporting különböző területi beállításokhoz (nyelv) a hello ismét ugyanazt az indexet. Területibeállítás-specifikus mezők lehetséges tooassign egy megfelelő analyzer révén. Például hozzárendelése hello Microsoft francia Analyzer tooa mező francia karakterláncokat tartalmazó. Emellett leegyszerűsíti az szűrést. Ha tudja, hogy egy lekérdezést lehet kezdeményezni fr-fr oldalon, korlátozhatja a keresési eredmények toothis mező. Vagy hozzon létre egy [pontozási profil](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) toogive hello további relatív súly mezőben.

## <a name="next-steps"></a>Következő lépések

Egy hiányzó szolgáltatást és funkciót a kérdése van? Hello hello szolgáltatást kérelem [User Voice webhely](https://feedback.azure.com/forums/263029-azure-search).

## <a name="see-also"></a>Lásd még:

 [StackOverflow: Az Azure Search](https://stackoverflow.com/questions/tagged/azure-search)   
 [Hogyan teljes szöveges keresés az Azure Search működik](search-lucene-query-architecture.md)  
 [Mi az az Azure Search?](search-what-is-azure-search.md)

 