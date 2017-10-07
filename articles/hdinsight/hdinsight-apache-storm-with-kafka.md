---
title: "Apache Kafka futó Storm - Azure aaaUse |} Microsoft Docs"
description: "Apache Kafka HDInsight alatt futó Apache Storm együtt települ. Ismerje meg, hogyan toowrite tooKafka, és azt, majd olvassa használatával hello Storm KafkaBolt és KafkaSpout összetevői. Megtudhatja, hogyan toouse hello fluxus keretrendszer toodefine, és küldje el a Storm-topológiák."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: e4941329-1580-4cd8-b82e-a2258802c1a7
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/21/2017
ms.author: larryfr
ms.openlocfilehash: 95701f51dfdf6f1a859dcde96d7053df4f21701f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-apache-kafka-preview-with-storm-on-hdinsight"></a>Apache Kafka (előzetes verzió) használata a HDInsight alatt futó Storm

Megtudhatja, hogyan toouse alatt futó Apache Storm tooread az írási és olvasási tooApache Kafka. Ez a példa is mutatja be, hogyan toosave adatait egy Storm-topológia toohello HDFS-kompatibilis fájl használt HDInsight rendszert.

> [!NOTE]
> hello jelen dokumentumban leírt lépések, amely a HDInsight alatt futó Storm, mind a HDInsight-fürt Kafka tartalmazza az Azure erőforrás-csoport létrehozása. Ezeken a fürtökön hello Kafka fürt kommunikálnak mindkét egy Azure virtuális hálózatot, amely lehetővé teszi a Storm-fürt toodirectly hello belül található.
> 
> Amikor elkészült, a jelen dokumentumban leírt lépések hello, ne felejtse el toodelete hello fürtök tooavoid felesleges költségek.

## <a name="get-hello-code"></a>Hello kód beolvasása

Ez a dokumentum hello példában hello kódját érhető el: [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka).

toocompile ebben a projektben van szüksége a fejlesztési környezet konfigurációját a következő hello:

* [Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) vagy újabb verzióját. HDInsight 3.5-ös vagy újabb rendszer szükséges Java 8.

