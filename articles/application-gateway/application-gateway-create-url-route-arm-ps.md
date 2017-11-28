---
title: "Alkalmazásátjáró használatával URL-cím útválasztási szabályok aaaCreate |} Microsoft Docs"
description: "Ezen a lapon nyújt útmutatást toocreate, Azure Alkalmazásátjáró használatával URL-útválasztási szabályok konfigurálása"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: d141cfbb-320a-4fc9-9125-10001c6fa4cf
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/03/2017
ms.author: gwallace
ms.openlocfilehash: 54fcccc39e48a933576968ce3d8160518c0d0b7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-using-path-based-routing"></a><span data-ttu-id="ad42a-103">Elérési út alapú útválasztás használatával Alkalmazásátjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="ad42a-103">Create an application gateway using Path-based routing</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="ad42a-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ad42a-104">Azure portal</span></span>](application-gateway-create-url-route-portal.md)
> * [<span data-ttu-id="ad42a-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="ad42a-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-url-route-arm-ps.md)
> * [<span data-ttu-id="ad42a-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="ad42a-106">Azure CLI 2.0</span></span>](application-gateway-create-url-route-cli.md)

<span data-ttu-id="ad42a-107">URL-cím elérési út-alapú útválasztási lehetővé teszi, hogy Ön tooassociate útvonalak HTTP-kérések hello URL-címe alapján.</span><span class="sxs-lookup"><span data-stu-id="ad42a-107">URL Path-based routing enables you tooassociate routes based on hello URL path of an Http request.</span></span> <span data-ttu-id="ad42a-108">Ellenőrzi, hogy van-e egy útvonal tooa háttér-készlet szerepel a hello Alkalmazásátjáró hello URL-lel konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="ad42a-108">It checks if there is a route tooa back-end pool configured for hello URL presented in hello Application Gateway.</span></span> <span data-ttu-id="ad42a-109">Majd küldi hello hálózati forgalom toohello definiált háttér-készlet.</span><span class="sxs-lookup"><span data-stu-id="ad42a-109">It then sends hello network traffic toohello defined back-end pool.</span></span> <span data-ttu-id="ad42a-110">URL-alapú útválasztási gyakori felhasználási tooload-érkező kérések elosztása a különböző típusú tartalmakat toodifferent háttér-kiszolgálófiók készletek.</span><span class="sxs-lookup"><span data-stu-id="ad42a-110">A common use for URL-based routing is tooload balance requests for different content types toodifferent back-end server pools.</span></span>

<span data-ttu-id="ad42a-111">Új szabály típusa tooapplication átjáró URL-alapú útválasztási vezet be.</span><span class="sxs-lookup"><span data-stu-id="ad42a-111">URL-based routing introduces a new rule type tooapplication gateway.</span></span> <span data-ttu-id="ad42a-112">Alkalmazásátjáró két szabály tartozik: alapvető és PathBasedRouting.</span><span class="sxs-lookup"><span data-stu-id="ad42a-112">Application gateway has two rule types: basic and PathBasedRouting.</span></span> <span data-ttu-id="ad42a-113">Alapszintű szabálytípus biztosít a ciklikus multiplexelési hello háttér-szolgáltatás a készletbe közben PathBasedRouting továbbá tooround multiplexelés terjesztési, a is hello kérelem URL-címének elérési út mintája figyelembe veszi a hello háttérkészlet kiválasztásakor.</span><span class="sxs-lookup"><span data-stu-id="ad42a-113">Basic rule type provides round-robin service for hello back-end pools while PathBasedRouting in addition tooround robin distribution, also takes path pattern of hello request URL into account while choosing hello backend pool.</span></span>

## <a name="scenario"></a><span data-ttu-id="ad42a-114">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="ad42a-114">Scenario</span></span>

<span data-ttu-id="ad42a-115">A következő példa hello, Application Gateway van szolgáltató contoso.com forgalmát a két háttér-kiszolgálófiók rendelkezik: videó kiszolgálókészlet és lemezkép kiszolgálókészlethez.</span><span class="sxs-lookup"><span data-stu-id="ad42a-115">In hello following example, Application Gateway is serving traffic for contoso.com with two back-end server pools: video server pool and image server pool.</span></span>

<span data-ttu-id="ad42a-116">Tooimage (pool1), és hogy http://contoso.com/video * legyenek átirányítva a legyenek átirányítva http://contoso.com/image * kérelmek toovideo kiszolgálókészlet (pool2).</span><span class="sxs-lookup"><span data-stu-id="ad42a-116">Requests for http://contoso.com/image* are routed tooimage server pool (pool1), and http://contoso.com/video* are routed toovideo server pool (pool2).</span></span> <span data-ttu-id="ad42a-117">Ha hello elérési minták egyike, egy alapértelmezett kiszolgálókészletet (pool1) van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="ad42a-117">if none of hello path patterns match, a default server pool (pool1) is selected.</span></span>

![URL-cím útvonal](./media/application-gateway-create-url-route-arm-ps/figure1.png)

## <a name="before-you-begin"></a><span data-ttu-id="ad42a-119">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="ad42a-119">Before you begin</span></span>

1. <span data-ttu-id="ad42a-120">Webplatform-telepítő hello segítségével hello hello Azure PowerShell-parancsmagok legújabb verzióját telepítse.</span><span class="sxs-lookup"><span data-stu-id="ad42a-120">Install hello latest version of hello Azure PowerShell cmdlets by using hello Web Platform Installer.</span></span> <span data-ttu-id="ad42a-121">Töltse le, és telepítse hello legújabb verziót a hello **Windows PowerShell** hello szakasza [letöltési oldalon](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="ad42a-121">You can download and install hello latest version from hello **Windows PowerShell** section of hello [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="ad42a-122">Az Alkalmazásátjáró hoz létre egy virtuális hálózatot és az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="ad42a-122">You create a virtual network and subnet for Application Gateway.</span></span> <span data-ttu-id="ad42a-123">Győződjön meg arról, hogy egyetlen virtuális gépek vagy a felhőben történő alkalmazáshoz hello alhálózat használja.</span><span class="sxs-lookup"><span data-stu-id="ad42a-123">Make sure that no virtual machines or cloud deployments are using hello subnet.</span></span> <span data-ttu-id="ad42a-124">a virtuális hálózati alhálózat hello Alkalmazásátjáró önmagában kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ad42a-124">hello application gateway must be by itself in a virtual network subnet.</span></span>
3. <span data-ttu-id="ad42a-125">hello kiszolgálók toohello háttér-készlet toouse hello Alkalmazásátjáró kell lennie, különben a végpontokat hozott létre hello virtuális hálózatban vagy egy nyilvános IP-cím/VIP rendelt hozzá.</span><span class="sxs-lookup"><span data-stu-id="ad42a-125">hello servers added toohello back-end pool toouse hello application gateway must exist or have their endpoints created either in hello virtual network or with a public IP/VIP assigned.</span></span>

## <a name="what-is-required-toocreate-an-application-gateway"></a><span data-ttu-id="ad42a-126">Mi az a szükséges toocreate olyan átjárót?</span><span class="sxs-lookup"><span data-stu-id="ad42a-126">What is required toocreate an application gateway?</span></span>

* <span data-ttu-id="ad42a-127">**Háttér-kiszolgálófiók készlet:** hello hello háttér-kiszolgálók IP-címek listáját.</span><span class="sxs-lookup"><span data-stu-id="ad42a-127">**Back-end server pool:** hello list of IP addresses of hello back-end servers.</span></span> <span data-ttu-id="ad42a-128">hello IP-címek felsorolt toohello virtuális hálózati alhálózat vagy kell tartoznia, vagy egy nyilvános IP-cím/VIP kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ad42a-128">hello IP addresses listed should either belong toohello virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="ad42a-129">**Háttér-kiszolgálókészlet beállításai:** Minden készletnek vannak beállításai, például port, protokoll vagy cookie-alapú affinitás.</span><span class="sxs-lookup"><span data-stu-id="ad42a-129">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="ad42a-130">Ezek a beállítások esetén tooa kapcsolt verem és a hello készlet alkalmazott tooall-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="ad42a-130">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="ad42a-131">**Előtér-port:** Ez a port nem hello nyilvános portot, amelyet a hello Alkalmazásátjáró meg van nyitva.</span><span class="sxs-lookup"><span data-stu-id="ad42a-131">**Front-end port:** This port is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="ad42a-132">Forgalom találatok ezt a portot, és lekérdezi átirányítja tooone hello háttér-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="ad42a-132">Traffic hits this port, and then gets redirected tooone of hello back-end servers.</span></span>
* <span data-ttu-id="ad42a-133">**Figyelő:** hello figyelő rendelkezik egy előtér-portot, a protokollt (Http vagy Https, ezek az értékek kis-és nagybetűket), és hello SSL tanúsítvány neve (ha az SSL beállításának-kiszervezés).</span><span class="sxs-lookup"><span data-stu-id="ad42a-133">**Listener:** hello listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="ad42a-134">**Szabály:** hello szabály hello figyelő, hello háttér-kiszolgálófiók alkalmazáskészlet van kötve, és azt határozza meg, mely háttér-kiszolgálófiók készlet hello forgalom irányított toowhen találatok száma a egy adott figyelő.</span><span class="sxs-lookup"><span data-stu-id="ad42a-134">**Rule:** hello rule binds hello listener, hello back-end server pool and defines which back-end server pool hello traffic should be directed toowhen it hits a particular listener.</span></span>

## <a name="create-an-application-gateway"></a><span data-ttu-id="ad42a-135">Application Gateway létrehozása</span><span class="sxs-lookup"><span data-stu-id="ad42a-135">Create an application gateway</span></span>

<span data-ttu-id="ad42a-136">hello közötti Azure klasszikus és az Azure Resource Manager használatával különbség hello rendelés hello Alkalmazásátjáró és hello elemeket toobe konfigurálva kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="ad42a-136">hello difference between using Azure Classic and Azure Resource Manager is hello order in which you create hello application gateway and hello items that need toobe configured.</span></span>

<span data-ttu-id="ad42a-137">A Resource Manager az összes olyan átjárót elemeit egyedien vannak konfigurálva, majd tegye együtt toocreate hello alkalmazás átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="ad42a-137">With Resource Manager, all items that make an application gateway are configured individually and then put together toocreate hello application gateway resource.</span></span>

<span data-ttu-id="ad42a-138">Az alábbiakban a szükséges toocreate Alkalmazásátjáró hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="ad42a-138">Here are hello steps that are needed toocreate an application gateway:</span></span>

1. <span data-ttu-id="ad42a-139">Egy erőforráscsoport létrehozása a Resource Manager számára.</span><span class="sxs-lookup"><span data-stu-id="ad42a-139">Create a resource group for Resource Manager.</span></span>
2. <span data-ttu-id="ad42a-140">Hozzon létre egy virtuális hálózati alhálózat és hello alkalmazás átjáró nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="ad42a-140">Create a virtual network, subnet, and public IP for hello application gateway.</span></span>
3. <span data-ttu-id="ad42a-141">Egy Application Gateway konfigurációs objektum létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ad42a-141">Create an application gateway configuration object.</span></span>
4. <span data-ttu-id="ad42a-142">Egy Application Gateway erőforrás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ad42a-142">Create an application gateway resource.</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="ad42a-143">Erőforráscsoport létrehozása a Resource Managerhez</span><span class="sxs-lookup"><span data-stu-id="ad42a-143">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="ad42a-144">Győződjön meg arról, hogy azok be hello Azure PowerShell legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="ad42a-144">Make sure that you are using hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="ad42a-145">További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="ad42a-145">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="ad42a-146">1. lépés</span><span class="sxs-lookup"><span data-stu-id="ad42a-146">Step 1</span></span>

<span data-ttu-id="ad42a-147">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="ad42a-147">Log in tooAzure</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="ad42a-148">A hitelesítő adataival felszólító tooauthenticate áll.</span><span class="sxs-lookup"><span data-stu-id="ad42a-148">You are prompted tooauthenticate with your credentials.</span></span><BR>

### <a name="step-2"></a><span data-ttu-id="ad42a-149">2. lépés</span><span class="sxs-lookup"><span data-stu-id="ad42a-149">Step 2</span></span>

<span data-ttu-id="ad42a-150">Hello előfizetések hello fiók ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="ad42a-150">Check hello subscriptions for hello account.</span></span>

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a><span data-ttu-id="ad42a-151">3. lépés</span><span class="sxs-lookup"><span data-stu-id="ad42a-151">Step 3</span></span>

<span data-ttu-id="ad42a-152">Válassza ki, amely az Azure-előfizetések toouse.</span><span class="sxs-lookup"><span data-stu-id="ad42a-152">Choose which of your Azure subscriptions toouse.</span></span> <BR>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a><span data-ttu-id="ad42a-153">4. lépés</span><span class="sxs-lookup"><span data-stu-id="ad42a-153">Step 4</span></span>

<span data-ttu-id="ad42a-154">Hozzon létre egy erőforráscsoportot (hagyja ki ezt a lépést, ha egy meglévő erőforráscsoportot használ).</span><span class="sxs-lookup"><span data-stu-id="ad42a-154">Create a resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US"
```

<span data-ttu-id="ad42a-155">Másik lehetőségként az Alkalmazásátjáró erőforráscsoport címkéket is létrehozhat:</span><span class="sxs-lookup"><span data-stu-id="ad42a-155">Alternatively you can also create tags for a resource group for application gateway:</span></span>

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway URL routing"} 
```

<span data-ttu-id="ad42a-156">Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet.</span><span class="sxs-lookup"><span data-stu-id="ad42a-156">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="ad42a-157">Ez az erőforráscsoport hello alapértelmezett helyeként szolgál az erőforráscsoport erőforrások.</span><span class="sxs-lookup"><span data-stu-id="ad42a-157">This resource group is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="ad42a-158">Győződjön meg arról, hogy az összes parancsok toocreate egy alkalmazás átjáró használata hello ugyanabban az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="ad42a-158">Make sure that all commands toocreate an application gateway use hello same resource group.</span></span>

<span data-ttu-id="ad42a-159">A hello a fenti példában létrehozott egy erőforráscsoport neve "appgw-RG" és "Velünk nyugati" helyen.</span><span class="sxs-lookup"><span data-stu-id="ad42a-159">In hello example above, we created a resource group called "appgw-RG" and location "West US".</span></span>

> [!NOTE]
> <span data-ttu-id="ad42a-160">Ha egy egyéni mintavételt tooconfigure az Alkalmazásátjáró van szüksége, tekintse meg [PowerShell használatával hozzon létre olyan átjárót egyéni mintavételt](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="ad42a-160">If you need tooconfigure a custom probe for your application gateway, see [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md).</span></span> <span data-ttu-id="ad42a-161">További információért lásd: [egyéni minták és állapotfigyelés](application-gateway-probe-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ad42a-161">Check out [custom probes and health monitoring](application-gateway-probe-overview.md) for more information.</span></span>
> 
> 

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a><span data-ttu-id="ad42a-162">Hozzon létre egy virtuális hálózatot és hello Alkalmazásátjáró alhálózatot</span><span class="sxs-lookup"><span data-stu-id="ad42a-162">Create a virtual network and a subnet for hello application gateway</span></span>

<span data-ttu-id="ad42a-163">a következő példa azt mutatja meg hogyan hello toocreate erőforrás-kezelő használatával egy virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="ad42a-163">hello following example shows how toocreate a virtual network by using Resource Manager.</span></span> <span data-ttu-id="ad42a-164">Ez a példa egy VNETET az Alkalmazásátjáró hello hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ad42a-164">This example creates a VNET for hello Application Gateway.</span></span> <span data-ttu-id="ad42a-165">Alkalmazásátjáró saját alhálózatba hello Alkalmazásátjáró készült hello alhálózati értéke kisebb a VNET címterület hello ezért van szükség.</span><span class="sxs-lookup"><span data-stu-id="ad42a-165">Application Gateway requires its own subnet, for this reason hello subnet created for hello Application Gateway is smaller than hello VNET address space.</span></span> <span data-ttu-id="ad42a-166">Ez lehetővé teszi a további erőforrások, többek között, de nem korlátozott tooweb kiszolgálók toobe konfigurált hello azonos virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="ad42a-166">This allows for other resources, including but not limited tooweb servers toobe configured in hello same VNET.</span></span>

### <a name="step-1"></a><span data-ttu-id="ad42a-167">1. lépés</span><span class="sxs-lookup"><span data-stu-id="ad42a-167">Step 1</span></span>

<span data-ttu-id="ad42a-168">Rendelje hozzá a hello cím tartomány 10.0.0.0/24 toohello alhálózati használt változó toobe toocreate egy virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="ad42a-168">Assign hello address range 10.0.0.0/24 toohello subnet variable toobe used toocreate a virtual network.</span></span>  <span data-ttu-id="ad42a-169">Hello alhálózati konfigurációs objektum hello alkalmazás átjárón, amely a következő példában hello létrejön.</span><span class="sxs-lookup"><span data-stu-id="ad42a-169">This creates hello subnet configuration object for hello Application Gateway, which is used in hello next example.</span></span>

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

### <a name="step-2"></a><span data-ttu-id="ad42a-170">2. lépés</span><span class="sxs-lookup"><span data-stu-id="ad42a-170">Step 2</span></span>

<span data-ttu-id="ad42a-171">Hozzon létre egy virtuális hálózatot nevű **appgwvnet** erőforráscsoportban **appgw-rg** hello USA nyugati régiójában hello előtag 10.0.0.0/16 használata a 10.0.0.0/24 alhálózat.</span><span class="sxs-lookup"><span data-stu-id="ad42a-171">Create a virtual network named **appgwvnet** in resource group **appgw-rg** for hello West US region using hello prefix 10.0.0.0/16 with subnet 10.0.0.0/24.</span></span> <span data-ttu-id="ad42a-172">Ezzel befejezte a hello virtuális Hálózatot egyetlen alhálózattal a hello Alkalmazásátjáró tooreside hello konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="ad42a-172">This completes hello configuration of hello VNET with a single subnet for hello Application Gateway tooreside.</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

### <a name="step-3"></a><span data-ttu-id="ad42a-173">3. lépés</span><span class="sxs-lookup"><span data-stu-id="ad42a-173">Step 3</span></span>

<span data-ttu-id="ad42a-174">Rendelje hozzá a hello alhálózati változója hello lépések, ez toohello átadása `New-AzureRMApplicationGateway` parancsmag egy későbbi lépésben.</span><span class="sxs-lookup"><span data-stu-id="ad42a-174">Assign hello subnet variable for hello next steps, this is passed toohello `New-AzureRMApplicationGateway` cmdlet in a future step.</span></span>

```powershell
$subnet=$vnet.Subnets[0]
```

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a><span data-ttu-id="ad42a-175">A nyilvános IP-cím hello előtér-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="ad42a-175">Create a public IP address for hello front-end configuration</span></span>

<span data-ttu-id="ad42a-176">Hozzon létre egy nyilvános IP-erőforrás **publicIP01** erőforráscsoportban **appgw-rg** hello USA nyugati régiójában.</span><span class="sxs-lookup"><span data-stu-id="ad42a-176">Create a public IP resource **publicIP01** in resource group **appgw-rg** for hello West US region.</span></span> <span data-ttu-id="ad42a-177">Alkalmazásátjáró használhatja egy nyilvános IP-címet, a belső IP-címet vagy mindkét tooreceive kérelmek a(z) terheléselosztást.</span><span class="sxs-lookup"><span data-stu-id="ad42a-177">Application Gateway can use a public IP address, internal IP address or both tooreceive requests for load balancing.</span></span>  <span data-ttu-id="ad42a-178">Ebben a példában csak nyilvános IP-címet használunk.</span><span class="sxs-lookup"><span data-stu-id="ad42a-178">This example only uses a public IP address.</span></span> <span data-ttu-id="ad42a-179">A következő példa hello DNS-neve nem úgy van konfigurálva, a hello nyilvános IP-cím létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="ad42a-179">In hello following example, no DNS name is configured for creating hello Public IP address.</span></span>  <span data-ttu-id="ad42a-180">Az Application Gateway nem támogatja az egyéni DNS-neveket a nyilvános IP-címeken.</span><span class="sxs-lookup"><span data-stu-id="ad42a-180">Application Gateway does not support custom DNS names on public IP addresses.</span></span>  <span data-ttu-id="ad42a-181">Ha egy egyéni nevet hello nyilvános végpontot, egy CNAME rekordot kell létrehozni a toopoint automatikusan generált toohello DNS-név hello nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="ad42a-181">If a custom name is required for hello public endpoint, a CNAME record should be created toopoint toohello automatically generated DNS name for hello public IP address.</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

<span data-ttu-id="ad42a-182">IP-cím hozzá van rendelve toohello Alkalmazásátjáró hello szolgáltatás elindulásakor.</span><span class="sxs-lookup"><span data-stu-id="ad42a-182">An IP address is assigned toohello application gateway when hello service starts.</span></span>

## <a name="create-application-gateway-configuration"></a><span data-ttu-id="ad42a-183">Alkalmazás átjáró-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="ad42a-183">Create application gateway configuration</span></span>

<span data-ttu-id="ad42a-184">Az összes konfigurációs elemek hello Alkalmazásátjáró létrehozása előtt kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="ad42a-184">All configuration items must be set up before creating hello application gateway.</span></span> <span data-ttu-id="ad42a-185">hello lépések hello konfigurációs elemek létrehozását, amelyek szükségesek az alkalmazás átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="ad42a-185">hello following steps create hello configuration items that are needed for an application gateway resource.</span></span>

### <a name="step-1"></a><span data-ttu-id="ad42a-186">1. lépés</span><span class="sxs-lookup"><span data-stu-id="ad42a-186">Step 1</span></span>

<span data-ttu-id="ad42a-187">Hozzon létre egy **gatewayIP01** nevű Application Gateway IP-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="ad42a-187">Create an application gateway IP configuration named **gatewayIP01**.</span></span> <span data-ttu-id="ad42a-188">Alkalmazásátjáró indításakor szerzi be hello alhálózati konfigurált IP-címeit, és útvonal-hello háttér IP-készletet a hálózati forgalom toohello IP-címek.</span><span class="sxs-lookup"><span data-stu-id="ad42a-188">When Application Gateway starts, it picks up an IP address from hello subnet configured and route network traffic toohello IP addresses in hello back-end IP pool.</span></span> <span data-ttu-id="ad42a-189">Ne feledje, hogy minden példány egy IP-címet vesz fel.</span><span class="sxs-lookup"><span data-stu-id="ad42a-189">Keep in mind that each instance takes one IP address.</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

### <a name="step-2"></a><span data-ttu-id="ad42a-190">2. lépés</span><span class="sxs-lookup"><span data-stu-id="ad42a-190">Step 2</span></span>

<span data-ttu-id="ad42a-191">Konfigurálja hello háttér IP-címkészletet nevű **pool01** és **pool2** az IP-címekkel rendelkező **pool1** és **pool2**.</span><span class="sxs-lookup"><span data-stu-id="ad42a-191">Configure hello back-end IP address pool named **pool01** and **pool2** with IP addresses for **pool1** and **pool2**.</span></span> <span data-ttu-id="ad42a-192">A következő IP-címek azokat hello IP-címeket hello erőforrások üzemeltető hello webes alkalmazás toobe hello Alkalmazásátjáró védi.</span><span class="sxs-lookup"><span data-stu-id="ad42a-192">These IP addresses are hello IP addresses of hello resources that are hosting hello web application toobe protected by hello application gateway.</span></span> <span data-ttu-id="ad42a-193">Ezek háttér készlettagra megfelelő mintavételt által érvényesített toobe, hogy-e mintavételt alapszintű vagy egyéni mintavételt.</span><span class="sxs-lookup"><span data-stu-id="ad42a-193">These backend pool members are all validated toobe healthy by probes whether they are basic probes or custom probes.</span></span>  <span data-ttu-id="ad42a-194">Majd továbbítódik toothem, amikor hello Alkalmazásátjáró kérelmek kerülnek.</span><span class="sxs-lookup"><span data-stu-id="ad42a-194">Traffic is then routed toothem when requests come into hello application gateway.</span></span> <span data-ttu-id="ad42a-195">Háttérkészlet hello Alkalmazásátjáró, ami azt jelenti, egy háttér címkészletet használható több olyan webes alkalmazásokhoz, amelyek megtalálhatók a hello azonos belül több szabály által használható állomás.</span><span class="sxs-lookup"><span data-stu-id="ad42a-195">Backend pools can be used by multiple rules within hello application gateway, which means one backend pool could be used for multiple web applications that reside on hello same host.</span></span>

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 134.170.186.47, 134.170.189.222, 134.170.186.51
```

<span data-ttu-id="ad42a-196">Ebben a példában a rendszer két háttér-készletek tooroute hálózati forgalom hello URL-címe alapján.</span><span class="sxs-lookup"><span data-stu-id="ad42a-196">In this example, there are two back-end pools tooroute network traffic based on hello URL path.</span></span> <span data-ttu-id="ad42a-197">Az egyik készlet a „/video” URL-útvonalról, a másik az „/image” útvonalról fogadja a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="ad42a-197">One pool receives traffic from URL path "/video" and other pool receive traffic from path "/image".</span></span> <span data-ttu-id="ad42a-198">Cserélje le a fenti IP-címek tooadd saját alkalmazás IP-cím végpontok hello.</span><span class="sxs-lookup"><span data-stu-id="ad42a-198">Replace hello preceding IP addresses tooadd your own application IP address endpoints.</span></span> 

### <a name="step-3"></a><span data-ttu-id="ad42a-199">3. lépés</span><span class="sxs-lookup"><span data-stu-id="ad42a-199">Step 3</span></span>

<span data-ttu-id="ad42a-200">Alkalmazásbeállítás átjáró konfigurálása **poolsetting01** és **poolsetting02** hello terhelésű hálózati forgalmára hello háttér-készlet.</span><span class="sxs-lookup"><span data-stu-id="ad42a-200">Configure application gateway setting **poolsetting01** and **poolsetting02** for hello load-balanced network traffic in hello back-end pool.</span></span> <span data-ttu-id="ad42a-201">Ebben a példában ehhez beállításokat lehet megadni másik háttér címkészletet hello háttér-készletek.</span><span class="sxs-lookup"><span data-stu-id="ad42a-201">In this example, you configure different back-end pool settings for hello back-end pools.</span></span> <span data-ttu-id="ad42a-202">Mindegyik háttérkészlet saját háttérkészlet-beállításokkal rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="ad42a-202">Each back-end pool can have its own back-end pool setting.</span></span>  <span data-ttu-id="ad42a-203">Szabályok tooroute forgalom toohello megfelelő háttér a készlet tagjainak háttér HTTP-beállításokat használja.</span><span class="sxs-lookup"><span data-stu-id="ad42a-203">Backend HTTP settings are used by rules tooroute traffic toohello correct backend pool members.</span></span> <span data-ttu-id="ad42a-204">Ez határozza meg, hello protokoll és toohello háttér a készlet tagjainak forgalom küldéséhez használt port.</span><span class="sxs-lookup"><span data-stu-id="ad42a-204">This determines hello protocol and port that is used when sending traffic toohello backend pool members.</span></span> <span data-ttu-id="ad42a-205">A munkamenetek cookie-alapú is a backendhttpsettings osztályhoz hello határozza meg.</span><span class="sxs-lookup"><span data-stu-id="ad42a-205">Cookie-based sessions are also determined by hello backend HTTP settings.</span></span>  <span data-ttu-id="ad42a-206">Ha engedélyezve van, a munkamenet cookie-alapú kapcsolat küld-e a forgalom toohello azonos háttér, egyes csomagok előző kérelmekben.</span><span class="sxs-lookup"><span data-stu-id="ad42a-206">If enabled, cookie-based session affinity sends traffic toohello same backend as previous requests for each packet.</span></span>

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120

$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a><span data-ttu-id="ad42a-207">4. lépés</span><span class="sxs-lookup"><span data-stu-id="ad42a-207">Step 4</span></span>

<span data-ttu-id="ad42a-208">Nyilvános IP-végponton hello előtér-IP konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="ad42a-208">Configure hello front-end IP with public IP endpoint.</span></span> <span data-ttu-id="ad42a-209">hello előtér-IP-konfigurációs objektum használja egy figyelő toorelate hello kifelé irányuló hello figyelő IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="ad42a-209">hello front-end IP configuration object is used by a listener toorelate hello outward facing IP address with hello listener.</span></span>

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a><span data-ttu-id="ad42a-210">5. lépés</span><span class="sxs-lookup"><span data-stu-id="ad42a-210">Step 5</span></span>

<span data-ttu-id="ad42a-211">Hello előtér-port konfigurálása az Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="ad42a-211">Configure hello front-end port for an application gateway.</span></span> <span data-ttu-id="ad42a-212">hello előtér-port konfigurációs objektum használja egy figyelő toodefine melyik portot hello Alkalmazásátjáró figyeli a hello figyelő-forgalmat.</span><span class="sxs-lookup"><span data-stu-id="ad42a-212">hello front-end port configuration object is used by a listener toodefine what port hello Application Gateway listens for traffic on hello listener.</span></span>

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 80
```

### <a name="step-6"></a><span data-ttu-id="ad42a-213">6. lépés</span><span class="sxs-lookup"><span data-stu-id="ad42a-213">Step 6</span></span>

<span data-ttu-id="ad42a-214">Hello figyelő konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="ad42a-214">Configure hello listener.</span></span> <span data-ttu-id="ad42a-215">Ez a lépés konfigurál hello figyelő hello nyilvános IP-cím és port használt tooreceive bejövő hálózati forgalom.</span><span class="sxs-lookup"><span data-stu-id="ad42a-215">This step configures hello listener for hello public IP address and port used tooreceive incoming network traffic.</span></span> <span data-ttu-id="ad42a-216">a következő példa hello hello korábban konfigurált előtér-IP-konfiguráció, előtér-konfiguráció és a protokollt (http vagy https), és konfigurálja a hello figyelő.</span><span class="sxs-lookup"><span data-stu-id="ad42a-216">hello following example takes hello previously configured front-end IP configuration,  front-end port configuration, and a protocol (http or https) and configures hello listener.</span></span> <span data-ttu-id="ad42a-217">Ebben a példában hello figyelő tooHTTP forgalom hello nyilvános IP-címet, amely korábban a 80-as porton figyel.</span><span class="sxs-lookup"><span data-stu-id="ad42a-217">In this example, hello listener listens tooHTTP traffic on port 80 on hello public IP address that was created earlier.</span></span>

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Http -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01
```

### <a name="step-7"></a><span data-ttu-id="ad42a-218">7. lépés</span><span class="sxs-lookup"><span data-stu-id="ad42a-218">Step 7</span></span>

<span data-ttu-id="ad42a-219">URL-cím szabály útvonalak hello háttér-címkészletek konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="ad42a-219">Configure URL rule paths for hello back-end pools.</span></span> <span data-ttu-id="ad42a-220">Ebben a lépésben hello relatív elérési út alkalmazás-átjáró által használt konfigurálja, és határozza meg a hello leképezése hello URL-címe és hello háttér-készlet, amely hozzá van rendelve toohandle hello bejövő forgalmat.</span><span class="sxs-lookup"><span data-stu-id="ad42a-220">This step configures hello relative path used by application gateway and defines hello mapping between hello URL path and hello back-end pool that is assigned toohandle hello incoming traffic.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ad42a-221">Egyes elérési utakat kell kezdődnie / és hello egyetlen hely, ahol a "\*" engedélyezett, a hello végén.</span><span class="sxs-lookup"><span data-stu-id="ad42a-221">Each path must start with / and hello only place a "\*" is allowed, is at hello end.</span></span> <span data-ttu-id="ad42a-222">Érvényes többek között az /xyz, /xyz* vagy/xyz / *.</span><span class="sxs-lookup"><span data-stu-id="ad42a-222">Valid examples are /xyz, /xyz* or /xyz/*.</span></span> <span data-ttu-id="ad42a-223">hello toohello elérési matcher táplált karakterlánc nem tartalmaz sem szöveges hello után először "?" vagy "#", és ezek a karakterek nem engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="ad42a-223">hello string fed toohello path matcher does not include any text after hello first "?" or "#", and those characters are not allowed.</span></span> 

<span data-ttu-id="ad42a-224">hello alábbi példa létrehoz két szabályt: egy a "/ kép /" elérési út útválasztási forgalom tooback-end "pool1" és a "/ videó /" elérési útválasztási forgalom tooback-end "pool2."</span><span class="sxs-lookup"><span data-stu-id="ad42a-224">hello following example creates two rules: one for "/image/" path routing traffic tooback-end "pool1" and another one for "/video/" path routing traffic tooback-end "pool2."</span></span> <span data-ttu-id="ad42a-225">Ezek a szabályok arról, hogy az egyes URL-címek forgalmát irányított toohello háttér.</span><span class="sxs-lookup"><span data-stu-id="ad42a-225">These rules ensure that traffic for each set of urls is routed toohello backend.</span></span> <span data-ttu-id="ad42a-226">Például http://contoso.com/image/figure1.jpg toopool1 kerül, és http://contoso.com/video/example.mp4 toopool2 kerül.</span><span class="sxs-lookup"><span data-stu-id="ad42a-226">For example, http://contoso.com/image/figure1.jpg goes toopool1 and http://contoso.com/video/example.mp4 goes toopool2.</span></span>

```powershell
$imagePathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule1" -Paths "/image/*" -BackendAddressPool $pool1 -BackendHttpSettings $poolSetting01

$videoPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule2" -Paths "/video/*" -BackendAddressPool $pool2 -BackendHttpSettings $poolSetting02
```

<span data-ttu-id="ad42a-227">Hello elérési út bármely hello előre definiált elérésiút-szabály nem megfelelő, ha a szabály térkép konfiguráció hello is konfigurálja egy háttér címkészletet.</span><span class="sxs-lookup"><span data-stu-id="ad42a-227">If hello path doesn't match any of hello pre-defined path rules, hello rule path map configuration also configures a default back-end address pool.</span></span> <span data-ttu-id="ad42a-228">Például a http://contoso.com/shoppingcart/test.html toopool1 kerül, mint hello alapértelmezett alkalmazáskészlet páratlan forgalom van definiálva.</span><span class="sxs-lookup"><span data-stu-id="ad42a-228">For example, http://contoso.com/shoppingcart/test.html goes toopool1 as it is defined as hello default pool for unmatched traffic.</span></span>

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $videoPathRule, $imagePathRule -DefaultBackendAddressPool $pool1 -DefaultBackendHttpSettings $poolSetting02
```

### <a name="step-8"></a><span data-ttu-id="ad42a-229">8. lépés</span><span class="sxs-lookup"><span data-stu-id="ad42a-229">Step 8</span></span>

<span data-ttu-id="ad42a-230">Hozzon létre egy szabályt beállítást.</span><span class="sxs-lookup"><span data-stu-id="ad42a-230">Create a rule setting.</span></span> <span data-ttu-id="ad42a-231">Ez a lépés konfigurál hello alkalmazás átjáró toouse URL-cím elérési út-alapú útválasztási.</span><span class="sxs-lookup"><span data-stu-id="ad42a-231">This step configures hello application gateway toouse URL path-based routing.</span></span> <span data-ttu-id="ad42a-232">Hello `$urlPathMap` definiált változó hello a korábbi. lépés: most használt toocreate hello elérési alapuló szabály.</span><span class="sxs-lookup"><span data-stu-id="ad42a-232">hello `$urlPathMap` variable defined in hello earlier step is now used toocreate hello path-based rule.</span></span> <span data-ttu-id="ad42a-233">Ebben a lépésben azt társítsa hello szabály egy figyelő és a korábban létrehozott hello URL-cím elérési útjának leképezése.</span><span class="sxs-lookup"><span data-stu-id="ad42a-233">In this step, we associate hello rule with a listener and hello url path mapping created earlier.</span></span>

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-9"></a><span data-ttu-id="ad42a-234">9. lépés</span><span class="sxs-lookup"><span data-stu-id="ad42a-234">Step 9</span></span>

<span data-ttu-id="ad42a-235">Példányok és hello Alkalmazásátjáró mérete hello számának konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="ad42a-235">Configure hello number of instances and size for hello application gateway.</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Small" -Tier Standard -Capacity 2
```

## <a name="create-application-gateway"></a><span data-ttu-id="ad42a-236">Alkalmazásátjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="ad42a-236">Create Application Gateway</span></span>

<span data-ttu-id="ad42a-237">Hozzon létre egy alkalmazást az előző lépésekben hello összes konfigurációs objektumok.</span><span class="sxs-lookup"><span data-stu-id="ad42a-237">Create an application gateway with all configuration objects from hello preceding steps.</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku
```

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="ad42a-238">Az Application Gateway DNS-nevének beszerzése</span><span class="sxs-lookup"><span data-stu-id="ad42a-238">Get application gateway DNS name</span></span>

<span data-ttu-id="ad42a-239">Hello átjáró létrehozása után hello következő lépésre tooconfigure hello előtér-kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="ad42a-239">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="ad42a-240">Nyilvános IP-cím esetén az Application Gateway használatához dinamikusan hozzárendelt DNS-névre van szükség, amely nem valódi név.</span><span class="sxs-lookup"><span data-stu-id="ad42a-240">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="ad42a-241">tooensure a végfelhasználók is találati hello Alkalmazásátjáró, egy olyan CNAME rekordot is használt toopoint toohello nyilvános végpontot hello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="ad42a-241">tooensure end users can hit hello application gateway, a CNAME record can be used toopoint toohello public endpoint of hello application gateway.</span></span> <span data-ttu-id="ad42a-242">[Egyéni tartománynév konfigurálása az Azure-ban](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ad42a-242">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="ad42a-243">tooconfigure hello előtérbeli IP-CNAME rekord részleteit hello Alkalmazásátjáró és a társított IP-/ DNS-nevét, hello PublicIPAddress elem csatolt toohello Alkalmazásátjáró beolvasása.</span><span class="sxs-lookup"><span data-stu-id="ad42a-243">tooconfigure hello frontend IP CNAME record, retrieve details of hello application gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello application gateway.</span></span> <span data-ttu-id="ad42a-244">hello alkalmazás átjáró DNS-név használt toocreate egy olyan CNAME rekordot kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ad42a-244">hello application gateway's DNS name should be used toocreate a CNAME record.</span></span> <span data-ttu-id="ad42a-245">A-rekordok hello használata nem javasolt, mert hello VIP módosíthatja az Alkalmazásátjáró újra kell indítani.</span><span class="sxs-lookup"><span data-stu-id="ad42a-245">hello use of A-records is not recommended since hello VIP may change on restart of application gateway.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="ad42a-246">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ad42a-246">Next steps</span></span>

<span data-ttu-id="ad42a-247">Ha azt szeretné, hogy Secure Sockets Layer (SSL)-kiszervezés toolearn, [konfigurálása az SSL-kiszervezés Alkalmazásátjáró](application-gateway-ssl-arm.md).</span><span class="sxs-lookup"><span data-stu-id="ad42a-247">If you want toolearn Secure Sockets Layer (SSL) offload, see [Configure an application gateway for SSL offload](application-gateway-ssl-arm.md).</span></span>

