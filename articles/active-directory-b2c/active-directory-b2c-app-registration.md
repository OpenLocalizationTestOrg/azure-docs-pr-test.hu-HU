---
title: "Azure Active Directory B2C: Alkalmazás regisztrációja | Microsoft Docs"
description: "Hogyan tooregister az alkalmazás Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: PatAltimore
ms.assetid: 20e92275-b25d-45dd-9090-181a60c99f69
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/13/2017
ms.author: parakhj
ms.openlocfilehash: bd58e123751db387d6c8f16bd010291ba698b1a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-register-your-application"></a>Azure Active Directory B2C: Az alkalmazás regisztrációja

Ez a gyors útmutató pár percben összefoglalja egy alkalmazás regisztrálásának folyamatát egy Microsoft Azure Active Directory (Azure AD) B2C-bérlőben. Amikor végzett, az alkalmazás regisztrálva van hello Azure B2C-bérlőben.

## <a name="prerequisites"></a>Előfeltételek

a felhasználói regisztrációt és bejelentkezést elfogadó alkalmazás toobuild, először tooregister hello alkalmazás Azure Active Directory B2C-bérlő. Című rész lépéseit hello segítségével könnyebben nyerhet saját bérlőt [az Azure AD B2C bérlő létrehozása](active-directory-b2c-get-started.md).

Az Azure AD B2C hello panel az hello Azure-portálon létrehozott alkalmazások felügyelni a hello ugyanazon a helyen. Ha manuálisan szerkeszti a PowerShell vagy egy másik portálon hello B2C alkalmazások, nem támogatott lesz, és nem működik együtt az Azure AD B2C. A részleteket a hello [hibát jelzett az alkalmazások](#faulted-apps) szakasz. 

## <a name="navigate-toob2c-settings"></a>Keresse meg a tooB2C beállítások

Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/) , hello hello B2C bérlő globális rendszergazdája. 

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](../../includes/active-directory-b2c-portal-navigate-b2c-service.md)]

Válassza ki a következő lépések regisztrál hello alkalmazás típusától függően:

