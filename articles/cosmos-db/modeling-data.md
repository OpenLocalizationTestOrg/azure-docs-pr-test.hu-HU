---
title: "NoSQL-adatbázis aaaModeling dokumentum adatok |} Microsoft Docs"
description: "További tudnivalók a modellezési adatok NoSQL-adatbázisok"
keywords: "adatok modellezését"
services: cosmos-db
author: arramac
manager: jhubbard
editor: mimig1
documentationcenter: 
ms.assetid: 69521eb9-590b-403c-9b36-98253a4c88b5
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2016
ms.author: arramac
ms.openlocfilehash: 2e388c833f204287896dfa8e6f79c88073731b6b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="modeling-document-data-for-nosql-databases"></a>Modellezési dokumentum adatok NoSQL-adatbázisok
Amíg a sémamentes adatbázisok, például Azure Cosmos DB, könnyen super tooembrace módosítások tooyour adatmodell továbbra is kell töltött időt vesz igénybe, továbbléphetnek az adatok. 

Hogyan érintetlen toobe tárolt adatokat? Hogyan van az alkalmazások folyamatos tooretrieve és lekérdezés adatait? Az a alkalmazás vastag olvasási vagy írási nehéz? 

A cikk elolvasása után fog tudni tooanswer hello a következő kérdéseket:

* Hogyan kell egy dokumentumot egy dokumentum-adatbázisban fontolnunk?
* Mi az adatok modellezési, és miért érdemes I fontos? 
* Hogyan van dokumentum adatbázis különböző tooa relációs adatbázis modellezési adatokat?
* Hogyan express a nem relációs adatbázis adatok kapcsolatokat?
* Ha adatok beágyazása, és ha hivatkozás toodata?

## <a name="embedding-data"></a>Adatok beágyazása
Amikor elkezdi az adatok Azure Cosmos DB, például egy dokumentum áruházban modellezését próbálja tootreat a szervezetek, mint a **önálló dokumentumok** JSON jelöli.

Ahhoz, hogy férhet hozzá túl sokkal tovább, ossza meg velünk néhány lépésekkel vissza és tekintse meg a következő hogyan azt előfordulhat, hogy a modell egy relációs adatbázisban, velünk számos már ismeri a tárgy valamit. hello következő példa bemutatja, hogyan tárolódhat egy személy egy relációs adatbázisban. 

![Relációs adatbázis-modell](./media/documentdb-modeling-data/relational-data-model.png)

Az használatakor a relációs adatbázisok azt az év toonormalize a tanulási már normalizálása, optimalizálására.

Az adatok általában normalizálása magában foglalja a entitás, például egy személy véve, és az adatok toodiscrete kódrészletek bontásához. Hello a fenti példában egy személy több kapcsolattartási részletes rekordok, valamint több címadatokat lehetnek. Azt még egy lépéssel további, és kapcsolattartási adatai szerint lebontva további kibontása közös mezők, például egy típust. Ugyanazt a címet, itt rekordokban típussal rendelkezik például *Home* vagy *üzleti* 

helyi irányítsa, amikor adatokat normalizálása túl hello**ne tárolja a redundáns adatok** egyes jegyezze fel, és inkább tekintse meg a toodata. Ebben a példában egy személy, a kapcsolattartási adatait és a címek tooread kell toouse ILLESZTÉSEK tooeffectively összesített az adatokat a futási időt.

    SELECT p.FirstName, p.LastName, a.City, cd.Detail
    FROM Person p
    JOIN ContactDetail cd ON cd.PersonId = p.Id
    JOIN ContactDetailType on cdt ON cdt.Id = cd.TypeId
    JOIN Address a ON a.PersonId = p.Id

Az írási műveletek egyetlen frissítése a kapcsolattartási adatait és a címek igényel sok egyes táblák között. 

Most tegyük egy pillantást hogyan azt kellene modell hajtsa végre a megfelelő hello azonos adatok önálló egységként dokumentum-adatbázisban.

    {
        "id": "1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "addresses": [
            {            
                "line1": "100 Some Street",
                "line2": "Unit 1",
                "city": "Seattle",
                "state": "WA",
                "zip": 98012
            }
        ],
        "contactDetails": [
            {"email: "thomas@andersen.com"},
            {"phone": "+1 555 555-5555", "extension": 5555}
        ] 
    }

