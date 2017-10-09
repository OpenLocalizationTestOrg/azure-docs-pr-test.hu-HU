---
title: "az Event Hubs Storm - Azure HDInsight aaaProcess események |} Microsoft Docs"
description: "Ismerje meg, hogyan tooprocess adatait az Azure Event Hubs C# Storm-topológia hozza létre a Visual Studio hello segítségével a HDInsight tools Visual Studio."
services: hdinsight,notification hubs
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 67f9d08c-eea0-401b-952b-db765655dad0
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/03/2017
ms.author: larryfr
ms.openlocfilehash: 30cd910d80eba066f283197bcbbaf11145bc5524
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="process-events-from-azure-event-hubs-with-storm-on-hdinsight-c"></a>Az Azure Event Hubs (C#) futó Storm eseményeinek

Ismerje meg, hogy az Azure Event Hubs a HDInsight alatt futó Apache Storm toowork. Ez a dokumentum egy C# Storm topológia tooread és írási adatait használja Evbent hubok

> [!NOTE]
> Ez a projekt Java verziója: [feldolgozni az eseményeket az Azure Event Hubs (Java) futó Storm](hdinsight-storm-develop-java-event-hub-topology.md).

## <a name="scpnet"></a>SCP.NET

jelen dokumentumban leírt lépések hello SCP.NET, a NuGet-csomagot, így könnyen toocreate C#-topológiák és összetevők Storm való használathoz a HDInsight használata.

> [!IMPORTANT]
> Amíg ez a dokumentum lépéseit hello a Visual Studio Windows fejlesztői környezetre támaszkodnak, hello lefordított projekt elküldött tooa Storm on HDInsight-fürt által használt Linux lehet. Csak a Linux-alapú fürtök létrehozása után 28 2016 októberétől kezdve, SCP.NET topológiákat támogatja.

HDInsight 3.4 és nagyobb használata monó toorun C#-topológiák. Ez a dokumentum hello példában HDInsight 3.6 működik. Ha a HDInsight a saját .NET megoldások létrehozását tervezi, ellenőrizze a hello [monó kompatibilitási](http://www.mono-project.com/docs/about-mono/compatibility/) lehetséges incompatibilities dokumentumában.

### <a name="cluster-versioning"></a>Fürt versioning

hello Microsoft.SCP.Net.SDK NuGet-csomagot a projekthez használt hello főverzióját telepítve a HDInsight alatt futó Storm egyeznie kell. HDInsight-verziókról 3.5-ös és 3.6 alatt futó Stormot használni 1.x, így ezeken a fürtökön SCP.NET verzió 1.0.x.x kell használnia.

> [!IMPORTANT]
> Ebben a dokumentumban hello példa egy HDInsight 3.5-ös vagy 3,6 fürt vár.
>
> Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

C#-topológiák is kell használnia célként .NET 4.5.

## <a name="how-toowork-with-event-hubs"></a>Hogyan toowork az Event Hubs

A Microsoft biztosít a Java-összetevők, amelyek az Event Hubs egy Storm-topológia a használt toocommunicate lehetnek. Hello Java archív (JAR) fájl, amely tartalmazza ezeket az összetevőket, egy HDInsight 3.6 kompatibilis verzióját [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).

> [!IMPORTANT]
> Amíg hello összetevők Java nyelven íródtak, könnyen használhatja őket a C#-topológiák.

a következő összetevők hello szerepelnek ebben a példában:

* __EventHubSpout__: olvassa be az adatokat az Event Hubs.
* __EventHubBolt__: adatok tooEvent hubok írja.
* __EventHubSpoutConfig__: tooconfigure EventHubSpout használt.
* __EventHubBoltConfig__: tooconfigure EventHubBolt használt.

### <a name="example-spout-usage"></a>Spout-használat – példa

SCP.NET egy EventHubSpout tooyour topológia hozzáadására szolgáló módszert biztosít. Ezek a módszerek révén könnyebben tooadd egy spout mint hello az általános metódusok használata a Java-összetevő hozzáadásához. hello következő példa bemutatja, hogyan toocreate használatával egy spout hello __SetEventHubSpout__ és **EventHubSpoutConfig** SCP.NET által biztosított módszerek:

```csharp
 topologyBuilder.SetEventHubSpout(
    "EventHubSpout",
    new EventHubSpoutConfig(
        ConfigurationManager.AppSettings["EventHubSharedAccessKeyName"],
        ConfigurationManager.AppSettings["EventHubSharedAccessKey"],
        ConfigurationManager.AppSettings["EventHubNamespace"],
        ConfigurationManager.AppSettings["EventHubEntityPath"],
        eventHubPartitions),
    eventHubPartitions);
```

hello előző példa létrehoz egy új spout nevű összetevő __EventHubSpout__, és az eseményközpontban toocommunicate azt konfigurálja. hello párhuzamossági mutató hello összetevő hello eseményközpont toohello több partíció van megadva. Ez a beállítás lehetővé teszi, hogy a Storm toocreate hello minden partíció esetében példányt.

### <a name="example-bolt-usage"></a>Példa a bolt használatra

Használjon hello **JavaComponmentConstructor** metódus toocreate hello bolt példánya. hello következő példa bemutatja, hogyan toocreate és állítsa be egy új példányát hello **EventHubBolt**:

```csharp
// Java construcvtor for hello Event Hub Bolt
JavaComponentConstructor constructor = JavaComponentConstructor.CreateFromClojureExpr(
    String.Format(@"(org.apache.storm.eventhubs.bolt.EventHubBolt. (org.apache.storm.eventhubs.bolt.EventHubBoltConfig. " +
        @"""{0}"" ""{1}"" ""{2}"" ""{3}"" ""{4}"" {5}))",
        ConfigurationManager.AppSettings["EventHubPolicyName"],
        ConfigurationManager.AppSettings["EventHubPolicyKey"],
        ConfigurationManager.AppSettings["EventHubNamespace"],
        "servicebus.windows.net",
        ConfigurationManager.AppSettings["EventHubName"],
        "true"));

// Set hello bolt toosubscribe toodata from hello spout
topologyBuilder.SetJavaBolt(
    "eventhubbolt",
    constructor,
    partitionCount)
        .shuffleGrouping("Spout");
```

> [!NOTE]
> Ez a példa egy karakterláncként használata helyett átadott Clojure kifejezés **JavaComponentConstructor** toocreate egy **EventHubBoltConfig**, mint hello spout példa. Mindkét módszer használható. Ajánlott tooyou érzi hello módot használ.

## <a name="download-hello-completed-project"></a>Hello befejeződött-projekt letöltése

Ebben az oktatóanyagban a létrehozott hello projekt teljes verziója letölthető [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub). Azonban továbbra is szükség tooprovide konfigurációs beállítások ebben az oktatóanyagban hello lépéseket követve.

### <a name="prerequisites"></a>Előfeltételek

* Egy [Apache Storm on HDInsight-fürt verziószáma 3.5-ös vagy 3.6](hdinsight-apache-storm-tutorial-get-started.md).

    > [!WARNING]
    > Ez a dokumentum hello példában 3.5-ös vagy 3.6 HDInsight alatt futó Storm igényel. Ez nem működik toobreaking osztály nevének módosítása miatt a HDInsight, korábbi verzióival. Ebben a példában, amely kompatibilis a régebbi fürtök verziója: [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub/releases).

* Egy [Azure event hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).

* Hello [Azure .NET SDK](http://azure.microsoft.com/downloads/).

* Hello [a HDInsight tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

* Java JDK 1.8 vagy később a fejlesztési környezetet. JDK letöltések érhetők el a [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).

  * Hello **JAVA_HOME** környezeti változó kell pont toohello tartalmazó könyvtár Java.
  * Hello **%JAVA_HOME%/bin** könyvtárnak hello elérési úton kell lennie.

## <a name="download-hello-event-hubs-components"></a>Hello Event Hubs összetevők letöltése

Letöltési hello Event Hubs spout, és az összetevőt erről a boltok [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).

Hozzon létre egy könyvtárat nevű `eventhubspout`, és mentse a hello fájlt hello könyvtárba.

## <a name="configure-event-hubs"></a>Az Event Hubs konfigurálása

Az Event Hubs hello adatforrás ehhez a példához. Használja hello hello "Létrehoz egy eseményközpontot" szakaszban található adatokat a [Bevezetés az Event Hubs használatába](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).

1. Hello eseményközpont létrehozása után megtekintheti a hello **EventHub** panel az Azure portál, majd válassza hello **megosztott elérési házirendek**. Válassza ki **+ Hozzáadás** tooadd hello következő házirendek:

   | Név | Engedélyek |
   | --- | --- |
   | író |Küldés |
   | olvasó |Figyelés |

    ![Képernyőkép a fájlmegosztás hozzáférési házirendek ablak](./media/hdinsight-storm-develop-csharp-event-hub-topology/sas.png)

2. Jelölje be hello **olvasó** és **író** házirendek. Másolja ki és mentse a hello elsődleges kulcs értéke mindkét házirend később használja ezeket az értékeket.

## <a name="configure-hello-eventhubwriter"></a>Hello EventHubWriter konfigurálása

1. Ha még nem telepítette hello hello HDInsight eszközök legújabb verzióját a Visual Studio, lásd: [első lépései a HDInsight tools for Visual Studio használatával](hdinsight-hadoop-visual-studio-tools-get-started.md).

2. Töltse le a hello megoldást [eventhub-storm-hibrid](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).

3. A hello **EventHubWriter** projektet, nyissa meg hello **App.config** fájlt. Hello adatait hello eseményközpont korábbi toofill konfigurálta a következő kulcsok hello értékének hello használata:

   | Kulcs | Érték |
   | --- | --- |
   | EventHubPolicyName |író (Ha használja-e egy másik nevet a hello házirend *küldése* engedéllyel, akkor használja helyette.) |
   | EventHubPolicyKey |hello kulcs hello író házirend. |
   | EventHubNamespace |hello névtér, amely tartalmazza az eseményközpont. |
   | EventHubName |Az eseményközpont neveként. |
   | EventHubPartitionCount |a partíciók az eseményközpont hello száma. |

4. Mentse és zárja be a hello **App.config** fájlt.

## <a name="configure-hello-eventhubreader"></a>Hello EventHubReader konfigurálása

1. Nyissa meg hello **EventHubReader** projekt.

2. Nyissa meg hello **App.config** hello fájlt **EventHubReader**. Hello adatait hello eseményközpont korábbi toofill konfigurálta a következő kulcsok hello értékének hello használata:

   | Kulcs | Érték |
   | --- | --- |
   | EventHubPolicyName |olvasó (Ha használja-e egy másik nevet a hello házirend *figyelésére* engedéllyel, használja helyette.) |
   | EventHubPolicyKey |hello kulcs hello olvasó házirend. |
   | EventHubNamespace |hello névtér, amely tartalmazza az eseményközpont. |
   | EventHubName |Az eseményközpont neveként. |
   | EventHubPartitionCount |a partíciók az eseményközpont hello száma. |

3. Mentse és zárja be a hello **App.config** fájlt.

## <a name="deploy-hello-topologies"></a>Hello topológiák telepítése

1. A **Megoldáskezelőben**, kattintson a jobb gombbal hello **EventHubReader** projektre, és válassza a **elküldeni a HDInsight tooStorm**.

    ![Képernyőfelvétel a Solution Explorer, a Küldés tooStorm kiemelt hdinsight](./media/hdinsight-storm-develop-csharp-event-hub-topology/submittostorm.png)

2. A hello **nyújt topológia** párbeszédpanelen jelölje ki a **Storm-fürt**. Bontsa ki a **további konfigurációs**, jelölje be **Java elérési utat**, jelölje be **...** , és a korábban letöltött hello JAR-fájlt tartalmazó select hello könyvtár. Végezetül kattintson **Submit**.

    ![Topológia nyújt képernyőkép párbeszédpanel](./media/hdinsight-storm-develop-csharp-event-hub-topology/submit.png)

3. Hello topológia elküldésekor hello **Storm-topológiák Viewer** jelenik meg. hello topológia, jelölje be hello tooview információ **EventHubReader** topológia hello bal oldali ablaktáblán.

    ![A Storm-topológiák megjelenítő képernyőkép](./media/hdinsight-storm-develop-csharp-event-hub-topology/topologyviewer.png)

4. A **Megoldáskezelőben**, kattintson a jobb gombbal hello **EventHubWriter** projektre, és válassza a **elküldeni a HDInsight tooStorm**.

5. A hello **nyújt topológia** párbeszédpanelen jelölje ki a **Storm-fürt**. Bontsa ki a **további konfigurációs**, jelölje be **Java elérési utat**, jelölje be **...** , és jelölje be hello directory hello JAR-fájlt tartalmazó korábban letöltött. Végezetül kattintson **Submit**.

6. Hello topológia elküldésekor frissítse hello topológia listáját a hello **Storm-topológiák Viewer** tooverify futtató mindkét topológia hello fürtön.

7. A **Storm-topológiák Viewer**, jelölje be hello **EventHubReader** topológia.

8. tooopen hello összetevő összegzése hello bolt, kattintson duplán a hello **LogBolt** hello diagram komponens.

9. A hello **végrehajtója** területen válassza ki a hello hivatkozások egyikét a hello **Port** oszlop. Hello összetevő által naplózott adatokat jeleníti meg. hello naplózott információ a következő szöveg hasonló toohello:

        2017-03-02 14:51:29.255 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,255 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1830978598,"deviceId":"8566ccbc-034d-45db-883d-d8a31f34068e"}
        2017-03-02 14:51:29.283 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,283 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1756413275,"deviceId":"647a5eff-823d-482f-a8b4-b95b35ae570b"}
        2017-03-02 14:51:29.313 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,312 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1108478910,"deviceId":"206a68fa-8264-4d61-9100-bfdb68ee8f0a"}

## <a name="stop-hello-topologies"></a>Állítsa le a hello topológiák

toostop hello topológiákat, jelölje ki minden egyes topológia hello **Storm-topológia Viewer**, majd kattintson a **Kill**.

![Képernyőfelvétel a Storm topológia megjelenítő, a Kill gomb](./media/hdinsight-storm-develop-csharp-event-hub-topology/killtopology.png)

## <a name="delete-your-cluster"></a>A fürt törlése

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Következő lépések

Ebben a dokumentumban megtanulhatta, hogyan toouse hello Java Event Hubs spout és egy C# topológia toowork adatokkal az Azure Event Hubs a boltok. További információ az létrehozása a C#-topológiák, toolearn hello következő lásd:

* [Visual Studio használatával HDInsight alatt futó Apache Storm a C#-topológiák fejlesztése](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [Szolgáltatáskapcsolódási pont programozási útmutató](hdinsight-storm-scp-programming-guide.md)
* [HDInsight alatt futó Storm példatopológiái](hdinsight-storm-example-topology.md)
