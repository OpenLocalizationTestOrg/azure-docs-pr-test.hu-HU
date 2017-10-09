---
title: aaaManage Azure Analysis Services a PowerShell-lel |} Microsoft Docs
description: "Az Azure Analysis Services kezelése a PowerShell használatával."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: owend
ms.openlocfilehash: bc4250bf77b5a0d86c1049ee57493bcf2a1f0c1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-analysis-services-with-powershell"></a>A PowerShell segítségével az Azure Analysis Services kezelése

Ez a cikk ismerteti a PowerShell-parancsmagok tooperform Azure Analysis Services-kiszolgáló és adatbázis felügyeleti feladatokat. 

Server kezelési feladatainak például létrehozása vagy kiszolgáló törlése, felfüggesztése vagy folytatása a kiszolgáló műveletek vagy hello szolgáltatási szint (réteg) módosítása az Azure Resource Manager (AzureRM) parancsmagok használata. Egyéb feladatok adatbázisok kezeléséhez, például fel szerepkör tagjai, feldolgozás, vagy nem használható parancsmagokkal particionálás szereplő hello SQL Server Analysis Services azonos SqlServer modul.

## <a name="permissions"></a>Engedélyek
A legtöbb PowerShell feladatok szükséges kezelt hello Analysis Services kiszolgálón rendszergazdai jogosultsággal rendelkezik. Felügyelet nélküli műveletek PowerShell ütemezett feladatok, amelyek. hello Feladatütemező hello használt fiók rendszergazdai jogosultságokat kell hello Analysis Services-kiszolgálóhoz. 

A kiszolgáló műveleteket AzureRm-parancsmagok használatával, a fiók vagy hello használt ütemező fiók is tartoznia kell toohello tulajdonosi szerepkör hello erőforrás a [átruházásához hozzáférés-vezérlés (RBAC)](../active-directory/role-based-access-control-what-is.md). 

## <a name="server-operations"></a>Kiszolgáló műveletei 
Az Azure Analysis Services-parancsmagok hello szerepelnek [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices) összetevő modul. tooinstall AzureRM parancsmag modulok: [Azure Resource Manager parancsmagjainak](/powershell/azure/overview) a PowerShell-galériában hello.

|Parancsmag|Leírás| 
|------------|-----------------| 
|[Export-AzureAnalysisServicesInstance](/powershell/module/azurerm.analysisservices/export-azureanalysisservicesinstancelog)|Export toofile naplózása.| 
|[Get-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/get-azurermanalysisservicesserver)|Lekérdezi a server-példány részletes adatait.|  
|[Új AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver)|Hoz létre egy kiszolgálópéldányt.|
|[Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/remove-azurermanalysisservicesserver)|Eltávolítja a server-példányt.|  
|[Suspend-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/suspend-azurermanalysisservicesserver)|Felfüggeszti a server-példányt.| 
|[RESUME-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/resume-azurermanalysisservicesserver)|A server-példány folytatja.|  
|[Set-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver)|A server-példány módosítása.|   
|[Teszt-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/test-azurermanalysisservicesserver)|A server-példány tesztek hello megléte.| 

## <a name="database-operations"></a>Adatbázis-műveletek

Az Azure Analysis Services adatbázis műveletei pedig hello azonos [SqlServer](https://www.powershellgallery.com/packages/SqlServer) SQL Server Analysis Services modul. Azonban nem minden parancsmagok Azure Analysis Services használata támogatott. 

hello SqlServer modul adja meg a tevékenység-specifikus adatbázis parancsmagokat, valamint hello általános célú Invoke-ASCmd parancsmag, amely fogad egy táblázatos modell Scripting Language (TMSL) lekérdezés vagy parancsfájl. hello hello SqlServer modul a következő parancsmagok használhatók az Azure Analysis Services.

  
|Parancsmag|Leírás|
|------------|-----------------| 
|[Adja hozzá RoleMember](https://msdn.microsoft.com/library/hh510167.aspx)|Egy tag tooa adatbázis-szerepkör hozzáadása.| 
|[Biztonsági mentés-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet)|Analysis Services-adatbázis biztonsági mentése.|  
|[Remove-RoleMember](https://msdn.microsoft.com/library/hh510173.aspx)|Tag eltávolítása egy adatbázis-szerepkör.|   
|[Invoke-ASCmd](https://msdn.microsoft.com/library/hh479579.aspx)|Hajtsa végre a TMSL parancsfájlt.|
|[Invoke-ProcessASDatabase](https://msdn.microsoft.com/library/mt651773.aspx)|Egy adatbázis feldolgozni.|  
|[Invoke-ProcessPartition](https://msdn.microsoft.com/library/hh510164.aspx)|A partíció feldolgozásához.| 
|[Invoke-ProcessTable](https://msdn.microsoft.com/library/mt651774.aspx)|Egy tábla feldolgozni.|  
|[Egyesítési-partíció](https://msdn.microsoft.com/library/hh479576.aspx)|Egy partíció egyesíteni.|  
|[Visszaállítás-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet)|Analysis Services-adatbázis visszaállítása.| 
  

## <a name="related-information"></a>Kapcsolódó információk

* [SQL-kiszolgáló PowerShell-modul letöltése](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [Töltse le a szolgáltatáshoz az SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   
* [SQL Server-modul a PowerShell-galériában](https://www.powershellgallery.com/packages/SqlServer)    
* [A táblázatos modell programozási kompatibilitási szintet 1200-as és újabb rendszer](https://msdn.microsoft.com/library/mt712541.aspx)
