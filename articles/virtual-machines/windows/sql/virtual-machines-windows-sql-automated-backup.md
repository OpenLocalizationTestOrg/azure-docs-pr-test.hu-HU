---
title: "az SQL Server 2014 Azure virtuális gépek biztonsági mentése aaaAutomated |} Microsoft Docs"
description: "Ismerteti a hello automatikus biztonsági mentés szolgáltatás az SQL Server 2014 rendszert futtató virtuális gépek Azure-ban. Ez a cikk az adott tooVMs hello erőforrás-kezelő használatával."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: bdc63fd1-db49-4e76-87d5-b5c6a890e53c
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.openlocfilehash: c6803d8ef9f80e44a2f87918d87e099f1b562483
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automated-backup-for-sql-server-2014-virtual-machines-resource-manager"></a><span data-ttu-id="f2581-104">Automatikus biztonsági mentés SQL Server 2014 virtuális gépek (erőforrás-kezelő)</span><span class="sxs-lookup"><span data-stu-id="f2581-104">Automated Backup for SQL Server 2014 Virtual Machines (Resource Manager)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="f2581-105">SQL Server 2014</span><span class="sxs-lookup"><span data-stu-id="f2581-105">SQL Server 2014</span></span>](virtual-machines-windows-sql-automated-backup.md)
> * [<span data-ttu-id="f2581-106">SQL Server 2016</span><span class="sxs-lookup"><span data-stu-id="f2581-106">SQL Server 2016</span></span>](virtual-machines-windows-sql-automated-backup-v2.md)

