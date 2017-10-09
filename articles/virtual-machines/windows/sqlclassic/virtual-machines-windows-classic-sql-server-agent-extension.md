---
title: "SQL virtuális gép (klasszikus) aaaAutomate felügyeleti feladatai |} Microsoft Docs"
description: "Ez a témakör ismerteti, hogyan toomanage hello SQL Server agent-kiterjesztés, automatizálja az adott SQL Server felügyeleti feladatokat. Ezek közé tartoznak az automatikus biztonsági mentés, automatikus javítás és az Azure Key Vault-integráció. Ez a témakör hello klasszikus üzembe helyezési módot használ."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: a9bda2e7-cdba-427c-bc30-77cde4376f3a
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a1dc011e0526845701eaf0c365007938f9ee32ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-hello-sql-server-agent-extension-classic"></a>Hello SQL Server Agent Extension (klasszikus) Azure virtuális gépeken felügyeleti műveletek automatizálása
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-server-agent-extension.md)
> * [Klasszikus](../classic/sql-server-agent-extension.md)
> 
>
 
SQL Server infrastruktúra-szolgáltatási ügynök Extension (SQLIaaSAgent) hello futó Azure virtuális gépek tooautomate felügyeleti feladatok. Ez a témakör áttekintést hello szolgáltatások hello bővítményt, valamint utasításokat a telepítési, állapot és eltávolítási által támogatott.

> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. tooview hello erőforrás-kezelő változata ebből a cikkből: [SQL Server Agent bővítményt az SQL Server virtuális gépek erőforrás-kezelő](../sql/virtual-machines-windows-sql-server-agent-extension.md).

## <a name="supported-services"></a>Támogatott szolgáltatások
SQL Server infrastruktúra-szolgáltatási ügynök bővítmény hello felügyeleti feladatok a következő hello támogatja:

| Felügyeleti szolgáltatás | Leírás |
| --- | --- |
| **SQL automatikus biztonsági mentés** |Automatikusan hello ütemezés a biztonsági mentések az összes olyan adatbázis hello alapértelmezett példány az SQL Server a virtuális gép hello. További információkért lásd: [automatikus biztonsági mentés az SQL Server az Azure virtuális gépek (klasszikus)](../classic/sql-automated-backup.md). |
| **SQL automatikus javítás** |Konfigurálja a karbantartási időszak során mely frissítések VM is igénybe vehet tooyour helyezze el, a munkaterhelés csúcsidőben frissítések elkerülése érdekében. További információkért lásd: [automatikus javítás az SQL Server az Azure virtuális gépek (klasszikus)](../classic/sql-automated-patching.md). |
| **Azure Key Vault-integráció** |Lehetővé teszi, hogy tooautomatically telepítse és konfigurálja az Azure Key Vault az az SQL Server virtuális gép. További információkért lásd: [konfigurálása Azure Key Vault-integráció az SQL Server Azure virtuális gépeken (klasszikus)](../classic/ps-sql-keyvault.md). |

## <a name="prerequisites"></a>Előfeltételek
Követelmények toouse hello SQL Server IaaS ügynök bővítményt a virtuális gépen:

### <a name="operating-system"></a>Operációs rendszer:
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

### <a name="sql-server-versions"></a>SQL Server-verziók:
* SQL Server 2012-ben
* SQL Server 2014
* SQL Server 2016

### <a name="azure-powershell"></a>Az Azure PowerShell:
[Töltse le és konfigurálja a legújabb Azure PowerShell-parancsok hello](/powershell/azure/overview).

Indítsa el a Windows Powershellt, valamint elérheti az Azure-előfizetés tooyour hello **Add-AzureAccount** parancsot.

    Add-AzureAccount

Ha több előfizetéssel rendelkezik, **válasszon-AzureSubscription** tooselect hello előfizetés, amely tartalmazza a cél klasszikus virtuális gép.

    Select-AzureSubscription -SubscriptionName <subscriptionname>

Ezen a ponton kaphat a hello klasszikus virtuális gépek és a társított szolgáltatás nevüket hello **Get-AzureVM** parancsot.

    Get-AzureVM

## <a name="installation"></a>Telepítés
Klasszikus virtuális gépekhez PowerShell tooinstall hello SQL Server infrastruktúra-szolgáltatási ügynök bővítmény használatára és annak társított szolgáltatásaihoz konfigurálnia kell. Használjon hello **Set-AzureVMSqlServerExtension** PowerShell parancsmag tooinstall hello bővítmény. Például a következő parancs hello hello bővítmény telepítését a Windows Server virtuális gép (klasszikus) és ennek a "SQLIaaSExtension".

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -ReferenceName "SQLIaasExtension" -Version "1.2" | Update-AzureVM

Ha toohello hello SQL infrastruktúra-szolgáltatási ügynök bővítmény legújabb verziójára frissíti, hello-bővítmény frissítése után újra kell indítania a virtuális gép.

> [!NOTE]
> Klasszikus virtuális gépek nem egy beállítás tooinstall és hello portálon keresztül SQL infrastruktúra-szolgáltatási ügynök bővítmény hello konfigurálása.
> 
> 

## <a name="status"></a>status
Egyirányú tooverify, hogy telepítve van-e a hello bővítmény hello Azure Portal tooview hello ügynök állapotát. Válasszon ki egy virtuális gépet felsorolt hello virtuális gépek paneljét, és kattintson a **bővítmények**. Megtekintheti az hello **SQLIaaSAgent** felsorolt bővítmény.

![SQL Server IaaS-ügynök bővítmény Azure-portálon](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-portal.png)

Is használhatja a hello **Get-AzureVMSqlServerExtension** Azure Powershell-parancsmagot.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## <a name="removal"></a>Eltávolítás
Hello Azure portál, eltávolíthatja hello bővítmény hello három pont a hello kattintva **bővítmények** panelen található a virtuálisgép-tulajdonságokat. Kattintson a **Eltávolítás**.

![Távolítsa el a hello SQL Server infrastruktúra-szolgáltatási ügynök bővítmény Azure-portálon](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-uninstall.png)

Is használhatja a hello **Remove-AzureVMSqlServerExtension** Powershell-parancsmagot.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Remove-AzureVMSqlServerExtension | Update-AzureVM

## <a name="next-steps"></a>Következő lépések
Hello bővítmény által támogatott hello szolgáltatást használatának megkezdéséhez. További részletekért lásd: hello hivatkozott hello témakörök [támogatott szolgáltatások](#supported-services) című szakaszát.

További információ az SQL Servert futtató Azure virtuális gépeken: [SQL Server Azure virtuális gépek – áttekintés](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

