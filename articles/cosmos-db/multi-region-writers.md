---
title: "Az Azure Cosmos DB több főkiszolgálós adatbázis architektúrák |} Microsoft Docs"
description: "További tudnivalók a helyi olvasási és írási alkalmazási architektúrákban tervezéséről Azure Cosmos DB több földrajzi régiók között."
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
ms.openlocfilehash: cf1482ae7b1070023703f5dbe861d151f5d64fd8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="multi-master-globally-replicated-database-architectures-with-azure-cosmos-db"></a><span data-ttu-id="f25ec-103">Több főkiszolgálós globálisan replikált Azure Cosmos DB az adatbázis-architektúra</span><span class="sxs-lookup"><span data-stu-id="f25ec-103">Multi-master globally replicated database architectures with Azure Cosmos DB</span></span>
<span data-ttu-id="f25ec-104">Azure Cosmos DB támogatja kulcsrakész [globális replikációs](distribute-data-globally.md), amely lehetővé teszi, hogy több területre kis késleltetésű hozzáféréssel bárhol a munkaterhelési adatok terjesztése.</span><span class="sxs-lookup"><span data-stu-id="f25ec-104">Azure Cosmos DB supports turnkey [global replication](distribute-data-globally.md), which allows you to distribute data to multiple regions with low latency access anywhere in the workload.</span></span> <span data-ttu-id="f25ec-105">Ebben a modellben van általánosan használt publisher/fogyasztói ahol egy-egy földrajzi régiót íróhoz és egyéb (olvasás) régiókban globálisan elosztott olvasók van.</span><span class="sxs-lookup"><span data-stu-id="f25ec-105">This model is commonly used for publisher/consumer workloads where there is a writer in a single geographic region and globally distributed readers in other (read) regions.</span></span> 

<span data-ttu-id="f25ec-106">Azure Cosmos DB globális replikációs támogatási segítségével is, amelyben írók és olvasók globálisan fel vannak osztva alkalmazások létrehozását.</span><span class="sxs-lookup"><span data-stu-id="f25ec-106">You can also use Azure Cosmos DB's global replication support to build applications in which writers and readers are globally distributed.</span></span> <span data-ttu-id="f25ec-107">Ez a dokumentum egy mintát, amely lehetővé teszi a helyi írási és olvasási helyi hozzáférési elérése az Azure Cosmos DB használatával elosztott íróhoz ismertet.</span><span class="sxs-lookup"><span data-stu-id="f25ec-107">This document outlines a pattern that enables achieving local write and local read access for distributed writers using Azure Cosmos DB.</span></span>

## <span data-ttu-id="f25ec-108"><a id="ExampleScenario"></a>Tartalom közzététele - bemutató példa</span><span class="sxs-lookup"><span data-stu-id="f25ec-108"><a id="ExampleScenario"></a>Content Publishing - an example scenario</span></span>
<span data-ttu-id="f25ec-109">Nézzük leírására, hogyan használható globálisan elosztott több-region/több-főkulcs olvasási írási minták Azure Cosmos DB valós forgatókönyvekben.</span><span class="sxs-lookup"><span data-stu-id="f25ec-109">Let's look at a real world scenario to describe how you can use globally distributed multi-region/multi-master read write patterns with Azure Cosmos DB.</span></span> <span data-ttu-id="f25ec-110">Fontolja meg a közzétételi tartalomplatform Azure Cosmos DB épül.</span><span class="sxs-lookup"><span data-stu-id="f25ec-110">Consider a content publishing platform built on Azure Cosmos DB.</span></span> <span data-ttu-id="f25ec-111">Az alábbiakban olyan követelményekkel, amelyeket ezen a platformon meg kell felelnie a kiváló felhasználói élmény a közzétevők és a fogyasztók.</span><span class="sxs-lookup"><span data-stu-id="f25ec-111">Here are some requirements that this platform must meet for a great user experience for both publishers and consumers.</span></span>

