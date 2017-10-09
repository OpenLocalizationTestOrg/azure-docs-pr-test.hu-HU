---
title: "egy egyéni által fejlesztett alkalmazás adott mezők kimenő aaaHow toofill |} Microsoft Docs"
description: "Útmutatás a hogyan toofill kimenő egyedi mezők, ha regisztrál egyéni fejlett alkalmazást az Azure AD"
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
ms.openlocfilehash: 7e07bc45c58542edb3863db5aad7c845f1a1772e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toofill-out-specific-fields-for-a-custom-developed-application"></a>Hogyan toofill meg egy egyéni által fejlesztett alkalmazás adott mezők

Ez a cikk Önnek összes hello elérhető mezők hello alkalmazás regisztrációs űrlap rövid leírását a hello [Azure-portálon](https://portal.azure.com).

## <a name="register-a-new-application"></a>Egy új alkalmazás regisztrálása

-   tooregister egy új alkalmazást, nyissa meg a toohello [Azure-portálon](https://portal.azure.com).

-   Hello bal oldali navigációs ablaktáblán kattintson **Azure Active Directoryban.**

-   Válasszon **App regisztrációk** kattintson **Hozzáadás**.

-   A nyílt hello alkalmazás regisztrációs űrlap.

## <a name="fields-in-hello-application-registration-form"></a>Hello alkalmazás regisztrációs űrlap mezők


| Mező            | Leírás                                                                              |
|------------------|------------------------------------------------------------------------------------------|
| Név             | hello hello alkalmazás neve. Ennek tartalmaznia kell legalább 4 karakter.                |
| Alkalmazás típusa | **Webalkalmazást vagy webes API**: egy alkalmazás, amely egy webes alkalmazás, egy webes API vagy mindkettő 
| |**Natív**: a felhasználói eszköz vagy a számítógépen telepített alkalmazás           |
| Bejelentkezési URL-cím      | Ha felhasználó tud egyszerre bejelentkezni toouse az alkalmazás hello URL-címe                                  |

Miután megadta a hello mezők felett, hello alkalmazást regisztrálni kell a hello Azure-portálon, és akkor irányítják át toohello alkalmazás lapján. Hello **beállítások** hello beállítások oldalon, amelynek több mezőt meg toocustomize az alkalmazás megnyílik hello alkalmazás ablaktáblájának gombjára. hello az alábbi táblázat hello beállítások lapon található összes hello mezőhöz. Vegye figyelembe, hogy csak mutatunk be ezeket a mezőket, attól függően, hogy egy webes alkalmazás vagy a natív alkalmazás létrehozása egy részét.

| Mező           | Leírás                                                                                                                                                                                                                                                                                                     |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Alkalmazásazonosító  | Ha egy alkalmazás regisztrálása az Azure AD hozzárendeli az alkalmazás egy azonosítót. hello Alkalmazásazonosító használható toouniquely azonosítása a hitelesítési kérések tooAzure AD, valamint tooaccess erőforrások, például a Graph API hello.                                                          |
| App ID URI      | Egy egyedi URI Azonosítóját, általában hello űrlap legyen **https://&lt;bérlői\_neve&gt;/&lt;alkalmazás\_neve&gt;.** Ez lesz során hello engedélyezési grant flow, mely hello jogkivonat számára kell kiadni, az egyedi azonosító toospecify hello erőforrás. Hello "és" jogcím a kiállított hello hozzáférési jogkivonat is válik. |
| Új embléma feltöltése | Az alkalmazás a tooupload emblémát is használhatja. hello embléma .bmp, .jpg vagy .png formátumúnak kell lennie, és hello mérete kisebb, mint 100KB kell lennie. hello dimenziók hello lemezkép 215 x 215 képpont, központi lemezkép dimenziókkal 94 x 94 képpontban kell lennie.                                                       |
| Kezdőlap   | Ez az hello bejelentkezési URL-alkalmazás regisztrációja során megadott.                                                                                                                                                                                                                                              |
| Kijelentkezési URL-címe      | A hello egyetlen kijelentkezési kijelentkezési URL-címe. Az Azure AD küld a kijelentkezési kérelem toothis URL-címe hello felhasználó törli a munkamenetet és az Azure AD más regisztrált alkalmazás használatával.                                                                                                                                       |
| Többszörös központjaként  | Ez a kapcsoló határozza meg, hogy több bérlő hello alkalmazás használhatja. Ez általában azt jelenti, hogy a külső szervezetek használhatja-e a az alkalmazás regisztrálása a bérlőben és hozzáférési tootheir szervezeti adatok megadása.                                                                   |
| Válasz URL-címek      | hello válasz URL-címei hello végpontok, ahol az Azure AD vissza az alkalmazás által kért jogkivonatokhoz.                                                                                                                                                                                                          |
| Átirányítási URI   | Natív alkalmazások ez pedig ahol hello felhasználói kell elküldött toofollowing a sikeres hitelesítést. Az Azure AD, ellenőrizze, hogy hello átirányítási URI-címe az alkalmazás rendelkezik, az OAuth 2.0 hello kérelem egyezik egy regisztrált hello értékek hello portálon.                                                            |
| Kulcsok            | Kulcsok tooprogrammatically hozzáférés webes API-k, felhasználói beavatkozás nélkül az Azure AD által védett hozhat létre. A hello \* \*kulcsok\* \* lapon adjon meg egy kulcs leírását és hello lejárati dátumot és toogenerate hello kulcs mentése. Győződjön meg arról, hogy toosave valahol biztonságos, nem fogja tudni tooaccess, azt később.             |

## <a name="next-steps"></a>Következő lépések
[Alkalmazások kezelése az Azure Active Directoryban](active-directory-enable-sso-scenario.md)
