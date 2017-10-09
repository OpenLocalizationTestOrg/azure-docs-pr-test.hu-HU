---
title: "aaaUnderstand az Azure külső szolgáltatási díjak |} Microsoft Docs"
description: "További információk a számlázási külső szolgáltatások, korábbi nevén a piactéren, az Azure-ban díjakat."
services: 
documentationcenter: 
author: adpick
manager: tonguyen
editor: 
tags: billing
ms.assetid: 5e0e2a3c-d111-4054-8508-0c111c1b749b
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: adpick
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1d2cb28319e2ab4eff753177220993cbf94c96ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-your-azure-billing-for-external-service-charges"></a><span data-ttu-id="92bfa-103">Az Azure számlázás külső szolgáltatás költségek ismertetése</span><span class="sxs-lookup"><span data-stu-id="92bfa-103">Understand your Azure billing for external service charges</span></span>
<span data-ttu-id="92bfa-104">Külső szolgáltatások használt Azure piactér nevű toobe.</span><span class="sxs-lookup"><span data-stu-id="92bfa-104">External services used toobe called Azure Marketplace.</span></span> <span data-ttu-id="92bfa-105">Általában által közzétett harmadik felek érhető el az Azure-szolgáltatásokat, de teljesen integrált Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="92bfa-105">Generally, they're services published by third-parties available for Azure but are integrated completely within Azure.</span></span> <span data-ttu-id="92bfa-106">Például ClearDB és SendGrid a külső szolgáltatások, Azure-ban vásárolhatnak, de nem a Microsoft által közzétett.</span><span class="sxs-lookup"><span data-stu-id="92bfa-106">For example, ClearDB and SendGrid are external services that you can purchase in Azure, but are not published by Microsoft.</span></span>

<span data-ttu-id="92bfa-107">Ha egy új külső szolgáltatás vagy az erőforrás, figyelmeztetés jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="92bfa-107">When you provision a new external service or resource, a warning is shown:</span></span>