Hello megközelítéssel fenti most tudunk **denormalizált** személy rekord hello ahol azt **beágyazott** összes hello toothis személy, például a kapcsolattartási adatait és a címet, egyetlen tooa vonatkozó információkat JSON-dokumentum.
Ezenkívül mivel azt még nem korlátozódik tooa rögzített tudunk séma hello rugalmasságot toodo többek között a teljes mértékben rendelkező különböző alakzatok kapcsolattartási adatait. 

A teljes személy rekord lekérése hello adatbázis már olvasási művelete egyetlen gyűjtemény ellen, és egyetlen dokumentum egyetlen. Frissítés személy rekord, a kapcsolattartási adatait és a címet, akkor is egyetlen dokumentum szemben egyetlen írási művelet.

Az alkalmazás által denormalizing adatok, esetleg tooissue kevesebb lekérdezések és frissítések toocomplete gyakori műveletekhez. 

### <a name="when-tooembed"></a>Ha tooembed
Általában a beágyazott adatok használata esetén a modellek:

* Nincsenek **tartalmaz** entitások közötti kapcsolatok.
* Nincsenek **egy néhány** entitások közötti kapcsolatok.
* Beágyazott adat, amely **ritkán változó**.
* Nincs beágyazott adatok nem nő **nélkül kötött**.
* Nincs a beágyazott adatok **szerves** toodata a dokumentumban.

> [!NOTE]
> Általában a modellek biztosítják jobban adatok denormalizált **olvasási** teljesítményét.
> 
> 

### <a name="when-not-tooembed"></a>Ha nem tooembed
Hello tapasztalatok dokumentum-adatbázisban toodenormalize minden rendben, és minden adat beágyazása tooa egyetlen dokumentum, ennek eredményeképpen előfordulhat toosome olyan helyzetekben, el kell kerülni.

A JSON-részlet igénybe vehet.

    {
        "id": "1",
        "name": "What's new in hello coolest Cloud",
        "summary": "A blog post by someone real famous",
        "comments": [
            {"id": 1, "author": "anon", "comment": "something useful, I'm sure"},
            {"id": 2, "author": "bob", "comment": "wisdom from hello interwebs"},
            …
            {"id": 100001, "author": "jane", "comment": "and on we go ..."},
            …
            {"id": 1000000001, "author": "angry", "comment": "blah angry blah angry"},
            …
            {"id": ∞ + 1, "author": "bored", "comment": "oh man, will this ever end?"},
        ]
    }

Ez akkor lehet, hogy mi a feladás egy vagy több entitás beágyazott megjegyzésekkel néz azt modellezési volt egy tipikus blogba vagy CMS, a rendszer. hello ebben a példában probléma a megjegyzések tömb hello **unbounded**, ami azt jelenti, hogy van-e bármilyen egyetlen post rendelkezhet megjegyzések számának nincs (gyakorlati) korlátozása toohello. Ez a növekedésével hello dokumentum hello mérete sikerült jelentősen probléma lesz.

Hello hello méretét, a dokumentum bővül hello képességét tootransmit hello adatok hello keresztülhaladnak a hálózaton, valamint az olvasási és frissítési hello a dokumentumban léptékű csökkenhet.

Ebben az esetben a következő modell jobban tooconsider hello lenne.

    Post document:
    {
        "id": "1",
        "name": "What's new in hello coolest Cloud",
        "summary": "A blog post by someone real famous",
        "recentComments": [
            {"id": 1, "author": "anon", "comment": "something useful, I'm sure"},
            {"id": 2, "author": "bob", "comment": "wisdom from hello interwebs"},
            {"id": 3, "author": "jane", "comment": "....."}
        ]
    }

    Comment documents:
    {
        "postId": "1"
        "comments": [
            {"id": 4, "author": "anon", "comment": "more goodness"},
            {"id": 5, "author": "bob", "comment": "tails from hello field"},
            ...
            {"id": 99, "author": "angry", "comment": "blah angry blah angry"}
        ]
    },
    {
        "postId": "1"
        "comments": [
            {"id": 100, "author": "anon", "comment": "yet more"},
            ...
            {"id": 199, "author": "bored", "comment": "will this ever end?"}
        ]
    }

