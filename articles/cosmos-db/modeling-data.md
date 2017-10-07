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
# <a name="modeling-document-data-for-nosql-databases"></a><span data-ttu-id="5c047-104">Modellezési dokumentum adatok NoSQL-adatbázisok</span><span class="sxs-lookup"><span data-stu-id="5c047-104">Modeling document data for NoSQL databases</span></span>
<span data-ttu-id="5c047-105">Amíg a sémamentes adatbázisok, például Azure Cosmos DB, könnyen super tooembrace módosítások tooyour adatmodell továbbra is kell töltött időt vesz igénybe, továbbléphetnek az adatok.</span><span class="sxs-lookup"><span data-stu-id="5c047-105">While schema-free databases, like Azure Cosmos DB, make it super easy tooembrace changes tooyour data model you should still spend some time thinking about your data.</span></span> 

<span data-ttu-id="5c047-106">Hogyan érintetlen toobe tárolt adatokat?</span><span class="sxs-lookup"><span data-stu-id="5c047-106">How is data going toobe stored?</span></span> <span data-ttu-id="5c047-107">Hogyan van az alkalmazások folyamatos tooretrieve és lekérdezés adatait?</span><span class="sxs-lookup"><span data-stu-id="5c047-107">How is your application going tooretrieve and query data?</span></span> <span data-ttu-id="5c047-108">Az a alkalmazás vastag olvasási vagy írási nehéz?</span><span class="sxs-lookup"><span data-stu-id="5c047-108">Is your application read heavy, or write heavy?</span></span> 

<span data-ttu-id="5c047-109">A cikk elolvasása után fog tudni tooanswer hello a következő kérdéseket:</span><span class="sxs-lookup"><span data-stu-id="5c047-109">After reading this article, you will be able tooanswer hello following questions:</span></span>

* <span data-ttu-id="5c047-110">Hogyan kell egy dokumentumot egy dokumentum-adatbázisban fontolnunk?</span><span class="sxs-lookup"><span data-stu-id="5c047-110">How should I think about a document in a document database?</span></span>
* <span data-ttu-id="5c047-111">Mi az adatok modellezési, és miért érdemes I fontos?</span><span class="sxs-lookup"><span data-stu-id="5c047-111">What is data modeling and why should I care?</span></span> 
* <span data-ttu-id="5c047-112">Hogyan van dokumentum adatbázis különböző tooa relációs adatbázis modellezési adatokat?</span><span class="sxs-lookup"><span data-stu-id="5c047-112">How is modeling data in a document database different tooa relational database?</span></span>
* <span data-ttu-id="5c047-113">Hogyan express a nem relációs adatbázis adatok kapcsolatokat?</span><span class="sxs-lookup"><span data-stu-id="5c047-113">How do I express data relationships in a non-relational database?</span></span>
* <span data-ttu-id="5c047-114">Ha adatok beágyazása, és ha hivatkozás toodata?</span><span class="sxs-lookup"><span data-stu-id="5c047-114">When do I embed data and when do I link toodata?</span></span>

## <a name="embedding-data"></a><span data-ttu-id="5c047-115">Adatok beágyazása</span><span class="sxs-lookup"><span data-stu-id="5c047-115">Embedding data</span></span>
<span data-ttu-id="5c047-116">Amikor elkezdi az adatok Azure Cosmos DB, például egy dokumentum áruházban modellezését próbálja tootreat a szervezetek, mint a **önálló dokumentumok** JSON jelöli.</span><span class="sxs-lookup"><span data-stu-id="5c047-116">When you start modeling data in a document store, such as Azure Cosmos DB, try tootreat your entities as **self-contained documents** represented in JSON.</span></span>

<span data-ttu-id="5c047-117">Ahhoz, hogy férhet hozzá túl sokkal tovább, ossza meg velünk néhány lépésekkel vissza és tekintse meg a következő hogyan azt előfordulhat, hogy a modell egy relációs adatbázisban, velünk számos már ismeri a tárgy valamit.</span><span class="sxs-lookup"><span data-stu-id="5c047-117">Before we dive in too much further, let us take a few steps back and have a look at how we might model something in a relational database, a subject many of us are already familiar with.</span></span> <span data-ttu-id="5c047-118">hello következő példa bemutatja, hogyan tárolódhat egy személy egy relációs adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="5c047-118">hello following example shows how a person might be stored in a relational database.</span></span> 

![Relációs adatbázis-modell](./media/documentdb-modeling-data/relational-data-model.png)

<span data-ttu-id="5c047-120">Az használatakor a relációs adatbázisok azt az év toonormalize a tanulási már normalizálása, optimalizálására.</span><span class="sxs-lookup"><span data-stu-id="5c047-120">When working with relational databases, we've been taught for years toonormalize, normalize, normalize.</span></span>

