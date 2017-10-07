---
title: "az Azure Active Directoryban egy törölt Office 365 aaaRestore csoport |} Microsoft Docs"
description: "Hogyan toorestore a törölt csoportban, visszaállítható csoportok megtekintése és permamnently csoport törlése az Azure Active Directoryban"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: cc5f232a-1e77-45c2-b28b-1fcb4621725c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: curtand
ms.openlocfilehash: 4e6650d64aa19f0c909f1c511f48681de8032f63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="restore-a-deleted-office-365-group-in-azure-active-directory"></a>Állítsa vissza egy törölt Office 365-csoport az Azure Active Directoryban

Ha töröl egy Office 365 csoportot hello Azure Active Directory (Azure AD), törölt hello csoport származik megtartott, de nem látható 30 napig hello törlés dátuma. Ez az, hogy hello csoportot és annak tartalmát is állítható vissza, ha szükséges. Ez a funkció nem használható kizárólag tooOffice 365 csoportok az Azure ad-ben. Nincs elérhető biztonsági és terjesztési csoportok.

> [!NOTE] 
> Ne használjon `Remove-MsolGroup` , mert azt az hello csoport véglegesen kiürítése. Mindig `Remove-AzureADMSGroup` toodelete az Office 365-csoport. 

a szükséges hello jogosultságok toorestore csoport hello következő lehet:

Szerepkör  | Engedélyek 
--------- | ---------
Vállalati rendszergazda, a Partner Tier2 támogatása és az InTune szolgáltatás-rendszergazdák | Visszaállíthatja a törölt Office 365-csoport 
Felhasználói fiók rendszergazdájához, és a Partner Tier1 támogatása | E hozzárendelt toohello vállalati rendszergazda szerepkört kivéve bármely törölt Office 365-csoport állíthatja vissza. 
Felhasználó | Bármely törölt Office 365-csoport, amelynek azok tulajdonosa állíthatja vissza. 


## <a name="view-hello-deleted-office-365-groups-that-are-available-toorestore"></a>Nézet hello Office 365-csoportokat, amelyek a rendelkezésre álló toorestore törlése
a következő parancsmagok hello lehet használt tooview törölt hello csoportok tooverify egy hello, vagy azokat, továbbra is érdekli nem még véglegesen eltávolításuk. Ezek a parancsmagok hello részét képező [Azure AD PowerShell modult](https://www.powershellgallery.com/packages/AzureAD/). Ez a modul további információ található a hello [Azure Active Directory PowerShell 2-es verzió](/powershell/azure/install-adv2?view=azureadps-2.0) cikk.

1.  Futtassa a következő parancsmag törli az összes toodisplay Office 365-csoportok az Ön bérlőjében, amelyek továbbra is elérhető toorestore hello.
  ```
  Get-AzureADMSDeletedGroup
  ```

2.  A másik lehetőség Ha tudja, egy adott csoport hello objectID (és lekérheti az 1. lépésben hello parancsmag), futtassa a következő parancsmag tooverify, amelyek adott törölt csoportban hello hello nem még véglegesen törölve lettek.
  ```
  Get-AzureADMSDeletedGroup –Id <objectId>
  ```



## <a name="how-toorestore-your-deleted-office-365-group"></a>Hogyan toorestore a törölt Office 365-csoport
Miután ellenőrizte, hogy hello csoport továbbra is elérhető toorestore, állítsa vissza a törölt hello csoport hello lépések egyike. Ha hello csoport tartalmazza a dokumentumok, SP helyek vagy más állandó objektumok, azt is eltarthat too24 óra toofully visszaállítási, egy csoportot és annak tartalmát.

1.  Futtatási hello következő parancsmag toorestore hello csoportot és annak tartalmát.
  
  ```
  Restore-AzureADMSDeletedDirectoryObject –Id <objectId>
  ``` 

Azt is megteheti hello következő parancsmagot futtathatja törölt hello csoport toopermanently eltávolítása.
  ```
  Remove-AzureADMSDeletedDirectoryObject –Id <objectId>
  ```

## <a name="how-do-you-know-this-worked"></a>Honnan tudhatja ez dolgozott?
hogy az Office 365 csoport hello futtatása sikeresen visszaállította már tooverify `Get-AzureADGroup –ObjectId <objectId>` hello csoport parancsmag toodisplay információit. Hello visszaállítás után kérelem teljesítése:
- hello csoport megjelenik hello bal oldali navigációs sávban a Exchange
- Planner jelennek meg hello csoport hello tervezése
- Minden olyan Sharepoint webhelyet, és minden, a tartalom elérhető lesz
- hello csoport is elérhetők, bármelyik hello Exchange végpontok és más Office 365 számítási feladattal, amely támogatja az Office 365-csoportokat


## <a name="next-steps"></a>Következő lépések
Ezek a cikkek további információkkal az Azure Active Directory csoportokat.

* [Tekintse meg a meglévő csoportok](active-directory-groups-view-azure-portal.md)
* [Egy csoport beállításainak kezelése](active-directory-groups-settings-azure-portal.md)
* [A csoport tagjai kezelése](active-directory-groups-members-azure-portal.md)
* [A csoport tagságát kezelése](active-directory-groups-membership-azure-portal.md)
* [A csoport dinamikus szabályok kezelése](active-directory-groups-dynamic-membership-azure-portal.md)
