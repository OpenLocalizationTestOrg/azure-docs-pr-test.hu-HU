---
title: "az Apache Storm és HBase aaaAnalyze érzékelőadatait |} Microsoft Docs"
description: "Megtudhatja, hogyan tooconnect tooApache Storm egy virtuális hálózattal. A HBase tooprocess érzékelőadatait az eseményközpontban lévő Storm használjon, és jelenítheti meg azt a D3.js."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: a9a1ac8e-5708-4833-b965-e453815e671f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/09/2017
ms.author: larryfr
ms.openlocfilehash: e54fe9ffc720b0089f90e302b24a9438bd43999a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-sensor-data-with-apache-storm-event-hub-and-hbase-in-hdinsight-hadoop"></a>Apache Storm, az Event Hubs és a HBase a HDInsight (Hadoop) érzékelőadatok elemzése

Megtudhatja, hogyan toouse Apache Storm a HDInsight tooprocess az Azure Event Hubs érzékelőadatait. hello adatok az Apache HBase a HDInsight majd tárolja, és ábrázolt D3.js használatával.

Ebben a dokumentumban használt hello Azure Resource Manager sablon bemutatja, hogyan toocreate több Azure-erőforrások erőforráscsoportban. hello sablont hoz létre egy Azure virtuális hálózatra, két HDInsight-fürtök (a Storm és HBase) és az Azure Web Apps. A valós idejű webes irányítópult node.js végrehajtásának automatikusan telepített toohello webalkalmazás.

> [!NOTE]
> a dokumentum és a jelen dokumentum példa hello adatokat HDInsight 3.6 verzió szükséges.
>
> Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Előfeltételek

