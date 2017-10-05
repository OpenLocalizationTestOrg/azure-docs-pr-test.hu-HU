---
title: "Az IntelliJ Azure eszköztára: Debug Spark alkalmazások távolról SSH |} Microsoft Docs"
description: "Részletes útmutatás Azure eszköztára IntelliJ HDInsight eszközök segítségével távolról a HDInsight-alkalmazások hibakeresését fürtök SSH-n keresztül"
keywords: "Debug távolról intellij, távoli hibakeresés intellij, ssh, az intellij hdinsight, a debug intellij, hibakeresés"
services: hdinsight
documentationcenter: 
author: jejiang
manager: DJ
editor: Jenny Jiang
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 08/24/2017
ms.author: Jenny Jiang
ms.openlocfilehash: 19053e31d6eb097bc91a04ef9c6af5772aaa16da
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="debug-spark-applications-on-an-hdinsight-cluster-with-azure-toolkit-for-intellij-through-ssh"></a><span data-ttu-id="07915-104">Az SSH-n keresztül az IntelliJ HDInsight-fürtök az Azure-eszközkészlet a Spark-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="07915-104">Debug Spark applications on an HDInsight cluster with Azure Toolkit for IntelliJ through SSH</span></span>

<span data-ttu-id="07915-105">Ez a cikk nyújt részletes útmutatást Azure eszköztára IntelliJ HDInsight eszközök segítségével távolról a HDInsight-fürtök-alkalmazások hibakeresését.</span><span class="sxs-lookup"><span data-stu-id="07915-105">This article provides step-by-step guidance on how to use HDInsight Tools in Azure Toolkit for IntelliJ to debug applications remotely on an HDInsight cluster.</span></span> <span data-ttu-id="07915-106">A projekt hibakeresési, megtekintheti a [az IntelliJ Debug HDInsight Spark Azure eszközkészlet alkalmazások](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) videó.</span><span class="sxs-lookup"><span data-stu-id="07915-106">To debug your project, you can also view the [Debug HDInsight Spark applications with Azure Toolkit for IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) video.</span></span>

<span data-ttu-id="07915-107">**Előfeltételek**</span><span class="sxs-lookup"><span data-stu-id="07915-107">**Prerequisites**</span></span>

