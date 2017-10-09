---
title: "Gyakori kérdések (GYIK) a Security Center aaaAzure |} Microsoft Docs"
description: "Ez a GYIK az Azure Security Center kapcsolatos kérdésekre ad választ."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: cd0c0f8bdf15cdaf5889f2da5ac3cadf6017a9e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-frequently-asked-questions-faq"></a>Azure Security Center – gyakori kérdések
Ez a GYIK az Azure Security Center, egy szolgáltatás, amely segít a megakadályozása, észlelésében és kezelésében toothreats láthatóság növelésével és szabályozhatják, a Microsoft Azure-erőforrások biztonsági hello kapcsolatos kérdésekre ad választ.

> [!NOTE]
> Korai. június 2017 verziótól kezdve a Security Center használnak hello Microsoft Monitoring Agent toocollect, illetve adatainak tárolásához. több, lásd: toolearn [Azure Security Center Platform áttelepítési](security-center-platform-migration.md). a cikkben szereplő információkat hello Security Center funkció átmenet toohello Microsoft Monitoring Agent után jelöli.
>
>

## <a name="general-questions"></a>Általános kérdések
### <a name="what-is-azure-security-center"></a>Mi az az Azure Security Center?
Az Azure Security Center segítséget nyújt a megakadályozása, észlelésében és kezelésében toothreats láthatóság növelésével és az Azure-erőforrások hello biztonságát vezérelheti. Az ügyfél összes előfizetésére kiterjedő, integrált biztonsági monitorozást és szabályzatkezelést biztosít, megkönnyíti a nehezen észlelhető fenyegetések azonosítását, és számos biztonsági megoldással együttműködik.