<span data-ttu-id="5c047-121">Az adatok általában normalizálása magában foglalja a entitás, például egy személy véve, és az adatok toodiscrete kódrészletek bontásához.</span><span class="sxs-lookup"><span data-stu-id="5c047-121">Normalizing your data typically involves taking an entity, such as a person, and breaking it down in toodiscrete pieces of data.</span></span> <span data-ttu-id="5c047-122">Hello a fenti példában egy személy több kapcsolattartási részletes rekordok, valamint több címadatokat lehetnek.</span><span class="sxs-lookup"><span data-stu-id="5c047-122">In hello example above, a person can have multiple contact detail records as well as multiple address records.</span></span> <span data-ttu-id="5c047-123">Azt még egy lépéssel további, és kapcsolattartási adatai szerint lebontva további kibontása közös mezők, például egy típust.</span><span class="sxs-lookup"><span data-stu-id="5c047-123">We even go one step further and break down contact details by further extracting common fields like a type.</span></span> <span data-ttu-id="5c047-124">Ugyanazt a címet, itt rekordokban típussal rendelkezik például *Home* vagy *üzleti*</span><span class="sxs-lookup"><span data-stu-id="5c047-124">Same for address, each record here has a type like *Home* or *Business*</span></span> 

<span data-ttu-id="5c047-125">helyi irányítsa, amikor adatokat normalizálása túl hello**ne tárolja a redundáns adatok** egyes jegyezze fel, és inkább tekintse meg a toodata.</span><span class="sxs-lookup"><span data-stu-id="5c047-125">hello guiding premise when normalizing data is too**avoid storing redundant data** on each record and rather refer toodata.</span></span> <span data-ttu-id="5c047-126">Ebben a példában egy személy, a kapcsolattartási adatait és a címek tooread kell toouse ILLESZTÉSEK tooeffectively összesített az adatokat a futási időt.</span><span class="sxs-lookup"><span data-stu-id="5c047-126">In this example, tooread a person, with all their contact details and addresses, you need toouse JOINS tooeffectively aggregate your data at run time.</span></span>

    SELECT p.FirstName, p.LastName, a.City, cd.Detail
    FROM Person p
    JOIN ContactDetail cd ON cd.PersonId = p.Id
    JOIN ContactDetailType on cdt ON cdt.Id = cd.TypeId
    JOIN Address a ON a.PersonId = p.Id

<span data-ttu-id="5c047-127">Az írási műveletek egyetlen frissítése a kapcsolattartási adatait és a címek igényel sok egyes táblák között.</span><span class="sxs-lookup"><span data-stu-id="5c047-127">Updating a single person with their contact details and addresses requires write operations across many individual tables.</span></span> 

<span data-ttu-id="5c047-128">Most tegyük egy pillantást hogyan azt kellene modell hajtsa végre a megfelelő hello azonos adatok önálló egységként dokumentum-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="5c047-128">Now let's take a look at how we would model hello same data as a self-contained entity in a document database.</span></span>

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

<span data-ttu-id="5c047-129">Hello megközelítéssel fenti most tudunk **denormalizált** személy rekord hello ahol azt **beágyazott** összes hello toothis személy, például a kapcsolattartási adatait és a címet, egyetlen tooa vonatkozó információkat JSON-dokumentum.</span><span class="sxs-lookup"><span data-stu-id="5c047-129">Using hello approach above we have now **denormalized** hello person record where we **embedded** all hello information relating toothis person, such as their contact details and addresses, in tooa single JSON document.</span></span>
<span data-ttu-id="5c047-130">Ezenkívül mivel azt még nem korlátozódik tooa rögzített tudunk séma hello rugalmasságot toodo többek között a teljes mértékben rendelkező különböző alakzatok kapcsolattartási adatait.</span><span class="sxs-lookup"><span data-stu-id="5c047-130">In addition, because we're not confined tooa fixed schema we have hello flexibility toodo things like having contact details of different shapes entirely.</span></span> 

<span data-ttu-id="5c047-131">A teljes személy rekord lekérése hello adatbázis már olvasási művelete egyetlen gyűjtemény ellen, és egyetlen dokumentum egyetlen.</span><span class="sxs-lookup"><span data-stu-id="5c047-131">Retrieving a complete person record from hello database is now a single read operation against a single collection and for a single document.</span></span> <span data-ttu-id="5c047-132">Frissítés személy rekord, a kapcsolattartási adatait és a címet, akkor is egyetlen dokumentum szemben egyetlen írási művelet.</span><span class="sxs-lookup"><span data-stu-id="5c047-132">Updating a person record, with their contact details and addresses, is also a single write operation against a single document.</span></span>

<span data-ttu-id="5c047-133">Az alkalmazás által denormalizing adatok, esetleg tooissue kevesebb lekérdezések és frissítések toocomplete gyakori műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="5c047-133">By denormalizing data, your application may need tooissue fewer queries and updates toocomplete common operations.</span></span> 

### <a name="when-tooembed"></a><span data-ttu-id="5c047-134">Ha tooembed</span><span class="sxs-lookup"><span data-stu-id="5c047-134">When tooembed</span></span>
<span data-ttu-id="5c047-135">Általában a beágyazott adatok használata esetén a modellek:</span><span class="sxs-lookup"><span data-stu-id="5c047-135">In general, use embedded data models when:</span></span>

