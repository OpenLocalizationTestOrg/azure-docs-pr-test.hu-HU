---
title: "jelentések aaaAccess – Azure RBAC |} Microsoft Docs"
description: "Hogy felsorolja az összes módosításait hozzáférés tooyour szerepköralapú hozzáférés-vezérléssel keresztül hello Azure-előfizetések az elmúlt 90 napban jelentés készítése."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
ms.assetid: 2bc68595-145e-4de3-8b71-3a21890d13d9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/17/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9ad85d3d8e66ce167032638a35e4afffb46d3892
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-access-report-for-role-based-access-control"></a>A szerepköralapú hozzáférés-vezérléshez access jelentés létrehozása
Valaki engedélyezi, vagy visszavonja a hozzáférési jogosultságok előfizetése, amikor az Azure események hello módosítások naplózásra. Létrehozhat hozzáférés módosítási előzményeit jelentések toosee hello összes változás az elmúlt 90 napban.

## <a name="create-a-report-with-azure-powershell"></a>Az Azure PowerShell-jelentés létrehozása
hozzáférés toocreate módosítása teljesítményelőzményeinek jelentése a PowerShellben használja hello [Get-AzureRMAuthorizationChangeLog](/powershell/module/azurerm.resources/get-azurermauthorizationchangelog) parancsot.

Ez a parancs hívásakor mely hello-hozzárendelésekhez felsorolt, beleértve a következőket hello tulajdonsága adhat meg:

| Tulajdonság | Leírás |
| --- | --- |
| **Művelet** |E hozzáférést kap vagy visszavonása |
| **Hívó** |hello hozzáférés felelős hello tulajdonos módosítása |
| **PrincipalId** | hello hello felhasználó, csoport vagy hello szerepkör rendelte alkalmazás egyedi azonosítója |
| **Egyszerű név** |hello hello felhasználó, csoport vagy alkalmazás neve |
| **PrincipalType** |Hello hozzárendelés felhasználó, csoport vagy alkalmazás volt-e |
| **Roledefinitionid-értékkel** |hello megadott vagy visszavont hello szerepkör GUID azonosítója |
| **RoleName** |megadott vagy visszavont hello szerepkör |
| **Hatókör** | hello egyedi azonosítója hello előfizetés, erőforrás vagy az erőforrások, amelyek a hozzárendelés hello túl vonatkozik| 
| **ScopeName** |hello hello előfizetés, az erőforráscsoportot, vagy az erőforrás neve |
| **ScopeType** |Hello hozzárendelés hello előfizetés, erőforráscsoporthoz vagy erőforrás hatókör volt-e |
| **Időbélyeg** |hello dátum és idő, amelyet megváltozott |

Példa felsorolja az összes változásokat hello az előfizetéshez tartozó hello elmúlt hét napban:

```
Get-AzureRMAuthorizationChangeLog -StartTime ([DateTime]::Now - [TimeSpan]::FromDays(7)) | FT Caller,Action,RoleName,PrincipalType,PrincipalName,ScopeType,ScopeName
```

![PowerShell Get-AzureRMAuthorizationChangeLog – képernyőkép](./media/role-based-access-control-configure/access-change-history.png)

## <a name="create-a-report-with-azure-cli"></a>Az Azure CLI-jelentés létrehozása
toocreate hello Azure parancssori felület (CLI), az access módosítás előzmények jelentés használata hello `azure role assignment changelog list` parancsot.

## <a name="export-tooa-spreadsheet"></a>Tooa számolótábla exportálása
toosave jelentés hello vagy kezelhetők hello adatok, az Exportálás hello hozzáférés módosítja egy .csv-fájlba. Megtekintheti a hello jelentés felülvizsgálati összegyűjtheti egy táblázatban.

![Változásnaplója tekinthetők meg a táblázat – képernyőkép](./media/role-based-access-control-configure/change-history-spreadsheet.png)

## <a name="next-steps"></a>Következő lépések
* Együttműködve [egyéni szerepkörök az Azure RBAC](role-based-access-control-custom-roles.md)
* Megtudhatja, hogyan toomanage [Azure RBAC a PowerShell használatával](role-based-access-control-manage-access-powershell.md)

