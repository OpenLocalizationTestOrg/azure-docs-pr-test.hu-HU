---
title: "a StorSimple 8000 series támogatási jegy aaaLog |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate támogatási kérelem és a StorSimple eszköz támogatási munkamenet indításához."
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
ms.openlocfilehash: e1a3aa3c56e036c782c4fb502c477dc0feaa0ccd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="contact-microsoft-support-for-your-storsimple"></a><span data-ttu-id="34026-103">A StorSimple a Microsoft támogatási szolgálatához</span><span class="sxs-lookup"><span data-stu-id="34026-103">Contact Microsoft Support for your StorSimple</span></span>
<span data-ttu-id="34026-104">Ha problémába ütközik a Microsoft Azure StorSimple megoldás, létrehozhat egy szolgáltatási kérelem a technikai támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="34026-104">If you encounter any issues with your Microsoft Azure StorSimple solution, you can create a service request for technical support.</span></span> <span data-ttu-id="34026-105">Egy online munkamenetben a támogatási szakértőhöz szükség lehet a támogatási munkamenet toostart a StorSimple eszköz.</span><span class="sxs-lookup"><span data-stu-id="34026-105">In an online session with your support engineer, you may also need toostart a support session on your StorSimple device.</span></span> <span data-ttu-id="34026-106">Ez a cikk bemutatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="34026-106">This article walks you through:</span></span>

* <span data-ttu-id="34026-107">Hogyan toocreate támogatási kérelmet.</span><span class="sxs-lookup"><span data-stu-id="34026-107">How toocreate a support request.</span></span>
* <span data-ttu-id="34026-108">A támogatási munkamenet toostart hogyan Windows PowerShell felületet a StorSimple eszköz hello.</span><span class="sxs-lookup"><span data-stu-id="34026-108">How toostart a support session in hello Windows PowerShell interface of your StorSimple device.</span></span>

