---
title: "Azure Application Gateway belső terheléselosztóval aaaUsing |} Microsoft Docs"
description: "Ezen a lapon nyújt útmutatást tooconfigure egy Azure Application Gateway egy belső elosztott terhelésű végponthoz"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 7403d28e-909f-46a2-b282-43a8e942f53c
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: 272ef84a02f92a8521c35aad6f1d9f9bf1675718
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb"></a><span data-ttu-id="d99aa-103">Alkalmazásátjáró létrehozása belső Load Balancerrel (ILB)</span><span class="sxs-lookup"><span data-stu-id="d99aa-103">Create an Application Gateway with an Internal Load Balancer (ILB)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="d99aa-104">Klasszikus Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="d99aa-104">Azure Classic PowerShell</span></span>](application-gateway-ilb.md)
> * [<span data-ttu-id="d99aa-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="d99aa-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ilb-arm.md)

<span data-ttu-id="d99aa-106">Alkalmazásátjáró Internet felé néző virtuális IP-cím vagy egy belső végpont nem lesz közzétéve toohello kell konfigurálni az internet, más néven belső Load Balancer (ILB) végpont.</span><span class="sxs-lookup"><span data-stu-id="d99aa-106">Application Gateway can be configured with an internet facing virtual IP or with an internal end-point not exposed toohello internet, also known as Internal Load Balancer (ILB) endpoint.</span></span> <span data-ttu-id="d99aa-107">Belső üzleti alkalmazások közötti kapcsolat nincs felfedve toointernet hello átjáró konfigurálása egy ILB.</span><span class="sxs-lookup"><span data-stu-id="d99aa-107">Configuring hello gateway with an ILB is useful for internal line-of-business applications not exposed toointernet.</span></span> <span data-ttu-id="d99aa-108">Is célszerű a szolgáltatások vagy rétegek, egy többrétegű alkalmazást, amely az egy biztonsági határ nem lesz közzétéve toointernet helyezkedik el, de továbbra is szükséges a ciklikus multiplexelés terheléselosztási, a munkamenet tölcsérútvonalak vagy az SSL-lezárást belül.</span><span class="sxs-lookup"><span data-stu-id="d99aa-108">It's also useful for services/tiers within a multi-tier application, which sits in a security boundary not exposed toointernet, but still require round robin load distribution, session stickiness, or SSL termination.</span></span> <span data-ttu-id="d99aa-109">Ez a cikk bemutatja, hogyan hello lépéseket tooconfigure egy ILB az Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="d99aa-109">This article walks you through hello steps tooconfigure an application gateway with an ILB.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="d99aa-110">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="d99aa-110">Before you begin</span></span>

