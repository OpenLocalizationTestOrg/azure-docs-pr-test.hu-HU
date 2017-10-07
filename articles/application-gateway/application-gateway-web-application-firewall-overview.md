---
title: "aaaIntroduction tooweb alkalmazás tűzfalat (waf-ot) az Azure Application Gateway |} Microsoft Docs"
description: "Ez az oldal áttekintést nyújt az Application Gateway webalkalmazási tűzfalának (WAF) működéséről"
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: 04b362bc-6653-4765-86f6-55ee8ec2a0ff
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: amsriva
ms.openlocfilehash: 5a42ce0fb2bd12a391844099e2de8fa2571195e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="web-application-firewall-waf"></a>Webalkalmazási tűzfal (WAF)

A webalkalmazási tűzfal (WAF) az Application Gateway egyik szolgáltatása, amely központi védelmet nyújt a webalkalmazásoknak a gyakori biztonsági rések ellen. 

Webalkalmazási tűzfal hello szabályok alapján [OWASP core szabálykészletek](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 3.0 vagy a program 2.2.9-es. A webalkalmazások egyre inkább ki vannak téve rosszindulatú támadásoknak, amelyek az ismert biztonsági réseket használják ki. A biztonsági rések között közös SQL injektálási támadások, helyközi scripting támadások tooname néhány. Megakadályozza az ilyen jellegű támadások alkalmazáskód kihívást jelenthet, és előfordulhat, hogy szigorú karbantartása, javítását és ellenőrzés hello alkalmazás topológia több réteget. Központosított webalkalmazási tűzfal segít tisztázni biztonságkezelés jóval egyszerűbb, és lehetővé teszi a nagyobb megbízhatósági tooapplication rendszergazdák fenyegetések és a behatolás elleni. WAF megoldás is reagálhasson tooa biztonsági kockázatot jelentenek gyorsabb által egy ismert biztonsági rések egy központi helyen, és minden egyes webalkalmazás biztonságossá tétele érdekében. Meglévő alkalmazásátjárót könnyen lehet konvertált tooa webes alkalmazás engedélyezve van a tűzfal Alkalmazásátjáró.

![imageURLroute](./media/application-gateway-web-application-firewall-overview/WAF1.png)

Alkalmazásátjáró csúszóablakszerűen történik, az alkalmazás kézbesítési vezérlő és ajánlatok SSL-lezárást, munkamenet cookie-alapú kapcsolat, ciklikus multiplexelés terheléselosztási, tartalomalapú útválasztás, képes toohost több webhelyeket és biztonsági javításokat. Alkalmazásátjáró által kínált biztonsági fejlesztések közé tartozik a SSL házirendkezelés, záró tooend SSL támogatja. Az alkalmazásbiztonság most fokozni WAF (webalkalmazási tűzfal) közvetlenül hello LÉPETT ajánlat történő integrálását. Ez egy egyszerű tooconfigure központi helyen toomanage biztosít, és a webes alkalmazások közös webes biztonsági rések elleni védelmét.

## <a name="benefits"></a>Előnyök

Az alábbiakban hello Application Gateway és a webes alkalmazás tűzfal biztosító hello core előnyei:

### <a name="protection"></a>Védelem

* A webes alkalmazás webes biztonsági rések és toobackend kód módosítása nélkül támadások védelméhez.

* Több webkiszolgáló védelme hello alkalmazásokat azonos időben Alkalmazásátjáró mögött. Alkalmazásátjáró támogatja az üzemeltető too20 webhelyek mögött csak egyetlen átjáró, amely sikerült az összes védeni webes támadások ellen, és WAF fel.

### <a name="monitoring"></a>Figyelés

* Valós idejű WAF-naplók segítségével követheti nyomon a webalkalmazást fenyegető támadásokat. Ez a napló integrálva van [Azure figyelő](../monitoring-and-diagnostics/monitoring-overview.md) tootrack WAF riasztást, és naplózza, és könnyedén figyelheti a trendeket.

* A WAF hamarosan az Azure Security Centerrel is integrálva lesz. Az Azure Security Center lehetővé teszi, hogy az összes Azure-erőforrások biztonsági állapotának hello központi nézet.

### <a name="customization"></a>Testreszabás

* hello képességét toocustomize WAF szabályok és a szabály toosuit alkalmazás igényeinek csoportnak, és hogy a vakriasztások megszüntetéséhez.

## <a name="features"></a>Szolgáltatások

Webalkalmazási tűzfal rendelkezik előre beállított CRS 3.0 alapértelmezés szerint, vagy dönthet úgy toouse program 2.2.9-es. A CRS 3.0-s verziója esetén kevesebb hibás riasztással kell számolnia, mint a 2.2.9-es verziónál. képes túl hello[testreszabása szabályok toosuit igényeinek](application-gateway-customize-waf-rules-portal.md) valósul meg. Néhány hello közös webes biztonsági rések mely webalkalmazási tűzfal véd tartalmazza:

* SQL-injektálás elleni védelem
* Webhelyek közötti, parancsprogramot alkalmazó támadások elleni védelem
* Gyakori webes támadások (például parancsinjektálás, HTTP-kéréscsempészet, HTTP-válaszfelosztás és távolifájl-beszúrásos támadás) elleni védelem
* HTTP protokoll megsértése elleni védelem
* HTTP protokollanomáliák (például hiányzó gazdagép-felhasználói ügynök és Accept (Elfogadás) fejlécek) elleni védelem
* Robotprogramok, webbejárók és képolvasók elleni védelem
* Alkalmazások (vagyis Apache, IIS stb.) gyakori konfigurációs hibáinak észlelése

További szabályok részletes listáját és azok védelmét: hello következő [szabálykészletek alapvető](#core-rule-sets).

### <a name="core-rule-sets"></a>Alapvető szabálykészletek

Az Application Gateway a következő két szabálykészletet támogatja: CRS 3.0 és CRS 2.2.9. Ezek az alapvető szabálykészletek olyan szabályok gyűjteményei, amelyek megvédik a webalkalmazásokat a kártékony tevékenységek ellen.

#### <a name="owasp30"></a>OWASP_3.0

hello 3.0 core szabálykészlet megadott van 13 szabály hello a következő táblázatban ismertetett módon. Ezen szabálycsoportok mindegyike több, egyenként letiltható szabályt tartalmaz.

|Szabálycsoport|Leírás|
|---|---|
|**[REQUEST-910-IP-REPUTATION](application-gateway-crs-rulegroups-rules.md#crs910)**|Szabályok tooprotect ismert levélszemétküldők vagy rosszindulatú tevékenységet tartalmaz.|
|**[REQUEST-911-METHOD-ENFORCEMENT](application-gateway-crs-rulegroups-rules.md#crs911)**|Szabályok toolock módszereket tartalmazza (PUT, javítás <...)|
|**[REQUEST-912-DOS-PROTECTION](application-gateway-crs-rulegroups-rules.md#crs912)**| Tartalmazza a szabályok tooprotect szolgáltatásmegtagadásos (DoS) támadásokkal szemben.|
|**[REQUEST-913-SCANNER-DETECTION](application-gateway-crs-rulegroups-rules.md#crs913)**| Port és a környezet képolvasók elleni szabályok tooprotect tartalmazza.|
|**[REQUEST-920-PROTOCOL-ENFORCEMENT](application-gateway-crs-rulegroups-rules.md#crs920)**|Protokoll és a kódolás problémák elleni szabályok tooprotect tartalmazza.|
|**[REQUEST-921-PROTOCOL-ATTACK](application-gateway-crs-rulegroups-rules.md#crs921)**|Szabályok tooprotect fejléc injektálási, kérelmek és a felosztás válasz tartalmazza|
|**[REQUEST-930-APPLICATION-ATTACK-LFI](application-gateway-crs-rulegroups-rules.md#crs930)**|Fájl- és elérési útja támadások elleni szabályok tooprotect tartalmazza.|
|**[REQUEST-931-APPLICATION-ATTACK-RFI](application-gateway-crs-rulegroups-rules.md#crs931)**|Szabályok tooprotect szemben a távoli fájl befoglalási (RFI) tartalmazza.|
|**[REQUEST-932-APPLICATION-ATTACK-RCE](application-gateway-crs-rulegroups-rules.md#crs932)**|Szabályok tooprotect tartalmaz újra távoli kód végrehajtása.|
|**[REQUEST-933-APPLICATION-ATTACK-PHP](application-gateway-crs-rulegroups-rules.md#crs933)**|Szabályok tooprotect PHP injektálási támadások elleni tartalmazza.|
|**[REQUEST-941-APPLICATION-ATTACK-XSS](application-gateway-crs-rulegroups-rules.md#crs941)**|A helyközi, parancsfájlt alkalmazó támadások ellen védelmet biztosító szabályokat tartalmaz.|
|**[REQUEST-942-APPLICATION-ATTACK-SQLI](application-gateway-crs-rulegroups-rules.md#crs942)**|Az SQL-injektálási támadások ellen védelmet biztosító szabályokat tartalmaz.|
|**[REQUEST-943-APPLICATION-ATTACK-SESSION-FIXATION](application-gateway-crs-rulegroups-rules.md#crs943)**|Szabályok tooprotect munkamenet rögzítés támadások elleni tartalmazza.|

#### <a name="owasp229"></a>OWASP_2.2.9

hello program 2.2.9-es core szabálykészlet megadott van 10 szabály hello a következő táblázatban ismertetett módon. Ezen szabálycsoportok mindegyike több, egyenként letiltható szabályt tartalmaz.

|Szabálycsoport|Leírás|
|---|---|
|**[crs_20_protocol_violations](application-gateway-crs-rulegroups-rules.md#crs20)**|Tartalmazza a szabályok tooprotect elleni protokoll megsértése (érvénytelen karaktereket, GET egy kérelemtörzset, stb.)|
|**[crs_21_protocol_anomalies](application-gateway-crs-rulegroups-rules.md#crs21)**|Szabályok tooprotect elleni helytelen fejléc-információ tartalmazza.|
|**[crs_23_request_limits](application-gateway-crs-rulegroups-rules.md#crs23)**|Szabályok tooprotect argumentumok vagy fájlokat, amelyek mérete meghaladja a korlátozások tartalmazza.|
|**[crs_30_http_policy](application-gateway-crs-rulegroups-rules.md#crs30)**|Szabályok tooprotect korlátozott módszerek, fejlécek és fájltípusokat tartalmazza. |
|**[crs_35_bad_robots](application-gateway-crs-rulegroups-rules.md#crs35)**|Szabályok tooprotect szemben a szabványos és -képolvasók tartalmazza.|
|**[crs_40_generic_attacks](application-gateway-crs-rulegroups-rules.md#crs40)**|Tartalmazza a szabályok tooprotect általános támadások (munkamenet rögzítés távoli fájl befoglalási, PHP injektálási, stb.)|
|**[crs_41_sql_injection_attacks](application-gateway-crs-rulegroups-rules.md#crs41sql)**|Tartalmazza a szabályok tooprotect SQL injektálási támadások ellen|
|**[crs_41_xss_attacks](application-gateway-crs-rulegroups-rules.md#crs41xss)**|Szabályok tooprotect elleni közötti helyközi tartalmazza.|
|**[crs_42_tight_security](application-gateway-crs-rulegroups-rules.md#crs42)**|Tartalmaz egy szabály tooprotect elérési átjárás támadások ellen|
|**[crs_45_trojans](application-gateway-crs-rulegroups-rules.md#crs45)**|Szabályok tooprotect backdoor trójai programok ellen tartalmazza.|

### <a name="waf-modes"></a>WAF-üzemmódok

Alkalmazás átjáró WAF lehet a következő két mód hello konfigurált toorun:

* **Észlelési mód** – Ha az alkalmazás átjáró WAF észlelési módban konfigurált toorun figyeli, és az összes fenyegetés riasztás jelentkezik be tooa naplófájl. Az Alkalmazásátjáró naplózási diagnosztika kell bekapcsolni hello segítségével **diagnosztika** szakasz. Emellett szükség van, amely hello WAF tooensure napló be van jelölve, és kapcsolja be. Az észlelési üzemmódban futtatott webalkalmazási tűzfal nem blokkolja a bejövő kéréseket.
* **Megelőző módja** – Ha a beállított toorun megelőzési módban Application Gateway aktívan letiltja a szabályok által észlelt támadásokkal szemben. hello támadó megkapja a 403-as jogosulatlan hozzáférési kivétel és hello kapcsolat megszakad. Megelőző módja folytatódik toolog ilyen jellegű támadások hello WAF naplókat.

### <a name="application-gateway-waf-reports"></a>WAF-figyelés

Az Alkalmazásátjáró hello állapotának figyelése fontos. A webes alkalmazás tűzfal és hello alkalmazások, amelyek az általa védett hello állapotának figyelése szolgáltatáson keresztül naplózása és az Azure figyelő, az Azure Security Center (hamarosan elérhető) és Log Analyticshez való integráció.

![diagnosztika](./media/application-gateway-web-application-firewall-overview/diagnostics.png)

#### <a name="azure-monitor"></a>Azure Monitor

Az Application Gateway-naplók integrálva vannak az [Azure Monitorral](../monitoring-and-diagnostics/monitoring-overview.md),  Ez lehetővé teszi tootrack diagnosztikai adatokat – például WAF riasztások és a naplók.  Ez a funkció hello Alkalmazásátjáró erőforrás hello hello portál belül megadott **diagnosztika** lapon vagy keresztül hello Azure-figyelő szolgáltatás közvetlenül. az Alkalmazásátjáró diagnosztikai naplók engedélyezése olvashat toolearn [Alkalmazásátjáró diagnosztika](application-gateway-diagnostics.md)

#### <a name="azure-security-center"></a>Azure Security Center

[Az Azure Security Center](../security-center/security-center-intro.md) és nyújt segítséget megakadályozása, észleli, és a láthatóság növelésével toothreats válaszolni vezérlése hello Azure-erőforrások biztonsági. Az alkalmazásátjáró most már [integrálható az Azure Security Centerbe](application-gateway-integration-security-center.md). Az Azure Security Center ellenőrzése a környezetben nem védett toodetect webes alkalmazások. Azt is most ajánlott alkalmazás átjáró WAF tooprotect sebezhető erőforrásokról. Alkalmazás átjáró WAF közvetlenül az Azure Security Center hello hozhat létre.  Ezek a példányok WAF az Azure Security Center integrálva vannak, és elküldi a riasztásokat és állapotadatokat tooAzure Security Center biztonsági jelentéskészítéshez.

![1. ábra](./media/application-gateway-web-application-firewall-overview/figure1.png)

#### <a name="logging"></a>Naplózás

Az Application Gateway WAF részletes jelentéseket biztosít az összes észlelt fenyegetésről. A naplózás integrálva van az Azure Diagnostics naplóival, a riasztások pedig JSON formátumban vannak rögzítve. Ezek a naplók integrálhatók a [Log Analytics-be](../log-analytics/log-analytics-azure-networking-analytics.md).

![imageURLroute](./media/application-gateway-web-application-firewall-overview/waf2.png)

```json
{
  "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupId}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{appGatewayName}",
  "operationName": "ApplicationGatewayFirewall",
  "time": "2017-03-20T15:52:09.1494499Z",
  "category": "ApplicationGatewayFirewallLog",
  "properties": {
    "instanceId": "ApplicationGatewayRole_IN_0",
    "clientIp": "104.210.252.3",
    "clientPort": "4835",
    "requestUri": "/?a=%3Cscript%3Ealert(%22Hello%22);%3C/script%3E",
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "ruleId": "941320",
    "message": "Possible XSS Attack Detected - HTML Tag Handler",
    "action": "Blocked",
    "site": "Global",
    "details": {
      "message": "Warning. Pattern match \"<(a|abbr|acronym|address|applet|area|audioscope|b|base|basefront|bdo|bgsound|big|blackface|blink|blockquote|body|bq|br|button|caption|center|cite|code|col|colgroup|comment|dd|del|dfn|dir|div|dl|dt|em|embed|fieldset|fn|font|form|frame|frameset|h1|head|h ...\" at ARGS:a.",
      "data": "Matched Data: <script> found within ARGS:a: <script>alert(\\x22hello\\x22);</script>",
      "file": "rules/REQUEST-941-APPLICATION-ATTACK-XSS.conf",
      "line": "865"
    }
  }
} 

```

## <a name="application-gateway-waf-sku-pricing"></a>Application Gateway WAF – A termékváltozat díjszabása

A webalkalmazási tűzfal az új WAF termékváltozatban érhető el. A Termékváltozat csak Azure Resource Manager üzembe helyezési modell és hello klasszikus telepítési modell mellett nem érhető el. A WAF termékváltozat csak közepes és nagy méretű alkalmazásátjáró-példányokhoz használható. Az Alkalmazásátjáró összes hello korlátok toohello WAF SKU is érvényesek. A díjszabás az átjárópéldányok óránkénti díján és az adatfeldolgozási díjon alapul. A WAF termékváltozathoz tartozó óránkénti átjáródíj eltér a normál termékváltozat díjaitól, és az [Application Gateway díjszabását](https://azure.microsoft.com/pricing/details/application-gateway/) ismertető webhelyen tekinthető meg. Az adatfeldolgozás díjakat továbbra is hello azonos. Nincsenek szabályonként vagy szabálycsoportonként kiszabott díjak. Több webalkalmazás megvédheti mögött hello azonos webalkalmazási tűzfal, és nincs további díjakat támogatásához több alkalmazás van. 

A számlázás WAF a indul hatékonyan 5/5/2017, amíg WAF SKU átjárók továbbra is a normál díjszabás díjakon toobe majd hello.

## <a name="next-steps"></a>Következő lépések

Után további információk a WAF hello képességeit, látogasson el a [hogyan tooconfigure web Application Gateway alkalmazás tűzfala](application-gateway-web-application-firewall-portal.md).

