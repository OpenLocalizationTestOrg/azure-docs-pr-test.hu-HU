---
title: "Az Azure-előfizetést |} Microsoft Docs"
description: "Ismerteti, hogyan lehet Azure előfizetés, például az ingyenes próba-előfizetést"
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.assetid: 3051d6b0-179f-4e3a-bda4-3fee7135eac5
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: genli
ms.openlocfilehash: c415fada30aa0b0bd9b9d1e416bc37ef30653f68
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="cancel-your-subscription-for-azure"></a><span data-ttu-id="11a17-103">Az Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="11a17-103">Cancel your subscription for Azure</span></span>

<span data-ttu-id="11a17-104">Az Azure-előfizetéssel, visszavonhatja a [Fiókadminisztrátort](billing-subscription-transfer.md#whoisaa).</span><span class="sxs-lookup"><span data-stu-id="11a17-104">You can cancel your Azure subscription as the [Account Administrator](billing-subscription-transfer.md#whoisaa).</span></span> <span data-ttu-id="11a17-105">Az előfizetés megszüntetése után megszűnik az Azure-szolgáltatásokhoz és erőforrásokhoz való hozzáférését.</span><span class="sxs-lookup"><span data-stu-id="11a17-105">After you cancel the subscription, your access to Azure services and resources ends.</span></span>

<span data-ttu-id="11a17-106">Mielőtt az előfizetés megszüntetése:</span><span class="sxs-lookup"><span data-stu-id="11a17-106">Before you cancel your subscription:</span></span>

* <span data-ttu-id="11a17-107">Készítsen biztonsági másolatot az adatairól.</span><span class="sxs-lookup"><span data-stu-id="11a17-107">Back up your data.</span></span> <span data-ttu-id="11a17-108">Például ha az Azure storage vagy SQL adatokat tárolja, töltse le.</span><span class="sxs-lookup"><span data-stu-id="11a17-108">For example, if you're storing data in Azure storage or SQL, download a copy.</span></span> <span data-ttu-id="11a17-109">Ha egy virtuális gépet, mentse a lemezképet, helyileg.</span><span class="sxs-lookup"><span data-stu-id="11a17-109">If you have a virtual machine, save an image of it locally.</span></span>
* <span data-ttu-id="11a17-110">A szolgáltatások leállítása.</span><span class="sxs-lookup"><span data-stu-id="11a17-110">Shut down your services.</span></span> <span data-ttu-id="11a17-111">Lépjen a [erőforrások lapon a kezelési portál](https://ms.portal.azure.com/?flight=1#blade/HubsExtension/Resources/resourceType/Microsoft.Resources%2Fresources), és **leállítása** minden futó virtuális gépek, alkalmazások vagy más szolgáltatásokkal.</span><span class="sxs-lookup"><span data-stu-id="11a17-111">Go to the [resources page in the management portal](https://ms.portal.azure.com/?flight=1#blade/HubsExtension/Resources/resourceType/Microsoft.Resources%2Fresources), and **Stop** any running virtual machines, applications, or other services.</span></span>
* <span data-ttu-id="11a17-112">Vegye figyelembe az adatok áttelepítését.</span><span class="sxs-lookup"><span data-stu-id="11a17-112">Consider migrating your data.</span></span> <span data-ttu-id="11a17-113">Lásd: [erőforrások áthelyezése új erőforráscsoportba vagy előfizetésbe](../azure-resource-manager/resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="11a17-113">See [Move resources to new resource group or subscription](../azure-resource-manager/resource-group-move-resources.md).</span></span>

<span data-ttu-id="11a17-114">Ha megszakítja a fizetett [Azure támogatási csomag](https://azure.microsoft.com/support/plans/), akkor rendszer továbbra is a számlázás havonta a többi a 6 hónapos időszakra.</span><span class="sxs-lookup"><span data-stu-id="11a17-114">If you cancel a paid [Azure Support plan](https://azure.microsoft.com/support/plans/), you are still billed monthly for the rest of the 6-months term.</span></span>

## <a name="cancel-subscription-using-the-azure-portal"></a><span data-ttu-id="11a17-115">Az Azure portál használatával előfizetés megszüntetése</span><span class="sxs-lookup"><span data-stu-id="11a17-115">Cancel subscription using the Azure portal</span></span>

1. <span data-ttu-id="11a17-116">Válassza ki az előfizetést a [előfizetések oldalán](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).</span><span class="sxs-lookup"><span data-stu-id="11a17-116">Select your subscription from the [Subscriptions page](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).</span></span>

1. <span data-ttu-id="11a17-117">Válassza ki az előfizetést, szakítsa meg, és kattintson a kívánt **előfizetés megszüntetése**.</span><span class="sxs-lookup"><span data-stu-id="11a17-117">Select the subscription that you want to cancel and click **Cancel subscription**.</span></span>

    ![A Mégse gomb képernyőkép](./media/billing-how-to-cancel-azure-subscription/cancel_ibiza.png)

1. <span data-ttu-id="11a17-119">Kövesse az utasításokat, és fejezze be a megszakítását.</span><span class="sxs-lookup"><span data-stu-id="11a17-119">Follow prompts and finish cancellation.</span></span>

## <a name="cancel-subscription-using-the-azure-account-center"></a><span data-ttu-id="11a17-120">Az Azure Account Center használata előfizetés megszüntetése</span><span class="sxs-lookup"><span data-stu-id="11a17-120">Cancel subscription using the Azure Account Center</span></span>

1. <span data-ttu-id="11a17-121">Jelentkezzen be a [Azure Account Center](https://account.windowsazure.com/subscriptions) fiók rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="11a17-121">Sign in to the [Azure Account Center](https://account.windowsazure.com/subscriptions) as the Account Administrator.</span></span>

1. <span data-ttu-id="11a17-122">A **kattintson egy előfizetésre, a részletek és a használati**, válassza ki az előfizetést, amely megszakítja.</span><span class="sxs-lookup"><span data-stu-id="11a17-122">Under **Click a subscription to view details and usage**, select the subscription that you want to cancel.</span></span>

    ![Egy példa kijelölt előfizetésben képernyőkép](./media/billing-how-to-cancel-azure-subscription/Selectsub.png)

1. <span data-ttu-id="11a17-124">A lap jobb oldalán válassza ki a **előfizetés törlése**.</span><span class="sxs-lookup"><span data-stu-id="11a17-124">On the right side of the page, select **Cancel Subscription**.</span></span>

    ![Az előfizetés elutasítógombja képernyőkép](./media/billing-how-to-cancel-azure-subscription/cancelsub.png)

1. <span data-ttu-id="11a17-126">Válassza ki **Igen, az előfizetés megszüntetése**.</span><span class="sxs-lookup"><span data-stu-id="11a17-126">Select **Yes, cancel my subscription**.</span></span>

    ![A párbeszédpanel bezárása képernyőkép](./media/billing-how-to-cancel-azure-subscription/cancelbox.png)

1. <span data-ttu-id="11a17-128">Kattintson a következőre:</span><span class="sxs-lookup"><span data-stu-id="11a17-128">Click</span></span> ![Ellenőrizze a szimbólum gomb](./media/billing-how-to-cancel-azure-subscription/checkbutton.png) <span data-ttu-id="11a17-130">a párbeszédpanel bezárásához, és az előfizetési laphoz való visszatéréshez.</span><span class="sxs-lookup"><span data-stu-id="11a17-130">to close the dialog window and return to your subscription page.</span></span>

## <a name="what-happens-after-i-cancel-my-subscription"></a><span data-ttu-id="11a17-131">Mi történik az I-előfizetés megszüntetése után?</span><span class="sxs-lookup"><span data-stu-id="11a17-131">What happens after I cancel my subscription?</span></span>

<span data-ttu-id="11a17-132">Ha megszakítja, számlázási azonnal leáll.</span><span class="sxs-lookup"><span data-stu-id="11a17-132">Once you cancel, billing is stopped immediately.</span></span> <span data-ttu-id="11a17-133">A portál azonban is igénybe vehet a megszakítási műsor akár 10 percet.</span><span class="sxs-lookup"><span data-stu-id="11a17-133">However, it can take up to 10 minutes for the cancellation show in the portal.</span></span>

<span data-ttu-id="11a17-134">Ezt követően a szolgáltatások le vannak tiltva.</span><span class="sxs-lookup"><span data-stu-id="11a17-134">After that, your services are disabled.</span></span> <span data-ttu-id="11a17-135">Ez azt jelenti, hogy a virtuális gépek fel van szabadítva, ideiglenes IP-címek felszabadítását és tárolási csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="11a17-135">That means your virtual machines are deallocated, temporary IP addresses are freed, and storage is read-only.</span></span>

<span data-ttu-id="11a17-136">Kivéve, ha használ, egy ingyenes próbaverziót, vagy elérhető kreditek rendelkezik, akkor bármely függőben lévő használati díjak, az utolsó számlázási ciklus és a megszakítási dátum közötti díjon számlázzuk.</span><span class="sxs-lookup"><span data-stu-id="11a17-136">Unless you’re on a Free Trial or have credits available, you’re billed for any outstanding usage charges generated between your last billing cycle and the cancellation date.</span></span> <span data-ttu-id="11a17-137">Az elszámolási időszak végén az utolsó számlázási kap.</span><span class="sxs-lookup"><span data-stu-id="11a17-137">You get your last bill at the end of the billing cycle.</span></span>

<span data-ttu-id="11a17-138">Az előfizetés megszüntetése után 90 nappal korábbinak véglegesen törli az adatokat, abban az esetben kell-e férni vagy megváltoztatja döntését várunk.</span><span class="sxs-lookup"><span data-stu-id="11a17-138">After you cancel your subscription, we wait 90 days before permanently deleting your data in case you need to access it or you change your mind.</span></span> <span data-ttu-id="11a17-139">Jelenleg nem díjat számítanak fel az adatok megőrzésével.</span><span class="sxs-lookup"><span data-stu-id="11a17-139">We don't charge you for retaining the data.</span></span> <span data-ttu-id="11a17-140">További tudnivalókért lásd: [Microsoft Trust Center – hogyan azt kezeli az adatokat](https://go.microsoft.com/fwLink/p/?LinkID=822930&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="11a17-140">To learn more, see [Microsoft Trust Center - How we manage your data](https://go.microsoft.com/fwLink/p/?LinkID=822930&clcid=0x409).</span></span>

## <a name="reactivate-subscription"></a><span data-ttu-id="11a17-141">Előfizetés újraaktiválása</span><span class="sxs-lookup"><span data-stu-id="11a17-141">Reactivate subscription</span></span>

<span data-ttu-id="11a17-142">Ha véletlenül a használatalapú előfizetés megszüntetése, akkor [aktiválja a fiókok Center](billing-subscription-become-disable.md).</span><span class="sxs-lookup"><span data-stu-id="11a17-142">If you cancel your Pay-As-You-Go subscription accidentally, you can [reactivate it in the Accounts Center](billing-subscription-become-disable.md).</span></span>

<span data-ttu-id="11a17-143">Ha az előfizetés nincs használatalapú fizetés, forduljon a támogatási szolgálathoz, az előfizetés újraaktiválásához törlését követő 90 napon belül.</span><span class="sxs-lookup"><span data-stu-id="11a17-143">If your subscription is not Pay-As-You-Go, contact support within 90 days of cancellation to reactivate your subscription.</span></span>

## <a name="need-help-contact-support"></a><span data-ttu-id="11a17-144">Segítség</span><span class="sxs-lookup"><span data-stu-id="11a17-144">Need help?</span></span> <span data-ttu-id="11a17-145">Forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="11a17-145">Contact support.</span></span>

<span data-ttu-id="11a17-146">Ha további kérdései, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma elhárítva gyors eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="11a17-146">If you still have questions, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to get your issue resolved quickly.</span></span>
