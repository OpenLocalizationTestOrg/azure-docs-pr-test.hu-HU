---
title: "aaaDelete Site Recovery-tároló"
description: "Ismerje meg, hogyan toodelete egy Azure Site Recovery-tárolóban alapján hello hely helyreállítási helyzetet."
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
ms.openlocfilehash: 36db202d8b790bb5d31d1348fb72f51acb5d559c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="delete-a-site-recovery-vault"></a><span data-ttu-id="ea9bd-103">A Site Recovery-tároló törlése</span><span class="sxs-lookup"><span data-stu-id="ea9bd-103">Delete a Site Recovery vault</span></span>
<span data-ttu-id="ea9bd-104">Függőségek megakadályozzák az az Azure Site Recovery-tároló törlése.</span><span class="sxs-lookup"><span data-stu-id="ea9bd-104">Dependencies can prevent you from deleting an Azure Site Recovery vault.</span></span> <span data-ttu-id="ea9bd-105">hello műveletek tootake függően változhat a hello Site Recovery forgatókönyv: VMware tooAzure, Hyper-V (a és a System Center Virtual Machine Manager nélkül) tooAzure, és az Azure biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="ea9bd-105">hello actions you need tootake vary based on hello Site Recovery scenario: VMware tooAzure, Hyper-V (with and without System Center Virtual Machine Manager) tooAzure, and Azure Backup.</span></span> <span data-ttu-id="ea9bd-106">egy Azure Backup szolgáltatásban használt tároló toodelete lásd [az Azure biztonsági mentési tároló törlése](../backup/backup-azure-delete-vault.md).</span><span class="sxs-lookup"><span data-stu-id="ea9bd-106">toodelete a vault used in Azure Backup, see [Delete a Backup vault in Azure](../backup/backup-azure-delete-vault.md).</span></span>

>[!Important]
><span data-ttu-id="ea9bd-107">Ha hello termék tesztel, és nem részletesebben szeretnének foglalkozni az adatvesztés, használja hello kényszerített törlése metódus toorapidly eltávolítása hello tárolóhoz és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="ea9bd-107">If you're testing hello product and aren't concerned about data loss, use hello force delete method toorapidly remove hello vault and all its dependencies.</span></span>

> <span data-ttu-id="ea9bd-108">PowerShell-paranccsal hello hello tároló összes hello tartalmának törlése, és nincs visszafejthető.</span><span class="sxs-lookup"><span data-stu-id="ea9bd-108">hello PowerShell command deletes all hello contents of hello vault and is not reversible.</span></span>

## <a name="use-powershell-tooforce-delete-hello-vault"></a><span data-ttu-id="ea9bd-109">Használjon PowerShell tooforce hello-tároló törlése</span><span class="sxs-lookup"><span data-stu-id="ea9bd-109">Use PowerShell tooforce delete hello vault</span></span> 

<span data-ttu-id="ea9bd-110">a Site Recovery tároló, még akkor is, ha nincsenek toodelete hello védett elemek, ezek a parancsok:</span><span class="sxs-lookup"><span data-stu-id="ea9bd-110">toodelete hello Site Recovery vault even if there are protected items, use these commands:</span></span>

    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionName "XXXXX"

    $vault = Get-AzureRmSiteRecoveryVault -Name "vaultname"

    Remove-AzureRmSiteRecoveryVault -Vault $vault


## <a name="delete-a-site-recovery-vault"></a><span data-ttu-id="ea9bd-111">A Site Recovery-tároló törlése</span><span class="sxs-lookup"><span data-stu-id="ea9bd-111">Delete a Site Recovery vault</span></span> 
<span data-ttu-id="ea9bd-112">toodelete hello tárolóban, kövesse hello ajánlott az a forgatókönyv lépéseit.</span><span class="sxs-lookup"><span data-stu-id="ea9bd-112">toodelete hello vault, follow hello recommended steps for your scenario.</span></span>

### <a name="vmware-vms-tooazure"></a><span data-ttu-id="ea9bd-113">VMware virtuális gépek tooAzure</span><span class="sxs-lookup"><span data-stu-id="ea9bd-113">VMware VMs tooAzure</span></span>

