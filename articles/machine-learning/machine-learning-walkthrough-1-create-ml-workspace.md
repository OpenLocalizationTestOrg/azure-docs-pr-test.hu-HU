---
title: "1. lépés: A Machine Learning-munkaterület létrehozása |} Microsoft Docs"
description: "1. lépésében hello fejlesztése egy prediktív megoldás forgatókönyv: megtudhatja, hogyan tooset egy új Azure Machine Learning Studio munkaterületet."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: b3c97e3d-16ba-4e42-9657-2562854a1e04
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 93d2e240826db9768e85b00cab0eb62510b4efb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-1-create-a-machine-learning-workspace"></a><span data-ttu-id="d31f5-103">Az útmutató 1. lépése: Machine Learning-munkaterület létrehozása</span><span class="sxs-lookup"><span data-stu-id="d31f5-103">Walkthrough Step 1: Create a Machine Learning workspace</span></span>
<span data-ttu-id="d31f5-104">Ez az első lépés hello hello forgatókönyv [az Azure Machine Learning a prediktív elemzési megoldás fejlesztése](machine-learning-walkthrough-develop-predictive-solution.md).</span><span class="sxs-lookup"><span data-stu-id="d31f5-104">This is hello first step of hello walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md).</span></span>

1. <span data-ttu-id="d31f5-105">**Machine Learning-munkaterület létrehozása**</span><span class="sxs-lookup"><span data-stu-id="d31f5-105">**Create a Machine Learning workspace**</span></span>
2. [<span data-ttu-id="d31f5-106">Meglévő adatok feltöltése</span><span class="sxs-lookup"><span data-stu-id="d31f5-106">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. [<span data-ttu-id="d31f5-107">Új kísérlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="d31f5-107">Create a new experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. [<span data-ttu-id="d31f5-108">Betanítása és kiértékelése hello modellek</span><span class="sxs-lookup"><span data-stu-id="d31f5-108">Train and evaluate hello models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="d31f5-109">Hello webszolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="d31f5-109">Deploy hello Web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. [<span data-ttu-id="d31f5-110">Hozzáférés hello webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="d31f5-110">Access hello Web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

- - -
<!-- This needs toobe updated toorefer toohello new way of creating workspaces in hello Ibiza portal -->

<span data-ttu-id="d31f5-111">a Machine Learning Studio toouse, kell toohave Microsoft Azure Machine Learning munkaterülettel.</span><span class="sxs-lookup"><span data-stu-id="d31f5-111">toouse Machine Learning Studio, you need toohave a Microsoft Azure Machine Learning workspace.</span></span> <span data-ttu-id="d31f5-112">A munkaterület toocreate kell hello eszközöket tartalmazza, kezelése és kísérletek közzététele.</span><span class="sxs-lookup"><span data-stu-id="d31f5-112">This workspace contains hello tools you need toocreate, manage, and publish experiments.</span></span>  

<!--
## toocreate a workspace
1. Sign in toohello [Azure classic portal](https://manage.windowsazure.com).
2. In hello  Azure services panel, click **MACHINE LEARNING**.  
   ![Create workspace][1]
3. Click **CREATE AN ML WORKSPACE**.
4. On hello **QUICK CREATE** page, enter your workspace information and then click **CREATE AN ML WORKSPACE**.
-->

<span data-ttu-id="d31f5-113">az Azure-előfizetése rendszergazdája hello toocreate hello munkaterület kell, és adja meg a tulajdonos vagy közreműködő.</span><span class="sxs-lookup"><span data-stu-id="d31f5-113">hello administrator for your Azure subscription needs toocreate hello workspace and then add you as an owner or contributor.</span></span> <span data-ttu-id="d31f5-114">További információkért lásd: [létrehozása és a megosztáshoz egy Azure Machine Learning munkaterülettel](machine-learning-create-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="d31f5-114">For details, see [Create and share an Azure Machine Learning workspace](machine-learning-create-workspace.md).</span></span>

<span data-ttu-id="d31f5-115">A munkaterület létrehozását követően nyissa meg a Machine Learning Studio ([https://studio.azureml.net/Home](https://studio.azureml.net/Home)).</span><span class="sxs-lookup"><span data-stu-id="d31f5-115">After your workspace is created, open Machine Learning Studio ([https://studio.azureml.net/Home](https://studio.azureml.net/Home)).</span></span> <span data-ttu-id="d31f5-116">Ha egynél több munkaterület, igény szerint hello munkaterület a hello eszköztár hello ablak jobb felső sarkában hello.</span><span class="sxs-lookup"><span data-stu-id="d31f5-116">If you have more than one workspace, you can select hello workspace in hello toolbar in hello upper-right corner of hello window.</span></span>

![A Studio munkaterület kiválasztása][2]

> [!TIP]
> <span data-ttu-id="d31f5-118">Ha hello munkaterület tulajdonos történt, azzal mások dolgozik hello kísérletek megoszthatja toohello munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="d31f5-118">If you were made an owner of hello workspace, you can share hello experiments you're working on by inviting others toohello workspace.</span></span> <span data-ttu-id="d31f5-119">Ehhez a Machine Learning Studióban a hello **beállítások** lap.</span><span class="sxs-lookup"><span data-stu-id="d31f5-119">You can do this in Machine Learning Studio on hello **SETTINGS** page.</span></span> <span data-ttu-id="d31f5-120">Egyszerűen hello Microsoft-fiókkal vagy szervezeti fiókkal minden felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="d31f5-120">You just need hello Microsoft account or organizational account for each user.</span></span>
> 
> <span data-ttu-id="d31f5-121">A hello **beállítások** kattintson **felhasználók**, kattintson a **több felhasználók MEGHÍVÁSA** hello ablak hello alján.</span><span class="sxs-lookup"><span data-stu-id="d31f5-121">On hello **SETTINGS** page, click **USERS**, then click **INVITE MORE USERS** at hello bottom of hello window.</span></span>
> 
> 

- - -
<span data-ttu-id="d31f5-122">**Következő: [meglévő adatok feltöltése](machine-learning-walkthrough-2-upload-data.md)**</span><span class="sxs-lookup"><span data-stu-id="d31f5-122">**Next: [Upload existing data](machine-learning-walkthrough-2-upload-data.md)**</span></span>

[1]: ./media/machine-learning-walkthrough-1-create-ml-workspace/create1.png
[2]: ./media/machine-learning-walkthrough-1-create-ml-workspace/open-workspace.png
