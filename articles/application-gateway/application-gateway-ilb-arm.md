---
title: "Azure Alkalmazásátjáró használata belső elosztott terhelésű - PowerShell |} Microsoft Docs"
description: "Ez az oldal utasításokat tartalmaz egy belső terheléselosztóval (ILB) rendelkező Azure Application Gateway létrehozásához, konfigurálásához, indításához és törléséhez az Azure Resource Manager számára"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 75cfd5a2-e378-4365-99ee-a2b2abda2e0d
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: d218eab7e9f124e4825a8a781b4eeb0dcca58b4a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb-by-using-azure-resource-manager"></a><span data-ttu-id="1ed5b-103">Belső terheléselosztóval (ILB) rendelkező Application Gateway létrehozása az Azure Resource Manager használatával</span><span class="sxs-lookup"><span data-stu-id="1ed5b-103">Create an application gateway with an internal load balancer (ILB) by using Azure Resource Manager</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="1ed5b-104">Klasszikus Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="1ed5b-104">Azure Classic PowerShell</span></span>](application-gateway-ilb.md)
> * [<span data-ttu-id="1ed5b-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="1ed5b-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ilb-arm.md)

<span data-ttu-id="1ed5b-106">Az Azure Application Gateway konfigurálható egy internetre irányuló virtuális IP-címhez vagy egy internettel nem érintkező belső végponthoz, más néven egy belső terheléselosztó (ILB) végponthoz.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-106">Azure Application Gateway can be configured with an Internet-facing VIP or with an internal endpoint that is not exposed to the Internet, also known as an internal load balancer (ILB) endpoint.</span></span> <span data-ttu-id="1ed5b-107">Az átjáró ILB-vel történő konfigurálása a belső üzletági alkalmazásoknál hasznos, amelyek nem érintkeznek az internettel.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-107">Configuring the gateway with an ILB is useful for internal line-of-business applications that are not exposed to the Internet.</span></span> <span data-ttu-id="1ed5b-108">Emellett a többrétegű alkalmazások szolgáltatásai és rétegei számára is hasznos, amelyek egy internettel nem érintkező biztonsági korláton belül vannak, de attól még igényelik az időszeleteléses terheléselosztást, a munkamenet tartós használatát vagy a Secure Sockets Layer (SSL) lezárását.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-108">It's also useful for services and tiers within a multi-tier application that sit in a security boundary that is not exposed to the Internet but still require round-robin load distribution, session stickiness, or Secure Sockets Layer (SSL) termination.</span></span>

<span data-ttu-id="1ed5b-109">Ez a cikk részletesen ismerteti egy Application Gateway ILB-hez történő konfigurálásának lépéseit.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-109">This article walks you through the steps to configure an application gateway with an ILB.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="1ed5b-110">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="1ed5b-110">Before you begin</span></span>

