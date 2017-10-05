---
title: "NoSQL-adatbázis dokumentum adatok modellezését |} Microsoft Docs"
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
ms.openlocfilehash: 16c387fe574243544cf54cf283c7713ddcaa1942
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="modeling-document-data-for-nosql-databases"></a><span data-ttu-id="d6821-104">Modellezési dokumentum adatok NoSQL-adatbázisok</span><span class="sxs-lookup"><span data-stu-id="d6821-104">Modeling document data for NoSQL databases</span></span>
<span data-ttu-id="d6821-105">Amíg a sémamentes adatbázisok, például Azure Cosmos DB, könnyebben super könnyen vezessék be a módosításokat az adatmodellbe kell továbbra is töltött bizonyos idő számbavétele szolgál az adatokról.</span><span class="sxs-lookup"><span data-stu-id="d6821-105">While schema-free databases, like Azure Cosmos DB, make it super easy to embrace changes to your data model you should still spend some time thinking about your data.</span></span> 

<span data-ttu-id="d6821-106">Hogyan adatokat fog tárolni?</span><span class="sxs-lookup"><span data-stu-id="d6821-106">How is data going to be stored?</span></span> <span data-ttu-id="d6821-107">Hogyan lesz beolvasásához és kérdezhet le adatokat az alkalmazást?</span><span class="sxs-lookup"><span data-stu-id="d6821-107">How is your application going to retrieve and query data?</span></span> <span data-ttu-id="d6821-108">Az a alkalmazás vastag olvasási vagy írási nehéz?</span><span class="sxs-lookup"><span data-stu-id="d6821-108">Is your application read heavy, or write heavy?</span></span> 

<span data-ttu-id="d6821-109">A cikk elolvasása után lesz a következő kérdések megválaszolásához:</span><span class="sxs-lookup"><span data-stu-id="d6821-109">After reading this article, you will be able to answer the following questions:</span></span>

* <span data-ttu-id="d6821-110">Hogyan kell egy dokumentumot egy dokumentum-adatbázisban fontolnunk?</span><span class="sxs-lookup"><span data-stu-id="d6821-110">How should I think about a document in a document database?</span></span>
* <span data-ttu-id="d6821-111">Mi az adatok modellezési, és miért érdemes I fontos?</span><span class="sxs-lookup"><span data-stu-id="d6821-111">What is data modeling and why should I care?</span></span> 
* <span data-ttu-id="d6821-112">Hogyan különbözik modellezési adatok dokumentum-adatbázisban és a relációs?</span><span class="sxs-lookup"><span data-stu-id="d6821-112">How is modeling data in a document database different to a relational database?</span></span>
* <span data-ttu-id="d6821-113">Hogyan express a nem relációs adatbázis adatok kapcsolatokat?</span><span class="sxs-lookup"><span data-stu-id="d6821-113">How do I express data relationships in a non-relational database?</span></span>
* <span data-ttu-id="d6821-114">Ha adatok beágyazása, és ha hivatkozás adatokhoz?</span><span class="sxs-lookup"><span data-stu-id="d6821-114">When do I embed data and when do I link to data?</span></span>

## <a name="embedding-data"></a><span data-ttu-id="d6821-115">Adatok beágyazása</span><span class="sxs-lookup"><span data-stu-id="d6821-115">Embedding data</span></span>
<span data-ttu-id="d6821-116">Amikor elkezdi az adatok Azure Cosmos DB, például egy dokumentum áruházban modellezését megpróbálja az entitások tekinti **önálló dokumentumok** JSON jelöli.</span><span class="sxs-lookup"><span data-stu-id="d6821-116">When you start modeling data in a document store, such as Azure Cosmos DB, try to treat your entities as **self-contained documents** represented in JSON.</span></span>

<span data-ttu-id="d6821-117">Ahhoz, hogy férhet hozzá túl sokkal tovább, ossza meg velünk néhány lépésekkel vissza és tekintse meg a következő hogyan azt előfordulhat, hogy a modell egy relációs adatbázisban, velünk számos már ismeri a tárgy valamit.</span><span class="sxs-lookup"><span data-stu-id="d6821-117">Before we dive in too much further, let us take a few steps back and have a look at how we might model something in a relational database, a subject many of us are already familiar with.</span></span> <span data-ttu-id="d6821-118">A következő példa bemutatja, hogyan tárolódhat egy személy egy relációs adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="d6821-118">The following example shows how a person might be stored in a relational database.</span></span> 

![Relációs adatbázis-modell](./media/documentdb-modeling-data/relational-data-model.png)

<span data-ttu-id="d6821-120">Relációs adatbázisok használata, ha azt korábban lett tanított évig optimalizálására, optimalizálására, optimalizálására.</span><span class="sxs-lookup"><span data-stu-id="d6821-120">When working with relational databases, we've been taught for years to normalize, normalize, normalize.</span></span>

