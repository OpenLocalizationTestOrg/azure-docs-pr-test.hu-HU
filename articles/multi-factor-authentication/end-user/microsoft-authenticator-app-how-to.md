---
title: "a mobiltelefonok aaaMicrosoft hitelesítőalkalmazása |} Microsoft Docs"
description: "Megtudhatja, hogyan tooupgrade toohello legújabb verziójára Azure Authenticator."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 3065a1ee-f253-41f0-a68d-2bd84af5ffba
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: kgremban
ms.reviewer: librown
ms.custom: H1Hack27Feb2017, end-user
ms.openlocfilehash: d895d92d89613cbafd9fc09d4ff4810cbf25652e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-microsoft-authenticator-app"></a><span data-ttu-id="91115-103">Ismerkedés a hello Microsoft Authenticator alkalmazásról</span><span class="sxs-lookup"><span data-stu-id="91115-103">Get started with hello Microsoft Authenticator app</span></span>
<span data-ttu-id="91115-104">hello Microsoft Authenticator alkalmazást egy további szintű biztonságot nyújt a munkahelyi vagy iskolai fiókját (például bsimon@contoso.com) vagy a Microsoft-fiókját (például bsimon@outlook.com).</span><span class="sxs-lookup"><span data-stu-id="91115-104">hello Microsoft Authenticator app provides an additional level of security in your work or school account (for example, bsimon@contoso.com) or your Microsoft account (for example, bsimon@outlook.com).</span></span>

<span data-ttu-id="91115-105">hello az alkalmazás akkor működik az alábbi két módszer egyikével:</span><span class="sxs-lookup"><span data-stu-id="91115-105">hello app works in one of two ways:</span></span>

* <span data-ttu-id="91115-106">**Értesítési**.</span><span class="sxs-lookup"><span data-stu-id="91115-106">**Notification**.</span></span> <span data-ttu-id="91115-107">hello alkalmazás megakadályozza a jogosulatlan hozzáférés tooaccounts, és állítsa le a csalárd tranzakciókat egy értesítési tooyour okostelefonján vagy táblagépén küldésével segítségével.</span><span class="sxs-lookup"><span data-stu-id="91115-107">hello app can help prevent unauthorized access tooaccounts and stop fraudulent transactions by pushing a notification tooyour smartphone or tablet.</span></span> <span data-ttu-id="91115-108">Egyszerűen hello értesítést, és ha az megbízható, választhatja ki **ellenőrizze**.</span><span class="sxs-lookup"><span data-stu-id="91115-108">Simply view hello notification, and if it's legitimate, select **Verify**.</span></span> <span data-ttu-id="91115-109">Ellenkező esetben kiválaszthatja **Megtagadás**.</span><span class="sxs-lookup"><span data-stu-id="91115-109">Otherwise, you can select **Deny**.</span></span> 
* <span data-ttu-id="91115-110">**Az ellenőrző kódot**.</span><span class="sxs-lookup"><span data-stu-id="91115-110">**Verification code**.</span></span> <span data-ttu-id="91115-111">hello alkalmazás használhat egy szoftverfrissítési token toogenerate az OAuth-ellenőrző kódot.</span><span class="sxs-lookup"><span data-stu-id="91115-111">hello app can be used as a software token toogenerate an OAuth verification code.</span></span> <span data-ttu-id="91115-112">Miután megadta a felhasználónevét és jelszavát, meg kell adnia a hello bejelentkezési képernyő hello alkalmazás által biztosított hello kódot.</span><span class="sxs-lookup"><span data-stu-id="91115-112">After you enter your username and password, you enter hello code provided by hello app into hello sign-in screen.</span></span> <span data-ttu-id="91115-113">hello ellenőrzőkódot hitelesítés második formáját nyújtja.</span><span class="sxs-lookup"><span data-stu-id="91115-113">hello verification code provides a second form of authentication.</span></span>

