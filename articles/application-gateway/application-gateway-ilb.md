---
title: "Az Azure alkalmazás átjáró használata belső elosztott terhelésű |} Microsoft Docs"
description: "Ezen a lapon egy Azure-alkalmazás átjáró konfigurálása egy belső elosztott terhelésű végpont utasításokat tartalmazza."
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
ms.openlocfilehash: d6f3af61934c8c645be1f2c6b4c056fc7ee2e3aa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb"></a><span data-ttu-id="6c506-103">Alkalmazásátjáró létrehozása belső Load Balancerrel (ILB)</span><span class="sxs-lookup"><span data-stu-id="6c506-103">Create an Application Gateway with an Internal Load Balancer (ILB)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="6c506-104">Klasszikus Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="6c506-104">Azure Classic PowerShell</span></span>](application-gateway-ilb.md)
> * [<span data-ttu-id="6c506-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="6c506-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ilb-arm.md)

<span data-ttu-id="6c506-106">Alkalmazásátjáró Internet felé néző virtuális IP-cím vagy egy belső végpont nem kommunikál az internettel, más néven belső Load Balancer (ILB) végpont is kell konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="6c506-106">Application Gateway can be configured with an internet facing virtual IP or with an internal end-point not exposed to the internet, also known as Internal Load Balancer (ILB) endpoint.</span></span> <span data-ttu-id="6c506-107">Az átjáró konfigurálása egy ILB akkor hasznos, a belső üzleti alkalmazások nem kommunikál az internettel.</span><span class="sxs-lookup"><span data-stu-id="6c506-107">Configuring the gateway with an ILB is useful for internal line-of-business applications not exposed to internet.</span></span> <span data-ttu-id="6c506-108">Is célszerű a szolgáltatások vagy rétegek, egy többrétegű alkalmazást, amely a nem kommunikál az internettel biztonsági határ helyezkedik el, de továbbra is szükséges a ciklikus multiplexelés terheléselosztási, a munkamenet tölcsérútvonalak vagy az SSL-lezárást belül.</span><span class="sxs-lookup"><span data-stu-id="6c506-108">It's also useful for services/tiers within a multi-tier application, which sits in a security boundary not exposed to internet, but still require round robin load distribution, session stickiness, or SSL termination.</span></span> <span data-ttu-id="6c506-109">Ez a cikk részletesen ismerteti egy Application Gateway ILB-hez történő konfigurálásának lépéseit.</span><span class="sxs-lookup"><span data-stu-id="6c506-109">This article walks you through the steps to configure an application gateway with an ILB.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="6c506-110">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="6c506-110">Before you begin</span></span>

