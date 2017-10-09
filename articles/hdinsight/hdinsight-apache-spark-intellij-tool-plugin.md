---
title: "Az IntelliJ Azure eszköztára: Spark-alkalmazások a HDInsight-fürtök létrehozása |} Microsoft Docs"
description: "IntelliJ toodevelop Spark-alkalmazások scalában írt hello Azure eszközkészlet használni, és küldje el azokat a HDInsight Spark-fürt tooan."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 73304272-6c8b-482e-af7c-cd25d95dab4d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: nitinme
ms.openlocfilehash: 22cce014bb848a54e198e77a50bf13448012310e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-toolkit-for-intellij-toocreate-spark-applications-for-an-hdinsight-cluster"></a><span data-ttu-id="dc8d1-103">Az IntelliJ toocreate Spark-alkalmazások HDInsight-fürtök használata Azure eszköztára</span><span class="sxs-lookup"><span data-stu-id="dc8d1-103">Use Azure Toolkit for IntelliJ toocreate Spark applications for an HDInsight cluster</span></span>

<span data-ttu-id="dc8d1-104">Az IntelliJ beépülő modul toodevelop Spark-alkalmazások scalában írt hello Azure eszközkészlet használni, és ezután küldenie kell őket tooan hello IntelliJ integrált fejlesztési környezeti (IDE) közvetlenül a HDInsight Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-104">Use hello Azure Toolkit for IntelliJ plug-in toodevelop Spark applications written in Scala, and then submit them tooan HDInsight Spark cluster directly from hello IntelliJ integrated development environment (IDE).</span></span> <span data-ttu-id="dc8d1-105">Néhány beépülő modulja hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="dc8d1-105">You can use hello plug-in in a few ways:</span></span>

* <span data-ttu-id="dc8d1-106">Fejleszthet, és küldje el a Scala Spark alkalmazás egy HDInsight Spark-fürtön.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-106">Develop and submit a Scala Spark application on an HDInsight Spark cluster.</span></span>
* <span data-ttu-id="dc8d1-107">Az Azure HDInsight Spark-fürt erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-107">Access your Azure HDInsight Spark cluster resources.</span></span>
* <span data-ttu-id="dc8d1-108">Fejleszthet, és egy Scala Spark alkalmazás helyileg történő futtatása.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-108">Develop and run a Scala Spark application locally.</span></span>

<span data-ttu-id="dc8d1-109">toocreate a projekt, a nézet hello [Spark-alkalmazások létrehozása az intellij-t Azure eszköztára hello](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ) videó.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-109">toocreate your project, view hello [Create Spark Applications with hello Azure Toolkit for IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ) video.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dc8d1-110">A beépülő modul toocreate használja, és küldje el az alkalmazásokat csak egy HDInsight Spark-fürt Linux rendszeren.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-110">You can use this plug-in toocreate and submit applications only for an HDInsight Spark cluster on Linux.</span></span>
> 

## <a name="prerequisites"></a><span data-ttu-id="dc8d1-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="dc8d1-111">Prerequisites</span></span>

