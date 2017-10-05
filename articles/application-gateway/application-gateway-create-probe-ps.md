---
title: "Hozzon létre egy egyéni mintavételt - Azure Application Gateway - PowerShell |} Microsoft Docs"
description: "Megtudhatja, hogyan hozzon létre egy egyéni mintavétel az Alkalmazásátjáró PowerShell erőforrás-kezelő használatával"
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
ms.openlocfilehash: b54fe5267d87a41eb9e81d5d1dc9b1b16c5c5e88
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-custom-probe-for-azure-application-gateway-by-using-powershell-for-azure-resource-manager"></a><span data-ttu-id="253c5-103">Egy egyéni mintavétel létrehozása az Azure Application Gateway az Azure Resource Manager PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="253c5-103">Create a custom probe for Azure Application Gateway by using PowerShell for Azure Resource Manager</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="253c5-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="253c5-104">Azure portal</span></span>](application-gateway-create-probe-portal.md)
> * [<span data-ttu-id="253c5-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="253c5-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-probe-ps.md)
> * [<span data-ttu-id="253c5-106">Klasszikus Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="253c5-106">Azure Classic PowerShell</span></span>](application-gateway-create-probe-classic-ps.md)

<span data-ttu-id="253c5-107">Ebben a cikkben ad hozzá egy egyéni mintavételt meglévő Alkalmazásátjáró a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="253c5-107">In this article, you add a custom probe to an existing application gateway with PowerShell.</span></span> <span data-ttu-id="253c5-108">Egyéni mintavételt az alkalmazásokat, amelyek egy adott állapotának ellenőrzése lapon vagy az alapértelmezett webes alkalmazás a sikeres válasz nem biztosító alkalmazások hasznosak.</span><span class="sxs-lookup"><span data-stu-id="253c5-108">Custom probes are useful for applications that have a specific health check page or for applications that do not provide a successful response on the default web application.</span></span>

> [!NOTE]
> <span data-ttu-id="253c5-109">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="253c5-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="253c5-110">Ez a cikk a Resource Manager-alapú üzemi modell használatát ismerteti, amelyet a Microsoft a legtöbb új telepítéshez a [klasszikus üzemi modell](application-gateway-create-probe-classic-ps.md) helyett javasol.</span><span class="sxs-lookup"><span data-stu-id="253c5-110">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the [classic deployment model](application-gateway-create-probe-classic-ps.md).</span></span>

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway-with-a-custom-probe"></a><span data-ttu-id="253c5-111">Hozzon létre egy alkalmazás egy egyéni mintavétel</span><span class="sxs-lookup"><span data-stu-id="253c5-111">Create an application gateway with a custom probe</span></span>

### <a name="sign-in-and-create-resource-group"></a><span data-ttu-id="253c5-112">Jelentkezzen be, és az erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="253c5-112">Sign in and create resource group</span></span>

1. <span data-ttu-id="253c5-113">Használjon `Login-AzureRmAccount` hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="253c5-113">Use `Login-AzureRmAccount` to authenticate.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

1. <span data-ttu-id="253c5-114">A fiókhoz tartozó előfizetések beolvasása.</span><span class="sxs-lookup"><span data-stu-id="253c5-114">Get the subscriptions for the account.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```

1. <span data-ttu-id="253c5-115">Válassza ki, hogy melyik Azure előfizetést fogja használni.</span><span class="sxs-lookup"><span data-stu-id="253c5-115">Choose which of your Azure subscriptions to use.</span></span>

  ```powershell
  Select-AzureRmSubscription -Subscriptionid '{subscriptionGuid}'
  ```

1. <span data-ttu-id="253c5-116">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="253c5-116">Create a resource group.</span></span> <span data-ttu-id="253c5-117">Ezt a lépést kihagyhatja, ha egy meglévő erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="253c5-117">You can skip this step if you have an existing resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name appgw-rg -Location 'West US'
  ```

<span data-ttu-id="253c5-118">Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet.</span><span class="sxs-lookup"><span data-stu-id="253c5-118">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="253c5-119">Ez a hely lesz az erőforráscsoport erőforrásainak alapértelmezett helye.</span><span class="sxs-lookup"><span data-stu-id="253c5-119">This location is used as the default location for resources in that resource group.</span></span> <span data-ttu-id="253c5-120">Győződjön meg arról, hogy minden parancs Alkalmazásátjáró létrehozása ugyanabban az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="253c5-120">Make sure that all commands to create an application gateway use the same resource group.</span></span>

