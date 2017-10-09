---
title: "aaaUsing szerepköralapú hozzáférés-vezérlés toomanage Azure Site Recovery |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooapply és -felhasználási szerepköralapú hozzáférés-vezérlés (RBAC) toomanage az Azure Site Recovery-központitelepítések"
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: manayar
ms.openlocfilehash: 7b721090351e561b28317ccdcf0ff283e0b146ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-role-based-access-control-toomanage-azure-site-recovery-deployments"></a>Szerepköralapú hozzáférés-vezérlés toomanage Azure Site Recovery központi telepítéssel

Az Azure Szerepköralapú hozzáférés-vezérlés (RBAC) részletes hozzáférés-vezérlést biztosít az Azure-hoz. RBAC használata, feladatkörök elkülönítse a munkacsoporton belül, és csak adott hozzáférési engedélyek toousers szükséges tooperform adott feladat engedélyezése.

Az Azure Site Recovery biztosít 3 beépített szerepkörök toocontrol Site Recovery felügyeleti műveleteket. A további [beépített Azure RBAC-szerepkörök](../active-directory/role-based-access-built-in-roles.md)

* [Hely helyreállítási közreműködői](../active-directory/role-based-access-built-in-roles.md#site-recovery-contributor) – Ez a szerepkör rendelkezik minden engedélyek szükséges toomanage Azure Site Recovery-műveleteket a Recovery Services-tároló. Ezzel a szerepkörrel rendelkező felhasználók azonban nem létrehozása vagy Recovery Services-tároló törlése vagy a hozzáférési jogok tooother felhasználók hozzárendelése. Ezt a szerepkört olyan vész helyreállítási-rendszergazdák számára engedélyezheti és vészhelyreállítás az alkalmazások vagy a teljes szervezet számára hello esetben.
* [Helyreállítási operátor hely](../active-directory/role-based-access-built-in-roles.md#site-recovery-operator) – Ez a szerepkör engedélyek tooexecute és manager feladatátvétel és a feladat-visszavétel műveleteket tartalmazza. A felhasználói szerephez nem tudja engedélyezni vagy tiltsa le a replikációt, létrehozása vagy törlése a tárolóból, új infrastruktúra regisztrálása vagy hozzáférési jogok tooother felhasználók hozzárendelése. Ezt a szerepkört egy feladatátvételi virtuális gépek is vész helyreállítási kezelők a legalkalmasabb vagy alkalmazások, ha arra utasította az alkalmazástulajdonosok és a rendszergazdák egy tényleges vagy szimulált katasztrófa helyzetben, például egy vész-Helyreállítási részletezést. POST hello katasztrófa felbontása, hello vész-Helyreállítási operátor ismételt védelemmel láthatná, és a feladat-visszavételt a virtuális gépek hello.
* [Hely helyreállítási olvasó](../active-directory/role-based-access-built-in-roles.md#site-recovery-reader) -ezt a szerepkört engedélyek tooview összes Site Recovery felügyeleti műveleteket tartalmazza. Ezt a szerepkört olyan egy informatikai felügyeleti vezető, akik hello védelmi aktuális állapotának figyelése és támogatási jegyek ablakába, ha szükséges.

Ha saját szerepköröket még jobban kézben toodefine van szüksége, lásd: hogyan túl[egyéni szerepkörök létrehozása](../active-directory/role-based-access-control-custom-roles.md) az Azure-ban.

## <a name="permissions-required-tooenable-replication-for-new-virtual-machines"></a>Engedélyek szükséges tooEnable új virtuális gépek replikálása
Amikor új virtuális gép Azure Site Recovery segítségével replikált tooAzure, hello kapcsolódó felhasználói hozzáférési szintek-e érvényesített tooensure, amely a felhasználó hello hello szükséges engedélyek toouse hello Azure-erőforrások megadott tooSite helyreállítási.

egy új virtuális gép replikációs tooenable, a felhasználóknak rendelkezniük kell:
* Engedély toocreate hello kijelölt erőforráscsoporthoz tartozik, a virtuális gép
* Engedély toocreate hello a kiválasztott virtuális hálózat a virtuális gép
* Engedély toowrite toohello kijelölve tárfiók

A felhasználó számára a következő engedélyek toocomplete replikációs egy új virtuális gép hello szükséges.

> [!IMPORTANT]
>Győződjön meg arról, hogy megfelelő engedélyeket / hello telepítési modell kerülnek (erőforrás-kezelő / klasszikus) erőforrás-telepítéshez használt.

| **Erőforrás típusa** | **Telepítési modell** | **Engedély** |
| --- | --- | --- |
| Számítás | Resource Manager | Microsoft.Compute/availabilitySets/read |
|  |  | Microsoft.Compute/virtualMachines/read |
|  |  | Microsoft.Compute/virtualMachines/write |
|  |  | Microsoft.Compute/virtualMachines/delete |
|  | Klasszikus | Microsoft.ClassicCompute/domainNames/read |
|  |  | Microsoft.ClassicCompute/domainNames/write |
|  |  | Microsoft.ClassicCompute/domainNames/delete |
|  |  | Microsoft.ClassicCompute/virtualMachines/read |
|  |  | Microsoft.ClassicCompute/virtualMachines/write |
|  |  | Microsoft.ClassicCompute/virtualMachines/delete |
| Network (Hálózat) | Resource Manager | Microsoft.Network/networkInterfaces/read |
|  |  | Microsoft.Network/networkInterfaces/write |
|  |  | Microsoft.Network/networkInterfaces/delete |
|  |  | Microsoft.Network/networkInterfaces/join/action |
|  |  | Microsoft.Network/virtualNetworks/read |
|  |  | Microsoft.Network/virtualNetworks/subnets/read |
|  |  | Microsoft.Network/virtualNetworks/subnets/join/action |
|  | Klasszikus | Microsoft.ClassicNetwork/virtualNetworks/read |
|  |  | Microsoft.ClassicNetwork/virtualNetworks/join/action |
| Storage | Resource Manager | Microsoft.Storage/storageAccounts/read |
|  |  | Microsoft.Storage/storageAccounts/listkeys/action |
|  | Klasszikus | Microsoft.ClassicStorage/storageAccounts/read |
|  |  | Microsoft.ClassicStorage/storageAccounts/listKeys/action |
| Erőforráscsoport | Resource Manager | Microsoft.Resources/deployments/* |
|  |  | Microsoft.Resources/subscriptions/resourceGroups/read |

Érdemes lehet hello "Virtuális gép közreműködő" és "Klasszikus virtuális gép közreműködő" [beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md) a Resource Manager és klasszikus telepítési modellek kulcsattribútumokkal.

## <a name="next-steps"></a>Következő lépések
* [Szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md): első lépések az RBAC a hello Azure-portálon.
* Megtudhatja, hogyan toomanage elérni:
  * [PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md)
  * [Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md)
  * [REST API](../active-directory/role-based-access-control-manage-access-rest.md)
* [Szerepköralapú hozzáférés-vezérlés hibaelhárítási](../active-directory/role-based-access-control-troubleshooting.md): kapcsolatos gyakori hibák elhárítására vonatkozó javaslatok beolvasása.
