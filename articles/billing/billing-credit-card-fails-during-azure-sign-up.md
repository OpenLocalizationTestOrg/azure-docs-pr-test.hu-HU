---
title: "Be Azure kukac elutasítva hitelkártya |} Microsoft Docs"
description: "Útmutató: regisztráció az Azure megkísérlésekor a rendszer elutasította a vagy követel kártyáját kapcsolatos problémák megoldásához."
services: 
documentationcenter: 
author: JiangChen79
manager: adpick
editor: 
tags: billing,top-support-issue
keywords: "elutasított hitelkártya elutasítva, bankkártyával a hitelkártya vissza lett utasítva, nem fogadják el a hitelkártya"
ms.assetid: 407ef3db-2a64-4a04-a08f-7d1d1c4860c7
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: cjiang
ms.openlocfilehash: bad37f2447ac8de727326914b611f81effc9cf3f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="your-debit-card-or-credit-card-is-declined-at-azure-sign-up"></a><span data-ttu-id="93560-104">A bankkártyával vagy hitelkártya a rendszer elutasította, Azure-előfizetési:</span><span class="sxs-lookup"><span data-stu-id="93560-104">Your debit card or credit card is declined at Azure sign-up</span></span>
<span data-ttu-id="93560-105">Ha a bankkártyával hitelkártya elutasította, vagy nem fogadja el az Azure-beli regisztráció során, akkor előfordulhat, hogy lehet elérhető az alábbi problémák egyike:</span><span class="sxs-lookup"><span data-stu-id="93560-105">If your debit card or credit card is declined or not accepted when you sign up for Azure, you might be facing one of the following issues:</span></span>