<span data-ttu-id="d6821-121">Az adatokat általában normalizálása magában foglalja a entitás, például egy személy véve, és bontásához, akkor különálló darabokra adatok.</span><span class="sxs-lookup"><span data-stu-id="d6821-121">Normalizing your data typically involves taking an entity, such as a person, and breaking it down in to discrete pieces of data.</span></span> <span data-ttu-id="d6821-122">A fenti példában egy személy rendelkezhet több kapcsolattartási részletes rekordok, valamint több címadatokat.</span><span class="sxs-lookup"><span data-stu-id="d6821-122">In the example above, a person can have multiple contact detail records as well as multiple address records.</span></span> <span data-ttu-id="d6821-123">Azt még egy lépéssel további, és kapcsolattartási adatai szerint lebontva további kibontása közös mezők, például egy típust.</span><span class="sxs-lookup"><span data-stu-id="d6821-123">We even go one step further and break down contact details by further extracting common fields like a type.</span></span> <span data-ttu-id="d6821-124">Ugyanazt a címet, itt rekordokban típussal rendelkezik például *Home* vagy *üzleti*</span><span class="sxs-lookup"><span data-stu-id="d6821-124">Same for address, each record here has a type like *Home* or *Business*</span></span> 

<span data-ttu-id="d6821-125">A irányítsa helyi, amikor adatokat normalizálása **ne tárolja a redundáns adatok** minden egyes jegyezze fel, és inkább adatokra hivatkoznak.</span><span class="sxs-lookup"><span data-stu-id="d6821-125">The guiding premise when normalizing data is to **avoid storing redundant data** on each record and rather refer to data.</span></span> <span data-ttu-id="d6821-126">Ebben a példában olvasni egy személy, a kapcsolattartási adatait és a címek, szükség hatékonyan összesítésére az adatokat a futási időben a joins ZÁRADÉKOT használják.</span><span class="sxs-lookup"><span data-stu-id="d6821-126">In this example, to read a person, with all their contact details and addresses, you need to use JOINS to effectively aggregate your data at run time.</span></span>

    SELECT p.FirstName, p.LastName, a.City, cd.Detail
    FROM Person p
    JOIN ContactDetail cd ON cd.PersonId = p.Id
    JOIN ContactDetailType on cdt ON cdt.Id = cd.TypeId
    JOIN Address a ON a.PersonId = p.Id

<span data-ttu-id="d6821-127">Az írási műveletek egyetlen frissítése a kapcsolattartási adatait és a címek igényel sok egyes táblák között.</span><span class="sxs-lookup"><span data-stu-id="d6821-127">Updating a single person with their contact details and addresses requires write operations across many individual tables.</span></span> 

<span data-ttu-id="d6821-128">Most vessen egy pillantást hogyan azt kellene modell ugyanazokat az adatokat egy önálló egységként dokumentum-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="d6821-128">Now let's take a look at how we would model the same data as a self-contained entity in a document database.</span></span>

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

<span data-ttu-id="d6821-129">Most már tudunk a fenti megközelítéssel **denormalizált** személy where rögzítse azt **beágyazott** a egyetlen JSON-dokumentumában, például a kapcsolattartási adatai és a címek, ennek a személynek kapcsolatos összes információt.</span><span class="sxs-lookup"><span data-stu-id="d6821-129">Using the approach above we have now **denormalized** the person record where we **embedded** all the information relating to this person, such as their contact details and addresses, in to a single JSON document.</span></span>
<span data-ttu-id="d6821-130">Ezenkívül mivel azt még nem korlátozódik a rögzített sémájába tudunk műveleteket, például a kapcsolattartási adatait különböző alakzatok teljesen rendelkező rugalmasságot.</span><span class="sxs-lookup"><span data-stu-id="d6821-130">In addition, because we're not confined to a fixed schema we have the flexibility to do things like having contact details of different shapes entirely.</span></span> 

<span data-ttu-id="d6821-131">A teljes személy rekord lekérése az adatbázis már olvasási művelete egyetlen gyűjtemény ellen, és egyetlen dokumentum egyetlen.</span><span class="sxs-lookup"><span data-stu-id="d6821-131">Retrieving a complete person record from the database is now a single read operation against a single collection and for a single document.</span></span> <span data-ttu-id="d6821-132">Frissítés személy rekord, a kapcsolattartási adatait és a címet, akkor is egyetlen dokumentum szemben egyetlen írási művelet.</span><span class="sxs-lookup"><span data-stu-id="d6821-132">Updating a person record, with their contact details and addresses, is also a single write operation against a single document.</span></span>

<span data-ttu-id="d6821-133">Által denormalizing adatok, az alkalmazás esetleg kevesebb lekérdezések és gyakori műveleteinek elvégzéséhez frissítések.</span><span class="sxs-lookup"><span data-stu-id="d6821-133">By denormalizing data, your application may need to issue fewer queries and updates to complete common operations.</span></span> 

### <a name="when-to-embed"></a><span data-ttu-id="d6821-134">Mikor érdemes beágyazása</span><span class="sxs-lookup"><span data-stu-id="d6821-134">When to embed</span></span>
<span data-ttu-id="d6821-135">Általában a beágyazott adatok használata esetén a modellek:</span><span class="sxs-lookup"><span data-stu-id="d6821-135">In general, use embedded data models when:</span></span>

* <span data-ttu-id="d6821-136">Nincsenek **tartalmaz** entitások közötti kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="d6821-136">There are **contains** relationships between entities.</span></span>
* <span data-ttu-id="d6821-137">Nincsenek **egy néhány** entitások közötti kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="d6821-137">There are **one-to-few** relationships between entities.</span></span>
* <span data-ttu-id="d6821-138">Beágyazott adat, amely **ritkán változó**.</span><span class="sxs-lookup"><span data-stu-id="d6821-138">There is embedded data that **changes infrequently**.</span></span>
* <span data-ttu-id="d6821-139">Nincs beágyazott adatok nem nő **nélkül kötött**.</span><span class="sxs-lookup"><span data-stu-id="d6821-139">There is embedded data won't grow **without bound**.</span></span>
* <span data-ttu-id="d6821-140">Nincs a beágyazott adatok **szerves** a dokumentumban szereplő adatokat.</span><span class="sxs-lookup"><span data-stu-id="d6821-140">There is embedded data that is **integral** to data in a document.</span></span>

