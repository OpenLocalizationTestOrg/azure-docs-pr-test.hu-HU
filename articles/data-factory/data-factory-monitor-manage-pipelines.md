---
title: "aaaMonitor és folyamatok kezelése hello Azure-portál és a PowerShell használatával |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure-portál és az Azure PowerShell toomonitor és hello Azure adat-előállítók és a létrehozott folyamatok kezelése."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 9b0fdc59-5bbe-44d1-9ebc-8be14d44def9
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: spelluru
ms.openlocfilehash: a8d3c7943e79450895ff754f06a37fdad1cbef92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-hello-azure-portal-and-powershell"></a>Figyelheti és kezelheti az Azure Data Factory folyamatok hello Azure-portál és a PowerShell használatával
> [!div class="op_single_selector"]
> * [Az Azure portálon vagy az Azure PowerShell használatával](data-factory-monitor-manage-pipelines.md)
> * [Használatával figyelése és a felügyeleti alkalmazás](data-factory-monitor-manage-app.md)


> [!IMPORTANT]
> hello figyelési & a felügyeleti alkalmazás figyelése és az adatok folyamatok kezelését és hibaelhárítását probléma merül fel egy jobb támogatást biztosít. Hello alkalmazás használatával kapcsolatos részletekért lásd: [felügyeletéhez és kezeléséhez az adat-előállító adatcsatornák hello figyelés és felügyelet alkalmazással](data-factory-monitor-manage-app.md). 


Ez a cikk ismerteti, hogyan toomonitor, kezelése és a folyamatok hibakeresése az Azure-portál és a PowerShell használatával. hello cikk információkat is biztosít toocreate riasztások és a get hibákkal kapcsolatos értesítést.

## <a name="understand-pipelines-and-activity-states"></a>Adatcsatornák és a tevékenység állapota
Hello Azure-portál használatával a következő műveletek végezhetők el:

* Megtekintheti a data factory mint ábrázoló diagram.
* A feldolgozási soros tevékenységek megtekintése.
* Bemeneti és kimeneti adatkészletek megtekintése.

Ez a szakasz azt is ismerteti, hogyan dataset szelet átkerül egy állapot tooanother állapotból.   

### <a name="navigate-tooyour-data-factory"></a>Keresse meg az adat-előállító tooyour
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson a **adat-előállítók** hello menü hello bal oldalon. Ha nem látja, kattintson a **további szolgáltatások >**, és kattintson a **adat-előállítók** alatt hello **ESZKÖZINTELLIGENCIA + ANALITIKA** kategóriát.

   ![Keresse meg az összes > adat-előállítók](./media/data-factory-monitor-manage-pipelines/browseall-data-factories.png)
3. A hello **adat-előállítók** panelen, jelölje be hello adat-előállító kíváncsiak vagyunk.

    ![Válassza ki az adat-előállító](./media/data-factory-monitor-manage-pipelines/select-data-factory.png)

   Hello kezdőlapját hello adat-előállító kell megjelennie.

   ![Data factory panel](./media/data-factory-monitor-manage-pipelines/data-factory-blade.png)

#### <a name="diagram-view-of-your-data-factory"></a>A data factory a diagram nézet
Hello **Diagram** egy adat-előállító nézete egytáblás, üveghatású toomonitor biztosít, és hello data factory és az eszközök kezelése. toosee hello **Diagram** , a data factory megtekintéséhez kattintson az **Diagram** hello kezdőlap hello adat-előállítóban.

![Diagramnézet](./media/data-factory-monitor-manage-pipelines/diagram-view.png)

Nagyítás, Kicsinyítés, nagyítás toofit, a Nagyítás too100 %, a zárolás hello hello diagram elrendezését, és adatcsatornákat és adathalmazokat automatikus formázása. Hello adatok Leszármaztatás információ is megtekinthető (Ez azt jelenti, hogy a kijelölt elemek felsőbb és alsóbb rétegbeli elemek megjelenítése).

### <a name="activities-inside-a-pipeline"></a>Egy folyamat belüli tevékenységek
1. Kattintson a jobb gombbal a hello folyamat, és kattintson **nyitott folyamat** toosee hello szereplő összes tevékenységnek a következő feldolgozási sorban, valamint bemeneti és kimeneti adatkészletek hello tevékenységekhez. Ez a szolgáltatás akkor hasznos, ha a feldolgozási sor egynél több tevékenységet tartalmaz, és azt szeretné, hogy toounderstand hello működési Leszármaztatás egyetlen folyamat.

    ![Folyamat megnyitása menü](./media/data-factory-monitor-manage-pipelines/open-pipeline-menu.png)     
