---
title: "aaaMonitor és adatok folyamatok - Azure kezelése |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse figyelés és a felügyeleti alkalmazás toomonitor hello és Azure adat-előállítók és folyamatok kezelése."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: f3f07bc4-6dc3-4d4d-ac22-0be62189d578
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: spelluru
ms.openlocfilehash: 5e4ef6ec5fb8ebc9bda0be7899a39a51d58403d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-hello-monitoring-and-management-app"></a>Figyelheti és kezelheti az Azure Data Factory folyamatok hello figyelés és felügyelet alkalmazással
> [!div class="op_single_selector"]
> * [Az Azure portálon vagy az Azure PowerShell használatával](data-factory-monitor-manage-pipelines.md)
> * [Használatával figyelése és a felügyeleti alkalmazás](data-factory-monitor-manage-app.md)
>
>

Ez a cikk ismerteti, hogyan toouse hello figyelés és a felügyeleti alkalmazás toomonitor, kezelése és az adat-előállító adatcsatornák hibakeresését. Azt is megtudhatja hogyan toocreate riasztások hibáiról értesítés tooget. Ismerkedés hello alkalmazás használatával a következő néznek hello által videót:

> [!NOTE]
> hello felhasználói felülete látható hello videó előfordulhat, hogy nem egyeznek pontosan kapcsolatban a hello portálon. Némileg régebbi, de fogalmak hello továbbra is azonos. 

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-Data-Factory-Monitoring-and-Managing-Big-Data-Piplines/player]
>

## <a name="launch-hello-monitoring-and-management-app"></a>Hello figyelés és a felügyeleti alkalmazás indítása
toolaunch hello figyelése és a felügyeleti alkalmazás, kattintson a hello **figyelő & kezelése** hello csempét **adat-előállító** a data factory paneljét.

![Hello adat-előállító kezdőlapján csempe figyelése](./media/data-factory-monitor-manage-app/MonitoringAppTile.png)

Nyisson meg egy külön ablakban hello figyelés és a felügyeleti alkalmazás kell megjelennie.  

![Figyelési és felügyeleti alkalmazás](./media/data-factory-monitor-manage-app/AppLaunched.png)

> [!NOTE]
> Ha azt látja, hogy hello webböngésző akadt-e a "Engedélyező...", törölje a jelet hello **külső cookie-k blokkolását, és a helyadatok** jelölőnégyzet – vagy a tárolás során is garantálják az kiválasztva, hozzon létre egy kivételt **login.microsoftonline.com** , majd próbálkozzon újra a tooopen hello alkalmazást.


Hello tevékenység Windows hello középső ablaktábla listájában megjelenik egy tevékenység ablakban tevékenység minden egyes futtatásához. Például ha hello ütemezett tevékenység toorun óránkénti rendelkezik öt órát, látható öt tevékenység windows társított öt adatszeletek. Ha nem lát tevékenység windows hello listában hello lap alján, a hello, a következő:
 
- Frissítés hello **kezdési időpont** és **befejező időpontja** szűrők hello felső toomatch hello: start és befejezési időpontja, a folyamat, és kattintson a hello **alkalmaz** gombra.  
- hello tevékenység Windows lista nem frissül automatikusan. Kattintson a hello **frissítése** hello gombjára a hello **tevékenység Windows** lista.  

