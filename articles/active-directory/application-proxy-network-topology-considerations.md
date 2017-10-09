---
title: "Azure Active Directory Alkalmazásproxyjával használatakor aaaNetwork topológiai szempontok |} Microsoft Docs"
description: "Hálózati topológia szempontjai vonatkozik, az Azure AD-alkalmazásproxy használata esetén."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 9b8cdd2196efeb92a74e44dde6511f7d3091a968
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="network-topology-considerations-when-using-azure-active-directory-application-proxy"></a>Hálózati topológia való használatának szempontjai Azure Active Directory Alkalmazásproxyjával

Ez a cikk ismerteti a hálózati topológia szempontjai közzétételéhez, és távolról fér hozzá az alkalmazások Azure Active Directory (Azure AD) alkalmazásproxy használata esetén.

## <a name="traffic-flow"></a>Forgalom

Ha egy alkalmazás az Azure AD-alkalmazásproxy használatával van közzétéve, hello felhasználók toohello alkalmazásokból a forgalom három kapcsolatokon keresztül:

1. hello felhasználó kapcsolódásakor toohello Azure AD alkalmazásproxy szolgáltatás nyilvános végpontot az Azure-on
2. hello alkalmazásproxy-szolgáltatás csatlakozik toohello alkalmazásproxy-összekötő
3. hello alkalmazásproxy-összekötő kapcsolódik toohello célalkalmazás

![Felhasználói tootarget alkalmazásból adatforgalmat bemutató ábra](./media/application-proxy-network-topologies/application-proxy-three-hops.png)

## <a name="tenant-location-and-application-proxy-service"></a>Bérlő helyét és az alkalmazásproxy-szolgáltatás

Amikor regisztrál az Azure AD-bérlő, a bérlő hello területet megadott hello ország határozza meg. Alkalmazásproxy engedélyezése, ha a hello alkalmazásproxy szolgáltatáspéldány, a bérlő a kiválasztott vagy hello létre azonos régióban legyen, mint az Azure AD-bérlő vagy hello legközelebbi régiót tooit.

Például ha az Azure AD-bérlő régió hello Európai Unió, az alkalmazásproxy-összekötő használata szolgáltatáspéldány az hello EU az Azure adatközpontjaiban. Amikor a felhasználók hozzáférést a közzétett alkalmazásokhoz, forgalmukat végighalad hello alkalmazásproxy szolgáltatáspéldány ezen a helyen.

## <a name="considerations-for-reducing-latency"></a>Csökkenti a késéseket szempontjai

Minden proxymegoldások késés bevezetéséhez a hálózati kapcsolatot. Függetlenül attól, milyen proxy- vagy VPN-megoldás a távelérési megoldás választja azt mindig tartalmazza a hello kapcsolat tooinside engedélyezése a vállalati hálózati kiszolgálók egy csoportja.

A szervezetek server végpontok rendszerint tartalmazza a szegélyhálózaton. Az Azure AD-alkalmazásproxy azonban forgalmat hello proxy szolgáltatás hello felhőben közben hello összekötők a vállalati hálózaton találhatók. Nincs szegélyhálózat hálózatra azért szükség.

hello következő szakaszok további javaslatok megosztásához toohelp késése még tovább csökkenti tartalmaznak. 

### <a name="connector-placement"></a>Összekötő elhelyezése

Alkalmazásproxy példányok hello helyét, a bérlő helye alapján választja ki. Azonban elérhetővé toodecide ahol tooinstall hello összekötő, felkínálva hello power toodefine hello késés jellemzőit a hálózati forgalom.

Hello alkalmazásproxy-szolgáltatás beállításakor kérje meg a következő kérdések hello:

* Hol található a hello alkalmazást?
* Hol találhatók a legtöbb olyan felhasználók számára, akik található hello alkalmazást?
* Hol található a hello alkalmazásproxy példányt?
* Már rendelkezik egy dedikált hálózati kapcsolat beállításához, például az Azure ExpressRoute vagy egy hasonló VPN tooAzure adatközpontok?

