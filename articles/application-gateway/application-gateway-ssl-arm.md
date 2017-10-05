---
title: "Konfigurálja az SSL kiszervezési - Azure Application Gateway - PowerShell |} Microsoft Docs"
description: "Ez az oldal utasításokat tartalmaz egy SSL-alapú kiszervezéssel rendelkező Application Gateway létrehozásához az Azure Resource Manager használatával"
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
ms.openlocfilehash: ededabc7c665d6bb05b91e4d21d01fb1379add32
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-resource-manager"></a><span data-ttu-id="4e53d-103">Application Gateway konfigurálása SSL-alapú kiszervezéshez az Azure Resource Manager használatával</span><span class="sxs-lookup"><span data-stu-id="4e53d-103">Configure an application gateway for SSL offload by using Azure Resource Manager</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="4e53d-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4e53d-104">Azure portal</span></span>](application-gateway-ssl-portal.md)
> * [<span data-ttu-id="4e53d-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="4e53d-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ssl-arm.md)
> * [<span data-ttu-id="4e53d-106">Klasszikus Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="4e53d-106">Azure Classic PowerShell</span></span>](application-gateway-ssl.md)
> * [<span data-ttu-id="4e53d-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="4e53d-107">Azure CLI 2.0</span></span>](application-gateway-ssl-cli.md)

<span data-ttu-id="4e53d-108">Az Azure Application Gateway konfigurálható úgy, hogy leállítsa a Secure Sockets Layer (SSL) munkamenetét az átjárónál, így elkerülhetők a költséges SSL visszafejtési feladatok a webfarmon.</span><span class="sxs-lookup"><span data-stu-id="4e53d-108">Azure Application Gateway can be configured to terminate the Secure Sockets Layer (SSL) session at the gateway to avoid costly SSL decryption tasks to happen at the web farm.</span></span> <span data-ttu-id="4e53d-109">Az SSL-alapú kiszervezés emellett leegyszerűsíti az előtér-kiszolgáló számára webalkalmazás telepítését és kezelését.</span><span class="sxs-lookup"><span data-stu-id="4e53d-109">SSL offload also simplifies the front-end server setup and management of the web application.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="4e53d-110">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="4e53d-110">Before you begin</span></span>

1. <span data-ttu-id="4e53d-111">Telepítse az Azure PowerShell-parancsmagok legújabb verzióját a Webplatform-telepítővel.</span><span class="sxs-lookup"><span data-stu-id="4e53d-111">Install the latest version of the Azure PowerShell cmdlets by using the Web Platform Installer.</span></span> <span data-ttu-id="4e53d-112">A [Letöltések lap](https://azure.microsoft.com/downloads/) **Windows PowerShell** szakaszából letöltheti és telepítheti a legújabb verziót.</span><span class="sxs-lookup"><span data-stu-id="4e53d-112">You can download and install the latest version from the **Windows PowerShell** section of the [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="4e53d-113">Létre kell hozni egy virtuális hálózatot és alhálózatot az Application Gateway számára.</span><span class="sxs-lookup"><span data-stu-id="4e53d-113">You create a virtual network and a subnet for the application gateway.</span></span> <span data-ttu-id="4e53d-114">Győződjön meg arról, hogy egy virtuális gép vagy felhőalapú telepítés sem használja az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="4e53d-114">Make sure that no virtual machines or cloud deployments are using the subnet.</span></span> <span data-ttu-id="4e53d-115">Az Application Gateway-nek egyedül kell lennie a virtuális hálózat alhálózatán.</span><span class="sxs-lookup"><span data-stu-id="4e53d-115">Application Gateway must be by itself in a virtual network subnet.</span></span>
3. <span data-ttu-id="4e53d-116">A kiszolgálóknak, amelyeket az Application Gateway használatára konfigurál, már létezniük kell, illetve a virtuális hálózatban vagy hozzárendelt nyilvános/virtuális IP-címmel létrehozott végpontokkal kell rendelkezniük.</span><span class="sxs-lookup"><span data-stu-id="4e53d-116">The servers you configure to use the application gateway must exist or have their endpoints created either in the virtual network or with a public IP/VIP assigned.</span></span>

## <a name="what-is-required-to-create-an-application-gateway"></a><span data-ttu-id="4e53d-117">Mire van szükség egy Application Gateway létrehozásához?</span><span class="sxs-lookup"><span data-stu-id="4e53d-117">What is required to create an application gateway?</span></span>

* <span data-ttu-id="4e53d-118">**Háttér-kiszolgálókészlet:** A háttérkiszolgálók IP-címeinek listája.</span><span class="sxs-lookup"><span data-stu-id="4e53d-118">**Back-end server pool:** The list of IP addresses of the back-end servers.</span></span> <span data-ttu-id="4e53d-119">A listán szereplő IP-címeknek a virtuális hálózat alhálózatához kell tartozniuk, vagy nyilvános/virtuális IP-címnek kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="4e53d-119">The IP addresses listed should either belong to the virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="4e53d-120">**Háttér-kiszolgálókészlet beállításai:** Minden készletnek vannak beállításai, például port, protokoll vagy cookie-alapú affinitás.</span><span class="sxs-lookup"><span data-stu-id="4e53d-120">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="4e53d-121">Ezek a beállítások egy adott készlethez kapcsolódnak, és a készlet minden kiszolgálójára érvényesek.</span><span class="sxs-lookup"><span data-stu-id="4e53d-121">These settings are tied to a pool and are applied to all servers within the pool.</span></span>
* <span data-ttu-id="4e53d-122">**Előtérbeli port:** Az Application Gateway-en megnyitott nyilvános port.</span><span class="sxs-lookup"><span data-stu-id="4e53d-122">**Front-end port:** This port is the public port that is opened on the application gateway.</span></span> <span data-ttu-id="4e53d-123">Amikor a forgalom eléri ezt a portot, a port átirányítja az egyik háttérkiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="4e53d-123">Traffic hits this port, and then gets redirected to one of the back-end servers.</span></span>
* <span data-ttu-id="4e53d-124">**Figyelő:** A figyelő egy előtérbeli porttal, egy protokollal (Http vagy Https, ezek kis- és nagybetűt megkülönböztető beállítások) és SSL tanúsítványnévvel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="4e53d-124">**Listener:** The listener has a front-end port, a protocol (Http or Https, these settings are case-sensitive), and the SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="4e53d-125">**Szabály:** A szabály összeköti a figyelőt és a háttérkiszolgáló-készletet, és meghatározza, hogy mely háttérkiszolgáló-készletre legyen átirányítva a forgalom, ha elér egy adott figyelőt.</span><span class="sxs-lookup"><span data-stu-id="4e53d-125">**Rule:** The rule binds the listener and the back-end server pool and defines which back-end server pool the traffic should be directed to when it hits a particular listener.</span></span> <span data-ttu-id="4e53d-126">Jelenleg csak a *basic* szabály támogatott.</span><span class="sxs-lookup"><span data-stu-id="4e53d-126">Currently, only the *basic* rule is supported.</span></span> <span data-ttu-id="4e53d-127">A *basic* szabály a ciklikus időszeleteléses terheléselosztás.</span><span class="sxs-lookup"><span data-stu-id="4e53d-127">The *basic* rule is round-robin load distribution.</span></span>

<span data-ttu-id="4e53d-128">**További konfigurációs megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="4e53d-128">**Additional configuration notes**</span></span>

<span data-ttu-id="4e53d-129">Az SSL-tanúsítványok konfigurálásához *Https*-re kell módosítani a **HttpListener** protokollját (megkülönböztetve a kis- és nagybetűket).</span><span class="sxs-lookup"><span data-stu-id="4e53d-129">For SSL certificates configuration, the protocol in **HttpListener** should change to *Https* (case sensitive).</span></span> <span data-ttu-id="4e53d-130">Az **SslCertificate** elemet hozzá kell adni a **HttpListener** elemhez az SSL-tanúsítvány számára konfigurált változóértékkel.</span><span class="sxs-lookup"><span data-stu-id="4e53d-130">The **SslCertificate** element is added to **HttpListener** with the variable value configured for the SSL certificate.</span></span> <span data-ttu-id="4e53d-131">Az előtérbeli portot frissíteni kell 443-ra.</span><span class="sxs-lookup"><span data-stu-id="4e53d-131">The front-end port should be updated to 443.</span></span>

<span data-ttu-id="4e53d-132">**A cookie-alapú affinitás engedélyezése**: Egy Application Gateway konfigurálható úgy, hogy egy ügyfélmunkamenetből érkező kérelmet mindig a webfarm ugyanazon virtuális gépére irányítsa.</span><span class="sxs-lookup"><span data-stu-id="4e53d-132">**To enable cookie-based affinity**: An application gateway can be configured to ensure that a request from a client session is always directed to the same VM in the web farm.</span></span> <span data-ttu-id="4e53d-133">Ez a forgatókönyv egy munkameneti cookie beszúrásával lesz végrehajtva, amely lehetővé teszi az átjáró számára a forgalom megfelelő irányítását.</span><span class="sxs-lookup"><span data-stu-id="4e53d-133">This scenario is done by injection of a session cookie that allows the gateway to direct traffic appropriately.</span></span> <span data-ttu-id="4e53d-134">A cookie-alapú affinitás engedélyezéséhez a **CookieBasedAffinity** paraméter beállítása legyen *Enabled* a **BackendHttpSettings** elemen belül.</span><span class="sxs-lookup"><span data-stu-id="4e53d-134">To enable cookie-based affinity, set **CookieBasedAffinity** to *Enabled* in the **BackendHttpSettings** element.</span></span>

## <a name="create-an-application-gateway"></a><span data-ttu-id="4e53d-135">Application Gateway létrehozása</span><span class="sxs-lookup"><span data-stu-id="4e53d-135">Create an application gateway</span></span>

<span data-ttu-id="4e53d-136">A klasszikus Azure üzembe helyezési modell és az Azure Resource Manager használata abban tér el egymástól, hogy más sorrendben kell létrehoznia az Application Gateway-t és a konfigurálást igénylő elemeket.</span><span class="sxs-lookup"><span data-stu-id="4e53d-136">The difference between using the Azure Classic deployment model and Azure Resource Manager is the order that you create an application gateway and the items that need to be configured.</span></span>

<span data-ttu-id="4e53d-137">A Resource Managerrel az Application Gateway összes összetevőjét külön kell konfigurálni, és csak utána kell őket összeállítani az Application Gateway-erőforrás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="4e53d-137">With Resource Manager, all components of an application gateway are configured individually and then put together to create an application gateway resource.</span></span>

<span data-ttu-id="4e53d-138">Egy Application Gateway létrehozásához a következő lépéseket kell végrehajtania:</span><span class="sxs-lookup"><span data-stu-id="4e53d-138">Here are the steps needed to create an application gateway:</span></span>

1. <span data-ttu-id="4e53d-139">Erőforráscsoport létrehozása a Resource Managerhez</span><span class="sxs-lookup"><span data-stu-id="4e53d-139">Create a resource group for Resource Manager</span></span>
2. <span data-ttu-id="4e53d-140">Virtuális hálózat, alhálózat és nyilvános IP-cím létrehozása az Application Gateway számára</span><span class="sxs-lookup"><span data-stu-id="4e53d-140">Create virtual network, subnet, and public IP for the application gateway</span></span>
3. <span data-ttu-id="4e53d-141">Hozzon létre egy Application Gateway konfigurációs objektumot</span><span class="sxs-lookup"><span data-stu-id="4e53d-141">Create an application gateway configuration object</span></span>
4. <span data-ttu-id="4e53d-142">Application Gateway erőforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="4e53d-142">Create an application gateway resource</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="4e53d-143">Erőforráscsoport létrehozása a Resource Managerhez</span><span class="sxs-lookup"><span data-stu-id="4e53d-143">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="4e53d-144">Az Azure Resource Manager parancsmagjainak használatához váltson át PowerShell módba.</span><span class="sxs-lookup"><span data-stu-id="4e53d-144">Make sure that you switch PowerShell mode to use the Azure Resource Manager cmdlets.</span></span> <span data-ttu-id="4e53d-145">További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="4e53d-145">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="4e53d-146">1. lépés</span><span class="sxs-lookup"><span data-stu-id="4e53d-146">Step 1</span></span>

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a><span data-ttu-id="4e53d-147">2. lépés</span><span class="sxs-lookup"><span data-stu-id="4e53d-147">Step 2</span></span>

<span data-ttu-id="4e53d-148">Keresse meg a fiókot az előfizetésekben.</span><span class="sxs-lookup"><span data-stu-id="4e53d-148">Check the subscriptions for the account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="4e53d-149">A rendszer kérni fogja a hitelesítő adatokkal történő hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="4e53d-149">You are prompted to authenticate with your credentials.</span></span>

### <a name="step-3"></a><span data-ttu-id="4e53d-150">3. lépés</span><span class="sxs-lookup"><span data-stu-id="4e53d-150">Step 3</span></span>

<span data-ttu-id="4e53d-151">Válassza ki, hogy melyik Azure előfizetést fogja használni.</span><span class="sxs-lookup"><span data-stu-id="4e53d-151">Choose which of your Azure subscriptions to use.</span></span>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a><span data-ttu-id="4e53d-152">4. lépés</span><span class="sxs-lookup"><span data-stu-id="4e53d-152">Step 4</span></span>

<span data-ttu-id="4e53d-153">Hozzon létre egy erőforráscsoportot (hagyja ki ezt a lépést, ha egy meglévő erőforráscsoportot használ).</span><span class="sxs-lookup"><span data-stu-id="4e53d-153">Create a resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

<span data-ttu-id="4e53d-154">Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet.</span><span class="sxs-lookup"><span data-stu-id="4e53d-154">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="4e53d-155">Ez a beállítás szolgál az erőforráscsoport erőforrásainak alapértelmezett helyeként.</span><span class="sxs-lookup"><span data-stu-id="4e53d-155">This setting is used as the default location for resources in that resource group.</span></span> <span data-ttu-id="4e53d-156">Győződjön meg arról, hogy az Application Gateway létrehozására irányuló összes parancs ugyanazt az erőforráscsoportot használja.</span><span class="sxs-lookup"><span data-stu-id="4e53d-156">Make sure that all commands to create an application gateway uses the same resource group.</span></span>

<span data-ttu-id="4e53d-157">A fenti példában létrehoztunk egy **appgw-RG** nevű erőforráscsoportot, amelynek a helye az USA nyugati régiója (**West US**).</span><span class="sxs-lookup"><span data-stu-id="4e53d-157">In the example above, we created a resource group called **appgw-RG** and location **West US**.</span></span>

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a><span data-ttu-id="4e53d-158">Virtuális hálózat és alhálózat létrehozása az Application Gateway számára</span><span class="sxs-lookup"><span data-stu-id="4e53d-158">Create a virtual network and a subnet for the application gateway</span></span>

<span data-ttu-id="4e53d-159">Az alábbi példa bemutatja, hogyan hozhat létre egy virtuális hálózatot a Resource Manager használatával:</span><span class="sxs-lookup"><span data-stu-id="4e53d-159">The following example shows how to create a virtual network by using Resource Manager:</span></span>

### <a name="step-1"></a><span data-ttu-id="4e53d-160">1. lépés</span><span class="sxs-lookup"><span data-stu-id="4e53d-160">Step 1</span></span>

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

<span data-ttu-id="4e53d-161">Ez a példa hozzárendeli a 10.0.0.0/24 címtartományt a virtuális hálózat létrehozásához használni kívánt egyik alhálózati változóhoz.</span><span class="sxs-lookup"><span data-stu-id="4e53d-161">This sample assigns the address range 10.0.0.0/24 to a subnet variable to be used to create a virtual network.</span></span>

### <a name="step-2"></a><span data-ttu-id="4e53d-162">2. lépés</span><span class="sxs-lookup"><span data-stu-id="4e53d-162">Step 2</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

<span data-ttu-id="4e53d-163">Ez a minta létrehoz egy virtuális hálózatot nevű **appgwvnet** erőforráscsoportban **appgw-rg** az USA nyugati régiója régió a előtag 10.0.0.0/16 használata a 10.0.0.0/24 alhálózat.</span><span class="sxs-lookup"><span data-stu-id="4e53d-163">This sample creates a virtual network named **appgwvnet** in resource group **appgw-rg** for the West US region using the prefix 10.0.0.0/16 with subnet 10.0.0.0/24.</span></span>

### <a name="step-3"></a><span data-ttu-id="4e53d-164">3. lépés</span><span class="sxs-lookup"><span data-stu-id="4e53d-164">Step 3</span></span>

```powershell
$subnet = $vnet.Subnets[0]
```

<span data-ttu-id="4e53d-165">Ez a példa hozzárendeli az alhálózati objektumot a $subnet változóhoz a következő lépésekhez.</span><span class="sxs-lookup"><span data-stu-id="4e53d-165">This sample assigns the subnet object to variable $subnet for the next steps.</span></span>

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a><span data-ttu-id="4e53d-166">Nyilvános IP-cím létrehozása az előtérbeli konfigurációhoz</span><span class="sxs-lookup"><span data-stu-id="4e53d-166">Create a public IP address for the front-end configuration</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

<span data-ttu-id="4e53d-167">Ez a minta létrehoz egy nyilvános IP-erőforrás **publicIP01** erőforráscsoportban **appgw-rg** az USA nyugati régiójában.</span><span class="sxs-lookup"><span data-stu-id="4e53d-167">This sample creates a public IP resource **publicIP01** in resource group **appgw-rg** for the West US region.</span></span>

## <a name="create-an-application-gateway-configuration-object"></a><span data-ttu-id="4e53d-168">Hozzon létre egy Application Gateway konfigurációs objektumot</span><span class="sxs-lookup"><span data-stu-id="4e53d-168">Create an application gateway configuration object</span></span>

### <a name="step-1"></a><span data-ttu-id="4e53d-169">1. lépés</span><span class="sxs-lookup"><span data-stu-id="4e53d-169">Step 1</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

<span data-ttu-id="4e53d-170">Ez a minta létrehoz egy alkalmazás átjáró IP-konfiguráció nevű **gatewayIP01**.</span><span class="sxs-lookup"><span data-stu-id="4e53d-170">This sample creates an application gateway IP configuration named **gatewayIP01**.</span></span> <span data-ttu-id="4e53d-171">Amikor az Application Gateway elindul, a konfigurált alhálózatból felvesz egy IP-címet, és a hálózati forgalmat a háttérbeli IP-készlet IP-címeihez irányítja.</span><span class="sxs-lookup"><span data-stu-id="4e53d-171">When Application Gateway starts, it picks up an IP address from the subnet configured and route network traffic to the IP addresses in the back-end IP pool.</span></span> <span data-ttu-id="4e53d-172">Ne feledje, hogy minden példány egy IP-címet vesz fel.</span><span class="sxs-lookup"><span data-stu-id="4e53d-172">Keep in mind that each instance takes one IP address.</span></span>

### <a name="step-2"></a><span data-ttu-id="4e53d-173">2. lépés</span><span class="sxs-lookup"><span data-stu-id="4e53d-173">Step 2</span></span>

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50
```

<span data-ttu-id="4e53d-174">Ez a minta beállítja a háttér IP-címkészlet nevű **pool01** IP-címekkel rendelkező **134.170.185.46**, **134.170.188.221**, **134.170.185.50**.</span><span class="sxs-lookup"><span data-stu-id="4e53d-174">This sample configures the back-end IP address pool named **pool01** with IP addresses **134.170.185.46**, **134.170.188.221**, **134.170.185.50**.</span></span> <span data-ttu-id="4e53d-175">Ezek az értékek azok az IP-címek, amelyek fogadják majd az előtérbeli IP-végpontból érkező hálózati forgalmat.</span><span class="sxs-lookup"><span data-stu-id="4e53d-175">Those values are the IP addresses that receive the network traffic that comes from the front-end IP endpoint.</span></span> <span data-ttu-id="4e53d-176">Cserélje le az előző példában szereplő IP-címeket a webalkalmazás végpontjainak IP-címeire.</span><span class="sxs-lookup"><span data-stu-id="4e53d-176">Replace the IP addresses from the preceding example with the IP addresses of your web application endpoints.</span></span>

### <a name="step-3"></a><span data-ttu-id="4e53d-177">3. lépés</span><span class="sxs-lookup"><span data-stu-id="4e53d-177">Step 3</span></span>

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled
```

<span data-ttu-id="4e53d-178">Ez a minta konfigurálja az átjáró Alkalmazásbeállítás **poolsetting01** terhelésű hálózati forgalmat a háttér-készletben.</span><span class="sxs-lookup"><span data-stu-id="4e53d-178">This sample configures application gateway setting **poolsetting01** to load-balanced network traffic in the back-end pool.</span></span>

### <a name="step-4"></a><span data-ttu-id="4e53d-179">4. lépés</span><span class="sxs-lookup"><span data-stu-id="4e53d-179">Step 4</span></span>

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443
```

<span data-ttu-id="4e53d-180">Ez a minta konfigurálja az előtér-IP-port nevű **frontendport01** a nyilvános IP-végponton.</span><span class="sxs-lookup"><span data-stu-id="4e53d-180">This sample configures the front-end IP port named **frontendport01** for the public IP endpoint.</span></span>

### <a name="step-5"></a><span data-ttu-id="4e53d-181">5. lépés</span><span class="sxs-lookup"><span data-stu-id="4e53d-181">Step 5</span></span>

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password "<password>"
```

<span data-ttu-id="4e53d-182">Ez a példa az SSL-kapcsolathoz használt tanúsítványt konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="4e53d-182">This sample configures the certificate used for SSL connection.</span></span> <span data-ttu-id="4e53d-183">A tanúsítványnak .pfx formátumúnak, a jelszónak pedig 4 és 12 karakter közötti hosszúságúnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4e53d-183">The certificate needs to be in .pfx format, and the password must be between 4 to 12 characters.</span></span>

### <a name="step-6"></a><span data-ttu-id="4e53d-184">6. lépés</span><span class="sxs-lookup"><span data-stu-id="4e53d-184">Step 6</span></span>

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip
```

<span data-ttu-id="4e53d-185">Ez a minta hoz létre az előtér-IP-konfiguráció **fipconfig01** és a nyilvános IP-címet társít az előtér-IP-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="4e53d-185">This sample creates the front-end IP configuration named **fipconfig01** and associates the public IP address with the front-end IP configuration.</span></span>

### <a name="step-7"></a><span data-ttu-id="4e53d-186">7. lépés</span><span class="sxs-lookup"><span data-stu-id="4e53d-186">Step 7</span></span>

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert
```

<span data-ttu-id="4e53d-187">Ez a minta hozza létre a figyelő nevét **listener01** , és hozzárendeli az előtér-IP-konfiguráció és a tanúsítvány az előtér-port.</span><span class="sxs-lookup"><span data-stu-id="4e53d-187">This sample creates the listener name **listener01** and associates the front-end port to the front-end IP configuration and certificate.</span></span>

### <a name="step-8"></a><span data-ttu-id="4e53d-188">8. lépés</span><span class="sxs-lookup"><span data-stu-id="4e53d-188">Step 8</span></span>

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

<span data-ttu-id="4e53d-189">Ez a minta hoz létre a terheléselosztó útválasztási szabály nevű **rule01** , konfigurálja a terheléselosztó terheléselosztási viselkedését.</span><span class="sxs-lookup"><span data-stu-id="4e53d-189">This sample creates the load balancer routing rule named **rule01** that configures the load balancer behavior.</span></span>

### <a name="step-9"></a><span data-ttu-id="4e53d-190">9. lépés</span><span class="sxs-lookup"><span data-stu-id="4e53d-190">Step 9</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

<span data-ttu-id="4e53d-191">Ez az Application Gateway példányméretét konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="4e53d-191">This sample configures the instance size of the application gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="4e53d-192">Az *InstanceCount* alapértelmezett értéke 2, a maximális értéke pedig 10.</span><span class="sxs-lookup"><span data-stu-id="4e53d-192">The default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="4e53d-193">A *GatewaySize* alapértelmezett értéke Közepes.</span><span class="sxs-lookup"><span data-stu-id="4e53d-193">The default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="4e53d-194">A Standard_Small, Standard_Medium és a Standard_Large lehetőségek közül választhat.</span><span class="sxs-lookup"><span data-stu-id="4e53d-194">You can choose between Standard_Small, Standard_Medium, and Standard_Large.</span></span>

### <a name="step-10"></a><span data-ttu-id="4e53d-195">10. lépés</span><span class="sxs-lookup"><span data-stu-id="4e53d-195">Step 10</span></span>

```powershell
$policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S
```

<span data-ttu-id="4e53d-196">Ez a lépés határozza meg az SSL-házirend az Alkalmazásátjáró használatára.</span><span class="sxs-lookup"><span data-stu-id="4e53d-196">This step defines the SSL policy to use on the application gateway.</span></span> <span data-ttu-id="4e53d-197">Látogasson el [SSL konfigurálása házirend verziója és az Application Gateway titkosító csomagok](application-gateway-configure-ssl-policy-powershell.md) további.</span><span class="sxs-lookup"><span data-stu-id="4e53d-197">Visit [Configure SSL policy versions and cipher suites on Application Gateway](application-gateway-configure-ssl-policy-powershell.md) to learn more.</span></span>

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a><span data-ttu-id="4e53d-198">Application Gateway létrehozása a New-AzureApplicationGateway használatával</span><span class="sxs-lookup"><span data-stu-id="4e53d-198">Create an application gateway by using New-AzureApplicationGateway</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert -SslPolicy $policy
```

<span data-ttu-id="4e53d-199">Ez létrehoz egy Application Gateway-t az előző lépések konfigurációs elemeivel.</span><span class="sxs-lookup"><span data-stu-id="4e53d-199">This sample creates an application gateway with all configuration items from the preceding steps.</span></span> <span data-ttu-id="4e53d-200">A példában az Alkalmazásátjáró nevezik **appgwtest**.</span><span class="sxs-lookup"><span data-stu-id="4e53d-200">In the example, the application gateway is called **appgwtest**.</span></span>

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="4e53d-201">Az Application Gateway DNS-nevének beszerzése</span><span class="sxs-lookup"><span data-stu-id="4e53d-201">Get application gateway DNS name</span></span>

<span data-ttu-id="4e53d-202">Az átjáró létrehozása után a következő lépés a kommunikációra szolgáló előtér konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="4e53d-202">Once the gateway is created, the next step is to configure the front end for communication.</span></span> <span data-ttu-id="4e53d-203">Nyilvános IP-cím esetén az Application Gateway használatához dinamikusan hozzárendelt DNS-névre van szükség, amely nem valódi név.</span><span class="sxs-lookup"><span data-stu-id="4e53d-203">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="4e53d-204">Ha szeretné, hogy a végfelhasználók elérjék az Application Gatewayt, használjon egy Application Gateway nyilvános végpontjára mutató CNAME-rekordot.</span><span class="sxs-lookup"><span data-stu-id="4e53d-204">To ensure end users can hit the application gateway, a CNAME record can be used to point to the public endpoint of the application gateway.</span></span> <span data-ttu-id="4e53d-205">[Egyéni tartománynév konfigurálása az Azure-ban](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4e53d-205">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="4e53d-206">A művelet végrehajtásához az Application Gateway részleteinek beszerzésére és a kapcsolódó IP/DNS-név lekérésére van szükség az Application Gatewayhez csatolt PublicIPAddress használatával.</span><span class="sxs-lookup"><span data-stu-id="4e53d-206">To do this, retrieve details of the application gateway and its associated IP/DNS name using the PublicIPAddress element attached to the application gateway.</span></span> <span data-ttu-id="4e53d-207">Az Application Gateway DNS-nevének használatával létrehozhat egy CNAME rekordot, amely a két webalkalmazást erre a DNS-névre irányítja.</span><span class="sxs-lookup"><span data-stu-id="4e53d-207">The application gateway's DNS name should be used to create a CNAME record, which points the two web applications to this DNS name.</span></span> <span data-ttu-id="4e53d-208">Az A-bejegyzések használata nem javasolt, mivel a virtuális IP-cím változhat az Application Gateway újraindításakor.</span><span class="sxs-lookup"><span data-stu-id="4e53d-208">The use of A-records is not recommended since the VIP may change on restart of application gateway.</span></span>


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

## <a name="next-steps"></a><span data-ttu-id="4e53d-209">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4e53d-209">Next steps</span></span>

<span data-ttu-id="4e53d-210">Ha konfigurálni szeretne egy ILB-vel használni kívánt Application Gateway-t: [Application Gateway létrehozása belső terheléselosztóval (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="4e53d-210">If you want to configure an application gateway to use with an internal load balancer (ILB), see [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="4e53d-211">Ha további általános információra van szüksége a terheléselosztás beállításaival kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="4e53d-211">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="4e53d-212">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="4e53d-212">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="4e53d-213">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="4e53d-213">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

