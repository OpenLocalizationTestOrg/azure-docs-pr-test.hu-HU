---
title: az Azure Security Centerben aaaPermissions |} Microsoft Docs
description: "Ez a cikk azt ismerteti, hogyan használja az Azure Security Center a szerepköralapú hozzáférés-vezérlési tooassign engedélyek toousers, és azonosítja az egyes szerepkörökhöz műveletek engedélyezett hello."
services: security-center
cloud: na
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
ms.assetid: 
ms.service: security-center
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: terrylan
ms.openlocfilehash: 03e16132dc3d951ef8ad9e86b9970b9e4d15c76b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="permissions-in-azure-security-center"></a>Engedélyek az Azure Security Centerben

Az Azure Security Center által használt [szerepköralapú hozzáférés-vezérlést (RBAC)](../active-directory/role-based-access-control-configure.md), amely biztosítja [beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md) , amely a toousers, csoportok és az Azure rendelhetők.

A Security Center értékeli az erőforrások tooidentify biztonsági problémák és biztonsági rések hello konfigurációja. A biztonsági központban, csak akkor jelenik meg információ tooa erőforrás kapcsolatos hello szerepkör tulajdonos, közreműködő vagy olvasó hello előfizetés vagy az erőforrás csoport, amely egy erőforrás tartozik.

Továbbá toothese szerepkörök szerepkör is létezik két adott Security Center:

* **Biztonsági olvasó**: toothis szerepkörhöz tartozó felhasználó rendelkezik jogosultságokkal tooSecurity Center megtekintése. hello felhasználó megtekintheti a javaslatok, a riasztások, a biztonsági házirend és a biztonsági állapotok, de nem módosíthatja.
* **Biztonsági rendszergazda**: egy felhasználó toothis szerepkörhöz tartozó azonos hello biztonsági olvasó, tartalomvédelmi hello rendelkezik és hello biztonsági házirend frissítése, és zárja be a riasztások és javaslatok.

> [!NOTE]
> hello biztonsági szerepkörök, biztonsági olvasó és a biztonsági rendszergazda, csak a Security Center rendelkezik hozzáféréssel. hello biztonsági szerepkörök nem rendelkezik hozzáférési tooother szolgáltatás például a tárolóhely & webes, mobil vagy az eszközök internetes hálózatát Azure területéhez.
>
>

## <a name="roles-and-allowed-actions"></a>Szerepkörök és az engedélyezett műveletek

hello következő táblázat megjeleníti a szerepkört, és a Security Center műveletek engedélyezett. Az X jelzi, hogy ez a szerepkör hello művelet engedélyezett.

| Szerepkör | Biztonsági házirend szerkesztése | Egy erőforrás biztonsági javaslatok alkalmazása | Hagyja figyelmen kívül a riasztások és javaslatok | Riasztások megtekintése és javaslatok |
|:--- |:---:|:---:|:---:|:---:|
| Előfizetés tulajdonosa | X | X | X | X |
| Előfizetés közreműködő | X | X | X | X |
| Erőforráscsoport tulajdonosa | -- | X | -- | X |
| Erőforráscsoport közreműködő | -- | X | -- | X |
| Olvasó | -- | -- | -- | X |
| Biztonsági rendszergazda | X | -- | X | X |
| Biztonsági olvasó | -- | -- | -- | X |

> [!NOTE]
> Azt javasoljuk, hogy rendelje hello legkevésbé megengedő szerepkörre felhasználók toocomplete a feladataik ellátásához szükséges. Például rendeljen hello olvasó szerepkört toousers csak a tooview erőforrás biztonsági állapota hello információra van szüksége, de nem tesznek lépéseket, például a alkalmazhatnak javaslatokat, és módosíthatják a szabályzatokat.
>
>

## <a name="next-steps"></a>Következő lépések
Ez a cikk azt, hogyan használja a Security Center az RBAC tooassign engedélyek toousers, és engedélyezett műveletek az egyes szerepkörökhöz hello azonosított. Most, hogy ismeri hello szerepkör-hozzárendelések toomonitor hello szükséges biztonsági állapotát, az előfizetés, szerkesztheti a biztonsági szabályzatokat, és a javaslatok alkalmazni, megtudhatja, hogyan:

- [A Security Center biztonsági házirendek beállítása](security-center-policies.md)
- [A Security Center biztonsági javaslatok kezelése](security-center-recommendations.md)
- [Az Azure-erőforrások biztonsági állapotát hello figyelése](security-center-monitoring.md)
- [Kezelésének és megoldásának toosecurity riasztásokat a Security Center](security-center-managing-and-responding-alerts.md)
- [Biztonsági partnermegoldások figyelése](security-center-partner-solutions.md)
