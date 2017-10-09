---
title: "aaaRunbook-kimenet és üzenetek az Azure Automationben |} Microsoft Docs"
description: "Hogyan toocreate és lekérése kimenet és a hiba érkező üzenetek az Azure Automation runbookjai Desribes."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 13a414f5-0e2c-4be2-9558-a3e3ec84b6b2
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/11/2016
ms.author: magoedte;bwren
ms.openlocfilehash: c1505fa889473766bfa47e43aaed2449d60ad851
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="runbook-output-and-messages-in-azure-automation"></a>Runbook-kimenet és üzenetek az Azure Automationben
Azure Automation-forgatókönyv a legtöbb valamilyen kimenetet például egy hiba üzenet toohello felhasználó fog rendelkezni, vagy egy összetett objektumot egy másik munkafolyamat által használt toobe készült. A Windows PowerShell szintén [több adatfolyam](http://blogs.technet.com/heyscriptingguy/archive/2014/03/30/understanding-streams-redirection-and-write-host-in-powershell.aspx) egy parancsfájl vagy a munkafolyamat toosend kimenetét. Azure Automation szolgáltatásbeli eltérő módon működik az összes ezekbe az adatfolyamokba, és a bevált gyakorlatokat kövesse toouse minden runbook létrehozásakor.

hello következő táblázat röviden hello adatfolyamok és hello Azure felügyeleti portálon viselkedésük mind a közzétett runbookok futtatásakor, és ha [runbook tesztelése](automation-testing-runbook.md). További részleteket az egyes adatfolyamokkal az ezt követő szakaszok szolgálnak.

| Az adatfolyam | Leírás | Közzétéve | Tesztelés |
|:--- |:--- |:--- |:--- |
| Kimenet |Másik runbookok toobe készült objektum. |Írt toohello feladatelőzményekben. |Hello Tesztkimenet ablaktáblán jelennek meg. |
| Figyelmeztetés |Hello felhasználónak szóló figyelmeztető üzenetet. |Írt toohello feladatelőzményekben. |Hello Tesztkimenet ablaktáblán jelennek meg. |
| Hiba |Hello felhasználónak szóló hibaüzenet. A kivételek hello runbook továbbra is fut egy hibaüzenet megjelenésekor alapértelmezés szerint. |Írt toohello feladatelőzményekben. |Hello Tesztkimenet ablaktáblán jelennek meg. |
| Részletes |Általános vagy a hibakeresési információk üzeneteket. |Ha a részletes naplózás is engedélyezve van a hello runbook írt csak toojob előzményeit. |Hello Tesztkimenet ablaktáblán jelenik meg, csak ha $VerbosePreference tooContinue hello runbook. |
| Folyamatban van |Automatikusan létrehozott előtt és után hello runbook minden tevékenysége rögzíti. hello runbook nem szabadna megpróbálniuk toocreate a saját állapotrekordjait, mivel egy interaktív felhasználó szolgálnak. |Ha folyamatban van a naplózás kapcsolva hello runbook írt csak toojob előzményeit. |Nem jelenik meg a hello Tesztkimenet ablaktáblán. |
| Hibakeresés |Egy interaktív felhasználó számára készült üzenetek. Nem használható a runbookok. |Nem írt toojob előzményeit. |Nem írt tooTest Tesztkimenet ablaktáblán. |

## <a name="output-stream"></a>Kimeneti adatfolyam
hello kimeneti adatfolyamba megfelelően egy parancsfájl vagy a munkafolyamat által létrehozott objektumok kimenetéhez használható. Azure Automation, ez az adatfolyam elsősorban használják a által felhasznált objektumok toobe [runbookokat hello aktuális runbookot meghívó szülő](automation-child-runbooks.md). Ha Ön [beágyazottan indított runbook hívja](automation-child-runbooks.md#invoking-a-child-runbook-using-inline-execution) a szülőrunbookból, akkor ad vissza adatokat hello kimeneti adatfolyam toohello szülő. Hello kimeneti adatfolyam toocommunicate általános információkat hátsó toohello felhasználói csak akkor ajánlott, ha soha nem fogja meghívni egy másik runbook hello runbook tudja. Ajánlott eljárásként, azonban általában használjon hello [részletes adatfolyam](#Verbose) toocommunicate általános információkat toohello felhasználó.

Adatok toohello kimeneti adatfolyam használatával írhat [Write-Output](http://technet.microsoft.com/library/hh849921.aspx) vagy hello objektum saját sort hello runbook helyezésével.

    #hello following lines both write an object toohello output stream.
    Write-Output –InputObject $object
    $object

### <a name="output-from-a-function"></a>Függvények kimenete
Ha a runbookban szereplő függvényből ír toohello kimeneti adatfolyamba, hello kimeneti hátsó toohello runbook lett átadva. Ha hello runbook kimeneti tooa változóhoz rendeli hozzá, majd a program nem készült toohello kimeneti adatfolyam. Írása tooany hello függvényen belül másik adatfolyamba ír toohello hello runbook megfelelő adatfolyamába.

Vegye figyelembe a következő példában a runbook hello.

    Workflow Test-Runbook
    {
        Write-Verbose "Verbose outside of function" -Verbose
        Write-Output "Output outside of function"
        $functionOutput = Test-Function
        $functionOutput

    Function Test-Function
     {
        Write-Verbose "Verbose inside of function" -Verbose
        Write-Output "Output inside of function"
      }
    }


hello kimeneti adatfolyama így hello runbook-feladat a következő lesz:

    Output inside of function
    Output outside of function

hello részletes adatfolyama így hello runbook-feladat a következő lesz:

    Verbose outside of function
    Verbose inside of function

Miután hello runbook közzétett, és azt megkezdése előtt, kell is bekapcsolja a részletes naplózást hello runbook beállításaiban rendelés tooget hello részletes adatfolyam-kimenetét.

### <a name="declaring-output-data-type"></a>Deklarálási kimeneti adatok típusát
Egy munkafolyamat megadható hello adatok típusa a kimeneti hello segítségével [OutputType attribútummal](http://technet.microsoft.com/library/hh847785.aspx). Ennek az attribútumnak nincs hatása futásidőben, de a runbook szerzők arra utal, hogy toohello hello várható kimenet hello runbook tervezési időben. Hello a runbookok eszközkészlete folyamatosan tooevolve, fontos tervezési időben elvárt kimeneti adattípus fontossága hello növekszik. Ennek eredményeképpen bevált gyakorlat az tooinclude Ez a nyilatkozat az Ön által létrehozott runbookokat.

Ez egy lista példa kimenet típusa:

* System.String
* System.Int32
* System.Collections.Hashtable
* Microsoft.Azure.Commands.Compute.Models.PSVirtualMachine

hello alábbi példában a runbook kimenete egy karakterlánc-objektum, és megadja az elvárt kimenettípust. Ha a runbook kimenete egy bizonyos típusú tömb, majd kell továbbra is meg hello típus megakadályozását tooan tömbként hello típusú.

    Workflow Test-Runbook
    {
       [OutputType([string])]

       $output = "This is some string output."
       Write-Output $output
    }

Adja meg egy kimeneti Grapical vagy grafikus PowerShell-munkafolyamati forgatókönyvek toodeclare, kiválaszthatja hello **bemeneti és kimeneti** menüpontra, majd hello típusú hello nevét a kimeneti típus.  Azt javasoljuk hello teljes .NET osztály neve toomake használja az egyszerűen azonosítható, ha a szülőrunbookból hivatkoznak rá.  Az adott osztály toohello adatbusz hello runbook összes hello tulajdonságok, és a használatuk feltételes logikához, a naplózás, és a hello runbook más tevékenységei értékként hivatkozó biztosítja a magas fokú rugalmasságot biztosít.<br> ![A Runbook bemeneti és kimeneti beállítás](media/automation-runbook-output-and-messages/runbook-menu-input-and-output-option.png)

A következő példa hello tudunk két grafikus forgatókönyvek toodemonstrate ezt a szolgáltatást.  Hello moduláris runbook hálózattervezési modell érvénybe lépni, hogy egy runbookot, amely hello *hitelesítési Runbook sablon* hitelesítéséhez az Azure használatával hello futtató fiók.  A második runbookot, amely általában hajtaná végre a hello core logika tooautomate egy adott forgatókönyv esetén, ebben az esetben lesz tooexecute hello *hitelesítési Runbook sablon* , és megjeleníti a hello eredmények tooyour **tesztelése** tesztkimenet ablaktáblán.  Normál körülmények azt kellene foglalkozhat elleni egy erőforrás-emelés hello kimeneti hello gyermekrunbook a runbookot.    

Itt látható az alapszintű logikája hello hello **AuthenticateTo-Azure** runbook.<br> ![Runbook-sablont példa hitelesítéséhez](media/automation-runbook-output-and-messages/runbook-authentication-template.png).  

Hello kimeneti típust tartalmazza *Microsoft.Azure.Commands.Profile.Models.PSAzureContext*, amely hello hitelesítési profiltulajdonságok ad vissza.<br> ![Runbook-kimenet típusa – példa](media/automation-runbook-output-and-messages/runbook-input-and-output-add-blade.png) 

Bár ez a forgatókönyv igen közvetlen, van egy konfigurációs elem toocall itt meg.  hello utolsó tevékenység végrehajtása történik hello **Write-Output** parancsmag és az írás hello profil adatok tooa $_ változó PowerShell-kifejezést használ hello **Inputobject** paraméter, amely szükséges parancsmag.  

Hello második forgatókönyv ebben a példában, nevű *teszt-ChildOutputType*, egyszerűen tudunk két tevékenység.<br> ![Példa gyermek típusú Runbook kimeneti](media/automation-runbook-output-and-messages/runbook-display-authentication-results-example.png) 

hello első tevékenység meghívja a hello **AuthenticateTo-Azure** hello fut a runbook és hello második tevékenység **Write-Verbose** hello parancsmagot **adatforrás** a  **Tevékenység kimeneti** és hello értéke **mező elérési útja** van **Context.Subscription.SubscriptionName**, amely hello környezetben kimenete hello meg  **AuthenticateTo-Azure** runbook.<br> ![Write-Verbose parancsmag paraméter adatforrás](media/automation-runbook-output-and-messages/runbook-write-verbose-parameters-config.png)    

Ennek kimenete hello hello előfizetés hello neve.<br> ![Teszt-ChildOutputType eredmények összesítése](media/automation-runbook-output-and-messages/runbook-test-childoutputtype-results.png)

A OneNote kapcsolatos hello kimeneti típusú vezérlő hello viselkedését.  Amikor beírja egy az hello kimeneti típust mezőben a hello bemeneti és kimeneti tulajdonságok panelére léphet, lehetősége van tooclick hello vezérlőn kívül, ahhoz, hogy a belépési toobe ismeri fel hello vezérlő beírása után.  

## <a name="message-streams"></a>Üzenet-adatfolyamok
Hello kimeneti adatfolyamokkal ellentétben az üzenet-adatfolyamok tervezett toocommunicate információk toohello felhasználókként szerepelnek. A különböző információtípusoknak saját üzenet-adatfolyamuk van, és minden Azure Automation a másképpen kezeli.

### <a name="warning-and-error-streams"></a>Figyelmeztetés és hiba adatfolyamok
Figyelmeztetés és hiba adatfolyamok hello egy runbook tervezett toolog problémák. Amikor egy runbook végrehajtásakor, és a hello Azure felügyeleti portál Tesztkimenet ablaktábláján hello szerepel egy runbook tesztelésekor az oktatóprogram toohello feladatelőzményekben. Alapértelmezés szerint hello runbook fog szakítják meg a figyelmeztető vagy hibaüzenetek. Megadhatja, hogy hello runbook kell fel legyen függesztve figyelmeztető vagy hibaüzenet úgy, hogy egy [preferenciaváltozót](#PreferenceVariables) hello runbook üdvözlőüzenetére létrehozása előtt. Egy hiba történt a runbook toosuspend toocause, mintha a kivétel, állítsa például **$ErrorActionPreference** tooStop.

Hozzon létre egy figyelmeztetés vagy hibaüzenet üzenetet hello segítségével [Write-Warning](https://technet.microsoft.com/library/hh849931.aspx) vagy [Write-Error](http://technet.microsoft.com/library/hh849962.aspx) parancsmag. A tevékenységek is írhatnak toothese adatfolyamokat.

    #hello following lines create a warning message and then an error message that will suspend hello runbook.

    $ErrorActionPreference = "Stop"
    Write-Warning –Message "This is a warning message."
    Write-Error –Message "This is an error message that will stop hello runbook because of hello preference variable."

### <a name="verbose-stream"></a>Részletes adatfolyam
hello részletes üzenet-adatfolyam hello runbook műveletekre vonatkozó általános információk van. Hello óta [hibakeresési adatfolyam](#Debug) nem érhető el egy runbook részletes üzenetek használja a rendszer hibakeresési információ. Alapértelmezés szerint a közzétett runbookok részletes üzenetei nem tárolódnak hello feladatelőzményekben. toostore részletes üzenetek, konfigurálja a közzétett runbookok tooLog részletes rekordok hello konfigurálása lapon hello runbook hello Azure felügyeleti portálon. A legtöbb esetben érdemes megtartani hello beállítás alapértelmezés szerint nem naplózza a teljesítményre vonatkozó megfontolásból runbook részletes rekordjait. Kapcsolja be ezt a beállítást csak tootroubleshoot, vagy a hibakeresési egy runbookot.

Ha [runbook tesztelése](automation-testing-runbook.md), részletes üzenet nem jelenik meg akkor is, ha hello runbook konfigurált toolog részletes rekordok. részletes üzenetek toodisplay közben [runbook tesztelése](automation-testing-runbook.md), hello $VerbosePreference változó tooContinue meg kell adni. A változó beállítása részletes üzenetek megjelennek hello hello Azure-portál Tesztkimenet ablaktábláján.

Hozzon létre egy részletes üzenetet hello segítségével [Write-Verbose](http://technet.microsoft.com/library/hh849951.aspx) parancsmag.

    #hello following line creates a verbose message.

    Write-Verbose –Message "This is a verbose message."

### <a name="debug-stream"></a>Hibakeresési adatfolyam
hello hibakeresési adatfolyam interaktív felhasználót számára készült, és nem használható a runbookok.

## <a name="progress-records"></a>Állapotrekordok
Ha konfigurál egy runbook toolog folyamatban (az hello konfigurálása lapon hello runbook hello Azure-portál) rögzíti, majd egy rekord fog szerepelni a feladatelőzményekben toohello előtt és után a tevékenység futtatása. A legtöbb esetben érdemes megtartani hello beállítás alapértelmezés szerint nem naplózza a runbook az állapotrekordok rendelés toomaximize teljesítményét. Kapcsolja be ezt a beállítást csak tootroubleshoot, vagy a hibakeresési egy runbookot. A runbook tesztelésekor hibaüzenetei nem jelennek meg, akkor is, ha hello runbook konfigurált toolog állapotrekordokat.

Hello [Write-Progress](http://technet.microsoft.com/library/hh849902.aspx) parancsmag érvénytelen runbookokban, mivel ez egy interaktív felhasználó való használatra készült.

## <a name="preference-variables"></a>Preferenciaváltozók
A Windows PowerShell használ [preferenciaváltozók](http://technet.microsoft.com/library/hh847796.aspx) toodetermine hogyan toorespond küldött toodata toodifferent kimenetét. Ezek a változók egy runbook toocontrol, hogyan válaszol a különböző adatfolyamokba küldött toodata állíthatja be.

hello következő táblázatban hello preferenciaváltozók használható a runbookok, valamint azok érvényes és alapértelmezett értékeit. Vegye figyelembe, hogy ez a táblázat csak tartalmaz hello értékek, amelyek érvényesek a runbookokban. Hello preferenciaváltozók Azure Automation kívül a Windows PowerShell további értékek érvényesek.

| Változó | Alapértelmezett érték | Érvényes értékek |
|:--- |:--- |:--- |
| WarningPreference |Folytatás |Leállítás<br>Folytatás<br>Folytatáscsendben |
| ErrorActionPreference |Folytatás |Leállítás<br>Folytatás<br>Folytatáscsendben |
| VerbosePreference |Folytatáscsendben |Leállítás<br>Folytatás<br>Folytatáscsendben |

a következő táblázat hello hello változó preferenciák, amelyek érvényesek a runbookokat hello viselkedése sorolja fel.

| Érték | Viselkedés |
|:--- |:--- |
| Folytatás |Üdvözlőüzenetére naplózza, és továbbra is fennáll, hello runbook futtatását. |
| Folytatáscsendben |Továbbra is fennáll, hello üzenet naplózása nélkül hello runbook futtatását. Ennek hatása hello üdvözlőüzenetére jelenti. |
| Leállítás |Üdvözlőüzenetére naplózza, és felfüggeszti a hello runbook. |

## <a name="retrieving-runbook-output-and-messages"></a>Runbook-kimenet és üzenetek lekérése
### <a name="azure-portal"></a>Azure Portal
A hello hello feladatok lapján a runbookok az Azure-portálon tekintheti hello a runbook-feladatok részleteit. hello hello feladat összegzését jeleníti meg hello bemeneti paraméterek és hello [kimeneti adatfolyamba](#Output) hozzáadása toogeneral információt hello feladat és a kivételek, ha azok történt. hello előzmények hello üzeneteit tartalmazzák [kimeneti adatfolyamba](#Output) és [figyelmeztető és Hibaadatfolyamok](#WarningError) hozzáadása toohello a [részletes adatfolyam](#Verbose) és [folyamatban Rekordok](#Progress) hello runbook esetén konfigurált toolog részletes és állapotrekordokat.

### <a name="windows-powershell"></a>Windows PowerShell
A Windows PowerShell parancssorába beolvasható-kimenet és üzenetek a runbookot hello [Get-AzureAutomationJobOutput](https://msdn.microsoft.com/library/mt603476.aspx) parancsmag. Ehhez a parancsmaghoz szükséges hello hello feladat Azonosítóját, és egy paraméterrel hívják mely adatfolyam tooreturn megadott adatfolyam. Bármely tooreturn hello feladathoz tartozó összes adatfolyamot adhatja meg.

a következő példa hello megkezdi a minta-runbookhoz és vár, amíg toocomplete. Ezt követően a kimeneti adatfolyamot a folyamat során gyűjtött hello feladat.

    $job = Start-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook"

    $doLoop = $true
    While ($doLoop) {
       $job = Get-AzureRmAutomationJob -ResourceGroupName "ResourceGroup01" `
       –AutomationAccountName "MyAutomationAccount" -Id $job.JobId
       $status = $job.Status
       $doLoop = (($status -ne "Completed") -and ($status -ne "Failed") -and ($status -ne "Suspended") -and ($status -ne "Stopped")
    }

    Get-AzureRmAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" -Id $job.JobId –Stream Output

### <a name="graphical-authoring"></a>Grafikus készítése
Grafikus forgatókönyvekhez további naplózás érhető el a tevékenység szintű nyomkövetésre hello formában.  Nyomkövetés két szint: Basic, és részletesen.  Alapszintű nyomkövetés, a hello start látható, és minden hello runbook és információk a tevékenység befejezési időpontja kapcsolatos tooany tevékenység újrapróbálkozások, például a kísérletek száma és hello tevékenység kezdési időpontja.  Részletes nyomkövetés, a kapott egyszerű nyomkövetést plusz bemeneti és kimeneti minden egyes tevékenységhez.  Vegye figyelembe, hogy jelenleg hello nyomkövetési rekord segítségével készül hello részletes adatfolyam, engedélyeznie kell a részletes naplózást, ha engedélyezi a nyomkövetést.  A kérelmek nyomon követése engedélyezve grafikus forgatókönyvekhez nincs szükség toolog állapotrekordokat, mert hello alapvető nyomkövetés szolgál hello ugyanerre a célra, és több adatot.

![Grafikus szerzői feladat adatfolyamokat megtekintése](media/automation-runbook-output-and-messages/job-streams-view-blade.png)

A fenti képernyőfelvételen hello látható, hogy ha engedélyezi a részletes naplózás és nyomkövetés grafikus forgatókönyvekhez, sokkal több adatot legyen hello éles feladat adatfolyamok megtekintése.  Lehet, hogy ez további információt a runbook üzemi kapcsolatos problémák alapvető, és ezért csak akkor engedélyezze az erre a célra, nem pedig egy általános gyakorlat.    
lehet, hogy különösen számos hello nyomkövetési rögzíti.  Grafikus forgatókönyvnek a nyomkövetés akkor kaphat két toofour rekordok száma attól függően, hogy konfigurálta az alap vagy részletes nyomkövetés tevékenység.  Ha nem kell az adatokat tootrack hello folyamatban van egy runbook hibaelhárítási, érdemes tookeep nyomkövetés ki van kapcsolva.

**tevékenység szintű tooenable nyomkövetés, hajtsa végre a lépéseket követve hello.**

1. Hello Azure portál nyissa meg az Automation-fiók.
2. Kattintson a hello **Runbookok** csempe tooopen hello forgatókönyvek listája.
3. A Runbookokat hello panelen kattintson a tooselect egy grafikus forgatókönyvnek a runbookok a listából.
4. A kiválasztott hello runbook hello beállítási paneljén kattintson **naplózás és nyomkövetés**.
5. Hello a naplózás és a nyomkövetés panelen, a részletes rekordok naplózása kattintson **a** tooenable részletes naplózást és udner tevékenység szintű nyomkövetés módosítása hello nyomkövetési szint túl**alapvető** vagy **részletes**  hello szintje szükséges nyomkövetés alapján.<br>
   
   ![Grafikus szerzői naplózás és nyomkövetés panel](media/automation-runbook-output-and-messages/logging-and-tracing-settings-blade.png)

### <a name="microsoft-operations-management-suite-oms-log-analytics"></a>A Microsoft Operations Management Suite (OMS) szolgáltatáshoz
Automatizálási runbook feladat állapotát és a feladat adatfolyamok tooyour a Microsoft Operations Management Suite (OMS) Naplóelemzési munkaterület küldhet.  A Naplóelemzési is,

* Az automatizálási feladatok insight letöltése 
* A runbook-feladat állapota (pl. felfüggesztett vagy sikertelen) alapuló riasztás vagy e-mail eseményindító 
* Speciális lekérdezéseket írhat a feladat adatfolyamok között 
* Feladatok összefüggéseket Automation-fiókok között 
* A feladatelőzmények megjelenítheti az adott idő alatt    

Naplóelemzési toocollect tooconfigure integrációja összefüggéseket és feladatadatok intézkedjen további információkért lásd: [feladat állapotát és a feladat adatfolyam továbbítása Automation tooLog szolgáltatás (OMS)](automation-manage-send-joblogs-log-analytics.md).

## <a name="next-steps"></a>Következő lépések
* További információk a runbook végrehajtása toomonitor runbook feladatokat, valamint egyéb technikai részleteket lásd: hogyan toolearn [nyomon követheti a runbook-feladatok](automation-runbook-execution.md)
* Hogyan toodesign és -felhasználási gyermekforgatókönyvek: toounderstand [gyermek az Azure Automation runbookjai](automation-child-runbooks.md)

