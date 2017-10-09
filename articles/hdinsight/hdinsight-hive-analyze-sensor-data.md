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
# <a name="analyze-sensor-data-using-hello-hive-query-console-on-hadoop-in-hdinsight"></a><span data-ttu-id="42df6-103">HDInsight hadoop Hive lekérdezés konzol hello segítségével érzékelőadatok elemzése</span><span class="sxs-lookup"><span data-stu-id="42df6-103">Analyze sensor data using hello Hive Query Console on Hadoop in HDInsight</span></span>

<span data-ttu-id="42df6-104">Ismerje meg, hogyan tooanalyze érzékelőadatait használatával hello Hive lekérdezés konzol és a HDInsight (Hadoop) együttes, majd a Microsoft Excel hello adatok megjelenítése Power View használatával.</span><span class="sxs-lookup"><span data-stu-id="42df6-104">Learn how tooanalyze sensor data by using hello Hive Query Console with HDInsight (Hadoop), then visualize hello data in Microsoft Excel by using Power View.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="42df6-105">Ez a dokumentum csak olyan feladaton végezhető Windows-alapú HDInsight-fürtökkel hello szükséges lépések.</span><span class="sxs-lookup"><span data-stu-id="42df6-105">hello steps in this document only work with Windows-based HDInsight clusters.</span></span> <span data-ttu-id="42df6-106">HDInsight csak érhető el a Windows korábbi, mint a HDInsight 3.4-es verziójához.</span><span class="sxs-lookup"><span data-stu-id="42df6-106">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="42df6-107">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="42df6-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="42df6-108">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="42df6-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>


<span data-ttu-id="42df6-109">Ez a minta Hive tooprocess előzményadatokat használja, és azonosíthatja a problémákat, a fűtésrendszerek, a légkondicionáló rendszerek.</span><span class="sxs-lookup"><span data-stu-id="42df6-109">In this sample, you use Hive tooprocess historical data and identify problems with heating and air conditioning systems.</span></span> <span data-ttu-id="42df6-110">Rendszerek azonosításához, amelyek nem képesek tooreliably fenntartani egy megadott hőmérsékletet hello a következő feladatok végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="42df6-110">Specifically, you identify systems are not able tooreliably maintain a set temperature by performing hello following tasks:</span></span>

* <span data-ttu-id="42df6-111">Hozzon létre HIVE táblák vesszővel tagolt (CSV) fájlban tárolt tooquery adatokat.</span><span class="sxs-lookup"><span data-stu-id="42df6-111">Create HIVE tables tooquery data stored in comma-separated value (CSV) files.</span></span>
* <span data-ttu-id="42df6-112">HIVE-lekérdezések tooanalyze hello adatok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="42df6-112">Create HIVE queries tooanalyze hello data.</span></span>
* <span data-ttu-id="42df6-113">tooretrieve hello elemzett adatokat, használja a Microsoft Excel tooconnect tooHDInsight.</span><span class="sxs-lookup"><span data-stu-id="42df6-113">tooretrieve hello analyzed data, use Microsoft Excel tooconnect tooHDInsight.</span></span>
* <span data-ttu-id="42df6-114">toovisualize hello adatokat, használja a Power View nézetet.</span><span class="sxs-lookup"><span data-stu-id="42df6-114">toovisualize hello data, use Power View.</span></span>

![Egy hello megoldásarchitektúra ábrája](./media/hdinsight-hive-analyze-sensor-data/hvac-architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="42df6-116">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="42df6-116">Prerequisites</span></span>

* <span data-ttu-id="42df6-117">HDInsight (Hadoop)-fürtöt: lásd: [Hadoop létrehozása a HDInsight-fürtök](hdinsight-hadoop-provision-linux-clusters.md) fürt létrehozásáról további információt.</span><span class="sxs-lookup"><span data-stu-id="42df6-117">An HDInsight (Hadoop) cluster: See [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md) for information about creating a cluster.</span></span>
* <span data-ttu-id="42df6-118">A Microsoft Excel 2013</span><span class="sxs-lookup"><span data-stu-id="42df6-118">Microsoft Excel 2013</span></span>

  > [!NOTE]
  > <span data-ttu-id="42df6-119">A Microsoft Excel szolgál az adatok vizuális [Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US).</span><span class="sxs-lookup"><span data-stu-id="42df6-119">Microsoft Excel is used for data visualization with [Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US).</span></span>

* [<span data-ttu-id="42df6-120">A Microsoft Hive ODBC-illesztőprogram</span><span class="sxs-lookup"><span data-stu-id="42df6-120">Microsoft Hive ODBC Driver</span></span>](http://www.microsoft.com/download/details.aspx?id=40886)

## <a name="toorun-hello-sample"></a><span data-ttu-id="42df6-121">toorun hello minta</span><span class="sxs-lookup"><span data-stu-id="42df6-121">toorun hello sample</span></span>

1. <span data-ttu-id="42df6-122">Webböngészőből keresse meg a következő URL-cím toohello:</span><span class="sxs-lookup"><span data-stu-id="42df6-122">From your web browser, navigate toohello following URL:</span></span> 

         https://<clustername>.azurehdinsight.net

    <span data-ttu-id="42df6-123">Cserélje le `<clustername>` hello nevet, a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="42df6-123">Replace `<clustername>` with hello name of your HDInsight cluster.</span></span>

    <span data-ttu-id="42df6-124">Amikor a rendszer kéri, hello rendszergazda felhasználónevet és jelszót ehhez a fürthöz létesítésekor használt használatával hitelesíteni.</span><span class="sxs-lookup"><span data-stu-id="42df6-124">When prompted, authenticate by using hello administrator user name and password you used when provisioning this cluster.</span></span>

2. <span data-ttu-id="42df6-125">Weblapról hello megnyíló, kattintson a hello **Getting Started gyűjtemény** fülre, majd a hello **mintaadatokkal megoldások** kategória, hello kattintson **érzékelő adatelemzés** minta.</span><span class="sxs-lookup"><span data-stu-id="42df6-125">From hello web page that opens, click hello **Getting Started Gallery** tab, and then under hello **Solutions with Sample Data** category, click hello **Sensor Data Analysis** sample.</span></span>

    ![Első lépések gyűjtemény kép](./media/hdinsight-hive-analyze-sensor-data/getting-started-gallery.png)

3. <span data-ttu-id="42df6-127">Kövesse a hello weblap toofinish hello minta hello megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="42df6-127">Follow hello instructions provided on hello web page toofinish hello sample.</span></span>
