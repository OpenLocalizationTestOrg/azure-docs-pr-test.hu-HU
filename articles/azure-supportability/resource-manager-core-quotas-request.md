---
title: "Az Azure Resource Manager core kvóta növelése kérelmek |} Microsoft Docs"
description: "Az Azure Resource Manager core kvóta növelése kérelmek"
author: ganganarayanan
ms.author: gangan
ms.date: 1/18/2017
ms.topic: article
ms.service: microsoft-docs
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: cb6c5b3e86f126d4110d1cd29d8c9891e356e414
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="resource-manager-core-quota-increase-requests"></a><span data-ttu-id="48306-103">Erőforrás-kezelő core kvóta növelése kérelmek</span><span class="sxs-lookup"><span data-stu-id="48306-103">Resource Manager core quota increase requests</span></span>

<span data-ttu-id="48306-104">Erőforrás-kezelő core kvótái a régió szint és az SKU termékcsalád szintjén lépnek érvénybe.</span><span class="sxs-lookup"><span data-stu-id="48306-104">Resource Manager core quotas are enforced at the region level and SKU family level.</span></span>
<span data-ttu-id="48306-105">További tudnivalók a módját a kvóták kényszeríti ki a [Azure-előfizetés és a szolgáltatásra vonatkozó korlátozások](http://aka.ms/quotalimits) lap.</span><span class="sxs-lookup"><span data-stu-id="48306-105">Learn more about how quotas are enforced on the [Azure subscription and service limits](http://aka.ms/quotalimits) page.</span></span>
<span data-ttu-id="48306-106">További információt a Termékváltozat-családok, előfordulhat, hogy összehasonlítja költségeket és a teljesítmény a a [Virtual Machines díjszabása](http://aka.ms/pricingcompute) lap.</span><span class="sxs-lookup"><span data-stu-id="48306-106">To learn more about SKU Families, you may compare cost and performance on the [Virtual Machines Pricing](http://aka.ms/pricingcompute) page.</span></span>

<span data-ttu-id="48306-107">A korlátozás megnövelésére a magok kvóta támogatási esetet az Azure-portálon hozzon létre [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="48306-107">To request an increase, create a Quota support case for Cores in the Azure portal, [https://portal.azure.com](https://portal.azure.com).</span></span>

> [!NOTE]
> <span data-ttu-id="48306-108">Megtudhatja, hogyan [hozzon létre egy támogatási kérést](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="48306-108">Learn how to [create a support request](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) in the Azure portal</span></span>

1. <span data-ttu-id="48306-109">Új támogatási kérelem lapján válassza ki a probléma típusú, mint "Kvóta" és "Magok" kvóta típus lehet.</span><span class="sxs-lookup"><span data-stu-id="48306-109">On the new support request page, select Issue type as "Quota" and Quota type as "Cores".</span></span>

    ![Kvóta alapvető beállítások panel](./media/resource-manager-core-quotas-request/Basics-blade.png)

2. <span data-ttu-id="48306-111">Válassza ki az üzembe helyezési modellel, mint a "Erőforrás-kezelő", és jelöljön ki egy helyet.</span><span class="sxs-lookup"><span data-stu-id="48306-111">Select Deployment model as "Resource Manager" and select a location.</span></span>

    ![Kvóta probléma panel](./media/resource-manager-core-quotas-request/Problem-step.png)

3. <span data-ttu-id="48306-113">Válassza ki a Termékváltozat családok igénylő növelését.</span><span class="sxs-lookup"><span data-stu-id="48306-113">Select the SKU Families that require an increase.</span></span>

    ![Kiválasztott Termékváltozat-sorozat](./media/resource-manager-core-quotas-request/SKU-selected.png)

4. <span data-ttu-id="48306-115">Írja be az új korlátai által megszabott szeretné az előfizetést.</span><span class="sxs-lookup"><span data-stu-id="48306-115">Enter the new limits you would like on the subscription.</span></span>

    ![Új kvótakérelemhez Termékváltozat](./media/resource-manager-core-quotas-request/SKU-new-quota.png)

- <span data-ttu-id="48306-117">Távolíthatja el, törölje a jelet a Termékváltozat az SKU-család legördülő menüből, vagy kattintson az "x" elvetési ikonra.</span><span class="sxs-lookup"><span data-stu-id="48306-117">To remove a line, uncheck the SKU from the SKU family dropdown or click the discard "x" icon.</span></span>
<span data-ttu-id="48306-118">A kívánt kvóta megadása minden SKU-család, után kattintson a "Tovább" gombra, a támogatási kérelem létrehozása folytatásához probléma lépés oldalon.</span><span class="sxs-lookup"><span data-stu-id="48306-118">After entering the desired quota for each SKU family, click "Next" on the Problem step page to continue with the support request creation.</span></span>
