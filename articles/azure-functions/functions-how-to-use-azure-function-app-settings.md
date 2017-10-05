---
title: "Az Azure-függvény Alkalmazásbeállítások konfigurálása |} Microsoft Docs"
description: "Útmutató: Azure funkció beállításainak konfigurálása."
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
ms.openlocfilehash: 3b23011f66592151d517d61bf806da8743f38e03
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-manage-a-function-app-in-the-azure-portal"></a><span data-ttu-id="c0f7d-103">Egy függvény alkalmazást az Azure portálon kezelése</span><span class="sxs-lookup"><span data-stu-id="c0f7d-103">How to manage a function app in the Azure portal</span></span> 

<span data-ttu-id="c0f7d-104">Az Azure Functions egy függvény alkalmazást a végrehajtási környezet biztosít a az egyes funkcióihoz.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-104">In Azure Functions, a function app provides the execution context for your individual functions.</span></span> <span data-ttu-id="c0f7d-105">Függvény app viselkedések egy adott funkció alkalmazás által futtatott összes funkciójának vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-105">Function app behaviors apply to all functions hosted by a given function app.</span></span> <span data-ttu-id="c0f7d-106">Ez a témakör ismerteti, hogyan konfigurálja és kezelje a függvény alkalmazások az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-106">This topic describes how to configure and manage your function apps in the Azure portal.</span></span>