* <span data-ttu-id="f25ec-112">Szerzők és a előfizetők oszlik el a világ</span><span class="sxs-lookup"><span data-stu-id="f25ec-112">Both authors and subscribers are spread over the world</span></span> 
* <span data-ttu-id="f25ec-113">Szerzők közzé kell tennie (írás) cikkeket a helyi (legközelebbi) régió</span><span class="sxs-lookup"><span data-stu-id="f25ec-113">Authors must publish (write) articles to their local (closest) region</span></span>
* <span data-ttu-id="f25ec-114">Szerzők olvasók/előfizetők a cikkek felvételéhez világ különböző pontjain lehet.</span><span class="sxs-lookup"><span data-stu-id="f25ec-114">Authors have readers/subscribers of their articles who are distributed across the globe.</span></span> 
* <span data-ttu-id="f25ec-115">Előfizetők kell értesítést kap, amikor új cikkek.</span><span class="sxs-lookup"><span data-stu-id="f25ec-115">Subscribers should get a notification when new articles are published.</span></span>
* <span data-ttu-id="f25ec-116">Előfizetők cikkek olvasni a helyi régió képesnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f25ec-116">Subscribers must be able to read articles from their local region.</span></span> <span data-ttu-id="f25ec-117">Is kell adhat hozzá az alábbi cikkek értékelést.</span><span class="sxs-lookup"><span data-stu-id="f25ec-117">They should also be able to add reviews to these articles.</span></span> 
* <span data-ttu-id="f25ec-118">Bárki, beleértve a szerző, kattintson az alábbi cikkek cikkek csatlakozó összes értékelést egy helyi területet képes nézet kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f25ec-118">Anyone including the author of the articles should be able view all the reviews attached to articles from a local region.</span></span> 

<span data-ttu-id="f25ec-119">Feltéve, hogy a fogyasztók és a közzétevők milliárd cikkeket, több millió hamarosan tudunk skála helység hozzáférés biztosítása mellett a problémák alkalmazásra.</span><span class="sxs-lookup"><span data-stu-id="f25ec-119">Assuming millions of consumers and publishers with billions of articles, soon we have to confront the problems of scale along with guaranteeing locality of access.</span></span> <span data-ttu-id="f25ec-120">A legtöbb méretezhetőség problémát, a megoldás egy jó particionálási stratégia rejlik.</span><span class="sxs-lookup"><span data-stu-id="f25ec-120">As with most scalability problems, the solution lies in a good partitioning strategy.</span></span> <span data-ttu-id="f25ec-121">A következő modellhez tartozó cikkek, tekintse át és értesítések-dokumentumokként, Azure Cosmos DB fiókok konfigurálása és valósítja meg az adatelérési réteg vizsgáljuk meg.</span><span class="sxs-lookup"><span data-stu-id="f25ec-121">Next, let's look at how to model articles, review, and notifications as documents, configure Azure Cosmos DB accounts, and implement a data access layer.</span></span> 

