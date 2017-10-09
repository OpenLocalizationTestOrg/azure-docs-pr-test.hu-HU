---
title: "SQL virtuális gépek (erőforrás-kezelő) aaaAutomate fájlkezelési feladatai |} Microsoft Docs"
description: "Ez a témakör ismerteti, hogyan toomanage hello SQL Server agent-kiterjesztés, automatizálja az adott SQL Server felügyeleti feladatokat. Ezek közé tartoznak az automatikus biztonsági mentés, automatikus javítás és az Azure Key Vault-integráció. Ez a témakör hello erőforrás-kezelő telepítési módot használ."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: effe4e2f-35b5-490a-b5ef-b06746083da4
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/07/2017
ms.author: jroth
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ae917612c4af59f12c0b083440673bdc555e9d56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-hello-sql-server-agent-extension-resource-manager"></a>SQL Server Agent bővítmény (erőforrás-kezelő) hello Azure virtuális gépeken felügyeleti műveletek automatizálása
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-server-agent-extension.md)
> * [Klasszikus](../classic/sql-server-agent-extension.md)
> 
> 

SQL Server infrastruktúra-szolgáltatási ügynök Extension (SQLIaaSExtension) hello futó Azure virtuális gépek tooautomate felügyeleti feladatok. Ez a témakör áttekintést hello szolgáltatások hello bővítményt, valamint utasításokat a telepítési, állapot és eltávolítási által támogatott.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

tooview hello klasszikus ebben a cikkben forduljon [SQL Server Agent bővítmény SQL Server VMs Classic](../classic/sql-server-agent-extension.md).

## <a name="supported-services"></a>Támogatott szolgáltatások
SQL Server infrastruktúra-szolgáltatási ügynök bővítmény hello felügyeleti feladatok a következő hello támogatja:

| Felügyeleti szolgáltatás | Leírás |
| --- | --- |
| **SQL automatikus biztonsági mentés** |Automatikusan hello ütemezés a biztonsági mentések az összes olyan adatbázis hello alapértelmezett példány az SQL Server a virtuális gép hello. További információkért lásd: [automatikus biztonsági mentés az SQL Server az Azure virtuális gépek (erőforrás-kezelő)](virtual-machines-windows-sql-automated-backup.md). |
| **SQL automatikus javítás** |Konfigurálja a karbantartási időszak során mely frissítések VM is igénybe vehet tooyour helyezze el, a munkaterhelés csúcsidőben frissítések elkerülése érdekében. További információkért lásd: [automatikus javítás az SQL Server az Azure virtuális gépek (erőforrás-kezelő)](virtual-machines-windows-sql-automated-patching.md). |
| **Azure Key Vault-integráció** |Lehetővé teszi, hogy tooautomatically telepítse és konfigurálja az Azure Key Vault az az SQL Server virtuális gép. További információkért lásd: [konfigurálása Azure Key Vault-integráció az SQL Server Azure virtuális gépeken (erőforrás-kezelő)](virtual-machines-windows-ps-sql-keyvault.md). |

Amikor telepítve és fut, hello SQL Server infrastruktúra-szolgáltatási ügynök bővítmény elérhetővé teszi ezeket a felügyeleti funkciókat hello SQL Server panelen hello virtuális gép hello Azure portálon, és Azure PowerShell használatával az SQL Server piactéren elérhető rendszerkép és keresztül Az Azure PowerShell hello bővítmény manuális telepítésekor. 

## <a name="prerequisites"></a>Előfeltételek
Követelmények toouse hello SQL Server IaaS ügynök bővítményt a virtuális gépen:

**Operációs rendszer**:

* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

**SQL Server-verziók**:

* SQL Server 2012-ben
* SQL Server 2014
* SQL Server 2016

**Az Azure PowerShell**:

* [Töltse le és konfigurálja a hello legújabb Azure PowerShell-parancsok](/powershell/azure/overview)

## <a name="installation"></a>Telepítés
hello SQL Server virtuális gép a gyűjtemény lemezképei kiépítése a rendszer automatikusan telepíti az SQL Server infrastruktúra-szolgáltatási ügynök bővítmény hello. Ha tooreinstall hello bővítmény manuálisan az SQL Server virtuális gépeken egyik van szüksége, használja a következő PowerShell-paranccsal hello:

```powershell
Set-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension" -Version "1.2" -Location "East US 2"
```

Akkor is lehetséges tooinstall hello SQL Server infrastruktúra-szolgáltatási ügynök kiterjesztés csak az operációs rendszer Windows Server virtuális géphez. Ez csak akkor támogatott, ha manuálisan is, hogy a gép van telepítve az SQL Server. Majd a telepítés hello bővítmény használatával manuálisan hello azonos **Set-AzureVMSqlServerExtension** PowerShell-parancsmagot.

> [!NOTE]
> Ha manuálisan telepíti az SQL Server infrastruktúra-szolgáltatási ügynök bővítmény hello egy csak az operációs rendszer Windows Server virtuális gépen, nem kezelheti hello SQL Server-konfigurációs beállítások hello Azure-portálon keresztül. Ebben a forgatókönyvben módosítania kell az összes a PowerShell használatával.

## <a name="status"></a>status
Egyirányú tooverify, hogy telepítve van-e a hello bővítmény hello Azure Portal tooview hello ügynök állapotát. Válassza ki **összes beállítás** a hello virtuális gépek paneljét, és kattintson a **bővítmények**. Megtekintheti az hello **SQLIaaSExtension** felsorolt bővítmény.

![SQL Server IaaS-ügynök bővítmény Azure-portálon](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

Is használhatja a hello **Get-AzureVMSqlServerExtension** Azure Powershell-parancsmagot.

    Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

előző parancs hello megerősíti, hogy hello ügynök telepítve van, és az általános állapotadatokat szolgáltat. Automatikus biztonsági mentés és javításokkal bizonyos állapot információt a következő parancsok hello is lekérdezni.

    $sqlext = Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings

## <a name="removal"></a>Eltávolítás
Hello Azure portál, eltávolíthatja hello bővítmény hello három pont a hello kattintva **bővítmények** panelen található a virtuálisgép-tulajdonságokat. Kattintson a **törlése**.

![Távolítsa el a hello SQL Server infrastruktúra-szolgáltatási ügynök bővítmény Azure-portálon](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

Is használhatja a hello **Remove-AzureRmVMSqlServerExtension** Powershell-parancsmagot.

    Remove-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension"

## <a name="next-steps"></a>Következő lépések
Hello bővítmény által támogatott hello szolgáltatást használatának megkezdéséhez. További részletekért lásd: hello hivatkozott hello témakörök [támogatott szolgáltatások](#supported-services) című szakaszát.

További információ az SQL Servert futtató Azure virtuális gépeken: [SQL Server Azure virtuális gépek – áttekintés](virtual-machines-windows-sql-server-iaas-overview.md).