A modellnek hello három legutóbbi megjegyzések hello a beágyazott utáni magát, amely egy tömb rögzített kötött az ez idő. hello más megjegyzések toobatches 100 megjegyzések találhatók, és külön dokumentumok tárolja. hello köteg mérete hello mint 100 lett választva, mert a fiktív alkalmazás lehetővé teszi, hogy hello felhasználói tooload 100 megjegyzések egyszerre.  

Egy másik esetet, amikor beágyazási adatok nem jó ötlet esetén hello a beágyazott adatok gyakran használt dokumentumok között, és gyakran változik. 

A JSON-részlet igénybe vehet.

    {
        "id": "1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "holdings": [
            {
                "numberHeld": 100,
                "stock": { "symbol": "zaza", "open": 1, "high": 2, "low": 0.5 }
            },
            {
                "numberHeld": 50,
                "stock": { "symbol": "xcxc", "open": 89, "high": 93.24, "low": 88.87 }
            }
        ]
    }

Ez lehet egy személy készlet portfóliót. Azt választotta, hogy tooembed hello tőzsdei információk tooeach portfóliót dokumentumban. Egy olyan környezetben, ahol kapcsolódó adatok változik gyakran, például egy alkalmazás, mivel a készlet adatok gyakran változnak beágyazás állapotra vált, hogy folyamatosan frissítjük az egyes portfóliót dokumentum minden alkalommal, amikor egy készlet forog toomean.

Készlet *zaza* egyetlen alkalommal több száz kerülhetnek rendelkezhetnek a nap és a felhasználók ezreit *zaza* a saját portfóliót. A fenti hello például adatmodellt azt kellene tooupdate portfóliót dokumentumok sok ezer sokszor nem jól méretezhető tooa rendszer vezető naponta. 

## <a id="Refer"></a>Adatok hivatkozik
Igen sok esetben szépen adatok beágyazása működik, de egyértelmű, hogy nincsenek helyzetek, amikor az adatok denormalizing mint érdemes további problémákat okozhat. Ezért Mi a teendő most? 

Relációs adatbázisok nincsenek hello egyetlen hely, ahol létrehozhat entitások közötti kapcsolatok. A dokumentum-adatbázisban mindig egy dokumentum más dokumentumban toodata ténylegesen kapcsolódó információk. Most I vagyok nem javasolni egy percig, még akkor is, hogy azt rendszerek jobb olyan környezethez a legalkalmasabb tooa relációs adatbázis az Azure Cosmos Adatbázisba, vagy bármely más dokumentum-adatbázis létrehozása, de egyszerű kapcsolatok rendben, és nagyon hasznos lehet. 

Az alábbi JSON hello választottuk korábban, de azt tekintse meg a készlet cikk toohello hello portfóliót beágyazás helyett most egy készlet portfóliót toouse hello példát. Ezzel a módszerrel megváltozásakor hello készlet elem gyakran hello nap hello csak a dokumentum, amelyet a frissített toobe hello egyetlen készlet dokumentum. 

    Person document:
    {
        "id": "1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "holdings": [
            { "numberHeld":  100, "stockId": 1},
            { "numberHeld":  50, "stockId": 2}
        ]
    }

    Stock documents:
    {
        "id": "1",
        "symbol": "zaza",
        "open": 1,
        "high": 2,
        "low": 0.5,
        "vol": 11970000,
        "mkt-cap": 42000000,
        "pe": 5.89
    },
    {
        "id": "2",
        "symbol": "xcxc",
        "open": 89,
        "high": 93.24,
        "low": 88.87,
        "vol": 2970200,
        "mkt-cap": 1005000,
        "pe": 75.82
    }


Az azonnali hátránya toothis megoldás azonban, ha az alkalmazás szerepel-e minden olyan személy portfóliót; megjelenítésekor tartott készlet szükséges tooshow információ Ebben az esetben kellene toomake több utazgatással toohello adatbázis tooload hello adatai készlet dokumentumok. Itt hajtottunk írási műveleteket, amelyek gyakran hello nap folyamán fordulhat elő, de az olvasási műveleteket, amelyek vélhetően kisebb mértékű befolyásolása mellett a hello teljesítmény az adott rendszer hello pedig sérült a döntési tooimprove hello hatékonyságát.