* <span data-ttu-id="5c047-136">Nincsenek **tartalmaz** entitások közötti kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="5c047-136">There are **contains** relationships between entities.</span></span>
* <span data-ttu-id="5c047-137">Nincsenek **egy néhány** entitások közötti kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="5c047-137">There are **one-to-few** relationships between entities.</span></span>
* <span data-ttu-id="5c047-138">Beágyazott adat, amely **ritkán változó**.</span><span class="sxs-lookup"><span data-stu-id="5c047-138">There is embedded data that **changes infrequently**.</span></span>
* <span data-ttu-id="5c047-139">Nincs beágyazott adatok nem nő **nélkül kötött**.</span><span class="sxs-lookup"><span data-stu-id="5c047-139">There is embedded data won't grow **without bound**.</span></span>
* <span data-ttu-id="5c047-140">Nincs a beágyazott adatok **szerves** toodata a dokumentumban.</span><span class="sxs-lookup"><span data-stu-id="5c047-140">There is embedded data that is **integral** toodata in a document.</span></span>

> [!NOTE]
> <span data-ttu-id="5c047-141">Általában a modellek biztosítják jobban adatok denormalizált **olvasási** teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="5c047-141">Typically denormalized data models provide better **read** performance.</span></span>
> 
> 

### <a name="when-not-tooembed"></a><span data-ttu-id="5c047-142">Ha nem tooembed</span><span class="sxs-lookup"><span data-stu-id="5c047-142">When not tooembed</span></span>
<span data-ttu-id="5c047-143">Hello tapasztalatok dokumentum-adatbázisban toodenormalize minden rendben, és minden adat beágyazása tooa egyetlen dokumentum, ennek eredményeképpen előfordulhat toosome olyan helyzetekben, el kell kerülni.</span><span class="sxs-lookup"><span data-stu-id="5c047-143">While hello rule of thumb in a document database is toodenormalize everything and embed all data in tooa single document, this can lead toosome situations that should be avoided.</span></span>

<span data-ttu-id="5c047-144">A JSON-részlet igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="5c047-144">Take this JSON snippet.</span></span>

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

<span data-ttu-id="5c047-145">Ez akkor lehet, hogy mi a feladás egy vagy több entitás beágyazott megjegyzésekkel néz azt modellezési volt egy tipikus blogba vagy CMS, a rendszer.</span><span class="sxs-lookup"><span data-stu-id="5c047-145">This might be what a post entity with embedded comments would look like if we were modeling a typical blog, or CMS, system.</span></span> <span data-ttu-id="5c047-146">hello ebben a példában probléma a megjegyzések tömb hello **unbounded**, ami azt jelenti, hogy van-e bármilyen egyetlen post rendelkezhet megjegyzések számának nincs (gyakorlati) korlátozása toohello.</span><span class="sxs-lookup"><span data-stu-id="5c047-146">hello problem with this example is that hello comments array is **unbounded**, meaning that there is no (practical) limit toohello number of comments any single post can have.</span></span> <span data-ttu-id="5c047-147">Ez a növekedésével hello dokumentum hello mérete sikerült jelentősen probléma lesz.</span><span class="sxs-lookup"><span data-stu-id="5c047-147">This will become a problem as hello size of hello document could grow significantly.</span></span>

<span data-ttu-id="5c047-148">Hello hello méretét, a dokumentum bővül hello képességét tootransmit hello adatok hello keresztülhaladnak a hálózaton, valamint az olvasási és frissítési hello a dokumentumban léptékű csökkenhet.</span><span class="sxs-lookup"><span data-stu-id="5c047-148">As hello size of hello document grows hello ability tootransmit hello data over hello wire as well as reading and updating hello document, at scale, will be impacted.</span></span>

<span data-ttu-id="5c047-149">Ebben az esetben a következő modell jobban tooconsider hello lenne.</span><span class="sxs-lookup"><span data-stu-id="5c047-149">In this case it would be better tooconsider hello following model.</span></span>

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

<span data-ttu-id="5c047-150">A modellnek hello három legutóbbi megjegyzések hello a beágyazott utáni magát, amely egy tömb rögzített kötött az ez idő.</span><span class="sxs-lookup"><span data-stu-id="5c047-150">This model has hello three most recent comments embedded on hello post itself, which is an array with a fixed bound this time.</span></span> <span data-ttu-id="5c047-151">hello más megjegyzések toobatches 100 megjegyzések találhatók, és külön dokumentumok tárolja.</span><span class="sxs-lookup"><span data-stu-id="5c047-151">hello other comments are grouped in toobatches of 100 comments and stored in separate documents.</span></span> <span data-ttu-id="5c047-152">hello köteg mérete hello mint 100 lett választva, mert a fiktív alkalmazás lehetővé teszi, hogy hello felhasználói tooload 100 megjegyzések egyszerre.</span><span class="sxs-lookup"><span data-stu-id="5c047-152">hello size of hello batch was chosen as 100 because our fictitious application allows hello user tooload 100 comments at a time.</span></span>  

<span data-ttu-id="5c047-153">Egy másik esetet, amikor beágyazási adatok nem jó ötlet esetén hello a beágyazott adatok gyakran használt dokumentumok között, és gyakran változik.</span><span class="sxs-lookup"><span data-stu-id="5c047-153">Another case where embedding data is not a good idea is when hello embedded data is used often across documents and will change frequently.</span></span> 