hello összekötő toocommunicate Azure és az alkalmazások (2. és 3 hello forgalmának folyamatábrája lépéseket) egyaránt van, ezért hello hello összekötő érinti hello késés e két kapcsolatok elhelyezését. Hello összekötő hello elhelyezésének értékelésekor tartsa szem előtt tartva hello pontok a következő:

* Ha szeretne toouse Kerberos által korlátozott delegálás (KCD) egyszeri bejelentkezést, hello összekötő egy sor a láthatáron tooa datacenter szüksége lesz. Emellett hello összekötő kiszolgálója kell toobe tartományhoz.  
* Ha kétségei vannak, a hello összekötő szorosabb toohello alkalmazás telepítéséhez.

### <a name="general-approach-toominimize-latency"></a>Általános megközelítés toominimize késés

Minden hálózati kapcsolat optimalizálásával minimalizálhatja hello késés hello végpontok közötti forgalom. Minden kapcsolat segítségével optimalizálható:

* Hello két vége hello Ugrás hello távolságát csökkentése.
* Hello ideális hálózati tootraverse kiválasztása. Például egy helyett hello áthaladó nyilvános Internet gyorsabb lehet, toodedicated hivatkozások miatt.

Ha egy dedikált VPN- vagy ExpressRoute hivatkozást Azure és a vállalati hálózat között, érdemes lehet a toouse, amely.

## <a name="focus-your-optimization-strategy"></a>Összpontosítson az optimalizálás stratégia

Csak kis, hogy elvégezhető-e toocontrol hello kapcsolat a felhasználók és a hello alkalmazásproxy-szolgáltatás között. Felhasználók férhetnek hozzá az alkalmazások egy otthoni hálózatot, egy kávézóban vagy egy másik országból. Ehelyett hello kapcsolatok hello alkalmazásproxy szolgáltatás toohello proxyval összekötők toohello alkalmazásokból is optimalizálhatja. Vegye fontolóra a következő minták környezetében hello.

### <a name="pattern-1-put-hello-connector-close-toohello-application"></a>1. minta: Put hello összekötő Bezárás toohello alkalmazás

Hely hello összekötő Bezárás toohello célalkalmazás hello ügyfél hálózatban. Ez a konfiguráció minimálisra hello topográfia diagramon, 3. lépés, mert bezárás hello összekötő és az alkalmazás. 

Ha az összekötő egy sor a láthatáron toohello tartományvezérlő, akkor ebben a mintában értéke előnyös. A felhasználók a legtöbb ebben a mintában használható, mert a legtöbb esetben jól alkalmazható. Ebben a mintában is kombinálható mintát 2 toooptimize forgalom hello szolgáltatást és hello összekötő között.

### <a name="pattern-2-take-advantage-of-expressroute-with-public-peering"></a>2. szabály: A nyilvános társviszony ExpressRoute előnyeinek kihasználása

Ha ExpressRoute állítsa be a nyilvános társviszony-létesítést, a Proxy és hello connector közötti forgalom használhatja hello gyorsabb ExpressRoute-kapcsolatot. hello összekötő van-e a hálózaton, a Bezárás toohello alkalmazást.

### <a name="pattern-3-take-advantage-of-expressroute-with-private-peering"></a>3. szabály: A magánhálózati társviszony-létesítés ExpressRoute előnyeinek kihasználása

Ha egy dedikált VPN vagy ExpressRoute állítsa be a magánhálózati társviszony-létesítés Azure és a vállalati hálózat között, akkor is van lehetősége. Ebben a konfigurációban hello virtuális hálózatot az Azure-ban általában a vállalati hálózat hello bővítményeként tekinteni. Ezért hello connector telepítése az hello Azure-adatközpontban, és továbbra is megfelel a hello alacsony késési követelményekkel hello összekötő-alkalmazás kapcsolat.

Késés nem sérül, mert a forgalom áramlik dedikált kapcsolaton keresztül. Továbbfejlesztett alkalmazásproxy-szolgáltatás-összekötő várakozási ideje is mert hello összekötő telepítve van egy Azure-adatközpontban Bezárás tooyour az Azure AD bérlő helye az beszerzése.

