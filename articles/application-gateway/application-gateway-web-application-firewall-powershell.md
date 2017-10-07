---
title: "aaaConfigure webalkalmazási tűzfal - Azure Application Gateway |} Microsoft Docs"
description: "Ez a cikk hogyan toostart használatával webalkalmazási tűzfal a meglévő vagy új Alkalmazásátjáró nyújt útmutatást."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 670b9732-874b-43e6-843b-d2585c160982
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: gwallace
ms.openlocfilehash: bd2a69901f0ec0d6551d633e2588b74c3ab59a71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway"></a>Webalkalmazási tűzfal egy új vagy meglévő Alkalmazásátjáró konfigurálása

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Azure CLI](application-gateway-web-application-firewall-cli.md)

Ismerje meg, hogyan toocreate webalkalmazási tűzfal engedélyezve Application Gateway, vagy adja hozzá a webes alkalmazás tűzfal tooan meglévő Alkalmazásátjáró.

hello webalkalmazási tűzfal (waf-ot) az Azure alkalmazás átjáró webalkalmazások védje a közös web-alapú támadások, például az SQL-injektálás, a többhelyes parancsfájlok futtatására és a munkamenet kihasználásának.

Az Azure Application Gateway egy 7. rétegbeli terheléselosztó. Akár hello felhőalapú vagy helyszíni biztosít feladatátvételt, HTTP-kérelmek teljesítmény-útválasztási különböző kiszolgálók között. Alkalmazásátjáró biztosít számos alkalmazás kézbesítési vezérlő (LÉPETT) szolgáltatást HTTP beleértve betöltése terheléselosztási, a cookie-alapú munkamenet-kapcsolatot, és a secure sockets layer (SSL)-kiszervezés el egyéni állapotteljesítmény, többhelyes támogatása, és sok más. támogatott szolgáltatások teljes listáját toofind látogassa meg: [Alkalmazásátjáró áttekintése](application-gateway-introduction.md).

hello következő a cikk bemutatja, hogyan túl[hozzáadása a webes alkalmazás tűzfal tooan meglévő Alkalmazásátjáró](#add-web-application-firewall-to-an-existing-application-gateway) és [webalkalmazási tűzfal használó Alkalmazásátjáró létrehozása](#create-an-application-gateway-with-web-application-firewall).

![a forgatókönyv kép][scenario]

## <a name="waf-configuration-differences"></a>WAF konfigurációs különbségek

Ha mindenképpen olvassa el [Alkalmazásátjáró létrehozása a PowerShell használatával](application-gateway-create-gateway-arm.md), tisztában hello SKU beállítások tooconfigure Alkalmazásátjáró létrehozása során. WAF biztosít a további beállítások toodefine Alkalmazásátjáró hello SKU konfigurálásakor. Nincsenek további módosítások, amely akkor adja meg a hello Alkalmazásátjáró magát.

| **Beállítás** | **Részletek**
|---|---|
|**Termékváltozat** |Egy normál Alkalmazásátjáró nélkül WAF támogatja **szabványos\_kis**, **szabványos\_Közepes**, és **szabványos\_nagy** méretét. WAF hello bevezetését, a esetén két további SKU, **WAF\_Közepes** és **WAF\_nagy**. WAF kis Alkalmazásátjárót nem támogatott.|
|**Réteg** | hello elérhető értékek a következők **szabványos** vagy **WAF**. Webalkalmazási tűzfal, használatakor **WAF** ki kell választani.|
|**Mód** | Ez a beállítás akkor WAF hello módját. két érték engedélyezett **észlelési** és **megelőzési**. Ha WAF észlelési mód van beállítva, minden fenyegetések naplófájl vannak tárolva. Megelőzési módban eseményeket naplózza, de hello támadó 403-as jogosulatlan választ kap hello Alkalmazásátjáró.|

## <a name="add-web-application-firewall-tooan-existing-application-gateway"></a>Webes alkalmazás tűzfal tooan meglévő Alkalmazásátjáró hozzáadása

Győződjön meg arról, hogy azok be hello Azure PowerShell legújabb verzióját. További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).

1. Jelentkezzen be Azure-fiók tooyour.

    ```powershell
    Login-AzureRmAccount
    ```

2. Válassza ki a hello előfizetés toouse ehhez a forgatókönyvhöz.

    ```powershell
    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"
    ```

3. Webalkalmazási tűzfal a felvenni kívánt hello átjáró beolvasása.

    ```powershell
    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"
    ```

1. Hello webes alkalmazás tűzfal sku konfigurálása. hello elérhető értékek **WAF\_nagy** és **WAF\_Közepes**. Webalkalmazási tűzfal használata esetén a hello rétegnek különböznie kell **WAF**, meg kell erősíteni hello kapacitás, hello sku beállításakor.

    ```powershell
    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF -Capacity 2
    ```

1. Hello WAF beállítások konfigurálása a következő példa hello meghatározottak szerint:

   A **FirewallMode**, hello elérhető értékek a következők megelőzése és észlelését.

    ```powershell
    $gw | Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention
    ```

1. Frissítés hello Alkalmazásátjáró lépést megelőző hello definiált hello beállításokkal.

    ```powershell
    Set-AzureRmApplicationGateway -ApplicationGateway $gw
    ```

Ez a parancs webalkalmazási tűzfal hello Alkalmazásátjáró frissítése. Látogasson el [Alkalmazásátjáró diagnosztika](application-gateway-diagnostics.md) toounderstand, hogy miként naplózza az tooview az alkalmazás átjáró. WAF toohello biztonsági jellege miatt naplók kell toobe rendszeresen ellenőrizni a webalkalmazások toounderstand hello biztonsági állapotát.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Webalkalmazási tűzfal Alkalmazásátjáró létrehozása

