---
title: "vegyes egyesítéses szolgáltatás aaaDeploy |} Microsoft Docs"
description: "A felosztás és rugalmas adatbáziseszközöket egyesítés"
services: sql-database
documentationcenter: 
author: ddove
manager: jhubbard
editor: 
ms.assetid: 9a993c0f-7052-46cd-aa59-073bea8d535a
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 6fef641cbc1e73ce34a851c53298a072dade8f9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-split-merge-service"></a>Felosztási-egyesítési szolgáltatás üzembe helyezése
hello vegyes egyesítéses eszköz lehetővé teszi az adatok áthelyezése a szilánkos adatbázisok között. Lásd: [adatok kiterjesztett felhő adatbázisok közötti áthelyezése](sql-database-elastic-scale-overview-split-and-merge.md)

## <a name="download-hello-split-merge-packages"></a>Hello vegyes egyesítéses csomagok letöltése
1. Töltse le a NuGet legfrissebb hello a [NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).
2. Nyisson meg egy parancssort, és keresse meg a toohello könyvtárat, amelybe letöltötte nuget.exe. hello letöltése a PowerShell commmands tartalmazza.
3. Töltse le a hello legújabb vegyes egyesítéses csomagot az alábbi parancs hello hello aktuális könyvtárba:
   ```
   nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge
   ```  

hello fájlok kerülnek nevű **Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge.x.x.xxx.x** ahol *x.x.xxx.x* hello verziószám tükrözi. Található hello vegyes egyesítéses szolgáltatásfájlokat hello **content\splitmerge\service** alkönyvtára, és hello vegyes egyesítéses PowerShell-parancsfájlok (és a szükséges ügyfél .dll) a hello **content\splitmerge\powershell** alkönyvtárát.

