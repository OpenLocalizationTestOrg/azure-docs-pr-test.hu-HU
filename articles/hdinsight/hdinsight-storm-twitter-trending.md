---
title: "a HDInsight alatt futó Apache Storm aaaTwitter trendekkel kapcsolatos témakörök |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Trident toocreate határozza meg, hogy a Twitteren trendekkel kapcsolatos témakörök az Apache Storm-topológia alapján hashtageket."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 63b280ea-5c07-4dc8-a35f-dccf5a96ba93
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/14/2017
ms.author: larryfr
ms.openlocfilehash: 0281b495d10833c63868b36856c96369b139c553
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="determine-twitter-trending-topics-with-apache-storm-on-hdinsight"></a><span data-ttu-id="c0406-103">Határozza meg a HDInsight alatt futó Apache Storm Twitter trendekkel kapcsolatos témakörök</span><span class="sxs-lookup"><span data-stu-id="c0406-103">Determine Twitter trending topics with Apache Storm on HDInsight</span></span>

<span data-ttu-id="c0406-104">Ismerje meg, hogyan toouse Trident toocreate a Storm-topológia, amely meghatározza, hogy trendek témakörök (kivonatoló címkék) a Twitteren.</span><span class="sxs-lookup"><span data-stu-id="c0406-104">Learn how toouse Trident toocreate a Storm topology that determines trending topics (hash tags) on Twitter.</span></span>

<span data-ttu-id="c0406-105">Trident egy magas szintű absztrakció, amely biztosítja az eszközöket, például a illesztéseket, összesítéseket, csoportosítás, funkciók és szűrőket.</span><span class="sxs-lookup"><span data-stu-id="c0406-105">Trident is a high-level abstraction that provides tools such as joins, aggregations, grouping, functions, and filters.</span></span> <span data-ttu-id="c0406-106">Trident emellett primitívek az állapotalapú, a növekményes feldolgozás során.</span><span class="sxs-lookup"><span data-stu-id="c0406-106">Additionally, Trident adds primitives for doing stateful, incremental processing.</span></span> <span data-ttu-id="c0406-107">Ez a dokumentum hello példában a Trident-topológia egyéni spout és függvény.</span><span class="sxs-lookup"><span data-stu-id="c0406-107">hello example used in this document is a Trident topology with a custom spout and function.</span></span> <span data-ttu-id="c0406-108">Több, a Trident által biztosított beépített funkciók is használja.</span><span class="sxs-lookup"><span data-stu-id="c0406-108">It also uses several built-in functions provided by Trident.</span></span>

## <a name="requirements"></a><span data-ttu-id="c0406-109">Követelmények</span><span class="sxs-lookup"><span data-stu-id="c0406-109">Requirements</span></span>

* <span data-ttu-id="c0406-110"><a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Java és hello JDK 1.8</a></span><span class="sxs-lookup"><span data-stu-id="c0406-110"><a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Java and hello JDK 1.8</a></span></span>

* <span data-ttu-id="c0406-111"><a href="http://maven.apache.org/what-is-maven.html" target="_blank">Maven 3</a></span><span class="sxs-lookup"><span data-stu-id="c0406-111"><a href="http://maven.apache.org/what-is-maven.html" target="_blank">Maven</a></span></span>

* <span data-ttu-id="c0406-112"><a href="http://git-scm.com/" target="_blank">Git</a></span><span class="sxs-lookup"><span data-stu-id="c0406-112"><a href="http://git-scm.com/" target="_blank">Git</a></span></span>

* <span data-ttu-id="c0406-113">Twitter-fejlesztői fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="c0406-113">A Twitter developer account</span></span>

## <a name="download-hello-project"></a><span data-ttu-id="c0406-114">Hello projekt letöltése</span><span class="sxs-lookup"><span data-stu-id="c0406-114">Download hello project</span></span>

<span data-ttu-id="c0406-115">A következő kód tooclone hello projekt helyi hello használata.</span><span class="sxs-lookup"><span data-stu-id="c0406-115">Use hello following code tooclone hello project locally.</span></span>

    git clone https://github.com/Blackmist/TwitterTrending

## <a name="understanding-hello-topology"></a><span data-ttu-id="c0406-116">Hello topológia ismertetése</span><span class="sxs-lookup"><span data-stu-id="c0406-116">Understanding hello topology</span></span>

