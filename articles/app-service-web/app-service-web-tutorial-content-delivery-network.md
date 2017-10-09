---
title: a CDN tooan Azure App Service aaaAdd |} Microsoft Docs
description: "Adja hozzá a Content Delivery Network (CDN) tooan Azure App Service toocache, és Bezárás tooyour ügyfelek hello világ biztosításához a statikus fájlok a kiszolgálókról."
services: app-service\web
author: syntaxc4
ms.author: cfowler
ms.date: 05/31/2017
ms.topic: tutorial
ms.service: app-service-web
manager: erikre
ms.workload: web
ms.custom: mvc
ms.openlocfilehash: 88b7fd884517279064472b804a6d1dc2921cbd24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-content-delivery-network-cdn-tooan-azure-app-service"></a><span data-ttu-id="3a7f8-103">Adja hozzá a Content Delivery Network (CDN) tooan Azure App Service</span><span class="sxs-lookup"><span data-stu-id="3a7f8-103">Add a Content Delivery Network (CDN) tooan Azure App Service</span></span>

<span data-ttu-id="3a7f8-104">[Az Azure Content Delivery Network (CDN)](../cdn/cdn-overview.md) gyorsítótárazza a statikus webtartalmakat stratégiailag kiválasztott helyeken tooprovide maximális átviteli sebesség tartalom toousers kézbesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-104">[Azure Content Delivery Network (CDN)](../cdn/cdn-overview.md) caches static web content at strategically placed locations tooprovide maximum throughput for delivering content toousers.</span></span> <span data-ttu-id="3a7f8-105">hello CDN is csökkenti a kiszolgáló terhelését a webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-105">hello CDN also decreases server load on your web app.</span></span> <span data-ttu-id="3a7f8-106">Ez az oktatóanyag bemutatja, hogyan tooadd Azure CDN tooa [az Azure App Service webalkalmazás](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3a7f8-106">This tutorial shows how tooadd Azure CDN tooa [web app in Azure App Service](app-service-web-overview.md).</span></span> 

<span data-ttu-id="3a7f8-107">Íme hello kezdőlap hello minta statikus HTML hely fog dolgozni:</span><span class="sxs-lookup"><span data-stu-id="3a7f8-107">Here's hello home page of hello sample static HTML site that you'll work with:</span></span>

![Mintaalkalmazás kezdőlapja](media/app-service-web-tutorial-content-delivery-network/sample-app-home-page.png)

<span data-ttu-id="3a7f8-109">Ismertetett témák:</span><span class="sxs-lookup"><span data-stu-id="3a7f8-109">What you'll learn:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3a7f8-110">CDN-végpont létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-110">Create a CDN endpoint.</span></span>
> * <span data-ttu-id="3a7f8-111">Gyorsítótárazott objektumok frissítése.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-111">Refresh cached assets.</span></span>
> * <span data-ttu-id="3a7f8-112">Használjon lekérdezési karakterláncok gyorsítótárazott toocontrol verziók.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-112">Use query strings toocontrol cached versions.</span></span>
> * <span data-ttu-id="3a7f8-113">Használja az egyéni tartománynév hello CDN-végponthoz.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-113">Use a custom domain for hello CDN endpoint.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3a7f8-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3a7f8-114">Prerequisites</span></span>

<span data-ttu-id="3a7f8-115">toocomplete Ez az oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="3a7f8-115">toocomplete this tutorial:</span></span>

- [<span data-ttu-id="3a7f8-116">A Git telepítése</span><span class="sxs-lookup"><span data-stu-id="3a7f8-116">Install Git</span></span>](https://git-scm.com/)
- [<span data-ttu-id="3a7f8-117">Az Azure CLI 2.0 telepítése</span><span class="sxs-lookup"><span data-stu-id="3a7f8-117">Install Azure CLI 2.0</span></span>](https://docs.microsoft.com/cli/azure/install-azure-cli)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-hello-web-app"></a><span data-ttu-id="3a7f8-118">Hello webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="3a7f8-118">Create hello web app</span></span>

<span data-ttu-id="3a7f8-119">toocreate hello webes alkalmazás is együttműködik, hajtsa végre hello [statikus HTML gyors üzembe helyezés](app-service-web-get-started-html.md) keresztül hello **Tallózás toohello app** lépés.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-119">toocreate hello web app that you'll work with, follow hello [static HTML quickstart](app-service-web-get-started-html.md) through hello **Browse toohello app** step.</span></span>

### <a name="have-a-custom-domain-ready"></a><span data-ttu-id="3a7f8-120">Rendelkezésre álló egyéni tartománynév</span><span class="sxs-lookup"><span data-stu-id="3a7f8-120">Have a custom domain ready</span></span>

<span data-ttu-id="3a7f8-121">toocomplete hello egyéni tartománynév lépéssel ebben az oktatóanyagban a tooown az egyéni tartománynév kell, és hozzáférést tooyour DNS beállításjegyzék rendelkezik a tartomány-szolgáltató (például a GoDaddy).</span><span class="sxs-lookup"><span data-stu-id="3a7f8-121">toocomplete hello custom domain step of this tutorial, you need tooown a custom domain and have access tooyour DNS registry for your domain provider (such as GoDaddy).</span></span> <span data-ttu-id="3a7f8-122">Például tooadd DNS-bejegyzéseket `contoso.com` és `www.contoso.com`, rendelkeznie kell hozzáféréssel tooconfigure hello DNS-beállításainak hello `contoso.com` gyökértartomány.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-122">For example, tooadd DNS entries for `contoso.com` and `www.contoso.com`, you must have access tooconfigure hello DNS settings for hello `contoso.com` root domain.</span></span>

<span data-ttu-id="3a7f8-123">Ha még nem rendelkezik egy tartomány nevét, fontolja meg a következő hello [App Service tartomány oktatóanyag](custom-dns-web-site-buydomains-web-app.md) egy tartomány használatával toopurchase hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-123">If you don't already have a domain name, consider following hello [App Service domain tutorial](custom-dns-web-site-buydomains-web-app.md) toopurchase a domain using hello Azure portal.</span></span> 

## <a name="log-in-toohello-azure-portal"></a><span data-ttu-id="3a7f8-124">Jelentkezzen be toohello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="3a7f8-124">Log in toohello Azure portal</span></span>

<span data-ttu-id="3a7f8-125">Nyisson meg egy böngészőt, és keresse meg a toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3a7f8-125">Open a browser and navigate toohello [Azure portal](https://portal.azure.com).</span></span>

## <a name="create-a-cdn-profile-and-endpoint"></a><span data-ttu-id="3a7f8-126">CDN-profil és -végpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="3a7f8-126">Create a CDN profile and endpoint</span></span>

<span data-ttu-id="3a7f8-127">A bal oldali navigációs hello, válassza ki **alkalmazásszolgáltatások**, majd válassza ki a hello hello alkalmazást [statikus HTML gyors üzembe helyezés](app-service-web-get-started-html.md).</span><span class="sxs-lookup"><span data-stu-id="3a7f8-127">In hello left navigation, select **App Services**, and then select hello app that you created in hello [static HTML quickstart](app-service-web-get-started-html.md).</span></span>

![Válassza ki az App Service alkalmazás hello portálon](media/app-service-web-tutorial-content-delivery-network/portal-select-app-services.png)

<span data-ttu-id="3a7f8-129">A hello **App Service** lap hello **beállítások** szakaszban jelölje be **hálózati > konfigurálása Azure CDN szolgáltatás használata az alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-129">In hello **App Service** page, in hello **Settings** section, select **Networking > Configure Azure CDN for your app**.</span></span>

![Válassza ki a CDN hello portálon](media/app-service-web-tutorial-content-delivery-network/portal-select-cdn.png)

<span data-ttu-id="3a7f8-131">A hello **Azure Content Delivery Network** lapján adja meg a hello **új végpont** beállításokat hello táblázatban megadottak szerint.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-131">In hello **Azure Content Delivery Network** page, provide hello **New endpoint** settings as specified in hello table.</span></span>

![Hello portálon profil és -végpont létrehozása](media/app-service-web-tutorial-content-delivery-network/portal-new-endpoint.png)

| <span data-ttu-id="3a7f8-133">Beállítás</span><span class="sxs-lookup"><span data-stu-id="3a7f8-133">Setting</span></span> | <span data-ttu-id="3a7f8-134">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="3a7f8-134">Suggested value</span></span> | <span data-ttu-id="3a7f8-135">Leírás</span><span class="sxs-lookup"><span data-stu-id="3a7f8-135">Description</span></span> |
| ------- | --------------- | ----------- |
| <span data-ttu-id="3a7f8-136">**CDN-profil**</span><span class="sxs-lookup"><span data-stu-id="3a7f8-136">**CDN profile**</span></span> | <span data-ttu-id="3a7f8-137">myCDNProfile</span><span class="sxs-lookup"><span data-stu-id="3a7f8-137">myCDNProfile</span></span> | <span data-ttu-id="3a7f8-138">Válassza ki **hozzon létre új** toocreate a CDN-profil.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-138">Select **Create new** toocreate a CDN profile.</span></span> <span data-ttu-id="3a7f8-139">A CDN-profil hello a CDN-végpontok gyűjteménye azonos IP-címek.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-139">A CDN profile is a collection of CDN endpoints with hello same pricing tier.</span></span> |
| <span data-ttu-id="3a7f8-140">**Tarifacsomag**</span><span class="sxs-lookup"><span data-stu-id="3a7f8-140">**Pricing tier**</span></span> | <span data-ttu-id="3a7f8-141">Akamai Standard</span><span class="sxs-lookup"><span data-stu-id="3a7f8-141">Standard Akamai</span></span> | <span data-ttu-id="3a7f8-142">Hello [tarifacsomag](../cdn/cdn-overview.md#azure-cdn-features) hello szolgáltató és elérhető funkciókkal határozza meg.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-142">hello [pricing tier](../cdn/cdn-overview.md#azure-cdn-features) specifies hello provider and available features.</span></span> <span data-ttu-id="3a7f8-143">Ebben az oktatóanyagban Standard Akamai használunk.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-143">In this tutorial, we are using Standard Akamai.</span></span> |
| <span data-ttu-id="3a7f8-144">**CDN-végpont neve**</span><span class="sxs-lookup"><span data-stu-id="3a7f8-144">**CDN endpoint name**</span></span> | <span data-ttu-id="3a7f8-145">Hello azureedge.net tartomány egyedi nevek</span><span class="sxs-lookup"><span data-stu-id="3a7f8-145">Any name that is unique in hello azureedge.net domain</span></span> | <span data-ttu-id="3a7f8-146">A gyorsítótárazott erőforrások hello tartományban  *\<endpointname >. azureedge.net*.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-146">You access your cached resources at hello domain *\<endpointname>.azureedge.net*.</span></span>

<span data-ttu-id="3a7f8-147">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-147">Select **Create**.</span></span>

<span data-ttu-id="3a7f8-148">Azure hello-profil és -végpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="3a7f8-148">Azure creates hello profile and endpoint.</span></span> <span data-ttu-id="3a7f8-149">hello új végpont megjelenik hello **végpontok** listájából hello ugyanazon az oldalon, és ha ki van építve hello állapota **futtató**.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-149">hello new endpoint appears in hello **Endpoints** list on hello same page, and when it's provisioned hello status is **Running**.</span></span>

![Új végpont a listában](media/app-service-web-tutorial-content-delivery-network/portal-new-endpoint-in-list.png)

### <a name="test-hello-cdn-endpoint"></a><span data-ttu-id="3a7f8-151">Teszt hello CDN-végpont</span><span class="sxs-lookup"><span data-stu-id="3a7f8-151">Test hello CDN endpoint</span></span>

<span data-ttu-id="3a7f8-152">A Verizon tarifacsomag kiválasztása esetén a végpontok propagálása körülbelül 90 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-152">If you selected Verizon pricing tier, it typically takes about 90 minutes for endpoint propagation.</span></span> <span data-ttu-id="3a7f8-153">Az Akamai esetében a terjesztés csak néhány percig tart</span><span class="sxs-lookup"><span data-stu-id="3a7f8-153">For Akamai, it takes a couple minutes for propagation</span></span>

<span data-ttu-id="3a7f8-154">hello mintaalkalmazás rendelkezik egy `index.html` fájl és *css*, *img*, és *js* egyéb statikus eszközök tartalmazó mappákat.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-154">hello sample app has an `index.html` file and *css*, *img*, and *js* folders that contain other static assets.</span></span> <span data-ttu-id="3a7f8-155">a tartalom elérési az alábbi fájlokat hello hello CDN-végpont hello azonos.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-155">hello content paths for all of these files are hello same at hello CDN endpoint.</span></span> <span data-ttu-id="3a7f8-156">Például mindkét következő URL-címek hello hozzáférési hello *bootstrap.css* hello fájlban *css* mappába:</span><span class="sxs-lookup"><span data-stu-id="3a7f8-156">For example, both of hello following URLs access hello *bootstrap.css* file in hello *css* folder:</span></span>

```
http://<appname>.azurewebsites.net/css/bootstrap.css
```

```
http://<endpointname>.azureedge.net/css/bootstrap.css
```

<span data-ttu-id="3a7f8-157">Keresse meg a böngésző toohello a következő URL-címe:</span><span class="sxs-lookup"><span data-stu-id="3a7f8-157">Navigate a browser toohello following URL:</span></span>

```
http://<endpointname>.azureedge.net/index.html
```

![A mintaalkalmazás CDN által szolgáltatott kezdőlapja](media/app-service-web-tutorial-content-delivery-network/sample-app-home-page-cdn.png)

 <span data-ttu-id="3a7f8-159">Láthatja, hogy futtatta-e korábban az Azure-webalkalmazás lapon azonos hello.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-159">You see hello same page that you ran earlier in an Azure web app.</span></span> <span data-ttu-id="3a7f8-160">Az Azure CDN fogadta hello származási web app eszközök, és van szolgál a hello CDN-végpont</span><span class="sxs-lookup"><span data-stu-id="3a7f8-160">Azure CDN has retrieved hello origin web app's assets and is serving them from hello CDN endpoint</span></span>

<span data-ttu-id="3a7f8-161">hogy ezen a lapon tárolja a CDN, frissítési hello lap hello tooensure.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-161">tooensure that this page is cached in hello CDN, refresh hello page.</span></span> <span data-ttu-id="3a7f8-162">Két kéri a hello azonos eszköz néha szükség hello CDN toocache hello lekért tartalom.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-162">Two requests for hello same asset are sometimes required for hello CDN toocache hello requested content.</span></span>

<span data-ttu-id="3a7f8-163">Az Azure CDN-profilok és -végpontok létrehozásával kapcsolatos további információkért lásd: [Az Azure CDN használatának első lépései](../cdn/cdn-create-new-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="3a7f8-163">For more information about creating Azure CDN profiles and endpoints, see [Getting started with Azure CDN](../cdn/cdn-create-new-endpoint.md).</span></span>

## <a name="purge-hello-cdn"></a><span data-ttu-id="3a7f8-164">Hello CDN kiürítése</span><span class="sxs-lookup"><span data-stu-id="3a7f8-164">Purge hello CDN</span></span>

<span data-ttu-id="3a7f8-165">hello CDN hello származási webalkalmazás hello--élettartama (TTL) konfigurációja alapján erőforrását rendszeresen frissülnek.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-165">hello CDN periodically refreshes its resources from hello origin web app based on hello time-to-live (TTL) configuration.</span></span> <span data-ttu-id="3a7f8-166">hello alapértelmezett élettartam érték hét nap.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-166">hello default TTL is seven days.</span></span>

<span data-ttu-id="3a7f8-167">Néha szükség lehet toorefresh hello CDN előtt hello TTL lejárata – például frissített tartalom toohello webes alkalmazás központi telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-167">At times you might need toorefresh hello CDN before hello TTL expiration -- for example, when you deploy updated content toohello web app.</span></span> <span data-ttu-id="3a7f8-168">tootrigger frissítését, manuálisan törölheti hello CDN erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-168">tootrigger a refresh, you can manually purge hello CDN resources.</span></span> 

<span data-ttu-id="3a7f8-169">Ebben a szakaszban hello oktatóanyag módosítása toohello webalkalmazás üzembe helyezése, és hello CDN tootrigger hello CDN toorefresh a gyorsítótár kiürítése.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-169">In this section of hello tutorial, you deploy a change toohello web app and purge hello CDN tootrigger hello CDN toorefresh its cache.</span></span>

### <a name="deploy-a-change-toohello-web-app"></a><span data-ttu-id="3a7f8-170">Változás toohello webalkalmazás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="3a7f8-170">Deploy a change toohello web app</span></span>

<span data-ttu-id="3a7f8-171">Nyissa meg hello `index.html` fájlt, és "-V2" toohello H1 címsor, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="3a7f8-171">Open hello `index.html` file and add "- V2" toohello H1 heading, as shown in hello following example:</span></span> 

```
<h1>Azure App Service - Sample Static HTML Site - V2</h1>
```

<span data-ttu-id="3a7f8-172">A módosítás véglegesítése, és telepítse azt toohello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-172">Commit your change and deploy it toohello web app.</span></span>

```bash
git commit -am "version 2"
git push azure master
```

<span data-ttu-id="3a7f8-173">Központi telepítés befejezése után a Tallózás toohello webes alkalmazás URL-CÍMÉT, és tekintse meg a hello módosítása.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-173">Once deployment has completed, browse toohello web app URL and you see hello change.</span></span>

```
http://<appname>.azurewebsites.net/index.html
```

![V2 a webapp címében](media/app-service-web-tutorial-content-delivery-network/v2-in-web-app-title.png)

<span data-ttu-id="3a7f8-175">Keresse meg a toohello CDN-végpont URL-címe hello kezdőlapjára, és nem jelenik meg a hello módosítani, mert a gyorsítótárazott verzió hello hello CDN még a még nem járt le.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-175">Browse toohello CDN endpoint URL for hello home page and you don't see hello change because hello cached version in hello CDN hasn't expired yet.</span></span> 

```
http://<endpointname>.azureedge.net/index.html
```

![A V2 nem jelenik meg a CDN-beli címben](media/app-service-web-tutorial-content-delivery-network/no-v2-in-cdn-title.png)

### <a name="purge-hello-cdn-in-hello-portal"></a><span data-ttu-id="3a7f8-177">Hello CDN hello portálon kiürítése</span><span class="sxs-lookup"><span data-stu-id="3a7f8-177">Purge hello CDN in hello portal</span></span>

<span data-ttu-id="3a7f8-178">tootrigger hello CDN tooupdate a gyorsítótárazott verzió, hello CDN kiürítése.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-178">tootrigger hello CDN tooupdate its cached version, purge hello CDN.</span></span>

<span data-ttu-id="3a7f8-179">Válassza ki a portál bal oldali navigációs hello, **erőforráscsoportok**, majd válassza ki a webalkalmazás (contoso.com) létrehozott erőforráscsoport hello.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-179">In hello portal left navigation, select **Resource groups**, and then select hello resource group that you created for your web app (myResourceGroup).</span></span>

![Erőforráscsoport kiválasztása](media/app-service-web-tutorial-content-delivery-network/portal-select-group.png)

<span data-ttu-id="3a7f8-181">Az erőforrások listájához hello válassza ki a CDN-végpontot.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-181">In hello list of resources, select your CDN endpoint.</span></span>

![Végpont kiválasztása](media/app-service-web-tutorial-content-delivery-network/portal-select-endpoint.png)

<span data-ttu-id="3a7f8-183">Hello hello tetején **végpont** kattintson **kiürítése**.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-183">At hello top of hello **Endpoint** page, click **Purge**.</span></span>

![Végleges törlés kiválasztva](media/app-service-web-tutorial-content-delivery-network/portal-select-purge.png)

<span data-ttu-id="3a7f8-185">Adja meg a tartalomelérési út hello toopurge kívánja.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-185">Enter hello content paths you wish toopurge.</span></span> <span data-ttu-id="3a7f8-186">A fájl teljes elérési útja toopurge át egyetlen fájl vagy egy elérési utat szegmens toopurge, és frissítse egy mappát teljes tartalmával.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-186">You can pass a complete file path toopurge an individual file, or a path segment toopurge and refresh all content in a folder.</span></span> <span data-ttu-id="3a7f8-187">Mivel módosította `index.html`, győződjön meg arról, hogy a hello elérési utak egyike.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-187">Since you changed `index.html`, make sure that is one of hello paths.</span></span>

<span data-ttu-id="3a7f8-188">Hello hello lap alsó részén, jelölje ki a **kiürítése**.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-188">At hello bottom of hello page, select **Purge**.</span></span>

![Oldal végleges törlése](media/app-service-web-tutorial-content-delivery-network/app-service-web-purge-cdn.png)

### <a name="verify-that-hello-cdn-is-updated"></a><span data-ttu-id="3a7f8-190">Győződjön meg arról, hogy hello CDN frissül.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-190">Verify that hello CDN is updated</span></span>

<span data-ttu-id="3a7f8-191">Várjon, amíg hello kiürítési kérés befejezze a feldolgozást, általában néhány percig.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-191">Wait until hello purge request finishes processing, typically a couple of minutes.</span></span> <span data-ttu-id="3a7f8-192">toosee hello aktuális állapotát, jelölje be hello harang ikonra hello oldal hello tetején.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-192">toosee hello current status, select hello bell icon at hello top of hello page.</span></span> 

![Törlési értesítés](media/app-service-web-tutorial-content-delivery-network/portal-purge-notification.png)

<span data-ttu-id="3a7f8-194">Keresse meg a toohello CDN végponti URL-Címének `index.html`, és ekkor megjelenik a hello toohello címét hozzáadta hello kezdőlapján V2.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-194">Browse toohello CDN endpoint URL for `index.html`, and now you see hello V2 that you added toohello title on hello home page.</span></span> <span data-ttu-id="3a7f8-195">Ez azt jelenti, hogy hello CDN gyorsítótár frissítése.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-195">This shows that hello CDN cache has been refreshed.</span></span>

```
http://<endpointname>.azureedge.net/index.html
```

![V2 a CDN-beli címben](media/app-service-web-tutorial-content-delivery-network/v2-in-cdn-title.png)

<span data-ttu-id="3a7f8-197">További információkért lásd az [Azure CDN-végpontok végleges törléséről](../cdn/cdn-purge-endpoint.md) szóló cikket.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-197">For more information, see [Purge an Azure CDN endpoint](../cdn/cdn-purge-endpoint.md).</span></span> 

## <a name="use-query-strings-tooversion-content"></a><span data-ttu-id="3a7f8-198">Lekérdezési karakterláncok tooversion tartalom használata</span><span class="sxs-lookup"><span data-stu-id="3a7f8-198">Use query strings tooversion content</span></span>

<span data-ttu-id="3a7f8-199">hello Azure CDN alábbi gyorsítótárazási viselkedés beállítások hello kínálja:</span><span class="sxs-lookup"><span data-stu-id="3a7f8-199">hello Azure CDN offers hello following caching behavior options:</span></span>

* <span data-ttu-id="3a7f8-200">Lekérdezési karakterláncok figyelmen kívül hagyása</span><span class="sxs-lookup"><span data-stu-id="3a7f8-200">Ignore query strings</span></span>
* <span data-ttu-id="3a7f8-201">Lekérdezési karakterláncok gyorsítótárazásának megkerülése</span><span class="sxs-lookup"><span data-stu-id="3a7f8-201">Bypass caching for query strings</span></span>
* <span data-ttu-id="3a7f8-202">Minden egyedi URL gyorsítótárazása</span><span class="sxs-lookup"><span data-stu-id="3a7f8-202">Cache every unique URL</span></span> 

<span data-ttu-id="3a7f8-203">először hello ezek hello alapértelmezett, ami azt jelenti, hogy egy eszköz függetlenül hello lekérdezési karakterlánc hello URL-cím csak egy gyorsítótárazott verzió van.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-203">hello first of these is hello default, which means there is only one cached version of an asset regardless of hello query string in hello URL.</span></span> 

<span data-ttu-id="3a7f8-204">Ebben a szakaszban hello oktatóanyag módosítja hello viselkedés toocache minden egyedi URL-cím gyorsítótárazása.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-204">In this section of hello tutorial, you change hello caching behavior toocache every unique URL.</span></span>

### <a name="change-hello-cache-behavior"></a><span data-ttu-id="3a7f8-205">Gyorsítótár-viselkedést hello módosítása</span><span class="sxs-lookup"><span data-stu-id="3a7f8-205">Change hello cache behavior</span></span>

<span data-ttu-id="3a7f8-206">Az Azure-portálon hello **CDN-végpont** lapon jelölje be **gyorsítótár**.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-206">In hello Azure portal **CDN Endpoint** page, select **Cache**.</span></span>

<span data-ttu-id="3a7f8-207">Válassza ki **minden egyedi URL-cím gyorsítótárazása** a hello **lekérdezési sztringek gyorsítótárazásának** legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-207">Select **Cache every unique URL** from hello **Query string caching behavior** drop-down list.</span></span>

<span data-ttu-id="3a7f8-208">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-208">Select **Save**.</span></span>

![Lekérdezési karakterláncok gyorsítótárazási működésének kiválasztása](media/app-service-web-tutorial-content-delivery-network/portal-select-caching-behavior.png)

### <a name="verify-that-unique-urls-are-cached-separately"></a><span data-ttu-id="3a7f8-210">Az egyedi URL-címek külön gyorsítótárazásának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="3a7f8-210">Verify that unique URLs are cached separately</span></span>

<span data-ttu-id="3a7f8-211">Egy böngészőben nyissa meg a kezdőlapján toohello hello CDN-végpont, de közé tartozik a lekérdezési karakterlánc:</span><span class="sxs-lookup"><span data-stu-id="3a7f8-211">In a browser, navigate toohello home page at hello CDN endpoint, but include a query string:</span></span> 

```
http://<endpointname>.azureedge.net/index.html?q=1
```

<span data-ttu-id="3a7f8-212">hello CDN hello aktuális webes alkalmazás tartalmát, amelyet "2" hello fejléc tartalmazza adja vissza.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-212">hello CDN returns hello current web app content, which includes "V2" in hello heading.</span></span> 

<span data-ttu-id="3a7f8-213">hogy ezen a lapon tárolja a CDN, frissítési hello lap hello tooensure.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-213">tooensure that this page is cached in hello CDN, refresh hello page.</span></span> 

<span data-ttu-id="3a7f8-214">Nyissa meg `index.html` túl módosítsa a "2" és "V3", valamint hello.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-214">Open `index.html` and change "V2" too"V3", and deploy hello change.</span></span> 

```bash
git commit -am "version 3"
git push azure master
```

<span data-ttu-id="3a7f8-215">Egy böngészőben nyissa meg toohello CDN végponti URL-cím új lekérdezési karakterláncot, mint `q=2`.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-215">In a browser, go toohello CDN endpoint URL with a new query string such as `q=2`.</span></span> <span data-ttu-id="3a7f8-216">hello CDN lekérdezi hello aktuális `index.html` fájlt, és megjeleníti a "V3".</span><span class="sxs-lookup"><span data-stu-id="3a7f8-216">hello CDN gets hello current `index.html` file and displays "V3".</span></span>  <span data-ttu-id="3a7f8-217">De ha manuálisan lép toohello CDN-végpont a hello `q=1` lekérdezési karakterlánc, lásd a "2".</span><span class="sxs-lookup"><span data-stu-id="3a7f8-217">But if you navigate toohello CDN endpoint with hello `q=1` query string, you see "V2".</span></span>

```
http://<endpointname>.azureedge.net/index.html?q=2
```

![V3 a CDN-beli címben, 2. lekérdezési karakterlánc](media/app-service-web-tutorial-content-delivery-network/v3-in-cdn-title-qs2.png)

```
http://<endpointname>.azureedge.net/index.html?q=1
```

![V2 a CDN-beli címben, 1. lekérdezési karakterlánc](media/app-service-web-tutorial-content-delivery-network/v2-in-cdn-title-qs1.png)

<span data-ttu-id="3a7f8-220">A kimeneti jeleníti meg, hogy minden egyes lekérdezési karakterlánc eltérően kell-e kezelni:</span><span class="sxs-lookup"><span data-stu-id="3a7f8-220">This output shows that each query string is treated differently:</span></span>

* <span data-ttu-id="3a7f8-221">q = 1 használt előtt, így a gyorsítótárazott tartalom (V2) ad vissza.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-221">q=1 was used before, so cached contents are returned (V2).</span></span>
* <span data-ttu-id="3a7f8-222">q = 2 egy új, így hello legújabb web app tartalom letöltésének és (V3) adott vissza.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-222">q=2 is new, so hello latest web app contents are retrieved and returned (V3).</span></span>

<span data-ttu-id="3a7f8-223">További információkért lásd: [Az Azure CDN gyorsítótárazási viselkedésének vezérlése lekérdezési karakterláncokkal](../cdn/cdn-query-string.md).</span><span class="sxs-lookup"><span data-stu-id="3a7f8-223">For more information, see [Control Azure CDN caching behavior with query strings](../cdn/cdn-query-string.md).</span></span>

## <a name="map-a-custom-domain-tooa-cdn-endpoint"></a><span data-ttu-id="3a7f8-224">Az egyéni tartomány tooa CDN-végpont leképezése</span><span class="sxs-lookup"><span data-stu-id="3a7f8-224">Map a custom domain tooa CDN endpoint</span></span>

<span data-ttu-id="3a7f8-225">Az egyéni tartomány tooyour CDN-végpont hozzon létre egy CNAME rekordot kell rendelni.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-225">You'll map your custom domain tooyour CDN Endpoint by creating a CNAME record.</span></span> <span data-ttu-id="3a7f8-226">Egy olyan CNAME rekordot a DNS szolgáltatás, amely leképezi a forrás tartományi tooa cél tartomány.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-226">A CNAME record is a DNS feature that maps a source domain tooa destination domain.</span></span> <span data-ttu-id="3a7f8-227">Például, hogy le lehet képezni `cdn.contoso.com` vagy `static.contoso.com` túl`contoso.azureedge.net`.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-227">For example, you might map `cdn.contoso.com` or `static.contoso.com` too`contoso.azureedge.net`.</span></span>

<span data-ttu-id="3a7f8-228">Ha egyéni tartományt nem rendelkezik, fontolja meg hello [App Service tartomány oktatóanyag](custom-dns-web-site-buydomains-web-app.md) egy tartomány használatával toopurchase hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-228">If you don't have a custom domain, consider following hello [App Service domain tutorial](custom-dns-web-site-buydomains-web-app.md) toopurchase a domain using hello Azure portal.</span></span> 

### <a name="find-hello-hostname-toouse-with-hello-cname"></a><span data-ttu-id="3a7f8-229">A hello CNAME hello állomásnév toouse keresése</span><span class="sxs-lookup"><span data-stu-id="3a7f8-229">Find hello hostname toouse with hello CNAME</span></span>

<span data-ttu-id="3a7f8-230">A hello Azure-portálon **végpont** lapon, győződjön meg arról, hogy **áttekintése** van kiválasztva a bal oldali navigációs, és jelölje ki hello hello **+ egyéni tartomány** hello oldal hello tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-230">In hello Azure portal **Endpoint** page, make sure **Overview** is selected in hello left navigation, and then select hello **+ Custom Domain** button at hello top of hello page.</span></span>

![Egyéni tartomány hozzáadása kiválasztva](media/app-service-web-tutorial-content-delivery-network/portal-select-add-domain.png)

<span data-ttu-id="3a7f8-232">A hello **hozzá egyéni tartományt** lapon látni hello végpont állomás neve toouse a CNAME rekord létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-232">In hello **Add a custom domain** page, you see hello endpoint host name toouse in creating a CNAME record.</span></span> <span data-ttu-id="3a7f8-233">hello állomásnevet a CDN-végpont URL-címe van származtatva:  **&lt;EndpointName >. azureedge.net**.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-233">hello host name is derived from your CDN endpoint URL: **&lt;EndpointName>.azureedge.net**.</span></span> 

![Tartomány hozzáadása lap](media/app-service-web-tutorial-content-delivery-network/portal-add-domain.png)

### <a name="configure-hello-cname-with-your-domain-registrar"></a><span data-ttu-id="3a7f8-235">Hello CNAME REKORDOT a tartományregisztrálónál konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3a7f8-235">Configure hello CNAME with your domain registrar</span></span>

<span data-ttu-id="3a7f8-236">Keresse meg a tooyour tartomány regisztráló webhelyén, és keresse meg a DNS-rekordok létrehozásához hello szakaszt.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-236">Navigate tooyour domain registrar's web site, and locate hello section for creating DNS records.</span></span> <span data-ttu-id="3a7f8-237">Ezt a **Tartománynév**, **DNS**, **Névkiszolgáló kezelése** vagy hasonló területen találja.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-237">You might find this in a section such as **Domain Name**, **DNS**, or **Name Server Management**.</span></span>

<span data-ttu-id="3a7f8-238">Hello szakaszban található CNAME kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-238">Find hello section for managing CNAMEs.</span></span> <span data-ttu-id="3a7f8-239">Valószínűleg toogo tooan speciális beállításai oldal és CNAME, az Alias vagy a altartományok hello szavakat keresi.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-239">You may have toogo tooan advanced settings page and look for hello words CNAME, Alias, or Subdomains.</span></span>

<span data-ttu-id="3a7f8-240">Hozzon létre egy CNAME rekordot, amely leképezhető a kiválasztott altartomány (például **statikus** vagy **cdn**) toohello **végpont állomásnév** korábbi hello portal látható.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-240">Create a CNAME record that maps your chosen subdomain (for example, **static** or **cdn**) toohello **Endpoint host name** shown earlier in hello portal.</span></span> 

### <a name="enter-hello-custom-domain-in-azure"></a><span data-ttu-id="3a7f8-241">Adja meg az egyéni tartomány hello az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="3a7f8-241">Enter hello custom domain in Azure</span></span>

<span data-ttu-id="3a7f8-242">Térjen vissza a toohello **hozzá egyéni tartományt** lapon, és adja meg az egyéni tartomány, hello altartomány, beleértve a hello párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-242">Return toohello **Add a custom domain** page, and enter your custom domain, including hello subdomain, in hello dialog box.</span></span> <span data-ttu-id="3a7f8-243">Adja meg például a következőt: `cdn.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-243">For example, enter `cdn.contoso.com`.</span></span>   
   
<span data-ttu-id="3a7f8-244">Azure ellenőrzi, hogy létezik-e hello CNAME rekord megadott hello tartománynév.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-244">Azure verifies that hello CNAME record exists for hello domain name you have entered.</span></span> <span data-ttu-id="3a7f8-245">Ha hello CNAME helyességéről, az egyéni tartomány érvényességét.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-245">If hello CNAME is correct, your custom domain is validated.</span></span>

<span data-ttu-id="3a7f8-246">Hello CNAME rekord toopropagate tooname kiszolgálók hello Internet időt is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-246">It can take time for hello CNAME record toopropagate tooname servers on hello Internet.</span></span> <span data-ttu-id="3a7f8-247">Ha a tartomány nincs érvényesítve azonnal, várjon néhány percet, és próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-247">If your domain is not validated immediately, wait a few minutes and try again.</span></span>

### <a name="test-hello-custom-domain"></a><span data-ttu-id="3a7f8-248">Teszt hello egyéni tartományt</span><span class="sxs-lookup"><span data-stu-id="3a7f8-248">Test hello custom domain</span></span>

<span data-ttu-id="3a7f8-249">Egy böngészőben nyissa meg a toohello `index.html` fájlt az egyéni tartomány (például `cdn.contoso.com/index.html`), amely hello eredmény tooverify hello azonos, amikor módba közvetlenül túl`<endpointname>azureedge.net/index.html`.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-249">In a browser, navigate toohello `index.html` file using your custom domain (for example, `cdn.contoso.com/index.html`) tooverify that hello result is hello same as when you go directly too`<endpointname>azureedge.net/index.html`.</span></span>

![Mintaalkalmazás egyéni tartományi URL-címet használó kezdőlapja](media/app-service-web-tutorial-content-delivery-network/home-page-custom-domain.png)

<span data-ttu-id="3a7f8-251">További információkért lásd: [tartalom tooa egyéni térkép Azure CDN-tartomány](../cdn/cdn-map-content-to-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="3a7f8-251">For more information, see [Map Azure CDN content tooa custom domain](../cdn/cdn-map-content-to-custom-domain.md).</span></span>

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a><span data-ttu-id="3a7f8-252">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3a7f8-252">Next steps</span></span>

<span data-ttu-id="3a7f8-253">Hogy mit tudott:</span><span class="sxs-lookup"><span data-stu-id="3a7f8-253">What you learned:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3a7f8-254">CDN-végpont létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-254">Create a CDN endpoint.</span></span>
> * <span data-ttu-id="3a7f8-255">Gyorsítótárazott objektumok frissítése.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-255">Refresh cached assets.</span></span>
> * <span data-ttu-id="3a7f8-256">Használjon lekérdezési karakterláncok gyorsítótárazott toocontrol verziók.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-256">Use query strings toocontrol cached versions.</span></span>
> * <span data-ttu-id="3a7f8-257">Használja az egyéni tartománynév hello CDN-végponthoz.</span><span class="sxs-lookup"><span data-stu-id="3a7f8-257">Use a custom domain for hello CDN endpoint.</span></span>

<span data-ttu-id="3a7f8-258">Megtudhatja, hogyan toooptimize CDN teljesítmény hello a következő cikkek:</span><span class="sxs-lookup"><span data-stu-id="3a7f8-258">Learn how toooptimize CDN performance in hello following articles:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="3a7f8-259">A teljesítmény javítása a fájlok tömörítésével az Azure CDN-ben</span><span class="sxs-lookup"><span data-stu-id="3a7f8-259">Improve performance by compressing files in Azure CDN</span></span>](../cdn/cdn-improve-performance.md)

> [!div class="nextstepaction"]
> [<span data-ttu-id="3a7f8-260">Eszközök előzetes betöltése Azure CDN-végponton</span><span class="sxs-lookup"><span data-stu-id="3a7f8-260">Pre-load assets on an Azure CDN endpoint</span></span>](../cdn/cdn-preload-endpoint.md)