- <span data-ttu-id="dc8d1-112">HDInsight Linux rendszeren Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-112">An Apache Spark cluster on HDInsight Linux.</span></span> <span data-ttu-id="dc8d1-113">Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="dc8d1-113">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>
- <span data-ttu-id="dc8d1-114">Oracle Java fejlesztői készlet.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-114">Oracle Java Development Kit.</span></span> <span data-ttu-id="dc8d1-115">A későbbiekben telepítheti az hello [Oracle webhely](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="dc8d1-115">You can install it from hello [Oracle website](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
- <span data-ttu-id="dc8d1-116">IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-116">IntelliJ IDEA.</span></span> <span data-ttu-id="dc8d1-117">Ez a cikk 2017.1 verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-117">This article uses version 2017.1.</span></span> <span data-ttu-id="dc8d1-118">A későbbiekben telepítheti az hello [JetBrains webhely](https://www.jetbrains.com/idea/download/).</span><span class="sxs-lookup"><span data-stu-id="dc8d1-118">You can install it from hello [JetBrains website](https://www.jetbrains.com/idea/download/).</span></span>

## <a name="install-azure-toolkit-for-intellij"></a><span data-ttu-id="dc8d1-119">Az intellij-t Azure eszközkészlet telepítése</span><span class="sxs-lookup"><span data-stu-id="dc8d1-119">Install Azure Toolkit for IntelliJ</span></span>
<span data-ttu-id="dc8d1-120">A telepítési utasításokért lásd: [Azure eszközkészlet telepítése az IntelliJ](../azure-toolkit-for-intellij-installation.md).</span><span class="sxs-lookup"><span data-stu-id="dc8d1-120">For installation instructions, see [Install Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij-installation.md).</span></span>

## <a name="sign-in-tooyour-azure-subscription"></a><span data-ttu-id="dc8d1-121">Jelentkezzen be tooyour Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="dc8d1-121">Sign in tooyour Azure subscription</span></span>

1. <span data-ttu-id="dc8d1-122">Indítsa el a hello IntelliJ IDE, és nyissa meg az Azure-kezelővel.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-122">Start hello IntelliJ IDE, and open Azure Explorer.</span></span> <span data-ttu-id="dc8d1-123">A hello **nézet** menüjében válassza **eszköz Windows**, majd válassza ki **Azure Explorer**.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-123">On hello **View** menu, select **Tool Windows**, and then select **Azure Explorer**.</span></span>
       
   ![hello Azure Explorer hivatkozás](./media/hdinsight-apache-spark-intellij-tool-plugin/show-azure-explorer.png)

2. <span data-ttu-id="dc8d1-125">Kattintson a jobb gombbal hello **Azure** csomópont, és válassza **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-125">Right-click hello **Azure** node, and then select **Sign In**.</span></span>

3. <span data-ttu-id="dc8d1-126">A hello **Azure bejelentkezés** párbeszédpanelen jelölje ki **bejelentkezés**, és írja be Azure hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-126">In hello **Azure Sign In** dialog box, select **Sign in**, and then enter your Azure credentials.</span></span>

    ![hello Azure bejelentkezés párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-2.png)

4. <span data-ttu-id="dc8d1-128">Be van jelentkezve, miután hello **előfizetések kiválasztása** párbeszédpanel bezárásához listák összes hello Azure-előfizetések társított hello hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-128">After you're signed in, hello **Select Subscriptions** dialog box lists all hello Azure subscriptions that are associated with hello credentials.</span></span> <span data-ttu-id="dc8d1-129">Jelölje be hello **válasszon** gombra.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-129">Select hello **Select** button.</span></span>

    ![hello előfizetések kiválasztása párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/Select-Subscriptions.png)

5. <span data-ttu-id="dc8d1-131">A hello **Azure Explorer** lapján bontsa ki a **HDInsight** tooview hello HDInsight Spark-fürtök, amelyek az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-131">On hello **Azure Explorer** tab, expand **HDInsight** tooview hello HDInsight Spark clusters that are in your subscription.</span></span>
   
    ![HDInsight Spark-fürtjei az Azure-kezelővel](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-3.png)

6. <span data-ttu-id="dc8d1-133">tooview hello erőforrások (például storage-fiókok) hello fürt társított ennél jobban is kibonthatja a fürtnév csomópont.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-133">tooview hello resources (for example, storage accounts) that are associated with hello cluster, you can further expand a cluster-name node.</span></span>
   
    ![Egy bővített fürtnév csomópont](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-4.png)

## <a name="run-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a><span data-ttu-id="dc8d1-135">Futtassa a Spark Scala-alkalmazások HDInsight Spark-fürt</span><span class="sxs-lookup"><span data-stu-id="dc8d1-135">Run a Spark Scala application on an HDInsight Spark cluster</span></span>

1. <span data-ttu-id="dc8d1-136">Indítsa el az IntelliJ IDEA, és ezután a projekt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-136">Start IntelliJ IDEA, and then create a project.</span></span> <span data-ttu-id="dc8d1-137">A hello **új projekt** párbeszédpanel mezőbe hello a következő:</span><span class="sxs-lookup"><span data-stu-id="dc8d1-137">In hello **New Project** dialog box, do hello following:</span></span> 

   <span data-ttu-id="dc8d1-138">a.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-138">a.</span></span> <span data-ttu-id="dc8d1-139">Válassza ki **HDInsight** > **a Spark on HDInsight (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-139">Select **HDInsight** > **Spark on HDInsight (Scala)**.</span></span>

   <span data-ttu-id="dc8d1-140">b.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-140">b.</span></span> <span data-ttu-id="dc8d1-141">A hello **Build eszköz** listára, válassza ki a következő, tooyour szükség szerint hello valamelyikét:</span><span class="sxs-lookup"><span data-stu-id="dc8d1-141">In hello **Build tool** list, select either of hello following, according tooyour need:</span></span>

      * <span data-ttu-id="dc8d1-142">**Maven**, Scala-projekt létrehozása varázsló támogatásához</span><span class="sxs-lookup"><span data-stu-id="dc8d1-142">**Maven**, for Scala project-creation wizard support</span></span>
      * <span data-ttu-id="dc8d1-143">**SBT**, hello függőségek kezelése és hello Scala-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="dc8d1-143">**SBT**, for managing hello dependencies and building for hello Scala project</span></span>

    ![hello új projekt párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)

2. <span data-ttu-id="dc8d1-145">Válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-145">Select **Next**.</span></span>

3. <span data-ttu-id="dc8d1-146">hello Scala projekt-létrehozási varázsló automatikusan észleli, hogy telepítette-e hello Scala beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-146">hello Scala project-creation wizard automatically detects whether you've installed hello Scala plug-in.</span></span> <span data-ttu-id="dc8d1-147">Válassza ki **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-147">Select **Install**.</span></span>

   ![Scala beépülő modul ellenőrzése](./media/hdinsight-apache-spark-intellij-tool-plugin/Scala-Plugin-check-Reminder.PNG) 

4. <span data-ttu-id="dc8d1-149">toodownload hello beépülő modult, jelölje be a Scala **OK**.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-149">toodownload hello Scala plug-in, select **OK**.</span></span> <span data-ttu-id="dc8d1-150">Hajtsa végre a hello utasításokat toorestart intellij-t.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-150">Follow hello instructions toorestart IntelliJ.</span></span> 

   ![hello Scala beépülő modul telepítése párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/Choose-Scala-Plugin.PNG)

5. <span data-ttu-id="dc8d1-152">A hello **új projekt** ablakban, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="dc8d1-152">In hello **New Project** window, do hello following:</span></span>  

    ![Hello Spark SDK kiválasztása](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-new-project.png)

   <span data-ttu-id="dc8d1-154">a.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-154">a.</span></span> <span data-ttu-id="dc8d1-155">Adja meg a projekt nevét és helyét.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-155">Enter a project name and location.</span></span>

   <span data-ttu-id="dc8d1-156">b.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-156">b.</span></span> <span data-ttu-id="dc8d1-157">A hello **projekt SDK** legördülő listában válassza **Java 1.8** hello Spark 2.x fürt, vagy válassza ki a **Java 1.7** hello Spark 1.x fürthöz.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-157">In hello **Project SDK** drop-down list, select **Java 1.8** for hello Spark 2.x cluster, or select **Java 1.7** for hello Spark 1.x cluster.</span></span>

   <span data-ttu-id="dc8d1-158">c.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-158">c.</span></span> <span data-ttu-id="dc8d1-159">A hello **Spark verzió** legördülő listából válassza ki, Scala-projekt létrehozása varázsló hello a verzió megfelelőségének integrálja a Spark SDK és a Scala SDK.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-159">In hello **Spark version** drop-down list, Scala project creation wizard integrates hello proper version for Spark SDK and Scala SDK.</span></span> <span data-ttu-id="dc8d1-160">Ha hello Spark-fürt verziója korábbi, mint 2,0, válassza ki a **Spark 1.x**.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-160">If hello Spark cluster version is earlier than 2.0, select **Spark 1.x**.</span></span> <span data-ttu-id="dc8d1-161">Máskülönben válassza **Spark2.x**.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-161">Otherwise, select **Spark2.x**.</span></span> <span data-ttu-id="dc8d1-162">Ez a példa **Spark 2.0.2 (Scala 2.11.8)**.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-162">This example uses **Spark 2.0.2 (Scala 2.11.8)**.</span></span>

6. <span data-ttu-id="dc8d1-163">Válassza a **Finish** (Befejezés) elemet.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-163">Select **Finish**.</span></span>

7. <span data-ttu-id="dc8d1-164">hello Spark projekt automatikusan létrehoz egy összetevő.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-164">hello Spark project automatically creates an artifact for you.</span></span> <span data-ttu-id="dc8d1-165">tooview hello összetevő, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="dc8d1-165">tooview hello artifact, do hello following:</span></span>

   <span data-ttu-id="dc8d1-166">a.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-166">a.</span></span> <span data-ttu-id="dc8d1-167">A hello **fájl** menü **szerkezetének**.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-167">On hello **File** menu, select **Project Structure**.</span></span>

   <span data-ttu-id="dc8d1-168">b.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-168">b.</span></span> <span data-ttu-id="dc8d1-169">A hello **szerkezetének** párbeszédpanelen jelölje ki **összetevők** tooview hello alapértelmezett összetevő, amely jön létre.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-169">In hello **Project Structure** dialog box, select **Artifacts** tooview hello default artifact that is created.</span></span> <span data-ttu-id="dc8d1-170">A saját összetevő hello plusz jel kiválasztásával is létrehozhat (**+**).</span><span class="sxs-lookup"><span data-stu-id="dc8d1-170">You can also create your own artifact by selecting hello plus sign (**+**).</span></span>

      ![Összetevő info hello párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/default-artifact.png)
      
8. <span data-ttu-id="dc8d1-172">Adja hozzá az alkalmazás forráskódjához hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="dc8d1-172">Add your application source code by doing hello following:</span></span>

   <span data-ttu-id="dc8d1-173">a.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-173">a.</span></span> <span data-ttu-id="dc8d1-174">A Project Explorer, kattintson a jobb gombbal **src**, pont túl**új**, majd válassza ki **Scala osztály**.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-174">In Project Explorer, right-click **src**, point too**New**, and then select **Scala Class**.</span></span>
      
      ![Parancsok a Project Explorer Scala osztály létrehozása](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code.png)

   <span data-ttu-id="dc8d1-176">b.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-176">b.</span></span> <span data-ttu-id="dc8d1-177">A hello **hozzon létre új Scala osztály** párbeszédpanel mezőben adjon meg egy nevet, válasszon **objektum** a hello **jellegű** mezőbe, majd válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-177">In hello **Create New Scala Class** dialog box, provide a name, select **Object** in hello **Kind** box, and then select **OK**.</span></span>
      
      ![Hozzon létre új Scala osztály párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code-object.png)

   <span data-ttu-id="dc8d1-179">c.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-179">c.</span></span> <span data-ttu-id="dc8d1-180">A hello **MyClusterApp.scala** fájlt, illessze be a kódját a következő hello.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-180">In hello **MyClusterApp.scala** file, paste hello following code.</span></span> <span data-ttu-id="dc8d1-181">hello kód hello adatokat olvas HVAC.csv (az összes HDInsight Spark-fürtjei elérhető), lekéri a hello a sorokat hello hetedik oszlopban hello CSV-fájlban csak egy számot, és kiírja hello kimeneti túl**/HVACOut** hello alapértelmezés szerint a tároló hello fürthöz.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-181">hello code reads hello data from HVAC.csv (available on all HDInsight Spark clusters), retrieves hello rows that have only one digit in hello seventh column in hello CSV file, and writes hello output too**/HVACOut** under hello default storage container for hello cluster.</span></span>

        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
    
        object MyClusterApp{
            def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("MyClusterApp")
            val sc = new SparkContext(conf)
    
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
    
            //find hello rows that have only one digit in hello seventh column in hello CSV file
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
    
            rdd1.saveAsTextFile("wasb:///HVACOut")
            }
    
        }

