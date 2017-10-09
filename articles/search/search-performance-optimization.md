---
title: "aaaAzure keresési teljesítmény- és optimalizálási szempontok |} Microsoft Docs"
description: "Azure Search teljesítmény hangolására és optimális méretezési konfigurálása"
services: search
documentationcenter: 
author: LiamCavanagh
manager: pablocas
editor: 
ms.assetid: 4d3cd864-29d2-4921-be0d-a3f1a819de46
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: liamca
ms.openlocfilehash: 725149ba32773ab6b4c9d4b441424aba78db7c42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-search-performance-and-optimization-considerations"></a>Az Azure Search teljesítmény- és optimalizálási kapcsolatos szempontok
Egy remek keresési élményt a kulcs toosuccess sok Mobile és webes alkalmazásokhoz. Ingatlan, a tooused autó piacterek tooonline katalógusok, gyors keresési és vonatkozó eredményeket érinti hello felhasználói élmény. Ez a dokumentum azt tapasztalja, hogy hogyan tooget hello Azure Search, különösen ha speciális célú a lehető legjobban kifinomult méretezhetőséget, többnyelvű vagy egyéni rangsorolási követelményei ajánlott eljárásai tervezett toohelp.  Emellett ez a dokumentum belső ismerteti, és ismerteti a különböző módszer valós felhasználói alkalmazásokban hatékonyan működjön.

## <a name="performance-and-scale-tuning-for-search-services"></a>Teljesítmény és méretezés hangolása keresési szolgáltatások
Folyamatban, például a Bing és Google és hello nagy teljesítményt biztosítanak minden használt toosearch motor.  Ennek eredményeképpen, amikor az ügyfelek használja a keresés-t vagy a mobilalkalmazás, azok fog várhatóan hasonló teljesítményt nyújt.  A keresési teljesítmény optimalizálása, amikor hello legjobb módszer egyik toofocus késéseitől, amely hello idejét lekérdezés során a toocomplete, és térjen vissza az eredményeket.  Optimalizálja a keresési késés esetén fontos, hogy:

1. Válasszon ki egy célként megadott várakozási ideje (vagy maximális időtartama), hogy egy tipikus keresési kérelem igénybe vehet toocomplete.
2. Hozzon létre, és tesztelje a valódi munkaterhelés szemben a keresőszolgáltatása egy reális dataset toomeasure a késés sebesség.
3. / Másodperc (QPS) lekérdezések száma kevés kezdődnie, és továbbra is tooincrease hello szám hello teszt végrehajtása hello lekérdezés késés hello alá süllyed meg a célként megadott várakozási ideje.  Ez az egy fontos teljesítményteszt toohelp bővített azt tervezi, ahogy az alkalmazás használata során növekszik.
4. Ahol lehetséges, felhasználhatja a HTTP-kapcsolatoknál.  Ha hello Azure Search .NET SDK-t használ, ez azt jelenti, hogy szabad újra egy példányát, vagy [SearchIndexClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient) példányt, és ha használ hello REST API-t, egyetlen HttpClient szabad újra.

Ezek létrehozása során a munkaterhelések tesztelése, vannak bizonyos Azure Search tookeep figyelembe jellemzői:

1. Toopush sok keresési lekérdezések egy időben, hogy hello erőforrások az Azure Search szolgáltatás elérhető lesz túlterhelik lehetőség.  Ha ez történik, megjelenik a HTTP 503-as válaszkódot.  Emiatt is ajánlott toostart keresési kérelmeket toosee hello különbségek késés díjszabás különböző címtartományai hozzáadása során további keresési kérelmeket.
2. Az hatással lesz a keresési tartalom tooAzure feltöltését hello általános teljesítménye, és késését hello az Azure Search szolgáltatás.  Amíg a felhasználók keresést hajt végre toosend adatokat várt, esetén fontos tootake a munkaterhelést a tesztek-fiókot.
3. Nem minden keresési lekérdezés hajtja végre: hello azonos teljesítményszintet.  Például egy dokumentum keresési vagy keresési javaslat általában végrehajtják gyorsabb, mint az értékkorlátozás és a szűrők jelentős számú lekérdezés.  Több olyan lekérdezést, a toosee várt fiók létrehozásakor a tesztek legjobb tootake hello.  
4. A search kérelemmel mértékben eltérő változata azért fontos, mert ha folyamatosan végrehajtása hello azonos keresési kérelmeket, az adatok gyorsítótárazása fog start toomake teljesítmény keresse meg nagyobb, mint előfordulhat, hogy több különböző lekérdezéssel állítva, akkor.

