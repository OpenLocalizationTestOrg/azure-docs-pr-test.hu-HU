---
title: "Az Azure Application Gateway több webhelyet |} Microsoft Docs"
description: "Ezen a lapon konfiguráljon egy meglévő Azure-alkalmazásokban átjárót ugyanahhoz az átjáróhoz, és az Azure portál a több webalkalmazás üzemeltetéséhez utasításokat tartalmaz."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 95f892f6-fa27-47ee-b980-7abf4f2c66a9
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: 84bd62ae17b7f7ba4cd815ef1f9880679607ebce
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-an-existing-application-gateway-for-hosting-multiple-web-applications"></a><span data-ttu-id="ef0fd-103">Konfiguráljon egy meglévő alkalmazás átjárót több webalkalmazás üzemeltetéséhez</span><span class="sxs-lookup"><span data-stu-id="ef0fd-103">Configure an existing application gateway for hosting multiple web applications</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="ef0fd-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ef0fd-104">Azure portal</span></span>](application-gateway-create-multisite-portal.md)
> * [<span data-ttu-id="ef0fd-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="ef0fd-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-multisite-azureresourcemanager-powershell.md)
> 
> 

<span data-ttu-id="ef0fd-106">Több helyet üzemeltető lehetővé teszi az ugyanazon Alkalmazásátjáró egynél több webalkalmazás telepítését.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-106">Multiple site hosting allows you to deploy more than one web application on the same application gateway.</span></span> <span data-ttu-id="ef0fd-107">Az állomásfejlécnek meghatározni, mely figyelő kapja forgalom a bejövő HTTP-kérelmek jelenlétére támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-107">It relies on presence of host header in the incoming HTTP request, to determine which listener would receive traffic.</span></span> <span data-ttu-id="ef0fd-108">A figyelő majd arra utasítja a megfelelő háttérkészlet-forgalom, be az átjáró szabályok meghatározását.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-108">The listener then directs traffic to appropriate backend pool as configured in the rules definition of the gateway.</span></span> <span data-ttu-id="ef0fd-109">Az SSL engedélyezve van a webes alkalmazásokhoz Alkalmazásátjáró a kiszolgálónév jelzése (SNI) bővítménye válassza ki a megfelelő figyelő a webes forgalom támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-109">In SSL enabled web applications, application gateway relies on the Server Name Indication (SNI) extension to choose the correct listener for the web traffic.</span></span> <span data-ttu-id="ef0fd-110">A közös több hely üzemeltetéséhez rendeltetése különböző webtartományok különböző háttér-kiszolgálófiók tárolókészletekben az érkező kérések elosztása.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-110">A common use for multiple site hosting is to load balance requests for different web domains to different back-end server pools.</span></span> <span data-ttu-id="ef0fd-111">Az azonos gyökértartomány több altartományt hasonló módon is tárolt alkalmazás ugyanahhoz az átjáróhoz.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-111">Similarly multiple subdomains of the same root domain could also be hosted on the same application gateway.</span></span>

## <a name="scenario"></a><span data-ttu-id="ef0fd-112">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="ef0fd-112">Scenario</span></span>

<span data-ttu-id="ef0fd-113">A következő példában Alkalmazásátjáró van kiszolgáló a contoso.com és fabrikam.com forgalom a két háttér-kiszolgálófiók rendelkezik: contoso kiszolgálókészlet és a fabrikam kiszolgálókészlethez.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-113">In the following example, application gateway is serving traffic for contoso.com and fabrikam.com with two back-end server pools: contoso server pool and fabrikam server pool.</span></span> <span data-ttu-id="ef0fd-114">Hasonló telepítő állomás altartományok például app.contoso.com és blog.contoso.com használható.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-114">Similar setup could be used to host subdomains like app.contoso.com and blog.contoso.com.</span></span>

![többhelyes forgatókönyv][multisite]

## <a name="before-you-begin"></a><span data-ttu-id="ef0fd-116">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="ef0fd-116">Before you begin</span></span>

