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
# <a name="back-up-your-app-in-azure"></a><span data-ttu-id="e6525-103">Adatok biztonsági mentése az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="e6525-103">Back up your app in Azure</span></span>
<span data-ttu-id="e6525-104">hello biztonsági mentése és visszaállítása a szolgáltatással [Azure App Service](../app-service/app-service-value-prop-what-is.md) lehetővé teszi, hogy könnyen hozzanak létre alkalmazás biztonsági mentést, manuálisan vagy ütemezés szerint.</span><span class="sxs-lookup"><span data-stu-id="e6525-104">hello Back up and Restore feature in [Azure App Service](../app-service/app-service-value-prop-what-is.md) lets you easily create app backups manually or on a schedule.</span></span> <span data-ttu-id="e6525-105">Felülírása hello meglévő alkalmazás vagy a visszaállítási tooanother alkalmazás visszaállításához hello app tooa pillanatképe korábbi állapotába.</span><span class="sxs-lookup"><span data-stu-id="e6525-105">You can restore hello app tooa snapshot of a previous state by overwriting hello existing app or restoring tooanother app.</span></span> 

<span data-ttu-id="e6525-106">Az alkalmazás biztonsági másolatból történő visszaállítását információkért lásd: [visszaállítása egy alkalmazást az Azure-ban](web-sites-restore.md).</span><span class="sxs-lookup"><span data-stu-id="e6525-106">For information on restoring an app from backup, see [Restore an app in Azure](web-sites-restore.md).</span></span>

<a name="whatsbackedup"></a>

## <a name="what-gets-backed-up"></a><span data-ttu-id="e6525-107">Mi a biztonsági mentés beolvasása</span><span class="sxs-lookup"><span data-stu-id="e6525-107">What gets backed up</span></span>
<span data-ttu-id="e6525-108">App Service készíthet biztonsági másolatot a hello következő információk tooan Azure storage-fiókok és az alkalmazás toouse konfigurált tároló.</span><span class="sxs-lookup"><span data-stu-id="e6525-108">App Service can backup hello following information tooan Azure storage account and container that you have configured your app toouse.</span></span> 

* <span data-ttu-id="e6525-109">Alkalmazáskonfiguráció</span><span class="sxs-lookup"><span data-stu-id="e6525-109">App configuration</span></span>
* <span data-ttu-id="e6525-110">A fájl</span><span class="sxs-lookup"><span data-stu-id="e6525-110">File content</span></span>
* <span data-ttu-id="e6525-111">Adatbázis csatlakoztatott tooyour alkalmazás</span><span class="sxs-lookup"><span data-stu-id="e6525-111">Database connected tooyour app</span></span>

