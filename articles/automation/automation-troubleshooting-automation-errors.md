---
title: "közös Azure Automation-problémák aaaTroubleshooting |} Microsoft Docs"
description: "Ez a cikk tájékoztatást ad toohelp és Azure Automation gyakori hibák elhárítása."
services: automation
documentationcenter: 
author: mgoedtel
manager: stevenka
editor: tysonn
tags: top-support-issue
keywords: "Automation-hiba, hibaelhárítási, probléma"
ms.assetid: 5f3cfe61-70b0-4e9c-b892-d02daaeee07d
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/26/2017
ms.author: sngun; v-reagie
ms.openlocfilehash: eb7d1cc5726f2b7a86c860e8f0c8340fa4221b2e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-common-issues-in-azure-automation"></a>Az Azure Automationben kapcsolatos gyakori hibák elhárítása 
Ez a cikk ismerteti az Azure Automationben tapasztalhat, és lehetséges megoldásokat tooresolve javasol gyakori hibák elhárításához őket.

## <a name="authentication-errors-when-working-with-azure-automation-runbooks"></a>Az Azure Automation-runbook használatakor hitelesítési hibák
### <a name="scenario-sign-in-tooazure-account-failed"></a>Forgatókönyv: A bejelentkezés tooAzure fiókot nem sikerült
**Hiba:** hibaüzenet hello "Unknown_user_type: Ismeretlen felhasználó típusa" hello Add-AzureAccount vagy Login-AzureRmAccount parancsmagok használata.

**Hello hiba oka:** Ez a hiba akkor fordul elő, ha hello hitelesítőadat-eszköz neve érvénytelen, vagy ha hello felhasználónevét és jelszavát, amellyel toosetup hello Automation szolgáltatásbeli hitelesítőadat-eszköz nem érvényes.

**Hibaelhárítási tippek:** sorrendben toodetermine mi, a lépések hello érvénybe:  

1. Győződjön meg arról, hogy nincs-e a különleges karaktereket, beleértve a hello  **@**  hello automatizálási hitelesítő adat eszköz neve tooconnect tooAzure használt karakter.  
2. Ellenőrizze, hogy használható hello felhasználónévvel és jelszóval, a helyi PowerShell ISE-szerkesztőben hello Azure Automation szolgáltatásbeli hitelesítőadat tárolódnak. Ehhez futtassa a következő parancsmagok a PowerShell ISE hello hello:  

        $Cred = Get-Credential  
        #Using Azure Service Management   
        Add-AzureAccount –Credential $Cred  
        #Using Azure Resource Manager  
        Login-AzureRmAccount –Credential $Cred
3. Ha a hitelesítés sikertelen helyileg, ez azt jelenti, hogy nem állított be az Azure Active Directorybeli hitelesítő adatokat megfelelően. Tekintse meg a túl[hitelesítéséhez az Azure Active Directoryval tooAzure](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) blog post tooget hello Azure Active Directory-fiókot állítsa be megfelelően.  

### <a name="scenario-unable-toofind-hello-azure-subscription"></a>Forgatókönyv: Nem toofind hello Azure-előfizetés
**Hiba:** hello hibaüzenet "nevű előfizetést hello ``<subscription name>`` nem található" hello válassza-AzureSubscription vagy Select-AzureRmSubscription parancsmagok használata során.

**Hello hiba oka:** Ez a hiba akkor fordul elő, ha hello előfizetés neve érvénytelen, vagy ha tooget hello előfizetés részletei kívánó hello Azure Active Directory felhasználó nem van konfigurálva, hello előfizetés rendszergazdája.

**Hibaelhárítási tippek:** sorrendben toodetermine Ha tooAzure megfelelően hitelesített, és hozzáférést toohello előfizetése próbált tooselect, végezze el a lépéseket követve hello:  

1. Győződjön meg arról, hogy lefuttatta a hello **Add-AzureAccount** hello futtatása előtt **válasszon-AzureSubscription** parancsmag.  
2. Ha továbbra is megjelenik ez a hibaüzenet, a kód módosítása hello hozzáadásával **Get-AzureSubscription** hello a következő parancsmag **Add-AzureAccount** parancsmag és hello kód hajthat végre.  Most ellenőrizheti, hogy tartalmazza-e a Get-AzureSubscription hello kimenete az előfizetés részletei.  

   * Ha nem lát minden előfizetés részletei hello kimenet, ez azt jelenti, hogy hello előfizetés még nincs inicializálva.  
   * Ha hello előfizetés részletei hello kimenet látja, akkor győződjön meg arról, hogy használ hello megfelelő előfizetés nevű vagy Azonosítójú hello **válasszon-AzureSubscription** parancsmag.   

