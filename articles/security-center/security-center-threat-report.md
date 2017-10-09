---
title: "a Security Center fenyegetés az eszközintelligencia-jelentés aaaAzure |} Microsoft Docs"
description: "Ez a dokumentum segít toouse Azure Security Center fenyegetés intelligens jelentések egy vizsgálat toofind során egy biztonsági riasztás kapcsolatos további információt."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 5662e312-e8c2-4736-974e-576eeb333484
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: c888cfac1dd8b057616a6b8e6c6f6b67b552f2e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-threat-intelligence-report"></a>Azure Security Center fenyegetésfelderítési jelentés
Ez a dokumentum ismerteti, hogy az Azure Security Center fenyegetésfelderítési jelentés segítségével hogyan tudhat meg többet az olyan fenyegetésekről, amelyek biztonsági riasztást hoztak létre.

## <a name="what-is-a-threat-intelligence-report"></a>Mi a fenyegetésfelderítési jelentés?
A Security Center fenyegetésészlelés működése figyelése az Azure-erőforrások, hello és hálózati összekapcsolt partnermegoldások biztonsági adatait. Ezt az információt, gyakran adatok az adatokat több forrásból tooidentify fenyegetések megvizsgálja. Ez a folyamat része a Security Center hello [az észlelési képességek](security-center-detection-capabilities.md).

Ha a Security Center azonosít egy fenyegetést, akkor az [biztonsági riasztást](security-center-managing-and-responding-alerts.md) vált ki, amely részletes információkat tartalmaz az adott eseménnyel kapcsolatban, beleértve az elhárítási javaslatokat is. tooassist incidensválasz-csoportok vizsgálja meg, és szervizelheti azokat a fenyegetéseket, a Security Center tartalmaz egy fenyegetés az eszközintelligencia-jelentést hello fenyegetést észlelt, például a párbeszédeket kapcsolatos információkat tartalmazó a:

* A támadó identitása vagy társításai (ha ez az információ elérhető)
* A támadó céljai
* Jelenlegi és korábbi támadássorozatok (ha ez az információ elérhető)
* A támadó taktikája, eszközei és eljárásai
* A sérüléssel kapcsolatos mutatók (IoC), például URL-címek és fájlkivonatok
* Victimology, amely hello iparág és földrajzi elterjedtségének tooassist meghatározhatja, hogy az Azure-erőforrások fenyegeti a
* Kezelési és elhárítási információk

> [!NOTE]
> bármely adott jelentésben szereplő információk mennyisége hello változó; hello részletességi hello kártevő tevékenységet és az elterjedtségének alapul.
>
>

A Security Center háromféle fenyegetés jelentéseit, amelyek függően toohello támadás eltérőek lehetnek. hello elérhető jelentéseket a következők:

* **Tevékenységi csoport jelentés**: mélyreható információkat biztosít a támadókról, a céljaikról és a taktikáikról.
* **Kampányjelentés**: adott támadási kampányok részleteire összpontosít.
* **A fenyegetés összegző jelentés**: az előző két jelentés hello hello elemeket ismerteti.

Az ilyen információkat nagyon hasznos során hello [incidensválasz](security-center-incident-response.md) folyamatok, ahol van egy folyamatban lévő vizsgálat toounderstand hello forrás hello támadás, hello támadó célokat, és mi toodo toomitigate Ez a probléma soron.

## <a name="how-tooaccess-hello-threat-intelligence-report"></a>Hogyan tooaccess hello fenyegetés az eszközintelligencia-jelentés?
Hello bármikor áttekintheti az aktuális riasztásokat **biztonsági riasztások** csempére. Nyissa meg a hello Azure portálon, és lépések hello toosee alatt az egyes riasztásokkal kapcsolatos további részletek:

1. A Security Center irányítópultjának hello, látni fogja a hello **biztonsági riasztások** csempére.
2. Kattintson a hello csempe tooopen hello **biztonsági riasztások** panel, amelyen hello riasztások további információkat tartalmaz, és kattintson az lehetőségre hello biztonsági riasztást, amelyet az tooobtain további információt kapcsolatban.

    ![Biztonsági riasztások](./media/security-center-threat-report/security-center-threat-report-fig1.png)
3. Ebben az esetben hello **gyanús folyamat végrehajtása** panel hello riasztást, ahogy az az alábbi ábra hello hello részleteit jeleníti meg:

    ![Biztonsági riasztás részletei](./media/security-center-threat-report/security-center-threat-report-fig2.png)
4. információk érhetők el az egyes biztonsági riasztások hello mennyiségű riasztás toohello típusától függően változnak. A hello **jelentések** mező a hivatkozás toohello fenyegetés eszközintelligencia jelentést. Kattintson a hivatkozásra, és egy PDF-fájl jelenik meg egy új böngészőablakban.

   ![Tároló kiválasztása](./media/security-center-threat-report/security-center-threat-report-fig3.png)

Itt a rendszer észlelte a jelentésben és tájékozódhat a hello biztonsági probléma, amely olvasási PDF hello letöltése, és a cselekvéshez hello foglalt információk alapján.

## <a name="see-also"></a>Lásd még:
Ebben a dokumentumban megismerkedhetett azzal, hogyan segítheti Önt az Azure Security Center fenyegetésfelderítési jelentés a biztonsági riasztások vizsgálata során. További információ az Azure Security Center toolearn hello következő lásd:

* [Azure Security Center – gyakori kérdések](security-center-faq.md) Hello szolgáltatással kapcsolatos gyakran ismételt kérdések található.
* [Az Azure Security Center használata incidensmegoldásra](security-center-incident-response.md)
* [Az Azure Security Center észlelési képességei](security-center-detection-capabilities.md)
* [Útmutató az Azure Security Center tervezéséhez és működtetéséhez](security-center-planning-and-operations-guide.md). Megtudhatja, hogyan tooplan és hello kialakítási szempontok tooadopt az Azure Security Center ismertetése.
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md). Megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.
* [Biztonsági incidensek kezelése az Azure Security Centerben](security-center-incident.md)
* [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.
