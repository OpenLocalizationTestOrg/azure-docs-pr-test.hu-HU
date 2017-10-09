---
title: "Gyakori kérdések az Azure Application Gateway aaaFrequently |} Microsoft Docs"
description: "Ezen a lapon választ ad toofrequently Azure Application Gateway kapcsolatos kérdések"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: d54ee7ec-4d6b-4db7-8a17-6513fda7e392
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: gwallace
ms.openlocfilehash: b2df3a82a71a3264d3d34d317d08e4b4f72c6e3e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-for-application-gateway"></a>Az Alkalmazásátjáró gyakori kérdések

## <a name="general"></a>Általános kérdések

**Q. Mi az Application Gateway?**

Azure Application Gateway egy alkalmazás kézbesítési vezérlő LÉPETT szolgáltatásként, az alkalmazások képességei terheléselosztási különböző réteg 7 kínál. Magas rendelkezésre állású és méretezhető szolgáltatást, amely teljes mértékben kezeli az Azure biztosít.

**Q. Milyen funkciókat támogatja az Alkalmazásátjáró?**

Alkalmazásátjáró támogatja SSL kiszervezésével és a záró tooend SSL, webalkalmazási tűzfal, munkamenet cookie-alapú kapcsolat, URL-cím elérési út-alapú útválasztási, több hely üzemeltetéséhez és mások számára. Támogatott szolgáltatások teljes listájának megtekintéséhez keresse fel a [bemutatása tooApplication átjáró](application-gateway-introduction.md)

**Q. Mi az Application Gateway és az Azure Load Balancer hello különbségének?**

Alkalmazásátjáró 7 réteg terheléselosztó, amely azt jelenti, hogy működik együtt a csak internetes forgalmat (HTTP/HTTPS/WebSocket). Például az SSL-lezárást, a munkamenet cookie-alapú kapcsolat és a ciklikus multiplexelés képességek terheléselosztási forgalom támogatja. Terheléselosztó, kiegyensúlyozza forgalom rétegben 4 (TCP/UDP).

**Q. Milyen protokollokat támogatja az Alkalmazásátjáró?**

Alkalmazás-átjáró támogatja a HTTP, HTTPS és WebSocket.

**Q. Által támogatott ma háttérkészlet részeként?**

Háttérkészlet állhat hálózati adapter virtuálisgép-méretezési csoportok, nyilvános IP-címek, belső IP-címek, teljes tartománynév (FQDN) neve, és több-bérlős vissza-végpontok, például az Azure Web Apps. Az Alkalmazásátjáró háttér címkészletet tagjai nem tooan rendelkezésre állási csoport társítva. Háttér-készletek tagjai között, fürtök és adatközpontok, vagy lehet Azure-on kívüli mindaddig, amíg az IP-kapcsolattal rendelkeznek.

**Q. Milyen régiók érhető el hello szolgáltatást?**

