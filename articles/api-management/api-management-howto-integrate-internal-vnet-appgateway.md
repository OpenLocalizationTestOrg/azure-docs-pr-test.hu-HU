---
title: "Azure API Management virtuális hálózatban az Alkalmazásátjáró aaaHow toouse |} Microsoft Docs"
description: "Megtudhatja, hogyan toosetup és belső virtuális hálózat az alkalmazás átjáró (waf-ot), amelynek az Azure API Management konfigurálása"
services: api-management
documentationcenter: 
author: solankisamir
manager: kjoshi
editor: antonba
ms.assetid: a8c982b2-bca5-4312-9367-4a0bbc1082b1
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2017
ms.author: sasolank
ms.openlocfilehash: 74303a2ee8a10db633ab1740ec7267728eacb473
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-api-management-in-an-internal-vnet-with-application-gateway"></a>Egy belső virtuális Hálózatot az API Management integrálása Alkalmazásátjáró 

##<a name="overview"></a> – Áttekintés
 
hello API-kezelés szolgáltatás konfigurálható egy virtuális hálózatot belső módban, így azokat csak hello virtuális hálózaton belülről érhetők el. Az Azure Application Gateway egy olyan PAAS szolgáltatás, amely réteg-7 terheléselosztót biztosít. Fordított proxy szolgáltatásként működik, és biztosít ajánlat egy webes alkalmazás tűzfalat (WAF) között.

Egy belső virtuális Hálózatot a hello Alkalmazásátjáró előtér kiépítve API Management kombinálásával lehetővé teszi, hogy a következő forgatókönyvek hello:

* Használjon hello azonos API-kezelés erőforráshoz a belső fogyasztók és a külső felhasználók által megjeleníthető.
* Egyetlen API Management erőforrást használ, és rendelkezik a külső felhasználók API Management érhető el a megadott API-t.
* Adjon meg egy kulcsrakész módon tooswitch hozzáférés tooAPI felügyeleti hello a nyilvános interneten be- és kikapcsolható. 

##<a name="scenario"></a> Forgatókönyv
Ez a cikk foglalkozik, hogyan toouse egyetlen API-kezelési szolgáltatás fut a külső és belső fogyasztók, és a működjön, és egyetlen időtúllépést a két helyszíni és felhőalapú API-k révén. Látni fogja emellett hogyan tooexpose az API-k (hello példa zöld kijelölt) csak egy részhalmazát hello PathBasedRouting funkció, amely az alkalmazás átjáró használata külső felhasználásra.

Hello első telepítő példában minden API felügyelt csak virtuális hálózaton belül. Belső fogyasztóknak (a kijelölt narancs) férhetnek hozzá a belső és külső API. Forgalom soha nem kerül ki a rendszer egy nagy teljesítményű tooInternet Expressroute-Kapcsolatcsoportok keresztül.

![URL-cím útvonal](./media/api-management-howto-integrate-internal-vnet-appgateway/api-management-howto-integrate-internal-vnet-appgateway.png)

## <a name="before-you-begin"></a> Megkezdése előtt

