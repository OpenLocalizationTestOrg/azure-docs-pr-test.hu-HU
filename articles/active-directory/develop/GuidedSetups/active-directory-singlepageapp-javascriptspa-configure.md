---
title: "aaaAzure AD v2 JS SPA interaktív telepítés – konfigurálása |} Microsoft Docs"
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
ms.openlocfilehash: 1b93298d4bd4e17dd261dbb75502a122c30aac97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="register-your-application"></a><span data-ttu-id="67bf6-103">Alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="67bf6-103">Register your application</span></span>

<span data-ttu-id="67bf6-104">Nincs több módon toocreate egy alkalmazást, válasszon egyet a őket:</span><span class="sxs-lookup"><span data-stu-id="67bf6-104">There are multiple ways toocreate an application, please select one of them:</span></span>

### <a name="option-1-register-your-application-express-mode"></a><span data-ttu-id="67bf6-105">1. lehetőség: Az alkalmazás (Expressz mód) regisztrálása</span><span class="sxs-lookup"><span data-stu-id="67bf6-105">Option 1: Register your application (Express mode)</span></span>
<span data-ttu-id="67bf6-106">Most tooregister hello az alkalmazás *Microsoft alkalmazásregisztrációs portálra*:</span><span class="sxs-lookup"><span data-stu-id="67bf6-106">Now you need tooregister your application in hello *Microsoft Application Registration Portal*:</span></span>

1.  <span data-ttu-id="67bf6-107">Hello keresztül az alkalmazás regisztrálása [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/portal/register-app?appType=singlePageApp&appTech=javascriptSpa&step=configure)</span><span class="sxs-lookup"><span data-stu-id="67bf6-107">Register your application via hello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app?appType=singlePageApp&appTech=javascriptSpa&step=configure)</span></span>
2.  <span data-ttu-id="67bf6-108">Adjon meg egy nevet az alkalmazás és az e-maileket</span><span class="sxs-lookup"><span data-stu-id="67bf6-108">Enter a name for your application and your email</span></span>
3.  <span data-ttu-id="67bf6-109">Győződjön meg arról, hogy hello lehetőséget *interaktív telepítés* be van jelölve</span><span class="sxs-lookup"><span data-stu-id="67bf6-109">Make sure hello option for *Guided Setup* is checked</span></span>
4.  <span data-ttu-id="67bf6-110">Hajtsa végre a hello utasításokat tooobtain hello Alkalmazásazonosító, és illessze be a kódot</span><span class="sxs-lookup"><span data-stu-id="67bf6-110">Follow hello instructions tooobtain hello application ID and paste it into your code</span></span>

### <a name="option-2-register-your-application-advanced-mode"></a><span data-ttu-id="67bf6-111">2. lehetőség: Az alkalmazás (speciális módban) regisztrálása</span><span class="sxs-lookup"><span data-stu-id="67bf6-111">Option 2: Register your application (Advanced mode)</span></span>

1. <span data-ttu-id="67bf6-112">Nyissa meg toohello [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/portal/register-app) tooregister alkalmazás</span><span class="sxs-lookup"><span data-stu-id="67bf6-112">Go toohello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app) tooregister an application</span></span>
2. <span data-ttu-id="67bf6-113">Adjon meg egy nevet az alkalmazás és az e-maileket</span><span class="sxs-lookup"><span data-stu-id="67bf6-113">Enter a name for your application and your email</span></span> 
3. <span data-ttu-id="67bf6-114">Győződjön meg arról, hogy hello lehetőséget *interaktív telepítés* nincs bejelölve</span><span class="sxs-lookup"><span data-stu-id="67bf6-114">Make sure hello option for *Guided Setup* is unchecked</span></span>
4.  <span data-ttu-id="67bf6-115">Kattintson a `Add Platform`, majd jelölje be`Web`</span><span class="sxs-lookup"><span data-stu-id="67bf6-115">Click `Add Platform`, then select `Web`</span></span>
5. <span data-ttu-id="67bf6-116">Adja hozzá a hello `Redirect URL` , amelyek megfelelnek a toohello alkalmazás URL-CÍMÉT, a webkiszolgáló alapján.</span><span class="sxs-lookup"><span data-stu-id="67bf6-116">Add hello `Redirect URL` that correspond toohello application's URL based on your web server.</span></span> <span data-ttu-id="67bf6-117">Útmutatás hello szakaszokat meg, hogyan tooset / a Visual Studio és a Python hello átirányítási URL-cím beszerzése.</span><span class="sxs-lookup"><span data-stu-id="67bf6-117">See hello sections below for instructions on how tooset/ obtain hello redirect URL in Visual Studio and Python.</span></span>
6. <span data-ttu-id="67bf6-118">Kattintson a *mentése*</span><span class="sxs-lookup"><span data-stu-id="67bf6-118">Click *Save*</span></span>

> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a><span data-ttu-id="67bf6-119">A Visual Studio utasításokat átirányítási URL-cím beszerzése</span><span class="sxs-lookup"><span data-stu-id="67bf6-119">Visual Studio instructions for obtaining redirect URL</span></span>
> <span data-ttu-id="67bf6-120">Kövesse az utasításokat tooobtain hello az átirányítási URL-cím:</span><span class="sxs-lookup"><span data-stu-id="67bf6-120">Follow hello instructions tooobtain your redirect URL:</span></span>
> 1.    <span data-ttu-id="67bf6-121">A *Megoldáskezelőben*hello projekt között, nézze meg hello `Properties` ablakban (Ha nem látja a Tulajdonságok ablak, nyomja meg az `F4`)</span><span class="sxs-lookup"><span data-stu-id="67bf6-121">In *Solution Explorer*, select hello project and look at hello `Properties` window (if you don’t see a Properties window, press `F4`)</span></span>
> 2.    <span data-ttu-id="67bf6-122">Másolja a hello értéket `URL` toohello vágólapra:</span><span class="sxs-lookup"><span data-stu-id="67bf6-122">Copy hello value from `URL` toohello clipboard:</span></span><br/> <span data-ttu-id="67bf6-123">![Projekt tulajdonságai](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)</span><span class="sxs-lookup"><span data-stu-id="67bf6-123">![Project properties](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)</span></span><br />
> 3.    <span data-ttu-id="67bf6-124">Váltson vissza toohello *alkalmazásregisztrációs portálra* , majd illessze be a hello értéke megegyezik egy `Redirect URL` , és kattintson a "Mentés"</span><span class="sxs-lookup"><span data-stu-id="67bf6-124">Switch back toohello *Application Registration Portal* and paste hello value as a `Redirect URL` and click 'Save'</span></span>

<p/>

> #### <a name="setting-redirect-url-for-python"></a><span data-ttu-id="67bf6-125">Az átirányítási URL-ként Python</span><span class="sxs-lookup"><span data-stu-id="67bf6-125">Setting Redirect URL for Python</span></span>
> <span data-ttu-id="67bf6-126">Python hello a webtartalom-kiszolgáló portja parancssorból állíthatók be.</span><span class="sxs-lookup"><span data-stu-id="67bf6-126">For Python, you can set hello web server port via command line.</span></span> <span data-ttu-id="67bf6-127">Az interaktív telepítés használja a 8080-as porton hello referenciaként, de érzi, hogy a szabad toouse összes rendelkezésre álló portot.</span><span class="sxs-lookup"><span data-stu-id="67bf6-127">This guided setup uses hello port 8080 for reference but feel free toouse any other port available.</span></span> <span data-ttu-id="67bf6-128">Minden esetben kövesse az alábbi tooset hello alkalmazás regisztrációs adatai egy átirányítási URL-cím mentése hello utasításokat:</span><span class="sxs-lookup"><span data-stu-id="67bf6-128">In any case, follow hello instructions below tooset up a redirect URL in hello application registration information:</span></span><br/>
> - <span data-ttu-id="67bf6-129">Váltson vissza toohello *alkalmazásregisztrációs portálra* és állítsa be `http://localhost:8080/` , egy `Redirect URL`, vagy használjon `http://localhost:[port]/` egyéni TCP-port használata (ahol *[port]* hello egyéni TCP port szám), és kattintson a "Mentés"</span><span class="sxs-lookup"><span data-stu-id="67bf6-129">Switch back toohello *Application Registration Portal* and set `http://localhost:8080/` as a `Redirect URL`, or use `http://localhost:[port]/` if you are using a custom TCP port (where *[port]* is hello custom TCP port number) and click 'Save'</span></span>


#### <a name="configure-your-javascript-spa"></a><span data-ttu-id="67bf6-130">A JavaScript SPA konfigurálása</span><span class="sxs-lookup"><span data-stu-id="67bf6-130">Configure your JavaScript SPA</span></span>

1.  <span data-ttu-id="67bf6-131">Hozzon létre egy fájlt `msalconfig.js` tartalmazó hello alkalmazás regisztrációs adatokat.</span><span class="sxs-lookup"><span data-stu-id="67bf6-131">Create a file named `msalconfig.js` containing hello application registration information.</span></span> <span data-ttu-id="67bf6-132">Ha használja a Visual Studio, válassza hello project (projekt gyökérmappájában), kattintson a jobb gombbal, és válassza ki: `Add`  >  `New Item`  >  `JavaScript File`.</span><span class="sxs-lookup"><span data-stu-id="67bf6-132">If you are using Visual Studio, select hello project (project root folder), right-click and select: `Add` > `New Item` > `JavaScript File`.</span></span> <span data-ttu-id="67bf6-133">Nevezze el`msalconfig.js`</span><span class="sxs-lookup"><span data-stu-id="67bf6-133">Name it `msalconfig.js`</span></span>
2.  <span data-ttu-id="67bf6-134">Adja hozzá a következő kód tooyour hello `msalconfig.js` fájlt:</span><span class="sxs-lookup"><span data-stu-id="67bf6-134">Add hello following code tooyour `msalconfig.js` file:</span></span>

```javascript
var msalconfig = {
    clientID: "Enter_the_Application_Id_here",
    redirectUri: location.origin
};
```
<ol start="3">
<li>
<span data-ttu-id="67bf6-135">Cserélje le <code>Enter_the_Application_Id_here</code> a hello csak regisztrált alkalmazás azonosítója</span><span class="sxs-lookup"><span data-stu-id="67bf6-135">Replace <code>Enter_the_Application_Id_here</code> with hello Application Id you just registered</span></span>
</li>
</ol>
