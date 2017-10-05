---
title: "Azure Security Center bemutatása |} Microsoft Docs"
description: "Információk az Azure Security Centerről, annak főbb funkcióiról és működéséről."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: 8951167213da6ab5341c1ca420353ec625ef5424
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-azure-security-center"></a>Az Azure Security Center bemutatása
Információk az Azure Security Centerről, annak főbb funkcióiról és működéséről.

> [!NOTE]
> 2017 júniusának elejétől kezdve a Security Center a Microsoft Monitoring Agent használatával gyűjti össze és tárolja az adatokat. További információk: [Az Azure Security Center Platform migrálása](security-center-platform-migration.md). A jelen cikkben található információk a Security Center a Microsoft Monitoring Agentre való váltás után elérhető funkcióit ismertetik.
>
>

## <a name="what-is-azure-security-center"></a>Mi az az Azure Security Center?
 A Security Center az Azure-erőforrások biztonsági felügyeletének átláthatóbbá és szabályozhatóbbá tételével megkönnyíti a fenyegetések megelőzését, észlelését és elhárítását. Az ügyfél összes előfizetésére kiterjedő, integrált biztonsági monitorozást és szabályzatkezelést biztosít, megkönnyíti a nehezen észlelhető fenyegetések azonosítását, és számos biztonsági megoldással együttműködik.

## <a name="key-capabilities"></a>Főbb funkciók
 A Security Center az Azure portál beépített funkcióiként biztosítja a fenyegetések egyszerű és hatékony megelőzését, észlelését és az azokra való reagálást. A főbb funkciók a következők:

| Fázis | Képesség |
| --- | --- |
| Megelőzés |Az Azure-erőforrások biztonsági állapotának monitorozása |
| Megelőzés | Meghatározza az Azure-előfizetések alapján a vállalat biztonsági követelményeinek, milyen típusú alkalmazások, hogy használja, és az adatok érzékenysége házirendek |
| Megelőzés | A szabályzaton alapuló biztonsági javaslatok használatával végigvezeti a szolgáltatások tulajdonosait a szükséges szabályozási folyamat végrehajtásán |
| Megelőzés | Gyorsan telepíti a Microsoft és a partnerei biztonsági szolgáltatásait és készülékeit |
| Észlelés |Automatikusan gyűjti és elemzi az Azure-erőforrások, a hálózat és a partneri megoldások, például kártevőirtó-programok és tűzfalak biztonsági adatait |
| Észlelés | Eszközintelligencia, a Microsoft-termékek és szolgáltatások, a Microsoft Digital Crimes Unit (DCU), a Microsoft biztonsági válasz Center (MSRC), és külső hírcsatornák globális használja a fenyegetés |
| Észlelés | Bővített analitikát alkalmaz, beleértve a gépi tanulást és a viselkedéselemzést |
| Válasz |Rangsorolt biztonsági eseményeket/riasztásokat tartalmaz |
| Válasz | Áttekintést nyújt a támadás forrásáról és az érintett erőforrásokról |
| Válasz | Módszereket javasol a fennálló fenyegetés megszüntetésére és a jövőbeli fenyegetések megelőzésre |

## <a name="introductory-walkthrough"></a>Bevezető áttekintés

