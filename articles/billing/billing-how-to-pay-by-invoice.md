---
title: "az Azure-előfizetések számlán aaaPay |} Microsoft Docs"
description: "Ismerteti, hogyan toopay az Azure-előfizetések számlán"
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: genli
ms.openlocfilehash: 416890cc1e5109ef4d2bf6a9c2a779ba835f410c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="submit-a-request-toopay-azure-subscription-by-invoice"></a><span data-ttu-id="9a8ac-103">Küldje el a kérelmet toopay Azure-előfizetés számlán</span><span class="sxs-lookup"><span data-stu-id="9a8ac-103">Submit a request toopay Azure subscription by invoice</span></span>

<span data-ttu-id="9a8ac-104">Hello fizetési mód módosíthatja az az Azure-előfizetés tooinvoice tooAzure támogatási kérelem elküldése.</span><span class="sxs-lookup"><span data-stu-id="9a8ac-104">You can change hello payment method for your Azure subscription tooinvoice by submitting a request tooAzure support.</span></span> <span data-ttu-id="9a8ac-105">Amint jóváhagyják a kérelmét, hogyan utasításokat rendelkezésre állni az előfizetése hello számla fizetési mód tooset.</span><span class="sxs-lookup"><span data-stu-id="9a8ac-105">Once your request is approved, you are provided instructions on how tooset up your subscription for hello invoice payment method.</span></span>

> [!IMPORTANT]
> * <span data-ttu-id="9a8ac-106">Számla fizetési üzleti partnerek csak érhető el.</span><span class="sxs-lookup"><span data-stu-id="9a8ac-106">Invoice pay is only available for business accounts.</span></span>
> * <span data-ttu-id="9a8ac-107">[Harmadik féltől származó és a külső szolgáltatások](billing-understand-your-azure-marketplace-charges.md) nem vásárolt és kifizette számla fizetési használatával.</span><span class="sxs-lookup"><span data-stu-id="9a8ac-107">[Third party and external services](billing-understand-your-azure-marketplace-charges.md) cannot be purchased or paid for using invoice pay.</span></span> <span data-ttu-id="9a8ac-108">Ha az előfizetése tartalmazza majd a külső szolgáltatásokat, mint a ClearDB vagy SendGrid erőforrásaihoz, azok tooinvoice fizetési módosítása előtt kell törölhető.</span><span class="sxs-lookup"><span data-stu-id="9a8ac-108">If your subscription contains resources from external services like ClearDB or SendGrid, they need be deleted before changing tooinvoice pay.</span></span> <span data-ttu-id="9a8ac-109">toopurchase külső-szolgáltatások tooinvoice fizetési visszakapcsolása után kell a különálló előfizetést vagy követel kártya.</span><span class="sxs-lookup"><span data-stu-id="9a8ac-109">toopurchase external services after switching tooinvoice pay, you need a separate subscription with a credit or debit card.</span></span>
> * <span data-ttu-id="9a8ac-110">Tooinvoice fizetési vált, amennyiben nem lehet átállítani a hátsó toocredit vagy tartozik kártya fizetési.</span><span class="sxs-lookup"><span data-stu-id="9a8ac-110">Once you switch tooinvoice pay, you can't switch back toocredit or debit card payment.</span></span>

## <a name="request-pay-by-invoice"></a><span data-ttu-id="9a8ac-111">Kérelem fizetési számlán</span><span class="sxs-lookup"><span data-stu-id="9a8ac-111">Request pay by invoice</span></span>

1. <span data-ttu-id="9a8ac-112">Jelentkezzen be a hello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="9a8ac-112">Sign into hello [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="9a8ac-113">Válassza ki **súgó + támogatás** > **új támogatja a kérelem**.</span><span class="sxs-lookup"><span data-stu-id="9a8ac-113">Select **Help + support** > **New support request**.</span></span>

    ![a Súgó és támogatás gomb](./media/billing-how-to-pay-by-invoice/helpandsupport.png)
1. <span data-ttu-id="9a8ac-115">Válassza ki **számlázási** hello probléma típusa, válassza ki a hello előfizetést, amelynek meg toopay szeretné számlán, támogatási terv, majd válassza ki és **következő**.</span><span class="sxs-lookup"><span data-stu-id="9a8ac-115">Select **Billing** as hello issue type, select hello subscription for which you want toopay by invoice, select a support plan, and then select **Next**.</span></span>
1. <span data-ttu-id="9a8ac-116">A hello **probléma** panelen válassza **számla fizet** a hello **problématípust** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="9a8ac-116">In hello **Problem** blade, select **Pay by Invoice** in hello **Problem Type** box.</span></span>
1. <span data-ttu-id="9a8ac-117">Adja meg a következő információ a hello hello **részletek** mezőbe, majd válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="9a8ac-117">Enter hello following information in hello **Details** box, and then select **Next**.</span></span>

    * <span data-ttu-id="9a8ac-118">A cég neve</span><span class="sxs-lookup"><span data-stu-id="9a8ac-118">Company name</span></span>
    * <span data-ttu-id="9a8ac-119">Számlázási cím</span><span class="sxs-lookup"><span data-stu-id="9a8ac-119">Billing address</span></span>
    * [<span data-ttu-id="9a8ac-120">Fiók rendszergazdai e-mail címet</span><span class="sxs-lookup"><span data-stu-id="9a8ac-120">Account administrator's email address</span></span>](billing-add-change-azure-subscription-administrator.md#check-the-account-administrator-of-the-subscription)

1. <span data-ttu-id="9a8ac-121">Ellenőrizze a kapcsolattartási adatokat és az elsődleges kapcsolattartási módszert, és kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="9a8ac-121">Verify your contact information and preferred contact method, and then click **Create**.</span></span>

<span data-ttu-id="9a8ac-122">Ha igazolnia kell a szükséges jóváírás hello mennyisége miatt ellenőrizze követel toorun, kapni hitelkérelemben ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="9a8ac-122">If we need toorun a credit check because of hello amount of credit that you need, we send you a credit check application.</span></span> <span data-ttu-id="9a8ac-123">Hello kérelem elküldése után az hello hitelkérelemben 5-7 napos tooprocess is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="9a8ac-123">After you submit hello application, hello credit application can take 5-7 days tooprocess.</span></span>

## <a name="need-help-contact-support"></a><span data-ttu-id="9a8ac-124">Segítség</span><span class="sxs-lookup"><span data-stu-id="9a8ac-124">Need help?</span></span> <span data-ttu-id="9a8ac-125">Forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="9a8ac-125">Contact support.</span></span>

<span data-ttu-id="9a8ac-126">Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma megoldódik gyorsan tooget.</span><span class="sxs-lookup"><span data-stu-id="9a8ac-126">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget your issue resolved quickly.</span></span>
