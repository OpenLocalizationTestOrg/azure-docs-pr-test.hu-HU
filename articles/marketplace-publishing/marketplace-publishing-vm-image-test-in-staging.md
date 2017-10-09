---
title: "a virtuális gép hello Piactéri ajánlat aaaTest |} Microsoft Docs"
description: "Ismerje meg, hogyan tootest a virtuális gép rendszerképet az Azure piactér hello."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 7a41c3c6-625c-4478-b804-e124dee89040
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2016
ms.author: hascipio
ms.openlocfilehash: ab166d2c3c536810a3a8f48330f0482b9b4e58d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="test-your-vm-offer-for-hello-azure-marketplace-in-staging"></a><span data-ttu-id="a1521-103">A Virtuálisgép-ajánlat a hello Azure piactér tesztelése az átmeneti</span><span class="sxs-lookup"><span data-stu-id="a1521-103">Test your VM offer for hello Azure Marketplace in staging</span></span>
<span data-ttu-id="a1521-104">A Termékváltozat "védőfal", ahol tesztelése és ellenőrzése a működését toohello piactér üzembe helyezése előtt privát telepítése azt jelenti, hogy átmeneti.</span><span class="sxs-lookup"><span data-stu-id="a1521-104">Staging means deploying your SKU in a private “sandbox” where you can test and validate its functionality before deploying it toohello Marketplace.</span></span> <span data-ttu-id="a1521-105">hello SKU tooa felhasználói, akik már telepítették volna átmeneti jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a1521-105">hello SKU appears in staging just as it would tooa customer who has deployed it.</span></span> <span data-ttu-id="a1521-106">A Virtuálisgép-lemezkép leküldött hitelesített toobe toostaging kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a1521-106">Your VM image must be certified toobe pushed toostaging.</span></span>

## <a name="step-1-push-your-offer-toostaging"></a><span data-ttu-id="a1521-107">1. lépés: Az ajánlat toostaging leküldéses</span><span class="sxs-lookup"><span data-stu-id="a1521-107">Step 1: Push your offer toostaging</span></span>
1. <span data-ttu-id="a1521-108">A hello **közzététel** lapra, majd **tooStaging leküldéses**.</span><span class="sxs-lookup"><span data-stu-id="a1521-108">On hello **Publish** tab, click **Push tooStaging**.</span></span>
   
    ![rajz](media/marketplace-publishing-vm-image-test-in-staging/vm-image-push-to-staging.png)