<span data-ttu-id="5c047-154">A JSON-részlet igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="5c047-154">Take this JSON snippet.</span></span>

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

<span data-ttu-id="5c047-155">Ez lehet egy személy készlet portfóliót.</span><span class="sxs-lookup"><span data-stu-id="5c047-155">This could represent a person's stock portfolio.</span></span> <span data-ttu-id="5c047-156">Azt választotta, hogy tooembed hello tőzsdei információk tooeach portfóliót dokumentumban.</span><span class="sxs-lookup"><span data-stu-id="5c047-156">We have chosen tooembed hello stock information in tooeach portfolio document.</span></span> <span data-ttu-id="5c047-157">Egy olyan környezetben, ahol kapcsolódó adatok változik gyakran, például egy alkalmazás, mivel a készlet adatok gyakran változnak beágyazás állapotra vált, hogy folyamatosan frissítjük az egyes portfóliót dokumentum minden alkalommal, amikor egy készlet forog toomean.</span><span class="sxs-lookup"><span data-stu-id="5c047-157">In an environment where related data is changing frequently, like a stock trading application, embedding data that changes frequently is going toomean that you are constantly updating each portfolio document every time a stock is traded.</span></span>

<span data-ttu-id="5c047-158">Készlet *zaza* egyetlen alkalommal több száz kerülhetnek rendelkezhetnek a nap és a felhasználók ezreit *zaza* a saját portfóliót.</span><span class="sxs-lookup"><span data-stu-id="5c047-158">Stock *zaza* may be traded many hundreds of times in a single day and thousands of users could have *zaza* on their portfolio.</span></span> <span data-ttu-id="5c047-159">A fenti hello például adatmodellt azt kellene tooupdate portfóliót dokumentumok sok ezer sokszor nem jól méretezhető tooa rendszer vezető naponta.</span><span class="sxs-lookup"><span data-stu-id="5c047-159">With a data model like hello above we would have tooupdate many thousands of portfolio documents many times every day leading tooa system that won't scale very well.</span></span> 

## <span data-ttu-id="5c047-160"><a id="Refer"></a>Adatok hivatkozik</span><span class="sxs-lookup"><span data-stu-id="5c047-160"><a id="Refer"></a>Referencing data</span></span>
<span data-ttu-id="5c047-161">Igen sok esetben szépen adatok beágyazása működik, de egyértelmű, hogy nincsenek helyzetek, amikor az adatok denormalizing mint érdemes további problémákat okozhat.</span><span class="sxs-lookup"><span data-stu-id="5c047-161">So, embedding data works nicely for many cases but it is clear that there are scenarios when denormalizing your data will cause more problems than it is worth.</span></span> <span data-ttu-id="5c047-162">Ezért Mi a teendő most?</span><span class="sxs-lookup"><span data-stu-id="5c047-162">So what do we do now?</span></span> 

<span data-ttu-id="5c047-163">Relációs adatbázisok nincsenek hello egyetlen hely, ahol létrehozhat entitások közötti kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="5c047-163">Relational databases are not hello only place where you can create relationships between entities.</span></span> <span data-ttu-id="5c047-164">A dokumentum-adatbázisban mindig egy dokumentum más dokumentumban toodata ténylegesen kapcsolódó információk.</span><span class="sxs-lookup"><span data-stu-id="5c047-164">In a document database you can have information in one document that actually relates toodata in other documents.</span></span> <span data-ttu-id="5c047-165">Most I vagyok nem javasolni egy percig, még akkor is, hogy azt rendszerek jobb olyan környezethez a legalkalmasabb tooa relációs adatbázis az Azure Cosmos Adatbázisba, vagy bármely más dokumentum-adatbázis létrehozása, de egyszerű kapcsolatok rendben, és nagyon hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="5c047-165">Now, I am not advocating for even one minute that we build systems that would be better suited tooa relational database in Azure Cosmos DB, or any other document database, but simple relationships are fine and can be very useful.</span></span> 

<span data-ttu-id="5c047-166">Az alábbi JSON hello választottuk korábban, de azt tekintse meg a készlet cikk toohello hello portfóliót beágyazás helyett most egy készlet portfóliót toouse hello példát.</span><span class="sxs-lookup"><span data-stu-id="5c047-166">In hello JSON below we chose toouse hello example of a stock portfolio from earlier but this time we refer toohello stock item on hello portfolio instead of embedding it.</span></span> <span data-ttu-id="5c047-167">Ezzel a módszerrel megváltozásakor hello készlet elem gyakran hello nap hello csak a dokumentum, amelyet a frissített toobe hello egyetlen készlet dokumentum.</span><span class="sxs-lookup"><span data-stu-id="5c047-167">This way, when hello stock item changes frequently throughout hello day hello only document that needs toobe updated is hello single stock document.</span></span> 

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


