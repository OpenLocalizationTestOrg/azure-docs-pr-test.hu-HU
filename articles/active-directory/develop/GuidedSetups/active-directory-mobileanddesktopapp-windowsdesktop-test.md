---
title: "aaaAzure AD v2 Windows asztali bevezetés - teszt |} Microsoft Docs"
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
ms.openlocfilehash: 0ae9612e1585c54a3fe35ba9d18f92554099b2c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="test-your-code"></a><span data-ttu-id="20c5f-103">Tesztelheti a kódját</span><span class="sxs-lookup"><span data-stu-id="20c5f-103">Test your code</span></span>

<span data-ttu-id="20c5f-104">A rendezés tootest az alkalmazás, nyomja meg az `F5` toorun a projektre a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="20c5f-104">In order tootest your application, press `F5` toorun your project in Visual Studio.</span></span> <span data-ttu-id="20c5f-105">A fő ablak kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="20c5f-105">Your Main Window should appear:</span></span>

![A minta képernyőkép](media/active-directory-mobileanddesktopapp-windowsdesktop-test/samplescreenshot.png)

<span data-ttu-id="20c5f-107">Amikor készen áll a tootest, kattintson a *Microsoft Graph API hívása* , és használja a Microsoft Azure Active Directory (szervezeti fiók) vagy a Microsoft Account (live.com, outlook.com) fiók toosign a.</span><span class="sxs-lookup"><span data-stu-id="20c5f-107">When you're ready tootest, click *Call Microsoft Graph API* and use a Microsoft Azure Active Directory (organizational account) or a Microsoft Account (live.com, outlook.com) account toosign in.</span></span> <span data-ttu-id="20c5f-108">Az első alkalommal hello, a felhasználó toosign kérő ablak jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="20c5f-108">It it is hello first time, you will see a window asking user toosign in:</span></span>

![bejelentkezés](media/active-directory-mobileanddesktopapp-windowsdesktop-test/signinscreenshot.png)

### <a name="consent"></a><span data-ttu-id="20c5f-110">Hozzájárulás</span><span class="sxs-lookup"><span data-stu-id="20c5f-110">Consent</span></span>
<span data-ttu-id="20c5f-111">hello első bejelentkezéskor tooyour alkalmazást, akkor számára jelenik meg a hozzájárulási képernyő hasonló toohello az alábbi, amennyiben szükséges tooexplicitly, fogadja el:</span><span class="sxs-lookup"><span data-stu-id="20c5f-111">hello first time you sign in tooyour application, you will be presented with a consent screen similar toohello below, where you need tooexplicitly accept:</span></span>

![Hozzájárulás képernyő](media/active-directory-mobileanddesktopapp-windowsdesktop-test/consentscreen.png)

### <a name="expected-results"></a><span data-ttu-id="20c5f-113">Kívánt eredmény elérése érdekében</span><span class="sxs-lookup"><span data-stu-id="20c5f-113">Expected results</span></span>
<span data-ttu-id="20c5f-114">Felhasználói profil adatait hello API-hívás eredménye képernyőn hello Microsoft Graph API-hívás által visszaadott kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="20c5f-114">You should see user profile information returned by hello Microsoft Graph API call on hello API Call Results screen.</span></span>

<span data-ttu-id="20c5f-115">Emellett meg kell jelennie keresztül szerzett hello token alapvető információkat `AcquireTokenAsync` vagy `AcquireTokenSilentAsync` hello Token adatait mezőbe:</span><span class="sxs-lookup"><span data-stu-id="20c5f-115">You  should also see basic information about hello token acquired via `AcquireTokenAsync` or `AcquireTokenSilentAsync` in hello Token Info box:</span></span>

