---
title: "Application Gateway létrehozása, indítása vagy törlése | Microsoft Docs"
description: "Ez a lap utasításokat tartalmaz egy Azure Application Gateway létrehozásához, konfigurálásához, indításához és törléséhez"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 577054ca-8368-4fbf-8d53-a813f29dc3bc
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: c4932096229b1941e0966e7f3e97de39c6931392
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-start-or-delete-an-application-gateway-with-powershell"></a><span data-ttu-id="12b94-103">Application Gateway létrehozása, indítása vagy törlése PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="12b94-103">Create, start, or delete an application gateway with PowerShell</span></span> 

> [!div class="op_single_selector"]
> * [<span data-ttu-id="12b94-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="12b94-104">Azure portal</span></span>](application-gateway-create-gateway-portal.md)
> * [<span data-ttu-id="12b94-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="12b94-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-gateway-arm.md)
> * [<span data-ttu-id="12b94-106">Klasszikus Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="12b94-106">Azure Classic PowerShell</span></span>](application-gateway-create-gateway.md)
> * [<span data-ttu-id="12b94-107">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="12b94-107">Azure Resource Manager template</span></span>](application-gateway-create-gateway-arm-template.md)
> * [<span data-ttu-id="12b94-108">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="12b94-108">Azure CLI</span></span>](application-gateway-create-gateway-cli.md)

<span data-ttu-id="12b94-109">Az Azure Application Gateway egy 7. rétegbeli terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="12b94-109">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="12b94-110">Feladatátvételt és teljesítményalapú útválasztást biztosít a HTTP-kérelmek számára különböző kiszolgálók között, függetlenül attól, hogy a felhőben vagy a helyszínen vannak.</span><span class="sxs-lookup"><span data-stu-id="12b94-110">It provides failover, performance-routing HTTP requests between different servers, whether they are on the cloud or on-premises.</span></span> <span data-ttu-id="12b94-111">Az Application Gateway számos alkalmazáskézbesítési vezérlőszolgáltatást (ADC) biztosít, beleértve a HTTP-terheléselosztást, a cookie-alapú munkamenet-affinitást, a Secure Sockets Layer (SSL) alapú kiszervezést, az egyéni állapotteszteket, a többhelyes támogatást és még sok mást.</span><span class="sxs-lookup"><span data-stu-id="12b94-111">Application Gateway provides many Application Delivery Controller (ADC) features including HTTP load balancing, cookie-based session affinity, Secure Sockets Layer (SSL) offload, custom health probes, support for multi-site, and many others.</span></span> <span data-ttu-id="12b94-112">A támogatott szolgáltatások teljes listájáért látogasson el [Az Application Gateway áttekintése](application-gateway-introduction.md) című oldalra</span><span class="sxs-lookup"><span data-stu-id="12b94-112">To find a complete list of supported features, visit [Application Gateway Overview](application-gateway-introduction.md)</span></span>

<span data-ttu-id="12b94-113">Ez a cikk részletesen ismerteti a lépéseket, amelyekkel létrehozhat, konfigurálhat, elindíthat és törölhet egy Application Gateway-t.</span><span class="sxs-lookup"><span data-stu-id="12b94-113">This article walks you through the steps to create, configure, start, and delete an application gateway.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="12b94-114">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="12b94-114">Before you begin</span></span>

1. <span data-ttu-id="12b94-115">Telepítse az Azure PowerShell-parancsmagok legújabb verzióját a Webplatform-telepítővel.</span><span class="sxs-lookup"><span data-stu-id="12b94-115">Install the latest version of the Azure PowerShell cmdlets by using the Web Platform Installer.</span></span> <span data-ttu-id="12b94-116">A [Letöltések lap](https://azure.microsoft.com/downloads/) **Windows PowerShell** szakaszából letöltheti és telepítheti a legújabb verziót.</span><span class="sxs-lookup"><span data-stu-id="12b94-116">You can download and install the latest version from the **Windows PowerShell** section of the [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="12b94-117">Ha rendelkezik meglévő virtuális hálózattal, válasszon ki egy meglévő üres alhálózatot, vagy hozzon létre egy új alhálózatot a meglévő virtuális hálózatban kizárólag az Application Gateway számára.</span><span class="sxs-lookup"><span data-stu-id="12b94-117">If you have an existing virtual network, either select an existing empty subnet or create a new subnet in your existing virtual network solely for use by the application gateway.</span></span> <span data-ttu-id="12b94-118">Nem helyezheti üzembe az Application Gateway szolgáltatást az Application Gateway mögött üzembe helyezni kívánt erőforrásoktól eltérő virtuális hálózaton, kivéve ha a virtuális hálózatok között társviszony van.</span><span class="sxs-lookup"><span data-stu-id="12b94-118">You cannot deploy the application gateway to a different virtual network than the resources you intend to deploy behind the application gateway unless vnet peering is used.</span></span> <span data-ttu-id="12b94-119">További információért tekintse meg a [Társviszony létesítése virtuális hálózatok között](../virtual-network/virtual-network-peering-overview.md) című cikket</span><span class="sxs-lookup"><span data-stu-id="12b94-119">To learn more visit [Vnet Peering](../virtual-network/virtual-network-peering-overview.md)</span></span>
3. <span data-ttu-id="12b94-120">Ellenőrizze, hogy rendelkezik-e működő virtuális hálózattal és hozzá tartozó érvényes alhálózattal.</span><span class="sxs-lookup"><span data-stu-id="12b94-120">Verify that you have a working virtual network with a valid subnet.</span></span> <span data-ttu-id="12b94-121">Győződjön meg arról, hogy egy virtuális gép vagy felhőalapú telepítés sem használja az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="12b94-121">Make sure that no virtual machines or cloud deployments are using the subnet.</span></span> <span data-ttu-id="12b94-122">Az Application Gateway-nek egyedül kell lennie a virtuális hálózat alhálózatán.</span><span class="sxs-lookup"><span data-stu-id="12b94-122">The application gateway must be by itself in a virtual network subnet.</span></span>
4. <span data-ttu-id="12b94-123">A kiszolgálóknak, amelyeket az Application Gateway használatára konfigurál, már létezniük kell, illetve a virtuális hálózatban vagy hozzárendelt nyilvános/virtuális IP-címmel létrehozott végpontokkal kell rendelkezniük.</span><span class="sxs-lookup"><span data-stu-id="12b94-123">The servers that you configure to use the application gateway must exist or have their endpoints created either in the virtual network or with a public IP/VIP assigned.</span></span>

## <a name="what-is-required-to-create-an-application-gateway"></a><span data-ttu-id="12b94-124">Mire van szükség egy Application Gateway létrehozásához?</span><span class="sxs-lookup"><span data-stu-id="12b94-124">What is required to create an application gateway?</span></span>

<span data-ttu-id="12b94-125">Amikor a `New-AzureApplicationGateway` parancsot használja az Application Gateway létrehozására, a konfigurációk még nincsenek beállítva, és az újonnan létrehozott erőforrást konfigurálni kell egy XML-fájl vagy egy konfigurációs objektum használatával.</span><span class="sxs-lookup"><span data-stu-id="12b94-125">When you use the `New-AzureApplicationGateway` command to create the application gateway, no configuration is set at this point and the newly created resource are configured either by using XML or a configuration object.</span></span>

<span data-ttu-id="12b94-126">Az értékek a következők:</span><span class="sxs-lookup"><span data-stu-id="12b94-126">The values are:</span></span>

* <span data-ttu-id="12b94-127">**Háttér-kiszolgálókészlet:** A háttérkiszolgálók IP-címeinek listája.</span><span class="sxs-lookup"><span data-stu-id="12b94-127">**Back-end server pool:** The list of IP addresses of the back-end servers.</span></span> <span data-ttu-id="12b94-128">A listán szereplő IP-címeknek a virtuális hálózat alhálózatához kell tartozniuk, vagy nyilvános/virtuális IP-címnek kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="12b94-128">The IP addresses listed should either belong to the virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="12b94-129">**Háttér-kiszolgálókészlet beállításai:** Minden készletnek vannak beállításai, például port, protokoll vagy cookie-alapú affinitás.</span><span class="sxs-lookup"><span data-stu-id="12b94-129">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="12b94-130">Ezek a beállítások egy adott készlethez kapcsolódnak, és a készlet minden kiszolgálójára érvényesek.</span><span class="sxs-lookup"><span data-stu-id="12b94-130">These settings are tied to a pool and are applied to all servers within the pool.</span></span>
* <span data-ttu-id="12b94-131">**Előtérbeli port:** Az Application Gateway-en megnyitott nyilvános port.</span><span class="sxs-lookup"><span data-stu-id="12b94-131">**Front-end port:** This port is the public port that is opened on the application gateway.</span></span> <span data-ttu-id="12b94-132">Amikor a forgalom eléri ezt a portot, a port átirányítja az egyik háttérkiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="12b94-132">Traffic hits this port, and then gets redirected to one of the back-end servers.</span></span>
* <span data-ttu-id="12b94-133">**Figyelő:** A figyelő egy előtérbeli porttal, egy protokollal (Http vagy Https, a kis- és a nagybetűk megkülönböztetésével) és SSL tanúsítványnévvel rendelkezik (SSL-kiszervezés konfigurálásakor).</span><span class="sxs-lookup"><span data-stu-id="12b94-133">**Listener:** The listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and the SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="12b94-134">**Szabály:** A szabály összeköti a figyelőt és a háttérkiszolgáló-készletet, és meghatározza, hogy mely háttérkiszolgáló-készletre legyen átirányítva a forgalom, ha elér egy adott figyelőt.</span><span class="sxs-lookup"><span data-stu-id="12b94-134">**Rule:** The rule binds the listener and the back-end server pool and defines which back-end server pool the traffic should be directed to when it hits a particular listener.</span></span>

## <a name="create-an-application-gateway"></a><span data-ttu-id="12b94-135">Application Gateway létrehozása</span><span class="sxs-lookup"><span data-stu-id="12b94-135">Create an application gateway</span></span>

<span data-ttu-id="12b94-136">Application Gateway létrehozásához tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="12b94-136">To create an application gateway:</span></span>

1. <span data-ttu-id="12b94-137">Egy Application Gateway erőforrás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="12b94-137">Create an application gateway resource.</span></span>
2. <span data-ttu-id="12b94-138">Hozzon létre egy konfigurációs XML-fájlt vagy konfigurációs objektumot.</span><span class="sxs-lookup"><span data-stu-id="12b94-138">Create a configuration XML file or a configuration object.</span></span>
3. <span data-ttu-id="12b94-139">Véglegesítse az újonnan létrehozott Application Gateway erőforrás konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="12b94-139">Commit the configuration to the newly created application gateway resource.</span></span>

> [!NOTE]
> <span data-ttu-id="12b94-140">Ha egy egyéni mintát kell konfigurálnia az Application Gateway számára: [Application Gateway létrehozása egyéni mintákkal a PowerShell használatával](application-gateway-create-probe-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="12b94-140">If you need to configure a custom probe for your application gateway, see [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-classic-ps.md).</span></span> <span data-ttu-id="12b94-141">További információért lásd: [egyéni minták és állapotfigyelés](application-gateway-probe-overview.md).</span><span class="sxs-lookup"><span data-stu-id="12b94-141">Check out [custom probes and health monitoring](application-gateway-probe-overview.md) for more information.</span></span>

![Példaforgatókönyv][scenario]

### <a name="create-an-application-gateway-resource"></a><span data-ttu-id="12b94-143">Application Gateway erőforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="12b94-143">Create an application gateway resource</span></span>

<span data-ttu-id="12b94-144">Az átjáró létrehozásához használja a `New-AzureApplicationGateway` parancsmagot, és cserélje le az értékeket a saját értékeire.</span><span class="sxs-lookup"><span data-stu-id="12b94-144">To create the gateway, use the `New-AzureApplicationGateway` cmdlet, replacing the values with your own.</span></span> <span data-ttu-id="12b94-145">Az átjáró használati díjának felszámolása ekkor még nem kezdődik el.</span><span class="sxs-lookup"><span data-stu-id="12b94-145">Billing for the gateway does not start at this point.</span></span> <span data-ttu-id="12b94-146">A használati díj felszámolása egy későbbi lépésnél kezdődik, amikor az átjáró sikeresen elindul.</span><span class="sxs-lookup"><span data-stu-id="12b94-146">Billing begins in a later step, when the gateway is successfully started.</span></span>

<span data-ttu-id="12b94-147">Az alábbi példa egy új Application Gatewayt hoz létre egy „testvnet1” nevű virtuális hálózat és egy „subnet-1” nevű alhálózat használatával.</span><span class="sxs-lookup"><span data-stu-id="12b94-147">The following example creates an application gateway by using a virtual network called "testvnet1" and a subnet called "subnet-1":</span></span>

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

<span data-ttu-id="12b94-148">A *Description*, az *InstanceCount* és a *GatewaySize* opcionális paraméterek.</span><span class="sxs-lookup"><span data-stu-id="12b94-148">*Description*, *InstanceCount*, and *GatewaySize* are optional parameters.</span></span>

<span data-ttu-id="12b94-149">Az átjáró létrehozásának ellenőrzéséhez használhatja a `Get-AzureApplicationGateway` parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="12b94-149">To validate that the gateway was created, you can use the `Get-AzureApplicationGateway` cmdlet.</span></span>

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
Name          : AppGwTest
Description   :
VnetName      : testvnet1
Subnets       : {Subnet-1}
InstanceCount : 2
GatewaySize   : Medium
State         : Stopped
VirtualIPs    : {}
DnsName       :
```

> [!NOTE]
> <span data-ttu-id="12b94-150">Az *InstanceCount* alapértelmezett értéke 2, a maximális értéke pedig 10.</span><span class="sxs-lookup"><span data-stu-id="12b94-150">The default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="12b94-151">A *GatewaySize* alapértelmezett értéke Közepes.</span><span class="sxs-lookup"><span data-stu-id="12b94-151">The default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="12b94-152">A Kicsi, Közepes és a Nagy lehetőségek közül választhat.</span><span class="sxs-lookup"><span data-stu-id="12b94-152">You can choose between Small, Medium and Large.</span></span>

<span data-ttu-id="12b94-153">A *VirtualIPs* és a *DnsName* paraméterek azért üresek, mert az átjáró még nem indult el.</span><span class="sxs-lookup"><span data-stu-id="12b94-153">*VirtualIPs* and *DnsName* are shown as blank because the gateway has not started yet.</span></span> <span data-ttu-id="12b94-154">Ezek kitöltése akkor történik, amikor az átjáró futó állapotba kerül.</span><span class="sxs-lookup"><span data-stu-id="12b94-154">These are created once the gateway is in the running state.</span></span>

## <a name="configure-the-application-gateway"></a><span data-ttu-id="12b94-155">Az Application Gateway konfigurálása</span><span class="sxs-lookup"><span data-stu-id="12b94-155">Configure the application gateway</span></span>

<span data-ttu-id="12b94-156">Az Application Gateway-t egy XML-fájl vagy konfigurációs objektum segítségével konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="12b94-156">You can configure the application gateway by using XML or a configuration object.</span></span>

### <a name="configure-the-application-gateway-by-using-xml"></a><span data-ttu-id="12b94-157">Az Application Gateway konfigurálása XML-fájl használatával</span><span class="sxs-lookup"><span data-stu-id="12b94-157">Configure the application gateway by using XML</span></span>

<span data-ttu-id="12b94-158">Az alábbi példában egy XML-fájllal konfigurálja az Application Gateway beállításait, és véglegesíti őket az Application Gateway-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="12b94-158">In the following example, you use an XML file to configure all application gateway settings and commit them to the application gateway resource.</span></span>  

#### <a name="step-1"></a><span data-ttu-id="12b94-159">1. lépés</span><span class="sxs-lookup"><span data-stu-id="12b94-159">Step 1</span></span>

<span data-ttu-id="12b94-160">Másolja az alábbi szöveget a Jegyzettömbbe.</span><span class="sxs-lookup"><span data-stu-id="12b94-160">Copy the following text to Notepad.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendPorts>
        <FrontendPort>
            <Name>(name-of-your-frontend-port)</Name>
            <Port>(port number)</Port>
        </FrontendPort>
    </FrontendPorts>
    <BackendAddressPools>
        <BackendAddressPool>
            <Name>(name-of-your-backend-pool)</Name>
            <IPAddresses>
                <IPAddress>(your-IP-address-for-backend-pool)</IPAddress>
                <IPAddress>(your-second-IP-address-for-backend-pool)</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>(backend-setting-name-to-configure-rule)</Name>
            <Port>80</Port>
            <Protocol>[Http|Https]</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>(name-of-the-listener)</Name>
            <FrontendPort>(name-of-your-frontend-port)</FrontendPort>
            <Protocol>[Http|Https]</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>(name-of-load-balancing-rule)</Name>
            <Type>basic</Type>
            <BackendHttpSettings>(backend-setting-name-to-configure-rule)</BackendHttpSettings>
            <Listener>(name-of-the-listener)</Listener>
            <BackendAddressPool>(name-of-your-backend-pool)</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

<span data-ttu-id="12b94-161">Szerkessze a zárójelek közötti értékeket a konfigurációs elemeknek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="12b94-161">Edit the values between the parentheses for the configuration items.</span></span> <span data-ttu-id="12b94-162">Mentse a fájlt .xml kiterjesztéssel.</span><span class="sxs-lookup"><span data-stu-id="12b94-162">Save the file with extension .xml.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="12b94-163">A Http és Https protokollelem különbséget tesz a kis- és a nagybetűk között.</span><span class="sxs-lookup"><span data-stu-id="12b94-163">The protocol item Http or Https is case-sensitive.</span></span>

<span data-ttu-id="12b94-164">Az alábbi példa bemutatja, hogyan használhat egy konfigurációs fájlt az Application Gateway beállítására.</span><span class="sxs-lookup"><span data-stu-id="12b94-164">The following example shows how to use a configuration file to set up the application gateway.</span></span> <span data-ttu-id="12b94-165">A példa elosztja a nyilvános 80-as port HTTP-forgalmának a terhelését, illetve a háttérbeli 80-as portra küldi a két IP-cím közötti hálózati forgalmat.</span><span class="sxs-lookup"><span data-stu-id="12b94-165">The example load balances HTTP traffic on public port 80 and sends network traffic to back-end port 80 between two IP addresses.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendPorts>
        <FrontendPort>
            <Name>FrontendPort1</Name>
            <Port>80</Port>
        </FrontendPort>
    </FrontendPorts>
    <BackendAddressPools>
        <BackendAddressPool>
            <Name>BackendPool1</Name>
            <IPAddresses>
                <IPAddress>10.0.0.1</IPAddress>
                <IPAddress>10.0.0.2</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>BackendSetting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>HTTPListener1</Name>
            <FrontendPort>FrontendPort1</FrontendPort>
            <Protocol>Http</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>HttpLBRule1</Name>
            <Type>basic</Type>
            <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
            <Listener>HTTPListener1</Listener>
            <BackendAddressPool>BackendPool1</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

#### <a name="step-2"></a><span data-ttu-id="12b94-166">2. lépés</span><span class="sxs-lookup"><span data-stu-id="12b94-166">Step 2</span></span>

<span data-ttu-id="12b94-167">A következő lépésként állítsa be az Application Gateway-t.</span><span class="sxs-lookup"><span data-stu-id="12b94-167">Next, set the application gateway.</span></span> <span data-ttu-id="12b94-168">Használja a `Set-AzureApplicationGatewayConfig` parancsmagot egy konfigurációs XML-fájllal.</span><span class="sxs-lookup"><span data-stu-id="12b94-168">Use the `Set-AzureApplicationGatewayConfig` cmdlet with a configuration XML file.</span></span>

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"
```

### <a name="configure-the-application-gateway-by-using-a-configuration-object"></a><span data-ttu-id="12b94-169">Az Application Gateway konfigurálása konfigurációs objektum segítségével</span><span class="sxs-lookup"><span data-stu-id="12b94-169">Configure the application gateway by using a configuration object</span></span>

<span data-ttu-id="12b94-170">Az alábbi példa bemutatja, hogyan konfigurálhatja az Application Gateway-t konfigurációs objektumok segítségével.</span><span class="sxs-lookup"><span data-stu-id="12b94-170">The following example shows how to configure the application gateway by using configuration objects.</span></span> <span data-ttu-id="12b94-171">Minden konfigurációs elemet külön kell konfigurálni, és utána kell hozzáadni egy Application Gateway konfigurációs objektumhoz.</span><span class="sxs-lookup"><span data-stu-id="12b94-171">All configuration items must be configured individually and then added to an application gateway configuration object.</span></span> <span data-ttu-id="12b94-172">A konfigurációs objektum létrehozása után a `Set-AzureApplicationGateway` paranccsal véglegesíti a konfigurációt a korábban létrehozott Application Gateway-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="12b94-172">After creating the configuration object, you use the `Set-AzureApplicationGateway` command to commit the configuration to the previously created application gateway resource.</span></span>

> [!NOTE]
> <span data-ttu-id="12b94-173">Mielőtt értékeket rendelne a konfigurációs objektumokhoz, deklarálnia kell, hogy a PowerShell milyen típusú objektumot használ a tároláshoz.</span><span class="sxs-lookup"><span data-stu-id="12b94-173">Before assigning a value to each configuration object, you need to declare what kind of object PowerShell uses for storage.</span></span> <span data-ttu-id="12b94-174">Az egyéni elemek létrehozásának első sora határozza meg, hogy milyen `Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model(object name)` elemet használ a rendszer.</span><span class="sxs-lookup"><span data-stu-id="12b94-174">The first line to create the individual items defines what `Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model(object name)` are used.</span></span>

#### <a name="step-1"></a><span data-ttu-id="12b94-175">1. lépés</span><span class="sxs-lookup"><span data-stu-id="12b94-175">Step 1</span></span>

<span data-ttu-id="12b94-176">Hozza létre az összes egyedi konfigurációs elemet.</span><span class="sxs-lookup"><span data-stu-id="12b94-176">Create all individual configuration items.</span></span>

<span data-ttu-id="12b94-177">Hozza létre az előtérbeli IP-címet az alábbi példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="12b94-177">Create the front-end IP as shown in the following example.</span></span>

```powershell
$fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
$fip.Name = "fip1"
$fip.Type = "Private"
$fip.StaticIPAddress = "10.0.0.5"
```

<span data-ttu-id="12b94-178">Hozza létre az előtérbeli portot az alábbi példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="12b94-178">Create the front-end port as shown in the following example.</span></span>

```powershell
$fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
$fep.Name = "fep1"
$fep.Port = 80
```

<span data-ttu-id="12b94-179">Hozza létre a háttér-kiszolgálókészletet.</span><span class="sxs-lookup"><span data-stu-id="12b94-179">Create the back-end server pool.</span></span>

<span data-ttu-id="12b94-180">Határozza meg a háttérkiszolgáló-készlethez hozzáadni kívánt IP-címeket a következő példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="12b94-180">Define the IP addresses that are added to the back-end server pool as shown in the next example.</span></span>

```powershell
$servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
$servers.Add("10.0.0.1")
$servers.Add("10.0.0.2")
```

<span data-ttu-id="12b94-181">Adja hozzá az értékeket a háttérkészlet objektumhoz ($pool) a $server objektum segítségével.</span><span class="sxs-lookup"><span data-stu-id="12b94-181">Use the $server object to add the values to the back-end pool object ($pool).</span></span>

```powershell
$pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
$pool.BackendServers = $servers
$pool.Name = "pool1"
```

<span data-ttu-id="12b94-182">Hozza létre a háttér-kiszolgálókészlet beállítást.</span><span class="sxs-lookup"><span data-stu-id="12b94-182">Create the back-end server pool setting.</span></span>

```powershell
$setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
$setting.Name = "setting1"
$setting.CookieBasedAffinity = "enabled"
$setting.Port = 80
$setting.Protocol = "http"
```

<span data-ttu-id="12b94-183">Hozza létre a figyelőt.</span><span class="sxs-lookup"><span data-stu-id="12b94-183">Create the listener.</span></span>

```powershell
$listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
$listener.Name = "listener1"
$listener.FrontendPort = "fep1"
$listener.FrontendIP = "fip1"
$listener.Protocol = "http"
$listener.SslCert = ""
```

<span data-ttu-id="12b94-184">Hozza létre a szabályt.</span><span class="sxs-lookup"><span data-stu-id="12b94-184">Create the rule.</span></span>

```powershell
$rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
$rule.Name = "rule1"
$rule.Type = "basic"
$rule.BackendHttpSettings = "setting1"
$rule.Listener = "listener1"
$rule.BackendAddressPool = "pool1"
```

#### <a name="step-2"></a><span data-ttu-id="12b94-185">2. lépés</span><span class="sxs-lookup"><span data-stu-id="12b94-185">Step 2</span></span>

<span data-ttu-id="12b94-186">Rendelje hozzá az összes egyéni konfigurációs elemeket egy Application Gateway konfigurációs objektumhoz ($appgwconfig).</span><span class="sxs-lookup"><span data-stu-id="12b94-186">Assign all individual configuration items to an application gateway configuration object ($appgwconfig).</span></span>

<span data-ttu-id="12b94-187">Adja hozzá az előtérbeli IP-címet a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="12b94-187">Add the front-end IP to the configuration.</span></span>

```powershell
$appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
$appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
$appgwconfig.FrontendIPConfigurations.Add($fip)
```

<span data-ttu-id="12b94-188">Adja hozzá az előtérbeli portot a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="12b94-188">Add the front-end port to the configuration.</span></span>

```powershell
$appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
$appgwconfig.FrontendPorts.Add($fep)
```
<span data-ttu-id="12b94-189">Adja hozzá a háttér-kiszolgálókészletet a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="12b94-189">Add the back-end server pool to the configuration.</span></span>

```powershell
$appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
$appgwconfig.BackendAddressPools.Add($pool)
```

<span data-ttu-id="12b94-190">Adja hozzá a háttérkészlet beállításait a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="12b94-190">Add the back-end pool setting to the configuration.</span></span>

```powershell
$appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
$appgwconfig.BackendHttpSettingsList.Add($setting)
```

<span data-ttu-id="12b94-191">Adja hozzá a figyelőt a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="12b94-191">Add the listener to the configuration.</span></span>

```powershell
$appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
$appgwconfig.HttpListeners.Add($listener)
```

<span data-ttu-id="12b94-192">Adja hozzá a szabályt a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="12b94-192">Add the rule to the configuration.</span></span>

```powershell
$appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
$appgwconfig.HttpLoadBalancingRules.Add($rule)
```

### <a name="step-3"></a><span data-ttu-id="12b94-193">3. lépés</span><span class="sxs-lookup"><span data-stu-id="12b94-193">Step 3</span></span>
<span data-ttu-id="12b94-194">Véglegesítse a konfigurációs objektumot az Application Gateway-erőforráshoz a `Set-AzureApplicationGatewayConfig` paranccsal.</span><span class="sxs-lookup"><span data-stu-id="12b94-194">Commit the configuration object to the application gateway resource by using `Set-AzureApplicationGatewayConfig`.</span></span>

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig
```

## <a name="start-the-gateway"></a><span data-ttu-id="12b94-195">Az átjáró indítása</span><span class="sxs-lookup"><span data-stu-id="12b94-195">Start the gateway</span></span>

<span data-ttu-id="12b94-196">Az átjáró konfigurálása után indítsa el az átjárót a `Start-AzureApplicationGateway` parancsmaggal.</span><span class="sxs-lookup"><span data-stu-id="12b94-196">Once the gateway has been configured, use the `Start-AzureApplicationGateway` cmdlet to start the gateway.</span></span> <span data-ttu-id="12b94-197">Az Application Gateway használati díjának felszámolása az átjáró sikeres indítása után kezdődik.</span><span class="sxs-lookup"><span data-stu-id="12b94-197">Billing for an application gateway begins after the gateway has been successfully started.</span></span>

> [!NOTE]
> <span data-ttu-id="12b94-198">A `Start-AzureApplicationGateway` parancsmag futtatása akár 15–20 percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="12b94-198">The `Start-AzureApplicationGateway` cmdlet might take up to 15-20 minutes to finish.</span></span>

```powershell
Start-AzureApplicationGateway AppGwTest
```

## <a name="verify-the-gateway-status"></a><span data-ttu-id="12b94-199">Az átjáró állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="12b94-199">Verify the gateway status</span></span>

<span data-ttu-id="12b94-200">Ellenőrizze az átjáró állapotát a `Get-AzureApplicationGateway` parancsmaggal.</span><span class="sxs-lookup"><span data-stu-id="12b94-200">Use the `Get-AzureApplicationGateway` cmdlet to check the status of the gateway.</span></span> <span data-ttu-id="12b94-201">Ha a `Start-AzureApplicationGateway` sikeres volt az előző lépésben, a *State* paraméternél a „Running” (Fut) állapotnak kell szerepelnie, a *Vip* és a *DnsName* paraméternek pedig érvényes bejegyzéssel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="12b94-201">If `Start-AzureApplicationGateway` succeeded in the previous step, *State* should be Running, and *Vip* and *DnsName* should have valid entries.</span></span>

<span data-ttu-id="12b94-202">Az alábbi példa egy működő Application Gateway-t mutat, amely kész fogadni a `http://<generated-dns-name>.cloudapp.net` felé tartó forgalmat.</span><span class="sxs-lookup"><span data-stu-id="12b94-202">The following example shows an application gateway that is up, running, and ready to take traffic destined for `http://<generated-dns-name>.cloudapp.net`.</span></span>

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway
VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
Name          : AppGwTest
Description   :
VnetName      : testvnet1
Subnets       : {Subnet-1}
InstanceCount : 2
GatewaySize   : Medium
State         : Running
Vip           : 138.91.170.26
DnsName       : appgw-1b8402e8-3e0d-428d-b661-289c16c82101.cloudapp.net
```

## <a name="delete-the-application-gateway"></a><span data-ttu-id="12b94-203">Application Gateway törlése</span><span class="sxs-lookup"><span data-stu-id="12b94-203">Delete the application gateway</span></span>

<span data-ttu-id="12b94-204">Az Application Gateway törlése:</span><span class="sxs-lookup"><span data-stu-id="12b94-204">To delete the application gateway:</span></span>

1. <span data-ttu-id="12b94-205">Állítsa le az átjárót a `Stop-AzureApplicationGateway` parancsmaggal.</span><span class="sxs-lookup"><span data-stu-id="12b94-205">Use the `Stop-AzureApplicationGateway` cmdlet to stop the gateway.</span></span>
2. <span data-ttu-id="12b94-206">Távolítsa el az átjárót a `Remove-AzureApplicationGateway` parancsmaggal.</span><span class="sxs-lookup"><span data-stu-id="12b94-206">Use the `Remove-AzureApplicationGateway` cmdlet to remove the gateway.</span></span>
3. <span data-ttu-id="12b94-207">A `Get-AzureApplicationGateway` parancsmaggal győződjön meg arról, hogy az átjáró el lett távolítva.</span><span class="sxs-lookup"><span data-stu-id="12b94-207">Verify that the gateway has been removed by using the `Get-AzureApplicationGateway` cmdlet.</span></span>

<span data-ttu-id="12b94-208">Az alábbi példa a `Stop-AzureApplicationGateway` parancsmag első sorát mutatja, amelyet a kimenet követ.</span><span class="sxs-lookup"><span data-stu-id="12b94-208">The following example shows the `Stop-AzureApplicationGateway` cmdlet on the first line, followed by the output.</span></span>

```powershell
Stop-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8
```

<span data-ttu-id="12b94-209">Amint az Application Gateway leállított állapotba kerül, távolítsa el a szolgáltatást a `Remove-AzureApplicationGateway` parancsmaggal.</span><span class="sxs-lookup"><span data-stu-id="12b94-209">Once the application gateway is in a stopped state, use the `Remove-AzureApplicationGateway` cmdlet to remove the service.</span></span>

```powershell
Remove-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301
```

<span data-ttu-id="12b94-210">A szolgáltatás eltávolításának ellenőrzéséhez használhatja a `Get-AzureApplicationGateway` parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="12b94-210">To verify that the service has been removed, you can use the `Get-AzureApplicationGateway` cmdlet.</span></span> <span data-ttu-id="12b94-211">Ez a lépés nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="12b94-211">This step is not required.</span></span>

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
.....
```

## <a name="next-steps"></a><span data-ttu-id="12b94-212">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="12b94-212">Next steps</span></span>

<span data-ttu-id="12b94-213">Ha SSL-alapú kiszervezést szeretne konfigurálni: [Application Gateway konfigurálása SSL-alapú kiszervezéshez](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="12b94-213">If you want to configure SSL offload, see [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="12b94-214">Ha konfigurálni szeretne egy ILB-vel használni kívánt Application Gateway-t: [Application Gateway létrehozása belső terheléselosztóval (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="12b94-214">If you want to configure an application gateway to use with an internal load balancer, see [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="12b94-215">Ha további általános információra van szüksége a terheléselosztás beállításaival kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="12b94-215">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="12b94-216">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="12b94-216">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="12b94-217">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="12b94-217">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

[scenario]: ./media/application-gateway-create-gateway/scenario.png