<span data-ttu-id="ef0fd-117">Ebben a forgatókönyvben egy meglévő Alkalmazásátjáró többhelyes támogatást.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-117">This scenario adds multi-site support to an existing application gateway.</span></span> <span data-ttu-id="ef0fd-118">A forgatókönyv végrehajtásához, meglévő Alkalmazásátjáró kell konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-118">To complete this scenario, an existing application gateway needs to be available to configure.</span></span> <span data-ttu-id="ef0fd-119">Látogasson el [Alkalmazásátjáró létrehozása a portál használatával](application-gateway-create-gateway-portal.md) hogyan egy alapszintű application gateway létrehozása a portálon.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-119">Visit [Create an application gateway by using the portal](application-gateway-create-gateway-portal.md) to learn how to create a basic application gateway in the portal.</span></span>

<span data-ttu-id="ef0fd-120">Az Alkalmazásátjáró frissítéséhez szükséges lépéseket a következők:</span><span class="sxs-lookup"><span data-stu-id="ef0fd-120">The following are the steps needed to update the application gateway:</span></span>

1. <span data-ttu-id="ef0fd-121">Az egyes helyek használandó háttér-címkészletek létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-121">Create back-end pools to use for each site.</span></span>
2. <span data-ttu-id="ef0fd-122">Hozzon létre egy figyelőt a helyekhez Alkalmazásátjáró támogatja.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-122">Create a listener for each site application gateway supports.</span></span>
3. <span data-ttu-id="ef0fd-123">Minden egyes figyelő, amelynek a megfelelő háttér-hozzárendelését szabályokat létrehozni.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-123">Create rules to map each listener with the appropriate back-end.</span></span>

## <a name="requirements"></a><span data-ttu-id="ef0fd-124">Követelmények</span><span class="sxs-lookup"><span data-stu-id="ef0fd-124">Requirements</span></span>

* <span data-ttu-id="ef0fd-125">**Háttér-kiszolgálókészlet:** A háttérkiszolgálók IP-címeinek listája.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-125">**Back-end server pool:** The list of IP addresses of the back-end servers.</span></span> <span data-ttu-id="ef0fd-126">A listán szereplő IP-címeknek a virtuális hálózat alhálózatához kell tartozniuk, vagy nyilvános/virtuális IP-címnek kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-126">The IP addresses listed should either belong to the virtual network subnet or should be a public IP/VIP.</span></span> <span data-ttu-id="ef0fd-127">Teljes Tartománynevét is használható.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-127">FQDN can also be used.</span></span>
* <span data-ttu-id="ef0fd-128">**Háttér-kiszolgálókészlet beállításai:** Minden készletnek vannak beállításai, például port, protokoll vagy cookie-alapú affinitás.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-128">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="ef0fd-129">Ezek a beállítások egy adott készlethez kapcsolódnak, és a készlet minden kiszolgálójára érvényesek.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-129">These settings are tied to a pool and are applied to all servers within the pool.</span></span>
* <span data-ttu-id="ef0fd-130">**Előtérbeli port:** Az Application Gateway-en megnyitott nyilvános port.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-130">**Front-end port:** This port is the public port that is opened on the application gateway.</span></span> <span data-ttu-id="ef0fd-131">Amikor a forgalom eléri ezt a portot, a port átirányítja az egyik háttérkiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-131">Traffic hits this port, and then gets redirected to one of the back-end servers.</span></span>
* <span data-ttu-id="ef0fd-132">**Figyelő:** A figyelő egy előtérbeli porttal, egy protokollal (Http vagy Https, a kis- és a nagybetűk megkülönböztetésével) és SSL tanúsítványnévvel rendelkezik (SSL-kiszervezés konfigurálásakor).</span><span class="sxs-lookup"><span data-stu-id="ef0fd-132">**Listener:** The listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and the SSL certificate name (if configuring SSL offload).</span></span> <span data-ttu-id="ef0fd-133">A többhelyes engedélyezett alkalmazásátjárót, állomásnév és SNI mutatók is bekerülnek.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-133">For multi-site enabled application gateways, host name and SNI indicators are also added.</span></span>
* <span data-ttu-id="ef0fd-134">**Szabály:** a szabály van kötve a figyelő, a háttér-kiszolgálófiók-vermet, és határozza meg, mely a forgalom legyenek irányítva, amikor az adott figyelő találatok háttér-kiszolgálófiók készlet.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-134">**Rule:** The rule binds the listener, the back-end server pool, and defines which back-end server pool the traffic should be directed to when it hits a particular listener.</span></span> <span data-ttu-id="ef0fd-135">Szabályok feldolgozása a sorrendben, és a forgalmat a rendszer kéri az első szabály, amely megfelel a sajátlagossága figyelembe vétele függetlenül keresztül.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-135">Rules are processed in the order they are listed, and traffic will be directed via the first rule that matches regardless of specificity.</span></span> <span data-ttu-id="ef0fd-136">Például ha egy szabályt egy alapszintű figyelő és egy többhelyes figyelő mindkét ugyanazt a portot használó szabály, a szabály a többhelyes figyelőjével szerepelnie kell a szabály a vártnak megfelelően működik az alapvető figyelő ahhoz, hogy a többhelyes szabály előtt.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-136">For example, if you have a rule using a basic listener and a rule using a multi-site listener both on the same port, the rule with the multi-site listener must be listed before the rule with the basic listener in order for the multi-site rule to function as expected.</span></span> 
* <span data-ttu-id="ef0fd-137">**Tanúsítványok:** minden egyes figyelő egy egyedi tanúsítványt igényel, ebben a példában 2 figyelői többhelyes jön létre.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-137">**Certificates:** Each listener requires a unique certificate, in this example 2 listeners are created for multi-site.</span></span> <span data-ttu-id="ef0fd-138">Két .pfx-tanúsítványok és azok jelszavait kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-138">Two .pfx certificates and the passwords for them need to be created.</span></span>