### <a name="how-do-i-get-azure-security-center"></a>Hogyan szerezhetek az Azure Security Center?
Az Azure Security Center a Microsoft Azure-előfizetés engedélyezve van, és elérhető az hello [Azure-portálon](https://azure.microsoft.com/features/azure-portal/). ([Toohello portal bejelentkezés](https://portal.azure.com), jelölje be **Tallózás**, görgessen túl**Security Center**).  

## <a name="billing"></a>Számlázás
### <a name="how-does-billing-work-for-azure-security-center"></a>Hogyan működik az Azure Security Center számlázási tevékenységeket?
A Security Center érhető el, a két réteg:

Hello **ingyenes szint** hello biztonsági állapotát az Azure-erőforrások, az alapvető biztonsági házirend, a biztonsági javaslatok és a integrációs betekintést biztosít biztonsági termékeinek és szolgáltatásainak partnertől.

Hello **Standard csomagra** hozzáadja az advanced threat az észlelési képességek, beleértve az eszközintelligencia, viselkedéssel összefüggő elemzésekkel, anomáliadetektálás, biztonsági incidensek fenyegetés, és a fenyegetés szolgáló jelentések. hello Standard csomagra felszabadul hello az első 60 nap. Kell-e válasszon toocontinue toouse hello szolgáltatást 60 napon túl, automatikusan először toocharge hello szolgáltatás.  tooupgrade, hello válassza árképzési szintjében [biztonsági házirend](security-center-policies.md#set-security-policies). több, lásd: toolearn [Security Center árképzési](security-center-pricing.md).

## <a name="permissions"></a>Engedélyek
Az Azure Security Center által használt [szerepköralapú hozzáférés-vezérlést (RBAC)](../active-directory/role-based-access-control-configure.md), amely biztosítja [beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md) , amely a toousers, csoportok és az Azure rendelhetők.

A Security Center értékeli az erőforrások tooidentify biztonsági problémák és biztonsági rések hello konfigurációja. A biztonsági központban, csak akkor jelenik meg információ tooa erőforrás kapcsolatos hello szerepkör tulajdonos, közreműködő vagy olvasó hello előfizetés vagy az erőforrás csoport, amely egy erőforrás tartozik.

Lásd: [engedélyek az Azure Security Centerben](security-center-permissions.md) toolearn további szerepkörök és a Security Center engedélyezett műveletek.

## <a name="data-collection"></a>Adatgyűjtés
A Security Center adatokat gyűjti össze a virtuális gépek tooassess biztonsági állapotukra, adja meg a biztonsági javaslatok és riasztást küldjön, toothreats. A Security Center első megnyitásakor adatgyűjtés engedélyezett az előfizetésében szereplő összes virtuális gépet. Adatgyűjtés a Security Center házirend hello is engedélyezheti.

### <a name="how-do-i-disable-data-collection"></a>Hogyan tiltsa le az adatgyűjtő?
Ha hello Azure Security Center ingyenes csomagot használ, letilthatja a virtuális gépek bármikor adatok gyűjtése. Adatgyűjtés hello Standard csomagra előfizetések szükség. Biztonsági házirend hello előfizetésre vonatkozó adatok gyűjtésének letiltása ([Jelentkezzen be Azure-portálon toohello](https://portal.azure.com), jelölje be **Tallózás**, jelölje be **Security Center**, és válassza ki **házirend**.)  Amikor kiválaszt egy előfizetést, egy új panel megnyílik, és biztosít lehetőséget tooturn ki hello szövegrészt **adatgyűjtés**.

### <a name="how-do-i-enable-data-collection"></a>Hogyan engedélyezhető az adatok gyűjtését?
Adatgyűjtés engedélyezheti a biztonsági házirend hello Azure-előfizetése. tooenable adatgyűjtés. [Jelentkezzen be Azure-portálon toohello](https://portal.azure.com), jelölje be **Tallózás**, jelölje be **Security Center**, és válassza ki **házirend**. Állítsa be **adatgyűjtés** túl**a**.

### <a name="what-happens-when-data-collection-is-enabled"></a>Mi történik, ha engedélyezve van az adatok gyűjtését?
Adatgyűjtés engedélyezésekor a rendszer automatikusan létrehozza a Microsoft Monitoring Agent hello rendszer összes meglévő, és új telepített virtuális gépek támogatott hello előfizetésben.

### <a name="does-hello-monitoring-agent-impact-hello-performance-of-my-servers"></a>Hello Figyelőügynök hatás hello a kiszolgáló teljesítményét használ?
hello ügynök névleges mennyisége rendszererőforrásokat fogyaszt, és kell jelentősek hello teljesítményét. A teljesítményre gyakorolt hatás és a hello ügynök és a bővítmény további információkért lásd: hello [tervezési és műveletek útmutató](security-center-planning-and-operations-guide.md#data-collection-and-storage).

### <a name="where-is-my-data-stored"></a>Hol tárolják az adataimat?
Ez az ügynök gyűjtött adatokat vagy egy meglévő Naplóelemzési munkaterület az Ön előfizetéséhez rendelve, vagy új munkaterület tárolja. További információkért lásd: [adatbiztonság](security-center-data-security.md).

## <a name="using-azure-security-center"></a>Az Azure Security Centerben
### <a name="what-is-a-security-policy"></a>Mi az az olyan biztonsági házirenddel?
A biztonsági házirend vezérlőelemek megadott hello erőforrások ajánljuk, hogy hello csoportját határozza meg előfizetés. Az Azure Security Centerben, szabályzatok készítése az Azure-előfizetések tooyour vállalati biztonsági követelmények és hello szereplő alkalmazások típusának vagy az egyes előfizetések hello adatok érzékenységének megfelelően.

hello biztonsági házirend engedélyezve van. az Azure Security Center meghajtó biztonsági javaslatok és ellenőrzésére. További információ a biztonsági házirendek toolearn lásd [biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md).

### <a name="who-can-modify-a-security-policy"></a>Ki módosíthatja a biztonsági házirend?
a biztonsági házirend toomodify a biztonsági rendszergazda vagy a tulajdonosának vagy Közreműködőjének előfizetésben kell lennie.

Hogyan tooconfigure egy biztonsági házirend: toolearn [biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md).

### <a name="what-is-a-security-recommendation"></a>Mi az a biztonsági ajánlás olyan környezetekben?
Az Azure Security Center elemzi az Azure-erőforrások hello biztonsági állapotát. Amikor a potenciális biztonsági hiányosságokat azonosít, javaslatok jönnek létre. hello javaslatok útmutató le a hello folyamatán hello vezérlő szükséges. Például a következők:

* Kártevőirtó toohelp kiépítése azonosítása és eltávolítani a kártevő szoftvereket
* Konfigurálás [hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md) és toocontrol forgalom toovirtual gépek szabálya
* A webes alkalmazások célzó támadások elleni védelemre egy webes alkalmazás tűzfal toohelp kiépítése
* Hiányzó rendszerfrissítések telepítése
* Operációs rendszer azon konfigurációit, amelyek nem felelnek meg a hello címzési ajánlott alaptervek

Itt csak azok a javaslatok, amelyek engedélyezve vannak a biztonsági házirend látható.

### <a name="how-can-i-see-hello-current-security-state-of-my-azure-resources"></a>Honnan látom hello aktuális biztonsági állapotát az Azure-erőforrások?
Hello **Security Center áttekintése** panelen látható hello a számítási, hálózati, tárolási és adatok és alkalmazások szerinti bontásban környezet általános biztonsági állapotát. Minden erőforrás rendelkezik egy kijelző ábrázoló Ha azonosított potenciális biztonsági hiányosságok. Mindegyik mozaiknál kattintva megjelenik a biztonsági problémák és hello erőforrást az előfizetésében leltárt Security Center által azonosított listáját.

### <a name="what-triggers-a-security-alert"></a>Mi elindítja a biztonsági riasztások?
Az Azure Security Center automatikusan gyűjti, elemzi és biztosítók az Azure-erőforrások, a hello hálózati és a partneri megoldások, például kártevőirtó- és tűzfalak naplóadatait. Fenyegetések észlelése esetén a központ biztonsági riasztást hoz létre. Példák fenyegetés észlelésére:

* Feltört virtuális gépek, amelyek kártékonyként azonosított IP-címekkel kommunikálnak
* Windows hibajelentés észlelt speciális kártevő
* Virtuális gépek elleni, a teljes kipróbálás módszerén alapuló támadások
* Integrált biztonsági partnermegoldások például kártevőirtó vagy webalkalmazási tűzfalak biztonsági riasztásai

### <a name="whats-hello-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>Mi az a fenyegetéseket észlelt, és riasztást kap a Microsoft Security Response Center és az Azure Security Center által hello különbségének?
hello Microsoft biztonsági válasz Center (MSRC) hajtja végre, válassza ki a biztonság ellenőrzése hello Azure-hálózatot és az infrastruktúra, és fenyegetést eszközintelligencia és visszaélés panaszokat kapott harmadik felek számára. MSRC válik, hogy ügyféladatok fért lett egy törvénybe ütköző vagy jogosulatlan félnek vagy Azure adott hello ügyfél általi használata nem felel meg hello feltételek elfogadható használjanak, amikor egy biztonsági incidens manager értesíti a hello ügyfél. Értesítési rendszerint azért fordul elő, ha nincs megadva a biztonsági lépjen kapcsolatba az Azure Security Center vagy hello Azure-előfizetés tulajdonosának megadott kapcsolattartókat egy e-mailek toohello biztonsági elküldésével..

A Security Center, az Azure-szolgáltatások, amelyek folyamatosan figyeli a hello ügyfél Azure-alapú környezetben és analytics tooautomatically vonatkozik számos különböző esetlegesen kártékony tevékenység észleli. Ezek az észlelések hello Security Center irányítópultjának biztonsági riasztásként illesztett.

### <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Mely Azure-erőforrások az Azure Security Center által figyelt?
Az Azure Security Center figyelők hello Azure-erőforrások a következő:

* Virtuális gépek (VM) (beleértve a [Felhőszolgáltatások](../cloud-services/cloud-services-choose-me.md))
* Azure virtuális hálózatok
* Az Azure SQL-szolgáltatás
* Azure Storage-fiók
* Az Azure Web Apps (a [App Service Environment-környezet](../app-service/app-service-app-service-environments-readme.md))
* Webalkalmazási tűzfal virtuális gépeken és a például az Azure-előfizetésében integrált partnermegoldások [App Service Environment-környezet](../app-service/app-service-app-service-environments-readme.md)

## <a name="virtual-machines"></a>Virtuális gépek
### <a name="what-types-of-virtual-machines-are-supported"></a>Milyen típusú virtuális gépek támogatottak?
Figyelés és javaslatok érhetők el a virtuális gépek (VM) létrehozott mindkét hello [klasszikus és Resource Manager üzembe helyezési modell](../azure-classic-rm.md).

Lásd: [az Azure Security Center által támogatott platformok](security-center-os-coverage.md) a támogatott platformok listáját.

### <a name="why-doesnt-azure-security-center-recognize-hello-antimalware-solution-running-on-my-azure-vm"></a>Miért nem az Azure Security Center ismeri fel az Azure virtuális gépen hello kártevőirtó megoldás?
Az Azure Security Center rendelkezik Azure-bővítményeket keresztül telepített kártevőirtó láthatósága. Például a Security Center nem tudja toodetect kártevőirtó, de a előre telepítve van a megadott lemezkép, vagy ha a saját folyamatok (például a konfigurációs felügyeleti rendszerekhez) használata a virtuális gépek telepített kártevőirtó.

### <a name="why-do-i-get-hello-message-missing-scan-data-for-my-vm"></a>Miért kapok üdvözlőüzenetére "Hiányzó vizsgálati adatok" a virtuális géphez?
Ez az üzenet akkor jelenik meg, amikor nincs a virtuális gépek vizsgálat adat. Adatgyűjtés engedélyezése az Azure Security Centerben után vizsgálati adatok toopopulate némi időbe (kevesebb mint egy óra) is igénybe vehet. Után ellenőrző adatok sokaságát kezdeti hello ezt az üzenetet, mert nincs minden vizsgálat adat, vagy nincs legutóbbi vizsgálat adat jelenhet meg. Vizsgálat nem sikerült adatokkal feltölteni a virtuális gép leállított állapotban. Ezt az üzenetet is jelennek meg, ha az ellenőrző adatok nem nemrég (hello hello Windows-ügynök, amely alapértelmezett értéke 30 napos adatmegőrzési) megfelelően van feltöltve.

### <a name="why-do-i-get-hello-message-vm-agent-is-missing"></a>Miért jelenik meg üdvözlőüzenetére "Virtuálisgép-ügynök hiányzó?"
virtuális gépek tooenable adatgyűjtés hello ügynököt telepíteni kell. Virtuálisgép-ügynök hello alapértelmezés szerint telepítve van a hello Azure Piactérről származó központilag telepített virtuális gépekhez. Hogyan tooinstall hello Virtuálisgép-ügynök más virtuális gépeken a további információkért lásd: hello blogbejegyzésben [ügynök és Virtuálisgép-bővítmények](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/).
