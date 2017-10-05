---
title: "Hozzon létre egy egyéni mintavétel - Azure Application Gateway - klasszikus PowerShell |} Microsoft Docs"
description: "Megtudhatja, hogyan hozzon létre egy egyéni mintavétel az Alkalmazásátjáró a klasszikus üzembe helyezési modellel PowerShell használatával"
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
ms.openlocfilehash: bf190741b10c10e885d927ad21a9f2b25107943f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-custom-probe-for-azure-application-gateway-classic-by-using-powershell"></a><span data-ttu-id="156a5-103">Hozzon létre egy egyéni mintavétel az Azure Application Gateway (klasszikus) PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="156a5-103">Create a custom probe for Azure Application Gateway (classic) by using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="156a5-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="156a5-104">Azure portal</span></span>](application-gateway-create-probe-portal.md)
> * [<span data-ttu-id="156a5-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="156a5-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-probe-ps.md)
> * [<span data-ttu-id="156a5-106">Klasszikus Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="156a5-106">Azure Classic PowerShell</span></span>](application-gateway-create-probe-classic-ps.md)

<span data-ttu-id="156a5-107">Ebben a cikkben ad hozzá egy egyéni mintavételt meglévő Alkalmazásátjáró a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="156a5-107">In this article, you add a custom probe to an existing application gateway with PowerShell.</span></span> <span data-ttu-id="156a5-108">Egyéni mintavételt az alkalmazásokat, amelyek egy adott állapotának ellenőrzése lapon vagy az alapértelmezett webes alkalmazás a sikeres válasz nem biztosító alkalmazások hasznosak.</span><span class="sxs-lookup"><span data-stu-id="156a5-108">Custom probes are useful for applications that have a specific health check page or for applications that do not provide a successful response on the default web application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="156a5-109">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="156a5-109">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="156a5-110">Ez a cikk a klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="156a5-110">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="156a5-111">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="156a5-111">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="156a5-112">Ismerje meg, [hogyan hajthatja végre ezeket a lépéseket a Resource Manager-modell használatával](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="156a5-112">Learn how to [perform these steps using the Resource Manager model](application-gateway-create-probe-ps.md).</span></span>

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway"></a><span data-ttu-id="156a5-113">Application Gateway létrehozása</span><span class="sxs-lookup"><span data-stu-id="156a5-113">Create an application gateway</span></span>

<span data-ttu-id="156a5-114">Application Gateway létrehozásához tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="156a5-114">To create an application gateway:</span></span>

1. <span data-ttu-id="156a5-115">Egy Application Gateway erőforrás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="156a5-115">Create an application gateway resource.</span></span>
2. <span data-ttu-id="156a5-116">Hozzon létre egy konfigurációs XML-fájlt vagy konfigurációs objektumot.</span><span class="sxs-lookup"><span data-stu-id="156a5-116">Create a configuration XML file or a configuration object.</span></span>
3. <span data-ttu-id="156a5-117">Véglegesítse az újonnan létrehozott Application Gateway erőforrás konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="156a5-117">Commit the configuration to the newly created application gateway resource.</span></span>

### <a name="create-an-application-gateway-resource-with-a-custom-probe"></a><span data-ttu-id="156a5-118">Egy egyéni mintavételi alkalmazás átjáró erőforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="156a5-118">Create an application gateway resource with a custom probe</span></span>

<span data-ttu-id="156a5-119">Az átjáró létrehozásához használja a `New-AzureApplicationGateway` parancsmagot, és cserélje le az értékeket a saját értékeire.</span><span class="sxs-lookup"><span data-stu-id="156a5-119">To create the gateway, use the `New-AzureApplicationGateway` cmdlet, replacing the values with your own.</span></span> <span data-ttu-id="156a5-120">Az átjáró használati díjának felszámolása ekkor még nem kezdődik el.</span><span class="sxs-lookup"><span data-stu-id="156a5-120">Billing for the gateway does not start at this point.</span></span> <span data-ttu-id="156a5-121">A használati díj felszámolása egy későbbi lépésnél kezdődik, amikor az átjáró sikeresen elindul.</span><span class="sxs-lookup"><span data-stu-id="156a5-121">Billing begins in a later step, when the gateway is successfully started.</span></span>

<span data-ttu-id="156a5-122">Az alábbi példa egy új Application Gateway-t hoz létre egy „testvnet1” nevű virtuális hálózat és egy „subnet-1” nevű alhálózat használatával.</span><span class="sxs-lookup"><span data-stu-id="156a5-122">The following example creates an application gateway by using a virtual network called "testvnet1" and a subnet called "subnet-1".</span></span>

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

<span data-ttu-id="156a5-123">Az átjáró létrehozásának ellenőrzéséhez használhatja a `Get-AzureApplicationGateway` parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="156a5-123">To validate that the gateway was created, you can use the `Get-AzureApplicationGateway` cmdlet.</span></span>

```powershell
Get-AzureApplicationGateway AppGwTest
```

> [!NOTE]
> <span data-ttu-id="156a5-124">Az *InstanceCount* alapértelmezett értéke 2, a maximális értéke pedig 10.</span><span class="sxs-lookup"><span data-stu-id="156a5-124">The default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="156a5-125">A *GatewaySize* alapértelmezett értéke Közepes.</span><span class="sxs-lookup"><span data-stu-id="156a5-125">The default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="156a5-126">Kis, közepes és nagy közül választhat.</span><span class="sxs-lookup"><span data-stu-id="156a5-126">You can choose between Small, Medium, and Large.</span></span>
> 
> 

<span data-ttu-id="156a5-127">A *VirtualIPs* és a *DnsName* paraméterek azért üresek, mert az átjáró még nem indult el.</span><span class="sxs-lookup"><span data-stu-id="156a5-127">*VirtualIPs* and *DnsName* are shown as blank because the gateway has not started yet.</span></span> <span data-ttu-id="156a5-128">Ezek az értékek jönnek létre, ha az átjáró már szerepel a futó állapotot.</span><span class="sxs-lookup"><span data-stu-id="156a5-128">These values are created once the gateway is in the running state.</span></span>

### <a name="configure-an-application-gateway-by-using-xml"></a><span data-ttu-id="156a5-129">Alkalmazásátjáró konfigurálása XML használatával</span><span class="sxs-lookup"><span data-stu-id="156a5-129">Configure an application gateway by using XML</span></span>

<span data-ttu-id="156a5-130">Az alábbi példában egy XML-fájllal konfigurálja az Application Gateway beállításait, és véglegesíti őket az Application Gateway-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="156a5-130">In the following example, you use an XML file to configure all application gateway settings and commit them to the application gateway resource.</span></span>  

<span data-ttu-id="156a5-131">Másolja az alábbi szöveget a Jegyzettömbbe.</span><span class="sxs-lookup"><span data-stu-id="156a5-131">Copy the following text to Notepad.</span></span>

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

<span data-ttu-id="156a5-132">Szerkessze a zárójelek közötti értékeket a konfigurációs elemeknek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="156a5-132">Edit the values between the parentheses for the configuration items.</span></span> <span data-ttu-id="156a5-133">Mentse a fájlt .xml kiterjesztéssel.</span><span class="sxs-lookup"><span data-stu-id="156a5-133">Save the file with extension .xml.</span></span>

<span data-ttu-id="156a5-134">A következő példa bemutatja, hogyan egy konfigurációs fájl használatával állítsa be a nyilvános port 80-as HTTP-forgalom és a hálózati forgalom elküldése a háttér-két IP-címre egy egyéni mintavételt a 80-as port az Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="156a5-134">The following example shows how to use a configuration file to set up the application gateway to load balance HTTP traffic on public port 80 and send network traffic to back-end port 80 between two IP addresses by using a custom probe.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="156a5-135">A Http és Https protokollelem különbséget tesz a kis- és a nagybetűk között.</span><span class="sxs-lookup"><span data-stu-id="156a5-135">The protocol item Http or Https is case-sensitive.</span></span>

<span data-ttu-id="156a5-136">Egy új konfigurációelemet \<mintavételi\> konfigurálása egyéni mintavételt kerül.</span><span class="sxs-lookup"><span data-stu-id="156a5-136">A new configuration item \<Probe\> is added to configure custom probes.</span></span>

<span data-ttu-id="156a5-137">A konfigurációs paraméterek a következők:</span><span class="sxs-lookup"><span data-stu-id="156a5-137">The configuration parameters are:</span></span>

|<span data-ttu-id="156a5-138">Paraméter</span><span class="sxs-lookup"><span data-stu-id="156a5-138">Parameter</span></span>|<span data-ttu-id="156a5-139">Leírás</span><span class="sxs-lookup"><span data-stu-id="156a5-139">Description</span></span>|
|---|---|
|<span data-ttu-id="156a5-140">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="156a5-140">**Name**</span></span> |<span data-ttu-id="156a5-141">Egyéni mintavétel hivatkozás neve.</span><span class="sxs-lookup"><span data-stu-id="156a5-141">Reference name for custom probe.</span></span> |
<span data-ttu-id="156a5-142">* **Protokoll**</span><span class="sxs-lookup"><span data-stu-id="156a5-142">* **Protocol**</span></span> | <span data-ttu-id="156a5-143">Használt protokoll (a lehetséges értékek: HTTP vagy HTTPS).</span><span class="sxs-lookup"><span data-stu-id="156a5-143">Protocol used (possible values are HTTP or HTTPS).</span></span>|
| <span data-ttu-id="156a5-144">**Állomás** és **elérési útja**</span><span class="sxs-lookup"><span data-stu-id="156a5-144">**Host** and **Path**</span></span> | <span data-ttu-id="156a5-145">Fejezze be az URL-címet, amelyet az Alkalmazásátjáró példány állapotának meghatározására.</span><span class="sxs-lookup"><span data-stu-id="156a5-145">Complete URL path that is invoked by the application gateway to determine the health of the instance.</span></span> <span data-ttu-id="156a5-146">Például ha egy webhely http://contoso.com/, majd az egyéni vizsgálat is konfigurálható "http://contoso.com/path/custompath.htm" mintavételi ellenőrzi, hogy rendelkezik a sikeres HTTP-válasz.</span><span class="sxs-lookup"><span data-stu-id="156a5-146">For example, if you have a website http://contoso.com/, then the custom probe can be configured for "http://contoso.com/path/custompath.htm" for probe checks to have a successful HTTP response.</span></span>|
| <span data-ttu-id="156a5-147">**Időköz**</span><span class="sxs-lookup"><span data-stu-id="156a5-147">**Interval**</span></span> | <span data-ttu-id="156a5-148">Konfigurálja a mintavételi intervallum ellenőrzések másodpercben.</span><span class="sxs-lookup"><span data-stu-id="156a5-148">Configures the probe interval checks in seconds.</span></span>|
| <span data-ttu-id="156a5-149">**Időtúllépés**</span><span class="sxs-lookup"><span data-stu-id="156a5-149">**Timeout**</span></span> | <span data-ttu-id="156a5-150">Egy HTTP-válasz ellenőrzése mintavételi időtúllépésének meghatározása.</span><span class="sxs-lookup"><span data-stu-id="156a5-150">Defines the probe time-out for an HTTP response check.</span></span>|
| <span data-ttu-id="156a5-151">**UnhealthyThreshold**</span><span class="sxs-lookup"><span data-stu-id="156a5-151">**UnhealthyThreshold**</span></span> | <span data-ttu-id="156a5-152">A szükséges jelzőt a háttér-példányához, a sikertelen HTTP-válaszok száma *sérült*.</span><span class="sxs-lookup"><span data-stu-id="156a5-152">The number of failed HTTP responses needed to flag the back-end instance as *unhealthy*.</span></span>|

<span data-ttu-id="156a5-153">A mintavétel nevének hivatkozik a \<BackendHttpSettings\> konfigurációját, és rendeljen mely háttér címkészletet egyéni tesztműveleti beállításokat használja.</span><span class="sxs-lookup"><span data-stu-id="156a5-153">The probe name is referenced in the \<BackendHttpSettings\> configuration to assign which back-end pool uses custom probe settings.</span></span>

## <a name="add-a-custom-probe-to-an-existing-application-gateway"></a><span data-ttu-id="156a5-154">Egyéni tesztműveleti hozzáadása egy meglévő Alkalmazásátjáró</span><span class="sxs-lookup"><span data-stu-id="156a5-154">Add a custom probe to an existing application gateway</span></span>

<span data-ttu-id="156a5-155">Az Alkalmazásátjáró a jelenlegi konfiguráció módosításához szükséges három lépés: az aktuális XML konfigurációs fájl beolvasása módosítani szeretné, hogy egy egyéni mintavételt és az új XML-beállítások konfigurálása az Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="156a5-155">Changing the current configuration of an application gateway requires three steps: Get the current XML configuration file, modify to have a custom probe, and configure the application gateway with the new XML settings.</span></span>

1. <span data-ttu-id="156a5-156">Az XML-fájl segítségével könnyebben nyerhet `Get-AzureApplicationGatewayConfig`.</span><span class="sxs-lookup"><span data-stu-id="156a5-156">Get the XML file by using `Get-AzureApplicationGatewayConfig`.</span></span> <span data-ttu-id="156a5-157">Ez a parancsmag exportálja a konfigurációs XML-kód módosítani kell hozzáadni a mintavételi beállítást.</span><span class="sxs-lookup"><span data-stu-id="156a5-157">This cmdlet exports the configuration XML to be modified to add a probe setting.</span></span>

  ```powershell
  Get-AzureApplicationGatewayConfig -Name "<application gateway name>" -Exporttofile "<path to file>"
  ```

1. <span data-ttu-id="156a5-158">Nyissa meg az XML-fájlt egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="156a5-158">Open the XML file in a text editor.</span></span> <span data-ttu-id="156a5-159">Adja hozzá a `<probe>` után szakasz `<frontendport>`.</span><span class="sxs-lookup"><span data-stu-id="156a5-159">Add a `<probe>` section after `<frontendport>`.</span></span>

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

  <span data-ttu-id="156a5-160">Az XML a backendhttpsettings beállítások területen adja hozzá a mintavétel nevének a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="156a5-160">In the backendHttpSettings section of the XML, add the probe name as shown in the following example:</span></span>

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

  <span data-ttu-id="156a5-161">Mentse az XML-fájlt.</span><span class="sxs-lookup"><span data-stu-id="156a5-161">Save the XML file.</span></span>

1. <span data-ttu-id="156a5-162">Frissítse az alkalmazás átjáró konfigurációját az új XML-fájl használatával `Set-AzureApplicationGatewayConfig`.</span><span class="sxs-lookup"><span data-stu-id="156a5-162">Update the application gateway configuration with the new XML file by using `Set-AzureApplicationGatewayConfig`.</span></span> <span data-ttu-id="156a5-163">Ez a parancsmag az Alkalmazásátjáró frissítése az új konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="156a5-163">This cmdlet updates your application gateway with the new configuration.</span></span>

```powershell
Set-AzureApplicationGatewayConfig -Name "<application gateway name>" -Configfile "<path to file>"
```

## <a name="next-steps"></a><span data-ttu-id="156a5-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="156a5-164">Next steps</span></span>

<span data-ttu-id="156a5-165">Ha a Secure Sockets Layer (SSL) kiszervezési konfigurálni szeretné, lásd: [konfigurálása az SSL-kiszervezés Alkalmazásátjáró](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="156a5-165">If you want to configure Secure Sockets Layer (SSL) offload, see [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="156a5-166">Ha konfigurálni szeretne egy ILB-vel használni kívánt Application Gateway-t: [Application Gateway létrehozása belső terheléselosztóval (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="156a5-166">If you want to configure an application gateway to use with an internal load balancer, see [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

