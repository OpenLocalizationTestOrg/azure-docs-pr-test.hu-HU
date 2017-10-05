---
title: "Az Azure AD v2 Windows asztali első lépések – tesztelése |} Microsoft Docs"
description: "Hogyan Windows asztali .NET (XAML) alkalmazások Azure Active Directory-v2 végpontja hozzáférési jogkivonatok igénylő API meghívása"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 972cc48057c13271d725b0c973c3ccf651ad27c4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
## <a name="test-your-code"></a><span data-ttu-id="98783-103">Tesztelheti a kódját</span><span class="sxs-lookup"><span data-stu-id="98783-103">Test your code</span></span>

<span data-ttu-id="98783-104">Az alkalmazás teszteléséhez nyomja le az `F5` a projektet a Visual Studio futtatásához.</span><span class="sxs-lookup"><span data-stu-id="98783-104">In order to test your application, press `F5` to run your project in Visual Studio.</span></span> <span data-ttu-id="98783-105">A fő ablak kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="98783-105">Your Main Window should appear:</span></span>

![A minta képernyőkép](media/active-directory-mobileanddesktopapp-windowsdesktop-test/samplescreenshot.png)

<span data-ttu-id="98783-107">Amikor készen áll a tesztelése, kattintson *Microsoft Graph API hívása* és a Microsoft Azure Active Directory (szervezeti fiók) vagy egy Microsoft Account (live.com, outlook.com) fiók használatával jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="98783-107">When you're ready to test, click *Call Microsoft Graph API* and use a Microsoft Azure Active Directory (organizational account) or a Microsoft Account (live.com, outlook.com) account to sign in.</span></span> <span data-ttu-id="98783-108">Az első, az látni fogja kérni a felhasználót, hogy jelentkezzen be egy ablakban:</span><span class="sxs-lookup"><span data-stu-id="98783-108">It it is the first time, you will see a window asking user to sign in:</span></span>

![bejelentkezés](media/active-directory-mobileanddesktopapp-windowsdesktop-test/signinscreenshot.png)

### <a name="consent"></a><span data-ttu-id="98783-110">Hozzájárulás</span><span class="sxs-lookup"><span data-stu-id="98783-110">Consent</span></span>
<span data-ttu-id="98783-111">Az első alkalommal bejelentkezik az alkalmazás választhat hasonló hozzájárulási képernyő a alatt, ahol kell explicit módon fogadja el:</span><span class="sxs-lookup"><span data-stu-id="98783-111">The first time you sign in to your application, you will be presented with a consent screen similar to the below, where you need to explicitly accept:</span></span>

![Hozzájárulás képernyő](media/active-directory-mobileanddesktopapp-windowsdesktop-test/consentscreen.png)

### <a name="expected-results"></a><span data-ttu-id="98783-113">Kívánt eredmény elérése érdekében</span><span class="sxs-lookup"><span data-stu-id="98783-113">Expected results</span></span>
<span data-ttu-id="98783-114">Felhasználói profil adatait a API-hívási eredmények képernyőn a Microsoft Graph API-hívás által visszaadott kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="98783-114">You should see user profile information returned by the Microsoft Graph API call on the API Call Results screen.</span></span>

<span data-ttu-id="98783-115">Emellett meg kell jelennie a token keresztül szerzett alapvető információkat `AcquireTokenAsync` vagy `AcquireTokenSilentAsync` a Token adatait mezőbe:</span><span class="sxs-lookup"><span data-stu-id="98783-115">You  should also see basic information about the token acquired via `AcquireTokenAsync` or `AcquireTokenSilentAsync` in the Token Info box:</span></span>

