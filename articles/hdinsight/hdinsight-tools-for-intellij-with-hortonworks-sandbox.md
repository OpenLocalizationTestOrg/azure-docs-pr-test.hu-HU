---
title: "Azure eszközkészlet használata a Hortonworks védőfal IntelliJ |} Microsoft Docs"
description: "Megtudhatja, hogyan használhat HDInsight eszközöket az Azure-eszközkészlet a Hortonworks védőfal az intellij-t."
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
ms.openlocfilehash: c49f185db5a035f70a711bf309b973182d94a2b0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-hdinsight-tools-for-intellij-with-hortonworks-sandbox"></a>Használja a HDInsight Tools for IntelliJ a Hortonworks védőfal

A HDInsight Tools for IntelliJ használata Apache Scala-alkalmazások fejlesztéséhez és tesztelje az alkalmazásokat a [Hortonworks védőfal](http://hortonworks.com/products/sandbox/) fut a munkaállomáson. 

[IntelliJ IDEA](https://www.jetbrains.com/idea/) fejlesztési számítógép szoftver a Java-integrált fejlesztési környezeti (IDE) van. Kifejlesztett és tesztelt Hortonworks a(z) az alkalmazások, után áthelyezheti őket [Azure HDInsight](hdinsight-hadoop-introduction.md).

## <a name="prerequisites"></a>Előfeltételek

Az oktatóanyag elindításának feltétele:

- Hortonworks Data Platform (HDP) 2.4 a helyi környezetben futó Hortonworks a(z). HDP konfigurálásához lásd: [Ismerkedés a Hadoop ökoszisztémájának a virtuális gépen Hadoop védőfalat](hdinsight-hadoop-emulator-get-started.md). 
    >[!NOTE]
    >A HDInsight Tools for IntelliJ csak HDP 2.4 tesztelték. Ahhoz, hogy HDP 2.4, bontsa ki a **Hortonworks védőfal archív** a a [Hortonworks védőfal tölti le a hely](http://hortonworks.com/downloads/#sandbox).

- [Java fejlesztői készlet (JDK) 1,8 vagy újabb verzió](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html). JDK az intellij-t az Azure-eszközkészlet használata szükséges.

- [IntelliJ IDEA community edition](https://www.jetbrains.com/idea/download) rendelkező a [Scala](https://plugins.jetbrains.com/idea/plugin/1347-scala) beépülő modul és a [IntelliJ Azure eszköztára](../azure-toolkit-for-intellij.md) beépülő modult. A HDInsight Tools for IntelliJ az intellij-t Azure eszköztára részeként érhető el. 

  A beépülő modulok telepítéséhez tegye a következőket:

  1. Nyissa meg az IntelliJ IDEA.
  2. Az a **üdvözlő** képernyőn válassza ki **konfigurálása**, majd válassza ki **beépülő modulok**.
  3. Válassza ki **JetBrains telepítése beépülő modul** bal alsó sarokban.
  4. A keresési funkció segítségével kereshet **Scala**, majd válassza ki **telepítése**.
  5. Válassza ki **indítsa újra az IntelliJ IDEA** a telepítés befejezéséhez.
  6. 4. és 5 telepítéséhez megismétli a **IntelliJ Azure eszköztára**. További információkért lásd: [az intellij-t az Azure-eszközkészlet telepítése](../azure-toolkit-for-intellij-installation.md).

## <a name="create-a-spark-scala-application"></a>Spark Scala-alkalmazás létrehozása

Ebben a szakaszban IntelliJ IDEA használatával hoz létre egy minta Scala-projektet. A következő szakaszban kapcsolhat IntelliJ IDEA a Hortonworks védőfal (emulátor) a projekt mentése előtt.

1. Nyissa meg az IntelliJ IDEA a munkaállomáson. Az a **új projekt** párbeszédpanelen tegye a következőket:

   a. Válassza ki **HDInsight** > **a Spark on HDInsight (Scala)**.

   b. Az a **Build eszköz** listára, válassza ki, az igényeknek megfelelően az alábbiak valamelyikét:

    * **Maven**, Scala-projekt létrehozása varázsló támogatásához
    * **SBT**, a függőségek kezelésére, és a Scala-projekt létrehozása

   ![Az új projekt párbeszédpanel](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project.png)

2. Válassza ki **következő**.

3. A következő **új projekt** párbeszédpanelen tegye a következőket:

    ![Az IntelliJ Scala projekt Tulajdonságok létrehozása](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project-properties.png)

    a. Az a **projektnevet** mezőbe írja be a projekt nevét.

    b. Az a **projekt** mezőbe írja be a projekt helyére.

    c. Mellett a **projekt SDK** legördülő listában válassza **új**, jelölje be **JDK**, és adja meg a mappát, a Java JDK 1.7 vagy újabb verziója. Válassza ki **Java 1.8** a Spark-fürt 2.x, vagy válassza ki a **Java 1.7** a Spark 1.x fürthöz. Az alapértelmezett hely: C:\Program Files\Java\jdk1.8.x_xxx.

    d. Az a **Spark verzió** legördülő listából válassza ki, Scala-projekt létrehozása varázsló Spark SDK és Scala SDK integrálja a megfelelő verzióját. Ha a Spark-fürt verziója korábbi, mint 2,0, válassza ki a **Spark 1.x**. Máskülönben válassza **Spark2.x**. A példa Spark 1.6.2 (Scala 2.10.5). Győződjön meg arról, hogy a tárház Scala megjelölve használunk 2.10.x. Ne használja a tárház Scala megjelölve 2.11.x.

4. Válassza a **Finish** (Befejezés) elemet.

5. Ha a **projekt** nézet még nincs megnyitva, nyomja le az ENTER **Alt + 1** való megnyitásához.

6. A **Project Explorer**, bontsa ki a projektet, és válassza **src**.

7. Kattintson a jobb gombbal **src**, mutasson a **új**, majd válassza ki **Scala osztály**.

8. Az a **neve** mezőbe írjon be egy nevet a a **jellegű** mezőben válassza **objektum**, majd válassza ki **OK**.

    ![Az új Scala osztály létrehozása ablak](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-new-scala-class.png)

9. A .scala fájlban illessze be a következő kódot:

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

10. Az a **Build** menüjében válassza **Build projekt**. Győződjön meg arról, hogy a fordítás sikeresen befejeződött.


## <a name="link-to-the-hortonworks-sandbox"></a>A Hortonworks védőfal mutató hivatkozás

Mielőtt Hortonworks védőfalat (emulátor) is kapcsolhat, egy meglévő IntelliJ alkalmazást kell rendelkeznie.

Csatolja az emulátor, tegye a következőket:

1. Nyissa meg a projektet az intellij-t.

2. Az a **nézet** menüjében válassza **eszközök Windows**, majd válassza ki **Azure Explorer**.

3. Bontsa ki a **Azure**, kattintson a jobb gombbal **HDInsight**, majd válassza ki **hivatkozásra az emulátor**.

4. Az a **hivatkozásra egy új emulátor** ablak, írja be a jelszót, hogy Ön a Hortonworks védőfal rendszergazdafiók beállított, adja meg az értékeket az alábbi képernyőfelvételen a hasonló, és válassza **OK**. 

   ![A "Hivatkozás egy új emulátor" ablak](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-link-an-emulator.png)

5. Az emulátor konfigurálásához jelölje ki **Igen**.

Ha az emulátorban sikeresen csatlakoztatva van, az emulátor (Hortonworks védőfal) megjelenik a HDInsight-csomópont.

## <a name="submit-the-spark-scala-application-to-the-hortonworks-sandbox"></a>A Spark Scala-kérelmet a Hortonworks védőfal felé

Miután a IntelliJ IDEA emulátorának, elküldheti a a projekthez.

Küldje el a projektet az emulátor, tegye a következőket:

1. A **Project Explorer**, kattintson jobb gombbal a projektre, majd válassza ki **küldje el a Spark-alkalmazást, amely**.

2. Tegye a következőket:

    a. Az a **Spark-fürt (csak Linux)** legördülő listára, válassza ki a helyi Hortonworks védőfal.

    b. Az a **fő osztálynév** mezőben válassza ki vagy adja meg a fő osztály nevét. Ebben az oktatóanyagban a értéke **GroupByTest**.

3. Válassza ki **nyújt**. A feladat elküldése naplók a Spark küldésének eszköz ablakban jelennek meg.

## <a name="next-steps"></a>Következő lépések

- További információk az IntelliJ HDInsight Tools használatával az alkalmazások hdinsight Spark létrehozása, [használata a HDInsight Tools a Spark-alkalmazások HDInsight Spark Linux-fürt létrehozása az IntelliJ Azure eszköztára](hdinsight-apache-spark-intellij-tool-plugin.md).

- Az IntelliJ HDInsight eszközök videó megtekintése, keresse fel [a HDInsight Tools bevezetni az IntelliJ Spark fejlesztési](https://www.youtube.com/watch?v=YTZzYVgut6c).

- További információk az eszközkészlet segítségével távolról SSH-n keresztül a HDInsight Spark-alkalmazások hibakeresése, [távolról az IntelliJ SSH-n keresztül a Azure eszközkészlet a HDInsight-fürtök a Spark-alkalmazások hibakeresését](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md).

- További információk az eszközkészlet távolról segítségével a VPN-en keresztül a HDInsight Spark-alkalmazások hibakeresése, [használata a HDInsight Tools az intellij-t a Spark-alkalmazások HDInsight Spark Linux fürtön távolról Azure eszköztára](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md).

- További információk a HDInsight Tools for Eclipse használata Spark-alkalmazás létrehozásának, [HDInsight eszközök használata Spark-alkalmazások létrehozása az eclipse-ben Azure eszköztára](hdinsight-apache-spark-eclipse-tool-plugin.md).

- Videót, a HDInsight Tools for Eclipse, Ugrás [használható HDInsight Spark-alkalmazások létrehozása az eclipse-ben eszköz](https://mix.office.com/watch/1rau2mopb6fha).
