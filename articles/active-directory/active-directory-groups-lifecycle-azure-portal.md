---
title: "Office 365 aaaPreview csoportosítja az Azure Active Directoryban lejárati |} Microsoft Docs"
description: "Hogyan tooset be az Office 365 lejárati csoportosítja az Azure Active Directoryban (előzetes verzió)"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.openlocfilehash: a8c99961cff3aad3f4d8b0cc1b2eb8e8a4c9ba95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-office-365-groups-expiration-preview"></a>Office 365 csoportok lejárati (előzetes verzió) konfigurálása

Office 365-csoportok hello életciklusát úgy, hogy kiválasztott Office 365 csoportok lejárati most kezelhetik. A lejárati be van állítva, ha ezeket a csoportokat tulajdonosainak vannak-e hogy az alkalmazás megkéri toorenew csoportjaikra hello csoportok továbbra is szükséges. Minden Office 365-csoportot, amely nem hosszabbítja meg törlődni fog. Minden Office 365 csoport törölt hello csoport tulajdonosainak vagy hello rendszergazda számított 30 napon belül állítható vissza.  


> [!NOTE]
> Beállíthatja, hogy csak az Office 365-csoportok lejárati idejét.
>
> Az Office 365-csoportok lejárati beállításához, hogy be van-e rendelve egy Azure AD Premium-licenc
>   - hello rendszergazda, aki hello bérlő hello lejárati beállítások konfigurálása
>   - Ez a beállítás a kijelölt hello csoportok összes tagja

## <a name="set-office-365-groups-expiration"></a>Állítsa be az Office 365 csoportok lejárata

1. Nyissa meg hello [az Azure AD felügyeleti központban](https://aad.portal.azure.com) egy olyan fiókkal, amely az Azure AD-bérlő globális rendszergazdája.

2. Nyissa meg az Azure AD, válassza ki **felhasználók és csoportok**.

3. Válassza ki **csoportbeállítások** majd **lejárati** tooopen hello lejárati beállítások.
  
  ![Lejárati panel](./media/active-directory-groups-lifecycle-azure-portal/expiration-settings.png)

4. A hello **lejárati** panelen is:

  * Hello csoport élettartamának beállítása napban. Kiválaszthatja az egyik hello beállított értékeket, vagy egy egyéni érték (kell 31 nap vagy több). 
  * Adjon meg egy e-mail címet, ahol hello megújítási és lejárati kell értesítéseket küldeni, ha egy csoport már nincs tulajdonosa. 
  * Válassza ki, melyik Office 365-csoportok lejár. Engedélyezheti a lejárati **összes** Office 365-csoportokat, választhatók ki hello Office 365-csoportok, vagy választja **nincs** letiltása az összes csoport lejárati.
  * Mentse a beállításokat, amikor elkészült, kiválasztásával **mentése**.

Hogyan toodownload és a telepítés hello Microsoft PowerShell modul tooconfigure lejárati PowerShell Office 365-csoportokat, lásd: [Azure Active Directory V2 PowerShell-modul - nyilvános előzetes 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).

Például az e-mail értesítések küldését toohello Office 365 csoport tulajdonosainak 30 nap, 15 nap, és 1 nap előzetes tooexpiration hello csoport.

![Lejárati értesítést](./media/active-directory-groups-lifecycle-azure-portal/expiration-notification.png)

hello csoporttulajdonos ezután kijelölhet **megújítási csoport** toorenew Office 365 csoport hello, vagy választhatja **toogroup Ugrás** toosee hello tagok és egyéb részleteit hello csoport.

Amikor lejár egy csoportot, hello csoport egy nap hello lejárati dátum után törlődik. Erre például e-mailben értesítést küldött toohello Office 365 csoport tulajdonosainak értesítheti őket hello lejárati és az Office 365 csoport későbbi törlését.

![Csoport törlése e-mail-értesítések](./media/active-directory-groups-lifecycle-azure-portal/deletion-notification.png)

hello csoport kiválasztásával visszaállítható **visszaállítása** vagy a PowerShell-parancsmagok használatával [visszaállítása egy törölt Office 365 csoport az Azure Active Directoryban] leírtak (https://docs.microsoft.com/azure/active-directory/ Active-Directory-groups-restore-Azure-Portal).
    
Ha most visszaállítása hello csoport tartalmazza a dokumentumok, SharePoint-webhelyeken vagy más állandó objektumok, eltarthat too24 óra toofully visszaállítási hello csoportot és annak tartalmát.

> [!NOTE]
> * Hello lejárati beállítások telepítésekor előfordulhat néhány olyan csoportot, a régebbi, mint hello lejárati időszak. Ezek a csoportok nem lehet azonnal törlődnek, de beállítani lejárati too30 napok száma. hello első megújítási értesítő e-mailt által kiküldött egy napon belül. Például csoport 400 nappal ezelőtt lett létrehozva, és hello lejárati időköz értéke too180 nap. Lejárati beállítások alkalmazásakor a csoport van 30 nap elteltével törlődik, ha hello tulajdonos megújítja azt.
> * Ha egy dinamikus csoport törlése és visszaállítása, az új csoport és újra ki van töltve függően toohello szabály látható. Ez a folyamat eltarthat too24 órában.

## <a name="next-steps"></a>Következő lépések
Ezek a cikkek kiegészítő információt nyújt az Azure Active Directory csoportokat.

* [Tekintse meg a meglévő csoportok](active-directory-groups-view-azure-portal.md)
* [Egy csoport beállításainak kezelése](active-directory-groups-settings-azure-portal.md)
* [A csoport tagjai kezelése](active-directory-groups-members-azure-portal.md)
* [A csoport tagságát kezelése](active-directory-groups-membership-azure-portal.md)
* [A csoport dinamikus szabályok kezelése](active-directory-groups-dynamic-membership-azure-portal.md)
