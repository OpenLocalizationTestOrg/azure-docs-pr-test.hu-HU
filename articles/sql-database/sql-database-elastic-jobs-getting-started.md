---
title: "a rugalmas feladatok használatába aaaGetting |} Microsoft Docs"
description: "Hogyan toouse rugalmas adatbázis-feladatok"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 2540de0e-2235-4cdd-9b6a-b841adba00e5
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: ddove
ms.openlocfilehash: bc5894d2df4235738ab961db4f69c11cdf786cc6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-elastic-database-jobs"></a>Ismerkedés a rugalmas adatbázis-feladatok
Rugalmas adatbázis-feladatok (előzetes verzió) az Azure SQL Database lehetővé teszi tooreliability hajtható végre T-SQL-parancsfájlok, amelyek több adatbázis több közben automatikus újrapróbálkozása és végleges befejezési biztosító biztosítja, hogy az. Hello rugalmas feladat szolgáltatással kapcsolatos további információkért lásd: hello [szolgáltatás – áttekintés oldalra](sql-database-elastic-jobs-overview.md).

Ebben a témakörben található hello minta bővíti [Ismerkedés a rugalmas adatbáziseszközöket](sql-database-elastic-scale-get-started.md). Befejezése után lesz: megtudhatja, hogyan toocreate feladatok által kezelt kapcsolódó adatbázisok csoportja és kezelését. Már nem szükséges toouse hello rugalmas bővítést eszközök a rugalmas feladatok hello előnyei rendelés tootake előnyeit.

## <a name="prerequisites"></a>Előfeltételek
Töltse le és futtassa a hello [Ismerkedés a rugalmas adatbázis eszközök minta](sql-database-elastic-scale-get-started.md).

## <a name="create-a-shard-map-manager-using-hello-sample-app"></a>A shard térkép létrehozásához-kezelőt hello mintaalkalmazás
Itt hoz létre a shard térképre manager együtt több szilánkok hello szilánkok be az adatok beszúrását követ. Ha már rendelkezik a bennük foglalt horizontálisan skálázott adatok állítsa be a szilánkok, hello lépéseket kihagyhatja, és helyezze át a következő szakaszban toohello.

1. Hozza létre, és futtassa a hello **Ismerkedés a rugalmas adatbáziseszközöket** mintaalkalmazást. Amíg hello szakasz 7. lépés hello lépésekkel [hello minta alkalmazás letöltése és futtatása](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app). A 7. lépés hello végén jelenik meg a következő parancssor hello:

   ![parancssor](./media/sql-database-elastic-query-getting-started/cmd-prompt.png)

