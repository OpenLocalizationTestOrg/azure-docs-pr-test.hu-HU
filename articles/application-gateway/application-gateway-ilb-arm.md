---
title: "a belső terheléselosztó - PowerShell Azure Application Gateway aaaUsing |} Microsoft Docs"
description: "Ezen a lapon útmutatás toocreate, konfigurálása, indítsa el, és a belső terheléselosztón (ILB) Azure Alkalmazásátjáró törlése az Azure Resource Manager"
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
ms.openlocfilehash: dd0d7e954b1fa219ae6ebe42cb4b479dbcf08653
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb-by-using-azure-resource-manager"></a><span data-ttu-id="2bb35-103">Belső terheléselosztóval (ILB) rendelkező Application Gateway létrehozása az Azure Resource Manager használatával</span><span class="sxs-lookup"><span data-stu-id="2bb35-103">Create an application gateway with an internal load balancer (ILB) by using Azure Resource Manager</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="2bb35-104">Klasszikus Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="2bb35-104">Azure Classic PowerShell</span></span>](application-gateway-ilb.md)
> * [<span data-ttu-id="2bb35-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="2bb35-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ilb-arm.md)

<span data-ttu-id="2bb35-106">Azure Application Gateway egy internetre irányuló virtuális IP-címre, vagy egy belső végponttal, amely nincs kitett toohello Internet, más néven belső terheléselosztó (ILB) végpont konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="2bb35-106">Azure Application Gateway can be configured with an Internet-facing VIP or with an internal endpoint that is not exposed toohello Internet, also known as an internal load balancer (ILB) endpoint.</span></span> <span data-ttu-id="2bb35-107">Hello átjáró konfigurálása egy ILB akkor hasznos, amelyek nincsenek kitett toohello Internet belső üzleti alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="2bb35-107">Configuring hello gateway with an ILB is useful for internal line-of-business applications that are not exposed toohello Internet.</span></span> <span data-ttu-id="2bb35-108">Szintén hasznosak lehetnek a szolgáltatások és rétegek belül egy többrétegű alkalmazást, amely nincs kitett toohello internetes biztonsági határt a elhelyezkedik, de továbbra is a ciklikus multiplexelési igénylő betöltése terjesztési, a munkamenet tölcsérútvonalak vagy a Secure Sockets Layer (SSL) megszüntetése.</span><span class="sxs-lookup"><span data-stu-id="2bb35-108">It's also useful for services and tiers within a multi-tier application that sit in a security boundary that is not exposed toohello Internet but still require round-robin load distribution, session stickiness, or Secure Sockets Layer (SSL) termination.</span></span>

<span data-ttu-id="2bb35-109">Ez a cikk bemutatja, hogyan hello lépéseket tooconfigure egy ILB az Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="2bb35-109">This article walks you through hello steps tooconfigure an application gateway with an ILB.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="2bb35-110">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="2bb35-110">Before you begin</span></span>

