---
title: "aaaCopy gépi tanulási a példa kísérletek - Azure |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse például machine learning kísérleteket toocreate új kísérletek Cortana Intelligence Gallery és a Microsoft Azure Machine Learning."
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
ms.openlocfilehash: 555f05cf9af2040d1703707f7583396d5da9f156
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="copy-example-experiments-toocreate-new-machine-learning-experiments"></a><span data-ttu-id="ebf22-104">Példa kísérletek toocreate új gépi tanulási kísérletekhez másolása</span><span class="sxs-lookup"><span data-stu-id="ebf22-104">Copy example experiments toocreate new machine learning experiments</span></span>
<span data-ttu-id="ebf22-105">Ismerje meg, hogyan toostart példa a kísérleteket a [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) gépi tanulási kísérletekhez teljesen új létrehozása helyett.</span><span class="sxs-lookup"><span data-stu-id="ebf22-105">Learn how toostart with example experiments from [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) instead of creating machine learning experiments from scratch.</span></span> <span data-ttu-id="ebf22-106">Hello példák toobuild használhatja a saját gépi tanulási a megoldás.</span><span class="sxs-lookup"><span data-stu-id="ebf22-106">You can use hello examples toobuild your own machine learning solution.</span></span>

<span data-ttu-id="ebf22-107">hello gyűjteménye van például kísérletek hello Microsoft Azure Machine Learning team, valamint a hello Machine Learning Közösség által megosztott példák szerint.</span><span class="sxs-lookup"><span data-stu-id="ebf22-107">hello gallery has example experiments by hello Microsoft Azure Machine Learning team as well as examples shared by hello Machine Learning community.</span></span> <span data-ttu-id="ebf22-108">Továbbá kérdéseket tehet fel, illetve megjegyzéseket is fűzhet a kísérletekhez.</span><span class="sxs-lookup"><span data-stu-id="ebf22-108">You also can ask questions or post comments about experiments.</span></span>