2. Hello parancsablakot, írja be az "1", és nyomja le az ENTER **Enter**. Hello shard térkép manager hoz létre, és két szilánkok toohello kiszolgálót. Ezután írja be a "3", és nyomja le az ENTER **Enter**; Ez a művelet megismétlése négy alkalommal. Ez a szilánkok minta adatsorok szúrja be.
3. Hello [Azure Portal](https://portal.azure.com) három új adatbázist kell megjelenítenie:

   ![A Visual Studio-jóváhagyás](./media/sql-database-elastic-query-getting-started/portal.png)

   Ezen a ponton létre fogunk hozni egy egyéni adatbázis-gyűjteményt, amely hello shard térkép összes hello adatbázisának tükrözi. Ezzel lehetővé teszik a számunkra toocreate és végrehajtani egy feladatot, amely az új táblázat hozzáadása szilánkok között.

Itt azt kellene általában shard térkép cél létrehozásához, hello segítségével **New-AzureSqlJobTarget** parancsmag. hello shard manager adatbázist be kell állítani egy adatbázis célként, és majd hello adott shard térkép cél van megadva. Ehelyett azt folyamatos tooenumerate hello kiszolgáló összes hello adatbázisának és hello adatbázisok toohello új egyéni gyűjtemény hozzáadása fő adatbázis hello kivétel miatt.

## <a name="creates-a-custom-collection-and-add-all-databases-in-hello-server-toohello-custom-collection-target-with-hello-exception-of-master"></a>Létrehoz egy egyéni gyűjteményt, és adja hozzá az összes adatbázis hello server toohello egyéni gyűjtemény cél főkiszolgáló hello kivétel miatt.
   ```
    $customCollectionName = "dbs_in_server"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $ResourceGroupName = "ddove_samples"
    $ServerName = "samples"
    $dbsinserver = Get-AzureRMSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName
    $dbsinserver | %{
    $currentdb = $_.DatabaseName
    $ErrorActionPreference = "Stop"
    Write-Output ""

    Try
    {
       New-AzureSqlJobTarget -ServerName $ServerName -DatabaseName $currentdb | Write-Output
    }
    Catch
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason

        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already a database target."
        }

        else
        {
            throw $_
        }

    }

    Try
    {
        if ($currentdb -eq "master")
        {
            Write-Host $currentdb "will not be added custom collection target" $CustomCollectionName "."
        }

        else
        {
            Add-AzureSqlJobChildTarget -CustomCollectionName $CustomCollectionName -ServerName $ServerName -DatabaseName $currentdb
            Write-Host $currentdb "was added to" $CustomCollectionName "."
        }

    }
    Catch
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason

        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already in hello custom collection target" $CustomCollectionName"."
        }

        else
        {
            throw $_
        }
    }
    $ErrorActionPreference = "Continue"
   }
   ```
## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Az adatbázisok közötti végrehajtási T-SQL parancsfájl létrehozása
   ```
    $scriptName = "NewTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'Test')
    BEGIN
        CREATE TABLE Test(
            TestId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO Test(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script
   ```

## <a name="create-hello-job-tooexecute-a-script-across-hello-custom-group-of-databases"></a>Teremtsen hello feladat tooexecute parancsfájl hello egyéni adatbázisok csoportja

   ```
    $jobName = "create on server dbs"
    $scriptName = "NewTable"
    $customCollectionName = "dbs_in_server"
    $credentialName = "ddove66"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job
   ```

## <a name="execute-hello-job"></a>Hello feladat végrehajtása
a következő PowerShell-parancsfájl hello használt tooexecute egy meglévő feladat lehet:

Frissítés hello változó tooreflect hello kívánt feladat neve toohave hajtotta végre a következő:

   ```
    $jobName = "create on server dbs"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution
   ```

## <a name="retrieve-hello-state-of-a-single-job-execution"></a>Egyetlen feladat-végrehajtási hello állapotának beolvasása
Használja az azonos hello **Get-AzureSqlJobExecution** hello parancsmagot **IncludeChildren** paraméter tooview hello állapotának gyermek feladat végrehajtások, nevezetesen hello minden egyes feladatok végrehajtásának meghatározott állapotban hello feladat által megcélzott adatbázis.

   ```
    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions
   ```

## <a name="view-hello-state-across-multiple-job-executions"></a>Több feladat végrehajtások közötti hello állapotának megtekintése
Hello **Get-AzureSqlJobExecution** parancsmag rendelkezik több nem kötelező paraméter, amely használt toodisplay több feladat végrehajtások, szűrt hello megadott paramétereknek. hello következő hello lehetséges módjait toouse Get-AzureSqlJobExecution némelyike mutatja be:

Lekéri az összes aktív felső szintű feladat végrehajtások:

   ```
    Get-AzureSqlJobExecution
   ```

Lekéri az összes felső szintű feladat végrehajtások, beleértve az inaktív feladat végrehajtások:

   ```
    Get-AzureSqlJobExecution -IncludeInactive
   ```

Lekéri az összes gyermek feladat végrehajtások inaktív feladat végrehajtások többek között a megadott feladat végrehajtási Azonosítóhoz:

   ```
    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId -IncludeInactive -IncludeChildren
   ```

Ütemezés használatával létrehozott összes feladat végrehajtások beolvasása / feladat-kombináció, beleértve az inaktív feladatok:

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive
   ```

Lekéri az összes feladat megadott shard térképre, beleértve az inaktív feladatokat célcsoportkezelést:

   ```
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive
   ```

Lekéri az összes feladat a megadott egyéni gyűjtemény, beleértve az inaktív feladatokat célcsoportkezelést:

   ```
    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive
   ```

Feladat feladat végrehajtások belül egy adott feladat végrehajtási hello listájának beolvasása:

   ```
    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions
   ```

Feladat végrehajtási részlete beolvasása:

a következő PowerShell-parancsfájl hello lehet egy feladat a feladat a végrehajtás, amely akkor különösen hasznos, ha végrehajtási hibakeresésére használt tooview hello részleteit.
   ```
    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution
   ```

## <a name="retrieve-failures-within-job-task-executions"></a>Sikertelen feladat feladat végrehajtások belül beolvasása
hello JobTaskExecution objektum tartalmaz egy hello együtt egy üzenettulajdonság hello feladat életciklusa tulajdonság. Ha egy feladat a feladat végrehajtása sikertelen volt, hello életciklus tulajdonság túl állítható*sikertelen* és hello üzenettulajdonság lesz toohello eredményül kapott hibaüzenetet, és a verem. Ha egy feladat sikertelen volt, akkor fontos tooview hello részleteit munka feladatai, amelyek egy adott feladat sikertelen volt.

   ```
    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions)
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }
   ```

## <a name="waiting-for-a-job-execution-toocomplete"></a>Várakozás a feladat végrehajtási toocomplete
a következő PowerShell-parancsfájl hello lehet egy feladat feladat toocomplete a használt toowait:

   ```
    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId
   ```

## <a name="create-a-custom-execution-policy"></a>Egyéni végrehajtási házirend létrehozása
Rugalmas adatbázis-feladatok feladat indításakor alkalmazható egyéni végrehajtási házirendek létrehozását támogatja.

Végrehajtási házirendek definiálása jelenleg engedélyezése:

* Name: Azonosítója hello végrehajtási házirendet.
* Feladat időtúllépése: Teljes idő előtt rugalmas adatbázis-feladatok megszakítja a feladatot.
* Kezdeti újrapróbálkozási időköz: Intervallum toowait előtt próbálkozik újra.
* Maximális újrapróbálkozási időköz: Az újrapróbálkozási intervallumok toouse kap.
* Újrapróbálkozási időköz leállítási együttható: Együttható használt toocalculate hello következő intervallum újrapróbálkozások között.  hello alábbi képlet használható: (kezdeti újrapróbálkozási időközét) * Math.pow ((időköz leállítási együttható), (próbálkozások száma) - 2).
* Kísérletek maximális száma: hello maximális száma újrapróbálkozási kísérletek tooperform feladat.

hello alapértelmezett végrehajtási házirend hello a következő értékeket használja:

* Name: Alapértelmezett végrehajtási házirend
* Feladat időtúllépése: 1 hét
* Kezdeti újrapróbálkozási időköz: 100 milliomod másodperc
* Maximális újrapróbálkozási időköz: 30 perc
* Ismételje meg a időköz együttható: 2. régiója
* Kísérletek maximális száma: 2 147 483 647

Szükségeskonfiguráció-hello végrehajtási szabályzat létrehozása:

   ```
    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy
   ```

### <a name="update-a-custom-execution-policy"></a>Egy egyéni végrehajtási házirend frissítése
Frissítés hello végrehajtási házirend tooupdate szükséges:

   ```
    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy
   ```

## <a name="cancel-a-job"></a>Feladatok megszakítása
Rugalmas adatbázis-feladatok feladatok megszakítását kérelmeket támogatja.  Rugalmas adatbázis-feladatok jelenleg végrehajtás alatt álló feladat a lemondási kérelmet észleli, ha a program megpróbálja toostop hello feladat.

Rugalmas adatbázis-feladatok is hajtsa végre a megszakítási két különböző módja van:

1. Megszakítása jelenleg feldolgozás alatt álló feladatok: törlése közben jelenleg fut egy feladat észleli, ha a megszakítási belül jelenleg végrehajtás alatt álló hello feladat aspektusa hello történt kísérlet.  Példa: Ha egy hosszú ideig tartó jelenleg végrehajtás alatt álló lekérdezés törlése megkísérelt van, nem lesznek egy kísérlet toocancel hello lekérdezést.
2. Zároló feladat újrapróbálkozások: Törlése észlelésekor hello vezérlő szál feladat a végrehajtás elindítása előtt hello vezérlő szál fog elkerüléséhez hello feladat elindítása és hello kérelem deklarálható megszakítottként.

Ha a feladat megszakításának szülő-feladat van szükség, hello lemondási kérelmet szerződéses kötelezettségeket a hello szülő feladat és összes a gyermek-feladatokkal.

a megszakítási kérelmet, toosubmit hello használata **Stop-AzureSqlJobExecution** parancsmag és a set hello **JobExecutionId** paraméter.

   ```
    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId
   ```

## <a name="delete-a-job-by-name-and-hello-jobs-history"></a>A feladat nevét és hello feladatok előzményeinek törlése
Rugalmas adatbázis-feladatok támogatja az aszinkron feladatok törlése. Egy feladat is törlésre, és hello rendszer törölni fogja a hello feladat és a feladatelőzményekben összes feladat végrehajtások hello feladat befejezése után. hello rendszer nem fogja automatikusan megszakítja a aktív feladat végrehajtások.  

Ehelyett a Stop-AzureSqlJobExecution meghívott toocancel aktív feladat végrehajtások kell lennie.

tootrigger feladat törlése, használjon hello **Remove-AzureSqlJob** parancsmag és a set hello **JobName** paraméter.

   ```
    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
   ```

## <a name="create-a-custom-database-target"></a>Hozzon létre egy egyéni adatbázis-cél
Egyéni adatbázis célok meghatározása a rugalmas adatbázis-feladatokhoz, amelyek a végrehajtás közvetlenül vagy egy egyéni adatbázis csoportban foglalható is használható. Mivel a **rugalmas készletek** nem még közvetlenül támogatottak hello PowerShell API-k, keresztül egyszerűen létrehozhat egy egyéni adatbázis célként és egy egyéni adatbázis gyűjtemény célként, ami magában foglalja az összes hello adatbázis hello készletben.

Állítsa be a következő változók tooreflect hello szükséges adatbázis-információ hello:

   ```
    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName
   ```

## <a name="create-a-custom-database-collection-target"></a>Hozzon létre egy egyéni adatbázis-gyűjtemény célja
Egyéni adatbázis gyűjtemény célja lehet meghatározott tooenable végrehajtása több meghatározott adatbázis cél között. Egy adatbázis-csoport létrehozása után adatbázisok társított toohello egyéni gyűjtemény célja lehet.

Állítsa be a következő változók tooreflect hello kívánt egyéni gyűjtemény konfigurációjához hello:

   ```
    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName
   ```

### <a name="add-databases-tooa-custom-database-collection-target"></a>Adatbázisok tooa egyéni adatbázis gyűjtemény cél hozzáadása
Adatbázis célok társítható egyéni adatbázis gyűjtemény célok toocreate adatbázisok csoportja. Amikor egy feladat jön létre, amely egyéni adatbázis gyűjtemény céloz, bővített tootarget hello adatbázisok társított toohello csoport végrehajtás hello időpontban lesz.

Szükségeskonfiguráció-hello adatbázis tooa meghatározott egyéni gyűjtemény hozzáadása:

   ```
    $serverName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName
   ```

#### <a name="review-hello-databases-within-a-custom-database-collection-target"></a>Tekintse át a hello adatbázisok belül egy egyéni adatbázis-gyűjtemény célja
Használjon hello **Get-AzureSqlJobTarget** parancsmag tooretrieve hello gyermek adatbázis egy egyéni adatbázis-gyűjtemény célja.

   ```
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets
   ```

### <a name="create-a-job-tooexecute-a-script-across-a-custom-database-collection-target"></a>Hozzon létre egy feladat tooexecute egy parancsprogramot egy egyéni adatbázis-gyűjtemény célja között
Használjon hello **New-AzureSqlJob** parancsmag toocreate egy feladatot, egy egyéni adatbázis gyűjtemény tároló által definiált adatbázisok csoportja ellen. Rugalmas adatbázis-feladatok hello feladat kibővítése minden megfelelő tooa adatbázis társított hello egyéni adatbázis-gyűjtemény célja, és győződjön meg arról, hogy minden egyes adatbázison hello parancsfájl végrehajtása több gyermek feladat. Ebben az esetben fontos, hogy a parancsfájlok az idempotent toobe rugalmas tooretries.

   ```
    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job
   ```

## <a name="data-collection-across-databases"></a>Az adatbázisok közötti adatok gyűjtése
**Rugalmas adatbázis-feladatok** támogatja, a lekérdezés végrehajtása adatbázisok csoportja között, és elküldi a hello eredmények tooa megadott adatbázis tábla. hello tábla hello tény toosee hello lekérdezés eredményében minden adatbázisból után kérdezhetők le. Így lehetővé teszi egy aszinkron mechanizmus tooexecute a lekérdezés több adatbázis közötti. Hiba az esetekben egy hello adatbázisok alatt átmenetileg nem érhető el, automatikusan kezeli, újrapróbálkozások keresztül.

hello megadott céltábla automatikusan létrejön, ha még nem létezik, a megfelelő hello sémája hello eredményhalmazt adott. A parancsfájl végrehajtása több eredménykészlet adja vissza, ha a rugalmas adatbázis-feladatok csak küldi hello első megadott egy toohello célként megadott táblája.

a következő PowerShell-parancsfájl hello használt tooexecute gyűjtése az eredményeket egy megadott táblába parancsfájlt is lehet. Ez a parancsfájl feltételezi, hogy egy T-SQL parancsfájl létrehozva a következő amelyek kimenete egy eredményhalmaz és létrehoztak egy egyéni adatbázis gyűjtemény célhelyet.

Állítsa be a következő tooreflect szükséges hello parancsfájl, a hitelesítő adatokat és a végrehajtási célját hello:

   ```
    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $executionCredentialName = "{Execution Credential Name}"
    $customCollectionName = "{Custom Collection Name}"
    $destinationCredentialName = "{Destination Credential Name}"
    $destinationServerName = "{Destination Server Name}"
    $destinationDatabaseName = "{Destination Database Name}"
    $destinationSchemaName = "{Destination Schema Name}"
    $destinationTableName = "{Destination Table Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
   ```

### <a name="create-and-start-a-job-for-data-collection-scenarios"></a>Hozzon létre, és elindíthat egy feladatot a adatáttelepítések gyűjtemény esetében
   ```
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $executionCredentialName -ContentName $scriptName -ResultSetDestinationServerName $destinationServerName -ResultSetDestinationDatabaseName $destinationDatabaseName -ResultSetDestinationSchemaName $destinationSchemaName -ResultSetDestinationTableName $destinationTableName -ResultSetDestinationCredentialName $destinationCredentialName -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution
   ```

## <a name="create-a-schedule-for-job-execution-using-a-job-trigger"></a>A feladat végrehajtását egy feladat eseményindító ütemezés létrehozása
a következő PowerShell-parancsfájl hello lehet használt toocreate feladatról ütemezés szerint. A parancsfájl egy percen, de a New-AzureSqlJobSchedule is támogat - DayInterval, - HourInterval, - MonthInterval, és - WeekInterval paraméterek. Csak egyszer hajtható végre ütemezéseket is létrehozható, hogy - alkalommal.

Új ütemezés létrehozása:
   ```
    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule -MinuteInterval $minuteInterval -ScheduleName $scheduleName -StartTime $startTime
    Write-Output $schedule
   ```

### <a name="create-a-job-trigger-toohave-a-job-executed-on-a-time-schedule"></a>Egy feladat eseményindító toohave ütemezéssel végrehajtott feladat létrehozása
Egy feladat eseményindító lehet meghatározott toohave egy feladat végrehajtása függően tooa ütemezéssel. a következő PowerShell-parancsfájl hello lehet használt toocreate feladat eseményindítót.

Állítsa be a következő változók toocorrespond toohello kívánt feladat hello és ütemezése:

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName
    Write-Output $jobTrigger
   ```