1. <span data-ttu-id="ea9bd-114">Töröljön minden védett virtuális gépek hello utasításait követve [tiltsa le a védelmet a VMware](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span><span class="sxs-lookup"><span data-stu-id="ea9bd-114">Delete all protected VMs by following hello steps in [Disable protection for a VMware](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span></span>

2. <span data-ttu-id="ea9bd-115">Törölje az összes replikációs házirendek hello utasításait követve [törölheti a replikációs házirendet](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span><span class="sxs-lookup"><span data-stu-id="ea9bd-115">Delete all replication policies by following hello steps in [Delete a replication policy](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span></span>

3. <span data-ttu-id="ea9bd-116">Törli a következő hello által toovCenter szükséges lépések hivatkozások [törli a vCenter](site-recovery-vmware-to-azure-manage-vCenter.md##delete-a-vcenter-in-azure-site-recovery).</span><span class="sxs-lookup"><span data-stu-id="ea9bd-116">Delete references toovCenter by following hello steps in [Delete a vCenter](site-recovery-vmware-to-azure-manage-vCenter.md##delete-a-vcenter-in-azure-site-recovery).</span></span>

4. <span data-ttu-id="ea9bd-117">Törölje a hello konfigurációs kiszolgálót hello utasításait követve [konfigurációs kiszolgáló leszerelése](site-recovery-vmware-to-azure-manage-configuration-server.md##decommissioning-a-configuration-server).</span><span class="sxs-lookup"><span data-stu-id="ea9bd-117">Delete hello configuration server by following hello steps in [Decommission a configuration server](site-recovery-vmware-to-azure-manage-configuration-server.md##decommissioning-a-configuration-server).</span></span>

5. <span data-ttu-id="ea9bd-118">Hello-tároló törlése.</span><span class="sxs-lookup"><span data-stu-id="ea9bd-118">Delete hello vault.</span></span>


### <a name="hyper-v-vms-with-virtual-machine-manager-tooazure"></a><span data-ttu-id="ea9bd-119">Hyper-V virtuális gépek (a Virtual Machine Managerrel) tooAzure</span><span class="sxs-lookup"><span data-stu-id="ea9bd-119">Hyper-V VMs (with Virtual Machine Manager) tooAzure</span></span>
1. <span data-ttu-id="ea9bd-120">Töröljön minden védett virtuális gépek hello utasításait követve [tiltsa le a védelmet a VMware virtuális gép vagy fizikai kiszolgálón](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span><span class="sxs-lookup"><span data-stu-id="ea9bd-120">Delete all protected VMs by following hello steps in [Disable protection for a VMware VM or physical server](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span></span>

2. <span data-ttu-id="ea9bd-121">Törölje az összes replikációs házirendek hello utasításait követve [törölheti a replikációs házirendet](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span><span class="sxs-lookup"><span data-stu-id="ea9bd-121">Delete all replication policies by following hello steps in [Delete a replication policy](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span></span>

3.  <span data-ttu-id="ea9bd-122">Törlés tooVirtual Machine Manager-kiszolgálók hivatkozik hello utasításait követve [csatlakoztatott VMM-kiszolgáló regisztrációját](site-recovery-manage-registration-and-protection.md##unregister-a-connected-vmm-server).</span><span class="sxs-lookup"><span data-stu-id="ea9bd-122">Delete references tooVirtual Machine Manager servers by following hello steps in [Unregister a connected VMM server](site-recovery-manage-registration-and-protection.md##unregister-a-connected-vmm-server).</span></span>

4.  <span data-ttu-id="ea9bd-123">Hello-tároló törlése.</span><span class="sxs-lookup"><span data-stu-id="ea9bd-123">Delete hello vault.</span></span>

### <a name="hyper-v-vms-without-virtual-machine-manager-tooazure"></a><span data-ttu-id="ea9bd-124">Hyper-V virtuális gépek (nélkül Virtuálisgép-kezelő) tooAzure</span><span class="sxs-lookup"><span data-stu-id="ea9bd-124">Hyper-V VMs (without Virtual Machine Manager) tooAzure</span></span>
1. <span data-ttu-id="ea9bd-125">Töröljön minden védett virtuális gépek hello utasításait követve [tiltsa le a védelmet a VMware virtuális gép vagy fizikai kiszolgálón](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span><span class="sxs-lookup"><span data-stu-id="ea9bd-125">Delete all protected VMs by following hello steps in [Disable protection for a VMware VM or physical server](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span></span>

2. <span data-ttu-id="ea9bd-126">Törölje az összes replikációs házirendek hello utasításait követve [törölheti a replikációs házirendet](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span><span class="sxs-lookup"><span data-stu-id="ea9bd-126">Delete all replication policies by following hello steps in [Delete a replication policy](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span></span>

3. <span data-ttu-id="ea9bd-127">Törlés tooHyper-V kiszolgálók hivatkozik hello utasításait követve [regisztrációjának törlése a Hyper-V gazdagépek](/site-recovery-manage-registration-and-protection.md##unregister-a-hyper-v-host-in-a-hyper-v-site).</span><span class="sxs-lookup"><span data-stu-id="ea9bd-127">Delete references tooHyper-V servers by following hello steps in [Unregister a Hyper-V host](/site-recovery-manage-registration-and-protection.md##unregister-a-hyper-v-host-in-a-hyper-v-site).</span></span>

4. <span data-ttu-id="ea9bd-128">Hello Hyper-V hely törlése.</span><span class="sxs-lookup"><span data-stu-id="ea9bd-128">Delete hello Hyper-V site.</span></span>

5. <span data-ttu-id="ea9bd-129">Hello-tároló törlése.</span><span class="sxs-lookup"><span data-stu-id="ea9bd-129">Delete hello vault.</span></span>
