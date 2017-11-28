---
title: "aaaRestore egy alkalmazást az Azure-ban"
description: "Megtudhatja, hogyan toorestore az alkalmazás egy biztonsági másolatból."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: 4444dbf7-363c-47e2-b24a-dbd45cb08491
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.openlocfilehash: 4b54029a9197064f990f29a3c4558c8322668714
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="restore-an-app-in-azure"></a><span data-ttu-id="04f5e-103">Alkalmazás visszaállítása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="04f5e-103">Restore an app in Azure</span></span>
<span data-ttu-id="04f5e-104">Ez a cikk bemutatja, hogyan egy alkalmazás toorestore [Azure App Service](../app-service/app-service-value-prop-what-is.md) korábban biztonsági (lásd: [készítsen biztonsági másolatot az alkalmazás az Azure-ban](web-sites-backup.md)).</span><span class="sxs-lookup"><span data-stu-id="04f5e-104">This article shows you how toorestore an app in [Azure App Service](../app-service/app-service-value-prop-what-is.md) that you have previously backed up (see [Back up your app in Azure](web-sites-backup.md)).</span></span> <span data-ttu-id="04f5e-105">Az alkalmazás a csatolt adatbázisok igény szerinti tooa korábbi állapotának visszaállításához, vagy hozzon létre egy új alkalmazást, az eredeti alkalmazás biztonsági mentési valamelyike alapján.</span><span class="sxs-lookup"><span data-stu-id="04f5e-105">You can restore your app with its linked databases on-demand tooa previous state, or create a new app based on one of your original app's backup.</span></span> <span data-ttu-id="04f5e-106">Az Azure App Service adatbázisok biztonsági mentéséhez és visszaállításához a következő hello támogatja:</span><span class="sxs-lookup"><span data-stu-id="04f5e-106">Azure App Service supports hello following databases for backup and restore:</span></span>
- [<span data-ttu-id="04f5e-107">SQL Database</span><span class="sxs-lookup"><span data-stu-id="04f5e-107">SQL Database</span></span>](https://azure.microsoft.com/en-us/services/sql-database/)
- [<span data-ttu-id="04f5e-108">A MySQL (előzetes verzió) Azure-adatbázis</span><span class="sxs-lookup"><span data-stu-id="04f5e-108">Azure Database for MySQL (Preview)</span></span>](https://azure.microsoft.com/en-us/services/mysql)
- [<span data-ttu-id="04f5e-109">Azure-adatbázis PostgreSQL (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="04f5e-109">Azure Database for PostgreSQL (Preview)</span></span>](https://azure.microsoft.com/en-us/services/postgres)
- [<span data-ttu-id="04f5e-110">ClearDB MySQL</span><span class="sxs-lookup"><span data-stu-id="04f5e-110">ClearDB MySQL</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SuccessBricksInc.ClearDBMySQLDatabase?tab=Overview)
- [<span data-ttu-id="04f5e-111">MySQL alkalmazásbeli</span><span class="sxs-lookup"><span data-stu-id="04f5e-111">MySQL in-app</span></span>](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)

<span data-ttu-id="04f5e-112">Visszaállítása biztonsági másolatból a rendszer fut rendelkezésre álló tooapps **szabványos** és **prémium** réteg.</span><span class="sxs-lookup"><span data-stu-id="04f5e-112">Restoring from backups is available tooapps running in **Standard** and **Premium** tier.</span></span> <span data-ttu-id="04f5e-113">Az alkalmazás vertikális felskálázásával kapcsolatos információkért lásd: [vertikális felskálázás az Azure alkalmazásban](web-sites-scale.md).</span><span class="sxs-lookup"><span data-stu-id="04f5e-113">For information about scaling up your app, see [Scale up an app in Azure](web-sites-scale.md).</span></span> <span data-ttu-id="04f5e-114">**Prémium szintű** réteg lehetővé teszi, hogy a napi biztonsági mentések toobe végre, mint nagyobb számú **szabványos** réteg.</span><span class="sxs-lookup"><span data-stu-id="04f5e-114">**Premium** tier allows a greater number of daily backups toobe performed than **Standard** tier.</span></span>

<a name="PreviousBackup"></a>

## <a name="restore-an-app-from-an-existing-backup"></a><span data-ttu-id="04f5e-115">Egy alkalmazás meglévő biztonsági másolaton visszaállítása</span><span class="sxs-lookup"><span data-stu-id="04f5e-115">Restore an app from an existing backup</span></span>
1. <span data-ttu-id="04f5e-116">A hello **beállítások** panelen található az alkalmazáshoz a hello Azure portál, kattintson a **biztonsági mentések** toodisplay hello **biztonsági mentések** panelen.</span><span class="sxs-lookup"><span data-stu-id="04f5e-116">On hello **Settings** blade of your app in hello Azure Portal, click **Backups** toodisplay hello **Backups** blade.</span></span> <span data-ttu-id="04f5e-117">Kattintson a **visszaállítása**.</span><span class="sxs-lookup"><span data-stu-id="04f5e-117">Then click **Restore**.</span></span>
   
    ![Válassza ki a visszaállítási most][ChooseRestoreNow]
2. <span data-ttu-id="04f5e-119">A hello **visszaállítása** panelen, először válassza hello biztonsági mentési forrás.</span><span class="sxs-lookup"><span data-stu-id="04f5e-119">In hello **Restore** blade, first select hello backup source.</span></span>
   
    ![](./media/web-sites-restore/021ChooseSource1.png)
   
    <span data-ttu-id="04f5e-120">Hello **alkalmazás biztonsági mentési** beállítást választja, megjelenik az összes hello hello aktuális alkalmazás létező biztonsági másolatai, és egyszerűen kiválaszthatja az egyik.</span><span class="sxs-lookup"><span data-stu-id="04f5e-120">hello **App backup** option shows you all hello existing backups of hello current app, and you can easily select one.</span></span>
    <span data-ttu-id="04f5e-121">Hello **tárolási** beállítás lehetővé teszi bármely biztonsági mentési ZIP-fájl válasszon a meglévő Azure Storage-fiók és tároló az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="04f5e-121">hello **Storage** option lets you select any backup ZIP file from any existing Azure Storage account and container in your subscription.</span></span>
    <span data-ttu-id="04f5e-122">Ha azt egy másik alkalmazás biztonsági toorestore, használja a hello **tárolási** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="04f5e-122">If you're trying toorestore a backup of another app, use hello **Storage** option.</span></span>
3. <span data-ttu-id="04f5e-123">Ezt követően adja a hello alkalmazás visszaállításához hello céljának **visszaállítási célhelyének ellenőrzése**.</span><span class="sxs-lookup"><span data-stu-id="04f5e-123">Then, specify hello destination for hello app restore in **Restore destination**.</span></span>
   
    ![](./media/web-sites-restore/022ChooseDestination1.png)
   
   > [!WARNING]
   > <span data-ttu-id="04f5e-124">Ha úgy dönt, **felülírása**, az összes meglévő adatok az aktuális alkalmazás törlése, és írja felül.</span><span class="sxs-lookup"><span data-stu-id="04f5e-124">If you choose **Overwrite**, all existing data in your current app is erased and overwritten.</span></span> <span data-ttu-id="04f5e-125">Kattintás előtt **OK**, győződjön meg arról, hogy pontosan mit toodo.</span><span class="sxs-lookup"><span data-stu-id="04f5e-125">Before you click **OK**, make sure that it is exactly what you want toodo.</span></span>
   > 
   > 
   
    <span data-ttu-id="04f5e-126">Kiválaszthatja **meglévő App** toorestore hello alkalmazás biztonsági mentési tooanother alkalmazás hello resoure ugyanabban a csoportban.</span><span class="sxs-lookup"><span data-stu-id="04f5e-126">You can select **Existing App** toorestore hello app backup tooanother app in hello same resoure group.</span></span> <span data-ttu-id="04f5e-127">Mielőtt ezt a beállítást használja, kell már létrehozott egy másik alkalmazás a tükrözés adatbázis konfigurációs toohello több hello alkalmazás biztonsági mentése van definiálva az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="04f5e-127">Before you use this option, you should have already created another app in your resource group with mirroring database configuration toohello one defined in hello app backup.</span></span> <span data-ttu-id="04f5e-128">Létrehozhat egy **új** app toorestore a tartalmat.</span><span class="sxs-lookup"><span data-stu-id="04f5e-128">You can also Create a **New** app toorestore your content to.</span></span>

4. <span data-ttu-id="04f5e-129">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="04f5e-129">Click **OK**.</span></span>

<a name="StorageAccount"></a>

## <a name="download-or-delete-a-backup-from-a-storage-account"></a><span data-ttu-id="04f5e-130">Töltse le, illetve törölhet egy biztonsági mentés a storage-fiók</span><span class="sxs-lookup"><span data-stu-id="04f5e-130">Download or delete a backup from a storage account</span></span>
1. <span data-ttu-id="04f5e-131">A fő hello **Tallózás** hello Azure portálon, válassza a panel **tárfiókok**.</span><span class="sxs-lookup"><span data-stu-id="04f5e-131">From hello main **Browse** blade of hello Azure portal, select **Storage accounts**.</span></span> <span data-ttu-id="04f5e-132">A meglévő tárfiókok listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="04f5e-132">A list of your existing storage accounts is displayed.</span></span>
2. <span data-ttu-id="04f5e-133">Válassza ki a kívánt hello tárfiók toodownload vagy delete.hello panel jelenik meg hello biztonsági másolatot tartalmazó hello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="04f5e-133">Select hello storage account that contains hello backup that you want toodownload or delete.hello blade for hello storage account is displayed.</span></span>
3. <span data-ttu-id="04f5e-134">A hello storage-fiók panelen válassza ki a kívánt hello tároló</span><span class="sxs-lookup"><span data-stu-id="04f5e-134">In hello storage account blade, select hello container you want</span></span>
   
    ![Nézet tárolók][ViewContainers]
4. <span data-ttu-id="04f5e-136">Válassza ki a biztonságimásolat-fájl szeretné, hogy toodownload vagy törlése.</span><span class="sxs-lookup"><span data-stu-id="04f5e-136">Select backup file you want toodownload or delete.</span></span>
   
    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)
5. <span data-ttu-id="04f5e-138">Kattintson a **letöltése** vagy **törlése** attól függően, hogy milyen azt szeretné, hogy toodo.</span><span class="sxs-lookup"><span data-stu-id="04f5e-138">Click **Download** or **Delete** depending on what you want toodo.</span></span>  

<a name="OperationLogs"></a>

## <a name="monitor-a-restore-operation"></a><span data-ttu-id="04f5e-139">A figyelő a visszaállítási művelet</span><span class="sxs-lookup"><span data-stu-id="04f5e-139">Monitor a restore operation</span></span>
<span data-ttu-id="04f5e-140">toosee adatait hello sikerességét vagy sikertelenségét hello app visszaállítási műveletet, nyissa meg a toohello **tevékenységnapló** panel az Azure-portálon hello.</span><span class="sxs-lookup"><span data-stu-id="04f5e-140">toosee details about hello success or failure of hello app restore operation, navigate toohello **Activity Log** blade in hello Azure portal.</span></span>  
 

<span data-ttu-id="04f5e-141">Görgessen le a szükséges hello visszaállítási művelet, és kattintson tooselect toofind azt.</span><span class="sxs-lookup"><span data-stu-id="04f5e-141">Scroll down toofind hello desired restore operation and click tooselect it.</span></span>

<span data-ttu-id="04f5e-142">hello Részletek panel információit jeleníti meg hello elérhető kapcsolatos toohello visszaállítási művelet.</span><span class="sxs-lookup"><span data-stu-id="04f5e-142">hello details blade displays hello available information related toohello restore operation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="04f5e-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="04f5e-143">Next Steps</span></span>
<span data-ttu-id="04f5e-144">Biztonsági mentéshez, és állítsa vissza az App Service apps REST API használatával (lásd: [használata REST tooback össze, és állítsa vissza az App Service apps](websites-csm-backup.md)).</span><span class="sxs-lookup"><span data-stu-id="04f5e-144">You can backup and restore App Service apps using REST API (see [Use REST tooback up and restore App Service apps](websites-csm-backup.md)).</span></span>


<!-- IMAGES -->
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow1.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
[StorageAccountFile]: ./media/web-sites-restore/02StorageAccountFile.png
[BrowseCloudStorage]: ./media/web-sites-restore/03BrowseCloudStorage.png
[StorageAccountFileSelected]: ./media/web-sites-restore/04StorageAccountFileSelected.png
[ChooseRestoreSettings]: ./media/web-sites-restore/05ChooseRestoreSettings.png
[ChooseDBServer]: ./media/web-sites-restore/06ChooseDBServer.png
[RestoreToNewSQLDB]: ./media/web-sites-restore/07RestoreToNewSQLDB.png
[NewSQLDBConfig]: ./media/web-sites-restore/08NewSQLDBConfig.png
[RestoredContosoWebSite]: ./media/web-sites-restore/09RestoredContosoWebSite.png
[DashboardOperationLogsLink]: ./media/web-sites-restore/10DashboardOperationLogsLink.png
[ManagementServicesOperationLogsList]: ./media/web-sites-restore/11ManagementServicesOperationLogsList.png
[DetailsButton]: ./media/web-sites-restore/12DetailsButton.png
[OperationDetails]: ./media/web-sites-restore/13OperationDetails.png
