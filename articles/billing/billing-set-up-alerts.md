---
title: "Az Azure-előfizetések számlázással vagy jóváírás riasztások beállítása |} Microsoft Docs"
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
ms.openlocfilehash: 7a1b579fdde831fdc3afa0a2aee4c24890216ed1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-billing-or-credit-alerts-for-your-microsoft-azure-subscriptions"></a><span data-ttu-id="96112-104">A Microsoft Azure-előfizetések a számlázással vagy jóváírás riasztások beállítása</span><span class="sxs-lookup"><span data-stu-id="96112-104">Set up billing or credit alerts for your Microsoft Azure subscriptions</span></span>
<span data-ttu-id="96112-105">Ha a Fiókadminisztrátor az Azure-előfizetésre, az Azure számlázási értesítési szolgáltatás testreszabott létrehozásához használhatja számlázási riasztásokat, amelyek segítenek figyelése és kezelése az Azure-fiókra számlázási tevékenységet.</span><span class="sxs-lookup"><span data-stu-id="96112-105">If you’re the Account Admin for an Azure subscription, you can use the Azure Billing Alert Service to create customized billing alerts that help you monitor and manage billing activity for your Azure accounts.</span></span>

<span data-ttu-id="96112-106">Ez a szolgáltatás a helyzet a képen az előzetes verziójú Funkciók lapon engedélyezni kell.</span><span class="sxs-lookup"><span data-stu-id="96112-106">This service is in preview, so you need to enable it in the Preview Features page first.</span></span>