<span data-ttu-id="91115-114">hello Microsoft Authenticator alkalmazást a felváltja hello Azure Authenticator alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="91115-114">hello Microsoft Authenticator app replaces hello Azure Authenticator app.</span></span> <span data-ttu-id="91115-115">hello Azure Authenticator alkalmazás továbbra is működik, de ha úgy dönt, hogy toomove toohello új Microsoft Authenticator alkalmazást, ez a cikk segítséget nyújthatnak.</span><span class="sxs-lookup"><span data-stu-id="91115-115">hello Azure Authenticator app still works, but if you decide toomove toohello new Microsoft Authenticator app, this article can assist you.</span></span>  

## <a name="opt-in-for-two-step-verification"></a><span data-ttu-id="91115-116">A kétlépéses ellenőrzéshez részt</span><span class="sxs-lookup"><span data-stu-id="91115-116">Opt in for two-step verification</span></span>

<span data-ttu-id="91115-117">hello Microsoft Authenticator alkalmazás önmagában nem működik.</span><span class="sxs-lookup"><span data-stu-id="91115-117">hello Microsoft Authenticator app doesn't work by itself.</span></span> <span data-ttu-id="91115-118">Konfigurálható, a fiókok tooprompt egy második hitelesítési módszert, miután a jelentkezik be a felhasználónevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="91115-118">Configure each of your accounts tooprompt you for a second verification method after you sign in with your username and password.</span></span> 

<span data-ttu-id="91115-119">A munkahelyi vagy iskolai fiókkal nem általában kap toochoose Ez a funkció a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="91115-119">For a work or school account, you don't usually get toochoose this feature for yourself.</span></span> <span data-ttu-id="91115-120">Ehelyett a biztonsági rendszergazda jóváhagyja a nevünkben, és majd értesíti, tooregister ellenőrzési módszereket a fiókjához.</span><span class="sxs-lookup"><span data-stu-id="91115-120">Instead, a security administrator opts in on your behalf and then notifies you tooregister verification methods for your account.</span></span> <span data-ttu-id="91115-121">Ebben a forgatókönyvben tooyou vonatkozik, ha további információk: [mit Azure multi-factor Authentication jelent a számomra](multi-factor-authentication-end-user.md).</span><span class="sxs-lookup"><span data-stu-id="91115-121">If this scenario applies tooyou, learn more in [What does Azure Multi-Factor Authentication mean for me](multi-factor-authentication-end-user.md).</span></span>

<span data-ttu-id="91115-122">Egy személyes fiók kell tooset fel saját maga a kétlépéses ellenőrzést.</span><span class="sxs-lookup"><span data-stu-id="91115-122">For a personal account, you need tooset up two-step verification for yourself.</span></span> <span data-ttu-id="91115-123">Ha a Microsoft-fiókkal rendelkezik, ezeket a lépéseket elérhetők-e a [tudnivalók a kétlépéses ellenőrzést](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).</span><span class="sxs-lookup"><span data-stu-id="91115-123">If you have a Microsoft account, those steps are available in [About two-step verification](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).</span></span> 

<span data-ttu-id="91115-124">A Microsoft Authenticator hello nem Microsoft-fiókok is használható.</span><span class="sxs-lookup"><span data-stu-id="91115-124">You can also use hello Microsoft Authenticator with non-Microsoft accounts.</span></span> <span data-ttu-id="91115-125">Előfordulhat, hogy meghívják hello szolgáltatás nem a kétlépéses ellenőrzést, de meg kell tudni toofind azt a biztonsági vagy bejelentkezési beállítások részben.</span><span class="sxs-lookup"><span data-stu-id="91115-125">They may call hello feature something other than two-step verification, but you should be able toofind it under security or sign-in settings.</span></span> 

