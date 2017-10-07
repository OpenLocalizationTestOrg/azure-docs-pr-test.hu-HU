---
title: "az IntelliJ - hibakeresési alkalmazások távolról a HDInsight Spark eszköztára aaaAzure |} Microsoft Docs"
description: "Megtudhatja, hogyan használhat HDInsight eszközöket az Azure-eszközkészlet IntelliJ HDInsight Spark-fürtjei VPN-en keresztül futó tooremotely hibakeresési alkalmazás."
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
ms.openlocfilehash: ad67d23bf609d0a7afb38b2acb110062f8b27b39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-toolkit-for-intellij-toodebug-applications-remotely-on-hdinsight-spark-through-vpn"></a>Az IntelliJ toodebug alkalmazások távolról a VPN-en keresztül a HDInsight Spark Azure eszközkészlet használata

Azt javasoljuk, hogy a hibakeresési távolról a spark applicaltion ssh hello módon. Útmutatásért lásd: [távolról az IntelliJ SSH-n keresztül a Azure eszközkészlet a HDInsight-fürtök a Spark-alkalmazások hibakeresését](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).

Ez a cikk lépésenként bemutató biztosítanak toouse hello Azure eszközkészlet a HDInsight Tools IntelliJ toosubmit HDInsight Spark-fürt Spark feladatot és majd debug távolról, az asztali számítógépről. toodo Igen, hello magas szintű lépéseket kell végrehajtania:

1. Pont-pont vagy a pont-pont Azure virtuális hálózat létrehozása. Ez a dokumentum hello lépések azt feltételezik, hogy a pont-pont hálózati használható.
2. Hello-helyek Azure-beli virtuális hálózat részét képező Azure hdinsight Spark-fürt létrehozása.
3. Ellenőrizze a hello kapcsolatát hello fürt headnode és az asztal között.
4. Az IntelliJ IDEA Scala-alkalmazás létrehozása, és konfigurálja a távoli hibakereséshez.
5. Futtassa, és hello alkalmazás hibakeresése.

