---
title: "Mahout és-(SSH) HDInsight - Azure aaaGenerate javaslatok |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Apache Mahout gépi tanulási a szalagtár toogenerate movie ajánlások a hdinsight (Hadoop) eszközzel."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c78ec37c-9a8c-4bb6-9e38-0bdb9e89fbd7
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: larryfr
ms.openlocfilehash: fedac9ceb4268f8421bce4623a5ad271041b8b3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="generate-movie-recommendations-by-using-apache-mahout-with-linux-based-hadoop-in-hdinsight-ssh"></a>Film javaslatok generálása Apache Mahout Linux-alapú hadooppal a HDInsight-(SSH) használatával

[!INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Megtudhatja, hogyan toouse hello [Apache Mahout](http://mahout.apache.org) machine learning függvénytár Azure HDInsight toogenerate movie javaslatokat.

Mahout van egy [gépi tanulás] [ ml] könyvtára Apache Hadoop. Mahout adatok, például a szűrést, besorolást, és a fürtszolgáltatás feldolgozására algoritmusok tartalmazza. Ebben a cikkben egy javaslat motor toogenerate movie javaslatok az ismerősök láthatta filmek alapuló használja.

## <a name="prerequisites"></a>Előfeltételek

* A Linux-alapú HDInsight-fürtöt. Egy létrehozásával kapcsolatos további információkért lásd: [hdinsight Linux-alapú Hadoop használatának megkezdésében][getstarted].

> [!IMPORTANT]
> Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Egy SSH-ügyfél. További információkért lásd: hello [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentum.

## <a name="mahout-versioning"></a>Mahout versioning

Hdinsight Mahout hello verziójával kapcsolatos további információkért lásd: [HDInsight verziója és a Hadoop-összetevők](hdinsight-component-versioning.md).

## <a name="recommendations"></a>Understanding javaslatok

Mahout által biztosított hello függvények egyike egy javaslat motor. Ez a motor hello formátumban adatokat fogad `userID`, `itemId`, és `prefValue` (hello hello elem preferencia). Mahout végezhet el, közös elemként elemzés toodetermine: *egy elem előnyben rendelkező felhasználók is hozzáférhetnek ezeket a beállításokat szabályozó egyéb elemek*. Mahout majd határozza meg a felhasználók hasonló-cikk beállítások, amelyek használt toomake javaslatok is.

hello következő munkafolyamat egy egyszerűsített példában látható, amely movie adatait használja:

* **Közös elemként**: Joe, Ágnes és minden tetszését Bob *csillag ütközések*, *Empire sztrájkok vissza hello*, és *Return a hello Jedi*. Mahout határozza meg, hogy a felhasználók, akik például ezek filmek egyikét sem is, például más két hello.

* **Közös elemként**: Bálint és Alice is tetszett *látszólagos támadása hello*, *támadás hello klónja*, és *a hello Sith megtorlás*. Mahout határozza meg, hogy felhasználók, akik hello előző három filmek is tetszett, például a három filmek.

* **Hasonlóság ajánlás**: mivel Joe tetszését hello első három filmek, Mahout ellenőrzi, hogy az filmek, hogy más, hasonló beállítások tetszett, de Joe rendelkezik nem figyelt (tetszését/névleges). Ebben az esetben Mahout javasolja *látszólagos támadása hello*, *támadás hello klónja*, és *a hello Sith megtorlás*.

### <a name="understanding-hello-data"></a>Hello adatok ismertetése

Kényelmesen [GroupLens kutatási] [ movielens] minősítés adatokat biztosít a filmek formátuma nem kompatibilis a Mahout. Ezek az adatok érhető el a fürt alapértelmezett tárolás `/HdiSamples/HdiSamples/MahoutMovieData`.

Két fájlt `moviedb.txt` és `user-ratings.txt`. hello felhasználói-ratings.txt fájllal elemzés, amíg moviedb.txt használt tooprovide felhasználóbarát szöveges információ hello elemzés eredményeinek hello megjelenítésekor.

hello felhasználói-ratings.txt szereplő adatok rendelkezik egy szerkezete `userID`, `movieID`, `userRating`, és `timestamp`, amely közli a gép magas hogyan minden felhasználó besorolású film. Íme egy példa a hello adatok:

    196    242    3    881250949
    186    302    3    891717742
    22    377    1    878887116
    244    51    2    880606923
    166    346    1    886397596

## <a name="run-hello-analysis"></a>Hello futtatása

SSH kapcsolat toohello fürtök a következő parancs toorun hello javaslat feladat hello használata:

```bash
mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt -o /example/data/mahoutout --tempDir /temp/mahouttemp
```

> [!NOTE]
> hello feladat eltarthat néhány percig toocomplete, és előfordulhat, hogy több MapReduce-feladatok futtatásához.

## <a name="view-hello-output"></a>Nézet hello kimeneti

1. Amikor hello feladat befejeződik, használja a következő tooview generált hello parancskimenet hello:

    ```bash
    hdfs dfs -text /example/data/mahoutout/part-r-00000
    ```

    hello kimenete a következőképpen jelenik meg:

        1    [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2    [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3    [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4    [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    hello első oszlopa hello `userID`. hello található értékek ' ["és"] "vannak `movieId`:`recommendationScore`.

2. Hello kimeneti hello moviedb.txt, tooprovide együtt további információt használhatja hello ajánlása. Először létre kell toocopy hello fájlokat a helyileg hello a következő parancsokat:

    ```bash
    hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt
    hdfs dfs -get /HdiSamples/HdiSamples/MahoutMovieData/* .
    ```

    Ez a parancs másolatok hello kimeneti tooa adatfájlt nevű **recommendations.txt** hello aktuális könyvtárban, hello movie adatfájlok együtt.

3. A következő parancs toocreate egy Python-parancsfájl, amely megkeresi az movie nevek hello javaslatok kimeneti hello adatainak hello használata:

    ```bash
    nano show_recommendations.py
    ```

    Hello szerkesztő megnyitása után szöveg hello hello fájl tartalmát, a következő hello használata:

   ```python
   #!/usr/bin/env python

   import sys

   if len(sys.argv) != 5:
        print "Arguments: userId userDataFilename movieFilename recommendationFilename"
        sys.exit(1)

   userId, userDataFilename, movieFilename, recommendationFilename = sys.argv[1:]

   print "Reading Movies Descriptions"
   movieFile = open(movieFilename)
   movieById = {}
   for line in movieFile:
       tokens = line.split("|")
       movieById[tokens[0]] = tokens[1:]
   movieFile.close()

   print "Reading Rated Movies"
   userDataFile = open(userDataFilename)
   ratedMovieIds = []
   for line in userDataFile:
       tokens = line.split("\t")
       if tokens[0] == userId:
           ratedMovieIds.append((tokens[1],tokens[2]))
   userDataFile.close()

   print "Reading Recommendations"
   recommendationFile = open(recommendationFilename)
   recommendations = []
   for line in recommendationFile:
       tokens = line.split("\t")
       if tokens[0] == userId:
           movieIdAndScores = tokens[1].strip("[]\n").split(",")
           recommendations = [ movieIdAndScore.split(":") for movieIdAndScore in movieIdAndScores ]
           break
   recommendationFile.close()

   print "Rated Movies"
   print "------------------------"
   for movieId, rating in ratedMovieIds:
       print "%s, rating=%s" % (movieById[movieId][0], rating)
   print "------------------------"

   print "Recommended Movies"
   print "------------------------"
   for movieId, score in recommendations:
       print "%s, score=%s" % (movieById[movieId][0], score)
   print "------------------------"
   ```

    Nyomja le az **Ctrl-X**, **Y**, és végül **Enter** toosave hello adatokat.

4. Hello Python-parancsfájl futtatása. hello alábbi parancs feltételezi, hogy hello könyvtárban, ahol az összes hello fájl letöltése:

    ```bash
    python show_recommendations.py 4 user-ratings.txt moviedb.txt recommendations.txt
    ```

    Ez a parancs ellenőrzi, hogy az hello javaslatok a felhasználói azonosító 4 jön létre.

    * Hello **felhasználói-ratings.txt** fájl értékelése használt tooretrieve filmek.

    * Hello **moviedb.txt** fájl hello filmek használt tooretrieve hello nevét.

    * Hello **recommendations.txt** használt tooretrieve hello movie javaslatok a felhasználó van.

     a kimeneti hello ezt a parancsot a rendszer hasonló toohello a következő szöveget:

        Tibet (1997), a hét éven pontozása = 5.0 Indiana János és hello utolsó Crusade (1989) pontozása = 5.0 Jaws (1975) pontszám = 5.0 logika és Sensibility (1995) pontszám = 5.0 függetlenség napja (ID4) (1996) pontszám = 5.0 a legjobb barátjának Esküvői (1997), pontozása = 5.0 Jerry Maguire (1996 ), pontszám = 5.0 Scream 2 (1997) pontszám = 5.0 idő tooKill, egy (1996), pontozása = 5.0

## <a name="delete-temporary-data"></a>Ideiglenes adatok törlése

Mahout feladatok hello feladat feldolgozása közben létrehozott ideiglenes adatok nem távolítja el. Hello `--tempDir` paraméter megadott hello példa tooisolate hello ideiglenes feladatfájlokhoz könnyen törlésre megadott elérési útra. tooremove hello ideiglenes fájlok, a következő parancs hello használata:

```bash
hdfs dfs -rm -f -r /temp/mahouttemp
```

> [!WARNING]
> Ha újra toorun hello parancs, hello kimeneti könyvtár is törölnie kell. Használja a következő toodelete hello ebben a könyvtárban:
>
> `hdfs dfs -rm -f -r /example/data/mahoutout`


## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta, hogyan toouse Mahout, felderíteni a más módon a HDInsight-adatokkal:

* [A HDInsight Hive](hdinsight-use-hive.md)
* [Pig with HDInsight](hdinsight-use-pig.md)
* [MapReduce a hdinsight eszközzel](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
