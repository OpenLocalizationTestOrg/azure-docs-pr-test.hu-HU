---
title: "az Azure Cosmos DB aaaMulti főadatbázisának architektúrák |} Microsoft Docs"
description: "Hogyan toodesign alkalmazási architektúrákban a helyi olvas, és az Azure Cosmos DB több földrajzi régióban elhelyezkedő szét megismerése."
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: 706ced74-ea67-45dd-a7de-666c3c893687
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2017
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3269c8405afe16f75db69b42e576fe76e00a8e16
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="multi-master-globally-replicated-database-architectures-with-azure-cosmos-db"></a>Több főkiszolgálós globálisan replikált Azure Cosmos DB az adatbázis-architektúra
Azure Cosmos DB támogatja kulcsrakész [globális replikációs](distribute-data-globally.md), amely toomultiple adatterületek toodistribute kis késésű hozzáférést hello munkaterhelés a bárhol lehetővé teszi. Ebben a modellben van általánosan használt publisher/fogyasztói ahol egy-egy földrajzi régiót íróhoz és egyéb (olvasás) régiókban globálisan elosztott olvasók van. 

Használhatja az Azure Cosmos DB globális replikációs toobuild alkalmazások támogatását, amelyben írók és olvasók globálisan fel vannak osztva. Ez a dokumentum egy mintát, amely lehetővé teszi a helyi írási és olvasási helyi hozzáférési elérése az Azure Cosmos DB használatával elosztott íróhoz ismertet.

## <a id="ExampleScenario"></a>Tartalom közzététele - bemutató példa
Vizsgáljuk meg olyan valós forgatókönyv toodescribe használatát globálisan elosztott több-region/több-főkulcs olvasási írási minták rendelkező Azure Cosmos DB. Fontolja meg a közzétételi tartalomplatform Azure Cosmos DB épül. Az alábbiakban olyan követelményekkel, amelyeket ezen a platformon meg kell felelnie a kiváló felhasználói élmény a közzétevők és a fogyasztók.

* Szerzők és a előfizetők eloszlatva hello world 
* Szerzők közzé kell tennie (írás) cikkek tootheir helyi (legközelebbi) régió
* Szerzők olvasók/előfizetők a cikkek felvételéhez hello földgömb elosztott rendelkezik. 
* Előfizetők kell értesítést kap, amikor új cikkek.
* Előfizetők képes tooread cikkeket a helyi régióban kell lennie. Képes tooadd értékelést toothese cikkek is kell. 
* Bárki, beleértve a hello cikkek hello Szerző képes nézet az összes értékelést csatolt tooarticles egy helyi területről hello kell lennie. 

Feltéve, hogy a fogyasztók és a közzétevők milliárd cikkeket, több millió hamarosan tudunk tooconfront hello problémák skála helység hozzáférés biztosítása mellett. Csakúgy, mint a legtöbb méretezhetőség problémát hello megoldás egy jó particionálási stratégia rejlik. A következő most hogyan toomodel cikkeket, tekintse át és -dokumentumokként, értesítések konfigurálása Azure Cosmos DB fiókok meg, és megvalósítja az adatelérési réteg. 

Ha szeretne további particionálása és partíciókulcsok toolearn, [particionálás és az Azure Cosmos Adatbázisba skálázás](partition-data.md).

## <a id="ModelingNotifications"></a>Modellezési értesítések
Értesítések adatok hírcsatornák adott tooa felhasználókként szerepelnek. Emiatt hello hozzáférési minták értesítések dokumentumok lehet mindig egyetlen felhasználó hello környezetében. Például akkor "közzétett egy értesítési tooa felhasználó" vagy "beolvasni a megadott felhasználó összes értesítésben". Igen, a particionálás kulcs ehhez a típushoz a optimális választás hello `UserId`.

    class Notification 
    { 
        // Unique ID for Notification. 
        public string Id { get; set; }

        // hello user Id for which notification is addressed to. 
        public string UserId { get; set; }

        // hello partition Key for hello resource. 
        public string PartitionKey 
        { 
            get 
            { 
                return this.UserId; 
            }
        }

        // Subscription for which this notification is raised. 
        public string SubscriptionFilter { get; set; }

        // Subject of hello notification. 
        public string ArticleId { get; set; } 
    }

## <a id="ModelingSubscriptions"></a>Modellezési előfizetések
Az előfizetések különböző feltételek, például egy adott konkrét kategóriával a cikkek, vagy egy adott publisher hozható létre. Ezért hello `SubscriptionFilter` a partíciós kulcs jó választás.

    class Subscriptions 
    { 
        // Unique ID for Subscription 
        public string Id { get; set; }

        // Subscription source. Could be Author | Category etc. 
        public string SubscriptionFilter { get; set; }

        // subscribing User. 
        public string UserId { get; set; }

        public string PartitionKey 
        { 
            get 
            { 
                return this.SubscriptionFilter; 
            } 
        } 
    }

