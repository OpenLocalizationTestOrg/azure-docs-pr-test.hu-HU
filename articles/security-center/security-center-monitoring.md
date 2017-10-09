---
title: "aaaSecurity figyelése az Azure Security Centerben |} Microsoft Docs"
description: "Ez a cikk segít tooget, figyelési képességek az Azure Security Center használatába."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 3bd5b122-1695-495f-ad9a-7c2a4cd1c808
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2017
ms.author: yurid
ms.openlocfilehash: 43c2a8864d5fe27ba44b0d7bc979db970305ec17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="security-health-monitoring-in-azure-security-center"></a>Biztonsági állapotfigyelés az Azure Security Centerben
Ez a cikk segít az Azure Security Center toomonitor szabályzatoknak való megfelelőségét a figyelési képességek hello használata.

## <a name="what-is-security-health-monitoring"></a>Mi az a biztonsági állapotfigyelés?
Gyakran szerintünk "figyelés" figyeli, és úgy, hogy aztán reagálhassunk toohello helyzet egy esemény toooccur vár. A biztonságfigyelés toohaving proaktív stratégiát jelent, amely a vállalati szabványoknak vagy ajánlott eljárásoknak eleget nem tevő erőforrások tooidentify rendszerek naplózás.

## <a name="monitoring-security-health"></a>A biztonsági állapot figyelése
Miután engedélyezte a [biztonsági házirendek](security-center-policies.md) az előfizetéshez tartozó erőforrásokra, a Security Center elemzi az erőforrások tooidentify potenciális biztonsági réseket hello biztonságát. A hálózati konfigurációval kapcsolatos információk azonnal elérhetők. Egy óráig is eltarthat, vagy további információ a virtuális gép konfigurációját, például biztonsági frissítése, állapot és az operációs rendszer konfigurálása, toobecome érhető el. Az erőforrások és probléma merül fel hello biztonsági állapotát megtekintheti a hello **megelőzési** szakasz. Ezek a problémák listáját is megtekintheti a hello **javaslatok** csempére.

További információ tooapply ajánlásokat, olvassa el [végrehajtási biztonsági javaslatok az Azure Security Centerben](security-center-recommendations.md).

Hello alatt **megelőzési** szakaszban figyelheti hello az erőforrások biztonsági állapotát. A következő példa hello, megtekintheti az egyes erőforrások csempén (számítási, hálózati, tárolási & adat és alkalmazás) rendelkező hello azonosított problémák teljes száma.

![A Resources security health (Erőforrások biztonsági állapota) csempe](./media/security-center-monitoring/security-center-monitoring-fig1-newUI-2017.png)


### <a name="monitor-compute"></a>Számítási tevékenység figyelése
Amikor rákattint **számítási** csempére, hello **számítási** panelt megnyitó bemutatja a három lappal:

- **Áttekintés**: megfigyelés és a virtuális gépre vonatkozó javaslatok.
- **Virtuális gépek**: az összes virtuális gép és azok aktuális biztonsági állapotának listája.
- **Felhőszolgáltatások**: a Security Center által figyelt összes webes és feldolgozói szerepkör listája.

![Hiányzó rendszerfrissítések virtuális gépenként](./media/security-center-monitoring/security-center-monitoring-fig1-new002-2017.png)

Az egyes lapokon rendelkezhet több szakaszt, és minden egyes részben válassza ki az egyéni beállítás toosee hello kapcsolatos további részletekért javasolt lépéseket tooaddress, hogy az adott problémát. 

#### <a name="monitoring-recommendations"></a>Figyelési javaslatok
Ez a szakasz bemutatja a virtuális gépekhez, amelyek az adatgyűjtés és a jelenlegi állapot volt inicializálva hello teljes száma. Miután az összes virtuális gép rendelkezik az adatgyűjtést, fogják készen tooreceive Security Center biztonsági szabályzatainak. Ha ez a bejegyzés gombra kattint, hello **Virtuálisgép-ügynök hiányzik vagy nem válaszol** panel nyílik meg. 