<span data-ttu-id="c0f7d-107">Első lépésként nyissa meg a [Azure-portálon](http://portal.azure.com) és az Azure-fiókjába történő bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-107">To begin, go to the [Azure portal](http://portal.azure.com) and sign in to your Azure account.</span></span> <span data-ttu-id="c0f7d-108">A portál tetején a keresősávba írja be a függvényalkalmazás nevét, majd válassza ki a listáról.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-108">In the search bar at the top of the portal, type the name of your function app and select it from the list.</span></span> <span data-ttu-id="c0f7d-109">Miután kiválasztotta a függvény alkalmazás, a következő lap jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="c0f7d-109">After selecting your function app, you see the following page:</span></span>

![Az Azure portálon függvény app – áttekintés](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-main.png)

## <span data-ttu-id="c0f7d-111"><a name="manage-app-service-settings"></a>Függvény app beállítások lap</span><span class="sxs-lookup"><span data-stu-id="c0f7d-111"><a name="manage-app-service-settings"></a>Function app settings tab</span></span>

![Függvény-alkalmazás – áttekintés az Azure portálon.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

<span data-ttu-id="c0f7d-113">A **beállítások** lap, ahol frissítheti a függvény alkalmazás által használt funkciók futásidejű verzió.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-113">The **Settings** tab is where you can update the Functions runtime version used by your function app.</span></span> <span data-ttu-id="c0f7d-114">Akkor is, amelyeken kezelheti az állomások kulcsait, HTTP-hozzáférés korlátozása a függvény alkalmazás által futtatott összes funkciójának használatával.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-114">It is also where you manage the host keys used to restrict HTTP access to all functions hosted by the function app.</span></span>

<span data-ttu-id="c0f7d-115">Funkciók fogyasztás futtató és az App Service csomagokban üzemeltető támogatja.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-115">Functions supports both Consumption hosting and App Service hosting plans.</span></span> <span data-ttu-id="c0f7d-116">További információkért lásd: [válassza ki a megfelelő service-csomag az Azure Functions](functions-scale.md).</span><span class="sxs-lookup"><span data-stu-id="c0f7d-116">For more information, see [Choose the correct service plan for Azure Functions](functions-scale.md).</span></span> <span data-ttu-id="c0f7d-117">A felhasználási tervben jobb kiszámítható funkciók lehetővé teszi a platform-használatát korlátozása úgy, hogy a napi használat kvóta GB-másodperc.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-117">For better predictability in the Consumption plan, Functions lets you limit platform usage by setting a daily usage quota, in gigabytes-seconds.</span></span> <span data-ttu-id="c0f7d-118">A napi memóriahasználati kvóta elérésekor, a függvény alkalmazás leállt.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-118">Once the daily usage quota is reached, the function app is stopped.</span></span> <span data-ttu-id="c0f7d-119">Egy függvény alkalmazást a költségkeret kvóta elérése miatt leállt az ugyanabban a környezetben, mint a kvóta költségeik napi létrehozó újra engedélyezni lehet.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-119">A function app stopped as a result of reaching the spending quota can be re-enabled from the same context as establishing the daily spending quota.</span></span> <span data-ttu-id="c0f7d-120">Tekintse meg a [árképzést ismertető oldalra Azure Functions](http://azure.microsoft.com/pricing/details/functions/) számlázással kapcsolatos részletekért.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-120">See the [Azure Functions pricing page](http://azure.microsoft.com/pricing/details/functions/) for details on billing.</span></span>   

## <a name="platform-features-tab"></a><span data-ttu-id="c0f7d-121">Platform szolgáltatások lap</span><span class="sxs-lookup"><span data-stu-id="c0f7d-121">Platform features tab</span></span>

![Függvény alkalmazás platform szolgáltatások lapon.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-features-tab.png)

<span data-ttu-id="c0f7d-123">Függvény alkalmazások futnak, és tartható fenn, az Azure App Service platformon.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-123">Function apps run in, and are maintained, by the Azure App Service platform.</span></span> <span data-ttu-id="c0f7d-124">A függvény alkalmazásokhoz, Azure alapvető webszolgáltatási platform részeit, a legtöbb hozzáféréssel rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-124">As such, your function apps have access to most of the features of Azure's core web hosting platform.</span></span> <span data-ttu-id="c0f7d-125">A **Platform funkciói** lapon látható, ahol az App Service platform, amely a függvény alkalmazásokban használható sok funkcióhoz férhet hozzá.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-125">The **Platform features** tab is where you access the many features of the App Service platform that you can use in your function apps.</span></span> 

> [!NOTE]
> <span data-ttu-id="c0f7d-126">Nem minden App Service-szolgáltatások érhetők el, ha egy függvény alkalmazás fut a felhasználásra vonatkozó üzemeltetési terv.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-126">Not all App Service features are available when a function app runs on the Consumption hosting plan.</span></span>

<span data-ttu-id="c0f7d-127">Ez a témakör hátralévő része a következő App Service-szolgáltatások az Azure portálon, amelyek hasznosak a funkciók koncentrál:</span><span class="sxs-lookup"><span data-stu-id="c0f7d-127">The rest of this topic focuses on the following App Service features in the Azure portal that are useful for Functions:</span></span>

+ [<span data-ttu-id="c0f7d-128">App Service-szerkesztő</span><span class="sxs-lookup"><span data-stu-id="c0f7d-128">App Service editor</span></span>](#editor)
+ [<span data-ttu-id="c0f7d-129">Alkalmazásbeállítások</span><span class="sxs-lookup"><span data-stu-id="c0f7d-129">Application settings</span></span>](#settings) 
+ [<span data-ttu-id="c0f7d-130">Console</span><span class="sxs-lookup"><span data-stu-id="c0f7d-130">Console</span></span>](#console)
+ [<span data-ttu-id="c0f7d-131">Speciális eszközöket (a Kudu)</span><span class="sxs-lookup"><span data-stu-id="c0f7d-131">Advanced tools (Kudu)</span></span>](#kudu)
+ [<span data-ttu-id="c0f7d-132">Telepítési lehetőségek</span><span class="sxs-lookup"><span data-stu-id="c0f7d-132">Deployment options</span></span>](#deployment)
+ [<span data-ttu-id="c0f7d-133">CORS</span><span class="sxs-lookup"><span data-stu-id="c0f7d-133">CORS</span></span>](#cors)
+ [<span data-ttu-id="c0f7d-134">Hitelesítés</span><span class="sxs-lookup"><span data-stu-id="c0f7d-134">Authentication</span></span>](#auth)
+ [<span data-ttu-id="c0f7d-135">API-definíció</span><span class="sxs-lookup"><span data-stu-id="c0f7d-135">API definition</span></span>](#swagger)

<span data-ttu-id="c0f7d-136">App Service-beállításokkal kapcsolatos további információkért lásd: [Azure App Service beállításainak konfigurálása](../app-service-web/web-sites-configure.md).</span><span class="sxs-lookup"><span data-stu-id="c0f7d-136">For more information about how to work with App Service settings, see [Configure Azure App Service Settings](../app-service-web/web-sites-configure.md).</span></span>

### <span data-ttu-id="c0f7d-137"><a name="editor"></a>App Service-szerkesztő</span><span class="sxs-lookup"><span data-stu-id="c0f7d-137"><a name="editor"></a>App Service Editor</span></span>

| | |
|-|-|
| ![Függvény alkalmazás az App Service-szerkesztőt.](./media/functions-how-to-use-azure-function-app-settings/function-app-appsvc-editor.png)  | <span data-ttu-id="c0f7d-139">Az App Service-szerkesztő egy speciális-portal szerkesztő JSON-konfigurációs fájlok és kód fájlok egyaránt használható.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-139">The App Service editor is an advanced in-portal editor that you can use to modify JSON configuration files and code files alike.</span></span> <span data-ttu-id="c0f7d-140">Ez a beállítás egy külön böngészőlapon alapvető szerkesztővel indít.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-140">Choosing this option launches a separate browser tab with a basic editor.</span></span> <span data-ttu-id="c0f7d-141">Ez lehetővé teszi a Git-tárház integrálása, futtatása és kód hibakereséséhez és funkció beállításainak módosításához.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-141">This enables you to integrate with the Git repository, run and debug code, and modify function app settings.</span></span> <span data-ttu-id="c0f7d-142">A szerkesztő az alapértelmezett függvény panelen összehasonlítja a funkciók bővített fejlesztési környezetet biztosít.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-142">This editor provides an enhanced development environment for your functions compared with the default function app blade.</span></span>    |

![Az App Service-szerkesztő](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-appservice-editor.png)

### <span data-ttu-id="c0f7d-144"><a name="settings"></a>Alkalmazásbeállítások</span><span class="sxs-lookup"><span data-stu-id="c0f7d-144"><a name="settings"></a>Application settings</span></span>

| | |
|-|-|
| ![Alkalmazás függvény Alkalmazásbeállítások.](./media/functions-how-to-use-azure-function-app-settings/function-app-application-settings.png) | <span data-ttu-id="c0f7d-146">Az App Service **Alkalmazásbeállítások** panel, ahol konfigurálhatja és kezelheti a keretrendszer-verziók, távoli hibakeresés, Alkalmazásbeállítások és kapcsolati karakterláncok.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-146">The App Service **Application settings** blade is where you configure and manage framework versions, remote debugging, app settings, and connection strings.</span></span> <span data-ttu-id="c0f7d-147">A függvény alkalmazás integrációjához át az egyéb Azure és a harmadik féltől származó szolgáltatással, módosíthatja itt ezeket a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-147">When you integrate your function app with other Azure and third-party services, you can modify those settings here.</span></span> |

![Az Alkalmazásbeállítások konfigurálása](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-settings.png)

### <span data-ttu-id="c0f7d-149"><a name="console"></a>Konzol</span><span class="sxs-lookup"><span data-stu-id="c0f7d-149"><a name="console"></a>Console</span></span>

| | |
|-|-|
| ![Függvény app konzol az Azure-portálon](./media/functions-how-to-use-azure-function-app-settings/function-app-console.png) | <span data-ttu-id="c0f7d-151">A portál konzol az ideális fejlesztői eszközét esetén interakciót folytatni a függvény app a parancssorból szeretné végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-151">The in-portal console is an ideal developer tool when you prefer to interact with your function app from the command line.</span></span> <span data-ttu-id="c0f7d-152">Általános jellegű parancsok közé tartozik a címtár és a fájl létrehozásának és navigációs, valamint parancsfájlok végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-152">Common commands include directory and file creation and navigation, as well as executing batch files and scripts.</span></span> |

![Függvény app konzol](./media/functions-how-to-use-azure-function-app-settings/configure-function-console.png)

### <span data-ttu-id="c0f7d-154"><a name="kudu"></a>Speciális eszközöket (a Kudu)</span><span class="sxs-lookup"><span data-stu-id="c0f7d-154"><a name="kudu"></a>Advanced tools (Kudu)</span></span>

| | |
|-|-|
| ![Függvényalkalmazásnak Kudu az Azure-portálon](./media/functions-how-to-use-azure-function-app-settings/function-app-advanced-tools.png) | <span data-ttu-id="c0f7d-156">A speciális eszközök az App Service (más néven a Kudu) adja meg a függvény alkalmazás Speciális felügyeleti funkcióhoz hozzáférése.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-156">The advanced tools for App Service (also known as Kudu) provide access to advanced administrative features of your function app.</span></span> <span data-ttu-id="c0f7d-157">Kudu kezelheti az Rendszerinformáció, Alkalmazásbeállítások, környezeti változókat, helyhez kiterjesztések, HTTP-fejlécek és kiszolgáló-változók.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-157">From Kudu, you manage system information, app settings, environment variables, site extensions, HTTP headers, and server variables.</span></span> <span data-ttu-id="c0f7d-158">Is **Kudu** az SCM végpont függvény alkalmazás, például tallózással`https://<myfunctionapp>.scm.azurewebsites.net/`</span><span class="sxs-lookup"><span data-stu-id="c0f7d-158">You can also launch **Kudu** by browsing to the SCM endpoint for your function app, like `https://<myfunctionapp>.scm.azurewebsites.net/`</span></span> |

![A Kudu konfigurálása](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-kudu.png)


### <a name="a-namedeploymentdeployment-options"></a><span data-ttu-id="c0f7d-160"><a name="deployment">Telepítési lehetőségek</span><span class="sxs-lookup"><span data-stu-id="c0f7d-160"><a name="deployment">Deployment options</span></span>

| | |
|-|-|
| ![Függvény alkalmazás központi telepítési beállítások az Azure-portálon](./media/functions-how-to-use-azure-function-app-settings/function-app-deployment-source.png) | <span data-ttu-id="c0f7d-162">Funkciók lehetővé teszi, hogy a helyi számítógépen a funkciókódot fejleszthet.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-162">Functions lets you develop your function code on your local machine.</span></span> <span data-ttu-id="c0f7d-163">A helyi függvény alkalmazásprojektet majd feltöltheti az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-163">You can then upload your local function app project to Azure.</span></span> <span data-ttu-id="c0f7d-164">Hagyományos FTP feltöltés mellett a funkciók lehetővé teszi a népszerű folyamatos integrációt megoldások, például a Githubon, VSTS, Dropbox, Bitbucket és mások segítségével függvény alkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-164">In addition to traditional FTP upload, Functions lets you deploy your function app using popular continuous integration solutions, like GitHub, VSTS, Dropbox, Bitbucket, and others.</span></span> <span data-ttu-id="c0f7d-165">További információkért lásd: [folyamatos üzembe helyezés az Azure Functions](functions-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="c0f7d-165">For more information, see [Continuous deployment for Azure Functions](functions-continuous-deployment.md).</span></span> <span data-ttu-id="c0f7d-166">Töltse fel az FTP- vagy helyi Git segítségével manuálisan, le is kell [az üzembe helyezési hitelesítő adatok konfigurálása](functions-continuous-deployment.md#credentials).</span><span class="sxs-lookup"><span data-stu-id="c0f7d-166">To upload manually using FTP or local Git, you also must [configure your deployment credentials](functions-continuous-deployment.md#credentials).</span></span> |


### <span data-ttu-id="c0f7d-167"><a name="cors"></a>A CORS</span><span class="sxs-lookup"><span data-stu-id="c0f7d-167"><a name="cors"></a>CORS</span></span>

| | |
|-|-|
| ![Függvény alkalmazás CORS az Azure-portálon](./media/functions-how-to-use-azure-function-app-settings/function-app-cors.png) | <span data-ttu-id="c0f7d-169">A szolgáltatások rosszindulatú kód végrehajtása érdekében az App Service külső forrásból függvény alkalmazásaira hívások blokkolja.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-169">To prevent malicious code execution in your services, App Service blocks calls to your function apps from external sources.</span></span> <span data-ttu-id="c0f7d-170">Funkciók támogatja az eltérő eredetű erőforrások megosztása (CORS) lehetővé teszi, hogy adja meg egy engedélyezett eredetet, amelyből a funkciók távoli kérelmek fogadhat "engedélyezett".</span><span class="sxs-lookup"><span data-stu-id="c0f7d-170">Functions supports cross-origin resource sharing (CORS) to let you define a "whitelist" of allowed origins from which your functions can accept remote requests.</span></span>  |

![Függvény alkalmazás konfigurálása CORS](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-cors.png)

### <span data-ttu-id="c0f7d-172"><a name="auth"></a>Hitelesítés</span><span class="sxs-lookup"><span data-stu-id="c0f7d-172"><a name="auth"></a>Authentication</span></span>

| | |
|-|-|
| ![Függvény app hitelesítés az Azure-portálon](./media/functions-how-to-use-azure-function-app-settings/function-app-authentication.png) | <span data-ttu-id="c0f7d-174">Ha funkciók egy HTTP-eseményindítóval, megkövetelheti hívások először hitelesítését.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-174">When functions use an HTTP trigger, you can require calls to first be authenticated.</span></span> <span data-ttu-id="c0f7d-175">App Service támogatja az Azure Active Directory-hitelesítés és be kell jelentkezniük a közösségi szolgáltatók, köztük a Facebook, a Microsoft és a Twitter.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-175">App Service supports Azure Active Directory authentication and sign in with social providers, such as Facebook, Microsoft, and Twitter.</span></span> <span data-ttu-id="c0f7d-176">További részletek az adott hitelesítési szolgáltatókat konfigurál: [Azure App Service hitelesítés áttekintése](../app-service/app-service-authentication-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c0f7d-176">For details on configuring specific authentication providers, see [Azure App Service authentication overview](../app-service/app-service-authentication-overview.md).</span></span> |

![Egy függvény alkalmazások hitelesítésének konfigurálása](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-authentication.png)


### <span data-ttu-id="c0f7d-178"><a name="swagger"></a>API-definíció</span><span class="sxs-lookup"><span data-stu-id="c0f7d-178"><a name="swagger"></a>API definition</span></span>

| | |
|-|-|
| ![Függvény app API definition az Azure portálon swagger](./media/functions-how-to-use-azure-function-app-settings/function-app-api-definition.png) | <span data-ttu-id="c0f7d-180">Funkciók támogatja az ügyfelek a HTTP-eseményindítókkal aktivált függvényeket könnyebben felhasználásához Swagger.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-180">Functions supports Swagger to allow clients to more easily consume your HTTP-triggered functions.</span></span> <span data-ttu-id="c0f7d-181">A Swagger API-definíciók létrehozásával kapcsolatos további információkért látogasson el a [Ismerkedés az API Apps, az ASP.NET és az Azure-ban Swagger](../app-service-api/app-service-api-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c0f7d-181">For more information on creating API definitions with Swagger, visit [Get Started with API Apps, ASP.NET, and Swagger in Azure](../app-service-api/app-service-api-dotnet-get-started.md).</span></span> <span data-ttu-id="c0f7d-182">Funkciók proxyk segítségével több függvények egyetlen API felület meghatározása.</span><span class="sxs-lookup"><span data-stu-id="c0f7d-182">You can also use Functions Proxies to define a single API surface for multiple functions.</span></span> <span data-ttu-id="c0f7d-183">További információkért lásd: [használata az Azure Functions proxyk](functions-proxies.md).</span><span class="sxs-lookup"><span data-stu-id="c0f7d-183">For more information, see [Working with Azure Functions Proxies](functions-proxies.md).</span></span> |

![Függvény App API konfigurálása](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-apidef.png)



## <a name="next-steps"></a><span data-ttu-id="c0f7d-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c0f7d-185">Next steps</span></span>

+ [<span data-ttu-id="c0f7d-186">Az Azure App Service-beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c0f7d-186">Configure Azure App Service Settings</span></span>](../app-service-web/web-sites-configure.md)
+ [<span data-ttu-id="c0f7d-187">Azure Functions – folyamatos üzembe helyezés</span><span class="sxs-lookup"><span data-stu-id="c0f7d-187">Continuous deployment for Azure Functions</span></span>](functions-continuous-deployment.md)



