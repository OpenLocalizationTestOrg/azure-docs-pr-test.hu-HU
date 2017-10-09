---
title: "virtuális gépek során munkaidőn kívüli [előzetes verzió] megoldás aaaStart/Stop |} Microsoft Docs"
description: "hello VM megoldásokat elindul, és leállítja az Azure Resource Manager virtuális gépek ütemezés szerint, és proaktív módon figyelheti a Naplóelemzési."
services: automation
documentationCenter: 
authors: mgoedtel
manager: carmonm
editor: 
ms.assetid: 06c27f72-ac4c-4923-90a6-21f46db21883
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: magoedte
ms.openlocfilehash: 6cbe16dfb40bf13f29d9e58ca0bc8c5c7979879d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="startstop-vms-during-off-hours-preview-solution-in-automation"></a>Virtuális gépek indítása/leállítása munkaidőn kívül [előzetes verzió] megoldás az Automation szolgáltatásban

hello indítása/leállítása virtuális gépek munkaidőn kívüli [előzetes verzió] megoldás során elindul, és az Azure Resource Manager virtuális gépek nem felhasználó által definiált ütemezés szerint, és hello sikeres hello automatizálási feladatok elindítása és leállítása a virtuális gépek OMS betekintést nyújt Naplóelemzési.  

## <a name="prerequisites"></a>Előfeltételek