1. <span data-ttu-id="1ed5b-111">Telepítse az Azure PowerShell-parancsmagok legújabb verzióját a Webplatform-telepítővel.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-111">Install the latest version of the Azure PowerShell cmdlets by using the Web Platform Installer.</span></span> <span data-ttu-id="1ed5b-112">A [Letöltések lap](https://azure.microsoft.com/downloads/) **Windows PowerShell** szakaszából letöltheti és telepítheti a legújabb verziót.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-112">You can download and install the latest version from the **Windows PowerShell** section of the [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="1ed5b-113">Létre kell hozni egy virtuális hálózatot és alhálózatot az Application Gateway számára.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-113">You create a virtual network and a subnet for Application Gateway.</span></span> <span data-ttu-id="1ed5b-114">Győződjön meg arról, hogy egy virtuális gép vagy felhőalapú telepítés sem használja az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-114">Make sure that no virtual machines or cloud deployments are using the subnet.</span></span> <span data-ttu-id="1ed5b-115">Az Application Gateway-nek egyedül kell lennie a virtuális hálózat alhálózatán.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-115">Application Gateway must be by itself in a virtual network subnet.</span></span>
3. <span data-ttu-id="1ed5b-116">A kiszolgálóknak, amelyeket az Application Gateway használatára konfigurál, már létezniük kell, illetve a virtuális hálózatban vagy hozzárendelt nyilvános/virtuális IP-címmel létrehozott végpontokkal kell rendelkezniük.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-116">The servers that you configure to use the application gateway must exist or have their endpoints created either in the virtual network or with a public IP/VIP assigned.</span></span>

## <a name="what-is-required-to-create-an-application-gateway"></a><span data-ttu-id="1ed5b-117">Mire van szükség egy Application Gateway létrehozásához?</span><span class="sxs-lookup"><span data-stu-id="1ed5b-117">What is required to create an application gateway?</span></span>

* <span data-ttu-id="1ed5b-118">**Háttér-kiszolgálókészlet:** A háttérkiszolgálók IP-címeinek listája.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-118">**Back-end server pool:** The list of IP addresses of the back-end servers.</span></span> <span data-ttu-id="1ed5b-119">A lenti listán szereplő IP-címeknek a virtuális hálózathoz kell tartozniuk, egy Application Gateway számára fenntartott másik alhálózatban, vagy nyilvános/virtuális IP-címnek kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-119">The IP addresses listed should either belong to the virtual network but in a different subnet for the application gateway or should be a public IP/VIP.</span></span>
* <span data-ttu-id="1ed5b-120">**Háttér-kiszolgálókészlet beállításai:** Minden készletnek vannak beállításai, például port, protokoll vagy cookie-alapú affinitás.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-120">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="1ed5b-121">Ezek a beállítások egy adott készlethez kapcsolódnak, és a készlet minden kiszolgálójára érvényesek.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-121">These settings are tied to a pool and are applied to all servers within the pool.</span></span>
* <span data-ttu-id="1ed5b-122">**Előtérbeli port:** Az Application Gateway-en megnyitott nyilvános port.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-122">**Front-end port:** This port is the public port that is opened on the application gateway.</span></span> <span data-ttu-id="1ed5b-123">Amikor a forgalom eléri ezt a portot, a port átirányítja az egyik háttérkiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-123">Traffic hits this port, and then gets redirected to one of the back-end servers.</span></span>
* <span data-ttu-id="1ed5b-124">**Figyelő:** A figyelő egy előtérbeli porttal, egy protokollal (Http vagy Https, kis- és a nagybetűk megkülönböztetésével) és az SSL tanúsítványnévvel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-124">**Listener:** The listener has a front-end port, a protocol (Http or Https, these are case-sensitive), and the SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="1ed5b-125">**Szabály:** A szabály összeköti a figyelőt és a háttérkiszolgáló-készletet, és meghatározza, hogy mely háttérkiszolgáló-készletre legyen átirányítva a forgalom, ha elér egy adott figyelőt.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-125">**Rule:** The rule binds the listener and the back-end server pool and defines which back-end server pool the traffic should be directed to when it hits a particular listener.</span></span> <span data-ttu-id="1ed5b-126">Jelenleg csak a *basic* szabály támogatott.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-126">Currently, only the *basic* rule is supported.</span></span> <span data-ttu-id="1ed5b-127">A *basic* szabály a ciklikus időszeleteléses terheléselosztás.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-127">The *basic* rule is round-robin load distribution.</span></span>

## <a name="create-an-application-gateway"></a><span data-ttu-id="1ed5b-128">Application Gateway létrehozása</span><span class="sxs-lookup"><span data-stu-id="1ed5b-128">Create an application gateway</span></span>

<span data-ttu-id="1ed5b-129">A klasszikus Azure és az Azure Resource Manager használata abban tér el egymástól, hogy más sorrendben kell létrehoznia az Application Gateway-t és a konfigurálást igénylő elemeket.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-129">The difference between using Azure Classic and Azure Resource Manager is the order in which you create the application gateway and the items that need to be configured.</span></span>
<span data-ttu-id="1ed5b-130">A Resource Managerrel az Application Gateway összes alkotóelemét külön kell konfigurálni, és csak utána kell őket összeállítani az Application Gateway-erőforrás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-130">With Resource Manager, all items that make an application gateway is configured individually and then put together to create the application gateway resource.</span></span>

<span data-ttu-id="1ed5b-131">A következő lépések szükségesek egy Application Gateway létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="1ed5b-131">Here are the steps that are needed to create an application gateway:</span></span>

1. <span data-ttu-id="1ed5b-132">Erőforráscsoport létrehozása a Resource Managerhez</span><span class="sxs-lookup"><span data-stu-id="1ed5b-132">Create a resource group for Resource Manager</span></span>
2. <span data-ttu-id="1ed5b-133">Virtuális hálózat és alhálózat létrehozása az Application Gateway számára</span><span class="sxs-lookup"><span data-stu-id="1ed5b-133">Create a virtual network and a subnet for the application gateway</span></span>
3. <span data-ttu-id="1ed5b-134">Hozzon létre egy Application Gateway konfigurációs objektumot</span><span class="sxs-lookup"><span data-stu-id="1ed5b-134">Create an application gateway configuration object</span></span>
4. <span data-ttu-id="1ed5b-135">Application Gateway erőforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1ed5b-135">Create an application gateway resource</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="1ed5b-136">Erőforráscsoport létrehozása a Resource Managerhez</span><span class="sxs-lookup"><span data-stu-id="1ed5b-136">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="1ed5b-137">Az Azure Resource Manager parancsmagjainak használatához váltson át PowerShell módba.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-137">Make sure that you switch PowerShell mode to use the Azure Resource Manager cmdlets.</span></span> <span data-ttu-id="1ed5b-138">További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="1ed5b-138">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="1ed5b-139">1. lépés</span><span class="sxs-lookup"><span data-stu-id="1ed5b-139">Step 1</span></span>

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a><span data-ttu-id="1ed5b-140">2. lépés</span><span class="sxs-lookup"><span data-stu-id="1ed5b-140">Step 2</span></span>

<span data-ttu-id="1ed5b-141">Keresse meg a fiókot az előfizetésekben.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-141">Check the subscriptions for the account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="1ed5b-142">A rendszer kérni fogja a hitelesítő adatokkal történő hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-142">You are prompted to authenticate with your credentials.</span></span>

### <a name="step-3"></a><span data-ttu-id="1ed5b-143">3. lépés</span><span class="sxs-lookup"><span data-stu-id="1ed5b-143">Step 3</span></span>

<span data-ttu-id="1ed5b-144">Válassza ki, hogy melyik Azure előfizetést fogja használni.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-144">Choose which of your Azure subscriptions to use.</span></span>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a><span data-ttu-id="1ed5b-145">4. lépés</span><span class="sxs-lookup"><span data-stu-id="1ed5b-145">Step 4</span></span>

<span data-ttu-id="1ed5b-146">Hozzon létre egy új erőforráscsoportot (hagyja ki ezt a lépést, ha egy meglévő erőforráscsoportot használ).</span><span class="sxs-lookup"><span data-stu-id="1ed5b-146">Create a new resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
New-AzureRmResourceGroup -Name appgw-rg -location "West US"
```

<span data-ttu-id="1ed5b-147">Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport adjon meg egy helyet.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-147">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="1ed5b-148">Ez szolgál az erőforráscsoport erőforrásainak alapértelmezett helyeként.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-148">This is used as the default location for resources in that resource group.</span></span> <span data-ttu-id="1ed5b-149">Győződjön meg arról, hogy az Application Gateway létrehozására irányuló összes parancs ugyanazt az erőforráscsoportot használja.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-149">Make sure that all commands to create an application gateway uses the same resource group.</span></span>

<span data-ttu-id="1ed5b-150">Az előző példában létrehozott egy erőforráscsoport neve "appgw-rg" és "Velünk nyugati" helyen.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-150">In the preceding example, we created a resource group called "appgw-rg" and location "West US".</span></span>

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a><span data-ttu-id="1ed5b-151">Virtuális hálózat és alhálózat létrehozása az Application Gateway számára</span><span class="sxs-lookup"><span data-stu-id="1ed5b-151">Create a virtual network and a subnet for the application gateway</span></span>

<span data-ttu-id="1ed5b-152">Az alábbi példa bemutatja, hogyan hozhat létre egy virtuális hálózatot a Resource Manager használatával:</span><span class="sxs-lookup"><span data-stu-id="1ed5b-152">The following example shows how to create a virtual network by using Resource Manager:</span></span>

### <a name="step-1"></a><span data-ttu-id="1ed5b-153">1. lépés</span><span class="sxs-lookup"><span data-stu-id="1ed5b-153">Step 1</span></span>

```powershell
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

<span data-ttu-id="1ed5b-154">Ebben a lépésben a cím tartomány 10.0.0.0/24 rendel hozzá egy virtuális hálózat létrehozásához használandó alhálózati változó.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-154">This step assigns the address range 10.0.0.0/24 to a subnet variable to be used to create a virtual network.</span></span>

### <a name="step-2"></a><span data-ttu-id="1ed5b-155">2. lépés</span><span class="sxs-lookup"><span data-stu-id="1ed5b-155">Step 2</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnetconfig
```

<span data-ttu-id="1ed5b-156">Ebben a lépésben egy csoport "appgw-rg erőforrás" a "appgwvnet" nevű az USA nyugati régiója régió a előtag 10.0.0.0/16 használata a 10.0.0.0/24 alhálózat virtuális hálózatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-156">This step creates a virtual network named "appgwvnet" in resource group "appgw-rg" for the West US region using the prefix 10.0.0.0/16 with subnet 10.0.0.0/24.</span></span>

### <a name="step-3"></a><span data-ttu-id="1ed5b-157">3. lépés</span><span class="sxs-lookup"><span data-stu-id="1ed5b-157">Step 3</span></span>

```powershell
$subnet = $vnet.subnets[0]
```

<span data-ttu-id="1ed5b-158">Ez a lépés rendel $subnet változó a következő lépéseket a alhálózati objektum alhálózatát.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-158">This step assigns the subnet object to variable $subnet for the next steps.</span></span>

## <a name="create-an-application-gateway-configuration-object"></a><span data-ttu-id="1ed5b-159">Hozzon létre egy Application Gateway konfigurációs objektumot</span><span class="sxs-lookup"><span data-stu-id="1ed5b-159">Create an application gateway configuration object</span></span>

### <a name="step-1"></a><span data-ttu-id="1ed5b-160">1. lépés</span><span class="sxs-lookup"><span data-stu-id="1ed5b-160">Step 1</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

<span data-ttu-id="1ed5b-161">Ebben a lépésben létrehozza a "gatewayIP01" nevű alkalmazás átjáró IP-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-161">This step creates an application gateway IP configuration named "gatewayIP01".</span></span> <span data-ttu-id="1ed5b-162">Amikor az Application Gateway elindul, a konfigurált alhálózatból felvesz egy IP-címet, és a hálózati forgalmat a háttérbeli IP-készlet IP-címeihez irányítja.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-162">When Application Gateway starts, it picks up an IP address from the subnet configured and route network traffic to the IP addresses in the back-end IP pool.</span></span> <span data-ttu-id="1ed5b-163">Ne feledje, hogy minden példány egy IP-címet vesz fel.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-163">Keep in mind that each instance takes one IP address.</span></span>

### <a name="step-2"></a><span data-ttu-id="1ed5b-164">2. lépés</span><span class="sxs-lookup"><span data-stu-id="1ed5b-164">Step 2</span></span>

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.1.1.8,10.1.1.9,10.1.1.10
```

<span data-ttu-id="1ed5b-165">Ez a lépés konfigurál a háttér IP-címkészlet neve "pool01" IP-címek "10.1.1.8, 10.1.1.9, 10.1.1.10".</span><span class="sxs-lookup"><span data-stu-id="1ed5b-165">This step configures the back-end IP address pool named "pool01" with IP addresses "10.1.1.8, 10.1.1.9, 10.1.1.10".</span></span> <span data-ttu-id="1ed5b-166">Ezek az IP-címek fogadják az előtérbeli IP-végpontból érkező hálózati forgalmat.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-166">Those are the IP addresses that receive the network traffic that comes from the front-end IP endpoint.</span></span> <span data-ttu-id="1ed5b-167">Az előző IP-címeket lecseréli a saját alkalmazása IP-címvégpontjaira.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-167">You replace the preceding IP addresses to add your own application IP address endpoints.</span></span>

### <a name="step-3"></a><span data-ttu-id="1ed5b-168">3. lépés</span><span class="sxs-lookup"><span data-stu-id="1ed5b-168">Step 3</span></span>

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

<span data-ttu-id="1ed5b-169">Ez a lépés konfigurál alkalmazás átjáró beállítás "poolsetting01" a terhelés eloszlik hálózati forgalmat a háttér-készletben.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-169">This step configures application gateway setting "poolsetting01" for the load balanced network traffic in the back-end pool.</span></span>

### <a name="step-4"></a><span data-ttu-id="1ed5b-170">4. lépés</span><span class="sxs-lookup"><span data-stu-id="1ed5b-170">Step 4</span></span>

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80
```

<span data-ttu-id="1ed5b-171">Ez a lépés konfigurál a "frontendport01" nevű a Példánynak az előtér-IP-port.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-171">This step configures the front-end IP port named "frontendport01" for the ILB.</span></span>

### <a name="step-5"></a><span data-ttu-id="1ed5b-172">5. lépés</span><span class="sxs-lookup"><span data-stu-id="1ed5b-172">Step 5</span></span>

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -Subnet $subnet
```

<span data-ttu-id="1ed5b-173">Ezzel a lépéssel hoz létre az előtér-IP-konfiguráció "fipconfig01" néven, és társítja azt egy privát IP-cím az az aktuális virtuális hálózati alhálózat.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-173">This step creates the front-end IP configuration called "fipconfig01" and associates it with a private IP from the current virtual network subnet.</span></span>

### <a name="step-6"></a><span data-ttu-id="1ed5b-174">6. lépés</span><span class="sxs-lookup"><span data-stu-id="1ed5b-174">Step 6</span></span>

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp
```

<span data-ttu-id="1ed5b-175">Ebben a lépésben létrehozza a "listener01" nevű figyelő, és hozzárendeli az előtér-portot az előtér-IP-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-175">This step creates the listener called "listener01" and associates the front-end port to the front-end IP configuration.</span></span>

### <a name="step-7"></a><span data-ttu-id="1ed5b-176">7. lépés</span><span class="sxs-lookup"><span data-stu-id="1ed5b-176">Step 7</span></span>

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

<span data-ttu-id="1ed5b-177">Ebben a lépésben létrehozza a terheléselosztó útválasztási szabályának neve "rule01", amely a terheléselosztó GUID konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-177">This step creates the load balancer routing rule called "rule01" that configures the load balancer behavior.</span></span>

### <a name="step-8"></a><span data-ttu-id="1ed5b-178">8. lépés</span><span class="sxs-lookup"><span data-stu-id="1ed5b-178">Step 8</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

<span data-ttu-id="1ed5b-179">Ez a lépés konfigurál az Alkalmazásátjáró meg.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-179">This step configures the instance size of the application gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="1ed5b-180">Az *InstanceCount* alapértelmezett értéke 2, a maximális értéke pedig 10.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-180">The default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="1ed5b-181">A *GatewaySize* alapértelmezett értéke Közepes.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-181">The default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="1ed5b-182">A Standard_Small, Standard_Medium és a Standard_Large lehetőségek közül választhat.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-182">You can choose between Standard_Small, Standard_Medium, and Standard_Large.</span></span>

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a><span data-ttu-id="1ed5b-183">Application Gateway létrehozása a New-AzureApplicationGateway használatával</span><span class="sxs-lookup"><span data-stu-id="1ed5b-183">Create an application gateway by using New-AzureApplicationGateway</span></span>

<span data-ttu-id="1ed5b-184">Az előző lépéseket az összes konfigurációelemre Alkalmazásátjáró hoz létre.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-184">Creates an application gateway with all configuration items from the preceding steps.</span></span> <span data-ttu-id="1ed5b-185">Ebben a példában az Application Gateway neve „appgwtest”.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-185">In this example, the application gateway is called "appgwtest".</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

<span data-ttu-id="1ed5b-186">Ebben a lépésben Alkalmazásátjáró az előző lépések származó összes konfigurációelemeket hoz létre.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-186">This step creates an application gateway with all configuration items from the preceding steps.</span></span> <span data-ttu-id="1ed5b-187">Ebben a példában az Application Gateway neve „appgwtest”.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-187">In the example, the application gateway is called "appgwtest".</span></span>

## <a name="delete-an-application-gateway"></a><span data-ttu-id="1ed5b-188">Application Gateway törlése</span><span class="sxs-lookup"><span data-stu-id="1ed5b-188">Delete an application gateway</span></span>

<span data-ttu-id="1ed5b-189">Az Alkalmazásátjáró törlése, a következő lépések sorrendben kell:</span><span class="sxs-lookup"><span data-stu-id="1ed5b-189">To delete an application gateway, you need to do the following steps in order:</span></span>

1. <span data-ttu-id="1ed5b-190">Állítsa le az átjárót a `Stop-AzureRmApplicationGateway` parancsmaggal.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-190">Use the `Stop-AzureRmApplicationGateway` cmdlet to stop the gateway.</span></span>
2. <span data-ttu-id="1ed5b-191">Távolítsa el az átjárót a `Remove-AzureRmApplicationGateway` parancsmaggal.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-191">Use the `Remove-AzureRmApplicationGateway` cmdlet to remove the gateway.</span></span>
3. <span data-ttu-id="1ed5b-192">A `Get-AzureApplicationGateway` parancsmaggal győződjön meg arról, hogy az átjáró el lett távolítva.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-192">Verify that the gateway has been removed by using the `Get-AzureApplicationGateway` cmdlet.</span></span>

### <a name="step-1"></a><span data-ttu-id="1ed5b-193">1. lépés</span><span class="sxs-lookup"><span data-stu-id="1ed5b-193">Step 1</span></span>

<span data-ttu-id="1ed5b-194">Társítsa az Application Gateway objektumot a „$getgw” változóhoz.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-194">Get the application gateway object and associate it to a variable "$getgw".</span></span>

```powershell
$getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

### <a name="step-2"></a><span data-ttu-id="1ed5b-195">2. lépés</span><span class="sxs-lookup"><span data-stu-id="1ed5b-195">Step 2</span></span>

<span data-ttu-id="1ed5b-196">Állítsa le az Application Gatewayt a `Stop-AzureRmApplicationGateway` parancsmaggal.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-196">Use `Stop-AzureRmApplicationGateway` to stop the application gateway.</span></span> <span data-ttu-id="1ed5b-197">Ez a példa bemutatja a `Stop-AzureRmApplicationGateway` az első sorban, a parancsmag kimenete követ.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-197">This sample shows the `Stop-AzureRmApplicationGateway` cmdlet on the first line, followed by the output.</span></span>

```powershell
Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  
```

```
VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8
```

<span data-ttu-id="1ed5b-198">Amint az Application Gateway leállított állapotba kerül, távolítsa el a szolgáltatást a `Remove-AzureRmApplicationGateway` parancsmaggal.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-198">Once the application gateway is in a stopped state, use the `Remove-AzureRmApplicationGateway` cmdlet to remove the service.</span></span>

```powershell
Remove-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Force
```

```
VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301
```

> [!NOTE]
> <span data-ttu-id="1ed5b-199">A **-force** kapcsolóval le lehet tiltani az eltávolítás jóváhagyása üzenetet.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-199">The **-force** switch can be used to suppress the remove confirmation message.</span></span>

<span data-ttu-id="1ed5b-200">A szolgáltatás eltávolításának ellenőrzéséhez használhatja a `Get-AzureRmApplicationGateway` parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-200">To verify that the service has been removed, you can use the `Get-AzureRmApplicationGateway` cmdlet.</span></span> <span data-ttu-id="1ed5b-201">Ez a lépés nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-201">This step is not required.</span></span>

```powershell
Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

```
VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
```

## <a name="next-steps"></a><span data-ttu-id="1ed5b-202">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1ed5b-202">Next steps</span></span>

<span data-ttu-id="1ed5b-203">Ha SSL-alapú kiszervezést szeretne konfigurálni: [Application Gateway konfigurálása SSL-alapú kiszervezéshez](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="1ed5b-203">If you want to configure SSL offload, see [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="1ed5b-204">Ha konfigurálni szeretne egy ILB-vel használni kívánt Application Gateway-t: [Application Gateway létrehozása belső terheléselosztóval (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="1ed5b-204">If you want to configure an application gateway to use with an ILB, see [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="1ed5b-205">Ha további általános információra van szüksége a terheléselosztás beállításaival kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="1ed5b-205">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="1ed5b-206">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="1ed5b-206">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="1ed5b-207">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="1ed5b-207">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

