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
# <a name="determine-twitter-trending-topics-with-apache-storm-on-hdinsight"></a>Határozza meg a HDInsight alatt futó Apache Storm Twitter trendekkel kapcsolatos témakörök

Ismerje meg, hogyan toouse Trident toocreate a Storm-topológia, amely meghatározza, hogy trendek témakörök (kivonatoló címkék) a Twitteren.

Trident egy magas szintű absztrakció, amely biztosítja az eszközöket, például a illesztéseket, összesítéseket, csoportosítás, funkciók és szűrőket. Trident emellett primitívek az állapotalapú, a növekményes feldolgozás során. Ez a dokumentum hello példában a Trident-topológia egyéni spout és függvény. Több, a Trident által biztosított beépített funkciók is használja.

## <a name="requirements"></a>Követelmények

* <a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Java és hello JDK 1.8</a>

* <a href="http://maven.apache.org/what-is-maven.html" target="_blank">Maven 3</a>

* <a href="http://git-scm.com/" target="_blank">Git</a>

* Twitter-fejlesztői fiók létrehozása

## <a name="download-hello-project"></a>Hello projekt letöltése

A következő kód tooclone hello projekt helyi hello használata.

    git clone https://github.com/Blackmist/TwitterTrending

## <a name="understanding-hello-topology"></a>Hello topológia ismertetése

hello következő ábra azt mutatja, hogyan adatáramlás ebben a topológiában keresztül:

![topology](./media/hdinsight-storm-twitter-trending/trident.png)

> [!NOTE]
> Ez a diagram hello topológia egyszerűsített nézete. Hello összetevők több példánya elosztott hello hello fürt csomópontja.


hello hello topológia megvalósító Trident kódot a következőképpen történik:

    topology.newStream("spout", spout)
        .each(new Fields("tweet"), new HashtagExtractor(), new Fields("hashtag"))
        .groupBy(new Fields("hashtag"))
        .persistentAggregate(new MemoryMapState.Factory(), new Count(), new Fields("count"))
        .newValuesStream()
        .applyAssembly(new FirstN(10, "count"))
        .each(new Fields("hashtag", "count"), new Debug());

Ez a kód hello a következő műveleteket hajtja végre:

1. Hoz létre hello spout. hello spout Twitter-üzeneteket lekéri a Twitter, és a szűrők őket meghatározott kulcsszavak (szerelem, zene és kávé ebben a példában).

2. HashtagExtractor, egy egyéni függvény használt tooextract kivonatoló címkék az egyes tweetet. hello kivonatoló címke található kibocsátott toohello adatfolyam.

3. hello adatfolyam kivonatoló címke szerint csoportosítva, és tooan gyűjtő átadott. Ebbe a gyűjtőbe hoz létre az egyes kivonatoló címkék történt hány alkalommal számát. Ezek az adatok a memóriában megőrződjenek. Végül egy új adatfolyam kibocsátott, amely tartalmazza a hello kivonatoló címke és hello száma.

4. Hello **FirstN** szerelvény alkalmazott tooreturn hello első 10 értékek kizárólag, hello száma alapján.

> [!NOTE]
> A Trident munkáról bővebben lásd: hello [Trident API – áttekintés](http://storm.apache.org/releases/current/Trident-API-Overview.html) dokumentum.

### <a name="hello-spout"></a>hello spout

hello spout **TwitterSpout**, használja a [Twitter4j](http://twitter4j.org/en/) tooretrieve Twitter-üzeneteket a Twitteren. Hello szavak is létrejön __kedvelt__, **zene**, és **kávé**. Bejövő Twitter-üzeneteket (állapot) hello szűrőnek csatolt blokkoló várólista vannak tárolva. Végezetül elemek hello várólista ki vannak lekért és kibocsátott toohello topológia.

### <a name="hello-hashtagextractor"></a>hello HashtagExtractor

tooextract kivonatoló címkék, [getHashtagEntities](http://twitter4j.org/javadoc/twitter4j/EntitySupport.html#getHashtagEntities--) van használt tooretrieve hello tweetet található összes kivonatoló címkét. Ezek azok majd kibocsátott toohello adatfolyam.

## <a name="configure-twitter"></a>Twitter konfigurálása

A következő lépéseket tooregister új Twitter-alkalmazás hello használja, és ezt hello fogyasztói, valamint a hozzáférési token fordítás tooread Twitter:

1. Nyissa meg túl[Twitter alkalmazások](https://apps.twitter.com) hello kattintson **hozzon létre új alkalmazás** gombra. Amikor hello űrlap kitöltése, hagyja hello **visszahívási URL-cím** mező üres.

2. Amikor hello alkalmazást hoz létre, kattintson a hello **kulcsok és a hozzáférési jogkivonatok** fülre.

3. Másolás hello **kulcsa** és **felhasználói titok** információkat.

4. Hello hello lap alsó részén, jelölje ki a **a hozzáférési jogkivonat létrehozása** nem jogkivonatok esetén. Ha hello jogkivonatok létrejött, a Másolás hello **Access Token** és **Access Token titkos** információkat.

5. A hello **TwitterSpoutTopology** projektre, hogy korábban klónozott, nyissa meg hello **resources/twitter4j.properties** fájlt, adja hozzá az összegyűjtött információkat felhasználva hello hello előző lépésekben, és mentse a hello fájl .

## <a name="build-hello-topology"></a>Build hello topológia

A következő kód toobuild hello projekt hello használata:

        cd [directoryname]
        mvn compile

## <a name="test-hello-topology"></a>Teszt hello topológia

A következő parancs tootest hello topológia helyileg hello használata:

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.TwitterTrendingTopology

Hello topológia elindulása után meg kell jelennie a hibakeresési információ hello kivonatoló tartalmazó címkéket és hello topológia kibocsátott száma. hello kimeneti megjelenjen-e a következő szöveg hasonló toohello:

    DEBUG: [Quicktellervalentine, 7]
    DEBUG: [GRAMMYs, 7]
    DEBUG: [AskSam, 7]
    DEBUG: [poppunk, 1]
    DEBUG: [rock, 1]
    DEBUG: [punkrock, 1]
    DEBUG: [band, 1]
    DEBUG: [punk, 1]
    DEBUG: [indonesiapunkrock, 1]

## <a name="next-steps"></a>Következő lépések

Most, hogy hello topológia helyileg tesztelt, hogyan toodeploy hello topológia felderítése: [központi telepítése és kezelése a HDInsight alatt futó Apache Storm-topológiák](hdinsight-storm-deploy-monitor-topology.md).

Akkor is érdeklődik hello Storm témakörök a következő:

* [Java-topológiák fejlesztése alatt futó Storm példatopológiái Maven használatával](hdinsight-storm-develop-java-topology.md)
* [Visual Studio használatával HDInsight alatt futó Storm a C#-topológiák fejlesztése](hdinsight-storm-develop-csharp-visual-studio-topology.md)

További a HDInsight alatt futó Storm példákat:

* [HDInsight alatt futó Storm példatopológiái](hdinsight-storm-example-topology.md)

