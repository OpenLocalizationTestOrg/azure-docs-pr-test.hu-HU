---
title: "Szerepköralapú hozzáférés-vezérlés az Azure API Management aaaHow tooUse |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse hello beépített szerepkörök, és hozzon létre egyéni szerepkörök az Azure API Management"
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
ms.openlocfilehash: c51da2ff6886ebcaf796022e3a759c67f36670a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-role-based-access-control-in-azure-api-management"></a>Hogyan tooUse szerepköralapú hozzáférés-vezérlés az Azure API Management
Az Azure API Management átruházásához hozzáférés-vezérlés (RBAC) tooenable részletes hozzáféréskezelést az API Management-szolgáltatások és entitásokat (például API-k, házirendek) alapul. Ez a cikk áttekintést nyújt a hello beépített és egyéni szerepkörök az API Management. Ha a hozzáférés-kezelés az Azure-portálon hello további adatokra van szüksége, tekintse meg [kezelési hello Azure-portálon az első lépések](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/)

## <a name="built-in-roles"></a>Beépített szerepkörök
Az API Management jelenleg 3 beépített szerepkörök biztosít, és 2 további szerepkörök közeli jövő hello adja hozzá. Ezeket a szerepköröket, például előfizetés, erőforráscsoport és egyéni API Management-példány különböző hatóköröket rendelhetők hozzá. Például ha hello "Azure API Management szolgáltatás olvasó" szerepkör tooan felhasználójának hello erőforráscsoport szintjén van hozzárendelve, majd hello felhasználói kell olvasási hozzáférés tooall API Management példányok hello erőforrás csoporton belül. 

hello következő tábla rövid leírást ad a hello beépített szerepkörök. Ezek a szerepkörök hello Azure portál vagy más eszközeit, beleértve az Azure használatával rendelhet [PowerShell](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-manage-access-powershell), Azure [parancssori felület](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-manage-access-azure-cli), és [REST API](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-manage-access-rest). Részletes információt tooassign beépített szerepkörök; Lásd: [szerepkör hozzárendelések toomanage tooyour Azure-előfizetés erőforrások eléréséhez használjon](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/).

| Szerepkör          | Olvasási hozzáférés<sup>[1]</sup> | Írási<sup>[2]</sup> | Szolgáltatás létrehozása, törlés, a méretezés, a VPN- és az egyéni tartomány beállítása | Hozzáférés toolegacy Publsiher portál | Leírás
| ------------- | ---- | ---- | ---- | ---- | ---- | ---- |
| Az Azure API Management szolgáltatás közreműködő | ✓ | ✓ | ✓ | ✓ | Felügyelői. Teljes hozzáférés tooAPI szolgáltatások CRUD és entitások (pl. a API-k, a házirendek) rendelkezik. Hozzáférés toohello örökölt publisher portál rendelkezik. |
| Az Azure API Management szolgáltatás olvasó | ✓ | | || Csak olvasási hozzáféréssel tooAPI szolgáltatások és entitások rendelkezik. |
| Az Azure API Management szolgáltatást üzemeltető | ✓ | | ✓ | | Kezelheti az API Management-szolgáltatások, de nem entitás.|
| Az Azure API Management Service-szerkesztő<sup>*</sup> | ✓ | ✓ | |  | Az API Management entitások, de nem szolgáltatások kezelheti.|
| Az Azure API Management tartalomkezelő<sup>*</sup> | ✓ | | | ✓ | Kezelheti a fejlesztői portálján. Csak olvasási hozzáféréssel tooservices és entitások.|

<sup>[1] olvasási hozzáférés tooAPI szolgáltatások és entitásokat (például API-k, házirendek)</sup>

<sup>[2] írási tooAPI szolgáltatások és entitások, kivéve a következő opeartions: 1) példány létrehozása, törlés és 2) VPN-konfiguráció 3) egyéni tartomány nevét beállítást skálázás</sup>

<sup>\*hello szolgáltatás szerkesztő szerepkör az összes rendszergazda felhasználói felület azt át hello meglévő publisher portál toohello Azure-portálon után lesz elérhető. hello tartalomkezelő szerepkör lesz elérhető után hello publisher portal átkerült tooonly tartalmaz-e funkciók kapcsolódó toomanaging hello fejlesztői portálján.</sup>  


## <a name="custom-roles"></a>Egyéni szerepkörök
Ha az adott igényeknek hello beépített szerepkörök egyike, egyéni szerepkörök hozhatók létre tooprovide részletesebb access management az API Management entitásokat. Például létrehozhat egy egyéni biztonsági szerepkört, amely csak olvasási hozzáféréssel tooan API Management szolgáltatást, de csak van az írási hozzáférést tooone adott API. toolearn egyéni szerepkörök kapcsolatos további részletekért lásd: [egyéni szerepkörök az Azure RBAC](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-custom-roles). 

Amikor létrehoz egy egyéni biztonsági szerepkört, könnyebben toostart hello beépített szerepkörök egyikét is. Hello attribútumok tooadd hello műveletek, NotActions vagy AssignableScopes szerkesztése, majd új szerepkörként hello módosítások mentéséhez. hello alábbi példa kezdődik hello "Azure API Managmentet szolgáltatás olvasó" szerepkört, és létrehoz egy egyéni biztonsági szerepkört "Számológép API szerkesztő" néven. hello egyéni biztonsági szerepkört csak az adott API ezért fog csak tooa rendelkezik hozzáférési toothat API rendelhetők. 

```
$role = Get-AzureRmRoleDefinition "API Management Service Reader Role"
$role.Id = $null
$role.Name = 'Calculator API Contributor'
$role.Description = 'Has read access tooContoso APIM instance and write access toohello Calculator API.'
$role.Actions.Add('Microsoft.ApiManagement/service/apis/write')
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add('/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>')
New-AzureRmRoleDefinition -Role $role
New-AzureRmRoleAssignment -ObjectId <object ID of hello user account> -RoleDefinitionName 'Calculator API Contributor' -Scope '/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>'
```

## <a name="watch-a-video-overview"></a>Áttekintő videó megtekintése

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Role-Based-Access-Control-in-API-Management/player]
> 
> 

## <a name="next-steps"></a>Következő lépések

* További információ a szerepköralapú hozzáférés-vezérlés az Azure-ban
  * [Hozzáférés-kezelés hello Azure-portálon az első lépések](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/)
  * [Szerepkör hozzárendelések toomanage hozzáférés tooyour Azure-előfizetés erőforrásainak használatához](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/)
  * [Egyéni szerepkörök az Azure RBAC](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-custom-roles)
