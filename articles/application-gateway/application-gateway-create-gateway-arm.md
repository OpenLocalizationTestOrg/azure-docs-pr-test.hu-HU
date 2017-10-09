---
title: "aaaCreate és kezelése az Azure Application Gateway - PowerShell |} Microsoft Docs"
description: "Ezen a lapon útmutatás toocreate, konfigurálása, indítsa el, és Azure Alkalmazásátjáró törlése az Azure Resource Manager használatával"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: ab98d5f9aa0dc309f8353b7f72591359e1121849
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a>Application Gateway létrehozása, indítása vagy törlése az Azure Resource Manager használatával

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Klasszikus Azure PowerShell](application-gateway-create-gateway.md)
> * [Azure Resource Manager-sablon](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI](application-gateway-create-gateway-cli.md)

Az Azure Application Gateway egy 7. rétegbeli terheléselosztó. Azt is biztosít feladatátvételi és HTTP-kérelmek teljesítmény-útválasztási különböző kiszolgálók között hello felhőalapú vagy helyszíni legyenek. Az Application Gateway számos alkalmazáskézbesítési vezérlőszolgáltatást (ADC) biztosít, beleértve a HTTP-terheléselosztást, a cookie-alapú munkamenet-affinitást, a Secure Sockets Layer- (SSL-) alapú kiszervezést, az egyéni állapotmintákat, a többhelyes támogatást és még sok mást. látogasson el a támogatott funkciók teljes listáját toofind [Alkalmazásátjáró áttekintése](application-gateway-introduction.md).

Ez a cikk bemutatja, hogyan hello lépéseket toocreate, konfigurálása, indítsa el és Alkalmazásátjáró törlése.

> [!IMPORTANT]
> Azure-erőforrások használata előtt-e, hogy Azure jelenleg két üzembe helyezési modellel rendelkezik fontos toounderstand: Resource Manager és klasszikus. Bizonyosodjon meg arról, hogy megfelelő ismeretekkel rendelkezik az [üzembe helyezési modellekről és eszközökről](../azure-classic-rm.md), mielőtt elkezdene dolgozni az Azure-erőforrásokkal. Ez a cikk hello tetején hello fülekre kattintva megtekintheti a különféle eszközök dokumentációit hello. Jelen dokumentum az Application Gateway az Azure Resource Manager használatával történő létrehozását ismerteti. toouse hello klasszikus verzióra, nyissa meg túl[egy alkalmazás átjáró klasszikus üzembe helyezési létrehozása a PowerShell használatával](application-gateway-create-gateway.md).

## <a name="before-you-begin"></a>Előkészületek

1. Webplatform-telepítő hello segítségével hello hello Azure PowerShell-parancsmagok legújabb verzióját telepítse. Töltse le, és telepítse hello legújabb verziót a hello **Windows PowerShell** hello szakasza [letöltési oldalon](https://azure.microsoft.com/downloads/).
1. Ha egy meglévő virtuális hálózattal rendelkezik, válasszon egy létező üres alhálózatot, vagy hozzon létre egy alhálózatot a meglévő virtuális hálózatban kizárólag használatra hello Alkalmazásátjáró által. Nem telepíthet központilag hello alkalmazás átjáró tooa másik virtuális hálózatot hello erőforrások mint mögött hello Alkalmazásátjáró toodeploy szeretné.
1. hello kiszolgálók toouse hello Alkalmazásátjáró konfigurált léteznie kell, különben a végpontok hello virtuális hálózatban vagy a nyilvános IP-cím/VIP rendelt hozzá.

## <a name="what-is-required-toocreate-an-application-gateway"></a>Mi az a szükséges toocreate olyan átjárót?

* **Kiszolgáló háttérkészlet:** IP-címek, teljes tartománynevek vagy hálózati adaptert a hello háttérkiszolgálók hello listája. IP-címek használata esetén toohello virtuális hálózati alhálózat vagy kell tartoznia, vagy egy nyilvános IP-cím/VIP kell lennie.
* **Háttérkiszolgáló-készlet beállításai:** Minden készlet rendelkezik olyan beállításokkal, mint a port, a protokoll vagy a cookie-alapú affinitás. Ezek a beállítások esetén tooa kapcsolt verem és a hello készlet alkalmazott tooall-kiszolgálók.
* **az elülső rétegbeli portot:** Ez a port nem hello nyilvános portot, amelyet a hello Alkalmazásátjáró meg van nyitva. Forgalom találatok ezt a portot, és ezután lekérdezi a hello háttérkiszolgálók átirányított tooone.
* **Figyelő:** hello figyelő rendelkezik egy elülső rétegbeli portot, a protokollt (Http vagy Https, ezek az értékek kis-és nagybetűket), és hello SSL tanúsítvány neve (ha az SSL beállításának-kiszervezés).
* **Szabály:** hello szabály hello figyelő, a kiszolgáló háttérkészlet hello van kötve, és azt határozza meg, melyik háttér címkészletet hello forgalom irányított toowhen találatok száma a egy adott figyelő.

## <a name="create-a-resource-group-for-resource-manager"></a>Erőforráscsoport létrehozása a Resource Managerhez

Győződjön meg arról, hogy azok be hello Azure PowerShell legújabb verzióját. További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).

