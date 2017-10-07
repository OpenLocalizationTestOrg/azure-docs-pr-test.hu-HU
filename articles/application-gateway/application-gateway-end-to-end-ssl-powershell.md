---
title: "aaaConfigure end tooend SSL-Titkosítást az Azure Application Gateway |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooconfigure end tooend SSL-Titkosítást az Alkalmazásátjáró Azure Resource Manager PowerShell használatával"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: e6d80a33-4047-4538-8c83-e88876c8834e
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: gwallace
ms.openlocfilehash: 7c280478e143d309e7665219441cbee8c81d9a80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-end-tooend-ssl-with-application-gateway-using-powershell"></a><span data-ttu-id="0d95a-103">Záró tooend SSL konfigurálása az Alkalmazásátjáró PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="0d95a-103">Configure end tooend SSL with Application Gateway using PowerShell</span></span>

## <a name="overview"></a><span data-ttu-id="0d95a-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="0d95a-104">Overview</span></span>

<span data-ttu-id="0d95a-105">Alkalmazás átjáró támogatja a forgalom titkosítása tooend végén.</span><span class="sxs-lookup"><span data-stu-id="0d95a-105">Application Gateway supports end tooend encryption of traffic.</span></span> <span data-ttu-id="0d95a-106">Alkalmazásátjáró ennek érdekében hello SSL-kapcsolat hello Alkalmazásátjáró leáll.</span><span class="sxs-lookup"><span data-stu-id="0d95a-106">Application Gateway does this by terminating hello SSL connection at hello application gateway.</span></span> <span data-ttu-id="0d95a-107">hello átjáró majd hello útválasztási szabályok toohello forgalom újra titkosítja a hello csomagot, majd továbbítja hello csomagok toohello megfelelő háttér hello megadott útválasztási szabályok alapján alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="0d95a-107">hello gateway then applies hello routing rules toohello traffic, re-encrypts hello packet, and forwards hello packet toohello appropriate backend based on hello routing rules defined.</span></span> <span data-ttu-id="0d95a-108">Hello webkiszolgáló válaszának végighalad hello hátsó toohello végfelhasználói ugyanezt a folyamatot.</span><span class="sxs-lookup"><span data-stu-id="0d95a-108">Any response from hello web server goes through hello same process back toohello end user.</span></span>