> [!NOTE]
> Ez a dokumentum egy üzembe helyezést szemléltető példa segítségével mutatja be a szolgáltatást. Ez a dokumentum nem tartalmaz lépésenkénti útmutatót.
>
>

 A Security Center az [Azure Portalról](https://azure.microsoft.com/features/azure-portal/) érhető el. [Jelentkezzen be a portálra](https://portal.azure.com). A fő portál menüjében, görgessen a **Security Center** lehetőséget, vagy válassza ki a **Security Center** korábban már a portál Irányítópultjára rögzített csempén.

![Biztonság csempe az Azure Portalon][1]

A Security Centerből biztonsági szabályzatokat állíthat be, figyelheti a biztonsági konfigurációkat, és megtekintheti a biztonsági riasztásokat.

### <a name="security-policies"></a>Biztonsági szabályzatok
Az Azure-előfizetések a vállalat biztonsági követelményeinek megfelelően szabályzatokat adhat meg. Ezeket testre is szabhatja az adott alkalmazás típusa vagy az egyes előfizetések adatainak érzékenysége alapján. Például különböző biztonsági követelmények vonatkozhatnak a fejlesztésben vagy tesztelésben, illetve az éles környezetben használt erőforrásokra. A szabályozott adatokkal (például személyes adatokkal) működő alkalmazások is magasabb szintű biztonságot követelhetnek meg.

> [!NOTE]
> Módosíthatja a biztonsági szabályzatot, a biztonsági rendszergazda vagy az előfizetés tulajdonosának vagy Közreműködőjének kell lennie. További információ a szerepkörök és engedélyezett műveletek a biztonsági központban, [engedélyek az Azure Security Centerben](security-center-permissions.md).
>
>

A **Security Center** panelen válassza az előfizetések és az erőforráscsoportok kívánt listájához tartozó **Szabályzat** csempét.   

![Security Center panel][2]

Az a **biztonsági házirend** panelen válasszon ki egy előfizetést a szabályzat részleteinek megtekintéséhez.

**Adatgyűjtés** lehetővé teszi az adatok gyűjtése az olyan biztonsági házirenddel. Az engedélyezés esetén a következő műveletekre kerül sor:

* Napi vizsgálata az összes támogatott virtuális gépek (VM) biztonsági figyelést és javaslatokat.
* Biztonsági események gyűjtése elemzési célokra és a fenyegetések észlelésére.

> [!NOTE]
> Adatgyűjtés úgy van beállítva, az előfizetés szintjén.
>
>

Válassza ki **megakadályozási szabályzat** megnyitásához a **megakadályozási szabályzat** panelen. **Javaslatok megjelenítése** megadható, hogy a biztonsági funkciók, amelyek segítségével nyomon követni kívánt és a javaslatok, hogy meg szeretné tekinteni az előfizetésen belüli erőforrások biztonsági igényeinek alapján választja.

### <a name="security-recommendations"></a>Biztonsági javaslatok
 A Security Center a potenciális biztonsági hiányosságok azonosítása érdekében elemzi az Azure-erőforrások biztonsági állapotát. A javaslatok listája végigvezeti Önt a szükséges szabályozási folyamatok konfigurálásának folyamatán. Példák erre vonatkozóan:

* Kártevőirtók kiépítése a kártékony szoftverek azonosításához és eltávolításához
* Hálózati biztonsági csoportok és a virtuális gépek forgalmának ellenőrzésére vonatkozó szabályok konfigurálása
* Webalkalmazási tűzfalak kiépítése a webalkalmazások ellen irányuló támadások elleni védelemre
* Hiányzó rendszerfrissítések telepítése
* Az operációs rendszer azon konfigurációinak kezelése, amelyek nem felelnek meg a javasolt alapkonfigurációknak

Kattintson a **Javaslatok** csempére a javaslatok listájának megtekintéséhez. Kattintson az egyes javaslatot a további információk megjelenítéséhez, vagy a probléma megoldásához.

![Biztonsági javaslatok az Azure Security Centerben][5]

### <a name="security-state-of-azure-resources"></a>Az Azure-erőforrások biztonsági állapota
A **megelőzési** az irányítópult része mutatja meg erőforrástípusok szerint, a környezet általános biztonsági állapotát, beleértve a virtuális gépek, webes alkalmazások és más erőforrásokat.   

Válasszon ki egy erőforrástípust a **megelőzési** további információk, beleértve a potenciális biztonsági hiányosságok azonosított listájának megjelenítéséhez. (**Számítási** az alábbi példában az van kiválasztva.)

![Az Erőforrások állapota csempe][6]

### <a name="security-alerts"></a>Biztonsági riasztások
 A Security Center automatikusan gyűjti, elemzi és integrálja az Azure-erőforrások, a hálózat és az olyan partnermegoldások, mint például a kártevőirtó-programok és a tűzfalak naplóadatait. Fenyegetések észlelése esetén a központ biztonsági riasztást hoz létre. Példák fenyegetés észlelésére:

* Feltört virtuális gépek kommunikál az ismert kártékony IP-címek
* Windows hibajelentés által észlelt speciális kártevő
* Virtuális gépek elleni találgatásos támadások ellen
* Az integrált kártevőirtó programok és a tűzfalak biztonsági riasztásai

A **Biztonsági riasztások** csempére kattintva megjelenítheti a rangsorolt riasztásokat.

![Biztonsági riasztások][7]

Egy riasztás kiválasztásával további információkat tekinthet meg a támadásról, valamint javaslatokat kap a támadás elhárítására vonatkozóan.

![Biztonsági figyelmeztetés részletei][8]

### <a name="partner-solutions"></a>Partneri megoldások
A **partneri megoldások** csempén egy pillanat alatt az Azure előfizetésében integrált partneri megoldások biztonsági állapotát. A Security Center itt a partnermegoldások riasztásait jeleníti meg.

Válassza a **Partneri megoldások** csempét. Ekkor megnyílik egy panel, amely a központtal összekapcsolt partnermegoldások listáját tartalmazza.

![Partneri megoldások][9]

## <a name="get-started"></a>Bevezetés
A kezdéshez a Security Center szüksége van a Microsoft Azure-előfizetéssel. A Security Center engedélyezve van Azure- előfizetéséhez. Ha nem rendelkezik előfizetéssel, regisztrálhat egy [ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/).

 A Security Center az [Azure Portalról](https://azure.microsoft.com/features/azure-portal/) érhető el. További információt a [portál dokumentációjában](https://azure.microsoft.com/documentation/services/azure-portal/) talál.

Az [Ismerkedés az Azure Security Centerrel](security-center-get-started.md) segítségével gyorsan megismerheti a Security Center biztonságfelügyeleti és szabályzat-kezelési összetevőit.

## <a name="next-steps"></a>Következő lépések
Ebben a dokumentumban megismerhette a Security Centert, annak főbb funkcióit és használatának első lépéseit. További tudnivalókért lásd a következőket:

* [Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – útmutató az Azure-előfizetések és -erőforráscsoportok biztonsági szabályzatainak konfigurálásához.
* [Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.
* [Biztonsági állapotmonitorozás az Azure Security Centerben](security-center-monitoring.md) – Útmutató az Azure-erőforrások állapotának monitorozásához.
* [Kezelése és válaszadás a biztonsági riasztásokra az Azure Security Center](security-center-managing-and-responding-alerts.md) – útmutató kezelése és válaszadás a biztonsági riasztásokra.
* [Partneri megoldások monitorozása az Azure Security Centerrel](security-center-partner-solutions.md) – Útmutató a partneri megoldások biztonsági állapotának monitorozásához.
- [Az Azure Security Center adatainak biztonsági](security-center-data-security.md) -megtudhatja, hogyan adatok felügyelt és a Security Center védelmét.
* [Azure Security Center FAQ](security-center-faq.md) (Azure Security Center: Gyakran ismételt kérdések) – Válaszok a szolgáltatás használatára vonatkozó gyakori kérdésekre.
* [Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – a legújabb Azure biztonsági híreket és információkat.

<!--Image references-->
[1]: ./media/security-center-intro/security-tile.png
[2]: ./media/security-center-intro/security-center.png
[3]: ./media/security-center-intro/security-policy.png
[4]: ./media/security-center-intro/security-policy-blade.png
[5]: ./media/security-center-intro/recommendations.png
[6]: ./media/security-center-intro/resources-health.png
[7]: ./media/security-center-intro/security-alert.png
[8]: ./media/security-center-intro/security-alert-detail.png
[9]: ./media/security-center-intro/partner-solutions.png
