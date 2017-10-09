---
title: "aaaProtect személyes adatokat az Azure Security Center |} Microsoft Docs"
description: "az Azure security Centerben személyes adatok védelme"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 8e2c893d13318392f47fa912089d52618f9e7b45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="protect-personal-data-from-breaches-and-attacks-azure-security-center"></a>Személyes adatok védelmét a problémák és támadások: az Azure Security Center

Ez a cikk segít megérteni, hogyan feltöri a toouse az Azure Security Center tooprotect személyes adatait, és támadásokat.

## <a name="scenario"></a>Forgatókönyv 

A nagy körutazás vállalat, az Amerikai Egyesült Államokban, hello telephelyének bővíti a műveletek toooffer útvonalak hello mediterrán, és Balti tengerek, valamint hello Brit-szigetekre. az adott erőfeszítéseket toohelp, szerzett több kisebb körutazás sorok Olaszország, német, Dánia és hello brit

hello vállalati hello felhő Microsoft Azure toostore vállalati adatait használja. Ez magában foglalja a személyes azonosításra alkalmas adatok, például a nevét, a címét, a telefonszámokat és a hitelkártya adatait. Az adatokat is tartalmaz az emberi erőforrások, mint:

- Címek
- telefonszámok
- azonosító számokat
- orvosi adatok

hello körutazás sor is fenntartja, nagy adatbázis ellenszolgáltatás és hűség program tagjainak. Vállalati alkalmazottak hozzáférési hello hálózati hello vállalati távoli iroda és utazás ügynökök található hello világ rendelkezik toosome vállalati erőforrások eléréséhez.
Személyes adatok között ezeket a helyeket és hello Microsoft adatközpont hello hálózaton áthaladó.

## <a name="problem-statement"></a>Probléma leírása

hello vállalat aggódik az Azure-erőforrások elleni támadások veszélye hello. Kívánják-ügyfelek és az alkalmazottak a személyes adatok toounauthorized személyek tooprevent kapta. Szeretnék egyaránt megelőzése és a válasz/szervizelés, valamint egy hatékony módot toomonitor hello a biztonság folyamatos a felhőalapú erőforrások útmutatást.
Egy erős elleni védelmi vonalat mai valamint támadók van szükségük.

## <a name="company-goal"></a>Vállalati cél

Hello vállalati célok egyik tooensure hello adatvédelmi ügyfelek és az alkalmazottak a személyes adatok védelmével fenyegetésekkel szembeni hatékony által. A cél egyik toorespond azonnal a biztonsági szabályok megsértésére toomitigate toosigns hello hatása. Tooassess hello biztonsági aktuális állapotát úgy van szükség, sebezhető felismerésében, és elháríthatja a.

## <a name="solutions"></a>Megoldások

A Microsoft Azure Security Center (ASC) az integrált biztonsági monitorozást és felügyeleti megoldást nyújt. Könnyen használható és hatékony megelőzését, észlelését és ellenállási képességeket kínál biztosítja.

### <a name="prevention"></a>Megelőzés

ASC segítségével megakadályozása megsértése azáltal, hogy engedélyezi tooset biztonsági házirendek, közvetlenül az időponthoz kötött hozzáférést biztosítanak, és biztonsági ajánlások megvalósításával.

A biztonsági házirend ajánlott a megadott hello erőforrások vezérlők hello csoportját határozza meg előfizetés. Igény szerint hozzáférést lehet használt toolock le a bejövő forgalom tooyour Azure virtuális gépeken, tooattacks veszélyeztetettségének csökkentése. Biztonsági javaslatok az Azure-erőforrások biztonsági állapotának hello elemzése után ASC hozza létre.

#### <a name="how-do-i-set-security-policies-in-asc"></a>Biztonsági házirendek beállítása a ASC

Az egyes előfizetésekhez külön-külön biztonsági szabályzatot állíthat be. a biztonsági házirend toomodify tulajdonosának vagy közreműködőjének előfizetésben kell lennie. Az hello hello Azure-portálon, a következő:

1. Válassza ki **házirend** hello ASC irányítópulton.

2. Válassza ki kívánja tooenable hello házirend hello előfizetést.

3. Válasszon **megakadályozási szabályzat** előfizetésenként tooconfigure házirendek. **Adatgyűjtés a virtuális gépek** túl meg kell**a.**

4. A hello **megakadályozási szabályzat** beállításnál válassza **a** tooenable hello biztonsági javaslatok szempontjából releváns hello előfizetés.

![](media/protect-personal-data-azure-security-center/prevention-policy.png)

