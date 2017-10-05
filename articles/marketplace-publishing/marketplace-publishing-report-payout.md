---
title: "Az Azure piactér kifizetés reporting megértése |} Microsoft Docs"
description: "Ismerje meg, hogyan lehet áttekinteni és az Azure piactér kifizetés jelentés betöltési."
services: marketplace-publishing
documentationcenter: na
author: v-jeana
manager: lakoch
editor: 
ms.assetid: 3e99aefe-abeb-414c-8689-15352d25aefd
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: v-jeana; hascipio; v-dabosl
ms.openlocfilehash: 5a89e9ba4376d0c4f49feb3783692e28a28902a2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="understand-your-azure-marketplace-payout-reports"></a><span data-ttu-id="ccd81-103">Az Azure piactér kifizetés jelentések ismertetése</span><span class="sxs-lookup"><span data-stu-id="ccd81-103">Understand your Azure Marketplace payout reports</span></span>
## <a name="access-and-view-your-payout-reports"></a><span data-ttu-id="ccd81-104">Hozzáférés és a kifizetés jelentések megtekintése</span><span class="sxs-lookup"><span data-stu-id="ccd81-104">Access and view your payout reports</span></span>
<span data-ttu-id="ccd81-105">Amíg a Microsoft fejlesztői központban átmenet néhány kifizetés jelentések érhetők el a Dev Center webhely https://dev.windows.com/en-us, míg mások is találhatók meg a közzétételi Portáljára https://publish.windowsazure.com.</span><span class="sxs-lookup"><span data-stu-id="ccd81-105">While we transition to Dev Center some of your payout reports may be available in the Dev Center at https://dev.windows.com/en-us while others may still be found in Publishing Portal at https://publish.windowsazure.com.</span></span>

<span data-ttu-id="ccd81-106">Kifizetés reporting most rendelkezésre áll a **Dev Center webhely** bármely piactér ajánlatokról társított modern payouts; ez jelenleg tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="ccd81-106">Payout reporting will now be available in **Dev Center** for any Marketplace offerings that are associated with modern payouts; this currently includes:</span></span>

* <span data-ttu-id="ccd81-107">Virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="ccd81-107">VMs</span></span>
* <span data-ttu-id="ccd81-108">B + C ajánlatok</span><span class="sxs-lookup"><span data-stu-id="ccd81-108">B+C offers</span></span>
* <span data-ttu-id="ccd81-109">Adatok és a fejlesztői szolgáltatások kínált EA alatt</span><span class="sxs-lookup"><span data-stu-id="ccd81-109">Data and Dev Services offered under EA</span></span>

<span data-ttu-id="ccd81-110">Kifizetés reporting marad a **közzétételi Portáljára** számára:</span><span class="sxs-lookup"><span data-stu-id="ccd81-110">Payout reporting will still be in **Publishing Portal** for:</span></span>

* <span data-ttu-id="ccd81-111">Adatok és a fejlesztői szolgáltatások kínált webes közvetlen (amely a továbbra is a hagyományos kifizetés rendszer) alatt.</span><span class="sxs-lookup"><span data-stu-id="ccd81-111">Data and Dev Services offered under Web Direct (which still uses the legacy payout system).</span></span>

<span data-ttu-id="ccd81-112">Jelentések negyedév lezárása után rendelkezésre álló 45 nap, és bármely visszatérítés kell számítani.</span><span class="sxs-lookup"><span data-stu-id="ccd81-112">Reports are available 45 days after the close of the quarter and are calculated after any refunds.</span></span>

### <a name="access-payout-reports-in-dev-center"></a><span data-ttu-id="ccd81-113">A fejlesztői központjában Access kifizetés jelentések</span><span class="sxs-lookup"><span data-stu-id="ccd81-113">Access payout reports in Dev Center</span></span>
1. <span data-ttu-id="ccd81-114">Https://dev.windows.com/en-us, navigáljon a fejlesztői központban.</span><span class="sxs-lookup"><span data-stu-id="ccd81-114">Navigate to Dev Center at https://dev.windows.com/en-us.</span></span>
2. <span data-ttu-id="ccd81-115">Kattintson a **irányítópult**.</span><span class="sxs-lookup"><span data-stu-id="ccd81-115">Click **Dashboard**.</span></span>

    ![LandingPageDashboardHighlight][1]
3. <span data-ttu-id="ccd81-117">Kattintson a **kifizetés összegzés**.</span><span class="sxs-lookup"><span data-stu-id="ccd81-117">Click **Payout Summary**.</span></span>

    ![DashboardPayoutSummary][2]

## <a name="view-your-payout-reports-in-dev-center"></a><span data-ttu-id="ccd81-119">A fejlesztői központjában kifizetés jelentések megtekintése</span><span class="sxs-lookup"><span data-stu-id="ccd81-119">View your payout reports in Dev Center</span></span>
<span data-ttu-id="ccd81-120">A negyedév kifizetés jelentés negyedévben történt összes tranzakció rögzíti.</span><span class="sxs-lookup"><span data-stu-id="ccd81-120">The payout report for each quarter records all transactions that occurred within that quarter.</span></span>

