---
title: "Az Azure AD v2 JS SPA interaktív telepítés - telepítő |} Microsoft Docs"
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
ms.openlocfilehash: fc9f88cc8d23abcfa8ea30e346192732b422ffa2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
## <a name="setting-up-your-web-server-or-project"></a><span data-ttu-id="841d3-103">A webkiszolgáló vagy a projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="841d3-103">Setting up your web server or project</span></span>

> <span data-ttu-id="841d3-104">Ez a minta-projekt letöltése helyette inkább?</span><span class="sxs-lookup"><span data-stu-id="841d3-104">Prefer to download this sample's project instead?</span></span> 
> - [<span data-ttu-id="841d3-105">A Visual Studio-projekt letöltése</span><span class="sxs-lookup"><span data-stu-id="841d3-105">Download the Visual Studio project</span></span>](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/VisualStudio.zip)
>
> <span data-ttu-id="841d3-106">vagy</span><span class="sxs-lookup"><span data-stu-id="841d3-106">or</span></span>
> - <span data-ttu-id="841d3-107">[Töltse le a projektfájlok](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/core.zip) helyi webkiszolgáló, például a Python</span><span class="sxs-lookup"><span data-stu-id="841d3-107">[Download the project files](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/core.zip) for a local web server, such as Python</span></span>
>
> <span data-ttu-id="841d3-108">Majd folytassa a és a [konfigurációs lépés](#create-an-application-express) a kódminta konfigurálása előtt futtatnia kell.</span><span class="sxs-lookup"><span data-stu-id="841d3-108">And then  skip to the [Configuration step](#create-an-application-express) to configure the code sample before executing it.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="841d3-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="841d3-109">Prerequisites</span></span>
<span data-ttu-id="841d3-110">Például egy helyi webkiszolgálót [Python http.server](https://www.python.org/downloads/), [http-kiszolgáló](https://www.npmjs.com/package/http-server/), [.NET Core](https://www.microsoft.com/net/core), vagy az IIS Express integrációja [Visual Studio 2017](https://www.visualstudio.com/downloads/) Ez az interaktív telepítés futtatásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="841d3-110">A local web server such as [Python http.server](https://www.python.org/downloads/), [http-server](https://www.npmjs.com/package/http-server/), [.NET Core](https://www.microsoft.com/net/core), or IIS Express integration with [Visual Studio 2017](https://www.visualstudio.com/downloads/) is required to run this guided setup.</span></span> 

<span data-ttu-id="841d3-111">Ez az útmutató utasításait a Python és a Visual Studio 2017 alapulnak, de szabadon használhatja a fejlesztési környezet vagy webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="841d3-111">Instructions in this guide are based on both Python and Visual Studio 2017, but feel free to use any other development environment or Web Server.</span></span>

## <a name="create-your-project"></a><span data-ttu-id="841d3-112">A projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="841d3-112">Create your project</span></span> 

> ### <a name="option-1-visual-studio"></a><span data-ttu-id="841d3-113">1. lehetőség: A Visual Studio</span><span class="sxs-lookup"><span data-stu-id="841d3-113">Option 1: Visual Studio</span></span> 
> <span data-ttu-id="841d3-114">Ha a Visual Studio használ, és a rendszer létrehoz egy új projektet, kövesse az alábbi lépéseket egy új Visual Studio megoldás létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="841d3-114">If you are using Visual Studio and are creating a new project, follow the steps below to create a new Visual Studio solution:</span></span>
> 1.    <span data-ttu-id="841d3-115">A Visual Studio:`File` > `New` > `Project`</span><span class="sxs-lookup"><span data-stu-id="841d3-115">In Visual Studio:  `File` > `New` > `Project`</span></span>
> 2.    <span data-ttu-id="841d3-116">A `Visual C#\Web`, jelölje be`ASP.NET Web Application (.NET Framework)`</span><span class="sxs-lookup"><span data-stu-id="841d3-116">Under `Visual C#\Web`, select `ASP.NET Web Application (.NET Framework)`</span></span>
> 3.    <span data-ttu-id="841d3-117">Az alkalmazás neve, és kattintson a *OK*</span><span class="sxs-lookup"><span data-stu-id="841d3-117">Name your application and click *OK*</span></span>
> 4.    <span data-ttu-id="841d3-118">A `New ASP.NET Web Application`, jelölje be`Empty`</span><span class="sxs-lookup"><span data-stu-id="841d3-118">Under `New ASP.NET Web Application`, select `Empty`</span></span>

<p/><!-- -->

> ### <a name="option-2-python-other-web-servers"></a><span data-ttu-id="841d3-119">2. lehetőség: Python vagy egyéb webkiszolgálókon</span><span class="sxs-lookup"><span data-stu-id="841d3-119">Option 2: Python/ other web servers</span></span>
> <span data-ttu-id="841d3-120">Győződjön meg arról, hogy a telepített [Python](https://www.python.org/downloads/), majd hajtsa végre az alábbi:</span><span class="sxs-lookup"><span data-stu-id="841d3-120">Make sure you have installed [Python](https://www.python.org/downloads/), then follow the step below:</span></span>
> - <span data-ttu-id="841d3-121">Hozzon létre egy mappát, az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="841d3-121">Create a folder to host your application.</span></span>


## <a name="create-your-single-page-applications-ui"></a><span data-ttu-id="841d3-122">Az egyetlen oldal alkalmazás felhasználói Felületüket létrehozni</span><span class="sxs-lookup"><span data-stu-id="841d3-122">Create your single page application’s UI</span></span>
1.  <span data-ttu-id="841d3-123">Hozzon létre egy *index.html* a JavaScript SPA fájlt.</span><span class="sxs-lookup"><span data-stu-id="841d3-123">Create an *index.html* file for your JavaScript SPA.</span></span> <span data-ttu-id="841d3-124">Ha a Visual Studio használ, válassza ki a project (projekt gyökérmappájában), kattintson a jobb gombbal, és válassza: `Add`  >  `New Item`  >  `HTML page` és adjon neki nevet index.html</span><span class="sxs-lookup"><span data-stu-id="841d3-124">If you are using Visual Studio, select the project (project root folder), right click and select: `Add` > `New Item` > `HTML page` and name it index.html</span></span>
2.  <span data-ttu-id="841d3-125">Adja hozzá a következő kódot a lap:</span><span class="sxs-lookup"><span data-stu-id="841d3-125">Add the following code to your page:</span></span>
```html
<!DOCTYPE html>
<html>
<head>
    <!-- bootstrap reference used for styling the page -->
    <link href="//maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <title>JavaScript SPA Guided Setup</title>
</head>
<body style="margin: 40px">
    <button id="callGraphButton" type="button" class="btn btn-primary" onclick="callGraphApi()">Call Microsoft Graph API</button>
    <div id="errorMessage" class="text-danger"></div>
    <div class="hidden">
        <h3>Graph API Call Response</h3>
        <pre class="well" id="graphResponse"></pre>
    </div>
    <div class="hidden">
        <h3>Access Token</h3>
        <pre class="well" id="accessToken"></pre>
    </div>
    <div class="hidden">
        <h3>ID Token Claims</h3>
        <pre class="well" id="userInfo"></pre>
    </div>
    <button id="signOutButton" type="button" class="btn btn-primary hidden" onclick="signOut()">Sign out</button>

    <!-- This app uses cdn to reference msal.js (recommended). 
         You can also download it from: https://github.com/AzureAD/microsoft-authentication-library-for-js -->
    <script src="https://secure.aadcdn.microsoftonline-p.com/lib/0.1.1/js/msal.min.js"></script>

    <!-- The 'bluebird' and 'fetch' references below are required if you need to run this application on Internet Explorer -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/fetch/2.0.3/fetch.min.js"></script>

    <script type="text/javascript" src="msalconfig.js"></script>
    <script type="text/javascript" src="app.js"></script>
</body>
</html>
````
