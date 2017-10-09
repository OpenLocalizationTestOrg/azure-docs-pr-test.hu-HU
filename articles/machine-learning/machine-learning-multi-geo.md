---
title: "aaaMulti-földrajzi súgót |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate munkaterület és egy webszolgáltatás-bővítmény közzététele az Azure-régióban hello Dél központi az Amerikai Egyesült Államok (SCUS) eltér az Azure-régió."
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
ms.openlocfilehash: 77b055950ebfe329131b40e5f0a2f6be1e33c51e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="multi-geo-help-documentation"></a><span data-ttu-id="ed620-103">Több régióra kiterjedő súgódokumentáció</span><span class="sxs-lookup"><span data-stu-id="ed620-103">Multi-Geo Help documentation</span></span>
<span data-ttu-id="ed620-104">Ez a cikk ismerteti, hogyan hozhat létre munkaterületet, és egy webszolgáltatás-bővítmény közzététele a különböző Azure-régiók.</span><span class="sxs-lookup"><span data-stu-id="ed620-104">This article describes how you can create a workspace and publish a web service in different Azure regions.</span></span>  <span data-ttu-id="ed620-105">Hello [Azure termékek régió lap](https://azure.microsoft.com/en-us/regions/services/) sorolja fel a régiókban, ahol az Azure Machine Learning áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="ed620-105">hello [Azure Products by Region page](https://azure.microsoft.com/en-us/regions/services/) lists regions where Azure Machine Learning is available.</span></span>

## <a name="create-a-workspace"></a><span data-ttu-id="ed620-106">Munkaterületek létrehozása</span><span class="sxs-lookup"><span data-stu-id="ed620-106">Create a workspace</span></span>
1. <span data-ttu-id="ed620-107">Jelentkezzen be a klasszikus Azure portál toohello.</span><span class="sxs-lookup"><span data-stu-id="ed620-107">Sign in toohello Azure Classic Portal.</span></span>
2. <span data-ttu-id="ed620-108">Kattintson a **+ új** > **ADATSZOLGÁLTATÁSOK** > **gépi tanulás** > **gyors létrehozás**.</span><span class="sxs-lookup"><span data-stu-id="ed620-108">Click **+NEW** > **DATA SERVICES** > **MACHINE LEARNING** > **QUICK CREATE**.</span></span>  <span data-ttu-id="ed620-109">A **hely** válassza ki, mint egy másik régióban **Délkelet-Ázsia**.</span><span class="sxs-lookup"><span data-stu-id="ed620-109">Under **LOCATION** select another region, such as **Southeast Asia**.</span></span>
   <span data-ttu-id="ed620-110">![1 kép multi-földrajzi Súgó][1]</span><span class="sxs-lookup"><span data-stu-id="ed620-110">![Multi-Geo Help image 1][1]</span></span>
3. <span data-ttu-id="ed620-111">Válasszon hello munkaterületet, és kattintson **bejelentkezési tooML Studio**.</span><span class="sxs-lookup"><span data-stu-id="ed620-111">Select hello workspace, and then click **Sign-in tooML Studio**.</span></span>
   <span data-ttu-id="ed620-112">![2 kép multi-földrajzi Súgó][2]</span><span class="sxs-lookup"><span data-stu-id="ed620-112">![Multi-Geo Help image 2][2]</span></span>
4. <span data-ttu-id="ed620-113">Most már rendelkezik egy munkaterület egy másik régióban, csakúgy, mint bármely más munkaterület használhat.</span><span class="sxs-lookup"><span data-stu-id="ed620-113">You now have a workspace in another region that you may use just like any other workspace.</span></span> <span data-ttu-id="ed620-114">a munkaterületek között tooswitch a hely toohello jobb felső sarkában a képernyőn.</span><span class="sxs-lookup"><span data-stu-id="ed620-114">tooswitch among your workspaces, look toohello upper right of your screen.</span></span> <span data-ttu-id="ed620-115">Kattintson a hello legördülő, jelölje be hello terület és jelölje ki hello munkaterület.</span><span class="sxs-lookup"><span data-stu-id="ed620-115">Click hello dropdown, select hello region, and then select hello workspace.</span></span> <span data-ttu-id="ed620-116">Helyi toohello munkaterület régióban.</span><span class="sxs-lookup"><span data-stu-id="ed620-116">Everything is local toohello workspace region.</span></span>  <span data-ttu-id="ed620-117">Például a létrehozott egy olyan munkaterületről, webszolgáltatások mindegyikét lesz hello ugyanabban a régióban hello munkaterületen található.</span><span class="sxs-lookup"><span data-stu-id="ed620-117">For example, all of your web services created from a workspace will be in hello same region hello workspace is located in.</span></span>
   <span data-ttu-id="ed620-118">![3 kép multi-földrajzi Súgó][3]</span><span class="sxs-lookup"><span data-stu-id="ed620-118">![Multi-Geo Help image 3][3]</span></span>

## <a name="open-an-experiment-from-gallery"></a><span data-ttu-id="ed620-119">Egy kísérlet megnyitásához a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="ed620-119">Open an experiment from Gallery</span></span>
<span data-ttu-id="ed620-120">Ha egy kísérlet megnyitásához a gyűjteményből, is kiválaszthatja, melyik régiót szeretné toocopy hello végeztük el.</span><span class="sxs-lookup"><span data-stu-id="ed620-120">If you open an experiment from Gallery, you can also select which region you want toocopy hello experiment to.</span></span>

![4 kép multi-földrajzi Súgó][4a]

## <a name="web-service-management"></a><span data-ttu-id="ed620-122">Rendszerfelügyeleti webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="ed620-122">Web service management</span></span>
<span data-ttu-id="ed620-123">tooprogrammatically webes szolgáltatások, például átképezési, használjon hello régióspecifikus cím kezelése:</span><span class="sxs-lookup"><span data-stu-id="ed620-123">tooprogrammatically manage web services, such as for retraining, use hello region-specific address:</span></span>

* <span data-ttu-id="ed620-124">https://asiasoutheast.Management.azureml.NET</span><span class="sxs-lookup"><span data-stu-id="ed620-124">https://asiasoutheast.management.azureml.net</span></span>
* <span data-ttu-id="ed620-125">https://europewest.Management.azureml.NET</span><span class="sxs-lookup"><span data-stu-id="ed620-125">https://europewest.management.azureml.net</span></span>

### <a name="things-toonote"></a><span data-ttu-id="ed620-126">Dolgot toonote</span><span class="sxs-lookup"><span data-stu-id="ed620-126">Things toonote</span></span>
1. <span data-ttu-id="ed620-127">Csak olyan kísérletek toohello tartozó munkaterületek között másolhat ugyanabban a régióban ily módon.</span><span class="sxs-lookup"><span data-stu-id="ed620-127">You can only copy experiments between workspaces that belong toohello same region this way.</span></span> <span data-ttu-id="ed620-128">Ha szüksége toocopy kísérlet között különböző régiókban munkaterületek, használhatja a hello [PowerShell](http://aka.ms/amlps) parancsmag [ *másolási-AmlExperiment* ](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) tooaccomplish, amely.</span><span class="sxs-lookup"><span data-stu-id="ed620-128">If you need toocopy experiment across workspaces in different regions, you can use hello [PowerShell](http://aka.ms/amlps) commandlet [*Copy-AmlExperiment*](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) tooaccomplish that.</span></span> <span data-ttu-id="ed620-129">Egy másik megoldás toopublish hello kísérlet gyűjteménye a fel nem sorolt módban, majd nyissa meg a hello munkaterület a hello más régióban.</span><span class="sxs-lookup"><span data-stu-id="ed620-129">Another workaround is toopublish hello experiment into Gallery in unlisted mode, then open it in hello workspace from hello other region.</span></span>
2. <span data-ttu-id="ed620-130">hello régió választó csak egy régió munkaterületek jelennek egyszerre.</span><span class="sxs-lookup"><span data-stu-id="ed620-130">hello region selector will only show workspaces for one region at a time.</span></span>  
3. <span data-ttu-id="ed620-131">Egy szabad munkaterület vagy Vendég Access (névtelen) munkaterületen létrejön és Dél központi USA tárolva</span><span class="sxs-lookup"><span data-stu-id="ed620-131">A free workspace or Guest Access (anonymous) workspace will be created and hosted in South Central U.S.</span></span>  
4. <span data-ttu-id="ed620-132">Egy olyan munkaterületről, Délkelet-Ázsiában található telepített webszolgáltatások is lehet üzemeltetni Délkelet-Ázsia.</span><span class="sxs-lookup"><span data-stu-id="ed620-132">Web services deployed from a workspace in Southeast Asia will also be hosted in Southeast Asia.</span></span>  

## <a name="more-information"></a><span data-ttu-id="ed620-133">További információ</span><span class="sxs-lookup"><span data-stu-id="ed620-133">More information</span></span>
<span data-ttu-id="ed620-134">Tegyen fel kérdést a hello [Azure Machine Learning fórum](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).</span><span class="sxs-lookup"><span data-stu-id="ed620-134">Ask a question on hello [Azure Machine Learning forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).</span></span>

<!--Image references-->
[1]: ./media/machine-learning-multi-geo/multi-geo_1.png
[2]: ./media/machine-learning-multi-geo/multi-geo_2.png
[3]: ./media/machine-learning-multi-geo/multi-geo_3.png
[4a]: ./media/machine-learning-multi-geo/multi-geo_4a.png