> [!NOTE]
> Normalizált adatmodellekben **megkövetelheti további kiszolgálókkal való adatváltások számát** toohello kiszolgáló.
> 
> 

### <a name="what-about-foreign-keys"></a>Mi a helyzet a külső kulcsokat?
Nincs jelenleg egy megkötés, mert külső kulcsok vagy egyéb, rendelkezésre álló dokumentumok közötti dokumentum kapcsolat hatékony "gyenge links" és hello adatbázis maga nem ellenőrzi. Ha azt szeretné, hogy a dokumentum hivatkozik adatok hello tooensure tooactually létezik-e, majd toodo ez szükséges az alkalmazás vagy hello használata kiszolgálóoldali eseményindítók és tárolt eljárások az Azure Cosmos-adatbázis.

### <a name="when-tooreference"></a>Ha tooreference
Általában a normalizált adatok használata esetén a modellek:

* Képviselő **egy-a-többhöz** kapcsolatokat.
* Képviselő **több-a-többhöz** kapcsolatokat.
* A kapcsolódó adatok **változik gyakran**.
* Hivatkozásban szereplő lehet **unbounded**.

> [!NOTE]
> Általában normalizálása jobban biztosít **írási** teljesítményét.
> 
> 

### <a name="where-do-i-put-hello-relationship"></a>Ha helyezze a hello kapcsolatban?
hello növekedési hello kapcsolat segítségével megállapíthatja, hogy mely toostore hello hivatkozás található.

Ha úgy tekintünk hello JSON az alábbi, gyártók és -könyvekkel modellek.

    Publisher document:
    {
        "id": "mspress",
        "name": "Microsoft Press",
        "books": [ 1, 2, 3, ..., 100, ..., 1000]
    }

    Book documents:
    {"id": "1", "name": "Azure Cosmos DB 101" }
    {"id": "2", "name": "Azure Cosmos DB for RDBMS Users" }
    {"id": "3", "name": "Taking over hello world one JSON doc at a time" }
    ...
    {"id": "100", "name": "Learn about Azure Cosmos DB" }
    ...
    {"id": "1000", "name": "Deep Dive in tooAzure Cosmos DB" }

Ha hello könyvek / publisher hello száma korlátozott növekedési kicsi, majd tárolja a hello könyv hivatkozás hello publisher dokumentumban lévő lehetnek hasznosak. Azonban ha hello száma / publisher könyvek unbounded, majd az adatmodell átmásolásának toomutable, tömbök, mint hello példa publisher dokumentum fenti növekszik. 

Váltás körül bit dolgot volna okozza, hogy továbbra is jelöli most hello-e ugyanazokat az adatokat egy modell Ezzel elkerülheti nagy változtatható gyűjtemények.

    Publisher document: 
    {
        "id": "mspress",
        "name": "Microsoft Press"
    }

    Book documents: 
    {"id": "1","name": "Azure Cosmos DB 101", "pub-id": "mspress"}
    {"id": "2","name": "Azure Cosmos DB for RDBMS Users", "pub-id": "mspress"}
    {"id": "3","name": "Taking over hello world one JSON doc at a time"}
    ...
    {"id": "100","name": "Learn about Azure Cosmos DB", "pub-id": "mspress"}
    ...
    {"id": "1000","name": "Deep Dive in tooAzure Cosmos DB", "pub-id": "mspress"}

A fenti példa hello, azt áthúzott hello hello publisher dokumentum unbounded gyűjtemény. Ehelyett csak kell a hivatkozás toohello közzétevő minden könyv-dokumentum.

### <a name="how-do-i-model-manymany-relationships"></a>Hogyan a több: többhöz kapcsolatok modell?
Egy relációs adatbázisban *több: több* kapcsolatok gyakran van modellezve a táblákat, amelyek csak csatlakozás együtt más táblákból származó rekordokat. 

![Táblák illesztése](./media/documentdb-modeling-data/join-table.png)

