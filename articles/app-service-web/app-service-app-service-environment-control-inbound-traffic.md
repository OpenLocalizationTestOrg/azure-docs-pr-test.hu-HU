---
title: "aaaHow tooControl bejövő forgalom tooan App Service Environment-környezet"
description: "További információk a hogyan tooconfigure hálózati biztonsági szabályok toocontrol bejövő forgalom tooan App Service Environment-környezet."
services: app-service
documentationcenter: 
author: ccompy
manager: erikre
editor: 
ms.assetid: 4cc82439-8791-48a4-9485-de6d8e1d1a08
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: stefsch
ms.openlocfilehash: e7c6e6201db6a1ea77f7a2eee29a3b5445175495
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocontrol-inbound-traffic-tooan-app-service-environment"></a>Hogyan tooControl bejövő forgalom tooan App Service Environment-környezet
## <a name="overview"></a>Áttekintés
Az App Service-környezetek hozhatók létre **vagy** az Azure Resource Manager virtuális hálózati **vagy** klasszikus telepítési modell [virtuális hálózati][virtualnetwork].  Egy új virtuális hálózat és az új alhálózat hello létrehozásakor az App Service-környezetek adhatók meg.  Másik lehetőségként az App Service-környezetek is létrehozható egy már meglévő virtuális hálózat és a korábban létrehozott alhálózati.  2016. június a módosítások a ASEs is is telepíthető az nyilvános címtartományt, vagy RFC1918 címterek (azaz Magáncímeket) használó virtuális hálózatok.  További információ az App Service-környezetek létrehozásáról: [hogyan tooCreate az App Service-környezetek][HowToCreateAnAppServiceEnvironment].

Az App Service-környezetek mindig kell létrehozni egy alhálózaton belül mert alhálózat biztosít egy olyan hálózathatárhoz használt toolock bejövő forgalom felsőbb rétegbeli eszközök és a szolgáltatások le lesz úgy, hogy a HTTP és HTTPS-forgalom csak elfogadható meghatározott felsőbb rétegbeli IP-címeket.

Bejövő és kimenő hálózati forgalom az alhálózat segítségével kezelhető egy [hálózati biztonsági csoport][NetworkSecurityGroups]. Bejövő forgalom vezérlése igényel a hálózati biztonsági szabályok létrehozása a hálózati biztonsági csoport, és ezután rendelje hozzá a hello hálózati biztonsági csoport hello alhálózati tartalmazó hello App Service Environment-környezet.

Ha hálózati biztonsági csoport tooa alhálózathoz van hozzárendelve, a bejövő forgalom tooapps az App Service Environment-környezet, engedélyezett vagy letiltott hello alapján hello engedélyezése, és a megtagadási hello hálózati biztonsági csoport meghatározott szabályokat.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="inbound-network-ports-used-in-an-app-service-environment"></a>Szerepel az App Service-környezetek bejövő hálózati portok
A hálózati biztonsági csoportok bejövő hálózati forgalom zárolása, mielőtt azt egy fontos tooknow hello egy App Service Environment-környezet által használt szükséges és választható hálózati portok.  Ki a forgalom toosome portok véletlenül bezárja azt eredményezheti, hogy működésképtelenné egy App Service Environment-környezetben.

hello egy App Service Environment-környezet által használt portok listája látható. Minden portjait **TCP**, ha nincs másként jelezve egyértelműen:

