---
title: "Multi-földrajzi súgó dokumentumai |} Microsoft Docs"
description: "Megtudhatja, hogyan hozhat létre munkaterületet, és egy webszolgáltatás-bővítmény közzététele az Azure-régióban a Dél központi az Amerikai Egyesült Államok (SCUS) a különböző Azure-régiót."
services: machine-learning
documentationcenter: 
author: tedway
manager: jhubbard
editor: rmca14
tags: 
ms.assetid: ed0ca8a8-fa53-4e56-b824-2d7e44641967
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/6/2017
ms.author: tedway; neerajkh
ms.openlocfilehash: 32f80863308c00c32b1496bb92d39a7ae7d0cc6f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="multi-geo-help-documentation"></a><span data-ttu-id="affba-103">Több régióra kiterjedő súgódokumentáció</span><span class="sxs-lookup"><span data-stu-id="affba-103">Multi-Geo Help documentation</span></span>
<span data-ttu-id="affba-104">Ez a cikk ismerteti, hogyan hozhat létre munkaterületet, és egy webszolgáltatás-bővítmény közzététele a különböző Azure-régiók.</span><span class="sxs-lookup"><span data-stu-id="affba-104">This article describes how you can create a workspace and publish a web service in different Azure regions.</span></span>  <span data-ttu-id="affba-105">A [Azure termékek régió lap](https://azure.microsoft.com/en-us/regions/services/) sorolja fel a régiókban, ahol az Azure Machine Learning áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="affba-105">The [Azure Products by Region page](https://azure.microsoft.com/en-us/regions/services/) lists regions where Azure Machine Learning is available.</span></span>

## <a name="create-a-workspace"></a><span data-ttu-id="affba-106">Munkaterületek létrehozása</span><span class="sxs-lookup"><span data-stu-id="affba-106">Create a workspace</span></span>
1. <span data-ttu-id="affba-107">Jelentkezzen be a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="affba-107">Sign in to the Azure Classic Portal.</span></span>
2. <span data-ttu-id="affba-108">Kattintson a **+ új** > **ADATSZOLGÁLTATÁSOK** > **gépi tanulás** > **gyors létrehozás**.</span><span class="sxs-lookup"><span data-stu-id="affba-108">Click **+NEW** > **DATA SERVICES** > **MACHINE LEARNING** > **QUICK CREATE**.</span></span>  <span data-ttu-id="affba-109">A **hely** válassza ki, mint egy másik régióban **Délkelet-Ázsia**.</span><span class="sxs-lookup"><span data-stu-id="affba-109">Under **LOCATION** select another region, such as **Southeast Asia**.</span></span>
   <span data-ttu-id="affba-110">![1 kép multi-földrajzi Súgó][1]</span><span class="sxs-lookup"><span data-stu-id="affba-110">![Multi-Geo Help image 1][1]</span></span>
3. <span data-ttu-id="affba-111">Válassza ki a munkaterületet, és kattintson a **bejelentkezés az ML Studio**.</span><span class="sxs-lookup"><span data-stu-id="affba-111">Select the workspace, and then click **Sign-in to ML Studio**.</span></span>
   <span data-ttu-id="affba-112">![2 kép multi-földrajzi Súgó][2]</span><span class="sxs-lookup"><span data-stu-id="affba-112">![Multi-Geo Help image 2][2]</span></span>
4. <span data-ttu-id="affba-113">Most már rendelkezik egy munkaterület egy másik régióban, csakúgy, mint bármely más munkaterület használhat.</span><span class="sxs-lookup"><span data-stu-id="affba-113">You now have a workspace in another region that you may use just like any other workspace.</span></span> <span data-ttu-id="affba-114">A munkaterületek között válthat, keresse meg a képernyő jobb felső sarkában.</span><span class="sxs-lookup"><span data-stu-id="affba-114">To switch among your workspaces, look to the upper right of your screen.</span></span> <span data-ttu-id="affba-115">Kattintson a legördülő listában, válassza ki azt a régiót, és válassza ki a munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="affba-115">Click the dropdown, select the region, and then select the workspace.</span></span> <span data-ttu-id="affba-116">Helyi a munkaterület-régióban.</span><span class="sxs-lookup"><span data-stu-id="affba-116">Everything is local to the workspace region.</span></span>  <span data-ttu-id="affba-117">Például a létrehozott egy olyan munkaterületről, webszolgáltatások mindegyikét lesz ugyanabban a régióban található, a munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="affba-117">For example, all of your web services created from a workspace will be in the same region the workspace is located in.</span></span>
   <span data-ttu-id="affba-118">![3 kép multi-földrajzi Súgó][3]</span><span class="sxs-lookup"><span data-stu-id="affba-118">![Multi-Geo Help image 3][3]</span></span>

## <a name="open-an-experiment-from-gallery"></a><span data-ttu-id="affba-119">Egy kísérlet megnyitásához a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="affba-119">Open an experiment from Gallery</span></span>
<span data-ttu-id="affba-120">Egy kísérlet megnyitásához a katalógusból, ha át kívánja másolni a kísérletet, és melyik régióban is kiválaszthatja.</span><span class="sxs-lookup"><span data-stu-id="affba-120">If you open an experiment from Gallery, you can also select which region you want to copy the experiment to.</span></span>

![4 kép multi-földrajzi Súgó][4a]

## <a name="web-service-management"></a><span data-ttu-id="affba-122">Rendszerfelügyeleti webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="affba-122">Web service management</span></span>
<span data-ttu-id="affba-123">Programozott módon webes szolgáltatások kezelésére, többek között az átképezési, a régióspecifikus cím használata:</span><span class="sxs-lookup"><span data-stu-id="affba-123">To programmatically manage web services, such as for retraining, use the region-specific address:</span></span>

* <span data-ttu-id="affba-124">https://asiasoutheast.Management.azureml.NET</span><span class="sxs-lookup"><span data-stu-id="affba-124">https://asiasoutheast.management.azureml.net</span></span>
* <span data-ttu-id="affba-125">https://europewest.Management.azureml.NET</span><span class="sxs-lookup"><span data-stu-id="affba-125">https://europewest.management.azureml.net</span></span>

### <a name="things-to-note"></a><span data-ttu-id="affba-126">Ügyeljen a következőkre</span><span class="sxs-lookup"><span data-stu-id="affba-126">Things to note</span></span>
1. <span data-ttu-id="affba-127">Kísérletek között munkaterületek, így ugyanahhoz a régióhoz tartozó csak másolhatja.</span><span class="sxs-lookup"><span data-stu-id="affba-127">You can only copy experiments between workspaces that belong to the same region this way.</span></span> <span data-ttu-id="affba-128">Ha másolja kísérlet között különböző régiókban munkaterületek, használhatja a [PowerShell](http://aka.ms/amlps) parancsmag [ *másolási-AmlExperiment* ](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) hajthatja végre, hogy.</span><span class="sxs-lookup"><span data-stu-id="affba-128">If you need to copy experiment across workspaces in different regions, you can use the [PowerShell](http://aka.ms/amlps) commandlet [*Copy-AmlExperiment*](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) to accomplish that.</span></span> <span data-ttu-id="affba-129">Egy másik megoldás, hogy a kísérletben a gyűjteményelem közzététele fel nem sorolt módban, majd nyissa meg a munkaterület más régióban.</span><span class="sxs-lookup"><span data-stu-id="affba-129">Another workaround is to publish the experiment into Gallery in unlisted mode, then open it in the workspace from the other region.</span></span>
2. <span data-ttu-id="affba-130">A régió választó csak egy régió munkaterületek jelennek egyszerre.</span><span class="sxs-lookup"><span data-stu-id="affba-130">The region selector will only show workspaces for one region at a time.</span></span>  
3. <span data-ttu-id="affba-131">Egy szabad munkaterület vagy Vendég Access (névtelen) munkaterületen létrejön és Dél központi USA tárolva</span><span class="sxs-lookup"><span data-stu-id="affba-131">A free workspace or Guest Access (anonymous) workspace will be created and hosted in South Central U.S.</span></span>  
4. <span data-ttu-id="affba-132">Egy olyan munkaterületről, Délkelet-Ázsiában található telepített webszolgáltatások is lehet üzemeltetni Délkelet-Ázsia.</span><span class="sxs-lookup"><span data-stu-id="affba-132">Web services deployed from a workspace in Southeast Asia will also be hosted in Southeast Asia.</span></span>  

## <a name="more-information"></a><span data-ttu-id="affba-133">További információ</span><span class="sxs-lookup"><span data-stu-id="affba-133">More information</span></span>
<span data-ttu-id="affba-134">Tegyen fel kérdést az a [Azure Machine Learning fórum](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).</span><span class="sxs-lookup"><span data-stu-id="affba-134">Ask a question on the [Azure Machine Learning forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).</span></span>

<!--Image references-->
[1]: ./media/machine-learning-multi-geo/multi-geo_1.png
[2]: ./media/machine-learning-multi-geo/multi-geo_2.png
[3]: ./media/machine-learning-multi-geo/multi-geo_3.png
[4a]: ./media/machine-learning-multi-geo/multi-geo_4a.png
