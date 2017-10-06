---
title: "Alkalmazások, engedélyek és jóváhagyás az Azure Active Directoryban | Microsoft Docs"
description: "Az Azure AD Connect integrálja a helyszíni címtárakat az Azure Active Directoryval. Ez lehetővé teszi az Azure ad-vel integrált Office 365, az Azure és az SaaS-alkalmazásokhoz közös identitás tooprovide."
keywords: "Bevezetés tooAzure AD, alkalmazások, mi az az Azure AD Connect, az active directory telepítése"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 25897cc4-7687-49b6-b0d5-71f51302b6b1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/31/2017
ms.author: billmath
ms.reviewer: jesakowi
ms.custom: oldportal;it-pro;
ms.openlocfilehash: af0c2669199736fdb41e85876a7e3a7064e80770
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="apps-permissions-and-consent-in-azure-active-directory"></a>Alkalmazások, engedélyek és jóváhagyás az Azure Active Directoryban
Az Azure Active Directoryban hozzáadhat alkalmazásokat tooyour könyvtárat.  hello alkalmazások alkalmazás hello típusától függően változhat.  klasszikus portál hello tooview alkalmazások jelöljön ki egy könyvtárat, és alkalmazások kiválasztása.

![](media/active-directory-apps-permissions-consent/apps1.png)

> [!IMPORTANT]
> A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon.

## <a name="types-of-apps"></a>Alkalmazástípusok

1. **Egybérlős alkalmazások** </br>
    - **Single-bérlői alkalmazások** -nevezik tooas üzletági (LOB) alkalmazások. Ez az hello esetben, ha valaki a szervezeten belüli házon belül fejlesztett alkalmazásokra saját alkalmazás, kíván tenni a felhasználók a hello szervezet toobe képes toosign toohello alkalmazásban.
    ![](media/active-directory-apps-permissions-consent/apps2.png)
    - **Alkalmazás Proxy alkalmazások** – Ha az Azure AD alkalmazás Proxy, helyszíni alkalmazás teszi ki a single-bérlői alkalmazások, a bérlő által (hozzáadása toohello App proxyszolgáltatás) regisztrálva van. Ez az alkalmazás képviseli a helyszíni alkalmazást minden felhőbeli interakció (például hitelesítés) során. (Az alkalmazásproxy használatához Basic vagy magasabb szintű Azure AD-licenc szükséges.)


2. **Több-bérlős alkalmazások**
    - **Több-bérlős alkalmazásokhoz, amelyek mások is** - hasonló túl "single-bérlői alkalmazások, a szervezet házon belül fejlesztett alkalmazásokra". hello fő (mellett hello alkalmazás maga a logikai hello) különbség, hogy a felhasználók a többi bérlőtől is is hozzájárulás tooand bejelentkezési toohello alkalmazásban.</br>
    ![](media/active-directory-apps-permissions-consent/apps4.png)
    - **Mások által fejlesztett több-bérlős alkalmazások, amelyeket a Contoso jóváhagyhat**. (Röviden „jóváhagyott alkalmazások”.) Ez a "több-bérlős alkalmazások a szervezet házon belül fejlesztett alkalmazásokra" hello tükrözés oldalán. Ha egy másik szervezet egy több-bérlős alkalmazást fejleszt, a szervezet felhasználói hozzájárulás toohello alkalmazás, és tooit bejelentkezés.
    - **Belső Microsoft-alkalmazások** – A Microsoft szolgáltatásait képviselő alkalmazások. Hello tényt, hogy regisztrál hello szolgáltatás célja a hozzájárulásukat adják. Nincs néha különleges UX és egyes belső alkalmazások logikát, gyakran használt szabályzatok hozzáférés toohello alkalmazások körül kialakítása során.</br>
    ![](media/active-directory-apps-permissions-consent/apps3.png)
    - **Előzetesen beépített alkalmazások** -hello Azure AD-Alkalmazásgyűjtemény, amely is hozzáadhat az elérhető alkalmazások tooyour directory tooprovide egyszeri bejelentkezés (és egyes esetekben kiépítése) toopopular SaaS-alkalmazásokhoz.
    - **Azure AD egyszeri bejelentkezés**: „Valódi” egyszeri bejelentkezés (SSO) olyan alkalmazások számára, amelyek integrálhatók az Azure AD-vel egy támogatott bejelentkezési protokollon, például az SAML 2.0-n vagy az OpenID Connecten keresztül. hello varázsló bemutatja, hogyan állítsa be azt.
    - **Jelszó az egyszeri bejelentkezés**: az Azure AD biztonságosan tárolja a hello app hello felhasználói hitelesítő adatokat, és hello hitelesítő adatok vannak "be a nézetmodellbe" hello bejelentkezési űrlap hello Azure AD alkalmazás-hozzáférés bővítmény által. Ez a bejelentkezési módszer „jelszótárolásként” is ismert.

