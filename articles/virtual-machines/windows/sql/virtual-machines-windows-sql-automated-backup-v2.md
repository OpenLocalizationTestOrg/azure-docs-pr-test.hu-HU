---
title: "az SQL Server 2016 Azure virtuális gépek biztonsági mentése v2 aaaAutomated |} Microsoft Docs"
description: "Ismerteti a hello automatikus biztonsági mentés szolgáltatás az SQL Server 2016 rendszert futtató virtuális gépek Azure-ban. Ez a cikk az adott tooVMs hello erőforrás-kezelő használatával."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: ebd23868-821c-475b-b867-06d4a2e310c7
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/05/2017
ms.author: jroth
ms.openlocfilehash: a322792fb22c76bfa74fafb711b8b1927a6e2b3a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automated-backup-v2-for-sql-server-2016-azure-virtual-machines-resource-manager"></a><span data-ttu-id="9c1c6-104">Automatikus biztonsági mentési v2 az SQL Server 2016 az Azure virtuális gépek (erőforrás-kezelő)</span><span class="sxs-lookup"><span data-stu-id="9c1c6-104">Automated Backup v2 for SQL Server 2016 Azure Virtual Machines (Resource Manager)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="9c1c6-105">SQL Server 2014</span><span class="sxs-lookup"><span data-stu-id="9c1c6-105">SQL Server 2014</span></span>](virtual-machines-windows-sql-automated-backup.md)
> * [<span data-ttu-id="9c1c6-106">SQL Server 2016</span><span class="sxs-lookup"><span data-stu-id="9c1c6-106">SQL Server 2016</span></span>](virtual-machines-windows-sql-automated-backup-v2.md)

