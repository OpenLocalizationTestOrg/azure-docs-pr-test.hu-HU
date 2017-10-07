---
title: "egyéni aaaCreate mintavételi - Azure Application Gateway - PowerShell |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egyéni mintavételi az Alkalmazásátjáró PowerShell erőforrás-kezelő használatával"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 68feb660-7fa4-4f69-a7e4-bdd7bdc474db
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: gwallace
ms.openlocfilehash: 44c9ffa75401d6d0db023e66fa82c701fb0cf8bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-probe-for-azure-application-gateway-by-using-powershell-for-azure-resource-manager"></a><span data-ttu-id="ffb4c-103">Egy egyéni mintavétel létrehozása az Azure Application Gateway az Azure Resource Manager PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-103">Create a custom probe for Azure Application Gateway by using PowerShell for Azure Resource Manager</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="ffb4c-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ffb4c-104">Azure portal</span></span>](application-gateway-create-probe-portal.md)
> * [<span data-ttu-id="ffb4c-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="ffb4c-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-probe-ps.md)
> * [<span data-ttu-id="ffb4c-106">Klasszikus Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="ffb4c-106">Azure Classic PowerShell</span></span>](application-gateway-create-probe-classic-ps.md)

<span data-ttu-id="ffb4c-107">Ebben a cikkben egy egyéni mintavételi tooan meglévő Alkalmazásátjáró a PowerShell használatával adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-107">In this article, you add a custom probe tooan existing application gateway with PowerShell.</span></span> <span data-ttu-id="ffb4c-108">Egyéni mintavételt hasznosak, az alkalmazásokat, amelyek egy adott állapotának ellenőrzése lapon vagy az alkalmazásokat, amelyek nem a sikeres válasz hello alapértelmezett webalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-108">Custom probes are useful for applications that have a specific health check page or for applications that do not provide a successful response on hello default web application.</span></span>

> [!NOTE]
> <span data-ttu-id="ffb4c-109">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="ffb4c-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="ffb4c-110">Ez a cikk ismerteti a használatával a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello hello Resource Manager üzembe helyezési modellben [klasszikus üzembe helyezési modellel](application-gateway-create-probe-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="ffb4c-110">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello [classic deployment model](application-gateway-create-probe-classic-ps.md).</span></span>

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway-with-a-custom-probe"></a><span data-ttu-id="ffb4c-111">Hozzon létre egy alkalmazás egy egyéni mintavétel</span><span class="sxs-lookup"><span data-stu-id="ffb4c-111">Create an application gateway with a custom probe</span></span>

### <a name="sign-in-and-create-resource-group"></a><span data-ttu-id="ffb4c-112">Jelentkezzen be, és az erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="ffb4c-112">Sign in and create resource group</span></span>

1. <span data-ttu-id="ffb4c-113">Használjon `Login-AzureRmAccount` tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-113">Use `Login-AzureRmAccount` tooauthenticate.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

1. <span data-ttu-id="ffb4c-114">Hello előfizetések hello fiók lekérése.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-114">Get hello subscriptions for hello account.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```

1. <span data-ttu-id="ffb4c-115">Válassza ki, amely az Azure-előfizetések toouse.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-115">Choose which of your Azure subscriptions toouse.</span></span>

  ```powershell
  Select-AzureRmSubscription -Subscriptionid '{subscriptionGuid}'
  ```