2. A következő példa hello a másolási tevékenység során hello folyamat a bemeneti és egy kimeneti látható. 

    ![Egy folyamat belüli tevékenységek](./media/data-factory-monitor-manage-pipelines/activities-inside-pipeline.png)
3. Hello kattintva léphet vissza toohello kezdőlapján hello adat-előállító **adat-előállító** hello navigációs hello bal felső sarokban lévő hivatkozásra.

    ![Keresse meg a visszafelé toodata gyári](./media/data-factory-monitor-manage-pipelines/navigate-back-to-data-factory.png)

### <a name="view-hello-state-of-each-activity-inside-a-pipeline"></a>Minden tevékenység egy folyamat belül hello állapotának megtekintése
Egy tevékenység jelenlegi állapotában hello megtekintéséhez hello adatkészletek hello tevékenység által gyártott bármelyikének hello állapotának megtekintése.

Ehhez kattintson duplán a hello **OutputBlobTable** a hello **Diagram**, megtekintheti az összes hello szelet, amely különböző tevékenység fut egy folyamat belül hozzák létre. Láthatja, hogy hello másolási tevékenység már sikeresen futott az hello utolsó nyolc óra és hello hello szeletek előállított **készen** állapotát.  

![Hello folyamatának állapota](./media/data-factory-monitor-manage-pipelines/state-of-pipeline.png)

hello dataset szeletek hello adat-előállítóban hello a következő állapotok egyike lehet:

<table>
<tr>
    <th align="left">Állapot</th><th align="left">Alállapota</th><th align="left">Leírás</th>
</tr>
<tr>
    <td rowspan="8">Várakozás</td><td>ScheduleTime</td><td>hello szelet toorun hello ideje még nem érkezett.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>hello fölérendelt függőségek nem állnak készen.</td>
</tr>
<tr>
<td>ComputeResources</td><td>hello számítási erőforrások nem érhetők el.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Minden hello Tevékenységpéldány elfoglalva más szeletek futtatásával.</td>
</tr>
<tr>
<td>ActivityResume</td><td>hello tevékenység szüneteltetve van, és csak hello tevékenység folytatásakor hello szeletek futtatható.</td>
</tr>
<tr>
<td>Próbálja meg újra</td><td>Tevékenység végrehajtási lesz hajtva.</td>
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
<td>hello szelet feldolgozása folyamatban van.</td>
</tr>
<tr>
<td rowspan="4">Nem sikerült</td><td>Időtúllépésbe került</td><td>hello tevékenység végrehajtási hello tevékenység által megengedett érték időt vett igénybe.</td>
</tr>
<tr>
<td>Törölve</td><td>hello szelet felhasználói művelet megszakította.</td>
</tr>
<tr>
<td>Ellenőrzés</td><td>Sikertelen volt.</td>
</tr>
<tr>
<td>-</td><td>hello szelet toobe jön létre és/vagy érvényesítése nem sikerült.</td>
</tr>
<td>Készen áll</td><td>-</td><td>hello szelet készen áll a felhasználásra.</td>
</tr>
<tr>
<td>Kihagyva</td><td>None</td><td>hello szelet feldolgozása folyamatban nem.</td>
</tr>
<tr>
<td>None</td><td>-</td><td>A szelet használt tooexist egy eltérő állapottal, de a rendszer visszaállította.</td>
</tr>
</table>



A szelet bevitelének hello kattintva megtekintheti a szelet hello adatait **szeletek nemrég frissített** panelen.

![Szelet részletei](./media/data-factory-monitor-manage-pipelines/slice-details.png)

Ha hello szelet több alkalommal végre lett hajtva, hello több sor látható **tevékenység fut** listája. Hello futtatása hello bejegyzést kattintva futtatható tevékenységgel kapcsolatos részleteket **tevékenység fut** listája. hello listán látható összes hello naplófájl, valamint egy hibaüzenet, ha van ilyen. Ez a szolgáltatás akkor hasznos tooview és hibakeresési naplók a data factory tooleave nélkül.

![Tevékenységfuttatás részletei](./media/data-factory-monitor-manage-pipelines/activity-run-details.png)

Ha hello szelet nem a hello **készen áll a** állapot, hello felfelé irányuló szeletek nem állnak készen, és blokkol hello aktuális szelet hello végrehajtás alatt látható **felfelé irányuló szeletek nem áll készen** listája. A szolgáltatás akkor hasznos, ha a szelet **Várakozás** állapotát és azt szeretné, toounderstand hello fölérendelt függőségek, hogy a szelet hello vár a.

