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
## <a name="setting-up-your-web-server-or-project"></a>A webkiszolgáló vagy a projekt létrehozása

> Inkább toodownload Ez a minta projekt helyett? 
> - [Hello Visual Studio-projekt letöltése](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/VisualStudio.zip)
>
> vagy
> - [Töltse le a hello projektfájlokat](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/core.zip) helyi webkiszolgáló, például a Python
>
> Majd kihagyása toohello [konfigurációs lépés](#create-an-application-express) tooconfigure hello példakód azt végrehajtása előtt.

## <a name="prerequisites"></a>Előfeltételek
Például egy helyi webkiszolgálót [Python http.server](https://www.python.org/downloads/), [http-kiszolgáló](https://www.npmjs.com/package/http-server/), [.NET Core](https://www.microsoft.com/net/core), vagy az IIS Express integrációja [Visual Studio 2017](https://www.visualstudio.com/downloads/) van szükség toorun az interaktív telepítés. 

Ez az útmutató utasításait a Python és a Visual Studio 2017 alapulnak, de szabad toouse érzi, hogy más fejlesztési környezet vagy a webkiszolgálón.

## <a name="create-your-project"></a>A projekt létrehozása 

> ### <a name="option-1-visual-studio"></a>1. lehetőség: A Visual Studio 
> Ha a Visual Studio használ, és a rendszer létrehoz egy új projektet, lépésekkel hello toocreate egy új Visual Studio megoldás alatt:
> 1.    A Visual Studio:`File` > `New` > `Project`
> 2.    A `Visual C#\Web`, jelölje be`ASP.NET Web Application (.NET Framework)`
> 3.    Az alkalmazás neve, és kattintson a *OK*
> 4.    A `New ASP.NET Web Application`, jelölje be`Empty`

<p/><!-- -->

> ### <a name="option-2-python-other-web-servers"></a>2. lehetőség: Python vagy egyéb webkiszolgálókon
> Győződjön meg arról, hogy a telepített [Python](https://www.python.org/downloads/), hajtsa végre az alábbi hello lépést:
> - Az alkalmazás egy mappában toohost létrehozása.


## <a name="create-your-single-page-applications-ui"></a>Az egyetlen oldal alkalmazás felhasználói Felületüket létrehozni
1.  Hozzon létre egy *index.html* a JavaScript SPA fájlt. Ha használja a Visual Studio, válassza hello project (projekt gyökérmappájában), kattintson a jobb gombbal, és válassza ki: `Add`  >  `New Item`  >  `HTML page` és adjon neki nevet index.html
2.  Adja hozzá a következő kódlap tooyour hello:
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
