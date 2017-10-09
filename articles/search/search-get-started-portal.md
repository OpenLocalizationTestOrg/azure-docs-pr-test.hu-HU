---
cím: aaa "oktatóanyag: az első Azure Search-index létrehozása hello portálon |} Microsoft Docs"Leírás: hello Azure-portálon, az előre definiált minta adatok toogenerate index. Használhatja a teljes szöveges keresést, a szűrőket, az aspektusokat, az intelligens keresést, a geosearch funkciót és sok mást.
szolgáltatások: documentationcenter keresése: "Szerző: HeidiSteen manager: jhubbard szerkesztőben:" címkék: azure-portálon

MS.AssetId: 21adc351-69bb-4a39-bc59-598c60c8f958 ms.service: ms.devlang keresése: na ms.workload: ms.topic keresése: hero-article ms.tgt_pltfrm: na ms.date: 06/26/2017 ms.author: heidist

---
# <a name="tutorial-create-your-first-azure-search-index-in-hello-portal"></a>Oktatóanyag: Az első Azure Search-index létrehozása hello portálon

Hello Azure-portálon, az útmutató egy előre meghatározott minta dataset tooquickly készítése hello használatával **adatimportálás** varázsló. A **keresési ablakban** használhatja a teljes szöveges keresést, a szűrőket, az aspektusokat, az intelligens keresést és a geosearch funkciót.  

Így kódolás nélkül juthat előre meghatározott adatokhoz, és azonnal lehetősége van Önt érdeklő lekérdezések írására. A portál eszközei nem helyettesítik a kódolást, de az eszközök a következő feladatokhoz hasznosak lehetnek:

+ Gyakorlati tanulás minimális bevezetés után
+ Index prototípusának elkészítése kód írása előtt az **Adatok importálása** területen
+ A lekérdezések és az elemzőszintaxis tesztelése a **keresési ablakban**
+ Megtekintheti a meglévő index közzétett tooyour szolgáltatás, és a hozzá tartozó attribútumok keressen

**Becsült időtartam:** Körülbelül 15 perc, de tovább is tarthat, ha szükség van a fiók vagy szolgáltatás regisztrálására. 

Másik lehetőségként hatékonyságának növeléséhez használja a [kódalapú bemutatása tooprogramming Azure Search .NET](search-howto-dotnet-sdk.md).

## <a name="prerequisites"></a>Előfeltételek

Az oktatóanyag azt feltételezi, hogy rendelkezik [Azure-előfizetéssel](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) és az [Azure Search szolgáltatással](search-create-service-portal.md). 