## <a name="prerequisites"></a>Előfeltételek
* Azure-előfizetés. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* A HDInsight az Apache Spark-fürt. Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).
* Oracle Java fejlesztői készlet. A későbbiekben telepítheti az [Itt](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* IntelliJ IDEA. Ez a cikk 2017.1 verzióját használja. A későbbiekben telepítheti az [Itt](https://www.jetbrains.com/idea/download/).
* Az Azure eszköztára IntelliJ HDInsight eszközök. A HDInsight tools for IntelliJ elérhetők hello Azure eszköztára IntelliJ részeként. Hogyan tooinstall hello Azure eszközkészlet, lásd: [telepítése hello Azure eszköztára IntelliJ](../azure-toolkit-for-intellij-installation.md).
* Jelentkezzen be az Azure-előfizetéshez az IntelliJ IDEA. Útmutatás alapján hello [Itt](hdinsight-apache-spark-intellij-tool-plugin.md).
* Futtatásakor a Spark Scala-alkalmazások a távoli hibakereséshez a Windows rendszerű számítógépeken, előfordulhat, hogy beolvasása kivételt, a [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356) , amely akkor fordul elő, miatt tooa hiányzó WinUtils.exe Windows rendszeren. Ezt a hibát körül toowork, kell [végrehajtható hello letölthető innen](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) tooa helyre, például **C:\WinUtils\bin**. Majd adjon hozzá egy környezeti változó **HADOOP_HOME** és hello hello változó értékének túl**C\WinUtils**.

## <a name="step-1-create-an-azure-virtual-network"></a>1. lépés: Az Azure virtuális hálózat létrehozása
Hello utasításokat kövesse az alábbi hivatkozások toocreate egy Azure virtuális hálózatra hello, és ellenőrizze a hello asztal és az Azure Virtual Network hello összekapcsolását.

* [VNet létrehozása az Azure portál használatával pont-pont VPN-kapcsolattal](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [VNet létrehozása a PowerShell használatával pont-pont VPN-kapcsolattal](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
* [Egy pont – hely kapcsolat tooa virtuális hálózatnak a PowerShell használatával](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

## <a name="step-2-create-an-hdinsight-spark-cluster"></a>2. lépés: Egy HDInsight Spark-fürt létrehozása
Apache Spark-fürt hello létrehozott Azure virtuális hálózat részét képező Azure hdinsight is készítsen. Használja a rendelkezésre álló hello információ [hdinsight létrehozása Linux-alapú fürtökön](hdinsight-hadoop-provision-linux-clusters.md). Választható konfiguráció részeként hello hello előző lépésben létrehozott Azure virtuális hálózat kiválasztása

## <a name="step-3-verify-hello-connectivity-between-hello-cluster-headnode-and-your-desktop"></a>3. lépés: Hello fürt headnode és az asztal hello összekapcsolását ellenőrzése
1. Első hello headnode hello IP-címét. Ambari felhasználói felületének megnyitásához hello fürthöz. Hello-fürt panelén kattintson **irányítópult**.

    ![Headnode IP-cím keresése](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/launch-ambari-ui.png)
2. A hello Ambari felhasználói felületén, a hello jobb felső sarokban, kattintson az **állomások**.

    ![Headnode IP-cím keresése](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/ambari-hosts.png)
3. Headnodes, a feldolgozó csomópontok és a zookeeper csomópontok listáját kell megjelennie. hello headnodes rendelkezik hello **hn*** előtag. Kattintson az első headnode hello.

    ![Headnode IP-cím keresése](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/cluster-headnodes.png)
4. Hello lap nyílik meg, hello hello alján **összegzés** másolási hello IP-címe hello headnode és hello állomás neve mezőben.

    ![Headnode IP-cím keresése](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/headnode-ip-address.png)
5. Hello IP-cím és hello állomásnevét hello headnode toohello **állomások** hello számítógépen, amelyen szeretné, hogy toorun, és távolról debug hello Spark feladatok a fájlt. Ez lehetővé teszi az toocommunicate a hello headnode hello IP-címet, valamint a hello állomásnév használatával.

   1. Nyissa meg a Jegyzettömbben emelt szintű engedélyekkel. Hello fájl menüben kattintson a **nyitott** , majd lépjen a toohello hello hosts fájl helyét. A Windows-számítógépen van `C:\Windows\System32\Drivers\etc\hosts`.
   2. Adja hozzá a következő toohello hello **állomások** fájlt.

           # For headnode0
           192.xxx.xx.xx hn0-nitinp
           192.xxx.xx.xx hn0-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net

           # For headnode1
           192.xxx.xx.xx hn1-nitinp
           192.xxx.xx.xx hn1-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net
6. Számítógépről hello toohello hello HDInsight-fürt által használt Azure-beli virtuális hálózatra csatlakozó győződjön meg arról, hogy pingelhető mindkét hello headnodes hello IP-címet, valamint a hello állomásnév használatával.
7. SSH-ból: hello utasításokat követve hello fürt headnode [Connect tooan HDInsight-fürtjéhez SSH](hdinsight-hadoop-linux-use-ssh-unix.md). Hello fürt headnode, a ping hello asztali számítógép hello IP-címét. Kapcsolat tooboth hello rendelt IP-címekre toohello számítógép, egy a hello hálózati kapcsolatot tesztelje, és az Azure Virtual Network számítógép hello hello hello csatlakozik-e.
8. Ismételje hello hello más headnode is.

## <a name="step-4-create-a-spark-scala-application-using-hello-hdinsight-tools-in-azure-toolkit-for-intellij-and-configure-it-for-remote-debugging"></a>4. lépés: Hello HDInsight Tools for IntelliJ az Azure-eszközkészlet használatával Spark Scala-alkalmazás létrehozása, és konfigurálja a távoli hibakeresés
1. Indítsa el az IntelliJ IDEA, és hozzon létre egy új projektet. Hello új projekt párbeszédpanel, győződjön meg a következő lehetőségek hello, és kattintson a **következő**.

    ![Spark Scala-alkalmazás létrehozása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-hdi-scala-app.png)

   * Hello bal oldali ablaktáblában jelölje ki a **HDInsight**.
   * Hello jobb oldali ablaktáblában jelölje ki **a Spark on HDInsight (Scala)**.
   * Kattintson a **Tovább** gombra.
2. Hello következő ablakban adja meg a projekt részleteit a következő hello, és kattintson a **Befejezés**.  
   - Adja meg a projekt nevét és a projekt helyére.
   - A **projekt SDK**, használja a Java 1.8 spark 2.x fürthöz, a spark-fürt 1.x 1.7 Java.
   - A **Spark verzió**, Scala-projekt létrehozása varázsló Spark SDK és Scala SDK integrálja a megfelelő verzióját. Ha hello spark-fürt verziószáma alacsonyabb 2.0, válassza a spark 1.x. Ellenkező esetben spark2.x kell választania. A példa Spark2.0.2 (Scala 2.11.8).
       ![Spark Scala-alkalmazás létrehozása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-scala-project-details.png)
  
3. hello Spark projekt automatikusan összetevő hozza létre. toosee hello összetevő, kövesse az alábbi lépéseket.

   1. A hello **fájl** menüben kattintson a **szerkezetének**.
   2. A hello **szerkezetének** párbeszédpanel, kattintson a **összetevők** toosee hello alapértelmezett összetevő, amely jön létre.
   ![Hozzon létre JAR](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/default-artifact.png)

      Is létrehozhat saját összetevő hello kattintva bly  **+**  ikonra, a fenti kép hello kiemelve.

4. Szalagtárak tooyour projekt hozzáadása. a szalagtár tooadd kattintson a jobb gombbal a hello projekt neve hello projekt elemére, és kattintson **beállítások modul megnyitása**. A hello **szerkezetének** párbeszédpanelen hello bal oldali ablaktáblában kattintson a **szalagtárak**, kattintson a szimbólum hello (+), majd **a Maven**.

    ![Könyvtár hozzáadása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/add-library.png)

    A hello **letöltése könyvtár Maven tárházból** párbeszédpanel mezőben keressen, és adja hozzá a következő könyvtárak hello.

   * `org.scalatest:scalatest_2.10:2.2.1`
   * `org.apache.hadoop:hadoop-azure:2.7.1`
5. Másolás `yarn-site.xml` és `core-site.xml` hello a fürt headnode és toohello projekt hozzáadása. Használja a következő parancsok toocopy hello fájlok hello. Használhat [Cygwin](https://cygwin.com/install.html) toorun hello következő `scp` toocopy hello fájlok hello fürt headnodes parancsokat.

        scp <ssh user name>@<headnode IP address or host name>://etc/hadoop/conf/core-site.xml .

    Már hozzáadott hello fürt headnode IP cím és állomásnevekkel fő hello gazdafájl hello asztali, mert hello használhatjuk **scp** hello módon a következő parancsokat.

        scp sshuser@hn0-nitinp:/etc/hadoop/conf/core-site.xml .
        scp sshuser@hn0-nitinp:/etc/hadoop/conf/yarn-site.xml .

    Ezen fájlok tooyour projekt hozzáadása a hello másolásával **/src** mappa a projekt csomópontjára, például `<your project directory>\src`.
6. Frissítés hello `core-site.xml` toomake hello a következő módosításokat.

   1. `core-site.xml`hello titkosított kulcs toohello tárfiók hello-fürthöz tartozó tartalmazza. A hello `core-site.xml` toohello projekt, csere hello titkosított kulcs hello tényleges biztonságitár-kulcs társított hello alapértelmezett tárfiók hozzáadásának. Lásd: [a tárelérési kulcsok kezelése](../storage/common/storage-create-storage-account.md#manage-your-storage-account).

           <property>
                 <name>fs.azure.account.key.hdistoragecentral.blob.core.windows.net</name>
                 <value>access-key-associated-with-the-account</value>
           </property>
   2. Távolítsa el a bejegyzések követően – hello hello `core-site.xml`.

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
   3. Hello fájl mentéséhez.
7. Adja hozzá az alkalmazás hello fő osztály. A hello **Project Explorer**, kattintson a jobb gombbal **src**, pont túl**új**, és kattintson a **Scala osztály**.

    ![Forráskód hozzáadása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-spark-scala-code.png)
8. A hello **új Scala osztály létrehozása** párbeszédpanelen adja meg egy nevet, **jellegű** kiválasztása **objektum**, és kattintson a **OK**.

    ![Forráskód hozzáadása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-spark-scala-code-object.png)
9. A hello `MyClusterAppMain.scala` fájlt, illessze be a kódját a következő hello. Ez a kód hello Spark-környezetet hoz létre, és elindítja egy `executeJob` hello metódusnak `SparkSample` objektum.

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

10. Ismételje meg a 8. és 9 fent egy új Scala objektum nevű tooadd `SparkSample`. toothis osztály adja hozzá a következő kód hello. Ez a kód hello adatokat olvas hello HVAC.csv (az összes HDInsight Spark-fürtjei elérhető), hello tartalmazó sorok csak egy számjegy hello CSV hetedik oszlopban hello lekéri és hello kimenetet túl írja**/HVACOut** hello alapértelmezés szerint a tároló hello fürthöz.

        import org.apache.spark.SparkContext

        object SparkSample {
         def executeJob (sc: SparkContext, input: String, output: String): Unit = {
           val rdd = sc.textFile(input)

           //find hello rows which have only one digit in hello 7th column in hello CSV
           val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)

           val s = sc.parallelize(rdd.take(5)).cartesian(rdd).count()
           println(s)

           rdd1.saveAsTextFile(output)
           //rdd1.collect().foreach(println)
         }
        }
11. Ismételje meg a 8. és 9 fent egy új osztályt nevű tooadd `RemoteClusterDebugging`. Ez az osztály hello Spark keretrendszeréhez alkalmazások hibakereséshez használt valósítja meg. Adja hozzá a következő kód toohello hello `RemoteClusterDebugging` osztály.

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

     Néhány fontos dolgot toonote itt:

   * A `.set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")`, győződjön meg arról, Spark szerelvény JAR hello hello fürttároló hello megadott elérési úton található.
   * A `setJars`, ahol létrejön hello összetevő jar hello helyének megadása. Általában akkor `<Your IntelliJ project directory>\out\<project name>_DefaultArtifact\default_artifact.jar`.
12. A hello `RemoteClusterDebugging` osztály, kattintson a jobb gombbal a hello `test` kulcsszót, és válassza **létrehozása RemoteClusterDebugging konfigurációs**.

    ![A távoli konfiguráció létrehozása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-remote-config.png)

13. A hello párbeszédpanelen adjon meg egy nevet hello konfigurációs, és válassza a hello **jellegű tesztelése** , **teszt neve**. Minden más értéket alapértelmezett hagyja, kattintson a **alkalmaz**, és kattintson a **OK**.

    ![A távoli konfiguráció létrehozása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/provide-config-value.png)
14. Ekkor megjelenik egy **távoli Futtatás** konfigurációs hello menüsávon legördülő.

    ![A távoli konfiguráció létrehozása](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/config-run.png)

## <a name="step-5-run-hello-application-in-debug-mode"></a>5. lépés: A hibakeresési módban hello alkalmazás futtatása
1. Az IntelliJ IDEA projektben nyissa meg `SparkSample.scala` , és hozzon létre egy töréspontot következő too'val rdd1 ". Hello előugró menüben töréspont létrehozásához válasszon ki **függvény executeJob sor**.

    ![Töréspont](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-breakpoint.png)
2. Hello kattintson **Debug futtatása** gomb következő toohello **távoli Futtatás** konfigurációs legördülő toostart hello alkalmazást futtat.

    ![Hello programfuttatást hibakeresési módban](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-run-mode.png)
3. Hello töréspont hello program végrehajtását elérésekor kell megjelennie a **hibakereső** hello alsó lapját.

    ![Hello programfuttatást hibakeresési módban](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch.png)
4. Kattintson a hello (**+**) ikonra tooadd a figyelés, az alábbi hello ábrán látható módon.

    ![Hello programfuttatást hibakeresési módban](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch-variable.png)

    Itt, mert hello alkalmazás túllépte a hello változó előtt `rdd1` lett létrehozva, mi van hello hello változóban első 5 sorok láthatja a figyelési `rdd`. Nyomja le az **ENTER** billentyűt.

    ![Hello programfuttatást hibakeresési módban](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch-variable-value.png)

    A fenti kép hello lásd az, hogy futásidőben, akkor lekérdezhet terrabytes adatok és a hibakeresési módját az alkalmazás időtartamára. Például a hello kimeneti fenti hello ábrán látható, megtekintheti, hogy hello első sorát hello kimeneti fejléc. Ennek alapján módosíthatja az alkalmazás kódja tooskip hello fejlécsor szükség esetén.
5. Most kattintson hello **folytatása Program** ikon tooproceed futtassa az alkalmazással.

    ![Hello programfuttatást hibakeresési módban](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-continue-run.png)
6. Ha hello alkalmazás sikeresen befejeződött, hello hasonló kimenetnek kell megjelennie.

    ![Hello programfuttatást hibakeresési módban](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-complete.png)

## <a name="seealso"></a>Lásd még:
* [Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)](hdinsight-apache-spark-overview.md)

### <a name="demo"></a>Bemutató
* (Videó) Scala-projekt létrehozása: [Spark Scala-alkalmazások létrehozása](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Távoli hibakeresési (videó): [IntelliJ toodebug Spark-alkalmazások távolról a HDInsight-fürt használata Azure eszköztára](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

### <a name="scenarios"></a>Forgatókönyvek
* [Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel](hdinsight-apache-spark-use-bi-tools.md)
* [Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására](hdinsight-apache-spark-eventhub-streaming.md)
* [A webhelynapló elemzése a Spark on HDInsight használatával](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Alkalmazások létrehozása és futtatása
* [Önálló alkalmazás létrehozása a Scala használatával](hdinsight-apache-spark-create-standalone-application.md)
* [Feladatok távoli futtatása Spark-fürtön a Livy használatával](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Eszközök és bővítmények
* [Használata a HDInsight Tools Azure eszközkészlet IntelliJ toocreate, és küldje el a Spark Scala applicatons](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Az IntelliJ toodebug Spark-alkalmazások SSH keresztül távolról használható Azure eszköztára](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Használja a HDInsight Tools for IntelliJ a Hortonworks védőfal](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [HDInsight-eszközök használata az Eclipse toocreate Spark-alkalmazások Azure eszköztára](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Zeppelin notebookok használata Spark-fürttel HDInsighton](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Külső csomagok használata Jupyter notebookokkal](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter telepítse a számítógépre, és csatlakozzon a HDInsight Spark-fürt tooan](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Erőforrások kezelése
* [Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése](hdinsight-apache-spark-resource-manager.md)
* [Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban](hdinsight-apache-spark-job-debugging.md)
