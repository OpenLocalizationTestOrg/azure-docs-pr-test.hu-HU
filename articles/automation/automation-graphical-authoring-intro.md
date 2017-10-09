---
title: "aaaGraphical készítése az Azure Automationben |} Microsoft Docs"
description: "Grafikus szerzői lehetővé teszi runbookok toocreate az Azure Automation kód használata nélkül. Ez a cikk ismerteti, hogy egy bevezető toographical szerzői, és minden hello részletek toostart grafikus runbook létrehozása szükséges."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 4b6f840c-e941-4293-a728-b33407317943
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/14/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 6ddf18b992d5e5f7f4af95f344007a63ac498549
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="graphical-authoring-in-azure-automation"></a>Grafikus készítése az Azure Automationben
## <a name="introduction"></a>Bevezetés
Grafikus szerzői műveletek lehetővé teszi runbookok toocreate az Azure Automation hello bonyolultságára hello alapul szolgáló Windows PowerShell vagy a PowerShell-munkafolyamati kód nélkül. Adja hozzá a tevékenységek toohello vászonra parancsmagok és a runbookok a könyvtárból, kapcsolja össze, és tooform munkafolyamat konfigurálása.  Ha valaha is dolgozott System Center Orchestrator és Service Management Automation (SMA), a megszokott tooyou kell kinéznie.   

A cikkben egy bevezető toographical jelentéskészítő és -hello elveket, tooget meg lett kezdve egy grafikus runbook létrehozása.

## <a name="graphical-runbooks"></a>Grafikus forgatókönyvek
Minden, az Azure Automation runbookjai Windows PowerShell-munkafolyamatok.  Grafikus és grafikus PowerShell-munkafolyamati forgatókönyvek hello Automation dolgozók futtatott PowerShell-kódot létrehozni, de nem képes tooview azt vagy módosítható közvetlenül.  Lehet, hogy egy grafikus forgatókönyvnek konvertált tooa grafikus PowerShell-munkafolyamati forgatókönyv és fordítva, de konvertált tooa szöveges forgatókönyvként nem. Egy meglévő szöveges forgatókönyvként hello grafikus szerkesztő nem lehet importálni.  

## <a name="overview-of-graphical-editor"></a>Grafikus szerkesztő áttekintése
Úgy is megnyithatja hello grafikus szerkesztő hello Azure-portálon a létrehozásakor vagy szerkesztésekor a grafikus forgatókönyv.

![Grafikus munkaterület](media/automation-graphical-authoring-intro/runbook-graphical-editor.png)

hello következő részek a hello vezérlők hello grafikus szerkesztőben.

### <a name="canvas"></a>Vászonra
hello vászonra, ahol a runbook tervezésekor.  Hello könyvtár vezérlő toohello runbook hello csomópontjáról tevékenységek hozzáadása, és csatlakoztassa őket a hivatkozások toodefine hello logikával hello runbook.

Hello vezérlőket használhatja hello vászonra toozoom hello alján növelésére és csökkentésére.

![Grafikus munkaterület](media/automation-graphical-authoring-intro/runbook-canvas-controls.png)

