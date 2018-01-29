---
title: "SSIS-csomag használata az Azure Data Factory - tárolt eljárási tevékenység meghívása |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan lehet meghívni egy Azure Data Factory-folyamat a tárolt eljárási tevékenység használja az SQL Server Integration Services (SSIS) csomag."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: powershell
ms.topic: article
ms.date: 12/07/2017
ms.author: jingwang
ms.openlocfilehash: 749deb6549e0ac90da4b44424026c897108a4bb7
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/19/2018
---
# <a name="invoke-an-ssis-package-using-stored-procedure-activity-in-azure-data-factory"></a>Egy SSIS-csomagot, a tárolt eljárási tevékenység az Azure Data Factory meghívása
Ez a cikk ismerteti, hogyan lehet meghívni egy SSIS-csomagot az Azure Data Factory-folyamat a tárolt eljárási tevékenység használatával. 

> [!NOTE]
> Ez a cikk a Data Factory 2. verziójára vonatkozik, amely jelenleg előzetes verzióban érhető el. A Data Factory szolgáltatásnak, amely általánosan elérhető (GA), 1 verziójának használatakor lásd [meghívása SSIS-csomagok használata tárolt eljárási tevékenység az 1-es](v1/how-to-invoke-ssis-package-stored-procedure-activity.md).

## <a name="prerequisites"></a>Előfeltételek

### <a name="azure-sql-database"></a>Azure SQL Database 
Ez a cikk a forgatókönyv, amelyen az SSIS-katalógus Azure SQL-adatbázis. Egy Azure SQL felügyelt példányát (magán előnézetben) is használhatja.

## <a name="create-an-azure-ssis-integration-runtime"></a>Azure SSIS integrációs modul létrehozása
Ha még nem rendelkezik a részletes utasításokat a következő, hozzon létre egy Azure-SSIS-integrációs futásidejű a [oktatóanyag: telepítése SSIS-csomagok](tutorial-deploy-ssis-packages-azure.md).

## <a name="data-factory-ui-azure-portal"></a>Data Factory felhasználói felület (Azure-portál)
Ebben a szakaszban a Data Factory felhasználói felület egy tárolt eljárás tevékenység, amely hívja meg az SSIS-csomag létrehozása a Data Factory-folyamat használhat.

### <a name="create-a-data-factory"></a>Data factory létrehozása
Első lépés az adat-előállító létrehozása az Azure-portál használatával. 

