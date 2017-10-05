---
title: "Az Azure AD v2 JS SPA interaktív telepítés - teszt |} Microsoft Docs"
description: "Hogyan JavaScript SPA-alkalmazások Azure Active Directory-v2 végpontja hozzáférési jogkivonatok igénylő API meghívása"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/01/2017
ms.author: andret
ms.openlocfilehash: c888760ab311e8ac08b1e625bb837f91047db645
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
## <a name="test-your-code"></a><span data-ttu-id="44651-103">Tesztelheti a kódját</span><span class="sxs-lookup"><span data-stu-id="44651-103">Test your code</span></span>

> ### <a name="testing-with-visual-studio"></a><span data-ttu-id="44651-104">A Visual Studio tesztelése</span><span class="sxs-lookup"><span data-stu-id="44651-104">Testing with Visual Studio</span></span>
> <span data-ttu-id="44651-105">Amennyiben a Visual Studio használ, nyomja meg az `F5` a projekt futtatásához: a böngészőben megnyílik, és arra utasítja, hogy *http://localhost: {port}* válaszoknál láthatja a *Microsoft Graph API hívása* gombra.</span><span class="sxs-lookup"><span data-stu-id="44651-105">If you are using Visual Studio, press `F5` to run your project: the browser opens and directs you to *http://localhost:{port}* where you see the *Call Microsoft Graph API* button.</span></span>

<p/><!-- -->

> ### <a name="testing-with-python-or-another-web-server"></a><span data-ttu-id="44651-106">A Python vagy egy másik webkiszolgálón tesztelése</span><span class="sxs-lookup"><span data-stu-id="44651-106">Testing with Python or another web server</span></span>
> <span data-ttu-id="44651-107">Ha a Visual Studio nem használ, győződjön meg arról, a webkiszolgáló elindult, és a mappa tartalmazó alapján TCP-port figyelésére van konfigurálva a *index.html* fájlt.</span><span class="sxs-lookup"><span data-stu-id="44651-107">If you are not using Visual Studio, make sure your web server is started and it is configured to listen to a TCP port based on the folder containing your *index.html* file.</span></span> <span data-ttu-id="44651-108">A Python, megkezdheti a porton figyel futtatásával a parancsban a kérdés / terminál, az alkalmazás mappájában:</span><span class="sxs-lookup"><span data-stu-id="44651-108">For Python, you can start listening to the port by running the in the command prompt/ terminal, from the app's folder:</span></span>
> 
> ```bash
> python -m http.server 8080
> ```
>  <span data-ttu-id="44651-109">Ezután nyissa meg a böngészőben, és a típus *8080* vagy *http://localhost: {port}* – Ha a *port* felel meg a portot, amelyet a webkiszolgáló figyel.</span><span class="sxs-lookup"><span data-stu-id="44651-109">Then, open the browser and type *http://localhost:8080* or *http://localhost:{port}* - where the *port* corresponds to the port that your web server is listening to.</span></span> <span data-ttu-id="44651-110">A tartalmát a index.html lapon kell megjelennie a *Microsoft Graph API hívása* gombra.</span><span class="sxs-lookup"><span data-stu-id="44651-110">You should see the contents of your index.html page with the *Call Microsoft Graph API* button.</span></span>

## <a name="test-your-application"></a><span data-ttu-id="44651-111">Az alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="44651-111">Test your application</span></span>

<span data-ttu-id="44651-112">A böngésző után betölti a *index.html*, kattintson a *Microsoft Graph API hívása* gombra.</span><span class="sxs-lookup"><span data-stu-id="44651-112">After the browser loads your *index.html*, click the *Call Microsoft Graph API* button.</span></span> <span data-ttu-id="44651-113">Ha az első alkalommal, a böngésző átirányítja Önt a Microsoft Azure Active Directory v2 végpont, ahová kéri a bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="44651-113">If this is the first time, the browser redirects you to the Microsoft Azure Active Directory v2 endpoint, where you are  prompted to sign in.</span></span>
 
![A minta képernyőkép](media/active-directory-singlepageapp-javascriptspa-test/javascriptspascreenshot1.png)