## <a name="create-back-end-pools-for-each-site"></a><span data-ttu-id="ef0fd-139">Minden egyes hely esetében a háttér-címkészletek létrehozása</span><span class="sxs-lookup"><span data-stu-id="ef0fd-139">Create back-end pools for each site</span></span>

<span data-ttu-id="ef0fd-140">A háttér-készlet minden egyes hely esetében, hogy szükség van az alkalmazás átjáró támogatja, ebben az esetben 2 vannak hozható létre, egy a contoso11.com és egy fabrikam11.com.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-140">A back-end pool for each site that application gateway supports is needed, in this case 2 are be created, one for contoso11.com and one for fabrikam11.com.</span></span>

### <a name="step-1"></a><span data-ttu-id="ef0fd-141">1. lépés</span><span class="sxs-lookup"><span data-stu-id="ef0fd-141">Step 1</span></span>

<span data-ttu-id="ef0fd-142">Nyissa meg az Azure portálon (https://portal.azure.com) meglévő Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-142">Navigate to an existing application gateway in the Azure portal (https://portal.azure.com).</span></span> <span data-ttu-id="ef0fd-143">Válassza ki **háttérkészletek** kattintson **hozzáadása**</span><span class="sxs-lookup"><span data-stu-id="ef0fd-143">Select **Backend pools** and click **Add**</span></span>

![háttér-készletek hozzáadása][7]

### <a name="step-2"></a><span data-ttu-id="ef0fd-145">2. lépés</span><span class="sxs-lookup"><span data-stu-id="ef0fd-145">Step 2</span></span>

<span data-ttu-id="ef0fd-146">Írja be az adatokat a háttér-készlet **pool1**, az IP-cím vagy teljes tartománynevek hozzáadása a háttér-kiszolgálókhoz, és kattintson a **OK**</span><span class="sxs-lookup"><span data-stu-id="ef0fd-146">Fill in the information for the back-end pool **pool1**, adding the ip addresses or FQDNs for the back-end servers and click **OK**</span></span>

![háttér Készletbeállítások pool1][8]

### <a name="step-3"></a><span data-ttu-id="ef0fd-148">3. lépés</span><span class="sxs-lookup"><span data-stu-id="ef0fd-148">Step 3</span></span>

<span data-ttu-id="ef0fd-149">A háttér-készletek panelen kattintson a **Hozzáadás** hozzáadása egy további háttér címkészletet **pool2**, az IP-cím vagy teljes TARTOMÁNYNEVEK hozzáadása a háttér-kiszolgálókhoz, és kattintson a **OK**</span><span class="sxs-lookup"><span data-stu-id="ef0fd-149">On the backend-pools blade click **Add** to add an additional back-end pool **pool2**, adding the ip addresses or FQDNS for the back-end servers and click **OK**</span></span>

![háttér alkalmazáskészlet pool2 beállításai][9]

## <a name="create-listeners-for-each-back-end"></a><span data-ttu-id="ef0fd-151">Az egyes háttér-figyelők létrehozása</span><span class="sxs-lookup"><span data-stu-id="ef0fd-151">Create listeners for each back-end</span></span>

<span data-ttu-id="ef0fd-152">Az Application Gateway a HTTP 1.1-állomásfejlécek segítségével üzemeltet egynél több webhelyet ugyanarról a nyilvános IP-címről és portról.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-152">Application Gateway relies on HTTP 1.1 host headers to host more than one website on the same public IP address and port.</span></span> <span data-ttu-id="ef0fd-153">Az alapvető a portálon létrehozott figyelő nem tartalmazza ezt a tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-153">The basic listener created in the portal does not contain this property.</span></span>

### <a name="step-1"></a><span data-ttu-id="ef0fd-154">1. lépés</span><span class="sxs-lookup"><span data-stu-id="ef0fd-154">Step 1</span></span>

<span data-ttu-id="ef0fd-155">Kattintson a **figyelői** a meglévő Alkalmazásátjáró, majd kattintson a **többhelyes** hozzáadása az első figyelő.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-155">Click **Listeners** on the existing application gateway and click **Multi-site** to add the first listener.</span></span>

![figyelők áttekintése panel][1]

### <a name="step-2"></a><span data-ttu-id="ef0fd-157">2. lépés</span><span class="sxs-lookup"><span data-stu-id="ef0fd-157">Step 2</span></span>

<span data-ttu-id="ef0fd-158">Töltse ki a figyelő adatait.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-158">Fill out the information for the listener.</span></span> <span data-ttu-id="ef0fd-159">Ebben a példában SSL lezárást van konfigurálva, hozzon létre egy új elülső rétegbeli portot.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-159">In this example SSL termination is configured, create a new frontend port.</span></span> <span data-ttu-id="ef0fd-160">Az SSL-lezárást használandó PFX-tanúsítvány feltöltése.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-160">Upload the .pfx certificate to be used for SSL termination.</span></span> <span data-ttu-id="ef0fd-161">Az egyetlen különbség a panel a szabványos alapvető figyelő panel képest az állomásnevet.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-161">The only difference on this blade compared to the standard basic listener blade is the hostname.</span></span>

![figyelő tulajdonságok panelen][2]

### <a name="step-3"></a><span data-ttu-id="ef0fd-163">3. lépés</span><span class="sxs-lookup"><span data-stu-id="ef0fd-163">Step 3</span></span>

<span data-ttu-id="ef0fd-164">Kattintson a **többhelyes** , és hozzon létre egy másik figyelő, a második helyet az előző lépésben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-164">Click **Multi-site** and create another listener as described in the previous step for the second site.</span></span> <span data-ttu-id="ef0fd-165">Ügyeljen arra, hogy egy másik tanúsítványt használ a második figyelő.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-165">Make sure to use a different certificate for the second listener.</span></span> <span data-ttu-id="ef0fd-166">Az egyetlen különbség a panel a szabványos alapvető figyelő panel képest az állomásnevet.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-166">The only difference on this blade compared to the standard basic listener blade is the hostname.</span></span> <span data-ttu-id="ef0fd-167">A figyelőre, majd kattintson az adatok **OK**.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-167">Fill out the information for the listener and click **OK**.</span></span>

![figyelő tulajdonságok panelen][3]

> [!NOTE]
> <span data-ttu-id="ef0fd-169">Figyelők az Azure portálon az Alkalmazásátjáró létrehozása egy hosszú ideig futó feladatot, a hosszabb ideig tart a két figyelői ebben a forgatókönyvben is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-169">Creation of listeners in the Azure portal for application gateway is a long running task, it may take some time to create the two listeners in this scenario.</span></span> <span data-ttu-id="ef0fd-170">Ha végzett a figyelők megjelenítése a portálon, az alábbi képen látható módon:</span><span class="sxs-lookup"><span data-stu-id="ef0fd-170">When complete the listeners show in the portal as seen in the following image:</span></span>

![figyelő áttekintése][4]

## <a name="create-rules-to-map-listeners-to-backend-pools"></a><span data-ttu-id="ef0fd-172">Figyelők hozzárendelése háttérkészletek szabályok létrehozása</span><span class="sxs-lookup"><span data-stu-id="ef0fd-172">Create rules to map listeners to backend pools</span></span>

### <a name="step-1"></a><span data-ttu-id="ef0fd-173">1. lépés</span><span class="sxs-lookup"><span data-stu-id="ef0fd-173">Step 1</span></span>

<span data-ttu-id="ef0fd-174">Nyissa meg az Azure portálon (https://portal.azure.com) meglévő Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-174">Navigate to an existing application gateway in the Azure portal (https://portal.azure.com).</span></span> <span data-ttu-id="ef0fd-175">Válassza ki **szabályok** , és válassza a meglévő alapértelmezett szabály **Szabály1** kattintson **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-175">Select **Rules** and choose the existing default rule **rule1** and click **Edit**.</span></span>

### <a name="step-2"></a><span data-ttu-id="ef0fd-176">2. lépés</span><span class="sxs-lookup"><span data-stu-id="ef0fd-176">Step 2</span></span>

<span data-ttu-id="ef0fd-177">Töltse ki a szabályok panelt, az alábbi képen látható módon.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-177">Fill out the rules blade as seen in the following image.</span></span> <span data-ttu-id="ef0fd-178">A figyelő első és az első készlet kiválasztása, és kattintson a **mentése** teljes.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-178">Choosing the first listener and first pool and clicking **Save** when complete.</span></span>

![meglévő szabály szerkesztése][6]

### <a name="step-3"></a><span data-ttu-id="ef0fd-180">3. lépés</span><span class="sxs-lookup"><span data-stu-id="ef0fd-180">Step 3</span></span>

<span data-ttu-id="ef0fd-181">Kattintson a **alapvető szabály** a második szabály létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-181">Click **Basic rule** to create the second rule.</span></span> <span data-ttu-id="ef0fd-182">Töltse ki az űrlapot, a második figyelő, a második háttérkészlet, és kattintson a **OK** mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-182">Fill out the form with the second listener and second backend pool and click **OK** to save.</span></span>

![hozzáadása alapszintű szabály panel][10]

<span data-ttu-id="ef0fd-184">Ez a forgatókönyv befejezése meglévő Alkalmazásátjáró konfigurálása a többhelyes támogatás az Azure portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="ef0fd-184">This scenario completes configuring an existing application gateway with multi-site support through the Azure portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ef0fd-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ef0fd-185">Next steps</span></span>

<span data-ttu-id="ef0fd-186">Ismerje meg, hogyan védi meg a webhelyek [Application Gateway - webalkalmazási tűzfal](application-gateway-webapplicationfirewall-overview.md)</span><span class="sxs-lookup"><span data-stu-id="ef0fd-186">Learn how to protect your websites with [Application Gateway - Web Application Firewall](application-gateway-webapplicationfirewall-overview.md)</span></span>

<!--Image references-->
[1]: ./media/application-gateway-create-multisite-portal/figure1.png
[2]: ./media/application-gateway-create-multisite-portal/figure2.png
[3]: ./media/application-gateway-create-multisite-portal/figure3.png
[4]: ./media/application-gateway-create-multisite-portal/figure4.png
[5]: ./media/application-gateway-create-multisite-portal/figure5.png
[6]: ./media/application-gateway-create-multisite-portal/figure6.png
[7]: ./media/application-gateway-create-multisite-portal/figure7.png
[8]: ./media/application-gateway-create-multisite-portal/figure8.png
[9]: ./media/application-gateway-create-multisite-portal/figure9.png
[10]: ./media/application-gateway-create-multisite-portal/figure10.png
[multisite]: ./media/application-gateway-create-multisite-portal/multisite.png