![Hiányzó rendszerfrissítések virtuális gépenként](./media/security-center-monitoring/security-center-monitoring-fig1-new003-2017.png)


#### <a name="virtual-machine-recommendations"></a>Virtual machine recommendations (A virtuális gépre vonatkozó javaslatok)
Ebben a szakaszban az Azure Security Center által megfigyelt [virtuális gépekre vonatkozó javaslatokat](security-center-virtual-machine-recommendations.md) olvashat. hello első oszlopban található hello javaslat. hello második oszlopban látható a virtuális gépeket, amelyekre érvényes által érintett hello teljes száma. hello harmadik oszlopban látható a hello hello probléma súlyossága, a következő képernyőkép hello ismertetett módon.

![Virtual machine recommendations (A virtuális gépre vonatkozó javaslatok)](./media/security-center-monitoring/security-center-monitoring-fig1-new004-2017.png)

> [!NOTE]
> Csak a legalább egy nyilvános végpontot virtuális gépek megjelennek-e hello **hálózati állapotfigyelő** hello paneljén **hálózati topológia** listája.
>
>

Az egyes javaslatokra kattintva különböző műveleteket végezhet el. Ha például **hiányzó rendszerfrissítések**, hello **hiányzó rendszerfrissítések** panel nyílik meg. Felsorolja, hogy hiányoznak a javítások és hello hello hiányzó frissítés súlyosságát, ahogy az alábbi hello hello virtuális gépek képernyőkép.

![Virtuális gépek hiányzó rendszerfrissítései](./media/security-center-monitoring/security-center-monitoring-fig5-ga.png)

Hello **hiányzó rendszerfrissítések** panelen a következő információ hello táblázatát jeleníti meg:

* **VIRTUÁLIS gép**: hello hello virtuális gép nevét, amelyről frissítések hiányoznak.
* **RENDSZERFRISSÍTÉSEK**: hello hiányzó rendszerfrissítések száma.
* **LEGUTÓBBI ellenőrzés időpontja**: hello ideje, hogy a Security Center legutóbb ellenőrizte hello virtuális gép a frissítéseket.
* **ÁLLAPOT**: hello hello javaslat aktuális állapota:
  * **Nyissa meg**: hello javaslat még nem foglalkoztak még.
  * **A folyamatban lévő**: hello javaslat jelenleg folyamatban van a alkalmazott toothose erőforrásokat, és nincs szükség felhasználói műveletek is.
  * **Megoldott**: hello javaslat már befejeződött. (Ha hello problémát sikerült megoldani, hello bejegyzés szürkén jelenik meg).
* **SÚLYOSSÁGI**: ismerteti, hogy adott javaslat súlyosságát hello:
  * **High** (Magas): biztonsági rés található egy fontos erőforrásnál (alkalmazás, virtuális gép, hálózati biztonsági csoport) és beavatkozást igényel.
  * **Közepes**: nem kritikus vagy kiegészítő lépések elvégzése szükséges toocomplete egy folyamat vagy egy biztonsági rés megszüntetéséhez.
  * **Low** (Alacsony): a biztonsági rést be kell tömni, de a probléma nem igényel azonnali beavatkozást. (Alapértelmezés szerint alacsony súlyosságú javaslatok nem láthatók, de ha azt szeretné, hogy tooview alacsony súlyosságú javaslatok végezhet azokat.)

tooview hello ajánlás részleteit, kattintson a hello hello virtuális gép nevét. Új a virtuális gép ekkor megnyílik egy panel hello frissítések listáját a következő képernyőkép hello ismertetett módon.

![Adott virtuális gép hiányzó rendszerfrissítései](./media/security-center-monitoring/security-center-monitoring-fig6-ga.png)

> [!NOTE]
> hello biztonsági javaslatok itt még hello ugyanaz, mint a hello **javaslatok** panelen. Lásd: hello [végrehajtási biztonsági javaslatok az Azure Security Centerben](security-center-recommendations.md) további információt a cikk tooresolve javaslatok. Ez a tulajdonság vonatkozik, nem csupán a virtuális gépek is hello elérhető erőforrások **Resource Health** csempére.
>
>

