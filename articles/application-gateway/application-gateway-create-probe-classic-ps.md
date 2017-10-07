---
title: "egyéni tesztműveleti - Azure Application Gateway - PowerShell klasszikus aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egyéni mintavételi az Alkalmazásátjáró hello klasszikus üzembe helyezési modellel PowerShell használatával"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 338a7be1-835c-48e9-a072-95662dc30f5e
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: gwallace
ms.openlocfilehash: 68332367c99328bd6456b0c339923765637be986
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-probe-for-azure-application-gateway-classic-by-using-powershell"></a><span data-ttu-id="56db6-103">Hozzon létre egy egyéni mintavétel az Azure Application Gateway (klasszikus) PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="56db6-103">Create a custom probe for Azure Application Gateway (classic) by using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="56db6-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="56db6-104">Azure portal</span></span>](application-gateway-create-probe-portal.md)
> * [<span data-ttu-id="56db6-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="56db6-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-probe-ps.md)
> * [<span data-ttu-id="56db6-106">Klasszikus Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="56db6-106">Azure Classic PowerShell</span></span>](application-gateway-create-probe-classic-ps.md)

<span data-ttu-id="56db6-107">Ebben a cikkben egy egyéni mintavételi tooan meglévő Alkalmazásátjáró a PowerShell használatával adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="56db6-107">In this article, you add a custom probe tooan existing application gateway with PowerShell.</span></span> <span data-ttu-id="56db6-108">Egyéni mintavételt hasznosak, az alkalmazásokat, amelyek egy adott állapotának ellenőrzése lapon vagy az alkalmazásokat, amelyek nem a sikeres válasz hello alapértelmezett webalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="56db6-108">Custom probes are useful for applications that have a specific health check page or for applications that do not provide a successful response on hello default web application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="56db6-109">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="56db6-109">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="56db6-110">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="56db6-110">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="56db6-111">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="56db6-111">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="56db6-112">Ismerje meg, hogyan túl[hello Resource Manager modellt használja a következő lépésekkel](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="56db6-112">Learn how too[perform these steps using hello Resource Manager model](application-gateway-create-probe-ps.md).</span></span>

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway"></a><span data-ttu-id="56db6-113">Application Gateway létrehozása</span><span class="sxs-lookup"><span data-stu-id="56db6-113">Create an application gateway</span></span>

<span data-ttu-id="56db6-114">Alkalmazásátjáró toocreate:</span><span class="sxs-lookup"><span data-stu-id="56db6-114">toocreate an application gateway:</span></span>

1. <span data-ttu-id="56db6-115">Egy Application Gateway erőforrás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="56db6-115">Create an application gateway resource.</span></span>
2. <span data-ttu-id="56db6-116">Hozzon létre egy konfigurációs XML-fájlt vagy konfigurációs objektumot.</span><span class="sxs-lookup"><span data-stu-id="56db6-116">Create a configuration XML file or a configuration object.</span></span>
3. <span data-ttu-id="56db6-117">Az újonnan létrehozott alkalmazás átjáró erőforrás hello konfigurációs toohello véglegesítése.</span><span class="sxs-lookup"><span data-stu-id="56db6-117">Commit hello configuration toohello newly created application gateway resource.</span></span>

### <a name="create-an-application-gateway-resource-with-a-custom-probe"></a><span data-ttu-id="56db6-118">Egy egyéni mintavételi alkalmazás átjáró erőforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="56db6-118">Create an application gateway resource with a custom probe</span></span>

<span data-ttu-id="56db6-119">toocreate hello átjáró használata hello `New-AzureApplicationGateway` hello értékeket cserélje le a saját, a parancsmag.</span><span class="sxs-lookup"><span data-stu-id="56db6-119">toocreate hello gateway, use hello `New-AzureApplicationGateway` cmdlet, replacing hello values with your own.</span></span> <span data-ttu-id="56db6-120">Számlázási hello átjáró nem indul el ezen a ponton.</span><span class="sxs-lookup"><span data-stu-id="56db6-120">Billing for hello gateway does not start at this point.</span></span> <span data-ttu-id="56db6-121">Számlázási egy későbbi lépésben, akkor kezdődik, amikor hello átjáró sikeresen elindult.</span><span class="sxs-lookup"><span data-stu-id="56db6-121">Billing begins in a later step, when hello gateway is successfully started.</span></span>

<span data-ttu-id="56db6-122">hello alábbi példa használatával hozza létre az Alkalmazásátjáró nevű, "testvnet1" és "alhálózat-1" nevű alhálózat virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="56db6-122">hello following example creates an application gateway by using a virtual network called "testvnet1" and a subnet called "subnet-1".</span></span>

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

<span data-ttu-id="56db6-123">amely átjáró hello toovalidate lett létrehozva, használhatja a hello `Get-AzureApplicationGateway` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="56db6-123">toovalidate that hello gateway was created, you can use hello `Get-AzureApplicationGateway` cmdlet.</span></span>

```powershell
Get-AzureApplicationGateway AppGwTest
```

> [!NOTE]
> <span data-ttu-id="56db6-124">az alapértelmezett érték hello *InstanceCount* 2, maximális értéke 10.</span><span class="sxs-lookup"><span data-stu-id="56db6-124">hello default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="56db6-125">az alapértelmezett érték hello *GatewaySize* közepes.</span><span class="sxs-lookup"><span data-stu-id="56db6-125">hello default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="56db6-126">Kis, közepes és nagy közül választhat.</span><span class="sxs-lookup"><span data-stu-id="56db6-126">You can choose between Small, Medium, and Large.</span></span>
> 
> 

<span data-ttu-id="56db6-127">*Virtualip értékek* és *DnsName* jelennek meg az üres mert hello átjáró még nem kezdődött meg.</span><span class="sxs-lookup"><span data-stu-id="56db6-127">*VirtualIPs* and *DnsName* are shown as blank because hello gateway has not started yet.</span></span> <span data-ttu-id="56db6-128">Ezeket az értékeket jönnek létre, ha hello átjáró hello futó állapotban van.</span><span class="sxs-lookup"><span data-stu-id="56db6-128">These values are created once hello gateway is in hello running state.</span></span>

### <a name="configure-an-application-gateway-by-using-xml"></a><span data-ttu-id="56db6-129">Alkalmazásátjáró konfigurálása XML használatával</span><span class="sxs-lookup"><span data-stu-id="56db6-129">Configure an application gateway by using XML</span></span>

<span data-ttu-id="56db6-130">A következő példa hello egy XML-fájl tooconfigure összes alkalmazás átjáró beállításait használja, és véglegesítse azokat toohello alkalmazás átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="56db6-130">In hello following example, you use an XML file tooconfigure all application gateway settings and commit them toohello application gateway resource.</span></span>  

<span data-ttu-id="56db6-131">A következő szöveg tooNotepad hello másolja.</span><span class="sxs-lookup"><span data-stu-id="56db6-131">Copy hello following text tooNotepad.</span></span>

```xml
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
<FrontendIPConfigurations>
    <FrontendIPConfiguration>
        <Name>fip1</Name>
        <Type>Private</Type>
    </FrontendIPConfiguration>
</FrontendIPConfigurations>
<FrontendPorts>
    <FrontendPort>
        <Name>port1</Name>
        <Port>80</Port>
    </FrontendPort>
</FrontendPorts>
<Probes>
    <Probe>
        <Name>Probe01</Name>
        <Protocol>Http</Protocol>
        <Host>contoso.com</Host>
        <Path>/path/custompath.htm</Path>
        <Interval>15</Interval>
        <Timeout>15</Timeout>
        <UnhealthyThreshold>5</UnhealthyThreshold>
    </Probe>
    </Probes>
    <BackendAddressPools>
    <BackendAddressPool>
        <Name>pool1</Name>
        <IPAddresses>
            <IPAddress>1.1.1.1</IPAddress>
            <IPAddress>2.2.2.2</IPAddress>
        </IPAddresses>
    </BackendAddressPool>
</BackendAddressPools>
<BackendHttpSettingsList>
    <BackendHttpSettings>
        <Name>setting1</Name>
        <Port>80</Port>
        <Protocol>Http</Protocol>
        <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        <RequestTimeout>120</RequestTimeout>
        <Probe>Probe01</Probe>
    </BackendHttpSettings>
</BackendHttpSettingsList>
<HttpListeners>
    <HttpListener>
        <Name>listener1</Name>
        <FrontendIP>fip1</FrontendIP>
    <FrontendPort>port1</FrontendPort>
        <Protocol>Http</Protocol>
    </HttpListener>
</HttpListeners>
<HttpLoadBalancingRules>
    <HttpLoadBalancingRule>
        <Name>lbrule1</Name>
        <Type>basic</Type>
        <BackendHttpSettings>setting1</BackendHttpSettings>
        <Listener>listener1</Listener>
        <BackendAddressPool>pool1</BackendAddressPool>
    </HttpLoadBalancingRule>
</HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

<span data-ttu-id="56db6-132">Módosíthatja a hello konfigurációs elemek hello zárójelek között hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="56db6-132">Edit hello values between hello parentheses for hello configuration items.</span></span> <span data-ttu-id="56db6-133">Bővítmény .xml hello fájl mentése.</span><span class="sxs-lookup"><span data-stu-id="56db6-133">Save hello file with extension .xml.</span></span>

<span data-ttu-id="56db6-134">hello következő példa bemutatja, hogyan toouse egy konfigurációs fájl tooset hello alkalmazás átjáró tooload be HTTP-forgalom 80-as nyilvános portjához egyensúlyba és a hálózati forgalom elküldése a 80-as port tooback-a befejezési egy egyéni mintavételt használatával két IP-címre.</span><span class="sxs-lookup"><span data-stu-id="56db6-134">hello following example shows how toouse a configuration file tooset up hello application gateway tooload balance HTTP traffic on public port 80 and send network traffic tooback-end port 80 between two IP addresses by using a custom probe.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="56db6-135">hello protokoll Http vagy Https elem kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="56db6-135">hello protocol item Http or Https is case-sensitive.</span></span>

<span data-ttu-id="56db6-136">Egy új konfigurációelemet \<mintavételi\> tooconfigure egyéni mintavételt kerül.</span><span class="sxs-lookup"><span data-stu-id="56db6-136">A new configuration item \<Probe\> is added tooconfigure custom probes.</span></span>

<span data-ttu-id="56db6-137">hello konfigurációs paraméterek a következők:</span><span class="sxs-lookup"><span data-stu-id="56db6-137">hello configuration parameters are:</span></span>

|<span data-ttu-id="56db6-138">Paraméter</span><span class="sxs-lookup"><span data-stu-id="56db6-138">Parameter</span></span>|<span data-ttu-id="56db6-139">Leírás</span><span class="sxs-lookup"><span data-stu-id="56db6-139">Description</span></span>|
|---|---|
|<span data-ttu-id="56db6-140">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="56db6-140">**Name**</span></span> |<span data-ttu-id="56db6-141">Egyéni mintavétel hivatkozás neve.</span><span class="sxs-lookup"><span data-stu-id="56db6-141">Reference name for custom probe.</span></span> |
<span data-ttu-id="56db6-142">* **Protokoll**</span><span class="sxs-lookup"><span data-stu-id="56db6-142">* **Protocol**</span></span> | <span data-ttu-id="56db6-143">Használt protokoll (a lehetséges értékek: HTTP vagy HTTPS).</span><span class="sxs-lookup"><span data-stu-id="56db6-143">Protocol used (possible values are HTTP or HTTPS).</span></span>|
| <span data-ttu-id="56db6-144">**Állomás** és **elérési útja**</span><span class="sxs-lookup"><span data-stu-id="56db6-144">**Host** and **Path**</span></span> | <span data-ttu-id="56db6-145">Végezze el, amelyet hello alkalmazás átjáró toodetermine hello állapotának hello példány URL-címe.</span><span class="sxs-lookup"><span data-stu-id="56db6-145">Complete URL path that is invoked by hello application gateway toodetermine hello health of hello instance.</span></span> <span data-ttu-id="56db6-146">Például ha egy webhely http://contoso.com/ majd hello egyéni mintavételi konfigurálhatja a "http://contoso.com/path/custompath.htm" mintavétel toohave sikeres HTTP-válasz ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="56db6-146">For example, if you have a website http://contoso.com/, then hello custom probe can be configured for "http://contoso.com/path/custompath.htm" for probe checks toohave a successful HTTP response.</span></span>|
| <span data-ttu-id="56db6-147">**Időköz**</span><span class="sxs-lookup"><span data-stu-id="56db6-147">**Interval**</span></span> | <span data-ttu-id="56db6-148">Konfigurálja a hello mintavételi időköze ellenőrzések másodpercben.</span><span class="sxs-lookup"><span data-stu-id="56db6-148">Configures hello probe interval checks in seconds.</span></span>|
| <span data-ttu-id="56db6-149">**Időtúllépés**</span><span class="sxs-lookup"><span data-stu-id="56db6-149">**Timeout**</span></span> | <span data-ttu-id="56db6-150">Határozza meg egy HTTP-válasz ellenőrzést hello mintavételi időkorlátját.</span><span class="sxs-lookup"><span data-stu-id="56db6-150">Defines hello probe time-out for an HTTP response check.</span></span>|
| <span data-ttu-id="56db6-151">**UnhealthyThreshold**</span><span class="sxs-lookup"><span data-stu-id="56db6-151">**UnhealthyThreshold**</span></span> | <span data-ttu-id="56db6-152">tooflag hello háttér-példány szükséges a sikertelen HTTP-válaszok száma hello *sérült*.</span><span class="sxs-lookup"><span data-stu-id="56db6-152">hello number of failed HTTP responses needed tooflag hello back-end instance as *unhealthy*.</span></span>|

<span data-ttu-id="56db6-153">hello mintavétel nevének hivatkozik hello \<BackendHttpSettings\> konfigurációs tooassign egyéni tesztműveleti beállításokat használja, mely háttér-készlet.</span><span class="sxs-lookup"><span data-stu-id="56db6-153">hello probe name is referenced in hello \<BackendHttpSettings\> configuration tooassign which back-end pool uses custom probe settings.</span></span>

## <a name="add-a-custom-probe-tooan-existing-application-gateway"></a><span data-ttu-id="56db6-154">Egyéni tesztműveleti tooan meglévő Alkalmazásátjáró hozzáadása</span><span class="sxs-lookup"><span data-stu-id="56db6-154">Add a custom probe tooan existing application gateway</span></span>

<span data-ttu-id="56db6-155">Változó hello aktuális konfigurációja Alkalmazásátjáró három lépésből áll: hello aktuális XML konfigurációs fájl beolvasása módosítani egy egyéni mintavételt toohave és hello Alkalmazásátjáró hello új XML-beállításainak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="56db6-155">Changing hello current configuration of an application gateway requires three steps: Get hello current XML configuration file, modify toohave a custom probe, and configure hello application gateway with hello new XML settings.</span></span>

1. <span data-ttu-id="56db6-156">Hello XML-fájl segítségével könnyebben nyerhet `Get-AzureApplicationGatewayConfig`.</span><span class="sxs-lookup"><span data-stu-id="56db6-156">Get hello XML file by using `Get-AzureApplicationGatewayConfig`.</span></span> <span data-ttu-id="56db6-157">Ez a parancsmag kivitel hello konfigurációs XML toobe módosítani tooadd mintavételi beállítást.</span><span class="sxs-lookup"><span data-stu-id="56db6-157">This cmdlet exports hello configuration XML toobe modified tooadd a probe setting.</span></span>

  ```powershell
  Get-AzureApplicationGatewayConfig -Name "<application gateway name>" -Exporttofile "<path toofile>"
  ```

1. <span data-ttu-id="56db6-158">Nyissa meg a hello XML-fájlt egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="56db6-158">Open hello XML file in a text editor.</span></span> <span data-ttu-id="56db6-159">Adja hozzá a `<probe>` után szakasz `<frontendport>`.</span><span class="sxs-lookup"><span data-stu-id="56db6-159">Add a `<probe>` section after `<frontendport>`.</span></span>

  ```xml
<Probes>
    <Probe>
        <Name>Probe01</Name>
        <Protocol>Http</Protocol>
        <Host>contoso.com</Host>
        <Path>/path/custompath.htm</Path>
        <Interval>15</Interval>
        <Timeout>15</Timeout>
        <UnhealthyThreshold>5</UnhealthyThreshold>
    </Probe>
</Probes>
  ```

  <span data-ttu-id="56db6-160">Hello XML hello backendhttpsettings beállítások területen hozzáadása hello mintavétel nevének, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="56db6-160">In hello backendHttpSettings section of hello XML, add hello probe name as shown in hello following example:</span></span>

  ```xml
    <BackendHttpSettings>
        <Name>setting1</Name>
        <Port>80</Port>
        <Protocol>Http</Protocol>
        <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        <RequestTimeout>120</RequestTimeout>
        <Probe>Probe01</Probe>
    </BackendHttpSettings>
  ```

  <span data-ttu-id="56db6-161">Hello XML-fájl mentése.</span><span class="sxs-lookup"><span data-stu-id="56db6-161">Save hello XML file.</span></span>

1. <span data-ttu-id="56db6-162">Frissítés hello alkalmazás átjáró konfigurációját hello új XML-fájl használatával `Set-AzureApplicationGatewayConfig`.</span><span class="sxs-lookup"><span data-stu-id="56db6-162">Update hello application gateway configuration with hello new XML file by using `Set-AzureApplicationGatewayConfig`.</span></span> <span data-ttu-id="56db6-163">Ez a parancsmag frissíti az Alkalmazásátjáró hello új konfigurációval.</span><span class="sxs-lookup"><span data-stu-id="56db6-163">This cmdlet updates your application gateway with hello new configuration.</span></span>

```powershell
Set-AzureApplicationGatewayConfig -Name "<application gateway name>" -Configfile "<path toofile>"
```

## <a name="next-steps"></a><span data-ttu-id="56db6-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="56db6-164">Next steps</span></span>

<span data-ttu-id="56db6-165">Ha azt szeretné, hogy Secure Sockets Layer (SSL)-kiszervezés tooconfigure, [konfigurálása az SSL-kiszervezés Alkalmazásátjáró](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="56db6-165">If you want tooconfigure Secure Sockets Layer (SSL) offload, see [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="56db6-166">Ha azt szeretné, hogy egy alkalmazás átjáró toouse belső terheléselosztással tooconfigure, [hozzon létre egy alkalmazást egy belső terheléselosztón (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="56db6-166">If you want tooconfigure an application gateway toouse with an internal load balancer, see [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

