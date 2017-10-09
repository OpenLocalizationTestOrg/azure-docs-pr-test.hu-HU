---
title: "aaaScale kvótái és korlátai, a laborban a Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Megtudhatja, hogyan tooscale egy tesztkörnyezetet a Azure DevTest Labs szolgáltatásban"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: ae624155-9181-45fa-bd2b-1983339b7e0e
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: tarcher
ms.openlocfilehash: 7fb429c0aabdfbce3a4a5aa6d9259daa2ee270d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scale-quotas-and-limits-in-devtest-labs"></a><span data-ttu-id="1e2af-103">Skála kvótái és korlátai a DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="1e2af-103">Scale quotas and limits in DevTest Labs</span></span>
<span data-ttu-id="1e2af-104">Munka a DevTest Labs szolgáltatásban, Észreveheti, hogy nincsenek bizonyos alapértelmezett korlátok toosome Azure-erőforrások, ami kihathat a hello DevTest Labs szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="1e2af-104">As you work in DevTest Labs, you might notice that there are certain default limits toosome Azure resources, which can affect hello DevTest Labs service.</span></span> <span data-ttu-id="1e2af-105">A hivatkozott tooas azok **kvóták**.</span><span class="sxs-lookup"><span data-stu-id="1e2af-105">These limits are referred tooas **quotas**.</span></span>

> [!NOTE]
> <span data-ttu-id="1e2af-106">DevTest Labs szolgáltatás hello nem ugyanazok a kvóták.</span><span class="sxs-lookup"><span data-stu-id="1e2af-106">hello DevTest Labs service doesn't impose any quotas.</span></span> <span data-ttu-id="1e2af-107">Bármely kvóták léphetnek fel hello a teljes Azure-előfizetés alapértelmezett korlátai.</span><span class="sxs-lookup"><span data-stu-id="1e2af-107">Any quotas you might encounter are default constraints of hello overall Azure subscription.</span></span>

<span data-ttu-id="1e2af-108">Minden Azure-erőforrás is használhat, amíg el nem éri a kvótát.</span><span class="sxs-lookup"><span data-stu-id="1e2af-108">You can use each Azure resource until you reach its quota.</span></span> <span data-ttu-id="1e2af-109">Minden előfizetés külön kvóták és a használati előfizetésenként követhető nyomon.</span><span class="sxs-lookup"><span data-stu-id="1e2af-109">Each subscription has separate quotas and usage is tracked per subscription.</span></span>

<span data-ttu-id="1e2af-110">Például az egyes előfizetések rendelkezik 20 magszámra vonatkozó alapértelmezett kvótát.</span><span class="sxs-lookup"><span data-stu-id="1e2af-110">For example, each subscription has a default quota of 20 cores.</span></span> <span data-ttu-id="1e2af-111">Igen ha virtuális gépeket a tesztlaborban négy magok hoz létre, majd csak létrehozhat öt virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="1e2af-111">So, if you are creating VMs in your lab with four cores each, then you can only create five VMs.</span></span> 

