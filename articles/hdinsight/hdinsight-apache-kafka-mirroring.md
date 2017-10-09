---
title: "aaaMirror Apache Kafka témakörök - Azure HDInsight |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Apache Kafka tükrözési funkció toomaintain egy replikát készít egy HDInsight-fürt Kafka témakörök tooa másodlagos fürt tükrözése révén."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 015d276e-f678-4f2b-9572-75553c56625b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/13/2017
ms.author: larryfr
ms.openlocfilehash: 5ace0251d7402d4d7d9b28726e253ce7091a87ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-mirrormaker-tooreplicate-apache-kafka-topics-with-kafka-on-hdinsight-preview"></a>A HDInsight (előzetes verzió) Kafka MirrorMaker tooreplicate Apache Kafka témakörök használata

Ismerje meg, hogyan toouse Apache Kafka a tükrözést a szolgáltatás tooreplicate témakörök tooa másodlagos fürt. Tükrözés folyamatos folyamatként lefutott, és áttelepítése módszerként időnként használt adatokat egy fürt tooanother.

Ebben a példában a tükrözés jelenleg használt tooreplicate témakörök két HDInsight-fürtök között. Mindkét fürt szerepelnek a hello egy Azure virtuális hálózat ugyanabban a régióban.

> [!WARNING]
> Tükrözés nem kell tekinteni a azt jelenti, hogy tooachieve hibatűrést. hello eltolási tooitems egy témakör különböznek hello forrás és cél fürtök, így nem tudják használni hello két azonos értelemben.
>
> Ha aggódik hibatűrést, a fürtön belül kell beállítani a replikációs hello témaköröket. További információkért lásd: [Ismerkedjen meg a HDInsight Kafka](hdinsight-apache-kafka-get-started.md).

## <a name="how-kafka-mirroring-works"></a>Tükrözés Kafka működése

Tükrözési works hello MirrorMaker eszköz (Apache Kafka része) tooconsume használatával rögzíti a kiindulási fürt hello témaköröktől, és létrehozhat egy helyi példány hello cél fürtön. MirrorMaker használ egy (vagy több) *fogyasztók* hello forrásfürt, olvasó és egy *készítő* , írja az toohello (cél) helyi fürt.

a következő diagram hello hello tükrözés folyamatát mutatja be:

![Hello tükrözés folyamat diagramja](./media/hdinsight-apache-kafka-mirroring/kafka-mirroring.png)

A HDInsight Apache Kafka nem biztosít hozzáférést toohello Kafka szolgáltatás képest hello nyilvános internethez. Kafka gyártók vagy fogyasztók kell hello hello Kafka fürtben csomópontként hello azonos Azure virtuális hálózatban. Ehhez a példához hello Kafka forrás és a cél fürtök találhatók egy Azure virtuális hálózatra. a következő ábra azt mutatja be, hogyan kommunikációs hello fürtök között zajló kommunikációról hello:

![Egy Azure virtuális hálózatban fürtök forrás és cél Kafka ábrája](./media/hdinsight-apache-kafka-mirroring/spark-kafka-vnet.png)

csomópontok és a partíciók száma hello hello forrás és cél fürtök eltérhet, és eltolások hello témakörök belül különböző is. Particionálás, amellyel kulcsérték hello tükrözés tart fenn, így sorrendje megőrződik kulcs alapú.

### <a name="mirroring-across-network-boundaries"></a>Hálózati határokon keresztül történő tükrözés

Különböző hálózatokon Kafka fürtök közötti toomirror van szüksége, ha nincsenek további szempontokról a következő hello:

* **Átjárók**: hello hálózatok: hello TCPIP szint képes toocommunicate kell lennie.

