---
title: "aaaAzure AD v2 JS SPA interaktív telepítés - konfigurálása (ARP) |} Microsoft Docs"
description: "Hogyan JavaScript SPA-alkalmazások az Azure Active Directory-v2 végpontja (ARP) hozzáférési jogkivonatok igénylő API meghívása"
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
ms.openlocfilehash: 157f4e342cd684294e24da6ee1fad8a7c2fc266a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="add-hello-applications-registration-information-tooyour-app"></a><span data-ttu-id="6f7f3-103">Hello alkalmazás regisztrációs információ tooyour alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6f7f3-103">Add hello application’s registration information tooyour App</span></span>

<span data-ttu-id="6f7f3-104">Ebben a lépésben kell tooconfigure hello átirányítási URL-CÍMÉT a alkalmazás regisztrációs adatait, és adja hozzá a hello alkalmazásazonosító tooyour JavaScript SPA-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="6f7f3-104">In this step, you need tooconfigure hello Redirect URL of your application registration information and then add hello Application Id tooyour JavaScript SPA application.</span></span>

### <a name="configure-redirect-url"></a><span data-ttu-id="6f7f3-105">Az átirányítási URL-CÍMEK konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6f7f3-105">Configure redirect URL</span></span>

<span data-ttu-id="6f7f3-106">Hello konfigurálása `Redirect URL` hello URL-címe alapján a webkiszolgáló index.html lapon a fenti mezőben, majd kattintson az *frissítés*.</span><span class="sxs-lookup"><span data-stu-id="6f7f3-106">Configure hello `Redirect URL` field above with hello URL for your index.html page based on your web server, then click *Update*.</span></span>


> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a><span data-ttu-id="6f7f3-107">A Visual Studio utasításokat átirányítási URL-cím beszerzése</span><span class="sxs-lookup"><span data-stu-id="6f7f3-107">Visual Studio instructions for obtaining redirect URL</span></span>
> <span data-ttu-id="6f7f3-108">tooobtain az átirányítási URL-cím, kövesse az alábbi hello utasítások:</span><span class="sxs-lookup"><span data-stu-id="6f7f3-108">tooobtain your redirect URL, follow hello instructions below:</span></span>
> 1.    <span data-ttu-id="6f7f3-109">A *Megoldáskezelőben*hello projekt között, nézze meg hello `Properties` ablakban (Ha nem látja a Tulajdonságok ablak, nyomja meg az `F4`)</span><span class="sxs-lookup"><span data-stu-id="6f7f3-109">In *Solution Explorer*, select hello project and look at hello `Properties` window (if you don’t see a Properties window, press `F4`)</span></span>
> 2.    <span data-ttu-id="6f7f3-110">Másolja a hello értéket `URL` toohello vágólapra:</span><span class="sxs-lookup"><span data-stu-id="6f7f3-110">Copy hello value from `URL` toohello clipboard:</span></span><br/> <span data-ttu-id="6f7f3-111">![Projekt tulajdonságai](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)</span><span class="sxs-lookup"><span data-stu-id="6f7f3-111">![Project properties](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)</span></span><br />
> 3.    <span data-ttu-id="6f7f3-112">Illessze be a hello értéke megegyezik egy `Redirect URL` hello az oldal tetején, majd kattintson`Update`</span><span class="sxs-lookup"><span data-stu-id="6f7f3-112">Paste hello value as a `Redirect URL` on hello top of this page, then click `Update`</span></span>

<p/>

> #### <a name="setting-redirect-url-for-python"></a><span data-ttu-id="6f7f3-113">Az átirányítási URL-ként Python</span><span class="sxs-lookup"><span data-stu-id="6f7f3-113">Setting Redirect URL for Python</span></span>
> <span data-ttu-id="6f7f3-114">Python hello a webtartalom-kiszolgáló portja parancssorból állíthatók be.</span><span class="sxs-lookup"><span data-stu-id="6f7f3-114">For Python, you can set hello web server port via command line.</span></span> <span data-ttu-id="6f7f3-115">Az interaktív telepítés használja a 8080-as porton hello referenciaként, de érzi, hogy a szabad toouse összes rendelkezésre álló portot.</span><span class="sxs-lookup"><span data-stu-id="6f7f3-115">This guided setup uses hello port 8080 for reference but feel free toouse any other port available.</span></span> <span data-ttu-id="6f7f3-116">Minden esetben kövesse az alábbi tooset hello alkalmazás regisztrációs adatai egy átirányítási URL-cím mentése hello utasításokat:</span><span class="sxs-lookup"><span data-stu-id="6f7f3-116">In any case, follow hello instructions below tooset up a redirect URL in hello application registration information:</span></span><br/>
> <span data-ttu-id="6f7f3-117">Állítsa be `http://localhost:8080/` , egy `Redirect URL` az oldal tetején hello, vagy használjon `http://localhost:[port]/` egyéni TCP-port használata (Ha *[port]* hello egyéni TCP-port száma), és kattintson a "Frissítés"</span><span class="sxs-lookup"><span data-stu-id="6f7f3-117">Set `http://localhost:8080/` as a `Redirect URL` on hello top of this page, or use `http://localhost:[port]/` if you are using a custom TCP port (where *[port]* is hello custom TCP port number), and then click 'Update'</span></span>

### <a name="configure-your-javascript-spa-application"></a><span data-ttu-id="6f7f3-118">A JavaScript SPA-alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6f7f3-118">Configure your JavaScript SPA application</span></span>

1.  <span data-ttu-id="6f7f3-119">Hozzon létre egy fájlt `msalconfig.js` tartalmazó hello alkalmazás regisztrációs adatokat.</span><span class="sxs-lookup"><span data-stu-id="6f7f3-119">Create a file named `msalconfig.js` containing hello application registration information.</span></span> <span data-ttu-id="6f7f3-120">Ha használja a Visual Studio, válassza hello project (projekt gyökérmappájában), kattintson a jobb gombbal, és válassza ki: `Add`  >  `New Item`  >  `JavaScript File`.</span><span class="sxs-lookup"><span data-stu-id="6f7f3-120">If you are using Visual Studio, select hello project (project root folder), right-click and select: `Add` > `New Item` > `JavaScript File`.</span></span> <span data-ttu-id="6f7f3-121">Nevezze el`msalconfig.js`</span><span class="sxs-lookup"><span data-stu-id="6f7f3-121">Name it `msalconfig.js`</span></span>
2.  <span data-ttu-id="6f7f3-122">Adja hozzá a következő kód tooyour hello `msalconfig.js` fájlt:</span><span class="sxs-lookup"><span data-stu-id="6f7f3-122">Add hello following code tooyour `msalconfig.js` file:</span></span>

```javascript
var msalconfig = {
    clientID: "[Enter hello application Id here]",
    redirectUri: location.origin
};
``` 