|<span data-ttu-id="20c5f-116">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="20c5f-116">Property</span></span>  |<span data-ttu-id="20c5f-117">Formátumban</span><span class="sxs-lookup"><span data-stu-id="20c5f-117">Format</span></span>  |<span data-ttu-id="20c5f-118">Leírás</span><span class="sxs-lookup"><span data-stu-id="20c5f-118">Description</span></span> |
|---------|---------|---------|
|<span data-ttu-id="20c5f-119">Név</span><span class="sxs-lookup"><span data-stu-id="20c5f-119">Name</span></span> | <span data-ttu-id="20c5f-120">{Felhasználó teljes neve}</span><span class="sxs-lookup"><span data-stu-id="20c5f-120">{User Full name}</span></span> |<span data-ttu-id="20c5f-121">hello felhasználói vezeték- és keresztneve</span><span class="sxs-lookup"><span data-stu-id="20c5f-121">hello user’s first and last name</span></span>|
|<span data-ttu-id="20c5f-122">Felhasználónév</span><span class="sxs-lookup"><span data-stu-id="20c5f-122">Username</span></span> |<span>user@domain.com</span> |<span data-ttu-id="20c5f-123">hello felhasználónév használt tooidentify hello felhasználó</span><span class="sxs-lookup"><span data-stu-id="20c5f-123">hello username used tooidentify hello user</span></span>|
|<span data-ttu-id="20c5f-124">Jogkivonat lejár</span><span class="sxs-lookup"><span data-stu-id="20c5f-124">Token Expires</span></span> |<span data-ttu-id="20c5f-125">{DateTime}</span><span class="sxs-lookup"><span data-stu-id="20c5f-125">{DateTime}</span></span>         |<span data-ttu-id="20c5f-126">hello idő mely hello jogkivonat lejár.</span><span class="sxs-lookup"><span data-stu-id="20c5f-126">hello time on which hello token expires.</span></span> <span data-ttu-id="20c5f-127">MSAL fog hello lejárati dátum meghosszabbításához meg megújításával hello token szükség esetén</span><span class="sxs-lookup"><span data-stu-id="20c5f-127">MSAL will extend hello expiration date for you by renewing hello token when necessary</span></span>|
|<span data-ttu-id="20c5f-128">hozzáférési jogkivonat</span><span class="sxs-lookup"><span data-stu-id="20c5f-128">Access token</span></span> |<span data-ttu-id="20c5f-129">{Karakterlánc}</span><span class="sxs-lookup"><span data-stu-id="20c5f-129">{String}</span></span>         |<span data-ttu-id="20c5f-130">a lexikális elem karakterlánca hello küldött, küld egy engedélyezési fejléc igénylő tooHTTP kérelmek</span><span class="sxs-lookup"><span data-stu-id="20c5f-130">hello token string sent that will be sent tooHTTP requests that require an authorization header</span></span>|

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a><span data-ttu-id="20c5f-131">További információ a hatókörök és delegált jogosultságokkal sikeresen telepítették</span><span class="sxs-lookup"><span data-stu-id="20c5f-131">More information about scopes and delegated permissions</span></span>
<span data-ttu-id="20c5f-132">Graph API szükséges hello `user.read` tooread felhasználói profil hatókörének.</span><span class="sxs-lookup"><span data-stu-id="20c5f-132">Graph API requires hello `user.read` scope tooread user profile.</span></span> <span data-ttu-id="20c5f-133">Minden egyes a regisztrációs portál regisztrált alkalmazás alapértelmezés szerint automatikusan megjelenik az ebben a hatókörben.</span><span class="sxs-lookup"><span data-stu-id="20c5f-133">This scope is automatically added by default in every application being registered on our registration portal.</span></span> <span data-ttu-id="20c5f-134">Néhány egyéb Graph API-k, valamint a háttérkiszolgáló az egyéni API-k szükséges további hatókörökkel.</span><span class="sxs-lookup"><span data-stu-id="20c5f-134">Some other Graph APIs as well as custom APIs for your backend server require additional scopes.</span></span> <span data-ttu-id="20c5f-135">Például a diagramot `Calendars.Read` van szükség toolist felhasználók naptáraiban.</span><span class="sxs-lookup"><span data-stu-id="20c5f-135">For example, for Graph, `Calendars.Read` is required toolist user’s calendars.</span></span> <span data-ttu-id="20c5f-136">Sorrendben tooaccess hello kontextusban az alkalmazás felhasználó naptár, tooadd kell `Calendars.Read` meghatalmazott alkalmazás regisztrációs adatait, majd adja hozzá `Calendars.Read` toohello `AcquireTokenAsync` hívható meg.</span><span class="sxs-lookup"><span data-stu-id="20c5f-136">In order tooaccess hello user’s calendar in a context of an application, you need tooadd `Calendars.Read` delegated application registration’s information and then add `Calendars.Read` toohello `AcquireTokenAsync` call.</span></span> <span data-ttu-id="20c5f-137">Felhasználó kérheti további hozzájárulásokat azoktól hello hatókörök számának növelésével.</span><span class="sxs-lookup"><span data-stu-id="20c5f-137">User may be prompted for additional consents as you increase hello number of scopes.</span></span>

<!--end-collapse-->



