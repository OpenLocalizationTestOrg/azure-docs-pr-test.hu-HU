---
title: "Machine Learning példakísérletek másolása – Azure | Microsoft Docs"
description: "Ebből a cikkből megtudhatja, hogyan használhatja a Machine Learning-példakísérleteket új kísérletek létrehozására a Cortana Intelligence Gallery és a Microsoft Azure Machine Learning alkalmazásával."
keywords: "machine learning példák, mintakísérlet, machine learning minta"
services: machine-learning
documentationcenter: 
author: cjgronlund
manager: jhubbard
editor: cgronlun
ms.assetid: 81e6c1d8-682c-4db3-bfd5-d7bfb1150ff3
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/28/2017
ms.author: cgronlun
ms.openlocfilehash: 55f9bd2ed0d555a14d31bf3d262707d65bd70244
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="copy-example-experiments-to-create-new-machine-learning-experiments"></a><span data-ttu-id="15366-104">Új Machine Learning-kísérletek létrehozása példakísérletek másolásával</span><span class="sxs-lookup"><span data-stu-id="15366-104">Copy example experiments to create new machine learning experiments</span></span>
<span data-ttu-id="15366-105">Ebből a cikkből megtudhatja, hogy teljesen új Machine Learning-kísérletek létrehozása helyett hogyan kezdhet hozzá a munkához a [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) példakísérleteivel.</span><span class="sxs-lookup"><span data-stu-id="15366-105">Learn how to start with example experiments from [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) instead of creating machine learning experiments from scratch.</span></span> <span data-ttu-id="15366-106">A példák segítségével felépítheti saját Machine Learning-megoldását.</span><span class="sxs-lookup"><span data-stu-id="15366-106">You can use the examples to build your own machine learning solution.</span></span>

<span data-ttu-id="15366-107">A katalógusban szereplő példakísérleteket a Microsoft Azure Machine Learning csapata készítette, illetve a Machine Learning közösség osztotta meg.</span><span class="sxs-lookup"><span data-stu-id="15366-107">The gallery has example experiments by the Microsoft Azure Machine Learning team as well as examples shared by the Machine Learning community.</span></span> <span data-ttu-id="15366-108">Továbbá kérdéseket tehet fel, illetve megjegyzéseket is fűzhet a kísérletekhez.</span><span class="sxs-lookup"><span data-stu-id="15366-108">You also can ask questions or post comments about experiments.</span></span>

