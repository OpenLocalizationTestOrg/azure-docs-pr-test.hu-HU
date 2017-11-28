---
title: "SQL-adatbázis Eseményfájlt kód aaaXEvent |} Microsoft Docs"
description: "PowerShell és a Transact-SQL biztosít egy kétfázisú példakód azt mutatja be, az Azure SQL Database-kiterjesztett esemény hello esemény File célnál. Az Azure Storage ebben a forgatókönyvben egy kötelező részét képezi."
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: 
tags: 
ms.assetid: bbb10ecc-739f-4159-b844-12b4be161231
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: genemi
ms.openlocfilehash: 4457bd3250f4644b54da2f7daddb9da12070e93a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="event-file-target-code-for-extended-events-in-sql-database"></a><span data-ttu-id="72029-104">Fájl cél eseménykód kiterjesztett események az SQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="72029-104">Event File target code for extended events in SQL Database</span></span>

[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

<span data-ttu-id="72029-105">A hatékony módon toocapture és a jelentés adatait egy kiterjesztett esemény teljes kódminta használni szeretne.</span><span class="sxs-lookup"><span data-stu-id="72029-105">You want a complete code sample for a robust way toocapture and report information for an extended event.</span></span>

<span data-ttu-id="72029-106">A Microsoft SQL Server hello [esemény cél](http://msdn.microsoft.com/library/ff878115.aspx) használt toostore esemény kimenetek egy helyi merevlemezen fájlba van.</span><span class="sxs-lookup"><span data-stu-id="72029-106">In Microsoft SQL Server, hello [Event File target](http://msdn.microsoft.com/library/ff878115.aspx) is used toostore event outputs into a local hard drive file.</span></span> <span data-ttu-id="72029-107">Azonban az ilyen fájlok nincsenek elérhető tooAzure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="72029-107">But such files are not available tooAzure SQL Database.</span></span> <span data-ttu-id="72029-108">Ehelyett hello Azure Storage szolgáltatás toosupport hello Eseményfájlt célja használjuk.</span><span class="sxs-lookup"><span data-stu-id="72029-108">Instead we use hello Azure Storage service toosupport hello Event File target.</span></span>

<span data-ttu-id="72029-109">Ez a témakör egy kétfázisú példakód mutatja be:</span><span class="sxs-lookup"><span data-stu-id="72029-109">This topic presents a two-phase code sample:</span></span>

* <span data-ttu-id="72029-110">PowerShell, toocreate hello felhőben egy Azure Storage-tárolót.</span><span class="sxs-lookup"><span data-stu-id="72029-110">PowerShell, toocreate an Azure Storage container in hello cloud.</span></span>
* <span data-ttu-id="72029-111">Transact-SQL:</span><span class="sxs-lookup"><span data-stu-id="72029-111">Transact-SQL:</span></span>
  
  * <span data-ttu-id="72029-112">tooassign hello Azure Storage tároló tooan Eseményfájlt cél.</span><span class="sxs-lookup"><span data-stu-id="72029-112">tooassign hello Azure Storage container tooan Event File target.</span></span>
  * <span data-ttu-id="72029-113">toocreate és kezdő hello esemény-munkamenethez, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="72029-113">toocreate and start hello event session, and so on.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="72029-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="72029-114">Prerequisites</span></span>

* <span data-ttu-id="72029-115">Azure-fiók és -előfizetés.</span><span class="sxs-lookup"><span data-stu-id="72029-115">An Azure account and subscription.</span></span> <span data-ttu-id="72029-116">Regisztrálhat [ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="72029-116">You can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="72029-117">Létrehozhat egy olyan táblázatában adatbázisoknak.</span><span class="sxs-lookup"><span data-stu-id="72029-117">Any database you can create a table in.</span></span>
  
  * <span data-ttu-id="72029-118">Igény szerint is [hozzon létre egy **AdventureWorksLT** mintaadatbázis](sql-database-get-started.md) perc múlva.</span><span class="sxs-lookup"><span data-stu-id="72029-118">Optionally you can [create an **AdventureWorksLT** demonstration database](sql-database-get-started.md) in minutes.</span></span>
* <span data-ttu-id="72029-119">SQL Server Management Studio (ssms.exe) ideális esetben a havi frissítés letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="72029-119">SQL Server Management Studio (ssms.exe), ideally its latest monthly update version.</span></span> 
  <span data-ttu-id="72029-120">Letöltheti a legújabb ssms.exe hello származó:</span><span class="sxs-lookup"><span data-stu-id="72029-120">You can download hello latest ssms.exe from:</span></span>
  
  * <span data-ttu-id="72029-121">Című témakör [töltse le az SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="72029-121">Topic titled [Download SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span></span>
  * [<span data-ttu-id="72029-122">Egy közvetlen hivatkozást toohello letölthető.</span><span class="sxs-lookup"><span data-stu-id="72029-122">A direct link toohello download.</span></span>](http://go.microsoft.com/fwlink/?linkid=616025)
* <span data-ttu-id="72029-123">Rendelkeznie kell hello [Azure PowerShell-modulok](http://go.microsoft.com/?linkid=9811175) telepítve.</span><span class="sxs-lookup"><span data-stu-id="72029-123">You must have hello [Azure PowerShell modules](http://go.microsoft.com/?linkid=9811175) installed.</span></span>
  
  * <span data-ttu-id="72029-124">hello modulok biztosítanak parancsok, mint - **New-AzureStorageAccount**.</span><span class="sxs-lookup"><span data-stu-id="72029-124">hello modules provide commands such as - **New-AzureStorageAccount**.</span></span>

## <a name="phase-1-powershell-code-for-azure-storage-container"></a><span data-ttu-id="72029-125">1. fázis: PowerShell-kódjába Azure Storage-tároló</span><span class="sxs-lookup"><span data-stu-id="72029-125">Phase 1: PowerShell code for Azure Storage container</span></span>

<span data-ttu-id="72029-126">A PowerShell hello kétfázisú kódminta 1. fázis.</span><span class="sxs-lookup"><span data-stu-id="72029-126">This PowerShell is phase 1 of hello two-phase code sample.</span></span>

<span data-ttu-id="72029-127">hello parancsfájl kezdődik parancsok tooclean után a korábbi valószínűleg futtatni, és rerunnable.</span><span class="sxs-lookup"><span data-stu-id="72029-127">hello script starts with commands tooclean up after a possible previous run, and is rerunnable.</span></span>

1. <span data-ttu-id="72029-128">Illessze be egy egyszerű szövegszerkesztőben, például a Notepad.exe hello PowerShell-parancsfájlt, és mentse a hello parancsfájl hello kiterjesztésű fájlként **.ps1**.</span><span class="sxs-lookup"><span data-stu-id="72029-128">Paste hello PowerShell script into a simple text editor such as Notepad.exe, and save hello script as a file with hello extension **.ps1**.</span></span>
2. <span data-ttu-id="72029-129">Indítsa el a PowerShell ISE rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="72029-129">Start PowerShell ISE as an Administrator.</span></span>
3. <span data-ttu-id="72029-130">Hello parancssorába írja be a</span><span class="sxs-lookup"><span data-stu-id="72029-130">At hello prompt, type</span></span><br/>`Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser`<br/><span data-ttu-id="72029-131">és nyomja le az ENTER billentyűt.</span><span class="sxs-lookup"><span data-stu-id="72029-131">and then press Enter.</span></span>
4. <span data-ttu-id="72029-132">A PowerShell ISE, nyissa meg a **.ps1** fájlt.</span><span class="sxs-lookup"><span data-stu-id="72029-132">In PowerShell ISE, open your **.ps1** file.</span></span> <span data-ttu-id="72029-133">Hello parancsprogrammal.</span><span class="sxs-lookup"><span data-stu-id="72029-133">Run hello script.</span></span>
5. <span data-ttu-id="72029-134">hello parancsfájl először elindul, amelyben a bejelentkezéskor tooAzure új ablakban.</span><span class="sxs-lookup"><span data-stu-id="72029-134">hello script first starts a new window in which you log in tooAzure.</span></span>
   
   * <span data-ttu-id="72029-135">Ha Újrafuttatja hello parancsfájl a munkamenet megszakítása nélkül, lehetősége van hello kényelmes a kimenő hello fűzött megjegyzések **Add-AzureAccount** parancsot.</span><span class="sxs-lookup"><span data-stu-id="72029-135">If you rerun hello script without disrupting your session, you have hello convenient option of commenting out hello **Add-AzureAccount** command.</span></span>

![PowerShell ISE, készen áll a toorun parancsfájl telepítve, az Azure modullal.][30_powershell_ise]


### <a name="powershell-code"></a><span data-ttu-id="72029-137">PowerShell-kódot</span><span class="sxs-lookup"><span data-stu-id="72029-137">PowerShell code</span></span>

```powershell
## TODO: Before running, find all 'TODO' and make each edit!!

#--------------- 1 -----------------------


# You can comment out or skip this Add-AzureAccount
# command after hello first run.
# Current PowerShell environment retains hello successful outcome.

'Expect a pop-up window in which you log in tooAzure.'


Add-AzureAccount

#-------------- 2 ------------------------


'
TODO: Edit hello values assigned toothese variables, especially hello first few!
'

# Ensure hello current date is between
# hello Expiry and Start time values that you edit here.

$subscriptionName    = 'YOUR_SUBSCRIPTION_NAME'
$policySasExpiryTime = '2016-01-28T23:44:56Z'
$policySasStartTime  = '2015-08-01'


$storageAccountName     = 'gmstorageaccountxevent'
$storageAccountLocation = 'West US'
$contextName            = 'gmcontext'
$containerName          = 'gmcontainerxevent'
$policySasToken         = 'gmpolicysastoken'


# Leave this value alone, as 'rwl'.
$policySasPermission = 'rwl'

#--------------- 3 -----------------------


# hello ending display lists your Azure subscriptions.
# One should match hello $subscriptionName value you assigned
#   earlier in this PowerShell script. 

'Choose an existing subscription for hello current PowerShell environment.'


Select-AzureSubscription -SubscriptionName $subscriptionName


#-------------- 4 ------------------------


'
Clean up hello old Azure Storage Account after any previous run, 
before continuing this new run.'


If ($storageAccountName)
{
    Remove-AzureStorageAccount -StorageAccountName $storageAccountName
}

#--------------- 5 -----------------------

[System.DateTime]::Now.ToString()

'
Create a storage account. 
This might take several minutes, will beep when ready.
  ...PLEASE WAIT...'

New-AzureStorageAccount `
    -StorageAccountName $storageAccountName `
    -Location           $storageAccountLocation

[System.DateTime]::Now.ToString()

[System.Media.SystemSounds]::Beep.Play()


'
Get hello primary access key for your storage account.
'


$primaryAccessKey_ForStorageAccount = `
    (Get-AzureStorageKey `
        -StorageAccountName $storageAccountName).Primary

"`$primaryAccessKey_ForStorageAccount = $primaryAccessKey_ForStorageAccount"

'Azure Storage Account cmdlet completed.
Remainder of PowerShell .ps1 script continues.
'

#--------------- 6 -----------------------


# hello context will be needed toocreate a container within hello storage account.

'Create a context object from hello storage account and its primary access key.
'

$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey  $primaryAccessKey_ForStorageAccount


'Create a container within hello storage account.
'


$containerObjectInStorageAccount = New-AzureStorageContainer `
    -Name    $containerName `
    -Context $context


'Create a security policy toobe applied toohello SAS token.
'

New-AzureStorageContainerStoredAccessPolicy `
    -Container  $containerName `
    -Context    $context `
    -Policy     $policySasToken `
    -Permission $policySasPermission `
    -ExpiryTime $policySasExpiryTime `
    -StartTime  $policySasStartTime 

'
Generate a SAS token for hello container.
'
Try
{
    $sasTokenWithPolicy = New-AzureStorageContainerSASToken `
        -Name    $containerName `
        -Context $context `
        -Policy  $policySasToken
}
Catch 
{
    $Error[0].Exception.ToString()
}

#-------------- 7 ------------------------


'Display hello values that YOU must edit into hello Transact-SQL script next!:
'

"storageAccountName: $storageAccountName"
"containerName:      $containerName"
"sasTokenWithPolicy: $sasTokenWithPolicy"

'
REMINDER: sasTokenWithPolicy here might start with "?" character, which you must exclude from Transact-SQL.
'

'
(Later, return here toodelete your Azure Storage account. See hello preceding - Remove-AzureStorageAccount -StorageAccountName $storageAccountName)'

'
Now shift toohello Transact-SQL portion of hello two-part code sample!'

# EOFile
```


<span data-ttu-id="72029-138">Jegyezze fel a hello néhány olyan nevesített értékek, amelyek hello PowerShell parancsfájl kiírja, amikor ez befejeződik.</span><span class="sxs-lookup"><span data-stu-id="72029-138">Take note of hello few named values that hello PowerShell script prints when it ends.</span></span> <span data-ttu-id="72029-139">A 2. fázis, a következő Transact-SQL parancsfájl hello szerkesztenie kell ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="72029-139">You must edit those values into hello Transact-SQL script that follows as phase 2.</span></span>

## <a name="phase-2-transact-sql-code-that-uses-azure-storage-container"></a><span data-ttu-id="72029-140">2. fázis: Transact-SQL Azure-tárolót használó kódot</span><span class="sxs-lookup"><span data-stu-id="72029-140">Phase 2: Transact-SQL code that uses Azure Storage container</span></span>

* <span data-ttu-id="72029-141">A fenti 1 fázisban futtatta a PowerShell parancsfájl toocreate egy Azure Storage-tárolót.</span><span class="sxs-lookup"><span data-stu-id="72029-141">In phase 1 of this code sample, you ran a PowerShell script toocreate an Azure Storage container.</span></span>
* <span data-ttu-id="72029-142">Ezután a 2. fázis, hello következő Transact-SQL parancsfájlt kell használnia hello tároló.</span><span class="sxs-lookup"><span data-stu-id="72029-142">Next in phase 2, hello following Transact-SQL script must use hello container.</span></span>

<span data-ttu-id="72029-143">hello parancsfájl kezdődik parancsok tooclean után a korábbi valószínűleg futtatni, és rerunnable.</span><span class="sxs-lookup"><span data-stu-id="72029-143">hello script starts with commands tooclean up after a possible previous run, and is rerunnable.</span></span>

<span data-ttu-id="72029-144">PowerShell parancsfájl hello néhány névvel ellátott értékek nyomtatva ért véget.</span><span class="sxs-lookup"><span data-stu-id="72029-144">hello PowerShell script printed a few named values when it ended.</span></span> <span data-ttu-id="72029-145">Hello Transact-SQL parancsfájl toouse ezeket az értékeket kell szerkeszteni.</span><span class="sxs-lookup"><span data-stu-id="72029-145">You must edit hello Transact-SQL script toouse those values.</span></span> <span data-ttu-id="72029-146">Található **TODO** hello Transact-SQL parancsfájl toolocate hello szerkeszteni pontok.</span><span class="sxs-lookup"><span data-stu-id="72029-146">Find **TODO** in hello Transact-SQL script toolocate hello edit points.</span></span>

1. <span data-ttu-id="72029-147">Nyissa meg az SQL Server Management Studio (ssms.exe).</span><span class="sxs-lookup"><span data-stu-id="72029-147">Open SQL Server Management Studio (ssms.exe).</span></span>
2. <span data-ttu-id="72029-148">Csatlakozás tooyour Azure SQL Database adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="72029-148">Connect tooyour Azure SQL Database database.</span></span>
3. <span data-ttu-id="72029-149">Kattintson a tooopen egy új lekérdezési ablak.</span><span class="sxs-lookup"><span data-stu-id="72029-149">Click tooopen a new query pane.</span></span>
4. <span data-ttu-id="72029-150">Illessze be a következő Transact-SQL parancsfájl hello lekérdezési ablakba hello.</span><span class="sxs-lookup"><span data-stu-id="72029-150">Paste hello following Transact-SQL script into hello query pane.</span></span>
5. <span data-ttu-id="72029-151">Található minden **TODO** a parancsfájl hello és, hogy megfelelő hello kell végrehajtania.</span><span class="sxs-lookup"><span data-stu-id="72029-151">Find every **TODO** in hello script and make hello appropriate edits.</span></span>
6. <span data-ttu-id="72029-152">Mentse, és futtassa a hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="72029-152">Save, and then run hello script.</span></span>


> [!WARNING]
> <span data-ttu-id="72029-153">SAS-kulcs értékét PowerShell parancsfájlt hello által generált hello kezdődhet a "?" (kérdőjel).</span><span class="sxs-lookup"><span data-stu-id="72029-153">hello SAS key value generated by hello preceding PowerShell script might begin with a '?' (question mark).</span></span> <span data-ttu-id="72029-154">Ha a következő T-SQL parancsfájl hello hello SAS-kulcsot használ, akkor meg kell *hello bevezető eltávolítása "?"* .</span><span class="sxs-lookup"><span data-stu-id="72029-154">When you use hello SAS key in hello following T-SQL script, you must *remove hello leading '?'*.</span></span> <span data-ttu-id="72029-155">Ellenkező esetben a próbálkozások biztonsági blokkolhatja.</span><span class="sxs-lookup"><span data-stu-id="72029-155">Otherwise your efforts might be blocked by security.</span></span>


### <a name="transact-sql-code"></a><span data-ttu-id="72029-156">Transact-SQL-kódot</span><span class="sxs-lookup"><span data-stu-id="72029-156">Transact-SQL code</span></span>

```sql
---- TODO: First, run hello PowerShell portion of this two-part code sample.
---- TODO: Second, find every 'TODO' in this Transact-SQL file, and edit each.

---- Transact-SQL code for Event File target on Azure SQL Database.


SET NOCOUNT ON;
GO


----  Step 1.  Establish one little table, and  ---------
----  insert one row of data.


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'gmTabEmployee')
BEGIN
    DROP TABLE gmTabEmployee;
END
GO


CREATE TABLE gmTabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO gmTabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO


------  Step 2.  Create key, and  ------------
------  Create credential (your Azure Storage container must already exist).


IF NOT EXISTS
    (SELECT * FROM sys.symmetric_keys
        WHERE symmetric_key_id = 101)
BEGIN
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '0C34C960-6621-4682-A123-C7EA08E3FC46' -- Or any newid().
END
GO


IF EXISTS
    (SELECT * FROM sys.database_scoped_credentials
        -- TODO: Assign AzureStorageAccount name, and hello associated Container name.
        WHERE name = 'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent')
BEGIN
    DROP DATABASE SCOPED CREDENTIAL
        -- TODO: Assign AzureStorageAccount name, and hello associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent] ;
END
GO


CREATE
    DATABASE SCOPED
    CREDENTIAL
        -- use '.blob.',   and not '.queue.' or '.table.' etc.
        -- TODO: Assign AzureStorageAccount name, and hello associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    WITH
        IDENTITY = 'SHARED ACCESS SIGNATURE',  -- "SAS" token.
        -- TODO: Paste in hello long SasToken string here for Secret, but exclude any leading '?'.
        SECRET = 'sv=2014-02-14&sr=c&si=gmpolicysastoken&sig=EjAqjo6Nu5xMLEZEkMkLbeF7TD9v1J8DNB2t8gOKTts%3D'
    ;
GO


------  Step 3.  Create (define) an event session.  --------
------  hello event session has an event with an action,
------  and a has a target.

IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'gmeventsessionname240b')
BEGIN
    DROP
        EVENT SESSION
            gmeventsessionname240b
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE

    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE 'UPDATE gmTabEmployee%'
            )
    ADD TARGET
        package0.event_file
            (
            -- TODO: Assign AzureStorageAccount name, and hello associated Container name.
            -- Also, tweak hello .xel file name at end, if you like.
            SET filename =
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b.xel'
            )
    WITH
        (MAX_MEMORY = 10 MB,
        MAX_DISPATCH_LATENCY = 3 SECONDS)
    ;
GO


------  Step 4.  Start hello event session.  ----------------
------  Issue hello SQL Update statements that will be traced.
------  Then stop hello session.

------  Note: If hello target fails tooattach,
------  hello session must be stopped and restarted.

ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = START;
GO


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
GO


ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = STOP;
GO


-------------- Step 5.  Select hello results. ----------

SELECT
        *, 'CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS!' as [CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS],
        CAST(event_data AS XML) AS [event_data_XML]  -- TODO: In ssms.exe results grid, double-click this cell!
    FROM
        sys.fn_xe_file_target_read_file
            (
                -- TODO: Fill in Storage Account name, and hello associated Container name.
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b',
                null, null, null
            );
GO


-------------- Step 6.  Clean up. ----------

DROP
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE;
GO

DROP DATABASE SCOPED CREDENTIAL
    -- TODO: Assign AzureStorageAccount name, and hello associated Container name.
    [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    ;
GO

DROP TABLE gmTabEmployee;
GO

PRINT 'Use PowerShell Remove-AzureStorageAccount toodelete your Azure Storage account!';
GO
```


<span data-ttu-id="72029-157">Ha hello cél tooattach nem sikerül, ha futtatja, akkor állítsa le, és hello esemény-munkamenet újraindítása:</span><span class="sxs-lookup"><span data-stu-id="72029-157">If hello target fails tooattach when you run, you must stop and restart hello event session:</span></span>

```sql
ALTER EVENT SESSION ... STATE = STOP;
GO
ALTER EVENT SESSION ... STATE = START;
GO
```


## <a name="output"></a><span data-ttu-id="72029-158">Kimenet</span><span class="sxs-lookup"><span data-stu-id="72029-158">Output</span></span>

<span data-ttu-id="72029-159">Hello Transact-SQL parancsfájl befejeztével kattintson egy cella alatt hello **event_data_XML** oszlop fejlécére.</span><span class="sxs-lookup"><span data-stu-id="72029-159">When hello Transact-SQL script completes, click a cell under hello **event_data_XML** column header.</span></span> <span data-ttu-id="72029-160">Egy  **<event>**  elem akkor jelenik meg, amely egy UPDATE utasítás mutatja.</span><span class="sxs-lookup"><span data-stu-id="72029-160">One **<event>** element is displayed which shows one UPDATE statement.</span></span>

<span data-ttu-id="72029-161">Itt az egyik  **<event>**  elem, amely a tesztelés során jött létre:</span><span class="sxs-lookup"><span data-stu-id="72029-161">Here is one **<event>** element that was generated during testing:</span></span>


```xml
<event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T19:18:45.420Z">
  <data name="state">
    <value>0</value>
    <text>Normal</text>
  </data>
  <data name="line_number">
    <value>5</value>
  </data>
  <data name="offset">
    <value>148</value>
  </data>
  <data name="offset_end">
    <value>368</value>
  </data>
  <data name="statement">
    <value>UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe'</value>
  </data>
  <action name="sql_text" package="sqlserver">
    <value>

SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
</value>
  </action>
</event>
```


<span data-ttu-id="72029-162">rendszer függvény tooread hello event_file a következő Transact-SQL parancsfájl használt hello megelőző hello:</span><span class="sxs-lookup"><span data-stu-id="72029-162">hello preceding Transact-SQL script used hello following system function tooread hello event_file:</span></span>

* [<span data-ttu-id="72029-163">sys.fn_xe_file_target_read_file (Transact-SQL)</span><span class="sxs-lookup"><span data-stu-id="72029-163">sys.fn_xe_file_target_read_file (Transact-SQL)</span></span>](http://msdn.microsoft.com/library/cc280743.aspx)

<span data-ttu-id="72029-164">Speciális beállítások hello megtekintésre kiterjesztett események adatainak magyarázata érhető el:</span><span class="sxs-lookup"><span data-stu-id="72029-164">An explanation of advanced options for hello viewing of data from extended events is available at:</span></span>

* [<span data-ttu-id="72029-165">Speciális cél kiterjesztett események adatainak megtekintése</span><span class="sxs-lookup"><span data-stu-id="72029-165">Advanced Viewing of Target Data from Extended Events</span></span>](http://msdn.microsoft.com/library/mt752502.aspx)


## <a name="converting-hello-code-sample-toorun-on-sql-server"></a><span data-ttu-id="72029-166">Átalakítás hello kód a minta toorun SQL-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="72029-166">Converting hello code sample toorun on SQL Server</span></span>

<span data-ttu-id="72029-167">Tegyük a Microsoft SQL Server Transact-SQL minta megelőző toorun hello.</span><span class="sxs-lookup"><span data-stu-id="72029-167">Suppose you wanted toorun hello preceding Transact-SQL sample on Microsoft SQL Server.</span></span>

* <span data-ttu-id="72029-168">Az egyszerűség kedvéért célszerű hello Azure tároló toocompletely csere használatát egy egyszerű fájlt például **C:\myeventdata.xel**.</span><span class="sxs-lookup"><span data-stu-id="72029-168">For simplicity, you would want toocompletely replace use of hello Azure Storage container with a simple file such as **C:\myeventdata.xel**.</span></span> <span data-ttu-id="72029-169">hello fájl toohello helyi merevlemez-meghajtóról, amelyen az SQL Server számítógép hello tartalmazná.</span><span class="sxs-lookup"><span data-stu-id="72029-169">hello file would be written toohello local hard drive of hello computer that hosts SQL Server.</span></span>
* <span data-ttu-id="72029-170">Nem kell semmilyen Transact-SQL-utasításainak **FŐKULCS létrehozása** és **hitelesítő adat létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="72029-170">You would not need any kind of Transact-SQL statements for **CREATE MASTER KEY** and **CREATE CREDENTIAL**.</span></span>
* <span data-ttu-id="72029-171">A hello **munkamenet esemény létrehozása** utasítás, a a **tároló hozzáadása** záradék, cserélje a hozzárendelt hello Http érték túl végrehajtott**Fájlnév =** példáulteljeselérésiútjakarakterláncot **C:\myfile.xel**.</span><span class="sxs-lookup"><span data-stu-id="72029-171">In hello **CREATE EVENT SESSION** statement, in its **ADD TARGET** clause, you would replace hello Http value assigned made too**filename=** with a full path string like **C:\myfile.xel**.</span></span>
  
  * <span data-ttu-id="72029-172">Nincs Azure-tárfiók be kell vonni.</span><span class="sxs-lookup"><span data-stu-id="72029-172">No Azure Storage account need be involved.</span></span>

## <a name="more-information"></a><span data-ttu-id="72029-173">További információ</span><span class="sxs-lookup"><span data-stu-id="72029-173">More information</span></span>

<span data-ttu-id="72029-174">Fiókok és a tárolók hello Azure Storage szolgáltatás kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="72029-174">For more info about accounts and containers in hello Azure Storage service, see:</span></span>

* [<span data-ttu-id="72029-175">Hogyan toouse a .NET-Blob-tároló</span><span class="sxs-lookup"><span data-stu-id="72029-175">How toouse Blob storage from .NET</span></span>](../storage/blobs/storage-dotnet-how-to-use-blobs.md)
* [<span data-ttu-id="72029-176">Elnevezésekor és a hivatkozó, tárolók, Blobok és metaadatok</span><span class="sxs-lookup"><span data-stu-id="72029-176">Naming and Referencing Containers, Blobs, and Metadata</span></span>](http://msdn.microsoft.com/library/azure/dd135715.aspx)
* [<span data-ttu-id="72029-177">Legfelső szintű tároló hello használata</span><span class="sxs-lookup"><span data-stu-id="72029-177">Working with hello Root Container</span></span>](http://msdn.microsoft.com/library/azure/ee395424.aspx)
* [<span data-ttu-id="72029-178">1. lecke: Egy tárolt hozzáférési házirend és a közös hozzáférésű jogosultságkód létrehozása egy Azure-tárolót a</span><span class="sxs-lookup"><span data-stu-id="72029-178">Lesson 1: Create a stored access policy and a shared access signature on an Azure container</span></span>](http://msdn.microsoft.com/library/dn466430.aspx)
  * [<span data-ttu-id="72029-179">2. lecke: SQL Server hitelesítő adatok használatával a közös hozzáférésű jogosultságkód létrehozása</span><span class="sxs-lookup"><span data-stu-id="72029-179">Lesson 2: Create a SQL Server credential using a shared access signature</span></span>](http://msdn.microsoft.com/library/dn466435.aspx)

<!--
Image references.
-->

[30_powershell_ise]: ./media/sql-database-xevent-code-event-file/event-file-powershell-ise-b30.png

