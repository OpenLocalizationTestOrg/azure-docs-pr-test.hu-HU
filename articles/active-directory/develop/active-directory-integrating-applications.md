---
title: "Alkalmazások az Azure Active Directoryval aaaIntegrating |} Microsoft Docs"
description: "Hogyan tooadd, frissítése, vagy távolítsa el a kérelmet az Azure Active Directory (Azure AD-) adatokat."
services: active-directory
documentationcenter: 
author: lnalepa
manager: mbaldwin
editor: mbaldwin
ms.assetid: ae637be5-0b71-4b1e-b1fe-b83df3eb4845
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: lenalepa
ms.custom: aaddev
ms.reviewer: luleon
ms.openlocfilehash: c6bf1123bb3a4d78b55c1c55558e684fb844e687
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrating-applications-with-azure-active-directory"></a>Alkalmazások integrálása az Azure Active Directoryban
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Vállalati fejlesztők és a szoftver-,-a-(SaaS) szolgáltatók kereskedelmi felhőszolgáltatások vagy, amely integrálható az Azure Active Directory (Azure AD) tooprovide biztonságos bejelentkezéshez és az engedélyezést az üzletági alkalmazások is fejleszthetők a szolgáltatások. toointegrate egy alkalmazás vagy szolgáltatás az Azure ad-vel, a fejlesztő először regisztrálnia kell a hello adatait az alkalmazásokban az Azure ad-val hello a klasszikus Azure portálon keresztül.

Ez a cikk bemutatja, hogyan tooadd, frissítés, vagy távolítsa el a kérelmet az Azure ad-ben. Megtudhatja, hogyan integrálható az Azure ad-vel, hogyan alkalmazások különböző típusú hello tooconfigure az alkalmazások tooaccess más erőforrások, például webes API-k és még sok más.

toolearn hello két Azure AD-objektumok, amelyek megfelelnek egy bejegyzett alkalmazás- és hello kapcsolat, bővebben lásd: [alkalmazás és szolgáltatás egyszerű objektumok](active-directory-application-objects.md); toolearn további vonatkozó irányelvek branding hello meg érdemes az Azure Active Directoryval alkalmazások fejlesztése során használatát, lásd: [Branding irányelveket az integrált alkalmazások](active-directory-branding-guidelines.md).

## <a name="adding-an-application"></a>Egy alkalmazás hozzáadása
Bármely olyan szeretne toouse hello képességek az Azure AD alkalmazás előbb regisztrálni kell az Azure AD-bérlő. A regisztrációs folyamat során jogosultságot ad az Azure AD adatait az alkalmazás, például a hello URL-címe, ha található, hello URL-cím toosend válaszokat a felhasználó hitelesítése után, hello URI azonosító hello alkalmazást, és így tovább.

