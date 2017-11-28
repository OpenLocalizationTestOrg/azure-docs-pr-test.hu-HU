---
title: "aaaConfigure SSL - Azure Application Gateway - PowerShell klasszikus kiürítési |} Microsoft Docs"
description: "Ez a cikk útmutatást Alkalmazásátjáró SSL használatával kiszervezése toocreate hello Azure klasszikus üzembe helyezési modellben."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 63f28d96-9c47-410e-97dd-f5ca1ad1b8a4
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: 5cb128015747ed4b71802cf751c80b60634601a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-hello-classic-deployment-model"></a><span data-ttu-id="4b4cf-103">SSL kiszervezési Alkalmazásátjáró konfigurálása hello klasszikus telepítési modell segítségével</span><span class="sxs-lookup"><span data-stu-id="4b4cf-103">Configure an application gateway for SSL offload by using hello classic deployment model</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="4b4cf-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4b4cf-104">Azure portal</span></span>](application-gateway-ssl-portal.md)
> * [<span data-ttu-id="4b4cf-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="4b4cf-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ssl-arm.md)
> * [<span data-ttu-id="4b4cf-106">Klasszikus Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="4b4cf-106">Azure Classic PowerShell</span></span>](application-gateway-ssl.md)
> * [<span data-ttu-id="4b4cf-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="4b4cf-107">Azure CLI 2.0</span></span>](application-gateway-ssl-cli.md)

<span data-ttu-id="4b4cf-108">Az Azure Application Gateway konfigurált tooterminate hello Secure Sockets Layer (SSL) munkamenet: hello átjáró tooavoid költséges SSL visszafejtési feladatok toohappen: hello webfarm lehet.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-108">Azure Application Gateway can be configured tooterminate hello Secure Sockets Layer (SSL) session at hello gateway tooavoid costly SSL decryption tasks toohappen at hello web farm.</span></span> <span data-ttu-id="4b4cf-109">SSL kiszervezési is egyszerűbbé teszi a hello előtér-kiszolgáló beállítása és felügyelete hello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-109">SSL offload also simplifies hello front-end server setup and management of hello web application.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="4b4cf-110">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="4b4cf-110">Before you begin</span></span>

1. <span data-ttu-id="4b4cf-111">Webplatform-telepítő hello segítségével hello hello Azure PowerShell-parancsmagok legújabb verzióját telepítse.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-111">Install hello latest version of hello Azure PowerShell cmdlets by using hello Web Platform Installer.</span></span> <span data-ttu-id="4b4cf-112">Töltse le, és telepítse hello legújabb verziót a hello **Windows PowerShell** hello szakasza [letöltési oldalon](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="4b4cf-112">You can download and install hello latest version from hello **Windows PowerShell** section of hello [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="4b4cf-113">Ellenőrizze, hogy rendelkezik-e működő virtuális hálózattal és hozzá tartozó érvényes alhálózattal.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-113">Verify that you have a working virtual network with a valid subnet.</span></span> <span data-ttu-id="4b4cf-114">Győződjön meg arról, hogy egyetlen virtuális gépek vagy a felhőben történő alkalmazáshoz hello alhálózat használja.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-114">Make sure that no virtual machines or cloud deployments are using hello subnet.</span></span> <span data-ttu-id="4b4cf-115">a virtuális hálózati alhálózat hello Alkalmazásátjáró önmagában kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-115">hello application gateway must be by itself in a virtual network subnet.</span></span>
3. <span data-ttu-id="4b4cf-116">hello kiszolgálók toouse hello Alkalmazásátjáró konfigurált léteznie kell, különben a végpontok hello virtuális hálózatban vagy a nyilvános IP-cím/VIP rendelt hozzá.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-116">hello servers that you configure toouse hello application gateway must exist or have their endpoints created either in hello virtual network or with a public IP/VIP assigned.</span></span>

<span data-ttu-id="4b4cf-117">tooconfigure SSL Alkalmazásátjáró kiszervezésére, hello a következő lépéseket a hello sorrendben:</span><span class="sxs-lookup"><span data-stu-id="4b4cf-117">tooconfigure SSL offload on an application gateway, do hello following steps in hello order listed:</span></span>

1. [<span data-ttu-id="4b4cf-118">Alkalmazásátjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="4b4cf-118">Create an application gateway</span></span>](#create-an-application-gateway)
2. [<span data-ttu-id="4b4cf-119">SSL-tanúsítványok feltöltése</span><span class="sxs-lookup"><span data-stu-id="4b4cf-119">Upload SSL certificates</span></span>](#upload-ssl-certificates)
3. [<span data-ttu-id="4b4cf-120">Hello átjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4b4cf-120">Configure hello gateway</span></span>](#configure-the-gateway)
4. [<span data-ttu-id="4b4cf-121">Hello átjáró konfigurációjának beállítása</span><span class="sxs-lookup"><span data-stu-id="4b4cf-121">Set hello gateway configuration</span></span>](#set-the-gateway-configuration)
5. [<span data-ttu-id="4b4cf-122">Indítsa el a hello átjáró</span><span class="sxs-lookup"><span data-stu-id="4b4cf-122">Start hello gateway</span></span>](#start-the-gateway)
6. [<span data-ttu-id="4b4cf-123">Hello az átjáró állapotának megerősítése</span><span class="sxs-lookup"><span data-stu-id="4b4cf-123">Verify hello gateway status</span></span>](#verify-the-gateway-status)

## <a name="create-an-application-gateway"></a><span data-ttu-id="4b4cf-124">Application Gateway létrehozása</span><span class="sxs-lookup"><span data-stu-id="4b4cf-124">Create an application gateway</span></span>

<span data-ttu-id="4b4cf-125">toocreate hello átjáró használata hello `New-AzureApplicationGateway` hello értékeket cserélje le a saját, a parancsmag.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-125">toocreate hello gateway, use hello `New-AzureApplicationGateway` cmdlet, replacing hello values with your own.</span></span> <span data-ttu-id="4b4cf-126">Számlázási hello átjáró nem indul el ezen a ponton.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-126">Billing for hello gateway does not start at this point.</span></span> <span data-ttu-id="4b4cf-127">Számlázási egy későbbi lépésben, akkor kezdődik, amikor hello átjáró sikeresen elindult.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-127">Billing begins in a later step, when hello gateway is successfully started.</span></span>

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

<span data-ttu-id="4b4cf-128">amely átjáró hello toovalidate lett létrehozva, használhatja a hello `Get-AzureApplicationGateway` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-128">toovalidate that hello gateway was created, you can use hello `Get-AzureApplicationGateway` cmdlet.</span></span>

<span data-ttu-id="4b4cf-129">Hello mintában *leírás*, *InstanceCount*, és *GatewaySize* opcionális paraméterek.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-129">In hello sample, *Description*, *InstanceCount*, and *GatewaySize* are optional parameters.</span></span> <span data-ttu-id="4b4cf-130">az alapértelmezett érték hello *InstanceCount* 2, maximális értéke 10.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-130">hello default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="4b4cf-131">az alapértelmezett érték hello *GatewaySize* közepes.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-131">hello default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="4b4cf-132">Kis és nagy más elérhető értékek.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-132">Small and Large are other available values.</span></span> <span data-ttu-id="4b4cf-133">*Virtualip értékek* és *DnsName* jelennek meg az üres mert hello átjáró még nem kezdődött meg.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-133">*VirtualIPs* and *DnsName* are shown as blank because hello gateway has not started yet.</span></span> <span data-ttu-id="4b4cf-134">Ezeket az értékeket jönnek létre, ha hello átjáró hello futó állapotban van.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-134">These values are created once hello gateway is in hello running state.</span></span>

```powershell
Get-AzureApplicationGateway AppGwTest
```

## <a name="upload-ssl-certificates"></a><span data-ttu-id="4b4cf-135">SSL-tanúsítványok feltöltése</span><span class="sxs-lookup"><span data-stu-id="4b4cf-135">Upload SSL certificates</span></span>

<span data-ttu-id="4b4cf-136">Használjon `Add-AzureApplicationGatewaySslCertificate` tooupload hello kiszolgálói tanúsítvány *pfx* formátum toohello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-136">Use `Add-AzureApplicationGatewaySslCertificate` tooupload hello server certificate in *pfx* format toohello application gateway.</span></span> <span data-ttu-id="4b4cf-137">hello tanúsítvány neve egy felhasználó által választott név és hello Alkalmazásátjáró belül egyedieknek kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-137">hello certificate name is a user-chosen name and must be unique within hello application gateway.</span></span> <span data-ttu-id="4b4cf-138">Ez a tanúsítvány az említett tooby ezt a nevet az összes hello Alkalmazásátjáró a felügyeleti műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-138">This certificate is referred tooby this name in all certificate management operations on hello application gateway.</span></span>

<span data-ttu-id="4b4cf-139">Ez a következő példa bemutatja hello parancsmag, cserélje le hello mintában hello értékeket a saját.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-139">This following sample shows hello cmdlet, replace hello values in hello sample with your own.</span></span>

```powershell
Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path toopfx file>
```

<span data-ttu-id="4b4cf-140">A következő érvényesítse hello tanúsítvány feltöltése.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-140">Next, validate hello certificate upload.</span></span> <span data-ttu-id="4b4cf-141">Használjon hello `Get-AzureApplicationGatewayCertificate` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-141">Use hello `Get-AzureApplicationGatewayCertificate` cmdlet.</span></span>

<span data-ttu-id="4b4cf-142">Ez a példa bemutatja hello parancsmag hello első sor hello kimeneti követi.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-142">This sample shows hello cmdlet on hello first line, followed by hello output.</span></span>

```powershell
Get-AzureApplicationGatewaySslCertificate AppGwTest
```

```
VERBOSE: 5:07:54 PM - Begin Operation: Get-AzureApplicationGatewaySslCertificate
VERBOSE: 5:07:55 PM - Completed Operation: Get-AzureApplicationGatewaySslCertificate
Name           : SslCert
SubjectName    : CN=gwcert.app.test.contoso.com
Thumbprint     : AF5ADD77E160A01A6......EE48D1A
ThumbprintAlgo : sha1RSA
State..........: Provisioned
```

> [!NOTE]
> <span data-ttu-id="4b4cf-143">hello tanúsítvány jelszava toobe között 4 too12 karakterek, betűket és számokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-143">hello certificate password has toobe between 4 too12 characters, letters, or numbers.</span></span> <span data-ttu-id="4b4cf-144">Speciális karakterek használata nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-144">Special characters are not accepted.</span></span>

## <a name="configure-hello-gateway"></a><span data-ttu-id="4b4cf-145">Hello átjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4b4cf-145">Configure hello gateway</span></span>

<span data-ttu-id="4b4cf-146">Egy alkalmazás átjáró konfigurálása több érték áll.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-146">An application gateway configuration consists of multiple values.</span></span> <span data-ttu-id="4b4cf-147">hello értékeket is kötődik együtt tooconstruct hello konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-147">hello values can be tied together tooconstruct hello configuration.</span></span>

<span data-ttu-id="4b4cf-148">hello értékek a következők:</span><span class="sxs-lookup"><span data-stu-id="4b4cf-148">hello values are:</span></span>

* <span data-ttu-id="4b4cf-149">**Háttér-kiszolgálófiók készlet:** hello hello háttér-kiszolgálók IP-címek listáját.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-149">**Back-end server pool:** hello list of IP addresses of hello back-end servers.</span></span> <span data-ttu-id="4b4cf-150">hello IP-címek felsorolt toohello virtuális hálózati alhálózat vagy kell tartoznia, vagy egy nyilvános IP-cím/VIP kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-150">hello IP addresses listed should either belong toohello virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="4b4cf-151">**Háttér-kiszolgálókészlet beállításai:** Minden készletnek vannak beállításai, például port, protokoll vagy cookie-alapú affinitás.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-151">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="4b4cf-152">Ezek a beállítások esetén tooa kapcsolt verem és a hello készlet alkalmazott tooall-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-152">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="4b4cf-153">**Előtér-port:** Ez a port nem hello nyilvános portot, amelyet a hello Alkalmazásátjáró meg van nyitva.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-153">**Front-end port:** This port is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="4b4cf-154">Forgalom találatok ezt a portot, és lekérdezi átirányítja tooone hello háttér-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-154">Traffic hits this port, and then gets redirected tooone of hello back-end servers.</span></span>
* <span data-ttu-id="4b4cf-155">**Figyelő:** hello figyelő rendelkezik egy előtér-portot, a protokollt (Http vagy Https, ezek az értékek kis-és nagybetűket), és hello SSL tanúsítvány neve (ha az SSL beállításának-kiszervezés).</span><span class="sxs-lookup"><span data-stu-id="4b4cf-155">**Listener:** hello listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="4b4cf-156">**Szabály:** hello szabály hello figyelő és hello háttér-kiszolgálófiók alkalmazáskészlet van kötve, és azt határozza meg, mely háttér-kiszolgálófiók készlet hello forgalom irányított toowhen találatok száma a egy adott figyelő.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-156">**Rule:** hello rule binds hello listener and hello back-end server pool and defines which back-end server pool hello traffic should be directed toowhen it hits a particular listener.</span></span> <span data-ttu-id="4b4cf-157">Jelenleg csak hello *alapvető* szabály használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-157">Currently, only hello *basic* rule is supported.</span></span> <span data-ttu-id="4b4cf-158">Hello *alapvető* szabály-e időszeletelés terheléselosztási.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-158">hello *basic* rule is round-robin load distribution.</span></span>

<span data-ttu-id="4b4cf-159">**További konfigurációs megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="4b4cf-159">**Additional configuration notes**</span></span>

<span data-ttu-id="4b4cf-160">SSL-tanúsítványok beállítása, a hello protokoll **HttpListener** túl kell módosítani*Https* (kis-és nagybetűket).</span><span class="sxs-lookup"><span data-stu-id="4b4cf-160">For SSL certificates configuration, hello protocol in **HttpListener** should change too*Https* (case sensitive).</span></span> <span data-ttu-id="4b4cf-161">Hello **SslCert** elem túl kerül**HttpListener** a hello értéke toohello azonos hello feltöltése az előző SSL-tanúsítványok szakaszban használt nevet.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-161">hello **SslCert** element is added too**HttpListener** with hello value set toohello same name as used in hello upload of preceding SSL certificates section.</span></span> <span data-ttu-id="4b4cf-162">hello előtér-port frissített too443 lehet.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-162">hello front-end port should be updated too443.</span></span>

<span data-ttu-id="4b4cf-163">**tooenable cookie-alapú kapcsolat**: Alkalmazásátjáró lehet győződjön meg arról, hogy egy kérelem egy ügyfél-munkamenetből mindig irányított toohello konfigurált tooensure hello webfarm azonos virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-163">**tooenable cookie-based affinity**: An application gateway can be configured tooensure that a request from a client session is always directed toohello same VM in hello web farm.</span></span> <span data-ttu-id="4b4cf-164">Ebben a forgatókönyvben történik, hogy az egy munkamenetcookie-t, amely lehetővé teszi annak megfelelően hello átjáró toodirect forgalom.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-164">This scenario is done by injection of a session cookie that allows hello gateway toodirect traffic appropriately.</span></span> <span data-ttu-id="4b4cf-165">cookie-alapú tooenable affinitás beállítása **CookieBasedAffinity** túl*engedélyezve* a hello **BackendHttpSettings** elemet.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-165">tooenable cookie-based affinity, set **CookieBasedAffinity** too*Enabled* in hello **BackendHttpSettings** element.</span></span>

<span data-ttu-id="4b4cf-166">A konfigurációs hogyan hozhat létre, vagy hozzon létre egy konfigurációs objektumot, vagy egy konfigurációs XML-fájl használatával.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-166">You can construct your configuration either by creating a configuration object or by using a configuration XML file.</span></span>
<span data-ttu-id="4b4cf-167">tooconstruct egy konfigurációs XML-fájl használatával a konfigurációt használja következő minta hello:</span><span class="sxs-lookup"><span data-stu-id="4b4cf-167">tooconstruct your configuration by using a configuration XML file, use hello following sample:</span></span>

<span data-ttu-id="4b4cf-168">**Konfigurációs XML-minta**</span><span class="sxs-lookup"><span data-stu-id="4b4cf-168">**Configuration XML sample**</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations />
    <FrontendPorts>
        <FrontendPort>
            <Name>FrontendPort1</Name>
            <Port>443</Port>
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
            <Protocol>Https</Protocol>
            <SslCert>GWCert</SslCert>
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

## <a name="set-hello-gateway-configuration"></a><span data-ttu-id="4b4cf-169">Hello átjáró konfigurációjának beállítása</span><span class="sxs-lookup"><span data-stu-id="4b4cf-169">Set hello gateway configuration</span></span>

<span data-ttu-id="4b4cf-170">Ezután állítsa be hello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-170">Next, you set hello application gateway.</span></span> <span data-ttu-id="4b4cf-171">Használhatja a hello `Set-AzureApplicationGatewayConfig` parancsmag vagy a konfigurációs objektum vagy egy konfigurációs XML-fájlt.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-171">You can use hello `Set-AzureApplicationGatewayConfig` cmdlet with either a configuration object or with a configuration XML file.</span></span>

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml
```

## <a name="start-hello-gateway"></a><span data-ttu-id="4b4cf-172">Indítsa el a hello átjáró</span><span class="sxs-lookup"><span data-stu-id="4b4cf-172">Start hello gateway</span></span>

<span data-ttu-id="4b4cf-173">Ha hello átjáró van konfigurálva, a hello `Start-AzureApplicationGateway` parancsmag toostart hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-173">Once hello gateway has been configured, use hello `Start-AzureApplicationGateway` cmdlet toostart hello gateway.</span></span> <span data-ttu-id="4b4cf-174">Alkalmazásátjáró számlázás megkezdése után hello átjáró sikeresen elindult.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-174">Billing for an application gateway begins after hello gateway has been successfully started.</span></span>

> [!NOTE]
> <span data-ttu-id="4b4cf-175">Hello `Start-AzureApplicationGateway` parancsmag is igénybe vehet fel toofinish too15-20 perc.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-175">hello `Start-AzureApplicationGateway` cmdlet might take up too15-20 minutes toofinish.</span></span>
>
>

```powershell
Start-AzureApplicationGateway AppGwTest
```

## <a name="verify-hello-gateway-status"></a><span data-ttu-id="4b4cf-176">Hello az átjáró állapotának megerősítése</span><span class="sxs-lookup"><span data-stu-id="4b4cf-176">Verify hello gateway status</span></span>

<span data-ttu-id="4b4cf-177">Használjon hello `Get-AzureApplicationGateway` parancsmag toocheck hello hello átjáró állapotának.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-177">Use hello `Get-AzureApplicationGateway` cmdlet toocheck hello status of hello gateway.</span></span> <span data-ttu-id="4b4cf-178">Ha `Start-AzureApplicationGateway` hello a korábbi lépésben sikeres *állapot* kell futnia, és *virtualip értékek* és *DnsName* érvényes bejegyzést kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-178">If `Start-AzureApplicationGateway` succeeded in hello previous step, *State* should be Running, and *VirtualIPs* and *DnsName* should have valid entries.</span></span>

<span data-ttu-id="4b4cf-179">Ez a példa bemutatja, hogy működik-e, fut, és készen áll a tootake forgalom Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="4b4cf-179">This sample shows an application gateway that is up, running, and is ready tootake traffic.</span></span>

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
Name          : AppGwTest2
Description   :
VnetName      : testvnet1
Subnets       : {Subnet-1}
InstanceCount : 2
GatewaySize   : Medium
State         : Running
VirtualIPs    : {23.96.22.241}
DnsName       : appgw-4c960426-d1e6-4aae-8670-81fd7a519a43.cloudapp.net
```

## <a name="next-steps"></a><span data-ttu-id="4b4cf-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4b4cf-180">Next steps</span></span>

<span data-ttu-id="4b4cf-181">Ha további általános információra van szüksége a terheléselosztás beállításaival kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="4b4cf-181">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="4b4cf-182">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="4b4cf-182">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="4b4cf-183">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="4b4cf-183">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