> [!NOTE]
> <span data-ttu-id="d6821-141">Általában a modellek biztosítják jobban adatok denormalizált **olvasási** teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="d6821-141">Typically denormalized data models provide better **read** performance.</span></span>
> 
> 

### <a name="when-not-to-embed"></a><span data-ttu-id="d6821-142">Mikor érdemes Nincs beágyazás</span><span class="sxs-lookup"><span data-stu-id="d6821-142">When not to embed</span></span>
<span data-ttu-id="d6821-143">Dokumentum-adatbázisban a tapasztalatok denormalize mindent, és egyetlen dokumentum lévő összes adatot beágyazni pedig ez bizonyos helyzetekben, el kell kerülni is járhat.</span><span class="sxs-lookup"><span data-stu-id="d6821-143">While the rule of thumb in a document database is to denormalize everything and embed all data in to a single document, this can lead to some situations that should be avoided.</span></span>

<span data-ttu-id="d6821-144">A JSON-részlet igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="d6821-144">Take this JSON snippet.</span></span>

    {
        "id": "1",
        "name": "What's new in the coolest Cloud",
        "summary": "A blog post by someone real famous",
        "comments": [
            {"id": 1, "author": "anon", "comment": "something useful, I'm sure"},
            {"id": 2, "author": "bob", "comment": "wisdom from the interwebs"},
            …
            {"id": 100001, "author": "jane", "comment": "and on we go ..."},
            …
            {"id": 1000000001, "author": "angry", "comment": "blah angry blah angry"},
            …
            {"id": ∞ + 1, "author": "bored", "comment": "oh man, will this ever end?"},
        ]
    }

<span data-ttu-id="d6821-145">Ez akkor lehet, hogy mi a feladás egy vagy több entitás beágyazott megjegyzésekkel néz azt modellezési volt egy tipikus blogba vagy CMS, a rendszer.</span><span class="sxs-lookup"><span data-stu-id="d6821-145">This might be what a post entity with embedded comments would look like if we were modeling a typical blog, or CMS, system.</span></span> <span data-ttu-id="d6821-146">Ebben a példában a probléma az, hogy a megjegyzések tömb **unbounded**, ami azt jelenti, hogy nincs-e bármilyen egyetlen post rendelkezhet megjegyzések számával (gyakorlati) korlát.</span><span class="sxs-lookup"><span data-stu-id="d6821-146">The problem with this example is that the comments array is **unbounded**, meaning that there is no (practical) limit to the number of comments any single post can have.</span></span> <span data-ttu-id="d6821-147">Ez válik, hogy a program probléma, mivel a dokumentum méretének sikerült jelentősen megnő.</span><span class="sxs-lookup"><span data-stu-id="d6821-147">This will become a problem as the size of the document could grow significantly.</span></span>

<span data-ttu-id="d6821-148">Ez a dokumentum méretének növekedésével képes továbbítani az adatokat a hálózaton, valamint olvasása és frissítése a dokumentum léptékű keresztül befolyásolja.</span><span class="sxs-lookup"><span data-stu-id="d6821-148">As the size of the document grows the ability to transmit the data over the wire as well as reading and updating the document, at scale, will be impacted.</span></span>

