---
title: "aaaConfigure webalkalmazási tűzfal - Azure Application Gateway |} Microsoft Docs"
description: "Ez a cikk hogyan toostart használatával webalkalmazási tűzfal a meglévő vagy új Alkalmazásátjáró nyújt útmutatást."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 670b9732-874b-43e6-843b-d2585c160982
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: gwallace
ms.openlocfilehash: bd2a69901f0ec0d6551d633e2588b74c3ab59a71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway"></a><span data-ttu-id="2000b-103">Webalkalmazási tűzfal egy új vagy meglévő Alkalmazásátjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2000b-103">Configure web application firewall on a new or existing Application Gateway</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="2000b-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2000b-104">Azure portal</span></span>](application-gateway-web-application-firewall-portal.md)
> * [<span data-ttu-id="2000b-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2000b-105">PowerShell</span></span>](application-gateway-web-application-firewall-powershell.md)
> * [<span data-ttu-id="2000b-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2000b-106">Azure CLI</span></span>](application-gateway-web-application-firewall-cli.md)

<span data-ttu-id="2000b-107">Ismerje meg, hogyan toocreate webalkalmazási tűzfal engedélyezve Application Gateway, vagy adja hozzá a webes alkalmazás tűzfal tooan meglévő Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="2000b-107">Learn how toocreate a web application firewall enabled Application Gateway or add web application firewall tooan existing Application Gateway.</span></span>

<span data-ttu-id="2000b-108">hello webalkalmazási tűzfal (waf-ot) az Azure alkalmazás átjáró webalkalmazások védje a közös web-alapú támadások, például az SQL-injektálás, a többhelyes parancsfájlok futtatására és a munkamenet kihasználásának.</span><span class="sxs-lookup"><span data-stu-id="2000b-108">hello web application firewall (WAF) in Azure Application Gateway protects web applications from common web-based attacks like SQL injection, cross-site scripting attacks, and session hijacks.</span></span>

