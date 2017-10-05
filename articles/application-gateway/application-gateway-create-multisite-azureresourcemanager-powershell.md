---
title: "Több hely üzemeltetéséhez Alkalmazásátjáró létrehozása |} Microsoft Docs"
description: "Ez a lap létrehozásához, konfiguráljon egy Azure-alkalmazásokban átjárót ugyanahhoz az átjáróhoz a több webalkalmazás üzemeltetéséhez utasításokat tartalmaz."
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
ms.openlocfilehash: d42efa7d359f5c87c14afbfd138328b37c8ae6c2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-application-gateway-for-hosting-multiple-web-applications"></a><span data-ttu-id="4e307-103">Hozzon létre egy alkalmazás több webalkalmazás üzemeltetéséhez</span><span class="sxs-lookup"><span data-stu-id="4e307-103">Create an application gateway for hosting multiple web applications</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="4e307-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4e307-104">Azure portal</span></span>](application-gateway-create-multisite-portal.md)
> * [<span data-ttu-id="4e307-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="4e307-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-multisite-azureresourcemanager-powershell.md)

<span data-ttu-id="4e307-106">Több helyet üzemeltető lehetővé teszi az ugyanazon Alkalmazásátjáró egynél több webalkalmazás telepítését.</span><span class="sxs-lookup"><span data-stu-id="4e307-106">Multiple site hosting allows you to deploy more than one web application on the same application gateway.</span></span> <span data-ttu-id="4e307-107">Az állomásfejlécnek meghatározni, mely figyelő kapja forgalom a bejövő HTTP-kérelmek jelenlétére támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="4e307-107">It relies on presence of host header in the incoming HTTP request, to determine which listener would receive traffic.</span></span> <span data-ttu-id="4e307-108">A figyelő majd arra utasítja a megfelelő háttérkészlet-forgalom, be az átjáró szabályok meghatározását.</span><span class="sxs-lookup"><span data-stu-id="4e307-108">The listener then directs traffic to appropriate backend pool as configured in the rules definition of the gateway.</span></span> <span data-ttu-id="4e307-109">Az SSL engedélyezve van a webes alkalmazásokhoz Alkalmazásátjáró a kiszolgálónév jelzése (SNI) bővítménye válassza ki a megfelelő figyelő a webes forgalom támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="4e307-109">In SSL enabled web applications, application gateway relies on the Server Name Indication (SNI) extension to choose the correct listener for the web traffic.</span></span> <span data-ttu-id="4e307-110">A közös több hely üzemeltetéséhez rendeltetése különböző webtartományok különböző háttér-kiszolgálófiók tárolókészletekben az érkező kérések elosztása.</span><span class="sxs-lookup"><span data-stu-id="4e307-110">A common use for multiple site hosting is to load balance requests for different web domains to different back-end server pools.</span></span> <span data-ttu-id="4e307-111">Az azonos gyökértartomány több altartományt hasonló módon is tárolt alkalmazás ugyanahhoz az átjáróhoz.</span><span class="sxs-lookup"><span data-stu-id="4e307-111">Similarly multiple subdomains of the same root domain could also be hosted on the same application gateway.</span></span>

## <a name="scenario"></a><span data-ttu-id="4e307-112">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="4e307-112">Scenario</span></span>

<span data-ttu-id="4e307-113">A következő példában Alkalmazásátjáró van kiszolgáló a contoso.com és fabrikam.com forgalom a két háttér-kiszolgálófiók rendelkezik: contoso kiszolgálókészlet és a fabrikam kiszolgálókészlethez.</span><span class="sxs-lookup"><span data-stu-id="4e307-113">In the following example, application gateway is serving traffic for contoso.com and fabrikam.com with two back-end server pools: contoso server pool and fabrikam server pool.</span></span> <span data-ttu-id="4e307-114">Hasonló telepítő állomás altartományok például app.contoso.com és blog.contoso.com használható.</span><span class="sxs-lookup"><span data-stu-id="4e307-114">Similar setup could be used to host subdomains like app.contoso.com and blog.contoso.com.</span></span>

![imageURLroute](./media/application-gateway-create-multisite-azureresourcemanager-powershell/multisite.png)

## <a name="before-you-begin"></a><span data-ttu-id="4e307-116">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="4e307-116">Before you begin</span></span>