Ha azonnal szeretné tooprovision egy szolgáltatás, figyelheti az ebben az oktatóanyagban szereplő lépések hello a 6-perc bemutatója három perc kezdődő ebbe a [Azure keresési áttekintő videó](https://channel9.msdn.com/Events/Connect/2016/138).

## <a name="find-your-service"></a>A szolgáltatása megkeresése
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Nyissa meg az Azure Search szolgáltatás szolgáltatási irányítópultját hello. Ha hello szolgáltatás csempe tooyour irányítópult nem rögzíti, találja meg a szolgáltatás ezzel a módszerrel: 
   
   * Hello Ugrósávon, kattintson **további szolgáltatások** hello bal oldali navigációs panelen hello alján.
   * Hello keresési mezőbe, írja be a *keresési* tooget az előfizetéshez tartozó szolgáltatások keresési listáját. A szolgáltatás meg kell jelennie hello listájában. 

## <a name="check-for-space"></a>Szabad terület ellenőrzése
Sok ügyfél kezdődnie hello ingyenes szolgáltatás. Ebben a verzióban, korlátozott toothree indexek, három adatforrásra és három indexelőre. Mielőtt hozzákezdene, ellenőrizze, hogy elegendő hellyel rendelkezik-e további elemek számára. Az oktatóanyagban minden objektumból egyet hozhat majd létre. 

> [!TIP] 
> A hello szolgáltatás irányítópult csempéi megjelenítése, hogy hány indexek indexelők és adatforrások már rendelkezik. hello indexelő csempe megjeleníti a sikeres és sikertelen mutatók. Kattintásszám hello csempe tooview hello indexelő. 
>
> ![Indexelők és adatforrások csempéje][1]
>

## <a name="create-index"></a> Index létrehozása és az adatok betöltése
A keresési lekérdezések egy *index* segítségével ismétlődnek, amely kereshető adatokat, metaadatokat és bizonyos keresési viselkedések optimalizálásához használt szerkezeteket tartalmaz.

tookeep Ez a feladat portálalapú, beépített mintát adatkészletre mutató egy indexelővel hello segítségével bejárható használjuk **adatimportálás** varázsló. 

#### <a name="step-1-start-hello-import-data-wizard"></a>1. lépés: Hello adatok importálása varázsló indítása
1. Az Azure Search szolgáltatás irányítópultján kattintson **adatimportálás** a hello parancs sáv toostart egy varázslót, amely létrehozó és feltöltő index.
   
    ![Adatok importálása parancs][2]

2. Hello varázslóban kattintson **adatforrás** > **minták** > **realestate-us-minta**. Az adatforrás neve, típusa és kapcsolódási adatai előre konfigurálva vannak. Létrehozását követően „meglévő adatforrássá” válik, amely más importálási műveletek során ismét felhasználható.

    ![Minta adatkészlet kiválasztása][9]

3. Kattintson a **OK** toouse azt.

#### <a name="step-2-define-hello-index"></a>2. lépés: Hello index meghatározása
Az index létrehozása jellemzően manuális kódalapú, de hello varázsló hozhat létre bármely más adatforrásét, akkor által bejárható index. A minimális követelmény index szükséges, egy nevet, és egy mezőt, módon dokumentumban kulcs toouniquely hello megjelölni azonosíthatja a minden dokumentumot.

A mezők adattípusokkal és attribútumokkal rendelkeznek. hello jelölőnégyzetek hello tetején nem *indexattribútumok* hello mezővel hogyan vezérlése. 

* **Lekérhető**: azt jelenti, hogy a mező a keresési eredmények listájában jelenik meg. A jelölőnégyzet törlésével az egyes mezőket a keresési eredmények korlátjain kívül esőként jelölheti meg, például amikor a mezőket csak szűrőkifejezésekben használják. 
* **Szűrhető**, **Rendezhető** és **Kategorizálható**: azt határozzák meg, hogy egy mező használható-e szűrésben, rendezésben vagy jellemzőalapú navigációs szerkezetben. 
* **Kereshető**: azt jelenti, hogy a mező szerepel a teljes szöveges keresésben. A karakterláncok kereshetők. A numerikus és logikai mezőket gyakran nem kereshetőként jelölik meg. 

Alapértelmezés szerint hello varázsló megvizsgálja a hello adatforrását egyedi azonosítói hello kulcsmező hello alapjaként. A karakterláncok lekérhetőként és kereshetőként vannak megjelölve. Az egész számok lekérhetőként, szűrhetőként, rendezhetőként és kategorizálhatóként vannak megjelölve.

  ![Létrehozott ingatlanindex][3]

Kattintson a **OK** toocreate hello index.

#### <a name="step-3-define-hello-indexer"></a>3. lépés: Hello indexelő meghatározása
Továbbra is a hello **adatimportálás** varázsló, kattintson a **indexelő** > **neve**, és adjon meg egy nevet az hello indexelő. 

Ez az objektum egy végrehajtható folyamatot határoz meg. Ütemezésbe helyezheti, de ismétlődő ütemezés szerint most használja hello alapértelmezett beállítás toorun hello indexelő után azonnal, amikor rákattint **OK**.  

  ![ingatlanindexelő][8]

## <a name="check-progress"></a>Folyamat állapotának ellenőrzése
toomonitor adatok importálása, lépjen vissza toohello szolgáltatás irányítópultját, görgessen lefelé, és kattintson duplán a hello **indexelők** csempe tooopen hello indexelők listájának. Megtekintheti az újonnan létrehozott hello indexelő hello listában, az állapotát jelző "folyamatban" vagy sikeres indexelt dokumentumok számának hello együtt.

   ![Indexelő állapotüzenete][4]

## <a name="query-index"></a>Lekérdezés hello indexe
Most már rendelkezik egy keresési indexszel, amely készen áll a tooquery. **Keresési ablak** hello portálba épített lekérdezési eszköz. Biztosít egy keresőmezőt, amellyel ellenőrizheti, hogy a keresési eredmények megfelelnek-e a vártaknak. 

> [!TIP]
> A hello [Azure keresési áttekintő videó](https://channel9.msdn.com/Events/Connect/2016/138), a lépéseket követve hello hello videóban 6m08s, egy.
>

1. Kattintson a **keresési ablak** hello parancssávon.

   ![Keresési ablak parancs][5]

2. Kattintson a **index módosítása** hello a parancs sáv tooswitch túl*realestate-us-minta*.

   ![Index és API-parancsok][6]

3. Kattintson a **be API-verzió** a hello parancs sáv toosee REST API-kat is elérhetők. API-k joga általában még nem kiadott toonew funkciók megtekintése. Az alábbi hello lekérdezések esetén használja hello általánosan elérhető verzió (2016-09-01), kivéve irányítja a rendszer. 

    > [!NOTE]
    > [Az Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents) és hello [.NET kódtár](search-howto-dotnet-sdk.md#core-scenarios) teljesen egyenértékű, de **keresési ablak** csak felszerelt toohandle REST hívások van. Mindkét fogad a szintaxis [egyszerű lekérdezés szintaxisát](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) és [Lucene lekérdezéselemzőben teljes](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), és az összes elérhető, a keresési paraméterek hello [keresés a dokumentum](https://docs.microsoft.com/rest/api/searchservice/search-documents) műveletek.
    > 

4. Hello keresősávban, adja meg az alábbi hello lekérdezési karakterláncot, és kattintson a **keresési**.

  ![Példa keresési lekérdezésre][7]

**`search=seattle`**

+ Hello `search` paraméter használt tooinput egy kulcsszavas keresést a teljes szöveges keresés, ebben az esetben listaelemek visszaadó érvényesíthet állapotban, amely *Budapest* hello dokumentumban bármely kereshető mező. 

+ **Keresési ablak** eredményeket ad vissza a JSON-ban, ha ez utóbbi érték részletes és rögzített tooread dokumentumok sűrű struktúrával rendelkezik. Attól függően, hogy a dokumentumok szükség lehet, hogy kezeli keresési eredmények tooextract fontos elemeit toowrite kódot. 

+ Dokumentumok álló minden mező lekérhető hello index jelölésű. tooview indexattribútumok hello portálon, kattintson a *realestate-us-minta* a hello **indexek** csempére.

**`search=seattle&$count=true&$top=100`**

+ Hello `&` szimbólum használt tooappend keresési paramétereket, amely bármilyen sorrendben adható meg. 

+  Hello `$count=true` paraméter az adott vissza az összes dokumentum hello sum számát adja vissza. A szűrőlekérdezések ellenőrzéséhez megfigyelheti a `$count=true` által jelentett módosításokat. 

+ Hello `$top=100` legmagasabb értéket ad vissza hello kívül teljes hello 100 dokumentumok rangsorolását. Alapértelmezés szerint Azure Search találatokat hello első 50 ajánlott. Akkor növelhető és csökkenthető az összeget hello `$top`.

**`search=*&facet=city&$top=2`**

+ A `search=*` egy üres keresés. Az üres keresések mindenben keresnek. Egy üres lekérdezés túl van szűrése elküldése vagy dokumentumok hello teljes készleten dimenzió egyik oka. Például szeretné egy értékkorlátozás navigációs struktúra tooconsist hello index összes város.

+  `facet`a navigációs beolvasása struktúra, hogy átadhatók tooa felhasználói felületének vezérlői. Kategóriákat és egy számot ad vissza. Ebben az esetben a kategóriák városokat hello száma alapulnak. Az Azure Searchben nincs összesítés, de megbecsülheti az összesítést a `facet` használatával, amely az egyes kategóriákban lévő dokumentumok számát adja meg.

+ `$top=2`vissza számos lehetőséget kínál két dokumentumot, használható ábrázoló `top` tooboth csökkentésére, vagy növelje az eredményeket.

**`search=seattle&facet=beds`**

+ Ez a lekérdezés az ágyak értékkorlátozását jelenti a *Seattle* szöveges kereséséhez. `"beds"`adható meg, mert hello négyzet be van jelölve, a lekérhető, szűrhető, kategorizálható hello index és hello értékek azt tartalmaz (numerikus, 1 – 5) egy dimenzió, a listaelemek kategorizálásához csoportokba (Hálószobák 3, 4 Hálószobák a listaelemek) megfelelő. 

+ Csak a szűrhető mezők értéke korlátozható. Csak lekérhető mezők adhatók vissza a hello eredményei között.

**`search=seattle&$filter=beds gt 3`**

+ Hello `filter` paraméter megadott hello feltételeknek megfelelő eredményeket ad vissza. Ebben az esetben: 3-nál több hálószoba. 

+ A szűrőszintaxis egy OData-konstrukció. További információk: [OData-szűrőszintaxis](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search).

**`search=granite countertops&highlight=description`**

+ A találatok kiemelése hivatkozik tooformatting szöveg megfelelő hello kulcsszóval megadott megfelel találhatók egy adott mező. Ha a keresett mélyen van megbúvó leírását, találati kijelölő toomake adhat hozzá az egyszerűbb toospot. Ebben az esetben hello formázott kifejezést `"granite countertops"` könnyebb toosee hello Leírás mezőben van.

**`search=mice&highlight=description`**

+ A teljes szöveges keresés hasonló szemantikával rendelkező szóalakok keresésére használható. Ebben az esetben a keresési eredmények "egér", a válasz tooa kulcsszavas keresést a "egerek" egér fertőzés rendelkező házak kijelölt szöveget tartalmaznak. Azonos word megjelenhet eredmények nyelvi elemzés miatt hello másik formája. 

+ Az Azure Search szolgáltatás összesen 56, a Lucene-től és Microsoft-tól származó elemzőt támogat. hello Azure Search által használt alapérték hello szabványos Lucene analyzer. 

**`search=samamish`**

+ Hibás szavak, például a "samamish" hello Samammish plató hello Budapest terület, a sikertelen jellegzetes keresésben tooreturn megegyezik. toohandle talál helyesírási hibát, az intelligens egyeztetésű keresési hello a következő példában bemutatott is használhatja.

**`search=samamish~&queryType=full`**

+ Engedélyezve van az intelligens egyeztetésű keresési, hello megadásakor `~` szimbólumának és hello teljes lekérdezéselemzőben, amely értelmezi, és megfelelően elemez hello használata `~` szintaxist. 

+ Intelligens egyeztetésű keresési akkor használható, ha részt hello teljes lekérdezéselemzőben, amely akkor fordul elő, ha úgy állítja be a `queryType=full`. Hello teljes lekérdezéselemzőben által engedélyezett lekérdezés forgatókönyvekkel kapcsolatos további információkért lásd: [Lucene lekérdezés szintaxisát az Azure Search](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search).

+ Ha `queryType` értéke nincs megadva, alapértelmezett egyszerű lekérdezéselemzőben hello szolgál. hello egyszerű lekérdezéselemzőben gyorsabb, de ha intelligens egyeztetésű keresési, a reguláris kifejezések, közelségi kapcsolat keresési, illetve egyéb speciális lekérdezési van szüksége, szüksége lesz a hello teljes szintaxisát. 

**`search=*&$count=true&$filter=geo.distance(location,geography'POINT(-122.121513 47.673988)') le 5`**

+ Keresztül hello támogatott földrajzi keresés [edm. GeographyPoint adattípus](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) koordináták tartalmazó mező. A geosearch egy szűrőtípus, amelynek meghatározása a [OData-szűrőszintaxis](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) című témakörben olvasható. 

+ hello példalekérdezés szűrők minden eredmény pozicionális adatok, ahol eredményei kisebb, mint 5 kilométerben (szélességi és hosszúsági koordinátákkal megadva) egy adott pontról. Hozzáadásával `$count`, láthatja, hogy hány eredményeinek hello távolság vagy hello koordináták módosításakor. 

+ A térinformatikai keresés hasznos lehet, ha a keresőalkalmazás rendelkezik „keresés a közelben” funkcióval vagy térképes navigációt használ. Ez azonban nem teljes szöveges keresés. Ha felhasználói igényeket a város vagy ország neve alapján keres, adja hozzá a város vagy ország nevét, továbbá toocoordinates tartalmazó mezőket.

## <a name="next-steps"></a>Következő lépések

+ Most létrehozott hello objektumok módosítása Miután egyszer lefuttatta a hello varázsló, visszatérhet és megtekintheti vagy módosíthatja az egyes összetevőket: index, indexelőt vagy adatforrást. Bizonyos szerkesztések, például hello hello mező adattípusának módosítása nem engedélyezett hello index, de a legtöbb tulajdonságok és beállítás módosítható.

  egyes összetevők tooview, kattintson a hello **Index**, **indexelő**, vagy **adatforrások** tartalmazó csempék éppen úgy az irányítópult toodisplay a meglévő objektumok listája. toolearn, amelyekhez nem szükséges egy rebuild index módosításokat kapcsolatos további információkért lásd: [Index frissítése (Azure Search REST API)](https://docs.microsoft.com/rest/api/searchservice/update-index).

+ Próbálja hello eszközöket és más adatforrásokkal rendelkező lépéseket. hello mintahalmazt `realestate-us-sample`, egy Azure SQL-adatbázis, amely az Azure Search által bejárható származik. Az Azure SQL Database-en kívül az Azure Search be tudja járni és indexre tud következtetni sima adatszerkezetekből az alábbiakban: Azure Table Storage, Blob Storage, Azure-beli virtuális gépen futtatott SQL Server, illetve Azure Cosmos DB. Hello varázsló használható összes ezeket az adatforrásokat. *Indexelő* használatával könnyedén tölthet fel indexeket a kódban.

+ Minden más az indexelő adatforrás leküldési modelljével, ahol a kódot leküldéses értesítések új és megváltozott sorkészletek JSON tooyour index keresztül támogatottak. További információk: [Dokumentumok hozzáadása, frissítése vagy törlése az Azure Search szolgáltatásban](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents).

A jelen cikkben említett egyéb szolgáltatásokról az alábbi hivatkozások követésével tudhat meg többet:

* [Az indexelők áttekintése](search-indexer-overview.md)
* [Hozzon létre indexet (hello attribútum részletes leírását tartalmazza)](https://docs.microsoft.com/rest/api/searchservice/create-index)
* [Keresési ablak](search-explorer.md)
* [Dokumentumok keresése (a lekérdezési szintaxisra vonatkozó példákat tartalmaz)](https://docs.microsoft.com/rest/api/searchservice/search-documents)


<!--Image references-->
[1]: ./media/search-get-started-portal/tiles-indexers-datasources2.png
[2]: ./media/search-get-started-portal/import-data-cmd2.png
[3]: ./media/search-get-started-portal/realestateindex2.png
[4]: ./media/search-get-started-portal/indexers-inprogress2.png
[5]: ./media/search-get-started-portal/search-explorer-cmd2.png
[6]: ./media/search-get-started-portal/search-explorer-changeindex-se2.png
[7]: ./media/search-get-started-portal/search-explorer-query2.png
[8]: ./media/search-get-started-portal/realestate-indexer2.png
[9]: ./media/search-get-started-portal/import-datasource-sample2.png