* <span data-ttu-id="07915-108">**A HDInsight Tools az intellij-t Azure eszköztára**.</span><span class="sxs-lookup"><span data-stu-id="07915-108">**HDInsight Tools in Azure Toolkit for IntelliJ**.</span></span> <span data-ttu-id="07915-109">Ez az eszköz az intellij-t Azure Toolkit részét képezi.</span><span class="sxs-lookup"><span data-stu-id="07915-109">This tool is part of Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="07915-110">További információkért lásd: [Azure eszközkészlet telepítése az IntelliJ](https://docs.microsoft.com/en-us/azure/azure-toolkit-for-intellij-installation).</span><span class="sxs-lookup"><span data-stu-id="07915-110">For more information, see [Install Azure Toolkit for IntelliJ](https://docs.microsoft.com/en-us/azure/azure-toolkit-for-intellij-installation).</span></span>
* <span data-ttu-id="07915-111">**Az IntelliJ Azure eszköztára**.</span><span class="sxs-lookup"><span data-stu-id="07915-111">**Azure Toolkit for IntelliJ**.</span></span> <span data-ttu-id="07915-112">Ez az eszközkészlet használata Spark-alkalmazások a HDInsight-fürtök létrehozása.</span><span class="sxs-lookup"><span data-stu-id="07915-112">Use this toolkit to create Spark applications for an HDInsight cluster.</span></span> <span data-ttu-id="07915-113">További információkért kövesse az utasításokat a [használata Azure eszköztára Spark-alkalmazások a HDInsight-fürtök létrehozása az IntelliJ](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin).</span><span class="sxs-lookup"><span data-stu-id="07915-113">For more information, follow the instructions in [Use Azure Toolkit for IntelliJ to create Spark applications for an HDInsight cluster](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin).</span></span>
* <span data-ttu-id="07915-114">**HDInsight SSH szolgáltatást a felhasználónév és jelszó felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="07915-114">**HDInsight SSH service with username and password management**.</span></span> <span data-ttu-id="07915-115">További információkért lásd: [HDInsight (Hadoop) SSH használatával csatlakozhat](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) és [az SSH tunneling elérni az Ambari webes felhasználói felület, JobHistory, NameNode, Oozie és egyéb web UI](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-linux-ambari-ssh-tunnel).</span><span class="sxs-lookup"><span data-stu-id="07915-115">For more information, see [Connect to HDInsight (Hadoop) by using SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) and [Use SSH tunneling to access Ambari web UI, JobHistory, NameNode, Oozie, and other web UIs](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-linux-ambari-ssh-tunnel).</span></span> 
 

## <a name="create-a-spark-scala-application-and-configure-it-for-remote-debugging"></a><span data-ttu-id="07915-116">Spark Scala-alkalmazás létrehozása, és konfigurálja a távoli hibakeresés</span><span class="sxs-lookup"><span data-stu-id="07915-116">Create a Spark Scala application and configure it for remote debugging</span></span>

1. <span data-ttu-id="07915-117">Indítsa el az IntelliJ IDEA, és ezután a projekt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="07915-117">Start IntelliJ IDEA, and then create a project.</span></span> <span data-ttu-id="07915-118">Az a **új projekt** párbeszédpanelen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="07915-118">In the **New Project** dialog box, do the following:</span></span>

   <span data-ttu-id="07915-119">a.</span><span class="sxs-lookup"><span data-stu-id="07915-119">a.</span></span> <span data-ttu-id="07915-120">Válassza ki **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="07915-120">Select **HDInsight**.</span></span> 

   <span data-ttu-id="07915-121">b.</span><span class="sxs-lookup"><span data-stu-id="07915-121">b.</span></span> <span data-ttu-id="07915-122">A Sablonválasztás Java vagy Scala a beállítások alapján.</span><span class="sxs-lookup"><span data-stu-id="07915-122">Select a Java or Scala template based on your preference.</span></span> <span data-ttu-id="07915-123">Az alábbi lehetőségek közül választhat:</span><span class="sxs-lookup"><span data-stu-id="07915-123">Select between the following options:</span></span>

      - <span data-ttu-id="07915-124">**A Spark on HDInsight (Scala)**</span><span class="sxs-lookup"><span data-stu-id="07915-124">**Spark on HDInsight (Scala)**</span></span>

      - <span data-ttu-id="07915-125">**A Spark on HDInsight (Java)**</span><span class="sxs-lookup"><span data-stu-id="07915-125">**Spark on HDInsight (Java)**</span></span>

      - <span data-ttu-id="07915-126">**A Spark on HDInsight-fürt futtatási minta (Scala)**</span><span class="sxs-lookup"><span data-stu-id="07915-126">**Spark on HDInsight Cluster Run Sample (Scala)**</span></span>

      <span data-ttu-id="07915-127">Ez a példa egy **a Spark on HDInsight fürt futtatása minta (Scala)** sablont.</span><span class="sxs-lookup"><span data-stu-id="07915-127">This example uses a **Spark on HDInsight Cluster Run Sample (Scala)** template.</span></span>

   <span data-ttu-id="07915-128">c.</span><span class="sxs-lookup"><span data-stu-id="07915-128">c.</span></span> <span data-ttu-id="07915-129">Az a **Build eszköz** listára, válassza ki, az igényeknek megfelelően az alábbiak valamelyikét:</span><span class="sxs-lookup"><span data-stu-id="07915-129">In the **Build tool** list, select either of the following, according to your need:</span></span>

      - <span data-ttu-id="07915-130">**Maven**, Scala-projekt létrehozása varázsló támogatásához</span><span class="sxs-lookup"><span data-stu-id="07915-130">**Maven**, for Scala project-creation wizard support</span></span>

      -  <span data-ttu-id="07915-131">**SBT**, a függőségek kezelésére, és a Scala-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="07915-131">**SBT**, for managing the dependencies and building for the Scala project</span></span> 

      ![Hibakeresési-projekt létrehozása](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-create-projectfor-debug-remotely.png)

   <span data-ttu-id="07915-133">d.</span><span class="sxs-lookup"><span data-stu-id="07915-133">d.</span></span> <span data-ttu-id="07915-134">Válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="07915-134">Select **Next**.</span></span>     
 
3. <span data-ttu-id="07915-135">A következő **új projekt** ablakban tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="07915-135">In the next **New Project** window, do the following:</span></span>

   ![Válassza ki a Spark SDK](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-new-project.png)

   <span data-ttu-id="07915-137">a.</span><span class="sxs-lookup"><span data-stu-id="07915-137">a.</span></span> <span data-ttu-id="07915-138">Adja meg a projekt nevét és a projekt helyére.</span><span class="sxs-lookup"><span data-stu-id="07915-138">Enter a project name and project location.</span></span>

   <span data-ttu-id="07915-139">b.</span><span class="sxs-lookup"><span data-stu-id="07915-139">b.</span></span> <span data-ttu-id="07915-140">Az a **projekt SDK** legördülő listában válassza **Java 1.8** a **Spark 2.x** fürt, vagy válasszon **Java 1.7** a **Spark 1.x**  fürt.</span><span class="sxs-lookup"><span data-stu-id="07915-140">In the **Project SDK** drop-down list, select **Java 1.8** for **Spark 2.x** cluster or select **Java 1.7** for **Spark 1.x** cluster.</span></span>

   <span data-ttu-id="07915-141">c.</span><span class="sxs-lookup"><span data-stu-id="07915-141">c.</span></span> <span data-ttu-id="07915-142">Az a **Spark verzió** legördülő listából válassza ki, a Scala-projekt létrehozása varázsló Spark SDK és Scala SDK integrálja a megfelelő verziójával.</span><span class="sxs-lookup"><span data-stu-id="07915-142">In the **Spark Version** drop-down list, the Scala project creation wizard integrates the correct version for Spark SDK and Scala SDK.</span></span> <span data-ttu-id="07915-143">Ha a spark-fürt verziója korábbi, mint 2,0, válassza ki a **Spark 1.x**.</span><span class="sxs-lookup"><span data-stu-id="07915-143">If the spark cluster version is earlier than 2.0, select **Spark 1.x**.</span></span> <span data-ttu-id="07915-144">Máskülönben válassza **Spark 2.x.**</span><span class="sxs-lookup"><span data-stu-id="07915-144">Otherwise, select **Spark 2.x.**</span></span> <span data-ttu-id="07915-145">Ez a példa **Spark 2.0.2 (Scala 2.11.8)**.</span><span class="sxs-lookup"><span data-stu-id="07915-145">This example uses **Spark 2.0.2 (Scala 2.11.8)**.</span></span>

   <span data-ttu-id="07915-146">d.</span><span class="sxs-lookup"><span data-stu-id="07915-146">d.</span></span> <span data-ttu-id="07915-147">Válassza a **Finish** (Befejezés) elemet.</span><span class="sxs-lookup"><span data-stu-id="07915-147">Select **Finish**.</span></span>

4. <span data-ttu-id="07915-148">Válassza ki **src** > **fő** > **scala** az ebben a projektben nyissa meg a kódot.</span><span class="sxs-lookup"><span data-stu-id="07915-148">Select **src** > **main** > **scala** to open your code in the project.</span></span> <span data-ttu-id="07915-149">Ez a példa a **SparkCore_wasbloTest** parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="07915-149">This example uses the **SparkCore_wasbloTest** script.</span></span>

5. <span data-ttu-id="07915-150">Hozzáférés a **szerkesztése konfigurációk** menüben válassza ki a ikonra a jobb felső sarokban.</span><span class="sxs-lookup"><span data-stu-id="07915-150">To access the **Edit Configurations** menu, select the icon in the upper-right corner.</span></span> <span data-ttu-id="07915-151">Ebből a menüből is készít vagy szerkeszt a konfigurációk a távoli hibakereséshez.</span><span class="sxs-lookup"><span data-stu-id="07915-151">From this menu, you can create or edit the configurations for remote debugging.</span></span>

   ![Konfigurációk szerkesztése](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-edit-configurations.png) 

6. <span data-ttu-id="07915-153">Az a **Futtatás/Debug konfigurációk** párbeszédpanelen jelölje ki a plusz jelre (**+**).</span><span class="sxs-lookup"><span data-stu-id="07915-153">In the **Run/Debug Configurations** dialog box, select the plus sign (**+**).</span></span> <span data-ttu-id="07915-154">Válassza ki a **Spark feladat elküldése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="07915-154">Then select the **Submit Spark Job** option.</span></span>

   ![Adja hozzá az új konfiguráció](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-add-new-Configuration.png)
7. <span data-ttu-id="07915-156">Adja meg a információkat **neve**, **Spark-fürt**, és **fő osztálynév**.</span><span class="sxs-lookup"><span data-stu-id="07915-156">Enter information for **Name**, **Spark cluster**, and **Main class name**.</span></span> <span data-ttu-id="07915-157">Válassza ki **speciális konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="07915-157">Then select **Advanced configuration**.</span></span> 

   ![Futtassa a hibakeresési konfigurációk](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-run-debug-configurations.png)

8. <span data-ttu-id="07915-159">Az a **Spark küldésének speciális konfiguráció** párbeszédpanelen jelölje ki **engedélyezése Spark távoli hibakeresési**.</span><span class="sxs-lookup"><span data-stu-id="07915-159">In the **Spark Submission Advanced Configuration** dialog box, select **Enable Spark remote debug**.</span></span> <span data-ttu-id="07915-160">Adja meg az SSH-felhasználónév, majd adjon meg egy jelszót vagy titkos kulcsfájlt.</span><span class="sxs-lookup"><span data-stu-id="07915-160">Enter the SSH username, and then enter a password or use a private key file.</span></span> <span data-ttu-id="07915-161">A konfiguráció mentéséhez, válassza ki a **OK**.</span><span class="sxs-lookup"><span data-stu-id="07915-161">To save the configuration, select **OK**.</span></span>

   ![Spark távoli hibakeresés engedélyezése](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-enable-spark-remote-debug.png)

9. <span data-ttu-id="07915-163">A konfiguráció most menti a megadott névvel.</span><span class="sxs-lookup"><span data-stu-id="07915-163">The configuration is now saved with the name you provided.</span></span> <span data-ttu-id="07915-164">A konfigurációs részletek megtekintéséhez válassza ki a konfiguráció nevét.</span><span class="sxs-lookup"><span data-stu-id="07915-164">To view the configuration details, select the configuration name.</span></span> <span data-ttu-id="07915-165">A módosításokat, válassza ki a **szerkesztése konfigurációk**.</span><span class="sxs-lookup"><span data-stu-id="07915-165">To make changes, select **Edit Configurations**.</span></span> 

10. <span data-ttu-id="07915-166">A konfigurációs beállítások elvégzése után a projekt futtatni a távoli fürt, vagy hajtsa végre a távoli hibakeresés.</span><span class="sxs-lookup"><span data-stu-id="07915-166">After you complete the configurations settings, you can run the project against the remote cluster or perform remote debugging.</span></span>

## <a name="learn-how-to-perform-remote-debugging"></a><span data-ttu-id="07915-167">Ismerje meg, hogyan hajthat végre a távoli hibakeresés</span><span class="sxs-lookup"><span data-stu-id="07915-167">Learn how to perform remote debugging</span></span>
### <a name="scenario-1-perform-remote-run"></a><span data-ttu-id="07915-168">1. forgatókönyv: Hajtsa végre a távoli Futtatás</span><span class="sxs-lookup"><span data-stu-id="07915-168">Scenario 1: Perform remote run</span></span>

<span data-ttu-id="07915-169">Ebben a szakaszban azt mutatja be az illesztőprogramok és végrehajtója hibakeresését.</span><span class="sxs-lookup"><span data-stu-id="07915-169">In this section, we show you how to debug drivers and executors.</span></span>

    import org.apache.spark.{SparkConf, SparkContext}

    object LogQuery {
      val exampleApacheLogs = List(
        """10.10.10.10 - "FRED" [18/Jan/2013:17:56:07 +1100] "GET http://images.com/2013/Generic.jpg
          | HTTP/1.1" 304 315 "http://referall.com/" "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1;
          | GTB7.4; .NET CLR 2.0.50727; .NET CLR 3.0.04506.30; .NET CLR 3.0.04506.648; .NET CLR
          | 3.5.21022; .NET CLR 3.0.4506.2152; .NET CLR 1.0.3705; .NET CLR 1.1.4322; .NET CLR
          | 3.5.30729; Release=ARP)" "UD-1" - "image/jpeg" "whatever" 0.350 "-" - "" 265 923 934 ""
          | 62.24.11.25 images.com 1358492167 - Whatup""".stripMargin.lines.mkString,
        """10.10.10.10 - "FRED" [18/Jan/2013:18:02:37 +1100] "GET http://images.com/2013/Generic.jpg
          | HTTP/1.1" 304 306 "http:/referall.com" "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1;
          | GTB7.4; .NET CLR 2.0.50727; .NET CLR 3.0.04506.30; .NET CLR 3.0.04506.648; .NET CLR
          | 3.5.21022; .NET CLR 3.0.4506.2152; .NET CLR 1.0.3705; .NET CLR 1.1.4322; .NET CLR
          | 3.5.30729; Release=ARP)" "UD-1" - "image/jpeg" "whatever" 0.352 "-" - "" 256 977 988 ""
          | 0 73.23.2.15 images.com 1358492557 - Whatup""".stripMargin.lines.mkString
      )
      def main(args: Array[String]) {
        val sparkconf = new SparkConf().setAppName("Log Query")
        val sc = new SparkContext(sparkconf)
        val dataSet = sc.parallelize(exampleApacheLogs)
        // scalastyle:off
        val apacheLogRegex =
          """^([\d.]+) (\S+) (\S+) \[([\w\d:/]+\s[+\-]\d{4})\] "(.+?)" (\d{3}) ([\d\-]+) "([^"]+)" "([^"]+)".*""".r
        // scalastyle:on
        /** Tracks the total query count and number of aggregate bytes for a particular group. */
        class Stats(val count: Int, val numBytes: Int) extends Serializable {
          def merge(other: Stats): Stats = new Stats(count + other.count, numBytes + other.numBytes)
          override def toString: String = "bytes=%s\tn=%s".format(numBytes, count)
        }
        def extractKey(line: String): (String, String, String) = {
          apacheLogRegex.findFirstIn(line) match {
            case Some(apacheLogRegex(ip, _, user, dateTime, query, status, bytes, referer, ua)) =>
              if (user != "\"-\"") (ip, user, query)
              else (null, null, null)
            case _ => (null, null, null)
          }
        }
        def extractStats(line: String): Stats = {
          apacheLogRegex.findFirstIn(line) match {
            case Some(apacheLogRegex(ip, _, user, dateTime, query, status, bytes, referer, ua)) =>
              new Stats(1, bytes.toInt)
            case _ => new Stats(1, 0)
          }
        }
        
        dataSet.map(line => (extractKey(line), extractStats(line)))
          .reduceByKey((a, b) => a.merge(b))
          .collect().foreach{
          case (user, query) => println("%s\t%s".format(user, query))}

        sc.stop()
      }
    }


1. <span data-ttu-id="07915-170">A legfrissebb pontok beállítása, majd válassza ki a **Debug** ikonra.</span><span class="sxs-lookup"><span data-stu-id="07915-170">Set up breaking points, and then select the **Debug** icon.</span></span>

   ![Válassza ki a hibakeresési ikon](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debug-icon.png)

2. <span data-ttu-id="07915-172">A program végrehajtását a legfrissebb pontot ér el, ha megjelenik egy **illesztőprogram** lapra, és két **végrehajtó** adhatja a **hibakereső** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="07915-172">When the program execution reaches the breaking point, you see a **Driver** tab and two **Executor** tabs in the **Debugger** pane.</span></span> <span data-ttu-id="07915-173">Válassza ki a **folytatása Program** továbbra is fut a kód, amely ezután a következő töréspont eléri, és elsősorban a megfelelő ikonra **végrehajtó** fülre. Tekintse át a végrehajtási naplók a megfelelő **konzol** fülre.</span><span class="sxs-lookup"><span data-stu-id="07915-173">Select the **Resume Program** icon to continue running the code, which then reaches the next breakpoint and focuses on the corresponding **Executor** tab. You can review the execution logs on the corresponding **Console** tab.</span></span>

   ![Hibakeresés lap](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debugger-tab.png)

### <a name="scenario-2-perform-remote-debugging-and-bug-fixing"></a><span data-ttu-id="07915-175">2. forgatókönyv: Hajtsa végre a távoli hibakereséssel és a hiba elhárítása</span><span class="sxs-lookup"><span data-stu-id="07915-175">Scenario 2: Perform remote debugging and bug fixing</span></span>
<span data-ttu-id="07915-176">Ez a szakasz azt mutatja be dinamikus frissítése a változó értéke egy egyszerű javítás az IntelliJ hibakeresési funkció használatával.</span><span class="sxs-lookup"><span data-stu-id="07915-176">In this section, we show you how to dynamically update the variable value by using the IntelliJ debugging capability for a simple fix.</span></span> <span data-ttu-id="07915-177">Az alábbi példakódban kivételt vált ki, mert a célfájl már létezik.</span><span class="sxs-lookup"><span data-stu-id="07915-177">In the following code example, an exception is thrown because the target file already exists.</span></span>
  
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext

        object SparkCore_WasbIOTest {
          def main(arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("SparkCore_WasbIOTest")
            val sc = new SparkContext(conf)
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

            // Find the rows that have only one digit in the sixth column.
            val rdd1 = rdd.filter(s => s.split(",")(6).length() == 1)

            try {
              var target = "wasb:///HVACout2_testdebug1";
              rdd1.saveAsTextFile(target);
            } catch {
              case ex: Exception => {
                throw ex;
              }
            }
          }
        }


#### <a name="to-perform-remote-debugging-and-bug-fixing"></a><span data-ttu-id="07915-178">Távoli hibakereséssel és a hiba elhárítása</span><span class="sxs-lookup"><span data-stu-id="07915-178">To perform remote debugging and bug fixing</span></span>
1. <span data-ttu-id="07915-179">Két legfrissebb pontok beállítása, majd válassza ki a **Debug** ikonra a távoli hibakeresési megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="07915-179">Set up two breaking points, and then select the **Debug** icon to start the remote debugging process.</span></span>

2. <span data-ttu-id="07915-180">A kód nem az első legfrissebb ponton, és a paraméter és a változó információk jelennek meg a **változók** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="07915-180">The code stops at the first breaking point, and the parameter and variable information are shown in the **Variables** pane.</span></span> 

3. <span data-ttu-id="07915-181">Válassza ki a **folytatása Program** ikon a folytatáshoz.</span><span class="sxs-lookup"><span data-stu-id="07915-181">Select the **Resume Program** icon to continue.</span></span> <span data-ttu-id="07915-182">A kódot a második ponton leáll.</span><span class="sxs-lookup"><span data-stu-id="07915-182">The code stops at the second point.</span></span> <span data-ttu-id="07915-183">A kivétel várt módon.</span><span class="sxs-lookup"><span data-stu-id="07915-183">The exception is caught as expected.</span></span>

  ![A throw hiba](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-throw-error.png) 

4. <span data-ttu-id="07915-185">Válassza ki a **folytatása Program** újra ikonra.</span><span class="sxs-lookup"><span data-stu-id="07915-185">Select the **Resume Program** icon again.</span></span> <span data-ttu-id="07915-186">A **HDInsight Spark küldésének** ablak egy "feladat futtatása nem sikerült" hibaüzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="07915-186">The **HDInsight Spark Submission** window displays a "job run failed" error.</span></span>

  ![Hibaüzenet küldése](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-error-submission.png) 

5. <span data-ttu-id="07915-188">Dinamikus frissítése a változó értéke az IntelliJ-hibakeresés funkció használatával, jelölje be **Debug** újra.</span><span class="sxs-lookup"><span data-stu-id="07915-188">To dynamically update the variable value by using the IntelliJ debugging capability, select **Debug** again.</span></span> <span data-ttu-id="07915-189">A **változók** ablaktáblán jelenik meg újra.</span><span class="sxs-lookup"><span data-stu-id="07915-189">The **Variables** pane appears again.</span></span> 

6. <span data-ttu-id="07915-190">Kattintson a jobb gombbal a cél a **Debug** lapra, majd válassza ki **érték beállítása**.</span><span class="sxs-lookup"><span data-stu-id="07915-190">Right-click the target on the **Debug** tab, and then select **Set Value**.</span></span> <span data-ttu-id="07915-191">Ezután adja meg az új értéket a változóhoz.</span><span class="sxs-lookup"><span data-stu-id="07915-191">Next, enter a new value for the variable.</span></span> <span data-ttu-id="07915-192">Válassza ki **Enter** mentheti az értéket.</span><span class="sxs-lookup"><span data-stu-id="07915-192">Then select **Enter** to save the value.</span></span> 

  ![Érték beállítása](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-set-value.png) 

7. <span data-ttu-id="07915-194">Válassza ki a **folytatása Program** ikonra kattintva továbbra is futtassa a programot.</span><span class="sxs-lookup"><span data-stu-id="07915-194">Select the **Resume Program** icon to continue to run the program.</span></span> <span data-ttu-id="07915-195">Nincs kivétel ebben az esetben.</span><span class="sxs-lookup"><span data-stu-id="07915-195">This time, no exception is caught.</span></span> <span data-ttu-id="07915-196">Láthatja, hogy a projekt sikeresen kivételek nélkül futtatja.</span><span class="sxs-lookup"><span data-stu-id="07915-196">You can see that the project runs successfully without any exceptions.</span></span>

  ![Kivétel nélkül hibakeresése](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debug-without-exception.png)

## <span data-ttu-id="07915-198"><a name="seealso"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="07915-198"><a name="seealso"></a>Next steps</span></span>
* [<span data-ttu-id="07915-199">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="07915-199">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="demo"></a><span data-ttu-id="07915-200">Bemutató</span><span class="sxs-lookup"><span data-stu-id="07915-200">Demo</span></span>
* <span data-ttu-id="07915-201">Hozzon létre Scala project (videó): [Spark Scala-alkalmazások létrehozása](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="07915-201">Create Scala project (video): [Create Spark Scala Applications](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span></span>
* <span data-ttu-id="07915-202">Távoli hibakeresési (videó): [IntelliJ a Spark-alkalmazások távolról a HDInsight-fürtök használata Azure eszköztára](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="07915-202">Remote debug (video): [Use Azure Toolkit for IntelliJ to debug Spark applications remotely on an HDInsight cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span></span>

### <a name="scenarios"></a><span data-ttu-id="07915-203">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="07915-203">Scenarios</span></span>
* [<span data-ttu-id="07915-204">Spark és BI: interaktív adatelemzés végrehajtása a Spark hdinsight BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="07915-204">Spark with BI: Perform interactive data analysis by using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="07915-205">Spark és Machine Learning: Spark on HDInsight HVAC-adatok épület-hőmérséklet elemzésére használata</span><span class="sxs-lookup"><span data-stu-id="07915-205">Spark with Machine Learning: Use Spark in HDInsight to analyze building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="07915-206">Spark és Machine Learning: A Spark on HDInsight használata az élelmiszervizsgálati eredmények előrejelzésére</span><span class="sxs-lookup"><span data-stu-id="07915-206">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="07915-207">Spark Streaming: Spark on HDInsight használata valós idejű streamelési alkalmazásokat hozhatnak létre</span><span class="sxs-lookup"><span data-stu-id="07915-207">Spark Streaming: Use Spark in HDInsight to build real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="07915-208">A webhelynapló elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="07915-208">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="07915-209">Alkalmazások létrehozása és futtatása</span><span class="sxs-lookup"><span data-stu-id="07915-209">Create and run applications</span></span>
* [<span data-ttu-id="07915-210">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="07915-210">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="07915-211">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="07915-211">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="07915-212">Eszközök és bővítmények</span><span class="sxs-lookup"><span data-stu-id="07915-212">Tools and extensions</span></span>
* [<span data-ttu-id="07915-213">Az IntelliJ Azure eszköztára használata Spark-alkalmazások a HDInsight-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="07915-213">Use Azure Toolkit for IntelliJ to create Spark applications for an HDInsight cluster</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="07915-214">Az intellij-t Azure eszközkészlet segítségével VPN-en keresztül távolról Spark-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="07915-214">Use Azure Toolkit for IntelliJ to debug Spark applications remotely through VPN</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="07915-215">Használja a HDInsight Tools for IntelliJ a Hortonworks védőfal</span><span class="sxs-lookup"><span data-stu-id="07915-215">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [<span data-ttu-id="07915-216">Az Eclipse Azure eszközkészlet a HDInsight Tools használatával Spark-alkalmazások létrehozása</span><span class="sxs-lookup"><span data-stu-id="07915-216">Use HDInsight Tools in Azure Toolkit for Eclipse to create Spark applications</span></span>](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [<span data-ttu-id="07915-217">Zeppelin notebookok használata Spark-fürttel HDInsighton</span><span class="sxs-lookup"><span data-stu-id="07915-217">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="07915-218">A HDInsight Spark-fürt Jupyter notebookokhoz elérhető kernelek</span><span class="sxs-lookup"><span data-stu-id="07915-218">Kernels available for Jupyter notebook in the Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="07915-219">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="07915-219">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="07915-220">A Jupyter telepítése a számítógépre, majd csatlakozás egy HDInsight Spark-fürthöz</span><span class="sxs-lookup"><span data-stu-id="07915-220">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="07915-221">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="07915-221">Manage resources</span></span>
* [<span data-ttu-id="07915-222">Apache Spark-fürt erőforrásainak kezelése az Azure HDInsightban</span><span class="sxs-lookup"><span data-stu-id="07915-222">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="07915-223">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="07915-223">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