## <a id="ModelingArticles"></a>Modellezési cikkek
Miután egy cikk, amelynél értesítések keresztül, lekérdezések általában alapján hello `Article.Id`. Kiválasztása `Article.Id` partíció hello kulcs így biztosít hello ajánlott terjesztési cikkek az Azure Cosmos DB gyűjteményt belül tárolásához. 

    class Article 
    { 
        // Unique ID for Article 
        public string Id { get; set; }
        
        public string PartitionKey 
        { 
            get 
            { 
                return this.Id; 
            } 
        }
        
        // Author of hello article
        public string Author { get; set; }

        // Category/genre of hello article
        public string Category { get; set; }

        // Tags associated with hello article
        public string[] Tags { get; set; }

        // Title of hello article
        public string Title { get; set; }
        
        //... 
    }

## <a id="ModelingReviews"></a>Ellenőrzi, hogy modellezési
Ellenőrzések hasonló cikkek, főleg írása és hello környezetében cikkben olvasható. Kiválasztása `ArticleId` egy partíció kulcs biztosít az ajánlott terjesztési és hatékony hozzáférés cikk társított felülvizsgálat. 

    class Review 
    { 
        // Unique ID for Review 
        public string Id { get; set; }

        // Article Id of hello review 
        public string ArticleId { get; set; }

        public string PartitionKey 
        { 
            get 
            { 
                return this.ArticleId; 
            } 
        }
        
        //Reviewer Id 
        public string UserId { get; set; }
        public string ReviewText { get; set; }
        
        public int Rating { get; set; } }
    }

## <a id="DataAccessMethods"></a>Adatok hozzáférési réteg módszerek
Most nézzük hello fő adatok tooimplement szükséges hozzáférési metódust. Hello listája hello módszerek `ContentPublishDatabase` kell:

    class ContentPublishDatabase 
    { 
        public async Task CreateSubscriptionAsync(string userId, string category);
    
        public async Task<IEnumerable<Notification>> ReadNotificationFeedAsync(string userId);
    
        public async Task<Article> ReadArticleAsync(string articleId);
    
        public async Task WriteReviewAsync(string articleId, string userId, string reviewText, int rating);
    
        public async Task<IEnumerable<Review>> ReadReviewsAsync(string articleId); 
    }

## <a id="Architecture"></a>Azure Cosmos DB fiók konfigurálása
helyi tooguarantee olvas és ír, azt kell particionálni nem csak a partíciós kulcs adatokat, de is hello földrajzi hozzáférési szabály alapján területekre. hello modell támaszkodik, amely egy georeplikált Azure Cosmos DB adatbázisfiók mindegyik régióhoz. Például két régiókban, itt van a telepítés több területi írási műveletek esetében:

| Fiók neve | Írási régió | Olvasási régió |
| --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.com` | `West US` |`North Europe` |
| `contentpubdatabase-europe.documents.azure.com` | `North Europe` |`West US` |

a következő ábra azt mutatja be, hogyan történik meg ezzel a beállítással egy tipikus alkalmazás olvasási és írási hello:

![Azure Cosmos DB több főkiszolgálós architektúra](./media/multi-region-writers/multi-master.png)

Íme egy kódrészletet, amely a hogyan tooinitialize hello a DAL hello futó ügyfelek `West US` régióban.
    
    ConnectionPolicy writeClientPolicy = new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp };
    writeClientPolicy.PreferredLocations.Add(LocationNames.WestUS);
    writeClientPolicy.PreferredLocations.Add(LocationNames.NorthEurope);

    DocumentClient writeClient = new DocumentClient(
        new Uri("https://contentpubdatabase-usa.documents.azure.com"), 
        writeRegionAuthKey,
        writeClientPolicy);

    ConnectionPolicy readClientPolicy = new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp };
    readClientPolicy.PreferredLocations.Add(LocationNames.NorthEurope);
    readClientPolicy.PreferredLocations.Add(LocationNames.WestUS);

    DocumentClient readClient = new DocumentClient(
        new Uri("https://contentpubdatabase-europe.documents.azure.com"),
        readRegionAuthKey,
        readClientPolicy);

A telepítő megelőző hello hello az adatelérési réteg továbbíthatja az összes írások toohello helyi fiók alapján, ahol központilag telepítették. Olvasási hajtja végre a következő mindkét fiókok tooget hello globális nézet adatainak olvasásakor. Ez a megközelítés lehet kiterjesztett tooas sok terület szükséges. Íme például egy három földrajzi régiókhoz telepítése:

| Fiók neve | Írási régió | Olvassa el az 1 régió | Olvassa el a régió 2 |
| --- | --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.com` | `West US` |`North Europe` |`Southeast Asia` |
| `contentpubdatabase-europe.documents.azure.com` | `North Europe` |`West US` |`Southeast Asia` |
| `contentpubdatabase-asia.documents.azure.com` | `Southeast Asia` |`North Europe` |`West US` |

## <a id="DataAccessImplementation"></a>Adatok hozzáférési réteg végrehajtása
Most hello az adatelérési réteg (DAL) az alkalmazás két írható régió hello végrehajtásának vizsgáljuk meg. hello DAL meg kell valósítania az alábbi lépésekkel hello:

