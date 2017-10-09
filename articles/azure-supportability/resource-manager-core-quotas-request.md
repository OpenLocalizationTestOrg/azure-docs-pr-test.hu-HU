---
title: "aaaAzure erőforrás-kezelő core kvóta növelése kérelmek |} Microsoft Docs"
description: "Az Azure Resource Manager core kvóta növelése kérelmek"
author: ganganarayanan
ms.author: gangan
ms.date: 1/18/2017
ms.topic: article
ms.service: microsoft-docs
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: b158b9f0e0338eb239da9253c2146ea93c02e316
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="resource-manager-core-quota-increase-requests"></a><span data-ttu-id="46b16-103">Erőforrás-kezelő core kvóta növelése kérelmek</span><span class="sxs-lookup"><span data-stu-id="46b16-103">Resource Manager core quota increase requests</span></span>

<span data-ttu-id="46b16-104">Erőforrás-kezelő core kvótái hello régió szint és SKU termékcsalád szintjén lépnek érvénybe.</span><span class="sxs-lookup"><span data-stu-id="46b16-104">Resource Manager core quotas are enforced at hello region level and SKU family level.</span></span>
<span data-ttu-id="46b16-105">Tudjon meg többet a hogyan kvóták érvényes hello [Azure-előfizetés és a szolgáltatásra vonatkozó korlátozások](http://aka.ms/quotalimits) lap.</span><span class="sxs-lookup"><span data-stu-id="46b16-105">Learn more about how quotas are enforced on hello [Azure subscription and service limits](http://aka.ms/quotalimits) page.</span></span>
<span data-ttu-id="46b16-106">További információk az SKU-családok toolearn, előfordulhat, hogy összehasonlítására, hello teljesítményére és költség [Virtual Machines díjszabása](http://aka.ms/pricingcompute) lap.</span><span class="sxs-lookup"><span data-stu-id="46b16-106">toolearn more about SKU Families, you may compare cost and performance on hello [Virtual Machines Pricing](http://aka.ms/pricingcompute) page.</span></span>

<span data-ttu-id="46b16-107">növelését, egy toorequest létrehozása kvóta támogatási esetet magok hello Azure-portálon [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="46b16-107">toorequest an increase, create a Quota support case for Cores in hello Azure portal, [https://portal.azure.com](https://portal.azure.com).</span></span>

> [!NOTE]
> <span data-ttu-id="46b16-108">Ismerje meg, hogyan túl[hozzon létre egy támogatási kérést](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="46b16-108">Learn how too[create a support request](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) in hello Azure portal</span></span>

1. <span data-ttu-id="46b16-109">Hello új támogatási kérelem lapon jelölje be probléma típusú, mint "Kvóta" és "Magok" kvóta típus lehet.</span><span class="sxs-lookup"><span data-stu-id="46b16-109">On hello new support request page, select Issue type as "Quota" and Quota type as "Cores".</span></span>

    ![Kvóta alapvető beállítások panel](./media/resource-manager-core-quotas-request/Basics-blade.png)

2. <span data-ttu-id="46b16-111">Válassza ki az üzembe helyezési modellel, mint a "Erőforrás-kezelő", és jelöljön ki egy helyet.</span><span class="sxs-lookup"><span data-stu-id="46b16-111">Select Deployment model as "Resource Manager" and select a location.</span></span>

    ![Kvóta probléma panel](./media/resource-manager-core-quotas-request/Problem-step.png)

3. <span data-ttu-id="46b16-113">Válassza ki a hello SKU családok igénylő növelését.</span><span class="sxs-lookup"><span data-stu-id="46b16-113">Select hello SKU Families that require an increase.</span></span>

    ![Kiválasztott Termékváltozat-sorozat](./media/resource-manager-core-quotas-request/SKU-selected.png)

4. <span data-ttu-id="46b16-115">Adja meg a hello új korlátok szeretné hello az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="46b16-115">Enter hello new limits you would like on hello subscription.</span></span>

    ![Új kvótakérelemhez Termékváltozat](./media/resource-manager-core-quotas-request/SKU-new-quota.png)

- <span data-ttu-id="46b16-117">egy sor tooremove törölje a jelet hello SKU hello SKU termékcsalád kattintson a legördülő lista vagy hello elvetési "x" ikontól.</span><span class="sxs-lookup"><span data-stu-id="46b16-117">tooremove a line, uncheck hello SKU from hello SKU family dropdown or click hello discard "x" icon.</span></span>
<span data-ttu-id="46b16-118">Hello kívánt kvóta megadása minden SKU-család, után kattintson a "Tovább" gombra. a hello probléma lépés lap toocontinue hello támogatási kérés létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="46b16-118">After entering hello desired quota for each SKU family, click "Next" on hello Problem step page toocontinue with hello support request creation.</span></span>
