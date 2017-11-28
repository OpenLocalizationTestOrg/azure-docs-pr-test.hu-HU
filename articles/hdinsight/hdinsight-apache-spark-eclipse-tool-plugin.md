---
title: "aaaAzure Eclipse - alkalmazások HDInsight Spark Scala létrehozása eszköztára |} Microsoft Docs"
description: "A HDInsight Tools használni az Azure-eszközkészlet Eclipse toodevelop Spark-alkalmazások scalában írt és küldheti el ezeket a HDInsight Spark-fürt tooan, azokat közvetlenül a hello Eclipse IDE."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: f6c79550-5803-4e13-b541-e86c4abb420b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: nitinme
ms.openlocfilehash: 3ab70857c1e81f591a1c7e29bc1706ec4899ff58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-toolkit-for-eclipse-toocreate-spark-applications-for-an-hdinsight-cluster"></a><span data-ttu-id="d6313-103">Azure eszközkészlet használata az Eclipse toocreate Spark-alkalmazások HDInsight-fürtök</span><span class="sxs-lookup"><span data-stu-id="d6313-103">Use Azure Toolkit for Eclipse toocreate Spark applications for an HDInsight cluster</span></span>

<span data-ttu-id="d6313-104">A HDInsight Tools használni az Azure-eszközkészlet Eclipse toodevelop Spark-alkalmazások scalában írt, és küldje el azokat tooan Azure HDInsight Spark-fürt közvetlenül a hello Eclipse IDE.</span><span class="sxs-lookup"><span data-stu-id="d6313-104">Use HDInsight Tools in Azure Toolkit for Eclipse toodevelop Spark applications written in Scala and submit them tooan Azure HDInsight Spark cluster, directly from hello Eclipse IDE.</span></span> <span data-ttu-id="d6313-105">Hello HDInsight eszközök beépülő modul néhány különböző módokon használhatja:</span><span class="sxs-lookup"><span data-stu-id="d6313-105">You can use hello HDInsight Tools plug-in in a few different ways:</span></span>

* <span data-ttu-id="d6313-106">toodevelop, és küldje el a Scala Spark-alkalmazás egy HDInsight Spark-fürt</span><span class="sxs-lookup"><span data-stu-id="d6313-106">toodevelop and submit a Scala Spark application on an HDInsight Spark cluster</span></span>
* <span data-ttu-id="d6313-107">tooaccess az Azure HDInsight Spark-fürt erőforrások</span><span class="sxs-lookup"><span data-stu-id="d6313-107">tooaccess your Azure HDInsight Spark cluster resources</span></span>
* <span data-ttu-id="d6313-108">toodevelop, és futtassa helyileg a Scala Spark-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="d6313-108">toodevelop and run a Scala Spark application locally</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d6313-109">Ez az eszköz használt toocreate kell, és küldje el az alkalmazásokat csak egy HDInsight Spark-fürt Linux rendszeren.</span><span class="sxs-lookup"><span data-stu-id="d6313-109">This tool can be used toocreate and submit applications only for an HDInsight Spark cluster on Linux.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="d6313-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d6313-110">Prerequisites</span></span>

