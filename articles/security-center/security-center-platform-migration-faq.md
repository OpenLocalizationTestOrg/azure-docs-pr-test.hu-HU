---
title: "aaaSecurity Center platform áttelepítési – gyakori kérdések |} Microsoft Docs"
description: "Ez a GYIK hello Azure biztonsági központ platform áttelepítési kapcsolatos kérdésekre ad választ."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 4d1364cd-7847-425a-bb3a-722cb0779f78
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: terrylan
ms.openlocfilehash: fcb14ae83167ef79a60371e4fcb625cf99bee6c9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="security-center-platform-migration-faq"></a>A Security Center platform áttelepítési – gyakori kérdések
Korai. június 2017 az Azure Security Center megkezdte hello Microsoft Monitoring Agent toocollect és a tároló adatok felhasználásával. több, lásd: toolearn [Azure Security Center Platform áttelepítési](security-center-platform-migration.md). Ez a GYIK hello platform áttelepítésével kapcsolatos kérdésekre ad választ.

## <a name="data-collection-agents-and-workspaces"></a>Adatok gyűjtése, az ügynökök és a munkaterületek

### <a name="how-is-data-collected"></a>Hogyan gyűjti az adatokat?
A Security Center hello Microsoft Monitoring Agent toocollect biztonsági adatait használja a virtuális gépek. hello biztonsági adatok biztonsági konfigurációkat, amelyek használt tooidentify biztonsági rések, és a biztonsági események, amelyek használt toodetect fenyegetésekkel kapcsolatos adatokat tartalmaz. Hello ügynök által gyűjtött adatok egy meglévő Naplóelemzési munkaterület csatlakoztatott toohello VM vagy a Security Center által létrehozott új munkaterület tárolja. Biztonsági központ létrehoz egy új munkaterületet, ha hello földrajzi hely meghatározásának hello a virtuális gép figyelembe venni.

> [!NOTE]
> a Microsoft Monitoring Agent hello hello ugyanannak az ügynöknek használt hello Operations Management Suite (OMS), Naplóelemzés szolgáltatás és System Center Operations Manager (SCOM).
>
>

Amikor először, vagy az előfizetések áttelepítésekor adatgyűjtés hello engedélyezett, az a Security Center ellenőrzi, hogy hello Microsoft Monitoring Agent már telepítve van egy Azure-bővítményt a virtuális gépek minden egyes toosee. Ha nincs telepítve a Microsoft Monitoring Agent hello, majd a Security Center lesz:

- a virtuális gép hello hello Microsoft Monitoring agent telepítése
   - Ha a Security Center által már létrehozott egy munkaterület megtalálható az azonos földrajzi hely, a virtuális gép, hello hello hello ügynök-e a csatlakoztatott toothis munkaterület
   - Ha a munkaterületet nem létezik, a Security Center hoz létre egy új erőforráscsoportot alapértelmezett adott földrajzi hely meghatározásának munkaterületén és hello ügynök toothat munkaterület csatlakozzon. hello elnevezési hello munkaterületet, és az erőforrás-csoport a következők:

       Munkaterület: DefaultWorkspace-[előfizetés-azonosító]-[földrajzi]

       Erőforráscsoport: DefaultResouceGroup-[földrajzi]
- a Security Center megoldás telepítése hello munkaterület

hello hely hello munkaterület hello VM hello helyét alapul. több, lásd: toolearn [adatbiztonság](security-center-data-security.md).

> [!NOTE]
> Előzetes tooplatform áttelepítési, a Security Center gyűjtött biztonsági adatokat a virtuális gépek hello Azure Figyelőügynök használatával, és a tárolási a tárfiók. Hello platformhoz az áttelepítés után a Security Center hello Microsoft Monitoring Agent használ, és munkaterület toocollect és a tároló hello ugyanazokat az adatokat. hello tárfiók hello áttelepítés után eltávolítható.
>
>

### <a name="am-i-billed-for-log-analytics-or-oms-on-hello-workspaces-created-by-security-center"></a>Vagyok I Naplóelemzési vagy a Security Center által létrehozott hello munkaterületek OMS számlázása?
Nem. Security Center által létrehozott munkaterületek közben egy csomópont számlázási OMS konfigurált nem számítunk OMS díjakat. A Security Center számlázási mindig alapján a Security Center biztonsági házirend és hello megoldások munkaterület telepítve:

- **Ingyenes szint** – a Security Center hello "SecurityCenterFree" megoldás telepít hello alapértelmezett munkaterületen. Az ingyenes szint hello nem kell fizetni.
- **Standard szint** – a Security Center telepíti a "SecurityCenterFree" hello, és "Security" megoldások a hello alapértelmezett munkaterületen.

