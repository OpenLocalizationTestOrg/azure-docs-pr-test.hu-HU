---
title: "Azure-függvény Alkalmazásbeállítások aaaConfigure |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure Azure működni Alkalmazásbeállítások."
services: 
documentationcenter: .net
author: rachelappel
manager: erikre
editor: 
ms.assetid: 81eb04f8-9a27-45bb-bf24-9ab6c30d205c
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 04/23/2017
ms.author: glenga
ms.openlocfilehash: 539e203ac449061ef3ceae5e93df3bdbb326e43b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-a-function-app-in-hello-azure-portal"></a><span data-ttu-id="a13c7-103">Hogyan toomanage egy függvény alkalmazás hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="a13c7-103">How toomanage a function app in hello Azure portal</span></span> 

<span data-ttu-id="a13c7-104">Az Azure Functions egy függvény app kontextust biztosít a hello végrehajtása az egyéni függvényei.</span><span class="sxs-lookup"><span data-stu-id="a13c7-104">In Azure Functions, a function app provides hello execution context for your individual functions.</span></span> <span data-ttu-id="a13c7-105">Függvény app viselkedések tooall funkciók egy adott funkció alkalmazás által üzemeltetett alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="a13c7-105">Function app behaviors apply tooall functions hosted by a given function app.</span></span> <span data-ttu-id="a13c7-106">Ez a témakör ismerteti, hogyan tooconfigure és hello Azure-portálon függvény alkalmazások kezelése.</span><span class="sxs-lookup"><span data-stu-id="a13c7-106">This topic describes how tooconfigure and manage your function apps in hello Azure portal.</span></span>

