---
title: "A Microsoft Azure StorSimple és szolgáltatón Program áttekintése |} Microsoft Docs"
description: "Áttekintése a StorSimple és a CSP a StorSimple-partnerek számára."
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
ms.openlocfilehash: c8cb51093142146fc7d43b51a62d949f6cc38988
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-storsimple-virtual-array-for-cloud-solution-provider-program"></a><span data-ttu-id="c91d5-103">Cloud Solution Provider programot a StorSimple virtuális tömb telepíteni</span><span class="sxs-lookup"><span data-stu-id="c91d5-103">Deploy StorSimple Virtual Array for Cloud Solution Provider Program</span></span>

## <a name="overview"></a><span data-ttu-id="c91d5-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="c91d5-104">Overview</span></span>

<span data-ttu-id="c91d5-105">A StorSimple virtuális tömb is telepíthető a Cloud Solution Provider (CSP) partnerek biztosítják az ügyfelek számára.</span><span class="sxs-lookup"><span data-stu-id="c91d5-105">StorSimple Virtual Array can be deployed by the Cloud Solution Provider (CSP) partners for their customers.</span></span> <span data-ttu-id="c91d5-106">Kriptográfiai Szolgáltató partner hozhat létre a StorSimple eszköz Manager szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="c91d5-106">A CSP partner can create a StorSimple Device Manager service.</span></span> <span data-ttu-id="c91d5-107">Ez a szolgáltatás telepítéséhez és kezeléséhez a StorSimple virtuális tömb és a kapcsolódó megosztások, kötetek és biztonsági mentések majd használható.</span><span class="sxs-lookup"><span data-stu-id="c91d5-107">This service can then be used to deploy and manage StorSimple Virtual Array and the associated shares, volumes, and backups.</span></span>

<span data-ttu-id="c91d5-108">Ez a cikk ismerteti, hogyan CSP partner az ügyfél vagy egy új előfizetés hozzáadása egy meglévő ügyfél és majd hozzon létre egy StorSimple virtuális tömb CSP a telepítendő szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="c91d5-108">This article describes how a CSP partner can add a customer or a new subscription to an existing customer and then create a service to deploy a StorSimple Virtual Array in CSP.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c91d5-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c91d5-109">Prerequisites</span></span>

<span data-ttu-id="c91d5-110">Mielőtt elkezdené, győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="c91d5-110">Before you begin, ensure that:</span></span>

