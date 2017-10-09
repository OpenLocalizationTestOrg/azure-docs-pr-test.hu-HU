---
<span data-ttu-id="aacdd-101">cím: aaa "Azure Cosmos DB DocumentDB API: SQL-szintaxis |} Microsoft Docs"Leírás: hello Azure Cosmos DB DocumentDB API SQL lekérdező nyelve a dokumentáció.</span><span class="sxs-lookup"><span data-stu-id="aacdd-101">title: aaa"Azure Cosmos DB DocumentDB API: SQL syntax | Microsoft Docs" description: Reference documentation for hello Azure Cosmos DB DocumentDB API SQL query language.</span></span>
<span data-ttu-id="aacdd-102">szolgáltatások: cosmos-db Szerző: mimig1 manager: jhubbard szerkesztő: mimig documentationcenter: "</span><span class="sxs-lookup"><span data-stu-id="aacdd-102">services: cosmos-db author: mimig1 manager: jhubbard editor: mimig documentationcenter: ''</span></span>

<span data-ttu-id="aacdd-103">MS.AssetId: ms.service: cosmos-db ms.workload:-adatszolgáltatások ms.tgt_pltfrm: na ms.devlang: na ms.topic: hivatkozhat ms.date: 06/13/2017 ms.author: mimig</span><span class="sxs-lookup"><span data-stu-id="aacdd-103">ms.assetid: ms.service: cosmos-db ms.workload: data-services ms.tgt_pltfrm: na ms.devlang: na ms.topic: reference ms.date: 06/13/2017 ms.author: mimig</span></span>

---

# <a name="azure-cosmos-db-documentdb-api-sql-syntax-reference"></a><span data-ttu-id="aacdd-104">Az Azure Cosmos DB DocumentDB API: SQL-szintaxis referencia</span><span class="sxs-lookup"><span data-stu-id="aacdd-104">Azure Cosmos DB DocumentDB API: SQL syntax reference</span></span>

<span data-ttu-id="aacdd-105">hello Azure Cosmos DB DocumentDB API támogatja a dokumentumok lekérdezését a megszokott SQL (Structured Query Language) használatával például nyelvtan hierarchikus JSON-dokumentumok keresztül anélkül, hogy explicit séma vagy a másodlagos indexek létrehozását.</span><span class="sxs-lookup"><span data-stu-id="aacdd-105">hello Azure Cosmos DB DocumentDB API supports querying documents using a familiar SQL (Structured Query Language) like grammar over hierarchical JSON documents without requiring explicit schema or creation of secondary indexes.</span></span> <span data-ttu-id="aacdd-106">Ez a témakör a DocumentDB API SQL lekérdező nyelve hello dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="aacdd-106">This topic provides reference documentation for hello DocumentDB API SQL query language.</span></span>

