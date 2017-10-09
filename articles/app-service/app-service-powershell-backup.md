---
title: "aaaUse PowerShell tooback össze, és állítsa vissza az App Service-alkalmazásokhoz"
description: "Megtudhatja, hogyan toouse PowerShell tooback össze, és az Azure App Service alkalmazás visszaállítása"
services: app-service
documentationcenter: 
author: NKing92
manager: erikre
editor: 
ms.assetid: 7ea8661e-aefb-4823-9626-6bff980cdebf
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2016
ms.author: nicking
ms.openlocfilehash: 4042166f6c650841926f010056d6c80ab2de57e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-tooback-up-and-restore-app-service-apps"></a><span data-ttu-id="8ea11-103">PowerShell tooback használja, és állítsa vissza az App Service-alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="8ea11-103">Use PowerShell tooback up and restore App Service apps</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="8ea11-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8ea11-104">PowerShell</span></span>](app-service-powershell-backup.md)
> * [<span data-ttu-id="8ea11-105">REST API</span><span class="sxs-lookup"><span data-stu-id="8ea11-105">REST API</span></span>](../app-service-web/websites-csm-backup.md)
> 
> 

<span data-ttu-id="8ea11-106">Megtudhatja, hogyan toouse Azure PowerShell tooback mentését és visszaállítását [App Service apps](https://azure.microsoft.com/services/app-service/web/).</span><span class="sxs-lookup"><span data-stu-id="8ea11-106">Learn how toouse Azure PowerShell tooback up and restore [App Service apps](https://azure.microsoft.com/services/app-service/web/).</span></span> <span data-ttu-id="8ea11-107">További információ a webes alkalmazás biztonsági mentést, beleértve a követelményeket és korlátozásokat, lásd: [készítsen biztonsági másolatot egy webalkalmazást az Azure App Service](../app-service-web/web-sites-backup.md).</span><span class="sxs-lookup"><span data-stu-id="8ea11-107">For more information about web app backups, including requirements and restrictions, see [Back up a web app in Azure App Service](../app-service-web/web-sites-backup.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8ea11-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8ea11-108">Prerequisites</span></span>
<span data-ttu-id="8ea11-109">toouse PowerShell toomanage az alkalmazás biztonsági mentést, a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="8ea11-109">toouse PowerShell toomanage your app backups, you need hello following:</span></span>

* <span data-ttu-id="8ea11-110">**A SAS URL-cím** , amely lehetővé teszi, hogy olvasási és írási hozzáférés tooan Azure Storage tárolót.</span><span class="sxs-lookup"><span data-stu-id="8ea11-110">**A SAS URL** that allows read and write access tooan Azure Storage container.</span></span> <span data-ttu-id="8ea11-111">Lásd: [ismertetése hello SAS-modell](../storage/common/storage-dotnet-shared-access-signature-part-1.md) az SAS URL-címek magyarázatot.</span><span class="sxs-lookup"><span data-stu-id="8ea11-111">See [Understanding hello SAS model](../storage/common/storage-dotnet-shared-access-signature-part-1.md) for an explanation of SAS URLs.</span></span> <span data-ttu-id="8ea11-112">Lásd: [Azure PowerShell használata az Azure Storage](../storage/common/storage-powershell-guide-full.md) példákat kezelése az Azure Storage PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="8ea11-112">See [Using Azure PowerShell with Azure Storage](../storage/common/storage-powershell-guide-full.md) for examples of managing Azure Storage using PowerShell.</span></span>
* <span data-ttu-id="8ea11-113">**Adatbázis-kapcsolati karakterlánc** Ha azt szeretné, tooback adatbázis és a webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="8ea11-113">**A database connection string** if you want tooback up a database along with your web app.</span></span>

### <a name="how-toogenerate-a-sas-url-toouse-with-hello-web-app-backup-cmdlets"></a><span data-ttu-id="8ea11-114">Hogyan toogenerate egy SAS URL-cím toouse hello webes alkalmazás biztonsági mentése parancsmagokkal</span><span class="sxs-lookup"><span data-stu-id="8ea11-114">How toogenerate a SAS URL toouse with hello web app backup cmdlets</span></span>
<span data-ttu-id="8ea11-115">SAS URL-címet a PowerShell használatával hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="8ea11-115">A SAS URL can be generated with PowerShell.</span></span> <span data-ttu-id="8ea11-116">Íme egy példa hogyan toogenerate egy hello parancsmagokkal használható cikkben említett.</span><span class="sxs-lookup"><span data-stu-id="8ea11-116">Here is an example of how toogenerate one that can be used with hello cmdlets discussed in this article.</span></span>

        $storageAccountName = "<your storage account's name>"
        $storageAccountRg = "<your storage account's resource group>"

        # This returns an array of keys for your storage account. Be sure tooselect hello appropriate key. Here we select hello first key as a default.
        $storageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccountRg -Name $storageAccountName
        $context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey[0].Value

        $blobContainerName = "<name of blob container for app backups>"
        $sasUrl = New-AzureStorageContainerSASToken -Name $blobContainerName -Permission rwdl -Context $context -ExpiryTime (Get-Date).AddMonths(1) -FullUri

## <a name="install-azure-powershell-132-or-greater"></a><span data-ttu-id="8ea11-117">Telepítse az Azure PowerShell 1.3.2 vagy gyorsabb</span><span class="sxs-lookup"><span data-stu-id="8ea11-117">Install Azure PowerShell 1.3.2 or greater</span></span>
<span data-ttu-id="8ea11-118">Lásd: [az Azure PowerShell használata Azure Resource Managerrel](/powershell/azure/overview) telepítése és az Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="8ea11-118">See [Using Azure PowerShell with Azure Resource Manager](/powershell/azure/overview) for instructions on installing and using Azure PowerShell.</span></span>

## <a name="create-a-backup"></a><span data-ttu-id="8ea11-119">Biztonsági mentés létrehozása</span><span class="sxs-lookup"><span data-stu-id="8ea11-119">Create a backup</span></span>
<span data-ttu-id="8ea11-120">Új-AzureRmWebAppBackup hello parancsmag toocreate webalkalmazás biztonsági másolatot használja.</span><span class="sxs-lookup"><span data-stu-id="8ea11-120">Use hello New-AzureRmWebAppBackup cmdlet toocreate a backup of a web app.</span></span>

        $sasUrl = "<your SAS URL>"
        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl

<span data-ttu-id="8ea11-121">Ezzel másolatot hoz létre egy automatikusan létrehozott névvel.</span><span class="sxs-lookup"><span data-stu-id="8ea11-121">This creates a backup with an automatically generated name.</span></span> <span data-ttu-id="8ea11-122">Ha szeretné, hogy egy nevet a biztonsági másolat tooprovide, használja a hello BiztonságiMásolatNeve nem kötelező paraméter.</span><span class="sxs-lookup"><span data-stu-id="8ea11-122">If you would like tooprovide a name for your backup, use hello BackupName optional parameter.</span></span>

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl -BackupName MyBackup

<span data-ttu-id="8ea11-123">tooinclude egy adatbázis biztonsági mentése hello, először hozzon létre új-AzureRmWebAppDatabaseBackupSetting parancsmaggal hello adatbázis biztonsági mentési beállítást, majd adja meg, hogy a hello adatbázisok paraméter beállítását hello New-AzureRmWebAppBackup parancsmag.</span><span class="sxs-lookup"><span data-stu-id="8ea11-123">tooinclude a database in hello backup, first create a database backup setting using hello New-AzureRmWebAppDatabaseBackupSetting cmdlet, then supply that setting in hello Databases parameter of hello New-AzureRmWebAppBackup cmdlet.</span></span> <span data-ttu-id="8ea11-124">hello adatbázisok paramétert fogad adatbázis beállításait, így több adatbázis tooback tömbjét.</span><span class="sxs-lookup"><span data-stu-id="8ea11-124">hello Databases parameter accepts an array of database settings, allowing you tooback up more than one database.</span></span>

        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbBackup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupName MyBackup -StorageAccountUrl $sasUrl -Databases $dbSetting1,$dbSetting2

## <a name="get-backups"></a><span data-ttu-id="8ea11-125">Biztonsági mentések beolvasása</span><span class="sxs-lookup"><span data-stu-id="8ea11-125">Get backups</span></span>
<span data-ttu-id="8ea11-126">hello Get-AzureRmWebAppBackupList parancsmag az összes biztonsági mentés egy webalkalmazás tömbjét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8ea11-126">hello Get-AzureRmWebAppBackupList cmdlet returns an array of all backups for a web app.</span></span> <span data-ttu-id="8ea11-127">Meg kell adnia a hello webalkalmazás hello nevét és az erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="8ea11-127">You must supply hello name of hello web app and its resource group.</span></span>

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $backups = Get-AzureRmWebAppBackupList -Name $appName -ResourceGroupName $resourceGroupName

<span data-ttu-id="8ea11-128">a biztonsági másolat, tooget hello Get-AzureRmWebAppBackup parancsmaggal.</span><span class="sxs-lookup"><span data-stu-id="8ea11-128">tooget a specific backup, use hello Get-AzureRmWebAppBackup cmdlet.</span></span>

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102

<span data-ttu-id="8ea11-129">Egy webes alkalmazás objektum is átadható a hello biztonságimásolat-felügyeleti parancsmagokat kényelmi célokat szolgál.</span><span class="sxs-lookup"><span data-stu-id="8ea11-129">You can also pipe a web app object into any of hello backup management cmdlets for convenience.</span></span>

        $app = Get-AzureRmWebApp -Name ContosoApp -ResourceGroupName Default-Web-WestUS
        $backupList = $app | Get-AzureRmWebAppBackupList
        $backup = $app | Get-AzureRmWebAppBackup -BackupId 10102

## <a name="schedule-automatic-backups"></a><span data-ttu-id="8ea11-130">Az automatikus biztonsági mentés ütemezése</span><span class="sxs-lookup"><span data-stu-id="8ea11-130">Schedule automatic backups</span></span>
<span data-ttu-id="8ea11-131">Biztonsági mentések toohappen automatikusan ütemezhet a megadott időközönként.</span><span class="sxs-lookup"><span data-stu-id="8ea11-131">You can schedule backups toohappen automatically at a specified interval.</span></span> <span data-ttu-id="8ea11-132">a biztonsági mentés ütemezését tooconfigure hello Szerkesztés-AzureRmWebAppBackupConfiguration parancsmaggal.</span><span class="sxs-lookup"><span data-stu-id="8ea11-132">tooconfigure a backup schedule, use hello Edit-AzureRmWebAppBackupConfiguration cmdlet.</span></span> <span data-ttu-id="8ea11-133">Ez a parancsmag több paramétereket fogadja:</span><span class="sxs-lookup"><span data-stu-id="8ea11-133">This cmdlet takes several parameters:</span></span>

* <span data-ttu-id="8ea11-134">**Név** – hello hello webes alkalmazás neve.</span><span class="sxs-lookup"><span data-stu-id="8ea11-134">**Name** - hello name of hello web app.</span></span>
* <span data-ttu-id="8ea11-135">**ResourceGroupName** – hello hello erőforrás csoportot tartalmazó hello webes alkalmazás neve.</span><span class="sxs-lookup"><span data-stu-id="8ea11-135">**ResourceGroupName** - hello name of hello resource group containing hello web app.</span></span>
* <span data-ttu-id="8ea11-136">**Tárolóhely** – nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="8ea11-136">**Slot** - Optional.</span></span> <span data-ttu-id="8ea11-137">hello webes tárolóhelye hello neve.</span><span class="sxs-lookup"><span data-stu-id="8ea11-137">hello name of hello web app slot.</span></span>
* <span data-ttu-id="8ea11-138">**StorageAccountUrl** -SAS URL-cím hello hello Azure Storage tárolót használja a toostore hello biztonsági mentéseket.</span><span class="sxs-lookup"><span data-stu-id="8ea11-138">**StorageAccountUrl** - hello SAS URL for hello Azure Storage container used toostore hello backups.</span></span>
* <span data-ttu-id="8ea11-139">**FrequencyInterval** -milyen gyakran hello biztonsági mentést kell tenni a numerikus értéket.</span><span class="sxs-lookup"><span data-stu-id="8ea11-139">**FrequencyInterval** - Numeric value for how often hello backups should be made.</span></span> <span data-ttu-id="8ea11-140">Pozitív egész számnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="8ea11-140">Must be a positive integer.</span></span>
* <span data-ttu-id="8ea11-141">**FrequencyUnit** -egysége milyen gyakran hello biztonsági mentést kell tenni.</span><span class="sxs-lookup"><span data-stu-id="8ea11-141">**FrequencyUnit** - Unit of time for how often hello backups should be made.</span></span> <span data-ttu-id="8ea11-142">A beállítások esetén órára és napra.</span><span class="sxs-lookup"><span data-stu-id="8ea11-142">Options are Hour and Day.</span></span>
* <span data-ttu-id="8ea11-143">**RetentionPeriodInDays** - hány nap hello automatikus biztonsági mentésekhez mentésére előtt automatikusan törlődnek.</span><span class="sxs-lookup"><span data-stu-id="8ea11-143">**RetentionPeriodInDays** - How many days hello automatic backups should be saved before being automatically deleted.</span></span>
* <span data-ttu-id="8ea11-144">**StartTime** – nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="8ea11-144">**StartTime** - Optional.</span></span> <span data-ttu-id="8ea11-145">hello idő, mikor kezdjen hello automatikus biztonsági mentésekhez.</span><span class="sxs-lookup"><span data-stu-id="8ea11-145">hello time when hello automatic backups should begin.</span></span> <span data-ttu-id="8ea11-146">Biztonsági mentések azonnal megkezdődik, ha ennek értéke null.</span><span class="sxs-lookup"><span data-stu-id="8ea11-146">Backups begin immediately if this is null.</span></span> <span data-ttu-id="8ea11-147">Egy DateTime kell lennie.</span><span class="sxs-lookup"><span data-stu-id="8ea11-147">Must be a DateTime.</span></span>
* <span data-ttu-id="8ea11-148">**Adatbázisok** – nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="8ea11-148">**Databases** - Optional.</span></span> <span data-ttu-id="8ea11-149">Tömbje DatabaseBackupSettings hello adatbázisok toobackup számára.</span><span class="sxs-lookup"><span data-stu-id="8ea11-149">An array of DatabaseBackupSettings for hello databases toobackup.</span></span>
* <span data-ttu-id="8ea11-150">**KeepAtLeastOneBackup** – nem kötelező paraméter kapcsolni.</span><span class="sxs-lookup"><span data-stu-id="8ea11-150">**KeepAtLeastOneBackup** - Optional switched parameter.</span></span> <span data-ttu-id="8ea11-151">Adjon meg, ez egy biztonsági másolat kell mindig meg kell őrizni, ha hello tárfiókot, függetlenül attól, hogy mennyi legyen.</span><span class="sxs-lookup"><span data-stu-id="8ea11-151">Supply this if one backup should always be kept in hello storage account, regardless of how old it is.</span></span>

<span data-ttu-id="8ea11-152">Az alábbiakban látható egy példa bemutatja, hogyan toouse ennek a parancsmagnak.</span><span class="sxs-lookup"><span data-stu-id="8ea11-152">Following is an example of how toouse this cmdlet.</span></span>

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Edit-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName -Slot $slotName `
          -StorageAccountUrl "<your SAS URL>" -FrequencyInterval 6 -FrequencyUnit Hour -Databases $dbSetting1,$dbSetting2 `
          -KeepAtLeastOneBackup -StartTime (Get-Date).AddHours(1)

<span data-ttu-id="8ea11-153">tooget hello aktuális biztonsági mentési ütemezést, használja a Get-AzureRmWebAppBackupConfiguration hello parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="8ea11-153">tooget hello current backup schedule, use hello Get-AzureRmWebAppBackupConfiguration cmdlet.</span></span> <span data-ttu-id="8ea11-154">Ez lehet hasznos, ha módosítja egy ütemezést, amely már be van állítva.</span><span class="sxs-lookup"><span data-stu-id="8ea11-154">This can be useful for modifying a schedule that has already been configured.</span></span>

        $configuration = Get-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName

        # Modify hello configuration slightly
        $configuration.FrequencyInterval = 2
        $configuration.FrequencyUnit = "Day"

        # Apply hello new configuration by piping it into hello Edit-AzureRmWebAppBackupConfiguration cmdlet
        $configuration | Edit-AzureRmWebAppBackupConfiguration

## <a name="restore-a-web-app-from-a-backup"></a><span data-ttu-id="8ea11-155">A webes alkalmazás visszaállítása biztonsági másolatból</span><span class="sxs-lookup"><span data-stu-id="8ea11-155">Restore a web app from a backup</span></span>
<span data-ttu-id="8ea11-156">a webes alkalmazás, egy biztonsági másolatból, használja a Restore-AzureRmWebAppBackup hello parancsmagot toorestore.</span><span class="sxs-lookup"><span data-stu-id="8ea11-156">toorestore a web app from a backup, use hello Restore-AzureRmWebAppBackup cmdlet.</span></span> <span data-ttu-id="8ea11-157">hello legegyszerűbb módja toouse Ez a parancsmag annak toopipe hello Get-AzureRmWebAppBackup parancsmag vagy a Get-AzureRmWebAppBackupList parancsmag biztonsági mentési objektumban.</span><span class="sxs-lookup"><span data-stu-id="8ea11-157">hello easiest way toouse this cmdlet is toopipe in a backup object retrieved from hello Get-AzureRmWebAppBackup cmdlet or Get-AzureRmWebAppBackupList cmdlet.</span></span>

<span data-ttu-id="8ea11-158">Miután egy biztonsági mentési objektum, a hello visszaállítási-AzureRmWebAppBackup parancsmag is átadhatja.</span><span class="sxs-lookup"><span data-stu-id="8ea11-158">Once you have a backup object, you can pipe it into hello Restore-AzureRmWebAppBackup cmdlet.</span></span> <span data-ttu-id="8ea11-159">Adja meg a hello felülírása kapcsoló paraméter tooindicate, hogy szeretné-e toooverwrite hello tartalmak webalkalmazás hello biztonsági mentés hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="8ea11-159">Specify hello Overwrite switch parameter tooindicate that you intend toooverwrite hello contents of your web app with hello contents of hello backup.</span></span> <span data-ttu-id="8ea11-160">Ha hello biztonsági mentés adatbázist tartalmaz, ezeket az adatbázisokat vissza is.</span><span class="sxs-lookup"><span data-stu-id="8ea11-160">If hello backup contains databases, those databases are restored as well.</span></span>

        $backup | Restore-AzureRmWebAppBackup -Overwrite

<span data-ttu-id="8ea11-161">Az alábbiakban látható egy példa hogyan toouse hello összes hello paraméter megadásával visszaállítási-AzureRmWebAppBackup.</span><span class="sxs-lookup"><span data-stu-id="8ea11-161">Following is an example of how toouse hello Restore-AzureRmWebAppBackup by specifying all hello parameters.</span></span>

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $blobName = "ContosoBackup.zip"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Restore-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -Slot $slotName -StorageAccountUrl "<your SAS URL>" -BlobName $blobName -Databases $dbSetting1,$dbSetting2 -Overwrite

## <a name="delete-a-backup"></a><span data-ttu-id="8ea11-162">Biztonsági</span><span class="sxs-lookup"><span data-stu-id="8ea11-162">Delete a backup</span></span>
<span data-ttu-id="8ea11-163">toodelete biztonsági másolatot, használja a Remove-AzureRmWebAppBackup hello parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="8ea11-163">toodelete a backup, use hello Remove-AzureRmWebAppBackup cmdlet.</span></span> <span data-ttu-id="8ea11-164">Ezzel eltávolítja a tárfiók hello biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="8ea11-164">This removes hello backup from your storage account.</span></span> <span data-ttu-id="8ea11-165">Adja meg az alkalmazás nevét, az erőforráscsoport és hello azonosítója hello toodelete kívánt biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="8ea11-165">Specify your app name, its resource group, and hello ID of hello backup you want toodelete.</span></span>

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        Remove-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupId 10102

<span data-ttu-id="8ea11-166">A Remove-AzureRmWebAppBackup hello parancsmag toodelete is átadható a biztonsági mentési objektum azt.</span><span class="sxs-lookup"><span data-stu-id="8ea11-166">You can also pipe a backup object into hello Remove-AzureRmWebAppBackup cmdlet toodelete it.</span></span>

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102
        $backup | Remove-AzureRmWebAppBackup -Overwrite