1. <span data-ttu-id="6c506-111">Telepítse az Azure PowerShell-parancsmagok a Webplatform-telepítővel történő legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="6c506-111">Install latest version of the Azure PowerShell cmdlets using the Web Platform Installer.</span></span> <span data-ttu-id="6c506-112">Töltse le, és telepítse a legújabb verziót a **Windows PowerShell** szakasza a [letöltési oldala](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="6c506-112">You can download and install the latest version from the **Windows PowerShell** section of the [Download page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="6c506-113">Győződjön meg arról, hogy rendelkezik-e egy működő virtuális alhálózattal rendelkező hálózatot érvényes.</span><span class="sxs-lookup"><span data-stu-id="6c506-113">Verify that you have a working virtual network with valid subnet.</span></span>
3. <span data-ttu-id="6c506-114">Ellenőrizze, a virtuális hálózaton, vagy egy nyilvános IP-cím/VIP hozzárendelve a háttérkiszolgálók rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="6c506-114">Verify that you have backend servers either in the virtual network, or with a public IP/VIP assigned.</span></span>

<span data-ttu-id="6c506-115">Alkalmazásátjáró létrehozásához a megadott sorrendben hajtsa végre az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="6c506-115">To create an application gateway, perform the following steps in the order listed.</span></span> 

1. [<span data-ttu-id="6c506-116">Alkalmazásátjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="6c506-116">Create an application gateway</span></span>](#create-a-new-application-gateway)
2. [<span data-ttu-id="6c506-117">Az átjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6c506-117">Configure the gateway</span></span>](#configure-the-gateway)
3. [<span data-ttu-id="6c506-118">Állítsa be az átjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6c506-118">Set the gateway configuration</span></span>](#set-the-gateway-configuration)
4. [<span data-ttu-id="6c506-119">Az átjáró elindítása</span><span class="sxs-lookup"><span data-stu-id="6c506-119">Start the gateway</span></span>](#start-the-gateway)
5. [<span data-ttu-id="6c506-120">Az átjáró ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="6c506-120">Verify the gateway</span></span>](#verify-the-gateway-status)

## <a name="create-an-application-gateway"></a><span data-ttu-id="6c506-121">Alkalmazásátjáró létrehozása:</span><span class="sxs-lookup"><span data-stu-id="6c506-121">Create an application gateway:</span></span>

<span data-ttu-id="6c506-122">**Az átjáró létrehozása**, használja a `New-AzureApplicationGateway` parancsmag, az értékeket a saját cserélje le.</span><span class="sxs-lookup"><span data-stu-id="6c506-122">**To create the gateway**, use the `New-AzureApplicationGateway` cmdlet, replacing the values with your own.</span></span> <span data-ttu-id="6c506-123">Ne feledje, hogy az átjáró használati díjának felszámolása nem indul el ezen a ponton.</span><span class="sxs-lookup"><span data-stu-id="6c506-123">Note that billing for the gateway does not start at this point.</span></span> <span data-ttu-id="6c506-124">A használati díj felszámolása egy későbbi lépésnél kezdődik, amikor az átjáró sikeresen elindul.</span><span class="sxs-lookup"><span data-stu-id="6c506-124">Billing begins in a later step, when the gateway is successfully started.</span></span>

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

<span data-ttu-id="6c506-125">**Érvényesítéséhez** , hogy az átjáró lett létrehozva, használja a `Get-AzureApplicationGateway` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="6c506-125">**To validate** that the gateway was created, you can use the `Get-AzureApplicationGateway` cmdlet.</span></span> 

<span data-ttu-id="6c506-126">A minta *leírás*, *InstanceCount*, és *GatewaySize* opcionális paraméterek.</span><span class="sxs-lookup"><span data-stu-id="6c506-126">In the sample, *Description*, *InstanceCount*, and *GatewaySize* are optional parameters.</span></span> <span data-ttu-id="6c506-127">Az *InstanceCount* alapértelmezett értéke 2, a maximális értéke pedig 10.</span><span class="sxs-lookup"><span data-stu-id="6c506-127">The default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="6c506-128">A *GatewaySize* alapértelmezett értéke Közepes.</span><span class="sxs-lookup"><span data-stu-id="6c506-128">The default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="6c506-129">Kis és nagy más elérhető értékek.</span><span class="sxs-lookup"><span data-stu-id="6c506-129">Small and Large are other available values.</span></span> <span data-ttu-id="6c506-130">*VIP* és *DnsName* jelennek meg az üres, mert az átjáró még nem kezdődött meg.</span><span class="sxs-lookup"><span data-stu-id="6c506-130">*Vip* and *DnsName* are shown as blank because the gateway has not started yet.</span></span> <span data-ttu-id="6c506-131">Ezek kitöltése akkor történik, amikor az átjáró futó állapotba kerül.</span><span class="sxs-lookup"><span data-stu-id="6c506-131">These are created once the gateway is in the running state.</span></span> 

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

## <a name="configure-the-gateway"></a><span data-ttu-id="6c506-132">Az átjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6c506-132">Configure the gateway</span></span>
<span data-ttu-id="6c506-133">Egy alkalmazás átjáró konfigurálása több érték áll.</span><span class="sxs-lookup"><span data-stu-id="6c506-133">An application gateway configuration consists of multiple values.</span></span> <span data-ttu-id="6c506-134">Az értékek is kötődik együtt a konfiguráció létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="6c506-134">The values can be tied together to construct the configuration.</span></span>

<span data-ttu-id="6c506-135">Az értékek a következők:</span><span class="sxs-lookup"><span data-stu-id="6c506-135">The values are:</span></span>

* <span data-ttu-id="6c506-136">**Kiszolgáló háttérkészlet:** e a háttérkiszolgálók IP-címek listáját.</span><span class="sxs-lookup"><span data-stu-id="6c506-136">**Backend server pool:** The list of IP addresses of the backend servers.</span></span> <span data-ttu-id="6c506-137">A felsorolt IP-címek vagy a virtuális hálózat alhálózathoz kell tartoznia, vagy egy nyilvános IP-cím/VIP kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6c506-137">The IP addresses listed should either belong to the VNet subnet, or should be a public IP/VIP.</span></span> 
* <span data-ttu-id="6c506-138">**Háttérkiszolgáló-készlet beállításai:** Minden készlet rendelkezik olyan beállításokkal, mint a port, a protokoll vagy a cookie-alapú affinitás.</span><span class="sxs-lookup"><span data-stu-id="6c506-138">**Backend server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="6c506-139">Ezek a beállítások egy adott készlethez kapcsolódnak, és a készlet minden kiszolgálójára érvényesek.</span><span class="sxs-lookup"><span data-stu-id="6c506-139">These settings are tied to a pool and are applied to all servers within the pool.</span></span>
* <span data-ttu-id="6c506-140">**Az elülső rétegbeli portot:** Ez a port nem a nyilvános portot nyit meg az Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="6c506-140">**Frontend Port:** This port is the public port opened on the application gateway.</span></span> <span data-ttu-id="6c506-141">Amikor a forgalom eléri ezt a portot, a port átirányítja az egyik háttérkiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="6c506-141">Traffic hits this port, and then gets redirected to one of the backend servers.</span></span>
* <span data-ttu-id="6c506-142">**Figyelő:** a figyelő rendelkezik egy elülső rétegbeli portot, a protokollt (Http vagy Https, amelyek kis-és nagybetűket), és az SSL-tanúsítvány neve (ha az SSL beállításának-kiszervezés).</span><span class="sxs-lookup"><span data-stu-id="6c506-142">**Listener:** The listener has a frontend port, a protocol (Http or Https, these are case-sensitive), and the SSL certificate name (if configuring SSL offload).</span></span> 
* <span data-ttu-id="6c506-143">**Szabály:** a szabály a figyelő és a kiszolgáló háttérkészlet van kötve, és határozza meg, melyik kiszolgáló háttérkészlet a forgalom legyenek irányítva, ha a találatok száma a egy adott figyelő.</span><span class="sxs-lookup"><span data-stu-id="6c506-143">**Rule:** The rule binds the listener and the backend server pool and defines which backend server pool the traffic should be directed to when it hits a particular listener.</span></span> <span data-ttu-id="6c506-144">Jelenleg csak a *basic* szabály támogatott.</span><span class="sxs-lookup"><span data-stu-id="6c506-144">Currently, only the *basic* rule is supported.</span></span> <span data-ttu-id="6c506-145">A *basic* szabály a ciklikus időszeleteléses terheléselosztás.</span><span class="sxs-lookup"><span data-stu-id="6c506-145">The *basic* rule is round-robin load distribution.</span></span>

<span data-ttu-id="6c506-146">A konfigurációs hogyan hozhat létre, vagy hozzon létre egy konfigurációs objektumot, vagy egy konfigurációs XML-fájl használatával.</span><span class="sxs-lookup"><span data-stu-id="6c506-146">You can construct your configuration either by creating a configuration object, or by using a configuration XML file.</span></span> <span data-ttu-id="6c506-147">A konfiguráció létrehozásához egy konfigurációs XML-fájl használatával, használja az alábbi minta.</span><span class="sxs-lookup"><span data-stu-id="6c506-147">To construct your configuration by using a configuration XML file, use the sample below.</span></span>

<span data-ttu-id="6c506-148">Vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="6c506-148">Note the following:</span></span>

* <span data-ttu-id="6c506-149">A *frontendipconfiguration osztálya lehet* elem Alkalmazásátjáró konfigurálható egy ILB vonatkozó ILB részleteit ismerteti.</span><span class="sxs-lookup"><span data-stu-id="6c506-149">The *FrontendIPConfigurations* element describes the ILB details relevant for configuring Application Gateway with an ILB.</span></span> 
* <span data-ttu-id="6c506-150">Az előtér-IP- *típus* "Private" értékre kell állítani</span><span class="sxs-lookup"><span data-stu-id="6c506-150">The Frontend IP *Type* should be set to 'Private'</span></span>
* <span data-ttu-id="6c506-151">A *StaticIPAddress* kell beállítani a kívánt belső IP-, amelyen az átjáró fogadja a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="6c506-151">The *StaticIPAddress* should be set to the desired internal IP on which the gateway receives traffic.</span></span> <span data-ttu-id="6c506-152">Vegye figyelembe, hogy a *StaticIPAddress* elem nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="6c506-152">Note that the *StaticIPAddress* element is optional.</span></span> <span data-ttu-id="6c506-153">Ha nincs olyan elérhető belső IP-címet a telepített alhálózatból be van állítva, van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="6c506-153">If not set, an available internal IP from the deployed subnet is chosen.</span></span> 
* <span data-ttu-id="6c506-154">Értékét a *neve* elemben megadott *FrontendIPConfiguration* a HTTPListener használandó *FrontendIP* tekintse meg a FrontendIPConfiguration elemet.</span><span class="sxs-lookup"><span data-stu-id="6c506-154">The value of the *Name* element specified in *FrontendIPConfiguration* should be used in the HTTPListener's *FrontendIP* element to refer to the FrontendIPConfiguration.</span></span>
  
  <span data-ttu-id="6c506-155">**Konfigurációs XML-minta**</span><span class="sxs-lookup"><span data-stu-id="6c506-155">**Configuration XML sample**</span></span>
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


## <a name="set-the-gateway-configuration"></a><span data-ttu-id="6c506-156">Állítsa be az átjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6c506-156">Set the gateway configuration</span></span>
<span data-ttu-id="6c506-157">A következő lesznek állítva az Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="6c506-157">Next, you'll set the application gateway.</span></span> <span data-ttu-id="6c506-158">Használhatja a `Set-AzureApplicationGatewayConfig` parancsmag egy konfigurációs objektumot, vagy egy konfigurációs XML-fájlt.</span><span class="sxs-lookup"><span data-stu-id="6c506-158">You can use the `Set-AzureApplicationGatewayConfig` cmdlet with a configuration object, or with a configuration XML file.</span></span> 

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

## <a name="start-the-gateway"></a><span data-ttu-id="6c506-159">Az átjáró indítása</span><span class="sxs-lookup"><span data-stu-id="6c506-159">Start the gateway</span></span>

<span data-ttu-id="6c506-160">Az átjáró konfigurálása után indítsa el az átjárót a `Start-AzureApplicationGateway` parancsmaggal.</span><span class="sxs-lookup"><span data-stu-id="6c506-160">Once the gateway has been configured, use the `Start-AzureApplicationGateway` cmdlet to start the gateway.</span></span> <span data-ttu-id="6c506-161">Az Application Gateway használati díjának felszámolása az átjáró sikeres indítása után kezdődik.</span><span class="sxs-lookup"><span data-stu-id="6c506-161">Billing for an application gateway begins after the gateway has been successfully started.</span></span> 

> [!NOTE]
> <span data-ttu-id="6c506-162">A `Start-AzureApplicationGateway` parancsmag előfordulhat, hogy akár 15-20 percig tarthat.</span><span class="sxs-lookup"><span data-stu-id="6c506-162">The `Start-AzureApplicationGateway` cmdlet might take up to 15-20 minutes to complete.</span></span> 
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

## <a name="verify-the-gateway-status"></a><span data-ttu-id="6c506-163">Az átjáró állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="6c506-163">Verify the gateway status</span></span>

<span data-ttu-id="6c506-164">Használja a `Get-AzureApplicationGateway` parancsmag az átjáró állapotának ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="6c506-164">Use the `Get-AzureApplicationGateway` cmdlet to check the status of gateway.</span></span> <span data-ttu-id="6c506-165">Ha `Start-AzureApplicationGateway` sikeres volt az előző lépésben, az állapot kell *futtató*, és a Vip és DnsName érvényes bejegyzést kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="6c506-165">If `Start-AzureApplicationGateway` succeeded in the previous step, the State should be *Running*, and the Vip and DnsName should have valid entries.</span></span> <span data-ttu-id="6c506-166">Ez a példa bemutatja a parancsmag az első sorba a kimeneti követ.</span><span class="sxs-lookup"><span data-stu-id="6c506-166">This sample shows the cmdlet on the first line, followed by the output.</span></span> <span data-ttu-id="6c506-167">Ez a példa az átjáró fut, és készen áll a forgalom igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="6c506-167">In this sample, the gateway is running, and is ready to take traffic.</span></span> 

> [!NOTE]
> <span data-ttu-id="6c506-168">Az Alkalmazásátjáró konfigurálva van a konfigurált ILB végpont 10.0.0.10 ebben a példában a forgalom fogadására.</span><span class="sxs-lookup"><span data-stu-id="6c506-168">The application gateway is configured to accept traffic at the configured ILB endpoint of 10.0.0.10 in this example.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="6c506-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6c506-169">Next steps</span></span>
<span data-ttu-id="6c506-170">Ha további általános információra van szüksége a terheléselosztás beállításaival kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="6c506-170">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="6c506-171">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="6c506-171">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="6c506-172">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="6c506-172">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