<span data-ttu-id="f25ec-122">Ha azt szeretné, további információt a particionálás és partíciós kulcsok, lásd: [particionálás és az Azure Cosmos Adatbázisba skálázás](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="f25ec-122">If you would like to learn more about partitioning and partition keys, see [Partitioning and Scaling in Azure Cosmos DB](partition-data.md).</span></span>

## <span data-ttu-id="f25ec-123"><a id="ModelingNotifications"></a>Modellezési értesítések</span><span class="sxs-lookup"><span data-stu-id="f25ec-123"><a id="ModelingNotifications"></a>Modeling notifications</span></span>
<span data-ttu-id="f25ec-124">A rendszer értesítések egy felhasználó számára az adatok adott hírcsatornák.</span><span class="sxs-lookup"><span data-stu-id="f25ec-124">Notifications are data feeds specific to a user.</span></span> <span data-ttu-id="f25ec-125">Emiatt a hozzáférési minták értesítések dokumentumok lehet mindig egyetlen felhasználó környezetében.</span><span class="sxs-lookup"><span data-stu-id="f25ec-125">Therefore, the access patterns for notifications documents are always in the context of single user.</span></span> <span data-ttu-id="f25ec-126">Például akkor "értesítést közzétett egy felhasználó számára" vagy "beolvasni a megadott felhasználó összes értesítésben".</span><span class="sxs-lookup"><span data-stu-id="f25ec-126">For example, you would "post a notification to a user" or "fetch all notifications for a given user".</span></span> <span data-ttu-id="f25ec-127">Igen, az optimális választás az ilyen típusú kulcs particionálás lenne `UserId`.</span><span class="sxs-lookup"><span data-stu-id="f25ec-127">So, the optimal choice of partitioning key for this type would be `UserId`.</span></span>

    class Notification 
    { 
        // Unique ID for Notification. 
        public string Id { get; set; }

        // The user Id for which notification is addressed to. 
        public string UserId { get; set; }

        // The partition Key for the resource. 
        public string PartitionKey 
        { 
            get 
            { 
                return this.UserId; 
            }
        }

        // Subscription for which this notification is raised. 
        public string SubscriptionFilter { get; set; }

        // Subject of the notification. 
        public string ArticleId { get; set; } 
    }

## <span data-ttu-id="f25ec-128"><a id="ModelingSubscriptions"></a>Modellezési előfizetések</span><span class="sxs-lookup"><span data-stu-id="f25ec-128"><a id="ModelingSubscriptions"></a>Modeling subscriptions</span></span>
<span data-ttu-id="f25ec-129">Az előfizetések különböző feltételek, például egy adott konkrét kategóriával a cikkek, vagy egy adott publisher hozható létre.</span><span class="sxs-lookup"><span data-stu-id="f25ec-129">Subscriptions can be created for various criteria like a specific category of articles of interest, or a specific publisher.</span></span> <span data-ttu-id="f25ec-130">Ezért a `SubscriptionFilter` a partíciós kulcs jó választás.</span><span class="sxs-lookup"><span data-stu-id="f25ec-130">Hence the `SubscriptionFilter` is a good choice for partition key.</span></span>

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

## <span data-ttu-id="f25ec-131"><a id="ModelingArticles"></a>Modellezési cikkek</span><span class="sxs-lookup"><span data-stu-id="f25ec-131"><a id="ModelingArticles"></a>Modeling articles</span></span>
<span data-ttu-id="f25ec-132">Egy cikk, amelynél értesítések keresztül, amennyiben a általában alapuló lekérdezések a `Article.Id`.</span><span class="sxs-lookup"><span data-stu-id="f25ec-132">Once an article is identified through notifications, subsequent queries are typically based on the `Article.Id`.</span></span> <span data-ttu-id="f25ec-133">Kiválasztása `Article.Id` partíció a kulcs így biztosítja a legjobb terjesztési cikkek az Azure Cosmos DB gyűjteményt belül tárolásához.</span><span class="sxs-lookup"><span data-stu-id="f25ec-133">Choosing `Article.Id` as partition the key thus provides the best distribution for storing articles inside an Azure Cosmos DB collection.</span></span> 

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
        
        // Author of the article
        public string Author { get; set; }

        // Category/genre of the article
        public string Category { get; set; }

        // Tags associated with the article
        public string[] Tags { get; set; }

        // Title of the article
        public string Title { get; set; }
        
        //... 
    }

