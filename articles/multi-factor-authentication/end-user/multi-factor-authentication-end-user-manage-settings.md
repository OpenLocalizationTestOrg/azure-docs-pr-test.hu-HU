---
title: "aaaManage a kétlépéses ellenőrzés beállításait |} Microsoft Docs"
description: "Kezelése, beleértve a kapcsolattartási adatok módosítása vagy az eszközök konfigurálása Azure multi-factor Authentication használatát."
services: multi-factor-authentication
keywords: "többtényezős hitelesítés ügyfél, hitelesítési probléma korrelációs azonosító"
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: d3372d9a-9ad1-4609-bdcf-2c4ca9679a3b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: kgremban
ms.custom: end-user
ms.openlocfilehash: 2c974b08c584943f3c5a6b9bf16497d1706e8329
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-settings-for-two-step-verification"></a><span data-ttu-id="c1418-104">A kétlépéses ellenőrzést beállításainak kezelése</span><span class="sxs-lookup"><span data-stu-id="c1418-104">Manage your settings for two-step verification</span></span>
<span data-ttu-id="c1418-105">Ez a cikk kapcsolatos kérdésekre ad választ tooupdate beállítások kétlépéses ellenőrzés vagy a multi-factor Authentication hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="c1418-105">This article answers questions about how tooupdate settings for two-step verification or multi-factor authentication.</span></span> <span data-ttu-id="c1418-106">Ha bejelentkezik tooyour fiók problémát tapasztal, tekintse meg a túl[problémák adódtak a kétlépéses ellenőrzéshez használttal](multi-factor-authentication-end-user-troubleshoot.md) hibaelhárítási segítséget.</span><span class="sxs-lookup"><span data-stu-id="c1418-106">If you are having issues signing in tooyour account, refer too[Having trouble with two-step verification](multi-factor-authentication-end-user-troubleshoot.md) for troubleshooting help.</span></span>

## <a name="where-toofind-hello-settings-page"></a><span data-ttu-id="c1418-107">Ha toofind hello beállítások lap</span><span class="sxs-lookup"><span data-stu-id="c1418-107">Where toofind hello settings page</span></span>
<span data-ttu-id="c1418-108">Attól függően, hogy vállalata hogyan Azure multi-factor Authentication beállítása van néhány olyan helyen, ahol módosíthatók a beállítások például a telefonszámát.</span><span class="sxs-lookup"><span data-stu-id="c1418-108">Depending on how your company set up Azure Multi-Factor Authentication, there are a few places where you can change your settings like your phone number.</span></span>

<span data-ttu-id="c1418-109">Ha a vállalat informatikai rendszergazdája egy adott URL-cím vagy lépéseket toomanage kétlépéses ellenőrzés kiküldött, kövesse ezeket az utasításokat.</span><span class="sxs-lookup"><span data-stu-id="c1418-109">If your IT admin sent out a specific URL or steps toomanage two-step verification, follow those instructions.</span></span> <span data-ttu-id="c1418-110">Ellenkező esetben az utasításoknak hello a Mindenki más kell működnie.</span><span class="sxs-lookup"><span data-stu-id="c1418-110">Otherwise, hello following instructions should work for everybody else.</span></span> <span data-ttu-id="c1418-111">Ha kövesse az alábbi lépéseket, de nem jelenik meg hello ugyanazokkal a beállításokkal, amely azt jelenti, hogy a munkahelyi vagy iskolai testreszabott saját portálon.</span><span class="sxs-lookup"><span data-stu-id="c1418-111">If you follow these steps but don't see hello same options, that means that your work or school customized their own portal.</span></span> <span data-ttu-id="c1418-112">Kérje meg a rendszergazdát a hello hivatkozás tooyour Azure multi-factor Authentication portálon.</span><span class="sxs-lookup"><span data-stu-id="c1418-112">Ask your admin for hello link tooyour Azure Multi-Factor Authentication portal.</span></span>

