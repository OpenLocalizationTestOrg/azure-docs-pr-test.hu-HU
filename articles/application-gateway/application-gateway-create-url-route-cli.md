---
title: "URL-útválasztási szabályok - Azure CLI 2.0 használatával Alkalmazásátjáró létrehozása |} Microsoft Docs"
description: "Ezen a lapon létrehozása, megadni az URL-útválasztási szabályok használata Azure Alkalmazásátjáró utasításokat tartalmazza."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: gwallace
ms.openlocfilehash: 958049830d6753ec26635f18f8f8b2fabdec0733
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-application-gateway-using-path-based-routing-with-azure-cli-20"></a><span data-ttu-id="f2bad-103">Az Azure CLI 2.0 elérési alapú útválasztás használatával Alkalmazásátjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="f2bad-103">Create an application gateway using Path-based routing with Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="f2bad-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f2bad-104">Azure portal</span></span>](application-gateway-create-url-route-portal.md)
> * [<span data-ttu-id="f2bad-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="f2bad-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-url-route-arm-ps.md)
> * [<span data-ttu-id="f2bad-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f2bad-106">Azure CLI 2.0</span></span>](application-gateway-create-url-route-cli.md)

<span data-ttu-id="f2bad-107">URL-cím elérési út-alapú útválasztási lehetővé teszi a HTTP-kérések URL-címe alapján útvonalak hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="f2bad-107">URL Path-based routing enables you to associate routes based on the URL path of an Http request.</span></span> <span data-ttu-id="f2bad-108">Ellenőrzi, hogy nincs konfigurálva az URL-cím szerepel az Alkalmazásátjáró a háttér-készlet egy útvonalat, és elküldi a hálózati forgalom a meghatározott háttér-készlethez.</span><span class="sxs-lookup"><span data-stu-id="f2bad-108">It checks if there is a route to a back-end pool configured for the URL presented in the Application Gateway and sends the network traffic to the defined back-end pool.</span></span> <span data-ttu-id="f2bad-109">URL-alapú útválasztási gyakori felhasználási-hoz való különböző háttér-kiszolgálófiók tartalom különböző érkező kérések elosztása.</span><span class="sxs-lookup"><span data-stu-id="f2bad-109">A common use for URL-based routing is to load balance requests for different content types to different back-end server pools.</span></span>

<span data-ttu-id="f2bad-110">Az Alkalmazásátjáró URL-alapú útválasztási vezet be egy új szabály típusa.</span><span class="sxs-lookup"><span data-stu-id="f2bad-110">URL-based routing introduces a new rule type to application gateway.</span></span> <span data-ttu-id="f2bad-111">Alkalmazásátjáró két szabály tartozik: alapvető és PathBasedRouting.</span><span class="sxs-lookup"><span data-stu-id="f2bad-111">Application gateway has two rule types: basic and PathBasedRouting.</span></span> <span data-ttu-id="f2bad-112">Alapszintű szabály típusa mellett ciklikus multiplexelés terjesztési PathBasedRouting közben a háttér-készletek ciklikus multiplexelés szolgáltatást biztosít, is a kérelem URL-címének elérési út mintája figyelembe veszi a háttér-készlet kiválasztása során.</span><span class="sxs-lookup"><span data-stu-id="f2bad-112">Basic rule type provides round-robin service for the back-end pools while PathBasedRouting in addition to round robin distribution, also takes path pattern of the request URL into account while choosing the back-end pool.</span></span>

## <a name="scenario"></a><span data-ttu-id="f2bad-113">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="f2bad-113">Scenario</span></span>

<span data-ttu-id="f2bad-114">A következő példában Application Gateway van szolgáltató contoso.com forgalmát a két háttér-kiszolgálófiók rendelkezik: egy alapértelmezett kiszolgálókészletet és egy kép kiszolgálókészlethez.</span><span class="sxs-lookup"><span data-stu-id="f2bad-114">In the following example, Application Gateway is serving traffic for contoso.com with two back-end server pools: a default server pool and an image server pool.</span></span>

<span data-ttu-id="f2bad-115">Kérések a http://contoso.com/image * legyenek átirányítva kép kiszolgálókészlethez (imagesBackendPool), ha az elérési út mintája nem egyezik, egy alapértelmezett kiszolgálókészletet (appGatewayBackendPool) van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="f2bad-115">Requests for http://contoso.com/image* are routed to image server pool (imagesBackendPool), if the path pattern does not match, a default server pool (appGatewayBackendPool) is selected.</span></span>

