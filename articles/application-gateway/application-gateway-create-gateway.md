---
title: "aaaCreate, indítsa el, vagy meglévő Alkalmazásátjáró törlése |} Microsoft Docs"
description: "Ezen a lapon útmutatás toocreate, konfigurálása, indítsa el, és az Azure Alkalmazásátjáró törlése"
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
ms.openlocfilehash: 3efef5b49880c9efdafad8b88d4bce5b749b82af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-start-or-delete-an-application-gateway-with-powershell"></a><span data-ttu-id="2090f-103">Application Gateway létrehozása, indítása vagy törlése PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="2090f-103">Create, start, or delete an application gateway with PowerShell</span></span> 

> [!div class="op_single_selector"]
> * [<span data-ttu-id="2090f-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2090f-104">Azure portal</span></span>](application-gateway-create-gateway-portal.md)
> * [<span data-ttu-id="2090f-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="2090f-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-gateway-arm.md)
> * [<span data-ttu-id="2090f-106">Klasszikus Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="2090f-106">Azure Classic PowerShell</span></span>](application-gateway-create-gateway.md)
> * [<span data-ttu-id="2090f-107">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="2090f-107">Azure Resource Manager template</span></span>](application-gateway-create-gateway-arm-template.md)
> * [<span data-ttu-id="2090f-108">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2090f-108">Azure CLI</span></span>](application-gateway-create-gateway-cli.md)

<span data-ttu-id="2090f-109">Az Azure Application Gateway egy 7. rétegbeli terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="2090f-109">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="2090f-110">Akár hello felhőalapú vagy helyszíni biztosít feladatátvételt, HTTP-kérelmek teljesítmény-útválasztási különböző kiszolgálók között.</span><span class="sxs-lookup"><span data-stu-id="2090f-110">It provides failover, performance-routing HTTP requests between different servers, whether they are on hello cloud or on-premises.</span></span> <span data-ttu-id="2090f-111">Az Application Gateway számos alkalmazáskézbesítési vezérlőszolgáltatást (ADC) biztosít, beleértve a HTTP-terheléselosztást, a cookie-alapú munkamenet-affinitást, a Secure Sockets Layer (SSL) alapú kiszervezést, az egyéni állapotteszteket, a többhelyes támogatást és még sok mást.</span><span class="sxs-lookup"><span data-stu-id="2090f-111">Application Gateway provides many Application Delivery Controller (ADC) features including HTTP load balancing, cookie-based session affinity, Secure Sockets Layer (SSL) offload, custom health probes, support for multi-site, and many others.</span></span> <span data-ttu-id="2090f-112">látogasson el a támogatott funkciók teljes listáját toofind [átjáró – áttekintés](application-gateway-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="2090f-112">toofind a complete list of supported features, visit [Application Gateway Overview](application-gateway-introduction.md)</span></span>

<span data-ttu-id="2090f-113">Ez a cikk bemutatja, hogyan hello lépéseket toocreate, konfigurálása, indítsa el és Alkalmazásátjáró törlése.</span><span class="sxs-lookup"><span data-stu-id="2090f-113">This article walks you through hello steps toocreate, configure, start, and delete an application gateway.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="2090f-114">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="2090f-114">Before you begin</span></span>

1. <span data-ttu-id="2090f-115">Webplatform-telepítő hello segítségével hello hello Azure PowerShell-parancsmagok legújabb verzióját telepítse.</span><span class="sxs-lookup"><span data-stu-id="2090f-115">Install hello latest version of hello Azure PowerShell cmdlets by using hello Web Platform Installer.</span></span> <span data-ttu-id="2090f-116">Töltse le, és telepítse hello legújabb verziót a hello **Windows PowerShell** hello szakasza [letöltési oldalon](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="2090f-116">You can download and install hello latest version from hello **Windows PowerShell** section of hello [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="2090f-117">Ha egy meglévő virtuális hálózattal rendelkezik, válasszon egy létező üres alhálózatot, vagy hozzon létre egy új alhálózatot a meglévő virtuális hálózatban kizárólag használatra hello Alkalmazásátjáró által.</span><span class="sxs-lookup"><span data-stu-id="2090f-117">If you have an existing virtual network, either select an existing empty subnet or create a new subnet in your existing virtual network solely for use by hello application gateway.</span></span> <span data-ttu-id="2090f-118">Nem telepíthet központilag hello alkalmazás átjáró tooa másik virtuális hálózatot hello erőforrások mint szeretné mögött hello Alkalmazásátjáró toodeploy kivéve, ha a vnetben társviszony-létesítés szolgál.</span><span class="sxs-lookup"><span data-stu-id="2090f-118">You cannot deploy hello application gateway tooa different virtual network than hello resources you intend toodeploy behind hello application gateway unless vnet peering is used.</span></span> <span data-ttu-id="2090f-119">több látogasson el az toolearn [Vnetben társviszony-létesítés](../virtual-network/virtual-network-peering-overview.md)</span><span class="sxs-lookup"><span data-stu-id="2090f-119">toolearn more visit [Vnet Peering](../virtual-network/virtual-network-peering-overview.md)</span></span>
3. <span data-ttu-id="2090f-120">Ellenőrizze, hogy rendelkezik-e működő virtuális hálózattal és hozzá tartozó érvényes alhálózattal.</span><span class="sxs-lookup"><span data-stu-id="2090f-120">Verify that you have a working virtual network with a valid subnet.</span></span> <span data-ttu-id="2090f-121">Győződjön meg arról, hogy egyetlen virtuális gépek vagy a felhőben történő alkalmazáshoz hello alhálózat használja.</span><span class="sxs-lookup"><span data-stu-id="2090f-121">Make sure that no virtual machines or cloud deployments are using hello subnet.</span></span> <span data-ttu-id="2090f-122">a virtuális hálózati alhálózat hello Alkalmazásátjáró önmagában kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2090f-122">hello application gateway must be by itself in a virtual network subnet.</span></span>
4. <span data-ttu-id="2090f-123">hello kiszolgálók toouse hello Alkalmazásátjáró konfigurált léteznie kell, különben a végpontok hello virtuális hálózatban vagy a nyilvános IP-cím/VIP rendelt hozzá.</span><span class="sxs-lookup"><span data-stu-id="2090f-123">hello servers that you configure toouse hello application gateway must exist or have their endpoints created either in hello virtual network or with a public IP/VIP assigned.</span></span>

## <a name="what-is-required-toocreate-an-application-gateway"></a><span data-ttu-id="2090f-124">Mi az a szükséges toocreate olyan átjárót?</span><span class="sxs-lookup"><span data-stu-id="2090f-124">What is required toocreate an application gateway?</span></span>

<span data-ttu-id="2090f-125">Hello használata esetén `New-AzureApplicationGateway` parancs toocreate hello Alkalmazásátjáró, nem történik meg ezen a ponton, és az újonnan létrehozott hello erőforrás konfigurált XML vagy a konfigurációs objektum használva.</span><span class="sxs-lookup"><span data-stu-id="2090f-125">When you use hello `New-AzureApplicationGateway` command toocreate hello application gateway, no configuration is set at this point and hello newly created resource are configured either by using XML or a configuration object.</span></span>

<span data-ttu-id="2090f-126">hello értékek a következők:</span><span class="sxs-lookup"><span data-stu-id="2090f-126">hello values are:</span></span>

* <span data-ttu-id="2090f-127">**Háttér-kiszolgálófiók készlet:** hello hello háttér-kiszolgálók IP-címek listáját.</span><span class="sxs-lookup"><span data-stu-id="2090f-127">**Back-end server pool:** hello list of IP addresses of hello back-end servers.</span></span> <span data-ttu-id="2090f-128">hello IP-címek felsorolt toohello virtuális hálózati alhálózat vagy kell tartoznia, vagy egy nyilvános IP-cím/VIP kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2090f-128">hello IP addresses listed should either belong toohello virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="2090f-129">**Háttér-kiszolgálókészlet beállításai:** Minden készletnek vannak beállításai, például port, protokoll vagy cookie-alapú affinitás.</span><span class="sxs-lookup"><span data-stu-id="2090f-129">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="2090f-130">Ezek a beállítások esetén tooa kapcsolt verem és a hello készlet alkalmazott tooall-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="2090f-130">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="2090f-131">**Előtér-port:** Ez a port nem hello nyilvános portot, amelyet a hello Alkalmazásátjáró meg van nyitva.</span><span class="sxs-lookup"><span data-stu-id="2090f-131">**Front-end port:** This port is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="2090f-132">Forgalom találatok ezt a portot, és lekérdezi átirányítja tooone hello háttér-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="2090f-132">Traffic hits this port, and then gets redirected tooone of hello back-end servers.</span></span>
* <span data-ttu-id="2090f-133">**Figyelő:** hello figyelő rendelkezik egy előtér-portot, a protokollt (Http vagy Https, ezek az értékek kis-és nagybetűket), és hello SSL tanúsítvány neve (ha az SSL beállításának-kiszervezés).</span><span class="sxs-lookup"><span data-stu-id="2090f-133">**Listener:** hello listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="2090f-134">**Szabály:** hello szabály hello figyelő és hello háttér-kiszolgálófiók alkalmazáskészlet van kötve, és azt határozza meg, mely háttér-kiszolgálófiók készlet hello forgalom irányított toowhen találatok száma a egy adott figyelő.</span><span class="sxs-lookup"><span data-stu-id="2090f-134">**Rule:** hello rule binds hello listener and hello back-end server pool and defines which back-end server pool hello traffic should be directed toowhen it hits a particular listener.</span></span>

## <a name="create-an-application-gateway"></a><span data-ttu-id="2090f-135">Application Gateway létrehozása</span><span class="sxs-lookup"><span data-stu-id="2090f-135">Create an application gateway</span></span>

<span data-ttu-id="2090f-136">Alkalmazásátjáró toocreate:</span><span class="sxs-lookup"><span data-stu-id="2090f-136">toocreate an application gateway:</span></span>

1. <span data-ttu-id="2090f-137">Egy Application Gateway erőforrás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2090f-137">Create an application gateway resource.</span></span>
2. <span data-ttu-id="2090f-138">Hozzon létre egy konfigurációs XML-fájlt vagy konfigurációs objektumot.</span><span class="sxs-lookup"><span data-stu-id="2090f-138">Create a configuration XML file or a configuration object.</span></span>
3. <span data-ttu-id="2090f-139">Az újonnan létrehozott alkalmazás átjáró erőforrás hello konfigurációs toohello véglegesítése.</span><span class="sxs-lookup"><span data-stu-id="2090f-139">Commit hello configuration toohello newly created application gateway resource.</span></span>

> [!NOTE]
> <span data-ttu-id="2090f-140">Ha egy egyéni mintavételt tooconfigure az Alkalmazásátjáró van szüksége, tekintse meg [PowerShell használatával hozzon létre olyan átjárót egyéni mintavételt](application-gateway-create-probe-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="2090f-140">If you need tooconfigure a custom probe for your application gateway, see [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-classic-ps.md).</span></span> <span data-ttu-id="2090f-141">További információért lásd: [egyéni minták és állapotfigyelés](application-gateway-probe-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2090f-141">Check out [custom probes and health monitoring](application-gateway-probe-overview.md) for more information.</span></span>

![Példaforgatókönyv][scenario]

### <a name="create-an-application-gateway-resource"></a><span data-ttu-id="2090f-143">Application Gateway erőforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="2090f-143">Create an application gateway resource</span></span>

<span data-ttu-id="2090f-144">toocreate hello átjáró használata hello `New-AzureApplicationGateway` hello értékeket cserélje le a saját, a parancsmag.</span><span class="sxs-lookup"><span data-stu-id="2090f-144">toocreate hello gateway, use hello `New-AzureApplicationGateway` cmdlet, replacing hello values with your own.</span></span> <span data-ttu-id="2090f-145">Számlázási hello átjáró nem indul el ezen a ponton.</span><span class="sxs-lookup"><span data-stu-id="2090f-145">Billing for hello gateway does not start at this point.</span></span> <span data-ttu-id="2090f-146">Számlázási egy későbbi lépésben, akkor kezdődik, amikor hello átjáró sikeresen elindult.</span><span class="sxs-lookup"><span data-stu-id="2090f-146">Billing begins in a later step, when hello gateway is successfully started.</span></span>

<span data-ttu-id="2090f-147">hello alábbi példa használatával hozza létre az Alkalmazásátjáró nevű, "testvnet1" és "alhálózat-1" nevű alhálózat virtuális hálózat:</span><span class="sxs-lookup"><span data-stu-id="2090f-147">hello following example creates an application gateway by using a virtual network called "testvnet1" and a subnet called "subnet-1":</span></span>

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

<span data-ttu-id="2090f-148">A *Description*, az *InstanceCount* és a *GatewaySize* opcionális paraméterek.</span><span class="sxs-lookup"><span data-stu-id="2090f-148">*Description*, *InstanceCount*, and *GatewaySize* are optional parameters.</span></span>

<span data-ttu-id="2090f-149">amely átjáró hello toovalidate lett létrehozva, használhatja a hello `Get-AzureApplicationGateway` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="2090f-149">toovalidate that hello gateway was created, you can use hello `Get-AzureApplicationGateway` cmdlet.</span></span>

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
> <span data-ttu-id="2090f-150">az alapértelmezett érték hello *InstanceCount* 2, maximális értéke 10.</span><span class="sxs-lookup"><span data-stu-id="2090f-150">hello default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="2090f-151">az alapértelmezett érték hello *GatewaySize* közepes.</span><span class="sxs-lookup"><span data-stu-id="2090f-151">hello default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="2090f-152">A Kicsi, Közepes és a Nagy lehetőségek közül választhat.</span><span class="sxs-lookup"><span data-stu-id="2090f-152">You can choose between Small, Medium and Large.</span></span>

<span data-ttu-id="2090f-153">*Virtualip értékek* és *DnsName* jelennek meg az üres mert hello átjáró még nem kezdődött meg.</span><span class="sxs-lookup"><span data-stu-id="2090f-153">*VirtualIPs* and *DnsName* are shown as blank because hello gateway has not started yet.</span></span> <span data-ttu-id="2090f-154">Miután hello átjáró fut. hello jön létre.</span><span class="sxs-lookup"><span data-stu-id="2090f-154">These are created once hello gateway is in hello running state.</span></span>

## <a name="configure-hello-application-gateway"></a><span data-ttu-id="2090f-155">Alkalmazásátjáró hello konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2090f-155">Configure hello application gateway</span></span>

<span data-ttu-id="2090f-156">Hello Alkalmazásátjáró XML- vagy konfiguráció-objektum használatával konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="2090f-156">You can configure hello application gateway by using XML or a configuration object.</span></span>

### <a name="configure-hello-application-gateway-by-using-xml"></a><span data-ttu-id="2090f-157">Alkalmazásátjáró hello konfigurálása XML használatával</span><span class="sxs-lookup"><span data-stu-id="2090f-157">Configure hello application gateway by using XML</span></span>

<span data-ttu-id="2090f-158">A következő példa hello egy XML-fájl tooconfigure összes alkalmazás átjáró beállításait használja, és véglegesítse azokat toohello alkalmazás átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="2090f-158">In hello following example, you use an XML file tooconfigure all application gateway settings and commit them toohello application gateway resource.</span></span>  

#### <a name="step-1"></a><span data-ttu-id="2090f-159">1. lépés</span><span class="sxs-lookup"><span data-stu-id="2090f-159">Step 1</span></span>

<span data-ttu-id="2090f-160">A következő szöveg tooNotepad hello másolja.</span><span class="sxs-lookup"><span data-stu-id="2090f-160">Copy hello following text tooNotepad.</span></span>

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

<span data-ttu-id="2090f-161">Módosíthatja a hello konfigurációs elemek hello zárójelek között hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="2090f-161">Edit hello values between hello parentheses for hello configuration items.</span></span> <span data-ttu-id="2090f-162">Bővítmény .xml hello fájl mentése.</span><span class="sxs-lookup"><span data-stu-id="2090f-162">Save hello file with extension .xml.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2090f-163">hello protokoll Http vagy Https elem kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="2090f-163">hello protocol item Http or Https is case-sensitive.</span></span>

<span data-ttu-id="2090f-164">hello a következő példa bemutatja, hogyan toouse egy konfigurációs fájl mentése hello Alkalmazásátjáró tooset.</span><span class="sxs-lookup"><span data-stu-id="2090f-164">hello following example shows how toouse a configuration file tooset up hello application gateway.</span></span> <span data-ttu-id="2090f-165">hello példa terhelés kiegyensúlyozza a nyilvános port 80-as HTTP-forgalom, és elküldi a hálózati forgalom 80-as port tooback-a befejezési két IP-címre.</span><span class="sxs-lookup"><span data-stu-id="2090f-165">hello example load balances HTTP traffic on public port 80 and sends network traffic tooback-end port 80 between two IP addresses.</span></span>

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

#### <a name="step-2"></a><span data-ttu-id="2090f-166">2. lépés</span><span class="sxs-lookup"><span data-stu-id="2090f-166">Step 2</span></span>

<span data-ttu-id="2090f-167">Következő lépésként állítsa hello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="2090f-167">Next, set hello application gateway.</span></span> <span data-ttu-id="2090f-168">Használjon hello `Set-AzureApplicationGatewayConfig` parancsmag és egy konfigurációs XML-fájlt.</span><span class="sxs-lookup"><span data-stu-id="2090f-168">Use hello `Set-AzureApplicationGatewayConfig` cmdlet with a configuration XML file.</span></span>

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"
```

### <a name="configure-hello-application-gateway-by-using-a-configuration-object"></a><span data-ttu-id="2090f-169">Hello Alkalmazásátjáró konfigurációs objektum használatával konfigurálja</span><span class="sxs-lookup"><span data-stu-id="2090f-169">Configure hello application gateway by using a configuration object</span></span>

<span data-ttu-id="2090f-170">hello a következő példa bemutatja, hogyan tooconfigure hello konfigurációs objektumok használatával az Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="2090f-170">hello following example shows how tooconfigure hello application gateway by using configuration objects.</span></span> <span data-ttu-id="2090f-171">Az összes konfigurációs elemek kell külön-külön konfigurálhatók és ezután tooan átjáró konfigurációs objektum.</span><span class="sxs-lookup"><span data-stu-id="2090f-171">All configuration items must be configured individually and then added tooan application gateway configuration object.</span></span> <span data-ttu-id="2090f-172">Hello konfigurációs objektum létrehozása után használhat hello `Set-AzureApplicationGateway` parancs toocommit hello konfigurációs toohello korábban létrehozott alkalmazás átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="2090f-172">After creating hello configuration object, you use hello `Set-AzureApplicationGateway` command toocommit hello configuration toohello previously created application gateway resource.</span></span>

> [!NOTE]
> <span data-ttu-id="2090f-173">Hozzárendelése egy érték tooeach konfigurációs objektumát, előtt kell toodeclare milyen típusú objektumot PowerShell használja a tároláshoz.</span><span class="sxs-lookup"><span data-stu-id="2090f-173">Before assigning a value tooeach configuration object, you need toodeclare what kind of object PowerShell uses for storage.</span></span> <span data-ttu-id="2090f-174">hello első sor toocreate hello elemek határozza meg, mi `Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model(object name)` szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="2090f-174">hello first line toocreate hello individual items defines what `Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model(object name)` are used.</span></span>

#### <a name="step-1"></a><span data-ttu-id="2090f-175">1. lépés</span><span class="sxs-lookup"><span data-stu-id="2090f-175">Step 1</span></span>

<span data-ttu-id="2090f-176">Hozza létre az összes egyedi konfigurációs elemet.</span><span class="sxs-lookup"><span data-stu-id="2090f-176">Create all individual configuration items.</span></span>

<span data-ttu-id="2090f-177">Hozzon létre hello előtér-IP, ahogy az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="2090f-177">Create hello front-end IP as shown in hello following example.</span></span>

```powershell
$fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
$fip.Name = "fip1"
$fip.Type = "Private"
$fip.StaticIPAddress = "10.0.0.5"
```

<span data-ttu-id="2090f-178">Hozzon létre hello előtér-port, ahogy az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="2090f-178">Create hello front-end port as shown in hello following example.</span></span>

```powershell
$fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
$fep.Name = "fep1"
$fep.Port = 80
```

<span data-ttu-id="2090f-179">Hozzon létre hello háttér-kiszolgálókészletet.</span><span class="sxs-lookup"><span data-stu-id="2090f-179">Create hello back-end server pool.</span></span>

<span data-ttu-id="2090f-180">Adja meg a hello IP-címek hozzáadott toohello háttér-kiszolgálófiók készlet hello a következő példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="2090f-180">Define hello IP addresses that are added toohello back-end server pool as shown in hello next example.</span></span>

```powershell
$servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
$servers.Add("10.0.0.1")
$servers.Add("10.0.0.2")
```

<span data-ttu-id="2090f-181">Hello $server objektum tooadd hello értékek toohello háttér-objektumot ($pool) használja.</span><span class="sxs-lookup"><span data-stu-id="2090f-181">Use hello $server object tooadd hello values toohello back-end pool object ($pool).</span></span>

```powershell
$pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
$pool.BackendServers = $servers
$pool.Name = "pool1"
```

<span data-ttu-id="2090f-182">Hozzon létre hello háttér-kiszolgálófiók készlet beállítást.</span><span class="sxs-lookup"><span data-stu-id="2090f-182">Create hello back-end server pool setting.</span></span>

```powershell
$setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
$setting.Name = "setting1"
$setting.CookieBasedAffinity = "enabled"
$setting.Port = 80
$setting.Protocol = "http"
```

<span data-ttu-id="2090f-183">Hozzon létre hello figyelőt.</span><span class="sxs-lookup"><span data-stu-id="2090f-183">Create hello listener.</span></span>

```powershell
$listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
$listener.Name = "listener1"
$listener.FrontendPort = "fep1"
$listener.FrontendIP = "fip1"
$listener.Protocol = "http"
$listener.SslCert = ""
```

<span data-ttu-id="2090f-184">Hello szabály létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2090f-184">Create hello rule.</span></span>

```powershell
$rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
$rule.Name = "rule1"
$rule.Type = "basic"
$rule.BackendHttpSettings = "setting1"
$rule.Listener = "listener1"
$rule.BackendAddressPool = "pool1"
```

#### <a name="step-2"></a><span data-ttu-id="2090f-185">2. lépés</span><span class="sxs-lookup"><span data-stu-id="2090f-185">Step 2</span></span>

<span data-ttu-id="2090f-186">Rendelje hozzá minden egyes konfigurációs elemek tooan alkalmazás átjáró konfigurációs objektumot ($appgwconfig).</span><span class="sxs-lookup"><span data-stu-id="2090f-186">Assign all individual configuration items tooan application gateway configuration object ($appgwconfig).</span></span>

<span data-ttu-id="2090f-187">Adja hozzá a hello előtér-IP-toohello konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="2090f-187">Add hello front-end IP toohello configuration.</span></span>

```powershell
$appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
$appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
$appgwconfig.FrontendIPConfigurations.Add($fip)
```

<span data-ttu-id="2090f-188">Adja hozzá a hello előtér-port toohello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="2090f-188">Add hello front-end port toohello configuration.</span></span>

```powershell
$appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
$appgwconfig.FrontendPorts.Add($fep)
```
<span data-ttu-id="2090f-189">Adja hozzá a hello háttér-kiszolgálófiók alkalmazáskészlet toohello konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="2090f-189">Add hello back-end server pool toohello configuration.</span></span>

```powershell
$appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
$appgwconfig.BackendAddressPools.Add($pool)
```

<span data-ttu-id="2090f-190">Adja hozzá a hello háttér-tárolókészlet beállítás toohello konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="2090f-190">Add hello back-end pool setting toohello configuration.</span></span>

```powershell
$appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
$appgwconfig.BackendHttpSettingsList.Add($setting)
```

<span data-ttu-id="2090f-191">Adja hozzá a hello figyelő toohello konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="2090f-191">Add hello listener toohello configuration.</span></span>

```powershell
$appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
$appgwconfig.HttpListeners.Add($listener)
```

<span data-ttu-id="2090f-192">Adja hozzá a hello szabály toohello konfigurálását.</span><span class="sxs-lookup"><span data-stu-id="2090f-192">Add hello rule toohello configuration.</span></span>

```powershell
$appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
$appgwconfig.HttpLoadBalancingRules.Add($rule)
```

### <a name="step-3"></a><span data-ttu-id="2090f-193">3. lépés</span><span class="sxs-lookup"><span data-stu-id="2090f-193">Step 3</span></span>
<span data-ttu-id="2090f-194">Hello konfigurációs objektum toohello alkalmazás átjáró erőforrás véglegesítése használatával `Set-AzureApplicationGatewayConfig`.</span><span class="sxs-lookup"><span data-stu-id="2090f-194">Commit hello configuration object toohello application gateway resource by using `Set-AzureApplicationGatewayConfig`.</span></span>

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig
```

## <a name="start-hello-gateway"></a><span data-ttu-id="2090f-195">Indítsa el a hello átjáró</span><span class="sxs-lookup"><span data-stu-id="2090f-195">Start hello gateway</span></span>

<span data-ttu-id="2090f-196">Ha hello átjáró van konfigurálva, a hello `Start-AzureApplicationGateway` parancsmag toostart hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="2090f-196">Once hello gateway has been configured, use hello `Start-AzureApplicationGateway` cmdlet toostart hello gateway.</span></span> <span data-ttu-id="2090f-197">Alkalmazásátjáró számlázás megkezdése után hello átjáró sikeresen elindult.</span><span class="sxs-lookup"><span data-stu-id="2090f-197">Billing for an application gateway begins after hello gateway has been successfully started.</span></span>

> [!NOTE]
> <span data-ttu-id="2090f-198">Hello `Start-AzureApplicationGateway` parancsmag is igénybe vehet fel toofinish too15-20 perc.</span><span class="sxs-lookup"><span data-stu-id="2090f-198">hello `Start-AzureApplicationGateway` cmdlet might take up too15-20 minutes toofinish.</span></span>

```powershell
Start-AzureApplicationGateway AppGwTest
```

## <a name="verify-hello-gateway-status"></a><span data-ttu-id="2090f-199">Hello az átjáró állapotának megerősítése</span><span class="sxs-lookup"><span data-stu-id="2090f-199">Verify hello gateway status</span></span>

<span data-ttu-id="2090f-200">Használjon hello `Get-AzureApplicationGateway` parancsmag toocheck hello hello átjáró állapotának.</span><span class="sxs-lookup"><span data-stu-id="2090f-200">Use hello `Get-AzureApplicationGateway` cmdlet toocheck hello status of hello gateway.</span></span> <span data-ttu-id="2090f-201">Ha `Start-AzureApplicationGateway` hello a korábbi lépésben sikeres *állapot* kell futnia, és *Vip* és *DnsName* érvényes bejegyzést kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="2090f-201">If `Start-AzureApplicationGateway` succeeded in hello previous step, *State* should be Running, and *Vip* and *DnsName* should have valid entries.</span></span>

<span data-ttu-id="2090f-202">hello alábbi példa azt mutatja, amely akár, nem fut, Alkalmazásátjáró, és készen áll a tootake forgalom szánt `http://<generated-dns-name>.cloudapp.net`.</span><span class="sxs-lookup"><span data-stu-id="2090f-202">hello following example shows an application gateway that is up, running, and ready tootake traffic destined for `http://<generated-dns-name>.cloudapp.net`.</span></span>

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

## <a name="delete-hello-application-gateway"></a><span data-ttu-id="2090f-203">Hello Alkalmazásátjáró törlése</span><span class="sxs-lookup"><span data-stu-id="2090f-203">Delete hello application gateway</span></span>

<span data-ttu-id="2090f-204">toodelete hello Alkalmazásátjáró:</span><span class="sxs-lookup"><span data-stu-id="2090f-204">toodelete hello application gateway:</span></span>

1. <span data-ttu-id="2090f-205">Használjon hello `Stop-AzureApplicationGateway` parancsmag toostop hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="2090f-205">Use hello `Stop-AzureApplicationGateway` cmdlet toostop hello gateway.</span></span>
2. <span data-ttu-id="2090f-206">Használjon hello `Remove-AzureApplicationGateway` parancsmag tooremove hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="2090f-206">Use hello `Remove-AzureApplicationGateway` cmdlet tooremove hello gateway.</span></span>
3. <span data-ttu-id="2090f-207">Győződjön meg arról, hogy hello átjáró el lett távolítva a hello `Get-AzureApplicationGateway` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="2090f-207">Verify that hello gateway has been removed by using hello `Get-AzureApplicationGateway` cmdlet.</span></span>

<span data-ttu-id="2090f-208">hello alábbi példa bemutatja hello `Stop-AzureApplicationGateway` hello első sor parancsmag hello kimeneti követ.</span><span class="sxs-lookup"><span data-stu-id="2090f-208">hello following example shows hello `Stop-AzureApplicationGateway` cmdlet on hello first line, followed by hello output.</span></span>

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

<span data-ttu-id="2090f-209">Ha hello Alkalmazásátjáró leállított állapotban van, a hello `Remove-AzureApplicationGateway` parancsmag tooremove hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="2090f-209">Once hello application gateway is in a stopped state, use hello `Remove-AzureApplicationGateway` cmdlet tooremove hello service.</span></span>

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

<span data-ttu-id="2090f-210">tooverify, amely hello szolgáltatás el lett távolítva, használhatja a hello `Get-AzureApplicationGateway` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="2090f-210">tooverify that hello service has been removed, you can use hello `Get-AzureApplicationGateway` cmdlet.</span></span> <span data-ttu-id="2090f-211">Ez a lépés nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="2090f-211">This step is not required.</span></span>

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

Get-AzureApplicationGateway : ResourceNotFound: hello gateway does not exist.
.....
```

## <a name="next-steps"></a><span data-ttu-id="2090f-212">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2090f-212">Next steps</span></span>

<span data-ttu-id="2090f-213">Ha azt szeretné, hogy az SSL-kiszervezés tooconfigure, [konfigurálása az SSL-kiszervezés Alkalmazásátjáró](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="2090f-213">If you want tooconfigure SSL offload, see [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="2090f-214">Ha azt szeretné, hogy egy alkalmazás átjáró toouse belső terheléselosztással tooconfigure, [hozzon létre egy alkalmazást egy belső terheléselosztón (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="2090f-214">If you want tooconfigure an application gateway toouse with an internal load balancer, see [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="2090f-215">Ha további általános információra van szüksége a terheléselosztás beállításaival kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="2090f-215">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="2090f-216">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="2090f-216">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="2090f-217">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="2090f-217">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

[scenario]: ./media/application-gateway-create-gateway/scenario.png
