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
# <a name="delete-a-site-recovery-vault"></a>A Site Recovery-tároló törlése
Függőségek megakadályozzák az az Azure Site Recovery-tároló törlése. A Site Recovery forgatókönyvön függően változnak, milyen lépéseket kell tennie: az Azure-ba, a Hyper-V (a és a System Center Virtual Machine Manager nélkül) Azure és az Azure Backup VMware. Törli a tárolót, az Azure Backup szolgáltatáshoz használt, lásd: [az Azure biztonsági mentési tároló törlése](../backup/backup-azure-delete-vault.md).

>[!Important]
>Ha a termék tesztel, és nem részletesebben szeretnének foglalkozni az adatvesztés, a kényszerített delete metódus segítségével gyorsan távolítsa el a tárolóhoz és annak függőségeit.

> A PowerShell-parancsot a tároló tartalmának törlése, és nincs visszafejthető.

## <a name="use-powershell-to-force-delete-the-vault"></a>A PowerShell szolgáltatás használatával kényszerítheti a tároló törlése 

A Site Recovery-tároló törlése, még akkor is, ha nincsenek védett elemek, ezen parancsok használatával:

    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionName "XXXXX"

    $vault = Get-AzureRmSiteRecoveryVault -Name "vaultname"

    Remove-AzureRmSiteRecoveryVault -Vault $vault


## <a name="delete-a-site-recovery-vault"></a>A Site Recovery-tároló törlése 
Törli a tárolót, hajtsa végre a javasolt lépéseket a forgatókönyvéhez.

### <a name="vmware-vms-to-azure"></a>VMware virtuális gépek az Azure-ba

1. Töröljön minden védett virtuális gépek a megfelelő lépéseket követve [tiltsa le a védelmet a VMware](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).

2. Minden replikációs házirendek törlése lépéseit követve [törölheti a replikációs házirendet](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).

3. Törli a vCenter mutató hivatkozások lépéseit követve [törli a vCenter](site-recovery-vmware-to-azure-manage-vCenter.md##delete-a-vcenter-in-azure-site-recovery).

4. A konfigurációs kiszolgáló törlése a lépések [konfigurációs kiszolgáló leszerelése](site-recovery-vmware-to-azure-manage-configuration-server.md##decommissioning-a-configuration-server).

5. Törli a tárolót.


### <a name="hyper-v-vms-with-virtual-machine-manager-to-azure"></a>A Hyper-V virtuális gépek (a Virtual Machine Managerrel) az Azure-bA
1. Töröljön minden védett virtuális gépek a megfelelő lépéseket követve [tiltsa le a védelmet a VMware virtuális gép vagy fizikai kiszolgálón](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).

2. Minden replikációs házirendek törlése lépéseit követve [törölheti a replikációs házirendet](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).

3.  A Virtual Machine Manager-kiszolgálók hivatkoznak törlése lépéseit követve [csatlakoztatott VMM-kiszolgáló regisztrációját](site-recovery-manage-registration-and-protection.md##unregister-a-connected-vmm-server).

4.  Törli a tárolót.

### <a name="hyper-v-vms-without-virtual-machine-manager-to-azure"></a>A Hyper-V virtuális gépek (nélkül Virtuálisgép-kezelő) az Azure-bA
1. Töröljön minden védett virtuális gépek a megfelelő lépéseket követve [tiltsa le a védelmet a VMware virtuális gép vagy fizikai kiszolgálón](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).

2. Minden replikációs házirendek törlése lépéseit követve [törölheti a replikációs házirendet](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).

3. Hyper-V kiszolgálók mutató hivatkozások törlése a lépések [regisztrációjának törlése a Hyper-V gazdagépek](/site-recovery-manage-registration-and-protection.md##unregister-a-hyper-v-host-in-a-hyper-v-site).

4. A Hyper-V hely törlése.

5. Törli a tárolót.
