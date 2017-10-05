---
title: "Az Eclipse - alkalmazások HDInsight Spark Scala létrehozása az Azure eszközkészlet |} Microsoft Docs"
description: "Azure eszköztára eclipse-ben a HDInsight Tools használatával scalában írt Spark-alkalmazások fejlesztéséhez és küldheti el ezeket a HDInsight Spark-fürt közvetlenül a számukra az Eclipse IDE."
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
ms.openlocfilehash: 4bcb1987a62c0b7f4965e6fd257315e820004238
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-toolkit-for-eclipse-to-create-spark-applications-for-an-hdinsight-cluster"></a><span data-ttu-id="ec73e-103">Azure eszköztára Eclipse használata Spark-alkalmazások a HDInsight-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="ec73e-103">Use Azure Toolkit for Eclipse to create Spark applications for an HDInsight cluster</span></span>

<span data-ttu-id="ec73e-104">Az Eclipse eszköztára Azure HDInsight Tools használatával scalában írt Spark-alkalmazások fejlesztéséhez és küldheti el ezeket az Azure HDInsight Spark-fürt az Eclipse IDE-ről.</span><span class="sxs-lookup"><span data-stu-id="ec73e-104">Use HDInsight Tools in Azure Toolkit for Eclipse to develop Spark applications written in Scala and submit them to an Azure HDInsight Spark cluster, directly from the Eclipse IDE.</span></span> <span data-ttu-id="ec73e-105">A HDInsight Tools beépülő modul néhány különböző módokon használhatja:</span><span class="sxs-lookup"><span data-stu-id="ec73e-105">You can use the HDInsight Tools plug-in in a few different ways:</span></span>

* <span data-ttu-id="ec73e-106">Fejlesztésére, és küldje el a Scala Spark-alkalmazás egy HDInsight Spark-fürt</span><span class="sxs-lookup"><span data-stu-id="ec73e-106">To develop and submit a Scala Spark application on an HDInsight Spark cluster</span></span>
* <span data-ttu-id="ec73e-107">Az Azure HDInsight Spark-fürt erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="ec73e-107">To access your Azure HDInsight Spark cluster resources</span></span>
* <span data-ttu-id="ec73e-108">Fejlesztésére és Scala Spark-alkalmazás helyileg történő futtatása</span><span class="sxs-lookup"><span data-stu-id="ec73e-108">To develop and run a Scala Spark application locally</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ec73e-109">Ez az eszköz létrehozásához és elküldéséhez az alkalmazások csak a HDInsight Spark-fürt Linux rendszeren használható.</span><span class="sxs-lookup"><span data-stu-id="ec73e-109">This tool can be used to create and submit applications only for an HDInsight Spark cluster on Linux.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="ec73e-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ec73e-110">Prerequisites</span></span>

