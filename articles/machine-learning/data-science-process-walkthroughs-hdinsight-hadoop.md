---
title: "aaaHDInsight Hadoop adatok tudományos forgatókönyvek Azure a Hive használatával |} Microsoft Docs"
description: "Példák hello Team adatok tudományos folyamat, amely az Azure HDInsight Hadoop toodo prediktív elemzési Hive hello segítségével ismerteti."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: bradsev
ms.openlocfilehash: 77efbe4ea6377f309987849d9f44e8b2b859ae9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="hdinsight-hadoop-data-science-walkthroughs-using-hive-on-azure"></a><span data-ttu-id="a0452-103">HDInsight Hadoop adatok tudományos forgatókönyvek az Azure-on a Hive eszközzel</span><span class="sxs-lookup"><span data-stu-id="a0452-103">HDInsight Hadoop data science walkthroughs using Hive on Azure</span></span> 

<span data-ttu-id="a0452-104">Ezek a forgatókönyvek a Hive használata a egy HDInsight Hadoop fürthöz toodo prediktív elemzési.</span><span class="sxs-lookup"><span data-stu-id="a0452-104">These walkthroughs use Hive with an HDInsight Hadoop cluster toodo predictive analytics.</span></span> <span data-ttu-id="a0452-105">Hello Team adatok tudományos folyamat leírt hello lépéseket követik.</span><span class="sxs-lookup"><span data-stu-id="a0452-105">They follow hello steps outlined in hello Team Data Science Process.</span></span> <span data-ttu-id="a0452-106">Hello Team adatok tudományos folyamat áttekintését lásd: [adatok tudományos folyamat](data-science-process-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a0452-106">For an overview of hello Team Data Science Process, see [Data Science Process](data-science-process-overview.md).</span></span> <span data-ttu-id="a0452-107">Egy bevezető tooAzure HDInsight, lásd: [bemutatása tooAzure HDInsight Hadoop technológiai területekre, és a Hadoop-fürtök hello](../hdinsight/hdinsight-hadoop-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a0452-107">For an introduction tooAzure HDInsight, see [Introduction tooAzure HDInsight, hello Hadoop technology stack, and Hadoop clusters](../hdinsight/hdinsight-hadoop-introduction.md).</span></span>

<span data-ttu-id="a0452-108">További adatok tudományos forgatókönyvek, amelyek hello Team adatok tudományos folyamat végrehajtása hello szerint vannak csoportosítva **platform** használnak:</span><span class="sxs-lookup"><span data-stu-id="a0452-108">Additional data science walkthroughs that execute hello Team Data Science Process are grouped by hello **platform** that they use:</span></span> 

[!INCLUDE [tdsp-walkthroughs-by-platform](../../includes/tdsp-walkthroughs-by-platform.md)]


## <a name="predict-taxi-tips-using-hive-with-hdinsight-hadoop"></a><span data-ttu-id="a0452-109">A HDInsight Hadoop Hive eszközzel taxi tippek előrejelzése</span><span class="sxs-lookup"><span data-stu-id="a0452-109">Predict taxi tips using Hive with HDInsight Hadoop</span></span>

<span data-ttu-id="a0452-110">Hello [használata a HDInsight Hadoop-fürtök](machine-learning-data-science-process-hive-walkthrough.md) forgatókönyv Győr taxikra toopredict adatait használja:</span><span class="sxs-lookup"><span data-stu-id="a0452-110">hello [Use HDInsight Hadoop clusters](machine-learning-data-science-process-hive-walkthrough.md) walkthrough uses data from New York taxis toopredict:</span></span> 

- <span data-ttu-id="a0452-111">Tipp: fizetnek e</span><span class="sxs-lookup"><span data-stu-id="a0452-111">Whether a tip is paid</span></span> 
- <span data-ttu-id="a0452-112">hello terjesztési tipp díjak</span><span class="sxs-lookup"><span data-stu-id="a0452-112">hello distribution of tip amounts</span></span>

<span data-ttu-id="a0452-113">hello forgatókönyv a Hive segítségével van megvalósítva egy [Azure HDInsight Hadoop-fürt](https://azure.microsoft.com/services/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="a0452-113">hello scenario is implemented using Hive with an [Azure HDInsight Hadoop cluster](https://azure.microsoft.com/services/hdinsight/).</span></span> <span data-ttu-id="a0452-114">Megismerheti, hogyan toostore, vizsgálatát, és egy nyilvánosan elérhető NYC taxi út visszafejtés adatait a beállítást, és díjszabás adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="a0452-114">You learn how toostore, explore, and feature engineer data from a publicly available NYC taxi trip and fare dataset.</span></span> <span data-ttu-id="a0452-115">Ekkor is használja az Azure Machine Learning toobuild, és telepítheti a hello modellek.</span><span class="sxs-lookup"><span data-stu-id="a0452-115">You also use Azure Machine Learning toobuild and deploy hello models.</span></span>

## <a name="predict-advertisement-clicks-using-hive-with-hdinsight-hadoop"></a><span data-ttu-id="a0452-116">HDInsight Hadoop Hive használata a hirdetés kattintással előrejelzése</span><span class="sxs-lookup"><span data-stu-id="a0452-116">Predict advertisement clicks using Hive with HDInsight Hadoop</span></span>

<span data-ttu-id="a0452-117">Hello [használata Azure HDInsight Hadoop-fürtök az 1 TB méretű dataset](machine-learning-data-science-process-hive-criteo-walkthrough.md) forgatókönyv használ egy nyilvánosan elérhető [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) kattintson a dataset toopredict, hogy tipp fizetnek és díjak a várható tartomány hello.</span><span class="sxs-lookup"><span data-stu-id="a0452-117">hello [Use Azure HDInsight Hadoop Clusters on a 1-TB dataset](machine-learning-data-science-process-hive-criteo-walkthrough.md) walkthrough uses a publicly available [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) click dataset toopredict whether a tip is paid and hello range of amounts expected.</span></span> <span data-ttu-id="a0452-118">hello forgatókönyv a Hive segítségével van megvalósítva egy [Azure HDInsight Hadoop-fürt](https://azure.microsoft.com/services/hdinsight/) toostore, megismerkedhet a beállítást, a visszafejtés és mintaadatokat tölthet le.</span><span class="sxs-lookup"><span data-stu-id="a0452-118">hello scenario is implemented using Hive with an [Azure HDInsight Hadoop cluster](https://azure.microsoft.com/services/hdinsight/) toostore, explore, feature engineer, and down sample data.</span></span> <span data-ttu-id="a0452-119">Az Azure Machine Learning toobuild, tanítási és pontszám bináris osztályozási modellt használ előrejelzésére, hogy a felhasználó hirdetmény kattint.</span><span class="sxs-lookup"><span data-stu-id="a0452-119">It uses Azure Machine Learning toobuild, train, and score a binary classification model predicting whether a user clicks on an advertisement.</span></span> <span data-ttu-id="a0452-120">hello forgatókönyv azt állapítja meg, hogyan toopublish ezek közül a modellek webszolgáltatásként megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="a0452-120">hello walkthrough concludes showing how toopublish one of these models as a Web service.</span></span>


## <a name="next-steps"></a><span data-ttu-id="a0452-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a0452-121">Next steps</span></span>

<span data-ttu-id="a0452-122">Hello fő összetevőből épül fel hello Team adatok tudományos folyamat tárgyalását lásd: [Team adatok tudományos folyamat áttekintése](data-science-process-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a0452-122">For a discussion of hello key components that comprise hello Team Data Science Process, see [Team Data Science Process overview](data-science-process-overview.md).</span></span>

<span data-ttu-id="a0452-123">Hello Team adatok tudományos folyamat életciklus használható toostructure leírását a tudományos projektek, lásd: [Team adatok tudományos folyamat életciklus](data-science-process-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="a0452-123">For a discussion of hello Team Data Science Process lifecycle that you can use toostructure your data science projects, see [Team Data Science Process lifecycle](data-science-process-lifecycle.md).</span></span> <span data-ttu-id="a0452-124">hello életciklus hello lépéseit, a start toofinish, amelyek projektek általában végrehajtás sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="a0452-124">hello lifecycle outlines hello steps, from start toofinish, that projects usually follow when they are executed.</span></span> 