## <a name="permissions"></a>Engedélyek

Az alkalmazás regisztrálásakor hello felhasználói hello app regisztrációs (Ez azt jelenti, hogy hello fejlesztői) végrehajtása határozza meg, melyik engedélyek hello alkalmazásnak kell elérnie, és mely erőforrásokat. (hello erőforrásokat, maguk is, más alkalmazások meghatározva.) Például valaki a mail olvasó alkalmazás elkészítése volna állapotban, hogy az alkalmazás engedélyre van szüksége, hello "Postaládák hello bejelentkezett felhasználó nevében" hello "Office 365 Exchange online-hoz" erőforrás a:
    
![](media/active-directory-apps-permissions-consent/apps6.png)

Ahhoz, hogy egy alkalmazás (hello ügyfél) toorequest (hello erőforrás) egy másik alkalmazás egy bizonyos engedélyt hello fejlesztői hello erőforrás alkalmazás létező hello engedélyek határozza meg. A példánkban a Microsoft hello tulajdonos hello "Office 365 Exchange online-hoz" erőforrás alkalmazás definiált "Postaládák hello bejelentkezett felhasználó nevében" nevű engedély.

Engedélyek meghatározásakor hello alkalmazás fejlesztőjének definiálnia kell, ha hello engedéllyel is kell átadni kívánt hozzájárult e, vagy ha a rendszergazda jóváhagyását igényli. Ez lehetővé teszi a fejlesztők tooallow felhasználók tooconsent a saját tooapps csak a bizalmas alacsony engedéllyel a kért, de a rendszergazdák tooconsent toomore bizalmas engedély szükséges. Például az erőforrás alkalmazás "Azure Active Directory" hello, definiálva van, így a felhasználó be tudja hozzájárulás egy tooapps, korlátozott csak olvasási engedéllyel a kért.  A teljes olvasási és minden írási engedélyhez azonban rendszergazdai jóváhagyás szükséges.

Mivel a natív ügyfelek nincsenek hitelesítve, a natív ügyfélalkalmazásként meghatározott alkalmazások csak delegált engedélyeket kérhetnek. Ez azt jelenti, hogy a tokenek beszerzését egy tényleges felhasználónak kell intéznie. A webes alkalmazásoknak és webes API-knak (bizalmas ügyfeleknek) mindig hitelesíteniük kell az Azure AD-vel, amikor hozzáférési tokent igényelnek. Tehát ezenkívül hello lehetőségét csak alkalmazás engedéllyel rendelkeznek. Ha például egy háttér-szolgáltatás tooauthenticate tooanother háttérszolgáltatásnak szüksége van. A csak az alkalmazásra vonatkozó engedélyeket kérő alkalmazások mindig rendszergazdai jóváhagyást igényelnek.

Összefoglalva:



- Egy alkalmazás (ügyfél) szerint hello engedélyek más alkalmazások (erőforrások) szükséges.
- (Erőforrás) alkalmazás-állapotok az engedélyeit olyan kitett tooother alkalmazások (ügyfelek).
- Az engedély lehet csak az alkalmazásra vonatkozó vagy delegált engedély is.
- A delegált engedély megjelölhető „felhasználói jóváhagyást engedélyező” vagy „rendszergazdai jóváhagyást igénylő” engedélyként.
- Egy alkalmazás viselkedhet ügyfélként (azáltal, hogy kell-e engedélyekkel tooa erőforrás), egy erőforrást (is deklarálni kell jogosultságokat az érheti el), vagy mindkettőt.

## <a name="controls"></a>Vezérlők

hello hello különböző felügyeleti vezérlő érhető el ez a viselkedés az listája látható. Üdvözöljük a rendszergazdákat vezérlők is elérhetők a klasszikus portálon hello konfigurálása hello könyvtára alatt tárolja.

![](media/active-directory-apps-permissions-consent/apps7.png)

A hello Azure portál, a **kezelése**, **felhasználói beállítások**.

![](media/active-directory-apps-permissions-consent/apps11.png)



- Szabályozhatja, hogy a felhasználók is hozzájárulás tooapps:

Válassza hello klasszikus portál **felhasználók adhat alkalmazások engedélyek tooaccess adataikat.**
![](media/active-directory-apps-permissions-consent/apps8.png)

Hello Azure-portálon, válassza ki **felhasználók engedélyezhetik alkalmazások tooaccess adataikat**.
![](media/active-directory-apps-permissions-consent/apps12.png)