* **Nevek feloldása**: az egyes hálózati fürtök Kafka hello segítségével hostnames kell lennie más képes tooconnect tooeach. Ez lehet szükség, a tartománynévrendszer (DNS) kiszolgáló, amely minden egyes hálózati beállítva tooforward kérelmek toohello más hálózatokkal.

    Hello hálózattal hello automatikus DNS megadott helyett egy Azure virtuális hálózat létrehozásakor meg kell adnia egy egyéni DNS-kiszolgáló és a hello kiszolgáló IP-címe hello. Után a rendszer létrehozta a virtuális hálózati hello, majd hozzon létre egy Azure virtuális gép által használt IP-címet, majd telepítenie és konfigurálnia kell DNS szoftver rajta.

    > [!WARNING]
    > Hozzon létre és hello egyéni DNS-kiszolgáló konfigurálása a virtuális hálózati hello HDInsight telepítése előtt. A HDInsight toouse hello DNS-kiszolgálóhoz konfigurált virtuális hálózati hello szükség további beállításokra van.

A két Azure virtuális hálózatokhoz csatlakozó további információkért lásd: [VNet – VNet-kapcsolatot konfiguráló](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).

## <a name="create-kafka-clusters"></a>Kafka fürtök létrehozása

Létrehozhat egy Azure virtuális hálózatra, és manuálisan fürtök Kafka, akkor könnyebb toouse Azure Resource Manager-sablonok. Használja a következő lépéseket toodeploy hello Azure virtuális hálózat és két Kafka fürtök tooyour Azure-előfizetés.

1. A tooAzure gomb toosign és hello Azure-portál megnyitása hello sablon hello használata.
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json" target="_blank"><img src="./media/hdinsight-apache-kafka-mirroring/deploy-to-azure.png" alt="Deploy tooAzure"></a>
   
    hello Azure Resource Manager sablon itt található: **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json**.

    > [!WARNING]
    > a HDInsight Kafka tooguarantee rendelkezésre állását, a fürt tartalmaznia kell legalább három munkavégző csomópontokhoz. Ez a sablon a három munkavégző csomópontokhoz tartalmaz Kafka fürtöt hoz létre.

2. Információk toopopulate hello tételek követően hello használata hello **egyéni telepítési** panel:
    
    ![HDInsight egyéni központi telepítés](./media/hdinsight-apache-kafka-mirroring/parameters.png)
    
    * **Erőforráscsoport**: hozzon létre egy csoportot, vagy válasszon egy meglévőt. Ez a csoport hello HDInsight-fürtöt tartalmaz.

    * **Hely**: Válasszon egy helyet a földrajzi elhelyezkedés alapján Bezárás tooyou.
     
    * **Fürt neve kiinduló**: Ez az érték neveként hello alapszintű hello Kafka fürtök használható. Ha például **hdi** nevű fürtök létrehozása **forrás-hdi** és **cél-hdi**.

    * **A fürt bejelentkezési felhasználónevét**: hello rendszergazda felhasználóneve hello forrás és cél Kafka fürtök.

    * **A fürt bejelentkezési jelszó**: hello rendszergazdai jelszóval hello forrás és cél Kafka fürtök.

    * **SSH-felhasználónév**: hello SSH felhasználói toocreate hello forrás és cél Kafka fürtök.

    * **SSH-jelszónak**: hello jelszó hello SSH hello forrás és cél Kafka fürtök.

3. Olvasási hello **feltételek és kikötések**, majd válassza ki **toohello feltételek és kikötések fenti elfogadom**.

4. Végül ellenőrizze **PIN-kód toodashboard** majd **beszerzési**. Körülbelül 20 percet toocreate hello fürtök szükséges.

Létrehozása után a hello erőforrásokat, átirányított tooa panel hello fürtök és a webes irányítópult tartalmazó erőforráscsoport hello áll.

![Erőforráscsoport panel hello vnet és fürtök](./media/hdinsight-apache-kafka-mirroring/groupblade.png)

> [!IMPORTANT]
> Figyelje meg, hogy vannak-e a HDInsight-fürtök hello hello nevei **forrás-BASENAME** és **cél-BASENAME**, ahol BASENAME toohello sablon megadott hello nevét. Ezeket a neveket használja a későbbi lépésekben toohello fürtök kapcsolódáskor.

## <a name="create-topics"></a>Hozzon létre kapcsolatos témakörök

