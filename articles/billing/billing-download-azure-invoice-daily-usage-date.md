---
title: "Töltse le a számla és a napi használati adatok számlázási Azure |} Microsoft Docs"
description: "Ismerteti az Azure számlázási számla és a napi használati adatok megtekintése és letöltése."
keywords: "számlázási számla, számla letöltési, azure számla, azure kihasználtsága"
services: 
documentationcenter: 
author: genlin
manager: tonguyen
editor: 
tags: billing
ms.assetid: 6d568d1d-3bd6-4348-97d0-1098b5fe0661
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c63d9523555f47b4e5c0f3766f3ba080f76ac19e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="download-or-view-your-azure-billing-invoice-and-daily-usage-data"></a><span data-ttu-id="581f8-104">Az Azure számlázási számla és a napi használati adatok megtekintése és letöltése</span><span class="sxs-lookup"><span data-stu-id="581f8-104">Download or view your Azure billing invoice and daily usage data</span></span>
<span data-ttu-id="581f8-105">A számla a programot letöltheti a [Azure-portálon](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) vagy annak az e-mailben küldött.</span><span class="sxs-lookup"><span data-stu-id="581f8-105">You can download your invoice from the [Azure portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) or have it sent in email.</span></span> <span data-ttu-id="581f8-106">A napi használat letöltéséhez keresse fel a [Azure Account Center](https://account.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="581f8-106">To download your daily usage, go to the [Azure Account Center](https://account.windowsazure.com).</span></span> <span data-ttu-id="581f8-107">Csak egyes szerepkörök engedélye számlázási számla és használati adatokat, például a fiók rendszergazdájához.</span><span class="sxs-lookup"><span data-stu-id="581f8-107">Only certain roles have permission to get billing invoice and usage information, like the Account Administrator.</span></span> <span data-ttu-id="581f8-108">Első számlázási információhoz való hozzáféréssel kapcsolatos további információkért lásd: [Azure számlázási szerepkörök használatával való hozzáférés kezelése](billing-manage-access.md).</span><span class="sxs-lookup"><span data-stu-id="581f8-108">To learn more about getting access to billing information, see [Manage access to Azure billing using roles](billing-manage-access.md).</span></span>

## <a name="get-your-invoice-in-email-pdf"></a><span data-ttu-id="581f8-109">A számla beolvasni e-mailben (.pdf)</span><span class="sxs-lookup"><span data-stu-id="581f8-109">Get your invoice in email (.pdf)</span></span>
<span data-ttu-id="581f8-110">Részt, és konfigurálja az Azure fogadásához további címzetteket számla e-mailben.</span><span class="sxs-lookup"><span data-stu-id="581f8-110">You can opt in and configure additional recipients to receive your Azure invoice in an email.</span></span> <span data-ttu-id="581f8-111">Ez a szolgáltatás nem az egyes előfizetések például támogatási ajánlatokat, kötött nagyvállalati szerződése vagy Azure in Open licencprogram érhetők el.</span><span class="sxs-lookup"><span data-stu-id="581f8-111">This feature may not be available for certain subscriptions such as support offers, Enterprise Agreements, or Azure in Open.</span></span>

1. <span data-ttu-id="581f8-112">Válassza ki az előfizetést a [előfizetések panel](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).</span><span class="sxs-lookup"><span data-stu-id="581f8-112">Select your subscription from the [Subscriptions blade](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).</span></span> <span data-ttu-id="581f8-113">Részvétel az egyes előfizetésekhez Ön a tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="581f8-113">Opt-in for each subscription you own.</span></span> <span data-ttu-id="581f8-114">Kattintson a **számlák** majd **E-mail-a számla**.</span><span class="sxs-lookup"><span data-stu-id="581f8-114">Click **Invoices** then **Email my invoice**.</span></span> 

    ![Képernyőkép a szemlélteti a szolgáltatás aktiválása](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)
    
2. <span data-ttu-id="581f8-116">Kattintson a **részt vevő** és fogadja el a feltételeket.</span><span class="sxs-lookup"><span data-stu-id="581f8-116">Click **Opt in** and accept the terms.</span></span>

    ![Képernyőkép a szemlélteti a szolgáltatás aktiválása](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)
 
3. <span data-ttu-id="581f8-118">A szerződés elfogadása után további címzetteket is konfigurálhat.</span><span class="sxs-lookup"><span data-stu-id="581f8-118">Once you've accepted the agreement, you can configure additional recipients.</span></span>

    ![Képernyőkép a szemlélteti a szolgáltatás aktiválása](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)
    
<span data-ttu-id="581f8-120">Ha lépések végrehajtása után nem kap egy e-mailt, győződjön meg arról, hogy az e-mail cím helyes-e a [kapcsolattartási beállítások a profil](https://account.windowsazure.com/profile).</span><span class="sxs-lookup"><span data-stu-id="581f8-120">If you don't get an email after following the steps, make sure your email address is correct in the [communication preferences on your profile](https://account.windowsazure.com/profile).</span></span>

## <a name="download-invoice-from-azure-portal-pdf"></a><span data-ttu-id="581f8-121">Töltse le a számla az Azure portál (.pdf)</span><span class="sxs-lookup"><span data-stu-id="581f8-121">Download invoice from Azure portal (.pdf)</span></span>

1. <span data-ttu-id="581f8-122">Válassza ki az előfizetést a [előfizetések panel](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) , Azure-portálon [számlák hozzáférést egy felhasználó](billing-manage-access.md).</span><span class="sxs-lookup"><span data-stu-id="581f8-122">Select your subscription from the [Subscriptions blade](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in Azure portal as [an user with access to invoices](billing-manage-access.md).</span></span>

2. <span data-ttu-id="581f8-123">Válassza ki **számlák**.</span><span class="sxs-lookup"><span data-stu-id="581f8-123">Select **Invoices**.</span></span> 

    ![Képernyőkép a számlázási és használati beállítás](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png) 

3. <span data-ttu-id="581f8-125">Kattintson a **töltse le a számla** a PDF számla megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="581f8-125">Click **Download Invoice** to view a copy of your PDF invoice.</span></span> <span data-ttu-id="581f8-126">Ha a felirat látható **nem érhető el**, lásd: [miért nem látom számla a legutóbbi számlázási időszakban?](#noinvoice)</span><span class="sxs-lookup"><span data-stu-id="581f8-126">If it says **Not available**, see [Why don't I see an invoice for the last billing period?](#noinvoice)</span></span>

    ![Képernyőkép a számlázási időszak letöltése beállítás, és az összes díj minden számlázási időszakban](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

4. <span data-ttu-id="581f8-128">A napi használat a számlázott időszak kattintva is megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="581f8-128">You can also view your daily usage by clicking the billing period.</span></span> 

<span data-ttu-id="581f8-129">A számla kapcsolatos további információkért lásd: [a számlázási megérteni a Microsoft Azure](billing-understand-your-bill.md).</span><span class="sxs-lookup"><span data-stu-id="581f8-129">For more information about your invoice, see [Understand your bill for Microsoft Azure](billing-understand-your-bill.md).</span></span> <span data-ttu-id="581f8-130">Költségek kezelésére, lásd: [Azure számlázás és költség felügyeleti váratlan költségek megakadályozása](billing-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="581f8-130">For help managing costs, see [Prevent unexpected costs with Azure billing and cost management](billing-getting-started.md).</span></span>

## <a name="download-usage-from-the-account-center-csv"></a><span data-ttu-id="581f8-131">Használati letöltését az Account Center (.csv)</span><span class="sxs-lookup"><span data-stu-id="581f8-131">Download usage from the Account Center (.csv)</span></span>

1. <span data-ttu-id="581f8-132">Jelentkezzen be a [Azure Account Center](https://account.windowsazure.com/subscriptions) fiók rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="581f8-132">Sign into the [Azure Account Center](https://account.windowsazure.com/subscriptions) as the Account Administrator.</span></span>

2. <span data-ttu-id="581f8-133">Válassza ki az előfizetést, legyen a számla és használati adatokat.</span><span class="sxs-lookup"><span data-stu-id="581f8-133">Select the subscription for which you want the invoice and usage information.</span></span>

3. <span data-ttu-id="581f8-134">Válassza ki **számlázási előzmények**.</span><span class="sxs-lookup"><span data-stu-id="581f8-134">Select **BILLING HISTORY**.</span></span> 

    ![Képernyőkép a Számlázási előzmények beállítás](./media/billing-download-azure-invoice-daily-usage-date/Billinghisotry.png)

4. <span data-ttu-id="581f8-136">Az utasítás a legutóbbi hat számlázási időszak és az aktuális teljesüléséig időszak tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="581f8-136">You can see your statements for the last six billing periods and the current unbilled period.</span></span> 

    ![Képernyőkép a számlázási időszak, töltse le a számla és a napi használat és a teljes költségek minden számlázási időszak lehetőségek](./media/billing-download-azure-invoice-daily-usage-date/billingSum.png)

5. <span data-ttu-id="581f8-138">Válassza ki **aktuális nyilatkozat megtekintése** becsült készítésének időpontjában a díjak becsült megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="581f8-138">Select **View Current Statement** to see an estimate of your charges at the time the estimate was generated.</span></span> <span data-ttu-id="581f8-139">Ezek az adatok naponta csak frissül, és nem tartalmazhatják a használati.</span><span class="sxs-lookup"><span data-stu-id="581f8-139">This information is only updated daily and may not include all your usage.</span></span> <span data-ttu-id="581f8-140">A havi számla Ez a becslés eltérhet.</span><span class="sxs-lookup"><span data-stu-id="581f8-140">Your monthly invoice may differ from this estimate.</span></span>

    ![A nézet jelenlegi utasítást beállítás képernyőkép](./media/billing-download-azure-invoice-daily-usage-date/billingSum2.png)

    ![A jelenlegi díjak becsült képernyőkép](./media/billing-download-azure-invoice-daily-usage-date/billingSum3.png)

6. <span data-ttu-id="581f8-143">Válassza ki **letöltése használati** CSV-fájlként a napi használati adatok letöltése.</span><span class="sxs-lookup"><span data-stu-id="581f8-143">Select **Download Usage** to download the daily usage data as a CSV file.</span></span> <span data-ttu-id="581f8-144">Ha elérhető két verzióját látja, töltse le a 2-es verzióját.</span><span class="sxs-lookup"><span data-stu-id="581f8-144">If you see two versions available, download version 2.</span></span>

    ![A használati letöltése beállítás képernyőkép](./media/billing-download-azure-invoice-daily-usage-date/DLusage.png)

<span data-ttu-id="581f8-146">Csak a Fiókadminisztrátor az Azure Account Center férhetnek hozzá.</span><span class="sxs-lookup"><span data-stu-id="581f8-146">Only the Account Administrator can access the Azure Account Center.</span></span> <span data-ttu-id="581f8-147">Más számlázási adminisztrátorok, például egy olyan tulajdonost, a használati adatokat a beszerezheti a [számlázási API-k](billing-usage-rate-card-overview.md).</span><span class="sxs-lookup"><span data-stu-id="581f8-147">Other billing admins, such as an Owner, can get usage information using the [Billing APIs](billing-usage-rate-card-overview.md).</span></span>

<span data-ttu-id="581f8-148">A napi használatával kapcsolatos további információkért lásd: [a számlázási megérteni a Microsoft Azure](billing-understand-your-bill.md).</span><span class="sxs-lookup"><span data-stu-id="581f8-148">For more information about your daily usage, see [Understand your bill for Microsoft Azure](billing-understand-your-bill.md).</span></span> <span data-ttu-id="581f8-149">Költségek kezelésére, lásd: [Azure számlázás és költség felügyeleti váratlan költségek megakadályozása](billing-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="581f8-149">For help managing costs, see [Prevent unexpected costs with Azure billing and cost management](billing-getting-started.md).</span></span>

## <span data-ttu-id="581f8-150"><a name="noinvoice"></a>Miért nem látja a legutóbbi számlázási időszakban számla?</span><span class="sxs-lookup"><span data-stu-id="581f8-150"><a name="noinvoice"></a> Why don't I see an invoice for the last billing period?</span></span>

<span data-ttu-id="581f8-151">Számla nem látható több oka lehet:</span><span class="sxs-lookup"><span data-stu-id="581f8-151">There could be several reasons that you don't see an invoice:</span></span>

- <span data-ttu-id="581f8-152">Ingyenes próbaverzió vagy havi összege van, amely nem túllépi az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="581f8-152">You have a monthly credit amount with your subscription that you didn't exceed or you have a Free Trial.</span></span> <span data-ttu-id="581f8-153">Számla csak akkor keletkezik, amikor pénz akikkel.</span><span class="sxs-lookup"><span data-stu-id="581f8-153">An invoice is only generated when you owe money.</span></span>

- <span data-ttu-id="581f8-154">Azure előfizetett napjától számított 30 napon belül.</span><span class="sxs-lookup"><span data-stu-id="581f8-154">It's less than 30 days from the day you subscribed to Azure.</span></span>

- <span data-ttu-id="581f8-155">A számla még a nem jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="581f8-155">The invoice isn't generated yet.</span></span> <span data-ttu-id="581f8-156">Várjon, amíg a számlázott időszak végén.</span><span class="sxs-lookup"><span data-stu-id="581f8-156">Wait until the end of the billing period.</span></span>

- <span data-ttu-id="581f8-157">Ha még nincs a Fiókadminisztrátort, korábbi számlák nem lehet avaialbe Önnek.</span><span class="sxs-lookup"><span data-stu-id="581f8-157">If you're not the Account Administrator, older invoices may not be avaialbe to you.</span></span>

## <a name="need-help-contact-support"></a><span data-ttu-id="581f8-158">Segítség</span><span class="sxs-lookup"><span data-stu-id="581f8-158">Need help?</span></span> <span data-ttu-id="581f8-159">Forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="581f8-159">Contact support.</span></span>
<span data-ttu-id="581f8-160">Ha további kérdései további, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma elhárítva gyors eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="581f8-160">If you still have further questions, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to get your issue resolved quickly.</span></span>

