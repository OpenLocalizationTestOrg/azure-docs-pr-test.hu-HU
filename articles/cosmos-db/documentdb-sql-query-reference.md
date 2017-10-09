---
cím: aaa "Azure Cosmos DB DocumentDB API: SQL-szintaxis |} Microsoft Docs"Leírás: hello Azure Cosmos DB DocumentDB API SQL lekérdező nyelve a dokumentáció.
szolgáltatások: cosmos-db Szerző: mimig1 manager: jhubbard szerkesztő: mimig documentationcenter: "

MS.AssetId: ms.service: cosmos-db ms.workload:-adatszolgáltatások ms.tgt_pltfrm: na ms.devlang: na ms.topic: hivatkozhat ms.date: 06/13/2017 ms.author: mimig

---

# <a name="azure-cosmos-db-documentdb-api-sql-syntax-reference"></a>Az Azure Cosmos DB DocumentDB API: SQL-szintaxis referencia

hello Azure Cosmos DB DocumentDB API támogatja a dokumentumok lekérdezését a megszokott SQL (Structured Query Language) használatával például nyelvtan hierarchikus JSON-dokumentumok keresztül anélkül, hogy explicit séma vagy a másodlagos indexek létrehozását. Ez a témakör a DocumentDB API SQL lekérdező nyelve hello dokumentációját.

A DocumentDB API SQL lekérdező nyelve hello útmutatást lásd: [Azure Cosmos DB DocumentDB API SQL-lekérdezések](documentdb-sql-query.md).  
  
