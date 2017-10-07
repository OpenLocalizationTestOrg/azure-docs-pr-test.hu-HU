---
title: "aaaBack be az alkalmazást az Azure-ban"
description: "Megtudhatja, hogyan az alkalmazások az Azure App Service toocreate biztonsági másolatait."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: 6223b6bd-84ec-48df-943f-461d84605694
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.openlocfilehash: e41d93d322bbc48b45b28eeaa817928d83c2b9d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-your-app-in-azure"></a>Adatok biztonsági mentése az Azure-ban
hello biztonsági mentése és visszaállítása a szolgáltatással [Azure App Service](../app-service/app-service-value-prop-what-is.md) lehetővé teszi, hogy könnyen hozzanak létre alkalmazás biztonsági mentést, manuálisan vagy ütemezés szerint. Felülírása hello meglévő alkalmazás vagy a visszaállítási tooanother alkalmazás visszaállításához hello app tooa pillanatképe korábbi állapotába. 

Az alkalmazás biztonsági másolatból történő visszaállítását információkért lásd: [visszaállítása egy alkalmazást az Azure-ban](web-sites-restore.md).

<a name="whatsbackedup"></a>

## <a name="what-gets-backed-up"></a>Mi a biztonsági mentés beolvasása
App Service készíthet biztonsági másolatot a hello következő információk tooan Azure storage-fiókok és az alkalmazás toouse konfigurált tároló. 

* Alkalmazáskonfiguráció
* A fájl
* Adatbázis csatlakoztatott tooyour alkalmazás

