---
title: "aaaCreate és a PowerShell segítségével a rugalmas feladatok kezelése |} Microsoft Docs"
description: "PowerShell használt toomanage Azure SQL Database-készletek"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 737d8d13-5632-4e18-9cb0-4d3b8a19e495
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: f6c18aecfa7e8c0b102a3b7cd2f266f5542ae400
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-sql-database-elastic-jobs-using-powershell-preview"></a>SQL Database PowerShell (előzetes verzió) segítségével a rugalmas feladatok létrehozásához és kezeléséhez

hello PowerShell API-k a **rugalmas adatbázis-feladatok** (az előzetes verzió), lehetővé teszik, hogy olyan adatbázisok, amely végrehajtja a parancsfájlok csoportját. Ez a cikk bemutatja, hogyan toocreate és kezelése **rugalmas adatbázis-feladatok** PowerShell-parancsmagok használatával. Lásd: [rugalmas feladatok áttekintése](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Előfeltételek
* Azure-előfizetés. Ingyenes próbaverzió, lásd: [ingyenes egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).
* Hello rugalmas adatbázis eszközzel létrehozott adatbázisok készleteit. Lásd: [Ismerkedés a rugalmas adatbáziseszközöket](sql-database-elastic-scale-get-started.md).
* Azure PowerShell. Részletes információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](https://docs.microsoft.com/powershell/azure/overview).
* **Rugalmas adatbázis-feladatok** PowerShell csomag: lásd: [telepítése rugalmas adatbázis-feladatok](sql-database-elastic-jobs-service-installation.md)

### <a name="select-your-azure-subscription"></a>Válassza ki az Azure-előfizetéshez
az előfizetés-azonosítót kell tooselect hello előfizetés (**- SubscriptionId**) vagy előfizetés neve (**- SubscriptionName**). Ha több előfizetéssel rendelkezik futtatása hello **Get-AzureRmSubscription** parancsmag és a példány hello szükséges hello eredményhalmazából előfizetési adatokat. Miután az előfizetési adatai, futtassa a következő parancsmag tooset hello ehhez az előfizetéshez hello alapértelmezett, nevezetesen a cél a feladatok létrehozását és kezelését hello:

    Select-AzureRmSubscription -SubscriptionId {SubscriptionID}

Hello [PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx) használati toodevelop ajánlott, és a PowerShell-parancsfájlok elleni hello rugalmas adatbázis-feladatok végrehajtása.

## <a name="elastic-database-jobs-objects"></a>A rugalmas adatbázis-feladatok objektumok
a következő táblázat fel minden hello objektum típusú hello **rugalmas adatbázis-feladatok** együtt a leírása és a megfelelő PowerShell API-k.

<table style="width:100%">
  <tr>
    <th>Objektumtípus</th>
    <th>Leírás</th>
    <th>Kapcsolódó PowerShell API-k</th>
  </tr>
  <tr>
    <td>Hitelesítő adat</td>
    <td>Felhasználónév és jelszó toouse parancsprogramok végrehajtását vagy DACPACs alkalmazásának toodatabases kapcsolódáskor. <p>hello jelszó titkosított hello rugalmas adatbázis-feladatok adatbázis tárolási tooand elküldése előtt.  hello jelszó visszafejti hello rugalmas adatbázis-feladatok szolgáltatás hello hitelesítő adat létrehozása és fel kell tölteni a hello telepítési parancsfájl használatával.</td>
    <td><p>Get-AzureSqlJobCredential</p>
    <p>Új AzureSqlJobCredential</p><p>Set-AzureSqlJobCredential</p></td></td>
  </tr>

  <tr>
    <td>Szkript</td>
    <td>Transact-SQL parancsfájl toobe az adatbázisok közötti végrehajtási használatos.  hello parancsfájl kell lennie a szerzői toobe idempotent, mivel hello szolgáltatás újra próbálkozik a hibák után hello parancsprogram végrehajtása.
    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Get-AzureSqlJobContentDefinition</p>
    <p>Új AzureSqlJobContent</p>
    <p>Set-AzureSqlJobContentDefinition</p>
    </td>
  </tr>

  <tr>
    <td>DACPAC</td>
    <td><a href="https://msdn.microsoft.com/library/ee210546.aspx">Adatrétegbeli alkalmazás </a> alkalmazza az adatbázisok közötti toobe csomag.

    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Új AzureSqlJobContent</p>
    <p>Set-AzureSqlJobContentDefinition</p>
    </td>
  </tr>
  <tr>
    <td>Adatbázis-cél</td>
    <td>Adatbázis és a kiszolgáló neve mutató tooan Azure SQL Database.

    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Új AzureSqlJobTarget</p>
    </td>
  </tr>
  <tr>
    <td>A shard térkép cél</td>
    <td>Egy adatbázis cél és a hitelesítő adatok toobe használt toodetermine adatok egy rugalmas adatbázist shard leképezés tárolja.
    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Új AzureSqlJobTarget</p>
    <p>Set-AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Egyéni gyűjtemény célja</td>
    <td>Adatbázisok toocollectively meghatározott csoportjára használja a végrehajtáshoz.</td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Új AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Egyéni gyűjtemény gyermek cél</td>
    <td>Egy egyéni gyűjteményből hivatkozott adatbázis cél.</td>
    <td>
    <p>Adja hozzá AzureSqlJobChildTarget</p>
    <p>Remove-AzureSqlJobChildTarget</p>
    </td>
  </tr>

<tr>
    <td>Feladat</td>
    <td>
    <p>Paraméterek használt tootrigger végrehajtási vagy toofulfill ütemezett feladat meghatározását.</p>
    </td>
    <td>
    <p>Get-AzureSqlJob</p>
    <p>Új AzureSqlJob</p>
    <p>Set-AzureSqlJob</p>
    </td>
  </tr>

<tr>
    <td>Feladat végrehajtása</td>
    <td>
    <p>Tároló szükséges toofulfill tevékenységek a parancsfájl végrehajtása vagy a hitelesítő adatok használatával az adatbázis-kapcsolat hibákkal DACPAC tooa target alkalmazása kezelje a megfelelően tooan végrehajtási házirend.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Start-AzureSqlJobExecution</p>
    <p>STOP-AzureSqlJobExecution</p>
    <p>Várjon, amíg-AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>A feladat a végrehajtás feladat</td>
    <td>
    <p>Munkahelyi toofulfill egy feladatot egyetlen egységként.</p>
    <p>Ha egy feladat nem tud toosuccessfully hajtható végre, hello eredményül kapott Kivételüzenet naplózza a rendszer és egy új egyező feladata létrejön és hajtotta végre a megadott összhangban toohello végrehajtási házirendet.</p></p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Start-AzureSqlJobExecution</p>
    <p>STOP-AzureSqlJobExecution</p>
    <p>Várjon, amíg-AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>Feladat-végrehajtási házirend</td>
    <td>
    <p>Vezérlők sikertelen feladat-végrehajtási időtúllépés, a újrapróbálkozási korlátozások és a próbálkozások közötti időintervallumot.</p>
    <p>Rugalmas adatbázis-feladatok tartalmaz egy alapértelmezett feladat végrehajtási házirendet, amely a feladat sikertelen feladatok gyakorlatilag végtelen újrapróbálkozások okozhat a exponenciális leállítási az intervallumok között minden próbálkozzon újra.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecutionPolicy</p>
    <p>Új AzureSqlJobExecutionPolicy</p>
    <p>Set-AzureSqlJobExecutionPolicy</p>
    </td>
  </tr>

<tr>
    <td>Ütemezés</td>
    <td>
    <p>Ideje végrehajtási tootake hely feladatról intervallumban vagy egyetlen egyszerre specifikáció alapján.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobSchedule</p>
    <p>Új AzureSqlJobSchedule</p>
    <p>Set-AzureSqlJobSchedule</p>
    </td>
  </tr>

<tr>
    <td>Feladat indítja el</td>
    <td>
    <p>Egy feladat és egy ütemezés tootrigger feladat végrehajtása toohello ütemezés szerint közötti leképezést.</p>
    </td>
    <td>
    <p>Új AzureSqlJobTrigger</p>
    <p>Remove-AzureSqlJobTrigger</p>
    </td>
  </tr>
</table>

## <a name="supported-elastic-database-jobs-group-types"></a>Támogatott rugalmas adatbázis-feladatok csoportban típusok
hello feladat végrehajtja a Transact-SQL (T-SQL) parancsfájlok vagy DACPACs alkalmazásának adatbázisok csoportja között. Ha egy feladat végrehajtása között elküldött toobe egy csoportot az adatbázisok, hello feladat "kibontja" hello gyermek feladatok, ahol minden egyes végez hello kért végrehajtási hello csoport egyetlen adatbázis. 

A csoportokat, amelyek hozhat létre két típusa van: 

* [A shard térkép](sql-database-elastic-scale-shard-map-management.md) csoport: Ha egy feladat elküldött tootarget shard térképet, hello feladat hello shard térkép toodetermine lekérdezi az aktuális készletében lévő szilánkok, és majd létrehoz gyermek minden shard feladataihoz hello shard leképezés.
* Egyéni gyűjtési csoportjának: egy egyénileg definiált adatbázisok készleteit. Ha egy feladat egyéni gyűjtemény célozza, hozna létre gyermek feladatokat az egyes adatbázisok jelenleg hello egyéni gyűjtéshez.

## <a name="tooset-hello-elastic-database-jobs-connection"></a>tooset hello rugalmas adatbázis-feladatok kapcsolat
A kapcsolat kell toohello feladatok toobe beállítása *feladatvezérlő adatbázishoz* előzetes toousing hello feladatok API-k. A hitelesítő adatok ablak toopop fel a kért hello felhasználónevet és jelszót a rugalmas adatbázis-feladatok telepítésekor létrehozott futtatja ezt a parancsmagot váltja ki. Ebben a témakörben közölt összes példák feltételezik, hogy az első lépéshez már végbementek.

Nyissa meg a kapcsolat toohello rugalmas feladatok:

    Use-AzureSqlJobConnection -CurrentAzureSubscription 

## <a name="encrypted-credentials-within-hello-elastic-database-jobs"></a>Hello rugalmas adatbázis-feladatok belül titkosított hitelesítő adatokat
Adatbázis-hitelesítő adatok szúrhatók be hello feladatok *feladatvezérlő adatbázishoz* a jelszóval titkosított. Akkor szükséges toostore hitelesítő adatok tooenable feladatok toobe egy későbbi időpontban végre (a feladatok ütemezésének használatával).

Titkosítási működik keresztül hello telepítési parancsfájl részeként létrehozott tanúsítványt. hello telepítési parancsfájlja létrehozza, és az Azure Cloud Service hello hello visszafejtése a feltöltések hello tanúsítvány titkosított jelszavak. Azure Cloud Service hello később hello nyilvános kulcsot hello feladatok belül tárolja *feladatvezérlő adatbázishoz* amely hello PowerShell API-t vagy a klasszikus Azure portál felület tooencrypt megadott jelszóval lehetővé anélkül, hogy hello tanúsítvány helyileg telepített toobe.

titkosított és védett tooElastic adatbázis-feladatok objektumok csak olvasási hozzáféréssel rendelkező felhasználókat a rendszer a hello hitelesítő adat jelszavak. Azonban lehetséges, hogy egy rosszindulatú felhasználó olvasási és írási hozzáférése tooElastic adatbázis feladatok objektumok tooextract jelszót. Hitelesítő adatok tervezett toobe feladat végrehajtások ismételten. Hitelesítő adatok tootarget adatbázisok átadott kapcsolatok létesítéséhez. Jelenleg nincs korlátozás az egyes hitelesítési használt hello céladatbázisokhoz, rosszindulatú felhasználó hozzáadhat egy adatbázis cél adatbázis a hello támadásokat. hello felhasználó később volt az adatbázis toogain hello hitelesítő adatához tartozó jelszó célzó feladat indítása.

Ajánlott biztonsági eljárások a rugalmas adatbázis-feladatok a következők:

* Hello API-k tootrusted egyének-használatát korlátozása.
* Hitelesítő adatok lehetnek hello legalacsonyabb jogosultságok szükséges tooperform hello feladata.  További információ látható belül ez [engedélyezési és engedélyek](https://msdn.microsoft.com/library/bb669084.aspx) SQL Server MSDN-cikk tárgyalja.

### <a name="toocreate-an-encrypted-credential-for-job-execution-across-databases"></a>egy titkosított hitelesítő adatokat, a feladat végrehajtása az adatbázisok közötti toocreate
egy új titkosított toocreate hitelesítőadat-, hello [ **Get-Credential parancsmag** ](https://technet.microsoft.com/library/hh849815.aspx) megadását kéri a felhasználónevet és jelszót, amelyek átadhatók toohello [ **New-AzureSqlJobCredential a parancsmag**](/powershell/module/elasticdatabasejobs/new-azuresqljobcredential).

    $credentialName = "{Credential Name}"
    $databaseCredential = Get-Credential
    $credential = New-AzureSqlJobCredential -Credential $databaseCredential -CredentialName $credentialName
    Write-Output $credential

### <a name="tooupdate-credentials"></a>tooupdate hitelesítő adatok
Ha jelszavak módosításához használja a hello [ **Set-AzureSqlJobCredential parancsmag** ](/powershell/module/elasticdatabasejobs/set-azuresqljobcredential) és set hello **CredentialName** paraméter.

    $credentialName = "{Credential Name}"
    Set-AzureSqlJobCredential -CredentialName $credentialName -Credential $credential 

## <a name="toodefine-an-elastic-database-shard-map-target"></a>egy rugalmas adatbázist shard térkép célhoz toodefine
tooexecute egy feladat shard csoportban lévő összes adatbázis ellen (használatával létrehozott [Elastic Database ügyféloldali kódtárának](sql-database-elastic-database-client-library.md)), használja a shard leképezését hello adatbázis célként. Ebben a példában létrehozott hello Elastic Database ügyféloldali kódtárának szilánkos alkalmazás szükséges. Lásd: [Ismerkedés a rugalmas adatbázis eszközök minta](sql-database-elastic-scale-get-started.md).

hello shard manager adatbázist be kell állítani egy adatbázis célként, és majd hello adott shard térkép meg kell adni, célként.

    $shardMapCredentialName = "{Credential Name}"
    $shardMapDatabaseName = "{ShardMapDatabaseName}" #example: ElasticScaleStarterKit_ShardMapManagerDb
    $shardMapDatabaseServerName = "{ShardMapServerName}"
    $shardMapName = "{MyShardMap}" #example: CustomerIDShardMap
    $shardMapDatabaseTarget = New-AzureSqlJobTarget -DatabaseName $shardMapDatabaseName -ServerName $shardMapDatabaseServerName
    $shardMapTarget = New-AzureSqlJobTarget -ShardMapManagerCredentialName $shardMapCredentialName -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapDatabaseServerName -ShardMapName $shardMapName
    Write-Output $shardMapTarget

## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Az adatbázisok közötti végrehajtási T-SQL parancsfájl létrehozása
T-SQL-parancsfájlok végrehajtásra létrehozásakor ajánlott toobuild őket toobe [idempotent](https://en.wikipedia.org/wiki/Idempotence) és refs-hibákkal szemben. Rugalmas adatbázis-feladatok parancsfájl végrehajtásának próbálkozik, ha végrehajtási hiba, függetlenül hello besorolás hello hiba észlel.

Használjon hello [ **New-AzureSqlJobContent parancsmag** ](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent) toocreate és mentse a parancsfájlt a végrehajtás, és állítsa be a hello **- ContentName** és **- CommandText**paraméterek.

    $scriptName = "Create a TestTable"

    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
        CREATE TABLE TestTable(
            TestTableId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="create-a-new-script-from-a-file"></a>Új parancsfájl létrehozása fájlból
Ha a T-SQL parancsfájl hello fájlban van definiálva, a tooimport hello parancsprogram használata:

    $scriptName = "My Script Imported from a File"
    $scriptPath = "{Path tooSQL File}"
    $scriptCommandText = Get-Content -Path $scriptPath
    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="tooupdate-a-t-sql-script-for-execution-across-databases"></a>az adatbázisok közötti végrehajtásra tooupdate egy T-SQL parancsfájl
A PowerShell parancsfájl frissítések hello T-SQL egy meglévő parancsfájl parancsszövege.

A következő változók tooreflect hello beállítása hello parancsfájl definition toobe set szükséges:

    $scriptName = "Create a TestTable"
    $scriptUpdateComment = "Adding AdditionalInformation column tooTestTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
    CREATE TABLE TestTable(
        TestTableId INT PRIMARY KEY IDENTITY,
        InsertionTime DATETIME2
    );
    END
    GO

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN
    ALTER TABLE TestTable
    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO"

### <a name="tooupdate-hello-definition-tooan-existing-script"></a>tooupdate hello definition tooan meglévő parancsfájl
    Set-AzureSqlJobContentDefinition -ContentName $scriptName -CommandText $scriptCommandText -Comment $scriptUpdateComment 

## <a name="toocreate-a-job-tooexecute-a-script-across-a-shard-map"></a>egy feladat tooexecute keresztül shard térképre parancsfájl toocreate
A PowerShell parancsfájl keresztül minden shard rugalmasan méretezhető shard leképezés indít el egy parancsfájl végrehajtásának feladatot.

A következő változók tooreflect hello beállítása hello szükséges parancsfájl és a cél:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $credentialName = "{Credential Name}"
    $shardMapTarget = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName 
    $job = New-AzureSqlJob -ContentName $scriptName -CredentialName $credentialName -JobName $jobName -TargetId $shardMapTarget.TargetId
    Write-Output $job

## <a name="tooexecute-a-job"></a>egy feladat tooexecute
A PowerShell-parancsfájl egy létező feladat végrehajtása:

Frissítés hello változó tooreflect hello kívánt feladat neve toohave hajtotta végre a következő:

    $jobName = "{Job Name}"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution

## <a name="tooretrieve-hello-state-of-a-single-job-execution"></a>egyetlen feladat-végrehajtás tooretrieve hello állapota
Használjon hello [ **Get-AzureSqlJobExecution parancsmag** ](/powershell/module/elasticdatabasejobs/get-azuresqljobexecution) és set hello **JobExecutionId** paraméter tooview hello állapotának feladat végrehajtása.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecution = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId
    Write-Output $jobExecution

Használja az azonos hello **Get-AzureSqlJobExecution** hello parancsmagot **IncludeChildren** paraméter tooview hello állapotának gyermek feladat végrehajtások, nevezetesen hello minden egyes feladatok végrehajtásának meghatározott állapotban hello feladat által megcélzott adatbázis.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="tooview-hello-state-across-multiple-job-executions"></a>több feladat végrehajtások közötti tooview hello állapota
Hello [ **Get-AzureSqlJobExecution parancsmag** ](/powershell/module/elasticdatabasejobs/new-azuresqljob) több nem kötelező paraméter, amely használt toodisplay több feladat végrehajtások, megadott hello paramétereknek szűrve van. hello következő hello lehetséges módjait toouse Get-AzureSqlJobExecution némelyike mutatja be:

Lekéri az összes aktív felső szintű feladat végrehajtások:

    Get-AzureSqlJobExecution

Lekéri az összes felső szintű feladat végrehajtások, beleértve az inaktív feladat végrehajtások:

    Get-AzureSqlJobExecution -IncludeInactive

Lekéri az összes gyermek feladat végrehajtások inaktív feladat végrehajtások többek között a megadott feladat végrehajtási Azonosítóhoz:

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId -IncludeInactive -IncludeChildren

Ütemezés használatával létrehozott összes feladat végrehajtások beolvasása / feladat-kombináció, beleértve az inaktív feladatok:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive

Lekéri az összes feladat megadott shard térképre, beleértve az inaktív feladatokat célcsoportkezelést:

    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive

Lekéri az összes feladat a megadott egyéni gyűjtemény, beleértve az inaktív feladatokat célcsoportkezelést:

    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive

Feladat feladat végrehajtások belül egy adott feladat végrehajtási hello listájának beolvasása:

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions 

Feladat végrehajtási részlete beolvasása:

a következő PowerShell-parancsfájl hello lehet egy feladat a feladat a végrehajtás, amely akkor különösen hasznos, ha végrehajtási hibakeresésére használt tooview hello részleteit.

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="tooretrieve-failures-within-job-task-executions"></a>feladat feladat végrehajtások belül tooretrieve hibák
Hello **JobTaskExecution objektum** hello életciklus hello feladat egy üzenettulajdonság együtt egy tulajdonság tartalmazza. Ha egy feladat a feladat végrehajtása sikertelen volt, hello életciklus tulajdonság túl állítható*sikertelen* és hello üzenettulajdonság lesz toohello eredményül kapott hibaüzenetet, és a verem. Ha egy feladat sikertelen volt, akkor fontos tooview hello részleteit munka feladatai, amelyek egy adott feladat sikertelen volt.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="toowait-for-a-job-execution-toocomplete"></a>a feladat végrehajtási toocomplete a toowait
a következő PowerShell-parancsfájl hello lehet egy feladat feladat toocomplete a használt toowait:

    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId 

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

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval 
    -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy

### <a name="update-a-custom-execution-policy"></a>Egy egyéni végrehajtási házirend frissítése
Frissítés hello végrehajtási házirend tooupdate szükséges:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy

## <a name="cancel-a-job"></a>Feladatok megszakítása
Rugalmas adatbázis-feladatok a feladatok megszakítását kérelmeket támogatja.  Rugalmas adatbázis-feladatok jelenleg végrehajtás alatt álló feladat a lemondási kérelmet észleli, ha a program megpróbálja toostop hello feladat.

Rugalmas adatbázis-feladatok is hajtsa végre a megszakítási két különböző módja van:

1. Jelenleg feldolgozás alatt álló feladatok Mégse: törlése közben jelenleg fut egy feladat észleli, ha a megszakítási belül jelenleg végrehajtás alatt álló hello feladat aspektusa hello történt kísérlet.  Példa: Ha egy hosszú ideig tartó jelenleg végrehajtás alatt álló lekérdezés törlése megkísérelt van, nem lesznek egy kísérlet toocancel hello lekérdezést.
2. Tevékenység-újrapróbálkozások megszakítása: törlése észlelésekor hello vezérlő szál feladat a végrehajtás elindítása előtt hello vezérlő szál elkerülése hello feladat elindítása és hello kérelem deklarálható megszakítottként.

Ha a feladat megszakításának szülő-feladat van szükség, hello lemondási kérelmet szerződéses kötelezettségeket a hello szülő feladat és összes a gyermek-feladatokkal.

a megszakítási kérelmet, toosubmit hello használata [ **Stop-AzureSqlJobExecution parancsmag** ](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) és set hello **JobExecutionId** paraméter.

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="toodelete-a-job-and-job-history-asynchronously"></a>egy feladat toodelete és aszinkron módon feladatelőzmények
Rugalmas adatbázis-feladatok támogatja az aszinkron feladatok törlése. Egy feladat is törlésre, és hello rendszer törölni fogja a hello feladat és a feladatelőzményekben összes feladat végrehajtások hello feladat befejezése után. hello rendszer nem fogja automatikusan megszakítja a aktív feladat végrehajtások.  

Invoke [ **Stop-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) toocancel aktív feladat végrehajtások.

tootrigger feladat törlése, használjon hello [ **Remove-AzureSqlJob parancsmag** ](/powershell/module/elasticdatabasejobs/remove-azuresqljob) és set hello **JobName** paraméter.

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName

## <a name="toocreate-a-custom-database-target"></a>Egyéni adatbázis target toocreate
Egyéni adatbázis célok közvetlen végrehajtásra vagy egy egyéni adatbázis csoportban foglalható adhat meg. Például mert **rugalmas készletek** van még nem támogatott PowerShell API-val, létrehozhat egy egyéni adatbázis célként és egy egyéni adatbázis gyűjtemény célként, ami magában foglalja az összes hello adatbázis hello készletben.

Állítsa be a következő változók tooreflect hello szükséges adatbázis-információ hello:

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="toocreate-a-custom-database-collection-target"></a>Egyéni adatbázis gyűjtemény célja toocreate
Használjon hello [ **New-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) parancsmag toodefine egy egyéni adatbázis gyűjtemény cél tooenable végrehajtása több meghatározott adatbázis cél között. Egy adatbázis-csoport létrehozása után adatbázisok társítható hello egyéni gyűjtemény célja.

Állítsa be a következő változók tooreflect hello kívánt egyéni gyűjtemény konfigurációjához hello:

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="tooadd-databases-tooa-custom-database-collection-target"></a>tooadd adatbázisok tooa egyéni adatbázis gyűjtemény célja
egy adatbázis tooa egyéni gyűjteményre tooadd hello használata [ **Add-AzureSqlJobChildTarget** ](/powershell/module/elasticdatabasejobs/add-azuresqljobchildtarget) parancsmag.

    $databaseServerName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-hello-databases-within-a-custom-database-collection-target"></a>Tekintse át a hello adatbázisok belül egy egyéni adatbázis-gyűjtemény célja
Használjon hello [ **Get-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) parancsmag tooretrieve hello gyermek adatbázis egy egyéni adatbázis-gyűjtemény célja. 

    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-tooexecute-a-script-across-a-custom-database-collection-target"></a>Hozzon létre egy feladat tooexecute egy parancsprogramot egy egyéni adatbázis-gyűjtemény célja között
Használjon hello [ **New-AzureSqlJob** ](/powershell/module/elasticdatabasejobs/new-azuresqljob) parancsmag toocreate egy feladatot, egy egyéni adatbázis gyűjtemény tároló által definiált adatbázisok csoportja ellen. Rugalmas adatbázis-feladatok hello feladat kibővítése minden megfelelő tooa adatbázis társított hello egyéni adatbázis-gyűjtemény célja, és győződjön meg arról, hogy minden egyes adatbázison hello parancsfájl végrehajtása több gyermek feladat. Ebben az esetben fontos, hogy a parancsfájlok az idempotent toobe rugalmas tooretries.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Az adatbázisok közötti adatok gyűjtése
Egy feladat tooexecute lekérdezés használja az adatbázisok csoportja és küldhet hello eredmények tooa adott táblához. hello tábla hello tény toosee hello lekérdezés eredményében minden adatbázisból után kérdezhetők le. Így lehetővé teszi egy aszinkron metódus tooexecute lekérdezés több adatbázis közötti. Sikertelen bejelentkezési kísérletek újrapróbálkozások keresztül automatikusan kezeli.

hello megadott céltábla automatikusan létrejön, ha még nem létezik. Új tábla hello eredményhalmazt adott hello hello sémája megegyezik. Egy parancsfájl több eredménykészlet adja vissza, ha a rugalmas adatbázis-feladatok csak küldi hello első toohello célként megadott táblája.

hello következő PowerShell-parancsfájl végrehajtja egy parancsfájlt, és összegyűjti az eredményeket egy megadott táblába. Ez a parancsfájl feltételezi, hogy egy T-SQL parancsfájl létrejött-e amelyek kimenete egy eredményhalmaz és, hogy létrejött-e egy egyéni adatbázis-gyűjtemény célja.

A parancsfájl a hello [ **Get-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) parancsmag. A parancsfájl, a hitelesítő adatokat és a végrehajtási cél hello paraméterek beállítása:

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

### <a name="toocreate-and-start-a-job-for-data-collection-scenarios"></a>toocreate és kezdő egy feladatot az adatok gyűjtése forgatókönyvek
A parancsfájl a hello [ **Start-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/start-azuresqljobexecution) parancsmag.

    $job = New-AzureSqlJob -JobName $jobName 
    -CredentialName $executionCredentialName 
    -ContentName $scriptName 
    -ResultSetDestinationServerName $destinationServerName 
    -ResultSetDestinationDatabaseName $destinationDatabaseName 
    -ResultSetDestinationSchemaName $destinationSchemaName 
    -ResultSetDestinationTableName $destinationTableName 
    -ResultSetDestinationCredentialName $destinationCredentialName 
    -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="tooschedule-a-job-execution-trigger"></a>a feladat végrehajtási eseményindító tooschedule
a következő PowerShell-parancsfájl hello lehet használt toocreate ismétlődő ütemezés szerint. A parancsfájl a perces időközt, de [ **New-AzureSqlJobSchedule** ](/powershell/module/elasticdatabasejobs/new-azuresqljobschedule) - DayInterval, - HourInterval, - MonthInterval, és - WeekInterval paramétereket is támogatja. Csak egyszer hajtható végre ütemezéseket is létrehozható, hogy - alkalommal.

Új ütemezés létrehozása:

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule 
    -MinuteInterval $minuteInterval 
    -ScheduleName $scheduleName 
    -StartTime $startTime 
    Write-Output $schedule

### <a name="tootrigger-a-job-executed-on-a-time-schedule"></a>a feladat végrehajtása ütemezéssel tootrigger
Egy feladat eseményindító lehet meghatározott toohave egy feladat végrehajtása függően tooa ütemezéssel. a következő PowerShell-parancsfájl hello lehet használt toocreate feladat eseményindítót.

Használjon [New-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/new-azuresqljobtrigger) és hello beállítása a következő változók toocorrespond toohello kívánt feladat és ütemezése:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger
    -ScheduleName $scheduleName
    -JobName $jobName
    Write-Output $jobTrigger

### <a name="tooremove-a-scheduled-association-toostop-job-from-executing-on-schedule"></a>tooremove egy ütemezett társítás toostop feladat futtatásának ütemezés szerint
Ismétlődés feladat végrehajtása a feladat eseményindítót, hello feladat eseményindító keresztül toodiscontinue távolítható el. Távolítsa el a egy feladat eseményindító toostop egy feladat a végrehajtás alatt álló hello függően tooa ütemezés [ **Remove-AzureSqlJobTrigger parancsmag**](/powershell/module/elasticdatabasejobs/remove-azuresqljobtrigger).

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger 
    -ScheduleName $scheduleName 
    -JobName $jobName

### <a name="retrieve-job-triggers-bound-tooa-time-schedule"></a>Eseményindítók kötött tooa idő ütemezett feladat beolvasása
a következő PowerShell-parancsfájl hello használt tooobtain lehetnek és hello eseményindítók regisztrált tooa adott időpontban az ütemezett feladat megjelenítése.

    $scheduleName = "{Schedule Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -ScheduleName $scheduleName
    Write-Output $jobTriggers

### <a name="tooretrieve-job-triggers-bound-tooa-job"></a>tooretrieve feladat eseményindítók kötött tooa feladat
Használjon [Get-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/get-azuresqljobtrigger) egy regisztrált feladatot tartalmazó tooobtain és megjelenítési ütemezéseket.

    $jobName = "{Job Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -JobName $jobName
    Write-Output $jobTriggers

## <a name="toocreate-a-data-tier-application-dacpac-for-execution-across-databases"></a>az adatbázisok közötti végrehajtásra adatrétegbeli alkalmazás (DACPAC) toocreate
egy DACPAC toocreate lásd [Adatrétegbeli alkalmazások](https://msdn.microsoft.com/library/ee210546.aspx). egy DACPAC toodeploy hello használata [New-AzureSqlJobContent parancsmag](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent). hello DACPAC elérhető toohello szolgáltatásnak kell lennie. Az ajánlott tooupload létrehozott DACPAC tooAzure tárolási, és hozzon létre egy [közös hozzáférésű Jogosultságkód](../storage/common/storage-dotnet-shared-access-signature-part-1.md) hello DACPAC számára.

    $dacpacUri = "{Uri}"
    $dacpacName = "{Dacpac Name}"
    $dacpac = New-AzureSqlJobContent -DacpacUri $dacpacUri -ContentName $dacpacName 
    Write-Output $dacpac

### <a name="tooupdate-a-data-tier-application-dacpac-for-execution-across-databases"></a>az adatbázisok közötti végrehajtásra adatrétegbeli alkalmazás (DACPAC) tooupdate
Rugalmas adatbázis-feladatok belül regisztrálva meglévő DACPACs frissített toopoint toonew URI lehet. Használjon hello [ **Set-AzureSqlJobContentDefinition parancsmag** ](/powershell/module/elasticdatabasejobs/set-azuresqljobcontentdefinition) tooupdate hello DACPAC URI egy olyan regisztrált DACPAC:

    $dacpacName = "{Dacpac Name}"
    $newDacpacUri = "{Uri}"
    $updatedDacpac = Set-AzureSqlJobDacpacDefinition -ContentName $dacpacName -DacpacUri $newDacpacUri
    Write-Output $updatedDacpac

## <a name="toocreate-a-job-tooapply-a-data-tier-application-dacpac-across-databases"></a>egy feladat tooapply egy adatrétegbeli alkalmazás (DACPAC) az adatbázisok közötti toocreate
Miután egy DACPAC rugalmas adatbázis-feladatok belül létrehozott, egy feladat hozhatók létre tooapply hello DACPAC adatbázisok csoportja között. a következő PowerShell-parancsfájl hello használt toocreate DACPAC feladat lehet egy egyéni gyűjtéshez. az adatbázisok között:

    $jobName = "{Job Name}"
    $dacpacName = "{Dacpac Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget 
    -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob 
    -JobName $jobName 
    -CredentialName $credentialName 
    -ContentName $dacpacName -TargetId $target.TargetId
    Write-Output $job 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-powershell/cmd-prompt.png
[2]: ./media/sql-database-elastic-jobs-powershell/portal.png
<!--anchors-->