* <span data-ttu-id="ccd81-121">A fenntartott mennyiség azt jelzi, hogy a jövőbeli fizetési ciklus (pl. ezt a mennyiséget helyezi át a következő hónap jövőbeli fizetési) kívül van keletkezhetnek fizetési.</span><span class="sxs-lookup"><span data-stu-id="ccd81-121">The Reserved amount indicates any payments that are accruing outside of the upcoming payment cycle (e.g. this amount will move to upcoming payment the following month).</span></span>  <span data-ttu-id="ccd81-122">Ez a mennyiség általában $0 lesz (kivéve, ha az ügyfél és az előzetes fizet).</span><span class="sxs-lookup"><span data-stu-id="ccd81-122">This amount will typically be $0 (unless a customer pays well in advance).</span></span>
* <span data-ttu-id="ccd81-123">A közeljövőben fizetési vagy a legutóbbi fizetési **részleteinek megtekintéséhez** hivatkozásokra, hogy ezek payouts megjegyzést.</span><span class="sxs-lookup"><span data-stu-id="ccd81-123">Click on the Upcoming payment or Most recent payment **View details** links to see a note about those payouts.</span></span>
* <span data-ttu-id="ccd81-124">Kattintson a **fizetési utasítások** alkalmazás vagy termék a hibákra vonatkozó részletes adatainak megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="ccd81-124">Click on **Payment Statements** to view the details under proceeds by app/product.</span></span>
* <span data-ttu-id="ccd81-125">Kattintson a **nézet** hivatkozásra kattintva megtekintheti az egyes utasításokat.</span><span class="sxs-lookup"><span data-stu-id="ccd81-125">Click on the **View** link to see individual statements.</span></span>

    ![PayoutSummaryUpcomingMostRecentLinksStatement][3]
* <span data-ttu-id="ccd81-127">Használja a **halad lebontása** szűrő az egyes utasítás több apps/termék megtekintéséhez, ha vannak ilyenek alján.</span><span class="sxs-lookup"><span data-stu-id="ccd81-127">Use the **Proceeds Breakdown** filter at the bottom of the individual statement to view multiple apps/products if they exist.</span></span>

    ![PayoutSummaryPaymentStatementsFilterControl][4]

## <a name="view-your-payout-reports-in-publishing-portal"></a><span data-ttu-id="ccd81-129">A közzétételi Portáljára a kifizetés jelentések megtekintése</span><span class="sxs-lookup"><span data-stu-id="ccd81-129">View your payout reports in Publishing Portal</span></span>
<span data-ttu-id="ccd81-130">A negyedév kifizetés jelentés negyedévben történt összes tranzakció rögzíti.</span><span class="sxs-lookup"><span data-stu-id="ccd81-130">The payout report for each quarter records all transactions that occurred within that quarter.</span></span>

1. <span data-ttu-id="ccd81-131">Keresse meg a közzétételi portálon, a https://publish.windowsazure.com.</span><span class="sxs-lookup"><span data-stu-id="ccd81-131">Navigate to the publishing portal at https://publish.windowsazure.com.</span></span>
2. <span data-ttu-id="ccd81-132">Az a **közzétevők** kattintson **kifizetés jelentések**.</span><span class="sxs-lookup"><span data-stu-id="ccd81-132">From the **Publishers** section, click **Payout Reports**.</span></span>
3. <span data-ttu-id="ccd81-133">Kattintson a legördülő listán a negyedéves kifizetés összes elérhető jelentések megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="ccd81-133">Click the drop-down to display all available quarterly payout reports.</span></span>

    ![accessingpayoutreport][5]

### <a name="read-your-payout-reports"></a><span data-ttu-id="ccd81-135">Kifizetés jelentések olvasása</span><span class="sxs-lookup"><span data-stu-id="ccd81-135">Read your payout reports</span></span>
<span data-ttu-id="ccd81-136">A negyedév kifizetés jelentés negyedévben történt összes tranzakció rögzíti.</span><span class="sxs-lookup"><span data-stu-id="ccd81-136">The payout report for each quarter records all transactions that occurred within that quarter.</span></span>