* 454: **port szükséges** kezelése és karbantartása az App Service Environment-környezetek SSL-en keresztüli Azure-infrastruktúra használják.  Forgalom toothis portot nem blokkolja.  Ez a port nem mindig kötött toohello nyilvános VIP egy hajlamosnak.
* 455: **port szükséges** kezelése és karbantartása az App Service Environment-környezetek SSL-en keresztüli Azure-infrastruktúra használják.  Forgalom toothis portot nem blokkolja.  Ez a port nem mindig kötött toohello nyilvános VIP egy hajlamosnak.
* 80: alapértelmezett port a HTTP-forgalom bejövő tooapps App Service-csomag egy App Service Environment-környezetben futtatja.  A ILB-kompatibilis ASE Ez a port nem kötött toohello ILB címe hello ASE.
* 443-as: alapértelmezett port a bejövő SSL forgalom tooapps App Service-csomag egy App Service Environment-környezetben futtatja.  A ILB-kompatibilis ASE Ez a port nem kötött toohello ILB címe hello ASE.
* 21: FTP-vezérlőcsatorna.  Ez a port biztonságosan blokkolható, ha az FTP nem használja.  Ezt a portot egy ILB-kompatibilis ASE lehetnek egy ASE kötött toohello ILB címet.
* 990: ftps-t a vezérlőcsatornához.  Ez a port biztonságosan blokkolható, ha még nem használ ftps-t.  Ezt a portot egy ILB-kompatibilis ASE lehetnek egy ASE kötött toohello ILB címet.
* 10001-10020: FTP-adatcsatornákhoz.  Mivel a hello vezérlőcsatorna, ezeket a portokat biztonságosan blokkolható Ha FTP nincs használatban.  Ezt a portot egy ILB-kompatibilis ASE lehetnek kötött toohello ASE tartozó ILB cím.
* 4016: a Visual Studio 2012 távoli hibakereséshez.  Ez a port biztonságosan blokkolható Ha hello szolgáltatást nem használja.  A ILB-kompatibilis ASE Ez a port nem kötött toohello ILB címe hello ASE.
* 4018: a Visual Studio 2013 távoli hibakereséshez.  Ez a port biztonságosan blokkolható Ha hello szolgáltatást nem használja.  A ILB-kompatibilis ASE Ez a port nem kötött toohello ILB címe hello ASE.
* 4020: a Visual Studio 2015-höz távoli hibakereséshez.  Ez a port biztonságosan blokkolható Ha hello szolgáltatást nem használja.  A ILB-kompatibilis ASE Ez a port nem kötött toohello ILB címe hello ASE.

## <a name="outbound-connectivity-and-dns-requirements"></a>Kimenő kapcsolatok és a DNS-követelmények
Az App Service Environment-környezet toofunction megfelelően, azt is szükséges kimenő hozzáférést toovarious végpontok. Hello külső végpontok egy ASE által használt teljes listája megtalálható-e hello hello "A hálózati kapcsolat szükséges" szakasza [ExpressRoute tartozó fürthálózat-konfiguráció](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) cikk.

App Service-környezetek hello virtuális hálózat egy érvényes DNS-infrastruktúra szükséges.  Ha bármilyen okból hello DNS-konfiguráció esetén megváltozik egy App Service Environment-környezet létrehozása után, a fejlesztők kényszerítheti az App Service Environment-környezet toopick hello új DNS-konfiguráció.  Működés közbeni környezet újraindítás hello App Service Environment-környezet felügyeleti panel az hello hello tetején található ikon "Restart" hello segítségével váltanak [Azure-portálon] [ NewPortal] hello környezet toopick, akkor másolatot hello új DNS-konfiguráció.

Ajánlott továbbá, hogy egyéni DNS-kiszolgálók hello a virtuális hálózaton kell előre idő előzetes toocreating az App Service-környezetek beállítása.  Ha egy virtuális hálózat DNS-konfiguráció megváltozott egy App Service Environment-környezet létrehozása közben, hello App Service Environment-környezet létrehozása folyamat sikertelen vezethet.  Egy hasonló tekintettel az egyéni DNS-kiszolgáló létezik a hello másik végén egy VPN gateway és hello DNS-kiszolgáló esetén nem érhető el vagy nem érhető el, hello App Service Environment-környezet létrehozása is sikertelen lesz.

## <a name="creating-a-network-security-group"></a>Hálózati biztonsági csoport létrehozása
Teljes hogyan hálózati biztonsági csoportok témakör hello következő [információk][NetworkSecurityGroups].  hálózati biztonsági csoportok konfigurálása és alkalmazása az olyan hálózati biztonsági csoport tooa alhálózatot, amely tartalmazza az App Service Environment-környezet egy aktuális alábbi simításokat a hello Azure szolgáltatásfelügyelet példa mutatja be.

