---
title: "Támogatási jegy vagy a StorSimple 8000 Series eset létrehozása |} Microsoft Docs"
description: "Megtudhatja, hogyan jelentkezzen támogatási kérelmet, és a StorSimple 8000 series eszközön támogatási munkamenet indításához."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: alkohli;
ms.openlocfilehash: 4b5a14237ce79100f980b2186b2c3c887abaa296
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="contact-microsoft-support"></a><span data-ttu-id="b7cd1-103">Forduljon a Microsoft támogatási szolgálatához.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-103">Contact Microsoft Support</span></span>

<span data-ttu-id="b7cd1-104">A StorSimple Device Manager lehetővé teszi a **új támogatási kérelem jelentkezzen** belül a szolgáltatás összefoglaló panelre.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-104">The StorSimple Device Manager provides the capability to **log a new support request** within the service summary blade.</span></span> <span data-ttu-id="b7cd1-105">Ha problémába ütközik a StorSimple-megoldás, létrehozhat egy szolgáltatási kérelem a technikai támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-105">If you encounter any issues with your StorSimple solution, you can create a service request for technical support.</span></span> <span data-ttu-id="b7cd1-106">Egy online munkamenetben a támogatási szakértőhöz is szükség lehet a StorSimple eszköz támogatási munkamenet indításához.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-106">In an online session with your support engineer, you may also need to start a support session on your StorSimple device.</span></span> <span data-ttu-id="b7cd1-107">Ez a cikk bemutatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="b7cd1-107">This article walks you through:</span></span>

* <span data-ttu-id="b7cd1-108">Megtudhatja, hogyan hozzon létre egy támogatási kérést.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-108">How to create a support request.</span></span>
* <span data-ttu-id="b7cd1-109">Hogyan kezelheti a támogatási kérelem életciklusa, az a portálon.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-109">How to manage a support request lifecycle from within the portal.</span></span>
* <span data-ttu-id="b7cd1-110">Támogatási munkamenetet indítani a Windows PowerShell felületen, a StorSimple eszköz módját.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-110">How to start a support session in the Windows PowerShell interface of your StorSimple device.</span></span>