<span data-ttu-id="253c5-121">Az előző példában létrehozott nevű erőforráscsoport **appgw-RG** helyen **USA nyugati régiója**.</span><span class="sxs-lookup"><span data-stu-id="253c5-121">In the preceding example, we created a resource group called **appgw-RG** in location **West US**.</span></span>

### <a name="create-a-virtual-network-and-a-subnet"></a><span data-ttu-id="253c5-122">Hozzon létre egy virtuális hálózatot és egy alhálózatot</span><span class="sxs-lookup"><span data-stu-id="253c5-122">Create a virtual network and a subnet</span></span>

<span data-ttu-id="253c5-123">Az alábbi példakód létrehozza a virtuális hálózat és az Alkalmazásátjáró alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="253c5-123">The following example creates a virtual network and a subnet for the application gateway.</span></span> <span data-ttu-id="253c5-124">Alkalmazásátjáró saját alhálózatba használatát igényli.</span><span class="sxs-lookup"><span data-stu-id="253c5-124">Application gateway requires its own subnet for use.</span></span> <span data-ttu-id="253c5-125">Emiatt az Alkalmazásátjáró létrehozott alhálózati kisebb, mint a vnet, hogy más alhálózatok létrehozott és használt lehetővé a címterületen belülre kell lennie.</span><span class="sxs-lookup"><span data-stu-id="253c5-125">For this reason, the subnet created for the application gateway should be smaller than the address space of the VNET to allow for other subnets to be created and used.</span></span>

```powershell
# Assign the address range 10.0.0.0/24 to a subnet variable to be used to create a virtual network.
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

# Create a virtual network named appgwvnet in resource group appgw-rg for the West US region using the prefix 10.0.0.0/16 with subnet 10.0.0.0/24.
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet

# Assign a subnet variable for the next steps, which create an application gateway.
$subnet = $vnet.Subnets[0]
```

### <a name="create-a-public-ip-address-for-the-front-end-configuration"></a><span data-ttu-id="253c5-126">Nyilvános IP-cím létrehozása az előtérbeli konfigurációhoz</span><span class="sxs-lookup"><span data-stu-id="253c5-126">Create a public IP address for the front-end configuration</span></span>

