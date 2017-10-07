---
title: "Alkalmazásátjáró aaaIntroduction tooAzure |} Microsoft Docs"
description: "Ezen a lapon áttekintést hello Application Gateway szolgáltatás a réteg 7 terheléselosztást, beleértve az átjáró méretek, HTTP betöltése terheléselosztási, a cookie-alapú munkamenet-kapcsolatot, és SSL-kiszervezés."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: b37a2473-4f0e-496b-95e7-c0594e96f83e
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: gwallace
ms.openlocfilehash: c40c9dba64ab03d9f6f81b3cb8f26c6562ac26c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-application-gateway"></a>Application Gateway – áttekintés

A Microsoft Azure Application Gateway egy alkalmazáskézbesítési vezérlőszolgáltatást (ADC) biztosító dedikált virtuális berendezés, amely számos 7. rétegbeli terheléselosztási lehetőséget nyújt alkalmazásának. Ez lehetővé teszi az ügyfelek toooptimize webkiszolgáló farm termelékenység kiürítésével a CPU intenzív SSL lezárást toohello Alkalmazásátjáró. Emellett biztosítja az egyéb réteg 7 útválasztási lehetőségeit, köztük a bejövő forgalmat, a munkamenet cookie-alapú kapcsolat, a URL-cím elérési út-alapú útválasztási és hello képességét toohost ciklikus multiplexelés terjesztési mögött egyetlen alkalmazás átjáró több webhelyek. Webalkalmazási tűzfal (WAF) is megadva hello Alkalmazásátjáró WAF SKU részeként. Védelmet nyújt a közös webes biztonsági rések és biztonsági rések tooweb alkalmazások. Az Application Gateway szolgáltatást internetes átjáróként, csak belső használatú átjáróként vagy a kettő kombinációjaként lehet konfigurálni. 

![forgatókönyv](./media/application-gateway-introduction/scenario.png)

## <a name="features"></a>Szolgáltatások

Alkalmazásátjáró jelenleg hello a következő lehetőségeket biztosítja:


* **[Webalkalmazási tűzfal](application-gateway-webapplicationfirewall-overview.md)**  -hello webalkalmazási tűzfal (waf-ot) az Azure alkalmazás átjáró webalkalmazások védje a közös web-alapú támadások, például az SQL-injektálás, a többhelyes parancsfájlok futtatására és a munkamenet kihasználásának.
* **HTTP-terheléselosztás** – Az Application Gateway ciklikus időszeleteléses terheléselosztást biztosít. A terheléselosztás a 7. rétegben történik, és kizárólag a HTTP(S)-forgalmat érinti.
* **A munkamenet cookie-alapú kapcsolat** -hello munkamenet cookie-alapú affinitási szolgáltatás akkor hasznos, ha azt szeretné, hogy a felhasználó munkamenetének hello tookeep azonos háttér. Átjáró által kezelt cookie-k használatával hello Alkalmazásátjáró egy felhasználói munkamenet toohello későbbi forgalmát képes toodirect azonos háttér-feldolgozás. Ez a funkció fontos azokban az esetekben, ahol a munkamenet-állapot mentett hello háttér-kiszolgálófiók felhasználói munkamenet.
* **[Secure Sockets Layer (SSL) kiszervezési](application-gateway-ssl-arm.md)**  – Ez a szolgáltatás foglal hello költséges feladat fejti vissza a HTTPS-forgalmat a webkiszolgálók ki. SSL-kapcsolat hello Alkalmazásátjáró, és továbbítsa hello kérelem toohello server titkosítatlanul hello lezáró által hello webkiszolgálón van unburdened visszafejtési által.  Alkalmazásátjáró hello válasz újból titkosítja hátsó toohello ügyfél elküldése előtt. Ez a funkció olyan esetekben, ahol hello háttér-hasznos, a hello azonos védett virtuális hálózat, az Azure-ban Alkalmazásátjáró hello.
* **[TooEnd SSL end](application-gateway-backend-ssl.md)**  -Alkalmazásátjáró támogatja end forgalom tooend titkosítása. Alkalmazásátjáró ennek érdekében hello SSL-kapcsolat hello Alkalmazásátjáró leáll. hello átjáró majd hello útválasztási szabályok toohello forgalom újra titkosítja a hello csomagot, majd továbbítja hello csomagok toohello megfelelő háttér hello megadott útválasztási szabályok alapján alkalmazza. Hello webkiszolgáló válaszának végighalad hello hátsó toohello végfelhasználói ugyanezt a folyamatot.
* **[Tartalom útválasztási URL-alapú](application-gateway-url-route-overview.md)**  – Ez a szolgáltatás biztosít hello funkció toouse különböző háttér-kiszolgálók különböző adatforgalmi. Egy mappába a hello webkiszolgáló vagy egy CDN forgalom irányított tooa különböző háttér-lehet. Ez a lehetőség csökkenti azoknak a háttérkiszolgálóknak a felesleges terhelését, amelyek nem adott tartalmat szolgálnak ki.
* **[Többhelyes útválasztási](application-gateway-multi-site-overview.md)**  -Alkalmazásátjáró lehetővé teszi, hogy tooconsolidate too20-webhely üzemeltetését egyetlen Alkalmazásátjáró fel.
* **[Támogatás a Websocket](application-gateway-websocket.md)**  -Alkalmazásátjáró egy másik remek szolgáltatás hello támogatja a Websocket.
* **[Állapotfigyelés](application-gateway-probe-overview.md)**  -Alkalmazásátjáró biztosít háttér-erőforrások figyelési és egyéni alapértelmezett állapot-mintavételi csomagjai toomonitor pontosabb forgatókönyvek esetén.
* **[SSL-házirend és a Rejtjelek](application-gateway-ssl-policy-overview.md)**  – Ez a szolgáltatás hello alábbi képességekkel rendelkezik toolimit hello SSL protokoll verziói és hello titkosítási csomagok, amelyek támogatottak, ahol feldolgozásra kerülnek rendelés hello.
* **[Az átirányítási kérelem](application-gateway-redirect-overview.md)**  – Ez a szolgáltatás biztosít hello funkció tooredirect HTTP kérelmeket tooan HTTPS-figyelő.
* **[Több-bérlős háttérszolgáltatások támogatása](application-gateway-web-app-overview.md)** – Az Application Gateway támogatja olyan több-bérlős háttérszolgáltatások konfigurálását háttérkészlet-tagokként, mint az Azure Web Apps és az API Gateway. 
* **[Speciális diagnosztika](application-gateway-diagnostics.md)** – Az Application Gateway teljes diagnosztikát és hozzáférési naplókat biztosít. A tűzfalnaplók olyan Application Gateway-erőforrásokhoz érhetők el, amelyekhez engedélyezve van a WAF.