1. <span data-ttu-id="4e307-117">Telepítse az Azure PowerShell-parancsmagok legújabb verzióját a Webplatform-telepítővel.</span><span class="sxs-lookup"><span data-stu-id="4e307-117">Install the latest version of the Azure PowerShell cmdlets by using the Web Platform Installer.</span></span> <span data-ttu-id="4e307-118">A [Letöltések lap](https://azure.microsoft.com/downloads/) **Windows PowerShell** szakaszából letöltheti és telepítheti a legújabb verziót.</span><span class="sxs-lookup"><span data-stu-id="4e307-118">You can download and install the latest version from the **Windows PowerShell** section of the [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="4e307-119">Az alkalmazás átjáró használatához a háttér-készlethez hozzáadott kiszolgálók léteznie kell, különben a végpontokat hozott létre vagy egy külön alhálózatot vagy egy nyilvános IP-cím/VIP hozzárendelése a virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="4e307-119">The servers added to the back-end pool to use the application gateway must exist or have their endpoints created either in the virtual network in a separate subnet or with a public IP/VIP assigned.</span></span>

## <a name="requirements"></a><span data-ttu-id="4e307-120">Követelmények</span><span class="sxs-lookup"><span data-stu-id="4e307-120">Requirements</span></span>

* <span data-ttu-id="4e307-121">**Háttér-kiszolgálókészlet:** A háttérkiszolgálók IP-címeinek listája.</span><span class="sxs-lookup"><span data-stu-id="4e307-121">**Back-end server pool:** The list of IP addresses of the back-end servers.</span></span> <span data-ttu-id="4e307-122">A listán szereplő IP-címeknek a virtuális hálózat alhálózatához kell tartozniuk, vagy nyilvános/virtuális IP-címnek kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="4e307-122">The IP addresses listed should either belong to the virtual network subnet or should be a public IP/VIP.</span></span> <span data-ttu-id="4e307-123">Teljes Tartománynevét is használható.</span><span class="sxs-lookup"><span data-stu-id="4e307-123">FQDN can also be used.</span></span>
* <span data-ttu-id="4e307-124">**Háttér-kiszolgálókészlet beállításai:** Minden készletnek vannak beállításai, például port, protokoll vagy cookie-alapú affinitás.</span><span class="sxs-lookup"><span data-stu-id="4e307-124">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="4e307-125">Ezek a beállítások egy adott készlethez kapcsolódnak, és a készlet minden kiszolgálójára érvényesek.</span><span class="sxs-lookup"><span data-stu-id="4e307-125">These settings are tied to a pool and are applied to all servers within the pool.</span></span>
* <span data-ttu-id="4e307-126">**Előtérbeli port:** Az Application Gateway-en megnyitott nyilvános port.</span><span class="sxs-lookup"><span data-stu-id="4e307-126">**Front-end port:** This port is the public port that is opened on the application gateway.</span></span> <span data-ttu-id="4e307-127">Amikor a forgalom eléri ezt a portot, a port átirányítja az egyik háttérkiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="4e307-127">Traffic hits this port, and then gets redirected to one of the back-end servers.</span></span>
* <span data-ttu-id="4e307-128">**Figyelő:** A figyelő egy előtérbeli porttal, egy protokollal (Http vagy Https, a kis- és a nagybetűk megkülönböztetésével) és SSL tanúsítványnévvel rendelkezik (SSL-kiszervezés konfigurálásakor).</span><span class="sxs-lookup"><span data-stu-id="4e307-128">**Listener:** The listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and the SSL certificate name (if configuring SSL offload).</span></span> <span data-ttu-id="4e307-129">A többhelyes engedélyezett alkalmazásátjárót, állomásnév és SNI mutatók is bekerülnek.</span><span class="sxs-lookup"><span data-stu-id="4e307-129">For multi-site enabled application gateways, host name and SNI indicators are also added.</span></span>
* <span data-ttu-id="4e307-130">**Szabály:** a szabály van kötve a figyelő, a háttér-kiszolgálófiók-vermet, és határozza meg, mely a forgalom legyenek irányítva, amikor az adott figyelő találatok háttér-kiszolgálófiók készlet.</span><span class="sxs-lookup"><span data-stu-id="4e307-130">**Rule:** The rule binds the listener, the back-end server pool, and defines which back-end server pool the traffic should be directed to when it hits a particular listener.</span></span> <span data-ttu-id="4e307-131">Szabályok feldolgozása a sorrendben, és a forgalmat a rendszer kéri az első szabály, amely megfelel a sajátlagossága figyelembe vétele függetlenül keresztül.</span><span class="sxs-lookup"><span data-stu-id="4e307-131">Rules are processed in the order they are listed, and traffic will be directed via the first rule that matches regardless of specificity.</span></span> <span data-ttu-id="4e307-132">Például ha egy szabályt egy alapszintű figyelő és egy többhelyes figyelő mindkét ugyanazt a portot használó szabály, a szabály a többhelyes figyelőjével szerepelnie kell a szabály a vártnak megfelelően működik az alapvető figyelő ahhoz, hogy a többhelyes szabály előtt.</span><span class="sxs-lookup"><span data-stu-id="4e307-132">For example, if you have a rule using a basic listener and a rule using a multi-site listener both on the same port, the rule with the multi-site listener must be listed before the rule with the basic listener in order for the multi-site rule to function as expected.</span></span>

## <a name="create-an-application-gateway"></a><span data-ttu-id="4e307-133">Application Gateway létrehozása</span><span class="sxs-lookup"><span data-stu-id="4e307-133">Create an application gateway</span></span>

<span data-ttu-id="4e307-134">Az Alkalmazásátjáró létrehozásához szükséges lépéseket a következők:</span><span class="sxs-lookup"><span data-stu-id="4e307-134">The following are the steps needed to create an application gateway:</span></span>

1. <span data-ttu-id="4e307-135">Egy erőforráscsoport létrehozása a Resource Manager számára.</span><span class="sxs-lookup"><span data-stu-id="4e307-135">Create a resource group for Resource Manager.</span></span>
2. <span data-ttu-id="4e307-136">Hozzon létre egy virtuális hálózatot, alhálózatok és az alkalmazás átjáró nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="4e307-136">Create a virtual network, subnets, and public IP for the application gateway.</span></span>
3. <span data-ttu-id="4e307-137">Egy Application Gateway konfigurációs objektum létrehozása.</span><span class="sxs-lookup"><span data-stu-id="4e307-137">Create an application gateway configuration object.</span></span>
4. <span data-ttu-id="4e307-138">Egy Application Gateway erőforrás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="4e307-138">Create an application gateway resource.</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="4e307-139">Erőforráscsoport létrehozása a Resource Managerhez</span><span class="sxs-lookup"><span data-stu-id="4e307-139">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="4e307-140">Ügyeljen arra, hogy az Azure PowerShell legújabb verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="4e307-140">Make sure that you are using the latest version of Azure PowerShell.</span></span> <span data-ttu-id="4e307-141">További információt [Windows PowerShell használata a Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="4e307-141">More information is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="4e307-142">1. lépés</span><span class="sxs-lookup"><span data-stu-id="4e307-142">Step 1</span></span>

<span data-ttu-id="4e307-143">Jelentkezzen be az Azure-ba</span><span class="sxs-lookup"><span data-stu-id="4e307-143">Log in to Azure</span></span>

```powershell
Login-AzureRmAccount
```
<span data-ttu-id="4e307-144">A rendszer kérni fogja a hitelesítő adatokkal történő hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="4e307-144">You are prompted to authenticate with your credentials.</span></span>

### <a name="step-2"></a><span data-ttu-id="4e307-145">2. lépés</span><span class="sxs-lookup"><span data-stu-id="4e307-145">Step 2</span></span>

<span data-ttu-id="4e307-146">Keresse meg a fiókot az előfizetésekben.</span><span class="sxs-lookup"><span data-stu-id="4e307-146">Check the subscriptions for the account.</span></span>

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a><span data-ttu-id="4e307-147">3. lépés</span><span class="sxs-lookup"><span data-stu-id="4e307-147">Step 3</span></span>

<span data-ttu-id="4e307-148">Válassza ki, hogy melyik Azure előfizetést fogja használni.</span><span class="sxs-lookup"><span data-stu-id="4e307-148">Choose which of your Azure subscriptions to use.</span></span>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a><span data-ttu-id="4e307-149">4. lépés</span><span class="sxs-lookup"><span data-stu-id="4e307-149">Step 4</span></span>

<span data-ttu-id="4e307-150">Hozzon létre egy erőforráscsoportot (hagyja ki ezt a lépést, ha egy meglévő erőforráscsoportot használ).</span><span class="sxs-lookup"><span data-stu-id="4e307-150">Create a resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
New-AzureRmResourceGroup -Name appgw-RG -location "West US"
```

<span data-ttu-id="4e307-151">Másik lehetőségként az Alkalmazásátjáró erőforráscsoport címkéket is létrehozhat:</span><span class="sxs-lookup"><span data-stu-id="4e307-151">Alternatively you can also create tags for a resource group for application gateway:</span></span>

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway multiple site"}
```

<span data-ttu-id="4e307-152">Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet.</span><span class="sxs-lookup"><span data-stu-id="4e307-152">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="4e307-153">Ez a hely lesz az erőforráscsoport erőforrásainak alapértelmezett helye.</span><span class="sxs-lookup"><span data-stu-id="4e307-153">This location is used as the default location for resources in that resource group.</span></span> <span data-ttu-id="4e307-154">Győződjön meg arról, hogy minden parancs Alkalmazásátjáró létrehozása ugyanabban az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="4e307-154">Make sure that all commands to create an application gateway use the same resource group.</span></span>

<span data-ttu-id="4e307-155">A fenti példában létrehozott nevű erőforráscsoport **appgw-RG** a hellyel rendelkező **USA nyugati régiója**.</span><span class="sxs-lookup"><span data-stu-id="4e307-155">In the example above, we created a resource group called **appgw-RG** with a location of **West US**.</span></span>

> [!NOTE]
> <span data-ttu-id="4e307-156">Ha egy egyéni mintát kell konfigurálnia az Application Gateway számára: [Application Gateway létrehozása egyéni mintákkal a PowerShell használatával](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="4e307-156">If you need to configure a custom probe for your application gateway, see [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md).</span></span> <span data-ttu-id="4e307-157">Látogasson el [egyéni mintavételt és az állapotfigyelés](application-gateway-probe-overview.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="4e307-157">Visit [custom probes and health monitoring](application-gateway-probe-overview.md) for more information.</span></span>

## <a name="create-a-virtual-network-and-subnets"></a><span data-ttu-id="4e307-158">Hozzon létre egy virtuális hálózatot és alhálózatok</span><span class="sxs-lookup"><span data-stu-id="4e307-158">Create a virtual network and subnets</span></span>

<span data-ttu-id="4e307-159">Az alábbi példa bemutatja, hogyan hozhat létre virtuális hálózatot a Resource Manager használatával.</span><span class="sxs-lookup"><span data-stu-id="4e307-159">The following example shows how to create a virtual network by using Resource Manager.</span></span> <span data-ttu-id="4e307-160">Két alhálózat ebben a lépésben jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="4e307-160">Two subnets are created in this step.</span></span> <span data-ttu-id="4e307-161">Az első alhálózat van az Alkalmazásátjáró magát.</span><span class="sxs-lookup"><span data-stu-id="4e307-161">The first subnet is for the application gateway itself.</span></span> <span data-ttu-id="4e307-162">Alkalmazásátjáró saját alhálózatba a példányok tárolásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="4e307-162">Application gateway requires its own subnet to hold its instances.</span></span> <span data-ttu-id="4e307-163">Csak más alkalmazásátjárót alhálózat is telepíthető.</span><span class="sxs-lookup"><span data-stu-id="4e307-163">Only other application gateways can be deployed in that subnet.</span></span> <span data-ttu-id="4e307-164">A második alhálózat alkalmazás háttérkiszolgálók tárolására szolgál.</span><span class="sxs-lookup"><span data-stu-id="4e307-164">The second subnet is used to hold the application backend servers.</span></span>

### <a name="step-1"></a><span data-ttu-id="4e307-165">1. lépés</span><span class="sxs-lookup"><span data-stu-id="4e307-165">Step 1</span></span>

<span data-ttu-id="4e307-166">Rendelje hozzá a cím tartomány 10.0.0.0/24 alhálózat változó az Alkalmazásátjáró tárolására használandó.</span><span class="sxs-lookup"><span data-stu-id="4e307-166">Assign the address range 10.0.0.0/24 to the subnet variable to be used to hold the application gateway.</span></span>

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -AddressPrefix 10.0.0.0/24
```
### <a name="step-2"></a><span data-ttu-id="4e307-167">2. lépés</span><span class="sxs-lookup"><span data-stu-id="4e307-167">Step 2</span></span>

<span data-ttu-id="4e307-168">Rendelje hozzá a cím tartomány 10.0.1.0/24 a háttérkészlet használandó Alhalozat_2 változó.</span><span class="sxs-lookup"><span data-stu-id="4e307-168">Assign the address range 10.0.1.0/24 to the subnet2 variable to be used for the backend pools.</span></span>

```powershell
$subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -AddressPrefix 10.0.1.0/24
```

### <a name="step-3"></a><span data-ttu-id="4e307-169">3. lépés</span><span class="sxs-lookup"><span data-stu-id="4e307-169">Step 3</span></span>

<span data-ttu-id="4e307-170">Hozzon létre egy virtuális hálózatot nevű **appgwvnet** erőforráscsoportban **appgw-rg** az USA nyugati régiója régió a előtag 10.0.0.0/16 használata a 10.0.0.0/24 alhálózat, és 10.0.1.0/24.</span><span class="sxs-lookup"><span data-stu-id="4e307-170">Create a virtual network named **appgwvnet** in resource group **appgw-rg** for the West US region using the prefix 10.0.0.0/16 with subnet 10.0.0.0/24, and 10.0.1.0/24.</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet,$subnet2
```

### <a name="step-4"></a><span data-ttu-id="4e307-171">4. lépés</span><span class="sxs-lookup"><span data-stu-id="4e307-171">Step 4</span></span>

<span data-ttu-id="4e307-172">Rendelje hozzá egy olyan átjárót hoz létre, amely alhálózati változó a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="4e307-172">Assign a subnet variable for the next steps, which creates an application gateway.</span></span>

```powershell
$appgatewaysubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -VirtualNetwork $vnet
$backendsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -VirtualNetwork $vnet
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a><span data-ttu-id="4e307-173">Nyilvános IP-cím létrehozása az előtérbeli konfigurációhoz</span><span class="sxs-lookup"><span data-stu-id="4e307-173">Create a public IP address for the front-end configuration</span></span>

<span data-ttu-id="4e307-174">Hozzon létre egy **publicIP01** nevű, nyilvános IP-címhez tartozó erőforrást az **appgw-rg** nevű erőforráscsoportban, az USA nyugati régiójában.</span><span class="sxs-lookup"><span data-stu-id="4e307-174">Create a public IP resource **publicIP01** in resource group **appgw-rg** for the West US region.</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

<span data-ttu-id="4e307-175">Amikor a szolgáltatás elindul, egy IP-cím lesz kiosztva az Application Gatewaynek.</span><span class="sxs-lookup"><span data-stu-id="4e307-175">An IP address is assigned to the application gateway when the service starts.</span></span>

## <a name="create-application-gateway-configuration"></a><span data-ttu-id="4e307-176">Alkalmazás átjáró-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="4e307-176">Create application gateway configuration</span></span>

<span data-ttu-id="4e307-177">Az Application Gateway létrehozása előtt minden konfigurációs elemet be kell állítania.</span><span class="sxs-lookup"><span data-stu-id="4e307-177">You have to set up all configuration items before creating the application gateway.</span></span> <span data-ttu-id="4e307-178">Az alábbi lépések létrehozzák az Application Gateway erőforráshoz szükséges konfigurációs elemeket.</span><span class="sxs-lookup"><span data-stu-id="4e307-178">The following steps create the configuration items that are needed for an application gateway resource.</span></span>

### <a name="step-1"></a><span data-ttu-id="4e307-179">1. lépés</span><span class="sxs-lookup"><span data-stu-id="4e307-179">Step 1</span></span>

<span data-ttu-id="4e307-180">Hozzon létre egy **gatewayIP01** nevű Application Gateway IP-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="4e307-180">Create an application gateway IP configuration named **gatewayIP01**.</span></span> <span data-ttu-id="4e307-181">Alkalmazásátjáró indításakor szerzi be a konfigurált alhálózat IP-címeit és hálózati forgalom átirányítása a háttér IP-címkészlet IP-címek.</span><span class="sxs-lookup"><span data-stu-id="4e307-181">When application gateway starts, it picks up an IP address from the subnet configured and route network traffic to the IP addresses in the back-end IP pool.</span></span> <span data-ttu-id="4e307-182">Ne feledje, hogy minden példány egy IP-címet vesz fel.</span><span class="sxs-lookup"><span data-stu-id="4e307-182">Keep in mind that each instance takes one IP address.</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $appgatewaysubnet
```

### <a name="step-2"></a><span data-ttu-id="4e307-183">2. lépés</span><span class="sxs-lookup"><span data-stu-id="4e307-183">Step 2</span></span>

<span data-ttu-id="4e307-184">Konfigurálja a háttér IP-címkészletet nevű **pool01** és **pool2** IP-címekkel rendelkező **134.170.185.46**, **134.170.188.221**, **134.170.185.50** a **pool1** és **134.170.186.46**, **134.170.189.221**, **134.170.186.50** a **pool2**.</span><span class="sxs-lookup"><span data-stu-id="4e307-184">Configure the back-end IP address pool named **pool01** and **pool2** with IP addresses **134.170.185.46**, **134.170.188.221**, **134.170.185.50** for **pool1** and **134.170.186.46**, **134.170.189.221**, **134.170.186.50** for **pool2**.</span></span>

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.0.1.100, 10.0.1.101, 10.0.1.102
$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 10.0.1.103, 10.0.1.104, 10.0.1.105
```

<span data-ttu-id="4e307-185">Ebben a példában azonban két háttér-készletek a hálózati forgalmat a kért hely alapján.</span><span class="sxs-lookup"><span data-stu-id="4e307-185">In this example, there are two back-end pools to route network traffic based on the requested site.</span></span> <span data-ttu-id="4e307-186">Egy készlet a "contoso.com" hely származó forgalmat megkapja és egyéb készlet hely "fabrikam.com" származó forgalmat megkapja.</span><span class="sxs-lookup"><span data-stu-id="4e307-186">One pool receives traffic from site "contoso.com" and other pool receives traffic from site "fabrikam.com".</span></span> <span data-ttu-id="4e307-187">Cserélje le a fenti IP-címek hozzáadása a saját alkalmazás IP-cím végpontokat kell.</span><span class="sxs-lookup"><span data-stu-id="4e307-187">You have to replace the preceding IP addresses to add your own application IP address endpoints.</span></span> <span data-ttu-id="4e307-188">Belső IP-címek, helyett is használhatja nyilvános IP-címek, FQDN vagy a virtuális gép hálózati háttér-példányok.</span><span class="sxs-lookup"><span data-stu-id="4e307-188">In place of internal IP addresses, you could also use public IP addresses, FQDN, or a VM's NIC for backend instances.</span></span> <span data-ttu-id="4e307-189">A PowerShell használata IP-cím helyett teljes tartományneveket adja meg "-BackendFQDNs" paraméter.</span><span class="sxs-lookup"><span data-stu-id="4e307-189">To specify FQDNs instead of IPs in PowerShell use "-BackendFQDNs" parameter.</span></span>

### <a name="step-3"></a><span data-ttu-id="4e307-190">3. lépés</span><span class="sxs-lookup"><span data-stu-id="4e307-190">Step 3</span></span>

<span data-ttu-id="4e307-191">Alkalmazásbeállítás átjáró konfigurálása **poolsetting01** és **poolsetting02** az elosztott terhelésű hálózati forgalmat a háttér-készletben.</span><span class="sxs-lookup"><span data-stu-id="4e307-191">Configure application gateway setting **poolsetting01** and **poolsetting02** for the load-balanced network traffic in the back-end pool.</span></span> <span data-ttu-id="4e307-192">Ebben a példában a háttér-készletek különböző háttér-készlet beállításait konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="4e307-192">In this example, you configure different back-end pool settings for the back-end pools.</span></span> <span data-ttu-id="4e307-193">Mindegyik háttérkészlet saját háttérkészlet-beállításokkal rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="4e307-193">Each back-end pool can have its own back-end pool setting.</span></span>

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120
$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a><span data-ttu-id="4e307-194">4. lépés</span><span class="sxs-lookup"><span data-stu-id="4e307-194">Step 4</span></span>

<span data-ttu-id="4e307-195">Konfigurálja az előtérbeli IP-portot egy nyilvános IP-címvégponttal.</span><span class="sxs-lookup"><span data-stu-id="4e307-195">Configure the front-end IP with public IP endpoint.</span></span>

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a><span data-ttu-id="4e307-196">5. lépés</span><span class="sxs-lookup"><span data-stu-id="4e307-196">Step 5</span></span>

<span data-ttu-id="4e307-197">Konfigurálja az előtérbeli portot egy Application Gatewayhez.</span><span class="sxs-lookup"><span data-stu-id="4e307-197">Configure the front-end port for an application gateway.</span></span>

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 443
```

### <a name="step-6"></a><span data-ttu-id="4e307-198">6. lépés</span><span class="sxs-lookup"><span data-stu-id="4e307-198">Step 6</span></span>

<span data-ttu-id="4e307-199">Konfigurálja a két SSL-tanúsítványok esetében a két webhely támogatja az ebben a példában fogjuk.</span><span class="sxs-lookup"><span data-stu-id="4e307-199">Configure two SSL certificates for the two websites we are going to support in this example.</span></span> <span data-ttu-id="4e307-200">Egy tanúsítvány a contoso.com forgalmat, a másik fabrikam.com forgalom.</span><span class="sxs-lookup"><span data-stu-id="4e307-200">One certificate is for contoso.com traffic and the other is for fabrikam.com traffic.</span></span> <span data-ttu-id="4e307-201">Ezeket a tanúsítványokat a webhelyeknek a tanúsítványokat kiadó hitelesítésszolgáltató kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4e307-201">These certificates should be a Certificate Authority issued certificates for your websites.</span></span> <span data-ttu-id="4e307-202">Önaláírt tanúsítványok támogatottak, de nem javasolt éles forgalom.</span><span class="sxs-lookup"><span data-stu-id="4e307-202">Self-signed certificates are supported but not recommended for production traffic.</span></span>

```powershell
$cert01 = New-AzureRmApplicationGatewaySslCertificate -Name contosocert -CertificateFile <file path> -Password <password>
$cert02 = New-AzureRmApplicationGatewaySslCertificate -Name fabrikamcert -CertificateFile <file path> -Password <password>
```

### <a name="step-7"></a><span data-ttu-id="4e307-203">7. lépés</span><span class="sxs-lookup"><span data-stu-id="4e307-203">Step 7</span></span>

<span data-ttu-id="4e307-204">Ebben a példában két figyelők a két webhelyek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4e307-204">Configure two listeners for the two web sites in this example.</span></span> <span data-ttu-id="4e307-205">Ez a lépés konfigurál a nyilvános IP-cím, port és a bejövő forgalom fogadására szolgáló gazdagép figyelők.</span><span class="sxs-lookup"><span data-stu-id="4e307-205">This step configures the listeners for public IP address, port, and host used to receive incoming traffic.</span></span> <span data-ttu-id="4e307-206">Állomásnév paraméter több webhely támogatásához szükséges, és jelölje meg a megfelelő webhelyen, amelynek a forgalom érkezik.</span><span class="sxs-lookup"><span data-stu-id="4e307-206">HostName parameter is required for multiple site support and should be set to the appropriate website for which the traffic is received.</span></span> <span data-ttu-id="4e307-207">RequireServerNameIndication paraméter meg igaz a webhelyeket, amelyeket a támogatásra van szüksége az SSL több állomás esetén.</span><span class="sxs-lookup"><span data-stu-id="4e307-207">RequireServerNameIndication parameter should be set to true for websites that need support for SSL in a multiple host scenario.</span></span> <span data-ttu-id="4e307-208">Ha SSL támogatására szükség, akkor is meg kell adnia az SSL-tanúsítvány, amely védi a webalkalmazáshoz tartozó forgalmat a.</span><span class="sxs-lookup"><span data-stu-id="4e307-208">If SSL support is required, you also need to specify the SSL certificate that is used to secure traffic for that web application.</span></span> <span data-ttu-id="4e307-209">A FrontendIPConfiguration FrontendPort és állomásnév kombinációja egy figyelő egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4e307-209">The combination of FrontendIPConfiguration, FrontendPort, and HostName must be unique to a listener.</span></span> <span data-ttu-id="4e307-210">Minden egyes figyelő több tanúsítvány is támogatja.</span><span class="sxs-lookup"><span data-stu-id="4e307-210">Each listener can support one certificate.</span></span>

```powershell
$listener01 = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "contoso11.com" -RequireServerNameIndication true  -SslCertificate $cert01
$listener02 = New-AzureRmApplicationGatewayHttpListener -Name "listener02" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "fabrikam11.com" -RequireServerNameIndication true -SslCertificate $cert02
```

### <a name="step-8"></a><span data-ttu-id="4e307-211">8. lépés</span><span class="sxs-lookup"><span data-stu-id="4e307-211">Step 8</span></span>

<span data-ttu-id="4e307-212">Hozzon létre két szabály beállítást a két webes alkalmazásokhoz, ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="4e307-212">Create two rule setting for the two web applications in this example.</span></span> <span data-ttu-id="4e307-213">Egy szabály kötelékek együtt, figyelők, a háttérkészlet és a HTTP-beállításait.</span><span class="sxs-lookup"><span data-stu-id="4e307-213">A rule ties together listeners, backend pools and http settings.</span></span> <span data-ttu-id="4e307-214">Ez a lépés konfigurál az Alkalmazásátjáró alapvető útválasztási szabály, egy minden webhelyre vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="4e307-214">This step configures the application gateway to use Basic routing rule, one for each website.</span></span> <span data-ttu-id="4e307-215">Minden webhelyre irányuló forgalom érkezik a beállított figyelő, és a beállított háttérkészlet, a backendhttpsettings beállítások megadott tulajdonságok a rendszer ekkor átirányítja.</span><span class="sxs-lookup"><span data-stu-id="4e307-215">Traffic to each website is received by its configured listener, and is then directed to its configured backend pool, using the properties specified in the BackendHttpSettings.</span></span>

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule01" -RuleType Basic -HttpListener $listener01 -BackendHttpSettings $poolSetting01 -BackendAddressPool $pool1
$rule02 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule02" -RuleType Basic -HttpListener $listener02 -BackendHttpSettings $poolSetting02 -BackendAddressPool $pool2
```

### <a name="step-9"></a><span data-ttu-id="4e307-216">9. lépés</span><span class="sxs-lookup"><span data-stu-id="4e307-216">Step 9</span></span>

<span data-ttu-id="4e307-217">Konfigurálja az Application Gatewayben a példányok számát és a méretet.</span><span class="sxs-lookup"><span data-stu-id="4e307-217">Configure the number of instances and size for the application gateway.</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Medium" -Tier Standard -Capacity 2
```

## <a name="create-application-gateway"></a><span data-ttu-id="4e307-218">Alkalmazásátjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="4e307-218">Create application gateway</span></span>

<span data-ttu-id="4e307-219">Hozzon létre egy alkalmazást az előző lépéseket az összes konfigurációs objektumok.</span><span class="sxs-lookup"><span data-stu-id="4e307-219">Create an application gateway with all configuration objects from the preceding steps.</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener01, $listener02 -RequestRoutingRules $rule01, $rule02 -Sku $sku -SslCertificates $cert01, $cert02
```

> [!IMPORTANT]
> <span data-ttu-id="4e307-220">Alkalmazás átjáró kiépítése hosszú ideig futó művelet, és eltarthat egy ideig.</span><span class="sxs-lookup"><span data-stu-id="4e307-220">Application Gateway provisioning is a long running operation and may take some time to complete.</span></span>
> 
> 

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="4e307-221">Az Application Gateway DNS-nevének beszerzése</span><span class="sxs-lookup"><span data-stu-id="4e307-221">Get application gateway DNS name</span></span>

<span data-ttu-id="4e307-222">Az átjáró létrehozása után a következő lépés a kommunikációra szolgáló előtér konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="4e307-222">Once the gateway is created, the next step is to configure the front end for communication.</span></span> <span data-ttu-id="4e307-223">Nyilvános IP-cím esetén az Application Gateway használatához dinamikusan hozzárendelt DNS-névre van szükség, amely nem valódi név.</span><span class="sxs-lookup"><span data-stu-id="4e307-223">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="4e307-224">Ha szeretné, hogy a végfelhasználók elérjék az Application Gatewayt, használjon egy Application Gateway nyilvános végpontjára mutató CNAME-rekordot.</span><span class="sxs-lookup"><span data-stu-id="4e307-224">To ensure end users can hit the application gateway, a CNAME record can be used to point to the public endpoint of the application gateway.</span></span> <span data-ttu-id="4e307-225">[Egyéni tartománynév konfigurálása az Azure-ban](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4e307-225">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="4e307-226">A művelet végrehajtásához az Application Gateway részleteinek beszerzésére és a kapcsolódó IP/DNS-név lekérésére van szükség az Application Gatewayhez csatolt PublicIPAddress használatával.</span><span class="sxs-lookup"><span data-stu-id="4e307-226">To do this, retrieve details of the application gateway and its associated IP/DNS name using the PublicIPAddress element attached to the application gateway.</span></span> <span data-ttu-id="4e307-227">Az Application Gateway DNS-nevének használatával létrehozhat egy CNAME rekordot, amely a két webalkalmazást erre a DNS-névre irányítja.</span><span class="sxs-lookup"><span data-stu-id="4e307-227">The application gateway's DNS name should be used to create a CNAME record, which points the two web applications to this DNS name.</span></span> <span data-ttu-id="4e307-228">Az A-bejegyzések használata nem javasolt, mivel a virtuális IP-cím változhat az Application Gateway újraindításakor.</span><span class="sxs-lookup"><span data-stu-id="4e307-228">The use of A-records is not recommended since the VIP may change on restart of application gateway.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="4e307-229">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4e307-229">Next steps</span></span>

<span data-ttu-id="4e307-230">Ismerje meg, hogyan védi meg a webhelyek [Application Gateway - webalkalmazási tűzfal](application-gateway-webapplicationfirewall-overview.md)</span><span class="sxs-lookup"><span data-stu-id="4e307-230">Learn how to protect your websites with [Application Gateway - Web Application Firewall](application-gateway-webapplicationfirewall-overview.md)</span></span>