9. <span data-ttu-id="dc8d1-182">Futtassa a hello alkalmazást a HDInsight Spark-fürt hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="dc8d1-182">Run hello application on an HDInsight Spark cluster by doing hello following:</span></span>

   <span data-ttu-id="dc8d1-183">a.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-183">a.</span></span> <span data-ttu-id="dc8d1-184">A Project Explorer, kattintson a jobb gombbal a hello projekt nevét, majd válassza ki **küldje el a külső alkalmazás tooHDInsight**.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-184">In Project Explorer, right-click hello project name, and then select **Submit Spark Application tooHDInsight**.</span></span>
      
      ![hello küldje el a külső alkalmazás tooHDInsight parancs](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-1.png)

   <span data-ttu-id="dc8d1-186">b.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-186">b.</span></span> <span data-ttu-id="dc8d1-187">Meg vannak felszólító tooenter Azure-előfizetés hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-187">You are prompted tooenter your Azure subscription credentials.</span></span> <span data-ttu-id="dc8d1-188">A hello **Spark küldésének** párbeszédpanelen adja meg a következő értékek hello, és válassza **Submit**.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-188">In hello **Spark Submission** dialog box, provide hello following values, and then select **Submit**.</span></span>
      
      * <span data-ttu-id="dc8d1-189">A **Spark-fürtök (csak Linux)**, válassza ki hello HDInsight Spark-fürt toorun kívánja az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-189">For **Spark clusters (Linux only)**, select hello HDInsight Spark cluster on which you want toorun your application.</span></span>

      * <span data-ttu-id="dc8d1-190">Összetevő hello IntelliJ projektben, vagy válasszon egy hello merevlemez-meghajtóról.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-190">Select an artifact from hello IntelliJ project, or select one from hello hard drive.</span></span>

      * <span data-ttu-id="dc8d1-191">A hello **fő osztálynév** mezőben, válassza hello három pont (**...** ), hello fő osztályban található az alkalmazás forráskódjához, majd válassza ki és **OK**.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-191">In hello **Main class name** box, select hello ellipsis (**...**), select hello main class in your application source code, and then select **OK**.</span></span>

        ![hello fő osztály kiválasztása párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-3.png)

      * <span data-ttu-id="dc8d1-193">Nem igényel parancssori argumentumot a hello alkalmazáskód ebben a példában, vagy hivatkozzon a JAR-fájlok kivételével vagy fájlokat, mert üres mezőkbe fennmaradó hello hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-193">Because hello application code in this example does not require command-line arguments or reference JARs or files, you can leave hello remaining boxes empty.</span></span> <span data-ttu-id="dc8d1-194">Miután megadta a hello kapcsolatos összes információ, hello párbeszédpanel kép a következő hello kell hasonlítania.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-194">After you provide all hello information, hello dialog box should resemble hello following image.</span></span>
        
        ![hello Spark küldésének párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-2.png)

   <span data-ttu-id="dc8d1-196">c.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-196">c.</span></span> <span data-ttu-id="dc8d1-197">Hello **Spark küldésének** hello ablak hello alján lapon kell kezdenie megjelenítése hello folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-197">hello **Spark Submission** tab at hello bottom of hello window should start displaying hello progress.</span></span> <span data-ttu-id="dc8d1-198">Hello alkalmazás állítsa le a hello piros hello gombra kattintva **Spark küldésének** ablak.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-198">You can also stop hello application by selecting hello red button in hello **Spark Submission** window.</span></span>
      
      ![hello Spark küldésének ablak](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-result.png)
      
      <span data-ttu-id="dc8d1-200">toolearn hogyan tooaccess hello feladatkiemenetét, tekintse meg a hello "hozzáférés és a HDInsight Spark-fürtök kezelése az intellij-t Azure eszközkészlet használatával" című szakaszban ebben a cikkben található.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-200">toolearn how tooaccess hello job output, see hello "Access and manage HDInsight Spark clusters by using Azure Toolkit for IntelliJ" section later in this article.</span></span>

