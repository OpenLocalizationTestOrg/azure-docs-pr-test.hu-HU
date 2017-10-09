---
title: "Az IntelliJ Azure eszköztára: Debug Spark alkalmazások távolról SSH |} Microsoft Docs"
description: "Részletes útmutatás hogyan toouse Azure eszközkészlet a HDInsight Tools IntelliJ toodebug alkalmazások távolról a HDInsight-fürtök SSH-n keresztül"
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
ms.openlocfilehash: bf3ab9d04c2ff9fcb6bbbdeefb11f55a12fbd845
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="debug-spark-applications-on-an-hdinsight-cluster-with-azure-toolkit-for-intellij-through-ssh"></a><span data-ttu-id="db0db-104">Az SSH-n keresztül az IntelliJ HDInsight-fürtök az Azure-eszközkészlet a Spark-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="db0db-104">Debug Spark applications on an HDInsight cluster with Azure Toolkit for IntelliJ through SSH</span></span>

<span data-ttu-id="db0db-105">Ez a cikk részletes útmutatást hogyan toouse Azure eszközkészlet a HDInsight Tools for IntelliJ toodebug applications távolról a HDInsight-fürtök.</span><span class="sxs-lookup"><span data-stu-id="db0db-105">This article provides step-by-step guidance on how toouse HDInsight Tools in Azure Toolkit for IntelliJ toodebug applications remotely on an HDInsight cluster.</span></span> <span data-ttu-id="db0db-106">toodebug a projekt is megtekintheti a hello [az IntelliJ Debug HDInsight Spark Azure eszközkészlet alkalmazások](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) videó.</span><span class="sxs-lookup"><span data-stu-id="db0db-106">toodebug your project, you can also view hello [Debug HDInsight Spark applications with Azure Toolkit for IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) video.</span></span>

<span data-ttu-id="db0db-107">**Előfeltételek**</span><span class="sxs-lookup"><span data-stu-id="db0db-107">**Prerequisites**</span></span>

