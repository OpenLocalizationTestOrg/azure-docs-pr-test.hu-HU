---
title: "Az SQL Server 2016 Azure virtuális gépek biztonsági mentési v2 automatikus |} Microsoft Docs"
description: "Ismerteti az automatikus biztonsági mentés szolgáltatás az SQL Server 2016 rendszert futtató virtuális gépek Azure-ban. Ez a cikk csak a virtuális gépeknek a Resource Manager használatával."
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
ms.openlocfilehash: e7e14b0243f82c672392d5ab4bb6aca01156465b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="automated-backup-v2-for-sql-server-2016-azure-virtual-machines-resource-manager"></a><span data-ttu-id="23ece-104">Automatikus biztonsági mentési v2 az SQL Server 2016 az Azure virtuális gépek (erőforrás-kezelő)</span><span class="sxs-lookup"><span data-stu-id="23ece-104">Automated Backup v2 for SQL Server 2016 Azure Virtual Machines (Resource Manager)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="23ece-105">SQL Server 2014</span><span class="sxs-lookup"><span data-stu-id="23ece-105">SQL Server 2014</span></span>](virtual-machines-windows-sql-automated-backup.md)
> * [<span data-ttu-id="23ece-106">SQL Server 2016</span><span class="sxs-lookup"><span data-stu-id="23ece-106">SQL Server 2016</span></span>](virtual-machines-windows-sql-automated-backup-v2.md)