<span data-ttu-id="e6525-112">biztonsági másolat szolgáltatás a következő adatbázis megoldások hello támogatottak:</span><span class="sxs-lookup"><span data-stu-id="e6525-112">hello following database solutions are supported with backup feature:</span></span> 
   - [<span data-ttu-id="e6525-113">SQL Database</span><span class="sxs-lookup"><span data-stu-id="e6525-113">SQL Database</span></span>](https://azure.microsoft.com/en-us/services/sql-database/)
   - [<span data-ttu-id="e6525-114">A MySQL (előzetes verzió) Azure-adatbázis</span><span class="sxs-lookup"><span data-stu-id="e6525-114">Azure Database for MySQL (Preview)</span></span>](https://azure.microsoft.com/en-us/services/mysql)
   - [<span data-ttu-id="e6525-115">Azure-adatbázis PostgreSQL (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="e6525-115">Azure Database for PostgreSQL (Preview)</span></span>](https://azure.microsoft.com/en-us/services/postgres)
   - [<span data-ttu-id="e6525-116">ClearDB MySQL</span><span class="sxs-lookup"><span data-stu-id="e6525-116">ClearDB MySQL</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SuccessBricksInc.ClearDBMySQLDatabase?tab=Overview)
   - [<span data-ttu-id="e6525-117">MySQL alkalmazásbeli</span><span class="sxs-lookup"><span data-stu-id="e6525-117">MySQL in-app</span></span>](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)
 

> [!NOTE]
>  <span data-ttu-id="e6525-118">Minden biztonsági mentés az alkalmazás nem növekményes frissítés offline teljes másolata.</span><span class="sxs-lookup"><span data-stu-id="e6525-118">Each backup is a complete offline copy of your app, not an incremental update.</span></span>
>  

<a name="requirements"></a>

## <a name="requirements-and-restrictions"></a><span data-ttu-id="e6525-119">Követelmények és korlátozások</span><span class="sxs-lookup"><span data-stu-id="e6525-119">Requirements and restrictions</span></span>
* <span data-ttu-id="e6525-120">Készítsen biztonsági másolatot hello és visszaállítási funkció használatához az alkalmazásszolgáltatási csomag toobe a hello hello **szabványos** réteg vagy **prémium** réteg.</span><span class="sxs-lookup"><span data-stu-id="e6525-120">hello Back up and Restore feature requires hello App Service plan toobe in hello **Standard** tier or **Premium** tier.</span></span> <span data-ttu-id="e6525-121">Az App Service-csomag toouse magasabb szintű használható méretezésével kapcsolatos további információkért lásd: [vertikális felskálázás az Azure alkalmazásban](web-sites-scale.md).</span><span class="sxs-lookup"><span data-stu-id="e6525-121">For more information about scaling your App Service plan toouse a higher tier, see [Scale up an app in Azure](web-sites-scale.md).</span></span>  
  <span data-ttu-id="e6525-122">**Prémium szintű** réteg lehetővé teszi, hogy a napi nagyobb számú biztonsági ups mint **szabványos** réteg.</span><span class="sxs-lookup"><span data-stu-id="e6525-122">**Premium** tier allows a greater number of daily back ups than **Standard** tier.</span></span>
* <span data-ttu-id="e6525-123">Egy Azure storage-fiók és a tároló az hello van szükség, amelyet az toobackup hello alkalmazásként ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="e6525-123">You need an Azure storage account and container in hello same subscription as hello app that you want toobackup.</span></span> <span data-ttu-id="e6525-124">Az Azure storage-fiókokról további információkért lásd: hello [hivatkozások](#moreaboutstorage) hello Ez a cikk végén.</span><span class="sxs-lookup"><span data-stu-id="e6525-124">For more information on Azure storage accounts, see hello [links](#moreaboutstorage) at hello end of this article.</span></span>
* <span data-ttu-id="e6525-125">Biztonsági mentések be az alkalmazás- és tartalom too10 GB lehet.</span><span class="sxs-lookup"><span data-stu-id="e6525-125">Backups can be up too10 GB of app and database content.</span></span> <span data-ttu-id="e6525-126">Hello biztonsági másolatának mérete túllépi ezt a korlátozást, ha hibaüzenetet kap.</span><span class="sxs-lookup"><span data-stu-id="e6525-126">If hello backup size exceeds this limit, you get an error .</span></span>

<a name="manualbackup"></a>

## <a name="create-a-manual-backup"></a><span data-ttu-id="e6525-127">Manuális biztonsági mentés létrehozása</span><span class="sxs-lookup"><span data-stu-id="e6525-127">Create a manual backup</span></span>
1. <span data-ttu-id="e6525-128">A hello [Azure Portal](https://portal.azure.com)tooyour alkalmazás paneljén lépjen, válassza ki **biztonsági mentések**.</span><span class="sxs-lookup"><span data-stu-id="e6525-128">In hello [Azure Portal](https://portal.azure.com), navigate tooyour app's blade, select **Backups**.</span></span> <span data-ttu-id="e6525-129">Hello **biztonsági mentések** panel fog megjelenni.</span><span class="sxs-lookup"><span data-stu-id="e6525-129">hello **Backups** blade will be displayed.</span></span>
   
    ![Biztonsági mentések lap][ChooseBackupsPage]
   
   > [!NOTE]
   > <span data-ttu-id="e6525-131">Ha az alábbi hello üzenet jelenik meg, kattintson rá az App Service-csomag előtt lépne tooupgrade biztonsági.</span><span class="sxs-lookup"><span data-stu-id="e6525-131">If you see hello message below, click it tooupgrade your App Service plan before you can proceed with backups.</span></span>
   > <span data-ttu-id="e6525-132">Lásd: [vertikális felskálázás az Azure alkalmazásban](web-sites-scale.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="e6525-132">See [Scale up an app in Azure](web-sites-scale.md) for more information.</span></span>  
   > <span data-ttu-id="e6525-133">![Válassza ki a tárfiók](./media/web-sites-backup/01UpgradePlan1.png)</span><span class="sxs-lookup"><span data-stu-id="e6525-133">![Choose storage account](./media/web-sites-backup/01UpgradePlan1.png)</span></span>
   > 
   > 

2. <span data-ttu-id="e6525-134">A hello **biztonsági mentés** paneljén kattintson **konfigurálása**
![kattintson konfigurálása](./media/web-sites-backup/ClickConfigure1.png)</span><span class="sxs-lookup"><span data-stu-id="e6525-134">In hello **Backup** blade, Click **Configure**
![Click Configure](./media/web-sites-backup/ClickConfigure1.png)</span></span>
3. <span data-ttu-id="e6525-135">A hello **biztonsági mentési konfigurációhoz** panelen kattintson a **tárolási: nincs konfigurálva** tooconfigure egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="e6525-135">In hello **Backup Configuration** blade, click **Storage: Not configured** tooconfigure a storage account.</span></span>
   
    ![Válassza ki a tárfiók][ChooseStorageAccount]
4. <span data-ttu-id="e6525-137">A biztonsági mentés célhelyének megadásához jelöljön ki egy **Tárfiók** és **tároló**.</span><span class="sxs-lookup"><span data-stu-id="e6525-137">Choose your backup destination by selecting a **Storage Account** and **Container**.</span></span> <span data-ttu-id="e6525-138">hello tárfiókot kell tartozniuk toohello tooback akarja hello alkalmazásként ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="e6525-138">hello storage account must belong toohello same subscription as hello app you want tooback up.</span></span> <span data-ttu-id="e6525-139">Ha kívánja, létrehozhat egy új tárfiókot vagy egy új tároló hello megfelelő panelt a.</span><span class="sxs-lookup"><span data-stu-id="e6525-139">If you wish, you can create a new storage account or a new container in hello respective blades.</span></span> <span data-ttu-id="e6525-140">Amikor elkészült, kattintson a **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="e6525-140">When you're done, click **Select**.</span></span>
   
    ![Válassza ki a tárfiók](./media/web-sites-backup/02ChooseStorageAccount1-1.png)
5. <span data-ttu-id="e6525-142">A hello **biztonsági mentési konfigurációhoz** még mindig nyitva marad panelen beállíthatja **adatbázis biztonsági másolata**, majd válassza ki a hello adatbázisokat szeretné, hogy tooinclude hello biztonsági mentései (SQL-adatbázis vagy MySQL), majd kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="e6525-142">In hello **Backup Configuration** blade that is still left open, you can configure **Backup Database**, then select hello databases you want tooinclude in hello backups (SQL database or MySQL), then click **OK**.</span></span>  
   
    ![Válassza ki a tárfiók](./media/web-sites-backup/03ConfigureDatabase1.png)
   
   > [!NOTE]
   > <span data-ttu-id="e6525-144">Egy adatbázis tooappear ezen a listán, a kapcsolati karakterláncában szerepelnie kell hello **kapcsolati karakterláncok** hello szakasza **Alkalmazásbeállítások** alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e6525-144">For a database tooappear in this list, its connection string must exist in hello **Connection strings** section of hello **Application settings** blade for your app.</span></span>
   > 
   > 
6. <span data-ttu-id="e6525-145">A hello **biztonsági mentési konfigurációhoz** panelen kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="e6525-145">In hello **Backup Configuration** blade, click **Save**.</span></span>    
7. <span data-ttu-id="e6525-146">A hello **biztonsági mentések** panelen kattintson a **biztonsági mentés**.</span><span class="sxs-lookup"><span data-stu-id="e6525-146">In hello  **Backups** blade, click **Backup**.</span></span>
   
    ![BackUpNow gomb][BackUpNow]
   
    <span data-ttu-id="e6525-148">Megjelenik egy folyamatban lévő üzenet hello biztonsági mentési folyamat során.</span><span class="sxs-lookup"><span data-stu-id="e6525-148">You see a progress message during hello backup process.</span></span>

<span data-ttu-id="e6525-149">Miután beállította hello tárfiók és tároló manuális biztonsági mentés bármikor kezdeményezhető.</span><span class="sxs-lookup"><span data-stu-id="e6525-149">Once hello storage account and container is configured you can initiate a manual backup at any time.</span></span>  

<a name="automatedbackups"></a>

## <a name="configure-automated-backups"></a><span data-ttu-id="e6525-150">Az automatikus biztonsági mentések konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e6525-150">Configure automated backups</span></span>
1. <span data-ttu-id="e6525-151">A hello **biztonsági mentési konfigurációhoz** panelen állítsa **ütemezett biztonsági mentés** túl**a**.</span><span class="sxs-lookup"><span data-stu-id="e6525-151">In hello **Backup Configuration** blade, set **Scheduled backup** too**On**.</span></span> 
   
    ![Válassza ki a tárfiók](./media/web-sites-backup/05ScheduleBackup1.png)
2. <span data-ttu-id="e6525-153">Beállítások megjelenik, biztonsági mentési ütemezés beállítása **ütemezett biztonsági mentési** túl**a**, majd konfigurálja a biztonsági mentés ütemezése hello tetszés szerint, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="e6525-153">Backup schedule options will show up, set **Scheduled Backup** too**On**, then configure hello backup schedule as desired and click **OK**.</span></span>
   
    ![Az automatikus biztonsági mentés engedélyezése][SetAutomatedBackupOn]

<a name="partialbackups"></a>

## <a name="configure-partial-backups"></a><span data-ttu-id="e6525-155">Részleges biztonsági mentések konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e6525-155">Configure Partial Backups</span></span>
<span data-ttu-id="e6525-156">Néha nem szeretné, hogy toobackup mindent az alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="e6525-156">Sometimes you don't want toobackup everything on your app.</span></span> <span data-ttu-id="e6525-157">Íme, néhány példa:</span><span class="sxs-lookup"><span data-stu-id="e6525-157">Here are a few examples:</span></span>

* <span data-ttu-id="e6525-158">Ön [beállítása heti biztonsági mentései](web-sites-backup.md#configure-automated-backups) az alkalmazás, amely tartalmazza a statikus tartalom, amely soha nem változik, például a régi blogbejegyzések vagy képeket.</span><span class="sxs-lookup"><span data-stu-id="e6525-158">You [set up weekly backups](web-sites-backup.md#configure-automated-backups) of your app that contains static content that never changes, such as old blog posts or images.</span></span>
* <span data-ttu-id="e6525-159">Az alkalmazás még több mint 10 GB-tartalmat (hello maximális mennyiség készíthet biztonsági másolatot egy időben).</span><span class="sxs-lookup"><span data-stu-id="e6525-159">Your app has over 10 GB of content (that's hello max amount you can backup at a time).</span></span>
* <span data-ttu-id="e6525-160">Nem szeretné, hogy toobackup hello naplófájlokat.</span><span class="sxs-lookup"><span data-stu-id="e6525-160">You don't want toobackup hello log files.</span></span>

<span data-ttu-id="e6525-161">Részleges biztonsági másolatok lehetővé teszi, hogy úgy dönt, hogy pontosan amely fájlokat szeretné, hogy toobackup.</span><span class="sxs-lookup"><span data-stu-id="e6525-161">Partial backups allows you choose exactly which files you want toobackup.</span></span>

### <a name="exclude-files-from-your-backup"></a><span data-ttu-id="e6525-162">Fájlok kizárása a biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="e6525-162">Exclude files from your backup</span></span>
<span data-ttu-id="e6525-163">Tegyük fel, amely tartalmazza a naplófájlok és a biztonsági mentési egyszer és nem fog toochange statikus képek alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e6525-163">Suppose you have an app that contains log files and static images that have been backup once and are not going toochange.</span></span> <span data-ttu-id="e6525-164">Ilyen esetekben kizárhatja azokat a fájlokat és mappákat a jövőbeni biztonsági mentések tárolják.</span><span class="sxs-lookup"><span data-stu-id="e6525-164">In such cases you can exclude those folders and files from being stored in your future backups.</span></span> <span data-ttu-id="e6525-165">tooexclude fájlokat és mappákat a biztonsági másolatból, hozzon létre egy `_backup.filter` hello fájlban `D:\home\site\wwwroot` az alkalmazás mappájában.</span><span class="sxs-lookup"><span data-stu-id="e6525-165">tooexclude files and folders from your backups, create a `_backup.filter` file in hello `D:\home\site\wwwroot` folder of your app.</span></span> <span data-ttu-id="e6525-166">Fájlok és mappák azt szeretné, hogy a fájl tooexclude hello listáját adja meg.</span><span class="sxs-lookup"><span data-stu-id="e6525-166">Specify hello list of files and folders you want tooexclude in this file.</span></span> 

<span data-ttu-id="e6525-167">Egy egyszerű módot tooaccess a fájlok toouse Kudu.</span><span class="sxs-lookup"><span data-stu-id="e6525-167">An easy way tooaccess your files is toouse Kudu .</span></span> <span data-ttu-id="e6525-168">Kattintson a **speciális eszközök -> Ugrás** a webes alkalmazás tooaccess Kudu beállítása.</span><span class="sxs-lookup"><span data-stu-id="e6525-168">Click **Advanced Tools -> Go** setting for your web app tooaccess Kudu.</span></span>

![A kudu portál használatával][kudu-portal]

<span data-ttu-id="e6525-170">Azonosítsa, hogy a kívánt tooexclude a biztonsági másolatok hello mappákat.</span><span class="sxs-lookup"><span data-stu-id="e6525-170">Identify hello folders that you want tooexclude from your backups.</span></span>  <span data-ttu-id="e6525-171">Például azt szeretné, toofilter hello kijelölt mappa és a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="e6525-171">For example , you want toofilter out hello highlighted folder and files.</span></span>

![Képek mappához][ImagesFolder]

<span data-ttu-id="e6525-173">Hozzon létre egy nevű fájlt `_backup.filter` és a fenti hello lista be hello fájlt, de eltávolítása `D:\home`.</span><span class="sxs-lookup"><span data-stu-id="e6525-173">Create a file called `_backup.filter` and put hello list above in hello file, but remove `D:\home`.</span></span> <span data-ttu-id="e6525-174">Egy soronként fájl vagy könyvtár felsorolása.</span><span class="sxs-lookup"><span data-stu-id="e6525-174">List one directory or file per line.</span></span> <span data-ttu-id="e6525-175">Ezért hello fájl tartalma hello kell:</span><span class="sxs-lookup"><span data-stu-id="e6525-175">So hello content of hello file should be:</span></span>
 ```bash
    \site\wwwroot\Images\brand.png
    \site\wwwroot\Images\2014
    \site\wwwroot\Images\2013
```

<span data-ttu-id="e6525-176">Töltse fel `_backup.filter` toohello fájl `D:\home\site\wwwroot\` mappában található a webhely használatával [ftp](web-sites-deploy.md#ftp) vagy más módszerrel.</span><span class="sxs-lookup"><span data-stu-id="e6525-176">Upload `_backup.filter` file toohello `D:\home\site\wwwroot\` directory of your site using [ftp](web-sites-deploy.md#ftp) or any other method.</span></span> <span data-ttu-id="e6525-177">Ha kívánja, létrehozhat hello fájl közvetlenül a Kudu `DebugConsole` és szúrja be a hiba a hello tartalmat.</span><span class="sxs-lookup"><span data-stu-id="e6525-177">If you wish, you can create hello file directly using Kudu  `DebugConsole` and insert hello content there.</span></span>

<span data-ttu-id="e6525-178">Futtatási biztonsági mentések hello azonos módon szokásos módon teheti meg, [manuálisan](#create-a-manual-backup) vagy [automatikusan](#configure-automated-backups).</span><span class="sxs-lookup"><span data-stu-id="e6525-178">Run backups hello same way you would normally do it, [manually](#create-a-manual-backup) or [automatically](#configure-automated-backups).</span></span> <span data-ttu-id="e6525-179">Most, bármely fájlok és mappák megadott `_backup.filter` hello jövőbeli biztonsági mentések ütemezett, vagy manuálisan indítják el ki van zárva.</span><span class="sxs-lookup"><span data-stu-id="e6525-179">Now, any files and folders that are specified in `_backup.filter` is excluded from hello future backups scheduled or manually initiated.</span></span> 

> [!NOTE]
> <span data-ttu-id="e6525-180">A hely hello részleges biztonsági másolatainak visszaállítása módon [a rendszeres biztonsági másolat visszaállításával](web-sites-restore.md).</span><span class="sxs-lookup"><span data-stu-id="e6525-180">You restore partial backups of your site hello same way you would [restore a regular backup](web-sites-restore.md).</span></span> <span data-ttu-id="e6525-181">hello visszaállítási folyamat hello jobb oldali dolgot tegyenek.</span><span class="sxs-lookup"><span data-stu-id="e6525-181">hello restore process does hello right thing.</span></span>
> 
> <span data-ttu-id="e6525-182">Ha teljes biztonsági mentés helyreállítása, hello hely összes tartalmat helyére függetlenül hello biztonsági mentés van.</span><span class="sxs-lookup"><span data-stu-id="e6525-182">When a full backup is restored, all content on hello site is replaced with whatever is in hello backup.</span></span> <span data-ttu-id="e6525-183">Ha egy fájl hello helyen, de nem hello biztonsági mentés az lekérdezi törlődni fog.</span><span class="sxs-lookup"><span data-stu-id="e6525-183">If a file is on hello site, but not in hello backup it gets deleted.</span></span> <span data-ttu-id="e6525-184">De részleges biztonsági másolat visszaállításakor egyik feketelistára teszi hello könyvtárban, vagy bármely Feketelistára tett fájlban található tartalmakhoz marad, mert a.</span><span class="sxs-lookup"><span data-stu-id="e6525-184">But when a partial backup is restored, any content that is located in one of hello blacklisted directories, or any blacklisted file, is left as is.</span></span>
> 


<a name="aboutbackups"></a>

## <a name="how-backups-are-stored"></a><span data-ttu-id="e6525-185">Biztonsági másolatok tárolási módját</span><span class="sxs-lookup"><span data-stu-id="e6525-185">How backups are stored</span></span>
<span data-ttu-id="e6525-186">Egy vagy több biztonsági mentés az alkalmazás elkészítése után hello biztonsági másolatok jelennek meg hello **tárolók** panelen található a tárfiók, és az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e6525-186">After you have made one or more backups for your app, hello backups are visible on hello **Containers** blade of your storage account, and your app.</span></span> <span data-ttu-id="e6525-187">Hello tárfiókot, minden egyes biztonsági másolat tartalmaz egy`.zip` hello biztonsági mentési adatokat tartalmazó fájlt, és egy `.xml` hello jegyzékfájl tartalmazó fájl `.zip` fájl tartalmát.</span><span class="sxs-lookup"><span data-stu-id="e6525-187">In hello storage account, each backup consists of a`.zip` file that contains hello backup data and an `.xml` file that contains a manifest of hello `.zip` file contents.</span></span> <span data-ttu-id="e6525-188">Csomagolja ki, és keresse meg ezeket a fájlokat, ha azt szeretné, tooaccess a biztonsági másolatok egy alkalmazás-visszaállítási végrehajtása nélkül.</span><span class="sxs-lookup"><span data-stu-id="e6525-188">You can unzip and browse these files if you want tooaccess your backups without actually performing an app restore.</span></span>

<span data-ttu-id="e6525-189">hello az adatbázis biztonsági mentése hello alkalmazás hello legfelső szintű the.zip fájl tárolja.</span><span class="sxs-lookup"><span data-stu-id="e6525-189">hello database backup for hello app is stored in hello root of the.zip file.</span></span> <span data-ttu-id="e6525-190">SQL-adatbázis Ez egy BACPAC-fájl (nincs fájl kiterjesztése) és importálhatók.</span><span class="sxs-lookup"><span data-stu-id="e6525-190">For a SQL database, this is a BACPAC file (no file extension) and can be imported.</span></span> <span data-ttu-id="e6525-191">toocreate a SQL-adatbázis BACPAC exportálási hello alapján című [importálni egy új felhasználói adatbázis BACPAC fájl tooCreate](http://technet.microsoft.com/library/hh710052.aspx).</span><span class="sxs-lookup"><span data-stu-id="e6525-191">toocreate a SQL database based on hello BACPAC export, see [Import a BACPAC File tooCreate a New User Database](http://technet.microsoft.com/library/hh710052.aspx).</span></span>

> [!WARNING]
> <span data-ttu-id="e6525-192">Hello fájlokat a módosítása a **websitebackups** tároló hello biztonsági mentési toobecome érvénytelen, és ezért nem visszaállítható okozhat.</span><span class="sxs-lookup"><span data-stu-id="e6525-192">Altering any of hello files in your **websitebackups** container can cause hello backup toobecome invalid and therefore non-restorable.</span></span>
> 
> 

<a name="nextsteps"></a>

## <a name="next-steps"></a><span data-ttu-id="e6525-193">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e6525-193">Next Steps</span></span>
<span data-ttu-id="e6525-194">A visszaállítása egy alkalmazás olyan biztonsági információ: [visszaállítása egy alkalmazást az Azure-ban](web-sites-restore.md).</span><span class="sxs-lookup"><span data-stu-id="e6525-194">For information on restoring an app from a backup, see [Restore an app in Azure](web-sites-restore.md).</span></span> <span data-ttu-id="e6525-195">Is biztonsági mentése és visszaállítása a REST API használatával App Service apps (lásd: [használata REST toobackup és visszaállítási App Service apps](websites-csm-backup.md)).</span><span class="sxs-lookup"><span data-stu-id="e6525-195">You can also backup and restore App Service apps using REST API (see [Use REST toobackup and restore App Service apps](websites-csm-backup.md)).</span></span>


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