<span data-ttu-id="253c5-127">Hozzon létre egy **publicIP01** nevű, nyilvános IP-címhez tartozó erőforrást az **appgw-rg** nevű erőforráscsoportban, az USA nyugati régiójában.</span><span class="sxs-lookup"><span data-stu-id="253c5-127">Create a public IP resource **publicIP01** in resource group **appgw-rg** for the West US region.</span></span> <span data-ttu-id="253c5-128">Ebben a példában az Alkalmazásátjáró előtér-IP-címét egy nyilvános IP-címet használja.</span><span class="sxs-lookup"><span data-stu-id="253c5-128">This example uses a public IP address for the front-end IP address of the application gateway.</span></span>  <span data-ttu-id="253c5-129">Alkalmazásátjáró igényel a nyilvános IP-cím, ezért rendelkeznie egy dinamikusan létrehozott DNS-nevet a `-DomainNameLabel` a nyilvános IP-cím létrehozása közben nem adható meg.</span><span class="sxs-lookup"><span data-stu-id="253c5-129">Application gateway requires the public IP address to have a dynamically created DNS name therefore the `-DomainNameLabel` cannot be specified during the creation of the public IP address.</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name publicIP01 -Location 'West US' -AllocationMethod Dynamic
```

### <a name="create-an-application-gateway"></a><span data-ttu-id="253c5-130">Application Gateway létrehozása</span><span class="sxs-lookup"><span data-stu-id="253c5-130">Create an application gateway</span></span>

<span data-ttu-id="253c5-131">Az Alkalmazásátjáró létrehozása előtt beállítása összes konfigurációs elemet.</span><span class="sxs-lookup"><span data-stu-id="253c5-131">You set up all configuration items before creating the application gateway.</span></span> <span data-ttu-id="253c5-132">Az alábbi példakód létrehozza a konfigurációs elemek, amelyek szükségesek az alkalmazás átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="253c5-132">The following example creates the configuration items that are needed for an application gateway resource.</span></span>

| <span data-ttu-id="253c5-133">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="253c5-133">**Component**</span></span> | <span data-ttu-id="253c5-134">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="253c5-134">**Description**</span></span> |
|---|---|
| <span data-ttu-id="253c5-135">**Átjáró IP-konfiguráció**</span><span class="sxs-lookup"><span data-stu-id="253c5-135">**Gateway IP configuration**</span></span> | <span data-ttu-id="253c5-136">Egy alkalmazás átjáró IP-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="253c5-136">An IP configuration for an application gateway.</span></span>|
| <span data-ttu-id="253c5-137">**Háttérkészlet**</span><span class="sxs-lookup"><span data-stu-id="253c5-137">**Backend pool**</span></span> | <span data-ttu-id="253c5-138">IP-címek, FQDN-eknek vagy, a webszolgáltatási alkalmazás üzemeltetéséhez alkalmazáskiszolgálókra két hálózati adapterrel áll</span><span class="sxs-lookup"><span data-stu-id="253c5-138">A pool of IP addresses, FQDN's, or NICs that are to the application servers that host the web application</span></span>|
| <span data-ttu-id="253c5-139">**Állapotmintáihoz**</span><span class="sxs-lookup"><span data-stu-id="253c5-139">**Health probe**</span></span> | <span data-ttu-id="253c5-140">Egy egyéni mintavételt a háttérrendszer a készlet tagjainak állapotának figyelésére szolgál</span><span class="sxs-lookup"><span data-stu-id="253c5-140">A custom probe used to monitor the health of the backend pool members</span></span>|
| <span data-ttu-id="253c5-141">**HTTP-beállítások**</span><span class="sxs-lookup"><span data-stu-id="253c5-141">**HTTP settings**</span></span> | <span data-ttu-id="253c5-142">Beleértve, port, protokoll, affinitási cookie-alapú, mintavételi és időtúllépés gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="253c5-142">A collection of settings including, port, protocol, cookie-based affinity, probe, and timeout.</span></span>  <span data-ttu-id="253c5-143">Ezek a beállítások határozzák meg, hogyan továbbítódik a háttér címkészletet tagok</span><span class="sxs-lookup"><span data-stu-id="253c5-143">These settings determine how traffic is routed to the backend pool members</span></span>|
| <span data-ttu-id="253c5-144">**Az elülső rétegbeli portot**</span><span class="sxs-lookup"><span data-stu-id="253c5-144">**Frontend port**</span></span> | <span data-ttu-id="253c5-145">A port, amelyet figyeli az Alkalmazásátjáró forgalom</span><span class="sxs-lookup"><span data-stu-id="253c5-145">The port that the application gateway listens for traffic on</span></span>|
| <span data-ttu-id="253c5-146">**Figyelő**</span><span class="sxs-lookup"><span data-stu-id="253c5-146">**Listener**</span></span> | <span data-ttu-id="253c5-147">A protokoll előtérbeli IP-konfigurációja és az elülső rétegbeli portot kombinációja.</span><span class="sxs-lookup"><span data-stu-id="253c5-147">A combination of a protocol, frontend IP configuration, and frontend port.</span></span> <span data-ttu-id="253c5-148">Ez az, hogy mi a bejövő kéréseket figyeli.</span><span class="sxs-lookup"><span data-stu-id="253c5-148">This is what listens for incoming requests.</span></span>
|<span data-ttu-id="253c5-149">**A szabály**</span><span class="sxs-lookup"><span data-stu-id="253c5-149">**Rule**</span></span>| <span data-ttu-id="253c5-150">Útvonalak a forgalmat a megfelelő háttér HTTP-beállításai alapján.</span><span class="sxs-lookup"><span data-stu-id="253c5-150">Routes the traffic to the appropriate backend based on HTTP settings.</span></span>|

```powershell
# Creates a application gateway Frontend IP configuration named gatewayIP01
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

#Creates a back-end IP address pool named pool01 with IP addresses 134.170.185.46, 134.170.188.221, 134.170.185.50.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

# Creates a probe that will check health at http://contoso.com/path/path.htm
$probe = New-AzureRmApplicationGatewayProbeConfig -Name probe01 -Protocol Http -HostName 'contoso.com' -Path '/path/path.htm' -Interval 30 -Timeout 120 -UnhealthyThreshold 8

# Creates the backend http settings to be used. This component references the $probe created in the previous command.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 80

# Creates a frontend port for the application gateway to listen on port 80 that will be used by the listener.
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01 -Port 80

# Creates a frontend IP configuration. This associates the $publicip variable defined previously with the front-end IP that will be used by the listener.
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

# Creates the listener. The listener is a combination of protocol and the frontend IP configuration $fipconfig and frontend port $fp created in previous steps.
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

# Creates the rule that routes traffic to the backend pools.  In this example we create a basic rule that uses the previous defined http settings and backend address pool.  It also associates the listener to the rule
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Sets the SKU of the application gateway, in this example we create a small standard application gateway with 2 instances.
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

# The final step creates the application gateway with all the previously defined components.
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location 'West US' -BackendAddressPools $pool -Probes $probe -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

