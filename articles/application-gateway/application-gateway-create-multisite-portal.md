---
title: "aaaHost Azure Application Gateway több hely |} Microsoft Docs"
description: "Ezen a lapon nyújt útmutatást tooconfigure egy meglévő Azure Alkalmazásátjáró üzemeltetéséhez a hello több webalkalmazás az Azure-portálon hello ugyanahhoz az átjáróhoz."
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
ms.openlocfilehash: 2172aa2c80720f6f1ab7dd91745b44654bcaee00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-existing-application-gateway-for-hosting-multiple-web-applications"></a><span data-ttu-id="a7c62-103">Konfiguráljon egy meglévő alkalmazás átjárót több webalkalmazás üzemeltetéséhez</span><span class="sxs-lookup"><span data-stu-id="a7c62-103">Configure an existing application gateway for hosting multiple web applications</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="a7c62-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a7c62-104">Azure portal</span></span>](application-gateway-create-multisite-portal.md)
> * [<span data-ttu-id="a7c62-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="a7c62-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-multisite-azureresourcemanager-powershell.md)
> 
> 

<span data-ttu-id="a7c62-106">Több helyet üzemeltető lehetővé teszi egy webalkalmazás több toodeploy a hello ugyanazt az Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="a7c62-106">Multiple site hosting allows you toodeploy more than one web application on hello same application gateway.</span></span> <span data-ttu-id="a7c62-107">Az állomásfejlécnek hello bejövő HTTP-kérelmek, mely figyelő kapja forgalom toodetermine jelenlétére támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="a7c62-107">It relies on presence of host header in hello incoming HTTP request, toodetermine which listener would receive traffic.</span></span> <span data-ttu-id="a7c62-108">hello figyelő majd arra utasítja a forgalom tooappropriate háttérkészlet be hello átjáró hello szabályok meghatározását.</span><span class="sxs-lookup"><span data-stu-id="a7c62-108">hello listener then directs traffic tooappropriate backend pool as configured in hello rules definition of hello gateway.</span></span> <span data-ttu-id="a7c62-109">Az SSL engedélyezve van a webalkalmazások Alkalmazásátjáró hello kiszolgálónév jelzése (SNI) bővítmény toochoose hello megfelelő figyelő hello webes forgalom támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="a7c62-109">In SSL enabled web applications, application gateway relies on hello Server Name Indication (SNI) extension toochoose hello correct listener for hello web traffic.</span></span> <span data-ttu-id="a7c62-110">Több hely üzemeltetéséhez használatos tooload-érkező kérések elosztása a különböző webes tartományok toodifferent háttér-kiszolgálófiók készletek.</span><span class="sxs-lookup"><span data-stu-id="a7c62-110">A common use for multiple site hosting is tooload balance requests for different web domains toodifferent back-end server pools.</span></span> <span data-ttu-id="a7c62-111">Hasonlóképpen a legfelső szintű tartománynak is futhat a hello több altartományt hello ugyanazt az Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="a7c62-111">Similarly multiple subdomains of hello same root domain could also be hosted on hello same application gateway.</span></span>

## <a name="scenario"></a><span data-ttu-id="a7c62-112">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="a7c62-112">Scenario</span></span>

<span data-ttu-id="a7c62-113">A következő példa hello, Alkalmazásátjáró van kiszolgáló a contoso.com és fabrikam.com forgalom a két háttér-kiszolgálófiók rendelkezik: contoso kiszolgálókészlet és a fabrikam kiszolgálókészlethez.</span><span class="sxs-lookup"><span data-stu-id="a7c62-113">In hello following example, application gateway is serving traffic for contoso.com and fabrikam.com with two back-end server pools: contoso server pool and fabrikam server pool.</span></span> <span data-ttu-id="a7c62-114">Hasonló lehet, például app.contoso.com és blog.contoso.com használt toohost altartományokat.</span><span class="sxs-lookup"><span data-stu-id="a7c62-114">Similar setup could be used toohost subdomains like app.contoso.com and blog.contoso.com.</span></span>

![többhelyes forgatókönyv][multisite]

## <a name="before-you-begin"></a><span data-ttu-id="a7c62-116">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="a7c62-116">Before you begin</span></span>

