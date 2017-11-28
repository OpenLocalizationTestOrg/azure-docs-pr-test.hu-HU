---
title: "Elemezhet érzékelőadatokat a Hive és a Hadoop - Azure HDInsight használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan elemezhet érzékelőadatokat a Hive lekérdezés konzol és a HDInsight (Hadoop) együttes használatával, majd a Microsoft Excel PowerView az adatok megjelenítéséhez."
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
ms.openlocfilehash: 3abb71c12b4769bebd808276f8bdd832aad22d7a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="analyze-sensor-data-using-the-hive-query-console-on-hadoop-in-hdinsight"></a><span data-ttu-id="d92da-103">HDInsight hadoop Hive lekérdezés konzol használata érzékelőadatok elemzése</span><span class="sxs-lookup"><span data-stu-id="d92da-103">Analyze sensor data using the Hive Query Console on Hadoop in HDInsight</span></span>

<span data-ttu-id="d92da-104">Megtudhatja, hogyan elemezhet érzékelőadatokat a Hive lekérdezés konzol és a HDInsight (Hadoop) együttes használatával, majd a Microsoft Excelben az adatok megjelenítése Power View használatával.</span><span class="sxs-lookup"><span data-stu-id="d92da-104">Learn how to analyze sensor data by using the Hive Query Console with HDInsight (Hadoop), then visualize the data in Microsoft Excel by using Power View.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d92da-105">A jelen dokumentumban leírt lépések csak a Windows-alapú HDInsight-fürtök dolgozhat.</span><span class="sxs-lookup"><span data-stu-id="d92da-105">The steps in this document only work with Windows-based HDInsight clusters.</span></span> <span data-ttu-id="d92da-106">HDInsight csak érhető el a Windows korábbi, mint a HDInsight 3.4-es verziójához.</span><span class="sxs-lookup"><span data-stu-id="d92da-106">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="d92da-107">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="d92da-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="d92da-108">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="d92da-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>


<span data-ttu-id="d92da-109">Ez a példa a Hive előzményadatokat feldolgozni, és azonosíthatja a problémákat, a fűtésrendszerek, a légkondicionáló rendszerek használhatja.</span><span class="sxs-lookup"><span data-stu-id="d92da-109">In this sample, you use Hive to process historical data and identify problems with heating and air conditioning systems.</span></span> <span data-ttu-id="d92da-110">Pontosabban azonosíthatja a rendszerek nem képesek megbízhatóan fenntartani egy megadott hőmérsékletet az alábbi feladatokat:</span><span class="sxs-lookup"><span data-stu-id="d92da-110">Specifically, you identify systems are not able to reliably maintain a set temperature by performing the following tasks:</span></span>

* <span data-ttu-id="d92da-111">A vesszővel tagolt (CSV) fájlok adataihoz lekérdezés HIVE táblák létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d92da-111">Create HIVE tables to query data stored in comma-separated value (CSV) files.</span></span>
* <span data-ttu-id="d92da-112">Az adatok elemzése a HIVE-lekérdezések létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d92da-112">Create HIVE queries to analyze the data.</span></span>
* <span data-ttu-id="d92da-113">Az adatok lekéréséhez a Microsoft Excel használatával csatlakozhat a HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d92da-113">To retrieve the analyzed data, use Microsoft Excel to connect to HDInsight.</span></span>
* <span data-ttu-id="d92da-114">Adatok megjelenítéséhez használja a Power View nézetet.</span><span class="sxs-lookup"><span data-stu-id="d92da-114">To visualize the data, use Power View.</span></span>

![Egy a megoldásarchitektúra ábrája](./media/hdinsight-hive-analyze-sensor-data/hvac-architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="d92da-116">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d92da-116">Prerequisites</span></span>

* <span data-ttu-id="d92da-117">HDInsight (Hadoop)-fürtöt: lásd: [Hadoop létrehozása a HDInsight-fürtök](hdinsight-hadoop-provision-linux-clusters.md) fürt létrehozásáról további információt.</span><span class="sxs-lookup"><span data-stu-id="d92da-117">An HDInsight (Hadoop) cluster: See [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md) for information about creating a cluster.</span></span>
* <span data-ttu-id="d92da-118">A Microsoft Excel 2013</span><span class="sxs-lookup"><span data-stu-id="d92da-118">Microsoft Excel 2013</span></span>

  > [!NOTE]
  > <span data-ttu-id="d92da-119">A Microsoft Excel szolgál az adatok vizuális [Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US).</span><span class="sxs-lookup"><span data-stu-id="d92da-119">Microsoft Excel is used for data visualization with [Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US).</span></span>

* [<span data-ttu-id="d92da-120">A Microsoft Hive ODBC-illesztőprogram</span><span class="sxs-lookup"><span data-stu-id="d92da-120">Microsoft Hive ODBC Driver</span></span>](http://www.microsoft.com/download/details.aspx?id=40886)

## <a name="to-run-the-sample"></a><span data-ttu-id="d92da-121">A minta futtatásához</span><span class="sxs-lookup"><span data-stu-id="d92da-121">To run the sample</span></span>

1. <span data-ttu-id="d92da-122">Webböngészőből keresse meg a következő URL-címe:</span><span class="sxs-lookup"><span data-stu-id="d92da-122">From your web browser, navigate to the following URL:</span></span> 

         https://<clustername>.azurehdinsight.net

    <span data-ttu-id="d92da-123">Cserélje le a `<clustername>` kifejezést a HDInsight-fürt nevére.</span><span class="sxs-lookup"><span data-stu-id="d92da-123">Replace `<clustername>` with the name of your HDInsight cluster.</span></span>

    <span data-ttu-id="d92da-124">Amikor a rendszer kéri, a rendszergazdai felhasználónevet és a fürt létesítésekor használt jelszó használatával hitelesíteni.</span><span class="sxs-lookup"><span data-stu-id="d92da-124">When prompted, authenticate by using the administrator user name and password you used when provisioning this cluster.</span></span>

2. <span data-ttu-id="d92da-125">A megnyíló weblapon, kattintson a **Getting Started gyűjteménye** fülre, majd a a **mintaadatokkal megoldások** kategória, kattintson a **érzékelő adatelemzés** minta.</span><span class="sxs-lookup"><span data-stu-id="d92da-125">From the web page that opens, click the **Getting Started Gallery** tab, and then under the **Solutions with Sample Data** category, click the **Sensor Data Analysis** sample.</span></span>

    ![Első lépések gyűjtemény kép](./media/hdinsight-hive-analyze-sensor-data/getting-started-gallery.png)

3. <span data-ttu-id="d92da-127">Kövesse a megjelenő utasításokat a weblap a minta befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="d92da-127">Follow the instructions provided on the web page to finish the sample.</span></span>