![Nem kész állapotú felfelé irányuló szeletek](./media/data-factory-monitor-manage-pipelines/upstream-slices-not-ready.png)

### <a name="dataset-state-diagram"></a>A DataSet állapot diagramja
Miután telepít egy adat-előállítót, és hello folyamatok érvényes aktív idővel rendelkeznek, hello dataset szeletek több állapotot tooanother átmenetet. Hello szelet állapota jelenleg a következő ábra állapot hello követi:

![Állapotdiagram](./media/data-factory-monitor-manage-pipelines/state-diagram.png)

hello dataset állapot átmenet folyamata a data factory hello alábbi: várakozás közben a rendszer -> folyamatban lévő vagy a-folyamatban (érvényesítés) -> kész/nem sikerült.

hello szelet elindul egy **Várakozás** állapot teljesíteni végrehajtása előtt az Előfeltételek toobe vár. Ezután hello tevékenység végrehajtásának elkezdése, és hello szelet kerülnek egy **folyamatban** állapotát. hello tevékenység végrehajtási előfordulhat, hogy sikeres vagy sikertelen. hello szelet jelölésű **készen** vagy **sikertelen**hello végrehajtási hello eredménye alapján.

Alaphelyzetbe állíthatja a hello szelet toogo újból az hello **készen** vagy **sikertelen** állapot toohello **Várakozás** állapotát. Megjelölheti a hello szelet állapota túl**kihagyása**, amely megakadályozza, hogy hello tevékenység végrehajtása, és nem a hello szelet feldolgozása.

## <a name="pause-and-resume-pipelines"></a>Felfüggesztése és folytatása folyamatok
A folyamatok Azure PowerShell használatával kezelheti. Például szüneteltetése és folytatása folyamatok Azure PowerShell-parancsmag futtatásával. 

> [!NOTE] 
> hello diagram nézet nem támogatja a felfüggesztése és folytatása folyamatok. Ha azt szeretné, hogy egy felhasználói felületet toouse, használja a hello figyeléséhez és felügyeletéhez alkalmazás. Hello alkalmazás használatával kapcsolatos részletekért lásd: [felügyeletéhez és kezeléséhez az adat-előállító adatcsatornák hello figyelés és felügyelet alkalmazással](data-factory-monitor-manage-app.md) cikk. 

Is szüneteltetése vagy felfüggesztjük folyamatok hello segítségével **Suspend-AzureRmDataFactoryPipeline** PowerShell-parancsmagot. Ez a parancsmag akkor hasznos, ha nem szeretné, hogy toorun a folyamatok egy probléma a megoldásáig. 

```powershell
Suspend-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
Példa:

```powershell
Suspend-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

Hello a probléma kijavítása a hello csővezeték, után folytathatja a felfüggesztett hello csővezeték hello a következő PowerShell-parancs futtatásával:

```powershell
Resume-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
Példa:

```powershell
Resume-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

## <a name="debug-pipelines"></a>Folyamatok hibakeresése
Az Azure Data Factory gazdag képességeket biztosít az Ön toodebug, és a folyamatok hibaelhárítása hello Azure-portál és az Azure PowerShell használatával.

> [! Vegye figyelembe} sokkal könnyebben használatával hibák hello figyelési tootroubleshot & felügyeleti alkalmazás. Hello alkalmazás használatával kapcsolatos részletekért lásd: [felügyeletéhez és kezeléséhez az adat-előállító adatcsatornák hello figyelés és felügyelet alkalmazással](data-factory-monitor-manage-app.md) cikk. 

### <a name="find-errors-in-a-pipeline"></a>Hiba található egy folyamatot
Hello tevékenységfuttatási egy folyamat sikertelen lesz, hello dataset hello folyamat által létrehozott van hello hibája miatt hibás állapotú. Hibakeresés, és az Azure Data Factory hibák elhárítása hello a következő módszerek használatával.

#### <a name="use-hello-azure-portal-toodebug-an-error"></a>Az Azure portál toodebug hiba hello használata
1. A hello **tábla** panelen hello probléma szeletre hello rendelkező **állapot** túl beállítása**sikertelen**.

   ![Probléma szelet tábla panelről](./media/data-factory-monitor-manage-pipelines/table-blade-with-error.png)
2. A hello **adatszelet** panelen kattintson a hello tevékenység futtatása sikertelen.

   ![Hiba történt az adatszelet](./media/data-factory-monitor-manage-pipelines/dataslice-with-error.png)