<span data-ttu-id="a13c7-107">toobegin, nyissa meg toohello [Azure-portálon](http://portal.azure.com) , jelentkezzen be Azure-fiók tooyour.</span><span class="sxs-lookup"><span data-stu-id="a13c7-107">toobegin, go toohello [Azure portal](http://portal.azure.com) and sign in tooyour Azure account.</span></span> <span data-ttu-id="a13c7-108">Hello hello portál hello felső keresősávban írja be a függvény alkalmazás hello nevét, és hello listából válassza ki.</span><span class="sxs-lookup"><span data-stu-id="a13c7-108">In hello search bar at hello top of hello portal, type hello name of your function app and select it from hello list.</span></span> <span data-ttu-id="a13c7-109">Miután kiválasztotta a függvény alkalmazás, a következő lap hello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="a13c7-109">After selecting your function app, you see hello following page:</span></span>

![A hello Azure-portálon függvény alkalmazás – áttekintés](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-main.png)

## <span data-ttu-id="a13c7-111"><a name="manage-app-service-settings"></a>Függvény app beállítások lap</span><span class="sxs-lookup"><span data-stu-id="a13c7-111"><a name="manage-app-service-settings"></a>Function app settings tab</span></span>

![Függvény app áttekintése a hello Azure-portálon.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

<span data-ttu-id="a13c7-113">Hello **beállítások** lap, ahol frissítheti a függvény alkalmazás által használt hello funkciók futásidejű verzióját.</span><span class="sxs-lookup"><span data-stu-id="a13c7-113">hello **Settings** tab is where you can update hello Functions runtime version used by your function app.</span></span> <span data-ttu-id="a13c7-114">Akkor is hello állomás használt kulcsok toorestrict HTTP hozzáférés tooall funkciók hello függvény alkalmazás által üzemeltetett kezelhetik.</span><span class="sxs-lookup"><span data-stu-id="a13c7-114">It is also where you manage hello host keys used toorestrict HTTP access tooall functions hosted by hello function app.</span></span>

<span data-ttu-id="a13c7-115">Funkciók fogyasztás futtató és az App Service csomagokban üzemeltető támogatja.</span><span class="sxs-lookup"><span data-stu-id="a13c7-115">Functions supports both Consumption hosting and App Service hosting plans.</span></span> <span data-ttu-id="a13c7-116">További információkért lásd: [válasszon hello megfelelő service-csomag az Azure Functions](functions-scale.md).</span><span class="sxs-lookup"><span data-stu-id="a13c7-116">For more information, see [Choose hello correct service plan for Azure Functions](functions-scale.md).</span></span> <span data-ttu-id="a13c7-117">Hello fogyasztás tervben jobb kiszámítható funkciók lehetővé teszi a platform-használatát korlátozása úgy, hogy a napi használat kvóta GB-másodperc.</span><span class="sxs-lookup"><span data-stu-id="a13c7-117">For better predictability in hello Consumption plan, Functions lets you limit platform usage by setting a daily usage quota, in gigabytes-seconds.</span></span> <span data-ttu-id="a13c7-118">Hello napi memóriahasználati kvóta elérésekor hello függvény alkalmazás leállt.</span><span class="sxs-lookup"><span data-stu-id="a13c7-118">Once hello daily usage quota is reached, hello function app is stopped.</span></span> <span data-ttu-id="a13c7-119">Egy függvény app hello költségeik kvóta elérése miatt leállt újból engedélyezhető a hello naponta költségeik kvóta hello létrehozó azonos összefüggésben.</span><span class="sxs-lookup"><span data-stu-id="a13c7-119">A function app stopped as a result of reaching hello spending quota can be re-enabled from hello same context as establishing hello daily spending quota.</span></span> <span data-ttu-id="a13c7-120">Lásd: hello [árképzést ismertető oldalra Azure Functions](http://azure.microsoft.com/pricing/details/functions/) számlázással kapcsolatos részletekért.</span><span class="sxs-lookup"><span data-stu-id="a13c7-120">See hello [Azure Functions pricing page](http://azure.microsoft.com/pricing/details/functions/) for details on billing.</span></span>   

## <a name="platform-features-tab"></a><span data-ttu-id="a13c7-121">Platform szolgáltatások lap</span><span class="sxs-lookup"><span data-stu-id="a13c7-121">Platform features tab</span></span>

![Függvény alkalmazás platform szolgáltatások lapon.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-features-tab.png)

<span data-ttu-id="a13c7-123">Függvény alkalmazások futnak, és tartható fenn, hello Azure App Service platformon.</span><span class="sxs-lookup"><span data-stu-id="a13c7-123">Function apps run in, and are maintained, by hello Azure App Service platform.</span></span> <span data-ttu-id="a13c7-124">A függvény alkalmazások, a hozzáférés toomost hello szolgáltatást az Azure alapvető webszolgáltatási platform is rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="a13c7-124">As such, your function apps have access toomost of hello features of Azure's core web hosting platform.</span></span> <span data-ttu-id="a13c7-125">Hello **Platform funkciói** lap, ahol a hozzáférést, a függvény alkalmazásokban használható App Service platform hello számos szolgáltatása hello.</span><span class="sxs-lookup"><span data-stu-id="a13c7-125">hello **Platform features** tab is where you access hello many features of hello App Service platform that you can use in your function apps.</span></span> 

> [!NOTE]
> <span data-ttu-id="a13c7-126">Nem minden App Service-szolgáltatások érhetők el a hello fogyasztás üzemeltetési terv egy függvény alkalmazás futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="a13c7-126">Not all App Service features are available when a function app runs on hello Consumption hosting plan.</span></span>

<span data-ttu-id="a13c7-127">a következő hello az App Service-szolgáltatások hello Azure-portálon, amelyek hasznosak a funkciók összpontosít hello a témakör hátralévő része:</span><span class="sxs-lookup"><span data-stu-id="a13c7-127">hello rest of this topic focuses on hello following App Service features in hello Azure portal that are useful for Functions:</span></span>

+ [<span data-ttu-id="a13c7-128">App Service-szerkesztő</span><span class="sxs-lookup"><span data-stu-id="a13c7-128">App Service editor</span></span>](#editor)
+ [<span data-ttu-id="a13c7-129">Alkalmazásbeállítások</span><span class="sxs-lookup"><span data-stu-id="a13c7-129">Application settings</span></span>](#settings) 
+ [<span data-ttu-id="a13c7-130">Console</span><span class="sxs-lookup"><span data-stu-id="a13c7-130">Console</span></span>](#console)
+ [<span data-ttu-id="a13c7-131">Speciális eszközöket (a Kudu)</span><span class="sxs-lookup"><span data-stu-id="a13c7-131">Advanced tools (Kudu)</span></span>](#kudu)
+ [<span data-ttu-id="a13c7-132">Telepítési lehetőségek</span><span class="sxs-lookup"><span data-stu-id="a13c7-132">Deployment options</span></span>](#deployment)
+ [<span data-ttu-id="a13c7-133">CORS</span><span class="sxs-lookup"><span data-stu-id="a13c7-133">CORS</span></span>](#cors)
+ [<span data-ttu-id="a13c7-134">Hitelesítés</span><span class="sxs-lookup"><span data-stu-id="a13c7-134">Authentication</span></span>](#auth)
+ [<span data-ttu-id="a13c7-135">API-definíció</span><span class="sxs-lookup"><span data-stu-id="a13c7-135">API definition</span></span>](#swagger)

<span data-ttu-id="a13c7-136">További információ az App Service-beállítások segítségével toowork lásd: [Azure App Service beállításainak konfigurálása](../app-service-web/web-sites-configure.md).</span><span class="sxs-lookup"><span data-stu-id="a13c7-136">For more information about how toowork with App Service settings, see [Configure Azure App Service Settings](../app-service-web/web-sites-configure.md).</span></span>

### <span data-ttu-id="a13c7-137"><a name="editor"></a>App Service-szerkesztő</span><span class="sxs-lookup"><span data-stu-id="a13c7-137"><a name="editor"></a>App Service Editor</span></span>

| | |
|-|-|
| ![Függvény alkalmazás az App Service-szerkesztőt.](./media/functions-how-to-use-azure-function-app-settings/function-app-appsvc-editor.png)  | <span data-ttu-id="a13c7-139">App Service-szerkesztő hello egy speciális-portal szerkesztő toomodify JSON konfigurációs fájlokat és a kód fájlok egyaránt használható.</span><span class="sxs-lookup"><span data-stu-id="a13c7-139">hello App Service editor is an advanced in-portal editor that you can use toomodify JSON configuration files and code files alike.</span></span> <span data-ttu-id="a13c7-140">Ez a beállítás egy külön böngészőlapon alapvető szerkesztővel indít.</span><span class="sxs-lookup"><span data-stu-id="a13c7-140">Choosing this option launches a separate browser tab with a basic editor.</span></span> <span data-ttu-id="a13c7-141">Lehetővé teszi a hello Git tárház, futtatása és hibakeresési kód toointegrate, és funkció beállításainak módosításához.</span><span class="sxs-lookup"><span data-stu-id="a13c7-141">This enables you toointegrate with hello Git repository, run and debug code, and modify function app settings.</span></span> <span data-ttu-id="a13c7-142">A szerkesztő hello alapértelmezett függvény alkalmazás paneljének összehasonlítja a funkciók bővített fejlesztési környezetet biztosít.</span><span class="sxs-lookup"><span data-stu-id="a13c7-142">This editor provides an enhanced development environment for your functions compared with hello default function app blade.</span></span>    |

![hello App Service-szerkesztő](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-appservice-editor.png)

### <span data-ttu-id="a13c7-144"><a name="settings"></a>Alkalmazásbeállítások</span><span class="sxs-lookup"><span data-stu-id="a13c7-144"><a name="settings"></a>Application settings</span></span>

| | |
|-|-|
| ![Alkalmazás függvény Alkalmazásbeállítások.](./media/functions-how-to-use-azure-function-app-settings/function-app-application-settings.png) | <span data-ttu-id="a13c7-146">App Service hello **Alkalmazásbeállítások** panel, ahol konfigurálhatja és kezelheti a keretrendszer-verziók, távoli hibakeresés, Alkalmazásbeállítások és kapcsolati karakterláncok.</span><span class="sxs-lookup"><span data-stu-id="a13c7-146">hello App Service **Application settings** blade is where you configure and manage framework versions, remote debugging, app settings, and connection strings.</span></span> <span data-ttu-id="a13c7-147">A függvény alkalmazás integrációjához át az egyéb Azure és a harmadik féltől származó szolgáltatással, módosíthatja itt ezeket a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="a13c7-147">When you integrate your function app with other Azure and third-party services, you can modify those settings here.</span></span> |

![Az Alkalmazásbeállítások konfigurálása](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-settings.png)

### <span data-ttu-id="a13c7-149"><a name="console"></a>Konzol</span><span class="sxs-lookup"><span data-stu-id="a13c7-149"><a name="console"></a>Console</span></span>

| | |
|-|-|
| ![Függvény app konzol hello Azure-portálon](./media/functions-how-to-use-azure-function-app-settings/function-app-console.png) | <span data-ttu-id="a13c7-151">hello-portal konzol az ideális fejlesztői eszközét esetén szeretné toointeract függvény alkalmazása hello parancssorból.</span><span class="sxs-lookup"><span data-stu-id="a13c7-151">hello in-portal console is an ideal developer tool when you prefer toointeract with your function app from hello command line.</span></span> <span data-ttu-id="a13c7-152">Általános jellegű parancsok közé tartozik a címtár és a fájl létrehozásának és navigációs, valamint parancsfájlok végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="a13c7-152">Common commands include directory and file creation and navigation, as well as executing batch files and scripts.</span></span> |

![Függvény app konzol](./media/functions-how-to-use-azure-function-app-settings/configure-function-console.png)

### <span data-ttu-id="a13c7-154"><a name="kudu"></a>Speciális eszközöket (a Kudu)</span><span class="sxs-lookup"><span data-stu-id="a13c7-154"><a name="kudu"></a>Advanced tools (Kudu)</span></span>

| | |
|-|-|
| ![Függvény alkalmazás Kudu hello Azure-portálon](./media/functions-how-to-use-azure-function-app-settings/function-app-advanced-tools.png) | <span data-ttu-id="a13c7-156">hello speciális eszközök az App Service (más néven a Kudu) hozzáférést biztosítson tooadvanced felügyeleti funkciókat az függvény alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a13c7-156">hello advanced tools for App Service (also known as Kudu) provide access tooadvanced administrative features of your function app.</span></span> <span data-ttu-id="a13c7-157">Kudu kezelheti az Rendszerinformáció, Alkalmazásbeállítások, környezeti változókat, helyhez kiterjesztések, HTTP-fejlécek és kiszolgáló-változók.</span><span class="sxs-lookup"><span data-stu-id="a13c7-157">From Kudu, you manage system information, app settings, environment variables, site extensions, HTTP headers, and server variables.</span></span> <span data-ttu-id="a13c7-158">Is **Kudu** toohello SCM végpont függvény alkalmazás, például tallózással`https://<myfunctionapp>.scm.azurewebsites.net/`</span><span class="sxs-lookup"><span data-stu-id="a13c7-158">You can also launch **Kudu** by browsing toohello SCM endpoint for your function app, like `https://<myfunctionapp>.scm.azurewebsites.net/`</span></span> |

![A Kudu konfigurálása](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-kudu.png)


### <a name="a-namedeploymentdeployment-options"></a><span data-ttu-id="a13c7-160"><a name="deployment">Telepítési lehetőségek</span><span class="sxs-lookup"><span data-stu-id="a13c7-160"><a name="deployment">Deployment options</span></span>

| | |
|-|-|
| ![Függvény alkalmazás központi telepítési beállítások hello Azure-portálon](./media/functions-how-to-use-azure-function-app-settings/function-app-deployment-source.png) | <span data-ttu-id="a13c7-162">Funkciók lehetővé teszi, hogy a helyi számítógépen a funkciókódot fejleszthet.</span><span class="sxs-lookup"><span data-stu-id="a13c7-162">Functions lets you develop your function code on your local machine.</span></span> <span data-ttu-id="a13c7-163">A helyi függvény app projektet tooAzure majd feltöltheti.</span><span class="sxs-lookup"><span data-stu-id="a13c7-163">You can then upload your local function app project tooAzure.</span></span> <span data-ttu-id="a13c7-164">Ezenkívül tootraditional FTP feltöltés funkciók lehetővé teszi a népszerű folyamatos integrációt megoldások, például a Githubon, VSTS, Dropbox, Bitbucket és mások segítségével függvény alkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="a13c7-164">In addition tootraditional FTP upload, Functions lets you deploy your function app using popular continuous integration solutions, like GitHub, VSTS, Dropbox, Bitbucket, and others.</span></span> <span data-ttu-id="a13c7-165">További információkért lásd: [folyamatos üzembe helyezés az Azure Functions](functions-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="a13c7-165">For more information, see [Continuous deployment for Azure Functions](functions-continuous-deployment.md).</span></span> <span data-ttu-id="a13c7-166">FTP- vagy helyi Git segítségével manuálisan tooupload is kell [az üzembe helyezési hitelesítő adatok konfigurálása](functions-continuous-deployment.md#credentials).</span><span class="sxs-lookup"><span data-stu-id="a13c7-166">tooupload manually using FTP or local Git, you also must [configure your deployment credentials](functions-continuous-deployment.md#credentials).</span></span> |


### <span data-ttu-id="a13c7-167"><a name="cors"></a>A CORS</span><span class="sxs-lookup"><span data-stu-id="a13c7-167"><a name="cors"></a>CORS</span></span>

| | |
|-|-|
| ![Függvény alkalmazás CORS hello Azure-portálon](./media/functions-how-to-use-azure-function-app-settings/function-app-cors.png) | <span data-ttu-id="a13c7-169">tooprevent rosszindulatú kód végrehajtása a szolgáltatások, az App Service-blokkok tooyour függvény alkalmazások meghívja a külső forrásból.</span><span class="sxs-lookup"><span data-stu-id="a13c7-169">tooprevent malicious code execution in your services, App Service blocks calls tooyour function apps from external sources.</span></span> <span data-ttu-id="a13c7-170">Funkciók támogatja az eltérő eredetű erőforrások megosztása (CORS) toolet adhat meg egy "engedélyezett" az engedélyezett eredeteket, amelyből a funkciók fogadhat távoli kérelmek.</span><span class="sxs-lookup"><span data-stu-id="a13c7-170">Functions supports cross-origin resource sharing (CORS) toolet you define a "whitelist" of allowed origins from which your functions can accept remote requests.</span></span>  |

![Függvény alkalmazás konfigurálása CORS](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-cors.png)

### <span data-ttu-id="a13c7-172"><a name="auth"></a>Hitelesítés</span><span class="sxs-lookup"><span data-stu-id="a13c7-172"><a name="auth"></a>Authentication</span></span>

| | |
|-|-|
| ![Függvény app hitelesítéshez hello Azure-portálon](./media/functions-how-to-use-azure-function-app-settings/function-app-authentication.png) | <span data-ttu-id="a13c7-174">Egy HTTP-eseményindítóval használatakor a funkciók megkövetelheti hívások toofirst hitelesíthető.</span><span class="sxs-lookup"><span data-stu-id="a13c7-174">When functions use an HTTP trigger, you can require calls toofirst be authenticated.</span></span> <span data-ttu-id="a13c7-175">App Service támogatja az Azure Active Directory-hitelesítés és be kell jelentkezniük a közösségi szolgáltatók, köztük a Facebook, a Microsoft és a Twitter.</span><span class="sxs-lookup"><span data-stu-id="a13c7-175">App Service supports Azure Active Directory authentication and sign in with social providers, such as Facebook, Microsoft, and Twitter.</span></span> <span data-ttu-id="a13c7-176">További részletek az adott hitelesítési szolgáltatókat konfigurál: [Azure App Service hitelesítés áttekintése](../app-service/app-service-authentication-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a13c7-176">For details on configuring specific authentication providers, see [Azure App Service authentication overview](../app-service/app-service-authentication-overview.md).</span></span> |

![Egy függvény alkalmazások hitelesítésének konfigurálása](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-authentication.png)


### <span data-ttu-id="a13c7-178"><a name="swagger"></a>API-definíció</span><span class="sxs-lookup"><span data-stu-id="a13c7-178"><a name="swagger"></a>API definition</span></span>

| | |
|-|-|
| ![Függvény app API swagger-definíciójában hello Azure-portálon](./media/functions-how-to-use-azure-function-app-settings/function-app-api-definition.png) | <span data-ttu-id="a13c7-180">Funkciókat támogatja a Swagger tooallow ügyfelek toomore könnyen használni a HTTP-eseményindítókkal aktivált függvényeket.</span><span class="sxs-lookup"><span data-stu-id="a13c7-180">Functions supports Swagger tooallow clients toomore easily consume your HTTP-triggered functions.</span></span> <span data-ttu-id="a13c7-181">A Swagger API-definíciók létrehozásával kapcsolatos további információkért látogasson el a [Ismerkedés az API Apps, az ASP.NET és az Azure-ban Swagger](../app-service-api/app-service-api-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="a13c7-181">For more information on creating API definitions with Swagger, visit [Get Started with API Apps, ASP.NET, and Swagger in Azure](../app-service-api/app-service-api-dotnet-get-started.md).</span></span> <span data-ttu-id="a13c7-182">Funkciók proxyk toodefine egyetlen API felülete több függvények is használható.</span><span class="sxs-lookup"><span data-stu-id="a13c7-182">You can also use Functions Proxies toodefine a single API surface for multiple functions.</span></span> <span data-ttu-id="a13c7-183">További információkért lásd: [használata az Azure Functions proxyk](functions-proxies.md).</span><span class="sxs-lookup"><span data-stu-id="a13c7-183">For more information, see [Working with Azure Functions Proxies](functions-proxies.md).</span></span> |

![Függvény App API konfigurálása](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-apidef.png)



## <a name="next-steps"></a><span data-ttu-id="a13c7-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a13c7-185">Next steps</span></span>

+ [<span data-ttu-id="a13c7-186">Az Azure App Service-beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a13c7-186">Configure Azure App Service Settings</span></span>](../app-service-web/web-sites-configure.md)
+ [<span data-ttu-id="a13c7-187">Azure Functions – folyamatos üzembe helyezés</span><span class="sxs-lookup"><span data-stu-id="a13c7-187">Continuous deployment for Azure Functions</span></span>](functions-continuous-deployment.md)