## <a name="run-or-debug-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a><span data-ttu-id="dc8d1-201">Futtassa, vagy egy HDInsight Spark-fürt Spark Scala alkalmazások hibakeresése</span><span class="sxs-lookup"><span data-stu-id="dc8d1-201">Run or debug a Spark Scala application on an HDInsight Spark cluster</span></span>
<span data-ttu-id="dc8d1-202">Javasoljuk továbbá egy másik módszer a hello Spark alkalmazás toohello fürt elküldése.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-202">We also recommend another way of submitting hello Spark application toohello cluster.</span></span> <span data-ttu-id="dc8d1-203">Ehhez hello hello paraméterek beállításával **Futtatás/Debug konfigurációk** IDE.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-203">You can do so by setting hello parameters in hello **Run/Debug configurations** IDE.</span></span> <span data-ttu-id="dc8d1-204">További információkért lásd: [távolról az IntelliJ SSH-n keresztül a Azure eszközkészlet a HDInsight-fürtök a Spark-alkalmazások hibakeresését](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).</span><span class="sxs-lookup"><span data-stu-id="dc8d1-204">For more information, see [Remotely debug Spark applications on an HDInsight cluster with Azure Toolkit for IntelliJ through SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).</span></span>

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-azure-toolkit-for-intellij"></a><span data-ttu-id="dc8d1-205">Férhessen hozzá és felügyelhesse a HDInsight Spark-fürtjei IntelliJ Azure eszközkészlet használatával</span><span class="sxs-lookup"><span data-stu-id="dc8d1-205">Access and manage HDInsight Spark clusters by using Azure Toolkit for IntelliJ</span></span>
<span data-ttu-id="dc8d1-206">Az intellij-t Azure eszközkészlet használatával különféle műveleteket hajthat végre.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-206">You can perform various operations by using Azure Toolkit for IntelliJ.</span></span>

### <a name="access-hello-job-view"></a><span data-ttu-id="dc8d1-207">Hozzáférés hello feladat megtekintése</span><span class="sxs-lookup"><span data-stu-id="dc8d1-207">Access hello job view</span></span>
1. <span data-ttu-id="dc8d1-208">Az Azure Explorerben bontsa ki a **HDInsight**, bontsa ki hello Spark-fürt nevét, majd válassza ki **feladatok**.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-208">In Azure Explorer, expand **HDInsight**, expand hello Spark cluster name, and then select **Jobs**.</span></span>  

    ![Feladatok nézet csomópont](./media/hdinsight-apache-spark-intellij-tool-plugin/job-view-node.png)

2. <span data-ttu-id="dc8d1-210">A jobb oldali hello hello **Spark feladat megtekintése** lap hello fürtön futó összes hello-alkalmazást jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-210">In hello right pane, hello **Spark Job View** tab displays all hello applications that were run on hello cluster.</span></span> <span data-ttu-id="dc8d1-211">Válassza ki, amelynek toosee további részleteket szeretne hello alkalmazás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-211">Select hello name of hello application for which you want toosee more details.</span></span>

    ![Az alkalmazás részletei](./media/hdinsight-apache-spark-intellij-tool-plugin/view-job-logs.png)

