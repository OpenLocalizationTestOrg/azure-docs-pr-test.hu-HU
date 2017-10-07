---
title: "egyéni aaaCreate mintavételi - Azure Application Gateway - PowerShell |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egyéni mintavételi az Alkalmazásátjáró PowerShell erőforrás-kezelő használatával"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 68feb660-7fa4-4f69-a7e4-bdd7bdc474db
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: gwallace
ms.openlocfilehash: 44c9ffa75401d6d0db023e66fa82c701fb0cf8bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-probe-for-azure-application-gateway-by-using-powershell-for-azure-resource-manager"></a>Egy egyéni mintavétel létrehozása az Azure Application Gateway az Azure Resource Manager PowerShell használatával.

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-probe-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-probe-ps.md)
> * [Klasszikus Azure PowerShell](application-gateway-create-probe-classic-ps.md)

Ebben a cikkben egy egyéni mintavételi tooan meglévő Alkalmazásátjáró a PowerShell használatával adja hozzá. Egyéni mintavételt hasznosak, az alkalmazásokat, amelyek egy adott állapotának ellenőrzése lapon vagy az alkalmazásokat, amelyek nem a sikeres válasz hello alapértelmezett webalkalmazáshoz.

> [!NOTE]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).  Ez a cikk ismerteti a használatával a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello hello Resource Manager üzembe helyezési modellben [klasszikus üzembe helyezési modellel](application-gateway-create-probe-classic-ps.md).

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway-with-a-custom-probe"></a>Hozzon létre egy alkalmazás egy egyéni mintavétel

### <a name="sign-in-and-create-resource-group"></a>Jelentkezzen be, és az erőforráscsoport létrehozása

1. Használjon `Login-AzureRmAccount` tooauthenticate.

  ```powershell
  Login-AzureRmAccount
  ```

1. Hello előfizetések hello fiók lekérése.

  ```powershell
  Get-AzureRmSubscription
  ```

1. Válassza ki, amely az Azure-előfizetések toouse.

  ```powershell
  Select-AzureRmSubscription -Subscriptionid '{subscriptionGuid}'
  ```

1. Hozzon létre egy erőforráscsoportot. Ezt a lépést kihagyhatja, ha egy meglévő erőforráscsoportot.

  ```powershell
  New-AzureRmResourceGroup -Name appgw-rg -Location 'West US'
  ```

Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet. Ezen a helyen az erőforráscsoport erőforrások lesz hello alapértelmezett helye. Győződjön meg arról, hogy az összes parancsok toocreate egy alkalmazás átjáró használata hello ugyanabban az erőforráscsoportban.

A fenti példa hello, nevű erőforráscsoport létrehozott **appgw-RG** helyen **USA nyugati régiója**.

### <a name="create-a-virtual-network-and-a-subnet"></a>Hozzon létre egy virtuális hálózatot és egy alhálózatot

hello alábbi példa létrehoz egy virtuális hálózatot és egy hello alkalmazás átjáró-alhálózatot. Alkalmazásátjáró saját alhálózatba használatát igényli. Emiatt az Alkalmazásátjáró hello létrehozott hello alhálózati kisebb, mint a más alhálózatokon toobe létrehozott és használt hello VNET tooallow hello címterében kell lennie.

```powershell
# Assign hello address range 10.0.0.0/24 tooa subnet variable toobe used toocreate a virtual network.
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

# Create a virtual network named appgwvnet in resource group appgw-rg for hello West US region using hello prefix 10.0.0.0/16 with subnet 10.0.0.0/24.
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet

# Assign a subnet variable for hello next steps, which create an application gateway.
$subnet = $vnet.Subnets[0]
```

### <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a>A nyilvános IP-cím hello előtér-konfiguráció létrehozása

Hozzon létre egy nyilvános IP-erőforrás **publicIP01** erőforráscsoportban **appgw-rg** hello USA nyugati régiójában. A példa egy nyilvános IP-cím hello Alkalmazásátjáró hello előtér-IP-címhez.  Alkalmazás-átjáró által igényelt hello nyilvános IP cím toohave dinamikusan létrehozott DNS-név ezért hello `-DomainNameLabel` hello hello nyilvános IP-cím létrehozása közben nem adható meg.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name publicIP01 -Location 'West US' -AllocationMethod Dynamic
```

### <a name="create-an-application-gateway"></a>Application Gateway létrehozása

Az összes konfigurációs elemek beállítása hello Alkalmazásátjáró létrehozása előtt. hello alábbi példakód létrehozza hello konfigurációs elemek, amelyek szükségesek az alkalmazás átjáró-erőforráshoz.

| **Összetevő** | **Leírás** |
|---|---|
| **Átjáró IP-konfiguráció** | Egy alkalmazás átjáró IP-konfigurációt.|
| **Háttérkészlet** | IP-címek, FQDN-eknek vagy hálózati adapterrel, amelyek toohello alkalmazás hello webalkalmazást működtető kiszolgálók készlete|
| **Állapotmintáihoz** | Egyéni tesztműveleti használt hello háttér a készlet tagjainak toomonitor hello állapotát|
| **HTTP-beállítások** | Beleértve, port, protokoll, affinitási cookie-alapú, mintavételi és időtúllépés gyűjteménye.  Ezeket a beállításokat annak megállapítása, hogy forgalom irányított toohello háttér a készlet tagjainak|
| **Az elülső rétegbeli portot** | Alkalmazásátjáró hello hello portot figyeli a forgalmat a|
| **Figyelő** | A protokoll előtérbeli IP-konfigurációja és az elülső rétegbeli portot kombinációja. Ez az, hogy mi a bejövő kéréseket figyeli.
|**A szabály**| Útvonalak hello forgalom toohello megfelelő háttér HTTP-beállításai alapján.|

```powershell
# Creates a application gateway Frontend IP configuration named gatewayIP01
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

