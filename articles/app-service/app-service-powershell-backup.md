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
# <a name="use-powershell-tooback-up-and-restore-app-service-apps"></a>PowerShell tooback használja, és állítsa vissza az App Service-alkalmazásokhoz
> [!div class="op_single_selector"]
> * [PowerShell](app-service-powershell-backup.md)
> * [REST API](../app-service-web/websites-csm-backup.md)
> 
> 

Megtudhatja, hogyan toouse Azure PowerShell tooback mentését és visszaállítását [App Service apps](https://azure.microsoft.com/services/app-service/web/). További információ a webes alkalmazás biztonsági mentést, beleértve a követelményeket és korlátozásokat, lásd: [készítsen biztonsági másolatot egy webalkalmazást az Azure App Service](../app-service-web/web-sites-backup.md).

## <a name="prerequisites"></a>Előfeltételek
toouse PowerShell toomanage az alkalmazás biztonsági mentést, a következő hello szüksége:

* **A SAS URL-cím** , amely lehetővé teszi, hogy olvasási és írási hozzáférés tooan Azure Storage tárolót. Lásd: [ismertetése hello SAS-modell](../storage/common/storage-dotnet-shared-access-signature-part-1.md) az SAS URL-címek magyarázatot. Lásd: [Azure PowerShell használata az Azure Storage](../storage/common/storage-powershell-guide-full.md) példákat kezelése az Azure Storage PowerShell használatával.
* **Adatbázis-kapcsolati karakterlánc** Ha azt szeretné, tooback adatbázis és a webes alkalmazást.

### <a name="how-toogenerate-a-sas-url-toouse-with-hello-web-app-backup-cmdlets"></a>Hogyan toogenerate egy SAS URL-cím toouse hello webes alkalmazás biztonsági mentése parancsmagokkal
SAS URL-címet a PowerShell használatával hozhatók létre. Íme egy példa hogyan toogenerate egy hello parancsmagokkal használható cikkben említett.

        $storageAccountName = "<your storage account's name>"
        $storageAccountRg = "<your storage account's resource group>"

        # This returns an array of keys for your storage account. Be sure tooselect hello appropriate key. Here we select hello first key as a default.
        $storageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccountRg -Name $storageAccountName
        $context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey[0].Value

        $blobContainerName = "<name of blob container for app backups>"
        $sasUrl = New-AzureStorageContainerSASToken -Name $blobContainerName -Permission rwdl -Context $context -ExpiryTime (Get-Date).AddMonths(1) -FullUri

## <a name="install-azure-powershell-132-or-greater"></a>Telepítse az Azure PowerShell 1.3.2 vagy gyorsabb
Lásd: [az Azure PowerShell használata Azure Resource Managerrel](/powershell/azure/overview) telepítése és az Azure PowerShell használatával.

## <a name="create-a-backup"></a>Biztonsági mentés létrehozása
Új-AzureRmWebAppBackup hello parancsmag toocreate webalkalmazás biztonsági másolatot használja.

        $sasUrl = "<your SAS URL>"
        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl

Ezzel másolatot hoz létre egy automatikusan létrehozott névvel. Ha szeretné, hogy egy nevet a biztonsági másolat tooprovide, használja a hello BiztonságiMásolatNeve nem kötelező paraméter.

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl -BackupName MyBackup

tooinclude egy adatbázis biztonsági mentése hello, először hozzon létre új-AzureRmWebAppDatabaseBackupSetting parancsmaggal hello adatbázis biztonsági mentési beállítást, majd adja meg, hogy a hello adatbázisok paraméter beállítását hello New-AzureRmWebAppBackup parancsmag. hello adatbázisok paramétert fogad adatbázis beállításait, így több adatbázis tooback tömbjét.

        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbBackup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupName MyBackup -StorageAccountUrl $sasUrl -Databases $dbSetting1,$dbSetting2

## <a name="get-backups"></a>Biztonsági mentések beolvasása
hello Get-AzureRmWebAppBackupList parancsmag az összes biztonsági mentés egy webalkalmazás tömbjét adja vissza. Meg kell adnia a hello webalkalmazás hello nevét és az erőforráscsoportot.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $backups = Get-AzureRmWebAppBackupList -Name $appName -ResourceGroupName $resourceGroupName

a biztonsági másolat, tooget hello Get-AzureRmWebAppBackup parancsmaggal.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102

Egy webes alkalmazás objektum is átadható a hello biztonságimásolat-felügyeleti parancsmagokat kényelmi célokat szolgál.

        $app = Get-AzureRmWebApp -Name ContosoApp -ResourceGroupName Default-Web-WestUS
        $backupList = $app | Get-AzureRmWebAppBackupList
        $backup = $app | Get-AzureRmWebAppBackup -BackupId 10102

## <a name="schedule-automatic-backups"></a>Az automatikus biztonsági mentés ütemezése
Biztonsági mentések toohappen automatikusan ütemezhet a megadott időközönként. a biztonsági mentés ütemezését tooconfigure hello Szerkesztés-AzureRmWebAppBackupConfiguration parancsmaggal. Ez a parancsmag több paramétereket fogadja:

* **Név** – hello hello webes alkalmazás neve.
* **ResourceGroupName** – hello hello erőforrás csoportot tartalmazó hello webes alkalmazás neve.
* **Tárolóhely** – nem kötelező. hello webes tárolóhelye hello neve.
* **StorageAccountUrl** -SAS URL-cím hello hello Azure Storage tárolót használja a toostore hello biztonsági mentéseket.
* **FrequencyInterval** -milyen gyakran hello biztonsági mentést kell tenni a numerikus értéket. Pozitív egész számnak kell lennie.
* **FrequencyUnit** -egysége milyen gyakran hello biztonsági mentést kell tenni. A beállítások esetén órára és napra.
* **RetentionPeriodInDays** - hány nap hello automatikus biztonsági mentésekhez mentésére előtt automatikusan törlődnek.
* **StartTime** – nem kötelező. hello idő, mikor kezdjen hello automatikus biztonsági mentésekhez. Biztonsági mentések azonnal megkezdődik, ha ennek értéke null. Egy DateTime kell lennie.
* **Adatbázisok** – nem kötelező. Tömbje DatabaseBackupSettings hello adatbázisok toobackup számára.
* **KeepAtLeastOneBackup** – nem kötelező paraméter kapcsolni. Adjon meg, ez egy biztonsági másolat kell mindig meg kell őrizni, ha hello tárfiókot, függetlenül attól, hogy mennyi legyen.

Az alábbiakban látható egy példa bemutatja, hogyan toouse ennek a parancsmagnak.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Edit-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName -Slot $slotName `
          -StorageAccountUrl "<your SAS URL>" -FrequencyInterval 6 -FrequencyUnit Hour -Databases $dbSetting1,$dbSetting2 `
          -KeepAtLeastOneBackup -StartTime (Get-Date).AddHours(1)

tooget hello aktuális biztonsági mentési ütemezést, használja a Get-AzureRmWebAppBackupConfiguration hello parancsmagot. Ez lehet hasznos, ha módosítja egy ütemezést, amely már be van állítva.

        $configuration = Get-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName

        # Modify hello configuration slightly
        $configuration.FrequencyInterval = 2
        $configuration.FrequencyUnit = "Day"

        # Apply hello new configuration by piping it into hello Edit-AzureRmWebAppBackupConfiguration cmdlet
        $configuration | Edit-AzureRmWebAppBackupConfiguration

## <a name="restore-a-web-app-from-a-backup"></a>A webes alkalmazás visszaállítása biztonsági másolatból
a webes alkalmazás, egy biztonsági másolatból, használja a Restore-AzureRmWebAppBackup hello parancsmagot toorestore. hello legegyszerűbb módja toouse Ez a parancsmag annak toopipe hello Get-AzureRmWebAppBackup parancsmag vagy a Get-AzureRmWebAppBackupList parancsmag biztonsági mentési objektumban.

Miután egy biztonsági mentési objektum, a hello visszaállítási-AzureRmWebAppBackup parancsmag is átadhatja. Adja meg a hello felülírása kapcsoló paraméter tooindicate, hogy szeretné-e toooverwrite hello tartalmak webalkalmazás hello biztonsági mentés hello tartalmát. Ha hello biztonsági mentés adatbázist tartalmaz, ezeket az adatbázisokat vissza is.

        $backup | Restore-AzureRmWebAppBackup -Overwrite

Az alábbiakban látható egy példa hogyan toouse hello összes hello paraméter megadásával visszaállítási-AzureRmWebAppBackup.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $blobName = "ContosoBackup.zip"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Restore-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -Slot $slotName -StorageAccountUrl "<your SAS URL>" -BlobName $blobName -Databases $dbSetting1,$dbSetting2 -Overwrite

## <a name="delete-a-backup"></a>Biztonsági
toodelete biztonsági másolatot, használja a Remove-AzureRmWebAppBackup hello parancsmagot. Ezzel eltávolítja a tárfiók hello biztonsági mentés. Adja meg az alkalmazás nevét, az erőforráscsoport és hello azonosítója hello toodelete kívánt biztonsági mentés.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        Remove-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupId 10102

A Remove-AzureRmWebAppBackup hello parancsmag toodelete is átadható a biztonsági mentési objektum azt.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102
        $backup | Remove-AzureRmWebAppBackup -Overwrite