<span data-ttu-id="b7cd1-111">Tekintse át a [a StorSimple 8000 Series támogatási SLA-k és információk](https://msdn.microsoft.com/library/mt433077.aspx) a támogatási kérelem létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-111">Review the [StorSimple 8000 Series Support SLAs and information](https://msdn.microsoft.com/library/mt433077.aspx) before you create a Support request.</span></span>

## <a name="create-a-support-request"></a><span data-ttu-id="b7cd1-112">Támogatási kérés létrehozása</span><span class="sxs-lookup"><span data-stu-id="b7cd1-112">Create a support request</span></span>

<span data-ttu-id="b7cd1-113">Attól függően, a [támogatási csomag](https://azure.microsoft.com/support/plans/), létrehozhat támogatási jegyeket egy problémát a StorSimple eszközt a StorSimple Device Manager szolgáltatás összefoglaló blade-ről.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-113">Depending upon your [support plan](https://azure.microsoft.com/support/plans/), you can create support tickets for an issue on your StorSimple device directly from the StorSimple Device Manager service summary blade.</span></span> <span data-ttu-id="b7cd1-114">A következő lépésekkel hozzon létre egy támogatási kérést:</span><span class="sxs-lookup"><span data-stu-id="b7cd1-114">Perform the following steps to create a support request:</span></span>

#### <a name="to-create-a-support-request"></a><span data-ttu-id="b7cd1-115">A támogatási kérelem létrehozása</span><span class="sxs-lookup"><span data-stu-id="b7cd1-115">To create a support request</span></span>

1. <span data-ttu-id="b7cd1-116">Nyissa meg a StorSimple-eszközkezelő szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-116">Go to your StorSimple Device Manager service.</span></span> <span data-ttu-id="b7cd1-117">A Szolgáltatásbeállítások összefoglaló panelen keresse meg **támogatási + hibaelhárítás** szakaszt, és kattintson a **új támogatja a kérelem**.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-117">In the service summary blade settings, go to **SUPPORT + TROUBLESHOOTING** section and then click **New support request**.</span></span>
     
    ![Új portálon keresztül forduljon az MS-támogatás](./media/storsimple-8000-contact-microsoft-support/contactsupport1.png)
   
2. <span data-ttu-id="b7cd1-119">Az a **új támogatja a kérelem** panelen válassza **alapjai**.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-119">In the **New support request** blade, select **Basics**.</span></span> <span data-ttu-id="b7cd1-120">Az a **alapjai** panelen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="b7cd1-120">In the **Basics** blade, do the following steps:</span></span>
   1. <span data-ttu-id="b7cd1-121">Az a **típusú** legördülő listában válassza **műszaki**.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-121">From the **Issue type** drop-down list , select **Technical**.</span></span>
   2. <span data-ttu-id="b7cd1-122">Az aktuális **előfizetés**, **szolgáltatás** típus, és a **erőforrás** (a StorSimple eszköz kezelő szolgáltatás) közül automatikusan választ.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-122">The current **Subscription**, **Service** type, and the **Resource** (StorSimple Device Manager service) are automatically chosen.</span></span> 
   3. <span data-ttu-id="b7cd1-123">Válassza ki a **támogatási csomag** a legördülő listából, ha több tervek az Ön előfizetéséhez rendelve van.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-123">Select a **Support plan** from the drop-down list if you have multiple plans associated with your subscription.</span></span> <span data-ttu-id="b7cd1-124">Szüksége van ahhoz, hogy technikai támogatási szolgálatával fizetős támogatási csomagot.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-124">You need a paid support plan to enable Technical Support.</span></span>
   4. <span data-ttu-id="b7cd1-125">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-125">Click **Next**.</span></span>

       ![Új portálon keresztül forduljon az MS-támogatás](./media/storsimple-8000-contact-microsoft-support/contactsupport2.png)

3. <span data-ttu-id="b7cd1-127">Az a **új támogatja a kérelem** panelen válassza **lépés 2 probléma**.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-127">In the **New support request** blade, select **Step 2 Problem**.</span></span> <span data-ttu-id="b7cd1-128">Az a **probléma** panelen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="b7cd1-128">In the **Problem** blade, do the following steps:</span></span>
    
    1. <span data-ttu-id="b7cd1-129">Válassza ki a **súlyossági**.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-129">Choose the **Severity**.</span></span>
    2. <span data-ttu-id="b7cd1-130">Adja meg, ha a készülék vagy a StorSimple Device Manager szolgáltatással kapcsolatos hibát.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-130">Specify if the issue is related to the appliance or the StorSimple Device Manager service.</span></span>
    3. <span data-ttu-id="b7cd1-131">Válasszon egy **kategória** ehhez adja ki, és adjon meg több **részletek** a problémával kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-131">Choose a **Category** for this issue and provide more **Details** about the issue.</span></span>
    4. <span data-ttu-id="b7cd1-132">Adja meg a kezdő dátum és idő, a probléma.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-132">Provide the start date and time for the problem.</span></span>
    5. <span data-ttu-id="b7cd1-133">Az a **fájlfeltöltés**, tallózással keresse meg a támogatási csomag a mappa ikonra.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-133">In the **File upload**, click the folder icon to browse to your support package.</span></span>
    6. <span data-ttu-id="b7cd1-134">Ellenőrizze **diagnosztikai információkat**.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-134">Check **Share diagnostic information**.</span></span>
    7. <span data-ttu-id="b7cd1-135">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-135">Click **Next**.</span></span>

       ![Új portálon keresztül forduljon az MS-támogatás](./media/storsimple-8000-contact-microsoft-support/contactsupport3.png) 

4. <span data-ttu-id="b7cd1-137">Az a **új támogatja a kérelem** panelen kattintson a **lépés 3 elérhetőségi adatait**.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-137">In the **New support request** blade, click **Step 3 Contact information**.</span></span> <span data-ttu-id="b7cd1-138">Az a **elérhetőségi adatai** panelen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="b7cd1-138">In the **Contact information** blade, do the following steps:</span></span>

    1. <span data-ttu-id="b7cd1-139">Az a **beállítások forduljon**, adja meg az elsődleges kapcsolattartási módszert, (telefonon vagy e-mailek) és a nyelv.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-139">In the **Contact options**, provide your preferred contact method (phone or email) and the language.</span></span> <span data-ttu-id="b7cd1-140">A válaszidő automatikusan ki van jelölve, az előfizetési terv alapján.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-140">The response time is automatically selected based on your subscription plan.</span></span>
    2. <span data-ttu-id="b7cd1-141">Adja meg a nevét, e-mail, opcionális forduljon, ország elérhetőségi adatait.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-141">In the Contact information, provide your name, email, optional contact, country.</span></span> <span data-ttu-id="b7cd1-142">Válassza ki a **kapcsolattartási adatok módosításainak mentése az a későbbi támogatási kérelmekhez** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-142">Select the **Save contact changes for future support requests** check box.</span></span>
    3. <span data-ttu-id="b7cd1-143">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-143">Click **Create**.</span></span>
   
        ![Új portálon keresztül forduljon az MS-támogatás](./media/storsimple-8000-contact-microsoft-support/contactsupport5.png)   

    <span data-ttu-id="b7cd1-145">Microsoft Support ezen információk használatával érheti el, további információt, a diagnosztikai és a megoldás.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-145">Microsoft Support will use this information to reach out to you for further information, diagnosis, and resolution.</span></span>
<span data-ttu-id="b7cd1-146">Miután elküldte a kérést, a támogatási szakértőhöz felveszi Önnel a lehető leghamarabb folytatja a kérelemhez.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-146">After you have submitted your request, a Support engineer will contact you as soon as possible to proceed with your request.</span></span>

## <a name="manage-a-support-request"></a><span data-ttu-id="b7cd1-147">Egy támogatási kérést kezelése</span><span class="sxs-lookup"><span data-stu-id="b7cd1-147">Manage a support request</span></span>

<span data-ttu-id="b7cd1-148">A támogatási jegy létrehozása után a jegyet a teljes életciklusán keresztül kezelheti a portálon.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-148">After creating a support ticket, you can manage the lifecycle of the ticket from within the portal.</span></span>

#### <a name="to-manage-your-support-requests"></a><span data-ttu-id="b7cd1-149">A támogatási kérelmek kezelése</span><span class="sxs-lookup"><span data-stu-id="b7cd1-149">To manage your support requests</span></span>

1. <span data-ttu-id="b7cd1-150">Ahhoz, hogy a Súgó és támogatás oldalra, keresse meg **Tallózás > Súgó + támogatás**.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-150">To get to the help and support page, navigate to **Browse > Help + support**.</span></span>

    ![Támogatási kérelmek kezelése](./media/storsimple-8000-contact-microsoft-support/managesupport1.png)

2. <span data-ttu-id="b7cd1-152">A támogatási kérelmek táblázatos listája jelenik meg a **súgó + támogatás** panelen.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-152">A tabular listing of All the support requests is displayed in the **Help + support** blade.</span></span>

    ![Támogatási kérelmek kezelése](./media/storsimple-8000-contact-microsoft-support/managesupport2.png)

3. <span data-ttu-id="b7cd1-154">Jelölje ki, majd kattintson egy támogatási kérést.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-154">Select and click a support request.</span></span> <span data-ttu-id="b7cd1-155">Az állapot és a részletek a kérelemhez tartozó tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-155">You can view the status and the details for this request.</span></span> <span data-ttu-id="b7cd1-156">Kattintson a **+ új üzenet** Ha azt szeretné, hogy a kérés nyomon követéséhez.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-156">Click **+ New message** if you want to follow up on this request.</span></span>

    ![Támogatási kérelmek kezelése](./media/storsimple-8000-contact-microsoft-support/managesupport3.png)

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a><span data-ttu-id="b7cd1-158">Támogatási munkamenetet indítani a Windows PowerShellben a StorSimple</span><span class="sxs-lookup"><span data-stu-id="b7cd1-158">Start a support session in Windows PowerShell for StorSimple</span></span>

<span data-ttu-id="b7cd1-159">Hárítsa el a StorSimple eszköz esetleg előforduló, szüksége lesz a Microsoft Support csoport bevonásához.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-159">To troubleshoot any issues that you might experience with the StorSimple device, you will need to engage with the Microsoft Support team.</span></span> <span data-ttu-id="b7cd1-160">Microsoft Support lehet szükség a támogatási munkamenet bejelentkezni az eszközt.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-160">Microsoft Support may need to use a support session to log on to your device.</span></span>

<span data-ttu-id="b7cd1-161">A következő lépésekkel támogatási munkamenet indításához:</span><span class="sxs-lookup"><span data-stu-id="b7cd1-161">Perform the following steps to start a support session:</span></span>

#### <a name="to-start-a-support-session"></a><span data-ttu-id="b7cd1-162">Támogatási munkamenet indításához</span><span class="sxs-lookup"><span data-stu-id="b7cd1-162">To start a support session</span></span>

1. <span data-ttu-id="b7cd1-163">Az eszközhöz való hozzáféréshez, közvetlenül a soros konzol használatával, vagy egy telnet-munkamenet távoli számítógépről keresztül.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-163">Access the device directly by using the serial console or through a telnet session from a remote computer.</span></span> <span data-ttu-id="b7cd1-164">Ehhez kövesse a lépéseket [a PuTTY használata az eszköz soros konzoljához való csatlakozáshoz](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="b7cd1-164">To do this, follow the steps in [Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
2. <span data-ttu-id="b7cd1-165">A megnyitott munkamenetben nyomja le a **Enter** kulcsot használva egy parancssori ablakot.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-165">In the session that opens, press the **Enter** key to get a command prompt.</span></span>
3. <span data-ttu-id="b7cd1-166">A soros konzol menüben válassza az 1. lehetőség – **jelentkezzen be a teljes körű hozzáférési**.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-166">In the serial console menu, select option 1, **Log in with full access**.</span></span>
4. <span data-ttu-id="b7cd1-167">A parancssorba írja be a következő jelszóval:</span><span class="sxs-lookup"><span data-stu-id="b7cd1-167">At the prompt, type the following password:</span></span>
   
    `Password1`
5. <span data-ttu-id="b7cd1-168">A parancssorba írja be a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="b7cd1-168">At the prompt, type the following command:</span></span>
   
    `Enable-HcsSupportAccess`
6. <span data-ttu-id="b7cd1-169">Egy titkosított karakterláncot jelennek majd meg.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-169">An encrypted string will be presented to you.</span></span> <span data-ttu-id="b7cd1-170">Másolja ezt a karakterláncot egy szövegszerkesztőbe, például a Jegyzettömböt.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-170">Copy this string into a text editor such as Notepad.</span></span>
7. <span data-ttu-id="b7cd1-171">Mentse ezt a karakterláncot, és az e-mailt küldeni a Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-171">Save this string and send it in an email message to Microsoft Support.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b7cd1-172">Letilthatja a támogatás elérését futtatásával `Disable-HcsSupportAccess`.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-172">You can disable support access by running `Disable-HcsSupportAccess`.</span></span> <span data-ttu-id="b7cd1-173">A StorSimple eszköz megkísérli is támogatási hozzáférés letiltása a 8 órával azt követően, a munkamenet kezdeményezték.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-173">The StorSimple device will also attempt to disable support access 8 hours after the session was initiated.</span></span> <span data-ttu-id="b7cd1-174">A StorSimple eszköz hitelesítő adatok módosítása a támogatási munkamenet kezdeményezése után célszerű.</span><span class="sxs-lookup"><span data-stu-id="b7cd1-174">It is a best practice to change your StorSimple device credentials after initiating a support session.</span></span>


## <a name="next-steps"></a><span data-ttu-id="b7cd1-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b7cd1-175">Next steps</span></span>

<span data-ttu-id="b7cd1-176">Megtudhatja, hogyan [diagnosztizálhatja és a StorSimple 8000 series eszköz kapcsolatos problémák megoldásához](storsimple-troubleshoot-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="b7cd1-176">Learn how to [diagnose and solve problems related to your StorSimple 8000 series device](storsimple-troubleshoot-deployment.md)</span></span>
