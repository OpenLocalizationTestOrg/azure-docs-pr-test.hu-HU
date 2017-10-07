---
title: "aaaProcess vehicle érzékelő adatokat a HDInsight alatt futó Apache Storm |} Microsoft Docs"
description: "Megtudhatja, hogyan tooprocess vehicle érzékelő adatokat az Event Hubs a HDInsight alatt futó Apache Storm használatával. Vegye fel a modell adatai a Azure Cosmos-Adatbázisból, és kimeneti toostorage tárolja."
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
ms.openlocfilehash: 8f7b1dbb9072e095ea32160bb731bedd071288af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="process-vehicle-sensor-data-from-azure-event-hubs-using-apache-storm-on-hdinsight"></a>Az Azure Event Hubs a HDInsight alatt futó Apache Storm használatával vehicle érzékelő adatok feldolgozása

Megtudhatja, hogyan tooprocess vehicle a HDInsight alatt futó Apache Storm használatával Azure Event hubs érzékelőadatait. Ez a példa olvassa be az Azure Event Hubs érzékelőadatait, a következőképpen színesíti hello adatok Azure Cosmos DB tárolt adatok Vezérlőpultjának. hello tárolja az Azure Storage hello Hadoop fájlrendszerrel (HDFS) használatával.

![HDInsight és hello az eszközök internetes hálózatát (IoT) architektúra diagramja](./media/hdinsight-storm-iot-eventhub-documentdb/iot.png)

## <a name="overview"></a>Áttekintés

Érzékelők toovehicles hozzáadását teszi lehetővé toopredict berendezések problémákat előzményadatokat trendek alapján. Lehetővé teszi toomake fejlesztései toofuture verziók használatelemzés minta alapján. Képes tooquickly legyen, és hatékonyan hello adatok betöltése az összes járművekről gyűjtött az Hadoop MapReduce feldolgozása előtt. Emellett Kezdésként kritikus hibával elérési út (motor, fékek stb.) toodo elemzés valós időben.

Az Azure Event Hubs-t toohandle hello nagy kötet érzékelők által generált adatmennyiséget. Apache Storm csak használt tooload és hello adatfeldolgozásra tárolja a HDFS előtt.

## <a name="solution"></a>Megoldás

Az érzékelők motor, a környezeti hőmérséklet és a vehicle sebesség telemetriai adatokat rögzíti. Adatokat küldi el a rendszer tooEvent hubok hello car Vehicle azonosító szám (VIN) és egy időbélyegzőt együtt. Ezekből a egy Apache Storm on HDInsight-fürt futó Storm-topológia hello adatokat olvas, folyamatokat engedélyez, és tárolja a HDFS be.

A feldolgozás során hello VIN használt tooretrieve típusra vonatkozó adatokat a Cosmos-Adatbázisból. Ezek az adatok toohello adatfolyam kerül, mielőtt a rendszer tárolja.

a Storm-topológia hello használt hello összetevők a következők:

* **EventHubSpout** -olvassa be az adatokat az Azure Event Hubs
* **TypeConversionBolt** -konvertálja a JSON-karakterláncban az Event Hubs érzékelőadatait következő hello tartalmazó rekordot a hello:
    * Motor
    * Környezeti hőmérséklet
    * Gyorsaság
    * VIN
    * időbélyeg
* **DataReferencBolt** -hello vehicle modell megkeresi a hello VIN használatával Cosmos-Adatbázisból
* **WasbStoreBolt** -tárolók hello adatok tooHDFS (Azure Storage)

a következő kép hello diagramját, illetve az ehhez a megoldáshoz:

![Storm-topológia](./media/hdinsight-storm-iot-eventhub-documentdb/iottopology.png)

## <a name="implementation"></a>Megvalósítás

A teljes automatikus megoldás az ebben a forgatókönyvben hello részeként [HDInsight-Storm-példák](https://github.com/hdinsight/hdinsight-storm-examples) GitHub tárházából. toouse ebben a példában hello hello lépéseit kövesse [IoTExample fontos. MD](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md).

## <a name="next-steps"></a>Következő lépések

További példa Storm-topológiák, lásd: [a HDInsight alatt futó Storm](hdinsight-storm-example-topology.md).