1. <span data-ttu-id="c1418-113">Jelentkezzen be a túl[https://myapps.microsoft.com](https://myapps.microsoft.com)</span><span class="sxs-lookup"><span data-stu-id="c1418-113">Sign in too[https://myapps.microsoft.com](https://myapps.microsoft.com)</span></span>  
2. <span data-ttu-id="c1418-114">Hello jobb felső jelölje be a fiók nevére, majd válassza ki **profil**.</span><span class="sxs-lookup"><span data-stu-id="c1418-114">Select your account name in hello top right, then select **profile**.</span></span>  
3. <span data-ttu-id="c1418-115">Válassza ki **további biztonsági ellenőrzés**.</span><span class="sxs-lookup"><span data-stu-id="c1418-115">Select **Additional security verification**.</span></span>  

    ![Myapps](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. <span data-ttu-id="c1418-117">hello további biztonsági ellenőrzési lapot betölti a beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="c1418-117">hello Additional security verification page loads with your settings.</span></span>

    ![Proofup](./media/multi-factor-authentication-end-user-manage/proofup.png)

## <a name="i-want-toochange-my-phone-number-or-add-a-secondary-number"></a><span data-ttu-id="c1418-119">Toochange telefonszámmal, és nem sikerül egy másodlagos szám hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c1418-119">I want toochange my phone number, or add a secondary number</span></span>
<span data-ttu-id="c1418-120">Fontos tooconfigure egy másodlagos hitelesítéshez használni kívánt telefonszámot is.</span><span class="sxs-lookup"><span data-stu-id="c1418-120">It is important tooconfigure a secondary authentication phone number.</span></span>  <span data-ttu-id="c1418-121">Mivel az elsődleges telefonszám száma és a mobilalkalmazás vannak valószínűleg a hello azonos telefon, hello másodlagos telefonszám hello csak úgy fogja tudni tooget visszaállítása fiókjába Ha telefonja elveszett vagy ellopták.</span><span class="sxs-lookup"><span data-stu-id="c1418-121">Because your primary phone number and your mobile app are probably on hello same phone, hello secondary phone number is hello only way you will be able tooget back into your account if your phone is lost or stolen.</span></span>

> [!NOTE]
> <span data-ttu-id="c1418-122">Ha nem rendelkezik hozzáférési tooyour elsődleges telefonszámot, és tooyour fiók első segítségre van szüksége, tekintse meg a súgótémakörök [problémák adódtak a kétlépéses ellenőrzéshez használttal](multi-factor-authentication-end-user-troubleshoot.md).</span><span class="sxs-lookup"><span data-stu-id="c1418-122">If you don't have access tooyour primary phone number, and need help getting in tooyour account, see our help topics in [Having trouble with two-step verification](multi-factor-authentication-end-user-troubleshoot.md).</span></span>  

<span data-ttu-id="c1418-123">**toochange az elsődleges telefonszám:**</span><span class="sxs-lookup"><span data-stu-id="c1418-123">**toochange your primary phone number:**</span></span>  

1. <span data-ttu-id="c1418-124">Hello további biztonsági ellenőrzési lapot jelölje ki a hello szövegmezőbe az aktuális telefonszámát és szerkesztheti az új telefonszámot.</span><span class="sxs-lookup"><span data-stu-id="c1418-124">On hello Additional security verification page, select hello text box with your current phone number and edit it with your new phone number.</span></span>  
2. <span data-ttu-id="c1418-125">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="c1418-125">Select **Save**.</span></span>  
3. <span data-ttu-id="c1418-126">Ez a kedvenc hitelesítési lehetőségnek a használt hello számát, lehetősége van tooverify hello új szám előtt mentse azt.</span><span class="sxs-lookup"><span data-stu-id="c1418-126">If this is hello number that you use for your preferred verification option, you have tooverify hello new number before you can save it.</span></span>  

<span data-ttu-id="c1418-127">**egy másodlagos telefonszámot tooadd:**</span><span class="sxs-lookup"><span data-stu-id="c1418-127">**tooadd a secondary phone number:**</span></span>  

1. <span data-ttu-id="c1418-128">Az oldalon hello további biztonsági ellenőrzési jelölőnégyzetet hello mellett túl**másik hitelesítési phone.**</span><span class="sxs-lookup"><span data-stu-id="c1418-128">On hello Additional security verification page, check hello box next too**Alternate authentication phone.**</span></span>  
2. <span data-ttu-id="c1418-129">Hello mezőben adja meg a másodlagos telefonszámát.</span><span class="sxs-lookup"><span data-stu-id="c1418-129">Enter your secondary phone number in hello text box.</span></span>  
3. <span data-ttu-id="c1418-130">Válassza ki **mentése** és a módosítások befejezte volna.</span><span class="sxs-lookup"><span data-stu-id="c1418-130">Select **Save** and your changes are finished.</span></span>  

## <a name="require-two-step-verification-again-on-a-device-youve-marked-as-trusted"></a><span data-ttu-id="c1418-131">A megbízhatóként megjelölt eszközön újra kétlépéses ellenőrzés megkövetelése</span><span class="sxs-lookup"><span data-stu-id="c1418-131">Require two-step verification again on a device you've marked as trusted</span></span>

<span data-ttu-id="c1418-132">A szervezet beállításoktól függően előfordulhat, hogy a jelölőnégyzet, amely szerint "Ne jelenjen meg többé a **X** nap" a böngésző kétlépéses ellenőrzés végrehajtásakor.</span><span class="sxs-lookup"><span data-stu-id="c1418-132">Depending on your organization settings, you may have a checkbox that says "Don't ask again for **X** days" when you perform two-step verification on your browser.</span></span> <span data-ttu-id="c1418-133">Ha ezt a jelölőnégyzetet és majd az eszköz elvesztése vagy gondolja, hogy a fiók biztonsága sérül, az eszközök kétlépéses ellenőrzés tooall kell visszaállítani.</span><span class="sxs-lookup"><span data-stu-id="c1418-133">If you check this box and then lose your device or think that your account is compromised, you should restore two-step verification tooall your devices.</span></span> 

1. <span data-ttu-id="c1418-134">Hello további biztonsági ellenőrzési lapot, jelölje ki **visszaállítási többtényezős hitelesítést a korábban megbízhatónak minősített eszközökön**.</span><span class="sxs-lookup"><span data-stu-id="c1418-134">On hello Additional security verification page, select **Restore multi-factor authentication on previously trusted devices**.</span></span>
2. <span data-ttu-id="c1418-135">hello bejelentkezik bármilyen eszközön, amikor legközelebb lesz felszólító tooperform kétlépéses ellenőrzést.</span><span class="sxs-lookup"><span data-stu-id="c1418-135">hello next time you sign in on any device, you'll be prompted tooperform two-step verification.</span></span> 

## <a name="how-do-i-clean-up-microsoft-authenticator-from-my-old-device-and-move-tooa-new-one"></a><span data-ttu-id="c1418-136">Hogyan Microsoft Authenticator tisztítása a régi eszközről és helyezze át a tooa újat?</span><span class="sxs-lookup"><span data-stu-id="c1418-136">How do I clean up Microsoft Authenticator from my old device and move tooa new one?</span></span>
<span data-ttu-id="c1418-137">Hello alkalmazást az eszközről, vagy alaphelyzetbe állítása hello eszköz eltávolításakor hello aktiválási hello háttérben futó nem távolítja el.</span><span class="sxs-lookup"><span data-stu-id="c1418-137">When you uninstall hello app from your device or reset hello device, it does not remove hello activation on hello back end.</span></span> <span data-ttu-id="c1418-138">További információkért lásd: [Microsoft Authenticator](microsoft-authenticator-app-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="c1418-138">For more information, see [Microsoft Authenticator](microsoft-authenticator-app-how-to.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c1418-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c1418-139">Next steps</span></span>
* <span data-ttu-id="c1418-140">Hibaelhárítási tippeket és súgó [problémák adódtak a kétlépéses ellenőrzéshez használttal](multi-factor-authentication-end-user-troubleshoot.md)</span><span class="sxs-lookup"><span data-stu-id="c1418-140">Get troubleshooting tips and help on [Having trouble with two-step verification](multi-factor-authentication-end-user-troubleshoot.md)</span></span>
* <span data-ttu-id="c1418-141">Állítson be [alkalmazásjelszók](multi-factor-authentication-end-user-app-passwords.md) alkalmazások, amelyek nem támogatják a kétlépéses ellenőrzést.</span><span class="sxs-lookup"><span data-stu-id="c1418-141">Set up [app passwords](multi-factor-authentication-end-user-app-passwords.md) for any apps that don't support two-step verification.</span></span>