* [Webalkalmazás regisztrációja](#register-a-web-app)
* [Webes API regisztrálása](#register-a-web-api)
* [Mobil vagy natív alkalmazás regisztrálása](#register-a-mobile-or-native-app)
 
## <a name="register-a-web-app"></a>Webalkalmazás regisztrációja

[!INCLUDE [active-directory-b2c-register-web-app](../../includes/active-directory-b2c-register-web-app.md)]

Ha webalkalmazása az Azure AD B2C által védett webes API-t hív meg, kövesse a következő lépéseket:
   1. Hozzon létre egy alkalmazás titkos kulcs is toohello **kulcsok** panelen, majd kattintson a hello **készítése kulcs** gombra. Jegyezze fel a hello **Alkalmazáskulcs** érték. Az alkalmazás kódjában hello alkalmazás titkos kulcsként hello érték használata.
   2. Kattintson az **API-hozzáférés**, majd a **Hozzáadás**, lehetőségre, és válassza ki webes API-ját és a hatóköröket (engedélyeket).

> [!NOTE]
> Az **Application Secret** (Alkalmazástitok) fontos biztonsági hitelesítő adat, amelynek megfelelő biztonságáról gondoskodni kell.
> 

[Túl Jump**további lépések**](#next-steps)

## <a name="register-a-web-api"></a>Webes API regisztrálása

[!INCLUDE [active-directory-b2c-register-web-api](../../includes/active-directory-b2c-register-web-api.md)]

Kattintson a **hatókörök közzétett** tooadd több hatóköröket annak megfelelően szükség szerint. Alapértelmezés szerint hello "user_impersonation" hatókörben van definiálva. hello user_impersonation hatókör az api ad más alkalmazások hello képességét tooaccess hello bejelentkezett felhasználó nevében. Ha kívánja, hello user_impersonation hatókör távolíthatja el.

[Túl Jump**további lépések**](#next-steps)

## <a name="register-a-mobile-or-native-app"></a>Mobil vagy natív alkalmazás regisztrálása

[!INCLUDE [active-directory-b2c-register-mobile-native-app](../../includes/active-directory-b2c-register-mobile-native-app.md)]

[Túl Jump**további lépések**](#next-steps)

## <a name="limitations"></a>Korlátozások

### <a name="choosing-a-web-app-or-api-reply-url"></a>Webalkalmazás vagy API-válasz URL-címének kiválasztása

Alkalmazások regisztrált az Azure AD B2C jelenleg korlátozott korlátozott tooa értékhalmazt válasz URL-CÍMÉT. válasz URL-címet a webalkalmazások és szolgáltatások hello sémát kell kezdődnie hello `https`, és minden válasz URL-cím értékek közös egyetlen DNS-tartományt. Például nem regisztrálhat olyan webalkalmazást, amely az alábbi URL-címek egyikével rendelkezik:

`https://login-east.contoso.com`

`https://login-west.contoso.com`

hello regisztrációs rendszer összehasonlítja a hello meglévő válasz URL-cím toohello DNS-neve hello válasz URL-címet ad hozzá hello teljes DNS-nevét. hello kérelem tooadd hello DNS-név sikertelen lesz, ha hello a következő feltételek valamelyike teljesül:

* hello új válasz URL-CÍMEN hello teljes DNS-neve nem egyezik meg a DNS-neve hello hello meglévő válasz URL-CÍMEN.
* teljes DNS-neve hello hello új válasz URL-CÍMEN altartománya nem hello meglévő válasz URL-CÍMEN.

Például ha hello app van a válasz URL-címe:

`https://login.contoso.com`

Hozzáadhat tooit ehhez hasonló:

`https://login.contoso.com/new`

Ebben az esetben hello DNS-név pontosan megegyezik. Esetleg a következőt teheti meg:

`https://new.login.contoso.com`

Ebben az esetben hivatkozott login.contoso.com a tooa DNS altartományában. Ha azt szeretné, hogy egy alkalmazás, amely rendelkezik bejelentkezési-east.contoso.com és a bejelentkezési-west.contoso.com, válasz URL-címek toohave, hozzá kell adnia az válasz URL-címeket az itt megadott sorrendben:

`https://contoso.com`

`https://login-east.contoso.com`

`https://login-west.contoso.com`

Hello utóbbi két adhat hozzá, mivel azok hello első válasz URL-CÍMEN, az altartományok contoso.com.

### <a name="choosing-a-native-app-redirect-uri"></a>Natív alkalmazás átirányítási URI-jának kiválasztása

A mobil-/natív alkalmazások átirányítási URI-jának kiválasztásakor két fontos szempontot kell megfontolnia:

* **Egyedi**: hello sémája hello átirányítási URI-t minden egyes alkalmazás egyedinek kell lennie. Ebben a példában (com.onmicrosoft.contoso.appname://redirect/path) a com.onmicrosoft.contoso.appname hello sémát használjuk. Javasoljuk, hogy ezt a mintát kövesse. Ha a két alkalmazás megosztása hello azonos sémáját, hello felhasználónál "Válasszon app" párbeszédpanelen. Hello felhasználó esetén a megfelelő választás, ha sikertelen hello bejelentkezést.
* **Teljes**: Az átirányítási URI-nak sémával és elérési útvonallal kell rendelkeznie. hello elérési utat tartalmaznia kell legalább egy perjel után hello tartomány (például //contoso/ works és //contoso sikertelen).

Gondoskodjon arról, hogy nincsenek különleges karakterek, például aláhúzásjelet hello az átirányítási URI-címe.

### <a name="faulted-apps"></a>Hibás alkalmazások

A B2C-alkalmazások NEM szerkeszthetők:

* Egyéb alkalmazáskezelési portálokon, például a [klasszikus Azure portálon](https://manage.windowsazure.com/) vagy az [alkalmazásregisztrációs portálon](https://apps.dev.microsoft.com/).
* Graph API vagy PowerShell használata

Ha hello B2C alkalmazás szerkesztése a fent leírt módon, majd próbálja tooedit azt újra hello Azure AD B2C funkciók panelére hello Azure-portálon válik, a hibás alkalmazást és az alkalmazás már nem használható az Azure AD B2C. Toodelete hello alkalmazást, és hozza létre újra.

toodelete hello alkalmazást, és lépjen toohello [alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/) és hello alkalmazás törlése. Ahhoz, hogy hello alkalmazás toobe látható kell hello alkalmazás tulajdonosának toobe hello (és nem csak egy hello Bérlői rendszergazda).

## <a name="next-steps"></a>Következő lépések

Most, hogy az Azure AD B2C-vel regisztrált egy alkalmazást, akkor fejezheti be egyik [a gyors üzembe helyezési oktatóanyag](active-directory-b2c-overview.md#get-started) tooget lépéseivel.

> [!div class="nextstepaction"]
> [ASP.NET-webalkalmazás létrehozása regisztrációval, bejelentkezéssel és új jelszó kérésével](active-directory-b2c-devquickstarts-web-dotnet-susi.md)