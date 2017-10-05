---
title: "Az IntelliJ - hibakeresési alkalmazások távolról a HDInsight Spark az Azure eszköztára |} Microsoft Docs"
description: "Megtudhatja, hogyan használják az Azure-eszközkészlet a HDInsight Tools az IntelliJ HDInsight Spark futtatott távolról hibakeresési alkalmazásokkal fürtök VPN-en keresztül."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 55fb454f-c7dc-46de-a978-e242e9a94f4c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 5ce282aac94d0f22ea587cbe4005819310e23b1f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-toolkit-for-intellij-to-debug-applications-remotely-on-hdinsight-spark-through-vpn"></a>Az intellij-t Azure eszközkészlet segítségével távolról a VPN-en keresztül a HDInsight Spark-alkalmazások hibakeresését

Azt javasoljuk, hogy a hibakeresési távolról a spark applicaltion ssh módja. Útmutatásért lásd: [távolról az IntelliJ SSH-n keresztül a Azure eszközkészlet a HDInsight-fürtök a Spark-alkalmazások hibakeresését](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).

Ez a cikk részletes útmutatást a HDInsight Tools használatával IntelliJ Azure eszköztára a Spark on HDInsight Spark-fürt a feladat elküldése, és ezután az asztali számítógépről távolról hibakeresési azt. Ehhez a következő magas szintű lépéseket kell végrehajtani:

1. Pont-pont vagy a pont-pont Azure virtuális hálózat létrehozása. A jelen dokumentumban leírt lépések azt feltételezik, hogy a pont-pont hálózatot használ.
2. A pont-pont Azure virtuális hálózat részét képező Azure hdinsight Spark-fürt létrehozása.
3. Ellenőrizze a fürt headnode és az asztal közötti kapcsolatot.
4. Az IntelliJ IDEA Scala-alkalmazás létrehozása, és konfigurálja a távoli hibakereséshez.
5. Futtassa, és az alkalmazás hibakeresése.