* Hozzon létre több példánya `DocumentClient` az egyes fiókok számára. Két régiókban, a DAL feltünteti van egy `writeClient` és egy `readClient`. 
* Hello alkalmazás telepített hello régió alapján, konfigurálja a hello végpontjainak `writeclient` és `readClient`. Például a DAL hello telepített `West US` használ `contentpubdatabase-usa.documents.azure.com` írási műveletek végrehajtásához. a hello DAL rendszerbe `NorthEurope` használ `contentpubdatabase-europ.documents.azure.com` az írásokhoz.

A telepítő megelőző hello hello adatok hozzáférési metódusokat valósítható meg. Írási műveletek továbbítsa hello írási toohello megfelelő `writeClient`.

    public async Task CreateSubscriptionAsync(string userId, string category)
    {
        await this.writeClient.CreateDocumentAsync(this.contentCollection, new Subscriptions
        {
            UserId = userId,
            SubscriptionFilter = category
        });
    }

    public async Task WriteReviewAsync(string articleId, string userId, string reviewText, int rating)
    {
        await this.writeClient.CreateDocumentAsync(this.contentCollection, new Review
        {
            UserId = userId,
            ArticleId = articleId,
            ReviewText = reviewText,
            Rating = rating
        });
    }

Értesítések és az ellenőrzések olvasásához, kell olvasási régiók és a "union" hello eredmények ahogy az alábbi részlet hello:

    public async Task<IEnumerable<Notification>> ReadNotificationFeedAsync(string userId)
    {
        IDocumentQuery<Notification> writeAccountNotification = (
            from notification in this.writeClient.CreateDocumentQuery<Notification>(this.contentCollection) 
            where notification.UserId == userId 
            select notification).AsDocumentQuery();
        
        IDocumentQuery<Notification> readAccountNotification = (
            from notification in this.readClient.CreateDocumentQuery<Notification>(this.contentCollection) 
            where notification.UserId == userId 
            select notification).AsDocumentQuery();

        List<Notification> notifications = new List<Notification>();

        while (writeAccountNotification.HasMoreResults || readAccountNotification.HasMoreResults)
        {
            IList<Task<FeedResponse<Notification>>> results = new List<Task<FeedResponse<Notification>>>();

            if (writeAccountNotification.HasMoreResults)
            {
                results.Add(writeAccountNotification.ExecuteNextAsync<Notification>());
            }

            if (readAccountNotification.HasMoreResults)
            {
                results.Add(readAccountNotification.ExecuteNextAsync<Notification>());
            }

            IList<FeedResponse<Notification>> notificationFeedResult = await Task.WhenAll(results);

            foreach (FeedResponse<Notification> feed in notificationFeedResult)
            {
                notifications.AddRange(feed);
            }
        }
        return notifications;
    }

    public async Task<IEnumerable<Review>> ReadReviewsAsync(string articleId)
    {
        IDocumentQuery<Review> writeAccountReviews = (
            from review in this.writeClient.CreateDocumentQuery<Review>(this.contentCollection) 
            where review.ArticleId == articleId 
            select review).AsDocumentQuery();
        
        IDocumentQuery<Review> readAccountReviews = (
            from review in this.readClient.CreateDocumentQuery<Review>(this.contentCollection) 
            where review.ArticleId == articleId 
            select review).AsDocumentQuery();

        List<Review> reviews = new List<Review>();
        
        while (writeAccountReviews.HasMoreResults || readAccountReviews.HasMoreResults)
        {
            IList<Task<FeedResponse<Review>>> results = new List<Task<FeedResponse<Review>>>();

            if (writeAccountReviews.HasMoreResults)
            {
                results.Add(writeAccountReviews.ExecuteNextAsync<Review>());
            }

            if (readAccountReviews.HasMoreResults)
            {
                results.Add(readAccountReviews.ExecuteNextAsync<Review>());
            }

            IList<FeedResponse<Review>> notificationFeedResult = await Task.WhenAll(results);

            foreach (FeedResponse<Review> feed in notificationFeedResult)
            {
                reviews.AddRange(feed);
            }
        }

        return reviews;
    }

Így egy jó particionálási kulcs és a statikus ügyfélalapú particionálás kiválasztásával érhető el több területi helyi írások és olvasások Azure Cosmos DB használatával.

## <a id="NextSteps"></a>Következő lépések
Ez a cikk azt leírt használatát globálisan elosztott több területi olvasási-írási minták rendelkező Azure Cosmos DB használatával tartalom közzététele egy mintaforgatókönyv.

* További tudnivalók arról, hogyan támogatja a Azure Cosmos DB [globális terjesztési](distribute-data-globally.md)
* További tudnivalók [automatikus és manuális feladatátvételt az Azure Cosmos-Adatbázisba](regional-failover.md)
* További tudnivalók [az Azure Cosmos DB globális egységesítése](consistency-levels.md)
* Több régiókat hello fejlesztést [Azure Cosmos DB - DocumentDB API](tutorial-global-distribution-documentdb.md)
* Több régiókat hello fejlesztést [Azure Cosmos DB - MongoDB API](tutorial-global-distribution-MongoDB.md)
* Több régiókat hello fejlesztést [Azure Cosmos DB - tábla API](tutorial-global-distribution-table.md)
