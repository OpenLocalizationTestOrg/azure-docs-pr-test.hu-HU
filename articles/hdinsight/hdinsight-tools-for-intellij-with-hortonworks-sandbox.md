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
# <a name="use-hdinsight-tools-for-intellij-with-hortonworks-sandbox"></a><span data-ttu-id="6bb83-104">Használja a HDInsight Tools for IntelliJ a Hortonworks védőfal</span><span class="sxs-lookup"><span data-stu-id="6bb83-104">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>

<span data-ttu-id="6bb83-105">Ismerje meg, hogyan toouse a HDInsight Tools for IntelliJ toodevelop Apache Scala-alkalmazások és végül pedig tesztelheti hello az alkalmazások [Hortonworks védőfal](http://hortonworks.com/products/sandbox/) fut a munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="6bb83-105">Learn how toouse HDInsight Tools for IntelliJ toodevelop Apache Scala applications and then test hello applications on [Hortonworks Sandbox](http://hortonworks.com/products/sandbox/) running on your workstation.</span></span> 

<span data-ttu-id="6bb83-106">[IntelliJ IDEA](https://www.jetbrains.com/idea/) fejlesztési számítógép szoftver a Java-integrált fejlesztési környezeti (IDE) van.</span><span class="sxs-lookup"><span data-stu-id="6bb83-106">[IntelliJ IDEA](https://www.jetbrains.com/idea/) is a Java-integrated development environment (IDE) for developing computer software.</span></span> <span data-ttu-id="6bb83-107">Kifejlesztett és tesztelt Hortonworks a(z) az alkalmazások, után áthelyezheti őket túl[Azure HDInsight](hdinsight-hadoop-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="6bb83-107">After you have developed and tested your applications on Hortonworks Sandbox, you can move them too[Azure HDInsight](hdinsight-hadoop-introduction.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6bb83-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6bb83-108">Prerequisites</span></span>

<span data-ttu-id="6bb83-109">Az oktatóanyag elindításának feltétele:</span><span class="sxs-lookup"><span data-stu-id="6bb83-109">Before you begin this tutorial, you must have:</span></span>

- <span data-ttu-id="6bb83-110">Hortonworks Data Platform (HDP) 2.4 a helyi környezetben futó Hortonworks a(z).</span><span class="sxs-lookup"><span data-stu-id="6bb83-110">Hortonworks Data Platform (HDP) 2.4 on Hortonworks Sandbox running on your local environment.</span></span> <span data-ttu-id="6bb83-111">tooconfigure HDP, lásd: [megismerheti a hello Hadoop ökoszisztémájának a virtuális gépen Hadoop védőfalat](hdinsight-hadoop-emulator-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="6bb83-111">tooconfigure HDP, see [Get started in hello Hadoop ecosystem with a Hadoop sandbox on a virtual machine](hdinsight-hadoop-emulator-get-started.md).</span></span> 
    >[!NOTE]
    ><span data-ttu-id="6bb83-112">A HDInsight Tools for IntelliJ csak HDP 2.4 tesztelték.</span><span class="sxs-lookup"><span data-stu-id="6bb83-112">HDInsight Tools for IntelliJ has been tested only with HDP 2.4.</span></span> <span data-ttu-id="6bb83-113">tooget HDP 2.4, bontsa ki a **Hortonworks védőfal archív** a hello [Hortonworks védőfal tölti le a hely](http://hortonworks.com/downloads/#sandbox).</span><span class="sxs-lookup"><span data-stu-id="6bb83-113">tooget HDP 2.4, expand **Hortonworks Sandbox Archive** on hello [Hortonworks Sandbox downloads site](http://hortonworks.com/downloads/#sandbox).</span></span>

- <span data-ttu-id="6bb83-114">[Java fejlesztői készlet (JDK) 1,8 vagy újabb verzió](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="6bb83-114">[Java Developer Kit (JDK) version 1.8 or later](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span> <span data-ttu-id="6bb83-115">JDK az IntelliJ hello Azure eszközkészlet használata szükséges.</span><span class="sxs-lookup"><span data-stu-id="6bb83-115">JDK is required by hello Azure Toolkit for IntelliJ.</span></span>

- <span data-ttu-id="6bb83-116">[IntelliJ IDEA community edition](https://www.jetbrains.com/idea/download) a hello [Scala](https://plugins.jetbrains.com/idea/plugin/1347-scala) beépülő modul és hello [IntelliJ Azure eszköztára](../azure-toolkit-for-intellij.md) beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="6bb83-116">[IntelliJ IDEA community edition](https://www.jetbrains.com/idea/download) with hello [Scala](https://plugins.jetbrains.com/idea/plugin/1347-scala) plug-in and hello [Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij.md) plug-in.</span></span> <span data-ttu-id="6bb83-117">A HDInsight Tools for IntelliJ hello Azure eszköztára IntelliJ részeként érhető el.</span><span class="sxs-lookup"><span data-stu-id="6bb83-117">HDInsight Tools for IntelliJ is available as a part of hello Azure Toolkit for IntelliJ.</span></span> 

  <span data-ttu-id="6bb83-118">tooinstall hello beépülő modulok, hello a következő:</span><span class="sxs-lookup"><span data-stu-id="6bb83-118">tooinstall hello plug-ins, do hello following:</span></span>

  1. <span data-ttu-id="6bb83-119">Nyissa meg az IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="6bb83-119">Open IntelliJ IDEA.</span></span>
  2. <span data-ttu-id="6bb83-120">A hello **üdvözlő** képernyőn válassza ki **konfigurálása**, majd válassza ki **beépülő modulok**.</span><span class="sxs-lookup"><span data-stu-id="6bb83-120">On hello **Welcome** screen, select **Configure**, and then select **Plugins**.</span></span>
  3. <span data-ttu-id="6bb83-121">Válassza ki **JetBrains telepítése beépülő modul** hello bal alsó sarokban.</span><span class="sxs-lookup"><span data-stu-id="6bb83-121">Select **Install JetBrains plugin** in hello lower-left corner.</span></span>
  4. <span data-ttu-id="6bb83-122">Használjon hello keresési funkció toosearch a **Scala**, majd válassza ki **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="6bb83-122">Use hello search function toosearch for **Scala**, and then select **Install**.</span></span>
  5. <span data-ttu-id="6bb83-123">Válassza ki **indítsa újra az IntelliJ IDEA** toocomplete hello telepítését.</span><span class="sxs-lookup"><span data-stu-id="6bb83-123">Select **Restart IntelliJ IDEA** toocomplete hello installation.</span></span>
  6. <span data-ttu-id="6bb83-124">4. és 5 megismétli tooinstall hello **IntelliJ Azure eszköztára**.</span><span class="sxs-lookup"><span data-stu-id="6bb83-124">Repeat step 4 and 5 tooinstall hello **Azure Toolkit for IntelliJ**.</span></span> <span data-ttu-id="6bb83-125">További információkért lásd: [telepítés hello Azure eszköztára IntelliJ](../azure-toolkit-for-intellij-installation.md).</span><span class="sxs-lookup"><span data-stu-id="6bb83-125">For more information, see [Install hello Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij-installation.md).</span></span>

## <a name="create-a-spark-scala-application"></a><span data-ttu-id="6bb83-126">Spark Scala-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6bb83-126">Create a Spark Scala application</span></span>

<span data-ttu-id="6bb83-127">Ebben a szakaszban IntelliJ IDEA használatával hoz létre egy minta Scala-projektet.</span><span class="sxs-lookup"><span data-stu-id="6bb83-127">In this section, you create a sample Scala project by using IntelliJ IDEA.</span></span> <span data-ttu-id="6bb83-128">A következő szakaszban hello csatolunk IntelliJ IDEA toohello Hortonworks védőfal (emulátor) hello projekt mentése előtt.</span><span class="sxs-lookup"><span data-stu-id="6bb83-128">In hello next section, you link IntelliJ IDEA toohello Hortonworks Sandbox (emulator) before you submit hello project.</span></span>

1. <span data-ttu-id="6bb83-129">Nyissa meg az IntelliJ IDEA a munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="6bb83-129">Open IntelliJ IDEA from your workstation.</span></span> <span data-ttu-id="6bb83-130">A hello **új projekt** párbeszédpanel mezőbe hello a következő:</span><span class="sxs-lookup"><span data-stu-id="6bb83-130">In hello **New Project** dialog box, do hello following:</span></span>

   <span data-ttu-id="6bb83-131">a.</span><span class="sxs-lookup"><span data-stu-id="6bb83-131">a.</span></span> <span data-ttu-id="6bb83-132">Válassza ki **HDInsight** > **a Spark on HDInsight (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="6bb83-132">Select **HDInsight** > **Spark on HDInsight (Scala)**.</span></span>

   <span data-ttu-id="6bb83-133">b.</span><span class="sxs-lookup"><span data-stu-id="6bb83-133">b.</span></span> <span data-ttu-id="6bb83-134">A hello **Build eszköz** listára, válassza ki a következő, tooyour szükség szerint hello valamelyikét:</span><span class="sxs-lookup"><span data-stu-id="6bb83-134">In hello **Build tool** list, select either of hello following, according tooyour need:</span></span>

    * <span data-ttu-id="6bb83-135">**Maven**, Scala-projekt létrehozása varázsló támogatásához</span><span class="sxs-lookup"><span data-stu-id="6bb83-135">**Maven**, for Scala project-creation wizard support</span></span>
    * <span data-ttu-id="6bb83-136">**SBT**, hello függőségek kezelése és hello Scala-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="6bb83-136">**SBT**, for managing hello dependencies and building for hello Scala project</span></span>

   ![hello új projekt párbeszédpanel](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project.png)

2. <span data-ttu-id="6bb83-138">Válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="6bb83-138">Select **Next**.</span></span>

3. <span data-ttu-id="6bb83-139">Hello a következő **új projekt** párbeszédpanel mezőbe hello a következő:</span><span class="sxs-lookup"><span data-stu-id="6bb83-139">In hello next **New Project** dialog box, do hello following:</span></span>

    ![Az IntelliJ Scala projekt Tulajdonságok létrehozása](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project-properties.png)

    <span data-ttu-id="6bb83-141">a.</span><span class="sxs-lookup"><span data-stu-id="6bb83-141">a.</span></span> <span data-ttu-id="6bb83-142">A hello **projektnevet** mezőbe írja be a projekt nevét.</span><span class="sxs-lookup"><span data-stu-id="6bb83-142">In hello **Project name** box, enter a project name.</span></span>

    <span data-ttu-id="6bb83-143">b.</span><span class="sxs-lookup"><span data-stu-id="6bb83-143">b.</span></span> <span data-ttu-id="6bb83-144">A hello **projekt** mezőbe írja be a projekt helyére.</span><span class="sxs-lookup"><span data-stu-id="6bb83-144">In hello **Project location** box, enter a project location.</span></span>

    <span data-ttu-id="6bb83-145">c.</span><span class="sxs-lookup"><span data-stu-id="6bb83-145">c.</span></span> <span data-ttu-id="6bb83-146">Következő toohello **projekt SDK** legördülő listában válassza **új**, jelölje be **JDK**, majd adja meg a hello mappában található Java JDK 1.7 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="6bb83-146">Next toohello **Project SDK** drop-down list, select **New**, select **JDK**, and then specify hello folder of Java JDK version 1.7 or later.</span></span> <span data-ttu-id="6bb83-147">Válassza ki **Java 1.8** hello Spark 2.x fürt, vagy válassza ki a **Java 1.7** hello Spark 1.x fürthöz.</span><span class="sxs-lookup"><span data-stu-id="6bb83-147">Select **Java 1.8** for hello Spark 2.x cluster, or select **Java 1.7** for hello Spark 1.x cluster.</span></span> <span data-ttu-id="6bb83-148">hello alapértelmezett hely: C:\Program Files\Java\jdk1.8.x_xxx.</span><span class="sxs-lookup"><span data-stu-id="6bb83-148">hello default location is C:\Program Files\Java\jdk1.8.x_xxx.</span></span>

    <span data-ttu-id="6bb83-149">d.</span><span class="sxs-lookup"><span data-stu-id="6bb83-149">d.</span></span> <span data-ttu-id="6bb83-150">A hello **Spark verzió** legördülő listából válassza ki, Scala-projekt létrehozása varázsló hello a verzió megfelelőségének integrálja a Spark SDK és a Scala SDK.</span><span class="sxs-lookup"><span data-stu-id="6bb83-150">In hello **Spark version** drop-down list, Scala project creation wizard integrates hello proper version for Spark SDK and Scala SDK.</span></span> <span data-ttu-id="6bb83-151">Ha hello Spark-fürt verziója korábbi, mint 2,0, válassza ki a **Spark 1.x**.</span><span class="sxs-lookup"><span data-stu-id="6bb83-151">If hello Spark cluster version is earlier than 2.0, select **Spark 1.x**.</span></span> <span data-ttu-id="6bb83-152">Máskülönben válassza **Spark2.x**.</span><span class="sxs-lookup"><span data-stu-id="6bb83-152">Otherwise, select **Spark2.x**.</span></span> <span data-ttu-id="6bb83-153">A példa Spark 1.6.2 (Scala 2.10.5).</span><span class="sxs-lookup"><span data-stu-id="6bb83-153">This example uses Spark 1.6.2 (Scala 2.10.5).</span></span> <span data-ttu-id="6bb83-154">Ellenőrizze, hogy a megjelölt Scala hello tárház használt 2.10.x.</span><span class="sxs-lookup"><span data-stu-id="6bb83-154">Make sure that you are using hello repository marked Scala 2.10.x.</span></span> <span data-ttu-id="6bb83-155">Ne használjon Scala megjelölve hello tárház 2.11.x.</span><span class="sxs-lookup"><span data-stu-id="6bb83-155">Do not use hello repository marked Scala 2.11.x.</span></span>

4. <span data-ttu-id="6bb83-156">Válassza a **Finish** (Befejezés) elemet.</span><span class="sxs-lookup"><span data-stu-id="6bb83-156">Select **Finish**.</span></span>

5. <span data-ttu-id="6bb83-157">Ha hello **projekt** nézet még nincs megnyitva, nyomja le az ENTER **Alt + 1** tooopen azt.</span><span class="sxs-lookup"><span data-stu-id="6bb83-157">If hello **Project** view is not already open, press **Alt+1** tooopen it.</span></span>

6. <span data-ttu-id="6bb83-158">A **Project Explorer**, bontsa ki hello projektet, és válassza **src**.</span><span class="sxs-lookup"><span data-stu-id="6bb83-158">In **Project Explorer**, expand hello project, and then select **src**.</span></span>

7. <span data-ttu-id="6bb83-159">Kattintson a jobb gombbal **src**, pont túl**új**, majd válassza ki **Scala osztály**.</span><span class="sxs-lookup"><span data-stu-id="6bb83-159">Right-click **src**, point too**New**, and then select **Scala class**.</span></span>

8. <span data-ttu-id="6bb83-160">A hello **neve** mezőbe írjon be egy nevet, a hello **jellegű** mezőben válassza **objektum**, majd válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="6bb83-160">In hello **Name** box, enter a name, in hello **Kind** box, select **Object**, and then select **OK**.</span></span>

    ![hello új Scala osztály létrehozása ablak](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-new-scala-class.png)

9. <span data-ttu-id="6bb83-162">Hello .scala fájlban illessze be a kódját a következő hello:</span><span class="sxs-lookup"><span data-stu-id="6bb83-162">In hello .scala file, paste hello following code:</span></span>

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

10. <span data-ttu-id="6bb83-163">A hello **Build** menüjében válassza **Build projekt**.</span><span class="sxs-lookup"><span data-stu-id="6bb83-163">On hello **Build** menu, select **Build project**.</span></span> <span data-ttu-id="6bb83-164">Győződjön meg arról, hogy hello fordítási sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="6bb83-164">Make sure that hello compilation has been completed successfully.</span></span>


## <a name="link-toohello-hortonworks-sandbox"></a><span data-ttu-id="6bb83-165">Toohello Hortonworks védőfal hivatkozás</span><span class="sxs-lookup"><span data-stu-id="6bb83-165">Link toohello Hortonworks Sandbox</span></span>

<span data-ttu-id="6bb83-166">Mielőtt tooa Hortonworks védőfal (emulátor) is kapcsolhat, egy meglévő IntelliJ alkalmazást kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="6bb83-166">Before you can link tooa Hortonworks Sandbox (emulator), you must have an existing IntelliJ application.</span></span>

<span data-ttu-id="6bb83-167">toolink tooan emulátor, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="6bb83-167">toolink tooan emulator, do hello following:</span></span>

1. <span data-ttu-id="6bb83-168">Az IntelliJ projekt megnyitása hello.</span><span class="sxs-lookup"><span data-stu-id="6bb83-168">Open hello project in IntelliJ.</span></span>

2. <span data-ttu-id="6bb83-169">A hello **nézet** menüjében válassza **eszközök Windows**, majd válassza ki **Azure Explorer**.</span><span class="sxs-lookup"><span data-stu-id="6bb83-169">On hello **View** menu, select **Tools Windows**, and then select **Azure Explorer**.</span></span>

3. <span data-ttu-id="6bb83-170">Bontsa ki a **Azure**, kattintson a jobb gombbal **HDInsight**, majd válassza ki **hivatkozásra az emulátor**.</span><span class="sxs-lookup"><span data-stu-id="6bb83-170">Expand **Azure**, right-click **HDInsight**, and then select **Link an Emulator**.</span></span>

4. <span data-ttu-id="6bb83-171">A hello **hivatkozásra egy új emulátor** ablakban adja meg, hogy Ön hello Hortonworks védőfal hello legfelső szintű fiókjához beállított, adja meg a következő képernyőkép hello értékek hasonló toothose, majd válassza ki a hello jelszót **OK** .</span><span class="sxs-lookup"><span data-stu-id="6bb83-171">In hello **Link A New Emulator** window, enter hello password that you've configured for hello root account of hello Hortonworks Sandbox, enter values similar toothose in hello following screenshot, and then select **OK**.</span></span> 

   ![hello "Hivatkozásra egy új emulátor" ablak](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-link-an-emulator.png)

5. <span data-ttu-id="6bb83-173">tooconfigure hello emulátor, jelölje be **Igen**.</span><span class="sxs-lookup"><span data-stu-id="6bb83-173">tooconfigure hello emulator, select **Yes**.</span></span>

<span data-ttu-id="6bb83-174">Ha hello emulátor sikeresen csatlakoztatva van, hello emulátor (Hortonworks védőfal) hello HDInsight csomópont megjelenik.</span><span class="sxs-lookup"><span data-stu-id="6bb83-174">When hello emulator is connected successfully, hello emulator (Hortonworks Sandbox) is listed in hello HDInsight node.</span></span>

## <a name="submit-hello-spark-scala-application-toohello-hortonworks-sandbox"></a><span data-ttu-id="6bb83-175">Hello Spark Scala alkalmazás toohello Hortonworks védőfal elküldése</span><span class="sxs-lookup"><span data-stu-id="6bb83-175">Submit hello Spark Scala application toohello Hortonworks Sandbox</span></span>

<span data-ttu-id="6bb83-176">Miután a IntelliJ IDEA toohello emulátor, elküldheti a projekthez.</span><span class="sxs-lookup"><span data-stu-id="6bb83-176">After you have linked IntelliJ IDEA toohello emulator, you can submit your project.</span></span>

<span data-ttu-id="6bb83-177">a projekt tooan emulátor toosubmit hello a következő:</span><span class="sxs-lookup"><span data-stu-id="6bb83-177">toosubmit a project tooan emulator, do hello following:</span></span>

1. <span data-ttu-id="6bb83-178">A **Project Explorer**, kattintson a jobb gombbal a hello projektet, és válassza **küldje el a külső alkalmazás tooHDInsight**.</span><span class="sxs-lookup"><span data-stu-id="6bb83-178">In **Project Explorer**, right-click hello project, and then select **Submit Spark application tooHDInsight**.</span></span>

2. <span data-ttu-id="6bb83-179">A következő hello:</span><span class="sxs-lookup"><span data-stu-id="6bb83-179">Do hello following:</span></span>

    <span data-ttu-id="6bb83-180">a.</span><span class="sxs-lookup"><span data-stu-id="6bb83-180">a.</span></span> <span data-ttu-id="6bb83-181">A hello **Spark-fürt (csak Linux)** legördülő listára, válassza ki a helyi Hortonworks védőfal.</span><span class="sxs-lookup"><span data-stu-id="6bb83-181">In hello **Spark cluster (Linux only)** drop-down list, select your local Hortonworks Sandbox.</span></span>

    <span data-ttu-id="6bb83-182">b.</span><span class="sxs-lookup"><span data-stu-id="6bb83-182">b.</span></span> <span data-ttu-id="6bb83-183">A hello **fő osztálynév** mezőben válassza ki vagy hello fő osztály neve.</span><span class="sxs-lookup"><span data-stu-id="6bb83-183">In hello **Main class name** box, choose or enter hello main class name.</span></span> <span data-ttu-id="6bb83-184">Ebben az oktatóanyagban hello értéke **GroupByTest**.</span><span class="sxs-lookup"><span data-stu-id="6bb83-184">For this tutorial, hello name is **GroupByTest**.</span></span>

3. <span data-ttu-id="6bb83-185">Válassza ki **nyújt**.</span><span class="sxs-lookup"><span data-stu-id="6bb83-185">Select **Submit**.</span></span> <span data-ttu-id="6bb83-186">hello küldésének feladatnaplóit hello Spark küldésének eszköz ablakban láthatók.</span><span class="sxs-lookup"><span data-stu-id="6bb83-186">hello job submission logs are shown in hello Spark submission tool window.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6bb83-187">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6bb83-187">Next steps</span></span>

- <span data-ttu-id="6bb83-188">Hogyan toocreate Spark-alkalmazások HDInsight az intellij-t, a HDInsight Tools használatával nyissa meg túl toolearn[használata a HDInsight Tools az IntelliJ toocreate Spark-alkalmazások HDInsight Spark Linux-fürt Azure eszköztára](hdinsight-apache-spark-intellij-tool-plugin.md).</span><span class="sxs-lookup"><span data-stu-id="6bb83-188">toolearn how toocreate Spark applications for HDInsight by using HDInsight Tools for IntelliJ, go too[Use HDInsight Tools in Azure Toolkit for IntelliJ toocreate Spark applications for HDInsight Spark Linux cluster](hdinsight-apache-spark-intellij-tool-plugin.md).</span></span>

- <span data-ttu-id="6bb83-189">a HDInsight Tools for IntelliJ, videó toowatch lépjen túl[a HDInsight Tools bevezetni az IntelliJ Spark fejlesztési](https://www.youtube.com/watch?v=YTZzYVgut6c).</span><span class="sxs-lookup"><span data-stu-id="6bb83-189">toowatch a video of HDInsight Tools for IntelliJ, go too[Introduce HDInsight Tools for IntelliJ for Spark Development](https://www.youtube.com/watch?v=YTZzYVgut6c).</span></span>

- <span data-ttu-id="6bb83-190">toolearn hogyan toodebug Spark-alkalmazások használatával távolról a hdinsight platformon keresztül SSH-eszközkészlet hello lépjen túl[távolról az IntelliJ SSH-n keresztül a Azure eszközkészlet a HDInsight-fürtök a Spark-alkalmazások hibakeresését](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md).</span><span class="sxs-lookup"><span data-stu-id="6bb83-190">toolearn how toodebug Spark applications by using hello toolkit remotely on HDInsight through SSH, go too[Remotely debug Spark applications on an HDInsight cluster with Azure Toolkit for IntelliJ through SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md).</span></span>

- <span data-ttu-id="6bb83-191">toolearn toodebug Spark-alkalmazások használatával hello eszközkészlet távolról a hdinsight platformon keresztül VPN-profilok, hogyan lépjen túl[használata a HDInsight Tools az IntelliJ toodebug Spark-alkalmazások HDInsight Spark Linux fürtön távolról Azure eszköztára](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md).</span><span class="sxs-lookup"><span data-stu-id="6bb83-191">toolearn how toodebug Spark applications by using hello toolkit remotely on HDInsight through VPN, go too[Use HDInsight Tools in Azure Toolkit for IntelliJ toodebug Spark applications remotely on HDInsight Spark Linux cluster](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md).</span></span>

- <span data-ttu-id="6bb83-192">Hogyan lépjen az Eclipse toocreate egy Spark-alkalmazást, a HDInsight Tools toouse túl toolearn[használata a HDInsight Tools az Eclipse toocreate Spark-alkalmazások Azure eszköztára](hdinsight-apache-spark-eclipse-tool-plugin.md).</span><span class="sxs-lookup"><span data-stu-id="6bb83-192">toolearn how toouse HDInsight Tools for Eclipse toocreate a Spark application, go too[Use HDInsight Tools in Azure Toolkit for Eclipse toocreate Spark applications](hdinsight-apache-spark-eclipse-tool-plugin.md).</span></span>

- <span data-ttu-id="6bb83-193">a HDInsight Tools for eclipse-ben videó toowatch lépjen túl[HDInsight eszköz használata az Eclipse toocreate Spark-alkalmazások](https://mix.office.com/watch/1rau2mopb6fha).</span><span class="sxs-lookup"><span data-stu-id="6bb83-193">toowatch a video of HDInsight Tools for Eclipse, go too[Use HDInsight Tool for Eclipse toocreate Spark applications](https://mix.office.com/watch/1rau2mopb6fha).</span></span>