* Azure-előfizetés.
* [NODE.js](http://nodejs.org/): használt toopreview hello webes irányítópult helyileg a fejlesztési környezetet az.
* [Java és hello JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): toodevelop hello Storm-topológia használt.
* [Maven](http://maven.apache.org/what-is-maven.html): használt toobuild és fordítási hello projekt.
* [Git](http://git-scm.com/): használt toodownload hello projektet a Githubról.
* Egy **SSH** ügyfél: tooconnect toohello Linux-alapú HDInsight-fürtök használt. További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).


> [!IMPORTANT]
> Nem kell egy meglévő HDInsight-fürtre. jelen dokumentumban leírt lépések hello hozza létre a következő erőforrások hello:
> 
> * Egy Azure virtuális hálózat
> * A Storm on HDInsight-fürt (Linux-alapú, két munkavégző csomópontokhoz)
> * HBase on HDInsight-fürt (Linux-alapú, két munkavégző csomópontokhoz)
> * Az Azure Web Apps, amelyen hello webes irányítópult

## <a name="architecture"></a>Architektúra

![architektúra ábrája](./media/hdinsight-storm-sensor-data-analysis/devicesarchitecture.png)

Ebben a példában a következő összetevők hello áll:

* **Az Azure Event Hubs**: az érzékelők összegyűjtött adatokat tartalmazza.
* **A HDInsight alatt futó Storm**: az Eseményközpont kiválasztásával valós idejű adatok feldolgozása biztosít.
* **A HDInsight HBase**: biztosítja a állandó NoSQL-adattár adatok feldolgozása a Storm után.
* **Az Azure virtuális hálózat szolgáltatás**: lehetővé teszi, hogy a biztonságos kommunikáció közötti hello HDInsight alatt futó Storm és HBase a HDInsight-fürtökön.
  
  > [!NOTE]
  > A virtuális hálózati hello Java HBase ügyfél API használata esetén szükséges. Nincs felfedve hello nyilvános átjáró a HBase fürtök keresztül. Hello lehetővé teszi a virtuális hálózaton történő telepítésekor a HBase és a Storm fürtök hello Storm fürthöz (vagy más rendszereit hello virtuális hálózaton) toodirectly ügyfél API-val HBase eléréséhez.

* **Irányítópult webhely**: egy példa-irányítópultot, amely valós idejű adatok diagramokat.
  
  * hello webhely node.js valósul meg.
  * [Socket.IO](http://socket.io/) hello Storm-topológia és hello webhely közötti valós idejű kommunikációhoz használatos.
    
    > [!NOTE]
    > Egy megvalósítási részletes Socket.io segítségével kommunikációra. Bármely kommunikációs keretrendszert, például a nyers websocket elemek vagy SignalR is használhatja.

  * [D3.js](http://d3js.org/) használt toograph hello toohello webhely küldött adatok van.

> [!IMPORTANT]
> Két fürt szükség, mert nem támogatott metódus toocreate egy HDInsight-fürt Storm és HBase.

hello topológia olvassa be az adatokat az Event Hubs hello segítségével [org.apache.storm.eventhubs.spout.EventHubSpout](http://storm.apache.org/releases/0.10.1/javadocs/org/apache/storm/eventhubs/spout/class-use/EventHubSpout.html) osztály és írt adatok hello használata HBase [org.apache.storm.hbase.bolt.HBaseBolt](https://storm.apache.org/releases/1.0.1/javadocs/org/apache/storm/hbase/bolt/HBaseBolt.html) az osztály. Kommunikáció a hello webhely végezhető el [socket.io-client.java](https://github.com/nkzawa/socket.io-client.java).

a következő diagram hello hello topológia elrendezését hello ismerteti:

![topológia ábrája](./media/hdinsight-storm-sensor-data-analysis/sensoranalysis.png)

> [!NOTE]
> Ez a diagram hello topológia egyszerűsített nézete. Az egyes összetevők példánya létrejön az Eseményközpont minden partícióján. Ezek a példányok elosztott hello hello fürt csomópontja, és az adatok az alábbiak szerint történik közöttük:
> 
> * Hello spout toohello elemző adatait az elosztott terhelésű.
> * Hello elemző toohello irányítópult és a HBase adatokból Eszközazonosító szerint vannak csoportosítva, így mindig a hello ugyanarra az eszközre érkező üzenetek flow toohello adott összetevő.

### <a name="topology-components"></a>Topológia összetevők

* **Event Hub Spout**: hello spout rendszer alatt futó Apache Storm-verziójú 0.10.0-s megadott és magasabb.
  
  > [!NOTE]
  > Ebben a példában használt hello Event Hub spout egy Storm on HDInsight-fürt verziószáma 3.5-ös vagy 3.6 igényel.

* **ParserBolt.java**: hello adatok hello spout által kibocsátott nyers JSON, és esetenként egynél több esemény is ki lesz adva egyszerre. Tartalmaz a bolt hello által kibocsátott hello adatok spout és elemez JSON üdvözlőüzenetére olvassa be. hello bolt több mezőt tartalmazó rekordot, majd hello adatok bocsát ki.
* **DashboardBolt.java**: Ez az összetevő bemutatja, hogyan toouse hello Socket.io ügyféloldali kódtára a Java toosend adatok valós idejű toohello webes irányítópulton.
* **nem-hbase.yaml**: hello helyi módban történő futtatásakor használt topológia meghatározása. A HBase-összetevők nem használ.
* **a hbase.yaml**: hello hello topológia hello fürtön futtatásakor használt topológia meghatározása. Az a HBase összetevőket használnak.
* **dev.Properties**: hello hello Event Hub spout, HBase bolt és Irányítópult-összetevők beállításait.

## <a name="prepare-your-environment"></a>A környezet előkészítése

Ebben a példában használata előtt létre kell hoznia egy Azure Eseményközponthoz, mely hello Storm-topológia olvassa be.

### <a name="configure-event-hub"></a>Az Event Hubs konfigurálása

Az Event Hubs hello adatforrás ehhez a példához. A következő lépéseket toocreate Eseményközpontban hello használata.

1. A hello [Azure-portálon](https://portal.azure.com), jelölje be **+ új** -> **az eszközök internetes hálózatát** -> **Event Hubs**.
2. A hello **létrehozása Namespace** csoportjában hajtsa végre a következő feladatok hello:
   
   1. Adjon meg egy **neve** hello névtérhez.
   2. Válasszon tarifacsomagot. **Alapszintű** elegendő-e ebben a példában.
   3. Jelölje be hello Azure **előfizetés** toouse.
   4. Válasszon ki egy meglévő erőforráscsoportot, vagy hozzon létre egy újat.
   5. Jelölje be hello **hely** az Eseményközpont hello.
   6. Válassza ki **PIN-kód toodashboard**, és kattintson a **létrehozása**.

3. Hello létrehozási folyamat befejeződik, a névtér az Event Hubs adatairól hello jelenik meg. Itt válassza **+ Hozzáadás Eseményközpont**. A hello **Eseményközpont létrehozása** területen adjon meg egy **sensordata**, majd válassza ki **létrehozása**. Hello többi mezőt hello alapértelmezett értéken hagyja.
4. Hello Eseményközpontokból származó tekintse meg a névtér, jelölje be **Event Hubs**. Jelölje be hello **sensordata** bejegyzés.
5. Hello sensordata Eseményközpont, válassza ki **megosztott elérési házirendek**. Használjon hello **+ Hozzáadás** hivatkozás tooadd hello következő házirendek:

    | Házirend neve | Jogcímek |
    | ----- | ----- |
    | eszközök | Küldés |
    | A Storm | Figyelés |

1. Válassza ki mindkét házirend, és jegyezze fel a hello **elsődleges kulcs** érték. A későbbi lépésekben mindkét házirend hello érték szükséges.

## <a name="download-and-configure-hello-project"></a>Töltse le és hello projekt konfigurálása

Használja a következő toodownload hello projektet a Githubról hello.

    git clone https://github.com/Blackmist/hdinsight-eventhub-example

Hello parancs befejezése után a következő könyvtárstruktúrát hello:

    hdinsight-eventhub-example/
        TemperatureMonitor/ - this contains hello topology
            resources/
                log4j2.xml - set logging toominimal.
                no-hbase.yaml - topology definition without hbase components.
                with-hbase.yaml - topology definition with hbase components.
            src/main/java/com/microsoft/examples/bolts/
                ParserBolt.java - parses JSON data into tuples
                DashboardBolt.java - sends data over Socket.IO toohello web dashboard.
        dashboard/nodejs/ - this is hello node.js web dashboard.
        SendEvents/ - utilities toosend fake sensor data.

> [!NOTE]
> Ez a dokumentum nem lép az ebben a példában szereplő hello kód toofull részleteit. Azonban hello kódot teljesen megjegyzésként.

tooconfigure hello projekt tooread az Eseményközpont, nyissa meg hello `hdinsight-eventhub-example/TemperatureMonitor/dev.properties` fájlt, és az Event Hubs információk toohello a következő sorokat:

```bash
eventhub.read.policy.name: your_read_policy_name
eventhub.read.policy.key: your_key_here
eventhub.namespace: your_namespace_here
eventhub.name: your_event_hub_name
eventhub.partitions: 2
```

## <a name="compile-and-test-locally"></a>Fordítsa le és helyi tesztelése

> [!IMPORTANT]
> Storm fejlesztői környezetben működő hello topológia helyileg a szükség van. További információkért lásd: [egy Storm-fejlesztési környezet létrehozása](http://storm.apache.org/releases/1.1.0/Setting-up-development-environment.html) az Apache.org webhelyen.

> [!WARNING]
> A Windows környezet használatakor jelenhet meg a `java.io.IOException` amikor helyileg futó hello topológia. Ha igen, helyezze át a toorunning hello topológia a HDInsight.

Előtt nem hello irányítópult tooview hello kimeneti hello topológia start és az Event Hubs adatok toostore létrehozni.

> [!IMPORTANT]
> Ez a topológia hello HBase összetevő állapota nem aktív, helyi tesztelése során. Java API hello hello HBase fürt nem érhető el külső hello Azure Virtual Network hello fürtöt tartalmaz.

### <a name="start-hello-web-application"></a>Hello webalkalmazás elindítása

1. Nyisson meg egy parancssort, és módosítsa a könyvtárat túl`hdinsight-eventhub-example/dashboard`. A következő parancs tooinstall hello függőségek hello webes alkalmazás által igényelt hello használata:
   
    ```bash
    npm install
    ```

2. A következő parancs toostart hello webalkalmazás hello használata:
   
    ```bash
    node server.js
    ```
   
    Megjelenik egy üzenet hasonló toohello, a következő szöveget:
   
        Server listening at port 3000

3. Nyisson meg egy webböngészőt, és írja be `http://localhost:3000/` hello címként. A kép a következő lap hasonló toohello jelenik meg:
   
    ![webes irányítópult](./media/hdinsight-storm-sensor-data-analysis/emptydashboard.png)
   
    Hagyja megnyitva a parancssort. A vizsgálatot követően használja a Ctrl-C toostop hello webkiszolgáló.

### <a name="generate-data"></a>Adatok létrehozása

> [!NOTE]
> hello ebben a szakaszban található lépéseket használata Node.js, hogy bármilyen platformon is használhatók. Más nyelv, tekintse meg a hello `SendEvents` könyvtár.

1. Nyisson meg egy új parancssor, rendszerhéjat vagy terminált, és módosítsa a könyvtárat túl`hdinsight-eventhub-example/SendEvents/nodejs`. hello alkalmazásnak szüksége tooinstall hello függőségek hello a következő parancsot használja:

    ```bash
    npm install
    ```

2. Nyissa meg hello `app.js` fájlt egy szövegszerkesztőben, és hello korábban beszerzett Eseményközpont információkat:
   
    ```javascript
    // ServiceBus Namespace
    var namespace = 'YourNamespace';
    // Event Hub Name
    var hubname ='sensordata';
    // Shared access Policy name and key (from Event Hub configuration)
    var my_key_name = 'devices';
    var my_key = 'YourKey';
    ```
   
   > [!NOTE]
   > Ebben a példában feltételezzük, hogy használt `sensordata` hello az Eseményközpont neveként. És hogy `devices` hello neveként hello házirend, amely rendelkezik egy `Send` jogcímek.

3. A következő parancs tooinsert új bejegyzést az Event Hubs hello használata:
   
    ```bash
    node app.js
    ```
   
    Kimeneti hello adatokat tartalmazó több sornyi küldött tooEvent Hub jelenik meg:
   
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"0","Temperature":7}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"1","Temperature":39}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"2","Temperature":86}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"3","Temperature":29}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"4","Temperature":30}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"5","Temperature":5}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"6","Temperature":24}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"7","Temperature":40}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"8","Temperature":43}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"9","Temperature":84}