2. <span data-ttu-id="a1521-110">Ha hello közzétételi portál értesíti az esetleges hibákat, javítsa ki őket.</span><span class="sxs-lookup"><span data-stu-id="a1521-110">If hello Publishing Portal notifies you of any errors, correct them.</span></span>
3. <span data-ttu-id="a1521-111">A hello **ki férhet hozzá az előkészített ajánlatot?** párbeszédpanelen adja meg, hogy szüksége lesz toopreview ajánlatát hello Azure-előfizetések listája hello [Azure betekintő portál](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a1521-111">In hello **Who can access your staged offer?** dialog box, enter hello list of Azure subscriptions that you will use toopreview your offer in hello [Azure preview portal](https://portal.azure.com).</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="a1521-112">Virtuális gépek esetén és a megoldás sablonok adjon **nem** típusú kriptográfiai Szolgáltató, DreamSpark- vagy Azure in Open licencprogram engedélyezett előfizetések.</span><span class="sxs-lookup"><span data-stu-id="a1521-112">In case of Virtual Machines and Solution templates, please **do not** whitelist subscriptions of type CSP, DreamSpark or Azure in Open.</span></span>
   > 
   > 

    > <span data-ttu-id="a1521-113">Virtuális gépek, a hello gombra kattintva esetén **LEKÜLDÉSES tooSTAGING**, a lépéseket követve hello mögött hello helyszín hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="a1521-113">In case of Virtual Machines, when you click on hello button **PUSH tooSTAGING**, hello following steps are performed behind hello scene.</span></span> <span data-ttu-id="a1521-114">Képes tooview hello folyamat minden egyes lépést hello közzététel lap hello fogja portal közzététele.</span><span class="sxs-lookup"><span data-stu-id="a1521-114">You will be able tooview hello progress of each step under hello PUBLISH tab in hello Publishing portal.</span></span> <span data-ttu-id="a1521-115">Ellenőrizze ezen a lapon rendszeres időközönként (amíg hello állapotánál ELŐKÉSZÍTETT) javítása a átfogóan igénylő hiba információkat.</span><span class="sxs-lookup"><span data-stu-id="a1521-115">You must check this page at regular interval (until hello status shows STAGED) for any failure information which need correction from your end.</span></span>

    > - <span data-ttu-id="a1521-116">Először az átmeneti kéréseket, amelyekre toohello hitelesítő csoport hello vhd ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="a1521-116">At first your staging request goes toohello certification team who validate hello vhd.</span></span> <span data-ttu-id="a1521-117">Azonban ha a kérés rendelkezik kapott módosítása csak marketing, majd hello hitelesítő a lépés kimarad.</span><span class="sxs-lookup"><span data-stu-id="a1521-117">However, if your request has got only marketing change, then hello certification step is skipped.</span></span>
    > - <span data-ttu-id="a1521-118">Hello hitelesítő végrehajtása után a replikáció hello ajánlat indításának összes hello Azure adatközpontjaiban.</span><span class="sxs-lookup"><span data-stu-id="a1521-118">Once hello certification is complete, replication of hello offer start across all hello Azure datacenters.</span></span> <span data-ttu-id="a1521-119">Általában 24-48hours hello replikációs toocomplete az szükséges, de tooa hét attól függően, hogy hello virtuális merevlemez méretére hello is tarthat.</span><span class="sxs-lookup"><span data-stu-id="a1521-119">It generally takes 24-48hours for hello replication toocomplete but may take up tooa week depending on hello size of hello vhd.</span></span> <span data-ttu-id="a1521-120">Ha a kérés rendelkezik kapott módosítása csak marketing, majd hello replikációs azonban gyorsabb.</span><span class="sxs-lookup"><span data-stu-id="a1521-120">However, if your request has got only marketing change, then hello replication is faster.</span></span>
    > - <span data-ttu-id="a1521-121">Ha hello replikáció befejeződött, majd hello ajánlat rendelkezésre áll a hello [Azure-portálon](http:/portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a1521-121">When hello replication is complete, then hello offer will be available in hello [Azure portal](http:/portal.azure.com).</span></span> <span data-ttu-id="a1521-122">Adott idő hello: állapota lesz ELŐKÉSZÍTETT hello a portál közzététele.</span><span class="sxs-lookup"><span data-stu-id="a1521-122">At that time hello status become STAGED in hello Publishing portal.</span></span> <span data-ttu-id="a1521-123">Egy előkészített ajánlat is elérhetővé válik a hello [Azure-portálon](http:/portal.azure.com) csak a mely hello az ajánlat elő van készítve hello előfizetéshez társított hello e-mail azonosítót.</span><span class="sxs-lookup"><span data-stu-id="a1521-123">A staged offer is visible in hello [Azure portal](http:/portal.azure.com) only using hello email id(s) associated with hello subscription with which hello offer is staged.</span></span>

1. <span data-ttu-id="a1521-124">Jelentkezzen be toohello [Azure betekintő portál](https://portal.azure.com) hello segítségével az Azure-előfizetések hello előző lépésben felsorolt.</span><span class="sxs-lookup"><span data-stu-id="a1521-124">Sign in toohello [Azure preview portal](https://portal.azure.com) by using one of hello Azure subscriptions listed in hello previous step.</span></span>
2. <span data-ttu-id="a1521-125">Keresse meg az ajánlatot, és ellenőrizze a VM-lemezkép pontok:</span><span class="sxs-lookup"><span data-stu-id="a1521-125">Find your offer and validate your VM image points:</span></span>
   
   * <span data-ttu-id="a1521-126">Győződjön meg arról, hogy tartalom marketing jeleníti meg helyesen hello piactéren.</span><span class="sxs-lookup"><span data-stu-id="a1521-126">Make sure that marketing content shows up correctly in hello Marketplace.</span></span>
   * <span data-ttu-id="a1521-127">Végpontok közötti hello Virtuálisgép-lemezkép központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="a1521-127">End-to-end deployment of hello VM image.</span></span>
     
      ![img-térkép-portál](media/marketplace-publishing-push-to-staging/pubportal-mapping-azure-portal.jpg)

> [!IMPORTANT]
> <span data-ttu-id="a1521-129">Az ajánlatot marad, amíg tájékoztatja a Microsoftot hello közzétételi portálon keresztül átmeneti [**közzététel** lapon > hello gombra kattintva **"Jóváhagyási tooPush tooProduction lekérő"**] készen toopush abban tooproduction.</span><span class="sxs-lookup"><span data-stu-id="a1521-129">Your offer will remain in staging until you notify Microsoft via hello Publishing Portal [**Publish** tab > click on hello button **"Request Approval tooPush tooProduction"**] that you are ready toopush tooproduction.</span></span> <span data-ttu-id="a1521-130">Ez az az ideális idő toohave keresztül minden felsorolt fog ajánlatát előkészítésekor ellenőrizze a csoport összes tagja.</span><span class="sxs-lookup"><span data-stu-id="a1521-130">This is an ideal time toohave all members of your team check over everything in preparation for your offer going listed.</span></span>
> 
> <span data-ttu-id="a1521-131">átmeneti platform hello tesztelési hello ajánlat egy előnézeti módban készült hello közzétevő.</span><span class="sxs-lookup"><span data-stu-id="a1521-131">hello staging platform is designed for testing hello offer in a preview mode by hello publisher.</span></span> <span data-ttu-id="a1521-132">Kifejezetten nem ajánlott a platofrm kereskedelmi célra használja.</span><span class="sxs-lookup"><span data-stu-id="a1521-132">We strongly discourage using this platofrm for commerical purposes.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="a1521-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a1521-133">Next steps</span></span>
<span data-ttu-id="a1521-134">Az ajánlatot "előkészített", és amelyeket tesztelt, funkciókat, és a tartalom marketing, folytathatja a toohello végső közzététel fázisban **4. lépés**: [központi telepítése a ajánlat toohello piactér](marketplace-publishing-push-to-production.md).</span><span class="sxs-lookup"><span data-stu-id="a1521-134">Now that your offer is "staged" and you have tested its functionality and marketing content, you can proceed toohello final publishing phase, **Step 4**: [Deploying your offer toohello Marketplace](marketplace-publishing-push-to-production.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="a1521-135">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="a1521-135">See also</span></span>
* [<span data-ttu-id="a1521-136">Első lépések: hogyan toopublish egy ajánlat toohello Azure piactéren</span><span class="sxs-lookup"><span data-stu-id="a1521-136">Getting started: How toopublish an offer toohello Azure Marketplace</span></span>](marketplace-publishing-getting-started.md)

