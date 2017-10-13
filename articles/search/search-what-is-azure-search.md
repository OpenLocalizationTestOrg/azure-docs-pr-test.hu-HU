---
title: Mi az Azure Search |} Microsoft Docs
description: "Az Azure Search egy teljes körűen felügyelt, üzemeltetett felhőalapú keresőszolgáltatás. További információ: a szolgáltatás áttekintését."
services: search
manager: jhubbard
author: ashmaka
documentationcenter: 
ms.assetid: 50bed849-b716-4cc9-bbbc-b5b34e2c6153
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 06/26/2017
ms.author: ashmaka
ms.openlocfilehash: baf73639eb6506b14d0d3a4de1bf55b66e973b95
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="what-is-azure-search"></a>Mi az az Azure Search?
Az Azure Search egy keresési,--szolgáltatás felhőalapú megoldás, amely lehetőséget nyújt a fejlesztők API-k, valamint eszközei hozzáadása egy hatékony keresési élményt biztosít az adatok a webes, mobil és vállalati alkalmazások.

Funkció van közzétéve, az egyszerű [REST API](/rest/api/searchservice/) vagy [.NET SDK](search-howto-dotnet-sdk.md) , amely elfedi keresési technológia rejlő összetettsége. API-k az Azure-portálon lehetőség felügyeleti vagy prototípusának támogatása. Infrastruktúra- és rendelkezésre állás a Microsoft által felügyelt.

<a name="feature-drilldown"></a>

## <a name="feature-summary"></a>Szolgáltatás összegzése