Előfordulhat, hogy kísértésbe tooreplicate hello azonos dolog használatával dokumentumokat, és előállít egy, a következőhöz hasonló toohello következő adatmodell.

    Author documents: 
    {"id": "a1", "name": "Thomas Andersen" }
    {"id": "a2", "name": "William Wakefield" }

    Book documents:
    {"id": "b1", "name": "Azure Cosmos DB 101" }
    {"id": "b2", "name": "Azure Cosmos DB for RDBMS Users" }
    {"id": "b3", "name": "Taking over hello world one JSON doc at a time" }
    {"id": "b4", "name": "Learn about Azure Cosmos DB" }
    {"id": "b5", "name": "Deep Dive in tooAzure Cosmos DB" }

    Joining documents: 
    {"authorId": "a1", "bookId": "b1" }
    {"authorId": "a2", "bookId": "b1" }
    {"authorId": "a1", "bookId": "b2" }
    {"authorId": "a1", "bookId": "b3" }

Ez akkor működik. Azonban betöltésekor vagy a szerző könyveiben rendelkező, vagy a könyv betölteni a szerző, a mindig igényelnének hello adatbázis legalább két további lekérdezéseket. Egy lekérdezés toohello és majd egy másik lekérdezés toofetch hello tényleges dokumentum alatt álló tartományhoz való csatlakozás. 

