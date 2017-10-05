---
title: "Az Azure Data Lake Analytics használatának első lépései az Azure Portallal | Microsoft Docs"
description: "Ebből a cikkből megtudhatja, hogyan használhatja az Azure Portalt egy Data Lake Analytics-fiók létrehozásához, egy Data Lake Analytics-feladat létrehozásához U-SQL használatával, valamint a feladat elküldéséhez. "
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
ms.openlocfilehash: 2722a2d72ed90ea0005362563ecaee30750c040a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-portal"></a><span data-ttu-id="f9669-103">Az Azure Data Lake Analytics használatának első lépései az Azure Portallal</span><span class="sxs-lookup"><span data-stu-id="f9669-103">Get started with Azure Data Lake Analytics using Azure portal</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

<span data-ttu-id="f9669-104">Ebből a cikkből megtudhatja, hogyan használhatja az Azure Portalt Azure Data Lake Analytics-fiókok létrehozásához, feladatok definiálásához [U-SQL](data-lake-analytics-u-sql-get-started.md) segítségével, valamint feladatok Data Lake Analytics-szolgáltatásokba való elküldéséhez.</span><span class="sxs-lookup"><span data-stu-id="f9669-104">Learn how to use the Azure portal to create Azure Data Lake Analytics accounts, define jobs in [U-SQL](data-lake-analytics-u-sql-get-started.md), and submit jobs to the Data Lake Analytics service.</span></span> <span data-ttu-id="f9669-105">További információk a Data Lake Analyticsről: [Azure Data Lake Analytics overview](data-lake-analytics-overview.md) (Az Azure Data Lake Analytics áttekintése).</span><span class="sxs-lookup"><span data-stu-id="f9669-105">For more information about Data Lake Analytics, see [Azure Data Lake Analytics overview](data-lake-analytics-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f9669-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f9669-106">Prerequisites</span></span>

<span data-ttu-id="f9669-107">Az oktatóanyag elindításához **Azure-előfizetéssel** kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="f9669-107">Before you begin this tutorial, you must have an **Azure subscription**.</span></span> <span data-ttu-id="f9669-108">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f9669-108">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="create-a-data-lake-analytics-account"></a><span data-ttu-id="f9669-109">Data Lake Analytics-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="f9669-109">Create a Data Lake Analytics account</span></span>

<span data-ttu-id="f9669-110">A következő lépésben egyidejűleg hozhat létre fiókot a Data Lake Analytics és a Data Lake Store szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="f9669-110">Now, you will create a Data Lake Analytics and a Data Lake Store account at the same time.</span></span>  <span data-ttu-id="f9669-111">Ez az egyszerű lépés csupán 60 másodpercet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="f9669-111">This step is simple and only takes about 60 seconds to finish.</span></span>

1. <span data-ttu-id="f9669-112">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f9669-112">Sign on to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f9669-113">Kattintson az **Új** >  **Adatok + analitika** > **Data Lake Analytics** elemre.</span><span class="sxs-lookup"><span data-stu-id="f9669-113">Click **New** >  **Data + Analytics** > **Data Lake Analytics**.</span></span>
3. <span data-ttu-id="f9669-114">Adja meg az alábbi elemek értékeit:</span><span class="sxs-lookup"><span data-stu-id="f9669-114">Select values for the following items:</span></span>
   * <span data-ttu-id="f9669-115">**Név**: Nevezze el a Data Lake Analytics-fiókot (kizárólag kisbetűk és számok használhatók).</span><span class="sxs-lookup"><span data-stu-id="f9669-115">**Name**: Name your Data Lake Analytics account (Only lower case letters and numbers allowed).</span></span>
   * <span data-ttu-id="f9669-116">**Előfizetés:** Válassza ki az Analytics-fiókhoz használt Azure-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="f9669-116">**Subscription**: Choose the Azure subscription used for the Analytics account.</span></span>
   * <span data-ttu-id="f9669-117">**Erőforráscsoport**.</span><span class="sxs-lookup"><span data-stu-id="f9669-117">**Resource Group**.</span></span> <span data-ttu-id="f9669-118">Válasszon ki egy meglévő Azure-erőforráscsoportot, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="f9669-118">Select an existing Azure Resource Group or create a new one.</span></span>
   * <span data-ttu-id="f9669-119">**Hely**.</span><span class="sxs-lookup"><span data-stu-id="f9669-119">**Location**.</span></span> <span data-ttu-id="f9669-120">Válasszon egy Azure-adatközpontot az Azure Data Lake Analytics-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="f9669-120">Select an Azure data center for the Data Lake Analytics account.</span></span>
   * <span data-ttu-id="f9669-121">**Data Lake Store**: Az útmutatót követve hozzon létre egy új Data Lake Store-fiókot, vagy válasszon ki egy már meglévőt.</span><span class="sxs-lookup"><span data-stu-id="f9669-121">**Data Lake Store**: Follow the instruction to create a new Data Lake Store account, or select an existing one.</span></span> 
4. <span data-ttu-id="f9669-122">Igény szerint tarifacsomagot is választhat a Data Lake Analytics-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="f9669-122">Optionally, select a pricing tier for your Data Lake Analytics account.</span></span>
5. <span data-ttu-id="f9669-123">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="f9669-123">Click **Create**.</span></span> 


## <a name="your-first-u-sql-script"></a><span data-ttu-id="f9669-124">Az első U-SQL-szkript</span><span class="sxs-lookup"><span data-stu-id="f9669-124">Your first U-SQL script</span></span>

<span data-ttu-id="f9669-125">A következő szöveg egy igen egyszerű U-SQL-szkript.</span><span class="sxs-lookup"><span data-stu-id="f9669-125">The following text is a very simple U-SQL script.</span></span> <span data-ttu-id="f9669-126">Az egyetlen funkciója, hogy meghatároz egy kisebb adatkészletet a szkriptben, amelyet aztán egy `/data.csv` nevű fájlként kiír a Data Lake Store-ba.</span><span class="sxs-lookup"><span data-stu-id="f9669-126">All it does is define a small dataset within the script and then write that dataset out to the default Data Lake Store as a file called `/data.csv`.</span></span>

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

## <a name="submit-a-u-sql-job"></a><span data-ttu-id="f9669-127">U-SQL-feladat elküldése</span><span class="sxs-lookup"><span data-stu-id="f9669-127">Submit a U-SQL job</span></span>

1. <span data-ttu-id="f9669-128">A Data Lake Analytics-fiókban kattintson az **Új feladat** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="f9669-128">From the Data Lake Analytics account, click **New Job**.</span></span>
2. <span data-ttu-id="f9669-129">Illessze be a fent bemutatott U-SQL-szkript szövegét.</span><span class="sxs-lookup"><span data-stu-id="f9669-129">Paste in the text of the U-SQL script shown above.</span></span> 
3. <span data-ttu-id="f9669-130">Kattintson a **Feladat elküldése** elemre.</span><span class="sxs-lookup"><span data-stu-id="f9669-130">Click **Submit Job**.</span></span>   
4. <span data-ttu-id="f9669-131">Várjon, amíg a feladat állapota **Sikeres** nem lesz.</span><span class="sxs-lookup"><span data-stu-id="f9669-131">Wait until the job status changes to **Succeeded**.</span></span>
5. <span data-ttu-id="f9669-132">Sikertelen művelet esetén lásd: [Data Lake Analytics-feladatok figyelése és hibaelhárítása](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="f9669-132">If the job failed, see [Monitor and troubleshoot Data Lake Analytics jobs](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).</span></span>
6. <span data-ttu-id="f9669-133">Kattintson a **Kimenet** fülre, majd a `data.csv` elemre.</span><span class="sxs-lookup"><span data-stu-id="f9669-133">Click the **Output** tab, and then click `data.csv`.</span></span> 

## <a name="see-also"></a><span data-ttu-id="f9669-134">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="f9669-134">See also</span></span>

* <span data-ttu-id="f9669-135">Ismerkedés a U-SQL-alkalmazások fejlesztésével: [Develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md) (U-SQL-parancsfájlok fejlesztése a Data Lake Tools for Visual Studio használatával).</span><span class="sxs-lookup"><span data-stu-id="f9669-135">To get started developing U-SQL applications, see [Develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>
* <span data-ttu-id="f9669-136">A U-SQL nyelv megismerése: [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md) (Ismerkedés az Azure Data Lake Analytics U-SQL nyelvével).</span><span class="sxs-lookup"><span data-stu-id="f9669-136">To learn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>
* <span data-ttu-id="f9669-137">Felügyeleti feladatok: [Manage Azure Data Lake Analytics using Azure Portal](data-lake-analytics-manage-use-portal.md) (Az Azure Data Lake Analytics kezelése az Azure Portallal).</span><span class="sxs-lookup"><span data-stu-id="f9669-137">For management tasks, see [Manage Azure Data Lake Analytics using Azure portal](data-lake-analytics-manage-use-portal.md).</span></span>