* <span data-ttu-id="93560-106">**A hitelkártya vagy tartozik kártya szolgáltatója nem fogadja el a rendszer az országot.**</span><span class="sxs-lookup"><span data-stu-id="93560-106">**The credit card or debit card provider is not accepted for your country.**</span></span> <span data-ttu-id="93560-107">Ha úgy dönt, a kártya, csak akkor jelenik meg a beállításokat, amelyek érvényesek a választott ország.</span><span class="sxs-lookup"><span data-stu-id="93560-107">When you choose a card, you only see the options that are valid in the country that you select.</span></span> <span data-ttu-id="93560-108">Lépjen kapcsolatba a bank vagy kártya kiállítói annak ellenőrzéséhez, hogy a hitelkártya nemzetközi tranzakciók engedélyezve van-e.</span><span class="sxs-lookup"><span data-stu-id="93560-108">Contact your bank or card issuer to confirm that your credit card is enabled for international transactions.</span></span> <span data-ttu-id="93560-109">További részletekért lásd a támogatott országokból és pénznemek [Azure beszerzési gyakran ismételt kérdések](https://azure.microsoft.com/pricing/faq/).</span><span class="sxs-lookup"><span data-stu-id="93560-109">See supported countries and currencies in [Azure Purchase FAQ](https://azure.microsoft.com/pricing/faq/).</span></span>
* <span data-ttu-id="93560-110">**A vagy követel kártyájának információi hibás vagy hiányos.**</span><span class="sxs-lookup"><span data-stu-id="93560-110">**Your credit or debit card information is inaccurate or incomplete.**</span></span> <span data-ttu-id="93560-111">A nevét, címét és CVV-kód beírása egyeznie kell pontosan nyomtatandó a kártya.</span><span class="sxs-lookup"><span data-stu-id="93560-111">The name, address, and CVV code you enter must match exactly what’s printed on the card.</span></span>
* <span data-ttu-id="93560-112">**A virtuális vagy előre kártya használata.**</span><span class="sxs-lookup"><span data-stu-id="93560-112">**You're using a virtual or prepaid card.**</span></span> <span data-ttu-id="93560-113">Az Azure-előfizetések fizetési virtuális vagy előre vagy követel kártyák nem elfogadott.</span><span class="sxs-lookup"><span data-stu-id="93560-113">Virtual or prepaid credit or debit cards aren't accepted as payment for Azure subscriptions.</span></span>
* <span data-ttu-id="93560-114">**A kártya egy inaktív vagy letiltott.**</span><span class="sxs-lookup"><span data-stu-id="93560-114">**The card is inactive or blocked.**</span></span> <span data-ttu-id="93560-115">Lépjen kapcsolatba a bank annak biztosítása érdekében, a kártya aktív.</span><span class="sxs-lookup"><span data-stu-id="93560-115">Contact your bank to ensure your card is active.</span></span>
* <span data-ttu-id="93560-116">**Egyéb regisztrációs problémák akadtak** megismeréséhez [hibáinak elhárítása az Azure-előfizetési](billing-troubleshoot-azure-sign-up-issues.md).</span><span class="sxs-lookup"><span data-stu-id="93560-116">**You may be experiencing other sign-up issues** Check out how to [troubleshoot Azure sign-up](billing-troubleshoot-azure-sign-up-issues.md).</span></span>

<span data-ttu-id="93560-117">A fizetési adatok frissítése után próbálja meg újra regisztrál.</span><span class="sxs-lookup"><span data-stu-id="93560-117">After you update your payment information, try signing up again.</span></span>

## <a name="representing-a-business-that-doesnt-want-to-pay-by-card-set-up-invoicing"></a><span data-ttu-id="93560-118">Jelző, amely nem érdemes fizetni kártya által üzleti?</span><span class="sxs-lookup"><span data-stu-id="93560-118">Representing a business that doesn't want to pay by card?</span></span> <span data-ttu-id="93560-119">Adja meg a számlázás</span><span class="sxs-lookup"><span data-stu-id="93560-119">Set up invoicing</span></span>
<span data-ttu-id="93560-120">Ha egy üzleti kijelenti, is kell fizetnie az Azure-előfizetéshez a számla fizetési módok ellenőrzések, a egynapos ellenőrzéseket vagy a hálózati átvitel érdekében.</span><span class="sxs-lookup"><span data-stu-id="93560-120">If you represent a business, you can pay for your Azure subscription with invoice payment methods like checks, overnight checks, or wire transfers.</span></span> <span data-ttu-id="93560-121">Nem módosítható egy másik fizetési lehetőség, nagy számlán még beállítása után.</span><span class="sxs-lookup"><span data-stu-id="93560-121">You can’t change to another payment option after you’re set up to pay by invoice.</span></span> <span data-ttu-id="93560-122">Nagy számlán, lásd: [Azure számlázási - számlázási útmutató](https://azure.microsoft.com/pricing/invoicing/).</span><span class="sxs-lookup"><span data-stu-id="93560-122">To pay by invoice, see [Azure Billing - How to invoice](https://azure.microsoft.com/pricing/invoicing/).</span></span>

## <a name="update-your-credit-card-or-debit-card-information"></a><span data-ttu-id="93560-123">A hitelkártya vagy tartozik kártya adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="93560-123">Update your credit card or debit card information</span></span>
<span data-ttu-id="93560-124">A regisztrációt követően, [a fizetési adatok kezelése](billing-how-to-change-credit-card.md) módosítani vagy eltávolítani egy kártyát.</span><span class="sxs-lookup"><span data-stu-id="93560-124">After you sign up, [manage your payment information](billing-how-to-change-credit-card.md) to change or remove a card.</span></span>

## <a name="need-more-help-contact-support"></a><span data-ttu-id="93560-125">További segítségre van szüksége?</span><span class="sxs-lookup"><span data-stu-id="93560-125">Need more help?</span></span> <span data-ttu-id="93560-126">Forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="93560-126">Contact support.</span></span>
<span data-ttu-id="93560-127">Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma elhárítva gyors eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="93560-127">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to get your issue resolved quickly.</span></span>