Ha az illesztés tábla csak akkor van kapcsolása együtt adatok két darab, miért nem előfordulásoknál hagyja el teljesen?
Vegye figyelembe a következőket hello.

    Author documents:
    {"id": "a1", "name": "Thomas Andersen", "books": ["b1, "b2", "b3"]}
    {"id": "a2", "name": "William Wakefield", "books": ["b1", "b4"]}

    Book documents: 
    {"id": "b1", "name": "Azure Cosmos DB 101", "authors": ["a1", "a2"]}
    {"id": "b2", "name": "Azure Cosmos DB for RDBMS Users", "authors": ["a1"]}
    {"id": "b3", "name": "Learn about Azure Cosmos DB", "authors": ["a1"]}
    {"id": "b4", "name": "Deep Dive in tooAzure Cosmos DB", "authors": ["a2"]}

Most ha egy szerző, azonnal tudható, hogy mely általa könyvek, és ezzel szemben ha betöltött könyv dokumentum akkor tudható, hogy hello szerzőjét vagy hello azonosítói. Ezzel a kiszolgáló hello számának csökkentése hello illesztési táblázaton közvetítő lekérdezés takaríthat meg az alkalmazás rendelkezik toomake kiszolgálókkal való adatváltások számát. 

## <a id="WrapUp"></a>Adatok a hibrid modellek
A Microsoft most kikeresi beágyazás (vagy denormalizing) és azok upsides rendelkező hivatkozó (vagy normalizálása) adatokat, és minden a biztonság sérüléseinek rendelkezik, és úgy találtuk. 

Azt nem mindig rendelkeznek toobe vagy, vagy nem Ijedt toomix dolgot még be kell. 

Az alkalmazás a konkrét használati mintákat és előfordulhat, hogy hol keverése beágyazott esetekben munkaterhelések alapján és hivatkozott adatok szabálykészletében, és sikerült átfutási toosimpler úgy az alkalmazáslogikát kevesebb kiszolgálóval kerekíteni való adatváltások számát továbbra is a megfelelő szintű teljesítmény megőrzése .

Vegye figyelembe a következő JSON hello. 

    Author documents: 
    {
        "id": "a1",
        "firstName": "Thomas",
        "lastName": "Andersen",        
        "countOfBooks": 3,
         "books": ["b1", "b2", "b3"],
        "images": [
            {"thumbnail": "http://....png"}
            {"profile": "http://....png"}
            {"large": "http://....png"}
        ]
    },
    {
        "id": "a2",
        "firstName": "William",
        "lastName": "Wakefield",
        "countOfBooks": 1,
        "books": ["b1"],
        "images": [
            {"thumbnail": "http://....png"}
        ]
    }

    Book documents:
    {
        "id": "b1",
        "name": "Azure Cosmos DB 101",
        "authors": [
            {"id": "a1", "name": "Thomas Andersen", "thumbnailUrl": "http://....png"},
            {"id": "a2", "name": "William Wakefield", "thumbnailUrl": "http://....png"}
        ]
    },
    {
        "id": "b2",
        "name": "Azure Cosmos DB for RDBMS Users",
        "authors": [
            {"id": "a1", "name": "Thomas Andersen", "thumbnailUrl": "http://....png"},
        ]
    }

Itt azt (főleg) követte hello beágyazott modellt, amikor más entitás adatait beágyazott hello legfelső szintű dokumentumban, de más adatok hivatkozik. 

Hello könyv-dokumentum tekinti meg, ha néhány láthatja érdekes mezők, ha úgy tekintünk, a szerzők hello tömb. Van egy *azonosító* mező, amelyben hello mező használjuk toorefer hátsó tooan Szerző dokumentum, a normalizált modell- kat majd szabványos eljárása is rendelkezik *neve* és *thumbnailUrl*. Azt sikerült már csak állapottal *azonosító* és hello alkalmazás tooget szükség további adatokat hello megfelelő Szerző dokumentumból használatával "link" hello, de mivel az alkalmazás hello Szerző neve és a minden könyv jelenik meg a miniatűr kép is mentheti egy könyv oda-vissza toohello kiszolgálónként listaként által denormalizing **néhány** hello szerzőjétől származó adatokat.

Ellenőrizze, ha hello Szerző neve megváltozott, vagy hogy fényképező kívánta tooupdate azt kellene toogo frissítést minden könyv legalább egyszer közzététel, de az alkalmazás, hello azt feltételezi, hogy szerzők ne nagyon gyakran, módosítsa a nevek alapján ez az egy elfogadható terv döntés.  

Hello példa nincsenek **összesítések előre számított** költséges olvasási művelet feldolgozási toosave értékeket. Hello példában hello Szerző dokumentumban beágyazott hello adatok egy részét az adatai, számított futásidőben. Egy könyv-dokumentum létrehozása minden alkalommal, amikor egy új könyv közzé van téve, **és** hello countOfBooks mező értéke, amely egy adott szerző léteznek könyv dokumentumok hello száma alapján számított tooa érték. Az optimalizálás lenne a helyes olvasási nehéz rendszerekben ahol azt megfizethető toodo számítások a rendelés toooptimize olvasások az írási műveletek.

előre számított mezők modell lehetséges legyen, mivel az Azure Cosmos DB támogatja képességét toohave hello **többdokumentumos tranzakció**. Sok NoSQL-tárolókon nem tranzakciók tegye a dokumentumok között, és ezért a tervezési döntéseit, például a "mindig beágyazásához mindent" toothis korlátozás miatt támogatják. Az Azure Cosmos DB kiszolgálóoldali eseményindítókat vagy tárolt eljárásokat, könyvek beszúrása és szerzők ACID tranzakción belül az összes frissítése is használhatja. Most nem **rendelkezik** tooembed mindent tooone a dokumentum csak toobe meg arról, hogy az adatok konzisztensek maradnak.

## <a name="NextSteps"></a>Következő lépések
hello legnagyobb takeaways a cikkből, hogy sémamentes világában modellezési adata éppen olyan fontos szerint bármikor toounderstand. 

Nem lehet egyetlen konkrét módszert toorepresent az adatok a képernyőn van, mint nincs nincs egyetlen konkrét módszert meghatározni toomodel adatait. Szükségesek toounderstand az alkalmazást, és hogyan eredményeznek, használnak, és hello adatok feldolgozása. Majd néhány hello alkalmazásával irányelvek jelenik itt állíthatja be modell, amely hello azonnali igényeket elégíti ki az alkalmazás létrehozása. Az alkalmazások toochange kell, amikor kihasználhatja a sémamentes adatbázis tooembrace hello rugalmasságát, módosítása és könnyen fejleszteni az adatmodellt. 

toolearn Azure Cosmos DB, kapcsolatos további információkért tekintse meg a toohello szolgáltatás [dokumentáció](https://azure.microsoft.com/documentation/services/cosmos-db/) lap. 

toounderstand hogyan tooshard az adatok így vannak elrendezve több partíciót, tekintse meg a túl[particionálás adatokat az Adatbázisba az Azure Cosmos](documentdb-partition-data.md). 
