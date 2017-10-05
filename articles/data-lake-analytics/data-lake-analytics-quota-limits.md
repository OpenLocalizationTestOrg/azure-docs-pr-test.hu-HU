---
title: "Az Azure Data Lake Analytics kvótakorlát |} Microsoft Docs"
description: "Megtudhatja, hogyan állítsa be, és növelje a kvótakorlát az Azure Data Lake Analytics (ADLA) esetében."
services: data-lake-analytics
keywords: Azure Data Lake Analytics
documentationcenter: 
author: omidm1
editor: omidm1
ms.assetid: 49416f38-fcc7-476f-a55e-d67f3f9c1d34
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/18/2017
ms.author: omidm
ms.openlocfilehash: 957f306ea0e80b5830ad64e5ef06c6d122d9eccc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-data-lake-analytics-quota-limits"></a><span data-ttu-id="cdd67-104">Az Azure Data Lake Analytics kvótakorlát</span><span class="sxs-lookup"><span data-stu-id="cdd67-104">Azure Data Lake Analytics Quota Limits</span></span>

<span data-ttu-id="cdd67-105">Megtudhatja, hogyan állítsa be, és növelje a kvótakorlát az Azure Data Lake Analytics (ADLA) esetében.</span><span class="sxs-lookup"><span data-stu-id="cdd67-105">Learn how to adjust and increase quota limits in Azure Data Lake Analytics (ADLA) accounts.</span></span> <span data-ttu-id="cdd67-106">Ezek a korlátozások ismerete előfordulhat, hogy segítenek megérteni a U-SQL projekt viselkedését.</span><span class="sxs-lookup"><span data-stu-id="cdd67-106">Knowing these limits may help you understand your U-SQL job behavior.</span></span> <span data-ttu-id="cdd67-107">Minden kvótakorlát letölthető, így növelve a határok által a számunkra elérése.</span><span class="sxs-lookup"><span data-stu-id="cdd67-107">All quota limits are soft, so you can increase the maximum limits by reaching out to us.</span></span>

## <a name="azure-subscriptions-limits"></a><span data-ttu-id="cdd67-108">Azure-előfizetések korlátok</span><span class="sxs-lookup"><span data-stu-id="cdd67-108">Azure subscriptions limits</span></span>

