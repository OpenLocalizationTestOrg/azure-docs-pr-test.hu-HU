---
title: "Operations Management Suite biztonsági és naplózási megoldás forrásokra aaaMonitoring |} Microsoft Docs"
description: "Ez a dokumentum segít az OMS biztonsági toouse és naplózási képességek toomonitor az erőforrások és biztonsági problémák azonosításához."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: d6752120-821f-4aa7-a049-25bf5a653b95
ms.service: operations-management-suite
ms.custom: oms-security
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: 932b946ae1ffa3b979c02f419702d42d46abf7ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-resources-in-operations-management-suite-security-and-audit-solution"></a>Az Operations Management Suite biztonsági és naplózási megoldás erőforrások figyelése
Ez a dokumentum segít az OMS biztonsági és naplózási képességek toomonitor az erőforrásokat, illetve biztonsági problémák azonosításához.

## <a name="what-is-oms"></a>Mi az az OMS?
A Microsoft Operations Management Suite (OMS), a Microsoft felhő alapú informatikai felügyeleti megoldás, amely segít a kezelése és védelme a helyszíni és felhőalapú infrastruktúra. További információ az OMS hello a cikk elolvasása [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="monitoring-resources"></a>Erőforrások figyelése
Amikor csak lehetséges, érdemes tooprevent biztonsági incidensek hello első helyen le. Lehetetlen tooprevent azonban az összes biztonsági incidens. Egy biztonsági incidens fordulhat elő, ha szüksége lesz a tooensure, hogy a hatása másodpercekre csökken.  Három kritikus javaslatok, amelyek lehetnek használt toominimize hello száma és biztonsági események hatását hello:

* Rendszeresen felmérheti a biztonsági rések a környezetben.
* Rendszeresen ellenőrzi a számítógépes rendszereken, és a hálózati eszközök tooensure, amelyek rendelkeznek az összes hello legújabb javítások vannak telepítve.
* Rendszeresen ellenőrzi a naplók és a naplózás mechanizmusok, beleértve az operációs rendszerek eseménynaplóit, az adott alkalmazásnaplók és a behatolás-észlelés rendszernaplókat.

OMS biztonsági és hitelesítési megoldás lehetővé teszi, hogy informatikai tooactively összes erőforrást, amely segítségével figyelheti a biztonsági események hello gyakorolt hatásának minimalizálása érdekében. OMS biztonsági és naplózási rendelkezik biztonsági tartományok erőforrások figyeléséhez használható. hello biztonsági tartományok gyors hozzáférést biztosít tooa beállítások, a további részletek szerepelnek következő tartományokkal a biztonsági figyelési hello:

* kártevő szoftver értékelése
* Frissítések felmérése
* Identitás és hozzáférés

> [!NOTE]
> Ezek a beállítások áttekintéséhez olvassa el a [Ismerkedés az Operations Management Suite biztonsági és naplózási megoldás](oms-security-getting-started.md).
> 
> 

### <a name="monitoring-system-protection"></a>Figyelési Rendszervédelem
A mélység megközelítés védelmi, minden védelmi réteget fontos hello az objektum általános biztonsági állapotát. Rendelkező számítógépek észlelt fenyegetéseket, és nem megfelelő védelemmel ellátott számítógépek jelennek meg hello kártevő Assessment csempe biztonsági tartományok alatt. Hello kártevő Assessment hello információk segítségével azonosíthatja a terv tooapply védelmi toohello kiszolgálók szükség lenne rá. Ez a beállítás kövesse hello lépések alatt tooaccess:

1. A hello **a Microsoft Operations Management Suite** fő irányítópultján kattintson **biztonsági és naplózási** csempére.
   
    ![Biztonsági és naplózása](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)
2. A hello **biztonsági és naplózási** irányítópultot, kattintson a **kártevőirtó Assessment** alatt **biztonsági tartományok**. Hello **kártevőirtó Assessment** irányítópult az alábbihoz hasonló:

![kártevő szoftver értékelése](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig2-ga.png)

Használhatja a hello **kártevő Assessment** irányítópult tooidentify hello a következő biztonsági problémáit:

* **Aktív fenyegetések**: feltörték, és aktív fenyegetések hello rendszert a számítógépekre.
* **Fenyegetések szervizelt**: szervizelt számítógépek, de hello fenyegetések feltörték.
* **Lejárt aláírás**: számítógépek, amelyeken engedélyezve van, de hello aláírás kártevők elleni védekezés elavult.
* **Nincs valós idejű védelem**: számítógépek, amelyek nem rendelkeznek telepített kártevőirtó.

### <a name="monitoring-updates"></a>frissítések ellenőrzése
Biztonsági szempontból ajánlott hello legfrissebb biztonsági frissítések alkalmazására, és azt a frissítés-kezelési stratégia kell foglalni. A Microsoft Monitoring Agent szolgáltatása (HealthService.exe) frissítés eszközadatokat olvas be a figyelt számítógépekről, és ezután elküldi a frissített információk toohello OMS szolgáltatás hello felhőben feldolgozásra. Microsoft Monitoring Agent szolgáltatást hello van konfigurálva, az automatikus szolgáltatás, és azt mindig futnia kell célszámítógépre hello.

![frissítések ellenőrzése](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig3.png)

Logikai alkalmazott toohello adatok frissítése és hello felhőszolgáltatás hello adatait rögzíti. Hiányzó frissítések találhatók, ha azok látható a hello **frissítések** irányítópult. Hello használható **frissítések** irányítópult toowork hiányoznak frissítések és a terv tooapply fejlesztéséhez szüksége van rájuk toohello kiszolgálók őket. Kövesse az alábbi tooaccess hello hello lépéseket **frissítések** irányítópult:

1. A hello **a Microsoft Operations Management Suite** fő irányítópultján kattintson **biztonsági és naplózási** csempére.
2. A hello **biztonsági és naplózási** irányítópultján kattintson **frissítések értékelését** alatt **biztonsági tartományok**. hello frissítés irányítópult az alábbihoz hasonló:

![Frissítések értékelését](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig4.png)

Az Irányítópulton egy frissítés assessment toounderstand hello aktuális állapotát a számítógép és a cím hello legfontosabb fenyegetések végezheti el. Hello segítségével **kritikus vagy biztonsági frissítések** csempe, a rendszergazdák lesz képes tooaccess részletes információkat hello frissítések hiányoznak alább látható módon:

![keresési eredmény](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig5.png)

Ez a jelentés tartalmazza a fontos információkat használt hello Microsoft KB cikkeket társított hello biztonsági frissítés és hello MS Bulletin hello kapcsolatos további részleteket tartalmaz a rendszer ki van téve a, fenyegetésről tooidentify hello típusa a biztonsági rés.

### <a name="monitoring-identity-and-access"></a>Identitások és hozzáférések figyelése
A felhasználók helyfüggetlen munkavégzéséből és különböző eszközök használatával és a felhőalapú és helyszíni alkalmazásokat, hatalmas mennyiségű elérése rendkívül fontos a hitelesítő adatok védelmének biztosításához. Hitelesítő adatok ellopását támadások megegyeznek, amelyben egy kezdetben támadó hozzáférést tooa rendszeres felhasználói hitelesítő adatok tooaccess hello hálózaton belül a rendszer. Számos esetben a kezdeti támadás csak egy módon tooget hozzáférési toohello hálózat, a hello végső célja toodiscover jogosultsággal rendelkező fiókot. 

A támadók marad hello hálózati használatával ingyenesen elérhető tooling eszköz tooextract hello munkamenetek más bejelentkezett fiókok hitelesítő adatait. Hello rendszer konfigurációjától függően ezeket a hitelesítő adatokat kinyerhetők, jegyek vagy a még akkor is, titkosítatlan szöveges jelszó hello formájában.  

> [!NOTE]
> gépek, amelyek közvetlenül elérhetővé tett toohello Internet jelenik meg, hogy az összes típusú jól ismert felhasználónevek (pl. rendszergazda) használatával próbálja toologin kísérletek sok sikertelen volt. A legtöbb esetben célszerű OK gombra, ha nincsenek használatban hello jól ismert felhasználónevek, és ha hello jelszó nem elég erős.
> 
> 

Ez az e támadóknak lehetséges tooidentify ahhoz, azok veszélyeztetheti a jogosultságú fiók. Kihasználhatja **OMS biztonsági és naplózási megoldás** toomonitor identitások és hozzáférések. Kövesse az alábbi tooaccess hello hello lépéseket **identitások és hozzáférések** irányítópult:

1. A hello **a Microsoft Operations Management Suite** fő irányítópult kattintson a biztonsági, naplózási csempére.
2. A hello **biztonsági és naplózási** irányítópultján kattintson **identitás- és hozzáférés** alatt **biztonsági tartományok**. Hello **identitások és hozzáférések** irányítópult az alábbihoz hasonló:

![identitás és hozzáférés](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig6-ga.png)

A rendszeres felügyeleti stratégia részeként meg kell adnia identitásának ellenőrzésére. Rendszergazda, ha egy adott érvényes felhasználónevet, amelynek sokszor kell kinéznie. Ez előfordulhat, hogy vagy támadástól, amely a megszerzett hello valós felhasználónév jelzi, és próbálkozzon toobrute kényszerített vagy egy automatikus eszközt, amely kódolt jelszó lejárt.

Az irányítópult engedélyezése informatikai tooquickly azonosíthatók a potenciális fenyegetések kapcsolódó tooidentity és hozzáférési toocompany tartozó erőforrások. Különösen akkor fontos tooalso azonosíthatja a potenciális trendeket, például a hello bejelentkezések keresztül idő csempe, látható hányszor történt meg a sikertelen bejelentkezési kísérletek időtartamon belül. Ebben az esetben a számítógép hello **fájlkiszolgáló** bejelentkezések 35 kapott. Azt is ismerheti meg a számítógép további részleteit. 

![További részletekért](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig7-new.png)

Ezen a számítógépen létrehozott hello jelentés során az ebben a mintában értékes részleteit. Első fellépése adott hello **fiók** oszlop ad meg a felhasználói fiók, de a használt tootry tooaccess hello hello rendszer hello **TIMEGENERATED** oszlop ad meg hello alatt az időtartam alatt a mely hello kísérlet történt és Hello **LOGONTYPENAME** oszlop ad meg hello helyre, ahol ez a kísérlet történt. Ha ezeket a kísérleteket forráskörnyezetben végzett helyileg hello rendszer egy program, hello **folyamat** oszlop volna megjelenítő hello folyamat nevét. Olyan esetekben, ahol hello bejelentkezési kísérlet egy program érkezik már hello Folyamatnév érhető el, és most hajthat végre további vizsgálat hello célrendszerben.

## <a name="see-also"></a>Lásd még:
Ebben a dokumentumban, megtudta, hogyan toouse OMS biztonsági és hitelesítési megoldás toomonitor az erőforrások. További információ az OMS biztonsági toolearn tekintse meg a következő cikkek hello:

* [Az Operations Management Suite (OMS) áttekintése](operations-management-suite-overview.md)
* [Ismerkedés az Operations Management Suite biztonsági és naplózási megoldás](oms-security-getting-started.md)
* [Figyelés és a válaszoló tooSecurity riasztásait az Operations Management Suite biztonsági és naplózási megoldás](oms-security-responding-alerts.md)

