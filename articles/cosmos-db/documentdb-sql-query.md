---
title: "Azure Cosmos DB DocumentDB API aaaSQL lekérdezések |} Microsoft Docs"
description: "A Azure Cosmos DB SQL-szintaxis, adatbázis fogalmait és az SQL-lekérdezések megismerése. SQL Azure Cosmos adatbázis a JSON lekérdezésnyelvet is használja."
keywords: "SQL-szintaxis, sql-lekérdezést, az sql-lekérdezések, json lekérdezési nyelv, adatbázis fogalmait és az sql-lekérdezések, összesítő függvények"
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: a73b4ab3-0786-42fd-b59b-555fce09db6e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: arramac
ms.openlocfilehash: f4db95b87f5796c4e4299aaf016435cb6301bbfe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sql-queries-for-azure-cosmos-db-documentdb-api"></a>Az SQL-lekérdezések Azure Cosmos DB DocumentDB API-hoz.
A Microsoft Azure Cosmos DB támogatja a JSON lekérdezésnyelvet SQL (Structured Query Language) használatával dokumentumok lekérdezését. A cosmos DB valóban sémamentes. A kötelezettségvállalás toohello JSON adatmodell közvetlenül hello adatbázismotor belül, címtár biztosít automatikus indexeléshez JSON-dokumentumok explicit séma vagy a másodlagos indexek létrehozása nélkül. 

A Cosmos DB hello lekérdezési nyelv tervezésekor két célok szem előtt tartásával volt:

* Helyett egy új JSON lekérdező nyelv inventing, akartunk toosupport SQL. SQL hello ismerős és a népszerű lekérdezési nyelv egyike. Cosmos DB SQL formális programozási modellt biztosít a részletes lekérdezéseket JSON-dokumentumok keresztül.
* JSON-adatbázisként dokumentum, amelyek képesek JavaScript közvetlenül a hello adatbázismotor akartunk toouse JavaScript programozási modell hello foundation, a lekérdezési nyelvhez. a JavaScript típusrendszernek, kifejezés kiértékelése és függvényhívások feltörték hello DocumentDB API SQL. Ez szolgálna természetes programozási modellt biztosít a leképezések relációs, hierarchikus navigációs JSON-dokumentumokat, automatikus illesztések, térbeli lekérdezéseket és teljesen egyéb funkciók között a JavaScript nyelven írt felhasználói függvény (UDF) meghívását. 

Biztosak vagyunk abban, hogy ezeket a képességeket kulcs tooreducing hello súrlódás hello alkalmazás- és hello adatbázis közötti és fontosságúak a fejlesztést tesz lehetővé.

Azt javasoljuk, első lépések a következő videót, amely Aravind Ramachandran mutatja Cosmos DB lekérdező képességek, hello figyeli, és látogasson el a [Tesztlekérdezéseket](http://www.documentdb.com/sql/demo), ahol Cosmos DB kipróbálásához és SQL lekérdezések futtatása az adathalmaz.

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/DataExposedQueryingDocumentDB/player]
> 
> 

Ezt követően térjen vissza toothis cikk, ha először egy SQL-lekérdezés oktatóanyag, amely bemutatja, hogyan néhány egyszerű JSON-dokumentumokat és az SQL-parancsokat.

## <a id="GettingStarted"></a>Ismerkedés az SQL-parancsokat az Cosmos-Adatbázisba
toosee Cosmos DB SQL: működnek, most kezdődnie néhány egyszerű JSON-dokumentumok és bízná néhány egyszerű lekérdezéseket. Fontolja meg e két JSON-dokumentumok két családok kapcsolatban. A Cosmos DB azt nem kell toocreate bármely sémák vagy másodlagos indexek explicit módon. A Microsoft egyszerűen kell tooinsert hello JSON-dokumentumok tooa Cosmos DB gyűjteményben, és ezt követően lekérdezése. Itt egy egyszerű JSON tudunk dokumentum hello Andersen családhoz, hello szülők, gyermekek (és azok kedvtelésből), címe és regisztrációs adatait. hello dokumentumnak karakterláncok, számok, a logikai, tömbök és beágyazott tulajdonságait. 

**A dokumentum**  

```JSON
{
  "id": "AndersenFamily",
  "lastName": "Andersen",
  "parents": [
     { "firstName": "Thomas" },
     { "firstName": "Mary Kay"}
  ],
  "children": [
     {
         "firstName": "Henriette Thaulow", 
         "gender": "female", 
         "grade": 5,
         "pets": [{ "givenName": "Fluffy" }]
     }
  ],
  "address": { "state": "WA", "county": "King", "city": "seattle" },
  "creationDate": 1431620472,
  "isRegistered": true
}
```

Ez az egyetlen különbség – a második dokumentum `givenName` és `familyName` helyett használt `firstName` és `lastName`.