## <a name="benefits"></a>Előnyök

Az Application Gateway az alábbi esetekben hasznos:

* Kérelmek igénylő alkalmazások a hello ugyanazon felhasználó vagy Windows-ügyfélen munkamenet tooreach hello ugyanahhoz a háttér-virtuális géphez. Ilyenek például a bevásárlókocsi-alkalmazások vagy a webes levelezőkiszolgálók.
* Webkiszolgálófarmok mentesítése az SSL-lezárással járó többletterhelés alól.
* Alkalmazások, például egy tartalomkézbesítési hálózat, amelyhez azonos hosszan futó TCP-kapcsolati toobe irányíthatja át vagy elosztott terhelésű toodifferent háttérkiszolgálók betöltése hello több HTTP-kérelmekre.
* Olyan alkalmazásokhoz, amelyek támogatják a WebSocket-forgalmat.
* A webalkalmazások ismert webalapú támadásoktól, például az SQL-injektálástól, a helyközi, szkriptet alkalmazó támadásoktól és a munkamenet-eltérítésektől való megvédéséhez.
* A forgalom logikai elosztása a különböző útválasztási kritériumok alapján, mint például az URL-elérési út vagy a tartományfejlécek.

Az Application Gateway egy teljes körűen felügyelt Azure-szolgáltatás, amely skálázható és magas rendelkezésre állást kínál. Diagnosztikai és naplózási képességek széles skáláját biztosítja a jobb kezelhetőség érdekében. Amikor alkalmazásátjárót hoz létre, a rendszer egy végpontot (nyilvános virtuális IP-cím vagy belső ILB IP) rendel hozzá, amely a bejövő hálózati forgalom kezelésére szolgál. A VIP vagy ILB IP-által biztosított Azure terheléselosztó hello (TCP/UDP) átviteli szinten működik, és rendelkezik az összes bejövő hálózati forgalom alatt terhelés kiegyensúlyozott toohello Alkalmazásátjáró worker-példány. Alkalmazásátjáró hello útvonalak hello HTTP/HTTPS-forgalmat a konfigurációján alapul, hogy a virtuális gép, majd a felhőalapú szolgáltatás, a belső vagy külső IP-címet.

Alkalmazásátjáró terheléselosztás egy Azure által kezelt szolgáltatás lehetővé teszi, hogy hello kiépítés 7 réteg terheléselosztó mögött hello Azure szoftveres terheléselosztóként üzemeljen. A TRAFFIC manager lehet használt toocomplete hello forgatókönyv látható a következő kép, ahol a Traffic Manager biztosít átirányítása és a forgalom rendelkezésre állását toomultiple alkalmazás átjáró-erőforrásokat különböző régiókban, míg szolgál az Alkalmazásátjáró hello kereszt-régió réteg 7 terheléselosztás. Ebben a forgatókönyvben például helyen találhatók: [Using terheléselosztási a hello Azure cloud services](../traffic-manager/traffic-manager-load-balancing-azure.md)

![traffic manager- és application gateway-forgatókönyv](./media/application-gateway-introduction/tm-lb-ag-scenario.png)

