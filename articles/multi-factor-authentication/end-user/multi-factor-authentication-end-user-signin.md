---
title: "többtényezős hitelesítés aaaAzure bejelentkezhet a kétlépéses ellenőrzéshez használttal |} Microsoft Docs"
description: "Ezen az oldalon pedig akkor útmutatást nyújtanak ahol toogo toosee hello különböző bejelentkezési módszer áll rendelkezésre az Azure MFA Használatát."
keywords: "felhasználó hitelesítése, a bejelentkezés során tapasztal élmény, jelentkezzen be a mobiltelefon, bejelentkezés az irodai telefon"
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: b310b762-471b-4b26-887a-a321c9e81d46
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2017
ms.author: kgremban
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: fcd5eb5e8426eda537db9e099bf247bde29c195b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="hello-sign-in-experience-with-azure-multi-factor-authentication"></a><span data-ttu-id="b2430-104">Azure multi-factor Authentication hello bejelentkezési élmény</span><span class="sxs-lookup"><span data-stu-id="b2430-104">hello sign-in experience with Azure Multi-Factor Authentication</span></span>
> [!NOTE]
> <span data-ttu-id="b2430-105">hello Ez a cikk célja toowalk egy szokásos bejelentkezési élmény keresztül.</span><span class="sxs-lookup"><span data-stu-id="b2430-105">hello purpose of this article is toowalk through a typical sign-in experience.</span></span> <span data-ttu-id="b2430-106">Jelentkezzen be, vagy tootroubleshoot problémák kapcsolatban lásd: [problémák adódtak a Azure multi-factor Authentication](multi-factor-authentication-end-user-troubleshoot.md).</span><span class="sxs-lookup"><span data-stu-id="b2430-106">For help with signing in, or tootroubleshoot problems, see [Having trouble with Azure Multi-Factor Authentication](multi-factor-authentication-end-user-troubleshoot.md).</span></span>

## <a name="what-will-your-sign-in-experience-be"></a><span data-ttu-id="b2430-107">Mi lesz a bejelentkezés során tapasztal élmény?</span><span class="sxs-lookup"><span data-stu-id="b2430-107">What will your sign-in experience be?</span></span>
<span data-ttu-id="b2430-108">A bejelentkezés során tapasztal élmény eltér a kiválasztott beállításoktól függően toouse a második tényezőként: telefonhívást, hitelesítési alkalmazást vagy szövegét.</span><span class="sxs-lookup"><span data-stu-id="b2430-108">Your sign-in experience differs depending on what you choose toouse as your second factor: a phone call, an authentication app, or texts.</span></span> <span data-ttu-id="b2430-109">Válassza ki, amely a leginkább illik az Ön tevékenységeit hello beállítást:</span><span class="sxs-lookup"><span data-stu-id="b2430-109">Choose hello option that best describes what you are doing:</span></span>