Alkalmazásátjáró globális Azure minden területen érhető el. Rendszerben is elérhető [Azure Kína](https://www.azure.cn/) és [Azure Government](https://azure.microsoft.com/en-us/overview/clouds/government/)

**Q. Ez az előfizetésem dedikált telepítésének vagy azt megoszthatja ügyfelek?**

Alkalmazásátjáró egy dedikált központi telepítés a virtuális hálózat.

**Q. Van HTTP -> támogatott HTTPS átirányítása?**

Átirányítás használata támogatott. Látogasson el [Alkalmazásátjáró átirányítási áttekintése](application-gateway-redirect-overview.md) további toolearn.

**Q. Milyen sorrendben figyelői feldolgozása?**

Figyelők feldolgozása hello ahhoz, azok láthatók. Ezért ha egy alapszintű figyelő egy bejövő kérelem megfelel feldolgozza azt először.  Többhelyes figyelők úgy kell konfigurálni, mielőtt egy alapszintű figyelő tooensure forgalom irányított toohello megfelelő háttér.

**Q. Hol található Application Gateway IP- és DNS?**

A végpont egy nyilvános IP-címet használ, ha ezek az információk található hello nyilvános IP-cím erőforrás vagy hello – áttekintés oldalra hello Alkalmazásátjáró hello portálon. A belső IP-címek ez található hello áttekintése lapon.

**Q. Hello IP- vagy DNS változik az Alkalmazásátjáró hello hello élettartamuk során?**

hello VIP módosíthatja, ha hello átjáró leáll, majd hello ügyfél által indított. hello Alkalmazásátjáró társított DNS hello átjáró hello életciklusa során nem változik. Ezért az ajánlott toouse CNAME alias, majd mutasson az Alkalmazásátjáró hello toohello DNS-címét.

**Q. Alkalmazásátjáró támogatja a statikus IP-címet?**

Nem, az Alkalmazásátjáró nem támogatja a statikus nyilvános IP-címek, de statikus belső IP-címek támogatja.

**Q. Alkalmazásátjáró támogatja a több nyilvános IP-cím hello átjárón?**

Csak egy nyilvános IP-cím egy Application Gateway esetén támogatott.

**Q. Támogatja az Alkalmazásátjáró x-továbbított-a fejlécek?**

Igen, Alkalmazásátjáró beszúrása x-továbbított- esetén az x továbbított protokoll és az x továbbított port fejlécek hello kérelem a továbbított toohello háttér. hello x-továbbított-a fejléc formátuma IP:Port vesszővel tagolt listája. hello érvényes x továbbított protokoll értékei http vagy HTTPS protokollt. X továbbított port hello portot mely hello kérésére hello Alkalmazásátjáró címen érhető határozza meg.

**Q. Mennyi ideig tart toodeploy olyan átjárót? Az Alkalmazásátjáró továbbra is működik, ha frissítése során?**

Új Alkalmazásátjáró telepítések too20 perc tooprovision is eltarthat. Nincsenek zavaró módosításokat tooinstance mérete és száma, és ebben az időszakban hello átjáró aktív marad.

## <a name="configuration"></a>Konfiguráció

**Q. Alkalmazásátjáró mindig telepítve van a virtuális hálózaton?**

Igen, az Alkalmazásátjáró mindig a rendszer a virtuális hálózati alhálózat. Ez az alhálózat csak tartalmazhat Alkalmazásátjárót.

**Q. Alkalmazásátjáró működik a virtuális hálózaton kívüli tooinstances?**

Alkalmazásátjáró hello mindaddig, amíg nincs IP-kapcsolat van virtuális hálózaton kívüli tooinstances működik. Ha azt tervezi, toouse belső IP-címek, a háttér a készlet tagjainak, akkor azt igényli [VNETBEN társviszony-létesítés](../virtual-network/virtual-network-peering-overview.md) vagy [VPN-átjáró](../vpn-gateway/vpn-gateway-about-vpngateways.md).

**Q. Központi telepítését bármi más hello Alkalmazásátjáró alhálózat?**

Nem, de telepítheti a más alkalmazásátjárót hello alhálózat.

**Q. Hálózati biztonsági csoportok hello Alkalmazásátjáró alhálózaton támogatottak?**

A következő korlátozások hello hello Alkalmazásátjáró alhálózaton hálózati biztonsági csoportok használata támogatott:

* Kivételek kell elhelyezni, a bejövő forgalmat a háttérrendszer állapotfigyelő toowork 65503-65534 portok megfelelően.

* Nem blokkolható a kimenő internetkapcsolat.

* Engedélyezni kell az AzureLoadBalancer címke hello forgalmát.

**Q. Az Alkalmazásátjáró hello korlátai Tudom növelni a működés felső korlátjának?**

Látogasson el [alkalmazás átjáró korlátok](../azure-subscription-service-limits.md#application-gateway-limits) tooview hello korlátok.

**Q. Használhatok Alkalmazásátjáró külső és belső forgalmát egyidejűleg?**

Alkalmazásátjáró Igen, támogatja az egy belső IP-cím és egy külső IP-cím az Alkalmazásátjáró.

**Q. Támogatott Vnetben társviszony-létesítés?**

Igen, Vnetben társviszony-létesítés támogatott és hasznos a terheléselosztás forgalom más virtuális hálózatok.

**Q. I működik tooon helyszíni kiszolgálók expressroute-on vagy VPN-alagutat csatlakozáskor?**

Igen, mindaddig, amíg forgalom engedélyezve van.

**Q. Rendelkezhet eltérő portokon számos alkalmazás szolgál egy háttérkészletéből?**

Micro service-architektúra esetén támogatott. Több http konfigurált beállítások tooprobe eltérő portokon kellene.

**Q. Támogatják egyéni mintavételt helyettesítő karakterekkel vagy reguláris kifejezéssel az érkezett válasz adatait?**

Egyéni mintavételt nem támogatják a helyettesítő karakteres vagy regex érkezett válasz adatait a. 

**Q. Szabályok feldolgozásának módja?**

Szabályok feldolgozása hello sorrendben vannak konfigurálva. Javasoljuk, hogy többhelyes szabályok konfigurálva vannak-e, mielőtt alapvető szabályok tooreduce hello esélye annak, hogy az adatforgalom irányított toohello nem megfelelő háttér alapszintű hello szabály alapján előzetes toohello többhelyes portszabály értékelt forgalom megfelelő módon.

**Q. Szabályok feldolgozásának módja?**

Szabályok feldolgozása hello ahhoz, azok létrehozásakor. Javasoljuk, hogy a többhelyes szabályok előtt alapvető szabályok vannak konfigurálva. Többhelyes figyelői először konfigurálásával, ez a konfiguráció csökkenti a hello esélye annak, hogy a forgalom irányított toohello nem megfelelő háttér legyen. Alapszintű hello szabály alapján előzetes toohello többhelyes portszabály értékelt forgalom megfelelő útválasztási probléma fordulhatnak elő.

**Q. Mi az egyéni mintavételt hello a gazdagép mező jelölésére?**

A gazdagép mező hello neve toosend hello mintavételi a határozza meg. Alkalmazandó csak akkor, ha több hely van beállítva az alkalmazás-átjárón, ellenkező esetben használja a "127.0.0.1". Ez az érték eltér a virtuális gép állomásnevét, és formátumú \<protokoll\>://\<állomás\>:\<port\>\<elérési\>.

**Q. Engedélyezési lista Alkalmazásátjáró hozzáférés tooa telepíthetek kevés forrás IP-cím?**

Ebben a forgatókönyvben végezhető Alkalmazásátjáró alhálózaton NSG-ket használ. a következő korlátozások hello a prioritásuk szerinti sorrendben felsorolva hello hello alhálózaton kell rendezni:

* Engedélyezi a bejövő forgalom forrás IP-/ IP-címtartomány.

* Az összes források tooports 65503-65534 a bejövő kérések engedélyezése [háttér állapotfigyelő kommunikációja](application-gateway-diagnostics.md).

* Annak engedélyezése, hogy a bejövő Azure Load Balancer mintavételt (AzureLoadBalancer címke) és a bejövő virtuális hálózati forgalom (VirtualNetwork címke) hello [NSG](../virtual-network/virtual-networks-nsg.md).

* Megtagadási minden egyéb bejövő forgalom blokkolása minden szabály.

* Engedélyezze a kimenő forgalom toohello internet összes célhoz.

## <a name="performance"></a>Teljesítmény

**Q. Hogyan támogatja a Alkalmazásátjáró a magas rendelkezésre állás és méretezhetőség?**

Alkalmazásátjáró támogatja a magas rendelkezésre állás elérésére, ha két vagy több példányt. Azure ezek a példányok elosztása az összes példány nem meghiúsulhatnak hello frissítés és a tartalék tartományok tooensure ugyanannyi időt vesz igénybe. Alkalmazásátjáró méretezhetőség támogatja több példány hello hozzáadásával azonos átjáró tooshare hello terhelés.

**Q. Hogyan érhetők el vész-Helyreállítási forgatókönyv Alkalmazásátjáró adatközpontjaiban között?**

Az ügyfelek használhatják a Traffic Manager toodistribute forgalom különböző adatközpontokban több alkalmazás-átjáró között.

**Q. Automatikus skálázással támogatva van?**

Nem, de Application Gateway használható használt tooalert átviteli metrika, amikor egy küszöbértéket. Példányok hozzáadásának vagy kicserélésének mérete manuálisan hello átjáró nem indul újra, és nem befolyásolja a meglévő forgalom.

**Q. Fel/le OK állásidő, nem manuális méretezési?**

Állásidő nélkül, a frissítési tartományok és a tartalék tartományok között elosztott példányok.

**Q. Módosítható a közepes toolarge megszakítása nélkül példányméretének?**

Igen, Azure elosztja példányok nem meghiúsulhatnak, minden példány hello frissítés és a tartalék tartományok tooensure ugyanannyi időt vesz igénybe. Alkalmazás átjáró támogatja a több példánya hozzáadásával skálázás hello azonos átjáró tooshare hello terhelés.

## <a name="ssl-configuration"></a>SSL-beállítása

**Q. Milyen tanúsítványok Alkalmazásátjáró támogatottak?**

Önaláírt tanúsítványok, a CA-tanúsítványok, és a helyettesítő tanúsítványok támogatottak. EV tanúsítványok használata nem támogatott.

**Q. Mik azok a hello alkalmazás átjáró által támogatott aktuális titkosító csomagok?**

Az alábbiakban hello hello alkalmazás átjáró által támogatott aktuális titkosító csomagok. Látogasson el: [SSL konfigurálása házirend verziója és az Application Gateway titkosító csomagok](application-gateway-configure-ssl-policy-powershell.md) toolearn hogyan toocustomize SSL-beállítások.

- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
- TLS_DHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_DHE_RSA_WITH_AES_256_CBC_SHA
- TLS_DHE_RSA_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_AES_256_GCM_SHA384
- TLS_RSA_WITH_AES_128_GCM_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA256
- TLS_RSA_WITH_AES_128_CBC_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA
- TLS_RSA_WITH_AES_128_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_3DES_EDE_CBC_SHA
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA

**Q. Alkalmazásátjáró is támogatja a forgalom toohello háttér újbóli titkosítása?**

Igen, Alkalmazásátjáró támogatja az SSL kiszervezési és end tooend SSL, amely hello forgalom toohello háttér újra titkosítja.

**Q. Konfigurálhatja a SSL házirend toocontrol SSL protokoll verziója?**

Igen, konfigurálhatja az Application Gateway toodeny TLS1.0 TLS1.1 és TLS1.2. Az SSL 2.0 és 3.0 vannak már le van tiltva alapértelmezés szerint, és amelyek nem konfigurálhatók.

**Q. Titkosítási csomagok és a házirendek sorrendjének is konfigurálni?**

Igen, [titkosító csomagok konfigurációjának](application-gateway-ssl-policy-overview.md) esetén támogatott. Egyéni házirend meghatározása esetén titkosító csomagok a következő hello legalább egyikét engedélyezni kell. Alkalmazásátjáró SHA256 toofor háttér-kezelést használ.

* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
* TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_256_CBC_SHA256
* TLS_RSA_WITH_AES_128_CBC_SHA256

**Q. Hány SSL-tanúsítványok támogatottak?**

Másolatot too20 SSL támogatottak.

**Q. A háttérrendszer újbóli titkosítása hány tanúsítványhitelesítést biztosítsanak támogatottak?**

Too10 be a hitelesítési tanúsítványok az alapértelmezett érték 5 támogatottak.

**Q. Nem Alkalmazásátjáró integrálása az Azure Key Vault natív módon?**

Nem, nem integrálva van az Azure Key Vault.

## <a name="web-application-firewall-waf-configuration"></a>Webes alkalmazás tűzfalat (WAF) konfigurációja

**Q. Hello WAF SKU nyújtja a hello Standard Termékváltozat elérhető összes hello szolgáltatások?**

WAF Igen, a Standard Termékváltozat hello támogatja az összes hello szolgáltatás.

**Q. Mi az a hello CRS verziója támogatja a Alkalmazásátjáró?**

Alkalmazásátjáró támogatja CRS [program 2.2.9-es](application-gateway-crs-rulegroups-rules.md#owasp229) és CRS [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).

**Q. Hogyan figyelhetek WAF?**

WAF keresztül diagnosztikai naplózás felügyelet alatt, további információt a diagnosztikai naplózás helyen találhatók [diagnosztikai naplózás és az Alkalmazásátjáró metrikák](application-gateway-diagnostics.md)

**Q. Blokkolja az észlelési mód forgalom?**

Nem, a észlelési mód csak naplózza a forgalmat, amely egy WAF szabály elindul.

**Q. Hogyan testre szabhatja a WAF szabályokat?**

Igen, WAF szabályok lettek testre szabható, hogyan toocustomize őket látogasson el a további információt [testreszabása WAF csoportok és szabályok](application-gateway-customize-waf-rules-portal.md)

**Q. Milyen szabályok jelenleg érhetők el?**

WAF jelenleg CRS [program 2.2.9-es](application-gateway-crs-rulegroups-rules.md#owasp229) és [3.0](application-gateway-crs-rulegroups-rules.md#owasp30), első 10 biztonsági réseit által megnyitott webes alkalmazás biztonsági Project (OWASP) itt található hello nyújt elleni hello többségét eredeti biztonsági [OWASP felső 10 biztonsági réseket](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013)

* SQL-injektálás elleni védelem

* Webhelyek közötti, parancsprogramot alkalmazó támadások elleni védelem

* Gyakori webes támadások (például parancsinjektálás, HTTP-kéréscsempészet, HTTP-válaszfelosztás és távolifájl-beszúrásos támadás) elleni védelem

* HTTP protokoll megsértése elleni védelem

* HTTP protokollanomáliák (például hiányzó gazdagép-felhasználói ügynök és Accept (Elfogadás) fejlécek) elleni védelem

* Robotprogramok, webbejárók és képolvasók elleni védelem

* A gyakori alkalmazás konfigurációs hibák (Ez azt jelenti, hogy Apache, az IIS, stb.) észlelése

**Q. WAF is támogatja a DDoS megelőzési?**

WAF nem, nem biztosít DDoS megelőzése.

## <a name="diagnostics-and-logging"></a>Diagnosztika és naplózás

**Q. Milyen típusú naplók az Alkalmazásátjáró érhetők el?**

Nincsenek elérhető az Alkalmazásátjáró három naplókat. Ezek a naplók és más diagnosztikai képességek a további tudnivalókért keresse fel [háttér állapot, a diagnosztikai naplók és a metrikák az Alkalmazásátjáró](application-gateway-diagnostics.md).

- **ApplicationGatewayAccessLog** -hello hozzáférési napló kérelmet tartalmaz, minden küldött toohello Alkalmazásátjáró előtér. hello adatok hello hívó IP, kért, URL-cím válasz késés, bejövő és kimenő adatforgalma visszatérési kód, bájt. Hozzáférési napló gyűjtése 300 másodpercenként. Ez a napló Application Gateway-példányonként egy bejegyzést tartalmaz.
- **ApplicationGatewayPerformanceLog** -hello teljesítmény naplófájl rögzíti a teljesítményadatok alapon / példány teljes kérelem kiszolgálása, beleértve az átviteli sebesség (bájt), a kiszolgált kérelmek teljes száma a sikertelen kérelmek száma, a megfelelő és nem kifogástalan háttér-példányok száma.
- **ApplicationGatewayFirewallLog** -hello tűzfal a napló tartalmazza, amelyeket a rendszer a webalkalmazási tűzfal a konfigurált Alkalmazásátjáró észlelési vagy megelőzési módban kérelmeket.

**Q. Hogyan állapítható meg, ha a háttérkiszolgáló készlettagra megfelelő?**

Hello PowerShell-parancsmag `Get-AzureRmApplicationGatewayBackendHealth` vagy hello portálon keresztül állapotát ellenőrizni ellátogatva [Alkalmazásdiagnosztika átjáró](application-gateway-diagnostics.md)

**Q. Mi az az adatmegőrzési hello hello diagnosztika napló?**

Diagnosztikai naplók folyamat toohello ügyfelek tárfiók, és az ügyfelek hello megőrzési házirend alapján választaniuk be. Diagnosztikai naplók is küldhetők tooan Eseményközpont vagy Naplóelemzési. Látogasson el [átjáró Alkalmazásdiagnosztika](application-gateway-diagnostics.md) további részleteket.

**Q. Hogyan szerezhetek naplók az Alkalmazásátjáró?**

Az Alkalmazásátjáró naplók érhetők el. Hello portálon kattintson **tevékenységnapló** hello menü panelen a naplók Alkalmazásátjáró tooaccess hello. 

**Q. Beállíthatja a Alkalmazásátjáró riasztások?**

Igen, Alkalmazásátjáró támogatja a riasztások, értesítések metrikák ki vannak konfigurálva.  Alkalmazásátjáró "átviteli", amely lehet konfigurált tooalert metrika van. További információ a riasztások, toolearn látogasson el [riasztási értesítéseket](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

**Q. Háttér állapotfigyelő adja vissza állapota ismeretlen, mi okozza ezt az állapotot?**

hello leggyakoribb oka hozzáférés toohello háttér egy NSG-t vagy egyéni DNS-megjelenítését blokkolják. Látogasson el [háttér állapot, a diagnosztikai naplózás és a metrikák az Alkalmazásátjáró](application-gateway-diagnostics.md) további toolearn.

## <a name="next-steps"></a>Következő lépések

Alkalmazásátjáró olvashat toolearn [bemutatása tooApplication átjáró](application-gateway-introduction.md).