- <span data-ttu-id="c91d5-111">A kriptográfiai Szolgáltató program alatt csatlakozott.</span><span class="sxs-lookup"><span data-stu-id="c91d5-111">You are enrolled under the CSP program.</span></span>
- <span data-ttu-id="c91d5-112">Rendelkezik érvényes [Partnerközpontjában](http://partnercenter.microsoft.com/) bejelentkezési hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="c91d5-112">You have valid [Partner Center](http://partnercenter.microsoft.com/) login credentials.</span></span> <span data-ttu-id="c91d5-113">A hitelesítő adatok lehetővé teszik a jelentkezzen be a partnerportálra, új felhasználók hozzáadása, az ügyfelek kereséséhez, vagy keresse meg a Partner irányítópultról egy felhasználói fiókot.</span><span class="sxs-lookup"><span data-stu-id="c91d5-113">The credentials enable you to sign in to the Partner portal to add new customers, search for customers, or navigate to a customer account from the Partner dashboard.</span></span> <span data-ttu-id="c91d5-114">A kriptográfiai Szolgáltató működhessen, a StorSimple-rendszergazdák az Azure-portálon az ügyfél nevében.</span><span class="sxs-lookup"><span data-stu-id="c91d5-114">The CSP can function as a StorSimple administrator on behalf of the customer in the Azure portal.</span></span>
                             
## <a name="add-a-customer"></a><span data-ttu-id="c91d5-115">Az ügyfél hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c91d5-115">Add a customer</span></span>

<span data-ttu-id="c91d5-116">Ha ad hozzá, az ügyfél, a rendszer automatikusan létrehoz egy előfizetést.</span><span class="sxs-lookup"><span data-stu-id="c91d5-116">If you add a customer, a subscription is automatically created.</span></span> <span data-ttu-id="c91d5-117">Az ügyfél hozzáadása (és automatikusan az előfizetés létrehozása), hajtsa végre az alábbi lépéseket a Partner portál.</span><span class="sxs-lookup"><span data-stu-id="c91d5-117">To add a customer (and automatically create a subscription), perform the following steps in the Partner portal.</span></span>

1. <span data-ttu-id="c91d5-118">Lépjen a [Partnerközpontjában](http://partnercenter.microsoft.com/) , jelentkezzen be a kriptográfiai Szolgáltató hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="c91d5-118">Go to the [Partner Center](http://partnercenter.microsoft.com/) and sign in using your CSP credentials.</span></span> <span data-ttu-id="c91d5-119">Kattintson a **irányítópult**.</span><span class="sxs-lookup"><span data-stu-id="c91d5-119">Click **Dashboard**.</span></span>

     ![A Partner Center irányítópult](./media/storsimple-partner-csp-deploy/image1.png)
                              
2. <span data-ttu-id="c91d5-121">Kattintson a bal oldali ablaktáblán, **ügyfelek**.</span><span class="sxs-lookup"><span data-stu-id="c91d5-121">In the left-pane, click **Customers**.</span></span> <span data-ttu-id="c91d5-122">Kattintson a jobb oldali ablaktáblában **adja hozzá az ügyfelek**.</span><span class="sxs-lookup"><span data-stu-id="c91d5-122">In the right-pane, click **Add customers**.</span></span> <span data-ttu-id="c91d5-123">Adja meg az ügyfél adatait.</span><span class="sxs-lookup"><span data-stu-id="c91d5-123">Enter the details of the customer.</span></span> <span data-ttu-id="c91d5-124">Kattintson a **tovább: előfizetések** hozhat létre olyan ügyfél-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="c91d5-124">Click **Next: Subscriptions** to create a customer subscription.</span></span>

    ![Ügyfél hozzáadása](./media/storsimple-partner-csp-deploy/image2.png)

3.  <span data-ttu-id="c91d5-126">Válassza ki **Microsoft Azure** kínálnak.</span><span class="sxs-lookup"><span data-stu-id="c91d5-126">Select **Microsoft Azure** offer.</span></span> <span data-ttu-id="c91d5-127">Görgessen a lap alján, és kattintson a **felülvizsgálati**.</span><span class="sxs-lookup"><span data-stu-id="c91d5-127">Scroll to the bottom of the page and click **Review**.</span></span>

    ![Tekintse át az előfizetési adatok](./media/storsimple-partner-csp-deploy/image3.png)
                              
4. <span data-ttu-id="c91d5-129">Tekintse át az adatokat, és kattintson a **Submit**.</span><span class="sxs-lookup"><span data-stu-id="c91d5-129">Review the information and click **Submit**.</span></span>

    ![Küldje el az előfizetéshez](./media/storsimple-partner-csp-deploy/image4.png)

5. <span data-ttu-id="c91d5-131">Mentse a megerősítő adatokat későbbi felhasználás céljából.</span><span class="sxs-lookup"><span data-stu-id="c91d5-131">Save the confirmation information for future reference.</span></span>

    ![Mentés megerősítése](./media/storsimple-partner-csp-deploy/image5.png)

6. <span data-ttu-id="c91d5-133">Található, vagy navigáljon arra az ügyfél, az előzőekben adott hozzá.</span><span class="sxs-lookup"><span data-stu-id="c91d5-133">Find or navigate to the customer you just added.</span></span> <span data-ttu-id="c91d5-134">Kattintson a **vállalatnév** részleteit feltárásához.</span><span class="sxs-lookup"><span data-stu-id="c91d5-134">Click the **Company name** to drill down into the details.</span></span>

    ![Keresse meg az ügyfél](./media/storsimple-partner-csp-deploy/image6.png)  

7. <span data-ttu-id="c91d5-136">Válassza ki a bal oldali ablaktáblán, **szolgáltatásfelügyelet**.</span><span class="sxs-lookup"><span data-stu-id="c91d5-136">In the left-pane, select **Service management**.</span></span> <span data-ttu-id="c91d5-137">A jobb oldali ablaktáblában a **felügyelje a szolgáltatásokat**, kattintson a **Microsoft Azure Management Portal** jelentkezhet be a felhasználó Azure rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="c91d5-137">In the right-pane, under **Administer services**, click **Microsoft Azure Management Portal** to sign in as an Azure administrator for your customer.</span></span>

    ![Jelentkezzen be Azure-portálon](./media/storsimple-partner-csp-deploy/image9.png)

8. <span data-ttu-id="c91d5-139">A StorSimple Device Manager létrehozásához kattintson a **+ új** , és keresse meg, vagy navigáljon arra **StorSimple virtuális eszköz adatsorozat**.</span><span class="sxs-lookup"><span data-stu-id="c91d5-139">To create a StorSimple Device Manager, click **+ New** and search for or navigate to **StorSimple Virtual Device Series**.</span></span> <span data-ttu-id="c91d5-140">További információkért látogasson el [StorSimple Device Manager szolgáltatás telepítése](storsimple-virtual-array-manage-service.md).</span><span class="sxs-lookup"><span data-stu-id="c91d5-140">For more information, go to [Deploy a StorSimple Device Manager service](storsimple-virtual-array-manage-service.md).</span></span>

    ![StorSimple Device Manager szolgáltatás létrehozása](./media/storsimple-partner-csp-deploy/image8.png)


## <a name="add-a-subscription"></a><span data-ttu-id="c91d5-142">Előfizetés hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c91d5-142">Add a subscription</span></span>

<span data-ttu-id="c91d5-143">Bizonyos esetekben előfordulhat, hogy egy meglévő ügyfél, és hozzá kell adnia egy előfizetést.</span><span class="sxs-lookup"><span data-stu-id="c91d5-143">In some instances, you may have an existing customer, and you need to add a subscription.</span></span> <span data-ttu-id="c91d5-144">Előfizetés hozzáadása egy meglévő ügyfél, hajtsa végre az alábbi lépéseket a Partner portál.</span><span class="sxs-lookup"><span data-stu-id="c91d5-144">To add a subscription to an existing customer, perform the following steps in the Partner portal.</span></span>

1. <span data-ttu-id="c91d5-145">Lépjen a [Partnerközpontjában](http://partnercenter.microsoft.com/) , jelentkezzen be a kriptográfiai Szolgáltató hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="c91d5-145">Go to the [Partner Center](http://partnercenter.microsoft.com/) and sign in using your CSP credentials.</span></span> <span data-ttu-id="c91d5-146">Kattintson a **irányítópult**.</span><span class="sxs-lookup"><span data-stu-id="c91d5-146">Click **Dashboard**.</span></span>

     ![A Partner Center irányítópult](./media/storsimple-partner-csp-deploy/image1.png)
                              
2. <span data-ttu-id="c91d5-148">Kattintson a bal oldali ablaktáblán, **ügyfelek**.</span><span class="sxs-lookup"><span data-stu-id="c91d5-148">In the left-pane, click **Customers**.</span></span> <span data-ttu-id="c91d5-149">Található, vagy keresse meg az ügyfélnek hozzá szeretne adni egy előfizetés.</span><span class="sxs-lookup"><span data-stu-id="c91d5-149">Find or navigate to the customer you want to add a subscription to.</span></span> <span data-ttu-id="c91d5-150">Kattintson a ![kibontott pipa ikon](./media/storsimple-partner-csp-deploy/expand_pane_icon.png) ikonra kattintva bontsa ki a vállalat nevét az ügyfél számára a sort.</span><span class="sxs-lookup"><span data-stu-id="c91d5-150">Click the ![Expand check icon](./media/storsimple-partner-csp-deploy/expand_pane_icon.png) icon to expand the row for the company name for your customer.</span></span> <span data-ttu-id="c91d5-151">Kattintson a részletek **előfizetéseket**.</span><span class="sxs-lookup"><span data-stu-id="c91d5-151">In the details, click **Add subscriptions**.</span></span>

    ![Ügyfelek](./media/storsimple-partner-csp-deploy/image10.png)

3. <span data-ttu-id="c91d5-153">Ellenőrizze **Microsoft Azure** a a **ajánlatok felső** a előfizetés, majd kattintson a **Submit**.</span><span class="sxs-lookup"><span data-stu-id="c91d5-153">Check **Microsoft Azure** for the **Top offers** in the subscription and click **Submit**.</span></span> <span data-ttu-id="c91d5-154">Ez egy új előfizetést hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c91d5-154">This creates a new subscription.</span></span>

    ![Új előfizetés hozzáadása](./media/storsimple-partner-csp-deploy/image11.png)

6. <span data-ttu-id="c91d5-156">Új előfizetés létrehozása után kattintson **<--ügyfelek** való visszatéréshez bal oldali ablaktáblájában a **ügyfelek** lap.</span><span class="sxs-lookup"><span data-stu-id="c91d5-156">After a new subscription is created, click **<-- Customers** in the left-pane to return to the **Customers** page.</span></span> <span data-ttu-id="c91d5-157">Keresse meg az ügyfél, amely most létrehozott egy előfizetést.</span><span class="sxs-lookup"><span data-stu-id="c91d5-157">Search for the customer for whom you just created a subscription.</span></span> <span data-ttu-id="c91d5-158">Kattintson a **vállalatnév** részleteit feltárásához.</span><span class="sxs-lookup"><span data-stu-id="c91d5-158">Click the **Company name** to drill down into the details.</span></span>

    ![Keresse meg az ügyfél](./media/storsimple-partner-csp-deploy/image6.png)  

7. <span data-ttu-id="c91d5-160">Válassza ki a bal oldali ablaktáblán, **szolgáltatásfelügyelet**.</span><span class="sxs-lookup"><span data-stu-id="c91d5-160">In the left-pane, select **Service management**.</span></span> <span data-ttu-id="c91d5-161">A jobb oldali ablaktáblában a **felügyelje a szolgáltatásokat**, kattintson a **Microsoft Azure Management Portal** jelentkezhet be a felhasználó Azure rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="c91d5-161">In the right-pane, under **Administer services**, click **Microsoft Azure Management Portal** to sign in as an Azure administrator for your customer.</span></span>

    ![Jelentkezzen be Azure-portálon](./media/storsimple-partner-csp-deploy/image9.png)

8. <span data-ttu-id="c91d5-163">A StorSimple Device Manager létrehozásához kattintson a **+ új** , és keresse meg, vagy navigáljon arra **StorSimple virtuális eszköz adatsorozat**.</span><span class="sxs-lookup"><span data-stu-id="c91d5-163">To create a StorSimple Device Manager, click **+ New** and search for or navigate to **StorSimple Virtual Device Series**.</span></span> <span data-ttu-id="c91d5-164">További információkért látogasson el [StorSimple Device Manager szolgáltatás telepítése](storsimple-virtual-array-manage-service.md).</span><span class="sxs-lookup"><span data-stu-id="c91d5-164">For more information, go to [Deploy a StorSimple Device Manager service](storsimple-virtual-array-manage-service.md).</span></span>

    ![StorSimple Device Manager szolgáltatás létrehozása](./media/storsimple-partner-csp-deploy/image8.png)

## <a name="next-steps"></a><span data-ttu-id="c91d5-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c91d5-166">Next steps</span></span>

- <span data-ttu-id="c91d5-167">Ha kérdései további a StorSimple a CSP-hez, folytassa a [CSP a StorSimple: gyakran ismételt kérdések](storsimple-partner-csp-faq.md).</span><span class="sxs-lookup"><span data-stu-id="c91d5-167">If you have more questions regarding the StorSimple in CSP, go to [StorSimple in CSP: Frequently asked questions](storsimple-partner-csp-faq.md).</span></span>
- <span data-ttu-id="c91d5-168">Ha készen áll a központi telepítése a StorSimple, folytassa a [központi telepítése a CSP a StorSimple](storsimple-partner-csp-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="c91d5-168">If you are ready to deploy your StorSimple, go to [Deploy your StorSimple in CSP](storsimple-partner-csp-deploy.md).</span></span>