### <a name="library-control"></a>Szalagtár-vezérlő
Szalagtár vezérlő hello, ahol ki kell választania [tevékenységek](#activities) tooadd tooyour runbook.  Ha csatlakoztatás tooother tevékenységek toohello vászonra hozzáadja őket.  Ez magában foglalja a hello a következő táblázatban ismertetett négy részből áll.

| A szakasz | Leírás |
|:--- |:--- |
| Parancsmagok |Minden hello olyan parancsmagokat tartalmaz, használhatja a runbookban.  Parancsmagok modul szerint vannak rendszerezve.  Az automation-fiókban telepített hello modulok mindegyikének lesz elérhető. |
| Runbookok |Az automation-fiók runbookokat hello tartalmazza. Ezek a runbookok gyermek runbookként használt toohello vászonra toobe adhatók hozzá. Az azonos alapvető típus szerint szerkesztett runbook hello hello csak forgatókönyvek jelennek meg; a grafikus forgatókönyvek csak PowerShell-alapú forgatókönyvek jelennek meg, amíg a runbookok grafikus PowerShell-munkafolyamati forgatókönyvek csak PowerShell-munkafolyamat-alapú jelennek meg. |
| Objektumok |Hello tartalmaz [automation eszközök](http://msdn.microsoft.com/library/dn939988.aspx) az automation-fiókban a runbookban használható.  Amikor egy eszköz tooa runbook, felveszi egy munkafolyamat-tevékenységet, amely a kijelölt eszköz hello beolvasása.  Változó eszközök hello esetben kiválaszthatja e tooadd egy tevékenység tooget hello változó vagy beállítva hello változó. |
| Runbook-vezérlés |Runbook-vezérlés tevékenységek használható a jelenlegi runbook tartalmazza. A *csatlakozási* több bemeneti adatokat fogad, és megvárja, amíg az összes előtt folyamatos hello munkafolyamat befejeződött. A *kód* tevékenység fut, a PowerShell vagy a PowerShell-munkafolyamati kód hello grafikus forgatókönyvnek típusától függően egy vagy több sort.  Ez a tevékenység használhatja egyéni kód vagy egyéb tevékenységeket tartalmazó nehéz tooachieve funkcióit. |

### <a name="configuration-control"></a>Konfiguráció-ellenőrzés
hello konfiguráció-ellenőrzés, ahol részletek meg kell adnia a vásznon a hello kiválasztott objektumhoz. hello tulajdonságok érhetők el a vezérlő hello kijelölt objektum típusától függ.  Amikor kiválaszt egy beállítást hello konfiguráció-ellenőrzés, az további paneleken nyílik meg a rendelés tooprovide további információt.

### <a name="test-control"></a>Ellenőrző tesztelése
hello teszt vezérlő nem hello grafikus szerkesztő első indításakor jelenik meg. Mikor nyitja meg interaktív módon [grafikus forgatókönyv-tesztelési](#graphical-runbook-procedures).  

## <a name="graphical-runbook-procedures"></a>Grafikus forgatókönyv eljárások
### <a name="exporting-and-importing-a-graphical-runbook"></a>Egy grafikus forgatókönyv importálása és exportálása
Csak egy grafikus forgatókönyv közzétett változata hello exportálhatja.  Ha hello runbook még nem lett közzétéve, majd hello **közzétett exportálási** gomb le lesz tiltva.  Amikor rákattint hello **közzétett exportálási** gombra kattint, a hello runbook letöltött tooyour helyi számítógépen.  hello hello fájl egyezik hello névvel rendelkező hello runbook egy *graphrunbook* bővítmény.

![Közzétett exportálása](media/automation-graphical-authoring-intro/runbook-export.png)

Egy grafikus vagy grafikus PowerShell-munkafolyamati forgatókönyv fájl importálásához hello kiválasztásával **importálása** lehetőséget egy runbook felvételekor.   Ha hello fájl tooimport választja, megtarthatja hello azonos **neve** , vagy adjon meg egy új.  hello Runbook mezőben azt a kijelölt hello fájl, és ha megpróbál tooselect különböző típusú, amely nem megfelelő, egy üzenet számára jelenik meg a lehetséges ütközések, és az átalakítás során lehetnek megjegyezni értékelésére után jelenik meg hello típusú forgatókönyvet szintaktikai hibákat.  

![Forgatókönyv importálása](media/automation-graphical-authoring-intro/runbook-import-revised20165.png)

### <a name="testing-a-graphical-runbook"></a>Grafikus runbook tesztelése
Egy runbook vázlatverzióját hello tesztelheti hello Azure-portálon hello hagyja változatlanul hello runbook verziójának közzétett, vagy tesztelheti egy új runbookot, ahhoz, hogy közzé lett téve. Ez lehetővé teszi, hogy tooverify, amely hello runbook megfelelően működik-e hello közzétett verziót cseréje előtt. Egy runbook tesztelésekor hello vázlatát hajtja végre, és minden elvégzett műveletet végrehajt befejezését. Nincs feladatelőzményekben létrejött, de kimeneti hello Tesztkimenet ablaktáblán jelenik meg. 

Nyissa meg a hello teszt egy runbook-vezérlés szerkesztése hello runbook megnyitásával, és kattintson a hello **teszt ablaktábla** gombra.

![Teszt gomb](media/automation-graphical-authoring-intro/runbook-edit-test-pane.png)

hello teszt vezérlő arra fogja kérni a bemeneti paramétereket, és hello runbook indításához kattintson a hello **Start** gombra.

![Teszt gomb](media/automation-graphical-authoring-intro/runbook-test-start.png)

### <a name="publishing-a-graphical-runbook"></a>Grafikus runbook közzététele
Az Azure Automationben minden runbook rendelkezik, egy Piszkozat és egy közzétett verziója. Csak a hello közzétett verzió elérhető toobe futtatni, és csak a hello piszkozat verzió szerkeszthető. hello közzétett verzió nem bármely módosítások toohello vázlatként megjelölt verziót. Ha hello vázlatként megjelölt verziót készen toobe érhető el, majd közzététel amely hello közzétett verzió felülírja hello vázlatként megjelölt verziót.

Nyissa meg szerkesztésre, és kattintson a hello hello forgatókönyv grafikus forgatókönyv közzéteheti **közzététel** gombra.

![Közzététel gomb](media/automation-graphical-authoring-intro/runbook-edit-publish.png)

Amikor egy runbook még nem lett közzétéve, állapotba került **új**.  A közzétett, állapotba került **közzétett**.  Ha manuálisan szerkeszti hello runbook után azt is közzé lett téve, és hello Vázlat és egy közzétett verzió eltérőek, hello runbook állapotba került **szerkesztési**.

![A Runbook állapota](media/automation-graphical-authoring-intro/runbook-statuses-revised20165.png) 

Akkor is hello beállítás toorevert toohello közzétett verzió egy runbook.  Ez a jelez számítógépnél hello runbook legutóbbi közzétételének, és lecseréli hello runbook vázlatverzióját hello hello közzétett verzió óta végrehajtott módosításokat.

![Állítsa vissza a toopublished gomb](media/automation-graphical-authoring-intro/runbook-edit-revert-published.png)

## <a name="activities"></a>Tevékenységek
Tevékenységek építőelemei hello egy runbookot.  Egy tevékenység lehet egy PowerShell-parancsmag, a gyermek runbook vagy a munkafolyamat-tevékenységet.  Jobb gombbal kattintson rá hello könyvtár vezérlő, majd válassza hozzáadhat egy tevékenységhez toohello runbook **toocanvas hozzáadása**.  Ezután kattintson és húzza hello tevékenység tooplace azt bárhol hello a vászonra, hogy hasonlóan.  hello hello vásznon a hello tevékenység hello helye nincs hatással az bármely módon hello runbook hello művelet.  Is elrendezés a runbook azonban legmegfelelőbb toovisualize találja meg a műveletet. 

![Toocanvas hozzáadása](media/automation-graphical-authoring-intro/add-to-canvas-revised20165.png)

Válassza ki a hello vászonra tooconfigure hello tevékenységet a tulajdonságok és a paraméterek hello konfigurációs panelen.  Módosíthatja a hello **címke** a hello tevékenység toosomething, amely leíró tooyou.  hello eredeti parancsmag továbbra is fut, a megjelenített név hello grafikus szerkesztőben használandó egyszerűen módosítani.  hello címke hello runbook belül egyedinek kell lennie. 

### <a name="parameter-sets"></a>A paraméter beállítása
A paraméterhalmaz hello kötelező és választható paramétereit, amely elfogadja a parancsmag adott értékek határozza meg.  Az összes parancsmag be legalább egy paramétere lehet, és több rendelkeznek.  Ha a parancsmag több paraméterkészletei, majd ki kell választania melyiket paraméterek konfigurálása előtt fogja használni.  hello paraméterek konfigurálható az Ön által hello paraméterhalmaz függ.  Módosíthatja a hello paraméterkészletet alakítanak kiválasztásával egy tevékenység által használt **paraméter** és egy másik készlet kiválasztása.  Ebben az esetben konfigurált paraméterértékeket elvesznek.

A következő példa hello hello Get-AzureRmVM parancsmag három paraméterkészletei tartozik.  A paraméterértékek nem konfigurálható, amíg ki nem választ egy hello paraméterkészletei.  hello ListVirtualMachineInResourceGroupParamSet paraméterhalmaz ad vissza az összes virtuális gép egy erőforráscsoportban, és egy nem kötelező paraméter tartozik.  hello GetVirtualMachineInResourceGroupParamSet van hello virtuális gép kívánt tooreturn, és két kötelező és egy nem kötelező paraméter megadásával.

![A paraméterhalmaz](media/automation-graphical-authoring-intro/get-azurermvm-parameter-sets.png)

#### <a name="parameter-values"></a>A paraméterértékek
Amikor megad egy paraméter értékét, ki kell választania egy data source toodetermine hogyan hello értéket kell megadni.  egy adott paraméter elérhető hello adatforrások, hogy a paraméter érvényes értékei hello függ.  Null például nem lesz elérhető lehetőségként az egyik paraméter, amely nem engedélyezi a null értékeket.

| Adatforrás | Leírás |
|:--- |:--- |
| Állandó érték |Írja be a hello paraméter értékét.  Ez a tulajdonság csak a következő adattípusokat hello érhető el: Int32, Int64, String, Boolean, DateTime, kapcsoló. |
| Tevékenység kimeneti |Egy tevékenység, amely megelőzi a jelenlegi tevékenység hello hello munkafolyamat kimenetét.  Az összes érvényes tevékenység megjelenik.  Válassza ki a csak a hello tevékenység toouse hello paraméterérték kimenetét.  Ha hello tevékenység kimenete több tulajdonságait azonosítójú objektum, majd beírhatja a hello tulajdonság hello neve hello tevékenység kiválasztása után. |
| A Runbook bemeneti |Jelölje ki a forgatókönyv bemeneti paramétert bemeneti toohello tevékenység paraméterként. |
| Változóeszköz |Válassza ki a egy automatizálási változó bemeneti adatként. |
| Hitelesítőadat-eszköz |Válassza ki az automatizálási hitelesítő adatok bemeneti adatként. |
| Tanúsítványeszköz |Válassza ki az Automation-tanúsítvány bemeneti adatként. |
| Kapcsolat eszköz |Válassza ki az automatizálási kapcsolat bemeneti adatként. |
| PowerShell-kifejezés |Adja meg az egyszerű [PowerShell-kifejezést](#powershell-expressions).  hello kifejezéssel kiértékelendő előtt hello tevékenység és hello eredmény hello paraméter értékét használja.  Változók toorefer toohello kimeneti tevékenységet vagy egy runbook bemeneti paraméter használható. |
| Nincs konfigurálva |Törli a korábban beállított értéket. |

#### <a name="optional-additional-parameters"></a>További választható paraméterek:
Az összes parancsmag lesz hello beállítás tooprovide további paramétereket.  Ezek a PowerShell általános paramétereket, illetve más egyéni paramétereket.  A szövegmezőben, ahol megadhatja a paramétereket a PowerShell-szintaxis jelenik meg.  Például toouse hello **részletes** általános paraméter kellene megadnia **"-Verbose: $True"**.

### <a name="retry-activity"></a>Ismételje meg a tevékenység
**Újrapróbálási viselkedés** lehetővé teszi, hogy egy tevékenység toobe többször is lefuthat, amíg egy bizonyos feltétel nem teljesül, hasonlóan a hurok.  A szolgáltatás használható, amely többször kell futtatnia, nagyon eséllyel fordulnak elő hiba tevékenységek és előfordulhat, hogy kell egynél több kísérlet történt a sikeres, vagy az érvényes adatok hello tevékenység kimeneti információi hello tesztelése.    

Ha egy tevékenység újra engedélyezi, beállíthatja a késést és egy feltétel.  hello késleltetési idő legyen hello idő (másodperceken vagy perceken kifejezve), hogy hello lépne, megvárja hello tevékenység ismételt futtatása előtt.  Ha késleltetés meg van adva, majd hello tevékenység fog futni újra befejezését követően azonnal. 

![Tevékenység újrapróbálkozási késleltetést](media/automation-graphical-authoring-intro/retry-delay.png)

hello újrapróbálkozási feltétele, miután minden alkalommal hello tevékenység kiértékelt PowerShell-kifejezést.  Ha hello kifejezés tooTrue mutat, majd hello tevékenység, ismét elindul.  Ha hello kifejezés megoldja tooFalse hello tevékenység nem futtassa újból a, és hello runbook toohello következő tevékenység helyezi át. 

![Tevékenység újrapróbálkozási késleltetést](media/automation-graphical-authoring-intro/retry-condition.png)

hello újrapróbálkozási feltétel használható hozzáférés tooinformation kapcsolatos hello tevékenység-újrapróbálkozások biztosító $RetryData nevű változó.  Ez a változó a következő táblázat hello hello tulajdonságokkal rendelkezik.

| Tulajdonság | Leírás |
|:--- |:--- |
| NumberOfAttempts |Ennyiszer hello tevékenység futott. |
| Kimenet |Hello kimenete legutóbbi futtatás hello tevékenység. |
| Teljesidőtartam |Túllépte az időkorlátot hello tevékenység kezdete óta eltelt hello első alkalommal. |
| StartedAt |Ideje UTC formátumban hello tevékenység az első elindítása. |

Az alábbiakban példák tevékenység ismételje meg a feltételeket.

    # Run hello activity exactly 10 times.
    $RetryData.NumberOfAttempts -ge 10 

    # Run hello activity repeatedly until it produces any output.
    $RetryData.Output.Count -ge 1 

    # Run hello activity repeatedly until 2 minutes has elapsed. 
    $RetryData.TotalDuration.TotalMinutes -ge 2

Egy tevékenység újrapróbálkozási feltétel konfigurálása után hello tevékenység két vizuális jelek tooremind tartalmazza-e meg.  Egy van hello tevékenység és más hello amikor hello konfigurációs hello tevékenység.

![Tevékenység újrapróbálkozási Visual mutatók](media/automation-graphical-authoring-intro/runbook-activity-retry-visual-cue.png)

### <a name="workflow-script-control"></a>Munkafolyamat-parancsprogram-vezérlők
Forráskód-kezelési egy, speciális tevékenységen, amely fogadja a PowerShell vagy a PowerShell-munkafolyamati parancsprogram attól függően, hogy hello típusa grafikus forgatókönyv alatt létrehozott rendelés tooprovide működésének egyébként nem érhető el.  Nem fogadnak el paramétereket, de használni tudja változók tevékenység kimeneti és runbook-bemeneti paraméterekhez.  Hello tevékenység kimenete kerül toohello adatbuszba kimenő kapcsolat, amelyben az eset mindaddig van toohello kimenet hozzáadva hello runbook.

Például hello alábbira számításokat dátum $NumberOfDays nevű runbook bemeneti változóra használatával.  Kimeneti toobe hello runbook ezt követő tevékenységei által használt, a számítás dátuma és időpontja ezután elküldi.

    $DateTimeNow = (Get-Date).ToUniversalTime()
    $DateTimeStart = ($DateTimeNow).AddDays(-$NumberOfDays)}
    $DateTimeStart


## <a name="links-and-workflow"></a>Hivatkozások és a munkafolyamat
A **hivatkozás** egy grafikus runbook összeköti a két tevékenység.  Hello tevékenység toohello cél forrástevékenység mutató nyíl, megjelenik a vásznon a hello.  hello tevékenységek hello irányban hello nyíl hello céltevékenységre indítása hello forrástevékenység befejeződését követően futtassa.  

### <a name="create-a-link"></a>Hivatkozás létrehozása
Hozzon létre két tevékenység által kiválasztásával hello forrástevékenység és gombra kattintva hello kör közötti kapcsolatot hello alakzat hello alján.  Húzza a hello nyíl toohello céltevékenységre és kiadása.

![Hivatkozás létrehozása](media/automation-graphical-authoring-intro/create-link-revised20165.png)

Válassza ki hello hivatkozás tooconfigure tulajdonságát hello konfigurációs panelen.  Ebbe beleértendők a hello kapcsolattípus, aminek ismertetését a következő táblázat hello.

| Kapcsolat típusa | Leírás |
|:--- |:--- |
| Folyamat |hello céltevékenységre van egyszeri futtatás minden objektum kimeneti hello forrás tevékenységből.  hello cél tevékenység nem működik, ha hello forrástevékenység nincs kimenet eredményez.  Hello forrástevékenység kimeneti objektumként érhető el. |
| Feladatütemezési |hello céltevékenységre csak egyszer fut le.  Hello forrástevékenység kap egy objektumokból álló tömb.  Hello forrástevékenység kimeneti objektumokat tömbként érhető el. |

### <a name="starting-activity"></a>Kezdő tevékenység
Egy grafikus forgatókönyvnek a tevékenységeket, amelyek nem rendelkeznek olyan bejövő hivatkozás indul el.  Ez gyakran lesz csak egy tevékenység hello indítása tevékenység hello runbook volna el.  Ha több tevékenység nem rendelkezik olyan bejövő hivatkozás, majd hello runbook indul el, párhuzamosan futtatásával.  Azt fogja majd kövesse hello hivatkozások toorun más tevékenységek egyes befejeződik.

### <a name="conditions"></a>Feltételek
Ha egy feltételt ad meg egy hivatkozást, hello céltevékenységre csak akkor fut, ha hello feltétel tootrue oldja fel.  Egy feltétel tooretrieve hello kimenet hello forrástevékenység általában egy $ActivityOutput változó fogja használni.  

Egy folyamatkapcsolódása egyetlen objektumhoz olyan feltételt, és hello feltétel kiértékelése minden objektum kimeneti hello forrástevékenység.  hello céltevékenységre ezután minden objektum, amely eleget tesz a hello feltétel fut.  Például a Get-AzureRmVm forrástevékenység, a szintaxis a következő hello felhasználhatók egy feltételes csővezeték hivatkozás tooretrieve csak virtuális gépek hello nevű erőforráscsoport *csoport1*.  

    $ActivityOutput['Get Azure VMs'].Name -match "Group1"

A feladatütemezési hivatkozásoknál hello feltétel csak értékeli egyszer óta az összes objektumok kimeneti hello forrástevékenység tartalmazó egyetlen tömböt adott vissza.  Emiatt egy feladatütemezési hivatkozás nem használható, például egy folyamatkapcsolódása szűréshez, de meghatározzák, hogy egyszerűen a hello következő tevékenység fut-e. Például a virtuális gép elindítása a runbook-tevékenység a következő hello érvénybe.<br> ![A feladatütemezések feltételes hivatkozás](media/automation-graphical-authoring-intro/runbook-conditional-links-sequence.png)<br>
Három különböző kapcsolat tootwo runbook bemeneti paraméterek nevet a virtuális gép és az erőforráscsoport neve rendelés toodetermine hello megfelelő lépéseket tootake egyben a jelölő megadott értékek - elindítása egy virtuális ellenőrzését, hello minden virtuális gép elindítása erőforrás-csoport, vagy egy előfizetésben található összes virtuális gépet.  Hello feladatütemezési hivatkozás Connect tooAzure és Get egyetlen virtuális gép között ez hello feltétel programot:

    <# 
    Both VMName and ResourceGroupName runbook input parameters have values 
    #>
    (
    (($VMName -ne $null) -and ($VMName.Length -gt 0))
    ) -and (
    (($ResourceGroupName -ne $null) -and ($ResourceGroupName.Length -gt 0))
    )

Egy feltételes hivatkozás használatakor hello-adatok a hello forrás tevékenység tooother tevékenységek a fiókirodában lévő hello feltétel szűri.  Ha a tevékenység hello forrás toomultiple hivatkozások, majd hello adatokat minden egyes fiókiroda elérhető tooactivities toothat fiókirodai hello kapcsolat feltételének hello függ.

Például hello **Start-AzureRmVm** az alábbi hello runbook tevékenysége az összes virtuális gép elindul.  Két feltételes hivatkozások rendelkezik.  hello első feltételes hivatkozás hello kifejezést használ *$ActivityOutput ["Start-AzureRmVM"]. IsSuccessStatusCode - eq $true* toofilter Ha hello Start-AzureRmVm tevékenység sikeresen befejeződött.  hello második hello kifejezést használ, *$ActivityOutput ["Start-AzureRmVM"]. IsSuccessStatusCode - ne $true* toofilter, ha a Start-AzureRmVm hello tevékenység toostart hello virtuális gépet nem sikerült.  

![Feltételes hivatkozás – példa](media/automation-graphical-authoring-intro/runbook-conditional-links.png)

Minden olyan tevékenységet, amely a következő hello első hivatkozás, és használja a Get-AzureVM hello tevékenység kimenete csak kap, amely a Get-AzureVM futtatott hello időpontban indított hello virtuális gépek.  Minden olyan tevékenységet, amely a következő hello második hivatkozásra csak kap hello hello virtuális gépek leállított Get-AzureVM futtatott hello időpontban.  Hello harmadik hivatkozás a következő tevékenység megkapja az összes virtuális gép függetlenül a futó állapotot.

### <a name="junctions"></a>Elhelyezni pontokra
Elágazás egy megvárja, amíg az összes bejövő ágak befejeződött, speciális tevékenységen.  Ez lehetővé teszi, hogy toorun párhuzamosan több tevékenységet, és győződjön meg arról, hogy az összes lépés előtt végrehajtotta.

Míg a szinkronizációs pont a Bejövő hivatkozások korlátlan számú, a hivatkozások nem egynél több folyamat lehet.  nem korlátozott a bejövő sorozat hivatkozások hello száma.  Toocreate hello csatlakozási folyamat mutató hivatkozásokat tartalmaz több bejövő engedélyezett lesz, és mentse hello runbook, de nem futtatásakor.

az alábbi példa hello elindul a virtuális gépek halmaza, miközben egyidejűleg letöltése javítások toobe alkalmazott toothose gépek runbook részét képezi.  Elágazás használt tooensure, hogy mindkét folyamat befejeződött hello runbook folytatása előtt.

![Szinkronizációs pont](media/automation-graphical-authoring-intro/runbook-junction.png)

### <a name="cycles"></a>Ciklus
A ciklus akkor, ha a céltevékenységre hivatkozik vissza tooits forrás-, hogy végül hivatkozások biztonsági tooits forrás tevékenység vagy tooanother tevékenység.  Ciklus jelenleg nem engedélyezettek grafikus szerzői.  Ha a runbook rendelkezik ciklust, megfelelően menti, de egy hibaüzenetet fog kapni, amikor fut az.

![Ciklus](media/automation-graphical-authoring-intro/runbook-cycle.png)

### <a name="sharing-data-between-activities"></a>Tevékenységek adatok megosztása
Olyan adatot, amely által egy kimenő kapcsolat nélküli tevékenységet írása toohello *adatbuszba* hello runbook.  Hello runbook minden tevékenysége is adatok hello adatbuszba toopopulate paraméterértékeket a vagy használni a parancsprogramkódot.  Egy tevékenység bármely hello munkafolyamat korábbi tevékenységének kimenete hello érheti el.     

Hogyan hello adatot ír toohello adatbuszba függ hello hello tevékenység hivatkozásra kattintva.  Az egy **csővezeték**, hello adatok egy kimenet Többszörösök objektumként.  Az egy **feladatütemezési** társítani, hello adatok egy tömbként kimenet.  Ha csak egy érték, egy egyszeres elemnek tömbként kimeneti lesz.

A két módszer egyikével hello adatbuszba adatok érheti el.  Először használ egy **tevékenység kimeneti** adatforrás-toopopulate egy másik tevékenység paraméter.  Ha hello kimeneti objektum, megadhat egy-egy tulajdonság.

![Tevékenység kimeneti](media/automation-graphical-authoring-intro/activity-output-datasource-revised20165.png)

Tevékenység hello kimeneti is le egy **PowerShell-kifejezést** adatforrás vagy egy **munkafolyamat-parancsfájl** egy ActivityOutput változó tevékenységet.  Ha hello kimeneti objektum, megadhat egy-egy tulajdonság.  ActivityOutput változók hello a következő szintaxist használja.

    $ActivityOutput['Activity Label']
    $ActivityOutput['Activity Label'].PropertyName 

### <a name="checkpoints"></a>Az ellenőrzőpontok
Beállíthatja azt [ellenőrzőpontokat](automation-powershell-workflow.md#checkpoints) egy grafikus PowerShell-munkafolyamati forgatókönyv kiválasztásával a *ellenőrzőpont runbook* olyan tevékenységeket.  Ez azt eredményezi, hogy egy ellenőrzőpont toobe hello tevékenység futtatását követően állítsa be.

![Ellenőrzőpont](media/automation-graphical-authoring-intro/set-checkpoint.png)

Ellenőrzőpontok csak engedélyezett grafikus PowerShell munkafolyamat-forgatókönyvekről, a grafikus forgatókönyvek nem érhető el.  Ha hello runbook Azure-parancsmagokat használ, kövesse az Add-AzureRMAccount bármely alkulcsaihoz tevékenységet abban az esetben hello runbook fel van függesztve, és újraindítja a ezt az ellenőrzőpontot a különböző munkavégző. 

## <a name="authenticating-tooazure-resources"></a>TooAzure erőforrások hitelesítéséhez
Azure-erőforrások kezeléséhez, az Azure Automation Runbookjai hitelesítési tooAzure van szükség.  Hello [Futtatás mint fiók](automation-offering-get-started.md#creating-an-automation-account) (is hivatkozott tooas, egy egyszerű szolgáltatásnév) hello alapértelmezett metódus tooaccess Azure Resource Manager erőforrást az előfizetésében az Automation-forgatókönyv.  A funkció tooa grafikus forgatókönyv hello hozzáadásával is hozzáadhat **AzureRunAsConnection** kapcsolódási eszköz, amely hello PowerShell használ [Get-AutomationConnection](https://technet.microsoft.com/library/dn919922%28v=sc.16%29.aspx) parancsmagot, és [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) parancsmag toohello vászonra. Ezt szemlélteti az alábbi példa hello.<br>![A futtató hitelesítési tevékenységek](media/automation-graphical-authoring-intro/authenticate-run-as-account.png)<br>
hello beolvasása Futtatás mint a kapcsolat tevékenység (pl. Get-AutomationConnection) nevű AzureRunAsConnection konstans adatforrás van konfigurálva.<br>![Futtatás mint kapcsolat konfigurációja](media/automation-graphical-authoring-intro/authenticate-runas-parameterset.png)<br>
hello következő tevékenység, Add-AzureRmAccount, használatra hitelesített hello futtató fiókot ad a hello runbook.<br>
![Adja hozzá-AzureRmAccount paraméterkészletet alakítanak](media/automation-graphical-authoring-intro/authenticate-conn-to-azure-parameter-set.png)<br>
Hello paraméterek **APPLICATIONID**, **CERTIFICATETHUMBPRINT**, és **TENANTID** szüksége lesz hello tulajdonság toospecify hello neve hello mező elérési útja mert hello tevékenység kimenete több tulajdonságait azonosítójú objektum.  Ellenkező esetben hello runbook végrehajtásakor, tett kísérlet során tooauthenticate sikertelen lesz.  Ez azért, a minimális tooauthenticate rendelkező hello a runbookot futtató fiók szükséges.

toomaintain visszamenőleges kompatibilitási előfizetők felvételéhez hozott létre az automatizálási fiók használatával egy [Azure AD-felhasználó fiók](automation-create-aduser-account.md) toomanage Azure klasszikus üzembe helyezési vagy az Azure Resource Manager erőforrások hello metódus tooauthenticate az Add-AzureAccount hello parancsmagot egy [hitelesítőadat-eszköz](automation-credentials.md) , amely az Active Directory-felhasználó, az Azure-fiók hozzáférési toohello jelöli.

A funkció tooa grafikus forgatókönyv adja hozzá a hitelesítő adatok eszköz toohello vásznon Add-AzureAccount tevékenység után is hozzáadhat.  Adja hozzá AzureAccount a bemeneti hello credential tevékenység használja.  Ezt szemlélteti az alábbi példa hello.

![Hitelesítési tevékenységek](media/automation-graphical-authoring-intro/authentication-activities.png)

Tooauthenticate állandóan hello start hello runbook és minden ellenőrzőpont után.  Ez azt jelenti, továbbá Add-AzureAccount tevékenység hozzáadása után bármely ellenőrzőpont-munkafolyamat tevékenysége. Nincs szükség az hozzáadása hitelesítő adatát tevékenységnek, mivel használható hello azonos 

![Tevékenység kimeneti](media/automation-graphical-authoring-intro/authentication-activity-output.png)

## <a name="runbook-input-and-output"></a>A forgatókönyv bemeneti és kimeneti
### <a name="runbook-input"></a>A Runbook bemeneti
Egy runbook bemeneti vagy a felhasználó igényelhet, amikor elindítja hello runbookot hello Azure-portálon keresztül, vagy egy másik runbookból hello jelenlegivel gyermek használata.
Például ha egy runbookot, amely létrehoz egy virtuális gépet, szükség lehet tooprovide adatokat például hello nevét hello virtuális gép és egyéb tulajdonságokat hello runbook minden egyes indításakor.  

Elfogadja a runbook a megadott meghatározhat egy vagy több bemeneti paraméterek.  Ezek a paraméterek minden alkalommal hello runbook indítását értékeket ad meg.  Amikor elindít egy forgatókönyvet az Azure-portálon hello, felszólítja tooprovide értékeinek hello minden hello runbook bemeneti paramétereket.

Egy runbook bemeneti paraméterek hello kattintva érheti **bemeneti és kimeneti** hello runbook gombjára.  

![A Runbook bemeneti](media/automation-graphical-authoring-intro/runbook-edit-input-output.png) 

Ekkor megnyílik a hello **bemeneti és kimeneti** vezérlő, ahol szerkesztheti a meglévő bemeneti paramétert, vagy hozzon létre egy újat kattintva **bemenet hozzáadása**. 

![Bemenet hozzáadása](media/automation-graphical-authoring-intro/runbook-edit-add-input.png)

Minden egyes bemeneti paraméter a következő táblázat hello hello tulajdonságok határozzák meg.

| Tulajdonság | Leírás |
|:--- |:--- |
| Név |hello hello paraméternek egyedi neve.  Ez csak alfanumerikus karaktereket tartalmazhat, és nem tartalmazhat szóközt. |
| Leírás |Hello bemeneti paraméter nem kötelező leírása. |
| Típus |Hello paraméterértéket várt adattípus.  hello Azure-portálon biztosít egy megfelelő vezérlő hello adattípus minden paraméter beviteli során. |
| Kötelező |Meghatározza, hogy egy érték hello paramétert meg kell adni.  hello runbook nem indítható el, ha nem ad meg értéket minden kötelező paraméter, amely nem rendelkezik alapértelmezett értékkel. |
| Alapértelmezett érték |Itt adhatja meg, milyen értéket hello paraméter akkor használatos, ha nincs megadva.  Ez lehet Null vagy egy adott értéket. |

### <a name="runbook-output"></a>Runbook kimenete
Egy kimenő hivatkozás nem rendelkező tevékenység által létrehozott adatokat megkapja toohello [hello runbook kimeneti](http://msdn.microsoft.com/library/azure/dn879148.aspx).  hello kimeneti hello runbook-feladat mentik, és elérhető tooa szülőrunbook hello runbook gyermek használata esetén.  

## <a name="powershell-expressions"></a>PowerShell kifejezések
Egy grafikus szerzői hello előnyei biztosítja hello képességét toobuild egy runbook PowerShell alapos ismeretére.  Jelenleg kell tooknow PowerShell bit, ha az egyes feltöltését szolgáló [paraméterértékek](#activities) és beállítás [hivatkozási feltételek](#links-and-workflow).  Ez a témakör egy gyors bevezetés tooPowerShell kifejezések azoknak a felhasználóknak, akik tisztában van, nem lehet.  PowerShell teljes részletek [a Windows PowerShell parancsfájlok](http://technet.microsoft.com/library/bb978526.aspx). 

### <a name="powershell-expression-data-source"></a>PowerShell kifejezés adatforrása
Használhatja a PowerShell-kifejezést egy forrás toopopulate hello értékeként az egy [tevékenység-paraméter](#activities) hello eredményekkel néhány PowerShell-kód.  Ennek oka lehet egy egyszerű szövegsor néhány egyszerű függvény vagy több sor néhány komplex logikai végző kódot.  Bármely olyan kimenete egy parancsot, amely nincs hozzárendelve tooa változó kimeneti toohello paraméter értékét. 

Például a következő parancs hello volna kimenő hello aktuális dátumot. 

    Get-Date

hello következő parancsok hozza létre a hello karakterlánc aktuális dátum és rendelje hozzá tooa változó.  hello változó hello tartalmát majd kerülnek toohello kimeneti 

    $string = "hello current date is " + (Get-Date)
    $string

hello következő parancsok kiértékelése hello aktuális dátumot és jelző hello aktuális napon a hétvégi vagy a hét napja adja vissza. 

    $date = Get-Date
    if (($date.DayOfWeek = "Saturday") -or ($date.DayOfWeek = "Sunday")) { "Weekend" }
    else { "Weekday" }


### <a name="activity-output"></a>Tevékenység kimeneti
egy előző tevékenység runbookban hello használata hello $ActivityOutput változó a következő szintaxist hello toouse hello kimenetét.

    $ActivityOutput['Activity Label'].PropertyName

Például előfordulhat, hogy egy tevékenység, hello egy virtuális gép nevét, amelyhez tulajdonsággal ebben az esetben használhatja a következő kifejezés hello.

    $ActivityOutput['Get-AzureVm'].Name

Ha hello tulajdonság szükséges hello virtuális gép objektum helyett csak egy tulajdonság, akkor alakítanák vissza hello hello teljes objektum használatával, a következő szintaxist.

    $ActivityOutput['Get-AzureVm']

Használhatja a hello kimeneti tevékenység, amely összefűzi a szöveg toohello virtuálisgép neve a következő hello összetett kifejezésben.

    "hello computer name is " + $ActivityOutput['Get-AzureVm'].Name


### <a name="conditions"></a>Feltételek
Használjon [összehasonlító operátorok](https://technet.microsoft.com/library/hh847759.aspx) toocompare értékeket, vagy határozza meg, ha egy értéke megegyezik-e egy adott mintával.  Összehasonlítás $true vagy $false értéket ad vissza.

Például hello feltétel a következő határozza meg, hogy a virtuális gép hello tevékenység nevű *Get-AzureVM* jelenleg *leállt*. 

    $ActivityOutput["Get-AzureVM"].PowerState –eq "Stopped"

hello alábbi feltételeket ellenőrzi, hogy hello ugyanahhoz a virtuális géphez állapotban van. semmilyen más, mint *leállt*.

    $ActivityOutput["Get-AzureVM"].PowerState –ne "Stopped"

Több feltételt is összekapcsolhat egy [logikai operátor](https://technet.microsoft.com/library/hh847789.aspx) például **- és** vagy **- vagy**.  Például a következő hello feltétel ellenőrzések hello hello előző példában szereplő ugyanaz a virtuális gép olyan állapotban van, hogy *leállt* vagy *leállítása*.

    ($ActivityOutput["Get-AzureVM"].PowerState –eq "Stopped") -or ($ActivityOutput["Get-AzureVM"].PowerState –eq "Stopping") 


### <a name="hashtables"></a>Szórótáblában
[Szórótáblában](http://technet.microsoft.com/library/hh847780.aspx) név/érték párok, amelyek hasznosak lehetnek egy értékhalmazt ad vissza a rendszer.  Az egyes tevékenységek tulajdonságok ettől a egy kivonattáblát egy egyszerű érték helyett.  Kivonattábla említett tooas dictionary is találkozhat. 

A szintaxis a következő hello létrehozni egy kivonattáblát.  Egy kivonattáblát tetszőleges számú bejegyzést tartalmazhat, de egyes határozzák meg a nevét és értékét.

    @{ <name> = <value>; [<name> = <value> ] ...}

Például hello következő kifejezés hoz létre a kivonattábla toobe hello adatforrás szerepel egy tevékenység-paraméter várt egy kivonattáblát egy internetes keresési értékekkel.

    $query = "Azure Automation"
    $count = 10
    $h = @{'q'=$query; 'lr'='lang_ja';  'count'=$Count}
    $h

hello alábbi példában nevű tevékenység *Twitter kapcsolat beolvasása* toopopulate egy kivonattáblát.

    @{'ApiKey'=$ActivityOutput['Get Twitter Connection'].ConsumerAPIKey;
      'ApiSecret'=$ActivityOutput['Get Twitter Connection'].ConsumerAPISecret;
      'AccessToken'=$ActivityOutput['Get Twitter Connection'].AccessToken;
      'AccessTokenSecret'=$ActivityOutput['Get Twitter Connection'].AccessTokenSecret}



## <a name="next-steps"></a>Következő lépések
* a PowerShell munkafolyamat-forgatókönyvekről, használatába tooget lásd: [az első PowerShell-munkafolyamati forgatókönyv](automation-first-runbook-textual.md) 
* Grafikus forgatókönyvek használatába tooget lásd [saját első grafikus forgatókönyv](automation-first-runbook-graphical.md)
* További információ az runbooktípusokkal, azok előnyeit, és a korlátozásokról tooknow lásd [Azure Automation-runbook típusok](automation-runbook-types.md)
* toounderstand hogyan tooauthenticate használatával hello Automation Futtatás mint fiókot, lásd: [konfigurálása Azure futtató fiók](automation-sec-configure-azure-runas-account.md)

