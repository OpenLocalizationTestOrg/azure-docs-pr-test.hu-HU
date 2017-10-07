---
title: a Security Center aaaIntroduction tooAzure |} Microsoft Docs
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
ms.openlocfilehash: 287dbaaa7e2004c522f103595bc316261daf05b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-security-center"></a>A Security Center bemutatása tooAzure
Információk az Azure Security Centerről, annak főbb funkcióiról és működéséről.

> [!NOTE]
> Korai. június 2017 verziótól kezdve a Security Center használnak hello Microsoft Monitoring Agent toocollect, illetve adatainak tárolásához. Lásd: [Azure Security Center Platform áttelepítési](security-center-platform-migration.md) további toolearn. a cikkben szereplő információkat hello Security Center funkció átmenet toohello Microsoft Monitoring Agent után jelöli.
>
>

## <a name="what-is-azure-security-center"></a>Mi az az Azure Security Center?
 A Security Center segítséget nyújt a megakadályozása, észlelésében és kezelésében toothreats láthatóság növelésével és az Azure-erőforrások hello biztonságát vezérelheti. Az ügyfél összes előfizetésére kiterjedő, integrált biztonsági monitorozást és szabályzatkezelést biztosít, megkönnyíti a nehezen észlelhető fenyegetések azonosítását, és számos biztonsági megoldással együttműködik.

## <a name="key-capabilities"></a>Főbb képességek
 A Security Center könnyen használható és hatékony megelőzését, felderítését és a válasz szolgáltatás képességeket nyújt tooAzure beépített. A főbb funkciók a következők:

| Fázis | Képesség |
| --- | --- |
| Megelőzés |Figyelők hello az Azure-erőforrások biztonsági állapota |
| Megelőzés | Az Azure-előfizetések a vállalat biztonsági követelményeinek, hello típusú alkalmazást használja, és hello az adatok érzékenysége alapján szabályzatokat határoz meg |
| Megelőzés | A szükséges szabályozási szabályzaton alapuló biztonsági javaslatok tooguide szolgáltatások tulajdonosait hello folyamat végrehajtásán |
| Megelőzés | Gyorsan telepíti a Microsoft és a partnerei biztonsági szolgáltatásait és készülékeit |
| Észlelés |Automatikusan gyűjti és elemzi az Azure-erőforrások, a hello hálózati és a partneri megoldások, például kártevőirtó-programok és tűzfalak biztonsági adatait |
| Észlelés | Globális használja a információkat a Microsoft termékei és szolgáltatásai hello Microsoft Digital Crimes Unit (DCU), hello Microsoft biztonsági válasz Center (MSRC), és a külső hírcsatornák fenyegetés |
| Észlelés | Bővített analitikát alkalmaz, beleértve a gépi tanulást és a viselkedéselemzést |
| Válasz |Rangsorolt biztonsági eseményeket/riasztásokat tartalmaz |
| Válasz | Hello támadás és az érintett erőforrásokról hello forrását betekintést nyújt |
| Válasz | Toostop hello fennálló fenyegetés megszüntetésére és a későbbi támadások megelőzése érdekében módszereket javasol |

## <a name="introductory-walkthrough"></a>Bevezető áttekintés

