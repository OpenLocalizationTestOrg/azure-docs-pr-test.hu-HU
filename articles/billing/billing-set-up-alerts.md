---
title: "az Azure-előfizetések számlázással vagy jóváírás figyelmeztetéseket aaaSet |} Microsoft Docs"
description: "Ismerteti, hogyan állíthat be riasztásokat a az Azure számlázásának számlázási meglepetések számát elkerülése érdekében."
keywords: "jóváírás riasztást, számlázási riasztás"
services: 
documentationcenter: 
author: vikdesai
manager: tonguyen
editor: 
tags: billing
ms.assetid: 9b7b3eeb-cd9d-4690-86a3-51b1e2a8974f
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2017
ms.author: vikdesai
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 711b9c72c59874792b0e229cdc5ec0fa517c24c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-billing-or-credit-alerts-for-your-microsoft-azure-subscriptions"></a><span data-ttu-id="bd3c5-104">A Microsoft Azure-előfizetések a számlázással vagy jóváírás riasztások beállítása</span><span class="sxs-lookup"><span data-stu-id="bd3c5-104">Set up billing or credit alerts for your Microsoft Azure subscriptions</span></span>
<span data-ttu-id="bd3c5-105">Ha Ön hello Fiókadminisztrátor az Azure-előfizetéssel, használhatja a hello Azure számlázási riasztás szolgáltatás testre szabott toocreate számlázási riasztásokat, amelyek segítik a figyelheti és kezelheti a számlázási tevékenységet az Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="bd3c5-105">If you’re hello Account Admin for an Azure subscription, you can use hello Azure Billing Alert Service toocreate customized billing alerts that help you monitor and manage billing activity for your Azure accounts.</span></span>

<span data-ttu-id="bd3c5-106">Ez a szolgáltatás ennyi az egész Preview, ezért meg kell tooenable hello előzetes verziójú funkciók a lapon először.</span><span class="sxs-lookup"><span data-stu-id="bd3c5-106">This service is in preview, so you need tooenable it in hello Preview Features page first.</span></span>