<span data-ttu-id="aacdd-107">A DocumentDB API SQL lekérdező nyelve hello útmutatást lásd: [Azure Cosmos DB DocumentDB API SQL-lekérdezések](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="aacdd-107">For a walkthrough of hello DocumentDB API SQL query language, see [SQL queries for Azure Cosmos DB DocumentDB API](documentdb-sql-query.md).</span></span>  
  
<span data-ttu-id="aacdd-108">Azt is meghívhatjuk toovisit hello [Tesztlekérdezéseket](http://www.documentdb.com/sql/demo) ahol Azure Cosmos DB próbálja és SQL-lekérdezések futtatása az adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="aacdd-108">We also invite you toovisit hello [Query Playground](http://www.documentdb.com/sql/demo) where you can try Azure Cosmos DB and run SQL queries against our dataset.</span></span>  
  
## <a name="select-query"></a><span data-ttu-id="aacdd-109">SELECT lekérdezés</span><span class="sxs-lookup"><span data-stu-id="aacdd-109">SELECT query</span></span>  
<span data-ttu-id="aacdd-110">Hello adatbázisból kéri le a JSON-dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="aacdd-110">Retrieves JSON documents from hello database.</span></span> <span data-ttu-id="aacdd-111">Kifejezések kiértékelésével, olyan leképezések szűrés támogatja, és csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="aacdd-111">Supports expression evaluation, projections, filtering and joins.</span></span>  <span data-ttu-id="aacdd-112">hello jelölések hello KIVÁLASZTÓ utasítást leíró megjelennének a hello szintaxis egyezmények szakasz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-112">hello conventions used for describing hello SELECT statements are tabulated in hello Syntax conventions section.</span></span>  
  
<span data-ttu-id="aacdd-113">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-113">**Syntax**</span></span>  
  
```
<select_query> ::=  
SELECT <select_specification>   
    [ FROM <from_specification>]   
    [ WHERE <filter_condition> ]  
    [ ORDER BY <sort_specification> ]  
```  
  
 <span data-ttu-id="aacdd-114">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="aacdd-114">**Remarks**</span></span>  
  
 <span data-ttu-id="aacdd-115">Lásd az alábbi szakaszok további részletek az egyes záradékban:</span><span class="sxs-lookup"><span data-stu-id="aacdd-115">See following sections for details on each clause:</span></span>  
  
-   [<span data-ttu-id="aacdd-116">SELECT záradékban</span><span class="sxs-lookup"><span data-stu-id="aacdd-116">SELECT clause</span></span>](#bk_select_query)  
  
-   [<span data-ttu-id="aacdd-117">FROM záradékban</span><span class="sxs-lookup"><span data-stu-id="aacdd-117">FROM clause</span></span>](#bk_from_clause)  
  
-   [<span data-ttu-id="aacdd-118">A WHERE záradék</span><span class="sxs-lookup"><span data-stu-id="aacdd-118">WHERE clause</span></span>](#bk_where_clause)  
  
-   [<span data-ttu-id="aacdd-119">ORDER BY záradék</span><span class="sxs-lookup"><span data-stu-id="aacdd-119">ORDER BY clause</span></span>](#bk_orderby_clause)  
  
<span data-ttu-id="aacdd-120">hello záradékok hello SELECT utasítással kell lehetnek rendezve, ahogy fent látható.</span><span class="sxs-lookup"><span data-stu-id="aacdd-120">hello clauses in hello SELECT statement must be ordered as shown above.</span></span> <span data-ttu-id="aacdd-121">A hello választható záradékok elhagyható.</span><span class="sxs-lookup"><span data-stu-id="aacdd-121">Any one of hello optional clauses can be omitted.</span></span> <span data-ttu-id="aacdd-122">De választható záradék használata esetén azok kell megjelennie hello megfelelő sorrendben.</span><span class="sxs-lookup"><span data-stu-id="aacdd-122">But when optional clauses are used, they must appear in hello right order.</span></span>  
  
<span data-ttu-id="aacdd-123">**Logikai a feldolgozási sorrendben hello SELECT utasítás**</span><span class="sxs-lookup"><span data-stu-id="aacdd-123">**Logical Processing Order of hello SELECT statement**</span></span>  
  
<span data-ttu-id="aacdd-124">amely záradékok feldolgozásának hello sorrendjét van:</span><span class="sxs-lookup"><span data-stu-id="aacdd-124">hello order in which clauses are processed is:</span></span>  

1.  [<span data-ttu-id="aacdd-125">FROM záradékban</span><span class="sxs-lookup"><span data-stu-id="aacdd-125">FROM clause</span></span>](#bk_from_clause)  
2.  [<span data-ttu-id="aacdd-126">A WHERE záradék</span><span class="sxs-lookup"><span data-stu-id="aacdd-126">WHERE clause</span></span>](#bk_where_clause)  
3.  [<span data-ttu-id="aacdd-127">ORDER BY záradék</span><span class="sxs-lookup"><span data-stu-id="aacdd-127">ORDER BY clause</span></span>](#bk_orderby_clause)  
4.  [<span data-ttu-id="aacdd-128">SELECT záradékban</span><span class="sxs-lookup"><span data-stu-id="aacdd-128">SELECT clause</span></span>](#bk_select_query)  

<span data-ttu-id="aacdd-129">Vegye figyelembe, hogy ez nem azonos a hello hello szintaxisa látható sorrendben.</span><span class="sxs-lookup"><span data-stu-id="aacdd-129">Note that this is different from hello order in which they appear in hello syntax.</span></span> <span data-ttu-id="aacdd-130">hello rendelési van, úgy, hogy a feldolgozott záradék által bevezetett új szimbólumok láthatóak, és később feldolgozott záradékban használható.</span><span class="sxs-lookup"><span data-stu-id="aacdd-130">hello ordering is such that all new symbols introduced by a processed clause are visible and can be used in clauses processed later.</span></span> <span data-ttu-id="aacdd-131">Például egy FROM záradékban megadott aliasok érhetők el, ha és SELECT záradékban.</span><span class="sxs-lookup"><span data-stu-id="aacdd-131">For instance, aliases declared in a FROM clause are accessible in WHERE and SELECT clauses.</span></span>  

<span data-ttu-id="aacdd-132">**Az elválasztó karakterek és megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="aacdd-132">**Whitespace characters and comments**</span></span>  

<span data-ttu-id="aacdd-133">Minden szóköztől eltérő karakterek nem idézőjelek közé zárt karakterlánc egy részét vagy idézőjelek között azonosítója nem hello nyelvi szintaxis részét képezik, és figyelmen kívül lesznek hagyva elemzése során.</span><span class="sxs-lookup"><span data-stu-id="aacdd-133">All white space characters which are not part of a quoted string or quoted identifier are not part of hello language grammar and are ignored during parsing.</span></span>  

<span data-ttu-id="aacdd-134">hello lekérdezési nyelv támogatja például a T-SQL stílus megjegyzések</span><span class="sxs-lookup"><span data-stu-id="aacdd-134">hello query language supports T-SQL style comments like</span></span>  

-   <span data-ttu-id="aacdd-135">SQL-utasításban`-- comment text [newline]`</span><span class="sxs-lookup"><span data-stu-id="aacdd-135">SQL Statement `-- comment text [newline]`</span></span>  

<span data-ttu-id="aacdd-136">Amíg az elválasztó karakterek és a megjegyzések nem bármely többszörösére hello nyelvtanban, használt tooseparate jogkivonatok kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="aacdd-136">While whitespace characters and comments do not have any significance in hello grammar, they must be used tooseparate tokens.</span></span> <span data-ttu-id="aacdd-137">Például: `-1e5` számú token, igénybe van`: – 1 e5` mínusz jogkivonat követi 1-es számú azonosítóval e5.</span><span class="sxs-lookup"><span data-stu-id="aacdd-137">For instance: `-1e5` is a single number token, while`: – 1 e5` is a minus token followed by number 1 and identifier e5.</span></span>  

##  <span data-ttu-id="aacdd-138"><a name="bk_select_query"></a>SELECT záradékban</span><span class="sxs-lookup"><span data-stu-id="aacdd-138"><a name="bk_select_query"></a> SELECT clause</span></span>  
<span data-ttu-id="aacdd-139">hello záradékok hello SELECT utasítással kell lehetnek rendezve, ahogy fent látható.</span><span class="sxs-lookup"><span data-stu-id="aacdd-139">hello clauses in hello SELECT statement must be ordered as shown above.</span></span> <span data-ttu-id="aacdd-140">A hello választható záradékok elhagyható.</span><span class="sxs-lookup"><span data-stu-id="aacdd-140">Any one of hello optional clauses can be omitted.</span></span> <span data-ttu-id="aacdd-141">De választható záradék használata esetén azok kell megjelennie hello megfelelő sorrendben.</span><span class="sxs-lookup"><span data-stu-id="aacdd-141">But when optional clauses are used, they must appear in hello right order.</span></span>  

<span data-ttu-id="aacdd-142">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-142">**Syntax**</span></span>  
```  
SELECT <select_specification>  

<select_specification> ::=   
      '*'   
      | <object_property_list>   
      | VALUE <scalar_expression> [[ AS ] value_alias]  
  
<object_property_list> ::=   
{ <scalar_expression> [ [ AS ] property_alias ] } [ ,...n ]  
  
```  
  
 <span data-ttu-id="aacdd-143">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-143">**Arguments**</span></span>  
  
 `<select_specification>`  
  
 <span data-ttu-id="aacdd-144">Hello eredmény kijelölt tulajdonságok vagy -érték toobe beállítása.</span><span class="sxs-lookup"><span data-stu-id="aacdd-144">Properties or value toobe selected for hello result set.</span></span>  
  
 `'*'`  
  
<span data-ttu-id="aacdd-145">Meghatározza, hogy olvassa-hello értéket változtatás nélkül.</span><span class="sxs-lookup"><span data-stu-id="aacdd-145">Specifies that hello value should be retrieved without making any changes.</span></span> <span data-ttu-id="aacdd-146">Kifejezetten feldolgozott hello értéke egy objektumot, ha az összes tulajdonság veszi.</span><span class="sxs-lookup"><span data-stu-id="aacdd-146">Specifically if hello processed value is an object, all properties will be retrieved.</span></span>  
  
 `<object_property_list>`  
  
<span data-ttu-id="aacdd-147">Felsorolja hello tulajdonságok toobe lekérése.</span><span class="sxs-lookup"><span data-stu-id="aacdd-147">Specifies hello list of properties toobe retrieved.</span></span> <span data-ttu-id="aacdd-148">Minden visszaadott értéket fogja hello tulajdonságok lettek megadva objektum.</span><span class="sxs-lookup"><span data-stu-id="aacdd-148">Each returned value will be an object with hello properties specified.</span></span>  
  
`VALUE`  
  
<span data-ttu-id="aacdd-149">Meghatározza, hogy kell-e hello JSON-érték olvasni hello teljes JSON-objektum helyett.</span><span class="sxs-lookup"><span data-stu-id="aacdd-149">Specifies that hello JSON value should be retrieved instead of hello complete JSON object.</span></span> <span data-ttu-id="aacdd-150">Ezzel szemben, `<property_list>` nem törik több sorba tervezett hello érték egy objektumban.</span><span class="sxs-lookup"><span data-stu-id="aacdd-150">This, unlike `<property_list>` does not wrap hello projected value in an object.</span></span>  
  
`<scalar_expression>`  
  
<span data-ttu-id="aacdd-151">Számított hello érték toobe-kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-151">Expression representing hello value toobe computed.</span></span> <span data-ttu-id="aacdd-152">Lásd: [skaláris kifejezések](#bk_scalar_expressions) című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="aacdd-152">See [Scalar expressions](#bk_scalar_expressions) section for details.</span></span>  
  
<span data-ttu-id="aacdd-153">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="aacdd-153">**Remarks**</span></span>  
  
<span data-ttu-id="aacdd-154">Hello `SELECT *` szintaxis csak akkor érvényes, ha a FROM záradékban deklarálva van: pontosan egy aliast.</span><span class="sxs-lookup"><span data-stu-id="aacdd-154">hello `SELECT *` syntax is only valid if FROM clause has declared exactly one alias.</span></span> <span data-ttu-id="aacdd-155">`SELECT *`Itt egy identitás-leképezés, amely lehet hasznos, ha nincs leképezés van szükség.</span><span class="sxs-lookup"><span data-stu-id="aacdd-155">`SELECT *` provides an identity projection, which can be useful if no projection is needed.</span></span> <span data-ttu-id="aacdd-156">Válassza ki * csak akkor érvényes, ha a FROM záradékban megadott és bevezetett csak egyetlen bemenetet.</span><span class="sxs-lookup"><span data-stu-id="aacdd-156">SELECT * is only valid if FROM clause is specified and introduced only a single input source.</span></span>  
  
<span data-ttu-id="aacdd-157">Vegye figyelembe, hogy `SELECT <select_list>` és `SELECT *` "szintaktikai cukor", és azt is megteheti lehetnek a lent látható módon egyszerű SELECT utasítás használatával.</span><span class="sxs-lookup"><span data-stu-id="aacdd-157">Note that `SELECT <select_list>` and `SELECT *` are "syntactic sugar" and can be alternatively expressed by using simple SELECT statements as shown below.</span></span>  
  
1.  `SELECT * FROM ... AS from_alias ...`  
  
     <span data-ttu-id="aacdd-158">az egyenértékű:</span><span class="sxs-lookup"><span data-stu-id="aacdd-158">is equivalent to:</span></span>  
  
     `SELECT from_alias FROM ... AS from_alias ...`  
  
2.  `SELECT <expr1> AS p1, <expr2> AS p2,..., <exprN> AS pN [other clauses...]`  
  
     <span data-ttu-id="aacdd-159">az egyenértékű:</span><span class="sxs-lookup"><span data-stu-id="aacdd-159">is equivalent to:</span></span>  
  
     `SELECT VALUE { p1: <expr1>, p2: <expr2>, ..., pN: <exprN> }[other clauses...]`  
  
<span data-ttu-id="aacdd-160">**Lásd még:**</span><span class="sxs-lookup"><span data-stu-id="aacdd-160">**See Also**</span></span>  
  
[<span data-ttu-id="aacdd-161">Skaláris kifejezések</span><span class="sxs-lookup"><span data-stu-id="aacdd-161">Scalar expressions</span></span>](#bk_scalar_expressions)  
[<span data-ttu-id="aacdd-162">SELECT záradékban</span><span class="sxs-lookup"><span data-stu-id="aacdd-162">SELECT clause</span></span>](#bk_select_query)  
  
##  <span data-ttu-id="aacdd-163"><a name="bk_from_clause"></a>FROM záradékban</span><span class="sxs-lookup"><span data-stu-id="aacdd-163"><a name="bk_from_clause"></a> FROM clause</span></span>  
<span data-ttu-id="aacdd-164">Megadja a csatlakoztatott adatforrásokat, illetve hello forrás.</span><span class="sxs-lookup"><span data-stu-id="aacdd-164">Specifies hello source or joined sources.</span></span> <span data-ttu-id="aacdd-165">hello FROM záradék használata nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="aacdd-165">hello FROM clause is optional.</span></span> <span data-ttu-id="aacdd-166">Ha nem a megadott, más záradékok továbbra is hajtható végre, mint ha a FROM záradékban megadott egyetlen dokumentum.</span><span class="sxs-lookup"><span data-stu-id="aacdd-166">If not specified, other clauses will still be executed as if FROM clause provided a single document.</span></span>  
  
<span data-ttu-id="aacdd-167">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-167">**Syntax**</span></span>  
  
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
  
<span data-ttu-id="aacdd-168">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-168">**Arguments**</span></span>  
  
`<from_source>`  
  
<span data-ttu-id="aacdd-169">Meghatározza egy adatforrást, vagy az alias nélkül.</span><span class="sxs-lookup"><span data-stu-id="aacdd-169">Specifies a data source, with or without an alias.</span></span> <span data-ttu-id="aacdd-170">Ha nincs megadva alias, azt fogja következtethető hello `<collection_expression>` következő szabályok segítségével:</span><span class="sxs-lookup"><span data-stu-id="aacdd-170">If alias is not specified, it will be inferred from hello `<collection_expression>` using following rules:</span></span>  
  
-   <span data-ttu-id="aacdd-171">Ha hello kifejezés egy Lekérdezésnév, majd a lekérdezésnév használható aliasként.</span><span class="sxs-lookup"><span data-stu-id="aacdd-171">If hello expression is a collection_name, then collection_name will be used as an alias.</span></span>  
  
-   <span data-ttu-id="aacdd-172">Ha hello kifejezés `<collection_expression>`, akkor property_name, majd property_name használja a rendszer aliasként.</span><span class="sxs-lookup"><span data-stu-id="aacdd-172">If hello expression is `<collection_expression>`, then property_name, then property_name will be used as an alias.</span></span> <span data-ttu-id="aacdd-173">Ha hello kifejezés egy Lekérdezésnév, majd a lekérdezésnév használható aliasként.</span><span class="sxs-lookup"><span data-stu-id="aacdd-173">If hello expression is a collection_name, then collection_name will be used as an alias.</span></span>  
  
<span data-ttu-id="aacdd-174">MIVEL`input_alias`</span><span class="sxs-lookup"><span data-stu-id="aacdd-174">AS `input_alias`</span></span>  
  
<span data-ttu-id="aacdd-175">Meghatározza, hogy hello `input_alias` hello az alapul szolgáló gyűjtemény kifejezés által visszaadott értékek készlete.</span><span class="sxs-lookup"><span data-stu-id="aacdd-175">Specifies that hello `input_alias` is a set of values returned by hello underlying collection expression.</span></span>  
 
<span data-ttu-id="aacdd-176">`input_alias`IN</span><span class="sxs-lookup"><span data-stu-id="aacdd-176">`input_alias` IN</span></span>  
  
<span data-ttu-id="aacdd-177">Meghatározza, hogy hello `input_alias` értékeket a tömb összes tagjának minden hello az alapul szolgáló gyűjtemény kifejezés által visszaadott tömb keresztül léptetés hello készletét képviseli.</span><span class="sxs-lookup"><span data-stu-id="aacdd-177">Specifies that hello `input_alias` should represent hello set of values obtained by iterating over all array elements of each array returned by hello underlying collection expression.</span></span> <span data-ttu-id="aacdd-178">Minden alapul szolgáló gyűjtemény-kifejezés nem tömb által visszaadott értéket a rendszer figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="aacdd-178">Any value returned by underlying collection expression that is not an array is ignored.</span></span>  
  
`<collection_expression>`  
  
<span data-ttu-id="aacdd-179">Megadja a hello gyűjtemény kifejezés toobe használt tooretrieve hello dokumentumok.</span><span class="sxs-lookup"><span data-stu-id="aacdd-179">Specifies hello collection expression toobe used tooretrieve hello documents.</span></span>  
  
`ROOT`  
  
<span data-ttu-id="aacdd-180">Meghatározza, hogy a dokumentum hello alapértelmezés szerint a jelenleg csatlakoztatott gyűjtemény lehet beolvasni.</span><span class="sxs-lookup"><span data-stu-id="aacdd-180">Specifies that document should be retrieved from hello default, currently connected collection.</span></span>  
  
`collection_name`  
  
<span data-ttu-id="aacdd-181">Meghatározza, hogy a dokumentum be kell olvasni a megadott hello gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="aacdd-181">Specifies that document should be retrieved from hello provided collection.</span></span> <span data-ttu-id="aacdd-182">hello gyűjtemény hello nevének meg kell egyeznie a jelenleg csatlakoztatott hello gyűjtemény hello nevét.</span><span class="sxs-lookup"><span data-stu-id="aacdd-182">hello name of hello collection must match hello name of hello collection currently connected to.</span></span>  
  
`input_alias`  
  
<span data-ttu-id="aacdd-183">Meghatározza, hogy a dokumentum olvassa be a hello határozzák meg a megadott hello alias más forrásból.</span><span class="sxs-lookup"><span data-stu-id="aacdd-183">Specifies that document should be retrieved from hello other source defined by hello provided alias.</span></span>  
  
`<collection_expression> '.' property_`  
  
<span data-ttu-id="aacdd-184">Megadja, hogy a dokumentum be kell olvasni hello elérésével `property_name` tulajdonság vagy tömbindex tömbelem által beolvasott minden dokumentumhoz megadott gyűjtemény kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-184">Specifies that document should be retrieved by accessing hello `property_name` property or array_index array element for all documents retrieved by specified collection expression.</span></span>  
  
`<collection_expression> '[' "property_name" | array_index ']'`  
  
<span data-ttu-id="aacdd-185">Megadja, hogy a dokumentum be kell olvasni hello elérésével `property_name` tulajdonság vagy tömbindex tömbelem által beolvasott minden dokumentumhoz megadott gyűjtemény kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-185">Specifies that document should be retrieved by accessing hello `property_name` property or array_index array element for all documents retrieved by specified collection expression.</span></span>  
  
<span data-ttu-id="aacdd-186">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="aacdd-186">**Remarks**</span></span>  
  
<span data-ttu-id="aacdd-187">Összes alias megadott vagy hello következtetni `<from_source>(`s) egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="aacdd-187">All aliases provided or inferred in hello `<from_source>(`s) must be unique.</span></span> <span data-ttu-id="aacdd-188">hello szintaxis `<collection_expression>.`property_name van hello azonos `<collection_expression>' ['"property_name"']'`.</span><span class="sxs-lookup"><span data-stu-id="aacdd-188">hello Syntax `<collection_expression>.`property_name is hello same as `<collection_expression>' ['"property_name"']'`.</span></span> <span data-ttu-id="aacdd-189">Azonban ez utóbbi szintaxis hello használható, ha az egyik tulajdonságnév nem azonosító karakternél.</span><span class="sxs-lookup"><span data-stu-id="aacdd-189">However, hello latter syntax can be used if a property name contains a non-identifier characters.</span></span>  
  
<span data-ttu-id="aacdd-190">**Hiányzó tulajdonságok hiányzik a tömb elemei nem definiált értékek kezelése**</span><span class="sxs-lookup"><span data-stu-id="aacdd-190">**Missing properties, missing array elements, undefined values handling**</span></span>  
  
<span data-ttu-id="aacdd-191">Ha egy gyűjtemény kifejezés fér hozzá a Tulajdonságok vagy tömb elemeinek és, hogy az érték nem létezik, ezt az értéket figyelmen kívül hagyva, és további nincs feldolgozva.</span><span class="sxs-lookup"><span data-stu-id="aacdd-191">If a collection expression accesses properties or array elements and that value does not exist, that value will be ignored and not processed further.</span></span>  
  
<span data-ttu-id="aacdd-192">**Gyűjtemény kifejezés környezetben hatókörének beállítása**</span><span class="sxs-lookup"><span data-stu-id="aacdd-192">**Collection expression context scoping**</span></span>  
  
<span data-ttu-id="aacdd-193">Egy gyűjtemény kifejezés gyűjtemény hatóköre vagy dokumentum-hatóköre lehet:</span><span class="sxs-lookup"><span data-stu-id="aacdd-193">A collection expression may be collection-scoped or document-scoped:</span></span>  
  
-   <span data-ttu-id="aacdd-194">Kifejezés a gyűjtemény hatóköre, ha a hello hello gyűjtemény kifejezés alapul szolgáló adatforrás vagy a gyökér vagy `collection_name`.</span><span class="sxs-lookup"><span data-stu-id="aacdd-194">An expression is collection-scoped, if hello underlying source of hello collection expression is either ROOT or `collection_name`.</span></span> <span data-ttu-id="aacdd-195">Ilyen kifejezés hello gyűjtemény közvetlenül lekért dokumentumok készletét reprezentálja, és nincs gyűjtemény kifejezésekre hello feldolgozási függ.</span><span class="sxs-lookup"><span data-stu-id="aacdd-195">Such an expression represents a set of documents retrieved from hello collection directly, and is not dependent on hello processing of other collection expressions.</span></span>  
  
-   <span data-ttu-id="aacdd-196">Egy kifejezés dokumentum hatókörű, ha hello hello gyűjtemény kifejezés alapul szolgáló adatforrás `input_alias` korábban bemutatott hello lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-196">An expression is document-scoped, if hello underlying source of hello collection expression is `input_alias` introduced earlier in hello query.</span></span> <span data-ttu-id="aacdd-197">Ilyen kifejezés kiértékelése hello gyűjtemény kifejezés hello hatókörébe tartozó hello aliasnevet gyűjteményhez társított toohello beállítása a dokumentumot kapott dokumentumot jelöli.</span><span class="sxs-lookup"><span data-stu-id="aacdd-197">Such an expression represents a set of documents obtained by evaluating hello collection expression in hello scope of each document belonging toohello set associated with hello aliased collection.</span></span>  <span data-ttu-id="aacdd-198">hello eredő egy révén az egyes az alapul szolgáló set hello hello dokumentumainak hello gyűjtemény kifejezés kiértékelése készlet unióját lesz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-198">hello resulting set will be a union of sets obtained by evaluating hello collection expression for each of hello documents in hello underlying set.</span></span>  
  
<span data-ttu-id="aacdd-199">**Illesztése**</span><span class="sxs-lookup"><span data-stu-id="aacdd-199">**Joins**</span></span>  
  
<span data-ttu-id="aacdd-200">Hello jelenlegi kiadásban Azure Cosmos DB belső illesztések támogatja.</span><span class="sxs-lookup"><span data-stu-id="aacdd-200">In hello current release, Azure Cosmos DB supports inner joins.</span></span> <span data-ttu-id="aacdd-201">További illesztési képességek érkeznek.</span><span class="sxs-lookup"><span data-stu-id="aacdd-201">Additional join capabilities are forthcoming.</span></span>

<span data-ttu-id="aacdd-202">A belső illesztések eredménye egy teljes határokon termékben hello beállítása hello illesztési részt.</span><span class="sxs-lookup"><span data-stu-id="aacdd-202">Inner joins result in a complete cross product of hello sets participating in hello join.</span></span> <span data-ttu-id="aacdd-203">az N-módon illesztési hello eredményét olyan N-elem listának, ahol a hello rekordban szereplő értékekhez hello aliasnevet részt vevő beállított hello illesztés hozzárendelve, és ez az alias más záradékban Vezérlőpultjának érhető el.</span><span class="sxs-lookup"><span data-stu-id="aacdd-203">hello result of an N-way join is a set of N-element tuples, where each value in hello tuple is associated with hello aliased set participating in hello join and can be accessed by referencing that alias in other clauses.</span></span>  
  
<span data-ttu-id="aacdd-204">hello illesztési hello értékelése hello környezet hatókörét hello beállítása résztvevő függ:</span><span class="sxs-lookup"><span data-stu-id="aacdd-204">hello evaluation of hello join depends on hello context scope of hello participating sets:</span></span>  
  
-  <span data-ttu-id="aacdd-205">Illesztés közötti gyűjtemény- és a gyűjtemény hatóköre a B kiszolgálóra, egy határokon termékben eredményeit A és b készlet összes elemének beállítása</span><span class="sxs-lookup"><span data-stu-id="aacdd-205">A join between collection-set A and collection-scoped set B, results in a cross product of all elements in sets A and B.</span></span>
  
-   <span data-ttu-id="aacdd-206">Készlet és B, set dokumentum hatókörbe tartozó illesztést egy dokumentum hatókörű set nyilvántartott egyes dokumentumok, a B beállítása A. kiértékelésével kapott összes készlet unióját eredményez.</span><span class="sxs-lookup"><span data-stu-id="aacdd-206">A join between set A and document-scoped set B, results in a union of all sets obtained by evaluating document-scoped set B for each document from set A.</span></span>  
  
 <span data-ttu-id="aacdd-207">Hello jelenlegi kiadásban egy gyűjtemény hatókörbe tartozó kifejezés maximum hello lekérdezésfeldolgozó által támogatott.</span><span class="sxs-lookup"><span data-stu-id="aacdd-207">In hello current release, a maximum of one collection-scoped expression is supported by hello query processor.</span></span>  
  
<span data-ttu-id="aacdd-208">**Illesztések példái:**</span><span class="sxs-lookup"><span data-stu-id="aacdd-208">**Examples of joins:**</span></span>  
  
<span data-ttu-id="aacdd-209">Nézzük hello FROM záradék a következő:`<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`</span><span class="sxs-lookup"><span data-stu-id="aacdd-209">Let's look at hello following FROM clause: `<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`</span></span>  
  
 <span data-ttu-id="aacdd-210">Így minden forrás megadása `input_alias1, input_alias2, …, input_aliasN`.</span><span class="sxs-lookup"><span data-stu-id="aacdd-210">Let each source define `input_alias1, input_alias2, …, input_aliasN`.</span></span> <span data-ttu-id="aacdd-211">A FROM záradék N-listának (rekordot N értékekkel) adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-211">This FROM clause returns a set of N-tuples (tuple with N values).</span></span> <span data-ttu-id="aacdd-212">A táblakonstruktor minden rekordjának összes gyűjtemény alias léptetés alatt az megfelelő készletek által visszaadott érték tartozik.</span><span class="sxs-lookup"><span data-stu-id="aacdd-212">Each tuple has values produced by iterating all collection aliases over their respective sets.</span></span>  
  
<span data-ttu-id="aacdd-213">*CSATLAKOZZON például 1, 2 forrásokat:*</span><span class="sxs-lookup"><span data-stu-id="aacdd-213">*JOIN example 1, with 2 sources:*</span></span>  
  
- <span data-ttu-id="aacdd-214">Így `<from_source1>` kell gyűjtemény-hatókörű, és megfelelnek csoportjának {A, B, C}.</span><span class="sxs-lookup"><span data-stu-id="aacdd-214">Let `<from_source1>` be collection-scoped and represent set {A, B, C}.</span></span>  
  
- <span data-ttu-id="aacdd-215">Így `<from_source2>` dokumentum hatókörű input_alias1 hivatkozó és képviselő beállítása:</span><span class="sxs-lookup"><span data-stu-id="aacdd-215">Let `<from_source2>` be document-scoped referencing input_alias1 and represent sets:</span></span>  
  
    <span data-ttu-id="aacdd-216">{1, 2} számára`input_alias1 = A,`</span><span class="sxs-lookup"><span data-stu-id="aacdd-216">{1, 2} for `input_alias1 = A,`</span></span>  
  
    <span data-ttu-id="aacdd-217">a {3}`input_alias1 = B,`</span><span class="sxs-lookup"><span data-stu-id="aacdd-217">{3} for `input_alias1 = B,`</span></span>  
  
    <span data-ttu-id="aacdd-218">{4, 5} számára`input_alias1 = C,`</span><span class="sxs-lookup"><span data-stu-id="aacdd-218">{4, 5} for `input_alias1 = C,`</span></span>  
  
- <span data-ttu-id="aacdd-219">hello FROM záradékban `<from_source1> JOIN <from_source2>` rekordokat a következő hello eredményezi:</span><span class="sxs-lookup"><span data-stu-id="aacdd-219">hello FROM clause `<from_source1> JOIN <from_source2>` will result in hello following tuples:</span></span>  
  
    <span data-ttu-id="aacdd-220">(`input_alias1, input_alias2`):</span><span class="sxs-lookup"><span data-stu-id="aacdd-220">(`input_alias1, input_alias2`):</span></span>  
  
    `(A, 1), (A, 2), (B, 3), (C, 4), (C, 5)`  
  
<span data-ttu-id="aacdd-221">*CSATLAKOZZON például 2, 3 forrásokat:*</span><span class="sxs-lookup"><span data-stu-id="aacdd-221">*JOIN example 2, with 3 sources:*</span></span>  
  
- <span data-ttu-id="aacdd-222">Így `<from_source1>` kell gyűjtemény-hatókörű, és megfelelnek csoportjának {A, B, C}.</span><span class="sxs-lookup"><span data-stu-id="aacdd-222">Let `<from_source1>` be collection-scoped and represent set {A, B, C}.</span></span>  
  
- <span data-ttu-id="aacdd-223">Lehetővé teszik `<from_source2>` kell a dokumentum-hatókörű hivatkozó `input_alias1` készletek tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="aacdd-223">Let `<from_source2>` be document-scoped referencing `input_alias1` and represent sets:</span></span>  
  
    <span data-ttu-id="aacdd-224">{1, 2} számára`input_alias1 = A,`</span><span class="sxs-lookup"><span data-stu-id="aacdd-224">{1, 2} for `input_alias1 = A,`</span></span>  
  
    <span data-ttu-id="aacdd-225">a {3}`input_alias1 = B,`</span><span class="sxs-lookup"><span data-stu-id="aacdd-225">{3} for `input_alias1 = B,`</span></span>  
  
    <span data-ttu-id="aacdd-226">{4, 5} számára`input_alias1 = C,`</span><span class="sxs-lookup"><span data-stu-id="aacdd-226">{4, 5} for `input_alias1 = C,`</span></span>  
  
- <span data-ttu-id="aacdd-227">Lehetővé teszik `<from_source3>` kell a dokumentum-hatókörű hivatkozó `input_alias2` készletek tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="aacdd-227">Let `<from_source3>` be document-scoped referencing `input_alias2` and represent sets:</span></span>  
  
    <span data-ttu-id="aacdd-228">{100, 200} számára`input_alias2 = 1,`</span><span class="sxs-lookup"><span data-stu-id="aacdd-228">{100, 200} for `input_alias2 = 1,`</span></span>  
  
    <span data-ttu-id="aacdd-229">a {300}`input_alias2 = 3,`</span><span class="sxs-lookup"><span data-stu-id="aacdd-229">{300} for `input_alias2 = 3,`</span></span>  
  
- <span data-ttu-id="aacdd-230">hello FROM záradékban `<from_source1> JOIN <from_source2> JOIN <from_source3>` rekordokat a következő hello eredményezi:</span><span class="sxs-lookup"><span data-stu-id="aacdd-230">hello FROM clause `<from_source1> JOIN <from_source2> JOIN <from_source3>` will result in hello following tuples:</span></span>  
  
    <span data-ttu-id="aacdd-231">(input_alias1, input_alias2, input_alias3):</span><span class="sxs-lookup"><span data-stu-id="aacdd-231">(input_alias1, input_alias2, input_alias3):</span></span>  
  
    <span data-ttu-id="aacdd-232">(A, 1, 100), (A, 1, 200-AS), (B, 3, 300)</span><span class="sxs-lookup"><span data-stu-id="aacdd-232">(A, 1, 100), (A, 1, 200), (B, 3, 300)</span></span>  
  
> [!NOTE]
> <span data-ttu-id="aacdd-233">Más értékek a rekordokat hiánya `input_alias1`, `input_alias2`, mely hello a `<from_source3>` nem adott vissza semmilyen értéket.</span><span class="sxs-lookup"><span data-stu-id="aacdd-233">Lack of tuples for other values of `input_alias1`, `input_alias2`, for which hello `<from_source3>` did not return any values.</span></span>  
  
<span data-ttu-id="aacdd-234">*KAPCSOLÓDÁS a példa 3, 3 forrásokat:*</span><span class="sxs-lookup"><span data-stu-id="aacdd-234">*JOIN example 3, with 3 sources:*</span></span>  
  
- <span data-ttu-id="aacdd-235">< From_source1 > kell a gyűjtemény hatóköre, és megfelelnek {A, B, C} csoportjának segítségével.</span><span class="sxs-lookup"><span data-stu-id="aacdd-235">Let <from_source1> be collection-scoped and represent set {A, B, C}.</span></span>  
  
- <span data-ttu-id="aacdd-236">Így `<from_source1>` kell gyűjtemény-hatókörű, és megfelelnek csoportjának {A, B, C}.</span><span class="sxs-lookup"><span data-stu-id="aacdd-236">Let `<from_source1>` be collection-scoped and represent set {A, B, C}.</span></span>  
  
- <span data-ttu-id="aacdd-237">< From_source2 > hivatkozó input_alias1 dokumentum hatókörű, és készletek képviselő segítségével:</span><span class="sxs-lookup"><span data-stu-id="aacdd-237">Let <from_source2> be document-scoped referencing input_alias1 and represent sets:</span></span>  
  
    <span data-ttu-id="aacdd-238">{1, 2} számára`input_alias1 = A,`</span><span class="sxs-lookup"><span data-stu-id="aacdd-238">{1, 2} for `input_alias1 = A,`</span></span>  
  
    <span data-ttu-id="aacdd-239">a {3}`input_alias1 = B,`</span><span class="sxs-lookup"><span data-stu-id="aacdd-239">{3} for `input_alias1 = B,`</span></span>  
  
    <span data-ttu-id="aacdd-240">{4, 5} számára`input_alias1 = C,`</span><span class="sxs-lookup"><span data-stu-id="aacdd-240">{4, 5} for `input_alias1 = C,`</span></span>  
  
- <span data-ttu-id="aacdd-241">Lehetővé teszik `<from_source3>` túl tartozni`input_alias1` készletek tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="aacdd-241">Let `<from_source3>` be scoped too`input_alias1` and represent sets:</span></span>  
  
    <span data-ttu-id="aacdd-242">{100, 200} számára`input_alias2 = A,`</span><span class="sxs-lookup"><span data-stu-id="aacdd-242">{100, 200} for `input_alias2 = A,`</span></span>  
  
    <span data-ttu-id="aacdd-243">a {300}`input_alias2 = C,`</span><span class="sxs-lookup"><span data-stu-id="aacdd-243">{300} for `input_alias2 = C,`</span></span>  
  
- <span data-ttu-id="aacdd-244">hello FROM záradékban `<from_source1> JOIN <from_source2> JOIN <from_source3>` rekordokat a következő hello eredményezi:</span><span class="sxs-lookup"><span data-stu-id="aacdd-244">hello FROM clause `<from_source1> JOIN <from_source2> JOIN <from_source3>` will result in hello following tuples:</span></span>  
  
    <span data-ttu-id="aacdd-245">(`input_alias1, input_alias2, input_alias3`):</span><span class="sxs-lookup"><span data-stu-id="aacdd-245">(`input_alias1, input_alias2, input_alias3`):</span></span>  
  
    <span data-ttu-id="aacdd-246">(A, 1, 100), (A, 1, 200-AS) (A, 2, 100), (A, 2, 200-AS), C, 4, 300, (C, 5, 300)</span><span class="sxs-lookup"><span data-stu-id="aacdd-246">(A, 1, 100), (A, 1, 200), (A, 2, 100), (A, 2, 200),  (C, 4, 300) ,  (C, 5, 300)</span></span>  
  
> [!NOTE]
> <span data-ttu-id="aacdd-247">Ennek következtében határokon termék közötti `<from_source2>` és `<from_source3>` mert mindkettő hatókörön belüli toohello azonos `<from_source1>`.</span><span class="sxs-lookup"><span data-stu-id="aacdd-247">This resulted in cross product between `<from_source2>` and `<from_source3>` because both are scoped toohello same `<from_source1>`.</span></span>  <span data-ttu-id="aacdd-248">Ez 4 (2 x 2) eredményezett a érték 0 rekordokat B (1 x 0) értékkel rendelkező rekordokat és (2 x 1) 2 c-értékkel rekordokat</span><span class="sxs-lookup"><span data-stu-id="aacdd-248">This resulted in 4 (2x2) tuples having value A, 0 tuples having value B (1x0) and 2 (2x1) tuples having value C.</span></span>  
  
<span data-ttu-id="aacdd-249">**Lásd még:**</span><span class="sxs-lookup"><span data-stu-id="aacdd-249">**See also**</span></span>  
  
 [<span data-ttu-id="aacdd-250">SELECT záradékban</span><span class="sxs-lookup"><span data-stu-id="aacdd-250">SELECT clause</span></span>](#bk_select_query)  
  
##  <span data-ttu-id="aacdd-251"><a name="bk_where_clause"></a>A WHERE záradék</span><span class="sxs-lookup"><span data-stu-id="aacdd-251"><a name="bk_where_clause"></a> WHERE clause</span></span>  
 <span data-ttu-id="aacdd-252">Adja meg a keresési feltétel hello hello lekérdezés által visszaadott hello dokumentumok.</span><span class="sxs-lookup"><span data-stu-id="aacdd-252">Specifies hello search condition for hello documents returned by hello query.</span></span>  
  
 <span data-ttu-id="aacdd-253">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-253">**Syntax**</span></span>  
  
```  
WHERE <filter_condition>  
<filter_condition> ::= <scalar_expression>  
  
```  
  
 <span data-ttu-id="aacdd-254">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-254">**Arguments**</span></span>  
  
-   `<filter_condition>`  
  
     <span data-ttu-id="aacdd-255">Megadja a hello feltétel toobe teljesülniük hello dokumentumok toobe adott vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-255">Specifies hello condition toobe met for hello documents toobe returned.</span></span>  
  
-   `<scalar_expression>`  
  
     <span data-ttu-id="aacdd-256">Számított hello érték toobe-kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-256">Expression representing hello value toobe computed.</span></span> <span data-ttu-id="aacdd-257">Lásd: hello [skaláris kifejezések](#bk_scalar_expressions) című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="aacdd-257">See hello [Scalar expressions](#bk_scalar_expressions) section for details.</span></span>  
  
 <span data-ttu-id="aacdd-258">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="aacdd-258">**Remarks**</span></span>  
  
 <span data-ttu-id="aacdd-259">Ahhoz, hogy hello dokumentum toobe megadott szűrési feltételt ki kell értékelnie minden tootrue kifejezést adott vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-259">In order for hello document toobe returned an expression specified as filter condition must evaluate tootrue.</span></span> <span data-ttu-id="aacdd-260">Csak az IGAZ logikai értéket eleget tesz a hello feltétel, semmilyen más érték: nincs megadva, null, hamis, számot, tömb vagy objektum nem kielégítéséhez hello feltétel.</span><span class="sxs-lookup"><span data-stu-id="aacdd-260">Only Boolean value true will satisfy hello condition, any other value: undefined, null, false, Number, Array or Object will not satisfy hello condition.</span></span>  
  
##  <span data-ttu-id="aacdd-261"><a name="bk_orderby_clause"></a>ORDER BY záradék</span><span class="sxs-lookup"><span data-stu-id="aacdd-261"><a name="bk_orderby_clause"></a> ORDER BY clause</span></span>  
 <span data-ttu-id="aacdd-262">Megadja a rendezési sorrend a hello lekérdezés által visszaadott eredmény hello.</span><span class="sxs-lookup"><span data-stu-id="aacdd-262">Specifies hello sorting order for results returned by hello query.</span></span>  
  
 <span data-ttu-id="aacdd-263">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-263">**Syntax**</span></span>  
  
```  
ORDER BY <sort_specification>  
<sort_specification> ::= <sort_expression> [, <sort_expression>]  
<sort_expression> ::= <scalar_expression> [ASC | DESC]  
  
```  
  
 <span data-ttu-id="aacdd-264">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-264">**Arguments**</span></span>  
  
-   `<sort_specification>`  
  
     <span data-ttu-id="aacdd-265">Megadja azt a tulajdonságot vagy a kifejezés toosort hello lekérdezés eredménye állítsa be.</span><span class="sxs-lookup"><span data-stu-id="aacdd-265">Specifies a property or expression on which toosort hello query result set.</span></span> <span data-ttu-id="aacdd-266">A rendezési oszlop neve vagy oszlop aliasként adható meg.</span><span class="sxs-lookup"><span data-stu-id="aacdd-266">A sort column can be specified as a name or column alias.</span></span>  
  
     <span data-ttu-id="aacdd-267">Több rendezési oszlop adható meg.</span><span class="sxs-lookup"><span data-stu-id="aacdd-267">Multiple sort columns can be specified.</span></span> <span data-ttu-id="aacdd-268">Nevének egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="aacdd-268">Column names must be unique.</span></span> <span data-ttu-id="aacdd-269">hello oszlopok rendezése hello ORDER BY záradékban hello sorozatát hello szervezet rendezve hello eredményhalmaz határozza meg.</span><span class="sxs-lookup"><span data-stu-id="aacdd-269">hello sequence of hello sort columns in hello ORDER BY clause defines hello organization of hello sorted result set.</span></span> <span data-ttu-id="aacdd-270">Ez azt jelenti, hogy hello eredménykészlet hello első tulajdonság szerint van rendezve, és ezután a rendezett lista van rendezve, hello második tulajdonság, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="aacdd-270">That is, hello result set is sorted by hello first property and then that ordered list is sorted by hello second property, and so on.</span></span>  
  
     <span data-ttu-id="aacdd-271">hello ORDER BY záradékban hivatkozott hello oszlopnevek hello oszlopát válassza ki a lista vagy tooa oszlop a tábla bármely kétértelműséget nélkül hello FROM záradékban megadott tooeither kell megfelelnie.</span><span class="sxs-lookup"><span data-stu-id="aacdd-271">hello column names referenced in hello ORDER BY clause must correspond tooeither a column in hello select list or tooa column defined in a table specified in hello FROM clause without any ambiguities.</span></span>  
  
-   `<sort_expression>`  
  
     <span data-ttu-id="aacdd-272">Mely toosort hello lekérdezés eredményhalmazából megadja egy egyetlen tulajdonság vagy kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-272">Specifies a single property or expression on which toosort hello query result set.</span></span>  
  
-   `<scalar_expression>`  
  
     <span data-ttu-id="aacdd-273">Lásd: hello [skaláris kifejezések](#bk_scalar_expressions) című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="aacdd-273">See hello [Scalar expressions](#bk_scalar_expressions) section for details.</span></span>  
  
-   `ASC | DESC`  
  
     <span data-ttu-id="aacdd-274">Meghatározza, hogy hello hello megadott oszlop értékeit rendezni kell növekvő vagy csökkenő sorrendben.</span><span class="sxs-lookup"><span data-stu-id="aacdd-274">Specifies that hello values in hello specified column should be sorted in ascending or descending order.</span></span> <span data-ttu-id="aacdd-275">ASC értékből hello legkisebb érték toohighest rendezi.</span><span class="sxs-lookup"><span data-stu-id="aacdd-275">ASC sorts from hello lowest value toohighest value.</span></span> <span data-ttu-id="aacdd-276">DESC rendezi a legmagasabb érték toolowest értékből.</span><span class="sxs-lookup"><span data-stu-id="aacdd-276">DESC sorts from highest value toolowest value.</span></span> <span data-ttu-id="aacdd-277">ASC hello alapértelmezett rendezési sorrend.</span><span class="sxs-lookup"><span data-stu-id="aacdd-277">ASC is hello default sort order.</span></span> <span data-ttu-id="aacdd-278">Null értékek hello legkisebb lehetséges értékek tekintendők.</span><span class="sxs-lookup"><span data-stu-id="aacdd-278">Null values are treated as hello lowest possible values.</span></span>  
  
 <span data-ttu-id="aacdd-279">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="aacdd-279">**Remarks**</span></span>  
  
 <span data-ttu-id="aacdd-280">Hello lekérdezési szintaxis támogatja több sorrend tulajdonságai, amíg hello Azure Cosmos DB lekérdezés futásidejű támogatja a rendezést csak egyetlen tulajdonság, és csak elleni tulajdonságnevek, azaz nem számított tulajdonságokhoz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-280">While hello query grammar supports multiple order by properties, hello Azure Cosmos DB query runtime supports sorting only against a single property, and only against property names, i.e., not against computed properties.</span></span> <span data-ttu-id="aacdd-281">Rendezés is szükséges, hogy hello indexelő házirend tartalmazza a tartományindexszel hello tulajdonság és a megadott hello típus, hello maximális pontosság.</span><span class="sxs-lookup"><span data-stu-id="aacdd-281">Sorting also requires that hello indexing policy includes a range index for hello property and hello specified type, with hello maximum precision.</span></span> <span data-ttu-id="aacdd-282">Tekintse meg a házirend dokumentációjában indexeléshez toohello.</span><span class="sxs-lookup"><span data-stu-id="aacdd-282">Refer toohello indexing policy documentation for more details.</span></span>  
  
##  <span data-ttu-id="aacdd-283"><a name="bk_scalar_expressions"></a>Skaláris kifejezések</span><span class="sxs-lookup"><span data-stu-id="aacdd-283"><a name="bk_scalar_expressions"></a> Scalar expressions</span></span>  
 <span data-ttu-id="aacdd-284">Egy skaláris kifejezés szimbólumok kombinációját, és lehet operátorok kiértékelve tooobtain egyetlen értéket.</span><span class="sxs-lookup"><span data-stu-id="aacdd-284">A scalar expression is a combination of symbols and operators that can be evaluated tooobtain a single value.</span></span> <span data-ttu-id="aacdd-285">Egyszerű kifejezések állandók, tulajdonsághivatkozást, tömb hivatkozásokkal, alias hivatkozik, vagy lehet függvényhívásokat.</span><span class="sxs-lookup"><span data-stu-id="aacdd-285">Simple expressions can be constants, property references, array element references, alias references, or function calls.</span></span> <span data-ttu-id="aacdd-286">Egyszerű kifejezések összetett kifejezések operátorok használatával való egyesíthetők.</span><span class="sxs-lookup"><span data-stu-id="aacdd-286">Simple expressions can be combined into complex expressions using operators.</span></span>  
  
 <span data-ttu-id="aacdd-287">További részletek a értékek melyik skaláris kifejezést lehet: [állandók](#bk_constants) szakasz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-287">For details on values which scalar expression may have, see [Constants](#bk_constants) section.</span></span>  
  
 <span data-ttu-id="aacdd-288">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-288">**Syntax**</span></span>  
  
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
  
 <span data-ttu-id="aacdd-289">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-289">**Arguments**</span></span>  
  
-   `<constant>`  
  
     <span data-ttu-id="aacdd-290">Egy állandó értékét jelöli.</span><span class="sxs-lookup"><span data-stu-id="aacdd-290">Represents a constant value.</span></span> <span data-ttu-id="aacdd-291">Lásd: [állandók](#bk_constants) című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="aacdd-291">See [Constants](#bk_constants) section for details.</span></span>  
  
-   `input_alias`  
  
     <span data-ttu-id="aacdd-292">Hello által megadott értéket jelöli `input_alias` hello bevezetett `FROM` záradékban.</span><span class="sxs-lookup"><span data-stu-id="aacdd-292">Represents a value defined by hello `input_alias` introduced in hello `FROM` clause.</span></span>  
    <span data-ttu-id="aacdd-293">Ez az érték garantáltan toonot kell **nem definiált** –**nem definiált** kimarad a hello bemeneti értékeket.</span><span class="sxs-lookup"><span data-stu-id="aacdd-293">This value is guaranteed toonot be **undefined** –**undefined** values in hello input are skipped.</span></span>  
  
-   `<scalar_expression>.property_name`  
  
     <span data-ttu-id="aacdd-294">Egy objektum hello tulajdonság értékét jelöli.</span><span class="sxs-lookup"><span data-stu-id="aacdd-294">Represents a value of hello property of an object.</span></span> <span data-ttu-id="aacdd-295">Ha hello tulajdonság nem létezik vagy tulajdonság hivatkozik egy értéket, amely nem egy objektumot, majd hello kifejezés értéke túl**nem definiált** érték.</span><span class="sxs-lookup"><span data-stu-id="aacdd-295">If hello property does not exist or property is referenced on a value which is not an object, then hello expression evaluates too**undefined** value.</span></span>  
  
-   `<scalar_expression>'['"property_name"|array_index']'`  
  
     <span data-ttu-id="aacdd-296">Hello tulajdonság neve képviseli `property_name` vagy az index tömbelem `array_index` objektum vagy tömb.</span><span class="sxs-lookup"><span data-stu-id="aacdd-296">Represents a value of hello property with name `property_name` or array element with index `array_index` of an object/array.</span></span> <span data-ttu-id="aacdd-297">Ha hello/tulajdonságtömb-index nem létezik, vagy egy érték, amely nem objektum vagy tömb hivatkozott hello/tulajdonságtömb-index, hello kifejezés tooundefined érték értékelődik ki.</span><span class="sxs-lookup"><span data-stu-id="aacdd-297">If hello property/array index does not exist or hello property/array index is referenced on a value which is not an object/array, then hello expression evaluates tooundefined value.</span></span>  
  
-   `unary_operator <scalar_expression>`  
  
     <span data-ttu-id="aacdd-298">Egy operátort alkalmazott tooa által képviselt egyetlen érték.</span><span class="sxs-lookup"><span data-stu-id="aacdd-298">Represents an operator that is applied tooa single value.</span></span> <span data-ttu-id="aacdd-299">Lásd: [operátorok](#bk_operators) című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="aacdd-299">See [Operators](#bk_operators) section for details.</span></span>  
  
-   `<scalar_expression> binary_operator <scalar_expression>`  
  
     <span data-ttu-id="aacdd-300">Alkalmazott tootwo értékek operátor jelöli.</span><span class="sxs-lookup"><span data-stu-id="aacdd-300">Represents an operator that is applied tootwo values.</span></span> <span data-ttu-id="aacdd-301">Lásd: [operátorok](#bk_operators) című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="aacdd-301">See [Operators](#bk_operators) section for details.</span></span>  
  
-   `<scalar_function_expression>`  
  
     <span data-ttu-id="aacdd-302">Egy adott hívás eredménye által megadott értéket jelöli.</span><span class="sxs-lookup"><span data-stu-id="aacdd-302">Represents a value defined by a result of a function call.</span></span>  
  
-   `udf_scalar_function`  
  
     <span data-ttu-id="aacdd-303">Hello felhasználó neve meghatározott skaláris függvényt.</span><span class="sxs-lookup"><span data-stu-id="aacdd-303">Name of hello user defined scalar function.</span></span>  
  
-   `builtin_scalar_function`  
  
     <span data-ttu-id="aacdd-304">Hello beépített skaláris függvény neve.</span><span class="sxs-lookup"><span data-stu-id="aacdd-304">Name of hello built-in scalar function.</span></span>  
  
-   `<create_object_expression>`  
  
     <span data-ttu-id="aacdd-305">Által megadott tulajdonságokkal rendelkező új objektum létrehozása és azok értékei jelöli.</span><span class="sxs-lookup"><span data-stu-id="aacdd-305">Represents a value obtained by creating a new object with specified properties and their values.</span></span>  
  
-   `<create_array_expression>`  
  
     <span data-ttu-id="aacdd-306">Hozzon létre egy új tömb elemként megadott értékekkel kapott értéket jelöli.</span><span class="sxs-lookup"><span data-stu-id="aacdd-306">Represents a value obtained by creating a new array with specified values as elements</span></span>  
  
-   `parameter_name`  
  
     <span data-ttu-id="aacdd-307">A megadott paraméternév hello értékét jelöli.</span><span class="sxs-lookup"><span data-stu-id="aacdd-307">Represents a value of hello specified parameter name.</span></span> <span data-ttu-id="aacdd-308">Paraméterek nevei rendelkeznie kell egyetlen @ hello első karakter.</span><span class="sxs-lookup"><span data-stu-id="aacdd-308">Parameter names must have a single @ as hello first character.</span></span>  
  
 <span data-ttu-id="aacdd-309">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="aacdd-309">**Remarks**</span></span>  
  
 <span data-ttu-id="aacdd-310">Amikor egy beépített vagy felhasználói függvény skaláris összes argumentumot meg kell határozni.</span><span class="sxs-lookup"><span data-stu-id="aacdd-310">When calling a built-in or user defined scalar function all arguments must be defined.</span></span> <span data-ttu-id="aacdd-311">Ha bármely hello argumentuma nincs meghatározva, hello függvény nem hívható, és hello lesz: nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="aacdd-311">If any of hello arguments is undefined, hello function will not be called and hello result will be undefined.</span></span>  
  
 <span data-ttu-id="aacdd-312">Egy objektum létrehozásakor bármely tulajdonság nem definiált érték hozzárendelt kihagyja és hello létrehozott objektum nem tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-312">When creating an object, any property that is assigned undefined value will be skipped and not included in hello created object.</span></span>  
  
 <span data-ttu-id="aacdd-313">Ha egy tömb, bármely elemérték létrehozható, amely hozzá van rendelve **nem definiált** kihagyja és hello létrehozott objektum nem szerepel érték.</span><span class="sxs-lookup"><span data-stu-id="aacdd-313">When creating an array, any element value that is assigned **undefined** value will be skipped and not included in hello created object.</span></span> <span data-ttu-id="aacdd-314">Ennek hatására mellett definiált hello elem tootake helyére oly módon, hogy létre hello tömb fog nem a kihagyott indexeket.</span><span class="sxs-lookup"><span data-stu-id="aacdd-314">This will cause hello next defined element tootake its place in such a way that hello created array will not have skipped indexes.</span></span>  
  
##  <span data-ttu-id="aacdd-315"><a name="bk_operators"></a>Operátorok</span><span class="sxs-lookup"><span data-stu-id="aacdd-315"><a name="bk_operators"></a> Operators</span></span>  
 <span data-ttu-id="aacdd-316">Ez a szakasz ismerteti a támogatott hello operátorok.</span><span class="sxs-lookup"><span data-stu-id="aacdd-316">This section describes hello supported operators.</span></span> <span data-ttu-id="aacdd-317">Minden egyes operátor lehet hozzárendelt tooexactly egy kategóriát.</span><span class="sxs-lookup"><span data-stu-id="aacdd-317">Each operator can be assigned tooexactly one category.</span></span>  
  
 <span data-ttu-id="aacdd-318">Lásd: **operátor kategóriák** részletes, az alábbi táblázatban kezelésére vonatkozó **nem definiált** érték található, a bemeneti értékek és kezelésére vonatkozó értékek nem egyező típusok szemben támasztott követelményeit.</span><span class="sxs-lookup"><span data-stu-id="aacdd-318">See **Operator categories** table below, for details regarding handling of **undefined** values, type requirements for input values and handling of values with not matching types.</span></span>  
  
 <span data-ttu-id="aacdd-319">**Operátor kategóriák:**</span><span class="sxs-lookup"><span data-stu-id="aacdd-319">**Operator categories:**</span></span>  
  
|<span data-ttu-id="aacdd-320">**Kategória**</span><span class="sxs-lookup"><span data-stu-id="aacdd-320">**Category**</span></span>|<span data-ttu-id="aacdd-321">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="aacdd-321">**Details**</span></span>|  
|-|-|  
|<span data-ttu-id="aacdd-322">**aritmetikai**</span><span class="sxs-lookup"><span data-stu-id="aacdd-322">**arithmetic**</span></span>|<span data-ttu-id="aacdd-323">Operátor vár input(s) toobe száma.</span><span class="sxs-lookup"><span data-stu-id="aacdd-323">Operator expects input(s) toobe Number(s).</span></span> <span data-ttu-id="aacdd-324">Kimeneti az a szám.</span><span class="sxs-lookup"><span data-stu-id="aacdd-324">Output is also a Number.</span></span> <span data-ttu-id="aacdd-325">Ha hello bemenetek bármelyike **nem definiált** vagy típustól eltérő, számot, majd hello eredmény **nem definiált**.</span><span class="sxs-lookup"><span data-stu-id="aacdd-325">If any of hello inputs is **undefined** or type other than Number then hello result is **undefined**.</span></span>|  
|<span data-ttu-id="aacdd-326">**Bitenkénti**</span><span class="sxs-lookup"><span data-stu-id="aacdd-326">**bitwise**</span></span>|<span data-ttu-id="aacdd-327">Operátor vár input(s) toobe 32 bites előjeles egész számokat száma.</span><span class="sxs-lookup"><span data-stu-id="aacdd-327">Operator expects input(s) toobe 32-bit signed integer Number(s).</span></span> <span data-ttu-id="aacdd-328">Kimeneti is 32 bites, előjeles egész szám.</span><span class="sxs-lookup"><span data-stu-id="aacdd-328">Output is also 32-bit signed integer Number.</span></span><br /><br /> <span data-ttu-id="aacdd-329">Nem egész értéket a rendszer kerekíti.</span><span class="sxs-lookup"><span data-stu-id="aacdd-329">Any non-integer value will be rounded.</span></span> <span data-ttu-id="aacdd-330">Pozitív értéket lefelé, negatív értékeket kerekíti.</span><span class="sxs-lookup"><span data-stu-id="aacdd-330">Positive value will be rounded down, negative values rounded up.</span></span><br /><br /> <span data-ttu-id="aacdd-331">Bármely érték, amely hello 32 bites egész tartományon kívül esik a két tartozó hexadecimális utolsó 32-bites megtételével konvertálja.</span><span class="sxs-lookup"><span data-stu-id="aacdd-331">Any value that is outside of hello 32-bit integer range will be converted, by taking last 32-bits of its two's complement notation.</span></span><br /><br /> <span data-ttu-id="aacdd-332">Ha hello bemenetek bármelyike **nem definiált** vagy adjon meg másik számot, majd hello eredmény **nem definiált**.</span><span class="sxs-lookup"><span data-stu-id="aacdd-332">If any of hello inputs is **undefined** or type other than Number, then hello result is **undefined**.</span></span><br /><br /> <span data-ttu-id="aacdd-333">**Megjegyzés:** fent viselkedés hello összeegyeztethető JavaScript bitenkénti operátor viselkedését.</span><span class="sxs-lookup"><span data-stu-id="aacdd-333">**Note:** hello above behavior is compatible with JavaScript bitwise operator behavior.</span></span>|  
|<span data-ttu-id="aacdd-334">**logikai**</span><span class="sxs-lookup"><span data-stu-id="aacdd-334">**logical**</span></span>|<span data-ttu-id="aacdd-335">Operátor vár input(s) toobe Boolean(s).</span><span class="sxs-lookup"><span data-stu-id="aacdd-335">Operator expects input(s) toobe Boolean(s).</span></span> <span data-ttu-id="aacdd-336">Kimeneti is olyan logikai érték.</span><span class="sxs-lookup"><span data-stu-id="aacdd-336">Output is also a Boolean.</span></span><br /><span data-ttu-id="aacdd-337">Ha hello bemenetek bármelyike **nem definiált** vagy adjon meg eltérő logikai érték, akkor rendszer hello eredmény **nem definiált**.</span><span class="sxs-lookup"><span data-stu-id="aacdd-337">If any of hello inputs is **undefined** or type other than Boolean, then hello result will be **undefined**.</span></span>|  
|<span data-ttu-id="aacdd-338">**összehasonlítása**</span><span class="sxs-lookup"><span data-stu-id="aacdd-338">**comparison**</span></span>|<span data-ttu-id="aacdd-339">Operátor vár input(s) toohave hello azonos írja be, és nem lehet nem definiált.</span><span class="sxs-lookup"><span data-stu-id="aacdd-339">Operator expects input(s) toohave hello same type and not be undefined.</span></span> <span data-ttu-id="aacdd-340">Olyan logikai érték eredménye.</span><span class="sxs-lookup"><span data-stu-id="aacdd-340">Output is a Boolean.</span></span><br /><br /> <span data-ttu-id="aacdd-341">Ha hello bemenetek bármelyike **nem definiált** vagy hello bemenetek különböző rendelkezik, majd hello eredmény **nem definiált**.</span><span class="sxs-lookup"><span data-stu-id="aacdd-341">If any of hello inputs is **undefined** or hello inputs have different types, then hello result is **undefined**.</span></span><br /><br /> <span data-ttu-id="aacdd-342">Lásd: **összehasonlított értékek rendezési** tábla értékhez rendelés részleteit.</span><span class="sxs-lookup"><span data-stu-id="aacdd-342">See **Ordering of values for comparison** table for value ordering details.</span></span>|  
|<span data-ttu-id="aacdd-343">**karakterlánc**</span><span class="sxs-lookup"><span data-stu-id="aacdd-343">**string**</span></span>|<span data-ttu-id="aacdd-344">Operátor vár input(s) toobe karakterlánc(ok).</span><span class="sxs-lookup"><span data-stu-id="aacdd-344">Operator expects input(s) toobe String(s).</span></span> <span data-ttu-id="aacdd-345">Kimenet: karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="aacdd-345">Output is also a String.</span></span><br /><span data-ttu-id="aacdd-346">Ha hello bemenetek bármelyike **nem definiált** vagy típustól eltérő karakterláncot, majd hello eredmény **nem definiált**.</span><span class="sxs-lookup"><span data-stu-id="aacdd-346">If any of hello inputs is **undefined** or type other than String then hello result is **undefined**.</span></span>|  
  
 <span data-ttu-id="aacdd-347">**Az egyoperandusú operátorral:**</span><span class="sxs-lookup"><span data-stu-id="aacdd-347">**Unary operators:**</span></span>  
  
|<span data-ttu-id="aacdd-348">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="aacdd-348">**Name**</span></span>|<span data-ttu-id="aacdd-349">**Operátor**</span><span class="sxs-lookup"><span data-stu-id="aacdd-349">**Operator**</span></span>|<span data-ttu-id="aacdd-350">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="aacdd-350">**Details**</span></span>|  
|-|-|-|  
|<span data-ttu-id="aacdd-351">**aritmetikai**</span><span class="sxs-lookup"><span data-stu-id="aacdd-351">**arithmetic**</span></span>|+<br /><br /> -|<span data-ttu-id="aacdd-352">Hello számú értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-352">Returns hello number value.</span></span><br /><br /> <span data-ttu-id="aacdd-353">Bitenkénti negálást.</span><span class="sxs-lookup"><span data-stu-id="aacdd-353">Bitwise negation.</span></span> <span data-ttu-id="aacdd-354">Számú értéket ad vissza negated.</span><span class="sxs-lookup"><span data-stu-id="aacdd-354">Returns negated number value.</span></span>|  
|<span data-ttu-id="aacdd-355">**Bitenkénti**</span><span class="sxs-lookup"><span data-stu-id="aacdd-355">**bitwise**</span></span>|~|<span data-ttu-id="aacdd-356">Egyesek komplemens számnak.</span><span class="sxs-lookup"><span data-stu-id="aacdd-356">Ones' complement.</span></span> <span data-ttu-id="aacdd-357">Az érték egy szám kiegészítése adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-357">Returns a complement of a number value.</span></span>|  
|<span data-ttu-id="aacdd-358">**Logikai**</span><span class="sxs-lookup"><span data-stu-id="aacdd-358">**Logical**</span></span>|<span data-ttu-id="aacdd-359">**NEM**</span><span class="sxs-lookup"><span data-stu-id="aacdd-359">**NOT**</span></span>|<span data-ttu-id="aacdd-360">Tagadásának.</span><span class="sxs-lookup"><span data-stu-id="aacdd-360">Negation.</span></span> <span data-ttu-id="aacdd-361">Beolvasása negated logikai érték.</span><span class="sxs-lookup"><span data-stu-id="aacdd-361">Returns negated Boolean value.</span></span>|  
  
 <span data-ttu-id="aacdd-362">**Bináris operátor:**</span><span class="sxs-lookup"><span data-stu-id="aacdd-362">**Binary operators:**</span></span>  
  
|<span data-ttu-id="aacdd-363">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="aacdd-363">**Name**</span></span>|<span data-ttu-id="aacdd-364">**Operátor**</span><span class="sxs-lookup"><span data-stu-id="aacdd-364">**Operator**</span></span>|<span data-ttu-id="aacdd-365">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="aacdd-365">**Details**</span></span>|  
|-|-|-|  
|<span data-ttu-id="aacdd-366">**aritmetikai**</span><span class="sxs-lookup"><span data-stu-id="aacdd-366">**arithmetic**</span></span>|+<br /><br /> -<br /><br /> *<br /><br /> /<br /><br /> %|<span data-ttu-id="aacdd-367">Hozzáadását.</span><span class="sxs-lookup"><span data-stu-id="aacdd-367">Addition.</span></span><br /><br /> <span data-ttu-id="aacdd-368">Kivonás.</span><span class="sxs-lookup"><span data-stu-id="aacdd-368">Subtraction.</span></span><br /><br /> <span data-ttu-id="aacdd-369">Szorzást végezhet.</span><span class="sxs-lookup"><span data-stu-id="aacdd-369">Multiplication.</span></span><br /><br /> <span data-ttu-id="aacdd-370">Osztás.</span><span class="sxs-lookup"><span data-stu-id="aacdd-370">Division.</span></span><br /><br /> <span data-ttu-id="aacdd-371">Modulációs.</span><span class="sxs-lookup"><span data-stu-id="aacdd-371">Modulation.</span></span>|  
|<span data-ttu-id="aacdd-372">**Bitenkénti**</span><span class="sxs-lookup"><span data-stu-id="aacdd-372">**bitwise**</span></span>|<span data-ttu-id="aacdd-373">&#124;</span><span class="sxs-lookup"><span data-stu-id="aacdd-373">&#124;</span></span><br /><br /> &<br /><br /> ^<br /><br /> <<<br /><br /> >><br /><br /> >>>|<span data-ttu-id="aacdd-374">Bitenkénti vagy.</span><span class="sxs-lookup"><span data-stu-id="aacdd-374">Bitwise OR.</span></span><br /><br /> <span data-ttu-id="aacdd-375">Bitenkénti és művelet</span><span class="sxs-lookup"><span data-stu-id="aacdd-375">Bitwise AND.</span></span><br /><br /> <span data-ttu-id="aacdd-376">Bitenkénti kizáró vagy.</span><span class="sxs-lookup"><span data-stu-id="aacdd-376">Bitwise XOR.</span></span><br /><br /> <span data-ttu-id="aacdd-377">Balra Tolást.</span><span class="sxs-lookup"><span data-stu-id="aacdd-377">Left Shift.</span></span><br /><br /> <span data-ttu-id="aacdd-378">Jobbra Tolást.</span><span class="sxs-lookup"><span data-stu-id="aacdd-378">Right Shift.</span></span><br /><br /> <span data-ttu-id="aacdd-379">Nulla-Kitöltés jobbra Tolást.</span><span class="sxs-lookup"><span data-stu-id="aacdd-379">Zero-fill Right Shift.</span></span>|  
|<span data-ttu-id="aacdd-380">**logikai**</span><span class="sxs-lookup"><span data-stu-id="aacdd-380">**logical**</span></span>|<span data-ttu-id="aacdd-381">**ÉS**</span><span class="sxs-lookup"><span data-stu-id="aacdd-381">**AND**</span></span><br /><br /> <span data-ttu-id="aacdd-382">**VAGY**</span><span class="sxs-lookup"><span data-stu-id="aacdd-382">**OR**</span></span>|<span data-ttu-id="aacdd-383">Logikai együtt.</span><span class="sxs-lookup"><span data-stu-id="aacdd-383">Logical conjunction.</span></span> <span data-ttu-id="aacdd-384">Visszaadja **igaz** , ha mindkét argumentuma **igaz**, adja vissza **hamis** ellenkező esetben.</span><span class="sxs-lookup"><span data-stu-id="aacdd-384">Returns **true** if both arguments are **true**, returns **false** otherwise.</span></span><br /><br /> <span data-ttu-id="aacdd-385">Logikai együtt.</span><span class="sxs-lookup"><span data-stu-id="aacdd-385">Logical conjunction.</span></span> <span data-ttu-id="aacdd-386">Visszaadja **igaz** , ha mindkét argumentuma **igaz**, adja vissza **hamis** ellenkező esetben.</span><span class="sxs-lookup"><span data-stu-id="aacdd-386">Returns **true** if both arguments are **true**, returns **false** otherwise.</span></span>|  
|<span data-ttu-id="aacdd-387">**összehasonlítása**</span><span class="sxs-lookup"><span data-stu-id="aacdd-387">**comparison**</span></span>|**=**<br /><br /> <span data-ttu-id="aacdd-388">**!=, <>**</span><span class="sxs-lookup"><span data-stu-id="aacdd-388">**!=, <>**</span></span><br /><br /> **>**<br /><br /> **>=**<br /><br /> **<**<br /><br /> **<=**<br /><br /> <span data-ttu-id="aacdd-389">**??**</span><span class="sxs-lookup"><span data-stu-id="aacdd-389">**??**</span></span>|<span data-ttu-id="aacdd-390">Egyenlő.</span><span class="sxs-lookup"><span data-stu-id="aacdd-390">Equals.</span></span> <span data-ttu-id="aacdd-391">Visszaadja **igaz** Ha az argumentum értéke, akkor adja vissza **hamis** egyéb.</span><span class="sxs-lookup"><span data-stu-id="aacdd-391">Returns **true** if arguments are equal, returns **false** otherwise.</span></span><br /><br /> <span data-ttu-id="aacdd-392">Nem egyenlő.</span><span class="sxs-lookup"><span data-stu-id="aacdd-392">Not equal to.</span></span> <span data-ttu-id="aacdd-393">Visszaadja **igaz** Ha az argumentum nem egyenlő, adja vissza **hamis** egyéb.</span><span class="sxs-lookup"><span data-stu-id="aacdd-393">Returns **true** if arguments are not equal, returns **false** otherwise.</span></span><br /><br /> <span data-ttu-id="aacdd-394">Nagyobb, mint.</span><span class="sxs-lookup"><span data-stu-id="aacdd-394">Greater Than.</span></span> <span data-ttu-id="aacdd-395">Beolvasása **igaz** első argumentum értéke nagyobb, mint a második hello, ha vissza **hamis** más módon.</span><span class="sxs-lookup"><span data-stu-id="aacdd-395">Returns **true** if first argument is greater than hello second one, return **false** otherwise.</span></span><br /><br /> <span data-ttu-id="aacdd-396">Nagyobb vagy egyenlő.</span><span class="sxs-lookup"><span data-stu-id="aacdd-396">Greater Than or Equal To.</span></span> <span data-ttu-id="aacdd-397">Beolvasása **igaz** első argumentum nagyobb vagy egyenlő toohello másikat, ha vissza **hamis** más módon.</span><span class="sxs-lookup"><span data-stu-id="aacdd-397">Returns **true** if first argument is greater than or equal toohello second one, return **false** otherwise.</span></span><br /><br /> <span data-ttu-id="aacdd-398">Kisebb, mint.</span><span class="sxs-lookup"><span data-stu-id="aacdd-398">Less Than.</span></span> <span data-ttu-id="aacdd-399">Beolvasása **igaz** első argumentum értéke kisebb, mint a második hello, ha vissza **hamis** más módon.</span><span class="sxs-lookup"><span data-stu-id="aacdd-399">Returns **true** if first argument is less than hello second one, return **false** otherwise.</span></span><br /><br /> <span data-ttu-id="aacdd-400">Kisebb vagy egyenlő, mint.</span><span class="sxs-lookup"><span data-stu-id="aacdd-400">Less Than or Equal To.</span></span> <span data-ttu-id="aacdd-401">Beolvasása **igaz** első argumentum értéke kisebb vagy egyenlő, mint ha toohello második egy visszatérési **hamis** más módon.</span><span class="sxs-lookup"><span data-stu-id="aacdd-401">Returns **true** if first argument is less than or equal toohello second one, return **false** otherwise.</span></span><br /><br /> <span data-ttu-id="aacdd-402">A Coalesce.</span><span class="sxs-lookup"><span data-stu-id="aacdd-402">Coalesce.</span></span> <span data-ttu-id="aacdd-403">Értéket ad vissza hello második argumentum kötelező, ha hello első argumentum egy **nem definiált** érték.</span><span class="sxs-lookup"><span data-stu-id="aacdd-403">Returns hello second argument if hello first argument is an **undefined** value.</span></span>|  
|<span data-ttu-id="aacdd-404">**Karakterlánc**</span><span class="sxs-lookup"><span data-stu-id="aacdd-404">**String**</span></span>|<span data-ttu-id="aacdd-405">**&#124;&#124;**</span><span class="sxs-lookup"><span data-stu-id="aacdd-405">**&#124;&#124;**</span></span>|<span data-ttu-id="aacdd-406">Kapott.</span><span class="sxs-lookup"><span data-stu-id="aacdd-406">Concatenation.</span></span> <span data-ttu-id="aacdd-407">Mindkét argumentumot összefűzése adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-407">Returns a concatenation of both arguments.</span></span>|  
  
 <span data-ttu-id="aacdd-408">**Ternáris kezelők:**</span><span class="sxs-lookup"><span data-stu-id="aacdd-408">**Ternary operators:**</span></span>  
  
|<span data-ttu-id="aacdd-409">Ternáris operátor</span><span class="sxs-lookup"><span data-stu-id="aacdd-409">Ternary operator</span></span>|<span data-ttu-id="aacdd-410">?</span><span class="sxs-lookup"><span data-stu-id="aacdd-410">?</span></span>|<span data-ttu-id="aacdd-411">Értéket ad vissza hello a második argumentum kötelező, ha hello első argumentum értéke túl**igaz**; ellenkező esetben vissza hello harmadik argumentum.</span><span class="sxs-lookup"><span data-stu-id="aacdd-411">Returns hello second argument if hello first argument evaluates too**true**; return hello third argument otherwise.</span></span>|  
|-|-|-|  
  
 <span data-ttu-id="aacdd-412">**Az összehasonlított értékek sorrendje**</span><span class="sxs-lookup"><span data-stu-id="aacdd-412">**Ordering of values for comparison**</span></span>  
  
|<span data-ttu-id="aacdd-413">**Típus**</span><span class="sxs-lookup"><span data-stu-id="aacdd-413">**Type**</span></span>|<span data-ttu-id="aacdd-414">**Értékek sorrendje**</span><span class="sxs-lookup"><span data-stu-id="aacdd-414">**Values order**</span></span>|  
|-|-|  
|<span data-ttu-id="aacdd-415">**Nincs definiálva**</span><span class="sxs-lookup"><span data-stu-id="aacdd-415">**Undefined**</span></span>|<span data-ttu-id="aacdd-416">Nem hasonlítható össze.</span><span class="sxs-lookup"><span data-stu-id="aacdd-416">Not comparable.</span></span>|  
|<span data-ttu-id="aacdd-417">**NULL értékű**</span><span class="sxs-lookup"><span data-stu-id="aacdd-417">**Null**</span></span>|<span data-ttu-id="aacdd-418">Egyetlen érték: **null értékű**</span><span class="sxs-lookup"><span data-stu-id="aacdd-418">Single value: **null**</span></span>|  
|<span data-ttu-id="aacdd-419">**Szám**</span><span class="sxs-lookup"><span data-stu-id="aacdd-419">**Number**</span></span>|<span data-ttu-id="aacdd-420">Természetes valós szám.</span><span class="sxs-lookup"><span data-stu-id="aacdd-420">Natural real number.</span></span><br /><br /> <span data-ttu-id="aacdd-421">Negatív végtelen értéke kisebb, mint bármely más számértéket.</span><span class="sxs-lookup"><span data-stu-id="aacdd-421">Negative Infinity value is smaller than any other Number value.</span></span><br /><br /> <span data-ttu-id="aacdd-422">Pozitív végtelen értéke nagyobb, mint bármely más számértéket. **NaN** értéke nem hasonlítható össze.</span><span class="sxs-lookup"><span data-stu-id="aacdd-422">Positive Infinity value is larger than any other Number value.**NaN** value is not comparable.</span></span> <span data-ttu-id="aacdd-423">Összehasonlítva az **NaN** eredményez **nem definiált** érték.</span><span class="sxs-lookup"><span data-stu-id="aacdd-423">Comparing with **NaN** will result in **undefined** value.</span></span>|  
|<span data-ttu-id="aacdd-424">**Karakterlánc**</span><span class="sxs-lookup"><span data-stu-id="aacdd-424">**String**</span></span>|<span data-ttu-id="aacdd-425">Lexicographical sorrendje.</span><span class="sxs-lookup"><span data-stu-id="aacdd-425">Lexicographical order.</span></span>|  
|<span data-ttu-id="aacdd-426">**A tömb**</span><span class="sxs-lookup"><span data-stu-id="aacdd-426">**Array**</span></span>|<span data-ttu-id="aacdd-427">Nincs rendezést, de egyenlő.</span><span class="sxs-lookup"><span data-stu-id="aacdd-427">No ordering, but equitable.</span></span>|  
|<span data-ttu-id="aacdd-428">**Objektum**</span><span class="sxs-lookup"><span data-stu-id="aacdd-428">**Object**</span></span>|<span data-ttu-id="aacdd-429">Nincs rendezést, de egyenlő.</span><span class="sxs-lookup"><span data-stu-id="aacdd-429">No ordering, but equitable.</span></span>|  
  
 <span data-ttu-id="aacdd-430">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="aacdd-430">**Remarks**</span></span>  
  
 <span data-ttu-id="aacdd-431">Az Azure Cosmos Adatbázisba hello típusú értékeket gyakran nem ismert lekéréséig azokat ténylegesen hello adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="aacdd-431">In Azure Cosmos DB, hello types of values are often not known until they are actually retrieved from hello database.</span></span> <span data-ttu-id="aacdd-432">A sorrend toosupport hatékony végrehajtásának lekérdezések hello operátorok többsége a szigorú szemben támasztott követelményeit.</span><span class="sxs-lookup"><span data-stu-id="aacdd-432">In order toosupport efficient execution of queries, most of hello operators have strict type requirements.</span></span> <span data-ttu-id="aacdd-433">Operátorok önmagában is ne hajtsa végre implicit konverzió.</span><span class="sxs-lookup"><span data-stu-id="aacdd-433">Also operators by themselves do not perform implicit conversions.</span></span>  
  
 <span data-ttu-id="aacdd-434">Ez azt jelenti, hogy a lekérdezés, például: válasszon * a ROOT r WHERE r.Age = 21 csak tulajdonság kora egyenlő toohello szám 21-dokumentumok adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-434">This means that a query like: SELECT * FROM ROOT r WHERE r.Age = 21 will only return documents with property Age equal toohello number 21.</span></span> <span data-ttu-id="aacdd-435">A tulajdonság kora egyenlő toohello karakterlánc "21" vagy "0021" hello karakterlánc nem fog egyezni, "21" hello kifejezésként = 21 tooundefined értékelődik ki.</span><span class="sxs-lookup"><span data-stu-id="aacdd-435">Documents with property Age equal toohello string "21" or hello string "0021" will not match, as hello expression "21" = 21 evaluates tooundefined.</span></span> <span data-ttu-id="aacdd-436">Ez lehetővé teszi a hatékonyabb felhasználása indexek, mert hello keresés egy adott érték (azaz számú 21) gyorsabb, mint lehetséges megegyezik (21 szám vagy karakterlánc "21", "021", "21.0"...) határozatlan számú keresése.</span><span class="sxs-lookup"><span data-stu-id="aacdd-436">This allows for a better use of indexes, because hello lookup of a specific value (i.e. number 21) is faster than search for indefinite number of potential matches (i.e. number 21 or strings "21", "021", "21.0" …).</span></span> <span data-ttu-id="aacdd-437">Ez eltér hogyan értékeli ki a JavaScript a felügyelői eltérő típusú értékeket.</span><span class="sxs-lookup"><span data-stu-id="aacdd-437">This is different from how JavaScript evaluates operators on values of different types.</span></span>  
  
 <span data-ttu-id="aacdd-438">**Tömbök és objektumok egyenlőség és összehasonlítása**</span><span class="sxs-lookup"><span data-stu-id="aacdd-438">**Arrays and objects equality and comparison**</span></span>  
  
 <span data-ttu-id="aacdd-439">A tartomány operátorral egymáshoz csatolt tömb vagy objektum értékek összehasonlításával (>, > =, <, < =) eredményez, mivel nem az objektum és tömb meghatározásának sorrendje nincs definiálva.</span><span class="sxs-lookup"><span data-stu-id="aacdd-439">Comparing of Array or Object values using range operators (>, >=, <, <=) will result in undefined as there is not order defined on Object or Array values.</span></span> <span data-ttu-id="aacdd-440">Azonban a egyenlőség operátorral egymáshoz csatolt (=,! =, <>) támogatott és értékek szerkezetileg össze.</span><span class="sxs-lookup"><span data-stu-id="aacdd-440">However using equality/inequality operators (=, !=, <>) is supported and values will be compared structurally.</span></span>  
  
 <span data-ttu-id="aacdd-441">Tömbök azonosak, ha mindkét értéktömbök azonos számú elemből állnak, és megfelelő pozíciók elemekben is egyenlő.</span><span class="sxs-lookup"><span data-stu-id="aacdd-441">Arrays are equal if both arrays have same number of elements and elements at matching positions are also equal.</span></span> <span data-ttu-id="aacdd-442">Ha nincs megadva, hello eredménye array összehasonlítása bármely két elem összehasonlításával eredményez nincs definiálva.</span><span class="sxs-lookup"><span data-stu-id="aacdd-442">If comparing any pair of elements results in undefined, hello result of array comparison is undefined.</span></span>  
  
 <span data-ttu-id="aacdd-443">Objektumok azonosak, ha mindkét objektum definiált azonos jellemzőkkel rendelkezik, és a megfelelő tulajdonságainak értékei is egyenlő.</span><span class="sxs-lookup"><span data-stu-id="aacdd-443">Objects are equal if both objects have same properties defined, and if values of matching properties are also equal.</span></span> <span data-ttu-id="aacdd-444">Ha bármely pár tulajdonságérték összehasonlításával objektum összehasonlítás eredménye nem definiált, hello nincs definiálva.</span><span class="sxs-lookup"><span data-stu-id="aacdd-444">If comparing any pair of property values results in undefined, hello result of object comparison is undefined.</span></span>  
  
##  <span data-ttu-id="aacdd-445"><a name="bk_constants"></a>Állandók</span><span class="sxs-lookup"><span data-stu-id="aacdd-445"><a name="bk_constants"></a> Constants</span></span>  
 <span data-ttu-id="aacdd-446">Egy konstans, más néven szövegkonstans vagy skaláris, egy meghatározott értéket jelölő szimbólumot.</span><span class="sxs-lookup"><span data-stu-id="aacdd-446">A constant, also known as a literal or a scalar value, is a symbol that represents a specific data value.</span></span> <span data-ttu-id="aacdd-447">egy konstans hello formátuma hello adattípus hello érték azt jelenti, hogy függ.</span><span class="sxs-lookup"><span data-stu-id="aacdd-447">hello format of a constant depends on hello data type of hello value it represents.</span></span>  
  
 <span data-ttu-id="aacdd-448">**Skaláris adattípusokat támogatja:**</span><span class="sxs-lookup"><span data-stu-id="aacdd-448">**Supported scalar data types:**</span></span>  
  
|<span data-ttu-id="aacdd-449">**Típus**</span><span class="sxs-lookup"><span data-stu-id="aacdd-449">**Type**</span></span>|<span data-ttu-id="aacdd-450">**Értékek sorrendje**</span><span class="sxs-lookup"><span data-stu-id="aacdd-450">**Values order**</span></span>|  
|-|-|  
|<span data-ttu-id="aacdd-451">**Nincs definiálva**</span><span class="sxs-lookup"><span data-stu-id="aacdd-451">**Undefined**</span></span>|<span data-ttu-id="aacdd-452">Egyetlen érték: **nincs megadva**</span><span class="sxs-lookup"><span data-stu-id="aacdd-452">Single value: **undefined**</span></span>|  
|<span data-ttu-id="aacdd-453">**NULL értékű**</span><span class="sxs-lookup"><span data-stu-id="aacdd-453">**Null**</span></span>|<span data-ttu-id="aacdd-454">Egyetlen érték: **null értékű**</span><span class="sxs-lookup"><span data-stu-id="aacdd-454">Single value: **null**</span></span>|  
|<span data-ttu-id="aacdd-455">**Logikai érték**</span><span class="sxs-lookup"><span data-stu-id="aacdd-455">**Boolean**</span></span>|<span data-ttu-id="aacdd-456">Értékek: **hamis**, **igaz**.</span><span class="sxs-lookup"><span data-stu-id="aacdd-456">Values: **false**, **true**.</span></span>|  
|<span data-ttu-id="aacdd-457">**Szám**</span><span class="sxs-lookup"><span data-stu-id="aacdd-457">**Number**</span></span>|<span data-ttu-id="aacdd-458">Egy kétszeres pontosságú lebegőpontos számnál, szabványos IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="aacdd-458">A double-precision floating-point number, IEEE 754 standard.</span></span>|  
|<span data-ttu-id="aacdd-459">**Karakterlánc**</span><span class="sxs-lookup"><span data-stu-id="aacdd-459">**String**</span></span>|<span data-ttu-id="aacdd-460">Nulla vagy több Unicode-karaktereket sorozata.</span><span class="sxs-lookup"><span data-stu-id="aacdd-460">A sequence of zero or more Unicode characters.</span></span> <span data-ttu-id="aacdd-461">Karakterláncok egyetlen vagy dupla idézőjelek között kell foglalni.</span><span class="sxs-lookup"><span data-stu-id="aacdd-461">Strings must be enclosed in single or double quotes.</span></span>|  
|<span data-ttu-id="aacdd-462">**A tömb**</span><span class="sxs-lookup"><span data-stu-id="aacdd-462">**Array**</span></span>|<span data-ttu-id="aacdd-463">Nulla vagy több elemek sorrendjét.</span><span class="sxs-lookup"><span data-stu-id="aacdd-463">A sequence of zero or more elements.</span></span> <span data-ttu-id="aacdd-464">Minden elem meghatározatlan kivételével minden skaláris adattípusú érték lehet.</span><span class="sxs-lookup"><span data-stu-id="aacdd-464">Each element can be a value of any scalar data type, except Undefined.</span></span>|  
|<span data-ttu-id="aacdd-465">**Objektum**</span><span class="sxs-lookup"><span data-stu-id="aacdd-465">**Object**</span></span>|<span data-ttu-id="aacdd-466">Egy nulla vagy több név/érték párok rendezetlen készlete.</span><span class="sxs-lookup"><span data-stu-id="aacdd-466">An unordered set of zero or more name/value pairs.</span></span> <span data-ttu-id="aacdd-467">Értéke a Unicode-karakterláncot, kivéve értéke lehet bármely skaláris adattípusú, **meghatározatlan**.</span><span class="sxs-lookup"><span data-stu-id="aacdd-467">Name is a Unicode string, value can be of any scalar data type, except **Undefined**.</span></span>|  
  
 <span data-ttu-id="aacdd-468">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-468">**Syntax**</span></span>  
  
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
  
 <span data-ttu-id="aacdd-469">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-469">**Arguments**</span></span>  
  
1.  `<undefined_constant>; undefined`  
  
     <span data-ttu-id="aacdd-470">Meghatározatlan típusú érték nincs definiálva jelöli.</span><span class="sxs-lookup"><span data-stu-id="aacdd-470">Represents undefined value of type Undefined.</span></span>  
  
2.  `<null_constant>; null`  
  
     <span data-ttu-id="aacdd-471">Jelöli **null** típusú érték **Null**.</span><span class="sxs-lookup"><span data-stu-id="aacdd-471">Represents **null** value of type **Null**.</span></span>  
  
3.  `<boolean_constant>`  
  
     <span data-ttu-id="aacdd-472">Logikai érték típusú konstans jelöli.</span><span class="sxs-lookup"><span data-stu-id="aacdd-472">Represents constant of type Boolean.</span></span>  
  
4.  `false`  
  
     <span data-ttu-id="aacdd-473">Jelöli **hamis** logikai típusú érték.</span><span class="sxs-lookup"><span data-stu-id="aacdd-473">Represents **false** value of type Boolean.</span></span>  
  
5.  `true`  
  
     <span data-ttu-id="aacdd-474">Jelöli **igaz** logikai típusú érték.</span><span class="sxs-lookup"><span data-stu-id="aacdd-474">Represents **true** value of type Boolean.</span></span>  
  
6.  `<number_constant>`  
  
     <span data-ttu-id="aacdd-475">Egy konstans jelöli.</span><span class="sxs-lookup"><span data-stu-id="aacdd-475">Represents a constant.</span></span>  
  
7.  `decimal_literal`  
  
     <span data-ttu-id="aacdd-476">Decimális literálok képviselt decimális jelöléssel, vagy tudományos jelölés használatával számok.</span><span class="sxs-lookup"><span data-stu-id="aacdd-476">Decimal literals are numbers represented using either decimal notation, or scientific notation.</span></span>  
  
8.  `hexadecimal_literal`  
  
     <span data-ttu-id="aacdd-477">Hexadecimális literálok értékét "0 x", egy vagy több hexadecimális számjegy követ előtag számok.</span><span class="sxs-lookup"><span data-stu-id="aacdd-477">Hexadecimal literals are numbers represented using prefix '0x' followed by one or more hexadecimal digits.</span></span>  
  
9. `<string_constant>`  
  
     <span data-ttu-id="aacdd-478">Egy karakterlánc típusú konstans jelöli.</span><span class="sxs-lookup"><span data-stu-id="aacdd-478">Represents a constant of type String.</span></span>  
  
10. `string _literal`  
  
     <span data-ttu-id="aacdd-479">A szövegkonstansok olyan Unicode karakterláncok sorozatát nulla vagy több Unicode-karaktereket vagy escape-karaktersorozatokat.</span><span class="sxs-lookup"><span data-stu-id="aacdd-479">String literals are Unicode strings represented by a sequence of zero or more Unicode characters or escape sequences.</span></span> <span data-ttu-id="aacdd-480">A szövegkonstansok vannak szimpla zárójelek között (aposztróf: ") vagy dupla idézőjel (idézőjel:").</span><span class="sxs-lookup"><span data-stu-id="aacdd-480">String literals are enclosed in single quotes (apostrophe: ' ) or double quotes (quotation mark: ").</span></span>  
  
 <span data-ttu-id="aacdd-481">Következő escape-karaktersorozatokat engedélyezettek:</span><span class="sxs-lookup"><span data-stu-id="aacdd-481">Following escape sequences are allowed:</span></span>  
  
|<span data-ttu-id="aacdd-482">**Escape-karaktersorozatot**</span><span class="sxs-lookup"><span data-stu-id="aacdd-482">**Escape sequence**</span></span>|<span data-ttu-id="aacdd-483">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="aacdd-483">**Description**</span></span>|<span data-ttu-id="aacdd-484">**Unicode-karakter**</span><span class="sxs-lookup"><span data-stu-id="aacdd-484">**Unicode character**</span></span>|  
|-|-|-|  
|<span data-ttu-id="aacdd-485">\\'</span><span class="sxs-lookup"><span data-stu-id="aacdd-485">\\'</span></span>|<span data-ttu-id="aacdd-486">aposztróf (')</span><span class="sxs-lookup"><span data-stu-id="aacdd-486">apostrophe (')</span></span>|<span data-ttu-id="aacdd-487">U + 0027</span><span class="sxs-lookup"><span data-stu-id="aacdd-487">U+0027</span></span>|  
|<span data-ttu-id="aacdd-488">\\"</span><span class="sxs-lookup"><span data-stu-id="aacdd-488">\\"</span></span>|<span data-ttu-id="aacdd-489">az idézőjel (")</span><span class="sxs-lookup"><span data-stu-id="aacdd-489">quotation mark (")</span></span>|<span data-ttu-id="aacdd-490">U + 0022</span><span class="sxs-lookup"><span data-stu-id="aacdd-490">U+0022</span></span>|  
|\\\|<span data-ttu-id="aacdd-491">fordított solidus (\\)</span><span class="sxs-lookup"><span data-stu-id="aacdd-491">reverse solidus (\\)</span></span>|<span data-ttu-id="aacdd-492">U + 005C</span><span class="sxs-lookup"><span data-stu-id="aacdd-492">U+005C</span></span>|  
|\\/|<span data-ttu-id="aacdd-493">solidus (/)</span><span class="sxs-lookup"><span data-stu-id="aacdd-493">solidus (/)</span></span>|<span data-ttu-id="aacdd-494">U + 002F</span><span class="sxs-lookup"><span data-stu-id="aacdd-494">U+002F</span></span>|  
|<span data-ttu-id="aacdd-495">\b</span><span class="sxs-lookup"><span data-stu-id="aacdd-495">\b</span></span>|<span data-ttu-id="aacdd-496">BACKSPACE</span><span class="sxs-lookup"><span data-stu-id="aacdd-496">backspace</span></span>|<span data-ttu-id="aacdd-497">U + 0008</span><span class="sxs-lookup"><span data-stu-id="aacdd-497">U+0008</span></span>|  
|<span data-ttu-id="aacdd-498">\f</span><span class="sxs-lookup"><span data-stu-id="aacdd-498">\f</span></span>|<span data-ttu-id="aacdd-499">Lapdobás</span><span class="sxs-lookup"><span data-stu-id="aacdd-499">form feed</span></span>|<span data-ttu-id="aacdd-500">U + 000C</span><span class="sxs-lookup"><span data-stu-id="aacdd-500">U+000C</span></span>|  
|\n|<span data-ttu-id="aacdd-501">soremelés</span><span class="sxs-lookup"><span data-stu-id="aacdd-501">line feed</span></span>|<span data-ttu-id="aacdd-502">U + 000A</span><span class="sxs-lookup"><span data-stu-id="aacdd-502">U+000A</span></span>|  
|<span data-ttu-id="aacdd-503">\r</span><span class="sxs-lookup"><span data-stu-id="aacdd-503">\r</span></span>|<span data-ttu-id="aacdd-504">kocsivissza</span><span class="sxs-lookup"><span data-stu-id="aacdd-504">carriage return</span></span>|<span data-ttu-id="aacdd-505">U + 000D</span><span class="sxs-lookup"><span data-stu-id="aacdd-505">U+000D</span></span>|  
|<span data-ttu-id="aacdd-506">\t</span><span class="sxs-lookup"><span data-stu-id="aacdd-506">\t</span></span>|<span data-ttu-id="aacdd-507">Lap</span><span class="sxs-lookup"><span data-stu-id="aacdd-507">tab</span></span>|<span data-ttu-id="aacdd-508">U + 0009</span><span class="sxs-lookup"><span data-stu-id="aacdd-508">U+0009</span></span>|  
|<span data-ttu-id="aacdd-509">\uXXXX</span><span class="sxs-lookup"><span data-stu-id="aacdd-509">\uXXXX</span></span>|<span data-ttu-id="aacdd-510">4 hexadecimális számjegy által megadott Unicode-karakter.</span><span class="sxs-lookup"><span data-stu-id="aacdd-510">A Unicode character defined by 4 hexadecimal digits.</span></span>|<span data-ttu-id="aacdd-511">U + XXXX</span><span class="sxs-lookup"><span data-stu-id="aacdd-511">U+XXXX</span></span>|  
  
##  <span data-ttu-id="aacdd-512"><a name="bk_query_perf_guidelines"></a>Lekérdezés teljesítmény irányelvek</span><span class="sxs-lookup"><span data-stu-id="aacdd-512"><a name="bk_query_perf_guidelines"></a> Query performance guidelines</span></span>  
 <span data-ttu-id="aacdd-513">Ahhoz, hogy a lekérdezés toobe egy nagy méretű gyűjtemény hatékonyan hajtotta végre akkor az szűrőket, amelyek egy vagy több indexek keresztül szolgáltatható kell használhat.</span><span class="sxs-lookup"><span data-stu-id="aacdd-513">In order for a query toobe executed efficiently for a large collection, it should use filters which can be served through one or more indexes.</span></span>  
  
 <span data-ttu-id="aacdd-514">a következő szűrők hello veszi figyelembe a keresési index:</span><span class="sxs-lookup"><span data-stu-id="aacdd-514">hello following filters will be considered for index lookup:</span></span>  
  
-   <span data-ttu-id="aacdd-515">Egyenlő operátor (=) használata a dokumentum az elérésiút-kifejezés és egy konstans.</span><span class="sxs-lookup"><span data-stu-id="aacdd-515">Use equality operator ( = ) with a document path expression and a constant.</span></span>  
  
-   <span data-ttu-id="aacdd-516">Tartomány operátorokkal (<, \<=, >, > =) számú állandók és elérésiút-kifejezésben.</span><span class="sxs-lookup"><span data-stu-id="aacdd-516">Use range operators (<, \<=, >, >=) with a document path expression and number constants.</span></span>  
  
-   <span data-ttu-id="aacdd-517">A dokumentum az elérésiút-kifejezés bármely kifejezés végzi: azonosítja a hivatkozott hello adatbázis gyűjteményből hello dokumentumok állandó elérési útját jelöli.</span><span class="sxs-lookup"><span data-stu-id="aacdd-517">Document path expression stands for any expression which identifies a constant path in hello documents from hello referenced database collection.</span></span>  
  
 <span data-ttu-id="aacdd-518">**A dokumentum az elérésiút-kifejezés**</span><span class="sxs-lookup"><span data-stu-id="aacdd-518">**Document path expression**</span></span>  
  
 <span data-ttu-id="aacdd-519">Dokumentum elérési kifejezések kifejezések szerepelnek, amelyek egy elérési utat az indexelő tulajdonság vagy tömb vizsgáztatók egy dokumentumot, az adatbázis gyűjtemény dokumentumok érkező keresztül.</span><span class="sxs-lookup"><span data-stu-id="aacdd-519">Document path expressions are expressions that a path of property or array indexer assessors over a document coming from database collection documents.</span></span> <span data-ttu-id="aacdd-520">Az elérési út lehet közvetlenül a hello dokumentumok hello adatbázis gyűjtemény szűrőben hivatkozott értékek használt tooidentify hello helyét.</span><span class="sxs-lookup"><span data-stu-id="aacdd-520">This path can be used tooidentify hello location of values referenced in a filter directly within hello documents in hello database collection.</span></span>  
  
 <span data-ttu-id="aacdd-521">Egy kifejezés toobe figyelembe veendő elérésiút-kifejezésben, a következőket:</span><span class="sxs-lookup"><span data-stu-id="aacdd-521">For an expression toobe considered a document path expression, it should:</span></span>  
  
1.  <span data-ttu-id="aacdd-522">Hivatkozás hello gyűjtemény legfelső szintű közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="aacdd-522">Reference hello collection root directly.</span></span>  
  
2.  <span data-ttu-id="aacdd-523">Hivatkozási tulajdonság vagy állandó tömb indexelő néhány dokumentum elérési út kifejezés</span><span class="sxs-lookup"><span data-stu-id="aacdd-523">Reference property or constant array indexer of some document path expression</span></span>  
  
3.  <span data-ttu-id="aacdd-524">Az alias, amely az egyes dokumentum elérésiút-kifejezés hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="aacdd-524">Reference an alias, which represents some document path expression.</span></span>  
  
     <span data-ttu-id="aacdd-525">**Szintaxis konvenciók**</span><span class="sxs-lookup"><span data-stu-id="aacdd-525">**Syntax conventions**</span></span>  
  
     <span data-ttu-id="aacdd-526">a következő táblázat hello hello jelölések toodescribe szintaxist hello DocumentDB API lekérdezés nyelvi dokumentáció ismerteti.</span><span class="sxs-lookup"><span data-stu-id="aacdd-526">hello following table describes hello conventions used toodescribe syntax in hello DocumentDB API Query Language reference.</span></span>  
  
    |<span data-ttu-id="aacdd-527">**Egyezmény**</span><span class="sxs-lookup"><span data-stu-id="aacdd-527">**Convention**</span></span>|<span data-ttu-id="aacdd-528">**A használt**</span><span class="sxs-lookup"><span data-stu-id="aacdd-528">**Used for**</span></span>|  
    |-|-|    
    |<span data-ttu-id="aacdd-529">NAGYBETŰK</span><span class="sxs-lookup"><span data-stu-id="aacdd-529">UPPERCASE</span></span>|<span data-ttu-id="aacdd-530">Nem betűérzékeny kulcsszavakat.</span><span class="sxs-lookup"><span data-stu-id="aacdd-530">Case-insensitive keywords.</span></span>|  
    |<span data-ttu-id="aacdd-531">kisbetűk</span><span class="sxs-lookup"><span data-stu-id="aacdd-531">lowercase</span></span>|<span data-ttu-id="aacdd-532">Kis-és nagybetűket kulcsszavakat.</span><span class="sxs-lookup"><span data-stu-id="aacdd-532">Case-sensitive keywords.</span></span>|  
    |<span data-ttu-id="aacdd-533">\<nonterminal ></span><span class="sxs-lookup"><span data-stu-id="aacdd-533">\<nonterminal></span></span>|<span data-ttu-id="aacdd-534">Nem, külön definiált.</span><span class="sxs-lookup"><span data-stu-id="aacdd-534">Nonterminal, defined separately.</span></span>|  
    |<span data-ttu-id="aacdd-535">\<nonterminal >:: =</span><span class="sxs-lookup"><span data-stu-id="aacdd-535">\<nonterminal> ::=</span></span>|<span data-ttu-id="aacdd-536">Hello nonterminal szintaxis meghatározása.</span><span class="sxs-lookup"><span data-stu-id="aacdd-536">Syntax definition of hello nonterminal.</span></span>|  
    |<span data-ttu-id="aacdd-537">other_terminal</span><span class="sxs-lookup"><span data-stu-id="aacdd-537">other_terminal</span></span>|<span data-ttu-id="aacdd-538">Terminálszolgáltatások (jogkivonat) részletesen szavakat.</span><span class="sxs-lookup"><span data-stu-id="aacdd-538">Terminal (token), described in detail in words.</span></span>|  
    |<span data-ttu-id="aacdd-539">Azonosítója</span><span class="sxs-lookup"><span data-stu-id="aacdd-539">identifier</span></span>|<span data-ttu-id="aacdd-540">Azonosítója.</span><span class="sxs-lookup"><span data-stu-id="aacdd-540">Identifier.</span></span> <span data-ttu-id="aacdd-541">Lehetővé teszi, hogy a következő karaktereket csak: a – z A – Z 0 – 9 _First karakter nem lehet egy számjegy.</span><span class="sxs-lookup"><span data-stu-id="aacdd-541">Allows following characters only: a-z A-Z 0-9 _First character cannot be a digit.</span></span>|  
    |<span data-ttu-id="aacdd-542">"karakterlánc"</span><span class="sxs-lookup"><span data-stu-id="aacdd-542">"string"</span></span>|<span data-ttu-id="aacdd-543">Idézőjelek közé zárt karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="aacdd-543">Quoted string.</span></span> <span data-ttu-id="aacdd-544">Lehetővé teszi, hogy minden érvényes karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="aacdd-544">Allows any valid string.</span></span> <span data-ttu-id="aacdd-545">Tekintse meg a string_literal leírása.</span><span class="sxs-lookup"><span data-stu-id="aacdd-545">See description of a string_literal.</span></span>|  
    |<span data-ttu-id="aacdd-546">"szimbólum"</span><span class="sxs-lookup"><span data-stu-id="aacdd-546">'symbol'</span></span>|<span data-ttu-id="aacdd-547">Hello szintaxis részét képező literális szimbólum.</span><span class="sxs-lookup"><span data-stu-id="aacdd-547">Literal symbol which is part of hello syntax.</span></span>|  
    |<span data-ttu-id="aacdd-548">&#124; (a függőleges vonal)</span><span class="sxs-lookup"><span data-stu-id="aacdd-548">&#124; (vertical bar)</span></span>|<span data-ttu-id="aacdd-549">Alternatívák szintaxis elemekhez.</span><span class="sxs-lookup"><span data-stu-id="aacdd-549">Alternatives for syntax items.</span></span> <span data-ttu-id="aacdd-550">Csak megadott hello elemek egyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="aacdd-550">You can use only one of hello items specified.</span></span>|  
    |<span data-ttu-id="aacdd-551">[] /(brackets)</span><span class="sxs-lookup"><span data-stu-id="aacdd-551">[ ] /(brackets)</span></span>|<span data-ttu-id="aacdd-552">Zárójelek közé egy vagy több választható elemek.</span><span class="sxs-lookup"><span data-stu-id="aacdd-552">Brackets enclose one or more optional items.</span></span>|  
    |<span data-ttu-id="aacdd-553">[,.. .n]</span><span class="sxs-lookup"><span data-stu-id="aacdd-553">[ ,...n ]</span></span>|<span data-ttu-id="aacdd-554">Azt jelzi, hogy megelőző elem hello lehet ismételt n számú alkalommal.</span><span class="sxs-lookup"><span data-stu-id="aacdd-554">Indicates hello preceding item can be repeated n number of times.</span></span> <span data-ttu-id="aacdd-555">hello előfordulások vesszővel kell elválasztani.</span><span class="sxs-lookup"><span data-stu-id="aacdd-555">hello occurrences are separated by commas.</span></span>|  
    |<span data-ttu-id="aacdd-556">[.. .n]</span><span class="sxs-lookup"><span data-stu-id="aacdd-556">[ ...n ]</span></span>|<span data-ttu-id="aacdd-557">Azt jelzi, hogy megelőző elem hello lehet ismételt n számú alkalommal.</span><span class="sxs-lookup"><span data-stu-id="aacdd-557">Indicates hello preceding item can be repeated n number of times.</span></span> <span data-ttu-id="aacdd-558">hello előfordulások üres cellákat el egymástól.</span><span class="sxs-lookup"><span data-stu-id="aacdd-558">hello occurrences are separated by blanks.</span></span>|  
  
##  <span data-ttu-id="aacdd-559"><a name="bk_built_in_functions"></a>Beépített funkciók</span><span class="sxs-lookup"><span data-stu-id="aacdd-559"><a name="bk_built_in_functions"></a> Built-in functions</span></span>  
 <span data-ttu-id="aacdd-560">Azure Cosmos-adatbázis SQL számos beépített funkciót biztosít.</span><span class="sxs-lookup"><span data-stu-id="aacdd-560">Azure Cosmos DB provides many built-in SQL functions.</span></span> <span data-ttu-id="aacdd-561">beépített függvények hello kategóriák listája látható.</span><span class="sxs-lookup"><span data-stu-id="aacdd-561">hello categories of built-in functions are listed below.</span></span>  
  
|<span data-ttu-id="aacdd-562">Függvény</span><span class="sxs-lookup"><span data-stu-id="aacdd-562">Function</span></span>|<span data-ttu-id="aacdd-563">Leírás</span><span class="sxs-lookup"><span data-stu-id="aacdd-563">Description</span></span>|  
|--------------|-----------------|  
|[<span data-ttu-id="aacdd-564">Matematikai funkciók</span><span class="sxs-lookup"><span data-stu-id="aacdd-564">Mathematical functions</span></span>](#bk_mathematical_functions)|<span data-ttu-id="aacdd-565">hello matematikai funkciók minden hajtsa végre a számítás, rendszerint bemeneti értékeket, mint szerepkör argumentumokban szolgálnak, és a visszaadandó numerikus érték alapján.</span><span class="sxs-lookup"><span data-stu-id="aacdd-565">hello mathematical functions each perform a calculation, usually based on input values that are provided as arguments, and return a numeric value.</span></span>|  
|[<span data-ttu-id="aacdd-566">Írja be az ellenőrzési funkciók</span><span class="sxs-lookup"><span data-stu-id="aacdd-566">Type checking functions</span></span>](#bk_type_checking_functions)|<span data-ttu-id="aacdd-567">hello típus ellenőrzési funkciók lehetővé teszik az SQL-lekérdezések lévő kifejezés toocheck hello típusú.</span><span class="sxs-lookup"><span data-stu-id="aacdd-567">hello type checking functions allow you toocheck hello type of an expression within SQL queries.</span></span>|  
|[<span data-ttu-id="aacdd-568">Karakterlánc-függvények</span><span class="sxs-lookup"><span data-stu-id="aacdd-568">String functions</span></span>](#bk_string_functions)|<span data-ttu-id="aacdd-569">hello karakterlánc funkciók végrehajtania egy műveletet a bemeneti karakterlánc-értékkel, és a karakterlánc, a numerikus és logikai értéket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-569">hello string functions perform an operation on a string input value and return a string, numeric or Boolean value.</span></span>|  
|[<span data-ttu-id="aacdd-570">A tömb funkciók</span><span class="sxs-lookup"><span data-stu-id="aacdd-570">Array functions</span></span>](#bk_array_functions)|<span data-ttu-id="aacdd-571">hello tömb funkciók egy logikai érték vagy tömb érték, egy tömb bemeneti érték és a numerikus visszatérési művelet végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="aacdd-571">hello array functions perform an operation on an array input value and return numeric, Boolean or array value.</span></span>|  
|[<span data-ttu-id="aacdd-572">Térbeli funkciók</span><span class="sxs-lookup"><span data-stu-id="aacdd-572">Spatial functions</span></span>](#bk_spatial_functions)|<span data-ttu-id="aacdd-573">hello térbeli funkciók végrehajtania egy műveletet a olyan térbeli objektum beviteli értéket, és numerikus vagy logikai érték visszaadása.</span><span class="sxs-lookup"><span data-stu-id="aacdd-573">hello spatial functions perform an operation on an spatial object input value and return a numeric or Boolean value.</span></span>|  
  
###  <span data-ttu-id="aacdd-574"><a name="bk_mathematical_functions"></a>Matematikai funkciók</span><span class="sxs-lookup"><span data-stu-id="aacdd-574"><a name="bk_mathematical_functions"></a> Mathematical functions</span></span>  
 <span data-ttu-id="aacdd-575">hello alábbi funkciók számítás elvégzése, általában a bemeneti értékeket, mint szerepkör argumentumokban szolgálnak, és a visszaadandó numerikus érték alapján.</span><span class="sxs-lookup"><span data-stu-id="aacdd-575">hello following functions each perform a calculation, usually based on input values that are provided as arguments, and return a numeric value.</span></span>  
  
||||  
|-|-|-|  
|[<span data-ttu-id="aacdd-576">ABS</span><span class="sxs-lookup"><span data-stu-id="aacdd-576">ABS</span></span>](#bk_abs)|[<span data-ttu-id="aacdd-577">ARCCOS</span><span class="sxs-lookup"><span data-stu-id="aacdd-577">ACOS</span></span>](#bk_acos)|[<span data-ttu-id="aacdd-578">ARCSIN</span><span class="sxs-lookup"><span data-stu-id="aacdd-578">ASIN</span></span>](#bk_asin)|  
|[<span data-ttu-id="aacdd-579">ATAN</span><span class="sxs-lookup"><span data-stu-id="aacdd-579">ATAN</span></span>](#bk_atan)|[<span data-ttu-id="aacdd-580">ATN2</span><span class="sxs-lookup"><span data-stu-id="aacdd-580">ATN2</span></span>](#bk_atn2)|[<span data-ttu-id="aacdd-581">FELSŐ HATÁR</span><span class="sxs-lookup"><span data-stu-id="aacdd-581">CEILING</span></span>](#bk_ceiling)|  
|[<span data-ttu-id="aacdd-582">COS</span><span class="sxs-lookup"><span data-stu-id="aacdd-582">COS</span></span>](#bk_cos)|[<span data-ttu-id="aacdd-583">TŰZ</span><span class="sxs-lookup"><span data-stu-id="aacdd-583">COT</span></span>](#bk_cot)|[<span data-ttu-id="aacdd-584">FOK</span><span class="sxs-lookup"><span data-stu-id="aacdd-584">DEGREES</span></span>](#bk_degrees)|  
|[<span data-ttu-id="aacdd-585">EXP</span><span class="sxs-lookup"><span data-stu-id="aacdd-585">EXP</span></span>](#bk_exp)|[<span data-ttu-id="aacdd-586">EMELET</span><span class="sxs-lookup"><span data-stu-id="aacdd-586">FLOOR</span></span>](#bk_floor)|[<span data-ttu-id="aacdd-587">NAPLÓ</span><span class="sxs-lookup"><span data-stu-id="aacdd-587">LOG</span></span>](#bk_log)|  
|[<span data-ttu-id="aacdd-588">LOG10</span><span class="sxs-lookup"><span data-stu-id="aacdd-588">LOG10</span></span>](#bk_log10)|[<span data-ttu-id="aacdd-589">A PI</span><span class="sxs-lookup"><span data-stu-id="aacdd-589">PI</span></span>](#bk_pi)|[<span data-ttu-id="aacdd-590">ENERGIAGAZDÁLKODÁSI</span><span class="sxs-lookup"><span data-stu-id="aacdd-590">POWER</span></span>](#bk_power)|  
|[<span data-ttu-id="aacdd-591">RADIÁNBAN MEGADOTT SZÖG</span><span class="sxs-lookup"><span data-stu-id="aacdd-591">RADIANS</span></span>](#bk_radians)|[<span data-ttu-id="aacdd-592">CIKLIKUS</span><span class="sxs-lookup"><span data-stu-id="aacdd-592">ROUND</span></span>](#bk_round)|[<span data-ttu-id="aacdd-593">EG</span><span class="sxs-lookup"><span data-stu-id="aacdd-593">SIN</span></span>](#bk_sin)|  
|[<span data-ttu-id="aacdd-594">SQRT</span><span class="sxs-lookup"><span data-stu-id="aacdd-594">SQRT</span></span>](#bk_sqrt)|[<span data-ttu-id="aacdd-595">NÉGYZETES</span><span class="sxs-lookup"><span data-stu-id="aacdd-595">SQUARE</span></span>](#bk_square)|[<span data-ttu-id="aacdd-596">BEJELENTKEZÉS</span><span class="sxs-lookup"><span data-stu-id="aacdd-596">SIGN</span></span>](#bk_sign)|  
|[<span data-ttu-id="aacdd-597">TAN</span><span class="sxs-lookup"><span data-stu-id="aacdd-597">TAN</span></span>](#bk_tan)|[<span data-ttu-id="aacdd-598">CSONK</span><span class="sxs-lookup"><span data-stu-id="aacdd-598">TRUNC</span></span>](#bk_trunc)||  
  
####  <span data-ttu-id="aacdd-599"><a name="bk_abs"></a>ABS</span><span class="sxs-lookup"><span data-stu-id="aacdd-599"><a name="bk_abs"></a> ABS</span></span>  
 <span data-ttu-id="aacdd-600">Értéket ad vissza hello abszolút (pozitív) hello a megadott numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-600">Returns hello absolute (positive) value of hello specified numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-601">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-601">**Syntax**</span></span>  
  
```  
ABS (<numeric_expression>)  
```  
  
 <span data-ttu-id="aacdd-602">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-602">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="aacdd-603">Van egy numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-603">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-604">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-604">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-605">Egy numerikus kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-605">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-606">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-606">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-607">hello következő példában három eltérő számú hello ABS funkcióval hello eredményeit.</span><span class="sxs-lookup"><span data-stu-id="aacdd-607">hello following example shows hello results of using hello ABS function on three different numbers.</span></span>  
  
```  
SELECT ABS(-1), ABS(0), ABS(1)  
```  
  
 <span data-ttu-id="aacdd-608">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-608">Here is hello result set.</span></span>  
  
```  
[{$1: 1, $2: 0, $3: 1}]  
```  
  
####  <span data-ttu-id="aacdd-609"><a name="bk_acos"></a>ARCCOS</span><span class="sxs-lookup"><span data-stu-id="aacdd-609"><a name="bk_acos"></a> ACOS</span></span>  
 <span data-ttu-id="aacdd-610">Hello szög értéket ad vissza, az radiánban megadott szög, amelynek koszinusza hello megadott numerikus kifejezés; más néven koszinuszát.</span><span class="sxs-lookup"><span data-stu-id="aacdd-610">Returns hello angle, in radians, whose cosine is hello specified numeric expression; also called arccosine.</span></span>  
  
 <span data-ttu-id="aacdd-611">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-611">**Syntax**</span></span>  
  
```  
ACOS(<numeric_expression>)  
```  
  
 <span data-ttu-id="aacdd-612">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-612">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="aacdd-613">Van egy numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-613">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-614">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-614">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-615">Egy numerikus kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-615">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-616">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-616">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-617">hello alábbi példa adja vissza a-1 ARCCOS hello.</span><span class="sxs-lookup"><span data-stu-id="aacdd-617">hello following example returns hello ACOS of -1.</span></span>  
  
```  
SELECT ACOS(-1)  
```  
  
 <span data-ttu-id="aacdd-618">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-618">Here is hello result set.</span></span>  
  
```  
[{"$1": 3.1415926535897931}]  
```  
  
####  <span data-ttu-id="aacdd-619"><a name="bk_asin"></a>ARCSIN</span><span class="sxs-lookup"><span data-stu-id="aacdd-619"><a name="bk_asin"></a> ASIN</span></span>  
 <span data-ttu-id="aacdd-620">Hello szög értéket ad vissza, az radiánban megadott szög, amelynek szinusza hello megadott numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-620">Returns hello angle, in radians, whose sine is hello specified numeric expression.</span></span> <span data-ttu-id="aacdd-621">Ez rövidítése szinuszát.</span><span class="sxs-lookup"><span data-stu-id="aacdd-621">This is also called arcsine.</span></span>  
  
 <span data-ttu-id="aacdd-622">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-622">**Syntax**</span></span>  
  
```  
ASIN(<numeric_expression>)  
```  
  
 <span data-ttu-id="aacdd-623">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-623">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="aacdd-624">Van egy numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-624">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-625">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-625">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-626">Egy numerikus kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-626">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-627">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-627">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-628">hello alábbi példa adja vissza a-1 ARCSIN hello.</span><span class="sxs-lookup"><span data-stu-id="aacdd-628">hello following example returns hello ASIN of -1.</span></span>  
  
```  
SELECT ASIN(-1)  
```  
  
 <span data-ttu-id="aacdd-629">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-629">Here is hello result set.</span></span>  
  
```  
[{"$1": -1.5707963267948966}]  
```  
  
####  <span data-ttu-id="aacdd-630"><a name="bk_atan"></a>ATAN</span><span class="sxs-lookup"><span data-stu-id="aacdd-630"><a name="bk_atan"></a> ATAN</span></span>  
 <span data-ttu-id="aacdd-631">Hello szög értéket ad vissza, az radiánban megadott szög, amelynek tangense a hello megadott numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-631">Returns hello angle, in radians, whose tangent is hello specified numeric expression.</span></span> <span data-ttu-id="aacdd-632">Ezt arkusz is nevezik.</span><span class="sxs-lookup"><span data-stu-id="aacdd-632">This is also called arctangent.</span></span>  
  
 <span data-ttu-id="aacdd-633">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-633">**Syntax**</span></span>  
  
```  
ATAN(<numeric_expression>)  
```  
  
 <span data-ttu-id="aacdd-634">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-634">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="aacdd-635">Van egy numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-635">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-636">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-636">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-637">Egy numerikus kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-637">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-638">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-638">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-639">a következő példa értéket ad vissza a hello ATAN hello hello megadott értéket.</span><span class="sxs-lookup"><span data-stu-id="aacdd-639">hello following example returns hello ATAN of hello specified value.</span></span>  
  
```  
SELECT ATAN(-45.01)  
```  
  
 <span data-ttu-id="aacdd-640">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-640">Here is hello result set.</span></span>  
  
```  
[{"$1": -1.5485826962062663}]  
```  
  
####  <span data-ttu-id="aacdd-641"><a name="bk_atn2"></a>ATN2</span><span class="sxs-lookup"><span data-stu-id="aacdd-641"><a name="bk_atn2"></a> ATN2</span></span>  
 <span data-ttu-id="aacdd-642">Hello arkusz tangens / x, y radiánban kifejezett hello tag értékét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-642">Returns hello principal value of hello arc tangent of y/x, expressed in radians.</span></span>  
  
 <span data-ttu-id="aacdd-643">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-643">**Syntax**</span></span>  
  
```  
ATN2(<numeric_expression>, <numeric_expression>)  
```  
  
 <span data-ttu-id="aacdd-644">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-644">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="aacdd-645">Van egy numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-645">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-646">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-646">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-647">Egy numerikus kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-647">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-648">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-648">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-649">hello alábbi példa kiszámítja a megadott hello ATN2 hello x és y összetevőket.</span><span class="sxs-lookup"><span data-stu-id="aacdd-649">hello following example calculates hello ATN2 for hello specified x and y components.</span></span>  
  
```  
SELECT ATN2(35.175643, 129.44)  
```  
  
 <span data-ttu-id="aacdd-650">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-650">Here is hello result set.</span></span>  
  
```  
[{"$1": 1.3054517947300646}]  
```  
  
####  <span data-ttu-id="aacdd-651"><a name="bk_ceiling"></a>FELSŐ HATÁR</span><span class="sxs-lookup"><span data-stu-id="aacdd-651"><a name="bk_ceiling"></a> CEILING</span></span>  
 <span data-ttu-id="aacdd-652">Beolvasása hello legkisebb egész szám nagyobb értékre, vagy egyenlő, hello megadott numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-652">Returns hello smallest integer value greater than, or equal to, hello specified numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-653">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-653">**Syntax**</span></span>  
  
```  
CEILING (<numeric_expression>)  
```  
  
 <span data-ttu-id="aacdd-654">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-654">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="aacdd-655">Van egy numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-655">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-656">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-656">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-657">Egy numerikus kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-657">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-658">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-658">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-659">hello a következő példa bemutatja a pozitív szám, negatív, és nulla érték a felső határ függvény hello.</span><span class="sxs-lookup"><span data-stu-id="aacdd-659">hello following example shows positive numeric, negative, and zero values with hello CEILING function.</span></span>  
  
```  
SELECT CEILING(123.45), CEILING(-123.45), CEILING(0.0)  
```  
  
 <span data-ttu-id="aacdd-660">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-660">Here is hello result set.</span></span>  
  
```  
[{$1: 124, $2: -123, $3: 0}]  
```  
  
####  <span data-ttu-id="aacdd-661"><a name="bk_cos"></a>COS</span><span class="sxs-lookup"><span data-stu-id="aacdd-661"><a name="bk_cos"></a> COS</span></span>  
 <span data-ttu-id="aacdd-662">Beolvasása hello trigonometric koszinusza hello radiánban megadott szög, hello a megadott kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-662">Returns hello trigonometric cosine of hello specified angle, in radians, in hello specified expression.</span></span>  
  
 <span data-ttu-id="aacdd-663">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-663">**Syntax**</span></span>  
  
```  
COS(<numeric_expression>)  
```  
  
 <span data-ttu-id="aacdd-664">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-664">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="aacdd-665">Van egy numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-665">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-666">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-666">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-667">Egy numerikus kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-667">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-668">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-668">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-669">a következő példa hello kiszámítja a hello COS hello a megadott szög.</span><span class="sxs-lookup"><span data-stu-id="aacdd-669">hello following example calculates hello COS of hello specified angle.</span></span>  
  
```  
SELECT COS(14.78)  
```  
  
 <span data-ttu-id="aacdd-670">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-670">Here is hello result set.</span></span>  
  
```  
[{"$1": -0.59946542619465426}]  
```  
  
####  <span data-ttu-id="aacdd-671"><a name="bk_cot"></a>TŰZ</span><span class="sxs-lookup"><span data-stu-id="aacdd-671"><a name="bk_cot"></a> COT</span></span>  
 <span data-ttu-id="aacdd-672">Beolvasása hello trigonometric kotangensét hello radiánban megadott szög, a hello megadva a numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-672">Returns hello trigonometric cotangent of hello specified angle, in radians, in hello specified numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-673">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-673">**Syntax**</span></span>  
  
```  
COT(<numeric_expression>)  
```  
  
 <span data-ttu-id="aacdd-674">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-674">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="aacdd-675">Van egy numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-675">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-676">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-676">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-677">Egy numerikus kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-677">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-678">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-678">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-679">a következő példa hello hello hello megadott szög tűz számítja ki.</span><span class="sxs-lookup"><span data-stu-id="aacdd-679">hello following example calculates hello COT of hello specified angle.</span></span>  
  
```  
SELECT COT(124.1332)  
```  
  
 <span data-ttu-id="aacdd-680">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-680">Here is hello result set.</span></span>  
  
```  
[{"$1": -0.040311998371148884}]  
```  
  
####  <span data-ttu-id="aacdd-681"><a name="bk_degrees"></a>FOK</span><span class="sxs-lookup"><span data-stu-id="aacdd-681"><a name="bk_degrees"></a> DEGREES</span></span>  
 <span data-ttu-id="aacdd-682">Értéket ad vissza megfelelő szög (fokban megadva) az radiánban megadott szög hello.</span><span class="sxs-lookup"><span data-stu-id="aacdd-682">Returns hello corresponding angle in degrees for an angle specified in radians.</span></span>  
  
 <span data-ttu-id="aacdd-683">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-683">**Syntax**</span></span>  
  
```  
DEGREES (<numeric_expression>)  
```  
  
 <span data-ttu-id="aacdd-684">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-684">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="aacdd-685">Van egy numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-685">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-686">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-686">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-687">Egy numerikus kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-687">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-688">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-688">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-689">hello alábbi példa számát adja vissza hello fokban szög radiánban PI/2.</span><span class="sxs-lookup"><span data-stu-id="aacdd-689">hello following example returns hello number of degrees in an angle of PI/2 radians.</span></span>  
  
```  
SELECT DEGREES(PI()/2)  
```  
  
 <span data-ttu-id="aacdd-690">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-690">Here is hello result set.</span></span>  
  
```  
[{"$1": 90}]  
```  
  
####  <span data-ttu-id="aacdd-691"><a name="bk_floor"></a>EMELET</span><span class="sxs-lookup"><span data-stu-id="aacdd-691"><a name="bk_floor"></a> FLOOR</span></span>  
 <span data-ttu-id="aacdd-692">Visszaadja hello legnagyobb egész szám kisebb vagy egyenlő, mint a toohello megadott numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-692">Returns hello largest integer less than or equal toohello specified numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-693">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-693">**Syntax**</span></span>  
  
```  
FLOOR (<numeric_expression>)  
```  
  
 <span data-ttu-id="aacdd-694">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-694">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="aacdd-695">Van egy numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-695">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-696">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-696">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-697">Egy numerikus kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-697">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-698">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-698">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-699">hello a következő példa bemutatja a pozitív szám, negatív, és a nulla érték hello EMELET függvény.</span><span class="sxs-lookup"><span data-stu-id="aacdd-699">hello following example shows positive numeric, negative, and zero values with hello FLOOR function.</span></span>  
  
```  
SELECT FLOOR(123.45), FLOOR(-123.45), FLOOR(0.0)  
```  
  
 <span data-ttu-id="aacdd-700">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-700">Here is hello result set.</span></span>  
  
```  
[{$1: 123, $2: -124, $3: 0}]  
```  
  
####  <span data-ttu-id="aacdd-701"><a name="bk_exp"></a>EXP</span><span class="sxs-lookup"><span data-stu-id="aacdd-701"><a name="bk_exp"></a> EXP</span></span>  
 <span data-ttu-id="aacdd-702">Értéket ad vissza hello exponenciális hello a megadott numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-702">Returns hello exponential value of hello specified numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-703">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-703">**Syntax**</span></span>  
  
```  
EXP (<numeric_expression>)  
```  
  
 <span data-ttu-id="aacdd-704">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-704">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="aacdd-705">Van egy numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-705">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-706">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-706">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-707">Egy numerikus kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-707">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-708">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="aacdd-708">**Remarks**</span></span>  
  
 <span data-ttu-id="aacdd-709">hello állandó **e** (2.718281...), akkor hello természetes logaritmus alapja.</span><span class="sxs-lookup"><span data-stu-id="aacdd-709">hello constant **e** (2.718281…), is hello base of natural logarithms.</span></span>  
  
 <span data-ttu-id="aacdd-710">hello hatványát egy szám hello állandó **e** toohello power hello szám következik be.</span><span class="sxs-lookup"><span data-stu-id="aacdd-710">hello exponent of a number is hello constant **e** raised toohello power of hello number.</span></span> <span data-ttu-id="aacdd-711">Például EXP(1.0) = e ^ 1.0 = 2.71828182845905 és EXP(10) = e ^ 10 = 22026.4657948067.</span><span class="sxs-lookup"><span data-stu-id="aacdd-711">For example EXP(1.0) = e^1.0 = 2.71828182845905 and EXP(10) = e^10 = 22026.4657948067.</span></span>  
  
 <span data-ttu-id="aacdd-712">hello exponenciális hello természetes alapú logaritmus egy szám maga hello szám: EXP (napló (n)) = n.</span><span class="sxs-lookup"><span data-stu-id="aacdd-712">hello exponential of hello natural logarithm of a number is hello number itself: EXP (LOG (n)) = n.</span></span> <span data-ttu-id="aacdd-713">Egy szám exponenciális hello hello természetes alapú logaritmusát pedig magát a hello száma: (EXP (n)) napló = n.</span><span class="sxs-lookup"><span data-stu-id="aacdd-713">And hello natural logarithm of hello exponential of a number is hello number itself: LOG (EXP (n)) = n.</span></span>  
  
 <span data-ttu-id="aacdd-714">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-714">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-715">hello alábbi példa egy változót deklarál és hello exponenciális hello változóhoz (10) értékét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-715">hello following example declares a variable and returns hello exponential value of hello specified variable (10).</span></span>  
  
```  
SELECT EXP(10)  
```  
  
 <span data-ttu-id="aacdd-716">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-716">Here is hello result set.</span></span>  
  
```  
[{$1: 22026.465794806718}]  
```  
  
 <span data-ttu-id="aacdd-717">hello alábbi példa értéket ad vissza hello exponenciális hello természetes alapú logaritmusát 20 hello természetes alapú logaritmusát hello exponenciális 20.</span><span class="sxs-lookup"><span data-stu-id="aacdd-717">hello following example returns hello exponential value of hello natural logarithm of 20 and hello natural logarithm of hello exponential of 20.</span></span> <span data-ttu-id="aacdd-718">Mivel ezek a funkciók egy másik, lebegőpontos szám matematikai mindkét esetben 20 kerekítési visszatérési értékének hello inverz funkcióit.</span><span class="sxs-lookup"><span data-stu-id="aacdd-718">Because these functions are inverse functions of one another, hello return value with rounding for floating point math in both cases is 20.</span></span>  
  
```  
SELECT EXP(LOG(20)), LOG(EXP(20))  
```  
  
 <span data-ttu-id="aacdd-719">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-719">Here is hello result set.</span></span>  
  
```  
[{$1: 19.999999999999996, $2: 20}]  
```  
  
####  <span data-ttu-id="aacdd-720"><a name="bk_log"></a>NAPLÓ</span><span class="sxs-lookup"><span data-stu-id="aacdd-720"><a name="bk_log"></a> LOG</span></span>  
 <span data-ttu-id="aacdd-721">Beolvasása hello természetes alapú logaritmusát hello megadott numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-721">Returns hello natural logarithm of hello specified numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-722">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-722">**Syntax**</span></span>  
  
```  
LOG (<numeric_expression> [, <base>])  
```  
  
 <span data-ttu-id="aacdd-723">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-723">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="aacdd-724">Van egy numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-724">Is a numeric expression.</span></span>  
  
-   `base`  
  
     <span data-ttu-id="aacdd-725">Nem kötelező numerikus argumentum hello hello logaritmus alapjának állítja be.</span><span class="sxs-lookup"><span data-stu-id="aacdd-725">Optional numeric argument that sets hello base for hello logarithm.</span></span>  
  
 <span data-ttu-id="aacdd-726">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-726">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-727">Egy numerikus kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-727">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-728">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="aacdd-728">**Remarks**</span></span>  
  
 <span data-ttu-id="aacdd-729">Alapértelmezés szerint LOG() hello természetes alapú logaritmusát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-729">By default, LOG() returns hello natural logarithm.</span></span> <span data-ttu-id="aacdd-730">Hello alapját jelentő hello logaritmusát tooanother érték hello választható alap paraméter használatával módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="aacdd-730">You can change hello base of hello logarithm tooanother value by using hello optional base parameter.</span></span>  
  
 <span data-ttu-id="aacdd-731">hello természetes alapú logaritmusát hello logaritmusát toohello alap **e**, ahol **e** egy ésszerűtlen állandó megközelítőleg azonos too2.718281828 van.</span><span class="sxs-lookup"><span data-stu-id="aacdd-731">hello natural logarithm is hello logarithm toohello base **e**, where **e** is an irrational constant approximately equal too2.718281828.</span></span>  
  
 <span data-ttu-id="aacdd-732">hello természetes alapú logaritmusát hello exponenciális egy szám maga hello száma: (EXP (n)) napló = n.</span><span class="sxs-lookup"><span data-stu-id="aacdd-732">hello natural logarithm of hello exponential of a number is hello number itself: LOG( EXP( n ) ) = n.</span></span> <span data-ttu-id="aacdd-733">Hello természetes alapú logaritmus egy számot, exponenciális hello pedig magát a hello száma: EXP (napló (n)) = n.</span><span class="sxs-lookup"><span data-stu-id="aacdd-733">And hello exponential of hello natural logarithm of a number is hello number itself: EXP( LOG( n ) ) = n.</span></span>  
  
 <span data-ttu-id="aacdd-734">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-734">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-735">hello alábbi példa egy változót deklarál és hello változóhoz (10) értékét hello alapú logaritmusát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-735">hello following example declares a variable and returns hello logarithm value of hello specified variable (10).</span></span>  
  
```  
SELECT LOG(10)  
```  
  
 <span data-ttu-id="aacdd-736">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-736">Here is hello result set.</span></span>  
  
```  
[{$1: 2.3025850929940459}]  
```  
  
 <span data-ttu-id="aacdd-737">hello alábbi példa kiszámítja hello napló a hello hatványát szám.</span><span class="sxs-lookup"><span data-stu-id="aacdd-737">hello following example calculates hello LOG for hello exponent of a number.</span></span>  
  
```  
SELECT EXP(LOG(10))  
```  
  
 <span data-ttu-id="aacdd-738">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-738">Here is hello result set.</span></span>  
  
```  
[{$1: 10.000000000000002}]  
```  
  
####  <span data-ttu-id="aacdd-739"><a name="bk_log10"></a>LOG10</span><span class="sxs-lookup"><span data-stu-id="aacdd-739"><a name="bk_log10"></a> LOG10</span></span>  
 <span data-ttu-id="aacdd-740">Beolvasása hello 10-es alapú logaritmusát hello megadott numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-740">Returns hello base-10 logarithm of hello specified numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-741">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-741">**Syntax**</span></span>  
  
```  
LOG10 (<numeric_expression>)  
```  
  
 <span data-ttu-id="aacdd-742">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-742">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="aacdd-743">Van egy numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-743">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-744">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-744">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-745">Egy numerikus kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-745">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-746">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="aacdd-746">**Remarks**</span></span>  
  
 <span data-ttu-id="aacdd-747">hello LOG10 és POWER funkciók intenzitásfokozatok kapcsolódó tooone egy másik.</span><span class="sxs-lookup"><span data-stu-id="aacdd-747">hello LOG10 and POWER functions are inversely related tooone another.</span></span> <span data-ttu-id="aacdd-748">Például 10 ^ LOG10(n) = n.</span><span class="sxs-lookup"><span data-stu-id="aacdd-748">For example, 10 ^ LOG10(n) = n.</span></span>  
  
 <span data-ttu-id="aacdd-749">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-749">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-750">hello alábbi példa egy változót deklarál és hello változóhoz (100) hello LOG10 értékét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-750">hello following example declares a variable and returns hello LOG10 value of hello specified variable (100).</span></span>  
  
```  
SELECT LOG10(100)  
```  
  
 <span data-ttu-id="aacdd-751">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-751">Here is hello result set.</span></span>  
  
```  
[{$1: 2}]  
```  
  
####  <span data-ttu-id="aacdd-752"><a name="bk_pi"></a>A PI</span><span class="sxs-lookup"><span data-stu-id="aacdd-752"><a name="bk_pi"></a> PI</span></span>  
 <span data-ttu-id="aacdd-753">Beolvasása hello pi konstans érték.</span><span class="sxs-lookup"><span data-stu-id="aacdd-753">Returns hello constant value of PI.</span></span>  
  
 <span data-ttu-id="aacdd-754">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-754">**Syntax**</span></span>  
  
```  
PI ()  
```  
  
 <span data-ttu-id="aacdd-755">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-755">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="aacdd-756">Van egy numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-756">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-757">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-757">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-758">Egy numerikus kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-758">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-759">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-759">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-760">a következő példa a hello értéket ad vissza a PI hello.</span><span class="sxs-lookup"><span data-stu-id="aacdd-760">hello following example returns hello value of PI.</span></span>  
  
```  
SELECT PI()  
```  
  
 <span data-ttu-id="aacdd-761">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-761">Here is hello result set.</span></span>  
  
```  
[{"$1": 3.1415926535897931}]  
```  
  
####  <span data-ttu-id="aacdd-762"><a name="bk_power"></a>ENERGIAGAZDÁLKODÁSI</span><span class="sxs-lookup"><span data-stu-id="aacdd-762"><a name="bk_power"></a> POWER</span></span>  
 <span data-ttu-id="aacdd-763">Értéket ad vissza a megadott hello értékének hello kifejezés toohello megadott power.</span><span class="sxs-lookup"><span data-stu-id="aacdd-763">Returns hello value of hello specified expression toohello specified power.</span></span>  
  
 <span data-ttu-id="aacdd-764">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-764">**Syntax**</span></span>  
  
```  
POWER (<numeric_expression>, <y>)  
```  
  
 <span data-ttu-id="aacdd-765">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-765">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="aacdd-766">Van egy numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-766">Is a numeric expression.</span></span>  
  
-   `y`  
  
     <span data-ttu-id="aacdd-767">Hello power toowhich tooraise van `numeric_expression`.</span><span class="sxs-lookup"><span data-stu-id="aacdd-767">Is hello power toowhich tooraise `numeric_expression`.</span></span>  
  
 <span data-ttu-id="aacdd-768">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-768">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-769">Egy numerikus kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-769">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-770">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-770">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-771">hello a következő példa bemutatja, 3 (hello adatkocka hello szám) számú toohello hatványa előléptetése.</span><span class="sxs-lookup"><span data-stu-id="aacdd-771">hello following example demonstrates raising a number toohello power of 3 (hello cube of hello number).</span></span>  
  
```  
SELECT POWER(2, 3), POWER(2.5, 3)  
```  
  
 <span data-ttu-id="aacdd-772">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-772">Here is hello result set.</span></span>  
  
```  
[{$1: 8, $2: 15.625}]  
```  
  
####  <span data-ttu-id="aacdd-773"><a name="bk_radians"></a>RADIÁNBAN MEGADOTT SZÖG</span><span class="sxs-lookup"><span data-stu-id="aacdd-773"><a name="bk_radians"></a> RADIANS</span></span>  
 <span data-ttu-id="aacdd-774">Vissza a radiánban megadott szög, ha egy numerikus kifejezés fokban, is meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="aacdd-774">Returns radians when a numeric expression, in degrees, is entered.</span></span>  
  
 <span data-ttu-id="aacdd-775">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-775">**Syntax**</span></span>  
  
```  
RADIANS (<numeric_expression>)  
```  
  
 <span data-ttu-id="aacdd-776">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-776">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="aacdd-777">Van egy numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-777">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-778">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-778">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-779">Egy numerikus kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-779">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-780">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-780">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-781">hello következő példa néhány szögek fogadja bemeneti adatként, és adja vissza a hozzájuk tartozó radián értékek.</span><span class="sxs-lookup"><span data-stu-id="aacdd-781">hello following example takes a few angles as input and returns their corresponding radian values.</span></span>  
  
```  
SELECT RADIANS(-45.01), RADIANS(-181.01), RADIANS(0), RADIANS(0.1472738), RADIANS(197.1099392)  
```  
  
 <span data-ttu-id="aacdd-782">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-782">Here is hello result set.</span></span>  
  
```  
[{  
       "$1": -0.7855726963226477,  
       "$2": -3.1592204790349356,  
       "$3": 0,  
       "$4": 0.0025704127119236249,  
       "$5": 3.4402174274458375  
   }]  
```  
  
####  <span data-ttu-id="aacdd-783"><a name="bk_round"></a>CIKLIKUS</span><span class="sxs-lookup"><span data-stu-id="aacdd-783"><a name="bk_round"></a> ROUND</span></span>  
 <span data-ttu-id="aacdd-784">Egy numerikus érték, kerekített toohello legközelebbi egész értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-784">Returns a numeric value, rounded toohello closest integer value.</span></span>  
  
 <span data-ttu-id="aacdd-785">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-785">**Syntax**</span></span>  
  
```  
ROUND(<numeric_expression>)  
```  
  
 <span data-ttu-id="aacdd-786">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-786">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="aacdd-787">Van egy numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-787">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-788">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-788">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-789">Egy numerikus kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-789">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-790">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-790">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-791">a következő példa kerekítés hello toohello pozitív és negatív számokat a legközelebbi egész számra a következő hello.</span><span class="sxs-lookup"><span data-stu-id="aacdd-791">hello following example rounds hello following positive and negative numbers toohello nearest integer.</span></span>  
  
```  
SELECT ROUND(2.4), ROUND(2.6), ROUND(2.5), ROUND(-2.4), ROUND(-2.6)  
```  
  
 <span data-ttu-id="aacdd-792">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-792">Here is hello result set.</span></span>  
  
```  
[{$1: 2, $2: 3, $3: 3, $4: -2, $5: -3}]  
```  
  
####  <span data-ttu-id="aacdd-793"><a name="bk_sign"></a>BEJELENTKEZÉS</span><span class="sxs-lookup"><span data-stu-id="aacdd-793"><a name="bk_sign"></a> SIGN</span></span>  
 <span data-ttu-id="aacdd-794">Beolvasása hello pozitív (+ 1), nulla (0), vagy a hello mínuszjel (-1) megadott numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-794">Returns hello positive (+1), zero (0), or negative (-1) sign of hello specified numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-795">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-795">**Syntax**</span></span>  
  
```  
SIGN(<numeric_expression>)  
```  
  
 <span data-ttu-id="aacdd-796">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-796">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="aacdd-797">Van egy numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-797">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-798">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-798">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-799">Egy numerikus kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-799">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-800">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-800">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-801">hello alábbi példa vissza hello bejelentkezési értékek számának-2 too2.</span><span class="sxs-lookup"><span data-stu-id="aacdd-801">hello following example returns hello SIGN values of numbers from -2 too2.</span></span>  
  
```  
SELECT SIGN(-2), SIGN(-1), SIGN(0), SIGN(1), SIGN(2)  
```  
  
 <span data-ttu-id="aacdd-802">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-802">Here is hello result set.</span></span>  
  
```  
[{$1: -1, $2: -1, $3: 0, $4: 1, $5: 1}]  
```  
  
####  <span data-ttu-id="aacdd-803"><a name="bk_sin"></a>EG</span><span class="sxs-lookup"><span data-stu-id="aacdd-803"><a name="bk_sin"></a> SIN</span></span>  
 <span data-ttu-id="aacdd-804">Beolvasása hello trigonometric szinusza hello radiánban megadott szög, hello a megadott kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-804">Returns hello trigonometric sine of hello specified angle, in radians, in hello specified expression.</span></span>  
  
 <span data-ttu-id="aacdd-805">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-805">**Syntax**</span></span>  
  
```  
SIN(<numeric_expression>)  
```  
  
 <span data-ttu-id="aacdd-806">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-806">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="aacdd-807">Van egy numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-807">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-808">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-808">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-809">Egy numerikus kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-809">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-810">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-810">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-811">a következő példa hello hello megadott szög Szinusz hello számítja ki.</span><span class="sxs-lookup"><span data-stu-id="aacdd-811">hello following example calculates hello SIN of hello specified angle.</span></span>  
  
```  
SELECT SIN(45.175643)  
```  
  
 <span data-ttu-id="aacdd-812">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-812">Here is hello result set.</span></span>  
  
```  
[{"$1": 0.929607286611012}]  
```  
  
####  <span data-ttu-id="aacdd-813"><a name="bk_sqrt"></a>SQRT</span><span class="sxs-lookup"><span data-stu-id="aacdd-813"><a name="bk_sqrt"></a> SQRT</span></span>  
 <span data-ttu-id="aacdd-814">Hello négyzetgyökét adja vissza a hello megadott numerikus érték.</span><span class="sxs-lookup"><span data-stu-id="aacdd-814">Returns hello square root of hello specified numeric value.</span></span>  
  
 <span data-ttu-id="aacdd-815">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-815">**Syntax**</span></span>  
  
```  
SQRT(<numeric_expression>)  
```  
  
 <span data-ttu-id="aacdd-816">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-816">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="aacdd-817">Van egy numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-817">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-818">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-818">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-819">Egy numerikus kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-819">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-820">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-820">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-821">hello alábbi példa adja vissza hello szögletes gyökerek számok 1-3.</span><span class="sxs-lookup"><span data-stu-id="aacdd-821">hello following example returns hello square roots of numbers 1-3.</span></span>  
  
```  
SELECT SQRT(1), SQRT(2.0), SQRT(3)  
```  
  
 <span data-ttu-id="aacdd-822">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-822">Here is hello result set.</span></span>  
  
```  
[{$1: 1, $2: 1.4142135623730952, $3: 1.7320508075688772}]  
```  
  
####  <span data-ttu-id="aacdd-823"><a name="bk_square"></a>NÉGYZETES</span><span class="sxs-lookup"><span data-stu-id="aacdd-823"><a name="bk_square"></a> SQUARE</span></span>  
 <span data-ttu-id="aacdd-824">Beolvasása hello négyzetes hello a megadott numerikus érték.</span><span class="sxs-lookup"><span data-stu-id="aacdd-824">Returns hello square of hello specified numeric value.</span></span>  
  
 <span data-ttu-id="aacdd-825">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-825">**Syntax**</span></span>  
  
```  
SQUARE(<numeric_expression>)  
```  
  
 <span data-ttu-id="aacdd-826">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-826">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="aacdd-827">Van egy numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-827">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-828">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-828">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-829">Egy numerikus kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-829">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-830">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-830">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-831">hello alábbi példa adja vissza hello négyzetes számok 1-3.</span><span class="sxs-lookup"><span data-stu-id="aacdd-831">hello following example returns hello squares of numbers 1-3.</span></span>  
  
```  
SELECT SQUARE(1), SQUARE(2.0), SQUARE(3)  
```  
  
 <span data-ttu-id="aacdd-832">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-832">Here is hello result set.</span></span>  
  
```  
[{$1: 1, $2: 4, $3: 9}]  
```  
  
####  <span data-ttu-id="aacdd-833"><a name="bk_tan"></a>TAN</span><span class="sxs-lookup"><span data-stu-id="aacdd-833"><a name="bk_tan"></a> TAN</span></span>  
 <span data-ttu-id="aacdd-834">Hello hello tangensét adja vissza radiánban megadott szög, hello a megadott kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-834">Returns hello tangent of hello specified angle, in radians, in hello specified expression.</span></span>  
  
 <span data-ttu-id="aacdd-835">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-835">**Syntax**</span></span>  
  
```  
TAN (<numeric_expression>)  
```  
  
 <span data-ttu-id="aacdd-836">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-836">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="aacdd-837">Van egy numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-837">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-838">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-838">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-839">Egy numerikus kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-839">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-840">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-840">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-841">hello alábbi példa kiszámítja PI () hello tangensét / 2.</span><span class="sxs-lookup"><span data-stu-id="aacdd-841">hello following example calculates hello tangent of PI()/2.</span></span>  
  
```  
SELECT TAN(PI()/2);  
```  
  
 <span data-ttu-id="aacdd-842">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-842">Here is hello result set.</span></span>  
  
```  
[{"$1": 16331239353195370 }]  
```  
  
####  <span data-ttu-id="aacdd-843"><a name="bk_trunc"></a>CSONK</span><span class="sxs-lookup"><span data-stu-id="aacdd-843"><a name="bk_trunc"></a> TRUNC</span></span>  
 <span data-ttu-id="aacdd-844">Egy numerikus érték, csonkolt toohello legközelebbi egész értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-844">Returns a numeric value, truncated toohello closest integer value.</span></span>  
  
 <span data-ttu-id="aacdd-845">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-845">**Syntax**</span></span>  
  
```  
TRUNC(<numeric_expression>)  
```  
  
 <span data-ttu-id="aacdd-846">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-846">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="aacdd-847">Van egy numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-847">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-848">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-848">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-849">Egy numerikus kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-849">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-850">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-850">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-851">a következő példa hello csonkolja a következő egész számot a legközelebbi pozitív és negatív számok toohello hello.</span><span class="sxs-lookup"><span data-stu-id="aacdd-851">hello following example truncates hello following positive and negative numbers toohello nearest integer value.</span></span>  
  
```  
SELECT TRUNC(2.4), TRUNC(2.6), TRUNC(2.5), TRUNC(-2.4), TRUNC(-2.6)  
```  
  
 <span data-ttu-id="aacdd-852">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-852">Here is hello result set.</span></span>  
  
```  
[{$1: 2, $2: 2, $3: 2, $4: -2, $5: -2}]  
```  
  
###  <span data-ttu-id="aacdd-853"><a name="bk_type_checking_functions"></a>Írja be az ellenőrzési funkciók</span><span class="sxs-lookup"><span data-stu-id="aacdd-853"><a name="bk_type_checking_functions"></a> Type checking functions</span></span>  
 <span data-ttu-id="aacdd-854">hello következő funkciókat támogatja az típusú bemeneti értékekkel ellenőrzése, és minden egyes logikai értéket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-854">hello following functions support type checking against input values, and each return a Boolean value.</span></span>  
  
||||  
|-|-|-|  
|[<span data-ttu-id="aacdd-855">IS_ARRAY</span><span class="sxs-lookup"><span data-stu-id="aacdd-855">IS_ARRAY</span></span>](#bk_is_array)|[<span data-ttu-id="aacdd-856">IS_BOOL</span><span class="sxs-lookup"><span data-stu-id="aacdd-856">IS_BOOL</span></span>](#bk_is_bool)|[<span data-ttu-id="aacdd-857">IS_DEFINED</span><span class="sxs-lookup"><span data-stu-id="aacdd-857">IS_DEFINED</span></span>](#bk_is_defined)|  
|[<span data-ttu-id="aacdd-858">IS_NULL</span><span class="sxs-lookup"><span data-stu-id="aacdd-858">IS_NULL</span></span>](#bk_is_null)|[<span data-ttu-id="aacdd-859">IS_NUMBER</span><span class="sxs-lookup"><span data-stu-id="aacdd-859">IS_NUMBER</span></span>](#bk_is_number)|[<span data-ttu-id="aacdd-860">IS_OBJECT</span><span class="sxs-lookup"><span data-stu-id="aacdd-860">IS_OBJECT</span></span>](#bk_is_object)|  
|[<span data-ttu-id="aacdd-861">IS_PRIMITIVE</span><span class="sxs-lookup"><span data-stu-id="aacdd-861">IS_PRIMITIVE</span></span>](#bk_is_primitive)|[<span data-ttu-id="aacdd-862">IS_STRING</span><span class="sxs-lookup"><span data-stu-id="aacdd-862">IS_STRING</span></span>](#bk_is_string)||  
  
####  <span data-ttu-id="aacdd-863"><a name="bk_is_array"></a>IS_ARRAY</span><span class="sxs-lookup"><span data-stu-id="aacdd-863"><a name="bk_is_array"></a> IS_ARRAY</span></span>  
 <span data-ttu-id="aacdd-864">Adja vissza, ha hello hello típusú megadott kifejezés jelző logikai érték egy tömb.</span><span class="sxs-lookup"><span data-stu-id="aacdd-864">Returns a Boolean value indicating if hello type of hello specified expression is an array.</span></span>  
  
 <span data-ttu-id="aacdd-865">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-865">**Syntax**</span></span>  
  
```  
IS_ARRAY(<expression>)  
```  
  
 <span data-ttu-id="aacdd-866">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-866">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="aacdd-867">Ez bármilyen érvényes kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-867">Is any valid expression.</span></span>  
  
 <span data-ttu-id="aacdd-868">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-868">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-869">Egy logikai kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-869">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="aacdd-870">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-870">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-871">hello alábbi példa ellenőrzi objektumok JSON logikai, számot, NULL értékű karakterlánc, objektum, a tömb és hello IS_ARRAY függvény használata nem definiált típusok.</span><span class="sxs-lookup"><span data-stu-id="aacdd-871">hello following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using hello IS_ARRAY function.</span></span>  
  
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
  
 <span data-ttu-id="aacdd-872">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-872">Here is hello result set.</span></span>  
  
```  
[{$1: false, $2: false, $3: false, $4: false, $5: false, $6: true}]  
```  
  
####  <span data-ttu-id="aacdd-873"><a name="bk_is_bool"></a>IS_BOOL</span><span class="sxs-lookup"><span data-stu-id="aacdd-873"><a name="bk_is_bool"></a> IS_BOOL</span></span>  
 <span data-ttu-id="aacdd-874">Adja vissza egy logikai érték, amely jelzi, ha hello hello típusú megadott kifejezés olyan logikai érték.</span><span class="sxs-lookup"><span data-stu-id="aacdd-874">Returns a Boolean value indicating if hello type of hello specified expression is a Boolean.</span></span>  
  
 <span data-ttu-id="aacdd-875">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-875">**Syntax**</span></span>  
  
```  
IS_BOOL(<expression>)  
```  
  
 <span data-ttu-id="aacdd-876">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-876">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="aacdd-877">Ez bármilyen érvényes kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-877">Is any valid expression.</span></span>  
  
 <span data-ttu-id="aacdd-878">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-878">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-879">Egy logikai kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-879">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="aacdd-880">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-880">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-881">hello alábbi példa ellenőrzi objektumok JSON logikai, számot, NULL értékű karakterlánc, objektum, a tömb és hello IS_BOOL függvény használata nem definiált típusok.</span><span class="sxs-lookup"><span data-stu-id="aacdd-881">hello following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using hello IS_BOOL function.</span></span>  
  
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
  
 <span data-ttu-id="aacdd-882">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-882">Here is hello result set.</span></span>  
  
```  
[{$1: true, $2: false, $3: false, $4: false, $5: false, $6: false}]  
```  
  
####  <span data-ttu-id="aacdd-883"><a name="bk_is_defined"></a>IS_DEFINED</span><span class="sxs-lookup"><span data-stu-id="aacdd-883"><a name="bk_is_defined"></a> IS_DEFINED</span></span>  
 <span data-ttu-id="aacdd-884">Jelzi, ha hello tulajdonság van rendelve egy érték logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="aacdd-884">Returns a Boolean indicating if hello property has been assigned a value.</span></span>  
  
 <span data-ttu-id="aacdd-885">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-885">**Syntax**</span></span>  
  
```  
IS_DEFINED(<expression>)  
```  
  
 <span data-ttu-id="aacdd-886">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-886">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="aacdd-887">Ez bármilyen érvényes kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-887">Is any valid expression.</span></span>  
  
 <span data-ttu-id="aacdd-888">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-888">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-889">Egy logikai kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-889">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="aacdd-890">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-890">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-891">a következő példa keres belül hello tulajdonság hello jelenléte hello megadott JSON-dokumentum.</span><span class="sxs-lookup"><span data-stu-id="aacdd-891">hello following example checks for hello presence of a property within hello specified JSON document.</span></span> <span data-ttu-id="aacdd-892">hello első igaz értéket ad vissza, mert "a" jelen, de hello második hamis értéket ad vissza, mert hiányzik a "b".</span><span class="sxs-lookup"><span data-stu-id="aacdd-892">hello first returns true since "a" is present, but hello second returns false since "b" is absent.</span></span>  
  
```  
SELECT IS_DEFINED({ "a" : 5 }.a), IS_DEFINED({ "a" : 5 }.b)  
```  
  
 <span data-ttu-id="aacdd-893">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-893">Here is hello result set.</span></span>  
  
```  
[{  
       "$1": true,    
       "$2": false   
   }]  
```  
  
####  <span data-ttu-id="aacdd-894"><a name="bk_is_null"></a>IS_NULL</span><span class="sxs-lookup"><span data-stu-id="aacdd-894"><a name="bk_is_null"></a> IS_NULL</span></span>  
 <span data-ttu-id="aacdd-895">Visszaadja egy logikai érték, amely jelzi, ha hello hello típusú megadott kifejezés értéke null.</span><span class="sxs-lookup"><span data-stu-id="aacdd-895">Returns a Boolean value indicating if hello type of hello specified expression is null.</span></span>  
  
 <span data-ttu-id="aacdd-896">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-896">**Syntax**</span></span>  
  
```  
IS_NULL(<expression>)  
```  
  
 <span data-ttu-id="aacdd-897">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-897">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="aacdd-898">Ez bármilyen érvényes kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-898">Is any valid expression.</span></span>  
  
 <span data-ttu-id="aacdd-899">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-899">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-900">Egy logikai kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-900">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="aacdd-901">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-901">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-902">hello alábbi példa ellenőrzi objektumok JSON logikai, számot, NULL értékű karakterlánc, objektum, a tömb és hello IS_NULL függvény használata nem definiált típusok.</span><span class="sxs-lookup"><span data-stu-id="aacdd-902">hello following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using hello IS_NULL function.</span></span>  
  
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
  
 <span data-ttu-id="aacdd-903">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-903">Here is hello result set.</span></span>  
  
```  
[{$1: false, $2: false, $3: false, $4: true, $5: false, $6: false}]  
```  
  
####  <span data-ttu-id="aacdd-904"><a name="bk_is_number"></a>IS_NUMBER</span><span class="sxs-lookup"><span data-stu-id="aacdd-904"><a name="bk_is_number"></a> IS_NUMBER</span></span>  
 <span data-ttu-id="aacdd-905">Adja vissza, ha hello hello típusú megadott kifejezés jelző logikai érték egy szám.</span><span class="sxs-lookup"><span data-stu-id="aacdd-905">Returns a Boolean value indicating if hello type of hello specified expression is a number.</span></span>  
  
 <span data-ttu-id="aacdd-906">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-906">**Syntax**</span></span>  
  
```  
IS_NUMBER(<expression>)  
```  
  
 <span data-ttu-id="aacdd-907">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-907">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="aacdd-908">Ez bármilyen érvényes kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-908">Is any valid expression.</span></span>  
  
 <span data-ttu-id="aacdd-909">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-909">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-910">Egy logikai kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-910">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="aacdd-911">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-911">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-912">hello alábbi példa ellenőrzi objektumok JSON logikai, számot, NULL értékű karakterlánc, objektum, a tömb és hello IS_NULL függvény használata nem definiált típusok.</span><span class="sxs-lookup"><span data-stu-id="aacdd-912">hello following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using hello IS_NULL function.</span></span>  
  
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
  
 <span data-ttu-id="aacdd-913">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-913">Here is hello result set.</span></span>  
  
```  
[{$1: false, $2: true, $3: false, $4: false, $5: false, $6: false}]  
```  
  
####  <span data-ttu-id="aacdd-914"><a name="bk_is_object"></a>IS_OBJECT</span><span class="sxs-lookup"><span data-stu-id="aacdd-914"><a name="bk_is_object"></a> IS_OBJECT</span></span>  
 <span data-ttu-id="aacdd-915">Adja vissza egy logikai érték, amely jelzi, ha hello hello típusú megadott kifejezés egy JSON-objektum.</span><span class="sxs-lookup"><span data-stu-id="aacdd-915">Returns a Boolean value indicating if hello type of hello specified expression is a JSON object.</span></span>  
  
 <span data-ttu-id="aacdd-916">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-916">**Syntax**</span></span>  
  
```  
IS_OBJECT(<expression>)  
```  
  
 <span data-ttu-id="aacdd-917">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-917">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="aacdd-918">Ez bármilyen érvényes kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-918">Is any valid expression.</span></span>  
  
 <span data-ttu-id="aacdd-919">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-919">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-920">Egy logikai kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-920">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="aacdd-921">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-921">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-922">hello alábbi példa ellenőrzi objektumok JSON logikai, számot, NULL értékű karakterlánc, objektum, a tömb és hello IS_OBJECT függvény használata nem definiált típusok.</span><span class="sxs-lookup"><span data-stu-id="aacdd-922">hello following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using hello IS_OBJECT function.</span></span>  
  
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
  
 <span data-ttu-id="aacdd-923">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-923">Here is hello result set.</span></span>  
  
```  
[{$1: false, $2: false, $3: false, $4: false, $5: true, $6: false}]  
```  
  
####  <span data-ttu-id="aacdd-924"><a name="bk_is_primitive"></a>IS_PRIMITIVE</span><span class="sxs-lookup"><span data-stu-id="aacdd-924"><a name="bk_is_primitive"></a> IS_PRIMITIVE</span></span>  
 <span data-ttu-id="aacdd-925">Ha hello hello megadva kifejezés egy egyszerű jelző logikai érték beolvasása (string, Boolean, numerikus vagy null értékű).</span><span class="sxs-lookup"><span data-stu-id="aacdd-925">Returns a Boolean value indicating if hello type of hello specified expression is a primitive (string, Boolean, numeric or null).</span></span>  
  
 <span data-ttu-id="aacdd-926">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-926">**Syntax**</span></span>  
  
```  
IS_PRIMITIVE(<expression>)  
```  
  
 <span data-ttu-id="aacdd-927">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-927">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="aacdd-928">Ez bármilyen érvényes kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-928">Is any valid expression.</span></span>  
  
 <span data-ttu-id="aacdd-929">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-929">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-930">Egy logikai kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-930">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="aacdd-931">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-931">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-932">hello alábbi példa ellenőrzi objektumok JSON logikai, számot, NULL értékű karakterlánc, objektum, a tömb és hello IS_PRIMITIVE függvény használata nem definiált típusok.</span><span class="sxs-lookup"><span data-stu-id="aacdd-932">hello following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using hello IS_PRIMITIVE function.</span></span>  
  
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
  
 <span data-ttu-id="aacdd-933">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-933">Here is hello result set.</span></span>  
  
```  
[{"$1": true, "$2": true, "$3": true, "$4": true, "$5": false, "$6": false, "$7": false}]  
```  
  
####  <span data-ttu-id="aacdd-934"><a name="bk_is_string"></a>IS_STRING</span><span class="sxs-lookup"><span data-stu-id="aacdd-934"><a name="bk_is_string"></a> IS_STRING</span></span>  
 <span data-ttu-id="aacdd-935">Adja vissza, ha hello hello típusú megadott kifejezés jelző logikai érték: karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="aacdd-935">Returns a Boolean value indicating if hello type of hello specified expression is a string.</span></span>  
  
 <span data-ttu-id="aacdd-936">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-936">**Syntax**</span></span>  
  
```  
IS_STRING(<expression>)  
```  
  
 <span data-ttu-id="aacdd-937">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-937">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="aacdd-938">Ez bármilyen érvényes kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-938">Is any valid expression.</span></span>  
  
 <span data-ttu-id="aacdd-939">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-939">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-940">Egy logikai kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-940">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="aacdd-941">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-941">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-942">hello alábbi példa ellenőrzi objektumok JSON logikai, számot, NULL értékű karakterlánc, objektum, a tömb és hello IS_STRING függvény használata nem definiált típusok.</span><span class="sxs-lookup"><span data-stu-id="aacdd-942">hello following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using hello IS_STRING function.</span></span>  
  
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
  
 <span data-ttu-id="aacdd-943">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-943">Here is hello result set.</span></span>  
  
```  
[{$1: false, $2: false, $3: true, $4: false, $5: false, $6: false}]  
```  
  
###  <span data-ttu-id="aacdd-944"><a name="bk_string_functions"></a>Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="aacdd-944"><a name="bk_string_functions"></a> String functions</span></span>  
 <span data-ttu-id="aacdd-945">hello következő skaláris függvények végrehajtania egy műveletet a bemeneti karakterlánc-érték és a karakterlánc, a numerikus és logikai értéket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-945">hello following scalar functions perform an operation on a string input value and return a string, numeric or Boolean value.</span></span>  
  
||||  
|-|-|-|  
|[<span data-ttu-id="aacdd-946">CONCAT</span><span class="sxs-lookup"><span data-stu-id="aacdd-946">CONCAT</span></span>](#bk_concat)|[<span data-ttu-id="aacdd-947">TARTALMAZZA</span><span class="sxs-lookup"><span data-stu-id="aacdd-947">CONTAINS</span></span>](#bk_contains)|[<span data-ttu-id="aacdd-948">MEGADOTT MÓDON VÉGZŐDŐ</span><span class="sxs-lookup"><span data-stu-id="aacdd-948">ENDSWITH</span></span>](#bk_endswith)|  
|[<span data-ttu-id="aacdd-949">INDEX_OF</span><span class="sxs-lookup"><span data-stu-id="aacdd-949">INDEX_OF</span></span>](#bk_index_of)|[<span data-ttu-id="aacdd-950">BALRA</span><span class="sxs-lookup"><span data-stu-id="aacdd-950">LEFT</span></span>](#bk_left)|[<span data-ttu-id="aacdd-951">HOSSZA</span><span class="sxs-lookup"><span data-stu-id="aacdd-951">LENGTH</span></span>](#bk_length)|  
|[<span data-ttu-id="aacdd-952">ALACSONYABB</span><span class="sxs-lookup"><span data-stu-id="aacdd-952">LOWER</span></span>](#bk_lower)|[<span data-ttu-id="aacdd-953">LTRIM</span><span class="sxs-lookup"><span data-stu-id="aacdd-953">LTRIM</span></span>](#bk_ltrim)|[<span data-ttu-id="aacdd-954">CSERÉLJE LE</span><span class="sxs-lookup"><span data-stu-id="aacdd-954">REPLACE</span></span>](#bk_replace)|  
|[<span data-ttu-id="aacdd-955">REPLIKÁLÁS</span><span class="sxs-lookup"><span data-stu-id="aacdd-955">REPLICATE</span></span>](#bk_replicate)|[<span data-ttu-id="aacdd-956">FORDÍTOTT</span><span class="sxs-lookup"><span data-stu-id="aacdd-956">REVERSE</span></span>](#bk_reverse)|[<span data-ttu-id="aacdd-957">JOBBRA</span><span class="sxs-lookup"><span data-stu-id="aacdd-957">RIGHT</span></span>](#bk_right)|  
|[<span data-ttu-id="aacdd-958">RTRIM</span><span class="sxs-lookup"><span data-stu-id="aacdd-958">RTRIM</span></span>](#bk_rtrim)|[<span data-ttu-id="aacdd-959">STARTSWITH ELEMNEK</span><span class="sxs-lookup"><span data-stu-id="aacdd-959">STARTSWITH</span></span>](#bk_startswith)|[<span data-ttu-id="aacdd-960">SUBSTRING</span><span class="sxs-lookup"><span data-stu-id="aacdd-960">SUBSTRING</span></span>](#bk_substring)|  
|[<span data-ttu-id="aacdd-961">FELSŐ</span><span class="sxs-lookup"><span data-stu-id="aacdd-961">UPPER</span></span>](#bk_upper)|||  
  
####  <span data-ttu-id="aacdd-962"><a name="bk_concat"></a>CONCAT</span><span class="sxs-lookup"><span data-stu-id="aacdd-962"><a name="bk_concat"></a> CONCAT</span></span>  
 <span data-ttu-id="aacdd-963">Egy karakterlánc, amely legalább két karakterlánc-értékek hozzáfűzésével hello eredményét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-963">Returns a string that is hello result of concatenating two or more string values.</span></span>  
  
 <span data-ttu-id="aacdd-964">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-964">**Syntax**</span></span>  
  
```  
CONCAT(<str_expr>, <str_expr> [, <str_expr>])  
```  
  
 <span data-ttu-id="aacdd-965">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-965">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="aacdd-966">Van bármilyen érvényes karakterlánc-kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-966">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="aacdd-967">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-967">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-968">Egy karakterlánc-kifejezés adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-968">Returns a string expression.</span></span>  
  
 <span data-ttu-id="aacdd-969">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-969">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-970">a következő példa beolvasása összefűzendő hello karakterlánc hello hello megadott értéket.</span><span class="sxs-lookup"><span data-stu-id="aacdd-970">hello following example returns hello concatenated string of hello specified values.</span></span>  
  
```  
SELECT CONCAT("abc", "def")  
```  
  
 <span data-ttu-id="aacdd-971">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-971">Here is hello result set.</span></span>  
  
```  
[{"$1": "abcdef"}  
```  
  
####  <span data-ttu-id="aacdd-972"><a name="bk_contains"></a>TARTALMAZZA</span><span class="sxs-lookup"><span data-stu-id="aacdd-972"><a name="bk_contains"></a> CONTAINS</span></span>  
 <span data-ttu-id="aacdd-973">Hogy hello első karakterlánc-kifejezés második tartalmaz-e hello jelző logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="aacdd-973">Returns a Boolean indicating whether hello first string expression contains hello second.</span></span>  
  
 <span data-ttu-id="aacdd-974">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-974">**Syntax**</span></span>  
  
```  
CONTAINS(<str_expr>, <str_expr>)  
```  
  
 <span data-ttu-id="aacdd-975">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-975">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="aacdd-976">Van bármilyen érvényes karakterlánc-kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-976">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="aacdd-977">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-977">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-978">Egy logikai kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-978">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="aacdd-979">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-979">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-980">hello alábbi példa ellenőrzi, ha az "abc" tartalmazza az "ab", és tartalmazza a "d".</span><span class="sxs-lookup"><span data-stu-id="aacdd-980">hello following example checks if "abc" contains "ab" and contains "d".</span></span>  
  
```  
SELECT CONTAINS("abc", "ab"), CONTAINS("abc", "d")  
```  
  
 <span data-ttu-id="aacdd-981">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-981">Here is hello result set.</span></span>  
  
```  
[{"$1": true, "$2": false}]  
```  
  
####  <span data-ttu-id="aacdd-982"><a name="bk_endswith"></a>MEGADOTT MÓDON VÉGZŐDŐ</span><span class="sxs-lookup"><span data-stu-id="aacdd-982"><a name="bk_endswith"></a> ENDSWITH</span></span>  
 <span data-ttu-id="aacdd-983">E hello első karaktersorozat végződik hello második jelző logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="aacdd-983">Returns a Boolean indicating whether hello first string expression ends with hello second.</span></span>  
  
 <span data-ttu-id="aacdd-984">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-984">**Syntax**</span></span>  
  
```  
ENDSWITH(<str_expr>, <str_expr>)  
```  
  
 <span data-ttu-id="aacdd-985">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-985">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="aacdd-986">Van bármilyen érvényes karakterlánc-kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-986">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="aacdd-987">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-987">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-988">Egy logikai kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-988">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="aacdd-989">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-989">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-990">hello következő példa eredménye "abc" karakterlánccal végződik-e a "b" és "bc" hello.</span><span class="sxs-lookup"><span data-stu-id="aacdd-990">hello following example returns hello "abc" ends with "b" and "bc".</span></span>  
  
```  
SELECT ENDSWITH("abc", "b"), ENDSWITH("abc", "bc")  
```  
  
 <span data-ttu-id="aacdd-991">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-991">Here is hello result set.</span></span>  
  
```  
[{"$1": false, "$2": true}]  
```  
  
####  <span data-ttu-id="aacdd-992"><a name="bk_index_of"></a>INDEX_OF</span><span class="sxs-lookup"><span data-stu-id="aacdd-992"><a name="bk_index_of"></a> INDEX_OF</span></span>  
 <span data-ttu-id="aacdd-993">Hello hello második karakterlánc-kifejezés hello első megadott karakterlánc-kifejezés vagy-1 érték első előfordulásának pozícióját a indítása, ha hello karakterlánc nem található hello adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-993">Returns hello starting position of hello first occurrence of hello second string expression within hello first specified string expression, or -1 if hello string is not found.</span></span>  
  
 <span data-ttu-id="aacdd-994">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-994">**Syntax**</span></span>  
  
```  
INDEX_OF(<str_expr>, <str_expr>)  
```  
  
 <span data-ttu-id="aacdd-995">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-995">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="aacdd-996">Van bármilyen érvényes karakterlánc-kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-996">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="aacdd-997">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-997">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-998">Egy numerikus kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-998">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-999">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-999">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-1000">hello alábbi példa adja vissza "abc" belül különböző karakterláncrészletek hello indexét.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1000">hello following example returns hello index of various substrings inside "abc".</span></span>  
  
```  
SELECT INDEX_OF("abc", "ab"), INDEX_OF("abc", "b"), INDEX_OF("abc", "c")  
```  
  
 <span data-ttu-id="aacdd-1001">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1001">Here is hello result set.</span></span>  
  
```  
[{"$1": 0, "$2": 1, "$3": -1}]  
```  
  
####  <span data-ttu-id="aacdd-1002"><a name="bk_left"></a>BALRA</span><span class="sxs-lookup"><span data-stu-id="aacdd-1002"><a name="bk_left"></a> LEFT</span></span>  
 <span data-ttu-id="aacdd-1003">Értéket ad vissza egy karakterlánc bal oldalának hello megadott hello a karakterek száma.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1003">Returns hello left part of a string with hello specified number of characters.</span></span>  
  
 <span data-ttu-id="aacdd-1004">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1004">**Syntax**</span></span>  
  
```  
LEFT(<str_expr>, <num_expr>)  
```  
  
 <span data-ttu-id="aacdd-1005">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1005">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="aacdd-1006">Van bármilyen érvényes karakterlánc-kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1006">Is any valid string expression.</span></span>  
  
-   `num_expr`  
  
     <span data-ttu-id="aacdd-1007">Ez bármilyen érvényes numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1007">Is any valid numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-1008">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1008">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-1009">Egy karakterlánc-kifejezés adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1009">Returns a string expression.</span></span>  
  
 <span data-ttu-id="aacdd-1010">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1010">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-1011">hello alábbi példa adja vissza hello különböző hosszúságú értékek az "abc" része maradt.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1011">hello following example returns hello left part of "abc" for various length values.</span></span>  
  
```  
SELECT LEFT("abc", 1), LEFT("abc", 2)  
```  
  
 <span data-ttu-id="aacdd-1012">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1012">Here is hello result set.</span></span>  
  
```  
[{"$1": "ab", "$2": "ab"}]  
```  
  
####  <span data-ttu-id="aacdd-1013"><a name="bk_length"></a>HOSSZA</span><span class="sxs-lookup"><span data-stu-id="aacdd-1013"><a name="bk_length"></a> LENGTH</span></span>  
 <span data-ttu-id="aacdd-1014">A megadott karakterlánc-kifejezés hello karakterét hello számát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1014">Returns hello number of characters of hello specified string expression.</span></span>  
  
 <span data-ttu-id="aacdd-1015">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1015">**Syntax**</span></span>  
  
```  
LENGTH(<str_expr>)  
```  
  
 <span data-ttu-id="aacdd-1016">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1016">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="aacdd-1017">Van bármilyen érvényes karakterlánc-kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1017">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="aacdd-1018">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1018">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-1019">Egy karakterlánc-kifejezés adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1019">Returns a string expression.</span></span>  
  
 <span data-ttu-id="aacdd-1020">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1020">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-1021">hello alábbi példa adja vissza a karakterlánc hossza hello.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1021">hello following example returns hello length of a string.</span></span>  
  
```  
SELECT LENGTH("abc")  
```  
  
 <span data-ttu-id="aacdd-1022">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1022">Here is hello result set.</span></span>  
  
```  
[{"$1": 3}]  
```  
  
####  <span data-ttu-id="aacdd-1023"><a name="bk_lower"></a>ALACSONYABB</span><span class="sxs-lookup"><span data-stu-id="aacdd-1023"><a name="bk_lower"></a> LOWER</span></span>  
 <span data-ttu-id="aacdd-1024">Egy karakterlánc-kifejezés nagybetűt adatok toolowercase átalakítása után adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1024">Returns a string expression after converting uppercase character data toolowercase.</span></span>  
  
 <span data-ttu-id="aacdd-1025">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1025">**Syntax**</span></span>  
  
```  
LOWER(<str_expr>)  
```  
  
 <span data-ttu-id="aacdd-1026">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1026">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="aacdd-1027">Van bármilyen érvényes karakterlánc-kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1027">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="aacdd-1028">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1028">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-1029">Egy karakterlánc-kifejezés adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1029">Returns a string expression.</span></span>  
  
 <span data-ttu-id="aacdd-1030">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1030">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-1031">a következő példa azt mutatja meg hogyan hello toouse ALACSONYABB a lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1031">hello following example shows how toouse LOWER in a query.</span></span>  
  
```  
SELECT LOWER("Abc")  
```  
  
 <span data-ttu-id="aacdd-1032">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1032">Here is hello result set.</span></span>  
  
```  
[{"$1": "abc"}]  
  
```  
  
####  <span data-ttu-id="aacdd-1033"><a name="bk_ltrim"></a>LTRIM</span><span class="sxs-lookup"><span data-stu-id="aacdd-1033"><a name="bk_ltrim"></a> LTRIM</span></span>  
 <span data-ttu-id="aacdd-1034">Egy karakterlánc-kifejezés adja vissza, után eltávolítja a kezdő üres.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1034">Returns a string expression after it removes leading blanks.</span></span>  
  
 <span data-ttu-id="aacdd-1035">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1035">**Syntax**</span></span>  
  
```  
LTRIM(<str_expr>)  
```  
  
 <span data-ttu-id="aacdd-1036">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1036">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="aacdd-1037">Van bármilyen érvényes karakterlánc-kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1037">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="aacdd-1038">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1038">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-1039">Egy karakterlánc-kifejezés adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1039">Returns a string expression.</span></span>  
  
 <span data-ttu-id="aacdd-1040">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1040">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-1041">a következő példa azt mutatja meg hogyan hello toouse LTRIM egy lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1041">hello following example shows how toouse LTRIM inside a query.</span></span>  
  
```  
SELECT LTRIM("  abc"), LTRIM("abc"), LTRIM("abc   ")  
```  
  
 <span data-ttu-id="aacdd-1042">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1042">Here is hello result set.</span></span>  
  
```  
[{"$1": "abc", "$2": "abc", "$3": "abc   "}]  
```  
  
####  <span data-ttu-id="aacdd-1043"><a name="bk_replace"></a>CSERÉLJE LE</span><span class="sxs-lookup"><span data-stu-id="aacdd-1043"><a name="bk_replace"></a> REPLACE</span></span>  
 <span data-ttu-id="aacdd-1044">Megadott karakterlánc-érték összes előfordulását lecseréli egy másik karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1044">Replaces all occurrences of a specified string value with another string value.</span></span>  
  
 <span data-ttu-id="aacdd-1045">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1045">**Syntax**</span></span>  
  
```  
REPLACE(<str_expr>, <str_expr>, <str_expr>)  
```  
  
 <span data-ttu-id="aacdd-1046">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1046">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="aacdd-1047">Van bármilyen érvényes karakterlánc-kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1047">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="aacdd-1048">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1048">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-1049">Egy karakterlánc-kifejezés adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1049">Returns a string expression.</span></span>  
  
 <span data-ttu-id="aacdd-1050">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1050">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-1051">hello a következő példa bemutatja, hogyan toouse cserélje le a lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1051">hello following example shows how toouse REPLACE in a query.</span></span>  
  
```  
SELECT REPLACE("This is a Test", "Test", "desk")  
```  
  
 <span data-ttu-id="aacdd-1052">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1052">Here is hello result set.</span></span>  
  
```  
[{"$1": "This is a desk"}]  
```  
  
####  <span data-ttu-id="aacdd-1053"><a name="bk_replicate"></a>REPLIKÁLÁS</span><span class="sxs-lookup"><span data-stu-id="aacdd-1053"><a name="bk_replicate"></a> REPLICATE</span></span>  
 <span data-ttu-id="aacdd-1054">A megadott számú alkalommal megismétel egy karakterlánc-érték.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1054">Repeats a string value a specified number of times.</span></span>  
  
 <span data-ttu-id="aacdd-1055">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1055">**Syntax**</span></span>  
  
```  
REPLICATE(<str_expr>, <num_expr>)  
```  
  
 <span data-ttu-id="aacdd-1056">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1056">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="aacdd-1057">Van bármilyen érvényes karakterlánc-kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1057">Is any valid string expression.</span></span>  
  
-   `num_expr`  
  
     <span data-ttu-id="aacdd-1058">Ez bármilyen érvényes numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1058">Is any valid numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-1059">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1059">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-1060">Egy karakterlánc-kifejezés adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1060">Returns a string expression.</span></span>  
  
 <span data-ttu-id="aacdd-1061">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1061">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-1062">hello a következő példa bemutatja, hogyan toouse REPLIKÁLNI a lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1062">hello following example shows how toouse REPLICATE in a query.</span></span>  
  
```  
SELECT REPLICATE("a", 3)  
```  
  
 <span data-ttu-id="aacdd-1063">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1063">Here is hello result set.</span></span>  
  
```  
[{"$1": "aaa"}]  
```  
  
####  <span data-ttu-id="aacdd-1064"><a name="bk_reverse"></a>FORDÍTOTT</span><span class="sxs-lookup"><span data-stu-id="aacdd-1064"><a name="bk_reverse"></a> REVERSE</span></span>  
 <span data-ttu-id="aacdd-1065">Hello fordított sorrendben egy karakterlánc értékét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1065">Returns hello reverse order of a string value.</span></span>  
  
 <span data-ttu-id="aacdd-1066">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1066">**Syntax**</span></span>  
  
```  
REVERSE(<str_expr>)  
```  
  
 <span data-ttu-id="aacdd-1067">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1067">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="aacdd-1068">Van bármilyen érvényes karakterlánc-kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1068">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="aacdd-1069">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1069">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-1070">Egy karakterlánc-kifejezés adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1070">Returns a string expression.</span></span>  
  
 <span data-ttu-id="aacdd-1071">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1071">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-1072">hello a következő példa bemutatja, hogyan toouse fordított a lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1072">hello following example shows how toouse REVERSE in a query.</span></span>  
  
```  
SELECT REVERSE("Abc")  
```  
  
 <span data-ttu-id="aacdd-1073">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1073">Here is hello result set.</span></span>  
  
```  
[{"$1": "cbA"}]  
```  
  
####  <span data-ttu-id="aacdd-1074"><a name="bk_right"></a>JOBBRA</span><span class="sxs-lookup"><span data-stu-id="aacdd-1074"><a name="bk_right"></a> RIGHT</span></span>  
 <span data-ttu-id="aacdd-1075">A hello jobb része egy karakterlánc beolvasása hello megadott karakterek száma.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1075">Returns hello right part of a string with hello specified number of characters.</span></span>  
  
 <span data-ttu-id="aacdd-1076">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1076">**Syntax**</span></span>  
  
```  
RIGHT(<str_expr>, <num_expr>)  
```  
  
 <span data-ttu-id="aacdd-1077">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1077">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="aacdd-1078">Van bármilyen érvényes karakterlánc-kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1078">Is any valid string expression.</span></span>  
  
-   `num_expr`  
  
     <span data-ttu-id="aacdd-1079">Ez bármilyen érvényes numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1079">Is any valid numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-1080">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1080">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-1081">Egy karakterlánc-kifejezés adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1081">Returns a string expression.</span></span>  
  
 <span data-ttu-id="aacdd-1082">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1082">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-1083">hello alábbi példa részét adja vissza hello jobbra "abc" különböző hosszúságú értékek.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1083">hello following example returns hello right part of "abc" for various length values.</span></span>  
  
```  
SELECT RIGHT("abc", 1), RIGHT("abc", 2)  
```  
  
 <span data-ttu-id="aacdd-1084">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1084">Here is hello result set.</span></span>  
  
```  
[{"$1": "c", "$2": "bc"}]  
```  
  
####  <span data-ttu-id="aacdd-1085"><a name="bk_rtrim"></a>RTRIM</span><span class="sxs-lookup"><span data-stu-id="aacdd-1085"><a name="bk_rtrim"></a> RTRIM</span></span>  
 <span data-ttu-id="aacdd-1086">Egy karakterlánc-kifejezés adja vissza, miután eltávolítja a záró szóközöket.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1086">Returns a string expression after it removes trailing blanks.</span></span>  
  
 <span data-ttu-id="aacdd-1087">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1087">**Syntax**</span></span>  
  
```  
RTRIM(<str_expr>)  
```  
  
 <span data-ttu-id="aacdd-1088">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1088">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="aacdd-1089">Van bármilyen érvényes karakterlánc-kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1089">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="aacdd-1090">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1090">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-1091">Egy karakterlánc-kifejezés adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1091">Returns a string expression.</span></span>  
  
 <span data-ttu-id="aacdd-1092">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1092">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-1093">a következő példa azt mutatja meg hogyan hello toouse RTRIM egy lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1093">hello following example shows how toouse RTRIM inside a query.</span></span>  
  
```  
SELECT RTRIM("  abc"), RTRIM("abc"), RTRIM("abc   ")  
```  
  
 <span data-ttu-id="aacdd-1094">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1094">Here is hello result set.</span></span>  
  
```  
[{"$1": "   abc", "$2": "abc", "$3": "abc"}]  
```  
  
####  <span data-ttu-id="aacdd-1095"><a name="bk_startswith"></a>STARTSWITH ELEMNEK</span><span class="sxs-lookup"><span data-stu-id="aacdd-1095"><a name="bk_startswith"></a> STARTSWITH</span></span>  
 <span data-ttu-id="aacdd-1096">Hogy hello első karakterlánc-kifejezés indításakor hello második jelző logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1096">Returns a Boolean indicating whether hello first string expression starts with hello second.</span></span>  
  
 <span data-ttu-id="aacdd-1097">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1097">**Syntax**</span></span>  
  
```  
STARTSWITH(<str_expr>, <str_expr>)  
```  
  
 <span data-ttu-id="aacdd-1098">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1098">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="aacdd-1099">Van bármilyen érvényes karakterlánc-kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1099">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="aacdd-1100">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1100">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-1101">Egy logikai kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1101">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="aacdd-1102">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1102">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-1103">hello alábbi példa ellenőrzések Ha hello "abc" karakterlánc első lépése a "b" és "a".</span><span class="sxs-lookup"><span data-stu-id="aacdd-1103">hello following example checks if hello string "abc" begins with "b" and "a".</span></span>  
  
```  
SELECT STARTSWITH("abc", "b"), STARTSWITH("abc", "a")  
```  
  
 <span data-ttu-id="aacdd-1104">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1104">Here is hello result set.</span></span>  
  
```  
[{"$1": false, "$2": true}]  
```  
  
####  <span data-ttu-id="aacdd-1105"><a name="bk_substring"></a>SUBSTRING</span><span class="sxs-lookup"><span data-stu-id="aacdd-1105"><a name="bk_substring"></a> SUBSTRING</span></span>  
 <span data-ttu-id="aacdd-1106">Hello kezdődő karakterlánc-kifejezés részét adja vissza a megadott karakter nulláról indulva számolt helyzetét, és folytatja a toohello megadott hosszúság vagy hello karakterlánc toohello végét.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1106">Returns part of a string expression starting at hello specified character zero-based position and continues toohello specified length, or toohello end of hello string.</span></span>  
  
 <span data-ttu-id="aacdd-1107">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1107">**Syntax**</span></span>  
  
```  
SUBSTRING(<str_expr>, <num_expr> [, <num_expr>])  
```  
  
 <span data-ttu-id="aacdd-1108">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1108">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="aacdd-1109">Van bármilyen érvényes karakterlánc-kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1109">Is any valid string expression.</span></span>  
  
-   `num_expr`  
  
     <span data-ttu-id="aacdd-1110">Ez bármilyen érvényes numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1110">Is any valid numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-1111">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1111">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-1112">Egy karakterlánc-kifejezés adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1112">Returns a string expression.</span></span>  
  
 <span data-ttu-id="aacdd-1113">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1113">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-1114">hello alábbi példa részét adja vissza hello "abc" kezdődően: 1 és 1 karakter hosszúságú.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1114">hello following example returns hello substring of "abc" starting at 1 and for a length of 1 character.</span></span>  
  
```  
SELECT SUBSTRING("abc", 1, 1)  
```  
  
 <span data-ttu-id="aacdd-1115">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1115">Here is hello result set.</span></span>  
  
```  
[{"$1": "b"}]  
```  
  
####  <span data-ttu-id="aacdd-1116"><a name="bk_upper"></a>FELSŐ</span><span class="sxs-lookup"><span data-stu-id="aacdd-1116"><a name="bk_upper"></a> UPPER</span></span>  
 <span data-ttu-id="aacdd-1117">Egy karakterlánc-kifejezés kisbetűt adatok toouppercase átalakítása után adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1117">Returns a string expression after converting lowercase character data toouppercase.</span></span>  
  
 <span data-ttu-id="aacdd-1118">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1118">**Syntax**</span></span>  
  
```  
UPPER(<str_expr>)  
```  
  
 <span data-ttu-id="aacdd-1119">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1119">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="aacdd-1120">Van bármilyen érvényes karakterlánc-kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1120">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="aacdd-1121">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1121">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-1122">Egy karakterlánc-kifejezés adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1122">Returns a string expression.</span></span>  
  
 <span data-ttu-id="aacdd-1123">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1123">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-1124">a következő példa azt mutatja meg hogyan hello toouse felső lekérdezés</span><span class="sxs-lookup"><span data-stu-id="aacdd-1124">hello following example shows how toouse UPPER in a query</span></span>  
  
```  
SELECT UPPER("Abc")  
```  
  
 <span data-ttu-id="aacdd-1125">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1125">Here is hello result set.</span></span>  
  
```  
[{"$1": "ABC"}]  
```  
  
###  <span data-ttu-id="aacdd-1126"><a name="bk_array_functions"></a>A tömb funkciók</span><span class="sxs-lookup"><span data-stu-id="aacdd-1126"><a name="bk_array_functions"></a> Array functions</span></span>  
 <span data-ttu-id="aacdd-1127">a következő skaláris függvények hello végrehajtania egy műveletet a egy tömb bemeneti érték és a numerikus visszatérési, a logikai érték vagy tömb érték</span><span class="sxs-lookup"><span data-stu-id="aacdd-1127">hello following scalar functions perform an operation on an array input value and return numeric, Boolean or array value</span></span>  
  
||||  
|-|-|-|  
|[<span data-ttu-id="aacdd-1128">ARRAY_CONCAT</span><span class="sxs-lookup"><span data-stu-id="aacdd-1128">ARRAY_CONCAT</span></span>](#bk_array_concat)|[<span data-ttu-id="aacdd-1129">ARRAY_CONTAINS</span><span class="sxs-lookup"><span data-stu-id="aacdd-1129">ARRAY_CONTAINS</span></span>](#bk_array_contains)|[<span data-ttu-id="aacdd-1130">ARRAY_LENGTH</span><span class="sxs-lookup"><span data-stu-id="aacdd-1130">ARRAY_LENGTH</span></span>](#bk_array_length)|  
|[<span data-ttu-id="aacdd-1131">ARRAY_SLICE</span><span class="sxs-lookup"><span data-stu-id="aacdd-1131">ARRAY_SLICE</span></span>](#bk_array_slice)|||  
  
####  <span data-ttu-id="aacdd-1132"><a name="bk_array_concat"></a>ARRAY_CONCAT</span><span class="sxs-lookup"><span data-stu-id="aacdd-1132"><a name="bk_array_concat"></a> ARRAY_CONCAT</span></span>  
 <span data-ttu-id="aacdd-1133">Egy tömb, amely két vagy több tömb értékek hozzáfűzésével hello eredményét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1133">Returns an array that is hello result of concatenating two or more array values.</span></span>  
  
 <span data-ttu-id="aacdd-1134">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1134">**Syntax**</span></span>  
  
```  
ARRAY_CONCAT (<arr_expr>, <arr_expr> [, <arr_expr>])  
```  
  
 <span data-ttu-id="aacdd-1135">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1135">**Arguments**</span></span>  
  
-   `arr_expr`  
  
     <span data-ttu-id="aacdd-1136">Van bármilyen érvényes tömböt megadó kifejezést.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1136">Is any valid array expression.</span></span>  
  
 <span data-ttu-id="aacdd-1137">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1137">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-1138">Egy tömböt megadó kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1138">Returns an array expression.</span></span>  
  
 <span data-ttu-id="aacdd-1139">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1139">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-1140">hello következő hogyan két tooconcatenate tömbállandó példa.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1140">hello following example how tooconcatenate two arrays.</span></span>  
  
```  
SELECT ARRAY_CONCAT(["apples", "strawberries"], ["bananas"])  
```  
  
 <span data-ttu-id="aacdd-1141">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1141">Here is hello result set.</span></span>  
  
```  
[{"$1": ["apples", "strawberries", "bananas"]}]  
```  
  
####  <span data-ttu-id="aacdd-1142"><a name="bk_array_contains"></a>ARRAY_CONTAINS</span><span class="sxs-lookup"><span data-stu-id="aacdd-1142"><a name="bk_array_contains"></a> ARRAY_CONTAINS</span></span>  
 <span data-ttu-id="aacdd-1143">Vissza logikai érték, amely azt jelzi, hogy tartalmaz-e hello hello tömb a megadott érték.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1143">Returns a Boolean indicating whether hello array contains hello specified value.</span></span>  
  
 <span data-ttu-id="aacdd-1144">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1144">**Syntax**</span></span>  
  
```  
ARRAY_CONTAINS (<arr_expr>, <expr>)  
```  
  
 <span data-ttu-id="aacdd-1145">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1145">**Arguments**</span></span>  
  
-   `arr_expr`  
  
     <span data-ttu-id="aacdd-1146">Van bármilyen érvényes tömböt megadó kifejezést.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1146">Is any valid array expression.</span></span>  
  
-   `expr`  
  
     <span data-ttu-id="aacdd-1147">Ez bármilyen érvényes kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1147">Is any valid expression.</span></span>  
  
 <span data-ttu-id="aacdd-1148">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1148">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-1149">Egy logikai értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1149">Returns a Boolean value.</span></span>  
  
 <span data-ttu-id="aacdd-1150">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1150">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-1151">a következő példa hogyan hello toocheck tagság használatával ARRAY_CONTAINS tömbben.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1151">hello following example how toocheck for membership in an array using ARRAY_CONTAINS.</span></span>  
  
```  
SELECT   
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "apples"),  
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "mangoes")  
```  
  
 <span data-ttu-id="aacdd-1152">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1152">Here is hello result set.</span></span>  
  
```  
[{"$1": true, "$2": false}]  
```  
  
####  <span data-ttu-id="aacdd-1153"><a name="bk_array_length"></a>ARRAY_LENGTH</span><span class="sxs-lookup"><span data-stu-id="aacdd-1153"><a name="bk_array_length"></a> ARRAY_LENGTH</span></span>  
 <span data-ttu-id="aacdd-1154">Hello elemeinek hello számát adja vissza a megadott tömb kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1154">Returns hello number of elements of hello specified array expression.</span></span>  
  
 <span data-ttu-id="aacdd-1155">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1155">**Syntax**</span></span>  
  
```  
ARRAY_LENGTH(<arr_expr>)  
```  
  
 <span data-ttu-id="aacdd-1156">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1156">**Arguments**</span></span>  
  
-   `arr_expr`  
  
     <span data-ttu-id="aacdd-1157">Van bármilyen érvényes tömböt megadó kifejezést.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1157">Is any valid array expression.</span></span>  
  
 <span data-ttu-id="aacdd-1158">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1158">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-1159">Egy numerikus kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1159">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-1160">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1160">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-1161">hello következő példa hogyan tooget hello segítségével ARRAY_LENGTH tömb hossza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1161">hello following example how tooget hello length of an array using ARRAY_LENGTH.</span></span>  
  
```  
SELECT ARRAY_LENGTH(["apples", "strawberries", "bananas"])  
```  
  
 <span data-ttu-id="aacdd-1162">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1162">Here is hello result set.</span></span>  
  
```  
[{"$1": 3}]  
```  
  
####  <span data-ttu-id="aacdd-1163"><a name="bk_array_slice"></a>ARRAY_SLICE</span><span class="sxs-lookup"><span data-stu-id="aacdd-1163"><a name="bk_array_slice"></a> ARRAY_SLICE</span></span>  
 <span data-ttu-id="aacdd-1164">Egy tömböt megadó kifejezést részét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1164">Returns part of an array expression.</span></span>
  
 <span data-ttu-id="aacdd-1165">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1165">**Syntax**</span></span>  
  
```  
ARRAY_SLICE (<arr_expr>, <num_expr> [, <num_expr>])  
```  
  
 <span data-ttu-id="aacdd-1166">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1166">**Arguments**</span></span>  
  
-   `arr_expr`  
  
     <span data-ttu-id="aacdd-1167">Van bármilyen érvényes tömböt megadó kifejezést.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1167">Is any valid array expression.</span></span>  
  
-   `num_expr`  
  
     <span data-ttu-id="aacdd-1168">Ez bármilyen érvényes numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1168">Is any valid numeric expression.</span></span>  
  
 <span data-ttu-id="aacdd-1169">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1169">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-1170">Egy logikai értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1170">Returns a Boolean value.</span></span>  
  
 <span data-ttu-id="aacdd-1171">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1171">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-1172">a következő példa hogyan hello tooget ARRAY_SLICE használatával tömb részét.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1172">hello following example how tooget a part of an array using ARRAY_SLICE.</span></span>  
  
```  
SELECT   
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1),  
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1)  
```  
  
 <span data-ttu-id="aacdd-1173">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1173">Here is hello result set.</span></span>  
  
```  
[{  
           "$1": ["strawberries", "bananas"],   
           "$2": ["strawberries"]  
       }]  
```  
  
###  <span data-ttu-id="aacdd-1174"><a name="bk_spatial_functions"></a>Térbeli funkciók</span><span class="sxs-lookup"><span data-stu-id="aacdd-1174"><a name="bk_spatial_functions"></a> Spatial functions</span></span>  
 <span data-ttu-id="aacdd-1175">hello következő skaláris függvények végrehajtania egy műveletet egy térbeli objektum bemeneti érték a és numerikus vagy logikai érték visszaadása.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1175">hello following scalar functions perform an operation on an spatial object input value and return a numeric or Boolean value.</span></span>  
  
||||  
|-|-|-|  
|[<span data-ttu-id="aacdd-1176">ST_DISTANCE</span><span class="sxs-lookup"><span data-stu-id="aacdd-1176">ST_DISTANCE</span></span>](#bk_st_distance)|[<span data-ttu-id="aacdd-1177">ST_WITHIN</span><span class="sxs-lookup"><span data-stu-id="aacdd-1177">ST_WITHIN</span></span>](#bk_st_within)|[<span data-ttu-id="aacdd-1178">ST_INTERSECTS</span><span class="sxs-lookup"><span data-stu-id="aacdd-1178">ST_INTERSECTS</span></span>](#bk_st_intersects)|[<span data-ttu-id="aacdd-1179">ST_ISVALID</span><span class="sxs-lookup"><span data-stu-id="aacdd-1179">ST_ISVALID</span></span>](#bk_st_isvalid)|  
|[<span data-ttu-id="aacdd-1180">ST_ISVALIDDETAILED</span><span class="sxs-lookup"><span data-stu-id="aacdd-1180">ST_ISVALIDDETAILED</span></span>](#bk_st_isvaliddetailed)|||  
  
####  <span data-ttu-id="aacdd-1181"><a name="bk_st_distance"></a>ST_DISTANCE</span><span class="sxs-lookup"><span data-stu-id="aacdd-1181"><a name="bk_st_distance"></a> ST_DISTANCE</span></span>  
 <span data-ttu-id="aacdd-1182">A két hello GeoJSON-pont, sokszög vagy LineString kifejezések hello távolság visszaadása.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1182">Returns hello distance between hello two GeoJSON Point, Polygon, or LineString expressions.</span></span>  
  
 <span data-ttu-id="aacdd-1183">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1183">**Syntax**</span></span>  
  
```  
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  
  
 <span data-ttu-id="aacdd-1184">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1184">**Arguments**</span></span>  
  
-   `spatial_expr`  
  
     <span data-ttu-id="aacdd-1185">Ez az egyetlen érvényes GeoJSON-pont, sokszög vagy LineString objektum kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1185">Is any valid GeoJSON Point, Polygon, or LineString object expression.</span></span>  
  
 <span data-ttu-id="aacdd-1186">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1186">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-1187">Egy numerikus kifejezés tartalmazó hello távolságot adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1187">Returns a numeric expression containing hello distance.</span></span> <span data-ttu-id="aacdd-1188">Ez a mérőszámok hello alapértelmezett referenciarendszer szerint megadva.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1188">This is expressed in meters for hello default reference system.</span></span>  
  
 <span data-ttu-id="aacdd-1189">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1189">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-1190">a következő példa azt mutatja meg hogyan hello tooreturn 30 km-hello belüli összes családba tartozó dokumentumok megadott hely hello ST_DISTANCE beépített függvény használatával.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1190">hello following example shows how tooreturn all family documents that are within 30 km of hello specified location using hello ST_DISTANCE built-in function.</span></span> <span data-ttu-id="aacdd-1191">.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1191">.</span></span>  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000  
```  
  
 <span data-ttu-id="aacdd-1192">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1192">Here is hello result set.</span></span>  
  
```  
[{  
  "id": "WakefieldFamily"  
}]  
```  
  
####  <span data-ttu-id="aacdd-1193"><a name="bk_st_within"></a>ST_WITHIN</span><span class="sxs-lookup"><span data-stu-id="aacdd-1193"><a name="bk_st_within"></a> ST_WITHIN</span></span>  
 <span data-ttu-id="aacdd-1194">Visszaadja a egy logikai kifejezés, amely jelzi, hogy hello GeoJSON objektum (pont, Polygon, vagy LineString) hello első argumentumban megadott hello GeoJSON (pont, sokszög vagy LineString) belül hello második argumentum.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1194">Returns a Boolean expression indicating whether hello GeoJSON object (Point, Polygon, or LineString) specified in hello first argument is within hello GeoJSON (Point, Polygon, or LineString) in hello second argument.</span></span>  
  
 <span data-ttu-id="aacdd-1195">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1195">**Syntax**</span></span>  
  
```  
ST_WITHIN (<spatial_expr>, <spatial_expr>)  
```  
  
 <span data-ttu-id="aacdd-1196">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1196">**Arguments**</span></span>  
  
-   `spatial_expr`  
  
     <span data-ttu-id="aacdd-1197">Ez az egyetlen érvényes GeoJSON-pont, sokszög vagy LineString objektum kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1197">Is any valid GeoJSON Point, Polygon, or LineString object expression.</span></span>  
 
-   `spatial_expr`  
  
     <span data-ttu-id="aacdd-1198">Ez az egyetlen érvényes GeoJSON-pont, sokszög vagy LineString objektum kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1198">Is any valid GeoJSON Point, Polygon, or LineString object expression.</span></span>  
  
 <span data-ttu-id="aacdd-1199">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1199">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-1200">Egy logikai értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1200">Returns a Boolean value.</span></span>  
  
 <span data-ttu-id="aacdd-1201">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1201">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-1202">hello a következő példa bemutatja, hogyan toofind összes termékcsalád dokumentumok használatával ST_WITHIN sokszög belül.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1202">hello following example shows how toofind all family documents within a polygon using ST_WITHIN.</span></span>  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_WITHIN(f.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 <span data-ttu-id="aacdd-1203">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1203">Here is hello result set.</span></span>  
  
```  
[{ "id": "WakefieldFamily" }]  
```  

####  <span data-ttu-id="aacdd-1204"><a name="bk_st_intersects"></a>ST_INTERSECTS</span><span class="sxs-lookup"><span data-stu-id="aacdd-1204"><a name="bk_st_intersects"></a> ST_INTERSECTS</span></span>  
 <span data-ttu-id="aacdd-1205">Egy logikai kifejezés, amely azt jelzi, hogy hello GeoJSON objektum (pont, Polygon, vagy LineString) hello első argumentumban megadott metszi hello második argumentum (pont, sokszög vagy LineString) GeoJSON hello adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1205">Returns a Boolean expression indicating whether hello GeoJSON object (Point, Polygon, or LineString) specified in hello first argument intersects hello GeoJSON (Point, Polygon, or LineString) in hello second argument.</span></span>  
  
 <span data-ttu-id="aacdd-1206">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1206">**Syntax**</span></span>  
  
```  
ST_INTERSECTS (<spatial_expr>, <spatial_expr>)  
```  
  
 <span data-ttu-id="aacdd-1207">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1207">**Arguments**</span></span>  
  
-   `spatial_expr`  
  
     <span data-ttu-id="aacdd-1208">Ez az egyetlen érvényes GeoJSON-pont, sokszög vagy LineString objektum kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1208">Is any valid GeoJSON Point, Polygon, or LineString object expression.</span></span>  
 
-   `spatial_expr`  
  
     <span data-ttu-id="aacdd-1209">Ez az egyetlen érvényes GeoJSON-pont, sokszög vagy LineString objektum kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1209">Is any valid GeoJSON Point, Polygon, or LineString object expression.</span></span>  
  
 <span data-ttu-id="aacdd-1210">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1210">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-1211">Egy logikai értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1211">Returns a Boolean value.</span></span>  
  
 <span data-ttu-id="aacdd-1212">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1212">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-1213">a következő példa azt mutatja meg hogyan hello minden területet engedélyez, amelyek a metszi hello megadott sokszög toofind.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1213">hello following example shows how toofind all areas that intersects with hello given polygon.</span></span>  
  
```  
SELECT a.id   
FROM Areas a   
WHERE ST_INTERSECTS(a.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 <span data-ttu-id="aacdd-1214">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1214">Here is hello result set.</span></span>  
  
```  
[{ "id": "IntersectingPolygon" }]  
```  
  
####  <span data-ttu-id="aacdd-1215"><a name="bk_st_isvalid"></a>ST_ISVALID</span><span class="sxs-lookup"><span data-stu-id="aacdd-1215"><a name="bk_st_isvalid"></a> ST_ISVALID</span></span>  
 <span data-ttu-id="aacdd-1216">Jelzi, hogy hello megadott GeoJSON-pont, sokszög vagy LineString kifejezés érvényes logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1216">Returns a Boolean value indicating whether hello specified GeoJSON Point, Polygon, or LineString expression is valid.</span></span>  
  
 <span data-ttu-id="aacdd-1217">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1217">**Syntax**</span></span>  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 <span data-ttu-id="aacdd-1218">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1218">**Arguments**</span></span>  
  
-   `spatial_expr`  
  
     <span data-ttu-id="aacdd-1219">Ez az egyetlen érvényes GeoJSON-pont, sokszög vagy LineString kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1219">Is any valid GeoJSON Point, Polygon, or LineString expression.</span></span>  
  
 <span data-ttu-id="aacdd-1220">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1220">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-1221">Egy logikai kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1221">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="aacdd-1222">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1222">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-1223">a következő példa azt mutatja meg hogyan hello toocheck, ha a pont ST_VALID használatával.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1223">hello following example shows how toocheck if a point is valid using ST_VALID.</span></span>  
  
 <span data-ttu-id="aacdd-1224">Például ezt a pontot a szélességi értékkel rendelkezik, amely a értékkel [-90 és 90] hello érvényes tartományban van, ezért hello lekérdezés hamis értéket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1224">For example, this point has a latitude value that's not in hello valid range of values [-90, 90], so hello query returns false.</span></span>  
  
 <span data-ttu-id="aacdd-1225">Sokszögek hello specifikációja megköveteli, hogy legyen-e hello utolsó koordináta pár megadott GeoJSON hello ugyanaz, mint hello első, toocreate zárt alakzat.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1225">For polygons, hello GeoJSON specification requires that hello last coordinate pair provided should be hello same as hello first, toocreate a closed shape.</span></span> <span data-ttu-id="aacdd-1226">Egy sokszögön belül pontok balra érdekében meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1226">Points within a polygon must be specified in counter-clockwise order.</span></span> <span data-ttu-id="aacdd-1227">A sokszög jobbra sorrendben megadva hello régióját, hello inverzét jelöli.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1227">A polygon specified in clockwise order represents hello inverse of hello region within it.</span></span>  
  
```  
SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })  
```  
  
 <span data-ttu-id="aacdd-1228">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1228">Here is hello result set.</span></span>  
  
```  
[{ "$1": false }]  
```  
  
####  <span data-ttu-id="aacdd-1229"><a name="bk_st_isvaliddetailed"></a>ST_ISVALIDDETAILED</span><span class="sxs-lookup"><span data-stu-id="aacdd-1229"><a name="bk_st_isvaliddetailed"></a> ST_ISVALIDDETAILED</span></span>  
 <span data-ttu-id="aacdd-1230">Értéket ad eredményül, ha hello megadott GeoJSON-pont, sokszög vagy LineString kifejezés logikai értéket tartalmazó JSON-érték érvényes, és ha érvénytelen, továbbá hello OK karakterláncként.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1230">Returns a JSON value containing a Boolean value if hello specified GeoJSON Point, Polygon, or LineString expression is valid, and if invalid, additionally hello reason as a string value.</span></span>  
  
 <span data-ttu-id="aacdd-1231">**Szintaxis**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1231">**Syntax**</span></span>  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 <span data-ttu-id="aacdd-1232">**Argumentumok**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1232">**Arguments**</span></span>  
  
-   `spatial_expr`  
  
     <span data-ttu-id="aacdd-1233">Ez az egyetlen érvényes GeoJSON pontot vagy a sokszög kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1233">Is any valid GeoJSON point or polygon expression.</span></span>  
  
 <span data-ttu-id="aacdd-1234">**Visszatérési típusokat**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1234">**Return Types**</span></span>  
  
 <span data-ttu-id="aacdd-1235">Egy logikai értéket tartalmazó hello GeoJSON-pont vagy sokszög kifejezés érvényes, és ha érvénytelen, továbbá hello karakterláncként OK JSON értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1235">Returns a JSON value containing a Boolean value if hello specified GeoJSON point or polygon expression is valid, and if invalid, additionally hello reason as a string value.</span></span>  
  
 <span data-ttu-id="aacdd-1236">**Példák**</span><span class="sxs-lookup"><span data-stu-id="aacdd-1236">**Examples**</span></span>  
  
 <span data-ttu-id="aacdd-1237">a következő példa hogyan hello toocheck érvényességi (adatokkal) ST_ISVALIDDETAILED használatával.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1237">hello following example how toocheck validity (with details) using ST_ISVALIDDETAILED.</span></span>  
  
```  
SELECT ST_ISVALIDDETAILED({   
  "type": "Polygon",   
  "coordinates": [[ [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] ]]  
})  
```  
  
 <span data-ttu-id="aacdd-1238">Itt található hello eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="aacdd-1238">Here is hello result set.</span></span>  
  
```  
[{  
  "$1": {   
    "valid": false,   
    "reason": "hello Polygon input is not valid because hello start and end points of hello ring number 1 are not hello same. Each ring of a polygon must have hello same start and end points."   
  }  
}]  
```  
  
## <a name="next-steps"></a><span data-ttu-id="aacdd-1239">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aacdd-1239">Next steps</span></span>  
 <span data-ttu-id="aacdd-1240">[SQL-szintaxis és Azure Cosmos adatbázis SQL-lekérdezés](documentdb-sql-query.md) </span><span class="sxs-lookup"><span data-stu-id="aacdd-1240">[SQL syntax and SQL query for Azure Cosmos DB](documentdb-sql-query.md) </span></span>  
 [<span data-ttu-id="aacdd-1241">Az Azure Cosmos DB dokumentációja</span><span class="sxs-lookup"><span data-stu-id="aacdd-1241">Azure Cosmos DB documentation</span></span>](https://docs.microsoft.com/en-us/azure/cosmos-db/)  
  
  
