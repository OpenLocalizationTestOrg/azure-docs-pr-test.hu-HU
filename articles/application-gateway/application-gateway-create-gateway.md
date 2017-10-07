---
title: "aaaCreate, indítsa el, vagy meglévő Alkalmazásátjáró törlése |} Microsoft Docs"
description: "Ezen a lapon útmutatás toocreate, konfigurálása, indítsa el, és az Azure Alkalmazásátjáró törlése"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 577054ca-8368-4fbf-8d53-a813f29dc3bc
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: 3efef5b49880c9efdafad8b88d4bce5b749b82af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-start-or-delete-an-application-gateway-with-powershell"></a>Application Gateway létrehozása, indítása vagy törlése PowerShell-lel 

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Klasszikus Azure PowerShell](application-gateway-create-gateway.md)
> * [Azure Resource Manager-sablon](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI](application-gateway-create-gateway-cli.md)

Az Azure Application Gateway egy 7. rétegbeli terheléselosztó. Akár hello felhőalapú vagy helyszíni biztosít feladatátvételt, HTTP-kérelmek teljesítmény-útválasztási különböző kiszolgálók között. Az Application Gateway számos alkalmazáskézbesítési vezérlőszolgáltatást (ADC) biztosít, beleértve a HTTP-terheléselosztást, a cookie-alapú munkamenet-affinitást, a Secure Sockets Layer (SSL) alapú kiszervezést, az egyéni állapotteszteket, a többhelyes támogatást és még sok mást. látogasson el a támogatott funkciók teljes listáját toofind [átjáró – áttekintés](application-gateway-introduction.md)

Ez a cikk bemutatja, hogyan hello lépéseket toocreate, konfigurálása, indítsa el és Alkalmazásátjáró törlése.

## <a name="before-you-begin"></a>Előkészületek