## <span data-ttu-id="f25ec-134"><a id="ModelingReviews"></a>Ellenőrzi, hogy modellezési</span><span class="sxs-lookup"><span data-stu-id="f25ec-134"><a id="ModelingReviews"></a>Modeling reviews</span></span>
<span data-ttu-id="f25ec-135">Ellenőrzések hasonló cikkek, főleg írása és cikk olvassa.</span><span class="sxs-lookup"><span data-stu-id="f25ec-135">Like articles, reviews are mostly written and read in the context of article.</span></span> <span data-ttu-id="f25ec-136">Kiválasztása `ArticleId` egy partíció kulcs biztosít az ajánlott terjesztési és hatékony hozzáférés cikk társított felülvizsgálat.</span><span class="sxs-lookup"><span data-stu-id="f25ec-136">Choosing `ArticleId` as a partition key provides best distribution and efficient access of reviews associated with article.</span></span> 

    class Review 
    { 
        // Unique ID for Review 
        public string Id { get; set; }

        // Article Id of the review 
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

## <span data-ttu-id="f25ec-137"><a id="DataAccessMethods"></a>Adatok hozzáférési réteg módszerek</span><span class="sxs-lookup"><span data-stu-id="f25ec-137"><a id="DataAccessMethods"></a>Data access layer methods</span></span>
<span data-ttu-id="f25ec-138">Most vizsgáljuk meg a fő adatok hozzáférési metódust kell megvalósítani.</span><span class="sxs-lookup"><span data-stu-id="f25ec-138">Now let's look at the main data access methods we need to implement.</span></span> <span data-ttu-id="f25ec-139">A módszerek listája itt található, amely a `ContentPublishDatabase` kell:</span><span class="sxs-lookup"><span data-stu-id="f25ec-139">Here's the list of methods that the `ContentPublishDatabase` needs:</span></span>

    class ContentPublishDatabase 
    { 
        public async Task CreateSubscriptionAsync(string userId, string category);
    
        public async Task<IEnumerable<Notification>> ReadNotificationFeedAsync(string userId);
    
        public async Task<Article> ReadArticleAsync(string articleId);
    
        public async Task WriteReviewAsync(string articleId, string userId, string reviewText, int rating);
    
        public async Task<IEnumerable<Review>> ReadReviewsAsync(string articleId); 
    }

## <span data-ttu-id="f25ec-140"><a id="Architecture"></a>Azure Cosmos DB fiók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f25ec-140"><a id="Architecture"></a>Azure Cosmos DB account configuration</span></span>
<span data-ttu-id="f25ec-141">Helyi olvas és ír, igazolnia kell particionálni adatok nem csupán a partíción kulcs, de is alapján földrajzi hozzáférési mintázatát területekre.</span><span class="sxs-lookup"><span data-stu-id="f25ec-141">To guarantee local reads and writes, we must partition data not just on partition key, but also based on the geographical access pattern into regions.</span></span> <span data-ttu-id="f25ec-142">A modell támaszkodik, amely egy georeplikált Azure Cosmos DB adatbázisfiók mindegyik régióhoz.</span><span class="sxs-lookup"><span data-stu-id="f25ec-142">The model relies on having a geo-replicated Azure Cosmos DB database account for each region.</span></span> <span data-ttu-id="f25ec-143">Például két régiókban, itt van a telepítés több területi írási műveletek esetében:</span><span class="sxs-lookup"><span data-stu-id="f25ec-143">For example, with two regions, here's a setup for multi-region writes:</span></span>

| <span data-ttu-id="f25ec-144">Fióknév</span><span class="sxs-lookup"><span data-stu-id="f25ec-144">Account Name</span></span> | <span data-ttu-id="f25ec-145">Írási régió</span><span class="sxs-lookup"><span data-stu-id="f25ec-145">Write Region</span></span> | <span data-ttu-id="f25ec-146">Olvasási régió</span><span class="sxs-lookup"><span data-stu-id="f25ec-146">Read Region</span></span> |
| --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.com` | `West US` |`North Europe` |
| `contentpubdatabase-europe.documents.azure.com` | `North Europe` |`West US` |

<span data-ttu-id="f25ec-147">Az alábbi ábrán látható, hogyan olvasási és írási történik a telepítés egy tipikus alkalmazásban:</span><span class="sxs-lookup"><span data-stu-id="f25ec-147">The following diagram shows how reads and writes are performed in a typical application with this setup:</span></span>

![Azure Cosmos DB több főkiszolgálós architektúra](./media/multi-region-writers/multi-master.png)

<span data-ttu-id="f25ec-149">Íme egy kódrészletet, amely a DAL futtatja az ügyfelekhez inicializálásával a `West US` régióban.</span><span class="sxs-lookup"><span data-stu-id="f25ec-149">Here is a code snippet showing how to initialize the clients in a DAL running in the `West US` region.</span></span>
    
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

<span data-ttu-id="f25ec-150">Az előző beállítás az adatelérési réteg továbbíthatja az összes írási műveleteket ad ki a helyi fiók, ahol központilag telepítették alapján.</span><span class="sxs-lookup"><span data-stu-id="f25ec-150">With the preceding setup, the data access layer can forward all writes to the local account based on where it is deployed.</span></span> <span data-ttu-id="f25ec-151">Olvasás a számára a globális adatokat megtekintheti a fiókot is olvasásakor hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="f25ec-151">Reads are performed by reading from both accounts to get the global view of data.</span></span> <span data-ttu-id="f25ec-152">Ez a megközelítés kiterjeszthető annyi régiók szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="f25ec-152">This approach can be extended to as many regions as required.</span></span> <span data-ttu-id="f25ec-153">Íme például egy három földrajzi régiókhoz telepítése:</span><span class="sxs-lookup"><span data-stu-id="f25ec-153">For example, here's a setup with three geographic regions:</span></span>

| <span data-ttu-id="f25ec-154">Fióknév</span><span class="sxs-lookup"><span data-stu-id="f25ec-154">Account Name</span></span> | <span data-ttu-id="f25ec-155">Írási régió</span><span class="sxs-lookup"><span data-stu-id="f25ec-155">Write Region</span></span> | <span data-ttu-id="f25ec-156">Olvassa el az 1 régió</span><span class="sxs-lookup"><span data-stu-id="f25ec-156">Read Region 1</span></span> | <span data-ttu-id="f25ec-157">Olvassa el a régió 2</span><span class="sxs-lookup"><span data-stu-id="f25ec-157">Read Region 2</span></span> |
| --- | --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.com` | `West US` |`North Europe` |`Southeast Asia` |
| `contentpubdatabase-europe.documents.azure.com` | `North Europe` |`West US` |`Southeast Asia` |
| `contentpubdatabase-asia.documents.azure.com` | `Southeast Asia` |`North Europe` |`West US` |

## <span data-ttu-id="f25ec-158"><a id="DataAccessImplementation"></a>Adatok hozzáférési réteg végrehajtása</span><span class="sxs-lookup"><span data-stu-id="f25ec-158"><a id="DataAccessImplementation"></a>Data access layer implementation</span></span>
<span data-ttu-id="f25ec-159">Most már az adatelérési réteg (DAL) az alkalmazás a két írható régió végrehajtásának vizsgáljuk meg.</span><span class="sxs-lookup"><span data-stu-id="f25ec-159">Now let's look at the implementation of the data access layer (DAL) for an application with two writable regions.</span></span> <span data-ttu-id="f25ec-160">A DAL meg kell valósítania az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="f25ec-160">The DAL must implement the following steps:</span></span>

* <span data-ttu-id="f25ec-161">Hozzon létre több példánya `DocumentClient` az egyes fiókok számára.</span><span class="sxs-lookup"><span data-stu-id="f25ec-161">Create multiple instances of `DocumentClient` for each account.</span></span> <span data-ttu-id="f25ec-162">Két régiókban, a DAL feltünteti van egy `writeClient` és egy `readClient`.</span><span class="sxs-lookup"><span data-stu-id="f25ec-162">With two regions, each DAL instance has one `writeClient` and one `readClient`.</span></span> 
* <span data-ttu-id="f25ec-163">Az alkalmazás telepített régió alapján, konfigurálja a végpontokat a `writeclient` és `readClient`.</span><span class="sxs-lookup"><span data-stu-id="f25ec-163">Based on the deployed region of the application, configure the endpoints for `writeclient` and `readClient`.</span></span> <span data-ttu-id="f25ec-164">Például a DAL telepített `West US` használ `contentpubdatabase-usa.documents.azure.com` írási műveletek végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="f25ec-164">For example, the DAL deployed in `West US` uses `contentpubdatabase-usa.documents.azure.com` for performing writes.</span></span> <span data-ttu-id="f25ec-165">A DAL telepített `NorthEurope` használ `contentpubdatabase-europ.documents.azure.com` az írásokhoz.</span><span class="sxs-lookup"><span data-stu-id="f25ec-165">The DAL deployed in `NorthEurope` uses `contentpubdatabase-europ.documents.azure.com` for writes.</span></span>

<span data-ttu-id="f25ec-166">Az előző beállítás az adatok hozzáférési metódusokat valósítható meg.</span><span class="sxs-lookup"><span data-stu-id="f25ec-166">With the preceding setup, the data access methods can be implemented.</span></span> <span data-ttu-id="f25ec-167">Írási műveletek továbbítsa a megfelelő írási `writeClient`.</span><span class="sxs-lookup"><span data-stu-id="f25ec-167">Write operations forward the write to the corresponding `writeClient`.</span></span>

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

<span data-ttu-id="f25ec-168">Értesítések és az ellenőrzések olvasásához, kell olvasási régiók és Unió az eredmények látható módon a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="f25ec-168">For reading notifications and reviews, you must read from both regions and union the results as shown in the following snippet:</span></span>

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

<span data-ttu-id="f25ec-169">Így egy jó particionálási kulcs és a statikus ügyfélalapú particionálás kiválasztásával érhető el több területi helyi írások és olvasások Azure Cosmos DB használatával.</span><span class="sxs-lookup"><span data-stu-id="f25ec-169">Thus, by choosing a good partitioning key and static account-based partitioning, you can achieve multi-region local writes and reads using Azure Cosmos DB.</span></span>

## <span data-ttu-id="f25ec-170"><a id="NextSteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f25ec-170"><a id="NextSteps"></a>Next steps</span></span>
<span data-ttu-id="f25ec-171">Ez a cikk azt leírt használatát globálisan elosztott több területi olvasási-írási minták rendelkező Azure Cosmos DB használatával tartalom közzététele egy mintaforgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="f25ec-171">In this article, we described how you can use globally distributed multi-region read write patterns with Azure Cosmos DB using content publishing as a sample scenario.</span></span>

* <span data-ttu-id="f25ec-172">További tudnivalók arról, hogyan támogatja a Azure Cosmos DB [globális terjesztési](distribute-data-globally.md)</span><span class="sxs-lookup"><span data-stu-id="f25ec-172">Learn about how Azure Cosmos DB supports [global distribution](distribute-data-globally.md)</span></span>
* <span data-ttu-id="f25ec-173">További tudnivalók [automatikus és manuális feladatátvételt az Azure Cosmos-Adatbázisba](regional-failover.md)</span><span class="sxs-lookup"><span data-stu-id="f25ec-173">Learn about [automatic and manual failovers in Azure Cosmos DB](regional-failover.md)</span></span>
* <span data-ttu-id="f25ec-174">További tudnivalók [az Azure Cosmos DB globális egységesítése](consistency-levels.md)</span><span class="sxs-lookup"><span data-stu-id="f25ec-174">Learn about [global consistency with Azure Cosmos DB](consistency-levels.md)</span></span>
* <span data-ttu-id="f25ec-175">Több régiókat fejlesztést a [Azure Cosmos DB - DocumentDB API](tutorial-global-distribution-documentdb.md)</span><span class="sxs-lookup"><span data-stu-id="f25ec-175">Develop with multiple regions using the [Azure Cosmos DB - DocumentDB API](tutorial-global-distribution-documentdb.md)</span></span>
* <span data-ttu-id="f25ec-176">Több régiókat fejlesztést a [Azure Cosmos DB - MongoDB API](tutorial-global-distribution-MongoDB.md)</span><span class="sxs-lookup"><span data-stu-id="f25ec-176">Develop with multiple regions using the [Azure Cosmos DB - MongoDB API](tutorial-global-distribution-MongoDB.md)</span></span>
* <span data-ttu-id="f25ec-177">Több régiókat fejlesztést a [Azure Cosmos DB - tábla API](tutorial-global-distribution-table.md)</span><span class="sxs-lookup"><span data-stu-id="f25ec-177">Develop with multiple regions using the [Azure Cosmos DB - Table API](tutorial-global-distribution-table.md)</span></span>
