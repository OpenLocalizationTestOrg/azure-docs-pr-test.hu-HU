---
title: "Konfigurálja az SSL - Azure Application Gateway - kiszervezés klasszikus PowerShell |} Microsoft Docs"
description: "Ez a cikk ismerteti az Azure klasszikus telepítési modell segítségével kiszervezése SSL Alkalmazásátjáró létrehozásához nyújt útmutatást."
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
ms.openlocfilehash: 2eba6fb24c11add12ac16d04d3445e19a3486216
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-classic-deployment-model"></a><span data-ttu-id="dce38-103">SSL kiszervezési Alkalmazásátjáró konfigurálása a klasszikus telepítési modell segítségével</span><span class="sxs-lookup"><span data-stu-id="dce38-103">Configure an application gateway for SSL offload by using the classic deployment model</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="dce38-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="dce38-104">Azure portal</span></span>](application-gateway-ssl-portal.md)
> * [<span data-ttu-id="dce38-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="dce38-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ssl-arm.md)
> * [<span data-ttu-id="dce38-106">Klasszikus Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="dce38-106">Azure Classic PowerShell</span></span>](application-gateway-ssl.md)
> * [<span data-ttu-id="dce38-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="dce38-107">Azure CLI 2.0</span></span>](application-gateway-ssl-cli.md)

<span data-ttu-id="dce38-108">Az Azure Application Gateway konfigurálható úgy, hogy leállítsa a Secure Sockets Layer (SSL) munkamenetét az átjárónál, így elkerülhetők a költséges SSL visszafejtési feladatok a webfarmon.</span><span class="sxs-lookup"><span data-stu-id="dce38-108">Azure Application Gateway can be configured to terminate the Secure Sockets Layer (SSL) session at the gateway to avoid costly SSL decryption tasks to happen at the web farm.</span></span> <span data-ttu-id="dce38-109">Az SSL-alapú kiszervezés emellett leegyszerűsíti az előtér-kiszolgáló számára webalkalmazás telepítését és kezelését.</span><span class="sxs-lookup"><span data-stu-id="dce38-109">SSL offload also simplifies the front-end server setup and management of the web application.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="dce38-110">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="dce38-110">Before you begin</span></span>

1. <span data-ttu-id="dce38-111">Telepítse az Azure PowerShell-parancsmagok legújabb verzióját a Webplatform-telepítővel.</span><span class="sxs-lookup"><span data-stu-id="dce38-111">Install the latest version of the Azure PowerShell cmdlets by using the Web Platform Installer.</span></span> <span data-ttu-id="dce38-112">A [Letöltések lap](https://azure.microsoft.com/downloads/) **Windows PowerShell** szakaszából letöltheti és telepítheti a legújabb verziót.</span><span class="sxs-lookup"><span data-stu-id="dce38-112">You can download and install the latest version from the **Windows PowerShell** section of the [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="dce38-113">Ellenőrizze, hogy rendelkezik-e működő virtuális hálózattal és hozzá tartozó érvényes alhálózattal.</span><span class="sxs-lookup"><span data-stu-id="dce38-113">Verify that you have a working virtual network with a valid subnet.</span></span> <span data-ttu-id="dce38-114">Győződjön meg arról, hogy egy virtuális gép vagy felhőalapú telepítés sem használja az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="dce38-114">Make sure that no virtual machines or cloud deployments are using the subnet.</span></span> <span data-ttu-id="dce38-115">Az Application Gateway-nek egyedül kell lennie a virtuális hálózat alhálózatán.</span><span class="sxs-lookup"><span data-stu-id="dce38-115">The application gateway must be by itself in a virtual network subnet.</span></span>
3. <span data-ttu-id="dce38-116">A kiszolgálóknak, amelyeket az Application Gateway használatára konfigurál, már létezniük kell, illetve a virtuális hálózatban vagy hozzárendelt nyilvános/virtuális IP-címmel létrehozott végpontokkal kell rendelkezniük.</span><span class="sxs-lookup"><span data-stu-id="dce38-116">The servers that you configure to use the application gateway must exist or have their endpoints created either in the virtual network or with a public IP/VIP assigned.</span></span>

<span data-ttu-id="dce38-117">Az Alkalmazásátjáró SSL kiszervezési konfigurálásához hajtsa végre az alábbi lépéseket a megadott sorrendben:</span><span class="sxs-lookup"><span data-stu-id="dce38-117">To configure SSL offload on an application gateway, do the following steps in the order listed:</span></span>

1. [<span data-ttu-id="dce38-118">Alkalmazásátjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="dce38-118">Create an application gateway</span></span>](#create-an-application-gateway)
2. [<span data-ttu-id="dce38-119">SSL-tanúsítványok feltöltése</span><span class="sxs-lookup"><span data-stu-id="dce38-119">Upload SSL certificates</span></span>](#upload-ssl-certificates)
3. [<span data-ttu-id="dce38-120">Az átjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dce38-120">Configure the gateway</span></span>](#configure-the-gateway)
4. [<span data-ttu-id="dce38-121">Állítsa be az átjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dce38-121">Set the gateway configuration</span></span>](#set-the-gateway-configuration)
5. [<span data-ttu-id="dce38-122">Az átjáró elindítása</span><span class="sxs-lookup"><span data-stu-id="dce38-122">Start the gateway</span></span>](#start-the-gateway)
6. [<span data-ttu-id="dce38-123">Az átjáró állapotának megerősítése</span><span class="sxs-lookup"><span data-stu-id="dce38-123">Verify the gateway status</span></span>](#verify-the-gateway-status)

## <a name="create-an-application-gateway"></a><span data-ttu-id="dce38-124">Application Gateway létrehozása</span><span class="sxs-lookup"><span data-stu-id="dce38-124">Create an application gateway</span></span>

<span data-ttu-id="dce38-125">Az átjáró létrehozásához használja a `New-AzureApplicationGateway` parancsmagot, és cserélje le az értékeket a saját értékeire.</span><span class="sxs-lookup"><span data-stu-id="dce38-125">To create the gateway, use the `New-AzureApplicationGateway` cmdlet, replacing the values with your own.</span></span> <span data-ttu-id="dce38-126">Az átjáró használati díjának felszámolása ekkor még nem kezdődik el.</span><span class="sxs-lookup"><span data-stu-id="dce38-126">Billing for the gateway does not start at this point.</span></span> <span data-ttu-id="dce38-127">A használati díj felszámolása egy későbbi lépésnél kezdődik, amikor az átjáró sikeresen elindul.</span><span class="sxs-lookup"><span data-stu-id="dce38-127">Billing begins in a later step, when the gateway is successfully started.</span></span>

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

<span data-ttu-id="dce38-128">Az átjáró létrehozásának ellenőrzéséhez használhatja a `Get-AzureApplicationGateway` parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="dce38-128">To validate that the gateway was created, you can use the `Get-AzureApplicationGateway` cmdlet.</span></span>

<span data-ttu-id="dce38-129">A minta *leírás*, *InstanceCount*, és *GatewaySize* opcionális paraméterek.</span><span class="sxs-lookup"><span data-stu-id="dce38-129">In the sample, *Description*, *InstanceCount*, and *GatewaySize* are optional parameters.</span></span> <span data-ttu-id="dce38-130">Az *InstanceCount* alapértelmezett értéke 2, a maximális értéke pedig 10.</span><span class="sxs-lookup"><span data-stu-id="dce38-130">The default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="dce38-131">A *GatewaySize* alapértelmezett értéke Közepes.</span><span class="sxs-lookup"><span data-stu-id="dce38-131">The default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="dce38-132">Kis és nagy más elérhető értékek.</span><span class="sxs-lookup"><span data-stu-id="dce38-132">Small and Large are other available values.</span></span> <span data-ttu-id="dce38-133">A *VirtualIPs* és a *DnsName* paraméterek azért üresek, mert az átjáró még nem indult el.</span><span class="sxs-lookup"><span data-stu-id="dce38-133">*VirtualIPs* and *DnsName* are shown as blank because the gateway has not started yet.</span></span> <span data-ttu-id="dce38-134">Ezek az értékek jönnek létre, ha az átjáró már szerepel a futó állapotot.</span><span class="sxs-lookup"><span data-stu-id="dce38-134">These values are created once the gateway is in the running state.</span></span>

```powershell
Get-AzureApplicationGateway AppGwTest
```

## <a name="upload-ssl-certificates"></a><span data-ttu-id="dce38-135">SSL-tanúsítványok feltöltése</span><span class="sxs-lookup"><span data-stu-id="dce38-135">Upload SSL certificates</span></span>

<span data-ttu-id="dce38-136">Használjon `Add-AzureApplicationGatewaySslCertificate` töltse fel a kiszolgálói tanúsítvány *pfx* formátum az Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="dce38-136">Use `Add-AzureApplicationGatewaySslCertificate` to upload the server certificate in *pfx* format to the application gateway.</span></span> <span data-ttu-id="dce38-137">A tanúsítvány nevét a felhasználó által választott név, és az Alkalmazásátjáró belül egyedieknek kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="dce38-137">The certificate name is a user-chosen name and must be unique within the application gateway.</span></span> <span data-ttu-id="dce38-138">Ezt a tanúsítványt ezt a nevet a tanúsítvány az alkalmazás-átjárón összes felügyeleti művelet is hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="dce38-138">This certificate is referred to by this name in all certificate management operations on the application gateway.</span></span>

<span data-ttu-id="dce38-139">Ez a következő példa bemutatja a parancsmag cserélje le a mintában szereplő értékekkel saját.</span><span class="sxs-lookup"><span data-stu-id="dce38-139">This following sample shows the cmdlet, replace the values in the sample with your own.</span></span>

```powershell
Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path to pfx file>
```

<span data-ttu-id="dce38-140">A következő érvényesítse a tanúsítvány feltöltése.</span><span class="sxs-lookup"><span data-stu-id="dce38-140">Next, validate the certificate upload.</span></span> <span data-ttu-id="dce38-141">Használja a `Get-AzureApplicationGatewayCertificate` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="dce38-141">Use the `Get-AzureApplicationGatewayCertificate` cmdlet.</span></span>

<span data-ttu-id="dce38-142">Ez a példa bemutatja a parancsmag az első sorba a kimeneti követ.</span><span class="sxs-lookup"><span data-stu-id="dce38-142">This sample shows the cmdlet on the first line, followed by the output.</span></span>

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
> <span data-ttu-id="dce38-143">A tanúsítvány jelszava nem lehet 4 – 12 karakterből, betűk és számok között.</span><span class="sxs-lookup"><span data-stu-id="dce38-143">The certificate password has to be between 4 to 12 characters, letters, or numbers.</span></span> <span data-ttu-id="dce38-144">Speciális karakterek használata nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="dce38-144">Special characters are not accepted.</span></span>

## <a name="configure-the-gateway"></a><span data-ttu-id="dce38-145">Az átjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dce38-145">Configure the gateway</span></span>

<span data-ttu-id="dce38-146">Egy alkalmazás átjáró konfigurálása több érték áll.</span><span class="sxs-lookup"><span data-stu-id="dce38-146">An application gateway configuration consists of multiple values.</span></span> <span data-ttu-id="dce38-147">Az értékek is kötődik együtt a konfiguráció létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="dce38-147">The values can be tied together to construct the configuration.</span></span>

<span data-ttu-id="dce38-148">Az értékek a következők:</span><span class="sxs-lookup"><span data-stu-id="dce38-148">The values are:</span></span>

* <span data-ttu-id="dce38-149">**Háttér-kiszolgálókészlet:** A háttérkiszolgálók IP-címeinek listája.</span><span class="sxs-lookup"><span data-stu-id="dce38-149">**Back-end server pool:** The list of IP addresses of the back-end servers.</span></span> <span data-ttu-id="dce38-150">A listán szereplő IP-címeknek a virtuális hálózat alhálózatához kell tartozniuk, vagy nyilvános/virtuális IP-címnek kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="dce38-150">The IP addresses listed should either belong to the virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="dce38-151">**Háttér-kiszolgálókészlet beállításai:** Minden készletnek vannak beállításai, például port, protokoll vagy cookie-alapú affinitás.</span><span class="sxs-lookup"><span data-stu-id="dce38-151">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="dce38-152">Ezek a beállítások egy adott készlethez kapcsolódnak, és a készlet minden kiszolgálójára érvényesek.</span><span class="sxs-lookup"><span data-stu-id="dce38-152">These settings are tied to a pool and are applied to all servers within the pool.</span></span>
* <span data-ttu-id="dce38-153">**Előtérbeli port:** Az Application Gateway-en megnyitott nyilvános port.</span><span class="sxs-lookup"><span data-stu-id="dce38-153">**Front-end port:** This port is the public port that is opened on the application gateway.</span></span> <span data-ttu-id="dce38-154">Amikor a forgalom eléri ezt a portot, a port átirányítja az egyik háttérkiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="dce38-154">Traffic hits this port, and then gets redirected to one of the back-end servers.</span></span>
* <span data-ttu-id="dce38-155">**Figyelő:** A figyelő egy előtérbeli porttal, egy protokollal (Http vagy Https, a kis- és a nagybetűk megkülönböztetésével) és SSL tanúsítványnévvel rendelkezik (SSL-kiszervezés konfigurálásakor).</span><span class="sxs-lookup"><span data-stu-id="dce38-155">**Listener:** The listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and the SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="dce38-156">**Szabály:** A szabály összeköti a figyelőt és a háttérkiszolgáló-készletet, és meghatározza, hogy mely háttérkiszolgáló-készletre legyen átirányítva a forgalom, ha elér egy adott figyelőt.</span><span class="sxs-lookup"><span data-stu-id="dce38-156">**Rule:** The rule binds the listener and the back-end server pool and defines which back-end server pool the traffic should be directed to when it hits a particular listener.</span></span> <span data-ttu-id="dce38-157">Jelenleg csak a *basic* szabály támogatott.</span><span class="sxs-lookup"><span data-stu-id="dce38-157">Currently, only the *basic* rule is supported.</span></span> <span data-ttu-id="dce38-158">A *basic* szabály a ciklikus időszeleteléses terheléselosztás.</span><span class="sxs-lookup"><span data-stu-id="dce38-158">The *basic* rule is round-robin load distribution.</span></span>

<span data-ttu-id="dce38-159">**További konfigurációs megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="dce38-159">**Additional configuration notes**</span></span>

<span data-ttu-id="dce38-160">Az SSL-tanúsítványok konfigurálásához *Https*-re kell módosítani a **HttpListener** protokollját (megkülönböztetve a kis- és nagybetűket).</span><span class="sxs-lookup"><span data-stu-id="dce38-160">For SSL certificates configuration, the protocol in **HttpListener** should change to *Https* (case sensitive).</span></span> <span data-ttu-id="dce38-161">A **SslCert** elem hozzáadódik **HttpListener** az érték azonos nevű feltöltésének használt megelőző SSL-tanúsítványok szakasz.</span><span class="sxs-lookup"><span data-stu-id="dce38-161">The **SslCert** element is added to **HttpListener** with the value set to the same name as used in the upload of preceding SSL certificates section.</span></span> <span data-ttu-id="dce38-162">Az előtérbeli portot frissíteni kell 443-ra.</span><span class="sxs-lookup"><span data-stu-id="dce38-162">The front-end port should be updated to 443.</span></span>

<span data-ttu-id="dce38-163">**A cookie-alapú affinitás engedélyezése**: Egy Application Gateway konfigurálható úgy, hogy egy ügyfélmunkamenetből érkező kérelmet mindig a webfarm ugyanazon virtuális gépére irányítsa.</span><span class="sxs-lookup"><span data-stu-id="dce38-163">**To enable cookie-based affinity**: An application gateway can be configured to ensure that a request from a client session is always directed to the same VM in the web farm.</span></span> <span data-ttu-id="dce38-164">Ez a forgatókönyv egy munkameneti cookie beszúrásával lesz végrehajtva, amely lehetővé teszi az átjáró számára a forgalom megfelelő irányítását.</span><span class="sxs-lookup"><span data-stu-id="dce38-164">This scenario is done by injection of a session cookie that allows the gateway to direct traffic appropriately.</span></span> <span data-ttu-id="dce38-165">A cookie-alapú affinitás engedélyezéséhez a **CookieBasedAffinity** paraméter beállítása legyen *Enabled* a **BackendHttpSettings** elemen belül.</span><span class="sxs-lookup"><span data-stu-id="dce38-165">To enable cookie-based affinity, set **CookieBasedAffinity** to *Enabled* in the **BackendHttpSettings** element.</span></span>

<span data-ttu-id="dce38-166">A konfigurációs hogyan hozhat létre, vagy hozzon létre egy konfigurációs objektumot, vagy egy konfigurációs XML-fájl használatával.</span><span class="sxs-lookup"><span data-stu-id="dce38-166">You can construct your configuration either by creating a configuration object or by using a configuration XML file.</span></span>
<span data-ttu-id="dce38-167">Összeállíthatja a konfigurációs XML konfigurációs fájl használatával, használja a következő mintát:</span><span class="sxs-lookup"><span data-stu-id="dce38-167">To construct your configuration by using a configuration XML file, use the following sample:</span></span>

<span data-ttu-id="dce38-168">**Konfigurációs XML-minta**</span><span class="sxs-lookup"><span data-stu-id="dce38-168">**Configuration XML sample**</span></span>

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

## <a name="set-the-gateway-configuration"></a><span data-ttu-id="dce38-169">Állítsa be az átjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dce38-169">Set the gateway configuration</span></span>

<span data-ttu-id="dce38-170">Ezután állítsa be az Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="dce38-170">Next, you set the application gateway.</span></span> <span data-ttu-id="dce38-171">Használhatja a `Set-AzureApplicationGatewayConfig` parancsmag vagy a konfigurációs objektum vagy egy konfigurációs XML-fájlt.</span><span class="sxs-lookup"><span data-stu-id="dce38-171">You can use the `Set-AzureApplicationGatewayConfig` cmdlet with either a configuration object or with a configuration XML file.</span></span>

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml
```

## <a name="start-the-gateway"></a><span data-ttu-id="dce38-172">Az átjáró indítása</span><span class="sxs-lookup"><span data-stu-id="dce38-172">Start the gateway</span></span>

<span data-ttu-id="dce38-173">Az átjáró konfigurálása után indítsa el az átjárót a `Start-AzureApplicationGateway` parancsmaggal.</span><span class="sxs-lookup"><span data-stu-id="dce38-173">Once the gateway has been configured, use the `Start-AzureApplicationGateway` cmdlet to start the gateway.</span></span> <span data-ttu-id="dce38-174">Az Application Gateway használati díjának felszámolása az átjáró sikeres indítása után kezdődik.</span><span class="sxs-lookup"><span data-stu-id="dce38-174">Billing for an application gateway begins after the gateway has been successfully started.</span></span>

> [!NOTE]
> <span data-ttu-id="dce38-175">A `Start-AzureApplicationGateway` parancsmag futtatása akár 15–20 percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="dce38-175">The `Start-AzureApplicationGateway` cmdlet might take up to 15-20 minutes to finish.</span></span>
>
>

```powershell
Start-AzureApplicationGateway AppGwTest
```

## <a name="verify-the-gateway-status"></a><span data-ttu-id="dce38-176">Az átjáró állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="dce38-176">Verify the gateway status</span></span>

<span data-ttu-id="dce38-177">Ellenőrizze az átjáró állapotát a `Get-AzureApplicationGateway` parancsmaggal.</span><span class="sxs-lookup"><span data-stu-id="dce38-177">Use the `Get-AzureApplicationGateway` cmdlet to check the status of the gateway.</span></span> <span data-ttu-id="dce38-178">Ha `Start-AzureApplicationGateway` sikeres volt az előző lépésben *állapot* kell futnia, és *virtualip értékek* és *DnsName* érvényes bejegyzést kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="dce38-178">If `Start-AzureApplicationGateway` succeeded in the previous step, *State* should be Running, and *VirtualIPs* and *DnsName* should have valid entries.</span></span>

<span data-ttu-id="dce38-179">Ez a példa bemutatja, hogy működik-e, fut, és készen áll forgalom elkészítésére Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="dce38-179">This sample shows an application gateway that is up, running, and is ready to take traffic.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="dce38-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dce38-180">Next steps</span></span>

<span data-ttu-id="dce38-181">Ha további általános információra van szüksége a terheléselosztás beállításaival kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="dce38-181">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="dce38-182">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="dce38-182">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="dce38-183">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="dce38-183">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

