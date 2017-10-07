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
# <a name="create-an-access-report-for-role-based-access-control"></a><span data-ttu-id="7b11f-103">A szerepköralapú hozzáférés-vezérléshez access jelentés létrehozása</span><span class="sxs-lookup"><span data-stu-id="7b11f-103">Create an access report for Role-Based Access Control</span></span>
<span data-ttu-id="7b11f-104">Valaki engedélyezi, vagy visszavonja a hozzáférési jogosultságok előfizetése, amikor az Azure események hello módosítások naplózásra.</span><span class="sxs-lookup"><span data-stu-id="7b11f-104">Any time someone grants or revokes access within your subscriptions, hello changes get logged in Azure events.</span></span> <span data-ttu-id="7b11f-105">Létrehozhat hozzáférés módosítási előzményeit jelentések toosee hello összes változás az elmúlt 90 napban.</span><span class="sxs-lookup"><span data-stu-id="7b11f-105">You can create access change history reports toosee all changes for hello past 90 days.</span></span>

## <a name="create-a-report-with-azure-powershell"></a><span data-ttu-id="7b11f-106">Az Azure PowerShell-jelentés létrehozása</span><span class="sxs-lookup"><span data-stu-id="7b11f-106">Create a report with Azure PowerShell</span></span>
<span data-ttu-id="7b11f-107">hozzáférés toocreate módosítása teljesítményelőzményeinek jelentése a PowerShellben használja hello [Get-AzureRMAuthorizationChangeLog](/powershell/module/azurerm.resources/get-azurermauthorizationchangelog) parancsot.</span><span class="sxs-lookup"><span data-stu-id="7b11f-107">toocreate an access change history report in PowerShell, use hello [Get-AzureRMAuthorizationChangeLog](/powershell/module/azurerm.resources/get-azurermauthorizationchangelog) command.</span></span>

<span data-ttu-id="7b11f-108">Ez a parancs hívásakor mely hello-hozzárendelésekhez felsorolt, beleértve a következőket hello tulajdonsága adhat meg:</span><span class="sxs-lookup"><span data-stu-id="7b11f-108">When you call this command, you can specify which property of hello assignments you want listed, including hello following:</span></span>

| <span data-ttu-id="7b11f-109">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="7b11f-109">Property</span></span> | <span data-ttu-id="7b11f-110">Leírás</span><span class="sxs-lookup"><span data-stu-id="7b11f-110">Description</span></span> |
| --- | --- |
| <span data-ttu-id="7b11f-111">**Művelet**</span><span class="sxs-lookup"><span data-stu-id="7b11f-111">**Action**</span></span> |<span data-ttu-id="7b11f-112">E hozzáférést kap vagy visszavonása</span><span class="sxs-lookup"><span data-stu-id="7b11f-112">Whether access was granted or revoked</span></span> |
| <span data-ttu-id="7b11f-113">**Hívó**</span><span class="sxs-lookup"><span data-stu-id="7b11f-113">**Caller**</span></span> |<span data-ttu-id="7b11f-114">hello hozzáférés felelős hello tulajdonos módosítása</span><span class="sxs-lookup"><span data-stu-id="7b11f-114">hello owner responsible for hello access change</span></span> |
| <span data-ttu-id="7b11f-115">**PrincipalId**</span><span class="sxs-lookup"><span data-stu-id="7b11f-115">**PrincipalId**</span></span> | <span data-ttu-id="7b11f-116">hello hello felhasználó, csoport vagy hello szerepkör rendelte alkalmazás egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="7b11f-116">hello unique identifier of hello user, group, or application that was assigned hello role</span></span> |
| <span data-ttu-id="7b11f-117">**Egyszerű név**</span><span class="sxs-lookup"><span data-stu-id="7b11f-117">**PrincipalName**</span></span> |<span data-ttu-id="7b11f-118">hello hello felhasználó, csoport vagy alkalmazás neve</span><span class="sxs-lookup"><span data-stu-id="7b11f-118">hello name of hello user, group, or application</span></span> |
| <span data-ttu-id="7b11f-119">**PrincipalType**</span><span class="sxs-lookup"><span data-stu-id="7b11f-119">**PrincipalType**</span></span> |<span data-ttu-id="7b11f-120">Hello hozzárendelés felhasználó, csoport vagy alkalmazás volt-e</span><span class="sxs-lookup"><span data-stu-id="7b11f-120">Whether hello assignment was for a user, group, or application</span></span> |
| <span data-ttu-id="7b11f-121">**Roledefinitionid-értékkel**</span><span class="sxs-lookup"><span data-stu-id="7b11f-121">**RoleDefinitionId**</span></span> |<span data-ttu-id="7b11f-122">hello megadott vagy visszavont hello szerepkör GUID azonosítója</span><span class="sxs-lookup"><span data-stu-id="7b11f-122">hello GUID of hello role that was granted or revoked</span></span> |
| <span data-ttu-id="7b11f-123">**RoleName**</span><span class="sxs-lookup"><span data-stu-id="7b11f-123">**RoleName**</span></span> |<span data-ttu-id="7b11f-124">megadott vagy visszavont hello szerepkör</span><span class="sxs-lookup"><span data-stu-id="7b11f-124">hello role that was granted or revoked</span></span> |
| <span data-ttu-id="7b11f-125">**Hatókör**</span><span class="sxs-lookup"><span data-stu-id="7b11f-125">**Scope**</span></span> | <span data-ttu-id="7b11f-126">hello egyedi azonosítója hello előfizetés, erőforrás vagy az erőforrások, amelyek a hozzárendelés hello túl vonatkozik</span><span class="sxs-lookup"><span data-stu-id="7b11f-126">hello unique identifier of hello subscription, resource group, or resource that hello assignment applies too</span></span>| 
| <span data-ttu-id="7b11f-127">**ScopeName**</span><span class="sxs-lookup"><span data-stu-id="7b11f-127">**ScopeName**</span></span> |<span data-ttu-id="7b11f-128">hello hello előfizetés, az erőforráscsoportot, vagy az erőforrás neve</span><span class="sxs-lookup"><span data-stu-id="7b11f-128">hello name of hello subscription, resource group, or resource</span></span> |
| <span data-ttu-id="7b11f-129">**ScopeType**</span><span class="sxs-lookup"><span data-stu-id="7b11f-129">**ScopeType**</span></span> |<span data-ttu-id="7b11f-130">Hello hozzárendelés hello előfizetés, erőforráscsoporthoz vagy erőforrás hatókör volt-e</span><span class="sxs-lookup"><span data-stu-id="7b11f-130">Whether hello assignment was at hello subscription, resource group, or resource scope</span></span> |
| <span data-ttu-id="7b11f-131">**Időbélyeg**</span><span class="sxs-lookup"><span data-stu-id="7b11f-131">**Timestamp**</span></span> |<span data-ttu-id="7b11f-132">hello dátum és idő, amelyet megváltozott</span><span class="sxs-lookup"><span data-stu-id="7b11f-132">hello date and time that access was changed</span></span> |