## <a name="prerequisites"></a>Előfeltételek
* Azure-előfizetés. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* A HDInsight az Apache Spark-fürt. Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).
* Oracle Java fejlesztői készlet. A későbbiekben telepítheti az [Itt](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* IntelliJ IDEA. Ez a cikk 2017.1 verzióját használja. A későbbiekben telepítheti az [Itt](https://www.jetbrains.com/idea/download/).
* Az Azure eszköztára IntelliJ HDInsight eszközök. A HDInsight tools for IntelliJ érhetők el az intellij-t Azure eszköztára részeként. Az Azure-eszközkészlet telepítése, lásd: [az intellij-t az Azure eszközkészlet telepítésével](../azure-toolkit-for-intellij-installation.md).
* Jelentkezzen be az Azure-előfizetéshez az IntelliJ IDEA. Kövesse az utasításokat [Itt](hdinsight-apache-spark-intellij-tool-plugin.md).
* A távoli hibakereséshez a Windows rendszerű számítógépeken Spark Scala alkalmazás futtatásakor kaphat kivételt a [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356) , amelyek miatt a Windows egy hiányzó WinUtils.exe következik be. Ez a hiba megoldása érdekében kell [töltse le a végrehajtható fájl itt](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) egy olyan helyre, például a **C:\WinUtils\bin**. Majd adjon hozzá egy környezeti változó **HADOOP_HOME** és a változó értékét állíthatja be **C\WinUtils**.

## <a name="step-1-create-an-azure-virtual-network"></a>1. lépés: Az Azure virtuális hálózat létrehozása
Kövesse az utasításokat az alábbi hozzon létre egy Azure virtuális hálózatra, és ellenőrizze az asztal és az Azure virtuális hálózat közötti kapcsolat mutató hivatkozásokat tartalmaz.

* [VNet létrehozása az Azure portál használatával pont-pont VPN-kapcsolattal](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [VNet létrehozása a PowerShell használatával pont-pont VPN-kapcsolattal](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
* [PowerShell virtuális hálózat egy pont – hely kapcsolat beállítása](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

## <a name="step-2-create-an-hdinsight-spark-cluster"></a>2. lépés: Egy HDInsight Spark-fürt létrehozása
A létrehozott Azure virtuális hálózat részét képező Azure HDInsight is Apache Spark-fürt kell létrehoznia. Az információk rendelkezésre [hdinsight létrehozása Linux-alapú fürtökön](hdinsight-hadoop-provision-linux-clusters.md). Választható konfiguráció részeként válassza ki az előző lépésben létrehozott Azure virtuális hálózat.

## <a name="step-3-verify-the-connectivity-between-the-cluster-headnode-and-your-desktop"></a>3. lépés: Ellenőrizze a fürt headnode és az asztal közötti kapcsolat
1. Az IP-címét a headnode beolvasása. Ambari felhasználói felületének megnyitásához a fürthöz. A fürt paneljén kattintson **irányítópult**.

    ![Headnode IP-cím keresése](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/launch-ambari-ui.png)
2. Az Ambari felhasználói felülete, a jobb felső sarokban kattintson **állomások**.

    ![Headnode IP-cím keresése](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/ambari-hosts.png)
3. Headnodes, a feldolgozó csomópontok és a zookeeper csomópontok listáját kell megjelennie. A headnodes rendelkezik a **hn*** előtag. Kattintson az első headnode.

    ![Headnode IP-cím keresése](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/cluster-headnodes.png)
4. Megnyílik, az oldal alján a a **összegzés** mezőbe másolja át az IP-címe a headnode és az állomás neve.

    ![Headnode IP-cím keresése](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/headnode-ip-address.png)
5. Az IP-cím és a headnode állomásneve a **állomások** fájlt a számítógépen, ahonnan szeretné futtatni, és távolról a a Spark feladatok hibakeresési. Ez lehetővé teszi az IP-címet, valamint az állomásnevet használja headnode folytatott kommunikációhoz.

   1. Nyissa meg a Jegyzettömbben emelt szintű engedélyekkel. A Fájl menüben kattintson a **nyitott** , majd lépjen a hosts fájl helyét. A Windows-számítógépen van `C:\Windows\System32\Drivers\etc\hosts`.
   2. Adja hozzá a következőt a **állomások** fájlt.

           # For headnode0
           192.xxx.xx.xx hn0-nitinp
           192.xxx.xx.xx hn0-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net

           # For headnode1
           192.xxx.xx.xx hn1-nitinp
           192.xxx.xx.xx hn1-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net
6. A számítógépre, hogy csatlakozott az Azure-beli virtuális hálózathoz, a HDInsight-fürt által használt győződjön meg arról, hogy pingelhető-e mind a headnodes IP-címét, valamint az állomásnevet használja.
7. SSH-ból a következő utasításokat követve fürt headnode [egy HDInsight-fürthöz SSH használatával csatlakozhat](hdinsight-hadoop-linux-use-ssh-unix.md). A fürt headnode Pingelje meg az asztali számítógép IP-címét. Az IP-címe. a számítógép, a hálózati kapcsolat, a másik az Azure virtuális hálózat, amely a számítógép csatlakozik egy rendelt kapcsolat kell tesztelni.
8. Ismételje meg a lépést a többi headnode is.

## <a name="step-4-create-a-spark-scala-application-using-the-hdinsight-tools-in-azure-toolkit-for-intellij-and-configure-it-for-remote-debugging"></a>4. lépés: A HDInsight Tools használatával az Azure-eszközkészlet az IntelliJ Spark Scala-alkalmazás létrehozása, és konfigurálja a távoli hibakeresés
1. Indítsa el az IntelliJ IDEA, és hozzon létre egy új projektet. Az új projekt párbeszédpanel a következők közül választhat, és kattintson a **következő**.

    ![Spark Scala-alkalmazás létrehozása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-hdi-scala-app.png)

   * A bal oldali ablaktáblán válassza ki a **HDInsight**.
   * A jobb oldali ablaktáblában jelölje ki **a Spark on HDInsight (Scala)**.
   * Kattintson a **Tovább** gombra.
2. A következő ablakban adja meg a következő projekt részleteit, és kattintson **Befejezés**.  
   - Adja meg a projekt nevét és a projekt helyére.
   - A **projekt SDK**, használja a Java 1.8 spark 2.x fürthöz, a spark-fürt 1.x 1.7 Java.
   - A **Spark verzió**, Scala-projekt létrehozása varázsló Spark SDK és Scala SDK integrálja a megfelelő verzióját. Ha a spark-fürt verziószáma alacsonyabb 2.0, válassza a spark 1.x. Ellenkező esetben spark2.x kell választania. A példa Spark2.0.2 (Scala 2.11.8).
       ![Spark Scala-alkalmazás létrehozása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-scala-project-details.png)
  
3. A Spark-projekt automatikusan összetevő hozza létre. Az összetevő megtekintéséhez kövesse az alábbi lépéseket.

   1. Az a **fájl** menüben kattintson a **szerkezetének**.
   2. Az a **szerkezetének** párbeszédpanel, kattintson a **összetevők** létrehozott alapértelmezett összetevő megjelenítéséhez.
   ![Hozzon létre JAR](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/default-artifact.png)

      Is létrehozhat saját összetevő bly kattint a  **+**  ikonra, a fenti kép kiemelve.

4. Szalagtárak hozzáadása a projekthez. A szalagtár hozzáadásához kattintson a jobb gombbal a projekt nevét a projekt csomópontjára, és kattintson **beállítások modul megnyitása**. Az a **szerkezetének** párbeszédpanel bal oldali ablaktáblában kattintson a **szalagtárak**, kattintson a (+) szimbólum, és kattintson a **a Maven**.

    ![Könyvtár hozzáadása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/add-library.png)

    Az a **letöltése könyvtár Maven tárházból** párbeszédpanel mezőben keressen, és adja hozzá az alábbi kódtárak.

   * `org.scalatest:scalatest_2.10:2.2.1`
   * `org.apache.hadoop:hadoop-azure:2.7.1`
5. Másolás `yarn-site.xml` és `core-site.xml` a a fürt headnode és adja hozzá a projekthez. A következő parancsok segítségével másolja a fájlokat. Használhat [Cygwin](https://cygwin.com/install.html) futtatásához a következő `scp` a fájlok másolását a fürt headnodes parancsok.

        scp <ssh user name>@<headnode IP address or host name>://etc/hadoop/conf/core-site.xml .

    Mivel azt már adja meg a fürt headnode IP-cím és állomásnevekkel fő az állomások fájlba az asztalon, használhatjuk a **scp** parancsok a következő módon.

        scp sshuser@hn0-nitinp:/etc/hadoop/conf/core-site.xml .
        scp sshuser@hn0-nitinp:/etc/hadoop/conf/yarn-site.xml .

    Ezek a fájlok hozzáadása a projekthez a másolásával a **/src** mappa a projekt csomópontjára, például `<your project directory>\src`.
6. Frissítés a `core-site.xml` a következő módosításokat.

   1. `core-site.xml`a tárfiókhoz a fürthöz tartozó titkosított kulcsot tartalmaz. Az a `core-site.xml` , hogy hozzáadta a projekthez, a titkosított kulcs cserélje le a tényleges kulcsot társított az alapértelmezett tárfiók. Lásd: [a tárelérési kulcsok kezelése](../storage/common/storage-create-storage-account.md#manage-your-storage-account).

           <property>
                 <name>fs.azure.account.key.hdistoragecentral.blob.core.windows.net</name>
                 <value>access-key-associated-with-the-account</value>
           </property>
   2. Távolítsa el a következő bejegyzéseket a `core-site.xml`.

           <property>
                 <name>fs.azure.account.keyprovider.hdistoragecentral.blob.core.windows.net</name>
                 <value>org.apache.hadoop.fs.azure.ShellDecryptionKeyProvider</value>
           </property>

           <property>
                 <name>fs.azure.shellkeyprovider.script</name>
                 <value>/usr/lib/python2.7/dist-packages/hdinsight_common/decrypt.sh</value>
           </property>

           <property>
                 <name>net.topology.script.file.name</name>
                 <value>/etc/hadoop/conf/topology_script.py</value>
           </property>
   3. Mentse a fájlt.
7. Vegye fel a fő osztály az alkalmazáshoz. Az a **Project Explorer**, kattintson a jobb gombbal **src**, mutasson a **új**, és kattintson a **Scala osztály**.

    ![Forráskód hozzáadása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-spark-scala-code.png)
8. Az a **új Scala osztály létrehozása** párbeszédpanelen adja meg egy nevet, **jellegű** kiválasztása **objektum**, és kattintson a **OK**.

    ![Forráskód hozzáadása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-spark-scala-code-object.png)
9. Az a `MyClusterAppMain.scala` fájlt, az alábbi kódot. Ezt a kódot hoz létre a Spark, a környezetben, és elindítja egy `executeJob` metódust a `SparkSample` objektum.

        import org.apache.spark.{SparkConf, SparkContext}

        object SparkSampleMain {
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("SparkSample")
                                      .set("spark.hadoop.validateOutputSpecs", "false")
            val sc = new SparkContext(conf)

            SparkSample.executeJob(sc,
                                   "wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv",
                                   "wasb:///HVACOut")
          }
        }

10. Ismételje meg a 8. és 9 nevű új Scala objektum a fenti `SparkSample`. Ez az osztály fel a következő kódot. Ezt a kódot az adatokat olvas a HVAC.csv (az összes HDInsight Spark-fürtjei elérhető), lekéri a sor csak egy számot a fürt megosztott kötetei szolgáltatás hetedik oszlopban, és ír a kimeneti **/HVACOut** alatt az alapértelmezett tároló a fürthöz.

        import org.apache.spark.SparkContext

        object SparkSample {
         def executeJob (sc: SparkContext, input: String, output: String): Unit = {
           val rdd = sc.textFile(input)

           //find the rows which have only one digit in the 7th column in the CSV
           val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)

           val s = sc.parallelize(rdd.take(5)).cartesian(rdd).count()
           println(s)

           rdd1.saveAsTextFile(output)
           //rdd1.collect().foreach(println)
         }
        }
11. Ismételje meg a 8. és 9 a fenti adjon hozzá egy új osztályt `RemoteClusterDebugging`. Ez az osztály a Spark keretrendszeréhez alkalmazások hibakereséshez használt valósítja meg. Adja hozzá a következő kódot a `RemoteClusterDebugging` osztály.

        import org.apache.spark.{SparkConf, SparkContext}
        import org.scalatest.FunSuite

        class RemoteClusterDebugging extends FunSuite {

         test("Remote run") {
           val conf = new SparkConf().setAppName("SparkSample")
                                     .setMaster("yarn-client")
                                     .set("spark.yarn.am.extraJavaOptions", "-Dhdp.version=2.4")
                                     .set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")
                                     .setJars(Seq("""C:\workspace\IdeaProjects\MyClusterApp\out\artifacts\MyClusterApp_DefaultArtifact\default_artifact.jar"""))
                                     .set("spark.hadoop.validateOutputSpecs", "false")
           val sc = new SparkContext(conf)

           SparkSample.executeJob(sc,
             "wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv",
             "wasb:///HVACOut")
         }
        }

     Néhány fontos itt ügyeljen a következőkre:

   * A `.set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")`, ellenőrizze, hogy a külső szerelvény JAR érhető el a fürttároló, a megadott elérési úton.
   * A `setJars`, adja meg a helyet, ahol a összetevő jar létrejön. Általában akkor `<Your IntelliJ project directory>\out\<project name>_DefaultArtifact\default_artifact.jar`.
12. Az a `RemoteClusterDebugging` osztály, kattintson a jobb gombbal a `test` kulcsszót, és válassza **létrehozása RemoteClusterDebugging konfigurációs**.

    ![A távoli konfiguráció létrehozása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-remote-config.png)

13. A párbeszédpanelen adja meg a konfiguráció nevét, és válassza a **jellegű tesztelése** , **teszt neve**. Minden más értéket alapértelmezett hagyja, kattintson a **alkalmaz**, és kattintson a **OK**.

    ![A távoli konfiguráció létrehozása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/provide-config-value.png)
14. Ekkor megjelenik egy **távoli Futtatás** konfigurációs legördülő a menüsávon.

    ![A távoli konfiguráció létrehozása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/config-run.png)

## <a name="step-5-run-the-application-in-debug-mode"></a>5. lépés: Futtassa az alkalmazást hibakeresési módban
1. Az IntelliJ IDEA projektben nyissa meg `SparkSample.scala` , és hozzon létre egy töréspontot mellett látható "val rdd1". A töréspont létrehozásához, a megjelenő menüben válassza ki a **függvény executeJob sor**.

    ![Töréspont](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-breakpoint.png)
2. Kattintson a **Debug futtassa** megjelenítő gombra a **távoli Futtatás** konfigurációs legördülő futtatni az alkalmazást.

    ![A program futtatása a hibakeresési módban](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-run-mode.png)
3. Amikor a program végrehajtását eléri a töréspont, megjelenik egy **hibakereső** fülre az alsó ablaktáblán.

    ![A program futtatása a hibakeresési módban](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch.png)
4. Kattintson a (**+**) ikonra kattintva vegye fel a figyelés, az alábbi ábrán látható módon.

    ![A program futtatása a hibakeresési módban](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch-variable.png)

    Itt, mert a kérelem túllépte a változó előtt `rdd1` lett létrehozva, Mik a változó első 5 sora láthatja a figyelési `rdd`. Nyomja le az **ENTER** billentyűt.

    ![A program futtatása a hibakeresési módban](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch-variable-value.png)

    A fenti kép láthatók az, hogy futásidőben, akkor lekérdezhet terrabytes adatok és a hibakeresési módját az alkalmazás időtartamára. Például a kimenetben, az ábrán látható, láthatja, hogy a kimeneti első sora-e egy fejlécet. Ennek alapján módosíthatja az alkalmazás kódjában a fejlécsor kihagyhatja, ha szükséges.
5. Most kattintson a **folytatása Program** ikon gombra az alkalmazás futtatásához.

    ![A program futtatása a hibakeresési módban](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-continue-run.png)
6. Ha az alkalmazás sikeresen befejeződött, a következőhöz hasonló kimenetnek kell megjelennie.

    ![A program futtatása a hibakeresési módban](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-complete.png)

## <a name="seealso"></a>Lásd még:
* [Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)](hdinsight-apache-spark-overview.md)

### <a name="demo"></a>Bemutató
* (Videó) Scala-projekt létrehozása: [Spark Scala-alkalmazások létrehozása](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Távoli hibakeresési (videó): [IntelliJ a Spark-alkalmazások távolról a HDInsight-fürt használata Azure eszköztára](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

### <a name="scenarios"></a>Forgatókönyvek
* [Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel](hdinsight-apache-spark-use-bi-tools.md)
* [Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark és Machine Learning: A Spark on HDInsight használata az élelmiszervizsgálati eredmények előrejelzésére](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására](hdinsight-apache-spark-eventhub-streaming.md)
* [A webhelynapló elemzése a Spark on HDInsight használatával](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Alkalmazások létrehozása és futtatása
* [Önálló alkalmazás létrehozása a Scala használatával](hdinsight-apache-spark-create-standalone-application.md)
* [Feladatok távoli futtatása Spark-fürtön a Livy használatával](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Eszközök és bővítmények
* [Az intellij-t Azure eszköztára a HDInsight Tools használatával hozzon létre, és küldje el a Spark Scala applicatons](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Az intellij-t Azure eszközkészlet segítségével SSH keresztül távolról Spark-alkalmazások](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Használja a HDInsight Tools for IntelliJ a Hortonworks védőfal](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Az Eclipse Azure eszközkészlet a HDInsight Tools használatával Spark-alkalmazások létrehozása](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Zeppelin notebookok használata Spark-fürttel HDInsighton](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Külső csomagok használata Jupyter notebookokkal](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [A Jupyter telepítése a számítógépre, majd csatlakozás egy HDInsight Spark-fürthöz](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Erőforrások kezelése
* [Apache Spark-fürt erőforrásainak kezelése az Azure HDInsightban](hdinsight-apache-spark-resource-manager.md)
* [Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban](hdinsight-apache-spark-job-debugging.md)