<span data-ttu-id="cdd67-109">**Előfizetésenként fiókok ADLA maximális száma:** 5</span><span class="sxs-lookup"><span data-stu-id="cdd67-109">**Maximum number of ADLA accounts per subscription:**  5</span></span>

 <span data-ttu-id="cdd67-110">Ez az előfizetésenként hozhat létre ADLA fiókok maximális számát.</span><span class="sxs-lookup"><span data-stu-id="cdd67-110">This is the maximum number of ADLA accounts you can create per subscription.</span></span> <span data-ttu-id="cdd67-111">Hozzon létre egy hatodik ADLA fiókot kísérli meg, ha egy hiba jelenik "Elérte a maximális száma Data Lake Analytics-fiókok engedélyezett [5] terület előfizetési néven".</span><span class="sxs-lookup"><span data-stu-id="cdd67-111">If you try to create a sixth ADLA account, you will get an error "You have reached the maximum number of Data Lake Analytics accounts allowed (5) in region under subscription name".</span></span> <span data-ttu-id="cdd67-112">Ebben az esetben törölje az összes nem használt ADLA fiókot, vagy kapcsolatba velünk által [egy támogatási jegy megnyitásakor](#increase-maximum-quota-limits).</span><span class="sxs-lookup"><span data-stu-id="cdd67-112">In this case, either delete any unused ADLA accounts, or reach out to us by [opening a support ticket](#increase-maximum-quota-limits).</span></span>

## <a name="adla-account-limits"></a><span data-ttu-id="cdd67-113">ADLA korlátai</span><span class="sxs-lookup"><span data-stu-id="cdd67-113">ADLA account limits</span></span>

<span data-ttu-id="cdd67-114">**Fiókonként (ausztráliai) Analytics egységek maximális száma:** 250</span><span class="sxs-lookup"><span data-stu-id="cdd67-114">**Maximum number of Analytics Units (AUs) per account:** 250</span></span>

<span data-ttu-id="cdd67-115">Ez az egyidejűleg futtatható fiókjában ausztráliai maximális számát.</span><span class="sxs-lookup"><span data-stu-id="cdd67-115">This is the maximum number of AUs that can run concurrently in your account.</span></span> <span data-ttu-id="cdd67-116">A teljes ausztráliai futtatásáért az összes feladat túllépi ezt a korlátozást, ha újabb feladatok automatikusan sorba.</span><span class="sxs-lookup"><span data-stu-id="cdd67-116">If your total running AUs across all jobs exceeds this limit, newer jobs are queued automatically.</span></span> <span data-ttu-id="cdd67-117">Példa:</span><span class="sxs-lookup"><span data-stu-id="cdd67-117">For example:</span></span>

* <span data-ttu-id="cdd67-118">Ha csak egy feladat még fut 250 ausztráliai, amikor egy másik feladat azt a feladat-várólistán a rendszer megvárja, amíg az első feladat befejeződik.</span><span class="sxs-lookup"><span data-stu-id="cdd67-118">If you have only one job running with 250 AUs, when you submit a second job it will wait in the job queue until the first job completes.</span></span>
* <span data-ttu-id="cdd67-119">Ha már rendelkezik futó feladatokkal öt és a egyes 50 ausztráliai, amikor 20 igénylő hatodik feladat elküldése a feladat-várólistán arra vár, amíg nincsenek 20 ausztráliai ausztráliai érhető el.</span><span class="sxs-lookup"><span data-stu-id="cdd67-119">If you already have five jobs running and each is using 50 AUs, when you submit a sixth job that needs 20 AUs it waits in the job queue until there are 20 AUs available.</span></span>

<span data-ttu-id="cdd67-120">**Fiók száma párhuzamos U-SQL feladatok maximális száma:** 20</span><span class="sxs-lookup"><span data-stu-id="cdd67-120">**Maximum number of concurrent U-SQL jobs per account:** 20</span></span>

<span data-ttu-id="cdd67-121">Ez az a fiók egyidejűleg futtatható feladatok maximális számát.</span><span class="sxs-lookup"><span data-stu-id="cdd67-121">This is the maximum number of jobs that can run concurrently in your account.</span></span> <span data-ttu-id="cdd67-122">Ezt az értéket, fent újabb feladatok várólistára automatikusan.</span><span class="sxs-lookup"><span data-stu-id="cdd67-122">Above this value, newer jobs are queued automatically.</span></span>

## <a name="adjust-adla-quota-limits-per-account"></a><span data-ttu-id="cdd67-123">Állítsa be a kvótakorlát ADLA fiókonként</span><span class="sxs-lookup"><span data-stu-id="cdd67-123">Adjust ADLA quota limits per account</span></span>

1. <span data-ttu-id="cdd67-124">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="cdd67-124">Sign on to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="cdd67-125">Válasszon ki egy meglévő ADLA fiókot.</span><span class="sxs-lookup"><span data-stu-id="cdd67-125">Choose an existing ADLA account.</span></span>
3. <span data-ttu-id="cdd67-126">Kattintson a **Tulajdonságok** elemre.</span><span class="sxs-lookup"><span data-stu-id="cdd67-126">Click **Properties**.</span></span>
4. <span data-ttu-id="cdd67-127">Állítsa be **párhuzamossági** és **egyidejűleg futó feladatainak** az igényeinek.</span><span class="sxs-lookup"><span data-stu-id="cdd67-127">Adjust **Parallelism** and **Concurrent Jobs** to suit your needs.</span></span>

    ![Azure Data Lake Analytics portál panel](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-properties.png)

## <a name="increase-maximum-quota-limits"></a><span data-ttu-id="cdd67-129">Maximális kvótakorlát növelése</span><span class="sxs-lookup"><span data-stu-id="cdd67-129">Increase maximum quota limits</span></span>

1. <span data-ttu-id="cdd67-130">Nyissa meg a támogatási kérelmet az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="cdd67-130">Open a support request in Azure Portal.</span></span>

    ![Azure Data Lake Analytics portál panel](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-help-support.png)

    ![Azure Data Lake Analytics portál panel](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request.png)
2. <span data-ttu-id="cdd67-133">Adja meg a probléma **kvóta**.</span><span class="sxs-lookup"><span data-stu-id="cdd67-133">Select the issue type **Quota**.</span></span>
3. <span data-ttu-id="cdd67-134">Válassza ki a **előfizetés** (Ellenőrizze, hogy az nem a "" próbaelőfizetést).</span><span class="sxs-lookup"><span data-stu-id="cdd67-134">Select your **Subscription** (make sure it is not a "trial" subscription).</span></span>
4. <span data-ttu-id="cdd67-135">Válassza ki a kvótatípus **Data Lake Analytics**.</span><span class="sxs-lookup"><span data-stu-id="cdd67-135">Select quota type **Data Lake Analytics**.</span></span>

    ![Azure Data Lake Analytics portál panel](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-basics.png)

5. <span data-ttu-id="cdd67-137">A probléma paneljén ismertetik a kért növekedése korlát **részletek** , miért van szükség a további kapacitást.</span><span class="sxs-lookup"><span data-stu-id="cdd67-137">In the problem blade, explain your requested increase limit with **Details** of why you need this extra capacity.</span></span>

    ![Azure Data Lake Analytics portál panel](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-details.png)

6. <span data-ttu-id="cdd67-139">Ellenőrizze a kapcsolattartási adatokat, és a támogatási kérelem létrehozása.</span><span class="sxs-lookup"><span data-stu-id="cdd67-139">Verify your contact information and create the support request.</span></span>

<span data-ttu-id="cdd67-140">A Microsoft nem ellenőrzi kérelmét, és megkísérli a lehető leghamarabb megfeleljen az üzleti igényeknek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="cdd67-140">Microsoft reviews your request and tries to accommodate your business needs as soon as possible.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cdd67-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cdd67-141">Next steps</span></span>

* [<span data-ttu-id="cdd67-142">A Microsoft Azure Data Lake Analytics áttekintése</span><span class="sxs-lookup"><span data-stu-id="cdd67-142">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="cdd67-143">Az Azure Data Lake Analytics kezelése az Azure PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="cdd67-143">Manage Azure Data Lake Analytics using Azure PowerShell</span></span>](data-lake-analytics-manage-use-powershell.md)
* [<span data-ttu-id="cdd67-144">Az Azure Data Lake Analytics-feladatok figyelése és hibaelhárítása az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="cdd67-144">Monitor and troubleshoot Azure Data Lake Analytics jobs using Azure portal</span></span>](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