<span data-ttu-id="15366-109">A katalógus használatának megismeréséhez tekintse meg az [Adatelemzés kezdőknek](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) sorozat következő 3 perces videóját: [Copy other people's work to do data science](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) (Mások munkájának felhasználása adatelemzéshez).</span><span class="sxs-lookup"><span data-stu-id="15366-109">To see how to use the gallery, watch the 3-minute video [Copy other people's work to do data science](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) from the series [Data Science for Beginners](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="find-an-experiment-to-copy-in-cortana-intelligence-gallery"></a><span data-ttu-id="15366-110">Másolni kívánt kísérlet keresése a Cortana Intelligence Galleryben</span><span class="sxs-lookup"><span data-stu-id="15366-110">Find an experiment to copy in Cortana Intelligence Gallery</span></span>
<span data-ttu-id="15366-111">Az elérhető kísérletek megtekintéséhez a [Gallery](https://gallery.cortanaintelligence.com/) megnyitása után kattintson az **Experiments** (Kísérletek) fülre az oldal tetején.</span><span class="sxs-lookup"><span data-stu-id="15366-111">To see what experiments are available, go to the [Gallery](https://gallery.cortanaintelligence.com/) and click **Experiments** at the top of the page.</span></span>

### <a name="find-the-newest-or-most-popular-experiments"></a><span data-ttu-id="15366-112">A legújabb vagy a legnépszerűbb kísérletek megkeresése</span><span class="sxs-lookup"><span data-stu-id="15366-112">Find the newest or most popular experiments</span></span>
<span data-ttu-id="15366-113">Ezen az oldalon megtekintheti a **Recently added** (Újonnan hozzáadott) kísérleteket, illetve lefelé görgetve a **What's popular** (Népszerű) kísérleteket vagy a legújabb **Popular Microsoft experiments** (Népszerű Microsoft kísérletek) kísérletek között böngészhet.</span><span class="sxs-lookup"><span data-stu-id="15366-113">On this page, you can see **Recently added** experiments, or scroll down to look at **What's popular** or the latest **Popular Microsoft experiments**.</span></span>

### <a name="look-for-an-experiment-that-meets-specific-requirements"></a><span data-ttu-id="15366-114">Adott követelményeknek megfelelő kísérletek keresése</span><span class="sxs-lookup"><span data-stu-id="15366-114">Look for an experiment that meets specific requirements</span></span>
<span data-ttu-id="15366-115">Az összes kísérlet tallózásához:</span><span class="sxs-lookup"><span data-stu-id="15366-115">To browse all experiments:</span></span>

1. <span data-ttu-id="15366-116">Kattintson az **Összes tallózása** fülre az oldal tetején.</span><span class="sxs-lookup"><span data-stu-id="15366-116">Click **Browse all** at the top of the page.</span></span>
2. <span data-ttu-id="15366-117">A bal oldalon, a **Categories** (Kategóriák) szakasz **Refine by** (Pontosítás) területén válassza a **Experiment** (Kísérlet) fület a Katalógusban található összes kísérlet megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="15366-117">On the left-hand side, under **Refine by** in the **Categories** section, select **Experiment** to see all the experiments in the Gallery.</span></span>
3. <span data-ttu-id="15366-118">A követelményeknek megfelelő kísérletek keresése különféle módokon történhet:</span><span class="sxs-lookup"><span data-stu-id="15366-118">You can find experiments that meet your requirements a couple different ways:</span></span>
   * <span data-ttu-id="15366-119">**Válasszon ki szűrőket a bal oldalon.**</span><span class="sxs-lookup"><span data-stu-id="15366-119">**Select filters on the left.**</span></span> <span data-ttu-id="15366-120">A PCA-alapú anomáliaészlelő algoritmusokat használó kísérletek böngészéséhez válassza a **Experiment** (Kísérlet) fület a **Categories** (Kategóriák) alatt, majd kattintson az **Show all** (Összes megjelenítése) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="15366-120">For example, to browse experiments that use a PCA-based anomaly detection algorithm: With **Experiment** selected under **Categories**, click **Show all**.</span></span> <span data-ttu-id="15366-121">Ezt követően a **Algorithms Used** (Használt algoritmusok) alatt válassza a **PCA-Based Anomaly Detection** (PCA-alapú anomáliaészlelés) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="15366-121">Then, under **Algorithms Used**, choose **PCA-Based Anomaly Detection**.</span></span> <br></br><span data-ttu-id="15366-122">
     ![Szűrők kiválasztása](./media/machine-learning-sample-experiments/refine-the-view.png)</span><span class="sxs-lookup"><span data-stu-id="15366-122">
![Select filters](./media/machine-learning-sample-experiments/refine-the-view.png)</span></span>
   * <span data-ttu-id="15366-123">**Használja a keresőmezőt.**</span><span class="sxs-lookup"><span data-stu-id="15366-123">**Use the search box.**</span></span> <span data-ttu-id="15366-124">Ha például a Microsoft által közzétett, kétosztályos támogató vektorgép-algoritmust használó, számjegyfelismeréssel kapcsolatos kísérleteket szeretne keresni, a keresőmezőbe írja be a „digit recognition” (számjegyfelismerés) kifejezést.</span><span class="sxs-lookup"><span data-stu-id="15366-124">For example, to find experiments contributed by Microsoft related to digit recognition that use a two-class support vector machine algorithm, enter "digit recognition" in the search box.</span></span> <span data-ttu-id="15366-125">Ezt követően válassza ki az **Experiment** (Kísérlet), a **Microsoft content only** (Kizárólag Microsoft tartalom) és a **Two-Class Support Vector Machine** (Kétosztályos támogató vektorgép) szűrőt:</span><span class="sxs-lookup"><span data-stu-id="15366-125">Then, select the filters **Experiment**, **Microsoft content only**, and **Two-Class Support Vector Machine**:</span></span><br></br><span data-ttu-id="15366-126">
     ![Használja a keresőmezőt.](./media/machine-learning-sample-experiments/search-for-experiments.png)</span><span class="sxs-lookup"><span data-stu-id="15366-126">
![Use the search box](./media/machine-learning-sample-experiments/search-for-experiments.png)</span></span>
4. <span data-ttu-id="15366-127">Kattintson a kísérletre, ha többet szeretne megtudni róla.</span><span class="sxs-lookup"><span data-stu-id="15366-127">Click an experiment to learn more about it.</span></span>
5. <span data-ttu-id="15366-128">A kísérlet futtatásához és/vagy módosításához kattintson a **Megnyitás a Studióban** fülre a kísérlet oldalán.</span><span class="sxs-lookup"><span data-stu-id="15366-128">To run and/or modify the experiment, click **Open in Studio** on the experiment's page.</span></span> <br></br>

    ![Példakísérlet](./media/machine-learning-sample-experiments/example-experiment.png)

    > [!NOTE]
    > <span data-ttu-id="15366-130">Amikor először nyit meg egy kísérletet a Machine Learning Studióban, ingyen kipróbálhatja, vagy Azure-előfizetést vásárolhat.</span><span class="sxs-lookup"><span data-stu-id="15366-130">When you open an experiment in Machine Learning Studio for the first time, you can try it for free or buy an Azure subscription.</span></span> [<span data-ttu-id="15366-131">Információk a Machine Learning Studio ingyenes próbaverziójáról és fizetős szolgáltatásáról</span><span class="sxs-lookup"><span data-stu-id="15366-131">Learn about the Machine Learning Studio free trial vs. paid service</span></span>](https://azure.microsoft.com/pricing/details/machine-learning/)
    >
    >

## <a name="create-a-new-experiment-using-an-example-as-a-template"></a><span data-ttu-id="15366-132">Új kísérlet létrehozása példa sablonként való használatával</span><span class="sxs-lookup"><span data-stu-id="15366-132">Create a new experiment using an example as a template</span></span>
<span data-ttu-id="15366-133">A katalógusban található egyik példát sablonként használva új kísérletet hozhat létre a Machine Learning Studióban.</span><span class="sxs-lookup"><span data-stu-id="15366-133">You also can create a new experiment in Machine Learning Studio using a Gallery example as a template.</span></span>

1. <span data-ttu-id="15366-134">Jelentkezzen be a [Studióba](https://studio.azureml.net) a Microsoft-fiók hitelesítő adatait használva, és kattintson az **Új** fülre egy kísérlet létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="15366-134">Sign in with your Microsoft account credentials to the [Studio](https://studio.azureml.net), and then click **New** to create an experiment.</span></span>
2. <span data-ttu-id="15366-135">Tallózzon a példák között, és kattintson rá az egyikre.</span><span class="sxs-lookup"><span data-stu-id="15366-135">Browse through the example content and click one.</span></span>

<span data-ttu-id="15366-136">A mintakísérletet példaként használva új kísérlet jön létre a Machine Learning munkaterületén.</span><span class="sxs-lookup"><span data-stu-id="15366-136">A new experiment is created in your Machine Learning Studio workspace using the example experiment as a template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="15366-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="15366-137">Next steps</span></span>
* [<span data-ttu-id="15366-138">Adatok importálása különböző forrásokból</span><span class="sxs-lookup"><span data-stu-id="15366-138">Import data from various sources</span></span>](machine-learning-data-science-import-data.md)
* [<span data-ttu-id="15366-139">Gyors üzembe helyezési oktatóanyag az R nyelvhez a Machine Learning eszközben</span><span class="sxs-lookup"><span data-stu-id="15366-139">Quickstart tutorial for the R language in Machine Learning</span></span>](machine-learning-r-quickstart.md)
* [<span data-ttu-id="15366-140">Machine Learning webszolgáltatás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="15366-140">Deploy a Machine Learning web service</span></span>](machine-learning-publish-a-machine-learning-web-service.md)