<span data-ttu-id="f2581-107">Automatikus biztonsági mentés automatikusan konfigurálja a [való felügyelt biztonsági mentésének tooMicrosoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) az összes meglévő és új adatbázis egy SQL Server 2014 Standard vagy Enterprise rendszert futtató Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="f2581-107">Automated Backup automatically configures [Managed Backup tooMicrosoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) for all existing and new databases on an Azure VM running SQL Server 2014 Standard or Enterprise.</span></span> <span data-ttu-id="f2581-108">Ez lehetővé teszi tooconfigure rendszeres biztonsági tartós Azure blob-tárolók.</span><span class="sxs-lookup"><span data-stu-id="f2581-108">This enables you tooconfigure regular database backups that utilize durable Azure blob storage.</span></span> <span data-ttu-id="f2581-109">Automatikus biztonsági mentés függ hello [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="f2581-109">Automated Backup depends on hello [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a><span data-ttu-id="f2581-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f2581-110">Prerequisites</span></span>
<span data-ttu-id="f2581-111">toouse automatikus biztonsági mentés, vegye figyelembe a következő előfeltételek hello:</span><span class="sxs-lookup"><span data-stu-id="f2581-111">toouse Automated Backup, consider hello following prerequisites:</span></span>

<span data-ttu-id="f2581-112">**Operációs rendszer**:</span><span class="sxs-lookup"><span data-stu-id="f2581-112">**Operating System**:</span></span>

- <span data-ttu-id="f2581-113">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="f2581-113">Windows Server 2012</span></span>
- <span data-ttu-id="f2581-114">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="f2581-114">Windows Server 2012 R2</span></span>
- <span data-ttu-id="f2581-115">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="f2581-115">Windows Server 2016</span></span>

<span data-ttu-id="f2581-116">**SQL Server verziójához/kiadásához**:</span><span class="sxs-lookup"><span data-stu-id="f2581-116">**SQL Server version/edition**:</span></span>

- <span data-ttu-id="f2581-117">Az SQL Server 2014 Standard</span><span class="sxs-lookup"><span data-stu-id="f2581-117">SQL Server 2014 Standard</span></span>
- <span data-ttu-id="f2581-118">SQL Server 2014 Enterprise</span><span class="sxs-lookup"><span data-stu-id="f2581-118">SQL Server 2014 Enterprise</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f2581-119">Automatikus biztonsági mentés együttműködik az SQL Server 2014.</span><span class="sxs-lookup"><span data-stu-id="f2581-119">Automated Backup works with SQL Server 2014.</span></span> <span data-ttu-id="f2581-120">Ha az SQL Server 2016 használ, a v2 tooback automatikus biztonsági mentés másolatot az adatbázisokról is használhatja.</span><span class="sxs-lookup"><span data-stu-id="f2581-120">If you are using SQL Server 2016, you can use Automated Backup v2 tooback up your databases.</span></span> <span data-ttu-id="f2581-121">További információkért lásd: [az SQL Server 2016 Azure virtuális gépek automatikus biztonsági mentés v2](virtual-machines-windows-sql-automated-backup-v2.md).</span><span class="sxs-lookup"><span data-stu-id="f2581-121">For more information, see [Automated Backup v2 for SQL Server 2016 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup-v2.md).</span></span>

<span data-ttu-id="f2581-122">**Adatbázis-konfiguráció**:</span><span class="sxs-lookup"><span data-stu-id="f2581-122">**Database configuration**:</span></span>

- <span data-ttu-id="f2581-123">Cél adatbázisok hello teljes helyreállítási modell kell megadni.</span><span class="sxs-lookup"><span data-stu-id="f2581-123">Target databases must use hello full recovery model.</span></span> <span data-ttu-id="f2581-124">További információ hello hatás hello teljes helyreállítási modell biztonsági mentések, lásd: [biztonsági mentés alatt hello teljes helyreállítási modell](https://technet.microsoft.com/library/ms190217.aspx).</span><span class="sxs-lookup"><span data-stu-id="f2581-124">For more information about hello impact of hello full recovery model on backups, see [Backup Under hello Full Recovery Model](https://technet.microsoft.com/library/ms190217.aspx).</span></span>
- <span data-ttu-id="f2581-125">Céladatbázisokhoz hello alapértelmezett SQL Server-példányon kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f2581-125">Target databases must be on hello default SQL Server instance.</span></span> <span data-ttu-id="f2581-126">hello SQL Server IaaS-bővítmény nem támogatja az elnevezett példányok.</span><span class="sxs-lookup"><span data-stu-id="f2581-126">hello SQL Server IaaS Extension does not support named instances.</span></span>

<span data-ttu-id="f2581-127">**Az Azure-alapú üzemi modell**:</span><span class="sxs-lookup"><span data-stu-id="f2581-127">**Azure deployment model**:</span></span>

- <span data-ttu-id="f2581-128">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="f2581-128">Resource Manager</span></span>

<span data-ttu-id="f2581-129">**Az Azure PowerShell**:</span><span class="sxs-lookup"><span data-stu-id="f2581-129">**Azure PowerShell**:</span></span>

- <span data-ttu-id="f2581-130">[Telepítse a legújabb Azure PowerShell-parancsok hello](/powershell/azure/overview) Ha azt tervezi, hogy tooconfigure automatikus biztonsági mentés a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="f2581-130">[Install hello latest Azure PowerShell commands](/powershell/azure/overview) if you plan tooconfigure Automated Backup with PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="f2581-131">Automatikus biztonsági mentés SQL Server infrastruktúra-szolgáltatási ügynök bővítmény hello támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="f2581-131">Automated Backup relies on hello SQL Server IaaS Agent Extension.</span></span> <span data-ttu-id="f2581-132">A gyűjtemény lemezképei aktuális SQL virtuális gép alapértelmezés szerint adja hozzá ezt a bővítményt.</span><span class="sxs-lookup"><span data-stu-id="f2581-132">Current SQL virtual machine gallery images add this extension by default.</span></span> <span data-ttu-id="f2581-133">További információkért lásd: [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="f2581-133">For more information, see [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

## <a name="settings"></a><span data-ttu-id="f2581-134">Beállítások</span><span class="sxs-lookup"><span data-stu-id="f2581-134">Settings</span></span>

<span data-ttu-id="f2581-135">hello következő táblázat ismerteti, amelyek az automatikus biztonsági mentés hello-beállítások.</span><span class="sxs-lookup"><span data-stu-id="f2581-135">hello following table describes hello options that can be configured for Automated Backup.</span></span> <span data-ttu-id="f2581-136">hello tényleges konfigurációs lépései eltérőek attól függően, hogy használ-e hello Azure-portálon vagy az Azure PowerShell-parancsokkal.</span><span class="sxs-lookup"><span data-stu-id="f2581-136">hello actual configuration steps vary depending on whether you use hello Azure portal or Azure Windows PowerShell commands.</span></span>

| <span data-ttu-id="f2581-137">Beállítás</span><span class="sxs-lookup"><span data-stu-id="f2581-137">Setting</span></span> | <span data-ttu-id="f2581-138">Tartomány (alapértelmezett)</span><span class="sxs-lookup"><span data-stu-id="f2581-138">Range (Default)</span></span> | <span data-ttu-id="f2581-139">Leírás</span><span class="sxs-lookup"><span data-stu-id="f2581-139">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f2581-140">**Automatikus biztonsági mentés**</span><span class="sxs-lookup"><span data-stu-id="f2581-140">**Automated Backup**</span></span> | <span data-ttu-id="f2581-141">Engedélyezi/letiltja (letiltva)</span><span class="sxs-lookup"><span data-stu-id="f2581-141">Enable/Disable (Disabled)</span></span> | <span data-ttu-id="f2581-142">Engedélyezi vagy letiltja az automatikus biztonsági mentés az SQL Server 2014 Standard vagy Enterprise rendszert futtató Azure virtuális gép esetében.</span><span class="sxs-lookup"><span data-stu-id="f2581-142">Enables or disables Automated Backup for an Azure VM running SQL Server 2014 Standard or Enterprise.</span></span> |
| <span data-ttu-id="f2581-143">**Megőrzési időtartam**</span><span class="sxs-lookup"><span data-stu-id="f2581-143">**Retention Period**</span></span> | <span data-ttu-id="f2581-144">1-30 nap (30 nap)</span><span class="sxs-lookup"><span data-stu-id="f2581-144">1-30 days (30 days)</span></span> | <span data-ttu-id="f2581-145">nap tooretain biztonsági hello száma.</span><span class="sxs-lookup"><span data-stu-id="f2581-145">hello number of days tooretain a backup.</span></span> |
| <span data-ttu-id="f2581-146">**Tárfiók**</span><span class="sxs-lookup"><span data-stu-id="f2581-146">**Storage Account**</span></span> | <span data-ttu-id="f2581-147">Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="f2581-147">Azure storage account</span></span> | <span data-ttu-id="f2581-148">Egy Azure storage fiók toouse automatikus biztonsági mentés fájlok tárolásához a blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="f2581-148">An Azure storage account toouse for storing Automated Backup files in blob storage.</span></span> <span data-ttu-id="f2581-149">Egy tároló összes biztonságimásolat-fájl jön létre a hely toostore.</span><span class="sxs-lookup"><span data-stu-id="f2581-149">A container is created at this location toostore all backup files.</span></span> <span data-ttu-id="f2581-150">hello biztonságimásolat-fájl elnevezési konvenciója hello dátum, idő és a gép neve tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f2581-150">hello backup file naming convention includes hello date, time, and machine name.</span></span> |
| <span data-ttu-id="f2581-151">**Titkosítás**</span><span class="sxs-lookup"><span data-stu-id="f2581-151">**Encryption**</span></span> | <span data-ttu-id="f2581-152">Engedélyezi/letiltja (letiltva)</span><span class="sxs-lookup"><span data-stu-id="f2581-152">Enable/Disable (Disabled)</span></span> | <span data-ttu-id="f2581-153">Engedélyezi vagy letiltja a titkosítást.</span><span class="sxs-lookup"><span data-stu-id="f2581-153">Enables or disables encryption.</span></span> <span data-ttu-id="f2581-154">Ha engedélyezve van a titkosítás, hello használt tanúsítványok toorestore hello biztonsági mentés megadott hello találhatók-e tárolási fiókként hello azonos `automaticbackup` tárolót használó hello azonos elnevezési konvenciót.</span><span class="sxs-lookup"><span data-stu-id="f2581-154">When encryption is enabled, hello certificates used toorestore hello backup are located in hello specified storage account in hello same `automaticbackup` container using hello same naming convention.</span></span> <span data-ttu-id="f2581-155">Hello jelszó is módosul, ha új tanúsítványt hoz létre, hogy a jelszó, de a régi tanúsítvány hello marad toorestore korábbi biztonsági másolatok.</span><span class="sxs-lookup"><span data-stu-id="f2581-155">If hello password changes, a new certificate is generated with that password, but hello old certificate remains toorestore prior backups.</span></span> |
| <span data-ttu-id="f2581-156">**Jelszó**</span><span class="sxs-lookup"><span data-stu-id="f2581-156">**Password**</span></span> | <span data-ttu-id="f2581-157">Jelszó szöveg</span><span class="sxs-lookup"><span data-stu-id="f2581-157">Password text</span></span> | <span data-ttu-id="f2581-158">A titkosítási kulcsok jelszava.</span><span class="sxs-lookup"><span data-stu-id="f2581-158">A password for encryption keys.</span></span> <span data-ttu-id="f2581-159">Erre csak akkor van szükség, ha engedélyezve van-e a titkosítás.</span><span class="sxs-lookup"><span data-stu-id="f2581-159">This is only required if encryption is enabled.</span></span> <span data-ttu-id="f2581-160">A sorrend toorestore egy biztonsági másolat titkosításának engedélyezése, rendelkeznie kell hello helyes jelszót és a kapcsolódó hello a biztonsági mentés készült hello során használt tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="f2581-160">In order toorestore an encrypted backup, you must have hello correct password and related certificate that was used at hello time hello backup was taken.</span></span> |

## <a name="configuration-in-hello-portal"></a><span data-ttu-id="f2581-161">A portál hello konfiguráció</span><span class="sxs-lookup"><span data-stu-id="f2581-161">Configuration in hello Portal</span></span>

<span data-ttu-id="f2581-162">Hello Azure portál tooconfigure automatikus biztonsági mentés is használhatja, vagy meglévő SQL Server 2014 virtuális gépek kiépítése során.</span><span class="sxs-lookup"><span data-stu-id="f2581-162">You can use hello Azure portal tooconfigure Automated Backup during provisioning or for existing SQL Server 2014 VMs.</span></span>

### <a name="new-vms"></a><span data-ttu-id="f2581-163">Új virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="f2581-163">New VMs</span></span>

<span data-ttu-id="f2581-164">Amikor létrehoz egy új SQL Server 2014-virtuális gép hello Resource Manager üzembe helyezési modellel, használja a hello Azure portál tooconfigure automatikus biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="f2581-164">Use hello Azure portal tooconfigure Automated Backup when you create a new SQL Server 2014 Virtual Machine in hello Resource Manager deployment model.</span></span>

<span data-ttu-id="f2581-165">A hello **SQL Server-beállítások** panelen válassza **automatikus biztonsági mentés**.</span><span class="sxs-lookup"><span data-stu-id="f2581-165">In hello **SQL Server settings** blade, select **Automated backup**.</span></span> <span data-ttu-id="f2581-166">hello Azure portál következő képernyőfelvételen látható hello **SQL automatikus biztonsági mentés** panelen.</span><span class="sxs-lookup"><span data-stu-id="f2581-166">hello following Azure portal screenshot shows hello **SQL Automated Backup** blade.</span></span>

![SQL automatikus biztonsági mentés konfigurálása az Azure portálon](./media/virtual-machines-windows-sql-automated-backup/azure-sql-arm-autobackup.png)

<span data-ttu-id="f2581-168">A környezetben, témakörében hello teljes [az Azure SQL Server virtuális gépek kiépítése](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="f2581-168">For context, see hello complete topic on [provisioning a SQL Server virtual machine in Azure](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

### <a name="existing-vms"></a><span data-ttu-id="f2581-169">Meglévő virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="f2581-169">Existing VMs</span></span>

<span data-ttu-id="f2581-170">Meglévő SQL Server virtuális gépek válassza ki az SQL Server virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="f2581-170">For existing SQL Server virtual machines, select your SQL Server virtual machine.</span></span> <span data-ttu-id="f2581-171">Válassza ki hello **SQL Server-konfigurációs** hello szakasza **beállítások** panelen.</span><span class="sxs-lookup"><span data-stu-id="f2581-171">Then select hello **SQL Server configuration** section of hello **Settings** blade.</span></span>

![SQL automatikus biztonsági mentés a meglévő virtuális gépek](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-existing-vms.png)

<span data-ttu-id="f2581-173">A hello **SQL Server-konfigurációs** panelen kattintson hello **szerkesztése** hello gombjára automatikus biztonsági mentési szakasz.</span><span class="sxs-lookup"><span data-stu-id="f2581-173">In hello **SQL Server configuration** blade, click hello **Edit** button in hello Automated backup section.</span></span>

![Meglévő virtuális gépek SQL automatikus biztonsági mentés konfigurálása](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-configuration.png)

<span data-ttu-id="f2581-175">Ha elkészült, kattintson a hello **OK** hello alsó részén hello gombjára **SQL Server-konfigurációs** panel toosave a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="f2581-175">When finished, click hello **OK** button on hello bottom of hello **SQL Server configuration** blade toosave your changes.</span></span>

<span data-ttu-id="f2581-176">Ha engedélyezi az automatikus biztonsági mentés a hello először, Azure SQL Server IaaS-ügynök hello hello háttérben konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="f2581-176">If you are enabling Automated Backup for hello first time, Azure configures hello SQL Server IaaS Agent in hello background.</span></span> <span data-ttu-id="f2581-177">Ebben az időszakban hello Azure-portálon előfordulhat, hogy jelenjen meg, hogy az automatikus biztonsági mentés konfigurálva van-e.</span><span class="sxs-lookup"><span data-stu-id="f2581-177">During this time, hello Azure portal might not show that Automated Backup is configured.</span></span> <span data-ttu-id="f2581-178">Várjon egy pár percet hello ügynök toobe telepítve, a konfigurált.</span><span class="sxs-lookup"><span data-stu-id="f2581-178">Wait several minutes for hello agent toobe installed, configured.</span></span> <span data-ttu-id="f2581-179">Miután adott hello Azure portal hello új beállítások fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="f2581-179">After that hello Azure portal will reflect hello new settings.</span></span>

> [!NOTE]
> <span data-ttu-id="f2581-180">Automatikus biztonsági mentés sablon használatával is konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="f2581-180">You can also configure Automated Backup using a template.</span></span> <span data-ttu-id="f2581-181">További információkért lásd: [Azure gyors üzembe helyezés sablon automatikus biztonsági mentés](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update).</span><span class="sxs-lookup"><span data-stu-id="f2581-181">For more information, see [Azure quickstart template for Automated Backup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update).</span></span>

## <a name="configuration-with-powershell"></a><span data-ttu-id="f2581-182">PowerShell-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="f2581-182">Configuration with PowerShell</span></span>

<span data-ttu-id="f2581-183">Használhatja a PowerShell tooconfigure automatikus biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="f2581-183">You can use PowerShell tooconfigure Automated Backup.</span></span> <span data-ttu-id="f2581-184">Mielőtt elkezdené, a következőket kell tennie:</span><span class="sxs-lookup"><span data-stu-id="f2581-184">Before you begin, you must:</span></span>

- <span data-ttu-id="f2581-185">[Töltse le és telepítse a legújabb Azure PowerShell hello](http://aka.ms/webpi-azps).</span><span class="sxs-lookup"><span data-stu-id="f2581-185">[Download and install hello latest Azure PowerShell](http://aka.ms/webpi-azps).</span></span>
- <span data-ttu-id="f2581-186">Nyissa meg a Windows Powershellt, és rendelje hozzá azt a fiókot.</span><span class="sxs-lookup"><span data-stu-id="f2581-186">Open Windows PowerShell and associate it with your account.</span></span> <span data-ttu-id="f2581-187">Ehhez a hello hello lépéseket követve [az előfizetés konfigurálása](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-ps-sql-create#configure-your-subscription) témakör kiépítés hello szakasza.</span><span class="sxs-lookup"><span data-stu-id="f2581-187">You can do this by following hello steps in hello [Configure your subscription](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-ps-sql-create#configure-your-subscription) section of hello provisioning topic.</span></span>

### <a name="install-hello-sql-iaas-extension"></a><span data-ttu-id="f2581-188">Hello SQL IaaS-bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="f2581-188">Install hello SQL IaaS Extension</span></span>
<span data-ttu-id="f2581-189">Ha kiépített SQL Server virtuális gépek Azure-portálon hello létrehozását, hello SQL Server IaaS-bővítmény már telepítve kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f2581-189">If you provisioned a SQL Server virtual machine from hello Azure portal, hello SQL Server IaaS Extension should already be installed.</span></span> <span data-ttu-id="f2581-190">Segítségével meghatározhatja, hogy ha az telepítve van a virtuális gép meghívásával **Get-AzureRmVM** parancs és megvizsgálta az hello **bővítmények** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="f2581-190">You can determine if it is installed for your VM by calling **Get-AzureRmVM** command and examining hello **Extensions** property.</span></span>

```powershell
$vmname = "vmname"
$resourcegroupname = "resourcegroupname"

(Get-AzureRmVM -Name $vmname -ResourceGroupName $resourcegroupname).Extensions
```

<span data-ttu-id="f2581-191">Ha hello bővítmény SQL Server IaaS-ügynök telepítve van, megjelenik az "SqlIaaSAgent" vagy "SQLIaaSExtension" jelenik.</span><span class="sxs-lookup"><span data-stu-id="f2581-191">If hello SQL Server IaaS Agent extension is installed, you should see it listed as “SqlIaaSAgent” or “SQLIaaSExtension”.</span></span> <span data-ttu-id="f2581-192">**ProvisioningState** a hello bővítmény is jelenítsen meg a "Sikeres".</span><span class="sxs-lookup"><span data-stu-id="f2581-192">**ProvisioningState** for hello extension should also show “Succeeded”.</span></span>

<span data-ttu-id="f2581-193">Ha nincs telepítve, vagy toobe kiépítése sikertelen volt, a következő parancs hello telepítheti.</span><span class="sxs-lookup"><span data-stu-id="f2581-193">If it is not installed or failed toobe provisioned, you can install it with hello following command.</span></span> <span data-ttu-id="f2581-194">Továbbá toohello virtuális gép nevét és az erőforrás csoportjában is meg kell hello régió (**$region**), amely a virtuális gép található.</span><span class="sxs-lookup"><span data-stu-id="f2581-194">In addition toohello VM name and resource group, you must also specify hello region (**$region**) that your VM is located in.</span></span>

```powershell
$region = “EASTUS2”
Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region
```

### <span data-ttu-id="f2581-195"><a id="verifysettings"></a>Aktuális beállításainak ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="f2581-195"><a id="verifysettings"></a> Verify current settings</span></span>

<span data-ttu-id="f2581-196">Ha engedélyezte az automatikus biztonsági mentés kiépítése során, használhatja a jelenlegi konfiguráció PowerShell toocheck.</span><span class="sxs-lookup"><span data-stu-id="f2581-196">If you enabled automated backup during provisioning, you can use PowerShell toocheck your current configuration.</span></span> <span data-ttu-id="f2581-197">Futtassa a hello **Get-AzureRmVMSqlServerExtension** parancsot, és vizsgálja meg a hello **AutoBackupSettings** tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="f2581-197">Run hello **Get-AzureRmVMSqlServerExtension** command and examine hello **AutoBackupSettings** property:</span></span>

```powershell
(Get-AzureRmVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

<span data-ttu-id="f2581-198">Kimeneti hasonló toohello következő szerezheti be:</span><span class="sxs-lookup"><span data-stu-id="f2581-198">You should get output similar toohello following:</span></span>

```
Enable                      : False
EnableEncryption            : False
RetentionPeriod             : -1
StorageUrl                  : NOTSET
StorageAccessKey            : 
Password                    : 
BackupSystemDbs             : False
BackupScheduleType          : 
FullBackupFrequency         : 
FullBackupStartTime         : 
FullBackupWindowHours       : 
LogBackupFrequency          : 
```

<span data-ttu-id="f2581-199">Ha a kimeneti azt mutatja, hogy **engedélyezése** értéke túl**hamis**, akkor el kell tooenable automatikus biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="f2581-199">If your output shows that **Enable** is set too**False**, then you have tooenable automated backup.</span></span> <span data-ttu-id="f2581-200">hello jó hírünk engedélyezése és konfigurálása az automatikus biztonsági mentés hello a megszokott módon.</span><span class="sxs-lookup"><span data-stu-id="f2581-200">hello good news is that you enable and configure Automated Backup in hello same way.</span></span> <span data-ttu-id="f2581-201">Hello ezt az információt a következő szakaszban talál.</span><span class="sxs-lookup"><span data-stu-id="f2581-201">See hello next section for this information.</span></span>

> [!NOTE] 
> <span data-ttu-id="f2581-202">Hello beállítások módosítása után azonnal ellenőrzését, akkor lehetséges, hogy kap vissza hello régi konfigurációs értékeket.</span><span class="sxs-lookup"><span data-stu-id="f2581-202">If you check hello settings immediately after making a change, it is possible that you will get back hello old configuration values.</span></span> <span data-ttu-id="f2581-203">Várjon néhány percet, és ellenőrizze a hello beállításait újra toomake meg arról, hogy a módosítások alkalmazása megtörtént.</span><span class="sxs-lookup"><span data-stu-id="f2581-203">Wait a few minutes and check hello settings again toomake sure that your changes were applied.</span></span>

### <a name="configure-automated-backup"></a><span data-ttu-id="f2581-204">Automatikus biztonsági mentés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f2581-204">Configure Automated Backup</span></span>
<span data-ttu-id="f2581-205">Használható PowerShell tooenable automatikus biztonsági mentés, valamint a toomodify a konfiguráció és működés bármikor.</span><span class="sxs-lookup"><span data-stu-id="f2581-205">You can use PowerShell tooenable Automated Backup as well as toomodify its configuration and behavior at any time.</span></span>

<span data-ttu-id="f2581-206">Először válasszon, vagy hozzon létre egy tárfiókot a hello biztonságimásolat-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="f2581-206">First, select or create a storage account for hello backup files.</span></span> <span data-ttu-id="f2581-207">hello alábbi parancsfájl a storage-fiók kiválasztása vagy hoz létre, ha nem létezik.</span><span class="sxs-lookup"><span data-stu-id="f2581-207">hello following script selects a storage account or creates it if it does not exist.</span></span>

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
> <span data-ttu-id="f2581-208">Automatikus biztonsági mentés nem támogatja a prémium szintű storage tárolni a biztonsági mentések, de a prémium szintű Storage használó virtuális gépek lemezei készített biztonsági másolat is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="f2581-208">Automated Backup does not support storing backups in premium storage, but it can take backups from VM disks which use Premium Storage.</span></span>

<span data-ttu-id="f2581-209">Ezután használja az hello **New-AzureRmVMSqlServerAutoBackupConfig** tooenable parancsot, és hello automatikus biztonsági mentés beállításait toostore biztonsági mentések konfigurálása a hello Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="f2581-209">Then use hello **New-AzureRmVMSqlServerAutoBackupConfig** command tooenable and configure hello Automated Backup settings toostore backups in hello Azure storage account.</span></span> <span data-ttu-id="f2581-210">Ebben a példában a hello biztonsági mentések toobe 10 napig őrzi meg vannak beállítva.</span><span class="sxs-lookup"><span data-stu-id="f2581-210">In this example, hello backups are set toobe retained for 10 days.</span></span> <span data-ttu-id="f2581-211">a parancs második hello **Set-AzureRmVMSqlServerExtension**, frissítések hello Azure virtuális Géphez megadott ezekkel a beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="f2581-211">hello second command, **Set-AzureRmVMSqlServerExtension**, updates hello specified Azure VM with these settings.</span></span>

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

<span data-ttu-id="f2581-212">Nem sikerült eltarthat néhány percig tooinstall és hello SQL Server IaaS-ügynök konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="f2581-212">It could take several minutes tooinstall and configure hello SQL Server IaaS Agent.</span></span>

> [!NOTE]
> <span data-ttu-id="f2581-213">Nincsenek más beállítások **New-AzureRmVMSqlServerAutoBackupConfig** , amelyek csak a tooSQL Server 2016-os és az automatikus biztonsági mentés v2 érvényesek.</span><span class="sxs-lookup"><span data-stu-id="f2581-213">There are other settings for **New-AzureRmVMSqlServerAutoBackupConfig** that apply only tooSQL Server 2016 and Automated Backup v2.</span></span> <span data-ttu-id="f2581-214">Az SQL Server 2014 nem támogatja a következő beállítások hello: **BackupSystemDbs**, **BackupScheduleType**, **FullBackupFrequency**,  **FullBackupStartHour**, **FullBackupWindowInHours**, és **LogBackupFrequencyInMinutes**.</span><span class="sxs-lookup"><span data-stu-id="f2581-214">SQL Server 2014 does not support hello following settings: **BackupSystemDbs**, **BackupScheduleType**, **FullBackupFrequency**, **FullBackupStartHour**, **FullBackupWindowInHours**, and **LogBackupFrequencyInMinutes**.</span></span> <span data-ttu-id="f2581-215">Ha tooconfigure ezeket a beállításokat az SQL Server 2014 virtuális gépen, nincs hiba, de hello-beállítások nem érvényben.</span><span class="sxs-lookup"><span data-stu-id="f2581-215">If you attempt tooconfigure these settings on a SQL Server 2014 virtual machine, there is no error, but hello settings do not get applied.</span></span> <span data-ttu-id="f2581-216">Ha toouse ezeket a beállításokat SQL Server 2016-os virtuális gépen, lásd: [az SQL Server 2016 Azure virtuális gépek automatikus biztonsági mentés v2](virtual-machines-windows-sql-automated-backup-v2.md).</span><span class="sxs-lookup"><span data-stu-id="f2581-216">If you want toouse these settings on a SQL Server 2016 virtual machine, see [Automated Backup v2 for SQL Server 2016 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup-v2.md).</span></span>

<span data-ttu-id="f2581-217">tooenable titkosítási hello előző parancsfájl toopass hello módosítása **EnableEncryption** paraméter és egy jelszót (biztonságos karakterlánc) hello **CertificatePassword** paraméter.</span><span class="sxs-lookup"><span data-stu-id="f2581-217">tooenable encryption, modify hello previous script toopass hello **EnableEncryption** parameter along with a password (secure string) for hello **CertificatePassword** parameter.</span></span> <span data-ttu-id="f2581-218">hello következő parancsfájl lehetővé teszi, hogy az előző példában hello hello automatikus biztonsági mentés beállításait, és hozzáadja titkosítás.</span><span class="sxs-lookup"><span data-stu-id="f2581-218">hello following script enables hello Automated Backup settings in hello previous example and adds encryption.</span></span>

```powershell
$password = "P@ssw0rd"
$encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force

$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -EnableEncryption -CertificatePassword $encryptionpassword `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

<span data-ttu-id="f2581-219">a beállítások érvényesek, tooconfirm [hello automatikus biztonsági mentés konfigurációjának ellenőrzése](#verifysettings).</span><span class="sxs-lookup"><span data-stu-id="f2581-219">tooconfirm your settings are applied, [verify hello Automated Backup configuration](#verifysettings).</span></span>

### <a name="disable-automated-backup"></a><span data-ttu-id="f2581-220">Automatikus biztonsági mentés tiltása</span><span class="sxs-lookup"><span data-stu-id="f2581-220">Disable Automated Backup</span></span>

<span data-ttu-id="f2581-221">Automatikus biztonsági mentés, azonos hello nélkül parancsfájl futtatási hello toodisable **-engedélyezése** paraméter toohello **New-AzureRmVMSqlServerAutoBackupConfig** parancsot.</span><span class="sxs-lookup"><span data-stu-id="f2581-221">toodisable Automated Backup, run hello same script without hello **-Enable** parameter toohello **New-AzureRmVMSqlServerAutoBackupConfig** command.</span></span> <span data-ttu-id="f2581-222">hello hiányában hello **-engedélyezése** paraméter jelek hello parancs toodisable hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="f2581-222">hello absence of hello **-Enable** parameter signals hello command toodisable hello feature.</span></span> <span data-ttu-id="f2581-223">Csakúgy, mint a telepítés több percet toodisable automatikus biztonsági mentés is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="f2581-223">As with installation, it can take several minutes toodisable Automated Backup.</span></span>

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

### <a name="example-script"></a><span data-ttu-id="f2581-224">A példaként megadott parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="f2581-224">Example script</span></span>

<span data-ttu-id="f2581-225">hello következő parancsfájl biztosít a változók, hogy testre szabhatja a tooenable, és konfigurálja az automatikus biztonsági mentés a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="f2581-225">hello following script provides a set of variables that you can customize tooenable and configure Automated Backup for your VM.</span></span> <span data-ttu-id="f2581-226">Az Ön esetében szükség lehet a követelmények alapján toocustomize hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="f2581-226">In your case, you might need toocustomize hello script based on your requirements.</span></span> <span data-ttu-id="f2581-227">Például akkor toomake változásai Ha toodisable hello biztonsági mentés, a rendszer-adatbázisokat, vagy engedélyezheti a titkosítást.</span><span class="sxs-lookup"><span data-stu-id="f2581-227">For example, you would have toomake changes if you wanted toodisable hello backup of system databases or enable encryption.</span></span>

```powershell
$vmname = "yourvmname"
$resourcegroupname = "vmresourcegroupname"
$region = “Azure region name such as EASTUS2”
$storage_accountname = “storageaccountname”
$storage_resourcegroupname = $resourcegroupname
$retentionperiod = 10

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
    -ResourceGroupName $storage_resourcegroupname

# Apply hello Automated Backup settings toohello VM

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a><span data-ttu-id="f2581-228">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f2581-228">Next steps</span></span>

<span data-ttu-id="f2581-229">Automatikus biztonsági mentés való felügyelt biztonsági mentésének konfigurálása Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="f2581-229">Automated Backup configures Managed Backup on Azure VMs.</span></span> <span data-ttu-id="f2581-230">Ezért fontos túl[tekintse meg a felügyelt biztonsági mentéshez hello dokumentációt](https://msdn.microsoft.com/library/dn449496.aspx) toounderstand hello viselkedést és megvalósítását.</span><span class="sxs-lookup"><span data-stu-id="f2581-230">So it is important too[review hello documentation for Managed Backup](https://msdn.microsoft.com/library/dn449496.aspx) toounderstand hello behavior and implications.</span></span>

<span data-ttu-id="f2581-231">További biztonsági mentés található, és állítsa vissza a következő témakör hello útmutatást az SQL Server Azure virtuális gépeken: [biztonsági mentése és visszaállítása az SQL Server Azure virtuális gépek](virtual-machines-windows-sql-backup-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="f2581-231">You can find additional backup and restore guidance for SQL Server on Azure VMs in hello following topic: [Backup and Restore for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).</span></span>

<span data-ttu-id="f2581-232">Más elérhető automation feladatokkal kapcsolatos további információkért lásd: [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="f2581-232">For information about other available automation tasks, see [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

<span data-ttu-id="f2581-233">További információ az Azure virtuális gépeken futó SQL Server: [SQL Server Azure virtuális gépek – áttekintés](virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f2581-233">For more information about running SQL Server on Azure VMs, see [SQL Server on Azure Virtual Machines overview](virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