**Megjegyzés:** hálózati biztonsági csoport konfigurálható grafikusan segítségével hello [Azure Portal](https://portal.azure.com) vagy Azure PowerShell használatával.

Hálózati biztonsági csoportok először egy előfizetéshez tartozó önálló egységként jönnek létre. Mivel a hálózati biztonsági csoportok Azure-régióban jönnek létre, győződjön meg arról, adott hello hálózati biztonsági csoport jön létre a hello azonos régióban, az App Service Environment-környezet hello.

hello következő bemutatja a hálózati biztonsági csoport létrehozása:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Hálózati biztonsági csoport létrehozása után egy vagy több hálózati biztonsági szabályok hozzáadása történik meg tooit.  Mivel hello meghatározott szabályok idővel megváltozhat, javasolt kimenő számozási hello toospace használt szabály prioritások toomake azt könnyen tooinsert további szabályok adott idő alatt.

hello az alábbi példában egy szabályt, amely explicit módon engedélyezi a hozzáférést toohello kezelőportjai hello Azure-infrastruktúra toomanage igényli, és az App Service-környezetek karbantartásához.  Ne feledje, hogy az összes felügyeleti adatforgalmat SSL-en keresztül zajló kommunikációról védi-e ügyféltanúsítványokat, ezért annak ellenére, hogy hello portokat bármely Azure felügyeleti infrastruktúra eltérő entitás nem érhető el.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP


Amikor zárolása hozzáférés tooport 80-as és 443-as túl "elrejtőzni" felsőbb rétegbeli eszközök és szolgáltatások mögött egy App Service-környezet, tooknow hello felsőbb rétegbeli IP-címet kell.  Például ha webalkalmazási tűzfal (WAF) használ, hello WAF lesz a saját IP-cím (vagy címek) használó, amikor proxy használatát az ügynökökön forgalom tooa alárendelt App Service Environment-környezet.  Szüksége lesz toouse az IP-címének hello *SourceAddressPrefix* hálózati biztonsági szabály paraméter.

A hello az alábbi példában egy adott felsőbb rétegbeli IP-címről érkező bejövő forgalom kifejezetten engedélyezett.  cím hello *1.2.3.4* hello IP-címét egy fölérendelt WAF helyőrzőként szolgál.  Hello érték toomatch hello címet a felsőbb rétegbeli eszköz vagy a szolgáltatás által használt módosítani.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTP" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTPS" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

FTP-támogatás van szükség, ha a következő szabályok hello használható egy sablon toogrant hozzáférési toohello FTP vezérlő port és a channel-porttal.  Mivel FTP állapot-nyilvántartó protokoll, nem lehet egy hagyományos HTTP/HTTPS tűzfalhoz vagy proxyhoz eszközön keresztül képes tooroute FTP-forgalmat.  Ebben az esetben szüksége lesz tooset hello *SourceAddressPrefix* tooa eltérő érték - például hello IP-címtartományt fejlesztői vagy a központi telepítési gépek mely FTP ügyfelek futnak. 

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPCtrl" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '21' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPDataRange" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '10001-10020' -Protocol TCP

(**Megjegyzés:** hello adatcsatorna porttartománya hello próbaidőszak alatt módosíthatja.)

Távoli hibakeresés a Visual Studio használata esetén a következő szabályok hello bemutatják, hogyan férnek hozzá az toogrant.  Van külön szabály minden Visual Studio támogatott verziója óta egyes verzióihoz távoli hibakereséshez másik portot használ.  FTP-hozzáférést, a távoli hibakeresési forgalom előfordulhat, hogy nem megfelelő sorrendben egy hagyományos WAF vagy a proxy-eszközön keresztül.  Hello *SourceAddressPrefix* helyette toohello IP-címtartomány futó Visual Studio developer gépek állítható be.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2012" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4016' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2013" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4018' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2015" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4020' -Protocol TCP

## <a name="assigning-a-network-security-group-tooa-subnet"></a>A hálózati biztonsági csoport tooa alhálózati hozzárendelése
Hálózati biztonsági csoport tartozik egy alapértelmezett biztonsági szabály, amely megtagadja a hozzáférést tooall külső forgalom.  hello hálózati biztonsági szabályok a fent említett kombinálásával kapott eredmény hello hello alapértelmezett biztonsági szabály blokkolja a bejövő forgalmat, hogy csak a forrás címtartományokból forgalom társított nem egy *engedélyezése* művelet tudnak toosend forgalom tooapps fut egy App Service Environment-környezetben.

Hálózati biztonsági csoport biztonsági szabályai a telepítéskor, miután hozzárendelt toobe toohello alhálózati tartalmazó hello App Service Environment-környezet van szüksége.  hello hozzárendelés parancs hello virtuális hálózat hello App Service Environment-környezet tartalmazó, valamint hello neve hello alhálózat, amelyen létrehozták hello App Service Environment-környezet hello nevet is hivatkozik.  

az alábbi példa hello folyik a hozzárendelése tooa alhálózat és a virtuális hálózat hálózati biztonsági csoport jeleníti meg:

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

Miután hello hálózati biztonsági csoport a helyhozzárendelés képes lesz (hello hozzárendelés egy hosszú futású műveleteket és eltarthat néhány percig toocomplete), csak a forgalom megfelelő bejövő *engedélyezése* szabályok sikeresen eléri a hello App alkalmazást Service-környezet.

A teljesség hello a következő példa bemutatja, hogyan tooremove, ezért nem társítható hello hálózati biztonsági csoport hello alhálózatból:

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Remove-AzureNetworkSecurityGroupFromSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

## <a name="special-considerations-for-explicit-ip-ssl"></a>Explicit IP-SSL szempontjai
Ha egy alkalmazás explicit IP-SSL-címmel van konfigurálva (alkalmazható *csak* tooASEs, amelyek egy nyilvános VIP), hello alapértelmezett IP-címe hello App Service Environment-környezet helyett a HTTP és HTTPS forgalmat, hello alhálózati folytatódjon egy készleten különböző portok eltérő 80-as és 443-as portot.

egyes pár hello minden IP-SSL cím által használt portok hello portál felhasználói felületen hello App Service Environment részletek UX paneljén található.  Jelölje be "az összes beállítások"--> "IP-címek".  "IP-címek" Hello megjelenik egy tábla összes hello App Service Environment-környezet, explicit módon konfigurált IP-SSL címeinek együtt hello speciális port párt használt tooroute HTTP és HTTPS-forgalom társított minden IP-SSL cím.  A port pár, amelyet a toobe hello DestinationPortRange paraméterek használja, amikor a szabályok konfigurálása a hálózati biztonsági csoport.

Amikor egy alkalmazás egy ASE konfigurált toouse IP-SSL, a külső ügyfeleket nem jelenik meg, és nem kell tooworry hello különleges pár porthozzárendelés kapcsolatos.  Forgalom toohello alkalmazások általában erdőtől áramolnak toohello konfigurált IP-SSL cím.  hello fordítási toohello speciális port pár automatikusan történik, belső hello adatforgalom utolsó szakasza során hello alhálózati tartalmazó hello ASE be. 

## <a name="getting-started"></a>Bevezetés
Lásd az App Service-környezetek lépései tooget [bemutatása tooApp Service-környezet][IntroToAppServiceEnvironment]

Összes cikket, és hogyan-a következőre az App Service Environment-környezetek érhetők el hello [alkalmazásszolgáltatási környezetek – fontos fájl](../app-service/app-service-app-service-environments-readme.md).

Biztonságosan csatlakozik egy App Service Environment-környezet toobackend erőforrás alkalmazások körül, lásd: [tooBackend erőforrások biztonságosan csatlakozik egy App Service Environment-környezet][SecurelyConnecttoBackend]

Hello Azure App Service platformmal kapcsolatos további információkért lásd: [Azure App Service][AzureAppService].

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[SecurelyConnecttoBackend]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NewPortal]:  https://portal.azure.com  

<!-- IMAGES -->