1. <span data-ttu-id="ffb4c-116">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-116">Create a resource group.</span></span> <span data-ttu-id="ffb4c-117">Ezt a lépést kihagyhatja, ha egy meglévő erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-117">You can skip this step if you have an existing resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name appgw-rg -Location 'West US'
  ```

<span data-ttu-id="ffb4c-118">Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-118">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="ffb4c-119">Ezen a helyen az erőforráscsoport erőforrások lesz hello alapértelmezett helye.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-119">This location is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="ffb4c-120">Győződjön meg arról, hogy az összes parancsok toocreate egy alkalmazás átjáró használata hello ugyanabban az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-120">Make sure that all commands toocreate an application gateway use hello same resource group.</span></span>

<span data-ttu-id="ffb4c-121">A fenti példa hello, nevű erőforráscsoport létrehozott **appgw-RG** helyen **USA nyugati régiója**.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-121">In hello preceding example, we created a resource group called **appgw-RG** in location **West US**.</span></span>

### <a name="create-a-virtual-network-and-a-subnet"></a><span data-ttu-id="ffb4c-122">Hozzon létre egy virtuális hálózatot és egy alhálózatot</span><span class="sxs-lookup"><span data-stu-id="ffb4c-122">Create a virtual network and a subnet</span></span>

<span data-ttu-id="ffb4c-123">hello alábbi példa létrehoz egy virtuális hálózatot és egy hello alkalmazás átjáró-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-123">hello following example creates a virtual network and a subnet for hello application gateway.</span></span> <span data-ttu-id="ffb4c-124">Alkalmazásátjáró saját alhálózatba használatát igényli.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-124">Application gateway requires its own subnet for use.</span></span> <span data-ttu-id="ffb4c-125">Emiatt az Alkalmazásátjáró hello létrehozott hello alhálózati kisebb, mint a más alhálózatokon toobe létrehozott és használt hello VNET tooallow hello címterében kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-125">For this reason, hello subnet created for hello application gateway should be smaller than hello address space of hello VNET tooallow for other subnets toobe created and used.</span></span>

```powershell
# Assign hello address range 10.0.0.0/24 tooa subnet variable toobe used toocreate a virtual network.
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

# Create a virtual network named appgwvnet in resource group appgw-rg for hello West US region using hello prefix 10.0.0.0/16 with subnet 10.0.0.0/24.
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet

# Assign a subnet variable for hello next steps, which create an application gateway.
$subnet = $vnet.Subnets[0]
```

### <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a><span data-ttu-id="ffb4c-126">A nyilvános IP-cím hello előtér-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="ffb4c-126">Create a public IP address for hello front-end configuration</span></span>

<span data-ttu-id="ffb4c-127">Hozzon létre egy nyilvános IP-erőforrás **publicIP01** erőforráscsoportban **appgw-rg** hello USA nyugati régiójában.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-127">Create a public IP resource **publicIP01** in resource group **appgw-rg** for hello West US region.</span></span> <span data-ttu-id="ffb4c-128">A példa egy nyilvános IP-cím hello Alkalmazásátjáró hello előtér-IP-címhez.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-128">This example uses a public IP address for hello front-end IP address of hello application gateway.</span></span>  <span data-ttu-id="ffb4c-129">Alkalmazás-átjáró által igényelt hello nyilvános IP cím toohave dinamikusan létrehozott DNS-név ezért hello `-DomainNameLabel` hello hello nyilvános IP-cím létrehozása közben nem adható meg.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-129">Application gateway requires hello public IP address toohave a dynamically created DNS name therefore hello `-DomainNameLabel` cannot be specified during hello creation of hello public IP address.</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name publicIP01 -Location 'West US' -AllocationMethod Dynamic
```

### <a name="create-an-application-gateway"></a><span data-ttu-id="ffb4c-130">Application Gateway létrehozása</span><span class="sxs-lookup"><span data-stu-id="ffb4c-130">Create an application gateway</span></span>

<span data-ttu-id="ffb4c-131">Az összes konfigurációs elemek beállítása hello Alkalmazásátjáró létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-131">You set up all configuration items before creating hello application gateway.</span></span> <span data-ttu-id="ffb4c-132">hello alábbi példakód létrehozza hello konfigurációs elemek, amelyek szükségesek az alkalmazás átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-132">hello following example creates hello configuration items that are needed for an application gateway resource.</span></span>