## <a name="add-a-probe-to-an-existing-application-gateway"></a><span data-ttu-id="253c5-151">A mintavétel hozzáadása egy meglévő Alkalmazásátjáró</span><span class="sxs-lookup"><span data-stu-id="253c5-151">Add a probe to an existing application gateway</span></span>

<span data-ttu-id="253c5-152">A következő kódrészletet a mintavételi hozzáadása egy meglévő Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="253c5-152">The following code snippet adds a probe to an existing application gateway.</span></span>

```powershell
# Load the application gateway resource into a PowerShell variable by using Get-AzureRmApplicationGateway.
$getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

# Create the probe object that will check health at http://contoso.com/path/path.htm
$getgw = Add-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name probe01 -Protocol Http -HostName 'contoso.com' -Path '/path/custompath.htm' -Interval 30 -Timeout 120 -UnhealthyThreshold 8

# Set the backend HTTP settings to use the new probe
$getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 120

# Save the application gateway with the configuration changes
Set-AzureRmApplicationGateway -ApplicationGateway $getgw
```

## <a name="remove-a-probe-from-an-existing-application-gateway"></a><span data-ttu-id="253c5-153">Távolítsa el a Hálózatfigyelő a meglévő Alkalmazásátjáró</span><span class="sxs-lookup"><span data-stu-id="253c5-153">Remove a probe from an existing application gateway</span></span>

<span data-ttu-id="253c5-154">A következő kódrészletet a mintavételi eltávolítja a meglévő Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="253c5-154">The following code snippet removes a probe from an existing application gateway.</span></span>

```powershell
# Load the application gateway resource into a PowerShell variable by using Get-AzureRmApplicationGateway.
$getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

# Remove the probe from the application gateway configuration object
$getgw = Remove-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name $getgw.Probes.name

# Set the backend HTTP settings to remove the reference to the probe. The backend http settings now use the default probe
$getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol http -CookieBasedAffinity Disabled

# Save the application gateway with the configuration changes
Set-AzureRmApplicationGateway -ApplicationGateway $getgw
```

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="253c5-155">Az Application Gateway DNS-nevének beszerzése</span><span class="sxs-lookup"><span data-stu-id="253c5-155">Get application gateway DNS name</span></span>

<span data-ttu-id="253c5-156">Az átjáró létrehozása után a következő lépés a kommunikációra szolgáló előtér konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="253c5-156">Once the gateway is created, the next step is to configure the front end for communication.</span></span> <span data-ttu-id="253c5-157">Nyilvános IP-cím esetén az Application Gateway használatához dinamikusan hozzárendelt DNS-névre van szükség, amely nem valódi név.</span><span class="sxs-lookup"><span data-stu-id="253c5-157">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="253c5-158">Ha szeretné, hogy a végfelhasználók elérjék az Application Gatewayt, használjon egy Application Gateway nyilvános végpontjára mutató CNAME-rekordot.</span><span class="sxs-lookup"><span data-stu-id="253c5-158">To ensure end users can hit the application gateway a CNAME record can be used to point to the public endpoint of the application gateway.</span></span> <span data-ttu-id="253c5-159">[Egyéni tartománynév konfigurálása az Azure-ban](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="253c5-159">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="253c5-160">A művelet végrehajtásához az Application Gateway részleteinek beszerzésére és a kapcsolódó IP/DNS-név lekérésére van szükség az Application Gatewayhez csatolt PublicIPAddress használatával.</span><span class="sxs-lookup"><span data-stu-id="253c5-160">To do this, retrieve details of the application gateway and its associated IP/DNS name using the PublicIPAddress element attached to the application gateway.</span></span> <span data-ttu-id="253c5-161">Az Application Gateway DNS-nevének használatával létrehozhat egy CNAME rekordot, amely a két webalkalmazást erre a DNS-névre irányítja.</span><span class="sxs-lookup"><span data-stu-id="253c5-161">The application gateway's DNS name should be used to create a CNAME record, which points the two web applications to this DNS name.</span></span> <span data-ttu-id="253c5-162">Az A-bejegyzések használata nem javasolt, mivel a virtuális IP-cím változhat az Application Gateway újraindításakor.</span><span class="sxs-lookup"><span data-stu-id="253c5-162">The use of A-records is not recommended since the VIP may change on restart of application gateway.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="253c5-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="253c5-163">Next steps</span></span>

<span data-ttu-id="253c5-164">Ismerkedjen meg az SSL-feladatkiszervezést ellátogatva konfigurálása: [SSL-kiszervezés konfigurálása](application-gateway-ssl-arm.md)</span><span class="sxs-lookup"><span data-stu-id="253c5-164">Learn to configure SSL offloading by visiting: [Configure SSL Offload](application-gateway-ssl-arm.md)</span></span>