## <a name="install-hello-app"></a><span data-ttu-id="91115-126">Hello alkalmazás telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="91115-126">Install hello app</span></span>
<span data-ttu-id="91115-127">hello Microsoft Authenticator alkalmazás érhető el [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), és [iOS](http://go.microsoft.com/fwlink/?Linkid=825073).</span><span class="sxs-lookup"><span data-stu-id="91115-127">hello Microsoft Authenticator app is available for [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), and [iOS](http://go.microsoft.com/fwlink/?Linkid=825073).</span></span>

## <a name="add-accounts-toohello-app"></a><span data-ttu-id="91115-128">Fiókok toohello alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="91115-128">Add accounts toohello app</span></span>
<span data-ttu-id="91115-129">Az egyes fiókokhoz, hogy szeretné-e tooadd toohello Microsoft Authenticator alkalmazást használja a következő eljárások hello egyikét:</span><span class="sxs-lookup"><span data-stu-id="91115-129">For each account that you want tooadd toohello Microsoft Authenticator app, use one of hello following procedures:</span></span>

### <a name="add-a-personal-microsoft-account-toohello-app"></a><span data-ttu-id="91115-130">Személyes Microsoft-fiók toohello alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="91115-130">Add a personal Microsoft account toohello app</span></span>

<span data-ttu-id="91115-131">Személyes Microsoft-fiók (egy toosign használt tooOutlook.com, Xbox, Skype, a stb.), mindössze meg kell toodo tooyour fiók hello Microsoft Authenticator alkalmazást a bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="91115-131">For a personal Microsoft account (one that you use toosign in tooOutlook.com, Xbox, Skype, etc.), all you have toodo is sign in tooyour account in hello Microsoft Authenticator app.</span></span>

### <a name="add-a-work-or-school-account-toohello-app-using-hello-qr-code-scanner"></a><span data-ttu-id="91115-132">Adja hozzá a munkahelyi vagy iskolai fiók toohello alkalmazást hello QR-kód képolvasó segítségével</span><span class="sxs-lookup"><span data-stu-id="91115-132">Add a work or school account toohello app using hello QR code scanner</span></span>
1. <span data-ttu-id="91115-133">Nyissa meg toohello biztonsági ellenőrzési beállítások képernyő.</span><span class="sxs-lookup"><span data-stu-id="91115-133">Go toohello security verification settings screen.</span></span>  <span data-ttu-id="91115-134">Információ tooget toothis képernyő, lásd: [a biztonsági beállítások módosítása](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page).</span><span class="sxs-lookup"><span data-stu-id="91115-134">For information on how tooget toothis screen, see [Changing your security settings](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page).</span></span>
2. <span data-ttu-id="91115-135">Jelölőnégyzetet hello mellett túl**hitelesítőalkalmazása** válassza **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="91115-135">Check hello box next too**Authenticator app** then select **Configure**.</span></span>

    ![hello biztonsági ellenőrzési beállítások képernyőjén hello gomb](./media/authenticator-app-how-to/azureauthe.png)

    <span data-ttu-id="91115-137">Ekkor megjelenik egy képernyőt rajta egy QR-kódot.</span><span class="sxs-lookup"><span data-stu-id="91115-137">This brings up a screen with a QR code on it.</span></span>

    ![Hello QR-kód biztosító képernyő](./media/authenticator-app-how-to/barcode2.png)
3. <span data-ttu-id="91115-139">Nyissa meg hello Microsoft Authenticator alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="91115-139">Open hello Microsoft Authenticator app.</span></span> <span data-ttu-id="91115-140">A hello **fiókok** képernyőn válassza ki  **+** , és adja meg, hogy szeretné-e tooadd munkahelyi vagy iskolai fiókkal.</span><span class="sxs-lookup"><span data-stu-id="91115-140">On hello **accounts** screen, select **+**, and then specify that you want tooadd a work or school account.</span></span>
4. <span data-ttu-id="91115-141">Hello kamera tooscan hello QR-kódot, és válassza **végzett** tooclose hello QR-kód képernyő.</span><span class="sxs-lookup"><span data-stu-id="91115-141">Use hello camera tooscan hello QR code, and then select **Done** tooclose hello QR code screen.</span></span>

    <span data-ttu-id="91115-142">Ha a kamera nem működik megfelelően, akkor [hello QR-kódot és URL-cím manuális megadása](#add-an-account-to-the-app-manually).</span><span class="sxs-lookup"><span data-stu-id="91115-142">If your camera is not working properly, you can [enter hello QR code and URL manually](#add-an-account-to-the-app-manually).</span></span>

5. <span data-ttu-id="91115-143">Hello alkalmazás egy hatjegyű kódot alatta a fiók nevét jeleníti meg, amikor elkészült.</span><span class="sxs-lookup"><span data-stu-id="91115-143">When hello app shows your account name with a six-digit code underneath it, you're done.</span></span> 

    ![Fiókok képernyő](./media/authenticator-app-how-to/accounts.png)

### <a name="add-an-account-toohello-app-manually"></a><span data-ttu-id="91115-145">Egy fiók toohello alkalmazás manuális hozzáadása</span><span class="sxs-lookup"><span data-stu-id="91115-145">Add an account toohello app manually</span></span>
1. <span data-ttu-id="91115-146">Nyissa meg toohello biztonsági ellenőrzési beállítások képernyő.</span><span class="sxs-lookup"><span data-stu-id="91115-146">Go toohello security verification settings screen.</span></span>  <span data-ttu-id="91115-147">Információ tooget toothis képernyő, lásd: [a biztonsági beállítások módosítása](multi-factor-authentication-end-user-manage-settings.md).</span><span class="sxs-lookup"><span data-stu-id="91115-147">For information on how tooget toothis screen, see [Changing your security settings](multi-factor-authentication-end-user-manage-settings.md).</span></span>
2. <span data-ttu-id="91115-148">Válassza ki **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="91115-148">Select **Configure**.</span></span>

    ![hello biztonsági ellenőrzési beállítások képernyőjén hello gomb](./media/authenticator-app-how-to/azureauthe.png)

    <span data-ttu-id="91115-150">Ekkor megjelenik egy képernyőt rajta egy QR-kódot.</span><span class="sxs-lookup"><span data-stu-id="91115-150">This brings up a screen with a QR code on it.</span></span>  <span data-ttu-id="91115-151">Megjegyzés: hello kódot és URL-címet.</span><span class="sxs-lookup"><span data-stu-id="91115-151">Note hello code and URL.</span></span>

    ![Hello QR-kódot és URL-címet biztosító képernyő](./media/authenticator-app-how-to/barcode2.png)
3. <span data-ttu-id="91115-153">Nyissa meg hello Microsoft Authenticator alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="91115-153">Open hello Microsoft Authenticator app.</span></span> <span data-ttu-id="91115-154">A hello **fiókok** képernyőn válassza ki  **+** , és adja meg, hogy szeretné-e tooadd munkahelyi vagy iskolai fiókkal.</span><span class="sxs-lookup"><span data-stu-id="91115-154">On hello **accounts** screen, select **+**, and then specify that you want tooadd a work or school account.</span></span>

4. <span data-ttu-id="91115-155">Hello képolvasó, válassza ki **írja be a kódot kézzel**.</span><span class="sxs-lookup"><span data-stu-id="91115-155">In hello scanner, select **enter code manually**.</span></span>

    ![A QR-kód vizsgálatának képernyő](./media/multi-factor-authentication-end-user-first-time/scan2.png)
5. <span data-ttu-id="91115-157">Adja meg a hello kódot és hello URL hello megfelelő mezőkbe hello alkalmazásban, majd válasszon **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="91115-157">Enter hello code and hello URL in hello appropriate boxes in hello app, then select **Finish**.</span></span>

    ![Képernyőn írja be a kódot és URL-címe](./media/authenticator-app-how-to/manual.png)

6. <span data-ttu-id="91115-159">Hello alkalmazás egy hatjegyű kódot alatta a fiók nevét jeleníti meg, amikor elkészült.</span><span class="sxs-lookup"><span data-stu-id="91115-159">When hello app shows your account name with a six-digit code underneath it, you're done.</span></span>

    ![Fiókok képernyő](./media/authenticator-app-how-to/accounts.png)

### <a name="add-an-account-toohello-app-using-touch-id"></a><span data-ttu-id="91115-161">Touch ID használata fiók toohello alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="91115-161">Add an account toohello app using Touch ID</span></span>
<span data-ttu-id="91115-162">hello Microsoft Authenticator alkalmazás IOS rendszerű eszközökön támogatja-e Touch.</span><span class="sxs-lookup"><span data-stu-id="91115-162">hello Microsoft Authenticator app on iOS supports Touch ID.</span></span>  <span data-ttu-id="91115-163">Az Azure multi-factor Authentication lehetővé teszi a szervezetek toorequire eszközök PIN-kódot.</span><span class="sxs-lookup"><span data-stu-id="91115-163">Azure Multi-Factor Authentication allows organizations toorequire a PIN for devices.</span></span> <span data-ttu-id="91115-164">Touch ID, az iOS-felhasználói tooenter PIN-kód nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="91115-164">With Touch ID, iOS users don’t need tooenter a PIN.</span></span> <span data-ttu-id="91115-165">Ehelyett a válassza ki azokat és az ujjlenyomat beolvasása **jóváhagyás**.</span><span class="sxs-lookup"><span data-stu-id="91115-165">Instead, they can scan their fingerprint and select **Approve**.</span></span>

<span data-ttu-id="91115-166">A Microsoft Authenticator Touch ID beállítása felettébb egyszerű.</span><span class="sxs-lookup"><span data-stu-id="91115-166">Setting up Touch ID with Microsoft Authenticator is simple.</span></span> <span data-ttu-id="91115-167">Egy normál ellenőrző kérdéssel és a PIN-kód végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="91115-167">You complete a normal verification challenge with a PIN.</span></span> <span data-ttu-id="91115-168">Ha az eszköz támogatja a Touch ID, a Microsoft Authenticator beállítja az automatikusan fiók.</span><span class="sxs-lookup"><span data-stu-id="91115-168">If your device supports Touch ID, Microsoft Authenticator sets it up automatically for that account.</span></span>

![A Touch ID telepítés ellenőrzése](./media/authenticator-app-how-to/touchid1.png)

<span data-ttu-id="91115-170">A terjesztésipont-előre, amikor Ön tooverify szükséges a bejelentkezés hello kapott leküldéses értesítést válasszon ki, és a PIN-kód megadása helyett az ujjlenyomat beolvasása.</span><span class="sxs-lookup"><span data-stu-id="91115-170">From that point forward, when you're required tooverify your sign-in, you select hello received push notification and scan your fingerprint instead of entering your PIN.</span></span>

![Leküldéses értesítések](./media/authenticator-app-how-to/touchid2.png)

## <a name="use-hello-app-when-you-sign-in"></a><span data-ttu-id="91115-172">Hello alkalmazás használata, amikor bejelentkezik</span><span class="sxs-lookup"><span data-stu-id="91115-172">Use hello app when you sign in</span></span>

<span data-ttu-id="91115-173">A fiók toohello alkalmazás hozzáadása után esetleg felszólító toodo egy teszt ellenőrzési toomake, hogy minden helyesen lett-e beállítva.</span><span class="sxs-lookup"><span data-stu-id="91115-173">Once your account is added toohello app, you may be prompted toodo a test verification toomake sure everything was configured correctly.</span></span> <span data-ttu-id="91115-174">Ezt követően elkészült!</span><span class="sxs-lookup"><span data-stu-id="91115-174">After that, you're done!</span></span> <span data-ttu-id="91115-175">Nincs szükség a toodo bármi más amíg hello legközelebb bejelentkezik.</span><span class="sxs-lookup"><span data-stu-id="91115-175">You don't need toodo anything else until hello next time you sign in.</span></span>

<span data-ttu-id="91115-176">Ha úgy dönt, toouse ellenőrző kódok kezelésére hello alkalmazásban, megkezdése toosee hello kezdőlapján őket.</span><span class="sxs-lookup"><span data-stu-id="91115-176">If you chose toouse verification codes in hello app, you start toosee them on hello home page.</span></span> <span data-ttu-id="91115-177">Így ha szükség van egy mindig kell egy új kódot 30 másodpercenként változik.</span><span class="sxs-lookup"><span data-stu-id="91115-177">They change every 30 seconds so that you always have a new code when you need one.</span></span> <span data-ttu-id="91115-178">De nem kell toodo semmit, amíg bejelentkezhet, és felszólító tooenter ellenőrző kódot.</span><span class="sxs-lookup"><span data-stu-id="91115-178">But you don't need toodo anything with them until you sign in and are prompted tooenter a verification code.</span></span>  