| <span data-ttu-id="ffb4c-133">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="ffb4c-133">**Component**</span></span> | <span data-ttu-id="ffb4c-134">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="ffb4c-134">**Description**</span></span> |
|---|---|
| <span data-ttu-id="ffb4c-135">**Átjáró IP-konfiguráció**</span><span class="sxs-lookup"><span data-stu-id="ffb4c-135">**Gateway IP configuration**</span></span> | <span data-ttu-id="ffb4c-136">Egy alkalmazás átjáró IP-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-136">An IP configuration for an application gateway.</span></span>|
| <span data-ttu-id="ffb4c-137">**Háttérkészlet**</span><span class="sxs-lookup"><span data-stu-id="ffb4c-137">**Backend pool**</span></span> | <span data-ttu-id="ffb4c-138">IP-címek, FQDN-eknek vagy hálózati adapterrel, amelyek toohello alkalmazás hello webalkalmazást működtető kiszolgálók készlete</span><span class="sxs-lookup"><span data-stu-id="ffb4c-138">A pool of IP addresses, FQDN's, or NICs that are toohello application servers that host hello web application</span></span>|
| <span data-ttu-id="ffb4c-139">**Állapotmintáihoz**</span><span class="sxs-lookup"><span data-stu-id="ffb4c-139">**Health probe**</span></span> | <span data-ttu-id="ffb4c-140">Egyéni tesztműveleti használt hello háttér a készlet tagjainak toomonitor hello állapotát</span><span class="sxs-lookup"><span data-stu-id="ffb4c-140">A custom probe used toomonitor hello health of hello backend pool members</span></span>|
| <span data-ttu-id="ffb4c-141">**HTTP-beállítások**</span><span class="sxs-lookup"><span data-stu-id="ffb4c-141">**HTTP settings**</span></span> | <span data-ttu-id="ffb4c-142">Beleértve, port, protokoll, affinitási cookie-alapú, mintavételi és időtúllépés gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-142">A collection of settings including, port, protocol, cookie-based affinity, probe, and timeout.</span></span>  <span data-ttu-id="ffb4c-143">Ezeket a beállításokat annak megállapítása, hogy forgalom irányított toohello háttér a készlet tagjainak</span><span class="sxs-lookup"><span data-stu-id="ffb4c-143">These settings determine how traffic is routed toohello backend pool members</span></span>|
| <span data-ttu-id="ffb4c-144">**Az elülső rétegbeli portot**</span><span class="sxs-lookup"><span data-stu-id="ffb4c-144">**Frontend port**</span></span> | <span data-ttu-id="ffb4c-145">Alkalmazásátjáró hello hello portot figyeli a forgalmat a</span><span class="sxs-lookup"><span data-stu-id="ffb4c-145">hello port that hello application gateway listens for traffic on</span></span>|
| <span data-ttu-id="ffb4c-146">**Figyelő**</span><span class="sxs-lookup"><span data-stu-id="ffb4c-146">**Listener**</span></span> | <span data-ttu-id="ffb4c-147">A protokoll előtérbeli IP-konfigurációja és az elülső rétegbeli portot kombinációja.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-147">A combination of a protocol, frontend IP configuration, and frontend port.</span></span> <span data-ttu-id="ffb4c-148">Ez az, hogy mi a bejövő kéréseket figyeli.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-148">This is what listens for incoming requests.</span></span>
|<span data-ttu-id="ffb4c-149">**A szabály**</span><span class="sxs-lookup"><span data-stu-id="ffb4c-149">**Rule**</span></span>| <span data-ttu-id="ffb4c-150">Útvonalak hello forgalom toohello megfelelő háttér HTTP-beállításai alapján.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-150">Routes hello traffic toohello appropriate backend based on HTTP settings.</span></span>|

```powershell
# Creates a application gateway Frontend IP configuration named gatewayIP01
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

#Creates a back-end IP address pool named pool01 with IP addresses 134.170.185.46, 134.170.188.221, 134.170.185.50.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

# Creates a probe that will check health at http://contoso.com/path/path.htm
$probe = New-AzureRmApplicationGatewayProbeConfig -Name probe01 -Protocol Http -HostName 'contoso.com' -Path '/path/path.htm' -Interval 30 -Timeout 120 -UnhealthyThreshold 8

# Creates hello backend http settings toobe used. This component references hello $probe created in hello previous command.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 80

# Creates a frontend port for hello application gateway toolisten on port 80 that will be used by hello listener.
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01 -Port 80

# Creates a frontend IP configuration. This associates hello $publicip variable defined previously with hello front-end IP that will be used by hello listener.
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

# Creates hello listener. hello listener is a combination of protocol and hello frontend IP configuration $fipconfig and frontend port $fp created in previous steps.
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

# Creates hello rule that routes traffic toohello backend pools.  In this example we create a basic rule that uses hello previous defined http settings and backend address pool.  It also associates hello listener toohello rule
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Sets hello SKU of hello application gateway, in this example we create a small standard application gateway with 2 instances.
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

# hello final step creates hello application gateway with all hello previously defined components.
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location 'West US' -BackendAddressPools $pool -Probes $probe -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

## <a name="add-a-probe-tooan-existing-application-gateway"></a><span data-ttu-id="ffb4c-151">A mintavétel tooan meglévő Alkalmazásátjáró hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ffb4c-151">Add a probe tooan existing application gateway</span></span>

<span data-ttu-id="ffb4c-152">hello következő kódrészletet mintavételi tooan meglévő alkalmazás átjárót ad.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-152">hello following code snippet adds a probe tooan existing application gateway.</span></span>

```powershell
# Load hello application gateway resource into a PowerShell variable by using Get-AzureRmApplicationGateway.
$getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

