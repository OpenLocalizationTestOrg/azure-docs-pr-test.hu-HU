---
title: "aaaView hello havi becsült labor költségtrend a Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "További információk a hello Azure DevTest Labs havi becsült költség trend diagram."
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
ms.openlocfilehash: 47c7dd4cf91b76de74b502c50f05e79cd501ee35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="view-hello-monthly-estimated-lab-cost-trend-in-azure-devtest-labs"></a><span data-ttu-id="60e95-103">Nézet hello havi becsült labor költségtrend a Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="60e95-103">View hello monthly estimated lab cost trend in Azure DevTest Labs</span></span>
<span data-ttu-id="60e95-104">hello költség kezelése szolgáltatást a DevTest Labs segítségével nyomon követheti a labor hello költségét.</span><span class="sxs-lookup"><span data-stu-id="60e95-104">hello Cost Management feature of DevTest Labs helps you track hello cost of your lab.</span></span> <span data-ttu-id="60e95-105">Ez a cikk bemutatja, hogyan toouse hello **becsült havi Költségtrend** diagram tooview hello az aktuális naptári hónapra becsült költség-date és hello hello várható befejezési hónap költsége aktuális naptári hónapra.</span><span class="sxs-lookup"><span data-stu-id="60e95-105">This article illustrates how toouse hello **Monthly Estimated Cost Trend** chart tooview hello current calendar month's estimated cost-to-date and hello projected end-of-month cost for hello current calendar month.</span></span> <span data-ttu-id="60e95-106">Ebből a cikkből megismerheti, hogyan tooview hello havi becsült költség trend diagram hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="60e95-106">In this article, you learn how tooview hello monthly estimated cost trend chart in hello Azure portal.</span></span>

## <a name="viewing-hello-monthly-estimated-cost-trend-chart"></a><span data-ttu-id="60e95-107">Hello havi becsült Költségtrend diagram megtekintése</span><span class="sxs-lookup"><span data-stu-id="60e95-107">Viewing hello Monthly Estimated Cost Trend chart</span></span>
<span data-ttu-id="60e95-108">tooview hello havi becsült Költségtrend diagram, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="60e95-108">tooview hello Monthly Estimated Cost Trend chart, follow these steps:</span></span> 

1. <span data-ttu-id="60e95-109">Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="60e95-109">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="60e95-110">Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.</span><span class="sxs-lookup"><span data-stu-id="60e95-110">Select **More Services**, and then select **DevTest Labs** from hello list.</span></span>
3. <span data-ttu-id="60e95-111">Labs hello listában jelölje ki hello kívánt labor.</span><span class="sxs-lookup"><span data-stu-id="60e95-111">From hello list of labs, select hello desired lab.</span></span>   
4. <span data-ttu-id="60e95-112">Hello labor paneljén válassza **beállítások költség**.</span><span class="sxs-lookup"><span data-stu-id="60e95-112">On hello lab's blade, select **Cost settings**.</span></span>
5. <span data-ttu-id="60e95-113">A hello labor **beállítások költség** panelen válassza **Lab költségtrend**.</span><span class="sxs-lookup"><span data-stu-id="60e95-113">On hello lab's **Cost settings** blade, select **Lab cost trend**.</span></span>
6. <span data-ttu-id="60e95-114">hello következő képernyőfelvételen látható költség diagram példát.</span><span class="sxs-lookup"><span data-stu-id="60e95-114">hello following screen shot shows an example of a cost chart.</span></span> 
   
    ![A diagram költség](./media/devtest-lab-configure-cost-management/graph.png)

<span data-ttu-id="60e95-116">Hello **becsült költség** érték hello van az aktuális naptári hónapra becsült költség dátumig.</span><span class="sxs-lookup"><span data-stu-id="60e95-116">hello **Estimated cost** value is hello current calendar month's estimated cost-to-date.</span></span> <span data-ttu-id="60e95-117">Hello **becsült költség** hello teljes hello becsült költségét aktuális naptári hónapra számítja hello labor költség hello az előző öt nap.</span><span class="sxs-lookup"><span data-stu-id="60e95-117">hello **Projected cost** is hello estimated cost for hello entire current calendar month, calculated using hello lab cost for hello previous five days.</span></span>