1. Webplatform-telepítő hello segítségével hello hello Azure PowerShell-parancsmagok legújabb verzióját telepítse. Töltse le, és telepítse hello legújabb verziót a hello **Windows PowerShell** hello szakasza [letöltési oldalon](https://azure.microsoft.com/downloads/).
2. Ha egy meglévő virtuális hálózattal rendelkezik, válasszon egy létező üres alhálózatot, vagy hozzon létre egy új alhálózatot a meglévő virtuális hálózatban kizárólag használatra hello Alkalmazásátjáró által. Nem telepíthet központilag hello alkalmazás átjáró tooa másik virtuális hálózatot hello erőforrások mint szeretné mögött hello Alkalmazásátjáró toodeploy kivéve, ha a vnetben társviszony-létesítés szolgál. több látogasson el az toolearn [Vnetben társviszony-létesítés](../virtual-network/virtual-network-peering-overview.md)
3. Ellenőrizze, hogy rendelkezik-e működő virtuális hálózattal és hozzá tartozó érvényes alhálózattal. Győződjön meg arról, hogy egyetlen virtuális gépek vagy a felhőben történő alkalmazáshoz hello alhálózat használja. a virtuális hálózati alhálózat hello Alkalmazásátjáró önmagában kell lennie.
4. hello kiszolgálók toouse hello Alkalmazásátjáró konfigurált léteznie kell, különben a végpontok hello virtuális hálózatban vagy a nyilvános IP-cím/VIP rendelt hozzá.

## <a name="what-is-required-toocreate-an-application-gateway"></a>Mi az a szükséges toocreate olyan átjárót?

Hello használata esetén `New-AzureApplicationGateway` parancs toocreate hello Alkalmazásátjáró, nem történik meg ezen a ponton, és az újonnan létrehozott hello erőforrás konfigurált XML vagy a konfigurációs objektum használva.

hello értékek a következők:

* **Háttér-kiszolgálófiók készlet:** hello hello háttér-kiszolgálók IP-címek listáját. hello IP-címek felsorolt toohello virtuális hálózati alhálózat vagy kell tartoznia, vagy egy nyilvános IP-cím/VIP kell lennie.
* **Háttér-kiszolgálókészlet beállításai:** Minden készletnek vannak beállításai, például port, protokoll vagy cookie-alapú affinitás. Ezek a beállítások esetén tooa kapcsolt verem és a hello készlet alkalmazott tooall-kiszolgálók.
* **Előtér-port:** Ez a port nem hello nyilvános portot, amelyet a hello Alkalmazásátjáró meg van nyitva. Forgalom találatok ezt a portot, és lekérdezi átirányítja tooone hello háttér-kiszolgálók.
* **Figyelő:** hello figyelő rendelkezik egy előtér-portot, a protokollt (Http vagy Https, ezek az értékek kis-és nagybetűket), és hello SSL tanúsítvány neve (ha az SSL beállításának-kiszervezés).
* **Szabály:** hello szabály hello figyelő és hello háttér-kiszolgálófiók alkalmazáskészlet van kötve, és azt határozza meg, mely háttér-kiszolgálófiók készlet hello forgalom irányított toowhen találatok száma a egy adott figyelő.

## <a name="create-an-application-gateway"></a>Application Gateway létrehozása

Alkalmazásátjáró toocreate:

1. Egy Application Gateway erőforrás létrehozása.
2. Hozzon létre egy konfigurációs XML-fájlt vagy konfigurációs objektumot.
3. Az újonnan létrehozott alkalmazás átjáró erőforrás hello konfigurációs toohello véglegesítése.

> [!NOTE]
> Ha egy egyéni mintavételt tooconfigure az Alkalmazásátjáró van szüksége, tekintse meg [PowerShell használatával hozzon létre olyan átjárót egyéni mintavételt](application-gateway-create-probe-classic-ps.md). További információért lásd: [egyéni minták és állapotfigyelés](application-gateway-probe-overview.md).

![Példaforgatókönyv][scenario]

### <a name="create-an-application-gateway-resource"></a>Application Gateway erőforrás létrehozása

toocreate hello átjáró használata hello `New-AzureApplicationGateway` hello értékeket cserélje le a saját, a parancsmag. Számlázási hello átjáró nem indul el ezen a ponton. Számlázási egy későbbi lépésben, akkor kezdődik, amikor hello átjáró sikeresen elindult.

hello alábbi példa használatával hozza létre az Alkalmazásátjáró nevű, "testvnet1" és "alhálózat-1" nevű alhálózat virtuális hálózat:

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

A *Description*, az *InstanceCount* és a *GatewaySize* opcionális paraméterek.

amely átjáró hello toovalidate lett létrehozva, használhatja a hello `Get-AzureApplicationGateway` parancsmag.

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
Name          : AppGwTest
Description   :
VnetName      : testvnet1
Subnets       : {Subnet-1}
InstanceCount : 2
GatewaySize   : Medium
State         : Stopped
VirtualIPs    : {}
DnsName       :
```

> [!NOTE]
> az alapértelmezett érték hello *InstanceCount* 2, maximális értéke 10. az alapértelmezett érték hello *GatewaySize* közepes. A Kicsi, Közepes és a Nagy lehetőségek közül választhat.

*Virtualip értékek* és *DnsName* jelennek meg az üres mert hello átjáró még nem kezdődött meg. Miután hello átjáró fut. hello jön létre.

## <a name="configure-hello-application-gateway"></a>Alkalmazásátjáró hello konfigurálása

Hello Alkalmazásátjáró XML- vagy konfiguráció-objektum használatával konfigurálható.

### <a name="configure-hello-application-gateway-by-using-xml"></a>Alkalmazásátjáró hello konfigurálása XML használatával

A következő példa hello egy XML-fájl tooconfigure összes alkalmazás átjáró beállításait használja, és véglegesítse azokat toohello alkalmazás átjáró-erőforráshoz.  

#### <a name="step-1"></a>1. lépés

A következő szöveg tooNotepad hello másolja.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendPorts>
        <FrontendPort>
            <Name>(name-of-your-frontend-port)</Name>
            <Port>(port number)</Port>
        </FrontendPort>
    </FrontendPorts>
    <BackendAddressPools>
        <BackendAddressPool>
            <Name>(name-of-your-backend-pool)</Name>
            <IPAddresses>
                <IPAddress>(your-IP-address-for-backend-pool)</IPAddress>
                <IPAddress>(your-second-IP-address-for-backend-pool)</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>(backend-setting-name-to-configure-rule)</Name>
            <Port>80</Port>
            <Protocol>[Http|Https]</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>(name-of-the-listener)</Name>
            <FrontendPort>(name-of-your-frontend-port)</FrontendPort>
            <Protocol>[Http|Https]</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>(name-of-load-balancing-rule)</Name>
            <Type>basic</Type>
            <BackendHttpSettings>(backend-setting-name-to-configure-rule)</BackendHttpSettings>
            <Listener>(name-of-the-listener)</Listener>
            <BackendAddressPool>(name-of-your-backend-pool)</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

Módosíthatja a hello konfigurációs elemek hello zárójelek között hello értékeket. Bővítmény .xml hello fájl mentése.

> [!IMPORTANT]
> hello protokoll Http vagy Https elem kis-és nagybetűket.

hello a következő példa bemutatja, hogyan toouse egy konfigurációs fájl mentése hello Alkalmazásátjáró tooset. hello példa terhelés kiegyensúlyozza a nyilvános port 80-as HTTP-forgalom, és elküldi a hálózati forgalom 80-as port tooback-a befejezési két IP-címre.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
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

#### <a name="step-2"></a>2. lépés

Következő lépésként állítsa hello Alkalmazásátjáró. Használjon hello `Set-AzureApplicationGatewayConfig` parancsmag és egy konfigurációs XML-fájlt.

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"
```

### <a name="configure-hello-application-gateway-by-using-a-configuration-object"></a>Hello Alkalmazásátjáró konfigurációs objektum használatával konfigurálja

hello a következő példa bemutatja, hogyan tooconfigure hello konfigurációs objektumok használatával az Alkalmazásátjáró. Az összes konfigurációs elemek kell külön-külön konfigurálhatók és ezután tooan átjáró konfigurációs objektum. Hello konfigurációs objektum létrehozása után használhat hello `Set-AzureApplicationGateway` parancs toocommit hello konfigurációs toohello korábban létrehozott alkalmazás átjáró-erőforráshoz.

> [!NOTE]
> Hozzárendelése egy érték tooeach konfigurációs objektumát, előtt kell toodeclare milyen típusú objektumot PowerShell használja a tároláshoz. hello első sor toocreate hello elemek határozza meg, mi `Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model(object name)` szolgálnak.

#### <a name="step-1"></a>1. lépés

Hozza létre az összes egyedi konfigurációs elemet.

Hozzon létre hello előtér-IP, ahogy az alábbi példa hello.

```powershell
$fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
$fip.Name = "fip1"
$fip.Type = "Private"
$fip.StaticIPAddress = "10.0.0.5"
```

Hozzon létre hello előtér-port, ahogy az alábbi példa hello.

```powershell
$fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
$fep.Name = "fep1"
$fep.Port = 80
```

Hozzon létre hello háttér-kiszolgálókészletet.

Adja meg a hello IP-címek hozzáadott toohello háttér-kiszolgálófiók készlet hello a következő példában látható módon.

```powershell
$servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
$servers.Add("10.0.0.1")
$servers.Add("10.0.0.2")
```

Hello $server objektum tooadd hello értékek toohello háttér-objektumot ($pool) használja.

```powershell
$pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
$pool.BackendServers = $servers
$pool.Name = "pool1"
```

Hozzon létre hello háttér-kiszolgálófiók készlet beállítást.

```powershell
$setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
$setting.Name = "setting1"
$setting.CookieBasedAffinity = "enabled"
$setting.Port = 80
$setting.Protocol = "http"
```

Hozzon létre hello figyelőt.

```powershell
$listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
$listener.Name = "listener1"
$listener.FrontendPort = "fep1"
$listener.FrontendIP = "fip1"
$listener.Protocol = "http"
$listener.SslCert = ""
```

Hello szabály létrehozása.

```powershell
$rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
$rule.Name = "rule1"
$rule.Type = "basic"
$rule.BackendHttpSettings = "setting1"
$rule.Listener = "listener1"
$rule.BackendAddressPool = "pool1"
```

#### <a name="step-2"></a>2. lépés

Rendelje hozzá minden egyes konfigurációs elemek tooan alkalmazás átjáró konfigurációs objektumot ($appgwconfig).

Adja hozzá a hello előtér-IP-toohello konfigurációja.

```powershell
$appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
$appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
$appgwconfig.FrontendIPConfigurations.Add($fip)
```

Adja hozzá a hello előtér-port toohello konfigurálása.

```powershell
$appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
$appgwconfig.FrontendPorts.Add($fep)
```
Adja hozzá a hello háttér-kiszolgálófiók alkalmazáskészlet toohello konfigurációját.

```powershell
$appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
$appgwconfig.BackendAddressPools.Add($pool)
```

Adja hozzá a hello háttér-tárolókészlet beállítás toohello konfigurációját.

```powershell
$appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
$appgwconfig.BackendHttpSettingsList.Add($setting)
```

Adja hozzá a hello figyelő toohello konfigurációja.

```powershell
$appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
$appgwconfig.HttpListeners.Add($listener)
```

Adja hozzá a hello szabály toohello konfigurálását.

```powershell
$appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
$appgwconfig.HttpLoadBalancingRules.Add($rule)
```

### <a name="step-3"></a>3. lépés
Hello konfigurációs objektum toohello alkalmazás átjáró erőforrás véglegesítése használatával `Set-AzureApplicationGatewayConfig`.

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig
```

## <a name="start-hello-gateway"></a>Indítsa el a hello átjáró

Ha hello átjáró van konfigurálva, a hello `Start-AzureApplicationGateway` parancsmag toostart hello átjáró. Alkalmazásátjáró számlázás megkezdése után hello átjáró sikeresen elindult.

> [!NOTE]
> Hello `Start-AzureApplicationGateway` parancsmag is igénybe vehet fel toofinish too15-20 perc.

```powershell
Start-AzureApplicationGateway AppGwTest
```

## <a name="verify-hello-gateway-status"></a>Hello az átjáró állapotának megerősítése

Használjon hello `Get-AzureApplicationGateway` parancsmag toocheck hello hello átjáró állapotának. Ha `Start-AzureApplicationGateway` hello a korábbi lépésben sikeres *állapot* kell futnia, és *Vip* és *DnsName* érvényes bejegyzést kell rendelkeznie.

hello alábbi példa azt mutatja, amely akár, nem fut, Alkalmazásátjáró, és készen áll a tootake forgalom szánt `http://<generated-dns-name>.cloudapp.net`.

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
Vip           : 138.91.170.26
DnsName       : appgw-1b8402e8-3e0d-428d-b661-289c16c82101.cloudapp.net
```

## <a name="delete-hello-application-gateway"></a>Hello Alkalmazásátjáró törlése

toodelete hello Alkalmazásátjáró:

1. Használjon hello `Stop-AzureApplicationGateway` parancsmag toostop hello átjáró.
2. Használjon hello `Remove-AzureApplicationGateway` parancsmag tooremove hello átjáró.
3. Győződjön meg arról, hogy hello átjáró el lett távolítva a hello `Get-AzureApplicationGateway` parancsmag.

hello alábbi példa bemutatja hello `Stop-AzureApplicationGateway` hello első sor parancsmag hello kimeneti követ.

```powershell
Stop-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8
```

Ha hello Alkalmazásátjáró leállított állapotban van, a hello `Remove-AzureApplicationGateway` parancsmag tooremove hello szolgáltatást.

```powershell
Remove-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301
```

tooverify, amely hello szolgáltatás el lett távolítva, használhatja a hello `Get-AzureApplicationGateway` parancsmag. Ez a lépés nem kötelező.

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

Get-AzureApplicationGateway : ResourceNotFound: hello gateway does not exist.
.....
```

## <a name="next-steps"></a>Következő lépések

Ha azt szeretné, hogy az SSL-kiszervezés tooconfigure, [konfigurálása az SSL-kiszervezés Alkalmazásátjáró](application-gateway-ssl.md).

Ha azt szeretné, hogy egy alkalmazás átjáró toouse belső terheléselosztással tooconfigure, [hozzon létre egy alkalmazást egy belső terheléselosztón (ILB)](application-gateway-ilb.md).

Ha további általános információra van szüksége a terheléselosztás beállításaival kapcsolatban:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

[scenario]: ./media/application-gateway-create-gateway/scenario.png