Ha még nem rendelkezik a Data Factory alkalmazás tootest ezeket a lépéseket, hello oktatóanyag: [adatokat másolni a Blob Storage tooSQL adatbázis használata a Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="understand-hello-monitoring-and-management-app"></a>Figyelés és a felügyeleti alkalmazás hello ismertetése
Nincsenek hello bal oldali három lappal: **erőforrás-kezelő**, **figyelési nézetei**, és **riasztások**. hello első lapon (**erőforrás-kezelő**) alapértelmezés szerint engedélyezett.

### <a name="resource-explorer"></a>Erőforrás-kezelő
Hello következő jelenik meg:

* az erőforrás-kezelő hello **fanézetben** hello bal oldali ablaktáblán.
* Hello **diagramnézet** hello felső hello középső ablaktáblán.
* Hello **tevékenység Windows** lista hello alsó hello középső ablaktáblán.
* Hello **tulajdonságok**, **tevékenység ablak Explorer**, és **parancsfájl** lapok hello jobb oldali ablaktáblán.

Az erőforrás-kezelőben lásd: összes erőforrást (adatcsatornák, adatkészleteket, összekapcsolt szolgáltatások) fanézetben hello adat-előállítóban. Amikor kijelöl egy objektumot az erőforrás-kezelőben:

* hello entitás ki van jelölve, a Diagram nézet hello adat-előállító tartozik.
* [Hozzárendelt tevékenység windows](data-factory-scheduling-and-execution.md) hello tevékenység Windows listában hello alsó vannak kiemelve.  
* hello kijelölt objektum tulajdonságainak hello hello jobb oldali hello tulajdonságai ablakban láthatók.
* hello JSON-definícióból hello kijelölt objektum jelenik meg, ha van ilyen. Például: a társított szolgáltatás, a DataSet adatkészlet vagy folyamat.

![Erőforrás-kezelő](./media/data-factory-monitor-manage-app/ResourceExplorer.png)

Lásd: hello [ütemezés és a végrehajtás](data-factory-scheduling-and-execution.md) cikk tevékenység windows kapcsolatos további részletes információt.

### <a name="diagram-view"></a>Diagramnézet
egy adat-előállító Diagramnézetében hello egytáblás, üveghatású toomonitor biztosít, és a data factory és az eszközök kezelése. Ha bejelöli a Data Factory entitás (dataset/pipeline) a Diagram nézet hello:

* hello data factory entitás hello faszerkezetes nézetben kiválasztott.
* hello hozzárendelt tevékenység windows hello tevékenység Windows listán vannak kiemelve.
* hello kijelölt objektum tulajdonságainak hello hello tulajdonságai ablakban láthatók.

Ha hello folyamat engedélyezve van (nem szünetel), a zöld vonallal is látható:

![A folyamat fut](./media/data-factory-monitor-manage-app/PipelineRunning.png)

Szüneteltetése, folytatása vagy hello diagram nézetben jelölje ki azt, és hello gombokkal hello parancssávon folyamat leáll.

![/ Szüneteltet hello parancssávon](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)
 
Nincsenek három gombok hello adatcsatorna a Diagram nézet hello. Hello második gomb toopause hello folyamat is használhatja. Felfüggesztése nem a jelenleg futó tevékenységek hello leáll, és lehetővé teszi, hogy folytatni toocompletion. hello harmadik gomb hello folyamat megáll, és a meglévő tevékenységek végrehajtása leáll. hello első gomb hello folyamat folytatódik. Ha a feldolgozási sor fel van függesztve, hello csővezeték hello színe megváltozik. Például egy felfüggesztett folyamat a következőképpen néz a kép a következő hello: 

![Feldolgozási sor felfüggesztve](./media/data-factory-monitor-manage-app/PipelinePaused.png)

Segítségével többszörös kiválasztási két vagy több folyamatok hello Ctrl billentyűt. Használhat hello parancs sáv gombok toopause/Folytatás több folyamatok egyszerre.

Akkor is a jobb gombbal a feldolgozási sorban lévő és válassza a beállítások toosuspend folytatásához, vagy a folyamat leáll. 

![Az adatcsatorna helyi menü](./media/data-factory-monitor-manage-app/right-click-menu-for-pipeline.png)

Hello kattintson **nyitott folyamat** toosee hello folyamat összes hello tevékenység lehetőséget. 

![Folyamat megnyitása menü](./media/data-factory-monitor-manage-app/OpenPipelineMenu.png)

Megnyitott hello csővezeték nézetben lásd: az összes tevékenység hello folyamat. Ebben a példában csak egy tevékenység nincs: másolási tevékenység. 

![Megnyitott folyamat](./media/data-factory-monitor-manage-app/OpenedPipeline.png)

toogo biztonsági toohello előző nézetével, kattintson a hello adat-előállító hello navigációs menüjében hello tetején.

Hello csővezeték nézetben amikor kijelöl egy kimeneti adatkészletet, vagy amikor az egér átvitele hello kimeneti adatkészletet, látható hello tevékenység Windows előugró ablak, hogy a DataSet.

![Tevékenység Windows előugró ablak](./media/data-factory-monitor-manage-app/ActivityWindowsPopup.png)

Egy tevékenység ablak toosee a Részletek gombra ki azt a hello **tulajdonságok** hello jobb oldali ablak.

![Tevékenység ablak tulajdonságai](./media/data-factory-monitor-manage-app/ActivityWindowProperties.png)

Hello jobb oldali ablaktáblában kapcsoló toohello **tevékenység ablak Explorer** toosee további részletek lap.

![Tevékenység ablak Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer.png)

Azt is láthatja, **változók feloldva** az egyes futtatására tett kísérlet egy tevékenység hello **kísérletek** szakasz.

![Megoldott változók](./media/data-factory-monitor-manage-app/ResolvedVariables.PNG)

Váltás toohello **parancsfájl** toosee hello JSON parancsfájl definíciója hello kijelölt objektum fülre.   

![Parancsfájl lap](./media/data-factory-monitor-manage-app/ScriptTab.png)

Tevékenység windows három helyen tekintheti meg:

* a Diagram nézet (középső ablaktábla) hello előugró tevékenység Windows hello.
* hello tevékenység ablak Explorer hello jobb oldali ablaktáblán.
* hello tevékenység Windows hello alsó ablaktábla listáján.

Hello tevékenység Windows előugró üzenet és a tevékenység ablak Explorer görgetve toohello előző hét, és hello segítségével jövő héten hello jobb és bal nyíl.

![Tevékenység ablak Explorer balra vagy jobbra nyíl](./media/data-factory-monitor-manage-app/ActivityWindowExplorerLeftRightArrows.png)

Hello diagramnézet hello alsó részén, az ilyen gombokat látja: Nagyítás, Kicsinyítés, nagyítás tooFit Nagyítás 100 %-os, Elrendezés zárolása. Hello **zárolási elrendezés** gomb megakadályozza a táblák és folyamatok véletlen áthelyezését a hello Diagram nézetben. Alapértelmezés szerint le van. Kapcsolja ki, és entitások Navigálás hello diagramban. Kapcsolja ki, amikor hello utolsó gomb tooautomatically pozíció táblák és folyamatok is használhatja. Bejövő vagy kimenő hello egérkerék használatával is nagyítás.

![Diagram nézet Nagyítás parancsok](./media/data-factory-monitor-manage-app/DiagramViewZoomCommands.png)

### <a name="activity-windows-list"></a>A tevékenységek Windows listája
hello tevékenység Windows hello középső ablaktábláján hello alján megjelenik minden tevékenység windows hello adatkészlet hello erőforrás-kezelő vagy hello Diagram nézetben kiválasztott. Alapértelmezés szerint hello listát csökkenő sorrendben, ami azt jelenti, hogy megjelenik-e hello legújabb tevékenység ablak hello felső van.

![A tevékenységek Windows listája](./media/data-factory-monitor-manage-app/ActivityWindowsList.png)

Ebben a listában nem automatikusan, frissítse, használjon hello frissítés gomb hello eszköztár toomanually frissíti.  

Tevékenység windows hello a következő állapotok valamelyikében lehet:

<table>
<tr>
    <th align="left">status</th><th align="left">A részállapot</th><th align="left">Leírás</th>
</tr>
<tr>
    <td rowspan="8">Várakozás</td><td>ScheduleTime</td><td>hello tevékenység ablak toorun hello ideje még nem érkezett.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>hello fölérendelt függőségek nem állnak készen.</td>
</tr>
<tr>
<td>ComputeResources</td><td>hello számítási erőforrások nem érhetők el.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Minden hello Tevékenységpéldány futtatásával elfoglalva más tevékenység windows.</td>
</tr>
<tr>
<td>ActivityResume</td><td>hello tevékenység szüneteltetve van, és nem futtatható tevékenység windows hello folytatásáig.</td>
</tr>
<tr>
<td>Próbálja meg újra</td><td>hello tevékenység végrehajtási lesz hajtva.</td>
</tr>
<tr>
<td>Ellenőrzés</td><td>Érvényesítés még a még nem indult el.</td>
</tr>
<tr>
<td>ValidationRetry</td><td>Érvényesítési újrapróbált várakozási toobe.</td>
</tr>
<tr>
<tr>
<td rowspan="2">Esetbejegyzések</td><td>Ellenőrzése</td><td>Ellenőrzése folyamatban van.</td>
</tr>
<td>-</td>
<td>hello tevékenység ablakban feldolgozása folyamatban van.</td>
</tr>
<tr>
<td rowspan="4">Nem sikerült</td><td>Időtúllépésbe került</td><td>hello tevékenység végrehajtási hello tevékenység által megengedett érték időt vett igénybe.</td>
</tr>
<tr>
<td>Törölve</td><td>hello tevékenység ablakban felhasználói művelet megszakította.</td>
</tr>
<tr>
<td>Ellenőrzés</td><td>Sikertelen volt.</td>
</tr>
<tr>
<td>-</td><td>hello tevékenység ablakban toobe jön létre, vagy ha ellenőrizni nem sikerült.</td>
</tr>
<td>Készen áll</td><td>-</td><td>hello tevékenység ablakban készen áll a felhasználásra.</td>
</tr>
<tr>
<td>Kihagyva</td><td>-</td><td>hello tevékenység ablak nem lett feldolgozva.</td>
</tr>
<tr>
<td>None</td><td>-</td><td>Egy tevékenység ablakban tooexist használja egy eltérő állapottal, de alaphelyzetbe lett állítva.</td>
</tr>
</table>


Ha egy tevékenység ablakban hello listában gombra kattint, megjelenik az részleteit a hello **tevékenység Windows Explorer** vagy hello **tulajdonságok** hello jobb oldali ablak.

![Tevékenység ablak Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-2.png)

### <a name="refresh-activity-windows"></a>Tevékenység windows frissítése
hello részletek nem automatikusan frissülnek, ezért hello frissítési (gomb hello második) a hello parancssávon toomanually frissítési hello tevékenységek windows listája.  

### <a name="properties-window"></a>Tulajdonságok ablak
hello tulajdonságai ablakban hello figyelés és a felügyeleti alkalmazás hello jobb szélső panelén megtalálható.

![Tulajdonságok ablak](./media/data-factory-monitor-manage-app/PropertiesWindow.png)

Megjeleníti az erőforrás-kezelő (fanézetben) hello kiválasztott hello elem tulajdonságai Diagram nézetet, vagy a tevékenység Windows listája.

### <a name="activity-window-explorer"></a>Tevékenység ablak Explorer
Hello **tevékenység ablak Explorer** időszak van hello figyelés és a felügyeleti alkalmazás hello jobb szélső panelén. Hello tevékenység ablakban hello tevékenység Windows előugró ablak vagy hello tevékenység Windows listán kijelölt részleteit jeleníti meg.

![Tevékenység ablak Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-3.png)

Tooanother tevékenység ablakban hello naptár nézetben hello felső kattintva válthat. Hello/jobbra bal oldali gomb használatának hello felső toosee tevékenység windows hello az előző hét vagy jövő héten hello is.

Hello eszköztár gombjaival hello alsó ablaktáblán toorerun hello tevékenység ablakban, vagy hello részletek hello ablaktáblán frissítése.

### <a name="script"></a>Szkript
Használhatja a hello **parancsfájl** lapon tooview hello JSON-definícióból hello a kiválasztott adat-előállító entitás (társított szolgáltatás, adatkészlet vagy csővezeték).

![Parancsfájl lap](./media/data-factory-monitor-manage-app/ScriptTab.png)

## <a name="use-system-views"></a>Rendszer-nézetek
hello figyelés és a felügyeleti alkalmazás tartalmaz előre elkészített rendszernézetek (**legutóbbi tevékenységek windows**, **sikertelen volt a tevékenység windows**, **folyamatban lévő tevékenységek windows**), lehetővé teszi a data factory tooview legutóbbi/nem sikerült/folyamatban lévő tevékenységek időszakokat.

Váltás toohello **figyelési nézetei** fülre kattintva hello bal oldalon.

![Figyelési nézetek lap](./media/data-factory-monitor-manage-app/MonitoringViewsTab.png)

Jelenleg nincsenek három rendszernézetek támogatott. Válasszon egy lehetőséget toosee legutóbbi tevékenységek windows, a sikertelen tevékenységet windows vagy a folyamatban lévő tevékenységek windows hello tevékenység Windows listában (alján hello hello középső ablaktábla).

Ha bejelöli hello **legutóbbi tevékenységek windows** beállítást, megjelenik minden legutóbbi tevékenységek windows hello csökkenő **utolsó kísérlet ideje**.

Használhatja a hello **sikertelen volt a tevékenység windows** megtekintéséhez toosee összes sikertelen tevékenység windows hello listáján. Válassza ki a sikertelen tevékenységek ablakát hello lista toosee részleteit a hello **tulajdonságok** ablak vagy hello **tevékenység ablak Explorer**. A naplók a sikertelen tevékenységet időszak is letöltheti.

## <a name="sort-and-filter-activity-windows"></a>Rendezésére és szűrésére tevékenység windows
Változás hello **kezdési időpont** és **befejező időpontja** toofilter tevékenység windows hello parancssávon beállításait. Hello kezdési és befejezési időpontjának módosítása után kattintson hello gomb következő toohello befejezési idő toorefresh hello tevékenység Windows listára.

![Kezdő és befejező időpontja](./media/data-factory-monitor-manage-app/StartAndEndTimes.png)

> [!NOTE]
> Minden alkalommal jelenleg hello figyelés és a felügyeleti alkalmazás UTC formátumban.
>
>

A hello **tevékenység Windows lista**, kattintson egy hello neve (például: állapot).

![Tevékenység Windows lista oszlop menü](./media/data-factory-monitor-manage-app/ActivityWindowsListColumnMenu.png)

Mindent hello következő:

* Rendezés növekvő sorrendben.
* Rendezés csökkenő sorrendben.
* Szűrés egy vagy több (kész, Várakozás, és így tovább).

Ha szűrőt ad meg egy olyan oszlop, lásd: engedélyezve van az adott oszlop, amely azt jelzi, hogy hello hello oszlopban szereplő értékek szűrt értékek hello a szűrő gombra.

![Egy oszlop hello tevékenység Windows lista alapján szűrés](./media/data-factory-monitor-manage-app/ActivityWindowsListFilterInColumn.png)

Használhatja ugyanazt az előugró ablakban tooclear szűrők hello. Minden tevékenység Windows hello listát szűrők tooclear hello törlése gomb hello parancssávon kattintson.

![Hello tevékenység Windows lista az összes szűrő törlése](./media/data-factory-monitor-manage-app/ClearAllFiltersActivityWindowsList.png)

## <a name="perform-batch-actions"></a>Kötegelt műveleteket
### <a name="rerun-selected-activity-windows"></a>Futtassa újra a kijelölt tevékenység windows
Egy tevékenység ablak, kattintson a lefelé mutató nyíl hello első sáv parancsgomb hello válassza ki és **újrafuttatása** / **futtassa újra a előtt folyamat**. Hello kiválasztásakor **futtassa újra a előtt folyamat** beállítás, az összes felsőbb szintű tevékenység windows is Újrafuttatja.
    ![Futtassa újra a műveletet egy tevékenység ablak](./media/data-factory-monitor-manage-app/ReRunSlice.png)

Is több tevékenység windows hello listában jelölje ki, és futtatnia őket: hello ugyanannyi időt vesz igénybe. Érdemes lehet toofilter tevékenység windows hello állapota alapján (például: **sikertelen**) –, majd futtassa újra a sikertelen hello tevékenység windows hello tevékenység windows toofail okozó hello a probléma kijavítása után. Tekintse meg a következő tevékenység windows hello listában szűrése szakaszát hello.  

### <a name="pauseresume-multiple-pipelines"></a>Több folyamatok szüneteltet
Segítségével multiselect két vagy több folyamatok hello Ctrl billentyűt. Hello gombok (amely a következő kép hello hello piros téglalap kijelölt) is használhat toopause/Folytatás őket.

![/ Szüneteltet hello parancssávon](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

## <a name="create-alerts"></a>Riasztások létrehozása
Hello **riasztások** lap lehetővé teszi, hogy hozzon létre riasztást és nézet/Szerkesztés/törlés meglévő riasztást. Akkor is is tiltása/engedélyezése egy riasztást. toosee hello riasztások lapján, kattintson a hello **riasztások** fülre.

![Riasztások lap](./media/data-factory-monitor-manage-app/AlertsTab.png)

### <a name="toocreate-an-alert"></a>toocreate riasztás
1. Kattintson a **hozzáadása riasztás** tooadd riasztást. Megjelenik a hello **részletek** lap.

    ![Riasztások – Részletek lap létrehozása](./media/data-factory-monitor-manage-app/CreateAlertDetailsPage.png)
2. Adja meg a hello **neve** és **leírás** hello riasztást, majd kattintson a **következő**. Megtekintheti az hello **szűrők** lap.

    ![Riasztások – szűrők lap létrehozása](./media/data-factory-monitor-manage-app/CreateAlertFiltersPage.png)
3. Jelölje be hello **esemény**, **állapot**, és **részállapot** (nem kötelező), amelyet az toocreate a Data Factory szolgáltatásnak a riasztásra, majd kattintson **tovább**. Megtekintheti az hello **címzettek** lap.

    ![Riasztások – címzettek lap létrehozása](./media/data-factory-monitor-manage-app/CreateAlertRecipientsPage.png)
4. SELECT hello **E-mail-előfizetés rendszergazdái** lehetőséget és/vagy adjon meg egy **további rendszergazdai e-mail**, és kattintson a **Befejezés**. Hello riasztás hello listában kell megjelennie.

    ![Riasztások listája](./media/data-factory-monitor-manage-app/AlertsList.png)

Hello riasztáslistában gombokkal hello társított hello riasztási tooedit/Törlés/tiltása/engedélyezése egy riasztást.

### <a name="eventstatussubstatus"></a>A részállapot/esemény/állapota
hello következő táblázat elérhető eseményeket és állapotokat (és részállapotok) hello listája.

| esemény neve | status | A részállapot |
| --- | --- | --- |
| A tevékenység futtatása megkezdődött |Megkezdődött |Indulás alatt |
| A tevékenység futtatása befejeződött |Sikeres |Sikeres |
| A tevékenység futtatása befejeződött |Nem sikerült |Nem sikerült erőforrás-elosztás<br/><br/>Sikertelen végrehajtása<br/><br/>Túllépte az időkorlátot<br/><br/>Érvényesítés<br/><br/>Elhagyott |
| Igény szerinti HDI-fürtöt létrehozni elindítva |Megkezdődött |-|
| Igény szerinti HDI-fürtnek sikeresen létrehozva |Sikeres |-|
| Igény szerinti HDI-fürtnek törlése |Sikeres |-|

### <a name="tooedit-delete-or-disable-an-alert"></a>tooedit, törlése, vagy tiltsa le a riasztás

Használja a (pirossal kiemelt) gombok tooedit, delete vagy tiltsa le a riasztás a következő hello.

![Riasztások gombok](./media/data-factory-monitor-manage-app/AlertButtons.png)
