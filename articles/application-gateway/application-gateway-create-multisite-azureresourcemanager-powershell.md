---
title: "több hely üzemeltetéséhez Alkalmazásátjáró aaaCreate |} Microsoft Docs"
description: "Ezen a lapon nyújt útmutatást toocreate, konfiguráljon egy Azure-alkalmazásokban átjárót a hello több webalkalmazás üzemeltetéséhez ugyanahhoz az átjáróhoz."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: b107d647-c9be-499f-8b55-809c4310c783
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/12/2016
ms.author: amsriva
ms.openlocfilehash: bad9a76be0a73a7026a770630fa7156f6e5940c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-for-hosting-multiple-web-applications"></a><span data-ttu-id="2de48-103">Hozzon létre egy alkalmazás több webalkalmazás üzemeltetéséhez</span><span class="sxs-lookup"><span data-stu-id="2de48-103">Create an application gateway for hosting multiple web applications</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="2de48-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2de48-104">Azure portal</span></span>](application-gateway-create-multisite-portal.md)
> * [<span data-ttu-id="2de48-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="2de48-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-multisite-azureresourcemanager-powershell.md)

<span data-ttu-id="2de48-106">Több helyet üzemeltető lehetővé teszi egy webalkalmazás több toodeploy a hello ugyanazt az Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="2de48-106">Multiple site hosting allows you toodeploy more than one web application on hello same application gateway.</span></span> <span data-ttu-id="2de48-107">Az állomásfejlécnek hello bejövő HTTP-kérelmek, mely figyelő kapja forgalom toodetermine jelenlétére támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="2de48-107">It relies on presence of host header in hello incoming HTTP request, toodetermine which listener would receive traffic.</span></span> <span data-ttu-id="2de48-108">hello figyelő majd arra utasítja a forgalom tooappropriate háttérkészlet be hello átjáró hello szabályok meghatározását.</span><span class="sxs-lookup"><span data-stu-id="2de48-108">hello listener then directs traffic tooappropriate backend pool as configured in hello rules definition of hello gateway.</span></span> <span data-ttu-id="2de48-109">Az SSL engedélyezve van a webalkalmazások Alkalmazásátjáró hello kiszolgálónév jelzése (SNI) bővítmény toochoose hello megfelelő figyelő hello webes forgalom támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="2de48-109">In SSL enabled web applications, application gateway relies on hello Server Name Indication (SNI) extension toochoose hello correct listener for hello web traffic.</span></span> <span data-ttu-id="2de48-110">Több hely üzemeltetéséhez használatos tooload-érkező kérések elosztása a különböző webes tartományok toodifferent háttér-kiszolgálófiók készletek.</span><span class="sxs-lookup"><span data-stu-id="2de48-110">A common use for multiple site hosting is tooload balance requests for different web domains toodifferent back-end server pools.</span></span> <span data-ttu-id="2de48-111">Hasonlóképpen a legfelső szintű tartománynak is futhat a hello több altartományt hello ugyanazt az Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="2de48-111">Similarly multiple subdomains of hello same root domain could also be hosted on hello same application gateway.</span></span>

## <a name="scenario"></a><span data-ttu-id="2de48-112">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="2de48-112">Scenario</span></span>

<span data-ttu-id="2de48-113">A következő példa hello, Alkalmazásátjáró van kiszolgáló a contoso.com és fabrikam.com forgalom a két háttér-kiszolgálófiók rendelkezik: contoso kiszolgálókészlet és a fabrikam kiszolgálókészlethez.</span><span class="sxs-lookup"><span data-stu-id="2de48-113">In hello following example, application gateway is serving traffic for contoso.com and fabrikam.com with two back-end server pools: contoso server pool and fabrikam server pool.</span></span> <span data-ttu-id="2de48-114">Hasonló lehet, például app.contoso.com és blog.contoso.com használt toohost altartományokat.</span><span class="sxs-lookup"><span data-stu-id="2de48-114">Similar setup could be used toohost subdomains like app.contoso.com and blog.contoso.com.</span></span>

![imageURLroute](./media/application-gateway-create-multisite-azureresourcemanager-powershell/multisite.png)

## <a name="before-you-begin"></a><span data-ttu-id="2de48-116">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="2de48-116">Before you begin</span></span>