[!INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="gateway-sizes-and-instances"></a>Átjáróméretek és -példányok

Az Application Gateway jelenleg három méretben érhető el: **Kicsi**, **Közepes** és **Nagy**. A Kicsi méret ideális fejlesztési és tesztelési célokra.

Másolatot too50 alkalmazásátjárót előfizetésenként hozhat létre, és minden Alkalmazásátjáró legfeljebb too10 példányokat tartalmazhat. Egy alkalmazásátjáró 20 HTTP-figyelőből állhat. Az Application Gateway korlátainak teljes listáját lásd: [Az Application Gateway szolgáltatási korlátozásai](../azure-subscription-service-limits.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#application-gateway-limits).

a következő táblázat hello SSL kiürítése engedélyezve egy átlagos teljesítmény adatátviteli sebességét minden alkalmazás átjárópéldány tartalmazza:

| A háttérkiszolgáló lapválasza | Kicsi | Közepes | Nagy |
| --- | --- | --- | --- |
| 6K |7,5 Mbps |13 Mbps |50 Mbps |
| 100K |35 Mbps |100 Mbps |200 Mbps |

> [!NOTE]
> Ezek az értékek az alkalmazásátjáró hozzávetőleges átviteli sebességét jelzik. különböző környezet részletes adatait, például átlagos méretet, a háttér-példányok és feldolgozási idő tooserve oldal helyét függ hello tényleges átviteli sebesség. A pontos teljesítményszámokhoz saját teszteket kell futtatnia. Ezek az értékek csupán útmutatóul szolgálnak a kapacitástervezéshez.

## <a name="health-monitoring"></a>Állapotfigyelés

Az Azure Application Gateway automatikusan figyeli a hello háttér-példányok hello állapotát, alapszintű vagy egyéni állapotteljesítmény keresztül. Állapotteljesítmény segítségével biztosítja, hogy csak a megfelelő gazdagépek tootraffic válaszol-e. További információt az [Application Gateway-állapotfigyelés – áttekintés](application-gateway-probe-overview.md) című témakörben talál.

## <a name="configuring-and-managing"></a>Beállítás és felügyelet

Az alkalmazásátjáró végpontjához nyilvános és privát IP-címet is hozzárendelhet, vagy akár mindkettőt, ha a beállítások engedik. Az Application Gateway beállítása egy virtuális hálózaton történik, egy saját alhálózaton. hello alhálózat létre vagy használt nem tartalmazhat más típusú erőforrásokat, és hello csak azokat az erőforrásokat, amelyek futhatnak az hello alhálózati más alkalmazásátjárót. a háttér-erőforrások, hello háttérkiszolgálók egy másik alhálózat a hello lehetnek benne toosecure hello Alkalmazásátjáró megegyező virtuális hálózatban. Ez az alhálózat hello háttér alkalmazások esetén nincs szükség. Mindaddig, amíg hello Alkalmazásátjáró el lehet érni hello IP-cím, az Alkalmazásátjáró képes tooprovide LÉPETT képességek hello háttérkiszolgálókhoz. 

Alkalmazásátjárókat REST API-k, PowerShell-parancsmagok, az Azure parancssori felülete vagy az [Azure Portal](https://portal.azure.com/) segítségével hozhat lére. Az Alkalmazásátjáró további kérdésekre, keresse fel [alkalmazás átjáró – gyakori kérdések](application-gateway-faq.md) tooview listáját általános gyakran ismételt kérdések.

## <a name="pricing"></a>Díjszabás

A díjszabás az átjárópéldányok óránkénti díján és az adatfeldolgozási díjon alapul. Óránként átjáró árképzési hello WAF SKU eltér a Standard Termékváltozat díjakat. A díjszabás az [Application Gateway díjszabását](https://azure.microsoft.com/pricing/details/application-gateway/) ismertető webhelyen tekinthető meg. Az adatfeldolgozás díjakat továbbra is hello azonos.

## <a name="faq"></a>GYIK

Az Application Gatewayre vonatkozó gyakori kérdésekért lásd: [Application Gateway – gyakori kérdések](application-gateway-faq.md).

## <a name="next-steps"></a>Következő lépések

Alkalmazásátjáró megismerését követően is [Alkalmazásátjáró létrehozása](application-gateway-create-gateway-portal.md) , illetve [SSL-kiszervezés Alkalmazásátjáró létrehozása](application-gateway-ssl-arm.md) tooload-egyenleg HTTPS-kapcsolatok.

toolearn hogyan toocreate Alkalmazásátjáró URL-alapú tartalom útválasztás használatával, nyissa meg túl[URL-alapú útválasztás használatával Alkalmazásátjáró létrehozása](application-gateway-create-url-route-arm-ps.md) további információt.

azzal kapcsolatban, toolearn más hálózati lehetőségeket az Azure key hello című [Azure hálózati](../networking/networking-overview.md).