| Kategória | Szolgáltatások |
|----------|----------|
|Teljes szöveges keresés és a szöveg elemzése | [**Teljes szöveges keresés** ](search-lucene-query-architecture.md) egy elsődleges felhasználási eseténél legtöbb keresés-alapú alkalmazásokhoz. Lekérdezések kell kidolgozni támogatott szintaxis használatával: <br/><br/>[**Egyszerű lekérdezés szintaxisát**](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search), amely logikai operátorok, a kifejezés keresési operátorokat, a utótag operátorok, a sorrend operátorok kínál.<br/><br/>[**Lucene lekérdezés szintaxisát** ](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) kínál az összes egyszerű lekérdezés támogatása, valamint az intelligens egyeztetésű keresési, közelségi kapcsolat keresési kifejezés kiemelési és reguláris kifejezések.| 
| Adatintegráció | Az Azure Search-indexek forrásból, adatokat fogad, feltéve, egy JSON-adatszerkezet elküldve. <br/><br/> Szükség esetén a támogatott adatforrások az Azure-ban, használhat [ **indexelők** ](search-indexer-overview.md) automatikusan bejárható [Azure SQL Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md), [Azure Cosmos DB](search-howto-index-documentdb.md), vagy [Azure Blob Storage tárolóban](search-howto-indexing-azure-blob-storage.md) szinkronizálása az search-index által az elsődleges adattároló tartalom. Az Azure Blob indexelők hajthat végre *dokumentum repedés* a [fő fájlformátumok indexelő](search-howto-indexing-azure-blob-storage.md), például HTML, Microsoft Office és PDF-dokumentumokat. |
| Keresési elemzés | [**Egyéni lexikális elemzőkkel** ](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) fonetikus megfeleltetéssel összetett keresési lekérdezések és reguláris kifejezések állnak rendelkezésre. |
| Nyelvi támogatás | [**Nyelvi elemzőkkel** ](https://docs.microsoft.com/rest/api/searchservice/language-support) a Lucene, továbbá a Microsoft természetes nyelvű processzorok intelligens módon kezelje a nyelvspecifikus linguistics 56 különböző nyelveken érhető el például az művelet tenses, nemét, szabálytalan többes számú parancsokat (például "egér" és "egerek"), word deszerializálni összetételéhez, szótördelési (nyelvekhez, szóközök nélkül). |
| Földrajzi-keresés | Az Azure Search intelligens módon dolgozza fel a szűrőket, és földrajzi helyek jeleníti meg. Segítségével a felhasználók adatokba a keresési eredmény közelségbe kell kerülni a fizikai hely alapján. [A videó](https://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data) vagy [tekintse át ezt a mintát](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) további. |
| Felhasználói élményt nyújtó szolgáltatásokat | [**Keresési javaslatok** ](https://docs.microsoft.com/rest/api/searchservice/suggesters) található egy keresősáv begépelt lekérdezések engedélyezhető. Az indexben tényleges dokumentumok felhasználók meg részleges kereséshez bemeneti használata javasolt. <br/><br/>[**Jellemzőalapú navigációs** ](https://docs.microsoft.com/azure/search/search-faceted-navigation) keresztül egy lekérdezési paraméter engedélyezve van. Az Azure Search egy jellemzőalapú navigációs szerkezetben is használhatja a kategóriák listában mögötti kódban irányuló szűréséhez (például a szűrő katalóguselemek ár-tartomány vagy a márka) adja vissza. <br/><br/> [**Szűrők** ](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) segítségével jellemzőalapú navigációs beépítse az alkalmazás felhasználói felület javítása érdekében a lekérdezés létrehozását és szűrési felhasználói vagy fejlesztői megadott feltételek alapján. A szűrőkkel az OData-szintaxis használatával.<br/><br/> [**Kattintson a kijelölés** ](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) visual űrlap atting vonatkozik egyező kulcsszó a találatok között. Kiválaszthatja, hogy melyik mezőkre visszaadása a kijelölt kódtöredékek.<br/><br/>[**Rendezés** ](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) érhető el a több mező a sémát indexeli keresztül, és majd váltható időben-keresés egyetlen paraméterrel.<br/><br/> [**Lapozás** ](search-pagination-page-layout.md) , és a keresési eredmények szabályozás egyszerű Azure Search nyújt, mint a keresési eredmények finom bennünket vezérlővel.  
| Találati pontosság | [**Egyszerű pontozási** ](/rest/api/searchservice/add-scoring-profiles-to-a-search-index) Azure Search egyik legfontosabb előnye van. A pontozási profil segítségével modell relevancia magukat a dokumentumok értékeinek függvényében. Például előfordulhat, hogy szeretné, hogy újabb termékek vagy kedvezményes termékek magasabb a keresési eredmények között megjelenik. Hozza létre a személyre szabott pontozó ügyfél keresési beállítások is nyomon követheti és tárolt címkék használatával pontozási profilokat is. |
| Figyelés és jelentéskészítés | [**Keresési forgalom analytics** ](search-traffic-analytics.md) gyűjtött és elemzése információkat kaphat a mi felhasználók vannak írja be a keresőmezőbe feloldásához. <br/><br/>Egy második, a késés és a sávszélesség-szabályozás a lekérdezések metrikákat, és hogy a jelentett, a portál lapjai nincs szükség további konfigurációra. Is könnyen figyelő index, és a dokumentumok száma, hogy a kapacitás szükség szerint módosíthatja. További információkért lásd: [felügyeleti szolgáltatás](search-manage.md) |
| Eszközök prototípusának és ellenőrzés | A portálon, használhatja a [ **adatok importálása varázsló** ](search-import-data-portal.md) indexelők, index designer passzív másolatot index, konfigurálása és [ **keresési ablak** ](search-explorer.md) lekérdezések tesztelése és pontosítsa a pontozási profil. A séma megtekintéséhez index is megnyithatja. |
| Infrastruktúra | A **magas rendelkezésre állású platform** service rendkívül megbízható keresési élményt biztosítja. Ha megfelelően méretezhető [Azure Search 99,9 %-os SLA-t kínál](https://azure.microsoft.com/support/legal/sla/search/v1_0/).<br/><br/> **Teljes felügyelt, méretezhető** megoldásként végpont Azure Search feltétlenül nincs infrastruktúra kezelése szükséges. A szolgáltatás további dokumentumtároló, magasabb lekérdezésekkel vagy mindkettő kezeli a két dimenziókat a skálázással az igényeinek megfelelően testre szabható.

## <a name="how-it-works"></a>Működés
### <a name="step-1-provision-service"></a>1. lépés: Kiépítése szolgáltatáshoz
Akkor is lépett fel az Azure Search szolgáltatás a [Azure-portálon](https://portal.azure.com/) vagy a [Azure erőforrás-kezelési API](/rest/api/searchmanagement/). Dönthet úgy, vagy az ingyenes szolgáltatás más előfizetők megosztott vagy egy [réteg fizetett](https://azure.microsoft.com/pricing/details/search/) , amely csak a szolgáltatás által használt erőforrások dedicates. A fizetett szinteken a két dimenzió egy szolgáltatás is skálázható: 

- Adja hozzá a replikák nehéz lekérdezésekkel a kapacitás növelése sikertelen volt.   
- Adja hozzá a partíciók több dokumentumokat tároló növelése sikertelen volt. 

Külön-külön kezelnek dokumentum tárolás és a lekérdezési teljesítményt, továbbá bármikor kalibrálhatja finanszírozása éles követelmények alapján.

### <a name="step-2-create-index"></a>2. lépés: Az index létrehozása
Mielőtt feltöltheti a kereshető tartalom, először meg kell határoznia az Azure Search-index. Az index olyan, mint egy adatbázis tábláinak, amely tartalmazza az adatokat és a keresési lekérdezések fogad el. Megadhatja a sémát indexeli a dokumentumok kívánja a keresést, az adatbázis mezőit hasonló struktúrájának megfelelően leképezni.

A séma létrehozott Azure-portálon vagy programozottan használatával lehet a [.NET SDK](search-howto-dotnet-sdk.md) vagy [REST API](/rest/api/searchservice/).

### <a name="step-3-index-data"></a>3. lépés: Az Index adatok
Index definiálása után készen áll a tartalom feltöltése. A lekérés és küldés modell is használhatja.

A lekéréses modell külső forrásból származó lekéri az adatokat. A támogatott *indexelők* , amely egyszerűsítésére és automatizálhatja az adatfeldolgozást, például a kapcsolódás, az olvasást és adatok szerializálása aspektusait. [Indexelő](/rest/api/searchservice/Indexer-operations) elérhető Azure Cosmos DB, az Azure SQL Database, Azure Blob Storage és egy Azure virtuális gép futó SQL-kiszolgálót. Az indexelő igény szerint vagy ütemezett Adatfrissítés konfigurálhatja.

A leküldési modelljével az SDK vagy a REST API-k, indexek frissített dokumentumok küldésére szolgáló keresztül valósul meg. A JSON formátum használatával szinte bármilyen adatkészletből adatok tolható ki. Lásd: [hozzáadása, update vagy delete dokumentumok](/rest/api/searchservice/addupdate-or-delete-documents) vagy [a .NET SDK használatával)](search-howto-dotnet-sdk.md) útmutatót az adatok betöltéséhez.

### <a name="step-4-search"></a>4. lépés: keresés
Index feltöltése, után is [keresési lekérdezések ki](/rest/api/searchservice/Search-Documents) való a REST API-t vagy a .NET SDK egyszerű HTTP-kérelmek használatával.

## <a name="how-it-compares"></a>Összehasonlítja azt

Gyakran kérje meg az ügyfelek hogyan [teljes szöveges keresés az Azure Search](search-lucene-query-architecture.md) összehasonlítja [teljes szöveges keresés](https://en.wikipedia.org/wiki/Full_text_search) az adatbázis termékben. A rendszer a választ használt Azure Search nyelvi képességek gazdagabb és Lucene lekérdezések, Lucene és a Microsoft a nyelvi elemzőkkel, egyéni lekérdezések fonetikus vagy más speciális bemenetek és a keresési index több forrásból származó adatokat képes támogatása rugalmasabb-e. 

Egy másik nincsenek pont, hogy a keresési megoldás szünteti meg a teljes keresési élményt biztosít. Például érdemes egyéni pontozási eredményeinek, jellemzőalapú navigációs irányuló szűrést, a találatok kiemelése és typeahead konstrukciókat. 

Eszközök figyelése és megismerése lekérdezés tevékenység számára is be egy keresési megoldás döntési is figyelembe. Például támogatja az Azure Search [keresési forgalom analytics](search-traffic-analytics.md) metrikáihoz a átkattintós arány, felső keres keresés nélkül kattint, és így tovább. A termékek, ahol a keresési az bővítménye Ön nem valószínű, hogy ezek a funkciók található.

Erőforrás-használat egy másik szempont. Természetes nyelvű keresési legtöbbször számításilag intenzív. A felhasználók némelyike kiszervezett Azure Search keresési műveletek egyik módja a számítógép erőforrásai tranzakció feldolgozásra megőrzése érdekében. Externalizing keresés könnyen módosíthatja méretezési lekérdezés kötet kereséséhez.

Ha úgy döntött, dedikált keresési-nal, a következő döntés, szükség van egy felhőalapú szolgáltatás, vagy egy helyszíni kiszolgáló között. Egy felhőalapú szolgáltatás használata a megfelelő választás, ha azt szeretné egy [kulcsrakész megoldást minimális terhelés és karbantartási és állítható méretezési](#cloud-service-advantage).

Belül a felhő összeállítást a több szolgáltatók összehasonlíthatónak alapvető szolgáltatásokat, a teljes szöveges keresés, földrajzi-keresés és a keresési bemenet a félreérthetőség bizonyos szintű kezelésének képessége. Általában rendelkezik egy [speciális funkció](#feature-drilldown), vagy a könnyű, és a teljes API-k, eszközök és felügyeletéhez, amely meghatározza, hogy a legjobb térkihasználás érdekében egyszerűsége.

Felhő szolgáltatóknál Azure Search teljes szöveges keresés munkaterhelések esetében felett tartalom tárolók és a Azure-adatbázisok elsősorban a keresést a adatok beolvasása és a tartalom navigációs használó alkalmazások esetében. Kulcs szintjeiről a következők:

+ Az indexelő rétegben Azure Adatintegráció (webbejárók)
+ Azure-portál a központi felügyeleti
+ Az Azure méretezés, a megbízhatóság és a világszínvonalú rendelkezésre állása
+ A nyelvi és egyéni elemzést követően elemzőkkel 56 nyelvű teli teljes szöveges kereséshez
+ [Alapvető szolgáltatásokat vonatkozó keresési-központú alkalmazások](#feature-drilldown): pontozási, értékkorlátozás, vonatkozó javaslatokat, szinonimák, földrajzi-keresés, és több.

> [!Note]
> Ahhoz, hogy törölje a jelet,-Azure adatforrások teljes mértékben támogatottak, de indexelők, hanem több kód igényel ügyfélleküldéses módszer támaszkodnak. Az API-k használatával, átadható az Azure Search-index bármely JSON dokumentumgyűjteményt.

A felhasználók között azokat az Azure Search szolgáltatások legszélesebb kihasználhatják közé tartozik a online katalógusok, üzleti programok és dokumentum felderítési alkalmazások.

## <a name="rest-api--net-sdk"></a>REST API-t |} .net SDK

Számos feladatot a portálon hajtható végre, amíg Azure Search keresési funkcióját integrálja a meglévő alkalmazások fejlesztőknek szól. A következő alkalmazásprogramozási felületek érhetők el.

|Platform |Leírás |
|-----|------------|
|[REST](/rest/api/searchservice/) | Bármely programozási platform és nyelvi, többek között a Xamarin, Java és JavaScript által támogatott HTTP-parancsok|
|[.NET SDK](search-howto-dotnet-sdk.md) | .NET-burkoló a REST API kínál hatékony kódolása a C# és a .NET-keretrendszer célzó felügyeltkód-nyelvek |

## <a name="free-trial"></a>Ingyenes próbalehetőség
Az Azure-előfizetők is [egy szolgáltatás ingyenes szint kiépítése](search-create-service-portal.md).

Ha az előfizető nem, akkor [szabad nyissa meg az Azure-fiók](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F). Jóváírásokat kap a próbálhatja ki a fizetős Azure-szolgáltatásokat. Azok lejárta után, megtarthatja a fiókot, és használjon [ingyenes Azure-szolgáltatásokat](https://azure.microsoft.com/free/). A hitelkártya soha nem feladata, kivéve, ha explicit módon a beállítások módosításához és engedélyezéséhez.

Alternatív megoldásként, [aktiválhatja MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): az MSDN-előfizetés biztosít Önnek krediteket minden hónapban, fizetős Azure-szolgáltatásokat is használhat. 

## <a name="how-to-get-started"></a>Első lépések

1. A szolgáltatás létrehozása a [ingyenes szint](search-create-service-portal.md).

2. Legalább egy, az alábbi oktatóanyagok lépéseit. 

  + [A .NET SDK használatával](search-howto-dotnet-sdk.md) a felügyelt kódban a fő lépéseit mutatja be.  
  + [Ismerkedés a REST API-t a](https://github.com/Azure-Samples/search-rest-api-getting-started) ugyanazokat a lépéseket a REST API használatával jeleníti meg.  
  + [Az első index létrehozása a portálon](search-get-started-portal.md) a portál beépített indexelő és prototípus szolgáltatásokat mutatja be.   

A keresőmotorok adatok beolvasása a mobilalkalmazásokban, a weben, és a vállalati adatokat tárolja, a közös illesztőprogramok legyenek. Az Azure Search lehetővé teszi az eszközök egy keresési élményt biztosít a nagy kereskedelmi webhelyeken hasonló létrehozásához.

Ez a program manager Liam Cavanagh 9 perces videót megtudhatja, hogyan integrálható a keresőprogramok találatait is kihasználhatja az alkalmazás. Rövid bemutatók tér ki az Azure Search és egy tipikus munkafolyamat néz funkciói. 

>[!VIDEO https://channel9.msdn.com/Events/Connect/2016/138/player]
 
+ 0 – 3 perc magában foglalja az legfontosabb funkcióira és a használati eseteket.
+ 3 – 4 perc foglalkozik kiépítése szolgáltatáshoz. 
+ 4-6 percig adatok importálása varázsló használatával, a beépített ingatlan adatkészlet létrehozásához használt ismerteti.
+ 6 – 9 perc keresési ablak és különböző lekérdezések tartalmazza.