- Szabályozhatja, hogy a felhasználók regisztrálhatják saját single-bérlő LOB-alkalmazások: A klasszikus portál select hello **felhasználók hozzáadhatnak integrált alkalmazások.**
![](media/active-directory-apps-permissions-consent/apps9.png)

Hello Azure-portálon, válassza ki **felhasználók engedélyezhetik alkalmazások tooaccess adataikat**.
![](media/active-directory-apps-permissions-consent/apps13.png)

>[!NOTE]
>Akkor is, ha engedélyezi a felhasználók tooregister single-bérlő LOB-alkalmazások, nincsenek toowhat regisztrálható korlátozások.  
>A korlátozások vonatkoznak például olyan fejlesztőkre, akik nem címtár-rendszergazdák.
>
>- A felhasználók nem alakíthatnak át egy egybérlős alkalmazást több-bérlőssé.
>- Amikor regisztrál egy bérlői LOB-alkalmazások, felhasználók csak alkalmazás engedélyek tooother alkalmazások nem igényelhetnek.
>- Amikor regisztrál egy bérlői LOB-alkalmazások, a felhasználók nem kérik delegált jogosultságokkal sikeresen telepítették tooother alkalmazások, ha ezeket az engedélyeket a rendszergazda jóváhagyását van szükség.
>- Felhasználók, amelyek még nincsenek tulajdonosainak módosítások tooapps nem hajtható végre.



- Szabályozhatja, hogy a felhasználók hozzáadhatnak-e előre integrált, jelszavas egyszeri bejelentkezést (más néven „jelszótárolást”) használó alkalmazásokat. ![](media/active-directory-apps-permissions-consent/apps10.png)



- Szabályozhatja, hogy az alkalmazások milyen feltételek esetén férhetők hozzá (azaz feltételes hozzáférést is megadhat). Ügyeljen arra, hogy ez toohello ügyfélalkalmazás és toohello erőforrás alkalmazás is vonatkozik. Igen fel hogy feltételes hozzáférési szabályzatot, amely szerint a "Office 365 Exchange online-hoz" hello alkalmazást csak a számítógépek, amelyek megfelelő érhető el.  Ez a házirend is fog indítsa, amikor egy felhasználó megpróbál egy Online engedélyek tooExchange kérő ügyfélalkalmazás toouse.



- Lehetősége van, amelybe alkalmazások hozzájárult, melyeket használt tooand volt látható.

1.  Amikor a felhasználó hozzájárul tooan alkalmazás, egy szolgáltatásnév objektum hello bérlő jön létre. Szolgáltatásnév létrehozása hello ellenőrzési jelentés tartalmazza.
2.  Felhasználói bejelentkezési tevékenység jelentések meg, melyik alkalmazás hello felhasználó próbál bejelentkezni. 

## <a name="example"></a>Példa

Tegyük fel megtudhatja, hogy észrevette a bérlő felhasználói bejelentkezés hello "Office 365-höz FabrikamMail" alkalmazást. A „FabrikamMail” egy levélolvasó alkalmazás Android rendszerhez, amelyet a „Fabrikam, Inc.” tett közzé. Ez beleesik hello "több-bérlős alkalmazásokhoz, amelyek Contoso is más develop".

Ha tooconsent a felhasználók számára engedélyezett, akkor töltse le hozzájárulás kérése hello első bejelentkezéskor:![](media/active-directory-apps-permissions-consent/apps14.png)

Hozzáférés-postaládáit"érték hello felhasználók számára is elérhető hozzájárulási karakterlánc hello"Postaládák hello bejelentkezett felhasználó nevében"engedélyt"Office 365 Exchange online-hoz"(Ez azt jelenti, hogy az Exchange) tesz elérhetővé.

