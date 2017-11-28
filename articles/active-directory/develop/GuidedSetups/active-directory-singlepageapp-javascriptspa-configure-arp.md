---
title: "Az Azure AD v2 JS SPA interaktív telepítő - a (ARP) konfigurálása |} Microsoft Docs"
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
ms.openlocfilehash: 708f4ff606d79639de979918a9cacd4ed75db311
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
## <a name="add-the-applications-registration-information-to-your-app"></a><span data-ttu-id="d1296-103">Az alkalmazás regisztrációs adatok hozzáadása az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="d1296-103">Add the application’s registration information to your App</span></span>

<span data-ttu-id="d1296-104">Ebben a lépésben azt kell konfigurálja az átirányítási URL-cím az alkalmazás regisztrációs adatait, és adja meg az alkalmazásazonosítót a JavaScript SPA-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="d1296-104">In this step, you need to configure the Redirect URL of your application registration information and then add the Application Id to your JavaScript SPA application.</span></span>

### <a name="configure-redirect-url"></a><span data-ttu-id="d1296-105">Az átirányítási URL-CÍMEK konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d1296-105">Configure redirect URL</span></span>

<span data-ttu-id="d1296-106">Konfigurálja a `Redirect URL` a index.html lap a webkiszolgáló URL-CÍMÉT a fenti mezőben, majd kattintson az *frissítés*.</span><span class="sxs-lookup"><span data-stu-id="d1296-106">Configure the `Redirect URL` field above with the URL for your index.html page based on your web server, then click *Update*.</span></span>


> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a><span data-ttu-id="d1296-107">A Visual Studio utasításokat átirányítási URL-cím beszerzése</span><span class="sxs-lookup"><span data-stu-id="d1296-107">Visual Studio instructions for obtaining redirect URL</span></span>
> <span data-ttu-id="d1296-108">Az átirányítási URL-cím beszerzéséhez hajtsa végre az alábbi utasításokat:</span><span class="sxs-lookup"><span data-stu-id="d1296-108">To obtain your redirect URL, follow the instructions below:</span></span>
> 1.    <span data-ttu-id="d1296-109">A *Megoldáskezelőben*, válassza ki a projektet, és tekintse meg a `Properties` ablakban (Ha nem látja a Tulajdonságok ablak, nyomja le az `F4`)</span><span class="sxs-lookup"><span data-stu-id="d1296-109">In *Solution Explorer*, select the project and look at the `Properties` window (if you don’t see a Properties window, press `F4`)</span></span>
> 2.    <span data-ttu-id="d1296-110">Másolja a értéket `URL` a vágólapra:</span><span class="sxs-lookup"><span data-stu-id="d1296-110">Copy the value from `URL` to the clipboard:</span></span><br/> <span data-ttu-id="d1296-111">![Projekt tulajdonságai](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)</span><span class="sxs-lookup"><span data-stu-id="d1296-111">![Project properties](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)</span></span><br />
> 3.    <span data-ttu-id="d1296-112">Illessze be az érték egy `Redirect URL` Ez a lap tetején kattintson a`Update`</span><span class="sxs-lookup"><span data-stu-id="d1296-112">Paste the value as a `Redirect URL` on the top of this page, then click `Update`</span></span>

<p/>

> #### <a name="setting-redirect-url-for-python"></a><span data-ttu-id="d1296-113">Az átirányítási URL-ként Python</span><span class="sxs-lookup"><span data-stu-id="d1296-113">Setting Redirect URL for Python</span></span>
> <span data-ttu-id="d1296-114">A Python a web server port parancssorból állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="d1296-114">For Python, you can set the web server port via command line.</span></span> <span data-ttu-id="d1296-115">Ez az interaktív telepítés a 8080-hivatkozásban, de a kezelőfelület szabadon használhatja az összes rendelkezésre álló portot használ.</span><span class="sxs-lookup"><span data-stu-id="d1296-115">This guided setup uses the port 8080 for reference but feel free to use any other port available.</span></span> <span data-ttu-id="d1296-116">Minden esetben kövesse az alábbi utasításokat egy átirányítási URL-címet az alkalmazás-regisztrációs adatainak beállításához:</span><span class="sxs-lookup"><span data-stu-id="d1296-116">In any case, follow the instructions below to set up a redirect URL in the application registration information:</span></span><br/>
> <span data-ttu-id="d1296-117">Állítsa be `http://localhost:8080/` , egy `Redirect URL` tetején a lap, vagy használjon `http://localhost:[port]/` egyéni TCP-port használata (Ha *[port]* a egyéni TCP-port száma), és kattintson a "Frissítés"</span><span class="sxs-lookup"><span data-stu-id="d1296-117">Set `http://localhost:8080/` as a `Redirect URL` on the top of this page, or use `http://localhost:[port]/` if you are using a custom TCP port (where *[port]* is the custom TCP port number), and then click 'Update'</span></span>

### <a name="configure-your-javascript-spa-application"></a><span data-ttu-id="d1296-118">A JavaScript SPA-alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d1296-118">Configure your JavaScript SPA application</span></span>

1.  <span data-ttu-id="d1296-119">Hozzon létre egy fájlt `msalconfig.js` tartalmazó az alkalmazás regisztrációs adatait.</span><span class="sxs-lookup"><span data-stu-id="d1296-119">Create a file named `msalconfig.js` containing the application registration information.</span></span> <span data-ttu-id="d1296-120">Ha a Visual Studio használ, válassza ki a project (projekt gyökérmappájában), kattintson a jobb gombbal és válassza ki: `Add`  >  `New Item`  >  `JavaScript File`.</span><span class="sxs-lookup"><span data-stu-id="d1296-120">If you are using Visual Studio, select the project (project root folder), right-click and select: `Add` > `New Item` > `JavaScript File`.</span></span> <span data-ttu-id="d1296-121">Nevezze el`msalconfig.js`</span><span class="sxs-lookup"><span data-stu-id="d1296-121">Name it `msalconfig.js`</span></span>
2.  <span data-ttu-id="d1296-122">Adja hozzá a következő kódot a `msalconfig.js` fájlt:</span><span class="sxs-lookup"><span data-stu-id="d1296-122">Add the following code to your `msalconfig.js` file:</span></span>

```javascript
var msalconfig = {
    clientID: "[Enter the application Id here]",
    redirectUri: location.origin
};
``` 