<span data-ttu-id="a7c62-117">Ebben a forgatókönyvben a többhelyes támogatás tooan meglévő Alkalmazásátjáró ad hozzá.</span><span class="sxs-lookup"><span data-stu-id="a7c62-117">This scenario adds multi-site support tooan existing application gateway.</span></span> <span data-ttu-id="a7c62-118">toocomplete ebben a forgatókönyvben egy meglévő Alkalmazásátjáró toobe elérhető tooconfigure kell.</span><span class="sxs-lookup"><span data-stu-id="a7c62-118">toocomplete this scenario, an existing application gateway needs toobe available tooconfigure.</span></span> <span data-ttu-id="a7c62-119">Látogasson el [Alkalmazásátjáró létrehozása hello portál használatával](application-gateway-create-gateway-portal.md) toolearn hogyan toocreate egy alapszintű application gateway hello portálon.</span><span class="sxs-lookup"><span data-stu-id="a7c62-119">Visit [Create an application gateway by using hello portal](application-gateway-create-gateway-portal.md) toolearn how toocreate a basic application gateway in hello portal.</span></span>

<span data-ttu-id="a7c62-120">hello az alábbiakban hello lépéseket tooupdate hello Alkalmazásátjáró szükséges:</span><span class="sxs-lookup"><span data-stu-id="a7c62-120">hello following are hello steps needed tooupdate hello application gateway:</span></span>

1. <span data-ttu-id="a7c62-121">Hozzon létre a háttér-készletek toouse minden egyes hely esetében.</span><span class="sxs-lookup"><span data-stu-id="a7c62-121">Create back-end pools toouse for each site.</span></span>
2. <span data-ttu-id="a7c62-122">Hozzon létre egy figyelőt a helyekhez Alkalmazásátjáró támogatja.</span><span class="sxs-lookup"><span data-stu-id="a7c62-122">Create a listener for each site application gateway supports.</span></span>
3. <span data-ttu-id="a7c62-123">Hozzon létre szabályokat toomap minden egyes figyelő hello megfelelő háttér.</span><span class="sxs-lookup"><span data-stu-id="a7c62-123">Create rules toomap each listener with hello appropriate back-end.</span></span>

## <a name="requirements"></a><span data-ttu-id="a7c62-124">Követelmények</span><span class="sxs-lookup"><span data-stu-id="a7c62-124">Requirements</span></span>