Azt is meghívhatjuk toovisit hello [Tesztlekérdezéseket](http://www.documentdb.com/sql/demo) ahol Azure Cosmos DB próbálja és SQL-lekérdezések futtatása az adatkészletet.  
  
## <a name="select-query"></a>SELECT lekérdezés  
Hello adatbázisból kéri le a JSON-dokumentumokat. Kifejezések kiértékelésével, olyan leképezések szűrés támogatja, és csatlakozik.  hello jelölések hello KIVÁLASZTÓ utasítást leíró megjelennének a hello szintaxis egyezmények szakasz.  
  
**Szintaxis**  
  
```
<select_query> ::=  
SELECT <select_specification>   
    [ FROM <from_specification>]   
    [ WHERE <filter_condition> ]  
    [ ORDER BY <sort_specification> ]  
```  
  
 **Megjegyzések**  
  
 Lásd az alábbi szakaszok további részletek az egyes záradékban:  
  
-   [SELECT záradékban](#bk_select_query)  
  
-   [FROM záradékban](#bk_from_clause)  
  
-   [A WHERE záradék](#bk_where_clause)  
  
-   [ORDER BY záradék](#bk_orderby_clause)  
  
hello záradékok hello SELECT utasítással kell lehetnek rendezve, ahogy fent látható. A hello választható záradékok elhagyható. De választható záradék használata esetén azok kell megjelennie hello megfelelő sorrendben.  
  
**Logikai a feldolgozási sorrendben hello SELECT utasítás**  
  
amely záradékok feldolgozásának hello sorrendjét van:  

1.  [FROM záradékban](#bk_from_clause)  
2.  [A WHERE záradék](#bk_where_clause)  
3.  [ORDER BY záradék](#bk_orderby_clause)  
4.  [SELECT záradékban](#bk_select_query)  

Vegye figyelembe, hogy ez nem azonos a hello hello szintaxisa látható sorrendben. hello rendelési van, úgy, hogy a feldolgozott záradék által bevezetett új szimbólumok láthatóak, és később feldolgozott záradékban használható. Például egy FROM záradékban megadott aliasok érhetők el, ha és SELECT záradékban.  

**Az elválasztó karakterek és megjegyzések**  

Minden szóköztől eltérő karakterek nem idézőjelek közé zárt karakterlánc egy részét vagy idézőjelek között azonosítója nem hello nyelvi szintaxis részét képezik, és figyelmen kívül lesznek hagyva elemzése során.  

hello lekérdezési nyelv támogatja például a T-SQL stílus megjegyzések  

-   SQL-utasításban`-- comment text [newline]`  

Amíg az elválasztó karakterek és a megjegyzések nem bármely többszörösére hello nyelvtanban, használt tooseparate jogkivonatok kell lenniük. Például: `-1e5` számú token, igénybe van`: – 1 e5` mínusz jogkivonat követi 1-es számú azonosítóval e5.  

##  <a name="bk_select_query"></a>SELECT záradékban  
hello záradékok hello SELECT utasítással kell lehetnek rendezve, ahogy fent látható. A hello választható záradékok elhagyható. De választható záradék használata esetén azok kell megjelennie hello megfelelő sorrendben.  

**Szintaxis**  
```  
SELECT <select_specification>  

<select_specification> ::=   
      '*'   
      | <object_property_list>   
      | VALUE <scalar_expression> [[ AS ] value_alias]  
  
<object_property_list> ::=   
{ <scalar_expression> [ [ AS ] property_alias ] } [ ,...n ]  
  
```  
  
 **Argumentumok**  
  
 `<select_specification>`  
  
 Hello eredmény kijelölt tulajdonságok vagy -érték toobe beállítása.  
  
 `'*'`  
  
Meghatározza, hogy olvassa-hello értéket változtatás nélkül. Kifejezetten feldolgozott hello értéke egy objektumot, ha az összes tulajdonság veszi.  
  
 `<object_property_list>`  
  
Felsorolja hello tulajdonságok toobe lekérése. Minden visszaadott értéket fogja hello tulajdonságok lettek megadva objektum.  
  
`VALUE`  
  
Meghatározza, hogy kell-e hello JSON-érték olvasni hello teljes JSON-objektum helyett. Ezzel szemben, `<property_list>` nem törik több sorba tervezett hello érték egy objektumban.  
  
`<scalar_expression>`  
  
Számított hello érték toobe-kifejezés. Lásd: [skaláris kifejezések](#bk_scalar_expressions) című szakaszban talál információt.  
  
**Megjegyzések**  
  
Hello `SELECT *` szintaxis csak akkor érvényes, ha a FROM záradékban deklarálva van: pontosan egy aliast. `SELECT *`Itt egy identitás-leképezés, amely lehet hasznos, ha nincs leképezés van szükség. Válassza ki * csak akkor érvényes, ha a FROM záradékban megadott és bevezetett csak egyetlen bemenetet.  
  
Vegye figyelembe, hogy `SELECT <select_list>` és `SELECT *` "szintaktikai cukor", és azt is megteheti lehetnek a lent látható módon egyszerű SELECT utasítás használatával.  
  
1.  `SELECT * FROM ... AS from_alias ...`  
  
     az egyenértékű:  
  
     `SELECT from_alias FROM ... AS from_alias ...`  
  
2.  `SELECT <expr1> AS p1, <expr2> AS p2,..., <exprN> AS pN [other clauses...]`  
  
     az egyenértékű:  
  
     `SELECT VALUE { p1: <expr1>, p2: <expr2>, ..., pN: <exprN> }[other clauses...]`  
  
**Lásd még:**  
  
[Skaláris kifejezések](#bk_scalar_expressions)  
[SELECT záradékban](#bk_select_query)  
  
##  <a name="bk_from_clause"></a>FROM záradékban  
Megadja a csatlakoztatott adatforrásokat, illetve hello forrás. hello FROM záradék használata nem kötelező. Ha nem a megadott, más záradékok továbbra is hajtható végre, mint ha a FROM záradékban megadott egyetlen dokumentum.  
  
**Szintaxis**  
  
```  
FROM <from_specification>  
  
<from_specification> ::=   
        <from_source> {[ JOIN <from_source>][,...n]}  
  
<from_source> ::=   
          <collection_expression> [[AS] input_alias]  
        | input_alias IN <collection_expression>  
  
<collection_expression> ::=   
        ROOT   
     | collection_name  
     | input_alias  
     | <collection_expression> '.' property_name  
     | <collection_expression> '[' "property_name" | array_index ']'  
```  
  
**Argumentumok**  
  
`<from_source>`  
  
Meghatározza egy adatforrást, vagy az alias nélkül. Ha nincs megadva alias, azt fogja következtethető hello `<collection_expression>` következő szabályok segítségével:  
  
-   Ha hello kifejezés egy Lekérdezésnév, majd a lekérdezésnév használható aliasként.  
  
-   Ha hello kifejezés `<collection_expression>`, akkor property_name, majd property_name használja a rendszer aliasként. Ha hello kifejezés egy Lekérdezésnév, majd a lekérdezésnév használható aliasként.  
  
MIVEL`input_alias`  
  
Meghatározza, hogy hello `input_alias` hello az alapul szolgáló gyűjtemény kifejezés által visszaadott értékek készlete.  
 
`input_alias`IN  
  
Meghatározza, hogy hello `input_alias` értékeket a tömb összes tagjának minden hello az alapul szolgáló gyűjtemény kifejezés által visszaadott tömb keresztül léptetés hello készletét képviseli. Minden alapul szolgáló gyűjtemény-kifejezés nem tömb által visszaadott értéket a rendszer figyelmen kívül hagyja.  
  
`<collection_expression>`  
  
Megadja a hello gyűjtemény kifejezés toobe használt tooretrieve hello dokumentumok.  
  
`ROOT`  
  
Meghatározza, hogy a dokumentum hello alapértelmezés szerint a jelenleg csatlakoztatott gyűjtemény lehet beolvasni.  
  
`collection_name`  
  
Meghatározza, hogy a dokumentum be kell olvasni a megadott hello gyűjteményből. hello gyűjtemény hello nevének meg kell egyeznie a jelenleg csatlakoztatott hello gyűjtemény hello nevét.  
  
`input_alias`  
  
Meghatározza, hogy a dokumentum olvassa be a hello határozzák meg a megadott hello alias más forrásból.  
  
`<collection_expression> '.' property_`  
  
Megadja, hogy a dokumentum be kell olvasni hello elérésével `property_name` tulajdonság vagy tömbindex tömbelem által beolvasott minden dokumentumhoz megadott gyűjtemény kifejezés.  
  
`<collection_expression> '[' "property_name" | array_index ']'`  
  
Megadja, hogy a dokumentum be kell olvasni hello elérésével `property_name` tulajdonság vagy tömbindex tömbelem által beolvasott minden dokumentumhoz megadott gyűjtemény kifejezés.  
  
**Megjegyzések**  
  
Összes alias megadott vagy hello következtetni `<from_source>(`s) egyedinek kell lennie. hello szintaxis `<collection_expression>.`property_name van hello azonos `<collection_expression>' ['"property_name"']'`. Azonban ez utóbbi szintaxis hello használható, ha az egyik tulajdonságnév nem azonosító karakternél.  
  
**Hiányzó tulajdonságok hiányzik a tömb elemei nem definiált értékek kezelése**  
  
Ha egy gyűjtemény kifejezés fér hozzá a Tulajdonságok vagy tömb elemeinek és, hogy az érték nem létezik, ezt az értéket figyelmen kívül hagyva, és további nincs feldolgozva.  
  
**Gyűjtemény kifejezés környezetben hatókörének beállítása**  
  
Egy gyűjtemény kifejezés gyűjtemény hatóköre vagy dokumentum-hatóköre lehet:  
  
-   Kifejezés a gyűjtemény hatóköre, ha a hello hello gyűjtemény kifejezés alapul szolgáló adatforrás vagy a gyökér vagy `collection_name`. Ilyen kifejezés hello gyűjtemény közvetlenül lekért dokumentumok készletét reprezentálja, és nincs gyűjtemény kifejezésekre hello feldolgozási függ.  
  
-   Egy kifejezés dokumentum hatókörű, ha hello hello gyűjtemény kifejezés alapul szolgáló adatforrás `input_alias` korábban bemutatott hello lekérdezés. Ilyen kifejezés kiértékelése hello gyűjtemény kifejezés hello hatókörébe tartozó hello aliasnevet gyűjteményhez társított toohello beállítása a dokumentumot kapott dokumentumot jelöli.  hello eredő egy révén az egyes az alapul szolgáló set hello hello dokumentumainak hello gyűjtemény kifejezés kiértékelése készlet unióját lesz.  
  
**Illesztése**  
  
Hello jelenlegi kiadásban Azure Cosmos DB belső illesztések támogatja. További illesztési képességek érkeznek.

A belső illesztések eredménye egy teljes határokon termékben hello beállítása hello illesztési részt. az N-módon illesztési hello eredményét olyan N-elem listának, ahol a hello rekordban szereplő értékekhez hello aliasnevet részt vevő beállított hello illesztés hozzárendelve, és ez az alias más záradékban Vezérlőpultjának érhető el.  
  
hello illesztési hello értékelése hello környezet hatókörét hello beállítása résztvevő függ:  
  
-  Illesztés közötti gyűjtemény- és a gyűjtemény hatóköre a B kiszolgálóra, egy határokon termékben eredményeit A és b készlet összes elemének beállítása
  
-   Készlet és B, set dokumentum hatókörbe tartozó illesztést egy dokumentum hatókörű set nyilvántartott egyes dokumentumok, a B beállítása A. kiértékelésével kapott összes készlet unióját eredményez.  
  
 Hello jelenlegi kiadásban egy gyűjtemény hatókörbe tartozó kifejezés maximum hello lekérdezésfeldolgozó által támogatott.  
  
**Illesztések példái:**  
  
Nézzük hello FROM záradék a következő:`<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`  
  
 Így minden forrás megadása `input_alias1, input_alias2, …, input_aliasN`. A FROM záradék N-listának (rekordot N értékekkel) adja vissza. A táblakonstruktor minden rekordjának összes gyűjtemény alias léptetés alatt az megfelelő készletek által visszaadott érték tartozik.  
  
*CSATLAKOZZON például 1, 2 forrásokat:*  
  
- Így `<from_source1>` kell gyűjtemény-hatókörű, és megfelelnek csoportjának {A, B, C}.  
  
- Így `<from_source2>` dokumentum hatókörű input_alias1 hivatkozó és képviselő beállítása:  
  
    {1, 2} számára`input_alias1 = A,`  
  
    a {3}`input_alias1 = B,`  
  
    {4, 5} számára`input_alias1 = C,`  
  
- hello FROM záradékban `<from_source1> JOIN <from_source2>` rekordokat a következő hello eredményezi:  
  
    (`input_alias1, input_alias2`):  
  
    `(A, 1), (A, 2), (B, 3), (C, 4), (C, 5)`  
  
*CSATLAKOZZON például 2, 3 forrásokat:*  
  
- Így `<from_source1>` kell gyűjtemény-hatókörű, és megfelelnek csoportjának {A, B, C}.  
  
- Lehetővé teszik `<from_source2>` kell a dokumentum-hatókörű hivatkozó `input_alias1` készletek tartalmazzák:  
  
    {1, 2} számára`input_alias1 = A,`  
  
    a {3}`input_alias1 = B,`  
  
    {4, 5} számára`input_alias1 = C,`  
  
- Lehetővé teszik `<from_source3>` kell a dokumentum-hatókörű hivatkozó `input_alias2` készletek tartalmazzák:  
  
    {100, 200} számára`input_alias2 = 1,`  
  
    a {300}`input_alias2 = 3,`  
  
- hello FROM záradékban `<from_source1> JOIN <from_source2> JOIN <from_source3>` rekordokat a következő hello eredményezi:  
  
    (input_alias1, input_alias2, input_alias3):  
  
    (A, 1, 100), (A, 1, 200-AS), (B, 3, 300)  
  
> [!NOTE]
> Más értékek a rekordokat hiánya `input_alias1`, `input_alias2`, mely hello a `<from_source3>` nem adott vissza semmilyen értéket.  
  
*KAPCSOLÓDÁS a példa 3, 3 forrásokat:*  
  
- < From_source1 > kell a gyűjtemény hatóköre, és megfelelnek {A, B, C} csoportjának segítségével.  
  
- Így `<from_source1>` kell gyűjtemény-hatókörű, és megfelelnek csoportjának {A, B, C}.  
  
- < From_source2 > hivatkozó input_alias1 dokumentum hatókörű, és készletek képviselő segítségével:  
  
    {1, 2} számára`input_alias1 = A,`  
  
    a {3}`input_alias1 = B,`  
  
    {4, 5} számára`input_alias1 = C,`  
  
- Lehetővé teszik `<from_source3>` túl tartozni`input_alias1` készletek tartalmazzák:  
  
    {100, 200} számára`input_alias2 = A,`  
  
    a {300}`input_alias2 = C,`  
  
- hello FROM záradékban `<from_source1> JOIN <from_source2> JOIN <from_source3>` rekordokat a következő hello eredményezi:  
  
    (`input_alias1, input_alias2, input_alias3`):  
  
    (A, 1, 100), (A, 1, 200-AS) (A, 2, 100), (A, 2, 200-AS), C, 4, 300, (C, 5, 300)  
  
> [!NOTE]
> Ennek következtében határokon termék közötti `<from_source2>` és `<from_source3>` mert mindkettő hatókörön belüli toohello azonos `<from_source1>`.  Ez 4 (2 x 2) eredményezett a érték 0 rekordokat B (1 x 0) értékkel rendelkező rekordokat és (2 x 1) 2 c-értékkel rekordokat  
  
**Lásd még:**  
  
 [SELECT záradékban](#bk_select_query)  
  
##  <a name="bk_where_clause"></a>A WHERE záradék  
 Adja meg a keresési feltétel hello hello lekérdezés által visszaadott hello dokumentumok.  
  
 **Szintaxis**  
  
```  
WHERE <filter_condition>  
<filter_condition> ::= <scalar_expression>  
  
```  
  
 **Argumentumok**  
  
-   `<filter_condition>`  
  
     Megadja a hello feltétel toobe teljesülniük hello dokumentumok toobe adott vissza.  
  
-   `<scalar_expression>`  
  
     Számított hello érték toobe-kifejezés. Lásd: hello [skaláris kifejezések](#bk_scalar_expressions) című szakaszban talál információt.  
  
 **Megjegyzések**  
  
 Ahhoz, hogy hello dokumentum toobe megadott szűrési feltételt ki kell értékelnie minden tootrue kifejezést adott vissza. Csak az IGAZ logikai értéket eleget tesz a hello feltétel, semmilyen más érték: nincs megadva, null, hamis, számot, tömb vagy objektum nem kielégítéséhez hello feltétel.  
  
##  <a name="bk_orderby_clause"></a>ORDER BY záradék  
 Megadja a rendezési sorrend a hello lekérdezés által visszaadott eredmény hello.  
  
 **Szintaxis**  
  
```  
ORDER BY <sort_specification>  
<sort_specification> ::= <sort_expression> [, <sort_expression>]  
<sort_expression> ::= <scalar_expression> [ASC | DESC]  
  
```  
  
 **Argumentumok**  
  
-   `<sort_specification>`  
  
     Megadja azt a tulajdonságot vagy a kifejezés toosort hello lekérdezés eredménye állítsa be. A rendezési oszlop neve vagy oszlop aliasként adható meg.  
  
     Több rendezési oszlop adható meg. Nevének egyedinek kell lennie. hello oszlopok rendezése hello ORDER BY záradékban hello sorozatát hello szervezet rendezve hello eredményhalmaz határozza meg. Ez azt jelenti, hogy hello eredménykészlet hello első tulajdonság szerint van rendezve, és ezután a rendezett lista van rendezve, hello második tulajdonság, és így tovább.  
  
     hello ORDER BY záradékban hivatkozott hello oszlopnevek hello oszlopát válassza ki a lista vagy tooa oszlop a tábla bármely kétértelműséget nélkül hello FROM záradékban megadott tooeither kell megfelelnie.  
  
-   `<sort_expression>`  
  
     Mely toosort hello lekérdezés eredményhalmazából megadja egy egyetlen tulajdonság vagy kifejezés.  
  
-   `<scalar_expression>`  
  
     Lásd: hello [skaláris kifejezések](#bk_scalar_expressions) című szakaszban talál információt.  
  
-   `ASC | DESC`  
  
     Meghatározza, hogy hello hello megadott oszlop értékeit rendezni kell növekvő vagy csökkenő sorrendben. ASC értékből hello legkisebb érték toohighest rendezi. DESC rendezi a legmagasabb érték toolowest értékből. ASC hello alapértelmezett rendezési sorrend. Null értékek hello legkisebb lehetséges értékek tekintendők.  
  
 **Megjegyzések**  
  
 Hello lekérdezési szintaxis támogatja több sorrend tulajdonságai, amíg hello Azure Cosmos DB lekérdezés futásidejű támogatja a rendezést csak egyetlen tulajdonság, és csak elleni tulajdonságnevek, azaz nem számított tulajdonságokhoz. Rendezés is szükséges, hogy hello indexelő házirend tartalmazza a tartományindexszel hello tulajdonság és a megadott hello típus, hello maximális pontosság. Tekintse meg a házirend dokumentációjában indexeléshez toohello.  
  
##  <a name="bk_scalar_expressions"></a>Skaláris kifejezések  
 Egy skaláris kifejezés szimbólumok kombinációját, és lehet operátorok kiértékelve tooobtain egyetlen értéket. Egyszerű kifejezések állandók, tulajdonsághivatkozást, tömb hivatkozásokkal, alias hivatkozik, vagy lehet függvényhívásokat. Egyszerű kifejezések összetett kifejezések operátorok használatával való egyesíthetők.  
  
 További részletek a értékek melyik skaláris kifejezést lehet: [állandók](#bk_constants) szakasz.  
  
 **Szintaxis**  
  
```  
<scalar_expression> ::=  
       <constant>   
     | input_alias   
     | parameter_name  
     | <scalar_expression>.property_name  
     | <scalar_expression>'['"property_name"|array_index']'  
     | unary_operator <scalar_expression>  
     | <scalar_expression> binary_operator <scalar_expression>    
     | <scalar_expression> ? <scalar_expression> : <scalar_expression>  
     | <scalar_function_expression>  
     | <create_object_expression>   
     | <create_array_expression>  
     | (<scalar_expression>)   
  
<scalar_function_expression> ::=  
        'udf.' Udf_scalar_function([<scalar_expression>][,…n])  
        | builtin_scalar_function([<scalar_expression>][,…n])  
  
<create_object_expression> ::=  
   '{' [{property_name | "property_name"} : <scalar_expression>][,…n] '}'  
  
<create_array_expression> ::=  
   '[' [<scalar_expression>][,…n] ']'  
  
```  
  
 **Argumentumok**  
  
-   `<constant>`  
  
     Egy állandó értékét jelöli. Lásd: [állandók](#bk_constants) című szakaszban talál információt.  
  
-   `input_alias`  
  
     Hello által megadott értéket jelöli `input_alias` hello bevezetett `FROM` záradékban.  
    Ez az érték garantáltan toonot kell **nem definiált** –**nem definiált** kimarad a hello bemeneti értékeket.  
  
-   `<scalar_expression>.property_name`  
  
     Egy objektum hello tulajdonság értékét jelöli. Ha hello tulajdonság nem létezik vagy tulajdonság hivatkozik egy értéket, amely nem egy objektumot, majd hello kifejezés értéke túl**nem definiált** érték.  
  
-   `<scalar_expression>'['"property_name"|array_index']'`  
  
     Hello tulajdonság neve képviseli `property_name` vagy az index tömbelem `array_index` objektum vagy tömb. Ha hello/tulajdonságtömb-index nem létezik, vagy egy érték, amely nem objektum vagy tömb hivatkozott hello/tulajdonságtömb-index, hello kifejezés tooundefined érték értékelődik ki.  
  
-   `unary_operator <scalar_expression>`  
  
     Egy operátort alkalmazott tooa által képviselt egyetlen érték. Lásd: [operátorok](#bk_operators) című szakaszban talál információt.  
  
-   `<scalar_expression> binary_operator <scalar_expression>`  
  
     Alkalmazott tootwo értékek operátor jelöli. Lásd: [operátorok](#bk_operators) című szakaszban talál információt.  
  
-   `<scalar_function_expression>`  
  
     Egy adott hívás eredménye által megadott értéket jelöli.  
  
-   `udf_scalar_function`  
  
     Hello felhasználó neve meghatározott skaláris függvényt.  
  
-   `builtin_scalar_function`  
  
     Hello beépített skaláris függvény neve.  
  
-   `<create_object_expression>`  
  
     Által megadott tulajdonságokkal rendelkező új objektum létrehozása és azok értékei jelöli.  
  
-   `<create_array_expression>`  
  
     Hozzon létre egy új tömb elemként megadott értékekkel kapott értéket jelöli.  
  
-   `parameter_name`  
  
     A megadott paraméternév hello értékét jelöli. Paraméterek nevei rendelkeznie kell egyetlen @ hello első karakter.  
  
 **Megjegyzések**  
  
 Amikor egy beépített vagy felhasználói függvény skaláris összes argumentumot meg kell határozni. Ha bármely hello argumentuma nincs meghatározva, hello függvény nem hívható, és hello lesz: nincs megadva.  
  
 Egy objektum létrehozásakor bármely tulajdonság nem definiált érték hozzárendelt kihagyja és hello létrehozott objektum nem tartalmazza.  
  
 Ha egy tömb, bármely elemérték létrehozható, amely hozzá van rendelve **nem definiált** kihagyja és hello létrehozott objektum nem szerepel érték. Ennek hatására mellett definiált hello elem tootake helyére oly módon, hogy létre hello tömb fog nem a kihagyott indexeket.  
  
##  <a name="bk_operators"></a>Operátorok  
 Ez a szakasz ismerteti a támogatott hello operátorok. Minden egyes operátor lehet hozzárendelt tooexactly egy kategóriát.  
  
 Lásd: **operátor kategóriák** részletes, az alábbi táblázatban kezelésére vonatkozó **nem definiált** érték található, a bemeneti értékek és kezelésére vonatkozó értékek nem egyező típusok szemben támasztott követelményeit.  
  
 **Operátor kategóriák:**  
  
|**Kategória**|**Részletek**|  
|-|-|  
|**aritmetikai**|Operátor vár input(s) toobe száma. Kimeneti az a szám. Ha hello bemenetek bármelyike **nem definiált** vagy típustól eltérő, számot, majd hello eredmény **nem definiált**.|  
|**Bitenkénti**|Operátor vár input(s) toobe 32 bites előjeles egész számokat száma. Kimeneti is 32 bites, előjeles egész szám.<br /><br /> Nem egész értéket a rendszer kerekíti. Pozitív értéket lefelé, negatív értékeket kerekíti.<br /><br /> Bármely érték, amely hello 32 bites egész tartományon kívül esik a két tartozó hexadecimális utolsó 32-bites megtételével konvertálja.<br /><br /> Ha hello bemenetek bármelyike **nem definiált** vagy adjon meg másik számot, majd hello eredmény **nem definiált**.<br /><br /> **Megjegyzés:** fent viselkedés hello összeegyeztethető JavaScript bitenkénti operátor viselkedését.|  
|**logikai**|Operátor vár input(s) toobe Boolean(s). Kimeneti is olyan logikai érték.<br />Ha hello bemenetek bármelyike **nem definiált** vagy adjon meg eltérő logikai érték, akkor rendszer hello eredmény **nem definiált**.|  
|**összehasonlítása**|Operátor vár input(s) toohave hello azonos írja be, és nem lehet nem definiált. Olyan logikai érték eredménye.<br /><br /> Ha hello bemenetek bármelyike **nem definiált** vagy hello bemenetek különböző rendelkezik, majd hello eredmény **nem definiált**.<br /><br /> Lásd: **összehasonlított értékek rendezési** tábla értékhez rendelés részleteit.|  
|**karakterlánc**|Operátor vár input(s) toobe karakterlánc(ok). Kimenet: karakterlánc.<br />Ha hello bemenetek bármelyike **nem definiált** vagy típustól eltérő karakterláncot, majd hello eredmény **nem definiált**.|  
  
 **Az egyoperandusú operátorral:**  
  
|**Name (Név)**|**Operátor**|**Részletek**|  
|-|-|-|  
|**aritmetikai**|+<br /><br /> -|Hello számú értéket ad vissza.<br /><br /> Bitenkénti negálást. Számú értéket ad vissza negated.|  
|**Bitenkénti**|~|Egyesek komplemens számnak. Az érték egy szám kiegészítése adja vissza.|  
|**Logikai**|**NEM**|Tagadásának. Beolvasása negated logikai érték.|  
  
 **Bináris operátor:**  
  
|**Name (Név)**|**Operátor**|**Részletek**|  
|-|-|-|  
|**aritmetikai**|+<br /><br /> -<br /><br /> *<br /><br /> /<br /><br /> %|Hozzáadását.<br /><br /> Kivonás.<br /><br /> Szorzást végezhet.<br /><br /> Osztás.<br /><br /> Modulációs.|  
|**Bitenkénti**|&#124;<br /><br /> &<br /><br /> ^<br /><br /> <<<br /><br /> >><br /><br /> >>>|Bitenkénti vagy.<br /><br /> Bitenkénti és művelet<br /><br /> Bitenkénti kizáró vagy.<br /><br /> Balra Tolást.<br /><br /> Jobbra Tolást.<br /><br /> Nulla-Kitöltés jobbra Tolást.|  
|**logikai**|**ÉS**<br /><br /> **VAGY**|Logikai együtt. Visszaadja **igaz** , ha mindkét argumentuma **igaz**, adja vissza **hamis** ellenkező esetben.<br /><br /> Logikai együtt. Visszaadja **igaz** , ha mindkét argumentuma **igaz**, adja vissza **hamis** ellenkező esetben.|  
|**összehasonlítása**|**=**<br /><br /> **!=, <>**<br /><br /> **>**<br /><br /> **>=**<br /><br /> **<**<br /><br /> **<=**<br /><br /> **??**|Egyenlő. Visszaadja **igaz** Ha az argumentum értéke, akkor adja vissza **hamis** egyéb.<br /><br /> Nem egyenlő. Visszaadja **igaz** Ha az argumentum nem egyenlő, adja vissza **hamis** egyéb.<br /><br /> Nagyobb, mint. Beolvasása **igaz** első argumentum értéke nagyobb, mint a második hello, ha vissza **hamis** más módon.<br /><br /> Nagyobb vagy egyenlő. Beolvasása **igaz** első argumentum nagyobb vagy egyenlő toohello másikat, ha vissza **hamis** más módon.<br /><br /> Kisebb, mint. Beolvasása **igaz** első argumentum értéke kisebb, mint a második hello, ha vissza **hamis** más módon.<br /><br /> Kisebb vagy egyenlő, mint. Beolvasása **igaz** első argumentum értéke kisebb vagy egyenlő, mint ha toohello második egy visszatérési **hamis** más módon.<br /><br /> A Coalesce. Értéket ad vissza hello második argumentum kötelező, ha hello első argumentum egy **nem definiált** érték.|  
|**Karakterlánc**|**&#124;&#124;**|Kapott. Mindkét argumentumot összefűzése adja vissza.|  
  
 **Ternáris kezelők:**  
  
|Ternáris operátor|?|Értéket ad vissza hello a második argumentum kötelező, ha hello első argumentum értéke túl**igaz**; ellenkező esetben vissza hello harmadik argumentum.|  
|-|-|-|  
  
 **Az összehasonlított értékek sorrendje**  
  
|**Típus**|**Értékek sorrendje**|  
|-|-|  
|**Nincs definiálva**|Nem hasonlítható össze.|  
|**NULL értékű**|Egyetlen érték: **null értékű**|  
|**Szám**|Természetes valós szám.<br /><br /> Negatív végtelen értéke kisebb, mint bármely más számértéket.<br /><br /> Pozitív végtelen értéke nagyobb, mint bármely más számértéket. **NaN** értéke nem hasonlítható össze. Összehasonlítva az **NaN** eredményez **nem definiált** érték.|  
|**Karakterlánc**|Lexicographical sorrendje.|  
|**A tömb**|Nincs rendezést, de egyenlő.|  
|**Objektum**|Nincs rendezést, de egyenlő.|  
  
 **Megjegyzések**  
  
 Az Azure Cosmos Adatbázisba hello típusú értékeket gyakran nem ismert lekéréséig azokat ténylegesen hello adatbázisból. A sorrend toosupport hatékony végrehajtásának lekérdezések hello operátorok többsége a szigorú szemben támasztott követelményeit. Operátorok önmagában is ne hajtsa végre implicit konverzió.  
  
 Ez azt jelenti, hogy a lekérdezés, például: válasszon * a ROOT r WHERE r.Age = 21 csak tulajdonság kora egyenlő toohello szám 21-dokumentumok adja vissza. A tulajdonság kora egyenlő toohello karakterlánc "21" vagy "0021" hello karakterlánc nem fog egyezni, "21" hello kifejezésként = 21 tooundefined értékelődik ki. Ez lehetővé teszi a hatékonyabb felhasználása indexek, mert hello keresés egy adott érték (azaz számú 21) gyorsabb, mint lehetséges megegyezik (21 szám vagy karakterlánc "21", "021", "21.0"...) határozatlan számú keresése. Ez eltér hogyan értékeli ki a JavaScript a felügyelői eltérő típusú értékeket.  
  
 **Tömbök és objektumok egyenlőség és összehasonlítása**  
  
 A tartomány operátorral egymáshoz csatolt tömb vagy objektum értékek összehasonlításával (>, > =, <, < =) eredményez, mivel nem az objektum és tömb meghatározásának sorrendje nincs definiálva. Azonban a egyenlőség operátorral egymáshoz csatolt (=,! =, <>) támogatott és értékek szerkezetileg össze.  
  
 Tömbök azonosak, ha mindkét értéktömbök azonos számú elemből állnak, és megfelelő pozíciók elemekben is egyenlő. Ha nincs megadva, hello eredménye array összehasonlítása bármely két elem összehasonlításával eredményez nincs definiálva.  
  
 Objektumok azonosak, ha mindkét objektum definiált azonos jellemzőkkel rendelkezik, és a megfelelő tulajdonságainak értékei is egyenlő. Ha bármely pár tulajdonságérték összehasonlításával objektum összehasonlítás eredménye nem definiált, hello nincs definiálva.  
  
##  <a name="bk_constants"></a>Állandók  
 Egy konstans, más néven szövegkonstans vagy skaláris, egy meghatározott értéket jelölő szimbólumot. egy konstans hello formátuma hello adattípus hello érték azt jelenti, hogy függ.  
  
 **Skaláris adattípusokat támogatja:**  
  
|**Típus**|**Értékek sorrendje**|  
|-|-|  
|**Nincs definiálva**|Egyetlen érték: **nincs megadva**|  
|**NULL értékű**|Egyetlen érték: **null értékű**|  
|**Logikai érték**|Értékek: **hamis**, **igaz**.|  
|**Szám**|Egy kétszeres pontosságú lebegőpontos számnál, szabványos IEEE 754.|  
|**Karakterlánc**|Nulla vagy több Unicode-karaktereket sorozata. Karakterláncok egyetlen vagy dupla idézőjelek között kell foglalni.|  
|**A tömb**|Nulla vagy több elemek sorrendjét. Minden elem meghatározatlan kivételével minden skaláris adattípusú érték lehet.|  
|**Objektum**|Egy nulla vagy több név/érték párok rendezetlen készlete. Értéke a Unicode-karakterláncot, kivéve értéke lehet bármely skaláris adattípusú, **meghatározatlan**.|  
  
 **Szintaxis**  
  
```  
<constant> ::=  
   <undefined_constant>  
     | <null_constant>   
     | <boolean_constant>   
     | <number_constant>   
     | <string_constant>   
     | <array_constant>   
     | <object_constant>   
  
<undefined_constant> ::= undefined  
  
<null_constant> ::= null  
  
<boolean_constant> ::= false | true  
  
<number_constant> ::= decimal_literal | hexadecimal_literal  
  
<string_constant> ::= string_literal  
  
<array_constant> ::=  
    '[' [<constant>][,...n] ']'  
  
<object_constant> ::=   
   '{' [{property_name | "property_name"} : <constant>][,...n] '}'  
  
```  
  
 **Argumentumok**  
  
1.  `<undefined_constant>; undefined`  
  
     Meghatározatlan típusú érték nincs definiálva jelöli.  
  
2.  `<null_constant>; null`  
  
     Jelöli **null** típusú érték **Null**.  
  
3.  `<boolean_constant>`  
  
     Logikai érték típusú konstans jelöli.  
  
4.  `false`  
  
     Jelöli **hamis** logikai típusú érték.  
  
5.  `true`  
  
     Jelöli **igaz** logikai típusú érték.  
  
6.  `<number_constant>`  
  
     Egy konstans jelöli.  
  
7.  `decimal_literal`  
  
     Decimális literálok képviselt decimális jelöléssel, vagy tudományos jelölés használatával számok.  
  
8.  `hexadecimal_literal`  
  
     Hexadecimális literálok értékét "0 x", egy vagy több hexadecimális számjegy követ előtag számok.  
  
9. `<string_constant>`  
  
     Egy karakterlánc típusú konstans jelöli.  
  
10. `string _literal`  
  
     A szövegkonstansok olyan Unicode karakterláncok sorozatát nulla vagy több Unicode-karaktereket vagy escape-karaktersorozatokat. A szövegkonstansok vannak szimpla zárójelek között (aposztróf: ") vagy dupla idézőjel (idézőjel:").  
  
 Következő escape-karaktersorozatokat engedélyezettek:  
  
|**Escape-karaktersorozatot**|**Leírás**|**Unicode-karakter**|  
|-|-|-|  
|\\'|aposztróf (')|U + 0027|  
|\\"|az idézőjel (")|U + 0022|  
|\\\|fordított solidus (\\)|U + 005C|  
|\\/|solidus (/)|U + 002F|  
|\b|BACKSPACE|U + 0008|  
|\f|Lapdobás|U + 000C|  
|\n|soremelés|U + 000A|  
|\r|kocsivissza|U + 000D|  
|\t|Lap|U + 0009|  
|\uXXXX|4 hexadecimális számjegy által megadott Unicode-karakter.|U + XXXX|  
  
##  <a name="bk_query_perf_guidelines"></a>Lekérdezés teljesítmény irányelvek  
 Ahhoz, hogy a lekérdezés toobe egy nagy méretű gyűjtemény hatékonyan hajtotta végre akkor az szűrőket, amelyek egy vagy több indexek keresztül szolgáltatható kell használhat.  
  
 a következő szűrők hello veszi figyelembe a keresési index:  
  
-   Egyenlő operátor (=) használata a dokumentum az elérésiút-kifejezés és egy konstans.  
  
-   Tartomány operátorokkal (<, \<=, >, > =) számú állandók és elérésiút-kifejezésben.  
  
-   A dokumentum az elérésiút-kifejezés bármely kifejezés végzi: azonosítja a hivatkozott hello adatbázis gyűjteményből hello dokumentumok állandó elérési útját jelöli.  
  
 **A dokumentum az elérésiút-kifejezés**  
  
 Dokumentum elérési kifejezések kifejezések szerepelnek, amelyek egy elérési utat az indexelő tulajdonság vagy tömb vizsgáztatók egy dokumentumot, az adatbázis gyűjtemény dokumentumok érkező keresztül. Az elérési út lehet közvetlenül a hello dokumentumok hello adatbázis gyűjtemény szűrőben hivatkozott értékek használt tooidentify hello helyét.  
  
 Egy kifejezés toobe figyelembe veendő elérésiút-kifejezésben, a következőket:  
  
1.  Hivatkozás hello gyűjtemény legfelső szintű közvetlenül.  
  
2.  Hivatkozási tulajdonság vagy állandó tömb indexelő néhány dokumentum elérési út kifejezés  
  
3.  Az alias, amely az egyes dokumentum elérésiút-kifejezés hivatkozik.  
  
     **Szintaxis konvenciók**  
  
     a következő táblázat hello hello jelölések toodescribe szintaxist hello DocumentDB API lekérdezés nyelvi dokumentáció ismerteti.  
  
    |**Egyezmény**|**A használt**|  
    |-|-|    
    |NAGYBETŰK|Nem betűérzékeny kulcsszavakat.|  
    |kisbetűk|Kis-és nagybetűket kulcsszavakat.|  
    |\<nonterminal >|Nem, külön definiált.|  
    |\<nonterminal >:: =|Hello nonterminal szintaxis meghatározása.|  
    |other_terminal|Terminálszolgáltatások (jogkivonat) részletesen szavakat.|  
    |Azonosítója|Azonosítója. Lehetővé teszi, hogy a következő karaktereket csak: a – z A – Z 0 – 9 _First karakter nem lehet egy számjegy.|  
    |"karakterlánc"|Idézőjelek közé zárt karakterlánc. Lehetővé teszi, hogy minden érvényes karakterláncot. Tekintse meg a string_literal leírása.|  
    |"szimbólum"|Hello szintaxis részét képező literális szimbólum.|  
    |&#124; (a függőleges vonal)|Alternatívák szintaxis elemekhez. Csak megadott hello elemek egyikét használhatja.|  
    |[] /(brackets)|Zárójelek közé egy vagy több választható elemek.|  
    |[,.. .n]|Azt jelzi, hogy megelőző elem hello lehet ismételt n számú alkalommal. hello előfordulások vesszővel kell elválasztani.|  
    |[.. .n]|Azt jelzi, hogy megelőző elem hello lehet ismételt n számú alkalommal. hello előfordulások üres cellákat el egymástól.|  
  
##  <a name="bk_built_in_functions"></a>Beépített funkciók  
 Azure Cosmos-adatbázis SQL számos beépített funkciót biztosít. beépített függvények hello kategóriák listája látható.  
  
|Függvény|Leírás|  
|--------------|-----------------|  
|[Matematikai funkciók](#bk_mathematical_functions)|hello matematikai funkciók minden hajtsa végre a számítás, rendszerint bemeneti értékeket, mint szerepkör argumentumokban szolgálnak, és a visszaadandó numerikus érték alapján.|  
|[Írja be az ellenőrzési funkciók](#bk_type_checking_functions)|hello típus ellenőrzési funkciók lehetővé teszik az SQL-lekérdezések lévő kifejezés toocheck hello típusú.|  
|[Karakterlánc-függvények](#bk_string_functions)|hello karakterlánc funkciók végrehajtania egy műveletet a bemeneti karakterlánc-értékkel, és a karakterlánc, a numerikus és logikai értéket adja vissza.|  
|[A tömb funkciók](#bk_array_functions)|hello tömb funkciók egy logikai érték vagy tömb érték, egy tömb bemeneti érték és a numerikus visszatérési művelet végrehajtása.|  
|[Térbeli funkciók](#bk_spatial_functions)|hello térbeli funkciók végrehajtania egy műveletet a olyan térbeli objektum beviteli értéket, és numerikus vagy logikai érték visszaadása.|  
  
###  <a name="bk_mathematical_functions"></a>Matematikai funkciók  
 hello alábbi funkciók számítás elvégzése, általában a bemeneti értékeket, mint szerepkör argumentumokban szolgálnak, és a visszaadandó numerikus érték alapján.  
  
||||  
|-|-|-|  
|[ABS](#bk_abs)|[ARCCOS](#bk_acos)|[ARCSIN](#bk_asin)|  
|[ATAN](#bk_atan)|[ATN2](#bk_atn2)|[FELSŐ HATÁR](#bk_ceiling)|  
|[COS](#bk_cos)|[TŰZ](#bk_cot)|[FOK](#bk_degrees)|  
|[EXP](#bk_exp)|[EMELET](#bk_floor)|[NAPLÓ](#bk_log)|  
|[LOG10](#bk_log10)|[A PI](#bk_pi)|[ENERGIAGAZDÁLKODÁSI](#bk_power)|  
|[RADIÁNBAN MEGADOTT SZÖG](#bk_radians)|[CIKLIKUS](#bk_round)|[EG](#bk_sin)|  
|[SQRT](#bk_sqrt)|[NÉGYZETES](#bk_square)|[BEJELENTKEZÉS](#bk_sign)|  
|[TAN](#bk_tan)|[CSONK](#bk_trunc)||  
  
####  <a name="bk_abs"></a>ABS  
 Értéket ad vissza hello abszolút (pozitív) hello a megadott numerikus kifejezés.  
  
 **Szintaxis**  
  
```  
ABS (<numeric_expression>)  
```  
  
 **Argumentumok**  
  
-   `numeric_expression`  
  
     Van egy numerikus kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy numerikus kifejezést ad vissza.  
  
 **Példák**  
  
 hello következő példában három eltérő számú hello ABS funkcióval hello eredményeit.  
  
```  
SELECT ABS(-1), ABS(0), ABS(1)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{$1: 1, $2: 0, $3: 1}]  
```  
  
####  <a name="bk_acos"></a>ARCCOS  
 Hello szög értéket ad vissza, az radiánban megadott szög, amelynek koszinusza hello megadott numerikus kifejezés; más néven koszinuszát.  
  
 **Szintaxis**  
  
```  
ACOS(<numeric_expression>)  
```  
  
 **Argumentumok**  
  
-   `numeric_expression`  
  
     Van egy numerikus kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy numerikus kifejezést ad vissza.  
  
 **Példák**  
  
 hello alábbi példa adja vissza a-1 ARCCOS hello.  
  
```  
SELECT ACOS(-1)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": 3.1415926535897931}]  
```  
  
####  <a name="bk_asin"></a>ARCSIN  
 Hello szög értéket ad vissza, az radiánban megadott szög, amelynek szinusza hello megadott numerikus kifejezés. Ez rövidítése szinuszát.  
  
 **Szintaxis**  
  
```  
ASIN(<numeric_expression>)  
```  
  
 **Argumentumok**  
  
-   `numeric_expression`  
  
     Van egy numerikus kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy numerikus kifejezést ad vissza.  
  
 **Példák**  
  
 hello alábbi példa adja vissza a-1 ARCSIN hello.  
  
```  
SELECT ASIN(-1)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": -1.5707963267948966}]  
```  
  
####  <a name="bk_atan"></a>ATAN  
 Hello szög értéket ad vissza, az radiánban megadott szög, amelynek tangense a hello megadott numerikus kifejezés. Ezt arkusz is nevezik.  
  
 **Szintaxis**  
  
```  
ATAN(<numeric_expression>)  
```  
  
 **Argumentumok**  
  
-   `numeric_expression`  
  
     Van egy numerikus kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy numerikus kifejezést ad vissza.  
  
 **Példák**  
  
 a következő példa értéket ad vissza a hello ATAN hello hello megadott értéket.  
  
```  
SELECT ATAN(-45.01)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": -1.5485826962062663}]  
```  
  
####  <a name="bk_atn2"></a>ATN2  
 Hello arkusz tangens / x, y radiánban kifejezett hello tag értékét adja vissza.  
  
 **Szintaxis**  
  
```  
ATN2(<numeric_expression>, <numeric_expression>)  
```  
  
 **Argumentumok**  
  
-   `numeric_expression`  
  
     Van egy numerikus kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy numerikus kifejezést ad vissza.  
  
 **Példák**  
  
 hello alábbi példa kiszámítja a megadott hello ATN2 hello x és y összetevőket.  
  
```  
SELECT ATN2(35.175643, 129.44)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": 1.3054517947300646}]  
```  
  
####  <a name="bk_ceiling"></a>FELSŐ HATÁR  
 Beolvasása hello legkisebb egész szám nagyobb értékre, vagy egyenlő, hello megadott numerikus kifejezés.  
  
 **Szintaxis**  
  
```  
CEILING (<numeric_expression>)  
```  
  
 **Argumentumok**  
  
-   `numeric_expression`  
  
     Van egy numerikus kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy numerikus kifejezést ad vissza.  
  
 **Példák**  
  
 hello a következő példa bemutatja a pozitív szám, negatív, és nulla érték a felső határ függvény hello.  
  
```  
SELECT CEILING(123.45), CEILING(-123.45), CEILING(0.0)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{$1: 124, $2: -123, $3: 0}]  
```  
  
####  <a name="bk_cos"></a>COS  
 Beolvasása hello trigonometric koszinusza hello radiánban megadott szög, hello a megadott kifejezés.  
  
 **Szintaxis**  
  
```  
COS(<numeric_expression>)  
```  
  
 **Argumentumok**  
  
-   `numeric_expression`  
  
     Van egy numerikus kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy numerikus kifejezést ad vissza.  
  
 **Példák**  
  
 a következő példa hello kiszámítja a hello COS hello a megadott szög.  
  
```  
SELECT COS(14.78)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": -0.59946542619465426}]  
```  
  
####  <a name="bk_cot"></a>TŰZ  
 Beolvasása hello trigonometric kotangensét hello radiánban megadott szög, a hello megadva a numerikus kifejezés.  
  
 **Szintaxis**  
  
```  
COT(<numeric_expression>)  
```  
  
 **Argumentumok**  
  
-   `numeric_expression`  
  
     Van egy numerikus kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy numerikus kifejezést ad vissza.  
  
 **Példák**  
  
 a következő példa hello hello hello megadott szög tűz számítja ki.  
  
```  
SELECT COT(124.1332)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": -0.040311998371148884}]  
```  
  
####  <a name="bk_degrees"></a>FOK  
 Értéket ad vissza megfelelő szög (fokban megadva) az radiánban megadott szög hello.  
  
 **Szintaxis**  
  
```  
DEGREES (<numeric_expression>)  
```  
  
 **Argumentumok**  
  
-   `numeric_expression`  
  
     Van egy numerikus kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy numerikus kifejezést ad vissza.  
  
 **Példák**  
  
 hello alábbi példa számát adja vissza hello fokban szög radiánban PI/2.  
  
```  
SELECT DEGREES(PI()/2)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": 90}]  
```  
  
####  <a name="bk_floor"></a>EMELET  
 Visszaadja hello legnagyobb egész szám kisebb vagy egyenlő, mint a toohello megadott numerikus kifejezés.  
  
 **Szintaxis**  
  
```  
FLOOR (<numeric_expression>)  
```  
  
 **Argumentumok**  
  
-   `numeric_expression`  
  
     Van egy numerikus kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy numerikus kifejezést ad vissza.  
  
 **Példák**  
  
 hello a következő példa bemutatja a pozitív szám, negatív, és a nulla érték hello EMELET függvény.  
  
```  
SELECT FLOOR(123.45), FLOOR(-123.45), FLOOR(0.0)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{$1: 123, $2: -124, $3: 0}]  
```  
  
####  <a name="bk_exp"></a>EXP  
 Értéket ad vissza hello exponenciális hello a megadott numerikus kifejezés.  
  
 **Szintaxis**  
  
```  
EXP (<numeric_expression>)  
```  
  
 **Argumentumok**  
  
-   `numeric_expression`  
  
     Van egy numerikus kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy numerikus kifejezést ad vissza.  
  
 **Megjegyzések**  
  
 hello állandó **e** (2.718281...), akkor hello természetes logaritmus alapja.  
  
 hello hatványát egy szám hello állandó **e** toohello power hello szám következik be. Például EXP(1.0) = e ^ 1.0 = 2.71828182845905 és EXP(10) = e ^ 10 = 22026.4657948067.  
  
 hello exponenciális hello természetes alapú logaritmus egy szám maga hello szám: EXP (napló (n)) = n. Egy szám exponenciális hello hello természetes alapú logaritmusát pedig magát a hello száma: (EXP (n)) napló = n.  
  
 **Példák**  
  
 hello alábbi példa egy változót deklarál és hello exponenciális hello változóhoz (10) értékét adja vissza.  
  
```  
SELECT EXP(10)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{$1: 22026.465794806718}]  
```  
  
 hello alábbi példa értéket ad vissza hello exponenciális hello természetes alapú logaritmusát 20 hello természetes alapú logaritmusát hello exponenciális 20. Mivel ezek a funkciók egy másik, lebegőpontos szám matematikai mindkét esetben 20 kerekítési visszatérési értékének hello inverz funkcióit.  
  
```  
SELECT EXP(LOG(20)), LOG(EXP(20))  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{$1: 19.999999999999996, $2: 20}]  
```  
  
####  <a name="bk_log"></a>NAPLÓ  
 Beolvasása hello természetes alapú logaritmusát hello megadott numerikus kifejezés.  
  
 **Szintaxis**  
  
```  
LOG (<numeric_expression> [, <base>])  
```  
  
 **Argumentumok**  
  
-   `numeric_expression`  
  
     Van egy numerikus kifejezés.  
  
-   `base`  
  
     Nem kötelező numerikus argumentum hello hello logaritmus alapjának állítja be.  
  
 **Visszatérési típusokat**  
  
 Egy numerikus kifejezést ad vissza.  
  
 **Megjegyzések**  
  
 Alapértelmezés szerint LOG() hello természetes alapú logaritmusát adja vissza. Hello alapját jelentő hello logaritmusát tooanother érték hello választható alap paraméter használatával módosíthatja.  
  
 hello természetes alapú logaritmusát hello logaritmusát toohello alap **e**, ahol **e** egy ésszerűtlen állandó megközelítőleg azonos too2.718281828 van.  
  
 hello természetes alapú logaritmusát hello exponenciális egy szám maga hello száma: (EXP (n)) napló = n. Hello természetes alapú logaritmus egy számot, exponenciális hello pedig magát a hello száma: EXP (napló (n)) = n.  
  
 **Példák**  
  
 hello alábbi példa egy változót deklarál és hello változóhoz (10) értékét hello alapú logaritmusát adja vissza.  
  
```  
SELECT LOG(10)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{$1: 2.3025850929940459}]  
```  
  
 hello alábbi példa kiszámítja hello napló a hello hatványát szám.  
  
```  
SELECT EXP(LOG(10))  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{$1: 10.000000000000002}]  
```  
  
####  <a name="bk_log10"></a>LOG10  
 Beolvasása hello 10-es alapú logaritmusát hello megadott numerikus kifejezés.  
  
 **Szintaxis**  
  
```  
LOG10 (<numeric_expression>)  
```  
  
 **Argumentumok**  
  
-   `numeric_expression`  
  
     Van egy numerikus kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy numerikus kifejezést ad vissza.  
  
 **Megjegyzések**  
  
 hello LOG10 és POWER funkciók intenzitásfokozatok kapcsolódó tooone egy másik. Például 10 ^ LOG10(n) = n.  
  
 **Példák**  
  
 hello alábbi példa egy változót deklarál és hello változóhoz (100) hello LOG10 értékét adja vissza.  
  
```  
SELECT LOG10(100)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{$1: 2}]  
```  
  
####  <a name="bk_pi"></a>A PI  
 Beolvasása hello pi konstans érték.  
  
 **Szintaxis**  
  
```  
PI ()  
```  
  
 **Argumentumok**  
  
-   `numeric_expression`  
  
     Van egy numerikus kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy numerikus kifejezést ad vissza.  
  
 **Példák**  
  
 a következő példa a hello értéket ad vissza a PI hello.  
  
```  
SELECT PI()  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": 3.1415926535897931}]  
```  
  
####  <a name="bk_power"></a>ENERGIAGAZDÁLKODÁSI  
 Értéket ad vissza a megadott hello értékének hello kifejezés toohello megadott power.  
  
 **Szintaxis**  
  
```  
POWER (<numeric_expression>, <y>)  
```  
  
 **Argumentumok**  
  
-   `numeric_expression`  
  
     Van egy numerikus kifejezés.  
  
-   `y`  
  
     Hello power toowhich tooraise van `numeric_expression`.  
  
 **Visszatérési típusokat**  
  
 Egy numerikus kifejezést ad vissza.  
  
 **Példák**  
  
 hello a következő példa bemutatja, 3 (hello adatkocka hello szám) számú toohello hatványa előléptetése.  
  
```  
SELECT POWER(2, 3), POWER(2.5, 3)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{$1: 8, $2: 15.625}]  
```  
  
####  <a name="bk_radians"></a>RADIÁNBAN MEGADOTT SZÖG  
 Vissza a radiánban megadott szög, ha egy numerikus kifejezés fokban, is meg kell adni.  
  
 **Szintaxis**  
  
```  
RADIANS (<numeric_expression>)  
```  
  
 **Argumentumok**  
  
-   `numeric_expression`  
  
     Van egy numerikus kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy numerikus kifejezést ad vissza.  
  
 **Példák**  
  
 hello következő példa néhány szögek fogadja bemeneti adatként, és adja vissza a hozzájuk tartozó radián értékek.  
  
```  
SELECT RADIANS(-45.01), RADIANS(-181.01), RADIANS(0), RADIANS(0.1472738), RADIANS(197.1099392)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{  
       "$1": -0.7855726963226477,  
       "$2": -3.1592204790349356,  
       "$3": 0,  
       "$4": 0.0025704127119236249,  
       "$5": 3.4402174274458375  
   }]  
```  
  
####  <a name="bk_round"></a>CIKLIKUS  
 Egy numerikus érték, kerekített toohello legközelebbi egész értéket ad vissza.  
  
 **Szintaxis**  
  
```  
ROUND(<numeric_expression>)  
```  
  
 **Argumentumok**  
  
-   `numeric_expression`  
  
     Van egy numerikus kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy numerikus kifejezést ad vissza.  
  
 **Példák**  
  
 a következő példa kerekítés hello toohello pozitív és negatív számokat a legközelebbi egész számra a következő hello.  
  
```  
SELECT ROUND(2.4), ROUND(2.6), ROUND(2.5), ROUND(-2.4), ROUND(-2.6)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{$1: 2, $2: 3, $3: 3, $4: -2, $5: -3}]  
```  
  
####  <a name="bk_sign"></a>BEJELENTKEZÉS  
 Beolvasása hello pozitív (+ 1), nulla (0), vagy a hello mínuszjel (-1) megadott numerikus kifejezés.  
  
 **Szintaxis**  
  
```  
SIGN(<numeric_expression>)  
```  
  
 **Argumentumok**  
  
-   `numeric_expression`  
  
     Van egy numerikus kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy numerikus kifejezést ad vissza.  
  
 **Példák**  
  
 hello alábbi példa vissza hello bejelentkezési értékek számának-2 too2.  
  
```  
SELECT SIGN(-2), SIGN(-1), SIGN(0), SIGN(1), SIGN(2)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{$1: -1, $2: -1, $3: 0, $4: 1, $5: 1}]  
```  
  
####  <a name="bk_sin"></a>EG  
 Beolvasása hello trigonometric szinusza hello radiánban megadott szög, hello a megadott kifejezés.  
  
 **Szintaxis**  
  
```  
SIN(<numeric_expression>)  
```  
  
 **Argumentumok**  
  
-   `numeric_expression`  
  
     Van egy numerikus kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy numerikus kifejezést ad vissza.  
  
 **Példák**  
  
 a következő példa hello hello megadott szög Szinusz hello számítja ki.  
  
```  
SELECT SIN(45.175643)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": 0.929607286611012}]  
```  
  
####  <a name="bk_sqrt"></a>SQRT  
 Hello négyzetgyökét adja vissza a hello megadott numerikus érték.  
  
 **Szintaxis**  
  
```  
SQRT(<numeric_expression>)  
```  
  
 **Argumentumok**  
  
-   `numeric_expression`  
  
     Van egy numerikus kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy numerikus kifejezést ad vissza.  
  
 **Példák**  
  
 hello alábbi példa adja vissza hello szögletes gyökerek számok 1-3.  
  
```  
SELECT SQRT(1), SQRT(2.0), SQRT(3)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{$1: 1, $2: 1.4142135623730952, $3: 1.7320508075688772}]  
```  
  
####  <a name="bk_square"></a>NÉGYZETES  
 Beolvasása hello négyzetes hello a megadott numerikus érték.  
  
 **Szintaxis**  
  
```  
SQUARE(<numeric_expression>)  
```  
  
 **Argumentumok**  
  
-   `numeric_expression`  
  
     Van egy numerikus kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy numerikus kifejezést ad vissza.  
  
 **Példák**  
  
 hello alábbi példa adja vissza hello négyzetes számok 1-3.  
  
```  
SELECT SQUARE(1), SQUARE(2.0), SQUARE(3)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{$1: 1, $2: 4, $3: 9}]  
```  
  
####  <a name="bk_tan"></a>TAN  
 Hello hello tangensét adja vissza radiánban megadott szög, hello a megadott kifejezés.  
  
 **Szintaxis**  
  
```  
TAN (<numeric_expression>)  
```  
  
 **Argumentumok**  
  
-   `numeric_expression`  
  
     Van egy numerikus kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy numerikus kifejezést ad vissza.  
  
 **Példák**  
  
 hello alábbi példa kiszámítja PI () hello tangensét / 2.  
  
```  
SELECT TAN(PI()/2);  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": 16331239353195370 }]  
```  
  
####  <a name="bk_trunc"></a>CSONK  
 Egy numerikus érték, csonkolt toohello legközelebbi egész értéket ad vissza.  
  
 **Szintaxis**  
  
```  
TRUNC(<numeric_expression>)  
```  
  
 **Argumentumok**  
  
-   `numeric_expression`  
  
     Van egy numerikus kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy numerikus kifejezést ad vissza.  
  
 **Példák**  
  
 a következő példa hello csonkolja a következő egész számot a legközelebbi pozitív és negatív számok toohello hello.  
  
```  
SELECT TRUNC(2.4), TRUNC(2.6), TRUNC(2.5), TRUNC(-2.4), TRUNC(-2.6)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{$1: 2, $2: 2, $3: 2, $4: -2, $5: -2}]  
```  
  
###  <a name="bk_type_checking_functions"></a>Írja be az ellenőrzési funkciók  
 hello következő funkciókat támogatja az típusú bemeneti értékekkel ellenőrzése, és minden egyes logikai értéket adja vissza.  
  
||||  
|-|-|-|  
|[IS_ARRAY](#bk_is_array)|[IS_BOOL](#bk_is_bool)|[IS_DEFINED](#bk_is_defined)|  
|[IS_NULL](#bk_is_null)|[IS_NUMBER](#bk_is_number)|[IS_OBJECT](#bk_is_object)|  
|[IS_PRIMITIVE](#bk_is_primitive)|[IS_STRING](#bk_is_string)||  
  
####  <a name="bk_is_array"></a>IS_ARRAY  
 Adja vissza, ha hello hello típusú megadott kifejezés jelző logikai érték egy tömb.  
  
 **Szintaxis**  
  
```  
IS_ARRAY(<expression>)  
```  
  
 **Argumentumok**  
  
-   `expression`  
  
     Ez bármilyen érvényes kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy logikai kifejezést ad vissza.  
  
 **Példák**  
  
 hello alábbi példa ellenőrzi objektumok JSON logikai, számot, NULL értékű karakterlánc, objektum, a tömb és hello IS_ARRAY függvény használata nem definiált típusok.  
  
```  
SELECT   
 IS_ARRAY(true),   
 IS_ARRAY(1),  
 IS_ARRAY("value"),  
 IS_ARRAY(null),  
 IS_ARRAY({prop: "value"}),   
 IS_ARRAY([1, 2, 3]),  
 IS_ARRAY({prop: "value"}.prop2)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{$1: false, $2: false, $3: false, $4: false, $5: false, $6: true}]  
```  
  
####  <a name="bk_is_bool"></a>IS_BOOL  
 Adja vissza egy logikai érték, amely jelzi, ha hello hello típusú megadott kifejezés olyan logikai érték.  
  
 **Szintaxis**  
  
```  
IS_BOOL(<expression>)  
```  
  
 **Argumentumok**  
  
-   `expression`  
  
     Ez bármilyen érvényes kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy logikai kifejezést ad vissza.  
  
 **Példák**  
  
 hello alábbi példa ellenőrzi objektumok JSON logikai, számot, NULL értékű karakterlánc, objektum, a tömb és hello IS_BOOL függvény használata nem definiált típusok.  
  
```  
SELECT   
    IS_BOOL(true),   
    IS_BOOL(1),  
    IS_BOOL("value"),   
    IS_BOOL(null),  
    IS_BOOL({prop: "value"}),   
    IS_BOOL([1, 2, 3]),  
    IS_BOOL({prop: "value"}.prop2)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{$1: true, $2: false, $3: false, $4: false, $5: false, $6: false}]  
```  
  
####  <a name="bk_is_defined"></a>IS_DEFINED  
 Jelzi, ha hello tulajdonság van rendelve egy érték logikai érték beolvasása.  
  
 **Szintaxis**  
  
```  
IS_DEFINED(<expression>)  
```  
  
 **Argumentumok**  
  
-   `expression`  
  
     Ez bármilyen érvényes kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy logikai kifejezést ad vissza.  
  
 **Példák**  
  
 a következő példa keres belül hello tulajdonság hello jelenléte hello megadott JSON-dokumentum. hello első igaz értéket ad vissza, mert "a" jelen, de hello második hamis értéket ad vissza, mert hiányzik a "b".  
  
```  
SELECT IS_DEFINED({ "a" : 5 }.a), IS_DEFINED({ "a" : 5 }.b)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{  
       "$1": true,    
       "$2": false   
   }]  
```  
  
####  <a name="bk_is_null"></a>IS_NULL  
 Visszaadja egy logikai érték, amely jelzi, ha hello hello típusú megadott kifejezés értéke null.  
  
 **Szintaxis**  
  
```  
IS_NULL(<expression>)  
```  
  
 **Argumentumok**  
  
-   `expression`  
  
     Ez bármilyen érvényes kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy logikai kifejezést ad vissza.  
  
 **Példák**  
  
 hello alábbi példa ellenőrzi objektumok JSON logikai, számot, NULL értékű karakterlánc, objektum, a tömb és hello IS_NULL függvény használata nem definiált típusok.  
  
```  
SELECT   
    IS_NULL(true),   
    IS_NULL(1),  
    IS_NULL("value"),   
    IS_NULL(null),  
    IS_NULL({prop: "value"}),   
    IS_NULL([1, 2, 3]),  
    IS_NULL({prop: "value"}.prop2)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{$1: false, $2: false, $3: false, $4: true, $5: false, $6: false}]  
```  
  
####  <a name="bk_is_number"></a>IS_NUMBER  
 Adja vissza, ha hello hello típusú megadott kifejezés jelző logikai érték egy szám.  
  
 **Szintaxis**  
  
```  
IS_NUMBER(<expression>)  
```  
  
 **Argumentumok**  
  
-   `expression`  
  
     Ez bármilyen érvényes kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy logikai kifejezést ad vissza.  
  
 **Példák**  
  
 hello alábbi példa ellenőrzi objektumok JSON logikai, számot, NULL értékű karakterlánc, objektum, a tömb és hello IS_NULL függvény használata nem definiált típusok.  
  
```  
SELECT   
    IS_NUMBER(true),   
    IS_NUMBER(1),  
    IS_NUMBER("value"),   
    IS_NUMBER(null),  
    IS_NUMBER({prop: "value"}),   
    IS_NUMBER([1, 2, 3]),  
    IS_NUMBER({prop: "value"}.prop2)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{$1: false, $2: true, $3: false, $4: false, $5: false, $6: false}]  
```  
  
####  <a name="bk_is_object"></a>IS_OBJECT  
 Adja vissza egy logikai érték, amely jelzi, ha hello hello típusú megadott kifejezés egy JSON-objektum.  
  
 **Szintaxis**  
  
```  
IS_OBJECT(<expression>)  
```  
  
 **Argumentumok**  
  
-   `expression`  
  
     Ez bármilyen érvényes kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy logikai kifejezést ad vissza.  
  
 **Példák**  
  
 hello alábbi példa ellenőrzi objektumok JSON logikai, számot, NULL értékű karakterlánc, objektum, a tömb és hello IS_OBJECT függvény használata nem definiált típusok.  
  
```  
SELECT   
    IS_OBJECT(true),   
    IS_OBJECT(1),  
    IS_OBJECT("value"),   
    IS_OBJECT(null),  
    IS_OBJECT({prop: "value"}),   
    IS_OBJECT([1, 2, 3]),  
    IS_OBJECT({prop: "value"}.prop2)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{$1: false, $2: false, $3: false, $4: false, $5: true, $6: false}]  
```  
  
####  <a name="bk_is_primitive"></a>IS_PRIMITIVE  
 Ha hello hello megadva kifejezés egy egyszerű jelző logikai érték beolvasása (string, Boolean, numerikus vagy null értékű).  
  
 **Szintaxis**  
  
```  
IS_PRIMITIVE(<expression>)  
```  
  
 **Argumentumok**  
  
-   `expression`  
  
     Ez bármilyen érvényes kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy logikai kifejezést ad vissza.  
  
 **Példák**  
  
 hello alábbi példa ellenőrzi objektumok JSON logikai, számot, NULL értékű karakterlánc, objektum, a tömb és hello IS_PRIMITIVE függvény használata nem definiált típusok.  
  
```  
SELECT   
           IS_PRIMITIVE(true),   
           IS_PRIMITIVE(1),  
           IS_PRIMITIVE("value"),   
           IS_PRIMITIVE(null),  
           IS_PRIMITIVE({prop: "value"}),   
           IS_PRIMITIVE([1, 2, 3]),  
           IS_PRIMITIVE({prop: "value"}.prop2)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": true, "$2": true, "$3": true, "$4": true, "$5": false, "$6": false, "$7": false}]  
```  
  
####  <a name="bk_is_string"></a>IS_STRING  
 Adja vissza, ha hello hello típusú megadott kifejezés jelző logikai érték: karakterlánc.  
  
 **Szintaxis**  
  
```  
IS_STRING(<expression>)  
```  
  
 **Argumentumok**  
  
-   `expression`  
  
     Ez bármilyen érvényes kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy logikai kifejezést ad vissza.  
  
 **Példák**  
  
 hello alábbi példa ellenőrzi objektumok JSON logikai, számot, NULL értékű karakterlánc, objektum, a tömb és hello IS_STRING függvény használata nem definiált típusok.  
  
```  
SELECT   
       IS_STRING(true),   
       IS_STRING(1),  
       IS_STRING("value"),   
       IS_STRING(null),  
       IS_STRING({prop: "value"}),   
       IS_STRING([1, 2, 3]),  
       IS_STRING({prop: "value"}.prop2)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{$1: false, $2: false, $3: true, $4: false, $5: false, $6: false}]  
```  
  
###  <a name="bk_string_functions"></a>Karakterlánc  
 hello következő skaláris függvények végrehajtania egy műveletet a bemeneti karakterlánc-érték és a karakterlánc, a numerikus és logikai értéket adja vissza.  
  
||||  
|-|-|-|  
|[CONCAT](#bk_concat)|[TARTALMAZZA](#bk_contains)|[MEGADOTT MÓDON VÉGZŐDŐ](#bk_endswith)|  
|[INDEX_OF](#bk_index_of)|[BALRA](#bk_left)|[HOSSZA](#bk_length)|  
|[ALACSONYABB](#bk_lower)|[LTRIM](#bk_ltrim)|[CSERÉLJE LE](#bk_replace)|  
|[REPLIKÁLÁS](#bk_replicate)|[FORDÍTOTT](#bk_reverse)|[JOBBRA](#bk_right)|  
|[RTRIM](#bk_rtrim)|[STARTSWITH ELEMNEK](#bk_startswith)|[SUBSTRING](#bk_substring)|  
|[FELSŐ](#bk_upper)|||  
  
####  <a name="bk_concat"></a>CONCAT  
 Egy karakterlánc, amely legalább két karakterlánc-értékek hozzáfűzésével hello eredményét adja vissza.  
  
 **Szintaxis**  
  
```  
CONCAT(<str_expr>, <str_expr> [, <str_expr>])  
```  
  
 **Argumentumok**  
  
-   `str_expr`  
  
     Van bármilyen érvényes karakterlánc-kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy karakterlánc-kifejezés adja vissza.  
  
 **Példák**  
  
 a következő példa beolvasása összefűzendő hello karakterlánc hello hello megadott értéket.  
  
```  
SELECT CONCAT("abc", "def")  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": "abcdef"}  
```  
  
####  <a name="bk_contains"></a>TARTALMAZZA  
 Hogy hello első karakterlánc-kifejezés második tartalmaz-e hello jelző logikai érték beolvasása.  
  
 **Szintaxis**  
  
```  
CONTAINS(<str_expr>, <str_expr>)  
```  
  
 **Argumentumok**  
  
-   `str_expr`  
  
     Van bármilyen érvényes karakterlánc-kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy logikai kifejezést ad vissza.  
  
 **Példák**  
  
 hello alábbi példa ellenőrzi, ha az "abc" tartalmazza az "ab", és tartalmazza a "d".  
  
```  
SELECT CONTAINS("abc", "ab"), CONTAINS("abc", "d")  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": true, "$2": false}]  
```  
  
####  <a name="bk_endswith"></a>MEGADOTT MÓDON VÉGZŐDŐ  
 E hello első karaktersorozat végződik hello második jelző logikai érték beolvasása.  
  
 **Szintaxis**  
  
```  
ENDSWITH(<str_expr>, <str_expr>)  
```  
  
 **Argumentumok**  
  
-   `str_expr`  
  
     Van bármilyen érvényes karakterlánc-kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy logikai kifejezést ad vissza.  
  
 **Példák**  
  
 hello következő példa eredménye "abc" karakterlánccal végződik-e a "b" és "bc" hello.  
  
```  
SELECT ENDSWITH("abc", "b"), ENDSWITH("abc", "bc")  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": false, "$2": true}]  
```  
  
####  <a name="bk_index_of"></a>INDEX_OF  
 Hello hello második karakterlánc-kifejezés hello első megadott karakterlánc-kifejezés vagy-1 érték első előfordulásának pozícióját a indítása, ha hello karakterlánc nem található hello adja vissza.  
  
 **Szintaxis**  
  
```  
INDEX_OF(<str_expr>, <str_expr>)  
```  
  
 **Argumentumok**  
  
-   `str_expr`  
  
     Van bármilyen érvényes karakterlánc-kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy numerikus kifejezést ad vissza.  
  
 **Példák**  
  
 hello alábbi példa adja vissza "abc" belül különböző karakterláncrészletek hello indexét.  
  
```  
SELECT INDEX_OF("abc", "ab"), INDEX_OF("abc", "b"), INDEX_OF("abc", "c")  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": 0, "$2": 1, "$3": -1}]  
```  
  
####  <a name="bk_left"></a>BALRA  
 Értéket ad vissza egy karakterlánc bal oldalának hello megadott hello a karakterek száma.  
  
 **Szintaxis**  
  
```  
LEFT(<str_expr>, <num_expr>)  
```  
  
 **Argumentumok**  
  
-   `str_expr`  
  
     Van bármilyen érvényes karakterlánc-kifejezés.  
  
-   `num_expr`  
  
     Ez bármilyen érvényes numerikus kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy karakterlánc-kifejezés adja vissza.  
  
 **Példák**  
  
 hello alábbi példa adja vissza hello különböző hosszúságú értékek az "abc" része maradt.  
  
```  
SELECT LEFT("abc", 1), LEFT("abc", 2)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": "ab", "$2": "ab"}]  
```  
  
####  <a name="bk_length"></a>HOSSZA  
 A megadott karakterlánc-kifejezés hello karakterét hello számát adja vissza.  
  
 **Szintaxis**  
  
```  
LENGTH(<str_expr>)  
```  
  
 **Argumentumok**  
  
-   `str_expr`  
  
     Van bármilyen érvényes karakterlánc-kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy karakterlánc-kifejezés adja vissza.  
  
 **Példák**  
  
 hello alábbi példa adja vissza a karakterlánc hossza hello.  
  
```  
SELECT LENGTH("abc")  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": 3}]  
```  
  
####  <a name="bk_lower"></a>ALACSONYABB  
 Egy karakterlánc-kifejezés nagybetűt adatok toolowercase átalakítása után adja vissza.  
  
 **Szintaxis**  
  
```  
LOWER(<str_expr>)  
```  
  
 **Argumentumok**  
  
-   `str_expr`  
  
     Van bármilyen érvényes karakterlánc-kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy karakterlánc-kifejezés adja vissza.  
  
 **Példák**  
  
 a következő példa azt mutatja meg hogyan hello toouse ALACSONYABB a lekérdezésben.  
  
```  
SELECT LOWER("Abc")  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": "abc"}]  
  
```  
  
####  <a name="bk_ltrim"></a>LTRIM  
 Egy karakterlánc-kifejezés adja vissza, után eltávolítja a kezdő üres.  
  
 **Szintaxis**  
  
```  
LTRIM(<str_expr>)  
```  
  
 **Argumentumok**  
  
-   `str_expr`  
  
     Van bármilyen érvényes karakterlánc-kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy karakterlánc-kifejezés adja vissza.  
  
 **Példák**  
  
 a következő példa azt mutatja meg hogyan hello toouse LTRIM egy lekérdezésben.  
  
```  
SELECT LTRIM("  abc"), LTRIM("abc"), LTRIM("abc   ")  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": "abc", "$2": "abc", "$3": "abc   "}]  
```  
  
####  <a name="bk_replace"></a>CSERÉLJE LE  
 Megadott karakterlánc-érték összes előfordulását lecseréli egy másik karakterlánc.  
  
 **Szintaxis**  
  
```  
REPLACE(<str_expr>, <str_expr>, <str_expr>)  
```  
  
 **Argumentumok**  
  
-   `str_expr`  
  
     Van bármilyen érvényes karakterlánc-kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy karakterlánc-kifejezés adja vissza.  
  
 **Példák**  
  
 hello a következő példa bemutatja, hogyan toouse cserélje le a lekérdezésben.  
  
```  
SELECT REPLACE("This is a Test", "Test", "desk")  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": "This is a desk"}]  
```  
  
####  <a name="bk_replicate"></a>REPLIKÁLÁS  
 A megadott számú alkalommal megismétel egy karakterlánc-érték.  
  
 **Szintaxis**  
  
```  
REPLICATE(<str_expr>, <num_expr>)  
```  
  
 **Argumentumok**  
  
-   `str_expr`  
  
     Van bármilyen érvényes karakterlánc-kifejezés.  
  
-   `num_expr`  
  
     Ez bármilyen érvényes numerikus kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy karakterlánc-kifejezés adja vissza.  
  
 **Példák**  
  
 hello a következő példa bemutatja, hogyan toouse REPLIKÁLNI a lekérdezésben.  
  
```  
SELECT REPLICATE("a", 3)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": "aaa"}]  
```  
  
####  <a name="bk_reverse"></a>FORDÍTOTT  
 Hello fordított sorrendben egy karakterlánc értékét adja vissza.  
  
 **Szintaxis**  
  
```  
REVERSE(<str_expr>)  
```  
  
 **Argumentumok**  
  
-   `str_expr`  
  
     Van bármilyen érvényes karakterlánc-kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy karakterlánc-kifejezés adja vissza.  
  
 **Példák**  
  
 hello a következő példa bemutatja, hogyan toouse fordított a lekérdezésben.  
  
```  
SELECT REVERSE("Abc")  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": "cbA"}]  
```  
  
####  <a name="bk_right"></a>JOBBRA  
 A hello jobb része egy karakterlánc beolvasása hello megadott karakterek száma.  
  
 **Szintaxis**  
  
```  
RIGHT(<str_expr>, <num_expr>)  
```  
  
 **Argumentumok**  
  
-   `str_expr`  
  
     Van bármilyen érvényes karakterlánc-kifejezés.  
  
-   `num_expr`  
  
     Ez bármilyen érvényes numerikus kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy karakterlánc-kifejezés adja vissza.  
  
 **Példák**  
  
 hello alábbi példa részét adja vissza hello jobbra "abc" különböző hosszúságú értékek.  
  
```  
SELECT RIGHT("abc", 1), RIGHT("abc", 2)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": "c", "$2": "bc"}]  
```  
  
####  <a name="bk_rtrim"></a>RTRIM  
 Egy karakterlánc-kifejezés adja vissza, miután eltávolítja a záró szóközöket.  
  
 **Szintaxis**  
  
```  
RTRIM(<str_expr>)  
```  
  
 **Argumentumok**  
  
-   `str_expr`  
  
     Van bármilyen érvényes karakterlánc-kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy karakterlánc-kifejezés adja vissza.  
  
 **Példák**  
  
 a következő példa azt mutatja meg hogyan hello toouse RTRIM egy lekérdezésben.  
  
```  
SELECT RTRIM("  abc"), RTRIM("abc"), RTRIM("abc   ")  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": "   abc", "$2": "abc", "$3": "abc"}]  
```  
  
####  <a name="bk_startswith"></a>STARTSWITH ELEMNEK  
 Hogy hello első karakterlánc-kifejezés indításakor hello második jelző logikai érték beolvasása.  
  
 **Szintaxis**  
  
```  
STARTSWITH(<str_expr>, <str_expr>)  
```  
  
 **Argumentumok**  
  
-   `str_expr`  
  
     Van bármilyen érvényes karakterlánc-kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy logikai kifejezést ad vissza.  
  
 **Példák**  
  
 hello alábbi példa ellenőrzések Ha hello "abc" karakterlánc első lépése a "b" és "a".  
  
```  
SELECT STARTSWITH("abc", "b"), STARTSWITH("abc", "a")  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": false, "$2": true}]  
```  
  
####  <a name="bk_substring"></a>SUBSTRING  
 Hello kezdődő karakterlánc-kifejezés részét adja vissza a megadott karakter nulláról indulva számolt helyzetét, és folytatja a toohello megadott hosszúság vagy hello karakterlánc toohello végét.  
  
 **Szintaxis**  
  
```  
SUBSTRING(<str_expr>, <num_expr> [, <num_expr>])  
```  
  
 **Argumentumok**  
  
-   `str_expr`  
  
     Van bármilyen érvényes karakterlánc-kifejezés.  
  
-   `num_expr`  
  
     Ez bármilyen érvényes numerikus kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy karakterlánc-kifejezés adja vissza.  
  
 **Példák**  
  
 hello alábbi példa részét adja vissza hello "abc" kezdődően: 1 és 1 karakter hosszúságú.  
  
```  
SELECT SUBSTRING("abc", 1, 1)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": "b"}]  
```  
  
####  <a name="bk_upper"></a>FELSŐ  
 Egy karakterlánc-kifejezés kisbetűt adatok toouppercase átalakítása után adja vissza.  
  
 **Szintaxis**  
  
```  
UPPER(<str_expr>)  
```  
  
 **Argumentumok**  
  
-   `str_expr`  
  
     Van bármilyen érvényes karakterlánc-kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy karakterlánc-kifejezés adja vissza.  
  
 **Példák**  
  
 a következő példa azt mutatja meg hogyan hello toouse felső lekérdezés  
  
```  
SELECT UPPER("Abc")  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": "ABC"}]  
```  
  
###  <a name="bk_array_functions"></a>A tömb funkciók  
 a következő skaláris függvények hello végrehajtania egy műveletet a egy tömb bemeneti érték és a numerikus visszatérési, a logikai érték vagy tömb érték  
  
||||  
|-|-|-|  
|[ARRAY_CONCAT](#bk_array_concat)|[ARRAY_CONTAINS](#bk_array_contains)|[ARRAY_LENGTH](#bk_array_length)|  
|[ARRAY_SLICE](#bk_array_slice)|||  
  
####  <a name="bk_array_concat"></a>ARRAY_CONCAT  
 Egy tömb, amely két vagy több tömb értékek hozzáfűzésével hello eredményét adja vissza.  
  
 **Szintaxis**  
  
```  
ARRAY_CONCAT (<arr_expr>, <arr_expr> [, <arr_expr>])  
```  
  
 **Argumentumok**  
  
-   `arr_expr`  
  
     Van bármilyen érvényes tömböt megadó kifejezést.  
  
 **Visszatérési típusokat**  
  
 Egy tömböt megadó kifejezést ad vissza.  
  
 **Példák**  
  
 hello következő hogyan két tooconcatenate tömbállandó példa.  
  
```  
SELECT ARRAY_CONCAT(["apples", "strawberries"], ["bananas"])  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": ["apples", "strawberries", "bananas"]}]  
```  
  
####  <a name="bk_array_contains"></a>ARRAY_CONTAINS  
 Vissza logikai érték, amely azt jelzi, hogy tartalmaz-e hello hello tömb a megadott érték.  
  
 **Szintaxis**  
  
```  
ARRAY_CONTAINS (<arr_expr>, <expr>)  
```  
  
 **Argumentumok**  
  
-   `arr_expr`  
  
     Van bármilyen érvényes tömböt megadó kifejezést.  
  
-   `expr`  
  
     Ez bármilyen érvényes kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy logikai értéket ad vissza.  
  
 **Példák**  
  
 a következő példa hogyan hello toocheck tagság használatával ARRAY_CONTAINS tömbben.  
  
```  
SELECT   
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "apples"),  
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "mangoes")  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": true, "$2": false}]  
```  
  
####  <a name="bk_array_length"></a>ARRAY_LENGTH  
 Hello elemeinek hello számát adja vissza a megadott tömb kifejezés.  
  
 **Szintaxis**  
  
```  
ARRAY_LENGTH(<arr_expr>)  
```  
  
 **Argumentumok**  
  
-   `arr_expr`  
  
     Van bármilyen érvényes tömböt megadó kifejezést.  
  
 **Visszatérési típusokat**  
  
 Egy numerikus kifejezést ad vissza.  
  
 **Példák**  
  
 hello következő példa hogyan tooget hello segítségével ARRAY_LENGTH tömb hossza.  
  
```  
SELECT ARRAY_LENGTH(["apples", "strawberries", "bananas"])  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{"$1": 3}]  
```  
  
####  <a name="bk_array_slice"></a>ARRAY_SLICE  
 Egy tömböt megadó kifejezést részét adja vissza.
  
 **Szintaxis**  
  
```  
ARRAY_SLICE (<arr_expr>, <num_expr> [, <num_expr>])  
```  
  
 **Argumentumok**  
  
-   `arr_expr`  
  
     Van bármilyen érvényes tömböt megadó kifejezést.  
  
-   `num_expr`  
  
     Ez bármilyen érvényes numerikus kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy logikai értéket ad vissza.  
  
 **Példák**  
  
 a következő példa hogyan hello tooget ARRAY_SLICE használatával tömb részét.  
  
```  
SELECT   
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1),  
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1)  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{  
           "$1": ["strawberries", "bananas"],   
           "$2": ["strawberries"]  
       }]  
```  
  
###  <a name="bk_spatial_functions"></a>Térbeli funkciók  
 hello következő skaláris függvények végrehajtania egy műveletet egy térbeli objektum bemeneti érték a és numerikus vagy logikai érték visszaadása.  
  
||||  
|-|-|-|  
|[ST_DISTANCE](#bk_st_distance)|[ST_WITHIN](#bk_st_within)|[ST_INTERSECTS](#bk_st_intersects)|[ST_ISVALID](#bk_st_isvalid)|  
|[ST_ISVALIDDETAILED](#bk_st_isvaliddetailed)|||  
  
####  <a name="bk_st_distance"></a>ST_DISTANCE  
 A két hello GeoJSON-pont, sokszög vagy LineString kifejezések hello távolság visszaadása.  
  
 **Szintaxis**  
  
```  
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argumentumok**  
  
-   `spatial_expr`  
  
     Ez az egyetlen érvényes GeoJSON-pont, sokszög vagy LineString objektum kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy numerikus kifejezés tartalmazó hello távolságot adja vissza. Ez a mérőszámok hello alapértelmezett referenciarendszer szerint megadva.  
  
 **Példák**  
  
 a következő példa azt mutatja meg hogyan hello tooreturn 30 km-hello belüli összes családba tartozó dokumentumok megadott hely hello ST_DISTANCE beépített függvény használatával. .  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{  
  "id": "WakefieldFamily"  
}]  
```  
  
####  <a name="bk_st_within"></a>ST_WITHIN  
 Visszaadja a egy logikai kifejezés, amely jelzi, hogy hello GeoJSON objektum (pont, Polygon, vagy LineString) hello első argumentumban megadott hello GeoJSON (pont, sokszög vagy LineString) belül hello második argumentum.  
  
 **Szintaxis**  
  
```  
ST_WITHIN (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argumentumok**  
  
-   `spatial_expr`  
  
     Ez az egyetlen érvényes GeoJSON-pont, sokszög vagy LineString objektum kifejezés.  
 
-   `spatial_expr`  
  
     Ez az egyetlen érvényes GeoJSON-pont, sokszög vagy LineString objektum kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy logikai értéket ad vissza.  
  
 **Példák**  
  
 hello a következő példa bemutatja, hogyan toofind összes termékcsalád dokumentumok használatával ST_WITHIN sokszög belül.  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_WITHIN(f.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{ "id": "WakefieldFamily" }]  
```  

####  <a name="bk_st_intersects"></a>ST_INTERSECTS  
 Egy logikai kifejezés, amely azt jelzi, hogy hello GeoJSON objektum (pont, Polygon, vagy LineString) hello első argumentumban megadott metszi hello második argumentum (pont, sokszög vagy LineString) GeoJSON hello adja vissza.  
  
 **Szintaxis**  
  
```  
ST_INTERSECTS (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argumentumok**  
  
-   `spatial_expr`  
  
     Ez az egyetlen érvényes GeoJSON-pont, sokszög vagy LineString objektum kifejezés.  
 
-   `spatial_expr`  
  
     Ez az egyetlen érvényes GeoJSON-pont, sokszög vagy LineString objektum kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy logikai értéket ad vissza.  
  
 **Példák**  
  
 a következő példa azt mutatja meg hogyan hello minden területet engedélyez, amelyek a metszi hello megadott sokszög toofind.  
  
```  
SELECT a.id   
FROM Areas a   
WHERE ST_INTERSECTS(a.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{ "id": "IntersectingPolygon" }]  
```  
  
####  <a name="bk_st_isvalid"></a>ST_ISVALID  
 Jelzi, hogy hello megadott GeoJSON-pont, sokszög vagy LineString kifejezés érvényes logikai érték beolvasása.  
  
 **Szintaxis**  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 **Argumentumok**  
  
-   `spatial_expr`  
  
     Ez az egyetlen érvényes GeoJSON-pont, sokszög vagy LineString kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy logikai kifejezést ad vissza.  
  
 **Példák**  
  
 a következő példa azt mutatja meg hogyan hello toocheck, ha a pont ST_VALID használatával.  
  
 Például ezt a pontot a szélességi értékkel rendelkezik, amely a értékkel [-90 és 90] hello érvényes tartományban van, ezért hello lekérdezés hamis értéket adja vissza.  
  
 Sokszögek hello specifikációja megköveteli, hogy legyen-e hello utolsó koordináta pár megadott GeoJSON hello ugyanaz, mint hello első, toocreate zárt alakzat. Egy sokszögön belül pontok balra érdekében meg kell adni. A sokszög jobbra sorrendben megadva hello régióját, hello inverzét jelöli.  
  
```  
SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{ "$1": false }]  
```  
  
####  <a name="bk_st_isvaliddetailed"></a>ST_ISVALIDDETAILED  
 Értéket ad eredményül, ha hello megadott GeoJSON-pont, sokszög vagy LineString kifejezés logikai értéket tartalmazó JSON-érték érvényes, és ha érvénytelen, továbbá hello OK karakterláncként.  
  
 **Szintaxis**  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 **Argumentumok**  
  
-   `spatial_expr`  
  
     Ez az egyetlen érvényes GeoJSON pontot vagy a sokszög kifejezés.  
  
 **Visszatérési típusokat**  
  
 Egy logikai értéket tartalmazó hello GeoJSON-pont vagy sokszög kifejezés érvényes, és ha érvénytelen, továbbá hello karakterláncként OK JSON értéket ad vissza.  
  
 **Példák**  
  
 a következő példa hogyan hello toocheck érvényességi (adatokkal) ST_ISVALIDDETAILED használatával.  
  
```  
SELECT ST_ISVALIDDETAILED({   
  "type": "Polygon",   
  "coordinates": [[ [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] ]]  
})  
```  
  
 Itt található hello eredményhalmaz.  
  
```  
[{  
  "$1": {   
    "valid": false,   
    "reason": "hello Polygon input is not valid because hello start and end points of hello ring number 1 are not hello same. Each ring of a polygon must have hello same start and end points."   
  }  
}]  
```  
  
## <a name="next-steps"></a>Következő lépések  
 [SQL-szintaxis és Azure Cosmos adatbázis SQL-lekérdezés](documentdb-sql-query.md)   
 [Az Azure Cosmos DB dokumentációja](https://docs.microsoft.com/en-us/azure/cosmos-db/)  
  
  
