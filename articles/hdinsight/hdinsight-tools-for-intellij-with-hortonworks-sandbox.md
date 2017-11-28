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
# <a name="use-hdinsight-tools-for-intellij-with-hortonworks-sandbox"></a><span data-ttu-id="a3433-104">Használja a HDInsight Tools for IntelliJ a Hortonworks védőfal</span><span class="sxs-lookup"><span data-stu-id="a3433-104">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>

<span data-ttu-id="a3433-105">A HDInsight Tools for IntelliJ használata Apache Scala-alkalmazások fejlesztéséhez és tesztelje az alkalmazásokat a [Hortonworks védőfal](http://hortonworks.com/products/sandbox/) fut a munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="a3433-105">Learn how to use HDInsight Tools for IntelliJ to develop Apache Scala applications and then test the applications on [Hortonworks Sandbox](http://hortonworks.com/products/sandbox/) running on your workstation.</span></span> 

<span data-ttu-id="a3433-106">[IntelliJ IDEA](https://www.jetbrains.com/idea/) fejlesztési számítógép szoftver a Java-integrált fejlesztési környezeti (IDE) van.</span><span class="sxs-lookup"><span data-stu-id="a3433-106">[IntelliJ IDEA](https://www.jetbrains.com/idea/) is a Java-integrated development environment (IDE) for developing computer software.</span></span> <span data-ttu-id="a3433-107">Kifejlesztett és tesztelt Hortonworks a(z) az alkalmazások, után áthelyezheti őket [Azure HDInsight](hdinsight-hadoop-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a3433-107">After you have developed and tested your applications on Hortonworks Sandbox, you can move them to [Azure HDInsight](hdinsight-hadoop-introduction.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a3433-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a3433-108">Prerequisites</span></span>

<span data-ttu-id="a3433-109">Az oktatóanyag elindításának feltétele:</span><span class="sxs-lookup"><span data-stu-id="a3433-109">Before you begin this tutorial, you must have:</span></span>

- <span data-ttu-id="a3433-110">Hortonworks Data Platform (HDP) 2.4 a helyi környezetben futó Hortonworks a(z).</span><span class="sxs-lookup"><span data-stu-id="a3433-110">Hortonworks Data Platform (HDP) 2.4 on Hortonworks Sandbox running on your local environment.</span></span> <span data-ttu-id="a3433-111">HDP konfigurálásához lásd: [Ismerkedés a Hadoop ökoszisztémájának a virtuális gépen Hadoop védőfalat](hdinsight-hadoop-emulator-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="a3433-111">To configure HDP, see [Get started in the Hadoop ecosystem with a Hadoop sandbox on a virtual machine](hdinsight-hadoop-emulator-get-started.md).</span></span> 
    >[!NOTE]
    ><span data-ttu-id="a3433-112">A HDInsight Tools for IntelliJ csak HDP 2.4 tesztelték.</span><span class="sxs-lookup"><span data-stu-id="a3433-112">HDInsight Tools for IntelliJ has been tested only with HDP 2.4.</span></span> <span data-ttu-id="a3433-113">Ahhoz, hogy HDP 2.4, bontsa ki a **Hortonworks védőfal archív** a a [Hortonworks védőfal tölti le a hely](http://hortonworks.com/downloads/#sandbox).</span><span class="sxs-lookup"><span data-stu-id="a3433-113">To get HDP 2.4, expand **Hortonworks Sandbox Archive** on the [Hortonworks Sandbox downloads site](http://hortonworks.com/downloads/#sandbox).</span></span>

- <span data-ttu-id="a3433-114">[Java fejlesztői készlet (JDK) 1,8 vagy újabb verzió](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="a3433-114">[Java Developer Kit (JDK) version 1.8 or later](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span> <span data-ttu-id="a3433-115">JDK az intellij-t az Azure-eszközkészlet használata szükséges.</span><span class="sxs-lookup"><span data-stu-id="a3433-115">JDK is required by the Azure Toolkit for IntelliJ.</span></span>

- <span data-ttu-id="a3433-116">[IntelliJ IDEA community edition](https://www.jetbrains.com/idea/download) rendelkező a [Scala](https://plugins.jetbrains.com/idea/plugin/1347-scala) beépülő modul és a [IntelliJ Azure eszköztára](../azure-toolkit-for-intellij.md) beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="a3433-116">[IntelliJ IDEA community edition](https://www.jetbrains.com/idea/download) with the [Scala](https://plugins.jetbrains.com/idea/plugin/1347-scala) plug-in and the [Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij.md) plug-in.</span></span> <span data-ttu-id="a3433-117">A HDInsight Tools for IntelliJ az intellij-t Azure eszköztára részeként érhető el.</span><span class="sxs-lookup"><span data-stu-id="a3433-117">HDInsight Tools for IntelliJ is available as a part of the Azure Toolkit for IntelliJ.</span></span> 

  <span data-ttu-id="a3433-118">A beépülő modulok telepítéséhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="a3433-118">To install the plug-ins, do the following:</span></span>

  1. <span data-ttu-id="a3433-119">Nyissa meg az IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="a3433-119">Open IntelliJ IDEA.</span></span>
  2. <span data-ttu-id="a3433-120">Az a **üdvözlő** képernyőn válassza ki **konfigurálása**, majd válassza ki **beépülő modulok**.</span><span class="sxs-lookup"><span data-stu-id="a3433-120">On the **Welcome** screen, select **Configure**, and then select **Plugins**.</span></span>
  3. <span data-ttu-id="a3433-121">Válassza ki **JetBrains telepítése beépülő modul** bal alsó sarokban.</span><span class="sxs-lookup"><span data-stu-id="a3433-121">Select **Install JetBrains plugin** in the lower-left corner.</span></span>
  4. <span data-ttu-id="a3433-122">A keresési funkció segítségével kereshet **Scala**, majd válassza ki **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="a3433-122">Use the search function to search for **Scala**, and then select **Install**.</span></span>
  5. <span data-ttu-id="a3433-123">Válassza ki **indítsa újra az IntelliJ IDEA** a telepítés befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="a3433-123">Select **Restart IntelliJ IDEA** to complete the installation.</span></span>
  6. <span data-ttu-id="a3433-124">4. és 5 telepítéséhez megismétli a **IntelliJ Azure eszköztára**.</span><span class="sxs-lookup"><span data-stu-id="a3433-124">Repeat step 4 and 5 to install the **Azure Toolkit for IntelliJ**.</span></span> <span data-ttu-id="a3433-125">További információkért lásd: [az intellij-t az Azure-eszközkészlet telepítése](../azure-toolkit-for-intellij-installation.md).</span><span class="sxs-lookup"><span data-stu-id="a3433-125">For more information, see [Install the Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij-installation.md).</span></span>

## <a name="create-a-spark-scala-application"></a><span data-ttu-id="a3433-126">Spark Scala-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a3433-126">Create a Spark Scala application</span></span>

<span data-ttu-id="a3433-127">Ebben a szakaszban IntelliJ IDEA használatával hoz létre egy minta Scala-projektet.</span><span class="sxs-lookup"><span data-stu-id="a3433-127">In this section, you create a sample Scala project by using IntelliJ IDEA.</span></span> <span data-ttu-id="a3433-128">A következő szakaszban kapcsolhat IntelliJ IDEA a Hortonworks védőfal (emulátor) a projekt mentése előtt.</span><span class="sxs-lookup"><span data-stu-id="a3433-128">In the next section, you link IntelliJ IDEA to the Hortonworks Sandbox (emulator) before you submit the project.</span></span>

1. <span data-ttu-id="a3433-129">Nyissa meg az IntelliJ IDEA a munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="a3433-129">Open IntelliJ IDEA from your workstation.</span></span> <span data-ttu-id="a3433-130">Az a **új projekt** párbeszédpanelen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="a3433-130">In the **New Project** dialog box, do the following:</span></span>

   <span data-ttu-id="a3433-131">a.</span><span class="sxs-lookup"><span data-stu-id="a3433-131">a.</span></span> <span data-ttu-id="a3433-132">Válassza ki **HDInsight** > **a Spark on HDInsight (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="a3433-132">Select **HDInsight** > **Spark on HDInsight (Scala)**.</span></span>

   <span data-ttu-id="a3433-133">b.</span><span class="sxs-lookup"><span data-stu-id="a3433-133">b.</span></span> <span data-ttu-id="a3433-134">Az a **Build eszköz** listára, válassza ki, az igényeknek megfelelően az alábbiak valamelyikét:</span><span class="sxs-lookup"><span data-stu-id="a3433-134">In the **Build tool** list, select either of the following, according to your need:</span></span>

    * <span data-ttu-id="a3433-135">**Maven**, Scala-projekt létrehozása varázsló támogatásához</span><span class="sxs-lookup"><span data-stu-id="a3433-135">**Maven**, for Scala project-creation wizard support</span></span>
    * <span data-ttu-id="a3433-136">**SBT**, a függőségek kezelésére, és a Scala-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="a3433-136">**SBT**, for managing the dependencies and building for the Scala project</span></span>

   ![Az új projekt párbeszédpanel](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project.png)

2. <span data-ttu-id="a3433-138">Válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="a3433-138">Select **Next**.</span></span>

3. <span data-ttu-id="a3433-139">A következő **új projekt** párbeszédpanelen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="a3433-139">In the next **New Project** dialog box, do the following:</span></span>

    ![Az IntelliJ Scala projekt Tulajdonságok létrehozása](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project-properties.png)

    <span data-ttu-id="a3433-141">a.</span><span class="sxs-lookup"><span data-stu-id="a3433-141">a.</span></span> <span data-ttu-id="a3433-142">Az a **projektnevet** mezőbe írja be a projekt nevét.</span><span class="sxs-lookup"><span data-stu-id="a3433-142">In the **Project name** box, enter a project name.</span></span>

    <span data-ttu-id="a3433-143">b.</span><span class="sxs-lookup"><span data-stu-id="a3433-143">b.</span></span> <span data-ttu-id="a3433-144">Az a **projekt** mezőbe írja be a projekt helyére.</span><span class="sxs-lookup"><span data-stu-id="a3433-144">In the **Project location** box, enter a project location.</span></span>

    <span data-ttu-id="a3433-145">c.</span><span class="sxs-lookup"><span data-stu-id="a3433-145">c.</span></span> <span data-ttu-id="a3433-146">Mellett a **projekt SDK** legördülő listában válassza **új**, jelölje be **JDK**, és adja meg a mappát, a Java JDK 1.7 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="a3433-146">Next to the **Project SDK** drop-down list, select **New**, select **JDK**, and then specify the folder of Java JDK version 1.7 or later.</span></span> <span data-ttu-id="a3433-147">Válassza ki **Java 1.8** a Spark-fürt 2.x, vagy válassza ki a **Java 1.7** a Spark 1.x fürthöz.</span><span class="sxs-lookup"><span data-stu-id="a3433-147">Select **Java 1.8** for the Spark 2.x cluster, or select **Java 1.7** for the Spark 1.x cluster.</span></span> <span data-ttu-id="a3433-148">Az alapértelmezett hely: C:\Program Files\Java\jdk1.8.x_xxx.</span><span class="sxs-lookup"><span data-stu-id="a3433-148">The default location is C:\Program Files\Java\jdk1.8.x_xxx.</span></span>

    <span data-ttu-id="a3433-149">d.</span><span class="sxs-lookup"><span data-stu-id="a3433-149">d.</span></span> <span data-ttu-id="a3433-150">Az a **Spark verzió** legördülő listából válassza ki, Scala-projekt létrehozása varázsló Spark SDK és Scala SDK integrálja a megfelelő verzióját.</span><span class="sxs-lookup"><span data-stu-id="a3433-150">In the **Spark version** drop-down list, Scala project creation wizard integrates the proper version for Spark SDK and Scala SDK.</span></span> <span data-ttu-id="a3433-151">Ha a Spark-fürt verziója korábbi, mint 2,0, válassza ki a **Spark 1.x**.</span><span class="sxs-lookup"><span data-stu-id="a3433-151">If the Spark cluster version is earlier than 2.0, select **Spark 1.x**.</span></span> <span data-ttu-id="a3433-152">Máskülönben válassza **Spark2.x**.</span><span class="sxs-lookup"><span data-stu-id="a3433-152">Otherwise, select **Spark2.x**.</span></span> <span data-ttu-id="a3433-153">A példa Spark 1.6.2 (Scala 2.10.5).</span><span class="sxs-lookup"><span data-stu-id="a3433-153">This example uses Spark 1.6.2 (Scala 2.10.5).</span></span> <span data-ttu-id="a3433-154">Győződjön meg arról, hogy a tárház Scala megjelölve használunk 2.10.x.</span><span class="sxs-lookup"><span data-stu-id="a3433-154">Make sure that you are using the repository marked Scala 2.10.x.</span></span> <span data-ttu-id="a3433-155">Ne használja a tárház Scala megjelölve 2.11.x.</span><span class="sxs-lookup"><span data-stu-id="a3433-155">Do not use the repository marked Scala 2.11.x.</span></span>

4. <span data-ttu-id="a3433-156">Válassza a **Finish** (Befejezés) elemet.</span><span class="sxs-lookup"><span data-stu-id="a3433-156">Select **Finish**.</span></span>

5. <span data-ttu-id="a3433-157">Ha a **projekt** nézet még nincs megnyitva, nyomja le az ENTER **Alt + 1** való megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="a3433-157">If the **Project** view is not already open, press **Alt+1** to open it.</span></span>

6. <span data-ttu-id="a3433-158">A **Project Explorer**, bontsa ki a projektet, és válassza **src**.</span><span class="sxs-lookup"><span data-stu-id="a3433-158">In **Project Explorer**, expand the project, and then select **src**.</span></span>

7. <span data-ttu-id="a3433-159">Kattintson a jobb gombbal **src**, mutasson a **új**, majd válassza ki **Scala osztály**.</span><span class="sxs-lookup"><span data-stu-id="a3433-159">Right-click **src**, point to **New**, and then select **Scala class**.</span></span>

8. <span data-ttu-id="a3433-160">Az a **neve** mezőbe írjon be egy nevet a a **jellegű** mezőben válassza **objektum**, majd válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="a3433-160">In the **Name** box, enter a name, in the **Kind** box, select **Object**, and then select **OK**.</span></span>

    ![Az új Scala osztály létrehozása ablak](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-new-scala-class.png)

9. <span data-ttu-id="a3433-162">A .scala fájlban illessze be a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="a3433-162">In the .scala file, paste the following code:</span></span>

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

10. <span data-ttu-id="a3433-163">Az a **Build** menüjében válassza **Build projekt**.</span><span class="sxs-lookup"><span data-stu-id="a3433-163">On the **Build** menu, select **Build project**.</span></span> <span data-ttu-id="a3433-164">Győződjön meg arról, hogy a fordítás sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="a3433-164">Make sure that the compilation has been completed successfully.</span></span>


## <a name="link-to-the-hortonworks-sandbox"></a><span data-ttu-id="a3433-165">A Hortonworks védőfal mutató hivatkozás</span><span class="sxs-lookup"><span data-stu-id="a3433-165">Link to the Hortonworks Sandbox</span></span>

<span data-ttu-id="a3433-166">Mielőtt Hortonworks védőfalat (emulátor) is kapcsolhat, egy meglévő IntelliJ alkalmazást kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="a3433-166">Before you can link to a Hortonworks Sandbox (emulator), you must have an existing IntelliJ application.</span></span>

<span data-ttu-id="a3433-167">Csatolja az emulátor, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="a3433-167">To link to an emulator, do the following:</span></span>

1. <span data-ttu-id="a3433-168">Nyissa meg a projektet az intellij-t.</span><span class="sxs-lookup"><span data-stu-id="a3433-168">Open the project in IntelliJ.</span></span>

2. <span data-ttu-id="a3433-169">Az a **nézet** menüjében válassza **eszközök Windows**, majd válassza ki **Azure Explorer**.</span><span class="sxs-lookup"><span data-stu-id="a3433-169">On the **View** menu, select **Tools Windows**, and then select **Azure Explorer**.</span></span>

3. <span data-ttu-id="a3433-170">Bontsa ki a **Azure**, kattintson a jobb gombbal **HDInsight**, majd válassza ki **hivatkozásra az emulátor**.</span><span class="sxs-lookup"><span data-stu-id="a3433-170">Expand **Azure**, right-click **HDInsight**, and then select **Link an Emulator**.</span></span>

4. <span data-ttu-id="a3433-171">Az a **hivatkozásra egy új emulátor** ablak, írja be a jelszót, hogy Ön a Hortonworks védőfal rendszergazdafiók beállított, adja meg az értékeket az alábbi képernyőfelvételen a hasonló, és válassza **OK**.</span><span class="sxs-lookup"><span data-stu-id="a3433-171">In the **Link A New Emulator** window, enter the password that you've configured for the root account of the Hortonworks Sandbox, enter values similar to those in the following screenshot, and then select **OK**.</span></span> 

   ![A "Hivatkozás egy új emulátor" ablak](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-link-an-emulator.png)

5. <span data-ttu-id="a3433-173">Az emulátor konfigurálásához jelölje ki **Igen**.</span><span class="sxs-lookup"><span data-stu-id="a3433-173">To configure the emulator, select **Yes**.</span></span>

<span data-ttu-id="a3433-174">Ha az emulátorban sikeresen csatlakoztatva van, az emulátor (Hortonworks védőfal) megjelenik a HDInsight-csomópont.</span><span class="sxs-lookup"><span data-stu-id="a3433-174">When the emulator is connected successfully, the emulator (Hortonworks Sandbox) is listed in the HDInsight node.</span></span>

## <a name="submit-the-spark-scala-application-to-the-hortonworks-sandbox"></a><span data-ttu-id="a3433-175">A Spark Scala-kérelmet a Hortonworks védőfal felé</span><span class="sxs-lookup"><span data-stu-id="a3433-175">Submit the Spark Scala application to the Hortonworks Sandbox</span></span>

<span data-ttu-id="a3433-176">Miután a IntelliJ IDEA emulátorának, elküldheti a a projekthez.</span><span class="sxs-lookup"><span data-stu-id="a3433-176">After you have linked IntelliJ IDEA to the emulator, you can submit your project.</span></span>

<span data-ttu-id="a3433-177">Küldje el a projektet az emulátor, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="a3433-177">To submit a project to an emulator, do the following:</span></span>

1. <span data-ttu-id="a3433-178">A **Project Explorer**, kattintson jobb gombbal a projektre, majd válassza ki **küldje el a Spark-alkalmazást, amely**.</span><span class="sxs-lookup"><span data-stu-id="a3433-178">In **Project Explorer**, right-click the project, and then select **Submit Spark application to HDInsight**.</span></span>

2. <span data-ttu-id="a3433-179">Tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="a3433-179">Do the following:</span></span>

    <span data-ttu-id="a3433-180">a.</span><span class="sxs-lookup"><span data-stu-id="a3433-180">a.</span></span> <span data-ttu-id="a3433-181">Az a **Spark-fürt (csak Linux)** legördülő listára, válassza ki a helyi Hortonworks védőfal.</span><span class="sxs-lookup"><span data-stu-id="a3433-181">In the **Spark cluster (Linux only)** drop-down list, select your local Hortonworks Sandbox.</span></span>

    <span data-ttu-id="a3433-182">b.</span><span class="sxs-lookup"><span data-stu-id="a3433-182">b.</span></span> <span data-ttu-id="a3433-183">Az a **fő osztálynév** mezőben válassza ki vagy adja meg a fő osztály nevét.</span><span class="sxs-lookup"><span data-stu-id="a3433-183">In the **Main class name** box, choose or enter the main class name.</span></span> <span data-ttu-id="a3433-184">Ebben az oktatóanyagban a értéke **GroupByTest**.</span><span class="sxs-lookup"><span data-stu-id="a3433-184">For this tutorial, the name is **GroupByTest**.</span></span>

3. <span data-ttu-id="a3433-185">Válassza ki **nyújt**.</span><span class="sxs-lookup"><span data-stu-id="a3433-185">Select **Submit**.</span></span> <span data-ttu-id="a3433-186">A feladat elküldése naplók a Spark küldésének eszköz ablakban jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="a3433-186">The job submission logs are shown in the Spark submission tool window.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a3433-187">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a3433-187">Next steps</span></span>

- <span data-ttu-id="a3433-188">További információk az IntelliJ HDInsight Tools használatával az alkalmazások hdinsight Spark létrehozása, [használata a HDInsight Tools a Spark-alkalmazások HDInsight Spark Linux-fürt létrehozása az IntelliJ Azure eszköztára](hdinsight-apache-spark-intellij-tool-plugin.md).</span><span class="sxs-lookup"><span data-stu-id="a3433-188">To learn how to create Spark applications for HDInsight by using HDInsight Tools for IntelliJ, go to [Use HDInsight Tools in Azure Toolkit for IntelliJ to create Spark applications for HDInsight Spark Linux cluster](hdinsight-apache-spark-intellij-tool-plugin.md).</span></span>

- <span data-ttu-id="a3433-189">Az IntelliJ HDInsight eszközök videó megtekintése, keresse fel [a HDInsight Tools bevezetni az IntelliJ Spark fejlesztési](https://www.youtube.com/watch?v=YTZzYVgut6c).</span><span class="sxs-lookup"><span data-stu-id="a3433-189">To watch a video of HDInsight Tools for IntelliJ, go to [Introduce HDInsight Tools for IntelliJ for Spark Development](https://www.youtube.com/watch?v=YTZzYVgut6c).</span></span>

- <span data-ttu-id="a3433-190">További információk az eszközkészlet segítségével távolról SSH-n keresztül a HDInsight Spark-alkalmazások hibakeresése, [távolról az IntelliJ SSH-n keresztül a Azure eszközkészlet a HDInsight-fürtök a Spark-alkalmazások hibakeresését](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md).</span><span class="sxs-lookup"><span data-stu-id="a3433-190">To learn how to debug Spark applications by using the toolkit remotely on HDInsight through SSH, go to [Remotely debug Spark applications on an HDInsight cluster with Azure Toolkit for IntelliJ through SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md).</span></span>

- <span data-ttu-id="a3433-191">További információk az eszközkészlet távolról segítségével a VPN-en keresztül a HDInsight Spark-alkalmazások hibakeresése, [használata a HDInsight Tools az intellij-t a Spark-alkalmazások HDInsight Spark Linux fürtön távolról Azure eszköztára](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md).</span><span class="sxs-lookup"><span data-stu-id="a3433-191">To learn how to debug Spark applications by using the toolkit remotely on HDInsight through VPN, go to [Use HDInsight Tools in Azure Toolkit for IntelliJ to debug Spark applications remotely on HDInsight Spark Linux cluster](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md).</span></span>

- <span data-ttu-id="a3433-192">További információk a HDInsight Tools for Eclipse használata Spark-alkalmazás létrehozásának, [HDInsight eszközök használata Spark-alkalmazások létrehozása az eclipse-ben Azure eszköztára](hdinsight-apache-spark-eclipse-tool-plugin.md).</span><span class="sxs-lookup"><span data-stu-id="a3433-192">To learn how to use HDInsight Tools for Eclipse to create a Spark application, go to [Use HDInsight Tools in Azure Toolkit for Eclipse to create Spark applications](hdinsight-apache-spark-eclipse-tool-plugin.md).</span></span>

- <span data-ttu-id="a3433-193">Videót, a HDInsight Tools for Eclipse, Ugrás [használható HDInsight Spark-alkalmazások létrehozása az eclipse-ben eszköz](https://mix.office.com/watch/1rau2mopb6fha).</span><span class="sxs-lookup"><span data-stu-id="a3433-193">To watch a video of HDInsight Tools for Eclipse, go to [Use HDInsight Tool for Eclipse to create Spark applications](https://mix.office.com/watch/1rau2mopb6fha).</span></span>