* <span data-ttu-id="ccd81-137">Ha egy adott negyedév kapcsolódó tételek keres, válassza ki a kifizetés jelentést, hogy a legördülő lista a negyedév.</span><span class="sxs-lookup"><span data-stu-id="ccd81-137">If you are looking for ledger entries that relate to a particular quarter, select the payout report for that quarter from the drop-down.</span></span> <span data-ttu-id="ccd81-138">Például ha érdekli tételek a 2015. június áprilisban, választhat adott dátumtartományon belül a legördülő listán.</span><span class="sxs-lookup"><span data-stu-id="ccd81-138">For example, if you are interested in ledger entries for April to June 2015, select that date range from the drop-down.</span></span>
* <span data-ttu-id="ccd81-139">Ha egy adott negyedév kapcsolódó payouts részleteit keres, válassza ki a kifizetés jelentést az ezt követő negyedév.</span><span class="sxs-lookup"><span data-stu-id="ccd81-139">If you are looking for details of payouts that relate to a particular quarter, select the payout report for the subsequent quarter.</span></span> <span data-ttu-id="ccd81-140">Például ha érdekli a payouts a 2015. június áprilisban, ezek az összegek megjelennek a későbbi kifizetés jelentés júliusi a 2015. szeptember.</span><span class="sxs-lookup"><span data-stu-id="ccd81-140">For example, if you are interested in the payouts for April to June 2015, these amounts will appear in the subsequent payout report for July to September 2015.</span></span>
  <span data-ttu-id="ccd81-141">![readingpayoutreport][6]</span><span class="sxs-lookup"><span data-stu-id="ccd81-141">![readingpayoutreport][6]</span></span>
* <span data-ttu-id="ccd81-142">A pénzügyi összefoglaló panelen látható kiegyensúlyozza, kreditek, tartozik kategória szerint.</span><span class="sxs-lookup"><span data-stu-id="ccd81-142">The financial summary panel shows balances, credits, and debits by category.</span></span>
* <span data-ttu-id="ccd81-143">Tételek megjelenítése az egyes tranzakciók.</span><span class="sxs-lookup"><span data-stu-id="ccd81-143">Ledger entries show individual transactions.</span></span>

## <a name="definitions"></a><span data-ttu-id="ccd81-144">Meghatározások</span><span class="sxs-lookup"><span data-stu-id="ccd81-144">Definitions</span></span>
<span data-ttu-id="ccd81-145">**Pénzügyi összefoglaló panel:**</span><span class="sxs-lookup"><span data-stu-id="ccd81-145">**Financial summary panel:**</span></span>

![financialdefinitions][7]

<span data-ttu-id="ccd81-147">**Tételek:**</span><span class="sxs-lookup"><span data-stu-id="ccd81-147">**Ledger entries:**</span></span>

![ledgerdefinitions][8]

## <a name="payout-questions"></a><span data-ttu-id="ccd81-149">Kifizetés kérdések</span><span class="sxs-lookup"><span data-stu-id="ccd81-149">Payout questions</span></span>
<span data-ttu-id="ccd81-150">Ha a payouts kapcsolatos kérdése van, forduljon a támogatási csapat.</span><span class="sxs-lookup"><span data-stu-id="ccd81-150">If you have a question related to your payouts, contact our support team.</span></span>

![payoutquestions][9]

1. <span data-ttu-id="ccd81-152">Nyissa meg a támogatási lapokat.</span><span class="sxs-lookup"><span data-stu-id="ccd81-152">Navigate to the support pages.</span></span>
2. <span data-ttu-id="ccd81-153">Válassza ki **Payouts**.</span><span class="sxs-lookup"><span data-stu-id="ccd81-153">Select **Payouts**.</span></span>
3. <span data-ttu-id="ccd81-154">Válassza ki **kifizetés kapcsolódó lekérdezések**.</span><span class="sxs-lookup"><span data-stu-id="ccd81-154">Select **Payout related inquiries**.</span></span>
4. <span data-ttu-id="ccd81-155">Kattintson a **indítási kérésre**.</span><span class="sxs-lookup"><span data-stu-id="ccd81-155">Click **Start request**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ccd81-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ccd81-156">Next steps</span></span>
<span data-ttu-id="ccd81-157">Más támogatási lekérdezések esetén jelentkezzen egy problémáját <https://portal.azure.com>.</span><span class="sxs-lookup"><span data-stu-id="ccd81-157">For other support queries, please log an issue at <https://portal.azure.com>.</span></span>

[1]: ./media/marketplace-publishing-report-payout/LandingPage-DashboardHighlight.png
[2]: ./media/marketplace-publishing-report-payout/Dashboard-PayoutSummary.png
[3]: ./media/marketplace-publishing-report-payout/PayoutSummary-UpcomingOrMostRecentPaymentLinksSingleStatementLink.png
[4]: ./media/marketplace-publishing-report-payout/PayoutSummary-PaymentStatements-SingleStatement-FilterControl.png
[5]: ./media/marketplace-publishing-report-payout/accessingpayoutreport.png
[6]: ./media/marketplace-publishing-report-payout/readingpayoutreport.png
[7]: ./media/marketplace-publishing-report-payout/financialdefinitions.png
[8]: ./media/marketplace-publishing-report-payout/ledgerdefinitions.png
[9]: ./media/marketplace-publishing-report-payout/payoutquestions.png
