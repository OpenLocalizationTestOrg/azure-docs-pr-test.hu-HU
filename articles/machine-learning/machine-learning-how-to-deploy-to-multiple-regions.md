---
title: "A webszolgáltatás üzembe helyezése több régióba |} Microsoft Docs"
description: "(Másolás) egy új webszolgáltatás-bővítmény más régiókban telepítésének lépéseit."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: cgronlun
ms.assetid: 36c60411-f2db-4ee2-9b66-b1f1d77a8f44
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 3895537bbca72e687838ff5013c291dfee3be707
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-deploy-a-web-service-to-multiple-regions"></a><span data-ttu-id="c9b16-103">Webszolgáltatás üzembe helyezése több régióban</span><span class="sxs-lookup"><span data-stu-id="c9b16-103">How to deploy a Web Service to multiple regions</span></span>
<span data-ttu-id="c9b16-104">Az új Azure Web Services engedélyezi, hogy könnyen telepíthető egy webszolgáltatás-bővítmény több régióba több előfizetések vagy munkaterületek anélkül.</span><span class="sxs-lookup"><span data-stu-id="c9b16-104">The New Azure Web Services allow you to easily deploy a web service to multiple regions without needing multiple subscriptions or workspaces.</span></span> 

<span data-ttu-id="c9b16-105">Tarifacsomag az régió adott, ezért meg kell adnia egy számlázási csomagot minden egyes régió, amelyben a webszolgáltatást telepíti.</span><span class="sxs-lookup"><span data-stu-id="c9b16-105">Pricing is region specific, therefore you must define a billing plan for each region in which you will deploy the web service.</span></span>

## <a name="to-create-a-plan-in-another-region"></a><span data-ttu-id="c9b16-106">Egy másik régióban terv létrehozásához</span><span class="sxs-lookup"><span data-stu-id="c9b16-106">To create a plan in another region</span></span>
1. <span data-ttu-id="c9b16-107">Jelentkezzen be a [Microsoft Azure Machine Learning webszolgáltatások](https://services.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="c9b16-107">Sign into [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/).</span></span>
2. <span data-ttu-id="c9b16-108">Kattintson a **tervek** menüjét.</span><span class="sxs-lookup"><span data-stu-id="c9b16-108">Click the **Plans** menu option.</span></span>
3. <span data-ttu-id="c9b16-109">A csomagok keresztül nézet lap, kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="c9b16-109">On the Plans over view page, click **New**.</span></span>
4. <span data-ttu-id="c9b16-110">Az a **előfizetés** legördülő menüben válassza ki az előfizetést, amelyben létre kívánja hozni az új tervet.</span><span class="sxs-lookup"><span data-stu-id="c9b16-110">From the **Subscription** dropdown, select the subscription in which the new plan will reside.</span></span>
5. <span data-ttu-id="c9b16-111">Az a **régió** legördülő menüben válasszon ki egy régiót az új csomag.</span><span class="sxs-lookup"><span data-stu-id="c9b16-111">From the **Region** dropdown, select a region for the new plan.</span></span> <span data-ttu-id="c9b16-112">A terv beállításait a kiválasztott régióban jelenítse meg a **beállítások megtervezése** lap részében.</span><span class="sxs-lookup"><span data-stu-id="c9b16-112">The Plan Options for the selected region will display in the **Plan Options** section of the page.</span></span>
6. <span data-ttu-id="c9b16-113">Az a **erőforráscsoport** legördülő menüben válassza egy erőforráscsoport, a terv.</span><span class="sxs-lookup"><span data-stu-id="c9b16-113">From the **Resource Group** dropdown, select a resource group for the plan.</span></span> <span data-ttu-id="c9b16-114">További információ az erőforráscsoportokkal látható [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c9b16-114">From more information on resource groups, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>
7. <span data-ttu-id="c9b16-115">A **neve** írja be a csomag nevét.</span><span class="sxs-lookup"><span data-stu-id="c9b16-115">In **Plan Name** type the name of the plan.</span></span>
8. <span data-ttu-id="c9b16-116">A **terv beállítások**, kattintson az új csomag számlázási szintjét.</span><span class="sxs-lookup"><span data-stu-id="c9b16-116">Under **Plan Options**, click the billing level for the new plan.</span></span>
9. <span data-ttu-id="c9b16-117">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c9b16-117">Click **Create**.</span></span>

## <a name="deploying-the-web-service-to-another-region"></a><span data-ttu-id="c9b16-118">A webszolgáltatás egy másik régióban üzembe helyezni</span><span class="sxs-lookup"><span data-stu-id="c9b16-118">Deploying the web service to another region</span></span>
1. <span data-ttu-id="c9b16-119">Kattintson a **webszolgáltatások** menüjét.</span><span class="sxs-lookup"><span data-stu-id="c9b16-119">Click the **Web Services** menu option.</span></span>
2. <span data-ttu-id="c9b16-120">Válassza ki a webszolgáltatás új régióban üzembe helyezni.</span><span class="sxs-lookup"><span data-stu-id="c9b16-120">Select the Web Service you are deploying to a new region.</span></span>
3. <span data-ttu-id="c9b16-121">Kattintson a **másolási**.</span><span class="sxs-lookup"><span data-stu-id="c9b16-121">Click **Copy**.</span></span>
4. <span data-ttu-id="c9b16-122">A **webszolgáltatás neve**, adjon meg egy új nevet a webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="c9b16-122">In **Web Service Name**, type a new name for the web service.</span></span>
5. <span data-ttu-id="c9b16-123">A **szolgáltatásleírás webes**, írja be a webszolgáltatás leírását.</span><span class="sxs-lookup"><span data-stu-id="c9b16-123">In **Web service description**, type a description for the web service.</span></span>
6. <span data-ttu-id="c9b16-124">Az a **előfizetés** legördülő menüben válassza ki az előfizetést, amely az új webszolgáltatás legyen elhelyezve.</span><span class="sxs-lookup"><span data-stu-id="c9b16-124">From the **Subscription** dropdown, select the subscription in which the new web service will reside.</span></span>
7. <span data-ttu-id="c9b16-125">Az a **erőforráscsoport** legördülő menüben válassza a erőforráscsoport a webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="c9b16-125">From the **Resource Group** dropdown, select a resource group for the web service.</span></span> <span data-ttu-id="c9b16-126">További információ az erőforráscsoportokkal látható [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c9b16-126">From more information on resource groups, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>
8. <span data-ttu-id="c9b16-127">Az a **régió** legördülő menüben válassza ki azt a régiót, amelyben a webszolgáltatás telepítése.</span><span class="sxs-lookup"><span data-stu-id="c9b16-127">From the **Region** dropdown, select the region in which to deploy the web service.</span></span>
9. <span data-ttu-id="c9b16-128">Az a **tárfiók** legördülő menüben válassza ki a megfelelő tárolási fiók, amely a webszolgáltatás tárolja.</span><span class="sxs-lookup"><span data-stu-id="c9b16-128">From the **Storage account** dropdown, select a storage account in which to store the web service.</span></span>
10. <span data-ttu-id="c9b16-129">Az a **ár terv** legördülő menüben válasszon ki egy tervet a 8. lépésben kiválasztott régióban.</span><span class="sxs-lookup"><span data-stu-id="c9b16-129">From the **Price Plan** dropdown, select a plan in the region you selected in step 8.</span></span>
11. <span data-ttu-id="c9b16-130">Kattintson a **másolási**.</span><span class="sxs-lookup"><span data-stu-id="c9b16-130">Click **Copy**.</span></span>

