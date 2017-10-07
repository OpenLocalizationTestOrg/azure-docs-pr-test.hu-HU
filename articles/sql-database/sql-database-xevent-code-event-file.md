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
# <a name="event-file-target-code-for-extended-events-in-sql-database"></a>Fájl cél eseménykód kiterjesztett események az SQL-adatbázis

[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

A hatékony módon toocapture és a jelentés adatait egy kiterjesztett esemény teljes kódminta használni szeretne.

A Microsoft SQL Server hello [esemény cél](http://msdn.microsoft.com/library/ff878115.aspx) használt toostore esemény kimenetek egy helyi merevlemezen fájlba van. Azonban az ilyen fájlok nincsenek elérhető tooAzure SQL-adatbázis. Ehelyett hello Azure Storage szolgáltatás toosupport hello Eseményfájlt célja használjuk.

Ez a témakör egy kétfázisú példakód mutatja be:

* PowerShell, toocreate hello felhőben egy Azure Storage-tárolót.
* Transact-SQL:
  
  * tooassign hello Azure Storage tároló tooan Eseményfájlt cél.
  * toocreate és kezdő hello esemény-munkamenethez, és így tovább.

## <a name="prerequisites"></a>Előfeltételek

* Azure-fiók és -előfizetés. Regisztrálhat [ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/).
* Létrehozhat egy olyan táblázatában adatbázisoknak.
  
  * Igény szerint is [hozzon létre egy **AdventureWorksLT** mintaadatbázis](sql-database-get-started.md) perc múlva.
* SQL Server Management Studio (ssms.exe) ideális esetben a havi frissítés letöltéséhez. 
  Letöltheti a legújabb ssms.exe hello származó:
  
  * Című témakör [töltse le az SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).
  * [Egy közvetlen hivatkozást toohello letölthető.](http://go.microsoft.com/fwlink/?linkid=616025)
* Rendelkeznie kell hello [Azure PowerShell-modulok](http://go.microsoft.com/?linkid=9811175) telepítve.
  
  * hello modulok biztosítanak parancsok, mint - **New-AzureStorageAccount**.

## <a name="phase-1-powershell-code-for-azure-storage-container"></a>1. fázis: PowerShell-kódjába Azure Storage-tároló

A PowerShell hello kétfázisú kódminta 1. fázis.

hello parancsfájl kezdődik parancsok tooclean után a korábbi valószínűleg futtatni, és rerunnable.

1. Illessze be egy egyszerű szövegszerkesztőben, például a Notepad.exe hello PowerShell-parancsfájlt, és mentse a hello parancsfájl hello kiterjesztésű fájlként **.ps1**.
2. Indítsa el a PowerShell ISE rendszergazdaként.
3. Hello parancssorába írja be a<br/>`Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser`<br/>és nyomja le az ENTER billentyűt.
4. A PowerShell ISE, nyissa meg a **.ps1** fájlt. Hello parancsprogrammal.
5. hello parancsfájl először elindul, amelyben a bejelentkezéskor tooAzure új ablakban.
   
   * Ha Újrafuttatja hello parancsfájl a munkamenet megszakítása nélkül, lehetősége van hello kényelmes a kimenő hello fűzött megjegyzések **Add-AzureAccount** parancsot.

![PowerShell ISE, készen áll a toorun parancsfájl telepítve, az Azure modullal.][30_powershell_ise]


### <a name="powershell-code"></a>PowerShell-kódot

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


Jegyezze fel a hello néhány olyan nevesített értékek, amelyek hello PowerShell parancsfájl kiírja, amikor ez befejeződik. A 2. fázis, a következő Transact-SQL parancsfájl hello szerkesztenie kell ezeket az értékeket.

## <a name="phase-2-transact-sql-code-that-uses-azure-storage-container"></a>2. fázis: Transact-SQL Azure-tárolót használó kódot

* A fenti 1 fázisban futtatta a PowerShell parancsfájl toocreate egy Azure Storage-tárolót.
* Ezután a 2. fázis, hello következő Transact-SQL parancsfájlt kell használnia hello tároló.

hello parancsfájl kezdődik parancsok tooclean után a korábbi valószínűleg futtatni, és rerunnable.

PowerShell parancsfájl hello néhány névvel ellátott értékek nyomtatva ért véget. Hello Transact-SQL parancsfájl toouse ezeket az értékeket kell szerkeszteni. Található **TODO** hello Transact-SQL parancsfájl toolocate hello szerkeszteni pontok.

1. Nyissa meg az SQL Server Management Studio (ssms.exe).
2. Csatlakozás tooyour Azure SQL Database adatbázishoz.
3. Kattintson a tooopen egy új lekérdezési ablak.
4. Illessze be a következő Transact-SQL parancsfájl hello lekérdezési ablakba hello.
5. Található minden **TODO** a parancsfájl hello és, hogy megfelelő hello kell végrehajtania.
6. Mentse, és futtassa a hello parancsfájl.


> [!WARNING]
> SAS-kulcs értékét PowerShell parancsfájlt hello által generált hello kezdődhet a "?" (kérdőjel). Ha a következő T-SQL parancsfájl hello hello SAS-kulcsot használ, akkor meg kell *hello bevezető eltávolítása "?"* . Ellenkező esetben a próbálkozások biztonsági blokkolhatja.


### <a name="transact-sql-code"></a>Transact-SQL-kódot

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


Ha hello cél tooattach nem sikerül, ha futtatja, akkor állítsa le, és hello esemény-munkamenet újraindítása:

```sql
ALTER EVENT SESSION ... STATE = STOP;
GO
ALTER EVENT SESSION ... STATE = START;
GO
```


## <a name="output"></a>Kimenet

Hello Transact-SQL parancsfájl befejeztével kattintson egy cella alatt hello **event_data_XML** oszlop fejlécére. Egy  **<event>**  elem akkor jelenik meg, amely egy UPDATE utasítás mutatja.

Itt az egyik  **<event>**  elem, amely a tesztelés során jött létre:


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


rendszer függvény tooread hello event_file a következő Transact-SQL parancsfájl használt hello megelőző hello:

* [sys.fn_xe_file_target_read_file (Transact-SQL)](http://msdn.microsoft.com/library/cc280743.aspx)

Speciális beállítások hello megtekintésre kiterjesztett események adatainak magyarázata érhető el:

* [Speciális cél kiterjesztett események adatainak megtekintése](http://msdn.microsoft.com/library/mt752502.aspx)


## <a name="converting-hello-code-sample-toorun-on-sql-server"></a>Átalakítás hello kód a minta toorun SQL-kiszolgálón

Tegyük a Microsoft SQL Server Transact-SQL minta megelőző toorun hello.

* Az egyszerűség kedvéért célszerű hello Azure tároló toocompletely csere használatát egy egyszerű fájlt például **C:\myeventdata.xel**. hello fájl toohello helyi merevlemez-meghajtóról, amelyen az SQL Server számítógép hello tartalmazná.
* Nem kell semmilyen Transact-SQL-utasításainak **FŐKULCS létrehozása** és **hitelesítő adat létrehozása**.
* A hello **munkamenet esemény létrehozása** utasítás, a a **tároló hozzáadása** záradék, cserélje a hozzárendelt hello Http érték túl végrehajtott**Fájlnév =** példáulteljeselérésiútjakarakterláncot **C:\myfile.xel**.
  
  * Nincs Azure-tárfiók be kell vonni.

## <a name="more-information"></a>További információ

Fiókok és a tárolók hello Azure Storage szolgáltatás kapcsolatos további információkért lásd:

* [Hogyan toouse a .NET-Blob-tároló](../storage/blobs/storage-dotnet-how-to-use-blobs.md)
* [Elnevezésekor és a hivatkozó, tárolók, Blobok és metaadatok](http://msdn.microsoft.com/library/azure/dd135715.aspx)
* [Legfelső szintű tároló hello használata](http://msdn.microsoft.com/library/azure/ee395424.aspx)
* [1. lecke: Egy tárolt hozzáférési házirend és a közös hozzáférésű jogosultságkód létrehozása egy Azure-tárolót a](http://msdn.microsoft.com/library/dn466430.aspx)
  * [2. lecke: SQL Server hitelesítő adatok használatával a közös hozzáférésű jogosultságkód létrehozása](http://msdn.microsoft.com/library/dn466435.aspx)

<!--
Image references.
-->

[30_powershell_ise]: ./media/sql-database-xevent-code-event-file/event-file-powershell-ise-b30.png