![Összekötő telepítve vannak az Azure-adatközpontban bemutató ábra](./media/application-proxy-network-topologies/application-proxy-expressroute-private.png)

### <a name="other-approaches"></a>Más megoldások

Bár ez a cikk hello fókusz összekötő elhelyezése, hello alkalmazás tooget jobb késés jellemzőit elhelyezésére hello is módosíthatja.

Egyre több szervezet áthelyezi hálózataikat üzemeltetett környezetekben. Ez lehetővé teszi, hogy azok tooplace alkalmazások egy része is a vállalati hálózathoz, és továbbra is hello tartományon belül kell üzemeltetési környezetben. Ebben az esetben a hello a fenti szakaszokban ismertetett hello minták lehet alkalmazott toohello új alkalmazás helyét. Ha ezt a beállítást fontolóra vette, lásd: [Azure AD tartományi szolgáltatások](../active-directory-domain-services/active-directory-ds-overview.md).

Emellett vegye figyelembe az összekötők használatával rendszerezéséhez [összekötő csoportok](active-directory-application-proxy-connectors.md) tootarget alkalmazásokat, amelyek különböző helyekre és -hálózatok találhatók. 

## <a name="common-use-cases"></a>Gyakori használati helyzetek

Ez a szakasz azt ismerteti keresztül néhány olyan gyakori forgatókönyvet. Tegyük fel, hogy hello Azure AD-bérlő (és ezért proxy szolgáltatás végpont) hello az Amerikai Egyesült Államok található. hello ezek ismertetett szempontok használati esetekben tooother régiók körül hello földgömb is érvényesek.

A forgatókönyvekben azt hívjuk kapcsolatonként egy "ugrást" és egyszerűbb vitafórum number őket:

- **1 ugrások**: felhasználói toohello alkalmazásproxy-szolgáltatás
- **2 ugrások**: alkalmazásproxy szolgáltatás toohello alkalmazásproxy-összekötő
- **3 ugrások**: alkalmazásproxy-összekötő toohello célalkalmazás 

### <a name="use-case-1"></a>Használati eset 1

**Forgatókönyv:** hello app egy szervezet hálózatához hello a SZÁMUNKRA, hello felhasználóival ugyanabban a régióban. Nem expressroute-on vagy VPN hello Azure-adatközpontban és a hello vállalati hálózat között van.

**Javaslat:** hello előző szakaszban ismertetett 1, hajtsa végre mintának. Továbbfejlesztett késésének érdemes lehet ExpressRoute, ha szükséges.

Ez az egyszerű mintát. Ugrás 3 optimalizálhatja úgy, hogy hello összekötő hello app közelében. Ez a is természetes választani, mert hello összekötő általában telepítve van a sor a láthatáron toohello alkalmazást, és toohello datacenter tooperform Kerberos által korlátozott Delegálás műveletekkel.

![Hogy felhasználók, a proxy, a összekötő, és az alkalmazás legyenek minden hello VELÜNK bemutató ábra](./media/application-proxy-network-topologies/application-proxy-pattern1.png)

### <a name="use-case-2"></a>Használati eset 2

**Forgatókönyv:** hello app egy szervezet hálózatához hello a SZÁMUNKRA, globálisan felülbírálásokkal felhasználókkal. Nem expressroute-on vagy VPN hello Azure-adatközpontban és a hello vállalati hálózat között van.

**Javaslat:** hello előző szakaszban ismertetett 1, hajtsa végre mintának. 

Ebben az esetben hello megszokott mintát követi toooptimize Ugrás 3, hello összekötő közelében hello app helyétől. Ugrás 3 nincs általában költséges, ha az összes belül hello azonos régióban. Azonban Ugrás 1 lehet drágább, attól függően, hogy hol hello felhasználói, mert különböző hello world hello USA hello alkalmazásproxy-példányra kell elérni. Érdemes megjegyezni, hogy bármely proxy megoldás globálisan alatt felülbírálásokkal felhasználók vonatkozó hasonló jellemzőkkel rendelkezik-e.

