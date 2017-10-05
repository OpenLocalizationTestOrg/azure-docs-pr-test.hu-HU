---
title: "A CDN hozzáadása az Azure App Service |} Microsoft Docs"
description: "Ha hozzáad egy tartalomkézbesítési hálózatot (CDN-t) egy Azure App Service szolgáltatáshoz, a statikus fájlok gyorsítótárazását és továbbítását világszerte az ügyfeleihez közeli kiszolgálókról végezheti."
services: app-service\web
author: syntaxc4
ms.author: cfowler
ms.date: 05/31/2017
ms.topic: tutorial
ms.service: app-service-web
manager: erikre
ms.workload: web
ms.custom: mvc
ms.openlocfilehash: 257b75d01f3904661c1a188a2d53ffcb74f48f06
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="add-a-content-delivery-network-cdn-to-an-azure-app-service"></a><span data-ttu-id="b01b8-103">Content Delivery Network (CDN) hozzáadása Azure App Service platformon</span><span class="sxs-lookup"><span data-stu-id="b01b8-103">Add a Content Delivery Network (CDN) to an Azure App Service</span></span>

<span data-ttu-id="b01b8-104">Az [Azure Content Delivery Network (CDN)](../cdn/cdn-overview.md) a statikus webtartalmakat stratégiailag kiválasztott helyeken gyorsítótárazza, így maximális átviteli sebességgel tudja kézbesíteni a tartalmakat a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="b01b8-104">[Azure Content Delivery Network (CDN)](../cdn/cdn-overview.md) caches static web content at strategically placed locations to provide maximum throughput for delivering content to users.</span></span> <span data-ttu-id="b01b8-105">A CDN ezzel együtt csökkenti a kiszolgálók terhelését a webappban.</span><span class="sxs-lookup"><span data-stu-id="b01b8-105">The CDN also decreases server load on your web app.</span></span> <span data-ttu-id="b01b8-106">Ez az oktatóanyag bemutatja, hogyan adható hozzá az Azure CDN [a webappokhoz az Azure App Service szolgáltatásban](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b01b8-106">This tutorial shows how to add Azure CDN to a [web app in Azure App Service](app-service-web-overview.md).</span></span> 

<span data-ttu-id="b01b8-107">Íme a mintaként szolgáló statikus HTML-webhely kezdőlapja, amelyet használni fog:</span><span class="sxs-lookup"><span data-stu-id="b01b8-107">Here's the home page of the sample static HTML site that you'll work with:</span></span>

![Mintaalkalmazás kezdőlapja](media/app-service-web-tutorial-content-delivery-network/sample-app-home-page.png)

<span data-ttu-id="b01b8-109">Ismertetett témák:</span><span class="sxs-lookup"><span data-stu-id="b01b8-109">What you'll learn:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b01b8-110">CDN-végpont létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b01b8-110">Create a CDN endpoint.</span></span>
> * <span data-ttu-id="b01b8-111">Gyorsítótárazott objektumok frissítése.</span><span class="sxs-lookup"><span data-stu-id="b01b8-111">Refresh cached assets.</span></span>
> * <span data-ttu-id="b01b8-112">Gyorsítótárazott verziók felügyelete lekérdezési karakterláncok használatával.</span><span class="sxs-lookup"><span data-stu-id="b01b8-112">Use query strings to control cached versions.</span></span>
> * <span data-ttu-id="b01b8-113">Egyéni tartomány használata a CDN-végponthoz.</span><span class="sxs-lookup"><span data-stu-id="b01b8-113">Use a custom domain for the CDN endpoint.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b01b8-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b01b8-114">Prerequisites</span></span>

<span data-ttu-id="b01b8-115">Az oktatóanyag elvégzéséhez:</span><span class="sxs-lookup"><span data-stu-id="b01b8-115">To complete this tutorial:</span></span>

- [<span data-ttu-id="b01b8-116">A Git telepítése</span><span class="sxs-lookup"><span data-stu-id="b01b8-116">Install Git</span></span>](https://git-scm.com/)
- [<span data-ttu-id="b01b8-117">Az Azure CLI 2.0 telepítése</span><span class="sxs-lookup"><span data-stu-id="b01b8-117">Install Azure CLI 2.0</span></span>](https://docs.microsoft.com/cli/azure/install-azure-cli)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-web-app"></a><span data-ttu-id="b01b8-118">A webapp létrehozása</span><span class="sxs-lookup"><span data-stu-id="b01b8-118">Create the web app</span></span>

<span data-ttu-id="b01b8-119">A web app, akivel együtt dolgozik lesz létrehozásához kövesse a [statikus HTML gyors üzembe helyezés](app-service-web-get-started-html.md) keresztül a **keresse meg az alkalmazás** lépés.</span><span class="sxs-lookup"><span data-stu-id="b01b8-119">To create the web app that you'll work with, follow the [static HTML quickstart](app-service-web-get-started-html.md) through the **Browse to the app** step.</span></span>

### <a name="have-a-custom-domain-ready"></a><span data-ttu-id="b01b8-120">Rendelkezésre álló egyéni tartománynév</span><span class="sxs-lookup"><span data-stu-id="b01b8-120">Have a custom domain ready</span></span>

<span data-ttu-id="b01b8-121">Az egyéni tartomány lépés az oktatóanyag elvégzéséhez szüksége saját egyéni tartományt, és a tartomány szolgáltató (például a GoDaddy) a DNS-beállításjegyzék rendelkezik hozzáféréssel.</span><span class="sxs-lookup"><span data-stu-id="b01b8-121">To complete the custom domain step of this tutorial, you need to own a custom domain and have access to your DNS registry for your domain provider (such as GoDaddy).</span></span> <span data-ttu-id="b01b8-122">Például a `contoso.com` és a `www.contoso.com` DNS-bejegyzéseinek hozzáadásához hozzá kell férnie a `contoso.com` gyökértartomány DNS-beállításainak konfigurációjához.</span><span class="sxs-lookup"><span data-stu-id="b01b8-122">For example, to add DNS entries for `contoso.com` and `www.contoso.com`, you must have access to configure the DNS settings for the `contoso.com` root domain.</span></span>

<span data-ttu-id="b01b8-123">Ha még nem rendelkezik tartománynévvel, az [App Service-tartományokkal foglalkozó oktatóanyagban](custom-dns-web-site-buydomains-web-app.md) foglaltak szerint vásárolhat egy tartományt az Azure Portal használatával.</span><span class="sxs-lookup"><span data-stu-id="b01b8-123">If you don't already have a domain name, consider following the [App Service domain tutorial](custom-dns-web-site-buydomains-web-app.md) to purchase a domain using the Azure portal.</span></span> 

## <a name="log-in-to-the-azure-portal"></a><span data-ttu-id="b01b8-124">Bejelentkezés az Azure Portalra</span><span class="sxs-lookup"><span data-stu-id="b01b8-124">Log in to the Azure portal</span></span>

<span data-ttu-id="b01b8-125">Nyisson meg egy böngészőt, és keresse fel az [Azure Portalt](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b01b8-125">Open a browser and navigate to the [Azure portal](https://portal.azure.com).</span></span>

## <a name="create-a-cdn-profile-and-endpoint"></a><span data-ttu-id="b01b8-126">CDN-profil és -végpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="b01b8-126">Create a CDN profile and endpoint</span></span>

<span data-ttu-id="b01b8-127">A bal oldali navigációs felületen válassza az **App Services** lehetőséget, majd válassza ki a [statikus HTML gyorsútmutató](app-service-web-get-started-html.md) segítségével létrehozott alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b01b8-127">In the left navigation, select **App Services**, and then select the app that you created in the [static HTML quickstart](app-service-web-get-started-html.md).</span></span>

![Az App Service alkalmazás kiválasztása a portálon](media/app-service-web-tutorial-content-delivery-network/portal-select-app-services.png)

<span data-ttu-id="b01b8-129">Az **App Service** lap **Beállítások** területén válassza a **Hálózatkezelés > Az Azure CDN konfigurálása az alkalmazáshoz** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="b01b8-129">In the **App Service** page, in the **Settings** section, select **Networking > Configure Azure CDN for your app**.</span></span>

![A CDN kiválasztása a portálon](media/app-service-web-tutorial-content-delivery-network/portal-select-cdn.png)

<span data-ttu-id="b01b8-131">Az **Azure Content Delivery Network** lapon adja meg az **Új végpont** beállításait az alábbi táblának megfelelően.</span><span class="sxs-lookup"><span data-stu-id="b01b8-131">In the **Azure Content Delivery Network** page, provide the **New endpoint** settings as specified in the table.</span></span>

![Profil és végpont létrehozása a portálon](media/app-service-web-tutorial-content-delivery-network/portal-new-endpoint.png)

| <span data-ttu-id="b01b8-133">Beállítás</span><span class="sxs-lookup"><span data-stu-id="b01b8-133">Setting</span></span> | <span data-ttu-id="b01b8-134">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="b01b8-134">Suggested value</span></span> | <span data-ttu-id="b01b8-135">Leírás</span><span class="sxs-lookup"><span data-stu-id="b01b8-135">Description</span></span> |
| ------- | --------------- | ----------- |
| <span data-ttu-id="b01b8-136">**CDN-profil**</span><span class="sxs-lookup"><span data-stu-id="b01b8-136">**CDN profile**</span></span> | <span data-ttu-id="b01b8-137">myCDNProfile</span><span class="sxs-lookup"><span data-stu-id="b01b8-137">myCDNProfile</span></span> | <span data-ttu-id="b01b8-138">Válassza ki **hozzon létre új** a CDN-profil létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="b01b8-138">Select **Create new** to create a CDN profile.</span></span> <span data-ttu-id="b01b8-139">A CDN-profil ugyanabba a tarifacsomagba tartozó CDN-végpontok gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="b01b8-139">A CDN profile is a collection of CDN endpoints with the same pricing tier.</span></span> |
| <span data-ttu-id="b01b8-140">**Tarifacsomag**</span><span class="sxs-lookup"><span data-stu-id="b01b8-140">**Pricing tier**</span></span> | <span data-ttu-id="b01b8-141">Akamai Standard</span><span class="sxs-lookup"><span data-stu-id="b01b8-141">Standard Akamai</span></span> | <span data-ttu-id="b01b8-142">A [tarifacsomag](../cdn/cdn-overview.md#azure-cdn-features) határozza meg a szolgáltatót és az elérhető szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="b01b8-142">The [pricing tier](../cdn/cdn-overview.md#azure-cdn-features) specifies the provider and available features.</span></span> <span data-ttu-id="b01b8-143">Ebben az oktatóanyagban Standard Akamai használunk.</span><span class="sxs-lookup"><span data-stu-id="b01b8-143">In this tutorial, we are using Standard Akamai.</span></span> |
| <span data-ttu-id="b01b8-144">**CDN-végpont neve**</span><span class="sxs-lookup"><span data-stu-id="b01b8-144">**CDN endpoint name**</span></span> | <span data-ttu-id="b01b8-145">Bármely egyedi név az azureedge.net tartományban</span><span class="sxs-lookup"><span data-stu-id="b01b8-145">Any name that is unique in the azureedge.net domain</span></span> | <span data-ttu-id="b01b8-146">A gyorsítótárazott erőforrások a *\<végpont_neve>.azureedge.net* tartományban érhetőek el.</span><span class="sxs-lookup"><span data-stu-id="b01b8-146">You access your cached resources at the domain *\<endpointname>.azureedge.net*.</span></span>

<span data-ttu-id="b01b8-147">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="b01b8-147">Select **Create**.</span></span>

<span data-ttu-id="b01b8-148">Az Azure létrehozza a profilt és a végpontot.</span><span class="sxs-lookup"><span data-stu-id="b01b8-148">Azure creates the profile and endpoint.</span></span> <span data-ttu-id="b01b8-149">Az új végpont megjelenik ugyanezen a lapon a **Végpontok** listában, és a kiosztása után **Fut** állapotra vált.</span><span class="sxs-lookup"><span data-stu-id="b01b8-149">The new endpoint appears in the **Endpoints** list on the same page, and when it's provisioned the status is **Running**.</span></span>

![Új végpont a listában](media/app-service-web-tutorial-content-delivery-network/portal-new-endpoint-in-list.png)

### <a name="test-the-cdn-endpoint"></a><span data-ttu-id="b01b8-151">A CDN-végpont tesztelése</span><span class="sxs-lookup"><span data-stu-id="b01b8-151">Test the CDN endpoint</span></span>

<span data-ttu-id="b01b8-152">A Verizon tarifacsomag kiválasztása esetén a végpontok propagálása körülbelül 90 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="b01b8-152">If you selected Verizon pricing tier, it typically takes about 90 minutes for endpoint propagation.</span></span> <span data-ttu-id="b01b8-153">Az Akamai esetében a terjesztés csak néhány percig tart</span><span class="sxs-lookup"><span data-stu-id="b01b8-153">For Akamai, it takes a couple minutes for propagation</span></span>

<span data-ttu-id="b01b8-154">Ugyanehhez az alkalmazáshoz tartozik egy `index.html` fájl, valamint *css*, *img* és *js* mappák is, amelyek egyéb statikus objektumokat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="b01b8-154">The sample app has an `index.html` file and *css*, *img*, and *js* folders that contain other static assets.</span></span> <span data-ttu-id="b01b8-155">Az összes fájl tartalmának elérési útjai megegyezik a CDN-végponton.</span><span class="sxs-lookup"><span data-stu-id="b01b8-155">The content paths for all of these files are the same at the CDN endpoint.</span></span> <span data-ttu-id="b01b8-156">Például a következő két URL egyaránt a *bootstrap.css* fájlra mutat a *css* mappában:</span><span class="sxs-lookup"><span data-stu-id="b01b8-156">For example, both of the following URLs access the *bootstrap.css* file in the *css* folder:</span></span>

```
http://<appname>.azurewebsites.net/css/bootstrap.css
```

```
http://<endpointname>.azureedge.net/css/bootstrap.css
```

<span data-ttu-id="b01b8-157">Nyissa meg egy böngészőt, és a következő URL-címe:</span><span class="sxs-lookup"><span data-stu-id="b01b8-157">Navigate a browser to the following URL:</span></span>

```
http://<endpointname>.azureedge.net/index.html
```

![A mintaalkalmazás CDN által szolgáltatott kezdőlapja](media/app-service-web-tutorial-content-delivery-network/sample-app-home-page-cdn.png)

 <span data-ttu-id="b01b8-159">Azure-webalkalmazás a korábban futtatott ugyanazon az oldalon láthatja.</span><span class="sxs-lookup"><span data-stu-id="b01b8-159">You see the same page that you ran earlier in an Azure web app.</span></span> <span data-ttu-id="b01b8-160">Az Azure CDN fogadta a származási web app eszközök, és van szolgál a a CDN-végpont</span><span class="sxs-lookup"><span data-stu-id="b01b8-160">Azure CDN has retrieved the origin web app's assets and is serving them from the CDN endpoint</span></span>

<span data-ttu-id="b01b8-161">Annak érdekében, hogy a CDN gyorsítótárazza a lapot, frissítse azt.</span><span class="sxs-lookup"><span data-stu-id="b01b8-161">To ensure that this page is cached in the CDN, refresh the page.</span></span> <span data-ttu-id="b01b8-162">Néha két kérés is szükséges egyazon objektumra vonatkozóan, hogy a CDN gyorsítótárazza a kért tartalmat.</span><span class="sxs-lookup"><span data-stu-id="b01b8-162">Two requests for the same asset are sometimes required for the CDN to cache the requested content.</span></span>

<span data-ttu-id="b01b8-163">Az Azure CDN-profilok és -végpontok létrehozásával kapcsolatos további információkért lásd: [Az Azure CDN használatának első lépései](../cdn/cdn-create-new-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="b01b8-163">For more information about creating Azure CDN profiles and endpoints, see [Getting started with Azure CDN](../cdn/cdn-create-new-endpoint.md).</span></span>

## <a name="purge-the-cdn"></a><span data-ttu-id="b01b8-164">A CDN végleges törlése</span><span class="sxs-lookup"><span data-stu-id="b01b8-164">Purge the CDN</span></span>

<span data-ttu-id="b01b8-165">A CDN rendszeres időközönként frissíti az erőforrásait a forrásként szolgáló webappból az élettartam (TTL) konfigurációja alapján.</span><span class="sxs-lookup"><span data-stu-id="b01b8-165">The CDN periodically refreshes its resources from the origin web app based on the time-to-live (TTL) configuration.</span></span> <span data-ttu-id="b01b8-166">Az alapértelmezett élettartam érték hét nap.</span><span class="sxs-lookup"><span data-stu-id="b01b8-166">The default TTL is seven days.</span></span>

<span data-ttu-id="b01b8-167">Esetenként az élettartam lejárta előtt is szükséges lehet a CDN frissítése – például amikor frissített tartalmat telepít a webappba.</span><span class="sxs-lookup"><span data-stu-id="b01b8-167">At times you might need to refresh the CDN before the TTL expiration -- for example, when you deploy updated content to the web app.</span></span> <span data-ttu-id="b01b8-168">A frissítés indításához manuálisan törölheti a CDN-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="b01b8-168">To trigger a refresh, you can manually purge the CDN resources.</span></span> 

<span data-ttu-id="b01b8-169">Az oktatóanyag jelen szakaszában egy módosítást fog telepíteni a webappba, és törli a CDN-t, hogy az frissítse a gyorsítótárát.</span><span class="sxs-lookup"><span data-stu-id="b01b8-169">In this section of the tutorial, you deploy a change to the web app and purge the CDN to trigger the CDN to refresh its cache.</span></span>

### <a name="deploy-a-change-to-the-web-app"></a><span data-ttu-id="b01b8-170">Módosítás telepítése a webappba</span><span class="sxs-lookup"><span data-stu-id="b01b8-170">Deploy a change to the web app</span></span>

<span data-ttu-id="b01b8-171">Nyissa meg az `index.html` fájlt, és adja hozzá a „- V2” utótagot a H1 fejléchez, ahogy az az alábbi példában látható:</span><span class="sxs-lookup"><span data-stu-id="b01b8-171">Open the `index.html` file and add "- V2" to the H1 heading, as shown in the following example:</span></span> 

```
<h1>Azure App Service - Sample Static HTML Site - V2</h1>
```

<span data-ttu-id="b01b8-172">Véglegesítse a módosítást, és telepítse a webappba.</span><span class="sxs-lookup"><span data-stu-id="b01b8-172">Commit your change and deploy it to the web app.</span></span>

```bash
git commit -am "version 2"
git push azure master
```

<span data-ttu-id="b01b8-173">Miután befejeződött a telepítés, a böngészőben nyissa meg a webapp URL-címét, és láthatja a módosított tartalmat.</span><span class="sxs-lookup"><span data-stu-id="b01b8-173">Once deployment has completed, browse to the web app URL and you see the change.</span></span>

```
http://<appname>.azurewebsites.net/index.html
```

![V2 a webapp címében](media/app-service-web-tutorial-content-delivery-network/v2-in-web-app-title.png)

<span data-ttu-id="b01b8-175">Ha a böngészőben megnyitja a kezdőlap CDN-végponti URL-címét, láthatja, hogy a módosítás még nem jelent meg, mivel a CDN-ben gyorsítótárazott verzió még nem járt le.</span><span class="sxs-lookup"><span data-stu-id="b01b8-175">Browse to the CDN endpoint URL for the home page and you don't see the change because the cached version in the CDN hasn't expired yet.</span></span> 

```
http://<endpointname>.azureedge.net/index.html
```

![A V2 nem jelenik meg a CDN-beli címben](media/app-service-web-tutorial-content-delivery-network/no-v2-in-cdn-title.png)

### <a name="purge-the-cdn-in-the-portal"></a><span data-ttu-id="b01b8-177">A CDN törlése a portálon</span><span class="sxs-lookup"><span data-stu-id="b01b8-177">Purge the CDN in the portal</span></span>

<span data-ttu-id="b01b8-178">Ahhoz, hogy a CDN frissítse a gyorsítótárazott verziót, törölje a CDN-t.</span><span class="sxs-lookup"><span data-stu-id="b01b8-178">To trigger the CDN to update its cached version, purge the CDN.</span></span>

<span data-ttu-id="b01b8-179">A portál bal oldali navigációs felületén válassza az **Erőforráscsoportok** elemet, majd válassza ki a webapphoz létrehozott erőforráscsoportot (myResourceGroup).</span><span class="sxs-lookup"><span data-stu-id="b01b8-179">In the portal left navigation, select **Resource groups**, and then select the resource group that you created for your web app (myResourceGroup).</span></span>

![Erőforráscsoport kiválasztása](media/app-service-web-tutorial-content-delivery-network/portal-select-group.png)

<span data-ttu-id="b01b8-181">Az erőforrások listájában válassza ki a CDN-végpontot.</span><span class="sxs-lookup"><span data-stu-id="b01b8-181">In the list of resources, select your CDN endpoint.</span></span>

![Végpont kiválasztása](media/app-service-web-tutorial-content-delivery-network/portal-select-endpoint.png)

<span data-ttu-id="b01b8-183">A **Végpont** lap tetején kattintson a **Végleges törlés** gombra.</span><span class="sxs-lookup"><span data-stu-id="b01b8-183">At the top of the **Endpoint** page, click **Purge**.</span></span>

![Végleges törlés kiválasztva](media/app-service-web-tutorial-content-delivery-network/portal-select-purge.png)

<span data-ttu-id="b01b8-185">Adja meg a törölni kívánt tartalmak elérési útjait.</span><span class="sxs-lookup"><span data-stu-id="b01b8-185">Enter the content paths you wish to purge.</span></span> <span data-ttu-id="b01b8-186">Megadhat egy teljes elérési útvonalat egy adott fájl törléséhez, vagy egy részleges útvonalat egy adott mappa teljes tartalmának törléséhez és frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="b01b8-186">You can pass a complete file path to purge an individual file, or a path segment to purge and refresh all content in a folder.</span></span> <span data-ttu-id="b01b8-187">Mivel az `index.html` fájlt módosította, mindenképp ez legyen az egyik útvonal.</span><span class="sxs-lookup"><span data-stu-id="b01b8-187">Since you changed `index.html`, make sure that is one of the paths.</span></span>

<span data-ttu-id="b01b8-188">A lap alján kattintson a **Végleges törlés** gombra.</span><span class="sxs-lookup"><span data-stu-id="b01b8-188">At the bottom of the page, select **Purge**.</span></span>

![Oldal végleges törlése](media/app-service-web-tutorial-content-delivery-network/app-service-web-purge-cdn.png)

### <a name="verify-that-the-cdn-is-updated"></a><span data-ttu-id="b01b8-190">A CDN frissítésének ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="b01b8-190">Verify that the CDN is updated</span></span>

<span data-ttu-id="b01b8-191">Várjon, amíg a végleges törlési kérés feldolgozása befejeződik. Ez általában néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="b01b8-191">Wait until the purge request finishes processing, typically a couple of minutes.</span></span> <span data-ttu-id="b01b8-192">Az aktuális állapot megtekintéséhez válassza a harang ikont a lap tetején.</span><span class="sxs-lookup"><span data-stu-id="b01b8-192">To see the current status, select the bell icon at the top of the page.</span></span> 

![Törlési értesítés](media/app-service-web-tutorial-content-delivery-network/portal-purge-notification.png)

<span data-ttu-id="b01b8-194">Böngészőben nyissa meg az `index.html` CDN-végponti URL-címét, és láthatja, hogy a kezdőlap címéhez hozzáadott V2 megjelent.</span><span class="sxs-lookup"><span data-stu-id="b01b8-194">Browse to the CDN endpoint URL for `index.html`, and now you see the V2 that you added to the title on the home page.</span></span> <span data-ttu-id="b01b8-195">Ez jelzi, hogy a CDN gyorsítótára frissítve lett.</span><span class="sxs-lookup"><span data-stu-id="b01b8-195">This shows that the CDN cache has been refreshed.</span></span>

```
http://<endpointname>.azureedge.net/index.html
```

![V2 a CDN-beli címben](media/app-service-web-tutorial-content-delivery-network/v2-in-cdn-title.png)

<span data-ttu-id="b01b8-197">További információkért lásd az [Azure CDN-végpontok végleges törléséről](../cdn/cdn-purge-endpoint.md) szóló cikket.</span><span class="sxs-lookup"><span data-stu-id="b01b8-197">For more information, see [Purge an Azure CDN endpoint](../cdn/cdn-purge-endpoint.md).</span></span> 

## <a name="use-query-strings-to-version-content"></a><span data-ttu-id="b01b8-198">Tartalmak verziószámozása lekérdezési karakterláncok használatával</span><span class="sxs-lookup"><span data-stu-id="b01b8-198">Use query strings to version content</span></span>

<span data-ttu-id="b01b8-199">Az Azure CDN az alábbi gyorsítótárazási lehetőségeket kínálja:</span><span class="sxs-lookup"><span data-stu-id="b01b8-199">The Azure CDN offers the following caching behavior options:</span></span>

* <span data-ttu-id="b01b8-200">Lekérdezési karakterláncok figyelmen kívül hagyása</span><span class="sxs-lookup"><span data-stu-id="b01b8-200">Ignore query strings</span></span>
* <span data-ttu-id="b01b8-201">Lekérdezési karakterláncok gyorsítótárazásának megkerülése</span><span class="sxs-lookup"><span data-stu-id="b01b8-201">Bypass caching for query strings</span></span>
* <span data-ttu-id="b01b8-202">Minden egyedi URL gyorsítótárazása</span><span class="sxs-lookup"><span data-stu-id="b01b8-202">Cache every unique URL</span></span> 

<span data-ttu-id="b01b8-203">Az első érték az alapértelmezett, ami azt jelenti, hogy egy eszköz, függetlenül a lekérdezési karakterlánc az URL-cím csak egy gyorsítótárazott verzió.</span><span class="sxs-lookup"><span data-stu-id="b01b8-203">The first of these is the default, which means there is only one cached version of an asset regardless of the query string in the URL.</span></span> 

<span data-ttu-id="b01b8-204">Az oktatóanyag ezen szakaszában a gyorsítótárazás működésének módosításával minden egyedi URL-címet gyorsítótárazni fog.</span><span class="sxs-lookup"><span data-stu-id="b01b8-204">In this section of the tutorial, you change the caching behavior to cache every unique URL.</span></span>

### <a name="change-the-cache-behavior"></a><span data-ttu-id="b01b8-205">A gyorsítótárazás működésének módosítása</span><span class="sxs-lookup"><span data-stu-id="b01b8-205">Change the cache behavior</span></span>

<span data-ttu-id="b01b8-206">Az Azure Portal **CDN-végpont** lapján válassza a **Gyorsítótár** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="b01b8-206">In the Azure portal **CDN Endpoint** page, select **Cache**.</span></span>

<span data-ttu-id="b01b8-207">Válassza a **Minden egyedi URL-cím gyorsítótárazása** lehetőséget a **Lekérdezési karakterláncok gyorsítótárazásának működése** legördülő menüben.</span><span class="sxs-lookup"><span data-stu-id="b01b8-207">Select **Cache every unique URL** from the **Query string caching behavior** drop-down list.</span></span>

<span data-ttu-id="b01b8-208">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="b01b8-208">Select **Save**.</span></span>

![Lekérdezési karakterláncok gyorsítótárazási működésének kiválasztása](media/app-service-web-tutorial-content-delivery-network/portal-select-caching-behavior.png)

### <a name="verify-that-unique-urls-are-cached-separately"></a><span data-ttu-id="b01b8-210">Az egyedi URL-címek külön gyorsítótárazásának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="b01b8-210">Verify that unique URLs are cached separately</span></span>

<span data-ttu-id="b01b8-211">Egy böngészőben nyissa meg a kezdőlapot a CDN-végponton, de használjon egy lekérdezési karakterláncot is:</span><span class="sxs-lookup"><span data-stu-id="b01b8-211">In a browser, navigate to the home page at the CDN endpoint, but include a query string:</span></span> 

```
http://<endpointname>.azureedge.net/index.html?q=1
```

<span data-ttu-id="b01b8-212">A CDN a webapp aktuális tartalmát adja vissza, aminek a fejlécében szerepel a „V2” utótag.</span><span class="sxs-lookup"><span data-stu-id="b01b8-212">The CDN returns the current web app content, which includes "V2" in the heading.</span></span> 

<span data-ttu-id="b01b8-213">Annak érdekében, hogy a CDN gyorsítótárazza a lapot, frissítse azt.</span><span class="sxs-lookup"><span data-stu-id="b01b8-213">To ensure that this page is cached in the CDN, refresh the page.</span></span> 

<span data-ttu-id="b01b8-214">Nyissa meg az `index.html` fájlt, módosítsa a „V2”-t „V3”-ra, majd telepítse a módosítást.</span><span class="sxs-lookup"><span data-stu-id="b01b8-214">Open `index.html` and change "V2" to "V3", and deploy the change.</span></span> 

```bash
git commit -am "version 3"
git push azure master
```

<span data-ttu-id="b01b8-215">Böngészőben nyissa meg a CDN-végponti URL-címet egy új lekérdezési karakterlánccal, például a következővel: `q=2`.</span><span class="sxs-lookup"><span data-stu-id="b01b8-215">In a browser, go to the CDN endpoint URL with a new query string such as `q=2`.</span></span> <span data-ttu-id="b01b8-216">A CDN lekéri az aktuális `index.html` fájlt, és megjelenik a „V3” utótag.</span><span class="sxs-lookup"><span data-stu-id="b01b8-216">The CDN gets the current `index.html` file and displays "V3".</span></span>  <span data-ttu-id="b01b8-217">Ha azonban a `q=1` lekérdezési karakterlánccal nyitja meg a CDN-végpontot, a „V2” utótag látható.</span><span class="sxs-lookup"><span data-stu-id="b01b8-217">But if you navigate to the CDN endpoint with the `q=1` query string, you see "V2".</span></span>

```
http://<endpointname>.azureedge.net/index.html?q=2
```

![V3 a CDN-beli címben, 2. lekérdezési karakterlánc](media/app-service-web-tutorial-content-delivery-network/v3-in-cdn-title-qs2.png)

```
http://<endpointname>.azureedge.net/index.html?q=1
```

![V2 a CDN-beli címben, 1. lekérdezési karakterlánc](media/app-service-web-tutorial-content-delivery-network/v2-in-cdn-title-qs1.png)

<span data-ttu-id="b01b8-220">A kimeneti jeleníti meg, hogy minden egyes lekérdezési karakterlánc eltérően kell-e kezelni:</span><span class="sxs-lookup"><span data-stu-id="b01b8-220">This output shows that each query string is treated differently:</span></span>

* <span data-ttu-id="b01b8-221">q = 1 használt előtt, így a gyorsítótárazott tartalom (V2) ad vissza.</span><span class="sxs-lookup"><span data-stu-id="b01b8-221">q=1 was used before, so cached contents are returned (V2).</span></span>
* <span data-ttu-id="b01b8-222">q = 2 egy új, ezért a legújabb web app tartalom letöltésének és (V3) adott vissza.</span><span class="sxs-lookup"><span data-stu-id="b01b8-222">q=2 is new, so the latest web app contents are retrieved and returned (V3).</span></span>

<span data-ttu-id="b01b8-223">További információkért lásd: [Az Azure CDN gyorsítótárazási viselkedésének vezérlése lekérdezési karakterláncokkal](../cdn/cdn-query-string.md).</span><span class="sxs-lookup"><span data-stu-id="b01b8-223">For more information, see [Control Azure CDN caching behavior with query strings](../cdn/cdn-query-string.md).</span></span>

## <a name="map-a-custom-domain-to-a-cdn-endpoint"></a><span data-ttu-id="b01b8-224">Egyéni tartomány leképezése egy CDN-végpontra</span><span class="sxs-lookup"><span data-stu-id="b01b8-224">Map a custom domain to a CDN endpoint</span></span>

<span data-ttu-id="b01b8-225">Az egyéni tartományt a CDN-végpontra egy CNAME-rekord létrehozásával képezheti le.</span><span class="sxs-lookup"><span data-stu-id="b01b8-225">You'll map your custom domain to your CDN Endpoint by creating a CNAME record.</span></span> <span data-ttu-id="b01b8-226">A CNAME-rekord egy DNS-szolgáltatás, amellyel egy forrástartomány leképezhető egy céltartományra.</span><span class="sxs-lookup"><span data-stu-id="b01b8-226">A CNAME record is a DNS feature that maps a source domain to a destination domain.</span></span> <span data-ttu-id="b01b8-227">Például leképezheti a `cdn.contoso.com` vagy a `static.contoso.com` tartományt a `contoso.azureedge.net` tartományra.</span><span class="sxs-lookup"><span data-stu-id="b01b8-227">For example, you might map `cdn.contoso.com` or `static.contoso.com` to `contoso.azureedge.net`.</span></span>

<span data-ttu-id="b01b8-228">Ha nem rendelkezik egyéni tartománnyal, az [App Service-tartományokkal foglalkozó oktatóanyagban](custom-dns-web-site-buydomains-web-app.md) foglaltak szerint vásárolhat egyet az Azure Portal használatával.</span><span class="sxs-lookup"><span data-stu-id="b01b8-228">If you don't have a custom domain, consider following the [App Service domain tutorial](custom-dns-web-site-buydomains-web-app.md) to purchase a domain using the Azure portal.</span></span> 

### <a name="find-the-hostname-to-use-with-the-cname"></a><span data-ttu-id="b01b8-229">A CNAME-rekordhoz használandó gazdagépnév keresése</span><span class="sxs-lookup"><span data-stu-id="b01b8-229">Find the hostname to use with the CNAME</span></span>

<span data-ttu-id="b01b8-230">Az Azure Portal **Végpont** lapján ellenőrizze, hogy a bal oldali navigációs felületen az **Áttekintés** lehetőség van kiválasztva, majd válassza a **+ Egyéni tartomány** gombot a lap tetején.</span><span class="sxs-lookup"><span data-stu-id="b01b8-230">In the Azure portal **Endpoint** page, make sure **Overview** is selected in the left navigation, and then select the **+ Custom Domain** button at the top of the page.</span></span>

![Egyéni tartomány hozzáadása kiválasztva](media/app-service-web-tutorial-content-delivery-network/portal-select-add-domain.png)

<span data-ttu-id="b01b8-232">Az **Egyéni tartomány hozzáadása** lapon látható a CNAME rekord létrehozásához használandó végpont gazdagépneve.</span><span class="sxs-lookup"><span data-stu-id="b01b8-232">In the **Add a custom domain** page, you see the endpoint host name to use in creating a CNAME record.</span></span> <span data-ttu-id="b01b8-233">A gazdagépnév CDN-végponti URL-címből van származtatva: **&lt;VégpontNeve>.azureedge.net**.</span><span class="sxs-lookup"><span data-stu-id="b01b8-233">The host name is derived from your CDN endpoint URL: **&lt;EndpointName>.azureedge.net**.</span></span> 

![Tartomány hozzáadása lap](media/app-service-web-tutorial-content-delivery-network/portal-add-domain.png)

### <a name="configure-the-cname-with-your-domain-registrar"></a><span data-ttu-id="b01b8-235">A CNAME konfigurálása a tartományregisztrálóval</span><span class="sxs-lookup"><span data-stu-id="b01b8-235">Configure the CNAME with your domain registrar</span></span>

<span data-ttu-id="b01b8-236">Lépjen a tartományregisztráló webhelyére, és keresse meg a DNS-rekordok létrehozására szolgáló felületet.</span><span class="sxs-lookup"><span data-stu-id="b01b8-236">Navigate to your domain registrar's web site, and locate the section for creating DNS records.</span></span> <span data-ttu-id="b01b8-237">Ezt a **Tartománynév**, **DNS**, **Névkiszolgáló kezelése** vagy hasonló területen találja.</span><span class="sxs-lookup"><span data-stu-id="b01b8-237">You might find this in a section such as **Domain Name**, **DNS**, or **Name Server Management**.</span></span>

<span data-ttu-id="b01b8-238">Keresse meg a CNAME-rekordok kezelésére szolgáló felületet.</span><span class="sxs-lookup"><span data-stu-id="b01b8-238">Find the section for managing CNAMEs.</span></span> <span data-ttu-id="b01b8-239">Ehhez esetleg a különleges beállítások lapjára kell lépnie, és ott a CNAME, Alias vagy Altartományok kifejezést kell keresnie.</span><span class="sxs-lookup"><span data-stu-id="b01b8-239">You may have to go to an advanced settings page and look for the words CNAME, Alias, or Subdomains.</span></span>

<span data-ttu-id="b01b8-240">Hozzon létre egy CNAME rekordot, amely leképezhető a kiválasztott altartomány (például **statikus** vagy **cdn**) számára a **végpont állomásnév** korábban a portál látható.</span><span class="sxs-lookup"><span data-stu-id="b01b8-240">Create a CNAME record that maps your chosen subdomain (for example, **static** or **cdn**) to the **Endpoint host name** shown earlier in the portal.</span></span> 

### <a name="enter-the-custom-domain-in-azure"></a><span data-ttu-id="b01b8-241">Az egyéni tartománynév megadása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="b01b8-241">Enter the custom domain in Azure</span></span>

<span data-ttu-id="b01b8-242">Lépjen vissza az **Egyéni tartomány hozzáadása** lapra, és adja meg az egyéni tartományt az altartománnyal együtt a párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="b01b8-242">Return to the **Add a custom domain** page, and enter your custom domain, including the subdomain, in the dialog box.</span></span> <span data-ttu-id="b01b8-243">Adja meg például a következőt: `cdn.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="b01b8-243">For example, enter `cdn.contoso.com`.</span></span>   
   
<span data-ttu-id="b01b8-244">Az Azure ellenőrzi, hogy a megadott tartománynév esetében létezik-e a CNAME-rekord.</span><span class="sxs-lookup"><span data-stu-id="b01b8-244">Azure verifies that the CNAME record exists for the domain name you have entered.</span></span> <span data-ttu-id="b01b8-245">Ha a CNAME helyes, az egyéni tartomány érvényesítve lesz.</span><span class="sxs-lookup"><span data-stu-id="b01b8-245">If the CNAME is correct, your custom domain is validated.</span></span>

<span data-ttu-id="b01b8-246">Időbe telhet, amíg megtörténik a CNAME-rekord névkiszolgálókon való propagálása az interneten.</span><span class="sxs-lookup"><span data-stu-id="b01b8-246">It can take time for the CNAME record to propagate to name servers on the Internet.</span></span> <span data-ttu-id="b01b8-247">Ha a tartomány nincs érvényesítve azonnal, várjon néhány percet, és próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="b01b8-247">If your domain is not validated immediately, wait a few minutes and try again.</span></span>

### <a name="test-the-custom-domain"></a><span data-ttu-id="b01b8-248">Az egyéni tartomány tesztelése</span><span class="sxs-lookup"><span data-stu-id="b01b8-248">Test the custom domain</span></span>

<span data-ttu-id="b01b8-249">Egy böngészőben nyissa meg az `index.html` fájlt az egyéni tartomány (például a `cdn.contoso.com/index.html`) használatával annak ellenőrzéséhez, hogy az eredmény ugyanaz, mintha közvetlenül az `<endpointname>azureedge.net/index.html` helyre lépett volna.</span><span class="sxs-lookup"><span data-stu-id="b01b8-249">In a browser, navigate to the `index.html` file using your custom domain (for example, `cdn.contoso.com/index.html`) to verify that the result is the same as when you go directly to `<endpointname>azureedge.net/index.html`.</span></span>

![Mintaalkalmazás egyéni tartományi URL-címet használó kezdőlapja](media/app-service-web-tutorial-content-delivery-network/home-page-custom-domain.png)

<span data-ttu-id="b01b8-251">További információkért lásd: [Azure CDN-tartalom leképezése egyéni tartományra](../cdn/cdn-map-content-to-custom-domain.md)</span><span class="sxs-lookup"><span data-stu-id="b01b8-251">For more information, see [Map Azure CDN content to a custom domain](../cdn/cdn-map-content-to-custom-domain.md).</span></span>

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a><span data-ttu-id="b01b8-252">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b01b8-252">Next steps</span></span>

<span data-ttu-id="b01b8-253">Hogy mit tudott:</span><span class="sxs-lookup"><span data-stu-id="b01b8-253">What you learned:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b01b8-254">CDN-végpont létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b01b8-254">Create a CDN endpoint.</span></span>
> * <span data-ttu-id="b01b8-255">Gyorsítótárazott objektumok frissítése.</span><span class="sxs-lookup"><span data-stu-id="b01b8-255">Refresh cached assets.</span></span>
> * <span data-ttu-id="b01b8-256">Gyorsítótárazott verziók felügyelete lekérdezési karakterláncok használatával.</span><span class="sxs-lookup"><span data-stu-id="b01b8-256">Use query strings to control cached versions.</span></span>
> * <span data-ttu-id="b01b8-257">Egyéni tartomány használata a CDN-végponthoz.</span><span class="sxs-lookup"><span data-stu-id="b01b8-257">Use a custom domain for the CDN endpoint.</span></span>

<span data-ttu-id="b01b8-258">Útmutató: a következő cikkekben CDN teljesítményének optimalizálásához:</span><span class="sxs-lookup"><span data-stu-id="b01b8-258">Learn how to optimize CDN performance in the following articles:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b01b8-259">A teljesítmény javítása a fájlok tömörítésével az Azure CDN-ben</span><span class="sxs-lookup"><span data-stu-id="b01b8-259">Improve performance by compressing files in Azure CDN</span></span>](../cdn/cdn-improve-performance.md)

> [!div class="nextstepaction"]
> [<span data-ttu-id="b01b8-260">Eszközök előzetes betöltése Azure CDN-végponton</span><span class="sxs-lookup"><span data-stu-id="b01b8-260">Pre-load assets on an Azure CDN endpoint</span></span>](../cdn/cdn-preload-endpoint.md)