## <a name="set-the-alert-threshold-and-email-recipients"></a><span data-ttu-id="96112-107">A riasztási küszöbérték és e-mailek címzettjeinek beállítása</span><span class="sxs-lookup"><span data-stu-id="96112-107">Set the alert threshold and email recipients</span></span>
1. <span data-ttu-id="96112-108">Látogasson el [az előzetes verziójú funkciók oldalon](https://account.windowsazure.com/PreviewFeatures) , és engedélyezze **értesítési szolgáltatás számlázási**.</span><span class="sxs-lookup"><span data-stu-id="96112-108">Visit [the Preview Features page](https://account.windowsazure.com/PreviewFeatures) and enable **Billing Alert Service**.</span></span>

1. <span data-ttu-id="96112-109">Miután megkapta az e-mailes megerősítés, amely a számlázási szolgáltatás be van kapcsolva az előfizetéséhez, látogasson el a [az előfizetések oldalán](https://account.windowsazure.com/Subscriptions) a fiókportálon.</span><span class="sxs-lookup"><span data-stu-id="96112-109">After you receive the email confirmation that the billing service is turned on for your subscription, visit [the Subscriptions page](https://account.windowsazure.com/Subscriptions) in the account portal.</span></span> <span data-ttu-id="96112-110">Kattintson a figyelheti, és kattintson a kívánt előfizetést **riasztások**.</span><span class="sxs-lookup"><span data-stu-id="96112-110">Click the subscription you want to monitor, and then click **Alerts**.</span></span>

    ![Az előfizetések nézet az Azure Account center riasztások kiemelt – képernyőfelvétel][Image1]

2. <span data-ttu-id="96112-112">Ezután kattintson **riasztási hozzáadása** az elsőt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="96112-112">Next, click **Add Alert** to create your first one.</span></span> <span data-ttu-id="96112-113">Összesen előfizetésenként, egy másik küszöbérték és az egyes riasztások két e-mailek címzettjeinek legfeljebb öt számlázási riasztásokat állíthat be.</span><span class="sxs-lookup"><span data-stu-id="96112-113">You can set up a total of five billing alerts per subscription, with a different threshold and up to two email recipients for each alert.</span></span>

    ![A riasztások nézetben, ahol felveheti a riasztás képernyőképe][Image2]

3. <span data-ttu-id="96112-115">Amikor egy riasztást, adjon neki egy egyedi nevet, választja a költségkeret küszöbértéket, és válassza ki az e-mail címeket, ha elküldi a riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="96112-115">When you add an alert, you give it a unique name, choose a spending threshold, and choose the email addresses where alerts are sent.</span></span> <span data-ttu-id="96112-116">A küszöbérték beállításakor, vagy választhat egy **teljes számlázási** vagy egy **pénzügyi kreditet** a a **riasztási a** listája.</span><span class="sxs-lookup"><span data-stu-id="96112-116">When setting up the threshold, you can choose either a **Billing Total** or a **Monetary Credit** from the **Alert For** list.</span></span> <span data-ttu-id="96112-117">Számlázási összesen riasztást küldi, amikor az előfizetés költségeik meghaladja a küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="96112-117">For a billing total, an alert is sent when subscription spending exceeds the threshold.</span></span> <span data-ttu-id="96112-118">A pénzügyi kreditet riasztást küldi, amikor a pénzügyi kreditek meghaladja az vetett.</span><span class="sxs-lookup"><span data-stu-id="96112-118">For a monetary credit, an alert is sent when monetary credits drop below the limit.</span></span> <span data-ttu-id="96112-119">Pénzügyi kreditek általában az ingyenes próbaverzió és a Visual Studio előfizetések vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="96112-119">Monetary credits usually apply to Free Trial and Visual Studio subscriptions.</span></span>

    ![A riasztási hozzáadása nézet, ahol konfigurálhatja a címzettek képernyőképe][Image3]

<span data-ttu-id="96112-121">Azure akármilyen e-mail címet támogatja, de nem győződjön meg arról, hogy az e-mail cím működésével úgy exportálást elírás eredménye.</span><span class="sxs-lookup"><span data-stu-id="96112-121">Azure supports any email address but doesn't verify that the email address works, so double-check for typos.</span></span>

## <a name="check-on-your-alerts"></a><span data-ttu-id="96112-122">A riasztások ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="96112-122">Check on your alerts</span></span>
<span data-ttu-id="96112-123">Miután beállította a riasztásokat, a Account Center sorolja fel azokat, és bemutatja, hogy hány több állíthat be.</span><span class="sxs-lookup"><span data-stu-id="96112-123">After you set up alerts, the Account Center lists them and shows how many more you can set up.</span></span> <span data-ttu-id="96112-124">Az egyes riasztások látható, a dátumot és küldési időpontja, hogy egy riasztás teljes számlázási vagy pénzügyi kreditet-e, és a beállított korlátot.</span><span class="sxs-lookup"><span data-stu-id="96112-124">For each alert, you see the date and time it was sent, whether it’s an alert for Billing Total or Monetary Credit, and the limit you set up.</span></span> <span data-ttu-id="96112-125">A dátum és idő formátuma 24 órás világidő koordinálják (UTC), és a dátum, éééé-hh-nn formátumban.</span><span class="sxs-lookup"><span data-stu-id="96112-125">The date and time format is 24-hour Universal Time Coordinate (UTC) and the date is yyyy-mm-dd format.</span></span> <span data-ttu-id="96112-126">Kattintson a plusz jelre riasztás szerkesztheti a listában, vagy kattintson a Kuka törli-e.</span><span class="sxs-lookup"><span data-stu-id="96112-126">Click the plus sign for an alert in the list to edit it, or click the trash-can to delete it.</span></span>

## <a name="billing-alerts-for-enterprise-agreement-ea-customers"></a><span data-ttu-id="96112-127">A nagyvállalati szerződés (EA) ügyfelek számlázási riasztások</span><span class="sxs-lookup"><span data-stu-id="96112-127">Billing alerts for Enterprise Agreement (EA) customers</span></span>
<span data-ttu-id="96112-128">Nagyvállalati ügyfelek a beléptetési kvóták költségeik beállítás a minden részleg számára is megkapja az értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="96112-128">EA customers can get alerts for each department under an enrollment by setting spending quotas.</span></span> <span data-ttu-id="96112-129">Lásd: [részleg költségeik kvóták](https://ea.azure.com/helpdocs/departmentSpendingQuotas) a kezdéshez EA portálon.</span><span class="sxs-lookup"><span data-stu-id="96112-129">See [Department Spending Quotas](https://ea.azure.com/helpdocs/departmentSpendingQuotas) in the EA portal to get started.</span></span>

## <a name="learn-more-about-azure-cost-management"></a><span data-ttu-id="96112-130">További információ az Azure költség kezeléséről</span><span class="sxs-lookup"><span data-stu-id="96112-130">Learn more about Azure cost management</span></span>
- <span data-ttu-id="96112-131">Becsült költségeit használatával a [árképzési Számológép](https://azure.microsoft.com/pricing/calculator/), [összköltsége tulajdonjoga Számológép](https://aka.ms/azure-tco-calculator), és egy szolgáltatás hozzáadásakor.</span><span class="sxs-lookup"><span data-stu-id="96112-131">Estimate costs using the [pricing calculator](https://azure.microsoft.com/pricing/calculator/), [total cost of ownership calculator](https://aka.ms/azure-tco-calculator), and when you add a service.</span></span>
- <span data-ttu-id="96112-132">[Tekintse át a használati és költségek az Azure portálon rendszeresen](billing-getting-started.md#costs).</span><span class="sxs-lookup"><span data-stu-id="96112-132">[Review your usage and costs regularly in Azure portal](billing-getting-started.md#costs).</span></span>
- <span data-ttu-id="96112-133">Kapcsolja be a [Azure Advisor költség javaslatok](../advisor/advisor-cost-recommendations.md).</span><span class="sxs-lookup"><span data-stu-id="96112-133">Turn on [Azure Advisor cost recommendations](../advisor/advisor-cost-recommendations.md).</span></span>

<span data-ttu-id="96112-134">További tudnivalókért lásd: [Azure költség felügyeleti útmutató](billing-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="96112-134">To learn more, see [Azure cost management guidance](billing-getting-started.md).</span></span>

[Image1]: ./media/azure-billing-set-up-alerts/billingalert1.png 
[Image2]: ./media/azure-billing-set-up-alerts/billingalert2.png
[Image3]: ./media/azure-billing-set-up-alerts/billingalerts3.png 