![Felhasználók globálisan vesznek, de hello proxy, összekötő, és az alkalmazás hello VELÜNK bemutató ábra](./media/application-proxy-network-topologies/application-proxy-pattern2.png)

### <a name="use-case-3"></a>Használati eset 3

**Forgatókönyv:** hello app egy szervezet hálózatához hello a SZÁMUNKRA. A nyilvános társviszony ExpressRoute Azure és a hello vállalati hálózat között van.

**Javaslat:** hajtsa végre az 1. és 2 hello előző szakaszban ismertetett minták.

Először hello összekötő legközelebb lehetséges toohello alkalmazásként. Ezt követően hello rendszer automatikusan használja az ExpressRoute Ugrás 2. 

Ha hello ExpressRoute kapcsolat nyilvános társviszony használja, hello hello proxy és hello connector közötti forgalmat adott hivatkozáson keresztül. Ugrás 2 optimalizálta késés.

![Hello proxy és összekötő között ExpressRoute bemutató ábra](./media/application-proxy-network-topologies/application-proxy-pattern3.png)

### <a name="use-case-4"></a>Használati eset 4

**Forgatókönyv:** hello app egy szervezet hálózatához hello a SZÁMUNKRA. A magánhálózati társviszony-létesítés ExpressRoute Azure és a hello vállalati hálózat között van.

**Javaslat:** kövesse mintát 3 hello előző szakaszban ismertetett.

Tegyen hello összekötő hello Azure-adatközpontban, amely csatlakoztatott toohello vállalati hálózaton keresztül ExpressRoute magánhálózati társviszony-létesítés. 

hello összekötő helyezhető hello Azure-adatközpontban. Mivel hello összekötő továbbra is csak egy sor a láthatáron toohello alkalmazás- és hello datacenter hello magánhálózaton keresztül, hop 3 optimalizált marad. Ezenkívül további optimalizálták Ugrás 2.

![Ábra: hello összekötő egy Azure-adatközpontban és ExpressRoute hello összekötő és az alkalmazás között](./media/application-proxy-network-topologies/application-proxy-pattern4.png)

### <a name="use-case-5"></a>Használati eset 5

**Forgatókönyv:** hello app egy szervezet hálózatához hello Európa, a hello alkalmazásproxy példány és a legtöbb felhasználói hello a SZÁMUNKRA.

**Javaslat:** hely hello összekötő hello app közelében. Mivel az USA felhasználók elérik-e a hello toobe történik alkalmazásproxy példány ugyanabban a régióban, hop 1 nincs túl drága. Ugrás 3 megfelelően lett optimalizálva. Érdemes lehet ExpressRoute toooptimize Ugrás 2. 

![USA, hello hello összekötő és hello EU alkalmazás a felhasználók és a proxy bemutató ábra](./media/application-proxy-network-topologies/application-proxy-pattern5b.png)

Is megfontolhatja egy variant ebben a helyzetben. Ha a legtöbb hello szervezethez tartozó felhasználók hello VELÜNK, majd valószínűleg, hogy a hálózat bővíti toohello VELÜNK is. Hello összekötő elhelyezése hello USA, és a dedikált hello belső vállalati hálózaton sor toohello alkalmazás hello EU. Ez úgy ugrások 2 és 3 vannak optimalizálva.

![Az Amerikai Egyesült Államok, Európa hello alkalmazás hello felhasználók, proxy és összekötő bemutató ábra](./media/application-proxy-network-topologies/application-proxy-pattern5c.png)

## <a name="next-steps"></a>Következő lépések

- [Alkalmazásproxy engedélyezése](active-directory-application-proxy-enable.md)
- [Egyszeri bejelentkezés engedélyezése](active-directory-application-proxy-sso-using-kcd.md)
- [Feltételes hozzáférés engedélyezése](active-directory-application-proxy-conditional-access.md)
- [Az alkalmazásproxy problémák elhárítása](active-directory-application-proxy-troubleshoot.md)