3. A hello **tevékenység fut részletek** panelen letöltheti a hello HDInsight feldolgozási társított hello fájlokat. Kattintson a **letöltése** állapot/stderr toodownload hello hiba naplófájlt, amely hello hiba részleteit tartalmazza.

   ![A következő hiba tevékenységfuttatási részleteit megjelenítő panelen](./media/data-factory-monitor-manage-pipelines/activity-run-details-with-error.png)     

#### <a name="use-powershell-toodebug-an-error"></a>Használjon PowerShell toodebug hiba
1. Indítsa el a **PowerShellt**.
2. Futtassa a hello **Get-AzureRmDataFactorySlice** toosee hello szeletek és az állapotok parancsot. A szelet hello állapottal kell megjelennie **sikertelen**.        

    ```powershell   
    Get-AzureRmDataFactorySlice [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime] <DateTime> [[-EndDateTime] <DateTime> ] [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```   
   Példa:

    ```powershell   
    Get-AzureRmDataFactorySlice -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime 2014-05-04 20:00:00
    ```

   Cserélje le **StartDateTime** az a folyamat kezdési idejét. 
3. Most, futtassa a hello **Get-AzureRmDataFactoryRun** hello szelet Futtatás hello tevékenység parancsmag tooget részleteit.

    ```powershell   
    Get-AzureRmDataFactoryRun [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime]
    <DateTime> [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```

    Példa:

    ```powershell   
    Get-AzureRmDataFactoryRun -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime "5/5/2014 12:00:00 AM"
    ```

    hello StartDateTime értéke hello hiba vagy probléma szelet hello előző lépésben feljegyzett hello kezdési idejét. hello dátum idejű dupla idézőjelek között kell megadni.
4. Hello hiba, amely hasonló toohello következő részleteivel kimenetnek kell megjelennie:

    ```   
    Id                      : 841b77c9-d56c-48d1-99a3-8c16c3e77d39
    ResourceGroupName       : ADF
    DataFactoryName         : LogProcessingFactory3
    DatasetName               : EnrichedGameEventsTable
    ProcessingStartTime     : 10/10/2014 3:04:52 AM
    ProcessingEndTime       : 10/10/2014 3:06:49 AM
    PercentComplete         : 0
    DataSliceStart          : 5/5/2014 12:00:00 AM
    DataSliceEnd            : 5/6/2014 12:00:00 AM
    Status                  : FailedExecution
    Timestamp               : 10/10/2014 3:04:52 AM
    RetryAttempt            : 0
    Properties              : {}
    ErrorMessage            : Pig script failed with exit code '5'. See wasb://        adfjobs@spestore.blob.core.windows.net/PigQuery
                                    Jobs/841b77c9-d56c-48d1-99a3-
                8c16c3e77d39/10_10_2014_03_04_53_277/Status/stderr' for
                more details.
    ActivityName            : PigEnrichLogs
    PipelineName            : EnrichGameLogsPipeline
    Type                    :
    ```
5. Hello futtatása **mentés-AzureRmDataFactoryLog** hello azonosítóérték hello kimeneti megtekintheti és hello használva töltik le a hello naplófájlok parancsmagot **- DownloadLogsoption** hello parancsmag.

    ```powershell
    Save-AzureRmDataFactoryLog -ResourceGroupName "ADF" -DataFactoryName "LogProcessingFactory" -Id "841b77c9-d56c-48d1-99a3-8c16c3e77d39" -DownloadLogs -Output "C:\Test"
    ```

## <a name="rerun-failures-in-a-pipeline"></a>Futtassa újra a feldolgozási hibák

> [!IMPORTANT]
> Egyszerűbb tootroubleshoot hibák, és futtassa újra a sikertelen szeletek használatával hello figyelés & a felügyeleti alkalmazás. Hello alkalmazás használatával kapcsolatos részletekért lásd: [felügyeletéhez és kezeléséhez az adat-előállító adatcsatornák hello figyelés és felügyelet alkalmazással](data-factory-monitor-manage-app.md). 

### <a name="use-hello-azure-portal"></a>Hello Azure portál használata
Hibaelhárítás és a feldolgozási hibák hibakeresése, után újra futtathatja hibák toohello hiba szelet Navigálás, majd kattintson a hello **futtatása** hello parancssáv gombjára.

![Futtassa újra a hibás szeletet](./media/data-factory-monitor-manage-pipelines/rerun-slice.png)

Esetben hello szelet meghiúsult érvényesítési házirend hiba miatt (például ha adatokat nem érhető el), javítsa ki a hello hibát, és újra hello gombra kattintva érvényesítenie **ellenőrzése** hello parancssáv gombjára.

