---
title: az Orchestrator tooAzure Automation aaaMigrating |} Microsoft Docs
description: "Ismerteti, hogyan toomigrate runbookokat és az integrációs csomagokat a System Center Orchestrator tooAzure Automation."
services: automation
documentationcenter: 
author: bwren
manager: stevenka
editor: tysonn
ms.assetid: 1a7da58c-7a98-49b5-9d9d-001a9f6e631a
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2016
ms.author: bwren
ms.openlocfilehash: 797b50067ef2aa68470760e99d494b89ab7baacf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrating-from-orchestrator-tooazure-automation-beta"></a>Áttelepítés az Orchestrator tooAzure automatizálási (béta)
A Runbookok [System Center Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) tevékenységei során az Azure Automation runbookjai Windows PowerShell alapuló kimondottan az orchestrator alkalmazáshoz írt integrációs csomagok alapulnak.  [Grafikus forgatókönyvek](automation-runbook-types.md#graphical-runbooks) az Azure Automationben rendelkezik egy hasonló megjelenési tooOrchestrator runbookok azok eszközök, a PowerShell parancsmagok és a runbookok képviselő tevékenységek.

Hello [System Center Orchestrator áttelepítési eszközkészlet](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) eszközök tooassist tartalmazza a runbookok adatelemmé Orchestrator tooAzure Automation.  Ezenkívül tooconverting hello runbookok magukat, kell konvertálnia hello integrációs csomagok hello tevékenységek számára, hogy a runbookokat hello toointegration modulok használata a Windows PowerShell-parancsmagokkal.  

Az alábbiakban az Orchestrator runbookjai tooAzure Automation alakításának hello alapvető folyamat.  A lépések részletes leírását lásd az alábbi szakaszokban hello.

1. Töltse le a hello [System Center Orchestrator áttelepítési eszközkészlet](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) , amely hello eszközök és a cikkben szereplő modulok tartalmazza.
2. Importálás [szokásos tevékenységek modul](#standard-activities-module) az Azure Automation szolgáltatásbeli.  Ez magában foglalja a konvertált runbookok által használható szabványos Orchestrator-tevékenységeket konvertált verzióit.
3. Importálás [System Center Orchestrator integrációs modulok](#system-center-orchestrator-integration-modules) az adott integrációs csomagok a System Center elérő runbookok által használt Azure Automation-be.
4. Alakítsa át az egyéni és harmadik féltől származó integrációs csomagok hello [Integration Pack konverter](#integration-pack-converter) , és importálja az Azure Automation.
5. Alakítsa át az Orchestrator runbookokat hello segítségével [Runbook konverter](#runbook-converter) és az Azure Automationben telepítse.
6. Manuálisan szükséges Orchestrator eszközök létrehozása az Azure Automationben óta hello Runbook konverter nem konvertálja a ezeket az erőforrásokat.
7. Konfigurálja a [hibrid forgatókönyv-feldolgozó](#hybrid-runbook-worker) runbookokban a helyi adatok center toorun konvertálni, akik hozzáférhetnek a helyi erőforrások.

## <a name="service-management-automation"></a>Automatizált szolgáltatásfelügyelet
[A Szolgáltatáskezelési automatizálás](http://technet.microsoft.com/library/dn469260.aspx) (SMA) tárolja, és a helyi adatközpontban, például az Orchestrator felelős a runbookok és hello használ, az Azure Automation azonos integrációs modulok. Hello [Runbook konverter](#runbook-converter) alakítja át az Orchestrator runbookjai toographical runbookjai azonban ami nem támogatott az SMA.  Továbbra is telepíthet hello [szabványos tevékenységek modul](#standard-activities-module) és [System Center Orchestrator integrációs modulok](#system-center-orchestrator-integration-modules) az SMA rendszerbe, de manuálisan kell [a runbookok újraírási](http://technet.microsoft.com/library/dn469262.aspx).

## <a name="hybrid-runbook-worker"></a>hibrid runbook-feldolgozó
Az Orchestrator Runbookjai egy kiszolgálón tárolja, és futtassa a runbook-kiszolgálók, mind a helyi adatközpontban.  Azure Automation Runbookjai hello Azure felhőben vannak tárolva, és futtathat a helyi adatok center a egy [hibrid forgatókönyv-feldolgozó](automation-hybrid-runbook-worker.md).  Ez a hogyan általában fog futni az Orchestrator alakítja át, mert azok a helyi kiszolgálókon tervezett toorun runbookokat.

## <a name="integration-pack-converter"></a>Integrációs csomag konverter
Integrációs csomag konverter hello alakít hello használatával létrehozott integrációs csomagokat [Orchestrator Integration Toolkit (oit) használatával Kezdeményezett](http://technet.microsoft.com/library/hh855853.aspx) toointegration modulok alapján, hogy importálni lehessen Azure Automation Windows PowerShell vagy A Szolgáltatáskezelési automatizálás.  

Integrációs csomag konverter hello futtatásakor megnyílik egy varázsló, amely lehetővé teszi a tooselect egy integrációs csomag (oip) fájlját.  hello varázsló akkor integrációs csomag részeként hello tevékenységek listája, és lehetővé teszi tooselect, amely át lesz telepítve.  Hello varázsló befejeződése után, az integrációs modul, amely tartalmazza a megfelelő parancsmag minden hello eredeti integrációs csomag tevékenységei hello hoz létre.

### <a name="parameters"></a>Paraméterek
A rendszer hello megfelelő parancsmag hello integrációs modul konvertált tooparameters hello integrációs csomag tevékenység bármely tulajdonságokat.  A Windows PowerShell-parancsmagok rendelkeznek [közös paraméterek](http://technet.microsoft.com/library/hh847884.aspx) , amely az összes parancsmag használható.  Például hello - Verbose paraméter hatására a parancsmag toooutput részletes a működésével kapcsolatos információkat.  Előfordulhat, hogy nincs parancsmag azonos nevet közös paraméterként hello paraméterrel.  Ha egy tevékenység azonos nevet közös paraméterként hello tulajdonságot, hello varázsló kérni fogja azt tooprovide hello paraméter egy másik nevet.

### <a name="monitor-activities"></a>A figyelő tevékenységek
Útmutató az Orchestrator runbookjai figyelése egy [figyelése](http://technet.microsoft.com/library/hh403827.aspx) és egy adott esemény által meghívott várakozási toobe folyamatos Futtatás.  Azure Automation szolgáltatásbeli nem támogatja a figyelő runbookok, így hello integrációs csomag figyelő tevékenységeket nem lehet konvertálni.  Ehelyett egy helyőrző parancsmag hello integrációs modul hello figyelése tevékenység jön létre.  Ez a parancsmag nem funkciókkal rendelkezik, de lehetővé teszi bármely konvertált azt használó runbookot toobe telepítve.  Ez a forgatókönyv nem lesz képes toorun az Azure Automationben, de így módosíthatja is telepíthető.

### <a name="integration-packs-that-cannot-be-converted"></a>Nem konvertálható integrációs csomagok
OIT nem létrehozott integrációs csomagokat nem lehet konvertálni az integrációs csomag konverter hello. Van még néhány eszköz, amely jelenleg nem lehet konvertálni a Microsoft által biztosított integrációs csomagokat.  Ezeket az integrációs csomagokat konvertált verziói lettek [letöltésre](#system-center-orchestrator-integration-modules) , hogy az Azure Automation vagy a Service Management Automation telepíthető.

## <a name="standard-activities-module"></a>Szokásos tevékenységek modul
Az orchestrator készletét tartalmazza [szokásos tevékenységek](http://technet.microsoft.com/library/hh403832.aspx) , amelyek nem szerepelnek az integrációs csomag, de sok runbookok által használt.  hello szokásos tevékenységek modul az egyenértékű parancsmag minden ezeket a tevékenységeket tartalmazó integrációs modul létrehozása.  Telepítenie kell az integrációs modul az Azure Automationben szokásos tevékenység használható konvertált runbookokat importálása előtt.

Ezenkívül toosupporting alakítja át a runbookok, hello szokásos tevékenységek modulban hello parancsmagok segítségével valaki Orchestrator toobuild új Azure Automation runbookjai ismernie.  Hello funkció az összes hello szokásos tevékenységek parancsmagokkal hajtható végre, amíg azok másképp működhet.  hello parancsmagok hello alakítja át a szokásos tevékenységek modul fog működni a hello azonos módon a megfelelő és felhasználásával hello ugyanazokkal a paraméterekkel.  Ez segítheti a hello meglévő Orchestrator runbook Szerző az átmenet tooAzure Automation-forgatókönyv.

## <a name="system-center-orchestrator-integration-modules"></a>A System Center Orchestrator integrációs modulok
A Microsoft biztosít [integrációs csomagok](http://technet.microsoft.com/library/hh295851.aspx) runbookok tooautomate System Center-összetevők és egyéb termékek készítéséhez.  Néhány ezeket az integrációs csomagokat OIT jelenleg alapulnak, de jelenleg nem lehet konvertált toointegration modulok ismert problémák miatt.  [A System Center Orchestrator integrációs modulok](https://www.microsoft.com/download/details.aspx?id=49555) ezeket az integrációs csomagokat, amelyek az Azure Automation- és Service Management Automation importálható konvertált verzióját tartalmazza.  

Az eszköz hello RTM verzióját által frissített hello integrációs csomagok Integration Pack konverter közzé lesz téve hello az átalakítható OIT alapján verziói.  Útmutatást nyújtanak is runbookokat hello integrációs csomagok tevékenységek segítségével, nem próbálta konvertálni alapján OIT tooassist.

## <a name="runbook-converter"></a>Runbook konverter
hello Runbook konverter konvertálja az Orchestrator runbookjai [grafikus forgatókönyvek](automation-runbook-types.md#graphical-runbooks) , hogy importálni lehessen Azure Automation.  

Egy parancsmaggal nevű Runbook konverter valósul meg egy PowerShell-modul **ConvertFrom-SCORunbook** , amely hello konverziót hajt végre.  Hello eszköz telepítésekor egy helyi tooa PowerShell-munkamenetet, amely betölti a hello parancsmag hoz létre.   

Következő hello alapvető folyamat tooconvert Orchestrator runbookot, és importálja a fájlt az Azure Automation.  hello következő szakaszokban további részleteket a hello eszközzel és a konvertált runbookok használatával.

1. Az Orchestrator exportálja a runbookokat.
2. Integrációs modulok hello runbook minden tevékenység beszerzése.
3. Az átalakítás hello Orchestrator runbookokat hello exportált fájlban.
4. Tekintse át az összes szükséges manuális feladatok naplók toovalidate hello konvertálását és ezek toodetermine információit.
5. Azure Automation, átalakított runbookokat importálni.
6. Minden szükséges eszközök létrehozása az Azure Automationben.
7. Azure Automation toomodify hello runbook szerkesztése szükséges tevékenységeket.

### <a name="using-runbook-converter"></a>Runbook konverter használatával
hello szintaxisának **ConvertFrom-SCORunbook** a következőképpen történik:

    ConvertFrom-SCORunbook -RunbookPath <string> -Module <string[]> -OutputFolder <string>

* RunbookPath - elérési út toohello exportálási fájlt tartalmazó hello runbookok tooconvert.
* A modul - vesszővel elválasztott runbookokat hello tevékenységeket tartalmazó integrációs modulok felsorolása.
* OutputFolder - elérési út toohello mappa toocreate grafikus forgatókönyvek alakítja.

a következő példaparancs konvertálja nevű exportálási fájlba a runbookokat hello hello **MyRunbooks.ois_export**.  Ezeknél a runbookoknál hello Active Directory és a Data Protection Manager integrációs csomagok használata.

    ConvertFrom-SCORunbook -RunbookPath "c:\runbooks\MyRunbooks.ois_export" -Module c:\ip\SystemCenter_IntegrationModule_ActiveDirectory.zip,c:\ip\SystemCenter_IntegrationModule_DPM.zip -OutputFolder "c:\runbooks"


### <a name="log-files"></a>Naplófájlok
Runbook konverter hello hoz létre a következő naplófájlokban hello hello hello és ugyanazon a helyen runbook alakítja át.  Ha hello fájlok már léteznek, majd azok felülírja hello utolsó átalakítás származó információkkal.

| Fájl | Tartalom |
|:--- |:--- |
| Runbook konverter - Progress.log |Részletes utasítások hello átalakítás, beleértve az egyes tevékenységek sikeresen konvertálni információkat és figyelmeztetés minden egyes tevékenységhez konvertálás nem történt. |
| Runbook konverter - Summary.log |Hello összefoglalása utolsó átalakítás, beleértve a figyelmeztetéseket, és feladatokhoz például létrehozhat egy változó megadása kötelező, konvertálni hello runbook tooperform nyomon követéséhez. |

### <a name="exporting-runbooks-from-orchestrator"></a>Az Orchestrator runbookok exportálása
az Orchestrator, amely tartalmaz egy vagy több runbook exportálási fájlba hello Runbook konverter működik.  A megfelelő Azure Automation-runbook minden Orchestrator runbook hello exportfájl hoz létre.  

az Orchestrator, a runbook tooexport hello runbookot a Runbook Designer hello gombbal, és válassza ki **exportálása**.  tooexport hello hello mappa nevét és válassza ki az összes runbook egy mappát, kattintson a jobb gombbal **exportálása**.

### <a name="runbook-activities"></a>Runbook-tevékenységek
Runbook konverter hello hello az Orchestrator runbook tooa megfelelő tevékenység az Azure Automationben lévő minden tevékenység alakítja át.  Ezekre a tevékenységekre, amely nem lehet konvertálni egy helyőrző tevékenység figyelmeztető szöveg hello runbookban jön létre.  Miután az Azure Automation konvertálni hello runbookot importál, le kell cserélnie ezek a tevékenységek bármelyike érvényes hello szükséges funkciókat ellátó tevékenységek.

Hello Orchestrator-tevékenységeket [szabványos tevékenységek modul](#standard-activities-module) konvertálja.  Nincsenek néhány szokásos Orchestrator-tevékenységeket, amelyeken nem ebben a modulban, ha nem történik meg.  Például **Platformesemény küldése** rendelkezik Azure Automation egyenértékű, mivel hello esemény adott tooOrchestrator.

[Tevékenységek figyelését](https://technet.microsoft.com/library/hh403827.aspx) nem lettek konvertálva, mert nincs megfelelő toothem az Azure Automationben.  hello kivétel figyelő tevékenységek [konvertálni az integrációs csomagok](#integration-pack-converter) konvertált toohello Helyőrző tevékenység fogja tárolni.

A tevékenység egy [konvertálni az integrációs csomag](#integration-pack-converter) konvertálja, ha Ön által hello elérési toohello integrációs modul hello **modulok** paraméter.  A System Center integrációs csomagok használhatják hello [System Center Orchestrator integrációs modulok](#system-center-orchestrator-integration-modules).

### <a name="orchestrator-resources"></a>Erőforrások az orchestrator programhoz
hello Runbook konverter csak a runbookok, nem más Orchestrator erőforrások, például számlálókat, változókat és kapcsolatok alakítja.  Teljesítményszámlálók az Azure Automationben nem támogatottak.  Változók és a kapcsolatok támogatottak, de manuálisan kell létrehoznia őket.  hello naplófájlok fog arról tájékoztatják, ha hello runbooknak szüksége ezekhez az erőforrásokhoz, és adja meg a megfelelő forrásanyagokat, hogy az Azure Automationben toocreate hello runbook toooperate megfelelően konvertálni.

Egy runbook például egy tevékenység lehet használni egy változó toopopulate egy adott értéket.  hello konvertált runbook konvertálja hello tevékenységet, majd adja meg a változó eszköz az Azure Automation azonos nevet hello Orchestrator változóként hello.  Ez tájékoztat a hello **Runbook konverter - Summary.log** hello átalakítás után létrehozott fájl.  Szüksége lesz toomanually a változó eszköz létrehozása az Azure Automationben hello runbook használata előtt.

### <a name="input-parameters"></a>A bemeneti paraméterek
Az Orchestrator Runbookjai fogadja el a bemeneti paramétereket hello **adatok inicializálása** tevékenység.  Ha konvertálni hello runbook tartalmazza ennek a tevékenységnek, majd egy [bemeneti paraméter](automation-graphical-authoring-intro.md#runbook-input-and-output) hello Azure Automation a runbook létrejön mindegyik paraméterhez hello tevékenységben.  A [munkafolyamat parancsprogram-vezérlők](automation-graphical-authoring-intro.md#activities) tevékenység kéri le, és visszaadja az egyes paramétereket konvertálni hello runbook jön létre.  Hello runbookban levő tevékenységek, amelyek egy bemeneti paraméter a tevékenység toohello kimeneti hivatkoznia.

hello oka, hogy használja-e ezt a stratégiát toobest tükrözött hello funkció hello az Orchestrator runbook.  Új grafikus forgatókönyvek tevékenységek közvetlenül egy Runbook bemeneti adatforrást használó tooinput paraméterek kell hivatkoznia.

### <a name="invoke-runbook-activity"></a>Runbook meghívása tevékenységhez
Az Orchestrator Runbookjai elindíthatnak más runbookokat hello rendelkező **Runbook meghívása** tevékenység. Ha konvertálni hello runbook tartalmazza ezt a tevékenységet és hello **Várakozás a befejezésre** beállítás, akkor a runbook-tevékenység létrehozott hozzá konvertálni hello runbookban.  Ha hello **Várakozás a befejezésre** beállítása, majd a munkafolyamat-parancsprogram-tevékenység hoz létre, amely használja **Start-AzureAutomationRunbook** toostart hello runbook.  Miután az Azure Automation konvertálni hello runbookot importál, módosítania kell ezt a tevékenységet hello tevékenység meghatározott hello információkat.

## <a name="related-articles"></a>Kapcsolódó cikkek
* [A System Center 2012 – Orchestrator](http://technet.microsoft.com/library/hh237242.aspx)
* [A Szolgáltatáskezelési automatizálás](https://technet.microsoft.com/library/dn469260.aspx)
* [Hibrid forgatókönyv-feldolgozó](automation-hybrid-runbook-worker.md)
* [Az orchestrator szokásos tevékenységek](http://technet.microsoft.com/library/hh403832.aspx)
* [Letöltés a System Center Orchestrator áttelepítési eszközkészlet](https://www.microsoft.com/en-us/download/details.aspx?id=47323)
