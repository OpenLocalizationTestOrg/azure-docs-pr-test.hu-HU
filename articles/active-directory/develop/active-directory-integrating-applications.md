---
title: "Alkalmazások integrálása az Azure Active Directory |} Microsoft Docs"
description: "Megtudhatja, hogyan hozzáadása, frissítése, vagy távolítsa el a kérelmet az Azure Active Directory (Azure AD)."
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
ms.openlocfilehash: 3be341bcb897a1481f145825429a1a94dfaae3b0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="integrating-applications-with-azure-active-directory"></a>Alkalmazások integrálása az Azure Active Directoryban
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Vállalati fejlesztők és szoftverszolgáltatási (SaaS) szolgáltatók kifejleszthetnek az Azure Active Directory-be (Azure AD-ba) integrálható kereskedelmi felhőszolgáltatásokat és üzletági alkalmazásokat, hogy a szolgáltatásaik számára biztonságos bejelentkezést és engedélyezést biztosítsanak. Az alkalmazás vagy szolgáltatás integrálása az Azure AD, a fejlesztő először regisztrálnia kell az alkalmazás adatait az Azure ad-vel a klasszikus Azure portálon keresztül.

Ez a cikk bemutatja, hogyan hozzáadása, módosítása vagy az Azure AD alkalmazás eltávolítása. Megtudhatja, hogyan integrálható az Azure Active Directory, az alkalmazások különböző típusú konfigurálása az alkalmazások, például a webes API-k és még sok más egyéb erőforrásainak elérésére.

A két Azure AD-objektumok, amelyek megfelelnek a regisztrált alkalmazáshoz és a kapcsolat kapcsolatos további információkért lásd: [alkalmazás és szolgáltatás egyszerű objektumok](active-directory-application-objects.md); további információt a arculati útmutatóját, akkor az Azure Active Directoryval alkalmazások fejlesztése során használandó [Branding irányelveket az integrált alkalmazások](active-directory-branding-guidelines.md).

## <a name="adding-an-application"></a>Egy alkalmazás hozzáadása
Bármely alkalmazás, amely szeretné használni az Azure AD képességeit, előbb regisztrálni kell az Azure AD-bérlő. A regisztrációs folyamat során jogosultságot ad az Azure AD adatait az alkalmazás, például az URL-címet, akkor rendelkezik helyét, az URL-cím küldéséhez a válaszokat a felhasználó hitelesítése után az URI, amely azonosítja az alkalmazást, és így tovább.

