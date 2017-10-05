---
title: "A Site Recovery-tároló törlése"
description: "Megtudhatja, hogyan törölhető egy Azure Site Recovery-tárolóban, a Site Recovery forgatókönyv alapján."
service: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/04/2017
ms.author: rajani-janaki-ram
ms.openlocfilehash: b95b9defa0a037f7d7d3ef36b99bc7c53c751050
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="delete-a-site-recovery-vault"></a><span data-ttu-id="495ee-103">A Site Recovery-tároló törlése</span><span class="sxs-lookup"><span data-stu-id="495ee-103">Delete a Site Recovery vault</span></span>
<span data-ttu-id="495ee-104">Függőségek megakadályozzák az az Azure Site Recovery-tároló törlése.</span><span class="sxs-lookup"><span data-stu-id="495ee-104">Dependencies can prevent you from deleting an Azure Site Recovery vault.</span></span> <span data-ttu-id="495ee-105">A Site Recovery forgatókönyvön függően változnak, milyen lépéseket kell tennie: az Azure-ba, a Hyper-V (a és a System Center Virtual Machine Manager nélkül) Azure és az Azure Backup VMware.</span><span class="sxs-lookup"><span data-stu-id="495ee-105">The actions you need to take vary based on the Site Recovery scenario: VMware to Azure, Hyper-V (with and without System Center Virtual Machine Manager) to Azure, and Azure Backup.</span></span> <span data-ttu-id="495ee-106">Törli a tárolót, az Azure Backup szolgáltatáshoz használt, lásd: [az Azure biztonsági mentési tároló törlése](../backup/backup-azure-delete-vault.md).</span><span class="sxs-lookup"><span data-stu-id="495ee-106">To delete a vault used in Azure Backup, see [Delete a Backup vault in Azure](../backup/backup-azure-delete-vault.md).</span></span>

>[!Important]
><span data-ttu-id="495ee-107">Ha a termék tesztel, és nem részletesebben szeretnének foglalkozni az adatvesztés, a kényszerített delete metódus segítségével gyorsan távolítsa el a tárolóhoz és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="495ee-107">If you're testing the product and aren't concerned about data loss, use the force delete method to rapidly remove the vault and all its dependencies.</span></span>

> <span data-ttu-id="495ee-108">A PowerShell-parancsot a tároló tartalmának törlése, és nincs visszafejthető.</span><span class="sxs-lookup"><span data-stu-id="495ee-108">The PowerShell command deletes all the contents of the vault and is not reversible.</span></span>

## <a name="use-powershell-to-force-delete-the-vault"></a><span data-ttu-id="495ee-109">A PowerShell szolgáltatás használatával kényszerítheti a tároló törlése</span><span class="sxs-lookup"><span data-stu-id="495ee-109">Use PowerShell to force delete the vault</span></span> 

<span data-ttu-id="495ee-110">A Site Recovery-tároló törlése, még akkor is, ha nincsenek védett elemek, ezen parancsok használatával:</span><span class="sxs-lookup"><span data-stu-id="495ee-110">To delete the Site Recovery vault even if there are protected items, use these commands:</span></span>

    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionName "XXXXX"

    $vault = Get-AzureRmSiteRecoveryVault -Name "vaultname"

    Remove-AzureRmSiteRecoveryVault -Vault $vault


## <a name="delete-a-site-recovery-vault"></a><span data-ttu-id="495ee-111">A Site Recovery-tároló törlése</span><span class="sxs-lookup"><span data-stu-id="495ee-111">Delete a Site Recovery vault</span></span> 
<span data-ttu-id="495ee-112">Törli a tárolót, hajtsa végre a javasolt lépéseket a forgatókönyvéhez.</span><span class="sxs-lookup"><span data-stu-id="495ee-112">To delete the vault, follow the recommended steps for your scenario.</span></span>

### <a name="vmware-vms-to-azure"></a><span data-ttu-id="495ee-113">VMware virtuális gépek az Azure-ba</span><span class="sxs-lookup"><span data-stu-id="495ee-113">VMware VMs to Azure</span></span>

