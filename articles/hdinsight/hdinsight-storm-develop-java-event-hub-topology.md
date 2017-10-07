---
title: "az Event Hubs Java használatával futó Storm aaaProcess események |} Microsoft Docs"
description: "Ismerje meg, hogyan tooprocess Event Hubs adatok Java Storm-topológia Maven létre."
services: hdinsight,notification hubs
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 453fa7b0-c8a6-413e-8747-3ac3b71bed86
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/13/2017
ms.author: larryfr
ms.openlocfilehash: 6506f5bc8f6ab0e29350c071a3f84433382038e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="process-events-from-azure-event-hubs-with-storm-on-hdinsight-java"></a>Az Azure Event Hubs (Java) futó Storm eseményeinek

Megtudhatja, hogyan toouse Azure Event Hubs a HDInsight alatt futó Storm. Ebben a példában az Azure Event Hubs Java-alapú összetevők tooread és írási adatait használja.

Az Azure Event Hubs lehetővé teszi nagy mennyiségű tooprocess webhelyek, alkalmazások és eszközök adatait. hello Event Hub spout teszi, hogy könnyen toouse Apache Storm a HDInsight tooanalyze ezek az adatok valós időben. Az Event Hubs boltok hello segítségével a Storm is írhat adatok tooEvent hubok.

## <a name="prerequisites"></a>Előfeltételek

* Egy HDInsight-fürt verziószáma 3.6 Apache Storm. További információkért lásd: [Ismerkedés a Storm on HDInsight-fürt](hdinsight-apache-storm-tutorial-get-started-linux.md).

    > [!IMPORTANT]
    > Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Egy [Azure Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).

