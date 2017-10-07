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
# <a name="multi-master-globally-replicated-database-architectures-with-azure-cosmos-db"></a><span data-ttu-id="30272-103">Több főkiszolgálós globálisan replikált Azure Cosmos DB az adatbázis-architektúra</span><span class="sxs-lookup"><span data-stu-id="30272-103">Multi-master globally replicated database architectures with Azure Cosmos DB</span></span>
<span data-ttu-id="30272-104">Azure Cosmos DB támogatja kulcsrakész [globális replikációs](distribute-data-globally.md), amely toomultiple adatterületek toodistribute kis késésű hozzáférést hello munkaterhelés a bárhol lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="30272-104">Azure Cosmos DB supports turnkey [global replication](distribute-data-globally.md), which allows you toodistribute data toomultiple regions with low latency access anywhere in hello workload.</span></span> <span data-ttu-id="30272-105">Ebben a modellben van általánosan használt publisher/fogyasztói ahol egy-egy földrajzi régiót íróhoz és egyéb (olvasás) régiókban globálisan elosztott olvasók van.</span><span class="sxs-lookup"><span data-stu-id="30272-105">This model is commonly used for publisher/consumer workloads where there is a writer in a single geographic region and globally distributed readers in other (read) regions.</span></span> 

<span data-ttu-id="30272-106">Használhatja az Azure Cosmos DB globális replikációs toobuild alkalmazások támogatását, amelyben írók és olvasók globálisan fel vannak osztva.</span><span class="sxs-lookup"><span data-stu-id="30272-106">You can also use Azure Cosmos DB's global replication support toobuild applications in which writers and readers are globally distributed.</span></span> <span data-ttu-id="30272-107">Ez a dokumentum egy mintát, amely lehetővé teszi a helyi írási és olvasási helyi hozzáférési elérése az Azure Cosmos DB használatával elosztott íróhoz ismertet.</span><span class="sxs-lookup"><span data-stu-id="30272-107">This document outlines a pattern that enables achieving local write and local read access for distributed writers using Azure Cosmos DB.</span></span>

## <span data-ttu-id="30272-108"><a id="ExampleScenario"></a>Tartalom közzététele - bemutató példa</span><span class="sxs-lookup"><span data-stu-id="30272-108"><a id="ExampleScenario"></a>Content Publishing - an example scenario</span></span>
<span data-ttu-id="30272-109">Vizsgáljuk meg olyan valós forgatókönyv toodescribe használatát globálisan elosztott több-region/több-főkulcs olvasási írási minták rendelkező Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="30272-109">Let's look at a real world scenario toodescribe how you can use globally distributed multi-region/multi-master read write patterns with Azure Cosmos DB.</span></span> <span data-ttu-id="30272-110">Fontolja meg a közzétételi tartalomplatform Azure Cosmos DB épül.</span><span class="sxs-lookup"><span data-stu-id="30272-110">Consider a content publishing platform built on Azure Cosmos DB.</span></span> <span data-ttu-id="30272-111">Az alábbiakban olyan követelményekkel, amelyeket ezen a platformon meg kell felelnie a kiváló felhasználói élmény a közzétevők és a fogyasztók.</span><span class="sxs-lookup"><span data-stu-id="30272-111">Here are some requirements that this platform must meet for a great user experience for both publishers and consumers.</span></span>

* <span data-ttu-id="30272-112">Szerzők és a előfizetők eloszlatva hello world</span><span class="sxs-lookup"><span data-stu-id="30272-112">Both authors and subscribers are spread over hello world</span></span> 
* <span data-ttu-id="30272-113">Szerzők közzé kell tennie (írás) cikkek tootheir helyi (legközelebbi) régió</span><span class="sxs-lookup"><span data-stu-id="30272-113">Authors must publish (write) articles tootheir local (closest) region</span></span>
* <span data-ttu-id="30272-114">Szerzők olvasók/előfizetők a cikkek felvételéhez hello földgömb elosztott rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="30272-114">Authors have readers/subscribers of their articles who are distributed across hello globe.</span></span> 
* <span data-ttu-id="30272-115">Előfizetők kell értesítést kap, amikor új cikkek.</span><span class="sxs-lookup"><span data-stu-id="30272-115">Subscribers should get a notification when new articles are published.</span></span>
* <span data-ttu-id="30272-116">Előfizetők képes tooread cikkeket a helyi régióban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="30272-116">Subscribers must be able tooread articles from their local region.</span></span> <span data-ttu-id="30272-117">Képes tooadd értékelést toothese cikkek is kell.</span><span class="sxs-lookup"><span data-stu-id="30272-117">They should also be able tooadd reviews toothese articles.</span></span> 
* <span data-ttu-id="30272-118">Bárki, beleértve a hello cikkek hello Szerző képes nézet az összes értékelést csatolt tooarticles egy helyi területről hello kell lennie.</span><span class="sxs-lookup"><span data-stu-id="30272-118">Anyone including hello author of hello articles should be able view all hello reviews attached tooarticles from a local region.</span></span> 

