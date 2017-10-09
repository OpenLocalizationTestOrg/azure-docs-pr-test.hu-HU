---
title: aaaWhat az Azure Search |} Microsoft Docs
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
ms.openlocfilehash: b38a7e93a7f0e0d5f8a3779858032271b3883778
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-search"></a>Mi az az Azure Search?
Az Azure Search egy keresési,--szolgáltatás felhőalapú megoldás, amely lehetőséget nyújt a fejlesztők API-k, valamint eszközei hozzáadása egy hatékony keresési élményt biztosít az adatok a webes, mobil és vállalati alkalmazások.

Funkció van közzétéve, az egyszerű [REST API](/rest/api/searchservice/) vagy [.NET SDK](search-howto-dotnet-sdk.md) , amely elfedi hello rejlő összetettsége keresési technológia. Továbbá tooAPIs hello Azure-portál szolgáltatás felügyeleti és prototípusának támogatása. Infrastruktúra- és rendelkezésre állás a Microsoft által felügyelt.

<a name="feature-drilldown"></a>

## <a name="feature-summary"></a>Szolgáltatás összegzése

| Kategória | Szolgáltatások |
|----------|----------|
|Teljes szöveges keresés és a szöveg elemzése | [**Teljes szöveges keresés** ](search-lucene-query-architecture.md) egy elsődleges felhasználási eseténél legtöbb keresés-alapú alkalmazásokhoz. Lekérdezések kell kidolgozni támogatott szintaxis használatával: <br/><br/>[**Egyszerű lekérdezés szintaxisát**](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search), amely logikai operátorok, a kifejezés keresési operátorokat, a utótag operátorok, a sorrend operátorok kínál.<br/><br/>[**Lucene lekérdezés szintaxisát** ](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) kínál az összes egyszerű lekérdezés támogatása, valamint az intelligens egyeztetésű keresési, közelségi kapcsolat keresési kifejezés kiemelési és reguláris kifejezések.| 
| Adatintegráció | Az Azure Search-indexek forrásból, adatokat fogad, feltéve, egy JSON-adatszerkezet elküldve. <br/><br/> Szükség esetén a támogatott adatforrások az Azure-ban, használhat [ **indexelők** ](search-indexer-overview.md) tooautomatically bejárási [Azure SQL Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md), [Azure Cosmos DB](search-howto-index-documentdb.md), vagy [Azure Blob Storage tárolóban](search-howto-indexing-azure-blob-storage.md) toosync az search-index által az elsődleges adattároló tartalom. Az Azure Blob indexelők hajthat végre *dokumentum repedés* a [fő fájlformátumok indexelő](search-howto-indexing-azure-blob-storage.md), például HTML, Microsoft Office és PDF-dokumentumokat. |
| Keresési elemzés | [**Egyéni lexikális elemzőkkel** ](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) fonetikus megfeleltetéssel összetett keresési lekérdezések és reguláris kifejezések állnak rendelkezésre. |
| Nyelvi támogatás | [**Nyelvi elemzőkkel** ](https://docs.microsoft.com/rest/api/searchservice/language-support) Lucene, valamint a Microsoft által természetes nyelvű processzorok 56 különböző nyelveken toointelligently leíró nyelvspecifikus linguistics elérhető beleértve művelet tenses, nemét, szabálytalan többes szám parancsokat (például "egér" és "egerek"), word deszerializálni összetételéhez, szótördelési (nyelvekhez, szóközök nélkül), és több. |
| Földrajzi-keresés | Az Azure Search intelligens módon dolgozza fel a szűrőket, és földrajzi helyek jeleníti meg. Lehetővé teszi a felhasználók tooexplore adatok hello közelségi kapcsolat és a keresési eredmény tooa fizikai hely alapján. [A videó](https://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data) vagy [tekintse át ezt a mintát](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) további toolearn. |
| Felhasználói élményt nyújtó szolgáltatásokat | [**Keresési javaslatok** ](https://docs.microsoft.com/rest/api/searchservice/suggesters) található egy keresősáv begépelt lekérdezések engedélyezhető. Az indexben tényleges dokumentumok felhasználók meg részleges kereséshez bemeneti használata javasolt. <br/><br/>[**Jellemzőalapú navigációs** ](https://docs.microsoft.com/azure/search/search-faceted-navigation) keresztül egy lekérdezési paraméter engedélyezve van. Az Azure Search egy jellemzőalapú navigációs szerkezetben is használhatja a kategóriák listában hello háttérkódot irányuló szűréséhez (például toofilter katalóguselemek ár-tartomány vagy a márka) adja vissza. <br/><br/> [**Szűrők** ](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) lehet használt tooincorporate jellemzőalapú navigációs be az alkalmazás felhasználói felület javítása érdekében a lekérdezés létrehozását és szűrő felhasználói vagy fejlesztői megadott feltételek alapján. A szűrőkkel hello OData-szintaxis használatával.<br/><br/> [**Kattintson a kijelölés** ](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) visual űrlap atting tooa kulcsszó a találatok között megfelelő vonatkozik. Kiválaszthatja, hogy melyik mezőkre visszaadása a kijelölt kódtöredékek.<br/><br/>[**Rendezés** ](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) érhető el a több mező hello a sémát indexeli keresztül, és majd váltható időben-keresés egyetlen paraméterrel.<br/><br/> [**Lapozás** ](search-pagination-page-layout.md) , és a keresési eredmények szabályozás egyszerű Azure Search nyújt, mint a keresési eredmények hangolt hello-vezérléssel.  
| Találati pontosság | [**Egyszerű pontozási** ](/rest/api/searchservice/add-scoring-profiles-to-a-search-index) Azure Search egyik legfontosabb előnye van. A pontozási profil nem használt toomodel relevanciájának, mivel egy függvény hello szereplő értékek dokumentumok magukat. Például előfordulhat, hogy szeretné, hogy újabb termékek vagy kedvezményes termékek tooappear magasabb hello keresési eredmények között. Hozza létre a személyre szabott pontozó ügyfél keresési beállítások is nyomon követheti és tárolt címkék használatával pontozási profilokat is. |
| Figyelés és jelentéskészítés | [**Keresési forgalom analytics** ](search-traffic-analytics.md) gyűjtése és elemzése toounlock információkat kaphat a mi felhasználók vannak írja be a keresőmezőbe hello vannak. <br/><br/>Egy második, a késés és a sávszélesség-szabályozás a lekérdezések metrikákat, és hogy a jelentett, a portál lapjai nincs szükség további konfigurációra. Is könnyen figyelő index, és a dokumentumok száma, hogy a kapacitás szükség szerint módosíthatja. További információkért lásd: [felügyeleti szolgáltatás](search-manage.md) |
| Eszközök prototípusának és ellenőrzés | Hello portálon, használhatja a hello [ **adatok importálása varázsló** ](search-import-data-portal.md) tooconfigure indexelők index Tervező toostand fel indexet, és [ **keresési ablak** ](search-explorer.md) tootest kérdezi le, és pontosítsa a pontozási profil. Bármely index tooview sémájú is megnyithatja. |
| Infrastruktúra | Hello **magas rendelkezésre állású platform** service rendkívül megbízható keresési élményt biztosítja. Ha megfelelően méretezhető [Azure Search 99,9 %-os SLA-t kínál](https://azure.microsoft.com/support/legal/sla/search/v1_0/).<br/><br/> **Teljes felügyelt, méretezhető** megoldásként végpont Azure Search feltétlenül nincs infrastruktúra kezelése szükséges. A szolgáltatás lehet szabott tooyour igényeinek skálázással két dimenzió toohandle a dokumentum további tárhelyet, magasabb lekérdezésekkel, vagy mindkettőt.

## <a name="how-it-works"></a>Működés
### <a name="step-1-provision-service"></a>1. lépés: Kiépítése szolgáltatáshoz
Meg is lépett fel a hello Azure Search szolgáltatás [Azure-portálon](https://portal.azure.com/) vagy hello [Azure erőforrás-kezelési API](/rest/api/searchmanagement/). Dönthet úgy, vagy más előfizetők megosztott hello szabad service vagy egy [réteg fizetett](https://azure.microsoft.com/pricing/details/search/) , amely csak a szolgáltatás által használt erőforrások dedicates. A fizetett szinteken a két dimenzió egy szolgáltatás is skálázható: 

- Adja hozzá a replikák toogrow betölti a kapacitás toohandle nehéz lekérdezést.   
- Adja hozzá a további dokumentumok partíciók toogrow tárolót. 

Külön-külön kezelnek dokumentum tárolás és a lekérdezési teljesítményt, továbbá bármikor kalibrálhatja finanszírozása éles követelmények alapján.

### <a name="step-2-create-index"></a>2. lépés: Az index létrehozása
Mielőtt feltöltheti a kereshető tartalom, először meg kell határoznia az Azure Search-index. Az index olyan, mint egy adatbázis tábláinak, amely tartalmazza az adatokat és a keresési lekérdezések fogad el. Megadhatja a hello index séma toomap tooreflect hello struktúra hello dokumentumok toosearch, hasonló toofields-adatbázisban kívánja.

A séma hozhatók létre hello Azure portálon vagy programozottan használatával hello [.NET SDK](search-howto-dotnet-sdk.md) vagy [REST API](/rest/api/searchservice/).

### <a name="step-3-index-data"></a>3. lépés: Az Index adatok
Index definiálása után most készen áll a tooupload tartalmat. A lekérés és küldés modell is használhatja.

hello lekéréses modell külső forrásból származó lekéri az adatokat. A támogatott *indexelők* , amely egyszerűsítésére és automatizálhatja az adatfeldolgozást, például a kapcsolódás, az olvasást és adatok szerializálása aspektusait. [Indexelő](/rest/api/searchservice/Indexer-operations) elérhető Azure Cosmos DB, az Azure SQL Database, Azure Blob Storage és egy Azure virtuális gép futó SQL-kiszolgálót. Az indexelő igény szerint vagy ütemezett Adatfrissítés konfigurálhatja.

hello leküldési modelljével hello SDK vagy a REST API-k, tooan index frissített dokumentumok küldésére szolgáló keresztül valósul meg. Adatok szinte bármilyen adatkészletből hello JSON formátumban tolható ki. Lásd: [hozzáadása, update vagy delete dokumentumok](/rest/api/searchservice/addupdate-or-delete-documents) vagy [hogyan toouse hello .NET SDK-val)](search-howto-dotnet-sdk.md) útmutatót az adatok betöltéséhez.

### <a name="step-4-search"></a>4. lépés: keresés
Index feltöltése, után is [keresési lekérdezések ki](/rest/api/searchservice/Search-Documents) tooyour szolgáltatás egyszerű HTTP-n keresztül kérelmeket a REST API vagy hello .NET SDK-val.

## <a name="how-it-compares"></a>Összehasonlítja azt

Gyakran kérje meg az ügyfelek hogyan [teljes szöveges keresés az Azure Search](search-lucene-query-architecture.md) összehasonlítja [teljes szöveges keresés](https://en.wikipedia.org/wiki/Full_text_search) az adatbázis termékben. A rendszer a választ, hogy Azure Search nyelvi lehetőségek állnak-e a részletesebb és rugalmasabb, Lucene lekérdezéseket, a nyelvi elemzőkkel Lucene és a Microsoft, egyéni lekérdezések fonetikus vagy más speciális bemenetek és hello képességét toomerge adatok támogatása többféle forrás hello keresési index. 

Egy másik nincsenek pont, hogy a keresési megoldás címek hello teljes keresési élményt biztosít. Például érdemes egyéni pontozási eredményeinek, jellemzőalapú navigációs irányuló szűrést, a találatok kiemelése és typeahead konstrukciókat. 

Eszközök figyelése és megismerése lekérdezés tevékenység számára is be egy keresési megoldás döntési is figyelembe. Például támogatja az Azure Search [keresési forgalom analytics](search-traffic-analytics.md) metrikáihoz a átkattintós arány, felső keres keresés nélkül kattint, és így tovább. A termékek, ahol a keresési az bővítménye, nem valószínű toofind most ezeket a szolgáltatásokat.

Erőforrás-használat egy másik szempont. Természetes nyelvű keresési legtöbbször számításilag intenzív. A felhasználók némelyike kiszervezett keresési műveletek tooAzure keresési erőforrásként egy módon toopreserve gép tranzakció feldolgozásra. Externalizing keresés méretezési toomatch lekérdezés kötet könnyen módosíthatja.

Ha úgy döntött, a dedikált keresési toogo, a következő döntés, szükség van egy felhőalapú szolgáltatás, vagy egy helyszíni kiszolgáló között. Egy felhőalapú szolgáltatás használata a hello megfelelő választás, ha azt szeretné egy [kulcsrakész megoldást minimális terhelés és karbantartási és állítható méretezési](#cloud-service-advantage).

Belül hello felhő összeállítást a több szolgáltatók összehasonlíthatónak alapvető szolgáltatásokat, a teljes szöveges keresés, földrajzi keresési és hello képességét toohandle keresési bemenet a félreérthetőség bizonyos szintű. Általában rendelkezik egy [speciális funkció](#feature-drilldown), vagy a hello könnyű, és a teljes egyszerű API-k, eszközök és felügyeletéhez, amely meghatározza, hogy hello legjobb térkihasználás érdekében a.

Felhő szolgáltatóknál Azure Search teljes szöveges keresés munkaterhelések esetében felett tartalom tárolók és a Azure-adatbázisok elsősorban a keresést a adatok beolvasása és a tartalom navigációs használó alkalmazások esetében. Kulcs szintjeiről a következők:

+ Az indexelő rétegben hello Azure Adatintegráció (webbejárók)
+ Azure-portál a központi felügyeleti
+ Az Azure méretezés, a megbízhatóság és a világszínvonalú rendelkezésre állása
+ A nyelvi és egyéni elemzést követően elemzőkkel 56 nyelvű teli teljes szöveges kereséshez
+ [Alapvető szolgáltatások általános toosearch-központú alkalmazások](#feature-drilldown): pontozási, értékkorlátozás, vonatkozó javaslatokat, szinonimák, földrajzi-keresés, és több.

> [!Note]
> toobe egyszerű, -Azure adatforrások teljes mértékben támogatottak, de indexelők, hanem több kód igényel ügyfélleküldéses módszer támaszkodnak. Az API-k használatával, átadható bármely JSON dokumentum gyűjtemény tooan Azure Search-index.

Között a felhasználók e képes tooleverage hello legszélesebb az Azure Search szolgáltatások online katalógusok, üzleti programok és a dokumentum felderítési alkalmazások közé tartoznak.

## <a name="rest-api--net-sdk"></a>REST API-t |} .net SDK

Számos feladatot hello portálon hajtható végre, amíg a fejlesztők számára a meglévő alkalmazások toointegrate keresési funkciókat készült Azure Search. a következő alkalmazásprogramozási felületek hello érhetők el.

|Platform |Leírás |
|-----|------------|
|[REST](/rest/api/searchservice/) | Bármely programozási platform és nyelvi, többek között a Xamarin, Java és JavaScript által támogatott HTTP-parancsok|
|[.NET SDK](search-howto-dotnet-sdk.md) | .NET burkolót hello REST API-t kínál hatékony kódolása a C# és egyéb felügyelt kódú nyelvek célcsoport-kezelési hello .NET-keretrendszer |

## <a name="free-trial"></a>Ingyenes próbalehetőség
Az Azure-előfizetők is [telepíteni egy szolgáltatást a hello ingyenes szint](search-create-service-portal.md).

Ha az előfizető nem, akkor [szabad nyissa meg az Azure-fiók](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F). Jóváírásokat kap a próbálhatja ki a fizetős Azure-szolgáltatásokat. Azok lejárta után, megtarthatja hello fiókot, és használjon [ingyenes Azure-szolgáltatásokat](https://azure.microsoft.com/free/). A hitelkártya soha nem feladata, kivéve, ha explicit módon a beállítások módosításához és kérje meg a toobe számítjuk fel.

Alternatív megoldásként, [aktiválhatja MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): az MSDN-előfizetés biztosít Önnek krediteket minden hónapban, fizetős Azure-szolgáltatásokat is használhat. 

## <a name="how-tooget-started"></a>Hogyan tooget el

1. Szolgáltatás létrehozása a hello [ingyenes szint](search-create-service-portal.md).

2. Egy vagy több hello oktatóanyagok következő lépéseit. 

  + [Hogyan toouse hello .NET SDK](search-howto-dotnet-sdk.md) bemutatja a fő hello lépések a felügyelt kódban.  
  + [Ismerkedés a REST API hello](https://github.com/Azure-Samples/search-rest-api-getting-started) mutat be hello azonos hello REST API használatával.  
  + [Az első index létrehozása hello portálon](search-get-started-portal.md) hello portál beépített indexelő és prototípus szolgáltatásokat mutatja be.   

A keresőmotorok olyan hello közös illesztőprogramok az adatok beolvasása a mobilalkalmazásokban, hello webes, és a vállalati adatokat tárolja. Az Azure Search lehetővé teszi az eszközök létrehozásához egy keresési élményt hasonló toothose nagy kereskedelmi webhelyen.

Ez a program manager Liam Cavanagh 9 perces videót megtudhatja, hogyan integrálható a keresőprogramok találatait is kihasználhatja az alkalmazás. Rövid bemutatók tér ki az Azure Search és egy tipikus munkafolyamat néz funkciói. 

>[!VIDEO https://channel9.msdn.com/Events/Connect/2016/138/player]
 
+ 0 – 3 perc magában foglalja az legfontosabb funkcióira és a használati eseteket.
+ 3 – 4 perc foglalkozik kiépítése szolgáltatáshoz. 
+ 4-6 percig az adatok importálása varázsló használt toocreate hello beépített ingatlan adatkészlet index ismerteti.
+ 6 – 9 perc keresési ablak és különböző lekérdezések tartalmazza.