![Piactér-beszerzési figyelmeztetés](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

> [!NOTE]
> <span data-ttu-id="92bfa-109">Külső vállalatok, amelyek nem Microsoft által közzétett szolgáltatások, de más Microsoft-termékek is kategóriába sorolni külső szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="92bfa-109">External services are published by companies that are not Microsoft, but sometimes Microsoft products are also categorized as external services.</span></span>
> 
> 

## <a name="how-external-services-are-billed"></a><span data-ttu-id="92bfa-110">Hogyan külső szolgáltatások számlázása</span><span class="sxs-lookup"><span data-stu-id="92bfa-110">How external services are billed</span></span>
- <span data-ttu-id="92bfa-111">Külső szolgáltatások külön vannak számlázva.</span><span class="sxs-lookup"><span data-stu-id="92bfa-111">External services are billed separately.</span></span> <span data-ttu-id="92bfa-112">Az Azure-előfizetéshez belül egyedi rendelések tekintendők.</span><span class="sxs-lookup"><span data-stu-id="92bfa-112">They are treated as individual orders within your Azure subscription.</span></span> <span data-ttu-id="92bfa-113">hello számlázási időszakban az egyes szolgáltatások beállítása során vásárol hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="92bfa-113">hello billing period for each service is set when you purchase hello service.</span></span> <span data-ttu-id="92bfa-114">Nem toobe összetéveszthető hello számlázási időszakban hello előfizetés, amelynek keretében megvásárolta azt.</span><span class="sxs-lookup"><span data-stu-id="92bfa-114">Not toobe confused with hello billing period of hello subscription under which you purchased it.</span></span> <span data-ttu-id="92bfa-115">Külön váltók is kap, és a hitelkártya felszámítása külön történik.</span><span class="sxs-lookup"><span data-stu-id="92bfa-115">You also receive separate bills and your credit card is charged separately.</span></span>
- <span data-ttu-id="92bfa-116">Egyes külső szolgáltatásnak van egy másik számlázási modellt.</span><span class="sxs-lookup"><span data-stu-id="92bfa-116">Each external service has a different billing model.</span></span> <span data-ttu-id="92bfa-117">Egyes szolgáltatások számlázása a használatalapú módon történt mások havi alapján fizetési modell használatára.</span><span class="sxs-lookup"><span data-stu-id="92bfa-117">Some services are billed in a pay-as-you-go fashion while others use a monthly based payment model.</span></span> <span data-ttu-id="92bfa-118">Hitelkártya Azure külső-szolgáltatások van szüksége, külső szolgáltatások számla fizetéssel nem vásárolhat.</span><span class="sxs-lookup"><span data-stu-id="92bfa-118">You need a credit card for Azure external services, you can't buy external services with invoice pay.</span></span>
- <span data-ttu-id="92bfa-119">Ingyenes havi krediteket külső szolgáltatások nem használható.</span><span class="sxs-lookup"><span data-stu-id="92bfa-119">You can't use monthly free credits for external services.</span></span> <span data-ttu-id="92bfa-120">Amely tartalmazza az Azure-előfizetés használata [kreditek szabad](https://azure.microsoft.com/pricing/spending-limits/), akkor nem lehet alkalmazott tooexternal szolgáltatás váltók.</span><span class="sxs-lookup"><span data-stu-id="92bfa-120">If you are using an Azure subscription that includes [free credits](https://azure.microsoft.com/pricing/spending-limits/), they can't be applied tooexternal service bills.</span></span> <span data-ttu-id="92bfa-121">A hitelkártya toopurchase külső szolgáltatások használata.</span><span class="sxs-lookup"><span data-stu-id="92bfa-121">Use a credit card toopurchase external services.</span></span>


## <a name="view-external-service-spending-and-history-in-hello-azure-portal"></a><span data-ttu-id="92bfa-122">Nézet külső szolgáltatás költségeik és előzmények hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="92bfa-122">View external service spending and history in hello Azure portal</span></span>
<span data-ttu-id="92bfa-123">Megtekintheti az egyes előfizetések belül hello hello külső szolgáltatások [Azure-portálon](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="92bfa-123">You can view a list of hello external services that are on each subscription within hello [Azure portal](https://portal.azure.com/):</span></span> 

1. <span data-ttu-id="92bfa-124">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/) hello fiókadminisztrátora.</span><span class="sxs-lookup"><span data-stu-id="92bfa-124">Sign in toohello [Azure portal](https://portal.azure.com/) as hello account administrator.</span></span>
2. <span data-ttu-id="92bfa-125">Hello menüben válassza ki **előfizetések**.</span><span class="sxs-lookup"><span data-stu-id="92bfa-125">In hello Hub menu, select **Subscriptions**.</span></span>
   
    ![Válassza ki az előfizetések hello központ menü](./media/billing-understand-your-azure-marketplace-charges/sub-button.png) 
3. <span data-ttu-id="92bfa-127">A hello **előfizetések** panelen, jelölje be hello előfizetés tooview szeretne, és válassza **külső szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="92bfa-127">In hello **Subscriptions** blade, select hello subscription that you want tooview, and then select **External services**.</span></span>
   
    ![Válasszon egy előfizetést hello számlázási panelen](./media/billing-understand-your-azure-marketplace-charges/select-sub-external-services.png)
4. <span data-ttu-id="92bfa-129">A külső szolgáltatás rendeléseket hello közzétevő neve, vásárolt szolgáltatási rétegben, hello erőforrás és hello rendelés állapota megadott névnek mindegyikének meg kell jelennie.</span><span class="sxs-lookup"><span data-stu-id="92bfa-129">You should see each of your external service orders, hello publisher name, service tier you bought, name you gave hello resource, and hello current order status.</span></span> <span data-ttu-id="92bfa-130">toosee korábbi számlák, válasszon ki egy külső szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="92bfa-130">toosee past bills, select an external service.</span></span>
   
    ![Válasszon ki egy külső szolgáltatást](./media/billing-understand-your-azure-marketplace-charges/external-service-blade2.png)
5. <span data-ttu-id="92bfa-132">Itt megtekintheti, hello adó lebontása például számlázási összegek részhez.</span><span class="sxs-lookup"><span data-stu-id="92bfa-132">From here, you can view past bill amounts including hello tax breakdown.</span></span>
   
    ![Külső szolgáltatások számlázási előzmények megtekintése](./media/billing-understand-your-azure-marketplace-charges/billing-overview-blade.png)

## <a name="view-external-service-spending-for-enterprise-agreement-ea-customers"></a><span data-ttu-id="92bfa-134">Nagyvállalati Szerződés (EA) ügyfelek esetén ebből külső tartalom megtekintése</span><span class="sxs-lookup"><span data-stu-id="92bfa-134">View external service spending for Enterprise Agreement (EA) customers</span></span>
<span data-ttu-id="92bfa-135">Nagyvállalati ügyfelek Lásd: a külső szolgáltatás kiadásokat, és töltse le a jelentések hello EA portálon.</span><span class="sxs-lookup"><span data-stu-id="92bfa-135">EA customers can see external service spending and download reports in hello EA portal.</span></span> <span data-ttu-id="92bfa-136">Lásd: [Azure piactér a Nagyvállalati ügyfelek](https://ea.azure.com/helpdocs/azureMarketplace) tooget elindult.</span><span class="sxs-lookup"><span data-stu-id="92bfa-136">See [Azure Marketplace for EA Customers](https://ea.azure.com/helpdocs/azureMarketplace) tooget started.</span></span>

## <a name="manage-payment-methods-for-external-service-orders"></a><span data-ttu-id="92bfa-137">Külső szolgáltatás rendelések fizetési módok kezelése</span><span class="sxs-lookup"><span data-stu-id="92bfa-137">Manage payment methods for external service orders</span></span>
<span data-ttu-id="92bfa-138">Frissíti a fizetési módok, a külső szolgáltatás rendelést hello [Account Center](https://account.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="92bfa-138">Update your payment methods for external service orders from hello [Account Center](https://account.windowsazure.com/).</span></span>

> [!NOTE]
> <span data-ttu-id="92bfa-139">Ha a munkahelyi vagy iskolai fiókkal vásárolt [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) toomake változik tooyour fizetési módot.</span><span class="sxs-lookup"><span data-stu-id="92bfa-139">If you purchased your subscription with a Work or School account, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) toomake changes tooyour payment method.</span></span>
> 
> 

1. <span data-ttu-id="92bfa-140">Jelentkezzen be toohello [Account Center](https://account.windowsazure.com/) és [keresse meg a toohello **piactér** lap](https://account.windowsazure.com/Store)</span><span class="sxs-lookup"><span data-stu-id="92bfa-140">Sign in toohello [Account Center](https://account.windowsazure.com/) and [navigate toohello **marketplace** tab](https://account.windowsazure.com/Store)</span></span>
   
    ![Válassza ki a piactér hello fiók központban](./media/billing-understand-your-azure-marketplace-charges/select-marketplace.png)
2. <span data-ttu-id="92bfa-142">Válassza ki a kívánt toomanage hello külső szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="92bfa-142">Select hello external service you want toomanage</span></span>
   
    ![Válassza ki a kívánt toomanage hello külső szolgáltatás](./media/billing-understand-your-azure-marketplace-charges/select-ext-service.png)
3. <span data-ttu-id="92bfa-144">Kattintson a **fizetési mód megváltoztatása** hello jobb oldalán található hello.</span><span class="sxs-lookup"><span data-stu-id="92bfa-144">Click **Change payment method** on hello right side of hello page.</span></span> <span data-ttu-id="92bfa-145">Ez a hivatkozás számos lehetőséget kínál, tooa különböző portál toomanage a fizetési módot.</span><span class="sxs-lookup"><span data-stu-id="92bfa-145">This link brings you tooa different portal toomanage your payment method.</span></span>
   
    ![Rendelés összegzése](./media/billing-understand-your-azure-marketplace-charges/change-payment.PNG)
4. <span data-ttu-id="92bfa-147">Kattintson a **adatainak szerkesztése** , és kövesse az utasításokat tooupdate a fizetési adatok.</span><span class="sxs-lookup"><span data-stu-id="92bfa-147">Click **Edit info** and follow instructions tooupdate your payment information.</span></span>
   
    ![Válassza ki a Szerkesztés adatai](./media/billing-understand-your-azure-marketplace-charges/edit-info.png)

## <a name="cancel-an-external-service-order"></a><span data-ttu-id="92bfa-149">Egy külső szolgáltatás rendelés visszavonása</span><span class="sxs-lookup"><span data-stu-id="92bfa-149">Cancel an external service order</span></span>
<span data-ttu-id="92bfa-150">Ha szeretné toocancel külső szolgáltatás rendelés, a hello hello erőforrás törlése [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="92bfa-150">If you want toocancel your external service order, delete hello resource in hello [Azure portal](https://portal.azure.com).</span></span>

![Erőforrás törlése](./media/billing-understand-your-azure-marketplace-charges/deleteMarketplaceOrder.PNG)

## <a name="need-help-contact-support"></a><span data-ttu-id="92bfa-152">Segítség</span><span class="sxs-lookup"><span data-stu-id="92bfa-152">Need help?</span></span> <span data-ttu-id="92bfa-153">Forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="92bfa-153">Contact support.</span></span>
<span data-ttu-id="92bfa-154">Ha további kérdései, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma megoldódik gyorsan tooget.</span><span class="sxs-lookup"><span data-stu-id="92bfa-154">If you still have questions, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget your issue resolved quickly.</span></span>

