---
title: "A HDInsight alatt futó Apache Storm vehicle érzékelő adatok feldolgozása |} Microsoft Docs"
description: "Útmutató a vehicle érzékelő adatokat az Event Hubs a HDInsight alatt futó Apache Storm használatával feldolgozni. Vegye fel a modell adatai a Azure Cosmos-Adatbázisból, és a kimenetet tárhelyre."
services: hdinsight,documentdb,notification-hubs
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 78980635-8bef-4c33-96c3-fae50e932e31
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/03/2017
ms.author: larryfr
ms.openlocfilehash: 8e8ebc724e1c70e8fcd56312adef5da2342373ea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="process-vehicle-sensor-data-from-azure-event-hubs-using-apache-storm-on-hdinsight"></a>Az Azure Event Hubs a HDInsight alatt futó Apache Storm használatával vehicle érzékelő adatok feldolgozása

Útmutató a vehicle érzékelő adatokat az Azure Event Hubs a HDInsight alatt futó Apache Storm használatával feldolgozni. Ez a példa olvassa be az Azure Event Hubs érzékelőadatait, a következőképpen színesíti az adatok Azure Cosmos DB tárolt adatok Vezérlőpultjának. Az adatok Azure Storage a Hadoop fájlrendszerrel (HDFS) használatával történő tárolását.

![HDInsight és az eszközök internetes hálózatát (IoT) architektúrája](./media/hdinsight-storm-iot-eventhub-documentdb/iot.png)

## <a name="overview"></a>Áttekintés

Érzékelők hozzáadása járművekről gyűjtött teszi berendezések problémákat előzményadatokat trendek alapján előre jelezni. Lehetővé teszi a használatelemzés minta alapján a jövőbeli verziókkal továbbfejlesztésére. Gyors és hatékony az adatok betöltése az összes járművekről gyűjtött az Hadoop MapReduce feldolgozása előtt képesnek kell lennie. Emellett érdemes a kritikus hibával elérési utak (motor, fékek stb.) a valós idejű elemzést.

Azure Event Hubs-t a nagy mennyiségű érzékelők által létrehozott adatok kezelésére. Apache Storm használható betölteni, és az adatok feldolgozása előtt a HDFS tárolja.

## <a name="solution"></a>Megoldás

Az érzékelők motor, a környezeti hőmérséklet és a vehicle sebesség telemetriai adatokat rögzíti. Majd adatküldést Event hubs együtt a car Vehicle azonosító szám (VIN), valamint egy időbélyegző. Ott a egy Apache Storm on HDInsight-fürt futó Storm-topológia a adatokat olvas, folyamatokat engedélyez, és tárolja a HDFS be.

A feldolgozás során a VIN használatos típusra vonatkozó adatokat a Cosmos-Adatbázisból. Mielőtt a rendszer tárolja ezeket az adatokat az adatfolyamot kerül.

A Storm-topológia használt összetevők a következők:

* **EventHubSpout** -olvassa be az adatokat az Azure Event Hubs
* **TypeConversionBolt** -konvertálja a JSON-karakterláncban az Event Hubs egy rekord, a következő érzékelő adatokat tartalmazó:
    * Motor
    * Környezeti hőmérséklet
    * Gyorsaság
    * VIN
    * időbélyeg
* **DataReferencBolt** -keres a vehicle modell a használatával a VIN Cosmos-Adatbázisból
* **WasbStoreBolt** -tárolja az adatokat HDFS (Azure Storage)

Az alábbi képen egy diagram az ilyen típusú megoldásra:

![Storm-topológia](./media/hdinsight-storm-iot-eventhub-documentdb/iottopology.png)

## <a name="implementation"></a>Megvalósítás

Egy teljes automatikus megoldás az ebben a forgatókönyvben áll rendelkezésre, mert része a [HDInsight-Storm-példák](https://github.com/hdinsight/hdinsight-storm-examples) GitHub tárházából. Ebben a példában, hajtsa végre a lépéseket a [IoTExample fontos. MD](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md).

## <a name="next-steps"></a>Következő lépések

További példa Storm-topológiák, lásd: [a HDInsight alatt futó Storm](hdinsight-storm-example-topology.md).