1. <span data-ttu-id="d99aa-111">Hello Azure PowerShell-parancsmaggal hello Webplatform-telepítő legújabb verziójának telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="d99aa-111">Install latest version of hello Azure PowerShell cmdlets using hello Web Platform Installer.</span></span> <span data-ttu-id="d99aa-112">Töltse le, és telepítse hello legújabb verziót a hello **Windows PowerShell** hello szakasza [letöltési oldala](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="d99aa-112">You can download and install hello latest version from hello **Windows PowerShell** section of hello [Download page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="d99aa-113">Győződjön meg arról, hogy rendelkezik-e egy működő virtuális alhálózattal rendelkező hálózatot érvényes.</span><span class="sxs-lookup"><span data-stu-id="d99aa-113">Verify that you have a working virtual network with valid subnet.</span></span>
3. <span data-ttu-id="d99aa-114">Ellenőrizze, rendelkezik háttérkiszolgálók hello virtuális hálózatban, vagy egy nyilvános IP-cím/VIP hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="d99aa-114">Verify that you have backend servers either in hello virtual network, or with a public IP/VIP assigned.</span></span>

<span data-ttu-id="d99aa-115">toocreate Alkalmazásátjáró, hajtsa végre a következő lépéseket a hello sorrendben hello.</span><span class="sxs-lookup"><span data-stu-id="d99aa-115">toocreate an application gateway, perform hello following steps in hello order listed.</span></span> 

1. [<span data-ttu-id="d99aa-116">Alkalmazásátjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="d99aa-116">Create an application gateway</span></span>](#create-a-new-application-gateway)
2. [<span data-ttu-id="d99aa-117">Hello átjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d99aa-117">Configure hello gateway</span></span>](#configure-the-gateway)
3. [<span data-ttu-id="d99aa-118">Hello átjáró konfigurációjának beállítása</span><span class="sxs-lookup"><span data-stu-id="d99aa-118">Set hello gateway configuration</span></span>](#set-the-gateway-configuration)
4. [<span data-ttu-id="d99aa-119">Indítsa el a hello átjáró</span><span class="sxs-lookup"><span data-stu-id="d99aa-119">Start hello gateway</span></span>](#start-the-gateway)
5. [<span data-ttu-id="d99aa-120">Hello átjáró ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="d99aa-120">Verify hello gateway</span></span>](#verify-the-gateway-status)

## <a name="create-an-application-gateway"></a><span data-ttu-id="d99aa-121">Alkalmazásátjáró létrehozása:</span><span class="sxs-lookup"><span data-stu-id="d99aa-121">Create an application gateway:</span></span>

<span data-ttu-id="d99aa-122">**toocreate hello átjáró**, használja a hello `New-AzureApplicationGateway` hello értékeket cserélje le a saját, a parancsmag.</span><span class="sxs-lookup"><span data-stu-id="d99aa-122">**toocreate hello gateway**, use hello `New-AzureApplicationGateway` cmdlet, replacing hello values with your own.</span></span> <span data-ttu-id="d99aa-123">Vegye figyelembe, hogy számlázással hello átjáró nem indul el ezen a ponton.</span><span class="sxs-lookup"><span data-stu-id="d99aa-123">Note that billing for hello gateway does not start at this point.</span></span> <span data-ttu-id="d99aa-124">Számlázási egy későbbi lépésben, akkor kezdődik, amikor hello átjáró sikeresen elindult.</span><span class="sxs-lookup"><span data-stu-id="d99aa-124">Billing begins in a later step, when hello gateway is successfully started.</span></span>

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

```
VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway 
VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error 
----       ----------------     ------------                             ----
Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399
```

<span data-ttu-id="d99aa-125">**toovalidate** , hogy hello átjáró lett létrehozva, használhatja a hello `Get-AzureApplicationGateway` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="d99aa-125">**toovalidate** that hello gateway was created, you can use hello `Get-AzureApplicationGateway` cmdlet.</span></span> 

<span data-ttu-id="d99aa-126">Hello mintában *leírás*, *InstanceCount*, és *GatewaySize* opcionális paraméterek.</span><span class="sxs-lookup"><span data-stu-id="d99aa-126">In hello sample, *Description*, *InstanceCount*, and *GatewaySize* are optional parameters.</span></span> <span data-ttu-id="d99aa-127">az alapértelmezett érték hello *InstanceCount* 2, maximális értéke 10.</span><span class="sxs-lookup"><span data-stu-id="d99aa-127">hello default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="d99aa-128">az alapértelmezett érték hello *GatewaySize* közepes.</span><span class="sxs-lookup"><span data-stu-id="d99aa-128">hello default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="d99aa-129">Kis és nagy más elérhető értékek.</span><span class="sxs-lookup"><span data-stu-id="d99aa-129">Small and Large are other available values.</span></span> <span data-ttu-id="d99aa-130">*VIP* és *DnsName* jelennek meg az üres mert hello átjáró még nem kezdődött meg.</span><span class="sxs-lookup"><span data-stu-id="d99aa-130">*Vip* and *DnsName* are shown as blank because hello gateway has not started yet.</span></span> <span data-ttu-id="d99aa-131">Miután hello átjáró fut. hello jön létre.</span><span class="sxs-lookup"><span data-stu-id="d99aa-131">These are created once hello gateway is in hello running state.</span></span> 

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 4:39:39 PM - Begin Operation:
Get-AzureApplicationGateway VERBOSE: 4:39:40 PM - Completed 
Operation: Get-AzureApplicationGateway
Name: AppGwTest    
Description: 
VnetName: testvnet1 
Subnets: {Subnet-1} 
InstanceCount: 2 
GatewaySize: Medium 
State: Stopped 
VirtualIPs: 
DnsName:
```

## <a name="configure-hello-gateway"></a><span data-ttu-id="d99aa-132">Hello átjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d99aa-132">Configure hello gateway</span></span>
<span data-ttu-id="d99aa-133">Egy alkalmazás átjáró konfigurálása több érték áll.</span><span class="sxs-lookup"><span data-stu-id="d99aa-133">An application gateway configuration consists of multiple values.</span></span> <span data-ttu-id="d99aa-134">hello értékeket is kötődik együtt tooconstruct hello konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="d99aa-134">hello values can be tied together tooconstruct hello configuration.</span></span>

<span data-ttu-id="d99aa-135">hello értékek a következők:</span><span class="sxs-lookup"><span data-stu-id="d99aa-135">hello values are:</span></span>

* <span data-ttu-id="d99aa-136">**Kiszolgáló háttérkészlet:** IP-címek, a háttérkiszolgálók hello hello listáját.</span><span class="sxs-lookup"><span data-stu-id="d99aa-136">**Backend server pool:** hello list of IP addresses of hello backend servers.</span></span> <span data-ttu-id="d99aa-137">hello IP-címek felsorolt vagy kell tartoznia, mint toohello VNet subnet, vagy egy nyilvános IP-cím/VIP kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d99aa-137">hello IP addresses listed should either belong toohello VNet subnet, or should be a public IP/VIP.</span></span> 
* <span data-ttu-id="d99aa-138">**Háttérkiszolgáló-készlet beállításai:** Minden készlet rendelkezik olyan beállításokkal, mint a port, a protokoll vagy a cookie-alapú affinitás.</span><span class="sxs-lookup"><span data-stu-id="d99aa-138">**Backend server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="d99aa-139">Ezek a beállítások esetén tooa kapcsolt verem és a hello készlet alkalmazott tooall-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="d99aa-139">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="d99aa-140">**Az elülső rétegbeli portot:** Ez a port nem hello nyilvános portot nyit meg hello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="d99aa-140">**Frontend Port:** This port is hello public port opened on hello application gateway.</span></span> <span data-ttu-id="d99aa-141">Forgalom találatok ezt a portot, és ezután lekérdezi a hello háttérkiszolgálók átirányított tooone.</span><span class="sxs-lookup"><span data-stu-id="d99aa-141">Traffic hits this port, and then gets redirected tooone of hello backend servers.</span></span>
* <span data-ttu-id="d99aa-142">**Figyelő:** hello figyelő rendelkezik egy elülső rétegbeli portot, a protokollt (Http vagy Https, amelyek kis-és nagybetűket), és hello SSL tanúsítvány neve (ha az SSL beállításának-kiszervezés).</span><span class="sxs-lookup"><span data-stu-id="d99aa-142">**Listener:** hello listener has a frontend port, a protocol (Http or Https, these are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span> 
* <span data-ttu-id="d99aa-143">**Szabály:** hello szabály hello figyelő és a kiszolgáló háttérkészlet hello van kötve, és azt határozza meg, melyik háttér címkészletet hello forgalom irányított toowhen találatok száma a egy adott figyelő.</span><span class="sxs-lookup"><span data-stu-id="d99aa-143">**Rule:** hello rule binds hello listener and hello backend server pool and defines which backend server pool hello traffic should be directed toowhen it hits a particular listener.</span></span> <span data-ttu-id="d99aa-144">Jelenleg csak hello *alapvető* szabály használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="d99aa-144">Currently, only hello *basic* rule is supported.</span></span> <span data-ttu-id="d99aa-145">Hello *alapvető* szabály-e időszeletelés terheléselosztási.</span><span class="sxs-lookup"><span data-stu-id="d99aa-145">hello *basic* rule is round-robin load distribution.</span></span>

<span data-ttu-id="d99aa-146">A konfigurációs hogyan hozhat létre, vagy hozzon létre egy konfigurációs objektumot, vagy egy konfigurációs XML-fájl használatával.</span><span class="sxs-lookup"><span data-stu-id="d99aa-146">You can construct your configuration either by creating a configuration object, or by using a configuration XML file.</span></span> <span data-ttu-id="d99aa-147">tooconstruct egy konfigurációs XML-fájl, használjon hello segítségével a konfigurációs minta alatt.</span><span class="sxs-lookup"><span data-stu-id="d99aa-147">tooconstruct your configuration by using a configuration XML file, use hello sample below.</span></span>

<span data-ttu-id="d99aa-148">Vegye figyelembe a következőket hello:</span><span class="sxs-lookup"><span data-stu-id="d99aa-148">Note hello following:</span></span>

* <span data-ttu-id="d99aa-149">Hello *frontendipconfiguration osztálya lehet* elem hello ILB részletek vonatkozó Alkalmazásátjáró konfigurálható egy ILB írja le.</span><span class="sxs-lookup"><span data-stu-id="d99aa-149">hello *FrontendIPConfigurations* element describes hello ILB details relevant for configuring Application Gateway with an ILB.</span></span> 
* <span data-ttu-id="d99aa-150">hello előtér-IP- *típus* too'Private állítható be "</span><span class="sxs-lookup"><span data-stu-id="d99aa-150">hello Frontend IP *Type* should be set too'Private'</span></span>
* <span data-ttu-id="d99aa-151">Hello *StaticIPAddress* kell beállítani kívánt toohello belső IP-mely hello az átjáró fogadja a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="d99aa-151">hello *StaticIPAddress* should be set toohello desired internal IP on which hello gateway receives traffic.</span></span> <span data-ttu-id="d99aa-152">Vegye figyelembe, hogy hello *StaticIPAddress* elem nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="d99aa-152">Note that hello *StaticIPAddress* element is optional.</span></span> <span data-ttu-id="d99aa-153">Ha nincs olyan elérhető belső IP-címet a telepített hello alhálózatból be van állítva, van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="d99aa-153">If not set, an available internal IP from hello deployed subnet is chosen.</span></span> 
* <span data-ttu-id="d99aa-154">hello értékének hello *neve* elemben megadott *FrontendIPConfiguration* hello HTTPListener használandó *FrontendIP* elem toorefer toohello FrontendIPConfiguration.</span><span class="sxs-lookup"><span data-stu-id="d99aa-154">hello value of hello *Name* element specified in *FrontendIPConfiguration* should be used in hello HTTPListener's *FrontendIP* element toorefer toohello FrontendIPConfiguration.</span></span>
  
  <span data-ttu-id="d99aa-155">**Konfigurációs XML-minta**</span><span class="sxs-lookup"><span data-stu-id="d99aa-155">**Configuration XML sample**</span></span>
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations>
        <FrontendIPConfiguration>
            <Name>fip1</Name> 
            <Type>Private</Type> 
            <StaticIPAddress>10.0.0.10</StaticIPAddress> 
        </FrontendIPConfiguration>
    </FrontendIPConfigurations>
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
            <FrontendIP>fip1</FrontendIP>
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


## <a name="set-hello-gateway-configuration"></a><span data-ttu-id="d99aa-156">Hello átjáró konfigurációjának beállítása</span><span class="sxs-lookup"><span data-stu-id="d99aa-156">Set hello gateway configuration</span></span>
<span data-ttu-id="d99aa-157">A következő beállításokat a hello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="d99aa-157">Next, you'll set hello application gateway.</span></span> <span data-ttu-id="d99aa-158">Használhatja a hello `Set-AzureApplicationGatewayConfig` parancsmag egy konfigurációs objektumot, vagy egy konfigurációs XML-fájlt.</span><span class="sxs-lookup"><span data-stu-id="d99aa-158">You can use hello `Set-AzureApplicationGatewayConfig` cmdlet with a configuration object, or with a configuration XML file.</span></span> 

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml
```

```
VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig 
VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
Name       HTTP Status Code     Operation ID                             Error 
----       ----------------     ------------                             ----
Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d
```

## <a name="start-hello-gateway"></a><span data-ttu-id="d99aa-159">Indítsa el a hello átjáró</span><span class="sxs-lookup"><span data-stu-id="d99aa-159">Start hello gateway</span></span>

<span data-ttu-id="d99aa-160">Ha hello átjáró van konfigurálva, a hello `Start-AzureApplicationGateway` parancsmag toostart hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="d99aa-160">Once hello gateway has been configured, use hello `Start-AzureApplicationGateway` cmdlet toostart hello gateway.</span></span> <span data-ttu-id="d99aa-161">Alkalmazásátjáró számlázás megkezdése után hello átjáró sikeresen elindult.</span><span class="sxs-lookup"><span data-stu-id="d99aa-161">Billing for an application gateway begins after hello gateway has been successfully started.</span></span> 

> [!NOTE]
> <span data-ttu-id="d99aa-162">Hello `Start-AzureApplicationGateway` parancsmag is igénybe vehet fel toocomplete too15-20 perc.</span><span class="sxs-lookup"><span data-stu-id="d99aa-162">hello `Start-AzureApplicationGateway` cmdlet might take up too15-20 minutes toocomplete.</span></span> 
> 
> 

```powershell
Start-AzureApplicationGateway AppGwTest 
```

```
VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway 
VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error 
----       ----------------     ------------                             ----
Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b
```

## <a name="verify-hello-gateway-status"></a><span data-ttu-id="d99aa-163">Hello az átjáró állapotának megerősítése</span><span class="sxs-lookup"><span data-stu-id="d99aa-163">Verify hello gateway status</span></span>

<span data-ttu-id="d99aa-164">Használjon hello `Get-AzureApplicationGateway` parancsmag toocheck hello átjáró állapotának.</span><span class="sxs-lookup"><span data-stu-id="d99aa-164">Use hello `Get-AzureApplicationGateway` cmdlet toocheck hello status of gateway.</span></span> <span data-ttu-id="d99aa-165">Ha `Start-AzureApplicationGateway` hello állapotban kell lennie sikeres hello előző lépésben, *futtató*, és a hello Vip DnsName kell állnia, és érvényes bejegyzések.</span><span class="sxs-lookup"><span data-stu-id="d99aa-165">If `Start-AzureApplicationGateway` succeeded in hello previous step, hello State should be *Running*, and hello Vip and DnsName should have valid entries.</span></span> <span data-ttu-id="d99aa-166">Ez a példa bemutatja hello parancsmag hello első sor hello kimeneti követi.</span><span class="sxs-lookup"><span data-stu-id="d99aa-166">This sample shows hello cmdlet on hello first line, followed by hello output.</span></span> <span data-ttu-id="d99aa-167">Ez a példa hello átjáró fut, és készen áll a tootake forgalom.</span><span class="sxs-lookup"><span data-stu-id="d99aa-167">In this sample, hello gateway is running, and is ready tootake traffic.</span></span> 

> [!NOTE]
> <span data-ttu-id="d99aa-168">hello Alkalmazásátjáró van konfigurálva tooaccept-forgalom zárása a hello 10.0.0.10 ILB végpontja konfigurált ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="d99aa-168">hello application gateway is configured tooaccept traffic at hello configured ILB endpoint of 10.0.0.10 in this example.</span></span>

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
VirtualIPs    : {10.0.0.10}
DnsName       : appgw-b2a11563-2b3a-4172-a4aa-226ee4c23eed.cloudapp.net
```

## <a name="next-steps"></a><span data-ttu-id="d99aa-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d99aa-169">Next steps</span></span>
<span data-ttu-id="d99aa-170">Ha további általános információra van szüksége a terheléselosztás beállításaival kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="d99aa-170">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="d99aa-171">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="d99aa-171">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="d99aa-172">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="d99aa-172">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