* [Maven 3.x](https://maven.apache.org/download.cgi)

* Egy SSH-ügyfél (hello kell `ssh` és `scp` parancsok) - információkért lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

* Egy szövegszerkesztőben, vagy IDE.

hello következő környezeti változó lehet beállítani a fejlesztő munkaállomás Java és hello JDK telepítésekor. Azonban ellenőrizni kell, hogy léteznek, illetve hello a rendszer tartozó helyes értékeket tartalmaz.

* `JAVA_HOME`-toohello rendszert tartalmazó könyvtár hello JDK kell mutatnia.
* `PATH`-a következő elérési utak hello tartalmaznia kell:
  
    * `JAVA_HOME`(vagy ezzel egyenértékű elérési hello).
    * `JAVA_HOME\bin`(vagy ezzel egyenértékű elérési hello).
    * hello mappát, ahová a Maven telepítve van.

## <a name="create-hello-clusters"></a>Hello fürtök létrehozása

A HDInsight Apache Kafka nem biztosít hozzáférést toohello Kafka brókerek képest hello a nyilvános internethez. Kafka fürt hello, amelyeket tooKafka kell lennie a megbeszélések hello hello csomópontként azonos Azure virtuális hálózatban. Ehhez a példához hello Kafka és a Storm-fürtök találhatók egy Azure virtuális hálózatra. a következő ábra azt mutatja be, hogyan kommunikációs hello fürtök között zajló kommunikációról hello:

![A Storm és Kafka fürtök egy Azure virtuális hálózati diagramja](./media/hdinsight-apache-storm-with-kafka/storm-kafka-vnet.png)

> [!NOTE]
> Egyéb szolgáltatások hello fürtön például az SSH és az Ambari keresztül is elérhető hello internet. Nyilvános és a HDInsight együttes rendelkezésre álló hello-portok további információkért lásd: [portok és a HDInsight által használt URI-azonosítók](hdinsight-hadoop-port-settings-for-services.md).

Létrehozhat egy Azure virtuális hálózatra, Kafka, és a Storm-fürtök manuális, akkor könnyebb toouse Azure Resource Manager-sablonok. Használjon hello alábbi lépéseit toodeploy egy Azure virtuális hálózatra, Kafka, és Storm-fürtök tooyour Azure-előfizetés.

1. A tooAzure gomb toosign és hello Azure-portál megnyitása hello sablon hello használata.
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-storm-cluster-in-vnet-v2.json" target="_blank"><img src="./media/hdinsight-apache-storm-with-kafka/deploy-to-azure.png" alt="Deploy tooAzure"></a>
   
    hello Azure Resource Manager sablon itt található: **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-storm-cluster-in-vnet-v1.json**. Létrehozza a következő erőforrások hello:
    
    * Azure-erőforráscsoportot
    * Azure Virtual Network
    * Azure Storage-fiók
    * A HDInsight 3.6 (három munkavégző csomópontokhoz) verzió Kafka
    * Verzió (három munkavégző csomópontokhoz) 3.6 HDInsight alatt futó Storm

  > [!WARNING]
  > a HDInsight Kafka tooguarantee rendelkezésre állását, a fürt tartalmaznia kell legalább három munkavégző csomópontokhoz. Ez a sablon a három munkavégző csomópontokhoz tartalmaz Kafka fürtöt hoz létre.

2. Útmutatás toopopulate hello bejegyzések követően hello használata hello **egyéni telepítési** panel:
   
    ![HDInsight egyéni központi telepítés](./media/hdinsight-apache-storm-with-kafka/parameters.png)

    * **Erőforráscsoport**: hozzon létre egy csoportot, vagy válasszon egy meglévőt. Ez a csoport hello HDInsight-fürtöt tartalmaz.
   
    * **Hely**: Válasszon egy helyet a földrajzi elhelyezkedés alapján Bezárás tooyou.

    * **Fürt neve kiinduló**: Ez az érték neveként hello alapszintű hello Storm és Kafka fürtök használható. Például írja be **hdi** hoz létre egy Storm-fürt nevű **storm-hdi** és nevű Kafka fürt **kafka-hdi**.
   
    * **A fürt bejelentkezési felhasználónevét**: hello rendszergazda felhasználóneve hello Storm és Kafka fürt.
   
    * **A fürt bejelentkezési jelszó**: hello rendszergazdai jelszóval hello Storm és Kafka fürt.
    
    * **SSH-felhasználónév**: hello SSH felhasználói toocreate hello Storm és Kafka fürt.
    
    * **SSH-jelszónak**: hello SSH felhasználó hello Storm és Kafka fürt hello jelszavát.

3. Olvasási hello **feltételek és kikötések**, majd válassza ki **toohello feltételek és kikötések fenti elfogadom**.

4. Végül ellenőrizze **PIN-kód toodashboard** majd **beszerzési**. Körülbelül 20 percet toocreate hello fürtök szükséges.

Létrehozása után a hello erőforrásokat, hello erőforráscsoport hello panel jelenik meg.

![Erőforráscsoport panel hello vnet és fürtök](./media/hdinsight-apache-storm-with-kafka/groupblade.png)

> [!IMPORTANT]
> Figyelje meg, hogy vannak-e a HDInsight-fürtök hello hello nevei **storm-BASENAME** és **kafka-BASENAME**, ahol BASENAME toohello sablon megadott hello nevét. Ezeket a neveket használja a későbbi lépésekben toohello fürtök kapcsolódáskor.

## <a name="understanding-hello-code"></a>Hello kód ismertetése

A projekt két topológiát tartalmaz:

* **KafkaWriter**: hello által meghatározott **writer.yaml** fájl, ez a topológia ír véletlenszerű mondat tooKafka hello alatt futó Apache Storm megadott KafkaBolt használatával.

    Ez a topológia használ egy egyéni **SentenceSpout** összetevő toogenerate véletlenszerű mondatokat.

* **KafkaReader**: hello által meghatározott **reader.yaml** fájl, ez a topológia olvassa be az adatokat Kafka hello alatt futó Apache Storm megadott KafkaSpout használatával, majd a naplók hello adatok toostdout.

    Ez a topológia hello Storm-fürt hello Storm HdfsBolt toowrite adatok toodefault tárolást használ.
### <a name="flux"></a>Fluxus

hello topológia meghatározása [fluxus](https://storm.apache.org/releases/1.1.0/flux.html). Fluxus bemutatott Storm 0.10.x, és lehetővé teszi a tooseparate hello topológiakonfiguráció hello kódból. Hello fluxus keretrendszert használó topológia esetén a hello topológia YAM fájlban van definiálva. hello YAM fájl hello topológia része lehet. Amikor hello topológia használt önálló fájl is lehet. Fluxus futásidőben, ebben a példában használt változók behelyettesítését is támogatja.

hello következő paraméterei vannak beállítva az alábbi topológiák futás közben:

* `${kafka.topic}`: hello Kafka témakör, amely hello topológiák olvasására vagy írására hello nevét.

* `${kafka.broker.hosts}`: hello üzemelteti, hogy hello Kafka brókerek futtatnak. hello broker információknak az alkalmazása által hello KafkaBolt tooKafka írása közben.

* `${kafka.zookeeper.hosts}`: hello gazdagépeken futó Zookeeper a hello Kafka fürt.

Fluxus topológiák további információkért lásd: [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).

## <a name="download-and-compile-hello-project"></a>Töltse le és hello projekt lefordítása

1. A fejlesztési környezet le hello projektet a [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka), nyisson meg egy parancssori és hello projekt letöltött könyvtárak toohello helyének módosításához.

2. A hello **hdinsight-storm-java-kafka** könyvtárába, használjon hello következő toocompile hello projekt parancsot, és központi telepítési csomagot hozhat létre:

  ```bash
  mvn clean package
  ```

    hello csomag folyamat létrehoz egy fájlt nevű `KafkaTopology-1.0-SNAPSHOT.jar` a hello `target` könyvtár.

3. Használja a következő parancsok toocopy hello csomag tooyour Storm on HDInsight-fürt hello. Cserélje le **felhasználónév** hello SSH felhasználónévvel hello fürthöz. Cserélje le **BASENAME** hello Alap nevű hello fürt létrehozásakor használt.

  ```bash
  scp ./target/KafkaTopology-1.0-SNAPSHOT.jar USERNAME@storm-BASENAME-ssh.azurehdinsight.net:KafkaTopology-1.0-SNAPSHOT.jar
  ```

    Amikor a rendszer kéri, adja meg a hello fürtök létrehozásakor használt hello jelszót.

## <a name="configure-hello-topology"></a>Hello topológia konfigurálása

1. A következő módszerek toodiscover hello valamelyikével Kafka broker állomások hello:

    ```powershell
    $creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"
    $clusterName = Read-Host -Prompt "Enter hello Kafka cluster name"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $brokerHosts = $respObj.host_components.HostRoles.host_name[0..1]
    ($brokerHosts -join ":9092,") + ":9092"
    ```

    ```bash
    curl -su admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER" | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2
    ```

    > [!IMPORTANT]
    > hello Bash példa feltételezi, hogy `$CLUSTERNAME` hello HDInsight-fürt hello nevét tartalmazza. Azt is feltételezi, hogy [jq](https://stedolan.github.io/jq/) telepítve van. Amikor a rendszer kéri, adja meg hello jelszó hello fürt bejelentkezési fiók.

    hello visszaadott érték a következő szöveg hasonló toohello:

        wn0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092,wn1-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092

    > [!IMPORTANT]
    > Előfordulhat, hogy a fürt legalább két broker gazdagépek, amíg nem kell tooprovide összes állomások tooclients teljes listáját. Egy vagy két is elegendő.

2. A következő módszerek toodiscover hello Kafka Zookeeper állomások hello egyikét használja:

    ```powershell
    $creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"
    $clusterName = Read-Host -Prompt "Enter hello Kafka cluster name"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $zookeeperHosts = $respObj.host_components.HostRoles.host_name[0..1]
    ($zookeeperHosts -join ":2181,") + ":2181"
    ```

    ```bash
    curl -su admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2
    ```

    > [!IMPORTANT]
    > hello Bash példa feltételezi, hogy `$CLUSTERNAME` hello HDInsight-fürt hello nevét tartalmazza. Azt is feltételezi, hogy [jq](https://stedolan.github.io/jq/) telepítve van. Amikor a rendszer kéri, adja meg hello jelszó hello fürt bejelentkezési fiók.

    hello visszaadott érték a következő szöveg hasonló toohello:

        zk0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181,zk2-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181

    > [!IMPORTANT]
    > Miközben kettőnél több Zookeeper csomópontok, nem kell tooprovide összes állomások tooclients teljes listáját. Egy vagy két is elegendő.

    Mentse ezt az értéket, a rendszer később.

3. Hello szerkesztése `dev.properties` hello hello projekt gyökerében található fájl. Ebben a fájlban adja hozzá a hello Broker és Zookeeper állomások információk toohello egyező sor. hello alábbi példa segítségével konfigurálható: hello mintaértékek hello előző lépéseiből:

        kafka.zookeeper.hosts: zk0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181,zk2-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181
        kafka.broker.hosts: wn0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092,wn1-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092
        kafka.topic: stormtopic

4. Mentse a hello `dev.properties` fájlt, majd használja hello parancs tooupload következő azt toohello Storm-fürt:

     ```bash
    scp dev.properties USERNAME@storm-BASENAME-ssh.azurehdinsight.net:KafkaTopology-1.0-SNAPSHOT.jar
    ```

    Cserélje le **felhasználónév** hello SSH felhasználónévvel hello fürthöz. Cserélje le **BASENAME** hello Alap nevű hello fürt létrehozásakor használt.

## <a name="start-hello-writer"></a>Indítsa el a hello írója

1. A következő tooconnect toohello Storm-fürt SSH használatával hello használata. Cserélje le **felhasználónév** a hello hello fürt létrehozásakor használt SSH-felhasználónév. Cserélje le **BASENAME** hello fürt létrehozásakor használt hello Alap nevű.

  ```bash
  ssh USERNAME@storm-BASENAME-ssh.azurehdinsight.net
  ```

    Amikor a rendszer kéri, adja meg a hello fürtök létrehozásakor használt hello jelszót.
   
    További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Hello SSH-kapcsolat alkalmazás a következő parancs toocreate hello hello topológia által használt Kafka témakör hello:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic stormtopic --zookeeper $KAFKAZKHOSTS
    ```

    Cserélje le `$KAFKAZKHOSTS` hello Zookeeper a gazdagép hello előző szakaszban lekért információt.

2. Hello SSH kapcsolat toohello Storm-fürt használja a következő parancs toostart hello író topológia hello:

    ```bash
    storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writer.yaml --filter dev.properties
    ```

    a paranccsal hello paraméterek a következők:

    * `org.apache.storm.flux.Flux`: Fluxus tooconfigure használja, és futtassa ezt a topológiát.

    * `--remote`: Hello topológia tooNimbus nyújt. hello topológia hello munkavégző csomópontokhoz hello fürt között van elosztva.

    * `-R /writer.yaml`: Használat hello `writer.yaml` tooconfigure hello topológia fájlt. `-R`azt jelzi, hogy ehhez az erőforráshoz hello jar-fájl tartalmazza. Hello jar hello gyökerébe ezért van `/writer.yaml` hello elérési tooit van.

    * `--filter`: Hello bejegyzések feltöltése `writer.yaml` topológiáját értékek hello `dev.properties` fájlt. Például a hello értékének hello `kafka.topic` hello fájlban bejegyzés használt tooreplace hello `${kafka.topic}` hello topológia meghatározása bejegyzést.

5. Amikor hello topológia elindult, használja a hello parancs tooverify, hogy az adatok toohello Kafka témakör ír a következő:

  ```bash
  /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --from-beginning --topic stormtopic
  ```

    Cserélje le `$KAFKAZKHOSTS` hello Zookeeper a gazdagép hello előző szakaszban lekért információt.

    Ez a parancs Kafka toomonitor hello témakör rendszerrel szállított parancsfájlt használ. Néhány percet, miután kell kezdenie véletlen toohello témakör írt mondatokat vissza. hello hasonló toohello a következő példa a kimenetre:

        i am at two with nature             
        an apple a day keeps hello doctor away
        snow white and hello seven dwarfs     
        hello cow jumped over hello moon        
        an apple a day keeps hello doctor away
        an apple a day keeps hello doctor away
        hello cow jumped over hello moon        
        an apple a day keeps hello doctor away
        an apple a day keeps hello doctor away
        four score and seven years ago      
        snow white and hello seven dwarfs     
        snow white and hello seven dwarfs     
        i am at two with nature             
        an apple a day keeps hello doctor away

    Használja a Ctrl + c toostop hello parancsfájl.

## <a name="start-hello-reader"></a>Indítsa el a hello olvasó

1. Hello SSH munkamenet toohello Storm-fürt használja a következő parancs toostart hello olvasó topológia hello:

  ```bash
  storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /reader.yaml --filter dev.properties
  ```

2. Miután hello topológia elindítja, nyissa meg a hello Storm felhasználói felülete. A webes felhasználói felület https://storm-BASENAME.azurehdinsight.net/stormui helyezkedik el. Cserélje le __BASENAME__ hello fürt létrehozásakor használt hello Alap nevű. 

    Amikor a rendszer kéri, hello rendszergazda bejelentkezési név használata (alapértelmezés szerint `admin`) és hello fürt létrehozásakor használt jelszót. Megjelenik egy weblap hasonló toohello kép a következő:

    ![A Storm felhasználói felülete](./media/hdinsight-apache-storm-with-kafka/stormui.png)

3. Hello Storm felhasználói felülete, válassza ki hello __kafka-olvasó__ hello hivatkozásra __topológia összegzése__ hello toodisplay információ szakasz __kafka-olvasó__ topológia.

    ![Hello Storm webes felhasználói felület összefoglaló szakasza topológia](./media/hdinsight-apache-storm-with-kafka/topology-summary.png)

4. hello naplózó-bolt összetevő, jelölje be hello hello példányai toodisplay információ __naplózó-bolt__ hello hivatkozásra __Boltokhoz (mindig)__ szakasz.

    ![Hello boltokhoz szakaszban naplózó-bolt-hivatkozás](./media/hdinsight-apache-storm-with-kafka/bolts.png)

5. A hello __végrehajtója__ területen válassza ki a hivatkozást a hello __Port__ a hello-összetevő példányának oszlop toodisplay naplózási információt.

    ![Végrehajtója hivatkozás](./media/hdinsight-apache-storm-with-kafka/executors.png)

    hello napló hello Kafka témakörben olvasásakor hello adatokat tartalmazza. hello információ hello található a következő szöveg hasonló toohello:

        2016-11-04 17:47:14.907 c.m.e.LoggerBolt [INFO] Received data: four score and seven years ago
        2016-11-04 17:47:14.907 STDIO [INFO] hello cow jumped over hello moon
        2016-11-04 17:47:14.908 c.m.e.LoggerBolt [INFO] Received data: hello cow jumped over hello moon
        2016-11-04 17:47:14.911 STDIO [INFO] snow white and hello seven dwarfs
        2016-11-04 17:47:14.911 c.m.e.LoggerBolt [INFO] Received data: snow white and hello seven dwarfs
        2016-11-04 17:47:14.932 STDIO [INFO] snow white and hello seven dwarfs
        2016-11-04 17:47:14.932 c.m.e.LoggerBolt [INFO] Received data: snow white and hello seven dwarfs
        2016-11-04 17:47:14.969 STDIO [INFO] an apple a day keeps hello doctor away
        2016-11-04 17:47:14.970 c.m.e.LoggerBolt [INFO] Received data: an apple a day keeps hello doctor away

## <a name="stop-hello-topologies"></a>Állítsa le a hello topológiák

SSH munkamenet toohello alatt futó Storm fürtök a következő parancsok toostop hello Storm-topológiák hello használata:

  ```bash
  storm kill kafka-writer
  storm kill kafka-reader
  ```

## <a name="delete-hello-cluster"></a>Hello fürt törlése

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Mivel a jelen dokumentumban leírt lépések hello létrehozása a fürtök hello azonos Azure-erőforráscsoportot, törölheti a hello erőforráscsoportja hello Azure-portálon. Hello erőforráscsoport törlése eltávolítja a dokumentum a következő által létrehozott összes erőforrás.

## <a name="next-steps"></a>Következő lépések

Tekintse meg a HDInsight alatt futó Storm használható további példa topológiák [példa Storm-topológiák és összetevők](hdinsight-storm-example-topology.md).

A telepítését és megfigyelését a HDInsight Linux-alapú topológiák további információkért lásd: [központi telepítése és kezelése a Linux-alapú HDInsight alatt futó Apache Storm-topológiák](hdinsight-storm-deploy-monitor-topology-linux.md)