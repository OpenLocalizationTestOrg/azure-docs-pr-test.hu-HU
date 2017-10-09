---
title: "biztonsági mentés SQL Server virtuális gépek (klasszikus) aaaAutomated |} Microsoft Docs"
description: "Ismerteti a hello automatikus biztonsági mentés szolgáltatás az SQL Server fut, az Azure virtuális gépek erőforrás-kezelő használatával. "
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 3333e830-8a60-42f5-9f44-8e02e9868d7b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.openlocfilehash: 5d8f0412578c2d86edc6e54093a5da4891d3847e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-classic"></a><span data-ttu-id="92d88-103">Automatikus biztonsági mentés az SQL Server Azure virtuális gépekben (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="92d88-103">Automated Backup for SQL Server in Azure Virtual Machines (Classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="92d88-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="92d88-104">Resource Manager</span></span>](../sql/virtual-machines-windows-sql-automated-backup.md)
> * [<span data-ttu-id="92d88-105">Klasszikus</span><span class="sxs-lookup"><span data-stu-id="92d88-105">Classic</span></span>](../classic/sql-automated-backup.md)
> 
> 

<span data-ttu-id="92d88-106">Automatikus biztonsági mentés automatikusan konfigurálja a [való felügyelt biztonsági mentésének tooMicrosoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) az összes meglévő és új adatbázis egy SQL Server 2014 Standard vagy Enterprise rendszert futtató Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="92d88-106">Automated Backup automatically configures [Managed Backup tooMicrosoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) for all existing and new databases on an Azure VM running SQL Server 2014 Standard or Enterprise.</span></span> <span data-ttu-id="92d88-107">Ez lehetővé teszi tooconfigure rendszeres biztonsági tartós Azure blob-tárolók.</span><span class="sxs-lookup"><span data-stu-id="92d88-107">This enables you tooconfigure regular database backups that utilize durable Azure blob storage.</span></span> <span data-ttu-id="92d88-108">Automatikus biztonsági mentés függ hello [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](../classic/sql-server-agent-extension.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="92d88-108">Automated Backup depends on hello [SQL Server IaaS Agent Extension](../classic/sql-server-agent-extension.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="92d88-109">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="92d88-109">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="92d88-110">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="92d88-110">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="92d88-111">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="92d88-111">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="92d88-112">tooview hello erőforrás-kezelő változata ebből a cikkből: [automatikus biztonsági mentés az SQL Server az Azure virtuális gépek erőforrás-kezelő](../sql/virtual-machines-windows-sql-automated-backup.md).</span><span class="sxs-lookup"><span data-stu-id="92d88-112">tooview hello Resource Manager version of this article, see [Automated Backup for SQL Server in Azure Virtual Machines Resource Manager](../sql/virtual-machines-windows-sql-automated-backup.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="92d88-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="92d88-113">Prerequisites</span></span>
<span data-ttu-id="92d88-114">toouse automatikus biztonsági mentés, vegye figyelembe a következő előfeltételek hello:</span><span class="sxs-lookup"><span data-stu-id="92d88-114">toouse Automated Backup, consider hello following prerequisites:</span></span>

<span data-ttu-id="92d88-115">**Operációs rendszer**:</span><span class="sxs-lookup"><span data-stu-id="92d88-115">**Operating System**:</span></span>

* <span data-ttu-id="92d88-116">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="92d88-116">Windows Server 2012</span></span>
* <span data-ttu-id="92d88-117">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="92d88-117">Windows Server 2012 R2</span></span>
* <span data-ttu-id="92d88-118">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="92d88-118">Windows Server 2016</span></span>

<span data-ttu-id="92d88-119">**SQL Server verziójához/kiadásához**:</span><span class="sxs-lookup"><span data-stu-id="92d88-119">**SQL Server version/edition**:</span></span>

* <span data-ttu-id="92d88-120">Az SQL Server 2014 Standard</span><span class="sxs-lookup"><span data-stu-id="92d88-120">SQL Server 2014 Standard</span></span>
* <span data-ttu-id="92d88-121">SQL Server 2014 Enterprise</span><span class="sxs-lookup"><span data-stu-id="92d88-121">SQL Server 2014 Enterprise</span></span>

> [!NOTE]
> <span data-ttu-id="92d88-122">SQL Server 2016 jelenleg nem támogatott az automatikus biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="92d88-122">SQL Server 2016 is not yet supported for Automated Backup.</span></span>
> 
> 

<span data-ttu-id="92d88-123">**Adatbázis-konfiguráció**:</span><span class="sxs-lookup"><span data-stu-id="92d88-123">**Database configuration**:</span></span>

* <span data-ttu-id="92d88-124">Cél adatbázisok hello teljes helyreállítási modell kell megadni.</span><span class="sxs-lookup"><span data-stu-id="92d88-124">Target databases must use hello full recovery model.</span></span>

<span data-ttu-id="92d88-125">**Az Azure PowerShell**:</span><span class="sxs-lookup"><span data-stu-id="92d88-125">**Azure PowerShell**:</span></span>

* <span data-ttu-id="92d88-126">[Telepítse a legújabb Azure PowerShell-parancsok hello](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="92d88-126">[Install hello latest Azure PowerShell commands](/powershell/azure/overview).</span></span>

<span data-ttu-id="92d88-127">**SQL Server IaaS bővítmény**:</span><span class="sxs-lookup"><span data-stu-id="92d88-127">**SQL Server IaaS Extension**:</span></span>

* <span data-ttu-id="92d88-128">[Telepítse az SQL Server IaaS bővítmény hello](../classic/sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="92d88-128">[Install hello SQL Server IaaS Extension](../classic/sql-server-agent-extension.md).</span></span>

## <a name="settings"></a><span data-ttu-id="92d88-129">Beállítások</span><span class="sxs-lookup"><span data-stu-id="92d88-129">Settings</span></span>
<span data-ttu-id="92d88-130">hello következő táblázat ismerteti, amelyek az automatikus biztonsági mentés hello-beállítások.</span><span class="sxs-lookup"><span data-stu-id="92d88-130">hello following table describes hello options that can be configured for Automated Backup.</span></span> <span data-ttu-id="92d88-131">Klasszikus virtuális gépekhez PowerShell tooconfigure ezeket a beállításokat kell használnia.</span><span class="sxs-lookup"><span data-stu-id="92d88-131">For classic VMs, you must use PowerShell tooconfigure these settings.</span></span>

| <span data-ttu-id="92d88-132">Beállítás</span><span class="sxs-lookup"><span data-stu-id="92d88-132">Setting</span></span> | <span data-ttu-id="92d88-133">Tartomány (alapértelmezett)</span><span class="sxs-lookup"><span data-stu-id="92d88-133">Range (Default)</span></span> | <span data-ttu-id="92d88-134">Leírás</span><span class="sxs-lookup"><span data-stu-id="92d88-134">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="92d88-135">**Automatikus biztonsági mentés**</span><span class="sxs-lookup"><span data-stu-id="92d88-135">**Automated Backup**</span></span> |<span data-ttu-id="92d88-136">Engedélyezi/letiltja (letiltva)</span><span class="sxs-lookup"><span data-stu-id="92d88-136">Enable/Disable (Disabled)</span></span> |<span data-ttu-id="92d88-137">Engedélyezi vagy letiltja az automatikus biztonsági mentés az SQL Server 2014 Standard vagy Enterprise rendszert futtató Azure virtuális gép esetében.</span><span class="sxs-lookup"><span data-stu-id="92d88-137">Enables or disables Automated Backup for an Azure VM running SQL Server 2014 Standard or Enterprise.</span></span> |
| <span data-ttu-id="92d88-138">**Megőrzési időtartam**</span><span class="sxs-lookup"><span data-stu-id="92d88-138">**Retention Period**</span></span> |<span data-ttu-id="92d88-139">1-30 nap (30 nap)</span><span class="sxs-lookup"><span data-stu-id="92d88-139">1-30 days (30 days)</span></span> |<span data-ttu-id="92d88-140">nap tooretain biztonsági hello száma.</span><span class="sxs-lookup"><span data-stu-id="92d88-140">hello number of days tooretain a backup.</span></span> |
| <span data-ttu-id="92d88-141">**Tárfiók**</span><span class="sxs-lookup"><span data-stu-id="92d88-141">**Storage Account**</span></span> |<span data-ttu-id="92d88-142">Azure storage-fiók (hello létrehozott tárfiókban hello a megadott virtuális gép)</span><span class="sxs-lookup"><span data-stu-id="92d88-142">Azure storage account (hello storage account created for hello specified VM)</span></span> |<span data-ttu-id="92d88-143">Egy Azure storage fiók toouse automatikus biztonsági mentés fájlok tárolásához a blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="92d88-143">An Azure storage account toouse for storing Automated Backup files in blob storage.</span></span> <span data-ttu-id="92d88-144">Egy tároló összes biztonságimásolat-fájl jön létre a hely toostore.</span><span class="sxs-lookup"><span data-stu-id="92d88-144">A container is created at this location toostore all backup files.</span></span> <span data-ttu-id="92d88-145">hello biztonságimásolat-fájl elnevezési konvenciója hello dátum, idő és a gép neve tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="92d88-145">hello backup file naming convention includes hello date, time, and machine name.</span></span> |
| <span data-ttu-id="92d88-146">**Titkosítás**</span><span class="sxs-lookup"><span data-stu-id="92d88-146">**Encryption**</span></span> |<span data-ttu-id="92d88-147">Engedélyezi/letiltja (letiltva)</span><span class="sxs-lookup"><span data-stu-id="92d88-147">Enable/Disable (Disabled)</span></span> |<span data-ttu-id="92d88-148">Engedélyezi vagy letiltja a titkosítást.</span><span class="sxs-lookup"><span data-stu-id="92d88-148">Enables or disables encryption.</span></span> <span data-ttu-id="92d88-149">Ha engedélyezve van a titkosítás, hello tanúsítványok használt toorestore hello biztonsági mentés található hello megadott tárfiók hello a azonos automaticbackup tárolót használó hello azonos elnevezési konvenciót.</span><span class="sxs-lookup"><span data-stu-id="92d88-149">When encryption is enabled, hello certificates used toorestore hello backup are located in hello specified storage account in hello same automaticbackup container using hello same naming convention.</span></span> <span data-ttu-id="92d88-150">Hello jelszó is módosul, ha új tanúsítványt hoz létre, hogy a jelszó, de a régi tanúsítvány hello marad toorestore korábbi biztonsági másolatok.</span><span class="sxs-lookup"><span data-stu-id="92d88-150">If hello password changes, a new certificate is generated with that password, but hello old certificate remains toorestore prior backups.</span></span> |
| <span data-ttu-id="92d88-151">**Jelszó**</span><span class="sxs-lookup"><span data-stu-id="92d88-151">**Password**</span></span> |<span data-ttu-id="92d88-152">Jelszó szöveg (nincs)</span><span class="sxs-lookup"><span data-stu-id="92d88-152">Password text (None)</span></span> |<span data-ttu-id="92d88-153">A titkosítási kulcsok jelszava.</span><span class="sxs-lookup"><span data-stu-id="92d88-153">A password for encryption keys.</span></span> <span data-ttu-id="92d88-154">Erre csak akkor van szükség, ha engedélyezve van-e a titkosítás.</span><span class="sxs-lookup"><span data-stu-id="92d88-154">This is only required if encryption is enabled.</span></span> <span data-ttu-id="92d88-155">A sorrend toorestore egy biztonsági másolat titkosításának engedélyezése, rendelkeznie kell hello helyes jelszót és a kapcsolódó hello a biztonsági mentés készült hello során használt tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="92d88-155">In order toorestore an encrypted backup, you must have hello correct password and related certificate that was used at hello time hello backup was taken.</span></span> | <span data-ttu-id="92d88-156">**Biztonsági mentési rendszer adatbázisok**</span><span class="sxs-lookup"><span data-stu-id="92d88-156">**Backup system databases**</span></span> | <span data-ttu-id="92d88-157">Engedélyezi/letiltja (letiltva)</span><span class="sxs-lookup"><span data-stu-id="92d88-157">Enable/Disable (Disabled)</span></span> | <span data-ttu-id="92d88-158">A Master, Model és MSDB teljes biztonsági másolatok készítése</span><span class="sxs-lookup"><span data-stu-id="92d88-158">Take full backups of Master, Model, and MSDB</span></span> |
| <span data-ttu-id="92d88-159">**Konfigurálja a biztonsági mentés ütemezése**</span><span class="sxs-lookup"><span data-stu-id="92d88-159">**Configure backup schedule**</span></span> | <span data-ttu-id="92d88-160">Manuális vagy automatikus (automatikus)</span><span class="sxs-lookup"><span data-stu-id="92d88-160">Manual/Automated (Automated)</span></span> | <span data-ttu-id="92d88-161">Válassza ki **automatikus** tooautomatically teljes igénybe, valamint naplófájl-biztonsági mentések napló növekedési alapján.</span><span class="sxs-lookup"><span data-stu-id="92d88-161">Select **Automated** tooautomatically take full and log backups based on log growth.</span></span> <span data-ttu-id="92d88-162">Válassza ki **manuális** teljes toospecify hello ütemezését, valamint naplófájl-biztonsági mentések.</span><span class="sxs-lookup"><span data-stu-id="92d88-162">Select **Manual** toospecify hello schedule for full and log backups.</span></span> |

## <a name="configuration-with-powershell"></a><span data-ttu-id="92d88-163">PowerShell-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="92d88-163">Configuration with PowerShell</span></span>
<span data-ttu-id="92d88-164">A következő PowerShell-példa hello, az automatikus biztonsági mentés meglévő SQL Server 2014 virtuális van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="92d88-164">In hello following PowerShell example, Automated Backup is configured for an existing SQL Server 2014 VM.</span></span> <span data-ttu-id="92d88-165">Hello **New-AzureVMSqlServerAutoBackupConfig** parancs hello automatikus biztonsági mentés beállításait toostore biztonsági mentések konfigurálása hello $storageaccount változó által megadott hello Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="92d88-165">hello **New-AzureVMSqlServerAutoBackupConfig** command configures hello Automated Backup settings toostore backups in hello Azure storage account specified by hello $storageaccount variable.</span></span> <span data-ttu-id="92d88-166">Ezek a biztonsági másolatok 10 napig lesznek megőrizve.</span><span class="sxs-lookup"><span data-stu-id="92d88-166">These backups will be retained for 10 days.</span></span> <span data-ttu-id="92d88-167">Hello **Set-AzureVMSqlServerExtension** frissítések hello Azure virtuális Géphez megadott ezekkel a beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="92d88-167">hello **Set-AzureVMSqlServerExtension** command updates hello specified Azure VM with these settings.</span></span>

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

<span data-ttu-id="92d88-168">Nem sikerült eltarthat néhány percig tooinstall és hello SQL Server IaaS-ügynök konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="92d88-168">It could take several minutes tooinstall and configure hello SQL Server IaaS Agent.</span></span>

<span data-ttu-id="92d88-169">tooenable titkosítási hello CertificatePassword paraméter hello előző parancsfájl toopass hello EnableEncryption paraméter és egy jelszó (biztonságos karakterlánc) módosítása.</span><span class="sxs-lookup"><span data-stu-id="92d88-169">tooenable encryption, modify hello previous script toopass hello EnableEncryption parameter along with a password (secure string) for hello CertificatePassword parameter.</span></span> <span data-ttu-id="92d88-170">hello következő parancsfájl lehetővé teszi, hogy az előző példában hello hello automatikus biztonsági mentés beállításait, és hozzáadja titkosítás.</span><span class="sxs-lookup"><span data-stu-id="92d88-170">hello following script enables hello Automated Backup settings in hello previous example and adds encryption.</span></span>

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

<span data-ttu-id="92d88-171">toodisable automatikus biztonsági mentés, azonos hello nélkül parancsfájl futtatási hello **-engedélyezése** paraméter toohello **New-AzureVMSqlServerAutoBackupConfig**.</span><span class="sxs-lookup"><span data-stu-id="92d88-171">toodisable automatic backup, run hello same script without hello **-Enable** parameter toohello **New-AzureVMSqlServerAutoBackupConfig**.</span></span> <span data-ttu-id="92d88-172">Csakúgy, mint a telepítés több percet toodisable automatikus biztonsági mentés is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="92d88-172">As with installation, it can take several minutes toodisable Automated Backup.</span></span>

> [!NOTE]
> <span data-ttu-id="92d88-173">Letiltásával és hello SQL Server IaaS-ügynök eltávolítása nem távolítja el korábban konfigurált hello való felügyelt biztonsági mentésének beállításait.</span><span class="sxs-lookup"><span data-stu-id="92d88-173">Disabling and uninstalling hello SQL Server IaaS Agent does not remove hello previously configured Managed Backup settings.</span></span> <span data-ttu-id="92d88-174">Automatikus biztonsági mentés letiltása, vagy hello SQL Server IaaS-ügynök eltávolítása előtt tiltsa le.</span><span class="sxs-lookup"><span data-stu-id="92d88-174">You should disable Automated Backup before disabling or uninstalling hello SQL Server IaaS Agent.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="92d88-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="92d88-175">Next steps</span></span>
<span data-ttu-id="92d88-176">Automatikus biztonsági mentés való felügyelt biztonsági mentésének konfigurálása Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="92d88-176">Automated Backup configures Managed Backup on Azure VMs.</span></span> <span data-ttu-id="92d88-177">Ezért fontos túl[tekintse meg a felügyelt biztonsági mentéshez hello dokumentációt](https://msdn.microsoft.com/library/dn449496.aspx) toounderstand hello viselkedést és megvalósítását.</span><span class="sxs-lookup"><span data-stu-id="92d88-177">So it is important too[review hello documentation for Managed Backup](https://msdn.microsoft.com/library/dn449496.aspx) toounderstand hello behavior and implications.</span></span>

<span data-ttu-id="92d88-178">További biztonsági mentés található, és állítsa vissza a következő témakör hello útmutatást az SQL Server Azure virtuális gépeken: [biztonsági mentése és visszaállítása az SQL Server Azure virtuális gépek](../sql/virtual-machines-windows-sql-backup-recovery.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="92d88-178">You can find additional backup and restore guidance for SQL Server on Azure VMs in hello following topic: [Backup and Restore for SQL Server in Azure Virtual Machines](../sql/virtual-machines-windows-sql-backup-recovery.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).</span></span>

<span data-ttu-id="92d88-179">Más elérhető automation feladatokkal kapcsolatos további információkért lásd: [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](../classic/sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="92d88-179">For information about other available automation tasks, see [SQL Server IaaS Agent Extension](../classic/sql-server-agent-extension.md).</span></span>

<span data-ttu-id="92d88-180">További információ az Azure virtuális gépeken futó SQL Server: [SQL Server Azure virtuális gépek – áttekintés](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="92d88-180">For more information about running SQL Server on Azure VMs, see [SQL Server on Azure Virtual Machines overview](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