### <a name="consent"></a><span data-ttu-id="44651-115">Hozzájárulás</span><span class="sxs-lookup"><span data-stu-id="44651-115">Consent</span></span>
<span data-ttu-id="44651-116">Jelentkezzen be az alkalmazás első alkalommal lehetősége lesz az alábbihoz hasonló hozzájárulási képernyő ahol el kell fogadnia:</span><span class="sxs-lookup"><span data-stu-id="44651-116">The very first time you sign in to your application, you are presented with a consent screen similar to the following, where you need to accept:</span></span>

 ![Hozzájárulás képernyő](media/active-directory-singlepageapp-javascriptspa-test/javascriptspaconsent.png)


### <a name="expected-results"></a><span data-ttu-id="44651-118">Kívánt eredmény elérése érdekében</span><span class="sxs-lookup"><span data-stu-id="44651-118">Expected results</span></span>
<span data-ttu-id="44651-119">Felhasználói profil adatait a Microsoft Graph API-hívás válasz által visszaadott kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="44651-119">You should see user profile information returned by the Microsoft Graph API call response.</span></span>
 
 ![Results (Eredmények)](media/active-directory-singlepageapp-javascriptspa-test/javascriptsparesults.png)

<span data-ttu-id="44651-121">A token szerzett alapvető információkat is megjelenik a *Access Token* és *azonosító Token jogcímek* jelölőnégyzetéből.</span><span class="sxs-lookup"><span data-stu-id="44651-121">You also see basic information about the token acquired in the *Access Token* and *ID Token Claims* boxes.</span></span>

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a><span data-ttu-id="44651-122">További információ a hatókörök és delegált jogosultságokkal sikeresen telepítették</span><span class="sxs-lookup"><span data-stu-id="44651-122">More information about scopes and delegated permissions</span></span>

<span data-ttu-id="44651-123">A Microsoft Graph API megköveteli a `user.read` hatókörrel, hogy a felhasználói profil olvasása.</span><span class="sxs-lookup"><span data-stu-id="44651-123">The Microsoft Graph API requires the `user.read` scope to read the user's profile.</span></span> <span data-ttu-id="44651-124">Minden egyes a regisztrációs portál regisztrált alkalmazás alapértelmezés szerint automatikusan megjelenik az ebben a hatókörben.</span><span class="sxs-lookup"><span data-stu-id="44651-124">This scope is automatically added by default in every application being registered on our registration portal.</span></span> <span data-ttu-id="44651-125">Néhány más Microsoft Graph API, továbbá egyéni API-k, a háttér-kiszolgáló további hatókörökkel lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="44651-125">Some other APIs for Microsoft Graph as well as custom APIs for your backend server may require additional scopes.</span></span> <span data-ttu-id="44651-126">Például a Microsoft Graph-hatóköre `Calendars.Read` listát a felhasználók naptáraiban szükséges.</span><span class="sxs-lookup"><span data-stu-id="44651-126">For example, for Microsoft Graph, the scope `Calendars.Read` is required to list the user’s calendars.</span></span> <span data-ttu-id="44651-127">Ahhoz, hogy hozzáférhessen a környezetben, az alkalmazások a felhasználó naptár, hozzá kell adnia a `Calendars.Read` jogosultságot a az alkalmazás regisztrációs adatait, majd adja hozzá a `Calendars.Read` a hatókör a `acquireTokenSilent` hívható meg.</span><span class="sxs-lookup"><span data-stu-id="44651-127">In order to access the user’s calendar in the context of an application, you need to add the `Calendars.Read` delegated permission to the application registration’s information and then add the `Calendars.Read` scope to the `acquireTokenSilent` call.</span></span> <span data-ttu-id="44651-128">A felhasználó kérheti további hozzájárulásokat azoktól a hatókörök számának növelésével.</span><span class="sxs-lookup"><span data-stu-id="44651-128">The user may be prompted for additional consents as you increase the number of scopes.</span></span>

<span data-ttu-id="44651-129">Ha egy háttér-API nem igényli a hatókör (nem ajánlott), használhatja a `clientId` a hatóköreként a `acquireTokenSilent` és/vagy `acquireTokenRedirect` hívások.</span><span class="sxs-lookup"><span data-stu-id="44651-129">If a backend API does not require a scope (not recommended), you can use the `clientId` as the scope in the `acquireTokenSilent` and/or `acquireTokenRedirect` calls.</span></span>

<!--end-collapse-->