1. <span data-ttu-id="2bb35-111">Webplatform-telepítő hello segítségével hello hello Azure PowerShell-parancsmagok legújabb verzióját telepítse.</span><span class="sxs-lookup"><span data-stu-id="2bb35-111">Install hello latest version of hello Azure PowerShell cmdlets by using hello Web Platform Installer.</span></span> <span data-ttu-id="2bb35-112">Töltse le, és telepítse hello legújabb verziót a hello **Windows PowerShell** hello szakasza [letöltési oldalon](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="2bb35-112">You can download and install hello latest version from hello **Windows PowerShell** section of hello [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="2bb35-113">Létre kell hozni egy virtuális hálózatot és alhálózatot az Application Gateway számára.</span><span class="sxs-lookup"><span data-stu-id="2bb35-113">You create a virtual network and a subnet for Application Gateway.</span></span> <span data-ttu-id="2bb35-114">Győződjön meg arról, hogy egyetlen virtuális gépek vagy a felhőben történő alkalmazáshoz hello alhálózat használja.</span><span class="sxs-lookup"><span data-stu-id="2bb35-114">Make sure that no virtual machines or cloud deployments are using hello subnet.</span></span> <span data-ttu-id="2bb35-115">Az Application Gateway-nek egyedül kell lennie a virtuális hálózat alhálózatán.</span><span class="sxs-lookup"><span data-stu-id="2bb35-115">Application Gateway must be by itself in a virtual network subnet.</span></span>
3. <span data-ttu-id="2bb35-116">hello kiszolgálók toouse hello Alkalmazásátjáró konfigurált léteznie kell, különben a végpontok hello virtuális hálózatban vagy a nyilvános IP-cím/VIP rendelt hozzá.</span><span class="sxs-lookup"><span data-stu-id="2bb35-116">hello servers that you configure toouse hello application gateway must exist or have their endpoints created either in hello virtual network or with a public IP/VIP assigned.</span></span>

## <a name="what-is-required-toocreate-an-application-gateway"></a><span data-ttu-id="2bb35-117">Mi az a szükséges toocreate olyan átjárót?</span><span class="sxs-lookup"><span data-stu-id="2bb35-117">What is required toocreate an application gateway?</span></span>

* <span data-ttu-id="2bb35-118">**Háttér-kiszolgálófiók készlet:** hello hello háttér-kiszolgálók IP-címek listáját.</span><span class="sxs-lookup"><span data-stu-id="2bb35-118">**Back-end server pool:** hello list of IP addresses of hello back-end servers.</span></span> <span data-ttu-id="2bb35-119">hello IP-cím jelenik meg vagy tartozik toohello virtuális hálózat, de az Alkalmazásátjáró hello egy másik alhálózat vagy egy nyilvános IP-cím/VIP kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2bb35-119">hello IP addresses listed should either belong toohello virtual network but in a different subnet for hello application gateway or should be a public IP/VIP.</span></span>
* <span data-ttu-id="2bb35-120">**Háttér-kiszolgálókészlet beállításai:** Minden készletnek vannak beállításai, például port, protokoll vagy cookie-alapú affinitás.</span><span class="sxs-lookup"><span data-stu-id="2bb35-120">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="2bb35-121">Ezek a beállítások esetén tooa kapcsolt verem és a hello készlet alkalmazott tooall-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="2bb35-121">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="2bb35-122">**Előtér-port:** Ez a port nem hello nyilvános portot, amelyet a hello Alkalmazásátjáró meg van nyitva.</span><span class="sxs-lookup"><span data-stu-id="2bb35-122">**Front-end port:** This port is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="2bb35-123">Forgalom találatok ezt a portot, és lekérdezi átirányítja tooone hello háttér-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="2bb35-123">Traffic hits this port, and then gets redirected tooone of hello back-end servers.</span></span>
* <span data-ttu-id="2bb35-124">**Figyelő:** hello figyelő rendelkezik egy előtér-portot, a protokollt (Http vagy Https, amelyek kis-és nagybetűket), és hello SSL tanúsítvány neve (ha az SSL beállításának-kiszervezés).</span><span class="sxs-lookup"><span data-stu-id="2bb35-124">**Listener:** hello listener has a front-end port, a protocol (Http or Https, these are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="2bb35-125">**Szabály:** hello szabály hello figyelő és hello háttér-kiszolgálófiók alkalmazáskészlet van kötve, és azt határozza meg, mely háttér-kiszolgálófiók készlet hello forgalom irányított toowhen találatok száma a egy adott figyelő.</span><span class="sxs-lookup"><span data-stu-id="2bb35-125">**Rule:** hello rule binds hello listener and hello back-end server pool and defines which back-end server pool hello traffic should be directed toowhen it hits a particular listener.</span></span> <span data-ttu-id="2bb35-126">Jelenleg csak hello *alapvető* szabály használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="2bb35-126">Currently, only hello *basic* rule is supported.</span></span> <span data-ttu-id="2bb35-127">Hello *alapvető* szabály-e időszeletelés terheléselosztási.</span><span class="sxs-lookup"><span data-stu-id="2bb35-127">hello *basic* rule is round-robin load distribution.</span></span>

## <a name="create-an-application-gateway"></a><span data-ttu-id="2bb35-128">Application Gateway létrehozása</span><span class="sxs-lookup"><span data-stu-id="2bb35-128">Create an application gateway</span></span>

<span data-ttu-id="2bb35-129">hello közötti Azure klasszikus és az Azure Resource Manager használatával különbség hello rendelés hello Alkalmazásátjáró és hello elemeket toobe konfigurálva kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="2bb35-129">hello difference between using Azure Classic and Azure Resource Manager is hello order in which you create hello application gateway and hello items that need toobe configured.</span></span>
<span data-ttu-id="2bb35-130">A Resource Manager az összes olyan átjárót elemeit van konfigurálva külön-külön és helyreállíthatósága majd toocreate hello alkalmazás átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="2bb35-130">With Resource Manager, all items that make an application gateway is configured individually and then put together toocreate hello application gateway resource.</span></span>

<span data-ttu-id="2bb35-131">Az alábbiakban a szükséges toocreate Alkalmazásátjáró hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2bb35-131">Here are hello steps that are needed toocreate an application gateway:</span></span>

1. <span data-ttu-id="2bb35-132">Erőforráscsoport létrehozása a Resource Managerhez</span><span class="sxs-lookup"><span data-stu-id="2bb35-132">Create a resource group for Resource Manager</span></span>
2. <span data-ttu-id="2bb35-133">Hozzon létre egy virtuális hálózatot és hello Alkalmazásátjáró alhálózatot</span><span class="sxs-lookup"><span data-stu-id="2bb35-133">Create a virtual network and a subnet for hello application gateway</span></span>
3. <span data-ttu-id="2bb35-134">Hozzon létre egy Application Gateway konfigurációs objektumot</span><span class="sxs-lookup"><span data-stu-id="2bb35-134">Create an application gateway configuration object</span></span>
4. <span data-ttu-id="2bb35-135">Application Gateway erőforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="2bb35-135">Create an application gateway resource</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="2bb35-136">Erőforráscsoport létrehozása a Resource Managerhez</span><span class="sxs-lookup"><span data-stu-id="2bb35-136">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="2bb35-137">Győződjön meg arról, hogy a PowerShell mód toouse hello Azure Resource Manager parancsmagok váltani.</span><span class="sxs-lookup"><span data-stu-id="2bb35-137">Make sure that you switch PowerShell mode toouse hello Azure Resource Manager cmdlets.</span></span> <span data-ttu-id="2bb35-138">További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="2bb35-138">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="2bb35-139">1. lépés</span><span class="sxs-lookup"><span data-stu-id="2bb35-139">Step 1</span></span>

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a><span data-ttu-id="2bb35-140">2. lépés</span><span class="sxs-lookup"><span data-stu-id="2bb35-140">Step 2</span></span>

<span data-ttu-id="2bb35-141">Hello előfizetések hello fiók ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="2bb35-141">Check hello subscriptions for hello account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="2bb35-142">A hitelesítő adataival felszólító tooauthenticate áll.</span><span class="sxs-lookup"><span data-stu-id="2bb35-142">You are prompted tooauthenticate with your credentials.</span></span>

### <a name="step-3"></a><span data-ttu-id="2bb35-143">3. lépés</span><span class="sxs-lookup"><span data-stu-id="2bb35-143">Step 3</span></span>

<span data-ttu-id="2bb35-144">Válassza ki, amely az Azure-előfizetések toouse.</span><span class="sxs-lookup"><span data-stu-id="2bb35-144">Choose which of your Azure subscriptions toouse.</span></span>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a><span data-ttu-id="2bb35-145">4. lépés</span><span class="sxs-lookup"><span data-stu-id="2bb35-145">Step 4</span></span>

<span data-ttu-id="2bb35-146">Hozzon létre egy új erőforráscsoportot (hagyja ki ezt a lépést, ha egy meglévő erőforráscsoportot használ).</span><span class="sxs-lookup"><span data-stu-id="2bb35-146">Create a new resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
New-AzureRmResourceGroup -Name appgw-rg -location "West US"
```

<span data-ttu-id="2bb35-147">Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet.</span><span class="sxs-lookup"><span data-stu-id="2bb35-147">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="2bb35-148">Ez az erőforráscsoport erőforrások hello alapértelmezett helye szolgál.</span><span class="sxs-lookup"><span data-stu-id="2bb35-148">This is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="2bb35-149">Győződjön meg arról, hogy olyan átjárót használó összes parancsok toocreate hello azonos erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="2bb35-149">Make sure that all commands toocreate an application gateway uses hello same resource group.</span></span>

<span data-ttu-id="2bb35-150">A fenti példa hello létrehozott egy "Appgw-rg" és a hely "USA nyugati régiója" nevű erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="2bb35-150">In hello preceding example, we created a resource group called "appgw-rg" and location "West US".</span></span>

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a><span data-ttu-id="2bb35-151">Hozzon létre egy virtuális hálózatot és hello Alkalmazásátjáró alhálózatot</span><span class="sxs-lookup"><span data-stu-id="2bb35-151">Create a virtual network and a subnet for hello application gateway</span></span>

<span data-ttu-id="2bb35-152">a következő példa azt mutatja meg hogyan hello toocreate erőforrás-kezelő használatával egy virtuális hálózathoz:</span><span class="sxs-lookup"><span data-stu-id="2bb35-152">hello following example shows how toocreate a virtual network by using Resource Manager:</span></span>

### <a name="step-1"></a><span data-ttu-id="2bb35-153">1. lépés</span><span class="sxs-lookup"><span data-stu-id="2bb35-153">Step 1</span></span>

```powershell
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

<span data-ttu-id="2bb35-154">Ebben a lépésben a virtuális hálózati hello cím tartomány 10.0.0.0/24 tooa alhálózati használt változó toobe toocreate rendeli hozzá.</span><span class="sxs-lookup"><span data-stu-id="2bb35-154">This step assigns hello address range 10.0.0.0/24 tooa subnet variable toobe used toocreate a virtual network.</span></span>

### <a name="step-2"></a><span data-ttu-id="2bb35-155">2. lépés</span><span class="sxs-lookup"><span data-stu-id="2bb35-155">Step 2</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnetconfig
```

<span data-ttu-id="2bb35-156">Ebben a lépésben a virtuális hálózati "appgwvnet" nevű erőforrás csoport "appgw-rg" hello USA nyugati régiójában hello előtag 10.0.0.0/16 használata a 10.0.0.0/24 alhálózat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2bb35-156">This step creates a virtual network named "appgwvnet" in resource group "appgw-rg" for hello West US region using hello prefix 10.0.0.0/16 with subnet 10.0.0.0/24.</span></span>

### <a name="step-3"></a><span data-ttu-id="2bb35-157">3. lépés</span><span class="sxs-lookup"><span data-stu-id="2bb35-157">Step 3</span></span>

```powershell
$subnet = $vnet.subnets[0]
```

<span data-ttu-id="2bb35-158">Ez a lépés hozzárendel hello alhálózati objektum toovariable $subnet hello további lépéseket.</span><span class="sxs-lookup"><span data-stu-id="2bb35-158">This step assigns hello subnet object toovariable $subnet for hello next steps.</span></span>

## <a name="create-an-application-gateway-configuration-object"></a><span data-ttu-id="2bb35-159">Hozzon létre egy Application Gateway konfigurációs objektumot</span><span class="sxs-lookup"><span data-stu-id="2bb35-159">Create an application gateway configuration object</span></span>

### <a name="step-1"></a><span data-ttu-id="2bb35-160">1. lépés</span><span class="sxs-lookup"><span data-stu-id="2bb35-160">Step 1</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

<span data-ttu-id="2bb35-161">Ebben a lépésben létrehozza a "gatewayIP01" nevű alkalmazás átjáró IP-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="2bb35-161">This step creates an application gateway IP configuration named "gatewayIP01".</span></span> <span data-ttu-id="2bb35-162">Alkalmazásátjáró indításakor szerzi be hello alhálózati konfigurált IP-címeit, és útvonal-hello háttér IP-készletet a hálózati forgalom toohello IP-címek.</span><span class="sxs-lookup"><span data-stu-id="2bb35-162">When Application Gateway starts, it picks up an IP address from hello subnet configured and route network traffic toohello IP addresses in hello back-end IP pool.</span></span> <span data-ttu-id="2bb35-163">Ne feledje, hogy minden példány egy IP-címet vesz fel.</span><span class="sxs-lookup"><span data-stu-id="2bb35-163">Keep in mind that each instance takes one IP address.</span></span>

### <a name="step-2"></a><span data-ttu-id="2bb35-164">2. lépés</span><span class="sxs-lookup"><span data-stu-id="2bb35-164">Step 2</span></span>

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.1.1.8,10.1.1.9,10.1.1.10
```

<span data-ttu-id="2bb35-165">Ez a lépés konfigurál hello háttér IP-címkészlet neve "pool01" IP-címek "10.1.1.8, 10.1.1.9, 10.1.1.10".</span><span class="sxs-lookup"><span data-stu-id="2bb35-165">This step configures hello back-end IP address pool named "pool01" with IP addresses "10.1.1.8, 10.1.1.9, 10.1.1.10".</span></span> <span data-ttu-id="2bb35-166">Most már az hello IP-címek, amelyek megkapják a hello a hálózati forgalom, elérhető lesz a hello előtér-IP-végponton.</span><span class="sxs-lookup"><span data-stu-id="2bb35-166">Those are hello IP addresses that receive hello network traffic that comes from hello front-end IP endpoint.</span></span> <span data-ttu-id="2bb35-167">IP-címek tooadd megelőző hello lecseréli a saját alkalmazás IP-cím végpontokat.</span><span class="sxs-lookup"><span data-stu-id="2bb35-167">You replace hello preceding IP addresses tooadd your own application IP address endpoints.</span></span>

### <a name="step-3"></a><span data-ttu-id="2bb35-168">3. lépés</span><span class="sxs-lookup"><span data-stu-id="2bb35-168">Step 3</span></span>

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

<span data-ttu-id="2bb35-169">Ez a lépés konfigurál alkalmazás átjáró beállítás "poolsetting01" hello terhelésre eloszlik hálózati forgalom hello háttér-készletben.</span><span class="sxs-lookup"><span data-stu-id="2bb35-169">This step configures application gateway setting "poolsetting01" for hello load balanced network traffic in hello back-end pool.</span></span>

### <a name="step-4"></a><span data-ttu-id="2bb35-170">4. lépés</span><span class="sxs-lookup"><span data-stu-id="2bb35-170">Step 4</span></span>

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80
```

<span data-ttu-id="2bb35-171">Ez a lépés konfigurál hello "frontendport01" nevű hello ILB az előtér-IP-port.</span><span class="sxs-lookup"><span data-stu-id="2bb35-171">This step configures hello front-end IP port named "frontendport01" for hello ILB.</span></span>

### <a name="step-5"></a><span data-ttu-id="2bb35-172">5. lépés</span><span class="sxs-lookup"><span data-stu-id="2bb35-172">Step 5</span></span>

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -Subnet $subnet
```

<span data-ttu-id="2bb35-173">Ebben a lépésben hello "fipconfig01" nevű előtér-IP-konfiguráció hoz létre, és társítja azt egy privát IP-cím hello jelenlegi virtuális hálózati alhálózat.</span><span class="sxs-lookup"><span data-stu-id="2bb35-173">This step creates hello front-end IP configuration called "fipconfig01" and associates it with a private IP from hello current virtual network subnet.</span></span>

### <a name="step-6"></a><span data-ttu-id="2bb35-174">6. lépés</span><span class="sxs-lookup"><span data-stu-id="2bb35-174">Step 6</span></span>

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp
```

<span data-ttu-id="2bb35-175">Ebben a lépésben létrehozza a "listener01" nevű hello figyelő, és hozzárendeli a hello előtér-port toohello előtér-IP-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="2bb35-175">This step creates hello listener called "listener01" and associates hello front-end port toohello front-end IP configuration.</span></span>

### <a name="step-7"></a><span data-ttu-id="2bb35-176">7. lépés</span><span class="sxs-lookup"><span data-stu-id="2bb35-176">Step 7</span></span>

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

<span data-ttu-id="2bb35-177">Ebben a lépésben létrehoz hello terheléselosztási útválasztási szabály neve "rule01" által konfigurált házirendsablonok hello load balancer viselkedését.</span><span class="sxs-lookup"><span data-stu-id="2bb35-177">This step creates hello load balancer routing rule called "rule01" that configures hello load balancer behavior.</span></span>

### <a name="step-8"></a><span data-ttu-id="2bb35-178">8. lépés</span><span class="sxs-lookup"><span data-stu-id="2bb35-178">Step 8</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

<span data-ttu-id="2bb35-179">Ez a lépés konfigurál hello példányméretének hello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="2bb35-179">This step configures hello instance size of hello application gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="2bb35-180">az alapértelmezett érték hello *InstanceCount* 2, maximális értéke 10.</span><span class="sxs-lookup"><span data-stu-id="2bb35-180">hello default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="2bb35-181">az alapértelmezett érték hello *GatewaySize* közepes.</span><span class="sxs-lookup"><span data-stu-id="2bb35-181">hello default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="2bb35-182">A Standard_Small, Standard_Medium és a Standard_Large lehetőségek közül választhat.</span><span class="sxs-lookup"><span data-stu-id="2bb35-182">You can choose between Standard_Small, Standard_Medium, and Standard_Large.</span></span>

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a><span data-ttu-id="2bb35-183">Application Gateway létrehozása a New-AzureApplicationGateway használatával</span><span class="sxs-lookup"><span data-stu-id="2bb35-183">Create an application gateway by using New-AzureApplicationGateway</span></span>

<span data-ttu-id="2bb35-184">Az előző lépésekben hello összes konfigurációs elemet hoz létre a meglévő Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="2bb35-184">Creates an application gateway with all configuration items from hello preceding steps.</span></span> <span data-ttu-id="2bb35-185">Ebben a példában a hello Alkalmazásátjáró "appgwtest" nevezik.</span><span class="sxs-lookup"><span data-stu-id="2bb35-185">In this example, hello application gateway is called "appgwtest".</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

<span data-ttu-id="2bb35-186">Ez a lépés Alkalmazásátjáró az előző lépésekben hello összes konfigurációs elemet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2bb35-186">This step creates an application gateway with all configuration items from hello preceding steps.</span></span> <span data-ttu-id="2bb35-187">Hello a példában a hello Alkalmazásátjáró "appgwtest" nevezik.</span><span class="sxs-lookup"><span data-stu-id="2bb35-187">In hello example, hello application gateway is called "appgwtest".</span></span>

## <a name="delete-an-application-gateway"></a><span data-ttu-id="2bb35-188">Application Gateway törlése</span><span class="sxs-lookup"><span data-stu-id="2bb35-188">Delete an application gateway</span></span>

<span data-ttu-id="2bb35-189">Alkalmazásátjáró toodelete, toodo hello lépések sorrendben kell:</span><span class="sxs-lookup"><span data-stu-id="2bb35-189">toodelete an application gateway, you need toodo hello following steps in order:</span></span>

1. <span data-ttu-id="2bb35-190">Használjon hello `Stop-AzureRmApplicationGateway` parancsmag toostop hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="2bb35-190">Use hello `Stop-AzureRmApplicationGateway` cmdlet toostop hello gateway.</span></span>
2. <span data-ttu-id="2bb35-191">Használjon hello `Remove-AzureRmApplicationGateway` parancsmag tooremove hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="2bb35-191">Use hello `Remove-AzureRmApplicationGateway` cmdlet tooremove hello gateway.</span></span>
3. <span data-ttu-id="2bb35-192">Győződjön meg arról, hogy hello átjáró el lett távolítva a hello `Get-AzureApplicationGateway` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="2bb35-192">Verify that hello gateway has been removed by using hello `Get-AzureApplicationGateway` cmdlet.</span></span>

### <a name="step-1"></a><span data-ttu-id="2bb35-193">1. lépés</span><span class="sxs-lookup"><span data-stu-id="2bb35-193">Step 1</span></span>

<span data-ttu-id="2bb35-194">Hello application gateway-objektum, és társítsa azt tooa változó "$getgw".</span><span class="sxs-lookup"><span data-stu-id="2bb35-194">Get hello application gateway object and associate it tooa variable "$getgw".</span></span>

```powershell
$getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

### <a name="step-2"></a><span data-ttu-id="2bb35-195">2. lépés</span><span class="sxs-lookup"><span data-stu-id="2bb35-195">Step 2</span></span>

<span data-ttu-id="2bb35-196">Használjon `Stop-AzureRmApplicationGateway` toostop hello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="2bb35-196">Use `Stop-AzureRmApplicationGateway` toostop hello application gateway.</span></span> <span data-ttu-id="2bb35-197">Ez a példa bemutatja hello `Stop-AzureRmApplicationGateway` hello első sor parancsmag hello kimeneti követ.</span><span class="sxs-lookup"><span data-stu-id="2bb35-197">This sample shows hello `Stop-AzureRmApplicationGateway` cmdlet on hello first line, followed by hello output.</span></span>

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

<span data-ttu-id="2bb35-198">Ha hello Alkalmazásátjáró leállított állapotban van, a hello `Remove-AzureRmApplicationGateway` parancsmag tooremove hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="2bb35-198">Once hello application gateway is in a stopped state, use hello `Remove-AzureRmApplicationGateway` cmdlet tooremove hello service.</span></span>

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
> <span data-ttu-id="2bb35-199">Hello **-force** kapcsoló lehet használt toosuppress hello eltávolítása jóváhagyást kérő üzenet.</span><span class="sxs-lookup"><span data-stu-id="2bb35-199">hello **-force** switch can be used toosuppress hello remove confirmation message.</span></span>

<span data-ttu-id="2bb35-200">tooverify, amely hello szolgáltatás el lett távolítva, használhatja a hello `Get-AzureRmApplicationGateway` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="2bb35-200">tooverify that hello service has been removed, you can use hello `Get-AzureRmApplicationGateway` cmdlet.</span></span> <span data-ttu-id="2bb35-201">Ez a lépés nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="2bb35-201">This step is not required.</span></span>

```powershell
Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

```
VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

Get-AzureApplicationGateway : ResourceNotFound: hello gateway does not exist.
```

## <a name="next-steps"></a><span data-ttu-id="2bb35-202">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2bb35-202">Next steps</span></span>

<span data-ttu-id="2bb35-203">Ha azt szeretné, hogy az SSL-kiszervezés tooconfigure, [konfigurálása az SSL-kiszervezés Alkalmazásátjáró](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="2bb35-203">If you want tooconfigure SSL offload, see [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="2bb35-204">Ha azt szeretné, hogy egy alkalmazás átjáró toouse egy ILB rendelkező tooconfigure, [hozzon létre egy alkalmazást egy belső terheléselosztón (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="2bb35-204">If you want tooconfigure an application gateway toouse with an ILB, see [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="2bb35-205">Ha további általános információra van szüksége a terheléselosztás beállításaival kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="2bb35-205">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="2bb35-206">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="2bb35-206">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="2bb35-207">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="2bb35-207">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