<span data-ttu-id="1e2af-112">[Az Azure előfizetés és szolgáltatásra vonatkozó korlátozások](https://docs.microsoft.com/azure/azure-subscription-service-limits) egy Azure-erőforrások leggyakoribb kvótái hello sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="1e2af-112">[Azure Subscription and Service Limits](https://docs.microsoft.com/azure/azure-subscription-service-limits) lists some of hello most common quotas for Azure resources.</span></span> <span data-ttu-id="1e2af-113">egy tesztkörnyezetben leggyakrabban használt erőforrások hello, és amely léphetnek fel a kvótákat, az VM magok, nyilvános IP-címek, hálózati illesztő, kezelt lemezek, RBAC szerepkör-hozzárendelés, és ExpressRoute-Kapcsolatcsoportok.</span><span class="sxs-lookup"><span data-stu-id="1e2af-113">hello resources most commonly used in a lab, and for which you might encounter quotas, include VM cores, public IP addresses, network interface, managed disks, RBAC role assignment, and ExpressRoute circuits.</span></span>

## <a name="view-your-usage-and-quotas"></a><span data-ttu-id="1e2af-114">A használati és a kvóták megtekintése</span><span class="sxs-lookup"><span data-stu-id="1e2af-114">View your usage and quotas</span></span>
<span data-ttu-id="1e2af-115">Ezek a lépések bemutatják, hogyan tooview hello az adott Azure-erőforrások és toosee előfizetésében aktuális kvóták minden kvóta hány százalékát használja.</span><span class="sxs-lookup"><span data-stu-id="1e2af-115">These steps show you how tooview hello current quotas in your subscription for specific Azure resources, and toosee what percentage of each quota you have used.</span></span>

1. <span data-ttu-id="1e2af-116">Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="1e2af-116">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
1. <span data-ttu-id="1e2af-117">Válassza ki **több szolgáltatások**, majd válassza ki **számlázási** hello listából.</span><span class="sxs-lookup"><span data-stu-id="1e2af-117">Select **More Services**, and then select **Billing** from hello list.</span></span>
1. <span data-ttu-id="1e2af-118">Hello számlázási panelen válassza ki egy előfizetést.</span><span class="sxs-lookup"><span data-stu-id="1e2af-118">In hello Billing blade, select a subscription.</span></span>
4. <span data-ttu-id="1e2af-119">Válassza ki **használati + kvóták**.</span><span class="sxs-lookup"><span data-stu-id="1e2af-119">Select **Usage + quotas**.</span></span>

   ![Használati és kvóták gomb](./media/devtest-lab-scale-lab/devtestlab-usage-and-quotas.png)

   <span data-ttu-id="1e2af-121">Használati + kvóták hello panel listája jelenik különböző erőforrások hello kvóta erőforrásonként használt előfizetés és hello százalékosan érhető el.</span><span class="sxs-lookup"><span data-stu-id="1e2af-121">hello Usage + quotas blade appears, listing different resources available in that subscription and hello percentage of hello quota that is being used per resource.</span></span>

   ![Kvóták és használat](./media/devtest-lab-scale-lab/devtestlab-view-quotas.png)

## <a name="requesting-more-resources-in-your-subscription"></a><span data-ttu-id="1e2af-123">A kért további erőforrást az előfizetésében</span><span class="sxs-lookup"><span data-stu-id="1e2af-123">Requesting more resources in your subscription</span></span>
<span data-ttu-id="1e2af-124">Ha eléri a kvóta kap, hello alapértelmezett korlát előfizetés az erőforrások növelhető mentése tooa maximális száma, a [Azure-előfizetés és a szolgáltatásra vonatkozó korlátozások](https://docs.microsoft.com/azure/azure-subscription-service-limits).</span><span class="sxs-lookup"><span data-stu-id="1e2af-124">If you reach a quota cap, hello default limit of a resource in a subscription can be increased up tooa maximum limit, as described in [Azure Subscription and Service Limits](https://docs.microsoft.com/azure/azure-subscription-service-limits).</span></span>

<span data-ttu-id="1e2af-125">Ezek a lépések bemutatják, hogyan toorequest a kvóta növeléséhez keresztül hello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="1e2af-125">These steps show you how toorequest a quota increase through hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="1e2af-126">Válassza ki **több szolgáltatások**, jelölje be **számlázási**, majd válassza ki **használati + kvóták**.</span><span class="sxs-lookup"><span data-stu-id="1e2af-126">Select **More Services**, select **Billing**, and then select **Usage + quotas**.</span></span>
1. <span data-ttu-id="1e2af-127">A használati hello és kvóták panelen, jelölje ki a hello **kérelem növelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="1e2af-127">In hello Usage + quotas blade, select hello **Request Increase** button.</span></span>

   ![Kérésgombhoz növelése](./media/devtest-lab-scale-lab/devtestlab-request-increase.png)

1. <span data-ttu-id="1e2af-129">toocomplete és hello kérés elküldéséhez szükséges hello adatok minden három lapon a hello **új támogatja a kérelem** űrlap.</span><span class="sxs-lookup"><span data-stu-id="1e2af-129">toocomplete and submit hello request, fill out hello required information on all three tabs of hello **New support request** form.</span></span>

   ![Növelje a kérésűrlapra](./media/devtest-lab-scale-lab/devtestlab-support-form.png)

<span data-ttu-id="1e2af-131">[Understanding Azure korlátozásai és növekszik](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/) lépjen kapcsolatba az Azure támogatási toorequest további információt nyújt a kvóta növelését.</span><span class="sxs-lookup"><span data-stu-id="1e2af-131">[Understanding Azure Limits and Increases](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/) provides more information about contacting Azure support toorequest a quota increase.</span></span>



[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

### <a name="next-steps"></a><span data-ttu-id="1e2af-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1e2af-132">Next steps</span></span>
* <span data-ttu-id="1e2af-133">Fedezze fel hello [Office DevTest Labs Azure Resource Manager gyorsindítási sablonok](https://github.com/Azure/azure-devtestlab/tree/master/Samples).</span><span class="sxs-lookup"><span data-stu-id="1e2af-133">Explore hello [DevTest Labs Azure Resource Manager QuickStart template gallery](https://github.com/Azure/azure-devtestlab/tree/master/Samples).</span></span>
