---
title: "Azure szerepköralapú hozzáférés-vezérlés biztonsági mentéseit |} Microsoft Docs"
description: "Szerepköralapú hozzáférés-vezérlés toomanage toobackup felügyeleti műveletek használja a Recovery Services-tároló."
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 3bd46b97-4b29-47a5-b5ac-ac174dd36760
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/22/2017
ms.author: trinadhk;markgal
ms.openlocfilehash: 26d034d152f9b77fc6d5b2ffd5ef2648b1797f46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-role-based-access-control-toomanage-azure-backup-recovery-points"></a>Szerepköralapú hozzáférés-vezérlés toomanage Azure biztonsági mentési helyreállítási pontok használata
Az Azure Szerepköralapú hozzáférés-vezérlés (RBAC) részletes hozzáférés-vezérlést biztosít az Azure-hoz. Szerepalapú használ, feladatokat elkülönítse a munkacsoporton belül, és hozzáférési toousers csak hello összegét adja meg, hogy be kell tooperform a munkájukat.

> [!IMPORTANT]
> Azure Backup szolgáltatás által biztosított szerepkörök korlátozott tooactions végrehajtható az Azure portálon vagy Recovery Services-tároló PowerShell-parancsmagokkal. Az Azure biztonsági mentési ügynök ügyfél felhasználói felületének vagy a System center Data Protection Manager Kezelőfelületének vagy az Azure Backup Server felhasználói felületén ezek a szerepkörök irányítását kívül esnek a végrehajtott műveleteket.

Azure biztonsági mentés biztosít 3 beépített szerepkörök toocontrol biztonságimásolat-felügyeleti műveletek. A további [beépített Azure RBAC-szerepkörök](../active-directory/role-based-access-built-in-roles.md)

* [Biztonsági mentés közreműködői](../active-directory/role-based-access-built-in-roles.md#backup-contributor) -ezt a szerepkört minden engedélyek toocreate rendelkezik, és kezelheti a biztonsági mentéshez, kivéve a Recovery Services-tároló létrehozása és hozzáférési tooothers próbálkozik. Képzelje el a szerepkör a biztonságimásolat-felügyeleti ki minden biztonságimásolat-felügyeleti műveletet végezhet rendszergazdaként.
* [Biztonsági mentés operátor](../active-directory/role-based-access-built-in-roles.md#backup-operator) -ezt a szerepkört egy munkatárs kivételével a biztonsági mentési és kezelését egy biztonsági mentési házirendek eltávolítása engedélyek tooeverything rendelkezik. A szerepkör egyenértékű toocontributor, azonban nem ilyen beállítások mellett pusztító műveleteket, például az adatok biztonsági másolatának állítsa le és eltávolítja a helyszíni erőforrások regisztrációját.
* [Biztonsági mentés olvasó](../active-directory/role-based-access-built-in-roles.md#backup-reader) – Ez a szerepkör rendelkezik engedélyekkel tooview összes biztonságimásolat-felügyeleti műveletek. Képzelje el a szerepkör toobe figyelési személy.

Ha saját szerepköröket még jobban kézben toodefine van szüksége, lásd: hogyan túl[egyéni szerepkörök létrehozása](../active-directory/role-based-access-control-custom-roles.md) az Azure RBAC.



## <a name="mapping-backup-built-in-roles-toobackup-management-actions"></a>Biztonsági mentés beépített szerepkörök toobackup felügyeleti műveletek leképezése
a következő táblázat hello hello biztonsági mentés felügyeleti műveletek rögzíti, és megfelelő minimális RBAC szerepkör szükséges tooperform művelet.

| Ügynökfelügyeleti művelet | Szükséges minimális RBAC szerepkör |
| --- | --- |
| Recovery Services-tároló létrehozása | A tároló erőforráscsoport közreműködő |
| Azure virtuális gépek biztonsági mentésének engedélyezése | A tároló, virtuális gép közreműködő, virtuális gépeken biztonságimásolat-felelős |
| A tárolt virtuális gépek biztonsági mentése | Biztonságimásolat-felelős |
| Virtuális gép visszaállítása | Biztonságimásolat-felelősként, amelyben virtuális gép és a Vnetek fog tooget telepített erőforrás csoport közreműködő |
| Állítsa vissza a lemezek, a fájlokat a virtuális gép biztonsági mentése | Biztonságimásolat-felelős |
| Az Azure virtuális gép biztonsági mentése a biztonsági mentési házirend létrehozása | Biztonsági mentési közreműködő |
| Az Azure virtuális gép biztonsági mentése a biztonsági mentési házirend módosítása | Biztonsági mentési közreműködő |
| Az Azure virtuális gép biztonsági mentése a biztonsági mentési házirend törlése | Biztonsági mentési közreműködő |
| STOP biztonsági mentés (az adatok megőrzésével, illetve adatokat törölhet) a virtuális gép biztonsági mentése | Biztonsági mentési közreműködő |
| A helyi Windows Server/ügyfél/SCDPM vagy az Azure Backup-kiszolgáló regisztrálása | Biztonságimásolat-felelős |
| Regisztrált helyszíni Windows Server/ügyfél/SCDPM vagy az Azure Backup Server törlése | Biztonsági mentési közreműködő |

## <a name="next-steps"></a>Következő lépések
* [Szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md): első lépések az RBAC a hello Azure-portálon.
* Megtudhatja, hogyan toomanage elérni:
  * [PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md)
  * [Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md)
  * [REST API](../active-directory/role-based-access-control-manage-access-rest.md)
* [Szerepköralapú hozzáférés-vezérlés hibaelhárítási](../active-directory/role-based-access-control-troubleshooting.md): kapcsolatos gyakori hibák elhárítására vonatkozó javaslatok beolvasása.