<span data-ttu-id="34026-109">Felülvizsgálati hello [a StorSimple 8000 Series támogatási SLA-k és információk](https://msdn.microsoft.com/library/mt433077.aspx) a támogatási kérelem létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="34026-109">Review hello [StorSimple 8000 Series Support SLAs and information](https://msdn.microsoft.com/library/mt433077.aspx) before you create a Support request.</span></span>

## <a name="create-a-support-request"></a><span data-ttu-id="34026-110">Támogatási kérés létrehozása</span><span class="sxs-lookup"><span data-stu-id="34026-110">Create a support request</span></span>
<span data-ttu-id="34026-111">Hajtsa végre a következő lépéseket toocreate egy támogatási kérést hello:</span><span class="sxs-lookup"><span data-stu-id="34026-111">Perform hello following steps toocreate a support request:</span></span>

#### <a name="toocreate-a-support-request"></a><span data-ttu-id="34026-112">egy támogatási kérést toocreate</span><span class="sxs-lookup"><span data-stu-id="34026-112">toocreate a support request</span></span>
1. <span data-ttu-id="34026-113">A hello [a klasszikus Azure portálon](https://manage.windowsazure.com/)hello jobb felső sarokban, kattintson a fiók nevére, kattintson **forduljon a Microsoft ügyfélszolgálatához**.</span><span class="sxs-lookup"><span data-stu-id="34026-113">In hello [Azure classic portal](https://manage.windowsazure.com/), in hello upper right corner, click your account name and then click **Contact Microsoft Support**.</span></span>
   
    ![Forduljon az MS-támogatás ManagementPortal keresztül](./media/storsimple-contact-microsoft-support/Ibiza1.png)
2. <span data-ttu-id="34026-115">Átirányított toohello új Azure-portálon (portal.azure.com) lesz.</span><span class="sxs-lookup"><span data-stu-id="34026-115">You will be redirected toohello new Azure portal (portal.azure.com).</span></span> <span data-ttu-id="34026-116">Kattintson a hello **új támogatja a kérelem** csempére.</span><span class="sxs-lookup"><span data-stu-id="34026-116">Click hello **New support request** tile.</span></span>
   
    ![Új portálon keresztül forduljon az MS-támogatás](./media/storsimple-contact-microsoft-support/Ibiza2.png)
   
    <span data-ttu-id="34026-118">Üdvözlő képernyőt hello jobb oldalán, hello **új támogatja a kérelem** ablaktáblán jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="34026-118">On hello right side of hello screen, hello **New support request** pane appears.</span></span> 
   
    ![Új támogatási kérelem ablaktábla](./media/storsimple-contact-microsoft-support/Ibiza3a.png)
3. <span data-ttu-id="34026-120">A hello **alapjai** teljes hello a következő párbeszédpanelen:</span><span class="sxs-lookup"><span data-stu-id="34026-120">In hello **Basics** dialog box, complete hello following:</span></span>                                
   
   1. <span data-ttu-id="34026-121">A hello **típusú** legördülő listában válassza **műszaki**.</span><span class="sxs-lookup"><span data-stu-id="34026-121">From hello **Issue type** drop-down list , select **Technical**.</span></span>
   2. <span data-ttu-id="34026-122">Válassza ki a **előfizetés** hello legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="34026-122">Select a **Subscription** from hello drop-down list.</span></span>
   3. <span data-ttu-id="34026-123">A hello **szolgáltatás** legördülő listában válassza **StorSimple**.</span><span class="sxs-lookup"><span data-stu-id="34026-123">From hello **Service** drop-down list, select **StorSimple**.</span></span> 
   4. <span data-ttu-id="34026-124">Válassza ki a **támogatási csomag** hello legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="34026-124">Select a **Support plan** from hello drop-down list.</span></span> <span data-ttu-id="34026-125">A technikai támogatási szolgálatával fizetős támogatási terv tooenable van szüksége.</span><span class="sxs-lookup"><span data-stu-id="34026-125">You need a paid support plan tooenable Technical Support.</span></span>
4. <span data-ttu-id="34026-126">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="34026-126">Click **Next**.</span></span> <span data-ttu-id="34026-127">Hello **probléma** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="34026-127">hello **Problem** dialog box appears.</span></span>
   
    ![Új támogatási kérelem ablaktábla](./media/storsimple-contact-microsoft-support/Ibiza5a.png) 
5. <span data-ttu-id="34026-129">A hello **probléma** teljes hello a következő párbeszédpanelen:</span><span class="sxs-lookup"><span data-stu-id="34026-129">In hello **Problem** dialog box, complete hello following:</span></span>
   
   1. <span data-ttu-id="34026-130">Válassza ki a **súlyossági** szintű hello legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="34026-130">Select a **Severity** level from hello drop-down list.</span></span>
   2. <span data-ttu-id="34026-131">Válassza ki a **problématípust** hello legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="34026-131">Select a **Problem type** from hello drop-down list.</span></span>
   3. <span data-ttu-id="34026-132">Válassza ki a **kategória** hello legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="34026-132">Select a **Category** from hello drop-down list.</span></span> 
   4. <span data-ttu-id="34026-133">A hello **részletek** mezőben röviden ismertesse a problémát.</span><span class="sxs-lookup"><span data-stu-id="34026-133">In hello **Details** box, briefly describe your issue.</span></span>
   5. <span data-ttu-id="34026-134">A hello **időkereten belül** hello dátum, idő és időzóna, a probléma legutóbbi előfordulásának toohello megfelelő mezőbe.</span><span class="sxs-lookup"><span data-stu-id="34026-134">In hello **Time frame** box, indicate hello date, time, and time zone that corresponds toohello most recent occurrence of your issue.</span></span>
   6. <span data-ttu-id="34026-135">A **fájlfeltöltés**, kattintson a hello mappa ikon toobrowse tooyour támogatási csomag.</span><span class="sxs-lookup"><span data-stu-id="34026-135">Under **File upload**, click hello folder icon toobrowse tooyour support package.</span></span>
   7. <span data-ttu-id="34026-136">Jelölje be hello **diagnosztikai információkat** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="34026-136">Select hello **Share diagnostic information** check box.</span></span>
6. <span data-ttu-id="34026-137">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="34026-137">Click **Next**.</span></span> <span data-ttu-id="34026-138">Hello **elérhetőségi adatai** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="34026-138">hello **Contact information** dialog box appears.</span></span>
   
    ![Új támogatási kérelem ablaktábla](./media/storsimple-contact-microsoft-support/Ibiza6a.png) 
7. <span data-ttu-id="34026-140">Adja meg a kapcsolattartási adatait, és válasszon ki egy kapcsolattartási módszert, (telefonon vagy e-mailek).</span><span class="sxs-lookup"><span data-stu-id="34026-140">Enter your contact information and select a contact method (phone or email).</span></span> 
8. <span data-ttu-id="34026-141">Jelölje be hello **kapcsolattartási adatok módosításainak mentése az a későbbi támogatási kérelmekhez** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="34026-141">Select hello **Save contact changes for future support requests** check box.</span></span>
9. <span data-ttu-id="34026-142">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="34026-142">Click **Create**.</span></span>

<span data-ttu-id="34026-143">Miután elküldte a kérést, a támogatási szakértőhöz felveszi Önnel a lehető leghamarabb tooproceed a kérelemhez.</span><span class="sxs-lookup"><span data-stu-id="34026-143">After you have submitted your request, a Support engineer will contact you as soon as possible tooproceed with your request.</span></span>

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a><span data-ttu-id="34026-144">Támogatási munkamenetet indítani a Windows PowerShellben a StorSimple</span><span class="sxs-lookup"><span data-stu-id="34026-144">Start a support session in Windows PowerShell for StorSimple</span></span>
<span data-ttu-id="34026-145">bármely ad ki, hogy tootroubleshoot hello StorSimple eszköz tapasztalhat, hello Microsoft Support csapattal tooengage kell.</span><span class="sxs-lookup"><span data-stu-id="34026-145">tootroubleshoot any issues that you might experience with hello StorSimple device, you will need tooengage with hello Microsoft Support team.</span></span> <span data-ttu-id="34026-146">Előfordulhat, hogy a Microsoft Support toouse egy támogatási munkamenet toolog tooyour eszközön.</span><span class="sxs-lookup"><span data-stu-id="34026-146">Microsoft Support may need toouse a support session toolog on tooyour device.</span></span> 

<span data-ttu-id="34026-147">Hajtsa végre a következő hello támogatási munkamenet toostart lépések:</span><span class="sxs-lookup"><span data-stu-id="34026-147">Perform hello following steps toostart a support session:</span></span>

#### <a name="toostart-a-support-session"></a><span data-ttu-id="34026-148">egy támogatási munkamenet toostart</span><span class="sxs-lookup"><span data-stu-id="34026-148">toostart a support session</span></span>
1. <span data-ttu-id="34026-149">Hozzáférés hello eszköz általi közvetlen soros konzolon hello vagy egy telnet-munkamenet távoli számítógépről.</span><span class="sxs-lookup"><span data-stu-id="34026-149">Access hello device directly by using hello serial console or through a telnet session from a remote computer.</span></span> <span data-ttu-id="34026-150">toodo a, hajtsa végre hello lépéseit [PuTTY használata tooconnect toohello eszköz soros konzoljához](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="34026-150">toodo this, follow hello steps in [Use PuTTY tooconnect toohello device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
2. <span data-ttu-id="34026-151">A megnyitott munkamenetben hello, nyomja le a hello **Enter** kulcs tooget egy parancssori ablakot.</span><span class="sxs-lookup"><span data-stu-id="34026-151">In hello session that opens, press hello **Enter** key tooget a command prompt.</span></span>
3. <span data-ttu-id="34026-152">Hello soros konzol menüben válassza az 1. lehetőség – **jelentkezzen be a teljes körű hozzáférési**.</span><span class="sxs-lookup"><span data-stu-id="34026-152">In hello serial console menu, select option 1, **Log in with full access**.</span></span>
4. <span data-ttu-id="34026-153">Hello parancssorba írja be a következő jelszó hello:</span><span class="sxs-lookup"><span data-stu-id="34026-153">At hello prompt, type hello following password:</span></span> 
   
    `Password1`
5. <span data-ttu-id="34026-154">Hello parancssorba írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="34026-154">At hello prompt, type hello following command:</span></span>
   
    `Enable-HcsSupportAccess`
6. <span data-ttu-id="34026-155">Egy titkosított karakterláncot tooyou jelenik.</span><span class="sxs-lookup"><span data-stu-id="34026-155">An encrypted string will be presented tooyou.</span></span> <span data-ttu-id="34026-156">Másolja ezt a karakterláncot egy szövegszerkesztőbe, például a Jegyzettömböt.</span><span class="sxs-lookup"><span data-stu-id="34026-156">Copy this string into a text editor such as Notepad.</span></span>
7. <span data-ttu-id="34026-157">Mentse ezt a karakterláncot, és küldje el a egy e-mail üzenet tooMicrosoft támogatása.</span><span class="sxs-lookup"><span data-stu-id="34026-157">Save this string and send it in an email message tooMicrosoft Support.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="34026-158">Letilthatja a támogatás elérését futtatásával `Disable-HcsSupportAccess`.</span><span class="sxs-lookup"><span data-stu-id="34026-158">You can disable support access by running `Disable-HcsSupportAccess`.</span></span> <span data-ttu-id="34026-159">hello StorSimple eszközt is megpróbál toodisable támogatási hozzáférés 8 órával azt követően hello munkamenetet kezdeményezett.</span><span class="sxs-lookup"><span data-stu-id="34026-159">hello StorSimple device will also attempt toodisable support access 8 hours after hello session was initiated.</span></span> <span data-ttu-id="34026-160">A bevált gyakorlat toochange a StorSimple eszköz felhasználó hitelesítő adatait, a támogatási munkamenet kezdeményezése után.</span><span class="sxs-lookup"><span data-stu-id="34026-160">It is a best practice toochange your StorSimple device credentials after initiating a support session.</span></span>
> 
> 

