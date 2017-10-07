---
title: "egy új több-bérlős alkalmazás aaaHow tooconfigure |} Microsoft Docs"
description: "Hogyan tooconfigure egyszeri bejelentkezést az egyéni alkalmazás fejlesztés és regisztrálása az Azure ad-val."
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
ms.openlocfilehash: 4d3499d8885933516d6597fa9f87bcf88cd5a428
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-a-new-multi-tenant-application"></a>Hogyan tooconfigure új több-bérlős alkalmazás

Az alkalmazás összevont egyszeri bejelentkezést (SSO) engedélyezése automatikusan engedélyezi az Azure AD keresztül OpenID Connect, az SAML 2.0-s vagy a WS-Fed összevonását. Ha a végfelhasználók toosign annak ellenére, hogy már rendelkezik az Azure AD egy meglévő munkamenetben, valószínű, az alkalmazás nincs megfelelően konfigurálva.

* Ha az adal-t/MSAL használata esetén ellenőrizze, hogy **PromptBehavior** túl beállítása**automatikus** helyett **mindig**.

* A mobilalkalmazások most létrehozása, ha szükség lehet további konfigurációs tooenable által felügyelt vagy nem közvetítőalapú egyszeri Bejelentkezést.

Lásd: Android, [engedélyezése kereszt-alkalmazás SSO Android](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android).<br>

Az iOS számára, lásd: [Cross-SSO alkalmazás engedélyezése IOS](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios).

## <a name="next-steps"></a>Következő lépések

[Az Azure AD-egyszeri bejelentkezés](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)<br>

[Közötti App SSO Android engedélyezése](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android)<br>

[Cross-SSO App engedélyezése iOS-ban](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios)<br>

[Alkalmazások tooAzureAD integrálása](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)<br>

[Hozzájárulás és Permissioning AzureAD v2.0 az összevont alkalmazások](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[AzureAD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
