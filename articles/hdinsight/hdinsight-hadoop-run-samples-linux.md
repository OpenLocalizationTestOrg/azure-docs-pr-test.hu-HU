---
title: "a HDInsight - Azure aaaRun Hadoop MapReduce példák |} Microsoft Docs"
description: "MapReduce-minták a HDInsight szereplő jar-fájlok az első lépéseiben. SSH tooconnect toohello fürtöt használ, és aztán hello Hadoop parancs toorun mintafeladatok."
keywords: "hadoop példa jar, a hadoop példák jar, a hadoop-mapreduce példák, a mapreduce példák"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: e1d2a0b9-1659-4fab-921e-4a8990cbb30a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: 7a16bbd51eb17570fcaa3b1e0f5990fa889c106a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-hello-mapreduce-examples-included-in-hdinsight"></a>Futtassa a HDInsight szereplő hello MapReduce példák

[!INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Ismerje meg, hogyan toorun hello MapReduce példák a HDInsight Hadoop része.

## <a name="prerequisites"></a>Előfeltételek

* **HDInsight-fürtök**: lásd: [Hadoop használatának megkezdésében a Hive HDInsight Linux rendszeren](hdinsight-hadoop-linux-tutorial-get-started.md)

    > [!IMPORTANT]
    > Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* **Egy SSH-ügyfél**: további információkért lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="hello-mapreduce-examples"></a>hello MapReduce példák

**Hely**: hello minták a HDInsight-fürtöt: hello lévő `/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar`.

**Tartalom**: a következő minták hello ebből az archívumból tartalmazza:

* `aggregatewordcount`: Egy összesítés mapreduce program, amely a bemeneti fájlok hello hello szavak száma alapján.
* `aggregatewordhist`: Egy összesítés alapján kiszámítja a bemeneti fájlok hello hello szavak hello hisztogram mapreduce program.
* `bbp`: A Pi Bailey-Borwein-Plouffe toocompute számjegyeket használó a mapreduce programot.
* `dbcount`: Egy példa a feladat, amely megjeleníti az adatbázisban tárolt hello pageview naplók.
* `distbbp`: A Pi bit pontos BBP típusú képlet toocompute használó a mapreduce programot.
* `grep`: A mapreduce program hello megadó megegyezik a regex hello bemeneti a.
* `join`: Egy feladatot, amely végrehajtja a való csatlakozást keresztül rendezve, adatkészletek egyaránt particionálva.
* `multifilewc`: Egy feladatot, amely a szavakat több fájlok száma.
* `pentomino`: A mapreduce csempe elrendezése program toofind megoldások toopentomino problémákat.
* `pi`: A mapreduce program, amely segítségével látszólagos Monte Pi becslése Carlo metódust.
* `randomtextwriter`: A mapreduce program 10 GB-nyi véletlenszerű szöveges adatok csomópontonként írja.
* `randomwriter`: A mapreduce program 10 GB-nyi véletlenszerű adatokat csomópontonként írja.
* `secondarysort`: Példa egy másodlagos rendezési toohello meghatározása csökkenti fázisban.
* `sort`: Mapreduce program hello adatok hello véletlenszerű írási szerint rendezi.
* `sudoku`: A sudoku solver.
* `teragen`: Hello terasort adatainak létrehozása.
* `terasort`: Hello terasort futtatásához.
* `teravalidate`: Terasort eredményeinek ellenőrzése.
* `wordcount`: Mapreduce program hello szavak hello bemeneti fájlok száma.
* `wordmean`: Mapreduce program hello átlagos hossza hello szavak hello bemeneti fájlok száma.
* `wordmedian`: Mapreduce program hello közepes hossza hello szavak hello bemeneti fájlok száma.
* `wordstandarddeviation`: Mapreduce program hello szórása hello hosszát hello szavak hello bemeneti fájlok száma.

**Forráskód**: HDInsight-fürtöt: hello szerepel-e ezeket a mintákat forráskódja `/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples`.

> [!NOTE]
> Hello `2.2.4.9-1` hello elérési hello verziója hello Hortonworks Data Platform hello HDInsight-fürthöz, és lehet, hogy a fürt másik.

## <a name="run-hello-wordcount-example"></a>Futtassa a hello wordcount-példa

1. Csatlakozzon az SSH használatával tooHDInsight. További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

2. A hello `username@#######:~$` kérni, használja a következő Példaparancsok toolist hello hello:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar
    ```

    Ez a parancs a dokumentum korábbi szakaszában hello minta hello listát hoz létre.

3. Használjon hello következő parancsot a tooget súgó adott mintán. Ebben az esetben hello **wordcount** minta:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount
    ```

    Hello a következő üzenet jelenik meg:

        Usage: wordcount <in> [<in>...] <out>

    Ez az üzenet azt jelzi, hogy több bemeneti elérési utak biztosíthat hello forrás dokumentumokhoz. hello végső elérési út hello kimeneti (szavak hello forrás dokumentumok száma) tárolására.

4. A következő toocount hello minden szó használjon hello notebookok a Leonardo Da Vinci, amely vannak megadva, a mintaadatok a fürthöz:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/davinciwordcount
    ```

    Ez a feladat olvasása az adatokat `/example/data/gutenberg/davinci.txt`. Az ebben a példában tárolódik kimeneti `/example/data/davinciwordcount`. Mindkét elérési utak alapértelmezett tárolási hello fürt esetén nem hello helyi fájlrendszerben található.

   > [!NOTE]
   > Amint hello wordcount minta hello súgóját, több bemeneti fájl is megadhatja. Például `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` számolja davinci.txt és ulysses.txt szavakat.

5. Amikor hello feladat befejeződik, használja a következő tooview hello parancskimenet hello:

    ```bash
    hdfs dfs -cat /example/data/davinciwordcount/*
    ```

    Ez a parancs hello feladat által létrehozott összes hello kimeneti fájlok fűzi össze. Hello kimeneti toohello konzol jeleníti meg. a kimeneti hello hasonló toohello a következő szöveget:

        zum     1
        zur     1
        zwanzig 1
        zweite  1

    Soronként egy word és hányszor történt a hello a bemeneti adatok jelöli.

## <a name="hello-sudoku-example"></a>hello Sudoku – példa

[Sudoku](https://en.wikipedia.org/wiki/Sudoku) egy logikai kirakós kilenc 3 x 3 rácsok áll. Néhány hello rács celláinak számok, amíg üres, és hello célja az üres cellák hello toosolve. hello előző hivatkozás hello Kirakós további információkat, de ez a minta hello célját toosolve hello üres cellák. A bemeneti úgy, hogy megtalálható-e a következő formátumban hello fájlnak kell lennie:

* Kilenc oszlopok kilenc sorok
* Minden oszlop tartalmazhat vagy egy szám vagy `?` (amely azt jelzi, egy üres cella)
* Cellák egymástól elválasztva

Van egy bizonyos módon tooconstruct Sudoku rejtvények; egy szám oszlop vagy sor nem ismétlődhet. Például nincs megfelelően összeállított hello HDInsight-fürthöz. Az itt található: `/usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta` és a következő szöveg hello tartalmazza:

    8 5 ? 3 9 ? ? ? ?
    ? ? 2 ? ? ? ? ? ?
    ? ? 6 ? 1 ? ? ? 2
    ? ? 4 ? ? 3 ? 5 9
    ? ? 8 9 ? 1 4 ? ?
    3 2 ? 4 ? ? 8 ? ?
    9 ? ? ? 8 ? 5 ? ?
    ? ? ? ? ? ? 2 ? ?
    ? ? ? ? 4 5 ? 7 8

toorun hello Sudoku a példában a példa probléma használja hello a következő parancsot:

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar sudoku /usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta
```

hello eredményei jelennek meg a következő szöveg hasonló toohello:

    8 5 1 3 9 2 6 4 7
    4 3 2 6 7 8 1 9 5
    7 9 6 5 1 4 3 8 2
    6 1 4 8 2 3 7 5 9
    5 7 8 9 6 1 4 2 3
    3 2 9 4 5 7 8 1 6
    9 4 7 2 8 6 5 3 1
    1 8 5 7 3 9 2 6 4
    2 6 3 1 4 5 9 7 8

## <a name="pi--example"></a>A pi (π) – Példa

hello pi mintát használ a statisztikai (látszólagos Monte Carlo) metódus tooestimate hello a pi értékét. Pontok egység négyzet véletlenszerűen kerülnek. hello szögletes kör is tartalmaz. hello valószínűsége, hogy hello pontok tartoznak hello kör hello egyenlő toohello területén kör, pi/4. a pi értékét hello megbecsülhető 4R hello értékét. R hello aránya hello száma pontok belüli hello kör toohello pontok számát, amelyek hello szögletes belül. hello hello kódtöredéket használt pontok, hello jobb hello becsült értéke.

A minta a következő parancs toorun hello használata. Ez a parancs 16 maps 10,000,000 minták tooestimate hello a pi értékét használja:

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar pi 16 10000000
```

hello Ez a parancs által visszaadott érték hasonló túl**3.14159155000000000000**. A hivatkozásokat hello pi első 10 tizedesjegyre 3.1415926535.

## <a name="10-gb-greysort-example"></a>10 GB-os Greysort – példa

GraySort teljesítményteszt rendezés. hello metrika az hello rendezési gyakoriság (TB percenként), amelyek során nagy mennyiségű adat, általában egy 100 TB-os minimális rendezés érhető el.

Ez a minta egy mérsékelt 10 GB adatot használ, így viszonylag gyorsan futtatása. Hello MapReduce alkalmazások Owen O'Malley és Arun Murthy által fejlesztett használ. Ezek az alkalmazások hello éves általános célú ("daytona") terabájt rendezési teljesítményteszt nyert 2009, sebességet 0.578 TB/perc (173 percben 100 TB). Erről és más rendezési referenciaalapok további információkért lásd: hello [Sortbenchmark](http://sortbenchmark.org/) hely.

A példa a MapReduce programok három különböző használja:

* **TeraGen**: A MapReduce program által generált adatok toosort sorát

* **TeraSort**: hello bemeneti adatokat, és használja a MapReduce toosort hello adatok egy teljes rendelés

    TeraSort szabványos MapReduce rendezési, kivéve egy egyéni particionáló. hello particionáló hello kulcs tartomány minden csökkentse definiálása mintát N-1 kulcsok rendezett listáját használja. Ebben az esetben, minden kulcsok ilyen mintát [i-1] < kulcs = < minta [i] küldött tooreduce i. Ez a particionáló garantálja, hogy hello kimenetének csökkentése i segédanyagokra-nál kisebb hello kimenete csökkentése i + 1.

* **TeraValidate**: A MapReduce program, amely ellenőrzi, hogy hello kimeneti globálisan rendezése

    Fájlonként egy leképezést hello kimeneti könyvtár hozna létre, és minden leképezés biztosítja, hogy minden kulcs kisebb vagy egyenlő, mint toohello előzőre. hello leképezés funkció hello rekordjának először hoz létre, és a kulcsok az egyes fájlok utolsó. hello csökkentése függvény biztosítja, hogy hello fájl i első kulcsát érték nagyobb, mint a fájl i-1 hello utolsó kulcsa. Problémákat jelentett, a hello kimenete csökkentése fázisban, amelyek nem megfelelő sorrendben hello kulcsokkal.

Használjon hello alábbi lépéseit toogenerate adatok rendezéséhez, és ezután ellenőrizze a hello kimeneti:

1. 10 GB adatot, amely tárolt toohello HDInsight fürt alapértelmezett tárhely létrehozása a `/example/data/10GB-sort-input`:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=50 100000000 /example/data/10GB-sort-input
    ```

    Hello `-Dmapred.map.tasks` közli Hadoop hány térkép feladatok toouse ehhez a feladathoz. utolsó két hello paraméterek utasítani hello feladat toocreate 10 GB adat és toostore a `/example/data/10GB-sort-input`.

2. A következő parancs toosort hello adatok hello használata:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-input /example/data/10GB-sort-output
    ```

    Hello `-Dmapred.reduce.tasks` közli a Hadoop számának csökkentése feladatok toouse hello feladat. utolsó két hello paraméterek csak hello bemeneti és kimeneti adatok helyét.

3. A következő hello rendezési által létrehozott toovalidate hello adatok hello használata:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-output /example/data/10GB-sort-validate
    ```

## <a name="next-steps"></a>Következő lépések

Az ebben a cikkben megtanulta, hogyan toorun hello minták mellékelt hello Linux-alapú HDInsight-fürtökön. A Pig, a Hive és a MapReduce és a HDInsight együttes használatával kapcsolatos oktatóanyagok tekintse meg a következő témakörök hello:

* [A Pig használata a HDInsight Hadoop][hdinsight-use-pig]
* [A Hive használata a hdinsight Hadoop][hdinsight-use-hive]
* [A HDInsight Hadoop MapReduce használata][hdinsight-use-mapreduce]

[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md



[hdinsight-samples]: hdinsight-run-samples.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