> [!NOTE]
> Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be. Ez a dokumentum nem tartalmaz lépésenkénti útmutatót.
>
>

 A Security Center elérje hello [Azure-portálon](https://azure.microsoft.com/features/azure-portal/). [Jelentkezzen be toohello portal](https://portal.azure.com). Hello fő portál menüjében, görgessen a toohello **Security Center** lehetőséget, vagy válassza hello **Security Center** korábban már toohello portál Irányítópultjára rögzített csempén.

![Biztonság csempe az Azure Portalon][1]

A Security Centerből biztonsági szabályzatokat állíthat be, figyelheti a biztonsági konfigurációkat, és megtekintheti a biztonsági riasztásokat.

### <a name="security-policies"></a>Biztonsági szabályzatok
Az Azure-előfizetések tooyour a vállalat biztonsági követelményeinek megfelelően szabályzatokat adhat meg. Akkor is szabhatja toohello típusú használata alkalmazások vagy hello adatok érzékenységének toohello minden előfizetésben. Például különböző biztonsági követelmények vonatkozhatnak a fejlesztésben vagy tesztelésben, illetve az éles környezetben használt erőforrásokra. A szabályozott adatokkal (például személyes adatokkal) működő alkalmazások is magasabb szintű biztonságot követelhetnek meg.

> [!NOTE]
> a biztonsági házirend toomodify biztonsági rendszergazdai vagy hello előfizetés tulajdonosának vagy Közreműködőjének kell lennie. További információ a szerepkörök és a Security Center engedélyezett műveletek toolearn lásd [engedélyek az Azure Security Centerben](security-center-permissions.md).
>
>

A hello **Security Center** panelen, jelölje be hello **házirend** csempére az előfizetések és az erőforráscsoportok listáját.   

![Security Center panel][2]

A hello **biztonsági házirend** panelen válassza ki egy előfizetés tooview hello házirend részletei.

**Adatgyűjtés** lehetővé teszi az adatok gyűjtése az olyan biztonsági házirenddel. Az engedélyezés esetén a következő műveletekre kerül sor:

* Napi vizsgálata az összes támogatott virtuális gépek (VM) biztonsági figyelést és javaslatokat.
* Biztonsági események gyűjtése elemzési célokra és a fenyegetések észlelésére.

> [!NOTE]
> Adatgyűjtés hello előfizetés szintjén állítható be.
>
>

Válassza ki **megakadályozási szabályzat** tooopen hello **megakadályozási szabályzat** panelen. **Javaslatok megjelenítése** lehetővé teszi, hogy válassza ki a megjeleníteni kívánt hello biztonsági igényeinek megfelelően hello erőforrások hello előfizetésen belül toosee toomonitor és hello javaslatok kívánt hello biztonsági vezérlőket.

### <a name="security-recommendations"></a>Biztonsági javaslatok
 A Security Center elemzi az Azure-erőforrások tooidentify potenciális biztonsági hiányosságok hello biztonsági állapotát. A javaslatok listája végigvezeti hello szükséges szabályozási konfigurálásának lépésein. Példák erre vonatkozóan:

* Üzembe helyezési kártevőirtó toohelp azonosítása és eltávolítani a kártevő szoftvereket
* Hálózati biztonsági csoportok és a szabályok toocontrol forgalom tooVMs konfigurálása
* A webes alkalmazások célzó támadások elleni védelemre toohelp webalkalmazási tűzfalak kiépítése
* Hiányzó rendszerfrissítések telepítése
* Operációs rendszer azon konfigurációit, amelyek nem felelnek meg a hello címzési ajánlott alaptervek

Kattintson a hello **javaslatok** csempére a javaslatok listája. Kattintson az egyes ajánlás tooview további információkat vagy tootake művelet tooresolve hello probléma.

![Biztonsági javaslatok az Azure Security Centerben][5]

### <a name="security-state-of-azure-resources"></a>Az Azure-erőforrások biztonsági állapota
Hello **megelőzési** hello irányítópult a szakasz azt mutatja be hello erőforrástípusok szerint, beleértve a virtuális gépek, webes alkalmazások és más erőforrások hello környezet általános biztonsági állapotát.   

Válasszon ki egy erőforrástípust a **megelőzési** tooview további információt, beleértve a potenciális biztonsági hiányosságok azonosított listáját. (**Számítási** hello az alábbi példa az van kiválasztva.)

![Az Erőforrások állapota csempe][6]

### <a name="security-alerts"></a>Biztonsági riasztások
 A Security Center automatikusan gyűjti, elemzi és integrálja az Azure-erőforrások, a hello hálózati és a partneri megoldások, például kártevőirtó-programok és tűzfalak naplóadatait. Fenyegetések észlelése esetén a központ biztonsági riasztást hoz létre. Példák fenyegetés észlelésére:

* Feltört virtuális gépek kommunikál az ismert kártékony IP-címek
* Windows hibajelentés által észlelt speciális kártevő
* Virtuális gépek elleni találgatásos támadások ellen
* Az integrált kártevőirtó programok és a tűzfalak biztonsági riasztásai

Gombra kattintva hello **biztonsági riasztások** csempe megjeleníti a rangsorolt riasztásokat.

![Biztonsági riasztások][7]

Egy riasztás kiválasztásával további információ a hello támadás és javaslatokat arról, hogyan tooremediate azt.

![Biztonsági figyelmeztetés részletei][8]

### <a name="partner-solutions"></a>Partneri megoldások
Hello **partneri megoldások** csempe segítségével figyelheti a partnermegoldások egy pillanat alatt hello biztonsági állapotát, integrálva van az Azure-előfizetéshez. A Security Center hello megoldások riasztásait jeleníti meg.

Jelölje be hello **partneri megoldások** csempére. Ekkor megnyílik egy panel, amely a központtal összekapcsolt partnermegoldások listáját tartalmazza.

![Partneri megoldások][9]

## <a name="get-started"></a>Bevezetés
a Security Center használatába tooget, szüksége van egy előfizetési tooMicrosoft Azure. A Security Center engedélyezve van Azure- előfizetéséhez. Ha nem rendelkezik előfizetéssel, regisztrálhat egy [ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/).

 A Security Center elérje hello [Azure-portálon](https://azure.microsoft.com/features/azure-portal/). Lásd: hello [portál dokumentációjában](https://azure.microsoft.com/documentation/services/azure-portal/) további toolearn.

[Ismerkedés az Azure Security Center](security-center-get-started.md) gyorsan megismerheti a Security Center hello biztonsági biztonságfelügyeleti és szabályzat-kezelési összetevőit.

## <a name="next-steps"></a>Következő lépések
Ebben a dokumentumban volt a bevezetett tooSecurity Center, annak főbb funkcióiról, és hogyan tooget. toolearn több, tekintse meg a következő erőforrások hello:

* [Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – további hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.
* [Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.
* [Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – további hogyan toomanage és válaszoljon toosecurity riasztásokat.
* [Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.
- [Az Azure Security Center adatainak biztonsági](security-center-data-security.md) -megtudhatja, hogyan adatok felügyelt és a Security Center védelmét.
* [Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
* [Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – hello legújabb Azure biztonsági hírek és információ lekérése.

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