1. <span data-ttu-id="2de48-117">Webplatform-telepítő hello segítségével hello hello Azure PowerShell-parancsmagok legújabb verzióját telepítse.</span><span class="sxs-lookup"><span data-stu-id="2de48-117">Install hello latest version of hello Azure PowerShell cmdlets by using hello Web Platform Installer.</span></span> <span data-ttu-id="2de48-118">Töltse le, és telepítse hello legújabb verziót a hello **Windows PowerShell** hello szakasza [letöltési oldalon](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="2de48-118">You can download and install hello latest version from hello **Windows PowerShell** section of hello [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="2de48-119">hello kiszolgálók toohello háttér-készlet toouse hello Alkalmazásátjáró kell lennie, különben a végpontokat hozott létre vagy hello virtuális hálózatban egy külön alhálózatot, vagy egy nyilvános IP-cím/VIP rendelt hozzá.</span><span class="sxs-lookup"><span data-stu-id="2de48-119">hello servers added toohello back-end pool toouse hello application gateway must exist or have their endpoints created either in hello virtual network in a separate subnet or with a public IP/VIP assigned.</span></span>

## <a name="requirements"></a><span data-ttu-id="2de48-120">Követelmények</span><span class="sxs-lookup"><span data-stu-id="2de48-120">Requirements</span></span>

* <span data-ttu-id="2de48-121">**Háttér-kiszolgálófiók készlet:** hello hello háttér-kiszolgálók IP-címek listáját.</span><span class="sxs-lookup"><span data-stu-id="2de48-121">**Back-end server pool:** hello list of IP addresses of hello back-end servers.</span></span> <span data-ttu-id="2de48-122">hello IP-címek felsorolt toohello virtuális hálózati alhálózat vagy kell tartoznia, vagy egy nyilvános IP-cím/VIP kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2de48-122">hello IP addresses listed should either belong toohello virtual network subnet or should be a public IP/VIP.</span></span> <span data-ttu-id="2de48-123">Teljes Tartománynevét is használható.</span><span class="sxs-lookup"><span data-stu-id="2de48-123">FQDN can also be used.</span></span>
* <span data-ttu-id="2de48-124">**Háttér-kiszolgálókészlet beállításai:** Minden készletnek vannak beállításai, például port, protokoll vagy cookie-alapú affinitás.</span><span class="sxs-lookup"><span data-stu-id="2de48-124">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="2de48-125">Ezek a beállítások esetén tooa kapcsolt verem és a hello készlet alkalmazott tooall-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="2de48-125">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="2de48-126">**Előtér-port:** Ez a port nem hello nyilvános portot, amelyet a hello Alkalmazásátjáró meg van nyitva.</span><span class="sxs-lookup"><span data-stu-id="2de48-126">**Front-end port:** This port is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="2de48-127">Forgalom találatok ezt a portot, és lekérdezi átirányítja tooone hello háttér-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="2de48-127">Traffic hits this port, and then gets redirected tooone of hello back-end servers.</span></span>
* <span data-ttu-id="2de48-128">**Figyelő:** hello figyelő rendelkezik egy előtér-portot, a protokollt (Http vagy Https, ezek az értékek kis-és nagybetűket), és hello SSL tanúsítvány neve (ha az SSL beállításának-kiszervezés).</span><span class="sxs-lookup"><span data-stu-id="2de48-128">**Listener:** hello listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span> <span data-ttu-id="2de48-129">A többhelyes engedélyezett alkalmazásátjárót, állomásnév és SNI mutatók is bekerülnek.</span><span class="sxs-lookup"><span data-stu-id="2de48-129">For multi-site enabled application gateways, host name and SNI indicators are also added.</span></span>
* <span data-ttu-id="2de48-130">**Szabály:** hello szabály van kötve hello figyelő, hello háttér-kiszolgálófiók vermet, és azt határozza meg, mely háttér-kiszolgálófiók készlet hello forgalom irányított toowhen találatok száma a egy adott figyelő.</span><span class="sxs-lookup"><span data-stu-id="2de48-130">**Rule:** hello rule binds hello listener, hello back-end server pool, and defines which back-end server pool hello traffic should be directed toowhen it hits a particular listener.</span></span> <span data-ttu-id="2de48-131">Szabályok feldolgozása hello sorrendben, és a forgalom hello első egyező szabály függetlenül sajátlagossága figyelembe vétele keresztül jutnak.</span><span class="sxs-lookup"><span data-stu-id="2de48-131">Rules are processed in hello order they are listed, and traffic will be directed via hello first rule that matches regardless of specificity.</span></span> <span data-ttu-id="2de48-132">Például ha a szabályt egy alapszintű figyelő és az azonos port, hello szabály hello többhelyes figyelő mindkét használó szabály hello többhelyes figyelő szerepelnie kell a hello szabály előtt hello alapvető figyelőjével ahhoz, hogy hello többhelyes szabály toofunction várt.</span><span class="sxs-lookup"><span data-stu-id="2de48-132">For example, if you have a rule using a basic listener and a rule using a multi-site listener both on hello same port, hello rule with hello multi-site listener must be listed before hello rule with hello basic listener in order for hello multi-site rule toofunction as expected.</span></span>

## <a name="create-an-application-gateway"></a><span data-ttu-id="2de48-133">Application Gateway létrehozása</span><span class="sxs-lookup"><span data-stu-id="2de48-133">Create an application gateway</span></span>

<span data-ttu-id="2de48-134">hello az alábbiakban hello lépéseket toocreate Alkalmazásátjáró szükséges:</span><span class="sxs-lookup"><span data-stu-id="2de48-134">hello following are hello steps needed toocreate an application gateway:</span></span>

1. <span data-ttu-id="2de48-135">Egy erőforráscsoport létrehozása a Resource Manager számára.</span><span class="sxs-lookup"><span data-stu-id="2de48-135">Create a resource group for Resource Manager.</span></span>
2. <span data-ttu-id="2de48-136">Hozzon létre egy virtuális hálózatot, alhálózatok és hello alkalmazás átjáró nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="2de48-136">Create a virtual network, subnets, and public IP for hello application gateway.</span></span>
3. <span data-ttu-id="2de48-137">Egy Application Gateway konfigurációs objektum létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2de48-137">Create an application gateway configuration object.</span></span>
4. <span data-ttu-id="2de48-138">Egy Application Gateway erőforrás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2de48-138">Create an application gateway resource.</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="2de48-139">Erőforráscsoport létrehozása a Resource Managerhez</span><span class="sxs-lookup"><span data-stu-id="2de48-139">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="2de48-140">Győződjön meg arról, hogy azok be hello Azure PowerShell legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="2de48-140">Make sure that you are using hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="2de48-141">További információt [Windows PowerShell használata a Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="2de48-141">More information is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="2de48-142">1. lépés</span><span class="sxs-lookup"><span data-stu-id="2de48-142">Step 1</span></span>

<span data-ttu-id="2de48-143">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="2de48-143">Log in tooAzure</span></span>

```powershell
Login-AzureRmAccount
```
<span data-ttu-id="2de48-144">A hitelesítő adataival felszólító tooauthenticate áll.</span><span class="sxs-lookup"><span data-stu-id="2de48-144">You are prompted tooauthenticate with your credentials.</span></span>

### <a name="step-2"></a><span data-ttu-id="2de48-145">2. lépés</span><span class="sxs-lookup"><span data-stu-id="2de48-145">Step 2</span></span>

<span data-ttu-id="2de48-146">Hello előfizetések hello fiók ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="2de48-146">Check hello subscriptions for hello account.</span></span>

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a><span data-ttu-id="2de48-147">3. lépés</span><span class="sxs-lookup"><span data-stu-id="2de48-147">Step 3</span></span>

<span data-ttu-id="2de48-148">Válassza ki, amely az Azure-előfizetések toouse.</span><span class="sxs-lookup"><span data-stu-id="2de48-148">Choose which of your Azure subscriptions toouse.</span></span>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a><span data-ttu-id="2de48-149">4. lépés</span><span class="sxs-lookup"><span data-stu-id="2de48-149">Step 4</span></span>

<span data-ttu-id="2de48-150">Hozzon létre egy erőforráscsoportot (hagyja ki ezt a lépést, ha egy meglévő erőforráscsoportot használ).</span><span class="sxs-lookup"><span data-stu-id="2de48-150">Create a resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
New-AzureRmResourceGroup -Name appgw-RG -location "West US"
```

<span data-ttu-id="2de48-151">Másik lehetőségként az Alkalmazásátjáró erőforráscsoport címkéket is létrehozhat:</span><span class="sxs-lookup"><span data-stu-id="2de48-151">Alternatively you can also create tags for a resource group for application gateway:</span></span>

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway multiple site"}
```

<span data-ttu-id="2de48-152">Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet.</span><span class="sxs-lookup"><span data-stu-id="2de48-152">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="2de48-153">Ezen a helyen az erőforráscsoport erőforrások lesz hello alapértelmezett helye.</span><span class="sxs-lookup"><span data-stu-id="2de48-153">This location is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="2de48-154">Győződjön meg arról, hogy az összes parancsok toocreate egy alkalmazás átjáró használata hello ugyanabban az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="2de48-154">Make sure that all commands toocreate an application gateway use hello same resource group.</span></span>

<span data-ttu-id="2de48-155">A hello a fenti példában létrehozott nevű erőforráscsoport **appgw-RG** a hellyel rendelkező **USA nyugati régiója**.</span><span class="sxs-lookup"><span data-stu-id="2de48-155">In hello example above, we created a resource group called **appgw-RG** with a location of **West US**.</span></span>

> [!NOTE]
> <span data-ttu-id="2de48-156">Ha egy egyéni mintavételt tooconfigure az Alkalmazásátjáró van szüksége, tekintse meg [PowerShell használatával hozzon létre olyan átjárót egyéni mintavételt](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="2de48-156">If you need tooconfigure a custom probe for your application gateway, see [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md).</span></span> <span data-ttu-id="2de48-157">Látogasson el [egyéni mintavételt és az állapotfigyelés](application-gateway-probe-overview.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="2de48-157">Visit [custom probes and health monitoring](application-gateway-probe-overview.md) for more information.</span></span>

## <a name="create-a-virtual-network-and-subnets"></a><span data-ttu-id="2de48-158">Hozzon létre egy virtuális hálózatot és alhálózatok</span><span class="sxs-lookup"><span data-stu-id="2de48-158">Create a virtual network and subnets</span></span>

<span data-ttu-id="2de48-159">a következő példa azt mutatja meg hogyan hello toocreate erőforrás-kezelő használatával egy virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="2de48-159">hello following example shows how toocreate a virtual network by using Resource Manager.</span></span> <span data-ttu-id="2de48-160">Két alhálózat ebben a lépésben jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="2de48-160">Two subnets are created in this step.</span></span> <span data-ttu-id="2de48-161">hello első alhálózat van az hello Alkalmazásátjáró magát.</span><span class="sxs-lookup"><span data-stu-id="2de48-161">hello first subnet is for hello application gateway itself.</span></span> <span data-ttu-id="2de48-162">Alkalmazásátjáró saját alhálózati toohold a példányok van szükség.</span><span class="sxs-lookup"><span data-stu-id="2de48-162">Application gateway requires its own subnet toohold its instances.</span></span> <span data-ttu-id="2de48-163">Csak más alkalmazásátjárót alhálózat is telepíthető.</span><span class="sxs-lookup"><span data-stu-id="2de48-163">Only other application gateways can be deployed in that subnet.</span></span> <span data-ttu-id="2de48-164">hello második alhálózat használt toohold hello alkalmazás háttérkiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="2de48-164">hello second subnet is used toohold hello application backend servers.</span></span>

### <a name="step-1"></a><span data-ttu-id="2de48-165">1. lépés</span><span class="sxs-lookup"><span data-stu-id="2de48-165">Step 1</span></span>

<span data-ttu-id="2de48-166">Rendelje hozzá a hello cím tartomány 10.0.0.0/24 toohello alhálózati változó toobe használt toohold hello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="2de48-166">Assign hello address range 10.0.0.0/24 toohello subnet variable toobe used toohold hello application gateway.</span></span>

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -AddressPrefix 10.0.0.0/24
```
### <a name="step-2"></a><span data-ttu-id="2de48-167">2. lépés</span><span class="sxs-lookup"><span data-stu-id="2de48-167">Step 2</span></span>

<span data-ttu-id="2de48-168">Rendelje hozzá a hello cím tartomány 10.0.1.0/24 toohello Alhalozat_2 változó toobe hello háttérkészletek használatos.</span><span class="sxs-lookup"><span data-stu-id="2de48-168">Assign hello address range 10.0.1.0/24 toohello subnet2 variable toobe used for hello backend pools.</span></span>

```powershell
$subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -AddressPrefix 10.0.1.0/24
```

### <a name="step-3"></a><span data-ttu-id="2de48-169">3. lépés</span><span class="sxs-lookup"><span data-stu-id="2de48-169">Step 3</span></span>

<span data-ttu-id="2de48-170">Hozzon létre egy virtuális hálózatot nevű **appgwvnet** erőforráscsoportban **appgw-rg** hello USA nyugati régiójában hello előtag 10.0.0.0/16 használata a 10.0.0.0/24 alhálózat, és 10.0.1.0/24.</span><span class="sxs-lookup"><span data-stu-id="2de48-170">Create a virtual network named **appgwvnet** in resource group **appgw-rg** for hello West US region using hello prefix 10.0.0.0/16 with subnet 10.0.0.0/24, and 10.0.1.0/24.</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet,$subnet2
```

### <a name="step-4"></a><span data-ttu-id="2de48-171">4. lépés</span><span class="sxs-lookup"><span data-stu-id="2de48-171">Step 4</span></span>

<span data-ttu-id="2de48-172">Rendeljen hozzá egy alhálózati változóval hello további lépéseket, ami olyan átjárót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2de48-172">Assign a subnet variable for hello next steps, which creates an application gateway.</span></span>

```powershell
$appgatewaysubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -VirtualNetwork $vnet
$backendsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -VirtualNetwork $vnet
```

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a><span data-ttu-id="2de48-173">A nyilvános IP-cím hello előtér-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="2de48-173">Create a public IP address for hello front-end configuration</span></span>

<span data-ttu-id="2de48-174">Hozzon létre egy nyilvános IP-erőforrás **publicIP01** erőforráscsoportban **appgw-rg** hello USA nyugati régiójában.</span><span class="sxs-lookup"><span data-stu-id="2de48-174">Create a public IP resource **publicIP01** in resource group **appgw-rg** for hello West US region.</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

<span data-ttu-id="2de48-175">IP-cím hozzá van rendelve toohello Alkalmazásátjáró hello szolgáltatás elindulásakor.</span><span class="sxs-lookup"><span data-stu-id="2de48-175">An IP address is assigned toohello application gateway when hello service starts.</span></span>

## <a name="create-application-gateway-configuration"></a><span data-ttu-id="2de48-176">Alkalmazás átjáró-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="2de48-176">Create application gateway configuration</span></span>

<span data-ttu-id="2de48-177">Összes konfigurációs elemek mentése tooset hello Alkalmazásátjáró létrehozása előtt van.</span><span class="sxs-lookup"><span data-stu-id="2de48-177">You have tooset up all configuration items before creating hello application gateway.</span></span> <span data-ttu-id="2de48-178">hello lépések hello konfigurációs elemek létrehozását, amelyek szükségesek az alkalmazás átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="2de48-178">hello following steps create hello configuration items that are needed for an application gateway resource.</span></span>

### <a name="step-1"></a><span data-ttu-id="2de48-179">1. lépés</span><span class="sxs-lookup"><span data-stu-id="2de48-179">Step 1</span></span>

<span data-ttu-id="2de48-180">Hozzon létre egy **gatewayIP01** nevű Application Gateway IP-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="2de48-180">Create an application gateway IP configuration named **gatewayIP01**.</span></span> <span data-ttu-id="2de48-181">Alkalmazásátjáró indításakor szerzi be hello alhálózati konfigurált IP-címeit, és útvonal-hello háttér IP-készletet a hálózati forgalom toohello IP-címek.</span><span class="sxs-lookup"><span data-stu-id="2de48-181">When application gateway starts, it picks up an IP address from hello subnet configured and route network traffic toohello IP addresses in hello back-end IP pool.</span></span> <span data-ttu-id="2de48-182">Ne feledje, hogy minden példány egy IP-címet vesz fel.</span><span class="sxs-lookup"><span data-stu-id="2de48-182">Keep in mind that each instance takes one IP address.</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $appgatewaysubnet
```

### <a name="step-2"></a><span data-ttu-id="2de48-183">2. lépés</span><span class="sxs-lookup"><span data-stu-id="2de48-183">Step 2</span></span>

<span data-ttu-id="2de48-184">Konfigurálja hello háttér IP-címkészletet nevű **pool01** és **pool2** IP-címekkel rendelkező **134.170.185.46**, **134.170.188.221**, **134.170.185.50** a **pool1** és **134.170.186.46**, **134.170.189.221**, **134.170.186.50**  a **pool2**.</span><span class="sxs-lookup"><span data-stu-id="2de48-184">Configure hello back-end IP address pool named **pool01** and **pool2** with IP addresses **134.170.185.46**, **134.170.188.221**, **134.170.185.50** for **pool1** and **134.170.186.46**, **134.170.189.221**, **134.170.186.50** for **pool2**.</span></span>

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.0.1.100, 10.0.1.101, 10.0.1.102
$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 10.0.1.103, 10.0.1.104, 10.0.1.105
```

<span data-ttu-id="2de48-185">Ebben a példában a rendszer két háttér-készletek tooroute hálózati forgalom hello kért hely alapján.</span><span class="sxs-lookup"><span data-stu-id="2de48-185">In this example, there are two back-end pools tooroute network traffic based on hello requested site.</span></span> <span data-ttu-id="2de48-186">Egy készlet a "contoso.com" hely származó forgalmat megkapja és egyéb készlet hely "fabrikam.com" származó forgalmat megkapja.</span><span class="sxs-lookup"><span data-stu-id="2de48-186">One pool receives traffic from site "contoso.com" and other pool receives traffic from site "fabrikam.com".</span></span> <span data-ttu-id="2de48-187">IP-címek tooadd saját alkalmazás IP-cím végpontok megelőző tooreplace hello rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2de48-187">You have tooreplace hello preceding IP addresses tooadd your own application IP address endpoints.</span></span> <span data-ttu-id="2de48-188">Belső IP-címek, helyett is használhatja nyilvános IP-címek, FQDN vagy a virtuális gép hálózati háttér-példányok.</span><span class="sxs-lookup"><span data-stu-id="2de48-188">In place of internal IP addresses, you could also use public IP addresses, FQDN, or a VM's NIC for backend instances.</span></span> <span data-ttu-id="2de48-189">PowerShell használt teljes tartománynevek helyett IP-címek toospecify "-BackendFQDNs" paraméter.</span><span class="sxs-lookup"><span data-stu-id="2de48-189">toospecify FQDNs instead of IPs in PowerShell use "-BackendFQDNs" parameter.</span></span>

### <a name="step-3"></a><span data-ttu-id="2de48-190">3. lépés</span><span class="sxs-lookup"><span data-stu-id="2de48-190">Step 3</span></span>

<span data-ttu-id="2de48-191">Alkalmazásbeállítás átjáró konfigurálása **poolsetting01** és **poolsetting02** hello terhelésű hálózati forgalmára hello háttér-készlet.</span><span class="sxs-lookup"><span data-stu-id="2de48-191">Configure application gateway setting **poolsetting01** and **poolsetting02** for hello load-balanced network traffic in hello back-end pool.</span></span> <span data-ttu-id="2de48-192">Ebben a példában ehhez beállításokat lehet megadni másik háttér címkészletet hello háttér-készletek.</span><span class="sxs-lookup"><span data-stu-id="2de48-192">In this example, you configure different back-end pool settings for hello back-end pools.</span></span> <span data-ttu-id="2de48-193">Mindegyik háttérkészlet saját háttérkészlet-beállításokkal rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="2de48-193">Each back-end pool can have its own back-end pool setting.</span></span>

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120
$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a><span data-ttu-id="2de48-194">4. lépés</span><span class="sxs-lookup"><span data-stu-id="2de48-194">Step 4</span></span>

<span data-ttu-id="2de48-195">Nyilvános IP-végponton hello előtér-IP konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="2de48-195">Configure hello front-end IP with public IP endpoint.</span></span>

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a><span data-ttu-id="2de48-196">5. lépés</span><span class="sxs-lookup"><span data-stu-id="2de48-196">Step 5</span></span>

<span data-ttu-id="2de48-197">Hello előtér-port konfigurálása az Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="2de48-197">Configure hello front-end port for an application gateway.</span></span>

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 443
```

### <a name="step-6"></a><span data-ttu-id="2de48-198">6. lépés</span><span class="sxs-lookup"><span data-stu-id="2de48-198">Step 6</span></span>

<span data-ttu-id="2de48-199">Két SSL-tanúsítványok konfigurálása ebben a példában toosupport fogjuk hello két webhelyekhez.</span><span class="sxs-lookup"><span data-stu-id="2de48-199">Configure two SSL certificates for hello two websites we are going toosupport in this example.</span></span> <span data-ttu-id="2de48-200">Egy tanúsítvány a contoso.com forgalmat pedig más hello fabrikam.com forgalom.</span><span class="sxs-lookup"><span data-stu-id="2de48-200">One certificate is for contoso.com traffic and hello other is for fabrikam.com traffic.</span></span> <span data-ttu-id="2de48-201">Ezeket a tanúsítványokat a webhelyeknek a tanúsítványokat kiadó hitelesítésszolgáltató kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2de48-201">These certificates should be a Certificate Authority issued certificates for your websites.</span></span> <span data-ttu-id="2de48-202">Önaláírt tanúsítványok támogatottak, de nem javasolt éles forgalom.</span><span class="sxs-lookup"><span data-stu-id="2de48-202">Self-signed certificates are supported but not recommended for production traffic.</span></span>

```powershell
$cert01 = New-AzureRmApplicationGatewaySslCertificate -Name contosocert -CertificateFile <file path> -Password <password>
$cert02 = New-AzureRmApplicationGatewaySslCertificate -Name fabrikamcert -CertificateFile <file path> -Password <password>
```

### <a name="step-7"></a><span data-ttu-id="2de48-203">7. lépés</span><span class="sxs-lookup"><span data-stu-id="2de48-203">Step 7</span></span>

<span data-ttu-id="2de48-204">Ebben a példában két figyelői hello két webhelyek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2de48-204">Configure two listeners for hello two web sites in this example.</span></span> <span data-ttu-id="2de48-205">Ez a lépés konfigurál hello figyelői nyilvános IP-cím, port és gazdagép használt tooreceive bejövő forgalmat.</span><span class="sxs-lookup"><span data-stu-id="2de48-205">This step configures hello listeners for public IP address, port, and host used tooreceive incoming traffic.</span></span> <span data-ttu-id="2de48-206">Állomásnév paraméter több webhely támogatásához szükséges, és legyen beállítva toohello megfelelő webhely mely hello a forgalom érkezik.</span><span class="sxs-lookup"><span data-stu-id="2de48-206">HostName parameter is required for multiple site support and should be set toohello appropriate website for which hello traffic is received.</span></span> <span data-ttu-id="2de48-207">RequireServerNameIndication paraméter tootrue az webhely SSL-t, a több gazdagépen forgatókönyvek kell támogatnia kell állítani.</span><span class="sxs-lookup"><span data-stu-id="2de48-207">RequireServerNameIndication parameter should be set tootrue for websites that need support for SSL in a multiple host scenario.</span></span> <span data-ttu-id="2de48-208">Ha az SSL támogatására szükség, akkor is szükség toospecify hello SSL tanúsítvány, amely az adott webes alkalmazáshoz használt toosecure forgalmat.</span><span class="sxs-lookup"><span data-stu-id="2de48-208">If SSL support is required, you also need toospecify hello SSL certificate that is used toosecure traffic for that web application.</span></span> <span data-ttu-id="2de48-209">FrontendIPConfiguration FrontendPort és állomásnév hello kombinációjának egyedi tooa figyelő kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2de48-209">hello combination of FrontendIPConfiguration, FrontendPort, and HostName must be unique tooa listener.</span></span> <span data-ttu-id="2de48-210">Minden egyes figyelő több tanúsítvány is támogatja.</span><span class="sxs-lookup"><span data-stu-id="2de48-210">Each listener can support one certificate.</span></span>

```powershell
$listener01 = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "contoso11.com" -RequireServerNameIndication true  -SslCertificate $cert01
$listener02 = New-AzureRmApplicationGatewayHttpListener -Name "listener02" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "fabrikam11.com" -RequireServerNameIndication true -SslCertificate $cert02
```

### <a name="step-8"></a><span data-ttu-id="2de48-211">8. lépés</span><span class="sxs-lookup"><span data-stu-id="2de48-211">Step 8</span></span>

<span data-ttu-id="2de48-212">Hozzon létre beállítása hello két webes alkalmazások ebben a példában két szabályt.</span><span class="sxs-lookup"><span data-stu-id="2de48-212">Create two rule setting for hello two web applications in this example.</span></span> <span data-ttu-id="2de48-213">Egy szabály kötelékek együtt, figyelők, a háttérkészlet és a HTTP-beállításait.</span><span class="sxs-lookup"><span data-stu-id="2de48-213">A rule ties together listeners, backend pools and http settings.</span></span> <span data-ttu-id="2de48-214">Ez a lépés hello alkalmazás átjáró toouse alapvető útválasztási szabályokat konfigurálja, egy minden webhelyre vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="2de48-214">This step configures hello application gateway toouse Basic routing rule, one for each website.</span></span> <span data-ttu-id="2de48-215">Forgalom tooeach webhelyet a beállított figyelő fogad, és a rendszer ekkor átirányítja tooits konfigurált háttérkészlet, a backendhttpsettings beállítások hello megadott hello tulajdonságokkal.</span><span class="sxs-lookup"><span data-stu-id="2de48-215">Traffic tooeach website is received by its configured listener, and is then directed tooits configured backend pool, using hello properties specified in hello BackendHttpSettings.</span></span>

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule01" -RuleType Basic -HttpListener $listener01 -BackendHttpSettings $poolSetting01 -BackendAddressPool $pool1
$rule02 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule02" -RuleType Basic -HttpListener $listener02 -BackendHttpSettings $poolSetting02 -BackendAddressPool $pool2
```

### <a name="step-9"></a><span data-ttu-id="2de48-216">9. lépés</span><span class="sxs-lookup"><span data-stu-id="2de48-216">Step 9</span></span>

<span data-ttu-id="2de48-217">Példányok és hello Alkalmazásátjáró mérete hello számának konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="2de48-217">Configure hello number of instances and size for hello application gateway.</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Medium" -Tier Standard -Capacity 2
```

## <a name="create-application-gateway"></a><span data-ttu-id="2de48-218">Alkalmazásátjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="2de48-218">Create application gateway</span></span>

<span data-ttu-id="2de48-219">Hozzon létre egy alkalmazást az előző lépésekben hello összes konfigurációs objektumok.</span><span class="sxs-lookup"><span data-stu-id="2de48-219">Create an application gateway with all configuration objects from hello preceding steps.</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener01, $listener02 -RequestRoutingRules $rule01, $rule02 -Sku $sku -SslCertificates $cert01, $cert02
```

> [!IMPORTANT]
> <span data-ttu-id="2de48-220">Alkalmazás átjáró kiépítése hosszú ideig futó művelet, és némi időt toocomplete is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="2de48-220">Application Gateway provisioning is a long running operation and may take some time toocomplete.</span></span>
> 
> 

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="2de48-221">Az Application Gateway DNS-nevének beszerzése</span><span class="sxs-lookup"><span data-stu-id="2de48-221">Get application gateway DNS name</span></span>

<span data-ttu-id="2de48-222">Hello átjáró létrehozása után hello következő lépésre tooconfigure hello előtér-kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="2de48-222">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="2de48-223">Nyilvános IP-cím esetén az Application Gateway használatához dinamikusan hozzárendelt DNS-névre van szükség, amely nem valódi név.</span><span class="sxs-lookup"><span data-stu-id="2de48-223">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="2de48-224">tooensure a végfelhasználók is találati hello Alkalmazásátjáró, egy olyan CNAME rekordot is használt toopoint toohello nyilvános végpontot hello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="2de48-224">tooensure end users can hit hello application gateway, a CNAME record can be used toopoint toohello public endpoint of hello application gateway.</span></span> <span data-ttu-id="2de48-225">[Egyéni tartománynév konfigurálása az Azure-ban](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="2de48-225">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="2de48-226">toodo a, hello Alkalmazásátjáró és a társított IP-/ DNS-nevét, hello PublicIPAddress elem csatolt toohello Alkalmazásátjáró beolvasása részleteit.</span><span class="sxs-lookup"><span data-stu-id="2de48-226">toodo this, retrieve details of hello application gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello application gateway.</span></span> <span data-ttu-id="2de48-227">hello alkalmazás átjáró DNS-névnek kell lennie a használt toocreate egy olyan CNAME rekordot pontok hello két webes alkalmazások toothis DNS-név.</span><span class="sxs-lookup"><span data-stu-id="2de48-227">hello application gateway's DNS name should be used toocreate a CNAME record, which points hello two web applications toothis DNS name.</span></span> <span data-ttu-id="2de48-228">A-rekordok hello használata nem javasolt, mert hello VIP módosíthatja az Alkalmazásátjáró újra kell indítani.</span><span class="sxs-lookup"><span data-stu-id="2de48-228">hello use of A-records is not recommended since hello VIP may change on restart of application gateway.</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
```

```
Name                     : publicIP01
ResourceGroupName        : appgw-RG
Location                 : westus
Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
ResourceGuid             : 00000000-0000-0000-0000-000000000000
ProvisioningState        : Succeeded
Tags                     : 
PublicIpAllocationMethod : Dynamic
IpAddress                : xx.xx.xxx.xx
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                                "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                            Configurations/frontend1"
                            }
DnsSettings              : {
                                "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                            }
```

## <a name="next-steps"></a><span data-ttu-id="2de48-229">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2de48-229">Next steps</span></span>

<span data-ttu-id="2de48-230">Megtudhatja, hogyan tooprotect a webhelyek [Application Gateway - webalkalmazási tűzfal](application-gateway-webapplicationfirewall-overview.md)</span><span class="sxs-lookup"><span data-stu-id="2de48-230">Learn how tooprotect your websites with [Application Gateway - Web Application Firewall](application-gateway-webapplicationfirewall-overview.md)</span></span>

