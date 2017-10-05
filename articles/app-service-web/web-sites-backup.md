---
title: "Adatok biztonsági mentése az Azure-ban"
description: "Megtudhatja, hogyan az alkalmazások biztonsági mentéseinek létrehozását az Azure App Service-ben."
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
ms.openlocfilehash: 77e983afaaba8e944ab1f337e1c28ced83b63205
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="back-up-your-app-in-azure"></a><span data-ttu-id="8a760-103">Adatok biztonsági mentése az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="8a760-103">Back up your app in Azure</span></span>
<span data-ttu-id="8a760-104">A biztonsági mentése és visszaállítás szolgáltatás [Azure App Service](../app-service/app-service-value-prop-what-is.md) lehetővé teszi, hogy könnyen hozzanak létre alkalmazás biztonsági mentést, manuálisan vagy ütemezés szerint.</span><span class="sxs-lookup"><span data-stu-id="8a760-104">The Back up and Restore feature in [Azure App Service](../app-service/app-service-value-prop-what-is.md) lets you easily create app backups manually or on a schedule.</span></span> <span data-ttu-id="8a760-105">Az alkalmazás felülírja a meglévő alkalmazás vagy egy másik alkalmazásnak visszaállítása visszaállíthatja egy korábbi állapothoz pillanatképet.</span><span class="sxs-lookup"><span data-stu-id="8a760-105">You can restore the app to a snapshot of a previous state by overwriting the existing app or restoring to another app.</span></span> 

<span data-ttu-id="8a760-106">Az alkalmazás biztonsági másolatból történő visszaállítását információkért lásd: [visszaállítása egy alkalmazást az Azure-ban](web-sites-restore.md).</span><span class="sxs-lookup"><span data-stu-id="8a760-106">For information on restoring an app from backup, see [Restore an app in Azure](web-sites-restore.md).</span></span>

<a name="whatsbackedup"></a>

## <a name="what-gets-backed-up"></a><span data-ttu-id="8a760-107">Mi a biztonsági mentés beolvasása</span><span class="sxs-lookup"><span data-stu-id="8a760-107">What gets backed up</span></span>
<span data-ttu-id="8a760-108">App Service készíthet biztonsági másolatot egy Azure-tárfiók és tároló, amely az alkalmazás használatára konfigurálta a következő információkat.</span><span class="sxs-lookup"><span data-stu-id="8a760-108">App Service can backup the following information to an Azure storage account and container that you have configured your app to use.</span></span> 

* <span data-ttu-id="8a760-109">Alkalmazáskonfiguráció</span><span class="sxs-lookup"><span data-stu-id="8a760-109">App configuration</span></span>
* <span data-ttu-id="8a760-110">A fájl</span><span class="sxs-lookup"><span data-stu-id="8a760-110">File content</span></span>
* <span data-ttu-id="8a760-111">Az alkalmazáshoz kapcsolódó adatbázis</span><span class="sxs-lookup"><span data-stu-id="8a760-111">Database connected to your app</span></span>