<span data-ttu-id="60e95-118">hello költség összegeket toohello következő egész számra felfelé kerekíti.</span><span class="sxs-lookup"><span data-stu-id="60e95-118">hello cost amounts are rounded up toohello next whole number.</span></span> <span data-ttu-id="60e95-119">Példa:</span><span class="sxs-lookup"><span data-stu-id="60e95-119">For example:</span></span> 

* <span data-ttu-id="60e95-120">5.01 too6 kerekít.</span><span class="sxs-lookup"><span data-stu-id="60e95-120">5.01 rounds up too6</span></span> 
* <span data-ttu-id="60e95-121">5.50 too6 kerekít.</span><span class="sxs-lookup"><span data-stu-id="60e95-121">5.50 rounds up too6</span></span>
* <span data-ttu-id="60e95-122">5.99 too6 kerekít.</span><span class="sxs-lookup"><span data-stu-id="60e95-122">5.99 rounds up too6</span></span>

<span data-ttu-id="60e95-123">Azt jelzi, hello diagram felett, hello diagramon látható hello költségek során a rendszer *becsült* használatával költségek [használatalapú fizetés](https://azure.microsoft.com/offers/ms-azr-0003p/) díjakat kínál.</span><span class="sxs-lookup"><span data-stu-id="60e95-123">As it states above hello chart, hello costs you see in hello chart are *estimated* costs using [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) offer rates.</span></span>
<span data-ttu-id="60e95-124">Emellett hello az alábbiakban *nem* költségszámítás hello szerepel:</span><span class="sxs-lookup"><span data-stu-id="60e95-124">Additionally, hello following are *not* included in hello cost calculation:</span></span>

* <span data-ttu-id="60e95-125">CSP és Dreamspark-előfizetés használata jelenleg nem támogatott Azure DevTest Labs használ hello [Azure számlázási API-k](../billing/billing-usage-rate-card-overview.md) toocalculate hello labor költség, amely nem támogatja a kriptográfiai Szolgáltató vagy a Dreamspark-előfizetések.</span><span class="sxs-lookup"><span data-stu-id="60e95-125">CSP and Dreamspark subscriptions are currently not supported as Azure DevTest Labs uses hello [Azure billing APIs](../billing/billing-usage-rate-card-overview.md) toocalculate hello lab cost, which does not support CSP or Dreamspark subscriptions.</span></span>
* <span data-ttu-id="60e95-126">Az ajánlatok díjaival számolva.</span><span class="sxs-lookup"><span data-stu-id="60e95-126">Your offer rates.</span></span> <span data-ttu-id="60e95-127">A Microsoft jelenleg nem képes toouse a (lásd az előfizetéshez tartozó), hogy Ön rendelkezik egyeztet Microsoft vagy a Microsoft partnerei ajánlatok díjaival számolva.</span><span class="sxs-lookup"><span data-stu-id="60e95-127">Currently, we are not able toouse your offer rates (shown under your subscription) that you have negotiated with Microsoft or Microsoft partners.</span></span> <span data-ttu-id="60e95-128">Használatalapú fizetés díjszabás használjuk.</span><span class="sxs-lookup"><span data-stu-id="60e95-128">We are using Pay-As-You-Go rates.</span></span>
* <span data-ttu-id="60e95-129">A adók</span><span class="sxs-lookup"><span data-stu-id="60e95-129">Your taxes</span></span>
* <span data-ttu-id="60e95-130">A kedvezményeket</span><span class="sxs-lookup"><span data-stu-id="60e95-130">Your discounts</span></span>
* <span data-ttu-id="60e95-131">A Számlázás pénzneme.</span><span class="sxs-lookup"><span data-stu-id="60e95-131">Your billing currency.</span></span> <span data-ttu-id="60e95-132">Jelenleg hello labor költség csak USD pénznemben jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="60e95-132">Currently, hello lab cost is displayed only in USD currency.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="60e95-133">Kapcsolódó blogbejegyzések</span><span class="sxs-lookup"><span data-stu-id="60e95-133">Related blog posts</span></span>
* [<span data-ttu-id="60e95-134">Két további dolgot tookeep a költség, a nyomon követése a DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="60e95-134">Two more things tookeep your cost on track in DevTest Labs</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/06/21/keep-your-cost-on-track/)
* [<span data-ttu-id="60e95-135">Miért költség küszöbértékek?</span><span class="sxs-lookup"><span data-stu-id="60e95-135">Why Cost Thresholds?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/11/why-cost-thresholds/)

## <a name="next-steps"></a><span data-ttu-id="60e95-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="60e95-136">Next steps</span></span>
<span data-ttu-id="60e95-137">Az alábbiakban néhány dolgot tootry mellett:</span><span class="sxs-lookup"><span data-stu-id="60e95-137">Here are some things tootry next:</span></span>

* <span data-ttu-id="60e95-138">[Labor házirendeket definiálhat az](devtest-lab-set-lab-policy.md) -megtudhatja, hogyan tooset hello használt különféle szabályzatok hogyan használhatók a labor és a virtuális gépek toogovern.</span><span class="sxs-lookup"><span data-stu-id="60e95-138">[Define lab policies](devtest-lab-set-lab-policy.md) - Learn how tooset hello various policies used toogovern how your lab and its VMs are used.</span></span> 
* <span data-ttu-id="60e95-139">[Hozzon létre egyéni lemezkép](devtest-lab-create-template.md) – a virtuális gépek létrehozásakor megad egy talál, amely lehet, vagy egy egyéni lemezképet, vagy egy Piactéri lemezképhez.</span><span class="sxs-lookup"><span data-stu-id="60e95-139">[Create custom image](devtest-lab-create-template.md) - When you create a VM, you specify a base, which can be either a custom image or a Marketplace image.</span></span> <span data-ttu-id="60e95-140">Ez a cikk bemutatja, hogyan toocreate egyéni kép VHD-fájl.</span><span class="sxs-lookup"><span data-stu-id="60e95-140">This article illustrates how toocreate a custom image from a VHD file.</span></span>
* <span data-ttu-id="60e95-141">[Konfigurálja a piactéren elérhető rendszerkép](devtest-lab-configure-marketplace-images.md) - DevTest Labs támogatja az Azure piactéren elérhető rendszerkép alapján virtuális gépek létrehozását.</span><span class="sxs-lookup"><span data-stu-id="60e95-141">[Configure Marketplace images](devtest-lab-configure-marketplace-images.md) - DevTest Labs supports creating VMs based on Azure Marketplace images.</span></span> <span data-ttu-id="60e95-142">Ez a cikk bemutatja, hogyan toospecify, amely, ha bármely, az Azure piactéren elérhető rendszerkép lehet egy, amikor a virtuális gépek létrehozásakor használt.</span><span class="sxs-lookup"><span data-stu-id="60e95-142">This article illustrates how toospecify which, if any, Azure Marketplace images can be used when creating VMs in a lab.</span></span>
* <span data-ttu-id="60e95-143">[Hozzon létre egy virtuális Gépet egy tesztkörnyezetben](devtest-lab-add-vm-with-artifacts.md) -bemutatja, hogyan toocreate alapjául szolgáló lemezképhez a virtuális gép (vagy egyéni vagy piactér), és hogyan toowork együtt a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="60e95-143">[Create a VM in a lab](devtest-lab-add-vm-with-artifacts.md) - Illustrates how toocreate a VM from a base image (either custom or Marketplace), and how toowork with artifacts in your VM.</span></span>