<span data-ttu-id="d6821-149">Ebben az esetben lenne érdekében fontolja meg a következő modell.</span><span class="sxs-lookup"><span data-stu-id="d6821-149">In this case it would be better to consider the following model.</span></span>

    Post document:
    {
        "id": "1",
        "name": "What's new in the coolest Cloud",
        "summary": "A blog post by someone real famous",
        "recentComments": [
            {"id": 1, "author": "anon", "comment": "something useful, I'm sure"},
            {"id": 2, "author": "bob", "comment": "wisdom from the interwebs"},
            {"id": 3, "author": "jane", "comment": "....."}
        ]
    }

    Comment documents:
    {
        "postId": "1"
        "comments": [
            {"id": 4, "author": "anon", "comment": "more goodness"},
            {"id": 5, "author": "bob", "comment": "tails from the field"},
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

<span data-ttu-id="d6821-150">A modellnek az utolsó három megjegyzéseit a feladás egy vagy több saját magát, amely egy tömböt egy rögzített a beágyazott kötött most.</span><span class="sxs-lookup"><span data-stu-id="d6821-150">This model has the three most recent comments embedded on the post itself, which is an array with a fixed bound this time.</span></span> <span data-ttu-id="d6821-151">A más megjegyzések a 100 megjegyzések kötegekben szerint vannak csoportosítva, és külön dokumentumokban tárolt.</span><span class="sxs-lookup"><span data-stu-id="d6821-151">The other comments are grouped in to batches of 100 comments and stored in separate documents.</span></span> <span data-ttu-id="d6821-152">A Köteg mérete 100-nak lett választva, mert a fiktív alkalmazás lehetővé teszi, hogy a felhasználó egyszerre csak 100 megjegyzések betöltése.</span><span class="sxs-lookup"><span data-stu-id="d6821-152">The size of the batch was chosen as 100 because our fictitious application allows the user to load 100 comments at a time.</span></span>  

<span data-ttu-id="d6821-153">Egy másik esetet, amikor beágyazási adatok nem célszerű akkor, ha a beágyazott adatok gyakran használt dokumentumok között, és gyakran változik.</span><span class="sxs-lookup"><span data-stu-id="d6821-153">Another case where embedding data is not a good idea is when the embedded data is used often across documents and will change frequently.</span></span> 

<span data-ttu-id="d6821-154">A JSON-részlet igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="d6821-154">Take this JSON snippet.</span></span>

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

<span data-ttu-id="d6821-155">Ez lehet egy személy készlet portfóliót.</span><span class="sxs-lookup"><span data-stu-id="d6821-155">This could represent a person's stock portfolio.</span></span> <span data-ttu-id="d6821-156">Azt választotta, az készlet minden egyes portfóliót dokumentumhoz információk beágyazható.</span><span class="sxs-lookup"><span data-stu-id="d6821-156">We have chosen to embed the stock information in to each portfolio document.</span></span> <span data-ttu-id="d6821-157">Olyan környezetben, ahol kapcsolódó adatok gyakran változnak például egy készlet kereskedelmipartner-alkalmazások, adatok gyakran változnak beágyazás érintetlen jelenti azt, hogy folyamatosan frissítjük az egyes portfóliót dokumentum minden alkalommal, amikor egy készlet forog.</span><span class="sxs-lookup"><span data-stu-id="d6821-157">In an environment where related data is changing frequently, like a stock trading application, embedding data that changes frequently is going to mean that you are constantly updating each portfolio document every time a stock is traded.</span></span>

<span data-ttu-id="d6821-158">Készlet *zaza* egyetlen alkalommal több száz kerülhetnek rendelkezhetnek a nap és a felhasználók ezreit *zaza* a saját portfóliót.</span><span class="sxs-lookup"><span data-stu-id="d6821-158">Stock *zaza* may be traded many hundreds of times in a single day and thousands of users could have *zaza* on their portfolio.</span></span> <span data-ttu-id="d6821-159">Például a fenti azt kell frissíteni a sok ezer portfóliót dokumentumok sokszor adatok modell vezet, hogy a rendszer minden nap, amely nem nagyon jól méretezhető.</span><span class="sxs-lookup"><span data-stu-id="d6821-159">With a data model like the above we would have to update many thousands of portfolio documents many times every day leading to a system that won't scale very well.</span></span> 

## <span data-ttu-id="d6821-160"><a id="Refer"></a>Adatok hivatkozik</span><span class="sxs-lookup"><span data-stu-id="d6821-160"><a id="Refer"></a>Referencing data</span></span>
<span data-ttu-id="d6821-161">Igen sok esetben szépen adatok beágyazása működik, de egyértelmű, hogy nincsenek helyzetek, amikor az adatok denormalizing mint érdemes további problémákat okozhat.</span><span class="sxs-lookup"><span data-stu-id="d6821-161">So, embedding data works nicely for many cases but it is clear that there are scenarios when denormalizing your data will cause more problems than it is worth.</span></span> <span data-ttu-id="d6821-162">Ezért Mi a teendő most?</span><span class="sxs-lookup"><span data-stu-id="d6821-162">So what do we do now?</span></span> 

<span data-ttu-id="d6821-163">Relációs adatbázisok csak az egyetlen hely, ahol létrehozhat entitások közötti kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="d6821-163">Relational databases are not the only place where you can create relationships between entities.</span></span> <span data-ttu-id="d6821-164">A dokumentum-adatbázisban egy dokumentumot, hogy ténylegesen vonatkozik, más dokumentumban adatok információkat lehet.</span><span class="sxs-lookup"><span data-stu-id="d6821-164">In a document database you can have information in one document that actually relates to data in other documents.</span></span> <span data-ttu-id="d6821-165">Most I vagyok nem javasolni egy percig, még akkor is, hogy azt a rendszerek, akkor lehet jobban megfelel az Azure Cosmos Adatbázisba egy relációs adatbázisban, vagy más dokumentum-adatbázis létrehozása, de egyszerű kapcsolatok rendben, és nagyon hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="d6821-165">Now, I am not advocating for even one minute that we build systems that would be better suited to a relational database in Azure Cosmos DB, or any other document database, but simple relationships are fine and can be very useful.</span></span> 

<span data-ttu-id="d6821-166">Az alábbi JSON korábbi kívánja használni a példa egy készlet portfóliót a választottuk, de ezúttal hivatkozunk beágyazás helyett portfóliót készlet elemére.</span><span class="sxs-lookup"><span data-stu-id="d6821-166">In the JSON below we chose to use the example of a stock portfolio from earlier but this time we refer to the stock item on the portfolio instead of embedding it.</span></span> <span data-ttu-id="d6821-167">Így, ha a készlet elem gyakran megváltoznak egy nap a csak dokumentumot, frissíteni kell a készlet egyetlen dokumentum.</span><span class="sxs-lookup"><span data-stu-id="d6821-167">This way, when the stock item changes frequently throughout the day the only document that needs to be updated is the single stock document.</span></span> 

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


<span data-ttu-id="d6821-168">Egy azonnali hátránya, hogy ezt a módszert használja, ha szükség minden olyan személy portfóliót; megjelenítésekor tartott készlet információkat jelenít meg az alkalmazás esetén Ebben az esetben kellene több utazgatással legyen az adatbázis betölti az információt a rendszer dokumentumok.</span><span class="sxs-lookup"><span data-stu-id="d6821-168">An immediate downside to this approach though is if your application is required to show information about each stock that is held when displaying a person's portfolio; in this case you would need to make multiple trips to the database to load the information for each stock document.</span></span> <span data-ttu-id="d6821-169">Itt hajtottunk írási műveleteket, amelyek gyakran ismétlődnek a nap folyamán fordulhat elő, de az az olvasási műveletekre, amelyek esetleg kevesebb hatással vannak az adott rendszer teljesítményét pedig sérült hatékonyságának növelése érdekében döntést hoznak.</span><span class="sxs-lookup"><span data-stu-id="d6821-169">Here we've made a decision to improve the efficiency of write operations, which happen frequently throughout the day, but in turn compromised on the read operations that potentially have less impact on the performance of this particular system.</span></span>

> [!NOTE]
> <span data-ttu-id="d6821-170">Normalizált adatmodellekben **megkövetelheti további kiszolgálókkal való adatváltások számát** a kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="d6821-170">Normalized data models **can require more round trips** to the server.</span></span>
> 
> 

### <a name="what-about-foreign-keys"></a><span data-ttu-id="d6821-171">Mi a helyzet a külső kulcsokat?</span><span class="sxs-lookup"><span data-stu-id="d6821-171">What about foreign keys?</span></span>
<span data-ttu-id="d6821-172">Nincs jelenleg egy megkötés, mert külső kulcsok vagy egyéb, rendelkezésre álló dokumentumok közötti dokumentum kapcsolat hatékony "gyenge links" és maga az adatbázis nem ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="d6821-172">Because there is currently no concept of a constraint, foreign-key or otherwise, any inter-document relationships that you have in documents are effectively "weak links" and will not be verified by the database itself.</span></span> <span data-ttu-id="d6821-173">Ha azt szeretné, annak érdekében, hogy az adatok egy dokumentum hivatkozik valóban létezik, akkor szüksége ehhez az alkalmazás vagy kiszolgálóoldali eseményindítók és tárolt eljárások az Azure Cosmos-adatbázis használatával.</span><span class="sxs-lookup"><span data-stu-id="d6821-173">If you want to ensure that the data a document is referring to actually exists, then you need to do this in your application, or through the use of server-side triggers or stored procedures on Azure Cosmos DB.</span></span>

### <a name="when-to-reference"></a><span data-ttu-id="d6821-174">Mikor kell hivatkoznia</span><span class="sxs-lookup"><span data-stu-id="d6821-174">When to reference</span></span>
<span data-ttu-id="d6821-175">Általában a normalizált adatok használata esetén a modellek:</span><span class="sxs-lookup"><span data-stu-id="d6821-175">In general, use normalized data models when:</span></span>

* <span data-ttu-id="d6821-176">Képviselő **egy-a-többhöz** kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="d6821-176">Representing **one-to-many** relationships.</span></span>
* <span data-ttu-id="d6821-177">Képviselő **több-a-többhöz** kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="d6821-177">Representing **many-to-many** relationships.</span></span>
* <span data-ttu-id="d6821-178">A kapcsolódó adatok **változik gyakran**.</span><span class="sxs-lookup"><span data-stu-id="d6821-178">Related data **changes frequently**.</span></span>
* <span data-ttu-id="d6821-179">Hivatkozásban szereplő lehet **unbounded**.</span><span class="sxs-lookup"><span data-stu-id="d6821-179">Referenced data could be **unbounded**.</span></span>

> [!NOTE]
> <span data-ttu-id="d6821-180">Általában normalizálása jobban biztosít **írási** teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="d6821-180">Typically normalizing provides better **write** performance.</span></span>
> 
> 

### <a name="where-do-i-put-the-relationship"></a><span data-ttu-id="d6821-181">Ha helyezze a kapcsolatot?</span><span class="sxs-lookup"><span data-stu-id="d6821-181">Where do I put the relationship?</span></span>
<span data-ttu-id="d6821-182">A kapcsolat a növekedési segítségével megállapíthatja, hogy mely dokumentumban a hivatkozás tárolásához.</span><span class="sxs-lookup"><span data-stu-id="d6821-182">The growth of the relationship will help determine in which document to store the reference.</span></span>

<span data-ttu-id="d6821-183">Ha úgy tekintünk, a JSON, gyártók és -könyvekkel modellek.</span><span class="sxs-lookup"><span data-stu-id="d6821-183">If we look at the JSON below that models publishers and books.</span></span>

    Publisher document:
    {
        "id": "mspress",
        "name": "Microsoft Press",
        "books": [ 1, 2, 3, ..., 100, ..., 1000]
    }

    Book documents:
    {"id": "1", "name": "Azure Cosmos DB 101" }
    {"id": "2", "name": "Azure Cosmos DB for RDBMS Users" }
    {"id": "3", "name": "Taking over the world one JSON doc at a time" }
    ...
    {"id": "100", "name": "Learn about Azure Cosmos DB" }
    ...
    {"id": "1000", "name": "Deep Dive in to Azure Cosmos DB" }

<span data-ttu-id="d6821-184">Ha egy publisher könyvek száma korlátozott növekedési rendelkező kicsi, majd tárolja a könyv hivatkozás a közzétevő dokumentumban lévő lehetnek hasznosak.</span><span class="sxs-lookup"><span data-stu-id="d6821-184">If the number of the books per publisher is small with limited growth, then storing the book reference inside the publisher document may be useful.</span></span> <span data-ttu-id="d6821-185">Azonban ha könyvek publisher másodpercenkénti száma unbounded, majd az adatmodell vezetne változtatható, növekvő tömbök, ahogy a fenti példa publisher dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="d6821-185">However, if the number of books per publisher is unbounded, then this data model would lead to mutable, growing arrays, as in the example publisher document above.</span></span> 

<span data-ttu-id="d6821-186">Váltás körül bit dolgot modell, amely ugyanazokat az adatokat továbbra is jelenti, de most elkerülhetők a nagy változtatható gyűjtemények eredményezne.</span><span class="sxs-lookup"><span data-stu-id="d6821-186">Switching things around a bit would result in a model that still represents the same data but now avoids these large mutable collections.</span></span>

    Publisher document: 
    {
        "id": "mspress",
        "name": "Microsoft Press"
    }

    Book documents: 
    {"id": "1","name": "Azure Cosmos DB 101", "pub-id": "mspress"}
    {"id": "2","name": "Azure Cosmos DB for RDBMS Users", "pub-id": "mspress"}
    {"id": "3","name": "Taking over the world one JSON doc at a time"}
    ...
    {"id": "100","name": "Learn about Azure Cosmos DB", "pub-id": "mspress"}
    ...
    {"id": "1000","name": "Deep Dive in to Azure Cosmos DB", "pub-id": "mspress"}

<span data-ttu-id="d6821-187">A fenti példában a unbounded gyűjtemény csökkentek azt a közzétevő dokumentum.</span><span class="sxs-lookup"><span data-stu-id="d6821-187">In the above example, we have dropped the unbounded collection on the publisher document.</span></span> <span data-ttu-id="d6821-188">Ehelyett csak van egy minden könyv-dokumentum publisher mutató hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="d6821-188">Instead we just have a a reference to the publisher on each book document.</span></span>

### <a name="how-do-i-model-manymany-relationships"></a><span data-ttu-id="d6821-189">Hogyan a több: többhöz kapcsolatok modell?</span><span class="sxs-lookup"><span data-stu-id="d6821-189">How do I model many:many relationships?</span></span>
<span data-ttu-id="d6821-190">Egy relációs adatbázisban *több: több* kapcsolatok gyakran van modellezve a táblákat, amelyek csak csatlakozás együtt más táblákból származó rekordokat.</span><span class="sxs-lookup"><span data-stu-id="d6821-190">In a relational database *many:many* relationships are often modeled with join tables, which just join records from other tables together.</span></span> 

![Táblák illesztése](./media/documentdb-modeling-data/join-table.png)

<span data-ttu-id="d6821-192">Előfordulhat, hogy ugyanazt a dokumentumok használatával replikálja, és előállít egy adatmodell, az alábbihoz hasonló kísértésbe.</span><span class="sxs-lookup"><span data-stu-id="d6821-192">You might be tempted to replicate the same thing using documents and produce a data model that looks similar to the following.</span></span>

    Author documents: 
    {"id": "a1", "name": "Thomas Andersen" }
    {"id": "a2", "name": "William Wakefield" }

    Book documents:
    {"id": "b1", "name": "Azure Cosmos DB 101" }
    {"id": "b2", "name": "Azure Cosmos DB for RDBMS Users" }
    {"id": "b3", "name": "Taking over the world one JSON doc at a time" }
    {"id": "b4", "name": "Learn about Azure Cosmos DB" }
    {"id": "b5", "name": "Deep Dive in to Azure Cosmos DB" }

    Joining documents: 
    {"authorId": "a1", "bookId": "b1" }
    {"authorId": "a2", "bookId": "b1" }
    {"authorId": "a1", "bookId": "b2" }
    {"authorId": "a1", "bookId": "b3" }

<span data-ttu-id="d6821-193">Ez akkor működik.</span><span class="sxs-lookup"><span data-stu-id="d6821-193">This would work.</span></span> <span data-ttu-id="d6821-194">Azonban betöltésekor vagy a szerző könyveiben rendelkező, vagy a könyv betölteni a szerző, a mindig igényelnének legalább két további lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="d6821-194">However, loading either an author with their books, or loading a book with its author, would always require at least two additional queries against the database.</span></span> <span data-ttu-id="d6821-195">Egy lekérdezést a csatlakozó dokumentumot és a csatlakoztatni kívánt dokumentum beolvasási majd egy másik lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="d6821-195">One query to the joining document and then another query to fetch the actual document being joined.</span></span> 

<span data-ttu-id="d6821-196">Ha az illesztés tábla csak akkor van kapcsolása együtt adatok két darab, miért nem előfordulásoknál hagyja el teljesen?</span><span class="sxs-lookup"><span data-stu-id="d6821-196">If all this join table is doing is gluing together two pieces of data, then why not drop it completely?</span></span>
<span data-ttu-id="d6821-197">Vegye figyelembe a következőket.</span><span class="sxs-lookup"><span data-stu-id="d6821-197">Consider the following.</span></span>

    Author documents:
    {"id": "a1", "name": "Thomas Andersen", "books": ["b1, "b2", "b3"]}
    {"id": "a2", "name": "William Wakefield", "books": ["b1", "b4"]}

    Book documents: 
    {"id": "b1", "name": "Azure Cosmos DB 101", "authors": ["a1", "a2"]}
    {"id": "b2", "name": "Azure Cosmos DB for RDBMS Users", "authors": ["a1"]}
    {"id": "b3", "name": "Learn about Azure Cosmos DB", "authors": ["a1"]}
    {"id": "b4", "name": "Deep Dive in to Azure Cosmos DB", "authors": ["a2"]}

<span data-ttu-id="d6821-198">Mostantól Ha egy szerző, azonnal tudható, hogy mely általa könyvek, és ezzel szemben ha betöltött könyv dokumentum le kellett volna tudható, hogy szerzőjét vagy azonosítóit.</span><span class="sxs-lookup"><span data-stu-id="d6821-198">Now, if I had an author, I immediately know which books they have written, and conversely if I had a book document loaded I would know the ids of the author(s).</span></span> <span data-ttu-id="d6821-199">Ez menti a köztes irányuló lekérdezés az illesztési tábla csökkentése kiszolgáló szám kerekítése való adatváltások számát, hogy rendelkezik az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d6821-199">This saves that intermediary query against the join table reducing the number of server round trips your application has to make.</span></span> 

## <span data-ttu-id="d6821-200"><a id="WrapUp"></a>Adatok a hibrid modellek</span><span class="sxs-lookup"><span data-stu-id="d6821-200"><a id="WrapUp"></a>Hybrid data models</span></span>
<span data-ttu-id="d6821-201">A Microsoft most kikeresi beágyazás (vagy denormalizing) és azok upsides rendelkező hivatkozó (vagy normalizálása) adatokat, és minden a biztonság sérüléseinek rendelkezik, és úgy találtuk.</span><span class="sxs-lookup"><span data-stu-id="d6821-201">We've now looked embedding (or denormalizing) and referencing (or normalizing) data, each have their upsides and each have compromises as we have seen.</span></span> 

<span data-ttu-id="d6821-202">Mindig kell lennie, vagy nem rendelkezik vagy nem lehet összekeveri dolgot még a Ijedt.</span><span class="sxs-lookup"><span data-stu-id="d6821-202">It doesn't always have to be either or, don't be scared to mix things up a little.</span></span> 

<span data-ttu-id="d6821-203">Az alkalmazás a konkrét használati mintákat és előfordulhat, hogy hol keverése beágyazott esetekben munkaterhelések alapján és hivatkozott adatok szabálykészletében, és sikerült kevesebb kiszolgálóval egyszerűbb alkalmazáslogikát vezethet kerekíteni való adatváltások számát továbbra is a megfelelő szintű teljesítmény megőrzése.</span><span class="sxs-lookup"><span data-stu-id="d6821-203">Based on your application's specific usage patterns and workloads there may be cases where mixing embedded and referenced data makes sense and could lead to simpler application logic with fewer server round trips while still maintaining a good level of performance.</span></span>

<span data-ttu-id="d6821-204">Vegye figyelembe a következő JSON.</span><span class="sxs-lookup"><span data-stu-id="d6821-204">Consider the following JSON.</span></span> 

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

<span data-ttu-id="d6821-205">Itt azt (főleg) követte a beágyazott modellt, amikor más entitás adatait a legfelső szintű dokumentumban található beágyazva, de más adatok hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="d6821-205">Here we've (mostly) followed the embedded model, where data from other entities are embedded in the top-level document, but other data is referenced.</span></span> 

<span data-ttu-id="d6821-206">A címjegyzék dokumentum tekinti meg, ha néhány láthatja érdekes mezők, ha úgy tekintünk, a szerzők tömbje.</span><span class="sxs-lookup"><span data-stu-id="d6821-206">If you look at the book document, we can see a few interesting fields when we look at the array of authors.</span></span> <span data-ttu-id="d6821-207">Van egy *azonosító* mezőben, amely körkörösen a szerző dokumentumhoz, általános gyakorlat egy normalizált modell használatával mező, de akkor azt is meg kell *neve* és *thumbnailUrl*.</span><span class="sxs-lookup"><span data-stu-id="d6821-207">There is an *id* field which is the field we use to refer back to an author document, standard practice in a normalized model, but then we also have *name* and *thumbnailUrl*.</span></span> <span data-ttu-id="d6821-208">Azt sikerült már csak állapottal *azonosító* az alkalmazások azokat az adatokat, és azt használja a "link" megfelelő Szerző-dokumentumból szükséges, de mivel az alkalmazás megjelenik a szerző nevét és a miniatűr kép jelenik meg minden könyv azt menteni tudja oda-vissza listaként könyv kiszolgálónként denormalizing által **néhány** a szerző adatait.</span><span class="sxs-lookup"><span data-stu-id="d6821-208">We could've just stuck with *id* and left the application to get any additional information it needed from the respective author document using the "link", but because our application displays the author's name and a thumbnail picture with every book displayed we can save a round trip to the server per book in a list by denormalizing **some** data from the author.</span></span>

<span data-ttu-id="d6821-209">Biztos Ha a szerző neve megváltozott, vagy frissítse a fénykép azt volna meg kell nyitnia egy frissítés végrehajthat minden könyv legalább egyszer közzététel, de az alkalmazás azt feltételezi, hogy szerzők nem módosul a nevek túl gyakran, ez pedig egy elfogadható tervezési döntés a.</span><span class="sxs-lookup"><span data-stu-id="d6821-209">Sure, if the author's name changed or they wanted to update their photo we'd have to go an update every book they ever published but for our application, based on the assumption that authors don't change their names very often, this is an acceptable design decision.</span></span>  

<span data-ttu-id="d6821-210">A példa nincsenek **összesítések előre számított** értékek költséges feldolgozási menti az olvasási művelet.</span><span class="sxs-lookup"><span data-stu-id="d6821-210">In the example there are **pre-calculated aggregates** values to save expensive processing on a read operation.</span></span> <span data-ttu-id="d6821-211">A példában a szerző dokumentumban a beágyazott adatok egy részét az adatai, számított futásidőben.</span><span class="sxs-lookup"><span data-stu-id="d6821-211">In the example, some of the data embedded in the author document is data that is calculated at run-time.</span></span> <span data-ttu-id="d6821-212">Egy könyv-dokumentum létrehozása minden alkalommal, amikor egy új könyv közzé van téve, **és** countOfBooks mező egy adott szerző tartozó címjegyzék dokumentumok száma alapján számított értékre van beállítva.</span><span class="sxs-lookup"><span data-stu-id="d6821-212">Every time a new book is published, a book document is created **and** the countOfBooks field is set to a calculated value based on the number of book documents that exist for a particular author.</span></span> <span data-ttu-id="d6821-213">Az optimalizálás lenne a helyes írásvédett nehéz rendszerekben ahol azt megfizethető számítások végrehajtandó írások olvasási műveletek optimalizálása érdekében.</span><span class="sxs-lookup"><span data-stu-id="d6821-213">This optimization would be good in read heavy systems where we can afford to do computations on writes in order to optimize reads.</span></span>

<span data-ttu-id="d6821-214">A lehetővé teszi, hogy előre számított mezők modell lehetséges legyen, mivel az Azure Cosmos DB támogatja **többdokumentumos tranzakció**.</span><span class="sxs-lookup"><span data-stu-id="d6821-214">The ability to have a model with pre-calculated fields is made possible because Azure Cosmos DB supports **multi-document transactions**.</span></span> <span data-ttu-id="d6821-215">Sok NoSQL-tárolókon nem tranzakciók tegye a dokumentumok között, és ezért a tervezési döntéseit, például a "mindig beágyazásához mindent" Ez a korlátozás miatt támogatják.</span><span class="sxs-lookup"><span data-stu-id="d6821-215">Many NoSQL stores cannot do transactions across documents and therefore advocate design decisions, such as "always embed everything", due to this limitation.</span></span> <span data-ttu-id="d6821-216">Az Azure Cosmos DB kiszolgálóoldali eseményindítókat vagy tárolt eljárásokat, könyvek beszúrása és szerzők ACID tranzakción belül az összes frissítése is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d6821-216">With Azure Cosmos DB, you can use server-side triggers, or stored procedures, that insert books and update authors all within an ACID transaction.</span></span> <span data-ttu-id="d6821-217">Most nem **rendelkezik** beágyazása a tartalmát egy dokumentum csak annak ellenőrzése, hogy az adatok konzisztensek maradnak.</span><span class="sxs-lookup"><span data-stu-id="d6821-217">Now you don't **have** to embed everything in to one document just to be sure that your data remains consistent.</span></span>

## <span data-ttu-id="d6821-218"><a name="NextSteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d6821-218"><a name="NextSteps"></a>Next steps</span></span>
<span data-ttu-id="d6821-219">Ez a cikk a legnagyobb takeaways tisztában lenni azzal, hogy a sémamentes világ modellezési adatok éppen olyan fontos, mint valaha is.</span><span class="sxs-lookup"><span data-stu-id="d6821-219">The biggest takeaways from this article is to understand that data modeling in a schema-free world is just as important as ever.</span></span> 

<span data-ttu-id="d6821-220">Nincs nincs egyetlen konkrét módszert meghatározni a képernyőn megjelenő adatok egy részét képviseli, mint nincs nincs egyetlen konkrét módszert meghatározni a adatok.</span><span class="sxs-lookup"><span data-stu-id="d6821-220">Just as there is no single way to represent a piece of data on a screen, there is no single way to model your data.</span></span> <span data-ttu-id="d6821-221">Meg kell az alkalmazást, és hogyan azt eredményeznek, felhasználását, és feldolgozni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="d6821-221">You need to understand your application and how it will produce, consume, and process the data.</span></span> <span data-ttu-id="d6821-222">Az itt bemutatott útmutatást némelyike alkalmazásával, beállíthat egy modell, amely a azonnali igényeket elégíti ki az alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d6821-222">Then, by applying some of the guidelines presented here you can set about creating a model that addresses the immediate needs of your application.</span></span> <span data-ttu-id="d6821-223">Az alkalmazásokat módosítani kell, amikor kihasználhatják a sémamentes adatbázis bevezető, módosítása és könnyen fejleszteni az adatmodell a rugalmasságot.</span><span class="sxs-lookup"><span data-stu-id="d6821-223">When your applications need to change, you can leverage the flexibility of a schema-free database to embrace that change and evolve your data model easily.</span></span> 

<span data-ttu-id="d6821-224">Azure Cosmos DB kapcsolatos további tudnivalókért tekintse meg a szolgáltatás [dokumentáció](https://azure.microsoft.com/documentation/services/cosmos-db/) lap.</span><span class="sxs-lookup"><span data-stu-id="d6821-224">To learn more about Azure Cosmos DB, refer to the service's [documentation](https://azure.microsoft.com/documentation/services/cosmos-db/) page.</span></span> 

<span data-ttu-id="d6821-225">Megértése, hogyan shard az adatok így vannak elrendezve több partíciót lásd [particionálás adatokat az Adatbázisba az Azure Cosmos](documentdb-partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="d6821-225">To understand how to shard your data across multiple partitions, refer to [Partitioning Data in Azure Cosmos DB](documentdb-partition-data.md).</span></span> 