## <a name="set-hello-alert-threshold-and-email-recipients"></a><span data-ttu-id="bd3c5-107">Hello riasztási küszöbérték és e-mailek címzettjeinek beállítása</span><span class="sxs-lookup"><span data-stu-id="bd3c5-107">Set hello alert threshold and email recipients</span></span>
1. <span data-ttu-id="bd3c5-108">Látogasson el [hello előzetes verziójú funkciók lap](https://account.windowsazure.com/PreviewFeatures) , és engedélyezze **értesítési szolgáltatás számlázási**.</span><span class="sxs-lookup"><span data-stu-id="bd3c5-108">Visit [hello Preview Features page](https://account.windowsazure.com/PreviewFeatures) and enable **Billing Alert Service**.</span></span>

1. <span data-ttu-id="bd3c5-109">Miután megkapta hello e-mailes megerősítés hello számlázási szolgáltatás be van kapcsolva, az előfizetés, látogasson el [hello előfizetések oldalán](https://account.windowsazure.com/Subscriptions) hello fiókportálon.</span><span class="sxs-lookup"><span data-stu-id="bd3c5-109">After you receive hello email confirmation that hello billing service is turned on for your subscription, visit [hello Subscriptions page](https://account.windowsazure.com/Subscriptions) in hello account portal.</span></span> <span data-ttu-id="bd3c5-110">Szeretné, hogy toomonitor, és kattintson az előfizetés hello **riasztások**.</span><span class="sxs-lookup"><span data-stu-id="bd3c5-110">Click hello subscription you want toomonitor, and then click **Alerts**.</span></span>

    ![Képernyőfelvétel a hello előfizetések nézet az Azure Account center, a riasztások kiemelt][Image1]

2. <span data-ttu-id="bd3c5-112">Ezután kattintson **riasztási hozzáadása** toocreate az elsőt.</span><span class="sxs-lookup"><span data-stu-id="bd3c5-112">Next, click **Add Alert** toocreate your first one.</span></span> <span data-ttu-id="bd3c5-113">Is beállítása öt számlázási riasztásokat, amelyek különböző küszöbértéke előfizetésenként összesen és az egyes riasztások tootwo e-mailek címzettjeinek fel.</span><span class="sxs-lookup"><span data-stu-id="bd3c5-113">You can set up a total of five billing alerts per subscription, with a different threshold and up tootwo email recipients for each alert.</span></span>

    ![Képernyőfelvétel a hello riasztások nézet, ahol felveheti a riasztás][Image2]

3. <span data-ttu-id="bd3c5-115">Amikor egy riasztást, akkor adjon neki egy egyedi nevet, válassza ki a költségkeret küszöbérték, és válassza hello e-mail címeket, ha elküldi a riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="bd3c5-115">When you add an alert, you give it a unique name, choose a spending threshold, and choose hello email addresses where alerts are sent.</span></span> <span data-ttu-id="bd3c5-116">Hello küszöb beállításához, vagy választhat egy **teljes számlázási** vagy egy **dolláros kreditet** a hello **riasztási a** listája.</span><span class="sxs-lookup"><span data-stu-id="bd3c5-116">When setting up hello threshold, you can choose either a **Billing Total** or a **Monetary Credit** from hello **Alert For** list.</span></span> <span data-ttu-id="bd3c5-117">Számlázási összesen riasztást küldi, amikor hello küszöbértéket meghaladó előfizetés kiadásokat.</span><span class="sxs-lookup"><span data-stu-id="bd3c5-117">For a billing total, an alert is sent when subscription spending exceeds hello threshold.</span></span> <span data-ttu-id="bd3c5-118">A pénzügyi kreditet riasztást küldi, amikor a pénzügyi kreditek eshessen hello korlátot.</span><span class="sxs-lookup"><span data-stu-id="bd3c5-118">For a monetary credit, an alert is sent when monetary credits drop below hello limit.</span></span> <span data-ttu-id="bd3c5-119">Pénzügyi kreditek általában tooFree próbaverzió és a Visual Studio előfizetések alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="bd3c5-119">Monetary credits usually apply tooFree Trial and Visual Studio subscriptions.</span></span>

    ![Képernyőfelvétel a hello riasztási hozzáadása nézet, ahol konfigurálhatja a címzetteknek][Image3]

<span data-ttu-id="bd3c5-121">Azure támogatja akármilyen e-mail címet, de nem győződjön meg arról, hogy működik-e hello e-mail címét, így gondosan ellenőrizze a elírás eredménye.</span><span class="sxs-lookup"><span data-stu-id="bd3c5-121">Azure supports any email address but doesn't verify that hello email address works, so double-check for typos.</span></span>

## <a name="check-on-your-alerts"></a><span data-ttu-id="bd3c5-122">A riasztások ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="bd3c5-122">Check on your alerts</span></span>
<span data-ttu-id="bd3c5-123">Miután beállította a riasztások, hello Account Center sorolja fel azokat, és bemutatja, hogy hány több állíthat be.</span><span class="sxs-lookup"><span data-stu-id="bd3c5-123">After you set up alerts, hello Account Center lists them and shows how many more you can set up.</span></span> <span data-ttu-id="bd3c5-124">Az egyes riasztások látható a hello dátum és a küldési időpontja, hogy egy riasztás teljes számlázási vagy pénzügyi kreditet-e, és a hello korlátot állít be.</span><span class="sxs-lookup"><span data-stu-id="bd3c5-124">For each alert, you see hello date and time it was sent, whether it’s an alert for Billing Total or Monetary Credit, and hello limit you set up.</span></span> <span data-ttu-id="bd3c5-125">hello dátum és idő formátuma 24 órás világidő koordinálják (UTC), és hello dátum, éééé-hh-nn formátumban.</span><span class="sxs-lookup"><span data-stu-id="bd3c5-125">hello date and time format is 24-hour Universal Time Coordinate (UTC) and hello date is yyyy-mm-dd format.</span></span> <span data-ttu-id="bd3c5-126">Kattintson a hello és írja alá a hello lista tooedit riasztás vagy hello Kuka toodelete azt.</span><span class="sxs-lookup"><span data-stu-id="bd3c5-126">Click hello plus sign for an alert in hello list tooedit it, or click hello trash-can toodelete it.</span></span>

## <a name="billing-alerts-for-enterprise-agreement-ea-customers"></a><span data-ttu-id="bd3c5-127">A nagyvállalati szerződés (EA) ügyfelek számlázási riasztások</span><span class="sxs-lookup"><span data-stu-id="bd3c5-127">Billing alerts for Enterprise Agreement (EA) customers</span></span>
<span data-ttu-id="bd3c5-128">Nagyvállalati ügyfelek a beléptetési kvóták költségeik beállítás a minden részleg számára is megkapja az értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="bd3c5-128">EA customers can get alerts for each department under an enrollment by setting spending quotas.</span></span> <span data-ttu-id="bd3c5-129">Lásd: [részleg költségeik kvóták](https://ea.azure.com/helpdocs/departmentSpendingQuotas) a hello EA portál tooget elindult.</span><span class="sxs-lookup"><span data-stu-id="bd3c5-129">See [Department Spending Quotas](https://ea.azure.com/helpdocs/departmentSpendingQuotas) in hello EA portal tooget started.</span></span>

## <a name="learn-more-about-azure-cost-management"></a><span data-ttu-id="bd3c5-130">További információ az Azure költség kezeléséről</span><span class="sxs-lookup"><span data-stu-id="bd3c5-130">Learn more about Azure cost management</span></span>
- <span data-ttu-id="bd3c5-131">Becsült költségeit hello segítségével [árképzési Számológép](https://azure.microsoft.com/pricing/calculator/), [összköltsége tulajdonjoga Számológép](https://aka.ms/azure-tco-calculator), és egy szolgáltatás hozzáadásakor.</span><span class="sxs-lookup"><span data-stu-id="bd3c5-131">Estimate costs using hello [pricing calculator](https://azure.microsoft.com/pricing/calculator/), [total cost of ownership calculator](https://aka.ms/azure-tco-calculator), and when you add a service.</span></span>
- <span data-ttu-id="bd3c5-132">[Tekintse át a használati és költségek az Azure portálon rendszeresen](billing-getting-started.md#costs).</span><span class="sxs-lookup"><span data-stu-id="bd3c5-132">[Review your usage and costs regularly in Azure portal](billing-getting-started.md#costs).</span></span>
- <span data-ttu-id="bd3c5-133">Kapcsolja be a [Azure Advisor költség javaslatok](../advisor/advisor-cost-recommendations.md).</span><span class="sxs-lookup"><span data-stu-id="bd3c5-133">Turn on [Azure Advisor cost recommendations](../advisor/advisor-cost-recommendations.md).</span></span>

<span data-ttu-id="bd3c5-134">több, lásd: toolearn [Azure költség felügyeleti útmutató](billing-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="bd3c5-134">toolearn more, see [Azure cost management guidance](billing-getting-started.md).</span></span>

[Image1]: ./media/azure-billing-set-up-alerts/billingalert1.png 
[Image2]: ./media/azure-billing-set-up-alerts/billingalert2.png
[Image3]: ./media/azure-billing-set-up-alerts/billingalerts3.png 
