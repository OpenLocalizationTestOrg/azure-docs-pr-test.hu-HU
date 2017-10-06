---
title: "aaaAzure AD v2 JS SPA interaktív telepítés - teszt |} Microsoft Docs"
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
ms.openlocfilehash: b2339431a070b5c4ad4058e6c1a9b19b83c84c1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="test-your-code"></a><span data-ttu-id="cd554-103">Tesztelheti a kódját</span><span class="sxs-lookup"><span data-stu-id="cd554-103">Test your code</span></span>

> ### <a name="testing-with-visual-studio"></a><span data-ttu-id="cd554-104">A Visual Studio tesztelése</span><span class="sxs-lookup"><span data-stu-id="cd554-104">Testing with Visual Studio</span></span>
> <span data-ttu-id="cd554-105">Visual Studio használatakor nyomja le az `F5` toorun a projekthez: hello böngészőben megnyílik, és arra utasítja, hogy túl*http://localhost: {port}* hello megtapasztalhatja *Microsoft Graph API hívása* gombra.</span><span class="sxs-lookup"><span data-stu-id="cd554-105">If you are using Visual Studio, press `F5` toorun your project: hello browser opens and directs you too*http://localhost:{port}* where you see hello *Call Microsoft Graph API* button.</span></span>

<p/><!-- -->

> ### <a name="testing-with-python-or-another-web-server"></a><span data-ttu-id="cd554-106">A Python vagy egy másik webkiszolgálón tesztelése</span><span class="sxs-lookup"><span data-stu-id="cd554-106">Testing with Python or another web server</span></span>
> <span data-ttu-id="cd554-107">Ha a Visual Studio nem használ, győződjön meg arról, a webkiszolgáló elindult, és van-e konfigurálva toolisten tooa TCP-port alapján hello mappát tartalmazó a *index.html* fájlt.</span><span class="sxs-lookup"><span data-stu-id="cd554-107">If you are not using Visual Studio, make sure your web server is started and it is configured toolisten tooa TCP port based on hello folder containing your *index.html* file.</span></span> <span data-ttu-id="cd554-108">A Python, megkezdheti figyel toohello port futtatásával hello hello parancsban Rákérdezés / terminál, hello app mappából:</span><span class="sxs-lookup"><span data-stu-id="cd554-108">For Python, you can start listening toohello port by running hello in hello command prompt/ terminal, from hello app's folder:</span></span>
> 
> ```bash
> python -m http.server 8080
> ```
>  <span data-ttu-id="cd554-109">Ezt követően hello böngésző megnyitásához és típus *8080* vagy *http://localhost: {port}* - Ha hello *port* toohello portot, amelyet a webkiszolgáló megfelel figyel.</span><span class="sxs-lookup"><span data-stu-id="cd554-109">Then, open hello browser and type *http://localhost:8080* or *http://localhost:{port}* - where hello *port* corresponds toohello port that your web server is listening to.</span></span> <span data-ttu-id="cd554-110">Hello tartalmát a hello index.html lapon kell megjelennie *Microsoft Graph API hívása* gombra.</span><span class="sxs-lookup"><span data-stu-id="cd554-110">You should see hello contents of your index.html page with hello *Call Microsoft Graph API* button.</span></span>

## <a name="test-your-application"></a><span data-ttu-id="cd554-111">Az alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="cd554-111">Test your application</span></span>

<span data-ttu-id="cd554-112">Hello böngésző után betölti a *index.html*, kattintson a hello *Microsoft Graph API hívása* gombra.</span><span class="sxs-lookup"><span data-stu-id="cd554-112">After hello browser loads your *index.html*, click hello *Call Microsoft Graph API* button.</span></span> <span data-ttu-id="cd554-113">Ha hello első alkalommal, hello böngésző átirányítja, Microsoft Azure Active Directory v2 toohello végpont, ahol felszólítja a toosign.</span><span class="sxs-lookup"><span data-stu-id="cd554-113">If this is hello first time, hello browser redirects you toohello Microsoft Azure Active Directory v2 endpoint, where you are  prompted toosign in.</span></span>
 
![A minta képernyőkép](media/active-directory-singlepageapp-javascriptspa-test/javascriptspascreenshot1.png)