<span data-ttu-id="7b11f-133">Példa felsorolja az összes változásokat hello az előfizetéshez tartozó hello elmúlt hét napban:</span><span class="sxs-lookup"><span data-stu-id="7b11f-133">This example command lists all access changes in hello subscription for hello past seven days:</span></span>

```
Get-AzureRMAuthorizationChangeLog -StartTime ([DateTime]::Now - [TimeSpan]::FromDays(7)) | FT Caller,Action,RoleName,PrincipalType,PrincipalName,ScopeType,ScopeName
```

![PowerShell Get-AzureRMAuthorizationChangeLog – képernyőkép](./media/role-based-access-control-configure/access-change-history.png)

## <a name="create-a-report-with-azure-cli"></a><span data-ttu-id="7b11f-135">Az Azure CLI-jelentés létrehozása</span><span class="sxs-lookup"><span data-stu-id="7b11f-135">Create a report with Azure CLI</span></span>
<span data-ttu-id="7b11f-136">toocreate hello Azure parancssori felület (CLI), az access módosítás előzmények jelentés használata hello `azure role assignment changelog list` parancsot.</span><span class="sxs-lookup"><span data-stu-id="7b11f-136">toocreate an access change history report in hello Azure command-line interface (CLI), use hello `azure role assignment changelog list` command.</span></span>

## <a name="export-tooa-spreadsheet"></a><span data-ttu-id="7b11f-137">Tooa számolótábla exportálása</span><span class="sxs-lookup"><span data-stu-id="7b11f-137">Export tooa spreadsheet</span></span>
<span data-ttu-id="7b11f-138">toosave jelentés hello vagy kezelhetők hello adatok, az Exportálás hello hozzáférés módosítja egy .csv-fájlba.</span><span class="sxs-lookup"><span data-stu-id="7b11f-138">toosave hello report, or manipulate hello data, export hello access changes into a .csv file.</span></span> <span data-ttu-id="7b11f-139">Megtekintheti a hello jelentés felülvizsgálati összegyűjtheti egy táblázatban.</span><span class="sxs-lookup"><span data-stu-id="7b11f-139">You can then view hello report in a spreadsheet for review.</span></span>

![Változásnaplója tekinthetők meg a táblázat – képernyőkép](./media/role-based-access-control-configure/change-history-spreadsheet.png)

## <a name="next-steps"></a><span data-ttu-id="7b11f-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7b11f-141">Next steps</span></span>
* <span data-ttu-id="7b11f-142">Együttműködve [egyéni szerepkörök az Azure RBAC](role-based-access-control-custom-roles.md)</span><span class="sxs-lookup"><span data-stu-id="7b11f-142">Work with [Custom roles in Azure RBAC](role-based-access-control-custom-roles.md)</span></span>
* <span data-ttu-id="7b11f-143">Megtudhatja, hogyan toomanage [Azure RBAC a PowerShell használatával](role-based-access-control-manage-access-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="7b11f-143">Learn how toomanage [Azure RBAC with powershell](role-based-access-control-manage-access-powershell.md)</span></span>