#### <a name="virtual-machines-section"></a>A Virtual machines (Virtuális gépek) szakasz
hello virtuális gépek szakasz áttekintést nyújt az összes virtuális gépek és javaslatokat. Minden oszlop felel javasolt, ahogy az alábbi képernyőfelvétel a hello:

![Az összes virtuális gép és javaslat áttekintése](./media/security-center-monitoring/security-center-monitoring-fig1-new005-2017.png)

Minden javaslat megkönnyíti alatt megjelenő hello ikonok meg tooquickly azonosítása hello virtuális gépek kezeléséről és hello típusú javaslat érkezett.

Hello előző példában egy virtuális gépnek az endpoint protection vonatkozó kritikus súlyosságú javaslat. tooget a olvashat hello virtuális gépet, kattintson rá. Egy új panel, amely megnyitja ezt a virtuális gépet jelöl, ahogy az alábbi képernyőfelvétel a hello.

![Virtuális gépek biztonsági információi](./media/security-center-monitoring/security-center-monitoring-fig8-ga.png)

Ezen a panelen hello biztonsági részletek hello virtuális gép van. Ez a panel alsó részén hello láthatja, hello javasolt művelet és az egyes problémák súlyosságát hello.

#### <a name="cloud-services-section"></a>Cloud Services szakasz
A felhőszolgáltatások ajánlást létre elavult látható módon a következő képernyőkép hello hello operációs rendszer verziója esetén:

![Felhőszolgáltatások állapotinformációi](./media/security-center-monitoring/security-center-monitoring-fig1-new006-2017.png)

Forgatókönyv esetében, amelyhez rendelkezik (ami nem hello elő hello előző példához) javaslat kell toofollow hello lépéseit hello ajánlás tooupdate hello operációs rendszer verziója. Egy frissítést nem érhető el, hogy egy riasztást (piros vagy narancssárga - függ hello probléma súlyossága hello). Ha ez a riasztás hello WebRole1 (Windows Server fut, a webes alkalmazás automatikusan telepített tooIIS) vagy (a Windows Server fut, a webes alkalmazás automatikusan telepített tooIIS) WorkerRole1 egy sor kattint, egy új panel nyílik meg a további részletekért a javaslat látható módon a következő képernyőkép hello:

![Felhőszolgáltatás adatai](./media/security-center-monitoring/security-center-monitoring-fig8-new3.png)

több előíró ismertetése, ez a javaslat toosee kattintson **frissítés operációsrendszer-verzió** hello alatt **leírás** oszlop. Hello **frissítés operációsrendszer-verzió (előzetes verzió)** további részletek panel nyílik meg.

![Felhőszolgáltatásokkal kapcsolatos javaslatok](./media/security-center-monitoring/security-center-monitoring-fig8-new4.png)  

### <a name="monitor-virtual-networks"></a>Virtuális hálózatok figyelése
Amikor rákattint **hálózati** csempe, hello **hálózati** panel nyílik meg további részleteket, ahogy az alábbi képernyőfelvétel a hello:

![Hálózat panel](./media/security-center-monitoring/security-center-monitoring-fig9-new3.png)

#### <a name="networking-recommendations"></a>Hálózatokra vonatkozó javaslatok
Például a virtuálisgép-erőforrás állapotinformációkat hello, ezen a panelen hello panelről hello top és a megfigyelt hálózatok listája részén a problémák összefoglaló listája hello alsó biztosít.

hálózati állapotot részletező hello kínál, és tartalmazza a potenciális biztonsági problémákat [javaslatok](security-center-network-recommendations.md). Problémát jelenthetnek például a következők:

* Nincs telepítve újgenerációs tűzfal (NGFW)
* Az alhálózatokon nincsenek bekapcsolva a hálózati biztonsági csoportok
* Az virtuális gépeken nincsenek bekapcsolva a hálózati biztonsági csoportok
* Külső hozzáférés korlátozása nyilvános külső végponton keresztül
* Megfelelő állapotú internet felé néző végpontok

