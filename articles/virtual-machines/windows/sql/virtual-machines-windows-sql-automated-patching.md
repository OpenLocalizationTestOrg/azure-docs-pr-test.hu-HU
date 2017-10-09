---
title: "aaaAutomated javítás az SQL Server virtuális géphez (Resource Manager) |} Microsoft Docs"
description: "Ismerteti, az SQL Server rendszeren futó virtuális gépek az Azure Resource Manager használatával hello automatikus javítás funkció."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: 58232e92-318f-456b-8f0a-2201a541e08d
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.openlocfilehash: 8bb8d0fb265e69d7bbf1fa047f5ceef02e7c56fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Az SQL Server automatikus javítása az Azure Virtual Machines szolgáltatásban (Resource Manager)
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-automated-patching.md)
> * [Klasszikus](../classic/sql-automated-patching.md)
> 
> 

Automatikus javítás karbantartási időszak az az Azure virtuális gép fut az SQL Server hoz létre. Az automatikus frissítések csak a karbantartási időszak alatt lesznek telepítve. Az SQL Server a rescriction biztosítja, hogy a rendszerfrissítések és minden kapcsolódó újraindítások halasztja hello lehetséges érdemes hello adatbázis. Automatikus javítás függ hello [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

tooview hello klasszikus ebben a cikkben forduljon [automatikus javítás az SQL Server Azure virtuális gépek Classic](../classic/sql-automated-patching.md).

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

* [Telepítse a legújabb Azure PowerShell-parancsok hello](/powershell/azure/overview) Ha azt tervezi, hogy tooconfigure automatikus javítás a PowerShell használatával.

> [!NOTE]
> Automatikus javítás az SQL Server infrastruktúra-szolgáltatási ügynök bővítmény hello támaszkodik. A gyűjtemény lemezképei aktuális SQL virtuális gép alapértelmezés szerint adja hozzá ezt a bővítményt. További információkért lásd: [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md).
> 
> 

## <a name="settings"></a>Beállítások
hello következő táblázatban hello-beállítások, amelyek képesek automatikus javítás céljából. hello tényleges konfigurációs lépései eltérőek attól függően, hogy használ-e hello Azure-portálon vagy az Azure PowerShell-parancsokkal.

| Beállítás | Lehetséges értékek | Leírás |
| --- | --- | --- |
| **Automatikus javítás** |Engedélyezi/letiltja (letiltva) |Engedélyezi vagy letiltja az automatikus javítás egy Azure virtuális géphez. |
| **Karbantartási ütemezését** |Mindennap, hétfő, kedd, szerda, csütörtök, péntek, szombat, vasárnap |hello ütemezése a virtuális gép Windows, az SQL Server és a Microsoft-frissítések letöltése és telepítése. |
| **A karbantartás indításának időpontja** |0-24 |hello helyi kezdési idő tooupdate hello virtuális gépet. |
| **Karbantartási ablak időtartama** |30-180 |hello hány perc engedélyezett toocomplete hello letöltése és a frissítések telepítését. |
| **Javítás kategória** |Fontos |hello kategóriája frissítések toodownload és telepítése. |

## <a name="configuration-in-hello-portal"></a>A portál hello konfiguráció
Használhatja az Azure portál tooconfigure hello kiépítése során, vagy meglévő virtuális gépek automatikus javítás.

### <a name="new-vms"></a>Új virtuális gépek
Használjon hello Azure portál tooconfigure automatikus javítás, amikor létrehoz egy új SQL Server virtuális gép hello Resource Manager üzembe helyezési modellben.

A hello **SQL Server-beállítások** panelen válassza **automatikus javítás**. hello Azure portál következő képernyőfelvételen látható hello **SQL automatikus javítás** panelen.

![SQL automatikus javítás Azure-portálon](./media/virtual-machines-windows-sql-automated-patching/azure-sql-arm-patching.png)

A környezetben, témakörében hello teljes [az Azure SQL Server virtuális gépek kiépítése](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Meglévő virtuális gépek
Meglévő SQL Server virtuális gépek válassza ki az SQL Server virtuális gépet. Válassza ki hello **SQL Server-konfigurációs** hello szakasza **beállítások** panelen.

![SQL automatikus javítás meglévő virtuális gépek](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-existing-vms.png)

A hello **SQL Server-konfigurációs** panelen kattintson hello **szerkesztése** hello gombjára automatikus javítási szakasz.

![Meglévő virtuális gépek SQL automatikus javítás beállítása](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-configuration.png)

Ha elkészült, kattintson a hello **OK** hello alsó részén hello gombjára **SQL Server-konfigurációs** panel toosave a módosításokat.

Ha engedélyezi az automatikus javítás hello az első alkalommal, Azure SQL Server IaaS-ügynök hello hello háttérben konfigurálja. Ebben az időszakban hello Azure-portálon előfordulhat, hogy jelenjen meg, hogy konfigurálva van-e az automatikus javítás. Várjon egy pár percet hello ügynök toobe telepítve, a konfigurált. Követően, hogy hello Azure portal tükrözi hello új beállítások.

> [!NOTE]
> Automatikus javítás sablon használatával is konfigurálhatja. További információkért lásd: [Azure gyors üzembe helyezés sablon automatikus javítás céljából](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autopatching-update).
> 
> 

## <a name="configuration-with-powershell"></a>PowerShell-konfiguráció
Miután az SQL virtuális gép kiépítése, használja a PowerShell tooconfigure automatikus javítás.

A következő példa hello, a PowerShell használt tooconfigure egy meglévő SQL Server virtuális Gépen az automatikus javítás. Hello **AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig** parancs konfigurálja az automatikus frissítések egy új karbantartási időszakot.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $aps = AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Set-AzureRmVMSqlServerExtension -AutoPatchingSettings $aps -VMName $vmname -ResourceGroupName $resourcegroupname

Ez a példa alapján, hello következő táblázatban hello gyakorlati hatással hello cél Azure virtuális Gépen.

| Paraméter | Következmény |
| --- | --- |
| **DayOfWeek** |Javítások minden csütörtök telepítve. |
| **MaintenanceWindowStartingHour** |A kezdő frissítések 11:00 órakor. |
| **MaintenanceWindowsDuration** |Javítások 120 percen belül kell telepíteni. Hello kezdési ideje alapján, el kell végezniük 1:00 pm által. |
| **PatchCategory** |hello kapcsolatos Ez a paraméter csak lehetséges beállítása **fontos**. |

Nem sikerült eltarthat néhány percig tooinstall és hello SQL Server IaaS-ügynök konfigurálása.

toodisable automatikus javítás, azonos hello nélkül parancsfájl futtatási hello **-engedélyezése** paraméter toohello **AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig**. hello hiányában hello **-engedélyezése** paraméter jelek hello parancs toodisable hello szolgáltatást.

## <a name="next-steps"></a>Következő lépések
Más elérhető automation feladatokkal kapcsolatos további információkért lásd: [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md).

További információ az Azure virtuális gépeken futó SQL Server: [SQL Server Azure virtuális gépek – áttekintés](virtual-machines-windows-sql-server-iaas-overview.md).