* <span data-ttu-id="ec73e-111">A HDInsight az Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="ec73e-111">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="ec73e-112">Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="ec73e-112">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>
* <span data-ttu-id="ec73e-113">Oracle Java fejlesztői készlet 8-as verzió, az Eclipse IDE futásidejű használt.</span><span class="sxs-lookup"><span data-stu-id="ec73e-113">Oracle Java Development Kit version 8, which is used for the Eclipse IDE runtime.</span></span> <span data-ttu-id="ec73e-114">Letöltheti azt a [Oracle webhely](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="ec73e-114">You can download it from the [Oracle website](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
* <span data-ttu-id="ec73e-115">Eclipse IDE.</span><span class="sxs-lookup"><span data-stu-id="ec73e-115">Eclipse IDE.</span></span> <span data-ttu-id="ec73e-116">Ebben a cikkben az Eclipse Neonfény.</span><span class="sxs-lookup"><span data-stu-id="ec73e-116">This article uses Eclipse Neon.</span></span> <span data-ttu-id="ec73e-117">Telepítheti azt a [Eclipse webhely](https://www.eclipse.org/downloads/).</span><span class="sxs-lookup"><span data-stu-id="ec73e-117">You can install it from the [Eclipse website](https://www.eclipse.org/downloads/).</span></span>   
* <span data-ttu-id="ec73e-118">Spark SDK.</span><span class="sxs-lookup"><span data-stu-id="ec73e-118">Spark SDK.</span></span> <span data-ttu-id="ec73e-119">Letöltheti a [GitHub](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="ec73e-119">You can download it from [GitHub](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409).</span></span>


## <a name="install-hdinsight-tools-in-azure-toolkit-for-eclipse-and-scala-plugin"></a><span data-ttu-id="ec73e-120">A HDInsight Tools Azure eszközkészlet az eclipse-ben és a Scala beépülő modul telepítése</span><span class="sxs-lookup"><span data-stu-id="ec73e-120">Install HDInsight Tools in Azure Toolkit for Eclipse and Scala Plugin</span></span>
### <a name="install-hdinsight-tools"></a><span data-ttu-id="ec73e-121">HDInsight eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="ec73e-121">Install HDInsight Tools</span></span>
<span data-ttu-id="ec73e-122">A HDInsight Tools for eclipse-ben érhető el az Eclipse Azure eszköztára részeként.</span><span class="sxs-lookup"><span data-stu-id="ec73e-122">HDInsight Tools for Eclipse is available as part of Azure Toolkit for Eclipse.</span></span> <span data-ttu-id="ec73e-123">A telepítési utasításokért lásd: [Azure eszközkészlet telepítése az Eclipse](../azure-toolkit-for-eclipse-installation.md).</span><span class="sxs-lookup"><span data-stu-id="ec73e-123">For installation instructions, see [Installing Azure Toolkit for Eclipse](../azure-toolkit-for-eclipse-installation.md).</span></span>
### <a name="install-scala-plugin"></a><span data-ttu-id="ec73e-124">Scala beépülő modul telepítése</span><span class="sxs-lookup"><span data-stu-id="ec73e-124">Install Scala Plugin</span></span>
<span data-ttu-id="ec73e-125">Az Intellij megnyitásakor a HDInsight Tools automatikusan észleli, hogy Scala beépülő modul telepítve, vagy nem.</span><span class="sxs-lookup"><span data-stu-id="ec73e-125">When you open the Intellij, the HDInsight Tools auto detects whether you installed Scala plugin or not.</span></span> <span data-ttu-id="ec73e-126">Kattintson a **OK** folytatja, és kövesse az utasításokat az Eclipse piactér a telepítést.</span><span class="sxs-lookup"><span data-stu-id="ec73e-126">Click **OK** to continue and follow the instructions to install by the Eclipse Marketplace.</span></span>

 ![Automatikus telepítés Scala beépülő modul](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-scala.png)

## <a name="sign-in-to-your-azure-subscription"></a><span data-ttu-id="ec73e-128">Jelentkezzen be az Azure-előfizetésébe</span><span class="sxs-lookup"><span data-stu-id="ec73e-128">Sign in to your Azure subscription</span></span>
1. <span data-ttu-id="ec73e-129">Indítsa el az Eclipse IDE, és nyissa meg az Azure-kezelővel.</span><span class="sxs-lookup"><span data-stu-id="ec73e-129">Start the Eclipse IDE and open Azure Explorer.</span></span> <span data-ttu-id="ec73e-130">Az a **ablak** menüben kattintson a **nézet megjelenítése**, és kattintson a **más**.</span><span class="sxs-lookup"><span data-stu-id="ec73e-130">On the **Window** menu, click **Show View**, and then click **Other**.</span></span> <span data-ttu-id="ec73e-131">A megnyíló párbeszédpanelen bontsa ki a **Azure**, kattintson a **Azure Explorer**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="ec73e-131">In the dialog box that opens, expand **Azure**, click **Azure Explorer**, and then click **OK**.</span></span>

    ![Nézet párbeszédpanel megjelenítése](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-1.png)
2. <span data-ttu-id="ec73e-133">Kattintson a jobb gombbal a **Azure** csomópontra, majd **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="ec73e-133">Right-click the **Azure** node, and then click **Sign in**.</span></span>
3. <span data-ttu-id="ec73e-134">Az a **Azure bejelentkezés** párbeszédpanelen, a hitelesítési módszer kiválasztása párbeszédpanelen kattintson **jelentkezzen be a**, Azure hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="ec73e-134">In the **Azure Sign In** dialog box, choose the authentication method, click **Sign in**, and enter your Azure credentials.</span></span>
   
    ![Az Azure bejelentkezési párbeszédpanel](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-2.png)
4. <span data-ttu-id="ec73e-136">Miután bejelentkezett a, a **előfizetések kiválasztása** párbeszédpanel megjeleníti az összes Azure-előfizetések társított hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="ec73e-136">After you're signed in, the **Select Subscriptions** dialog box lists all the Azure subscriptions associated with the credentials.</span></span> <span data-ttu-id="ec73e-137">Kattintson a **válasszon** a párbeszédpanel bezárásához.</span><span class="sxs-lookup"><span data-stu-id="ec73e-137">Click **Select** to close the dialog box.</span></span>

    ![Válassza ki az előfizetések párbeszédpanel](./media/hdinsight-apache-spark-eclipse-tool-plugin/Select-Subscriptions.png)
5. <span data-ttu-id="ec73e-139">Az a **Azure Explorer** lapján bontsa ki a **HDInsight** a HDInsight Spark-fürtök az előfizetéshez tartozó megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="ec73e-139">On the **Azure Explorer** tab, expand **HDInsight** to see the HDInsight Spark clusters under your subscription.</span></span>
   
    ![HDInsight Spark-fürtjei az Azure-kezelővel](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-3.png)
6. <span data-ttu-id="ec73e-141">Ennél jobban is kibonthatja az erőforrások (például storage-fiókok) a fürthöz tartozó megjelenítéséhez fürtcsomópont nevét.</span><span class="sxs-lookup"><span data-stu-id="ec73e-141">You can further expand a cluster name node to see the resources (for example, storage accounts) associated with the cluster.</span></span>
   
    ![A fürt nevét cikkekben találhat kibontása](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-4.png)



## <a name="set-up-a-spark-scala-project-for-an-hdinsight-spark-cluster"></a><span data-ttu-id="ec73e-143">A Spark Scala-projektjét, amely egy HDInsight Spark-fürt beállítása</span><span class="sxs-lookup"><span data-stu-id="ec73e-143">Set up a Spark Scala project for an HDInsight Spark cluster</span></span>

1. <span data-ttu-id="ec73e-144">Az Eclipse IDE munkaterületen kattintson **fájl**, kattintson a **új**, és kattintson a **projekt**.</span><span class="sxs-lookup"><span data-stu-id="ec73e-144">In the Eclipse IDE workspace, click **File**, click **New**, and then click **Project**.</span></span> 
2. <span data-ttu-id="ec73e-145">Bontsa ki az új projekt varázsló **HDInsight**, jelölje be **a Spark on HDInsight (Scala)**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="ec73e-145">In the New Project wizard, expand **HDInsight**, select **Spark on HDInsight (Scala)**, and then click **Next**.</span></span>

    ![A Spark on HDInsight (Scala) projekt kiválasztása](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-2.png)
3. <span data-ttu-id="ec73e-147">A Scala projekt létrehozása varázsló automatikusan észleli, hogy Scala beépülő modul telepítve, vagy nem.</span><span class="sxs-lookup"><span data-stu-id="ec73e-147">The Scala project creation wizard auto detects whether you installed Scala plugin or not.</span></span> <span data-ttu-id="ec73e-148">Kattintson a **OK** folytatja, a Scala beépülő modul letöltése, és kövesse az utasításokat az Eclipse újraindítását.</span><span class="sxs-lookup"><span data-stu-id="ec73e-148">Click **OK** to continue downloading the Scala plugin, then follow the instructions to restart Eclipse.</span></span>

    ![scala ellenőrzése](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-scala-2.png)
4. <span data-ttu-id="ec73e-150">Az a **új HDInsight Scala projekt** párbeszédpanelen adja meg a következő értékeket, és kattintson **következő**:</span><span class="sxs-lookup"><span data-stu-id="ec73e-150">In the **New HDInsight Scala Project** dialog box, provide the following values, and then click **Next**:</span></span>
   * <span data-ttu-id="ec73e-151">Adja meg a projekt nevét.</span><span class="sxs-lookup"><span data-stu-id="ec73e-151">Enter a name for the project.</span></span>
   * <span data-ttu-id="ec73e-152">A a **JRE** területen győződjön meg arról, hogy **használja a végrehajtási környezet JRE** értéke **JavaSE-1.7** vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="ec73e-152">In the **JRE** area, make sure that **Use an execution environment JRE** is set to **JavaSE-1.7** or later.</span></span>
   * <span data-ttu-id="ec73e-153">Győződjön meg arról, hogy a helyet, ahol az SDK-val letöltött Spark SDK van beállítva.</span><span class="sxs-lookup"><span data-stu-id="ec73e-153">Make sure that Spark SDK is set to the location where you downloaded the SDK.</span></span> <span data-ttu-id="ec73e-154">A letöltési hely hivatkozás jelenik meg a [Előfeltételek](#prerequisites) korábbi ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="ec73e-154">The link to the download location is included in the [prerequisites](#prerequisites) earlier in this article.</span></span> <span data-ttu-id="ec73e-155">Az SDK-t is letölthetők a hivatkozás szerepel a párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="ec73e-155">You can also download the SDK from the link included in the dialog box.</span></span>

    ![Új HDInsight Scala projekt párbeszédpanel](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-3.png)
5.  <span data-ttu-id="ec73e-157">A következő párbeszédpanelen, kattintson a **szalagtárak** lapon, és hagyja az alapértelmezett beállításokat, majd kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="ec73e-157">In the next dialog box, click the **Libraries** tab and keep the defaults, and then click **Finish**.</span></span> 
   
    ![Szalagtárak lap](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-4.png)
  
## <a name="create-a-scala-application-for-an-hdinsight-spark-cluster"></a><span data-ttu-id="ec73e-159">A HDInsight Spark-fürtök Scala-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ec73e-159">Create a Scala application for an HDInsight Spark cluster</span></span>

1. <span data-ttu-id="ec73e-160">Az Eclipse IDE a csomag Explorer eszközből bontsa ki a korábban létrehozott projekt, kattintson a jobb gombbal **src**, mutasson a **új**, és kattintson a **más**.</span><span class="sxs-lookup"><span data-stu-id="ec73e-160">In the Eclipse IDE, from Package Explorer, expand the project that you created earlier, right-click **src**, point to **New**, and then click **Other**.</span></span>
2. <span data-ttu-id="ec73e-161">Az a **válassza ki a varázsló** párbeszédpanelen bontsa ki **Scala varázslók**, kattintson a **Scala objektum**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="ec73e-161">In the **Select a wizard** dialog box, expand **Scala Wizards**, click **Scala Object**, and then click **Next**.</span></span>
   
    ![Válassza ki a varázsló párbeszédpanelje](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-1.png)
3. <span data-ttu-id="ec73e-163">Az a **hozzon létre új fájl** párbeszédpanelen adja meg az objektum nevét, és kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="ec73e-163">In the **Create New File** dialog box, enter a name for the object, and then click **Finish**.</span></span>
   
    ![Hozzon létre új fájl párbeszédpanel](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-2.png)
4. <span data-ttu-id="ec73e-165">Illessze be a következő kódot a szövegszerkesztőben:</span><span class="sxs-lookup"><span data-stu-id="ec73e-165">Paste the following code in the text editor:</span></span>
   
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
   
        object MyClusterApp{
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("MyClusterApp")
            val sc = new SparkContext(conf)
   
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
            //find the rows that have only one digit in the seventh column in the CSV
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
   
            rdd1.saveAsTextFile("wasb:///HVACOut")
          }        
        }
5. <span data-ttu-id="ec73e-166">Futtassa az alkalmazást egy HDInsight Spark-fürt:</span><span class="sxs-lookup"><span data-stu-id="ec73e-166">Run the application on an HDInsight Spark cluster:</span></span>
   
   1. <span data-ttu-id="ec73e-167">A csomag Explorer, kattintson a jobb gombbal a projekt nevére, majd válassza ki **küldje el a Spark-alkalmazást, amely**.</span><span class="sxs-lookup"><span data-stu-id="ec73e-167">From Package Explorer, right-click the project name, and then select **Submit Spark Application to HDInsight**.</span></span>        
   2. <span data-ttu-id="ec73e-168">Az a **Spark küldésének** párbeszédpanelen adja meg a következő értékeket, és kattintson **Submit**:</span><span class="sxs-lookup"><span data-stu-id="ec73e-168">In the **Spark Submission** dialog box, provide the following values, and then click **Submit**:</span></span>
      
      * <span data-ttu-id="ec73e-169">A **fürtnév**, válassza ki a HDInsight Spark-fürt, amelyen szeretné futtatni az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ec73e-169">For **Cluster Name**, select the HDInsight Spark cluster on which you want to run your application.</span></span>
      * <span data-ttu-id="ec73e-170">Válasszon egy összetevő az Eclipse-projekt, vagy válasszon ki egy, a merevlemez-meghajtóról.</span><span class="sxs-lookup"><span data-stu-id="ec73e-170">Select an artifact from the Eclipse project, or select one from a hard drive.</span></span> <span data-ttu-id="ec73e-171">Az alapértelmezett érték a cikk a jobb gombbal a csomag explorer függ.</span><span class="sxs-lookup"><span data-stu-id="ec73e-171">The default value depends on the item you right-click from package explorer.</span></span>
      * <span data-ttu-id="ec73e-172">Az a **fő osztálynév** dropdownlist, küldésének varázsló megjeleníti a kijelölt projektből az összes objektum nevét.</span><span class="sxs-lookup"><span data-stu-id="ec73e-172">In the **Main class name** dropdownlist, submission wizard displays all object names from your selected project.</span></span> <span data-ttu-id="ec73e-173">Válassza ki vagy adjon meg egy, a futtatni kívánt.</span><span class="sxs-lookup"><span data-stu-id="ec73e-173">Select or input one that you want to run.</span></span> <span data-ttu-id="ec73e-174">Összetevő merevlemezről válassza ki, ha saját maga által megadott kell fő osztály nevét.</span><span class="sxs-lookup"><span data-stu-id="ec73e-174">If you select artifact from hard disk, you need input main class name by yourself.</span></span> 
      * <span data-ttu-id="ec73e-175">Mivel ebben a példában az alkalmazás kódja nem igényel parancssori argumentumokat vagy hivatkozás JAR-fájlok kivételével vagy fájlokat, üresen többi mezőbe.</span><span class="sxs-lookup"><span data-stu-id="ec73e-175">Because the application code in this example does not require any command-line arguments or reference JARs or files, you can leave the remaining text boxes empty.</span></span>
        
       ![Spark küldésének párbeszédpanel](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-3.png)
   3. <span data-ttu-id="ec73e-177">A **Spark küldésének** lapon kell kezdenie a folyamatban lévő megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="ec73e-177">The **Spark Submission** tab should start displaying the progress.</span></span> <span data-ttu-id="ec73e-178">A piros gombra kattintva leállíthatja az alkalmazás a **Spark küldésének** ablak.</span><span class="sxs-lookup"><span data-stu-id="ec73e-178">You can stop the application by clicking the red button in the **Spark Submission** window.</span></span> <span data-ttu-id="ec73e-179">A naplók az adott alkalmazáshoz, futtassa a földgömb ikon (az ábrán kék mező jelölik) kattintva is megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="ec73e-179">You can also view the logs for this specific application run by clicking the globe icon (denoted by the blue box in the image).</span></span>
      
       ![Spark küldésének ablak](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-4.png)

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-hdinsight-tools-in-azure-toolkit-for-eclipse"></a><span data-ttu-id="ec73e-181">Férhessen hozzá és felügyelhesse a HDInsight Spark-fürtjei a HDInsight Tools használatával Azure eszközkészlet az eclipse-ben</span><span class="sxs-lookup"><span data-stu-id="ec73e-181">Access and manage HDInsight Spark clusters by using HDInsight Tools in Azure Toolkit for Eclipse</span></span>
<span data-ttu-id="ec73e-182">A feladat kimenetére történő hozzáféréshez, a HDInsight Tools használatával különféle műveleteket hajthat végre.</span><span class="sxs-lookup"><span data-stu-id="ec73e-182">You can perform various operations by using HDInsight Tools, including accessing the job output.</span></span>

### <a name="access-the-job-view"></a><span data-ttu-id="ec73e-183">Hozzáférés a feladat megtekintése</span><span class="sxs-lookup"><span data-stu-id="ec73e-183">Access the job view</span></span>
1. <span data-ttu-id="ec73e-184">Az Azure Explorerben bontsa ki a **HDInsight**, bontsa ki a Spark-fürt nevére, majd **feladatok**.</span><span class="sxs-lookup"><span data-stu-id="ec73e-184">In Azure Explorer, expand **HDInsight**, expand the Spark cluster name, and then click **Jobs**.</span></span> 

    ![Feladatok nézet csomópont](./media/hdinsight-apache-spark-intellij-tool-plugin/job-view-node.png)

2. <span data-ttu-id="ec73e-186">Kattintson a **feladatok** csomópont.</span><span class="sxs-lookup"><span data-stu-id="ec73e-186">Click on the **Jobs** node.</span></span> <span data-ttu-id="ec73e-187">A HDInsight Tools automatikus-észleli, hogy az E (fx) clipse beépülő modul telepítve, vagy nem.</span><span class="sxs-lookup"><span data-stu-id="ec73e-187">The HDInsight Tools auto-detects whether you installed the E(fx)clipse plugin or not.</span></span> <span data-ttu-id="ec73e-188">Kattintson a **OK** folytatja, és kövesse az utasításokat az Eclipse piactér telepítése, majd indítsa újra az eclipse-ben.</span><span class="sxs-lookup"><span data-stu-id="ec73e-188">Click **OK** to continue and follow the instructions to install the Eclipse Marketplace and restart Eclipse.</span></span>

    ![E (fx) clipse telepítése](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-efxclipse.png)

3. <span data-ttu-id="ec73e-190">Nyissa meg a feladat nézetet a **feladatok** csomópont.</span><span class="sxs-lookup"><span data-stu-id="ec73e-190">Open the Job View from the **Jobs** node.</span></span> <span data-ttu-id="ec73e-191">A jobb oldali ablaktáblában a **Spark feladat megtekintése** lap megjeleníti a fürtön futó összes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ec73e-191">In the right pane, the **Spark Job View** tab displays all the applications that were run on the cluster.</span></span> <span data-ttu-id="ec73e-192">Kattintson a nevére, amelynek meg szeretné tekinteni a további részleteket az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ec73e-192">Click the name of the application for which you want to see more details.</span></span>

    ![Az alkalmazás részletei](./media/hdinsight-apache-spark-intellij-tool-plugin/view-job-logs.png)
4. <span data-ttu-id="ec73e-194">Ha a feladat grafikonon mutat, alapszintű futó feladat adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="ec73e-194">If you hover on the job graph, it displays basic running job info.</span></span> <span data-ttu-id="ec73e-195">A feladatgrafikon kattintva látható szakaszból graph és adatait, amely minden feladatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ec73e-195">Clicking on the job graph shows the stages graph and info that every job generates.</span></span>

    ![Feladat szakasz részletei](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

5. <span data-ttu-id="ec73e-197">A gyakran használt naplókat, illesztőprogram Stderr, illesztőprogram Stdout és Directory adatai jelennek meg a **napló** fülre.</span><span class="sxs-lookup"><span data-stu-id="ec73e-197">Frequently used logs, including Driver Stderr, Driver Stdout, and Directory Info, are listed in the **Log** tab.</span></span>

    ![Naplófájl részletei](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-log-info.png)
6. <span data-ttu-id="ec73e-199">A Spark-előzmények felhasználói felület és a YARN felhasználói felületen (az alkalmazás szintjén) az ablak tetején a megfelelő hivatkozásra kattintva is megnyithatja.</span><span class="sxs-lookup"><span data-stu-id="ec73e-199">You can also open the Spark history UI and the YARN UI (at the application level) by clicking the respective hyperlink at the top of the window.</span></span>

### <a name="access-the-storage-container-for-the-cluster"></a><span data-ttu-id="ec73e-200">A tároló hozzáférés a fürthöz</span><span class="sxs-lookup"><span data-stu-id="ec73e-200">Access the storage container for the cluster</span></span>
1. <span data-ttu-id="ec73e-201">Azure-kezelővel, bontsa ki a **HDInsight** gyökércsomópont elérhető HDInsight Spark-fürtjei listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="ec73e-201">In Azure Explorer, expand the **HDInsight** root node to see a list of HDInsight Spark clusters that are available.</span></span>
2. <span data-ttu-id="ec73e-202">Bontsa ki a fürt nevét, a tárfiók, és az alapértelmezett tároló, a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="ec73e-202">Expand the cluster name to see the storage account and the default storage container for the cluster.</span></span>
   
    ![Storage-fiókot és az alapértelmezett tároló](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-5.png)
3. <span data-ttu-id="ec73e-204">Kattintson a fürthöz rendelt tárolási tároló nevére.</span><span class="sxs-lookup"><span data-stu-id="ec73e-204">Click the storage container name associated with the cluster.</span></span> <span data-ttu-id="ec73e-205">A jobb oldali ablaktáblában kattintson duplán a **HVACOut** mappa.</span><span class="sxs-lookup"><span data-stu-id="ec73e-205">In the right pane, double-click the **HVACOut** folder.</span></span> <span data-ttu-id="ec73e-206">Nyissa meg az egyik a **rész -** fájlok, hogy az alkalmazás kimenete.</span><span class="sxs-lookup"><span data-stu-id="ec73e-206">Open one of the **part-** files to see the output of the application.</span></span>

### <a name="access-the-spark-history-server"></a><span data-ttu-id="ec73e-207">A Spark-előzmények kiszolgáló elérhető</span><span class="sxs-lookup"><span data-stu-id="ec73e-207">Access the Spark history server</span></span>
1. <span data-ttu-id="ec73e-208">Az Azure-kezelővel, kattintson a jobb gombbal a Spark-fürt nevét, majd válassza ki **nyissa meg a Spark feladatelőzmények felhasználói felület**.</span><span class="sxs-lookup"><span data-stu-id="ec73e-208">In Azure Explorer, right-click your Spark cluster name, and then select **Open Spark History UI**.</span></span> <span data-ttu-id="ec73e-209">Amikor a rendszer kéri, adja meg a fürt rendszergazdai hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="ec73e-209">When you're prompted, enter the admin credentials for the cluster.</span></span> <span data-ttu-id="ec73e-210">Ezek a fürt kiépítése során van megadva.</span><span class="sxs-lookup"><span data-stu-id="ec73e-210">You must have specified these while provisioning the cluster.</span></span>
2. <span data-ttu-id="ec73e-211">A Spark előzmények server Irányítópult segítségével az alkalmazás neve keresse meg az alkalmazás csak futása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="ec73e-211">In the Spark history server dashboard, you use the application name to look for the application that you just finished running.</span></span> <span data-ttu-id="ec73e-212">Az előző kódban használatával beállíthatja az alkalmazás neve `val conf = new SparkConf().setAppName("MyClusterApp")`.</span><span class="sxs-lookup"><span data-stu-id="ec73e-212">In the preceding code, you set the application name by using `val conf = new SparkConf().setAppName("MyClusterApp")`.</span></span> <span data-ttu-id="ec73e-213">Emiatt a Spark alkalmazásnév lett **MyClusterApp**.</span><span class="sxs-lookup"><span data-stu-id="ec73e-213">Hence, your Spark application name was **MyClusterApp**.</span></span>

### <a name="start-the-ambari-portal"></a><span data-ttu-id="ec73e-214">Indítsa el az Ambari portálon</span><span class="sxs-lookup"><span data-stu-id="ec73e-214">Start the Ambari portal</span></span>
1. <span data-ttu-id="ec73e-215">Az Azure-kezelővel, kattintson a jobb gombbal a Spark-fürt nevét, majd válassza ki **nyitott fürt Management Portal (Ambari)**.</span><span class="sxs-lookup"><span data-stu-id="ec73e-215">In Azure Explorer, right-click your Spark cluster name, and then select **Open Cluster Management Portal (Ambari)**.</span></span> 
2. <span data-ttu-id="ec73e-216">Amikor a rendszer kéri, adja meg a fürt rendszergazdai hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="ec73e-216">When you're prompted, enter the admin credentials for the cluster.</span></span> <span data-ttu-id="ec73e-217">Ezek a fürt kiépítése során van megadva.</span><span class="sxs-lookup"><span data-stu-id="ec73e-217">You must have specified these while provisioning the cluster.</span></span>

### <a name="manage-azure-subscriptions"></a><span data-ttu-id="ec73e-218">Az Azure-előfizetések kezelése</span><span class="sxs-lookup"><span data-stu-id="ec73e-218">Manage Azure subscriptions</span></span>
<span data-ttu-id="ec73e-219">Alapértelmezés szerint a HDInsight Tools az Eclipse Azure eszköztára a Spark-fürtök az összes Azure-előfizetések sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="ec73e-219">By default, HDInsight Tools in Azure Toolkit for Eclipse lists the Spark clusters from all your Azure subscriptions.</span></span> <span data-ttu-id="ec73e-220">Ha szükséges, megadhatja az előfizetéseket, amelyekhez a fürt eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="ec73e-220">If necessary, you can specify the subscriptions for which you want to access the cluster.</span></span> 

1. <span data-ttu-id="ec73e-221">Az Azure-kezelővel, kattintson a jobb gombbal a **Azure** gyökércsomópont, és kattintson a **előfizetések kezelése oldalt**.</span><span class="sxs-lookup"><span data-stu-id="ec73e-221">In Azure Explorer, right-click the **Azure** root node, and then click **Manage Subscriptions**.</span></span> 
2. <span data-ttu-id="ec73e-222">A párbeszédpanelen jelölőnégyzetek az előfizetés, amelyet szeretne elérni, és kattintson a **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="ec73e-222">In the dialog box, clear the check boxes for the subscription that you don't want to access, and then click **Close**.</span></span> <span data-ttu-id="ec73e-223">Is **Kijelentkezés** Ha azt szeretné, hogy kijelentkezik, az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="ec73e-223">You can also click **Sign Out** if you want to sign out of your Azure subscription.</span></span>

## <a name="run-a-spark-scala-application-locally"></a><span data-ttu-id="ec73e-224">A Spark Scala alkalmazás helyileg történő futtatása</span><span class="sxs-lookup"><span data-stu-id="ec73e-224">Run a Spark Scala application locally</span></span>
<span data-ttu-id="ec73e-225">Azure eszköztára eclipse-ben a HDInsight Tools használatával Spark Scala-alkalmazások helyi futtatása a munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="ec73e-225">You can use HDInsight Tools in Azure Toolkit for Eclipse to run Spark Scala applications locally on your workstation.</span></span> <span data-ttu-id="ec73e-226">Általában ezek az alkalmazások nem szükséges hozzáférési jogot a fürterőforrások például a tárolót, és futtatja, és helyben tesztelheti őket.</span><span class="sxs-lookup"><span data-stu-id="ec73e-226">Typically, these applications don't need access to cluster resources such as a storage container, and you can run and test them locally.</span></span>

### <a name="prerequisite"></a><span data-ttu-id="ec73e-227">Előfeltétel</span><span class="sxs-lookup"><span data-stu-id="ec73e-227">Prerequisite</span></span>
<span data-ttu-id="ec73e-228">A helyi Spark Scala-alkalmazások Windows rendszerű számítógépeken futtatása, közben kivétel kaphat, ahogy [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356).</span><span class="sxs-lookup"><span data-stu-id="ec73e-228">While you're running the local Spark Scala application on a Windows computer, you might get an exception as explained in [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356).</span></span> <span data-ttu-id="ec73e-229">Ez a kivétel akkor fordul elő, mert **WinUtils.exe** Windows hiányzik.</span><span class="sxs-lookup"><span data-stu-id="ec73e-229">This exception occurs because **WinUtils.exe** is missing in Windows.</span></span> 

<span data-ttu-id="ec73e-230">Ez a hiba elhárításához kell [töltse le a végrehajtható fájl](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) egy olyan helyre, például a **C:\WinUtils\bin**.</span><span class="sxs-lookup"><span data-stu-id="ec73e-230">To resolve this error, you must [download the executable](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) to a location like **C:\WinUtils\bin**.</span></span> <span data-ttu-id="ec73e-231">Majd adja hozzá a következő környezeti változó **HADOOP_HOME** és a változó értékét állíthatja be **C\WinUtils**.</span><span class="sxs-lookup"><span data-stu-id="ec73e-231">You must then add the environment variable **HADOOP_HOME** and set the value of the variable to **C\WinUtils**.</span></span>

### <a name="run-a-local-spark-scala-application"></a><span data-ttu-id="ec73e-232">A helyi Spark Scala-alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="ec73e-232">Run a local Spark Scala application</span></span>
1. <span data-ttu-id="ec73e-233">Indítsa el az Eclipse, és hozzon létre egy projektet.</span><span class="sxs-lookup"><span data-stu-id="ec73e-233">Start Eclipse and create a project.</span></span> <span data-ttu-id="ec73e-234">Az a **új projekt** párbeszédpanelen hajtsa végre a következők közül választhat, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="ec73e-234">In the **New Project** dialog box, make the following choices, and then click **Next**.</span></span>
   
   * <span data-ttu-id="ec73e-235">A bal oldali panelen válassza ki a **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="ec73e-235">In the left pane, select **HDInsight**.</span></span>
   * <span data-ttu-id="ec73e-236">A jobb oldali ablaktáblában jelölje ki a **a Spark on HDInsight helyi futtatása minta (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="ec73e-236">In the right pane, select **Spark on HDInsight Local Run Sample (Scala)**.</span></span>

    ![A New Project (Új projekt) párbeszédpanel](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run.png)
2. <span data-ttu-id="ec73e-238">Arra, hogy a projekt részleteit, hajtsa végre a 3-6. lépéseket a korábbi szakaszában [állítson be egy Spark Scala-projektjét, amely egy HDInsight Spark-fürt](#set-up-a-spark-scala-project-for-an-hdinsight-spark cluster).</span><span class="sxs-lookup"><span data-stu-id="ec73e-238">To provide the project details, follow steps 3 through 6 from the earlier section [Set up a Spark Scala project for an HDInsight Spark cluster](#set-up-a-spark-scala-project-for-an-hdinsight-spark cluster).</span></span>
3. <span data-ttu-id="ec73e-239">A sablon hozzáadása egy mintakód (**LogQuery**) alatt a **src** mappát, amelyet a számítógépen helyileg futtathat.</span><span class="sxs-lookup"><span data-stu-id="ec73e-239">The template adds a sample code (**LogQuery**) under the **src** folder that you can run locally on your computer.</span></span>
   
    ![LogQuery helye](./media/hdinsight-apache-spark-eclipse-tool-plugin/local-app.png)
4. <span data-ttu-id="ec73e-241">Kattintson a jobb gombbal a **LogQuery** alkalmazás, pont **futtató**, és kattintson a **1 Scala alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ec73e-241">Right-click the **LogQuery** application, point to **Run As**, and then click **1 Scala Application**.</span></span> <span data-ttu-id="ec73e-242">Az ehhez a hasonló kimenetet fog látni a **konzol** fül:</span><span class="sxs-lookup"><span data-stu-id="ec73e-242">You will see an output like this in the **Console** tab at the bottom:</span></span>
   
   ![Spark alkalmazás helyi futtatás eredménye](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="faq"></a><span data-ttu-id="ec73e-244">GYIK</span><span class="sxs-lookup"><span data-stu-id="ec73e-244">FAQ</span></span>
<span data-ttu-id="ec73e-245">Azure Data Lake Store kérelmet elküldéséhez válassza **interaktív** mód az Azure bejelentkezési folyamat során.</span><span class="sxs-lookup"><span data-stu-id="ec73e-245">To submit an application to Azure Data Lake Store, choose **Interactive** mode during the Azure sign-in process.</span></span> <span data-ttu-id="ec73e-246">Ha **automatikus** mód, hibaüzenetet kaphat.</span><span class="sxs-lookup"><span data-stu-id="ec73e-246">If you select **Automated** mode, you can get an error.</span></span>

![interaktív bejelentkezés](./media/hdinsight-apache-spark-eclipse-tool-plugin/interactive-authentication.png)

<span data-ttu-id="ec73e-248">Most azt feloldotta azt.</span><span class="sxs-lookup"><span data-stu-id="ec73e-248">Now, we resolved it.</span></span> <span data-ttu-id="ec73e-249">Az Azure Data Lake fürt elküldeni az alkalmazás egyik bejelentkezési módszer kiválasztása</span><span class="sxs-lookup"><span data-stu-id="ec73e-249">You can choose an Azure Data Lake Cluster to submit your application with any sign-in method.</span></span>

## <a name="feedback-and-known-issues"></a><span data-ttu-id="ec73e-250">Visszajelzések és ismert problémák</span><span class="sxs-lookup"><span data-stu-id="ec73e-250">Feedback and known issues</span></span>
<span data-ttu-id="ec73e-251">Spark kimenetek közvetlenül megtekintése jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="ec73e-251">Currently, viewing Spark outputs directly is not supported.</span></span>

<span data-ttu-id="ec73e-252">Ha javaslata vagy visszajelzést, vagy ha az eszköz használata során felmerülő problémákat, nyugodtan küldjön egy e-mailt hdivstool@microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="ec73e-252">If you have any suggestions or feedback, or if you encounter any problems when using this tool, feel free to send us an email at hdivstool@microsoft.com.</span></span>

## <span data-ttu-id="ec73e-253"><a name="seealso"></a>Lásd még:</span><span class="sxs-lookup"><span data-stu-id="ec73e-253"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="ec73e-254">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="ec73e-254">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="ec73e-255">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="ec73e-255">Scenarios</span></span>
* [<span data-ttu-id="ec73e-256">Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="ec73e-256">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="ec73e-257">Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján</span><span class="sxs-lookup"><span data-stu-id="ec73e-257">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="ec73e-258">Spark és Machine Learning: A Spark on HDInsight használata az élelmiszervizsgálati eredmények előrejelzésére</span><span class="sxs-lookup"><span data-stu-id="ec73e-258">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="ec73e-259">Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására</span><span class="sxs-lookup"><span data-stu-id="ec73e-259">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="ec73e-260">A webhelynapló elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="ec73e-260">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a><span data-ttu-id="ec73e-261">Létrehozása és alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="ec73e-261">Creating and running applications</span></span>
* [<span data-ttu-id="ec73e-262">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="ec73e-262">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="ec73e-263">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="ec73e-263">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="ec73e-264">Eszközök és bővítmények</span><span class="sxs-lookup"><span data-stu-id="ec73e-264">Tools and extensions</span></span>
* [<span data-ttu-id="ec73e-265">Az intellij-t Azure eszközkészlet segítségével hozza létre, és küldje el a Spark Scala-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="ec73e-265">Use Azure Toolkit for IntelliJ to create and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="ec73e-266">Az intellij-t Azure eszközkészlet segítségével VPN-en keresztül távolról Spark-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="ec73e-266">Use Azure Toolkit for IntelliJ to debug Spark applications remotely through VPN</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="ec73e-267">Az intellij-t Azure eszközkészlet segítségével SSH keresztül távolról Spark-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="ec73e-267">Use Azure Toolkit for IntelliJ to debug Spark applications remotely through SSH</span></span>](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [<span data-ttu-id="ec73e-268">Használja a HDInsight Tools for IntelliJ a Hortonworks védőfal</span><span class="sxs-lookup"><span data-stu-id="ec73e-268">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [<span data-ttu-id="ec73e-269">Zeppelin notebookok használata Spark-fürttel HDInsighton</span><span class="sxs-lookup"><span data-stu-id="ec73e-269">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="ec73e-270">Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében</span><span class="sxs-lookup"><span data-stu-id="ec73e-270">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="ec73e-271">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="ec73e-271">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="ec73e-272">A Jupyter telepítése a számítógépre, majd csatlakozás egy HDInsight Spark-fürthöz</span><span class="sxs-lookup"><span data-stu-id="ec73e-272">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a><span data-ttu-id="ec73e-273">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="ec73e-273">Managing resources</span></span>
* [<span data-ttu-id="ec73e-274">Apache Spark-fürt erőforrásainak kezelése az Azure HDInsightban</span><span class="sxs-lookup"><span data-stu-id="ec73e-274">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="ec73e-275">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="ec73e-275">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