|<span data-ttu-id="98783-116">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="98783-116">Property</span></span>  |<span data-ttu-id="98783-117">Formátumban</span><span class="sxs-lookup"><span data-stu-id="98783-117">Format</span></span>  |<span data-ttu-id="98783-118">Leírás</span><span class="sxs-lookup"><span data-stu-id="98783-118">Description</span></span> |
|---------|---------|---------|
|<span data-ttu-id="98783-119">Név</span><span class="sxs-lookup"><span data-stu-id="98783-119">Name</span></span> | <span data-ttu-id="98783-120">{Felhasználó teljes neve}</span><span class="sxs-lookup"><span data-stu-id="98783-120">{User Full name}</span></span> |<span data-ttu-id="98783-121">A felhasználó nagyapja vezeték- és keresztneve</span><span class="sxs-lookup"><span data-stu-id="98783-121">The user’s first and last name</span></span>|
|<span data-ttu-id="98783-122">Felhasználónév</span><span class="sxs-lookup"><span data-stu-id="98783-122">Username</span></span> |<span>user@domain.com</span> |<span data-ttu-id="98783-123">A felhasználó azonosítására használt felhasználónév</span><span class="sxs-lookup"><span data-stu-id="98783-123">The username used to identify the user</span></span>|
|<span data-ttu-id="98783-124">Jogkivonat lejár</span><span class="sxs-lookup"><span data-stu-id="98783-124">Token Expires</span></span> |<span data-ttu-id="98783-125">{DateTime}</span><span class="sxs-lookup"><span data-stu-id="98783-125">{DateTime}</span></span>         |<span data-ttu-id="98783-126">Az az idő, amikor a jogkivonat lejár.</span><span class="sxs-lookup"><span data-stu-id="98783-126">The time on which the token expires.</span></span> <span data-ttu-id="98783-127">MSAL lesz a lejárati dátum meghosszabbításához, szükség esetén a jogkivonatban megújításával</span><span class="sxs-lookup"><span data-stu-id="98783-127">MSAL will extend the expiration date for you by renewing the token when necessary</span></span>|
|<span data-ttu-id="98783-128">hozzáférési jogkivonat</span><span class="sxs-lookup"><span data-stu-id="98783-128">Access token</span></span> |<span data-ttu-id="98783-129">{Karakterlánc}</span><span class="sxs-lookup"><span data-stu-id="98783-129">{String}</span></span>         |<span data-ttu-id="98783-130">A lexikális elem karakterlánca küldött, küld egy engedélyezési fejléc igénylő HTTP-kérelmek</span><span class="sxs-lookup"><span data-stu-id="98783-130">The token string sent that will be sent to HTTP requests that require an authorization header</span></span>|

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a><span data-ttu-id="98783-131">További információ a hatókörök és delegált jogosultságokkal sikeresen telepítették</span><span class="sxs-lookup"><span data-stu-id="98783-131">More information about scopes and delegated permissions</span></span>
<span data-ttu-id="98783-132">Graph API megköveteli a `user.read` hatókörrel, hogy a felhasználói profil olvasása.</span><span class="sxs-lookup"><span data-stu-id="98783-132">Graph API requires the `user.read` scope to read user profile.</span></span> <span data-ttu-id="98783-133">Minden egyes a regisztrációs portál regisztrált alkalmazás alapértelmezés szerint automatikusan megjelenik az ebben a hatókörben.</span><span class="sxs-lookup"><span data-stu-id="98783-133">This scope is automatically added by default in every application being registered on our registration portal.</span></span> <span data-ttu-id="98783-134">Néhány egyéb Graph API-k, valamint a háttérkiszolgáló az egyéni API-k szükséges további hatókörökkel.</span><span class="sxs-lookup"><span data-stu-id="98783-134">Some other Graph APIs as well as custom APIs for your backend server require additional scopes.</span></span> <span data-ttu-id="98783-135">Például a diagramot `Calendars.Read` lista felhasználók naptáraiban szükséges.</span><span class="sxs-lookup"><span data-stu-id="98783-135">For example, for Graph, `Calendars.Read` is required to list user’s calendars.</span></span> <span data-ttu-id="98783-136">A felhasználó naptár kontextusban az alkalmazás eléréséhez, hozzá kell adnia `Calendars.Read` meghatalmazott alkalmazás regisztrációs adatait, majd adja hozzá `Calendars.Read` számára a `AcquireTokenAsync` hívható meg.</span><span class="sxs-lookup"><span data-stu-id="98783-136">In order to access the user’s calendar in a context of an application, you need to add `Calendars.Read` delegated application registration’s information and then add `Calendars.Read` to the `AcquireTokenAsync` call.</span></span> <span data-ttu-id="98783-137">Felhasználó kérheti további hozzájárulásokat azoktól a hatókörök számának növelésével.</span><span class="sxs-lookup"><span data-stu-id="98783-137">User may be prompted for additional consents as you increase the number of scopes.</span></span>

<!--end-collapse-->