<span data-ttu-id="0d95a-109">Egy másik szolgáltatás, hogy az Alkalmazásátjáró is támogatja egyéni SSL-beállítások.</span><span class="sxs-lookup"><span data-stu-id="0d95a-109">Another feature that application gateway supports defining custom SSL options.</span></span> <span data-ttu-id="0d95a-110">Alkalmazásátjáró támogatja a következő protokoll verziója; letiltása hello **TLSv1.0**, **TLSv1.1**, és **TLSv1.2** is meghatározása hello, amely cipher suites toouse és hello sorrend figyelembe vételével.</span><span class="sxs-lookup"><span data-stu-id="0d95a-110">Application Gateway supports disabling hello following protocol version; **TLSv1.0**, **TLSv1.1**, and **TLSv1.2** as well defining hello which cipher suites toouse and hello order of preference.</span></span>  <span data-ttu-id="0d95a-111">További információ az SSL-beállítások konfigurálhatók, toolearn látogasson el [SSL-házirend áttekintése](application-gateway-SSL-policy-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0d95a-111">toolearn more about configurable SSL options, visit [SSL Policy overview](application-gateway-SSL-policy-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="0d95a-112">Az SSL 2.0 és az SSL 3.0 alapértelmezés szerint le vannak tiltva, és nem lehet engedélyezni.</span><span class="sxs-lookup"><span data-stu-id="0d95a-112">SSL 2.0 and SSL 3.0 are disabled by default and cannot be enabled.</span></span> <span data-ttu-id="0d95a-113">Ezek nem biztonságos pedig nem képes toobe Alkalmazásátjáró együtt.</span><span class="sxs-lookup"><span data-stu-id="0d95a-113">They are considered unsecure and are not able toobe used with Application Gateway.</span></span>

![a forgatókönyv kép][scenario]

## <a name="scenario"></a><span data-ttu-id="0d95a-115">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="0d95a-115">Scenario</span></span>

<span data-ttu-id="0d95a-116">Ebben a forgatókönyvben a megismerheti, hogyan egy alkalmazást átjáró, amely toocreate end tooend SSL PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="0d95a-116">In this scenario, you learn how toocreate an application gateway using end tooend SSL using PowerShell.</span></span>

<span data-ttu-id="0d95a-117">Ez a forgatókönyv tartalma:</span><span class="sxs-lookup"><span data-stu-id="0d95a-117">This scenario will:</span></span>

* <span data-ttu-id="0d95a-118">Hozzon létre egy erőforráscsoportot nevű **appgw-rg-n**</span><span class="sxs-lookup"><span data-stu-id="0d95a-118">Create a resource group named **appgw-rg**</span></span>
* <span data-ttu-id="0d95a-119">Hozzon létre egy virtuális hálózatot nevű **appgwvnet** rendelkező 10.0.0.0/16 a címteret.</span><span class="sxs-lookup"><span data-stu-id="0d95a-119">Create a virtual network named **appgwvnet** with an address space of 10.0.0.0/16.</span></span>
* <span data-ttu-id="0d95a-120">Hozzon létre két alhálózat nevű **appgwsubnet** és **appsubnet**.</span><span class="sxs-lookup"><span data-stu-id="0d95a-120">Create two subnets called **appgwsubnet** and **appsubnet**.</span></span>
* <span data-ttu-id="0d95a-121">Hozzon létre egy kis alkalmazás átjáró támogató end tooend SSL-titkosítást, adott korlátok SSL protokollok verziójának és titkosító csomagok.</span><span class="sxs-lookup"><span data-stu-id="0d95a-121">Create a small application gateway supporting end tooend SSL encryption that limits SSL protocols versions and cipher suites.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="0d95a-122">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="0d95a-122">Before you begin</span></span>

<span data-ttu-id="0d95a-123">tooconfigure end tooend SSL az Alkalmazásátjáró, egy tanúsítványra szükség, hello átjáró számára, és hello háttérkiszolgálók tanúsítványok szükségesek.</span><span class="sxs-lookup"><span data-stu-id="0d95a-123">tooconfigure end tooend SSL with an application gateway, a certificate is required for hello gateway and certificates are required for hello backend servers.</span></span> <span data-ttu-id="0d95a-124">hello átjáró tanúsítvány használt tooencrypt és visszafejtése hello elküldött forgalom tooit SSL használatával.</span><span class="sxs-lookup"><span data-stu-id="0d95a-124">hello gateway certificate is used tooencrypt and decrypt hello traffic sent tooit using SSL.</span></span> <span data-ttu-id="0d95a-125">hello átjáró tanúsítványt kell toobe személyes információcsere (pfx) formátumú.</span><span class="sxs-lookup"><span data-stu-id="0d95a-125">hello gateway certificate needs toobe in Personal Information Exchange (pfx) format.</span></span> <span data-ttu-id="0d95a-126">Ez lehetővé teszi a hello titkos kulcs toobe exportált hello alkalmazás átjáró tooperform hello titkosítási és visszafejtési forgalom által előírt.</span><span class="sxs-lookup"><span data-stu-id="0d95a-126">This file format allows for hello private key toobe exported which is required by hello application gateway tooperform hello encryption and decryption of traffic.</span></span>

<span data-ttu-id="0d95a-127">Az end tooend SSL titkosítást hello háttér szerepel az engedélyezési listán alkalmazás átjáróval kell lennie.</span><span class="sxs-lookup"><span data-stu-id="0d95a-127">For end tooend SSL encryption hello backend must be whitelisted with application gateway.</span></span> <span data-ttu-id="0d95a-128">Nyilvános tanúsítvány hello hello háttérkiszolgálókon toohello Alkalmazásátjáró feltöltése ehhez.</span><span class="sxs-lookup"><span data-stu-id="0d95a-128">This is done by uploading hello public certificate of hello backends toohello application gateway.</span></span> <span data-ttu-id="0d95a-129">Ez biztosítja, hogy hello Alkalmazásátjáró csak ismert háttér példányok kommunikál.</span><span class="sxs-lookup"><span data-stu-id="0d95a-129">This ensures that hello application gateway only communicates with known backend instances.</span></span> <span data-ttu-id="0d95a-130">Ez biztosítja a hello közötti tooend kommunikáció további.</span><span class="sxs-lookup"><span data-stu-id="0d95a-130">This further secures hello end tooend communication.</span></span>

<span data-ttu-id="0d95a-131">Ez a folyamat ismertetett lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0d95a-131">This process is described in hello following steps:</span></span>

## <a name="create-hello-resource-group"></a><span data-ttu-id="0d95a-132">Hello erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="0d95a-132">Create hello Resource Group</span></span>

<span data-ttu-id="0d95a-133">Ez a szakasz bemutatja, hogyan hello Alkalmazásátjáró tartalmazó erőforráscsoport létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0d95a-133">This section walks you through creating a resource group, that contains hello application gateway.</span></span>

### <a name="step-1"></a><span data-ttu-id="0d95a-134">1. lépés</span><span class="sxs-lookup"><span data-stu-id="0d95a-134">Step 1</span></span>

<span data-ttu-id="0d95a-135">Jelentkezzen be Azure-fiók tooyour.</span><span class="sxs-lookup"><span data-stu-id="0d95a-135">Log in tooyour Azure Account.</span></span>

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a><span data-ttu-id="0d95a-136">2. lépés</span><span class="sxs-lookup"><span data-stu-id="0d95a-136">Step 2</span></span>

<span data-ttu-id="0d95a-137">Válassza ki a hello előfizetés toouse ehhez a forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="0d95a-137">Select hello subscription toouse for this scenario.</span></span>

```powershell
Select-AzureRmsubscription -SubscriptionName "<Subscription name>"
```

### <a name="step-3"></a><span data-ttu-id="0d95a-138">3. lépés</span><span class="sxs-lookup"><span data-stu-id="0d95a-138">Step 3</span></span>

<span data-ttu-id="0d95a-139">Hozzon létre egy erőforráscsoportot (hagyja ki ezt a lépést, ha egy meglévő erőforráscsoportot használ).</span><span class="sxs-lookup"><span data-stu-id="0d95a-139">Create a resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a><span data-ttu-id="0d95a-140">Hozzon létre egy virtuális hálózatot és hello Alkalmazásátjáró alhálózatot</span><span class="sxs-lookup"><span data-stu-id="0d95a-140">Create a virtual network and a subnet for hello application gateway</span></span>

<span data-ttu-id="0d95a-141">hello alábbi példa létrehoz egy virtuális hálózat és két alhálózat.</span><span class="sxs-lookup"><span data-stu-id="0d95a-141">hello following example creates a virtual network and two subnets.</span></span> <span data-ttu-id="0d95a-142">Egy alhálózat használt toohold hello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="0d95a-142">One subnet is used toohold hello application gateway.</span></span> <span data-ttu-id="0d95a-143">hello más alhálózati használható hello háttérkiszolgálókon üzemeltetési hello webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0d95a-143">hello other subnet is used for hello backends hosting hello web application.</span></span>

### <a name="step-1"></a><span data-ttu-id="0d95a-144">1. lépés</span><span class="sxs-lookup"><span data-stu-id="0d95a-144">Step 1</span></span>

<span data-ttu-id="0d95a-145">Rendelje hozzá a címtartomány hello alhálózati hello Alkalmazásátjáró magát a használható.</span><span class="sxs-lookup"><span data-stu-id="0d95a-145">Assign an address range for hello subnet be used for hello Application Gateway itself.</span></span>

```powershell
$gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24
```

> [!NOTE]
> <span data-ttu-id="0d95a-146">Az Alkalmazásátjáró konfigurált alhálózatok megfelelő méretű lehet.</span><span class="sxs-lookup"><span data-stu-id="0d95a-146">Subnets configured for application gateway should be properly sized.</span></span> <span data-ttu-id="0d95a-147">Alkalmazásátjáró konfigurálhatja a too10 példány mentése.</span><span class="sxs-lookup"><span data-stu-id="0d95a-147">An application gateway can be configured for up too10 instances.</span></span> <span data-ttu-id="0d95a-148">Minden példány hello alhálózat egy IP-címeit vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="0d95a-148">Each instance takes one IP address from hello subnet.</span></span> <span data-ttu-id="0d95a-149">Túl kicsi alhálózaton kedvezőtlen hatással lehet, olyan átjárót kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="0d95a-149">Too small of a subnet can adversely affect scaling out an application gateway.</span></span>
> 
> 

### <a name="step-2"></a><span data-ttu-id="0d95a-150">2. lépés</span><span class="sxs-lookup"><span data-stu-id="0d95a-150">Step 2</span></span>

<span data-ttu-id="0d95a-151">Rendeljen hozzá egy háttér címkészletet hello használt cím tartomány toobe.</span><span class="sxs-lookup"><span data-stu-id="0d95a-151">Assign an address range toobe used for hello Backend address pool.</span></span>

```powershell
$nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24
```

### <a name="step-3"></a><span data-ttu-id="0d95a-152">3. lépés</span><span class="sxs-lookup"><span data-stu-id="0d95a-152">Step 3</span></span>

<span data-ttu-id="0d95a-153">Virtuális hálózat létrehozása az előző lépésekben hello definiált hello alhálózatokon.</span><span class="sxs-lookup"><span data-stu-id="0d95a-153">Create a virtual network with hello subnets defined in hello preceding steps.</span></span>

```powershell
$vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
```

### <a name="step-4"></a><span data-ttu-id="0d95a-154">4. lépés</span><span class="sxs-lookup"><span data-stu-id="0d95a-154">Step 4</span></span>

<span data-ttu-id="0d95a-155">Lekéri az hello virtuális hálózati erőforrás és alhálózati erőforrások toobe használt hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="0d95a-155">Retrieve hello virtual network resource and subnet resources toobe used in hello following steps:</span></span>

```powershell
$vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
$gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
$nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet
```

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a><span data-ttu-id="0d95a-156">A nyilvános IP-cím hello előtér-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="0d95a-156">Create a public IP address for hello front-end configuration</span></span>

<span data-ttu-id="0d95a-157">Hozzon létre egy nyilvános IP erőforrás toobe hello használt.</span><span class="sxs-lookup"><span data-stu-id="0d95a-157">Create a public IP resource toobe used for hello application gateway.</span></span> <span data-ttu-id="0d95a-158">A nyilvános IP-címet használja a következő lépéssel.</span><span class="sxs-lookup"><span data-stu-id="0d95a-158">This public IP address is used a following step.</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "West US" -AllocationMethod Dynamic
```

> [!IMPORTANT]
> <span data-ttu-id="0d95a-159">Alkalmazásátjáró nem támogatja a nyilvános IP-cím, tartomány címkével rendelkező létrehozott hello használatát.</span><span class="sxs-lookup"><span data-stu-id="0d95a-159">Application Gateway does not support hello use of a public IP address created with a domain label defined.</span></span> <span data-ttu-id="0d95a-160">Csak nyilvános IP-címnek a dinamikusan létrehozott tartományi címkére esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="0d95a-160">Only a public IP address with a dynamically created domain label is supported.</span></span> <span data-ttu-id="0d95a-161">Ha egy rövid DNS-nevet az Alkalmazásátjáró hello van szüksége, ajánlott aliasként rögzítse toouse egy olyan CNAME REKORDOT.</span><span class="sxs-lookup"><span data-stu-id="0d95a-161">If you require a friendly dns name for hello application gateway, it is recommended toouse a CNAME record as an alias.</span></span>

## <a name="create-an-application-gateway-configuration-object"></a><span data-ttu-id="0d95a-162">Hozzon létre egy Application Gateway konfigurációs objektumot</span><span class="sxs-lookup"><span data-stu-id="0d95a-162">Create an application gateway configuration object</span></span>

<span data-ttu-id="0d95a-163">Az összes konfigurációs elemek hello Alkalmazásátjáró létrehozása előtt vannak állítva.</span><span class="sxs-lookup"><span data-stu-id="0d95a-163">All configuration items are set before creating hello application gateway.</span></span> <span data-ttu-id="0d95a-164">hello lépések hello konfigurációs elemek létrehozását, amelyek szükségesek az alkalmazás átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="0d95a-164">hello following steps create hello configuration items that are needed for an application gateway resource.</span></span>

### <a name="step-1"></a><span data-ttu-id="0d95a-165">1. lépés</span><span class="sxs-lookup"><span data-stu-id="0d95a-165">Step 1</span></span>

<span data-ttu-id="0d95a-166">Hozzon létre egy alkalmazás átjáró IP-konfiguráció, ezzel a beállítással milyen alhálózati hello alkalmazás átjárót használja.</span><span class="sxs-lookup"><span data-stu-id="0d95a-166">Create an application gateway IP configuration, this setting configures what subnet hello application gateway uses.</span></span> <span data-ttu-id="0d95a-167">Alkalmazásátjáró indításakor szerzi be hello alhálózati konfigurált IP-címeit, és továbbítja a hálózati forgalom toohello IP-címek hello háttér IP-címkészletben.</span><span class="sxs-lookup"><span data-stu-id="0d95a-167">When application gateway starts, it picks up an IP address from hello subnet configured and routes network traffic toohello IP addresses in hello back-end IP pool.</span></span> <span data-ttu-id="0d95a-168">Ne feledje, hogy minden példány egy IP-címet vesz fel.</span><span class="sxs-lookup"><span data-stu-id="0d95a-168">Keep in mind that each instance takes one IP address.</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet
```

### <a name="step-2"></a><span data-ttu-id="0d95a-169">2. lépés</span><span class="sxs-lookup"><span data-stu-id="0d95a-169">Step 2</span></span>

<span data-ttu-id="0d95a-170">Hozzon létre egy előtér-IP-konfiguráció, ezzel a beállítással egy privát vagy nyilvános IP-cím toohello előtér hello Alkalmazásátjáró képezi le.</span><span class="sxs-lookup"><span data-stu-id="0d95a-170">Create a front-end IP configuration, this setting maps a private or public ip address toohello front end of hello application gateway.</span></span> <span data-ttu-id="0d95a-171">a következő lépés hello társítja hello előtér-IP-konfigurációval lépést megelőző hello hello nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="0d95a-171">hello following step associates hello public IP address in hello preceding step with hello front-end IP configuration.</span></span>

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip
```

### <a name="step-3"></a><span data-ttu-id="0d95a-172">3. lépés</span><span class="sxs-lookup"><span data-stu-id="0d95a-172">Step 3</span></span>

<span data-ttu-id="0d95a-173">Konfigurálja a hello háttér IP-címkészletet a hello webes háttérkiszolgálók hello IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="0d95a-173">Configure hello back-end IP address pool with hello IP addresses of hello backend web servers.</span></span> <span data-ttu-id="0d95a-174">Az IP-címeket azokat hello IP-címeket, amelyek hello előtér-IP-végponton származik hello hálózati forgalom fogadására.</span><span class="sxs-lookup"><span data-stu-id="0d95a-174">These IP addresses are hello IP addresses that receive hello network traffic that comes from hello front-end IP endpoint.</span></span> <span data-ttu-id="0d95a-175">A következő IP-címek tooadd hello lecseréli a saját alkalmazás IP-cím végpontokat.</span><span class="sxs-lookup"><span data-stu-id="0d95a-175">You replace hello following IP addresses tooadd your own application IP address endpoints.</span></span>

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3
```

> [!NOTE]
> <span data-ttu-id="0d95a-176">Egy teljesen minősített tartománynevét (FQDN) érték is érvényes hello háttérkiszolgálókhoz IP-cím helyett hello - BackendFqdns kapcsoló használatával.</span><span class="sxs-lookup"><span data-stu-id="0d95a-176">A fully qualified domain name (FQDN) is also a valid value in place of an ip address for hello backend servers by using hello -BackendFqdns switch.</span></span> 

### <a name="step-4"></a><span data-ttu-id="0d95a-177">4. lépés</span><span class="sxs-lookup"><span data-stu-id="0d95a-177">Step 4</span></span>

<span data-ttu-id="0d95a-178">Konfigurálja az előtér-IP-port hello hello nyilvános IP-végponton.</span><span class="sxs-lookup"><span data-stu-id="0d95a-178">Configure hello front-end IP port for hello public IP endpoint.</span></span> <span data-ttu-id="0d95a-179">Ez a port nem hello port, amelyet a végfelhasználók csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="0d95a-179">This port is hello port that end users connect to.</span></span>

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443
```

### <a name="step-5"></a><span data-ttu-id="0d95a-180">5. lépés</span><span class="sxs-lookup"><span data-stu-id="0d95a-180">Step 5</span></span>

<span data-ttu-id="0d95a-181">Az Alkalmazásátjáró hello hello tanúsítvány konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="0d95a-181">Configure hello certificate for hello application gateway.</span></span> <span data-ttu-id="0d95a-182">Ez a tanúsítvány használt toodecrypt, és a hello Alkalmazásátjáró hello-forgalom újratitkosítása.</span><span class="sxs-lookup"><span data-stu-id="0d95a-182">This certificate is used toodecrypt and re-encrypt hello traffic on hello application gateway.</span></span>

```powershell
$cert = New-AzureRmApplicationGatewaySSLCertificate -Name cert01 -CertificateFile <full path too.pfx file> -Password <password for certificate file>
```

> [!NOTE]
> <span data-ttu-id="0d95a-183">Ez a minta hello tanúsítvány használt SSL-kapcsolat konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="0d95a-183">This sample configures hello certificate used for SSL connection.</span></span> <span data-ttu-id="0d95a-184">hello tanúsítványt kell toobe .pfx formátumú, és hello jelszó 4 too12 karakter közötti lehet.</span><span class="sxs-lookup"><span data-stu-id="0d95a-184">hello certificate needs toobe in .pfx format, and hello password must be between 4 too12 characters.</span></span>

### <a name="step-6"></a><span data-ttu-id="0d95a-185">6. lépés</span><span class="sxs-lookup"><span data-stu-id="0d95a-185">Step 6</span></span>

<span data-ttu-id="0d95a-186">Az Alkalmazásátjáró hello hello HTTP-figyelő létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0d95a-186">Create hello HTTP listener for hello application gateway.</span></span> <span data-ttu-id="0d95a-187">Rendelje hozzá a hello előtér-IP-konfiguráció, a port és az SSL-tanúsítvány toouse.</span><span class="sxs-lookup"><span data-stu-id="0d95a-187">Assign hello front-end ip configuration, port, and SSL certificate toouse.</span></span>

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SSLCertificate $cert
```

### <a name="step-7"></a><span data-ttu-id="0d95a-188">7. lépés</span><span class="sxs-lookup"><span data-stu-id="0d95a-188">Step 7</span></span>

<span data-ttu-id="0d95a-189">Hello SSL használt toobe engedélyezve van a háttér címkészletet erőforrások hello-tanúsítvány feltöltése.</span><span class="sxs-lookup"><span data-stu-id="0d95a-189">Upload hello certificate toobe used on hello SSL enabled backend pool resources.</span></span>

> [!NOTE]
> <span data-ttu-id="0d95a-190">hello alapértelmezett mintavétel hello nyilvános kulcs lekérése hello **alapértelmezett** hello háttér-tartozó IP-cím és a szolgáltatás összehasonlítja hello nyilvános kulcs értéke toohello nyilvános kulcs értéke itt kap SSL-kötést.</span><span class="sxs-lookup"><span data-stu-id="0d95a-190">hello default probe gets hello public key from hello **default** SSL binding on hello back-end's IP address and compares hello public key value it receives toohello public key value you provide here.</span></span> <span data-ttu-id="0d95a-191">hello lekérdezése nyilvános kulcs nem feltétlenül hello szánt hely toowhich adatforgalmakat **Ha** állomásfejléc és használ az SNI hello háttér-a.</span><span class="sxs-lookup"><span data-stu-id="0d95a-191">hello retrieved public key may not necessarily be hello intended site toowhich traffic flows **if** you are using host headers and SNI on hello back-end.</span></span> <span data-ttu-id="0d95a-192">Ha kétségei vannak, a keresse fel a https://127.0.0.1/ hello vissza-végpontok tooconfirm melyik tanúsítványt használják hello **alapértelmezett** SSL-kötést.</span><span class="sxs-lookup"><span data-stu-id="0d95a-192">If in doubt, visit https://127.0.0.1/ on hello back-ends tooconfirm which certificate is used for hello **default** SSL binding.</span></span> <span data-ttu-id="0d95a-193">Hello nyilvános kulcs használata ebben a szakaszban a kérelemből.</span><span class="sxs-lookup"><span data-stu-id="0d95a-193">Use hello public key from that request in this section.</span></span> <span data-ttu-id="0d95a-194">Ha a HTTPS-kötések használunk állomásfejléc és SNI, és nem kapott választ, és a tanúsítvány egy manuális böngésző kérelem toohttps://127.0.0.1/ hello vissza-végén, be kell állítania egy alapértelmezett SSL-kötés hello vissza-végén.</span><span class="sxs-lookup"><span data-stu-id="0d95a-194">If you are using host-headers and SNI on HTTPS bindings and you do not receive a response and certificate from a manual browser request toohttps://127.0.0.1/ on hello back-ends, you must set up a default SSL binding on hello back-ends.</span></span> <span data-ttu-id="0d95a-195">Ha ezt nem teszi meg, mintavételt sikertelen és hello háttér-nem szerepel az engedélyezési listán.</span><span class="sxs-lookup"><span data-stu-id="0d95a-195">If you do not do so, probes fail and hello back-end is not whitelisted.</span></span>

```powershell
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile C:\users\gwallace\Desktop\cert.cer
```

> [!NOTE]
> <span data-ttu-id="0d95a-196">Ebben a lépésben megadott hello tanúsítvány hello hello háttér megtalálható hello pfx-tanúsítvány nyilvános kulcsának kell lennie.</span><span class="sxs-lookup"><span data-stu-id="0d95a-196">hello certificate provided in this step should be hello public key of hello pfx cert present on hello backend.</span></span> <span data-ttu-id="0d95a-197">Hello háttérkiszolgálón a telepített hello tanúsítvány (nem hello főtanúsítvány) exportálja. CER formátumú, és használhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="0d95a-197">Export hello certificate (not hello root certificate) installed on hello backend server in .CER format and use it in this step.</span></span> <span data-ttu-id="0d95a-198">Ez a lépés whitelists hello hello Alkalmazásátjáró háttérrendszeréhez.</span><span class="sxs-lookup"><span data-stu-id="0d95a-198">This step whitelists hello backend with hello application gateway.</span></span>

### <a name="step-8"></a><span data-ttu-id="0d95a-199">8. lépés</span><span class="sxs-lookup"><span data-stu-id="0d95a-199">Step 8</span></span>

<span data-ttu-id="0d95a-200">Hello Alkalmazásátjáró háttér http beállításainak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="0d95a-200">Configure hello application gateway back-end http settings.</span></span> <span data-ttu-id="0d95a-201">Az előző lépés toohello http-beállítások hello feltöltött hello tanúsítvány hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="0d95a-201">Assign hello certificate uploaded in hello preceding step toohello http settings.</span></span>

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert
```

### <a name="step-9"></a><span data-ttu-id="0d95a-202">9. lépés</span><span class="sxs-lookup"><span data-stu-id="0d95a-202">Step 9</span></span>

<span data-ttu-id="0d95a-203">Hozzon létre egy terheléselosztási útválasztási szabály által konfigurált házirendsablonok hello load balancer viselkedését.</span><span class="sxs-lookup"><span data-stu-id="0d95a-203">Create a load balancer routing rule that configures hello load balancer behavior.</span></span> <span data-ttu-id="0d95a-204">Ebben a példában egy alapszintű ciklikus multiplexelés szabály jön létre.</span><span class="sxs-lookup"><span data-stu-id="0d95a-204">In this example, a basic round robin rule is created.</span></span>

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

### <a name="step-10"></a><span data-ttu-id="0d95a-205">10. lépés</span><span class="sxs-lookup"><span data-stu-id="0d95a-205">Step 10</span></span>

<span data-ttu-id="0d95a-206">Hello Alkalmazásátjáró hello példány méretének beállítása.</span><span class="sxs-lookup"><span data-stu-id="0d95a-206">Configure hello instance size of hello application gateway.</span></span>  <span data-ttu-id="0d95a-207">hello elérhető értékek **szabványos\_kis**, **szabványos\_Közepes**, és **szabványos\_nagy**.</span><span class="sxs-lookup"><span data-stu-id="0d95a-207">hello available sizes are **Standard\_Small**, **Standard\_Medium**, and **Standard\_Large**.</span></span>  <span data-ttu-id="0d95a-208">A kapacitás hello elérhető értékei 1 és 10 közötti.</span><span class="sxs-lookup"><span data-stu-id="0d95a-208">For capacity, hello available values are 1 through 10.</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

> [!NOTE]
> <span data-ttu-id="0d95a-209">Tesztelési célokra példányszámának 1 választható ki.</span><span class="sxs-lookup"><span data-stu-id="0d95a-209">An instance count of 1 can be chosen for testing purposes.</span></span> <span data-ttu-id="0d95a-210">Fontos, hogy bármely példányok száma a két példányt tooknow hello SLA nem vonatkozik, és ezért nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="0d95a-210">It is important tooknow that any instance count under two instances is not covered by hello SLA and are therefore not recommended.</span></span> <span data-ttu-id="0d95a-211">Kis átjárók jellemzően toobe fejlesztés tesztelés és nem üzemi célokra szolgál.</span><span class="sxs-lookup"><span data-stu-id="0d95a-211">Small gateways are toobe used for dev test and not for production purposes.</span></span>

### <a name="step-11"></a><span data-ttu-id="0d95a-212">11. lépés</span><span class="sxs-lookup"><span data-stu-id="0d95a-212">Step 11</span></span>

<span data-ttu-id="0d95a-213">Hello SSL házirend toobe használt hello Application Gateway konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="0d95a-213">Configure hello SSL policy toobe used on hello Application Gateway.</span></span> <span data-ttu-id="0d95a-214">Alkalmazásátjáró hello képességét tooset legalább támogatja az SSL protokoll verziója.</span><span class="sxs-lookup"><span data-stu-id="0d95a-214">Application Gateway supports hello ability tooset a minimum version for SSL protocol versions.</span></span>

<span data-ttu-id="0d95a-215">hello következő értékei, amelyek az protokoll-verziók listáját.</span><span class="sxs-lookup"><span data-stu-id="0d95a-215">hello following values are a list of protocol versions that can be defined.</span></span>

* <span data-ttu-id="0d95a-216">**TLSv1_0**</span><span class="sxs-lookup"><span data-stu-id="0d95a-216">**TLSv1_0**</span></span>
* <span data-ttu-id="0d95a-217">**TLSv1_1**</span><span class="sxs-lookup"><span data-stu-id="0d95a-217">**TLSv1_1**</span></span>
* <span data-ttu-id="0d95a-218">**TLSv1_2**</span><span class="sxs-lookup"><span data-stu-id="0d95a-218">**TLSv1_2**</span></span>

<span data-ttu-id="0d95a-219">Készlet túl hello minimális protokollverzió**TLSv1_2** és lehetővé teszi, hogy **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_ SHA-256**, **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**, és **TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** csak.</span><span class="sxs-lookup"><span data-stu-id="0d95a-219">Sets hello minimum protocol version too**TLSv1_2** and enables **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_SHA256**, **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**, and **TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** only.</span></span>

```powershell
$SSLPolicy = New-AzureRmApplicationGatewaySSLPolicy -MinProtocolVersion TLSv1_2 -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256"
```

## <a name="create-hello-application-gateway"></a><span data-ttu-id="0d95a-220">Hello Alkalmazásátjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="0d95a-220">Create hello Application Gateway</span></span>

<span data-ttu-id="0d95a-221">Alkalmazásátjáró hello előző lépésekben összes hello segítségével hozzon létre.</span><span class="sxs-lookup"><span data-stu-id="0d95a-221">Using all hello preceding steps, create hello Application Gateway.</span></span> <span data-ttu-id="0d95a-222">hello átjáró létrehozása hello rendkívül hosszú ideig futó folyamat.</span><span class="sxs-lookup"><span data-stu-id="0d95a-222">hello creation of hello gateway is a long running process.</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgateway -SSLCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SSLPolicy $SSLPolicy -AuthenticationCertificates $authcert -Verbose
```

## <a name="limit-ssl-protocol-versions-on-an-existing-application-gateway"></a><span data-ttu-id="0d95a-223">Az SSL protokoll verziói a meglévő Alkalmazásátjáró korlátozása</span><span class="sxs-lookup"><span data-stu-id="0d95a-223">Limit SSL protocol versions on an existing Application Gateway</span></span>

<span data-ttu-id="0d95a-224">hello előző lépések befejezési tooend SSL alkalmazás létrehozása és bizonyos SSL protokollverziója letiltása.</span><span class="sxs-lookup"><span data-stu-id="0d95a-224">hello preceding steps take you through creating an application with end tooend SSL and disabling certain SSL protocol versions.</span></span> <span data-ttu-id="0d95a-225">hello következő példa letiltja az egyes meglévő Alkalmazásátjáró SSL-házirendeket.</span><span class="sxs-lookup"><span data-stu-id="0d95a-225">hello following example disables certain SSL policies on an existing application gateway.</span></span>

### <a name="step-1"></a><span data-ttu-id="0d95a-226">1. lépés</span><span class="sxs-lookup"><span data-stu-id="0d95a-226">Step 1</span></span>

<span data-ttu-id="0d95a-227">Hello alkalmazás átjáró tooupdate beolvasása.</span><span class="sxs-lookup"><span data-stu-id="0d95a-227">Retrieve hello application gateway tooupdate.</span></span>

```powershell
$gw = Get-AzureRmApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG
```

### <a name="step-2"></a><span data-ttu-id="0d95a-228">2. lépés</span><span class="sxs-lookup"><span data-stu-id="0d95a-228">Step 2</span></span>

<span data-ttu-id="0d95a-229">Egy SSL-házirend meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="0d95a-229">Define an SSL policy.</span></span> <span data-ttu-id="0d95a-230">A következő példa hello, TLSv1.0 és TLSv1.1 le vannak tiltva, és titkosító csomagok hello **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_SHA256** , **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**, és  **A TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** hello csak ők engedélyezve vannak.</span><span class="sxs-lookup"><span data-stu-id="0d95a-230">In hello following example, TLSv1.0 and TLSv1.1 are disabled and hello cipher suites **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_SHA256**, **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**, and **TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** are hello only ones allowed.</span></span>

```powershell
Set-AzureRmApplicationGatewaySSLPolicy -MinProtocolVersion -PolicyType Custom -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256" -ApplicationGateway $gw

```

### <a name="step-3"></a><span data-ttu-id="0d95a-231">3. lépés</span><span class="sxs-lookup"><span data-stu-id="0d95a-231">Step 3</span></span>

<span data-ttu-id="0d95a-232">Végül frissítse hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="0d95a-232">Finally, update hello gateway.</span></span> <span data-ttu-id="0d95a-233">Fontos, hogy ezen utolsó lépésében-e a hosszú ideig futó feladatok toonote.</span><span class="sxs-lookup"><span data-stu-id="0d95a-233">It is important toonote that this last step is a long running task.</span></span> <span data-ttu-id="0d95a-234">Ha elkészült, záró tooend SSL hello Alkalmazásátjáró van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="0d95a-234">When it is done, end tooend SSL is configured on hello application gateway.</span></span>

```powershell
$gw | Set-AzureRmApplicationGateway
```

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="0d95a-235">Az Application Gateway DNS-nevének beszerzése</span><span class="sxs-lookup"><span data-stu-id="0d95a-235">Get application gateway DNS name</span></span>

<span data-ttu-id="0d95a-236">Hello átjáró létrehozása után hello következő lépésre tooconfigure hello előtér-kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="0d95a-236">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="0d95a-237">Nyilvános IP-cím esetén az Application Gateway használatához dinamikusan hozzárendelt DNS-névre van szükség, amely nem valódi név.</span><span class="sxs-lookup"><span data-stu-id="0d95a-237">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="0d95a-238">tooensure a végfelhasználók is találati hello Alkalmazásátjáró, egy olyan CNAME rekordot is használt toopoint toohello nyilvános végpontot hello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="0d95a-238">tooensure end users can hit hello application gateway, a CNAME record can be used toopoint toohello public endpoint of hello application gateway.</span></span> <span data-ttu-id="0d95a-239">[Egyéni tartománynév konfigurálása az Azure-ban](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="0d95a-239">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="0d95a-240">toodo a, hello Alkalmazásátjáró és a társított IP-/ DNS-nevét, hello PublicIPAddress elem csatolt toohello Alkalmazásátjáró beolvasása részleteit.</span><span class="sxs-lookup"><span data-stu-id="0d95a-240">toodo this, retrieve details of hello application gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello application gateway.</span></span> <span data-ttu-id="0d95a-241">hello alkalmazás átjáró DNS-névnek kell lennie a használt toocreate egy olyan CNAME rekordot pontok hello két webes alkalmazások toothis DNS-név.</span><span class="sxs-lookup"><span data-stu-id="0d95a-241">hello application gateway's DNS name should be used toocreate a CNAME record, which points hello two web applications toothis DNS name.</span></span> <span data-ttu-id="0d95a-242">A-rekordok hello használata nem javasolt, mert hello VIP módosíthatja az Alkalmazásátjáró újra kell indítani.</span><span class="sxs-lookup"><span data-stu-id="0d95a-242">hello use of A-records is not recommended since hello VIP may change on restart of application gateway.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="0d95a-243">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0d95a-243">Next steps</span></span>

<span data-ttu-id="0d95a-244">További tudnivalók a webalkalmazások a webalkalmazási tűzfal alkalmazás átjárón keresztül hello biztonsági korlátozások ellátogatva [webalkalmazás tűzfal áttekintése](application-gateway-webapplicationfirewall-overview.md)</span><span class="sxs-lookup"><span data-stu-id="0d95a-244">Learn about hardening hello security of your web applications with Web Application Firewall through Application Gateway by visiting [Web Application Firewall Overview](application-gateway-webapplicationfirewall-overview.md)</span></span>

[scenario]: ./media/application-gateway-end-to-end-SSL-powershell/scenario.png
