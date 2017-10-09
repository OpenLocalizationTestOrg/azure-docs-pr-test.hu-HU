---
title: "a HDInsight aaaExample Apache Storm-topológiák |} Microsoft Docs"
description: "Példa Storm-topológiák listája létrehozott, és többek között az alapvető C# és a Java-topológiákat, és működik-e az Event Hubs HDInsight alatt futó Apache Storm tesztelték."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: f9b1bdff-5928-4705-a76d-52fd200917cb
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/07/2017
ms.author: larryfr
ms.openlocfilehash: b20299112f6489b7c99360f0a539fc732703c64a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="example-storm-topologies-and-components-for-apache-storm-on-hdinsight"></a>Példa Storm-topológiák és a HDInsight alatt futó Apache Storm összetevők

hello példák létrehozása és használata a HDInsight alatt futó Apache Storm a Microsoft által kezelt listája látható. Ezek a példák témakörök, hozzon létre alapvető C# és a Java-topológiák tooworking például az Event Hubs, Cosmos DB, a Power BI, SQL-adatbázis, a HBase a HDInsight és az Azure Storage Azure-szolgáltatásokkal foglalkozik. Néhány példa is bemutatják, hogyan toowork az-Azure, vagy még nem a Microsofttól technológiákkal, például SignalR és Socket.IO.

| Leírás | Azt mutatja be | Nyelvi/keretrendszer |
|:--- |:--- |:--- |
| [Az Apache Storm tooAzure Data Lake Store írása](hdinsight-storm-write-data-lake-store.md) |TooAzure Data Lake Store írása |Java |
| [Event Hub Spout és Bolt adatforrás](https://github.com/apache/storm/tree/master/external/storm-eventhubs) |Az Event Hub Spout hello és Bolt forrás |Java |
| [Java-alapú topológiák fejlesztése hdinsighton futó Apache stormra][5797064f] |Maven |Java |
| [Visual Studio használatával HDInsight alatt futó Apache Storm a C#-topológiák fejlesztése][16fce2d1] |HDInsight Tools for Visual Studio |C#, Java |
| [Hozzon létre több adatfolyamokat a C# Storm-topológia][ec5a4064] |Több adatfolyam |C# |
| [Az Azure Event Hubs (C#) futó Storm eseményeinek][844d1d81] |Event Hubs |C# és Java |
| [Az Azure Event Hubs (Java) futó Storm eseményeinek](hdinsight-storm-develop-java-event-hub-topology.md) |Event Hubs |Java |
| [A Power Bi toovisualize adat használata a Storm-topológia][94d15238] |Power BI |C# |
| [Érzékelőadatok elemzése a Storm és HBase a hdinsight eszközben][ab894747] |Event Hubs, HBase, Socket.IO, webes irányítópult |C#, Java, JavaScript, HTML |
| [Az Event Hubs a HDInsight alatt futó Storm használatával vehicle érzékelő adatok feldolgozása][246ee964] |Event Hubs, Cosmos DB, Azure Storage-Blob (WASB) |C#, Java |
| [Kinyerési, átalakítási és betöltési (ETL) az Azure Event Hubs tooHBase, a HDInsight alatt futó Storm használatával][b4b68194] |Az Event Hubs, HBase |C# |
| [C# Storm topology sablonprojekt Azure HDInsight alatt futó Storm-szolgáltatásokhoz való munkához][ce0c02a2] |Event Hubs, Cosmos DB SQL adatbázis, HBase, SignalR |C#, Java |
| [Méretezhetőség referenciaalapok Azure Event hubs a HDInsight alatt futó Storm használatával olvasásra][d6c540e3] |Üzenet átviteli, az Event Hubs SQL-adatbázis |C#, Java |
| [A HDInsight alatt futó Storm és HBase használatával összefüggésbe események](hdinsight-storm-correlation-topology.md) |HBase |C# |
| [Python használata a HDInsight alatt futó Storm](hdinsight-storm-develop-python-topology.md) |Python-összetevők a fluxus topológia |Python |
| [Kafka használata a HDInsight alatt futó Storm](hdinsight-apache-storm-with-kafka.md) | Alatt futó Apache Storm olvasása és írása tooApache Kafka | Java |

### <a name="next-steps"></a>Következő lépések

* [Bevezetés a HDInsight-alapú Apache Storm használatába][2b8c3488]
* [Megtudhatja, hogyan toodeploy és kezelése a HDInsight alatt futó Storm Storm-topológiák][6eb0d3b8]

[2b8c3488]: hdinsight-apache-storm-tutorial-get-started-linux.md "Ismerje meg, hogyan toocreate egy HDInsight-fürt és a Storm hello a Storm irányítópultja toodeploy példa topológiákat."
[6eb0d3b8]: hdinsight-storm-deploy-monitor-topology.md "Megtudhatja, hogyan toodeploy és topológiák hello webalapú Storm irányítópultjának és a Storm felhasználói felületén vagy a hello a HDInsight Tools for Visual Studio használatával kezelheti."
[16fce2d1]: hdinsight-storm-develop-csharp-visual-studio-topology.md "Ismerje meg, hogyan toocreate C# Storm-topológiák a HDInsight Tools hello Visual Studio."
[5797064f]: hdinsight-storm-develop-java-topology.md "Megtudhatja, hogyan toocreate Storm-topológiák a Java, Maven, használatával hozzon létre egy alapszintű wordcount-topológiával."
[94d15238]: hdinsight-storm-power-bi-topology.md "Bemutatja, hogyan toowrite adatok tooPower BI a C#-topológiák, majd hozzon létre egy diagram és az irányítópult hello adatokból."
[ec5a4064]: https://github.com/Blackmist/csharp-storm-example "Azt mutatja be egy alapszintű Storm-topológia, amely elvégzi a wordcount, a C# megvalósítva. Ez is bemutatja, hogyan toocreate több adatfolyamot C#-topológiák belül."
[844d1d81]: hdinsight-storm-develop-csharp-event-hub-topology.md "Megtudhatja, hogyan Azure Event Hubs a HDInsight alatt futó Storm tooread és írási adatait."
[ab894747]: hdinsight-storm-sensor-data-analysis.md "Ismerje meg, hogyan toouse alatt futó Apache Storm a HDInsight tooprocess Azure Event hubs érzékelőadatait D3.js használatával jelenítheti meg, és (opcionálisan) tooHBase tárolja."
[246ee964]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md "Ismerje meg, hogyan toouse egy Storm-topológia tooread Azure Event hubs üzenetek dokumentumok olvasását Azure Cosmos adatbázisából az adatok hivatkozik, és mentse adatok tooAzure tároló."
[d6c540e3]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/EventCountExample "Több topológia toodemonstrate átviteli amikor adatbázis a HDInsight alatt futó Apache Storm használatával az Azure Event Hubs olvasása és tooSQL tárolásához."
[b4b68194]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample "Ismerje meg, hogyan tooread adatokat az Azure Event Hubs, összesítés & átalakító hello adatokat, majd a HDInsight tooHBase tárolja."
[ce0c02a2]: https://github.com/hdinsight/hdinsight-storm-examples/tree/master/templates/HDInsightStormExamples "A projekt tartalmaz a spoutokkal kapcsolatban, szögek és topológiák toointeract, mint az Event Hubs, Cosmos DB és SQL-adatbázis különböző Azure-szolgáltatásokkal sablonokat."