* <span data-ttu-id="d6313-111">A HDInsight az Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="d6313-111">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="d6313-112">Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="d6313-112">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>
* <span data-ttu-id="d6313-113">Oracle Java fejlesztői készlet 8-as verzió, amely hello Eclipse IDE futásidejű használható.</span><span class="sxs-lookup"><span data-stu-id="d6313-113">Oracle Java Development Kit version 8, which is used for hello Eclipse IDE runtime.</span></span> <span data-ttu-id="d6313-114">Letölthető hello [Oracle webhely](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="d6313-114">You can download it from hello [Oracle website](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
* <span data-ttu-id="d6313-115">Eclipse IDE.</span><span class="sxs-lookup"><span data-stu-id="d6313-115">Eclipse IDE.</span></span> <span data-ttu-id="d6313-116">Ebben a cikkben az Eclipse Neonfény.</span><span class="sxs-lookup"><span data-stu-id="d6313-116">This article uses Eclipse Neon.</span></span> <span data-ttu-id="d6313-117">A későbbiekben telepítheti az hello [Eclipse webhely](https://www.eclipse.org/downloads/).</span><span class="sxs-lookup"><span data-stu-id="d6313-117">You can install it from hello [Eclipse website](https://www.eclipse.org/downloads/).</span></span>   
* <span data-ttu-id="d6313-118">Spark SDK.</span><span class="sxs-lookup"><span data-stu-id="d6313-118">Spark SDK.</span></span> <span data-ttu-id="d6313-119">Letöltheti a [GitHub](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="d6313-119">You can download it from [GitHub](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409).</span></span>


## <a name="install-hdinsight-tools-in-azure-toolkit-for-eclipse-and-scala-plugin"></a><span data-ttu-id="d6313-120">A HDInsight Tools Azure eszközkészlet az eclipse-ben és a Scala beépülő modul telepítése</span><span class="sxs-lookup"><span data-stu-id="d6313-120">Install HDInsight Tools in Azure Toolkit for Eclipse and Scala Plugin</span></span>
### <a name="install-hdinsight-tools"></a><span data-ttu-id="d6313-121">HDInsight eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="d6313-121">Install HDInsight Tools</span></span>
<span data-ttu-id="d6313-122">A HDInsight Tools for eclipse-ben érhető el az Eclipse Azure eszköztára részeként.</span><span class="sxs-lookup"><span data-stu-id="d6313-122">HDInsight Tools for Eclipse is available as part of Azure Toolkit for Eclipse.</span></span> <span data-ttu-id="d6313-123">A telepítési utasításokért lásd: [Azure eszközkészlet telepítése az Eclipse](../azure-toolkit-for-eclipse-installation.md).</span><span class="sxs-lookup"><span data-stu-id="d6313-123">For installation instructions, see [Installing Azure Toolkit for Eclipse](../azure-toolkit-for-eclipse-installation.md).</span></span>
### <a name="install-scala-plugin"></a><span data-ttu-id="d6313-124">Scala beépülő modul telepítése</span><span class="sxs-lookup"><span data-stu-id="d6313-124">Install Scala Plugin</span></span>
<span data-ttu-id="d6313-125">Az Intellij hello megnyitásakor hello HDInsight eszközök automatikus észleli, hogy Scala beépülő modul telepítve, vagy nem.</span><span class="sxs-lookup"><span data-stu-id="d6313-125">When you open hello Intellij, hello HDInsight Tools auto detects whether you installed Scala plugin or not.</span></span> <span data-ttu-id="d6313-126">Kattintson a **OK** toocontinue és követi hello utasításokat tooinstall hello Eclipse piactér által.</span><span class="sxs-lookup"><span data-stu-id="d6313-126">Click **OK** toocontinue and follow hello instructions tooinstall by hello Eclipse Marketplace.</span></span>

 ![Automatikus telepítés Scala beépülő modul](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-scala.png)

## <a name="sign-in-tooyour-azure-subscription"></a><span data-ttu-id="d6313-128">Jelentkezzen be tooyour Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="d6313-128">Sign in tooyour Azure subscription</span></span>
1. <span data-ttu-id="d6313-129">Indítsa el a hello Eclipse IDE, és nyissa meg az Azure-kezelővel.</span><span class="sxs-lookup"><span data-stu-id="d6313-129">Start hello Eclipse IDE and open Azure Explorer.</span></span> <span data-ttu-id="d6313-130">A hello **ablak** menüben kattintson a **nézet megjelenítése**, és kattintson a **más**.</span><span class="sxs-lookup"><span data-stu-id="d6313-130">On hello **Window** menu, click **Show View**, and then click **Other**.</span></span> <span data-ttu-id="d6313-131">A hello párbeszédpanel, bontsa ki a **Azure**, kattintson a **Azure Explorer**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="d6313-131">In hello dialog box that opens, expand **Azure**, click **Azure Explorer**, and then click **OK**.</span></span>

    ![Nézet párbeszédpanel megjelenítése](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-1.png)
2. <span data-ttu-id="d6313-133">Kattintson a jobb gombbal hello **Azure** csomópontra, majd **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d6313-133">Right-click hello **Azure** node, and then click **Sign in**.</span></span>
3. <span data-ttu-id="d6313-134">A hello **Azure bejelentkezés** párbeszédpanel, hello hitelesítési módszer kiválasztása párbeszédpanelen kattintson **jelentkezzen be a**, és az Azure hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="d6313-134">In hello **Azure Sign In** dialog box, choose hello authentication method, click **Sign in**, and enter your Azure credentials.</span></span>
   
    ![Az Azure bejelentkezési párbeszédpanel](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-2.png)
4. <span data-ttu-id="d6313-136">Be van jelentkezve, miután hello **előfizetések kiválasztása** párbeszédpanel bezárásához listák összes hello Azure-előfizetéssel társított hello hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="d6313-136">After you're signed in, hello **Select Subscriptions** dialog box lists all hello Azure subscriptions associated with hello credentials.</span></span> <span data-ttu-id="d6313-137">Kattintson a **válasszon** tooclose hello párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="d6313-137">Click **Select** tooclose hello dialog box.</span></span>

    ![Válassza ki az előfizetések párbeszédpanel](./media/hdinsight-apache-spark-eclipse-tool-plugin/Select-Subscriptions.png)
5. <span data-ttu-id="d6313-139">A hello **Azure Explorer** lapján bontsa ki a **HDInsight** HDInsight Spark-fürtök az előfizetéshez tartozó toosee hello.</span><span class="sxs-lookup"><span data-stu-id="d6313-139">On hello **Azure Explorer** tab, expand **HDInsight** toosee hello HDInsight Spark clusters under your subscription.</span></span>
   
    ![HDInsight Spark-fürtjei az Azure-kezelővel](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-3.png)
6. <span data-ttu-id="d6313-141">Ennél jobban is kibonthatja a fürt neve csomópont toosee hello erőforrások (például storage-fiókok) hello-fürthöz tartozó.</span><span class="sxs-lookup"><span data-stu-id="d6313-141">You can further expand a cluster name node toosee hello resources (for example, storage accounts) associated with hello cluster.</span></span>
   
    ![A fürterőforrások toosee kibontása](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-4.png)



## <a name="set-up-a-spark-scala-project-for-an-hdinsight-spark-cluster"></a><span data-ttu-id="d6313-143">A Spark Scala-projektjét, amely egy HDInsight Spark-fürt beállítása</span><span class="sxs-lookup"><span data-stu-id="d6313-143">Set up a Spark Scala project for an HDInsight Spark cluster</span></span>

1. <span data-ttu-id="d6313-144">Hello Eclipse IDE munkaterületen kattintson **fájl**, kattintson a **új**, és kattintson a **projekt**.</span><span class="sxs-lookup"><span data-stu-id="d6313-144">In hello Eclipse IDE workspace, click **File**, click **New**, and then click **Project**.</span></span> 
2. <span data-ttu-id="d6313-145">A hello új projekt varázsló, bontsa ki a **HDInsight**, jelölje be **a Spark on HDInsight (Scala)**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="d6313-145">In hello New Project wizard, expand **HDInsight**, select **Spark on HDInsight (Scala)**, and then click **Next**.</span></span>

    ![Hello Spark on HDInsight (Scala) projekt kiválasztása](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-2.png)
3. <span data-ttu-id="d6313-147">hello Scala projekt létrehozása varázsló automatikusan észleli, hogy Scala beépülő modul telepítve, vagy nem.</span><span class="sxs-lookup"><span data-stu-id="d6313-147">hello Scala project creation wizard auto detects whether you installed Scala plugin or not.</span></span> <span data-ttu-id="d6313-148">Kattintson a **OK** toocontinue hello Scala beépülő modult, majd hajtsa végre hello utasításokat toorestart Eclipse letöltése.</span><span class="sxs-lookup"><span data-stu-id="d6313-148">Click **OK** toocontinue downloading hello Scala plugin, then follow hello instructions toorestart Eclipse.</span></span>

    ![scala ellenőrzése](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-scala-2.png)
4. <span data-ttu-id="d6313-150">A hello **új HDInsight Scala projekt** párbeszédpanelen adja meg a következő értékek hello, és kattintson **következő**:</span><span class="sxs-lookup"><span data-stu-id="d6313-150">In hello **New HDInsight Scala Project** dialog box, provide hello following values, and then click **Next**:</span></span>
   * <span data-ttu-id="d6313-151">Adja meg a hello projekt nevét.</span><span class="sxs-lookup"><span data-stu-id="d6313-151">Enter a name for hello project.</span></span>
   * <span data-ttu-id="d6313-152">A hello **JRE** területen győződjön meg arról, hogy **használja a végrehajtási környezet JRE** értéke túl**JavaSE-1.7** vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="d6313-152">In hello **JRE** area, make sure that **Use an execution environment JRE** is set too**JavaSE-1.7** or later.</span></span>
   * <span data-ttu-id="d6313-153">Győződjön meg arról, hogy a Spark SDK hello SDK kezelőportálon toohello hely beállítása.</span><span class="sxs-lookup"><span data-stu-id="d6313-153">Make sure that Spark SDK is set toohello location where you downloaded hello SDK.</span></span> <span data-ttu-id="d6313-154">hello hivatkozás toohello letöltési hely megtalálható hello [Előfeltételek](#prerequisites) korábbi ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="d6313-154">hello link toohello download location is included in hello [prerequisites](#prerequisites) earlier in this article.</span></span> <span data-ttu-id="d6313-155">Is letölthetők hello SDK hello hello párbeszédpanel szereplő hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="d6313-155">You can also download hello SDK from hello link included in hello dialog box.</span></span>

    ![Új HDInsight Scala projekt párbeszédpanel](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-3.png)
5.  <span data-ttu-id="d6313-157">A következő párbeszédpanelen hello, kattintson a hello **szalagtárak** lapon, és tartsa hello alapértelmezett beállításokat, majd kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="d6313-157">In hello next dialog box, click hello **Libraries** tab and keep hello defaults, and then click **Finish**.</span></span> 
   
    ![Szalagtárak lap](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-4.png)
  
## <a name="create-a-scala-application-for-an-hdinsight-spark-cluster"></a><span data-ttu-id="d6313-159">A HDInsight Spark-fürtök Scala-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="d6313-159">Create a Scala application for an HDInsight Spark cluster</span></span>

1. <span data-ttu-id="d6313-160">Kattintson a jobb gombbal a hello Eclipse IDE, a csomag Explorer eszközből bontsa ki a korábban létrehozott hello projekt **src**, pont túl**új**, és kattintson a **más**.</span><span class="sxs-lookup"><span data-stu-id="d6313-160">In hello Eclipse IDE, from Package Explorer, expand hello project that you created earlier, right-click **src**, point too**New**, and then click **Other**.</span></span>
2. <span data-ttu-id="d6313-161">A hello **válassza ki a varázsló** párbeszédpanelen bontsa ki **Scala varázslók**, kattintson a **Scala objektum**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="d6313-161">In hello **Select a wizard** dialog box, expand **Scala Wizards**, click **Scala Object**, and then click **Next**.</span></span>
   
    ![Válassza ki a varázsló párbeszédpanelje](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-1.png)
3. <span data-ttu-id="d6313-163">A hello **hozzon létre új fájl** párbeszédpanelen írja be a hello objektum nevét, és kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="d6313-163">In hello **Create New File** dialog box, enter a name for hello object, and then click **Finish**.</span></span>
   
    ![Hozzon létre új fájl párbeszédpanel](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-2.png)
4. <span data-ttu-id="d6313-165">Illessze be a következő kód hello szövegszerkesztőben hello:</span><span class="sxs-lookup"><span data-stu-id="d6313-165">Paste hello following code in hello text editor:</span></span>
   
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
   
        object MyClusterApp{
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("MyClusterApp")
            val sc = new SparkContext(conf)
   
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
            //find hello rows that have only one digit in hello seventh column in hello CSV
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
   
            rdd1.saveAsTextFile("wasb:///HVACOut")
          }        
        }
5. <span data-ttu-id="d6313-166">Hello az alkalmazás futtatása egy HDInsight Spark-fürt:</span><span class="sxs-lookup"><span data-stu-id="d6313-166">Run hello application on an HDInsight Spark cluster:</span></span>
   
   1. <span data-ttu-id="d6313-167">A csomag Explorer, kattintson a jobb gombbal a hello projekt nevét, majd válassza ki **küldje el a külső alkalmazás tooHDInsight**.</span><span class="sxs-lookup"><span data-stu-id="d6313-167">From Package Explorer, right-click hello project name, and then select **Submit Spark Application tooHDInsight**.</span></span>        
   2. <span data-ttu-id="d6313-168">A hello **Spark küldésének** párbeszédpanelen adja meg a következő értékek hello, és kattintson **Submit**:</span><span class="sxs-lookup"><span data-stu-id="d6313-168">In hello **Spark Submission** dialog box, provide hello following values, and then click **Submit**:</span></span>
      
      * <span data-ttu-id="d6313-169">A **fürtnév**, válassza ki hello HDInsight Spark-fürt toorun kívánja az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d6313-169">For **Cluster Name**, select hello HDInsight Spark cluster on which you want toorun your application.</span></span>
      * <span data-ttu-id="d6313-170">Válasszon egy közbülső hello Eclipse-projekt, vagy válasszon ki egy, a merevlemez-meghajtóról.</span><span class="sxs-lookup"><span data-stu-id="d6313-170">Select an artifact from hello Eclipse project, or select one from a hard drive.</span></span> <span data-ttu-id="d6313-171">hello alapértelmezett érték a jobb gombbal a csomag explorer hello elem függ.</span><span class="sxs-lookup"><span data-stu-id="d6313-171">hello default value depends on hello item you right-click from package explorer.</span></span>
      * <span data-ttu-id="d6313-172">A hello **fő osztálynév** dropdownlist, küldésének varázsló megjeleníti a kijelölt projektből az összes objektum nevét.</span><span class="sxs-lookup"><span data-stu-id="d6313-172">In hello **Main class name** dropdownlist, submission wizard displays all object names from your selected project.</span></span> <span data-ttu-id="d6313-173">Válassza ki vagy adjon meg egy megjeleníteni kívánt toorun.</span><span class="sxs-lookup"><span data-stu-id="d6313-173">Select or input one that you want toorun.</span></span> <span data-ttu-id="d6313-174">Összetevő merevlemezről válassza ki, ha saját maga által megadott kell fő osztály nevét.</span><span class="sxs-lookup"><span data-stu-id="d6313-174">If you select artifact from hard disk, you need input main class name by yourself.</span></span> 
      * <span data-ttu-id="d6313-175">Nem igényel parancssori argumentumokat a hello alkalmazáskód ebben a példában, vagy hivatkozzon a JAR-fájlok kivételével vagy fájlokat, mert üres szövegmezőkben fennmaradó hello hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="d6313-175">Because hello application code in this example does not require any command-line arguments or reference JARs or files, you can leave hello remaining text boxes empty.</span></span>
        
       ![Spark küldésének párbeszédpanel](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-3.png)
   3. <span data-ttu-id="d6313-177">Hello **Spark küldésének** lapon kell kezdenie megjelenítése hello folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="d6313-177">hello **Spark Submission** tab should start displaying hello progress.</span></span> <span data-ttu-id="d6313-178">Hello alkalmazás hello piros hello gombra kattintva leállíthatja **Spark küldésének** ablak.</span><span class="sxs-lookup"><span data-stu-id="d6313-178">You can stop hello application by clicking hello red button in hello **Spark Submission** window.</span></span> <span data-ttu-id="d6313-179">Hello naplók az adott alkalmazás futtassa hello földgömb ikon (hello kék mező hello kép jelölik) kattintva is megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="d6313-179">You can also view hello logs for this specific application run by clicking hello globe icon (denoted by hello blue box in hello image).</span></span>
      
       ![Spark küldésének ablak](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-4.png)

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-hdinsight-tools-in-azure-toolkit-for-eclipse"></a><span data-ttu-id="d6313-181">Férhessen hozzá és felügyelhesse a HDInsight Spark-fürtjei a HDInsight Tools használatával Azure eszközkészlet az eclipse-ben</span><span class="sxs-lookup"><span data-stu-id="d6313-181">Access and manage HDInsight Spark clusters by using HDInsight Tools in Azure Toolkit for Eclipse</span></span>
<span data-ttu-id="d6313-182">A HDInsight Tools történő hozzáféréshez hello feladatkiemenetét használatával különféle műveleteket hajthat végre.</span><span class="sxs-lookup"><span data-stu-id="d6313-182">You can perform various operations by using HDInsight Tools, including accessing hello job output.</span></span>

### <a name="access-hello-job-view"></a><span data-ttu-id="d6313-183">Hozzáférés hello feladat megtekintése</span><span class="sxs-lookup"><span data-stu-id="d6313-183">Access hello job view</span></span>
1. <span data-ttu-id="d6313-184">Az Azure Explorerben bontsa ki a **HDInsight**, bontsa ki hello Spark-fürt nevét, és kattintson a **feladatok**.</span><span class="sxs-lookup"><span data-stu-id="d6313-184">In Azure Explorer, expand **HDInsight**, expand hello Spark cluster name, and then click **Jobs**.</span></span> 

    ![Feladatok nézet csomópont](./media/hdinsight-apache-spark-intellij-tool-plugin/job-view-node.png)

2. <span data-ttu-id="d6313-186">Kattintson a hello **feladatok** csomópont.</span><span class="sxs-lookup"><span data-stu-id="d6313-186">Click on hello **Jobs** node.</span></span> <span data-ttu-id="d6313-187">hello HDInsight eszközök automatikus-észleli, hogy hello E (fx) clipse beépülő modul telepítve, vagy nem.</span><span class="sxs-lookup"><span data-stu-id="d6313-187">hello HDInsight Tools auto-detects whether you installed hello E(fx)clipse plugin or not.</span></span> <span data-ttu-id="d6313-188">Kattintson a **OK** tooinstall hello Eclipse piactéren, és indítsa újra az Eclipse toocontinue és követi hello utasításokat.</span><span class="sxs-lookup"><span data-stu-id="d6313-188">Click **OK** toocontinue and follow hello instructions tooinstall hello Eclipse Marketplace and restart Eclipse.</span></span>

    ![E (fx) clipse telepítése](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-efxclipse.png)

3. <span data-ttu-id="d6313-190">Nyissa meg a feladat megtekintése a hello hello **feladatok** csomópont.</span><span class="sxs-lookup"><span data-stu-id="d6313-190">Open hello Job View from hello **Jobs** node.</span></span> <span data-ttu-id="d6313-191">A jobb oldali hello hello **Spark feladat megtekintése** lap hello fürtön futó összes hello-alkalmazást jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="d6313-191">In hello right pane, hello **Spark Job View** tab displays all hello applications that were run on hello cluster.</span></span> <span data-ttu-id="d6313-192">Kattintson a kívánt toosee további részleteket hello alkalmazás hello neve.</span><span class="sxs-lookup"><span data-stu-id="d6313-192">Click hello name of hello application for which you want toosee more details.</span></span>

    ![Az alkalmazás részletei](./media/hdinsight-apache-spark-intellij-tool-plugin/view-job-logs.png)
4. <span data-ttu-id="d6313-194">Ha a feladat ábra hello mutat, alapszintű futó feladat adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="d6313-194">If you hover on hello job graph, it displays basic running job info.</span></span> <span data-ttu-id="d6313-195">Kattintson a hello feladatgrafikon hello szakaszból graph és adatait, amely minden feladatot hoz létre tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d6313-195">Clicking on hello job graph shows hello stages graph and info that every job generates.</span></span>

    ![Feladat szakasz részletei](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

5. <span data-ttu-id="d6313-197">Gyakran használt naplókat, például illesztőprogram Stderr, illesztőprogram Stdout és Directory adatai láthatók hello **napló** fülre.</span><span class="sxs-lookup"><span data-stu-id="d6313-197">Frequently used logs, including Driver Stderr, Driver Stdout, and Directory Info, are listed in hello **Log** tab.</span></span>

    ![Naplófájl részletei](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-log-info.png)
6. <span data-ttu-id="d6313-199">Hello Spark előzmények felhasználói felület és a YARN felhasználói felületen (hello alkalmazási szinten) hello hello megfelelő hivatkozás hello ablak hello tetején kattintva is megnyithatja.</span><span class="sxs-lookup"><span data-stu-id="d6313-199">You can also open hello Spark history UI and hello YARN UI (at hello application level) by clicking hello respective hyperlink at hello top of hello window.</span></span>

### <a name="access-hello-storage-container-for-hello-cluster"></a><span data-ttu-id="d6313-200">Hozzáférés a hello tároló hello fürt</span><span class="sxs-lookup"><span data-stu-id="d6313-200">Access hello storage container for hello cluster</span></span>
1. <span data-ttu-id="d6313-201">Az Azure Explorerben bontsa ki a hello **HDInsight** legfelső szintű csomópont toosee elérhető HDInsight Spark-fürtök listáját.</span><span class="sxs-lookup"><span data-stu-id="d6313-201">In Azure Explorer, expand hello **HDInsight** root node toosee a list of HDInsight Spark clusters that are available.</span></span>
2. <span data-ttu-id="d6313-202">Bontsa ki a hello fürt neve toosee hello tárfiók és hello alapértelmezett tároló hello fürthöz.</span><span class="sxs-lookup"><span data-stu-id="d6313-202">Expand hello cluster name toosee hello storage account and hello default storage container for hello cluster.</span></span>
   
    ![Storage-fiókot és az alapértelmezett tároló](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-5.png)
3. <span data-ttu-id="d6313-204">Kattintson a hello hello fürthöz rendelt tárolási Tárolónév.</span><span class="sxs-lookup"><span data-stu-id="d6313-204">Click hello storage container name associated with hello cluster.</span></span> <span data-ttu-id="d6313-205">Hello jobb oldali ablaktáblában kattintson duplán a hello **HVACOut** mappa.</span><span class="sxs-lookup"><span data-stu-id="d6313-205">In hello right pane, double-click hello **HVACOut** folder.</span></span> <span data-ttu-id="d6313-206">Nyissa meg az egyik hello **rész -** fájlok hello alkalmazás toosee hello kimenete.</span><span class="sxs-lookup"><span data-stu-id="d6313-206">Open one of hello **part-** files toosee hello output of hello application.</span></span>

### <a name="access-hello-spark-history-server"></a><span data-ttu-id="d6313-207">Hello Spark előzmények kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="d6313-207">Access hello Spark history server</span></span>
1. <span data-ttu-id="d6313-208">Az Azure-kezelővel, kattintson a jobb gombbal a Spark-fürt nevét, majd válassza ki **nyissa meg a Spark feladatelőzmények felhasználói felület**.</span><span class="sxs-lookup"><span data-stu-id="d6313-208">In Azure Explorer, right-click your Spark cluster name, and then select **Open Spark History UI**.</span></span> <span data-ttu-id="d6313-209">Amikor a rendszer kéri, adja meg hello fürt hello rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="d6313-209">When you're prompted, enter hello admin credentials for hello cluster.</span></span> <span data-ttu-id="d6313-210">Ezek hello fürt kiépítése során van megadva.</span><span class="sxs-lookup"><span data-stu-id="d6313-210">You must have specified these while provisioning hello cluster.</span></span>
2. <span data-ttu-id="d6313-211">Hello Spark előzmények kiszolgáló irányítópultjának használ hello alkalmazás neve toolook hello alkalmazás imént futása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="d6313-211">In hello Spark history server dashboard, you use hello application name toolook for hello application that you just finished running.</span></span> <span data-ttu-id="d6313-212">Hello megelőző kódot, akkor be hello alkalmazásnév `val conf = new SparkConf().setAppName("MyClusterApp")`.</span><span class="sxs-lookup"><span data-stu-id="d6313-212">In hello preceding code, you set hello application name by using `val conf = new SparkConf().setAppName("MyClusterApp")`.</span></span> <span data-ttu-id="d6313-213">Emiatt a Spark alkalmazásnév lett **MyClusterApp**.</span><span class="sxs-lookup"><span data-stu-id="d6313-213">Hence, your Spark application name was **MyClusterApp**.</span></span>

### <a name="start-hello-ambari-portal"></a><span data-ttu-id="d6313-214">Indítsa el a hello Ambari portál</span><span class="sxs-lookup"><span data-stu-id="d6313-214">Start hello Ambari portal</span></span>
1. <span data-ttu-id="d6313-215">Az Azure-kezelővel, kattintson a jobb gombbal a Spark-fürt nevét, majd válassza ki **nyitott fürt Management Portal (Ambari)**.</span><span class="sxs-lookup"><span data-stu-id="d6313-215">In Azure Explorer, right-click your Spark cluster name, and then select **Open Cluster Management Portal (Ambari)**.</span></span> 
2. <span data-ttu-id="d6313-216">Amikor a rendszer kéri, adja meg hello fürt hello rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="d6313-216">When you're prompted, enter hello admin credentials for hello cluster.</span></span> <span data-ttu-id="d6313-217">Ezek hello fürt kiépítése során van megadva.</span><span class="sxs-lookup"><span data-stu-id="d6313-217">You must have specified these while provisioning hello cluster.</span></span>

### <a name="manage-azure-subscriptions"></a><span data-ttu-id="d6313-218">Az Azure-előfizetések kezelése</span><span class="sxs-lookup"><span data-stu-id="d6313-218">Manage Azure subscriptions</span></span>
<span data-ttu-id="d6313-219">Alapértelmezés szerint a HDInsight Tools az Eclipse Azure eszköztára hello Spark-fürtök az összes Azure-előfizetések sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="d6313-219">By default, HDInsight Tools in Azure Toolkit for Eclipse lists hello Spark clusters from all your Azure subscriptions.</span></span> <span data-ttu-id="d6313-220">Ha szükséges, megadhatja a kívánt tooaccess hello fürt hello előfizetések.</span><span class="sxs-lookup"><span data-stu-id="d6313-220">If necessary, you can specify hello subscriptions for which you want tooaccess hello cluster.</span></span> 

1. <span data-ttu-id="d6313-221">Az Azure-kezelővel, kattintson a jobb gombbal hello **Azure** gyökércsomópont, és kattintson a **előfizetések kezelése oldalt**.</span><span class="sxs-lookup"><span data-stu-id="d6313-221">In Azure Explorer, right-click hello **Azure** root node, and then click **Manage Subscriptions**.</span></span> 
2. <span data-ttu-id="d6313-222">A hello párbeszédpanelen törölje a hello előfizetés, hogy nem szeretné, hogy tooaccess, és kattintson a hello jelölőnégyzeteit **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="d6313-222">In hello dialog box, clear hello check boxes for hello subscription that you don't want tooaccess, and then click **Close**.</span></span> <span data-ttu-id="d6313-223">Is **Kijelentkezés** toosign kívül az Azure-előfizetés tetszés.</span><span class="sxs-lookup"><span data-stu-id="d6313-223">You can also click **Sign Out** if you want toosign out of your Azure subscription.</span></span>

## <a name="run-a-spark-scala-application-locally"></a><span data-ttu-id="d6313-224">A Spark Scala alkalmazás helyileg történő futtatása</span><span class="sxs-lookup"><span data-stu-id="d6313-224">Run a Spark Scala application locally</span></span>
<span data-ttu-id="d6313-225">Használhatja a HDInsight Tools Azure eszközkészlet Eclipse toorun Spark Scala-alkalmazások helyileg a munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="d6313-225">You can use HDInsight Tools in Azure Toolkit for Eclipse toorun Spark Scala applications locally on your workstation.</span></span> <span data-ttu-id="d6313-226">Általában ezek az alkalmazások hozzáférési toocluster erőforrások, például a tároló nem szükséges, és futtatja, és helyben tesztelheti őket.</span><span class="sxs-lookup"><span data-stu-id="d6313-226">Typically, these applications don't need access toocluster resources such as a storage container, and you can run and test them locally.</span></span>

### <a name="prerequisite"></a><span data-ttu-id="d6313-227">Előfeltétel</span><span class="sxs-lookup"><span data-stu-id="d6313-227">Prerequisite</span></span>
<span data-ttu-id="d6313-228">Hello helyi Spark Scala-alkalmazások Windows rendszerű számítógépeken futtatása, közben kivétel kaphat, ahogy [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356).</span><span class="sxs-lookup"><span data-stu-id="d6313-228">While you're running hello local Spark Scala application on a Windows computer, you might get an exception as explained in [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356).</span></span> <span data-ttu-id="d6313-229">Ez a kivétel akkor fordul elő, mert **WinUtils.exe** Windows hiányzik.</span><span class="sxs-lookup"><span data-stu-id="d6313-229">This exception occurs because **WinUtils.exe** is missing in Windows.</span></span> 

<span data-ttu-id="d6313-230">tooresolve Ez a hiba kell [végrehajtható hello letöltése](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) tooa helyre, például **C:\WinUtils\bin**.</span><span class="sxs-lookup"><span data-stu-id="d6313-230">tooresolve this error, you must [download hello executable](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) tooa location like **C:\WinUtils\bin**.</span></span> <span data-ttu-id="d6313-231">Hozzá kell majd adnia a hello környezeti változó **HADOOP_HOME** és hello hello változó értékének túl**C\WinUtils**.</span><span class="sxs-lookup"><span data-stu-id="d6313-231">You must then add hello environment variable **HADOOP_HOME** and set hello value of hello variable too**C\WinUtils**.</span></span>

### <a name="run-a-local-spark-scala-application"></a><span data-ttu-id="d6313-232">A helyi Spark Scala-alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="d6313-232">Run a local Spark Scala application</span></span>
1. <span data-ttu-id="d6313-233">Indítsa el az Eclipse, és hozzon létre egy projektet.</span><span class="sxs-lookup"><span data-stu-id="d6313-233">Start Eclipse and create a project.</span></span> <span data-ttu-id="d6313-234">A hello **új projekt** párbeszédpanelen hajtsa végre a következő lehetőségek hello, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="d6313-234">In hello **New Project** dialog box, make hello following choices, and then click **Next**.</span></span>
   
   * <span data-ttu-id="d6313-235">Hello bal oldali ablaktáblában jelöljön ki **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="d6313-235">In hello left pane, select **HDInsight**.</span></span>
   * <span data-ttu-id="d6313-236">Hello jobb oldali ablaktáblában jelöljön ki **a Spark on HDInsight helyi futtatása minta (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="d6313-236">In hello right pane, select **Spark on HDInsight Local Run Sample (Scala)**.</span></span>

    ![A New Project (Új projekt) párbeszédpanel](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run.png)
2. <span data-ttu-id="d6313-238">tooprovide hello projekt részleteit, hajtsa végre lépéseket 3-6 a korábbi szakaszban hello [állítson be egy Spark Scala-projektjét, amely egy HDInsight Spark-fürt](#set-up-a-spark-scala-project-for-an-hdinsight-spark cluster).</span><span class="sxs-lookup"><span data-stu-id="d6313-238">tooprovide hello project details, follow steps 3 through 6 from hello earlier section [Set up a Spark Scala project for an HDInsight Spark cluster](#set-up-a-spark-scala-project-for-an-hdinsight-spark cluster).</span></span>
3. <span data-ttu-id="d6313-239">hello sablon hozzáadása egy mintakód (**LogQuery**) alatt hello **src** mappát, amelyet a számítógépen helyileg futtathat.</span><span class="sxs-lookup"><span data-stu-id="d6313-239">hello template adds a sample code (**LogQuery**) under hello **src** folder that you can run locally on your computer.</span></span>
   
    ![LogQuery helye](./media/hdinsight-apache-spark-eclipse-tool-plugin/local-app.png)
4. <span data-ttu-id="d6313-241">Kattintson a jobb gombbal hello **LogQuery** alkalmazás, pont túl**futtató**, és kattintson a **1 Scala alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d6313-241">Right-click hello **LogQuery** application, point too**Run As**, and then click **1 Scala Application**.</span></span> <span data-ttu-id="d6313-242">Egy hello az ehhez hasonló kimenetet fog látni **konzol** hello alsó lapon:</span><span class="sxs-lookup"><span data-stu-id="d6313-242">You will see an output like this in hello **Console** tab at hello bottom:</span></span>
   
   ![Spark alkalmazás helyi futtatás eredménye](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="faq"></a><span data-ttu-id="d6313-244">GYIK</span><span class="sxs-lookup"><span data-stu-id="d6313-244">FAQ</span></span>
<span data-ttu-id="d6313-245">Válasszon egy alkalmazás tooAzure Data Lake Store toosubmit **interaktív** mód hello Azure bejelentkezés során.</span><span class="sxs-lookup"><span data-stu-id="d6313-245">toosubmit an application tooAzure Data Lake Store, choose **Interactive** mode during hello Azure sign-in process.</span></span> <span data-ttu-id="d6313-246">Ha **automatikus** mód, hibaüzenetet kaphat.</span><span class="sxs-lookup"><span data-stu-id="d6313-246">If you select **Automated** mode, you can get an error.</span></span>

![interaktív bejelentkezés](./media/hdinsight-apache-spark-eclipse-tool-plugin/interactive-authentication.png)

<span data-ttu-id="d6313-248">Most azt feloldotta azt.</span><span class="sxs-lookup"><span data-stu-id="d6313-248">Now, we resolved it.</span></span> <span data-ttu-id="d6313-249">Választhat egy Azure Data Lake fürt toosubmit az alkalmazás egyik bejelentkezési módszer.</span><span class="sxs-lookup"><span data-stu-id="d6313-249">You can choose an Azure Data Lake Cluster toosubmit your application with any sign-in method.</span></span>

## <a name="feedback-and-known-issues"></a><span data-ttu-id="d6313-250">Visszajelzések és ismert problémák</span><span class="sxs-lookup"><span data-stu-id="d6313-250">Feedback and known issues</span></span>
<span data-ttu-id="d6313-251">Spark kimenetek közvetlenül megtekintése jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="d6313-251">Currently, viewing Spark outputs directly is not supported.</span></span>

<span data-ttu-id="d6313-252">Ha javaslata vagy visszajelzést, vagy ha az eszköz használata során felmerülő problémákat, érzi, hogy szabad toosend, egy e-mailt hdivstool@microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="d6313-252">If you have any suggestions or feedback, or if you encounter any problems when using this tool, feel free toosend us an email at hdivstool@microsoft.com.</span></span>

## <span data-ttu-id="d6313-253"><a name="seealso"></a>Lásd még:</span><span class="sxs-lookup"><span data-stu-id="d6313-253"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="d6313-254">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="d6313-254">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="d6313-255">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="d6313-255">Scenarios</span></span>
* [<span data-ttu-id="d6313-256">Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="d6313-256">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="d6313-257">Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján</span><span class="sxs-lookup"><span data-stu-id="d6313-257">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="d6313-258">Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények</span><span class="sxs-lookup"><span data-stu-id="d6313-258">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="d6313-259">Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására</span><span class="sxs-lookup"><span data-stu-id="d6313-259">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="d6313-260">A webhelynapló elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="d6313-260">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a><span data-ttu-id="d6313-261">Létrehozása és alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="d6313-261">Creating and running applications</span></span>
* [<span data-ttu-id="d6313-262">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="d6313-262">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="d6313-263">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="d6313-263">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="d6313-264">Eszközök és bővítmények</span><span class="sxs-lookup"><span data-stu-id="d6313-264">Tools and extensions</span></span>
* [<span data-ttu-id="d6313-265">Az IntelliJ toocreate Azure eszközkészlet használja, és küldje el a Spark Scala-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="d6313-265">Use Azure Toolkit for IntelliJ toocreate and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="d6313-266">Az IntelliJ toodebug Spark-alkalmazások VPN-en keresztül távolról használható Azure eszköztára</span><span class="sxs-lookup"><span data-stu-id="d6313-266">Use Azure Toolkit for IntelliJ toodebug Spark applications remotely through VPN</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="d6313-267">Az IntelliJ toodebug Spark-alkalmazások SSH keresztül távolról használható Azure eszköztára</span><span class="sxs-lookup"><span data-stu-id="d6313-267">Use Azure Toolkit for IntelliJ toodebug Spark applications remotely through SSH</span></span>](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [<span data-ttu-id="d6313-268">Használja a HDInsight Tools for IntelliJ a Hortonworks védőfal</span><span class="sxs-lookup"><span data-stu-id="d6313-268">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [<span data-ttu-id="d6313-269">Zeppelin notebookok használata Spark-fürttel HDInsighton</span><span class="sxs-lookup"><span data-stu-id="d6313-269">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="d6313-270">Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében</span><span class="sxs-lookup"><span data-stu-id="d6313-270">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="d6313-271">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="d6313-271">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="d6313-272">Jupyter telepítse a számítógépre, és csatlakozzon a HDInsight Spark-fürt tooan</span><span class="sxs-lookup"><span data-stu-id="d6313-272">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a><span data-ttu-id="d6313-273">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="d6313-273">Managing resources</span></span>
* [<span data-ttu-id="d6313-274">Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése</span><span class="sxs-lookup"><span data-stu-id="d6313-274">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="d6313-275">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="d6313-275">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

