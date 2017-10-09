---
title: "Alkalmazásátjáró használatával URL-cím útválasztási szabályok - aaaCreate Azure CLI 2.0 |} Microsoft Docs"
description: "Ezen a lapon nyújt útmutatást toocreate, Azure Alkalmazásátjáró használatával URL-útválasztási szabályok konfigurálása"
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
ms.openlocfilehash: 335b52be258945e1172eb0252b732e0e6ecb2ef0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-using-path-based-routing-with-azure-cli-20"></a><span data-ttu-id="57db6-103">Az Azure CLI 2.0 elérési alapú útválasztás használatával Alkalmazásátjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="57db6-103">Create an application gateway using Path-based routing with Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="57db6-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="57db6-104">Azure portal</span></span>](application-gateway-create-url-route-portal.md)
> * [<span data-ttu-id="57db6-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="57db6-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-url-route-arm-ps.md)
> * [<span data-ttu-id="57db6-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="57db6-106">Azure CLI 2.0</span></span>](application-gateway-create-url-route-cli.md)

<span data-ttu-id="57db6-107">URL-cím elérési út-alapú útválasztási lehetővé teszi, hogy Ön tooassociate útvonalak HTTP-kérések hello URL-címe alapján.</span><span class="sxs-lookup"><span data-stu-id="57db6-107">URL Path-based routing enables you tooassociate routes based on hello URL path of an Http request.</span></span> <span data-ttu-id="57db6-108">Ellenőrzi, hogy az útvonal tooa háttér-készlet hello URL-cím szerepel a hello Alkalmazásátjáró konfigurálva van, és elküldi a hello hálózati forgalom toohello definiált háttér-készlet.</span><span class="sxs-lookup"><span data-stu-id="57db6-108">It checks if there is a route tooa back-end pool configured for hello URL presented in hello Application Gateway and sends hello network traffic toohello defined back-end pool.</span></span> <span data-ttu-id="57db6-109">URL-alapú útválasztási gyakori felhasználási tooload-érkező kérések elosztása a különböző típusú tartalmakat toodifferent háttér-kiszolgálófiók készletek.</span><span class="sxs-lookup"><span data-stu-id="57db6-109">A common use for URL-based routing is tooload balance requests for different content types toodifferent back-end server pools.</span></span>

<span data-ttu-id="57db6-110">Új szabály típusa tooapplication átjáró URL-alapú útválasztási vezet be.</span><span class="sxs-lookup"><span data-stu-id="57db6-110">URL-based routing introduces a new rule type tooapplication gateway.</span></span> <span data-ttu-id="57db6-111">Alkalmazásátjáró két szabály tartozik: alapvető és PathBasedRouting.</span><span class="sxs-lookup"><span data-stu-id="57db6-111">Application gateway has two rule types: basic and PathBasedRouting.</span></span> <span data-ttu-id="57db6-112">Alapszintű szabálytípus biztosít a ciklikus multiplexelés hello háttér-szolgáltatás a készletbe közben PathBasedRouting továbbá tooround multiplexelés terjesztési, a is hello kérelem URL-címének elérési út mintája figyelembe veszi a hello háttér-készlet kiválasztásakor.</span><span class="sxs-lookup"><span data-stu-id="57db6-112">Basic rule type provides round-robin service for hello back-end pools while PathBasedRouting in addition tooround robin distribution, also takes path pattern of hello request URL into account while choosing hello back-end pool.</span></span>

## <a name="scenario"></a><span data-ttu-id="57db6-113">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="57db6-113">Scenario</span></span>

<span data-ttu-id="57db6-114">A következő példa hello, Application Gateway van szolgáltató contoso.com forgalmát a két háttér-kiszolgálófiók rendelkezik: egy alapértelmezett kiszolgálókészletet és egy kép kiszolgálókészlethez.</span><span class="sxs-lookup"><span data-stu-id="57db6-114">In hello following example, Application Gateway is serving traffic for contoso.com with two back-end server pools: a default server pool and an image server pool.</span></span>

<span data-ttu-id="57db6-115">Kérések a http://contoso.com/image * irányított tooimage kiszolgálókészlet (imagesBackendPool), ha hello elérési út mintája nem felel meg, egy alapértelmezett kiszolgálókészletet (appGatewayBackendPool) van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="57db6-115">Requests for http://contoso.com/image* are routed tooimage server pool (imagesBackendPool), if hello path pattern does not match, a default server pool (appGatewayBackendPool) is selected.</span></span>

![URL-cím útvonal](./media/application-gateway-create-url-route-cli/scenario.png)

## <a name="log-in-tooazure"></a><span data-ttu-id="57db6-117">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="57db6-117">Log in tooAzure</span></span>

<span data-ttu-id="57db6-118">Nyissa meg hello **Microsoft Azure parancssori**, és jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="57db6-118">Open hello **Microsoft Azure Command Prompt**, and log in.</span></span> 

```azurecli
az login -u "username"
```

> [!NOTE]
> <span data-ttu-id="57db6-119">Is `az login` az eszköz bejelentkezéshez írja be a kódot, aka.ms/devicelogin igénylő hello kapcsoló nélkül.</span><span class="sxs-lookup"><span data-stu-id="57db6-119">You can also use `az login` without hello switch for device login that requires entering a code at aka.ms/devicelogin.</span></span>

<span data-ttu-id="57db6-120">Amennyiben az előző példa hello írja be, a kód valósul meg.</span><span class="sxs-lookup"><span data-stu-id="57db6-120">Once you type hello preceding example, a code is provided.</span></span> <span data-ttu-id="57db6-121">Keresse meg a böngésző toocontinue hello bejelentkezési folyamat toohttps://aka.ms/devicelogin.</span><span class="sxs-lookup"><span data-stu-id="57db6-121">Navigate toohttps://aka.ms/devicelogin in a browser toocontinue hello login process.</span></span>

![cmd megjelenítő eszköz-bejelentkezés][1]

<span data-ttu-id="57db6-123">Hello böngészőben írja be a kapott hello kódot.</span><span class="sxs-lookup"><span data-stu-id="57db6-123">In hello browser, enter hello code you received.</span></span> <span data-ttu-id="57db6-124">Biztosan átirányított tooa bejelentkezési oldalára.</span><span class="sxs-lookup"><span data-stu-id="57db6-124">You are redirected tooa sign-in page.</span></span>

![böngésző tooenter kódot][2]

<span data-ttu-id="57db6-126">Ha nincs megadva hello kód be van jelentkezve, Bezárás hello böngésző toocontinue hello forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="57db6-126">Once hello code has been entered you are signed in, close hello browser toocontinue on with hello scenario.</span></span>

![sikeres volt][3]

## <a name="add-a-path-based-rule-tooan-existing-application-gateway"></a><span data-ttu-id="57db6-128">Adja hozzá az elérési út alapú szabály tooan meglévő Alkalmazásátjáró</span><span class="sxs-lookup"><span data-stu-id="57db6-128">Add a path-based rule tooan existing application gateway</span></span>

<span data-ttu-id="57db6-129">Alkalmazásátjáró definiált elérésiút-szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="57db6-129">Create an application gateway with a path rule defined</span></span>

### <a name="create-a-new-back-end-pool"></a><span data-ttu-id="57db6-130">Háttér-készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="57db6-130">Create a new back-end pool</span></span>

<span data-ttu-id="57db6-131">Alkalmazásbeállítás átjáró konfigurálása **imagesBackendPool** hello terhelésű hálózati forgalmára hello háttér-készlet.</span><span class="sxs-lookup"><span data-stu-id="57db6-131">Configure application gateway setting **imagesBackendPool** for hello load-balanced network traffic in hello back-end pool.</span></span> <span data-ttu-id="57db6-132">Ebben a példában beállítások másik háttér címkészletet hello új háttér-készlet.</span><span class="sxs-lookup"><span data-stu-id="57db6-132">In this example, you configure different back-end pool settings for hello new back-end pool.</span></span> <span data-ttu-id="57db6-133">Mindegyik háttérkészlet saját háttérkészlet-beállításokkal rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="57db6-133">Each back-end pool can have its own back-end pool setting.</span></span>  <span data-ttu-id="57db6-134">Szabályok tooroute forgalom toohello megfelelő háttér a készlet tagjainak háttér HTTP-beállításokat használja.</span><span class="sxs-lookup"><span data-stu-id="57db6-134">Backend HTTP settings are used by rules tooroute traffic toohello correct backend pool members.</span></span> <span data-ttu-id="57db6-135">Ez határozza meg, hello protokoll és toohello háttér a készlet tagjainak forgalom küldéséhez használt port.</span><span class="sxs-lookup"><span data-stu-id="57db6-135">This determines hello protocol and port that is used when sending traffic toohello backend pool members.</span></span> <span data-ttu-id="57db6-136">A munkamenetek cookie-alapú is a backendhttpsettings osztályhoz hello határozza meg.</span><span class="sxs-lookup"><span data-stu-id="57db6-136">Cookie-based sessions are also determined by hello backend HTTP settings.</span></span>  <span data-ttu-id="57db6-137">Ha engedélyezve van, a munkamenet cookie-alapú kapcsolat küld-e a forgalom toohello azonos háttér, egyes csomagok előző kérelmekben.</span><span class="sxs-lookup"><span data-stu-id="57db6-137">If enabled, cookie-based session affinity sends traffic toohello same backend as previous requests for each packet.</span></span>

```azurecli-interactive
az network application-gateway address-pool create \
--gateway-name AdatumAppGateway \
--name imagesBackendPool  \
--resource-group myresourcegroup \
--servers 10.0.0.6 10.0.0.7
```

### <a name="create-a-new-front-end-port"></a><span data-ttu-id="57db6-138">Hozzon létre egy új előtér-port</span><span class="sxs-lookup"><span data-stu-id="57db6-138">Create a new front-end port</span></span>

<span data-ttu-id="57db6-139">Hello előtér-port konfigurálása az Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="57db6-139">Configure hello front-end port for an application gateway.</span></span> <span data-ttu-id="57db6-140">hello előtér-port konfigurációs objektum használja egy figyelő toodefine melyik portot hello Alkalmazásátjáró figyeli a hello figyelő-forgalmat.</span><span class="sxs-lookup"><span data-stu-id="57db6-140">hello front-end port configuration object is used by a listener toodefine what port hello Application Gateway listens for traffic on hello listener.</span></span>

```azurecli-interactive
az network application-gateway frontend-port create --port 82 --gateway-name AdatumAppGateway --resource-group myresourcegroup --name port82
```

### <a name="create-a-new-listener"></a><span data-ttu-id="57db6-141">Hozzon létre egy új figyelőt</span><span class="sxs-lookup"><span data-stu-id="57db6-141">Create a new listener</span></span>

<span data-ttu-id="57db6-142">Hello figyelő konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="57db6-142">Configure hello listener.</span></span> <span data-ttu-id="57db6-143">Ez a lépés konfigurál hello figyelő hello nyilvános IP-cím és port használt tooreceive bejövő hálózati forgalom.</span><span class="sxs-lookup"><span data-stu-id="57db6-143">This step configures hello listener for hello public IP address and port used tooreceive incoming network traffic.</span></span> <span data-ttu-id="57db6-144">a következő példa hello hello korábban konfigurált előtér-IP-konfiguráció, előtér-konfiguráció és a protokollt (http vagy https), és konfigurálja a hello figyelő.</span><span class="sxs-lookup"><span data-stu-id="57db6-144">hello following example takes hello previously configured front-end IP configuration,  front-end port configuration, and a protocol (http or https) and configures hello listener.</span></span> <span data-ttu-id="57db6-145">Ebben a példában hello figyelő tooHTTP adatforgalmat hello nyilvános IP-címet, amely korábban a 82-es porton figyel.</span><span class="sxs-lookup"><span data-stu-id="57db6-145">In this example, hello listener listens tooHTTP traffic on port 82 on hello public IP address that was created earlier.</span></span>

```azurecli-interactive
az network application-gateway http-listener create --name imageListener --frontend-ip appGatewayFrontendIP  --frontend-port port82 --resource-group myresourcegroup --gateway-name AdatumAppGateway
```

### <a name="create-hello-url-path-map"></a><span data-ttu-id="57db6-146">Hello URL-cím elérési út térkép létrehozásához</span><span class="sxs-lookup"><span data-stu-id="57db6-146">Create hello Url path map</span></span>

<span data-ttu-id="57db6-147">URL-cím szabály útvonalak hello háttér-címkészletek konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="57db6-147">Configure URL rule paths for hello back-end pools.</span></span> <span data-ttu-id="57db6-148">Ez a lépés konfigurál hello relatív elérési út átjáró toodefine hello alkalmazástársításhoz közötti URL-címet, és melyik háttér-készlet kap toohandle hello bejövő forgalom által használt.</span><span class="sxs-lookup"><span data-stu-id="57db6-148">This step configures hello relative path used by application gateway toodefine hello mapping between URL path and which back-end pool is assigned toohandle hello incoming traffic.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="57db6-149">Egyes elérési utakat kell kezdődnie / és hello egyetlen hely, ahol a "\*" engedélyezett, a hello végén.</span><span class="sxs-lookup"><span data-stu-id="57db6-149">Each path must start with / and hello only place a "\*" is allowed, is at hello end.</span></span> <span data-ttu-id="57db6-150">Érvényes többek között az /xyz, /xyz* vagy/xyz / *.</span><span class="sxs-lookup"><span data-stu-id="57db6-150">Valid examples are /xyz, /xyz* or /xyz/*.</span></span> <span data-ttu-id="57db6-151">hello toohello elérési matcher táplált karakterlánc nem tartalmaz sem szöveges hello után először "?" vagy "#", és ezek a karakterek nem engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="57db6-151">hello string fed toohello path matcher does not include any text after hello first "?" or "#", and those characters are not allowed.</span></span> 

<span data-ttu-id="57db6-152">hello alábbi példa létrehoz egy szabály a "/ képek / *" elemzéshez forgalom tooback-end "imagesBackendPool."</span><span class="sxs-lookup"><span data-stu-id="57db6-152">hello following example creates one rule for "/images/*" path routing traffic tooback-end "imagesBackendPool."</span></span> <span data-ttu-id="57db6-153">Ez a szabály biztosítja, hogy az egyes URL-címek forgalom irányított toohello háttér.</span><span class="sxs-lookup"><span data-stu-id="57db6-153">This rule ensures that traffic for each set of urls is routed toohello backend.</span></span> <span data-ttu-id="57db6-154">Például http://adatum.com/images/figure1.jpg túl kerül "imagesBackendPool."</span><span class="sxs-lookup"><span data-stu-id="57db6-154">For example, http://adatum.com/images/figure1.jpg goes too"imagesBackendPool."</span></span> <span data-ttu-id="57db6-155">Hello elérési út bármely hello előre definiált elérésiút-szabály nem megfelelő, ha a szabály térkép konfiguráció hello is konfigurálja egy háttér címkészletet.</span><span class="sxs-lookup"><span data-stu-id="57db6-155">If hello path doesn't match any of hello pre-defined path rules, hello rule path map configuration also configures a default back-end address pool.</span></span> <span data-ttu-id="57db6-156">Például a http://adatum.com/shoppingcart/test.html toopool1 kerül, mint hello alapértelmezett alkalmazáskészlet páratlan forgalom van definiálva.</span><span class="sxs-lookup"><span data-stu-id="57db6-156">For example, http://adatum.com/shoppingcart/test.html goes toopool1 as it is defined as hello default pool for unmatched traffic.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="57db6-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="57db6-157">Next steps</span></span>

<span data-ttu-id="57db6-158">Ha azt szeretné, hogy Secure Sockets Layer (SSL) kiszervezési kapcsolatos toolearn, [konfigurálása az SSL-kiszervezés Alkalmazásátjáró](application-gateway-ssl-cli.md).</span><span class="sxs-lookup"><span data-stu-id="57db6-158">If you want toolearn about Secure Sockets Layer (SSL) offload, see [Configure an application gateway for SSL offload](application-gateway-ssl-cli.md).</span></span>


[scenario]: ./media/application-gateway-create-url-route-cli/scenario.png
[1]: ./media/application-gateway-create-url-route-cli/figure1.png
[2]: ./media/application-gateway-create-url-route-cli/figure2.png
[3]: ./media/application-gateway-create-url-route-cli/figure3.png