**A dokumentum**  

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```

Most próbáljon néhány lekérdezést az adatok toounderstand elleni néhány hello kulcs DocumentDB API SQL aspektusait. Például a hello alábbi lekérdezés a beolvasása hello dokumentumok Ha hello id mezője megegyezik `AndersenFamily`. Mivel ez egy `SELECT *`, hello hello lekérdezés eredménye hello teljes JSON-dokumentum:

**Lekérdezés**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Eredmények**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]


Most pedig nézzük meg hello esetben, ha igazolnia kell tooreformat hello egy másik alakzat JSON-kimenetét. Ez a lekérdezés egy új JSON projektek objektum két kijelölt mezővel, nevét és a város, ha hello cím város azonos nevet hello hello állapot. Ebben az esetben a "NY, NY" megegyezik.

**Lekérdezés**    

    SELECT {"Name":f.id, "City":f.address.city} AS Family 
    FROM Families f 
    WHERE f.address.city = f.address.state

**Eredmények**

    [{
        "Family": {
            "Name": "WakefieldFamily", 
            "City": "NY"
        }
    }]


hello tovább lekérdezés visszaadja az összes hello nevet kapnak gyermekek hello termékcsalád, amelynek azonosítója megegyezik `WakefieldFamily` hello város tartózkodási helye alapján rendezve.

**Lekérdezés**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC

**Eredmények**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


Szeretnénk toodraw figyelmet tooa hello Cosmos DB néhány fontos aspektusainak lekérdezési nyelv eddig is láttuk hello példákból:  

* Mivel a DocumentDB API SQL JSON értékek működéséről, alakú sorok és oszlopok helyett entitások fa foglalkozik. Ezért hello nyelv lehetővé teszi, hogy tekintse meg a hello fa bármilyen tetszőleges mélységben toonodes például `Node1.Node2.Node3…..Nodem`, hasonló toorelational SQL hivatkozó toohello két rész hivatkozását `<table>.<column>`.   
* hello strukturált lekérdezési nyelv works séma nélküli adatokkal. Ezért hello dinamikusan típus rendszer igényeinek toobe kötött. hello ugyanabban a kifejezésben sikerült yield különböző típusaihoz különböző dokumentumokat. a lekérdezések eredménye hello egy érvényes JSON-érték, de nem garantált toobe rögzített séma.  
* Cosmos DB csak szigorú JSON-dokumentumokat támogat. Ez azt jelenti, hello típusrendszernek és kifejezések korlátozott toodeal csak a JSON típusával. Tekintse meg a toohello [JSON-specifikáció](http://www.json.org/) további részleteket.  
* A Cosmos DB gyűjtemény egy olyan sémamentes tároló JSON-dokumentumot. tartalmazási, és nem a primary key és idegen kulcs kapcsolatokat a rendszer implicit módon rögzíti hello kapcsolatokat az entitások belül, és egy gyűjtemény dokumentumok között. Ez egy fontos eleme érdemes alapján hello intra-dokumentum illesztések ebben a cikkben korábban tárgyalt mutat.

## <a id="Indexing"></a>A cosmos DB indexelő
Hello DocumentDB API SQL-szintaxis feltölti azt, mielőtt Cosmos DB tervéhez indexelő hello felfedezése érdemes. 

hello adatbázisa indexei célja a különböző űrlapok és minimális erőforrás-használat (például CPU és a bemeneti/kimeneti) tartalmazó alakzatok tooserve lekérdezések ugyanakkor biztosítható a jó teljesítmény és kis késleltetése. Hello megfelelő index lekérdezése az adatbázis hello választott gyakran, sok tervezést és kísérletezés igényel. Ezt a módszert használja az adatbázisok séma nélküli, ahol hello adatok nem felelnek meg a tooa szigorú séma, és gyorsan fejlődésének kihívást jelent. 

Ezért hello Cosmos DB indexelési alrendszer tervezésekor be van állítva a következő célok hello:

* Indexeljük a dokumentumokat anélkül, hogy a séma: hello alrendszer indexelő bármely sémaadatok kér és nem bármely séma feltételezéseket hello dokumentumok ellenőrizze. 
* Hatékony és gazdag hierarchikus és relációs lekérdezések támogatása: hello index támogatja hello Cosmos DB lekérdezési nyelv hatékonyan, beleértve a hierarchikus és relációs leképezések támogatását.
* Egységes lekérdezések in face of írások tartós kötet támogatása: nagy írási átviteli munkaterhelések egységes lekérdezések esetén hello index frissítése Növekményesen, hatékony és online az írások tartós kötet hello felületét. hello konzisztens index frissítése kritikus fontosságú tooserve hello lekérdezések mely hello beállított felhasználó hello dokumentum szolgáltatásban hello konzisztencia szinten.
* Több vállalat kiszolgálása támogatása: hello foglalásalapú modell megadott erőforrás irányításhoz bérlők között, index frissítései hello költségvetési rendszererőforrást (Processzor, memória és i/o műveletek száma másodpercenként) replika felosztott belül. 
* Tároló-hatékonyságot biztosít: A költséghatékonyság, hello lemezen tárhelyre terhet hello index a kötött és előre jelezhető. Ez különösen fontos, mivel Cosmos DB hello fejlesztői toomake költség-alapú mellékhatásokkal közötti kapcsolat toohello a lekérdezések teljesítményét index terhelést.  

Tekintse meg a toohello [Azure Cosmos DB samples](https://github.com/Azure/azure-documentdb-net) MSDN minták jelenít meg, hogyan tooconfigure hello indexelési házirendet egy gyűjtemény. Most folytassuk hello részleteinek hello Azure Cosmos adatbázis SQL-szintaxis.

## <a id="Basics"></a>Az Azure Cosmos adatbázis SQL-lekérdezést alapjai
Minden egyes lekérdezés SELECT záradékában és választható FROM áll és a WHERE záradék / ANSI SQL szabványoknak. Általában minden lekérdezéshez hello forrás hello FROM záradékban számbavétele megtörtént. Hello szűréssel hello WHERE záradék hello forrás tooretrieve érvényesíti a JSON-dokumentumok egy részét. Végezetül hello SELECT záradékban használt tooproject hello kért hello értékek JSON válassza ki a listát.

    SELECT <select_list> 
    [FROM <from_specification>] 
    [WHERE <filter_condition>]
    [ORDER BY <sort_specification]    


## <a id="FromClause"></a>FROM záradékban
Hello `FROM <from_specification>` záradék használata nem kötelező, kivéve, ha hello forrás szűrt vagy tervezett később hello lekérdezésben. hello ehhez a záradékhoz célja toospecify hello adatforrás esetén mely hello lekérdezés működnie kell. Hello egész gyűjteményre gyakran hello forrás, de ehelyett egy adhat meg a hello gyűjtemény egy részét. 

A lekérdezés, például `SELECT * FROM Families` azt jelzi, hogy hello teljes családok gyűjteményt hello forrás mely tooenumerate keresztül. Egy különös azonosító legfelső szintű lehet használt toorepresent hello gyűjtemény hello gyűjteménynév használata helyett. hello alábbi lista a kényszerített lekérdezésenként hello szabályok.

* hello gyűjtemény akkor jelölhető meg aliasként, például a `SELECT f.id FROM Families AS f` vagy egyszerűen `SELECT f.id FROM Families f`. Itt `f` hello megfelelője az `Families`. `AS`nem kötelező kulcsszó tooalias hello azonosító, amely.
* Egyszer aliasnevet hello eredeti adatforrás nem köthető. Például `SELECT Families.id FROM Families f` szintaktikailag hibás, mert hello azonosítója "Családok" többé nem lehet feloldani.
* Lehet, hogy a hivatkozott toobe igénylő összes tulajdonság teljesen minősített. A szigorú séma való hello hiányában ez a kényszerített tooavoid egyetlen nem egyértelmű kötést. Ezért `SELECT id FROM Families f` óta hello tulajdonság szintaktikailag nem `id` nincs kötve.

### <a name="subdocuments"></a>Aldokumentumok
hello forrás csökkentett tooa kisebb részhalmazát is lehet. Például tooenumerating minden a dokumentumban csak egy részfája, hello subroot majd válhat hello forrás, ahogy az alábbi példa hello:

**Lekérdezés**

    SELECT * 
    FROM Families.children

**Eredmények**  

    [
      [
        {
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [
              {
                  "givenName": "Fluffy"
              }
            ]
        }
      ],
      [
        {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        },
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]

Amíg a fenti példa hello tömb hello forrásaként, az objektum is felhasználhatók hello forrásaként, amely a következő példa hello látható: hello eredménye, hogy minden érvényes JSON-érték (nem nincs definiálva), amely itt található: hello forrás tekinthető hello lekérdezés. Ha egyes termékcsaládok nem rendelkezik egy `address.state` érték, a lekérdezés eredményében hello ki vannak zárva.

**Lekérdezés**

    SELECT * 
    FROM Families.address.state

**Eredmények**

    [
      "WA", 
      "NY"
    ]


## <a id="WhereClause"></a>A WHERE záradék
a WHERE záradék hello (**`WHERE <filter_condition>`**) megadása nem kötelező. Meghatározza, hogy hello eredmény részét képező rendelés toobe meg kell felelnie a hello JSON-dokumentumok hello forrás által biztosított hello feltételeket. Ki kell értékelnie minden bármely JSON-dokumentum hello megadott feltételek túl "true" toobe hello eredmény figyelembe venni. hello záradék helyének hello index réteg rendelés toodetermine hello abszolút legkisebb részhalmazban forrás azt jelzi, hogy hello eredmény része lehet. 

hello következő lekérdezés kéri a name tulajdonság, amelynek értéke tartalmazó dokumentumok `AndersenFamily`. Bármely más dokumentum, amely nem rendelkezik name tulajdonsággal, vagy ha hello értéke nem egyezik `AndersenFamily` ki van zárva. 

**Lekérdezés**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Eredmények**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


hello előző példából kiderült, egy egyszerű egyenlőség lekérdezést. A DocumentDB API SQL számos skaláris kifejezést. leggyakrabban használt hello bináris és egyoperandusú kifejezés. Hello forrás JSON-objektumból tulajdonsághivatkozást egyaránt érvényes kifejezések. 

a következő bináris operátor hello jelenleg támogatott, és a lekérdezések, ahogy az alábbi példák hello használható:  

<table>
<tr>
<td>Aritmetikai</td>    
<td>+,-,*,/,%</td>
</tr>
<tr>
<td>Bitenként</td>    
<td>|}, &, ^, <<>>,, >>> (nulla-Kitöltés jobbra tolást)</td>
</tr>
<tr>
<td>Logikai</td>
<td>ÉS, VAGY SEM</td>
</tr>
<tr>
<td>Összehasonlítása</td>    
<td>=, !=, &lt;, &gt;, &lt;=, &gt;=, <></td>
</tr>
<tr>
<td>Karakterlánc</td>    
<td>|| (összefűzésére)</td>
</tr>
</table>  


Vessen egy pillantást néhány lekérdezést a bináris operátorok használatával.

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5     -- matching grades == 5


Az egyoperandusú operátorral hello +,-, ~, és nem is támogatottak, és is használhatók lekérdezéseken belül, ahogy az alábbi példa hello:

    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1

    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5



Ezenkívül toobinary és az egyoperandusú operátorokat tulajdonság vonatkozó hivatkozások használata is engedélyezett. Például `SELECT * FROM Families f WHERE f.isRegistered` értéket ad vissza hello hello tulajdonságot tartalmazó JSON-dokumentum `isRegistered` ahol a hello tulajdonság érték egyenlő toohello JSON `true` érték. Egyéb értékek (false, null, nem definiált, `<number>`, `<string>`, `<object>`, `<array>`stb) részletes útmutatást az hello eredményből kivételével toohello forrásdokumentum. 

### <a name="equality-and-comparison-operators"></a>Egyenlőség és összehasonlító operátorok
hello következő táblázatban egyenlőségi összehasonlítás eredménye hello DocumentDB API SQL bármely két JSON-típusok között.

<table style = "width:300px">
   <tbody>
      <tr>
         <td valign="top">
            <strong>Op</strong>
         </td>
         <td valign="top">
            <strong>Nincs definiálva</strong>
         </td>
         <td valign="top">
            <strong>NULL értékű</strong>
         </td>
         <td valign="top">
            <strong>Logikai érték</strong>
         </td>
         <td valign="top">
            <strong>Szám</strong>
         </td>
         <td valign="top">
            <strong>Karakterlánc</strong>
         </td>
         <td valign="top">
            <strong>Objektum</strong>
         </td>
         <td valign="top">
            <strong>A tömb</strong>
         </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Nincs definiálva<strong>
         </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>NULL értékű<strong>
         </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
            <strong>OKÉ</strong>
         </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Logikai érték<strong>
         </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
            <strong>OKÉ</strong>
         </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Szám<strong>
         </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
            <strong>OKÉ</strong>
         </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Karakterlánc<strong>
         </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
            <strong>OKÉ</strong>
         </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Objektum<strong>
         </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
            <strong>OKÉ</strong>
         </td>
         <td valign="top">
Nincs definiálva </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>A tömb<strong>
         </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
Nincs definiálva </td>
         <td valign="top">
            <strong>OKÉ</strong>
         </td>
      </tr>
   </tbody>
</table>

Más összehasonlító operátorok többek között a >, > =,! =, < és < =, hello következő szabályok lépnek érvénybe:   

* Összehasonlítás típusok között meghatározatlan eredményez.
* Két objektum vagy két összehasonlítása tömbállandó meghatározatlan eredményez.   

Ha hello hello szűrő hello skaláris kifejezés eredménye nem definiált, hello megfelelő dokumentum nem szerepel hello eredményt, mert meghatározatlan logikailag túl "igaz" nem egyenlő.

### <a name="between-keyword"></a>Kulcsszó között
Hello BETWEEN kulcsszó tooexpress lekérdezések értékek, például a tartományok ANSI SQL is használhatja. KÖZÖTTI használható karakterlánc vagy szám ellen.

Például a lekérdezés által visszaadott összes családba tartozó dokumentumok, mely hello az első gyermek osztályú (mind a két szélsőértéket beleértve) 1-5 között van. 

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5

Eltérően az ANSI-SQL-ben is használhatja hello BETWEEN záradék hello FROM záradékában például a következő példa hello.

    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c

A lekérdezés végrehajtása gyorsabb ne felejtse el az indexelési házirendet elleni bármely numerikus tulajdonságok/elérési utakat hello BETWEEN záradék a szűrt index Tartománytípus használó toocreate. 

hello fő különbség a DocumentDB API és az ANSI SQL BETWEEN használatával, hogy akkor is express vegyes típusú tulajdonságokhoz lekérdezések – például lehetséges, hogy "osztály" [5] szám lehet bizonyos dokumentumok és mások számára ("grade4") tartalmazó karakterlánc. Ezekben az esetekben például a JavaScript, a "nem definiált" két különböző típusú eredményt, és hello dokumentum összehasonlítása a rendszer kihagyja.

### <a name="logical-and-or-and-not-operators"></a>Logikai (AND, OR, és nem) operátorok
Logikai operátorok működhet a logikai értékek. a következő táblák hello ezen operátorok hello logikai igazság táblák láthatók.

| VAGY | True (Igaz) | False (Hamis) | Nincs definiálva |
| --- | --- | --- | --- |
| True (Igaz) |True (Igaz) |True (Igaz) |True (Igaz) |
| False (Hamis) |True (Igaz) |False (Hamis) |Nincs definiálva |
| Nincs definiálva |True (Igaz) |Nincs definiálva |Nincs definiálva |

| ÉS | True (Igaz) | False (Hamis) | Nincs definiálva |
| --- | --- | --- | --- |
| True (Igaz) |True (Igaz) |False (Hamis) |Nincs definiálva |
| False (Hamis) |False (Hamis) |False (Hamis) |False (Hamis) |
| Nincs definiálva |Nincs definiálva |False (Hamis) |Nincs definiálva |

| NEM |  |
| --- | --- |
| True (Igaz) |False (Hamis) |
| False (Hamis) |True (Igaz) |
| Nincs definiálva |Nincs definiálva |

### <a name="in-keyword"></a>A kulcsszó
hello IN kulcsszó használt toocheck lehet, hogy a megadott érték megegyezik-e a listában. Például a lekérdezés által visszaadott összes családba tartozó dokumentumok ahol hello azonosító: "WakefieldFamily" vagy "AndersenFamily". 

    SELECT *
    FROM Families 
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')

Ez a példa visszaadja az összes dokumentumot ahol hello állapota bármelyik hello megadott értékeket.

    SELECT *
    FROM Families 
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")

### <a name="ternary--and-coalesce--operators"></a>Ternáris (?) és a Coalesce (?) operátorok
hello Ternáris és Coalesce operátorok használt toobuild feltételes kifejezéseket, például a C# és JavaScript programnyelvek hasonló toopopular lehet. 

hello Ternáris (?) operátor akkor lehet hasznos, ha hello összeállításával új JSON tulajdonságok keresnie. Például most írhat lekérdezések tooclassify hello osztály szintek emberi olvasható űrlapot például kezdő/köztes/speciális alább látható módon.

     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel 
     FROM Families.children[0] c

Is ágyazhatja hello hívások toohello operátor például az alábbi hello lekérdezésben.

    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high")  AS gradeLevel 
    FROM Families.children[0] c

Más lekérdezési operátorok, ha hello hello feltételes kifejezésben hivatkozott tulajdonságai hiányoznak minden a dokumentumban, vagy ha összehasonlított hello típusok eltérőek, majd ezeket a dokumentumokat nem tartoznak a hello lekérdezés eredményei között.

hello Coalesce (?) operátor lehet (más néven tulajdonság hello jelenléte használt tooefficiently ellenőrzése van meghatározva) dokumentumban. Ez akkor hasznos, ha félig strukturált lekérdezését vagy vegyes típusú adatokat. Például, ez a lekérdezés visszaadja hello "Vezetéknév" Ha jelen van, vagy hello "Vezetéknév" Ha nem, akkor a jelen.

    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f

### <a id="EscapingReservedKeywords"></a>Idézőjelek közé zárt tulajdonságelérő
Emellett hello idézőjelek közé zárt tulajdonság operátorral tulajdonságok `[]`. Például `SELECT c.grade` és `SELECT c["grade"]` egyenértékű. Ez a szintaxis akkor hasznos, ha egy tulajdonság szóközöket, különleges karaktereket tartalmaz, vagy azonos nevet SQL kulcsszó vagy fenntartott szó tooshare hello történik tooescape van szüksége.

    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"


## <a id="SelectClause"></a>SELECT záradékban
hello SELECT záradékban (**`SELECT <select_list>`**) megadása kötelező, és határozza meg, milyen értékeket lekért hello lekérdezés fentiekhez hasonló ANSI-SQL-ben. hello részhalmazán, amely fölött hello forrás dokumentumok van szűrve átadott alakzatot hello leképezése fázisban, amelyben hello megadott JSON-értékek olvassa és egy új JSON-objektum összeállított, minden egyes azt az alakzatot átadott bemenet. 

a következő példa hello tipikus választó jeleníti meg. 

**Lekérdezés**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Eredmények**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


### <a name="nested-properties"></a>Beágyazott tulajdonságai
A következő példa hello, a két beágyazott tulajdonságok kiálló azt `f.address.state` és `f.address.city`.

**Lekérdezés**

    SELECT f.address.state, f.address.city
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Eredmények**

    [{
      "state": "WA", 
      "city": "seattle"
    }]


Leképezési JSON kifejezések is támogatja, ahogy az alábbi példa hello:

**Lekérdezés**

    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Eredmények**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle", 
        "name": "AndersenFamily"
      }
    }]


Nézzük hello szerepe `$1` itt. Hello `SELECT` záradékban kell toocreate egy JSON-objektum, és nem kulcsra azért van, mert implicit argumentum változónevek kezdve használjuk `$1`. Például a lekérdezés által visszaadott implicit argumentum két változót, címkével `$1` és `$2`.

**Lekérdezés**

    SELECT { "state": f.address.state, "city": f.address.city }, 
           { "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Eredmények**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]


### <a name="aliasing"></a>Aliasképző
Most tegyük kiterjesztése hello fenti példában az explicit aliasképző értékek. Ez hello kulcsszó használt aliassal való ellátását. Nem kötelező kiálló hello második értékével megegyező közben látható `NameInfo`. 

Abban az esetben, ha a lekérdezés tartozik hello két tulajdonság azonos nevű aliassal való ellátását egyik vagy mindkét hello tulajdonságait, hogy azok rendszer használatát a tervezett hello használt toorename kell lennie eredménye.

**Lekérdezés**

    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo, 
           { "name": f.id } NameInfo
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Eredmények**

    [{
      "AddressInfo": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]


### <a name="scalar-expressions"></a>Skaláris kifejezések
Ezenkívül tooproperty hivatkozik, a SELECT záradékban hello is támogatja a skaláris kifejezések állandók, aritmetikai kifejezésekben, logikai kifejezéseket és stb. Például ez egy egyszerű "Hello, World" lekérdezést.

**Lekérdezés**

    SELECT "Hello World"

**Eredmények**

    [{
      "$1": "Hello World"
    }]


Ez egy összetett példa, amely a skaláris kifejezést használ.

**Lekérdezés**

    SELECT ((2 + 11 % 7)-2)/3    

**Eredmények**

    [{
      "$1": 1.33333
    }]


A következő példa hello hello hello skaláris kifejezés eredménye egy logikai érték.

**Lekérdezés**

    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f    

**Eredmények**

    [
      {
        "AreFromSameCityState": false
      }, 
      {
        "AreFromSameCityState": true
      }
    ]


### <a name="object-and-array-creation"></a>Az objektum és tömb létrehozása
A DocumentDB API SQL egy másik alapfunkciója tömb vagy objektum-létrehozás. Az előző példában hello vegye figyelembe, hogy létrehoztunk egy új JSON-objektum. Ehhez hasonlóan egy is végezhet tömbök látható hello példák a következő módon:

**Lekérdezés**

    SELECT [f.address.city, f.address.state] AS CityState 
    FROM Families f    

**Eredmények**  

    [
      {
        "CityState": [
          "seattle", 
          "WA"
        ]
      }, 
      {
        "CityState": [
          "NY", 
          "NY"
        ]
      }
    ]

### <a id="ValueKeyword"></a>ÉRTÉK kulcsszó
Hello **érték** kulcsszó tartalmaz egy módja tooreturn JSON-érték. Például a lent látható módon adja vissza hello skaláris hello lekérdezés `"Hello World"` helyett `{$1: "Hello World"}`.

**Lekérdezés**

    SELECT VALUE "Hello World"

**Eredmények**

    [
      "Hello World"
    ]


hello következő lekérdezés visszaadja hello JSON-érték nélkül hello `"address"` hello eredmények címkéje.

**Lekérdezés**

    SELECT VALUE f.address
    FROM Families f    

**Eredmények**  

    [
      {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }, 
      {
        "state": "NY", 
        "county": "Manhattan", 
        "city": "NY"
      }
    ]

hello alábbi példa bővíti a tooshow hogyan tooreturn JSON primitív értékek (hello levélszintű hello JSON-fa). 

**Lekérdezés**

    SELECT VALUE f.address.state
    FROM Families f    

**Eredmények**

    [
      "WA",
      "NY"
    ]


### <a name="-operator"></a>* Operátor
hello különleges operátort (*) támogatott tooproject hello dokumentumot-van. Használatakor a hello csak tervezett mező kell legyen. Például a lekérdezés során `SELECT * FROM Families f` érvényes, `SELECT VALUE * FROM Families f ` és `SELECT *, f.id FROM Families f ` érvénytelen.

**Lekérdezés**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Eredmények**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

### <a id="TopKeyword"></a>TOP operátor
hello felső kulcsszó lehet használt toolimit hello számú értéket a lekérdezésből. FELSŐ együtt hello ORDER BY záradék használata esetén hello eredménykészlet-e a korlátozott toohello első N rendezett értékek száma; Ellenkező esetben az eredmény hello első N eredmények száma nem definiált sorrendje. Ajánlott eljárásként a SELECT utasítással, mindig használja az ORDER BY záradék hello TOP záradék. Ez az hello csak úgy toopredictably jelző felső által érintett sorok. 

**Lekérdezés**

    SELECT TOP 1 * 
    FROM Families f 

**Eredmények**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

FELSŐ egy állandó értékkel (ahogy fent), vagy a paraméteres lekérdezés változó érték használható. További részletekért lásd a paraméteres lekérdezés az alábbi.

### <a id="Aggregates"></a>Aggregátumfüggvények
Összesítéseket hajtsa végre a hello `SELECT` záradékban. Az aggregátumfüggvények egy értékhalmazt a számítás elvégzése, és egyetlen érték visszaadása. Például hello következő lekérdezés visszaadja hello száma családba tartozó dokumentumok hello gyűjteményen belül.

**Lekérdezés**

    SELECT COUNT(1) 
    FROM Families f 

**Eredmények**

    [{
        "$1": 2
    }]

Is visszatérhet hello skaláris értékét hello összesített hello segítségével `VALUE` kulcsszó. Például hello következő lekérdezés visszaadja hello száma értékek egyetlen számként:

**Lekérdezés**

    SELECT VALUE COUNT(1) 
    FROM Families f 

**Eredmények**

    [ 2 ]

A szűrők együtt is elvégezheti összesíti. Például hello következő lekérdezés adja vissza hello címmel dokumentumok száma hello Washington állam hello.

**Lekérdezés**

    SELECT VALUE COUNT(1) 
    FROM Families f
    WHERE f.address.state = "WA" 

**Eredmények**

    [ 1 ]

hello következő táblázatban hello támogatott összesítő függvények listáját a DocumentDB az API-ban. `SUM`és `AVG` numerikus érték, keresztül hajtja végre, mivel `COUNT`, `MIN`, és `MAX` karakterláncok, a logikai és nullák keresztül hajtható végre. 

| Használat | Leírás |
|-------|-------------|
| SZÁMA | Beolvasása hello hello kifejezésben szereplő elemek száma. |
| SUM   | Beolvasása hello hello kifejezés összes hello értékének összege. |
| PERC   | Beolvasása hello hello kifejezés minimális értéket. |
| MAXIMÁLIS SZÁMA   | Beolvasása hello hello kifejezésben maximális értéket. |
| ÁTLAGOS   | Beolvasása hello hello kifejezésben hello értékek átlaga. |

Összesíti egy tömb iteráció hello eredményeit keresztül is elvégezhető. További információkért lásd: [tömb iterációs lekérdezésekben](#Iteration).

> [!NOTE]
> Ha az Azure portál Query Explorer használatával hello, vegye figyelembe, hogy összesítési lekérdezések adhat vissza hello részben összesített eredmények a lekérdezés lap. SDK-k hello egyetlen értéket összesítő összes oldalán hoz létre. 
> 
> A sorrendben tooperform összesítési a lekérdezések kód használatával kell .NET SDK 1.12.0, a .NET Core SDK 1.1.0-ás vagy a Java SDK 1.9.5 vagy újabb.    
>

## <a id="OrderByClause"></a>ORDER BY záradék
Például az ANSI-SQL-ben megadhat egy választható Order By záradék lekérdezése során. hello záradékot tartalmazhat egy választható ASC vagy DESC argumentum toospecify hello sorrendet, amelyben eredményeket kell beolvasni.

Például ez kártevőcsaládok hello rezidens város neve sorrendjét, amely.

**Lekérdezés**

    SELECT f.id, f.address.city
    FROM Families f 
    ORDER BY f.address.city

**Eredmények**

    [
      {
        "id": "WakefieldFamily",
        "city": "NY"
      },
      {
        "id": "AndersenFamily",
        "city": "Seattle"    
      }
    ]

És családok sorrendben létrehozásának dátuma, amely egy szám, amely hello epoch idő, azaz, eltelt idő óta 1970 jan. 1 másodperc van tárolva, amely itt található.

**Lekérdezés**

    SELECT f.id, f.creationDate
    FROM Families f 
    ORDER BY f.creationDate DESC

**Eredmények**

    [
      {
        "id": "WakefieldFamily",
        "creationDate": 1431620462
      },
      {
        "id": "AndersenFamily",
        "creationDate": 1431620472    
      }
    ]

## <a id="Advanced"></a>Speciális adatbázis fogalmait és az SQL-lekérdezések

### <a id="Iteration"></a>Ismétlés
Egy új szerkezet hello művelettel lett hozzáadva **IN** DocumentDB API SQL tooprovide támogatása a JSON-tömbök keresztül léptetés kulcsszót. hello FROM forrás iterációs támogatja. Kezdjük a következő példa hello:

**Lekérdezés**

    SELECT * 
    FROM Families.children

**Eredmények**  

    [
      [
        {
          "firstName": "Henriette Thaulow", 
          "gender": "female", 
          "grade": 5, 
          "pets": [{ "givenName": "Fluffy"}]
        }
      ], 
      [
        {
            "familyName": "Merriam", 
            "givenName": "Jesse", 
            "gender": "female", 
            "grade": 1
        }, 
        {
            "familyName": "Miller", 
            "givenName": "Lisa", 
            "gender": "female", 
            "grade": 8
        }
      ]
    ]

Most már egy másik lekérdezés keresztül hello gyűjtemény gyermekek iterációs végző vizsgáljuk meg. Vegye figyelembe a hello kimeneti tömbben hello különbség. Ez a példa felosztja a `children` és hello eredmények simítja egyetlen tömbbe.  

**Lekérdezés**

    SELECT * 
    FROM c IN Families.children

**Eredmények**  

    [
      {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy" }]
      },
      {
          "familyName": "Merriam",
          "givenName": "Jesse",
          "gender": "female",
          "grade": 1
      },
      {
          "familyName": "Miller",
          "givenName": "Lisa",
          "gender": "female",
          "grade": 8
      }
    ]

Ez további meg minden egyes belépési hello tömb, ahogy az alábbi példa hello használt toofilter lehet:

**Lekérdezés**

    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8

**Eredmények**  

    [{
      "givenName": "Lisa"
    }]

Összesítési tömb iterációs hello eredményét keresztül is elvégezheti. Például hello következő lekérdezés számolja hello gyermekek összes családok között.

**Lekérdezés**

    SELECT COUNT(child) 
    FROM child IN Families.children

**Eredmények**  

    [
      { 
        "$1": 3
      }
    ]

### <a id="Joins"></a>Illesztése
Hello kell toojoin táblák között egy relációs adatbázisban, fontos. Az hello normalizált logikai corollary toodesigning sémák. Ellenkező toothis, a DocumentDB API hello nem normalizált adatok modell sémamentes dokumentumok foglalkozik. Ez az hello logikai megfelelője a "önillesztés".

hello hello nyelvi támogató szintaxisa < from_source1 > Csatlakozás < from_source2 > ILLESZTÉSI... CSATLAKOZTASSA az < from_sourceN >. A teljes, ezt adja vissza, amely **N**- rekordokat (a rekord **N** értékek). A táblakonstruktor minden rekordjának összes gyűjtemény alias léptetés alatt az megfelelő készletek által visszaadott érték tartozik. Más szóval ez az egy teljes hello illesztési részt hello-készlet keresztszorzatát.

hello következő példák azt szemléltetik, hogyan működik a hello JOIN záradékban. A következő példa hello hello eredménye üres mivel hello forrás- és üres minden dokumentumát keresztszorzatát üres.

**Lekérdezés**

    SELECT f.id
    FROM Families f
    JOIN f.NonExistent

**Eredmények**  

    [{
    }]


A következő példa hello, hello illesztési hello dokumentumgyökér és hello közé esik `children` subroot. Egy eltérő termék két JSON-objektumok között. hello arra, hogy gyermeke tömb nincs hello ILLESZTÉSI hatékonyan, mivel azt a egyetlen legfelső szintű hello gyermekek tömb nem foglalkoznak. Ezért hello eredmény tartalmazza csak két eredményt, mivel hello hello tömbbel rendelkező minden egyes dokumentum keresztszorzatát adja eredményül pontosan csak egy dokumentum.

**Lekérdezés**

    SELECT f.id
    FROM Families f
    JOIN f.children

**Eredmények**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]


a következő példa hello több hagyományos illesztés jeleníti meg:

**Lekérdezés**

    SELECT f.id
    FROM Families f
    JOIN c IN f.children 

**Eredmények**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]



hello először thing toonote, hogy hello `from_source` a hello **csatlakozás** záradék egy iterátor. Igen hello folyamata ebben az esetben a következőképpen történik:  

* Bontsa ki az egyes gyermekelem **c** hello tömbben.
* Hello a dokumentum gyökerének hello határokon termék alkalmazása **f** minden gyermekelemmel rendelkező **c** , amely lett egybesimított hello első lépésben.
* Végezetül projekt hello gyökérszintű objektum **f** névtulajdonság önmagában. 

hello első dokumentum (`AndersenFamily`) csak egy gyermekelemet tartalmaz, ezért a hello eredménykészlet csak egyetlen objektumhoz megfelelő toothis dokumentumot tartalmaz. hello második dokumentum (`WakefieldFamily`) két gyermekeket tartalmaz. Igen hello határokon termék hoz létre egy külön objektum minden gyermek, ezáltal két objektum, egy gyermek megfelelő toothis dokumentumok eredményez. mindkét ezeket a dokumentumokat a mezők kitöltése hello legfelső szintű hello ugyanaz, mint egy határokon termékben teheti meg.

hello valós segédprogram a hello ILLESZTÉS tooform rekordokat hello kereszt-terméket, amely nem egy alakzat nehéz tooproject. Továbbá, a hello az alábbi példában látható, szűrést, a rekordot hello kombinációja lehetővé tévő hello felhasználó teljes feltételfüggvényt hello rekordokat feltételt választotta.

**Lekérdezés**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets

**Eredmények**

    [
      {
        "familyName": "AndersenFamily", 
        "childFirstName": "Henriette Thaulow", 
        "petName": "Fluffy"
      }, 
      {
        "familyName": "WakefieldFamily", 
        "childGivenName": "Jesse", 
        "petName": "Goofy"
      }, 
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]



Ebben a példában az előző példa hello természetes bővítménye, és végrehajtja a dupla való csatlakozást. Igen hello határokon termék tekintheti meg a következő látszólagosan kód hello:

    for-each(Family f in Families)
    {    
        for-each(Child c in f.children)
        {
            for-each(Pet p in c.pets)
            {
                return (Tuple(f.id AS familyName, 
                  c.givenName AS childGivenName, 
                  c.firstName AS childFirstName,
                  p.givenName AS petName));
            }
        }
    }

`AndersenFamily`egy gyermek, aki rendelkezik egy háziállat rendelkezik. Igen, hello határokon termék eredményez több olyan sort (1\*1\*1) a család. WakefieldFamily, azonban a két gyermekelemek tartoznak, de csak egy "Jesse" gyermeket kedvtelésből. Jesse két kedvtelésből, ha rendelkezik. Ezért hello határokon termék eredményez 1\*1\*2 = 2 család a sort.

Hello a következő példában, nincs egy kiegészítő szűrőt `pet`. Ez nem tartalmazza az összes, ahol hello háziállatának neve nincs "Árnyékmásolat" hello rekordokat. Figyelje meg, hogy azt tudja toobuild rekordokat a tömböket, szűrő bármely hello elemek hello rekord, és projekt hello elemek kombinációja. 

**Lekérdezés**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"

**Eredmények**

    [
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]


## <a id="JavaScriptIntegration"></a>JavaScript-integráció
Azure Cosmos DB programozási modellt biztosít a feldolgozás alatt álló alapú JavaScript-alkalmazáslogika közvetlenül hello gyűjtemények, tárolt eljárások és eseményindítók tekintetében. Ez lehetővé teszi, hogy mindkét:

* Képes toodo nagy teljesítményű tranzakciós CRUD műveletek és egy gyűjtemény alapján hello szoros integrációja a JavaScript futásidejű közvetlenül belül hello adatbázismotor-dokumentumokon végzett lekérdezések. 
* Természetes modellezési folyamatábrán, változó hatókörének, és a hozzárendelés és az adatbázis-tranzakciókhoz a primitívek kivételkezelő integrálását. A JavaScript-integráció Azure Cosmos DB-támogatással kapcsolatos további információkért tekintse meg az toohello JavaScript kiszolgálóoldali programozhatóság dokumentációját.

### <a id="UserDefinedFunctions"></a>Felhasználói függvény (UDF)
Hello típusok már definiálva van ebben a cikkben, valamint a DocumentDB API SQL támogatja az a felhasználó definiált függvény (UDF). Skaláris felhasználó által megadott függvények támogatottak, ahol hello fejlesztők nulla vagy több argumentumot adjon át és vissza egyetlen argumentuma vissza eredményt. Minden egyes argumentum ellenőrzése alatt álló engedélyezett JSON-érték.  

a DocumentDB API SQL-szintaxis hello ki van bővítve toosupport egyéni alkalmazáslogika ezen felhasználó által definiált függvényeket használatával. Felhasználó által megadott függvények regisztrálhatók a DocumentDB API, és ezután lehet hivatkozni az SQL-lekérdezés részeként. Felhasználó által megadott függvények vannak exquisitely hello valójában lekérdezések által meghívott toobe tervezték. Corollary toothis lehetőség, felhasználó által megadott függvények nem rendelkeznek hozzáféréssel toohello környezeti objektumot amely hello más JavaScript típusoknak (tárolt eljárások és eseményindítók) lehet. Lekérdezések csak olvashatóként hajtható végre, mert futtathatják az elsődleges vagy másodlagos replikákon. Felhasználó által megadott függvények, ezért a másodlagos replikákon más JavaScript típusától eltérően tervezett toorun.

Alább példája egy UDF hogyan lehet regisztrálni: hello Cosmos DB adatbázist, kifejezetten egy dokumentumgyűjteményt.

       UserDefinedFunction regexMatchUdf = new UserDefinedFunction
       {
           Id = "REGEX_MATCH",
           Body = @"function (input, pattern) { 
                       return input.match(pattern) !== null;
                   };",
       };

       UserDefinedFunction createdUdf = client.CreateUserDefinedFunctionAsync(
           UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
           regexMatchUdf).Result;  

hello előző példa létrehoz egy UDF, amelynek a neve `REGEX_MATCH`. Elfogadja a JSON két karakterlánc-értékek `input` és `pattern` és hello első egyező hello mintát megadott hello második ellenőrzi a JavaScript string.match() függvény használatával.

A Microsoft most már használhatja a UDF leképezés lekérdezést. Felhasználó által megadott függvények kell minősíteni hello kis-és nagybetűket előtaggal "udf." Amikor meghívhatók lekérdezések. 

> [!NOTE]
> Előzetes too3 17 2015 / /, Cosmos DB támogatott UDF hívások nélkül hello "udf." Válasszon REGEX_MATCH() például előtag. A hívó mintát elavult.  
> 
> 

**Lekérdezés**

    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families

**Eredmények**

    [
      {
        "$1": true
      }, 
      {
        "$1": false
      }
    ]

hello UDF is használható egy szűrő belül hello példa az alábbi, is minősíti hello "udf." előtagja:

**Lekérdezés**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")

**Eredmények**

    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]


Felhasználó által megadott függvények lényegében érvényes skaláris kifejezések, és leképezések és szűrőket is használhat. 

tooexpand hello működik a felhasználó által megadott függvények, vizsgáljuk meg egy másik példa feltételes logikával:

       UserDefinedFunction seaLevelUdf = new UserDefinedFunction()
       {
           Id = "SEALEVEL",
           Body = @"function(city) {
                   switch (city) {
                       case 'seattle':
                           return 520;
                       case 'NY':
                           return 410;
                       case 'Chicago':
                           return 673;
                       default:
                           return -1;
                    }"
            };

            UserDefinedFunction createdUdf = await client.CreateUserDefinedFunctionAsync(
                UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
                seaLevelUdf);


Az alábbiakban látható egy példa, hogy a gyakorlatokban hello UDF.

**Lekérdezés**

    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f    

**Eredmények**

     [
      {
        "city": "seattle", 
        "seaLevel": 520
      }, 
      {
        "city": "NY", 
        "seaLevel": 410
      }
    ]


Mivel az előző példák showcase hello, felhasználó által megadott függvények integrálása hello power JavaScript nyelv hello DocumentDB API SQL tooprovide egy gazdag programozható felület toodo eljárási, feltételes összetettek beépített a JavaScript futásidejű hello segítségével képességek.

A DocumentDB API SQL biztosít hello argumentumok toohello felhasználó által megadott függvények nyilvántartott egyes dokumentumok hello forrás szakaszában hello aktuális (a WHERE záradékban vagy a SELECT záradékban) feldolgozási hello UDF. hello eredmény van beépítve zökkenőmentesen hello teljes végrehajtási folyamatban. Ha hello tulajdonságok hivatkozott tooby hello UDF paraméterek nem találhatók hello JSON-érték hello tekint a paraméter nincs definiálva, és ezért hello UDF meghívása teljesen kimarad. Hasonlóképpen ha hello UDF hello eredménye nem definiált, azt nem szerepel hello eredménye. 

Összefoglalva a felhasználó által megadott függvények kiváló eszközök toodo bonyolult üzleti logikát hello lekérdezés részeként.

### <a name="operator-evaluation"></a>A kiértékelési operátor
Cosmos DB, hello alapján, hogy az egy JSON-adatbázis megrajzolja fekvő JavaScript operátorok és az értékelés szemantikáját. Amíg Cosmos DB megpróbál toopreserve JavaScript szemantikáját JSON támogatási tekintetében, hello művelet kiértékelése százalékkal, bizonyos esetekben.

A DocumentDB API SQL, ellentétben a hagyományos SQL hello típusú értékeket gyakran nem ismert hello értékek adatbázisból lekéréséig. A sorrend tooefficiently hajtsa végre a lekérdezéseket, hello operátorok a többsége a szigorú szemben támasztott követelményeit. 

A DocumentDB API SQL nem hajtható végre implicit konverzió JavaScript eltérően. Például egy lekérdezést, például `SELECT * FROM Person p WHERE p.Age = 21` megegyezik egy kora tulajdonságot, amelynek értéke 21 tartalmazó dokumentumokat. Bármely más, amelynek kora tulajdonsága egyezést mutat a karakterlánc a "21", vagy más valószínűleg végtelen változata dokumentum, például "021", "21,0", "0021", "00021", nem fog egyeztetni stb. Ez ezzel szemben az implicit módon casted toonumbers hello karakterlánc-értékek esetén JavaScript toohello (pl. operátor szerinti szűrése, alapján: ==). Ez a beállítás nem megfelelő DocumentDB API SQL hatékony index számára elengedhetetlen. 

## <a name="parameterized-sql-queries"></a>A paraméteres SQL-lekérdezések
Cosmos DB lekérdezéseket támogat, a megszokott @ notation hello kifejezett paraméterekkel. A paraméteres SQL hatékony kezelése és escape-karaktersorozat felhasználói bevitelt, megakadályozza az SQL-injektálás az adatok véletlen kitettség biztosít. 

Például, hogy a Vezetéknév és címállapot fogad paraméterként, és hajthat végre különböző értékek vezetékneve és a felhasználói bevitel alapján cím állapotát.

    SELECT * 
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState

A kérelem küldheti például a paraméteres JSON lekérdezésként DB tooCosmos alább látható.

    {      
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",     
        "parameters": [          
            {"name": "@lastName", "value": "Wakefield"},         
            {"name": "@addressState", "value": "NY"},           
        ] 
    }

hello argumentum tooTOP állíthat be például a paraméteres lekérdezés alább látható.

    {      
        "query": "SELECT TOP @n * FROM Families",     
        "parameters": [          
            {"name": "@n", "value": 10},         
        ] 
    }

A paraméterértékek lehet bármely érvényes JSON (karakterláncok, számok, a logikai, null, akkor is igaz, tömbök, vagy beágyazott JSON). Is mivel Cosmos DB séma nélküli, paraméterek a rendszer nem érvényesíti bármilyen ellen.

## <a id="BuiltinFunctions"></a>Beépített funkciók
Cosmos DB számos beépített funkciót is támogatja a közös műveleteket, például a felhasználói függvény (UDF) lekérdezéseken belül használható.

| Csoport          | Műveletek                                                                                                                                          |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| Matematikai funkciók  | ABS, felső határ, EXP, EMELET, napló, LOG10, ENERGIAGAZDÁLKODÁSI, CIKLIKUS, bejelentkezési, SQRT, SZÖGLETES, csonk, ARCCOS, ARCSIN, ATAN, ATN2, COS, tűz, fok, PI, radiánban megadott szög, EG és TAN |
| Írja be az ellenőrzési funkciók | IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED és IS_PRIMITIVE                                                           |
| Karakterlánc        | CONCAT, tartalmazza, megadott módon VÉGZŐDŐ, INDEX_OF, balra, hossza, alsó, LTRIM, csere, REPLIKÁLJA, NÉVKERESÉSI, jobbra, RTRIM, megadott módon KEZDŐDŐ, SUBSTRING és felső       |
| A tömb funkciók         | ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH és ARRAY_SLICE                                                                                         |
| Térbeli funkciók       | ST_DISTANCE, ST_WITHIN, ST_INTERSECTS, ST_ISVALID és ST_ISVALIDDETAILED                                                                           | 

Jelenleg használata egy felhasználói függvény (UDF), amelynek beépített függvény már elérhető, használjon hello megfelelő beépített függvény, toobe gyorsabb toorun és egyéb lesz hatékony. 

### <a name="mathematical-functions"></a>Matematikai funkciók
hello matematikai funkciók minden végezhet a számítást, a bemeneti értékek, amelyek argumentumként szolgálnak, és a visszaadandó numerikus érték alapján. Itt található a támogatott beépített matematikai függvények táblázatát.


| Használat | Leírás |
|----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [[ABS (num_expr)](#bk_abs) | Értéket ad vissza hello abszolút (pozitív) hello a megadott numerikus kifejezés. |
| [Felső határ (num_expr)](#bk_ceiling) | Beolvasása hello legkisebb egész szám nagyobb értékre, vagy egyenlő, hello megadott numerikus kifejezés. |
| [EMELET (num_expr)](#bk_floor) | Visszaadja hello legnagyobb egész szám kisebb vagy egyenlő, mint a toohello megadott numerikus kifejezés. |
| [EXP (num_expr)](#bk_exp) | A megadott numerikus kifejezés hello hello hatványát adja vissza. |
| [NAPLÓ (num_expr [, Alap])](#bk_log) | Beolvasása hello természetes alapú logaritmusát hello megadva numerikus kifejezés, vagy hello segítségével hello logaritmus alapja |
| [LOG10 (num_expr)](#bk_log10) | A megadott értéket ad vissza hello 10-es logaritmikus érték hello a numerikus kifejezés. |
| [KEREK (num_expr)](#bk_round) | Egy numerikus érték, kerekített toohello legközelebbi egész értéket ad vissza. |
| [CSONK (num_expr)](#bk_trunc) | Egy numerikus érték, csonkolt toohello legközelebbi egész értéket ad vissza. |
| [SQRT (num_expr)](#bk_sqrt) | Hello négyzetgyökét adja vissza a hello megadott numerikus kifejezés. |
| [NÉGYZETES (num_expr)](#bk_square) | Beolvasása hello négyzetes hello a megadott numerikus kifejezés. |
| [ENERGIAGAZDÁLKODÁSI (num_expr, num_expr)](#bk_power) | A megadott értéket ad vissza hello hatványa hello numerikus kifejezés toohello érték van megadva. |
| [BEJELENTKEZÉSI (num_expr)](#bk_sign) | Beolvasása hello bejelentkezési értéket (-1, 0, 1) hello adott numerikus kifejezés. |
| [ARCCOS (num_expr)](#bk_acos) | Hello szög értéket ad vissza, az radiánban megadott szög, amelynek koszinusza hello megadott numerikus kifejezés; más néven koszinuszát. |
| [ARCSIN (num_expr)](#bk_asin) | Hello szög értéket ad vissza, az radiánban megadott szög, amelynek szinusza hello megadott numerikus kifejezés. Ez rövidítése szinuszát. |
| [ATAN (num_expr)](#bk_atan) | Hello szög értéket ad vissza, az radiánban megadott szög, amelynek tangense a hello megadott numerikus kifejezés. Ezt arkusz is nevezik. |
| [ATN2 (num_expr)](#bk_atn2) | Beolvasása hello szög radiánban közötti hello pozitív x tengely és hello ray pontról hello származási toohello (y, x), ahol x és y értékei hello hello a két megadott lebegőpontos kifejezés. |
| [COS (num_expr)](#bk_cos) | Beolvasása hello trigonometric koszinusza hello radiánban megadott szög, hello a megadott kifejezés. |
| [TŰZ (num_expr)](#bk_cot) | Beolvasása hello trigonometric kotangensét hello radiánban megadott szög, a hello megadva a numerikus kifejezés. |
| [Fokban megadva (num_expr)](#bk_degrees) | Értéket ad vissza megfelelő szög (fokban megadva) az radiánban megadott szög hello. |
| [PI)](#bk_pi) | Beolvasása hello pi konstans érték. |
| [RADIÁNBAN (num_expr)](#bk_radians) | Vissza a radiánban megadott szög, ha egy numerikus kifejezés fokban, is meg kell adni. |
| [EG (num_expr)](#bk_sin) | Beolvasása hello trigonometric szinusza hello radiánban megadott szög, hello a megadott kifejezés. |
| [TAN (num_expr)](#bk_tan) | A megadott kifejezés hello bemeneti kifejezést a hello hello tangensét adja vissza. |

Most futtathatja például hello következő lekérdezéseket:

**Lekérdezés**

    SELECT VALUE ABS(-4)

**Eredmények**

    [4]

hello fő közötti Cosmos DB képest funkciók tooANSI SQL különbség, hogy tervezett toowork séma nélküli és vegyes séma adatokkal is. Például ha egy dokumentum, ahol hello Size tulajdonság hiányzik, vagy rendelkezik-e nem numerikus érték, például "Ismeretlen", majd hello dokumentum keresztül, a rendszer kihagyja helyett hibát ad vissza.

### <a name="type-checking-functions"></a>Írja be az ellenőrzési funkciók
hello típus ellenőrzési funkciók lehetővé teszik az SQL-lekérdezések lévő kifejezés toocheck hello típusú. Ellenőrzési funkciók lehet típusú változó vagy ismeretlen esetén használt toodetermine hello típusú tulajdonságok dokumentumok belül hello menet közben. Ez a táblázat beépített típusa támogatott funkciók ellenőrzése.

<table>
<tr>
  <td><strong>Használat</strong></td>
  <td><strong>Leírás</strong></td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (kifejezés)</a></td>
  <td>Azt jelzi, hogy ha hello érték hello típusú tömb logikai érték beolvasása.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (kifejezés)</a></td>
  <td>Azt jelzi, hogy ha hello típusú hello érték logikai érték logikai érték beolvasása.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (kifejezés)</a></td>
  <td>Olyan logikai érték, amely azt jelzi, ha hello típusú hello érték null beolvasása.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (kifejezés)</a></td>
  <td>Azt jelzi, hogy ha hello típusú hello érték egy szám logikai érték beolvasása.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (kifejezés)</a></td>
  <td>Azt jelzi, hogy ha hello típusú hello érték egy JSON-objektum logikai érték beolvasása.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (kifejezés)</a></td>
  <td>Ha hello típusú hello érték: karakterlánc jelző logikai érték beolvasása.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (kifejezés)</a></td>
  <td>Jelzi, ha hello tulajdonság van rendelve egy érték logikai érték beolvasása.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (kifejezés)</a></td>
  <td>Azt jelzi, hogy ha hello típusú hello érték karakterlánc, szám, logikai érték vagy null logikai érték beolvasása.</td>
</tr>

</table>

Ezeket a funkciókat használ, most lekérdezéseket is futtathat hasonló hello:

**Lekérdezés**

    SELECT VALUE IS_NUMBER(-4)

**Eredmények**

    [true]

### <a name="string-functions"></a>Karakterlánc
hello következő skaláris függvények végrehajtania egy műveletet a bemeneti karakterlánc-érték és a karakterlánc, a numerikus és logikai értéket adja vissza. Itt a következő táblázat a beépített karakterlánc:

| Használat | Leírás |
| --- | --- |
| [A hossz (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_length) |Hello karakterét hello számát adja vissza a megadott karakterlánc-kifejezés |
| [CONCAT (str_expr, str_expr [, str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat) |Egy karakterlánc, amely legalább két karakterlánc-értékek hozzáfűzésével hello eredményét adja vissza. |
| [SUBSTRING (str_expr, num_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_substring) |Egy karakterlánc-kifejezés részét adja vissza. |
| [(Str_expr, str_expr) startswith ELEMNEK](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_startswith) |E hello első karaktersorozat végződik hello második jelző logikai érték beolvasása |
| [Megadott módon VÉGZŐDŐ (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_endswith) |E hello első karaktersorozat végződik hello második jelző logikai érték beolvasása |
| [CONTAINS (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_contains) |Hogy hello első karakterlánc-kifejezés második tartalmaz-e hello jelző logikai érték beolvasása. |
| [INDEX_OF (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_index_of) |Hello hello második karakterlánc-kifejezés hello első megadott karakterlánc-kifejezés vagy-1 érték első előfordulásának pozícióját a indítása, ha hello karakterlánc nem található hello adja vissza. |
| [LEFT (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_left) |Értéket ad vissza egy karakterlánc bal oldalának hello megadott hello a karakterek száma. |
| [JOBB (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_right) |A hello jobb része egy karakterlánc beolvasása hello megadott karakterek száma. |
| [LTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ltrim) |Egy karakterlánc-kifejezés adja vissza, után eltávolítja a kezdő üres. |
| [RTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_rtrim) |Egy karakterlánc-kifejezés az összes záró szóközöket csonkítása után adja vissza. |
| [ALSÓ (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_lower) |Egy karakterlánc-kifejezés nagybetűt adatok toolowercase átalakítása után adja vissza. |
| [FELSŐ (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_upper) |Egy karakterlánc-kifejezés kisbetűt adatok toouppercase átalakítása után adja vissza. |
| [Cserélje le a (str_expr, str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replace) |Megadott karakterlánc-érték összes előfordulását lecseréli egy másik karakterlánc. |
| [REPLIKÁLÁS (str_expr, num_expr)](https://docs.microsoft.com/azure/cosmos-db/documentdb-sql-query-reference#bk_replicate) |A megadott számú alkalommal megismétel egy karakterlánc-érték. |
| [NÉVKERESÉSI (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_reverse) |Hello fordított sorrendben egy karakterlánc értékét adja vissza. |

Ezeket a funkciókat használ, most lekérdezéseket is futtathat hello hasonló. Például visszatérhet hello családnév nagybetűs az alábbiak szerint:

**Lekérdezés**

    SELECT VALUE UPPER(Families.id)
    FROM Families

**Eredmények**

    [
        "WAKEFIELDFAMILY", 
        "ANDERSENFAMILY"
    ]

Vagy ebben a példában például karakterlánc összefűzésére:

**Lekérdezés**

    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families

**Eredmények**

    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "seattle,WA"
    }]


Karakterlánc funkciók is használható hello záradék toofilter eredményeket, hova hello a következő példa:

**Lekérdezés**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")

**Eredmények**

    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]

### <a name="array-functions"></a>A tömb funkciók
a következő skaláris függvények hello végrehajtania egy műveletet a egy tömb bemeneti érték és a numerikus visszatérési, a logikai érték vagy tömb érték. Beépített tömb függvények táblázatát itt található:

| Használat | Leírás |
| --- | --- |
| [ARRAY_LENGTH (arr_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_length) |Hello elemeinek hello számát adja vissza a megadott tömb kifejezés. |
| [ARRAY_CONCAT (arr_expr, arr_expr [, arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat) |Egy tömb, amely két vagy több tömb értékek hozzáfűzésével hello eredményét adja vissza. |
| [ARRAY_CONTAINS (arr_expr, kifejezés [, bool_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains) |Vissza logikai érték, amely azt jelzi, hogy tartalmaz-e hello hello tömb a megadott érték. Ha hello egyezés-e a teljes vagy részleges adhat meg. |
| [ARRAY_SLICE (arr_expr, num_expr [, num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice) |Egy tömböt megadó kifejezést részét adja vissza. |

Tömb funkciók JSON belül használt toomanipulate tömb lehet. Például az ide a lekérdezés, amely visszaadja az összes dokumentumot ahol hello szülők egyik "Multiplexelés Wakefield". 

**Lekérdezés**

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })

**Eredmények**

    [{
      "id": "WakefieldFamily"
    }]

Megadhat részleges töredékkel hello tömbön belüli egyező elemek esetében. hello következő lekérdezés megkeresi a hello minden szülő `givenName` a `Robin`.

**Lekérdezés**

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin" }, true)

**Eredmények**

    [{
      "id": "WakefieldFamily"
    }]


Íme egy másik példa a által használt ARRAY_LENGTH tooget hello gyermekek termékcsalád másodpercenkénti száma.

**Lekérdezés**

    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 

**Eredmények**

    [{
      "id": "WakefieldFamily",
      "numberOfChildren": 2
    },
    {
      "id": "AndersenFamily",
      "numberOfChildren": 1
    }]

### <a name="spatial-functions"></a>Térbeli funkciók
Cosmos DB nyissa meg a földrajzi konzorcium (OGC) beépített funkciók földrajzi lekérdezése a következő hello támogatja. 

<table>
<tr>
  <td><strong>Használat</strong></td>
  <td><strong>Leírás</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (point_expr, point_expr)</td>
  <td>A két hello GeoJSON-pont, sokszög vagy LineString kifejezések hello távolság visszaadása.</td>
</tr>
<tr>
  <td>ST_WITHIN (point_expr, polygon_expr)</td>
  <td>Egy logikai kifejezés, amely azt jelzi, hogy hello első GeoJSON objektum (pont, Polygon, vagy LineString) hello második GeoJSON objektumon belül (pont, sokszög vagy LineString) adja vissza.</td>
</tr>
<tr>
  <td>ST_INTERSECTS (spatial_expr, spatial_expr)</td>
  <td>Egy logikai kifejezés, amely azt jelzi, hogy az intersect hello két megadott GeoJSON objektumokat (pont, Polygon, vagy LineString) adja vissza.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Jelzi, hogy hello megadott GeoJSON-pont, sokszög vagy LineString kifejezés érvényes logikai érték beolvasása.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Értéket ad eredményül, ha hello megadott GeoJSON-pont, sokszög vagy LineString kifejezés logikai értéket tartalmazó JSON-érték érvényes, és ha érvénytelen, továbbá hello OK karakterláncként.</td>
</tr>
</table>

Térbeli funkciók lehet használt tooperform közelségi kapcsolat lekérdezések térbeli adatok alapján. Például az itt a lekérdezés, amely visszaadja az összes termékcsalád dokumentumokat, hogy vannak 30 km-hello belül megadott helyen hello ST_DISTANCE beépített függvény használatával. 

**Lekérdezés**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Eredmények**

    [{
      "id": "WakefieldFamily"
    }]

A földrajzi támogatásáról Cosmos DB további részletekért lásd: [földrajzi adatok az Azure Cosmos DB](geospatial.md). Amely foglalja össze a térbeli funkciók és hello SQL-szintaxis, a Cosmos DB. Most vessen egy pillantást, hogyan működik, és milyen hatással van az hello szintaxis lekérdezése LINQ is láttuk eddig.

## <a id="Linq"></a>LINQ tooDocumentDB API SQL
LINQ .NET programozási modell, amely szerint az objektumok adatfolyamok lekérdezései számítási kifejezze. Cosmos DB biztosít egy ügyféloldali könyvtár toointerface LINQ a JSON és a .NET objektumok közötti konverzió megkönnyítésével, és egy részhalmazát LINQ-leképezés lekérdezések tooCosmos DB lekérdezések. 

az alábbi képen hello a LINQ-lekérdezések Cosmos DB segítségével támogathatja hello architektúráját mutatja be.  Hello Cosmos DB-ügyfélprogram segítségével a fejlesztők hozhat létre egy **IQueryable** objektumot, hogy közvetlenül a lekérdezések hello Cosmos DB lekérdezésszolgáltató, amely ezután fordítja le hello a LINQ lekérdezés egy Cosmos-adatbázis-lekérdezés. hello lekérdezés majd lett átadva a toohello Cosmos DB server tooretrieve eredményt a JSON formátumban. hello visszaadott eredménye egy adatfolyamba való mentésre .NET objektumok hello ügyféloldalon deszerializált.

![A LINQ-lekérdezések használata a DocumentDB API - SQL-szintaxis, JSON lekérdező nyelv, adatbázis fogalmait és az SQL-lekérdezések architektúrája][1]

### <a name="net-and-json-mapping"></a>.NET és a JSON-leképezés
.NET-objektumokat és a JSON-dokumentumok közötti hello hozzárendelést természetes – minden tag adatmező le van képezve tooa JSON-objektumból, ahol hello mezőnév az hello objektum részének toohello "key", illetve az hello "érték" rész rekurzív módon csatlakoztatott toohello érték hello objektum része. Vegye figyelembe a következő példa hello: hello termékcsalád létrehozott célja csatlakoztatott toohello JSON-dokumentum alább látható módon. És ez fordítva is igaz, hello JSON-dokumentum csatlakoztatott hátsó tooa .NET objektum.

**C#-osztály**

    public class Family
    {
        [JsonProperty(PropertyName="id")]
        public string Id;
        public Parent[] parents;
        public Child[] children;
        public bool isRegistered;
    };

    public struct Parent
    {
        public string familyName;
        public string givenName;
    };

    public class Child
    {
        public string familyName;
        public string givenName;
        public string gender;
        public int grade;
        public List<Pet> pets;
    };

    public class Pet
    {
        public string givenName;
    };

    public class Address
    {
        public string state;
        public string county;
        public string city;
    };

    // Create a Family object.
    Parent mother = new Parent { familyName= "Wakefield", givenName="Robin" };
    Parent father = new Parent { familyName = "Miller", givenName = "Ben" };
    Child child = new Child { familyName="Merriam", givenName="Jesse", gender="female", grade=1 };
    Pet pet = new Pet { givenName = "Fluffy" };
    Address address = new Address { state = "NY", county = "Manhattan", city = "NY" };
    Family family = new Family { Id = "WakefieldFamily", parents = new Parent [] { mother, father}, children = new Child[] { child }, isRegistered = false };


**JSON**  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", 
                "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
              "familyName": "Miller", 
              "givenName": "Lisa", 
              "gender": "female", 
              "grade": 8 
            }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
    };



### <a name="linq-toosql-translation"></a>LINQ tooSQL fordítás
hello Cosmos DB lekérdezésszolgáltató hajt végre, egy Cosmos-adatbázis SQL-lekérdezést az elérhető legjobb leképezéseket a LINQ lekérdezés. A leírás a következő hello feltételezzük, hogy hello olvasó rendelkezik a LINQ alapszintű ismeretét.

Először hello típus rendszer esetében támogatott összes JSON egyszerű típusokhoz – numerikus típusok, logikai érték, karakterlánc vagy null. Ezek a JSON típusok támogatottak. a következő skaláris kifejezések hello támogatott.

* Állandó értékek – ezek közé tartozik a primitív adattípusokat hello állandó értékek hello tartalmazó lekérdezés kiértékelése hello időpontban.
* / Tulajdonságtömb-index kifejezések – ezek a kifejezések toohello tulajdonság az objektum vagy tömb elem hivatkozik.
  
     termékcsalád. Azonosító;    Family.children[0].familyName;    Family.children[0].grade;    Family.children[n].grade; n egy int változó
* Aritmetikai kifejezésekben - ezek közé tartozik a numerikus és logikai értékek a közös aritmetikai kifejezésekben. Hello teljes listájáért tekintse meg a toohello SQL megadását.
  
     2 * family.children[0].grade;    az x + y;
* Karakterlánc-összehasonlítási kifejezés - ilyenek összehasonlításával karakterlánc érték toosome állandó karakterlánc-érték.  
  
     mother.familyName == "Smith";    child.givenName == s; egy karakterlánc-változóvá-je
* Objektum vagy tömb létrehozása kifejezés - ezek a kifejezések visszatérési összetett érték vagy névtelen típusú objektum vagy egy ilyen objektumokból álló tömb. Ezek az értékek egymásba ágyazható.
  
     új szülő {familyName = a "Smith" givenName = "Joe"}; új {első = 1, a második = 2}; egy két mező rendelkező névtelen típusú              
     új int [] {3, child.grade, 5};

### <a id="SupportedLinqOperators"></a>Támogatott LINQ operátorok listája
Ez egy lista támogatott LINQ üzemeltetők a DocumentDB .NET SDK hello mellékelt hello LINQ szolgáltatónál.

* **Válassza ki**: leképezések fordítása SQL SELECT objektumkonstrukciók beleértve toohello
* **Ha**: szűrők, amelyek SQL WHERE toohello, és támogatja a közötti címfordítás & &, || és! toohello SQL operátorok
* **A selectmany metódus**: lehetővé teszi a tömbök toohello SQL JOIN záradékban visszagörgetésének. A tömb elemeinek használt toochain/nest kifejezések toofilter lehet
* **OrderBy és OrderByDescending**: az eszköz tooORDER BY növekvő/csökkenő
* **Count**, **Sum**, **Min**, **maximális**, és **átlagos** összesítő és a megfelelő aszinkron operátorok **CountAsync**, **SumAsync**, **MinAsync**, **MaxAsync**, és **AverageAsync**.
* **CompareTo**: az eszköz toorange összehasonlítást. Gyakran használt karakterláncok óta fontosságúak nem hasonlítható össze az .NET
* **Igénybe**: toohello SQL felső lefordítja a lekérdezés eredményeinek korlátozása
* **Matematikai függvények**: támogatja a fordítás. NET tartozó Abs, ARCCOS, ARCSIN, Atan Cos felső határa, Exp, emelet, napló, Log10, Pow, ciklikus, bejelentkezési, EG, Sqrt, Tan, Truncate toohello egyenértékű SQL beépített funkciók.
* **Karakterlánc**: támogatja a fordítás. NET tartozó Concat, Contains, megadott módon végződő, IndexOf, Count, ToLower, TrimStart, csere, névkeresési, TrimEnd, megadott módon kezdődő, SubString, ToUpper toohello egyenértékű SQL beépített funkciók.
* **A tömb funkciók**: támogatja a fordítás. NET tartozó Concat, tartalmazza, és a Count toohello egyenértékű SQL beépített függvény.
* **A földrajzi Kiterjesztésfüggvények**: támogatja a helyettes módszerek távolság belül IsValid, a fordítás és IsValidDetailed toohello egyenértékű SQL beépített funkciók.
* **Felhasználó által definiált függvény kiterjesztésfüggvény**: támogatja a fordítás a hello helyettes metódus UserDefinedFunctionProvider.Invoke toohello megfelelő felhasználó által definiált függvény.
* **Vegyes**: hello fordítása coalesce támogatja, és a feltételes operátort. Contains tooString lefordíthatja tartalmazza, ARRAY_CONTAINS vagy hello SQL IN attól függően, hogy a környezetben.

### <a name="sql-query-operators"></a>SQL-lekérdezési operátorok
Íme néhány példa, amelyek bemutatják, hogyan lefordítani néhány hello szabványos LINQ lekérdezési operátorok az tooCosmos DB lekérdezéseket.

#### <a name="select-operator"></a>Válasszon operátort
hello szintaxisa `input.Select(x => f(x))`, ahol `f` egy skaláris kifejezés.

**LINQ lambda kifejezés**

    input.Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f



**LINQ lambda kifejezés**

    input.Select(family => family.children[0].grade + c); // c is an int variable


**SQL** 

    SELECT VALUE f.children[0].grade + c
    FROM Families f 



**LINQ lambda kifejezés**

    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });


**SQL** 

    SELECT VALUE {"name":f.children[0].familyName, 
                  "grade": f.children[0].grade + 3 }
    FROM Families f



#### <a name="selectmany-operator"></a>A selectmany metódus operátor
hello szintaxisa `input.SelectMany(x => f(x))`, ahol `f` egy skaláris kifejezés, amely a gyűjteménytípus adja vissza.

**LINQ lambda kifejezés**

    input.SelectMany(family => family.children);

**SQL** 

    SELECT VALUE child
    FROM child IN Families.children



#### <a name="where-operator"></a>Ha operátor
hello szintaxisa `input.Where(x => f(x))`, ahol `f` van egy skaláris kifejezés, amely egy logikai értéket ad vissza.

**LINQ lambda kifejezés**

    input.Where(family=> family.parents[0].familyName == "Smith");

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith" 



**LINQ lambda kifejezés**

    input.Where(
        family => family.parents[0].familyName == "Smith" && 
        family.children[0].grade < 3);

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
    AND f.children[0].grade < 3


### <a name="composite-sql-queries"></a>Összetett SQL-lekérdezések
operátorok fent hello lehet össze tooform nagyobb teljesítményű lekérdezések. Mivel Cosmos DB támogatja a beágyazott gyűjtemények, hello összeállítás kell összefűzendő vagy beágyazott.

#### <a name="concatenation"></a>Összefűzése
hello szintaxisa `input(.|.SelectMany())(.Select()|.Where())*`. Olyan összefűzött lekérdezés elindíthatja és egy opcionális `SelectMany` lekérdezés követ több `Select` vagy `Where` operátorok.

**LINQ lambda kifejezés**

    input.Select(family=>family.parents[0])
        .Where(familyName == "Smith");

**SQL**

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"



**LINQ lambda kifejezés**

    input.Where(family => family.children[0].grade > 3)
        .Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f
    WHERE f.children[0].grade > 3



**LINQ lambda kifejezés**

    input.Select(family => new { grade=family.children[0].grade}).
        Where(anon=> anon.grade < 3);

**SQL** 

    SELECT *
    FROM Families f
    WHERE ({grade: f.children[0].grade}.grade > 3)



**LINQ lambda kifejezés**

    input.SelectMany(family => family.parents)
        .Where(parent => parents.familyName == "Smith");

**SQL** 

    SELECT *
    FROM p IN Families.parents
    WHERE p.familyName = "Smith"



#### <a name="nesting"></a>A beágyazási
hello szintaxisa `input.SelectMany(x=>x.Q())` Q esetén egy `Select`, `SelectMany`, vagy `Where` operátor.

Egy beágyazott lekérdezésen hello belső lekérdezés hello külső gyűjtemény alkalmazott tooeach eleme. Egyik fontos szolgáltatása hello belső lekérdezés hivatkozhat toohello mezők hello külső gyűjtemény például hello elemszámának Önillesztések.

**LINQ lambda kifejezés**

    input.SelectMany(family=> 
        family.parents.Select(p => p.familyName));

**SQL** 

    SELECT VALUE p.familyName
    FROM Families f
    JOIN p IN f.parents


**LINQ lambda kifejezés**

    input.SelectMany(family => 
        family.children.Where(child => child.familyName == "Jeff"));

**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = "Jeff"



**LINQ lambda kifejezés**

    input.SelectMany(family => family.children.Where(
        child => child.familyName == family.parents[0].familyName));

**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = f.parents[0].familyName


## <a id="ExecutingSqlQueries"></a>SQL-lekérdezések végrehajtása
A cosmos DB keresztül tesz elérhetővé erőforrásokat egy REST API-t, amely képes a HTTP/HTTPS-kérést bármely olyan nyelvvel hívható. Ezenfelül a Cosmos DB programozási kódtárakat, például a .NET, Node.js, JavaScript és Python számos népszerű nyelvhez biztosít. hello REST API-t és hello különböző szalagtárak összes támogatja az SQL keresztül lekérdezése. hello .NET SDK lekérdezése továbbá tooSQL LINQ támogatja.

a következő példák szemléltetik hogyan hello toocreate egy lekérdezést, és küldje el egy Cosmos-adatbázis adatbázis-fiók.

### <a id="RestAPI"></a>REST API-N
Cosmos DB egy megnyitott RESTful programozási modellt biztosít a HTTP Protokollon keresztül. Adatbázis-fiókok egy Azure-előfizetés használatával telepíthető. hello Cosmos DB erőforrás-modellje egy adatbázis-fiók, amelyek egy-címezhető logikai és állandó URI-k használata alatt lévő erőforrások készlete áll. Erőforráscsoport adatcsatornára ebben a dokumentumban említett tooas. Az adatbázisfiók áll az adatbázisok, mindegyike több gyűjteményt, mely szolgálna mindegyikének tartalmazza a dokumentumok, a felhasználó által megadott függvények és a más típusú erőforrások.

Ezekkel az erőforrásokkal hello alapvető interakció modell keresztül hello HTTP-műveletek GET, PUT, POST és DELETE a szabványos tolmácsolási szolgáltatással. hello POST művelet egy új erőforrást, egy tárolt eljárás végrehajtása vagy egy Cosmos-adatbázis-lekérdezés kiadására szolgál. Lekérdezéseket a rendszer mindig csak olvasható műveletekhez, nincs mellékhatásokkal.

hello következő példák azt szemléltetik, amennyiben azt már áttekintette felé irányuló hello két minta dokumentumok tartalmazó gyűjtemény DocumentDB API lekérdezés POST. hello lekérdezés egy egyszerű szűrési hello JSON name tulajdonsággal rendelkezik. Vegye figyelembe a hello hello használata `x-ms-documentdb-isquery` és a Content-Type: `application/query+json` fejlécek toodenote, amely hello a művelethez az a lekérdezés.

**Kérés**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {      
        "query": "SELECT * FROM Families f WHERE f.id = @familyId",     
        "parameters": [          
            {"name": "@familyId", "value": "AndersenFamily"}         
        ] 
    }


**Eredmények**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32

    <indented for readability, results highlighted>

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "id":"AndersenFamily",
             "lastName":"Andersen",
             "parents":[  
                {  
                   "firstName":"Thomas"
                },
                {  
                   "firstName":"Mary Kay"
                }
             ],
             "children":[  
                {  
                   "firstName":"Henriette Thaulow",
                   "gender":"female",
                   "grade":5,
                   "pets":[  
                      {  
                         "givenName":"Fluffy"
                      }
                   ]
                }
             ],
             "address":{  
                "state":"WA",
                "county":"King",
                "city":"seattle"
             },
             "_rid":"u1NXANcKogEcAAAAAAAAAA==",
             "_ts":1407691744,
             "_self":"dbs\/u1NXAA==\/colls\/u1NXANcKogE=\/docs\/u1NXANcKogEcAAAAAAAAAA==\/",
             "_etag":"00002b00-0000-0000-0000-53e7abe00000",
             "_attachments":"_attachments\/"
          }
       ],
       "count":1
    }


hello második példáját mutatja egy összetettebb lekérdezés, amely több eredményt ad vissza hello illesztési.

**Kérés**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {      
        "query": "SELECT 
                     f.id AS familyName, 
                     c.givenName AS childGivenName, 
                     c.firstName AS childFirstName, 
                     p.givenName AS petName 
                  FROM Families f 
                  JOIN c IN f.children 
                  JOIN p in c.pets",     
        "parameters": [] 
    }


**Eredmények**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84

    <indented for readability, results highlighted>

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "familyName":"AndersenFamily",
             "childFirstName":"Henriette Thaulow",
             "petName":"Fluffy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Goofy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Shadow"
          }
       ],
       "count":3
    }


Ha a lekérdezés eredményei nem férnek el az eredmények egyoldalas belül, akkor hello REST API-t adja vissza a folytatási kód keresztül hello `x-ms-continuation-token` válaszfejlécet. Ügyfelek későbbi eredmények hello fejléc-ot is megjelenítheti az eredményeket. hello száma laponként eredmények is szabályozható hello `x-ms-max-item-count` számú fejléc. Ha hello a megadott lekérdezés tartalmaz egy összesítő függvényt, például `COUNT`, majd hello lekérdezés lap hello oldalra, egy részben összesített értéket adhat vissza. hello ügyfelek kell az eredmények tooproduce hello végső eredményeket, például egy második szintű összesítő képes, összeg keresztül hello számok hello oldalakra tooreturn hello teljes számot adja vissza.

toomanage hello adatok konzisztencia házirend lekérdezések esetén használjon hello `x-ms-consistency-level` például minden REST API-kérés fejlécének. A munkamenet-konzisztencia esetén szükséges tooalso echo hello legújabb `x-ms-session-token` Cookie-fejlécének hello lekérdezési kérelemben. hello lekérdezett gyűjtemény indexelési házirendet is befolyásolhatják a lekérdezés eredményeinek hello konzisztencia. Hello alapértelmezett házirend-beállítások indexelő, gyűjtemények hello az index az mindig naprakész lesz hello dokumentum tartalma és a lekérdezési eredmények felel meg az adatok választott hello konzisztencia. Ha indexelés házirend hello laza tooLazy, lekérdezések elavult eredmények adhat vissza. További információkért lásd: [Azure Cosmos DB Konzisztenciaszintek][consistency-levels].

Ha indexelési házirendet konfigurált hello hello gyűjteményen hello megadott lekérdezés nem támogatja, a hello Azure Cosmos adatbázis-kiszolgálót 400 "hibás kérelem" adja vissza. A kivonatoló (egyenlő) keresések, és kifejezetten kizárja indexelő elérési út beállítva elérési utak tartomány lekérdezések esetében adja vissza. Hello `x-ms-documentdb-query-enable-scan` fejléc lehet a megadott tooallow hello lekérdezés tooperform a vizsgálat nem érhető el index.

Úgy, hogy a lekérdezés-végrehajtás részletes metrikák is ki `x-ms-documentdb-populatequerymetrics` fejléc túl`True`. További információkért lásd: [Azure Cosmos DB DocumentDB API SQL-lekérdezés metrikáját](documentdb-sql-query-metrics.md).

### <a id="DotNetSdk"></a>C# (.NET) SDK
hello .NET SDK támogatja a LINQ és az SQL lekérdezése. hello következő példa bemutatja, hogyan tooperform hello egyszerű szűrő lekérdezés rendszerben jelent meg a jelen dokumentum korábbi.

    foreach (var family in client.CreateDocumentQuery(collectionLink, 
        "SELECT * FROM Families f WHERE f.id = \"AndersenFamily\""))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    SqlQuerySpec query = new SqlQuerySpec("SELECT * FROM Families f WHERE f.id = @familyId");
    query.Parameters = new SqlParameterCollection();
    query.Parameters.Add(new SqlParameter("@familyId", "AndersenFamily"));

    foreach (var family in client.CreateDocumentQuery(collectionLink, query))
    {
        Console.WriteLine("\tRead {0} from parameterized SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery(collectionLink)
        where f.Id == "AndersenFamily"
        select f))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in client.CreateDocumentQuery(collectionLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


Ez a minta összehasonlítja két tulajdonságainak egyenlőség minden a dokumentumban, és névtelen leképezések használja. 

    foreach (var family in client.CreateDocumentQuery(collectionLink,
        @"SELECT {""Name"": f.id, ""City"":f.address.city} AS Family 
        FROM Families f 
        WHERE f.address.city = f.address.state"))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery<Family>(collectionLink)
        where f.address.city == f.address.state
        select new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in
        client.CreateDocumentQuery<Family>(collectionLink)
        .Where(f => f.address.city == f.address.state)
        .Select(f => new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


hello a következő példa bemutatja illesztések, LINQ selectmany metódus használatával.

    foreach (var pet in client.CreateDocumentQuery(collectionLink,
          @"SELECT p
            FROM Families f 
                 JOIN c IN f.children 
                 JOIN p in c.pets 
            WHERE p.givenName = ""Shadow"""))
    {
        Console.WriteLine("\tRead {0} from SQL", pet);
    }

    // Equivalent in Lambda expressions
    foreach (var pet in
        client.CreateDocumentQuery<Family>(collectionLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }



hello .NET ügyfél automatikusan a lekérdezési eredmények hello foreach blokkokban a fentiek szerint minden hello oldal telepítéseket. hello lekérdezés hello REST API-t a szakaszban bemutatott beállításokat is rendelkezésre állnak a .NET SDK használatával hello hello `FeedOptions` és `FeedResponse` hello CreateDocumentQuery metódus az osztályokat. lapok száma hello vezérelhető hello `MaxItemCount` beállítást. 

Lapozófájl létrehozásával közvetlenül is szabályozhatja `IDocumentQueryable` hello segítségével `IQueryable` objektumot, majd ehhez beolvassa a` ResponseContinuationToken` gépet értékeket, és átadja őket `RequestContinuationToken` a `FeedOptions`. `EnableScanInQuery`set tooenable vizsgálatok akkor is, ha hello lekérdezés konfigurált hello indexelési házirend által nem támogatott. A particionált gyűjtemények használhatják `PartitionKey` toorun hello lekérdezése egyetlen partícióazonosító (bár a Cosmos DB automatikusan nyerhet ki ez a lekérdezés szövegének hello), és `EnableCrossPartitionQuery` esetleg toobe toorun lekérdezéseket futtathat több partíciót. 

Tekintse meg a túl[Azure Cosmos DB .NET minták](https://github.com/Azure/azure-documentdb-net) további mintákat tartalmazó lekérdezések. 

### <a id="JavaScriptServerSideApi"></a>JavaScript kiszolgálóoldali API
A cosmos DB hajthatók végre alapú JavaScript-alkalmazáslogikát tárolt eljárások és eseményindítók hello gyűjtemények közvetlenül a programozási modellt biztosít. hello JavaScript-logika regisztrálva, a gyűjtemény szintjén majd adhatnak ki a megadott gyűjtemény hello hello hello dokumentumok műveleteket a Helyadatbázis-műveletekhez. Ezek a műveletek a környezeti ACID-tranzakciókat van burkolva.

hello következő példa bemutatja, hogyan toouse hello queryDocuments a JavaScript-kiszolgáló API hello toomake lekérdezi a vállalaton belüli tárolt eljárások és eseményindítók.

    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();
        var collectionLink = collectionManager.getSelfLink()

        // create a new document.
        collectionManager.createDocument(collectionLink,
            { name: name, author: author },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);

                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);

                        // Replace hello author name for all documents that satisfied hello query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don't need tooexecute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }

## <a id="References"></a>Hivatkozások
1. [Bevezetés tooAzure Cosmos DB][introduction]
2. [Az Azure Cosmos adatbázis SQL-specifikációja](http://go.microsoft.com/fwlink/p/?LinkID=510612)
3. [Azure Cosmos DB .NET-minták](https://github.com/Azure/azure-documentdb-net)
4. [Az Azure Cosmos DB Konzisztenciaszintek][consistency-levels]
5. ANSI SQL 2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
6. JSON [http://json.org/](http://json.org/)
7. JavaScript-specifikáció [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm) 
8. LINQ [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx) 
9. Lekérdezés kiértékelése technikák nagy adatbázisokhoz [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611)
10. A lekérdezés feldolgozás alatt álló párhuzamos relációs adatbázis-rendszerek, IEEE számítógép társadalom nyomja le az 1994.
11. Lu, Ooi, Tan, feldolgozás alatt álló párhuzamos relációs adatbázis-rendszerek, IEEE számítógép társadalom nyomja le az 1994 lekérdezés.
12. Christopher Olston, Benjamin Reed, Utkarsh Srivastava, Ravi Kumar, Andrew Tomkins: a Pig Latin: egy nem, külső nyelvi SIGMOD 2008 az adatok feldolgozásához.
13. G. Graefe. optimalizálás hello kaszkádokban kerete. IEEE adatok Eng. BULL., 18(3): 1995.

[1]: ./media/documentdb-sql-query/sql-query1.png
[introduction]: introduction.md
[consistency-levels]: consistency-levels.md