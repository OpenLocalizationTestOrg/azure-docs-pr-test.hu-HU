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
# <a name="getting-started-with-elastic-database-jobs"></a><span data-ttu-id="566fb-103">Ismerkedés a rugalmas adatbázis-feladatok</span><span class="sxs-lookup"><span data-stu-id="566fb-103">Getting started with Elastic Database jobs</span></span>
<span data-ttu-id="566fb-104">Rugalmas adatbázis-feladatok (előzetes verzió) az Azure SQL Database lehetővé teszi tooreliability hajtható végre T-SQL-parancsfájlok, amelyek több adatbázis több közben automatikus újrapróbálkozása és végleges befejezési biztosító biztosítja, hogy az.</span><span class="sxs-lookup"><span data-stu-id="566fb-104">Elastic Database jobs (preview) for Azure SQL Database allows you tooreliability execute T-SQL scripts that span multiple databases while automatically retrying and providing eventual completion guarantees.</span></span> <span data-ttu-id="566fb-105">Hello rugalmas feladat szolgáltatással kapcsolatos további információkért lásd: hello [szolgáltatás – áttekintés oldalra](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="566fb-105">For more information about hello Elastic Database job feature, please see hello [feature overview page](sql-database-elastic-jobs-overview.md).</span></span>

<span data-ttu-id="566fb-106">Ebben a témakörben található hello minta bővíti [Ismerkedés a rugalmas adatbáziseszközöket](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="566fb-106">This topic extends hello sample found in [Getting started with Elastic Database tools](sql-database-elastic-scale-get-started.md).</span></span> <span data-ttu-id="566fb-107">Befejezése után lesz: megtudhatja, hogyan toocreate feladatok által kezelt kapcsolódó adatbázisok csoportja és kezelését.</span><span class="sxs-lookup"><span data-stu-id="566fb-107">When completed, you will: learn how toocreate and manage jobs that manage a group of related databases.</span></span> <span data-ttu-id="566fb-108">Már nem szükséges toouse hello rugalmas bővítést eszközök a rugalmas feladatok hello előnyei rendelés tootake előnyeit.</span><span class="sxs-lookup"><span data-stu-id="566fb-108">It is not required toouse hello Elastic Scale tools in order tootake advantage of hello benefits of Elastic jobs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="566fb-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="566fb-109">Prerequisites</span></span>
<span data-ttu-id="566fb-110">Töltse le és futtassa a hello [Ismerkedés a rugalmas adatbázis eszközök minta](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="566fb-110">Download and run hello [Getting started with Elastic Database tools sample](sql-database-elastic-scale-get-started.md).</span></span>

## <a name="create-a-shard-map-manager-using-hello-sample-app"></a><span data-ttu-id="566fb-111">A shard térkép létrehozásához-kezelőt hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="566fb-111">Create a shard map manager using hello sample app</span></span>
<span data-ttu-id="566fb-112">Itt hoz létre a shard térképre manager együtt több szilánkok hello szilánkok be az adatok beszúrását követ.</span><span class="sxs-lookup"><span data-stu-id="566fb-112">Here you will create a shard map manager along with several shards, followed by insertion of data into hello shards.</span></span> <span data-ttu-id="566fb-113">Ha már rendelkezik a bennük foglalt horizontálisan skálázott adatok állítsa be a szilánkok, hello lépéseket kihagyhatja, és helyezze át a következő szakaszban toohello.</span><span class="sxs-lookup"><span data-stu-id="566fb-113">If you already have shards set up with sharded data in them, you can skip hello following steps and move toohello next section.</span></span>

1. <span data-ttu-id="566fb-114">Hozza létre, és futtassa a hello **Ismerkedés a rugalmas adatbáziseszközöket** mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="566fb-114">Build and run hello **Getting started with Elastic Database tools** sample application.</span></span> <span data-ttu-id="566fb-115">Amíg hello szakasz 7. lépés hello lépésekkel [hello minta alkalmazás letöltése és futtatása](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app).</span><span class="sxs-lookup"><span data-stu-id="566fb-115">Follow hello steps until step 7 in hello section [Download and run hello sample app](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app).</span></span> <span data-ttu-id="566fb-116">A 7. lépés hello végén jelenik meg a következő parancssor hello:</span><span class="sxs-lookup"><span data-stu-id="566fb-116">At hello end of Step 7, you will see hello following command prompt:</span></span>

   ![parancssor](./media/sql-database-elastic-query-getting-started/cmd-prompt.png)

2. <span data-ttu-id="566fb-118">Hello parancsablakot, írja be az "1", és nyomja le az ENTER **Enter**.</span><span class="sxs-lookup"><span data-stu-id="566fb-118">In hello command window, type "1" and press **Enter**.</span></span> <span data-ttu-id="566fb-119">Hello shard térkép manager hoz létre, és két szilánkok toohello kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="566fb-119">This creates hello shard map manager, and adds two shards toohello server.</span></span> <span data-ttu-id="566fb-120">Ezután írja be a "3", és nyomja le az ENTER **Enter**; Ez a művelet megismétlése négy alkalommal.</span><span class="sxs-lookup"><span data-stu-id="566fb-120">Then type "3" and press **Enter**; repeat this action four times.</span></span> <span data-ttu-id="566fb-121">Ez a szilánkok minta adatsorok szúrja be.</span><span class="sxs-lookup"><span data-stu-id="566fb-121">This inserts sample data rows in your shards.</span></span>
3. <span data-ttu-id="566fb-122">Hello [Azure Portal](https://portal.azure.com) három új adatbázist kell megjelenítenie:</span><span class="sxs-lookup"><span data-stu-id="566fb-122">hello [Azure Portal](https://portal.azure.com) should show three new databases:</span></span>

   ![A Visual Studio-jóváhagyás](./media/sql-database-elastic-query-getting-started/portal.png)

   <span data-ttu-id="566fb-124">Ezen a ponton létre fogunk hozni egy egyéni adatbázis-gyűjteményt, amely hello shard térkép összes hello adatbázisának tükrözi.</span><span class="sxs-lookup"><span data-stu-id="566fb-124">At this point, we will create a custom database collection that reflects all hello databases in hello shard map.</span></span> <span data-ttu-id="566fb-125">Ezzel lehetővé teszik a számunkra toocreate és végrehajtani egy feladatot, amely az új táblázat hozzáadása szilánkok között.</span><span class="sxs-lookup"><span data-stu-id="566fb-125">This will allow us toocreate and execute a job that add a new table across shards.</span></span>

<span data-ttu-id="566fb-126">Itt azt kellene általában shard térkép cél létrehozásához, hello segítségével **New-AzureSqlJobTarget** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="566fb-126">Here we would usually create a shard map target, using hello **New-AzureSqlJobTarget** cmdlet.</span></span> <span data-ttu-id="566fb-127">hello shard manager adatbázist be kell állítani egy adatbázis célként, és majd hello adott shard térkép cél van megadva.</span><span class="sxs-lookup"><span data-stu-id="566fb-127">hello shard map manager database must be set as a database target and then hello specific shard map is specified as a target.</span></span> <span data-ttu-id="566fb-128">Ehelyett azt folyamatos tooenumerate hello kiszolgáló összes hello adatbázisának és hello adatbázisok toohello új egyéni gyűjtemény hozzáadása fő adatbázis hello kivétel miatt.</span><span class="sxs-lookup"><span data-stu-id="566fb-128">Instead, we are going tooenumerate all hello databases in hello server and add hello databases toohello new custom collection with hello exception of master database.</span></span>

## <a name="creates-a-custom-collection-and-add-all-databases-in-hello-server-toohello-custom-collection-target-with-hello-exception-of-master"></a><span data-ttu-id="566fb-129">Létrehoz egy egyéni gyűjteményt, és adja hozzá az összes adatbázis hello server toohello egyéni gyűjtemény cél főkiszolgáló hello kivétel miatt.</span><span class="sxs-lookup"><span data-stu-id="566fb-129">Creates a custom collection and add all databases in hello server toohello custom collection target with hello exception of master.</span></span>
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
## <a name="create-a-t-sql-script-for-execution-across-databases"></a><span data-ttu-id="566fb-130">Az adatbázisok közötti végrehajtási T-SQL parancsfájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="566fb-130">Create a T-SQL Script for execution across databases</span></span>
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

## <a name="create-hello-job-tooexecute-a-script-across-hello-custom-group-of-databases"></a><span data-ttu-id="566fb-131">Teremtsen hello feladat tooexecute parancsfájl hello egyéni adatbázisok csoportja</span><span class="sxs-lookup"><span data-stu-id="566fb-131">Create hello job tooexecute a script across hello custom group of databases</span></span>

   ```
    $jobName = "create on server dbs"
    $scriptName = "NewTable"
    $customCollectionName = "dbs_in_server"
    $credentialName = "ddove66"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job
   ```

## <a name="execute-hello-job"></a><span data-ttu-id="566fb-132">Hello feladat végrehajtása</span><span class="sxs-lookup"><span data-stu-id="566fb-132">Execute hello job</span></span>
<span data-ttu-id="566fb-133">a következő PowerShell-parancsfájl hello használt tooexecute egy meglévő feladat lehet:</span><span class="sxs-lookup"><span data-stu-id="566fb-133">hello following PowerShell script can be used tooexecute an existing job:</span></span>

<span data-ttu-id="566fb-134">Frissítés hello változó tooreflect hello kívánt feladat neve toohave hajtotta végre a következő:</span><span class="sxs-lookup"><span data-stu-id="566fb-134">Update hello following variable tooreflect hello desired job name toohave executed:</span></span>

   ```
    $jobName = "create on server dbs"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution
   ```

## <a name="retrieve-hello-state-of-a-single-job-execution"></a><span data-ttu-id="566fb-135">Egyetlen feladat-végrehajtási hello állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="566fb-135">Retrieve hello state of a single job execution</span></span>
<span data-ttu-id="566fb-136">Használja az azonos hello **Get-AzureSqlJobExecution** hello parancsmagot **IncludeChildren** paraméter tooview hello állapotának gyermek feladat végrehajtások, nevezetesen hello minden egyes feladatok végrehajtásának meghatározott állapotban hello feladat által megcélzott adatbázis.</span><span class="sxs-lookup"><span data-stu-id="566fb-136">Use hello same **Get-AzureSqlJobExecution** cmdlet with hello **IncludeChildren** parameter tooview hello state of child job executions, namely hello specific state for each job execution against each database targeted by hello job.</span></span>

   ```
    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions
   ```

## <a name="view-hello-state-across-multiple-job-executions"></a><span data-ttu-id="566fb-137">Több feladat végrehajtások közötti hello állapotának megtekintése</span><span class="sxs-lookup"><span data-stu-id="566fb-137">View hello state across multiple job executions</span></span>
<span data-ttu-id="566fb-138">Hello **Get-AzureSqlJobExecution** parancsmag rendelkezik több nem kötelező paraméter, amely használt toodisplay több feladat végrehajtások, szűrt hello megadott paramétereknek.</span><span class="sxs-lookup"><span data-stu-id="566fb-138">hello **Get-AzureSqlJobExecution** cmdlet has multiple optional parameters that can be used toodisplay multiple job executions, filtered through hello provided parameters.</span></span> <span data-ttu-id="566fb-139">hello következő hello lehetséges módjait toouse Get-AzureSqlJobExecution némelyike mutatja be:</span><span class="sxs-lookup"><span data-stu-id="566fb-139">hello following demonstrates some of hello possible ways toouse Get-AzureSqlJobExecution:</span></span>

<span data-ttu-id="566fb-140">Lekéri az összes aktív felső szintű feladat végrehajtások:</span><span class="sxs-lookup"><span data-stu-id="566fb-140">Retrieve all active top level job executions:</span></span>

   ```
    Get-AzureSqlJobExecution
   ```

<span data-ttu-id="566fb-141">Lekéri az összes felső szintű feladat végrehajtások, beleértve az inaktív feladat végrehajtások:</span><span class="sxs-lookup"><span data-stu-id="566fb-141">Retrieve all top level job executions, including inactive job executions:</span></span>

   ```
    Get-AzureSqlJobExecution -IncludeInactive
   ```

<span data-ttu-id="566fb-142">Lekéri az összes gyermek feladat végrehajtások inaktív feladat végrehajtások többek között a megadott feladat végrehajtási Azonosítóhoz:</span><span class="sxs-lookup"><span data-stu-id="566fb-142">Retrieve all child job executions of a provided job execution ID, including inactive job executions:</span></span>

   ```
    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId -IncludeInactive -IncludeChildren
   ```

<span data-ttu-id="566fb-143">Ütemezés használatával létrehozott összes feladat végrehajtások beolvasása / feladat-kombináció, beleértve az inaktív feladatok:</span><span class="sxs-lookup"><span data-stu-id="566fb-143">Retrieve all job executions created using a schedule / job combination, including inactive jobs:</span></span>

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive
   ```

<span data-ttu-id="566fb-144">Lekéri az összes feladat megadott shard térképre, beleértve az inaktív feladatokat célcsoportkezelést:</span><span class="sxs-lookup"><span data-stu-id="566fb-144">Retrieve all jobs targeting a specified shard map, including inactive jobs:</span></span>

   ```
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive
   ```

<span data-ttu-id="566fb-145">Lekéri az összes feladat a megadott egyéni gyűjtemény, beleértve az inaktív feladatokat célcsoportkezelést:</span><span class="sxs-lookup"><span data-stu-id="566fb-145">Retrieve all jobs targeting a specified custom collection, including inactive jobs:</span></span>

   ```
    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive
   ```

<span data-ttu-id="566fb-146">Feladat feladat végrehajtások belül egy adott feladat végrehajtási hello listájának beolvasása:</span><span class="sxs-lookup"><span data-stu-id="566fb-146">Retrieve hello list of job task executions within a specific job execution:</span></span>

   ```
    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions
   ```

<span data-ttu-id="566fb-147">Feladat végrehajtási részlete beolvasása:</span><span class="sxs-lookup"><span data-stu-id="566fb-147">Retrieve job task execution details:</span></span>

<span data-ttu-id="566fb-148">a következő PowerShell-parancsfájl hello lehet egy feladat a feladat a végrehajtás, amely akkor különösen hasznos, ha végrehajtási hibakeresésére használt tooview hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="566fb-148">hello following PowerShell script can be used tooview hello details of a job task execution, which is particularly useful when debugging execution failures.</span></span>
   ```
    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution
   ```

## <a name="retrieve-failures-within-job-task-executions"></a><span data-ttu-id="566fb-149">Sikertelen feladat feladat végrehajtások belül beolvasása</span><span class="sxs-lookup"><span data-stu-id="566fb-149">Retrieve failures within job task executions</span></span>
<span data-ttu-id="566fb-150">hello JobTaskExecution objektum tartalmaz egy hello együtt egy üzenettulajdonság hello feladat életciklusa tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="566fb-150">hello JobTaskExecution object includes a property for hello Lifecycle of hello task along with a Message property.</span></span> <span data-ttu-id="566fb-151">Ha egy feladat a feladat végrehajtása sikertelen volt, hello életciklus tulajdonság túl állítható*sikertelen* és hello üzenettulajdonság lesz toohello eredményül kapott hibaüzenetet, és a verem.</span><span class="sxs-lookup"><span data-stu-id="566fb-151">If a job task execution failed, hello Lifecycle property will be set too*Failed* and hello Message property will be set toohello resulting exception message and its stack.</span></span> <span data-ttu-id="566fb-152">Ha egy feladat sikertelen volt, akkor fontos tooview hello részleteit munka feladatai, amelyek egy adott feladat sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="566fb-152">If a job did not succeed, it is important tooview hello details of job tasks that did not succeed for a given job.</span></span>

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

## <a name="waiting-for-a-job-execution-toocomplete"></a><span data-ttu-id="566fb-153">Várakozás a feladat végrehajtási toocomplete</span><span class="sxs-lookup"><span data-stu-id="566fb-153">Waiting for a job execution toocomplete</span></span>
<span data-ttu-id="566fb-154">a következő PowerShell-parancsfájl hello lehet egy feladat feladat toocomplete a használt toowait:</span><span class="sxs-lookup"><span data-stu-id="566fb-154">hello following PowerShell script can be used toowait for a job task toocomplete:</span></span>

   ```
    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId
   ```

## <a name="create-a-custom-execution-policy"></a><span data-ttu-id="566fb-155">Egyéni végrehajtási házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="566fb-155">Create a custom execution policy</span></span>
<span data-ttu-id="566fb-156">Rugalmas adatbázis-feladatok feladat indításakor alkalmazható egyéni végrehajtási házirendek létrehozását támogatja.</span><span class="sxs-lookup"><span data-stu-id="566fb-156">Elastic Database jobs supports creating custom execution policies that can be applied when starting jobs.</span></span>

<span data-ttu-id="566fb-157">Végrehajtási házirendek definiálása jelenleg engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="566fb-157">Execution policies currently allow for defining:</span></span>

* <span data-ttu-id="566fb-158">Name: Azonosítója hello végrehajtási házirendet.</span><span class="sxs-lookup"><span data-stu-id="566fb-158">Name: Identifier for hello execution policy.</span></span>
* <span data-ttu-id="566fb-159">Feladat időtúllépése: Teljes idő előtt rugalmas adatbázis-feladatok megszakítja a feladatot.</span><span class="sxs-lookup"><span data-stu-id="566fb-159">Job Timeout: Total time before a job will be canceled by Elastic Database Jobs.</span></span>
* <span data-ttu-id="566fb-160">Kezdeti újrapróbálkozási időköz: Intervallum toowait előtt próbálkozik újra.</span><span class="sxs-lookup"><span data-stu-id="566fb-160">Initial Retry Interval: Interval toowait before first retry.</span></span>
* <span data-ttu-id="566fb-161">Maximális újrapróbálkozási időköz: Az újrapróbálkozási intervallumok toouse kap.</span><span class="sxs-lookup"><span data-stu-id="566fb-161">Maximum Retry Interval: Cap of retry intervals toouse.</span></span>
* <span data-ttu-id="566fb-162">Újrapróbálkozási időköz leállítási együttható: Együttható használt toocalculate hello következő intervallum újrapróbálkozások között.</span><span class="sxs-lookup"><span data-stu-id="566fb-162">Retry Interval Backoff Coefficient: Coefficient used toocalculate hello next interval between retries.</span></span>  <span data-ttu-id="566fb-163">hello alábbi képlet használható: (kezdeti újrapróbálkozási időközét) * Math.pow ((időköz leállítási együttható), (próbálkozások száma) - 2).</span><span class="sxs-lookup"><span data-stu-id="566fb-163">hello following formula is used: (Initial Retry Interval) * Math.pow((Interval Backoff Coefficient), (Number of Retries) - 2).</span></span>
* <span data-ttu-id="566fb-164">Kísérletek maximális száma: hello maximális száma újrapróbálkozási kísérletek tooperform feladat.</span><span class="sxs-lookup"><span data-stu-id="566fb-164">Maximum Attempts: hello maximum number of retry attempts tooperform within a job.</span></span>

<span data-ttu-id="566fb-165">hello alapértelmezett végrehajtási házirend hello a következő értékeket használja:</span><span class="sxs-lookup"><span data-stu-id="566fb-165">hello default execution policy uses hello following values:</span></span>

* <span data-ttu-id="566fb-166">Name: Alapértelmezett végrehajtási házirend</span><span class="sxs-lookup"><span data-stu-id="566fb-166">Name: Default execution policy</span></span>
* <span data-ttu-id="566fb-167">Feladat időtúllépése: 1 hét</span><span class="sxs-lookup"><span data-stu-id="566fb-167">Job Timeout: 1 week</span></span>
* <span data-ttu-id="566fb-168">Kezdeti újrapróbálkozási időköz: 100 milliomod másodperc</span><span class="sxs-lookup"><span data-stu-id="566fb-168">Initial Retry Interval:  100 milliseconds</span></span>
* <span data-ttu-id="566fb-169">Maximális újrapróbálkozási időköz: 30 perc</span><span class="sxs-lookup"><span data-stu-id="566fb-169">Maximum Retry Interval: 30 minutes</span></span>
* <span data-ttu-id="566fb-170">Ismételje meg a időköz együttható: 2. régiója</span><span class="sxs-lookup"><span data-stu-id="566fb-170">Retry Interval Coefficient: 2</span></span>
* <span data-ttu-id="566fb-171">Kísérletek maximális száma: 2 147 483 647</span><span class="sxs-lookup"><span data-stu-id="566fb-171">Maximum Attempts: 2,147,483,647</span></span>

<span data-ttu-id="566fb-172">Szükségeskonfiguráció-hello végrehajtási szabályzat létrehozása:</span><span class="sxs-lookup"><span data-stu-id="566fb-172">Create hello desired execution policy:</span></span>

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

### <a name="update-a-custom-execution-policy"></a><span data-ttu-id="566fb-173">Egy egyéni végrehajtási házirend frissítése</span><span class="sxs-lookup"><span data-stu-id="566fb-173">Update a custom execution policy</span></span>
<span data-ttu-id="566fb-174">Frissítés hello végrehajtási házirend tooupdate szükséges:</span><span class="sxs-lookup"><span data-stu-id="566fb-174">Update hello desired execution policy tooupdate:</span></span>

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

## <a name="cancel-a-job"></a><span data-ttu-id="566fb-175">Feladatok megszakítása</span><span class="sxs-lookup"><span data-stu-id="566fb-175">Cancel a job</span></span>
<span data-ttu-id="566fb-176">Rugalmas adatbázis-feladatok feladatok megszakítását kérelmeket támogatja.</span><span class="sxs-lookup"><span data-stu-id="566fb-176">Elastic Database Jobs supports jobs cancellation requests.</span></span>  <span data-ttu-id="566fb-177">Rugalmas adatbázis-feladatok jelenleg végrehajtás alatt álló feladat a lemondási kérelmet észleli, ha a program megpróbálja toostop hello feladat.</span><span class="sxs-lookup"><span data-stu-id="566fb-177">If Elastic Database Jobs detects a cancellation request for a job currently being executed, it will attempt toostop hello job.</span></span>

<span data-ttu-id="566fb-178">Rugalmas adatbázis-feladatok is hajtsa végre a megszakítási két különböző módja van:</span><span class="sxs-lookup"><span data-stu-id="566fb-178">There are two different ways that Elastic Database Jobs can perform a cancellation:</span></span>

1. <span data-ttu-id="566fb-179">Megszakítása jelenleg feldolgozás alatt álló feladatok: törlése közben jelenleg fut egy feladat észleli, ha a megszakítási belül jelenleg végrehajtás alatt álló hello feladat aspektusa hello történt kísérlet.</span><span class="sxs-lookup"><span data-stu-id="566fb-179">Canceling Currently Executing Tasks: If a cancellation is detected while a task is currently running, a cancellation will be attempted within hello currently executing aspect of hello task.</span></span>  <span data-ttu-id="566fb-180">Példa: Ha egy hosszú ideig tartó jelenleg végrehajtás alatt álló lekérdezés törlése megkísérelt van, nem lesznek egy kísérlet toocancel hello lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="566fb-180">For example: If there is a long running query currently being performed when a cancellation is attempted, there will be an attempt toocancel hello query.</span></span>
2. <span data-ttu-id="566fb-181">Zároló feladat újrapróbálkozások: Törlése észlelésekor hello vezérlő szál feladat a végrehajtás elindítása előtt hello vezérlő szál fog elkerüléséhez hello feladat elindítása és hello kérelem deklarálható megszakítottként.</span><span class="sxs-lookup"><span data-stu-id="566fb-181">Canceling Task Retries: If a cancellation is detected by hello control thread before a task is launched for execution, hello control thread will avoid launching hello task and declare hello request as canceled.</span></span>

<span data-ttu-id="566fb-182">Ha a feladat megszakításának szülő-feladat van szükség, hello lemondási kérelmet szerződéses kötelezettségeket a hello szülő feladat és összes a gyermek-feladatokkal.</span><span class="sxs-lookup"><span data-stu-id="566fb-182">If a job cancellation is requested for a parent job, hello cancellation request will be honored for hello parent job and for all of its child jobs.</span></span>

<span data-ttu-id="566fb-183">a megszakítási kérelmet, toosubmit hello használata **Stop-AzureSqlJobExecution** parancsmag és a set hello **JobExecutionId** paraméter.</span><span class="sxs-lookup"><span data-stu-id="566fb-183">toosubmit a cancellation request, use hello **Stop-AzureSqlJobExecution** cmdlet and set hello **JobExecutionId** parameter.</span></span>

   ```
    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId
   ```

## <a name="delete-a-job-by-name-and-hello-jobs-history"></a><span data-ttu-id="566fb-184">A feladat nevét és hello feladatok előzményeinek törlése</span><span class="sxs-lookup"><span data-stu-id="566fb-184">Delete a job by name and hello job's history</span></span>
<span data-ttu-id="566fb-185">Rugalmas adatbázis-feladatok támogatja az aszinkron feladatok törlése.</span><span class="sxs-lookup"><span data-stu-id="566fb-185">Elastic Database jobs supports asynchronous deletion of jobs.</span></span> <span data-ttu-id="566fb-186">Egy feladat is törlésre, és hello rendszer törölni fogja a hello feladat és a feladatelőzményekben összes feladat végrehajtások hello feladat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="566fb-186">A job can be marked for deletion and hello system will delete hello job and all its job history after all job executions have completed for hello job.</span></span> <span data-ttu-id="566fb-187">hello rendszer nem fogja automatikusan megszakítja a aktív feladat végrehajtások.</span><span class="sxs-lookup"><span data-stu-id="566fb-187">hello system will not automatically cancel active job executions.</span></span>  

<span data-ttu-id="566fb-188">Ehelyett a Stop-AzureSqlJobExecution meghívott toocancel aktív feladat végrehajtások kell lennie.</span><span class="sxs-lookup"><span data-stu-id="566fb-188">Instead, Stop-AzureSqlJobExecution must be invoked toocancel active job executions.</span></span>

<span data-ttu-id="566fb-189">tootrigger feladat törlése, használjon hello **Remove-AzureSqlJob** parancsmag és a set hello **JobName** paraméter.</span><span class="sxs-lookup"><span data-stu-id="566fb-189">tootrigger job deletion, use hello **Remove-AzureSqlJob** cmdlet and set hello **JobName** parameter.</span></span>

   ```
    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
   ```

## <a name="create-a-custom-database-target"></a><span data-ttu-id="566fb-190">Hozzon létre egy egyéni adatbázis-cél</span><span class="sxs-lookup"><span data-stu-id="566fb-190">Create a custom database target</span></span>
<span data-ttu-id="566fb-191">Egyéni adatbázis célok meghatározása a rugalmas adatbázis-feladatokhoz, amelyek a végrehajtás közvetlenül vagy egy egyéni adatbázis csoportban foglalható is használható.</span><span class="sxs-lookup"><span data-stu-id="566fb-191">Custom database targets can be defined in Elastic Database jobs which can be used either for execution directly or for inclusion within a custom database group.</span></span> <span data-ttu-id="566fb-192">Mivel a **rugalmas készletek** nem még közvetlenül támogatottak hello PowerShell API-k, keresztül egyszerűen létrehozhat egy egyéni adatbázis célként és egy egyéni adatbázis gyűjtemény célként, ami magában foglalja az összes hello adatbázis hello készletben.</span><span class="sxs-lookup"><span data-stu-id="566fb-192">Since **elastic pools** are not yet directly supported via hello PowerShell APIs, you simply create a custom database target and custom database collection target which encompasses all hello databases in hello pool.</span></span>

<span data-ttu-id="566fb-193">Állítsa be a következő változók tooreflect hello szükséges adatbázis-információ hello:</span><span class="sxs-lookup"><span data-stu-id="566fb-193">Set hello following variables tooreflect hello desired database information:</span></span>

   ```
    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName
   ```

## <a name="create-a-custom-database-collection-target"></a><span data-ttu-id="566fb-194">Hozzon létre egy egyéni adatbázis-gyűjtemény célja</span><span class="sxs-lookup"><span data-stu-id="566fb-194">Create a custom database collection target</span></span>
<span data-ttu-id="566fb-195">Egyéni adatbázis gyűjtemény célja lehet meghatározott tooenable végrehajtása több meghatározott adatbázis cél között.</span><span class="sxs-lookup"><span data-stu-id="566fb-195">A custom database collection target can be defined tooenable execution across multiple defined database targets.</span></span> <span data-ttu-id="566fb-196">Egy adatbázis-csoport létrehozása után adatbázisok társított toohello egyéni gyűjtemény célja lehet.</span><span class="sxs-lookup"><span data-stu-id="566fb-196">After a database group is created, databases can be associated toohello custom collection target.</span></span>

<span data-ttu-id="566fb-197">Állítsa be a következő változók tooreflect hello kívánt egyéni gyűjtemény konfigurációjához hello:</span><span class="sxs-lookup"><span data-stu-id="566fb-197">Set hello following variables tooreflect hello desired custom collection target configuration:</span></span>

   ```
    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName
   ```

### <a name="add-databases-tooa-custom-database-collection-target"></a><span data-ttu-id="566fb-198">Adatbázisok tooa egyéni adatbázis gyűjtemény cél hozzáadása</span><span class="sxs-lookup"><span data-stu-id="566fb-198">Add databases tooa custom database collection target</span></span>
<span data-ttu-id="566fb-199">Adatbázis célok társítható egyéni adatbázis gyűjtemény célok toocreate adatbázisok csoportja.</span><span class="sxs-lookup"><span data-stu-id="566fb-199">Database targets can be associated with custom database collection targets toocreate a group of databases.</span></span> <span data-ttu-id="566fb-200">Amikor egy feladat jön létre, amely egyéni adatbázis gyűjtemény céloz, bővített tootarget hello adatbázisok társított toohello csoport végrehajtás hello időpontban lesz.</span><span class="sxs-lookup"><span data-stu-id="566fb-200">Whenever a job is created that targets a custom database collection target, it will be expanded tootarget hello databases associated toohello group at hello time of execution.</span></span>

<span data-ttu-id="566fb-201">Szükségeskonfiguráció-hello adatbázis tooa meghatározott egyéni gyűjtemény hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="566fb-201">Add hello desired database tooa specific custom collection:</span></span>

   ```
    $serverName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName
   ```

#### <a name="review-hello-databases-within-a-custom-database-collection-target"></a><span data-ttu-id="566fb-202">Tekintse át a hello adatbázisok belül egy egyéni adatbázis-gyűjtemény célja</span><span class="sxs-lookup"><span data-stu-id="566fb-202">Review hello databases within a custom database collection target</span></span>
<span data-ttu-id="566fb-203">Használjon hello **Get-AzureSqlJobTarget** parancsmag tooretrieve hello gyermek adatbázis egy egyéni adatbázis-gyűjtemény célja.</span><span class="sxs-lookup"><span data-stu-id="566fb-203">Use hello **Get-AzureSqlJobTarget** cmdlet tooretrieve hello child databases within a custom database collection target.</span></span>

   ```
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets
   ```

### <a name="create-a-job-tooexecute-a-script-across-a-custom-database-collection-target"></a><span data-ttu-id="566fb-204">Hozzon létre egy feladat tooexecute egy parancsprogramot egy egyéni adatbázis-gyűjtemény célja között</span><span class="sxs-lookup"><span data-stu-id="566fb-204">Create a job tooexecute a script across a custom database collection target</span></span>
<span data-ttu-id="566fb-205">Használjon hello **New-AzureSqlJob** parancsmag toocreate egy feladatot, egy egyéni adatbázis gyűjtemény tároló által definiált adatbázisok csoportja ellen.</span><span class="sxs-lookup"><span data-stu-id="566fb-205">Use hello **New-AzureSqlJob** cmdlet toocreate a job against a group of databases defined by a custom database collection target.</span></span> <span data-ttu-id="566fb-206">Rugalmas adatbázis-feladatok hello feladat kibővítése minden megfelelő tooa adatbázis társított hello egyéni adatbázis-gyűjtemény célja, és győződjön meg arról, hogy minden egyes adatbázison hello parancsfájl végrehajtása több gyermek feladat.</span><span class="sxs-lookup"><span data-stu-id="566fb-206">Elastic Database jobs will expand hello job into multiple child jobs each corresponding tooa database associated with hello custom database collection target and ensure that hello script is executed against each database.</span></span> <span data-ttu-id="566fb-207">Ebben az esetben fontos, hogy a parancsfájlok az idempotent toobe rugalmas tooretries.</span><span class="sxs-lookup"><span data-stu-id="566fb-207">Again, it is important that scripts are idempotent toobe resilient tooretries.</span></span>

   ```
    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job
   ```

## <a name="data-collection-across-databases"></a><span data-ttu-id="566fb-208">Az adatbázisok közötti adatok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="566fb-208">Data collection across databases</span></span>
<span data-ttu-id="566fb-209">**Rugalmas adatbázis-feladatok** támogatja, a lekérdezés végrehajtása adatbázisok csoportja között, és elküldi a hello eredmények tooa megadott adatbázis tábla.</span><span class="sxs-lookup"><span data-stu-id="566fb-209">**Elastic Database jobs** supports executing a query across a group of databases and sends hello results tooa specified database’s table.</span></span> <span data-ttu-id="566fb-210">hello tábla hello tény toosee hello lekérdezés eredményében minden adatbázisból után kérdezhetők le.</span><span class="sxs-lookup"><span data-stu-id="566fb-210">hello table can be queried after hello fact toosee hello query’s results from each database.</span></span> <span data-ttu-id="566fb-211">Így lehetővé teszi egy aszinkron mechanizmus tooexecute a lekérdezés több adatbázis közötti.</span><span class="sxs-lookup"><span data-stu-id="566fb-211">This provides an asynchronous mechanism tooexecute a query across many databases.</span></span> <span data-ttu-id="566fb-212">Hiba az esetekben egy hello adatbázisok alatt átmenetileg nem érhető el, automatikusan kezeli, újrapróbálkozások keresztül.</span><span class="sxs-lookup"><span data-stu-id="566fb-212">Failure cases such as one of hello databases being temporarily unavailable are handled automatically via retries.</span></span>

<span data-ttu-id="566fb-213">hello megadott céltábla automatikusan létrejön, ha még nem létezik, a megfelelő hello sémája hello eredményhalmazt adott.</span><span class="sxs-lookup"><span data-stu-id="566fb-213">hello specified destination table will be automatically created if it does not yet exist, matching hello schema of hello returned result set.</span></span> <span data-ttu-id="566fb-214">A parancsfájl végrehajtása több eredménykészlet adja vissza, ha a rugalmas adatbázis-feladatok csak küldi hello első megadott egy toohello célként megadott táblája.</span><span class="sxs-lookup"><span data-stu-id="566fb-214">If a script execution returns multiple result sets, Elastic Database jobs will only send hello first one toohello provided destination table.</span></span>

<span data-ttu-id="566fb-215">a következő PowerShell-parancsfájl hello használt tooexecute gyűjtése az eredményeket egy megadott táblába parancsfájlt is lehet.</span><span class="sxs-lookup"><span data-stu-id="566fb-215">hello following PowerShell script can be used tooexecute a script collecting its results into a specified table.</span></span> <span data-ttu-id="566fb-216">Ez a parancsfájl feltételezi, hogy egy T-SQL parancsfájl létrehozva a következő amelyek kimenete egy eredményhalmaz és létrehoztak egy egyéni adatbázis gyűjtemény célhelyet.</span><span class="sxs-lookup"><span data-stu-id="566fb-216">This script assumes that a T-SQL script has been created which outputs a single result set and a custom database collection target has been created.</span></span>

<span data-ttu-id="566fb-217">Állítsa be a következő tooreflect szükséges hello parancsfájl, a hitelesítő adatokat és a végrehajtási célját hello:</span><span class="sxs-lookup"><span data-stu-id="566fb-217">Set hello following tooreflect hello desired script, credentials, and execution target:</span></span>

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

### <a name="create-and-start-a-job-for-data-collection-scenarios"></a><span data-ttu-id="566fb-218">Hozzon létre, és elindíthat egy feladatot a adatáttelepítések gyűjtemény esetében</span><span class="sxs-lookup"><span data-stu-id="566fb-218">Create and start a job for data collection scenarios</span></span>
   ```
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $executionCredentialName -ContentName $scriptName -ResultSetDestinationServerName $destinationServerName -ResultSetDestinationDatabaseName $destinationDatabaseName -ResultSetDestinationSchemaName $destinationSchemaName -ResultSetDestinationTableName $destinationTableName -ResultSetDestinationCredentialName $destinationCredentialName -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution
   ```

## <a name="create-a-schedule-for-job-execution-using-a-job-trigger"></a><span data-ttu-id="566fb-219">A feladat végrehajtását egy feladat eseményindító ütemezés létrehozása</span><span class="sxs-lookup"><span data-stu-id="566fb-219">Create a schedule for job execution using a job trigger</span></span>
<span data-ttu-id="566fb-220">a következő PowerShell-parancsfájl hello lehet használt toocreate feladatról ütemezés szerint.</span><span class="sxs-lookup"><span data-stu-id="566fb-220">hello following PowerShell script can be used toocreate a reoccurring schedule.</span></span> <span data-ttu-id="566fb-221">A parancsfájl egy percen, de a New-AzureSqlJobSchedule is támogat - DayInterval, - HourInterval, - MonthInterval, és - WeekInterval paraméterek.</span><span class="sxs-lookup"><span data-stu-id="566fb-221">This script uses a one minute interval, but New-AzureSqlJobSchedule also supports -DayInterval, -HourInterval, -MonthInterval, and -WeekInterval parameters.</span></span> <span data-ttu-id="566fb-222">Csak egyszer hajtható végre ütemezéseket is létrehozható, hogy - alkalommal.</span><span class="sxs-lookup"><span data-stu-id="566fb-222">Schedules that execute only once can be created by passing -OneTime.</span></span>

<span data-ttu-id="566fb-223">Új ütemezés létrehozása:</span><span class="sxs-lookup"><span data-stu-id="566fb-223">Create a new schedule:</span></span>
   ```
    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule -MinuteInterval $minuteInterval -ScheduleName $scheduleName -StartTime $startTime
    Write-Output $schedule
   ```

### <a name="create-a-job-trigger-toohave-a-job-executed-on-a-time-schedule"></a><span data-ttu-id="566fb-224">Egy feladat eseményindító toohave ütemezéssel végrehajtott feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="566fb-224">Create a job trigger toohave a job executed on a time schedule</span></span>
<span data-ttu-id="566fb-225">Egy feladat eseményindító lehet meghatározott toohave egy feladat végrehajtása függően tooa ütemezéssel.</span><span class="sxs-lookup"><span data-stu-id="566fb-225">A job trigger can be defined toohave a job executed according tooa time schedule.</span></span> <span data-ttu-id="566fb-226">a következő PowerShell-parancsfájl hello lehet használt toocreate feladat eseményindítót.</span><span class="sxs-lookup"><span data-stu-id="566fb-226">hello following PowerShell script can be used toocreate a job trigger.</span></span>

<span data-ttu-id="566fb-227">Állítsa be a következő változók toocorrespond toohello kívánt feladat hello és ütemezése:</span><span class="sxs-lookup"><span data-stu-id="566fb-227">Set hello following variables toocorrespond toohello desired job and schedule:</span></span>

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName
    Write-Output $jobTrigger
   ```

### <a name="remove-a-scheduled-association-toostop-job-from-executing-on-schedule"></a><span data-ttu-id="566fb-228">Ütemezés szerint végrehajtása ütemezett társítás toostop feladat eltávolítása</span><span class="sxs-lookup"><span data-stu-id="566fb-228">Remove a scheduled association toostop job from executing on schedule</span></span>
<span data-ttu-id="566fb-229">Ismétlődés feladat végrehajtása a feladat eseményindítót, hello feladat eseményindító keresztül toodiscontinue távolítható el.</span><span class="sxs-lookup"><span data-stu-id="566fb-229">toodiscontinue reoccurring job execution through a job trigger, hello job trigger can be removed.</span></span>
<span data-ttu-id="566fb-230">Távolítsa el a egy feladat eseményindító toostop egy feladat a végrehajtás alatt álló hello függően tooa ütemezés **Remove-AzureSqlJobTrigger** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="566fb-230">Remove a job trigger toostop a job from being executed according tooa schedule using hello **Remove-AzureSqlJobTrigger** cmdlet.</span></span>

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName
   ```

## <a name="import-elastic-database-query-results-tooexcel"></a><span data-ttu-id="566fb-231">A rugalmas adatbázis-lekérdezés eredménye tooExcel importálása</span><span class="sxs-lookup"><span data-stu-id="566fb-231">Import elastic database query results tooExcel</span></span>
 <span data-ttu-id="566fb-232">A lekérdezés tooan Excel fájl eredményei hello importálhatja.</span><span class="sxs-lookup"><span data-stu-id="566fb-232">You can import hello results from of a query tooan Excel file.</span></span>

1. <span data-ttu-id="566fb-233">Indítsa el az Excel 2013.</span><span class="sxs-lookup"><span data-stu-id="566fb-233">Launch Excel 2013.</span></span>
2. <span data-ttu-id="566fb-234">Keresse meg a toohello **adatok** menüszalagján.</span><span class="sxs-lookup"><span data-stu-id="566fb-234">Navigate toohello **Data** ribbon.</span></span>
3. <span data-ttu-id="566fb-235">Kattintson a **egyéb forrásokból származó** kattintson **az SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="566fb-235">Click **From Other Sources** and click **From SQL Server**.</span></span>

   ![Excel importálása más forrásokból](./media/sql-database-elastic-query-getting-started/exel-sources.png)

4. <span data-ttu-id="566fb-237">A hello **Adatkapcsolat varázsló** írja be a hello kiszolgálónév és hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="566fb-237">In hello **Data Connection Wizard** type hello server name and login credentials.</span></span> <span data-ttu-id="566fb-238">Ezután kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="566fb-238">Then click **Next**.</span></span>
5. <span data-ttu-id="566fb-239">Hello párbeszédpanelen **hello adatokat tartalmazó adatbázis válassza hello**, jelölje be hello **ElasticDBQuery** adatbázis.</span><span class="sxs-lookup"><span data-stu-id="566fb-239">In hello dialog box **Select hello database that contains hello data you want**, select hello **ElasticDBQuery** database.</span></span>
6. <span data-ttu-id="566fb-240">Jelölje be hello **ügyfelek** hello listanézetben táblázatban, majd kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="566fb-240">Select hello **Customers** table in hello list view and click **Next**.</span></span> <span data-ttu-id="566fb-241">Kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="566fb-241">Then click **Finish**.</span></span>
7. <span data-ttu-id="566fb-242">A hello **és adatokat importálhat** űrlap **válassza ki, hogy tooview ezeket az adatokat a munkafüzet**, jelölje be **tábla** kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="566fb-242">In hello **Import Data** form, under **Select how you want tooview this data in your workbook**, select **Table** and click **OK**.</span></span>

<span data-ttu-id="566fb-243">Az összes sorát hello **ügyfelek** a különböző szilánkok tárolt tábla feltöltéséhez hello Excel-táblában.</span><span class="sxs-lookup"><span data-stu-id="566fb-243">All hello rows from **Customers** table, stored in different shards populate hello Excel sheet.</span></span>

## <a name="next-steps"></a><span data-ttu-id="566fb-244">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="566fb-244">Next steps</span></span>
<span data-ttu-id="566fb-245">Most már használhatja az Excel adatok funkciók.</span><span class="sxs-lookup"><span data-stu-id="566fb-245">You can now use Excel’s data functions.</span></span> <span data-ttu-id="566fb-246">Hello kapcsolati karakterláncot használ a kiszolgáló nevét, az adatbázisnév és a hitelesítő adatok tooconnect a BI és az integráció eszközök toohello rugalmas adatbázis lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="566fb-246">Use hello connection string with your server name, database name and credentials tooconnect your BI and data integration tools toohello elastic query database.</span></span> <span data-ttu-id="566fb-247">Győződjön meg arról, hogy az SQL Server támogatja-e az eszköz adatforrásként.</span><span class="sxs-lookup"><span data-stu-id="566fb-247">Make sure that SQL Server is supported as a data source for your tool.</span></span> <span data-ttu-id="566fb-248">Tekintse meg a toohello rugalmas lekérdezési adatbázis és a külső táblák csakúgy, mint bármely más SQL Server-adatbázis és, hogy Ön kapcsolódnának toowith az eszköz az SQL Server-táblákra.</span><span class="sxs-lookup"><span data-stu-id="566fb-248">Refer toohello elastic query database and external tables just like any other SQL Server database and SQL Server tables that you would connect toowith your tool.</span></span>

### <a name="cost"></a><span data-ttu-id="566fb-249">Költségek</span><span class="sxs-lookup"><span data-stu-id="566fb-249">Cost</span></span>
<span data-ttu-id="566fb-250">Nem kell külön fizetni a hello rugalmas adatbázis-lekérdezés szolgáltatása van.</span><span class="sxs-lookup"><span data-stu-id="566fb-250">There is no additional charge for using hello Elastic Database query feature.</span></span> <span data-ttu-id="566fb-251">Jelenleg ez a szolgáltatás rendelkezésre áll csak a premium adatbázisokat a végpont, azonban bármely szolgáltatásréteg hello szilánkok lehet.</span><span class="sxs-lookup"><span data-stu-id="566fb-251">However, at this time this feature is available only on premium databases as an end point, but hello shards can be of any service tier.</span></span>

<span data-ttu-id="566fb-252">Díjszabási információkért lásd: [SQL adatbázis díjszabás](https://azure.microsoft.com/pricing/details/sql-database/).</span><span class="sxs-lookup"><span data-stu-id="566fb-252">For pricing information see [SQL Database Pricing Details](https://azure.microsoft.com/pricing/details/sql-database/).</span></span>

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