1. Lépjen az [Azure Portalra](https://portal.azure.com). 
2. Kattintson az **Új** elemre, majd az **Adatok + analitika**, végül a **Data Factory** elemre. 
   
   ![New (Új)->DataFactory](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-azure-data-factory-menu.png)
2. Az a **új adat-előállító** lapján adja meg **ADFTutorialDataFactory** a a **neve**. 
      
     ![Új előállító lap](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-azure-data-factory.png)
 
   Az Azure data factory nevének **globálisan egyedinek** kell lennie. Ha a név mező a következő hibát látja, módosítsa az adat-előállítóban (például yournameADFTutorialDataFactory) nevét. Lásd: [Data Factory - elnevezési szabályait](naming-rules.md) cikk az adat-előállító összetevők elnevezési szabályait.
  
     ![Neve nem áll rendelkezésre – hiba](./media/how-to-invoke-ssis-package-stored-procedure-activity/name-not-available-error.png)
3. Válassza ki azt az **Azure-előfizetést**, amelyben az adat-előállítót létre szeretné hozni. 
4. Az **erőforráscsoportban** hajtsa végre a következő lépések egyikét:
     
      - Kattintson a **Meglévő használata** elemre, majd a legördülő listából válasszon egy meglévő erőforráscsoportot. 
      - Kattintson az **Új létrehozása** elemre, és adja meg az erőforráscsoport nevét.   
         
    Az erőforráscsoportokkal kapcsolatos információkért tekintse meg a [Using resource groups to manage your Azure resources](../azure-resource-manager/resource-group-overview.md) (Erőforráscsoportok használata az Azure-erőforrások kezeléséhez) című cikket.  
4. Válassza a **V2 (előzetes verzió)** értéket a **verzió** esetén.
5. Válassza ki a Data Factory **helyét**. Csak a Data Factory által támogatott helyek egyaránt megjelennek az a legördülő listában. Az adatokat tárolja (Azure Storage, Azure SQL Database stb.), és kiszámítja az (HDInsight, stb.) adat-előállító által használt más helyeken.
6. Válassza a **Rögzítés az irányítópulton** lehetőséget.     
7. Kattintson a **Create** (Létrehozás) gombra.
8. Az irányítópulton megjelenő csempén a következő állapotleírás látható: **Adat-előállító üzembe helyezése**. 

    ![adat-előállító üzembe helyezése csempe](media//how-to-invoke-ssis-package-stored-procedure-activity/deploying-data-factory.png)
9. A létrehozásának befejezése után megjelenik a **adat-előállító** lapon, az ábrán látható módon.
   
    ![Data factory kezdőlap](./media/how-to-invoke-ssis-package-stored-procedure-activity/data-factory-home-page.png)
10. Kattintson a **Szerző & figyelő** csempe az Azure Data Factory felhasználói felület (UI) alkalmazást egy külön lapján. 

### <a name="create-a-pipeline-with-stored-procedure-activity"></a>Hozzon létre egy folyamatot tárolt eljárási tevékenység
Ebben a lépésben használatával a Data Factory felhasználói felületén hozzon létre egy folyamatot. A tárolt eljárás tevékenység hozzáadása a folyamat, és konfigurálja úgy, hogy a sp_executesql tárolt eljárás használatával futtassa a SSIS-csomag. 

1. A get elindított oldalon kattintson **létrehozási folyamat**: 

    ![Első lépéseket ismertető oldal felkereséséhez](./media/how-to-invoke-ssis-package-stored-procedure-activity/get-started-page.png)
2. Az a **tevékenységek** eszközkészlet, bontsa ki a **SQL-adatbázis**, és az egérrel **tárolt eljárás** tevékenység és a feldolgozási sor desginer felületet. 

    ![Fogd és vidd tárolt eljárási tevékenység](./media/how-to-invoke-ssis-package-stored-procedure-activity/drag-drop-sproc-activity.png)
3. A tárolt eljárás tevékenység tulajdonságai ablakban váltson a **SQL fiók** fülre, majd **+ új**. Kapcsolatot hoz létre az Azure SQL Database, amely futtatja az SSIS-katalógusban (adatbázis SSIDB). 
   
    ![Új társított szolgáltatás gomb](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-linked-service-button.png)
4. Az a **új társított szolgáltatás** ablakban tegye a következőket: 

    1. Válassza ki **Azure SQL adatbázis** a **típus**.
    2. Válassza ki az Azure SQL-kiszolgáló, amelyen az SSISDB adatbázis a **kiszolgálónév** mező.
    3. Válassza ki **SSISDB** a **adatbázisnév**.
    4. A **felhasználónév**, adja meg a nevét, a felhasználó, aki hozzáféréssel rendelkezik az adatbázishoz.
    5. A **jelszó**, adja meg a felhasználó jelszavát. 
    6. Tesztelje a kapcsolatot az adatbázis kattintva **tesztkapcsolat** gombra.
    7. A társított szolgáltatás gombra kattintva mentse a **mentése** gombra. 

        ![Azure SQL Database társított szolgáltatás](./media/how-to-invoke-ssis-package-stored-procedure-activity/azure-sql-database-linked-service-settings.png)
5. A Tulajdonságok ablakban váltson a **tárolt eljárás** a lap a **SQL fiók** lapot, és hajtsa végre a következő lépéseket: 

    1. Az a **tárolt eljárás neve** mezőbe írjon be `sp_executesql` . 
    2. Kattintson a **+ új** a a **tárolt eljárás paramétereit** szakasz. 
    3. A **neve** adja meg a paraméter **utasításban**. 
    4. A **típus** adja meg a paraméter **karakterlánc** . 
    5. A **érték** a paraméter adja meg a következő SQL-lekérdezésben.

        Az SQL-lekérdezést, adja meg a megfelelő értékeket a **mappa_neve**, **projektnév**, és **csomag_neve** paraméterek. 

        ```sql
        DECLARE @return_value INT, @exe_id BIGINT, @err_msg NVARCHAR(150)    EXEC @return_value=[SSISDB].[catalog].[create_execution] @folder_name=N'<FOLDER name in SSIS Catalog>', @project_name=N'<PROJECT name in SSIS Catalog>', @package_name=N'<PACKAGE name>.dtsx', @use32bitruntime=0, @runinscaleout=1, @useanyworker=1, @execution_id=@exe_id OUTPUT    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1    EXEC [SSISDB].[catalog].[start_execution] @execution_id=@exe_id, @retry_count=0    IF(SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id=@exe_id)<>7 BEGIN SET @err_msg=N'Your package execution did not succeed for execution ID: ' + CAST(@exe_id AS NVARCHAR(20)) RAISERROR(@err_msg,15,1) END
        ```

        ![Azure SQL Database társított szolgáltatás](./media/how-to-invoke-ssis-package-stored-procedure-activity/stored-procedure-settings.png)
6. Az adatcsatorna konfiguráció érvényesítése céljából kattintson **ellenőrzése** az eszköztáron. Bezárja a **folyamat ellenőrzési jelentés**, kattintson a  **>>** .

    ![Feldolgozási sor ellenőrzése](./media/how-to-invoke-ssis-package-stored-procedure-activity/validate-pipeline.png)
7. A feldolgozási sor közzétételére az adat-előállító kattintva **összes** gombra. 

    ![Közzététel](./media/how-to-invoke-ssis-package-stored-procedure-activity/publish-all-button.png)    

### <a name="run-and-monitor-the-pipeline"></a>Futtathatja és figyelheti a folyamatot
Ebben a szakaszban indít folyamatot futtató, és megfigyeli azt. 

1. Futtassa egy folyamat indításához kattintson **eseményindító** az eszköztáron, és kattintson **aktiválhatja most**. 

    ![Aktiválhatja most](./media/how-to-invoke-ssis-package-stored-procedure-activity/trigger-now.png)
2. Váltás a **figyelő** a bal oldali lapon. A Futtatás futószalag és egyéb információk (például futtassa Kezdés időpontja) mellett állapota látható. Frissítse a nézetet, kattintson a **frissítése**.

    ![Folyamatfuttatások](./media/how-to-invoke-ssis-package-stored-procedure-activity/pipeline-runs.png)
3. Kattintson a **nézet tevékenység fut** hivatkozásra a **műveletek** oszlop. Futtassa az adatcsatorna esetében van (a tárolt eljárási tevékenység) csak egy tevékenység csak egy tevékenység megjelenik.

    ![Tevékenységek](./media/how-to-invoke-ssis-package-stored-procedure-activity/activity-runs.png) futtathatja a következő 4 **lekérdezés** szemben az SSISDB adatbázis annak ellenőrzésére, hogy az Azure SQL Server a csomag végrehajtása. 

    ```sql
    select * from catalog.executions
    ```

    ![Ellenőrizze a csomag végrehajtások](./media/how-to-invoke-ssis-package-stored-procedure-activity/verify-package-executions.png)

Is létrehozhat egy ütemezett eseményindító a tölcsér, hogy az adatcsatorna (houly, napi, stb.) ütemezés szerint futtatott. Egy vonatkozó példáért lásd: [tartalmazó data factory - létrehozása a Data Factory felhasználói felület](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule).

## <a name="azure-powershell"></a>Azure PowerShell
Ebben a szakaszban Azure PowerShell egy tárolt eljárás tevékenység, amely hívja meg az SSIS-csomag létrehozása a Data Factory-folyamat használja. 

Kövesse [az Azure PowerShell telepítését és konfigurálását](/powershell/azure/install-azurerm-ps) ismertető cikkben szereplő utasításokat a legújabb Azure PowerShell-modulok telepítéséhez. 

### <a name="create-a-data-factory"></a>Data factory létrehozása
Használja az ugyanazon adat-előállítóban, amely rendelkezik az Azure-SSIS infravörös, vagy hozzon létre egy külön adat-előállítóban. Az alábbi eljárás lépéseit egy adat-előállító létrehozása. A tárolt eljárási tevékenység a data factory hoz létre egy folyamatot. A tárolt eljárási tevékenység végrehajtja a tárolt eljárás az SSISDB adatbázis a SSIS-csomag futtatásához. 

1. Adjon meg egy olyan változót, amelyet később a PowerShell-parancsokban az erőforráscsoport neveként fog használni. Másolja az alábbi parancsszöveget a PowerShellbe, adja meg az [Azure-erőforráscsoport](../azure-resource-manager/resource-group-overview.md) nevét idézőjelek között, majd futtassa a parancsot. Például: `"adfrg"`. 
   
     ```powershell
    $resourceGroupName = "ADFTutorialResourceGroup";
    ```

    Ha az erőforráscsoport már létezik, előfordulhat, hogy nem kívánja felülírni. Rendeljen egy másik értéket a `$ResourceGroupName` változóhoz, majd futtassa újra a parancsot
2. Futtassa az alábbi parancsot az Azure-erőforráscsoport létrehozásához: 

    ```powershell
    $ResGrp = New-AzureRmResourceGroup $resourceGroupName -location 'eastus'
    ``` 
    Ha az erőforráscsoport már létezik, előfordulhat, hogy nem kívánja felülírni. Rendeljen egy másik értéket a `$ResourceGroupName` változóhoz, majd futtassa újra a parancsot. 
3. Adjon meg egy változót az adat-előállító nevéhez. 

    > [!IMPORTANT]
    >  Frissítse az adat-előállító nevét, hogy globálisan egyedi legyen. 

    ```powershell
    $DataFactoryName = "ADFTutorialFactory";
    ```

5. Az adat-előállító létrehozásához futtassa az alábbi **Set-AzureRmDataFactoryV2** parancsmagot a $ResGrp változó Location és ResourceGroupName tulajdonsága használatával: 
    
    ```powershell       
    $DataFactory = Set-AzureRmDataFactoryV2 -ResourceGroupName $ResGrp.ResourceGroupName -Location $ResGrp.Location -Name $dataFactoryName 
    ```

Vegye figyelembe a következő szempontokat:

* Az Azure data factory nevének globálisan egyedinek kell lennie. Ha a következő hibaüzenetet kapja, módosítsa a nevet, majd próbálkozzon újra.

    ```
    The specified Data Factory name 'ADFv2QuickStartDataFactory' is already in use. Data Factory names must be globally unique.
    ```
* Data Factory-példányok létrehozásához a felhasználói fióknak, amellyel belép az Azure-ba, a **közreműködő** vagy **tulajdonos** szerepkörök tagjának, vagy az Azure-előfizetés **rendszergazdájának** kell lennie.
* A Data Factory 2-es verziója jelenleg csak az USA keleti régiójában, az USA 2. keleti régiójában és a nyugat-európai régióban teszi lehetővé adat-előállítók létrehozását. Az adat-előállítók által használt adattárak (Azure Storage, Azure SQL Database stb.) és számítási erőforrások (HDInsight stb.) más régiókban is lehetnek.

### <a name="create-an-azure-sql-database-linked-service"></a>Azure SQL Database-beli társított szolgáltatás létrehozása
A társított szolgáltatás, amely futtatja az Azure SQL database összekapcsolására. az SSIS-katalógusban a data factory létrehozása. Data Factory szolgáltatásnak információk használatával kapcsolódik az SSISDB adatbázist, és végrehajtja a tárolt eljárás egy SSIS-csomag futtatásához. 

1. Hozzon létre egy JSON fájlt **AzureSqlDatabaseLinkedService.json** a **C:\ADF\RunSSISPackage** mappa a következő tartalmát: 

    > [!IMPORTANT]
    > Cserélje le &lt;kiszolgálónév&gt;, &lt;felhasználónév&gt;, és &lt;jelszó&gt; a fájl mentése előtt az Azure SQL Database értékekkel.

    ```json
    {
        "name": "AzureSqlDbLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "Server=tcp:<servername>.database.windows.net,1433;Database=SSISDB;User ID=<username>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
                }
            }
        }
    }
    ```

2. A **Azure PowerShell**, váltson a **C:\ADF\RunSSISPackage** mappa.

3. Futtassa a **Set-AzureRmDataFactoryV2LinkedService** parancsmagot az **AzureSqlDatabaseLinkedService** társított szolgáltatás létrehozásához. 

    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "AzureSqlDatabaseLinkedService" -File ".\AzureSqlDatabaseLinkedService.json"
    ```

### <a name="create-a-pipeline-with-stored-procedure-activity"></a>Hozzon létre egy folyamatot tárolt eljárási tevékenység 
Ebben a lépésben hoz létre egy folyamatot egy tárolt eljárás tevékenység. A tevékenység a SSIS-csomag futtatásához a sp_executesql tárolt eljárást hív meg. 

1. Hozzon létre egy JSON fájlt **RunSSISPackagePipeline.json** a a **C:\ADF\RunSSISPackage** mappa a következő tartalmát:

    > [!IMPORTANT]
    > Cserélje le &lt;MAPPANÉV&gt;, &lt;projekt neve&gt;, &lt;CSOMAGNÉV&gt; nevű mappa, a projekt és a csomag az SSIS-katalógusban a fájl mentése előtt. 

    ```json
    {
        "name": "RunSSISPackagePipeline",
        "properties": {
            "activities": [
                {
                    "name": "My SProc Activity",
                    "description":"Runs an SSIS package",
                    "type": "SqlServerStoredProcedure",
                    "linkedServiceName": {
                        "referenceName": "AzureSqlDbLinkedService",
                        "type": "LinkedServiceReference"
                    },
                    "typeProperties": {
                        "storedProcedureName": "sp_executesql",
                        "storedProcedureParameters": {
                            "stmt": {
                                "value": "DECLARE @return_value INT, @exe_id BIGINT, @err_msg NVARCHAR(150)    EXEC @return_value=[SSISDB].[catalog].[create_execution] @folder_name=N'<FOLDER NAME>', @project_name=N'<PROJECT NAME>', @package_name=N'<PACKAGE NAME>', @use32bitruntime=0, @runinscaleout=1, @useanyworker=1, @execution_id=@exe_id OUTPUT    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1    EXEC [SSISDB].[catalog].[start_execution] @execution_id=@exe_id, @retry_count=0    IF(SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id=@exe_id)<>7 BEGIN SET @err_msg=N'Your package execution did not succeed for execution ID: ' + CAST(@exe_id AS NVARCHAR(20)) RAISERROR(@err_msg,15,1) END"
                            }
                        }
                    }
                }
            ]
        }
    }
    ```

2. A folyamat létrehozása: **RunSSISPackagePipeline**- ben futtassa a **Set-AzureRmDataFactoryV2Pipeline** parancsmag.

    ```powershell
    $DFPipeLine = Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "RunSSISPackagePipeline" -DefinitionFile ".\RunSSISPackagePipeline.json"
    ```

    Itt látható a minta kimenete:

    ```
    PipelineName      : Adfv2QuickStartPipeline
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Activities        : {CopyFromBlobToBlob}
    Parameters        : {[inputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification], [outputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification]}
    ```

### <a name="create-a-pipeline-run"></a>Folyamat futásának létrehozása
Használja a **Invoke-AzureRmDataFactoryV2Pipeline** parancsmag futtatjuk a folyamatot. A parancsmag visszaadja a folyamat futásának azonosítóját a későbbi monitorozás céljából.

```powershell
$RunId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -PipelineName $DFPipeLine.Name
```

### <a name="monitor-the-pipeline-run"></a>A folyamat futásának monitorozása

A folyamat futási állapotának folyamatos, az adatok másolásának befejezéséig tartó ellenőrzéséhez futtassa az alábbi PowerShell-szkriptet. Másolja/illessze be az alábbi szkriptet a PowerShell-ablakba, majd nyomja le az Enter billentyűt. 

```powershell
while ($True) {
    $Run = Get-AzureRmDataFactoryV2PipelineRun -ResourceGroupName $ResGrp.ResourceGroupName -DataFactoryName $DataFactory.DataFactoryName -PipelineRunId $RunId

    if ($Run) {
        if ($run.Status -ne 'InProgress') {
            Write-Output ("Pipeline run finished. The status is: " +  $Run.Status)
            $Run
            break
        }
        Write-Output  "Pipeline is running...status: InProgress"
    }

    Start-Sleep -Seconds 10
}   
```

### <a name="create-a-trigger"></a>Egy eseményindító létrehozása
Az előző lépésben a csővezeték-igény szerinti meghívni. Egy ütemezés eseményindító fussanak ütemezés szerint (óránként, naponta, stb.) a feldolgozási sor is létrehozhat.

1. Hozzon létre egy JSON fájlt **MyTrigger.json** a **C:\ADF\RunSSISPackage** mappa a következő tartalmát: 

    ```json
    {
        "properties": {
            "name": "MyTrigger",
            "type": "ScheduleTrigger",
            "typeProperties": {
                "recurrence": {
                    "frequency": "Hour",
                    "interval": 1,
                    "startTime": "2017-12-07T00:00:00-08:00",
                    "endTime": "2017-12-08T00:00:00-08:00"
                }
            },
            "pipelines": [{
                    "pipelineReference": {
                        "type": "PipelineReference",
                        "referenceName": "RunSSISPackagePipeline"
                    },
                    "parameters": {}
                }
            ]
        }
    }    
    ```
2. A **Azure PowerShell**, váltson a **C:\ADF\RunSSISPackage** mappa.
3. Futtassa a **Set-AzureRmDataFactoryV2Trigger** parancsmaggal hozhat létre az eseményindító. 

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName -DataFactoryName $DataFactory.DataFactoryName -Name "MyTrigger" -DefinitionFile ".\MyTrigger.json"
    ```
4. Alapértelmezés szerint az eseményindító leállított állapotban van. Indítsa el az eseményindító futtatásával a **Start-AzureRmDataFactoryV2Trigger** parancsmag. 

    ```powershell
    Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName -DataFactoryName $DataFactory.DataFactoryName -Name "MyTrigger" 
    ```
5. Győződjön meg arról, hogy az eseményindító elindításával a **Get-AzureRmDataFactoryV2Trigger** parancsmag. 

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"     
    ```    
6. A következő parancsot a következő óra múlva. Például ha az aktuális idő du. 3:25 UTC, futtassa a parancsot du. 4 UTC. 
    
    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-06" -TriggerRunStartedBefore "2017-12-09"
    ```

    A következő lekérdezés alapján futtathatók az SSISDB adatbázis annak ellenőrzésére, hogy az Azure SQL Server a csomag végrehajtása. 

    ```sql
    select * from catalog.executions
    ```


## <a name="next-steps"></a>További lépések
Ugyanígy figyelheti a folyamatot, az Azure portál használatával. Részletes útmutatásért lásd: [a folyamat figyelése](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).
