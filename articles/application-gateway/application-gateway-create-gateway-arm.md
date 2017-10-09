---
title: "aaaCreate és kezelése az Azure Application Gateway - PowerShell |} Microsoft Docs"
description: "Ezen a lapon útmutatás toocreate, konfigurálása, indítsa el, és Azure Alkalmazásátjáró törlése az Azure Resource Manager használatával"
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
ms.openlocfilehash: ab98d5f9aa0dc309f8353b7f72591359e1121849
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a><span data-ttu-id="18193-103">Application Gateway létrehozása, indítása vagy törlése az Azure Resource Manager használatával</span><span class="sxs-lookup"><span data-stu-id="18193-103">Create, start, or delete an application gateway by using Azure Resource Manager</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="18193-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="18193-104">Azure portal</span></span>](application-gateway-create-gateway-portal.md)
> * [<span data-ttu-id="18193-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="18193-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-gateway-arm.md)
> * [<span data-ttu-id="18193-106">Klasszikus Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="18193-106">Azure Classic PowerShell</span></span>](application-gateway-create-gateway.md)
> * [<span data-ttu-id="18193-107">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="18193-107">Azure Resource Manager template</span></span>](application-gateway-create-gateway-arm-template.md)
> * [<span data-ttu-id="18193-108">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="18193-108">Azure CLI</span></span>](application-gateway-create-gateway-cli.md)