<span data-ttu-id="23ece-107">Automatikus biztonsági mentés v2 automatikusan konfigurálja a [Microsoft Azure Backup felügyelt](https://msdn.microsoft.com/library/dn449496.aspx) az összes meglévő és új adatbázis egy Azure virtuális gépen futó SQL Server 2016 Standard, Enterprise vagy fejlesztői kiadását.</span><span class="sxs-lookup"><span data-stu-id="23ece-107">Automated Backup v2 automatically configures [Managed Backup to Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) for all existing and new databases on an Azure VM running SQL Server 2016 Standard, Enterprise, or Developer editions.</span></span> <span data-ttu-id="23ece-108">Ez lehetővé teszi a tartós Azure blob-tárolók rendszeres biztonsági konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="23ece-108">This enables you to configure regular database backups that utilize durable Azure blob storage.</span></span> <span data-ttu-id="23ece-109">Az automatikus biztonsági mentés v2 függ a [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="23ece-109">Automated Backup v2 depends on the [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a><span data-ttu-id="23ece-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="23ece-110">Prerequisites</span></span>
<span data-ttu-id="23ece-111">Automatikus biztonsági mentés v2 használ, tekintse át a következő előfeltételeket:</span><span class="sxs-lookup"><span data-stu-id="23ece-111">To use Automated Backup v2, review the following prerequisites:</span></span>

<span data-ttu-id="23ece-112">**Operációs rendszer**:</span><span class="sxs-lookup"><span data-stu-id="23ece-112">**Operating System**:</span></span>

- <span data-ttu-id="23ece-113">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="23ece-113">Windows Server 2012 R2</span></span>
- <span data-ttu-id="23ece-114">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="23ece-114">Windows Server 2016</span></span>

<span data-ttu-id="23ece-115">**SQL Server verziójához/kiadásához**:</span><span class="sxs-lookup"><span data-stu-id="23ece-115">**SQL Server version/edition**:</span></span>

- <span data-ttu-id="23ece-116">SQL Server 2016 Standard</span><span class="sxs-lookup"><span data-stu-id="23ece-116">SQL Server 2016 Standard</span></span>
- <span data-ttu-id="23ece-117">SQL Server 2016 Enterprise</span><span class="sxs-lookup"><span data-stu-id="23ece-117">SQL Server 2016 Enterprise</span></span>
- <span data-ttu-id="23ece-118">SQL Server 2016 Developer</span><span class="sxs-lookup"><span data-stu-id="23ece-118">SQL Server 2016 Developer</span></span>

> [!IMPORTANT]
> <span data-ttu-id="23ece-119">Az automatikus biztonsági mentés v2 SQL Server 2016 működik.</span><span class="sxs-lookup"><span data-stu-id="23ece-119">Automated Backup v2 works with SQL Server 2016.</span></span> <span data-ttu-id="23ece-120">Ha SQL Server 2014-et használ, automatikus biztonsági mentés v1 segítségével készítsen biztonsági másolatot az adatbázisról.</span><span class="sxs-lookup"><span data-stu-id="23ece-120">If you are using SQL Server 2014, you can use Automated Backup v1 to back up your databases.</span></span> <span data-ttu-id="23ece-121">További információkért lásd: [automatikus biztonsági mentés az SQL Server 2014 Azure virtuális gépek](virtual-machines-windows-sql-automated-backup.md).</span><span class="sxs-lookup"><span data-stu-id="23ece-121">For more information, see [Automated Backup for SQL Server 2014 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).</span></span>

<span data-ttu-id="23ece-122">**Adatbázis-konfiguráció**:</span><span class="sxs-lookup"><span data-stu-id="23ece-122">**Database configuration**:</span></span>

- <span data-ttu-id="23ece-123">Cél adatbázisok kell megadni a teljes helyreállítási modell.</span><span class="sxs-lookup"><span data-stu-id="23ece-123">Target databases must use the full recovery model.</span></span> <span data-ttu-id="23ece-124">További információ arról, hogyan befolyásolják a teljes helyreállítási modell biztonsági mentések,: [biztonsági mentés alatt a teljes helyreállítási modell](https://technet.microsoft.com/library/ms190217.aspx).</span><span class="sxs-lookup"><span data-stu-id="23ece-124">For more information about the impact of the full recovery model on backups, see [Backup Under the Full Recovery Model](https://technet.microsoft.com/library/ms190217.aspx).</span></span>
- <span data-ttu-id="23ece-125">Rendszer-adatbázisokat nem rendelkeznek a teljes helyreállítási modell használatára.</span><span class="sxs-lookup"><span data-stu-id="23ece-125">System databases do not have to use full recovery model.</span></span> <span data-ttu-id="23ece-126">Napló-biztonságimásolatokat a modell vagy MSDB végrehajtását írja elő, ha a teljes helyreállítási modell kell használnia.</span><span class="sxs-lookup"><span data-stu-id="23ece-126">However, if you require log backups to be taken for Model or MSDB, you must use full recovery model.</span></span>
- <span data-ttu-id="23ece-127">Az alapértelmezett SQL Server-példány céladatbázisokhoz kell lennie.</span><span class="sxs-lookup"><span data-stu-id="23ece-127">Target databases must be on the default SQL Server instance.</span></span> <span data-ttu-id="23ece-128">Az SQL Server IaaS bővítmény nem támogatja az elnevezett példányok.</span><span class="sxs-lookup"><span data-stu-id="23ece-128">The SQL Server IaaS Extension does not support named instances.</span></span>

<span data-ttu-id="23ece-129">**Az Azure-alapú üzemi modell**:</span><span class="sxs-lookup"><span data-stu-id="23ece-129">**Azure deployment model**:</span></span>

- <span data-ttu-id="23ece-130">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="23ece-130">Resource Manager</span></span>

> [!NOTE]
> <span data-ttu-id="23ece-131">Automatikus biztonsági mentés támaszkodik a **SQL Server infrastruktúra-szolgáltatási ügynök bővítmény**.</span><span class="sxs-lookup"><span data-stu-id="23ece-131">Automated Backup relies on the **SQL Server IaaS Agent Extension**.</span></span> <span data-ttu-id="23ece-132">A gyűjtemény lemezképei aktuális SQL virtuális gép alapértelmezés szerint adja hozzá ezt a bővítményt.</span><span class="sxs-lookup"><span data-stu-id="23ece-132">Current SQL virtual machine gallery images add this extension by default.</span></span> <span data-ttu-id="23ece-133">További információkért lásd: [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="23ece-133">For more information, see [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

## <a name="settings"></a><span data-ttu-id="23ece-134">Beállítások</span><span class="sxs-lookup"><span data-stu-id="23ece-134">Settings</span></span>
<span data-ttu-id="23ece-135">A következő táblázat ismerteti a beállítások automatikus biztonsági mentés 2-es verzió konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="23ece-135">The following table describes the options that can be configured for Automated Backup v2.</span></span> <span data-ttu-id="23ece-136">A tényleges konfigurációs lépései eltérőek attól függően, hogy használ-e az Azure portálon vagy az Azure PowerShell-parancsokkal.</span><span class="sxs-lookup"><span data-stu-id="23ece-136">The actual configuration steps vary depending on whether you use the Azure portal or Azure Windows PowerShell commands.</span></span>

### <a name="basic-settings"></a><span data-ttu-id="23ece-137">Alapbeállítások</span><span class="sxs-lookup"><span data-stu-id="23ece-137">Basic Settings</span></span>

| <span data-ttu-id="23ece-138">Beállítás</span><span class="sxs-lookup"><span data-stu-id="23ece-138">Setting</span></span> | <span data-ttu-id="23ece-139">Tartomány (alapértelmezett)</span><span class="sxs-lookup"><span data-stu-id="23ece-139">Range (Default)</span></span> | <span data-ttu-id="23ece-140">Leírás</span><span class="sxs-lookup"><span data-stu-id="23ece-140">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="23ece-141">**Automatikus biztonsági mentés**</span><span class="sxs-lookup"><span data-stu-id="23ece-141">**Automated Backup**</span></span> | <span data-ttu-id="23ece-142">Engedélyezi/letiltja (letiltva)</span><span class="sxs-lookup"><span data-stu-id="23ece-142">Enable/Disable (Disabled)</span></span> | <span data-ttu-id="23ece-143">Engedélyezi vagy letiltja az automatikus biztonsági mentés az SQL Server 2016 Standard vagy Enterprise rendszert futtató Azure virtuális gép esetében.</span><span class="sxs-lookup"><span data-stu-id="23ece-143">Enables or disables Automated Backup for an Azure VM running SQL Server 2016 Standard or Enterprise.</span></span> |
| <span data-ttu-id="23ece-144">**Megőrzési időtartam**</span><span class="sxs-lookup"><span data-stu-id="23ece-144">**Retention Period**</span></span> | <span data-ttu-id="23ece-145">1-30 nap (30 nap)</span><span class="sxs-lookup"><span data-stu-id="23ece-145">1-30 days (30 days)</span></span> | <span data-ttu-id="23ece-146">A biztonsági mentések megőrzési napok száma.</span><span class="sxs-lookup"><span data-stu-id="23ece-146">The number of days to retain backups.</span></span> |
| <span data-ttu-id="23ece-147">**Tárfiók**</span><span class="sxs-lookup"><span data-stu-id="23ece-147">**Storage Account**</span></span> | <span data-ttu-id="23ece-148">Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="23ece-148">Azure storage account</span></span> | <span data-ttu-id="23ece-149">Azure-tárfiók a blob Storage tárolóban végzett tárolása automatikus biztonsági mentés fájlok használatára.</span><span class="sxs-lookup"><span data-stu-id="23ece-149">An Azure storage account to use for storing Automated Backup files in blob storage.</span></span> <span data-ttu-id="23ece-150">Egy tároló összes biztonsági mentési fájlok tárolására szolgáló ezen a helyen jön létre.</span><span class="sxs-lookup"><span data-stu-id="23ece-150">A container is created at this location to store all backup files.</span></span> <span data-ttu-id="23ece-151">A biztonságimásolat-fájl elnevezési tartalmazza a dátum, idő és adatbázis-GUID.</span><span class="sxs-lookup"><span data-stu-id="23ece-151">The backup file naming convention includes the date, time, and database GUID.</span></span> |
| <span data-ttu-id="23ece-152">**Titkosítás**</span><span class="sxs-lookup"><span data-stu-id="23ece-152">**Encryption**</span></span> |<span data-ttu-id="23ece-153">Engedélyezi/letiltja (letiltva)</span><span class="sxs-lookup"><span data-stu-id="23ece-153">Enable/Disable (Disabled)</span></span> | <span data-ttu-id="23ece-154">Engedélyezi vagy letiltja a titkosítást.</span><span class="sxs-lookup"><span data-stu-id="23ece-154">Enables or disables encryption.</span></span> <span data-ttu-id="23ece-155">Ha engedélyezve van, a biztonsági másolat visszaállítása a tanúsítványok találhatók-e a megadott tárfiók ugyanazon **automaticbackup** az azonos elnevezési konvenció tároló.</span><span class="sxs-lookup"><span data-stu-id="23ece-155">When encryption is enabled, the certificates used to restore the backup are located in the specified storage account in the same **automaticbackup** container using the same naming convention.</span></span> <span data-ttu-id="23ece-156">Ha a jelszó is módosul, egy új tanúsítványt hoz létre, hogy a jelszó, de a régi tanúsítvány marad a korábbi biztonsági másolatok.</span><span class="sxs-lookup"><span data-stu-id="23ece-156">If the password changes, a new certificate is generated with that password, but the old certificate remains to restore prior backups.</span></span> |
| <span data-ttu-id="23ece-157">**Jelszó**</span><span class="sxs-lookup"><span data-stu-id="23ece-157">**Password**</span></span> |<span data-ttu-id="23ece-158">Jelszó szöveg</span><span class="sxs-lookup"><span data-stu-id="23ece-158">Password text</span></span> | <span data-ttu-id="23ece-159">A titkosítási kulcsok jelszava.</span><span class="sxs-lookup"><span data-stu-id="23ece-159">A password for encryption keys.</span></span> <span data-ttu-id="23ece-160">Erre csak akkor van szükség, ha engedélyezve van-e a titkosítás.</span><span class="sxs-lookup"><span data-stu-id="23ece-160">This is only required if encryption is enabled.</span></span> <span data-ttu-id="23ece-161">Titkosított biztonsági másolat visszaállítása a helyes jelszót és a kapcsolódó kerül a biztonsági mentés idején használt tanúsítvány kell lennie.</span><span class="sxs-lookup"><span data-stu-id="23ece-161">In order to restore an encrypted backup, you must have the correct password and related certificate that was used at the time the backup was taken.</span></span> |

### <a name="advanced-settings"></a><span data-ttu-id="23ece-162">Speciális beállítások</span><span class="sxs-lookup"><span data-stu-id="23ece-162">Advanced Settings</span></span>

| <span data-ttu-id="23ece-163">Beállítás</span><span class="sxs-lookup"><span data-stu-id="23ece-163">Setting</span></span> | <span data-ttu-id="23ece-164">Tartomány (alapértelmezett)</span><span class="sxs-lookup"><span data-stu-id="23ece-164">Range (Default)</span></span> | <span data-ttu-id="23ece-165">Leírás</span><span class="sxs-lookup"><span data-stu-id="23ece-165">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="23ece-166">**Rendszer-adatbázis biztonsági mentése**</span><span class="sxs-lookup"><span data-stu-id="23ece-166">**System Database Backups**</span></span> | <span data-ttu-id="23ece-167">Engedélyezi/letiltja (letiltva)</span><span class="sxs-lookup"><span data-stu-id="23ece-167">Enable/Disable (Disabled)</span></span> | <span data-ttu-id="23ece-168">Ha engedélyezve van, ez a funkció is készít biztonsági másolatot a rendszer-adatbázisokat: Master, MSDB és modell.</span><span class="sxs-lookup"><span data-stu-id="23ece-168">When enabled, this feature will also back up the system databases: Master, MSDB, and Model.</span></span> <span data-ttu-id="23ece-169">Az msdb adatbázisban és modell adatbázisok esetén ellenőrizze, hogy azok teljes körű helyreállítási módban Ha azt szeretné, hogy a Naplók biztonsági másolatainak kell venni.</span><span class="sxs-lookup"><span data-stu-id="23ece-169">For the MSDB and Model databases, verify that they are in full recovery mode if you want log backups to be taken.</span></span> <span data-ttu-id="23ece-170">Napló-biztonságimásolatokat a rendszer soha nem főkiszolgáló hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="23ece-170">Log backups are never taken for Master.</span></span> <span data-ttu-id="23ece-171">És a TempDB nem készült biztonsági másolat készül.</span><span class="sxs-lookup"><span data-stu-id="23ece-171">And no backups are taken for TempDB.</span></span> |
| <span data-ttu-id="23ece-172">**Biztonsági mentés ütemezése**</span><span class="sxs-lookup"><span data-stu-id="23ece-172">**Backup Schedule**</span></span> | <span data-ttu-id="23ece-173">Manuális vagy automatikus (automatikus)</span><span class="sxs-lookup"><span data-stu-id="23ece-173">Manual/Automated (Automated)</span></span> | <span data-ttu-id="23ece-174">Alapértelmezés szerint a biztonsági mentés ütemezése lesz automatikusan alapján határozza meg a napló növekedési.</span><span class="sxs-lookup"><span data-stu-id="23ece-174">By default, the backup schedule will be automatically determined based on the log growth.</span></span> <span data-ttu-id="23ece-175">Manuális biztonsági mentés ütemezése a felhasználó adja meg a biztonsági mentés ideje ablakot.</span><span class="sxs-lookup"><span data-stu-id="23ece-175">Manual backup schedule allows the user to specify the time window for backups.</span></span> <span data-ttu-id="23ece-176">Ebben az esetben biztonsági másolatok csak történik a megadott gyakorisággal, a megadott időszakban egy adott nap.</span><span class="sxs-lookup"><span data-stu-id="23ece-176">In this case, backups will only ever take place at the specified frequency and during the specified time window of a given day.</span></span> |
| <span data-ttu-id="23ece-177">**Teljes biztonsági mentés gyakorisága**</span><span class="sxs-lookup"><span data-stu-id="23ece-177">**Full backup frequency**</span></span> | <span data-ttu-id="23ece-178">Napi vagy heti</span><span class="sxs-lookup"><span data-stu-id="23ece-178">Daily/Weekly</span></span> | <span data-ttu-id="23ece-179">Teljes biztonsági mentések gyakoriságát.</span><span class="sxs-lookup"><span data-stu-id="23ece-179">Frequency of full backups.</span></span> <span data-ttu-id="23ece-180">Mindkét esetben a teljes biztonsági mentés a következő ütemezett időszak alatt megkezdődik.</span><span class="sxs-lookup"><span data-stu-id="23ece-180">In both cases, full backups will begin during the next scheduled time window.</span></span> <span data-ttu-id="23ece-181">Heti kiválasztásakor a biztonsági mentések sikerült span több napon belül végre az összes adatbázis sikeresen biztonsági másolatából.</span><span class="sxs-lookup"><span data-stu-id="23ece-181">When weekly is selected, backups could span multiple days until all databases have successfully backed up.</span></span> |
| <span data-ttu-id="23ece-182">**Teljes biztonsági mentés kezdési ideje**</span><span class="sxs-lookup"><span data-stu-id="23ece-182">**Full backup start time**</span></span> | <span data-ttu-id="23ece-183">00:00 – 23:00 (01:00)</span><span class="sxs-lookup"><span data-stu-id="23ece-183">00:00 – 23:00 (01:00)</span></span> | <span data-ttu-id="23ece-184">Kezdési időpont egy adott nap során, ami teljes biztonsági mentés akkor kerül sor.</span><span class="sxs-lookup"><span data-stu-id="23ece-184">Start time of a given day during which full backups can take place.</span></span> |
| <span data-ttu-id="23ece-185">**Teljes biztonsági mentési időablak**</span><span class="sxs-lookup"><span data-stu-id="23ece-185">**Full backup time window**</span></span> | <span data-ttu-id="23ece-186">1 – 23 óra (1 óra)</span><span class="sxs-lookup"><span data-stu-id="23ece-186">1 – 23 hours (1 hour)</span></span> | <span data-ttu-id="23ece-187">A megadott nap során, ami teljes biztonsági mentés akkor kerül sor a időszak időtartama.</span><span class="sxs-lookup"><span data-stu-id="23ece-187">Duration of the time window of a given day during which full backups can take place.</span></span> |
| <span data-ttu-id="23ece-188">**Napló biztonsági mentési gyakoriság**</span><span class="sxs-lookup"><span data-stu-id="23ece-188">**Log backup frequency**</span></span> | <span data-ttu-id="23ece-189">5 – 60 perc (60 perc)</span><span class="sxs-lookup"><span data-stu-id="23ece-189">5 – 60 minutes (60 minutes)</span></span> | <span data-ttu-id="23ece-190">A napló biztonsági mentések gyakoriságát.</span><span class="sxs-lookup"><span data-stu-id="23ece-190">Frequency of log backups.</span></span> |

## <a name="understanding-full-backup-frequency"></a><span data-ttu-id="23ece-191">Teljes biztonsági mentés gyakoriságát ismertetése</span><span class="sxs-lookup"><span data-stu-id="23ece-191">Understanding full backup frequency</span></span>
<span data-ttu-id="23ece-192">Fontos, napi és heti teljes biztonsági mentések közötti különbségek megértése.</span><span class="sxs-lookup"><span data-stu-id="23ece-192">It is important to understand the difference between daily and weekly full backups.</span></span> <span data-ttu-id="23ece-193">Ebből a törekvésből végigvezetjük két példaforgatókönyvek keresztül.</span><span class="sxs-lookup"><span data-stu-id="23ece-193">In this effort, we will walk through two example scenarios.</span></span>

### <a name="scenario-1-weekly-backups"></a><span data-ttu-id="23ece-194">1. forgatókönyv: Heti biztonsági mentései</span><span class="sxs-lookup"><span data-stu-id="23ece-194">Scenario 1: Weekly backups</span></span>
<span data-ttu-id="23ece-195">Egy nagyon nagy adatbázisok számát tartalmazó SQL Server virtuális gép van.</span><span class="sxs-lookup"><span data-stu-id="23ece-195">You have a SQL Server VM which contains a number of very large databases.</span></span>

<span data-ttu-id="23ece-196">Hétfőn, engedélyezze az automatikus biztonsági mentés 2 a következő beállításokkal:</span><span class="sxs-lookup"><span data-stu-id="23ece-196">On Monday, you enable Automated Backup v2 with the following settings:</span></span>

- <span data-ttu-id="23ece-197">Biztonsági mentés ütemezése: **manuális**</span><span class="sxs-lookup"><span data-stu-id="23ece-197">Backup schedule: **Manual**</span></span>
- <span data-ttu-id="23ece-198">Teljes biztonsági mentési gyakoriságot: **heti**</span><span class="sxs-lookup"><span data-stu-id="23ece-198">Full backup frequency: **Weekly**</span></span>
- <span data-ttu-id="23ece-199">Teljes biztonsági mentés kezdési ideje: **01:00**</span><span class="sxs-lookup"><span data-stu-id="23ece-199">Full backup start time: **01:00**</span></span>
- <span data-ttu-id="23ece-200">Teljes biztonsági mentési időablak: **1 óra**</span><span class="sxs-lookup"><span data-stu-id="23ece-200">Full backup time window: **1 hour**</span></span>

<span data-ttu-id="23ece-201">Ez azt jelenti, hogy a következő elérhető biztonsági mentési időszakban kedd 1 óráig 1 órakor.</span><span class="sxs-lookup"><span data-stu-id="23ece-201">This means that the next available backup window is Tuesday at 1 AM for 1 hour.</span></span> <span data-ttu-id="23ece-202">Automatikus biztonsági mentés idő lejárta után megkezdődik egy adatbázis biztonsági másolatának egyszerre.</span><span class="sxs-lookup"><span data-stu-id="23ece-202">At that time, Automated Backup will begin backing up your databases one at a time.</span></span> <span data-ttu-id="23ece-203">Ebben a forgatókönyvben az adatbázisok kellőképpen hosszúak legyenek, hogy az első néhány adatbázisok teljes biztonsági mentés befejeződik.</span><span class="sxs-lookup"><span data-stu-id="23ece-203">In this scenario, your databases are large enough that full backups will complete for the first couple databases.</span></span> <span data-ttu-id="23ece-204">Azonban egy óra múlva összes az adatbázisok biztonsági mentése volt.</span><span class="sxs-lookup"><span data-stu-id="23ece-204">However, after one hour not all of the databases have been backed up.</span></span>

<span data-ttu-id="23ece-205">Ez akkor fordul elő, amikor automatikus biztonsági mentés megkezdődik adatbázisainak biztonsági mentése a fennmaradó a következő napon, szerda 13: 00 1 óra a következő.</span><span class="sxs-lookup"><span data-stu-id="23ece-205">When this happens, Automated Backup will begin backing up the remaining databases the next day, Wednesday at 1 AM for 1 hour.</span></span> <span data-ttu-id="23ece-206">Ha nem minden adatbázis biztonsági mentése volt, hogy időben, azt megpróbál egy időben újra a következő napon.</span><span class="sxs-lookup"><span data-stu-id="23ece-206">If not all databases have been backed up in that time, it will try again the next day at the same time.</span></span> <span data-ttu-id="23ece-207">Továbbiakban ez fog folytatódni mindaddig, amíg az összes adatbázis sikeresen nincs biztonsági másolata.</span><span class="sxs-lookup"><span data-stu-id="23ece-207">This will continue until all databases have been successfully backed up.</span></span>

<span data-ttu-id="23ece-208">Ha ismét eléri kedd, automatikus biztonsági mentés megkezdődik a adatbázisainak biztonsági mentése az összes még egyszer.</span><span class="sxs-lookup"><span data-stu-id="23ece-208">Once it reaches Tuesday again, Automated Backup will begin backing up all databases once again.</span></span>

<span data-ttu-id="23ece-209">Ebből a forgatókönyvből megtudhatja, hogy az automatikus biztonsági mentés csak a megadott időszak belül fog működni, és az egyes adatbázisok készül biztonsági másolat hetente egyszer.</span><span class="sxs-lookup"><span data-stu-id="23ece-209">This scenario shows that Automated Backup will only operate within the specified time window, and each database will be backed up once per week.</span></span> <span data-ttu-id="23ece-210">Ez is mutatja, hogy a biztonsági másolatok számára több napon abban az esetben, ahol nincs lehetőség egy nap az összes biztonsági mentés végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="23ece-210">This also shows that it is possible for backups to span multiple days in the case where it is not possible to complete all backups in a single day.</span></span>

### <a name="scenario-2-daily-backups"></a><span data-ttu-id="23ece-211">2. forgatókönyv: Napi biztonsági mentései</span><span class="sxs-lookup"><span data-stu-id="23ece-211">Scenario 2: Daily backups</span></span>
<span data-ttu-id="23ece-212">Egy nagyon nagy adatbázisok számát tartalmazó SQL Server virtuális gép van.</span><span class="sxs-lookup"><span data-stu-id="23ece-212">You have a SQL Server VM which contains a number of very large databases.</span></span>

<span data-ttu-id="23ece-213">Hétfőn, engedélyezze az automatikus biztonsági mentés 2 a következő beállításokkal:</span><span class="sxs-lookup"><span data-stu-id="23ece-213">On Monday, you enable Automated Backup v2 with the following settings:</span></span>

- <span data-ttu-id="23ece-214">Biztonsági mentés ütemezése: manuális</span><span class="sxs-lookup"><span data-stu-id="23ece-214">Backup schedule: Manual</span></span>
- <span data-ttu-id="23ece-215">Teljes biztonsági mentési gyakoriságot: naponta</span><span class="sxs-lookup"><span data-stu-id="23ece-215">Full backup frequency: Daily</span></span>
- <span data-ttu-id="23ece-216">Teljes biztonsági mentés kezdési ideje: 22:00</span><span class="sxs-lookup"><span data-stu-id="23ece-216">Full backup start time: 22:00</span></span>
- <span data-ttu-id="23ece-217">Teljes biztonsági mentési időablak: 6 óra</span><span class="sxs-lookup"><span data-stu-id="23ece-217">Full backup time window: 6 hours</span></span>

<span data-ttu-id="23ece-218">Ez azt jelenti, hogy a következő elérhető biztonsági mentési időszakban hétfő 10 du. hat órán át.</span><span class="sxs-lookup"><span data-stu-id="23ece-218">This means that the next available backup window is Monday at 10 PM for 6 hours.</span></span> <span data-ttu-id="23ece-219">Automatikus biztonsági mentés idő lejárta után megkezdődik egy adatbázis biztonsági másolatának egyszerre.</span><span class="sxs-lookup"><span data-stu-id="23ece-219">At that time, Automated Backup will begin backing up your databases one at a time.</span></span>

<span data-ttu-id="23ece-220">Ezt követően 10 hat órán át keddjén összes adatbázis teljes biztonsági mentés újra elindul.</span><span class="sxs-lookup"><span data-stu-id="23ece-220">Then, on Tuesday at 10 for 6 hours, full backups of all databases will start again.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="23ece-221">Napi biztonsági mentés ütemezése, ajánlott úgy ütemezni a széles időkerete annak érdekében, hogy minden adatbázisok biztonsági másolat készíthető az időben.</span><span class="sxs-lookup"><span data-stu-id="23ece-221">When scheduling daily backups, it is recommended that you schedule a wide time window to ensure all databases can be backed up within this time.</span></span> <span data-ttu-id="23ece-222">Ez különösen fontos abban az esetben, melyekben nagy mennyiségű adatok biztonsági mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="23ece-222">This is especially important in the case where you have a large amount of data to back up.</span></span>

## <a name="configuration-in-the-portal"></a><span data-ttu-id="23ece-223">A portál konfigurálása</span><span class="sxs-lookup"><span data-stu-id="23ece-223">Configuration in the Portal</span></span>

<span data-ttu-id="23ece-224">Az Azure portál segítségével konfigurálja automatikus biztonsági mentés v2 kiépítése során, vagy meglévő SQL Server 2016 virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="23ece-224">You can use the Azure portal to configure Automated Backup v2 during provisioning or for existing SQL Server 2016 VMs.</span></span>

### <a name="new-vms"></a><span data-ttu-id="23ece-225">Új virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="23ece-225">New VMs</span></span>

<span data-ttu-id="23ece-226">Az Azure portál segítségével konfigurálhatja az automatikus biztonsági mentés v2, egy új SQL Server 2016-virtuális gép létrehozásakor a Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="23ece-226">Use the Azure portal to configure Automated Backup v2 when you create a new SQL Server 2016 Virtual Machine in the Resource Manager deployment model.</span></span> 

<span data-ttu-id="23ece-227">Az a **SQL Server-beállítások** panelen válassza **automatikus biztonsági mentés**.</span><span class="sxs-lookup"><span data-stu-id="23ece-227">In the **SQL Server settings** blade, select **Automated backup**.</span></span> <span data-ttu-id="23ece-228">Az alábbi az Azure portál képernyőképe látható a **SQL automatikus biztonsági mentés** panelen.</span><span class="sxs-lookup"><span data-stu-id="23ece-228">The following Azure portal screenshot shows the **SQL Automated Backup** blade.</span></span>

![SQL automatikus biztonsági mentés konfigurálása az Azure portálon](./media/virtual-machines-windows-sql-automated-backup-v2/automated-backup-blade.png)

> [!NOTE]
> <span data-ttu-id="23ece-230">Az automatikus biztonsági mentés v2 alapértelmezés szerint le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="23ece-230">Automated Backup v2 is disabled by default.</span></span>

<span data-ttu-id="23ece-231">A környezetben, tekintse meg a teljes témakör [az Azure SQL Server virtuális gépek kiépítése](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="23ece-231">For context, see the complete topic on [provisioning a SQL Server virtual machine in Azure](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

### <a name="existing-vms"></a><span data-ttu-id="23ece-232">Meglévő virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="23ece-232">Existing VMs</span></span>

<span data-ttu-id="23ece-233">Meglévő SQL Server virtuális gépek válassza ki az SQL Server virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="23ece-233">For existing SQL Server virtual machines, select your SQL Server virtual machine.</span></span> <span data-ttu-id="23ece-234">Válassza ki a **SQL Server-konfigurációs** szakasza a **beállítások** panelen.</span><span class="sxs-lookup"><span data-stu-id="23ece-234">Then select the **SQL Server configuration** section of the **Settings** blade.</span></span>

![SQL automatikus biztonsági mentés a meglévő virtuális gépek](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration.png)

<span data-ttu-id="23ece-236">Az a **SQL Server-konfigurációs** panelen kattintson a **szerkesztése** gomb az automatikus biztonsági mentési szakaszban.</span><span class="sxs-lookup"><span data-stu-id="23ece-236">In the **SQL Server configuration** blade, click the **Edit** button in the Automated backup section.</span></span>

![Meglévő virtuális gépek SQL automatikus biztonsági mentés konfigurálása](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration-edit.png)

<span data-ttu-id="23ece-238">Ha elkészült, kattintson a **OK** gomb alján a **SQL Server-konfigurációs** panelt, és menti a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="23ece-238">When finished, click the **OK** button on the bottom of the **SQL Server configuration** blade to save your changes.</span></span>

<span data-ttu-id="23ece-239">Ha első alkalommal engedélyezi automatikus biztonsági mentés, Azure konfigurálja az SQL Server IaaS-ügynök a háttérben.</span><span class="sxs-lookup"><span data-stu-id="23ece-239">If you are enabling Automated Backup for the first time, Azure configures the SQL Server IaaS Agent in the background.</span></span> <span data-ttu-id="23ece-240">Ebben az időszakban az Azure-portálon előfordulhat, hogy jelenjen meg, hogy az automatikus biztonsági mentés konfigurálva van-e.</span><span class="sxs-lookup"><span data-stu-id="23ece-240">During this time, the Azure portal might not show that Automated Backup is configured.</span></span> <span data-ttu-id="23ece-241">Várjon egy pár percet, az ügynök telepítve, konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="23ece-241">Wait several minutes for the agent to be installed, configured.</span></span> <span data-ttu-id="23ece-242">Ezt követően az Azure-portálon az új beállítások fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="23ece-242">After that the Azure portal will reflect the new settings.</span></span>

## <a name="configuration-with-powershell"></a><span data-ttu-id="23ece-243">PowerShell-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="23ece-243">Configuration with PowerShell</span></span>

<span data-ttu-id="23ece-244">PowerShell segítségével konfigurálhatja az automatikus biztonsági mentés v2.</span><span class="sxs-lookup"><span data-stu-id="23ece-244">You can use PowerShell to configure Automated Backup v2.</span></span> <span data-ttu-id="23ece-245">Mielőtt elkezdené, a következőket kell tennie:</span><span class="sxs-lookup"><span data-stu-id="23ece-245">Before you begin, you must:</span></span>

- <span data-ttu-id="23ece-246">[Töltse le és telepítse a legújabb Azure PowerShell](http://aka.ms/webpi-azps).</span><span class="sxs-lookup"><span data-stu-id="23ece-246">[Download and install the latest Azure PowerShell](http://aka.ms/webpi-azps).</span></span>
- <span data-ttu-id="23ece-247">Nyissa meg a Windows Powershellt, és rendelje hozzá azt a fiókot.</span><span class="sxs-lookup"><span data-stu-id="23ece-247">Open Windows PowerShell and associate it with your account.</span></span> <span data-ttu-id="23ece-248">Ehhez a lépések a [az előfizetés konfigurálása](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-ps-sql-create#configure-your-subscription) a kiépítési témakör szakaszát.</span><span class="sxs-lookup"><span data-stu-id="23ece-248">You can do this by following the steps in the [Configure your subscription](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-ps-sql-create#configure-your-subscription) section of the provisioning topic.</span></span>

### <a name="install-the-sql-iaas-extension"></a><span data-ttu-id="23ece-249">Az SQL IaaS-bővítményének telepítése</span><span class="sxs-lookup"><span data-stu-id="23ece-249">Install the SQL IaaS Extension</span></span>
<span data-ttu-id="23ece-250">Kiépített Azure-portálról egy SQL Server virtuális gép, ha az SQL Server IaaS bővítmény már telepítve kell lennie.</span><span class="sxs-lookup"><span data-stu-id="23ece-250">If you provisioned a SQL Server virtual machine from the Azure portal, the SQL Server IaaS Extension should already be installed.</span></span> <span data-ttu-id="23ece-251">Segítségével meghatározhatja, hogy ha az telepítve van a virtuális gép meghívásával **Get-AzureRmVM** parancs és megvizsgálta a **bővítmények** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="23ece-251">You can determine if it is installed for your VM by calling **Get-AzureRmVM** command and examining the **Extensions** property.</span></span>

```powershell
$vmname = "vmname"
$resourcegroupname = "resourcegroupname"

(Get-AzureRmVM -Name $vmname -ResourceGroupName $resourcegroupname).Extensions 
```

<span data-ttu-id="23ece-252">Ha az SQL Server IaaS-ügynök bővítmény telepítve van, megjelenik az "SqlIaaSAgent" vagy "SQLIaaSExtension" jelenik.</span><span class="sxs-lookup"><span data-stu-id="23ece-252">If the SQL Server IaaS Agent extension is installed, you should see it listed as “SqlIaaSAgent” or “SQLIaaSExtension”.</span></span> <span data-ttu-id="23ece-253">**ProvisioningState** számára a kiterjesztést is kell megjelenítése "Sikeres".</span><span class="sxs-lookup"><span data-stu-id="23ece-253">**ProvisioningState** for the extension should also show “Succeeded”.</span></span> 

<span data-ttu-id="23ece-254">Ha van telepítve, vagy nem telepítése sikertelen volt, a következő paranccsal telepítheti.</span><span class="sxs-lookup"><span data-stu-id="23ece-254">If it is not installed or failed to be provisioned, you can install it with the following command.</span></span> <span data-ttu-id="23ece-255">A virtuális gép nevét és az erőforrás csoporton felül meg kell adnia a régióban (**$region**), amely a virtuális gép található.</span><span class="sxs-lookup"><span data-stu-id="23ece-255">In addition to the VM name and resource group, you must also specify the region (**$region**) that your VM is located in.</span></span>

```powershell
$region = “EASTUS2”
Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region 
```

### <span data-ttu-id="23ece-256"><a id="verifysettings"></a>Aktuális beállításainak ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="23ece-256"><a id="verifysettings"></a> Verify current settings</span></span>
<span data-ttu-id="23ece-257">Ha engedélyezte az automatikus biztonsági mentés kiépítése során, a PowerShell használatával ellenőrizze az aktuális konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="23ece-257">If you enabled automated backup during provisioning, you can use PowerShell to check your current configuration.</span></span> <span data-ttu-id="23ece-258">Futtassa a **Get-AzureRmVMSqlServerExtension** parancsot, és vizsgálja meg a **AutoBackupSettings** tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="23ece-258">Run the **Get-AzureRmVMSqlServerExtension** command and examine the **AutoBackupSettings** property:</span></span>

```powershell
(Get-AzureRmVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

<span data-ttu-id="23ece-259">Kimeneti kell kapnia a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="23ece-259">You should get output similar to the following:</span></span>

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

<span data-ttu-id="23ece-260">Ha a kimeneti azt mutatja, hogy **engedélyezése** értékre van állítva **hamis**, akkor el kell automatikus biztonsági mentés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="23ece-260">If your output shows that **Enable** is set to **False**, then you have to enable automated backup.</span></span> <span data-ttu-id="23ece-261">Jó hírünk engedélyezése és konfigurálása az automatikus biztonsági mentés azonos módon.</span><span class="sxs-lookup"><span data-stu-id="23ece-261">The good news is that you enable and configure Automated Backup in the same way.</span></span> <span data-ttu-id="23ece-262">Ezt az információt a következő részben olvashat.</span><span class="sxs-lookup"><span data-stu-id="23ece-262">See the next section for this information.</span></span>

> [!NOTE] 
> <span data-ttu-id="23ece-263">A módosítások elvégzése után azonnal ellenőrizze a beállításait, akkor lehetséges, hogy kap vissza a régi konfigurációs értékeket.</span><span class="sxs-lookup"><span data-stu-id="23ece-263">If you check the settings immediately after making a change, it is possible that you will get back the old configuration values.</span></span> <span data-ttu-id="23ece-264">Várjon néhány percet, és ellenőrizze a beállításait, ismét, és győződjön meg arról, hogy a módosítások alkalmazása megtörtént.</span><span class="sxs-lookup"><span data-stu-id="23ece-264">Wait a few minutes and check the settings again to make sure that your changes were applied.</span></span>

### <a name="configure-automated-backup-v2"></a><span data-ttu-id="23ece-265">Az automatikus biztonsági mentési v2 konfigurálása</span><span class="sxs-lookup"><span data-stu-id="23ece-265">Configure Automated Backup v2</span></span>
<span data-ttu-id="23ece-266">PowerShell segítségével engedélyezheti a automatikus biztonsági mentés, valamint hogy a konfiguráció és működés bármikor módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="23ece-266">You can use PowerShell to enable Automated Backup as well as to modify its configuration and behavior at any time.</span></span> 

<span data-ttu-id="23ece-267">Először is válassza ki, vagy hozzon létre egy tárfiókot, a biztonságimásolat-fájlok.</span><span class="sxs-lookup"><span data-stu-id="23ece-267">First, select or create a storage account for the backup files.</span></span> <span data-ttu-id="23ece-268">A következő parancsfájl választja ki egy tárfiókot, vagy hoz létre, ha nem létezik.</span><span class="sxs-lookup"><span data-stu-id="23ece-268">The following script selects a storage account or creates it if it does not exist.</span></span>

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
> <span data-ttu-id="23ece-269">Automatikus biztonsági mentés nem támogatja a prémium szintű storage tárolni a biztonsági mentések, de a prémium szintű Storage használó virtuális gépek lemezei készített biztonsági másolat is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="23ece-269">Automated Backup does not support storing backups in premium storage, but it can take backups from VM disks which use Premium Storage.</span></span>

<span data-ttu-id="23ece-270">Ezután a **New-AzureRmVMSqlServerAutoBackupConfig** parancs futtatásával engedélyezze és konfigurálja az automatikus biztonsági mentés v2 biztonsági másolatok tárolhatók az Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="23ece-270">Then use the **New-AzureRmVMSqlServerAutoBackupConfig** command to enable and configure the Automated Backup v2 settings to store backups in the Azure storage account.</span></span> <span data-ttu-id="23ece-271">Ebben a példában a biztonsági mentések őrzi meg a 10 napos vannak állítva.</span><span class="sxs-lookup"><span data-stu-id="23ece-271">In this example, the backups are set to be retained for 10 days.</span></span> <span data-ttu-id="23ece-272">Rendszer-adatbázis biztonsági másolatait engedélyezve vannak.</span><span class="sxs-lookup"><span data-stu-id="23ece-272">System database backups are enabled.</span></span> <span data-ttu-id="23ece-273">Teljes biztonsági mentések ütemezett heti 20:00 két órán keresztül kezdési idő ablakot.</span><span class="sxs-lookup"><span data-stu-id="23ece-273">Full backups are scheduled for weekly with a time window starting at 20:00 for two hours.</span></span> <span data-ttu-id="23ece-274">30 percenként naplóalapú biztonsági mentések ütemezett.</span><span class="sxs-lookup"><span data-stu-id="23ece-274">Log backups are scheduled for every 30 minutes.</span></span> <span data-ttu-id="23ece-275">A második parancs **Set-AzureRmVMSqlServerExtension**, ezekkel a beállításokkal a megadott Azure virtuális Gépet frissíti.</span><span class="sxs-lookup"><span data-stu-id="23ece-275">The second command, **Set-AzureRmVMSqlServerExtension**, updates the specified Azure VM with these settings.</span></span>

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

<span data-ttu-id="23ece-276">Eltarthat néhány percig, telepítése és konfigurálása az SQL Server IaaS-ügynök.</span><span class="sxs-lookup"><span data-stu-id="23ece-276">It could take several minutes to install and configure the SQL Server IaaS Agent.</span></span> 

<span data-ttu-id="23ece-277">Titkosítás engedélyezéséhez módosítsa a felelt meg az előző parancsfájl a **EnableEncryption** paraméterrel együtt (biztonságos karakterlánc) jelszavát a **CertificatePassword** paraméter.</span><span class="sxs-lookup"><span data-stu-id="23ece-277">To enable encryption, modify the previous script to pass the **EnableEncryption** parameter along with a password (secure string) for the **CertificatePassword** parameter.</span></span> <span data-ttu-id="23ece-278">A következő parancsfájl lehetővé teszi, hogy az előző példában az automatikus biztonsági mentés beállításait, és hozzáadja a titkosítás.</span><span class="sxs-lookup"><span data-stu-id="23ece-278">The following script enables the Automated Backup settings in the previous example and adds encryption.</span></span>

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

<span data-ttu-id="23ece-279">Ellenőrizze a beállításokat alkalmazzák, [az automatikus biztonsági mentés konfigurációjának ellenőrzése](#verifysettings).</span><span class="sxs-lookup"><span data-stu-id="23ece-279">To confirm your settings are applied, [verify the Automated Backup configuration](#verifysettings).</span></span>

### <a name="disable-automated-backup"></a><span data-ttu-id="23ece-280">Automatikus biztonsági mentés tiltása</span><span class="sxs-lookup"><span data-stu-id="23ece-280">Disable Automated Backup</span></span>
<span data-ttu-id="23ece-281">Automatikus biztonsági mentés letiltásához futtassa ugyanazt a parancsfájlt nélkül a **-engedélyezése** paramétert a **New-AzureRmVMSqlServerAutoBackupConfig** parancsot.</span><span class="sxs-lookup"><span data-stu-id="23ece-281">To disable Automated Backup, run the same script without the **-Enable** parameter to the **New-AzureRmVMSqlServerAutoBackupConfig** command.</span></span> <span data-ttu-id="23ece-282">Hiányában a **-engedélyezése** paraméter jelzi a parancs a funkció letiltásához.</span><span class="sxs-lookup"><span data-stu-id="23ece-282">The absence of the **-Enable** parameter signals the command to disable the feature.</span></span> <span data-ttu-id="23ece-283">Csakúgy, mint a telepítés, tiltsa le az automatikus biztonsági mentés több percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="23ece-283">As with installation, it can take several minutes to disable Automated Backup.</span></span>

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

### <a name="example-script"></a><span data-ttu-id="23ece-284">A példaként megadott parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="23ece-284">Example script</span></span>
<span data-ttu-id="23ece-285">A következő parancsfájl biztosít változók testre szabható engedélyezése és konfigurálása az automatikus biztonsági mentés a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="23ece-285">The following script provides a set of variables that you can customize to enable and configure Automated Backup for your VM.</span></span> <span data-ttu-id="23ece-286">Az Ön esetében szükség lehet a követelmények alapján a parancsfájl testreszabása.</span><span class="sxs-lookup"><span data-stu-id="23ece-286">In your case, you might need to customize the script based on your requirements.</span></span> <span data-ttu-id="23ece-287">Például kellene módosításokat, ha letiltja a rendszer adatbázisok biztonsági mentését, vagy engedélyezheti a titkosítást.</span><span class="sxs-lookup"><span data-stu-id="23ece-287">For example, you would have to make changes if you wanted to disable the backup of system databases or enable encryption.</span></span>

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

# ResourceGroupName is the resource group which is hosting the VM where you are deploying the SQL IaaS Extension 

Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region

# Creates/use a storage account to store the backups

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

# Apply the Automated Backup settings to the VM

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a><span data-ttu-id="23ece-288">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="23ece-288">Next steps</span></span>
<span data-ttu-id="23ece-289">Az automatikus biztonsági mentés v2 való felügyelt biztonsági mentésének konfigurálása Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="23ece-289">Automated Backup v2 configures Managed Backup on Azure VMs.</span></span> <span data-ttu-id="23ece-290">Ezért fontos, hogy [tekintse át a felügyelt biztonsági mentésének dokumentációjában](https://msdn.microsoft.com/library/dn449496.aspx) a viselkedést és alkalmazásainak megértéséhez.</span><span class="sxs-lookup"><span data-stu-id="23ece-290">So it is important to [review the documentation for Managed Backup](https://msdn.microsoft.com/library/dn449496.aspx) to understand the behavior and implications.</span></span>

<span data-ttu-id="23ece-291">További biztonsági mentés található, és állítsa vissza a következő témakörben található útmutatót az SQL Server Azure virtuális gépeken: [biztonsági mentése és visszaállítása az SQL Server Azure virtuális gépek](virtual-machines-windows-sql-backup-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="23ece-291">You can find additional backup and restore guidance for SQL Server on Azure VMs in the following topic: [Backup and Restore for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).</span></span>

<span data-ttu-id="23ece-292">Más elérhető automation feladatokkal kapcsolatos további információkért lásd: [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="23ece-292">For information about other available automation tasks, see [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

<span data-ttu-id="23ece-293">További információ az Azure virtuális gépeken futó SQL Server: [SQL Server Azure virtuális gépek – áttekintés](virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="23ece-293">For more information about running SQL Server on Azure VMs, see [SQL Server on Azure Virtual Machines overview](virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

