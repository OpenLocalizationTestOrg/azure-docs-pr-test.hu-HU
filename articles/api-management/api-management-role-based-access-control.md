---
title: "Szerepköralapú hozzáférés-vezérlés használata az Azure API Management |} Microsoft Docs"
description: "Útmutató: a beépített szerepkörök használatával, és hozzon létre egyéni szerepkörök az Azure API Management"
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 364cd53e-88fb-4301-a093-f132fa1f88f5
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: apimpm
ms.openlocfilehash: fa757a591d788f52d759bc24accedd3c55149ae7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-role-based-access-control-in-azure-api-management"></a>Szerepköralapú hozzáférés-vezérlés használata az Azure API Management
Az Azure API Management támaszkodik a átruházásához hozzáférés-vezérlés (RBAC) részletes hozzáféréskezelést az API Management-szolgáltatások és entitásokat (például API-k, házirendek) engedélyezéséhez. Ez a cikk áttekintést nyújt a beépített és egyéni szerepkörök az API Management. Ha a hozzáférés-kezelés az Azure portálon további adatokra van szüksége, tekintse meg [hozzáféréskezelést az Azure-portálon az első lépései](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/)

## <a name="built-in-roles"></a>Beépített szerepkörök
Az API Management jelenleg 3 beépített szerepkörök biztosít, és felveszi a közeljövőben további 2 szerepkörök. Ezeket a szerepköröket, például előfizetés, erőforráscsoport és egyéni API Management-példány különböző hatóköröket rendelhetők hozzá. Például ha az "Azure API Management szolgáltatás olvasó" szerepkörhöz van rendelve egy felhasználó, az erőforráscsoport szintjén, majd a felhasználó hozzáférhet olvasási belül az erőforráscsoport összes API Management példányára. 

A következő táblázat a beépített szerepkörök rövid leírása. Ezek a szerepkörök az Azure portál vagy más eszközeit, beleértve az Azure használatával rendelhet [PowerShell](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-manage-access-powershell), Azure [parancssori felület](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-manage-access-azure-cli), és [REST API](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-manage-access-rest). További beépített szerepkörök társításával kapcsolatos információkért lásd: [az Azure-előfizetés erőforrásokhoz való hozzáférés kezelése a szerepkör-hozzárendelések segítségével](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/).

| Szerepkör          | Olvasási hozzáférés<sup>[1]</sup> | Írási<sup>[2]</sup> | Szolgáltatás létrehozása, törlés, a méretezés, a VPN- és az egyéni tartomány beállítása | A hagyományos Publsiher portálhoz való hozzáférés | Leírás
| ------------- | ---- | ---- | ---- | ---- | ---- | ---- |
| Az Azure API Management szolgáltatás közreműködő | ✓ | ✓ | ✓ | ✓ | Felügyelői. Az API Management-szolgáltatások és entitásokat (pl. a API-k, a házirendek) teljes CRUD hozzáféréssel rendelkezik. Az örökölt publisher portal hozzáférése van. |
| Az Azure API Management szolgáltatás olvasó | ✓ | | || Az API Management-szolgáltatások és entitások olvasási hozzáférése van. |
| Az Azure API Management szolgáltatást üzemeltető | ✓ | | ✓ | | Kezelheti az API Management-szolgáltatások, de nem entitás.|
| Az Azure API Management Service-szerkesztő<sup>*</sup> | ✓ | ✓ | |  | Az API Management entitások, de nem szolgáltatások kezelheti.|
| Az Azure API Management tartalomkezelő<sup>*</sup> | ✓ | | | ✓ | Kezelheti a fejlesztői portálján. Csak olvasási hozzáféréssel szolgáltatásokhoz és entitások.|

<sup>[1] olvasási hozzáférés az API Management-szolgáltatások és entitások (például API-k, házirendek)</sup>

<sup>[2] írási hozzáféréssel az API Management-szolgáltatások és entitások, kivéve a következő opeartions: 1) példány létrehozása, törlés és 2) VPN-konfiguráció 3) egyéni tartomány nevét beállítást skálázás</sup>

<sup>\*A szolgáltatás szerkesztő szerepkör után azt át minden rendszergazda felhasználói felület a meglévő publisher portálon az Azure-portálon lesz elérhető. Tartalomkezelő szerepkör az csak magában foglalja a fejlesztői portálján felügyeletével kapcsolatos funkciók átkerült a közzétevő portal után lesz elérhető.</sup>  


## <a name="custom-roles"></a>Egyéni szerepkörök
Ha az adott igényeknek a beépített szerepkörök egyike, egyéni szerepkörök arra, hogy részletesebb access management az API Management entitásokat is létrehozható. Például létrehozhat egy egyéni biztonsági szerepkört API Management szolgáltatásnak olvasási hozzáféréssel rendelkezik, de csak az egy adott API írási hozzáféréssel rendelkezik. További információt az egyéni szerepkörök további tudnivalókért lásd: [egyéni szerepkörök az Azure RBAC](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-custom-roles). 

Amikor létrehoz egy egyéni biztonsági szerepkört, célszerűbb a beépített szerepkörök egyikével. A műveletek, NotActions vagy AssignableScopes attribútumok szerkesztése, majd mentse a módosításokat egy új szerepkör. Az alábbi példában az "Azure API Managmentet szolgáltatás olvasó" szerepkör kezdődik, és létrehoz egy egyéni biztonsági szerepkört "Számológép API szerkesztő" nevezik. Az egyéni biztonsági szerepkört is hozzárendelhető csak egy adott API ezért lesz csak az adott API hozzáféréssel rendelkezik. 

```
$role = Get-AzureRmRoleDefinition "API Management Service Reader Role"
$role.Id = $null
$role.Name = 'Calculator API Contributor'
$role.Description = 'Has read access to Contoso APIM instance and write access to the Calculator API.'
$role.Actions.Add('Microsoft.ApiManagement/service/apis/write')
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add('/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>')
New-AzureRmRoleDefinition -Role $role
New-AzureRmRoleAssignment -ObjectId <object ID of the user account> -RoleDefinitionName 'Calculator API Contributor' -Scope '/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>'
```

## <a name="watch-a-video-overview"></a>Áttekintő videó megtekintése

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Role-Based-Access-Control-in-API-Management/player]
> 
> 

## <a name="next-steps"></a>Következő lépések

* További információ a szerepköralapú hozzáférés-vezérlés az Azure-ban
  * [Bevezetés a hozzáférés-kezelés Azure Portalon történő használatába](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/)
  * [Az Azure-előfizetések erőforrásaihoz való hozzáférés kezelése szerepkör-hozzárendelésekkel](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/)
  * [Egyéni szerepkörök az Azure RBAC](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-custom-roles)