* <span data-ttu-id="db0db-108">**A HDInsight Tools az intellij-t Azure eszköztára**.</span><span class="sxs-lookup"><span data-stu-id="db0db-108">**HDInsight Tools in Azure Toolkit for IntelliJ**.</span></span> <span data-ttu-id="db0db-109">Ez az eszköz az intellij-t Azure Toolkit részét képezi.</span><span class="sxs-lookup"><span data-stu-id="db0db-109">This tool is part of Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="db0db-110">További információkért lásd: [Azure eszközkészlet telepítése az IntelliJ](https://docs.microsoft.com/en-us/azure/azure-toolkit-for-intellij-installation).</span><span class="sxs-lookup"><span data-stu-id="db0db-110">For more information, see [Install Azure Toolkit for IntelliJ](https://docs.microsoft.com/en-us/azure/azure-toolkit-for-intellij-installation).</span></span>
* <span data-ttu-id="db0db-111">**Az IntelliJ Azure eszköztára**.</span><span class="sxs-lookup"><span data-stu-id="db0db-111">**Azure Toolkit for IntelliJ**.</span></span> <span data-ttu-id="db0db-112">Az eszközkészlet toocreate Spark-alkalmazások használata a HDInsight-fürtök.</span><span class="sxs-lookup"><span data-stu-id="db0db-112">Use this toolkit toocreate Spark applications for an HDInsight cluster.</span></span> <span data-ttu-id="db0db-113">További információkért kövesse a hello utasításait [IntelliJ toocreate Spark-alkalmazások HDInsight-fürtök használata Azure eszköztára](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin).</span><span class="sxs-lookup"><span data-stu-id="db0db-113">For more information, follow hello instructions in [Use Azure Toolkit for IntelliJ toocreate Spark applications for an HDInsight cluster](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin).</span></span>
* <span data-ttu-id="db0db-114">**HDInsight SSH szolgáltatást a felhasználónév és jelszó felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="db0db-114">**HDInsight SSH service with username and password management**.</span></span> <span data-ttu-id="db0db-115">További információkért lásd: [tooHDInsight (Hadoop) csatlakozzon az ssh protokoll használatával](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) és [az SSH tunneling tooaccess Ambari webes felhasználói felület, JobHistory, NameNode, Oozie és egyéb web UI](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-linux-ambari-ssh-tunnel).</span><span class="sxs-lookup"><span data-stu-id="db0db-115">For more information, see [Connect tooHDInsight (Hadoop) by using SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) and [Use SSH tunneling tooaccess Ambari web UI, JobHistory, NameNode, Oozie, and other web UIs](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-linux-ambari-ssh-tunnel).</span></span> 
 

## <a name="create-a-spark-scala-application-and-configure-it-for-remote-debugging"></a><span data-ttu-id="db0db-116">Spark Scala-alkalmazás létrehozása, és konfigurálja a távoli hibakeresés</span><span class="sxs-lookup"><span data-stu-id="db0db-116">Create a Spark Scala application and configure it for remote debugging</span></span>

1. <span data-ttu-id="db0db-117">Indítsa el az IntelliJ IDEA, és ezután a projekt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="db0db-117">Start IntelliJ IDEA, and then create a project.</span></span> <span data-ttu-id="db0db-118">A hello **új projekt** párbeszédpanel mezőbe hello a következő:</span><span class="sxs-lookup"><span data-stu-id="db0db-118">In hello **New Project** dialog box, do hello following:</span></span>

   <span data-ttu-id="db0db-119">a.</span><span class="sxs-lookup"><span data-stu-id="db0db-119">a.</span></span> <span data-ttu-id="db0db-120">Válassza ki **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="db0db-120">Select **HDInsight**.</span></span> 

   <span data-ttu-id="db0db-121">b.</span><span class="sxs-lookup"><span data-stu-id="db0db-121">b.</span></span> <span data-ttu-id="db0db-122">A Sablonválasztás Java vagy Scala a beállítások alapján.</span><span class="sxs-lookup"><span data-stu-id="db0db-122">Select a Java or Scala template based on your preference.</span></span> <span data-ttu-id="db0db-123">Hello alábbi beállítások közül választhat:</span><span class="sxs-lookup"><span data-stu-id="db0db-123">Select between hello following options:</span></span>

      - <span data-ttu-id="db0db-124">**A Spark on HDInsight (Scala)**</span><span class="sxs-lookup"><span data-stu-id="db0db-124">**Spark on HDInsight (Scala)**</span></span>

      - <span data-ttu-id="db0db-125">**A Spark on HDInsight (Java)**</span><span class="sxs-lookup"><span data-stu-id="db0db-125">**Spark on HDInsight (Java)**</span></span>

      - <span data-ttu-id="db0db-126">**A Spark on HDInsight-fürt futtatási minta (Scala)**</span><span class="sxs-lookup"><span data-stu-id="db0db-126">**Spark on HDInsight Cluster Run Sample (Scala)**</span></span>

      <span data-ttu-id="db0db-127">Ez a példa egy **a Spark on HDInsight fürt futtatása minta (Scala)** sablont.</span><span class="sxs-lookup"><span data-stu-id="db0db-127">This example uses a **Spark on HDInsight Cluster Run Sample (Scala)** template.</span></span>

   <span data-ttu-id="db0db-128">c.</span><span class="sxs-lookup"><span data-stu-id="db0db-128">c.</span></span> <span data-ttu-id="db0db-129">A hello **Build eszköz** listára, válassza ki a következő, tooyour szükség szerint hello valamelyikét:</span><span class="sxs-lookup"><span data-stu-id="db0db-129">In hello **Build tool** list, select either of hello following, according tooyour need:</span></span>

      - <span data-ttu-id="db0db-130">**Maven**, Scala-projekt létrehozása varázsló támogatásához</span><span class="sxs-lookup"><span data-stu-id="db0db-130">**Maven**, for Scala project-creation wizard support</span></span>

      -  <span data-ttu-id="db0db-131">**SBT**, hello függőségek kezelése és hello Scala-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="db0db-131">**SBT**, for managing hello dependencies and building for hello Scala project</span></span> 

      ![Hibakeresési-projekt létrehozása](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-create-projectfor-debug-remotely.png)

   <span data-ttu-id="db0db-133">d.</span><span class="sxs-lookup"><span data-stu-id="db0db-133">d.</span></span> <span data-ttu-id="db0db-134">Válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="db0db-134">Select **Next**.</span></span>     
 
3. <span data-ttu-id="db0db-135">Hello a következő **új projekt** ablakban, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="db0db-135">In hello next **New Project** window, do hello following:</span></span>

   ![Válassza ki a hello Spark SDK](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-new-project.png)

   <span data-ttu-id="db0db-137">a.</span><span class="sxs-lookup"><span data-stu-id="db0db-137">a.</span></span> <span data-ttu-id="db0db-138">Adja meg a projekt nevét és a projekt helyére.</span><span class="sxs-lookup"><span data-stu-id="db0db-138">Enter a project name and project location.</span></span>

   <span data-ttu-id="db0db-139">b.</span><span class="sxs-lookup"><span data-stu-id="db0db-139">b.</span></span> <span data-ttu-id="db0db-140">A hello **projekt SDK** legördülő listában válassza **Java 1.8** a **Spark 2.x** fürt, vagy válasszon **Java 1.7** a **Spark 1. x** fürt.</span><span class="sxs-lookup"><span data-stu-id="db0db-140">In hello **Project SDK** drop-down list, select **Java 1.8** for **Spark 2.x** cluster or select **Java 1.7** for **Spark 1.x** cluster.</span></span>

   <span data-ttu-id="db0db-141">c.</span><span class="sxs-lookup"><span data-stu-id="db0db-141">c.</span></span> <span data-ttu-id="db0db-142">A hello **Spark verzió** legördülő listából válassza ki, hello Scala projekt létrehozása varázsló Spark SDK és Scala SDK integrálja a hello megfelelő verzióját.</span><span class="sxs-lookup"><span data-stu-id="db0db-142">In hello **Spark Version** drop-down list, hello Scala project creation wizard integrates hello correct version for Spark SDK and Scala SDK.</span></span> <span data-ttu-id="db0db-143">Ha hello spark-fürt verziója korábbi, mint 2,0, válassza ki a **Spark 1.x**.</span><span class="sxs-lookup"><span data-stu-id="db0db-143">If hello spark cluster version is earlier than 2.0, select **Spark 1.x**.</span></span> <span data-ttu-id="db0db-144">Máskülönben válassza **Spark 2.x.**</span><span class="sxs-lookup"><span data-stu-id="db0db-144">Otherwise, select **Spark 2.x.**</span></span> <span data-ttu-id="db0db-145">Ez a példa **Spark 2.0.2 (Scala 2.11.8)**.</span><span class="sxs-lookup"><span data-stu-id="db0db-145">This example uses **Spark 2.0.2 (Scala 2.11.8)**.</span></span>

   <span data-ttu-id="db0db-146">d.</span><span class="sxs-lookup"><span data-stu-id="db0db-146">d.</span></span> <span data-ttu-id="db0db-147">Válassza a **Finish** (Befejezés) elemet.</span><span class="sxs-lookup"><span data-stu-id="db0db-147">Select **Finish**.</span></span>

4. <span data-ttu-id="db0db-148">Válassza ki **src** > **fő** > **scala** tooopen hello projekt kódját.</span><span class="sxs-lookup"><span data-stu-id="db0db-148">Select **src** > **main** > **scala** tooopen your code in hello project.</span></span> <span data-ttu-id="db0db-149">Ez a példa hello **SparkCore_wasbloTest** parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="db0db-149">This example uses hello **SparkCore_wasbloTest** script.</span></span>

5. <span data-ttu-id="db0db-150">tooaccess hello **szerkesztése konfigurációk** menü, hello jobb felső sarokban válassza hello ikonra.</span><span class="sxs-lookup"><span data-stu-id="db0db-150">tooaccess hello **Edit Configurations** menu, select hello icon in hello upper-right corner.</span></span> <span data-ttu-id="db0db-151">Ebből a menüből is készít vagy szerkeszt hello konfigurációk távoli hibakereséshez.</span><span class="sxs-lookup"><span data-stu-id="db0db-151">From this menu, you can create or edit hello configurations for remote debugging.</span></span>

   ![Konfigurációk szerkesztése](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-edit-configurations.png) 

6. <span data-ttu-id="db0db-153">A hello **Futtatás/Debug konfigurációk** párbeszédpanel megnyitásához, jelölje be hello plusz jel (**+**).</span><span class="sxs-lookup"><span data-stu-id="db0db-153">In hello **Run/Debug Configurations** dialog box, select hello plus sign (**+**).</span></span> <span data-ttu-id="db0db-154">Válassza ki hello **Spark feladat elküldése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="db0db-154">Then select hello **Submit Spark Job** option.</span></span>

   ![Adja hozzá az új konfiguráció](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-add-new-Configuration.png)
7. <span data-ttu-id="db0db-156">Adja meg a információkat **neve**, **Spark-fürt**, és **fő osztálynév**.</span><span class="sxs-lookup"><span data-stu-id="db0db-156">Enter information for **Name**, **Spark cluster**, and **Main class name**.</span></span> <span data-ttu-id="db0db-157">Válassza ki **speciális konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="db0db-157">Then select **Advanced configuration**.</span></span> 

   ![Futtassa a hibakeresési konfigurációk](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-run-debug-configurations.png)

8. <span data-ttu-id="db0db-159">A hello **Spark küldésének speciális konfiguráció** párbeszédpanelen jelölje ki **engedélyezése Spark távoli hibakeresési**.</span><span class="sxs-lookup"><span data-stu-id="db0db-159">In hello **Spark Submission Advanced Configuration** dialog box, select **Enable Spark remote debug**.</span></span> <span data-ttu-id="db0db-160">Adja meg az SSH-felhasználónév hello, majd adjon meg egy jelszót vagy titkos kulcsfájlt.</span><span class="sxs-lookup"><span data-stu-id="db0db-160">Enter hello SSH username, and then enter a password or use a private key file.</span></span> <span data-ttu-id="db0db-161">toosave hello konfigurációs, jelölje be **OK**.</span><span class="sxs-lookup"><span data-stu-id="db0db-161">toosave hello configuration, select **OK**.</span></span>

   ![Spark távoli hibakeresés engedélyezése](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-enable-spark-remote-debug.png)

9. <span data-ttu-id="db0db-163">hello konfigurációs megadott hello nevű mentése.</span><span class="sxs-lookup"><span data-stu-id="db0db-163">hello configuration is now saved with hello name you provided.</span></span> <span data-ttu-id="db0db-164">tooview hello konfigurációs adatokat, válassza hello konfiguráció neve.</span><span class="sxs-lookup"><span data-stu-id="db0db-164">tooview hello configuration details, select hello configuration name.</span></span> <span data-ttu-id="db0db-165">toomake módosításokat, válassza ki **szerkesztése konfigurációk**.</span><span class="sxs-lookup"><span data-stu-id="db0db-165">toomake changes, select **Edit Configurations**.</span></span> 

10. <span data-ttu-id="db0db-166">Hello konfigurációs beállítások elvégzése után hello projekt futtatásához hello távoli fürt, vagy hajtsa végre a távoli hibakeresés.</span><span class="sxs-lookup"><span data-stu-id="db0db-166">After you complete hello configurations settings, you can run hello project against hello remote cluster or perform remote debugging.</span></span>

## <a name="learn-how-tooperform-remote-debugging"></a><span data-ttu-id="db0db-167">Megtudhatja, hogyan tooperform távoli hibakeresés</span><span class="sxs-lookup"><span data-stu-id="db0db-167">Learn how tooperform remote debugging</span></span>
### <a name="scenario-1-perform-remote-run"></a><span data-ttu-id="db0db-168">1. forgatókönyv: Hajtsa végre a távoli Futtatás</span><span class="sxs-lookup"><span data-stu-id="db0db-168">Scenario 1: Perform remote run</span></span>

<span data-ttu-id="db0db-169">Ez a szakasz azt mutatja be toodebug illesztőprogramok és végrehajtója.</span><span class="sxs-lookup"><span data-stu-id="db0db-169">In this section, we show you how toodebug drivers and executors.</span></span>

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
        /** Tracks hello total query count and number of aggregate bytes for a particular group. */
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


1. <span data-ttu-id="db0db-170">A legfrissebb pontok beállítása, majd válassza ki a hello **Debug** ikonra.</span><span class="sxs-lookup"><span data-stu-id="db0db-170">Set up breaking points, and then select hello **Debug** icon.</span></span>

   ![Válassza ki a hello hibakeresési ikon](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debug-icon.png)

2. <span data-ttu-id="db0db-172">Hello program végrehajtását hello legfrissebb pontot ér el, ha megjelenik egy **illesztőprogram** lapra, és két **végrehajtó** hello lapfülek **hibakereső** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="db0db-172">When hello program execution reaches hello breaking point, you see a **Driver** tab and two **Executor** tabs in hello **Debugger** pane.</span></span> <span data-ttu-id="db0db-173">Jelölje be hello **folytatása Program** ikon toocontinue hello kód, amely majd eléri a következő töréspont hello és hello megfelelő összpontosít futtató **végrehajtó** lapon. Áttekintheti a hello a végrehajtási naplók hello megfelelő **konzol** fülre.</span><span class="sxs-lookup"><span data-stu-id="db0db-173">Select hello **Resume Program** icon toocontinue running hello code, which then reaches hello next breakpoint and focuses on hello corresponding **Executor** tab. You can review hello execution logs on hello corresponding **Console** tab.</span></span>

   ![Hibakeresés lap](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debugger-tab.png)

### <a name="scenario-2-perform-remote-debugging-and-bug-fixing"></a><span data-ttu-id="db0db-175">2. forgatókönyv: Hajtsa végre a távoli hibakereséssel és a hiba elhárítása</span><span class="sxs-lookup"><span data-stu-id="db0db-175">Scenario 2: Perform remote debugging and bug fixing</span></span>
<span data-ttu-id="db0db-176">Ebben a szakaszban megmutatjuk, hogyan toodynamically frissítés hello változó értékét a hello IntelliJ hibakeresést egy egyszerű javítás funkció.</span><span class="sxs-lookup"><span data-stu-id="db0db-176">In this section, we show you how toodynamically update hello variable value by using hello IntelliJ debugging capability for a simple fix.</span></span> <span data-ttu-id="db0db-177">Az alábbi kódpéldát hello a rendszer kivételt hoz létre, mert hello célfájl már létezik.</span><span class="sxs-lookup"><span data-stu-id="db0db-177">In hello following code example, an exception is thrown because hello target file already exists.</span></span>
  
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext

        object SparkCore_WasbIOTest {
          def main(arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("SparkCore_WasbIOTest")
            val sc = new SparkContext(conf)
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

            // Find hello rows that have only one digit in hello sixth column.
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


#### <a name="tooperform-remote-debugging-and-bug-fixing"></a><span data-ttu-id="db0db-178">tooperform távoli hibakereséssel és a hiba elhárítása</span><span class="sxs-lookup"><span data-stu-id="db0db-178">tooperform remote debugging and bug fixing</span></span>
1. <span data-ttu-id="db0db-179">Két legfrissebb pontok beállítása, majd válassza ki a hello **Debug** ikon toostart hello távoli folyamat hibakeresése.</span><span class="sxs-lookup"><span data-stu-id="db0db-179">Set up two breaking points, and then select hello **Debug** icon toostart hello remote debugging process.</span></span>

2. <span data-ttu-id="db0db-180">hello kód nem hello első legfrissebb ponton, és hello paraméter és a változó információk láthatók hello **változók** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="db0db-180">hello code stops at hello first breaking point, and hello parameter and variable information are shown in hello **Variables** pane.</span></span> 

3. <span data-ttu-id="db0db-181">Jelölje be hello **folytatása Program** ikon toocontinue.</span><span class="sxs-lookup"><span data-stu-id="db0db-181">Select hello **Resume Program** icon toocontinue.</span></span> <span data-ttu-id="db0db-182">hello kód megáll hello második pont.</span><span class="sxs-lookup"><span data-stu-id="db0db-182">hello code stops at hello second point.</span></span> <span data-ttu-id="db0db-183">hello kivétel várt módon.</span><span class="sxs-lookup"><span data-stu-id="db0db-183">hello exception is caught as expected.</span></span>

  ![A throw hiba](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-throw-error.png) 

4. <span data-ttu-id="db0db-185">Jelölje be hello **folytatása Program** újra ikonra.</span><span class="sxs-lookup"><span data-stu-id="db0db-185">Select hello **Resume Program** icon again.</span></span> <span data-ttu-id="db0db-186">Hello **HDInsight Spark küldésének** ablak egy "feladat futtatása nem sikerült" hibaüzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="db0db-186">hello **HDInsight Spark Submission** window displays a "job run failed" error.</span></span>

  ![Hibaüzenet küldése](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-error-submission.png) 

5. <span data-ttu-id="db0db-188">toodynamically frissítés hello változó értéke által hello IntelliJ hibakeresési funkcióval, válassza ki **Debug** újra.</span><span class="sxs-lookup"><span data-stu-id="db0db-188">toodynamically update hello variable value by using hello IntelliJ debugging capability, select **Debug** again.</span></span> <span data-ttu-id="db0db-189">Hello **változók** ablaktáblán jelenik meg újra.</span><span class="sxs-lookup"><span data-stu-id="db0db-189">hello **Variables** pane appears again.</span></span> 

6. <span data-ttu-id="db0db-190">Kattintson a jobb gombbal hello célja a hello **Debug** lapra, majd válassza ki **érték beállítása**.</span><span class="sxs-lookup"><span data-stu-id="db0db-190">Right-click hello target on hello **Debug** tab, and then select **Set Value**.</span></span> <span data-ttu-id="db0db-191">Ezután adja meg az új érték hello változó.</span><span class="sxs-lookup"><span data-stu-id="db0db-191">Next, enter a new value for hello variable.</span></span> <span data-ttu-id="db0db-192">Válassza ki **Enter** toosave hello érték.</span><span class="sxs-lookup"><span data-stu-id="db0db-192">Then select **Enter** toosave hello value.</span></span> 

  ![Érték beállítása](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-set-value.png) 

7. <span data-ttu-id="db0db-194">Jelölje be hello **folytatása Program** ikon toocontinue toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="db0db-194">Select hello **Resume Program** icon toocontinue toorun hello program.</span></span> <span data-ttu-id="db0db-195">Nincs kivétel ebben az esetben.</span><span class="sxs-lookup"><span data-stu-id="db0db-195">This time, no exception is caught.</span></span> <span data-ttu-id="db0db-196">Láthatja, hogy hello projekt sikeresen kivételek nélkül futtatja.</span><span class="sxs-lookup"><span data-stu-id="db0db-196">You can see that hello project runs successfully without any exceptions.</span></span>

  ![Kivétel nélkül hibakeresése](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debug-without-exception.png)

## <span data-ttu-id="db0db-198"><a name="seealso"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="db0db-198"><a name="seealso"></a>Next steps</span></span>
* [<span data-ttu-id="db0db-199">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="db0db-199">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="demo"></a><span data-ttu-id="db0db-200">Bemutató</span><span class="sxs-lookup"><span data-stu-id="db0db-200">Demo</span></span>
* <span data-ttu-id="db0db-201">Hozzon létre Scala project (videó): [Spark Scala-alkalmazások létrehozása](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="db0db-201">Create Scala project (video): [Create Spark Scala Applications](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span></span>
* <span data-ttu-id="db0db-202">Távoli hibakeresési (videó): [IntelliJ toodebug Spark-alkalmazások távolról a HDInsight-fürtök használata Azure eszköztára](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="db0db-202">Remote debug (video): [Use Azure Toolkit for IntelliJ toodebug Spark applications remotely on an HDInsight cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span></span>

### <a name="scenarios"></a><span data-ttu-id="db0db-203">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="db0db-203">Scenarios</span></span>
* [<span data-ttu-id="db0db-204">Spark és BI: interaktív adatelemzés végrehajtása a Spark hdinsight BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="db0db-204">Spark with BI: Perform interactive data analysis by using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="db0db-205">Spark és Machine Learning: használja a Spark on HDInsight tooanalyze épület-hőmérséklet HVAC-adatok</span><span class="sxs-lookup"><span data-stu-id="db0db-205">Spark with Machine Learning: Use Spark in HDInsight tooanalyze building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="db0db-206">Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények</span><span class="sxs-lookup"><span data-stu-id="db0db-206">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="db0db-207">Spark Streaming: Használja a Spark on HDInsight toobuild valós idejű streamelési alkalmazások</span><span class="sxs-lookup"><span data-stu-id="db0db-207">Spark Streaming: Use Spark in HDInsight toobuild real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="db0db-208">A webhelynapló elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="db0db-208">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="db0db-209">Alkalmazások létrehozása és futtatása</span><span class="sxs-lookup"><span data-stu-id="db0db-209">Create and run applications</span></span>
* [<span data-ttu-id="db0db-210">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="db0db-210">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="db0db-211">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="db0db-211">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="db0db-212">Eszközök és bővítmények</span><span class="sxs-lookup"><span data-stu-id="db0db-212">Tools and extensions</span></span>
* [<span data-ttu-id="db0db-213">Az IntelliJ toocreate Spark-alkalmazások HDInsight-fürtök használata Azure eszköztára</span><span class="sxs-lookup"><span data-stu-id="db0db-213">Use Azure Toolkit for IntelliJ toocreate Spark applications for an HDInsight cluster</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="db0db-214">Az IntelliJ toodebug Spark-alkalmazások VPN-en keresztül távolról használható Azure eszköztára</span><span class="sxs-lookup"><span data-stu-id="db0db-214">Use Azure Toolkit for IntelliJ toodebug Spark applications remotely through VPN</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="db0db-215">Használja a HDInsight Tools for IntelliJ a Hortonworks védőfal</span><span class="sxs-lookup"><span data-stu-id="db0db-215">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [<span data-ttu-id="db0db-216">HDInsight-eszközök használata az Eclipse toocreate Spark-alkalmazások Azure eszköztára</span><span class="sxs-lookup"><span data-stu-id="db0db-216">Use HDInsight Tools in Azure Toolkit for Eclipse toocreate Spark applications</span></span>](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [<span data-ttu-id="db0db-217">Zeppelin notebookok használata Spark-fürttel HDInsighton</span><span class="sxs-lookup"><span data-stu-id="db0db-217">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="db0db-218">A HDInsight Spark-fürt hello Jupyter notebookokhoz elérhető kernelek</span><span class="sxs-lookup"><span data-stu-id="db0db-218">Kernels available for Jupyter notebook in hello Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="db0db-219">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="db0db-219">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="db0db-220">Jupyter telepítse a számítógépre, és csatlakozzon a HDInsight Spark-fürt tooan</span><span class="sxs-lookup"><span data-stu-id="db0db-220">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="db0db-221">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="db0db-221">Manage resources</span></span>
* [<span data-ttu-id="db0db-222">Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése</span><span class="sxs-lookup"><span data-stu-id="db0db-222">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="db0db-223">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="db0db-223">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
