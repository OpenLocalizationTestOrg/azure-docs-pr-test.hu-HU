---
title: "aaaConfigure SSL - Azure Application Gateway - PowerShell kiszervezése |} Microsoft Docs"
description: "Ezen a lapon nyújt útmutatást toocreate Alkalmazásátjáró SSL-kiszervezés Azure Resource Manager használatával"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 3c3681e0-f928-4682-9d97-567f8e278e13
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: gwallace
ms.openlocfilehash: c2855d8d3caaa97ec05475c67ff0f8dce72ef2a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-resource-manager"></a><span data-ttu-id="71766-103">Application Gateway konfigurálása SSL-alapú kiszervezéshez az Azure Resource Manager használatával</span><span class="sxs-lookup"><span data-stu-id="71766-103">Configure an application gateway for SSL offload by using Azure Resource Manager</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="71766-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="71766-104">Azure portal</span></span>](application-gateway-ssl-portal.md)
> * [<span data-ttu-id="71766-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="71766-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ssl-arm.md)
> * [<span data-ttu-id="71766-106">Klasszikus Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="71766-106">Azure Classic PowerShell</span></span>](application-gateway-ssl.md)
> * [<span data-ttu-id="71766-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="71766-107">Azure CLI 2.0</span></span>](application-gateway-ssl-cli.md)

<span data-ttu-id="71766-108">Az Azure Application Gateway konfigurált tooterminate hello Secure Sockets Layer (SSL) munkamenet: hello átjáró tooavoid költséges SSL visszafejtési feladatok toohappen: hello webfarm lehet.</span><span class="sxs-lookup"><span data-stu-id="71766-108">Azure Application Gateway can be configured tooterminate hello Secure Sockets Layer (SSL) session at hello gateway tooavoid costly SSL decryption tasks toohappen at hello web farm.</span></span> <span data-ttu-id="71766-109">SSL kiszervezési is egyszerűbbé teszi a hello előtér-kiszolgáló beállítása és felügyelete hello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="71766-109">SSL offload also simplifies hello front-end server setup and management of hello web application.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="71766-110">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="71766-110">Before you begin</span></span>

1. <span data-ttu-id="71766-111">Webplatform-telepítő hello segítségével hello hello Azure PowerShell-parancsmagok legújabb verzióját telepítse.</span><span class="sxs-lookup"><span data-stu-id="71766-111">Install hello latest version of hello Azure PowerShell cmdlets by using hello Web Platform Installer.</span></span> <span data-ttu-id="71766-112">Töltse le, és telepítse hello legújabb verziót a hello **Windows PowerShell** hello szakasza [letöltési oldalon](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="71766-112">You can download and install hello latest version from hello **Windows PowerShell** section of hello [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="71766-113">Létrehozhat egy virtuális hálózatot és egy hello alkalmazás átjáró-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="71766-113">You create a virtual network and a subnet for hello application gateway.</span></span> <span data-ttu-id="71766-114">Győződjön meg arról, hogy egyetlen virtuális gépek vagy a felhőben történő alkalmazáshoz hello alhálózat használja.</span><span class="sxs-lookup"><span data-stu-id="71766-114">Make sure that no virtual machines or cloud deployments are using hello subnet.</span></span> <span data-ttu-id="71766-115">Az Application Gateway-nek egyedül kell lennie a virtuális hálózat alhálózatán.</span><span class="sxs-lookup"><span data-stu-id="71766-115">Application Gateway must be by itself in a virtual network subnet.</span></span>
3. <span data-ttu-id="71766-116">hello kiszolgálók toouse hello Alkalmazásátjáró konfigurálnia kell lennie, különben a végpontokat hello virtuális hálózatban vagy a nyilvános IP/virtuális IP-címhez hozzárendelt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="71766-116">hello servers you configure toouse hello application gateway must exist or have their endpoints created either in hello virtual network or with a public IP/VIP assigned.</span></span>

## <a name="what-is-required-toocreate-an-application-gateway"></a><span data-ttu-id="71766-117">Mi az a szükséges toocreate olyan átjárót?</span><span class="sxs-lookup"><span data-stu-id="71766-117">What is required toocreate an application gateway?</span></span>

* <span data-ttu-id="71766-118">**Háttér-kiszolgálófiók készlet:** hello hello háttér-kiszolgálók IP-címek listáját.</span><span class="sxs-lookup"><span data-stu-id="71766-118">**Back-end server pool:** hello list of IP addresses of hello back-end servers.</span></span> <span data-ttu-id="71766-119">hello IP-címek felsorolt toohello virtuális hálózati alhálózat vagy kell tartoznia, vagy egy nyilvános IP-cím/VIP kell lennie.</span><span class="sxs-lookup"><span data-stu-id="71766-119">hello IP addresses listed should either belong toohello virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="71766-120">**Háttér-kiszolgálókészlet beállításai:** Minden készletnek vannak beállításai, például port, protokoll vagy cookie-alapú affinitás.</span><span class="sxs-lookup"><span data-stu-id="71766-120">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="71766-121">Ezek a beállítások esetén tooa kapcsolt verem és a hello készlet alkalmazott tooall-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="71766-121">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="71766-122">**Előtér-port:** Ez a port nem hello nyilvános portot, amelyet a hello Alkalmazásátjáró meg van nyitva.</span><span class="sxs-lookup"><span data-stu-id="71766-122">**Front-end port:** This port is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="71766-123">Forgalom találatok ezt a portot, és lekérdezi átirányítja tooone hello háttér-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="71766-123">Traffic hits this port, and then gets redirected tooone of hello back-end servers.</span></span>
* <span data-ttu-id="71766-124">**Figyelő:** hello figyelő rendelkezik egy előtér-portot, a protokollt (Http vagy Https, ezek a beállítások-és nagybetűk), és hello SSL tanúsítvány neve (ha az SSL beállításának-kiszervezés).</span><span class="sxs-lookup"><span data-stu-id="71766-124">**Listener:** hello listener has a front-end port, a protocol (Http or Https, these settings are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="71766-125">**Szabály:** hello szabály hello figyelő és hello háttér-kiszolgálófiók alkalmazáskészlet van kötve, és azt határozza meg, mely háttér-kiszolgálófiók készlet hello forgalom irányított toowhen találatok száma a egy adott figyelő.</span><span class="sxs-lookup"><span data-stu-id="71766-125">**Rule:** hello rule binds hello listener and hello back-end server pool and defines which back-end server pool hello traffic should be directed toowhen it hits a particular listener.</span></span> <span data-ttu-id="71766-126">Jelenleg csak hello *alapvető* szabály használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="71766-126">Currently, only hello *basic* rule is supported.</span></span> <span data-ttu-id="71766-127">Hello *alapvető* szabály-e időszeletelés terheléselosztási.</span><span class="sxs-lookup"><span data-stu-id="71766-127">hello *basic* rule is round-robin load distribution.</span></span>

<span data-ttu-id="71766-128">**További konfigurációs megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="71766-128">**Additional configuration notes**</span></span>

<span data-ttu-id="71766-129">SSL-tanúsítványok beállítása, a hello protokoll **HttpListener** túl kell módosítani*Https* (kis-és nagybetűket).</span><span class="sxs-lookup"><span data-stu-id="71766-129">For SSL certificates configuration, hello protocol in **HttpListener** should change too*Https* (case sensitive).</span></span> <span data-ttu-id="71766-130">Hello **SslCertificate** elem túl kerül**HttpListener** hello változó értékével hello SSL-tanúsítvány konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="71766-130">hello **SslCertificate** element is added too**HttpListener** with hello variable value configured for hello SSL certificate.</span></span> <span data-ttu-id="71766-131">hello előtér-port frissített too443 lehet.</span><span class="sxs-lookup"><span data-stu-id="71766-131">hello front-end port should be updated too443.</span></span>

<span data-ttu-id="71766-132">**tooenable cookie-alapú kapcsolat**: Alkalmazásátjáró lehet győződjön meg arról, hogy egy kérelem egy ügyfél-munkamenetből mindig irányított toohello konfigurált tooensure hello webfarm azonos virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="71766-132">**tooenable cookie-based affinity**: An application gateway can be configured tooensure that a request from a client session is always directed toohello same VM in hello web farm.</span></span> <span data-ttu-id="71766-133">Ebben a forgatókönyvben történik, hogy az egy munkamenetcookie-t, amely lehetővé teszi annak megfelelően hello átjáró toodirect forgalom.</span><span class="sxs-lookup"><span data-stu-id="71766-133">This scenario is done by injection of a session cookie that allows hello gateway toodirect traffic appropriately.</span></span> <span data-ttu-id="71766-134">cookie-alapú tooenable affinitás beállítása **CookieBasedAffinity** túl*engedélyezve* a hello **BackendHttpSettings** elemet.</span><span class="sxs-lookup"><span data-stu-id="71766-134">tooenable cookie-based affinity, set **CookieBasedAffinity** too*Enabled* in hello **BackendHttpSettings** element.</span></span>

## <a name="create-an-application-gateway"></a><span data-ttu-id="71766-135">Application Gateway létrehozása</span><span class="sxs-lookup"><span data-stu-id="71766-135">Create an application gateway</span></span>

<span data-ttu-id="71766-136">hello közötti hello Azure klasszikus telepítési modell és az Azure Resource Manager használatával különbség toobe konfigurálni kell egy alkalmazás átjáró és hello elemek létrehozását hello sorrendjében.</span><span class="sxs-lookup"><span data-stu-id="71766-136">hello difference between using hello Azure Classic deployment model and Azure Resource Manager is hello order that you create an application gateway and hello items that need toobe configured.</span></span>

<span data-ttu-id="71766-137">A Resource Manager összes összetevőjének olyan átjárót konfigurált külön-külön és majd együtt toocreate egy alkalmazás átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="71766-137">With Resource Manager, all components of an application gateway are configured individually and then put together toocreate an application gateway resource.</span></span>

<span data-ttu-id="71766-138">Az alábbiakban hello szükséges lépéseket toocreate Alkalmazásátjáró:</span><span class="sxs-lookup"><span data-stu-id="71766-138">Here are hello steps needed toocreate an application gateway:</span></span>

1. <span data-ttu-id="71766-139">Erőforráscsoport létrehozása a Resource Managerhez</span><span class="sxs-lookup"><span data-stu-id="71766-139">Create a resource group for Resource Manager</span></span>
2. <span data-ttu-id="71766-140">Virtuális hálózati alhálózat és hello alkalmazás átjáró nyilvános IP-cím létrehozása</span><span class="sxs-lookup"><span data-stu-id="71766-140">Create virtual network, subnet, and public IP for hello application gateway</span></span>
3. <span data-ttu-id="71766-141">Hozzon létre egy Application Gateway konfigurációs objektumot</span><span class="sxs-lookup"><span data-stu-id="71766-141">Create an application gateway configuration object</span></span>
4. <span data-ttu-id="71766-142">Application Gateway erőforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="71766-142">Create an application gateway resource</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="71766-143">Erőforráscsoport létrehozása a Resource Managerhez</span><span class="sxs-lookup"><span data-stu-id="71766-143">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="71766-144">Győződjön meg arról, hogy a PowerShell mód toouse hello Azure Resource Manager parancsmagok váltani.</span><span class="sxs-lookup"><span data-stu-id="71766-144">Make sure that you switch PowerShell mode toouse hello Azure Resource Manager cmdlets.</span></span> <span data-ttu-id="71766-145">További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="71766-145">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="71766-146">1. lépés</span><span class="sxs-lookup"><span data-stu-id="71766-146">Step 1</span></span>

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a><span data-ttu-id="71766-147">2. lépés</span><span class="sxs-lookup"><span data-stu-id="71766-147">Step 2</span></span>

<span data-ttu-id="71766-148">Hello előfizetések hello fiók ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="71766-148">Check hello subscriptions for hello account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="71766-149">A hitelesítő adataival felszólító tooauthenticate áll.</span><span class="sxs-lookup"><span data-stu-id="71766-149">You are prompted tooauthenticate with your credentials.</span></span>

### <a name="step-3"></a><span data-ttu-id="71766-150">3. lépés</span><span class="sxs-lookup"><span data-stu-id="71766-150">Step 3</span></span>

<span data-ttu-id="71766-151">Válassza ki, amely az Azure-előfizetések toouse.</span><span class="sxs-lookup"><span data-stu-id="71766-151">Choose which of your Azure subscriptions toouse.</span></span>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a><span data-ttu-id="71766-152">4. lépés</span><span class="sxs-lookup"><span data-stu-id="71766-152">Step 4</span></span>

<span data-ttu-id="71766-153">Hozzon létre egy erőforráscsoportot (hagyja ki ezt a lépést, ha egy meglévő erőforráscsoportot használ).</span><span class="sxs-lookup"><span data-stu-id="71766-153">Create a resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

<span data-ttu-id="71766-154">Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet.</span><span class="sxs-lookup"><span data-stu-id="71766-154">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="71766-155">Ezzel a beállítással hello alapértelmezett helye az erőforráscsoport erőforrások.</span><span class="sxs-lookup"><span data-stu-id="71766-155">This setting is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="71766-156">Győződjön meg arról, hogy olyan átjárót használó összes parancsok toocreate hello azonos erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="71766-156">Make sure that all commands toocreate an application gateway uses hello same resource group.</span></span>

<span data-ttu-id="71766-157">A hello a fenti példában létrehozott nevű erőforráscsoport **appgw-RG** és a hely **USA nyugati régiója**.</span><span class="sxs-lookup"><span data-stu-id="71766-157">In hello example above, we created a resource group called **appgw-RG** and location **West US**.</span></span>

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a><span data-ttu-id="71766-158">Hozzon létre egy virtuális hálózatot és hello Alkalmazásátjáró alhálózatot</span><span class="sxs-lookup"><span data-stu-id="71766-158">Create a virtual network and a subnet for hello application gateway</span></span>

<span data-ttu-id="71766-159">a következő példa azt mutatja meg hogyan hello toocreate erőforrás-kezelő használatával egy virtuális hálózathoz:</span><span class="sxs-lookup"><span data-stu-id="71766-159">hello following example shows how toocreate a virtual network by using Resource Manager:</span></span>

### <a name="step-1"></a><span data-ttu-id="71766-160">1. lépés</span><span class="sxs-lookup"><span data-stu-id="71766-160">Step 1</span></span>

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

<span data-ttu-id="71766-161">Ez a minta egy virtuális hálózati hello cím tartomány 10.0.0.0/24 tooa alhálózati használt változó toobe toocreate rendeli hozzá.</span><span class="sxs-lookup"><span data-stu-id="71766-161">This sample assigns hello address range 10.0.0.0/24 tooa subnet variable toobe used toocreate a virtual network.</span></span>

### <a name="step-2"></a><span data-ttu-id="71766-162">2. lépés</span><span class="sxs-lookup"><span data-stu-id="71766-162">Step 2</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

<span data-ttu-id="71766-163">Ez a minta létrehoz egy virtuális hálózatot nevű **appgwvnet** erőforráscsoportban **appgw-rg** hello USA nyugati régiójában hello előtag 10.0.0.0/16 használata a 10.0.0.0/24 alhálózat.</span><span class="sxs-lookup"><span data-stu-id="71766-163">This sample creates a virtual network named **appgwvnet** in resource group **appgw-rg** for hello West US region using hello prefix 10.0.0.0/16 with subnet 10.0.0.0/24.</span></span>

### <a name="step-3"></a><span data-ttu-id="71766-164">3. lépés</span><span class="sxs-lookup"><span data-stu-id="71766-164">Step 3</span></span>

```powershell
$subnet = $vnet.Subnets[0]
```

<span data-ttu-id="71766-165">Ez a minta hozzárendel hello alhálózati objektum toovariable $subnet hello további lépéseket.</span><span class="sxs-lookup"><span data-stu-id="71766-165">This sample assigns hello subnet object toovariable $subnet for hello next steps.</span></span>

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a><span data-ttu-id="71766-166">A nyilvános IP-cím hello előtér-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="71766-166">Create a public IP address for hello front-end configuration</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

<span data-ttu-id="71766-167">Ez a minta létrehoz egy nyilvános IP-erőforrás **publicIP01** erőforráscsoportban **appgw-rg** hello USA nyugati régiójában.</span><span class="sxs-lookup"><span data-stu-id="71766-167">This sample creates a public IP resource **publicIP01** in resource group **appgw-rg** for hello West US region.</span></span>

## <a name="create-an-application-gateway-configuration-object"></a><span data-ttu-id="71766-168">Hozzon létre egy Application Gateway konfigurációs objektumot</span><span class="sxs-lookup"><span data-stu-id="71766-168">Create an application gateway configuration object</span></span>

### <a name="step-1"></a><span data-ttu-id="71766-169">1. lépés</span><span class="sxs-lookup"><span data-stu-id="71766-169">Step 1</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

<span data-ttu-id="71766-170">Ez a minta létrehoz egy alkalmazás átjáró IP-konfiguráció nevű **gatewayIP01**.</span><span class="sxs-lookup"><span data-stu-id="71766-170">This sample creates an application gateway IP configuration named **gatewayIP01**.</span></span> <span data-ttu-id="71766-171">Alkalmazásátjáró indításakor szerzi be hello alhálózati konfigurált IP-címeit, és útvonal-hello háttér IP-készletet a hálózati forgalom toohello IP-címek.</span><span class="sxs-lookup"><span data-stu-id="71766-171">When Application Gateway starts, it picks up an IP address from hello subnet configured and route network traffic toohello IP addresses in hello back-end IP pool.</span></span> <span data-ttu-id="71766-172">Ne feledje, hogy minden példány egy IP-címet vesz fel.</span><span class="sxs-lookup"><span data-stu-id="71766-172">Keep in mind that each instance takes one IP address.</span></span>

### <a name="step-2"></a><span data-ttu-id="71766-173">2. lépés</span><span class="sxs-lookup"><span data-stu-id="71766-173">Step 2</span></span>

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50
```

<span data-ttu-id="71766-174">Ez a minta konfigurálja hello háttér IP-címkészlet nevű **pool01** IP-címekkel rendelkező **134.170.185.46**, **134.170.188.221**, **134.170.185.50** .</span><span class="sxs-lookup"><span data-stu-id="71766-174">This sample configures hello back-end IP address pool named **pool01** with IP addresses **134.170.185.46**, **134.170.188.221**, **134.170.185.50**.</span></span> <span data-ttu-id="71766-175">Ezeket az értékeket azokat hello IP-címeket, amelyek hello előtér-IP-végponton származik hello hálózati forgalom fogadására.</span><span class="sxs-lookup"><span data-stu-id="71766-175">Those values are hello IP addresses that receive hello network traffic that comes from hello front-end IP endpoint.</span></span> <span data-ttu-id="71766-176">Cserélje le a webes alkalmazás végpontok hello IP-címekkel rendelkező példa megelőző hello hello IP-címek.</span><span class="sxs-lookup"><span data-stu-id="71766-176">Replace hello IP addresses from hello preceding example with hello IP addresses of your web application endpoints.</span></span>

### <a name="step-3"></a><span data-ttu-id="71766-177">3. lépés</span><span class="sxs-lookup"><span data-stu-id="71766-177">Step 3</span></span>

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled
```

<span data-ttu-id="71766-178">Ez a minta konfigurálja az átjáró Alkalmazásbeállítás **poolsetting01** tooload kiegyensúlyozott hálózati forgalom hello háttér-készletben.</span><span class="sxs-lookup"><span data-stu-id="71766-178">This sample configures application gateway setting **poolsetting01** tooload-balanced network traffic in hello back-end pool.</span></span>

### <a name="step-4"></a><span data-ttu-id="71766-179">4. lépés</span><span class="sxs-lookup"><span data-stu-id="71766-179">Step 4</span></span>

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443
```

<span data-ttu-id="71766-180">Ez a minta hello nevű előtér-IP-port konfigurálása **frontendport01** a hello nyilvános IP-végponton.</span><span class="sxs-lookup"><span data-stu-id="71766-180">This sample configures hello front-end IP port named **frontendport01** for hello public IP endpoint.</span></span>

### <a name="step-5"></a><span data-ttu-id="71766-181">5. lépés</span><span class="sxs-lookup"><span data-stu-id="71766-181">Step 5</span></span>

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password "<password>"
```

<span data-ttu-id="71766-182">Ez a minta hello tanúsítvány használt SSL-kapcsolat konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="71766-182">This sample configures hello certificate used for SSL connection.</span></span> <span data-ttu-id="71766-183">hello tanúsítványt kell toobe .pfx formátumú, és hello jelszó 4 too12 karakter közötti lehet.</span><span class="sxs-lookup"><span data-stu-id="71766-183">hello certificate needs toobe in .pfx format, and hello password must be between 4 too12 characters.</span></span>

### <a name="step-6"></a><span data-ttu-id="71766-184">6. lépés</span><span class="sxs-lookup"><span data-stu-id="71766-184">Step 6</span></span>

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip
```

<span data-ttu-id="71766-185">Ez a minta létrehoz hello előtér-IP-konfiguráció **fipconfig01** és társult hello hello előtér-IP-konfigurációja nyilvános IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="71766-185">This sample creates hello front-end IP configuration named **fipconfig01** and associates hello public IP address with hello front-end IP configuration.</span></span>

### <a name="step-7"></a><span data-ttu-id="71766-186">7. lépés</span><span class="sxs-lookup"><span data-stu-id="71766-186">Step 7</span></span>

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert
```

<span data-ttu-id="71766-187">Ez a minta hello figyelő nevét hozza létre **listener01** és társult hello előtér-port toohello előtér-IP-konfiguráció és a tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="71766-187">This sample creates hello listener name **listener01** and associates hello front-end port toohello front-end IP configuration and certificate.</span></span>

### <a name="step-8"></a><span data-ttu-id="71766-188">8. lépés</span><span class="sxs-lookup"><span data-stu-id="71766-188">Step 8</span></span>

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

<span data-ttu-id="71766-189">Ez a minta létrehoz hello terheléselosztási útválasztási szabály nevű **rule01** , konfigurálja az hello load balancer viselkedését.</span><span class="sxs-lookup"><span data-stu-id="71766-189">This sample creates hello load balancer routing rule named **rule01** that configures hello load balancer behavior.</span></span>

### <a name="step-9"></a><span data-ttu-id="71766-190">9. lépés</span><span class="sxs-lookup"><span data-stu-id="71766-190">Step 9</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

<span data-ttu-id="71766-191">Ez a minta hello példányméretének hello Alkalmazásátjáró konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="71766-191">This sample configures hello instance size of hello application gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="71766-192">az alapértelmezett érték hello *InstanceCount* 2, maximális értéke 10.</span><span class="sxs-lookup"><span data-stu-id="71766-192">hello default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="71766-193">az alapértelmezett érték hello *GatewaySize* közepes.</span><span class="sxs-lookup"><span data-stu-id="71766-193">hello default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="71766-194">A Standard_Small, Standard_Medium és a Standard_Large lehetőségek közül választhat.</span><span class="sxs-lookup"><span data-stu-id="71766-194">You can choose between Standard_Small, Standard_Medium, and Standard_Large.</span></span>

### <a name="step-10"></a><span data-ttu-id="71766-195">10. lépés</span><span class="sxs-lookup"><span data-stu-id="71766-195">Step 10</span></span>

```powershell
$policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S
```

<span data-ttu-id="71766-196">Ez a lépés hello SSL házirend toouse hello Alkalmazásátjáró határozza meg.</span><span class="sxs-lookup"><span data-stu-id="71766-196">This step defines hello SSL policy toouse on hello application gateway.</span></span> <span data-ttu-id="71766-197">Látogasson el [SSL konfigurálása házirend verziója és az Application Gateway titkosító csomagok](application-gateway-configure-ssl-policy-powershell.md) további toolearn.</span><span class="sxs-lookup"><span data-stu-id="71766-197">Visit [Configure SSL policy versions and cipher suites on Application Gateway](application-gateway-configure-ssl-policy-powershell.md) toolearn more.</span></span>

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a><span data-ttu-id="71766-198">Application Gateway létrehozása a New-AzureApplicationGateway használatával</span><span class="sxs-lookup"><span data-stu-id="71766-198">Create an application gateway by using New-AzureApplicationGateway</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert -SslPolicy $policy
```

<span data-ttu-id="71766-199">Ez a minta Alkalmazásátjáró az előző lépésekben hello összes konfigurációs elemet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="71766-199">This sample creates an application gateway with all configuration items from hello preceding steps.</span></span> <span data-ttu-id="71766-200">Hello példában hello Alkalmazásátjáró nevezik **appgwtest**.</span><span class="sxs-lookup"><span data-stu-id="71766-200">In hello example, hello application gateway is called **appgwtest**.</span></span>

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="71766-201">Az Application Gateway DNS-nevének beszerzése</span><span class="sxs-lookup"><span data-stu-id="71766-201">Get application gateway DNS name</span></span>

<span data-ttu-id="71766-202">Hello átjáró létrehozása után hello következő lépésre tooconfigure hello előtér-kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="71766-202">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="71766-203">Nyilvános IP-cím esetén az Application Gateway használatához dinamikusan hozzárendelt DNS-névre van szükség, amely nem valódi név.</span><span class="sxs-lookup"><span data-stu-id="71766-203">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="71766-204">tooensure a végfelhasználók is találati hello Alkalmazásátjáró, egy olyan CNAME rekordot is használt toopoint toohello nyilvános végpontot hello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="71766-204">tooensure end users can hit hello application gateway, a CNAME record can be used toopoint toohello public endpoint of hello application gateway.</span></span> <span data-ttu-id="71766-205">[Egyéni tartománynév konfigurálása az Azure-ban](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="71766-205">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="71766-206">toodo a, hello Alkalmazásátjáró és a társított IP-/ DNS-nevét, hello PublicIPAddress elem csatolt toohello Alkalmazásátjáró beolvasása részleteit.</span><span class="sxs-lookup"><span data-stu-id="71766-206">toodo this, retrieve details of hello application gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello application gateway.</span></span> <span data-ttu-id="71766-207">hello alkalmazás átjáró DNS-névnek kell lennie a használt toocreate egy olyan CNAME rekordot pontok hello két webes alkalmazások toothis DNS-név.</span><span class="sxs-lookup"><span data-stu-id="71766-207">hello application gateway's DNS name should be used toocreate a CNAME record, which points hello two web applications toothis DNS name.</span></span> <span data-ttu-id="71766-208">A-rekordok hello használata nem javasolt, mert hello VIP módosíthatja az Alkalmazásátjáró újra kell indítani.</span><span class="sxs-lookup"><span data-stu-id="71766-208">hello use of A-records is not recommended since hello VIP may change on restart of application gateway.</span></span>


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

## <a name="next-steps"></a><span data-ttu-id="71766-209">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="71766-209">Next steps</span></span>

<span data-ttu-id="71766-210">Ha azt szeretné, hogy egy alkalmazás átjáró toouse egy belső terheléselosztón (ILB) rendelkező tooconfigure, [hozzon létre egy alkalmazást egy belső terheléselosztón (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="71766-210">If you want tooconfigure an application gateway toouse with an internal load balancer (ILB), see [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="71766-211">Ha további általános információra van szüksége a terheléselosztás beállításaival kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="71766-211">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="71766-212">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="71766-212">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="71766-213">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="71766-213">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