## <a name="prerequisites"></a>Előfeltételek
1. Hello vegyes egyesítéses állapot adatbázisként használható Azure SQL Database-adatbázis létrehozása. Nyissa meg toohello [Azure-portálon](https://portal.azure.com). Hozzon létre egy új **SQL-adatbázis**. Nevezze el hello adatbázis, és hozzon létre egy új rendszergazda és a jelszót. Lehet, hogy toorecord hello nevét és jelszavát későbbi használatra.
2. Győződjön meg arról, hogy az Azure SQL Database-kiszolgáló lehetővé teszi, hogy Azure Services tooconnect tooit. Hello portálon, a hello **tűzfalbeállítások**, győződjön meg arról, hogy hello **hozzáférést tooAzure szolgáltatások** beállítás értéke túl**a**. Kattintson a "Mentés" ikonra hello.
   
   ![Engedélyezett szolgáltatások][1]
3. Azure-tárfiók létrehozása, amely jelzi a diagnosztikai kimenetet. Nyissa meg toohello Azure portálon. Hello bal oldali sávon kattintson **új**, kattintson a **adatok + tárolás**, majd **tárolási**.
4. Hozzon létre egy Azure felhőalapú szolgáltatás, amely a vegyes egyesítéses szolgáltatás fogja tartalmazni.  Nyissa meg toohello Azure portálon. Hello bal oldali sávon kattintson **új**, majd **számítási**, **Felhőszolgáltatás**, és **létrehozása**. 

## <a name="configure-your-split-merge-service"></a>A felosztott egyesítéses szolgáltatás konfigurálása
### <a name="split-merge-service-configuration"></a>Vegyes egyesítéses szolgáltatás konfigurációja
1. Hello mappát, ahova letöltötte hello vegyes egyesítéses szerelvényeket, hozzon létre hello másolatát **ServiceConfiguration.Template.cscfg** fájl mellett szállított **SplitMergeService.cspkg** és adjon neki **ServiceConfiguration.cscfg**.
2. Nyissa meg **ServiceConfiguration.cscfg** egy szövegszerkesztőben, például a Visual Studio, amely a bemeneti adatok, például a tanúsítvány-ujjlenyomatok hello formátuma.
3. Hozzon létre egy új adatbázist, vagy válasszon ki egy létező adatbázis tooserve hello állapot adatbázis vegyes-Merge műveletek legyen, és az adatbázis hello kapcsolat-karakterlánc beolvasása. 
   
   > [!IMPORTANT]
   > Jelenleg állapot-adatbázis hello hello Latin rendezést kell használnia (SQL\_Latin1\_általános\_CP1\_CI\_AS). További információkért lásd: [Windows Collation Name (Transact-SQL)](https://msdn.microsoft.com/library/ms188046.aspx).
   >

   Az Azure SQL Database hello kapcsolati karakterlánc általában: hello űrlap:
      ```
      Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30
      ```

4. Adja meg a kapcsolati karakterlánc hello cscfg-fájl a mindkét hello **SplitMergeWeb** és **SplitMergeWorker** hello ElasticScaleMetadata beállítás szerepkör részei.
5. A hello **SplitMergeWorker** szerepkör, adjon meg egy érvényes kapcsolati karakterlánc tooAzure tárolási hello **WorkerRoleSynchronizationStorageAccountConnectionString** beállítást.

### <a name="configure-security"></a>Biztonság konfigurálása
Részletes utasítások tooconfigure hello biztonsági hello szolgáltatást, tekintse meg a toohello [vegyes egyesítéses biztonsági konfiguráció](sql-database-elastic-scale-split-merge-security-configuration.md).

Ebben az oktatóanyagban egy egyszerű próbatelepítés hello céljából a minimális konfigurációs készlet vonatkozik, lépés végre tooget hello szolgáltatás lépéseivel. Ezeket a lépéseket csak hello egy számítógép vagy fiók engedélyezése végrehajtása őket toocommunicate hello szolgáltatásban.

### <a name="create-a-self-signed-certificate"></a>Önaláírt tanúsítvány létrehozása
Hozzon létre egy új könyvtárat és a könyvtár végrehajtás hello a következő parancs használatával egy [Visual Studio Developer-parancssorból](http://msdn.microsoft.com/library/ms229859.aspx) ablakban:

   ```
    makecert ^
    -n "CN=*.cloudapp.net" ^
    -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.2" ^
    -a sha1 -len 2048 ^
    -sr currentuser -ss root ^
    -sv MyCert.pvk MyCert.cer
   ```

A jelszó tooprotect hello titkos kulcs kell adnia. Adjon meg egy erős jelszót, és erősítse meg. Majd kéri hello jelszó toobe még egyszer ezt követően használható. Kattintson a **Igen** : hello end tooimport azt toohello megbízható hitelesítésszolgáltató hatóságok gyökérszintű tárolóban.

### <a name="create-a-pfx-file"></a>A PFX-fájl létrehozása
Hajtható végre a következő parancsot a hello hello ugyanazon ablakra, ahol makecert végre lett hajtva; használja az Ön használt toocreate hello tanúsítvány hello ugyanazt a jelszót:

    pvk2pfx -pvk MyCert.pvk -spc MyCert.cer -pfx MyCert.pfx -pi <password>

### <a name="import-hello-client-certificate-into-hello-personal-store"></a>Hello személyes tárolóba hello ügyféltanúsítvány importálása
1. A Windows Intézőt, kattintson duplán a **MyCert.pfx**.
2. A hello **Tanúsítványimportáló varázsló** válasszon **aktuális felhasználó** kattintson **következő**.
3. Ellenőrizze a hello fájl elérési útját, majd kattintson **következő**.
4. Írja be a hello jelszót, hagyja **tartalmazza az összes kiterjesztett tulajdonságok** be van jelölve, és kattintson a **tovább**.
5. Hagyja **automatikusan választ hello tanúsítványtároló [...]**  be van jelölve, és kattintson a **következő**.
6. Kattintson a **Befejezés** és **OK**.

### <a name="upload-hello-pfx-file-toohello-cloud-service"></a>Hello PFX fájl toohello felhőszolgáltatás feltöltése
1. Nyissa meg toohello [Azure Portal](https://portal.azure.com).
2. Válassza ki **a felhőalapú szolgáltatások**.
3. Válassza ki a fent hello vegyes/egyesítés szolgáltatás számára létrehozott hello felhőalapú szolgáltatás.
4. Kattintson a **tanúsítványok** hello felső menüben.
5. Kattintson a **feltöltése** a hello alsó sáv.
6. Válassza ki a hello PFX-fájlt, és írja be a hello a fenti ugyanazt a jelszót.
7. Ezt követően másolása hello új bejegyzést hello lista hello tanúsítvány ujjlenyomata.

### <a name="update-hello-service-configuration-file"></a>Hello szolgáltatás konfigurációs fájljának frissítése
Illessze be a fenti másol hello ujjlenyomat-érték attribútum ezeket a beállításokat hello tanúsítvány ujjlenyomata.
A feldolgozói szerepkör hello:
   ```
    <Setting name="DataEncryptionPrimaryCertificateThumbprint" value="" />
    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />
   ```

Hello webes szerepkör:

   ```
    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />
    <Setting name="AllowedClientCertificateThumbprints" value="" />
    <Setting name="DataEncryptionPrimaryCertificateThumbprint" value="" />
    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />
    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />
    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />
   ```

Kérjük, vegye figyelembe, hogy az üzemi környezetek külön tanúsítványokat kell használni hello hitelesítésszolgáltató, a titkosításhoz, a kiszolgálói tanúsítvány és az ügyféltanúsítványok hello. Ez a részletes utasításokért lásd: [biztonsági konfiguráció](sql-database-elastic-scale-split-merge-security-configuration.md).

## <a name="deploy-your-service"></a>A szolgáltatás üzembe helyezése
1. Nyissa meg toohello [Azure-portálon](https://manage.windowsazure.com).
2. Kattintson a hello **Felhőszolgáltatások** hello bal oldali lapon, és válassza ki a korábban létrehozott hello felhőalapú szolgáltatás.
3. Kattintson a **irányítópult**.
4. Átmeneti környezet hello válasszon, majd kattintson a **töltse fel az új átmeneti üzembe helyezésének**.
   
   ![Átmeneti][3]
5. A hello párbeszédpanelen adjon meg egy üzemelő példány címkéje. "Csomag", mind a "Konfiguráció" részen kattintson "A helyi", és válassza ki a hello **SplitMergeService.cspkg** fájl- és a .cscfg fájlban korábban megadott értékektől.
6. Győződjön meg arról, hogy hello feliratú jelölőnégyzet **üzembe helyezés akkor is, ha egy vagy több szerepkör egyetlen példányt tartalmaz** be van jelölve.
7. Találati hello osztásjelek gomb hello alsó jobb toobegin hello telepítésben. Tehát az tootake néhány perc toocomplete.

   ![Feltöltés][4]

## <a name="troubleshoot-hello-deployment"></a>Hello telepítési hibáinak elhárítása
A webes szerepkör toocome online nem sikerül, esetén valószínűleg probléma hello biztonsági beállításaival. Ellenőrizze, hogy hello SSL van konfigurálva a fent leírt módon.

A feldolgozói szerepkör toocome online meghiúsul, de a webes szerepkör sikeres, esetén nagy valószínűséggel egy korábban létrehozott toohello állapot adatbázis kapcsolódás hibája.

* Győződjön meg arról, hogy a .cscfg hello kapcsolati karakterlánc pontos.
* Ellenőrizze, hogy hello server és adatbázis létezik, és hello felhasználóazonosító és jelszó helyességét.
* Az Azure SQL Database Szolgáltatásnak hello kapcsolati karakterlánc kötelező forma: hello:

   ```  
   Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30
   ```

* Győződjön meg arról, hogy hello kiszolgáló neve nem kezdődhet **https://**.
* Győződjön meg arról, hogy az Azure SQL Database-kiszolgáló lehetővé teszi, hogy Azure Services tooconnect tooit. toodo, nyissa meg a https://manage.windowsazure.com, balra kattintson "SQL-adatbázisok" hello, hello tetején kattintson a "Kiszolgáló", és jelölje ki a kiszolgálót. Kattintson a **konfigurálása** : hello top, és győződjön meg arról, hogy hello **Azure Services** beállítás értéke túl "Yes". (Lásd: hello Előfeltételek szakasz ebben a cikkben hello tetején).

## <a name="test-hello-service-deployment"></a>Hello szolgáltatás központi telepítés tesztelése
### <a name="connect-with-a-web-browser"></a>Egy webböngészővel rendelkező csatlakozás
Határozza meg, hogy a vegyes egyesítéses szolgáltatás hello webes végpontjának. Ez az található hello klasszikus Azure portálon is toohello által **irányítópult** a felhőszolgáltatás és a keresése **webhely URL-címe** hello jobb oldalán. Cserélje le **http://** rendelkező **https://** óta hello alapértelmezett biztonsági beállítások letiltása hello HTTP-végpont. A böngészőbe az URL-cím hello oldal betöltése.

### <a name="test-with-powershell-scripts"></a>A PowerShell-parancsfájlok tesztelése
hello telepítési és a környezet által tartalmazott hello minta PowerShell-parancsfájlok futtatásakor tesztelhető.

tartalmazott hello parancsprogramok a következők:

1. **SetupSampleSplitMergeEnvironment.ps1** -állítja be a teszt adatrétegbeli vegyes/egyesítés (lásd az alábbi táblázatban részletes leírása)
2. **ExecuteSampleSplitMerge.ps1** -hello teszt teszt műveleteket hajt végre adatok réteg (lásd az alábbi táblázatban részletes leírása)
3. **GetMappings.ps1** – legfelső szintű mintaparancsfájl hello shard hozzárendelések hello aktuális állapotát megjeleníti.
4. **ShardManagement.psm1** -segítő parancsfájl nagyságúra hello ShardManagement API
5. **SqlDatabaseHelpers.psm1** -segítő parancsfájl létrehozásához és kezeléséhez az SQL-adatbázisok
   
   <table style="width:100%">
     <tr>
       <th>PowerShell-fájl</th>
       <th>Lépések</th>
     </tr>
     <tr>
       <th rowspan="5">SetupSampleSplitMergeEnvironment.ps1</th>
       <td>1.    Létrehoz egy shard térkép manager adatbázis</td>
     </tr>
     <tr>
       <td>2.    2 shard-adatbázisokat hoz létre.
     </tr>
     <tr>
       <td>3.    Ezen adatbázis (törli ezeket az adatbázisokat a maps bármely létező szilánkok) shard leképezést hoz létre. </td>
     </tr>
     <tr>
       <td>4.    Táblát hoz létre egy kis minta mindkét hello szilánkok, és feltölti a hello tábla hello szilánkok egyikében.</td>
     </tr>
     <tr>
       <td>5.    Deklarálja hello SchemaInfo hello szilánkos táblához.</td>
     </tr>
   </table>
   <table style="width:100%">
     <tr>
       <th>PowerShell-fájl</th>
       <th>Lépések</th>
     </tr>
   <tr>
       <th rowspan="4">ExecuteSampleSplitMerge.ps1 </th>
       <td>1.    Vegyes kérelem toohello vegyes egyesítéses szolgáltatás webes időtúllépést, amely felosztja a hello első shard toohello második shard fele hello adatokat küld.</td>
     </tr>
     <tr>
       <td>2.    Szavazások hello webes előtér a hello ossza fel a kérelem állapotának és vár csak hello kérelem befejeződése után.</td>
     </tr>
     <tr>
       <td>3.    Egyesítési kérelem toohello vegyes egyesítéses szolgáltatás webes időtúllépést, amely hello második shard hátsó toohello első shard helyez hello adatokat küld.</td>
     </tr>
     <tr>
       <td>4.    Szavazások hello webes előtér hello egyesítési kérelem állapotának és megvárja, amíg hello kérelem befejeződött.</td>
     </tr>
   </table>
   
## <a name="use-powershell-tooverify-your-deployment"></a>A központi telepítés PowerShell tooverify használata
1. Új PowerShell ablakban keresse meg a toohello könyvtárat, amelybe letöltötte hello vegyes egyesítéses csomag és hello "powershell" könyvtárba, majd lépjen.
2. Hozzon létre egy Azure SQL adatbázis-kiszolgáló (vagy válasszon egy meglévő kiszolgálót) ahol hello shard térkép manager és a szilánkok létrejön.
   
   > [!NOTE]
   > hello SetupSampleSplitMergeEnvironment.ps1 parancsfájl ezeket az adatbázisokat hoz létre hello alapértelmezett tookeep hello parancsfájl egyszerű ugyanarra a kiszolgálóra. Ez a korlátozás nem hello vegyes egyesítéses szolgáltatás a saját magát.
   >
   
   Egy SQL hitelesítési-bejelentkezési az olvasási/írási hozzáférést toohello adatbázisok hello vegyes egyesítéses toomove szolgáltatásadatok és frissítési hello shard leképezés szükséges. Hello felhő hello vegyes egyesítéses szolgáltatás fut, mert azt jelenleg nem támogatja integrált hitelesítést.
   
   Ellenőrizze, hogy hello Azure SQL-kiszolgálót tooallow a hozzáférést a hello számítógépen, amelyen ezek a parancsfájlok hello IP-címét. Ez a beállítás hello Azure SQL-kiszolgálón található / configuration / engedélyezett IP-címeket.
3. Hello SetupSampleSplitMergeEnvironment.ps1 toocreate hello minta parancsfájlkörnyezetet hajtható végre.
   
   A parancsfájl futtatása lesz kitöröl shard térkép meglévő felügyeleti adatokat hello shard manager adatbázist és hello szilánkok struktúrákat. Ha toore inicializálásának hello shard térkép vagy szilánkok lehet hasznos toorerun hello parancsfájl.
   
   Minta parancssor:

   ```   
     .\SetupSampleSplitMergeEnvironment.ps1 
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net'
   ```      
4. Végrehajtás hello Getmappings.ps1 parancsfájl tooview hello hozzárendelések jelenleg létező hello minta környezetben.
   
   ```
     .\GetMappings.ps1 
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net'

   ```         
5. Hello ExecuteSampleSplitMerge.ps1 parancsfájl tooexecute hajtható végre, egy vegyes művelet (hello első shard toohello második shard helyezze át fele hello adatokat), majd egy partícióegyesítési művelet (adatmozgatás hello vissza alakzatot hello első szilánkok). Ha konfigurálta az SSL és a bal oldali hello http-végpont le van tiltva, győződjön meg arról, hello https:// végpont helyette használja.
   
   Minta parancssor:

   ```   
     .\ExecuteSampleSplitMerge.ps1
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net' 
         -SplitMergeServiceEndpoint 'https://mysplitmergeservice.cloudapp.net' 
         -CertificateThumbprint '0123456789abcdef0123456789abcdef01234567'
   ```      
   
   Hiba alább hello kapja, esetén valószínűleg a webalkalmazás-végpontot tanúsítványával kapcsolatos problémára. Próbáljon meg toohello webes végpontjának a kedvenc webböngészőt, és ellenőrizze, hogy a tanúsítvány hiba.
   
     ```
     Invoke-WebRequest : hello underlying connection was closed: Could not establish trust relationship for hello SSL/TLSsecure channel.
     ```
   
   Ha sikeres volt, hello kimeneti alábbi hello hasonlóan kell kinéznie:
   
   ```
   > .\ExecuteSampleSplitMerge.ps1 -UserName 'mysqluser' -Password 'MySqlPassw0rd' -ShardMapManagerServerName 'abcdefghij.database.windows.net' -SplitMergeServiceEndpoint 'http://mysplitmergeservice.cloudapp.net' -CertificateThumbprint 0123456789abcdef0123456789abcdef01234567
   > Sending split request
   > Began split operation with id dc68dfa0-e22b-4823-886a-9bdc903c80f3
   > Polling split-merge request status. Press Ctrl-C tooend
   > Progress: 0% | Status: Queued | Details: [Informational] Queued request
   > Progress: 5% | Status: Starting | Details: [Informational] Starting split-merge state machine for request.
   > Progress: 5% | Status: Starting | Details: [Informational] Performing data consistency checks on target     shards.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source tootarget shard.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Waiting for reference tables copy     completion.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source tootarget shard.
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Moving key range [100:110) of     Sharded tables
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Successfully copied key range     [100:110) for table [dbo].[MyShardedTable]
   > ...
   > ...
   > Progress: 90% | Status: Completing | Details: [Informational] Successfully deleted shardlets in table     [dbo].[MyShardedTable].
   > Progress: 90% | Status: Completing | Details: [Informational] Deleting any temp tables that were created     while processing hello request.
   > Progress: 100% | Status: Succeeded | Details: [Informational] Successfully processed request.
   > Sending merge request
   > Began merge operation with id 6ffc308f-d006-466b-b24e-857242ec5f66
   > Polling request status. Press Ctrl-C tooend
   > Progress: 0% | Status: Queued | Details: [Informational] Queued request
   > Progress: 5% | Status: Starting | Details: [Informational] Starting split-merge state machine for request.
   > Progress: 5% | Status: Starting | Details: [Informational] Performing data consistency checks on target     shards.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source tootarget shard.
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Moving key range [100:110) of     Sharded tables
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Successfully copied key range     [100:110) for table [dbo].[MyShardedTable]
   > ...
   > ...
   > Progress: 90% | Status: Completing | Details: [Informational] Successfully deleted shardlets in table     [dbo].[MyShardedTable].
   > Progress: 90% | Status: Completing | Details: [Informational] Deleting any temp tables that were created     while processing hello request.
   > Progress: 100% | Status: Succeeded | Details: [Informational] Successfully processed request.
   > 
   ```
6. Más adattípusok kísérletezhet! Mindegyik parancsfájl vennie a választható - ShardKeyType paraméter, amely lehetővé teszi a toospecify hello kulcs típusa. hello alapértelmezett Int32, de azt is megadhatja Int64, Guid vagy bináris.

## <a name="create-requests"></a>Létrehozási kérelmek
hello szolgáltatás használható hello webes felhasználói felület használatával vagy importálni és hello SplitMerge.psm1 PowerShell-modult, amely a kéréseket keresztül hello webes szerepkör használatával.

hello szolgáltatás mozgathatja az adatokat a szilánkos táblák és a hivatkozási táblák. A szilánkos táblához egy horizontális skálázási kulcsoszlopa, és különböző soradatok rendelkezzen minden szilánkcímtárban. Egy hivatkozási tábla nincs szilánkos hello tartalmaz, ugyanazt a sort adatok a minden szilánkcímtárban. Hivatkozási táblák hasznosak, amely nem változik gyakran, és a lekérdezésekben szilánkos táblákkal használt tooJOIN adatokat.

A sorrend tooperform vegyes egyesítésével hello szilánkos táblák és hivatkozási táblák toohave áthelyezni kívánt kell deklarálnia. Mindez a hello **SchemaInfo** API. Ez az API van hello **Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.Schema** névtér.

1. Minden szilánkos táblához, hozzon létre egy **ShardedTableInfo** hello tábla szülő sémanév leíró objektum (nem kötelező, alapértelmezés szerint használt érték túl "dbo"), táblanév hello és hello oszlopnév hello horizontális kulcsot tartalmazó tábla.
2. Minden egyes összefoglaló táblázatot is létrehozhat egy **ReferenceTableInfo** hello tábla szülő sémanév leíró objektum (nem kötelező, alapértelmezés szerint használt érték túl "dbo") és hello tábla neve.
3. Adja hozzá a fenti TableInfo objektumok tooa új hello **SchemaInfo** objektum.
4. Egy hivatkozási tooa beolvasása **ShardMapManager** objektum és hívás **GetSchemaInfoCollection**.
5. Adja hozzá a hello **SchemaInfo** toohello **SchemaInfoCollection**, hello shard leképezésnév biztosítása.

Például az hello SetupSampleSplitMergeEnvironment.ps1 parancsfájl látható.

hello vegyes egyesítéses szolgáltatás nem hello céladatbázis (vagy bármely hello adatbázis tábláit sémáját) hozza létre. Fel kell előre létrehozott egy kérést toohello szolgáltatás elküldése előtt.

## <a name="troubleshooting"></a>Hibaelhárítás
Hello minta powershell-parancsfájlok futtatásakor alatt üzenet hello jelenhetnek meg:

   ```
   Invoke-WebRequest : hello underlying connection was closed: Could not establish trust relationship for hello SSL/TLS secure channel.
   ```

Ez a hiba azt jelenti, hogy az SSL-tanúsítvány nincs megfelelően konfigurálva. Kérjük, kövesse a részben található útmutatást hello "Böngészővel csatlakozás".

Nem küldenek kéréseket jelenhet meg ezt:

```
[Exception] System.Data.SqlClient.SqlException (0x80131904): Could not find stored procedure 'dbo.InsertRequest'. 
```

Ebben az esetben ellenőrizze a konfigurációs fájlban, az adott hello beállítása **WorkerRoleSynchronizationStorageAccountConnectionString**. Ez a hiba általában azt jelzi, hogy hello feldolgozói szerepkör sikeresen inicializálása hello metaadatokat tároló adatbázis első használatkor. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/allowed-services.png
[2]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/manage.png
[3]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/staging.png
[4]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/upload.png
[5]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/storage.png