Részletesebb utasításokat és az előzetesben hello házirend ajánlások engedélyezhető, lásd: [biztonsági házirendek beállítása az Azure Security Centerben](https://docs.microsoft.com/azure/security-center/security-center-policies#set-security-policies).

#### <a name="how-do-i-configure-just-in-time-access-jit"></a>Hogyan konfigurálhatók a csak az idő Access (JIT)?

Igény szerinti engedélyezésekor a rendszer a Security Center zárolja bejövő forgalom tooyour Azure virtuális gépek egy NSG-szabály létrehozásával. Kiválaszthatja hello hello VM toowhich a bejövő forgalom lesz zárolva. igény szerinti toouse érni, a következő hello:

1. Jelölje be hello **idő VM hozzáférés csempén csak** hello ASC panelen.

2. Jelölje be hello **ajánlott** fülre.

3. A **virtuális gépek**, válassza ki a megjeleníteni kívánt tooenable hello virtuális gépeket. A pipa következő tooa VM ez helyezi. 
4. Válassza ki **JIT engedélyezése** virtuális gépeken.
5. Kattintson a **Mentés** gombra.

Láthatja majd ASC javasolja a rendszer engedélyezi az igény szerinti hello alapértelmezett portokat. Is hozzáadhat, és amikor azt szeretné csak az idő-megoldások tooenable hello új port konfigurálása. Hello **közvetlenül a virtuális gép elérhető** csempe hello Security Center az igény szerinti hozzáférési konfigurált virtuális géppel hello számát jeleníti meg. Is hello engedélyezett hozzáférési kérelmek száma a hello múlt héten jeleníti meg.

![](media/protect-personal-data-azure-security-center/jit-vm.png)

Útmutatást toodo, és csak további információt a időben való hozzáférésre, lásd: [JIT-virtuális gép hozzáférés kezelése.](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)

#### <a name="how-do-i-implement-asc-security-recommendations"></a>Hogyan valósítja meg a biztonsági javaslatok ASC?

A Security Center javaslatokat hoz létre, amikor lehetséges biztonsági réseket észlel. hello javaslatok végigvezetik hello hello szükséges vezérlők konfigurálásának lépésein. 
1. Jelölje be hello **javaslatok** hello ASC irányítópult csempét.

2. Nézet hello javaslatok, amelyben soronként jelöli egy javaslat táblázatos formában látható.

3. toofilter ajánlásokat, válassza ki **szűrő** és toosee kívánja hello súlyossági és állapot értékek.

4. azt ajánljuk, hogy nincs megfelelő toodismiss, is kattintson a jobb gombbal és **leállítási.**

5. Értékelje ki, melyik javaslat előbb alkalmazni kell.

6. Hello javaslatok a prioritásuk szerinti sorrendben alkalmazza.

Lehetséges követelményekkel, és hogyan útmutatók listája tooapply minden, lásd: [biztonsági javaslatok kezelése az Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-recommendations)

### <a name="detection-and-response"></a>Felderítését és a válasz

Felderítését és a válasz Ugrás együtt, ahányat csak szeretne toorespond minél gyorsabban után fenyegetést észlel.
ASC fenyegetésészlelés működik, automatikusan az Azure-erőforrások, hello és hálózati összekapcsolt partnermegoldások a biztonsági információk begyűjtése. ASC is gyorsan frissíti az észlelési algoritmusokat támadók kiadás új és egyre kifinomult biztonsági réseket. További információk a ASC tartozó fenyegetésészlelés működése, a következő témakörben: [az Azure Security Center az észlelési képességek](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities).

#### <a name="how-do-i-manage-and-respond-toosecurity-alerts"></a>Hogyan kezelheti és toosecurity riasztások válaszolni?

A rangsorolt biztonsági riasztások listája látható a Security Center együtt hello tooquickly szükséges információk, vizsgálja meg hello problémát. A Security Center is módjára vonatkozó javaslatokkal tooremediate támadás. a biztonsági riasztások, hajtsa végre a következő hello toomanage:

1. Jelölje be hello **biztonsági riasztások** hello ASC irányítópult csempére. Ez azt jelenti, az egyes riasztások részletei.

2. Válassza ki a toofilter riasztások dátum, állapot és súlyosság alapján **szűrő** , és válassza a kívánt toosee hello értékek.

3. toorespond tooan riasztásra, válassza ki azt hello tekintse át, majd válassza ki a támadásnak kitett erőforrásra hello erőforrás.

4. A hello **leírás** mezőben adatait, többek között az ajánlott javítási láthatja.

![](media/protect-personal-data-azure-security-center/security-alerts.png)

Részletes utasítások válaszol toosecurity riasztások, lásd: [kezelése és válaszol toosecurity az Azure Security Center riasztást küld.](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)

További segítségért a biztonsági riasztások kivizsgálásának hello vállalati ASC riasztások integrálhatja a saját SIEM-megoldás használatával [Azure napló integrációs](https://aka.ms/AzLog).

#### <a name="how-do-i-manage-security-incidents"></a>Hogyan kezelheti a biztonsági események?

A ASC egy biztonsági incidens az összes riasztás erőforrás, amely megfelel-e a kill lánc minták összesítést. Incidens jelenítenek meg hello kapcsolódó riasztások listája, amely lehetővé teszi, hogy tooobtain további információ a minden egyes előfordulásakor. Incidensek megjelennek a biztonsági riasztások csempe hello és panelen.

tooreview és biztonsági incidensek kezeléséhez, a következő hello:

1. Jelölje be hello **biztonsági riasztások** csempére. Ha egy biztonsági incidens észlel, akkor jelenik meg hello biztonsági riasztások graph alatt. Egy ikont, amely eltér a más riasztások lesz.

2. Válassza ki a hello incidens toosee további információt a biztonsági incidens. További részletek közé tartoznak a teljes leírását, a súlyosság, a jelenlegi állapotában, megtámadott hello erőforrás, hello javítási lépéseket hello incidens, és ezt az incidenst foglalt riasztások hello.

Szűrheti a toosee **incidensek csak**, **riasztásokat csak**, vagy **mindkét**.

#### <a name="how-do-i-access-hello-threat-intelligence-report"></a>Hogyan férhetek hello fenyegetés az Eszközintelligencia-jelentés?

ASC elemzi az adatokat több forrásból tooidentify fenyegetésekkel szembeni hatékony. tooassist incidensválasz-csoportok vizsgálja meg, és szervizelheti azokat a fenyegetéseket, a Security Center tartalmaz, amely leírja hello fenyegetést észlelt fenyegetés eszközintelligencia jelentést.

A Security Center háromféle fenyegetés jelentéseit, amelyek / támadás eltérőek lehetnek.
hello elérhető jelentéseket a következők:

- Tevékenységcsoport a jelentés: támadó, a célok és taktikai mély dives tartalmazza.

- Kampány jelentés: adott támadás kampányok részleteit összpontosít.

- Fenyegetés összesítő jelentés: hello előző két jelentés az összes elemet tartalmazza.

Az ilyen információkat nagyon hasznos hello incidensválasz-folyamata során, ahol egy folyamatban lévő vizsgálat toounderstand hello forrás hello támadások, hello támadó célokat, és milyen toodo toomitigate áthelyezése előre ez adja ki.

1. tooaccess hello fenyegetésfelderítési adataival jelentésből, és a következő hello:

2. Jelölje be hello **biztonsági riasztások** hello ASC irányítópult csempét.

3. Jelölje ki a kívánt tooobtain hello biztonsági riasztást további információt.

4. A hello **jelentések** , majd kattintson a hello hivatkozás toohello fenyegetés eszközintelligencia jelentés.

5. Ekkor megnyílik hello PDF-fájl, amely letölthető.

![](media/protect-personal-data-azure-security-center/security-alerts-suspicious-process.png)

További információ a hello ASC fenyegetés az eszközintelligencia-jelentés: [Azure Security Center fenyegetés Eszközintelligencia jelentés.](https://docs.microsoft.com/azure/security-center/security-center-threat-report)

### <a name="assessment"></a>Értékelés

toohelp tesztelése, felmérési és a biztonságot értékelése, ASC biztosít integrált biztonsági réseinek értékelése Qualys a felhő ügynökök, a virtuális gép javaslatok összetevő részeként.

hello Qualys ügynök jelentéseket küld a biztonsági adatok toohello Qualys felügyeleti platform, amely ezután elküldi a biztonsági rés és állapotfigyelési adatokat biztonsági tooASC. a biztonsági rés értékelése megoldás megjelenik a hello ajánlás tooadd hello **javaslatok** hello ASC irányítópult paneljét.

Hello biztonsági rés értékelési megoldás hello cél virtuális gép telepítését követően a Security Center vizsgálatok VM toodetect hello, és a rendszer- és biztonsági rések azonosítása. Észlelt problémák hello alatt látható **virtuális gépek javaslatok** lehetőséget.

#### <a name="how-do-i-implement-a-vulnerability-assessment-solution"></a>Hogyan valósítja meg a biztonsági rés értékelési megoldás? 

Ha egy virtuális gép nem rendelkezik az integrált biztonsági rés értékelési megoldás már üzembe van helyezve, a Security Center azt javasolja, hogy telepíteni kell.

1. Hello ASC irányítópulton hello **javaslatok** panelen válassza **biztonsági rés értékelése megoldás hozzáadása.**

2. Válassza ki a kívánt tooinstall hello biztonsági rés értékelési megoldás hello virtuális gépeket.

3. Kattintson a **telepíthető [száma] virtuális gépeket.**

4. Válassza ki egy partneri megoldást hello Azure piactérről, illetve a **meglévő megoldással** válasszon **Qualys.**

5. Hello automatikus frissítési beállítások ki- és kikapcsolása a hello kapcsolhatja **partneri megoldások** panelen.

További útmutatást tooimplement egy biztonsági rés megoldással, lásd: [biztonsági réseinek értékelése az Azure Security Centerben.](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations)

## <a name="next-steps"></a>Következő lépések

- [Az Azure Security Center bemutatása](https://docs.microsoft.com/azure/security-center/security-center-get-started)

- [A Security Center bemutatása tooAzure](https://docs.microsoft.com/azure/security-center/security-center-intro)

- [Azure Security Center riasztásait integrálása az Azure naplóelemzés integráció](https://docs.microsoft.com/azure/security-center/security-center-integrating-alerts-with-log-integration)

- [Program az Azure Security Center az integrált biztonsági réseinek értékelése](https://blogs.msdn.microsoft.com/azuresecurity/2016/12/16/boost-azure-security-center-with-integrated-vulnerability-assessment/)