<span data-ttu-id="30272-119">Feltéve, hogy a fogyasztók és a közzétevők milliárd cikkeket, több millió hamarosan tudunk tooconfront hello problémák skála helység hozzáférés biztosítása mellett.</span><span class="sxs-lookup"><span data-stu-id="30272-119">Assuming millions of consumers and publishers with billions of articles, soon we have tooconfront hello problems of scale along with guaranteeing locality of access.</span></span> <span data-ttu-id="30272-120">Csakúgy, mint a legtöbb méretezhetőség problémát hello megoldás egy jó particionálási stratégia rejlik.</span><span class="sxs-lookup"><span data-stu-id="30272-120">As with most scalability problems, hello solution lies in a good partitioning strategy.</span></span> <span data-ttu-id="30272-121">A következő most hogyan toomodel cikkeket, tekintse át és -dokumentumokként, értesítések konfigurálása Azure Cosmos DB fiókok meg, és megvalósítja az adatelérési réteg.</span><span class="sxs-lookup"><span data-stu-id="30272-121">Next, let's look at how toomodel articles, review, and notifications as documents, configure Azure Cosmos DB accounts, and implement a data access layer.</span></span> 

<span data-ttu-id="30272-122">Ha szeretne további particionálása és partíciókulcsok toolearn, [particionálás és az Azure Cosmos Adatbázisba skálázás](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="30272-122">If you would like toolearn more about partitioning and partition keys, see [Partitioning and Scaling in Azure Cosmos DB](partition-data.md).</span></span>

## <span data-ttu-id="30272-123"><a id="ModelingNotifications"></a>Modellezési értesítések</span><span class="sxs-lookup"><span data-stu-id="30272-123"><a id="ModelingNotifications"></a>Modeling notifications</span></span>
<span data-ttu-id="30272-124">Értesítések adatok hírcsatornák adott tooa felhasználókként szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="30272-124">Notifications are data feeds specific tooa user.</span></span> <span data-ttu-id="30272-125">Emiatt hello hozzáférési minták értesítések dokumentumok lehet mindig egyetlen felhasználó hello környezetében.</span><span class="sxs-lookup"><span data-stu-id="30272-125">Therefore, hello access patterns for notifications documents are always in hello context of single user.</span></span> <span data-ttu-id="30272-126">Például akkor "közzétett egy értesítési tooa felhasználó" vagy "beolvasni a megadott felhasználó összes értesítésben".</span><span class="sxs-lookup"><span data-stu-id="30272-126">For example, you would "post a notification tooa user" or "fetch all notifications for a given user".</span></span> <span data-ttu-id="30272-127">Igen, a particionálás kulcs ehhez a típushoz a optimális választás hello `UserId`.</span><span class="sxs-lookup"><span data-stu-id="30272-127">So, hello optimal choice of partitioning key for this type would be `UserId`.</span></span>

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

## <span data-ttu-id="30272-128"><a id="ModelingSubscriptions"></a>Modellezési előfizetések</span><span class="sxs-lookup"><span data-stu-id="30272-128"><a id="ModelingSubscriptions"></a>Modeling subscriptions</span></span>
<span data-ttu-id="30272-129">Az előfizetések különböző feltételek, például egy adott konkrét kategóriával a cikkek, vagy egy adott publisher hozható létre.</span><span class="sxs-lookup"><span data-stu-id="30272-129">Subscriptions can be created for various criteria like a specific category of articles of interest, or a specific publisher.</span></span> <span data-ttu-id="30272-130">Ezért hello `SubscriptionFilter` a partíciós kulcs jó választás.</span><span class="sxs-lookup"><span data-stu-id="30272-130">Hence hello `SubscriptionFilter` is a good choice for partition key.</span></span>

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

## <span data-ttu-id="30272-131"><a id="ModelingArticles"></a>Modellezési cikkek</span><span class="sxs-lookup"><span data-stu-id="30272-131"><a id="ModelingArticles"></a>Modeling articles</span></span>
<span data-ttu-id="30272-132">Miután egy cikk, amelynél értesítések keresztül, lekérdezések általában alapján hello `Article.Id`.</span><span class="sxs-lookup"><span data-stu-id="30272-132">Once an article is identified through notifications, subsequent queries are typically based on hello `Article.Id`.</span></span> <span data-ttu-id="30272-133">Kiválasztása `Article.Id` partíció hello kulcs így biztosít hello ajánlott terjesztési cikkek az Azure Cosmos DB gyűjteményt belül tárolásához.</span><span class="sxs-lookup"><span data-stu-id="30272-133">Choosing `Article.Id` as partition hello key thus provides hello best distribution for storing articles inside an Azure Cosmos DB collection.</span></span> 

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

## <span data-ttu-id="30272-134"><a id="ModelingReviews"></a>Ellenőrzi, hogy modellezési</span><span class="sxs-lookup"><span data-stu-id="30272-134"><a id="ModelingReviews"></a>Modeling reviews</span></span>
<span data-ttu-id="30272-135">Ellenőrzések hasonló cikkek, főleg írása és hello környezetében cikkben olvasható.</span><span class="sxs-lookup"><span data-stu-id="30272-135">Like articles, reviews are mostly written and read in hello context of article.</span></span> <span data-ttu-id="30272-136">Kiválasztása `ArticleId` egy partíció kulcs biztosít az ajánlott terjesztési és hatékony hozzáférés cikk társított felülvizsgálat.</span><span class="sxs-lookup"><span data-stu-id="30272-136">Choosing `ArticleId` as a partition key provides best distribution and efficient access of reviews associated with article.</span></span> 

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

## <span data-ttu-id="30272-137"><a id="DataAccessMethods"></a>Adatok hozzáférési réteg módszerek</span><span class="sxs-lookup"><span data-stu-id="30272-137"><a id="DataAccessMethods"></a>Data access layer methods</span></span>
<span data-ttu-id="30272-138">Most nézzük hello fő adatok tooimplement szükséges hozzáférési metódust.</span><span class="sxs-lookup"><span data-stu-id="30272-138">Now let's look at hello main data access methods we need tooimplement.</span></span> <span data-ttu-id="30272-139">Hello listája hello módszerek `ContentPublishDatabase` kell:</span><span class="sxs-lookup"><span data-stu-id="30272-139">Here's hello list of methods that hello `ContentPublishDatabase` needs:</span></span>

    class ContentPublishDatabase 
    { 
        public async Task CreateSubscriptionAsync(string userId, string category);
    
        public async Task<IEnumerable<Notification>> ReadNotificationFeedAsync(string userId);
    
        public async Task<Article> ReadArticleAsync(string articleId);
    
        public async Task WriteReviewAsync(string articleId, string userId, string reviewText, int rating);
    
        public async Task<IEnumerable<Review>> ReadReviewsAsync(string articleId); 
    }

## <span data-ttu-id="30272-140"><a id="Architecture"></a>Azure Cosmos DB fiók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="30272-140"><a id="Architecture"></a>Azure Cosmos DB account configuration</span></span>
<span data-ttu-id="30272-141">helyi tooguarantee olvas és ír, azt kell particionálni nem csak a partíciós kulcs adatokat, de is hello földrajzi hozzáférési szabály alapján területekre.</span><span class="sxs-lookup"><span data-stu-id="30272-141">tooguarantee local reads and writes, we must partition data not just on partition key, but also based on hello geographical access pattern into regions.</span></span> <span data-ttu-id="30272-142">hello modell támaszkodik, amely egy georeplikált Azure Cosmos DB adatbázisfiók mindegyik régióhoz.</span><span class="sxs-lookup"><span data-stu-id="30272-142">hello model relies on having a geo-replicated Azure Cosmos DB database account for each region.</span></span> <span data-ttu-id="30272-143">Például két régiókban, itt van a telepítés több területi írási műveletek esetében:</span><span class="sxs-lookup"><span data-stu-id="30272-143">For example, with two regions, here's a setup for multi-region writes:</span></span>

| <span data-ttu-id="30272-144">Fiók neve</span><span class="sxs-lookup"><span data-stu-id="30272-144">Account Name</span></span> | <span data-ttu-id="30272-145">Írási régió</span><span class="sxs-lookup"><span data-stu-id="30272-145">Write Region</span></span> | <span data-ttu-id="30272-146">Olvasási régió</span><span class="sxs-lookup"><span data-stu-id="30272-146">Read Region</span></span> |
| --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.com` | `West US` |`North Europe` |
| `contentpubdatabase-europe.documents.azure.com` | `North Europe` |`West US` |

<span data-ttu-id="30272-147">a következő ábra azt mutatja be, hogyan történik meg ezzel a beállítással egy tipikus alkalmazás olvasási és írási hello:</span><span class="sxs-lookup"><span data-stu-id="30272-147">hello following diagram shows how reads and writes are performed in a typical application with this setup:</span></span>

![Azure Cosmos DB több főkiszolgálós architektúra](./media/multi-region-writers/multi-master.png)

<span data-ttu-id="30272-149">Íme egy kódrészletet, amely a hogyan tooinitialize hello a DAL hello futó ügyfelek `West US` régióban.</span><span class="sxs-lookup"><span data-stu-id="30272-149">Here is a code snippet showing how tooinitialize hello clients in a DAL running in hello `West US` region.</span></span>
    
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

<span data-ttu-id="30272-150">A telepítő megelőző hello hello az adatelérési réteg továbbíthatja az összes írások toohello helyi fiók alapján, ahol központilag telepítették.</span><span class="sxs-lookup"><span data-stu-id="30272-150">With hello preceding setup, hello data access layer can forward all writes toohello local account based on where it is deployed.</span></span> <span data-ttu-id="30272-151">Olvasási hajtja végre a következő mindkét fiókok tooget hello globális nézet adatainak olvasásakor.</span><span class="sxs-lookup"><span data-stu-id="30272-151">Reads are performed by reading from both accounts tooget hello global view of data.</span></span> <span data-ttu-id="30272-152">Ez a megközelítés lehet kiterjesztett tooas sok terület szükséges.</span><span class="sxs-lookup"><span data-stu-id="30272-152">This approach can be extended tooas many regions as required.</span></span> <span data-ttu-id="30272-153">Íme például egy három földrajzi régiókhoz telepítése:</span><span class="sxs-lookup"><span data-stu-id="30272-153">For example, here's a setup with three geographic regions:</span></span>

| <span data-ttu-id="30272-154">Fiók neve</span><span class="sxs-lookup"><span data-stu-id="30272-154">Account Name</span></span> | <span data-ttu-id="30272-155">Írási régió</span><span class="sxs-lookup"><span data-stu-id="30272-155">Write Region</span></span> | <span data-ttu-id="30272-156">Olvassa el az 1 régió</span><span class="sxs-lookup"><span data-stu-id="30272-156">Read Region 1</span></span> | <span data-ttu-id="30272-157">Olvassa el a régió 2</span><span class="sxs-lookup"><span data-stu-id="30272-157">Read Region 2</span></span> |
| --- | --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.com` | `West US` |`North Europe` |`Southeast Asia` |
| `contentpubdatabase-europe.documents.azure.com` | `North Europe` |`West US` |`Southeast Asia` |
| `contentpubdatabase-asia.documents.azure.com` | `Southeast Asia` |`North Europe` |`West US` |

## <span data-ttu-id="30272-158"><a id="DataAccessImplementation"></a>Adatok hozzáférési réteg végrehajtása</span><span class="sxs-lookup"><span data-stu-id="30272-158"><a id="DataAccessImplementation"></a>Data access layer implementation</span></span>
<span data-ttu-id="30272-159">Most hello az adatelérési réteg (DAL) az alkalmazás két írható régió hello végrehajtásának vizsgáljuk meg.</span><span class="sxs-lookup"><span data-stu-id="30272-159">Now let's look at hello implementation of hello data access layer (DAL) for an application with two writable regions.</span></span> <span data-ttu-id="30272-160">hello DAL meg kell valósítania az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="30272-160">hello DAL must implement hello following steps:</span></span>

* <span data-ttu-id="30272-161">Hozzon létre több példánya `DocumentClient` az egyes fiókok számára.</span><span class="sxs-lookup"><span data-stu-id="30272-161">Create multiple instances of `DocumentClient` for each account.</span></span> <span data-ttu-id="30272-162">Két régiókban, a DAL feltünteti van egy `writeClient` és egy `readClient`.</span><span class="sxs-lookup"><span data-stu-id="30272-162">With two regions, each DAL instance has one `writeClient` and one `readClient`.</span></span> 
* <span data-ttu-id="30272-163">Hello alkalmazás telepített hello régió alapján, konfigurálja a hello végpontjainak `writeclient` és `readClient`.</span><span class="sxs-lookup"><span data-stu-id="30272-163">Based on hello deployed region of hello application, configure hello endpoints for `writeclient` and `readClient`.</span></span> <span data-ttu-id="30272-164">Például a DAL hello telepített `West US` használ `contentpubdatabase-usa.documents.azure.com` írási műveletek végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="30272-164">For example, hello DAL deployed in `West US` uses `contentpubdatabase-usa.documents.azure.com` for performing writes.</span></span> <span data-ttu-id="30272-165">a hello DAL rendszerbe `NorthEurope` használ `contentpubdatabase-europ.documents.azure.com` az írásokhoz.</span><span class="sxs-lookup"><span data-stu-id="30272-165">hello DAL deployed in `NorthEurope` uses `contentpubdatabase-europ.documents.azure.com` for writes.</span></span>

<span data-ttu-id="30272-166">A telepítő megelőző hello hello adatok hozzáférési metódusokat valósítható meg.</span><span class="sxs-lookup"><span data-stu-id="30272-166">With hello preceding setup, hello data access methods can be implemented.</span></span> <span data-ttu-id="30272-167">Írási műveletek továbbítsa hello írási toohello megfelelő `writeClient`.</span><span class="sxs-lookup"><span data-stu-id="30272-167">Write operations forward hello write toohello corresponding `writeClient`.</span></span>

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

<span data-ttu-id="30272-168">Értesítések és az ellenőrzések olvasásához, kell olvasási régiók és a "union" hello eredmények ahogy az alábbi részlet hello:</span><span class="sxs-lookup"><span data-stu-id="30272-168">For reading notifications and reviews, you must read from both regions and union hello results as shown in hello following snippet:</span></span>

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

<span data-ttu-id="30272-169">Így egy jó particionálási kulcs és a statikus ügyfélalapú particionálás kiválasztásával érhető el több területi helyi írások és olvasások Azure Cosmos DB használatával.</span><span class="sxs-lookup"><span data-stu-id="30272-169">Thus, by choosing a good partitioning key and static account-based partitioning, you can achieve multi-region local writes and reads using Azure Cosmos DB.</span></span>

## <span data-ttu-id="30272-170"><a id="NextSteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="30272-170"><a id="NextSteps"></a>Next steps</span></span>
<span data-ttu-id="30272-171">Ez a cikk azt leírt használatát globálisan elosztott több területi olvasási-írási minták rendelkező Azure Cosmos DB használatával tartalom közzététele egy mintaforgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="30272-171">In this article, we described how you can use globally distributed multi-region read write patterns with Azure Cosmos DB using content publishing as a sample scenario.</span></span>

* <span data-ttu-id="30272-172">További tudnivalók arról, hogyan támogatja a Azure Cosmos DB [globális terjesztési](distribute-data-globally.md)</span><span class="sxs-lookup"><span data-stu-id="30272-172">Learn about how Azure Cosmos DB supports [global distribution](distribute-data-globally.md)</span></span>
* <span data-ttu-id="30272-173">További tudnivalók [automatikus és manuális feladatátvételt az Azure Cosmos-Adatbázisba](regional-failover.md)</span><span class="sxs-lookup"><span data-stu-id="30272-173">Learn about [automatic and manual failovers in Azure Cosmos DB](regional-failover.md)</span></span>
* <span data-ttu-id="30272-174">További tudnivalók [az Azure Cosmos DB globális egységesítése](consistency-levels.md)</span><span class="sxs-lookup"><span data-stu-id="30272-174">Learn about [global consistency with Azure Cosmos DB](consistency-levels.md)</span></span>
* <span data-ttu-id="30272-175">Több régiókat hello fejlesztést [Azure Cosmos DB - DocumentDB API](tutorial-global-distribution-documentdb.md)</span><span class="sxs-lookup"><span data-stu-id="30272-175">Develop with multiple regions using hello [Azure Cosmos DB - DocumentDB API](tutorial-global-distribution-documentdb.md)</span></span>
* <span data-ttu-id="30272-176">Több régiókat hello fejlesztést [Azure Cosmos DB - MongoDB API](tutorial-global-distribution-MongoDB.md)</span><span class="sxs-lookup"><span data-stu-id="30272-176">Develop with multiple regions using hello [Azure Cosmos DB - MongoDB API](tutorial-global-distribution-MongoDB.md)</span></span>
* <span data-ttu-id="30272-177">Több régiókat hello fejlesztést [Azure Cosmos DB - tábla API](tutorial-global-distribution-table.md)</span><span class="sxs-lookup"><span data-stu-id="30272-177">Develop with multiple regions using hello [Azure Cosmos DB - Table API](tutorial-global-distribution-table.md)</span></span>
