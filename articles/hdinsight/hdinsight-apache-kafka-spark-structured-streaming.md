---
title: "Spark strukturált Stream továbbítása Kafka - Azure HDInsight aaaApache |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Apache Spark (DStream) tooget streamadatok virtuális gépbe vagy onnan Apache Kafka. Ebben a példában a HDInsight Spark a Jupyter notebook használatával adatok folyamatos átviteléhez."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/09/2017
ms.author: larryfr
ms.openlocfilehash: 0837e8fc5ea314e644daed029d596feeb2b02c68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-spark-structured-streaming-with-kafka-preview-on-hdinsight"></a>Használja a hdinsight Spark strukturált Stream továbbítása Kafka (előzetes verzió)

Megtudhatja, hogyan toouse Apache Kafka on Azure HDInsight Spark strukturált Streaming tooread adatokat.

Strukturált Spark streaming olyan adatfolyam feldolgozása a Spark SQL épül. Lehetővé teszi, tooexpress adatfolyam számítások hello ugyanaz, mint a batch számítási statikus adatok. A strukturált Streaming további információkért lásd: hello [strukturált Streaming programozási útmutató [Alpha]](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html) az Apache.org webhelyen.

> [!IMPORTANT]
> Ebben a példában HDInsight 3.6 Spark 2.1 használni. Strukturált Streaming tekinthető __alpha__ Spark 2.1-es verziójának.
>
> hello jelen dokumentumban leírt lépések, amely tartalmazza a Spark on HDInsight és egy HDInsight-fürt Kafka Azure erőforráscsoport-csoport létrehozása. Ezeken a fürtökön hello Kafka fürt kommunikálnak mindkét egy Azure virtuális hálózatot, amely lehetővé teszi a Spark-fürt toodirectly hello belül található.
>
> Amikor elkészült, a jelen dokumentumban leírt lépések hello, ne felejtse el toodelete hello fürtök tooavoid felesleges költségek.

## <a name="create-hello-clusters"></a>Hello fürtök létrehozása

A HDInsight Apache Kafka nem biztosít hozzáférést toohello Kafka brókerek képest hello a nyilvános internethez. Kafka fürt hello, amelyeket tooKafka kell lennie a megbeszélések hello hello csomópontként azonos Azure virtuális hálózatban. Ehhez a példához hello Kafka, mind a Spark-fürtök találhatók egy Azure virtuális hálózatra. a következő ábra azt mutatja be, hogyan kommunikációs hello fürtök között zajló kommunikációról hello:

![Spark és Kafka fürtök egy Azure virtuális hálózatban ábrája](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> hello Kafka szolgáltatás korlátozott toocommunication hello virtuális hálózaton belül. Egyéb szolgáltatások hello fürtön, például az SSH és az Ambari, keresztül is elérhető hello internet. Nyilvános és a HDInsight együttes rendelkezésre álló hello-portok további információkért lásd: [portok és a HDInsight által használt URI-azonosítók](hdinsight-hadoop-port-settings-for-services.md).

Bár manuálisan is létrehozhat egy Azure virtuális hálózatra, Kafka és Spark fürtök, ez a beállítás könnyebb toouse Azure Resource Manager-sablonok. Használjon hello alábbi lépéseit toodeploy egy Azure virtuális hálózatra, Kafka, és a Spark-fürtök tooyour Azure-előfizetés.

1. A tooAzure gomb toosign és hello Azure-portál megnyitása hello sablon hello használata.
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v4.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy tooAzure"></a>
    
    hello Azure Resource Manager sablon itt található: **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v4.1.json**.

    Ez a sablon a következő erőforrások hello hoz létre:

    * Egy Kafka HDInsight 3.5-fürtön.
    * A Spark on HDInsight 3.6 fürtön.
    * Egy Azure virtuális hálózatot, amely tartalmazza a HDInsight-fürtök hello.

    > [!IMPORTANT]
    > hello strukturált adatfolyam notebook használt ebben a példában a Spark on HDInsight 3.6 igényel. Ha a HDInsight Spark egy korábbi verzióját használja, hibák merülnek fel hello notebook használatakor.

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

Létrehozása után a hello erőforrásokat, átirányított toohello erőforráscsoport panel áll.

![Erőforráscsoport panel hello vnet és fürtök](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> Figyelje meg, hogy vannak-e a HDInsight-fürtök hello hello nevei **spark-BASENAME** és **kafka-BASENAME**, ahol BASENAME toohello sablon megadott hello nevét. Ezeket a neveket használja a későbbi lépésekben toohello fürtök kapcsolódáskor.

## <a name="get-hello-kafka-brokers"></a>Hello Kafka brókerek beolvasása

hello ebben a példában kód csatlakozik toohello Kafka replikaszervező hello Kafka fürt gazdagépei. toofind hello Kafka broker gazdagépek, a következő PowerShell- vagy Bash példa hello használata:

```powershell
$creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"
$clusterName = Read-Host -Prompt "Enter hello Kafka cluster name"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$brokerHosts = $respObj.host_components.HostRoles.host_name
($brokerHosts -join ":9092,") + ":9092"
```

```bash
curl -u admin:$PASSWORD -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER" | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")'
```

> [!NOTE]
> Ez a példa vár `$PASSWORD` toocontain hello jelszó hello fürt bejelentkezési azonosítóhoz, és `$CLUSTERNAME` toocontain hello hello Kafka fürt nevét.
>
> Ez a példa hello [jq](https://stedolan.github.io/jq/) segédprogram tooparse adatokat hello JSON-dokumentum.

a kimeneti hello hasonló toohello a következő szöveget:

`wn0-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn1-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn2-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn3-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092`

Ezt az információt akkor menteni, mert a következő szakaszok a jelen dokumentum hello használatban van.

## <a name="get-hello-notebooks"></a>Hello notebookok beolvasása

a jelen dokumentumban ismertetett hello például hello kód megtalálható [https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming).

## <a name="upload-hello-notebooks"></a>Hello notebookok feltöltése

Lépéseket tooupload hello notebookok követően – hello projekt tooyour Spark on HDInsight-fürt hello használata:

1. A böngészőben csatlakozzon a toohello Jupyter notebook a Spark-fürtön. Hello a következő URL-címe, cserélje le `CLUSTERNAME` hello nevű a Kafka fürt:

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    Amikor a rendszer kéri, adja meg a hello fürt felhasználónevet (rendszergazda) és hello fürt létrehozásakor használt jelszót.

2. Hello felső jobb oldalán álló hello oldal, használja a hello __feltöltése__ gomb tooupload hello __adatfolyam-Twitter-üzeneteket-To_Kafka.ipynb__ toohello fájlfürt. Válassza ki __nyitott__ toostart hello feltöltése.

    ![Hello feltöltési gomb tooselect használja, és töltse fel a notebook](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-button.png)

    ![Hello KafkaStreaming.ipynb fájl kiválasztása](./media/hdinsight-apache-kafka-spark-structured-streaming/select-notebook.png)

3. Hello található __adatfolyam-Twitter-üzeneteket-To_Kafka.ipynb__ hello lista notebookok, és válassza ki a bejegyzést __feltöltése__ mellette gombra.

    ![Használjon hello Feltöltés gombra hello KafkaStreaming.ipynb bejegyzés tooupload mellett azt toohello notebook kiszolgáló](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-notebook.png)

4. Ismételje meg a 1-3 tooload hello __Spark-strukturált-adatfolyam-a-Kafka.ipynb__ notebookot.

## <a name="load-tweets-into-kafka"></a>Twitter-üzeneteket betölthető Kafka

Hello fájlok feltöltése után válassza a hello __adatfolyam-Twitter-üzeneteket-To_Kafka.ipynb__ bejegyzés tooopen hello notebookot. Kövesse hello hello notebook tooload Twitter-üzeneteket Kafka be.

## <a name="process-tweets-using-spark-structured-streaming"></a>Folyamat Twitter-üzeneteket Spark strukturált adatfolyam használata

Hello Jupyter Notebook kezdőlapját, válassza ki hello __Spark-strukturált-adatfolyam-a-Kafka.ipynb__ bejegyzés. Kövesse hello hello notebook tooload Twitter-üzeneteket a Spark strukturált Streaming használatával Kafka.

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta, hogyan toouse Spark strukturált Streaming, tekintse meg a következő dokumentumok toolearn további információk a Spark és Kafka hello:

* [Hogyan toouse Spark streamelési (DStream) rendelkező Kafka](hdinsight-apache-spark-with-kafka.md).
* [Indítsa el a Jupyter Notebook és a Spark on HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md)