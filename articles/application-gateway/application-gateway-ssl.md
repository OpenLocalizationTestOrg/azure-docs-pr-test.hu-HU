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
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-classic-deployment-model"></a>SSL kiszervezési Alkalmazásátjáró konfigurálása a klasszikus telepítési modell segítségével

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
> * [Klasszikus Azure PowerShell](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Az Azure Application Gateway konfigurálható úgy, hogy leállítsa a Secure Sockets Layer (SSL) munkamenetét az átjárónál, így elkerülhetők a költséges SSL visszafejtési feladatok a webfarmon. Az SSL-alapú kiszervezés emellett leegyszerűsíti az előtér-kiszolgáló számára webalkalmazás telepítését és kezelését.

## <a name="before-you-begin"></a>Előkészületek

1. Telepítse az Azure PowerShell-parancsmagok legújabb verzióját a Webplatform-telepítővel. A [Letöltések lap](https://azure.microsoft.com/downloads/) **Windows PowerShell** szakaszából letöltheti és telepítheti a legújabb verziót.
2. Ellenőrizze, hogy rendelkezik-e működő virtuális hálózattal és hozzá tartozó érvényes alhálózattal. Győződjön meg arról, hogy egy virtuális gép vagy felhőalapú telepítés sem használja az alhálózatot. Az Application Gateway-nek egyedül kell lennie a virtuális hálózat alhálózatán.
3. A kiszolgálóknak, amelyeket az Application Gateway használatára konfigurál, már létezniük kell, illetve a virtuális hálózatban vagy hozzárendelt nyilvános/virtuális IP-címmel létrehozott végpontokkal kell rendelkezniük.

Az Alkalmazásátjáró SSL kiszervezési konfigurálásához hajtsa végre az alábbi lépéseket a megadott sorrendben:

1. [Alkalmazásátjáró létrehozása](#create-an-application-gateway)
2. [SSL-tanúsítványok feltöltése](#upload-ssl-certificates)
3. [Az átjáró konfigurálása](#configure-the-gateway)
4. [Állítsa be az átjáró konfigurálása](#set-the-gateway-configuration)
5. [Az átjáró elindítása](#start-the-gateway)
6. [Az átjáró állapotának megerősítése](#verify-the-gateway-status)

## <a name="create-an-application-gateway"></a>Application Gateway létrehozása

Az átjáró létrehozásához használja a `New-AzureApplicationGateway` parancsmagot, és cserélje le az értékeket a saját értékeire. Az átjáró használati díjának felszámolása ekkor még nem kezdődik el. A használati díj felszámolása egy későbbi lépésnél kezdődik, amikor az átjáró sikeresen elindul.

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

Az átjáró létrehozásának ellenőrzéséhez használhatja a `Get-AzureApplicationGateway` parancsmagot.

A minta *leírás*, *InstanceCount*, és *GatewaySize* opcionális paraméterek. Az *InstanceCount* alapértelmezett értéke 2, a maximális értéke pedig 10. A *GatewaySize* alapértelmezett értéke Közepes. Kis és nagy más elérhető értékek. A *VirtualIPs* és a *DnsName* paraméterek azért üresek, mert az átjáró még nem indult el. Ezek az értékek jönnek létre, ha az átjáró már szerepel a futó állapotot.

```powershell
Get-AzureApplicationGateway AppGwTest
```

## <a name="upload-ssl-certificates"></a>SSL-tanúsítványok feltöltése

Használjon `Add-AzureApplicationGatewaySslCertificate` töltse fel a kiszolgálói tanúsítvány *pfx* formátum az Alkalmazásátjáró. A tanúsítvány nevét a felhasználó által választott név, és az Alkalmazásátjáró belül egyedieknek kell lenniük. Ezt a tanúsítványt ezt a nevet a tanúsítvány az alkalmazás-átjárón összes felügyeleti művelet is hivatkozik.

Ez a következő példa bemutatja a parancsmag cserélje le a mintában szereplő értékekkel saját.

```powershell
Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path to pfx file>
```

A következő érvényesítse a tanúsítvány feltöltése. Használja a `Get-AzureApplicationGatewayCertificate` parancsmag.

Ez a példa bemutatja a parancsmag az első sorba a kimeneti követ.

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
> A tanúsítvány jelszava nem lehet 4 – 12 karakterből, betűk és számok között. Speciális karakterek használata nem engedélyezett.

## <a name="configure-the-gateway"></a>Az átjáró konfigurálása

Egy alkalmazás átjáró konfigurálása több érték áll. Az értékek is kötődik együtt a konfiguráció létrehozásához.

Az értékek a következők:

* **Háttér-kiszolgálókészlet:** A háttérkiszolgálók IP-címeinek listája. A listán szereplő IP-címeknek a virtuális hálózat alhálózatához kell tartozniuk, vagy nyilvános/virtuális IP-címnek kell lenniük.
* **Háttér-kiszolgálókészlet beállításai:** Minden készletnek vannak beállításai, például port, protokoll vagy cookie-alapú affinitás. Ezek a beállítások egy adott készlethez kapcsolódnak, és a készlet minden kiszolgálójára érvényesek.
* **Előtérbeli port:** Az Application Gateway-en megnyitott nyilvános port. Amikor a forgalom eléri ezt a portot, a port átirányítja az egyik háttérkiszolgálóra.
* **Figyelő:** A figyelő egy előtérbeli porttal, egy protokollal (Http vagy Https, a kis- és a nagybetűk megkülönböztetésével) és SSL tanúsítványnévvel rendelkezik (SSL-kiszervezés konfigurálásakor).
* **Szabály:** A szabály összeköti a figyelőt és a háttérkiszolgáló-készletet, és meghatározza, hogy mely háttérkiszolgáló-készletre legyen átirányítva a forgalom, ha elér egy adott figyelőt. Jelenleg csak a *basic* szabály támogatott. A *basic* szabály a ciklikus időszeleteléses terheléselosztás.

**További konfigurációs megjegyzések**

Az SSL-tanúsítványok konfigurálásához *Https*-re kell módosítani a **HttpListener** protokollját (megkülönböztetve a kis- és nagybetűket). A **SslCert** elem hozzáadódik **HttpListener** az érték azonos nevű feltöltésének használt megelőző SSL-tanúsítványok szakasz. Az előtérbeli portot frissíteni kell 443-ra.

**A cookie-alapú affinitás engedélyezése**: Egy Application Gateway konfigurálható úgy, hogy egy ügyfélmunkamenetből érkező kérelmet mindig a webfarm ugyanazon virtuális gépére irányítsa. Ez a forgatókönyv egy munkameneti cookie beszúrásával lesz végrehajtva, amely lehetővé teszi az átjáró számára a forgalom megfelelő irányítását. A cookie-alapú affinitás engedélyezéséhez a **CookieBasedAffinity** paraméter beállítása legyen *Enabled* a **BackendHttpSettings** elemen belül.

A konfigurációs hogyan hozhat létre, vagy hozzon létre egy konfigurációs objektumot, vagy egy konfigurációs XML-fájl használatával.
Összeállíthatja a konfigurációs XML konfigurációs fájl használatával, használja a következő mintát:

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

## <a name="set-the-gateway-configuration"></a>Állítsa be az átjáró konfigurálása

Ezután állítsa be az Alkalmazásátjáró. Használhatja a `Set-AzureApplicationGatewayConfig` parancsmag vagy a konfigurációs objektum vagy egy konfigurációs XML-fájlt.

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml
```

## <a name="start-the-gateway"></a>Az átjáró indítása

Az átjáró konfigurálása után indítsa el az átjárót a `Start-AzureApplicationGateway` parancsmaggal. Az Application Gateway használati díjának felszámolása az átjáró sikeres indítása után kezdődik.

> [!NOTE]
> A `Start-AzureApplicationGateway` parancsmag futtatása akár 15–20 percet is igénybe vehet.
>
>

```powershell
Start-AzureApplicationGateway AppGwTest
```

## <a name="verify-the-gateway-status"></a>Az átjáró állapotának ellenőrzése

Ellenőrizze az átjáró állapotát a `Get-AzureApplicationGateway` parancsmaggal. Ha `Start-AzureApplicationGateway` sikeres volt az előző lépésben *állapot* kell futnia, és *virtualip értékek* és *DnsName* érvényes bejegyzést kell rendelkeznie.

Ez a példa bemutatja, hogy működik-e, fut, és készen áll forgalom elkészítésére Alkalmazásátjáró.

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