![Javítsa a hibákat, és érvényesítése](./media/data-factory-monitor-manage-pipelines/fix-error-and-validate.png)

### <a name="use-azure-powershell"></a>Azure PowerShell használatával
Hibák hello segítségével futtathatja **Set-AzureRmDataFactorySliceStatus** parancsmag. Lásd: hello [Set-AzureRmDataFactorySliceStatus](https://msdn.microsoft.com/library/mt603522.aspx) témakör szintaxisát és egyéb hello parancsmag részleteit.

**Példa**

hello alábbi mintakód hello állapotának összes szeletek hello tábla "DAWikiAggregatedData" too'Waiting a "hello Azure data factoryban"WikiADF".

too'UpstreamInPipeline beállítása "Frissítés típusa" Hello ", ami azt jelenti, hogy minden szelet hello tábla és az összes hello függő (fölérendelt) tábla állapotok too'Waiting beállítása". hello más lehetséges Ez a paraméter értéke "Egyéni".

```powershell
Set-AzureRmDataFactorySliceStatus -ResourceGroupName ADF -DataFactoryName WikiADF -DatasetName DAWikiAggregatedData -Status Waiting -UpdateType UpstreamInPipeline -StartDateTime 2014-05-21T16:00:00 -EndDateTime 2014-05-21T20:00:00
```

## <a name="create-alerts"></a>Riasztások létrehozása
Azure naplók felhasználói eseményeket egy Azure-erőforrás (például a data factory) létrehozott, frissíteni vagy törölni. Ezek az események a riasztásokat hozhat létre. Használja a Data Factory toocapture különböző metrikákat, és hozzon létre a riasztások mérőszámokat. Azt javasoljuk, hogy használja az eseményeket a valós idejű figyelését, és metrikák használja az előzmények elérése érdekében.

### <a name="alerts-on-events"></a>Riasztások a események
Az Azure események mi történik az Azure-erőforrások a hasznos információkat adnak. Azure Data Factory használatakor események akkor jönnek létre, ha:

* Egy adat-előállító létrehozott, frissíteni vagy törölni.
* Adatok feldolgozása (a "fut") van elindítva, vagy befejeződött.
* Igény szerinti HDInsight-fürtök létrehozásakor vagy eltávolításakor.

A felhasználó eseményekből létre riasztásokat, és konfigurálásuk toosend e-mail értesítések toohello rendszergazda és coadministrators hello előfizetés. Emellett a felhasználók, akik tooreceive értesítő e-mailek hello feltételek teljesülése esetén további e-mail címeket is megadhat. Ez a szolgáltatás akkor hasznos, ha szeretné, hogy tooget hibáiról értesítést kap, és nem szeretné toocontinuously a figyelő a data factory.

> [!NOTE]
> Jelenleg hello portál nem riasztások megjelenítése a események. Használjon hello [figyelés és a felügyeleti alkalmazás](data-factory-monitor-manage-app.md) toosee minden riasztás.


#### <a name="specify-an-alert-definition"></a>Adjon meg egy értesítési definíciója
egy riasztás definíciójának toospecify, létrehozhat egy JSON-fájl, amely leírja a riasztást kap a toobe kívánt hello műveletek. A következő példa hello hello riasztás hello RunFinished művelet az e-mailben értesítést küld. adott toobe, e-mailben értesítést küldött hello adat-előállítóban futtatás befejeződött, és hello futtatása sikertelen volt (állapot = FailedExecution).

```JSON
{
    "contentVersion": "1.0.0.0",
     "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "parameters": {},
    "resources":
    [
        {
            "name": "ADFAlertsSlice",
            "type": "microsoft.insights/alertrules",
            "apiVersion": "2014-04-01",
            "location": "East US",
            "properties":
            {
                "name": "ADFAlertsSlice",
                "description": "One or more of hello data slices for hello Azure Data Factory has failed processing.",
                "isEnabled": true,
                "condition":
                {
                    "odata.type": "Microsoft.Azure.Management.Insights.Models.ManagementEventRuleCondition",
                    "dataSource":
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleManagementEventDataSource",
                        "operationName": "RunFinished",
                        "status": "Failed",
                        "subStatus": "FailedExecution"   
                    }
                },
                "action":
                {
                    "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                    "customEmails": [ "<your alias>@contoso.com" ]
                }
            }
        }
    ]
}
```

Eltávolíthatja **részállapot** a JSON-definícióból, ha nem szeretné, hogy egy adott hiba esetén figyelmeztetés toobe hello.

Ebben a példában az előfizetés az összes adat-előállítók hello riasztás beállítása. Ha azt szeretné, hogy állítsa be az egy adott data factory hello riasztási toobe, megadhatja az adat-előállító **resourceUri** a hello **dataSource**:

```JSON
"resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>"
```

hello következő táblázat elérhető műveletek és állapotok (és részállapotok) hello listája.

| A művelet neve | status | A részállapot |
| --- | --- | --- |
| RunStarted |Megkezdődött |Indulás alatt |
| RunFinished |Nem sikerült / sikeres volt. |FailedResourceAllocation<br/><br/>Sikeres<br/><br/>FailedExecution<br/><br/>Időtúllépésbe került<br/><br/>< megszakítva<br/><br/>FailedValidation<br/><br/>Elhagyott |
| OnDemandClusterCreateStarted |Megkezdődött | |
| OnDemandClusterCreateSuccessful |Sikeres | |
| OnDemandClusterDeleted |Sikeres | |

Lásd: [riasztási szabály létrehozása](https://msdn.microsoft.com/library/azure/dn510366.aspx) hello JSON-elemek szerepelnek hello példában használt vonatkozó további információért.

#### <a name="deploy-hello-alert"></a>Hello riasztás telepítése
toodeploy hello riasztás használata hello Azure PowerShell-parancsmag **New-AzureRmResourceGroupDeployment**, ahogy az alábbi példa hello:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\ADFAlertFailedSlice.json  
```

Miután hello erőforrás csoport központi telepítése sikeresen befejeződött, a következő üzenetek hello jelenik meg:

```
VERBOSE: 7:00:48 PM - Template is valid.
WARNING: 7:00:48 PM - hello StorageAccountName parameter is no longer used and will be removed in a future release.
Please update scripts tooremove this parameter.
VERBOSE: 7:00:49 PM - Create template deployment 'ADFAlertFailedSlice'.
VERBOSE: 7:00:57 PM - Resource microsoft.insights/alertrules 'ADFAlertsSlice' provisioning status is succeeded

DeploymentName    : ADFAlertFailedSlice
ResourceGroupName : adf
ProvisioningState : Succeeded
Timestamp         : 10/11/2014 2:01:00 AM
Mode              : Incremental
TemplateLink      :
Parameters        :
Outputs           :
```

> [!NOTE]
> Használhatja a hello [riasztást szabály létrehozása](https://msdn.microsoft.com/library/azure/dn510366.aspx) REST API toocreate riasztási szabályt. hello JSON-adattartalmat hasonló toohello JSON példaként szolgál.  


#### <a name="retrieve-hello-list-of-azure-resource-group-deployments"></a>Hello listáját, az Azure erőforrás-csoport központi telepítések
tooretrieve hello listája az Azure erőforrás-csoport központi telepítése, hello parancsmaggal **Get-AzureRmResourceGroupDeployment**, ahogy az alábbi példa hello:

```powershell
Get-AzureRmResourceGroupDeployment -ResourceGroupName adf
```

```
DeploymentName    : ADFAlertFailedSlice
ResourceGroupName : adf
ProvisioningState : Succeeded
Timestamp         : 10/11/2014 2:01:00 AM
Mode              : Incremental
TemplateLink      :
Parameters        :
Outputs           :
```

#### <a name="troubleshoot-user-events"></a>Felhasználói események hibaelhárítása
1. Megtekintheti a hello kattintás után létrehozott összes hello események **metrikák és műveletek** csempére.

    ![Metrikák és műveletek csempe](./media/data-factory-monitor-manage-pipelines/metrics-and-operations-tile.png)
2. Kattintson a hello **események** toosee hello események csempére.

    ![Események csempéje](./media/data-factory-monitor-manage-pipelines/events-tile.png)
3. A hello **események** panelen láthatja, hogy eseményeket, szűrt eseményeket, és egyéb adatait.

    ![Események panel](./media/data-factory-monitor-manage-pipelines/events-blade.png)
4. Kattintson egy **művelet** hello műveletek lista, amely a hibát.

    ![Válasszon ki egy műveletet](./media/data-factory-monitor-manage-pipelines/select-operation.png)
5. Kattintson egy **hiba** hello hiba esemény toosee részleteit.

    ![Esemény hiba](./media/data-factory-monitor-manage-pipelines/operation-error-event.png)

Lásd: [Azure Insight parancsmagok](https://msdn.microsoft.com/library/mt282452.aspx) tooadd használt PowerShell-parancsmagokkal, beolvasása, vagy távolítsa el a riasztásokat. Néhány példa a hello segítségével **Get-AlertRule** parancsmagot:

```powershell
get-alertrule -res $resourceGroup -n ADFAlertsSlice -det
```

```
Properties :
Action      : Microsoft.Azure.Management.Insights.Models.RuleEmailAction
Condition   :
DataSource :
EventName             :
Category              :
Level                 :
OperationName         : RunFinished
ResourceGroupName     :
ResourceProviderName  :
ResourceId            :
Status                : Failed
SubStatus             : FailedExecution
Claims                : Microsoft.Azure.Management.Insights.Models.RuleManagementEventClaimsDataSource
Condition      :
Description : One or more of hello data slices for hello Azure Data Factory has failed processing.
Status      : Enabled
Name:       : ADFAlertsSlice
Tags       :
$type          : Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage
Id: /subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/ADFAlertsSlice
Location   : West US
Name       : ADFAlertsSlice
```

```powershell
Get-AlertRule -res $resourceGroup
```
```
Properties : Microsoft.Azure.Management.Insights.Models.Rule
Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
Location   : West US
Name       : FailedExecutionRunsWest0

Properties : Microsoft.Azure.Management.Insights.Models.Rule
Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest3
Location   : West US
Name       : FailedExecutionRunsWest3
```

```powershell
Get-AlertRule -res $resourceGroup -Name FailedExecutionRunsWest0
```

```
Properties : Microsoft.Azure.Management.Insights.Models.Rule
Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
Location   : West US
Name       : FailedExecutionRunsWest0
```

Futtassa a get-help parancsok toosee részletek és a példákat hello Get-AlertRule parancsmag a következő hello.

```powershell
get-help Get-AlertRule -detailed
```

```powershell
get-help Get-AlertRule -examples
```


Ha látható a riasztás előállítás események hello hello portál panel, de nem kap értesítő e-mailek, ellenőrizze, hogy hello e-mail címet, amely a megadott külső feladóktól tooreceive e-mailek van beállítva. hello értesítő e-mailek előfordulhat, hogy le lettek tiltva az e-mail beállítások szerint.

### <a name="alerts-on-metrics"></a>Riasztások a metrikák
A Data Factory különböző metrikák rögzítése, és létre riasztásokat mérőszámokat. Figyelheti, és értesítést létrehozni a következő metrikák hello szeletek az adat-előállítóban hello:

* **Nem sikerült fut.**
* **Sikeres futtatása**

A metrikák hasznosak, és segítséget nyújtson tooget hello adat-előállítóban futtató áttekintését a teljes és a sikertelen. Minden alkalommal, amikor nincs szelet futtató metrikák kibocsátott. Elején hello hello óra, a metrikák összesítése és leküldött tooyour tárfiók. a storage-fiók beállítása tooenable metrikákat.

#### <a name="enable-metrics"></a>Metrikák engedélyezése
tooenable metrika, kattintson a hello hello következő **adat-előállító** panel:

**Figyelési** > **metrika** > **diagnosztikai beállítások** > **diagnosztika**

![Diagnosztika hivatkozás](./media/data-factory-monitor-manage-pipelines/diagnostics-link.png)

A hello **diagnosztika** panelen kattintson a **a**, válassza ki hello tárfiókot, és kattintson **mentése**.

![Diagnosztika panel](./media/data-factory-monitor-manage-pipelines/diagnostics-blade.png)

Másolatot hello metrikák toobe hello látható tooone órát vehet igénybe **figyelés** panel mert metrikák összesítési óránként történik.

### <a name="set-up-an-alert-on-metrics"></a>A metrikák riasztás beállítása
Kattintson a hello **adat-előállító metrikák** csempén:

![Data factory metrikák csempe](./media/data-factory-monitor-manage-pipelines/data-factory-metrics-tile.png)

A hello **metrika** panelen kattintson a **+ Hozzáadás riasztás** hello eszköztáron.
![Data factory metrika panel > riasztás hozzáadása](./media/data-factory-monitor-manage-pipelines/add-alert.png)

A hello **riasztási szabály felvétele** lapon hello a következő lépéseket, és kattintson a **OK**.

* Adjon meg egy nevet hello riasztás (Példa: "riasztás sikertelen").
* Adjon meg egy leírást, hello riasztás (Példa: "e-mail küldési hiba esetén").
* Jelöljön ki egy metrikát ("Futtatása sikertelen" vs. "Sikeres futtatása").
* Adjon meg egy feltételt és egy küszöbértéket.   
* Adja meg a hello időn belül.
* Adja meg, hogy egy e-mailt kell küldeni, tooowners, közreműködőknek és olvasóknak.

![Data factory metrika panel > Hozzáadás riasztási szabálya](./media/data-factory-monitor-manage-pipelines/add-an-alert-rule.png)

Hello riasztási szabályt hozzáadása sikeresen, hello panel bezárása után, és megtekintheti hello új riasztás hello után **metrika** panelen.

![Data factory metrika panel > Új értesítés hozzáadása](./media/data-factory-monitor-manage-pipelines/failed-alert-in-metric-blade.png)

Emellett meg kell jelennie a hello értesítések hello száma **riasztási szabályok** csempére. Kattintson a hello **riasztási szabályok** csempére.

![Data factory metrika panelről – riasztási szabályok](./media/data-factory-monitor-manage-pipelines/alert-rules-tile-rules.png)

A hello **szabályok riasztást** panelen bármely létező riasztások megtekintéséhez. Kattintson az értesítés, tooadd **riasztás hozzáadása** hello eszköztáron.

![A riasztási szabályok panel](./media/data-factory-monitor-manage-pipelines/alert-rules-blade.png)

### <a name="alert-notifications"></a>Riasztási értesítések
Miután hello riasztási szabály hello feltétel megegyezik, egy e-mailt, amely szerint a hello riasztás aktiválva szerezheti be. Miután hello probléma megoldódott, és többé nem egyezik a hello riasztási feltétel, kap egy e-mailt, amely szerint a hello riasztás oldódik meg.

Ez a viselkedés eltér események hol egy értesítést küld az összes hiba, amely jogosult riasztási szabályt.

### <a name="deploy-alerts-by-using-powershell"></a>Riasztások telepítését a PowerShell használatával
A metrikák hello azonos riasztások telepítheti úgy, hogy az eseményeket.

**A riasztás definíciója**

```JSON
{
    "contentVersion" : "1.0.0.0",
    "$schema" : "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "parameters" : {},
    "resources" : [
    {
            "name" : "FailedRunsGreaterThan5",
            "type" : "microsoft.insights/alertrules",
            "apiVersion" : "2014-04-01",
            "location" : "East US",
            "properties" : {
                "name" : "FailedRunsGreaterThan5",
                "description" : "Failed Runs greater than 5",
                "isEnabled" : true,
                "condition" : {
                    "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.ThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
                    "odata.type" : "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
                    "dataSource" : {
                        "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
                        "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
                        "resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName
>/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>",
                        "metricName" : "FailedRuns"
                    },
                    "threshold" : 5.0,
                    "windowSize" : "PT3H",
                    "timeAggregation" : "Total"
                },
                "action" : {
                    "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
                    "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                    "customEmails" : ["abhinav.gpt@live.com"]
                }
            }
        }
    ]
}
```

Cserélje le *subscriptionId*, *resourceGroupName*, és *dataFactoryName* hello mintában megfelelő értékekkel.

*metricName* jelenleg csak a két érték támogatja:

* FailedRuns
* SuccessfulRuns

**Hello riasztás telepítése**

toodeploy hello riasztás használata hello Azure PowerShell-parancsmag **New-AzureRmResourceGroupDeployment**, ahogy az alábbi példa hello:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\FailedRunsGreaterThan5.json
```

Üzenet a sikeres telepítés után a következő kell megjelennie:

```
VERBOSE: 12:52:47 PM - Template is valid.
VERBOSE: 12:52:48 PM - Create template deployment 'FailedRunsGreaterThan5'.
VERBOSE: 12:52:55 PM - Resource microsoft.insights/alertrules 'FailedRunsGreaterThan5' provisioning status is succeeded


DeploymentName    : FailedRunsGreaterThan5
ResourceGroupName : adf
ProvisioningState : Succeeded
Timestamp         : 7/27/2015 7:52:56 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
Outputs           
```

Is használhatja a hello **Add-AlertRule** parancsmag toodeploy riasztási szabályt. Lásd: hello [Add-AlertRule](https://msdn.microsoft.com/library/mt282468.aspx) részletek és példákat a témakör.  

## <a name="move-a-data-factory-tooa-different-resource-group-or-subscription"></a>Helyezze át a data factory tooa másik erőforráscsoportba vagy előfizetésbe
Áthelyezheti a data factory tooa eltérő erőforráscsoportban vagy egy másik előfizetésben található hello segítségével **áthelyezése** sáv gombra a data factory hello kezdőlapján parancsot.

![Adat-előállító áthelyezése](./media/data-factory-monitor-manage-pipelines/MoveDataFactory.png)

Is áthelyezheti bármely kapcsolódó erőforrások (például a riasztások hello adat-előállító társított), adat-előállító hello együtt.

![Források párbeszédpanelt](./media/data-factory-monitor-manage-pipelines/MoveResources.png)
