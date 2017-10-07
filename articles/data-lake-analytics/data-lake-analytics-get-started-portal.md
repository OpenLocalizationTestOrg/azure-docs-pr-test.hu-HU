---
title: "aaaGet elindítva az Azure Data Lake Analytics az Azure portál használatával |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure portál toocreate Data Lake Analytics-fiók, hozzon létre egy Data Lake Analytics-feladatot U-SQL használatával, valamint hello feladat elküldéséhez. "
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: b1584d16-e0d2-4019-ad1f-f04be8c5b430
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/21/2017
ms.author: edmaca
ms.openlocfilehash: 6bb54404fa42cfed25b18bc2bfb7c72e6c361149
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-portal"></a><span data-ttu-id="cdaff-103">Az Azure Data Lake Analytics használatának első lépései az Azure Portallal</span><span class="sxs-lookup"><span data-stu-id="cdaff-103">Get started with Azure Data Lake Analytics using Azure portal</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

<span data-ttu-id="cdaff-104">Ismerje meg, hogyan toouse hello Azure portál toocreate Azure Data Lake Analytics-fiókok, feladatok definiálásához [U-SQL](data-lake-analytics-u-sql-get-started.md), és küldje el a feladatok toohello Data Lake Analytics szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="cdaff-104">Learn how toouse hello Azure portal toocreate Azure Data Lake Analytics accounts, define jobs in [U-SQL](data-lake-analytics-u-sql-get-started.md), and submit jobs toohello Data Lake Analytics service.</span></span> <span data-ttu-id="cdaff-105">További információk a Data Lake Analyticsről: [Azure Data Lake Analytics overview](data-lake-analytics-overview.md) (Az Azure Data Lake Analytics áttekintése).</span><span class="sxs-lookup"><span data-stu-id="cdaff-105">For more information about Data Lake Analytics, see [Azure Data Lake Analytics overview](data-lake-analytics-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cdaff-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cdaff-106">Prerequisites</span></span>

<span data-ttu-id="cdaff-107">Az oktatóanyag elindításához **Azure-előfizetéssel** kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="cdaff-107">Before you begin this tutorial, you must have an **Azure subscription**.</span></span> <span data-ttu-id="cdaff-108">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cdaff-108">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="create-a-data-lake-analytics-account"></a><span data-ttu-id="cdaff-109">Data Lake Analytics-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="cdaff-109">Create a Data Lake Analytics account</span></span>

<span data-ttu-id="cdaff-110">Most, létrehozhat egy Data Lake Analytics és fiókot egy Data Lake Store a hello azonos idő.</span><span class="sxs-lookup"><span data-stu-id="cdaff-110">Now, you will create a Data Lake Analytics and a Data Lake Store account at hello same time.</span></span>  <span data-ttu-id="cdaff-111">Ez a lépés egyszerű, és csak 60 másodperc toofinish vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="cdaff-111">This step is simple and only takes about 60 seconds toofinish.</span></span>

1. <span data-ttu-id="cdaff-112">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="cdaff-112">Sign on toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="cdaff-113">Kattintson az **Új** >  **Adatok + analitika** > **Data Lake Analytics** elemre.</span><span class="sxs-lookup"><span data-stu-id="cdaff-113">Click **New** >  **Data + Analytics** > **Data Lake Analytics**.</span></span>
3. <span data-ttu-id="cdaff-114">Válassza ki a következő elemek hello értékeit:</span><span class="sxs-lookup"><span data-stu-id="cdaff-114">Select values for hello following items:</span></span>
   * <span data-ttu-id="cdaff-115">**Név**: Nevezze el a Data Lake Analytics-fiókot (kizárólag kisbetűk és számok használhatók).</span><span class="sxs-lookup"><span data-stu-id="cdaff-115">**Name**: Name your Data Lake Analytics account (Only lower case letters and numbers allowed).</span></span>
   * <span data-ttu-id="cdaff-116">**Előfizetés**: hello hello Analytics-fiókhoz használt Azure-előfizetés kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="cdaff-116">**Subscription**: Choose hello Azure subscription used for hello Analytics account.</span></span>
   * <span data-ttu-id="cdaff-117">**Erőforráscsoport**.</span><span class="sxs-lookup"><span data-stu-id="cdaff-117">**Resource Group**.</span></span> <span data-ttu-id="cdaff-118">Válasszon ki egy meglévő Azure-erőforráscsoportot, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="cdaff-118">Select an existing Azure Resource Group or create a new one.</span></span>
   * <span data-ttu-id="cdaff-119">**Hely**.</span><span class="sxs-lookup"><span data-stu-id="cdaff-119">**Location**.</span></span> <span data-ttu-id="cdaff-120">Válassza ki az Azure-adatközpont hello Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="cdaff-120">Select an Azure data center for hello Data Lake Analytics account.</span></span>
   * <span data-ttu-id="cdaff-121">**Data Lake Store**: hello utasítás toocreate új Data Lake Store-fiók kövesse, vagy válasszon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="cdaff-121">**Data Lake Store**: Follow hello instruction toocreate a new Data Lake Store account, or select an existing one.</span></span> 
4. <span data-ttu-id="cdaff-122">Igény szerint tarifacsomagot is választhat a Data Lake Analytics-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="cdaff-122">Optionally, select a pricing tier for your Data Lake Analytics account.</span></span>
5. <span data-ttu-id="cdaff-123">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="cdaff-123">Click **Create**.</span></span> 


## <a name="your-first-u-sql-script"></a><span data-ttu-id="cdaff-124">Az első U-SQL-szkript</span><span class="sxs-lookup"><span data-stu-id="cdaff-124">Your first U-SQL script</span></span>

<span data-ttu-id="cdaff-125">a következő szöveg hello egy nagyon egyszerű U-SQL-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="cdaff-125">hello following text is a very simple U-SQL script.</span></span> <span data-ttu-id="cdaff-126">Minden esetben hello parancsfájlban kisebb adatkészlet meghatározása, és jegyezze meg, hogy a dataset kimenő toohello alapértelmezett Data Lake Store nevű fájlba `/data.csv`.</span><span class="sxs-lookup"><span data-stu-id="cdaff-126">All it does is define a small dataset within hello script and then write that dataset out toohello default Data Lake Store as a file called `/data.csv`.</span></span>

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    too"/data.csv"
    USING Outputters.Csv();
```

## <a name="submit-a-u-sql-job"></a><span data-ttu-id="cdaff-127">U-SQL-feladat elküldése</span><span class="sxs-lookup"><span data-stu-id="cdaff-127">Submit a U-SQL job</span></span>

1. <span data-ttu-id="cdaff-128">A Data Lake Analytics-fiók hello, kattintson az **új feladat**.</span><span class="sxs-lookup"><span data-stu-id="cdaff-128">From hello Data Lake Analytics account, click **New Job**.</span></span>
2. <span data-ttu-id="cdaff-129">Hello hello szövege illessze be a fenti U-SQL parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="cdaff-129">Paste in hello text of hello U-SQL script shown above.</span></span> 
3. <span data-ttu-id="cdaff-130">Kattintson a **Feladat elküldése** elemre.</span><span class="sxs-lookup"><span data-stu-id="cdaff-130">Click **Submit Job**.</span></span>   
4. <span data-ttu-id="cdaff-131">Várjon, amíg hello feladat állapotmódosítások túl**sikeres**.</span><span class="sxs-lookup"><span data-stu-id="cdaff-131">Wait until hello job status changes too**Succeeded**.</span></span>
5. <span data-ttu-id="cdaff-132">Ha hello feladat sikertelen volt, lásd: [figyelése és hibaelhárítása a Data Lake Analytics-feladatok](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="cdaff-132">If hello job failed, see [Monitor and troubleshoot Data Lake Analytics jobs](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).</span></span>
6. <span data-ttu-id="cdaff-133">Kattintson a hello **kimeneti** fülre, majd `data.csv`.</span><span class="sxs-lookup"><span data-stu-id="cdaff-133">Click hello **Output** tab, and then click `data.csv`.</span></span> 

## <a name="see-also"></a><span data-ttu-id="cdaff-134">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="cdaff-134">See also</span></span>

* <span data-ttu-id="cdaff-135">megkezdődött a U-SQL-alkalmazások fejlesztésével tooget lásd [Data Lake Tools for Visual Studio használatával fejlesztése U-SQL-parancsfájlok](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="cdaff-135">tooget started developing U-SQL applications, see [Develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>
* <span data-ttu-id="cdaff-136">toolearn U-SQL, lásd: [Ismerkedés az Azure Data Lake Analytics U-SQL nyelv](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="cdaff-136">toolearn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>
* <span data-ttu-id="cdaff-137">Felügyeleti feladatok: [Manage Azure Data Lake Analytics using Azure Portal](data-lake-analytics-manage-use-portal.md) (Az Azure Data Lake Analytics kezelése az Azure Portallal).</span><span class="sxs-lookup"><span data-stu-id="cdaff-137">For management tasks, see [Manage Azure Data Lake Analytics using Azure portal](data-lake-analytics-manage-use-portal.md).</span></span>