### <a name="build-and-start-hello-topology"></a>Hozza létre, és indítsa el a hello topológia

1. Nyisson meg egy új parancssort, és módosítsa a könyvtárat túl`hdinsight-eventhub-example/TemperatureMonitor`. toobuild és a csomag hello topológia, a következő parancs hello használata: 

    ```bash
    mvn clean package
    ```

2. toostart hello topológia helyi módban, a következő parancs használata hello:

    ```bash
    storm jar target/TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local --filter dev.properties resources/no-hbase.yaml
    ```

    * `--local`hello topológia helyi módban indul.
    * `--filter`felhasználási hello `dev.properties` toopopulate paraméterek hello topológia meghatározása a fájl.
    * `resources/no-hbase.yaml`felhasználási hello `no-hbase.yaml` topológia meghatározása.
 
   Miután elindult, hello topológia olvassa be az Eseményközpont bejegyzéseket, és elküldi azokat a helyi gépen futó toohello irányítópult. Sorok jelennek meg hello webes irányítópult a következő kép hasonló toohello kell megjelennie:
   
    ![Irányítópult adatokkal](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

2. Hello irányítópult futása közben, használja a hello `node app.js` hello előző parancsot toosend új adatok tooEvent hubok lépések. Hello hőmérséklet értékek véletlenszerűen jönnek létre, mert a hello graph tooshow nagy változások hőmérséklet frissítenie kell.
   
   > [!NOTE]
   > Hello kell **hdinsight – az eventhub-példa/SendEvents/Nodejs** directory hello használatakor `node app.js` parancsot.

3. Az irányítópult hello frissíti, stop hello topológiájának megtekintését a Ctrl + C ellenőrzése után. Ctrl + C toostop hello helyi webkiszolgáló is használható.

## <a name="create-a-storm-and-hbase-cluster"></a>A Storm és HBase-fürt létrehozása

Ez a szakasz használata a lépések hello egy [Azure Resource Manager sablon](../azure-resource-manager/resource-group-template-deploy.md) toocreate egy Azure virtuális hálózat és a Storm és HBase fürt hello virtuális hálózaton. hello sablon is létrehoz az Azure Web Apps, és bele hello irányítópult másolatának telepíti.

> [!NOTE]
> Egy virtuális hálózati szolgál, hogy a futó Storm-fürt hello hello topológia közvetlenül kommunikálhatnak hello HBase-fürtöt hello HBase Java API használatával.

a dokumentumban használt hello Resource Manager-sablon a következő nyilvános blobtárolóban található **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet-3.6.json**.

1. Kattintson a következő gombra toosign tooAzure a és a nyitott hello Resource Manager sablon hello Azure-portálon hello.
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-storm-cluster-in-vnet-3.6.json" target="_blank"><img src="./media/hdinsight-storm-sensor-data-analysis/deploy-to-azure.png" alt="Deploy tooAzure"></a>

2. A hello **egyéni telepítési** területen adja meg a következő értékek hello:
   
    ![A HDInsight-paraméterek](./media/hdinsight-storm-sensor-data-analysis/parameters.png)
   
   * **Fürt neve kiinduló**: Ez az érték neveként hello alapszintű hello Storm és HBase fürtök használható. Ha például **abc** hoz létre egy Storm-fürt nevű **storm-abc** és nevű HBase-fürtöt **hbase-abc**.
   * **A fürt bejelentkezési felhasználónevét**: hello rendszergazda felhasználóneve hello Storm és HBase fürt.
   * **A fürt bejelentkezési jelszó**: hello rendszergazdai jelszóval hello Storm és HBase fürt.
   * **SSH-felhasználónév**: hello SSH felhasználói toocreate hello Storm és HBase fürt.
   * **SSH-jelszónak**: hello SSH felhasználó hello Storm és HBase fürt hello jelszavát.
   * **Hely**: hello fürtök létrehozott hello régióban.
     
     Kattintson a **OK** toosave hello paraméterek.

3. Használjon hello **alapjai** szakasz toocreate egy erőforráscsoportot, vagy válasszon egy meglévőt.
4. A hello **erőforráscsoport helye** legördülő menüben válassza hello ugyanazon a helyen, a kiválasztott hello **hely** hello paraméterének **beállítások** szakasz.
5. Olvassa el a hello használati feltételeket, és válassza ki **toohello feltételek és kikötések fenti elfogadom**.
6. Végül ellenőrizze **PIN-kód toodashboard** majd **beszerzési**. Körülbelül 20 percet toocreate hello fürtök szükséges.

Létrehozása után a hello erőforrásokat, hello erőforráscsoport információkat jelenít meg.

![Erőforráscsoport hello vnet és fürtök](./media/hdinsight-storm-sensor-data-analysis/groupblade.png)

> [!IMPORTANT]
> Figyelje meg, hogy vannak-e a HDInsight-fürtök hello hello nevei **storm-BASENAME** és **hbase-BASENAME**, ahol BASENAME toohello sablon megadott hello nevét. Ezeket a neveket az toohello fürtök kapcsolódáskor használni egy későbbi lépésben. Vegye figyelembe, hogy hello hello irányítópult hely nevét a rendszer **basename-irányítópult**. Ez az érték használható a dokumentum későbbi szakaszában.

## <a name="configure-hello-dashboard-bolt"></a>Hello irányítópult bolt konfigurálása

toosend a webes alkalmazás központilag telepített adatok toohello irányítópult, módosítania kell a következő sort a hello hello `dev.properties`fájlt:

```yaml
dashboard.uri: http://localhost:3000
```

Változás `http://localhost:3000` túl`http://BASENAME-dashboard.azurewebsites.net` és hello fájl mentéséhez. Cserélje le **BASENAME** hello előző lépésben megadott hello Alap nevű. Hello tooselect hello irányítópult és nézet hello URL-címet korábban létrehozott erőforráscsoportot is használható.

## <a name="create-hello-hbase-table"></a>Hello HBase tábla létrehozása

toostore adatok a HBase, azt először létre kell hoznia egy tábla. Előzetes létrehozása, amelyet a Storm a toowrite az erőforrásokat, közben toocreate erőforráshoz, a Storm-topológia több példányt próbált eredményezhet, toocreate hello ugyanazt az erőforrást. Hozzon létre hello erőforrások hello topológia kívül, és a Storm olvasási/írási és elemzés.

1. SSH tooconnect toohello HBase-fürtöt használ hello SSH-felhasználó és a fürt létrehozása során a toohello sablon megadott jelszó segítségével. Például, ha a kapcsolódás hello használatával `ssh` parancs szintaxisa a következő hello szeretné használni:
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```
   
    Cserélje le `sshuser` az SSH-felhasználónév hello hello fürt létrehozásakor megadott. Cserélje le `clustername` hello HBase fürt névvel.

2. Hello SSH-munkamenetet a start hello HBase rendszerhéjból.
   
    ```bash
    hbase shell
    ```
   
    Miután hello rendszerhéj be van töltve, megjelenik egy `hbase(main):001:0>` kérdés.

3. Hello HBase rendszerhéj adja meg a következő parancs toocreate egy tábla toostore hello érzékelőadatait hello:
   
    ```hbase
    create 'SensorData', 'cf'
    ```

4. Győződjön meg arról, hogy hello tábla hello a következő parancs használatával jött létre:
   
    ```hbase
    scan 'SensorData'
    ```
   
    Ez visszaadja a hasonló toohello információkat a következő példa, amely azt jelzi, hogy nincsenek-e 0 sor hello tábla.
   
        ROW                   COLUMN+CELL                                       0 row(s) in 0.1900 seconds

5. Adja meg `exit` tooexit hello HBase rendszerhéj:

## <a name="configure-hello-hbase-bolt"></a>Hello HBase bolt konfigurálása

a Storm-fürt hello tooHBase toowrite, meg kell adnia hello HBase bolt hello konfigurációs részleteit a HBase-fürtöt.

1. A következő példák tooretrieve hello Zookeeper a HBase fürt kvórumát hello egyikét használja:

    ```bash
    CLUSTERNAME='your_HDInsight_cluster_name'
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/HBASE/components/HBASE_MASTER" | jq '.metrics.hbase.master.ZookeeperQuorum'
    ```

    > [!NOTE]
    > Cserélje le `your_HDInsight_cluster_name` hello nevet, a HDInsight-fürthöz. Hello telepítéséről további információt `jq` segédprogram, lásd: [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/).
    >
    > Amikor a rendszer kéri, adja meg hello jelszó hello HDInsight rendszergazdai bejelentkezés.

    ```powershell
    $clusterName = 'your_HDInsight_cluster_name`
    $creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HBASE/components/HBASE_MASTER" -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.metrics.hbase.master.ZookeeperQuorum
    ```

    > [!NOTE]
    > Cserélje le a(z) your_HDInsight_cluster_name hello nevet, a HDInsight-fürthöz. Amikor a rendszer kéri, adja meg hello jelszó hello HDInsight rendszergazdai bejelentkezés.
    >
    > Ebben a példában az Azure PowerShell szükséges. További információ az Azure PowerShell használatával, lásd: [Ismerkedés az Azure PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/Getting-Started-with-Windows-PowerShell?view=powershell-6)

    Ezekben a példákban által visszaadott hello információkat a következő szöveg hasonló toohello:

    `zk2-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181,zk0-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181,zk3-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181`

    Ez az információ Storm toocommunicate hello HBase fürt használja.

2. Módosítsa a hello `dev.properties` fájlt, és hello Zookeeper kvórum információk toohello a következő sort:

    ```yaml
    hbase.zookeeper.quorum: your_hbase_quorum
    ```

## <a name="build-package-and-deploy-hello-solution-toohdinsight"></a>Hozza létre, csomagot, és hello megoldás tooHDInsight telepítése

A fejlesztői környezetben használja a következő lépéseket toodeploy hello Storm topology toohello storm-fürt hello.

1. A hello `TemperatureMonitor` könyvtárába, használjon hello következő tooperform új buildverziót parancsot, és a projektből JAR-csomag létrehozása:
   
        mvn clean package
   
    Ez a parancs létrehoz egy nevű fájlt `TemperatureMonitor-1.0-SNAPSHOT.jar in hello `cél "mappában található a projekthez.

2. Használjon scp tooupload hello `TemperatureMonitor-1.0-SNAPSHOT.jar` és `dev.properties` fájlok tooyour Storm-fürt. Hello a következő példában cserélje le `sshuser` hello fürt létrehozásakor megadott hello SSH felhasználóval és `clustername` hello nevet, a Storm-fürt. Amikor a rendszer kéri, adja meg hello jelszó hello SSH felhasználó.
   
    ```bash
    scp target/TemperatureMonitor-1.0-SNAPSHOT.jar dev.properties sshuser@clustername-ssh.azurehdinsight.net:
    ```

   > [!NOTE]
   > Tooupload hello fájlok több percig is tarthat.

    További információk hello az `scp` és `ssh` parancsok a hdinsight eszközzel, lásd: [az SSH a Hdinsighttal](./hdinsight-hadoop-linux-use-ssh-unix.md)

3. Hello fájl a feltöltést követően csatlakozzon az SSH használatával toohello Storm-fürt.
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    Cserélje le `sshuser` hello SSH felhasználónévvel. Cserélje le `clustername` hello Storm-fürt névvel.

4. toostart hello topológia, a következő parancsot a hello SSH-munkamenetet hello használata:
   
    ```bash
    storm jar TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote --filter dev.properties -R /with-hbase.yaml
    ```

    * `--remote`elküldi a hello topológia toohello Nimbus szolgáltatást, amely elküldi toohello felügyelő hello fürt csomópontja.
    * `--filter`felhasználási hello `dev.properties` toopopulate paraméterek hello topológia meghatározása a fájl.
    * `-R /with-hbase.yaml`felhasználási hello `with-hbase.yaml` topológia hello csomagban található.

5. Miután hello topológia elindult, nyisson meg egy böngésző toohello webhelyen közzétett Azure, majd használja hello `node app.js` parancs toosend adatok tooEvent központ. Hello webes irányítópult frissítés toodisplay hello adatokat kell megjelennie.
   
    ![irányítópult](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

## <a name="view-hbase-data"></a>A HBase-adatok megtekintése

A következő lépéseket tooconnect tooHBase hello használja, és győződjön meg arról, hogy hello adatok írt toohello táblázat:

1. SSH tooconnect toohello HBase-fürtöt használ.
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    Cserélje le `sshuser` hello SSH felhasználónévvel. Cserélje le `clustername` hello HBase fürt névvel.

2. Hello SSH-munkamenetet a start hello HBase rendszerhéjból.
   
    ```bash
    hbase shell
    ```
   
    Miután hello rendszerhéj be van töltve, megjelenik egy `hbase(main):001:0>` kérdés.

3. Hello tábla azon sorait megtekintése:
   
    ```hbase
    scan 'SensorData'
    ```
   
    Ez a parancs visszaadja a hasonló toohello információkat a következő szöveg, jelezve, hogy van-e adatok hello tábla.
   
        hbase(main):002:0> scan 'SensorData'
        ROW                             COLUMN+CELL
        \x00\x00\x00\x00               column=cf:temperature, timestamp=1467290788277, value=\x00\x00\x00\x04
        \x00\x00\x00\x00               column=cf:timestamp, timestamp=1467290788277, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x01               column=cf:temperature, timestamp=1467290788348, value=\x00\x00\x00M
        \x00\x00\x00\x01               column=cf:timestamp, timestamp=1467290788348, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x02               column=cf:temperature, timestamp=1467290788268, value=\x00\x00\x00R
        \x00\x00\x00\x02               column=cf:timestamp, timestamp=1467290788268, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x03               column=cf:temperature, timestamp=1467290788269, value=\x00\x00\x00#
        \x00\x00\x00\x03               column=cf:timestamp, timestamp=1467290788269, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x04               column=cf:temperature, timestamp=1467290788356, value=\x00\x00\x00>
        \x00\x00\x00\x04               column=cf:timestamp, timestamp=1467290788356, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x05               column=cf:temperature, timestamp=1467290788326, value=\x00\x00\x00\x0D
        \x00\x00\x00\x05               column=cf:timestamp, timestamp=1467290788326, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x06               column=cf:temperature, timestamp=1467290788253, value=\x00\x00\x009
        \x00\x00\x00\x06               column=cf:timestamp, timestamp=1467290788253, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x07               column=cf:temperature, timestamp=1467290788229, value=\x00\x00\x00\x12
        \x00\x00\x00\x07               column=cf:timestamp, timestamp=1467290788229, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x08               column=cf:temperature, timestamp=1467290788336, value=\x00\x00\x00\x16
        \x00\x00\x00\x08               column=cf:timestamp, timestamp=1467290788336, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x09               column=cf:temperature, timestamp=1467290788246, value=\x00\x00\x001
        \x00\x00\x00\x09               column=cf:timestamp, timestamp=1467290788246, value=2015-02-10T14:43.05.00320Z
        10 row(s) in 0.1800 seconds
   
   > [!NOTE]
   > A vizsgálati művelet legfeljebb 10 sor hello táblát ad vissza.

## <a name="delete-your-clusters"></a>A fürtök törlése

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

toodelete hello fürt, tárolás és a webes alkalmazás egyszerre, törölje a hello csoportot, amely tartalmazza azokat.

## <a name="next-steps"></a>Következő lépések

A HDInsight Storm-topológiák további példákért lásd [a HDInsight alatt futó Storm](hdinsight-storm-example-topology.md)

Apache Storm kapcsolatos további információkért lásd: hello [alatt futó Apache Storm](https://storm.incubator.apache.org/) hely.

A HDInsight HBase kapcsolatos további információkért lásd: hello [HBase a HDInsight áttekintése](hdinsight-hbase-overview.md).

Socket.io kapcsolatos további információkért lásd: hello [socket.io](http://socket.io/) hely.

D3.js kapcsolatos további információkért lásd: [D3.js - adatokat tartalmazó dokumentumok vezérelt](http://d3js.org/).

A Java topológiák létrehozásával kapcsolatos további információkért lásd: [HDInsight alatt futó Apache Storm topológiák fejlesztése Java](hdinsight-storm-develop-java-topology.md).

A .NET topológiák létrehozásával kapcsolatos további információkért lásd: [fejlesztésére C#-topológiák futó Apache stormra a Visual Studio használatával HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).

[azure-portal]: https://portal.azure.com