1. <span data-ttu-id="495ee-114">Töröljön minden védett virtuális gépek a megfelelő lépéseket követve [tiltsa le a védelmet a VMware](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span><span class="sxs-lookup"><span data-stu-id="495ee-114">Delete all protected VMs by following the steps in [Disable protection for a VMware](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span></span>

2. <span data-ttu-id="495ee-115">Minden replikációs házirendek törlése lépéseit követve [törölheti a replikációs házirendet](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span><span class="sxs-lookup"><span data-stu-id="495ee-115">Delete all replication policies by following the steps in [Delete a replication policy](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span></span>

3. <span data-ttu-id="495ee-116">Törli a vCenter mutató hivatkozások lépéseit követve [törli a vCenter](site-recovery-vmware-to-azure-manage-vCenter.md##delete-a-vcenter-in-azure-site-recovery).</span><span class="sxs-lookup"><span data-stu-id="495ee-116">Delete references to vCenter by following the steps in [Delete a vCenter](site-recovery-vmware-to-azure-manage-vCenter.md##delete-a-vcenter-in-azure-site-recovery).</span></span>

4. <span data-ttu-id="495ee-117">A konfigurációs kiszolgáló törlése a lépések [konfigurációs kiszolgáló leszerelése](site-recovery-vmware-to-azure-manage-configuration-server.md##decommissioning-a-configuration-server).</span><span class="sxs-lookup"><span data-stu-id="495ee-117">Delete the configuration server by following the steps in [Decommission a configuration server](site-recovery-vmware-to-azure-manage-configuration-server.md##decommissioning-a-configuration-server).</span></span>

5. <span data-ttu-id="495ee-118">Törli a tárolót.</span><span class="sxs-lookup"><span data-stu-id="495ee-118">Delete the vault.</span></span>


### <a name="hyper-v-vms-with-virtual-machine-manager-to-azure"></a><span data-ttu-id="495ee-119">A Hyper-V virtuális gépek (a Virtual Machine Managerrel) az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="495ee-119">Hyper-V VMs (with Virtual Machine Manager) to Azure</span></span>
1. <span data-ttu-id="495ee-120">Töröljön minden védett virtuális gépek a megfelelő lépéseket követve [tiltsa le a védelmet a VMware virtuális gép vagy fizikai kiszolgálón](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span><span class="sxs-lookup"><span data-stu-id="495ee-120">Delete all protected VMs by following the steps in [Disable protection for a VMware VM or physical server](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span></span>

2. <span data-ttu-id="495ee-121">Minden replikációs házirendek törlése lépéseit követve [törölheti a replikációs házirendet](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span><span class="sxs-lookup"><span data-stu-id="495ee-121">Delete all replication policies by following the steps in [Delete a replication policy](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span></span>

3.  <span data-ttu-id="495ee-122">A Virtual Machine Manager-kiszolgálók hivatkoznak törlése lépéseit követve [csatlakoztatott VMM-kiszolgáló regisztrációját](site-recovery-manage-registration-and-protection.md##unregister-a-connected-vmm-server).</span><span class="sxs-lookup"><span data-stu-id="495ee-122">Delete references to Virtual Machine Manager servers by following the steps in [Unregister a connected VMM server](site-recovery-manage-registration-and-protection.md##unregister-a-connected-vmm-server).</span></span>

4.  <span data-ttu-id="495ee-123">Törli a tárolót.</span><span class="sxs-lookup"><span data-stu-id="495ee-123">Delete the vault.</span></span>

### <a name="hyper-v-vms-without-virtual-machine-manager-to-azure"></a><span data-ttu-id="495ee-124">A Hyper-V virtuális gépek (nélkül Virtuálisgép-kezelő) az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="495ee-124">Hyper-V VMs (without Virtual Machine Manager) to Azure</span></span>
1. <span data-ttu-id="495ee-125">Töröljön minden védett virtuális gépek a megfelelő lépéseket követve [tiltsa le a védelmet a VMware virtuális gép vagy fizikai kiszolgálón](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span><span class="sxs-lookup"><span data-stu-id="495ee-125">Delete all protected VMs by following the steps in [Disable protection for a VMware VM or physical server](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span></span>

2. <span data-ttu-id="495ee-126">Minden replikációs házirendek törlése lépéseit követve [törölheti a replikációs házirendet](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span><span class="sxs-lookup"><span data-stu-id="495ee-126">Delete all replication policies by following the steps in [Delete a replication policy](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span></span>

3. <span data-ttu-id="495ee-127">Hyper-V kiszolgálók mutató hivatkozások törlése a lépések [regisztrációjának törlése a Hyper-V gazdagépek](/site-recovery-manage-registration-and-protection.md##unregister-a-hyper-v-host-in-a-hyper-v-site).</span><span class="sxs-lookup"><span data-stu-id="495ee-127">Delete references to Hyper-V servers by following the steps in [Unregister a Hyper-V host](/site-recovery-manage-registration-and-protection.md##unregister-a-hyper-v-host-in-a-hyper-v-site).</span></span>

4. <span data-ttu-id="495ee-128">A Hyper-V hely törlése.</span><span class="sxs-lookup"><span data-stu-id="495ee-128">Delete the Hyper-V site.</span></span>

5. <span data-ttu-id="495ee-129">Törli a tárolót.</span><span class="sxs-lookup"><span data-stu-id="495ee-129">Delete the vault.</span></span>