1. Csatlakozás toohello **forrás** fürtön SSH használatával:

    ```bash
    ssh sshuser@source-BASENAME-ssh.azurehdinsight.net
    ```

    Cserélje le **sshuser** a hello hello fürt létrehozásakor használt SSH-felhasználónév. Cserélje le **BASENAME** hello fürt létrehozásakor használt hello Alap nevű.

    További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Használjon hello következő toofind hello Zookeeper állomások hello forrásfürt parancsokat:

    ```bash
    # Install jq if it is not installed
    sudo apt -y install jq
    # get hello zookeeper hosts for hello source cluster
    export SOURCE_ZKHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    
    Replace `$PASSWORD` with hello password for hello cluster.

    Replace `$CLUSTERNAME` with hello name of hello source cluster.

3. toocreate a topic named `testtopic`, use hello following command:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 2 --partitions 8 --topic testtopic --zookeeper $SOURCE_ZKHOSTS
    ```

3. A következő parancs tooverify, amely a témakör hello használata hello jött létre:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $SOURCE_ZKHOSTS
    ```

    hello válaszban `testtopic`.

4. Használjon hello tooview hello Zookeeper állomás adatokat a következő (hello **forrás**) fürt:

    ```bash
    echo $SOURCE_ZKHOSTS
    ```

    Ez visszaadja a szöveg a következő információk hasonló toohello:

    `zk0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181,zk1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181`

    Mentse ezt az információt. A következő szakaszban hello szolgál.

## <a name="configure-mirroring"></a>Konfigurálja a tükrözés

1. Csatlakozás toohello **cél** a fürt egy másik SSH-munkamenetet használatával:

    ```bash
    ssh sshuser@dest-BASENAME-ssh.azurehdinsight.net
    ```

    Cserélje le **sshuser** a hello hello fürt létrehozásakor használt SSH-felhasználónév. Cserélje le **BASENAME** hello fürt létrehozásakor használt hello Alap nevű.

    További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Használjon hello következő parancsot a toocreate egy `consumer.properties` fájlt, amely ismerteti, hogyan toocommunicate a hello **forrás** fürt:

    ```bash
    nano consumer.properties
    ```

    Szöveg hello hello tartalmát, a következő használatát hello `consumer.properties` fájlt:

    ```yaml
    zookeeper.connect=SOURCE_ZKHOSTS
    group.id=mirrorgroup
    ```

    Cserélje le **SOURCE_ZKHOSTS** a hello Zookeeper hello adatait tároló **forrás** fürt.

    Ez a fájl hello fogyasztói információk toouse írja le, Kafka fürt hello forrásból olvasásakor. További információk a fogyasztói konfigurációhoz, lásd: [fogyasztói Configs](https://kafka.apache.org/documentation#consumerconfigs) kafka.apache.org címen.

    toosave hello fájl használata **Ctrl + X**, **Y**, majd **Enter**.

3. Mielőtt konfigurálná a kommunikáló hello készítő hello cél fürttel, keresse meg hello broker állomások a hello **cél** fürt. A következő parancsok tooretrieve hello ezeket az információkat használhatja:

    ```bash
    sudo apt -y install jq
    DEST_BROKERHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    echo $DEST_BROKERHOSTS
    ```

    Cserélje le `$PASSWORD` hello bejelentkezési fiók (felügyeleti) jelszóval hello fürthöz.

    Cserélje le `$CLUSTERNAME` hello célfürtöt hello nevére.

    Ezek a parancsok információk hasonló toohello következő vissza:

        wn0-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn1-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092

4. A következő toocreate használata hello egy `producer.properties` fájlt, amely ismerteti, hogyan toocommunicate hello a **cél** fürt:

    ```bash
    nano producer.properties
    ```

    Szöveg hello hello tartalmát, a következő használatát hello `producer.properties` fájlt:

    ```yaml
    bootstrap.servers=DEST_BROKERS
    compression.type=none
    ```

    Cserélje le **DEST_BROKERS** hello előző lépésben hello broker adatokkal.

    További információk készítő konfigurációs, lásd: [készítő Configs](https://kafka.apache.org/documentation#producerconfigs) kafka.apache.org címen.

## <a name="start-mirrormaker"></a>Indítsa el a MirrorMaker

1. Az SSH-kapcsolat toohello hello **cél** fürt esetén a következő parancs toostart hello MirrorMaker folyamat hello használata:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-run-class.sh kafka.tools.MirrorMaker --consumer.config consumer.properties --producer.config producer.properties --whitelist testtopic --num.streams 4
    ```

    Ebben a példában használt hello paraméterek a következők:

    * **--consumer.config**: hello fájlt, amelyben felhasználói tulajdonságok adja meg. Ezek a tulajdonságok, amelyek hello olvassa be az ügyféllel használt toocreate *forrás* Kafka fürt.

    * **--producer.config**: készítő tulajdonságokat tartalmazó hello fájlt adja meg. Ezek a Tulajdonságok használt toocreate írja az toohello termelő *cél* Kafka fürt.

    * **--az engedélyezett**: hello forrás fürt toohello cél a replikáló MirrorMaker témakörök listáját.

    * **--num.streams**: hello fogyasztói szálak toocreate száma.

 Rendszerindításkor MirrorMaker információkat a következő szöveg hasonló toohello adja vissza:

    ```json
    {metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-3, security.protocol=PLAINTEXT}{metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-0, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-kafka.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-2, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-1, security.protocol=PLAINTEXT}
    ```

