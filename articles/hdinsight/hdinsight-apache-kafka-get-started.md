---
title: az Apache Kafka - Azure HDInsight aaaStart |} Microsoft Docs
description: "Ismerje meg, hogyan toocreate egy Apache Kafka Azure HDInsight fürt. Megtudhatja, hogyan toocreate témakörök, előfizetőket és fogyasztók."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 43585abf-bec1-4322-adde-6db21de98d7f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/14/2017
ms.author: larryfr
ms.openlocfilehash: b93299d88dc2cf9a9764662509308ff75fd74474
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="start-with-apache-kafka-preview-on-hdinsight"></a>Az Apache Kafka (előzetes verzió) használatának első lépései a HDInsightban

Megtudhatja, hogyan toocreate, és egy [Apache Kafka](https://kafka.apache.org) on Azure HDInsight fürt. A Kafka egy, a HDInsighthoz is elérhető, nyílt forráskódú elosztott adatstreamelési platform. Gyakran használják üzenet közvetítőként, mivel hasonló funkciókat biztosít tooa közzétételi-feliratkozási üzenet-várólista.

> [!NOTE]
> Jelenleg a Kafka két verziója érhető el a HDInsighttal: a 0.9.0 (HDInsight 3.4) és a 0.10.0 (HDInsight 3.5 és 3.6). Ez a dokumentum hello lépések azt feltételezik, hogy a HDInsight 3.6 Kafka használ.

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="create-a-kafka-cluster"></a>Kafka-fürt létrehozása

Következő lépések toocreate egy Kafka HDInsight-fürt hello használata:

1. A hello [Azure-portálon](https://portal.azure.com), jelölje be **+ új**, **Eszközintelligencia + analitika**, majd válassza ki **HDInsight**.
   
    ![HDInsight-fürt létrehozása](./media/hdinsight-apache-kafka-get-started/create-hdinsight.png)

2. A **alapjai**, adja meg a következő információ hello:

    * **A fürt neve**: hello hello HDInsight-fürt nevét.
    * **Előfizetés**: hello előfizetés toouse kiválasztása.
    * **A fürt bejelentkezési felhasználónevének** és **fürt bejelentkezési jelszó**: hello bejelentkezési hello fürt eléréséhez a HTTPS-KAPCSOLATON keresztül. A hitelesítő adatok tooaccess szolgáltatások például hello Ambari webes felhasználói felületén vagy a REST API-t használja.
    * **Secure Shell (SSH) felhasználónév**: hello bejelentkezési SSH-n keresztül hello fürt eléréséhez használt. Alapértelmezés szerint hello jelszó hello ugyanaz, mint a hello fürt bejelentkezési jelszót.
    * **Erőforráscsoport**: hello erőforrás csoport toocreate hello fürtöt.
    * **Hely**: hello Azure-régiót toocreate hello fürtöt.
   
 ![Előfizetés kiválasztása](./media/hdinsight-apache-kafka-get-started/hdinsight-basic-configuration.png)

3. Válassza ki **típusú fürt**, majd a készlet hello következő értékeit és **fürtkonfiguráció**:
   
    * **Fürt típusa**: Kafka

    * **Verzió**: Kafka 0.10.0 (HDI 3.6)

    * **Fürt szintje**: Standard
     
 Végül, használja a hello **válasszon** toosave beállítások gombra.
     
 ![Fürttípus kiválasztása](./media/hdinsight-apache-kafka-get-started/set-hdinsight-cluster-type.png)

4. Hello fürt típusának kiválasztása után használja a hello __válasszon__ gomb tooset hello fürt típusa. Következő lépésként az hello __következő__ gomb toofinish alapszintű konfigurálása.

5. A **Tárolás** panelen válasszon ki vagy hozzon létre egy Storage-fiókot. A jelen dokumentumban leírt lépések hello hagyja hello más mezőlistából hello alapértelmezett értékeket. Használjon hello __következő__ gomb toosave tárolási konfigurációt.

    ![Állítsa be a HDInsight-hello tárolási fiók beállításai](./media/hdinsight-apache-kafka-get-started/set-hdinsight-storage-account.png)

6. A __(nem kötelező) alkalmazások__, jelölje be __következő__ toocontinue. Az alábbi példához nem szükséges alkalmazás.

7. A __a fürt méretét__, jelölje be __következő__ toocontinue.

    > [!WARNING]
    > a HDInsight Kafka tooguarantee rendelkezésre állását, a fürt tartalmaznia kell legalább három munkavégző csomópontokhoz.

    ![Set hello Kafka foglalásiegység-méret](./media/hdinsight-apache-kafka-get-started/kafka-cluster-size.png)

    > [!NOTE]
    > Hello **egyes feldolgozó csomópontok lemezek** bejegyzés vezérlők hello méretezhetőséget biztosít a hdinsight Kafka. További információ: [Configure storage and scalability of Kafka on HDInsight](hdinsight-apache-kafka-scalability.md) (A HDInsightban futó Kafka tárolójának és méretezhetőségének konfigurálása).

8. A __speciális beállítások__, jelölje be __következő__ toocontinue.

9. A hello **összegzés**, tekintse át a hello fürt hello konfigurációját. Használjon hello __szerkesztése__ hivatkozásait toochange életbe léptetett beállítások helytelenek. Végezetül the__Create__ gomb toocreate hello-fürt használatára.
   
    ![A fürtkonfiguráció összegzése](./media/hdinsight-apache-kafka-get-started/hdinsight-configuration-summary.png)
   
    > [!NOTE]
    > Too20 perc toocreate hello fürt is eltarthat.

## <a name="connect-toohello-cluster"></a>Csatlakoztassa toohello fürtöt

> [!IMPORTANT]
> Hello lépések végrehajtásához, egy SSH-ügyfelet kell használnia. További információkért lásd: hello [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentum.

Az ügyfél SSH tooconnect toohello-fürt használatára:

```ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net```

Cserélje le **SSHUSER** a fürt létrehozása során megadott hello SSH-felhasználónév. Cserélje le **CLUSTERNAME** hello nevű hello fürt.

Amikor a rendszer kéri, adja meg az SSH-fiókjának hello használt hello jelszót.

További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="getkafkainfo"></a>Hello Zookeeper és Broker állomásadatai beolvasása

Amikor olyan Kafka, ismernie kell a két állomás érték; Hello *Zookeeper* gazdagépek és hello *Broker* gazdagépek. Ezek a gazdagépek hello Kafka API, és küldje el hello segédeszközök számos Kafka használhatók.

A következő lépéseket toocreate környezeti változók hello állomás adatokat tartalmazó hello használata. Ezek a környezeti változók a jelen dokumentumban leírt lépések hello használják.

1. SSH kapcsolat toohello fürtök, használjon hello következő parancsot a tooinstall hello `jq` segédprogram. Ez a segédprogram használt tooparse JSON-dokumentumokat, és hasznos hello broker állomás adatok beolvasása:
   
    ```bash
    sudo apt -y install jq
    ```

2. tooset hello környezeti változók információkkal lekért Ambari, a következő parancsok használata hello:

    ```bash
    CLUSTERNAME='your cluster name'
    PASSWORD='your cluster password'
    export KAFKAZKHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`

    export KAFKABROKERS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`

    echo '$KAFKAZKHOSTS='$KAFKAZKHOSTS
    echo '$KAFKABROKERS='$KAFKABROKERS
    ```

    > [!IMPORTANT]
    > Állítsa be `CLUSTERNAME=` hello Kafka fürt toohello nevét. Állítsa be `PASSWORD=` hello fürt létrehozásakor használt toohello (rendszergazda) bejelentkezési jelszót.

    hello következő szöveg látható egy példa hello tartalmát `$KAFKAZKHOSTS`:
   
    `zk0-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181,zk2-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181`
   
    hello következő szöveg látható egy példa hello tartalmát `$KAFKABROKERS`:
   
    `wn1-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092,wn0-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092`

    > [!NOTE]
    > Hello `cut` parancs a gazdagépek tootwo állomás bejegyzés használt tootrim hello listája. Egy Kafka fogyasztó vagy készítő létrehozásakor nem kell tooprovide hello teljes állomások listája.
   
    > [!WARNING]
    > Ne használja a hello információkat a munkamenet által visszaadott tooalways pontos lehet. Ha hello fürt méretezéséhez új brókerek hozzáadásakor vagy eltávolításakor. Ha hiba lép fel, és egy csomópont váltja fel, hello host name hello csomópont módosíthatja.
    >
    > Hello Zookeeper és broker állomások adatokat kell beolvasni, hamarosan használat előtt mindig érvényes információk tooensure.

## <a name="create-a-topic"></a>Üzenettémakör létrehozása

A Kafka *témaköröknek* nevezett kategóriákban tárolja az adatstreameket. Az SSH kapcsolat tooa fürt headnode, a megadott Kafka toocreate témakör parancsfájl használata:

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
```

Ezzel a paranccsal összekapcsolja tooZookeeper tárolt hello állomás információk használata `$KAFKAZKHOSTS`, majd hozza létre a nevű Kafka témakör **tesztelése**. Hello témakör hozott létre a következő parancsfájl toolist témakörök hello ellenőrizheti:

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $KAFKAZKHOSTS
```

hello a parancs felsorolja Kafka témakörök hello tartalmazó **tesztelése** témakör.

## <a name="produce-and-consume-records"></a>Rekordok létrehozása és felhasználása

A Kafka témakörökben tárolja a *rekordokat*. A rekordokat *előállítók* hozzák létre, és *fogyasztók* használják fel. Az előállítók *Kafka-közvetítőktől* kérik le a rekordokat. A HDInsight-fürt mindegyik feldolgozó csomópontja egy Kafka-közvetítő.

Írja be a következő lépéseket toostore rekordok hello teszt témakörbe korábban létrehozott, és olvassa el őket az ügyféllel hello használata:

1. Hello SSH-munkamenetet, a megadott Kafka toowrite rekordok toohello témakörrel parancsfájl használata:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test
    ```
   
    Nem ismét toohello Rákérdezés a parancs után. Ehelyett írja be a néhány szöveges üzenet, majd **Ctrl + C** toostop toohello témakör küldésekor. A rendszer minden sort külön rekordként küld el.

2. A megadott Kafka tooread bejegyzéseit hello témakör parancsprogram használata:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic test --from-beginning
    ```
   
    Ez a parancs hello rekordok kikeresi hello témakör, és megjeleníti őket. Használatával `--from-beginning` hello fogyasztói toostart közli a hello elejétől hello adatfolyam, így minden rekordot a rendszer beolvassa.

3. Használjon __Ctrl + C__ toostop hello fogyasztói.

## <a name="producer-and-consumer-api"></a>Előállítói és fogyasztói API

Akkor is szoftveresen is előállításához és fogyasztásához hello rekordok [Kafka API-k](http://kafka.apache.org/documentation#api). toobuild egy Java-készítő és a fogyasztói, használja a fejlesztési környezetet az alábbi lépésekkel hello.

> [!IMPORTANT]
> A fejlesztési környezetben telepített összetevők a következő hello kell rendelkeznie:
>
> * [Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) vagy azzal egyenértékű, például az OpenJDK.
>
> * [Apache Maven](http://maven.apache.org/)
>
> * Egy SSH-ügyfél és a hello `scp` parancsot. További információkért lásd: hello [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentum.

1. Töltse le a hello példák [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started). Például hello készítő/fogyasztói használni hello projekt hello `Producer-Consumer` könyvtár. Ebben a példában a következő osztályok hello tartalmazza:
   
    * **Futtatás** -hello fogyasztó vagy a gyártó kezdődik.

    * **Gyártó** -tárolók 1 000 000 rekordot toohello témakör.

    * **Fogyasztó** -rekordot olvas hello témakör.

2. egy jar csomag toocreate módosítani könyvtárak toohello hello `Producer-Consumer` könyvtárra, és használja hello a következő parancsot:

    ```
    mvn clean package
    ```

    A parancs létrehozza a `target` nevű könyvtárat, amely a `kafka-producer-consumer-1.0-SNAPSHOT.jar` nevű fájlt tartalmazza.

3. Használjon hello következő parancsok toocopy hello `kafka-producer-consumer-1.0-SNAPSHOT.jar` tooyour HDInsight-fürt:
   
    ```bash
    scp ./target/kafka-producer-consumer-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-producer-consumer.jar
    ```
   
    Cserélje le **SSHUSER** hello SSH-felhasználó a fürt, és cserélje le a **CLUSTERNAME** hello néven a fürt. Amikor a rendszer kéri hello jelszó megadása hello SSH-felhasználó.

4. Egyszer hello `scp` parancs végrehajtása hello fájl másolása, csatlakoztassa toohello fürtöt SSH használatával. A következő parancs toowrite rekordok toohello Teszt témakör hello használata:

    ```bash
    java -jar kafka-producer-consumer.jar producer $KAFKABROKERS
    ```

5. Amikor hello véget ért, használja a hello parancs tooread hello a témakör a következő:
   
    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS
    ```
   
    hello rekordok Olvasás, bejegyzések, száma mellett jelenik meg. Láthatja, hogy néhány több mint 1 000 000 több rekordok toohello témakör parancsfájl használata a korábbi lépésben küldött hibaként naplózza.

6. Használjon __Ctrl + C__ tooexit hello fogyasztói.

### <a name="multiple-consumers"></a>Több fogyasztó

A Kafka-fogyasztók egy fogyasztói csoportot használnak a rekordok olvasásakor. Azonos csoport hello használata több felhasználóból terhelést eredmények eloszlik a témakör az olvasási műveletek. Hello csoport minden fogyasztó hello rekordok része. Ez a folyamat, a művelet a következő használatát hello lépések toosee:

1. Új SSH munkamenet toohello fürt, nyissa meg a, hogy két őket. Minden munkamenet használja a hello toostart követően az ügyféllel azonos fogyasztói Csoportazonosító hello:
   
    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS mygroup
    ```

    A paranccsal elindítja a fogyasztó hello csoport azonosítójával `mygroup`.

    > [!NOTE]
    > Hello parancsokat használhatja hello [információért hello Zookeeper és Broker állomás](#getkafkainfo) szakasz tooset `$KAFKABROKERS` a SSH-munkamenethez.

2. Nézze meg bejegyzésként minden munkamenet számok hello hello témakör megkapja. mindkét munkamenetek hello összesen kell kell hello azonos módon korábban kapott egy fogyasztói.

Az ügyfelek belül azonos csoport hello partíciókat a hello témakör segítségével kezel hello használatát. A hello `test` korábban létrehozott témakör nyolc partíciókkal rendelkezik. Nyissa meg a nyolc SSH-munkamenetet, és indítsa el a fogyasztó minden munkamenetben, ha mindegyik felhasználó rekordok hello témakör egyetlen partícióra olvassa be.

> [!IMPORTANT]
> A fogyasztói csoportban található fogyasztói példányok száma nem haladhatja meg a partíciók számát. Ebben a példában egy fogyasztói csoporton tartalmazhat tooeight fogyasztók mentése mivel az hello hello témakörben található partíciók száma. Emellett lehet több, legfeljebb nyolc fogyasztóval rendelkező fogyasztói csoportja is.

Kafka tárolt bejegyzések hello rendelés kaptak a partíción belül vannak tárolva. tooachieve-sorrendjük kézbesítési rekordok *a partíción belül*, hozzon létre egy felhasználói csoportot, ahol felhasználói példányok száma hello partíciók hello számának egyeznie kell. tooachieve-sorrendjük kézbesítési rekordok *hello témakör*, hozzon létre egy felhasználói csoport csak egy felhasználói példányt.

## <a name="streaming-api"></a>Streamelési API

adatfolyam-API hello hozzá lett adva tooKafka verziójában 0.10.0-s; a korábbi támaszkodnak Apache Spark vagy Storm adatfolyam feldolgozásra.

1. Ha még nem tette meg, töltse le a hello példák [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) tooyour fejlesztési környezet. A példa streaming hello, használjon hello projekt hello `streaming` könyvtár.
   
    A projekt tartalmaz csak egy osztály `Stream`, amely olvassa be az rekordok hello `test` korábban létrehozott témakör. Megszámolja hello szavakat, olvassa el, és bocsát ki minden egyes nevű word és count tooa témakör `wordcounts`. Hello `wordcounts` témakör ebben a szakaszban egy későbbi lépésben jön létre.

2. Hello parancssorban a fejlesztési környezetben módosítani könyvtárak toohello hello `Streaming` könyvtárra, és a következő parancs toocreate jar-csomag használata hello:

    ```bash
    mvn clean package
    ```

    A parancs létrehozza a `target` nevű könyvtárat, amely a `kafka-streaming-1.0-SNAPSHOT.jar` nevű fájlt tartalmazza.

3. Használjon hello következő parancsok toocopy hello `kafka-streaming-1.0-SNAPSHOT.jar` tooyour HDInsight-fürt:
   
    ```bash
    scp ./target/kafka-streaming-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-streaming.jar
    ```
   
    Cserélje le **SSHUSER** hello SSH-felhasználó a fürt, és cserélje le a **CLUSTERNAME** hello néven a fürt. Amikor a rendszer kéri hello jelszó megadása hello SSH-felhasználó.

4. Egyszer hello `scp` parancs végrehajtása hello fájl másolása, csatlakozzon az SSH használatával toohello fürt, és használja a következő parancs toocreate hello hello `wordcounts` témakör:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic wordcounts --zookeeper $KAFKAZKHOSTS
    ```

5. Ezután indítsa el az adatfolyam-folyamat a következő parancs hello segítségével hello:
   
    ```bash
    java -jar kafka-streaming.jar $KAFKABROKERS $KAFKAZKHOSTS 2>/dev/null &
    ```
   
    Adatfolyam-folyamat hello háttérben hello indítja el.

6. Használjon hello következő parancsot a toosend üzenetek toohello `test` témakör. Ezek az üzenetek dolgozza fel hello adatfolyam-példa:
   
    ```bash
    java -jar kafka-producer-consumer.jar producer $KAFKABROKERS &>/dev/null &
    ```

7. A következő parancs tooview hello kimeneti toohello írt használata hello `wordcounts` témakör hello adatfolyam-folyamat által:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic wordcounts --from-beginning --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer
    ```
   
    > [!NOTE]
    > tooview hello adatokat, akkor kell mondja el hello tooprint hello kulcsa és hello deszerializáló toouse a hello kulcs-érték. hello kulcsnév hello szó, és hello kulcsérték hello számát tartalmazza.
   
    a kimeneti hello hasonló toohello a következő szöveget:
   
        dwarfs  13635
        ago     13664
        snow    13636
        dwarfs  13636
        ago     13665
        a       13803
        ago     13666
        a       13804
        ago     13667
        ago     13668
        jumped  13640
        jumped  13641
        a       13805
        snow    13637
   
    > [!NOTE]
    > minden alkalommal, amikor a rendszer észlelt egy word-os hello száma.

7. Hello használata __Ctrl + C__ tooexit hello fogyasztói, akkor használja a hello `fg` toobring hello adatfolyam háttér feladat hátsó toohello előtér parancsot. Használjon __Ctrl + C__ tooexit azt is.

## <a name="delete-hello-cluster"></a>Hello fürt törlése

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Hibaelhárítás

Ha problémába ütközik a HDInsight-fürtök létrehozása során, tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Következő lépések

Ebből a dokumentumból megismerte rendelkezik használata a HDInsight Apache Kafka hello alapjait. Hello toolearn több Kafka használatával kapcsolatban a következő használja:

* [Magas rendelkezésre állású adatok biztosítása a HDInsightban futó Kafka platformmal](hdinsight-apache-kafka-high-availability.md)
* [A méretezhetőség növelése felügyelt lemezek HDInsightban futó Kafkával történő konfigurálásával](hdinsight-apache-kafka-scalability.md)
* Az [Apache Kafka dokumentációja](http://kafka.apache.org/documentation.html) a kafka.apache.org webhelyen.
* [A HDInsight MirrorMaker toocreate Kafka másolatának használata](hdinsight-apache-kafka-mirroring.md)
* [Az Apache Storm használata a HDInsighton futó Kafkával](hdinsight-apache-storm-with-kafka.md)
* [Az Apache Spark használata a Kafkával a HDInsighton](hdinsight-apache-spark-with-kafka.md)
* [Csatlakozás tooKafka egy Azure virtuális hálózaton keresztül](hdinsight-apache-kafka-connect-vpn-gateway.md)