Ha egy webalkalmazást, amelyet a csak toosupport bejelentkezhet a felhasználók számára az Azure ad-ben van felépítése, egyszerűen követésével hello az alábbi utasítások. Ha az alkalmazás hitelesítő adatai vagy az engedélyek tooaccess tooa webes API kell, vagy más tooallow felhasználók kell az Azure AD bérlők tooaccess, lásd: [frissíteni az alkalmazást](#updating-an-application) szakasz toocontinue az alkalmazás konfigurálása.

### <a name="tooregister-a-new-application-in-hello-azure-portal"></a>tooregister egy új alkalmazást a hello Azure-portálon
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Válassza ki az Azure AD-bérlő fiókja hello jobb felső sarkában hello lap választásával.
3. Hello bal oldali navigációs ablaktábláján válassza **több szolgáltatások**, kattintson a **App regisztrációk**, és kattintson a **Hozzáadás**.
4. Hello utasításokat követve, és hozzon létre egy új alkalmazást. Ha szeretné, hogy a webes alkalmazások és natív alkalmazások példák, tekintse meg a [quickstarts](active-directory-developers-guide.md).
  * A webes alkalmazásokhoz, adja meg a hello **bejelentkezési URL-cím**, hello alap URL-CÍMÉT az alkalmazásához. Ez az adott felhasználó tud egyszerre bejelentkezni például `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: hello **App ID URI** is a unique identifier for your application. hello convention is toouse `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * Natív alkalmazások esetén adja meg a **átirányítási URI-**, mely az Azure AD tooreturn token válaszok használja. Adjon meg egy értéket adott tooyour alkalmazást. például`http://MyFirstAADApp`
5. Miután végrehajtotta a regisztráció, az Azure AD rendeli hozzá az alkalmazás egy egyedi ügyfél-azonosítókra, hello azonosítót. Az alkalmazás hozzá van adva, és megnyílik toohello gyors kezdés lapon az alkalmazáshoz. Attól függően, hogy az alkalmazás egy webes vagy natív alkalmazás, látni fogja a azon különböző beállítások tooadd további képességeket tooyour alkalmazás. Ha az alkalmazás hozzá van adva, az alkalmazás tooenable felhasználók toosign hozzáférés webes API-k más alkalmazásokban a frissítés megkezdéséhez, vagy konfigurálja a több-bérlős alkalmazás (amelyek más szervezetek tooaccess lehetővé teszi az alkalmazás).

> [!NOTE]
> Alapértelmezés szerint az újonnan létrehozott hello regisztrációja a címtár toosign tooyour alkalmazásban konfigurált tooallow felhasználóit.
> 
> 

## <a name="updating-an-application"></a>Egy alkalmazás frissítése
Ha az alkalmazás regisztrálva van az Azure ad-val, toobe frissíteni kell tooprovide tooweb API-k eléréséhez, elérhetik más szervezetek és még sok más. Ez a szakasz ismerteti a különböző módokon, ahol esetleg tooconfigure tovább az alkalmazás. Először fog először hello hozzájárulás keretrendszer áttekintése olyan fontos toounderstand készítésekor erőforrás/API-alkalmazások, amelyeket a fejlesztők a szervezet vagy egy másik szervezet beépített ügyfélalkalmazások fogja használni.

További információ a hello módon authentication úgy működik, az Azure ad-ben, lásd: [hitelesítési forgatókönyvek az Azure AD](active-directory-authentication-scenarios.md).

### <a name="overview-of-hello-consent-framework"></a>Hello hozzájárulási keretrendszer áttekintése
Az Azure AD hozzájárulási keretrendszer teszi, hogy könnyen toodevelop több-bérlős webes és tooaccess natív ügyfél alkalmazásokat webes API-khoz, az Azure AD-bérlő, ahol ügyfélalkalmazás hello regisztrálva van egy hello eltérő által védett. A webes API-k közé tartozik a hello Microsoft Graph API-val (a tooaccess Azure Active Directory, a Intune és a szolgáltatások az Office 365-ben) és más Microsoft-szolgáltatásokban API-k, továbbá tooyour saját webes API-k. hello keretrendszer egy felhasználó vagy a rendszergazda jogosultságot ad a hozzájárulási tooan alkalmazás, amely rákérdez, hogy a címtárban, amelyek magukban foglalhatják directory adatokhoz hozzáférő regisztrált toobe alapul.

Például ha egy ügyfél webalkalmazás tooread naptár információ hello felhasználói az Office 365 szolgáltatásból, hogy a felhasználó lesz szükséges tooconsent toohello ügyfélalkalmazást. Hozzájárulás kap, miután hello ügyfélalkalmazás képes toocall hello Microsoft Graph API hello felhasználó nevében, és hello naptári adatok használatára, szükség szerint. Hello [Microsoft Graph API](https://graph.microsoft.io) biztosít hozzáférést toodata az Office 365-ben (például a naptárak és az Exchange, helyek és a SharePoint, a dokumentumot a onedrive-ról a OneNote, Planner, munkafüzetek feladatok notebookok listák üzenetek Excel-, stb.), valamint felhasználók és csoportok az Azure AD és más adatok több Microsoft felhőszolgáltatás objektumokat. 

hello hozzájárulási keretrendszer OAuth 2.0-s és a különböző folyamatokra épül, támogatást és az ügyfél hitelesítő adatai megadják például engedélyezési kódot, nyilvános vagy titkos ügyfél használatával. OAuth 2.0 használatával az Azure AD teszi lehetséges toobuild ügyfélalkalmazások, például a szerint számos különböző típusú telefon, tábla-, kiszolgáló vagy egy webes alkalmazás, és hozzáférést toohello szükséges erőforrásokat.

Kapcsolatos részletesebb információkért hello hozzájárulási keretrendszer, lásd: [OAuth 2.0, az Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx), [hitelesítési forgatókönyvek az Azure AD](active-directory-authentication-scenarios.md), és további információ a kezdeti hitelesített hozzáférést tooOffice 365 keresztül A Microsoft Graph, lásd: [App hitelesítést a Microsoft Graph](https://graph.microsoft.io/docs/authorization/auth_overview).

#### <a name="example-of-hello-consent-experience"></a>Hello hozzájárulási élmény példája
hello lépések bemutatja, mindez hello hozzájárulási élmény működése hello alkalmazásfejlesztő és a felhasználó.

1. A webes ügyfél alkalmazás konfigurációs lapján hello Azure-portálon az alkalmazás által igényelt, a szükséges engedélyek szakasz hello hello menük használatával hello engedélyek beállítása.
   
    ![Engedélyek tooother alkalmazások](./media/active-directory-integrating-applications/requiredpermissions.png)
2. Vegye figyelembe, hogy az alkalmazás engedélyek frissítve lett, hello alkalmazás fut, és egy felhasználó toouse kapcsolatos azt hello az első alkalommal. Ha hello alkalmazás már nem kapott hozzáférési és frissítési jogkivonat, hello alkalmazás egy új hozzáférési és frissítési jogkivonat kell toogo tooAzure AD meg engedélyezési végpont tooobtain egy használt tooacquire engedélyezési kódot.
3. Hello felhasználó már nem hitelesíthetők, ha azok kell adnia a tooAzure AD toosign.
   
    ![TooAzure AD felhasználói vagy rendszergazdai bejelentkezés](./media/active-directory-integrating-applications/usersignin.png)
4. Hello felhasználó bejelentkezett, miután az Azure AD határozza meg, hogy kell-e a hozzájárulási lapon látható toobe hello felhasználói. Ez a döntés alapján e felhasználó hello (vagy annak szervezeti rendszergazdája) már adott hello kérelem jóváhagyását. Ha hozzájárulási már nincs megadva, az Azure AD kérni fogja a felhasználótól hello a, és toofunction kell hello szükséges engedélyek megjeleníti. hello engedélykészletüket hello hozzájárulási párbeszédpanelen megjelenő vannak hello ugyanaz, mint mi választotta, az delegált engedélyek hello hello Azure-portálon.
   
    ![Felhasználói hozzájárulás élmény](./media/active-directory-integrating-applications/consent.png)
5. Miután hello felhasználói hozzájárulás biztosít, az engedélyezési kód tooyour alkalmazás, amely visszavásárolt tooacquire olyan hozzáférési jogkivonatot és frissítési token adja vissza. Ez a folyamat kapcsolatos további információkért lásd: hello [webes alkalmazás tooweb API szakaszban](active-directory-authentication-scenarios.md#web-application-to-web-api) szakasz [hitelesítési forgatókönyvek az Azure AD](active-directory-authentication-scenarios.md).

6. A rendszergazdák is az Ön bérelt szolgáltatásának is hozzájárulás tooan alkalmazás delegált jogosultságokkal sikeresen telepítették az összes hello felhasználók nevében. Ez megakadályozza, hogy hello hozzájárulási párbeszédpanelen szereplő minden felhasználó hello-bérlőben. Ehhez a hello [Azure-portálon](https://portal.azure.com) az alkalmazás oldalról. A hello **beállítások** az alkalmazás paneljén kattintson **szükséges engedélyek** , majd kattintson a hello **engedélyt adjon** gombra. 

    ![Adja meg explicit admin szerezni a hozzájárulásukat engedélyeket](./media/active-directory-integrating-applications/grantpermissions.png)
    
> [!NOTE]
> Hello segítségével explicit hozzájárulás megadása **engedélyt adjon** gombra kell jelenleg használó ADAL.js, egylapos alkalmazások (SPA), hello hozzáférési jogkivonat beleegyezést kérő üzenete, ami sikertelen lesz, ha a hozzájárulási nincs nélkül van szükség. már megadott.   

### <a name="configuring-a-client-application-tooaccess-web-apis"></a>Egy ügyfél tooaccess webes API-k konfigurálása
Ahhoz, hogy egy webes bizalmas ügyfél alkalmazás toobe képes tooparticipate egy engedélyezési grant folyamat, amely megköveteli a hitelesítést (és egy hozzáférési jogkivonat beszerzése) azt kell létesítenie a biztonságos hitelesítő adatok. hello alapértelmezett hitelesítési módszer hello Azure-portál által támogatott ügyfél-azonosító + szimmetrikus kulcs. Ebben a szakaszban foglalkozunk hello konfigurációs lépéseket szükséges tooprovide hello titkos kulcs az ügyfél hitelesítő adatait.

Emellett egy ügyfél előtti hozzáférés a webes API által elérhetővé tett egy erőforrás-alkalmazáshoz (ie: Microsoft Graph API), hello hozzájárulási biztosítanak az hello ügyfél hello engedély biztosítása a kért szükséges, az hello jogosultságokat szerez. Alapértelmezés szerint minden alkalmazás engedélyek közül választhatnak Azure Active Directory (Graph API-val) és az Azure szolgáltatásfelügyeleti API, alapértelmezés szerint kiválasztva hello Azure AD "Enable bejelentkezés és olvasási profil" engedéllyel. Ha az ügyfélalkalmazást Azure AD az Office 365-bérlő regisztrálva van folyamatban, a webes API-k és a SharePoint és Exchange online-hoz is kell kijelölésnél érhetők el. Választhat [engedélyek két típusú](active-directory-dev-glossary.md#permissions) hello a következő toohello szükség legördülő menükben webes API-t:

* Alkalmazásengedélyek: Az ügyfélalkalmazást kell tooaccess hello webes API közvetlenül, saját magát (felhasználói környezet). Ez a típus engedély rendszergazda jóváhagyását igényli, és nem is érhető el natív ügyfél-alkalmazások.
* Delegált engedélyek: Az ügyfélalkalmazást kell tooaccess hello webes API hello bejelentkezett felhasználó nevében, de által kiválasztott hello engedély korlátozott hozzáféréssel. Az ilyen típusú engedéllyel is kell biztosítani egy felhasználó által hello engedély van konfigurálva, hogy a rendszergazda jóváhagyását. 

> [!NOTE]
> Egy engedélyt tooan alkalmazás hozzáadása nem automatikusan megadja a hozzájárulási toohello felhasználóknak belül hello bérlői, mint a klasszikus Azure portál hello. hello felhasználók kell továbbra is futtathatja manuálisan hozzájárulás hozzáadott hello delegált felügyeletre vonatkozó engedélyek futásidőben, kivéve, ha hello rendszergazda kattint hello **engedélyt adjon** gomb a hello **szükséges engedélyek** szakasz hello alkalmazás oldalának hello Azure-portálon. 

#### <a name="tooadd-credentials-or-permissions-tooaccess-web-apis"></a>tooadd hitelesítő adatait, vagy az engedélyek tooaccess webes API-khoz
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Válassza ki az Azure AD-bérlő fiókja hello jobb felső sarkában hello lap választásával.
3. Hello felső menüben válassza a **Azure Active Directory**, kattintson a **App regisztrációk**, majd kattintson a kívánt tooconfigure hello alkalmazás. Ez fogja toohello alkalmazás gyors üzembe helyezés lap léphet, valamint hello alkalmazás hello-beállítások panel megnyitása.
4. tooadd egy titkos kulcsot a webes alkalmazás hitelesítő adatait, kattintson a "Kulcsok" szakaszának hello hello-beállítások panel.  
   
   * Adjon meg egy leírást a kulcshoz, és válassza a 1 vagy 2 év időtartama. 
   * hello jobb szélső oszlop fogja tartalmazni hello kulcsértékkel, hello konfigurációs módosítások mentése után. Vissza kell, hogy toocome toothis szakaszban, és másolja, azt követően kattint mentéséhez, hogy az használja az ügyfélalkalmazást futásidőben a hitelesítés során.
5. tooadd eldobása tooaccess erőforrás API-k az ügyfél hello "Szükséges engedélyek" szakaszának hello-beállítások panelen kattintson. 
   
   * Első lépésként kattintson hello "Hozzáadás" gombra.
   * Kattintson "Az API kiválasztása" tooselect hello toopick a kívánt erőforrásokat.
   * Tallózza végig elérhető API-kat hello listája, vagy használjon hello Keresés mezőbe tooselect alkalmazásokból hello elérhető erőforrás a könyvtárban, amelyek teszi közzé a webes API-k. Kattintson a hello erőforrás kapcsolatban, majd kattintson a **válasszon**.
   * A kijelölt áthelyezheti toohello **Select engedélyeket** menü, ahol igény szerint hello "alkalmazás" és "Delegált engedélyek" az alkalmazás.
   
6. Ha elkészült, kattintson a hello **végzett** gombra.

> [!NOTE]
> Gombra kattintva hello **végzett** gombra is automatikusan hello olyan engedélyeket ad meg az alkalmazás a címtárban alapú hello engedélyeivel konfigurált tooother alkalmazások.  Ezek az Alkalmazásengedélyek hello alkalmazás bármikor megtekintheti **beállítások** panelen.
> 
> 

### <a name="configuring-a-resource-application-tooexpose-web-apis"></a>Egy erőforrás tooexpose webes API-k konfigurálása
Webes API-k fejlesztése, és könnyebben elérhető tooclient alkalmazások jelentkezik, mintha a hozzáférési [hatókörök](active-directory-dev-glossary.md#scopes) és [szerepkörök](active-directory-dev-glossary.md#roles). Egy megfelelően konfigurált webes API-t ugyanúgy, mint más Microsoft webes API-k, beleértve a Graph API hello hello és az Office 365 API-k hello legyen elérhető. A hozzáférési hatókörök és szerepkörök keresztül közzétett a [alkalmazás jegyzékfájlja](active-directory-dev-glossary.md#application-manifest), vagyis a JSON-fájl, amely az alkalmazáskonfiguráció identitásának jelöli.  

a következő szakasz hello bemutatja, hogyan tooexpose hozzáférési hatókörök-hello erőforrás alkalmazás jegyzékfájlja módosításával.

#### <a name="adding-access-scopes-tooyour-resource-application"></a>Hozzáférési hatókörök tooyour erőforrás alkalmazás hozzáadása
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Válassza ki az Azure AD-bérlő fiókja hello jobb felső sarkában hello lap választásával.
3. Hello felső menüben válassza a **Azure Active Directory**, kattintson a **App regisztrációk**, majd kattintson a kívánt tooconfigure hello alkalmazás. Ez fogja toohello alkalmazás gyors üzembe helyezés lap léphet, valamint hello alkalmazás hello-beállítások panel megnyitása.
4. Kattintson a **Manifest** a hello alkalmazás tooopen hello beágyazott jegyzék szerkesztő. 
5. Cserélje le a következő JSON részlet hello "oauth2Permissions" csomópont. Ezt a kódrészletet példája hogyan tooexpose néven "felhasználó megszemélyesítése", amely lehetővé teszi egy erőforrás tulajdonosa toogive ügyfélalkalmazás olyan típusú hatókör meghatalmazott hozzáférést tooa erőforrás. Győződjön meg arról, hogy módosította hello szöveg és az értékeket a saját alkalmazáshoz:
   
        "oauth2Permissions": [
        {
            "adminConsentDescription": "Allow hello application full access toohello Todo List service on behalf of hello signed-in     user",
            "adminConsentDisplayName": "Have full access toohello Todo List service",
            "id": "b69ee3c9-c40d-4f2a-ac80-961cd1534e40",
            "isEnabled": true,
            "type": "User",
            "userConsentDescription": "Allow hello application full access toohello todo service on your behalf",
            "userConsentDisplayName": "Have full access toohello todo service",
            "value": "user_impersonation"
            }
        ],
   
    hello azonosító értéknek kell lennie egy új előállított GUID Azonosítóhoz, amelyek használatával hoz létre egy [GUID generációs eszköz](https://msdn.microsoft.com/library/ms241442%28v=vs.80%29.aspx) vagy programon keresztül. Azt jelenti, hogy hello engedély hello webes API-k által elérhetővé egyedi azonosítója. Miután az ügyfél megfelelően van-e konfigurálva toorequest hozzáférés tooyour webes API-k és hívások hello webes API, az OAuth 2.0 JWT jogkivonat, amelynek hello hatókör (scp) set toohello jogcímérték fenti, amely ebben az esetben user_impersonation jelent.
   
   > [!NOTE]
   > További hatókörökkel később szükség szerint is elérhetővé teheti. Vegye figyelembe, hogy a webes API számos különböző funkcióihoz tartozó több hatókör esetleg felfedi. Most vezérelheti hozzáférés toohello webes API-k hello hatókör (scp) használatával jogcím kapott hello OAuth 2.0 JWT jogkivonat.
   > 
   > 
6. Kattintson a **mentése** toosave hello jegyzékben. A webes API-t használják a címtárban lévő más alkalmazásokkal toobe beállítását.

#### <a name="tooverify-hello-web-api-is-exposed-tooother-applications-in-your-directory"></a>tooverify hello webes API-k kitett tooother alkalmazások a könyvtárban
1. A hello felső menüben kattintson **App regisztrációk**, jelölje be hello szükséges ügyfélalkalmazás szeretné, hogy tooconfigure hozzáférés toohello webes API és toohello beállítások panelen keresse meg.
2. A hello **szükséges engedélyek** területen válassza ki az imént kitett engedély hello webes API. Hello delegált engedélyek legördülő menüben válassza ki a hello új engedély.

![Teendőlista engedélyek láthatók.](./media/active-directory-integrating-applications/todolistpermissions.png)

#### <a name="more-on-hello-application-manifest"></a>További információk az alkalmazásjegyzék hello
hello alkalmazásjegyzék ténylegesen a frissítési hello alkalmazás entitás, amely meghatározza egy Azure AD-alkalmazást identitás konfiguráció – beleértve az hello API a hozzáférési hatókörök tárgyalt összes attribútumát egy mechanizmusként funkcionál. A hello alkalmazás entitás további információkért lásd: hello [Graph API-alkalmazás entitás dokumentáció](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity). Az oktatóanyagban megtalálja hello entitás használt tagok toospecify Alkalmazásengedélyek az API-információk teljes:  

* hello appRoles tagot, amely gyűjtemény a [birtokolhat](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type) lehet használt toodefine hello entitások **Alkalmazásengedélyek** a webes API-k  
* hello oauth2Permissions tagot, amely gyűjtemény a [OAuth2Permission](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type) lehet használt toodefine hello entitások **delegált engedélyek** a webes API-k

További információ az alkalmazás jegyzékének fogalmak általában tekintse meg túl[ismertetése hello Azure Active Directory alkalmazásjegyzékének](active-directory-application-manifest.md).

### <a name="accessing-hello-azure-ad-graph-and-office-365-via-microsoft-graph-apis"></a>Hello Azure AD Graph és az Office 365 keresztül Microsoft Graph API-k eléréséhez  
A saját erőforrás alkalmazások említett korábbi, továbbá tooexposing/használata API-, mint az ügyfél alkalmazás tooaccess API-k által Microsoft erőforrások is frissítheti.  hello Microsoft Graph API, amely "Microsoft Graph" hello listában engedélyek tooother alkalmazások neve, érhető el vagy az Azure ad-vel regisztrált összes alkalmazást. Ha az ügyfélalkalmazást Azure AD-bérlő, amely az Office 365 lett kiépítve a regisztrál, is elérhető az összes Microsoft Graph API toovarious Office 365-erőforrások hello által elérhetővé tett hello engedélyeket.

A Microsoft Graph API által a hozzáférési hatókörök teljes leírása, lásd: hello [engedélyhatókörök |} Microsoft Graph API-val kapcsolatos fogalmak](https://graph.microsoft.io/docs/authorization/permission_scopes) cikk.

> [!NOTE]
> Tooa jelenlegi korlátozás miatt natív ügyfélalkalmazások csak meghívhatja az Azure AD Graph API hello ha azok hello "Hozzáférés a munkahely címtárában" engedéllyel.  Ez a korlátozás vonatkozik webes alkalmazásokhoz.
> 
> 

### <a name="configuring-multi-tenant-applications"></a>Több-bérlős alkalmazások konfigurálása
Ha egy alkalmazás tooAzure AD hozzá, érdemes lehet az alkalmazás toobe csak a szervezeti felhasználók férhetnek hozzá. Azt is megteheti érdemes lehet az alkalmazás toobe elérhetők a felhasználók a külső szervezetek számára. A két alkalmazás nevezik egybérlős és a több-bérlős alkalmazásokhoz. Egy bérlő egyetlen alkalmazás toomake hello konfigurációját módosíthatja azt egy több-bérlős alkalmazást, amely alatt ez a szakasz tárgyalja.

Fontos fontos toonote hello különbségek egy bérlő egyetlen és több-bérlős alkalmazás között:  

* Egy bérlő egyetlen alkalmazás használatát az egyik szervezet szól. Általában egy-üzletági (LoB) alkalmazás egy vállalati fejlesztők által írt. Egy bérlő egyetlen alkalmazást csak kell toobe elérhetők a felhasználók számára pedig egy címtárban, ezért az emiatt csak egy címtárban kiépített toobe.
* Egy több-bérlős alkalmazás számos szervezet számára készült. A szoftver,--szolgáltatás (SaaS) webalkalmazás általában egy független szoftverszállító (ISV) által írt. Több-bérlős alkalmazásokhoz kiosztott összes könyvtárban, ahol használni fogják őket, felhasználói vagy rendszergazdai hozzájárulási tooregister igénylő toobe kell őket az Azure AD hello hozzájárulási keretrendszer használatával támogatott. Ne feledje, hogy minden natív ügyfélalkalmazások alapértelmezés szerint több-bérlős hello erőforrás tulajdonosa eszközön telepítettként. Hello áttekintése hello hozzájárulás keretrendszer szakasz fenti további részletekért tekintse meg hello hozzájárulási keretrendszer.

#### <a name="enabling-external-users-toogrant-your-application-access-tootheir-resources"></a>Külső felhasználók toogrant engedélyezése az alkalmazás-hozzáférés tootheir erőforrások
Ha egy alkalmazás, amelyet az toomake elérhető tooyour ügyfeleket vagy partnereket a szervezeten kívül, szüksége lesz a tooupdate hello definíciót a hello Azure-portálon.

> [!NOTE]
> Ha engedélyezve van a több-bérlős, győződjön meg róla, hogy az alkalmazás App ID URI egy ellenőrzött tartomány tartozik. Emellett hello visszatérési URL-csak a https:// kezdetű. További információkért lásd: [alkalmazás és szolgáltatás egyszerű objektumok](active-directory-application-objects.md).
> 
> 

tooenable hozzáférés tooyour alkalmazás a külső felhasználók számára: 

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Válassza ki az Azure AD-bérlő fiókja hello jobb felső sarkában hello lap választásával.
3. Hello felső menüben válassza a **Azure Active Directory**, kattintson a **App regisztrációk**, majd kattintson a kívánt tooconfigure hello alkalmazás. Ez fogja toohello alkalmazás gyors üzembe helyezés lap léphet, valamint hello alkalmazás hello-beállítások panel megnyitása.
4. Hello-beállítások panelen kattintson **tulajdonságok** és tükrözés hello **Multi-központjaként** túl kapcsoló**Igen**.

Fent, felhasználók és más vállalatok rendszergazdái módosítása hello végrehajtott állnak képes toogrant az alkalmazás hozzáférési tootheir könyvtárába és egyéb adatokat.

#### <a name="triggering-hello-azure-ad-consent-framework-at-runtime"></a>Az Azure AD hello hozzájárulási keretrendszer futásidőben időt.
toouse hello hozzájárulási keretrendszer, a több-bérlős ügyfélalkalmazások kell igényelnie engedélyezési az OAuth 2.0 verziót használja. [Kódminták](https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multi-tenant) vannak elérhető tooshow hogyan egy webes alkalmazás, a natív alkalmazás vagy a kiszolgáló/démon alkalmazás kérelmek engedélyezési kódok és hozzáférési jogkivonatok toocall webes API-khoz.

A webes alkalmazás is kínálhat bejelentkezési élmény a felhasználók számára. Ha egy olyan regisztrációs felület, várhatóan hello felhasználó fog kattintson a gombra egy bejelentkezési, hogy az átirányítási hello böngésző toohello Azure AD OAuth2.0 engedélyeztetéséhez használandó végpont vagy OpenID Connect userinfo végpontot. Ezeket a végpontokat hello alkalmazás tooget hello új felhasználó adatait hello id_token vizsgálatával engedélyezése. Következő hello előfizetési fázis hello felhasználó számára jelenik meg a hozzájárulás kérése hasonló toohello valamelyik fenti hello hello hozzájárulás keretrendszer szakasz áttekintést.

Másik lehetőségként a webes alkalmazás is kínálhat számára lehetővé teszi a rendszergazdák túl "iratkozzon fel a vállalatom". Ez a felület is tenné átirányítási hello felhasználói toohello Azure AD OAuth 2.0 végpont hitelesítéséhez. Ebben az esetben azonban adja meg a kérdés = admin_consent paraméter toohello végpont tooforce hello rendszergazdák hozzájárulási munkáját, ahol hello rendszergazda megadja a szervezet nevében hozzájárulás engedélyezéséhez. Csak egy felhasználó egy olyan fiókkal, amely toohello globális rendszergazdai szerepkör tartozik hitelesítő biztosíthat hozzájárulási; mások egy hibaüzenetet fog kapni. A sikeres hozzájárulási hello válasz admin_consent fogja tartalmazni = true. Olyan hozzáférési jogkivonatot váltja be, amikor is kap egy, amely tájékoztatást fogunk adni hello szervezet id_token és hello rendszergazdáját iratkozott fel az alkalmazáshoz.

### <a name="enabling-oauth-20-implicit-grant-for-single-page-applications"></a>Engedélyezése OAuth 2.0 implicit adja meg egy lap alkalmazások
Egyetlen lap alkalmazás (gyógyfürdők) általában felépítése hello alkalmazás webes API-k háttér tooperform meghívja az üzleti logika hello böngészőjében futó JavaScript-gyakori első vége. Az Azure ad-ben üzemeltetett gyógyfürdők, az OAuth 2.0 Implicit Grant tooauthenticate hello felhasználó használja az Azure ad-val, és használható jogkivonat beszerzése toosecure hívásait hello alkalmazás JavaScript ügyfél tooits vissza end webes API. Hello felhasználói hozzájárulás megadta, miután a hitelesítési protokollnak használt tooobtain jogkivonatok toosecure hívások hello ügyfél és a más hello alkalmazáshoz konfigurált webes API-erőforrások között lehet. További információ az hello implicit hitelesítésengedélyezési, és úgy dönt, hogy az alkalmazás helyzetnek megfelelő súgó toolearn lásd [ismertetése hello OAuth2 implicit adja meg az Azure Active Directoryban folyamat ](active-directory-dev-understanding-oauth2-implicit-grant.md).

OAuth 2.0 típusú implicit Grant alkalmazások alapértelmezés szerint le van tiltva. Engedélyezheti az alkalmazás OAuth 2.0 Implicit Grant hello beállítása `oauth2AllowImplicitFlow` értéket a [alkalmazásjegyzék](active-directory-application-manifest.md), vagyis a JSON-fájl, amely az alkalmazáskonfiguráció identitásának jelöli.

#### <a name="tooenable-oauth-20-implicit-grant"></a>tooenable OAuth 2.0 típusú implicit támogatás
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Válassza ki az Azure AD-bérlő fiókja hello jobb felső sarkában hello lap választásával.
3. Hello felső menüben válassza a **Azure Active Directory**, kattintson a **App regisztrációk**, majd kattintson a kívánt tooconfigure hello alkalmazás. Ez fogja toohello alkalmazás gyors üzembe helyezés lap léphet, valamint hello alkalmazás hello-beállítások panel megnyitása.
4. Hello alkalmazás lapon kattintson **Manifest** tooopen hello beágyazott jegyzék szerkesztő.
   Keresse meg és túl "true" hello "oauth2AllowImplicitFlow" érték beállítása. Alapértelmezés szerint értéke "false".
   
    `"oauth2AllowImplicitFlow": true,`
5. Frissített hello jegyzékfájl mentése. Miután menti, a webes API konfigurálva toouse OAuth 2.0 Implicit Grant tooauthenticate felhasználók.


## <a name="removing-an-application"></a>Az alkalmazás eltávolítása
Ez a szakasz ismerteti, hogyan tooremove olyan alkalmazást az Azure AD bérlői.

### <a name="removing-an-application-authored-by-your-organization"></a>A szervezet által létrehozott alkalmazás eltávolítása
Ezek a hello alkalmazások, amelyek megjelenítik a hello főoldalon "Alkalmazás" hello "Alkalmazások a vállalat tulajdonosa" szűrő az Azure AD-bérlő. Technikai szempontból ezeket manuálisan hello a klasszikus Azure portálon keresztül, vagy programozott módon PowerShell regisztrált alkalmazások vagy hello Graph API-val. Pontosabban, azok vannak objektum által jelképezett mindkét egy alkalmazás és egyszerű szolgáltatás az Ön bérelt szolgáltatásának. Lásd: [alkalmazás és szolgáltatás egyszerű objektumok](active-directory-application-objects.md) további információt.

#### <a name="tooremove-a-single-tenant-application-from-your-directory"></a>a címtárban lévő egy egybérlős alkalmazás tooremove
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Válassza ki az Azure AD-bérlő fiókja hello jobb felső sarkában hello lap választásával.
3. Hello felső menüben válassza a **Azure Active Directory**, kattintson a **App regisztrációk**, majd kattintson a kívánt tooconfigure hello alkalmazás. Ez fogja toohello alkalmazás gyors üzembe helyezés lap léphet, valamint hello alkalmazás hello-beállítások panel megnyitása.
4. Hello alkalmazás lapon kattintson **törlése**.
5. Kattintson a **Igen** hello megerősítő üzenet.

#### <a name="tooremove-a-multi-tenant-application-from-your-directory"></a>egy több-bérlős alkalmazást a címtárban lévő tooremove
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Válassza ki az Azure AD-bérlő fiókja hello jobb felső sarkában hello lap választásával.
3. Hello felső menüben válassza a **Azure Active Directory**, kattintson a **App regisztrációk**, majd kattintson a kívánt tooconfigure hello alkalmazás. Ez fogja toohello alkalmazás gyors üzembe helyezés lap léphet, valamint hello alkalmazás hello-beállítások panel megnyitása.
4. Hello-beállítások panelen válassza a **tulajdonságok** és tükrözés hello **Multi-központjaként** túl kapcsoló**nem**. Az alkalmazás toobe egybérlős alakítja, de egy szervezet, aki már hozzájárult tooit hello alkalmazás továbbra is megmarad.
5. Kattintson a hello **törlése** hello alkalmazás oldalról gombra.
6. Kattintson a **Igen** hello megerősítő üzenet.

### <a name="removing-a-multi-tenant-application-authorized-by-another-organization"></a>Egy másik szervezet által felhatalmazott több-bérlős alkalmazás eltávolítása
Ezek a hello alkalmazások, amelyek megjelenítik a hello hello fő "alkalmazás" lap "Alkalmazások a vállalat által használt" szűrőt az Azure AD-bérlőn, kifejezetten hello hello "Alkalmazások a vállalat tulajdonosa" listán nem szereplő gazdarendszerhez egy részét. Technikai szempontból olyan több-bérlős alkalmazásokhoz regisztrált hello hozzájárulási folyamat során. Pontosabban, azok vannak objektum által jelképezett csak egy egyszerű szolgáltatásnév az Ön bérelt szolgáltatásának. Lásd: [alkalmazás és szolgáltatás egyszerű objektumok](active-directory-application-objects.md) további információt.

A rendezés tooremove egy több-bérlős alkalmazás hozzáférés tooyour könyvtárába (után hozzájárulási megadó), hello vállalati rendszergazda hozzá kell férnie egy Azure-előfizetés tooremove hello Azure-portálon keresztül. Hello vállalati rendszergazda azonban használhat hello [Azure AD PowerShell-parancsmagok](http://go.microsoft.com/fwlink/?LinkId=294151) tooremove hozzáférést.

## <a name="next-steps"></a>Következő lépések
* Lásd: hello [Branding irányelveket az integrált alkalmazások](active-directory-branding-guidelines.md) kapcsolatos tippek az alkalmazások visual útmutatást.
* Az alkalmazás alkalmazás- és szolgáltatásnevet objektumok hello kapcsolatát a további részletekért lásd: [alkalmazás és szolgáltatás egyszerű objektumok](active-directory-application-objects.md).
* toolearn hello szerepkör hello alkalmazás jegyzékének betöltött szerepe, kapcsolatos további információkért lásd: [ismertetése hello Azure Active Directory alkalmazásjegyzékének](active-directory-application-manifest.md)
* Lásd: hello [az Azure AD fejlesztői szószedet](active-directory-dev-glossary.md) néhány hello core Azure Active Directory (AD) fejlesztői fogalmak definícióját.
* A Microsoft hello [Active Directory fejlesztői útmutatója](active-directory-developers-guide.md) összes fejlesztői áttekintést kapcsolódó tartalmat.