<span data-ttu-id="5c047-168">Az azonnali hátránya toothis megoldás azonban, ha az alkalmazás szerepel-e minden olyan személy portfóliót; megjelenítésekor tartott készlet szükséges tooshow információ Ebben az esetben kellene toomake több utazgatással toohello adatbázis tooload hello adatai készlet dokumentumok.</span><span class="sxs-lookup"><span data-stu-id="5c047-168">An immediate downside toothis approach though is if your application is required tooshow information about each stock that is held when displaying a person's portfolio; in this case you would need toomake multiple trips toohello database tooload hello information for each stock document.</span></span> <span data-ttu-id="5c047-169">Itt hajtottunk írási műveleteket, amelyek gyakran hello nap folyamán fordulhat elő, de az olvasási műveleteket, amelyek vélhetően kisebb mértékű befolyásolása mellett a hello teljesítmény az adott rendszer hello pedig sérült a döntési tooimprove hello hatékonyságát.</span><span class="sxs-lookup"><span data-stu-id="5c047-169">Here we've made a decision tooimprove hello efficiency of write operations, which happen frequently throughout hello day, but in turn compromised on hello read operations that potentially have less impact on hello performance of this particular system.</span></span>

> [!NOTE]
> <span data-ttu-id="5c047-170">Normalizált adatmodellekben **megkövetelheti további kiszolgálókkal való adatváltások számát** toohello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="5c047-170">Normalized data models **can require more round trips** toohello server.</span></span>
> 
> 

### <a name="what-about-foreign-keys"></a><span data-ttu-id="5c047-171">Mi a helyzet a külső kulcsokat?</span><span class="sxs-lookup"><span data-stu-id="5c047-171">What about foreign keys?</span></span>
<span data-ttu-id="5c047-172">Nincs jelenleg egy megkötés, mert külső kulcsok vagy egyéb, rendelkezésre álló dokumentumok közötti dokumentum kapcsolat hatékony "gyenge links" és hello adatbázis maga nem ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="5c047-172">Because there is currently no concept of a constraint, foreign-key or otherwise, any inter-document relationships that you have in documents are effectively "weak links" and will not be verified by hello database itself.</span></span> <span data-ttu-id="5c047-173">Ha azt szeretné, hogy a dokumentum hivatkozik adatok hello tooensure tooactually létezik-e, majd toodo ez szükséges az alkalmazás vagy hello használata kiszolgálóoldali eseményindítók és tárolt eljárások az Azure Cosmos-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="5c047-173">If you want tooensure that hello data a document is referring tooactually exists, then you need toodo this in your application, or through hello use of server-side triggers or stored procedures on Azure Cosmos DB.</span></span>

### <a name="when-tooreference"></a><span data-ttu-id="5c047-174">Ha tooreference</span><span class="sxs-lookup"><span data-stu-id="5c047-174">When tooreference</span></span>
<span data-ttu-id="5c047-175">Általában a normalizált adatok használata esetén a modellek:</span><span class="sxs-lookup"><span data-stu-id="5c047-175">In general, use normalized data models when:</span></span>

* <span data-ttu-id="5c047-176">Képviselő **egy-a-többhöz** kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="5c047-176">Representing **one-to-many** relationships.</span></span>
* <span data-ttu-id="5c047-177">Képviselő **több-a-többhöz** kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="5c047-177">Representing **many-to-many** relationships.</span></span>
* <span data-ttu-id="5c047-178">A kapcsolódó adatok **változik gyakran**.</span><span class="sxs-lookup"><span data-stu-id="5c047-178">Related data **changes frequently**.</span></span>
* <span data-ttu-id="5c047-179">Hivatkozásban szereplő lehet **unbounded**.</span><span class="sxs-lookup"><span data-stu-id="5c047-179">Referenced data could be **unbounded**.</span></span>

> [!NOTE]
> <span data-ttu-id="5c047-180">Általában normalizálása jobban biztosít **írási** teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="5c047-180">Typically normalizing provides better **write** performance.</span></span>
> 
> 

### <a name="where-do-i-put-hello-relationship"></a><span data-ttu-id="5c047-181">Ha helyezze a hello kapcsolatban?</span><span class="sxs-lookup"><span data-stu-id="5c047-181">Where do I put hello relationship?</span></span>
<span data-ttu-id="5c047-182">hello növekedési hello kapcsolat segítségével megállapíthatja, hogy mely toostore hello hivatkozás található.</span><span class="sxs-lookup"><span data-stu-id="5c047-182">hello growth of hello relationship will help determine in which document toostore hello reference.</span></span>

<span data-ttu-id="5c047-183">Ha úgy tekintünk hello JSON az alábbi, gyártók és -könyvekkel modellek.</span><span class="sxs-lookup"><span data-stu-id="5c047-183">If we look at hello JSON below that models publishers and books.</span></span>

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

<span data-ttu-id="5c047-184">Ha hello könyvek / publisher hello száma korlátozott növekedési kicsi, majd tárolja a hello könyv hivatkozás hello publisher dokumentumban lévő lehetnek hasznosak.</span><span class="sxs-lookup"><span data-stu-id="5c047-184">If hello number of hello books per publisher is small with limited growth, then storing hello book reference inside hello publisher document may be useful.</span></span> <span data-ttu-id="5c047-185">Azonban ha hello száma / publisher könyvek unbounded, majd az adatmodell átmásolásának toomutable, tömbök, mint hello példa publisher dokumentum fenti növekszik.</span><span class="sxs-lookup"><span data-stu-id="5c047-185">However, if hello number of books per publisher is unbounded, then this data model would lead toomutable, growing arrays, as in hello example publisher document above.</span></span> 