* <span data-ttu-id="a7c62-125">**Háttér-kiszolgálófiók készlet:** hello hello háttér-kiszolgálók IP-címek listáját.</span><span class="sxs-lookup"><span data-stu-id="a7c62-125">**Back-end server pool:** hello list of IP addresses of hello back-end servers.</span></span> <span data-ttu-id="a7c62-126">hello IP-címek felsorolt toohello virtuális hálózati alhálózat vagy kell tartoznia, vagy egy nyilvános IP-cím/VIP kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a7c62-126">hello IP addresses listed should either belong toohello virtual network subnet or should be a public IP/VIP.</span></span> <span data-ttu-id="a7c62-127">Teljes Tartománynevét is használható.</span><span class="sxs-lookup"><span data-stu-id="a7c62-127">FQDN can also be used.</span></span>
* <span data-ttu-id="a7c62-128">**Háttér-kiszolgálókészlet beállításai:** Minden készletnek vannak beállításai, például port, protokoll vagy cookie-alapú affinitás.</span><span class="sxs-lookup"><span data-stu-id="a7c62-128">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="a7c62-129">Ezek a beállítások esetén tooa kapcsolt verem és a hello készlet alkalmazott tooall-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="a7c62-129">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="a7c62-130">**Előtér-port:** Ez a port nem hello nyilvános portot, amelyet a hello Alkalmazásátjáró meg van nyitva.</span><span class="sxs-lookup"><span data-stu-id="a7c62-130">**Front-end port:** This port is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="a7c62-131">Forgalom találatok ezt a portot, és lekérdezi átirányítja tooone hello háttér-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="a7c62-131">Traffic hits this port, and then gets redirected tooone of hello back-end servers.</span></span>
* <span data-ttu-id="a7c62-132">**Figyelő:** hello figyelő rendelkezik egy előtér-portot, a protokollt (Http vagy Https, ezek az értékek kis-és nagybetűket), és hello SSL tanúsítvány neve (ha az SSL beállításának-kiszervezés).</span><span class="sxs-lookup"><span data-stu-id="a7c62-132">**Listener:** hello listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span> <span data-ttu-id="a7c62-133">A többhelyes engedélyezett alkalmazásátjárót, állomásnév és SNI mutatók is bekerülnek.</span><span class="sxs-lookup"><span data-stu-id="a7c62-133">For multi-site enabled application gateways, host name and SNI indicators are also added.</span></span>
* <span data-ttu-id="a7c62-134">**Szabály:** hello szabály van kötve hello figyelő, hello háttér-kiszolgálófiók vermet, és azt határozza meg, mely háttér-kiszolgálófiók készlet hello forgalom irányított toowhen találatok száma a egy adott figyelő.</span><span class="sxs-lookup"><span data-stu-id="a7c62-134">**Rule:** hello rule binds hello listener, hello back-end server pool, and defines which back-end server pool hello traffic should be directed toowhen it hits a particular listener.</span></span> <span data-ttu-id="a7c62-135">Szabályok feldolgozása hello sorrendben, és a forgalom hello első egyező szabály függetlenül sajátlagossága figyelembe vétele keresztül jutnak.</span><span class="sxs-lookup"><span data-stu-id="a7c62-135">Rules are processed in hello order they are listed, and traffic will be directed via hello first rule that matches regardless of specificity.</span></span> <span data-ttu-id="a7c62-136">Például ha a szabályt egy alapszintű figyelő és az azonos port, hello szabály hello többhelyes figyelő mindkét használó szabály hello többhelyes figyelő szerepelnie kell a hello szabály előtt hello alapvető figyelőjével ahhoz, hogy hello többhelyes szabály toofunction várt.</span><span class="sxs-lookup"><span data-stu-id="a7c62-136">For example, if you have a rule using a basic listener and a rule using a multi-site listener both on hello same port, hello rule with hello multi-site listener must be listed before hello rule with hello basic listener in order for hello multi-site rule toofunction as expected.</span></span> 
* <span data-ttu-id="a7c62-137">**Tanúsítványok:** minden egyes figyelő egy egyedi tanúsítványt igényel, ebben a példában 2 figyelői többhelyes jön létre.</span><span class="sxs-lookup"><span data-stu-id="a7c62-137">**Certificates:** Each listener requires a unique certificate, in this example 2 listeners are created for multi-site.</span></span> <span data-ttu-id="a7c62-138">Két .pfx-tanúsítványok és a számukra hello jelszavak kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="a7c62-138">Two .pfx certificates and hello passwords for them need toobe created.</span></span>

## <a name="create-back-end-pools-for-each-site"></a><span data-ttu-id="a7c62-139">Minden egyes hely esetében a háttér-címkészletek létrehozása</span><span class="sxs-lookup"><span data-stu-id="a7c62-139">Create back-end pools for each site</span></span>

<span data-ttu-id="a7c62-140">A háttér-készlet minden egyes hely esetében, hogy szükség van az alkalmazás átjáró támogatja, ebben az esetben 2 vannak hozható létre, egy a contoso11.com és egy fabrikam11.com.</span><span class="sxs-lookup"><span data-stu-id="a7c62-140">A back-end pool for each site that application gateway supports is needed, in this case 2 are be created, one for contoso11.com and one for fabrikam11.com.</span></span>

### <a name="step-1"></a><span data-ttu-id="a7c62-141">1. lépés</span><span class="sxs-lookup"><span data-stu-id="a7c62-141">Step 1</span></span>