Kattintva azt ajánljuk, ahogy az alábbi példa hello tartalmazó hello javaslat részletes adatainak megnyitása egy új panel.

![Hello hálózati panelén ajánlás részleteit](./media/security-center-monitoring/security-center-monitoring-fig9-ga.png)

Ebben a példában hello **hiányzó hálózati biztonsági csoportok beállítása az alhálózatok** panel rendelkezik alhálózatok listájának, és a virtuális gépek, hiányzó hálózati biztonsági csoport védelmét. Ha hello alhálózati toowhich tooapply hello hálózati biztonsági csoport gombra kattint, megnyílik egy újabb panel.

A hello **válassza a hálózati biztonsági csoport** panelen kiválaszthatja hello legmegfelelőbb hálózati biztonsági csoportot az alhálózathoz hello, vagy létrehozhat egy új hálózati biztonsági csoportot.

#### <a name="internet-facing-endpoints-section"></a>Internet facing endpoints (Internet felé néző végpontok) szakasz
A hello **végpontokat internetes** területen látható hello virtuális gépek, amelyeken jelenleg Internet felé néző végpont és az aktuális állapotával.

![Internet felé néző végpontokkal konfigurált virtuális gépek és a végpontok állapota](./media/security-center-monitoring/security-center-monitoring-fig10-ga.png)

Ez a táblázat rendelkezik hello tartalmazó végpont nevét hello virtuális gép, hello internetre irányuló IP-címet, és hello hello hálózati biztonsági csoport aktuális súlyossági állapotát, és hello NGFW. hello tábla súlyosság szerint rendezve:

* Piros (legfelül): a legmagasabb prioritás, azonnali beavatkozást igényel
* Narancssárga: közepes szintű prioritás, a lehető legrövidebb időn belül beavatkozást igényel
* Zöld (utolsó): megfelelő állapot

#### <a name="networking-topology-section"></a>Networking topology (Hálózati topológia) szakasz
Hello **hálózati topológia** szakaszában ismételten hello erőforrások hierarchikus nézete, ahogy az alábbi képernyőfelvétel a hello:

![Az erőforrások hierarchikus nézete a Hálózati topológia szakaszban](./media/security-center-monitoring/security-center-monitoring-fig121-new4.png)

A táblázat elemei (a virtuális gépek és az alhálózatok) súlyosság szerint rendezve jelennek meg:

* Piros (legfelül): a legmagasabb prioritás, azonnali beavatkozást igényel
* Narancssárga: közepes szintű prioritás, a lehető legrövidebb időn belül beavatkozást igényel
* Zöld (utolsó): megfelelő állapot

A topológia e nézetében hello első szinten vannak [virtuális hálózatok](../virtual-network/virtual-networks-overview.md), [virtuális hálózati átjárók](/vpn-gateway/vpn-gateway-site-to-site-create.md), és [virtuális hálózatok (klasszikus)](/virtual-network/virtual-networks-create-vnet-classic-pportal.md). hello második szinthez tartoznak az alhálózatok, és létezik hello virtuális gépek toothose alhálózatok tartozó hello harmadik szinten. jobb oldali oszlopban hello állapota hello aktuális hello hálózati biztonsági csoport az erőforrásokhoz, ahogy az alábbi példa hello:

![Hálózati topológia szakasz hello hálózati biztonsági csoport állapota](./media/security-center-monitoring/security-center-monitoring-fig12-ga.png)

hello a panel alsó részén van a virtuális gépet, amely hasonló hello javaslatok toowhat korábban leírt. Kattintson a további ajánlás toolearn, vagy hello szükséges biztonsági ellenőrzést vagy a konfiguráció alkalmazásához.

### <a name="monitor-storage--data"></a>A Tárolás és adatok figyelése