> [!NOTE]
> [A Visual Studio terhelés tesztelés](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release) van egy valóban jó módszer tooperform a referenciaalap teszteli, mert a tooexecute HTTP-kérelmek esetén egyébként használnia az Azure Search lekérdezések végrehajtása, és lehetővé teszi a kérelmek párhuzamos folyamatkezelést biztosítja.
> 
> 

## <a name="scaling-azure-search-for-high-query-rates-and-throttled-requests"></a>Azure Search szolgáltatást a lekérdezési díjszabás méretezés és szabályozva kérelmek
Túl sok szabályozottan halmozott kérelmet kap, illetve haladhatja meg a célként megadott várakozási ideje díjszabás egy nagyobb lekérdezés terhelését a meggyőződhet toodecrease késés sebességét az alábbi két módszer egyikével:

1. **Növelje a replikák:** replika Azure Search-tooload egyenleg kéréseket a meghatározott hello így több másolatot az adatok másolatát hasonlít.  Összes az terheléselosztás és a replikák között adatainak replikálása az Azure Search kezeli, és módosíthatja a szolgáltatás bármikor lefoglalt replikák hello száma.  Standard keresési szolgáltatást too12 replikák és egy egyszerű keresés szolgáltatásban 3 replikák foglalhatja. Replikák lehet beállítani származhatnak hello [Azure-portálon](search-create-service-portal.md) vagy [PowerShell](search-manage-powershell.md).
2. **Növelje keresési réteg:** az Azure Search érkezik egy [a rétegek száma](https://azure.microsoft.com/pricing/details/search/) , valamint ezek a rétegek egyes különböző teljesítményszintet.  Bizonyos esetekben előfordulhat, hogy a hello rétegű nem adhatók meg elég alacsony késési igényű díjszabás, akkor is, ha a replikákat a rendszer kimenő kihasználtságában sok lekérdezések.  Ebben az esetben érdemes lehet tooconsider hello magasabb keresési rétegek például, hogy kiválóan alkalmas a dokumentumok és a rendkívül nagy lekérdezés munkaterhelés nagy számú forgatókönyvek hello Azure keresési S3 szint egyikét használhatja.

## <a name="scaling-azure-search-for-slow-individual-queries"></a>Azure Search lassú egyes lekérdezések skálázás
Miért késés díjszabás lassú lehet akkor túl hosszú toocomplete véve egyetlen lekérdezésből.  Ebben az esetben replikák hozzáadása nem növeli a késés sebességét.  Ebben az esetben két módon érhető el:

1. **Növelje a partíciók** partíció egy olyan mechanizmus az adatok között további erőforrásokat a felosztás.  Emiatt egy második partíció hozzáadásakor az adatok beolvasása felosztása két.  Egy harmadik partíciót az index felosztja a három stb.  Ez is, hogy bizonyos esetekben lassú lekérdezések gyorsabban fog miatt toohello párhuzamos folyamatkezelést biztosítja számítás hello hatással van.  Van néhány példa ahol úgy találtuk, ez működnek jól rendkívül alacsony szelektivitás lekérdezések tartalmazó lekérdezések párhuzamos folyamatkezelést biztosítja.  Ez sok dokumentum megfelelő lekérdezések áll, vagy ha értékkorlátozás tooprovide keresztül nagyszámú dokumentumok száma.  Mivel a nagy mennyiségű számítási tooscore hello alkalmazhatóságát hello dokumentumok vagy a toocount hello számok dokumentumok, további partíciók segíthet tooprovide további számítási hozzáadása szükséges.  
   
   Legfeljebb 12 Standard keresési szolgáltatást a partíciók száma és a hello alapvető keresési szolgáltatás 1 partíció lehet.  Partíciók lehet beállítani származhatnak hello [Azure-portálon](search-create-service-portal.md) vagy [PowerShell](search-manage-powershell.md).
2. **Korlát nagy számosságot mezők:** nagy számosságot mező egy kategorizálható vagy szűrhető mezője, amely jelentős mennyiségű egyedi értékkel rendelkezik, és emiatt átveszi jelentős erőforrásokat toocompute eredmények tartalmaz.   Például termék azonosító vagy leírás mező beállítása kategorizálható szűrhető tenné a nagy számosságot mert dokumentum toodocument hello értékeit a legtöbb egyedi. Ahol lehetséges, hello nagy számosságot mezők számának korlátozása.
3. **Növelje keresési réteg:** áthelyezése felfelé tooa magasabb Azure Search réteg lehet egy másik módja tooimprove teljesítmény lassú lekérdezések.  Minden magasabb réteg emellett gyorsabb CPU, illetve több memóriával, amely pozitív hatással lehet a lekérdezések teljesítményét.

## <a name="scaling-for-availability"></a>Rendelkezésre állási csoportok skálázási módjának
Replikák nem csak lekérdezés késés csökkentése érdekében, de lehetővé teszi a magas rendelkezésre állásra.  Az egy replikához számíthat rendszeres leállás miatt tooserver újraindítások után a szoftverfrissítések vagy egyéb karbantartási események történik.  Ennek eredményeképpen fontos tooconsider, ha az alkalmazás által igényelt magas rendelkezésre állású (lekérdezések) keresések továbbá az írási műveletek (indexelési esemény).  Az Azure Search kínál SLA-beállítások az összes hello fizetett keresési ajánlatokat az alábbi attribútumok hello:

* a csak olvasható munkaterhelések (lekérdezések) magas rendelkezésre állás 2-replikával
* a magas rendelkezésre állás írható-olvasható munkaterhelések (lekérdezések és az indexelés) 3 vagy több replikák

A további részletekért látogasson el a hello [Azure Search szolgáltatásiszint-megállapodás](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

Mivel a replikák adatokról, amelyek több replika lehetővé teszi, hogy az Azure Search toodo gép újraindul, és karbantartási ellen úgy, hogy közben egyszerre egy másodpéldány lekérdezi toocontinue toobe elleni végre hello más replikákat.  Ezért is szüksége lesz tooconsider hogyan üzemszünet most már rendelkezik egy kisebb hello adatok példányán végre toobe hello lekérdezések hatással lehet.

## <a name="scaling-geo-distributed-workloads-and-provide-geo-redundancy"></a>Földrajzilag elosztott munkaterhelések méretezés és földrajzi redundancia
Földrajzilag elosztott munkaterheléseknél találja, hogy hello adatközpont, amelyen az Azure Search szolgáltatás található távol lévő felhasználók rendelkeznek-e a késés nagyobb sebesség.  Ezért is gyakran több keresési szolgáltatások régiókban lévő szorosabb közelségi kapcsolat toothese felhasználók fontos toohave.  Az Azure Search jelenleg nem biztosít egy földrajzi replikálása Azure Search-indexek automatikus módszerrel régiók között, de néhány módszer használható is a folyamat egyszerű tooimplement és kezelését. A cikkben szereplő hello következő néhány szakasz.

hello search szolgáltatás földrajzi eloszlása készlete célja toohave két, vagy további indexek elérhető két vagy több régiókban, ahol a felhasználó lesz átirányítva toohello Azure Search szolgáltatás, amely hello legkisebb mértékű késleltetést ebben a példában látható módon:

   ![Kereszt-régiói lapja][1]

### <a name="keeping-data-in-sync-across-multiple-azure-search-services"></a>Adatok szinkronban tartása több Azure Search szolgáltatásban
Két lehetőség, hogy az elosztott keresési szolgáltatások szinkronban segítségével álló hello [Azure Search-indexelőt](search-indexer-overview.md) vagy leküldéses API hello (más néven tooas hello [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/)).  

### <a name="azure-search-indexers"></a>Az Azure Search indexelő
Hello Azure Search-indexelőt használ, ha már importálás adatváltozásokat például az Azure SQL Database vagy az Azure Cosmos DB központi adattárolót. Amikor létrehoz egy új keresés szolgáltatás, akkor egyszerűen is hozzon létre egy új Azure Search-indexelőt, hogy a szolgáltatás adott pontok toothis ugyanazon adattár. Így, amikor új módosítások hello adattárba lépnek majd fogja indexelik hello különböző indexelők.  

Íme egy példa mi néz ki az architektúra.

   ![Az elosztott indexelő és kombinációja egyetlen adatforrás][2]

### <a name="push-api"></a>Leküldéses API
Ha használ hello Azure Search leküldéses API túl[az Azure Search-index a tartalom frissítése](https://docs.microsoft.com/rest/api/searchservice/update-index), megtarthatja a különböző keresési szolgáltatások szinkronban módosítások tooall keresési szolgáltatások küldésével, amikor szükség egy frissítésre.  Ennek során is fontos toomake meg arról, hogy toohandle esetekben egy frissítés tooone keresési szolgáltatás nem sikerült egy vagy több frissítést.

## <a name="leveraging-azure-traffic-manager"></a>Az Azure Traffic Manager kihasználva
[Az Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) tooroute kérelmek toomultiple földrajzi található webhelyeket, amelyeket majd üzemelnek több Azure Search szolgáltatás lehetővé teszi.  A Traffic Manager hello egyik előnye, hogy mintavételi Azure Search tooensure, hogy akkor érhető el, és útvonal-felhasználók tooalternate keresési szolgáltatások állásidő hello eseményben.  Emellett ha a search kérelmek használatával az Azure webhelyek útválasztáson Azure Traffic Manager lehetővé teszi tooload egyenleg esetekben, amikor hello webhely mentése, de nem az Azure Search.  Íme egy példa milyen hello architektúra, amely kihasználja a Traffic Manager.

   ![Kereszt-lapján régiói, a központi Traffic Managerrel][3]

## <a name="monitoring-performance"></a>A teljesítmény figyelése
Az Azure Search hello képességét tooanalyze és a figyelő hello teljesítményt nyújt a szolgáltatás keresztül [keresési forgalom Analytics (STA)](search-traffic-analytics.md). Keresztül STA opcionálisan bejelentkezhet hello egyedi keresési műveleteket, valamint az összesített metrikák tooan Azure Storage-fiók majd elemzés érdekében dolgoz fel vagy ábrázolt Power BI-ban.  STA metrikákat használ, például a lekérdezések vagy lekérdezési válaszidőt átlagos száma teljesítménystatisztikáit tekintheti meg.  Ezenkívül hello műveleti naplózás lehetővé teszi a toodrill részleteinek keresési műveletek.

STA egy értékes eszközt toounderstand késés sebesség az adott Azure Search szempontjából.  Mivel hello lekérdezés teljesítménymutatók naplózott hello idő alapulnak lekérdezés (a hello idő alatt, kért toowhen által kiküldött) az Azure Search teljes feldolgozása toobe vesz igénybe, képes toouse áll a toodetermine hello Azure Search szolgáltatás a késési problémák esetén oldalán található, vagy kívül hello szolgáltatást, például hálózati késés korlátozza.  

## <a name="next-steps"></a>Következő lépések
toolearn árképzési szinteket, és a szolgáltatások korlátok minden egy hello kapcsolatos további információkért lásd: [szolgáltatási korlátait, az Azure Search](search-limits-quotas-capacity.md).

Látogasson el [kapacitástervezés](search-capacity-planning.md) toolearn partícióazonosító és másodpéldány kombinációk többet.

A(z) további teljesítményére és néhány szemléltetés, hogyan tooimplement hello optimalizálásokat cikkben említett toosee tekintse meg a következő videó hello:

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON319/player]
> 
> 

<!--Image references-->
[1]: ./media/search-performance-optimization/geo-redundancy.png
[2]: ./media/search-performance-optimization/scale-indexers.png
[3]: ./media/search-performance-optimization/geo-search-traffic-mgr.png