# Create hello probe object that will check health at http://contoso.com/path/path.htm
$getgw = Add-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name probe01 -Protocol Http -HostName 'contoso.com' -Path '/path/custompath.htm' -Interval 30 -Timeout 120 -UnhealthyThreshold 8

# Set hello backend HTTP settings toouse hello new probe
$getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 120

# Save hello application gateway with hello configuration changes
Set-AzureRmApplicationGateway -ApplicationGateway $getgw
```

## <a name="remove-a-probe-from-an-existing-application-gateway"></a><span data-ttu-id="ffb4c-153">Távolítsa el a Hálózatfigyelő a meglévő Alkalmazásátjáró</span><span class="sxs-lookup"><span data-stu-id="ffb4c-153">Remove a probe from an existing application gateway</span></span>

<span data-ttu-id="ffb4c-154">a következő kódrészletet hello vizsgálatok eltávolítja a meglévő Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-154">hello following code snippet removes a probe from an existing application gateway.</span></span>

```powershell
# Load hello application gateway resource into a PowerShell variable by using Get-AzureRmApplicationGateway.
$getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

# Remove hello probe from hello application gateway configuration object
$getgw = Remove-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name $getgw.Probes.name

# Set hello backend HTTP settings tooremove hello reference toohello probe. hello backend http settings now use hello default probe
$getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol http -CookieBasedAffinity Disabled

# Save hello application gateway with hello configuration changes
Set-AzureRmApplicationGateway -ApplicationGateway $getgw
```

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="ffb4c-155">Az Application Gateway DNS-nevének beszerzése</span><span class="sxs-lookup"><span data-stu-id="ffb4c-155">Get application gateway DNS name</span></span>

<span data-ttu-id="ffb4c-156">Hello átjáró létrehozása után hello következő lépésre tooconfigure hello előtér-kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-156">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="ffb4c-157">Nyilvános IP-cím esetén az Application Gateway használatához dinamikusan hozzárendelt DNS-névre van szükség, amely nem valódi név.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-157">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="ffb4c-158">tooensure végfelhasználók hello Alkalmazásátjáró használhatók egy olyan CNAME rekordot is találati toopoint toohello nyilvános végpontot hello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-158">tooensure end users can hit hello application gateway a CNAME record can be used toopoint toohello public endpoint of hello application gateway.</span></span> <span data-ttu-id="ffb4c-159">[Egyéni tartománynév konfigurálása az Azure-ban](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ffb4c-159">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="ffb4c-160">toodo a, hello Alkalmazásátjáró és a társított IP-/ DNS-nevét, hello PublicIPAddress elem csatolt toohello Alkalmazásátjáró beolvasása részleteit.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-160">toodo this, retrieve details of hello application gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello application gateway.</span></span> <span data-ttu-id="ffb4c-161">hello alkalmazás átjáró DNS-névnek kell lennie a használt toocreate egy olyan CNAME rekordot pontok hello két webes alkalmazások toothis DNS-név.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-161">hello application gateway's DNS name should be used toocreate a CNAME record, which points hello two web applications toothis DNS name.</span></span> <span data-ttu-id="ffb4c-162">A-rekordok hello használata nem javasolt, mert hello VIP módosíthatja az Alkalmazásátjáró újra kell indítani.</span><span class="sxs-lookup"><span data-stu-id="ffb4c-162">hello use of A-records is not recommended since hello VIP may change on restart of application gateway.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="ffb4c-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ffb4c-163">Next steps</span></span>

<span data-ttu-id="ffb4c-164">Ismerje meg, tooconfigure SSL kiszervezésével ellátogatva: [SSL-kiszervezés konfigurálása](application-gateway-ssl-arm.md)</span><span class="sxs-lookup"><span data-stu-id="ffb4c-164">Learn tooconfigure SSL offloading by visiting: [Configure SSL Offload](application-gateway-ssl-arm.md)</span></span>

