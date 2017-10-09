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
# <a name="delete-a-site-recovery-vault"></a>A Site Recovery-tároló törlése
Függőségek megakadályozzák az az Azure Site Recovery-tároló törlése. hello műveletek tootake függően változhat a hello Site Recovery forgatókönyv: VMware tooAzure, Hyper-V (a és a System Center Virtual Machine Manager nélkül) tooAzure, és az Azure biztonsági mentés. egy Azure Backup szolgáltatásban használt tároló toodelete lásd [az Azure biztonsági mentési tároló törlése](../backup/backup-azure-delete-vault.md).

>[!Important]
>Ha hello termék tesztel, és nem részletesebben szeretnének foglalkozni az adatvesztés, használja hello kényszerített törlése metódus toorapidly eltávolítása hello tárolóhoz és annak függőségeit.

> PowerShell-paranccsal hello hello tároló összes hello tartalmának törlése, és nincs visszafejthető.

## <a name="use-powershell-tooforce-delete-hello-vault"></a>Használjon PowerShell tooforce hello-tároló törlése 

a Site Recovery tároló, még akkor is, ha nincsenek toodelete hello védett elemek, ezek a parancsok:

    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionName "XXXXX"

    $vault = Get-AzureRmSiteRecoveryVault -Name "vaultname"

    Remove-AzureRmSiteRecoveryVault -Vault $vault


## <a name="delete-a-site-recovery-vault"></a>A Site Recovery-tároló törlése 
toodelete hello tárolóban, kövesse hello ajánlott az a forgatókönyv lépéseit.

### <a name="vmware-vms-tooazure"></a>VMware virtuális gépek tooAzure

1. Töröljön minden védett virtuális gépek hello utasításait követve [tiltsa le a védelmet a VMware](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).

2. Törölje az összes replikációs házirendek hello utasításait követve [törölheti a replikációs házirendet](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).

3. Törli a következő hello által toovCenter szükséges lépések hivatkozások [törli a vCenter](site-recovery-vmware-to-azure-manage-vCenter.md##delete-a-vcenter-in-azure-site-recovery).

4. Törölje a hello konfigurációs kiszolgálót hello utasításait követve [konfigurációs kiszolgáló leszerelése](site-recovery-vmware-to-azure-manage-configuration-server.md##decommissioning-a-configuration-server).

5. Hello-tároló törlése.


### <a name="hyper-v-vms-with-virtual-machine-manager-tooazure"></a>Hyper-V virtuális gépek (a Virtual Machine Managerrel) tooAzure
1. Töröljön minden védett virtuális gépek hello utasításait követve [tiltsa le a védelmet a VMware virtuális gép vagy fizikai kiszolgálón](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).

2. Törölje az összes replikációs házirendek hello utasításait követve [törölheti a replikációs házirendet](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).

3.  Törlés tooVirtual Machine Manager-kiszolgálók hivatkozik hello utasításait követve [csatlakoztatott VMM-kiszolgáló regisztrációját](site-recovery-manage-registration-and-protection.md##unregister-a-connected-vmm-server).

4.  Hello-tároló törlése.

### <a name="hyper-v-vms-without-virtual-machine-manager-tooazure"></a>Hyper-V virtuális gépek (nélkül Virtuálisgép-kezelő) tooAzure
1. Töröljön minden védett virtuális gépek hello utasításait követve [tiltsa le a védelmet a VMware virtuális gép vagy fizikai kiszolgálón](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).

2. Törölje az összes replikációs házirendek hello utasításait követve [törölheti a replikációs házirendet](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).

3. Törlés tooHyper-V kiszolgálók hivatkozik hello utasításait követve [regisztrációjának törlése a Hyper-V gazdagépek](/site-recovery-manage-registration-and-protection.md##unregister-a-hyper-v-host-in-a-hyper-v-site).

4. Hello Hyper-V hely törlése.

5. Hello-tároló törlése.