Az árakkal kapcsolatos további információkért lásd: [Security Center árképzési](https://azure.microsoft.com/pricing/details/security-center/). lap címek árképzési hello toosecurity adattárolás és arányosított számlázási kezdve. június 2017 változik.

> [!NOTE]
> a Security Center által létrehozott munkaterületek hello OMS tarifacsomag nem érinti a Security Center számlázási.
>
>

### <a name="can-i-delete-hello-default-workspaces-created-by-security-center"></a>Törölheti a Security Center által létrehozott hello alapértelmezett munkaterületek?
**Hello alapértelmezett munkaterület törlése nem ajánlott.** A Security Center hello alapértelmezett munkaterületek toostore biztonsági adatait használja a virtuális gépek.  Ha a munkaterületet, a Security Center törli nem toocollect az adatokat, és egyes biztonsági javaslatok és értesítések nem érhetők el.

toorecover, hello virtuális gépek törölt csatlakoztatott toohello munkaterületen a Microsoft Monitoring Agent eltávolítása hello. A Security Center újratelepíti hello ügynök, és új alapértelmezett munkaterületek hoz létre.

### <a name="what-if-hello-microsoft-monitoring-agent-was-already-installed-as-an-extension-on-hello-vm"></a>Mi történik, ha a Microsoft Monitoring Agent hello már telepítve van a virtuális gép hello bővítményként?
A Security Center nem bírálja felül a meglévő kapcsolatok toouser munkaterületek. A Security Center tárolók biztonsági adatait hello hello munkaterületen virtuális gép már csatlakoztatva.

### <a name="what-if-i-had-a-microsoft-monitoring-agent-installed-on-hello-machine-but-not-as-an-extension"></a>Mi történik, ha korábban a Microsoft Monitoring Agent hello számítógépen, de nem bővítményként telepítve?
Ha Microsoft-Figyelőügynök hello közvetlenül telepítve van-e hello virtuális gép (nem pedig egy Azure-bővítményt), a Security Center nem telepíti a Microsoft Monitoring Agent hello, és biztonsági figyelés lesz korlátozva.

### <a name="what-is-hello-impact-of-removing-these-extensions"></a>Mi az, hogy ezek a bővítmények eltávolításának következményei hello?
Hello Microsoft Figyelőbővítmény eltávolítja, ha a Security Center nincs hello méretű képes toocollect biztonsági adatait, és egyes biztonsági javaslatokra és riasztásokra nem érhetők el. 24 órán belül a Security Center hello VM hello bővítmény hiányzik, és újratelepíti hello bővítmény határozza meg.

### <a name="how-do-i-stop-hello-automatic-agent-installation-and-workspace-creation"></a>Hogyan akadályozhatom meg hello automatikus ügynök telepítésére és a munkaterület létrehozását?
Adatgyűjtés az előfizetésekhez hello biztonsági házirendben kikapcsolható, de ez nem ajánlott. Adatok gyűjtése korlátok Security Center javaslatait és riasztások kikapcsolása. Adatgyűjtés hello a Standard tarifacsomag előfizetések szükség. toodisable adatok gyűjtése:

1. Ha az előfizetés hello Standard csomag van konfigurálva, nyissa meg az adott előfizetéshez tartozó hello biztonsági házirend, és válassza ki a hello **szabad** réteg.

   ![Tarifacsomag][1]

2. A következő adatok gyűjtésének kikapcsolása kiválasztásával **ki** a hello **biztonsági szabályzat – adatgyűjtés** panelen.

   ![Adatgyűjtés][2]

### <a name="how-do-i-remove-oms-extensions-installed-by-security-center"></a>Hogyan távolítsa el a Security Center által telepített OMS-kiterjesztéseket?
Távolítsa el a Microsoft Monitoring Agent hello manuálisan. Ez nem ajánlott, mivel a Security Center javaslatait és riasztások korlátozza.

> [!NOTE]
> Adatgyűjtés engedélyezése esetén a Security Center az eltávolítást követően újra a hello ügynök.  Adatgyűjtés toodisable hello ügynök manuális eltávolítása előtt van szüksége. Lásd: [hogyan akadályozható meg hello automatikus ügynök telepítése és a munkaterület létrehozását?](#how-do-i-stop-the-automatic-agent-installation-and-workspace-creation?) letiltásáról adatgyűjtés.
>
>

toomanually hello ügynök eltávolítása:

1.  Hello portálon, nyissa meg a **Naplóelemzési**.
2.  Hello Naplóelemzési panelen a munkaterület kiválasztása:
3.  Válassza ki, hogy nem szeretné, hogy toomonitor és válassza a virtuális gépek **Disconnect**.

   ![Hello ügynök eltávolítása][3]

> [!NOTE]
> Ha a Linux virtuális gép már nem bővítmény OMS-ügynököt, hello bővítmény eltávolítása, valamint a hello ügynök és hello ügyfélnek van tooreinstall azt.
>
>

## <a name="existing-oms-customers"></a>Meglévő OMS-ügyfeleknek

### <a name="does-security-center-override-any-existing-connections-between-vms-and-workspaces"></a>Bírálja felül a meglévő kapcsolatokat, virtuális gépek és a munkaterületek között a Security Center?
Ha egy virtuális gép már hello Azure bővítményként telepített Microsoft Monitoring Agent, a Security Center nem bírálja felül hello meglévő munkaterület kapcsolat. Ehelyett a Security Center hello meglévő munkaterületen használja.

A Security Center megoldás hello munkaterület van telepítve, ha nem már létezik, és hello megoldásban alkalmazott csak toohello megfelelő virtuális gépek. Ha hozzáad egy megoldást, a alapértelmezett tooall Windows és Linux ügynökök csatlakoztatott tooyour Naplóelemzési munkaterület automatikusan telepíti. [Megoldás célcsoportkezelést](../operations-management-suite/operations-management-suite-solution-targeting.md), amely az OMS szolgáltatása, lehetővé teszi a hatókör tooapply tooyour megoldásokat.

Hello Microsoft Monitoring Agent telepítése közvetlenül hello virtuális gép (nem pedig egy Azure-bővítményt), a Security Center nem telepíti a Microsoft Monitoring Agent hello, majd biztonsági figyelés korlátozva.

### <a name="what-should-i-do-if-i-suspect-that-hello-data-platform-migration-broke-hello-connection-between-one-of-my-vms-and-my-workspace"></a>Mit tegyek, ha valaki azt gyanítja, hogy hello platform adatáttelepítés túllépte hello kapcsolatot a virtuális gépek és a saját munkaterület egyik?
Ez nem következik. Ha akkor fordulhat elő, majd [hozzon létre egy Azure támogatási kérést](../azure-supportability/how-to-create-azure-support-request.md) , és tartalmazzák a következő adatok hello:

- az Azure erőforrás-azonosítója hello hello érintett VM
- az Azure erőforrás-azonosító hello hello munkaterület konfigurált hello kiterjesztése előtt hello kapcsolat megszakadt.
- hello ügynök és a korábban telepített verzió

### <a name="does-security-center-install-solutions-on-my-existing-oms-workspaces-what-are-hello-billing-implications"></a>Biztonsági központ telepítési megoldások a saját meglévő OMS munkaterületek? Mik azok a hello számlázási implications?
Amikor a Security Center azonosítja, hogy a virtuális gép már csatlakoztatott tooa munkaterületet hozott létre, a Security Center lehetővé teszi, hogy a megfelelő IP-címek tooyour munkaterület megoldások. hello megoldások keresztül megfelelő Azure virtuális gépek vannak a alkalmazott csak toohello [célcsoport-kezelési megoldás](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solution-targeting), így a hello számlázási marad hello azonos.

- **Ingyenes szint** – a Security Center hello "SecurityCenterFree" megoldás telepít hello munkaterületen. Az ingyenes szint hello nem kell fizetni.
- **Standard szint** – a Security Center telepíti a "SecurityCenterFree" hello, és "Security" megoldások a hello munkaterületen.

   ![Az alapértelmezett munkaterületi megoldások][4]

> [!NOTE]
> hello Naplóelemzési "Security" megoldás biztonsági hello & naplózási megoldás az OMS Szolgáltatáshoz.
>
>

### <a name="i-already-have-workspaces-in-my-environment-can-i-use-them-toocollect-security-data"></a>A környezet már van munkaterületek, használhatók toocollect biztonsági adatokat?
Ha egy virtuális gép már hello Azure bővítményként telepített Microsoft Monitoring Agent, a Security Center hello meglévő csatlakoztatott munkaterületen használja. A Security Center megoldás hello munkaterület van telepítve, ha nem már létezik, és hello megoldás alkalmazott csak toohello keresztül kapcsolódó virtuális gépek [célcsoport-kezelési megoldás](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solution-targeting).

Amikor a Security Center hello Microsoft Monitoring Agent telepítése virtuális gépeken, az hello alapértelmezett munkaterületek Security Center által létrehozott. Hamarosan ügyfelek is képes tooconfigure mely munkaterületek szolgálnak.

### <a name="i-already-have-security-solution-on-my-workspaces-what-are-hello-billing-implications"></a>Már van a saját munkaterületek biztonsági megoldás. Mik azok a hello számlázási implications?
Biztonsági és naplózási megoldás hello Azure virtuális gépeken használt tooenable biztonsági központ szabványos réteg szolgáltatások. Hello biztonsági & naplózási megoldás egy munkaterület már telepítve van, ha a Security Center hello meglévő megoldást használ. Nincs változás a számlázási.

## <a name="next-steps"></a>Következő lépések
toolearn hello Security Center platform áttelepítési bővebben lásd:

- [Az Azure Security Center Platform áttelepítése](security-center-platform-migration.md)
- [Az Azure Security Center hibaelhárítási útmutatója](security-center-troubleshooting-guide.md)

<!--Image references-->
[1]: ./media/security-center-platform-migration-faq/pricing-tier.png
[2]: ./media/security-center-platform-migration-faq/data-collection.png
[3]: ./media/security-center-platform-migration-faq/remove-the-agent.png
[4]: ./media/security-center-platform-migration-faq/solutions.png