### <a name="remove-a-scheduled-association-toostop-job-from-executing-on-schedule"></a>Ütemezés szerint végrehajtása ütemezett társítás toostop feladat eltávolítása
Ismétlődés feladat végrehajtása a feladat eseményindítót, hello feladat eseményindító keresztül toodiscontinue távolítható el.
Távolítsa el a egy feladat eseményindító toostop egy feladat a végrehajtás alatt álló hello függően tooa ütemezés **Remove-AzureSqlJobTrigger** parancsmag.

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName
   ```

## <a name="import-elastic-database-query-results-tooexcel"></a>A rugalmas adatbázis-lekérdezés eredménye tooExcel importálása
 A lekérdezés tooan Excel fájl eredményei hello importálhatja.

1. Indítsa el az Excel 2013.
2. Keresse meg a toohello **adatok** menüszalagján.
3. Kattintson a **egyéb forrásokból származó** kattintson **az SQL Server**.

   ![Excel importálása más forrásokból](./media/sql-database-elastic-query-getting-started/exel-sources.png)

4. A hello **Adatkapcsolat varázsló** írja be a hello kiszolgálónév és hitelesítő adatait. Ezután kattintson a **Next** (Tovább) gombra.
5. Hello párbeszédpanelen **hello adatokat tartalmazó adatbázis válassza hello**, jelölje be hello **ElasticDBQuery** adatbázis.
6. Jelölje be hello **ügyfelek** hello listanézetben táblázatban, majd kattintson **következő**. Kattintson a **Befejezés**.
7. A hello **és adatokat importálhat** űrlap **válassza ki, hogy tooview ezeket az adatokat a munkafüzet**, jelölje be **tábla** kattintson **OK**.

Az összes sorát hello **ügyfelek** a különböző szilánkok tárolt tábla feltöltéséhez hello Excel-táblában.

## <a name="next-steps"></a>Következő lépések
Most már használhatja az Excel adatok funkciók. Hello kapcsolati karakterláncot használ a kiszolgáló nevét, az adatbázisnév és a hitelesítő adatok tooconnect a BI és az integráció eszközök toohello rugalmas adatbázis lekérdezése. Győződjön meg arról, hogy az SQL Server támogatja-e az eszköz adatforrásként. Tekintse meg a toohello rugalmas lekérdezési adatbázis és a külső táblák csakúgy, mint bármely más SQL Server-adatbázis és, hogy Ön kapcsolódnának toowith az eszköz az SQL Server-táblákra.

### <a name="cost"></a>Költségek
Nem kell külön fizetni a hello rugalmas adatbázis-lekérdezés szolgáltatása van. Jelenleg ez a szolgáltatás rendelkezésre áll csak a premium adatbázisokat a végpont, azonban bármely szolgáltatásréteg hello szilánkok lehet.

Díjszabási információkért lásd: [SQL adatbázis díjszabás](https://azure.microsoft.com/pricing/details/sql-database/).

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
