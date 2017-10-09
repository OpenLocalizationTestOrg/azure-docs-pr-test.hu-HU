---
title: "aaaAzure AD v2 JS SPA interaktív telepítés - telepítő |} Microsoft Docs"
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
ms.openlocfilehash: 19e15c6c8db8bea2975f30e7505af79ccad17e02
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="setting-up-your-web-server-or-project"></a><span data-ttu-id="2d7b4-103">A webkiszolgáló vagy a projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="2d7b4-103">Setting up your web server or project</span></span>

> <span data-ttu-id="2d7b4-104">Inkább toodownload Ez a minta projekt helyett?</span><span class="sxs-lookup"><span data-stu-id="2d7b4-104">Prefer toodownload this sample's project instead?</span></span> 
> - [<span data-ttu-id="2d7b4-105">Hello Visual Studio-projekt letöltése</span><span class="sxs-lookup"><span data-stu-id="2d7b4-105">Download hello Visual Studio project</span></span>](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/VisualStudio.zip)
>
> <span data-ttu-id="2d7b4-106">vagy</span><span class="sxs-lookup"><span data-stu-id="2d7b4-106">or</span></span>
> - <span data-ttu-id="2d7b4-107">[Töltse le a hello projektfájlokat](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/core.zip) helyi webkiszolgáló, például a Python</span><span class="sxs-lookup"><span data-stu-id="2d7b4-107">[Download hello project files](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/core.zip) for a local web server, such as Python</span></span>
>
> <span data-ttu-id="2d7b4-108">Majd kihagyása toohello [konfigurációs lépés](#create-an-application-express) tooconfigure hello példakód azt végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="2d7b4-108">And then  skip toohello [Configuration step](#create-an-application-express) tooconfigure hello code sample before executing it.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2d7b4-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2d7b4-109">Prerequisites</span></span>
<span data-ttu-id="2d7b4-110">Például egy helyi webkiszolgálót [Python http.server](https://www.python.org/downloads/), [http-kiszolgáló](https://www.npmjs.com/package/http-server/), [.NET Core](https://www.microsoft.com/net/core), vagy az IIS Express integrációja [Visual Studio 2017](https://www.visualstudio.com/downloads/) van szükség toorun az interaktív telepítés.</span><span class="sxs-lookup"><span data-stu-id="2d7b4-110">A local web server such as [Python http.server](https://www.python.org/downloads/), [http-server](https://www.npmjs.com/package/http-server/), [.NET Core](https://www.microsoft.com/net/core), or IIS Express integration with [Visual Studio 2017](https://www.visualstudio.com/downloads/) is required toorun this guided setup.</span></span> 

<span data-ttu-id="2d7b4-111">Ez az útmutató utasításait a Python és a Visual Studio 2017 alapulnak, de szabad toouse érzi, hogy más fejlesztési környezet vagy a webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="2d7b4-111">Instructions in this guide are based on both Python and Visual Studio 2017, but feel free toouse any other development environment or Web Server.</span></span>

## <a name="create-your-project"></a><span data-ttu-id="2d7b4-112">A projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="2d7b4-112">Create your project</span></span> 

> ### <a name="option-1-visual-studio"></a><span data-ttu-id="2d7b4-113">1. lehetőség: A Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d7b4-113">Option 1: Visual Studio</span></span> 
> <span data-ttu-id="2d7b4-114">Ha a Visual Studio használ, és a rendszer létrehoz egy új projektet, lépésekkel hello toocreate egy új Visual Studio megoldás alatt:</span><span class="sxs-lookup"><span data-stu-id="2d7b4-114">If you are using Visual Studio and are creating a new project, follow hello steps below toocreate a new Visual Studio solution:</span></span>
> 1.    <span data-ttu-id="2d7b4-115">A Visual Studio:`File` > `New` > `Project`</span><span class="sxs-lookup"><span data-stu-id="2d7b4-115">In Visual Studio:  `File` > `New` > `Project`</span></span>
> 2.    <span data-ttu-id="2d7b4-116">A `Visual C#\Web`, jelölje be`ASP.NET Web Application (.NET Framework)`</span><span class="sxs-lookup"><span data-stu-id="2d7b4-116">Under `Visual C#\Web`, select `ASP.NET Web Application (.NET Framework)`</span></span>
> 3.    <span data-ttu-id="2d7b4-117">Az alkalmazás neve, és kattintson a *OK*</span><span class="sxs-lookup"><span data-stu-id="2d7b4-117">Name your application and click *OK*</span></span>
> 4.    <span data-ttu-id="2d7b4-118">A `New ASP.NET Web Application`, jelölje be`Empty`</span><span class="sxs-lookup"><span data-stu-id="2d7b4-118">Under `New ASP.NET Web Application`, select `Empty`</span></span>

<p/><!-- -->

> ### <a name="option-2-python-other-web-servers"></a><span data-ttu-id="2d7b4-119">2. lehetőség: Python vagy egyéb webkiszolgálókon</span><span class="sxs-lookup"><span data-stu-id="2d7b4-119">Option 2: Python/ other web servers</span></span>
> <span data-ttu-id="2d7b4-120">Győződjön meg arról, hogy a telepített [Python](https://www.python.org/downloads/), hajtsa végre az alábbi hello lépést:</span><span class="sxs-lookup"><span data-stu-id="2d7b4-120">Make sure you have installed [Python](https://www.python.org/downloads/), then follow hello step below:</span></span>
> - <span data-ttu-id="2d7b4-121">Az alkalmazás egy mappában toohost létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2d7b4-121">Create a folder toohost your application.</span></span>


## <a name="create-your-single-page-applications-ui"></a><span data-ttu-id="2d7b4-122">Az egyetlen oldal alkalmazás felhasználói Felületüket létrehozni</span><span class="sxs-lookup"><span data-stu-id="2d7b4-122">Create your single page application’s UI</span></span>
1.  <span data-ttu-id="2d7b4-123">Hozzon létre egy *index.html* a JavaScript SPA fájlt.</span><span class="sxs-lookup"><span data-stu-id="2d7b4-123">Create an *index.html* file for your JavaScript SPA.</span></span> <span data-ttu-id="2d7b4-124">Ha használja a Visual Studio, válassza hello project (projekt gyökérmappájában), kattintson a jobb gombbal, és válassza ki: `Add`  >  `New Item`  >  `HTML page` és adjon neki nevet index.html</span><span class="sxs-lookup"><span data-stu-id="2d7b4-124">If you are using Visual Studio, select hello project (project root folder), right click and select: `Add` > `New Item` > `HTML page` and name it index.html</span></span>
2.  <span data-ttu-id="2d7b4-125">Adja hozzá a következő kódlap tooyour hello:</span><span class="sxs-lookup"><span data-stu-id="2d7b4-125">Add hello following code tooyour page:</span></span>
```html
<!DOCTYPE html>
<html>
<head>
    <!-- bootstrap reference used for styling hello page -->
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

    <!-- This app uses cdn tooreference msal.js (recommended). 
         You can also download it from: https://github.com/AzureAD/microsoft-authentication-library-for-js -->
    <script src="https://secure.aadcdn.microsoftonline-p.com/lib/0.1.1/js/msal.min.js"></script>

    <!-- hello 'bluebird' and 'fetch' references below are required if you need toorun this application on Internet Explorer -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/fetch/2.0.3/fetch.min.js"></script>

    <script type="text/javascript" src="msalconfig.js"></script>
    <script type="text/javascript" src="app.js"></script>
</body>
</html>
````
