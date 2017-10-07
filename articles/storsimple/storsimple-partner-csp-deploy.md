---
title: "aaaMicrosoft Azure StorSimple és a felhőalapú megoldások szolgáltató Program áttekintése |} Microsoft Docs"
description: "Áttekintése hello StorSimple és a CSP a StorSimple-partnerek számára."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/08/2017
ms.author: alkohli
ms.openlocfilehash: b5d999f2fbb9a27e7404ff454957b29dbef56af6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-storsimple-virtual-array-for-cloud-solution-provider-program"></a><span data-ttu-id="ac090-103">Cloud Solution Provider programot a StorSimple virtuális tömb telepíteni</span><span class="sxs-lookup"><span data-stu-id="ac090-103">Deploy StorSimple Virtual Array for Cloud Solution Provider Program</span></span>

## <a name="overview"></a><span data-ttu-id="ac090-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="ac090-104">Overview</span></span>

<span data-ttu-id="ac090-105">A StorSimple virtuális tömb hello Cloud Solution Provider (CSP) partnerek az ügyfelek számára telepíthető.</span><span class="sxs-lookup"><span data-stu-id="ac090-105">StorSimple Virtual Array can be deployed by hello Cloud Solution Provider (CSP) partners for their customers.</span></span> <span data-ttu-id="ac090-106">Kriptográfiai Szolgáltató partner hozhat létre a StorSimple eszköz Manager szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="ac090-106">A CSP partner can create a StorSimple Device Manager service.</span></span> <span data-ttu-id="ac090-107">Ez a szolgáltatás majd használt toodeploy kell és kezelheti a StorSimple virtuális tömb és hello tartozó megosztások, kötetek és biztonsági másolatokat.</span><span class="sxs-lookup"><span data-stu-id="ac090-107">This service can then be used toodeploy and manage StorSimple Virtual Array and hello associated shares, volumes, and backups.</span></span>

<span data-ttu-id="ac090-108">Ez a cikk ismerteti, hogyan CSP partner hozzáadása az ügyfél vagy egy új előfizetés tooan meglévő ügyfél és majd hozzon létre egy szolgáltatás toodeploy egy StorSimple virtuális tömb CSP-hez.</span><span class="sxs-lookup"><span data-stu-id="ac090-108">This article describes how a CSP partner can add a customer or a new subscription tooan existing customer and then create a service toodeploy a StorSimple Virtual Array in CSP.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ac090-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ac090-109">Prerequisites</span></span>

<span data-ttu-id="ac090-110">Mielőtt elkezdené, győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="ac090-110">Before you begin, ensure that:</span></span>

