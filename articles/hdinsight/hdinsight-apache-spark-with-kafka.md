---
title: "aaaApache Spark Kafka - Azure HDInsight Stream továbbítása |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Spark Apache Spark toostream adatok virtuális gépbe vagy onnan Apache Kafka DStreams használatával. Ebben a példában a HDInsight Spark a Jupyter notebook használatával adatok folyamatos átviteléhez."
keywords: "kafka például kafka zookeeper, spark streamelési kafka, spark streamelési kafka – példa"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: dd8f53c1-bdee-4921-b683-3be4c46c2039
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/13/2017
ms.author: larryfr
ms.openlocfilehash: f48b37aadafa4979cd27af68e8417db6acc8a0e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="apache-spark-streaming-dstream-example-with-kafka-preview-on-hdinsight"></a>Apache Spark streaming (DStream) például Kafka (előzetes verzió) a HDInsight a

Megtudhatja, hogyan toouse Spark Apache Spark toostream adatok vagy a HDInsight a DStreams Apache Kafka abból. A példa hello Spark-fürtön futó Jupyter notebook.
> [!NOTE]
> hello jelen dokumentumban leírt lépések, amely tartalmazza a Spark on HDInsight és egy HDInsight-fürt Kafka Azure erőforráscsoport-csoport létrehozása. Ezeken a fürtökön hello Kafka fürt kommunikálnak mindkét egy Azure virtuális hálózatot, amely lehetővé teszi a Spark-fürt toodirectly hello belül található.
>
> Amikor elkészült, a jelen dokumentumban leírt lépések hello, ne felejtse el toodelete hello fürtök tooavoid felesleges költségek.

## <a name="create-hello-clusters"></a>Hello fürtök létrehozása

A HDInsight Apache Kafka nem biztosít hozzáférést toohello Kafka brókerek képest hello a nyilvános internethez. Kafka fürt hello, amelyeket tooKafka kell lennie a megbeszélések hello hello csomópontként azonos Azure virtuális hálózatban. Ehhez a példához hello Kafka, mind a Spark-fürtök találhatók egy Azure virtuális hálózatra. a következő ábra azt mutatja be, hogyan kommunikációs hello fürtök között zajló kommunikációról hello:

![Spark és Kafka fürtök egy Azure virtuális hálózatban ábrája](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> Bár az maga Kafka korlátozott toocommunication hello virtuális hálózaton belül, más szolgáltatásokkal hello fürtön például az SSH és az Ambari keresztül is elérhető hello internet. Nyilvános és a HDInsight együttes rendelkezésre álló hello-portok további információkért lásd: [portok és a HDInsight által használt URI-azonosítók](hdinsight-hadoop-port-settings-for-services.md).

Bár manuálisan is létrehozhat egy Azure virtuális hálózatra, Kafka és Spark fürtök, ez a beállítás könnyebb toouse Azure Resource Manager-sablonok. Használjon hello alábbi lépéseit toodeploy egy Azure virtuális hálózatra, Kafka, és a Spark-fürtök tooyour Azure-előfizetés.

1. A tooAzure gomb toosign és hello Azure-portál megnyitása hello sablon hello használata.
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v2.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy tooAzure"></a>
    
    hello Azure Resource Manager sablon itt található: **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v2.1.json**.

    > [!WARNING]
    > a HDInsight Kafka tooguarantee rendelkezésre állását, a fürt tartalmaznia kell legalább három munkavégző csomópontokhoz. Ez a sablon a három munkavégző csomópontokhoz tartalmaz Kafka fürtöt hoz létre.

    Ez a sablon egy HDInsight 3.6 fürtöt Kafka és Spark hoz létre.

2. Információk toopopulate hello tételek követően hello használata hello **egyéni telepítési** panel:
   
    ![HDInsight egyéni központi telepítés](./media/hdinsight-apache-spark-with-kafka/parameters.png)
   
    * **Erőforráscsoport**: hozzon létre egy csoportot, vagy válasszon egy meglévőt. Ez a csoport hello HDInsight-fürtöt tartalmaz.

    * **Hely**: Válasszon egy helyet a földrajzi elhelyezkedés alapján Bezárás tooyou.

    * **Fürt neve kiinduló**: Ez az érték neveként hello alapszintű hello Spark és Kafka fürtök használható. Ha például **hdi** hoz létre a Spark, spark-hdi__ nevű és egy Kafka fürtön nevű **kafka-hdi**.

    * **A fürt bejelentkezési felhasználónevét**: hello rendszergazda felhasználóneve hello Spark és Kafka fürt.

    * **A fürt bejelentkezési jelszó**: hello rendszergazdai jelszóval hello Spark és Kafka fürt.

    * **SSH-felhasználónév**: hello SSH felhasználói toocreate hello Spark és Kafka fürt.

    * **SSH-jelszónak**: hello SSH felhasználó hello Spark és Kafka fürt hello jelszavát.

3. Olvasási hello **feltételek és kikötések**, majd válassza ki **toohello feltételek és kikötések fenti elfogadom**.

4. Végül ellenőrizze **PIN-kód toodashboard** majd **beszerzési**. Körülbelül 20 percet toocreate hello fürtök szükséges.

Létrehozása után a hello erőforrásokat, átirányított tooa panel hello fürtök és a webes irányítópult tartalmazó erőforráscsoport hello áll.

![Erőforráscsoport panel hello vnet és fürtök](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> Figyelje meg, hogy vannak-e a HDInsight-fürtök hello hello nevei **spark-BASENAME** és **kafka-BASENAME**, ahol BASENAME toohello sablon megadott hello nevét. Ezeket a neveket használja a későbbi lépésekben toohello fürtök kapcsolódáskor.

## <a name="use-hello-notebooks"></a>Hello notebookok használata

a jelen dokumentumban ismertetett hello például hello kód megtalálható [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka).

Hello kövesse hello `README.md` toocomplete fájl ebben a példában.

## <a name="delete-hello-cluster"></a>Hello fürt törlése

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Mivel a jelen dokumentumban leírt lépések hello létrehozása a fürtök hello azonos Azure-erőforráscsoportot, törölheti a hello erőforráscsoportja hello Azure-portálon. Hello-csoport törlése eltávolítja a hozta létre a következő a dokumentum, hello Azure virtuális hálózat és tárfiók hello fürtök által használt összes erőforrást.

## <a name="next-steps"></a>Következő lépések

Ebben a példában megtanulta, hogyan toouse Spark tooread és tooKafka írni. Használja a következő hivatkozások toodiscover hello más módokon toowork Kafka:

* [Ismerkedés az Apache Kafka a HDInsight-on](hdinsight-apache-kafka-get-started.md)
* [A HDInsight MirrorMaker toocreate Kafka másolatának használata](hdinsight-apache-kafka-mirroring.md)
* [Az Apache Storm használata a HDInsighton futó Kafkával](hdinsight-apache-storm-with-kafka.md)