2. Az SSH-kapcsolat toohello hello **forrás** fürt, használja a következő parancs toostart termelő hello és üzenetek toohello témakör küldeni:

    ```bash
    SOURCE_BROKERHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $SOURCE_BROKERHOSTS --topic testtopic
    ```

    Cserélje le `$PASSWORD` hello forrás fürt hello (rendszergazda) bejelentkezési jelszóval.

    Cserélje le `$CLUSTERNAME` hello forrásfürt hello nevére.

     Amikor egy üres sort a kurzorral érkeznek, adja meg néhány szöveges üzeneteket. Ezek toohello témakör továbbküldené hello **forrás** fürt. Amikor végzett, **Ctrl + C** tooend hello készítő folyamat.

3. Az SSH-kapcsolat toohello hello **cél** fürt esetén használjon **Ctrl + C** tooend hello MirrorMaker folyamat. Akkor használja hello következő parancsok tooverify adott hello `testtopic` témakör lett létrehozva, és tárolt adatokat hello témakör replikált toothis tükrözött:

    ```bash
    DEST_ZKHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $DEST_ZKHOSTS
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $DEST_ZKHOSTS --topic testtopic --from-beginning
    ```

    Cserélje le `$PASSWORD` hello cél fürt hello (rendszergazda) bejelentkezési jelszóval.

    Cserélje le `$CLUSTERNAME` hello célfürtöt hello nevére.

    hello témakörlistáját most már tartalmaz `testtopic`, ami akkor jön létre, amikor MirrorMaster tükrözi hello forrás fürt toohello cél hello témakört. hello témakör lekért köszönőüzenetei hello megegyeznek a megadott hello kiindulási fürt.

## <a name="delete-hello-cluster"></a>Hello fürt törlése

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Mivel a jelen dokumentumban leírt lépések hello létrehozása a fürtök hello azonos Azure-erőforráscsoportot, törölheti a hello erőforráscsoportja hello Azure-portálon. Hello erőforráscsoport törlése eltávolítja a hozta létre a következő a dokumentum, hello Azure virtuális hálózat és tárfiók hello fürtök által használt összes erőforrást.

## <a name="next-steps"></a>Következő lépések

Ebből a dokumentumból megtanulta, hogyan toouse MirrorMaker toocreate egy replikát készít egy Kafka fürt. Használja a következő hivatkozások toodiscover hello más módokon toowork Kafka:

* [Apache Kafka MirrorMaker dokumentáció](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) cwiki.apache.org címen.
* [Ismerkedés az Apache Kafka a HDInsight-on](hdinsight-apache-kafka-get-started.md)
* [Az Apache Spark használata a Kafkával a HDInsighton](hdinsight-apache-spark-with-kafka.md)
* [Az Apache Storm használata a HDInsighton futó Kafkával](hdinsight-apache-storm-with-kafka.md)
* [Csatlakozás tooKafka egy Azure virtuális hálózaton keresztül](hdinsight-apache-kafka-connect-vpn-gateway.md)