- <span data-ttu-id="ac090-111">A hello CSP program regisztrált.</span><span class="sxs-lookup"><span data-stu-id="ac090-111">You are enrolled under hello CSP program.</span></span>
- <span data-ttu-id="ac090-112">Rendelkezik érvényes [Partnerközpontjában](http://partnercenter.microsoft.com/) bejelentkezési hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="ac090-112">You have valid [Partner Center](http://partnercenter.microsoft.com/) login credentials.</span></span> <span data-ttu-id="ac090-113">hello hitelesítő adatok lehetővé teszik a toohello Partner portál tooadd új ügyfelek toosign, keresse meg az ügyfelek vagy tooa felhasználói fiók hello Partner irányítópultról lépjen.</span><span class="sxs-lookup"><span data-stu-id="ac090-113">hello credentials enable you toosign in toohello Partner portal tooadd new customers, search for customers, or navigate tooa customer account from hello Partner dashboard.</span></span> <span data-ttu-id="ac090-114">hello CSP működhessen, a StorSimple rendszergazda hello Azure-portálon a hello ügyfél nevében.</span><span class="sxs-lookup"><span data-stu-id="ac090-114">hello CSP can function as a StorSimple administrator on behalf of hello customer in hello Azure portal.</span></span>
                             
## <a name="add-a-customer"></a><span data-ttu-id="ac090-115">Az ügyfél hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ac090-115">Add a customer</span></span>

<span data-ttu-id="ac090-116">Ha ad hozzá, az ügyfél, a rendszer automatikusan létrehoz egy előfizetést.</span><span class="sxs-lookup"><span data-stu-id="ac090-116">If you add a customer, a subscription is automatically created.</span></span> <span data-ttu-id="ac090-117">az ügyfél tooadd (és automatikusan az előfizetés létrehozása), hajtsa végre a következő lépéseket hello partnerportálon hello.</span><span class="sxs-lookup"><span data-stu-id="ac090-117">tooadd a customer (and automatically create a subscription), perform hello following steps in hello Partner portal.</span></span>

1. <span data-ttu-id="ac090-118">Nyissa meg toohello [Partnerközpontjában](http://partnercenter.microsoft.com/) , jelentkezzen be a kriptográfiai Szolgáltató hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="ac090-118">Go toohello [Partner Center](http://partnercenter.microsoft.com/) and sign in using your CSP credentials.</span></span> <span data-ttu-id="ac090-119">Kattintson a **irányítópult**.</span><span class="sxs-lookup"><span data-stu-id="ac090-119">Click **Dashboard**.</span></span>

     ![A Partner Center irányítópult](./media/storsimple-partner-csp-deploy/image1.png)
                              
2. <span data-ttu-id="ac090-121">A hello bal oldali ablaktáblában kattintson **ügyfelek**.</span><span class="sxs-lookup"><span data-stu-id="ac090-121">In hello left-pane, click **Customers**.</span></span> <span data-ttu-id="ac090-122">Hello jobb oldali ablaktáblában kattintson **adja hozzá az ügyfelek**.</span><span class="sxs-lookup"><span data-stu-id="ac090-122">In hello right-pane, click **Add customers**.</span></span> <span data-ttu-id="ac090-123">Írja be a hello ügyfél hello adatait.</span><span class="sxs-lookup"><span data-stu-id="ac090-123">Enter hello details of hello customer.</span></span> <span data-ttu-id="ac090-124">Kattintson a **tovább: előfizetések** toocreate egy ügyfél-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="ac090-124">Click **Next: Subscriptions** toocreate a customer subscription.</span></span>

    ![Ügyfél hozzáadása](./media/storsimple-partner-csp-deploy/image2.png)

3.  <span data-ttu-id="ac090-126">Válassza ki **Microsoft Azure** kínálnak.</span><span class="sxs-lookup"><span data-stu-id="ac090-126">Select **Microsoft Azure** offer.</span></span> <span data-ttu-id="ac090-127">Görgessen toohello alsó hello lap, és kattintson a **felülvizsgálati**.</span><span class="sxs-lookup"><span data-stu-id="ac090-127">Scroll toohello bottom of hello page and click **Review**.</span></span>

    ![Tekintse át az előfizetési adatok](./media/storsimple-partner-csp-deploy/image3.png)
                              
4. <span data-ttu-id="ac090-129">Tekintse át hello információit, és kattintson a **Submit**.</span><span class="sxs-lookup"><span data-stu-id="ac090-129">Review hello information and click **Submit**.</span></span>

    ![Küldje el az előfizetéshez](./media/storsimple-partner-csp-deploy/image4.png)

5. <span data-ttu-id="ac090-131">Mentse a hello megerősítő információ későbbi felhasználás céljából.</span><span class="sxs-lookup"><span data-stu-id="ac090-131">Save hello confirmation information for future reference.</span></span>

    ![Mentés megerősítése](./media/storsimple-partner-csp-deploy/image5.png)

6. <span data-ttu-id="ac090-133">Található, vagy keresse meg a hozzáadott toohello ügyfél.</span><span class="sxs-lookup"><span data-stu-id="ac090-133">Find or navigate toohello customer you just added.</span></span> <span data-ttu-id="ac090-134">Kattintson a hello **vállalatnév** hello részleteinek le toodrill.</span><span class="sxs-lookup"><span data-stu-id="ac090-134">Click hello **Company name** toodrill down into hello details.</span></span>

    ![Hello ügyfél keresése](./media/storsimple-partner-csp-deploy/image6.png)  

7. <span data-ttu-id="ac090-136">A hello bal oldali ablaktáblában jelölje ki a **szolgáltatásfelügyelet**.</span><span class="sxs-lookup"><span data-stu-id="ac090-136">In hello left-pane, select **Service management**.</span></span> <span data-ttu-id="ac090-137">A hello jobb oldali ablaktáblában, a **felügyelje a szolgáltatásokat**, kattintson a **Microsoft Azure Management Portal** toosign az Azure rendszergazdaként az ügyfél számára.</span><span class="sxs-lookup"><span data-stu-id="ac090-137">In hello right-pane, under **Administer services**, click **Microsoft Azure Management Portal** toosign in as an Azure administrator for your customer.</span></span>

    ![Jelentkezzen be tooAzure portál](./media/storsimple-partner-csp-deploy/image9.png)

8. <span data-ttu-id="ac090-139">toocreate egy StorSimple-kezelőt, kattintson a **+ új** , és keresse meg vagy nyissa meg a túl**StorSimple virtuális eszköz adatsorozat**.</span><span class="sxs-lookup"><span data-stu-id="ac090-139">toocreate a StorSimple Device Manager, click **+ New** and search for or navigate too**StorSimple Virtual Device Series**.</span></span> <span data-ttu-id="ac090-140">További információ: túl[StorSimple Device Manager szolgáltatás telepítése](storsimple-virtual-array-manage-service.md).</span><span class="sxs-lookup"><span data-stu-id="ac090-140">For more information, go too[Deploy a StorSimple Device Manager service](storsimple-virtual-array-manage-service.md).</span></span>

    ![StorSimple Device Manager szolgáltatás létrehozása](./media/storsimple-partner-csp-deploy/image8.png)


## <a name="add-a-subscription"></a><span data-ttu-id="ac090-142">Előfizetés hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ac090-142">Add a subscription</span></span>

<span data-ttu-id="ac090-143">Bizonyos esetekben előfordulhat, hogy egy meglévő ügyfél, és tooadd előfizetés van szüksége.</span><span class="sxs-lookup"><span data-stu-id="ac090-143">In some instances, you may have an existing customer, and you need tooadd a subscription.</span></span> <span data-ttu-id="ac090-144">tooadd előfizetés tooan meglévő ügyfél, hajtsa végre a lépéseket hello partnerportálon hello.</span><span class="sxs-lookup"><span data-stu-id="ac090-144">tooadd a subscription tooan existing customer, perform hello following steps in hello Partner portal.</span></span>

1. <span data-ttu-id="ac090-145">Nyissa meg toohello [Partnerközpontjában](http://partnercenter.microsoft.com/) , jelentkezzen be a kriptográfiai Szolgáltató hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="ac090-145">Go toohello [Partner Center](http://partnercenter.microsoft.com/) and sign in using your CSP credentials.</span></span> <span data-ttu-id="ac090-146">Kattintson a **irányítópult**.</span><span class="sxs-lookup"><span data-stu-id="ac090-146">Click **Dashboard**.</span></span>

     ![A Partner Center irányítópult](./media/storsimple-partner-csp-deploy/image1.png)
                              
2. <span data-ttu-id="ac090-148">A hello bal oldali ablaktáblában kattintson **ügyfelek**.</span><span class="sxs-lookup"><span data-stu-id="ac090-148">In hello left-pane, click **Customers**.</span></span> <span data-ttu-id="ac090-149">Található, vagy keresse meg az előfizetés tooadd kívánt toohello ügyfél.</span><span class="sxs-lookup"><span data-stu-id="ac090-149">Find or navigate toohello customer you want tooadd a subscription to.</span></span> <span data-ttu-id="ac090-150">Kattintson a hello ![kibontott pipa ikon](./media/storsimple-partner-csp-deploy/expand_pane_icon.png) ikon tooexpand hello sor hello vállalat nevét az ügyfél számára.</span><span class="sxs-lookup"><span data-stu-id="ac090-150">Click hello ![Expand check icon](./media/storsimple-partner-csp-deploy/expand_pane_icon.png) icon tooexpand hello row for hello company name for your customer.</span></span> <span data-ttu-id="ac090-151">Hello részleteit, kattintson a **előfizetéseket**.</span><span class="sxs-lookup"><span data-stu-id="ac090-151">In hello details, click **Add subscriptions**.</span></span>

    ![Ügyfelek](./media/storsimple-partner-csp-deploy/image10.png)

3. <span data-ttu-id="ac090-153">Ellenőrizze **Microsoft Azure** a hello **ajánlatok felső** hello előfizetés, és kattintson a **Submit**.</span><span class="sxs-lookup"><span data-stu-id="ac090-153">Check **Microsoft Azure** for hello **Top offers** in hello subscription and click **Submit**.</span></span> <span data-ttu-id="ac090-154">Ez egy új előfizetést hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ac090-154">This creates a new subscription.</span></span>

    ![Új előfizetés hozzáadása](./media/storsimple-partner-csp-deploy/image11.png)

6. <span data-ttu-id="ac090-156">Új előfizetés létrehozása után kattintson **<--ügyfelek** a hello bal oldali ablaktáblán tooreturn toohello **ügyfelek** lap.</span><span class="sxs-lookup"><span data-stu-id="ac090-156">After a new subscription is created, click **<-- Customers** in hello left-pane tooreturn toohello **Customers** page.</span></span> <span data-ttu-id="ac090-157">Keresse meg, amely most létrehozott előfizetés hello ügyfél.</span><span class="sxs-lookup"><span data-stu-id="ac090-157">Search for hello customer for whom you just created a subscription.</span></span> <span data-ttu-id="ac090-158">Kattintson a hello **vállalatnév** hello részleteinek le toodrill.</span><span class="sxs-lookup"><span data-stu-id="ac090-158">Click hello **Company name** toodrill down into hello details.</span></span>

    ![Hello ügyfél keresése](./media/storsimple-partner-csp-deploy/image6.png)  

7. <span data-ttu-id="ac090-160">A hello bal oldali ablaktáblában jelölje ki a **szolgáltatásfelügyelet**.</span><span class="sxs-lookup"><span data-stu-id="ac090-160">In hello left-pane, select **Service management**.</span></span> <span data-ttu-id="ac090-161">A hello jobb oldali ablaktáblában, a **felügyelje a szolgáltatásokat**, kattintson a **Microsoft Azure Management Portal** toosign az Azure rendszergazdaként az ügyfél számára.</span><span class="sxs-lookup"><span data-stu-id="ac090-161">In hello right-pane, under **Administer services**, click **Microsoft Azure Management Portal** toosign in as an Azure administrator for your customer.</span></span>

    ![Jelentkezzen be tooAzure portál](./media/storsimple-partner-csp-deploy/image9.png)

8. <span data-ttu-id="ac090-163">toocreate egy StorSimple-kezelőt, kattintson a **+ új** , és keresse meg vagy nyissa meg a túl**StorSimple virtuális eszköz adatsorozat**.</span><span class="sxs-lookup"><span data-stu-id="ac090-163">toocreate a StorSimple Device Manager, click **+ New** and search for or navigate too**StorSimple Virtual Device Series**.</span></span> <span data-ttu-id="ac090-164">További információ: túl[StorSimple Device Manager szolgáltatás telepítése](storsimple-virtual-array-manage-service.md).</span><span class="sxs-lookup"><span data-stu-id="ac090-164">For more information, go too[Deploy a StorSimple Device Manager service](storsimple-virtual-array-manage-service.md).</span></span>

    ![StorSimple Device Manager szolgáltatás létrehozása](./media/storsimple-partner-csp-deploy/image8.png)

## <a name="next-steps"></a><span data-ttu-id="ac090-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ac090-166">Next steps</span></span>

- <span data-ttu-id="ac090-167">Ha kérdései további hello StorSimple a CSP-hez, folytassa az túl[CSP a StorSimple: gyakran ismételt kérdések](storsimple-partner-csp-faq.md).</span><span class="sxs-lookup"><span data-stu-id="ac090-167">If you have more questions regarding hello StorSimple in CSP, go too[StorSimple in CSP: Frequently asked questions](storsimple-partner-csp-faq.md).</span></span>
- <span data-ttu-id="ac090-168">Ha kész toodeploy a StorSimple, nyissa meg túl[központi telepítése a CSP a StorSimple](storsimple-partner-csp-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="ac090-168">If you are ready toodeploy your StorSimple, go too[Deploy your StorSimple in CSP](storsimple-partner-csp-deploy.md).</span></span>