<span data-ttu-id="c0406-117">hello következő ábra azt mutatja, hogyan adatáramlás ebben a topológiában keresztül:</span><span class="sxs-lookup"><span data-stu-id="c0406-117">hello following diagram shows of how data flows through this topology:</span></span>

![topology](./media/hdinsight-storm-twitter-trending/trident.png)

> [!NOTE]
> <span data-ttu-id="c0406-119">Ez a diagram hello topológia egyszerűsített nézete.</span><span class="sxs-lookup"><span data-stu-id="c0406-119">This diagram is a simplified view of hello topology.</span></span> <span data-ttu-id="c0406-120">Hello összetevők több példánya elosztott hello hello fürt csomópontja.</span><span class="sxs-lookup"><span data-stu-id="c0406-120">Multiple instances of hello components are distributed across hello nodes in hello cluster.</span></span>


<span data-ttu-id="c0406-121">hello hello topológia megvalósító Trident kódot a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="c0406-121">hello Trident code that implements hello topology is as follows:</span></span>

    topology.newStream("spout", spout)
        .each(new Fields("tweet"), new HashtagExtractor(), new Fields("hashtag"))
        .groupBy(new Fields("hashtag"))
        .persistentAggregate(new MemoryMapState.Factory(), new Count(), new Fields("count"))
        .newValuesStream()
        .applyAssembly(new FirstN(10, "count"))
        .each(new Fields("hashtag", "count"), new Debug());

<span data-ttu-id="c0406-122">Ez a kód hello a következő műveleteket hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="c0406-122">This code performs hello following actions:</span></span>

1. <span data-ttu-id="c0406-123">Hoz létre hello spout.</span><span class="sxs-lookup"><span data-stu-id="c0406-123">Creates a stream from hello spout.</span></span> <span data-ttu-id="c0406-124">hello spout Twitter-üzeneteket lekéri a Twitter, és a szűrők őket meghatározott kulcsszavak (szerelem, zene és kávé ebben a példában).</span><span class="sxs-lookup"><span data-stu-id="c0406-124">hello spout retrieves tweets from Twitter, and filters them for specific keywords (love, music, and coffee in this example).</span></span>

2. <span data-ttu-id="c0406-125">HashtagExtractor, egy egyéni függvény használt tooextract kivonatoló címkék az egyes tweetet.</span><span class="sxs-lookup"><span data-stu-id="c0406-125">HashtagExtractor, a custom function, is used tooextract hash tags from each tweet.</span></span> <span data-ttu-id="c0406-126">hello kivonatoló címke található kibocsátott toohello adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="c0406-126">hello hash tags are emitted toohello stream.</span></span>

3. <span data-ttu-id="c0406-127">hello adatfolyam kivonatoló címke szerint csoportosítva, és tooan gyűjtő átadott.</span><span class="sxs-lookup"><span data-stu-id="c0406-127">hello stream is grouped by hash tag, and passed tooan aggregator.</span></span> <span data-ttu-id="c0406-128">Ebbe a gyűjtőbe hoz létre az egyes kivonatoló címkék történt hány alkalommal számát.</span><span class="sxs-lookup"><span data-stu-id="c0406-128">This aggregator creates a count of how many times each hash tag has occurred.</span></span> <span data-ttu-id="c0406-129">Ezek az adatok a memóriában megőrződjenek.</span><span class="sxs-lookup"><span data-stu-id="c0406-129">This data is persisted in memory.</span></span> <span data-ttu-id="c0406-130">Végül egy új adatfolyam kibocsátott, amely tartalmazza a hello kivonatoló címke és hello száma.</span><span class="sxs-lookup"><span data-stu-id="c0406-130">Finally, a new stream is emitted that contains hello hash tag and hello count.</span></span>

4. <span data-ttu-id="c0406-131">Hello **FirstN** szerelvény alkalmazott tooreturn hello első 10 értékek kizárólag, hello száma alapján.</span><span class="sxs-lookup"><span data-stu-id="c0406-131">hello **FirstN** assembly is applied tooreturn only hello top 10 values, based on hello count field.</span></span>

