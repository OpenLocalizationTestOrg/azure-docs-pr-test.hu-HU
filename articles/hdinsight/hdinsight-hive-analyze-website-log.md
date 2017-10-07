---
title: "a webhelynapló elemzése - Azure HDInsight Hive hadooppal aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogy miként naplózza az toouse Hive HDInsight tooanalyze webhelyet. A naplófájlt használja bemenetként egy HDInsight táblába lesz, és HiveQL tooquery hello adatok felhasználásával."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6fb7b5c2-8df4-40b1-a9e2-6815080004f9
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 9cbce3cc8cf8bc3ad104dc4ca6a5628802c8fe89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hive-with-windows-based-hdinsight-tooanalyze-logs-from-websites"></a>A Hive használata a Windows-alapú HDInsight tooanalyze naplók webhelyek
Ismerje meg, hogy miként naplózza az toouse HiveQL a HDInsight tooanalyze egy webhelyről. A webhelynapló elemzése használt toosegment a célközönséget, hasonló tevékenységek alapján kell, a látogatók demográfiai és kimenő hello tartalom megtekinthető, hello webhelyek származnak, és egyéb toofind kategorizálását.

> [!IMPORTANT]
> Ez a dokumentum csak olyan feladaton végezhető Windows-alapú HDInsight-fürtökkel hello szükséges lépések. HDInsight csak érhető el a Windows korábbi, mint a HDInsight 3.4-es verziójához. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

Ez a minta egy HDInsight fürt tooanalyze webhely napló fájlok tooget betekintést hello gyakoriság látogatások toohello webhely a külső webhelyekről egy nap fogja használni. Lesz generálhat, hogy a felhasználók hello webhely hibák összegzését. Megtudhatja, hogyan:

* Csatlakozás az Azure Blob Storage tárolóban, amely webhely naplófájlokat tartalmazó tooa.
* Hozzon létre HIVE táblák tooquery lesznek a naplók.
* HIVE-lekérdezések tooanalyze hello adatok létrehozásához.
* Használja a Microsoft Excel tooconnect tooHDInsight (adatbázis megnyitása (ODBC) tooretrieve elemzett hello kapcsolatadatainak használatával.

![HDI. Samples.Website.Log.Analysis][img-hdi-weblogs-sample]

## <a name="prerequisites"></a>Előfeltételek
* Az Azure HDInsight Hadoop-fürthöz van kiépítve. Útmutatásért lásd: [Provision HDInsight-fürtök][hdinsight-provision].
* Rendelkeznie kell Microsoft Excel 2013 vagy az Excel 2010 telepítve.
* Rendelkeznie kell [Microsoft Hive ODBC-illesztő](http://www.microsoft.com/download/details.aspx?id=40886) Excelbe Hive tooimport adatait.

## <a name="toorun-hello-sample"></a>toorun hello minta
1. A hello [Azure Portal](https://portal.azure.com/), a hello Kezdőpulton (ha rögzítette hello fürt nincs), kattintson a hello fürt csempe kívánja toorun hello minta.
2. Hello a fürt paneljén a **Gyorshivatkozások**, kattintson a **fürt irányítópult**, és majd a hello **fürt irányítópult** panelen kattintson a **HDInsight-fürt Irányítópult**. Képes közvetlenül megnyitható hello irányítópult hello URL-cím a következő használatával:

         https://<clustername>.azurehdinsight.net

    Amikor a rendszer kéri, hello rendszergazdai felhasználónév és jelszó használatával a hello fürt létesítésekor használatával hitelesíteni.
3. Weblapról hello megnyíló, kattintson a hello **Getting Started gyűjtemény** fülre, majd a hello **mintaadatokkal megoldások** kategória, hello kattintson **Webhelynapló elemzése** minta.
4. Kövesse a hello weblap toofinish hello minta hello megjelenő utasításokat.

## <a name="next-steps"></a>Következő lépések
Próbálja meg a következő minta hello: [Érzékelőadatok elemzése a HDInsight Hive eszközzel](hdinsight-hive-analyze-sensor-data.md).

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md

[img-hdi-weblogs-sample]: ./media/hdinsight-hive-analyze-website-log/hdinsight-weblogs-sample.png
