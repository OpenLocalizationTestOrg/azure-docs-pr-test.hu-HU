---
title: "aaaAutomated javítás az SQL Server VMs (klasszikus) |} Microsoft Docs"
description: "Ismerteti, az SQL Server rendszeren futó virtuális gépek az Azure-ban hello klasszikus üzembe helyezési mód hello automatikus javítás funkció."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 737b2f65-08b9-4f54-b867-e987730265a8
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.openlocfilehash: 2ef06b95705fc457605d6eb2fbc0afd490321843
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-classic"></a>Automatikus javítás az SQL Server Azure virtuális gépekben (klasszikus)
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-automated-patching.md)
> * [Klasszikus](../classic/sql-automated-patching.md)
> 
> 

Automatikus javítás karbantartási időszak az az Azure virtuális gép fut az SQL Server hoz létre. Az automatikus frissítések csak a karbantartási időszak alatt lesznek telepítve. Az SQL Server Ez biztosítja, hogy a rendszerfrissítések és minden kapcsolódó újraindítások halasztja hello lehetséges érdemes hello adatbázis. Automatikus javítás függ hello [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](../classic/sql-server-agent-extension.md).

> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. tooview hello erőforrás-kezelő változata ebből a cikkből: [automatikus javítás az SQL Server az Azure virtuális gépek erőforrás-kezelő](../sql/virtual-machines-windows-sql-automated-patching.md).

## <a name="prerequisites"></a>Előfeltételek
toouse automatikus javítás, fontolja meg a következő előfeltételek hello:

**Operációs rendszer**:

* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

**SQL Server-verzió**:

* SQL Server 2012-ben
* SQL Server 2014
* SQL Server 2016

**Az Azure PowerShell**:

* [Telepítse a legújabb Azure PowerShell-parancsok hello](/powershell/azure/overview).

**SQL Server IaaS bővítmény**:

* [Telepítse az SQL Server IaaS bővítmény hello](../classic/sql-server-agent-extension.md).

## <a name="settings"></a>Beállítások
hello következő táblázatban hello-beállítások, amelyek képesek automatikus javítás céljából. Klasszikus virtuális gépekhez PowerShell tooconfigure ezeket a beállításokat kell használnia.

| Beállítás | Lehetséges értékek | Leírás |
| --- | --- | --- |
| **Automatikus javítás** |Engedélyezi/letiltja (letiltva) |Engedélyezi vagy letiltja az automatikus javítás egy Azure virtuális géphez. |
| **Karbantartási ütemezését** |Mindennap, hétfő, kedd, szerda, csütörtök, péntek, szombat, vasárnap |hello ütemezése a virtuális gép Windows, az SQL Server és a Microsoft-frissítések letöltése és telepítése. |
| **A karbantartás indításának időpontja** |0-24 |hello helyi kezdési idő tooupdate hello virtuális gépet. |
| **Karbantartási ablak időtartama** |30-180 |hello hány perc engedélyezett toocomplete hello letöltése és a frissítések telepítését. |
| **Javítás kategória** |Fontos |hello kategóriája frissítések toodownload és telepítése. |

## <a name="configuration-with-powershell"></a>PowerShell-konfiguráció
A következő példa hello, a PowerShell használt tooconfigure egy meglévő SQL Server virtuális Gépen az automatikus javítás. Hello **New-AzureVMSqlServerAutoPatchingConfig** parancs konfigurálja az automatikus frissítések egy új karbantartási időszakot.

    $aps = New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoPatchingSettings $aps | Update-AzureVM

Ez a példa alapján, hello következő táblázatban hello gyakorlati hatással hello cél Azure virtuális Gépen.

| Paraméter | Következmény |
| --- | --- |
| **DayOfWeek** |Javítások minden csütörtök telepítve. |
| **MaintenanceWindowStartingHour** |A kezdő frissítések 11:00 órakor. |
| **MaintenanceWindowsDuration** |Javítások 120 percen belül kell telepíteni. Hello kezdési ideje alapján, el kell végezniük 1:00 pm által. |
| **PatchCategory** |hello csak lehetséges Ez a paraméter értéke "Fontos". |

Nem sikerült eltarthat néhány percig tooinstall és hello SQL Server IaaS-ügynök konfigurálása.

toodisable automatikus javítás, azonos nélkül parancsfájl futtatási hello hello - Enable paramétert a New-AzureVMSqlServerAutoPatchingConfig toohello. Mivel a telepítés folytatásához is igénybe vehet néhány percet toodisable automatikus javítás.

## <a name="next-steps"></a>Következő lépések
Más elérhető automation feladatokkal kapcsolatos további információkért lásd: [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](../classic/sql-server-agent-extension.md).

További információ az Azure virtuális gépeken futó SQL Server: [SQL Server Azure virtuális gépek – áttekintés](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