<span data-ttu-id="a7c62-142">Keresse meg a meglévő Alkalmazásátjáró tooan a hello Azure portal (https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a7c62-142">Navigate tooan existing application gateway in hello Azure portal (https://portal.azure.com).</span></span> <span data-ttu-id="a7c62-143">Válassza ki **háttérkészletek** kattintson **hozzáadása**</span><span class="sxs-lookup"><span data-stu-id="a7c62-143">Select **Backend pools** and click **Add**</span></span>

![háttér-készletek hozzáadása][7]

### <a name="step-2"></a><span data-ttu-id="a7c62-145">2. lépés</span><span class="sxs-lookup"><span data-stu-id="a7c62-145">Step 2</span></span>

<span data-ttu-id="a7c62-146">Töltse ki hello információkat hello háttér-készlet **pool1**, hello IP-cím vagy teljes tartománynevek hozzáadása hello háttér-kiszolgálókhoz, és kattintson a **OK**</span><span class="sxs-lookup"><span data-stu-id="a7c62-146">Fill in hello information for hello back-end pool **pool1**, adding hello ip addresses or FQDNs for hello back-end servers and click **OK**</span></span>

![háttér Készletbeállítások pool1][8]

### <a name="step-3"></a><span data-ttu-id="a7c62-148">3. lépés</span><span class="sxs-lookup"><span data-stu-id="a7c62-148">Step 3</span></span>

<span data-ttu-id="a7c62-149">Hello háttérkészletek panelen kattintson a **Hozzáadás** tooadd egy további háttér címkészletet **pool2**, hello IP-cím vagy teljes TARTOMÁNYNEVEK hozzáadása hello háttér-kiszolgálókhoz, és kattintson a **OK**</span><span class="sxs-lookup"><span data-stu-id="a7c62-149">On hello backend-pools blade click **Add** tooadd an additional back-end pool **pool2**, adding hello ip addresses or FQDNS for hello back-end servers and click **OK**</span></span>

![háttér alkalmazáskészlet pool2 beállításai][9]

## <a name="create-listeners-for-each-back-end"></a><span data-ttu-id="a7c62-151">Az egyes háttér-figyelők létrehozása</span><span class="sxs-lookup"><span data-stu-id="a7c62-151">Create listeners for each back-end</span></span>

<span data-ttu-id="a7c62-152">Alkalmazásátjáró támaszkodik a HTTP 1.1 egy webhely több állomás fejlécek toohost hello azonos nyilvános IP-cím és port.</span><span class="sxs-lookup"><span data-stu-id="a7c62-152">Application Gateway relies on HTTP 1.1 host headers toohost more than one website on hello same public IP address and port.</span></span> <span data-ttu-id="a7c62-153">hello alapvető figyelő hello portálon létrehozott nem tartalmazza ezt a tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="a7c62-153">hello basic listener created in hello portal does not contain this property.</span></span>

### <a name="step-1"></a><span data-ttu-id="a7c62-154">1. lépés</span><span class="sxs-lookup"><span data-stu-id="a7c62-154">Step 1</span></span>

<span data-ttu-id="a7c62-155">Kattintson a **figyelői** a meglévő Alkalmazásátjáró hello, és kattintson a **többhelyes** tooadd hello első figyelő.</span><span class="sxs-lookup"><span data-stu-id="a7c62-155">Click **Listeners** on hello existing application gateway and click **Multi-site** tooadd hello first listener.</span></span>

![figyelők áttekintése panel][1]

### <a name="step-2"></a><span data-ttu-id="a7c62-157">2. lépés</span><span class="sxs-lookup"><span data-stu-id="a7c62-157">Step 2</span></span>

<span data-ttu-id="a7c62-158">Töltse ki a hello hello figyelő adatait.</span><span class="sxs-lookup"><span data-stu-id="a7c62-158">Fill out hello information for hello listener.</span></span> <span data-ttu-id="a7c62-159">Ebben a példában SSL lezárást van konfigurálva, hozzon létre egy új elülső rétegbeli portot.</span><span class="sxs-lookup"><span data-stu-id="a7c62-159">In this example SSL termination is configured, create a new frontend port.</span></span> <span data-ttu-id="a7c62-160">Hello .pfx tanúsítvány toobe használt SSL-lezárást feltöltése.</span><span class="sxs-lookup"><span data-stu-id="a7c62-160">Upload hello .pfx certificate toobe used for SSL termination.</span></span> <span data-ttu-id="a7c62-161">hello csak a standard alapvető figyelő panel képest panel toohello különbség hello állomásnevet.</span><span class="sxs-lookup"><span data-stu-id="a7c62-161">hello only difference on this blade compared toohello standard basic listener blade is hello hostname.</span></span>

![figyelő tulajdonságok panelen][2]

### <a name="step-3"></a><span data-ttu-id="a7c62-163">3. lépés</span><span class="sxs-lookup"><span data-stu-id="a7c62-163">Step 3</span></span>

<span data-ttu-id="a7c62-164">Kattintson a **többhelyes** , és hozzon létre egy másik figyelő hello második hely hello előző lépésben leírt módon.</span><span class="sxs-lookup"><span data-stu-id="a7c62-164">Click **Multi-site** and create another listener as described in hello previous step for hello second site.</span></span> <span data-ttu-id="a7c62-165">Győződjön meg arról, hogy toouse hello második figyelő különböző tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="a7c62-165">Make sure toouse a different certificate for hello second listener.</span></span> <span data-ttu-id="a7c62-166">hello csak a standard alapvető figyelő panel képest panel toohello különbség hello állomásnevet.</span><span class="sxs-lookup"><span data-stu-id="a7c62-166">hello only difference on this blade compared toohello standard basic listener blade is hello hostname.</span></span> <span data-ttu-id="a7c62-167">Hello adatok hello figyelő, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="a7c62-167">Fill out hello information for hello listener and click **OK**.</span></span>

![figyelő tulajdonságok panelen][3]

> [!NOTE]
> <span data-ttu-id="a7c62-169">Egy hosszú ideig futó feladat létrehozása az Azure-portálon az Alkalmazásátjáró hello figyelők, eltarthat néhány alkalommal toocreate hello két figyelői ebben a forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="a7c62-169">Creation of listeners in hello Azure portal for application gateway is a long running task, it may take some time toocreate hello two listeners in this scenario.</span></span> <span data-ttu-id="a7c62-170">Ha teljes hello figyelők megjelenítése hello portálon hello kép a következő látható:</span><span class="sxs-lookup"><span data-stu-id="a7c62-170">When complete hello listeners show in hello portal as seen in hello following image:</span></span>

![figyelő áttekintése][4]

## <a name="create-rules-toomap-listeners-toobackend-pools"></a><span data-ttu-id="a7c62-172">Szabályok toomap figyelői toobackend-címkészletek létrehozása</span><span class="sxs-lookup"><span data-stu-id="a7c62-172">Create rules toomap listeners toobackend pools</span></span>

### <a name="step-1"></a><span data-ttu-id="a7c62-173">1. lépés</span><span class="sxs-lookup"><span data-stu-id="a7c62-173">Step 1</span></span>

<span data-ttu-id="a7c62-174">Keresse meg a meglévő Alkalmazásátjáró tooan a hello Azure portal (https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a7c62-174">Navigate tooan existing application gateway in hello Azure portal (https://portal.azure.com).</span></span> <span data-ttu-id="a7c62-175">Válassza ki **szabályok** és hello meglévő alapértelmezett szabály választása **Szabály1** kattintson **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="a7c62-175">Select **Rules** and choose hello existing default rule **rule1** and click **Edit**.</span></span>

### <a name="step-2"></a><span data-ttu-id="a7c62-176">2. lépés</span><span class="sxs-lookup"><span data-stu-id="a7c62-176">Step 2</span></span>

<span data-ttu-id="a7c62-177">Töltse ki a hello szabályok panelen a következő kép hello látható módon.</span><span class="sxs-lookup"><span data-stu-id="a7c62-177">Fill out hello rules blade as seen in hello following image.</span></span> <span data-ttu-id="a7c62-178">Hello első figyelő és első készlet kiválasztása, és kattintson a **mentése** teljes.</span><span class="sxs-lookup"><span data-stu-id="a7c62-178">Choosing hello first listener and first pool and clicking **Save** when complete.</span></span>

![meglévő szabály szerkesztése][6]

### <a name="step-3"></a><span data-ttu-id="a7c62-180">3. lépés</span><span class="sxs-lookup"><span data-stu-id="a7c62-180">Step 3</span></span>

<span data-ttu-id="a7c62-181">Kattintson a **alapvető szabály** toocreate hello második szabály.</span><span class="sxs-lookup"><span data-stu-id="a7c62-181">Click **Basic rule** toocreate hello second rule.</span></span> <span data-ttu-id="a7c62-182">Töltse ki a hello második figyelő, a második háttérkészlet hello űrlapot, és kattintson a **OK** toosave.</span><span class="sxs-lookup"><span data-stu-id="a7c62-182">Fill out hello form with hello second listener and second backend pool and click **OK** toosave.</span></span>

![hozzáadása alapszintű szabály panel][10]

<span data-ttu-id="a7c62-184">Ez a forgatókönyv befejezése meglévő Alkalmazásátjáró konfigurálása a többhelyes támogatás hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="a7c62-184">This scenario completes configuring an existing application gateway with multi-site support through hello Azure portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a7c62-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a7c62-185">Next steps</span></span>

<span data-ttu-id="a7c62-186">Megtudhatja, hogyan tooprotect a webhelyek [Application Gateway - webalkalmazási tűzfal](application-gateway-webapplicationfirewall-overview.md)</span><span class="sxs-lookup"><span data-stu-id="a7c62-186">Learn how tooprotect your websites with [Application Gateway - Web Application Firewall](application-gateway-webapplicationfirewall-overview.md)</span></span>

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