biztonsági másolat szolgáltatás a következő adatbázis megoldások hello támogatottak: 
   - [SQL Database](https://azure.microsoft.com/en-us/services/sql-database/)
   - [A MySQL (előzetes verzió) Azure-adatbázis](https://azure.microsoft.com/en-us/services/mysql)
   - [Azure-adatbázis PostgreSQL (előzetes verzió)](https://azure.microsoft.com/en-us/services/postgres)
   - [ClearDB MySQL](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SuccessBricksInc.ClearDBMySQLDatabase?tab=Overview)
   - [MySQL alkalmazásbeli](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)
 

> [!NOTE]
>  Minden biztonsági mentés az alkalmazás nem növekményes frissítés offline teljes másolata.
>  

<a name="requirements"></a>

## <a name="requirements-and-restrictions"></a>Követelmények és korlátozások
* Készítsen biztonsági másolatot hello és visszaállítási funkció használatához az alkalmazásszolgáltatási csomag toobe a hello hello **szabványos** réteg vagy **prémium** réteg. Az App Service-csomag toouse magasabb szintű használható méretezésével kapcsolatos további információkért lásd: [vertikális felskálázás az Azure alkalmazásban](web-sites-scale.md).  
  **Prémium szintű** réteg lehetővé teszi, hogy a napi nagyobb számú biztonsági ups mint **szabványos** réteg.
* Egy Azure storage-fiók és a tároló az hello van szükség, amelyet az toobackup hello alkalmazásként ugyanahhoz az előfizetéshez. Az Azure storage-fiókokról további információkért lásd: hello [hivatkozások](#moreaboutstorage) hello Ez a cikk végén.
* Biztonsági mentések be az alkalmazás- és tartalom too10 GB lehet. Hello biztonsági másolatának mérete túllépi ezt a korlátozást, ha hibaüzenetet kap.

<a name="manualbackup"></a>

## <a name="create-a-manual-backup"></a>Manuális biztonsági mentés létrehozása
1. A hello [Azure Portal](https://portal.azure.com)tooyour alkalmazás paneljén lépjen, válassza ki **biztonsági mentések**. Hello **biztonsági mentések** panel fog megjelenni.
   
    ![Biztonsági mentések lap][ChooseBackupsPage]
   
   > [!NOTE]
   > Ha az alábbi hello üzenet jelenik meg, kattintson rá az App Service-csomag előtt lépne tooupgrade biztonsági.
   > Lásd: [vertikális felskálázás az Azure alkalmazásban](web-sites-scale.md) további információt.  
   > ![Válassza ki a tárfiók](./media/web-sites-backup/01UpgradePlan1.png)
   > 
   > 

2. A hello **biztonsági mentés** paneljén kattintson **konfigurálása**
![kattintson konfigurálása](./media/web-sites-backup/ClickConfigure1.png)
3. A hello **biztonsági mentési konfigurációhoz** panelen kattintson a **tárolási: nincs konfigurálva** tooconfigure egy tárfiókot.
   
    ![Válassza ki a tárfiók][ChooseStorageAccount]
4. A biztonsági mentés célhelyének megadásához jelöljön ki egy **Tárfiók** és **tároló**. hello tárfiókot kell tartozniuk toohello tooback akarja hello alkalmazásként ugyanahhoz az előfizetéshez. Ha kívánja, létrehozhat egy új tárfiókot vagy egy új tároló hello megfelelő panelt a. Amikor elkészült, kattintson a **válasszon**.
   
    ![Válassza ki a tárfiók](./media/web-sites-backup/02ChooseStorageAccount1-1.png)
5. A hello **biztonsági mentési konfigurációhoz** még mindig nyitva marad panelen beállíthatja **adatbázis biztonsági másolata**, majd válassza ki a hello adatbázisokat szeretné, hogy tooinclude hello biztonsági mentései (SQL-adatbázis vagy MySQL), majd kattintson a **OK**.  
   
    ![Válassza ki a tárfiók](./media/web-sites-backup/03ConfigureDatabase1.png)
   
   > [!NOTE]
   > Egy adatbázis tooappear ezen a listán, a kapcsolati karakterláncában szerepelnie kell hello **kapcsolati karakterláncok** hello szakasza **Alkalmazásbeállítások** alkalmazás.
   > 
   > 
6. A hello **biztonsági mentési konfigurációhoz** panelen kattintson a **mentése**.    
7. A hello **biztonsági mentések** panelen kattintson a **biztonsági mentés**.
   
    ![BackUpNow gomb][BackUpNow]
   
    Megjelenik egy folyamatban lévő üzenet hello biztonsági mentési folyamat során.

Miután beállította hello tárfiók és tároló manuális biztonsági mentés bármikor kezdeményezhető.  

<a name="automatedbackups"></a>

## <a name="configure-automated-backups"></a>Az automatikus biztonsági mentések konfigurálása
1. A hello **biztonsági mentési konfigurációhoz** panelen állítsa **ütemezett biztonsági mentés** túl**a**. 
   
    ![Válassza ki a tárfiók](./media/web-sites-backup/05ScheduleBackup1.png)
2. Beállítások megjelenik, biztonsági mentési ütemezés beállítása **ütemezett biztonsági mentési** túl**a**, majd konfigurálja a biztonsági mentés ütemezése hello tetszés szerint, és kattintson a **OK**.
   
    ![Az automatikus biztonsági mentés engedélyezése][SetAutomatedBackupOn]

<a name="partialbackups"></a>

## <a name="configure-partial-backups"></a>Részleges biztonsági mentések konfigurálása
Néha nem szeretné, hogy toobackup mindent az alkalmazásnak. Íme, néhány példa:

* Ön [beállítása heti biztonsági mentései](web-sites-backup.md#configure-automated-backups) az alkalmazás, amely tartalmazza a statikus tartalom, amely soha nem változik, például a régi blogbejegyzések vagy képeket.
* Az alkalmazás még több mint 10 GB-tartalmat (hello maximális mennyiség készíthet biztonsági másolatot egy időben).
* Nem szeretné, hogy toobackup hello naplófájlokat.

Részleges biztonsági másolatok lehetővé teszi, hogy úgy dönt, hogy pontosan amely fájlokat szeretné, hogy toobackup.

### <a name="exclude-files-from-your-backup"></a>Fájlok kizárása a biztonsági mentés
Tegyük fel, amely tartalmazza a naplófájlok és a biztonsági mentési egyszer és nem fog toochange statikus képek alkalmazás. Ilyen esetekben kizárhatja azokat a fájlokat és mappákat a jövőbeni biztonsági mentések tárolják. tooexclude fájlokat és mappákat a biztonsági másolatból, hozzon létre egy `_backup.filter` hello fájlban `D:\home\site\wwwroot` az alkalmazás mappájában. Fájlok és mappák azt szeretné, hogy a fájl tooexclude hello listáját adja meg. 

Egy egyszerű módot tooaccess a fájlok toouse Kudu. Kattintson a **speciális eszközök -> Ugrás** a webes alkalmazás tooaccess Kudu beállítása.

![A kudu portál használatával][kudu-portal]

Azonosítsa, hogy a kívánt tooexclude a biztonsági másolatok hello mappákat.  Például azt szeretné, toofilter hello kijelölt mappa és a fájlokat.

![Képek mappához][ImagesFolder]

Hozzon létre egy nevű fájlt `_backup.filter` és a fenti hello lista be hello fájlt, de eltávolítása `D:\home`. Egy soronként fájl vagy könyvtár felsorolása. Ezért hello fájl tartalma hello kell:
 ```bash
    \site\wwwroot\Images\brand.png
    \site\wwwroot\Images\2014
    \site\wwwroot\Images\2013
```

Töltse fel `_backup.filter` toohello fájl `D:\home\site\wwwroot\` mappában található a webhely használatával [ftp](web-sites-deploy.md#ftp) vagy más módszerrel. Ha kívánja, létrehozhat hello fájl közvetlenül a Kudu `DebugConsole` és szúrja be a hiba a hello tartalmat.

Futtatási biztonsági mentések hello azonos módon szokásos módon teheti meg, [manuálisan](#create-a-manual-backup) vagy [automatikusan](#configure-automated-backups). Most, bármely fájlok és mappák megadott `_backup.filter` hello jövőbeli biztonsági mentések ütemezett, vagy manuálisan indítják el ki van zárva. 

> [!NOTE]
> A hely hello részleges biztonsági másolatainak visszaállítása módon [a rendszeres biztonsági másolat visszaállításával](web-sites-restore.md). hello visszaállítási folyamat hello jobb oldali dolgot tegyenek.
> 
> Ha teljes biztonsági mentés helyreállítása, hello hely összes tartalmat helyére függetlenül hello biztonsági mentés van. Ha egy fájl hello helyen, de nem hello biztonsági mentés az lekérdezi törlődni fog. De részleges biztonsági másolat visszaállításakor egyik feketelistára teszi hello könyvtárban, vagy bármely Feketelistára tett fájlban található tartalmakhoz marad, mert a.
> 


<a name="aboutbackups"></a>

## <a name="how-backups-are-stored"></a>Biztonsági másolatok tárolási módját
Egy vagy több biztonsági mentés az alkalmazás elkészítése után hello biztonsági másolatok jelennek meg hello **tárolók** panelen található a tárfiók, és az alkalmazás. Hello tárfiókot, minden egyes biztonsági másolat tartalmaz egy`.zip` hello biztonsági mentési adatokat tartalmazó fájlt, és egy `.xml` hello jegyzékfájl tartalmazó fájl `.zip` fájl tartalmát. Csomagolja ki, és keresse meg ezeket a fájlokat, ha azt szeretné, tooaccess a biztonsági másolatok egy alkalmazás-visszaállítási végrehajtása nélkül.

hello az adatbázis biztonsági mentése hello alkalmazás hello legfelső szintű the.zip fájl tárolja. SQL-adatbázis Ez egy BACPAC-fájl (nincs fájl kiterjesztése) és importálhatók. toocreate a SQL-adatbázis BACPAC exportálási hello alapján című [importálni egy új felhasználói adatbázis BACPAC fájl tooCreate](http://technet.microsoft.com/library/hh710052.aspx).

> [!WARNING]
> Hello fájlokat a módosítása a **websitebackups** tároló hello biztonsági mentési toobecome érvénytelen, és ezért nem visszaállítható okozhat.
> 
> 

<a name="nextsteps"></a>

## <a name="next-steps"></a>Következő lépések
A visszaállítása egy alkalmazás olyan biztonsági információ: [visszaállítása egy alkalmazást az Azure-ban](web-sites-restore.md). Is biztonsági mentése és visszaállítása a REST API használatával App Service apps (lásd: [használata REST toobackup és visszaállítási App Service apps](websites-csm-backup.md)).


<!-- IMAGES -->
[ChooseBackupsPage]: ./media/web-sites-backup/01ChooseBackupsPage1.png
[ChooseStorageAccount]: ./media/web-sites-backup/02ChooseStorageAccount-1.png
[IncludedDatabases]: ./media/web-sites-backup/03IncludedDatabases.png
[BackUpNow]: ./media/web-sites-backup/04BackUpNow1.png
[BackupProgress]: ./media/web-sites-backup/05BackupProgress.png
[SetAutomatedBackupOn]: ./media/web-sites-backup/06SetAutomatedBackupOn1.png
[Frequency]: ./media/web-sites-backup/07Frequency.png
[StartDate]: ./media/web-sites-backup/08StartDate.png
[StartTime]: ./media/web-sites-backup/09StartTime.png
[SaveIcon]: ./media/web-sites-backup/10SaveIcon.png
[ImagesFolder]: ./media/web-sites-backup/11Images.png
[LogsFolder]: ./media/web-sites-backup/12Logs.png
[GhostUpgradeWarning]: ./media/web-sites-backup/13GhostUpgradeWarning.png
[kudu-portal]:./media/web-sites-backup/kudu-portal.PNG

