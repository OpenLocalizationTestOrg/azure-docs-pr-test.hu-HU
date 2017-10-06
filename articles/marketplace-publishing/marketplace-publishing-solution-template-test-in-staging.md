---
title: "a megoldássablon hello Piactéri ajánlat aaaTesting |} Microsoft Docs"
description: "Ismerje meg, hogyan tootest a megoldássablon ajánlat hello Azure piactéren."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: ef8f9b5e-b98c-49f3-913f-cdf772c14c12
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/04/2015
ms.author: hascipio; v-divte
ms.openlocfilehash: 9c195c465d2fc6aa349e4bbcc348e5325f32850d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="test-your-solution-template-offer-in-staging"></a><span data-ttu-id="c573d-103">A megoldás sablon ajánlat tesztelése az átmeneti</span><span class="sxs-lookup"><span data-stu-id="c573d-103">Test your solution template offer in staging</span></span>
<span data-ttu-id="c573d-104">Átmeneti azt jelenti, hogy az ajánlatot "védőfal", ahol tesztelése és funkciókat ellenőrzése előtt az tooproduction privát telepítése.</span><span class="sxs-lookup"><span data-stu-id="c573d-104">Staging means deploying your offer in a private "sandbox" where you can test and verify its functionality before pushing it tooproduction.</span></span> <span data-ttu-id="c573d-105">hello ajánlat tooa felhasználói, akik már telepítették volna átmeneti jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="c573d-105">hello offer appears in staging just as it would tooa customer who has deployed it.</span></span> <span data-ttu-id="c573d-106">Az ajánlat leküldött hitelesített toobe toostaging kell lennie.</span><span class="sxs-lookup"><span data-stu-id="c573d-106">Your offer must be certified toobe pushed toostaging.</span></span>

<span data-ttu-id="c573d-107">Miután előkészített hello ajánlat, megtekintheti és teszteléséhez hello hello ajánlat [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c573d-107">After hello offer is staged, you can view and test hello offer in hello [Azure Portal](https://portal.azure.com/).</span></span>

<span data-ttu-id="c573d-108">Kövesse az alábbi toopush hello lépéseket az ajánlat toostaging és tesztelheti a hello [Azure Portal](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="c573d-108">Follow hello steps below toopush your offer toostaging and test it in hello [Azure Portal](https://portal.azure.com/):</span></span>

1. <span data-ttu-id="c573d-109">Nyissa meg toohello [közzétételi Portáljára](https://publish.windowsazure.com) > **Solution Templates** lapon > ajánlatát > **közzététel** > **tooStaging leküldéses** .</span><span class="sxs-lookup"><span data-stu-id="c573d-109">Go toohello [Publishing Portal](https://publish.windowsazure.com) > **Solution Templates** tab > your offer > **Publish** > **Push tooStaging**.</span></span>
2. <span data-ttu-id="c573d-110">Adja meg Azure-előfizetések hello listája, fog használni toopreview, és tesztelje az ajánlatot.</span><span class="sxs-lookup"><span data-stu-id="c573d-110">Provide hello list of Azure subscriptions that you will use toopreview and test your offer.</span></span>
3. <span data-ttu-id="c573d-111">Jelentkezzen be Azure betekintő portál toohello hello előző lépésben használt hello előfizetés-azonosító használatával.</span><span class="sxs-lookup"><span data-stu-id="c573d-111">Sign in toohello Azure preview portal by using hello subscription ID that you used in hello previous step.</span></span>
4. <span data-ttu-id="c573d-112">Legalább egy ciklikus hello Azure betekintő portál alábbi hello pontokon vizsgálati végrehajtására:</span><span class="sxs-lookup"><span data-stu-id="c573d-112">Carry out at least one round of testing in hello Azure preview portal on hello points mentioned below:</span></span>
   * <span data-ttu-id="c573d-113">Győződjön meg arról, hogy tartalom marketing jeleníti meg helyesen hello Azure piactéren.</span><span class="sxs-lookup"><span data-stu-id="c573d-113">Make sure that marketing content shows up correctly in hello Azure Marketplace.</span></span>
   * <span data-ttu-id="c573d-114">Végpontok közötti telepítés hello topológia.</span><span class="sxs-lookup"><span data-stu-id="c573d-114">End-to-end deployment of hello topology.</span></span>
   * <span data-ttu-id="c573d-115">Hajtsa végre a teljesítmény tesztelése, és magas terhelés tesztelése.</span><span class="sxs-lookup"><span data-stu-id="c573d-115">Perform performance testing and stress testing.</span></span>
   * <span data-ttu-id="c573d-116">Győződjön meg arról, hogy a topológia megfelelő toohello ajánlott eljárások.</span><span class="sxs-lookup"><span data-stu-id="c573d-116">Ensure that your topology adheres toohello best practices.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c573d-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c573d-117">Next steps</span></span>
<span data-ttu-id="c573d-118">Ha elégedett hello eredmények, majd folytathatja toohello végső ajánlat közzététele fázisban **4. lépés**: [központi telepítése a ajánlat toohello piactér](marketplace-publishing-push-to-production.md).</span><span class="sxs-lookup"><span data-stu-id="c573d-118">If you are satisfied with hello results, then you can proceed toohello final offer publishing phase, **Step 4**:  [Deploying your offer toohello Marketplace](marketplace-publishing-push-to-production.md).</span></span> <span data-ttu-id="c573d-119">Ellenkező esetben módosítsa az ajánlatot, és kérjen tanúsítási újra.</span><span class="sxs-lookup"><span data-stu-id="c573d-119">Otherwise, make changes in your offer and request certification again.</span></span>

> [!NOTE]
> <span data-ttu-id="c573d-120">Marketing a tartalmi változások, a hitelesítésszolgáltató szükség.</span><span class="sxs-lookup"><span data-stu-id="c573d-120">For marketing content changes, certification is not required.</span></span>
> 
> 

<span data-ttu-id="c573d-121">Lásd: [első lépések: hogyan toopublish egy ajánlat toohello Azure piactér](marketplace-publishing-getting-started.md) az útmutató tooall publisher feladatok számára.</span><span class="sxs-lookup"><span data-stu-id="c573d-121">See [Getting started: How toopublish an offer toohello Azure Marketplace](marketplace-publishing-getting-started.md) for a guide tooall publisher tasks.</span></span>