- runbookokat hello dolgozni egy [Azure-beli futtató fiók](automation-offering-get-started.md#authentication-methods).  a Futtatás mint fiók hello hello előnyben részesített hitelesítési módszer azért, mert a tanúsítvány alapú hitelesítést használ, amely lejár, vagy módosítsa gyakran jelszó helyett.  

- Ez a megoldás csak kezelhető hello képező virtuális gépek ugyanahhoz az előfizetéshez, ahol hello Automation-fiók található.  

- Ez a megoldás csak toohello a következő Azure-régiók - Ausztrália óceáni térség délkeleti régiója, USA keleti régiója, Délkelet-Ázsiában és Nyugat-Európa telepíti.  hello runbookokat hello VM ütemezés felügyelt virtuális gépek minden régióban célba.  

- toosend e-mail értesítések küldése, ha hello runbookok elindítása és leállítása VM teljes, vállalati szintű Office 365-előfizetéssel szükség.  

## <a name="solution-components"></a>Megoldás-összetevők

Ez a megoldás, hogy importálja, illetve tooyour Automation-fiók hozzáadva a következő hello áll.

### <a name="runbooks"></a>Runbookok

Forgatókönyv | Leírás|
--------|------------|
CleanSolution-MS-Mgmt-VM | Ez a forgatókönyv eltávolítja az összes benne lévő erőforrásokat és ütemezések lépve toodelete hello megoldás az előfizetésből.|  
SendMailO365-MS-Mgmt | Ez a runbook e-mailt küld az Office 365 Exchange-en keresztül.|
StartByResourceGroup-MS-Mgmt-VM | Ez a runbook tervezett toostart virtuális gépek (mindkét klasszikus és ARM alapú virtuális gépek), amely az Azure-erőforrás (ok) ból megadott listája helyezkedik el.
StopByResourceGroup-MS-Mgmt-VM | Ez a runbook tervezett toostop virtuális gépek (mindkét klasszikus és ARM alapú virtuális gépek), amely az Azure-erőforrás (ok) ból megadott listája helyezkedik el.|
<br>

### <a name="variables"></a>Változók

Változó | Leírás|
---------|------------|
**SendMailO365-MS-Mgmt** runbook ||
SendMailO365-IsSendEmail-MS-Mgmt | Meghatározza, hogy a StartByResourceGroup-MS-Mgmt-VM és a StopByResourceGroup-MS-Mgmt-VM runbook küldjön-e e-mail-értesítést a művelet befejezésekor.  Válassza ki **igaz** tooenable és **hamis** toodisable e-mailt küld figyelmeztetést. Az alapértelmezett érték a **False** (Hamis).| 
**StartByResourceGroup-MS-Mgmt-VM** runbook ||
StartByResourceGroup-ExcludeList-MS-Mgmt-VM | Adja meg a virtuális gép nevének toobe kizárja a kezelési művelet; külön nevek semi-colon(;) használatával, szóközök nélkül. Az értékeknél különbséget kell tenni a kis-és a nagybetűk között, és a helyettesítő karakter (csillag) használata támogatott.|
StartByResourceGroup-SendMailO365-EmailBodyPreFix-MS-Mgmt | Szöveg, amely hello e-mail üzenet törzsében toohello elejéhez lesz hozzáfűzve.|
StartByResourceGroup-SendMailO365-EmailRunBookAccount-MS-Mgmt | Automation-fiók, amely tartalmazza az üdvözlő E-mail runbook hello hello nevét adja meg.  **Ne módosítsa ezt a változót.**|
StartByResourceGroup-SendMailO365-EmailRunbookName-MS-Mgmt | Hello e-mail runbook hello nevét adja meg.  Ez hello StartByResourceGroup-MS-Mgmt-virtuális gép és használják StopByResourceGroup-MS-Mgmt-VM runbookok toosend e-mailt.  **Ne módosítsa ezt a változót.**|
StartByResourceGroup-SendMailO365-EmailRunbookResourceGroup-MS-Mgmt | Hello E-mail runbook tartalmazó erőforráscsoportot hello hello nevét adja meg.  **Ne módosítsa ezt a változót.**|
StartByResourceGroup-SendMailO365-EmailSubject-MS-Mgmt | Megadja az üdvözlő e-mail tárgysora hello hello szövegét.|  
StartByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt | Adja meg az e-mailek hello hello címzettjeként.  Adja meg a neveket, szóközök nélkül semi-colon(;) használatával.|
StartByResourceGroup-TargetResourceGroups-MS-Mgmt-VM | Adja meg a virtuális gép nevének toobe kizárja a kezelési művelet; külön nevek semi-colon(;) használatával, szóközök nélkül. Az értékeknél különbséget kell tenni a kis-és a nagybetűk között, és a helyettesítő karakter (csillag) használata támogatott.  Alapértelmezett érték (csillag) tartalmazza az összes erőforráscsoport hello előfizetésben.|
StartByResourceGroup-TargetSubscriptionID-MS-Mgmt-VM | Itt adhatja meg, amely tartalmazza a megoldás által kezelt virtuális gépek toobe hello előfizetés.  Az értéknek kell lennie hello hello Automation-fiók az ilyen típusú megoldásra tartalmazó ugyanahhoz az előfizetéshez.|
**StopByResourceGroup-MS-Mgmt-VM** runbook ||
StopByResourceGroup-ExcludeList-MS-Mgmt-VM | Adja meg a virtuális gép nevének toobe kizárja a kezelési művelet; külön nevek semi-colon(;) használatával, szóközök nélkül. Az értékeknél különbséget kell tenni a kis-és a nagybetűk között, és a helyettesítő karakter (csillag) használata támogatott.|
StopByResourceGroup-SendMailO365-EmailBodyPreFix-MS-Mgmt | Szöveg, amely hello e-mail üzenet törzsében toohello elejéhez lesz hozzáfűzve.|
StopByResourceGroup-SendMailO365-EmailRunBookAccount-MS-Mgmt | Automation-fiók, amely tartalmazza az üdvözlő E-mail runbook hello hello nevét adja meg.  **Ne módosítsa ezt a változót.**|
StopByResourceGroup-SendMailO365-EmailRunbookResourceGroup-MS-Mgmt | Hello E-mail runbook tartalmazó erőforráscsoportot hello hello nevét adja meg.  **Ne módosítsa ezt a változót.**|
StopByResourceGroup-SendMailO365-EmailSubject-MS-Mgmt | Megadja az üdvözlő e-mail tárgysora hello hello szövegét.|  
StopByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt | Adja meg az e-mailek hello hello címzettjeként.  Adja meg a neveket, szóközök nélkül semi-colon(;) használatával.|
StopByResourceGroup-TargetResourceGroups-MS-Mgmt-VM | Adja meg a virtuális gép nevének toobe kizárja a kezelési művelet; külön nevek semi-colon(;) használatával, szóközök nélkül. Az értékeknél különbséget kell tenni a kis-és a nagybetűk között, és a helyettesítő karakter (csillag) használata támogatott.  Alapértelmezett érték (csillag) tartalmazza az összes erőforráscsoport hello előfizetésben.|
StopByResourceGroup-TargetSubscriptionID-MS-Mgmt-VM | Itt adhatja meg, amely tartalmazza a megoldás által kezelt virtuális gépek toobe hello előfizetés.  Az értéknek kell lennie hello hello Automation-fiók az ilyen típusú megoldásra tartalmazó ugyanahhoz az előfizetéshez.|  
<br>

### <a name="schedules"></a>Ütemezések

Ütemezés | Leírás|
---------|------------|
StartByResourceGroup-Schedule-MS-Mgmt | StartByResourceGroup runbookot, amely végrehajtja a megoldás által kezelt virtuális gépek hello indítás ütemezése. Ha kész, tooUTC időzóna alapértelmezés szerint.|
StopByResourceGroup-Schedule-MS-Mgmt | StopByResourceGroup runbookot, amely végrehajtja a megoldás által kezelt virtuális gépek hello leállítás ütemezése. Ha kész, tooUTC időzóna alapértelmezés szerint.|

### <a name="credentials"></a>Hitelesítő adatok

Hitelesítő adat | Leírás|
-----------|------------|
O365Credential | Adja meg egy érvényes Office 365 felhasználói fiók toosend e-mailt.  Csak akkor szükséges, ha a változó SendMailO365-IsSendEmail-MS-Mgmt túl van-e állítva**igaz**.

## <a name="configuration"></a>Konfiguráció

Hajtsa végre a hello követő lépéseket tooadd hello indítása/leállítása virtuális gépek munkaidőn kívüli [előzetes verzió] megoldás tooyour Automation-fiók alatt, majd válassza a hello változók toocustomize hello megoldás.

1. Hello kezdőlap-képernyőn hello Azure-portálon, válassza ki hello **piactér** csempére.  Ha hello csempe már nem rögzített tooyour-kezdőképernyőn hello bal oldali navigációs ablakból válassza **új**.  
2. Hello piactér panelen írja be a **VM indítása** hello keresési mezőbe, és jelölje ki hello megoldás **indítása/leállítása virtuális gépek során munkaidőn kívüli [előzetes verzió]** hello a keresési eredmények.  
3. A hello **indítása/leállítása virtuális gépek során munkaidőn kívüli [előzetes verzió]** hello panelen kiválasztott megoldás, tekintse át hello összefoglaló információit, és kattintson **létrehozása**.  
4. Hello **megoldás hozzáadása** panel jelenik meg, hol felszólító tooconfigure hello megoldás előtt importálhatja az Automation-előfizetés.<br><br> ![Virtuálisgép-felügyelet – Megoldás hozzáadása panel](media/automation-solution-vm-management/vm-management-solution-add-solution-blade.png)<br><br>
5.  A hello **megoldás hozzáadása** panelen válassza **munkaterület** és itt választhatja ki, amelyek azonos Azure-előfizetéshez Automation-fiók hello a csatolt toohello, vagy hozzon létre egy új OMS-munkaterület OMS-munkaterület.  Ha még nem rendelkezik az OMS-munkaterület, akkor választhat **új munkaterület létrehozása** a hello **OMS-munkaterület** panel hello következőket hajthatja végre: 
   - Adjon meg egy nevet az új hello **OMS-munkaterület**.
   - Válassza ki a **előfizetés** toolink tooby kijelölésével hello legördülő lista esetén hello alapértelmezett beállítás nem megfelelő.
   - Az **Erőforráscsoport** területen létrehozhat egy új erőforráscsoportot, vagy kiválaszthat egy meglévőt.  
   - Válasszon ki egy **helyet**.  Jelenleg hello csak kijelölés előírt helyei **Ausztrália délkeleti**, **USA keleti régiója**, **Délkelet-Ázsia**, és **Nyugat-Európa**.
   - Válasszon egy tarifacsomagot a **Tarifacsomag** területen.  a két réteg tartományregisztráció hello megoldás: Szabadítson fel, és az OMS fizetett réteg.  ingyenes szint hello hello összeg naponta, megőrzési időtartam és runbook-feladat futásidejű perc adatgyűjtés van korlátozva.  hello réteg fizetett OMS nincs maximális hello naponta gyűjtött adatok mennyiségét.  

        > [!NOTE]
        > Hello réteg fizetős önálló lehetőség jelenik meg, amíg nincs alkalmazható.  Válassza ki azt, és folytassa a hello létrehozása ehhez a megoldáshoz az előfizetéshez, ha meghiúsul.  Csak akkor lesz választható, ha ezt a megoldást hivatalosan is kiadják.<br>Ha ezt a megoldást használja, az csak az automatizálási feladat idejét (percben), illetve a naplófeldolgozást fogja használni.  hello megoldás nem bővíti ki további OMS csomópontok tooyour környezetben.  

6. A hello hello szükséges információkat biztosít után **OMS-munkaterület** panelen kattintson a **létrehozása**.  Hello információk ellenőrizve, és hello munkaterület jön létre, nyomon követheti a folyamat állapotát **értesítések** hello menüből.  A rendszer visszairányítja toohello **megoldás hozzáadása** panelen.  
7. A hello **megoldás hozzáadása** panelen válassza **Automation-fiók**.  Ha egy új OMS-munkaterület hoz létre, Ön lesz szükség tooalso hozzon létre egy új Automation-fiók, amely a hello új OMS-munkaterület megadott korábbi, beleértve az Azure-előfizetéssel, erőforráscsoport és terület lesz társítva.  Kiválaszthatja **Automation-fiók létrehozása** a hello **hozzáadása Automation-fiók** panelen adja meg a következő hello: 
  - A hello **neve** mezőbe írja be a hello hello Automation-fiók nevét.

    Egyéb beállítások automatikusan fel van töltve hello OMS-munkaterület kijelölt alapján, és ezek a beállítások nem módosíthatók.  Egy Azure-beli futtató fiókot a hello alapértelmezett hitelesítési módszer hello runbookok a megoldásban.  Miután rákattintott **OK**, hello konfigurációs beállításokat érvényesíti, és hello Automation-fiók jön létre.  A folyamat állapotát nyomon követheti **értesítések** hello menüből. 

    Ellenkező esetben kiválaszthat egy meglévő Automation-futtatófiókot.  Vegye figyelembe, hogy bejelöli hello fiók már nem lehet csatolt tooanother OMS-munkaterület, ellenkező esetben üzenet számára jelenik meg a hello panel tooinform meg.  Már kapcsolódik, ha egy másik Automation Futtatás mint fiók tooselect kell, vagy hozzon létre egy újat.<br><br> ![Automation-fiók már csatolt tooOMS munkaterület](media/automation-solution-vm-management/vm-management-solution-add-solution-blade-autoacct-warning.png)<br>

8. Végül a hello **megoldás hozzáadása** panelen válassza **konfigurációs** és hello **paraméterek** panel jelenik meg.  A hello **paraméterek** panelen kéri:  
   - Adja meg a hello **célként megadott erőforráscsoport-nevek**, ez a megoldás által kezelt virtuális gépek toobe tartalmazó erőforráscsoport nevét.  Több nevet is megadhat, pontosvesszővel elválasztva őket egymástól (az értékek megkülönböztetik a kis- és a nagybetűket).  Helyettesítő karakterek használatával támogatott, ha azt szeretné, hogy a virtuális gépek tootarget az összes erőforráscsoport hello előfizetésben.
   - Válassza ki a **ütemezés** olyan ismétlődő dátumot és időpontot a indítása és leállítása hello a virtuális gép hello a cél erőforrás (ok) ból.  Alapértelmezés szerint hello ütemezés konfigurált toohello UTC időzóna- és egy másik régió kiválasztásával nem érhető el.  Ha a tooconfigure hello ütemezés tooyour adott időzóna hello megoldás konfigurálása után, lásd: [módosítása hello indítási és leállítási ütemezés](#modifying-the-startup-and-shutdown-schedule) alatt.    

10. Miután hello kezdeti beállításainak konfigurálását hello a megoldáshoz szükséges, válassza ki a **létrehozása**.  Minden beállítások lesznek érvényesítve, és ezután megpróbál toodeploy hello megoldás az előfizetésben.  A folyamat eltarthat néhány másodpercig toocomplete, és nyomon követheti a folyamat állapotát **értesítések** hello menüből. 

## <a name="collection-frequency"></a>A gyűjtés gyakorisága

Automation-feladat napló és a feladat adatfolyamokat van keresztül a szervezetbe történő hello OMS tárházba 5 perc.  

## <a name="using-hello-solution"></a>Hello megoldással

Amikor hozzáadja hello Virtuálisgép-felügyeleti megoldás, az OMS-munkaterület hello **StartStopVM nézet** csempe megkapja tooyour OMS irányítópult.  Ez a csempe egy száma és a grafikus ábrázolása hello runbookok feladatok hello megoldás, amely indult el, és sikeresen befejeződött jeleníti meg.<br><br> ![Virtuális gépek felügyelete – StartStopVM nézet csempe](media/automation-solution-vm-management/vm-management-solution-startstopvm-view-tile.png)  

Az Automation-fiókban férhessen hozzá és felügyelhesse hello megoldás hello kiválasztásával **megoldások** csempe és majd a hello **megoldások** panelen hello megoldás kiválasztása **Start-Stop-VM [ Munkaterület]** hello listából.<br><br> ![Automation-megoldások listája](media/automation-solution-vm-management/vm-management-solution-autoaccount-solution-list.png)  

Hello megoldás kiválasztása hello megjeleníti **Start-Stop-VM [munkaterület]** megoldás panelen, melyen áttekintheti a fontos részleteket, például a hello **StartStopVM** csempére, például az OMS-munkaterület, amely Megjeleníti egy száma és grafikus ábrázolása hello runbookok feladatok hello megoldás, amely indult el, és sikeresen befejeződött.<br><br> ![Automation VM-megoldás panel](media/automation-solution-vm-management/vm-management-solution-solution-blade.png)  

Itt is nyissa meg az OMS-munkaterület és hello feladatrekordok további elemzést.  Csak kattintson **összes beállítás**, és hello **beállítások** panelen válassza **gyors üzembe helyezés** , majd a hello **gyors üzembe helyezés** panelen válassza ki  **OMS-portálon**.   Ezzel megnyílik egy új lap vagy egy új böngésző-munkamenet, és megjelenik az Automation-fiókjához és -előfizetéséhez tartozó OMS-munkaterülete.  


### <a name="configuring-e-mail-notifications"></a>E-mail-értesítések konfigurálása

tooenable e-mail értesítések küldése, ha hello runbookok elindítása és leállítása VM teljes, szüksége lesz a toomodify hello **O365Credential** hitelesítési adatot, és legalább a következő változók hello:

 - SendMailO365-IsSendEmail-MS-Mgmt
 - StartByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt
 - StopByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt

tooconfigure hello **O365Credential** hitelesítőadat-, hajtsa végre az alábbi lépésekkel hello:

1. Az automation-fiók kattintson **összes beállítás** hello ablak hello tetején. 
2. A hello **beállítások** hello szakaszban panel **Automation erőforrások**, jelölje be **eszközök**. 
3. A hello **eszközök** panelen, jelölje be hello **hitelesítő adat** csempe és hello **hitelesítő adat** panelen, jelölje be hello **O365Credential**.  
4. Adjon meg egy érvényes Office 365-felhasználónevet és jelszót, és kattintson a **mentése** toosave a módosításokat.  

korábban, a kijelölt tooconfigure hello változók hajtsa végre a lépéseket követve hello:

1. Az automation-fiók kattintson **összes beállítás** hello ablak hello tetején. 
2. A hello **beállítások** hello szakaszban panel **Automation erőforrások**, jelölje be **eszközök**. 
3. A hello **eszközök** panelen, jelölje be hello **változók** csempe és hello **változók** panelen válassza ki a fent felsorolt hello változó, és módosítsa az értéket a következő hello Leírás az hello megadott [változó](##variables) korábbi szakaszában.  
4. Kattintson a **mentése** toosave hello módosítások toohello változó.   

### <a name="modifying-hello-startup-and-shutdown-schedule"></a>Módosítása hello indítási és leállítási ütemezése

Kezelése hello indítási és leállítási ütemezés ebben a megoldásban a következő azonos lépések a hello [az Azure Automationben runbook ütemezése](automation-schedules.md).  Ne feledje, hogy hello ütemezési konfiguráció nem módosítható.  Szüksége lesz a toodisable meglévő ütemezés hello és majd hozzon létre egy újat, és csatolja a toohello **StartByResourceGroup-MS-Mgmt-VM** vagy **StopByResourceGroup-MS-Mgmt-VM** hello kívánt runbook a tooapply ütemezni.   

## <a name="log-analytics-records"></a>Log Analytics-rekordok

Automatizálási kétféle típusú rekordok hello OMS-tárház hoz létre.

### <a name="job-logs"></a>Feladatnaplók

Tulajdonság | Leírás|
----------|----------|
Hívó |  Kik kezdeményeztek hello műveletet.  Lehetséges értékek: egy e-mail-cím vagy egy ütemezett feladatokat tartalmazó rendszer.|
Kategória | Hello típusú adatok besorolása.  Az automatizáláshoz a hello értéke JobLogs.|
CorrelationId | GUID azonosítója, amely hello hello runbook-feladat korrelációs azonosítója.|
JobId | GUID azonosítója, amely hello hello runbook feladatának azonosítója.|
operationName | Megadja az Azure-ban végrehajtott művelet hello típusát.  Az automatizáláshoz hello értéket fogja feladat.|
resourceId | Az Azure-ban hello erőforrás típusát határozza meg.  Az automatizáláshoz a hello értéke hello runbook társított hello Automation-fiók.|
ResourceGroup | Adja meg a runbook-feladat hello hello erőforráscsoport neve.|
ResourceProvider | Megadja a hello Azure szolgáltatás, amellyel hello erőforrások telepítheti és kezelheti.  Az automatizáláshoz a hello értéke Azure Automation.|
ResourceType | Az Azure-ban hello erőforrás típusát határozza meg.  Az automatizáláshoz a hello értéke hello runbook társított hello Automation-fiók.|
resultType | runbook-feladat hello hello állapota.  Lehetséges értékek:<br>- Elindítva<br>- Leállítva<br>- Felfüggesztve<br>- Sikertelen<br>- Sikeres|
resultDescription | Hello runbook feladat eredménye állapotokat ismerteti.  Lehetséges értékek:<br>- A feladat elindult<br>- A feladat nem sikerült<br>- A feladat befejeződött|
RunbookName | Hello runbook hello nevét adja meg.|
SourceSystem | Meghatározza, hogy hello forrás hello adatok elküldése megtörtént.  Az automatizáláshoz hello értéket fogja: OpsManager|
StreamType | Adja meg az esemény hello típusa. Lehetséges értékek:<br>- Részletes<br>- Kimenet<br>- Hiba<br>- Figyelmeztetés|
SubscriptionId | Adja meg az előfizetés-azonosító hello hello feladat.
Time | Dátum és idő, amikor hello runbook-feladat végrehajtása.|


### <a name="job-streams"></a>Feladatstreamek

Tulajdonság | Leírás|
----------|----------|
Hívó |  Kik kezdeményeztek hello műveletet.  Lehetséges értékek: egy e-mail-cím vagy egy ütemezett feladatokat tartalmazó rendszer.|
Kategória | Hello típusú adatok besorolása.  Az automatizáláshoz a hello értéke JobStreams.|
JobId | GUID azonosítója, amely hello hello runbook feladatának azonosítója.|
operationName | Megadja az Azure-ban végrehajtott művelet hello típusát.  Az automatizáláshoz hello értéket fogja feladat.|
ResourceGroup | Adja meg a runbook-feladat hello hello erőforráscsoport neve.|
resourceId | Adja meg a hello erőforrás-azonosítót az Azure-ban.  Az automatizáláshoz a hello értéke hello runbook társított hello Automation-fiók.|
ResourceProvider | Megadja a hello Azure szolgáltatás, amellyel hello erőforrások telepítheti és kezelheti.  Az automatizáláshoz a hello értéke Azure Automation.|
ResourceType | Az Azure-ban hello erőforrás típusát határozza meg.  Az automatizáláshoz a hello értéke hello runbook társított hello Automation-fiók.|
resultType | runbook-feladat hello hello idő hello eseményt hello eredményét jött létre.  Lehetséges értékek:<br>- Folyamatban|
resultDescription | Hello kimeneti adatfolyamba hello runbookból tartalmazza.|
RunbookName | hello runbook hello neve.|
SourceSystem | Meghatározza, hogy hello forrás hello adatok elküldése megtörtént.  Az automatizáláshoz a hello értéket fogja OpsManager|
StreamType | feladatfolyam hello típusa. Lehetséges értékek:<br>- Folyamatban<br>- Kimenet<br>- Figyelmeztetés<br>- Hiba<br>- Hibakeresés<br>- Részletes|
Time | Dátum és idő, amikor hello runbook-feladat végrehajtása.|

Bármely ad vissza rekordok kategóriájának napló keresési végrehajtásakor **JobLogs** vagy **JobStreams**, kiválaszthatja a hello **JobLogs** vagy **JobStreams** megjelenítése mozaikok hello keresés által visszaadott hello frissítések összefoglalójához nézetben.

## <a name="sample-log-searches"></a>Naplókeresési minták

hello következő táblázat a megoldás által gyűjtött feladatrekordok minta napló keres. 

Lekérdezés | Leírás|
----------|----------|
A StartVM runbook sikeresen befejeződött feladatainak megkeresése | Category=JobLogs RunbookName_s="StartByResourceGroup-MS-Mgmt-VM" ResultType=Succeeded &#124; count() mérése JobId_g szerint|
A StopVM runbook sikeresen befejeződött feladatainak megkeresése | Category=JobLogs RunbookName_s="StartByResourceGroup-MS-Mgmt-VM" ResultType=Failed &#124; count() mérése JobId_g szerint
A StartVM és a StopVM runbook feladatainak állapotmegjelenítése az idő múlásával | Category=JobLogs RunbookName_s="StartByResourceGroup-MS-Mgmt-VM" OR "StopByResourceGroup-MS-Mgmt-VM" NOT(ResultType="started") | Count() mérése ResultType szerint 1 napos időközönként|

## <a name="removing-hello-solution"></a>Hello megoldás eltávolítása

Ha úgy dönt, hogy már nincs szüksége toouse hello megoldás minden további, a hello Automation-fiók törlése.  Hello megoldás törlése runbookokat hello csupán eltávolítja, nem törli hello ütemezések vagy változók, amelyek létrehozásakor hello megoldást jelent.  Azok az eszközök akkor toodelete manuálisan Ha éppen nem használja másik runbookok.  

toodelete hello megoldás, hajtsa végre az alábbi lépésekkel hello:

1.  Válassza ki az automation-fiók hello **megoldások** csempére.  
2.  A hello **megoldások** panelen, jelölje be hello megoldás **Start-Stop-VM [munkaterület]**.  A hello **VMManagementSolution [munkaterület]** a hello menüben kattintson a panel **törlése**.<br><br> ![Virtuálisgép-felügyeleti megoldás törlése](media/automation-solution-vm-management/vm-management-solution-delete.png)
3.  A hello **megoldás törlése** ablakban jóváhagyásához toodelete hello megoldás.
4.  Hello információk ellenőrizve, és hello megoldás törlése, nyomon követheti a folyamat állapotát **értesítések** hello menüből.  A rendszer visszairányítja toohello **VMManagementSolution [munkaterület]** panel hello folyamat tooremove megoldás elindulása után.  

hello Automation-fiók és az OMS-munkaterület nem törlődnek a folyamat részeként.  Ha nem szeretné, hogy tooretain hello OMS-munkaterület, akkor toomanually törölje azt.  Ehhez is a hello Azure-portálon.   Hello kezdőlap-képernyőn hello Azure-portálon, válassza ki **Naplóelemzési** majd a hello **Naplóelemzési** panelen, jelölje be hello munkaterület, és kattintson **törlése** hello menüjéből hello munkaterület beállítások panelen.  
      
## <a name="next-steps"></a>Következő lépések

- toolearn hogyan tooconstruct különböző keresési lekérdezések és felülvizsgálati hello Automation feladat naplók és a Log Analyticshez kapcsolatos további információkért lásd: [Log Analytics-e jelentkezni a keresések](../log-analytics/log-analytics-log-searches.md)
- További információk a runbook végrehajtása toomonitor runbook feladatokat, valamint egyéb technikai részleteket lásd: hogyan toolearn [nyomon követheti a runbook-feladatok](automation-runbook-execution.md)
- További információ az OMS szolgáltatáshoz és a gyűjtemény adatforrások toolearn lásd [gyűjtése Azure storage adatok a Naplóelemzési – áttekintés](../log-analytics/log-analytics-azure-storage.md)






   