![URL-cím útvonal](./media/application-gateway-create-url-route-cli/scenario.png)

## <a name="log-in-to-azure"></a><span data-ttu-id="f2bad-117">Jelentkezzen be az Azure-ba</span><span class="sxs-lookup"><span data-stu-id="f2bad-117">Log in to Azure</span></span>

<span data-ttu-id="f2bad-118">Nyissa meg a **Microsoft Azure parancssori**, és jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="f2bad-118">Open the **Microsoft Azure Command Prompt**, and log in.</span></span> 

```azurecli
az login -u "username"
```

> [!NOTE]
> <span data-ttu-id="f2bad-119">Is `az login` az eszköz bejelentkezéshez írja be a kódot, aka.ms/devicelogin igénylő kapcsoló nélkül.</span><span class="sxs-lookup"><span data-stu-id="f2bad-119">You can also use `az login` without the switch for device login that requires entering a code at aka.ms/devicelogin.</span></span>

<span data-ttu-id="f2bad-120">Miután beírta a fenti példában, a kód valósul meg.</span><span class="sxs-lookup"><span data-stu-id="f2bad-120">Once you type the preceding example, a code is provided.</span></span> <span data-ttu-id="f2bad-121">Nyissa meg a böngészőben a bejelentkezési folyamat folytatásához https://aka.ms/devicelogin.</span><span class="sxs-lookup"><span data-stu-id="f2bad-121">Navigate to https://aka.ms/devicelogin in a browser to continue the login process.</span></span>

![cmd megjelenítő eszköz-bejelentkezés][1]

<span data-ttu-id="f2bad-123">A böngészőben írja be a kapott kódot.</span><span class="sxs-lookup"><span data-stu-id="f2bad-123">In the browser, enter the code you received.</span></span> <span data-ttu-id="f2bad-124">Ekkor megnyílik egy bejelentkezési oldalára.</span><span class="sxs-lookup"><span data-stu-id="f2bad-124">You are redirected to a sign-in page.</span></span>

![Írja be a kódját a böngésző][2]

<span data-ttu-id="f2bad-126">Ha nincs megadva a kód be van jelentkezve, zárja be a böngészőt, és a forgatókönyv a folytatáshoz.</span><span class="sxs-lookup"><span data-stu-id="f2bad-126">Once the code has been entered you are signed in, close the browser to continue on with the scenario.</span></span>

![sikeres volt][3]

## <a name="add-a-path-based-rule-to-an-existing-application-gateway"></a><span data-ttu-id="f2bad-128">Elérési út alapú szabályokat adhat hozzá egy meglévő Alkalmazásátjáró</span><span class="sxs-lookup"><span data-stu-id="f2bad-128">Add a path-based rule to an existing application gateway</span></span>

<span data-ttu-id="f2bad-129">Alkalmazásátjáró definiált elérésiút-szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="f2bad-129">Create an application gateway with a path rule defined</span></span>

### <a name="create-a-new-back-end-pool"></a><span data-ttu-id="f2bad-130">Háttér-készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="f2bad-130">Create a new back-end pool</span></span>

<span data-ttu-id="f2bad-131">Alkalmazásbeállítás átjáró konfigurálása **imagesBackendPool** az elosztott terhelésű hálózati forgalmat a háttér-készletben.</span><span class="sxs-lookup"><span data-stu-id="f2bad-131">Configure application gateway setting **imagesBackendPool** for the load-balanced network traffic in the back-end pool.</span></span> <span data-ttu-id="f2bad-132">Ebben a példában beállítások különböző háttér-tárolókészlet az új háttér-készlet.</span><span class="sxs-lookup"><span data-stu-id="f2bad-132">In this example, you configure different back-end pool settings for the new back-end pool.</span></span> <span data-ttu-id="f2bad-133">Mindegyik háttérkészlet saját háttérkészlet-beállításokkal rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="f2bad-133">Each back-end pool can have its own back-end pool setting.</span></span>  <span data-ttu-id="f2bad-134">A szabályok a háttérbeli HTTP-beállítások használatával irányítják a forgalmat a háttérkészlet megfelelő tagjaira.</span><span class="sxs-lookup"><span data-stu-id="f2bad-134">Backend HTTP settings are used by rules to route traffic to the correct backend pool members.</span></span> <span data-ttu-id="f2bad-135">Ez határozza meg a protokoll és a forgalom a háttérrendszer a készlet tagjainak való küldés során használt port.</span><span class="sxs-lookup"><span data-stu-id="f2bad-135">This determines the protocol and port that is used when sending traffic to the backend pool members.</span></span> <span data-ttu-id="f2bad-136">A cookie-alapú munkameneteket szintén a háttérbeli HTTP-beállítások határozzák meg.</span><span class="sxs-lookup"><span data-stu-id="f2bad-136">Cookie-based sessions are also determined by the backend HTTP settings.</span></span>  <span data-ttu-id="f2bad-137">Ha engedélyezve van, a cookie-alapú munkamenet-affinitás az egyes csomagokhoz tartozó korábbi kéréseknek megfelelő háttérrendszerekre irányítja a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="f2bad-137">If enabled, cookie-based session affinity sends traffic to the same backend as previous requests for each packet.</span></span>