Elemre **tárolási & adatok** a hello **megelőzése** című szakaszba hello **erőforrásokat** SQL- és tárolási javaslatok panel nyílik meg. Azt is [javaslatok](security-center-sql-service-recommendations.md) hello általános állapot hello adatbázis. A tárolás titkosításáról további információkat az [Azure-tárfiókok titkosításának engedélyezése az Azure Security Centerben](security-center-enable-encryption-for-storage-account.md) című cikkben találhat.

![Adatforrások](./media/security-center-monitoring/security-center-monitoring-fig13-newUI-2017.png)

A **SQL javaslatok**, is minden javaslat és a get több kapcsolatos információk további művelet tooresolve kapcsolatos problémát. hello alábbi példa bemutatja hello hello bővítése **Database Auditing & Threat detection SQL adatbázisok** javaslat.

![SQL-javaslat részletei](./media/security-center-monitoring/security-center-monitoring-fig14-ga-new.png)

Hello **naplózás engedélyezését & Threat detection SQL-adatbázisok** panel rendelkezik hello a következő információkat:

* Az SQL-adatbázisok listája
* hello kiszolgáló, amelyen találhatók-e
* E ezt a beállítást hello server öröklődött, vagy konkrétan az adatbázisban
* hello aktuális állapota
* hello hello probléma súlyossága

Ez a javaslat kattintva hello adatbázis tooaddress, hello **naplózási & Threat detection** panel nyílik meg, ahogy az a következő képernyő hello.

![Naplózás és fenyegetésészlelés panel](./media/security-center-monitoring/security-center-monitoring-fig15-ga.png)

tooenable naplózás esetén jelölje be **ON** alatt hello **naplózási** lehetőséget.

### <a name="monitor-applications"></a>Alkalmazások figyelése

Ha az Azure számítási található alkalmazások [(az Azure Resource Manager használatával létrehozott) virtuális gépeket](../azure-resource-manager/resource-manager-deployment-model.md) feladatban felfedett webes portokkal (80-as és 443-as TCP-portok), a Security Center képes figyelni azokat tooidentify potenciális biztonsági problémákat és javasolni. Hello elemre **alkalmazások** csempére, hello **alkalmazások** panel nyílik meg számos javaslat található hello **alkalmazás javaslatok** szakasz. Hello alkalmazások állomásonkénti/virtuális IP is látható módon a következő képernyőkép hello mutatja.

![Alkalmazások biztonsági állapota](./media/security-center-monitoring/security-center-monitoring-fig16-ga.png)

Ugyanúgy, mint Ön volt az hello más javaslatokról, kattintson a javaslat toosee hello problémával kapcsolatos további részletekért és hogyan tooremediate. hello hello a következő ábrán látható példája egy alkalmazás egy nem biztonságos webalkalmazásként való kezelése azonosított. Hello alkalmazás, amely nem biztonságosnak kiválasztásakor egy újabb panel társítás hello beállítása a következő:

![Információk a nem biztonságos alkalmazásról](./media/security-center-monitoring/security-center-monitoring-fig17-ga.png)

Ezen a panelen megjelenik az alkalmazáshoz tartozó összes javaslat listája. Amikor rákattint hello **webalkalmazási tűzfal hozzáadása** javaslat, hello **webalkalmazási tűzfal hozzáadása** panel megnyitása, tooinstall lehetőségeket webalkalmazási tűzfal (WAF) a partner a következő képernyőkép hello látható.

![Az Add Web Application Firewall (Webalkalmazás-tűzfal hozzáadása) párbeszédpanel](./media/security-center-monitoring/security-center-monitoring-fig18-ga.png)

## <a name="see-also"></a>Lásd még:
Ebben a cikkben megtanulta, hogyan meg toouse figyelési képességek az Azure Security Centerben. További információ az Azure Security Center toolearn hello következő lásd:

* [Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md): megtudhatja, hogyan tooconfigure biztonsági beállításait az Azure Security Centerben.
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md): megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.
* [Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md): megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.
* [Azure Security Center: GYIK](security-center-faq.md): gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
* [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/): Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.