<span data-ttu-id="18193-109">Az Azure Application Gateway egy 7. rétegbeli terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="18193-109">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="18193-110">Azt is biztosít feladatátvételi és HTTP-kérelmek teljesítmény-útválasztási különböző kiszolgálók között hello felhőalapú vagy helyszíni legyenek.</span><span class="sxs-lookup"><span data-stu-id="18193-110">It provides failover and performance-routing HTTP requests between different servers, whether they are on hello cloud or on-premises.</span></span> <span data-ttu-id="18193-111">Az Application Gateway számos alkalmazáskézbesítési vezérlőszolgáltatást (ADC) biztosít, beleértve a HTTP-terheléselosztást, a cookie-alapú munkamenet-affinitást, a Secure Sockets Layer- (SSL-) alapú kiszervezést, az egyéni állapotmintákat, a többhelyes támogatást és még sok mást.</span><span class="sxs-lookup"><span data-stu-id="18193-111">Application Gateway provides many application delivery controller (ADC) features including HTTP load balancing, cookie-based session affinity, Secure Sockets Layer (SSL) offload, custom health probes, support for multi-site, and many others.</span></span> <span data-ttu-id="18193-112">látogasson el a támogatott funkciók teljes listáját toofind [Alkalmazásátjáró áttekintése](application-gateway-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="18193-112">toofind a complete list of supported features, visit [Application Gateway overview](application-gateway-introduction.md).</span></span>

<span data-ttu-id="18193-113">Ez a cikk bemutatja, hogyan hello lépéseket toocreate, konfigurálása, indítsa el és Alkalmazásátjáró törlése.</span><span class="sxs-lookup"><span data-stu-id="18193-113">This article walks you through hello steps toocreate, configure, start, and delete an application gateway.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="18193-114">Azure-erőforrások használata előtt-e, hogy Azure jelenleg két üzembe helyezési modellel rendelkezik fontos toounderstand: Resource Manager és klasszikus.</span><span class="sxs-lookup"><span data-stu-id="18193-114">Before you work with Azure resources, it is important toounderstand that Azure currently has two deployment models: Resource Manager and classic.</span></span> <span data-ttu-id="18193-115">Bizonyosodjon meg arról, hogy megfelelő ismeretekkel rendelkezik az [üzembe helyezési modellekről és eszközökről](../azure-classic-rm.md), mielőtt elkezdene dolgozni az Azure-erőforrásokkal.</span><span class="sxs-lookup"><span data-stu-id="18193-115">Make sure that you understand [deployment models and tools](../azure-classic-rm.md) before working with any Azure resource.</span></span> <span data-ttu-id="18193-116">Ez a cikk hello tetején hello fülekre kattintva megtekintheti a különféle eszközök dokumentációit hello.</span><span class="sxs-lookup"><span data-stu-id="18193-116">You can view hello documentation for different tools by clicking hello tabs at hello top of this article.</span></span> <span data-ttu-id="18193-117">Jelen dokumentum az Application Gateway az Azure Resource Manager használatával történő létrehozását ismerteti.</span><span class="sxs-lookup"><span data-stu-id="18193-117">This document covers creating an application gateway by using Azure Resource Manager.</span></span> <span data-ttu-id="18193-118">toouse hello klasszikus verzióra, nyissa meg túl[egy alkalmazás átjáró klasszikus üzembe helyezési létrehozása a PowerShell használatával](application-gateway-create-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="18193-118">toouse hello classic version, go too[Create an application gateway classic deployment by using PowerShell](application-gateway-create-gateway.md).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="18193-119">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="18193-119">Before you begin</span></span>

1. <span data-ttu-id="18193-120">Webplatform-telepítő hello segítségével hello hello Azure PowerShell-parancsmagok legújabb verzióját telepítse.</span><span class="sxs-lookup"><span data-stu-id="18193-120">Install hello latest version of hello Azure PowerShell cmdlets by using hello Web Platform Installer.</span></span> <span data-ttu-id="18193-121">Töltse le, és telepítse hello legújabb verziót a hello **Windows PowerShell** hello szakasza [letöltési oldalon](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="18193-121">You can download and install hello latest version from hello **Windows PowerShell** section of hello [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
1. <span data-ttu-id="18193-122">Ha egy meglévő virtuális hálózattal rendelkezik, válasszon egy létező üres alhálózatot, vagy hozzon létre egy alhálózatot a meglévő virtuális hálózatban kizárólag használatra hello Alkalmazásátjáró által.</span><span class="sxs-lookup"><span data-stu-id="18193-122">If you have an existing virtual network, either select an existing empty subnet or create a subnet in your existing virtual network solely for use by hello application gateway.</span></span> <span data-ttu-id="18193-123">Nem telepíthet központilag hello alkalmazás átjáró tooa másik virtuális hálózatot hello erőforrások mint mögött hello Alkalmazásátjáró toodeploy szeretné.</span><span class="sxs-lookup"><span data-stu-id="18193-123">You cannot deploy hello application gateway tooa different virtual network than hello resources you intend toodeploy behind hello application gateway.</span></span>
1. <span data-ttu-id="18193-124">hello kiszolgálók toouse hello Alkalmazásátjáró konfigurált léteznie kell, különben a végpontok hello virtuális hálózatban vagy a nyilvános IP-cím/VIP rendelt hozzá.</span><span class="sxs-lookup"><span data-stu-id="18193-124">hello servers that you configure toouse hello application gateway must exist or have their endpoints created either in hello virtual network or with a public IP/VIP assigned.</span></span>

## <a name="what-is-required-toocreate-an-application-gateway"></a><span data-ttu-id="18193-125">Mi az a szükséges toocreate olyan átjárót?</span><span class="sxs-lookup"><span data-stu-id="18193-125">What is required toocreate an application gateway?</span></span>

* <span data-ttu-id="18193-126">**Kiszolgáló háttérkészlet:** IP-címek, teljes tartománynevek vagy hálózati adaptert a hello háttérkiszolgálók hello listája.</span><span class="sxs-lookup"><span data-stu-id="18193-126">**Backend server pool:** hello list of IP addresses, FQDNs, or NICs of hello backend servers.</span></span> <span data-ttu-id="18193-127">IP-címek használata esetén toohello virtuális hálózati alhálózat vagy kell tartoznia, vagy egy nyilvános IP-cím/VIP kell lennie.</span><span class="sxs-lookup"><span data-stu-id="18193-127">If IP addresses are used, they should either belong toohello virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="18193-128">**Háttérkiszolgáló-készlet beállításai:** Minden készlet rendelkezik olyan beállításokkal, mint a port, a protokoll vagy a cookie-alapú affinitás.</span><span class="sxs-lookup"><span data-stu-id="18193-128">**Backend server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="18193-129">Ezek a beállítások esetén tooa kapcsolt verem és a hello készlet alkalmazott tooall-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="18193-129">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="18193-130">**az elülső rétegbeli portot:** Ez a port nem hello nyilvános portot, amelyet a hello Alkalmazásátjáró meg van nyitva.</span><span class="sxs-lookup"><span data-stu-id="18193-130">**frontend port:** This port is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="18193-131">Forgalom találatok ezt a portot, és ezután lekérdezi a hello háttérkiszolgálók átirányított tooone.</span><span class="sxs-lookup"><span data-stu-id="18193-131">Traffic hits this port, and then gets redirected tooone of hello backend servers.</span></span>
* <span data-ttu-id="18193-132">**Figyelő:** hello figyelő rendelkezik egy elülső rétegbeli portot, a protokollt (Http vagy Https, ezek az értékek kis-és nagybetűket), és hello SSL tanúsítvány neve (ha az SSL beállításának-kiszervezés).</span><span class="sxs-lookup"><span data-stu-id="18193-132">**Listener:** hello listener has a frontend port, a protocol (Http or Https, these values are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="18193-133">**Szabály:** hello szabály hello figyelő, a kiszolgáló háttérkészlet hello van kötve, és azt határozza meg, melyik háttér címkészletet hello forgalom irányított toowhen találatok száma a egy adott figyelő.</span><span class="sxs-lookup"><span data-stu-id="18193-133">**Rule:** hello rule binds hello listener, hello backend server pool and defines which backend server pool hello traffic should be directed toowhen it hits a particular listener.</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="18193-134">Erőforráscsoport létrehozása a Resource Managerhez</span><span class="sxs-lookup"><span data-stu-id="18193-134">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="18193-135">Győződjön meg arról, hogy azok be hello Azure PowerShell legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="18193-135">Make sure that you are using hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="18193-136">További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="18193-136">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

1. <span data-ttu-id="18193-137">Jelentkezzen be tooAzure, és adja meg a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="18193-137">Log in tooAzure and enter your credentials.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

2. <span data-ttu-id="18193-138">Hello előfizetések hello fiók ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="18193-138">Check hello subscriptions for hello account.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```

3. <span data-ttu-id="18193-139">Válassza ki, amely az Azure-előfizetések toouse.</span><span class="sxs-lookup"><span data-stu-id="18193-139">Choose which of your Azure subscriptions toouse.</span></span>

  ```powershell
  Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
  ```

4. <span data-ttu-id="18193-140">Hozzon létre egy erőforráscsoportot (hagyja ki ezt a lépést, ha egy meglévő erőforráscsoportot használ).</span><span class="sxs-lookup"><span data-stu-id="18193-140">Create a resource group (skip this step if you're using an existing resource group).</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name ContosoRG -Location "West US"
  ```

<span data-ttu-id="18193-141">Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet.</span><span class="sxs-lookup"><span data-stu-id="18193-141">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="18193-142">Ezen a helyen az erőforráscsoport erőforrások lesz hello alapértelmezett helye.</span><span class="sxs-lookup"><span data-stu-id="18193-142">This location is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="18193-143">Győződjön meg arról, hogy olyan átjárót használó összes parancsok toocreate hello azonos erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="18193-143">Make sure that all commands toocreate an application gateway uses hello same resource group.</span></span>

<span data-ttu-id="18193-144">A hello a fenti példában létrehozott nevű erőforráscsoport **ContosoRG** és a hely **USA keleti régiója**.</span><span class="sxs-lookup"><span data-stu-id="18193-144">In hello example above, we created a resource group called **ContosoRG** and location **East US**.</span></span>

> [!NOTE]
> <span data-ttu-id="18193-145">Ha egy egyéni mintavételt tooconfigure az Alkalmazásátjáró van szüksége, látogasson el: [PowerShell használatával hozzon létre olyan átjárót egyéni mintavételt](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="18193-145">If you need tooconfigure a custom probe for your application gateway, visit: [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md).</span></span> <span data-ttu-id="18193-146">További információért lásd: [egyéni minták és állapotfigyelés](application-gateway-probe-overview.md).</span><span class="sxs-lookup"><span data-stu-id="18193-146">Check out [custom probes and health monitoring](application-gateway-probe-overview.md) for more information.</span></span>


## <a name="create-hello-application-gateway-configuration-objects"></a><span data-ttu-id="18193-147">Konfigurációs objektumok hello Alkalmazásátjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="18193-147">Create hello application gateway configuration objects</span></span>

<span data-ttu-id="18193-148">Az összes konfigurációs elemek hello Alkalmazásátjáró létrehozása előtt kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="18193-148">All configuration items must be set up before creating hello application gateway.</span></span> <span data-ttu-id="18193-149">hello lépések hello konfigurációs elemek létrehozását, amelyek szükségesek az alkalmazás átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="18193-149">hello following steps create hello configuration items that are needed for an application gateway resource.</span></span>

```powershell
# Create a subnet and assign hello address space of 10.0.0.0/24
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

# Create a virtual network with hello address space of 10.0.0.0/16 and add hello subnet
$vnet = New-AzureRmVirtualNetwork -Name ContosoVNET -ResourceGroupName ContosoRG -Location "East US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

# Retrieve hello newly created subnet
$subnet=$vnet.Subnets[0]

# Create a public IP address that is used tooconnect toohello application gateway. Application Gateway does not support custom DNS names on public IP addresses.  If a custom name is required for hello public endpoint, a CNAME record should be created toopoint toohello automatically generated DNS name for hello public IP address.
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName ContosoRG -name publicIP01 -location "East US" -AllocationMethod Dynamic

# Create a gateway IP configuration. hello gateway picks up an IP addressfrom hello configured subnet and routes network traffic toohello IP addresses in hello backend IP pool. Keep in mind that each instance takes one IP address.
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

# Configure a backend pool with hello addresses of your web servers. These backend pool members are all validated toobe healthy by probes, whether they are basic probes or custom probes.  Traffic is then routed toothem when requests come into hello application gateway. Backend pools can be used by multiple rules within hello application gateway, which means one backend pool could be used for multiple web applications that reside on hello same host.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

# Configure backend http settings toodetermine hello protocol and port that is used when sending traffic toohello backend servers. Cookie-based sessions are also determined by hello backend HTTP settings.  If enabled, cookie-based session affinity sends traffic toohello same backend as previous requests for each packet.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120

# Configure a frontend port that is used tooconnect toohello application gateway through hello public IP address
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

# Configure hello frontend IP configuration with hello public IP address created earlier.
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

# Configure hello listener.  hello listener is a combination of hello front end IP configuration, protocol, and port and is used tooreceive incoming network traffic. 
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

# Configure a basic rule that is used tooroute traffic toohello backend servers. hello backend pool settings, listener, and backend pool created in hello previous steps make up hello rule. Based on hello criteria defined traffic is routed toohello appropriate backend.
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Configure hello SKU for hello application gateway, this determines hello size and whether or not WAF is used.
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

# Create hello application gateway
$appgw = New-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG -Location "East US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

<span data-ttu-id="18193-150">Amikor végzett, DNS- és VIP részleteinek hello Alkalmazásátjáró hello nyilvános IP-erőforrás csatolt toohello alkalmazás átjáró lekérése.</span><span class="sxs-lookup"><span data-stu-id="18193-150">When complete, retrieve DNS and VIP details of hello application gateway from hello public IP resource attached toohello application gateway.</span></span>

```powershell
Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName ContosoRG
```

## <a name="delete-hello-application-gateway"></a><span data-ttu-id="18193-151">Hello Alkalmazásátjáró törlése</span><span class="sxs-lookup"><span data-stu-id="18193-151">Delete hello application gateway</span></span>

<span data-ttu-id="18193-152">a következő példa törlések hello Alkalmazásátjáró hello.</span><span class="sxs-lookup"><span data-stu-id="18193-152">hello following example deletes hello application gateway.</span></span>

```powershell
# Retrieve hello application gateway
$gw = Get-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG

# Stops hello application gateway
Stop-AzureRmApplicationGateway -ApplicationGateway $gw

# Once hello application gateway is in a stopped state, use hello `Remove-AzureRmApplicationGateway` cmdlet tooremove hello service.
Remove-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG -Force
```

> [!NOTE]
> <span data-ttu-id="18193-153">Hello **-force** kapcsoló lehet használt toosuppress hello eltávolítása jóváhagyást kérő üzenet.</span><span class="sxs-lookup"><span data-stu-id="18193-153">hello **-force** switch can be used toosuppress hello remove confirmation message.</span></span>

<span data-ttu-id="18193-154">tooverify, amely hello szolgáltatás el lett távolítva, használhatja a hello `Get-AzureRmApplicationGateway` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="18193-154">tooverify that hello service has been removed, you can use hello `Get-AzureRmApplicationGateway` cmdlet.</span></span> <span data-ttu-id="18193-155">Ez a lépés nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="18193-155">This step is not required.</span></span>

```powershell
Get-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG
```

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="18193-156">Az Application Gateway DNS-nevének beszerzése</span><span class="sxs-lookup"><span data-stu-id="18193-156">Get application gateway DNS name</span></span>

<span data-ttu-id="18193-157">Hello átjáró létrehozása után hello következő lépésre tooconfigure hello előtér-kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="18193-157">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="18193-158">Nyilvános IP-cím esetén az Application Gateway használatához dinamikusan hozzárendelt DNS-névre van szükség, amely nem valódi név.</span><span class="sxs-lookup"><span data-stu-id="18193-158">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="18193-159">tooensure a végfelhasználók is találati hello Alkalmazásátjáró, egy olyan CNAME rekordot is használt toopoint toohello nyilvános végpontot hello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="18193-159">tooensure end users can hit hello application gateway, a CNAME record can be used toopoint toohello public endpoint of hello application gateway.</span></span> <span data-ttu-id="18193-160">toodo a, hello Alkalmazásátjáró és a társított IP-/ DNS-nevét, hello PublicIPAddress elem csatolt toohello Alkalmazásátjáró beolvasása részleteit.</span><span class="sxs-lookup"><span data-stu-id="18193-160">toodo this, retrieve details of hello application gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello application gateway.</span></span> <span data-ttu-id="18193-161">Ezt megteheti az Azure DNS- vagy más DNS-szolgáltatók által létrehozható egy olyan CNAME rekordot, amely mutat toohello [nyilvános IP-cím](../dns/dns-custom-domain.md#public-ip-address).</span><span class="sxs-lookup"><span data-stu-id="18193-161">This can be done with Azure DNS or other DNS providers, by creating a CNAME record that points toohello [public IP address](../dns/dns-custom-domain.md#public-ip-address).</span></span> <span data-ttu-id="18193-162">A-rekordok hello használata nem javasolt, mert hello VIP módosíthatja az Alkalmazásátjáró újra kell indítani.</span><span class="sxs-lookup"><span data-stu-id="18193-162">hello use of A-records is not recommended since hello VIP may change on restart of application gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="18193-163">IP-cím hozzá van rendelve toohello Alkalmazásátjáró hello szolgáltatás elindulásakor.</span><span class="sxs-lookup"><span data-stu-id="18193-163">An IP address is assigned toohello application gateway when hello service starts.</span></span>

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

## <a name="delete-all-resources"></a><span data-ttu-id="18193-164">Az összes erőforrás törlése</span><span class="sxs-lookup"><span data-stu-id="18193-164">Delete all resources</span></span>

<span data-ttu-id="18193-165">Ebben a cikkben a következő teljes hello létrehozott összes erőforrás lépés toodelete:</span><span class="sxs-lookup"><span data-stu-id="18193-165">toodelete all resources created in this article, complete hello following step:</span></span>

```powershell
Remove-AzureRmResourceGroup -Name ContosoRG
```

## <a name="next-steps"></a><span data-ttu-id="18193-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="18193-166">Next steps</span></span>

<span data-ttu-id="18193-167">Ha azt szeretné, hogy az SSL-kiszervezés tooconfigure, látogasson el: [konfigurálása az SSL-kiszervezés Alkalmazásátjáró](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="18193-167">If you want tooconfigure SSL offload, visit: [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="18193-168">Ha azt szeretné, hogy egy alkalmazás átjáró toouse belső terheléselosztással tooconfigure, látogasson el: [hozzon létre egy alkalmazást egy belső terheléselosztón (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="18193-168">If you want tooconfigure an application gateway toouse with an internal load balancer, visit: [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="18193-169">Ha további általános információra van szüksége a terheléselosztás beállításaival kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="18193-169">If you want more information about load balancing options in general, visit:</span></span>

* [<span data-ttu-id="18193-170">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="18193-170">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="18193-171">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="18193-171">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)
