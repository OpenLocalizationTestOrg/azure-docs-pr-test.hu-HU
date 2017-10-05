---
title: "Kvótái és korlátai méretezni a laborban a Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Útmutató egy tesztlabor méretezése a Azure DevTest Labs szolgáltatásban"
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
ms.openlocfilehash: f11ed42b474e4f208eac92689bfa33fb188d15a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="scale-quotas-and-limits-in-devtest-labs"></a><span data-ttu-id="e6323-103">Skála kvótái és korlátai a DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="e6323-103">Scale quotas and limits in DevTest Labs</span></span>
<span data-ttu-id="e6323-104">Munka a DevTest Labs szolgáltatásban, Észreveheti, hogy nincsenek-e bizonyos bizonyos Azure-erőforrások, ami kihathat a DevTest Labs szolgáltatás alapértelmezett korlátozások.</span><span class="sxs-lookup"><span data-stu-id="e6323-104">As you work in DevTest Labs, you might notice that there are certain default limits to some Azure resources, which can affect the DevTest Labs service.</span></span> <span data-ttu-id="e6323-105">Ezek a korlátozások nevezzük **kvóták**.</span><span class="sxs-lookup"><span data-stu-id="e6323-105">These limits are referred to as **quotas**.</span></span>

> [!NOTE]
> <span data-ttu-id="e6323-106">A DevTest Labs szolgáltatás nem ugyanazok a kvóták.</span><span class="sxs-lookup"><span data-stu-id="e6323-106">The DevTest Labs service doesn't impose any quotas.</span></span> <span data-ttu-id="e6323-107">Bármely kvóták léphetnek olyan alapértelmezett tartoznak a teljes Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="e6323-107">Any quotas you might encounter are default constraints of the overall Azure subscription.</span></span>

<span data-ttu-id="e6323-108">Minden Azure-erőforrás is használhat, amíg el nem éri a kvótát.</span><span class="sxs-lookup"><span data-stu-id="e6323-108">You can use each Azure resource until you reach its quota.</span></span> <span data-ttu-id="e6323-109">Minden előfizetés külön kvóták és a használati előfizetésenként követhető nyomon.</span><span class="sxs-lookup"><span data-stu-id="e6323-109">Each subscription has separate quotas and usage is tracked per subscription.</span></span>

<span data-ttu-id="e6323-110">Például az egyes előfizetések rendelkezik 20 magszámra vonatkozó alapértelmezett kvótát.</span><span class="sxs-lookup"><span data-stu-id="e6323-110">For example, each subscription has a default quota of 20 cores.</span></span> <span data-ttu-id="e6323-111">Igen ha virtuális gépeket a tesztlaborban négy magok hoz létre, majd csak létrehozhat öt virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="e6323-111">So, if you are creating VMs in your lab with four cores each, then you can only create five VMs.</span></span> 

