---
title: "aaaAzure Analysis Services-adatbázis biztonsági mentése és visszaállítása |} Microsoft Docs"
description: "Ismerteti, hogyan toobackup és visszaállítása egy Azure Analysis Services adatbázis."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: cf0a782d237a95fdfa5ef628f998bd053aac0d9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="backup-and-restore"></a><span data-ttu-id="29f22-103">Biztonsági mentés és visszaállítás</span><span class="sxs-lookup"><span data-stu-id="29f22-103">Backup and restore</span></span>

<span data-ttu-id="29f22-104">Az Azure Analysis Services táblázatos modell adatbázisainak biztonsági mentése, sokkal hello ugyanaz, mint a helyszíni Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="29f22-104">Backing up tabular model databases in Azure Analysis Services is much hello same as for on-premises Analysis Services.</span></span> <span data-ttu-id="29f22-105">hello elsődleges különbség az, ahol a biztonsági mentési fájljait tárolja.</span><span class="sxs-lookup"><span data-stu-id="29f22-105">hello primary difference is where you store your backup files.</span></span> <span data-ttu-id="29f22-106">A biztonságimásolat-fájlok tooa tárolóhoz kell menteni egy [Azure storage-fiók](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="29f22-106">Backup files must be saved tooa container in an [Azure storage account](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="29f22-107">Használhatja a tárfiók és tároló már rendelkezik, vagy létrehozása a kiszolgáló tárolási beállításainak konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="29f22-107">You can use a storage account and container you already have, or they can be created when configuring storage settings for your server.</span></span>

> [!NOTE]
> <span data-ttu-id="29f22-108">A storage-fiók létrehozása egy új számlázható szolgáltatás létrejöttét eredményezheti.</span><span class="sxs-lookup"><span data-stu-id="29f22-108">Creating a storage account can result in a new billable service.</span></span> <span data-ttu-id="29f22-109">több, lásd: toolearn [Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/storage/blobs/).</span><span class="sxs-lookup"><span data-stu-id="29f22-109">toolearn more, see [Azure Storage Pricing](https://azure.microsoft.com/pricing/details/storage/blobs/).</span></span>
> 
> 

<span data-ttu-id="29f22-110">Biztonsági másolatok abf-fájl kiterjesztéssel együtt.</span><span class="sxs-lookup"><span data-stu-id="29f22-110">Backups are saved with an abf extension.</span></span> <span data-ttu-id="29f22-111">A memóriában táblázatos modellek esetében a modell adatai és a metaadatok tárolja.</span><span class="sxs-lookup"><span data-stu-id="29f22-111">For in-memory tabular models, both model data and metadata are stored.</span></span> <span data-ttu-id="29f22-112">DirectQuery táblázatos modellek esetében csak a modell metaadatait tárolja.</span><span class="sxs-lookup"><span data-stu-id="29f22-112">For DirectQuery tabular models, only model metadata is stored.</span></span> <span data-ttu-id="29f22-113">Biztonsági mentések tömörített, és attól függően, hogy a kiválasztott lehetőségektől hello titkosított.</span><span class="sxs-lookup"><span data-stu-id="29f22-113">Backups can be compressed and encrypted, depending on hello options you choose.</span></span> 



## <a name="configure-storage-settings"></a><span data-ttu-id="29f22-114">A tárolási beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="29f22-114">Configure storage settings</span></span>
<span data-ttu-id="29f22-115">A biztonsági másolatot, előtt tooconfigure tárolási beállítások a kiszolgáló szükséges.</span><span class="sxs-lookup"><span data-stu-id="29f22-115">Before backing up, you need tooconfigure storage settings for your server.</span></span>


### <a name="tooconfigure-storage-settings"></a><span data-ttu-id="29f22-116">tooconfigure tárolási beállításai</span><span class="sxs-lookup"><span data-stu-id="29f22-116">tooconfigure storage settings</span></span>
1.  <span data-ttu-id="29f22-117">Az Azure portál > **beállítások**, kattintson a **biztonsági mentés**.</span><span class="sxs-lookup"><span data-stu-id="29f22-117">In Azure portal > **Settings**, click **Backup**.</span></span>

    ![Beállítások a biztonsági másolatok](./media/analysis-services-backup/aas-backup-backups.png)

2.  <span data-ttu-id="29f22-119">Kattintson a **engedélyezve**, majd kattintson a **tárolási beállítások**.</span><span class="sxs-lookup"><span data-stu-id="29f22-119">Click **Enabled**, then click **Storage Settings**.</span></span>

    ![Bekapcsolás](./media/analysis-services-backup/aas-backup-enable.png)

3. <span data-ttu-id="29f22-121">Válassza ki a tárfiók, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="29f22-121">Select your storage account or create a new one.</span></span>

4. <span data-ttu-id="29f22-122">Jelöljön ki egy tárolót, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="29f22-122">Select a container or create a new one.</span></span>

    ![Válassza ki a tárolót](./media/analysis-services-backup/aas-backup-container.png)

5. <span data-ttu-id="29f22-124">A biztonsági mentési beállítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="29f22-124">Save your backup settings.</span></span>

    ![Biztonsági mentési beállításainak mentése](./media/analysis-services-backup/aas-backup-save.png)

## <a name="backup"></a><span data-ttu-id="29f22-126">Biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="29f22-126">Backup</span></span>

### <a name="toobackup-by-using-ssms"></a><span data-ttu-id="29f22-127">toobackup szolgáltatáshoz az SSMS használatával</span><span class="sxs-lookup"><span data-stu-id="29f22-127">toobackup by using SSMS</span></span>

1. <span data-ttu-id="29f22-128">Az SSMS, kattintson a jobb gombbal egy adatbázis > **készítsen biztonsági másolatot**.</span><span class="sxs-lookup"><span data-stu-id="29f22-128">In SSMS, right-click a database > **Back Up**.</span></span>

2. <span data-ttu-id="29f22-129">A **adatbázis biztonsági másolata** > **biztonságimásolat-fájl**, kattintson a **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="29f22-129">In **Backup Database** > **Backup file**, click **Browse**.</span></span>

3. <span data-ttu-id="29f22-130">A hello **mentés fájlt** párbeszédpanelen ellenőrizze hello mappa elérési útját, és írja be a hello biztonságimásolat-fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="29f22-130">In hello **Save file as** dialog, verify hello folder path, and then type a name for hello backup file.</span></span> 

4. <span data-ttu-id="29f22-131">A hello **adatbázis biztonsági másolata** párbeszédpanelen válassza a beállítások.</span><span class="sxs-lookup"><span data-stu-id="29f22-131">In hello **Backup Database** dialog, select options.</span></span>

    <span data-ttu-id="29f22-132">**Engedélyezi a fájl felülírása** -válassza ezt a beállítást toooverwrite biztonságimásolat-fájlok hello ugyanazzal a névvel.</span><span class="sxs-lookup"><span data-stu-id="29f22-132">**Allow file overwrite** - Select this option toooverwrite backup files of hello same name.</span></span> <span data-ttu-id="29f22-133">Ha ez a beállítás nincs bejelölve, hello fájlt ment hello ugyanaz a neve nem lehet egy már létező fájlt a hello azonos helyen.</span><span class="sxs-lookup"><span data-stu-id="29f22-133">If this option is not selected, hello file you are saving cannot have hello same name as a file that already exists in hello same location.</span></span>

    <span data-ttu-id="29f22-134">**Tömörítés alkalmazása** -ezt a beállítást toocompress hello a biztonsági mentési fájlt.</span><span class="sxs-lookup"><span data-stu-id="29f22-134">**Apply compression** - Select this option toocompress hello backup file.</span></span> <span data-ttu-id="29f22-135">Tömörített biztonságimásolat-fájlok helymegtakarítás, de valamivel nagyobb processzorhasználatot igényel.</span><span class="sxs-lookup"><span data-stu-id="29f22-135">Compressed backup files save disk space, but require slightly higher CPU utilization.</span></span> 

    <span data-ttu-id="29f22-136">**Biztonsági másolat titkosításához** -ezt a beállítást tooencrypt hello a biztonsági mentési fájlt.</span><span class="sxs-lookup"><span data-stu-id="29f22-136">**Encrypt backup file** - Select this option tooencrypt hello backup file.</span></span> <span data-ttu-id="29f22-137">Ehhez szükség van a felhasználó által megadott jelszó toosecure hello biztonsági másolatból.</span><span class="sxs-lookup"><span data-stu-id="29f22-137">This option requires a user-supplied password toosecure hello backup file.</span></span> <span data-ttu-id="29f22-138">hello jelszó megakadályozza, hogy bármilyen más módon, mint a visszaállítási művelet hello biztonsági mentési adatok olvasásakor.</span><span class="sxs-lookup"><span data-stu-id="29f22-138">hello password prevents reading of hello backup data any other means than a restore operation.</span></span> <span data-ttu-id="29f22-139">Ha tooencrypt biztonsági mentések, hello jelszó biztonságos helyen tárolja.</span><span class="sxs-lookup"><span data-stu-id="29f22-139">If you choose tooencrypt backups, store hello password in a safe location.</span></span>

5. <span data-ttu-id="29f22-140">Kattintson a **OK** toocreate és hello biztonságimásolat-fájl mentése.</span><span class="sxs-lookup"><span data-stu-id="29f22-140">Click **OK** toocreate and save hello backup file.</span></span>


### <a name="powershell"></a><span data-ttu-id="29f22-141">PowerShell</span><span class="sxs-lookup"><span data-stu-id="29f22-141">PowerShell</span></span>
<span data-ttu-id="29f22-142">Használjon [Backup-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="29f22-142">Use [Backup-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet) cmdlet.</span></span>

## <a name="restore"></a><span data-ttu-id="29f22-143">Visszaállítás</span><span class="sxs-lookup"><span data-stu-id="29f22-143">Restore</span></span>
<span data-ttu-id="29f22-144">Visszaállításakor, a biztonságimásolat-fájlt úgy konfigurálta, a kiszolgáló hello tárfiókban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="29f22-144">When restoring, your backup file must be in hello storage account you've configured for your server.</span></span> <span data-ttu-id="29f22-145">Ha egy biztonságimásolat-fájlt egy helyszíni hely tooyour tárkonfigurációt toomove van szüksége, [Microsoft Azure Tártallózó](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) vagy hello [AzCopy](../storage/common/storage-use-azcopy.md) parancssori segédprogram.</span><span class="sxs-lookup"><span data-stu-id="29f22-145">If you need toomove a backup file from an on-premises location tooyour storage account, use [Microsoft Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) or hello [AzCopy](../storage/common/storage-use-azcopy.md) command-line utility.</span></span> 



> [!NOTE]
> <span data-ttu-id="29f22-146">Ha egy helyi kiszolgálóról most visszaállítása, távolítsa el hello minden tartományi felhasználó hello modell szerepkörből, majd adja hozzá toohello szerepkörök Azure Active Directory-felhasználók biztonsági.</span><span class="sxs-lookup"><span data-stu-id="29f22-146">If you're restoring from an on-premises server, you must remove all hello domain users from hello model's roles and add them back toohello roles as Azure Active Directory users.</span></span>
> 
> 

### <a name="toorestore-by-using-ssms"></a><span data-ttu-id="29f22-147">toorestore szolgáltatáshoz az SSMS használatával</span><span class="sxs-lookup"><span data-stu-id="29f22-147">toorestore by using SSMS</span></span>

1. <span data-ttu-id="29f22-148">Az SSMS, kattintson a jobb gombbal egy adatbázis > **visszaállítása**.</span><span class="sxs-lookup"><span data-stu-id="29f22-148">In SSMS, right-click a database > **Restore**.</span></span>

2. <span data-ttu-id="29f22-149">A hello **adatbázis biztonsági másolata** párbeszédpanelen, a **biztonságimásolat-fájl**, kattintson a **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="29f22-149">In hello **Backup Database** dialog, in **Backup file**, click **Browse**.</span></span>

3. <span data-ttu-id="29f22-150">A hello **található adatbázisfájlok** párbeszédpanelen toorestore kívánt válassza hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="29f22-150">In hello **Locate Database Files** dialog, select hello file you want toorestore.</span></span>

4. <span data-ttu-id="29f22-151">A **vissza az adatbázist a**, jelölje be hello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="29f22-151">In **Restore database**, select hello database.</span></span>

5. <span data-ttu-id="29f22-152">Adja meg a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="29f22-152">Specify options.</span></span> <span data-ttu-id="29f22-153">Biztonsági beállítások hello biztonsági mentésekor használt biztonsági mentési beállításainak egyeznie kell.</span><span class="sxs-lookup"><span data-stu-id="29f22-153">Security options must match hello backup options you used when backing up.</span></span>


### <a name="powershell"></a><span data-ttu-id="29f22-154">PowerShell</span><span class="sxs-lookup"><span data-stu-id="29f22-154">PowerShell</span></span>

<span data-ttu-id="29f22-155">Használjon [visszaállítási-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="29f22-155">Use [Restore-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet) cmdlet.</span></span>


## <a name="related-information"></a><span data-ttu-id="29f22-156">Kapcsolódó információk</span><span class="sxs-lookup"><span data-stu-id="29f22-156">Related information</span></span>

[<span data-ttu-id="29f22-157">Az Azure storage-fiókok</span><span class="sxs-lookup"><span data-stu-id="29f22-157">Azure storage accounts</span></span>](../storage/common/storage-create-storage-account.md)  
<span data-ttu-id="29f22-158">[Magas rendelkezésre állás](analysis-services-bcdr.md)   </span><span class="sxs-lookup"><span data-stu-id="29f22-158">[High availability](analysis-services-bcdr.md)   </span></span>  
[<span data-ttu-id="29f22-159">Az Azure Analysis Services kezelése</span><span class="sxs-lookup"><span data-stu-id="29f22-159">Manage Azure Analysis Services</span></span>](analysis-services-manage.md)