Ha egy webes alkalmazás, amely csak támogatnia kell a bejelentkezéshez a felhasználók számára az Azure ad-ben van felépítése, akkor egyszerűen kövesse az alábbi utasításokat. Ha az alkalmazás van szüksége a hitelesítő adatok vagy az engedélyeket, hogy egy webes API-hoz, vagy a felhasználók más Azure AD-bérlőt szeretne-e férni, olvassa el a szükséges [frissíteni az alkalmazást](#updating-an-application) szakasz folytathatja az alkalmazás konfigurálását.

### <a name="to-register-a-new-application-in-the-azure-portal"></a>Egy új alkalmazást regisztrálni Azure-portálon
1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
2. Válassza ki az Azure AD-bérlő fiókja kiválasztja az oldal jobb felső sarkában.
3. A bal oldali navigációs ablaktábláján válassza **több szolgáltatások**, kattintson a **App regisztrációk**, és kattintson a **hozzáadása**.
4. Kövesse az utasításokat az új alkalmazás létrehozásához. Ha szeretné, hogy a webes alkalmazások és natív alkalmazások példák, tekintse meg a [quickstarts](active-directory-developers-guide.md).
  * A webes alkalmazásokhoz, adja meg a **bejelentkezési URL-cím**, az alap URL-CÍMÉT az alkalmazásához. Ez az adott felhasználó tud egyszerre bejelentkezni például `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * Natív alkalmazások esetén adja meg a **átirányítási URI-**, az Azure AD vissza a token válaszok használja. Adjon meg egy, az alkalmazáshoz tartozó értéket, például: `http://MyFirstAADApp`
5. Miután végrehajtotta a regisztráció, az Azure AD rendeli hozzá az alkalmazás egyedi ügyfél-azonosítókra, az alkalmazás azonosítóját. Az alkalmazás hozzá van adva, és megnyílik a gyors üzembe helyezés lap az alkalmazáshoz. Attól függően, hogy az alkalmazás egy webes vagy natív alkalmazás a további funkciók hozzáadása az alkalmazás különböző parancsot érhet el. Az alkalmazás hozzáadása után megkezdheti az alkalmazást az engedélyezése a felhasználóknak a bejelentkezéshez, hozzáférési webes API-k más alkalmazásokban, vagy állítson be több-bérlős (amely lehetővé teszi az alkalmazás eléréséhez más szervezetek).

> [!NOTE]
> Alapértelmezés szerint az újonnan létrehozott alkalmazás regisztrálása a felhasználók a címtárban lévő jelentkezzen be az alkalmazás van konfigurálva.
> 
> 

## <a name="updating-an-application"></a>Egy alkalmazás frissítése
Miután az alkalmazás regisztrálva van az Azure ad-vel, kell frissíteni, hogy a webes API-k eléréséhez, elérhetik más szervezetek és még sok más. Ez a szakasz ismerteti a különböző módokon, ahol konfigurálhatja az alkalmazás további szeretne. Először azt indul el a hozzájárulási keretrendszer áttekintést amely fontos tisztában lenni azzal, ha Ön által létrehozott erőforrás/API-alkalmazások, amelyeket a fejlesztők a szervezet vagy egy másik szervezet beépített ügyfélalkalmazások fogja használni.

További információt a módon authentication úgy működik, az Azure ad-ben, lásd: [hitelesítési forgatókönyvek az Azure AD](active-directory-authentication-scenarios.md).

### <a name="overview-of-the-consent-framework"></a>A hozzájárulási keretrendszer áttekintése
Az Azure AD hozzájárulási keretrendszer megkönnyíti a több-bérlős web- és webes API-k, az Azure AD-bérlő, különböző azzal, ha regisztrálva van-e az ügyfélalkalmazás által védett elérését igénylő natív ügyfél-alkalmazások fejlesztéséhez. A webes API-k tartalmazza a Microsoft Graph API (eléréséhez Azure Active Directory, az Intune és a szolgáltatások az Office 365-ben) és más Microsoft-szolgáltatásokban API-k, a saját webes API-k mellett. A keretrendszer egy felhasználó vagy a rendszergazda jogosultságot ad a beleegyezést kérő regisztrálni kell a könyvtárban, amelyek magukban foglalhatják directory adatokhoz hozzáférő alkalmazáshoz való alapul.

Például ha egy ügyfél webalkalmazás rendszernek végig kell olvasnia a felhasználóval kapcsolatos adatok az Office 365 szolgáltatásból, hogy a felhasználó le kell hozzájárul az ügyfélalkalmazást. Miután hozzájárulási kap, az ügyfélalkalmazás fog tudni hívja fel a Microsoft Graph API a felhasználó nevében, és a naptári adatok használatára, szükség szerint. A [Microsoft Graph API](https://graph.microsoft.io) hozzáférést biztosít az adatok az Office 365-ben (például a naptárak és az Exchange, helyek és a SharePoint, a dokumentumot a onedrive-ról a OneNote-ba, a tervező számára, Excel munkafüzetekhez feladatok notebookok listák üzenetek stb.), valamint felhasználók és csoportok az Azure AD és más adatok több Microsoft felhőszolgáltatás objektumokat. 

A hozzájárulási keretrendszer épül OAuth 2.0 és a különböző adatfolyamok, például engedélyezési code grant és az ügyfél hitelesítő adatai megadják, nyilvános vagy titkos ügyfél használatával. OAuth 2.0 használatával az Azure AD lehetővé teszi számos különböző típusú ügyfélalkalmazások, például a telefon, táblagép, kiszolgáló vagy egy webalkalmazás létrehozása és a szükséges erőforrások eléréséhez.

Részletesebb információ a hozzájárulási keretében, lásd: [OAuth 2.0, az Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx), [hitelesítési forgatókönyvek az Azure AD](active-directory-authentication-scenarios.md), és az Office 365 keresztül engedélyezett hozzáféréssel kapcsolatban A Microsoft Graph, lásd: [App hitelesítést a Microsoft Graph](https://graph.microsoft.io/docs/authorization/auth_overview).

#### <a name="example-of-the-consent-experience"></a>A hozzájárulási élmény példája
Az alábbi lépéseket bemutatja, hogyan a hozzájárulási tapasztalhat az alkalmazás fejlesztőjének és a felhasználó is működik.

1. A webes ügyfél alkalmazása konfigurálólapját az Azure portálon állítsa be a az alkalmazás által igényelt, a szükséges engedélyek szakaszában a menük használatával.
   
    ![Egyéb alkalmazások engedélyei](./media/active-directory-integrating-applications/requiredpermissions.png)
2. Vegye figyelembe, hogy az Alkalmazásengedélyek frissítve lett, az alkalmazás fut, és egy felhasználó először a használni kívánt. Ha az alkalmazás már nem megszerezte az access vagy a frissítési token, az alkalmazást kell jogkivonat beszerzése az engedélyezési kód, amely segítségével szerezzen be egy új hozzáférést, és frissítse az Azure AD engedélyezési végpont Ugrás.
3. Nem már hitelesíteni a felhasználót, ha azok a kell adnia az Azure ad Szolgáltatásba bejelentkezni.
   
    ![Felhasználói vagy rendszergazdai bejelentkezés az Azure AD](./media/active-directory-integrating-applications/usersignin.png)
4. Miután a felhasználó bejelentkezett, az Azure AD határozza meg a felhasználónak kell jeleníthető meg a hozzájárulási lap. Ez a döntés alapján e a felhasználó (vagy annak szervezeti rendszergazdája) már megadta a kérelem jóváhagyását. Hozzájárulás már nincs megadva, ha az Azure AD kérni fogja a felhasználótól a, és megjeleníti a működéséhez szükséges szükséges engedélyekkel. A hozzájárulási párbeszédpanelen megjelenő engedélykészletüket ugyanazok, mint mi választotta, az Azure-portálon delegált engedélyek.
   
    ![Felhasználói hozzájárulás élmény](./media/active-directory-integrating-applications/consent.png)
5. Miután a felhasználó engedélyezi a hozzájárulási, egy engedélyezési kód az alkalmazás, amely is váltható szerezzen be egy hozzáférési jogkivonatot, és frissítse a jogkivonatot küld vissza. Ez a folyamat kapcsolatos további információkért tekintse meg a [webes API szakasz webes alkalmazás](active-directory-authentication-scenarios.md#web-application-to-web-api) szakasz [hitelesítési forgatókönyvek az Azure AD](active-directory-authentication-scenarios.md).

6. A rendszergazdák is is beleegyezik a delegált engedélyek egy alkalmazás a felhasználók nevében az Ön bérelt szolgáltatásának. Ez megakadályozza, hogy a hozzájárulási párbeszédpanel a bérlő minden felhasználója esetében nem. Az ehhez a [Azure-portálon](https://portal.azure.com) az alkalmazás oldalát a. Az a **beállítások** az alkalmazás paneljén kattintson **szükséges engedélyek** , majd kattintson a a **engedélyt adjon** gombra. 

    ![Adja meg explicit admin szerezni a hozzájárulásukat engedélyeket](./media/active-directory-integrating-applications/grantpermissions.png)
    
> [!NOTE]
> Explicit megadását hozzájárulás használatával a **engedélyt adjon** gombra kell jelenleg használó ADAL.js, egylapos alkalmazások (SPA), a hozzáférési jogkivonat beleegyezést kérő üzenete, ami sikertelen lesz, ha a hozzájárulási nincs nélkül van szükség. már megadott.   

### <a name="configuring-a-client-application-to-access-web-apis"></a>A webes API-k elérésére ügyfélalkalmazás konfigurálása
Ahhoz, hogy részt vesz egy engedélyezési grant flow, amelyhez hitelesítés szükséges a (és egy hozzáférési jogkivonat beszerzése) webes bizalmas ügyfélalkalmazást azt kell létesítenie a biztonságos hitelesítő adatok. Az alapértelmezett hitelesítési módszer az Azure portál által támogatott ügyfél-azonosító + szimmetrikus kulcs. Ebben a szakaszban foglalkozunk a konfigurációs lépéseket kell adnia a titkos kulcsot az ügyfél hitelesítő adatait.

Emellett egy ügyfél előtti hozzáférés a webes API által elérhetővé tett egy erőforrás-alkalmazáshoz (ie: Microsoft Graph API), a hozzájárulási keretrendszer biztosíthatja, hogy az ügyfél beolvassa az engedély megadása szükséges, a kért engedélyek alapján. Alapértelmezés szerint minden alkalmazás engedélyek közül választhatnak Azure Active Directory (Graph API-val) és az Azure szolgáltatásfelügyeleti API, alapértelmezés szerint kiválasztva az Azure AD "Enable bejelentkezés és olvasási profil" engedéllyel rendelkező. Ha az ügyfélalkalmazást Azure AD az Office 365-bérlő regisztrálva van folyamatban, a webes API-k és a SharePoint és Exchange online-hoz is kell kijelölésnél érhetők el. Választhat [engedélyek két típusú](active-directory-dev-glossary.md#permissions) a legördülő menükben mellett a kívánt webes API-t:

* Alkalmazásengedélyek: Az ügyfélalkalmazást hozzá kell férniük a web API közvetlenül saját magát (felhasználói környezet). Ez a típus engedély rendszergazda jóváhagyását igényli, és nem is érhető el natív ügyfél-alkalmazások.
* Delegált engedélyek: Az ügyfélalkalmazást hozzá kell férniük a webes API-t a bejelentkezett felhasználó nevében, de az elérés korlátozója a kijelölt engedély. Az ilyen típusú engedély által biztosított a felhasználó kivéve, ha az engedély a rendszergazda jóváhagyását igénylő van konfigurálva. 

> [!NOTE]
> Delegált engedélyek hozzáadása egy alkalmazáshoz nem automatikusan hozzájárulását belül a bérlő számára, a klasszikus Azure portálon. A felhasználói hozzájárulás futásidőben, a hozzáadott delegált jogosultságokkal az továbbra is futtathatja manuálisan kell, kivéve, ha a rendszergazda kattint a **engedélyt adjon** gombot a **szükséges engedélyek** szakasza a alkalmazások lap az Azure portálon. 

#### <a name="to-add-credentials-or-permissions-to-access-web-apis"></a>Hitelesítő adatok, vagy engedélyeket az eléréséhez a webes API-k hozzáadása
1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
2. Válassza ki az Azure AD-bérlő fiókja kiválasztja az oldal jobb felső sarkában.
3. A felső menüben válassza a **Azure Active Directory**, kattintson a **App regisztrációk**, majd kattintson a konfigurálni kívánt alkalmazás. Ez lesz az alkalmazás gyors üzembe helyezés oldalt, valamint nyissa meg a beállítások panelről az alkalmazás.
4. Egy titkos kulcsot a webes alkalmazás hitelesítő adatainak hozzáadásához kattintson a "Kulcsok" szakaszban, a beállítások panelen.  
   
   * Adjon meg egy leírást a kulcshoz, és válassza a 1 vagy 2 év időtartama. 
   * A jobb szélső oszlop lesz a kulcs értékét, a konfigurációs módosítások mentése után. Győződjön meg arról, térjen vissza ehhez a szakaszhoz, és másolásához kattintson a mentés után, hogy azt használatra az ügyfél alkalmazás futásidőben a hitelesítés során.
5. Az ügyfél erőforrás API-k elérésére engedélyt hozzáadásához kattintson a "Szükséges engedélyek" szakaszban, a beállítások panelen. 
   
   * Először kattintson a "Hozzáadás" gombra.
   * Kattintson a "Jelölje ki az API" Válassza ki a kívánt erőforrások típusának kiválasztása.
   * Tallózza végig elérhető API-kat a listáját, vagy használja a keresőmezőt alkalmazásokból az elérhető erőforrás a könyvtárban, amelyek teszi közzé a webes API-k kiválasztásához. Kattintson az erőforrás által szüksége, majd kattintson a **válasszon**.
   * A kijelölt áthelyezheti a **Select engedélyeket** menü, ahol igény szerint a "Alkalmazásengedélyek" és "Delegált engedélyek" az alkalmazás.
   
6. Ha elkészült, kattintson a **végzett** gombra.

> [!NOTE]
> Kattintson a **végzett** gombra is automatikusan beállítja az engedélyeket az alkalmazáshoz beállított egyéb alkalmazások engedélyei alapján a könyvtárban.  Ezek az Alkalmazásengedélyek tekintheti meg az alkalmazás megnézi **beállítások** panelen.
> 
> 

### <a name="configuring-a-resource-application-to-expose-web-apis"></a>Egy erőforrás-alkalmazások – ezek általában a webes API-k konfigurálása
Webes API-k fejlesztése, és tegye elérhetővé számára jelentkezik, mintha a hozzáférési [hatókörök](active-directory-dev-glossary.md#scopes) és [szerepkörök](active-directory-dev-glossary.md#roles). Egy megfelelően konfigurált webes API-t szeretné elérhetővé tenni hasonlóan a más Microsoft webes API-k, beleértve a Graph API-t és az Office 365 API-kat. A hozzáférési hatókörök és szerepkörök keresztül közzétett a [alkalmazás jegyzékfájlja](active-directory-dev-glossary.md#application-manifest), vagyis a JSON-fájl, amely az alkalmazáskonfiguráció identitásának jelöli.  

A következő szakasz bemutatja, hogyan teszi közzé a hozzáférési hatókörök, az erőforrás alkalmazás jegyzékfájlja módosításával.

#### <a name="adding-access-scopes-to-your-resource-application"></a>Hozzáférési hatókörök hozzáadása az erőforrás alkalmazás
1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
2. Válassza ki az Azure AD-bérlő fiókja kiválasztja az oldal jobb felső sarkában.
3. A felső menüben válassza a **Azure Active Directory**, kattintson a **App regisztrációk**, majd kattintson a konfigurálni kívánt alkalmazás. Ez lesz az alkalmazás gyors üzembe helyezés oldalt, valamint nyissa meg a beállítások panelről az alkalmazás.
4. Kattintson a **Manifest** az alkalmazás oldalról a jegyzék beágyazott-szerkesztő megnyitásához. 
5. A következő kódtöredékre JSON cserélje le a "oauth2Permissions" csomópont. Ezt a kódrészletet példa bemutatja, hogyan teszi közzé a hatókör "felhasználó megszemélyesítése", amely lehetővé teszi az erőforrás tulajdonosa erőforrás ügyfélalkalmazás olyan típusú delegált hozzáférést biztosítanak néven. Győződjön meg arról, hogy módosítja a szöveg és az értékeket a saját alkalmazáshoz:
   
        "oauth2Permissions": [
        {
            "adminConsentDescription": "Allow the application full access to the Todo List service on behalf of the signed-in     user",
            "adminConsentDisplayName": "Have full access to the Todo List service",
            "id": "b69ee3c9-c40d-4f2a-ac80-961cd1534e40",
            "isEnabled": true,
            "type": "User",
            "userConsentDescription": "Allow the application full access to the todo service on your behalf",
            "userConsentDisplayName": "Have full access to the todo service",
            "value": "user_impersonation"
            }
        ],
   
    Az azonosító értéknek kell lennie egy új előállított GUID Azonosítóhoz, amelyek használatával hoz létre egy [GUID generációs eszköz](https://msdn.microsoft.com/library/ms241442%28v=vs.80%29.aspx) vagy programon keresztül. Ezt az egyedi azonosítója az engedély, amely a webes API-t tesz elérhetővé. Miután az ügyfél konfigurálása megfelelő kérjen hozzáférést a webes API-t és a hívások a webes API-t, az OAuth 2.0 JWT jogkivonat, amely rendelkezik a fentiek értékre, amely ebben az esetben user_impersonation hatókör (scp) jogcím jelent.
   
   > [!NOTE]
   > További hatókörökkel később szükség szerint is elérhetővé teheti. Vegye figyelembe, hogy a webes API számos különböző funkcióihoz tartozó több hatókör esetleg felfedi. Most már a hatókör (scp) jogcímek használatát a kapott OAuth 2.0 JWT jogkivonat szabályozhatja a webes API-k eléréséhez.
   > 
   > 
6. Kattintson a **mentése** a jegyzékfájl mentése. A webes API konfigurálva van a címtárban lévő más alkalmazásokkal használható.

#### <a name="to-verify-the-web-api-is-exposed-to-other-applications-in-your-directory"></a>Ellenőrizheti a webes API van közzétéve, hogy a címtárban lévő más alkalmazásokkal
1. Kattintson a felső menüben **App regisztrációk**, válassza ki a kívánt ügyfélalkalmazás konfigurálása a hozzáférés a webes API-hoz, és navigáljon a beállítások panelről.
2. Az a **szükséges engedélyek** területen válassza ki a webes API-t, amely közvetlenül elérhetővé tett engedély. A delegált engedélyek legördülő menüben válassza ki az új engedély.

![Teendőlista engedélyek láthatók.](./media/active-directory-integrating-applications/todolistpermissions.png)

#### <a name="more-on-the-application-manifest"></a>További információk az az alkalmazás jegyzékében
Az alkalmazás jegyzékében ténylegesen a frissítés az alkalmazás entitás, amely meghatározza egy Azure AD-alkalmazást identitás konfiguráció – beleértve az API a hozzáférési hatókörök tárgyalt összes attribútumát egy mechanizmusként funkcionál. Az alkalmazás entitáson további információkért lásd: a [Graph API-alkalmazás entitás dokumentáció](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity). Az oktatóanyagban megtalálja az alkalmazás entitástagok az API-engedélyek megadásához használja a teljes referenciája:  

* a appRoles számára, amely gyűjtemény a [birtokolhat](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type) entitások meghatározásához használható a **Alkalmazásengedélyek** a webes API-k  
* a oauth2Permissions számára, amely gyűjtemény a [OAuth2Permission](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type) entitások meghatározásához használható a **delegált engedélyek** a webes API-k

További információ az alkalmazás jegyzékének fogalmak általában tekintse meg [az Azure Active Directory alkalmazásjegyzékének megismerése](active-directory-application-manifest.md).

### <a name="accessing-the-azure-ad-graph-and-office-365-via-microsoft-graph-apis"></a>Az Azure AD Graph és az Office 365 keresztül Microsoft Graph API-k eléréséhez  
A korábbiak mellett az ilyen/elérését API-k azokon a saját erőforrás-alkalmazást, az ügyfélalkalmazás által Microsoft erőforrások API-k elérésére is frissítheti.  A Microsoft Graph API, melynek neve "Microsoft Graph" engedélyek más alkalmazásoknak listájában, érhető el vagy az Azure ad-vel regisztrált összes alkalmazást. Ha az ügyfélalkalmazást Azure AD-bérlő, amely az Office 365 lett kiépítve a regisztrál, emellett különböző Office 365-erőforrásokhoz, a Microsoft Graph API által elérhetővé tett engedélyeit.

A Microsoft Graph API által a hozzáférési hatókörök teljes leírása, lásd: a [engedélyhatókörök |} Microsoft Graph API-val kapcsolatos fogalmak](https://graph.microsoft.io/docs/authorization/permission_scopes) cikk.

> [!NOTE]
> A jelenlegi korlátozás miatt natív ügyfélalkalmazások csak hívhatják meg azokat az Azure AD Graph API ha azok a "Hozzáférés a munkahely címtárában" engedéllyel.  Ez a korlátozás vonatkozik webes alkalmazásokhoz.
> 
> 

### <a name="configuring-multi-tenant-applications"></a>Több-bérlős alkalmazások konfigurálása
Amikor egy alkalmazás az Azure AD hozzá, érdemes lehet az alkalmazás számára elérhetők, csak a szervezet felhasználói. Az alkalmazás a felhasználók a külső szervezetek elérni, érdemes lehet. A két alkalmazás nevezik egybérlős és a több-bérlős alkalmazásokhoz. Módosíthatja a konfiguráció egybérlős alkalmazás abba, hogy egy több-bérlős alkalmazást, amely alatt ez a szakasz ismerteti.

Fontos megjegyzés: egy bérlő egyetlen és több-bérlős alkalmazás közötti különbségek:  

* Egy bérlő egyetlen alkalmazás használatát az egyik szervezet szól. Általában egy-üzletági (LoB) alkalmazás egy vállalati fejlesztők által írt. Egy bérlő egyetlen alkalmazást csak kell egy címtárban a felhasználók ne férhessenek hozzá, ezért az ennek eredményeként, csak úgy kell létrehozni egy címtárban.
* Egy több-bérlős alkalmazás számos szervezet számára készült. A szoftver,--szolgáltatás (SaaS) webalkalmazás általában egy független szoftverszállító (ISV) által írt. Több-bérlős alkalmazásokhoz ki kell építeni minden könyvtárban ahol használni fogják őket, amelyhez a felhasználó vagy rendszergazda hozzájárul regisztrálja őket az Azure AD hozzájárulási keretrendszer használatával támogatott. Ne feledje, hogy minden natív ügyfélalkalmazások több-bérlős alapértelmezés szerint az erőforrás tulajdonosa eszközön telepítettként. Az áttekintésben a keretrendszer hozzájárulás szakasz fenti további részleteket a hozzájárulási keretrendszerre.

#### <a name="enabling-external-users-to-grant-your-application-access-to-their-resources"></a>Az alkalmazás hozzáférést biztosít az erőforrásaikat külső felhasználók engedélyezése
Ha egy alkalmazás, ha az ügyfelekre és partnerekre a szervezeten kívül szeretné elérhetővé tenni kívánt, szüksége lesz az Azure-portálon az alkalmazás definíciójának frissítése.

> [!NOTE]
> Ha engedélyezve van a több-bérlős, győződjön meg róla, hogy az alkalmazás App ID URI egy ellenőrzött tartomány tartozik. A visszatérési URL-emellett https:// kell kezdődnie. További információkért lásd: [alkalmazás és szolgáltatás egyszerű objektumok](active-directory-application-objects.md).
> 
> 

Ahhoz, hogy a külső felhasználók számára az alkalmazásához való hozzáférést: 

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
2. Válassza ki az Azure AD-bérlő fiókja kiválasztja az oldal jobb felső sarkában.
3. A felső menüben válassza a **Azure Active Directory**, kattintson a **App regisztrációk**, majd kattintson a konfigurálni kívánt alkalmazás. Ez lesz az alkalmazás gyors üzembe helyezés oldalt, valamint nyissa meg a beállítások panelről az alkalmazás.
4. A beállítási paneljén kattintson az **tulajdonságok** tükrözés és a **Multi-központjaként** váltani **Igen**.

Miután végzett a fenti módosítás, felhasználók és más vállalatok rendszergazdái tudják az alkalmazás-hozzáférést biztosít a címtár- és egyéb adatok.

#### <a name="triggering-the-azure-ad-consent-framework-at-runtime"></a>Az Azure AD hozzájárulási keretrendszer futásidőben időt.
A hozzájárulási keretrendszer használatához a több-bérlős ügyfélalkalmazások engedélyezési OAuth 2.0 használatával kell igényelnie. [Kódminták](https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multi-tenant) elérhető mutatjuk be, hogyan egy webes alkalmazás, a natív alkalmazás vagy a kiszolgáló/démon alkalmazás kérelmek hitelesítési kódok és a hozzáférési jogkivonatok webes API-k meghívásához.

A webes alkalmazás is kínálhat bejelentkezési élmény a felhasználók számára. Ha egy olyan regisztrációs felület, várható, hogy a felhasználó fog kattintson a gombra egy bejelentkezési, hogy a böngésző számára az Azure AD OAuth2.0 átirányítási engedélyeztetéséhez használandó végpont vagy OpenID Connect userinfo végpontot. Itt engedélyezheti az alkalmazásnak az új felhasználó információt lekérdezni a id_token vizsgálatával ezeket a végpontokat. A bejelentkezési fázis követően a felhasználó fog jelenik meg beleegyezést kérő üzenete hasonló a keretrendszer hozzájárulás szakaszban áttekintést a fent látható.

Azt is megteheti a webes alkalmazás is kínálhat, amely lehetővé teszi a rendszergazdák számára, hogy "a vállalati regisztráció" élményt. Ez a felület akkor is átirányítja a felhasználót az Azure AD OAuth 2.0 végpont hitelesítéséhez. Ebben az esetben azonban adja meg a kérdés = admin_consent paraméter, a rendszergazda jóváhagyását élményt, ha a rendszergazda megadja a szervezet nevében hozzájárulási kényszerítheti a hitelesítési végpontra. Csak egy felhasználó egy olyan fiókkal, amely a globális rendszergazdai szerepkör tartozik hitelesítő biztosíthat hozzájárulási; mások egy hibaüzenetet fog kapni. A sikeres jóváhagyását, a válasz tartalmazza admin_consent = true. Olyan hozzáférési jogkivonatot váltja be, amikor is kap egy id_token, amely tájékoztatást nyújtanak a szervezet és a rendszergazda, aki iratkozott fel az alkalmazáshoz.

### <a name="enabling-oauth-20-implicit-grant-for-single-page-applications"></a>Engedélyezése OAuth 2.0 implicit adja meg egy lap alkalmazások
Egyetlen lap alkalmazás (gyógyfürdők) általában felépítése a JavaScript-gyakori előtér futtató a böngészőben, amely az alkalmazás webes API-k visszahívja end üzleti logikája végrehajtásához. Az Azure ad-ben üzemeltetett gyógyfürdők, az OAuth 2.0 Implicit Grant hitelesíteni a felhasználót az Azure AD és használhat, amely segítségével biztonságos hívásokat az alkalmazás JavaScript-ügyfél és a háttérben futó webes API jogkivonat beszerzése. Miután a felhasználó számára engedélyezett a hozzájárulási, a hitelesítési protokollnak használható az alkalmazáshoz konfigurált API erőforrások biztonságba helyezése az ügyfél és a más webkiszolgáló közötti hívások-jogkivonat beszerzése. További tudnivalók az implicit hitelesítésengedélyezési, és segít eldönteni, hogy az alkalmazás forgatókönyv szerint jobb [OAuth2 implicit ismertetése adja meg az Azure Active Directoryban folyamat ](active-directory-dev-understanding-oauth2-implicit-grant.md).

OAuth 2.0 típusú implicit Grant alkalmazások alapértelmezés szerint le van tiltva. Úgy, hogy az alkalmazás OAuth 2.0 Implicit Grant is engedélyeznie a `oauth2AllowImplicitFlow` értéket a [alkalmazásjegyzék](active-directory-application-manifest.md), vagyis a JSON-fájl, amely az alkalmazáskonfiguráció identitásának jelöli.

#### <a name="to-enable-oauth-20-implicit-grant"></a>OAuth 2.0 típusú implicit támogatás engedélyezése
1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
2. Válassza ki az Azure AD-bérlő fiókja kiválasztja az oldal jobb felső sarkában.
3. A felső menüben válassza a **Azure Active Directory**, kattintson a **App regisztrációk**, majd kattintson a konfigurálni kívánt alkalmazás. Ez lesz az alkalmazás gyors üzembe helyezés oldalt, valamint nyissa meg a beállítások panelről az alkalmazás.
4. Az alkalmazás lapon kattintson a **Manifest** a jegyzék beágyazott-szerkesztő megnyitásához.
   Keresse meg és "oauth2AllowImplicitFlow" értéke "true"értékre állítható be. Alapértelmezés szerint értéke "false".
   
    `"oauth2AllowImplicitFlow": true,`
5. A frissített jegyzékfájl mentése. Miután menti, a webes API konfigurálva van az OAuth 2.0 Implicit Grant segítségével hitelesíti a felhasználókat.


## <a name="removing-an-application"></a>Az alkalmazás eltávolítása
Ez a szakasz ismerteti az Azure AD-bérlőről egy alkalmazás eltávolítása.

### <a name="removing-an-application-authored-by-your-organization"></a>A szervezet által létrehozott alkalmazás eltávolítása
Ezek azok az alkalmazások, amelyek az Azure AD-bérlő alatt az "Alkalmazás" főoldalon "Alkalmazások a vállalat tulajdonosa" szűrő megjelenítése. Technikai szempontból olyan manuálisan a klasszikus Azure portálon keresztül, vagy programozott módon PowerShell vagy a Graph API segítségével regisztrált alkalmazások. Pontosabban, azok vannak objektum által jelképezett mindkét egy alkalmazás és egyszerű szolgáltatás az Ön bérelt szolgáltatásának. Lásd: [alkalmazás és szolgáltatás egyszerű objektumok](active-directory-application-objects.md) további információt.

#### <a name="to-remove-a-single-tenant-application-from-your-directory"></a>A címtárban lévő egybérlős alkalmazás eltávolítása
1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
2. Válassza ki az Azure AD-bérlő fiókja kiválasztja az oldal jobb felső sarkában.
3. A felső menüben válassza a **Azure Active Directory**, kattintson a **App regisztrációk**, majd kattintson a konfigurálni kívánt alkalmazás. Ez lesz az alkalmazás gyors üzembe helyezés oldalt, valamint nyissa meg a beállítások panelről az alkalmazás.
4. Az alkalmazás lapon kattintson a **törlése**.
5. Kattintson a **Igen** a megerősítő üzeneten.

#### <a name="to-remove-a-multi-tenant-application-from-your-directory"></a>A címtárban lévő több-bérlős alkalmazás eltávolítása
1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
2. Válassza ki az Azure AD-bérlő fiókja kiválasztja az oldal jobb felső sarkában.
3. A felső menüben válassza a **Azure Active Directory**, kattintson a **App regisztrációk**, majd kattintson a konfigurálni kívánt alkalmazás. Ez lesz az alkalmazás gyors üzembe helyezés oldalt, valamint nyissa meg a beállítások panelről az alkalmazás.
4. A beállítások panelen válassza a **tulajdonságok** tükrözés és a **Multi-központjaként** váltani **nem**. Az alkalmazás egybérlős alakítja, de az alkalmazás egy szervezet, aki már hozzájárult, továbbra is megmarad.
5. Kattintson a **törlése** gombra az alkalmazás oldalról.
6. Kattintson a **Igen** a megerősítő üzeneten.

### <a name="removing-a-multi-tenant-application-authorized-by-another-organization"></a>Egy másik szervezet által felhatalmazott több-bérlős alkalmazás eltávolítása
Ezeket az alkalmazásokat, amelyek megjelenítik a fő "alkalmazás" oldalon "Az alkalmazások a vállalat használ" szűrő csoportban az Azure AD-bérlőn, kifejezetten a "Alkalmazások a vállalat tulajdonosa" listán nem szereplő megfelelően részhalmazát jelentik. Technikai szempontból olyan több-bérlős alkalmazásokhoz a hozzájárulási folyamat során regisztrálva. Pontosabban, azok vannak objektum által jelképezett csak egy egyszerű szolgáltatásnév az Ön bérelt szolgáltatásának. Lásd: [alkalmazás és szolgáltatás egyszerű objektumok](active-directory-application-objects.md) további információt.

El szeretné távolítani egy több-bérlős alkalmazás hozzáférés a címtárhoz (után hozzájárulási megadó), a vállalati rendszergazda megszünteti a hozzáférést az Azure portálon keresztül történő Azure-előfizetéssel kell rendelkeznie. A vállalati rendszergazda is használhatja a [Azure AD PowerShell-parancsmagok](http://go.microsoft.com/fwlink/?LinkId=294151) elérését.

## <a name="next-steps"></a>Következő lépések
* Tekintse meg a [Branding irányelveket az integrált alkalmazások](active-directory-branding-guidelines.md) kapcsolatos tippek az alkalmazások visual útmutatást.
* Az alkalmazás és szolgáltatás egyszerű objektumok az alkalmazás közötti kapcsolat a további részletekért lásd: [alkalmazás és szolgáltatás egyszerű objektumok](active-directory-application-objects.md).
* További információt a szerepkör az alkalmazás jegyzékének lejátszása közben, lásd: [az Azure Active Directory alkalmazásjegyzékének megismerése](active-directory-application-manifest.md)
* Tekintse meg a [az Azure AD fejlesztői szószedet](active-directory-dev-glossary.md) bizonyos Azure Active Directory (AD) fejlesztői alapfogalmakat-definíciók.
* Látogasson el a [Active Directory fejlesztői útmutatója](active-directory-developers-guide.md) összes fejlesztői áttekintést kapcsolódó tartalmat.

