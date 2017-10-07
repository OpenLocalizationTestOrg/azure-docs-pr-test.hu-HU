---
title: "az Azure Security Center biztonsági riasztások aaaManage |} Microsoft Docs"
description: "Ez a dokumentum segít, toouse az Azure Security Center képességek toomanage, és toosecurity riasztások válaszol."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: b88a8df7-6979-479b-8039-04da1b8737a7
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: yurid
ms.openlocfilehash: f1cb7e4770776827b75ed15893914678c1f44216
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-and-responding-toosecurity-alerts-in-azure-security-center"></a>Kezelése és reagálás toosecurity értesítések az Azure Security Centerben
Ez a dokumentum segítséget nyújt a használja az Azure Security Center toomanage és toosecurity riasztások válaszol.

> [!NOTE]
> Speciális tooenable észlelések, a Security Center szabványos frissítési tooAzure. A 60 napos próbaverzió ingyenes. tooupgrade, hello válassza árképzési szintjében [biztonsági házirend](security-center-policies.md). Lásd: [az Azure Security Center árképzési](security-center-pricing.md) további toolearn.
>
>

## <a name="what-are-security-alerts"></a>Mik azok a biztonsági riasztások?
A Security Center automatikusan gyűjti, elemzi, és integrálja az Azure-erőforrások, hello hálózati naplóadatait és partneri megoldások, például tűzfal és az endpoint protection megoldások, toodetect valós fenyegetések csatlakoztatva és vakriasztások számának csökkentése érdekében. A rangsorolt biztonsági riasztások listája látható a Security Center együtt hello tooquickly szükséges információk vizsgálata hello probléma és módjára vonatkozó javaslatokkal tooremediate támadás.


> [!NOTE]
> Ha részletes tájékoztatást szeretne kapni a Security Center észlelési funkcióinak működéséről, olvassa el [Az Azure Security Center észlelési funkciói](security-center-detection-capabilities.md) című cikket.
>
>

## <a name="managing-security-alerts"></a>Biztonsági riasztások kezelése
Hello bármikor áttekintheti az aktuális riasztásokat **biztonsági riasztások** csempére. Azure portál megnyitásához és a lépések hello toosee alatt az egyes riasztásokkal kapcsolatos további részletek:

1. A Security Center irányítópultjának hello, látni fogja a hello **biztonsági riasztások** csempére.

    ![Biztonsági riasztások csempe a Security Centerben](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig1-ga.png)

2. Kattintson a hello csempe tooopen hello **biztonsági riasztások** hello további információkat tartalmazó panel figyelmezteti a lent látható módon.

   ![hello biztonsági riasztások panel az Security Centerben](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig2-ga.png)

A panel alsó részén hello vannak az egyes riasztások részletei hello. toosort, kattintson a hello oszlop toosort által használni kívánt. az alábbi táblázat a hello minden oszlop definíciója:

* **Leírás**: hello riasztás rövid leírása.
* **Szám:** az adott típusú riasztások listája egy adott napra vonatkozóan.
* **Által észlelt**: hello hello riasztás kiváltásáért felelős szolgáltatás.
* **Dátum**: hello dátum, hogy hello esemény történt.
* **Állapot**: hello riasztás aktuális állapota. Kétféle állapot létezik:
  * **Aktív**: hello biztonsági riasztást észlelték.
* **Súlyossági**: hello súlyossági szintet, amely magas, közepes vagy alacsony lehet.

### <a name="filtering-alerts"></a>A riasztások szűrése
A riasztások dátum, állapot és súlyosság alapján szűrhetők. A riasztások szűrését forgatókönyvek esetén, ha a biztonsági riasztások megjelenítése toonarrow hello hatókörében kell hasznos lehet. Például akkor lehet, hogy azt szeretné, hogy tooaddress történt biztonsági riasztásokat hello az elmúlt 24 órában, mert egy történő lehetséges behatolást vizsgál hello rendszerben.

1. Kattintson a **szűrő** a hello **biztonsági riasztások** panelen. Hello **szűrő** panel megnyitása és toosee kívánja hello dátum, állapot és súlyosság értékek választja.

    ![A riasztások szűrése a Security Centerben](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig3-2017.png)

### <a name="respond-toosecurity-alerts"></a>Válaszoljon toosecurity riasztások
Válassza ki a biztonsági riasztás toolearn hello esemény, ha vannak ilyenek, lépéseket és mit, melyik hello riasztás kiváltó többet kell tootake tooremediate támadás. A biztonsági riasztások típus és dátum szerint vannak csoportosítva. Egy biztonsági riasztás kattintva megnyílik egy hello csoportosított riasztások listáját tartalmazó panel.

![Válaszoljon az Azure Security Centerben toosecurity riasztások](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig5-ga.png)

Ebben az esetben hello riasztások váltotta tekintse meg a toosuspicious Remote Desktop Protocol (RDP) tevékenységet. hello az első oszlopban látható kitett erőforrások; hello második bemutatja, hogy hányszor hello erőforrás támadásnak kitett erőforrásra; hello harmadik hello időt hello támadások; jeleníti meg hello negyedik hello riasztás; állapotának megjelenítése és hello ötödik hello támadás hello súlyosságát mutatja. Ezek az információk áttekintése után kattintson a támadásnak kitett erőforrásra hello erőforrás és egy új panel nyílik meg.

![Javaslatok az Azure Security Center milyen toodo biztonsággal kapcsolatos riasztások](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig6-ga.png)

A hello **leírás** mezőjét ezen a panelen, ez az esemény további információt talál. Ezek az információk milyen kiváltott hello biztonsági riasztást, hello cél erőforráson, betekintést ajánlatot a megfelelő hello forrás IP-címet, és a fenyegetés elhárítására vonatkozó javaslatokról tooremediate.  Bizonyos esetekben hello forrás IP-címe üres lesz (nem érhető el), mert nem minden Windows biztonsági eseménynaplója tartalmazza hello IP-címet.

a Security Center által javasolt elhárítási műveletek hello konfigurációtól függően toohello biztonsági riasztást. Bizonyos esetekben előfordulhat, hogy toouse más Azure-képességek tooimplement hello szervizelési ajánlott. Az ilyen jellegű támadás tooblacklist hello IP-címet, hogy a támadás használatával például hello a szervizelési egy [hálózati hozzáférés-vezérlési lista](../virtual-network/virtual-networks-acl.md) vagy egy [hálózati biztonsági csoport](../virtual-network/virtual-networks-nsg.md) szabály.

> [!NOTE]
> További információk a riasztások különböző típusú hello, [típusonként az Azure Security Center biztonsági riasztások](security-center-alerts-type.md).
>
>

## <a name="see-also"></a>Lásd még:
Ebben a dokumentumban, megtudta, hogyan tooconfigure biztonsági házirendek segítségével a Security Center. További információ a Security Center toolearn hello következő lásd:

* [Biztonsági incidensek kezelése az Azure Security Centerben](security-center-incident.md)
* [Az Azure Security Center észlelési funkciói](security-center-detection-capabilities.md)
* [Útmutató az Azure Security Center tervezéséhez és működtetéséhez](security-center-planning-and-operations-guide.md)
* [Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
* [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) – Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.