#Creates a back-end IP address pool named pool01 with IP addresses 134.170.185.46, 134.170.188.221, 134.170.185.50.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

# Creates a probe that will check health at http://contoso.com/path/path.htm
$probe = New-AzureRmApplicationGatewayProbeConfig -Name probe01 -Protocol Http -HostName 'contoso.com' -Path '/path/path.htm' -Interval 30 -Timeout 120 -UnhealthyThreshold 8

# Creates hello backend http settings toobe used. This component references hello $probe created in hello previous command.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 80

# Creates a frontend port for hello application gateway toolisten on port 80 that will be used by hello listener.
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01 -Port 80

# Creates a frontend IP configuration. This associates hello $publicip variable defined previously with hello front-end IP that will be used by hello listener.
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

# Creates hello listener. hello listener is a combination of protocol and hello frontend IP configuration $fipconfig and frontend port $fp created in previous steps.
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

# Creates hello rule that routes traffic toohello backend pools.  In this example we create a basic rule that uses hello previous defined http settings and backend address pool.  It also associates hello listener toohello rule
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Sets hello SKU of hello application gateway, in this example we create a small standard application gateway with 2 instances.
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

# hello final step creates hello application gateway with all hello previously defined components.
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location 'West US' -BackendAddressPools $pool -Probes $probe -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

## <a name="add-a-probe-tooan-existing-application-gateway"></a>A mintavétel tooan meglévő Alkalmazásátjáró hozzáadása

hello következő kódrészletet mintavételi tooan meglévő alkalmazás átjárót ad.

```powershell
# Load hello application gateway resource into a PowerShell variable by using Get-AzureRmApplicationGateway.
$getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

# Create hello probe object that will check health at http://contoso.com/path/path.htm
$getgw = Add-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name probe01 -Protocol Http -HostName 'contoso.com' -Path '/path/custompath.htm' -Interval 30 -Timeout 120 -UnhealthyThreshold 8

# Set hello backend HTTP settings toouse hello new probe
$getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 120

# Save hello application gateway with hello configuration changes
Set-AzureRmApplicationGateway -ApplicationGateway $getgw
```

## <a name="remove-a-probe-from-an-existing-application-gateway"></a>Távolítsa el a Hálózatfigyelő a meglévő Alkalmazásátjáró

a következő kódrészletet hello vizsgálatok eltávolítja a meglévő Alkalmazásátjáró.

```powershell
# Load hello application gateway resource into a PowerShell variable by using Get-AzureRmApplicationGateway.
$getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

# Remove hello probe from hello application gateway configuration object
$getgw = Remove-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name $getgw.Probes.name

# Set hello backend HTTP settings tooremove hello reference toohello probe. hello backend http settings now use hello default probe
$getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol http -CookieBasedAffinity Disabled

# Save hello application gateway with hello configuration changes
Set-AzureRmApplicationGateway -ApplicationGateway $getgw
```

## <a name="get-application-gateway-dns-name"></a>Az Application Gateway DNS-nevének beszerzése

Hello átjáró létrehozása után hello következő lépésre tooconfigure hello előtér-kommunikációhoz. Nyilvános IP-cím esetén az Application Gateway használatához dinamikusan hozzárendelt DNS-névre van szükség, amely nem valódi név. tooensure végfelhasználók hello Alkalmazásátjáró használhatók egy olyan CNAME rekordot is találati toopoint toohello nyilvános végpontot hello Alkalmazásátjáró. [Egyéni tartománynév konfigurálása az Azure-ban](../cloud-services/cloud-services-custom-domain-name-portal.md). toodo a, hello Alkalmazásátjáró és a társított IP-/ DNS-nevét, hello PublicIPAddress elem csatolt toohello Alkalmazásátjáró beolvasása részleteit. hello alkalmazás átjáró DNS-névnek kell lennie a használt toocreate egy olyan CNAME rekordot pontok hello két webes alkalmazások toothis DNS-név. A-rekordok hello használata nem javasolt, mert hello VIP módosíthatja az Alkalmazásátjáró újra kell indítani.

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

## <a name="next-steps"></a>Következő lépések

Ismerje meg, tooconfigure SSL kiszervezésével ellátogatva: [SSL-kiszervezés konfigurálása](application-gateway-ssl-arm.md)

