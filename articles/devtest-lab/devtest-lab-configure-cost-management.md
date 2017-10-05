---
title: "Tekintse meg a labor becsült havi költségtrend Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "További tudnivalók az Azure DevTest Labs havi becsült költség trend diagram."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 1f46fdc5-d917-46e3-a1ea-f6dd41212ba4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
ms.openlocfilehash: b3ad1ead522908d4b41b7cca98d20ac91664998e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="view-the-monthly-estimated-lab-cost-trend-in-azure-devtest-labs"></a><span data-ttu-id="833ee-103">Tekintse meg a labor becsült havi költségtrend Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="833ee-103">View the monthly estimated lab cost trend in Azure DevTest Labs</span></span>
<span data-ttu-id="833ee-104">A DevTest Labs költség felügyeleti funkciója segítségével nyomon követheti a labor költségét.</span><span class="sxs-lookup"><span data-stu-id="833ee-104">The Cost Management feature of DevTest Labs helps you track the cost of your lab.</span></span> <span data-ttu-id="833ee-105">Ez a cikk bemutatja, hogyan használja a **becsült havi Költségtrend** diagram megjelenítése az aktuális naptári hónapra az aktuális naptári hónapra becsült költség-date és a várható befejezési hónap költsége.</span><span class="sxs-lookup"><span data-stu-id="833ee-105">This article illustrates how to use the **Monthly Estimated Cost Trend** chart to view the current calendar month's estimated cost-to-date and the projected end-of-month cost for the current calendar month.</span></span> <span data-ttu-id="833ee-106">Ebből a cikkből megismerheti, hogyan a havi becsült költség trend diagram megtekintéséhez az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="833ee-106">In this article, you learn how to view the monthly estimated cost trend chart in the Azure portal.</span></span>

## <a name="viewing-the-monthly-estimated-cost-trend-chart"></a><span data-ttu-id="833ee-107">A havi becsült Költségtrend diagram megtekintése</span><span class="sxs-lookup"><span data-stu-id="833ee-107">Viewing the Monthly Estimated Cost Trend chart</span></span>
<span data-ttu-id="833ee-108">A havi becsült Költségtrend diagram megtekintéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="833ee-108">To view the Monthly Estimated Cost Trend chart, follow these steps:</span></span> 

