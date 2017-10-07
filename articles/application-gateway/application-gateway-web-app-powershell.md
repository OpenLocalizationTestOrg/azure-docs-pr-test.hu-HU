---
title: "aaaProtect webalkalmazások Azure Application Gateway - PowerShell |} Microsoft Docs"
description: "Ez a cikk hogyan tooconfigure webes alkalmazásokat vissza end gazdagépek egy meglévő vagy új Alkalmazásátjáró nyújt útmutatást."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: 
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: gwallace
ms.openlocfilehash: 2e5d3ba9acc2f60499e7102961e631ee3d44eede
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-app-service-web-apps-with-application-gateway"></a><span data-ttu-id="f6a6f-103">App Service Web Apps-alkalmazások konfigurálása az Application Gatewayjel</span><span class="sxs-lookup"><span data-stu-id="f6a6f-103">Configure App Service Web Apps with Application Gateway</span></span> 

<span data-ttu-id="f6a6f-104">Alkalmazásátjáró lehetővé teszi az Azure Web Apps toohave vagy más több-bérlős szolgáltatást háttér-készlet tagja.</span><span class="sxs-lookup"><span data-stu-id="f6a6f-104">Application gateway allows you toohave an Azure Web App or other multi-tenant service as a back-end pool member.</span></span> <span data-ttu-id="f6a6f-105">Tooyou ebből a cikkből megtudhatja, tooconfigure Azure-webalkalmazás az Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="f6a6f-105">In this article, tooyou learn tooconfigure an Azure web app with Application Gateway.</span></span> <span data-ttu-id="f6a6f-106">hello első példa bemutatja, hogyan tooconfigure egy meglévő alkalmazás átjáró toouse háttér-készlet tagja egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f6a6f-106">hello first example shows you how tooconfigure an existing application gateway toouse a web app as a back-end pool member.</span></span> <span data-ttu-id="f6a6f-107">hello második példa bemutatja, hogyan toocreate egy új Alkalmazásátjáró háttér-készlet tagja webes alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="f6a6f-107">hello second example shows you how toocreate a new application gateway with a web app as a back-end pool member.</span></span>

## <a name="configure-a-web-app-behind-an-existing-application-gateway"></a><span data-ttu-id="f6a6f-108">Webalkalmazás konfigurálása meglévő Application Gateway mögött</span><span class="sxs-lookup"><span data-stu-id="f6a6f-108">Configure a web app behind an existing application gateway</span></span>

<span data-ttu-id="f6a6f-109">hello következő példakóddal felveheti a webes alkalmazás, egy háttér címkészletet tag tooan meglévő Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="f6a6f-109">hello following example adds a web app as a back-end pool member tooan existing application gateway.</span></span> <span data-ttu-id="f6a6f-110">Mindkét hello kapcsoló `-PickHostNamefromBackendHttpSettings`hello mintavételi konfiguráció és `-PickHostNameFromBackendAddress` hello háttér HTTP-beállításokat kell megadni ahhoz, hogy a webes alkalmazások toowork.</span><span class="sxs-lookup"><span data-stu-id="f6a6f-110">Both hello switch `-PickHostNamefromBackendHttpSettings`on hello Probe configuration and `-PickHostNameFromBackendAddress` on hello back-end http settings must be provided in order for web apps toowork.</span></span>

```powershell
# FQDN of hello web app
$webappFQDN = "<enter your webapp FQDN i.e mywebsite.azurewebsites.net>"

# Retrieve an existing application gateway
$gw = Get-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName $rg.ResourceGroupName

# Define hello status codes toomatch for hello probe
$match=New-AzureRmApplicationGatewayProbeHealthResponseMatch -StatusCode 200-399

# Add a new probe toohello application gateway
Add-AzureRmApplicationGatewayProbeConfig -name webappprobe2 -ApplicationGateway $gw -Protocol Http -Path / -Interval 30 -Timeout 120 -UnhealthyThreshold 3 -PickHostNameFromBackendHttpSettings -Match $match

# Retrieve hello newly added probe
$probe = Get-AzureRmApplicationGatewayProbeConfig -name webappprobe2 -ApplicationGateway $gw

# Configure an existing backend http settings 
Set-AzureRmApplicationGatewayBackendHttpSettings -Name appGatewayBackendHttpSettings -ApplicationGateway $gw -PickHostNameFromBackendAddress -Port 80 -Protocol http -CookieBasedAffinity Disabled -RequestTimeout 30 -Probe $probe

# Add hello web app toohello backend pool
Set-AzureRmApplicationGatewayBackendAddressPool -Name appGatewayBackendPool -ApplicationGateway $gw -BackendIPAddresses $webappFQDN

# Update hello application gateway
Set-AzureRmApplicationGateway -ApplicationGateway $gw
```

## <a name="configure-a-web-application-behind-a-new-application-gateway"></a><span data-ttu-id="f6a6f-111">Webalkalmazás konfigurálása új Application Gateway mögött</span><span class="sxs-lookup"><span data-stu-id="f6a6f-111">Configure a web application behind a new application gateway</span></span>

<span data-ttu-id="f6a6f-112">Ebben a forgatókönyvben az első lépések webhelyen és olyan átjárót hello asp.net webalkalmazás telepíti.</span><span class="sxs-lookup"><span data-stu-id="f6a6f-112">This scenario deploys a web app with hello asp.net getting started website and an application gateway.</span></span>