<span data-ttu-id="ebf22-109">toosee hogyan toouse hello gyűjteménye, tekintse meg a hello 3 perces videót [mások munkahelyi toodo adattudomány másolása](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) hello sorozatból [Adattudomány kezdőknek](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md).</span><span class="sxs-lookup"><span data-stu-id="ebf22-109">toosee how toouse hello gallery, watch hello 3-minute video [Copy other people's work toodo data science](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) from hello series [Data Science for Beginners](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="find-an-experiment-toocopy-in-cortana-intelligence-gallery"></a><span data-ttu-id="ebf22-110">Egy kísérletben toocopy található Cortana Intelligence Gallery</span><span class="sxs-lookup"><span data-stu-id="ebf22-110">Find an experiment toocopy in Cortana Intelligence Gallery</span></span>
<span data-ttu-id="ebf22-111">toosee kísérletek is elérhető, lépjen toohello [gyűjtemény](https://gallery.cortanaintelligence.com/) kattintson **kísérletek** hello oldal hello tetején.</span><span class="sxs-lookup"><span data-stu-id="ebf22-111">toosee what experiments are available, go toohello [Gallery](https://gallery.cortanaintelligence.com/) and click **Experiments** at hello top of hello page.</span></span>

### <a name="find-hello-newest-or-most-popular-experiments"></a><span data-ttu-id="ebf22-112">Megkeresi a hello legújabb vagy a legnépszerűbb kísérletek</span><span class="sxs-lookup"><span data-stu-id="ebf22-112">Find hello newest or most popular experiments</span></span>
<span data-ttu-id="ebf22-113">Ezen a lapon látható **nemrégiben hozzáadott** kísérletek, és görgessen lefelé, toolook **népszerű tartalmakat** vagy hello legújabb **népszerű Microsoft kísérleteket**.</span><span class="sxs-lookup"><span data-stu-id="ebf22-113">On this page, you can see **Recently added** experiments, or scroll down toolook at **What's popular** or hello latest **Popular Microsoft experiments**.</span></span>

### <a name="look-for-an-experiment-that-meets-specific-requirements"></a><span data-ttu-id="ebf22-114">Adott követelményeknek megfelelő kísérletek keresése</span><span class="sxs-lookup"><span data-stu-id="ebf22-114">Look for an experiment that meets specific requirements</span></span>
<span data-ttu-id="ebf22-115">minden toobrowse kísérleteket:</span><span class="sxs-lookup"><span data-stu-id="ebf22-115">toobrowse all experiments:</span></span>

1. <span data-ttu-id="ebf22-116">Kattintson a **összes tallózása** hello oldal hello tetején.</span><span class="sxs-lookup"><span data-stu-id="ebf22-116">Click **Browse all** at hello top of hello page.</span></span>
2. <span data-ttu-id="ebf22-117">Hello bal oldalán a **pontosítás** a hello **kategóriák** szakaszban jelölje be **kísérlet** minden benne hello toosee hello gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="ebf22-117">On hello left-hand side, under **Refine by** in hello **Categories** section, select **Experiment** toosee all hello experiments in hello Gallery.</span></span>
3. <span data-ttu-id="ebf22-118">A követelményeknek megfelelő kísérletek keresése különféle módokon történhet:</span><span class="sxs-lookup"><span data-stu-id="ebf22-118">You can find experiments that meet your requirements a couple different ways:</span></span>
   * <span data-ttu-id="ebf22-119">**Válassza ki a hello bal oldalon található szűrőket.**</span><span class="sxs-lookup"><span data-stu-id="ebf22-119">**Select filters on hello left.**</span></span> <span data-ttu-id="ebf22-120">Például egy PCA alapú észlelési algoritmus toobrowse kísérletek: A **kísérlet** a kijelölt **kategóriák**, kattintson a **az összes megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="ebf22-120">For example, toobrowse experiments that use a PCA-based anomaly detection algorithm: With **Experiment** selected under **Categories**, click **Show all**.</span></span> <span data-ttu-id="ebf22-121">Ezt követően a **Algorithms Used** (Használt algoritmusok) alatt válassza a **PCA-Based Anomaly Detection** (PCA-alapú anomáliaészlelés) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ebf22-121">Then, under **Algorithms Used**, choose **PCA-Based Anomaly Detection**.</span></span> <br></br><span data-ttu-id="ebf22-122">
     ![Szűrők kiválasztása](./media/machine-learning-sample-experiments/refine-the-view.png)</span><span class="sxs-lookup"><span data-stu-id="ebf22-122">
![Select filters](./media/machine-learning-sample-experiments/refine-the-view.png)</span></span>
   * <span data-ttu-id="ebf22-123">**Hello keresőmező használata.**</span><span class="sxs-lookup"><span data-stu-id="ebf22-123">**Use hello search box.**</span></span> <span data-ttu-id="ebf22-124">Például a toofind kísérletek kapcsolódó Microsoft által közzétett toodigit használatát egy két osztályú támogatást vektoros gép algoritmust használó az "számjegyfelismerés" hello keresőmezőbe írja be.</span><span class="sxs-lookup"><span data-stu-id="ebf22-124">For example, toofind experiments contributed by Microsoft related toodigit recognition that use a two-class support vector machine algorithm, enter "digit recognition" in hello search box.</span></span> <span data-ttu-id="ebf22-125">Ezt követően válassza hello szűrők **kísérlet**, **kizárólag Microsoft tartalom**, és **két osztályú támogatást vektoros gép**:</span><span class="sxs-lookup"><span data-stu-id="ebf22-125">Then, select hello filters **Experiment**, **Microsoft content only**, and **Two-Class Support Vector Machine**:</span></span><br></br><span data-ttu-id="ebf22-126">
     ![Hello keresőmező használata](./media/machine-learning-sample-experiments/search-for-experiments.png)</span><span class="sxs-lookup"><span data-stu-id="ebf22-126">
![Use hello search box](./media/machine-learning-sample-experiments/search-for-experiments.png)</span></span>
4. <span data-ttu-id="ebf22-127">Kattintson egy kísérlet toolearn további információt.</span><span class="sxs-lookup"><span data-stu-id="ebf22-127">Click an experiment toolearn more about it.</span></span>
5. <span data-ttu-id="ebf22-128">toorun és/vagy hello kísérlet módosításához kattintson a **Megnyitás a Studióban** hello kísérlet oldalon.</span><span class="sxs-lookup"><span data-stu-id="ebf22-128">toorun and/or modify hello experiment, click **Open in Studio** on hello experiment's page.</span></span> <br></br>

    ![Példakísérlet](./media/machine-learning-sample-experiments/example-experiment.png)

    > [!NOTE]
    > <span data-ttu-id="ebf22-130">Egy kísérlet megnyitásához a Machine Learning Studio hello az első alkalommal, amikor próbálja ki ingyen, vagy egy Azure-előfizetés vásárlása.</span><span class="sxs-lookup"><span data-stu-id="ebf22-130">When you open an experiment in Machine Learning Studio for hello first time, you can try it for free or buy an Azure subscription.</span></span> [<span data-ttu-id="ebf22-131">További tudnivalók a Machine Learning Studio ingyenes és fizetős szolgáltatás hello:</span><span class="sxs-lookup"><span data-stu-id="ebf22-131">Learn about hello Machine Learning Studio free trial vs. paid service</span></span>](https://azure.microsoft.com/pricing/details/machine-learning/)
    >
    >

## <a name="create-a-new-experiment-using-an-example-as-a-template"></a><span data-ttu-id="ebf22-132">Új kísérlet létrehozása példa sablonként való használatával</span><span class="sxs-lookup"><span data-stu-id="ebf22-132">Create a new experiment using an example as a template</span></span>
<span data-ttu-id="ebf22-133">A katalógusban található egyik példát sablonként használva új kísérletet hozhat létre a Machine Learning Studióban.</span><span class="sxs-lookup"><span data-stu-id="ebf22-133">You also can create a new experiment in Machine Learning Studio using a Gallery example as a template.</span></span>

1. <span data-ttu-id="ebf22-134">Jelentkezzen be a Microsoft-fiók hitelesítő adatait toohello [Studio](https://studio.azureml.net), és kattintson a **új** toocreate kísérlet.</span><span class="sxs-lookup"><span data-stu-id="ebf22-134">Sign in with your Microsoft account credentials toohello [Studio](https://studio.azureml.net), and then click **New** toocreate an experiment.</span></span>
2. <span data-ttu-id="ebf22-135">Hello példa tartalmat keresni, és kattintson az egyik.</span><span class="sxs-lookup"><span data-stu-id="ebf22-135">Browse through hello example content and click one.</span></span>

<span data-ttu-id="ebf22-136">Egy új kísérlet jön létre a Machine Learning Studio munkaterület hello példa kísérlet használata sablonként.</span><span class="sxs-lookup"><span data-stu-id="ebf22-136">A new experiment is created in your Machine Learning Studio workspace using hello example experiment as a template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ebf22-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ebf22-137">Next steps</span></span>
* [<span data-ttu-id="ebf22-138">Adatok importálása különböző forrásokból</span><span class="sxs-lookup"><span data-stu-id="ebf22-138">Import data from various sources</span></span>](machine-learning-data-science-import-data.md)
* [<span data-ttu-id="ebf22-139">A Machine Learning hello R nyelvhez gyors üzembe helyezési útmutató</span><span class="sxs-lookup"><span data-stu-id="ebf22-139">Quickstart tutorial for hello R language in Machine Learning</span></span>](machine-learning-r-quickstart.md)
* [<span data-ttu-id="ebf22-140">Machine Learning webszolgáltatás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="ebf22-140">Deploy a Machine Learning web service</span></span>](machine-learning-publish-a-machine-learning-web-service.md)
