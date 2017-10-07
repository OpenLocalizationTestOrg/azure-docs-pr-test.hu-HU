---
title: "egy webszolgáltatás-bővítmény aaaHow toodeploy toomultiple régiók |} Microsoft Docs"
description: "Lépéseket toodeploy (Másolás) egy új webszolgáltatás tooother régiók."
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
ms.openlocfilehash: 21fcdb96f118c60ed98b60b1b2df833766c7c8bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-a-web-service-toomultiple-regions"></a><span data-ttu-id="116f8-103">Hogyan toodeploy egy webszolgáltatás-bővítmény toomultiple régiók</span><span class="sxs-lookup"><span data-stu-id="116f8-103">How toodeploy a Web Service toomultiple regions</span></span>
<span data-ttu-id="116f8-104">Új Azure Web Services hello lehetővé teszik tooeasily telepítése egy webes szolgáltatás toomultiple régiók több előfizetések vagy munkaterületek anélkül.</span><span class="sxs-lookup"><span data-stu-id="116f8-104">hello New Azure Web Services allow you tooeasily deploy a web service toomultiple regions without needing multiple subscriptions or workspaces.</span></span> 

<span data-ttu-id="116f8-105">Tarifacsomag az régió adott, ezért meg kell adnia mindegyik régióhoz fog üzembe helyezésének hello webszolgáltatás egy számlázási csomagot.</span><span class="sxs-lookup"><span data-stu-id="116f8-105">Pricing is region specific, therefore you must define a billing plan for each region in which you will deploy hello web service.</span></span>

## <a name="toocreate-a-plan-in-another-region"></a><span data-ttu-id="116f8-106">a csomag egy másik régióban toocreate</span><span class="sxs-lookup"><span data-stu-id="116f8-106">toocreate a plan in another region</span></span>
1. <span data-ttu-id="116f8-107">Jelentkezzen be a [Microsoft Azure Machine Learning webszolgáltatások](https://services.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="116f8-107">Sign into [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/).</span></span>
2. <span data-ttu-id="116f8-108">Kattintson a hello **tervek** menüjét.</span><span class="sxs-lookup"><span data-stu-id="116f8-108">Click hello **Plans** menu option.</span></span>
3. <span data-ttu-id="116f8-109">Hello csomagok keresztül nézet lap, kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="116f8-109">On hello Plans over view page, click **New**.</span></span>
4. <span data-ttu-id="116f8-110">A hello **előfizetés** legördülő menüben válassza hello előfizetés mely hello új terv legyen elhelyezve.</span><span class="sxs-lookup"><span data-stu-id="116f8-110">From hello **Subscription** dropdown, select hello subscription in which hello new plan will reside.</span></span>
5. <span data-ttu-id="116f8-111">A hello **régió** legördülő menüben válasszon ki egy régiót hello új csomag.</span><span class="sxs-lookup"><span data-stu-id="116f8-111">From hello **Region** dropdown, select a region for hello new plan.</span></span> <span data-ttu-id="116f8-112">hello beállítások megtervezése hello kijelölt terület jelennek majd meg hello **beállítások megtervezése** hello lap szakasza.</span><span class="sxs-lookup"><span data-stu-id="116f8-112">hello Plan Options for hello selected region will display in hello **Plan Options** section of hello page.</span></span>
6. <span data-ttu-id="116f8-113">A hello **erőforráscsoport** legördülő menüben válassza a erőforráscsoport hello terv.</span><span class="sxs-lookup"><span data-stu-id="116f8-113">From hello **Resource Group** dropdown, select a resource group for hello plan.</span></span> <span data-ttu-id="116f8-114">További információ az erőforráscsoportokkal látható [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="116f8-114">From more information on resource groups, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>
7. <span data-ttu-id="116f8-115">A **neve** hello terv hello típusnév.</span><span class="sxs-lookup"><span data-stu-id="116f8-115">In **Plan Name** type hello name of hello plan.</span></span>
8. <span data-ttu-id="116f8-116">A **terv beállítások**, hello számlázási szint hello új csomag gombra.</span><span class="sxs-lookup"><span data-stu-id="116f8-116">Under **Plan Options**, click hello billing level for hello new plan.</span></span>
9. <span data-ttu-id="116f8-117">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="116f8-117">Click **Create**.</span></span>

## <a name="deploying-hello-web-service-tooanother-region"></a><span data-ttu-id="116f8-118">Hello web service tooanother régióban üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="116f8-118">Deploying hello web service tooanother region</span></span>
1. <span data-ttu-id="116f8-119">Kattintson a hello **webszolgáltatások** menüjét.</span><span class="sxs-lookup"><span data-stu-id="116f8-119">Click hello **Web Services** menu option.</span></span>
2. <span data-ttu-id="116f8-120">Válassza ki a hello webszolgáltatás tooa új régió telepít.</span><span class="sxs-lookup"><span data-stu-id="116f8-120">Select hello Web Service you are deploying tooa new region.</span></span>
3. <span data-ttu-id="116f8-121">Kattintson a **másolási**.</span><span class="sxs-lookup"><span data-stu-id="116f8-121">Click **Copy**.</span></span>
4. <span data-ttu-id="116f8-122">A **webszolgáltatás neve**, adjon meg egy új nevet hello webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="116f8-122">In **Web Service Name**, type a new name for hello web service.</span></span>
5. <span data-ttu-id="116f8-123">A **szolgáltatásleírás webes**, hello webszolgáltatás leírását.</span><span class="sxs-lookup"><span data-stu-id="116f8-123">In **Web service description**, type a description for hello web service.</span></span>
6. <span data-ttu-id="116f8-124">A hello **előfizetés** legördülő menüben válassza hello előfizetés mely hello új webszolgáltatás legyen elhelyezve.</span><span class="sxs-lookup"><span data-stu-id="116f8-124">From hello **Subscription** dropdown, select hello subscription in which hello new web service will reside.</span></span>
7. <span data-ttu-id="116f8-125">A hello **erőforráscsoport** legördülő menüben válassza a erőforráscsoport hello webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="116f8-125">From hello **Resource Group** dropdown, select a resource group for hello web service.</span></span> <span data-ttu-id="116f8-126">További információ az erőforráscsoportokkal látható [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="116f8-126">From more information on resource groups, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>
8. <span data-ttu-id="116f8-127">A hello **régió** legördülő menüben válassza hello régióban mely toodeploy hello webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="116f8-127">From hello **Region** dropdown, select hello region in which toodeploy hello web service.</span></span>
9. <span data-ttu-id="116f8-128">A hello **tárfiók** legördülő menüben válassza a tárfiókokat mely toostore hello webes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="116f8-128">From hello **Storage account** dropdown, select a storage account in which toostore hello web service.</span></span>
10. <span data-ttu-id="116f8-129">A hello **ár terv** legördülő menüben válasszon ki egy tervet a 8. lépésben kiválasztott hello régióban.</span><span class="sxs-lookup"><span data-stu-id="116f8-129">From hello **Price Plan** dropdown, select a plan in hello region you selected in step 8.</span></span>
11. <span data-ttu-id="116f8-130">Kattintson a **másolási**.</span><span class="sxs-lookup"><span data-stu-id="116f8-130">Click **Copy**.</span></span>