1. Webplatform-telepítő hello segítségével hello hello Azure PowerShell-parancsmagok legújabb verzióját telepítse. Töltse le, és telepítse hello legújabb verziót a hello **Windows PowerShell** hello szakasza [letöltési oldalon](https://azure.microsoft.com/downloads/).
2. Hozzon létre egy virtuális hálózatot, és hozzon létre különálló alhálózatokat az API Management és az Alkalmazásátjáró. 
3. Ha azt tervezi, hogy egy egyéni DNS-kiszolgáló a virtuális hálózati hello toocreate, megteheti hello központi telepítés elindítása előtt. Ellenőrizze a virtuális hálózati hello új alhálózat létrehozott virtuális gépek biztosításával működik megoldhatja és Azure szolgáltatásvégpontokra eléréséhez.

## <a name="what-is-required-toocreate-an-integration-between-api-management-and-application-gateway"></a>Mi az az API Management és az Application Gateway közötti integrációs szükséges toocreate?

* **Háttér-kiszolgálófiók készlet:** hello belső virtuális IP-címét az API Management szolgáltatás hello azt.
* **Háttér-kiszolgálókészlet beállításai:** Minden készletnek vannak beállításai, például port, protokoll vagy cookie-alapú affinitás. Ezek a beállítások hello készlet alkalmazott tooall-kiszolgálók esetén.
* **Előtér-port:** hello nyilvános port már meg van nyitva a hello Alkalmazásátjáró. Forgalom elérte-e azt lekérése a hello átirányított tooone háttérkiszolgálók.
* **Figyelő:** hello figyelő rendelkezik egy előtér-portot, a protokollt (Http vagy Https, ezek az értékek kis-és nagybetűket), és hello SSL tanúsítvány neve (ha az SSL beállításának-kiszervezés).
* **Szabály:** hello szabály van kötve egy figyelő tooa háttér-kiszolgálókészletet.
* **Egyéni Állapotmintáihoz:** Alkalmazásátjáró, alapértelmezés szerint használt IP-cím alapú mintavételt toofigure meg azok a kiszolgálók a hello BackendAddressPool aktívak. hello API Management szolgáltatást csak hello megfelelő állomásfejléc rendelkező toorequests válaszol, ezért a hello alapértelmezett mintavételt művelet sikertelen. Egy egyéni állapotmintáihoz kell definiált toobe toohelp Alkalmazásátjáró határozza meg, hogy hello szolgáltatás életben, és továbbítsa a kérelmeket.
* **Az egyéni tartomány tanúsítvány:** hello az API Management tooaccess internet toocreate az állomásnév toohello Alkalmazásátjáró előtér-DNS-név egy CNAME-leképezés van szüksége. Ez biztosítja, hogy hello állomásnév fejléc és küldött tanúsítvány tooApplication tooAPI továbbított átjáró felügyeleti egy APIM fel tudja ismerni, mint érvényes.

## <a name="overview-steps"></a> API Management és az Application Gateway integrálásához szükséges lépések 

1. Egy erőforráscsoport létrehozása a Resource Manager számára.
2. Hozzon létre egy virtuális hálózati alhálózat és nyilvános IP-Címek hello Application Gateway. Hozzon létre egy másik alhálózatot az API Management.
3. Hozzon létre egy API-kezelés szolgáltatás a fenti létrehozott hello VNET subnet belül, és győződjön meg arról, hello belső módot használja.
4. A telepítő egyéni tartománynevet hello hello API-kezelés szolgáltatás.
5. Az Application Gateway konfigurációs objektum létrehozásához.
6. Alkalmazásátjáró erőforrás létrehozása.
7. Hozzon létre egy CNAME REKORDOT a hello Alkalmazásátjáró toohello API Management proxy állomásnév hello nyilvános DNS-nevét.

## <a name="create-a-resource-group-for-resource-manager"></a>Erőforráscsoport létrehozása a Resource Managerhez

Győződjön meg arról, hogy azok be hello Azure PowerShell legújabb verzióját. További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>1. lépés

Jelentkezzen be tooAzure

```powershell
Login-AzureRmAccount
```

Azokkal a hitelesítő adatait.<BR>

### <a name="step-2"></a>2. lépés

Ellenőrizze a hello előfizetések hello fiókhoz, és válassza ki azt.

```powershell
Get-AzureRmSubscription -Subscriptionid "GUID of subscription" | Select-AzureRmSubscription
```

### <a name="step-3"></a>3. lépés

Hozzon létre egy erőforráscsoportot (hagyja ki ezt a lépést, ha egy meglévő erőforráscsoportot használ).

```powershell
New-AzureRmResourceGroup -Name "apim-appGw-RG" -Location "West US"
```
Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet. Ez az erőforráscsoport erőforrások hello alapértelmezett helye szolgál. Győződjön meg arról, hogy az összes parancsok toocreate egy alkalmazás átjáró használata hello ugyanabban az erőforráscsoportban.

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a>Hozzon létre egy virtuális hálózatot és hello Alkalmazásátjáró alhálózatot

hello a következő példa bemutatja, hogyan egy virtuális hálózat használatával toocreate hello erőforrás-kezelő.

### <a name="step-1"></a>1. lépés

Rendelje hozzá a hello cím tartomány 10.0.0.0/24 toohello alhálózati változó toobe virtuális hálózat létrehozása során használt.

```powershell
$appgatewaysubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim01" -AddressPrefix "10.0.0.0/24"
```

### <a name="step-2"></a>2. lépés

Rendelje hozzá a hello cím tartomány 10.0.1.0/24 toohello alhálózati változó toobe API-felügyelethez használt virtuális hálózat létrehozása során.

```powershell
$apimsubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim02" -AddressPrefix "10.0.1.0/24"
```

### <a name="step-3"></a>3. lépés

Hozzon létre egy virtuális hálózatot nevű **appgwvnet** erőforráscsoportban **apim-appGw-RG** hello USA nyugati régiójában hello előtag 10.0.0.0/16 használatával a 10.0.0.0/24 alhálózat és 10.0.1.0/24.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name "appgwvnet" -ResourceGroupName "apim-appGw-RG" -Location "West US" -AddressPrefix "10.0.0.0/16" -Subnet $appgatewaysubnet,$apimsubnet
```

### <a name="step-4"></a>4. lépés

Rendelje hozzá a következő lépéseket hello alhálózati változó

```powershell
$appgatewaysubnetdata=$vnet.Subnets[0]
$apimsubnetdata=$vnet.Subnets[1]
```
## <a name="create-an-api-management-service-inside-a-vnet-configured-in-internal-mode"></a>Egy belső módban konfigurált virtuális hálózaton belül az API Management szolgáltatás létrehozása

hello következő példa bemutatja, hogyan toocreate egy API-kezelés szolgáltatás a VNETEN belül beállítást csak a belső hozzáférést.

### <a name="step-1"></a>1. lépés
A fenti létrehozott $apimsubnetdata hello alhálózati használata API felügyeleti virtuális hálózati objektumot létrehozni.

```powershell
$apimVirtualNetwork = New-AzureRmApiManagementVirtualNetwork -Location "West US" -SubnetResourceId $apimsubnetdata.Id
```
### <a name="step-2"></a>2. lépés
Hello virtuális hálózaton belül az API Management szolgáltatás létrehozása.

```powershell
$apimService = New-AzureRmApiManagement -ResourceGroupName "apim-appGw-RG" -Location "West US" -Name "ContosoApi" -Organization "Contoso" -AdminEmail "admin@contoso.com" -VirtualNetwork $apimVirtualNetwork -VpnType "Internal" -Sku "Developer"
```
Hello fent parancs sikeres követően tekintse meg a túl[DNS-konfiguráció szükséges tooaccess belső hálózatok API-kezelés szolgáltatás](api-management-using-with-internal-vnet.md#apim-dns-configuration) tooaccess azt.

## <a name="set-up-a-custom-domain-name-in-api-management"></a>Telepítés egy egyéni tartománynevet, az API Management

### <a name="step-1"></a>1. lépés
Hello-tanúsítvány feltöltése hello tartományhoz tartozó titkos kulccsal. Ehhez a példához lesz `*.contoso.net`. 

```powershell
$certUploadResult = Import-AzureRmApiManagementHostnameCertificate -ResourceGroupName "apim-appGw-RG" -Name "ContosoApi" -HostnameType "Proxy" -PfxPath <full path too.pfx file> -PfxPassword <password for certificate file> -PassThru
```

### <a name="step-2"></a>2. lépés
Hello tanúsítványt a feltöltést követően hozzon létre egy állomásnév konfigurációs objektuma hello proxy egy állomásnevét `api.contoso.net`, mert a hello példa tanúsítvány hatóság biztosít hello `*.contoso.net` tartomány. 

```powershell
$proxyHostnameConfig = New-AzureRmApiManagementHostnameConfiguration -CertificateThumbprint $certUploadResult.Thumbprint -Hostname "api.contoso.net"
$result = Set-AzureRmApiManagementHostnames -Name "ContosoApi" -ResourceGroupName "apim-appGw-RG" -ProxyHostnameConfiguration $proxyHostnameConfig
```

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a>A nyilvános IP-cím hello előtér-konfiguráció létrehozása

Hozzon létre egy nyilvános IP-erőforrás **publicIP01** erőforráscsoportban **apim-appGw-RG** hello USA nyugati régiójában.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -name "publicIP01" -location "West US" -AllocationMethod Dynamic
```

IP-cím hozzá van rendelve toohello Alkalmazásátjáró hello szolgáltatás elindulásakor.

## <a name="create-application-gateway-configuration"></a>Alkalmazás átjáró-konfiguráció létrehozása

Az összes konfigurációs elemek hello Alkalmazásátjáró létrehozása előtt kell beállítani. hello lépések hello konfigurációs elemek létrehozását, amelyek szükségesek az alkalmazás átjáró-erőforráshoz.

### <a name="step-1"></a>1. lépés

Hozzon létre egy **gatewayIP01** nevű Application Gateway IP-konfigurációt. Alkalmazásátjáró indításakor szerzi be hello alhálózati konfigurált IP-címeit, és útvonal-hello háttér IP-készletet a hálózati forgalom toohello IP-címek. Ne feledje, hogy minden példány egy IP-címet vesz fel.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name "gatewayIP01" -Subnet $appgatewaysubnetdata
```

### <a name="step-2"></a>2. lépés

Konfigurálja az előtér-IP-port hello hello nyilvános IP-végponton. Ez a port nem hello port, amelyet a végfelhasználók csatlakozni.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "port01"  -Port 443
```
### <a name="step-3"></a>3. lépés

Nyilvános IP-végponton hello előtér-IP konfigurálja.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-4"></a>4. lépés

Hello Alkalmazásátjáró, használja a toodecrypt hello (SCEP) konfigurálhat, és hogy át kellene haladnia hello-forgalom újratitkosítása.

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name "cert01" -CertificateFile <full path too.pfx file> -Password <password for certificate file>
```

### <a name="step-5"></a>5. lépés

Hello HTTP-figyelő hello Alkalmazásátjáró létrehozása. Rendelje hozzá a hello előtér-IP-konfiguráció, a port és az ssl-tanúsítvány tooit.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol "Https" -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -SslCertificate $cert
```

### <a name="step-6"></a>6. lépés

Hozzon létre egy egyéni mintavételi toohello API-kezelés szolgáltatás `ContosoApi` proxy tartomány végpont. hello elérési `/status-0123456789abcdef` egy alapértelmezett állapotfigyelő végpont futó összes hello API Management szolgáltatást. Állítsa be `api.contoso.net` egy egyéni mintavételi állomásnév toosecure, az SSL-tanúsítvánnyal.

> [!NOTE]
> állomásnév hello `contosoapi.azure-api.net` a rendszer hello alapértelmezett proxy állomásnév konfigurálja a szolgáltatás nevű `contosoapi` jön létre a nyilvános Azure-ban. 
> 

```powershell
$apimprobe = New-AzureRmApplicationGatewayProbeConfig -Name "apimproxyprobe" -Protocol "Https" -HostName "api.contoso.net" -Path "/status-0123456789abcdef" -Interval 30 -Timeout 120 -UnhealthyThreshold 8
```

### <a name="step-7"></a>7. lépés

Hello SSL Protokollt használó háttér címkészletet erőforrások használt toobe hello-tanúsítvány feltöltése. Ez a hello ugyanazt a tanúsítványt, amely a fenti 4 megadott.

```powershell
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name "whitelistcert1" -CertificateFile <full path too.cer file>
```

### <a name="step-8"></a>8. lépés

HTTP háttér hello Alkalmazásátjáró beállításainak konfigurálása. Ez magában foglalja a háttérrendszer kérelem megszakítása elteltével időtúllépési korlát. Ez az érték eltér hello mintavételi időtúllépés.

```powershell
$apimPoolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "apimPoolSetting" -Port 443 -Protocol "Https" -CookieBasedAffinity "Disabled" -Probe $apimprobe -AuthenticationCertificates $authcert -RequestTimeout 180
```

### <a name="step-9"></a>9. lépés

Konfigurálja a háttér IP-címkészletet nevű **apimbackend** hello belső virtuális IP-címe hello API-kezelés szolgáltatás a fenti létrehozott.

```powershell
$apimProxyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "apimbackend" -BackendIPAddresses $apimService.StaticIPs[0]
```

### <a name="step-10"></a>10. lépés

Hozzon létre egy üres (nem létező) háttér beállításait. Nem szeretnénk az API Management alkalmazás átjárón keresztül tooexpose a rendszer elérte a háttér, majd térjen vissza a 404-es tooAPI útvonal kérelmeket.

Hello dummy háttér HTTP beállításainak konfigurálása.

```powershell
$dummyBackendSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "dummySetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

Egy üres háttér konfigurálása **dummyBackendPool**, amely mutat tooa FQDN cím **dummybackend.com**. Az FQDN cím hello virtuális hálózat nem létezik.

```powershell
$dummyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "dummyBackendPool" -BackendFqdns "dummybackend.com"
```

Hozzon létre egy szabályt, hogy Alkalmazásátjáró toohello nem létező háttér mutat, amely alapértelmezés szerint használandó hello beállítása **dummybackend.com** hello virtuális hálózat található.

```powershell
$dummyPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "nonexistentapis" -Paths "/*" -BackendAddressPool $dummyBackendPool -BackendHttpSettings $dummyBackendSetting
```

### <a name="step-11"></a>11. lépés

URL-cím szabály útvonalak hello háttér-címkészletek konfigurálása. Ez lehetővé teszi, hogy csak néhány hello API-kat API Management ahhoz, hogy meg toohello nyilvánosan elérhetővé tett kiválasztása. Például akkor, ha nincsenek `Echo API` (/ echo /) `Calculator API` csak (/calc/) stb. Ellenőrizze `Echo API` internetről hozzáférhető). 

hello alábbi példa létrehoz egy egyszerű szabályt hello "/ echo /" elérési út útválasztási forgalom toohello back-end "apimProxyBackendPool".

```powershell
$echoapiRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "externalapis" -Paths "/echo/*" -BackendAddressPool $apimProxyBackendPool -BackendHttpSettings $apimPoolSetting
```

Ha hello elérési út nem felel meg a hello elérési szabályok azt szeretnénk, ha az API Management, térkép konfiguráció is konfigurálja egy alapértelmezett háttér címkészletet nevű hello szabály tooenable **dummyBackendPool**. Például http://api.contoso.net/calc/ * kerül túl**dummyBackendPool** , mint a nem egyező forgalom hello alapértelmezett alkalmazáskészlet van definiálva.

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $echoapiRule, $dummyPathRule -DefaultBackendAddressPool $dummyBackendPool -DefaultBackendHttpSettings $dummyBackendSetting
```

hello fenti lépés biztosítja, amelyek csak hello elérési vonatkozó kérések "/ echo" hello Alkalmazásátjáró engedélyezettek legyenek. Az API Management API-k konfigurált kérelmek tooother 404-es hibák kivételhibát hello Internet történő alkalmazás-átjáróról. 

### <a name="step-12"></a>12. lépés

Hello Alkalmazásátjáró toouse URL-cím elérési út-alapú útválasztási szabály beállítás létrehozása.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-13"></a>13. lépés

Példányok és az Application Gateway hello mérete hello számának konfigurálása. Itt használjuk hello [WAF SKU](../application-gateway/application-gateway-webapplicationfirewall-overview.md) hello API-kezelés erőforráshoz a biztonság fokozása érdekében.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "WAF_Medium" -Tier "WAF" -Capacity 2
```

### <a name="step-14"></a>14. lépés

WAF toobe konfigurálása "Megakadályozása" módban.
```powershell
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

## <a name="create-application-gateway"></a>Alkalmazásátjáró létrehozása

Hozzon létre egy alkalmazás összes hello konfigurációs objektumot az előző lépésekben hello.

```powershell
$appgw = New-AzureRmApplicationGateway -Name $applicationGatewayName -ResourceGroupName $resourceGroupName  -Location $location -BackendAddressPools $apimProxyBackendPool, $dummyBackendPool -BackendHttpSettingsCollection $apimPoolSetting, $dummyBackendSetting  -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert -Probes $apimprobe
```

## <a name="cname-hello-api-management-proxy-hostname-toohello-public-dns-name-of-hello-application-gateway-resource"></a>CNAME hello API Management proxy állomásnév toohello nyilvános DNS-neve hello Alkalmazásátjáró erőforrás

Hello átjáró létrehozása után hello következő lépésre tooconfigure hello előtér-kommunikációhoz. Egy nyilvános IP-cím használata esetén az Application Gateway egy dinamikusan hozzárendelt DNS-nevet, amely nem lehet könnyen toouse van szükség. 

hello Application Gateway DNS-nevét kell használt toocreate egy olyan CNAME rekordot, amely hello APIM proxyállomásnév mutat (pl. `api.contoso.net` a fenti példákban hello) toothis DNS-nevét. tooconfigure hello előtérbeli IP-CNAME rekordot, hello Alkalmazásátjáró hello részleteit és a társított IP-/ DNS-nevét, hello PublicIPAddress elem beolvasása. A-rekordok hello használata nem ajánlott, óta hello VIP előfordulhat, hogy az átjáró újra kell indítani.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -Name "publicIP01"
```

##<a name="summary"></a> Összegzése
Konfigurált egy VNETET az Azure API Management átjáró egyetlen felületet biztosít minden konfigurált API, legyenek azok helyszíni üzemeltetett vagy hello felhő. Alkalmazásátjáró integrálása az API Management rugalmasan hello szelektív engedélyezése az adott API-k toobe hello Internet elérhető, valamint a webalkalmazási tűzfal előtér tooyour API Management példányt biztosít.

##<a name="next-steps"></a> További lépések
* További tudnivalók az Azure Application Gateway
  * [Átjáró – áttekintés](../application-gateway/application-gateway-introduction.md)
  * [Alkalmazás átjáró webalkalmazási tűzfal](../application-gateway/application-gateway-webapplicationfirewall-overview.md)
  * [Az Alkalmazásátjáró elérési alapú útválasztás használatával](../application-gateway/application-gateway-create-url-route-arm-ps.md)
* Ismerje meg a további információk az API Management és Vnetek
  * [Rendelkezésre álló hello virtuális hálózaton belül csak az API Management használata](api-management-using-with-internal-vnet.md)
  * [Az API Management használatát a virtuális hálózat](api-management-using-with-vnet.md)
