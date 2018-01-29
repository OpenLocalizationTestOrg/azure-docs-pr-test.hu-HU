---
title: "Az MSI-hozzáférés hozzárendelése egy Azure-erőforrás, az Azure portál használatával"
description: "Részletes útmutatást ad egy olyan MSI Csomaghoz, egy erőforrás-hozzáférés hozzárendelése egy másik erőforrás, az Azure portál használatával."
services: active-directory
documentationcenter: 
author: BryanLa
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 7d0a50db28ba3d9926f7a83fe224b7a0dbe6ed20
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/22/2017
---
# <a name="assign-a-managed-service-identity-access-to-a-resource-by-using-the-azure-portal"></a>Egy felügyelt Szolgáltatásidentitás hozzáférés hozzárendelése egy erőforrást az Azure portál használatával

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Miután konfigurálta az Azure-erőforrás a egy felügyelt szolgáltatás Identity (MSI), a MSI hozzáférést biztosíthat más erőforráshoz, csakúgy, mint bármely rendszerbiztonsági tag. Ez a cikk bemutatja, hogyan való hozzáférésre egy Azure virtuális gép MSI egy Azure storage-fiókot az Azure portál használatával.

## <a name="prerequisites"></a>Előfeltételek

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Az MSI-hozzáférés hozzárendelése egy másik erőforrás RBAC használata

Egy Azure-erőforrás a MSI bekapcsolását követően [például egy Azure virtuális gép](msi-qs-configure-portal-windows-vm.md):

1. Jelentkezzen be a [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely alatt a MSI konfigurált Azure-előfizetéshez társítva.

2. Nyissa meg a kívánt erőforrás hozzáférés-vezérlés módosítani kívánja. Ebben a példában azt biztosít Azure virtuális gép hozzáférjen tárfiókba, így azt lépjen a tárfiókhoz.

3. Válassza ki a **hozzáférés-vezérlés (IAM)** az erőforrást, majd válassza a lap **+ Hozzáadás**. Adja meg a **szerepkör**, **hozzáférés hozzárendelése a virtuális gép**, és adja meg a megfelelő **előfizetés** és **erőforráscsoport** ahol az erőforrás található. A keresési feltételek területen láthatja az erőforráshoz. Válassza ki az erőforrás, majd **mentése**. 

   ![Hozzáférés-vezérlési (IAM) képernyőkép](~/articles/active-directory/media/msi-howto-assign-access-portal/assign-access-control-iam-blade-before.png)  

4. A fő ismét **hozzáférés-vezérlés (IAM)** lap, ahol jelennek meg új bejegyzést az erőforrás MSI. Ebben a példában a "SimpleWinVM" virtuális gép a bemutató erőforráscsoportból rendelkezik **közreműködő** a tárfiók eléréséhez.

   ![Hozzáférés-vezérlési (IAM) képernyőkép](~/articles/active-directory/media/msi-howto-assign-access-portal/assign-access-control-iam-blade-after.png)

## <a name="troubleshooting"></a>Hibaelhárítás

Ha az erőforrás MSI nem jelenik meg a rendelkezésre álló azonosítók listája, győződjön meg arról, hogy az MSI-fájl helytelenül engedélyezett. Ebben az esetben azt is lépjen vissza az Azure virtuális Gépen, és ellenőrizze a következőket:

- Tekintse meg a **konfigurációs** lapon, és győződjön meg arról, hogy az érték **engedélyezett MSI** van **Igen**.
- Tekintse meg a **bővítmények** lapon, és győződjön meg arról, hogy az MSI-bővítmény sikeresen telepítve.

Vagy nem megfelelő, ha szüksége lehet, hogy telepítse újra az erőforráson MSI újra, vagy a központi telepítési hiba elhárítása.

## <a name="related-content"></a>Kapcsolódó tartalom

- MSI áttekintését lásd: [Szolgáltatásidentitás felügyelete – áttekintés](msi-overview.md).
- Egy Azure virtuális gépen az MSI engedélyezéséről [konfigurálása az Azure virtuális gép felügyelt szolgáltatás identitásának (MSI) az Azure portál használatával](msi-qs-configure-portal-windows-vm.md).