```azurecli-interactive
az network application-gateway address-pool create \
--gateway-name AdatumAppGateway \
--name imagesBackendPool  \
--resource-group myresourcegroup \
--servers 10.0.0.6 10.0.0.7
```

### <a name="create-a-new-front-end-port"></a><span data-ttu-id="f2bad-138">Hozzon létre egy új előtér-port</span><span class="sxs-lookup"><span data-stu-id="f2bad-138">Create a new front-end port</span></span>

<span data-ttu-id="f2bad-139">Konfigurálja az előtérbeli portot egy Application Gatewayhez.</span><span class="sxs-lookup"><span data-stu-id="f2bad-139">Configure the front-end port for an application gateway.</span></span> <span data-ttu-id="f2bad-140">Az előtérbeli port konfigurációs objektumával egy figyelő meghatározza, melyik porton figyeli a forgalmat az Application Gateway a figyelőn.</span><span class="sxs-lookup"><span data-stu-id="f2bad-140">The front-end port configuration object is used by a listener to define what port the Application Gateway listens for traffic on the listener.</span></span>

```azurecli-interactive
az network application-gateway frontend-port create --port 82 --gateway-name AdatumAppGateway --resource-group myresourcegroup --name port82
```

### <a name="create-a-new-listener"></a><span data-ttu-id="f2bad-141">Hozzon létre egy új figyelőt</span><span class="sxs-lookup"><span data-stu-id="f2bad-141">Create a new listener</span></span>

<span data-ttu-id="f2bad-142">Konfigurálja a figyelőt.</span><span class="sxs-lookup"><span data-stu-id="f2bad-142">Configure the listener.</span></span> <span data-ttu-id="f2bad-143">Ebben a lépésben konfiguráljuk a figyelőt a bejövő hálózati forgalmat fogadó nyilvános IP-címhez és porthoz.</span><span class="sxs-lookup"><span data-stu-id="f2bad-143">This step configures the listener for the public IP address and port used to receive incoming network traffic.</span></span> <span data-ttu-id="f2bad-144">Az alábbi példában a korábban konfigurált előtér-IP-konfiguráció, előtér-konfiguráció és a protokollt (http vagy https), és konfigurálja a figyelő.</span><span class="sxs-lookup"><span data-stu-id="f2bad-144">The following example takes the previously configured front-end IP configuration,  front-end port configuration, and a protocol (http or https) and configures the listener.</span></span> <span data-ttu-id="f2bad-145">Ebben a példában a figyelő a HTTP-forgalom a nyilvános IP-címet, amely korábban a 82-es porton figyel.</span><span class="sxs-lookup"><span data-stu-id="f2bad-145">In this example, the listener listens to HTTP traffic on port 82 on the public IP address that was created earlier.</span></span>

```azurecli-interactive
az network application-gateway http-listener create --name imageListener --frontend-ip appGatewayFrontendIP  --frontend-port port82 --resource-group myresourcegroup --gateway-name AdatumAppGateway
```

### <a name="create-the-url-path-map"></a><span data-ttu-id="f2bad-146">Az URL-cím elérési út térkép létrehozásához</span><span class="sxs-lookup"><span data-stu-id="f2bad-146">Create the Url path map</span></span>