```powershell
# Defines a variable for a dotnet get started web app repository location
$gitrepo="https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git"

# Unique web app name
$webappname="mywebapp$(Get-Random)"

# Creates a resource group
$rg = New-AzureRmResourceGroup -Name ContosoRG -Location Eastus

# Create an App Service plan in Free tier.
New-AzureRmAppServicePlan -Name $webappname -Location EastUs -ResourceGroupName $rg.ResourceGroupName -Tier Free

# Creates a web app
$webapp = New-AzureRmWebApp -ResourceGroupName $rg.ResourceGroupName -Name $webappname -Location EastUs -AppServicePlan $webappname

# Configure GitHub deployment from your GitHub repo and deploy once tooweb app.
$PropertiesObject = @{
    repoUrl = "$gitrepo";
    branch = "master";
    isManualIntegration = "true";
}
Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName $rg.ResourceGroupName -ResourceType Microsoft.Web/sites/sourcecontrols -ResourceName $webappname/web -ApiVersion 2015-08-01 -Force

# Creates a subnet for hello application gateway
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

# Creates a vnet for hello application gateway
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName $rg.ResourceGroupName -Location EastUs -AddressPrefix 10.0.0.0/16 -Subnet $subnet

# Retrieve hello subnet object for use later
$subnet=$vnet.Subnets[0]

# Create a public IP address
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName $rg.ResourceGroupName -name publicIP01 -location EastUs -AllocationMethod Dynamic

# Create a new IP configuration
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

# Create a backend pool with hello hostname of hello web app
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name appGatewayBackendPool -BackendIPAddresses $webapp.HostNames

# Define hello status codes toomatch for hello probe
$match = New-AzureRmApplicationGatewayProbeHealthResponseMatch -StatusCode 200-399

# Create a probe with hello PickHostNameFromBackendHttpSettings switch for web apps
$probeconfig = New-AzureRmApplicationGatewayProbeConfig -name webappprobe -Protocol Http -Path / -Interval 30 -Timeout 120 -UnhealthyThreshold 3 -PickHostNameFromBackendHttpSettings -Match $match

# Define hello backend http settings
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name appGatewayBackendHttpSettings -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120 -PickHostNameFromBackendAddress -Probe $probeconfig

# Create a new front-end port
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

# Create a new front end IP configuration
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

# Create a new listener using hello front-end ip configuration and port created earlier
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

# Create a new rule
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool 

# Define hello application gateway SKU toouse
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

# Create hello application gateway
$appgw = New-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName $rg.ResourceGroupName -Location EastUs -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -Probes $probeconfig -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="f6a6f-113">Az Application Gateway DNS-nevének beszerzése</span><span class="sxs-lookup"><span data-stu-id="f6a6f-113">Get application gateway DNS name</span></span>

<span data-ttu-id="f6a6f-114">Hello átjáró létrehozása után hello következő lépésre tooconfigure hello előtér-kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="f6a6f-114">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="f6a6f-115">Nyilvános IP-cím esetén az Application Gateway használatához dinamikusan hozzárendelt DNS-névre van szükség, amely nem valódi név.</span><span class="sxs-lookup"><span data-stu-id="f6a6f-115">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="f6a6f-116">tooensure a végfelhasználók is találati hello Alkalmazásátjáró, egy olyan CNAME rekordot is használt toopoint toohello nyilvános végpontot hello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="f6a6f-116">tooensure end users can hit hello application gateway, a CNAME record can be used toopoint toohello public endpoint of hello application gateway.</span></span> <span data-ttu-id="f6a6f-117">toocreate hello alias, hello részleteit hello Alkalmazásátjáró és a társított IP-/ DNS-nevét, hello PublicIPAddress elem csatolt toohello Alkalmazásátjáró beolvasása.</span><span class="sxs-lookup"><span data-stu-id="f6a6f-117">toocreate hello alias, retrieve hello details of hello application gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello application gateway.</span></span> <span data-ttu-id="f6a6f-118">Ezt megteheti az Azure DNS- vagy más DNS-szolgáltatók által létrehozható egy olyan CNAME rekordot, amely mutat toohello [nyilvános IP-cím](../dns/dns-custom-domain.md#public-ip-address).</span><span class="sxs-lookup"><span data-stu-id="f6a6f-118">This can be done with Azure DNS or other DNS providers, by creating a CNAME record that points toohello [public IP address](../dns/dns-custom-domain.md#public-ip-address).</span></span> <span data-ttu-id="f6a6f-119">A-rekordok hello használata nem javasolt, mert hello VIP módosíthatja az Alkalmazásátjáró újra kell indítani.</span><span class="sxs-lookup"><span data-stu-id="f6a6f-119">hello use of A-records is not recommended since hello VIP may change on restart of application gateway.</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName ContosoRG -Name publicIP01
```

```
Name                     : publicIP01
ResourceGroupName        : ContosoRG
Location                 : eastus
Id                       : /subscriptions/<subscription_id>/resourceGroups/ContosoRG/providers/Microsoft.Network/publicIPAddresses/publicIP01
Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
ResourceGuid             : 00000000-0000-0000-0000-000000000000
ProvisioningState        : Succeeded
Tags                     : 
PublicIpAllocationMethod : Dynamic
IpAddress                : xx.xx.xxx.xx
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                                "Id": "/subscriptions/<subscription_id>/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/ContosoAppGateway/frontendIP
                            Configurations/frontend1"
                            }
DnsSettings              : {
                                "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                            }
```

## <a name="next-steps"></a><span data-ttu-id="f6a6f-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f6a6f-120">Next steps</span></span>

<span data-ttu-id="f6a6f-121">Megtudhatja, hogyan tooconfigure átirányítási ellátogatva: [átirányítás konfigurálása az alkalmazás-átjárón a PowerShell-lel](application-gateway-configure-redirect-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="f6a6f-121">Learn how tooconfigure redirection by visiting: [Configure redirection on Application Gateway with PowerShell](application-gateway-configure-redirect-powershell.md).</span></span>