<span data-ttu-id="2000b-109">Az Azure Application Gateway egy 7. rétegbeli terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="2000b-109">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="2000b-110">Akár hello felhőalapú vagy helyszíni biztosít feladatátvételt, HTTP-kérelmek teljesítmény-útválasztási különböző kiszolgálók között.</span><span class="sxs-lookup"><span data-stu-id="2000b-110">It provides failover, performance-routing HTTP requests between different servers, whether they are on hello cloud or on-premises.</span></span> <span data-ttu-id="2000b-111">Alkalmazásátjáró biztosít számos alkalmazás kézbesítési vezérlő (LÉPETT) szolgáltatást HTTP beleértve betöltése terheléselosztási, a cookie-alapú munkamenet-kapcsolatot, és a secure sockets layer (SSL)-kiszervezés el egyéni állapotteljesítmény, többhelyes támogatása, és sok más.</span><span class="sxs-lookup"><span data-stu-id="2000b-111">Application Gateway provides many application delivery controller (ADC) features including HTTP load balancing, cookie-based session affinity, secure sockets layer (SSL) offload, custom health probes, support for multi-site, and many others.</span></span> <span data-ttu-id="2000b-112">támogatott szolgáltatások teljes listáját toofind látogassa meg: [Alkalmazásátjáró áttekintése](application-gateway-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2000b-112">toofind a complete list of supported features, visit: [Overview of Application Gateway](application-gateway-introduction.md).</span></span>

<span data-ttu-id="2000b-113">hello következő a cikk bemutatja, hogyan túl[hozzáadása a webes alkalmazás tűzfal tooan meglévő Alkalmazásátjáró](#add-web-application-firewall-to-an-existing-application-gateway) és [webalkalmazási tűzfal használó Alkalmazásátjáró létrehozása](#create-an-application-gateway-with-web-application-firewall).</span><span class="sxs-lookup"><span data-stu-id="2000b-113">hello following article shows how too[add web application firewall tooan existing Application Gateway](#add-web-application-firewall-to-an-existing-application-gateway) and [create an Application Gateway that uses web application firewall](#create-an-application-gateway-with-web-application-firewall).</span></span>

![a forgatókönyv kép][scenario]

## <a name="waf-configuration-differences"></a><span data-ttu-id="2000b-115">WAF konfigurációs különbségek</span><span class="sxs-lookup"><span data-stu-id="2000b-115">WAF configuration differences</span></span>

<span data-ttu-id="2000b-116">Ha mindenképpen olvassa el [Alkalmazásátjáró létrehozása a PowerShell használatával](application-gateway-create-gateway-arm.md), tisztában hello SKU beállítások tooconfigure Alkalmazásátjáró létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="2000b-116">If you have read [Create an Application Gateway with PowerShell](application-gateway-create-gateway-arm.md), you understand hello SKU settings tooconfigure when creating an Application Gateway.</span></span> <span data-ttu-id="2000b-117">WAF biztosít a további beállítások toodefine Alkalmazásátjáró hello SKU konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="2000b-117">WAF provides additional settings toodefine when configuring hello SKU on an Application Gateway.</span></span> <span data-ttu-id="2000b-118">Nincsenek további módosítások, amely akkor adja meg a hello Alkalmazásátjáró magát.</span><span class="sxs-lookup"><span data-stu-id="2000b-118">There are no additional changes that you make on hello Application Gateway itself.</span></span>

| <span data-ttu-id="2000b-119">**Beállítás**</span><span class="sxs-lookup"><span data-stu-id="2000b-119">**Setting**</span></span> | <span data-ttu-id="2000b-120">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="2000b-120">**Details**</span></span>
|---|---|
|<span data-ttu-id="2000b-121">**Termékváltozat**</span><span class="sxs-lookup"><span data-stu-id="2000b-121">**SKU**</span></span> |<span data-ttu-id="2000b-122">Egy normál Alkalmazásátjáró nélkül WAF támogatja **szabványos\_kis**, **szabványos\_Közepes**, és **szabványos\_nagy** méretét.</span><span class="sxs-lookup"><span data-stu-id="2000b-122">A normal Application Gateway without WAF supports **Standard\_Small**, **Standard\_Medium**, and **Standard\_Large** sizes.</span></span> <span data-ttu-id="2000b-123">WAF hello bevezetését, a esetén két további SKU, **WAF\_Közepes** és **WAF\_nagy**.</span><span class="sxs-lookup"><span data-stu-id="2000b-123">With hello introduction of WAF, there are two additional SKUs, **WAF\_Medium** and **WAF\_Large**.</span></span> <span data-ttu-id="2000b-124">WAF kis Alkalmazásátjárót nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="2000b-124">WAF is not supported on small Application Gateways.</span></span>|
|<span data-ttu-id="2000b-125">**Réteg**</span><span class="sxs-lookup"><span data-stu-id="2000b-125">**Tier**</span></span> | <span data-ttu-id="2000b-126">hello elérhető értékek a következők **szabványos** vagy **WAF**.</span><span class="sxs-lookup"><span data-stu-id="2000b-126">hello available values are **Standard** or **WAF**.</span></span> <span data-ttu-id="2000b-127">Webalkalmazási tűzfal, használatakor **WAF** ki kell választani.</span><span class="sxs-lookup"><span data-stu-id="2000b-127">When using web application firewall, **WAF** must be chosen.</span></span>|
|<span data-ttu-id="2000b-128">**Mód**</span><span class="sxs-lookup"><span data-stu-id="2000b-128">**Mode**</span></span> | <span data-ttu-id="2000b-129">Ez a beállítás akkor WAF hello módját.</span><span class="sxs-lookup"><span data-stu-id="2000b-129">This setting is hello mode of WAF.</span></span> <span data-ttu-id="2000b-130">két érték engedélyezett **észlelési** és **megelőzési**.</span><span class="sxs-lookup"><span data-stu-id="2000b-130">allowed values are **Detection** and **Prevention**.</span></span> <span data-ttu-id="2000b-131">Ha WAF észlelési mód van beállítva, minden fenyegetések naplófájl vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="2000b-131">When WAF is set up in detection mode, all threats are stored in a log file.</span></span> <span data-ttu-id="2000b-132">Megelőzési módban eseményeket naplózza, de hello támadó 403-as jogosulatlan választ kap hello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="2000b-132">In prevention mode, events are still logged but hello attacker receives a 403 unauthorized response from hello Application Gateway.</span></span>|

## <a name="add-web-application-firewall-tooan-existing-application-gateway"></a><span data-ttu-id="2000b-133">Webes alkalmazás tűzfal tooan meglévő Alkalmazásátjáró hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2000b-133">Add web application firewall tooan existing Application Gateway</span></span>

<span data-ttu-id="2000b-134">Győződjön meg arról, hogy azok be hello Azure PowerShell legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="2000b-134">Make sure that you are using hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="2000b-135">További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="2000b-135">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

1. <span data-ttu-id="2000b-136">Jelentkezzen be Azure-fiók tooyour.</span><span class="sxs-lookup"><span data-stu-id="2000b-136">Log in tooyour Azure Account.</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

2. <span data-ttu-id="2000b-137">Válassza ki a hello előfizetés toouse ehhez a forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="2000b-137">Select hello subscription toouse for this scenario.</span></span>

    ```powershell
    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"
    ```

3. <span data-ttu-id="2000b-138">Webalkalmazási tűzfal a felvenni kívánt hello átjáró beolvasása.</span><span class="sxs-lookup"><span data-stu-id="2000b-138">Retrieve hello gateway that you are adding web application firewall to.</span></span>

    ```powershell
    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"
    ```

1. <span data-ttu-id="2000b-139">Hello webes alkalmazás tűzfal sku konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="2000b-139">Configure hello web application firewall sku.</span></span> <span data-ttu-id="2000b-140">hello elérhető értékek **WAF\_nagy** és **WAF\_Közepes**.</span><span class="sxs-lookup"><span data-stu-id="2000b-140">hello available sizes are **WAF\_Large** and **WAF\_Medium**.</span></span> <span data-ttu-id="2000b-141">Webalkalmazási tűzfal használata esetén a hello rétegnek különböznie kell **WAF**, meg kell erősíteni hello kapacitás, hello sku beállításakor.</span><span class="sxs-lookup"><span data-stu-id="2000b-141">When web application firewall is used hello tier must be **WAF**, hello capacity must be confirmed when setting hello sku.</span></span>

    ```powershell
    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF -Capacity 2
    ```

1. <span data-ttu-id="2000b-142">Hello WAF beállítások konfigurálása a következő példa hello meghatározottak szerint:</span><span class="sxs-lookup"><span data-stu-id="2000b-142">Configure hello WAF settings as defined in hello following example:</span></span>

   <span data-ttu-id="2000b-143">A **FirewallMode**, hello elérhető értékek a következők megelőzése és észlelését.</span><span class="sxs-lookup"><span data-stu-id="2000b-143">For **FirewallMode**, hello available values are Prevention and Detection.</span></span>

    ```powershell
    $gw | Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention
    ```

1. <span data-ttu-id="2000b-144">Frissítés hello Alkalmazásátjáró lépést megelőző hello definiált hello beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="2000b-144">Update hello Application Gateway with hello settings defined in hello preceding step.</span></span>

    ```powershell
    Set-AzureRmApplicationGateway -ApplicationGateway $gw
    ```

<span data-ttu-id="2000b-145">Ez a parancs webalkalmazási tűzfal hello Alkalmazásátjáró frissítése.</span><span class="sxs-lookup"><span data-stu-id="2000b-145">This command updates hello Application Gateway with web application firewall.</span></span> <span data-ttu-id="2000b-146">Látogasson el [Alkalmazásátjáró diagnosztika](application-gateway-diagnostics.md) toounderstand, hogy miként naplózza az tooview az alkalmazás átjáró.</span><span class="sxs-lookup"><span data-stu-id="2000b-146">Visit [Application Gateway diagnostics](application-gateway-diagnostics.md) toounderstand how tooview logs for your Application Gateway.</span></span> <span data-ttu-id="2000b-147">WAF toohello biztonsági jellege miatt naplók kell toobe rendszeresen ellenőrizni a webalkalmazások toounderstand hello biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="2000b-147">Due toohello security nature of WAF, logs need toobe reviewed regularly toounderstand hello security posture of your web applications.</span></span>

## <a name="create-an-application-gateway-with-web-application-firewall"></a><span data-ttu-id="2000b-148">Webalkalmazási tűzfal Alkalmazásátjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="2000b-148">Create an Application Gateway with web application firewall</span></span>

<span data-ttu-id="2000b-149">hello következő lépések Alkalmazásátjáró webalkalmazási tűzfal való létrehozásának kezdő tooend hello teljes folyamatot.</span><span class="sxs-lookup"><span data-stu-id="2000b-149">hello following steps take you through hello entire process from beginning tooend for creating an Application Gateway with web application firewall.</span></span>

<span data-ttu-id="2000b-150">Győződjön meg arról, hogy azok be hello Azure PowerShell legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="2000b-150">Make sure that you are using hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="2000b-151">További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="2000b-151">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

1. <span data-ttu-id="2000b-152">Jelentkezzen be tooAzure futtatásával `Login-AzureRmAccount`, rákérdezéses tooauthenticate áll a hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="2000b-152">Log in tooAzure by running `Login-AzureRmAccount`, you are prompted tooauthenticate with your credentials.</span></span>

1. <span data-ttu-id="2000b-153">Ellenőrizze a hello fiókhoz hello előfizetések futtatásával`Get-AzureRmSubscription`</span><span class="sxs-lookup"><span data-stu-id="2000b-153">Check hello subscriptions for hello account by running `Get-AzureRmSubscription`</span></span>

1. <span data-ttu-id="2000b-154">Válassza ki, amely az Azure-előfizetések toouse.</span><span class="sxs-lookup"><span data-stu-id="2000b-154">Choose which of your Azure subscriptions toouse.</span></span>

    ```powershell
    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"
    ```

### <a name="create-a-resource-group"></a><span data-ttu-id="2000b-155">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="2000b-155">Create a resource group</span></span>

<span data-ttu-id="2000b-156">Hozzon létre egy erőforráscsoportot hello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="2000b-156">Create a resource group for hello Application Gateway.</span></span>

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

<span data-ttu-id="2000b-157">Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet.</span><span class="sxs-lookup"><span data-stu-id="2000b-157">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="2000b-158">Ezen a helyen az erőforráscsoport erőforrások lesz hello alapértelmezett helye.</span><span class="sxs-lookup"><span data-stu-id="2000b-158">This location is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="2000b-159">Győződjön meg arról, hogy az Application Gateway toocreate használja minden parancs hello ugyanabban az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="2000b-159">Make sure that all commands toocreate an Application Gateway uses hello same resource group.</span></span>

<span data-ttu-id="2000b-160">A fenti példa hello létrehozott egy "Appgw-RG" és a hely "USA nyugati régiója." nevű erőforráscsoportban található</span><span class="sxs-lookup"><span data-stu-id="2000b-160">In hello preceding example, we created a resource group called "appgw-RG" and location "West US."</span></span>

> [!NOTE]
> <span data-ttu-id="2000b-161">Ha egy egyéni mintavételt tooconfigure az Alkalmazásátjáró van szüksége, tekintse meg [PowerShell használatával hozzon létre olyan átjárót egyéni mintavételt](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="2000b-161">If you need tooconfigure a custom probe for your Application Gateway, see [Create an Application Gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md).</span></span> <span data-ttu-id="2000b-162">További információért lásd: [egyéni minták és állapotfigyelés](application-gateway-probe-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2000b-162">Check out [custom probes and health monitoring](application-gateway-probe-overview.md) for more information.</span></span>

### <a name="configure-virtual-network"></a><span data-ttu-id="2000b-163">Virtuális hálózat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2000b-163">Configure virtual network</span></span>

<span data-ttu-id="2000b-164">Alkalmazásátjáró saját alhálózat szükséges.</span><span class="sxs-lookup"><span data-stu-id="2000b-164">Application Gateway requires a subnet of its own.</span></span> <span data-ttu-id="2000b-165">Ebben a lépésben egy címtartománnyal 10.0.0.0/16 és két alhálózat, egy a hello Alkalmazásátjáró és egy háttér a készlet tagjainak a virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2000b-165">In this step, you create a virtual network with an address space of 10.0.0.0/16 and two subnets, one for hello Application Gateway and one for backend pool members.</span></span>

```powershell
# Create a subnet configuration object for hello Application Gateway subnet. A subnet for an application should have a minimum of 28 mask bits. This value leaves 10 available addresses in hello subnet for Application Gateway instances. With a smaller subnet, you may not be able tooadd more instance of your Application Gateway in hello future.
$gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

# Create a subnet configuration object for hello backend pool members subnet
$nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

# Create hello virtual network with hello previous created subnets
$vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
```

### <a name="configure-public-ip-address"></a><span data-ttu-id="2000b-166">Nyilvános IP-cím konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2000b-166">Configure public IP address</span></span>

<span data-ttu-id="2000b-167">Alkalmazásátjáró rendelés toohandle külső kérelmeket, egy nyilvános IP-cím használatához szükséges.</span><span class="sxs-lookup"><span data-stu-id="2000b-167">In order toohandle external requests, Application Gateway requires a public IP address.</span></span> <span data-ttu-id="2000b-168">A nyilvános IP-cím nem lehet egy `DomainNameLabel` definiált toobe hello Alkalmazásátjáró használják.</span><span class="sxs-lookup"><span data-stu-id="2000b-168">This public IP address must not have a `DomainNameLabel` defined toobe used by hello Application Gateway.</span></span>

```powershell
# Create a public IP address for use with hello Application Gateway. Defining hello domainnamelabel during creation is not supported for use with Application Gateway
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic
```

### <a name="configure-hello-application-gateway"></a><span data-ttu-id="2000b-169">Alkalmazásátjáró hello konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2000b-169">Configure hello Application Gateway</span></span>

```powershell
# Create a IP configuration. This configures what subnet hello Application Gateway uses. When Application Gateway starts, it picks up an IP address from hello subnet configured and routes network traffic toohello IP addresses in hello back-end IP pool.
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

# Create a backend pool toohold hello addresses or NICs for hello application that Application Gateway is protecting.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

# Upload hello authenication certificate that will be used toocommunicate with hello backend servers
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path too.cer file>

# Conifugre hello backend HTTP settings toobe used toodefine how traffic is routed toohello backend pool. hello authenication certificate used in hello previous step is added toohello backend http settings.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

# Create a frontend port toobe used by hello listener.
$fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

# Create a frontend IP configuration tooassociate hello public IP address with hello Application Gateway
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

# Configure hello certificate for hello Application Gateway. This certificate is used toodecrypt and re-encrypt hello traffic on hello Application Gateway.
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path too.pfx file> -Password <password for certificate file>

# Create hello HTTP listener for hello Application Gateway. Assign hello front-end ip configuration, port, and ssl certificate toouse.
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

#Create a load balancer routing rule that configures hello load balancer behavior. In this example, a basic round robin rule is created.
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Configure hello SKU of hello Application Gateway
$sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

# Define hello SSL policy toouse
$policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S

#Configure hello waf configuration settings.
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"

# Create hello Application Gateway utilizing all hello previously created configuration objects
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert
```

> [!NOTE]
> <span data-ttu-id="2000b-170">Hello alapvető webalkalmazás tűzfal konfigurációjának létre Alkalmazásátjárót védelmet CRS 3.0-val van állítva.</span><span class="sxs-lookup"><span data-stu-id="2000b-170">Application Gateways created with hello basic web application firewall configuration are configured with CRS 3.0 for protections.</span></span>

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="2000b-171">Alkalmazás átjáró DNS-név beolvasása</span><span class="sxs-lookup"><span data-stu-id="2000b-171">Get Application Gateway DNS name</span></span>

<span data-ttu-id="2000b-172">Hello átjáró létrehozása után hello következő lépésre tooconfigure hello előtér-kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="2000b-172">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="2000b-173">Egy nyilvános IP-cím használata esetén az Application Gateway egy dinamikusan hozzárendelt DNS-nevet, amely nincs rövid van szükség.</span><span class="sxs-lookup"><span data-stu-id="2000b-173">When using a public IP, Application Gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="2000b-174">a végfelhasználók tooensure is találati hello Application Gateway, egy olyan CNAME rekordot is használt toopoint toohello nyilvános végpontja hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="2000b-174">tooensure end users can hit hello Application Gateway, a CNAME record can be used toopoint toohello public endpoint of hello Application Gateway.</span></span> <span data-ttu-id="2000b-175">[Egyéni tartománynév konfigurálása az Azure-ban](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="2000b-175">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="2000b-176">tooconfigure alias, hello Application Gateway és a társított IP-/ DNS-nevét, hello PublicIPAddress csatolt elem toohello Alkalmazásátjáró beolvasása.</span><span class="sxs-lookup"><span data-stu-id="2000b-176">tooconfigure an alias, retrieve details of hello Application Gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello Application Gateway.</span></span> <span data-ttu-id="2000b-177">hello Application Gateway DNS-névnek kell lennie a használt toocreate egy olyan CNAME rekordot pontok hello két webes alkalmazások toothis DNS-név.</span><span class="sxs-lookup"><span data-stu-id="2000b-177">hello Application Gateway's DNS name should be used toocreate a CNAME record, which points hello two web applications toothis DNS name.</span></span> <span data-ttu-id="2000b-178">A-rekordok hello használata nem ajánlott, óta hello VIP előfordulhat, hogy az Application Gateway újra kell indítani.</span><span class="sxs-lookup"><span data-stu-id="2000b-178">hello use of A-records is not recommended since hello VIP may change on restart of Application Gateway.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="2000b-179">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2000b-179">Next steps</span></span>

<span data-ttu-id="2000b-180">Megtudhatja, hogyan tooconfigure diagnosztikai naplózás, vagy látogasson el a webalkalmazási tűzfal megakadályozta toolog hello események [Alkalmazásdiagnosztika átjáró](application-gateway-diagnostics.md)</span><span class="sxs-lookup"><span data-stu-id="2000b-180">Learn how tooconfigure diagnostic logging, toolog hello events that are detected or prevented with web application firewall by visiting [Application Gateway Diagnostics](application-gateway-diagnostics.md)</span></span>

[scenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png
