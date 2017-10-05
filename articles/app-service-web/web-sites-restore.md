---
title: "Alkalmazás visszaállítása az Azure-ban"
description: "Útmutató: az alkalmazás visszaállítása biztonsági másolatból."
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
ms.openlocfilehash: 5fe74d992edb7028fa4a2500e427013d98ebc250
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="restore-an-app-in-azure"></a><span data-ttu-id="6ef4a-103">Alkalmazás visszaállítása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="6ef4a-103">Restore an app in Azure</span></span>
<span data-ttu-id="6ef4a-104">A cikkből megtudhatja, hogyan lehet visszaállítani az alkalmazásban [Azure App Service](../app-service/app-service-value-prop-what-is.md) korábban biztonsági (lásd: [készítsen biztonsági másolatot az alkalmazás az Azure-ban](web-sites-backup.md)).</span><span class="sxs-lookup"><span data-stu-id="6ef4a-104">This article shows you how to restore an app in [Azure App Service](../app-service/app-service-value-prop-what-is.md) that you have previously backed up (see [Back up your app in Azure](web-sites-backup.md)).</span></span> <span data-ttu-id="6ef4a-105">Az alkalmazás a csatolt adatbázisok az igény a visszaállítás egy korábbi állapotára, vagy hozzon létre egy új alkalmazást, az eredeti alkalmazás biztonsági mentési valamelyike alapján.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-105">You can restore your app with its linked databases on-demand to a previous state, or create a new app based on one of your original app's backup.</span></span> <span data-ttu-id="6ef4a-106">Az Azure App Service a következő adatbázisok biztonsági mentését és helyreállítását támogatja:</span><span class="sxs-lookup"><span data-stu-id="6ef4a-106">Azure App Service supports the following databases for backup and restore:</span></span>
- [<span data-ttu-id="6ef4a-107">SQL Database</span><span class="sxs-lookup"><span data-stu-id="6ef4a-107">SQL Database</span></span>](https://azure.microsoft.com/en-us/services/sql-database/)
- [<span data-ttu-id="6ef4a-108">A MySQL (előzetes verzió) Azure-adatbázis</span><span class="sxs-lookup"><span data-stu-id="6ef4a-108">Azure Database for MySQL (Preview)</span></span>](https://azure.microsoft.com/en-us/services/mysql)
- [<span data-ttu-id="6ef4a-109">Azure-adatbázis PostgreSQL (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="6ef4a-109">Azure Database for PostgreSQL (Preview)</span></span>](https://azure.microsoft.com/en-us/services/postgres)
- [<span data-ttu-id="6ef4a-110">ClearDB MySQL</span><span class="sxs-lookup"><span data-stu-id="6ef4a-110">ClearDB MySQL</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SuccessBricksInc.ClearDBMySQLDatabase?tab=Overview)
- [<span data-ttu-id="6ef4a-111">MySQL alkalmazásbeli</span><span class="sxs-lookup"><span data-stu-id="6ef4a-111">MySQL in-app</span></span>](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)

<span data-ttu-id="6ef4a-112">Futó alkalmazások számára elérhető visszaállítása biztonsági másolatból rendszer **szabványos** és **prémium** réteg.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-112">Restoring from backups is available to apps running in **Standard** and **Premium** tier.</span></span> <span data-ttu-id="6ef4a-113">Az alkalmazás vertikális felskálázásával kapcsolatos információkért lásd: [vertikális felskálázás az Azure alkalmazásban](web-sites-scale.md).</span><span class="sxs-lookup"><span data-stu-id="6ef4a-113">For information about scaling up your app, see [Scale up an app in Azure](web-sites-scale.md).</span></span> <span data-ttu-id="6ef4a-114">**Prémium szintű** réteg lehetővé teszi, hogy a napi biztonsági mentését, mint nagyobb számú **szabványos** réteg.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-114">**Premium** tier allows a greater number of daily backups to be performed than **Standard** tier.</span></span>

<a name="PreviousBackup"></a>

## <a name="restore-an-app-from-an-existing-backup"></a><span data-ttu-id="6ef4a-115">Egy alkalmazás meglévő biztonsági másolaton visszaállítása</span><span class="sxs-lookup"><span data-stu-id="6ef4a-115">Restore an app from an existing backup</span></span>
1. <span data-ttu-id="6ef4a-116">Az a **beállítások** panelen található az alkalmazás az Azure portálon kattintson **biztonsági mentések** megjelenítéséhez a **biztonsági mentések** panelen.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-116">On the **Settings** blade of your app in the Azure Portal, click **Backups** to display the **Backups** blade.</span></span> <span data-ttu-id="6ef4a-117">Kattintson a **visszaállítása**.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-117">Then click **Restore**.</span></span>
   
    ![Válassza ki a visszaállítási most][ChooseRestoreNow]
2. <span data-ttu-id="6ef4a-119">Az a **visszaállítása** panelen, először válassza ki a biztonsági mentési forrását.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-119">In the **Restore** blade, first select the backup source.</span></span>
   
    ![](./media/web-sites-restore/021ChooseSource1.png)
   
    <span data-ttu-id="6ef4a-120">A **alkalmazás biztonsági mentési** beállítást választja, megjelenik az összes meglévő biztonsági mentését az aktuális alkalmazás, és egyszerűen kiválaszthatja az egyik.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-120">The **App backup** option shows you all the existing backups of the current app, and you can easily select one.</span></span>
    <span data-ttu-id="6ef4a-121">A **tárolási** beállítás lehetővé teszi bármely biztonsági mentési ZIP-fájl válasszon a meglévő Azure Storage-fiók és tároló az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-121">The **Storage** option lets you select any backup ZIP file from any existing Azure Storage account and container in your subscription.</span></span>
    <span data-ttu-id="6ef4a-122">Ha próbál biztonsági másolatát egy másik alkalmazás használja a **tárolási** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-122">If you're trying to restore a backup of another app, use the **Storage** option.</span></span>
3. <span data-ttu-id="6ef4a-123">Ezt követően adja meg az alkalmazás visszaállításához **visszaállítási célhelyének ellenőrzése**.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-123">Then, specify the destination for the app restore in **Restore destination**.</span></span>
   
    ![](./media/web-sites-restore/022ChooseDestination1.png)
   
   > [!WARNING]
   > <span data-ttu-id="6ef4a-124">Ha úgy dönt, **felülírása**, az összes meglévő adatok az aktuális alkalmazás törlése, és írja felül.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-124">If you choose **Overwrite**, all existing data in your current app is erased and overwritten.</span></span> <span data-ttu-id="6ef4a-125">Kattintás előtt **OK**, győződjön meg arról, hogy pontosan mit kíván tenni.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-125">Before you click **OK**, make sure that it is exactly what you want to do.</span></span>
   > 
   > 
   
    <span data-ttu-id="6ef4a-126">Kiválaszthatja **meglévő App** alkalmazás biztonsági másolat visszaállítása a azonos resoure csoport egy másik alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-126">You can select **Existing App** to restore the app backup to another app in the same resoure group.</span></span> <span data-ttu-id="6ef4a-127">Mielőtt ezt a beállítást használja, kell már létrehozott egy másik alkalmazás a tükrözési adatbázis konfigurációja egy alkalmazás biztonsági mentési definiálva az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-127">Before you use this option, you should have already created another app in your resource group with mirroring database configuration to the one defined in the app backup.</span></span> <span data-ttu-id="6ef4a-128">Létrehozhat egy **új** app visszaállítani a tartalmat.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-128">You can also Create a **New** app to restore your content to.</span></span>

4. <span data-ttu-id="6ef4a-129">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-129">Click **OK**.</span></span>

<a name="StorageAccount"></a>

## <a name="download-or-delete-a-backup-from-a-storage-account"></a><span data-ttu-id="6ef4a-130">Töltse le, illetve törölhet egy biztonsági mentés a storage-fiók</span><span class="sxs-lookup"><span data-stu-id="6ef4a-130">Download or delete a backup from a storage account</span></span>
1. <span data-ttu-id="6ef4a-131">A fő **Tallózás** panel az Azure portálon, válassza a **tárfiókok**.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-131">From the main **Browse** blade of the Azure portal, select **Storage accounts**.</span></span> <span data-ttu-id="6ef4a-132">A meglévő tárfiókok listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-132">A list of your existing storage accounts is displayed.</span></span>
2. <span data-ttu-id="6ef4a-133">Válassza ki a kívánt letöltheti és törölheti a biztonsági másolatot tartalmazó tárfiókot. A storage-fiók panelen jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-133">Select the storage account that contains the backup that you want to download or delete.The blade for the storage account is displayed.</span></span>
3. <span data-ttu-id="6ef4a-134">A storage-fiók panelen válassza ki a kívánt tároló</span><span class="sxs-lookup"><span data-stu-id="6ef4a-134">In the storage account blade, select the container you want</span></span>
   
    ![Nézet tárolók][ViewContainers]
4. <span data-ttu-id="6ef4a-136">Válassza ki a biztonságimásolat-fájl letöltése vagy törölni szeretné.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-136">Select backup file you want to download or delete.</span></span>
   
    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)
5. <span data-ttu-id="6ef4a-138">Kattintson a **letöltése** vagy **törlése** attól függően, hogy mit kíván tenni.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-138">Click **Download** or **Delete** depending on what you want to do.</span></span>  

<a name="OperationLogs"></a>

## <a name="monitor-a-restore-operation"></a><span data-ttu-id="6ef4a-139">A figyelő a visszaállítási művelet</span><span class="sxs-lookup"><span data-stu-id="6ef4a-139">Monitor a restore operation</span></span>
<span data-ttu-id="6ef4a-140">A sikeres vagy sikertelen volt-e az alkalmazás-visszaállítási művelet részleteinek megtekintéséhez lépjen a **tevékenységnapló** panel az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-140">To see details about the success or failure of the app restore operation, navigate to the **Activity Log** blade in the Azure portal.</span></span>  
 

<span data-ttu-id="6ef4a-141">Megtekintéséhez görgessen le a kívánt visszaállítási műveletet, és kattintással jelölje ki azt.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-141">Scroll down to find the desired restore operation and click to select it.</span></span>

<span data-ttu-id="6ef4a-142">A részleteket tartalmazó panelt jeleníti meg a rendelkezésre álló információkat a visszaállítási művelethez kapcsolódó.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-142">The details blade displays the available information related to the restore operation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6ef4a-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6ef4a-143">Next Steps</span></span>
<span data-ttu-id="6ef4a-144">Biztonsági mentéshez, és állítsa vissza az App Service apps REST API használatával (lásd: [biztonsági mentése és visszaállítása az App Service apps használata REST](websites-csm-backup.md)).</span><span class="sxs-lookup"><span data-stu-id="6ef4a-144">You can backup and restore App Service apps using REST API (see [Use REST to back up and restore App Service apps](websites-csm-backup.md)).</span></span>


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