> [!NOTE]
> <span data-ttu-id="c0406-132">A Trident munkáról bővebben lásd: hello [Trident API – áttekintés](http://storm.apache.org/releases/current/Trident-API-Overview.html) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="c0406-132">For more information on working with Trident, see hello [Trident API overview](http://storm.apache.org/releases/current/Trident-API-Overview.html) document.</span></span>

### <a name="hello-spout"></a><span data-ttu-id="c0406-133">hello spout</span><span class="sxs-lookup"><span data-stu-id="c0406-133">hello spout</span></span>

<span data-ttu-id="c0406-134">hello spout **TwitterSpout**, használja a [Twitter4j](http://twitter4j.org/en/) tooretrieve Twitter-üzeneteket a Twitteren.</span><span class="sxs-lookup"><span data-stu-id="c0406-134">hello spout, **TwitterSpout**, uses [Twitter4j](http://twitter4j.org/en/) tooretrieve tweets from Twitter.</span></span> <span data-ttu-id="c0406-135">Hello szavak is létrejön __kedvelt__, **zene**, és **kávé**.</span><span class="sxs-lookup"><span data-stu-id="c0406-135">A filter is created for hello words __love__, **music**, and **coffee**.</span></span> <span data-ttu-id="c0406-136">Bejövő Twitter-üzeneteket (állapot) hello szűrőnek csatolt blokkoló várólista vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="c0406-136">Incoming tweets (status) that match hello filter are stored in a linked blocking queue.</span></span> <span data-ttu-id="c0406-137">Végezetül elemek hello várólista ki vannak lekért és kibocsátott toohello topológia.</span><span class="sxs-lookup"><span data-stu-id="c0406-137">Finally, items are pulled off hello queue and emitted toohello topology.</span></span>

### <a name="hello-hashtagextractor"></a><span data-ttu-id="c0406-138">hello HashtagExtractor</span><span class="sxs-lookup"><span data-stu-id="c0406-138">hello HashtagExtractor</span></span>

<span data-ttu-id="c0406-139">tooextract kivonatoló címkék, [getHashtagEntities](http://twitter4j.org/javadoc/twitter4j/EntitySupport.html#getHashtagEntities--) van használt tooretrieve hello tweetet található összes kivonatoló címkét.</span><span class="sxs-lookup"><span data-stu-id="c0406-139">tooextract hash tags, [getHashtagEntities](http://twitter4j.org/javadoc/twitter4j/EntitySupport.html#getHashtagEntities--) is used tooretrieve all hash tags that are contained in hello tweet.</span></span> <span data-ttu-id="c0406-140">Ezek azok majd kibocsátott toohello adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="c0406-140">These are then emitted toohello stream.</span></span>

## <a name="configure-twitter"></a><span data-ttu-id="c0406-141">Twitter konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c0406-141">Configure Twitter</span></span>

<span data-ttu-id="c0406-142">A következő lépéseket tooregister új Twitter-alkalmazás hello használja, és ezt hello fogyasztói, valamint a hozzáférési token fordítás tooread Twitter:</span><span class="sxs-lookup"><span data-stu-id="c0406-142">Use hello following steps tooregister a new Twitter application and obtain hello consumer and access token information needed tooread from Twitter:</span></span>

1. <span data-ttu-id="c0406-143">Nyissa meg túl[Twitter alkalmazások](https://apps.twitter.com) hello kattintson **hozzon létre új alkalmazás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c0406-143">Go too[Twitter Apps](https://apps.twitter.com) and click hello **Create new app** button.</span></span> <span data-ttu-id="c0406-144">Amikor hello űrlap kitöltése, hagyja hello **visszahívási URL-cím** mező üres.</span><span class="sxs-lookup"><span data-stu-id="c0406-144">When filling in hello form, leave hello **Callback URL** field empty.</span></span>

2. <span data-ttu-id="c0406-145">Amikor hello alkalmazást hoz létre, kattintson a hello **kulcsok és a hozzáférési jogkivonatok** fülre.</span><span class="sxs-lookup"><span data-stu-id="c0406-145">When hello app is created, click hello **Keys and Access Tokens** tab.</span></span>

3. <span data-ttu-id="c0406-146">Másolás hello **kulcsa** és **felhasználói titok** információkat.</span><span class="sxs-lookup"><span data-stu-id="c0406-146">Copy hello **Consumer Key** and **Consumer Secret** information.</span></span>

4. <span data-ttu-id="c0406-147">Hello hello lap alsó részén, jelölje ki a **a hozzáférési jogkivonat létrehozása** nem jogkivonatok esetén.</span><span class="sxs-lookup"><span data-stu-id="c0406-147">At hello bottom of hello page, select **Create my access token** if no tokens exist.</span></span> <span data-ttu-id="c0406-148">Ha hello jogkivonatok létrejött, a Másolás hello **Access Token** és **Access Token titkos** információkat.</span><span class="sxs-lookup"><span data-stu-id="c0406-148">When hello tokens have been created, copy hello **Access Token** and **Access Token Secret** information.</span></span>

5. <span data-ttu-id="c0406-149">A hello **TwitterSpoutTopology** projektre, hogy korábban klónozott, nyissa meg hello **resources/twitter4j.properties** fájlt, adja hozzá az összegyűjtött információkat felhasználva hello hello előző lépésekben, és mentse a hello fájl .</span><span class="sxs-lookup"><span data-stu-id="c0406-149">In hello **TwitterSpoutTopology** project you previously cloned, open hello **resources/twitter4j.properties** file, add hello information you gathered in hello previous steps, and then save hello file.</span></span>

## <a name="build-hello-topology"></a><span data-ttu-id="c0406-150">Build hello topológia</span><span class="sxs-lookup"><span data-stu-id="c0406-150">Build hello topology</span></span>

<span data-ttu-id="c0406-151">A következő kód toobuild hello projekt hello használata:</span><span class="sxs-lookup"><span data-stu-id="c0406-151">Use hello following code toobuild hello project:</span></span>

        cd [directoryname]
        mvn compile

## <a name="test-hello-topology"></a><span data-ttu-id="c0406-152">Teszt hello topológia</span><span class="sxs-lookup"><span data-stu-id="c0406-152">Test hello topology</span></span>

<span data-ttu-id="c0406-153">A következő parancs tootest hello topológia helyileg hello használata:</span><span class="sxs-lookup"><span data-stu-id="c0406-153">Use hello following command tootest hello topology locally:</span></span>

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.TwitterTrendingTopology

<span data-ttu-id="c0406-154">Hello topológia elindulása után meg kell jelennie a hibakeresési információ hello kivonatoló tartalmazó címkéket és hello topológia kibocsátott száma.</span><span class="sxs-lookup"><span data-stu-id="c0406-154">After hello topology starts, you should see debug information that contains hello hash tags and counts emitted by hello topology.</span></span> <span data-ttu-id="c0406-155">hello kimeneti megjelenjen-e a következő szöveg hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="c0406-155">hello output should appear similar toohello following text:</span></span>

    DEBUG: [Quicktellervalentine, 7]
    DEBUG: [GRAMMYs, 7]
    DEBUG: [AskSam, 7]
    DEBUG: [poppunk, 1]
    DEBUG: [rock, 1]
    DEBUG: [punkrock, 1]
    DEBUG: [band, 1]
    DEBUG: [punk, 1]
    DEBUG: [indonesiapunkrock, 1]

## <a name="next-steps"></a><span data-ttu-id="c0406-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c0406-156">Next steps</span></span>

<span data-ttu-id="c0406-157">Most, hogy hello topológia helyileg tesztelt, hogyan toodeploy hello topológia felderítése: [központi telepítése és kezelése a HDInsight alatt futó Apache Storm-topológiák](hdinsight-storm-deploy-monitor-topology.md).</span><span class="sxs-lookup"><span data-stu-id="c0406-157">Now that you have tested hello topology locally, discover how toodeploy hello topology: [Deploy and manage Apache Storm topologies on HDInsight](hdinsight-storm-deploy-monitor-topology.md).</span></span>

<span data-ttu-id="c0406-158">Akkor is érdeklődik hello Storm témakörök a következő:</span><span class="sxs-lookup"><span data-stu-id="c0406-158">You may also be interested in hello following Storm topics:</span></span>

* [<span data-ttu-id="c0406-159">Java-topológiák fejlesztése alatt futó Storm példatopológiái Maven használatával</span><span class="sxs-lookup"><span data-stu-id="c0406-159">Develop Java topologies for Storm on HDInsight using Maven</span></span>](hdinsight-storm-develop-java-topology.md)
* [<span data-ttu-id="c0406-160">Visual Studio használatával HDInsight alatt futó Storm a C#-topológiák fejlesztése</span><span class="sxs-lookup"><span data-stu-id="c0406-160">Develop C# topologies for Storm on HDInsight using Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)

<span data-ttu-id="c0406-161">További a HDInsight alatt futó Storm példákat:</span><span class="sxs-lookup"><span data-stu-id="c0406-161">For more Storm examples for HDInsight:</span></span>

* [<span data-ttu-id="c0406-162">HDInsight alatt futó Storm példatopológiái</span><span class="sxs-lookup"><span data-stu-id="c0406-162">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)