<span data-ttu-id="5c047-186">Váltás körül bit dolgot volna okozza, hogy továbbra is jelöli most hello-e ugyanazokat az adatokat egy modell Ezzel elkerülheti nagy változtatható gyűjtemények.</span><span class="sxs-lookup"><span data-stu-id="5c047-186">Switching things around a bit would result in a model that still represents hello same data but now avoids these large mutable collections.</span></span>

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

<span data-ttu-id="5c047-187">A fenti példa hello, azt áthúzott hello hello publisher dokumentum unbounded gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="5c047-187">In hello above example, we have dropped hello unbounded collection on hello publisher document.</span></span> <span data-ttu-id="5c047-188">Ehelyett csak kell a hivatkozás toohello közzétevő minden könyv-dokumentum.</span><span class="sxs-lookup"><span data-stu-id="5c047-188">Instead we just have a a reference toohello publisher on each book document.</span></span>

### <a name="how-do-i-model-manymany-relationships"></a><span data-ttu-id="5c047-189">Hogyan a több: többhöz kapcsolatok modell?</span><span class="sxs-lookup"><span data-stu-id="5c047-189">How do I model many:many relationships?</span></span>
<span data-ttu-id="5c047-190">Egy relációs adatbázisban *több: több* kapcsolatok gyakran van modellezve a táblákat, amelyek csak csatlakozás együtt más táblákból származó rekordokat.</span><span class="sxs-lookup"><span data-stu-id="5c047-190">In a relational database *many:many* relationships are often modeled with join tables, which just join records from other tables together.</span></span> 

![Táblák illesztése](./media/documentdb-modeling-data/join-table.png)

<span data-ttu-id="5c047-192">Előfordulhat, hogy kísértésbe tooreplicate hello azonos dolog használatával dokumentumokat, és előállít egy, a következőhöz hasonló toohello következő adatmodell.</span><span class="sxs-lookup"><span data-stu-id="5c047-192">You might be tempted tooreplicate hello same thing using documents and produce a data model that looks similar toohello following.</span></span>

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

<span data-ttu-id="5c047-193">Ez akkor működik.</span><span class="sxs-lookup"><span data-stu-id="5c047-193">This would work.</span></span> <span data-ttu-id="5c047-194">Azonban betöltésekor vagy a szerző könyveiben rendelkező, vagy a könyv betölteni a szerző, a mindig igényelnének hello adatbázis legalább két további lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="5c047-194">However, loading either an author with their books, or loading a book with its author, would always require at least two additional queries against hello database.</span></span> <span data-ttu-id="5c047-195">Egy lekérdezés toohello és majd egy másik lekérdezés toofetch hello tényleges dokumentum alatt álló tartományhoz való csatlakozás.</span><span class="sxs-lookup"><span data-stu-id="5c047-195">One query toohello joining document and then another query toofetch hello actual document being joined.</span></span> 