3. <span data-ttu-id="dc8d1-213">toodisplay futó feladat alapinformációk, hello feladatgrafikon az egérmutatót.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-213">toodisplay basic running job information, hover over hello job graph.</span></span> <span data-ttu-id="dc8d1-214">tooview hello szakaszból grafikon és információt, amely minden feladatot hoz létre, válasszon ki egy csomópontot, a hello feladatgrafikon.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-214">tooview hello stages graph and information that every job generates, select a node on hello job graph.</span></span>

    ![Feladat szakasz részletei](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

4. <span data-ttu-id="dc8d1-216">például csak a gyakran használt naplókat, tooview *illesztőprogram Stderr*, *illesztőprogram Stdout*, és *Directory Info*, jelölje be hello **napló** fülre.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-216">tooview frequently used logs, such as *Driver Stderr*, *Driver Stdout*, and *Directory Info*, select hello **Log** tab.</span></span>

    ![Naplófájl részletei](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-log-info.png)

5. <span data-ttu-id="dc8d1-218">Hello Spark előzmények felhasználói felület és a YARN felhasználói felületen (hello alkalmazási szinten) hello hello ablak hello tetején hivatkozás kiválasztásával is megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-218">You can also view hello Spark history UI and hello YARN UI (at hello application level) by selecting a link at hello top of hello window.</span></span>

### <a name="access-hello-spark-history-server"></a><span data-ttu-id="dc8d1-219">Hello Spark előzmények kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="dc8d1-219">Access hello Spark history server</span></span>
1. <span data-ttu-id="dc8d1-220">Az Azure Explorerben bontsa ki a **HDInsight**, kattintson a jobb gombbal a Spark-fürt nevét, majd válassza ki **nyissa meg a Spark feladatelőzmények felhasználói felület**.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-220">In Azure Explorer, expand **HDInsight**, right-click your Spark cluster name, and then select **Open Spark History UI**.</span></span> 

2. <span data-ttu-id="dc8d1-221">Amikor a rendszer kéri, adja meg a hello fürt rendszergazdai hitelesítő adataival, a megadott hello fürt beállításakor.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-221">When you're prompted, enter hello cluster's admin credentials, which you specified when you set up hello cluster.</span></span>

3. <span data-ttu-id="dc8d1-222">Hello Spark előzmények server irányítópult használható hello alkalmazás neve toolook hello alkalmazás imént futása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-222">On hello Spark history server dashboard, you can use hello application name toolook for hello application that you just finished running.</span></span> <span data-ttu-id="dc8d1-223">Hello megelőző kódot, akkor be hello alkalmazásnév `val conf = new SparkConf().setAppName("MyClusterApp")`.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-223">In hello preceding code, you set hello application name by using `val conf = new SparkConf().setAppName("MyClusterApp")`.</span></span> <span data-ttu-id="dc8d1-224">A Spark alkalmazásnév ezért **MyClusterApp**.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-224">Therefore, your Spark application name is **MyClusterApp**.</span></span>

### <a name="start-hello-ambari-portal"></a><span data-ttu-id="dc8d1-225">Indítsa el a hello Ambari portál</span><span class="sxs-lookup"><span data-stu-id="dc8d1-225">Start hello Ambari portal</span></span>
1. <span data-ttu-id="dc8d1-226">Az Azure Explorerben bontsa ki a **HDInsight**, kattintson a jobb gombbal a Spark-fürt nevét, majd válassza ki **nyitott fürt Management Portal (Ambari)**.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-226">In Azure Explorer, expand **HDInsight**, right-click your Spark cluster name, and then select **Open Cluster Management Portal (Ambari)**.</span></span> 

2. <span data-ttu-id="dc8d1-227">Amikor a rendszer kéri, adja meg hello fürt hello rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-227">When you're prompted, enter hello admin credentials for hello cluster.</span></span> <span data-ttu-id="dc8d1-228">Ezeket a hitelesítő adatokat adott meg hello fürt beállítási folyamata során.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-228">You specified these credentials during hello cluster setup process.</span></span>

### <a name="manage-azure-subscriptions"></a><span data-ttu-id="dc8d1-229">Az Azure-előfizetések kezelése</span><span class="sxs-lookup"><span data-stu-id="dc8d1-229">Manage Azure subscriptions</span></span>
<span data-ttu-id="dc8d1-230">Alapértelmezés szerint az intellij-t Azure eszköztára hello Spark-fürtök az összes Azure-előfizetések sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-230">By default, Azure Toolkit for IntelliJ lists hello Spark clusters from all your Azure subscriptions.</span></span> <span data-ttu-id="dc8d1-231">Ha szükséges, megadhatja, hogy szeretné-e tooaccess hello előfizetések.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-231">If necessary, you can specify hello subscriptions that you want tooaccess.</span></span> 

1. <span data-ttu-id="dc8d1-232">Az Azure-kezelővel, kattintson a jobb gombbal hello **Azure** gyökércsomópont, és válassza ki **előfizetések kezelése oldalt**.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-232">In Azure Explorer, right-click hello **Azure** root node, and then select **Manage Subscriptions**.</span></span> 

2. <span data-ttu-id="dc8d1-233">A hello párbeszédpanelen törölje a hello jelölőnégyzetek következő toohello előfizetések, hogy nem szeretné, hogy tooaccess, és válassza ki **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-233">In hello dialog box, clear hello check boxes next toohello subscriptions that you don't want tooaccess, and then select **Close**.</span></span> <span data-ttu-id="dc8d1-234">Igény szerint kiválaszthatja **Kijelentkezés** toosign kívül az Azure-előfizetés tetszés.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-234">You can also select **Sign Out** if you want toosign out of your Azure subscription.</span></span>

## <a name="run-a-spark-scala-application-locally"></a><span data-ttu-id="dc8d1-235">A Spark Scala alkalmazás helyileg történő futtatása</span><span class="sxs-lookup"><span data-stu-id="dc8d1-235">Run a Spark Scala application locally</span></span>
<span data-ttu-id="dc8d1-236">Használhat Azure eszközkészlet IntelliJ toorun Spark Scala-alkalmazások helyileg a munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-236">You can use Azure Toolkit for IntelliJ toorun Spark Scala applications locally on your workstation.</span></span> <span data-ttu-id="dc8d1-237">hello alkalmazások általában nem kell erőforrásokhoz férnek hozzá toocluster, például a tárolókban, és futtatja, és helyben tesztelheti őket.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-237">hello applications usually don't need access toocluster resources, such as storage containers, and you can run and test them locally.</span></span>

### <a name="prerequisite"></a><span data-ttu-id="dc8d1-238">Előfeltétel</span><span class="sxs-lookup"><span data-stu-id="dc8d1-238">Prerequisite</span></span>
<span data-ttu-id="dc8d1-239">Hello helyi Spark Scala-alkalmazások Windows rendszerű számítógépeken futtatása, közben kivétel, kaphat, ahogy [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356).</span><span class="sxs-lookup"><span data-stu-id="dc8d1-239">While you're running hello local Spark Scala application on a Windows computer, you might get an exception, as explained in [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356).</span></span> <span data-ttu-id="dc8d1-240">hello kivétel következik be, mert WinUtils.exe hiányzik a Windows.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-240">hello exception occurs because WinUtils.exe is missing on Windows.</span></span> 

<span data-ttu-id="dc8d1-241">tooresolve Ez a hiba [végrehajtható hello letöltése](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) tooa helyen **C:\WinUtils\bin**.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-241">tooresolve this error, [download hello executable](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) tooa location such as **C:\WinUtils\bin**.</span></span> <span data-ttu-id="dc8d1-242">Adja hozzá a hello környezeti változó **HADOOP_HOME**, és állítsa be a hello hello változó értékének túl**C\WinUtils**.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-242">Then, add hello environment variable **HADOOP_HOME**, and set hello value of hello variable too**C\WinUtils**.</span></span>

### <a name="run-a-local-spark-scala-application"></a><span data-ttu-id="dc8d1-243">A helyi Spark Scala-alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="dc8d1-243">Run a local Spark Scala application</span></span>
1. <span data-ttu-id="dc8d1-244">Indítsa el az IntelliJ IDEA, és hozzon létre egy projektet.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-244">Start IntelliJ IDEA, and create a project.</span></span> 

2. <span data-ttu-id="dc8d1-245">A hello **új projekt** párbeszédpanel mezőbe hello a következő:</span><span class="sxs-lookup"><span data-stu-id="dc8d1-245">In hello **New Project** dialog box, do hello following:</span></span>
   
    <span data-ttu-id="dc8d1-246">a.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-246">a.</span></span> <span data-ttu-id="dc8d1-247">Válassza ki **HDInsight** > **a Spark on HDInsight helyi futtatási minta (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-247">Select **HDInsight** > **Spark on HDInsight Local Run Sample (Scala)**.</span></span>

    <span data-ttu-id="dc8d1-248">b.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-248">b.</span></span> <span data-ttu-id="dc8d1-249">A hello **Build eszköz** listára, válassza ki a következő, tooyour szükség szerint hello valamelyikét:</span><span class="sxs-lookup"><span data-stu-id="dc8d1-249">In hello **Build tool** list, select either of hello following, according tooyour need:</span></span>

      * <span data-ttu-id="dc8d1-250">**Maven**, Scala-projekt létrehozása varázsló támogatásához</span><span class="sxs-lookup"><span data-stu-id="dc8d1-250">**Maven**, for Scala project-creation wizard support</span></span>
      * <span data-ttu-id="dc8d1-251">**SBT**, hello függőségek kezelése és hello Scala-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="dc8d1-251">**SBT**, for managing hello dependencies and building for hello Scala project</span></span>

    ![hello új projekt párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run.png)

3. <span data-ttu-id="dc8d1-253">Válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-253">Select **Next**.</span></span>
 
4. <span data-ttu-id="dc8d1-254">Az hello hello következő ablakban, a következő:</span><span class="sxs-lookup"><span data-stu-id="dc8d1-254">In hello next window, do hello following:</span></span>
   
    <span data-ttu-id="dc8d1-255">a.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-255">a.</span></span> <span data-ttu-id="dc8d1-256">Adja meg a projekt nevét és helyét.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-256">Enter a project name and location.</span></span>

    <span data-ttu-id="dc8d1-257">b.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-257">b.</span></span> <span data-ttu-id="dc8d1-258">A hello **projekt SDK** legördülő listára, válassza ki a 1.7 verziónál újabb Java-verziót.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-258">In hello **Project SDK** drop-down list, select a Java version that's later than version 1.7.</span></span>

    <span data-ttu-id="dc8d1-259">c.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-259">c.</span></span> <span data-ttu-id="dc8d1-260">A hello **Spark verzió** legördülő listában, jelölje be hello verzióját Scala, amelyet az toouse: Scala Spark 2.0-s vagy Scala 2.11.x 2.10.x a Spark 1.6-os.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-260">In hello **Spark Version** drop-down list, select hello version of Scala that you want toouse: Scala 2.11.x for Spark 2.0 or Scala 2.10.x for Spark 1.6.</span></span>

    ![hello új projekt párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/Create-local-project.PNG)

5. <span data-ttu-id="dc8d1-262">Válassza a **Finish** (Befejezés) elemet.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-262">Select **Finish**.</span></span>

6. <span data-ttu-id="dc8d1-263">hello sablon hozzáadása egy mintakód (**LogQuery**) alatt hello **src** mappát, amelyet a számítógépen helyileg futtathat.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-263">hello template adds a sample code (**LogQuery**) under hello **src** folder that you can run locally on your computer.</span></span>
   
    ![LogQuery helye](./media/hdinsight-apache-spark-intellij-tool-plugin/local-app.png)

7. <span data-ttu-id="dc8d1-265">Kattintson a jobb gombbal hello **LogQuery** alkalmazás, és adja **Futtatás "LogQuery"**.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-265">Right-click hello **LogQuery** application, and then select **Run 'LogQuery'**.</span></span> <span data-ttu-id="dc8d1-266">A hello **futtatása** lapon hello alján hello hasonló kimenetnek lásd:</span><span class="sxs-lookup"><span data-stu-id="dc8d1-266">On hello **Run** tab at hello bottom, you see an output like hello following:</span></span>
   
   ![Spark alkalmazás helyi futtatás eredménye](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="convert-existing-intellij-idea-applications-toouse-azure-toolkit-for-intellij"></a><span data-ttu-id="dc8d1-268">Az IntelliJ alakítsa át a meglévő IntelliJ IDEA alkalmazások toouse Azure eszközkészlet</span><span class="sxs-lookup"><span data-stu-id="dc8d1-268">Convert existing IntelliJ IDEA applications toouse Azure Toolkit for IntelliJ</span></span>
<span data-ttu-id="dc8d1-269">Hello meglévő Spark Scala alkalmazások meg az IntelliJ IDEA toobe kompatibilis Azure eszközkészlet az IntelliJ válthat.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-269">You can convert hello existing Spark Scala applications that you created in IntelliJ IDEA toobe compatible with Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="dc8d1-270">Ezután használhatja a hello beépülő modul toosubmit hello alkalmazások tooan HDInsight Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-270">You can then use hello plug-in toosubmit hello applications tooan HDInsight Spark cluster.</span></span>

1. <span data-ttu-id="dc8d1-271">Egy meglévő Spark Scala-alkalmazás, amely az IntelliJ IDEA használatával hozták létre nyissa meg a hello társított .iml fájlt.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-271">For an existing Spark Scala application that was created through IntelliJ IDEA, open hello associated .iml file.</span></span>

2. <span data-ttu-id="dc8d1-272">Hello gyökerében szintje a **modul** hello következő elem:</span><span class="sxs-lookup"><span data-stu-id="dc8d1-272">At hello root level is a **module** element like hello following:</span></span>
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4">

   <span data-ttu-id="dc8d1-273">Hello elem tooadd szerkesztése `UniqueKey="HDInsightTool"` úgy, hogy hello **modul** elem következőhöz hasonló hello:</span><span class="sxs-lookup"><span data-stu-id="dc8d1-273">Edit hello element tooadd `UniqueKey="HDInsightTool"` so that hello **module** element looks like hello following:</span></span>
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4" UniqueKey="HDInsightTool">

3. <span data-ttu-id="dc8d1-274">Hello módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-274">Save hello changes.</span></span> <span data-ttu-id="dc8d1-275">Az alkalmazás most már Azure eszköztára IntelliJ kompatibilisnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-275">Your application should now be compatible with Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="dc8d1-276">Kattintson a jobb gombbal a Project Explorer hello projektnevet tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-276">You can test it by right-clicking hello project name in Project Explorer.</span></span> <span data-ttu-id="dc8d1-277">hello előugró menüjét most már hello beállítás **küldje el a külső alkalmazás tooHDInsight**.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-277">hello pop-up menu now has hello option **Submit Spark Application tooHDInsight**.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="dc8d1-278">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="dc8d1-278">Troubleshooting</span></span>

### <a name="error-in-local-run-please-use-a-larger-heap-size"></a><span data-ttu-id="dc8d1-279">Hiba történt a helyi futtatáskor: *használjon egy nagyobb halommemória mérete*</span><span class="sxs-lookup"><span data-stu-id="dc8d1-279">Error in local run: *Please use a larger heap size*</span></span>
<span data-ttu-id="dc8d1-280">A Spark 1.6 használata egy 32 bites Java SDK helyi futtatás során léphetnek fel a következő hibák hello:</span><span class="sxs-lookup"><span data-stu-id="dc8d1-280">In Spark 1.6, if you're using a 32-bit Java SDK during local run, you might encounter hello following errors:</span></span>

    Exception in thread "main" java.lang.IllegalArgumentException: System memory 259522560 must be at least 4.718592E8. Please use a larger heap size.
        at org.apache.spark.memory.UnifiedMemoryManager$.getMaxMemory(UnifiedMemoryManager.scala:193)
        at org.apache.spark.memory.UnifiedMemoryManager$.apply(UnifiedMemoryManager.scala:175)
        at org.apache.spark.SparkEnv$.create(SparkEnv.scala:354)
        at org.apache.spark.SparkEnv$.createDriverEnv(SparkEnv.scala:193)
        at org.apache.spark.SparkContext.createSparkEnv(SparkContext.scala:288)
        at org.apache.spark.SparkContext.<init>(SparkContext.scala:457)
        at LogQuery$.main(LogQuery.scala:53)
        at LogQuery.main(LogQuery.scala)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:606)
        at com.intellij.rt.execution.application.AppMain.main(AppMain.java:144)

<span data-ttu-id="dc8d1-281">Ezek a hibák akkor fordulhat elő, mert hello halommemória mérete nem elég nagy a Spark toorun.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-281">These errors happen because hello heap size is not large enough for Spark toorun.</span></span> <span data-ttu-id="dc8d1-282">Spark legalább 471 MB terület szükséges.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-282">Spark requires at least 471 MB.</span></span> <span data-ttu-id="dc8d1-283">(További információkért lásd: [SPARK-12081](https://issues.apache.org/jira/browse/SPARK-12081).) Egy egyszerű megoldást egy 64 bites Java SDK toouse.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-283">(For more information, see [SPARK-12081](https://issues.apache.org/jira/browse/SPARK-12081).) One simple solution is toouse a 64-bit Java SDK.</span></span> <span data-ttu-id="dc8d1-284">Adja hozzá az alábbi beállítások hello IntelliJ hello JVM beállításait is módosíthatja:</span><span class="sxs-lookup"><span data-stu-id="dc8d1-284">You can also change hello JVM settings in IntelliJ by adding hello following options:</span></span>

    -Xms128m -Xmx512m -XX:MaxPermSize=300m -ea

!["Virtuális gép beállításai" mezőben beállítások toohello hozzáadása az IntelliJ](./media/hdinsight-apache-spark-intellij-tool-plugin/change-heap-size.png)

## <a name="faq"></a><span data-ttu-id="dc8d1-286">GYIK</span><span class="sxs-lookup"><span data-stu-id="dc8d1-286">FAQ</span></span>
<span data-ttu-id="dc8d1-287">Válasszon egy alkalmazás tooAzure Data Lake Store toosubmit **interaktív** mód hello Azure bejelentkezés során.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-287">toosubmit an application tooAzure Data Lake Store, choose **Interactive** mode during hello Azure sign-in process.</span></span> <span data-ttu-id="dc8d1-288">Ha **automatikus** mód, hibaüzenetet kaphat.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-288">If you select **Automated** mode, you can get an error.</span></span>

![interaktív bejelentkezés](./media/hdinsight-apache-spark-intellij-tool-plugin/interative-signin.png)

<span data-ttu-id="dc8d1-290">Most azt feloldotta azt.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-290">Now, we resolved it.</span></span> <span data-ttu-id="dc8d1-291">Választhat egy Azure Data Lake fürt toosubmit az alkalmazás egyik bejelentkezési módszer.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-291">You can choose an Azure Data Lake Cluster toosubmit your application with any sign-in method.</span></span>

## <a name="feedback-and-known-issues"></a><span data-ttu-id="dc8d1-292">Visszajelzések és ismert problémák</span><span class="sxs-lookup"><span data-stu-id="dc8d1-292">Feedback and known issues</span></span>
<span data-ttu-id="dc8d1-293">Spark kimenetek közvetlenül megtekintése jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-293">Currently, viewing Spark outputs directly is not supported.</span></span>

<span data-ttu-id="dc8d1-294">Ha javaslata vagy visszajelzést, vagy ha ez a beépülő modul használatakor bármely problémákat tapasztal, e-mail nekünk az hdivstool@microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="dc8d1-294">If you have any suggestions or feedback, or if you encounter any problems when you use this plug-in, email us at hdivstool@microsoft.com.</span></span>

## <span data-ttu-id="dc8d1-295"><a name="seealso"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dc8d1-295"><a name="seealso"></a>Next steps</span></span>
* [<span data-ttu-id="dc8d1-296">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="dc8d1-296">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="demo"></a><span data-ttu-id="dc8d1-297">Bemutató</span><span class="sxs-lookup"><span data-stu-id="dc8d1-297">Demo</span></span>
* <span data-ttu-id="dc8d1-298">Hozzon létre Scala project (videó): [Spark Scala-alkalmazások létrehozása](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="dc8d1-298">Create Scala project (video): [Create Spark Scala Applications](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span></span>
* <span data-ttu-id="dc8d1-299">Távoli hibakeresési (videó): [IntelliJ toodebug Spark-alkalmazások távolról a HDInsight-fürt használata Azure eszköztára](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="dc8d1-299">Remote debug (video): [Use Azure Toolkit for IntelliJ toodebug Spark applications remotely on HDInsight Cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span></span>

### <a name="scenarios"></a><span data-ttu-id="dc8d1-300">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="dc8d1-300">Scenarios</span></span>
* [<span data-ttu-id="dc8d1-301">Spark és BI: interaktív adatelemzés végrehajtása a Spark hdinsight BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="dc8d1-301">Spark with BI: Perform interactive data analysis by using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="dc8d1-302">Spark és Machine Learning: használja a Spark on HDInsight tooanalyze épület-hőmérséklet HVAC-adatok</span><span class="sxs-lookup"><span data-stu-id="dc8d1-302">Spark with Machine Learning: Use Spark in HDInsight tooanalyze building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="dc8d1-303">Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények</span><span class="sxs-lookup"><span data-stu-id="dc8d1-303">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="dc8d1-304">Spark Streaming: Használja a Spark on HDInsight toobuild valós idejű streamelési alkalmazások</span><span class="sxs-lookup"><span data-stu-id="dc8d1-304">Spark Streaming: Use Spark in HDInsight toobuild real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="dc8d1-305">A webhelynapló elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="dc8d1-305">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a><span data-ttu-id="dc8d1-306">Létrehozása és alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="dc8d1-306">Creating and running applications</span></span>
* [<span data-ttu-id="dc8d1-307">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="dc8d1-307">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="dc8d1-308">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="dc8d1-308">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="dc8d1-309">Eszközök és bővítmények</span><span class="sxs-lookup"><span data-stu-id="dc8d1-309">Tools and extensions</span></span>
* [<span data-ttu-id="dc8d1-310">Az IntelliJ toodebug Spark-alkalmazások VPN-en keresztül távolról használható Azure eszköztára</span><span class="sxs-lookup"><span data-stu-id="dc8d1-310">Use Azure Toolkit for IntelliJ toodebug Spark applications remotely through VPN</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="dc8d1-311">Az IntelliJ toodebug Spark-alkalmazások SSH keresztül távolról használható Azure eszköztára</span><span class="sxs-lookup"><span data-stu-id="dc8d1-311">Use Azure Toolkit for IntelliJ toodebug Spark applications remotely through SSH</span></span>](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [<span data-ttu-id="dc8d1-312">Használja a HDInsight Tools for IntelliJ a Hortonworks védőfal</span><span class="sxs-lookup"><span data-stu-id="dc8d1-312">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [<span data-ttu-id="dc8d1-313">HDInsight-eszközök használata az Eclipse toocreate Spark-alkalmazások Azure eszköztára</span><span class="sxs-lookup"><span data-stu-id="dc8d1-313">Use HDInsight Tools in Azure Toolkit for Eclipse toocreate Spark applications</span></span>](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [<span data-ttu-id="dc8d1-314">Zeppelin notebookok használata Spark-fürttel HDInsighton</span><span class="sxs-lookup"><span data-stu-id="dc8d1-314">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="dc8d1-315">Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében</span><span class="sxs-lookup"><span data-stu-id="dc8d1-315">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="dc8d1-316">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="dc8d1-316">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="dc8d1-317">Jupyter telepítse a számítógépre, és csatlakozzon a HDInsight Spark-fürt tooan</span><span class="sxs-lookup"><span data-stu-id="dc8d1-317">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a><span data-ttu-id="dc8d1-318">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="dc8d1-318">Managing resources</span></span>
* [<span data-ttu-id="dc8d1-319">Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése</span><span class="sxs-lookup"><span data-stu-id="dc8d1-319">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="dc8d1-320">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="dc8d1-320">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

