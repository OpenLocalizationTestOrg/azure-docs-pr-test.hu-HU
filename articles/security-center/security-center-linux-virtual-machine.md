---
title: "aaaAzure biztonsági központ és az Azure virtuális gépeken Linux |} Microsoft Docs"
description: "Ez a dokumentum segít toounderstand hogyan az Azure Security Center képes védelme érdekében Azure virtuális gépeken."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 5fe5a12c-5d25-430c-9d47-df9438b1d7c5
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2017
ms.author: yurid
ms.openlocfilehash: d7aa9e54032272839dabfefa30c4c614d5e5610a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-and-azure-virtual-machines-with-linux"></a>Az Azure Security Center és Linux rendszerű Azure-beli virtuális gépek
[Az Azure Security Center](https://azure.microsoft.com/services/security-center/) megakadályozása, észlelésében és kezelésében toothreats segít. Az ügyfél összes előfizetésére kiterjedő, integrált biztonsági monitorozást és szabályzatkezelést biztosít, megkönnyíti a nehezen észlelhető fenyegetések azonosítását, és számos biztonsági megoldással együttműködik.

Ez a cikk azt ismerteti, hogyan segíthet a Security Center a Linuxot futtató Azure-beli virtuális gépek (VM) védelmében.

## <a name="why-use-security-center"></a>Miért használja a Security Centert?
A Security Center segít megóvni az Azure-beli virtuális gépeken tárolt adatokat azáltal, hogy betekintést nyújt a virtuális gép biztonsági beállításaiba, és nyomon követi az esetleges fenyegetéseket. A Security Center a következőket tudja megfigyelés alatt tartani a virtuális gépeken: 

* Operációs rendszer (az operációs rendszer) biztonsági beállítások hello az ajánlott konfigurálási szabályok
* Rendszerbiztonság és a hiányzó kritikus frissítések
* Az Endpoint Protection javaslatai
* Lemeztitkosítás ellenőrzése
* Hálózatalapú támadások (csak a [standard verzióban](https://azure.microsoft.com/en-us/pricing/details/security-center/) érhető el)

Ezenkívül toohelping az Azure virtuális gépek védelme, a Security Center a biztonsági figyelést és Felhőszolgáltatások, alkalmazásszolgáltatások, virtuális hálózatok és több felügyeleti is biztosít. 

> [!NOTE]
> Lásd: [Security Center bemutatása tooAzure](security-center-intro.md) Azure Security Centerrel kapcsolatos további toolearn.
> 
> 

## <a name="prerequisites"></a>Előfeltételek
az Azure Security Center használatába tooget lesz tooknow kell, és vegye figyelembe a következőket hello:

* Rendelkeznie kell egy előfizetési tooMicrosoft Azure. A Security Center ingyenes és standard csomagjairól a [ Security Center díjszabását](https://azure.microsoft.com/pricing/details/security-center/) ismertető oldalon talál további információt.
* A Security Center bevezetését tervezése című [az Azure Security Center tervezésével és műveletek útmutatója](security-center-planning-and-operations-guide.md) toolearn további tervezési és műveletek használata.
* További információ a támogatott operációs rendszerekről: [Azure Security Centerhez kapcsolódó gyakori kérdések (GYIK)](security-center-faq.md). 

## <a name="set-security-policy"></a>Biztonsági házirend beállítása
Adatok gyűjtemény igényeinek toobe engedélyezve van, úgy, hogy az Azure Security Center képes összegyűjteni hello tooprovide javaslatok és a riasztásokat, amelyek akkor jönnek létre kell konfigurálnia hello biztonsági házirend alapján. A hello alábbi ábrán látható, amely **adatgyűjtés** lett kapcsolva a **a**.

A biztonsági házirend szabályozza, amely hello megadott előfizetés vagy az erőforrás-csoporton belüli erőforrások ajánlott hello csoportját határozza meg. Ahhoz, hogy a biztonsági házirend, rendelkeznie kell engedélyezett adatgyűjtés, a Security Center összegyűjti az adatokat a virtuális gépek biztonsági állapotukra biztonsági javaslatokkal, és riasztást küldjön, toothreats tooassess sorrendben. A biztonsági központban állíthatja be a szabályzatokat az Azure-előfizetések vagy erőforráscsoportok tooyour vállalat biztonsági igényeinek és hello szereplő alkalmazások típusának vagy az egyes előfizetések hello adatok érzékenységének megfelelően. 

![Biztonsági házirend](./media/security-center-linux-virtual-machine/security-center-linux-virtual-machine-fig1.png)

> [!NOTE]
> További információk az egyes toolearn **megakadályozási szabályzat** érhető el, lásd: [biztonsági házirendek beállítása](security-center-policies.md) cikk.
> 

## <a name="manage-security-recommendations"></a>Biztonsági javaslatok kezelése
A Security Center elemzi az Azure-erőforrások biztonsági állapotának hello. A Security Center javaslatokat hoz létre, amikor lehetséges biztonsági réseket észlel. hello javaslatok végigvezetik hello hello szükséges vezérlők konfigurálásának lépésein.

Miután beállította a biztonsági házirendet, a Security Center elemzi az erőforrások tooidentify potenciális biztonsági réseket hello biztonsági állapotát. hello javaslatok ahol mindegyik sor jelenti-e egy adott javaslat táblázatos formátumban jelennek meg. hello az alábbi táblázat néhány példa a javaslatok az Azure virtuális gépeken futó Linux operációs rendszert, és hogy a minden egyes milyen fogja elvégezni, ha alkalmazza azt. Amikor kiválaszt egy javaslatot, akkor nyújtanak információt, amely bemutatja, hogyan tooimplement hello érték a Security Center.

| Ajánlás | Leírás |
| --- | --- |
| [Adatgyűjtés engedélyezése az előfizetések számára](security-center-enable-data-collection.md) |Javasolja, hogy kapcsolja be hello biztonsági házirend adatgyűjtés minden egyes előfizetésnél és az összes virtuális gépek (VM) az Ön előfizetéseit. |
| [Operációs rendszerek sebezhetőségeinek javítása](security-center-remediate-os-vulnerabilities.md) |Az operációs rendszer azon konfigurációinak igazodni ajánlott a konfigurációs szabályok hello javasolja például nem engedélyezik a mentett jelszavak toobe. |
| [Rendszerfrissítések alkalmazása](security-center-apply-system-updates.md) |Javasolja a hiányzó rendszer biztonsági és kritikus frissítések tooVMs telepíteni. |
| [Rendszerfrissítések utáni újraindítás](security-center-apply-system-updates.md#reboot-after-system-updates) |Javasolja, hogy indítsa újra a virtuális gép toocomplete hello folyamat a rendszer frissítéseinek alkalmazása. |
| [Virtuálisgép-ügynök engedélyezése](security-center-enable-vm-agent.md) |Lehetővé teszi, hogy toosee virtuális gépek igénylő hello Virtuálisgép-ügynök. rendelés tooprovision javítás vizsgálatát, Alapterv vizsgálatát, és a kártevőirtó-programok virtuális gépeken hello ügynököt kell telepíteni. Virtuálisgép-ügynök hello alapértelmezés szerint telepítve van a hello Azure Piactérről származó központilag telepített virtuális gépekhez. hello cikk [ügynök és Virtuálisgép-bővítmények – 2. rész](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) bemutatja, hogyan tooinstall hello Virtuálisgép-ügynök. |
| [Lemeztitkosítás alkalmazása](security-center-apply-disk-encryption.md) |Javasolja, hogy végezze el a virtuális gép titkosítását az Azure Disk Encryption használatával (Windows és Linux rendszerű virtuális gépek esetében). Titkosítási ajánlott hello az operációs rendszer, mind az adatkötetek a virtuális gépen. |


> [!NOTE]
> toolearn vonatkozó javaslatokkal kapcsolatban bővebben lásd: [biztonsági javaslatok kezelése](security-center-recommendations.md) cikk.
> 

## <a name="monitor-security-health"></a>A biztonsági állapot figyelése
Miután engedélyezte a [biztonsági házirendek](security-center-policies.md) az előfizetéshez tartozó erőforrásokra, a Security Center elemzi az erőforrások tooidentify potenciális biztonsági réseket hello biztonságát.  Megtekintheti az erőforrások, valamint hello esetleg felmerülő problémákat hello biztonsági állapotát **erőforrás biztonsági állapota** panelen. Amikor rákattint **virtuális gépek** a hello **erőforrás biztonsági** állapota csempe, hello **virtuális gépek** panel nyílik meg a virtuális gépekre vonatkozó javaslatok. 

![Biztonsági állapot](./media/security-center-virtual-machine/security-center-virtual-machine-fig2.png)

## <a name="manage-and-respond-toosecurity-alerts"></a>Kezelésének és megoldásának toosecurity riasztások
A Security Center automatikusan gyűjti, elemzi és integrálja az Azure-erőforrások, hello és hálózati összekapcsolt partneri megoldások (például tűzfal és az endpoint protection megoldások), naplóadatait toodetect valós fenyegetések és a vakriasztások számának csökkentése érdekében. Használatával a különböző összesítése [az észlelési képességek](security-center-detection-capabilities.md), a Security Center képes toogenerate előrébb biztonsági riasztások toohelp gyorsan hello probléma vizsgálja meg és adja meg a módjára vonatkozó javaslatokkal tooremediate lehetséges támadások.

![Biztonsági riasztások](./media/security-center-virtual-machine/security-center-virtual-machine-fig3.png)

Válassza ki a biztonsági riasztás toolearn hello esemény, ha vannak ilyenek, lépéseket és mit, melyik hello riasztás kiváltó többet kell tootake tooremediate támadás. A biztonsági riasztások [típus](security-center-alerts-type.md) és dátum szerint vannak csoportosítva.

## <a name="monitor-security-health"></a>A biztonsági állapot figyelése
Miután engedélyezte a [biztonsági házirendek](security-center-policies.md) az előfizetéshez tartozó erőforrásokra, a Security Center elemzi az erőforrások tooidentify potenciális biztonsági réseket hello biztonságát.  Megtekintheti az erőforrások, valamint hello esetleg felmerülő problémákat hello biztonsági állapotát **erőforrás biztonsági állapota** panelen. Amikor rákattint **virtuális gépek** a hello **erőforrás biztonsági** állapota csempe, hello **virtuális gépek** panel nyílik meg a virtuális gépekre vonatkozó javaslatok. 

![Biztonsági állapot](./media/security-center-linux-virtual-machine/security-center-linux-virtual-machine-fig4.png)

Ha ez a javaslat kattint, látni fogja a hello bizonyos műveletek, amelyeket kapcsolatos további részletekért végrehajtott tooaddress ismertetünk. hello részletei jelennek meg hello hello panel alján a **javaslatok**. 

![Biztonsági állapot, 2](./media/security-center-linux-virtual-machine/security-center-linux-virtual-machine-fig5.png)


## <a name="see-also"></a>Lásd még:
További információ a Security Center toolearn hello következő lásd:

* [Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.
* [Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.