Az Exchange (hello erőforrás), amelyhez hozzá lett adva a regisztráció során az Office 365 hello szolgáltatásnév objektum szervezetifiók hello engedélyek tekintheti meg. Az eltolásokat tekintheti hello szolgáltatásnév objektum hello app "példányának" az Ön bérlőjében, különböző beállítások és konfigurációk rögzítése használt.  Erre úgy tekinthet hello segítségével `Get-AzureADServicePrincipal` a PowerShellben.

    PS C:\> Get-AzureADServicePrincipal -ObjectId 383f7b97-6754-4d3d-9474-3908ebcba1c6 | fl *
    
    DeletionTimeStamp         : 
    ObjectId                  : 383f7b97-6754-4d3d-9474-3908ebcba1c6
    ObjectType                : ServicePrincipal
    AccountEnabled            : True
    AppDisplayName            : Office 365 Exchange Online
    AppId                     : 00000002-0000-0ff1-ce00-000000000000
    AppOwnerTenantId          : 
    AppRoleAssignmentRequired : False
    AppRoles                  : {...}
    DisplayName               : Microsoft.Exchange
    ErrorUrl                  : 
    Homepage                  : 
    KeyCredentials            : {}
    LogoutUrl                 : 
    Oauth2Permissions         : {...
                                , class OAuth2Permission {
                                  AdminConsentDescription : Allows hello app toohave hello same access toomailboxes as hello signed-in user via Exchange Web Services.
                                  AdminConsentDisplayName : Access mailboxes as hello signed-in user via Exchange Web Services
                                  Id                      : 3b5f3d61-589b-4a3c-a359-5dd4b5ee5bd5
                                  IsEnabled               : True
                                  Type                    : User
                                  UserConsentDescription  : Allows hello app full access tooyour mailboxes on your behalf.
                                  UserConsentDisplayName  : Access your mailboxes
                                  Value                   : full_access_as_user
                                },
                                ...}
    PasswordCredentials       : {}
    PublisherName             : 
    ReplyUrl                  : 
    SamlMetadataUrl           : 
    ServicePrincipalNames     : {00000002-0000-0ff1-ce00-000000000000/outlook.office365.com, 00000002-0000-0ff1-ce00-000000000000/mail.office365.com, 00000002-0000-0ff1-ce00-000000000000/outlook.com, 
                                00000002-0000-0ff1-ce00-000000000000/*.outlook.com...}
    Tags                      : {}

Amikor hello felhasználó kattint az "Elfogadás" hozzájárulási indítható. Először egy szolgáltatásnév objektum "Office 365-höz FabrikamMail" hello bérlő jön létre. hello szolgáltatásnév a következőhöz hasonló:

    PS C:\> Get-AzureADServicePrincipal -SearchString "FabrikamMail for Office 365" | fl *
    
    DeletionTimeStamp         : 
    ObjectId                  : a8b16333-851d-42e8-acd2-eac155849b37
    ObjectType                : ServicePrincipal
    AccountEnabled            : True
    AppDisplayName            : FabrikamMail for Office 365
    AppId                     : aba7c072-2267-4031-8960-e7a2db6e0590
    AppOwnerTenantId          : 4a4076e0-a70f-41c6-b819-6f9c4a86df89
    AppRoleAssignmentRequired : False
    AppRoles                  : {}
    DisplayName               : FabrikamMail for Office 365
    ErrorUrl                  : 
    Homepage                  : 
    KeyCredentials            : {}
    LogoutUrl                 : 
    Oauth2Permissions         : {}
    PasswordCredentials       : {}
    PublisherName             : Fabrikam, Inc.
    ReplyUrl                  : 
    SamlMetadataUrl           : 
    ServicePrincipalNames     : {aba7c072-2267-4031-8960-e7a2db6e0590}
    Tags                      : {WindowsAzureActiveDirectoryIntegratedApp}

Küldőnek tooan app kapcsolatot hoz létre Oauth2PermissionGrant hello következő között:
  
- hello felhasználói objektum
- hello ügyfélalkalmazások ServicePrincipalName (SPN)
- hello erőforrás alkalmazások ServicePrincipalName (SPN)
- engedélyek hello erőforrás alkalmazásban.  

FabrikamMail hello esetben azt a következőhöz hasonló:

    PS C:\> Get-AzureADUserOAuth2PermissionGrant -ObjectId ddiggle@aadpremiumlab.onmicrosoft.com | fl *
    
    ClientId    : a8b16333-851d-42e8-acd2-eac155849b37
    ConsentType : Principal
    ExpiryTime  : 05/15/2017 07:02:39 AM
    ObjectId    : M2OxqB2F6EKs0urBVYSbN5d7PzhUZz1NlH25COvLocbJjoxkUFfRQauryBKwBWet
    PrincipalId : 648c8ec9-5750-41d1-abab-c812b00567ad
    ResourceId  : 383f7b97-6754-4d3d-9474-3908ebcba1c6
    Scope       : full_access_as_user
    StartTime   : 01/01/0001 12:00:00 AM

(**ClientId** FabrikamMail a szolgáltatás egyszerű objektum azonosítója (hello egy újonnan létrehozott kapott), **PrincipalId** hello felhasználói objektum azonosítója (hello felhasználó átadni kívánt hozzájárult e), **ResourceId**Exchange a szolgáltatás egyszerű Objektumazonosító, a hatókör lett átadni kívánt hozzájárult e Exchange hello engedélyre).

Ha a felhasználók nem írhatják tooconsent, látnak, amely szerint az engedélyt képernyő szükség.

