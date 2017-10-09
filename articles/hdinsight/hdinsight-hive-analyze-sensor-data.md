---
title: "Hive és a Hadoop - Azure HDInsight segítségével aaaAnalyze érzékelőadatait |} Microsoft Docs"
description: "Ismerje meg, hogyan tooanalyze érzékelőadatait használatával hello Hive lekérdezés konzol és a HDInsight (Hadoop) együttes, majd a Microsoft Excel PowerView a hello adatainak megjelenítése."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: a8ac160c-1cef-45d9-bf36-7beb5a439105
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/14/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 70e595705c33d9835dc9809161f79c3ac5ece870
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-sensor-data-using-hello-hive-query-console-on-hadoop-in-hdinsight"></a>HDInsight hadoop Hive lekérdezés konzol hello segítségével érzékelőadatok elemzése

Ismerje meg, hogyan tooanalyze érzékelőadatait használatával hello Hive lekérdezés konzol és a HDInsight (Hadoop) együttes, majd a Microsoft Excel hello adatok megjelenítése Power View használatával.

> [!IMPORTANT]
> Ez a dokumentum csak olyan feladaton végezhető Windows-alapú HDInsight-fürtökkel hello szükséges lépések. HDInsight csak érhető el a Windows korábbi, mint a HDInsight 3.4-es verziójához. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).


Ez a minta Hive tooprocess előzményadatokat használja, és azonosíthatja a problémákat, a fűtésrendszerek, a légkondicionáló rendszerek. Rendszerek azonosításához, amelyek nem képesek tooreliably fenntartani egy megadott hőmérsékletet hello a következő feladatok végrehajtásával:

* Hozzon létre HIVE táblák vesszővel tagolt (CSV) fájlban tárolt tooquery adatokat.
* HIVE-lekérdezések tooanalyze hello adatok létrehozásához.
* tooretrieve hello elemzett adatokat, használja a Microsoft Excel tooconnect tooHDInsight.
* toovisualize hello adatokat, használja a Power View nézetet.

![Egy hello megoldásarchitektúra ábrája](./media/hdinsight-hive-analyze-sensor-data/hvac-architecture.png)

## <a name="prerequisites"></a>Előfeltételek

* HDInsight (Hadoop)-fürtöt: lásd: [Hadoop létrehozása a HDInsight-fürtök](hdinsight-hadoop-provision-linux-clusters.md) fürt létrehozásáról további információt.
* A Microsoft Excel 2013

  > [!NOTE]
  > A Microsoft Excel szolgál az adatok vizuális [Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US).

* [A Microsoft Hive ODBC-illesztőprogram](http://www.microsoft.com/download/details.aspx?id=40886)

## <a name="toorun-hello-sample"></a>toorun hello minta

1. Webböngészőből keresse meg a következő URL-cím toohello: 

         https://<clustername>.azurehdinsight.net

    Cserélje le `<clustername>` hello nevet, a HDInsight-fürthöz.

    Amikor a rendszer kéri, hello rendszergazda felhasználónevet és jelszót ehhez a fürthöz létesítésekor használt használatával hitelesíteni.

2. Weblapról hello megnyíló, kattintson a hello **Getting Started gyűjtemény** fülre, majd a hello **mintaadatokkal megoldások** kategória, hello kattintson **érzékelő adatelemzés** minta.

    ![Első lépések gyűjtemény kép](./media/hdinsight-hive-analyze-sensor-data/getting-started-gallery.png)

3. Kövesse a hello weblap toofinish hello minta hello megjelenő utasításokat.
