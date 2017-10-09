---
title: aaaScheduler PowerShell parancsmag-referencia
description: "Feladatütemező – PowerShell-parancsmagok referencia"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 9a26c457-d7a1-4e4a-bc79-f26592155218
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: a2b23bcd3e4493ffba1dbf21fbb87818be7c01e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scheduler-powershell-cmdlets-reference"></a>Feladatütemező – PowerShell-parancsmagok referencia
hello a következő táblázat ismerteti, és hivatkozásokat tartalmaz az egyes hello fő parancsmagok a Azure Scheduler toohello referencialapja.

Azure PowerShell tooinstall és rendelje hozzá azt az Azure-előfizetéssel, lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview). 

További információ [Azure Resource Manager parancsmagjainak](/powershell/azure/overview), lásd: [az Azure PowerShell használata Azure Resource Managerrel](../powershell-azure-resource-manager.md).

| Parancsmag | A parancsmag leírása |
| --- | --- |
| [Disable-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/disable-azurermschedulerjobcollection) |A feladatgyűjtemény letiltása. |
| [Enable-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/enable-azurermschedulerjobcollection) |Lehetővé teszi, hogy a feladatgyűjtemény. |
| [Get-AzureRmSchedulerJob](/powershell/module/azurerm.scheduler/get-azurermschedulerjob) |A Feladatütemező szolgáltatás lekérdezi. |
| [Get-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/get-azurermschedulerjobcollection) |Lekérdezi a gyűjtemények feladat. |
| [Get-AzureRmSchedulerJobHistory](/powershell/module/azurerm.scheduler/get-azurermschedulerjobhistory) |Lekérdezi a feladatelőzmények. |
| [Új AzureRmSchedulerHttpJob](/powershell/module/azurerm.scheduler/new-azurermschedulerhttpjob) |Létrehoz egy HTTP-feladatot. |
| [Új AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/new-azurermschedulerjobcollection) |A feladatgyűjtemény hoz létre. |
| [Új AzureRmSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebusqueuejob) |Létrehoz egy service bus várólista feladatot. |
| [Új AzureRmSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebustopicjob) |Létrehoz egy service bus témakör feladatot. |
| [Új AzureRmSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerstoragequeuejob) |Létrehoz egy tároló várólista feladatot. |
| [Remove-AzureRmSchedulerJob](/powershell/module/azurerm.scheduler/remove-azurermschedulerjob) |Eltávolítja a Scheduler feladatot. |
| [Remove-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/remove-azurermschedulerjobcollection) |A feladatgyűjtemény eltávolítja. |
| [Set-AzureRmSchedulerHttpJob](/powershell/module/azurerm.scheduler/set-azurermschedulerhttpjob) |A Feladatütemező HTTP feladat módosítása. |
| [Set-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/set-azurermschedulerjobcollection) |A feladatgyűjtemény módosítja. |
| [Set-AzureRmSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebusqueuejob) |Módosítja egy service bus várólista feladat. |
| [Set-AzureRmSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebustopicjob) |Módosítja egy service bus témakör feladat. |
| [Set-AzureRmSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerstoragequeuejob) |Módosítja egy sor feladat. |

Részletesebb információkért hello a következő parancsmagok bármelyikével futtathatja: 

```
Get-Help <cmdlet name> -Detailed
```
```
Get-Help <cmdlet name> -Examples
```
```
Get-Help <cmdlet name> -Full
```

## <a name="see-also"></a>Lásd még:
 [A Scheduler ismertetése](scheduler-intro.md)

 [Az Azure Scheduler alapfogalmai, terminológiája és entitáshierarchiája](scheduler-concepts-terms.md)

 [Az ütemező hello Azure-portálon az első lépéseiben](scheduler-get-started-portal.md)

 [Csomagok és számlázás az Azure Schedulerben](scheduler-plans-billing.md)

 [Az Azure Scheduler REST API-jának leírása](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler – magas fokú rendelkezésre állás és megbízhatóság](scheduler-high-availability-reliability.md)

 [Azure Scheduler – korlátozások, alapértékek és hibakódok](scheduler-limits-defaults-errors.md)

 [Kimenő hitelesítés az Azure Schedulerben](scheduler-outbound-authentication.md)

