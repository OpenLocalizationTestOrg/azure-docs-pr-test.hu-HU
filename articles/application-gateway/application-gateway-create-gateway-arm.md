---
title: "Azure Application Gateway létrehozása és kezelése – PowerShell | Microsoft Docs"
description: "Ez a lap utasításokat tartalmaz egy Azure Application Gateway Azure Resource Manager segítségével történő létrehozásához, konfigurálásához, indításához és törléséhez"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: 5f1713365406764998de505ff62309bab9fa2567
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a><span data-ttu-id="c767a-103">Application Gateway létrehozása, indítása vagy törlése az Azure Resource Manager használatával</span><span class="sxs-lookup"><span data-stu-id="c767a-103">Create, start, or delete an application gateway by using Azure Resource Manager</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="c767a-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c767a-104">Azure portal</span></span>](application-gateway-create-gateway-portal.md)
> * [<span data-ttu-id="c767a-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="c767a-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-gateway-arm.md)
> * [<span data-ttu-id="c767a-106">Klasszikus Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="c767a-106">Azure Classic PowerShell</span></span>](application-gateway-create-gateway.md)
> * [<span data-ttu-id="c767a-107">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="c767a-107">Azure Resource Manager template</span></span>](application-gateway-create-gateway-arm-template.md)
> * [<span data-ttu-id="c767a-108">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c767a-108">Azure CLI</span></span>](application-gateway-create-gateway-cli.md)

<span data-ttu-id="c767a-109">Az Azure Application Gateway egy 7. rétegbeli terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="c767a-109">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="c767a-110">Feladatátvételt és teljesítményalapú útválasztást biztosít a HTTP-kérelmek számára különböző kiszolgálók között, függetlenül attól, hogy a felhőben vagy a helyszínen találhatóak.</span><span class="sxs-lookup"><span data-stu-id="c767a-110">It provides failover and performance-routing HTTP requests between different servers, whether they are on the cloud or on-premises.</span></span> <span data-ttu-id="c767a-111">Az Application Gateway számos alkalmazáskézbesítési vezérlőszolgáltatást (ADC) biztosít, beleértve a HTTP-terheléselosztást, a cookie-alapú munkamenet-affinitást, a Secure Sockets Layer- (SSL-) alapú kiszervezést, az egyéni állapotmintákat, a többhelyes támogatást és még sok mást.</span><span class="sxs-lookup"><span data-stu-id="c767a-111">Application Gateway provides many application delivery controller (ADC) features including HTTP load balancing, cookie-based session affinity, Secure Sockets Layer (SSL) offload, custom health probes, support for multi-site, and many others.</span></span> <span data-ttu-id="c767a-112">A támogatott szolgáltatások teljes listájáért látogasson el [az Application Gateway áttekintését biztosító](application-gateway-introduction.md) oldalra.</span><span class="sxs-lookup"><span data-stu-id="c767a-112">To find a complete list of supported features, visit [Application Gateway overview](application-gateway-introduction.md).</span></span>

<span data-ttu-id="c767a-113">Ez a cikk részletesen ismerteti a lépéseket, amelyekkel létrehozhat, konfigurálhat, elindíthat és törölhet egy Application Gateway-t.</span><span class="sxs-lookup"><span data-stu-id="c767a-113">This article walks you through the steps to create, configure, start, and delete an application gateway.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c767a-114">Az Azure-erőforrásokkal való munka megkezdése előtt fontos megérteni, hogy az Azure jelenleg két üzembe helyezési modellel rendelkezik, a Resource Managerrel és a klasszikussal.</span><span class="sxs-lookup"><span data-stu-id="c767a-114">Before you work with Azure resources, it is important to understand that Azure currently has two deployment models: Resource Manager and classic.</span></span> <span data-ttu-id="c767a-115">Bizonyosodjon meg arról, hogy megfelelő ismeretekkel rendelkezik az [üzembe helyezési modellekről és eszközökről](../azure-classic-rm.md), mielőtt elkezdene dolgozni az Azure-erőforrásokkal.</span><span class="sxs-lookup"><span data-stu-id="c767a-115">Make sure that you understand [deployment models and tools](../azure-classic-rm.md) before working with any Azure resource.</span></span> <span data-ttu-id="c767a-116">A különféle eszközök dokumentációit a cikk tetején található fülekre kattintva tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="c767a-116">You can view the documentation for different tools by clicking the tabs at the top of this article.</span></span> <span data-ttu-id="c767a-117">Jelen dokumentum az Application Gateway az Azure Resource Manager használatával történő létrehozását ismerteti.</span><span class="sxs-lookup"><span data-stu-id="c767a-117">This document covers creating an application gateway by using Azure Resource Manager.</span></span> <span data-ttu-id="c767a-118">A klasszikus verzió használatához lépjen az [Application Gateway klasszikus üzembe helyezéssel történő létrehozása a PowerShell használatával](application-gateway-create-gateway.md) című szakaszra.</span><span class="sxs-lookup"><span data-stu-id="c767a-118">To use the classic version, go to [Create an application gateway classic deployment by using PowerShell](application-gateway-create-gateway.md).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="c767a-119">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="c767a-119">Before you begin</span></span>

1. <span data-ttu-id="c767a-120">Telepítse az Azure PowerShell-parancsmagok legújabb verzióját a Webplatform-telepítővel.</span><span class="sxs-lookup"><span data-stu-id="c767a-120">Install the latest version of the Azure PowerShell cmdlets by using the Web Platform Installer.</span></span> <span data-ttu-id="c767a-121">A [Letöltések lap](https://azure.microsoft.com/downloads/) **Windows PowerShell** szakaszából letöltheti és telepítheti a legújabb verziót.</span><span class="sxs-lookup"><span data-stu-id="c767a-121">You can download and install the latest version from the **Windows PowerShell** section of the [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
1. <span data-ttu-id="c767a-122">Ha rendelkezik meglévő virtuális hálózattal, válasszon ki egy meglévő üres alhálózatot, vagy hozzon létre egyet a meglévő virtuális hálózatban kizárólag az alkalmazásátjáró számára.</span><span class="sxs-lookup"><span data-stu-id="c767a-122">If you have an existing virtual network, either select an existing empty subnet or create a subnet in your existing virtual network solely for use by the application gateway.</span></span> <span data-ttu-id="c767a-123">Nem telepítheti az alkalmazásátjárót egy másik virtuális hálózatra, csak amelyikre az erőforrást szeretné telepíteni az alkalmazásátjáró mögött.</span><span class="sxs-lookup"><span data-stu-id="c767a-123">You cannot deploy the application gateway to a different virtual network than the resources you intend to deploy behind the application gateway.</span></span>
1. <span data-ttu-id="c767a-124">A kiszolgálóknak, amelyeket az Application Gateway használatára konfigurál, már létezniük kell, illetve a virtuális hálózatban vagy hozzárendelt nyilvános/virtuális IP-címmel létrehozott végpontokkal kell rendelkezniük.</span><span class="sxs-lookup"><span data-stu-id="c767a-124">The servers that you configure to use the application gateway must exist or have their endpoints created either in the virtual network or with a public IP/VIP assigned.</span></span>

## <a name="what-is-required-to-create-an-application-gateway"></a><span data-ttu-id="c767a-125">Mire van szükség egy Application Gateway létrehozásához?</span><span class="sxs-lookup"><span data-stu-id="c767a-125">What is required to create an application gateway?</span></span>

* <span data-ttu-id="c767a-126">**Háttérkiszolgáló-készlet:** A háttérkiszolgálók IP-címeinek, teljes tartományneveinek vagy hálózati kártyáinak listája.</span><span class="sxs-lookup"><span data-stu-id="c767a-126">**Backend server pool:** The list of IP addresses, FQDNs, or NICs of the backend servers.</span></span> <span data-ttu-id="c767a-127">Ha IP-címeket használ, akkor vagy a virtuális hálózat alhálózatához kell tartozniuk, vagy nyilvános/virtuális IP-címnek kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="c767a-127">If IP addresses are used, they should either belong to the virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="c767a-128">**Háttérkiszolgáló-készlet beállításai:** Minden készlet rendelkezik olyan beállításokkal, mint a port, a protokoll vagy a cookie-alapú affinitás.</span><span class="sxs-lookup"><span data-stu-id="c767a-128">**Backend server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="c767a-129">Ezek a beállítások egy adott készlethez kapcsolódnak, és a készlet minden kiszolgálójára érvényesek.</span><span class="sxs-lookup"><span data-stu-id="c767a-129">These settings are tied to a pool and are applied to all servers within the pool.</span></span>
* <span data-ttu-id="c767a-130">**előtérbeli port:** Az Application Gatewayen megnyitott nyilvános port.</span><span class="sxs-lookup"><span data-stu-id="c767a-130">**frontend port:** This port is the public port that is opened on the application gateway.</span></span> <span data-ttu-id="c767a-131">Amikor a forgalom eléri ezt a portot, a port átirányítja az egyik háttérkiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="c767a-131">Traffic hits this port, and then gets redirected to one of the backend servers.</span></span>
* <span data-ttu-id="c767a-132">**Figyelő:** A figyelő egy előtérbeli porttal, egy protokollal (Http vagy Https, a kis- és a nagybetűk megkülönböztetésével) és SSL-tanúsítványnévvel rendelkezik (SSL-kiszervezés konfigurálásakor).</span><span class="sxs-lookup"><span data-stu-id="c767a-132">**Listener:** The listener has a frontend port, a protocol (Http or Https, these values are case-sensitive), and the SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="c767a-133">**Szabály:** A szabály köti a figyelőt és a háttérkiszolgáló-készletet, és meghatározza, hogy melyik háttérkiszolgáló-készletre legyen átirányítva a forgalom, ha elér egy adott figyelőt.</span><span class="sxs-lookup"><span data-stu-id="c767a-133">**Rule:** The rule binds the listener, the backend server pool and defines which backend server pool the traffic should be directed to when it hits a particular listener.</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="c767a-134">Erőforráscsoport létrehozása a Resource Managerhez</span><span class="sxs-lookup"><span data-stu-id="c767a-134">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="c767a-135">Ügyeljen arra, hogy az Azure PowerShell legújabb verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="c767a-135">Make sure that you are using the latest version of Azure PowerShell.</span></span> <span data-ttu-id="c767a-136">További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="c767a-136">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

1. <span data-ttu-id="c767a-137">Jelentkezzen be az Azure-ba, és adja meg hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="c767a-137">Log in to Azure and enter your credentials.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

2. <span data-ttu-id="c767a-138">Keresse meg a fiókot az előfizetésekben.</span><span class="sxs-lookup"><span data-stu-id="c767a-138">Check the subscriptions for the account.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```

3. <span data-ttu-id="c767a-139">Válassza ki, hogy melyik Azure előfizetést fogja használni.</span><span class="sxs-lookup"><span data-stu-id="c767a-139">Choose which of your Azure subscriptions to use.</span></span>

  ```powershell
  Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
  ```

4. <span data-ttu-id="c767a-140">Hozzon létre egy erőforráscsoportot (hagyja ki ezt a lépést, ha egy meglévő erőforráscsoportot használ).</span><span class="sxs-lookup"><span data-stu-id="c767a-140">Create a resource group (skip this step if you're using an existing resource group).</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name ContosoRG -Location "West US"
  ```

<span data-ttu-id="c767a-141">Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet.</span><span class="sxs-lookup"><span data-stu-id="c767a-141">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="c767a-142">Ez a hely lesz az erőforráscsoport erőforrásainak alapértelmezett helye.</span><span class="sxs-lookup"><span data-stu-id="c767a-142">This location is used as the default location for resources in that resource group.</span></span> <span data-ttu-id="c767a-143">Győződjön meg arról, hogy az Application Gateway létrehozására irányuló összes parancs ugyanazt az erőforráscsoportot használja.</span><span class="sxs-lookup"><span data-stu-id="c767a-143">Make sure that all commands to create an application gateway uses the same resource group.</span></span>

<span data-ttu-id="c767a-144">A fenti példában létrehoztunk egy **ContosoRG** nevű erőforráscsoportot, amelynek a helye az **USA keleti régiója (East US)**.</span><span class="sxs-lookup"><span data-stu-id="c767a-144">In the example above, we created a resource group called **ContosoRG** and location **East US**.</span></span>

> [!NOTE]
> <span data-ttu-id="c767a-145">Ha egy egyéni mintát kell konfigurálnia az Application Gateway számára: [Application Gateway létrehozása egyéni mintákkal a PowerShell használatával](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="c767a-145">If you need to configure a custom probe for your application gateway, visit: [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md).</span></span> <span data-ttu-id="c767a-146">További információért lásd: [egyéni minták és állapotfigyelés](application-gateway-probe-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c767a-146">Check out [custom probes and health monitoring](application-gateway-probe-overview.md) for more information.</span></span>


## <a name="create-the-application-gateway-configuration-objects"></a><span data-ttu-id="c767a-147">Hozza létre az Application Gateway konfigurációs objektumokat</span><span class="sxs-lookup"><span data-stu-id="c767a-147">Create the application gateway configuration objects</span></span>

<span data-ttu-id="c767a-148">Az Application Gateway létrehozása előtt minden konfigurációs elemet be kell állítani.</span><span class="sxs-lookup"><span data-stu-id="c767a-148">All configuration items must be set up before creating the application gateway.</span></span> <span data-ttu-id="c767a-149">Az alábbi lépések létrehozzák az Application Gateway erőforráshoz szükséges konfigurációs elemeket.</span><span class="sxs-lookup"><span data-stu-id="c767a-149">The following steps create the configuration items that are needed for an application gateway resource.</span></span>

```powershell
# Create a subnet and assign the address space of 10.0.0.0/24
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

# Create a virtual network with the address space of 10.0.0.0/16 and add the subnet
$vnet = New-AzureRmVirtualNetwork -Name ContosoVNET -ResourceGroupName ContosoRG -Location "East US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

# Retrieve the newly created subnet
$subnet=$vnet.Subnets[0]

# Create a public IP address that is used to connect to the application gateway. Application Gateway does not support custom DNS names on public IP addresses.  If a custom name is required for the public endpoint, a CNAME record should be created to point to the automatically generated DNS name for the public IP address.
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName ContosoRG -name publicIP01 -location "East US" -AllocationMethod Dynamic

# Create a gateway IP configuration. The gateway picks up an IP addressfrom the configured subnet and routes network traffic to the IP addresses in the backend IP pool. Keep in mind that each instance takes one IP address.
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

# Configure a backend pool with the addresses of your web servers. These backend pool members are all validated to be healthy by probes, whether they are basic probes or custom probes.  Traffic is then routed to them when requests come into the application gateway. Backend pools can be used by multiple rules within the application gateway, which means one backend pool could be used for multiple web applications that reside on the same host.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

# Configure backend http settings to determine the protocol and port that is used when sending traffic to the backend servers. Cookie-based sessions are also determined by the backend HTTP settings.  If enabled, cookie-based session affinity sends traffic to the same backend as previous requests for each packet.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120

# Configure a frontend port that is used to connect to the application gateway through the public IP address
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

# Configure the frontend IP configuration with the public IP address created earlier.
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

# Configure the listener.  The listener is a combination of the front end IP configuration, protocol, and port and is used to receive incoming network traffic. 
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

# Configure a basic rule that is used to route traffic to the backend servers. The backend pool settings, listener, and backend pool created in the previous steps make up the rule. Based on the criteria defined traffic is routed to the appropriate backend.
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Configure the SKU for the application gateway, this determines the size and whether or not WAF is used.
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

# Create the application gateway
$appgw = New-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG -Location "East US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

<span data-ttu-id="c767a-150">Ha elkészült, kérje le az Application Gateway DNS-hez és virtuális IP-címhez kapcsolódó adatait az Application Gatewayhez csatolt nyilvános IP-erőforrásból.</span><span class="sxs-lookup"><span data-stu-id="c767a-150">When complete, retrieve DNS and VIP details of the application gateway from the public IP resource attached to the application gateway.</span></span>

```powershell
Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName ContosoRG
```

## <a name="delete-the-application-gateway"></a><span data-ttu-id="c767a-151">Application Gateway törlése</span><span class="sxs-lookup"><span data-stu-id="c767a-151">Delete the application gateway</span></span>

<span data-ttu-id="c767a-152">Az alábbi példa az Application Gateway törlését mutatja be.</span><span class="sxs-lookup"><span data-stu-id="c767a-152">The following example deletes the application gateway.</span></span>

```powershell
# Retrieve the application gateway
$gw = Get-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG

# Stops the application gateway
Stop-AzureRmApplicationGateway -ApplicationGateway $gw

# Once the application gateway is in a stopped state, use the `Remove-AzureRmApplicationGateway` cmdlet to remove the service.
Remove-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG -Force
```

> [!NOTE]
> <span data-ttu-id="c767a-153">A **-force** kapcsolóval le lehet tiltani az eltávolítás jóváhagyása üzenetet.</span><span class="sxs-lookup"><span data-stu-id="c767a-153">The **-force** switch can be used to suppress the remove confirmation message.</span></span>

<span data-ttu-id="c767a-154">A szolgáltatás eltávolításának ellenőrzéséhez használhatja a `Get-AzureRmApplicationGateway` parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="c767a-154">To verify that the service has been removed, you can use the `Get-AzureRmApplicationGateway` cmdlet.</span></span> <span data-ttu-id="c767a-155">Ez a lépés nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="c767a-155">This step is not required.</span></span>

```powershell
Get-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG
```

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="c767a-156">Az Application Gateway DNS-nevének beszerzése</span><span class="sxs-lookup"><span data-stu-id="c767a-156">Get application gateway DNS name</span></span>

<span data-ttu-id="c767a-157">Az átjáró létrehozása után a következő lépés a kommunikációra szolgáló előtér konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="c767a-157">Once the gateway is created, the next step is to configure the front end for communication.</span></span> <span data-ttu-id="c767a-158">Nyilvános IP-cím esetén az Application Gateway használatához dinamikusan hozzárendelt DNS-névre van szükség, amely nem valódi név.</span><span class="sxs-lookup"><span data-stu-id="c767a-158">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="c767a-159">Ha szeretné, hogy a végfelhasználók elérjék az Application Gatewayt, használjon egy Application Gateway nyilvános végpontjára mutató CNAME-rekordot.</span><span class="sxs-lookup"><span data-stu-id="c767a-159">To ensure end users can hit the application gateway, a CNAME record can be used to point to the public endpoint of the application gateway.</span></span> <span data-ttu-id="c767a-160">A művelet végrehajtásához az Application Gateway részleteinek beszerzésére és a kapcsolódó IP/DNS-név lekérésére van szükség az Application Gatewayhez csatolt PublicIPAddress használatával.</span><span class="sxs-lookup"><span data-stu-id="c767a-160">To do this, retrieve details of the application gateway and its associated IP/DNS name using the PublicIPAddress element attached to the application gateway.</span></span> <span data-ttu-id="c767a-161">Ez az Azure DNS-sel vagy más DNS-szolgáltatókkal végezhető el egy olyan CNAME rekord létrehozásával, amely a [nyilvános IP-címre](../dns/dns-custom-domain.md#public-ip-address) mutat.</span><span class="sxs-lookup"><span data-stu-id="c767a-161">This can be done with Azure DNS or other DNS providers, by creating a CNAME record that points to the [public IP address](../dns/dns-custom-domain.md#public-ip-address).</span></span> <span data-ttu-id="c767a-162">Az A-bejegyzések használata nem javasolt, mivel a virtuális IP-cím változhat az Application Gateway újraindításakor.</span><span class="sxs-lookup"><span data-stu-id="c767a-162">The use of A-records is not recommended since the VIP may change on restart of application gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="c767a-163">Amikor a szolgáltatás elindul, egy IP-cím lesz kiosztva az Application Gatewaynek.</span><span class="sxs-lookup"><span data-stu-id="c767a-163">An IP address is assigned to the application gateway when the service starts.</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName ContosoRG -Name publicIP01
```

```
Name                     : publicIP01
ResourceGroupName        : ContosoRG
Location                 : westus
Id                       : /subscriptions/<subscription_id>/resourceGroups/ContosoRG/providers/Microsoft.Network/publicIPAddresses/publicIP01
Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
ResourceGuid             : 00000000-0000-0000-0000-000000000000
ProvisioningState        : Succeeded
Tags                     : 
PublicIpAllocationMethod : Dynamic
IpAddress                : xx.xx.xxx.xx
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                                "Id": "/subscriptions/<subscription_id>/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/ContosoAppGateway/frontendIP
                            Configurations/frontend1"
                            }
DnsSettings              : {
                                "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                            }
```

## <a name="delete-all-resources"></a><span data-ttu-id="c767a-164">Az összes erőforrás törlése</span><span class="sxs-lookup"><span data-stu-id="c767a-164">Delete all resources</span></span>

<span data-ttu-id="c767a-165">A jelen cikkben létrehozott összes erőforrás törléséhez hajtsa végre az alábbi lépést:</span><span class="sxs-lookup"><span data-stu-id="c767a-165">To delete all resources created in this article, complete the following step:</span></span>

```powershell
Remove-AzureRmResourceGroup -Name ContosoRG
```

## <a name="next-steps"></a><span data-ttu-id="c767a-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c767a-166">Next steps</span></span>

<span data-ttu-id="c767a-167">Ha SSL-alapú kiszervezést szeretne konfigurálni, tekintse meg a következőt: [Application Gateway konfigurálása SSL-alapú kiszervezéshez](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="c767a-167">If you want to configure SSL offload, visit: [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="c767a-168">Ha konfigurálni szeretne egy ILB-vel használni kívánt Application Gateway-t: [Application Gateway létrehozása belső terheléselosztóval (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="c767a-168">If you want to configure an application gateway to use with an internal load balancer, visit: [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="c767a-169">Ha további általános információra van szüksége a terheléselosztás beállításaival kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="c767a-169">If you want more information about load balancing options in general, visit:</span></span>

* [<span data-ttu-id="c767a-170">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="c767a-170">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="c767a-171">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="c767a-171">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)