hello következő lépések Alkalmazásátjáró webalkalmazási tűzfal való létrehozásának kezdő tooend hello teljes folyamatot.

Győződjön meg arról, hogy azok be hello Azure PowerShell legújabb verzióját. További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).

1. Jelentkezzen be tooAzure futtatásával `Login-AzureRmAccount`, rákérdezéses tooauthenticate áll a hitelesítő adataival.

1. Ellenőrizze a hello fiókhoz hello előfizetések futtatásával`Get-AzureRmSubscription`

1. Válassza ki, amely az Azure-előfizetések toouse.

    ```powershell
    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"
    ```

### <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Hozzon létre egy erőforráscsoportot hello Alkalmazásátjáró.

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet. Ezen a helyen az erőforráscsoport erőforrások lesz hello alapértelmezett helye. Győződjön meg arról, hogy az Application Gateway toocreate használja minden parancs hello ugyanabban az erőforráscsoportban.

A fenti példa hello létrehozott egy "Appgw-RG" és a hely "USA nyugati régiója." nevű erőforráscsoportban található

> [!NOTE]
> Ha egy egyéni mintavételt tooconfigure az Alkalmazásátjáró van szüksége, tekintse meg [PowerShell használatával hozzon létre olyan átjárót egyéni mintavételt](application-gateway-create-probe-ps.md). További információért lásd: [egyéni minták és állapotfigyelés](application-gateway-probe-overview.md).

### <a name="configure-virtual-network"></a>Virtuális hálózat konfigurálása

Alkalmazásátjáró saját alhálózat szükséges. Ebben a lépésben egy címtartománnyal 10.0.0.0/16 és két alhálózat, egy a hello Alkalmazásátjáró és egy háttér a készlet tagjainak a virtuális hálózat létrehozása.

```powershell
# Create a subnet configuration object for hello Application Gateway subnet. A subnet for an application should have a minimum of 28 mask bits. This value leaves 10 available addresses in hello subnet for Application Gateway instances. With a smaller subnet, you may not be able tooadd more instance of your Application Gateway in hello future.
$gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

# Create a subnet configuration object for hello backend pool members subnet
$nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

# Create hello virtual network with hello previous created subnets
$vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
```

### <a name="configure-public-ip-address"></a>Nyilvános IP-cím konfigurálása

Alkalmazásátjáró rendelés toohandle külső kérelmeket, egy nyilvános IP-cím használatához szükséges. A nyilvános IP-cím nem lehet egy `DomainNameLabel` definiált toobe hello Alkalmazásátjáró használják.

```powershell
# Create a public IP address for use with hello Application Gateway. Defining hello domainnamelabel during creation is not supported for use with Application Gateway
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic
```

### <a name="configure-hello-application-gateway"></a>Alkalmazásátjáró hello konfigurálása

```powershell
# Create a IP configuration. This configures what subnet hello Application Gateway uses. When Application Gateway starts, it picks up an IP address from hello subnet configured and routes network traffic toohello IP addresses in hello back-end IP pool.
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

# Create a backend pool toohold hello addresses or NICs for hello application that Application Gateway is protecting.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

# Upload hello authenication certificate that will be used toocommunicate with hello backend servers
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path too.cer file>

# Conifugre hello backend HTTP settings toobe used toodefine how traffic is routed toohello backend pool. hello authenication certificate used in hello previous step is added toohello backend http settings.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

# Create a frontend port toobe used by hello listener.
$fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

# Create a frontend IP configuration tooassociate hello public IP address with hello Application Gateway
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

# Configure hello certificate for hello Application Gateway. This certificate is used toodecrypt and re-encrypt hello traffic on hello Application Gateway.
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path too.pfx file> -Password <password for certificate file>

# Create hello HTTP listener for hello Application Gateway. Assign hello front-end ip configuration, port, and ssl certificate toouse.
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

#Create a load balancer routing rule that configures hello load balancer behavior. In this example, a basic round robin rule is created.
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Configure hello SKU of hello Application Gateway
$sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

# Define hello SSL policy toouse
$policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S

#Configure hello waf configuration settings.
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"

# Create hello Application Gateway utilizing all hello previously created configuration objects
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert
```

> [!NOTE]
> Hello alapvető webalkalmazás tűzfal konfigurációjának létre Alkalmazásátjárót védelmet CRS 3.0-val van állítva.

## <a name="get-application-gateway-dns-name"></a>Alkalmazás átjáró DNS-név beolvasása

Hello átjáró létrehozása után hello következő lépésre tooconfigure hello előtér-kommunikációhoz. Egy nyilvános IP-cím használata esetén az Application Gateway egy dinamikusan hozzárendelt DNS-nevet, amely nincs rövid van szükség. a végfelhasználók tooensure is találati hello Application Gateway, egy olyan CNAME rekordot is használt toopoint toohello nyilvános végpontja hello Application Gateway. [Egyéni tartománynév konfigurálása az Azure-ban](../cloud-services/cloud-services-custom-domain-name-portal.md). tooconfigure alias, hello Application Gateway és a társított IP-/ DNS-nevét, hello PublicIPAddress csatolt elem toohello Alkalmazásátjáró beolvasása. hello Application Gateway DNS-névnek kell lennie a használt toocreate egy olyan CNAME rekordot pontok hello két webes alkalmazások toothis DNS-név. A-rekordok hello használata nem ajánlott, óta hello VIP előfordulhat, hogy az Application Gateway újra kell indítani.

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

Megtudhatja, hogyan tooconfigure diagnosztikai naplózás, vagy látogasson el a webalkalmazási tűzfal megakadályozta toolog hello események [Alkalmazásdiagnosztika átjáró](application-gateway-diagnostics.md)

[scenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png