### <a name="consent"></a><span data-ttu-id="cd554-115">Hozzájárulás</span><span class="sxs-lookup"><span data-stu-id="cd554-115">Consent</span></span>
<span data-ttu-id="cd554-116">hello első bejelentkezéskor tooyour alkalmazás, lehetősége lesz a hozzájárulási képernyő hasonló toohello következő, ahol tooaccept kell:</span><span class="sxs-lookup"><span data-stu-id="cd554-116">hello very first time you sign in tooyour application, you are presented with a consent screen similar toohello following, where you need tooaccept:</span></span>

 ![Hozzájárulás képernyő](media/active-directory-singlepageapp-javascriptspa-test/javascriptspaconsent.png)


### <a name="expected-results"></a><span data-ttu-id="cd554-118">Kívánt eredmény elérése érdekében</span><span class="sxs-lookup"><span data-stu-id="cd554-118">Expected results</span></span>
<span data-ttu-id="cd554-119">Felhasználói profil adatait a Microsoft Graph API-hívás válasz hello által visszaadott kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="cd554-119">You should see user profile information returned by hello Microsoft Graph API call response.</span></span>
 
 ![Results (Eredmények)](media/active-directory-singlepageapp-javascriptspa-test/javascriptsparesults.png)

<span data-ttu-id="cd554-121">Hello szerzett hello token alapvető információkat is látni *Access Token* és *azonosító Token jogcímek* jelölőnégyzetéből.</span><span class="sxs-lookup"><span data-stu-id="cd554-121">You also see basic information about hello token acquired in hello *Access Token* and *ID Token Claims* boxes.</span></span>

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a><span data-ttu-id="cd554-122">További információ a hatókörök és delegált jogosultságokkal sikeresen telepítették</span><span class="sxs-lookup"><span data-stu-id="cd554-122">More information about scopes and delegated permissions</span></span>

<span data-ttu-id="cd554-123">hello Microsoft Graph API szükséges hello `user.read` tooread hello profil hatókörének.</span><span class="sxs-lookup"><span data-stu-id="cd554-123">hello Microsoft Graph API requires hello `user.read` scope tooread hello user's profile.</span></span> <span data-ttu-id="cd554-124">Minden egyes a regisztrációs portál regisztrált alkalmazás alapértelmezés szerint automatikusan megjelenik az ebben a hatókörben.</span><span class="sxs-lookup"><span data-stu-id="cd554-124">This scope is automatically added by default in every application being registered on our registration portal.</span></span> <span data-ttu-id="cd554-125">Néhány más Microsoft Graph API, továbbá egyéni API-k, a háttér-kiszolgáló további hatókörökkel lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="cd554-125">Some other APIs for Microsoft Graph as well as custom APIs for your backend server may require additional scopes.</span></span> <span data-ttu-id="cd554-126">Például a Microsoft Graph-hatókör hello `Calendars.Read` van szükség toolist hello felhasználók naptáraiban.</span><span class="sxs-lookup"><span data-stu-id="cd554-126">For example, for Microsoft Graph, hello scope `Calendars.Read` is required toolist hello user’s calendars.</span></span> <span data-ttu-id="cd554-127">Sorrendben tooaccess hello hello környezetben egy alkalmazás felhasználói naptár, tooadd hello kell `Calendars.Read` delegált engedéllyel toohello alkalmazás regisztrációs adatait, és adja meg az hello `Calendars.Read` hatókör toohello `acquireTokenSilent` hívható meg.</span><span class="sxs-lookup"><span data-stu-id="cd554-127">In order tooaccess hello user’s calendar in hello context of an application, you need tooadd hello `Calendars.Read` delegated permission toohello application registration’s information and then add hello `Calendars.Read` scope toohello `acquireTokenSilent` call.</span></span> <span data-ttu-id="cd554-128">hello felhasználói további hozzájárulásokat azoktól hello hatókörök számának növelésével kérheti.</span><span class="sxs-lookup"><span data-stu-id="cd554-128">hello user may be prompted for additional consents as you increase hello number of scopes.</span></span>

<span data-ttu-id="cd554-129">Ha egy háttér-API nem igényli a hatókör (nem ajánlott), használhatja a hello `clientId` hello hatóköreként a hello `acquireTokenSilent` és/vagy `acquireTokenRedirect` hívások.</span><span class="sxs-lookup"><span data-stu-id="cd554-129">If a backend API does not require a scope (not recommended), you can use hello `clientId` as hello scope in hello `acquireTokenSilent` and/or `acquireTokenRedirect` calls.</span></span>

<!--end-collapse-->