<span data-ttu-id="e6323-112">[Az Azure előfizetés és szolgáltatásra vonatkozó korlátozások](https://docs.microsoft.com/azure/azure-subscription-service-limits) egy Azure-erőforrások leggyakoribb kvótái sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="e6323-112">[Azure Subscription and Service Limits](https://docs.microsoft.com/azure/azure-subscription-service-limits) lists some of the most common quotas for Azure resources.</span></span> <span data-ttu-id="e6323-113">Az erőforrások leggyakrabban használt laborkörnyezetben, és amely léphetnek fel a kvótákat, az tartalmazza az VM mag, nyilvános IP-címek, hálózati adapter, felügyelt lemezek, RBAC szerepkör-hozzárendelés és ExpressRoute-Kapcsolatcsoportok.</span><span class="sxs-lookup"><span data-stu-id="e6323-113">The resources most commonly used in a lab, and for which you might encounter quotas, include VM cores, public IP addresses, network interface, managed disks, RBAC role assignment, and ExpressRoute circuits.</span></span>

## <a name="view-your-usage-and-quotas"></a><span data-ttu-id="e6323-114">A használati és a kvóták megtekintése</span><span class="sxs-lookup"><span data-stu-id="e6323-114">View your usage and quotas</span></span>
<span data-ttu-id="e6323-115">Ezeket a lépéseket mutatja be az aktuális kvóták tekintse meg az adott Azure-erőforrások előfizetés, és tekintse meg az egyes hány százalékát használja.</span><span class="sxs-lookup"><span data-stu-id="e6323-115">These steps show you how to view the current quotas in your subscription for specific Azure resources, and to see what percentage of each quota you have used.</span></span>

1. <span data-ttu-id="e6323-116">Jelentkezzen be az [Azure Portalra](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="e6323-116">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
1. <span data-ttu-id="e6323-117">Válassza ki **több szolgáltatások**, majd válassza ki **számlázási** a listából.</span><span class="sxs-lookup"><span data-stu-id="e6323-117">Select **More Services**, and then select **Billing** from the list.</span></span>
1. <span data-ttu-id="e6323-118">A számlázási panelen válassza ki egy előfizetést.</span><span class="sxs-lookup"><span data-stu-id="e6323-118">In the Billing blade, select a subscription.</span></span>
4. <span data-ttu-id="e6323-119">Válassza ki **használati + kvóták**.</span><span class="sxs-lookup"><span data-stu-id="e6323-119">Select **Usage + quotas**.</span></span>

   ![Használati és kvóták gomb](./media/devtest-lab-scale-lab/devtestlab-usage-and-quotas.png)

   <span data-ttu-id="e6323-121">A használati + kvóták panelen megjelenik, a listaelem különböző erőforrások érhető el, hogy az előfizetés és a kvóta erőforrásonként használt százalékos.</span><span class="sxs-lookup"><span data-stu-id="e6323-121">The Usage + quotas blade appears, listing different resources available in that subscription and the percentage of the quota that is being used per resource.</span></span>

   ![Kvóták és használat](./media/devtest-lab-scale-lab/devtestlab-view-quotas.png)

## <a name="requesting-more-resources-in-your-subscription"></a><span data-ttu-id="e6323-123">A kért további erőforrást az előfizetésében</span><span class="sxs-lookup"><span data-stu-id="e6323-123">Requesting more resources in your subscription</span></span>
<span data-ttu-id="e6323-124">Ha eléri a kvóta kap, az alapértelmezett határérték előfizetés az erőforrások növelhető legfeljebb, a [Azure-előfizetés és a szolgáltatásra vonatkozó korlátozások](https://docs.microsoft.com/azure/azure-subscription-service-limits).</span><span class="sxs-lookup"><span data-stu-id="e6323-124">If you reach a quota cap, the default limit of a resource in a subscription can be increased up to a maximum limit, as described in [Azure Subscription and Service Limits](https://docs.microsoft.com/azure/azure-subscription-service-limits).</span></span>

<span data-ttu-id="e6323-125">Ezeket a lépéseket mutatja be a kvóta növelését keresztül a [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="e6323-125">These steps show you how to request a quota increase through the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="e6323-126">Válassza ki **több szolgáltatások**, jelölje be **számlázási**, majd válassza ki **használati + kvóták**.</span><span class="sxs-lookup"><span data-stu-id="e6323-126">Select **More Services**, select **Billing**, and then select **Usage + quotas**.</span></span>
1. <span data-ttu-id="e6323-127">A használati + kvóták panelen válassza a **kérelem növelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e6323-127">In the Usage + quotas blade, select the **Request Increase** button.</span></span>

   ![Kérésgombhoz növelése](./media/devtest-lab-scale-lab/devtestlab-request-increase.png)

1. <span data-ttu-id="e6323-129">Végezze el, és küldje el a kérelmet, töltse ki a szükséges adatokat az összes három lap a **új támogatja a kérelem** űrlap.</span><span class="sxs-lookup"><span data-stu-id="e6323-129">To complete and submit the request, fill out the required information on all three tabs of the **New support request** form.</span></span>

   ![Növelje a kérésűrlapra](./media/devtest-lab-scale-lab/devtestlab-support-form.png)

<span data-ttu-id="e6323-131">[Understanding Azure korlátozásai és növekszik](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/) további tájékoztatást ad azokról a kvóta növelését az Azure támogatási szolgálatától.</span><span class="sxs-lookup"><span data-stu-id="e6323-131">[Understanding Azure Limits and Increases](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/) provides more information about contacting Azure support to request a quota increase.</span></span>



[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

### <a name="next-steps"></a><span data-ttu-id="e6323-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e6323-132">Next steps</span></span>
* <span data-ttu-id="e6323-133">Megismerkedhet a [Office DevTest Labs Azure Resource Manager gyorsindítási sablonok](https://github.com/Azure/azure-devtestlab/tree/master/Samples).</span><span class="sxs-lookup"><span data-stu-id="e6323-133">Explore the [DevTest Labs Azure Resource Manager QuickStart template gallery](https://github.com/Azure/azure-devtestlab/tree/master/Samples).</span></span>