<span data-ttu-id="f2bad-147">A háttér-készletek az URL-cím szabály elérési útvonalainak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="f2bad-147">Configure URL rule paths for the back-end pools.</span></span> <span data-ttu-id="f2bad-148">Ez a lépés konfigurál a relatív elérési utat határozza meg az URL-címet, és melyik háttér-készlet hozzá van rendelve, a bejövő forgalom kezeléséhez közötti leképezést Alkalmazásátjáró segítségével.</span><span class="sxs-lookup"><span data-stu-id="f2bad-148">This step configures the relative path used by application gateway to define the mapping between URL path and which back-end pool is assigned to handle the incoming traffic.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f2bad-149">Egyes elérési utakat kell kezdődnie, /, és csak egy "\*" engedélyezett, a végén.</span><span class="sxs-lookup"><span data-stu-id="f2bad-149">Each path must start with / and the only place a "\*" is allowed, is at the end.</span></span> <span data-ttu-id="f2bad-150">Érvényes többek között az /xyz, /xyz* vagy/xyz / *.</span><span class="sxs-lookup"><span data-stu-id="f2bad-150">Valid examples are /xyz, /xyz* or /xyz/*.</span></span> <span data-ttu-id="f2bad-151">Az az elérési út matcher táplált a karakterlánc nem tartalmaz sem szöveges az első után "?" vagy "#", és ezek a karakterek nem engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="f2bad-151">The string fed to the path matcher does not include any text after the first "?" or "#", and those characters are not allowed.</span></span> 

<span data-ttu-id="f2bad-152">Az alábbi példa létrehoz egy szabály a "/ képek / *" Háttér "imagesBackendPool." elérési út adatforgalom</span><span class="sxs-lookup"><span data-stu-id="f2bad-152">The following example creates one rule for "/images/*" path routing traffic to back-end "imagesBackendPool."</span></span> <span data-ttu-id="f2bad-153">Ez a szabály biztosítja, hogy az egyes URL-címek háttérkiszolgálóra továbbítódik.</span><span class="sxs-lookup"><span data-stu-id="f2bad-153">This rule ensures that traffic for each set of urls is routed to the backend.</span></span> <span data-ttu-id="f2bad-154">Például http://adatum.com/images/figure1.jpg kerül "imagesBackendPool."</span><span class="sxs-lookup"><span data-stu-id="f2bad-154">For example, http://adatum.com/images/figure1.jpg goes to "imagesBackendPool."</span></span> <span data-ttu-id="f2bad-155">Ha az elérési út nem felel meg az előre definiált elérésiút-szabály, a szabály térkép konfiguráció is konfigurálja egy háttér címkészletet.</span><span class="sxs-lookup"><span data-stu-id="f2bad-155">If the path doesn't match any of the pre-defined path rules, the rule path map configuration also configures a default back-end address pool.</span></span> <span data-ttu-id="f2bad-156">Például http://adatum.com/shoppingcart/test.html kerül pool1, mint az alapértelmezett alkalmazáskészlet páratlan forgalom van definiálva.</span><span class="sxs-lookup"><span data-stu-id="f2bad-156">For example, http://adatum.com/shoppingcart/test.html goes to pool1 as it is defined as the default pool for unmatched traffic.</span></span>

```azurecli-interactive
az network application-gateway url-path-map create \
--gateway-name AdatumAppGateway \
--name imagespathmap \
--paths /images/* \
--resource-group myresourcegroup2 \
--address-pool imagesBackendPool \
--default-address-pool appGatewayBackendPool \
--default-http-settings appGatewayBackendHttpSettings \
--http-settings appGatewayBackendHttpSettings \
--rule-name images
```

## <a name="next-steps"></a><span data-ttu-id="f2bad-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f2bad-157">Next steps</span></span>

<span data-ttu-id="f2bad-158">Ha azt szeretné, további információt a Secure Sockets Layer (SSL) kiszervezési, lásd: [konfigurálása az SSL-kiszervezés Alkalmazásátjáró](application-gateway-ssl-cli.md).</span><span class="sxs-lookup"><span data-stu-id="f2bad-158">If you want to learn about Secure Sockets Layer (SSL) offload, see [Configure an application gateway for SSL offload](application-gateway-ssl-cli.md).</span></span>


[scenario]: ./media/application-gateway-create-url-route-cli/scenario.png
[1]: ./media/application-gateway-create-url-route-cli/figure1.png
[2]: ./media/application-gateway-create-url-route-cli/figure2.png
[3]: ./media/application-gateway-create-url-route-cli/figure3.png