| <span data-ttu-id="b2430-110">Hogyan bejelentkezik az?</span><span class="sxs-lookup"><span data-stu-id="b2430-110">How do you sign in?</span></span> | 
| --- |
| [<span data-ttu-id="b2430-111">Telefonhívás toomy mobileszköz vagy irodai telefon</span><span class="sxs-lookup"><span data-stu-id="b2430-111">With a phone call toomy mobile or office phone</span></span>](#signing-in-with-a-phone-call) |
| [<span data-ttu-id="b2430-112">A szöveg toomy mobiltelefon</span><span class="sxs-lookup"><span data-stu-id="b2430-112">With a text toomy mobile phone</span></span>](#signing-in-with-a-text-message)
| [<span data-ttu-id="b2430-113">Értesítések hello Microsoft Authenticator alkalmazásról</span><span class="sxs-lookup"><span data-stu-id="b2430-113">With notifications from hello Microsoft Authenticator app</span></span>](#signing-in-with-the-microsoft-authenticator-app-using-notification) |
| [<span data-ttu-id="b2430-114">Az ellenőrző kódok kezelésére hello Microsoft Authenticator alkalmazásról</span><span class="sxs-lookup"><span data-stu-id="b2430-114">With verification codes from hello Microsoft Authenticator app</span></span>](#signing-in-with-the-microsoft-authenticator-app-using-verification-code) |
| [<span data-ttu-id="b2430-115">Az alternatív módszert mert jelenleg nem használható a kívánt módszer</span><span class="sxs-lookup"><span data-stu-id="b2430-115">With an alternate method, because I can't use my preferred method right now</span></span>](#signing-in-with-an-alternate-method) |

## <a name="signing-in-with-a-phone-call"></a><span data-ttu-id="b2430-116">Telefonhívás bejelentkezni</span><span class="sxs-lookup"><span data-stu-id="b2430-116">Signing in with a phone call</span></span>
<span data-ttu-id="b2430-117">a következő információk hello hello kétlépéses ellenőrzés kipróbálhatja egy hívás tooyour mobilalkalmazás vagy az irodai telefon ismerteti.</span><span class="sxs-lookup"><span data-stu-id="b2430-117">hello following information describes hello two-step verification experience with a call tooyour mobile or office phone.</span></span>

1. <span data-ttu-id="b2430-118">Jelentkezzen be a tooan alkalmazás vagy szolgáltatás, például az Office 365 a felhasználónévvel és jelszóval.</span><span class="sxs-lookup"><span data-stu-id="b2430-118">Sign in tooan application or service such as Office 365 using your username and password.</span></span>  
2. <span data-ttu-id="b2430-119">Microsoft hívja meg.</span><span class="sxs-lookup"><span data-stu-id="b2430-119">Microsoft calls you.</span></span>  
3. <span data-ttu-id="b2430-120">Hello phone választ, majd nyomja le az hello # gombját.</span><span class="sxs-lookup"><span data-stu-id="b2430-120">Answer hello phone and hit hello # key.</span></span>  

## <a name="signing-in-with-a-text-message"></a><span data-ttu-id="b2430-121">Bejelentkezés a szöveges üzenet</span><span class="sxs-lookup"><span data-stu-id="b2430-121">Signing in with a text message</span></span>
<span data-ttu-id="b2430-122">a következő információk hello a szöveges üzenet tooyour mobiltelefon hello kétlépéses ellenőrzés élményt ismerteti:</span><span class="sxs-lookup"><span data-stu-id="b2430-122">hello following information describes hello two-step verification experience with a text message tooyour mobile phone:</span></span>

1. <span data-ttu-id="b2430-123">Jelentkezzen be a tooan alkalmazás vagy szolgáltatás, például az Office 365 a felhasználónévvel és jelszóval.</span><span class="sxs-lookup"><span data-stu-id="b2430-123">Sign in tooan application or service such as Office 365 using your username and password.</span></span> 
2. <span data-ttu-id="b2430-124">A Microsoft egy kódot tartalmazó szöveges üzenetet küld.</span><span class="sxs-lookup"><span data-stu-id="b2430-124">Microsoft sends you a text message that contains a number code.</span></span> 
3. <span data-ttu-id="b2430-125">Hello a Bejelentkezés lapon található hello mezőbe írja be a hello kódot.</span><span class="sxs-lookup"><span data-stu-id="b2430-125">Enter hello code in hello box provided on hello sign-in page.</span></span> 

## <a name="signing-in-with-hello-microsoft-authenticator-app"></a><span data-ttu-id="b2430-126">Bejelentkezés hello Microsoft Authenticator alkalmazásról</span><span class="sxs-lookup"><span data-stu-id="b2430-126">Signing in with hello Microsoft Authenticator app</span></span> 
<span data-ttu-id="b2430-127">hello következő ismerteti hello élménye hello Microsoft Authenticator alkalmazással a kétlépéses ellenőrzést.</span><span class="sxs-lookup"><span data-stu-id="b2430-127">hello following information describes hello experience of using hello Microsoft Authenticator app for two-step verifications.</span></span> <span data-ttu-id="b2430-128">Nincsenek két különböző módon toouse hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b2430-128">There are two different ways toouse hello app.</span></span> <span data-ttu-id="b2430-129">Leküldéses értesítéseket kaphat az eszközön, illetve megnyithat hello app tooget ellenőrző kódot.</span><span class="sxs-lookup"><span data-stu-id="b2430-129">You can receive push notifications on your device, or you can open hello app tooget a verification code.</span></span>

### <a name="toosign-in-with-a-notification-from-hello-microsoft-authenticator-app"></a><span data-ttu-id="b2430-130">hello Microsoft Authenticator alkalmazás értesítéseinek be toosign</span><span class="sxs-lookup"><span data-stu-id="b2430-130">toosign in with a notification from hello Microsoft Authenticator app</span></span>
1. <span data-ttu-id="b2430-131">Jelentkezzen be a tooan alkalmazás vagy szolgáltatás, például az Office 365 a felhasználónévvel és jelszóval.</span><span class="sxs-lookup"><span data-stu-id="b2430-131">Sign in tooan application or service such as Office 365 using your username and password.</span></span>
2. <span data-ttu-id="b2430-132">Microsoft küld egy értesítési toohello Microsoft Authenticator alkalmazást az eszközön.</span><span class="sxs-lookup"><span data-stu-id="b2430-132">Microsoft sends a notification toohello Microsoft Authenticator app on your device.</span></span>

  ![Microsoft értesítést küld.](./media/multi-factor-authentication-end-user-signin/notify.png)

3. <span data-ttu-id="b2430-134">Nyissa meg hello értesítést a telefon és select hello **ellenőrizze** kulcs.</span><span class="sxs-lookup"><span data-stu-id="b2430-134">Open hello notification on your phone and select hello **Verify** key.</span></span> <span data-ttu-id="b2430-135">Ha a vállalat a PIN-kód szükséges, írja be ide.</span><span class="sxs-lookup"><span data-stu-id="b2430-135">If your company requires a PIN, enter it here.</span></span>
4. <span data-ttu-id="b2430-136">Meg kell a bejelentkezés megtörtént.</span><span class="sxs-lookup"><span data-stu-id="b2430-136">You should now be signed in.</span></span>

### <a name="toosign-in-using-a-verification-code-with-hello-microsoft-authenticator-app"></a><span data-ttu-id="b2430-137">az ellenőrző kód használata hello Microsoft Authenticator alkalmazás toosign</span><span class="sxs-lookup"><span data-stu-id="b2430-137">toosign in using a verification code with hello Microsoft Authenticator app</span></span>

<span data-ttu-id="b2430-138">Hello Microsoft Authenticator alkalmazás tooget ellenőrző kódok kezelésére használatakor majd hello alkalmazás megnyitásakor megjelenik egy számot a fiók alatt.</span><span class="sxs-lookup"><span data-stu-id="b2430-138">If you use hello Microsoft Authenticator app tooget verification codes, then when you open hello app you see a number under your account name.</span></span> <span data-ttu-id="b2430-139">Ez a szám 30 másodpercenként úgy módosítja, nem adja meg kétszer ugyanazt a number hello.</span><span class="sxs-lookup"><span data-stu-id="b2430-139">This number changes every 30 seconds so that you don't use hello same number twice.</span></span> <span data-ttu-id="b2430-140">Ha az ellenőrző kódot megkérdezi, nyissa meg hello alkalmazást, és használja a szám szerint jelenleg jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b2430-140">When you're asked for a verification code, open hello app and use whatever number is currently displayed.</span></span> 

1. <span data-ttu-id="b2430-141">Jelentkezzen be a tooan alkalmazás vagy szolgáltatás, például az Office 365 a felhasználónévvel és jelszóval.</span><span class="sxs-lookup"><span data-stu-id="b2430-141">Sign in tooan application or service such as Office 365 using your username and password.</span></span>
2. <span data-ttu-id="b2430-142">A Microsoft ellenőrző kódot kér.</span><span class="sxs-lookup"><span data-stu-id="b2430-142">Microsoft prompts you for a verification code.</span></span>

  ![Adja meg](./media/multi-factor-authentication-end-user-signin/verify3.png)

3. <span data-ttu-id="b2430-144">Nyissa meg a hello Microsoft Authenticator alkalmazást a telefonra, és írja be a hello kódot bejelentkezik a hello mezőbe.</span><span class="sxs-lookup"><span data-stu-id="b2430-144">Open hello Microsoft Authenticator app on your phone and enter hello code in hello box where you are signing in.</span></span>

## <a name="signing-in-with-an-alternate-method"></a><span data-ttu-id="b2430-145">Alternatív módszert bejelentkezni</span><span class="sxs-lookup"><span data-stu-id="b2430-145">Signing in with an alternate method</span></span>
<span data-ttu-id="b2430-146">Egyes esetekben nincs hello telefon vagy eszköz, amely akkor állíthatja be az előnyben részesített ellenőrzési módszert.</span><span class="sxs-lookup"><span data-stu-id="b2430-146">Sometimes you don't have hello phone or device that you set up as your preferred verification method.</span></span> <span data-ttu-id="b2430-147">Ebben az esetben is, ezért azt javasoljuk, hogy beállított biztonsági mentési módszerek a fiókjához.</span><span class="sxs-lookup"><span data-stu-id="b2430-147">This situation is why we recommend that you set up backup methods for your account.</span></span> <span data-ttu-id="b2430-148">hello következő szakasz bemutatja, hogyan toosign be egy alternatív módszert, ha az elsődleges módszer nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="b2430-148">hello following section shows you how toosign in with an alternate method when your primary method may not be available.</span></span>

1. <span data-ttu-id="b2430-149">Jelentkezzen be a tooan alkalmazás vagy szolgáltatás, például az Office 365 a felhasználónévvel és jelszóval.</span><span class="sxs-lookup"><span data-stu-id="b2430-149">Sign in tooan application or service such as Office 365 using your username and password.</span></span>
2. <span data-ttu-id="b2430-150">Válassza ki **használja egy másik ellenőrzési módszerrel**.</span><span class="sxs-lookup"><span data-stu-id="b2430-150">Select **Use a different verification option**.</span></span> <span data-ttu-id="b2430-151">Megjelenik a telepítés hogy hány másik ellenőrzési lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="b2430-151">You see different verification options based on how many you have setup.</span></span>
3. <span data-ttu-id="b2430-152">Válasszon egy másik módszert, és jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="b2430-152">Choose an alternate method and sign in.</span></span>

  ![Alternatív módszert használja.](./media/multi-factor-authentication-end-user-signin/alt.png)

## <a name="next-steps"></a><span data-ttu-id="b2430-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b2430-154">Next steps</span></span>

<span data-ttu-id="b2430-155">Ha bejelentkezik a kétlépéses ellenőrzést, akkor további információért [problémák adódtak a Azure multi-factor Authentication](multi-factor-authentication-end-user-troubleshoot.md).</span><span class="sxs-lookup"><span data-stu-id="b2430-155">If you have problems signing in with two-step verification, get more information at [Having trouble with Azure Multi-Factor Authentication](multi-factor-authentication-end-user-troubleshoot.md).</span></span>

<span data-ttu-id="b2430-156">Ismerje meg, hogyan túl[kezelése a kétlépéses ellenőrzés beállításait](multi-factor-authentication-end-user-manage-settings.md).</span><span class="sxs-lookup"><span data-stu-id="b2430-156">Learn how too[Manage your two-step verification settings](multi-factor-authentication-end-user-manage-settings.md).</span></span>

<span data-ttu-id="b2430-157">Megtudhatja, hogyan túl[Ismerkedés a Microsoft Authenticator alkalmazást hello](microsoft-authenticator-app-how-to.md) , hogy az értesítések toosign-szövegek és telefonhívásokat helyett használhat.</span><span class="sxs-lookup"><span data-stu-id="b2430-157">Find out how too[Get started with hello Microsoft Authenticator app](microsoft-authenticator-app-how-to.md) so that you can use notifications toosign in, instead of texts and phone calls.</span></span> 