<span data-ttu-id="5c047-196">Ha az illesztés tábla csak akkor van kapcsolása együtt adatok két darab, miért nem előfordulásoknál hagyja el teljesen?</span><span class="sxs-lookup"><span data-stu-id="5c047-196">If all this join table is doing is gluing together two pieces of data, then why not drop it completely?</span></span>
<span data-ttu-id="5c047-197">Vegye figyelembe a következőket hello.</span><span class="sxs-lookup"><span data-stu-id="5c047-197">Consider hello following.</span></span>

    Author documents:
    {"id": "a1", "name": "Thomas Andersen", "books": ["b1, "b2", "b3"]}
    {"id": "a2", "name": "William Wakefield", "books": ["b1", "b4"]}

    Book documents: 
    {"id": "b1", "name": "Azure Cosmos DB 101", "authors": ["a1", "a2"]}
    {"id": "b2", "name": "Azure Cosmos DB for RDBMS Users", "authors": ["a1"]}
    {"id": "b3", "name": "Learn about Azure Cosmos DB", "authors": ["a1"]}
    {"id": "b4", "name": "Deep Dive in tooAzure Cosmos DB", "authors": ["a2"]}

<span data-ttu-id="5c047-198">Most ha egy szerző, azonnal tudható, hogy mely általa könyvek, és ezzel szemben ha betöltött könyv dokumentum akkor tudható, hogy hello szerzőjét vagy hello azonosítói.</span><span class="sxs-lookup"><span data-stu-id="5c047-198">Now, if I had an author, I immediately know which books they have written, and conversely if I had a book document loaded I would know hello ids of hello author(s).</span></span> <span data-ttu-id="5c047-199">Ezzel a kiszolgáló hello számának csökkentése hello illesztési táblázaton közvetítő lekérdezés takaríthat meg az alkalmazás rendelkezik toomake kiszolgálókkal való adatváltások számát.</span><span class="sxs-lookup"><span data-stu-id="5c047-199">This saves that intermediary query against hello join table reducing hello number of server round trips your application has toomake.</span></span> 

## <span data-ttu-id="5c047-200"><a id="WrapUp"></a>Adatok a hibrid modellek</span><span class="sxs-lookup"><span data-stu-id="5c047-200"><a id="WrapUp"></a>Hybrid data models</span></span>
<span data-ttu-id="5c047-201">A Microsoft most kikeresi beágyazás (vagy denormalizing) és azok upsides rendelkező hivatkozó (vagy normalizálása) adatokat, és minden a biztonság sérüléseinek rendelkezik, és úgy találtuk.</span><span class="sxs-lookup"><span data-stu-id="5c047-201">We've now looked embedding (or denormalizing) and referencing (or normalizing) data, each have their upsides and each have compromises as we have seen.</span></span> 

<span data-ttu-id="5c047-202">Azt nem mindig rendelkeznek toobe vagy, vagy nem Ijedt toomix dolgot még be kell.</span><span class="sxs-lookup"><span data-stu-id="5c047-202">It doesn't always have toobe either or, don't be scared toomix things up a little.</span></span> 

<span data-ttu-id="5c047-203">Az alkalmazás a konkrét használati mintákat és előfordulhat, hogy hol keverése beágyazott esetekben munkaterhelések alapján és hivatkozott adatok szabálykészletében, és sikerült átfutási toosimpler úgy az alkalmazáslogikát kevesebb kiszolgálóval kerekíteni való adatváltások számát továbbra is a megfelelő szintű teljesítmény megőrzése .</span><span class="sxs-lookup"><span data-stu-id="5c047-203">Based on your application's specific usage patterns and workloads there may be cases where mixing embedded and referenced data makes sense and could lead toosimpler application logic with fewer server round trips while still maintaining a good level of performance.</span></span>

<span data-ttu-id="5c047-204">Vegye figyelembe a következő JSON hello.</span><span class="sxs-lookup"><span data-stu-id="5c047-204">Consider hello following JSON.</span></span> 

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

<span data-ttu-id="5c047-205">Itt azt (főleg) követte hello beágyazott modellt, amikor más entitás adatait beágyazott hello legfelső szintű dokumentumban, de más adatok hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="5c047-205">Here we've (mostly) followed hello embedded model, where data from other entities are embedded in hello top-level document, but other data is referenced.</span></span> 

<span data-ttu-id="5c047-206">Hello könyv-dokumentum tekinti meg, ha néhány láthatja érdekes mezők, ha úgy tekintünk, a szerzők hello tömb.</span><span class="sxs-lookup"><span data-stu-id="5c047-206">If you look at hello book document, we can see a few interesting fields when we look at hello array of authors.</span></span> <span data-ttu-id="5c047-207">Van egy *azonosító* mező, amelyben hello mező használjuk toorefer hátsó tooan Szerző dokumentum, a normalizált modell- kat majd szabványos eljárása is rendelkezik *neve* és *thumbnailUrl*.</span><span class="sxs-lookup"><span data-stu-id="5c047-207">There is an *id* field which is hello field we use toorefer back tooan author document, standard practice in a normalized model, but then we also have *name* and *thumbnailUrl*.</span></span> <span data-ttu-id="5c047-208">Azt sikerült már csak állapottal *azonosító* és hello alkalmazás tooget szükség további adatokat hello megfelelő Szerző dokumentumból használatával "link" hello, de mivel az alkalmazás hello Szerző neve és a minden könyv jelenik meg a miniatűr kép is mentheti egy könyv oda-vissza toohello kiszolgálónként listaként által denormalizing **néhány** hello szerzőjétől származó adatokat.</span><span class="sxs-lookup"><span data-stu-id="5c047-208">We could've just stuck with *id* and left hello application tooget any additional information it needed from hello respective author document using hello "link", but because our application displays hello author's name and a thumbnail picture with every book displayed we can save a round trip toohello server per book in a list by denormalizing **some** data from hello author.</span></span>

<span data-ttu-id="5c047-209">Ellenőrizze, ha hello Szerző neve megváltozott, vagy hogy fényképező kívánta tooupdate azt kellene toogo frissítést minden könyv legalább egyszer közzététel, de az alkalmazás, hello azt feltételezi, hogy szerzők ne nagyon gyakran, módosítsa a nevek alapján ez az egy elfogadható terv döntés.</span><span class="sxs-lookup"><span data-stu-id="5c047-209">Sure, if hello author's name changed or they wanted tooupdate their photo we'd have toogo an update every book they ever published but for our application, based on hello assumption that authors don't change their names very often, this is an acceptable design decision.</span></span>  

<span data-ttu-id="5c047-210">Hello példa nincsenek **összesítések előre számított** költséges olvasási művelet feldolgozási toosave értékeket.</span><span class="sxs-lookup"><span data-stu-id="5c047-210">In hello example there are **pre-calculated aggregates** values toosave expensive processing on a read operation.</span></span> <span data-ttu-id="5c047-211">Hello példában hello Szerző dokumentumban beágyazott hello adatok egy részét az adatai, számított futásidőben.</span><span class="sxs-lookup"><span data-stu-id="5c047-211">In hello example, some of hello data embedded in hello author document is data that is calculated at run-time.</span></span> <span data-ttu-id="5c047-212">Egy könyv-dokumentum létrehozása minden alkalommal, amikor egy új könyv közzé van téve, **és** hello countOfBooks mező értéke, amely egy adott szerző léteznek könyv dokumentumok hello száma alapján számított tooa érték.</span><span class="sxs-lookup"><span data-stu-id="5c047-212">Every time a new book is published, a book document is created **and** hello countOfBooks field is set tooa calculated value based on hello number of book documents that exist for a particular author.</span></span> <span data-ttu-id="5c047-213">Az optimalizálás lenne a helyes olvasási nehéz rendszerekben ahol azt megfizethető toodo számítások a rendelés toooptimize olvasások az írási műveletek.</span><span class="sxs-lookup"><span data-stu-id="5c047-213">This optimization would be good in read heavy systems where we can afford toodo computations on writes in order toooptimize reads.</span></span>

<span data-ttu-id="5c047-214">előre számított mezők modell lehetséges legyen, mivel az Azure Cosmos DB támogatja képességét toohave hello **többdokumentumos tranzakció**.</span><span class="sxs-lookup"><span data-stu-id="5c047-214">hello ability toohave a model with pre-calculated fields is made possible because Azure Cosmos DB supports **multi-document transactions**.</span></span> <span data-ttu-id="5c047-215">Sok NoSQL-tárolókon nem tranzakciók tegye a dokumentumok között, és ezért a tervezési döntéseit, például a "mindig beágyazásához mindent" toothis korlátozás miatt támogatják.</span><span class="sxs-lookup"><span data-stu-id="5c047-215">Many NoSQL stores cannot do transactions across documents and therefore advocate design decisions, such as "always embed everything", due toothis limitation.</span></span> <span data-ttu-id="5c047-216">Az Azure Cosmos DB kiszolgálóoldali eseményindítókat vagy tárolt eljárásokat, könyvek beszúrása és szerzők ACID tranzakción belül az összes frissítése is használhatja.</span><span class="sxs-lookup"><span data-stu-id="5c047-216">With Azure Cosmos DB, you can use server-side triggers, or stored procedures, that insert books and update authors all within an ACID transaction.</span></span> <span data-ttu-id="5c047-217">Most nem **rendelkezik** tooembed mindent tooone a dokumentum csak toobe meg arról, hogy az adatok konzisztensek maradnak.</span><span class="sxs-lookup"><span data-stu-id="5c047-217">Now you don't **have** tooembed everything in tooone document just toobe sure that your data remains consistent.</span></span>

## <span data-ttu-id="5c047-218"><a name="NextSteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5c047-218"><a name="NextSteps"></a>Next steps</span></span>
<span data-ttu-id="5c047-219">hello legnagyobb takeaways a cikkből, hogy sémamentes világában modellezési adata éppen olyan fontos szerint bármikor toounderstand.</span><span class="sxs-lookup"><span data-stu-id="5c047-219">hello biggest takeaways from this article is toounderstand that data modeling in a schema-free world is just as important as ever.</span></span> 

<span data-ttu-id="5c047-220">Nem lehet egyetlen konkrét módszert toorepresent az adatok a képernyőn van, mint nincs nincs egyetlen konkrét módszert meghatározni toomodel adatait.</span><span class="sxs-lookup"><span data-stu-id="5c047-220">Just as there is no single way toorepresent a piece of data on a screen, there is no single way toomodel your data.</span></span> <span data-ttu-id="5c047-221">Szükségesek toounderstand az alkalmazást, és hogyan eredményeznek, használnak, és hello adatok feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="5c047-221">You need toounderstand your application and how it will produce, consume, and process hello data.</span></span> <span data-ttu-id="5c047-222">Majd néhány hello alkalmazásával irányelvek jelenik itt állíthatja be modell, amely hello azonnali igényeket elégíti ki az alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5c047-222">Then, by applying some of hello guidelines presented here you can set about creating a model that addresses hello immediate needs of your application.</span></span> <span data-ttu-id="5c047-223">Az alkalmazások toochange kell, amikor kihasználhatja a sémamentes adatbázis tooembrace hello rugalmasságát, módosítása és könnyen fejleszteni az adatmodellt.</span><span class="sxs-lookup"><span data-stu-id="5c047-223">When your applications need toochange, you can leverage hello flexibility of a schema-free database tooembrace that change and evolve your data model easily.</span></span> 

<span data-ttu-id="5c047-224">toolearn Azure Cosmos DB, kapcsolatos további információkért tekintse meg a toohello szolgáltatás [dokumentáció](https://azure.microsoft.com/documentation/services/cosmos-db/) lap.</span><span class="sxs-lookup"><span data-stu-id="5c047-224">toolearn more about Azure Cosmos DB, refer toohello service's [documentation](https://azure.microsoft.com/documentation/services/cosmos-db/) page.</span></span> 

<span data-ttu-id="5c047-225">toounderstand hogyan tooshard az adatok így vannak elrendezve több partíciót, tekintse meg a túl[particionálás adatokat az Adatbázisba az Azure Cosmos](documentdb-partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="5c047-225">toounderstand how tooshard your data across multiple partitions, refer too[Partitioning Data in Azure Cosmos DB](documentdb-partition-data.md).</span></span> 
