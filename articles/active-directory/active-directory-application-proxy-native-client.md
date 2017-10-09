---
title: "natív ügyfél aaaPublish - alkalmazások az Azure AD |} Microsoft Docs"
description: "Ismerteti, hogyan tooenable natív ügyfél alkalmazások toocommunicate az Azure AD alkalmazásproxy-összekötő tooprovide biztonságos távoli hozzáférés tooyour a helyszíni alkalmazások."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: f0cae145-e346-4126-948f-3f699747b96e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 0ed2be217bf992f034d8321d5e66569b4cace24f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooenable-native-client-apps-toointeract-with-proxy-applications"></a>Hogyan tooenable natív ügyfél alkalmazások toointeract proxyval alkalmazások

Továbbá tooweb alkalmazásokban Azure Active Directory Alkalmazásproxyjával is használt toopublish natív ügyfél alkalmazásokat. Natív ügyfél alkalmazások eltérnek a webalkalmazások, mert telepítik őket egy eszközt, amíg a böngészőn keresztül elért webalkalmazások. 

Alkalmazásproxy által kiadott jogkivonatokat, amelyek szabványos engedélyezik HTTP-fejlécek elküldése megtörténjen fogadja el az Azure AD natív ügyfél alkalmazásokat támogatja.

![A végfelhasználók, az Azure Active Directory és a közzétett alkalmazások közötti kapcsolat](./media/active-directory-application-proxy-native-client/richclientflow.png)

Azure AD Authentication Library, amely gondoskodik a hitelesítésről és sok ügyfél környezetek és natív alkalmazások toopublish hello használata. Alkalmazásproxy illeszkedik hello [natív alkalmazás tooWeb API forgatókönyv](develop/active-directory-authentication-scenarios.md#native-application-to-web-api). Ez a cikk bemutatja, hogyan hello négy lépést toopublish alkalmazásproxy és hello Azure AD Authentication Library egy natív alkalmazást. 

## <a name="step-1-publish-your-application"></a>1. lépés: Az alkalmazás közzétételére.
A proxy alkalmazás közzététele, mint bármilyen más alkalmazást, és rendelje hozzá a felhasználók tooaccess az alkalmazást. További információkért lásd: [alkalmazások közzétételére az alkalmazásproxy](active-directory-application-proxy-publish.md).

## <a name="step-2-configure-your-application"></a>2. lépés: Az alkalmazás konfigurálása
A natív alkalmazások konfigurálása az alábbiak szerint:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Keresse meg a túl**Azure Active Directory** > **App regisztrációk**.
3. Válassza ki **új alkalmazás regisztrációja**.
4. Adjon meg egy nevet az alkalmazásnak, válassza **natív** hello alkalmazás típusa, és adja meg a hello átirányítási URI-alkalmazás. 

   ![Hozzon létre egy új alkalmazás regisztrálása](./media/active-directory-application-proxy-native-client/create.png)
5. Kattintson a **Létrehozás** gombra.

Új alkalmazás regisztrációjának létrehozásával kapcsolatos információért, lásd: [alkalmazások integrálása az Azure Active Directory](.//develop/active-directory-integrating-applications.md).


## <a name="step-3-grant-access-tooother-applications"></a>3. lépés: Grant tooother alkalmazásokat
Hello natív alkalmazás kitett toobe tooother alkalmazások a címtárban engedélyezése:

1. Még mindig **App regisztrációk**, válassza ki az imént létrehozott új natív alkalmazás hello.
2. Válassza ki **szükséges engedélyek**.
3. Válassza a **Hozzáadás** lehetőséget.
4. Nyissa meg hello első lépésként **API kiválasztása**.
5. Hello keresési sáv toofind hello alkalmazásproxy alkalmazás használata közzétett hello első szakaszában. Válassza ki, hogy az alkalmazást, majd kattintson az **válasszon**. 

   ![Keresse meg hello proxy alkalmazást](./media/active-directory-application-proxy-native-client/select_api.png)
6. Nyissa meg hello a második lépésben **engedélyként válassza**.
7. Használjon hello jelölőnégyzet toogrant a natív alkalmazás access tooyour proxy alkalmazást, majd kattintson a **válasszon**.

   ![Adjon hozzáférést tooproxy alkalmazás](./media/active-directory-application-proxy-native-client/select_perms.png)
8. Válassza ki **végzett**.


## <a name="step-4-edit-hello-active-directory-authentication-library"></a>4. lépés: Az Active Directory Authentication Library hello szerkesztése
Hello natív alkalmazáskód hello hitelesítési környezetében hello Active Directory Authentication Library (ADAL) tooinclude hello a következő szöveg szerkesztése:

```
// Acquire Access Token from AAD for Proxy Application
AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<Tenant ID>");
AuthenticationResult result = authContext.AcquireToken("< External Url of Proxy App >",
        "<App ID of hello Native app>",
        new Uri("<Redirect Uri of hello Native App>"),
        PromptBehavior.Never);

//Use hello Access Token tooaccess hello Proxy Application
HttpClient httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");
```

hello mintakód hello változók ki kell cserélni az alábbiak szerint:

* **A bérlői azonosító** hello Azure-portálon található. Keresse meg a túl**Azure Active Directory** > **tulajdonságok** és másolási hello Directory azonosítóját. 
* **Külső URL-cím** hello előtér URL-cím megadott hello Proxy alkalmazás. toofind Ez érték, keresse meg a toohello **alkalmazásproxy** hello proxy app szakasza.
* **Alkalmazásazonosító** hello a natív alkalmazással megtalálja az hello **tulajdonságok** hello natív alkalmazás lapján.
* **Átirányítási URI hello natív alkalmazás** hello található **átirányítási URI-azonosítók** hello natív alkalmazás lapján.


## <a name="see-also"></a>Lásd még:

Hello natív alkalmazási folyamatot kapcsolatos további információkért lásd: [natív alkalmazás tooweb API](develop/active-directory-authentication-scenarios.md#native-application-to-web-api)

Hello legfrissebb híreket és frissítéseket, tekintse meg a hello [alkalmazásproxy blog](http://blogs.technet.com/b/applicationproxyblog/)