* [Oracle Java fejlesztői készlet (JDK) 8-as verzió](http://www.oracle.com/technetwork/java/javase/downloads/index.html) vagy ezzel egyenértékű, például [OpenJDK](http://openjdk.java.net/).

* [Maven](https://maven.apache.org/download.cgi): Maven rendszer project build Java-projektek esetében.

* Szöveg- vagy integrált fejlesztési környezeti (IDE).

    > [!NOTE]
    > A szerkesztő vagy az IDE lehet bizonyos funkciók nem ebben a dokumentumban tárgyalt Maven való munkához. A szerkesztési környezetben hello képességekre vonatkozó információ: hello hello-termék dokumentációját.

    * Egy SSH-ügyfél. További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

* Hello `ssh` és `scp` parancsok. Ezek a használt toocopy fájlok toohello HDInsight-fürthöz. A Windows ezek a Bash Windows 10 rendszeren keresztül kaphat.

## <a name="understanding-hello-example"></a>Understanding hello – példa

Hello [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) példa két topológiát tartalmaz:

Hello `resources/writer.yaml` topológia véletlenszerű adatokat tooan Azure Event Hubs ír. hello által előállított hello adatokat `DeviceSpout` összetevő, és egy eszköz érték pedig véletlenszerű Eszközazonosító. Ezért, van néhány hardver, amely egy karakterlánc-azonosító és a numerikus értéket bocsát ki szimulálva.

Ezekkel `resources/reader.yaml` topológia beolvassa az adatokat az Event Hubs (hello EventHubWriter, által írt adatok) hello JSON-adatokat elemez és naplózza hello `deviceId` és `deviceValue` adatokat.

hello adatok formázott JSON-dokumentumként előtt tooEvent Hub írása, és ha olvasni hello olvasó elemzi azt JSON kívüli és rekordokat. hello JSON formátum a következőképpen történik:

    { "deviceId": "unique identifier", "deviceValue": some value }

### <a name="project-configuration"></a>Projekt konfigurálása

Hello `POM.xml` fájl a Maven project konfigurációs információkat tartalmaz. hello érdekes van:

#### <a name="event-hub-components"></a>Event Hub összetevők

hello komponenst, amely beolvassa és tooAzure Event Hubs található hello [HDInsight tárház](https://github.com/hdinsight/mvn-rep). következő részeiben arról olvashat hello hello `POM.xml` terhelés hello összetevők fájl ebben a tárházban lévő

```xml
<repositories>
    <repository>
        <id>hdinsight-examples</id>
        <url>http://raw.github.com/hdinsight/mvn-repo/master</url>
    </repository>
</repositories>
```

#### <a name="hello-eventhubs-storm-spout-dependency"></a>hello EventHubs Storm Spout függőség

```xml
<dependency>
    <groupId>com.microsoft</groupId>
    <artifactId>eventhubs</artifactId>
    <version>${storm.eventhub.version}</version>
</dependency>
```

Az XML-kód hello eventhubs csomag, amely tartalmaz egy spout az Event Hubs-ből történő olvasáshoz és tooit írásához. a bolt függőséget határoz meg.

```xml
</source>
    <target>1.8</target>
    </configuration>
</plugin>
```

Az XML-kód hello projekt toogenerate kimeneti HDInsight 3.5-ös vagy újabb verzióját használja 8, Java konfigurálja.

#### <a name="hello-maven-shade-plugin"></a>hello maven-árnyalatát-beépülő modul

```xml
<!-- build an uber jar -->
<plugin>
<groupId>org.apache.maven.plugins</groupId>
<artifactId>maven-shade-plugin</artifactId>
<version>2.3</version>
<configuration>
    <transformers>
    <!-- Keep us from getting a can't overwrite file error -->
    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer"/>
    <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer"/>
    </transformers>
    <!-- Keep us from getting a bad signature error -->
    <filters>
    <filter>
        <artifact>*:*</artifact>
        <excludes>
            <exclude>META-INF/*.SF</exclude>
            <exclude>META-INF/*.DSA</exclude>
            <exclude>META-INF/*.RSA</exclude>
        </excludes>
    </filter>
    </filters>
</configuration>
<executions>
    <execution>
    <phase>package</phase>
    <goals>
        <goal>shade</goal>
    </goals>
    </execution>
</executions>
</plugin>
```

Az XML-kód hello megoldás toopackage hello kimeneti be egy uber jar konfigurálja. hello jar hello projekt kód és a szükséges függőségek is tartalmazza. Azt is szolgál:

* Nevezze át a licencfájlok hello függőségek.
* Zárja ki a biztonsági/aláírások.
* Győződjön meg arról, hogy többféle megvalósítása hello azonos felület egyesülnek egy bejegyzést.

Ezeket a konfigurációs beállításokat futásidőben megelőzése érdekében.

#### <a name="topology-definitions"></a>Topológia definíciók

Ez a példa hello [fluxus](https://storm.apache.org/releases/1.1.0/flux.html) keretrendszer. Ezt a keretrendszert YAM toodefine hello topológiák használja. hello fő előnye, hogy még nem rögzített kódolási hello topológia a Java-kódot. Mivel hello definition YAM, anélkül, hogy toorecompile mindent hello topológia küldés előtt módosíthatja.

__Writer.yaml__:

```yaml
---
# Topology that reads from Event Hubs
name: "eventhubwriter"

components:
  # Configure hello Event Hub spout
  - id: "eventhubbolt-config"
    className: "org.apache.storm.eventhubs.bolt.EventHubBoltConfig"
    constructorArgs:
      # These are populated from hello .properties file when hello topology is started
      - "${eventhub.write.policy.name}"
      - "${eventhub.write.policy.key}"
      - "${eventhub.namespace}"
      - "servicebus.windows.net"
      - "${eventhub.name}"

spouts:
  - id: "device-emulator-spout"
    className: "com.microsoft.example.DeviceSpout"
    parallelism: ${eventhub.partitions}

bolts:
  - id: "eventhub-bolt"
    className: "org.apache.storm.eventhubs.bolt.EventHubBolt"
    constructorArgs:
      - ref: "eventhubbolt-config" # config declared in components section
    # parallelism hint. This should be hello same as hello number of partitions for your Event Hub, so we read it from hello dev.properties file passed at run time.
    parallelism: ${eventhub.partitions}

  # Log information
  - id: "log-bolt"
    className: "org.apache.storm.flux.wrappers.bolts.LogInfoBolt"
    parallelism: 1

# How data flows through hello components
streams:
  - name: "spout -> eventhub" # just a string used for logging
    from: "device-emulator-spout"
    to: "eventhub-bolt"
    grouping:
        type: SHUFFLE

  - name: "spout -> logger"
    from: "device-emulator-spout"
    to: "log-bolt"
    grouping:
        type: SHUFFLE
```

__Reader.yaml__:

```yaml
---
# Topology that reads from Event Hubs
name: "eventhubreader"

components:
  # Configure hello Event Hub spout
  - id: "eventhubspout-config"
    className: "org.apache.storm.eventhubs.spout.EventHubSpoutConfig"
    constructorArgs:
      # These are populated from hello .properties file when hello topology is started
      - "${eventhub.read.policy.name}"
      - "${eventhub.read.policy.key}"
      - "${eventhub.namespace}"
      - "${eventhub.name}"
      - ${eventhub.partitions}

spouts:
  - id: "eventhub-spout"
    className: "org.apache.storm.eventhubs.spout.EventHubSpout"
    constructorArgs:
      - ref: "eventhubspout-config" # config declared in components section
    # parallelism hint. This should be hello same as hello number of partitions for your Event Hub, so we read it from hello dev.properties file passed at run time.
    parallelism: ${eventhub.partitions}

bolts:
  # Log information
  - id: "log-bolt"
    className: "org.apache.storm.flux.wrappers.bolts.LogInfoBolt"
    parallelism: 1

  # Parses from JSON into tuples
  - id: "parser-bolt"
    className: "com.microsoft.example.ParserBolt"
    parallelism: ${eventhub.partitions}

# How data flows through hello components
streams:
  - name: "spout -> parser" # just a string used for logging
    from: "eventhub-spout"
    to: "parser-bolt"
    grouping:
        type: SHUFFLE

  - name: "parser -> log-bolt"
    from: "parser-bolt"
    to: "log-bolt"
    grouping:
        type: SHUFFLE
```

#### <a name="tell-hello-topology-about-event-hub"></a>Adja az Event Hubs hello topológia

Futási időben hello `dev.properties` fájl használt toopass hello Eseményközpont toohello topológiája. hello következő példája hello alapértelmezett hello fájl tartalma:

```yaml
eventhub.write.policy.name: writer
eventhub.write.policy.key: your_key_here
eventhub.read.policy.name: reader
eventhub.read.policy.key: your_key_here
eventhub.namespace: your_namespace_here
eventhub.name: storm
eventhub.partitions: 2
```

## <a name="configure-environment-variables"></a>Környezeti változók konfigurálása

hello következő környezeti változó lehet beállítani a fejlesztő munkaállomás Java és hello JDK telepítésekor. Azonban ellenőrizni kell, hogy léteznek, illetve hello a rendszer tartozó helyes értékeket tartalmaz.

* **JAVA_HOME** -hello Java-futtatókörnyezet (JRE) futtató toohello directory kell mutatnia. Például egy Unix vagy Linux terjesztési nem rendelkezhet hasonló érték túl`/usr/lib/jvm/java-7-oracle`. A Windows rendszerben kellene hasonló érték túl`c:\Program Files (x86)\Java\jre1.7`
* **Elérési út** -elérési utak a következő hello tartalmaznia kell:

  * **JAVA_HOME** (vagy ezzel egyenértékű elérési hello)
  * **JAVA_HOME\bin** (vagy ezzel egyenértékű elérési hello)
  * hello mappát, ahová a Maven telepítve van

## <a name="configure-event-hub"></a>Az Event Hubs konfigurálása

Az Event Hubs hello adatforrás ehhez a példához. A következő lépéseket toocreate egy Eseményközpont hello használata.

1. A hello [klasszikus Azure portál](https://manage.windowsazure.com), jelölje be **új** > **Service Bus** > **Eseményközpont**  >  **Egyéni létrehozás**.

2. A hello **hozzáadása egy új Eseményközpont** képernyőn, adjon meg egy **Eseményközpont neveként**. Jelölje be hello **régió** toocreate hello csomópontjában, majd hozzon létre egy névteret vagy válasszon egy meglévőt. Végül kattintson a hello **nyíl** toocontinue.

    ![1. varázslólap](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz1.png)

   > [!NOTE]
   > Válassza ki az azonos hello **hely** , a Storm on HDInsight server tooreduce késleltetés és a költségeket.

3. A hello **konfigurálása az Event Hubs** képernyőn, adja meg a hello **száma partícióazonosító** és **üzenet megőrzési** értékek. Az ebben a példában a partíciók száma 10 és a egy üzenet megőrzési 1 használhat. Megjegyzés: hello partíciók száma, mert később szüksége ezt az értéket.

    ![2. varázslólap](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz2.png)

4. Hello eseményközpont létre, jelölje be hello névtér után válassza ki a **Event Hubs**, majd válassza ki a korábban létrehozott eseményközpont hello.
5. Válassza ki **konfigurálása**, majd hozzon létre két új hozzáférési házirendek segítségével a következő információ hello:

    <table>
    <tr><th>Név</th><th>Engedélyek</th></tr>
    <tr><td>író</td><td>Küldés</td></tr>
    <tr><td>Olvasó</td><td>Figyelés</td></tr>
    </table>

    Miután létrehozta a hello engedélyek, válassza ki a hello **mentése** alján hello hello ikonra. A megosztott elérési házirendek használt tooread és tooEvent Hub írni.

    ![házirendek](./media/hdinsight-storm-develop-csharp-event-hub-topology/policy.png)

6. Miután menti hello házirendek, a hello **megosztott megosztottelérésikulcs-készítő** hello lap tooretrieve hello kulcsa hello hello alján **író** és **olvasó** házirendek. Ezek a kulcsok mentéséhez.

## <a name="download-and-build-hello-project"></a>Töltse le és hello projekt létrehozása

1. Töltse le a hello projektet a Githubról: [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub). Hello csomag, mint a zip-archívum létrehozása, vagy [git](https://git-scm.com/) tooclone hello projekt helyi küldése.

2. Módosítsa a hello `dev.properties` az Eseményközpont hello konfigurációs fájlt.

3. A következő toobuild és a csomag hello projekt hello használata:

        mvn package

    Ez a parancs letölti a szükséges függőségek, buildek, és majd csomagok hello projekt. hello kimeneti tárolódik hello **/target** könyvtárhoz, mint a **EventHubExample-1.0-SNAPSHOT.jar**.

## <a name="test-locally"></a>Helyi tesztelése

Mivel a topológiák csak olvasási és írási tooEvent hubok, tesztelheti őket helyileg, ha rendelkezik egy [Storm fejlesztőkörnyezet](http://storm.apache.org/releases/current/Setting-up-development-environment.html). A következő lépéseket toorun helyi környezetben hello fejlesztői hello használata:

1. Futtassa a hello író:

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /writer.yaml --filter dev.properties

2. Hello olvasó futtassa:

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /reader.yaml --filter dev.properties

> [!TIP]
> * `--local`: Hello topológia helyi módban fusson (nem osztott).
> * `-R /writer.yaml`: Hello topológia definíciójának betöltése hello `resources` hello jar csomagolva. Ha hello topológia egy fájl hello helyi fájlrendszerben, ehelyett adja meg hello elérési tooit hello utolsó paraméterként.
> * `--filter dev.properties`: Hello tartalmát használja `dev.properties` toofill hello topológia definíciókban hello értékeket. Például: `${eventhub.read.policy.name}`.

A helyi futtatás során eredménye a naplózott toohello konzol. Használjon __Ctrl + C__ toostop hello topológia.

## <a name="deploy-hello-topologies"></a>Hello topológiák telepítése

1. Szolgáltatáskapcsolódási pont toocopy hello jar csomag tooyour HDInsight-fürt használatára. Cserélje le felhasználónév hello SSH-felhasználó a fürt számára. CLUSTERNAME cserélje le a HDInsight fürt hello neve:

        scp ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.

    Ha SSH-fiókját használja jelszó, felszólító tooenter hello jelszót is. Ha SSH-kulcsot használt hello fiókkal, szükség lehet a toouse hello `-i` paraméter toospecify hello elérési toohello kulcsfájlt. Például: `scp -i ~/.ssh/id_rsa ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`

    Ez a parancs átmásolja hello fájl toohello a SSH felhasználójának kezdőkönyvtárát hello fürtön.

2. Miután hello fájl befejezte a feltöltést, használja az SSH tooconnect toohello HDInsight-fürthöz. Cserélje le **felhasználónév** az SSH-bejelentkezéskor hello nevét. Cserélje le **CLUSTERNAME** a HDInsight fürt nevű:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [!NOTE]
    > Ha SSH-fiókját használja jelszó, felszólító tooenter hello jelszót is. Ha SSH-kulcsot használt hello fiókkal, szükség lehet a toouse hello `-i` paraméter toospecify hello elérési toohello kulcsfájlt. hello alábbi példa betölti hello titkos kulcsát a `~/.ssh/id_rsa`:
    >
    > `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`

3. A következő parancs toostart hello topológiák hello használata:

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writer.yaml --filter dev.properties
        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /reader.yaml --filter dev.properties

    > [!TIP]
    > * `--remote`: Hello topológia toohello Nimbus szolgáltatást, amely elindítja hello munkavégző csomópontokhoz hello fürt küldi el.

4. tooview hello naplózott adatok, nyissa meg toohttps://CLUSTERNAME.azurehdinsight.net/stormui, ahol __CLUSTERNAME__ hello neve, a HDInsight-fürthöz. Válassza ki a hello topológiákat, és részletekbe menően tárhatják toohello összetevőket. Jelölje be hello __port__ egy összetevő tooview példányának bejegyzés információt naplózta.

5. A következő parancsok toostop hello topológiák hello használata:

        storm kill reader
        storm kill writer

## <a name="delete-your-cluster"></a>A fürt törlése

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Következő lépések

* [HDInsight alatt futó Storm példatopológiái](hdinsight-storm-example-topology.md)
