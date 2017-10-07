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
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-hello-classic-deployment-model"></a>SSL kiszervezési Alkalmazásátjáró konfigurálása hello klasszikus telepítési modell segítségével

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
> * [Klasszikus Azure PowerShell](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Az Azure Application Gateway konfigurált tooterminate hello Secure Sockets Layer (SSL) munkamenet: hello átjáró tooavoid költséges SSL visszafejtési feladatok toohappen: hello webfarm lehet. SSL kiszervezési is egyszerűbbé teszi a hello előtér-kiszolgáló beállítása és felügyelete hello webalkalmazás.

## <a name="before-you-begin"></a>Előkészületek

1. Webplatform-telepítő hello segítségével hello hello Azure PowerShell-parancsmagok legújabb verzióját telepítse. Töltse le, és telepítse hello legújabb verziót a hello **Windows PowerShell** hello szakasza [letöltési oldalon](https://azure.microsoft.com/downloads/).
2. Ellenőrizze, hogy rendelkezik-e működő virtuális hálózattal és hozzá tartozó érvényes alhálózattal. Győződjön meg arról, hogy egyetlen virtuális gépek vagy a felhőben történő alkalmazáshoz hello alhálózat használja. a virtuális hálózati alhálózat hello Alkalmazásátjáró önmagában kell lennie.
3. hello kiszolgálók toouse hello Alkalmazásátjáró konfigurált léteznie kell, különben a végpontok hello virtuális hálózatban vagy a nyilvános IP-cím/VIP rendelt hozzá.

tooconfigure SSL Alkalmazásátjáró kiszervezésére, hello a következő lépéseket a hello sorrendben:

1. [Alkalmazásátjáró létrehozása](#create-an-application-gateway)
2. [SSL-tanúsítványok feltöltése](#upload-ssl-certificates)
3. [Hello átjáró konfigurálása](#configure-the-gateway)
4. [Hello átjáró konfigurációjának beállítása](#set-the-gateway-configuration)
5. [Indítsa el a hello átjáró](#start-the-gateway)
6. [Hello az átjáró állapotának megerősítése](#verify-the-gateway-status)

## <a name="create-an-application-gateway"></a>Application Gateway létrehozása

toocreate hello átjáró használata hello `New-AzureApplicationGateway` hello értékeket cserélje le a saját, a parancsmag. Számlázási hello átjáró nem indul el ezen a ponton. Számlázási egy későbbi lépésben, akkor kezdődik, amikor hello átjáró sikeresen elindult.

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

amely átjáró hello toovalidate lett létrehozva, használhatja a hello `Get-AzureApplicationGateway` parancsmag.

Hello mintában *leírás*, *InstanceCount*, és *GatewaySize* opcionális paraméterek. az alapértelmezett érték hello *InstanceCount* 2, maximális értéke 10. az alapértelmezett érték hello *GatewaySize* közepes. Kis és nagy más elérhető értékek. *Virtualip értékek* és *DnsName* jelennek meg az üres mert hello átjáró még nem kezdődött meg. Ezeket az értékeket jönnek létre, ha hello átjáró hello futó állapotban van.

```powershell
Get-AzureApplicationGateway AppGwTest
```

## <a name="upload-ssl-certificates"></a>SSL-tanúsítványok feltöltése

Használjon `Add-AzureApplicationGatewaySslCertificate` tooupload hello kiszolgálói tanúsítvány *pfx* formátum toohello Alkalmazásátjáró. hello tanúsítvány neve egy felhasználó által választott név és hello Alkalmazásátjáró belül egyedieknek kell lenniük. Ez a tanúsítvány az említett tooby ezt a nevet az összes hello Alkalmazásátjáró a felügyeleti műveletekhez.

Ez a következő példa bemutatja hello parancsmag, cserélje le hello mintában hello értékeket a saját.

```powershell
Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path toopfx file>
```

A következő érvényesítse hello tanúsítvány feltöltése. Használjon hello `Get-AzureApplicationGatewayCertificate` parancsmag.

Ez a példa bemutatja hello parancsmag hello első sor hello kimeneti követi.

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
> hello tanúsítvány jelszava toobe között 4 too12 karakterek, betűket és számokat tartalmaz. Speciális karakterek használata nem engedélyezett.

## <a name="configure-hello-gateway"></a>Hello átjáró konfigurálása

Egy alkalmazás átjáró konfigurálása több érték áll. hello értékeket is kötődik együtt tooconstruct hello konfigurációs.

hello értékek a következők:

* **Háttér-kiszolgálófiók készlet:** hello hello háttér-kiszolgálók IP-címek listáját. hello IP-címek felsorolt toohello virtuális hálózati alhálózat vagy kell tartoznia, vagy egy nyilvános IP-cím/VIP kell lennie.
* **Háttér-kiszolgálókészlet beállításai:** Minden készletnek vannak beállításai, például port, protokoll vagy cookie-alapú affinitás. Ezek a beállítások esetén tooa kapcsolt verem és a hello készlet alkalmazott tooall-kiszolgálók.
* **Előtér-port:** Ez a port nem hello nyilvános portot, amelyet a hello Alkalmazásátjáró meg van nyitva. Forgalom találatok ezt a portot, és lekérdezi átirányítja tooone hello háttér-kiszolgálók.
* **Figyelő:** hello figyelő rendelkezik egy előtér-portot, a protokollt (Http vagy Https, ezek az értékek kis-és nagybetűket), és hello SSL tanúsítvány neve (ha az SSL beállításának-kiszervezés).
* **Szabály:** hello szabály hello figyelő és hello háttér-kiszolgálófiók alkalmazáskészlet van kötve, és azt határozza meg, mely háttér-kiszolgálófiók készlet hello forgalom irányított toowhen találatok száma a egy adott figyelő. Jelenleg csak hello *alapvető* szabály használata támogatott. Hello *alapvető* szabály-e időszeletelés terheléselosztási.

**További konfigurációs megjegyzések**

SSL-tanúsítványok beállítása, a hello protokoll **HttpListener** túl kell módosítani*Https* (kis-és nagybetűket). Hello **SslCert** elem túl kerül**HttpListener** a hello értéke toohello azonos hello feltöltése az előző SSL-tanúsítványok szakaszban használt nevet. hello előtér-port frissített too443 lehet.

**tooenable cookie-alapú kapcsolat**: Alkalmazásátjáró lehet győződjön meg arról, hogy egy kérelem egy ügyfél-munkamenetből mindig irányított toohello konfigurált tooensure hello webfarm azonos virtuális gép. Ebben a forgatókönyvben történik, hogy az egy munkamenetcookie-t, amely lehetővé teszi annak megfelelően hello átjáró toodirect forgalom. cookie-alapú tooenable affinitás beállítása **CookieBasedAffinity** túl*engedélyezve* a hello **BackendHttpSettings** elemet.

A konfigurációs hogyan hozhat létre, vagy hozzon létre egy konfigurációs objektumot, vagy egy konfigurációs XML-fájl használatával.
tooconstruct egy konfigurációs XML-fájl használatával a konfigurációt használja következő minta hello:

**Konfigurációs XML-minta**

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

## <a name="set-hello-gateway-configuration"></a>Hello átjáró konfigurációjának beállítása

Ezután állítsa be hello Alkalmazásátjáró. Használhatja a hello `Set-AzureApplicationGatewayConfig` parancsmag vagy a konfigurációs objektum vagy egy konfigurációs XML-fájlt.

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml
```

## <a name="start-hello-gateway"></a>Indítsa el a hello átjáró

Ha hello átjáró van konfigurálva, a hello `Start-AzureApplicationGateway` parancsmag toostart hello átjáró. Alkalmazásátjáró számlázás megkezdése után hello átjáró sikeresen elindult.

> [!NOTE]
> Hello `Start-AzureApplicationGateway` parancsmag is igénybe vehet fel toofinish too15-20 perc.
>
>

```powershell
Start-AzureApplicationGateway AppGwTest
```

## <a name="verify-hello-gateway-status"></a>Hello az átjáró állapotának megerősítése

Használjon hello `Get-AzureApplicationGateway` parancsmag toocheck hello hello átjáró állapotának. Ha `Start-AzureApplicationGateway` hello a korábbi lépésben sikeres *állapot* kell futnia, és *virtualip értékek* és *DnsName* érvényes bejegyzést kell rendelkeznie.

Ez a példa bemutatja, hogy működik-e, fut, és készen áll a tootake forgalom Alkalmazásátjáró.

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

## <a name="next-steps"></a>Következő lépések

Ha további általános információra van szüksége a terheléselosztás beállításaival kapcsolatban:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

