---
title: "IntelliJ Hortonworks védőfal rendelkező Azure eszköztára aaaUse |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse HDInsight eszközei Azure eszközkészlet a Hortonworks védőfal az intellij-t."
keywords: "hadoop-eszközök hive lekérdezés, az intellij, hortonworks védőfal, intellij azure eszköztára"
services: HDInsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: b587cc9b-a41a-49ac-998f-b54d6c0bdfe0
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 2bf97068a9cec99fcc7b4ff9469b91d8cbe2a8d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hdinsight-tools-for-intellij-with-hortonworks-sandbox"></a>Használja a HDInsight Tools for IntelliJ a Hortonworks védőfal

Ismerje meg, hogyan toouse a HDInsight Tools for IntelliJ toodevelop Apache Scala-alkalmazások és végül pedig tesztelheti hello az alkalmazások [Hortonworks védőfal](http://hortonworks.com/products/sandbox/) fut a munkaállomáson. 

[IntelliJ IDEA](https://www.jetbrains.com/idea/) fejlesztési számítógép szoftver a Java-integrált fejlesztési környezeti (IDE) van. Kifejlesztett és tesztelt Hortonworks a(z) az alkalmazások, után áthelyezheti őket túl[Azure HDInsight](hdinsight-hadoop-introduction.md).

## <a name="prerequisites"></a>Előfeltételek

Az oktatóanyag elindításának feltétele:

- Hortonworks Data Platform (HDP) 2.4 a helyi környezetben futó Hortonworks a(z). tooconfigure HDP, lásd: [megismerheti a hello Hadoop ökoszisztémájának a virtuális gépen Hadoop védőfalat](hdinsight-hadoop-emulator-get-started.md). 
    >[!NOTE]
    >A HDInsight Tools for IntelliJ csak HDP 2.4 tesztelték. tooget HDP 2.4, bontsa ki a **Hortonworks védőfal archív** a hello [Hortonworks védőfal tölti le a hely](http://hortonworks.com/downloads/#sandbox).

- [Java fejlesztői készlet (JDK) 1,8 vagy újabb verzió](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html). JDK az IntelliJ hello Azure eszközkészlet használata szükséges.

- [IntelliJ IDEA community edition](https://www.jetbrains.com/idea/download) a hello [Scala](https://plugins.jetbrains.com/idea/plugin/1347-scala) beépülő modul és hello [IntelliJ Azure eszköztára](../azure-toolkit-for-intellij.md) beépülő modult. A HDInsight Tools for IntelliJ hello Azure eszköztára IntelliJ részeként érhető el. 

  tooinstall hello beépülő modulok, hello a következő:

  1. Nyissa meg az IntelliJ IDEA.
  2. A hello **üdvözlő** képernyőn válassza ki **konfigurálása**, majd válassza ki **beépülő modulok**.
  3. Válassza ki **JetBrains telepítése beépülő modul** hello bal alsó sarokban.
  4. Használjon hello keresési funkció toosearch a **Scala**, majd válassza ki **telepítése**.
  5. Válassza ki **indítsa újra az IntelliJ IDEA** toocomplete hello telepítését.
  6. 4. és 5 megismétli tooinstall hello **IntelliJ Azure eszköztára**. További információkért lásd: [telepítés hello Azure eszköztára IntelliJ](../azure-toolkit-for-intellij-installation.md).

## <a name="create-a-spark-scala-application"></a>Spark Scala-alkalmazás létrehozása

Ebben a szakaszban IntelliJ IDEA használatával hoz létre egy minta Scala-projektet. A következő szakaszban hello csatolunk IntelliJ IDEA toohello Hortonworks védőfal (emulátor) hello projekt mentése előtt.

1. Nyissa meg az IntelliJ IDEA a munkaállomáson. A hello **új projekt** párbeszédpanel mezőbe hello a következő:

   a. Válassza ki **HDInsight** > **a Spark on HDInsight (Scala)**.

   b. A hello **Build eszköz** listára, válassza ki a következő, tooyour szükség szerint hello valamelyikét:

    * **Maven**, Scala-projekt létrehozása varázsló támogatásához
    * **SBT**, hello függőségek kezelése és hello Scala-projekt létrehozása

   ![hello új projekt párbeszédpanel](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project.png)

2. Válassza ki **következő**.

3. Hello a következő **új projekt** párbeszédpanel mezőbe hello a következő:

    ![Az IntelliJ Scala projekt Tulajdonságok létrehozása](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project-properties.png)

    a. A hello **projektnevet** mezőbe írja be a projekt nevét.

    b. A hello **projekt** mezőbe írja be a projekt helyére.

    c. Következő toohello **projekt SDK** legördülő listában válassza **új**, jelölje be **JDK**, majd adja meg a hello mappában található Java JDK 1.7 vagy újabb verziója. Válassza ki **Java 1.8** hello Spark 2.x fürt, vagy válassza ki a **Java 1.7** hello Spark 1.x fürthöz. hello alapértelmezett hely: C:\Program Files\Java\jdk1.8.x_xxx.

    d. A hello **Spark verzió** legördülő listából válassza ki, Scala-projekt létrehozása varázsló hello a verzió megfelelőségének integrálja a Spark SDK és a Scala SDK. Ha hello Spark-fürt verziója korábbi, mint 2,0, válassza ki a **Spark 1.x**. Máskülönben válassza **Spark2.x**. A példa Spark 1.6.2 (Scala 2.10.5). Ellenőrizze, hogy a megjelölt Scala hello tárház használt 2.10.x. Ne használjon Scala megjelölve hello tárház 2.11.x.

4. Válassza a **Finish** (Befejezés) elemet.

5. Ha hello **projekt** nézet még nincs megnyitva, nyomja le az ENTER **Alt + 1** tooopen azt.

6. A **Project Explorer**, bontsa ki hello projektet, és válassza **src**.

7. Kattintson a jobb gombbal **src**, pont túl**új**, majd válassza ki **Scala osztály**.

8. A hello **neve** mezőbe írjon be egy nevet, a hello **jellegű** mezőben válassza **objektum**, majd válassza ki **OK**.

    ![hello új Scala osztály létrehozása ablak](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-new-scala-class.png)

9. Hello .scala fájlban illessze be a kódját a következő hello:

        import java.util.Random
        import org.apache.spark.{SparkConf, SparkContext}
        import org.apache.spark.SparkContext._

        /**
        * Usage: GroupByTest [numMappers] [numKVPairs] [valSize] [numReducers]
        */
        object GroupByTest {
            def main(args: Array[String]) {
                val sparkConf = new SparkConf().setAppName("GroupBy Test")
                var numMappers = 3
                var numKVPairs = 10
                var valSize = 10
                var numReducers = 2

                val sc = new SparkContext(sparkConf)

                val pairs1 = sc.parallelize(0 until numMappers, numMappers).flatMap { p =>
                val ranGen = new Random
                var arr1 = new Array[(Int, Array[Byte])](numKVPairs)
                for (i <- 0 until numKVPairs) {
                    val byteArr = new Array[Byte](valSize)
                    ranGen.nextBytes(byteArr)
                    arr1(i) = (ranGen.nextInt(Int.MaxValue), byteArr)
                }
                arr1
                }.cache
                // Enforce that everything has been calculated and in cache
                pairs1.count

                println(pairs1.groupByKey(numReducers).count)
            }
        }

10. A hello **Build** menüjében válassza **Build projekt**. Győződjön meg arról, hogy hello fordítási sikeresen befejeződött.


## <a name="link-toohello-hortonworks-sandbox"></a>Toohello Hortonworks védőfal hivatkozás

Mielőtt tooa Hortonworks védőfal (emulátor) is kapcsolhat, egy meglévő IntelliJ alkalmazást kell rendelkeznie.

toolink tooan emulátor, a következő hello:

1. Az IntelliJ projekt megnyitása hello.

2. A hello **nézet** menüjében válassza **eszközök Windows**, majd válassza ki **Azure Explorer**.

3. Bontsa ki a **Azure**, kattintson a jobb gombbal **HDInsight**, majd válassza ki **hivatkozásra az emulátor**.

4. A hello **hivatkozásra egy új emulátor** ablakban adja meg, hogy Ön hello Hortonworks védőfal hello legfelső szintű fiókjához beállított, adja meg a következő képernyőkép hello értékek hasonló toothose, majd válassza ki a hello jelszót **OK** . 

   ![hello "Hivatkozásra egy új emulátor" ablak](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-link-an-emulator.png)

5. tooconfigure hello emulátor, jelölje be **Igen**.

Ha hello emulátor sikeresen csatlakoztatva van, hello emulátor (Hortonworks védőfal) hello HDInsight csomópont megjelenik.

## <a name="submit-hello-spark-scala-application-toohello-hortonworks-sandbox"></a>Hello Spark Scala alkalmazás toohello Hortonworks védőfal elküldése

Miután a IntelliJ IDEA toohello emulátor, elküldheti a projekthez.

a projekt tooan emulátor toosubmit hello a következő:

1. A **Project Explorer**, kattintson a jobb gombbal a hello projektet, és válassza **küldje el a külső alkalmazás tooHDInsight**.

2. A következő hello:

    a. A hello **Spark-fürt (csak Linux)** legördülő listára, válassza ki a helyi Hortonworks védőfal.

    b. A hello **fő osztálynév** mezőben válassza ki vagy hello fő osztály neve. Ebben az oktatóanyagban hello értéke **GroupByTest**.

3. Válassza ki **nyújt**. hello küldésének feladatnaplóit hello Spark küldésének eszköz ablakban láthatók.

## <a name="next-steps"></a>Következő lépések

- Hogyan toocreate Spark-alkalmazások HDInsight az intellij-t, a HDInsight Tools használatával nyissa meg túl toolearn[használata a HDInsight Tools az IntelliJ toocreate Spark-alkalmazások HDInsight Spark Linux-fürt Azure eszköztára](hdinsight-apache-spark-intellij-tool-plugin.md).

- a HDInsight Tools for IntelliJ, videó toowatch lépjen túl[a HDInsight Tools bevezetni az IntelliJ Spark fejlesztési](https://www.youtube.com/watch?v=YTZzYVgut6c).

- toolearn hogyan toodebug Spark-alkalmazások használatával távolról a hdinsight platformon keresztül SSH-eszközkészlet hello lépjen túl[távolról az IntelliJ SSH-n keresztül a Azure eszközkészlet a HDInsight-fürtök a Spark-alkalmazások hibakeresését](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md).

- toolearn toodebug Spark-alkalmazások használatával hello eszközkészlet távolról a hdinsight platformon keresztül VPN-profilok, hogyan lépjen túl[használata a HDInsight Tools az IntelliJ toodebug Spark-alkalmazások HDInsight Spark Linux fürtön távolról Azure eszköztára](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md).

- Hogyan lépjen az Eclipse toocreate egy Spark-alkalmazást, a HDInsight Tools toouse túl toolearn[használata a HDInsight Tools az Eclipse toocreate Spark-alkalmazások Azure eszköztára](hdinsight-apache-spark-eclipse-tool-plugin.md).

- a HDInsight Tools for eclipse-ben videó toowatch lépjen túl[HDInsight eszköz használata az Eclipse toocreate Spark-alkalmazások](https://mix.office.com/watch/1rau2mopb6fha).