<span data-ttu-id="9c1c6-107">Az automatikus biztonsági mentés v2 automatikusan konfigurálja a [való felügyelt biztonsági mentésének tooMicrosoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) az összes meglévő és új adatbázis egy Azure virtuális gépen futó SQL Server 2016 Standard, Enterprise vagy fejlesztői kiadását.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-107">Automated Backup v2 automatically configures [Managed Backup tooMicrosoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) for all existing and new databases on an Azure VM running SQL Server 2016 Standard, Enterprise, or Developer editions.</span></span> <span data-ttu-id="9c1c6-108">Ez lehetővé teszi tooconfigure rendszeres biztonsági tartós Azure blob-tárolók.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-108">This enables you tooconfigure regular database backups that utilize durable Azure blob storage.</span></span> <span data-ttu-id="9c1c6-109">Az automatikus biztonsági mentés v2 függ hello [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="9c1c6-109">Automated Backup v2 depends on hello [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a><span data-ttu-id="9c1c6-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9c1c6-110">Prerequisites</span></span>
<span data-ttu-id="9c1c6-111">Automatikus biztonsági mentés 2-es toouse tekintse át a következő előfeltételek hello:</span><span class="sxs-lookup"><span data-stu-id="9c1c6-111">toouse Automated Backup v2, review hello following prerequisites:</span></span>

<span data-ttu-id="9c1c6-112">**Operációs rendszer**:</span><span class="sxs-lookup"><span data-stu-id="9c1c6-112">**Operating System**:</span></span>

- <span data-ttu-id="9c1c6-113">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="9c1c6-113">Windows Server 2012 R2</span></span>
- <span data-ttu-id="9c1c6-114">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="9c1c6-114">Windows Server 2016</span></span>

<span data-ttu-id="9c1c6-115">**SQL Server verziójához/kiadásához**:</span><span class="sxs-lookup"><span data-stu-id="9c1c6-115">**SQL Server version/edition**:</span></span>

- <span data-ttu-id="9c1c6-116">SQL Server 2016 Standard</span><span class="sxs-lookup"><span data-stu-id="9c1c6-116">SQL Server 2016 Standard</span></span>
- <span data-ttu-id="9c1c6-117">SQL Server 2016 Enterprise</span><span class="sxs-lookup"><span data-stu-id="9c1c6-117">SQL Server 2016 Enterprise</span></span>
- <span data-ttu-id="9c1c6-118">SQL Server 2016 Developer</span><span class="sxs-lookup"><span data-stu-id="9c1c6-118">SQL Server 2016 Developer</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9c1c6-119">Az automatikus biztonsági mentés v2 SQL Server 2016 működik.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-119">Automated Backup v2 works with SQL Server 2016.</span></span> <span data-ttu-id="9c1c6-120">Ha az SQL Server 2014-et használ, a automatikus biztonsági mentés v1 tooback másolatot az adatbázisokról is használhatja.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-120">If you are using SQL Server 2014, you can use Automated Backup v1 tooback up your databases.</span></span> <span data-ttu-id="9c1c6-121">További információkért lásd: [automatikus biztonsági mentés az SQL Server 2014 Azure virtuális gépek](virtual-machines-windows-sql-automated-backup.md).</span><span class="sxs-lookup"><span data-stu-id="9c1c6-121">For more information, see [Automated Backup for SQL Server 2014 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).</span></span>

<span data-ttu-id="9c1c6-122">**Adatbázis-konfiguráció**:</span><span class="sxs-lookup"><span data-stu-id="9c1c6-122">**Database configuration**:</span></span>

- <span data-ttu-id="9c1c6-123">Cél adatbázisok hello teljes helyreállítási modell kell megadni.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-123">Target databases must use hello full recovery model.</span></span> <span data-ttu-id="9c1c6-124">További információ hello hatás hello teljes helyreállítási modell biztonsági mentések, lásd: [biztonsági mentés alatt hello teljes helyreállítási modell](https://technet.microsoft.com/library/ms190217.aspx).</span><span class="sxs-lookup"><span data-stu-id="9c1c6-124">For more information about hello impact of hello full recovery model on backups, see [Backup Under hello Full Recovery Model](https://technet.microsoft.com/library/ms190217.aspx).</span></span>
- <span data-ttu-id="9c1c6-125">Rendszer-adatbázisokat nem rendelkeznek toouse teljes helyreállítási modell.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-125">System databases do not have toouse full recovery model.</span></span> <span data-ttu-id="9c1c6-126">Ha a napló biztonsági mentések toobe igénybe vett minta vagy MSDB van szüksége, a teljes helyreállítási modell kell használnia.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-126">However, if you require log backups toobe taken for Model or MSDB, you must use full recovery model.</span></span>
- <span data-ttu-id="9c1c6-127">Céladatbázisokhoz hello alapértelmezett SQL Server-példányon kell lennie.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-127">Target databases must be on hello default SQL Server instance.</span></span> <span data-ttu-id="9c1c6-128">hello SQL Server IaaS-bővítmény nem támogatja az elnevezett példányok.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-128">hello SQL Server IaaS Extension does not support named instances.</span></span>

<span data-ttu-id="9c1c6-129">**Az Azure-alapú üzemi modell**:</span><span class="sxs-lookup"><span data-stu-id="9c1c6-129">**Azure deployment model**:</span></span>

- <span data-ttu-id="9c1c6-130">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="9c1c6-130">Resource Manager</span></span>

> [!NOTE]
> <span data-ttu-id="9c1c6-131">Automatikus biztonsági mentés támaszkodik hello **SQL Server infrastruktúra-szolgáltatási ügynök bővítmény**.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-131">Automated Backup relies on hello **SQL Server IaaS Agent Extension**.</span></span> <span data-ttu-id="9c1c6-132">A gyűjtemény lemezképei aktuális SQL virtuális gép alapértelmezés szerint adja hozzá ezt a bővítményt.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-132">Current SQL virtual machine gallery images add this extension by default.</span></span> <span data-ttu-id="9c1c6-133">További információkért lásd: [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="9c1c6-133">For more information, see [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

## <a name="settings"></a><span data-ttu-id="9c1c6-134">Beállítások</span><span class="sxs-lookup"><span data-stu-id="9c1c6-134">Settings</span></span>
<span data-ttu-id="9c1c6-135">hello következő táblázat ismerteti, amelyek az automatikus biztonsági mentés v2 hello-beállítások.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-135">hello following table describes hello options that can be configured for Automated Backup v2.</span></span> <span data-ttu-id="9c1c6-136">hello tényleges konfigurációs lépései eltérőek attól függően, hogy használ-e hello Azure-portálon vagy az Azure PowerShell-parancsokkal.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-136">hello actual configuration steps vary depending on whether you use hello Azure portal or Azure Windows PowerShell commands.</span></span>

### <a name="basic-settings"></a><span data-ttu-id="9c1c6-137">Alapbeállítások</span><span class="sxs-lookup"><span data-stu-id="9c1c6-137">Basic Settings</span></span>

| <span data-ttu-id="9c1c6-138">Beállítás</span><span class="sxs-lookup"><span data-stu-id="9c1c6-138">Setting</span></span> | <span data-ttu-id="9c1c6-139">Tartomány (alapértelmezett)</span><span class="sxs-lookup"><span data-stu-id="9c1c6-139">Range (Default)</span></span> | <span data-ttu-id="9c1c6-140">Leírás</span><span class="sxs-lookup"><span data-stu-id="9c1c6-140">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9c1c6-141">**Automatikus biztonsági mentés**</span><span class="sxs-lookup"><span data-stu-id="9c1c6-141">**Automated Backup**</span></span> | <span data-ttu-id="9c1c6-142">Engedélyezi/letiltja (letiltva)</span><span class="sxs-lookup"><span data-stu-id="9c1c6-142">Enable/Disable (Disabled)</span></span> | <span data-ttu-id="9c1c6-143">Engedélyezi vagy letiltja az automatikus biztonsági mentés az SQL Server 2016 Standard vagy Enterprise rendszert futtató Azure virtuális gép esetében.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-143">Enables or disables Automated Backup for an Azure VM running SQL Server 2016 Standard or Enterprise.</span></span> |
| <span data-ttu-id="9c1c6-144">**Megőrzési időtartam**</span><span class="sxs-lookup"><span data-stu-id="9c1c6-144">**Retention Period**</span></span> | <span data-ttu-id="9c1c6-145">1-30 nap (30 nap)</span><span class="sxs-lookup"><span data-stu-id="9c1c6-145">1-30 days (30 days)</span></span> | <span data-ttu-id="9c1c6-146">hello az nap tooretain biztonsági mentéseinek számát.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-146">hello number of days tooretain backups.</span></span> |
| <span data-ttu-id="9c1c6-147">**Tárfiók**</span><span class="sxs-lookup"><span data-stu-id="9c1c6-147">**Storage Account**</span></span> | <span data-ttu-id="9c1c6-148">Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="9c1c6-148">Azure storage account</span></span> | <span data-ttu-id="9c1c6-149">Egy Azure storage fiók toouse automatikus biztonsági mentés fájlok tárolásához a blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-149">An Azure storage account toouse for storing Automated Backup files in blob storage.</span></span> <span data-ttu-id="9c1c6-150">Egy tároló összes biztonságimásolat-fájl jön létre a hely toostore.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-150">A container is created at this location toostore all backup files.</span></span> <span data-ttu-id="9c1c6-151">hello biztonságimásolat-fájl elnevezési konvenciója hello dátum, idő és adatbázis GUID-ja tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-151">hello backup file naming convention includes hello date, time, and database GUID.</span></span> |
| <span data-ttu-id="9c1c6-152">**Titkosítás**</span><span class="sxs-lookup"><span data-stu-id="9c1c6-152">**Encryption**</span></span> |<span data-ttu-id="9c1c6-153">Engedélyezi/letiltja (letiltva)</span><span class="sxs-lookup"><span data-stu-id="9c1c6-153">Enable/Disable (Disabled)</span></span> | <span data-ttu-id="9c1c6-154">Engedélyezi vagy letiltja a titkosítást.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-154">Enables or disables encryption.</span></span> <span data-ttu-id="9c1c6-155">Ha engedélyezve van a titkosítás, hello használt tanúsítványok toorestore hello biztonsági mentés megadott hello találhatók-e tárolási fiókként hello azonos **automaticbackup** tárolót használó hello azonos elnevezési konvenciót.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-155">When encryption is enabled, hello certificates used toorestore hello backup are located in hello specified storage account in hello same **automaticbackup** container using hello same naming convention.</span></span> <span data-ttu-id="9c1c6-156">Hello jelszó is módosul, ha új tanúsítványt hoz létre, hogy a jelszó, de a régi tanúsítvány hello marad toorestore korábbi biztonsági másolatok.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-156">If hello password changes, a new certificate is generated with that password, but hello old certificate remains toorestore prior backups.</span></span> |
| <span data-ttu-id="9c1c6-157">**Jelszó**</span><span class="sxs-lookup"><span data-stu-id="9c1c6-157">**Password**</span></span> |<span data-ttu-id="9c1c6-158">Jelszó szöveg</span><span class="sxs-lookup"><span data-stu-id="9c1c6-158">Password text</span></span> | <span data-ttu-id="9c1c6-159">A titkosítási kulcsok jelszava.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-159">A password for encryption keys.</span></span> <span data-ttu-id="9c1c6-160">Erre csak akkor van szükség, ha engedélyezve van-e a titkosítás.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-160">This is only required if encryption is enabled.</span></span> <span data-ttu-id="9c1c6-161">A sorrend toorestore egy biztonsági másolat titkosításának engedélyezése, rendelkeznie kell hello helyes jelszót és a kapcsolódó hello a biztonsági mentés készült hello során használt tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-161">In order toorestore an encrypted backup, you must have hello correct password and related certificate that was used at hello time hello backup was taken.</span></span> |

### <a name="advanced-settings"></a><span data-ttu-id="9c1c6-162">Speciális beállítások</span><span class="sxs-lookup"><span data-stu-id="9c1c6-162">Advanced Settings</span></span>

| <span data-ttu-id="9c1c6-163">Beállítás</span><span class="sxs-lookup"><span data-stu-id="9c1c6-163">Setting</span></span> | <span data-ttu-id="9c1c6-164">Tartomány (alapértelmezett)</span><span class="sxs-lookup"><span data-stu-id="9c1c6-164">Range (Default)</span></span> | <span data-ttu-id="9c1c6-165">Leírás</span><span class="sxs-lookup"><span data-stu-id="9c1c6-165">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9c1c6-166">**Rendszer-adatbázis biztonsági mentése**</span><span class="sxs-lookup"><span data-stu-id="9c1c6-166">**System Database Backups**</span></span> | <span data-ttu-id="9c1c6-167">Engedélyezi/letiltja (letiltva)</span><span class="sxs-lookup"><span data-stu-id="9c1c6-167">Enable/Disable (Disabled)</span></span> | <span data-ttu-id="9c1c6-168">Ha engedélyezve van, ez a funkció is készít biztonsági másolatot hello rendszeradatbázisokban: Master, MSDB és modell.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-168">When enabled, this feature will also back up hello system databases: Master, MSDB, and Model.</span></span> <span data-ttu-id="9c1c6-169">Hello MSDB és modell olyan adatbázisai esetén ellenőrizze, hogy azok teljes körű helyreállítási módban, ha azt szeretné, hogy a napló biztonsági mentések toobe venni.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-169">For hello MSDB and Model databases, verify that they are in full recovery mode if you want log backups toobe taken.</span></span> <span data-ttu-id="9c1c6-170">Napló-biztonságimásolatokat a rendszer soha nem főkiszolgáló hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-170">Log backups are never taken for Master.</span></span> <span data-ttu-id="9c1c6-171">És a TempDB nem készült biztonsági másolat készül.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-171">And no backups are taken for TempDB.</span></span> |
| <span data-ttu-id="9c1c6-172">**Biztonsági mentés ütemezése**</span><span class="sxs-lookup"><span data-stu-id="9c1c6-172">**Backup Schedule**</span></span> | <span data-ttu-id="9c1c6-173">Manuális vagy automatikus (automatikus)</span><span class="sxs-lookup"><span data-stu-id="9c1c6-173">Manual/Automated (Automated)</span></span> | <span data-ttu-id="9c1c6-174">Alapértelmezés szerint biztonsági mentés ütemezése hello automatikusan határozza hello napló növekedési alapján.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-174">By default, hello backup schedule will be automatically determined based on hello log growth.</span></span> <span data-ttu-id="9c1c6-175">Manuális biztonsági mentés ütemezése hello felhasználói toospecify hello időkerete biztonsági mentést tesz lehetővé.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-175">Manual backup schedule allows hello user toospecify hello time window for backups.</span></span> <span data-ttu-id="9c1c6-176">Ebben az esetben a biztonsági másolatok csak érvénybe hello helyén megadott gyakoriságát és hello során megadott időkerete pedig egy adott napon.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-176">In this case, backups will only ever take place at hello specified frequency and during hello specified time window of a given day.</span></span> |
| <span data-ttu-id="9c1c6-177">**Teljes biztonsági mentés gyakorisága**</span><span class="sxs-lookup"><span data-stu-id="9c1c6-177">**Full backup frequency**</span></span> | <span data-ttu-id="9c1c6-178">Napi vagy heti</span><span class="sxs-lookup"><span data-stu-id="9c1c6-178">Daily/Weekly</span></span> | <span data-ttu-id="9c1c6-179">Teljes biztonsági mentések gyakoriságát.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-179">Frequency of full backups.</span></span> <span data-ttu-id="9c1c6-180">Mindkét esetben a teljes biztonsági mentés hello következő ütemezett időpontban időszak alatt megkezdődik.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-180">In both cases, full backups will begin during hello next scheduled time window.</span></span> <span data-ttu-id="9c1c6-181">Heti kiválasztásakor a biztonsági mentések sikerült span több napon belül végre az összes adatbázis sikeresen biztonsági másolatából.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-181">When weekly is selected, backups could span multiple days until all databases have successfully backed up.</span></span> |
| <span data-ttu-id="9c1c6-182">**Teljes biztonsági mentés kezdési ideje**</span><span class="sxs-lookup"><span data-stu-id="9c1c6-182">**Full backup start time**</span></span> | <span data-ttu-id="9c1c6-183">00:00 – 23:00 (01:00)</span><span class="sxs-lookup"><span data-stu-id="9c1c6-183">00:00 – 23:00 (01:00)</span></span> | <span data-ttu-id="9c1c6-184">Kezdési időpont egy adott nap során, ami teljes biztonsági mentés akkor kerül sor.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-184">Start time of a given day during which full backups can take place.</span></span> |
| <span data-ttu-id="9c1c6-185">**Teljes biztonsági mentési időablak**</span><span class="sxs-lookup"><span data-stu-id="9c1c6-185">**Full backup time window**</span></span> | <span data-ttu-id="9c1c6-186">1 – 23 óra (1 óra)</span><span class="sxs-lookup"><span data-stu-id="9c1c6-186">1 – 23 hours (1 hour)</span></span> | <span data-ttu-id="9c1c6-187">Egy adott nap során, ami teljes biztonsági mentés akkor kerül sor hello időkerete időtartama.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-187">Duration of hello time window of a given day during which full backups can take place.</span></span> |
| <span data-ttu-id="9c1c6-188">**Napló biztonsági mentési gyakoriság**</span><span class="sxs-lookup"><span data-stu-id="9c1c6-188">**Log backup frequency**</span></span> | <span data-ttu-id="9c1c6-189">5 – 60 perc (60 perc)</span><span class="sxs-lookup"><span data-stu-id="9c1c6-189">5 – 60 minutes (60 minutes)</span></span> | <span data-ttu-id="9c1c6-190">A napló biztonsági mentések gyakoriságát.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-190">Frequency of log backups.</span></span> |

## <a name="understanding-full-backup-frequency"></a><span data-ttu-id="9c1c6-191">Teljes biztonsági mentés gyakoriságát ismertetése</span><span class="sxs-lookup"><span data-stu-id="9c1c6-191">Understanding full backup frequency</span></span>
<span data-ttu-id="9c1c6-192">Fontos toounderstand hello különbségének napi és heti teljes biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-192">It is important toounderstand hello difference between daily and weekly full backups.</span></span> <span data-ttu-id="9c1c6-193">Ebből a törekvésből végigvezetjük két példaforgatókönyvek keresztül.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-193">In this effort, we will walk through two example scenarios.</span></span>

### <a name="scenario-1-weekly-backups"></a><span data-ttu-id="9c1c6-194">1. forgatókönyv: Heti biztonsági mentései</span><span class="sxs-lookup"><span data-stu-id="9c1c6-194">Scenario 1: Weekly backups</span></span>
<span data-ttu-id="9c1c6-195">Egy nagyon nagy adatbázisok számát tartalmazó SQL Server virtuális gép van.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-195">You have a SQL Server VM which contains a number of very large databases.</span></span>

<span data-ttu-id="9c1c6-196">Hétfőn automatikus biztonsági mentés v2 a beállítások a következő hello engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="9c1c6-196">On Monday, you enable Automated Backup v2 with hello following settings:</span></span>

- <span data-ttu-id="9c1c6-197">Biztonsági mentés ütemezése: **manuális**</span><span class="sxs-lookup"><span data-stu-id="9c1c6-197">Backup schedule: **Manual**</span></span>
- <span data-ttu-id="9c1c6-198">Teljes biztonsági mentési gyakoriságot: **heti**</span><span class="sxs-lookup"><span data-stu-id="9c1c6-198">Full backup frequency: **Weekly**</span></span>
- <span data-ttu-id="9c1c6-199">Teljes biztonsági mentés kezdési ideje: **01:00**</span><span class="sxs-lookup"><span data-stu-id="9c1c6-199">Full backup start time: **01:00**</span></span>
- <span data-ttu-id="9c1c6-200">Teljes biztonsági mentési időablak: **1 óra**</span><span class="sxs-lookup"><span data-stu-id="9c1c6-200">Full backup time window: **1 hour**</span></span>

<span data-ttu-id="9c1c6-201">Ez azt jelenti, hogy hello tovább elérhető biztonsági mentési időablak kedd 1 óráig 1 órakor.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-201">This means that hello next available backup window is Tuesday at 1 AM for 1 hour.</span></span> <span data-ttu-id="9c1c6-202">Automatikus biztonsági mentés idő lejárta után megkezdődik egy adatbázis biztonsági másolatának egyszerre.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-202">At that time, Automated Backup will begin backing up your databases one at a time.</span></span> <span data-ttu-id="9c1c6-203">Ebben a forgatókönyvben az adatbázisok kellőképpen hosszúak legyenek, hogy hello első néhány adatbázisok teljes biztonsági mentés befejeződik.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-203">In this scenario, your databases are large enough that full backups will complete for hello first couple databases.</span></span> <span data-ttu-id="9c1c6-204">Azonban egy óra múlva összes hello adatbázisok biztonsági mentése volt.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-204">However, after one hour not all of hello databases have been backed up.</span></span>

<span data-ttu-id="9c1c6-205">Ez akkor fordul elő, amikor automatikus biztonsági mentés megkezdődik a fennmaradó adatbázisok hello másnap, szerda 13: 00 1 óráig: hello biztonsági mentéséről.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-205">When this happens, Automated Backup will begin backing up hello remaining databases hello next day, Wednesday at 1 AM for 1 hour.</span></span> <span data-ttu-id="9c1c6-206">Ha nem minden adatbázis biztonsági mentése volt, hogy időben, hello újra következő nap hello, azok, azonos megpróbál idő.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-206">If not all databases have been backed up in that time, it will try again hello next day at hello same time.</span></span> <span data-ttu-id="9c1c6-207">Továbbiakban ez fog folytatódni mindaddig, amíg az összes adatbázis sikeresen nincs biztonsági másolata.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-207">This will continue until all databases have been successfully backed up.</span></span>

<span data-ttu-id="9c1c6-208">Ha ismét eléri kedd, automatikus biztonsági mentés megkezdődik a adatbázisainak biztonsági mentése az összes még egyszer.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-208">Once it reaches Tuesday again, Automated Backup will begin backing up all databases once again.</span></span>

<span data-ttu-id="9c1c6-209">Ebből a forgatókönyvből megtudhatja, hogy az automatikus biztonsági mentés csak belül hello megadott időkeretnél fog működni, és az egyes adatbázisok készül biztonsági másolat hetente egyszer.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-209">This scenario shows that Automated Backup will only operate within hello specified time window, and each database will be backed up once per week.</span></span> <span data-ttu-id="9c1c6-210">Ez is mutatja, hogy lehetséges a biztonsági mentések toospan hello több napokat eset amennyiben nincs lehetséges toocomplete minden biztonsági mentés az egy nap.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-210">This also shows that it is possible for backups toospan multiple days in hello case where it is not possible toocomplete all backups in a single day.</span></span>

### <a name="scenario-2-daily-backups"></a><span data-ttu-id="9c1c6-211">2. forgatókönyv: Napi biztonsági mentései</span><span class="sxs-lookup"><span data-stu-id="9c1c6-211">Scenario 2: Daily backups</span></span>
<span data-ttu-id="9c1c6-212">Egy nagyon nagy adatbázisok számát tartalmazó SQL Server virtuális gép van.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-212">You have a SQL Server VM which contains a number of very large databases.</span></span>

<span data-ttu-id="9c1c6-213">Hétfőn automatikus biztonsági mentés v2 a beállítások a következő hello engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="9c1c6-213">On Monday, you enable Automated Backup v2 with hello following settings:</span></span>

- <span data-ttu-id="9c1c6-214">Biztonsági mentés ütemezése: manuális</span><span class="sxs-lookup"><span data-stu-id="9c1c6-214">Backup schedule: Manual</span></span>
- <span data-ttu-id="9c1c6-215">Teljes biztonsági mentési gyakoriságot: naponta</span><span class="sxs-lookup"><span data-stu-id="9c1c6-215">Full backup frequency: Daily</span></span>
- <span data-ttu-id="9c1c6-216">Teljes biztonsági mentés kezdési ideje: 22:00</span><span class="sxs-lookup"><span data-stu-id="9c1c6-216">Full backup start time: 22:00</span></span>
- <span data-ttu-id="9c1c6-217">Teljes biztonsági mentési időablak: 6 óra</span><span class="sxs-lookup"><span data-stu-id="9c1c6-217">Full backup time window: 6 hours</span></span>

<span data-ttu-id="9c1c6-218">Ez azt jelenti, hogy hello tovább elérhető biztonsági mentési időablak hétfő 10 du. hat órán át.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-218">This means that hello next available backup window is Monday at 10 PM for 6 hours.</span></span> <span data-ttu-id="9c1c6-219">Automatikus biztonsági mentés idő lejárta után megkezdődik egy adatbázis biztonsági másolatának egyszerre.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-219">At that time, Automated Backup will begin backing up your databases one at a time.</span></span>

<span data-ttu-id="9c1c6-220">Ezt követően 10 hat órán át keddjén összes adatbázis teljes biztonsági mentés újra elindul.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-220">Then, on Tuesday at 10 for 6 hours, full backups of all databases will start again.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9c1c6-221">Napi biztonsági mentés ütemezése, ajánlott úgy ütemezni a széles idő ablak tooensure összes adatbázis biztonsági másolat készíthető az időben.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-221">When scheduling daily backups, it is recommended that you schedule a wide time window tooensure all databases can be backed up within this time.</span></span> <span data-ttu-id="9c1c6-222">Ez különösen fontos hello esetben be nagy mennyiségű adatok tooback esetében.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-222">This is especially important in hello case where you have a large amount of data tooback up.</span></span>

## <a name="configuration-in-hello-portal"></a><span data-ttu-id="9c1c6-223">A portál hello konfiguráció</span><span class="sxs-lookup"><span data-stu-id="9c1c6-223">Configuration in hello Portal</span></span>

<span data-ttu-id="9c1c6-224">Hello Azure portál tooconfigure automatikus biztonsági mentés v2 is használhatja, vagy meglévő SQL Server 2016 virtuális gépek kiépítése során.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-224">You can use hello Azure portal tooconfigure Automated Backup v2 during provisioning or for existing SQL Server 2016 VMs.</span></span>

### <a name="new-vms"></a><span data-ttu-id="9c1c6-225">Új virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="9c1c6-225">New VMs</span></span>

<span data-ttu-id="9c1c6-226">Hello Azure portál tooconfigure automatikus biztonsági mentés v2 használja, amikor létrehoz egy új SQL Server 2016-virtuális gép hello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-226">Use hello Azure portal tooconfigure Automated Backup v2 when you create a new SQL Server 2016 Virtual Machine in hello Resource Manager deployment model.</span></span> 

<span data-ttu-id="9c1c6-227">A hello **SQL Server-beállítások** panelen válassza **automatikus biztonsági mentés**.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-227">In hello **SQL Server settings** blade, select **Automated backup**.</span></span> <span data-ttu-id="9c1c6-228">hello Azure portál következő képernyőfelvételen látható hello **SQL automatikus biztonsági mentés** panelen.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-228">hello following Azure portal screenshot shows hello **SQL Automated Backup** blade.</span></span>

![SQL automatikus biztonsági mentés konfigurálása az Azure portálon](./media/virtual-machines-windows-sql-automated-backup-v2/automated-backup-blade.png)

> [!NOTE]
> <span data-ttu-id="9c1c6-230">Az automatikus biztonsági mentés v2 alapértelmezés szerint le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-230">Automated Backup v2 is disabled by default.</span></span>

<span data-ttu-id="9c1c6-231">A környezetben, témakörében hello teljes [az Azure SQL Server virtuális gépek kiépítése](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="9c1c6-231">For context, see hello complete topic on [provisioning a SQL Server virtual machine in Azure](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

### <a name="existing-vms"></a><span data-ttu-id="9c1c6-232">Meglévő virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="9c1c6-232">Existing VMs</span></span>

<span data-ttu-id="9c1c6-233">Meglévő SQL Server virtuális gépek válassza ki az SQL Server virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-233">For existing SQL Server virtual machines, select your SQL Server virtual machine.</span></span> <span data-ttu-id="9c1c6-234">Válassza ki hello **SQL Server-konfigurációs** hello szakasza **beállítások** panelen.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-234">Then select hello **SQL Server configuration** section of hello **Settings** blade.</span></span>

![SQL automatikus biztonsági mentés a meglévő virtuális gépek](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration.png)

<span data-ttu-id="9c1c6-236">A hello **SQL Server-konfigurációs** panelen kattintson hello **szerkesztése** hello gombjára automatikus biztonsági mentési szakasz.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-236">In hello **SQL Server configuration** blade, click hello **Edit** button in hello Automated backup section.</span></span>

![Meglévő virtuális gépek SQL automatikus biztonsági mentés konfigurálása](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration-edit.png)

<span data-ttu-id="9c1c6-238">Ha elkészült, kattintson a hello **OK** hello alsó részén hello gombjára **SQL Server-konfigurációs** panel toosave a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-238">When finished, click hello **OK** button on hello bottom of hello **SQL Server configuration** blade toosave your changes.</span></span>

<span data-ttu-id="9c1c6-239">Ha engedélyezi az automatikus biztonsági mentés a hello először, Azure SQL Server IaaS-ügynök hello hello háttérben konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-239">If you are enabling Automated Backup for hello first time, Azure configures hello SQL Server IaaS Agent in hello background.</span></span> <span data-ttu-id="9c1c6-240">Ebben az időszakban hello Azure-portálon előfordulhat, hogy jelenjen meg, hogy az automatikus biztonsági mentés konfigurálva van-e.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-240">During this time, hello Azure portal might not show that Automated Backup is configured.</span></span> <span data-ttu-id="9c1c6-241">Várjon egy pár percet hello ügynök toobe telepítve, a konfigurált.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-241">Wait several minutes for hello agent toobe installed, configured.</span></span> <span data-ttu-id="9c1c6-242">Miután adott hello Azure portal hello új beállítások fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-242">After that hello Azure portal will reflect hello new settings.</span></span>

## <a name="configuration-with-powershell"></a><span data-ttu-id="9c1c6-243">PowerShell-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="9c1c6-243">Configuration with PowerShell</span></span>

<span data-ttu-id="9c1c6-244">PowerShell tooconfigure automatikus biztonsági mentés v2 is használhatja.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-244">You can use PowerShell tooconfigure Automated Backup v2.</span></span> <span data-ttu-id="9c1c6-245">Mielőtt elkezdené, a következőket kell tennie:</span><span class="sxs-lookup"><span data-stu-id="9c1c6-245">Before you begin, you must:</span></span>

- <span data-ttu-id="9c1c6-246">[Töltse le és telepítse a legújabb Azure PowerShell hello](http://aka.ms/webpi-azps).</span><span class="sxs-lookup"><span data-stu-id="9c1c6-246">[Download and install hello latest Azure PowerShell](http://aka.ms/webpi-azps).</span></span>
- <span data-ttu-id="9c1c6-247">Nyissa meg a Windows Powershellt, és rendelje hozzá azt a fiókot.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-247">Open Windows PowerShell and associate it with your account.</span></span> <span data-ttu-id="9c1c6-248">Ehhez a hello hello lépéseket követve [az előfizetés konfigurálása](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-ps-sql-create#configure-your-subscription) témakör kiépítés hello szakasza.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-248">You can do this by following hello steps in hello [Configure your subscription](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-ps-sql-create#configure-your-subscription) section of hello provisioning topic.</span></span>

### <a name="install-hello-sql-iaas-extension"></a><span data-ttu-id="9c1c6-249">Hello SQL IaaS-bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="9c1c6-249">Install hello SQL IaaS Extension</span></span>
<span data-ttu-id="9c1c6-250">Ha kiépített SQL Server virtuális gépek Azure-portálon hello létrehozását, hello SQL Server IaaS-bővítmény már telepítve kell lennie.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-250">If you provisioned a SQL Server virtual machine from hello Azure portal, hello SQL Server IaaS Extension should already be installed.</span></span> <span data-ttu-id="9c1c6-251">Segítségével meghatározhatja, hogy ha az telepítve van a virtuális gép meghívásával **Get-AzureRmVM** parancs és megvizsgálta az hello **bővítmények** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-251">You can determine if it is installed for your VM by calling **Get-AzureRmVM** command and examining hello **Extensions** property.</span></span>

```powershell
$vmname = "vmname"
$resourcegroupname = "resourcegroupname"

(Get-AzureRmVM -Name $vmname -ResourceGroupName $resourcegroupname).Extensions 
```

<span data-ttu-id="9c1c6-252">Ha hello bővítmény SQL Server IaaS-ügynök telepítve van, megjelenik az "SqlIaaSAgent" vagy "SQLIaaSExtension" jelenik.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-252">If hello SQL Server IaaS Agent extension is installed, you should see it listed as “SqlIaaSAgent” or “SQLIaaSExtension”.</span></span> <span data-ttu-id="9c1c6-253">**ProvisioningState** a hello bővítmény is jelenítsen meg a "Sikeres".</span><span class="sxs-lookup"><span data-stu-id="9c1c6-253">**ProvisioningState** for hello extension should also show “Succeeded”.</span></span> 

<span data-ttu-id="9c1c6-254">Ha nincs telepítve, vagy toobe kiépítése sikertelen volt, a következő parancs hello telepítheti.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-254">If it is not installed or failed toobe provisioned, you can install it with hello following command.</span></span> <span data-ttu-id="9c1c6-255">Továbbá toohello virtuális gép nevét és az erőforrás csoportjában is meg kell hello régió (**$region**), amely a virtuális gép található.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-255">In addition toohello VM name and resource group, you must also specify hello region (**$region**) that your VM is located in.</span></span>

```powershell
$region = “EASTUS2”
Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region 
```

### <span data-ttu-id="9c1c6-256"><a id="verifysettings"></a>Aktuális beállításainak ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="9c1c6-256"><a id="verifysettings"></a> Verify current settings</span></span>
<span data-ttu-id="9c1c6-257">Ha engedélyezte az automatikus biztonsági mentés kiépítése során, használhatja a jelenlegi konfiguráció PowerShell toocheck.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-257">If you enabled automated backup during provisioning, you can use PowerShell toocheck your current configuration.</span></span> <span data-ttu-id="9c1c6-258">Futtassa a hello **Get-AzureRmVMSqlServerExtension** parancsot, és vizsgálja meg a hello **AutoBackupSettings** tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="9c1c6-258">Run hello **Get-AzureRmVMSqlServerExtension** command and examine hello **AutoBackupSettings** property:</span></span>

```powershell
(Get-AzureRmVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

<span data-ttu-id="9c1c6-259">Kimeneti hasonló toohello következő szerezheti be:</span><span class="sxs-lookup"><span data-stu-id="9c1c6-259">You should get output similar toohello following:</span></span>

```
Enable                      : True
EnableEncryption            : False
RetentionPeriod             : 30
StorageUrl                  : https://test.blob.core.windows.net/
StorageAccessKey            :  
Password                    : 
BackupSystemDbs             : False
BackupScheduleType          : Manual
FullBackupFrequency         : WEEKLY
FullBackupStartTime         : 2
FullBackupWindowHours       : 2
LogBackupFrequency          : 60
```

<span data-ttu-id="9c1c6-260">Ha a kimeneti azt mutatja, hogy **engedélyezése** értéke túl**hamis**, akkor el kell tooenable automatikus biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-260">If your output shows that **Enable** is set too**False**, then you have tooenable automated backup.</span></span> <span data-ttu-id="9c1c6-261">hello jó hírünk engedélyezése és konfigurálása az automatikus biztonsági mentés hello a megszokott módon.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-261">hello good news is that you enable and configure Automated Backup in hello same way.</span></span> <span data-ttu-id="9c1c6-262">Hello ezt az információt a következő szakaszban talál.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-262">See hello next section for this information.</span></span>

> [!NOTE] 
> <span data-ttu-id="9c1c6-263">Hello beállítások módosítása után azonnal ellenőrzését, akkor lehetséges, hogy kap vissza hello régi konfigurációs értékeket.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-263">If you check hello settings immediately after making a change, it is possible that you will get back hello old configuration values.</span></span> <span data-ttu-id="9c1c6-264">Várjon néhány percet, és ellenőrizze a hello beállításait újra toomake meg arról, hogy a módosítások alkalmazása megtörtént.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-264">Wait a few minutes and check hello settings again toomake sure that your changes were applied.</span></span>

### <a name="configure-automated-backup-v2"></a><span data-ttu-id="9c1c6-265">Az automatikus biztonsági mentési v2 konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9c1c6-265">Configure Automated Backup v2</span></span>
<span data-ttu-id="9c1c6-266">Használható PowerShell tooenable automatikus biztonsági mentés, valamint a toomodify a konfiguráció és működés bármikor.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-266">You can use PowerShell tooenable Automated Backup as well as toomodify its configuration and behavior at any time.</span></span> 

<span data-ttu-id="9c1c6-267">Először válasszon, vagy hozzon létre egy tárfiókot a hello biztonságimásolat-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-267">First, select or create a storage account for hello backup files.</span></span> <span data-ttu-id="9c1c6-268">hello alábbi parancsfájl a storage-fiók kiválasztása vagy hoz létre, ha nem létezik.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-268">hello following script selects a storage account or creates it if it does not exist.</span></span>

```powershell
$storage_accountname = “yourstorageaccount”
$storage_resourcegroupname = $resourcegroupname

$storage = Get-AzureRmStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzureRmStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region } 
```

> [!NOTE]
> <span data-ttu-id="9c1c6-269">Automatikus biztonsági mentés nem támogatja a prémium szintű storage tárolni a biztonsági mentések, de a prémium szintű Storage használó virtuális gépek lemezei készített biztonsági másolat is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-269">Automated Backup does not support storing backups in premium storage, but it can take backups from VM disks which use Premium Storage.</span></span>

<span data-ttu-id="9c1c6-270">Ezután használja az hello **New-AzureRmVMSqlServerAutoBackupConfig** tooenable parancsot, és a hello Azure storage-fiók automatikus biztonsági mentés v2 hello beállítások toostore biztonsági mentések konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-270">Then use hello **New-AzureRmVMSqlServerAutoBackupConfig** command tooenable and configure hello Automated Backup v2 settings toostore backups in hello Azure storage account.</span></span> <span data-ttu-id="9c1c6-271">Ebben a példában a hello biztonsági mentések toobe 10 napig őrzi meg vannak beállítva.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-271">In this example, hello backups are set toobe retained for 10 days.</span></span> <span data-ttu-id="9c1c6-272">Rendszer-adatbázis biztonsági másolatait engedélyezve vannak.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-272">System database backups are enabled.</span></span> <span data-ttu-id="9c1c6-273">Teljes biztonsági mentések ütemezett heti 20:00 két órán keresztül kezdési idő ablakot.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-273">Full backups are scheduled for weekly with a time window starting at 20:00 for two hours.</span></span> <span data-ttu-id="9c1c6-274">30 percenként naplóalapú biztonsági mentések ütemezett.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-274">Log backups are scheduled for every 30 minutes.</span></span> <span data-ttu-id="9c1c6-275">a parancs második hello **Set-AzureRmVMSqlServerExtension**, frissítések hello Azure virtuális Géphez megadott ezekkel a beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-275">hello second command, **Set-AzureRmVMSqlServerExtension**, updates hello specified Azure VM with these settings.</span></span>

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType Manual -FullBackupFrequency Weekly `
    -FullBackupStartHour 20 -FullBackupWindowInHours 2 `
    -LogBackupFrequencyInMinutes 30 

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname 
```

<span data-ttu-id="9c1c6-276">Nem sikerült eltarthat néhány percig tooinstall és hello SQL Server IaaS-ügynök konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-276">It could take several minutes tooinstall and configure hello SQL Server IaaS Agent.</span></span> 

<span data-ttu-id="9c1c6-277">tooenable titkosítási hello előző parancsfájl toopass hello módosítása **EnableEncryption** paraméter és egy jelszót (biztonságos karakterlánc) hello **CertificatePassword** paraméter.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-277">tooenable encryption, modify hello previous script toopass hello **EnableEncryption** parameter along with a password (secure string) for hello **CertificatePassword** parameter.</span></span> <span data-ttu-id="9c1c6-278">hello következő parancsfájl lehetővé teszi, hogy az előző példában hello hello automatikus biztonsági mentés beállításait, és hozzáadja titkosítás.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-278">hello following script enables hello Automated Backup settings in hello previous example and adds encryption.</span></span>

```powershell
$password = "P@ssw0rd"
$encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  

$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -EnableEncryption -CertificatePassword $encryptionpassword `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType Manual -FullBackupFrequency Weekly `
    -FullBackupStartHour 20 -FullBackupWindowInHours 2 `
    -LogBackupFrequencyInMinutes 30 

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

<span data-ttu-id="9c1c6-279">a beállítások érvényesek, tooconfirm [hello automatikus biztonsági mentés konfigurációjának ellenőrzése](#verifysettings).</span><span class="sxs-lookup"><span data-stu-id="9c1c6-279">tooconfirm your settings are applied, [verify hello Automated Backup configuration](#verifysettings).</span></span>

### <a name="disable-automated-backup"></a><span data-ttu-id="9c1c6-280">Automatikus biztonsági mentés tiltása</span><span class="sxs-lookup"><span data-stu-id="9c1c6-280">Disable Automated Backup</span></span>
<span data-ttu-id="9c1c6-281">Automatikus biztonsági mentés, azonos hello nélkül parancsfájl futtatási hello toodisable **-engedélyezése** paraméter toohello **New-AzureRmVMSqlServerAutoBackupConfig** parancsot.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-281">toodisable Automated Backup, run hello same script without hello **-Enable** parameter toohello **New-AzureRmVMSqlServerAutoBackupConfig** command.</span></span> <span data-ttu-id="9c1c6-282">hello hiányában hello **-engedélyezése** paraméter jelek hello parancs toodisable hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-282">hello absence of hello **-Enable** parameter signals hello command toodisable hello feature.</span></span> <span data-ttu-id="9c1c6-283">Csakúgy, mint a telepítés több percet toodisable automatikus biztonsági mentés is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-283">As with installation, it can take several minutes toodisable Automated Backup.</span></span>

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

### <a name="example-script"></a><span data-ttu-id="9c1c6-284">A példaként megadott parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="9c1c6-284">Example script</span></span>
<span data-ttu-id="9c1c6-285">hello következő parancsfájl biztosít a változók, hogy testre szabhatja a tooenable, és konfigurálja az automatikus biztonsági mentés a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-285">hello following script provides a set of variables that you can customize tooenable and configure Automated Backup for your VM.</span></span> <span data-ttu-id="9c1c6-286">Az Ön esetében szükség lehet a követelmények alapján toocustomize hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-286">In your case, you might need toocustomize hello script based on your requirements.</span></span> <span data-ttu-id="9c1c6-287">Például akkor toomake változásai Ha toodisable hello biztonsági mentés, a rendszer-adatbázisokat, vagy engedélyezheti a titkosítást.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-287">For example, you would have toomake changes if you wanted toodisable hello backup of system databases or enable encryption.</span></span>

```powershell
$vmname = "yourvmname"
$resourcegroupname = "vmresourcegroupname"
$region = “Azure region name such as EASTUS2”
$storage_accountname = “storageaccountname”
$storage_resourcegroupname = $resourcegroupname
$retentionperiod = 10
$backupscheduletype = "Manual"
$fullbackupfrequency = "Weekly"
$fullbackupstarthour = "20"
$fullbackupwindow = "2"
$logbackupfrequency = "30"

# ResourceGroupName is hello resource group which is hosting hello VM where you are deploying hello SQL IaaS Extension 

Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region

# Creates/use a storage account toostore hello backups

$storage = Get-AzureRmStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzureRmStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region }

# Configure Automated Backup settings

$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays $retentionperiod -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType $backupscheduletype -FullBackupFrequency $fullbackupfrequency `
    -FullBackupStartHour $fullbackupstarthour -FullBackupWindowInHours $fullbackupwindow `
    -LogBackupFrequencyInMinutes $logbackupfrequency

# Apply hello Automated Backup settings toohello VM

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a><span data-ttu-id="9c1c6-288">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9c1c6-288">Next steps</span></span>
<span data-ttu-id="9c1c6-289">Az automatikus biztonsági mentés v2 való felügyelt biztonsági mentésének konfigurálása Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-289">Automated Backup v2 configures Managed Backup on Azure VMs.</span></span> <span data-ttu-id="9c1c6-290">Ezért fontos túl[tekintse meg a felügyelt biztonsági mentéshez hello dokumentációt](https://msdn.microsoft.com/library/dn449496.aspx) toounderstand hello viselkedést és megvalósítását.</span><span class="sxs-lookup"><span data-stu-id="9c1c6-290">So it is important too[review hello documentation for Managed Backup](https://msdn.microsoft.com/library/dn449496.aspx) toounderstand hello behavior and implications.</span></span>

<span data-ttu-id="9c1c6-291">További biztonsági mentés található, és állítsa vissza a következő témakör hello útmutatást az SQL Server Azure virtuális gépeken: [biztonsági mentése és visszaállítása az SQL Server Azure virtuális gépek](virtual-machines-windows-sql-backup-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="9c1c6-291">You can find additional backup and restore guidance for SQL Server on Azure VMs in hello following topic: [Backup and Restore for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).</span></span>

<span data-ttu-id="9c1c6-292">Más elérhető automation feladatokkal kapcsolatos további információkért lásd: [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="9c1c6-292">For information about other available automation tasks, see [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

<span data-ttu-id="9c1c6-293">További információ az Azure virtuális gépeken futó SQL Server: [SQL Server Azure virtuális gépek – áttekintés](virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9c1c6-293">For more information about running SQL Server on Azure VMs, see [SQL Server on Azure Virtual Machines overview](virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