1. <span data-ttu-id="833ee-109">Jelentkezzen be az [Azure Portalra](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="833ee-109">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="833ee-110">Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** a listából.</span><span class="sxs-lookup"><span data-stu-id="833ee-110">Select **More Services**, and then select **DevTest Labs** from the list.</span></span>
3. <span data-ttu-id="833ee-111">Válassza ki a kívánt labor labs listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="833ee-111">From the list of labs, select the desired lab.</span></span>   
4. <span data-ttu-id="833ee-112">A labor paneljén válassza **beállítások költség**.</span><span class="sxs-lookup"><span data-stu-id="833ee-112">On the lab's blade, select **Cost settings**.</span></span>
5. <span data-ttu-id="833ee-113">A tesztlabor a **beállítások költség** panelen válassza **Lab költségtrend**.</span><span class="sxs-lookup"><span data-stu-id="833ee-113">On the lab's **Cost settings** blade, select **Lab cost trend**.</span></span>
6. <span data-ttu-id="833ee-114">Az alábbi képernyőfelvételen költség diagram példáját mutatja be.</span><span class="sxs-lookup"><span data-stu-id="833ee-114">The following screen shot shows an example of a cost chart.</span></span> 
   
    ![A diagram költség](./media/devtest-lab-configure-cost-management/graph.png)

<span data-ttu-id="833ee-116">A **becsült költség** érték az aktuális naptári hónapra becsült költség dátumig.</span><span class="sxs-lookup"><span data-stu-id="833ee-116">The **Estimated cost** value is the current calendar month's estimated cost-to-date.</span></span> <span data-ttu-id="833ee-117">A **becsült költség** számítja ki a labor költség előző az öt napig teljes aktuális naptári hónapra vonatkozó becsült költsége van.</span><span class="sxs-lookup"><span data-stu-id="833ee-117">The **Projected cost** is the estimated cost for the entire current calendar month, calculated using the lab cost for the previous five days.</span></span>

<span data-ttu-id="833ee-118">A költségek összegeket a következő egész számra kerekíti.</span><span class="sxs-lookup"><span data-stu-id="833ee-118">The cost amounts are rounded up to the next whole number.</span></span> <span data-ttu-id="833ee-119">Példa:</span><span class="sxs-lookup"><span data-stu-id="833ee-119">For example:</span></span> 

* <span data-ttu-id="833ee-120">5.01 kerekít legfeljebb 6</span><span class="sxs-lookup"><span data-stu-id="833ee-120">5.01 rounds up to 6</span></span> 
* <span data-ttu-id="833ee-121">5.50 kerekít legfeljebb 6</span><span class="sxs-lookup"><span data-stu-id="833ee-121">5.50 rounds up to 6</span></span>
* <span data-ttu-id="833ee-122">5.99 kerekít legfeljebb 6</span><span class="sxs-lookup"><span data-stu-id="833ee-122">5.99 rounds up to 6</span></span>

<span data-ttu-id="833ee-123">Azt jelzi, a diagram felett, a költségek a diagramon látható során a rendszer *becsült* használatával költségek [használatalapú fizetés](https://azure.microsoft.com/offers/ms-azr-0003p/) díjakat kínál.</span><span class="sxs-lookup"><span data-stu-id="833ee-123">As it states above the chart, the costs you see in the chart are *estimated* costs using [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) offer rates.</span></span>
<span data-ttu-id="833ee-124">Ezenkívül a következők *nem* költség kiszámításakor:</span><span class="sxs-lookup"><span data-stu-id="833ee-124">Additionally, the following are *not* included in the cost calculation:</span></span>

* <span data-ttu-id="833ee-125">CSP és Dreamspark-előfizetés használata jelenleg nem támogatott Azure DevTest Labs használja a [Azure számlázási API-k](../billing/billing-usage-rate-card-overview.md) a költség, amely nem támogatja a kriptográfiai Szolgáltató vagy a Dreamspark-előfizetések labor kiszámításához.</span><span class="sxs-lookup"><span data-stu-id="833ee-125">CSP and Dreamspark subscriptions are currently not supported as Azure DevTest Labs uses the [Azure billing APIs](../billing/billing-usage-rate-card-overview.md) to calculate the lab cost, which does not support CSP or Dreamspark subscriptions.</span></span>
* <span data-ttu-id="833ee-126">Az ajánlatok díjaival számolva.</span><span class="sxs-lookup"><span data-stu-id="833ee-126">Your offer rates.</span></span> <span data-ttu-id="833ee-127">A Microsoft jelenleg nem használhatják a (lásd az előfizetéshez tartozó), hogy Ön rendelkezik egyeztet Microsoft vagy a Microsoft partnerei ajánlatok díjaival számolva.</span><span class="sxs-lookup"><span data-stu-id="833ee-127">Currently, we are not able to use your offer rates (shown under your subscription) that you have negotiated with Microsoft or Microsoft partners.</span></span> <span data-ttu-id="833ee-128">Használatalapú fizetés díjszabás használjuk.</span><span class="sxs-lookup"><span data-stu-id="833ee-128">We are using Pay-As-You-Go rates.</span></span>
* <span data-ttu-id="833ee-129">A adók</span><span class="sxs-lookup"><span data-stu-id="833ee-129">Your taxes</span></span>
* <span data-ttu-id="833ee-130">A kedvezményeket</span><span class="sxs-lookup"><span data-stu-id="833ee-130">Your discounts</span></span>
* <span data-ttu-id="833ee-131">A Számlázás pénzneme.</span><span class="sxs-lookup"><span data-stu-id="833ee-131">Your billing currency.</span></span> <span data-ttu-id="833ee-132">Jelenleg a labor költség csak USD pénznemben jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="833ee-132">Currently, the lab cost is displayed only in USD currency.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="833ee-133">Kapcsolódó blogbejegyzések</span><span class="sxs-lookup"><span data-stu-id="833ee-133">Related blog posts</span></span>
* [<span data-ttu-id="833ee-134">Ennek érdekében a költségek a DevTest Labs szolgáltatásban két dolgot</span><span class="sxs-lookup"><span data-stu-id="833ee-134">Two more things to keep your cost on track in DevTest Labs</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/06/21/keep-your-cost-on-track/)
* [<span data-ttu-id="833ee-135">Miért költség küszöbértékek?</span><span class="sxs-lookup"><span data-stu-id="833ee-135">Why Cost Thresholds?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/11/why-cost-thresholds/)

## <a name="next-steps"></a><span data-ttu-id="833ee-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="833ee-136">Next steps</span></span>
<span data-ttu-id="833ee-137">Az alábbiakban ezután próbálja meg a következőkről:</span><span class="sxs-lookup"><span data-stu-id="833ee-137">Here are some things to try next:</span></span>

* <span data-ttu-id="833ee-138">[Labor házirendeket definiálhat az](devtest-lab-set-lab-policy.md) -megtudhatja, hogyan használja annak a szabályozására, hogyan használhatók a labor és a virtuális gépek különböző házirendek beállítása.</span><span class="sxs-lookup"><span data-stu-id="833ee-138">[Define lab policies](devtest-lab-set-lab-policy.md) - Learn how to set the various policies used to govern how your lab and its VMs are used.</span></span> 
* <span data-ttu-id="833ee-139">[Hozzon létre egyéni lemezkép](devtest-lab-create-template.md) – a virtuális gépek létrehozásakor megad egy talál, amely lehet, vagy egy egyéni lemezképet, vagy egy Piactéri lemezképhez.</span><span class="sxs-lookup"><span data-stu-id="833ee-139">[Create custom image](devtest-lab-create-template.md) - When you create a VM, you specify a base, which can be either a custom image or a Marketplace image.</span></span> <span data-ttu-id="833ee-140">Ez a cikk bemutatja, hogyan egyéni lemezkép létrehozása a VHD-fájl.</span><span class="sxs-lookup"><span data-stu-id="833ee-140">This article illustrates how to create a custom image from a VHD file.</span></span>
* <span data-ttu-id="833ee-141">[Konfigurálja a piactéren elérhető rendszerkép](devtest-lab-configure-marketplace-images.md) - DevTest Labs támogatja az Azure piactéren elérhető rendszerkép alapján virtuális gépek létrehozását.</span><span class="sxs-lookup"><span data-stu-id="833ee-141">[Configure Marketplace images](devtest-lab-configure-marketplace-images.md) - DevTest Labs supports creating VMs based on Azure Marketplace images.</span></span> <span data-ttu-id="833ee-142">Ez a cikk bemutatja, hogyan határozhatja meg, ha bármely, az Azure piactéren elérhető rendszerkép lehet egy, amikor a virtuális gépek létrehozásakor használt.</span><span class="sxs-lookup"><span data-stu-id="833ee-142">This article illustrates how to specify which, if any, Azure Marketplace images can be used when creating VMs in a lab.</span></span>
* <span data-ttu-id="833ee-143">[Hozzon létre egy virtuális Gépet egy tesztkörnyezetben](devtest-lab-add-vm-with-artifacts.md) -bemutatja, hogyan hozható létre a virtuális gépek alapjául szolgáló lemezképhez (vagy egyéni vagy Marketplace), és hogyan működnek együtt a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="833ee-143">[Create a VM in a lab](devtest-lab-add-vm-with-artifacts.md) - Illustrates how to create a VM from a base image (either custom or Marketplace), and how to work with artifacts in your VM.</span></span>

