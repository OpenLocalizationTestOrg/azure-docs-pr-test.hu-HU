---
title: "aaaHow tooAssign felhasználók tooapplications |} Microsoft Docs"
description: "Hogyan legyenek hozzárendelve felhasználók tooan alkalmazás az Ön bérelt szolgáltatásának ismertetése"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 4df60c7d723140d0d1bbd6ba8b34aa4e762d1138
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooassign-users-tooapplications"></a>Hogyan tooassign felhasználók tooapplications

Ez a cikk segítséget toounderstand hogyan legyenek hozzárendelve felhasználók tooan alkalmazás az Ön bérelt szolgáltatásának.

## <a name="how-do-users-get-assigned-tooan-application-in-azure-ad"></a>Hogyan tegye felhasználók legyenek hozzárendelve tooan alkalmazás az Azure AD?

A felhasználó tooaccess egy alkalmazást akkor először meg kell adni tooit valamilyen módon. Hozzárendelés végzi el a rendszergazda, egy üzleti delegált, vagy bizonyos esetekben hello felhasználói magukat. Alább ismerteti azokat a felhasználókat is legyenek hozzárendelve tooapplications hello módszereket:

1.  A rendszergazda [rendel egy felhasználói](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) közvetlenül toohello alkalmazás

2.  A rendszergazda [hozzárendel egy csoport](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) hello felhasználó tagja toohello alkalmazás, beleértve:

  * Egy csoport, szinkronizált helyszíni

  * A létrehozott hello felhőben statikus biztonsági csoport

  * A [dinamikus biztonsági csoport](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal) hello felhő létrehozása

  * Az Office 365 hello felhő létrehozott csoport

  * Hello [minden felhasználó](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-dedicated-groups) csoport

3.  Lehetővé teszi a rendszergazda [önkiszolgáló alkalmazás-hozzáférés](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) egy felhasználó tooadd egy alkalmazást, amely hello tooallow [alkalmazás hozzáférési Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **alkalmazás hozzáadása** szolgáltatás**üzleti jóváhagyása nélkül**

4.  Lehetővé teszi a rendszergazda [önkiszolgáló alkalmazás-hozzáférés](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) egy felhasználó tooadd egy alkalmazást, amely hello tooallow [alkalmazás hozzáférési Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **alkalmazás hozzáadása** szolgáltatást, de csak w **edik előzetes jóváhagyási vállalati jóváhagyó egy kijelölt készletből**

5.  Lehetővé teszi a rendszergazda [önkiszolgáló csoportkezelési](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) tooallow egy felhasználó toojoin egy csoportot, amely az alkalmazás hozzá van rendelve túl**üzleti jóváhagyása nélkül**

6.  Lehetővé teszi a rendszergazda [önkiszolgáló csoportkezelési](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) tooallow egy felhasználó toojoin egy csoportot, amely az alkalmazás hozzá van rendelve, a, de csak **vállalati jóváhagyó egy kijelölt készletből előzetes jóváhagyással**

7.  A rendszergazda hozzárendeli a licenc tooa felhasználó közvetlenül az első gyártótól származó alkalmazás, például [Microsoft Office 365](http://products.office.com/)

8.  Egy rendszergazda rendel, amely a felhasználó hello tooa licenccsoport tagja tooa első gyártótól származó alkalmazás, például a [Microsoft Office 365](http://products.office.com/)

9.  Egy [rendszergazda beleegyezik tooan alkalmazás](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) toobe használt minden felhasználó és a felhasználó bejelentkezik majd toohello alkalmazás

10. A felhasználó [tooan alkalmazás beleegyezik](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) magukat a bejelentkezéssel toohello alkalmazás

## <a name="next-steps"></a>Következő lépések
[Alkalmazások kezelése az Azure Active Directoryban](active-directory-enable-sso-scenario.md)