1. Jelentkezzen be tooAzure, és adja meg a hitelesítő adatokat.

  ```powershell
  Login-AzureRmAccount
  ```

2. Hello előfizetések hello fiók ellenőrzése.

  ```powershell
  Get-AzureRmSubscription
  ```

3. Válassza ki, amely az Azure-előfizetések toouse.

  ```powershell
  Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
  ```

4. Hozzon létre egy erőforráscsoportot (hagyja ki ezt a lépést, ha egy meglévő erőforráscsoportot használ).

  ```powershell
  New-AzureRmResourceGroup -Name ContosoRG -Location "West US"
  ```

Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet. Ezen a helyen az erőforráscsoport erőforrások lesz hello alapértelmezett helye. Győződjön meg arról, hogy olyan átjárót használó összes parancsok toocreate hello azonos erőforráscsoportot.

A hello a fenti példában létrehozott nevű erőforráscsoport **ContosoRG** és a hely **USA keleti régiója**.

> [!NOTE]
> Ha egy egyéni mintavételt tooconfigure az Alkalmazásátjáró van szüksége, látogasson el: [PowerShell használatával hozzon létre olyan átjárót egyéni mintavételt](application-gateway-create-probe-ps.md). További információért lásd: [egyéni minták és állapotfigyelés](application-gateway-probe-overview.md).


## <a name="create-hello-application-gateway-configuration-objects"></a>Konfigurációs objektumok hello Alkalmazásátjáró létrehozása

Az összes konfigurációs elemek hello Alkalmazásátjáró létrehozása előtt kell beállítani. hello lépések hello konfigurációs elemek létrehozását, amelyek szükségesek az alkalmazás átjáró-erőforráshoz.

```powershell
# Create a subnet and assign hello address space of 10.0.0.0/24
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

# Create a virtual network with hello address space of 10.0.0.0/16 and add hello subnet
$vnet = New-AzureRmVirtualNetwork -Name ContosoVNET -ResourceGroupName ContosoRG -Location "East US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

# Retrieve hello newly created subnet
$subnet=$vnet.Subnets[0]

# Create a public IP address that is used tooconnect toohello application gateway. Application Gateway does not support custom DNS names on public IP addresses.  If a custom name is required for hello public endpoint, a CNAME record should be created toopoint toohello automatically generated DNS name for hello public IP address.
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName ContosoRG -name publicIP01 -location "East US" -AllocationMethod Dynamic

# Create a gateway IP configuration. hello gateway picks up an IP addressfrom hello configured subnet and routes network traffic toohello IP addresses in hello backend IP pool. Keep in mind that each instance takes one IP address.
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

# Configure a backend pool with hello addresses of your web servers. These backend pool members are all validated toobe healthy by probes, whether they are basic probes or custom probes.  Traffic is then routed toothem when requests come into hello application gateway. Backend pools can be used by multiple rules within hello application gateway, which means one backend pool could be used for multiple web applications that reside on hello same host.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

# Configure backend http settings toodetermine hello protocol and port that is used when sending traffic toohello backend servers. Cookie-based sessions are also determined by hello backend HTTP settings.  If enabled, cookie-based session affinity sends traffic toohello same backend as previous requests for each packet.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120

# Configure a frontend port that is used tooconnect toohello application gateway through hello public IP address
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

# Configure hello frontend IP configuration with hello public IP address created earlier.
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

# Configure hello listener.  hello listener is a combination of hello front end IP configuration, protocol, and port and is used tooreceive incoming network traffic. 
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

# Configure a basic rule that is used tooroute traffic toohello backend servers. hello backend pool settings, listener, and backend pool created in hello previous steps make up hello rule. Based on hello criteria defined traffic is routed toohello appropriate backend.
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Configure hello SKU for hello application gateway, this determines hello size and whether or not WAF is used.
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

# Create hello application gateway
$appgw = New-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG -Location "East US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

Amikor végzett, DNS- és VIP részleteinek hello Alkalmazásátjáró hello nyilvános IP-erőforrás csatolt toohello alkalmazás átjáró lekérése.

```powershell
Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName ContosoRG
```

## <a name="delete-hello-application-gateway"></a>Hello Alkalmazásátjáró törlése

a következő példa törlések hello Alkalmazásátjáró hello.

```powershell
# Retrieve hello application gateway
$gw = Get-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG

# Stops hello application gateway
Stop-AzureRmApplicationGateway -ApplicationGateway $gw

# Once hello application gateway is in a stopped state, use hello `Remove-AzureRmApplicationGateway` cmdlet tooremove hello service.
Remove-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG -Force
```

> [!NOTE]
> Hello **-force** kapcsoló lehet használt toosuppress hello eltávolítása jóváhagyást kérő üzenet.

tooverify, amely hello szolgáltatás el lett távolítva, használhatja a hello `Get-AzureRmApplicationGateway` parancsmag. Ez a lépés nem kötelező.

```powershell
Get-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG
```

## <a name="get-application-gateway-dns-name"></a>Az Application Gateway DNS-nevének beszerzése

Hello átjáró létrehozása után hello következő lépésre tooconfigure hello előtér-kommunikációhoz. Nyilvános IP-cím esetén az Application Gateway használatához dinamikusan hozzárendelt DNS-névre van szükség, amely nem valódi név. tooensure a végfelhasználók is találati hello Alkalmazásátjáró, egy olyan CNAME rekordot is használt toopoint toohello nyilvános végpontot hello Alkalmazásátjáró. toodo a, hello Alkalmazásátjáró és a társított IP-/ DNS-nevét, hello PublicIPAddress elem csatolt toohello Alkalmazásátjáró beolvasása részleteit. Ezt megteheti az Azure DNS- vagy más DNS-szolgáltatók által létrehozható egy olyan CNAME rekordot, amely mutat toohello [nyilvános IP-cím](../dns/dns-custom-domain.md#public-ip-address). A-rekordok hello használata nem javasolt, mert hello VIP módosíthatja az Alkalmazásátjáró újra kell indítani.

> [!NOTE]
> IP-cím hozzá van rendelve toohello Alkalmazásátjáró hello szolgáltatás elindulásakor.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName ContosoRG -Name publicIP01
```

```
Name                     : publicIP01
ResourceGroupName        : ContosoRG
Location                 : westus
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

## <a name="delete-all-resources"></a>Az összes erőforrás törlése

Ebben a cikkben a következő teljes hello létrehozott összes erőforrás lépés toodelete:

```powershell
Remove-AzureRmResourceGroup -Name ContosoRG
```

## <a name="next-steps"></a>Következő lépések

Ha azt szeretné, hogy az SSL-kiszervezés tooconfigure, látogasson el: [konfigurálása az SSL-kiszervezés Alkalmazásátjáró](application-gateway-ssl.md).

Ha azt szeretné, hogy egy alkalmazás átjáró toouse belső terheléselosztással tooconfigure, látogasson el: [hozzon létre egy alkalmazást egy belső terheléselosztón (ILB)](application-gateway-ilb.md).

Ha további általános információra van szüksége a terheléselosztás beállításaival kapcsolatban:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