<span data-ttu-id="8a760-112">A következő adatbázis-megoldások biztonsági mentését végző szolgáltatás használata támogatott:</span><span class="sxs-lookup"><span data-stu-id="8a760-112">The following database solutions are supported with backup feature:</span></span> 
   - [<span data-ttu-id="8a760-113">SQL Database</span><span class="sxs-lookup"><span data-stu-id="8a760-113">SQL Database</span></span>](https://azure.microsoft.com/en-us/services/sql-database/)
   - [<span data-ttu-id="8a760-114">A MySQL (előzetes verzió) Azure-adatbázis</span><span class="sxs-lookup"><span data-stu-id="8a760-114">Azure Database for MySQL (Preview)</span></span>](https://azure.microsoft.com/en-us/services/mysql)
   - [<span data-ttu-id="8a760-115">Azure-adatbázis PostgreSQL (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="8a760-115">Azure Database for PostgreSQL (Preview)</span></span>](https://azure.microsoft.com/en-us/services/postgres)
   - [<span data-ttu-id="8a760-116">ClearDB MySQL</span><span class="sxs-lookup"><span data-stu-id="8a760-116">ClearDB MySQL</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SuccessBricksInc.ClearDBMySQLDatabase?tab=Overview)
   - [<span data-ttu-id="8a760-117">MySQL alkalmazásbeli</span><span class="sxs-lookup"><span data-stu-id="8a760-117">MySQL in-app</span></span>](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)
 

> [!NOTE]
>  <span data-ttu-id="8a760-118">Minden biztonsági mentés az alkalmazás nem növekményes frissítés offline teljes másolata.</span><span class="sxs-lookup"><span data-stu-id="8a760-118">Each backup is a complete offline copy of your app, not an incremental update.</span></span>
>  

<a name="requirements"></a>

## <a name="requirements-and-restrictions"></a><span data-ttu-id="8a760-119">Követelmények és korlátozások</span><span class="sxs-lookup"><span data-stu-id="8a760-119">Requirements and restrictions</span></span>
* <span data-ttu-id="8a760-120">A biztonsági mentését és visszaállítását funkció használatához az App Service-csomag kell lennie a **szabványos** réteg vagy **prémium** réteg.</span><span class="sxs-lookup"><span data-stu-id="8a760-120">The Back up and Restore feature requires the App Service plan to be in the **Standard** tier or **Premium** tier.</span></span> <span data-ttu-id="8a760-121">Az alkalmazásszolgáltatási csomag magasabb szintű használható használandó méretezésével kapcsolatos további információkért lásd: [vertikális felskálázás az Azure alkalmazásban](web-sites-scale.md).</span><span class="sxs-lookup"><span data-stu-id="8a760-121">For more information about scaling your App Service plan to use a higher tier, see [Scale up an app in Azure](web-sites-scale.md).</span></span>  
  <span data-ttu-id="8a760-122">**Prémium szintű** réteg lehetővé teszi, hogy a napi nagyobb számú biztonsági ups mint **szabványos** réteg.</span><span class="sxs-lookup"><span data-stu-id="8a760-122">**Premium** tier allows a greater number of daily back ups than **Standard** tier.</span></span>
* <span data-ttu-id="8a760-123">Egy Azure storage-fiók és a tároló ugyanazt az előfizetést, mint az alkalmazás kívánt kell biztonsági másolatot készíteni.</span><span class="sxs-lookup"><span data-stu-id="8a760-123">You need an Azure storage account and container in the same subscription as the app that you want to backup.</span></span> <span data-ttu-id="8a760-124">Az Azure storage-fiókokról további információkért lásd: a [hivatkozások](#moreaboutstorage) Ez a cikk végén.</span><span class="sxs-lookup"><span data-stu-id="8a760-124">For more information on Azure storage accounts, see the [links](#moreaboutstorage) at the end of this article.</span></span>
* <span data-ttu-id="8a760-125">Biztonsági mentés az alkalmazás- és a tartalom legfeljebb 10 GB-os lehet.</span><span class="sxs-lookup"><span data-stu-id="8a760-125">Backups can be up to 10 GB of app and database content.</span></span> <span data-ttu-id="8a760-126">Ha a biztonsági másolat mérete meghaladja ezt a korlátot, hibaüzenetet kap.</span><span class="sxs-lookup"><span data-stu-id="8a760-126">If the backup size exceeds this limit, you get an error .</span></span>

<a name="manualbackup"></a>

## <a name="create-a-manual-backup"></a><span data-ttu-id="8a760-127">Manuális biztonsági mentés létrehozása</span><span class="sxs-lookup"><span data-stu-id="8a760-127">Create a manual backup</span></span>
1. <span data-ttu-id="8a760-128">Az a [Azure Portal](https://portal.azure.com), keresse meg az alkalmazás panelen, jelölje ki **biztonsági mentések**.</span><span class="sxs-lookup"><span data-stu-id="8a760-128">In the [Azure Portal](https://portal.azure.com), navigate to your app's blade, select **Backups**.</span></span> <span data-ttu-id="8a760-129">A **biztonsági mentések** panel fog megjelenni.</span><span class="sxs-lookup"><span data-stu-id="8a760-129">The **Backups** blade will be displayed.</span></span>
   
    ![Biztonsági mentések lap][ChooseBackupsPage]
   
   > [!NOTE]
   > <span data-ttu-id="8a760-131">Ha az alábbi üzenet jelenik meg, kattintson rá az App Service-csomag frissítése előtt nyugodtan folytathatja a biztonsági másolatok.</span><span class="sxs-lookup"><span data-stu-id="8a760-131">If you see the message below, click it to upgrade your App Service plan before you can proceed with backups.</span></span>
   > <span data-ttu-id="8a760-132">Lásd: [vertikális felskálázás az Azure alkalmazásban](web-sites-scale.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="8a760-132">See [Scale up an app in Azure](web-sites-scale.md) for more information.</span></span>  
   > <span data-ttu-id="8a760-133">![Válassza ki a tárfiók](./media/web-sites-backup/01UpgradePlan1.png)</span><span class="sxs-lookup"><span data-stu-id="8a760-133">![Choose storage account](./media/web-sites-backup/01UpgradePlan1.png)</span></span>
   > 
   > 

2. <span data-ttu-id="8a760-134">A a **biztonsági mentés** paneljén kattintson **konfigurálása**
![kattintson konfigurálása](./media/web-sites-backup/ClickConfigure1.png)</span><span class="sxs-lookup"><span data-stu-id="8a760-134">In the **Backup** blade, Click **Configure**
![Click Configure](./media/web-sites-backup/ClickConfigure1.png)</span></span>
3. <span data-ttu-id="8a760-135">Az a **biztonsági mentési konfigurációhoz** panelen kattintson a **tárolási: nincs konfigurálva** storage-fiókok konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="8a760-135">In the **Backup Configuration** blade, click **Storage: Not configured** to configure a storage account.</span></span>
   
    ![Válassza ki a tárfiók][ChooseStorageAccount]
4. <span data-ttu-id="8a760-137">A biztonsági mentés célhelyének megadásához jelöljön ki egy **Tárfiók** és **tároló**.</span><span class="sxs-lookup"><span data-stu-id="8a760-137">Choose your backup destination by selecting a **Storage Account** and **Container**.</span></span> <span data-ttu-id="8a760-138">A tárfiók ugyanahhoz az előfizetéshez, mint a kívánt alkalmazást, készítsen biztonsági másolatot kell tartoznia.</span><span class="sxs-lookup"><span data-stu-id="8a760-138">The storage account must belong to the same subscription as the app you want to back up.</span></span> <span data-ttu-id="8a760-139">Ha kívánja, létrehozhat egy új tárfiókot vagy egy új tároló a megfelelő panelt a.</span><span class="sxs-lookup"><span data-stu-id="8a760-139">If you wish, you can create a new storage account or a new container in the respective blades.</span></span> <span data-ttu-id="8a760-140">Amikor elkészült, kattintson a **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="8a760-140">When you're done, click **Select**.</span></span>
   
    ![Válassza ki a tárfiók](./media/web-sites-backup/02ChooseStorageAccount1-1.png)
5. <span data-ttu-id="8a760-142">A a **biztonsági mentési konfigurációhoz** még mindig nyitva marad panelen beállíthatja **adatbázis biztonsági másolata**, majd válassza ki a szerepeljen a biztonsági mentések (SQL-adatbázis vagy MySQL), majd kattintson a kívánt adatbázisokat **OK**.</span><span class="sxs-lookup"><span data-stu-id="8a760-142">In the **Backup Configuration** blade that is still left open, you can configure **Backup Database**, then select the databases you want to include in the backups (SQL database or MySQL), then click **OK**.</span></span>  
   
    ![Válassza ki a tárfiók](./media/web-sites-backup/03ConfigureDatabase1.png)
   
   > [!NOTE]
   > <span data-ttu-id="8a760-144">Ebben a listában szerepelnek az adatbázis, a kapcsolati karakterláncában szerepelnie kell a **kapcsolati karakterláncok** szakasza a **Alkalmazásbeállítások** alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8a760-144">For a database to appear in this list, its connection string must exist in the **Connection strings** section of the **Application settings** blade for your app.</span></span>
   > 
   > 
6. <span data-ttu-id="8a760-145">Az a **biztonsági mentési konfigurációhoz** panelen kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="8a760-145">In the **Backup Configuration** blade, click **Save**.</span></span>    
7. <span data-ttu-id="8a760-146">Az a **biztonsági mentések** panelen kattintson a **biztonsági mentés**.</span><span class="sxs-lookup"><span data-stu-id="8a760-146">In the  **Backups** blade, click **Backup**.</span></span>
   
    ![BackUpNow gomb][BackUpNow]
   
    <span data-ttu-id="8a760-148">A folyamatban lévő üzenet jelenik meg a biztonsági mentési folyamat során.</span><span class="sxs-lookup"><span data-stu-id="8a760-148">You see a progress message during the backup process.</span></span>

<span data-ttu-id="8a760-149">Miután beállította a tárfiók és tároló manuális biztonsági mentés bármikor kezdeményezhető.</span><span class="sxs-lookup"><span data-stu-id="8a760-149">Once the storage account and container is configured you can initiate a manual backup at any time.</span></span>  

<a name="automatedbackups"></a>

## <a name="configure-automated-backups"></a><span data-ttu-id="8a760-150">Az automatikus biztonsági mentések konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8a760-150">Configure automated backups</span></span>
1. <span data-ttu-id="8a760-151">Az a **biztonsági mentési konfigurációhoz** panelen állítsa **ütemezett biztonsági mentés** való **a**.</span><span class="sxs-lookup"><span data-stu-id="8a760-151">In the **Backup Configuration** blade, set **Scheduled backup** to **On**.</span></span> 
   
    ![Válassza ki a tárfiók](./media/web-sites-backup/05ScheduleBackup1.png)
2. <span data-ttu-id="8a760-153">Beállítások megjelenik, biztonsági mentési ütemezés beállítása **ütemezett biztonsági mentési** való **a**, majd konfigurálja a biztonsági mentés ütemezése tetszés szerint, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="8a760-153">Backup schedule options will show up, set **Scheduled Backup** to **On**, then configure the backup schedule as desired and click **OK**.</span></span>
   
    ![Az automatikus biztonsági mentés engedélyezése][SetAutomatedBackupOn]

<a name="partialbackups"></a>

## <a name="configure-partial-backups"></a><span data-ttu-id="8a760-155">Részleges biztonsági mentések konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8a760-155">Configure Partial Backups</span></span>
<span data-ttu-id="8a760-156">Néha nem kívánja minden, az alkalmazás a biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="8a760-156">Sometimes you don't want to backup everything on your app.</span></span> <span data-ttu-id="8a760-157">Íme, néhány példa:</span><span class="sxs-lookup"><span data-stu-id="8a760-157">Here are a few examples:</span></span>

* <span data-ttu-id="8a760-158">Ön [beállítása heti biztonsági mentései](web-sites-backup.md#configure-automated-backups) az alkalmazás, amely tartalmazza a statikus tartalom, amely soha nem változik, például a régi blogbejegyzések vagy képeket.</span><span class="sxs-lookup"><span data-stu-id="8a760-158">You [set up weekly backups](web-sites-backup.md#configure-automated-backups) of your app that contains static content that never changes, such as old blog posts or images.</span></span>
* <span data-ttu-id="8a760-159">Az alkalmazás rendelkezik több mint 10 GB-os tartalomtípus (Ez az a maximális időtartam készíthet biztonsági másolatot egy időben).</span><span class="sxs-lookup"><span data-stu-id="8a760-159">Your app has over 10 GB of content (that's the max amount you can backup at a time).</span></span>
* <span data-ttu-id="8a760-160">Nem szeretné a naplófájlok biztonsági mentését.</span><span class="sxs-lookup"><span data-stu-id="8a760-160">You don't want to backup the log files.</span></span>

<span data-ttu-id="8a760-161">Részleges biztonsági másolatok lehetővé teszi, hogy úgy dönt, hogy pontosan amely fájlokat szeretne biztonsági másolatot készíteni.</span><span class="sxs-lookup"><span data-stu-id="8a760-161">Partial backups allows you choose exactly which files you want to backup.</span></span>

### <a name="exclude-files-from-your-backup"></a><span data-ttu-id="8a760-162">Fájlok kizárása a biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="8a760-162">Exclude files from your backup</span></span>
<span data-ttu-id="8a760-163">Tegyük fel, hogy egy alkalmazás, amely tartalmazza a naplófájlok és a statikus képeket, biztonsági mentési egyszer és nem kívánja módosítani.</span><span class="sxs-lookup"><span data-stu-id="8a760-163">Suppose you have an app that contains log files and static images that have been backup once and are not going to change.</span></span> <span data-ttu-id="8a760-164">Ilyen esetekben kizárhatja azokat a fájlokat és mappákat a jövőbeni biztonsági mentések tárolják.</span><span class="sxs-lookup"><span data-stu-id="8a760-164">In such cases you can exclude those folders and files from being stored in your future backups.</span></span> <span data-ttu-id="8a760-165">Fájlok és mappák kizárása a biztonsági másolatok, hozzon létre egy `_backup.filter` fájlt a `D:\home\site\wwwroot` az alkalmazás mappájában.</span><span class="sxs-lookup"><span data-stu-id="8a760-165">To exclude files and folders from your backups, create a `_backup.filter` file in the `D:\home\site\wwwroot` folder of your app.</span></span> <span data-ttu-id="8a760-166">Megadja azokat a fájlokat és mappákat szeretne kizárni a fájlban található.</span><span class="sxs-lookup"><span data-stu-id="8a760-166">Specify the list of files and folders you want to exclude in this file.</span></span> 

<span data-ttu-id="8a760-167">A fájlok eléréséhez egyszerűen, hogy a Kudu használja.</span><span class="sxs-lookup"><span data-stu-id="8a760-167">An easy way to access your files is to use Kudu .</span></span> <span data-ttu-id="8a760-168">Kattintson a **speciális eszközök -> Ugrás** Kudu eléréséhez a webalkalmazás beállítása.</span><span class="sxs-lookup"><span data-stu-id="8a760-168">Click **Advanced Tools -> Go** setting for your web app to access Kudu.</span></span>

![A kudu portál használatával][kudu-portal]

<span data-ttu-id="8a760-170">Azonosítsa a biztonsági másolatok kizárni kívánt mappákat.</span><span class="sxs-lookup"><span data-stu-id="8a760-170">Identify the folders that you want to exclude from your backups.</span></span>  <span data-ttu-id="8a760-171">Például szeretné a kijelölt mappa és a fájlok szűrik.</span><span class="sxs-lookup"><span data-stu-id="8a760-171">For example , you want to filter out the highlighted folder and files.</span></span>

![Képek mappához][ImagesFolder]

<span data-ttu-id="8a760-173">Hozzon létre egy nevű fájlt `_backup.filter` és a fenti lista be a fájlt, de eltávolítása `D:\home`.</span><span class="sxs-lookup"><span data-stu-id="8a760-173">Create a file called `_backup.filter` and put the list above in the file, but remove `D:\home`.</span></span> <span data-ttu-id="8a760-174">Egy soronként fájl vagy könyvtár felsorolása.</span><span class="sxs-lookup"><span data-stu-id="8a760-174">List one directory or file per line.</span></span> <span data-ttu-id="8a760-175">Ezért a fájl tartalma kell lennie:</span><span class="sxs-lookup"><span data-stu-id="8a760-175">So the content of the file should be:</span></span>
 ```bash
    \site\wwwroot\Images\brand.png
    \site\wwwroot\Images\2014
    \site\wwwroot\Images\2013
```

<span data-ttu-id="8a760-176">Töltse fel `_backup.filter` fájlt a `D:\home\site\wwwroot\` mappában található a webhely használatával [ftp](web-sites-deploy.md#ftp) vagy más módszerrel.</span><span class="sxs-lookup"><span data-stu-id="8a760-176">Upload `_backup.filter` file to the `D:\home\site\wwwroot\` directory of your site using [ftp](web-sites-deploy.md#ftp) or any other method.</span></span> <span data-ttu-id="8a760-177">Ha kívánja, a fájlt közvetlenül a Kudu segítségével létrehozhat `DebugConsole` és szúrja be a hiba a tartalmat.</span><span class="sxs-lookup"><span data-stu-id="8a760-177">If you wish, you can create the file directly using Kudu  `DebugConsole` and insert the content there.</span></span>

<span data-ttu-id="8a760-178">Biztonsági mentések futtatása a szokásos módon teheti meg, ugyanúgy [manuálisan](#create-a-manual-backup) vagy [automatikusan](#configure-automated-backups).</span><span class="sxs-lookup"><span data-stu-id="8a760-178">Run backups the same way you would normally do it, [manually](#create-a-manual-backup) or [automatically](#configure-automated-backups).</span></span> <span data-ttu-id="8a760-179">Most, bármely fájlok és mappák megadott `_backup.filter` ki van zárva a jövőbeli biztonsági mentések ütemezett, vagy manuálisan indítják el.</span><span class="sxs-lookup"><span data-stu-id="8a760-179">Now, any files and folders that are specified in `_backup.filter` is excluded from the future backups scheduled or manually initiated.</span></span> 

> [!NOTE]
> <span data-ttu-id="8a760-180">A webhely részleges biztonsági másolatok a módon visszaállítása [a rendszeres biztonsági másolat visszaállításával](web-sites-restore.md).</span><span class="sxs-lookup"><span data-stu-id="8a760-180">You restore partial backups of your site the same way you would [restore a regular backup](web-sites-restore.md).</span></span> <span data-ttu-id="8a760-181">A visszaállítási folyamat nem a megfelelő művelet.</span><span class="sxs-lookup"><span data-stu-id="8a760-181">The restore process does the right thing.</span></span>
> 
> <span data-ttu-id="8a760-182">Ha egy teljes biztonsági mentés helyreállítása, a hely összes tartalmat helyére függetlenül a biztonsági mentés van.</span><span class="sxs-lookup"><span data-stu-id="8a760-182">When a full backup is restored, all content on the site is replaced with whatever is in the backup.</span></span> <span data-ttu-id="8a760-183">Ha egy fájl a helyen, de nem a biztonsági mentés az lekérdezi törlődni fog.</span><span class="sxs-lookup"><span data-stu-id="8a760-183">If a file is on the site, but not in the backup it gets deleted.</span></span> <span data-ttu-id="8a760-184">De részleges biztonsági másolat visszaállításakor egyik Feketelistára tett könyvtárban, vagy bármely Feketelistára tett fájlban található tartalmakhoz marad, mert a.</span><span class="sxs-lookup"><span data-stu-id="8a760-184">But when a partial backup is restored, any content that is located in one of the blacklisted directories, or any blacklisted file, is left as is.</span></span>
> 


<a name="aboutbackups"></a>

## <a name="how-backups-are-stored"></a><span data-ttu-id="8a760-185">Biztonsági másolatok tárolási módját</span><span class="sxs-lookup"><span data-stu-id="8a760-185">How backups are stored</span></span>
<span data-ttu-id="8a760-186">Egy vagy több biztonsági mentés az alkalmazás elkészítése után a biztonsági másolatok jelennek meg a **tárolók** panelen található a tárfiók, és az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8a760-186">After you have made one or more backups for your app, the backups are visible on the **Containers** blade of your storage account, and your app.</span></span> <span data-ttu-id="8a760-187">A tárfiókban lévő minden egyes biztonsági másolat áll egy`.zip` a biztonsági mentési adatokat tartalmazó fájlt, és egy `.xml` a javítócsomagban adatait tartalmazó fájlt a `.zip` fájl tartalmát.</span><span class="sxs-lookup"><span data-stu-id="8a760-187">In the storage account, each backup consists of a`.zip` file that contains the backup data and an `.xml` file that contains a manifest of the `.zip` file contents.</span></span> <span data-ttu-id="8a760-188">Csomagolja ki, és keresse meg ezeket a fájlokat, ha azt szeretné, hogy egy alkalmazás-visszaállítási végrehajtása nélkül a biztonsági másolatok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="8a760-188">You can unzip and browse these files if you want to access your backups without actually performing an app restore.</span></span>

<span data-ttu-id="8a760-189">Az adatbázis biztonsági mentése az alkalmazás the.zip fájl tárolja.</span><span class="sxs-lookup"><span data-stu-id="8a760-189">The database backup for the app is stored in the root of the.zip file.</span></span> <span data-ttu-id="8a760-190">SQL-adatbázis Ez egy BACPAC-fájl (nincs fájl kiterjesztése) és importálhatók.</span><span class="sxs-lookup"><span data-stu-id="8a760-190">For a SQL database, this is a BACPAC file (no file extension) and can be imported.</span></span> <span data-ttu-id="8a760-191">Egy SQL-adatbázis BACPAC exportálás alapján létrehozásához lásd: [létrehozni egy új felhasználói adatbázis BACPAC fájl importálása](http://technet.microsoft.com/library/hh710052.aspx).</span><span class="sxs-lookup"><span data-stu-id="8a760-191">To create a SQL database based on the BACPAC export, see [Import a BACPAC File to Create a New User Database](http://technet.microsoft.com/library/hh710052.aspx).</span></span>

> [!WARNING]
> <span data-ttu-id="8a760-192">Módosítása a fájlokat a **websitebackups** tároló okozhat a biztonsági mentés érvénytelen, és ezért nem visszaállítható válik.</span><span class="sxs-lookup"><span data-stu-id="8a760-192">Altering any of the files in your **websitebackups** container can cause the backup to become invalid and therefore non-restorable.</span></span>
> 
> 

<a name="nextsteps"></a>

## <a name="next-steps"></a><span data-ttu-id="8a760-193">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8a760-193">Next Steps</span></span>
<span data-ttu-id="8a760-194">A visszaállítása egy alkalmazás olyan biztonsági információ: [visszaállítása egy alkalmazást az Azure-ban](web-sites-restore.md).</span><span class="sxs-lookup"><span data-stu-id="8a760-194">For information on restoring an app from a backup, see [Restore an app in Azure](web-sites-restore.md).</span></span> <span data-ttu-id="8a760-195">Is biztonsági mentése és visszaállítása a REST API használatával App Service apps (lásd: [biztonsági mentése és visszaállítása az App Service apps használata REST](websites-csm-backup.md)).</span><span class="sxs-lookup"><span data-stu-id="8a760-195">You can also backup and restore App Service apps using REST API (see [Use REST to backup and restore App Service apps](websites-csm-backup.md)).</span></span>


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

