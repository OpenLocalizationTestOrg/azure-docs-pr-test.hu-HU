---
title: "A StorSimple 8000 series támogatási jegy naplózása |} Microsoft Docs"
description: "Megtudhatja, hogyan hozzon létre egy támogatási kérést, és a StorSimple eszköz támogatási munkamenet indításához."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 2ebc20fe-f490-4749-8e43-c9fac86f1676
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/27/2017
ms.author: alkohli;anbacker
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cecc2566b432e897b5eff0c12e66598f0518ed80
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="contact-microsoft-support-for-your-storsimple"></a><span data-ttu-id="32c9c-103">A StorSimple a Microsoft támogatási szolgálatához</span><span class="sxs-lookup"><span data-stu-id="32c9c-103">Contact Microsoft Support for your StorSimple</span></span>
<span data-ttu-id="32c9c-104">Ha problémába ütközik a Microsoft Azure StorSimple megoldás, létrehozhat egy szolgáltatási kérelem a technikai támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="32c9c-104">If you encounter any issues with your Microsoft Azure StorSimple solution, you can create a service request for technical support.</span></span> <span data-ttu-id="32c9c-105">Egy online munkamenetben a támogatási szakértőhöz is szükség lehet a StorSimple eszköz támogatási munkamenet indításához.</span><span class="sxs-lookup"><span data-stu-id="32c9c-105">In an online session with your support engineer, you may also need to start a support session on your StorSimple device.</span></span> <span data-ttu-id="32c9c-106">Ez a cikk bemutatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="32c9c-106">This article walks you through:</span></span>

* <span data-ttu-id="32c9c-107">Megtudhatja, hogyan hozzon létre egy támogatási kérést.</span><span class="sxs-lookup"><span data-stu-id="32c9c-107">How to create a support request.</span></span>
* <span data-ttu-id="32c9c-108">Támogatási munkamenetet indítani a Windows PowerShell felületen, a StorSimple eszköz módját.</span><span class="sxs-lookup"><span data-stu-id="32c9c-108">How to start a support session in the Windows PowerShell interface of your StorSimple device.</span></span>