### <a name="scenario-authentication-tooazure-failed-because-multi-factor-authentication-is-enabled"></a>Forgatókönyv: Hitelesítési tooAzure sikertelen volt, mert a többtényezős hitelesítés engedélyezett-e
**Hiba:** hello hibaüzenet "Hozzáadás-AzureAccount: AADSTS50079: erős hitelesítés beléptetési (igazolása-up) kell" a Azure felhasználónévvel és jelszóval tooAzure hitelesítésekor.

**Hello hiba oka:** többtényezős hitelesítés az Azure-fiókjával rendelkezik, ha az Azure Active Directory felhasználói tooauthenticate tooAzure nem használható.  Ehelyett szükség toouse tanúsítvány vagy egy szolgáltatás egyszerű tooauthenticate tooAzure.

**Hibaelhárítási tippek:** toouse tanúsítvány hello Azure szolgáltatásfelügyelet parancsmagokat, és tekintse meg a túl[létrehozása és hozzáadása egy tanúsítvány toomanage Azure szolgáltatások.](http://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) toouse egy egyszerű Azure Resource Manager-parancsmagokkal, tekintse meg a túl[szolgáltatás egyszerű Azure-portál használatával létrehozása](../azure-resource-manager/resource-group-create-service-principal-portal.md) és [hitelesítése egy egyszerű szolgáltatást az Azure Resource Manager eszközzel.](../azure-resource-manager/resource-group-authenticate-service-principal.md)

## <a name="common-errors-when-working-with-runbooks"></a>A forgatókönyvek használata során előforduló hibákat
### <a name="scenario-hello-runbook-job-start-was-attempted-three-times-but-it-failed-toostart-each-time"></a>Forgatókönyv: hello runbook feladat kezdési háromszor történt kísérlet, de minden alkalommal, amikor nem toostart
**Hiba:** a runbook hello hibaüzenettel meghiúsul, "" hello feladatot a rendszer három alkalommal próbálta, de nem sikerült."

**Hello hiba oka:** ezt a hibát okozhatja a következő okok miatt hello:  

1. Memóriára vonatkozó korlátozást.  Azt kell dokumentálni vonatkozó korlátozások memória lefoglalt tooa védőfal [Automation szolgáltatásra vonatkozó korlátozások](../azure-subscription-service-limits.md#automation-limits) , a feladatok sikertelenek lehetnek, ha 400 MB-nál több memóriát használ. 

2. A modul nem kompatibilis.  Ez akkor fordulhat elő, ha a modul függőségek nem helyesek, és ha nem, a runbook általában visszaállítja egy "parancs nem található" vagy "Paraméter nem lehet kötni" üzenet. 

**Hibaelhárítási tippek:** fog hello probléma megoldására kijelölt bármelyik hello megoldások a következő:  

* Javasolt módszerek toowork belül hello memóriakorlátot toosplit hello munkaterhelés elérhetővé tétele több runbook, dolgoz fel mértékű adatokat a memóriában, nem toowrite szükségtelen kimenetét a runbookok, és vegye figyelembe, hogy hány ellenőrzőpontot ír be a PowerShell munkafolyamat-forgatókönyvekről.  

* Szüksége tooupdate az Azure modulok hello lépéseket követve [hogyan tooupdate Azure PowerShell-modulok az Azure Automationben](automation-update-azure-modules.md).  


### <a name="scenario-runbook-fails-because-of-deserialized-object"></a>Forgatókönyv: A Runbook nem deszerializált objektum miatt
**Hiba:** a runbook végrehajtása sikertelen, hiba: hello "paraméter nem lehet kötni ``<ParameterName>``. Nem alakítható át hello ``<ParameterType>`` Deserialized típusú érték ``<ParameterType>`` tootype ``<ParameterType>``".

**Hello hiba oka:** Ha a runbookban egy PowerShell-munkafolyamat, tárol összetett objektumok sorrendje toopersist deszerializált formátumban a runbook állapota Ha hello munkafolyamat fel van függesztve.  

**Hibaelhárítási tippeket:**  
A következő három megoldások hello bármelyikét fogja ezt a problémát:

1. Ha egy parancsmag tooanother összetett objektumok vannak ismertetett, tegye ezeket a parancsmagokat egy InlineScript.  
2. Ahelyett, hogy a teljes objektumot hello át hello a komplex objektum átadása hello neve vagy értéke, amelyekre szüksége van.  
3. Használja a PowerShell-forgatókönyv egy PowerShell-munkafolyamati forgatókönyv helyett.  

### <a name="scenario-runbook-job-failed-because-hello-allocated-quota-exceeded"></a>Forgatókönyv: Runbook-feladat meghiúsult, mert hello lefoglalt túllépte a kvótát.
**Hiba:** "hello hello havi feladatok teljes futási ideje elérte az előfizetés" hello hiba: A runbook-feladat meghiúsul.

**Hello hiba oka:** ez hibát okoz hello feladat végrehajtása meghaladja hello 500 perces szabad kvótát a fiókjához. Ez a kvóta tooall típusú feladat végrehajtási feladatok például tesztelése a feladatok, egy feladat kezdési hello portálról, egy feladat végrehajtása webhookok használatával, és a feladat tooexecute ütemezés vagy hello Azure-portál használatával, vagy az Adatközpont vonatkozik. az automatizálási díjszabása kapcsolatos információkért tekintse meg toolearn [Automation árképzési](https://azure.microsoft.com/pricing/details/automation/).

**Hibaelhárítási tippek:** Ha azt szeretné, toouse percnél hosszabb ideig 500 toochange kell havonta feldolgozásának hello ingyenes szint toohello alapszintű rétegben az előfizetést. Frissíthet toohello alapszintű rétegben által véve hello a következő lépéseket:  

1. Jelentkezzen be tooyour Azure-előfizetés  
2. Válassza ki a kívánt tooupgrade hello Automation-fiók  
3. Kattintson a **beállítások** > **Tarifacsomag és használat** > **tarifacsomag**  
4. A hello **válasszon tarifacsomagot** panelen válassza **alapvető**    

### <a name="scenario-cmdlet-not-recognized-when-executing-a-runbook"></a>Forgatókönyv: A parancsmag nem ismerhető fel, amikor egy runbook futtatását
**Hiba:** hello hibaüzenettel meghiúsul a runbook-feladat "``<cmdlet name>``: hello kifejezés ``<cmdlet name>`` nem ismerhető fel egy parancsmag, a függvény, a parancsfájl vagy a futtatható program hello neve."

**Hello hiba oka:** ezt a hibát az okozza, ha hello PowerShell motor hello parancsmag használ a runbook nem található.  Ennek oka lehet hello parancsmag tartalmazó hello modul hello fiók hiányoznak, egy neve ütközik, a runbook nevét, vagy hello parancsmag is létezik egy másik modul és automatizálási hello neve nem oldható fel.

**Hibaelhárítási tippek:** fog hello probléma megoldására kijelölt bármelyik hello megoldások a következő:  

* Ellenőrizze, hogy megfelelően van megadva hello parancsmag neve.  
* Ellenőrizze, hogy hello parancsmag szerepel az Automation-fiók, és hogy nincsenek-e ütközések. tooverify, ha az megtalálható a hello parancsmag, nyissa meg a runbook szerkesztési módban, majd keresse meg a kívánt toofind hello könyvtárban, vagy futtassa hello parancsmag **Get-Command ``<CommandName>``** .  Hello parancsmagot ellenőrzése után rendelkezésre álló toohello fiók, hogy vannak-e nem neve ütközik más parancsmagok vagy a runbookokat, és adja hozzá toohello vászonra, és győződjön meg arról, hogy a runbookban egy érvényes paraméterkészlet használunk.  
* Ha egy ütköző és hello parancsmag érhető el a két különböző modulok, a megoldást hello teljesen minősített nevet hello parancsmag használatával. Használhat például **ModuleName\CmdletName**.  
* Ha a hibrid feldolgozócsoport hello runbook helyszíni állnak végrehajtás alatt, majd győződjön meg arról, hogy hello modul/parancsmag hello hibridfeldolgozó üzemeltető hello gépen telepítve van.

### <a name="scenario-a-long-running-runbook-consistently-fails-with-hello-exception-hello-job-cannot-continue-running-because-it-was-repeatedly-evicted-from-hello-same-checkpoint"></a>Forgatókönyv: Egy hosszú ideig futó runbook rendszeresen nem hello kivétel miatt: "hello feladat nem tovább fut, mert azt a ismételten fürtből hello azonos ellenőrzőpont".
**Hello hiba oka:** Ez az elvárt működés toohello "Méltányosan" figyelési folyamatok Azure Automation, amely automatikusan felfüggeszti a runbook, ha tovább, mint 3 óra végrehajtása során belül esedékes. Azonban a következő hibaüzenet jelenik nem rendelkezik "a következő teendő" hello beállítások. Számos oka egy runbook is fel kell függeszteni. Felfüggeszti a hiba akkor fordulhat elő többnyire esedékes tooerrors. Például egy runbookot, hálózati hiba esetén vagy egy összeomlási a nem kezelt kivétel hello hello runbookot futtató Runbook Worker, lesz az összes okozhat hello runbook toobe felfüggesztve, és indítsa el az utolsó ellenőrzőponttól folytatásakor.

**Hibaelhárítási tippek:** hello dokumentált megoldás tooavoid Ez a hiba csak toouse ellenőrzőpontokat a munkafolyamatban.  több tekintse meg a túl toolearn[tanulási PowerShell-munkafolyamatok](automation-powershell-workflow.md#checkpoints).  "Valós Share utasítást" és ellenőrzőpont alaposabb magyarázata blog cikkben található [ellenőrzőpontok használata a Runbookokban](https://azure.microsoft.com/en-us/blog/azure-automation-reliable-fault-tolerant-runbook-execution-using-checkpoints/).

## <a name="common-errors-when-importing-modules"></a>A modulok importálása során előforduló hibákat
### <a name="scenario-module-fails-tooimport-or-cmdlets-cant-be-executed-after-importing"></a>Forgatókönyv: Modul sikertelen tooimport vagy parancsmagok nem hajtható végre importálása után
**Hiba:** modul tooimport sikertelen vagy sikeres importálása, de nincs parancsmagok ki kell olvasni.

**Hello hiba oka:** néhány általános oka, hogy egy modul előfordulhat, hogy nem sikeresen importálni Automation tooAzure:  

* hello struktúra nem egyezik meg a hello struktúra, hogy Automation kell azt a toobe.  
* hello modul az függ, amelyeket nem telepített tooyour Automation-fiók egy másik modul.  
* hello modul hello mappában függősége hiányzik.  
* Hello **New-AzureRmAutomationModule** parancsmag használt tooupload hello modul folyamatban van, és nem adott hello teljes tárolási elérési útja, illetve nem töltődtek be hello modul egy nyilvánosan elérhető URL-cím segítségével.  

**Hibaelhárítási tippeket:**  
A következő megoldások hello bármelyikét kijavítja az hello problémát:  

* Győződjön meg arról, hogy hello modul a következő formátum a következő hello:  
  ModuleName.Zip  **->**  ModuleName vagy a verziószám  **->**  (ModuleName.psm1, ModuleName.psd1)
* Nyissa meg hello .psd1 fájlt, és hello modul vannak-e az összes függőséget.  Ha igen, töltse fel a modulok toohello Automation-fiók.  
* Győződjön meg arról, hogy minden hivatkozott .dll hello modul mappában találhatók.  

## <a name="common-errors-when-working-with-desired-state-configuration-dsc"></a>A kívánt állapot konfigurációs szolgáltatása (DSC) használatakor előforduló gyakori hibák
### <a name="scenario-node-is-in-failed-status-with-a-not-found-error"></a>Forgatókönyv: Az csomópont "Nem található" hiba miatt sikertelen állapotban van.
**Hiba:** hello fürtcsomópont egy jelentés ezzel **sikertelen** állapotát és hello hibát tartalmazó "hello kísérlet tooget hello művelet server https://``<url>``//accounts/``<account-id>``/Nodes(AgentId=``<agent-id>``) / GetDscAction sikertelen volt, mert egy érvényes konfigurációs ``<guid>`` nem található. "

**Hello hiba oka:** Ez a hiba általában akkor fordul elő, amikor hello csomópont hozzá van rendelve tooa konfiguráció nevét (pl. ABC) helyett egy csomópont-konfiguráció nevét (pl. ABC. Webkiszolgáló).  

**Hibaelhárítási tippeket:**  

* Győződjön meg arról, hogy nem hello "konfiguráció neve" és "csomópont-konfiguráció neve" hello csomópont rendeli.  
* A csomópont konfigurációs tooa csomópont Azure portál vagy egy PowerShell-parancsmag használatával rendelhet hozzá.

  * A rendezés tooassign a csomópont konfigurációs tooa csomópont Azure-portál használatával, nyissa meg a hello **DSC-csomópontok** panelen, majd válasszon ki egy csomópontot, és kattintson a **csomópont-konfiguráció hozzárendelése** gombra.  
  * A rendezés tooassign a csomópont konfigurációs tooa csomópont PowerShell-parancsmag használatával, használja a **Set-AzureRmAutomationDscNode** parancsmag

### <a name="scenario--no-node-configurations-mof-files-were-produced-when-a-configuration-is-compiled"></a>Forgatókönyv: Nincsenek csomópont-konfigurációt (MOF-fájlok) keletkezett, amikor egy konfigurációs fordítása
**Hiba:** a DSC-fordítási feladat felfüggesztése hello hiba miatt: "fordítási sikeresen befejeződött, de nem a csomópont konfigurációs .mofs létrehozott".

**Hello a hibát:** Ha a kifejezés a következő hello hello **csomópont** hello DSC-konfiguráció kulcsszót kiértékeli túl$ null értékű, és nem a csomópont-konfigurációt gyűjtenek.    

**Hibaelhárítási tippeket:**  
A következő megoldások hello bármelyikét kijavítja az hello problémát:  

* Győződjön meg arról, hogy hello kifejezés következő toohello **csomópont** kulcsszót hello a konfiguráció definíciója nem értékeli túl$ null.  
* Ha ConfigurationData átadott hello konfiguráció fordítása során, győződjön meg arról, hogy hello várt értékek átadott szükséges, hogy, hogy a hello konfigurálása [ConfigurationData](automation-dsc-compile.md#configurationdata).

### <a name="scenario--hello-dsc-node-report-becomes-stuck-in-progress-state"></a>Forgatókönyv: a DSC-csomópont jelentés hello válik akadt "folyamatban" állapota
**Hiba:** DSC ügynök kiírja a "Nem található a megadott tulajdonságok értékeit az adott példány."

**Hello hiba oka:** a WMF verzióra frissítette, és rendelkezik WMI sérült.  

**Hibaelhárítási tippek:** hello hello utasításait követve [ismert problémák és korlátozások DSC](https://msdn.microsoft.com/powershell/wmf/5.0/limitation_dsc) toofix hello probléma.

### <a name="scenario--unable-toouse-a-credential-in-a-dsc-configuration"></a>Forgatókönyv: Nem toouse hitelesítő adatokat a DSC-konfiguráció
**Hiba:** a DSC-fordítási feladat fel lett függesztve, hello hiba miatt: "tulajdonság"-hitelesítőadat"típusú feldolgozása System.InvalidOperationException hiba ``<some resource name>``: átalakítása és egy titkosított jelszó tárolására, egyszerű szöveges érték adható meg, csak ha PSDscAllowPlainTextPassword be van állítva tootrue".

**Hello hiba oka:** konfigurációban használt hitelesítő adatokat, de nem adott meg megfelelő **ConfigurationData** tooset **PSDscAllowPlainTextPassword** tootrue az egyes csomópontok konfiguráció.  

**Hibaelhárítási tippeket:**  

* Győződjön meg arról, hogy toopass a megfelelő hello **ConfigurationData** tooset **PSDscAllowPlainTextPassword** tootrue hello konfigurációs szerepel minden csomópont-konfiguráció számára. További információkért tekintse meg túl[Azure Automation DSC eszközök](automation-dsc-compile.md#assets).

## <a name="next-steps"></a>Következő lépések
Ha követte a fenti hibaelhárítási hello és hello válaszfájl nem található, az alábbi hello további támogatási lehetőségek tekintheti meg.

* Az Azure-szakértők segítséget kérhet. Küldje el a problémát toohello [MSDN Azure vagy a Stack Overflow fórumok.](https://azure.microsoft.com/support/forums/).
* A fájl az Azure támogatási incidens. Nyissa meg toohello [Azure támogatási webhelyén](https://azure.microsoft.com/support/options/) kattintson **segítségre van szüksége** alatt **műszaki és a számlázási támogatás**.
* A parancsprogram-kérelem továbbfejlesztéséről [Script Center](https://azure.microsoft.com/documentation/scripts/) Ha egy Azure Automation-runbook megoldás vagy az integrációs modul keres.
* Visszajelzés vagy szolgáltatás-kérelmek POST az Azure Automation a [User Voice](https://feedback.azure.com/forums/34192--general-feedback).
