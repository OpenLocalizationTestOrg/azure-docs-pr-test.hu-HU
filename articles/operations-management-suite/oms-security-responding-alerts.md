---
title: "aaaMonitoring és a válaszoló tooSecurity riasztásokat az Operations Management Suite biztonsági és naplózási megoldás |} Microsoft Docs"
description: "Ez a dokumentum segít, toouse hello fenyegetés eszközintelligencia beállítás OMS biztonsági és naplózási toomonitor érhető el, és toosecurity riasztások válaszol."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 7d45a32b-1341-4bb5-a436-1f42a8a2590a
ms.service: operations-management-suite
ms.custom: oms-security
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/13/2017
ms.author: yurid
ms.openlocfilehash: 3d92b6809b7bd934c889afc119e5e34ff2b85f1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-and-responding-toosecurity-alerts-in-operations-management-suite-security-and-audit-solution"></a>Figyelés és válaszol toosecurity riasztásait az Operations Management Suite biztonsági és naplózási megoldás
Ez a dokumentum segítséget nyújt a hello fenyegetés eszközintelligencia beállítással OMS biztonsági és a naplózási toomonitor és toosecurity riasztások válaszol.

## <a name="what-is-oms"></a>Mi az az OMS?
A Microsoft Operations Management Suite (OMS), a Microsoft felhő alapú informatikai felügyeleti megoldás, amely segít a kezelése és védelme a helyszíni és felhőalapú infrastruktúra. További információ az OMS hello a cikk elolvasása [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="threat-intelligence"></a>Fenyegetésészlelési intelligencia
Egy vállalati környezetben, ahol a felhasználók széleskörű hozzáférést toohello hálózattal rendelkezik, és különböző eszközök tooconnect toocorporate adatokat használja rendkívül fontos, hogy az erőforrások aktív figyelését, és gyorsan válaszolni toosecurity incidensek. Ez az adott hello biztonsági életciklus szempontjából fontos, mert bizonyos fenyegetések nem vehet fel számítógépes riasztásokat vagy a gyanús tevékenységeket, amely segítségével azonosítható hagyományos biztonsági technikai vezérlők. 

Hello segítségével **Fenyegetésfelderítési adataival** lehetőség elérhető OMS biztonsági és a naplózási, a rendszergazdák is biztonsági fenyegetések ellen hello környezetben, például azonosíthatja, azonosítja, hogy egy adott számítógép része egy [ botnet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection). Számítógépek egy botnet csomópontjának válhat, a támadók törvénytelenül telepítésekor kártevők, amelyek a számítógép toohello parancs és a vezérlő titokban csatlakozik. Azt is megállapíthatja potenciális fenyegetések, például a föld kommunikációs csatornákon érkező [darknet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection_honeypots_darkents). 

A fenyegetésfelderítési adataival, OMS biztonsági és naplózási rendelés toobuild használni a Microsofton belül különböző forrásokból származó adatok. OMS biztonsági és naplózási fogja használni, az adatok tooidentify potenciális fenyegetések ellen a környezetben.

hello Fenyegetésfelderítési adataival ablaktábla három fő beállítások szerint jön létre:

* Kártékony kimenő forgalmat bonyolító kiszolgálók
* Észlelt fenyegetéseket típusok
* Fenyegetésfelderítési térkép

> [!NOTE]
> Ezek a beállítások áttekintéséhez olvassa el a [Ismerkedés az Operations Management Suite biztonsági és naplózási megoldás](oms-security-getting-started.md).
> 
> 

### <a name="responding-toosecurity-alerts"></a>Válaszol toosecurity riasztások
A hello lépések egyikét a [biztonsági incidensekre adott reakciók](https://technet.microsoft.com/library/cc512623.aspx) folyamat tooidentify hello súlyossági hello biztonsági sérülés rendszer(ek). Ebben a fázisban végre kell hajtania a következő feladatok hello:

* Hello támadás természetét hello meghatározása
* Hello támadási pont származási meghatározása
* Hello támadás hello célját határozza meg. A szervezet tooacquire konkrét részleteket irányuló hello támadás történt, vagy lett véletlenszerű?
* Hello, hogy melyik biztonsága sérült
* Hozzáfért lett, és határozza meg azokat a fájlokat hello érzékenységének hello fájlok azonosítása

Kihasználhatja **Fenyegetésfelderítési adataival** OMS biztonsági és hitelesítési megoldás toohelp ezeket a feladatokat az adatokat. Kövesse az alábbi tooaccess hello lépéseket ez **Fenyegetésfelderítési adataival** beállítások:

1. A hello **a Microsoft Operations Management Suite** fő irányítópultján kattintson **biztonsági és naplózási** csempére.
   
    ![Biztonsági és naplózása](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)
2. A hello **biztonsági és naplózási** irányítópult, látni fogja a hello **Fenyegetésfelderítési adataival** hello jobb beállításai alább látható módon:
   
    ![Fenyegetés intel](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig2-ga.png)

Ezek három csempék fogja diktálni hello aktuális fenyegetések áttekintését. A hello **Server kártékony kimenő forgalmat bonyolító** fogja tudni tooidentify (belül vagy kívül a hálózat) figyelt számítógépek esetén, amely küldő rosszindulatú forgalom-toohello Internet. 

Hello **észlelt fenyegetés típusok** csempe, amelyek az aktuális "a"helyettesítő hello szövegrészt hello fenyegetések összegzését jeleníti meg, ha erre a csempére kattintva akkor jelenik meg ezek a fenyegetések további információt az alábbi megjelenítése as:

![Felderített fenyegetéstípusok](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig3.png)

További információ az egyes fenyegetésekre azt nyerhet ki. az alábbi példa hello Botnet további részleteit jeleníti meg:

![További információt a fenyegetést](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig4.png)

Hello elejére ebben a szakaszban ismertetett ezt az információt nagyon hasznosak lehetnek az incidensekre adott reakciók esetben során. Azt is fontos lehet a törvényszéki nyomozások során toofind hello forrás hello támadások, mely a rendszer biztonsági szempontból sérült, és idővonal hello kell. Ebben a jelentésben könnyen azonosíthatja, néhány főbb részleteinek hello támadás, például: hello hello támadás forrására, helyi IP-cím biztonsági szempontból sérült hello és hello hello kapcsolat aktuális munkamenet-állapot. 

Hello **fenyegetés eszközintelligencia térkép** segít tooidentify hello aktuális körül hello földgömb rendelkező helyek rosszindulatú forgalmat. Narancssárga (bejövő) és a piros (kimenő) nyilak ezen a térképen azonosító hello forgalom irányát, ha valamelyik e nyíl gombra kattint, meg fog jelenni hello típusú trójai és hello Forgalomirány alább látható módon:

![fenyegetésfelderítési leképezés](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig5.png)

> [!NOTE]
> Megjelenik egy bemutató toouse ezt a képességet az incidensekre adott reakciók során folyamat hello bemutató megfigyelés útján [datacenter biztonsági fenyegetések mérséklésére az Operations Management Suite használata interaktív vizsgálat](https://myignite.microsoft.com/videos/5000) a Microsoft Ignite-i.
> 

### <a name="responding-toodistinct-malicious-ip-accessed"></a>Válaszol toodistinct kártékony IP érhető el
Bizonyos esetekben észlelhet egy potenciálisan kártékony IP-címet, amelyhez az egyik felügyelt számítógépről fértek hozzá:

![fenyegetésfelderítési leképezés](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig6.png)

Ezt a figyelmeztetést, míg mások belül hello ugyanilyen kategóriájú, OMS biztonsági keresztül, ami által előállított [Microsoft Fenyegetésfelderítési adataival](https://youtu.be/O4WtxgUrDc8). hello Fenyegetésfelderítési adataival adatok van a Microsoft által gyűjtött, valamint származó vezető fenyegetés eszközintelligencia szolgáltatók. Az adatok gyakran frissül, és leírtakon toofast áthelyezése fenyegetéseket. Tooits jellege miatt azt kombinálni kell egyéb információforrásokat biztonsági közben [kivizsgálása](https://blogs.technet.microsoft.com/msoms/2016/12/08/investigating-suspicious-activity-in-a-hybrid-cloud-with-oms-security/) egy biztonsági riasztást. 

## <a name="customize-alerts-received-via-e-mail"></a>Figyelmeztetés érkezett e-mail testreszabása

Testre szabhatja, mely a szervezet felhasználói rendszer értesíti, ha a biztonsági figyelmeztetést OMS biztonsági. Ez a beállítás érhető el – áttekintés csoportban / beállítások hello OMS irányítópult:

![E-mail cím](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig7.png)

## <a name="see-also"></a>Lásd még:
Ebben a dokumentumban, megtudta, hogyan toouse hello **Fenyegetésfelderítési adataival** OMS biztonsági és hitelesítési megoldás toorespond toosecurity riasztások beállítást. További információ az OMS biztonsági toolearn tekintse meg a következő cikkek hello:

* [Az Operations Management Suite (OMS) áttekintése](operations-management-suite-overview.md)
* [Ismerkedés az Operations Management Suite biztonsági és naplózási megoldás](oms-security-getting-started.md)
* [Az erőforrások figyelése az Operations Management Suite biztonsági és auditálási megoldásban](oms-security-monitoring-resources.md)