<span data-ttu-id="32c9c-109">Tekintse át a [a StorSimple 8000 Series támogatási SLA-k és információk](https://msdn.microsoft.com/library/mt433077.aspx) a támogatási kérelem létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="32c9c-109">Review the [StorSimple 8000 Series Support SLAs and information](https://msdn.microsoft.com/library/mt433077.aspx) before you create a Support request.</span></span>

## <a name="create-a-support-request"></a><span data-ttu-id="32c9c-110">Támogatási kérés létrehozása</span><span class="sxs-lookup"><span data-stu-id="32c9c-110">Create a support request</span></span>
<span data-ttu-id="32c9c-111">A következő lépésekkel hozzon létre egy támogatási kérést:</span><span class="sxs-lookup"><span data-stu-id="32c9c-111">Perform the following steps to create a support request:</span></span>

#### <a name="to-create-a-support-request"></a><span data-ttu-id="32c9c-112">A támogatási kérelem létrehozása</span><span class="sxs-lookup"><span data-stu-id="32c9c-112">To create a support request</span></span>
1. <span data-ttu-id="32c9c-113">Az a [a klasszikus Azure portálon](https://manage.windowsazure.com/), kattintson a jobb felső sarokban, a fiók nevére, és kattintson a **forduljon a Microsoft ügyfélszolgálatához**.</span><span class="sxs-lookup"><span data-stu-id="32c9c-113">In the [Azure classic portal](https://manage.windowsazure.com/), in the upper right corner, click your account name and then click **Contact Microsoft Support**.</span></span>
   
    ![Forduljon az MS-támogatás ManagementPortal keresztül](./media/storsimple-contact-microsoft-support/Ibiza1.png)
2. <span data-ttu-id="32c9c-115">Az új Azure-portálra (portal.azure.com) irányítja.</span><span class="sxs-lookup"><span data-stu-id="32c9c-115">You will be redirected to the new Azure portal (portal.azure.com).</span></span> <span data-ttu-id="32c9c-116">Kattintson a **új támogatja a kérelem** csempére.</span><span class="sxs-lookup"><span data-stu-id="32c9c-116">Click the **New support request** tile.</span></span>
   
    ![Új portálon keresztül forduljon az MS-támogatás](./media/storsimple-contact-microsoft-support/Ibiza2.png)
   
    <span data-ttu-id="32c9c-118">A képernyő jobb oldalán a **új támogatja a kérelem** ablaktáblán jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="32c9c-118">On the right side of the screen, the **New support request** pane appears.</span></span> 
   
    ![Új támogatási kérelem ablaktábla](./media/storsimple-contact-microsoft-support/Ibiza3a.png)
3. <span data-ttu-id="32c9c-120">Az a **alapjai** párbeszédpanelen adja meg a következőket:</span><span class="sxs-lookup"><span data-stu-id="32c9c-120">In the **Basics** dialog box, complete the following:</span></span>                                
   
   1. <span data-ttu-id="32c9c-121">Az a **típusú** legördülő listában válassza **műszaki**.</span><span class="sxs-lookup"><span data-stu-id="32c9c-121">From the **Issue type** drop-down list , select **Technical**.</span></span>
   2. <span data-ttu-id="32c9c-122">Válassza ki a **előfizetés** a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="32c9c-122">Select a **Subscription** from the drop-down list.</span></span>
   3. <span data-ttu-id="32c9c-123">Az a **szolgáltatás** legördülő listában válassza **StorSimple**.</span><span class="sxs-lookup"><span data-stu-id="32c9c-123">From the **Service** drop-down list, select **StorSimple**.</span></span> 
   4. <span data-ttu-id="32c9c-124">Válassza ki a **támogatási csomag** a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="32c9c-124">Select a **Support plan** from the drop-down list.</span></span> <span data-ttu-id="32c9c-125">Szüksége van ahhoz, hogy technikai támogatási szolgálatával fizetős támogatási csomagot.</span><span class="sxs-lookup"><span data-stu-id="32c9c-125">You need a paid support plan to enable Technical Support.</span></span>
4. <span data-ttu-id="32c9c-126">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="32c9c-126">Click **Next**.</span></span> <span data-ttu-id="32c9c-127">A **probléma** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="32c9c-127">The **Problem** dialog box appears.</span></span>
   
    ![Új támogatási kérelem ablaktábla](./media/storsimple-contact-microsoft-support/Ibiza5a.png) 
5. <span data-ttu-id="32c9c-129">Az a **probléma** párbeszédpanelen adja meg a következőket:</span><span class="sxs-lookup"><span data-stu-id="32c9c-129">In the **Problem** dialog box, complete the following:</span></span>
   
   1. <span data-ttu-id="32c9c-130">Válassza ki a **súlyossági** szint a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="32c9c-130">Select a **Severity** level from the drop-down list.</span></span>
   2. <span data-ttu-id="32c9c-131">Válassza ki a **problématípust** a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="32c9c-131">Select a **Problem type** from the drop-down list.</span></span>
   3. <span data-ttu-id="32c9c-132">Válassza ki a **kategória** a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="32c9c-132">Select a **Category** from the drop-down list.</span></span> 
   4. <span data-ttu-id="32c9c-133">Az a **részletek** mezőben röviden ismertesse a problémát.</span><span class="sxs-lookup"><span data-stu-id="32c9c-133">In the **Details** box, briefly describe your issue.</span></span>
   5. <span data-ttu-id="32c9c-134">Az a **időkereten belül** jelzik, hogy a dátum, idő és időzóna, a probléma legutóbbi előfordulásának megfelelő mezőbe.</span><span class="sxs-lookup"><span data-stu-id="32c9c-134">In the **Time frame** box, indicate the date, time, and time zone that corresponds to the most recent occurrence of your issue.</span></span>
   6. <span data-ttu-id="32c9c-135">A **fájlfeltöltés**, tallózással keresse meg a támogatási csomag a mappa ikonra.</span><span class="sxs-lookup"><span data-stu-id="32c9c-135">Under **File upload**, click the folder icon to browse to your support package.</span></span>
   7. <span data-ttu-id="32c9c-136">Válassza ki a **diagnosztikai információkat** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="32c9c-136">Select the **Share diagnostic information** check box.</span></span>
6. <span data-ttu-id="32c9c-137">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="32c9c-137">Click **Next**.</span></span> <span data-ttu-id="32c9c-138">A **elérhetőségi adatai** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="32c9c-138">The **Contact information** dialog box appears.</span></span>
   
    ![Új támogatási kérelem ablaktábla](./media/storsimple-contact-microsoft-support/Ibiza6a.png) 
7. <span data-ttu-id="32c9c-140">Adja meg a kapcsolattartási adatait, és válasszon ki egy kapcsolattartási módszert, (telefonon vagy e-mailek).</span><span class="sxs-lookup"><span data-stu-id="32c9c-140">Enter your contact information and select a contact method (phone or email).</span></span> 
8. <span data-ttu-id="32c9c-141">Válassza ki a **kapcsolattartási adatok módosításainak mentése az a későbbi támogatási kérelmekhez** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="32c9c-141">Select the **Save contact changes for future support requests** check box.</span></span>
9. <span data-ttu-id="32c9c-142">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="32c9c-142">Click **Create**.</span></span>

<span data-ttu-id="32c9c-143">Miután elküldte a kérést, a támogatási szakértőhöz felveszi Önnel a lehető leghamarabb folytatja a kérelemhez.</span><span class="sxs-lookup"><span data-stu-id="32c9c-143">After you have submitted your request, a Support engineer will contact you as soon as possible to proceed with your request.</span></span>

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a><span data-ttu-id="32c9c-144">Támogatási munkamenetet indítani a Windows PowerShellben a StorSimple</span><span class="sxs-lookup"><span data-stu-id="32c9c-144">Start a support session in Windows PowerShell for StorSimple</span></span>
<span data-ttu-id="32c9c-145">Hárítsa el a StorSimple eszköz esetleg előforduló, szüksége lesz a Microsoft Support csoport bevonásához.</span><span class="sxs-lookup"><span data-stu-id="32c9c-145">To troubleshoot any issues that you might experience with the StorSimple device, you will need to engage with the Microsoft Support team.</span></span> <span data-ttu-id="32c9c-146">Microsoft Support lehet szükség a támogatási munkamenet bejelentkezni az eszközt.</span><span class="sxs-lookup"><span data-stu-id="32c9c-146">Microsoft Support may need to use a support session to log on to your device.</span></span> 

<span data-ttu-id="32c9c-147">A következő lépésekkel támogatási munkamenet indításához:</span><span class="sxs-lookup"><span data-stu-id="32c9c-147">Perform the following steps to start a support session:</span></span>

#### <a name="to-start-a-support-session"></a><span data-ttu-id="32c9c-148">Támogatási munkamenet indításához</span><span class="sxs-lookup"><span data-stu-id="32c9c-148">To start a support session</span></span>
1. <span data-ttu-id="32c9c-149">Az eszközhöz való hozzáféréshez, közvetlenül a soros konzol használatával, vagy egy telnet-munkamenet távoli számítógépről keresztül.</span><span class="sxs-lookup"><span data-stu-id="32c9c-149">Access the device directly by using the serial console or through a telnet session from a remote computer.</span></span> <span data-ttu-id="32c9c-150">Ehhez kövesse a lépéseket [a PuTTY használata az eszköz soros konzoljához való csatlakozáshoz](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="32c9c-150">To do this, follow the steps in [Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
2. <span data-ttu-id="32c9c-151">A megnyitott munkamenetben nyomja le a **Enter** kulcsot használva egy parancssori ablakot.</span><span class="sxs-lookup"><span data-stu-id="32c9c-151">In the session that opens, press the **Enter** key to get a command prompt.</span></span>
3. <span data-ttu-id="32c9c-152">A soros konzol menüben válassza az 1. lehetőség – **jelentkezzen be a teljes körű hozzáférési**.</span><span class="sxs-lookup"><span data-stu-id="32c9c-152">In the serial console menu, select option 1, **Log in with full access**.</span></span>
4. <span data-ttu-id="32c9c-153">A parancssorba írja be a következő jelszóval:</span><span class="sxs-lookup"><span data-stu-id="32c9c-153">At the prompt, type the following password:</span></span> 
   
    `Password1`
5. <span data-ttu-id="32c9c-154">A parancssorba írja be a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="32c9c-154">At the prompt, type the following command:</span></span>
   
    `Enable-HcsSupportAccess`
6. <span data-ttu-id="32c9c-155">Egy titkosított karakterláncot jelennek majd meg.</span><span class="sxs-lookup"><span data-stu-id="32c9c-155">An encrypted string will be presented to you.</span></span> <span data-ttu-id="32c9c-156">Másolja ezt a karakterláncot egy szövegszerkesztőbe, például a Jegyzettömböt.</span><span class="sxs-lookup"><span data-stu-id="32c9c-156">Copy this string into a text editor such as Notepad.</span></span>
7. <span data-ttu-id="32c9c-157">Mentse ezt a karakterláncot, és az e-mailt küldeni a Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="32c9c-157">Save this string and send it in an email message to Microsoft Support.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="32c9c-158">Letilthatja a támogatás elérését futtatásával `Disable-HcsSupportAccess`.</span><span class="sxs-lookup"><span data-stu-id="32c9c-158">You can disable support access by running `Disable-HcsSupportAccess`.</span></span> <span data-ttu-id="32c9c-159">A StorSimple eszköz megkísérli is támogatási hozzáférés letiltása a 8 órával azt követően, a munkamenet kezdeményezték.</span><span class="sxs-lookup"><span data-stu-id="32c9c-159">The StorSimple device will also attempt to disable support access 8 hours after the session was initiated.</span></span> <span data-ttu-id="32c9c-160">A StorSimple eszköz hitelesítő adatok módosítása a támogatási munkamenet kezdeményezése után célszerű.</span><span class="sxs-lookup"><span data-stu-id="32c9c-160">It is a best practice to change your StorSimple device credentials after initiating a support session.</span></span>
> 
> 

